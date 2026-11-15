# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 16:04** · publicado em **15/11/2026 16:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 16:04 · sessao 9848e5608028 · - · - · origem=Ronald (tmux)

Achados do comando e da porta:

- **PROCEDE quanto ao DRY literal de produção:** o molde atual chama `aplicar()` mesmo sem `--aplicar`, e só desfaz a transação depois. Há escrita tentada e travas; não é execução somente leitura. A porta do envelope usa role sem privilégios de escrita. É necessário definir um DRY L2 realmente leitor, separado desse fluxo; isso não está explicitado no aval. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:578-584`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:217`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `bin/sonda_leitura.sh:11-16`; `bin/sonda_leitura.sh:142`)

- O molde **tem** DRY por default, flag `--aplicar` e saída PAREI com lista de PKs exportadas. Entretanto, o apply atual aplica grupos abertos e deixa inteiros de fora os grupos exportados; não para necessariamente o ato inteiro. O teste prende precisamente esse comportamento. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:425-427`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:478-482`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:620-628`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:164-175`)

- **Ensaio exportado na sombra:** o veto existente não contém exceção por ambiente; exclui exportadas antes de planejar/escrever. Portanto, simplesmente acrescentar item no mesmo molde não fornece o caminho para ensaiar escrita nas exportadas. Esse caminho depende de implementação adicional **[nao verificado]**, e o literal não distingue como o ensaio atravessa `exportada = PAREI`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:253-265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:302-309`; anexo `avais_1600.txt:4`)

- `registrar` **aceita** `ato=RETRATACAO`, `ato_motivo='obsoleta'` e `ato_em` datetime fornecido. `RETRATACAO` é ato existente; motivo é CharField sem choices; data não tem mecanismo automático nem validação temporal nessa porta. As constraints são de absorção e foto, não restringem esse motivo ou o instante histórico. “Arbitrário” não dispensa o tipo datetime válido. (`app/chamados/models.py:4001`; `app/chamados/models.py:4009`; `app/chamados/models.py:4016`; `app/chamados/models.py:4019`; `app/chamados/models.py:4033-4043`; `app/chamados/models.py:4086-4096`)

- A porta **não deduplica**: faz `bulk_create`. A idempotência de uma segunda execução sequencial pode nascer do antijoin proposto, mas sua implementação/execução L2 e proteção contra concorrência ficam **[nao verificado]**. (`app/chamados/models.py:4094-4098`; anexo `avais_1600.txt:4`)

- Cada chamada que registra DP acorda as células encontradas; emite uma por célula da chamada e pula pergunta sem célula resolvível. `emitir_evento_celula` cria efetivamente `EventoCelula`. Não dá para prometer zero efeito em fechamento/TXT a partir só dessa porta. (`app/chamados/models.py:4097-4098`; `app/chamados/models.py:3314-3331`; `app/ponto/portas/celula.py:945-954`)

- A marca em `LogAuditoria` cabe no schema, que tem ação, objeto, antes/depois e ator. A convenção concreta e a escrita L2 ainda são **[nao verificado]**. Atenção: a API `evento` escreve Log dentro de atomic, mas isso por si não prova que a convenção L2 está implementada. (`app/core/models.py:296-307`; `app/core/observ.py:133-137`)

- Chamar `registrar` respeita o escritor único de DP. Existe catraca que permite escrita do backfill apenas na função de módulo `backfill_decisoes_pergunta.aplicar`; uma nova função escritora em outro ponto quebrará esse selo, salvo alteração deliberada correspondente. (`app/chamados/models.py:3989-3992`; `app/chamados/tests/test_e1_entradas.py:185-222`; `app/chamados/tests/test_e1_v7_catracas.py:253-288`)

- `remover_motivo` grava **RETRATACAO**, mas o motivo é formatado `"<evento>: motivo <motivo> removido (via antes: <via>)"` e não o literal `'obsoleta'`; depois carimba identidade com `VIA_MOTIVO_RETRATADO`, mantendo carimbo anterior quando existe. Isso confirma o MORDE sobre o valor gravado, porém não prova exclusão de uma pergunta que também tenha um log obsoleta anterior. (`app/chamados/models.py:2118-2126`)

## 15/11 16:03 · sessao 7d408a8b92b7 · - · - · origem=Ronald (tmux)

- `_retratada_com_trilha` existe e exige `materializada_em` não nulo, mais `LogAuditoria` com **ação, modelo e objeto**: `acao='retratar_pergunta_obsoleta'`, `modelo='PerguntaDisputa'`, `objeto_id=p.pk`. A expressão resumida do aval precisa preservar a correlação modelo/PK para equivaler ao filtro. (`app/chamados/juizes.py:285`; `app/chamados/juizes.py:291`; `app/chamados/juizes.py:294`)
- `carga_pergunta_viva.py:111` está em **services**. Lê `LogAuditoria`, não `DecisaoPergunta`, para as PKs de `perguntas`; `retratada()` reaproveita `_retratada_com_trilha` com a trilha carregada. (`app/chamados/services/carga_pergunta_viva.py:110`; `app/chamados/services/carga_pergunta_viva.py:192`)
- `remover_motivo` grava `ato=RETRATACAO`, instante atual `_agora_ret`, e motivo textual `'%s: motivo %s removido (via antes: %s)'[:240]`; portanto **não grava motivo igual a `'obsoleta'`**. Depois carimba a via `VIA_MOTIVO_RETRATADO`, preservando carimbo existente. (`app/chamados/models.py:2088`; `app/chamados/models.py:2120`; `app/chamados/models.py:2123`; `app/chamados/models.py:2125`; `app/chamados/models.py:3265`)
- Isso **não prova exclusão universal da pergunta de `remover_motivo`**: o selecionador proposto não exclui essa via nem outras retrações; uma pergunta que também possua log obsoleta e careça da DP obsoleta satisfaz seus critérios. A existência concreta dessa interseção em produção fica **[nao verificado]**. A porta `remover_motivo` por si só não fornece esse log. (`app/chamados/models.py:2088`; `app/chamados/models.py:2118`; `app/chamados/models.py:2120`; `app/chamados/juizes.py:294`)
- A fórmula é idempotente **para rodadas sequenciais concluídas**, pois a linha criada passa a satisfazer a exclusão. Não há garantia concorrente nessa porta: `registrar` executa `bulk_create` sem deduplicação, e os constraints existentes regulam fotos/absorção, sem unicidade do ato obsoleta. Implementação e ensaio L2 permanecem **[nao verificado]**. (`app/chamados/models.py:4033`; `app/chamados/models.py:4086`; `app/chamados/models.py:4094`)
- O zeramento de validação está corretamente em `1494–1495`; `1480–1481` conserva decisões anteriores e atribui resposta. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1494`)

## 15/11 15:59 · sessao 1c1728d29871 · TELA-DIZ-O-QUE-O-SERVIDOR-DISSE · verificador · origem=Code

1. **PROCEDE**, no ramo `r.ok`: `ok` mantém o sucesso; `ja_registrada`, `ignorada_intervalo` e outro/ausente/JSON ilegível recebem exatamente os textos previstos. Todos usam o mesmo overlay com `inset:0` e retorno ao dashboard agendado em 3000 ms. Não há cálculo de próximo tipo nem exibição de texto recebido do servidor nesse tratamento. `app/templates/ponto/bater_ponto_v2.html:150`, `app/templates/ponto/bater_ponto_v2.html:156`, `app/templates/ponto/bater_ponto_v2.html:168`.

   Ressalva de alcance: respostas redirecionadas continuam saindo antes de `r.ok`, pelo caminho preexistente. `app/templates/ponto/bater_ponto_v2.html:364`.

2. **NÃO PROCEDE** a falha nos casos examinados: JSON ilegível é contido pelo `try` interno; sucesso exige `d.status === 'ok'`. O `catch` externo continua envolvendo também a apresentação, mas nenhum dos corpos examinados demonstra exceção escapando até ele. `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:171`, `app/templates/ponto/bater_ponto_v2.html:365`, `app/templates/ponto/bater_ponto_v2.html:369`.

   **PROCEDE**, pelo diff fornecido, que os ramos não-2xx e offline foram preservados; correspondem aos caminhos atuais em `app/templates/ponto/bater_ponto_v2.html:366`, `app/templates/ponto/bater_ponto_v2.html:370`, `app/templates/ponto/bater_ponto_v2.html:382`. Comparação independente com o commit base: **[não verificado]**.

3. **PROCEDE** o contrato lido pelo cliente:

   | Corpo/valor escrito | Servidor core | Servidor UI / serviço |
   |---|---|---|
   | Deduplicação: `status='ja_registrada'` | `app/api/views_core.py:521` | `app/api/views.py:535` |
   | Colisão: `status='ignorada_intervalo'` | `app/api/views_core.py:673` | `app/api/views.py:682` |
   | Reutilização: corpo produzido pelo serviço, `status='ja_registrada'` | `app/api/views_core.py:684` | `app/api/views.py:693`; `app/ponto/services/resposta_batida.py:21` |
   | Criação: `status='ok'` | `app/api/views_core.py:807` | `app/api/views.py:813` |
   | `aviso`: frase ou string vazia, somente no `ok` | `app/api/views_core.py:805`, `app/api/views_core.py:817` | `app/api/views.py:811`, `app/api/views.py:823`; produtor: `app/ponto/services/aviso_da_batida.py:55` |

   O cliente decodifica o corpo e lê somente `d.status`; não lê `aviso`, `mensagem`, `sucesso` ou `tipo` da resposta. `v2Aviso` é uma função de apresentação de textos locais, não um leitor de `d.aviso`. `app/templates/ponto/bater_ponto_v2.html:163`, `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:172`.

   **NÃO PROCEDE** haver outro status nos retornos 2xx dessas funções: os quatro caminhos acima produzem apenas os três valores. `app/api/views_core.py:521`, `app/api/views_core.py:673`, `app/api/views_core.py:686`, `app/api/views_core.py:807`; `app/ponto/services/resposta_batida.py:22`.

   Não há dupla semântica no campo lido: `r.status` é HTTP; `d.status` é resultado da batida. Existe ambiguidade preexistente em `sucesso=True`, inclusive para `ignorada_intervalo`, mas a cura não usa esse campo. `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:367`; `app/api/views_core.py:680`, `app/api/views_core.py:814`.

4. **PROCEDE** que o RED morde o texto visível nas duas cascas: elas são enumeradas explicitamente, renderizadas pelo helper, e a sonda lê `innerText` do elemento no centro da janela. `app/ponto/tests/test_camera_alternativa_previa.py:38`, `app/ponto/tests/test_camera_alternativa_previa.py:144`; `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:94`.

   **NÃO PROCEDE** passar com texto vazio: há guarda positiva para `ok`, igualdade com textos não vazios nos demais e exigência de um POST/zero registros offline. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:136`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:143`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:151`.

   **PROCEDE**, por análise, que ambos os mutantes morreriam: sucesso incondicional viola a proibição de “êxito”; JSON sem proteção alcança o registro offline e viola sua contagem zero. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:65`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:136`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:150`; `app/templates/ponto/bater_ponto_v2.html:371`. Execuções e quantidades de falhas relatadas: **[não verificado]**.

   Limites: callback vazio de retorno passaria, porque o teste apenas conta e descarta timers de 3000 ms; “tela cheia” verifica somente `position:fixed`. O código atual contém destino e geometria corretos. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:69`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:96`; `app/templates/ponto/bater_ponto_v2.html:153`, `app/templates/ponto/bater_ponto_v2.html:156`.

5. **NÃO PROCEDE**, no tratamento novo, introdução de segundo juiz, segundo escritor de batida, relógio de negócio ou derivação paralela: ele traduz o status recebido em apresentação. O selo tem mordida para essa tradução e percorre ambas as cascas. `app/templates/ponto/bater_ponto_v2.html:168`; `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:140`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:145`.

   Smoke automatizado não comprova o clique humano: a sonda chama `v2Confirmar` diretamente; a exigência de clique permanece distinta. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:92`; `CLAUDE.md:202`. Smoke iPhone/Android, RED/GREEN e suites efetivamente executados: **[não verificado]**. A aprovação aqui é da pré-verificação do código; esses resultados não ficam certificados.

6. **NÃO PROCEDE** consumidor omitido no censo estático de `templates/`: **N=1**, a própria v2. O outro consumidor encontrado é o sincronizador excluído pelo aval: ele decodifica JSON, ignora `data.status` e remove a pendência por `resp.ok`. `app/templates/ponto/bater_ponto_v2.html:360`; `app/static/pwa.js:153`, `app/static/pwa.js:158`, `app/static/pwa.js:162`.

   A tela antiga envia à própria rota e consome HTML; o service worker não intercepta POST. `app/templates/ponto/bater_ponto.html:237`, `app/templates/ponto/bater_ponto.html:533`, `app/templates/ponto/bater_ponto.html:539`; `app/static/sw.js:36`. Inclusão efetiva da linha proposta no BACKLOG: **[não verificado]**.

VEREDITO: APROVA

## 15/11 15:58 · sessao 62f3c873c59d · - · - · origem=Ronald (tmux)

6. **NÃO PROCEDE** a hipótese de outro consumidor omitido em `templates/`: o censo estático por endpoint, nome de rota, nomes das funções e valores de status encontrou **N=1**, a v2; envio em `app/templates/ponto/bater_ponto_v2.html:360`, tratamento em `:365`, leitura em `:170`.

- Fora de `templates/`, o consumidor encontrado é `app/static/pwa.js:153`: decide pelo HTTP em `:158`, lê JSON em `:159`, mas ignora `data.status` e remove a pendência em `:162`. Sua interface é o indicador agregado em `:194`; a exclusão do sincronizador está expressa no aval fornecido, anexo `reemissao_1215.txt`, item A [não verificado quanto a arquivo físico].
- A tela antiga não consome essa resposta: seu formulário não define `action` (`app/templates/ponto/bater_ponto.html:237`) e o fetch envia à própria localização (`:533`), consumindo HTML (`:539`). A rota web é separada: `app/ponto/urls.py:11`, com processamento em `app/ponto/views.py:271` e `:352`.
- `sw.js` não constitui outro consumidor desse POST: retorna antes de interceptar métodos diferentes de GET (`app/static/sw.js:35`).
- Rotas API confirmadas: `app/api/urls.py:67` aponta para `views`; `app/api/urls_core.py:14` aponta para `views_core` por importação em `:5`; inclusão dos roteadores em `app/config/urls.py:37` e `app/config/urls_core.py:6`.
- O template é classe A por exceção nominal: `app/docs/CORTES.md:411` e `:413`. O arquivo raspado `leis_sitios.txt` fornece numeração diferente; nesta cópia, os endereços vivos são 411/413.
- A linha `PWA-SYNC-NAO-DIZ-STATUS` aparece como proposta no anexo; sua efetiva inclusão no BACKLOG não foi demonstrada [não verificado].
- Smoke manual iPhone/Android permanece pendente segundo o próprio anexo `entrega.md`; execução real desses cliques [não verificado].

## 15/11 15:58 · sessao 3146cc6c831d · - · - · origem=Ronald (tmux)

Pergunta 3: **PROCEDE** o contrato usado pelo cliente; **NÃO PROCEDE** haver outro `status` 2xx emitido por essas funções. Os retornos são:

| Resultado | `api/views_core.py` | `api/views.py` |
|---|---|---|
| Deduplicação: `ja_registrada` | 521–531 | 535–545 |
| Colisão: `ignorada_intervalo` | 673–683 | 682–692 |
| Reutilização: `ja_registrada` | 684–686 | 693–695 |
| Criação: `ok` | 807–818 | 813–824 |

O corpo reutilizado vem de `app/ponto/services/resposta_batida.py:21`, com `status='ja_registrada'` em `:22`; o serviço identifica criação pela marca `criada_neste_toque` em `:14`, escrita nas criações em `app/ponto/registro_batida.py:155` e `:161`.

O cliente lê o corpo por `r.json()` e exclusivamente seu `d.status`; compara estritamente os três valores e usa textos locais. `aviso`, `mensagem`, `tipo` e `sucesso` do JSON não participam dessa decisão. `app/templates/ponto/bater_ponto_v2.html:170`, `:171`, `:172`, `:173`, `:174`.

`aviso` é uma string com frase ou `''`, produzida por `aviso_do_dia`; só aparece no retorno `ok`. `app/ponto/services/aviso_da_batida.py:55`, `:69`, `:79`, `:90`; `app/api/views_core.py:805`, `:817`; `app/api/views.py:811`, `:823`. O payload de reutilização não contém essa chave. `app/ponto/services/resposta_batida.py:21`.

A função cliente `v2Aviso` apresenta textos locais e não lê o campo JSON `aviso`. `app/templates/ponto/bater_ponto_v2.html:163`, `:172`. O aviso inicial da página é outro canal: contexto `aviso_do_dia`, preenchido em `app/ponto/services/triagem_batida.py:764`, `:765` e exibido em `app/templates/ponto/bater_ponto_v2.html:55`, `:56`.

Não aparece dupla semântica nova nos campos lidos: `r.ok` trata HTTP e `d.status` trata resultado da batida. `app/templates/ponto/bater_ponto_v2.html:365`, `:170`. Existe uma ambiguidade preexistente em **`sucesso=True`**, inclusive quando nada novo foi registrado, mas o diff não lê essa chave. `app/api/views_core.py:680`, `:814`; `app/api/views.py:689`, `:820`; `app/templates/ponto/bater_ponto_v2.html:170`.

As duas rotas de código existem: UI inclui `api.urls`, que aponta para `views.api_bater_ponto`; kernel inclui `api.urls_core`, que aponta para `views_core.api_bater_ponto`. `app/config/urls.py:37`; `app/api/urls.py:2`, `:67`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5`, `:14`. Qual delas atende produção: [não verificado].

Correção pontual da entrega: o literal `ignorada_intervalo` está em `app/api/views_core.py:674`; `:675` contém `id`.

## 15/11 15:58 · sessao cd9b16af6fec · - · - · origem=Ronald (tmux)

O RED morde os comportamentos pedidos nas duas cascas: o mock responde 200 com os seis corpos (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:51`), chama a função real de envio (`:92`), mede `innerText` do elemento no centro da janela (`:94`) e exige um POST, elemento fixo, um agendamento de 3000 ms e nenhuma escrita offline (`:136`). As duas cascas são explícitas (`app/ponto/tests/test_camera_alternativa_previa.py:38`), carregadas pelo loader com o conteúdo real de cada raiz (`:144`).

Não passa vazio: a sonda ausente e erros falham (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:132`); o caso `ok` exige o texto positivo (`:143`); cada não-ok exige igualdade com texto não vazio (`:151`). JS ausente também falharia pelo POST zero (`:136`).

Os mutantes descritos morreriam por análise: sucesso incondicional exibe “êxito” e viola `:150`; JSON sem `try` rejeita no caso ilegível (`:65`), alcança o catch externo que registra offline (`app/templates/ponto/bater_ponto_v2.html:369`), violando `:136`. Quantidades de falhas relatadas e execuções efetivas: [não verificado].

Limites reais da prova:

- O retorno não é executado: todo timer de 3000 ms é interceptado e descartado (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:67`). Um callback vazio passaria; o destino e o retorno correto estão demonstrados pelo código atual (`app/templates/ponto/bater_ponto_v2.html:156`).
- “Tela cheia” mede apenas `position:fixed` (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:96`); geometria integral está no código atual, `inset:0` (`app/templates/ponto/bater_ponto_v2.html:153`).
- A chamada é direta a `v2Confirmar`, sem clique (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:92`); o Chromium usa configuração desktop padrão (`app/core/tests/test_smoke_chromium.py:104`). Isso não comprova os smokes humanos iPhone/Android: [não verificado].
- A exigência de duas cascas está satisfeita no teste (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:141`, `:146`; `CLAUDE.md:203`). Smoke de clique continua uma prova distinta (`CLAUDE.md:202`).

## 15/11 15:40 · sessao 3347d475b5e6 · E3-RESTO-R3 · verificador · origem=Code

1. **F1 — Alta: `--apply` fecha pauta de colaborador cuja apuração falhou.** Se `_ins.cxr()` lança exceção, `apurar` omite o colaborador sem registrá-lo em `sem_leitura`. Sua empresa/competência permanece no recorte; `sincronizar` interpreta a ausência como saída do alvo e fecha a pauta, afirmando que os dias ganharam fio ou veredito novo. Caso sintético reproduzido em memória: col7, empresa1, competência10/2026, pauta viva e falha do espelho → `apurado=[]`, `sem_leitura=[]`, um fechamento. A cura protege outras empresas/competências, mas ainda permite fechar trabalho não apurado. (`app/ponto/management/commands/pautas_discordantes.py:103`, `app/ponto/management/commands/pautas_discordantes.py:126`, `app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:211`.)

   O teste do comando substitui `apurar` por `Mock(return_value=[])`; portanto não exercita essa falha. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:142`.)

2. **F2 — Média: retrato legado mascara alarme positivo conhecido.** Basta uma linha bloqueada sem `dias_sem_leitura` para o total virar `None`, inclusive quando outra linha já informou dias problemáticos. O comando trata `None` como não alarmante. Caso sintético reproduzido: uma linha nova com um dia sem leitura → contador `1`, saída `2`; acrescentando uma linha legada → contador `None`, saída `0`, mesmo com `--alarme --alarme-passivo`. (`app/folha/vigia_retido_calado.py:57`, `app/folha/vigia_retido_calado.py:63`, `app/folha/management/commands/vigiar_retido_calado.py:38`.)

   Os testes cobrem positivo e legado separadamente; não cobrem positivo misturado com legado. (`app/chamados/tests/test_e3_r3_c6_sem_banco.py:251`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:258`.)

3. **F3 — Média: a cura do relógio não alcançou os chamadores reais da tela.** `o_que_fazer` aceita `hoje`, mas seu chamador de produção não o fornece; `desenho_do_led` também chama o leitor sem ele. Filtro, LED e pílulas continuam chegando a `timezone.localdate()` separadamente. Caso concreto: uma resposta atravessa meia-noite com linha `ninguem`, `datas=['2026-11-15']`; ela pode entrar pelo filtro **Hoje** e depois receber LED/barra **Sem ação disponível**. (`app/colaboradores/views.py:986`, `app/folha/retrato_folha.py:460`, `app/folha/retrato_folha.py:553`, `app/folha/retrato_folha.py:650`, `app/folha/retrato_folha.py:830`.)

   O teste c11 prova somente o caminho com `hoje` explicitamente fornecido, não a integração desses chamadores. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:230`.)

4. **F4 — Média: o novo destino “Resolver dia” perde a casca necessária à navegação do calendário.** A Folha abre diretamente `calendario_ponto` no drawer. A resposta é o partial cujas setas e modos apontam para `#calendario-container`; esse elemento pertence ao painel anterior, substituído por `corpo.innerHTML = html`, e não é recriado pelo calendário. O mês inicial abre, mas os controles ficam sem seu destino HTMX. (`app/folha/retrato_folha.py:1042`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/colaboradores/views.py:1965`, `app/templates/core/_drawer_generico.html:26`, `app/templates/colaboradores/calendario_partial.html:38`, `app/templates/colaboradores/partials/painel_colaborador.html:183`.)

   Execução desse percurso em navegador: **[nao verificado]**.

5. **F5 — Baixa: residual `decidir` aparece como “Validar no fio”.** A tarefa conserva `acao=None` e depois promove o fio residual para `admin`. Sem ação publicada, a apresentação usa o fallback de `admin`, rotulado “Validar no fio”. Caso: fila `{101: 'decidir'}`, sem ação de instância. O destino continua abrindo o fio, mas a orientação nomeia outra ação. (`app/chamados/services/tarefa_do_dia.py:692`, `app/chamados/services/tarefa_do_dia.py:696`, `app/folha/retrato_folha.py:980`, `app/folha/retrato_folha.py:1055`.)

6. **F6 — Baixa: permaneceram afirmações documentais falsas.**

   - “QUATRO fios” e “baldes QUATRO” contradizem os cinco valores atuais. (`app/folha/retrato_folha.py:164`, `app/folha/fora_da_folha.py:39`, `app/folha/retrato_folha.py:521`, `app/folha/retrato_folha.py:190`.)
   - A promessa de marcar também os dias `nao_sei` não se realiza por data: o template marca apenas `dias_sem_porta`. (`app/folha/fora_da_folha.py:96`, `app/templates/folha/_retrato_o_que_fazer.html:46`.)
   - Os comentários dos crons ainda descrevem leituras de `VIVOS/respondida_em` e `fio_da_linha`; os caminhos atuais consomem a porta da tarefa e `dono_da_linha`. Os papéis declarados continuam coerentes. (`app/config/crons.py:2289`, `app/config/crons.py:2261`, `app/chamados/services/furo_sem_canal.py:98`, `app/folha/vigia_retido_calado.py:55`.)

Os casos acima são demonstrações pelo código e por execução em memória; sua ocorrência em produção é **[nao verificado]**.

Para **A**, a conferência das curas ficou assim:

| PROCEDE | Cura no sítio-fonte e mordida |
|---|---|
| **2/7 — LU-01** | **Real.** A escada contém `decidir → admin`; a publicação residual a consulta preservando rota/fila. O esperado alterado possui lei explícita. Mutante removendo a entrada morre. Resta F5 na apresentação. (`app/docs/CORTES.md:433`, `app/chamados/juizes.py:1653`, `app/chamados/services/tarefa_do_dia.py:696`, `app/chamados/tests/e3_r3_c11_mutantes.py:19`.) |
| **1a — data do Resolver dia** | **Real.** O dia atravessa projeção, Folha e href; o calendário consome ano/mês. Os quatro mutantes retirando os transportes e a querystring morrem. A integração da casca tem F4. (`app/chamados/services/tarefa_do_dia.py:714`, `app/folha/fora_da_folha.py:212`, `app/folha/retrato_folha.py:1047`, `app/colaboradores/services/calendario.py:194`, `app/chamados/tests/e3_r3_c11_mutantes.py:21`.) |
| **1b — marca dos dias sem porta** | **Real.** O template lê o campo; o mutante retirando a condição morre. (`app/templates/folha/_retrato_o_que_fazer.html:46`, `app/chamados/tests/e3_r3_c11_mutantes.py:29`.) |
| **3 — recorte das pautas** | **Parcial.** O filtro protege empresa/competência externa e `empresas=[]` não expande para a frota; ambos possuem mordida. Permanece F1. (`app/ponto/services/pautas_dos_discordantes.py:73`, `app/ponto/services/pautas_dos_discordantes.py:190`, `app/chamados/tests/e3_r3_c11_mutantes.py:31`.) |
| **4 — relógio** | **Parcial.** `censo` exige instante compatível com `hoje`; o reatador transmite o mesmo instante às duas consultas. Esses mutantes morrem. A transmissão até a tela permanece incompleta: F3. (`app/chamados/services/vigia_fio.py:273`, `app/chamados/services/vigia_fio.py:283`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/tests/e3_r3_c11_mutantes.py:37`.) |
| **5 — alarme sem leitura** | **Parcial.** O contador entrou em `ALARMA_EM_0` e retirar essa entrada mata o teste. Permanece F2 na formação do total. (`app/folha/management/commands/vigiar_retido_calado.py:17`, `app/chamados/tests/e3_r3_c11_mutantes.py:50`.) |
| **8 — cor** | **Implementada na paleta.** `neutro/#374151` é distinto dos outros sete tokens; fundo/borda são `#f9fafb/#e5e7eb`. Mutantes de colisão e alteração do hex morrem. O `neutro` de `STATUS_DO_APP` pertence a outro vocabulário, sem compartilhar esse lookup de cor. (`app/folha/retrato_folha.py:315`, `app/folha/retrato_folha.py:323`, `app/templates/core/_barra_gestao.html:67`, `app/ponto/status_dia.py:84`, `app/chamados/tests/e3_r3_c11_mutantes.py:53`.) |
| **9 — docstrings e editor da ausência** | **Os sítios apontados foram alterados.** O link solicita `somente_decisao=1`, a view desliga o editor dependente de `salvarAusencia`, e os dois mutantes morrem. Os testes documentais verificam texto; não demonstram comportamento por si. Restam F6. (`app/folha/retrato_folha.py:1051`, `app/ponto/views.py:2090`, `app/templates/ponto/partials/painel_ausencia.html:145`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:299`, `app/chamados/tests/e3_r3_c11_mutantes.py:63`.) |

Não encontrei cura feita **somente no teste** nesses itens; encontrei curas incompletas nos pontos 3, 4 e 5, conforme F1–F3 e os sítios acima. [nao verificado]

Para **B**:

- **6 — descarte sustentado.** A R3 reancora um pendente, sem acrescentar autoridade ao registro. As frases de nascimento de C19/C20 constam no dado fornecido da principal, em `app/docs/CORTES.md:418` e `app/docs/CORTES.md:419`; execução da trava no pouso: **[nao verificado]**. (`app/core/juizes.py:153`, `app/core/juizes.py:160`, `app/core/juizes.py:878`.)
- **3b — descarte sustentado quanto às entidades da obra.** `get_or_create` pode escrever `User`, não Chamado/Pauta/CelulaDia/DiaPago/FechamentoMensal. A existência garantida do usuário na frota é **[nao verificado]**; a docstring que promete `DoesNotExist` contradiz a implementação. (`app/core/utils.py:15`, `app/core/utils.py:18`.)
- **10 — descarte sustentado como obrigação anterior ao pouso.** A medida M1 está exigida antes do pouso; não há prova fornecida de seu cumprimento no hash final nem de pouso prematuro: **[nao verificado]**. (`app/chamados/tests/e3_r3_c6_entrega.txt:86`, `app/chamados/tests/e3_r3_c6_entrega.txt:94`.)

No **C1**, o rastreio dos campos e valores relevantes do diff, incluindo fontes e consumidores externos, é:

| Campos/valores | Quem escreve ou publica | Quem lê; significado |
|---|---|---|
| Identidade da pergunta, `data_evento`, motivo, instância | `app/chamados/models.py:2903`; `app/chamados/services/fio.py:829` | `app/chamados/services/tarefa_do_dia.py:242`, `app/chamados/services/tarefa_do_dia.py:404`: **dia do turno**, não instante nem dia civil da resposta. |
| Decisão, `ato_em`, texto, absorção | `app/chamados/models.py:4086` | `app/chamados/services/tarefa_do_dia.py:419`, `app/chamados/services/tarefa_do_dia.py:140`: última decisão com `ato_em <= agora`. |
| Ata, veredito, protestos, status do dia | Porta `app/ponto/portas/celula.py:1032` | Leitor `app/escala/services/leitor_celula.py:350`; tarefa `app/chamados/services/tarefa_do_dia.py:188`; censos `app/chamados/services/vigia_fio.py:332`. |
| Lâmpada `veredito_lavrado` e vigência | `app/chamados/veredito_lavrado.py:254` | `app/chamados/services/tarefa_do_dia.py:431`: leitura com `esta_stale`, sem novo julgamento. |
| Efeito da batida, `pergunta_origem`, `criado_em` | Campos do modelo em `app/ponto/models.py:40`, `app/ponto/models.py:79` | `app/chamados/services/tarefa_do_dia.py:440`: universo apurável, existência limitada por `criado_em <= agora`. |
| Ausência, status e id publicado | `app/ponto/services/ausencia.py:179`, `app/ponto/services/ausencia.py:872` | `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:307`; id vira destino em `app/folha/retrato_folha.py:1038`. |
| Chamado, estado, âncora, fila e verbo | Estado pela porta `app/chamados/models.py:447`; âncora por `app/chamados/services/fio.py:97`; alimentação `app/chamados/juizes.py:1562` | Tarefa `app/chamados/services/tarefa_do_dia.py:444`, `app/chamados/services/tarefa_do_dia.py:625`; terminais vêm do catálogo, não de lista nova. |
| `agora`, `hoje`, competência, `[ini,fim]` | Bordas `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/folha/management/commands/lavrar_retrato_folha.py:49`; janelas consultadas em `app/chamados/services/furo_sem_canal.py:66` | Carga inclusiva em `app/chamados/services/tarefa_do_dia.py:570`; decisão por instante e apresentação por dia local. Divergência: F3. |
| Oferta `resolver_dia_oferecido`, inclusive `None` | `app/colaboradores/services/calendario.py:799`, plural `app/colaboradores/services/calendario.py:125` | `app/chamados/services/tarefa_do_dia.py:611`, `app/chamados/services/tarefa_do_dia.py:632`: desconhecimento preservado em `nao_sei`. |
| `rota`, `fila`, `verbos`, `fio`, `acao`, `ausencia`, `dia_do_turno` | `app/chamados/services/tarefa_do_dia.py:698`, projeção `app/chamados/services/tarefa_do_dia.py:702` | Folha `app/folha/fora_da_folha.py:71`; furo `app/chamados/services/furo_sem_canal.py:98`; Vigia `app/chamados/services/vigia_fio.py:594`; beco `app/escala/services/cadastro_realidade.py:472`; pauta `app/ponto/services/pautas_dos_discordantes.py:85`. |
| Grupo, ações, datas, `dias_sem_porta`, `dias_sem_leitura` | `app/folha/fora_da_folha.py:202` | Copiados ao retrato em `app/folha/retrato_folha.py:1230`; marca no template `app/templates/folha/_retrato_o_que_fazer.html:46`; contagem em `app/folha/vigia_retido_calado.py:57`. |
| JSON do retrato: status, motivo, dono, tipo, saldo, dias, datas, ações, informativo | Montagem `app/folha/retrato_folha.py:1223`; persistência `app/folha/management/commands/lavrar_retrato_folha.py:78` | Tela `app/folha/views_retrato.py:196`, drawer `app/colaboradores/views.py:984`, Vigia `app/folha/vigia_retido_calado.py:53`. `dono` gravado contém **fio**; o balde visual vem de `dono_da_linha`. (`app/folha/retrato_folha.py:435`.) |
| Contadores de furo, rotas e `nao_sei` | `app/chamados/services/furo_sem_canal.py:108` | Command `app/chamados/management/commands/lavrar_furo_sem_canal.py:34`; snapshot em `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`. |
| Contadores do Vigia e linha de nove posições | `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:521`; formato em `app/chamados/services/vigia_fio.py:494` | Command e carimbo `app/chamados/management/commands/vigiar_fio_morto.py:180`. `alcance_nao_sei` conta dias independentemente da rota. (`app/chamados/services/vigia_fio.py:599`.) |
| `calados`, `roteados`, `dias_sem_leitura_sistema` | `app/folha/vigia_retido_calado.py:49` | Alarme `app/folha/management/commands/vigiar_retido_calado.py:37`: os dois primeiros contam pessoa por retrato/competência; o último conta dia-colab. Problema de agregação: F2. (`app/folha/vigia_retido_calado.py:7`.) |
| Pauta: texto/chave, âncora, destinatário, `feito_em`; recorte e retenções transitórias | Seleção `app/ponto/services/pautas_dos_discordantes.py:147`; escrita `app/pautas/services.py:153`; fechamento `app/pautas/services.py:256` | Sincronização `app/ponto/services/pautas_dos_discordantes.py:189`; âncora significa `empresa:AAAA-MM`, conforme `app/pautas/models.py:84`. Problema: F1. |
| Beco: M1, `pai_limbo`, perguntas, data e detalhe | `app/escala/services/cadastro_realidade.py:502`, `app/escala/services/cadastro_realidade.py:515` | Reatador `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:63`: lista vazia de perguntas não provoca reata nem criação de pai. |
| Hrefs, `somente_decisao`, `pode_editar`; token, hex, fundo e borda | `app/folha/retrato_folha.py:1033`, `app/folha/retrato_folha.py:315` | View `app/ponto/views.py:2090`; templates `app/templates/ponto/partials/painel_ausencia.html:145`, `app/templates/core/_barra_gestao.html:67`. Problemas de apresentação: F4/F5. |

Os demais itens de **C**:

- **C2 — nenhum segundo juiz novo encontrado.** Os leitores principais consomem a projeção; `TERMINAIS` vem do catálogo. `dono_fora_do_fio` subdivide somente dias sem rota, e C17 permanece no instrumento de sombra. (`app/chamados/services/tarefa_do_dia.py:447`, `app/chamados/services/vigia_fio.py:601`, `app/chamados/services/vigia_fio.py:615`, `app/core/juizes.py:136`.)
- **C3 — nenhum segundo escritor novo dessas cinco entidades encontrado; há F1 na decisão de fechar Pauta.** Pautas usam os serviços existentes; reata usa a porta do fio; os commands de pauta, reata e snapshot exigem `--apply`. Não apareceu escrita nova de CelulaDia, DiaPago ou FechamentoMensal no diff fornecido. (`app/ponto/services/pautas_dos_discordantes.py:201`, `app/chamados/services/limbo_folgas.py:245`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`.)
- **C4 — F3.** A injeção funciona no caminho testado, mas os leitores reais ainda recorrem ao relógio interno. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:239`, `app/folha/retrato_folha.py:460`.)
- **C5 — papéis coerentes; comentários envelhecidos em F6.** Snapshot é `lavra`; os dois comandos de contagem são `vigia`. (`app/config/crons.py:2291`, `app/config/crons.py:2236`, `app/config/crons.py:2262`.)
- **C6 — nenhum registro novo na R3.** A alteração é reancoragem do pendente TT3; C19/C20 são os registros anteriores. (`app/core/juizes.py:878`, `app/core/juizes.py:153`, `app/core/juizes.py:160`.)
- **C7 — não encontrei enfraquecimento na troca residual ou em `3272→3273`.** C12 testa preservação de rota/fila/ação; a mudança de dono tem LU-01. A linha real está em 3273, a igualdade da catraca permanece e o mutante voltando a 3272 morre. Comparação histórica integral dos corpos B: **[nao verificado]**. (`app/chamados/tests/test_e3_r3_c12_sem_banco.py:12`, `app/docs/CORTES.md:433`, `app/ponto/views.py:3273`, `app/core/tests/test_medida_turno_do_par.py:553`, `app/chamados/tests/e3_r3_c12_mutantes.py:32`.)
- **C8 — F4/F5.** Os números da barra são lidos de `it.numero`; a nova marca lê o campo gravado, sem contar dias no template. Permanecem textos técnicos “Datas (ISO)” e “Sem porta”. Renderização nas duas cascas e estilo computado: **[nao verificado]**. (`app/templates/core/_barra_gestao.html:67`, `app/templates/folha/_retrato_o_que_fazer.html:43`, `app/templates/folha/_retrato_o_que_fazer.html:46`.)
- **C9 — F6.** As correções documentais apontadas na triagem existem, mas não eliminaram as contradições enumeradas. (`app/chamados/services/furo_sem_canal.py:19`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:6`, `app/folha/retrato_folha.py:521`.)
- **C10 — nenhuma inversão nova de trava encontrada.** O reatador grava a reversão antes da escrita, trava Colaborador antes de Chamado e relê a fila sob a trava. Cumprimento da medida M1 final e ordem efetiva do pouso: **[nao verificado]**. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:112`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/tests/e3_r3_c6_entrega.txt:94`.)

Reexecutei os **314 testes locais**, os **23 mutantes c11** e os **9 mutantes c12**: controles verdes e mortes por asserção. Isso sustenta as curas específicas, sem cobrir os casos F1–F4. Suite integral, ORM e renderização permanecem **[nao verificado]**. (`app/chamados/tests/e3_r3_c12_verificar.py:68`, `app/chamados/tests/e3_r3_c11_mutantes.py:70`, `app/chamados/tests/e3_r3_c12_mutantes.py:84`, `app/chamados/tests/e3_r3_c12_entrega.txt:34`.)

**VEREDITO: REPROVA.**

## 15/11 15:34 · sessao 778c10d24aae · TELA-DIZ-O-QUE-O-SERVIDOR-DISSE · verificador · origem=Code

Conto as linhas dos anexos dentro de cada bloco, incluindo vazias, como na revisão anterior (`revisao_anterior.r:5`).

1. **NAO PROCEDE — não identifiquei condição inexequível no literal.**

   Entre os construtores identificados no dado, **F1 + C seriam dois**: C é opus, e a trava de um Codex construtor é específica do Codex. BUG-PAR está registrada esperando R3; se sua volta abrir antes, C espera entregar. A conferência efetiva do teto na abertura permanece **[nao verificado]** (`CLAUDE.md:291-292,353-356`; `queima_util_1530.txt:2,6`; `relato_topo_1530.txt:3-5`).

   Cópia nova do HEAD e deploy de UI com `--sem-migrate` são procedimentos previstos. A flag recusa migrations pendentes; ausência concreta de pendências **[nao verificado]** (`CLAUDE.md:39-40,309-313`; `bin/deploy.sh:205-219`).

   As duas cascas podem ser exercitadas pelos testes existentes. RED/verde de C e smoke humano cumpridos **[nao verificado]** (`app/ponto/tests/test_l030_templates_renderizados.py:15-30`; `queima_util_1530.txt:6`).

2. **NAO PROCEDE — os “senão” estão definidos.**

   Sem vaga, não abre; BUG-PAR aberta tem precedência; cota acabando determina commit parcial e RELATO. Status desconhecido, ausente ou JSON ilegível têm mensagem definida; sítios fora de `templates/` vão ao BACKLOG. Condição literal falsa encontra também o PAROU geral (`queima_util_1530.txt:6-8`; `reemissao_1215.txt:4-5`; `CLAUDE.md:37-38`). [nao verificado]

3. **NAO PROCEDE quanto a colisão demonstrada; cruzamento integral dos diffs [nao verificado].**

   C levanta nominalmente a proibição que motivou a devolução anterior de A. A exceção agora alcança construção dessa obra (`queima_util_1530.txt:6`; `revisao_anterior.r:29-33`).

   A v2 continua **classe A nominal**, com escada inteira: “tela” não dispensa preverificação nem suite inteira verde antes do opus final. O novo literal não revoga essas obrigações (`app/docs/CORTES.md:409,411,413`; `CLAUDE.md:372-381`).

   C exige expressamente smoke nas **DUAS cascas**, preservando BUG 73. iPhone e Android, sozinhos, não comprovam as árvores `base.html` e `base_app.html`; cliques efetivos **[nao verificado]** (`queima_util_1530.txt:6`; `CLAUDE.md:202-203`; `app/docs/LEIS.md:98-99`).

   **Não certifico ausência de arquivo comum** entre C e R3/F1/BUG-PAR: os três diffs `main...obra`, especialmente F1, permanecem **[nao verificado]**. O cruzamento registrado entre BUG-PAR e R3/F1 identifica outros arquivos e não resolve essa pergunta sobre C (`relato_topo_1530.txt:3`). A regra exige conferir arquivo comum, inclusive testes (`CLAUDE.md:309-313`; `complemento2_queima.txt:4`). [nao verificado]

4. **NAO PROCEDE quanto às referências materiais conferidas; referências sem comprovação seguem [nao verificado].**

   **Sim:** no main, `r.ok` chama diretamente `v2Sucesso(tipo,false)`, agora na **linha 348**. JSON só é lido depois, no ramo não 2xx. O êxito está em `:155`, e o retorno ao dashboard em 3 segundos em `:157` (`app/templates/ponto/bater_ponto_v2.html:150-157,343-350`).

   Os dois handlers devolvem os três status em `Response(...)` sem código HTTP alternativo:

   | Status 200 | `app/api/views_core.py` | `app/api/views.py` |
   |---|---|---|
   | `ok` | `:807-818` | `:813-824` |
   | `ja_registrada` | `:521-531` | `:535-545` |
   | `ignorada_intervalo` | `:673-683` | `:682-692` |

   **Não encontrei quarto valor de status 2xx.** Existe outro caminho de retorno 200 por reutilização, mas ele repete `ja_registrada` (`app/api/views_core.py:684-686`; `app/api/views.py:693-695`; `app/ponto/services/resposta_batida.py:14-31`).

   O sincronizador está em **`app/static/pwa.js`**: faz esse POST, lê JSON, mas não consulta `data.status`; remove a pendência por `resp.ok`. Seu indicador é agregado, sem anúncio individual (`app/static/pwa.js:153-165,182-201`).

   No censo desta cópia, **N=1 consumidor direto em `templates/`: a própria v2; zero outros encontrados**. A tela antiga recebe HTML de sua própria rota, distinta dessa API (`app/templates/ponto/bater_ponto_v2.html:343-350`; `app/templates/ponto/bater_ponto.html:533-539`; `app/ponto/urls.py:11-12`; `app/api/urls.py:67`).

   Existência física dos arquivos de logs citados pelos avais, diffs completos das obras e cumprimento dos portões operacionais permanecem **[nao verificado]**. Portanto, estes pareceres não certificam que **ABRE** já esteja satisfeito (`queima_util_1530.txt:3,6`; `CLAUDE.md:37-38`).

## 15/11 15:34 · sessao 70c4e6b3dc35 · - · - · origem=Ronald (tmux)

Achado confirmado, gravidade baixa: **residual `decidir` é apresentado como “Validar no fio”**. `_portas_da_tarefa` calcula `acao=None` antes de mudar o fio residual para `admin` (`app/chamados/services/tarefa_do_dia.py:692-699`); `portas_da_linha` usa então o fallback `PORTA_DO_FIO['admin']`, rotulado “Validar no fio” (`app/folha/retrato_folha.py:980`, `:1055-1064`). Caso sintético reproduzido: fila `{101: 'decidir'}` sem ação exigível publica fio `admin`, ação `None`, e esse botão. O destino ainda abre o fio; problema de orientação da ação.

**LU-01 está curada na produção**, não apenas no teste: a escada contém `('decidir', 'admin')` e a publicação residual a consulta (`app/chamados/juizes.py:1653-1669`; `app/chamados/services/tarefa_do_dia.py:696-699`). Mutante removendo essa entrada morreu por asserção; fonte do mutante e expectativa: `app/chamados/tests/e3_r3_c11_mutantes.py:19-20`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:263-268`.

**Dia no link também possui mordida real**: executei os quatro mutantes retirando a data da projeção, Folha, haiku e href; todos morreram com controles verdes. Sítios: `app/chamados/tests/e3_r3_c11_mutantes.py:21-28`; asserções: `app/chamados/tests/test_e3_r3_c11_sem_banco.py:27-56`.

**C12 preserva explicitamente rota/fila e ação**, e a mudança de esperado residual possui referência à LU-01: `app/chamados/tests/test_e3_r3_c12_sem_banco.py:12-34`; classificação A/B em `app/chamados/tests/e3_r3_c12_classificacao.txt:8-46`. Executei os dois módulos puros `test_e3_r3_c12_sem_banco` e `test_e3_degrau_dono_e_porta`: 77 testes passaram. Execução Django/ORM continua [nao verificado].

Mapa principal:

| Campo | Fonte / escritor | Leitura/publicação |
|---|---|---|
| `data_evento`, identidade | `app/chamados/models.py:2903`; `app/chamados/services/fio.py:829` | Dia do turno em `app/chamados/services/tarefa_do_dia.py:242-263`, `:404-410` |
| Decisão, `ato_em`, texto, absorção | `app/chamados/models.py:4014-4027`, `:4086` | Corte `<=agora`: `app/chamados/services/tarefa_do_dia.py:419-424`; julgamento `:140-185` |
| `veredito_lavrado` | `app/chamados/veredito_lavrado.py:254-276` | Vigência por `esta_stale`: `app/chamados/services/tarefa_do_dia.py:431-434` |
| Âncora / terminal | `app/chamados/services/fio.py:97-117`; catálogo `app/chamados/catalogo/motor.py:24-31` | `app/chamados/services/tarefa_do_dia.py:444-451` |
| Fila e verbo | `app/chamados/juizes.py:1562-1592` lê painel/partição | Publicação em `app/chamados/services/tarefa_do_dia.py:625-699` |
| Oferta Resolver dia | `app/colaboradores/services/calendario.py:126-176` | `app/chamados/services/tarefa_do_dia.py:603-619`; data projetada `:712-715` |

Leitores da publicação: `app/folha/fora_da_folha.py:78-89`, `:199-231`, `:264-283`; `app/chamados/services/furo_sem_canal.py:94-105`; `app/chamados/services/vigia_fio.py:357-374`, `:594-605`; `app/escala/services/cadastro_realidade.py:468-474`; `app/ponto/services/pautas_dos_discordantes.py:84-98`.

Não encontrei novo escritor persistente nem nova lista literal de status nos dois arquivos examinados: a carga usa `TERMINAIS` do catálogo e publica dicionários em memória (`app/chamados/services/tarefa_do_dia.py:447-451`, `:698-699`). Relógio entra por `agora`, validado na porta (`app/chamados/services/tarefa_do_dia.py:723-733`).

Candidato temporal reproduzido, **não recomendado como regressão da R3**: uma resposta posterior a `agora` já exclui a pergunta dos pendentes porque a partição lê `respondida_em` atual (`app/chamados/services/pergunta_no_app.py:76-85`), antes do C6 (`app/chamados/services/tarefa_do_dia.py:527-539`). Reproduzi fio `nada` para uma ação RESPONDER histórica. Esses trechos não aparecem alterados no diff fornecido; atribuição à obra [nao verificado].

## 15/11 15:34 · sessao ba29aa6690aa · - · - · origem=Ronald (tmux)

Achados na Folha/UI:

1. **P2 — positivo conhecido deixa de alarmar quando há retrato legado.** `vigia_retido_calado.censo` substitui o total por `None` se uma linha não tem `dias_sem_leitura`; isso também elimina dias conhecidos de outras linhas/empresas/competências. O comando ignora `None`. Caso reproduzido em memória: uma linha com um dia desconhecido → contador `1`, rc `2`; acrescentando linha legado → contador `None`, rc `0`. Fontes: `app/folha/vigia_retido_calado.py:57`, `app/folha/vigia_retido_calado.py:63`, `app/folha/management/commands/vigiar_retido_calado.py:38`. Os testes cobrem positivo e legado separadamente, não a combinação: `app/chamados/tests/test_e3_r3_c6_sem_banco.py:251`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:258`. A cura do item 5 é parcial.

2. **P2 — relógio ainda lido dentro dos leitores reais da tela.** O `hoje` passou a atravessar `o_que_fazer` quando fornecido, mas o chamador de produção não o fornece; `desenho_do_led` tampouco recebe/repassa o instante. O recorte e cada pílula também reconsultam o relógio por linha. Numa resposta atravessando meia-noite, a mesma linha pode passar pelo filtro como Hoje e ganhar LED/barra Sem ação disponível. Fontes: `app/colaboradores/views.py:986`, `app/folha/retrato_folha.py:460`, `app/folha/retrato_folha.py:553`, `app/folha/retrato_folha.py:650`, `app/folha/retrato_folha.py:830`, `app/folha/views_retrato.py:280`. Reproduzi em memória o mesmo dado retornando os dois LEDs conforme a segunda leitura. O teste c11 só exerce `hoje` explícito: `app/chamados/tests/test_e3_r3_c11_sem_banco.py:230`.

3. **P2 — navegação do calendário aberto pelo novo link não encontra o alvo HTMX.** A Folha abre `calendario_ponto` diretamente no drawer; a resposta é `calendario_partial.html`, cujas setas/modos usam `hx-target="#calendario-container"`. Esse container pertence ao painel anterior e é apagado por `corpo.innerHTML = html`; o partial novo não o cria. Portanto o mês inicial contém a ação, mas navegar mês/modo não encontra o destino. Fontes: `app/folha/retrato_folha.py:1042`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/colaboradores/views.py:1965`, `app/templates/core/_drawer_generico.html:26`, `app/templates/colaboradores/calendario_partial.html:38`, `app/templates/colaboradores/partials/painel_colaborador.html:183`. Execução em navegador [nao verificado].

4. **P3 — comentários continuam afirmando cardinalidades e apresentação incorretas.** “QUATRO fios” contraria os cinco de `FIOS`; “baldes QUATRO” contraria os cinco de `DONOS`; a promessa de marcar também `dias_sem_leitura` não se realiza no template, que só marca `dias_sem_porta`. Fontes: `app/folha/retrato_folha.py:164`, `app/folha/fora_da_folha.py:39`, `app/folha/retrato_folha.py:521`, `app/folha/retrato_folha.py:190`, `app/folha/fora_da_folha.py:96`, `app/templates/folha/_retrato_o_que_fazer.html:46`. Texto técnico segue visível em “Datas (ISO)” e “Sem porta”: `app/templates/folha/_retrato_o_que_fazer.html:43`, `app/templates/folha/_retrato_o_que_fazer.html:46`.

Curas e provas conferidas:

- **1a:** dia do turno atravessa porta → Folha → href; ano/mês são consumidos pelo calendário. `app/chamados/services/tarefa_do_dia.py:714`, `app/folha/fora_da_folha.py:212`, `app/folha/retrato_folha.py:1047`, `app/colaboradores/services/calendario.py:193`. Mutantes desses três transportes e href morreram por asserção; definição em `app/chamados/tests/e3_r3_c11_mutantes.py:21`.
- **1b:** marca é lida no template; mutante removendo a condição morre. `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/chamados/tests/e3_r3_c11_mutantes.py:29`.
- **5:** inclusão em `ALARMA_EM_0` existe e seu mutante morre, mas permanece o achado 1. `app/folha/management/commands/vigiar_retido_calado.py:17`, `app/chamados/tests/e3_r3_c11_mutantes.py:50`.
- **8/c13:** `neutro/#374151` não colide com os sete tokens da paleta; barra usa fundo `#f9fafb` e borda `#e5e7eb`. Os mutantes de colisão e hex morreram. `app/folha/retrato_folha.py:307`, `app/folha/retrato_folha.py:323`, `app/templates/core/_barra_gestao.html:67`, `app/chamados/tests/e3_r3_c11_mutantes.py:53`. Existe status de app chamado `neutro`, mas é outro vocabulário, sem uso dessa paleta: `app/ponto/status_dia.py:84`.
- **9/editor ausência:** link acrescenta `somente_decisao=1`, view suprime o editor dependente de `salvarAusencia`; os dois mutantes morreram. `app/folha/retrato_folha.py:1051`, `app/ponto/views.py:2090`, `app/templates/ponto/partials/painel_ausencia.html:145`, `app/chamados/tests/e3_r3_c11_mutantes.py:63`.

Reexecutei os 18 testes c11 e 12 mutantes relevantes: controles verdes, mortes por asserção; nenhum arquivo escrito. Os limites desses testes estão declarados em `app/chamados/tests/test_e3_r3_c11_sem_banco.py:1` e no runner em `app/chamados/tests/e3_r3_c11_mutantes.py:74`. Renderização Django, navegador e estilo computado [nao verificado].

## 15/11 15:34 · sessao eafaf45faa0a · - · - · origem=Ronald (tmux)

- **P1 — pauta fechada após falha de apuração.** Se `_ins.cxr()` falha, `apurar` omite o colaborador sem anotá-lo em `sem_leitura`; o recorte ainda inclui sua empresa/competência e `sincronizar` fecha sua pauta viva por ausência no alvo. Caso reproduzido em memória: col7, empresa1, competência10/2026, `RuntimeError` no espelho → `apurado=[]`, `sem_leitura=[]`, uma chamada de fechamento. Fontes: `app/ponto/management/commands/pautas_discordantes.py:103`, `:109`, `:126`, `:128`; `app/ponto/services/pautas_dos_discordantes.py:194`, `:198`, `:211`. O teste do comando substitui `apurar` por mock e não cobre esse caminho: `app/chamados/tests/test_e3_r3_c11_sem_banco.py:142`.

- **Cura 3 real, porém incompleta:** empresa/competência fora do recorte ficam protegidas pelo filtro `ancora_id__in`; lista vazia de empresas não expande para a frota. `app/ponto/services/pautas_dos_discordantes.py:73`, `:190`; `app/ponto/management/commands/pautas_discordantes.py:125`. Rodei os três mutantes correspondentes, todos mortos por asserção com controles verdes; definições em `app/chamados/tests/e3_r3_c11_mutantes.py:29`.

- **Cura 4 real:** `censo` exige `agora` quando recebe `hoje` e confere o dia local; o comando passa o instante; o reatador passa seu instante às duas consultas da fila. `app/chamados/services/vigia_fio.py:273`, `:282`; `app/chamados/management/commands/vigiar_fio_morto.py:171`, `:180`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`, `:59`, `:113`. Rodei os quatro mutantes desses pontos, todos mortos; definições em `app/chamados/tests/e3_r3_c11_mutantes.py:37`.

- **Descartado 6:** a R3 não acrescenta juiz ao registro; altera a impressão de um pendente. `app/core/juizes.py:878`. As frases de nascimento dos juízes C19/C20 estão no dado fornecido da principal, `app/docs/CORTES.md:418` e `:419`; execução da trava na principal [nao verificado].

- **Descartado 3b:** `get_user_sistema` escreve eventualmente `User`, não as entidades da obra; existência desse usuário na frota [nao verificado]. `app/core/utils.py:18`. Sua docstring promete exceção por inexistência, embora use `get_or_create`: dívida documental preexistente em `app/core/utils.py:15`.

- **Descartado 10:** permanece condição anterior ao pouso, e não obrigação de construção. A entrega exige medida comparativa das duas árvores e interrupção para resultado inesperado; medida no hash final e pouso [nao verificado]. `app/chamados/tests/e3_r3_c6_entrega.txt:86`, `:94`.

- **Portas/travas:** Pauta passa por `escrever`/`marcar_feita_pelo_sistema`; reata passa por `reatar_perguntas_ao_limbo` → `reapontar_resolvedora`; reversão é gravada antes, trava Colaborador antecede Chamado e fila é relida. `app/ponto/services/pautas_dos_discordantes.py:205`, `:211`; `app/pautas/services.py:153`, `:256`; `app/chamados/services/limbo_folgas.py:245`; `app/chamados/services/fio.py:920`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `:112`, `:113`.

- **M1/autoridade:** `perguntas=[]` é produzido apenas no caminho sem âncora terminal e impede reata; `dono_fora_do_fio` subdivide dias cuja rota já foi negada, sem refazer o dono publicado. `app/escala/services/cadastro_realidade.py:502`, `:520`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:63`; `app/chamados/services/vigia_fio.py:240`, `:601`, `:615`.

- **P3 documental:** os papéis de cron são coerentes, mas comentários ainda descrevem leitores antigos: `VIVOS/respondida_em` e `fio_da_linha`, enquanto os caminhos usam porta da tarefa e `dono_da_linha`. `app/config/crons.py:2289`, `:2261`; `app/chamados/services/furo_sem_canal.py:96`; `app/folha/vigia_retido_calado.py:55`.

## 15/11 15:34 · sessao 245db1a466e1 · E4C-F1V3 · construtor · origem=Code

Curas aplicadas; execução ORM e certificação Code/opus **[não verificado]**. Classificação e tracebacks: [nao verificado]

| Falha | Classe |
|---|---|
| [Selo SQL: executável git ausente](chamados/tests/e4c_f1v8_classificacao.txt:8) | B — pendente |
| [Competência: desarquivar](chamados/tests/e4c_f1v8_classificacao.txt:14) | B — especificação |
| [E3_v4: nove queries](chamados/tests/e4c_f1v8_classificacao.txt:20) | B |
| [C12: desligamento](chamados/tests/e4c_f1v8_classificacao.txt:26) | A/P5 |
| [C11: A6 benefício](chamados/tests/e4c_f1v8_classificacao.txt:32) | A/P5 |
| [C11: limbo misto](chamados/tests/e4c_f1v8_classificacao.txt:38) | A/P5 |
| [C6: limbo misto](chamados/tests/e4c_f1v8_classificacao.txt:44) | A/P5 |
| [Escala: F5](chamados/tests/e4c_f1v8_classificacao.txt:50) | B |
| [O261: custo constante](chamados/tests/e4c_f1v8_classificacao.txt:56) | B |
| [O44: caso1b](chamados/tests/e4c_f1v8_classificacao.txt:62) | B — especificação |
| [Memo: teto zero](chamados/tests/e4c_f1v8_classificacao.txt:68) | B |
| [SNC2: toque em lote](chamados/tests/e4c_f1v8_classificacao.txt:74) | B |
| [Modal: teto](chamados/tests/e4c_f1v8_classificacao.txt:80) | B |
| [Catálogo: quatro declarados](chamados/tests/e4c_f1v8_classificacao.txt:86) | A |
| [Catálogo: universo](chamados/tests/e4c_f1v8_classificacao.txt:92) | A |
| [Catálogo: declaração morta](chamados/tests/e4c_f1v8_classificacao.txt:98) | A |
| [Catálogo: leitura nova dentro](chamados/tests/e4c_f1v8_classificacao.txt:104) | A |
| [Catálogo: sítio novo fora](chamados/tests/e4c_f1v8_classificacao.txt:110) | A |
| [Catálogo: volta4 a](chamados/tests/e4c_f1v8_classificacao.txt:116) | A |
| [Catálogo: volta4 b](chamados/tests/e4c_f1v8_classificacao.txt:122) | A |
| [Catálogo: volta4 c](chamados/tests/e4c_f1v8_classificacao.txt:128) | A |

[Funções curadas](chamados/tests/e4c_f1v8_especificacao.txt:19), [campos: quem escreve/lê](chamados/tests/e4c_f1v8_especificacao.txt:10), [models: protocolo exato, sem migration](chamados/tests/e4c_f1v8_especificacao.txt:2).
GREEN local: **55 + 54 testes (2 skips preexistentes), catálogo 18, contratos AST 14**; [comandos e resultados](chamados/tests/e4c_f1v8_comandos.txt:2).
RED c7: **14 falhas**; RED models: **1 falha**; **20 mutantes novos + 11 anteriores morderam**, com [comandos individuais](chamados/tests/e4c_f1v8_comandos.txt:10).
C12 teve expectativas corrigidas por P5; ORM e frescor após escrita externa continuam [não verificado]: [limites](chamados/tests/e4c_f1v8_especificacao.txt:9).
LEI-AKITA: **padrão=Parallel Change / Expand-Contract**; L2 conserva, sem backfill. [Registro](chamados/tests/e4c_f1v8_especificacao.txt:23). Sem commit, conforme [restrição registrada](chamados/tests/e4c_f1v8_especificacao.txt:25).

PERGUNTA: autoriza substituir a aquisição via `git` por fixture histórica, preservando a asserção FieldError? [Proposta](chamados/tests/e4c_f1v8_especificacao.txt:22).

A regra recebida **“teste B intocado”** impede essa alteração; por isso o selo permanece pendente em [test_e4c_f1v5_tipos_sql.py:132](chamados/tests/test_e4c_f1v5_tipos_sql.py:132).

## 15/11 15:33 · sessao f50acd349e69 · - · - · origem=Ronald (tmux)

- **`r.ok` confirmado:** POST em `app/templates/ponto/bater_ponto_v2.html:343-346`; redirect tratado antes, em `:347`; ramo 2xx chama diretamente `v2Sucesso(tipo,false)` em `:348`, sem ler JSON. JSON só é lido depois, no tratamento não 2xx (`:349`). O texto “Sua batida foi registrada com êxito” está em `:155`; retorno ao `core:dashboard` após 3000 ms em `:157`.
- **Sincronizador:** caminho real `app/static/pwa.js`; faz esse POST em `:153-157`, lê JSON em `:159`, porém não consulta `data.status`: remove a pendência para qualquer `resp.ok` (`:158-165`). Atualiza indicador agregado em `:182`, com “Sincronizando...”/“Online” em `:194-201`, sem anunciar resultado individual. `app/static/js/pwa.js` não existe na cópia.
- **Censo de templates:** N=1 consumidor direto identificado, o próprio v2 (`app/templates/ponto/bater_ponto_v2.html:343-350`); nenhum outro consumidor dessa resposta encontrado no censo por endpoint, nome de rota e status. A tela antiga envia para `_form.action || location.href` e escreve HTML recebido (`app/templates/ponto/bater_ponto.html:533-539`); trata o endpoint de tela, cujo caminho é `app/ponto/urls.py:11`, distinto da API em `app/api/urls.py:67` e `app/api/urls_core.py:14`.
- **Duas cascas são tecnicamente exercitáveis:** teste existente compõe ambas as bases reais e renderiza v2 (`app/ponto/tests/test_l030_templates_renderizados.py:15-30`). UI-CAMERA também tem renderização real e teste de navegador para ambas (`app/ponto/tests/test_camera_alternativa_previa.py:144-158`, `:178-184`). RED/verde específicos de C e smoke humano efetivamente executados: **[nao verificado]**.
- **UI-CAMERA está presente no código:** prévia e remoção do aviso conforme `_fotoBlob`, com handler da alternativa, em `app/templates/ponto/bater_ponto_v2.html:247-273`.
- **Linhas antigas:** `:327` do aval original substituído não é mais o ramo `r.ok`; atualmente é linha vazia, e o ramo está em `:348`. REEMISSAO A não fixa essa linha antiga: sua descrição “hoje `v2Sucesso(tipo,false)`” confere com `:348`.

## 15/11 15:33 · sessao 3383ed049566 · - · - · origem=Ronald (tmux)

O universo da rota está comprovado nas duas cascas: `app/config/urls.py:37` inclui `api.urls`, que liga `ponto/bater/` a `views.api_bater_ponto` em `app/api/urls.py:67`; a casca core inclui `api.urls_core` em `app/config/urls_core.py:6`, e esse arquivo importa `views_core` em `app/api/urls_core.py:5` e liga a mesma rota em `:14`.

Os handlers só aceitam POST autenticado (`app/api/views.py:386-388`; `app/api/views_core.py:372-374`). Todos os retornos explícitos de sucesso usam `Response(...)` sem status HTTP alternativo, portanto 200; os três valores encontrados são:

| Status 2xx | `views.py` | `views_core.py` |
|---|---|---|
| `ok` | `app/api/views.py:813-824` — campo em `:814` | `app/api/views_core.py:807-818` — campo em `:808` |
| `ja_registrada` | `app/api/views.py:535-545` — campo em `:536` | `app/api/views_core.py:521-531` — campo em `:522` |
| `ignorada_intervalo` | `app/api/views.py:682-692` — campo em `:683` | `app/api/views_core.py:673-683` — campo em `:674` |

Existe **quarto caminho 2xx, mas não quarto status**: retorno por reaproveitamento da porta em `app/api/views.py:693-695` e `app/api/views_core.py:684-686`; o helper devolve novamente `ja_registrada` em `app/ponto/services/resposta_batida.py:14-31`, campo `:22`. Não achei outro valor de status 2xx no universo desses dois handlers (`app/api/views.py:388-824`; `app/api/views_core.py:374-818`; `app/ponto/services/resposta_batida.py:7-31`).

Os demais retornos explícitos dos handlers são 404, 403 e 400 (`app/api/views.py:392`, `:411`, `:427`; `app/api/views_core.py:378`, `:397`, `:413`). Ocorrência concreta dos três status em produção permanece **[nao verificado]**.

A própria suite escrita afirma 200 para `ok` e `ja_registrada`, mas não foi executada nesta revisão (`app/api/tests/test_api_bater_ponto.py:617-620`).

## 15/11 15:33 · sessao 162998a4eebf · - · - · origem=Ronald (tmux)

- **1 — NAO PROCEDE quanto à inexequibilidade demonstrada.** Um Codex da F1 mais C em opus soma dois construtores; o teto geral é dois, enquanto a trava de um limita especificamente Codex construtor (`CLAUDE.md:291-292`, `:353-356`; anexo `queima_util_1530.txt:2,6`). Conferência efetiva de todas as vagas antes da abertura **[nao verificado]**. Cópia do HEAD e raia de UI são procedimentos previstos (`CLAUDE.md:39-40`, `:309-313`). `--sem-migrate` existe e recusa migrations pendentes; pendência concreta **[nao verificado]** (`bin/deploy.sh:185-219`). A suite pode esperar a trava; rc 75 significa ausência de vez (`bin/suite.sh:31-33`, `:317-319`).

- **2 — NAO PROCEDE.** Sem vaga não abre; BUG-PAR aberta tem precedência; cota esgotada determina commit parcial e RELATO (anexo `queima_util_1530.txt:6-8`). Condição literal não satisfeita encontra o PAROU geral (`CLAUDE.md:37-38`). Respostas desconhecidas/ausentes e JSON ilegível têm mensagem definida; sítios fora de templates vão ao BACKLOG (anexo `reemissao_1215.txt:4-5`). [nao verificado]

- **3 — Não encontrei colisão normativa demonstrada; colisão por diff depende da conferência das outras cópias [nao verificado].** A proibição anterior recebe exceção nominal para C (anexo `queima_util_1530.txt:6`). A v2 continua **classe A nominal**, com escada inteira (`app/docs/CORTES.md:409,411,413`); “tela” não revoga preverificador e suite inteira antes do opus (`CLAUDE.md:372-381`; `app/docs/PISTA.md:102-114`). C exige expressamente DUAS cascas, portanto não elimina BUG 73 (`CLAUDE.md:202-203`; anexo `queima_util_1530.txt:6`). iPhone/Android identificam plataformas e não comprovam, por si, clique nas árvores admin `base.html` e colaborador `base_app.html`; smoke cumprido **[nao verificado]** (`app/docs/LEIS.md:98-99`).

- **4 — Referências operacionais conferidas:** cópia HEAD, teto, raia UI, deploy e smoke têm suporte nas linhas acima. A grafia `static/js/pwa.js` deve ser conferida: os cortes nominais apontam **`app/static/pwa.js`** (`app/docs/CORTES.md:411,413`). Estados reais de vagas, fila da VM, migrations e cliques não são provados pelas leis **[nao verificado]**.

## 15/11 15:13 · sessao 8a4705563cb1 · BUG-PAR-VOLTA-AGORA · verificador · origem=Code

**AVAL-DEVOLVIDO: COMPLEMENTO-4 QUEIMA — BUG-PAR-VOLTA-AGORA | itens 1, 2, 3 e 4.** Uso **PROCEDE** para objeção procedente, conforme o protocolo; qualquer PROCEDE impede executar. (`revisao_do_aval.txt:6`, bloco do anexo `avais.txt`.)

Nos anexos, conto linhas desde o primeiro conteúdo; nos avais, cada bloco começa em 1 após o separador `=====`.

1. **PROCEDE — as premissas não ficam integralmente certificadas.**

   - **Portão confirmado:** `_espera_do_aval_163` aplica a comparação às metades de intervalo e declara “Sem universo nem ordem de clique”; a escrita consulta esse portão antes de materializar. (`app/chamados/services/materializacao.py:720`, `app/chamados/services/materializacao.py:724`, `app/chamados/services/materializacao.py:1745`.)
   - **Os quatro testes existem:** resposta nova divergente, saída primeiro, volta primeiro e testemunha indisponível no modal. (`app/chamados/tests/test_bug_par_projetado.py:1107`, `app/chamados/tests/test_bug_par_projetado.py:1130`, `app/chamados/tests/test_bug_par_projetado.py:1135`, `app/chamados/tests/test_bug_par_projetado.py:1140`.)
   - **O RED fornecido mostra 61 testes descobertos e quatro falhas:** nas ordens de clique aparecem duas batidas; a resposta nova recebe `aplicavel`; o modal oferece Validar sem testemunha. Isso evidencia aquela execução, não o resultado atual desta cópia **[nao verificado]**. (`red163b.log:1`, `red163b.log:18`, blocos `FAIL` seguintes.) [nao verificado]
   - **As inversões estão documentadas no código:** `test_intervalo_volta_com_hora_diferente_do_marco_espera_e_nao_planta` exige nenhuma batida e espera; o outro é o atual `test_RED_resposta_nova_nunca_recusada_que_gravaria_diferente_espera`, cujo comentário registra o nome anterior `test_fora_do_universo_diferente_segue_aplicavel`. A atribuição histórica exata ao commit `43c820cbc` é **[nao verificado]** nesta cópia. (`app/chamados/tests/test_s139.py:26`, `app/chamados/tests/test_s139.py:49`, `app/chamados/tests/test_bug_par_projetado.py:1108`.)
   - **“RELATO:262” identifica a frase errada:** “metade validada em espera nao quita disputa (C7 pode_fechar le a testemunha estado_aplicacao_pergunta)”. O código exclui validadas da lista sem veredito; `pode_fechar` consulta essa lista e `fio_vivo`, sem consultar aquela testemunha. A posição atual da frase original no RELATO vivo é **[nao verificado]**; não confundir com a linha que relata sua reprovação. (`app/docs/RELATO.md:262`, `app/chamados/models.py:1905`, `app/chamados/models.py:1917`; anexo `relato_topo.txt:72`.)
   - **163 = 60 + 103 está documentado, mas o selecionador existente lê a recusa:** o queryset exclui `recusa_motivo=''`, o classificador traduz o campo e o predicado dos 103 também o exige. Portanto reutilizá-lo para montar o novo universo viola o PROIBIDO. (`app/chamados/services/materializacao.py:327`, `app/chamados/services/materializacao.py:251`, `app/chamados/services/materializacao.py:262`, `app/chamados/services/materializacao.py:278`.)
   - **Há uma alternativa documental por listas de PK**, mencionada pelos censos, independente dos cliques; porém o conteúdo integral dessas listas não foi fornecido. Remontar e conferir exatamente os 163 com o material disponível é **[nao verificado]**. Isso não prova impossibilidade técnica; impede certificar a montagem existente. (Anexo `logs/OITO-LEITORES/y-bugpar-alcance-espera/entrega.md:19`, `logs/OITO-LEITORES/y-bugpar-alcance-espera/entrega.md:22`.)

2. **PROCEDE — permanece dependência de condição de pouso devolvida.** [nao verificado]

   O COMPLEMENTO-4 exige as três provas do CORTES-1358 C2, mas o topo vivo declara **“C2 espera reemissao”**. Repetir essa referência não fornece o produtor que separa NOVA de legado nem o contrato do contador. (`complemento4_bugpar_volta_agora.txt:7`; anexo `relato_topo.txt:15`.)

   A exceção à QUEIMA e à espera do CORTES C é expressamente declarada para BUG-PAR. A política de aguardar a espinha cruzada permanece compatível com o próprio portão. Já a frase do COMPLEMENTO-3 — “nenhuma volta de construtor antes da redefinicao” — não é nominalmente substituída; sua compatibilização exige interpretar a autorização posterior como exceção, interpretação que o literal não explicita. (`complemento4_bugpar_volta_agora.txt:1`, `complemento4_bugpar_volta_agora.txt:3`; `complemento2_queima.txt:4`; `complemento3_queima.txt:5`.)

   **Não procede objeção automática por segunda vaga Codex:** o construtor pedido é opus; o teto escrito admite 1 opus + 1 Codex, mantendo um único Codex construtor por repositório. A ocupação efetiva completa desse teto é **[nao verificado]**. (`complemento4_bugpar_volta_agora.txt:4`; `app/docs/LEIS.md:147`; `CLAUDE.md:355`.)

   C7 intocado corresponde ao predicado registrado; não significa C7 curado. Colisão com o **PORTAO-DA-11** é **[nao verificado]**: seu literal não foi fornecido. (`app/docs/CORTES.md:83`; `app/core/juizes.py:302`; `complemento4_bugpar_volta_agora.txt:4`.)

3. **PROCEDE — a abertura já falhou e há exigências que não fecham juntas como estão.**

   **A condição (1) tem “senão” e ele já foi acionado:** cruzou `app/config/crons.py` e `app/core/tests/test_medida_turno_do_par.py` com F1 e R3. O aval manda registrar espera pelo pouso e **não abrir**; a medição fornecida conclui exatamente isso. Não cabe avançar à construção por disponibilidade de vaga. (`condicao1.txt:3`, `condicao1.txt:6`, `condicao1.txt:11`; `complemento4_bugpar_volta_agora.txt:3`; `CLAUDE.md:37`.)

   **Os testes precisam ser reconciliados explicitamente:** um dos quatro RED é justamente a caracterização invertida de “fora do universo segue aplicável”. O complemento manda desfazer essa inversão, manter os quatro verdes e exigir que fora163 divergente não espere. Os dois testes de ordem também apenas respondem perguntas novas, sem inclusão explícita no universo163. Não se pode presumir essa inclusão para fechar as expectativas. (`complemento4_bugpar_volta_agora.txt:4`, `complemento4_bugpar_volta_agora.txt:5`; `app/chamados/tests/test_bug_par_projetado.py:1108`, `app/chamados/tests/test_bug_par_projetado.py:1117`.)

   **C2 continua pendente:** o I18 disponível mede validação sem **carimbo** e sem recusa, não ausência de **batida**, nem distingue NOVA de legado. Os censos fornecidos documentam 15 do laço e 29 exportadas em disputas fechadas, mas mantêm 8×79 sem reconciliação e não entregam a execução exigida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`; anexo `logs/LU2-CORTES/15/entrega.md:52`, `logs/LU2-CORTES/15/entrega.md:54`; `relato_topo.txt:15`.)

   **“Cópia nova sobre 6e2fc9133 reaplicada no HEAD”** só atende à lei com base efetiva no HEAD e reaplicação do delta; construir simplesmente sobre o hash antigo não atende. Essa reaplicação é **[nao verificado]**. (`CLAUDE.md:39`; `complemento4_bugpar_volta_agora.txt:4`.)

   **A suíte esperar atrás da espinha não é defeito:** o aval preserva a prioridade e exige inteira antes do opus final, conforme a lei. Deve certificar a árvore resultante, não aproveitar o verde anterior. (`complemento4_bugpar_volta_agora.txt:6`, `complemento4_bugpar_volta_agora.txt:7`; `CLAUDE.md:320`, `CLAUDE.md:377`.) [nao verificado]

4. **PROCEDE — o alcance exportado não está operacionalmente fechado.**

   O universo163 **inclui metade em competência exportada pelo censo fornecido**: a projeção b103 contém `3 ABE + 1 EXP`. Isso demonstra pelo menos uma; a quantidade exportada de **todo** o universo163 é **[nao verificado]**. (`logs/LU2-CORTES/15/entrega.md:51`, anexo.)

   `bug_par_103 --apply` está declarado como ato separado, portanto sua menção não autoriza executá-lo nesta construção. Contudo, o comando apenas conta exportadas e depois percorre todas as linhas no apply; a porta descarimba e zera a validação. “Ato separado” não equivale a exclusão mecânica das exportadas. (`complemento4_bugpar_volta_agora.txt:6`, `complemento4_bugpar_volta_agora.txt:7`; `app/chamados/management/commands/bug_par_103.py:52`, `app/chamados/management/commands/bug_par_103.py:83`; `app/chamados/services/validacao.py:294`, `app/chamados/services/validacao.py:297`.)

   Restaurar o comportamento fora163 também altera a passagem de caminhos de escrita: o gate antecede o plantio e alcança laço, lote, passivo e irmã; no passivo exportado, plantio pode seguir para REGEN e Pauta. Logo “qualquer apply proibido” durante construção não comprova ausência de efeito posterior em exportada ou dinheiro. O impacto monetário é **[nao verificado]**; o gravado exportado continua sujeito a `!`, DIFF, reversão e prova. (`app/chamados/services/materializacao.py:1741`, `app/chamados/services/materializacao.py:1757`; `app/chamados/services/passivo_validado.py:444`, `app/chamados/services/passivo_validado.py:453`; `CLAUDE.md:411`.)

## 15/11 15:11 · sessao dc628975f96d · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **RELATO:262 confere nesta cópia.** Afirma: “metade validada em espera nao quita disputa (C7 pode_fechar le a testemunha estado_aplicacao_pergunta)”. O corpo contradiz essa explicação: `sem_veredito_entre` exige `validada_em is None`, e `pode_fechar` consulta essa lista e `fio_vivo`, sem chamar `estado_aplicacao_pergunta`. Fontes: `app/docs/RELATO.md:262`; `app/chamados/models.py:1905`; `app/chamados/models.py:1917`. No RELATO vivo do anexo, a posição atual exata dessa frase original não foi fornecida `[nao verificado]`.

- **O seletor disponível não satisfaz “sem ler recusa_motivo”.** `_qs_recusadas_paridade_de_intervalo` exclui recusa vazia; `_recusa_e_de_paridade` traduz esse campo; o contador chama ambos; a condição dos 103 também chama `_recusa_e_de_paridade`. Fontes: `app/chamados/services/materializacao.py:246`, `:251`, `:258`, `:262`, `:278`, `:330`. O contador documenta `total_aval = universo + len(bug_par_103)` e sua transição após devolução, mas o código não fixa nem prova cardinalidade 163. Fonte: `app/chamados/services/materializacao.py:327`, `:350`, `:353`. As listas primárias `u163.py`/`pks103.txt` citadas nos anexos não estão disponíveis nesta cópia `[nao verificado]`.

- **`bug_par_103 --apply` é escrita real separável.** Sem `--apply`, retorna após o DRY; com apply, grava reversão antes e chama a porta para cada linha. Fontes: `app/chamados/management/commands/bug_par_103.py:65`, `:77`, `:85`. A porta descarimba, zera validação e grava DP/trilha; conserva a disputa fechada. Fontes: `app/chamados/services/validacao.py:269`, `:272`, `:294`, `:297`, `:302`, `:307`. Assim, a proibição de apply durante construção não autoriza executar esse ato nesta revisão; sua menção em “DEPOIS” identifica ato futuro declarado, cujo fechamento operacional não foi provado `[nao verificado]`.

- **O comando conta exportadas, mas não as exclui.** O laço marca `exportada`, a saída conta, e o laço do apply percorre todas as linhas, sem veto de competência. Fontes: `app/chamados/management/commands/bug_par_103.py:48`, `:52`, `:55`, `:62`, `:83`. A devolução em si não planta batida no corpo dessa porta, mas a validação posterior agenda recálculo pela data do fato. Fontes: `app/chamados/services/validacao.py:294`, `:299`, `:580`, `:590`.

- **Restaurar comportamento fora do 163 também alcança caminhos que escrevem.** O gate fica antes de `_materializar_resposta_hora`; seu comentário nomeia laço, lote, passivo e irmã. Fontes: `app/chamados/services/materializacao.py:1741`, `:1745`, `:1755`. No passivo, plantio em exportada pode seguir para REGEN e Pauta. Fontes: `app/chamados/services/passivo_validado.py:442`, `:444`, `:453`, `:458`. Portanto “sem apply” sozinho não prova ausência de efeito posterior em exportada/dinheiro; impacto da implementação futura `[nao verificado]`.

- **Lei escrita sobre gravado exportado continua exigente.** Exige `!`, DIFF prévio, reversão, prova e porta REGEN; L-113 conserva versões TXT, trilha e DIFF anterior ao ato de frota. Fontes: `CLAUDE.md:404`, `:411`, `:412`, `:415`; `app/docs/LEIS.md:154`.

- **Cuidado com a contagem exportada do anexo.** O censo LU2-15 fornecido informa “b103 projecao 3 ABE + 1 EXP”; isso demonstra pelo dado fornecido pelo menos uma metade do universo em exportada. A subtração `76 − 75 = 1` refere-se às oito metades **em espera dentro do 163**, não a todas as 163. Contagem exportada de todo o universo 163 `[nao verificado]`. Os arquivos originais do censo não existem nesta cópia, portanto arquivo:linha primário dessa medição `[nao verificado]`.

## 15/11 15:11 · sessao 77ec9dcbca3e · - · - · origem=Ronald (tmux)

Leitura concluída sem escrita.

- **Portão sem universo confirmado:** `app/chamados/services/materializacao.py:720-731` compara toda metade de intervalo; `:724` diz literalmente “Sem universo nem ordem de clique”. Chamadas da testemunha em `:847` e `:861`; da escrita em `:1745-1749`; proteção tardia no ponto de criação em `:1355-1357`; validação antes da escrita em `app/chamados/services/validacao.py:532-541`. `no_universo_163` está morto/documentado em `materializacao.py:692-693`.

- **Os quatro testes do RED existem na classe**, com linhas deslocadas em relação ao log fornecido: [nao verificado]
  1. `test_RED_resposta_nova_nunca_recusada_que_gravaria_diferente_espera`: `app/chamados/tests/test_bug_par_projetado.py:1107-1115`.
  2. `test_RED_ordem_saida_primeiro_espera`: `:1130-1133`.
  3. `test_RED_ordem_volta_primeiro_o_MESMO_resultado`: `:1135-1138`.
  4. `test_RED_testemunha_que_falha_no_modal_nao_oferece_validar`: `:1140-1153`.
  
  O anexo `red163b.log` fornecido mostra precisamente quatro falhas, 61 testes, `rc=1`: dois testes de ordem plantaram duas batidas; o de resposta nova recebeu `aplicavel`; o modal ofereceu validar sem testemunha. Isso prova o RED daquela execução, **não prova falha nem verde desta cópia** `[nao verificado: execução atual]`. [nao verificado]

- **Incompatibilidade literal relevante:** três dos quatro RED exigem espera em respostas novas sem inclusão explicitada no universo 163. O teste `:1108-1109` diz que antes se chamava `test_fora_do_universo_diferente_segue_aplicavel`; a fixture apenas cria perguntas novas em `:74-76` e as responde em `:78-81`. Os testes das duas ordens usam `_as_duas_ordens` em `:1117-1128`, que apenas responde e exige zero batidas, imutabilidade e espera. O C4 manda “os testes invertidos ... voltam”, “os 4 ... verdes” e simultaneamente MORDE fora163 com divergência sem espera. **Sem redefinir/separar as fixtures, o literal mantém expectativas opostas para o caso fora163.** Inclusão destas fixtures sintéticas no universo 163 não está especificada `[nao verificado]`. O quarto RED (testemunha indisponível) é independente dessa divergência. [nao verificado]

- **Testes invertidos existentes:** `app/chamados/tests/test_s139.py:26` chama-se `test_intervalo_volta_com_hora_diferente_do_marco_espera_e_nao_planta`; `:49-56` documenta caracterização invertida e exige nenhuma batida/espera para E02 declarada versus E03 do marco. Outro é o acima, antigo `test_fora_do_universo_diferente_segue_aplicavel`, documentado em `test_bug_par_projetado.py:1108`. **Atribuição histórica exata ao commit 43c820cbc não verificável:** não há repositório `.git` nesta cópia `[nao verificado]`. Há ainda teste cuja cauda foi invertida pela obra, mas sem atribuição específica a esse commit: `test_RED_2_irma_implausivel_a_tela_nao_diz_aplicavel`, `:315-335`; comentário `:318-321` descreve a inversão para espera.

- **163 = 60 + 103 consta no código**, em `materializacao.py:315-328`; `total_aval` é computado em `:350-353`. **Mas o selecionador existente viola a nova proibição se reutilizado para montar o universo:** `_qs_recusadas_paridade_de_intervalo` exclui `recusa_motivo=''` em `:246-255`; `_recusa_e_de_paridade` lê e traduz esse texto em `:258-262`; `paridade_absolvida_bug_par_103` exige `_recusa_e_de_paridade` em `:265-278`; o contador aplica ambos em `:330-335`. Não existe no código examinado seletor persistente/estático do 163 independente desse texto. O rótulo de devolução lê trilha e independe da recusa em `:281-294`, mas só cobre quem já passou pela devolução, não as 60 nem os 103 ainda não devolvidos. [nao verificado]

- **Os anexos afirmam outra via viável, documental:** leitor y diz ter casado lista de PK de prod, U60 por `r.r:46`, U103 por `pks103.txt`, união sem sobreposição via `u163.py`; LU2-15 repete essa via. Esses arquivos/listas não constam da cópia e seus conteúdos não foram fornecidos; portanto **existência da técnica é evidência documental, mas remontar/verificar exatamente os 163 sem recusa, aqui e agora, é `[nao verificado]`**. Não transformar isso em prova de impossibilidade técnica: é ausência da lista/material executável na cópia, e a implementação foi expressamente deixada ao construtor.

- **RELATO:262 está exatamente na cópia** e afirma: “metade validada em espera nao quita disputa (C7 pode_fechar le a testemunha estado_aplicacao_pergunta)”. `app/docs/RELATO.md:262`. A frase é falsa pelo código: `app/chamados/models.py:1905-1909` só considera sem veredito quem não tem validação nem materialização; `:1911-1917` usa a lista sem veredito e `fio_vivo`, sem chamar a testemunha. Os juízes estão registrados em `app/core/juizes.py:287` e `:302`. O próprio teste legado documenta fechamento sem batida em `test_bug_par_projetado.py:1046-1049`. A ocorrência histórica da reprovação no RELATO vivo não é a frase que o aval manda corrigir.

## 15/11 15:11 · sessao ff9d18f38647 · - · - · origem=Ronald (tmux)

Conferências em leitura; nenhum arquivo alterado.

- **Escopo e condição:** `CLAUDE.md:37` manda parar quando condição literal não fecha. `CLAUDE.md:39-40` exige cópia nascida do HEAD na hora do patch. Portanto “cópia nova sobre 6e2fc9133 reaplicada no HEAD” só satisfaz a lei se a base efetiva for o HEAD, com o delta BUG-PAR reaplicado; uma cópia simplesmente fundada no hash antigo não satisfaz. O resultado reaplicado não está disponível `[nao verificado]`.
- **Cruzamento:** paralelismo é permitido somente sem arquivo comum, independentemente de família/app (`CLAUDE.md:309-311`; `app/docs/PISTA.md:44-48`). O anexo condicao1 mede cruzamento com F1 e R3; o próprio COMPLEMENTO-4 fornece o senão: registrar espera pelo pouso e não abrir. A medição fornecida não permite abertura; contagem independente de commits/diffs não foi possível nesta cópia sem `.git` `[nao verificado]`.
- **Teto:** conferir antes de abrir é obrigatório (`CLAUDE.md:290-292`). O teto é **1 opus + 1 Codex**, total 2 (`app/docs/LEIS.md:147`). Há somente **1 Codex construtor por repositório** (`CLAUDE.md:355-356`; guarda real `bin/codex_pergunta.sh:253`). C4 nomeia **opus** para BUG-PAR, portanto não pede segunda vaga Codex. Ocupação efetiva de todos os construtores não é demonstrada pelo material `[nao verificado]`.
- **QUEIMA/COMPLEMENTO-2/3:** só existem no texto fornecido `[nao verificado quanto a arquivo local]`. C4 substitui nominalmente a proibição QUEIMA e a espera CORTES C; não nomeia a frase do COMPLEMENTO-3 g “nenhuma volta de construtor antes da redefinicao”. Existe tensão textual a explicitar, embora o comando específico posterior de retorno da BUG-PAR possa ser lido como exceção; não afirmar revogação tácita como fato. A política C2 de esperar a espinha cruzada fica preservada pelo próprio portão C4.
- **CORTES C2 devolvido:** topo vivo fornecido declara “C2 espera reemissao” e ausência de produtor para separar NOVA de legado, além do instrumento contar “sem carimbo”, não “sem batida”. C4 volta a exigir as mesmas três provas no pouso, mas não reemite esse contrato nem fornece produtor. Isso continua pendência literal; censos fornecidos reforçam números documentais, mas não substituem execução/instrumento. [nao verificado]
- **Suite:** espera pela trava atrás da espinha é compatível com `CLAUDE.md:95-104` (porta canônica, motivo e trava) e `CLAUDE.md:372-381` (pré-verificador, inteira verde, depois opus). **Não existe colisão por a suite inteira não rodar imediatamente.** Toda a árvore reaplicada deve ser certificada (`CLAUDE.md:320-324`), e não a antiga suite do hash-base. [nao verificado]
- **C7 intocado:** corresponde ao registro escrito: C7 pergunta fio_vivo vazio + ausência de resposta sem veredito (`app/docs/CORTES.md:83`, literal completo em `:323`). “Sem veredito” exclui validadas/materializadas (`app/core/juizes.py:302`; `app/docs/CORTES.md:415` diz “validada/materializada fecha normal”). Logo não apresentar C7 como curado apenas por limitar portão.
- **Dinheiro/exportada:** regra fora do pedido e gravado de exportada exigem `!` (`CLAUDE.md:265-268`); apply em aberta exige reversão prévia, hash exportada intacta e prova de frota (`:406-410`); mudança de gravado exportado só com `!`, DIFF, reversão e porta REGEN (`:411-417`). “Nenhuma trava nem aviso por exportada” (`:421-422`) não revoga essas condições.
- **PORTAO-DA-11:** `rg` na cópia inteira não encontrou o nome; o literal não consta dos anexos fornecidos. Colisão com esse aval **[nao verificado]**.
- **Padrão:** Single Writer Principle existe no glossário (`app/docs/GLOSSARIO.md:12`); selo de padrão e classe existe em `CLAUDE.md:65-70`. Isso por si só não autoriza ampliar o universo, pois escopo literal permanece em `CLAUDE.md:37-38`.

A fonte textual dos anexos não existe como arquivo nesta cópia; para citar anexos com linha, contar as linhas do bloco fornecido e identificar explicitamente “anexo fornecido”.

## 15/11 15:10 · sessao 03daea7a0683 · - · - · origem=Ronald (tmux)

Proposta exata enviada e validada em memória: **17 testes OK**, sem editar arquivos. [nao verificado]

- Migrar declaração `_slot_esperado` → `_q_resposta_conservada`, com metadado explícito aceitando `PAR` e `DECLARADO`; manter classificador intacto.
- Preservar quatro sítios declarados e contador 21; nova leitura medida: SQL=2, demais=1/2/1. Atualizar mutantes para divergência `(3,2)` e compensação total seis.
- Manter censo PAR independente e exigir duas formas PAR no novo sítio; adicionar mutante removendo ambas para provar declaração morta.
- Origem: [catálogo](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e3_catraca_respondedores.py:65), [face SQL](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:1665).

Erro do selo: dependência direta de `git` no teste, [linha 132](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v5_tipos_sql.py:132). Não há cura produtiva compatível com teste intocado; alternativa seria fixture versionada dos dois corpos históricos, preservando a asserção FieldError, mediante autorização para alterar esse teste. [nao verificado]

_5690 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
