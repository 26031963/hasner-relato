# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 12:34** · publicado em **15/11/2026 12:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 12:34 · sessao 8ee20ef0a20d · - · - · origem=Ronald (tmux)

Classificação independente: **1 c; 2 d; 3 e; 4 a; 5 b; 6 b; 7 b; 8 b**.

- **2 d:** falta de selo nos commits e divergência entre padrões não aparecem nas perguntas, respostas ou triagem específica. Aplicar a definição de MODO-COMUM dada pelo usuário, que prevalece sobre CLAUDE.md:393. [nao verificado]
- **3 e:** faltou pedir conferência final da entrega contra a cura e os vereditos. O cron foi visto: “Reconstrução automática prometida sem mecanismo correspondente”; a triagem o **aceitou**, em “v1 rotulo falso ‘o cron reconstroi’ (...) -> c3”. Portanto não cabe b, pois não houve descarte desse achado. A falha remanescente é documental.
- **4 a:** v1.q pergunta quem lê/escreve `Ausencia.status` e se há escritores sem invalidação; não identifica o despertar desnecessário em não aprovada e seu efeito no contador.
- **5 b:** resposta identifica competência civil errada e recálculo de exportadas; triagem põe “COMPETENCIA ERRADA NO DESFAZER” na fila.
- **6 b:** resposta v2 identifica `RuntimeError` escapando das três views; triagem assume “trilha que falha agora vira 500 (...) FILA”. Isso sustenta b pelo componente explicitamente descartado; o componente KERNEL não deve ser apresentado como também visto.
- **7 b:** resposta lista mutantes sobreviventes; triagem “v1 mutantes nao mortos (FILA de reforco do RED...)”. A alteração c5 posterior à v3 não foi auditada naquela resposta.
- **8 b:** resposta “Derivação antiga de falta em ordem incorreta”; triagem “pre-existente em models.py (proibido); FILA”.

Totais: **a 1; b 4; c 1; d 1; e 1**.

## 15/11 12:31 · sessao f0c4e040e92a · REVISAO-REEMISSAO-1215 · verificador · origem=Code

**AVAL-DEVOLVIDO: REEMISSAO-1215 A | item 3.**  
**AVAL-DEVOLVIDO: REEMISSAO-1215 B | itens 3 e 4.**  
A devolução por parte está prevista em `reemissao_1215.txt:20`.

Nas referências aos anexos, conto as linhas dentro de cada bloco, incluindo vazias. **R** = `reemissao_1215.txt`; **P** = `portao_da_11.txt`; **Q** = `queima_ate_a_redefinicao.txt`; **E** = `entrega_relavra_item2.md`.

**Parte A**

1. **NAO PROCEDE — a mudança descrita é tecnicamente executável.**

   Hoje o ramo `r.ok` **não lê JSON**: chama diretamente `v2Sucesso`. A leitura de JSON aparece somente depois, no tratamento não 2xx. Isso confirma o defeito que o novo literal manda corrigir (`app/templates/ponto/bater_ponto_v2.html:327-328`; `R:4`).

   Os três status existem nos dois kernels, em respostas sem código HTTP alternativo:

   | Status | `views_core.py` | `views.py` |
   |---|---|---|
   | `ok` | `app/api/views_core.py:807-818` | `app/api/views.py:813-824` |
   | `ja_registrada` | `app/api/views_core.py:521-531` | `app/api/views.py:535-545` |
   | `ignorada_intervalo` | `app/api/views_core.py:673-683` | `app/api/views.py:682-692` |

   O quarto ramo é defensivo: pode ser exercitado com resposta 200 contendo status desconhecido, ausente ou JSON ilegível. O literal não exige alterar a API para produzi-lo; ocorrência real desse quarto caso em produção **[nao verificado]** (`R:4-6`).

   **O retorno em 3 segundos existe**, com destino `core:dashboard`. **O RED nas duas cascas é possível**: já existe composição desse template com `base.html` e `base_app.html` reais (`app/templates/ponto/bater_ponto_v2.html:157`; `app/ponto/tests/test_l030_templates_renderizados.py:15-30`). RED/verde novos executados **[nao verificado]**.

2. **NAO PROCEDE — os “senão” pedidos estão definidos.**

   O literal cobre os três status conhecidos e encaminha qualquer outro, ausência de status ou JSON ilegível à mensagem de confirmação impossível. Também encaminha sítios fora de `templates/` ao BACKLOG e retira o sincronizador offline do escopo (`R:3-5`). A exigência anterior de mostrar próximo tipo foi expressamente eliminada (`R:4`).

3. **PROCEDE — a abertura de construção encontra a proibição de construtor novo.**

   A manda **CONSTRUIR**, enquanto QUEIMA proíbe **“construtor novo”**. A lista de substituições não inclui QUEIMA; a exceção declarada permite esta revisão pelo Codex, sem liberar construtor (`R:1`, `R:3`, `R:20`; `Q:10`). Redefinição ocorrida, revogação dessa proibição ou construtor de A já em voo **[nao verificado]**. Portanto, o texto não sustenta abrir construtor novo hoje.

   A precedência sobre UI-CAMERA foi corrigida: agora é pouso próprio **depois** dela. Pouso efetivo da UI-CAMERA **[nao verificado]** (`R:3`; `avais_todos.txt`, bloco `ui_camera_alternativa.txt:1`). [nao verificado]

   Permanecem duas exigências que “tela” não dispensa:

   - A v2 é **classe A nominal**, com escada inteira (`app/docs/CORTES.md:409`, `:411`, `:413`).
   - iPhone e Android identificam plataformas; não comprovam, sozinhos, clique nas **duas cascas**. RED renderizado e smoke humano são provas distintas (`R:5`, `R:7`; `CLAUDE.md:202-203`).

   Não há colisão automática apenas pela expressão “smoke no pouso”: PRODUÇÃO-MÁXIMA admite smoke posterior para obra só de tela. Isso não declara os cliques cumpridos nem elimina as demais exigências (`avais_todos.txt`, bloco `producao_maxima.txt:1`, item 5). [nao verificado]

4. **NAO PROCEDE — não encontrei referência material falsa no novo literal de A.**

   Template, POST, três status, temporizador e possibilidade de renderização nas duas cascas conferem nas referências acima. O sincronizador também não anuncia resultado individual; atualiza o indicador de conexão (`app/templates/ponto/bater_ponto_v2.html:322-328`; `app/static/pwa.js:158-202`).

   Censo com N, suite própria e smoke efetivamente realizados continuam **[nao verificado]**; são requisitos futuros de `R:5-7`, não resultados demonstrados. [nao verificado]

**Parte B**

1. **NAO PROCEDE quanto à impossibilidade técnica do procedimento novo; sua entrega completa permanece [nao verificado].**

   Existem os mecanismos necessários para preparar a comparação:

   - **MAPA:** o motor oferece leitura sem gravação, retorno de objetos completos e coleta dos furos. A classificação de cada diferença ainda precisa de fundamento na regra; o retorno numérico não fornece automaticamente essa classificação (`app/ponto/services/fechamento.py:448-483`; `CLAUDE.md:45-51`).
   - **TXT sem escrita:** `montar_linhas_export` existe e declara que apenas devolve linhas e IDs (`app/folha/export.py:690-705`).
   - **Hash:** existe hash de FM por colaborador–competência, excluindo carimbos e outros campos expressamente enumerados. Ele não é, sozinho, hash integral das competências 09/10 (`app/ponto/portas/regen_exportada.py:35-38`, `:62-74`).

   O TXT filtra status e elegibilidade; colaboradores retidos podem ficar fora. Logo, TXT idêntico não substitui a comparação das diferenças desses colaboradores (`app/folha/export.py:716-727`; `CLAUDE.md:45-51`).

   **O MAPA integral dos 50, a lista resultante de B4 e os resultados de B5 não constam do dado fornecido [nao verificado].** “Diferença aplicada = MAPA” é uma comparação possível quando ambos existirem; essa igualdade concreta ainda não está demonstrada (`R:12-14`; `E:1-17`). [nao verificado]

   A foto atual do HEAD fotografa rubricas de FM, recalcula antes de gravar o arquivo e apenas avisa se a gravação falhar. Não satisfaz a foto anterior ao delete; B2 manda justamente reconstruir esse instrumento (`app/ponto/management/commands/recalcular_fechamento.py:57-71`, `:118-139`; `R:11`).

2. **NAO PROCEDE — os “senão” estão presentes.**

   Diferença fora da classe “regra” deixa o colaborador esperando a cura; lista vazia não aplica; comparação diferente determina PAREI com o número (`R:13-14`). Condição literal não satisfeita também encontra o PAROU geral (`CLAUDE.md:37-38`).

3. **PROCEDE na entrega descrita — permanece a questão do DELETE de FM fora de ensaio.**

   A entrega afirma apagar FMs inexistentes na foto e identifica expressamente a pergunta sobre o corte que restringe DELETE em massa a ensaio (`E:8`, `E:16`). No HEAD, a porta correspondente exige banco de ensaio, e o teste exige recusa fora dele (`app/ponto/services/fechamento.py:1764-1768`; `app/folha/tests/test_porta_folha.py:107-112`).

   B1 resolve carimbos; B2 exige reconstrução. Nenhum deles concede exceção explícita para esse DELETE em produção. Assim, **a entrega descrita não pode ser certificada mantendo esse mecanismo**. Solução na reconstrução **[nao verificado]** (`R:10-11`; `E:16`).

   As outras colisões precisam ser distinguidas:

   - **Carimbos:** B1 corrige a restauração byte a byte da entrega. No precedente real, `atualizado_em` recebe agora e `derivado_em` fica **None por padrão**, porque restaurar não é derivar. Não significa colocar todos os carimbos em “agora” (`R:10`; `E:8`, `E:15`; `app/ponto/services/fechamento.py:1383-1393`).
   - **DINHEIRO 1:** B5 acrescenta a prova de frota posterior, no mesmo marco. Continuam obrigatórios reversão antes do apply e hash antes/depois das exportadas; cumprimento **[nao verificado]** (`R:14`; `CLAUDE.md:406-410`).
   - **L-110:** MAPA classificado pela regra é compatível; igualdade de DIFF ou TXT não certifica correção. Fundamento das classificações concretas **[nao verificado]** (`R:12-14`; `app/docs/LEIS.md:136`; `CLAUDE.md:45-51`).
   - **COMPETENCIA-PAGA:** o item 2 preservado ainda exige empilhar depois do pouso quando há arquivo comum. Main/rebase não substitui essa espera. B2 pode ser cumprido respeitando-a; apply antecipado não está autorizado (`R:9`, `R:11`; `P:5`; `E:2`). Pouso efetivo da COMPETENCIA-PAGA **[nao verificado]**. [nao verificado]
   - **QUEIMA:** reconstrução não prova, por si, abertura de outro agente construtor. Continuidade do construtor existente **[nao verificado]**; abertura de novo continua proibida (`E:1`; `R:11`; `Q:10`).

4. **PROCEDE — há referência incorreta preservada e universos não demonstrados.**

   O item 2 original preservado cita `dia_pago.py:359`; o sítio efetivo é **`app/ponto/services/dia_pago.py:359-360`**, onde realmente ocorre delete seguido de recriação. A referência anterior `recalcular_fechamento.py:118` aponta à chamada do serviço; a consulta à guarda está em **`:123-126`** (`P:5`, `P:7`; `app/ponto/management/commands/recalcular_fechamento.py:118-126`).

   As referências e os resultados da entrega pertencem à cópia provisória declarada. Não demonstram reconstrução, suite ou pouso sobre main exigidos por B2; esses resultados são **[nao verificado]** (`E:1`, `E:4-6`, `E:14`; `R:11`). [nao verificado]

   Os **50** são o universo exigido, mas sua lista integral, MAPA por campo/classe, subconjunto B4, hashes 09/10 e igualdade aplicada não foram fornecidos **[nao verificado]** (`P:1`; `R:12-14`; `E:1-17`). Não cabe substituir o novo B4 pelos antigos “50 menos três”: o complemento que estabelecia essa exclusão foi substituído (`R:1`, `R:9`, `R:13`). [nao verificado]

## 15/11 12:30 · sessao 2f880eaea171 · - · - · origem=Ronald (tmux)

Parte B — achados para consolidação:

1. **Executabilidade técnica do MAPA/TXT/hash: não encontrei impossibilidade intrínseca.** O motor oferece leitura sem gravação, objetos completos e coleta de furos (`app/ponto/services/fechamento.py:448`, `:454`, `:471`). O TXT sem escrita existe (`app/folha/export.py:690`, `:697`); há hash por colaborador–competência (`app/ponto/portas/regen_exportada.py:62`). MAPA integral dos 50, associação das classes, lista B4 e resultados B5: **[nao verificado]**.

   A foto existente no HEAD não satisfaz a entrega futura: fotografa rubricas de FM (`app/ponto/management/commands/recalcular_fechamento.py:57`), recalcula primeiro (`:118`) e grava o arquivo depois, permitindo falha (`:127`, `:136`). B2 manda reconstruir; portanto ausência atual do instrumento não demonstra impossibilidade dessa construção (`reemissao_1215.txt:11`).

2. **“Senão”: NAO PROCEDE, no conjunto das cláusulas.** B4 encaminha diferenças não classificadas como regra para espera e lista vazia para não aplicar; B5 determina PAREI nas comparações diferentes (`reemissao_1215.txt:13`, `:14`). A regra geral determina PAROU quando condição literal não fecha (`CLAUDE.md:37`).

3. **Colisões concretas e limites:**

   - **B1 corrige o conflito de carimbos da entrega:** ela restaurava os carimbos antigos; o novo aval manda comparar dados (`entrega_relavra_item2.md:8`, `:15`; `reemissao_1215.txt:10`). No precedente real, `atualizado_em` recebe agora, enquanto `derivado_em` recebe **None por padrão**, pois restaurar não deriva (`app/ponto/services/fechamento.py:1383`, `:1391`, `:1393`). Não se pode afirmar que todos recebem agora; toque e derivação têm significados diferentes (`app/ponto/models.py:454`).
   - **DELETE de FM permanece pergunta não resolvida da entrega:** ela propõe excluir FMs inexistentes na foto e identifica a proibição (`entrega_relavra_item2.md:8`, `:16`). A porta atual de DELETE em massa só permite ensaio (`app/ponto/services/fechamento.py:1764`). B1 altera carimbos; não concede explicitamente exceção para esse DELETE (`reemissao_1215.txt:10`). Colisão de uma implementação que preserve esse mecanismo: **PROCEDE**.
   - A espera pós-COMPETENCIA-PAGA permanece no original preservado; B2 não precisa violá-la: pode reconstruir e empilhar respeitando a precedência (`portao_da_11.txt:5`; `reemissao_1215.txt:9`, `:11`). Apply antecipado não está autorizado; existência de pouso da competência: **[nao verificado]**. [nao verificado]
   - A entrega já existia; B2 não prova abertura de **novo** agente construtor. Colisão específica de B com QUEIMA item 6 depende desse fato: **[nao verificado]** (`entrega_relavra_item2.md:1`; `reemissao_1215.txt:11`; `queima_ate_a_redefinicao.txt:14`).
   - DINHEIRO 1 exige reversão antes, exportada intacta com hash e prova de frota depois no mesmo marco; B5 inclui a última exigência expressamente (`CLAUDE.md:406`; `reemissao_1215.txt:14`). Cumprimento efetivo: **[nao verificado]**.

4. **Referências/universos: parcialmente comprovados; resultados não comprovados.**

   - TXT montado sem escrita: confere; ele filtra elegibilidade e status, portanto pode omitir os retidos (`app/folha/export.py:697`, `:716`, `:725`). TXT igual sozinho não certifica diferenças desses colaboradores (`CLAUDE.md:45`).
   - Hash existente: por colaborador–competência, excluindo carimbos e outros campos; não é hash integral da tabela nem cobre DiaPago (`app/ponto/portas/regen_exportada.py:35`, `:56`, `:62`).
   - Dados de FM completos: usar o objeto retornado evita o recorte do dict; o código documenta campos omitidos (`app/ponto/services/fechamento.py:454`, `:457`).
   - `furo_apurado`/`detalhe` são DiaPago; `derivado_em` é FM (`app/ponto/models.py:369`, `:378`, `:464`).
   - “Diferença aplicada = MAPA” é conferível quando o artefato integral e as medições existirem; neste dado faltam ambos: **[nao verificado]** (`reemissao_1215.txt:12`, `:14`). [nao verificado]

## 15/11 12:30 · sessao 79bd3fc7c81f · - · - · origem=Ronald (tmux)

Achados de lei e precedência, somente leitura:

- **A e B: há impedimento para abrir construtor novo hoje.** A manda CONSTRUIR e B manda reconstruir, mas `queima_ate_a_redefinicao.txt:10` proíbe “construtor novo”. A reemissão substitui quatro avais nomeados e declara exceção apenas à distribuição de trabalho do QUEIMA-UTIL; não revoga esse bloqueio (`reemissao_1215.txt:1`, `:3`, `:11`, `:20`). Redefinição ocorrida e continuidade por construtor já vivo: **[nao verificado]**.

- **B2 versus PORTÃO: precedência continua preservada.** O item 2 original exige, havendo arquivo comum, empilhar depois do pouso da COMPETENCIA-PAGA (`portao_da_11.txt:5`). A entrega modifica `recalcular_fechamento.py` e `fechamento.py`, sobre base provisória (`entrega_relavra_item2.md:1-2`); B2 determina reconstruir sobre main e rebase se COMPETENCIA-PAGA pousar antes (`reemissao_1215.txt:9`, `:11`). Rebase/cópia não demonstram pouso prévio. O estado efetivo de COMPETENCIA-PAGA: **[nao verificado]**. [nao verificado]

- **B1 corrige a colisão dos carimbos.** O selo explica que carimbo parado após escrita afirma fato falso, e cobra carimbo em `update`/`bulk_update` (`app/ponto/tests/test_carimbo_acompanha_a_escrita.py:2`, `:11`, `:14-21`). O precedente real restaura com `atualizado_em=timezone.now()` e `derivado_em=None` por padrão (`app/ponto/services/fechamento.py:1383-1393`). Atenção: o precedente permite `derivado_em` explicitamente pedido; não é proibição absoluta de restaurar esse campo (`app/ponto/services/fechamento.py:1388-1391`).

- **DINHEIRO 1:** B5 agora exige prova de frota depois, no mesmo marco (`reemissao_1215.txt:14`), alinhada a `CLAUDE.md:406-410`. Permanecem necessários arquivo de reversão em `logs/` antes do apply e hash das exportadas antes/depois; prova na sombra não substitui prova posterior (`CLAUDE.md:407-410`). Cumprimento: **[nao verificado]**.

- **L-110:** igualdade de DIFF/TXT não certifica correção. Referência é regra aplicada ao cadastro/batidas, e DIFF mede impacto (`CLAUDE.md:45-51`; `app/docs/LEIS.md:136`). B3/B4 exigir classe “regra” é compatível em tese; MAPA, fundamento de cada classe e certificação correspondente: **[nao verificado]** (`reemissao_1215.txt:12-14`).

- **Front:** v2 é nominalmente classe A e pede escada inteira (`app/docs/CORTES.md:411`, `:413`). Renderizar duas cascas é obrigatório; clique iPhone/Android não prova sozinho clique nas duas cascas (`CLAUDE.md:202-203`; `reemissao_1215.txt:5`, `:7`). Não declarar colisão automática apenas por smoke no pouso: PRODUÇÃO-MÁXIMA autoriza smoke posterior para obra só de tela (`producao_maxima.txt:1`, item 5).

- **DELETE de FM permanece pergunta material da entrega.** Ela afirma apagar FM nascido depois da foto e pergunta sobre o corte (`entrega_relavra_item2.md:8`, `:16`). No HEAD, a porta de DELETE em massa é exclusiva de ensaio (`app/ponto/services/fechamento.py:1764-1768`), com teste que exige recusa fora dele (`app/folha/tests/test_porta_folha.py:107-112`). B1 responde carimbos; não responde explicitamente esse DELETE (`reemissao_1215.txt:10-14`). Isso impede certificar a entrega descrita; a implementação reconstruída e eventual solução sem essa colisão: **[nao verificado]**.

## 15/11 12:29 · sessao 83b17424df27 · - · - · origem=Ronald (tmux)

A — achados para consolidação:

1. **NAO PROCEDE quanto à executabilidade da mudança.** Hoje `r.ok` anuncia sucesso sem ler JSON; só o caminho não 2xx lê JSON (`app/templates/ponto/bater_ponto_v2.html:327-328`). O literal novo corrige isso, reconhece `'ok'`, elimina o próximo tipo e define saída para status desconhecido/ausente e JSON ilegível (`anexo avais_todos.txt, bloco reemissao_1215.txt:4`).

   - Os três status existem em `Response` sem código HTTP alternativo nos dois kernels: `app/api/views_core.py:521-531`, `:673-683`, `:807-818`; `app/api/views.py:535-545`, `:682-692`, `:813-824`. Testes existentes exigem HTTP 200 com `'ok'`/`'ja_registrada'` (`app/api/tests/test_api_bater_ponto.py:280-283`) e exercitam `'ignorada_intervalo'` (`:638-639`).
   - O quarto ramo é tratamento defensivo: resposta desse tipo efetivamente emitida pela API/prod **[nao verificado]**. Um RED com fixture HTTP 200 é compatível com a exigência literal (`anexo reemissao_1215.txt:4-5`); não é necessário que o servidor atual emita JSON inválido.
   - O retorno em 3 s existe e leva ao dashboard (`app/templates/ponto/bater_ponto_v2.html:157`; `app/core/urls.py:17`).
   - Renderizar o mesmo template nas duas bases reais já tem modelo implementado (`app/ponto/tests/test_l030_templates_renderizados.py:15-30`). RED/verde novos e cliques efetivos **[nao verificado]**.

2. **NAO PROCEDE quanto a “senão” ausente.** O novo literal define fallback para todos os demais status/ausência/JSON ilegível e destina sítios fora de `templates/` ao BACKLOG (`anexo reemissao_1215.txt:4-5`). Condição geral que não fecha também determina PAROU (`CLAUDE.md:37-38`). Não depende mais de próximo tipo nem da cura offline (`anexo reemissao_1215.txt:3-4`).

3. **PROCEDE pela proibição vigente de construtor novo.** A abre CONSTRUIR (`anexo reemissao_1215.txt:3`), enquanto QUEIMA proíbe “construtor novo” (`anexo queima_ate_a_redefinicao.txt:10`). A lista explícita de substituições não inclui QUEIMA; a exceção declarada ao QUEIMA-UTIL-TARDE é somente esta revisão pelo Codex (`anexo reemissao_1215.txt:1`, `:22`). Redefinição ocorrida ou outra revogação **[nao verificado]**.

   Ressalvas sem converter em colisão automática:
   - “Tela” não dispensa a escada inteira: `bater_ponto_v2.html` é exceção nominal classe A (`app/docs/CORTES.md:409`, `:411`, `:413`).
   - iPhone e Android identificam plataformas, não comprovam duas cascas; smoke humano nas duas cascas permanece exigido (`CLAUDE.md:202-203`; `app/docs/LEIS.md:99`). A não o revoga (`anexo reemissao_1215.txt:7`).
   - Pouso próprio depois da UI respeita a precedência declarada (`anexo reemissao_1215.txt:3`; `anexo ui_camera_alternativa.txt:1`). Estado atual/pouso/suite da UI **[nao verificado]**. A menção a `estado_uicam.txt` na revisão antiga não fornece esse arquivo no dado atual. [nao verificado]

4. **NAO PROCEDE quanto a referência material falsa demonstrada em A.** Template, chamada, três status, timer e possibilidade de renderização nas duas bases conferem nos sítios acima (`app/templates/ponto/bater_ponto_v2.html:150-157`, `:322-328`; `app/api/urls_core.py:14`; `app/config/urls_core.py:6`; `app/ponto/tests/test_l030_templates_renderizados.py:15-30`). O offline não anuncia resultado individual: apenas atualiza indicador de conexão (`app/static/pwa.js:182-202`). Censo com N, RED/verde, suite e smoke pedidos são entregas futuras **[nao verificado]**.

## 15/11 12:12 · sessao 3101cef531bc · REVISAO-COMBOIO-PRE-OPUS · verificador · origem=Code

**PROCEDE nas quatro perguntas.** A devolução prevista pelo próprio aval é: `AVAL-DEVOLVIDO: COMBOIO-PRE-OPUS | 1, 2, 3, 4`. (Anexo `comboio_pre_opus.txt:9`.)

Nas referências aos avais, conto as linhas dentro de cada bloco, começando por `aval Ronald`. Nenhum arquivo foi alterado.

1. **PROCEDE — há condição cuja execução literal não se sustenta.**

   - O item 7 pede um histórico das suites de 14/11 e 15/11 “pelos logs da trava”. A trava escreve somente o ocupante atual e a hora, sobrescrevendo esse registro com “livre” ao terminar; o wrapper identifica o chamador como `suite` por padrão, sem transmitir obra ou motivo. Esse código não fornece o histórico solicitado; sua existência em outra fonte é **[nao verificado]**. (`comboio_pre_opus.txt:8`; `bin/trava_teste.sh:21`; `bin/trava_teste.sh:40`; `bin/trava_teste.sh:41`; `bin/suite.sh:317`.)
   - **Motivo com várias obras:** o script recusa nomes separados por espaços, mas o item 6 já prevê usar a primeira obra e registrar as demais. Isso está coberto. (`bin/suite.sh:236`; `bin/suite.sh:255`; `comboio_pre_opus.txt:7`.)
   - **Montagem:** `arvore_do_push.sh --montagem` fornece argumentos Docker; o modo normal extrai um commit, e `suite.sh --dir` consome uma cópia previamente preparada. Essas opções não mesclam raias. A ausência dessa automação, sozinha, não demonstra impossibilidade: a regra existente já prevê preparar a cópia combinada. (`bin/arvore_do_push.sh:14`; `bin/arvore_do_push.sh:94`; `bin/arvore_do_push.sh:98`; `bin/suite.sh:284`; `CLAUDE.md:322`.)
   - **PISTA-2:** está documentada como máquina externa, no servidor GLPI, operada por `ssh pista ...`, inclusive `estado`; o registro diz “nenhum arquivo do repo”. Sua parada atual é **[nao verificado]**. (`app/docs/BACKLOG.md:2006`.)
   - **Atribuição do vermelho:** o log pode fornecer o teste para cruzamento com os diffs, mas não prova autoria causal; há inclusive falha reconhecida sem nome de teste. O item 3 já prevê separar quando não houver atribuição. (`bin/regua.sh:175`; `bin/regua.sh:205`; `comboio_pre_opus.txt:4`.)
   - **LOTE 3:** prontidão para suite é **[nao verificado]**. O `! de VINCULO` fornecido restringe expressamente o **pouso**, portanto não prova impedimento anterior à suite. (Anexo `queima_util_tarde.txt:9`.) [nao verificado]

2. **PROCEDE — faltam destinos para resultados negativos.**

   - Depois de retirar uma obra e repetir a suite “UMA vez”, falta dizer o que fazer se essa repetição continuar vermelha com atribuição possível. (`comboio_pre_opus.txt:4`.) [nao verificado]
   - Depois da reprovação de um opus, manda executar uma suite de pouso das restantes, mas não define o ramo dessa suite vermelha. (`comboio_pre_opus.txt:5`.) [nao verificado]
   - Depois do verde, falta definir o destino das obras restantes se um pouso falhar no deploy. O deploy pode terminar em erro inclusive após o reload; a sequência do comboio não trata esse resultado. (`comboio_pre_opus.txt:5`; `bin/deploy.sh:92`; `bin/deploy.sh:419`.)
   - **Conflito de merge tem saída:** essa obra roda sozinha. **Prontidão além de 30 minutos também tem saída:** não esperar mais que isso e rodar sozinha. (`comboio_pre_opus.txt:2`; `comboio_pre_opus.txt:3`.)

3. **PROCEDE — há divergência textual de papéis com aval anterior.**

   QUEIMA-UTIL-TARDE transfere censo, leitura e verificação intermediária ao opus, deixando Codex na final/certificação. COMBOIO exige “verificador Codex do meio fechado” e determina “Codex, papel censo”. O texto anuncia extensão de POUSO-EM-COMBOIO, mas não explicita substituir essa divisão de papéis; a precedência cronológica entre esses blocos é **[nao verificado]**. (Anexos `queima_util_tarde.txt:4`; `comboio_pre_opus.txt:1`; `comboio_pre_opus.txt:3`; `comboio_pre_opus.txt:8`.) [nao verificado]

   As demais acusações precisam destas distinções:

   - Suite conjunta antes dos opus, com finais paralelos, já aparece documentada. Portanto esse formato, isoladamente, não prova colisão com SUITE-ANTES-DO-OPUS. A exigência permanece: verde da árvore examinada antes do final e certificação da árvore que pousará. (`app/docs/RELATO.md:774`; `CLAUDE.md:377`; `app/docs/PISTA.md:113`; `app/docs/PISTA.md:35`.)
   - A remoção seletiva no vermelho altera expressamente a regra vigente de separar o comboio; é mudança proposta pelo item 3. (`CLAUDE.md:324`; `comboio_pre_opus.txt:4`.)
   - Os cinco campos do CONTRATO estão no item 6; publicação no RELATO e envio efetivo são **[nao verificado]**. (`comboio_pre_opus.txt:7`; `app/docs/PISTA.md:170`.)
   - A ordem serial antiga não sustenta colisão vigente sozinha: o BACKLOG registra substituição e construtores disjuntos; PRODUCAO-MAXIMA amplia as vagas novamente. (`app/docs/BACKLOG.md:1836`; `app/docs/BACKLOG.md:1837`; anexo `producao_maxima.txt:1`.)
   - UI-CAMERA já certificada é **[nao verificado]**. TELA-DIZ já prevê os dois ramos: volta conjunta antes da certificação, ou pouso próprio depois. Sua exclusão do primeiro comboio não contradiz esses ramos. (Anexos `backlog_candidatas.txt:1`; `tela_diz_o_que_o_servidor_disse.txt:7`; `comboio_pre_opus.txt:6`.) [nao verificado]

4. **PROCEDE — as referências e os universos não ficam integralmente comprovados.**

   - O anexo atribui UI-CAMERA ao BACKLOG:1930; nessa linha da cópia está **V2-I2-FILA**. O trecho fornecido da UI termina em “falta o” e não contém as anunciadas linhas de HE-REVOGADA e LOTE 3. (Anexo `backlog_candidatas.txt:1`; `app/docs/BACKLOG.md:1930`.)
   - POUSO-EM-COMBOIO existe e prevê duas raias; restrição a laterais e teto de três são alterações deste aval. (`app/docs/PISTA.md:29`; `app/docs/PISTA.md:31`; `comboio_pre_opus.txt:1`; `comboio_pre_opus.txt:2`.)
   - `volta_intermediaria.sh` existe e executa ruff, disjuntor e recorte por `--volta`; isso não fornece o censo histórico pedido. (`bin/volta_intermediaria.sh:7`; `bin/volta_intermediaria.sh:18`; `bin/volta_intermediaria.sh:29`; `bin/volta_intermediaria.sh:32`.)
   - “Espinha E” aparece expressamente no BACKLOG. A classificação específica de HE/LOTE 3 como laterais, seus diffs disjuntos e sua elegibilidade atual são **[nao verificado]**. (`app/docs/BACKLOG.md:767`; `app/docs/BACKLOG.md:768`.)

## 15/11 12:11 · sessao 2cfceb14a425 · - · - · origem=Ronald (tmux)

Para a pergunta 3, há divergências literais que merecem PROCEDE; distinguem-se de alterações que o próprio aval anuncia como extensão.

- **Árvore certificada e opus:** `CLAUDE.md:377-378` exige “Suite INTEIRA da arvore da obra VERDE, depois UM opus final”; `app/docs/PISTA.md:113-114` explicita que opus sobre árvore sem suite verde não conta. O comboio certifica a combinação e manda cada opus receber só o diff individual — verde da árvore individual não foi demonstrado pelo dado [nao verificado]. Também não informa expressamente qual árvore cada opus recebe [nao verificado].
- **Papéis incompatíveis entre avais:** QUEIMA-UTIL-TARDE item 2 transfere censo, leitura e verificador intermediário a opus; COMBOIO item 2 exige “verificador Codex do meio fechado” e item 7 atribui leitura de censo ao Codex. As duas prescrições divergem no literal do anexo, sem revogação expressa dessa divisão de papéis [nao verificado quanto à precedência cronológica entre esses blocos].

Não tratar como colisão automaticamente:

- **Vermelho seletivo:** a regra existente determina separar qualquer comboio vermelho (`CLAUDE.md:324`; `app/docs/PISTA.md:33`). O item 3 modifica isso expressamente como extensão da regra, portanto identifica-se a mudança, sem fingir que o aval a omitiu.
- **Contrato:** os cinco campos exigidos existem no item 6 do aval; requisitos estão em `CLAUDE.md:394` e `app/docs/PISTA.md:170-175`. Efetiva publicação no RELATO e envio pelo wrapper não constam da evidência [nao verificado].
- **ORDEM-POS-E4A2:** a série aparece em `app/docs/BACKLOG.md:1836`, mas a linha seguinte declara substituição do E4b em diante e teto de dois construtores disjuntos (`app/docs/BACKLOG.md:1837`); PRODUCAO-MAXIMA do anexo amplia novamente as vagas. Não sustentar conflito vigente usando só a linha antiga.
- **L-105/L-107:** pousos sequenciais e instrumento separado são compatíveis em princípio com `CLAUDE.md:325-328`, `app/docs/LEIS.md:131` e `app/docs/LEIS.md:133`. O literal não detalha recuperação de deploy que falha [nao verificado].

Achados úteis para pergunta 4:

- **PISTA-2 é externa:** `app/docs/BACKLOG.md:2006` a descreve no servidor GLPI, “nenhum arquivo do repo”, com `ssh pista pede`, `espera`, `log`, `estado`. Busca em `bin/` não retornou referência a pista-2; parada atual [nao verificado].
- **Referência do backlog diverge:** `app/docs/BACKLOG.md:1930` nesta cópia é **V2-I2-FILA**, enquanto o anexo rotula linha 1930 como UI-CAMERA. O trecho raspado termina incompleto e não fornece as anunciadas linhas de HE-REVOGADA ou LOTE 3 [nao verificado].
- **Espinha E está nomeada:** `app/docs/BACKLOG.md:768` classifica E3-RESTO-R3 como “fila 1, espinha E”. O universo completo “série E” do COMBOIO não está enumerado nessa regra [nao verificado].
- **POUSO-EM-COMBOIO existe:** âncora e texto em `app/docs/PISTA.md:28-38`; trata outra raia “principal ou wt-bos”, duas obras, e suite de árvore mesclada. Restrição só a laterais e teto três são mudanças propostas pelo novo aval, não conteúdo da regra preexistente.

## 15/11 12:11 · sessao 8275e2f61626 · - · - · origem=Ronald (tmux)

- **Prontidão de HE-REVOGADA e LOTE 3 para suite: [nao verificado].** O anexo BACKLOG fornecido contém somente UI-CAMERA, truncada em “falta o”. O anexo worktrees registra commits das candidatas, mas não apresenta verificador intermediário fechado, resultado de suite ou os diffs necessários ao item 2 [nao verificado]. [nao verificado]
- **LOTE 3 esperando `!` não prova impedimento à suite.** No literal fornecido, QUEIMA-UTIL-TARDE restringe expressamente o `! de VINCULO` ao pouso; transformá-lo em trava anterior à suite seria hipótese [nao verificado]. [nao verificado]
- **“UI-CAMERA suite já verde”: [nao verificado].** O trecho BACKLOG fornecido não chega ao resultado da suite; a descrição do commit em worktrees informa triagem, não certificação [nao verificado]. [nao verificado]
- **Exclusão da UI do primeiro comboio não demonstra colisão com TELA-DIZ.** O literal fornecido de TELA-DIZ já distingue suite certificada — pouso próprio depois — de suite ainda não certificada — volta conjunta; qual ramo aplica à foto fornecida permanece [nao verificado]. [nao verificado]
- **Há divergência comprovável entre a numeração do anexo e a cópia.** O anexo atribui UI-CAMERA a BACKLOG:1930; nesta cópia, a linha é `V2-I2-FILA`, referente ao motor/folha: `app/docs/BACKLOG.md:1930`. Não citar a linha local como confirmação da UI.
- **“Espinha E” existe no registro.** E3-V10 e E3-RESTO-R3 estão explicitamente classificados assim em `app/docs/BACKLOG.md:766`, `app/docs/BACKLOG.md:767`, `app/docs/BACKLOG.md:768`. Classificação específica HE/LOTE3 e arquivos comuns com espinha em voo: [nao verificado].
- **Suite conjunta antes dos opus e opus paralelos já aparecem no RELATO histórico.** `app/docs/RELATO.md:774` registra comboio verde e opus finais abertos em paralelo; `app/docs/RELATO.md:8809` registra suite conjunta antes dos finais. Portanto esse formato, isoladamente, não sustenta acusação de colisão.
- **Regra existente exige certificar a árvore efetivamente pousada.** `app/docs/PISTA.md:35`–`:38` e pergunta comprovada pelos status em `app/docs/PISTA.md:39`–`:41`.
- **Motivo com várias obras tem histórico de recusa**, mas o aval já oferece alternativa: `app/docs/RELATO.md:8812` registra recusa de motivo com espaço/+ e relançamento com identificador único.

## 15/11 12:11 · sessao d257ce6a77bc · REVISAO-CORTES-LU2-07-08-10 · verificador · origem=Code

**AVAL-DEVOLVIDO: CORTES-LU2-07-08-10 | itens 1, 2, 3, 4.** O próprio aval determina não aplicar quando houver qualquer PROCEDE (`cortes_lu2_07_08_10.txt:5`).

Referências aos avais abaixo usam o nome do bloco de `avais_todos.txt` e sua linha interna; nos outros anexos, a contagem começa no título.

**1) PROCEDE — a execução anunciada não entrega integralmente as condições literais.**

**LU2-07:** existem primitivas para conferir lavra e oráculo por dia-colab: `linhas_do_periodo` devolve DiaPago por data, e `InsumosDoDono` calcula o oráculo. Portanto, a medição não é impossível ([dia_pago.py:833](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/dia_pago.py:833), [e6_oraculo.py:149](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:149)).

Entretanto, os E6SemLavratura saem **antes** do teste zero/zero: sem `minutos_realizados` e `pago_h`, a função retorna `None`; o comando interrompe a comparação nesse ramo. Ausência de linha e linha sem rubricas também retornam `None` em `pago_do_dia`; não basta converter indiscriminadamente `None` em zero ([e6_oraculo.py:263](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:263), [e6_oraculo.py:343](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:343), [dia_pago.py:890](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/dia_pago.py:890)).

Além disso, mudar somente o gate do comando e `vigia_fio.py:697-703` **não faz os 22 desaparecerem da lista do admin**. A lista obtém o dono por `InsumosDoDono.dono`, cujo juiz continua retornando CADASTRO para folga indefinida/sem vínculo; esse caminho não consulta o gate do comando. O trecho indicado do vigia apenas classifica causas d/e ([cadastro_realidade.py:1005](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:1005), [e6_oraculo.py:98](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:98), [e6_oraculo.py:182](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:182), [vigia_fio.py:696](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:696)). A conferência individual atual dos 22 permanece **[nao verificado]**.

**LU2-10:** existe recálculo por competência, empresa e colaboradores, mas o aval não identifica qual execução produzirá a convergência; o comando não tem cron periódico, por decisão expressa ([recalcular_fechamento.py:77](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/recalcular_fechamento.py:77), [crons.py:1148](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/config/crons.py:1148)). O próprio k declara a convergência **não verificada** (`anexo_k.md:51`, `anexo_k.md:94`).

Usar agora a porta do HEAD não satisfaz o instrumento exigido pela RELAVRA: sua foto cobre apenas FechamentoMensal, o arquivo é gravado depois do apply e sua falha não impede a escrita; DiaPago é apagado/recriado. A RELAVRA exige foto/reversão do DiaPago antes do apply (`portao_da_11.txt:5`; [recalcular_fechamento.py:57](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/recalcular_fechamento.py:57), [recalcular_fechamento.py:118](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/recalcular_fechamento.py:118), [dia_pago.py:359](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/dia_pago.py:359)). A inclusão dos dois colaboradores nos 50/47 e a execução coordenada permanecem **[nao verificado]**.

A guarda atual contém somente 09/2026 e não impede nominalmente a 11; portanto, não fundamento a devolução numa suposta proibição geral de recalcular a 11 ([fechamento.py:36](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:36), [fechamento.py:560](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:560)).

**O fato “10 fechada à mão” não é contradito por `paga=True`:** esse campo significa existência de holerite de lote publicado, não pagamento por determinado TXT ([fechamento.py:1522](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1522), [fechamento.py:1539](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1539)). `exportada=False` tampouco prova inexistência de ExportacaoDominio histórico: o juiz considera o vigente não invalidado ([export.py:588](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:588)). Existência de exportações históricas, Pautas DP e eventual contradição com seus conteúdos: **[nao verificado]**.

**2) PROCEDE — faltam destinos para resultados falsos.**

Os **22 têm “senão” explícito**: continuam CADASTRO se não continuarem zero dos dois lados; esse trecho não fundamenta a objeção (`cortes_lu2_07_08_10.txt:2`).

Na **11**, falta dizer o que fazer se o recálculo escolhido não convergir. Se a execução for a RELAVRA, já existe um “senão” obrigatório: qualquer diferença nas provas previstas resulta em PAREI com o número; a frase nova não identifica essa execução nem demonstra essas provas (`cortes_lu2_07_08_10.txt:4`; `complemento_portao_da_11.txt:4`; `anexo_k.md:94`).

Para os **38**, incluí-los no censo é executável, mas o texto não define o destino dos que não satisfizerem “ausência APROVADA”. “Os 2 ficam CADASTRO” refere-se ao recorte dos 21 e não resolve os demais integrantes dos 38; aprovação individual e dono de todo esse conjunto permanecem **[nao verificado]** (`cortes_lu2_07_08_10.txt:3`; `anexo_b2.md:57`; `anexo_p.md:53`).

**3) PROCEDE — há colisões de conteúdo e de escopo.**

A regra geral **“folha não retém dia com ausência APROVADA”** alcança casos que cortes anteriores distinguem: parcial sem período deve ir para CADASTRO; hora ilegível pode conservar FATO_EM_AUSENCIA. O b2 contém precisamente PARCIAL_SEM_PERIODO e FATO_EM_AUSENCIA entre os 21 (`anexo_b2.md:26-28`; [CORTES.md:370](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/CORTES.md:370), [CORTES.md:385](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/CORTES.md:385)). O HEAD declara que ausência aprovada pode não explicar o dia inteiro e mantém retenção por FATO_EM_AUSENCIA ([turnos.py:2670](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/turnos.py:2670), [supra_juiz.py:413](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/supra_juiz.py:413)). L-039 trata de **falta decidida**; não estabelece dispensa geral de retenção para qualquer ausência aprovada ([LEIS.md:87](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/LEIS.md:87)).

Há também contradição literal entre **“não abre obra nova”** e **“vira obra REGISTRADA”**; não está identificada uma obra existente que receberia apenas complemento (`cortes_lu2_07_08_10.txt:1`, `:3`). Registrar uma cura, isoladamente, não viola L-117; transformar CADASTRO/BATIDA em cura de código viola L-099 e o aval CLASSE-D ([LEIS.md:138](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/LEIS.md:138), [CLAUDE.md:179](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:179); `classe_d_na_frente_da_e4d.txt:1`).

A alteração anunciada em `vigia_fio.py` precisa respeitar a preferência da E3-RESTO-R3 e a regra de arquivo comum. O registro local mostra a obra em construção; o diff exato em voo é **[nao verificado]** ([RELATO.md:7](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:7), [RELATO.md:53](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:53), [CLAUDE.md:309](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:309); `producao_maxima.txt:1`).

Quanto à **COMPETENCIA-PAGA c6**, declarar o fato da 10 não autoriza seu pouso: o zero anterior foi medido com cadastro vazio; após marcar a 10, o anexo informa **10.209 e 12.057 divergências** nos dois sítios. A condição LU2-06 P2 continua sem fechar nessa situação (`anexo_lu2.md:20`; `cortes_lu2_e_pista.txt:1`). LU2-10 não manda efetuar essa marcação, portanto uma colisão automática apenas pelo registro do fato é **[nao verificado]** (`cortes_lu2_07_08_10.txt:4`). [nao verificado]

**4) PROCEDE — parte dos números confere, mas há universos indevidamente equiparados.**

| Número/unidade | Conferência |
|---|---|
| **22 CADASTRO; 177 LU2-07** | Existem: 22 = 14 folga indefinida + 8 sem vínculo; 177 = 22 + 155. São dias-colab, não 22 colaboradores. O efeito na lista depende da correção da rota real, examinada no item 1 (`anexo_p.md:26-27`, `:37-40`). |
| **168 / 212 / 247** | O p mede **212**, com ramo A = 168. Outro 168 resulta de 155 + 13; nenhum deles é o total 177. O topo explicita que 247 não aparece na entrega do p (`anexo_p.md:29-32`; `topo_relato_lu2.txt:1`, `:7`). |
| **21 dias, 19 aprovados, 2 sem aprovação** | **Inconsistência documental:** topo/LU2 dizem 19 aprovados, mas b2 afirma literalmente que **todos os 21** têm ausência APROVADA. O p mede **2 CADASTRO / 19 ESTRUTURA**, o que não equivale a medir aprovação. “2 sem aprovação”: **[nao verificado]** (`topo_relato_lu2.txt:3`; `anexo_lu2.md:11`; `anexo_b2.md:33`; `anexo_p.md:53`). |
| **38 retidos sem tarefa** | Confere: D23 + O14 + A1; são 39 `tarefa_nada`, dos quais um não retém. O conjunto contém causas diferentes e se sobrepõe aos 21 pelos nove `tarefa_nada`; não representa 38 ausências aprovadas adicionais (`anexo_b2.md:27`, `:30-32`, `:50`, `:57`). |
| **13 linhas / 6 colaboradores** | Confere no conjunto: **10/2026 emp2 = 9; 11/2026 emp2 = 2; 11/2026 emp3 = 2**. As 13 não pertencem todas à 10 (`anexo_k.md:42`, `:47-52`). |
| **2+2 linhas da 11** | Conferem: col1020/emp2, rubrica 0243, 200→100; col67/emp3, rubrica 0025, 8332→8447. São dois colaboradores e duas alterações de rubrica; convergência futura permanece expressamente não verificada (`anexo_k.md:51-52`, `:94`). |
| **col43; 0200; 3,94 h** | Constam do k e o filtro de feriado somente em `_ft_certa` existe no código. O próprio k deixa sem verificar eventual compensação do feriado; não cabe transformar sua conclusão em prova irrestrita do devido (`anexo_k.md:47`, `:91-92`; [fechamento.py:934](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:934), [fechamento.py:946](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:946)). |
| **37 somente no lido** | Conferem como 33 emp2 + 4 emp3, com **zero linhas de TXT**; não são 37 diferenças de pagamento (`anexo_k.md:65-74`). |

## 15/11 12:11 · sessao 3c7aaadcfdea · - · - · origem=Ronald (tmux)

- `--motivo` aceita uma obra em token único; várias obras separadas por espaço são recusadas pela expressão e guarda de `bin/suite.sh:236,255-262`. O item 6 do aval fornecido prevê o fallback, portanto isso, isoladamente, **não bloqueia**.
- `--montagem` fornece argumentos Docker, estáticos e tmpfs (`bin/arvore_do_push.sh:14-17,28-82`); o modo normal extrai **um commit** (`bin/arvore_do_push.sh:94-103`). `suite.sh --dir` testa a cópia previamente preparada (`bin/suite.sh:284-296,322`). Esses parâmetros **não fazem merge de várias raias**; a impossibilidade de preparar essa cópia por outros comandos não foi demonstrada.
- A identidade operacional e o estado parado da “pista-2” são **[nao verificado]**. A busca em `bin/` não encontrou `pista.?2`; o instrumento existente descreve pista única e lock local (`bin/pista_lock.sh:2-14`).
- A trava guarda somente estado corrente: caminho `/tmp/juliani_db_test.lock`, dono e hora sobrescritos ao tomar; “livre” sobrescrito ao terminar (`bin/trava_teste.sh:21-23,40-42`). O wrapper identifica o chamador como `suite` por padrão, sem repassar o motivo/obra (`bin/suite.sh:317`). **O censo histórico do item 7 não sai dessa fonte literal**; registros por suite/obra de 14/11 e 15/11 são **[nao verificado]**.
- A suite chama Django (`bin/suite.sh:324-325`); o runner configurado herda `DiscoverRunner` e delega a execução (`app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:72,93-100`). Não há atribuição por obra nesses trechos. Há reconhecimento explícito de falhas sem nome e extração de `FAIL:/ERROR:` (`bin/regua.sh:175-179,205-206`). Um nome identificável pode ser cruzado com o diff, mas autoria causal pelo log é **[nao verificado]**; o item 3 já cobre ausência de atribuição.
- `bin/volta_intermediaria.sh` existe, recebe cópia/base/saída e executa ruff, disjuntor e suite com recorte por `--volta` (`bin/volta_intermediaria.sh:2-9,18-32`); produz `.log`, `.porque`, `.fim` e duração (`bin/volta_intermediaria.sh:42-46`).

## 15/11 12:08 · sessao 4f364e2913da · REVISAO-TELA-DIZ-O-QUE-O-SERVIDOR-DISSE · verificador · origem=Code

**AVAL-DEVOLVIDO: TELA-DIZ-O-QUE-O-SERVIDOR-DISSE | itens 1, 2, 3, 4.** O próprio aval manda devolver quando qualquer pergunta procede — `avais_todos.txt`, bloco `tela_diz_o_que_o_servidor_disse.txt:9`.

Nas referências aos anexos, conto as linhas dentro de cada bloco, começando em “aval Ronald”. “UI reconstruída” significa HEAD mais o diff fornecido, aplicado somente em memória. [nao verificado]

1. **PROCEDE — há condições que não fecham no literal.**

   - **O sucesso novo tem `status: 'ok'`, não `'registrada'`.** Implementar literalmente os três valores de MUDA deixa o sucesso normal sem tratamento. Isso ocorre nos dois kernels — `app/api/views_core.py:808`, `app/api/views.py:814`; literal em `avais_todos.txt`, bloco `tela_diz_o_que_o_servidor_disse.txt:3`.

   - **As duas respostas excepcionais não devolvem o próximo tipo.** `tipo` identifica a batida existente; em `ignorada_intervalo`, pode inclusive ser `None`. Nenhuma das respostas contém `proxima_batida` — `app/api/views_core.py:521–531`, `:673–683`. Portanto, a exigência não fecha lendo somente esse POST. Existe uma leitura separada que consulta a autoridade e devolve `proxima_batida`, em `/api/me/`; o aval não especifica essa consulta nem seu tratamento de falha — `app/api/views_core.py:296–297`, `:344`, `app/api/urls_core.py:13`. Inverter o tipo recebido seria dedução da tela, vedada por `CLAUDE.md:20–22`.

   - **O sincronizador não tem anúncio individual equivalente ao da v2.** Ele lê o JSON, descarta seu conteúdo e remove a pendência; depois atualiza apenas “Online/Sincronizando/Offline” — `app/static/pwa.js:158–165`, `:182–202`. Um RED que apenas proíba “registrada com êxito” nesse sincronizador já passa antes da cura: não demonstra o defeito exigido pelo selo — `CLAUDE.md:194`.

   - **Não procede a objeção de que o RED da v2 seria impossível nas duas cascas.** Já existe composição do mesmo template com ambas as bases reais — `app/ponto/tests/test_l030_templates_renderizados.py:15–30`. Isso prova a possibilidade do teste renderizado; não prova a disponibilidade de duas cascas para clique em produção **[não verificado]**.

2. **PROCEDE — faltam os “senão”.**

   - MUDA não determina o comportamento para `status` ausente/desconhecido, JSON ilegível ou o valor real `'ok'` — `avais_todos.txt`, bloco `tela_diz_o_que_o_servidor_disse.txt:3`; contrato real em `app/api/views_core.py:808`.
   - Não determina o comportamento quando falta o próximo tipo, situação presente nas duas respostas citadas; tampouco quando falha uma eventual leitura adicional — `app/api/views_core.py:521–531`, `:673–683`; literal no mesmo bloco, linha 3.
   - O censo possui um “senão” para **sítio encontrado que não couber**: BACKLOG com motivo. Porém, restringe a busca a `templates/` e `static/`, sem encaminhamento explícito para eventual leitor nativo fora desses diretórios — `avais_todos.txt`, bloco `tela_diz_o_que_o_servidor_disse.txt:4`. Existência e comportamento desse cliente na cópia **[não verificado]**.
   - Para o offline, não define destino do anúncio individual nem tratamento dos resultados do lote; o indicador atual pode simplesmente não existir na página — `app/static/pwa.js:185–186`; literal no mesmo bloco, linhas 3–5.

3. **PROCEDE — há conflito de executor; as demais colisões precisam ser distinguidas.**

   - **“Censo Codex” conflita textualmente com CODEX-SÓ-NA-FINAL**, que transfere censo, leitura e verificação intermediária ao opus. O literal não declara exceção a esse comando — `avais_todos.txt`, bloco `tela_diz_o_que_o_servidor_disse.txt:4`; bloco `queima_util_tarde.txt:4`. Uma revogação externa ou precedência temporal adicional **[não verificado]**.

   - **Não é necessário violar API/kernel para resolver a leitura do próximo tipo:** existe `/api/me/`. Acrescentar o campo ao POST violaria o PROIBIDO; deduzi-lo no JavaScript violaria AKITA 2. Nenhuma dessas duas alterações está autorizada — `app/api/urls_core.py:13`, `app/api/views_core.py:344`, `CLAUDE.md:20–22`; bloco `tela_diz_o_que_o_servidor_disse.txt:6`. O aval anterior mantém API/kernel/regra fora da obra — bloco `ui_camera_alternativa.txt:1`.

   - **O dado fornecido ativa pouso próprio depois da UI-CAMERA:** informa suite verde e HEAD `93b532e5a` — `estado_uicam.txt:1–5`; condição em `tela_diz_o_que_o_servidor_disse.txt:7`. Se decidirem pousar juntas, precisam certificar novamente a árvore combinada; o verde anterior não certifica o novo diff — `CLAUDE.md:320–324`. Template v2 e `pwa.js` são exceções nominais de classe A, com escada inteira — `app/docs/CORTES.md:409`, `:411–413`.

   - **Não cabe declarar colisão automática por smoke posterior:** PRODUÇÃO-MÁXIMA autoriza essa sequência para obra só de tela — `avais_todos.txt`, bloco `producao_maxima.txt:1`, item 5. Permanecem distintas a prova renderizada das duas cascas e a prova humana: iPhone/Android identifica plataformas, não comprova sozinho duas cascas — `CLAUDE.md:202–203`, `app/docs/LEIS.md:98–99`. Cliques efetivamente realizados **[não verificado]**.

4. **PROCEDE — parte das referências confere; parte não sustenta a afirmação.**

   | Referência | Conferência |
   |---|---|
   | `bater_ponto_v2.html:327` | Existe no HEAD: `if(r.ok){ v2Sucesso(tipo, false); return; }`. O anúncio aparece em `app/templates/ponto/bater_ponto_v2.html:155`. |
   | Mesmo trecho na UI-CAMERA | Permanece em `app/templates/ponto/bater_ponto_v2.html:348` na UI reconstruída; o hunk fornecido acrescenta 21 linhas antes dele. |
   | `views_core.py:523` | O campo `status: 'ja_registrada'` está em **`:522`**; `:523` contém `id`. A reancoragem pelo HEAD é expressamente permitida — bloco `tela_diz_o_que_o_servidor_disse.txt:2`. |
   | `views_core.py:674` | Confere: `status: 'ignorada_intervalo'`. Os dois ramos retornam `Response` sem código alternativo — `app/api/views_core.py:521–531`, `:673–683`. O teste também exige HTTP 200 para a duplicata — `app/api/tests/test_api_bater_ponto.py:282–283`. |
   | Sucesso `'registrada'` | Não corresponde ao contrato: o valor é `'ok'` — `app/api/views_core.py:808`, `app/api/views.py:814`. |
   | `/api/ponto/bater/` | Rota confirmada pelo prefixo e caminho — `app/config/urls_core.py:6`, `app/api/urls_core.py:14`; cliente em `app/templates/ponto/bater_ponto_v2.html:322`. |
   | `pwa.js` | Existe e envia ao endpoint — `app/static/pwa.js:153`; não anuncia o resultado individual — `:158–202`. |
   | Origin `4b75cba` | Há menção documental a `4b75cba10` — `app/docs/RELATO.md:6`, `app/docs/TICKETS.md:798`. Existência do objeto Git e estado real do origin **[não verificado]**. |
   | Sondas `5bc20b6bea0b6eb9` / `6fd09cb2b91b0585` | Execução, conteúdo e resultados **[não verificado]**. |
   | Universo | O literal mistura resposta imediata da tela com sincronização de pendências; são fluxos distintos — `app/templates/ponto/bater_ponto_v2.html:307–327`, `app/static/pwa.js:126–165`. Não há dado de frota para ampliar esse universo **[não verificado]**. |

## 15/11 12:06 · sessao 61c62b4dde62 · REVISAO-COMPLEMENTO-PORTAO-DA-11 · verificador · origem=Code

**AVAL-DEVOLVIDO: COMPLEMENTO PORTAO-DA-11 item 2 | perguntas 1, 3 e 4.** O procedimento manda devolver quando qualquer ressalva procede (anexo `revisao_avais_todos.txt`, bloco `revisao_do_aval.txt:6`).

Nas referências abaixo, as linhas dos anexos são contadas desde o início do conteúdo de cada arquivo/bloco, incluindo linhas vazias. **C** = bloco `complemento_portao_da_11.txt`; **P** = bloco `portao_da_11.txt`; **E** = `entrega.md`; **B** = `brief_item2.md`.

1. **PROCEDE — há condição inexequível com os elementos e a ordem apresentados.**

   O aval original exige, havendo arquivo comum, empilhar **depois do pouso** da COMPETENCIA-PAGA. O brief já coloca a construção sobre a c6 **provisória**, e adia a suite inteira até a árvore final dessa obra. Entregar foto/reversão nessa cópia não satisfaz o pouso exigido; **apply em prod antes desse pouso não está autorizado pelo literal preservado** (`P:5`; `B:1`; `B:11`; `C:1`). [nao verificado]

   A igualdade “DIFF aplicado = DIFF do z menos três” também não é conferível pelo material fornecido: a entrega apresenta somas/classes, mas não o mapa integral dos 50 por colaborador, competência e campo. Ela menciona `sonda_z.out`, cujo conteúdo integral não foi fornecido. O artefato comparável permanece **[não verificado]** (`C:4`; `E:11`; `E:21`; `E:25-36`; `E:65-72`). [nao verificado]

   **O TXT sem escrita existe:** `montar_linhas_export` declara que apenas devolve linhas e IDs (`app/folha/export.py:690-698`). Portanto, esse exemplo isolado **não** impede a execução; a cobertura dos 47 exige cuidado porque o gerador exclui retidos e filtra status (`app/folha/export.py:716-727`).

2. **NAO PROCEDE — o “senão” das condições está escrito.**

   Para qualquer comparação diferente, o complemento determina **PAREI com o número**. O original determina parar sem apply quando a condição não fecha, e o brief exige recusar o apply se a foto não puder ser gravada (`C:4`; `P:5`; `B:3`). A regra geral também manda parar diante de condição literal não satisfeita (`CLAUDE.md:37-38`).

   Executar automaticamente a reversão após uma divergência não está determinado nessa cláusula; o desfecho expresso é parar (`C:4`).

3. **PROCEDE — há colisão com a precedência preservada pelo próprio aval.**

   O complemento diz que não substitui o original, mas libera o apply “assim que” foto/reversão forem provadas na sombra; o original ainda exige o pouso prévio da COMPETENCIA-PAGA quando compartilham arquivo. O brief identifica justamente uma base provisória dessa obra. Logo, **foto/reversão aprovadas não bastam para antecipar o apply em prod** (`C:1`; `C:3`; `P:5`; `B:1`). [nao verificado]

   DINHEIRO continua exigindo os três elementos juntos: reversão gerada **antes do apply**, exportada intacta com hash antes/depois e prova de frota **depois**, no RELATO e no mesmo marco. A prova anterior na sombra não substitui a prova posterior; seu cumprimento futuro permanece **[não verificado]** (`CLAUDE.md:406-410`; `C:4`).

   Não procede apontar colisão com **L-039** pela decisão falta versus furo: falta lançada cujo tipo desconta é precisamente o significado escrito da lei (`C:2`; `app/docs/LEIS.md:104`). Preservar 09/10 também respeita a restrição específica do aval (`P:5`; `C:4-5`).

   Quanto à **L-110**, DIFF pode medir impacto; não certifica correção pela regra, e amostra não é prova (`CLAUDE.md:45-51`). A mudança para “explicado” não transforma valores errados em corretos: o z classifica 71.400 minutos como fora da regra, incluindo +12.240 de col647/938, que não estão entre os três excluídos (`C:3`; `E:29`; `E:36`). O TXT igual pode ser uma condição de preservação, mas não certifica esse abono (`app/folha/export.py:1076-1079`; `CLAUDE.md:45-51`).

4. **PROCEDE — há referências incorretas e universos ainda não demonstrados integralmente.**

   | Conferência | Resultado e evidência |
   |---|---|
   | `ponto/dia_pago.py:359` | O caminho correto nesta cópia é **`app/ponto/services/dia_pago.py:359-360`**; essas linhas realmente apagam e recriam DiaPago. |
   | Soma de abono sem catálogo | Confere no caminho corrigido: abona todo `tipo_dia='ausencia'` (`app/ponto/services/dia_pago.py:386-396`). Os dois tipos INSS são SUPRIME (`app/ponto/catalogo/ausencias.py:92-93`). |
   | `recalcular_fechamento.py:118` como guarda | Essa linha chama o serviço; a consulta à guarda está em `app/ponto/management/commands/recalcular_fechamento.py:123-126`. A guarda nominal da 09 está em `app/ponto/services/fechamento.py:36-43`. |
   | Foto/reversão no HEAD | A foto atual cobre rubricas numéricas de FM, grava depois do recálculo e apenas avisa em OSError. A entrega futura permanece **[não verificado]** (`app/ponto/management/commands/recalcular_fechamento.py:57-70`, `:118-139`; `B:3-9`). |
   | Unidade colab–competência | O comando aceita `--colabs`, e o serviço restringe o universo por esses IDs (`app/ponto/management/commands/recalcular_fechamento.py:96-102`, `:118-120`; `app/ponto/services/fechamento.py:554-557`). |
   | Universo 50 → 47 | Os 50 e os três excluídos têm suporte nos anexos; a lista completa dos 47 não foi fornecida **[não verificado]** (`E:17`; `E:30-31`; `E:69-71`; `C:3`). |
   | 206 furos | Confere como contagem relatada: 11 F0 + 192 F1 + 3 X. Os três X são **dias**, enquanto as exclusões são **colaboradores inteiros**; não cabe calcular “206 menos 3” como impacto dos 47 (`E:65-72`; `C:3`). |
   | TXT por colab/rubrica | Existe agregador, mas ele usa código Domínio, não PK `colNN`, e ignora registros que não sejam tipo 10. A associação integral dos 47 permanece **[não verificado]** (`app/folha/export.py:310-321`, `:728`). |
   | Hashes 09/10 e TXT 11 idênticos | São condições futuras, não resultados entregues pelo z. A comparação concreta permanece **[não verificado]** (`C:4`; `E:79`; `E:82`). |
   | Campos do brief | `furo_apurado` e `detalhe` pertencem a **DiaPago**; `derivado_em` pertence a **FechamentoMensal**. O trecho “FM: furo_apurado, detalhe” mistura as entidades (`B:8`; `app/ponto/models.py:369`, `:378`, `:398`, `:464`). |

   O resultado original “50 com gravado = lido” também não pode ser declarado cumprido pelo primeiro apply de 47; os três restantes continuam pendentes das curas (`P:5`; `C:3`).

## 15/11 12:05 · sessao 9cbbb5c766ea · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **LEI-AKITA 2:** tela não pode derivar o próximo tipo por conta própria; leitores leem a autoridade comum — `CLAUDE.md:20-22`, `app/docs/LEIS.md:60`. Condição impossível no literal exige parar com o número — `CLAUDE.md:37`, `app/docs/LEIS.md:62`.
- **Duas cascas não tornam o RED impossível:** há precedente que renderiza o mesmo filho com ambas mediante loader, inclusive `bater_ponto_v2.html` — `app/ponto/tests/test_l030_templates_renderizados.py:15-28`. O literal `extends base.html` não prova impossibilidade. O selo pode renderizar ambas ou provar o grafo de includes — `CLAUDE.md:203`, `app/docs/LEIS.md:98`. Smoke manual de sítio alcançável em produção é questão diferente — `CLAUDE.md:202`.
- **Suite certifica a árvore que pousa:** alteração empilhada exige certificação da árvore resultante; comboio exige main + ambas e uma suite — `CLAUDE.md:320-324`. O verde exclusivo da UI-CAMERA não certifica a nova cura. O anexo `estado_uicam.txt` informa que ela já certificou; a autenticidade dessa execução permanece **[não verificado]**, pois só há o dado fornecido. [nao verificado]
- **Classe A, escada inteira:** `bater_ponto_v2.html`, `pwa.js` e `sw.js` estão na exceção nominal — `app/docs/CORTES.md:411-413`; classe A exige escada inteira — `app/docs/CORTES.md:409`. O selo aplica a exceção antes de templates/static — `bin/selo_proporcao.py:113-114`.
- **Padrão tem de existir e ser declarado antes da cura entrar:** `CLAUDE.md:65-70`; “sinal derivado do estado” existe — `app/docs/GLOSSARIO.md:13`. O item PRONTO(d) não substitui essa exigência de entrada — `CLAUDE.md:63-68`.
- **Possível colisão explícita entre os anexos:** a ordem “censo Codex” encontra “CODEX-SO-NA-FINAL”, item 2 de `queima_util_tarde.txt`, que manda censo/leitura/verificação intermediária ao opus. Arquivo e linhas desse anexo na cópia: **[não verificado]**.
- **Smoke posterior requer leitura cuidadosa dos avais:** a regra escrita exige clique nas duas cascas antes do push para `static/js/`, service worker e template base — `CLAUDE.md:202`, `app/docs/LEIS.md:99`. O anexo PRODUCAO-MAXIMA item 5 autoriza pouso com smoke posterior para obra só de tela; não concluir colisão absoluta ignorando essa autorização — localização do anexo na cópia **[não verificado]**.

## 15/11 12:05 · sessao a451a6f522fb · - · - · origem=Ronald (tmux)

- **Censo direto: 2 sítios de POST ao endpoint**: `app/templates/ponto/bater_ponto_v2.html:322` e `app/static/pwa.js:153`. A v2 anuncia sucesso só por `r.ok`, sem ler JSON (`app/templates/ponto/bater_ponto_v2.html:327`). O sincronizador lê JSON, ignora `data.status` e apaga a pendência por HTTP 2xx (`app/static/pwa.js:158-165`).

- **O sincronizador não anuncia o resultado individual da batida.** Depois do lote, chama apenas o indicador (`app/static/pwa.js:182`), cujos textos são “Sincronizando...”, “Online” e “Offline” (`app/static/pwa.js:194-201`). Esse indicador pode nem existir: a função retorna se não encontrar o elemento (`app/static/pwa.js:185-186`). Os elementos encontrados estão no template legado (`app/templates/ponto/bater_ponto.html:41`) e nos dashboards (`app/templates/core/dashboard_colaborador.html:17`; `app/templates/core/dashboard_colaborador_v2.html:17`). Criar anúncio individual para o sincronizador exige definir destino/comportamento que o literal não especifica [não verificado].

- **Offline tem produtores adicionais, mas não novos POSTs diretos ao endpoint.** O legado enfileira em `app/templates/ponto/bater_ponto.html:543` e `:597`; a triagem enfileira em `app/templates/ponto/triagem/apto.html:282`. Online, o legado recebe HTML de `_form.action || location.href` (`app/templates/ponto/bater_ponto.html:533-539`); a triagem submete seu formulário (`app/templates/ponto/triagem/apto.html:235`). Não devem ser classificados automaticamente como leitores do JSON da API.

- **RED nas duas cascas é executável para a v2.** Já há teste que troca `base.html` pelas duas bases reais (`app/ponto/tests/test_l030_templates_renderizados.py:15-23`) e renderiza v2 e triagem (`app/ponto/tests/test_l030_templates_renderizados.py:28-37`). Portanto, afirmar que a v2 só pode ser testada em uma casca não se sustenta nessa cópia.

- **Sincronização sem página aberta não envia POST por conta própria:** o service worker delega às páginas abertas (`app/static/sw.js:116-119`); sem páginas, mostra notificação pedindo abertura do app (`app/static/sw.js:121-127`).

- **Censo do nativo:** não foram encontrados arquivos `.kt`, `.java`, `.swift` ou `.dart` na cópia; implementação/conduta do cliente nativo [não verificado]. A limitação literal do censo a `templates/` e `static/` não oferece tratamento explícito para eventual sítio fora desses diretórios — texto fornecido, sem arquivo local correspondente [não verificado].

## 15/11 12:04 · sessao fa0797d363c2 · - · - · origem=Ronald (tmux)

**Item 3 PROCEDE:** LU2-08 literal conflita com distinções escritas de ausência parcial/aprovada: `app/docs/CORTES.md:370` manda parcial sem período para CADASTRO; `:385` preserva FATO_EM_AUSENCIA em `nao_sei`. O HEAD afirma que ausência aprovada pode cobrir só parte do dia (`app/ponto/turnos.py:2670`) e que FATO_EM_AUSENCIA **acusa e retém**, para evitar dupla saída (`app/ponto/supra_juiz.py:413`). L-039 não autoriza o apagamento geral: trata de **falta decidida** (`app/docs/LEIS.md:87`; `app/docs/CORTES.md:230`).

**Os 38 precisam de censo e separação de donos**, pois o b2 agrega D23+O14+A1 e inclui 13 sem_divergência, chamados e outras causas; aprovação individual dos38 [nao verificado]. Não podem virar automaticamente ESTRUTURA: só ESTRUTURA é fatia, CADASTRO/BATIDA seguem lista (`CLAUDE.md:179`; `app/docs/LEIS.md:122`).

**“Não abre obra nova” × “vira obra REGISTRADA”** apresenta tensão literal interna; registrar é autorizado como rotina em `CLAUDE.md:285`, mas existência de obra anterior que LU2-08 apenas atualizaria [nao verificado]. L-117, isoladamente, não proíbe registrar cura estrutural amparada por corte literal: proíbe o Code inventar lei (`app/docs/LEIS.md:138`).

**Arquivo comum com E3 exige ordenação:** E3-RESTO-R3 consta em `app/docs/BACKLOG.md:768`, construtor c4 em voo em `app/docs/RELATO.md:7`; o registro de achados inclui mudanças no vigia/dono (`app/docs/RELATO.md:53`). A regra exige convivência por arquivo, não por família (`CLAUDE.md:309`). Diff exato da cópia em voo [nao verificado].

**Item 4 PROCEDE:** b2 afirma “Todos os21 ... têm ausência APROVADA”, enquanto topo/LU2 falam19; “2 sem aprovação” não está individualmente provado [nao verificado]. É contradição entre anexos, não ausência comprovada.

**Guardas relevantes:** função real, rótulo=universo e condição literal (`CLAUDE.md:35`, `:37`); consumidor lê a lâmpada (`:173`); retenção atual lê célula/protestos (`app/folha/export.py:1273`, `:1293`). FERIADO-SO-NA-ESCALA-CERTA não localizado na cópia; estado da obra além do anexo [nao verificado].

## 15/11 12:04 · sessao 7d357aae9472 · - · - · origem=Ronald (tmux)

LU2-10 tem ressalvas para as perguntas 1/2; o fato do pagamento manual não é contradito por `paga=True`.

- **Existe porta para recalcular a 11**, inclusive por colaborador: `--mes`, `--ano`, `--empresa`, `--colabs`, `--apply`; ela chama o serviço único. Porém o aval não nomeia qual execução fará isso. Não há recálculo periódico desse comando; evento depende de fato novo e captura falhas. (`app/ponto/management/commands/recalcular_fechamento.py:77-120`; `app/config/crons.py:1148-1155`; `app/ponto/services/fechamento.py:1798-1824`.)
- **A porta atual não atende à condição instrumental de RELAVRA-11-DOS-50**: `foto()` só lê FechamentoMensal; o apply precede a escrita do arquivo, e falha de gravação apenas avisa. O serviço também lavra DiaPago, cujo escritor apaga/recria as linhas. (`app/ponto/management/commands/recalcular_fechamento.py:57-71`, `:118-139`; `app/ponto/services/fechamento.py:1191-1201`; `app/ponto/services/dia_pago.py:359-360`.)
- **Não existe impedimento nominal à 11 pela guarda atual**: ela contém somente `(9,2026)` e exclui colaboradores quando a competência guardada tem empresas exportadas. Logo não afirmar que qualquer exportada é bloqueada; a recusa geral caiu pela L-113. (`app/ponto/services/fechamento.py:36-43`, `:539-564`; `app/docs/LEIS.md:154`.)
- A convergência de col1020/col67 não está provada pelo código; o próprio anexo k a declara não verificada. Interseção com os 50/47, ordem concreta e prova dos hashes/foto/reversão: **[nao verificado]**.
- **`paga=True` não prova pagamento por TXT**: o campo é existência de holerite de lote publicado. Portanto não contradiz “10 fechada à mão”. (`app/ponto/services/fechamento.py:1521-1540`.)
- **`exportada=False` não prova inexistência de ExportacaoDominio histórica**: o juiz consulta o vigente e desconsidera vigente invalidado. O modelo guarda conteúdo/hash e trilha de invalidação; isso não comprova importação/pagamento no Domínio. (`app/folha/export.py:556-594`; `app/folha/models.py:75-100`.) ED históricos/Pautas existentes na 10: **[nao verificado]**.
- O anexo LU2-06-P2-MEDIDA impede usar o zero anterior como prova de pouso c6 depois da marcação da 10: ele informa zero por cadastro vazio e divergências após marcar. LU2-10, literalmente, não manda marcar cadastro nem autoriza pousar c6; colisão automática entre ambos: **[nao verificado]**. [nao verificado]

## 15/11 12:04 · sessao 2f544e86c9f5 · - · - · origem=Ronald (tmux)

LU2-07: **PROCEDE no item 1, pelo alcance literal das mudanças anunciadas**.

- É possível medir os insumos por dia-colab: `InsumosDoDono` calcula `oraculo.minutos` por dia e `linhas_do_periodo` lê `DiaPago` por data. Não cabe afirmar inexistência das primitivas. (`app/ponto/management/commands/e6_oraculo.py:149-154`; `app/ponto/services/dia_pago.py:833-856`)
- Entretanto, o gate atual não alcança os `E6SemLavratura`: `minutos_do_espelho` devolve `None` sem `minutos_realizados`/`pago_h`; o comando sai nesse caso antes do teste zero/zero. (`app/ponto/management/commands/e6_oraculo.py:263-269,342-353`)
- Ausência de linha e linha com rubricas todas zero produzem igualmente `None` em `pago_do_dia`; a implementação precisa distinguir esses casos pela fonte, e não usar conversão genérica `None -> 0`. (`app/ponto/services/dia_pago.py:890-893`; `app/ponto/services/dia_pago.py:854-856`)
- Alterar apenas o gate do comando E6 e `vigia_fio.py:697-703` não produz a prometida mudança dos 22 na lista do admin: a lista chama `InsumosDoDono.dono`, que continua devolvendo CADASTRO primeiro para folga indefinida/sem vínculo; esse caminho não consulta o gate do comando. (`app/escala/services/cadastro_realidade.py:1001-1007`; `app/ponto/management/commands/e6_oraculo.py:98-99,182-195`)
- O vigia lê os dias retidos da folha, resolve portas/lista e só então atribui a causa d/e; `:697-703` não contém comparação de lavra com oráculo nem resultado `sem_divergencia`. (`app/chamados/services/vigia_fio.py:584-599,658-679,696-714`)
- A lista exige assinatura datada; alterar o vigia também não cria nem remove as assinaturas lidas pelo admin. (`app/escala/services/cadastro_realidade.py:1038-1045`)
- A rota da lista usa `sem_motor=True` e CXR lavrado; sem cobertura, o ramo residual pode continuar sem resposta, `None`. (`app/escala/services/cadastro_realidade.py:1001-1007`; `app/ponto/management/commands/e6_oraculo.py:189-195`)

A condição dos 22 possui “senão” expresso no aval fornecido; portanto, **não é ela que fundamenta PROCEDE no item 2**. A conferência atual, individual, dos 22 permanece **[nao verificado]**: os anexos fornecem contagens, não nova execução nem tabela individual verificável nesta cópia.

## 15/11 12:02 · sessao d6857b2e201f · - · - · origem=Ronald (tmux)

Achados de código, só leitura:

- O arquivo do anexo `ponto/dia_pago.py` corresponde, nesta cópia, a `app/ponto/services/dia_pago.py`. As linhas **359–360** realmente apagam e recriam DiaPago; **386–396** realmente abonam todo `tipo_dia='ausencia'` sem consultar o catálogo. O catálogo marca os dois INSS como SUPRIME em `app/ponto/catalogo/ausencias.py:92–93`; o motor diferencia ABONA/SUPRIME em `app/ponto/services/fechamento.py:680–701`.

- **Apply restrito é possível:** `--colabs` em `app/ponto/management/commands/recalcular_fechamento.py:96`, parsing em **102** e transmissão em **118–120**. O serviço restringe o universo em `app/ponto/services/fechamento.py:554–557`; o filtro não inclui colaborador já excluído pelo universo (`:511–515`). A lista literal dos 47 não vem na entrega raspada [não verificado].

- **Foto/reversão futura não está demonstrada no HEAD:** `foto` atual só lê rubricas numéricas de FechamentoMensal e transforma valores em float (`app/ponto/management/commands/recalcular_fechamento.py:57–70`); escreve arquivo **depois** do recálculo (`:118–134`) e OSError só avisa (`:136–139`). Não fotografa DiaPago, PK, `lavrado_em`, `derivado_em` ou metadados; a lista de rubricas importada está em `app/ponto/management/commands/aplicar_09_corte_b.py:67–74`. A implementação e o selo futuros do brief permanecem [não verificado].

- A foto atual nem restringe por `--colabs`: sua assinatura só recebe mês/ano/empresa (`app/ponto/management/commands/recalcular_fechamento.py:57`), e ambos os usos mantêm isso (`:106`, `:121`). Logo sua unidade é competência/empresa inteira, ainda que o apply seja restrito. A saída textual limita mudanças a 40 colabs (`:153–159`).

- **DIFF comparável existe como instrumento**, mas não substitui o artefato faltante do z: `app/ponto/services/diff_frota.py:45–46` retorna três mapas `{colaborador_id:{campo:float}}` para mês/ano fornecidos; `:101–125` produz deltas por campo/colaborador, com limiar de 0,01. A entrega raspada não contém esse mapa integral dos 50 [não verificado]. O instrumento parte de FechamentoMensal existentes (`:62–63`, `:76–88`), portanto não se deve afirmar que mede sozinho casos sem FM anterior.

- **“somente_leitura” com lavratura escreve:** `app/ponto/services/fechamento.py:535–538` e **1191–1201**. A sonda do z só é sem escrita porque afirma interceptação no anexo; não basta citar os argumentos. Interceptação real não foi disponibilizada [não verificado].

- As duas versões lavram pela mesma porta: motor em `app/ponto/services/fechamento.py:1200–1237`; oráculo em **1265–1296**. Há exceções capturadas separadamente em **1238–1241**, **1297–1301**, e erro por colab também é capturado e continua em **1303–1307**. `processados` não certifica restauração ou lavratura integral.

- Guarda nominal existe só para **09/2026** em `app/ponto/services/fechamento.py:36–43`; o comando a consulta em `app/ponto/management/commands/recalcular_fechamento.py:123–126`. **Não há proteção equivalente da 10 nesse HEAD** por essa guarda; a proteção futura pela COMPETENCIA-PAGA é [não verificado].

- O brief mistura campos de entidades: `furo_apurado` e `detalhe` pertencem a DiaPago (`app/ponto/models.py:369`, **378**); `lavrado_em` é `auto_now=True` (`:380`). FechamentoMensal começa em **398** e possui `derivado_em` em **464**. Não tratar “FM: furo_apurado, detalhe” como descrição correta do modelo.

- Referências secundárias do z conferem: grade em `app/escala/utils.py:1607`, encadeamento em **1619**, soma em **1635–1639**, autoridade em **2230** e cálculo do span em **2272–2274**. Os valores específicos de col675 só têm suporte no anexo; não são comprováveis pelo código isolado [não verificado].

## 15/11 12:01 · sessao 4b3cd5f46bd3 · - · - · origem=Ronald (tmux)

- **Existe gerador TXT sem gravação/exportação.** `montar_linhas_export` retorna linhas e IDs sem carimbar aprovação nem registrar exportação; aceita `FechamentoMensal` destacado ou lê o gravado. Portanto essa exigência é executável sem passar pela porta HTTP. `app/folha/export.py:690`, `:697`, `:700`, `:711`, `:746`.
- **A autoridade precisa estar explícita.** Sem `fechamentos=`, o gerador lê o gravado; a porta HTTP atualmente fornece objetos `_fechs_lido`. O comparador existente recusa misturar `gravado` e `lido`. `app/folha/export.py:700`, `:719`; `app/folha/views.py:266`; `app/folha/management/commands/simular_folha.py:182`.
- **Não usar a porta HTTP para essa prova.** Ela grava auditoria e `ExportacaoDominio`. `app/folha/views.py:288`, `:297`.
- **TXT igual pode omitir colaboradores.** Só entram os classificados `entra`, com fechamento de status `aberto`/`aprovado`; eventos zero e sem rubrica também não geram linha. Igualdade vazia não demonstra cobertura dos 47. Resultado concreto desses filtros para os 47: **[não verificado]**. `app/folha/export.py:687`, `:716`, `:717`, `:726`, `:733`.
- **Existe comparação por rubrica**, mas o agregador usa código Domínio preenchido com zeros, não PK `colNN`, e ignora registros 11/datas de faltas; o parser reconhece esses registros. A volta para colaborador exige associação `codigo_dominio`→PK. `app/folha/export.py:310`, `:319`, `:224`, `:253`, `:728`; `app/folha/haiku_export.py:111`.
- **O comparador de simulação também mede retidos**, além das linhas TXT, e exige mesma empresa/competência/autoridade. `app/folha/management/commands/simular_folha.py:160`, `:182`, `:189`, `:202`, `:205`.
- **INSS não entra diretamente nos eventos monetários pelo campo `minutos_abonados`.** A lista de eventos lê HE, noturno, intra e pontualidade; `minutos_abonados` participa da prontidão. Isso não prova TXT real dos 47 inalterado. `app/folha/export.py:439`, `:447`, `:451`, `:1078`.
- **A referência do z a “rubricas 8932/8792” merece restrição.** `tipos_abonados_8932` lê catálogo, mas `montar_linhas_export` aqui emite eventos e bloco de faltas; não chama `datas_abonadas`. Ausência de efeito no TXT dos 47 permanece **[não verificado]** até comparação concreta. `app/folha/export.py:193`, `:201`, `:730`, `:740`, `:746`.

## 15/11 11:46 · sessao 4bdcc344d986 · ABONO-LE-O-CATALOGO · censo · origem=Code

**O achado está confirmado no código:** o abono persistido nasce de `tipo_dia == 'ausencia'`, sem consultar o efeito do tipo; a grade coloca nesse grupo todo tipo coberto diferente do literal `falta`. A regra alcança, portanto, mais tipos que os dois INSS (`app/ponto/services/dia_pago.py:386-401`; `app/escala/utils.py:1210-1222`).

1. **Escritores e calculadores**

| Sítio | Conta ou gravação | De onde vem a classe |
|---|---|---|
| Grade, `_marcar_ausencias` | Produz o insumo: `falta` literal vira falta; demais tipos viram ausência. | Consulta cobertura e catálogo de parciais, mas **não usa o efeito para separar ABONA/SUPRIME/DESCONTA** (`app/escala/utils.py:1174-1191`, `1210-1230`). |
| Célula/cartório | Ata grava `tipo_dia`, previsto e realizado; não calcula minutos de abono nessa montagem. | Copia a grade; o leitor reconstrói esses campos da ata (`app/ponto/services/cartorio.py:738-740`; `app/escala/services/leitor_celula.py:397-423`). |
| `por_dia_da_grade` | Abono por data = previsto − `min(realizado, previsto)`, para todo dia ausência. | **Regra própria por `tipo_dia`**, sem catálogo nem `ausencia_tipo` (`app/ponto/services/dia_pago.py:386-401`). |
| `recalcular_fechamento_mes` → FM | Soma o mapa em `minutos_abonados`; salva aberto e atualiza os demais status. Falha da grade pode produzir `None`. | Herda `por_dia_da_grade`, sem reclassificação nessa conta (`app/ponto/services/fechamento.py:1060-1064`, `1094-1098`, `1137-1144`, `1184`). |
| `lavrar` → DiaPago, versão motor | Recebe o mapa, copia minutos e substitui as linhas da competência/versão. | Recebe valores prontos, sem decidir tipos (`app/ponto/services/fechamento.py:1200-1217`; `app/ponto/services/dia_pago.py:288-292`, `359-360`). |
| DiaPago, versão oráculo | Desde 10/2026 recebe **o mesmo mapa de abono**; o calculador fornece outras rubricas. | Mesma classificação herdada da grade (`app/ponto/services/fechamento.py:1265-1294`; `app/ponto/services/dia_pago.py:295-299`). |
| `retratar_exportada` → DiaPago | Calcula novamente os três campos da grade, cria/atualiza linhas e zera dias ausentes. | Chama a mesma `por_dia_da_grade`, sem catálogo (`app/ponto/services/dia_pago.py:464-495`, `538-545`). |
| Motor, `ResultadoMes.horas_abono` | Soma previsto/60 dos dias obrigatórios **sem trabalho e justificados**; a composição soma as fatias. | Recebe conjuntos: o fechamento usa `Ausencia.efeito`; ABONA justifica, efeito diferente de DESCONTA suprime. Aqui desconhecido também suprime (`app/ponto/motor_calculo_v2.py:2606-2622`; `app/ponto/services/fechamento.py:693-701`, `96-100`, `298-300`). |
| Crédito parcial | Soma `Ausencia.minutos` por data e abate atraso daquele dia. | `TIPOS_PAGOS`, catálogo de parciais e juiz `cobre_parte_do_dia`; não há lista nominal própria (`app/ponto/services/credito_parcial.py:20-29`, `50-61`; `app/ponto/services/fechamento.py:994-1001`). |
| Efeito de ausência para espelho/PDF | Calcula minutos parciais pagos e conjuntos justificados/suprimidos. | Usa `TIPOS_PAGOS` e `a.efeito`, separando ABONA de SUPRIME (`app/ponto/services/efeito_ausencia.py:38-65`). |
| Resumo inicial do PDF | `horas_abono = motor.horas_abono + minutos_parciais/60`. | Herda motor e efeito de ausência; posteriormente pode receber totais substitutos (`app/relatorios/pdf_espelho.py:478-479`, `628`). |
| Prontidão sem snapshot válido | Soma o **previsto inteiro** de todo dia ausência. | **Segunda regra própria**, sem catálogo; também não desconta realizado do abono (`app/folha/export.py:1095-1111`). |
| Restauração de FM | Pode gravar `minutos_abonados` recebido em snapshot. | Não classifica nem recalcula; copia campos fornecidos (`app/ponto/services/fechamento.py:1373-1393`; `app/ponto/management/commands/aplicar_09_corte_b.py:67-73`, `265-268`). |

2. **Leitores**

| Leitor | Gravado ou rederivado? |
|---|---|
| Tela/app do espelho e cartão/PDF por competência | Havendo FM, verifica sua existência e chama **`fechamento_lido`**, que recalcula em memória, somente leitura. O abono usual desses totais **não vem do FM gravado** (`app/relatorios/cartao_pela_celula.py:69-76`, `118`; `app/ponto/services/fechamento.py:1759-1761`; `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/pdf_espelho.py:628`). |
| Mesmos leitores, sem totais do FM e com DiaPago | Leem soma gravada do DiaPago: minutos/60 e quantidade de linhas com abono positivo (`app/relatorios/cartao_pela_celula.py:141-144`, `186-190`, `340-346`; `app/ponto/services/dia_pago.py:699`). |
| PDF em período livre | Permanece motor + parcial, pois a substituição exige período reconhecido como competência (`app/relatorios/pdf_espelho.py:478-479`; `app/relatorios/cartao_pela_celula.py:321-346`). |
| Prontidão com snapshot válido | Lê o objeto FM recebido: realizado + abonado, dividido pelo previsto, com teto de 100% (`app/folha/export.py:1045`, `1076-1080`). |
| Prontidão sem snapshot válido | Rederiva pela regra própria de ausência descrita acima (`app/folha/export.py:1095-1113`). |
| Inteligência | Conta registros de ausência pelo catálogo, não minutos de DP/FM; assiduidade e alertas excluem tipos de `abonam_vigentes()` (`app/inteligencia/resumo.py:39-44`; `app/inteligencia/management/commands/calcular_scores.py:11-16`; `app/inteligencia/management/commands/gerar_alertas.py:53-61`). |
| Tela de ausências; relatório de atestados/CSV | Reclassificam pelo catálogo vivo; relatório conta dias pela expansão da ausência. O contador INSS tem ramo nominal `atestado_inss` (`app/ponto/views.py:2199-2225`; `app/relatorios/views.py:275-319`). |
| Extrato parcial | Soma DiaPago, mas as colunas apresentadas nesse caminho não incluem abono (`app/relatorios/views.py:481-487`, `520-537`). |
| Porta/comparadores | Recalculam pela mesma `por_dia_da_grade` e comparam com FM e soma DiaPago; concordância entre eles não certifica a classificação pelo catálogo (`app/folha/porta_export.py:133-145`, `346-347`). |
| Holerite | Guarda PDF recebido; download abre esse arquivo e maço concatena suas páginas. Cálculo local de abono e conteúdo das rubricas reais: **[nao verificado]** (`app/holerite/models.py:103-110`; `app/holerite/views.py:1193-1203`, `1453-1458`, `935-938`). |

**TXT Domínio:** a montagem efetiva emite extras, folga trabalhada, adicional noturno, intrajornada, atraso/saída e bloco de faltas; nenhum desses eventos lê `minutos_abonados` ou `horas_abono`. A geração pela tela injeta fechamento rederivado; sem injeção, a montagem consulta FM gravado (`app/folha/export.py:439-461`, `717-745`; `app/folha/views.py:207-212`).

Os helpers de 8932 consultam `abonam_vigentes() − FORA_DO_8932`, mas **não são chamados pela montagem examinada**; não há prova de emissão de 8932 nessa cadeia. Faltas vêm de ausências aprovadas classificadas por `descontam_vigentes()`. O código numérico efetivo da rubrica de cada empresa é cadastro: **[nao verificado]** (`app/folha/export.py:179-206`, `730-745`, `984-991`).

A afirmação dos anexos de que o cartão “mostra horas de abono” exige ressalva: o resumo contém `horas_abono`, mas o renderer encontrado imprime **“Abonos: N dia(s)”**; impressão efetiva das horas: **[nao verificado]** (`app/relatorios/cartao_pela_celula.py:118`, `186`; `app/relatorios/pdf_espelho.py:786`).

3. **Autoridade e ESMERIL-SEMÂNTICA**

A autoridade é o **cadastro ativo `TipoAusencia.efeito`**, acessado por `catalogo.ausencias.vigente()`; `efeito_vigente(tipo)` responde a classe e `abonam_vigentes()` fornece o conjunto ABONA. `Ausencia.efeito` delega a essa função; `Ausencia.TIPOS_PAGOS` aponta para o conjunto vivo, não para lista própria (`app/ponto/models.py:571-582`, `680-684`, `850-852`; `app/ponto/catalogo/ausencias.py:249-251`, `267-281`, `293-310`, `455-457`).

O contrato distingue **ABONA = empresa paga/cumprido**, **SUPRIME = sai do previsto**, **DESCONTA = falta sem abono**. A semente classifica ambos os INSS como SUPRIME; o cadastro atual além do dado raspado é **[nao verificado]** (`app/ponto/catalogo/ausencias.py:9-12`, `92-93`, `278`).

Ignoram essa distinção na conta de abono: a classificação literal da grade, `por_dia_da_grade` e o fallback da prontidão; FM, DiaPago e retrato propagam a primeira conta (`app/escala/utils.py:1210-1222`; `app/ponto/services/dia_pago.py:395-396`, `464`; `app/ponto/services/fechamento.py:1060-1062`; `app/folha/export.py:1100-1101`).

| Dimensão | Divergência comprovada |
|---|---|
| Minutos × horas | FM tem `minutos_abonados` inteiro/anulável; DP, float; motor tem `horas_abono`. Leitores dividem explicitamente por 60: **não há erro de unidade demonstrado nessa conversão** (`app/ponto/models.py:349`, `440`; `app/ponto/motor_calculo_v2.py:292`; `app/relatorios/cartao_pela_celula.py:118`, `186`). |
| Significado | Motor abona dia obrigatório sem trabalho; grade abona complemento mesmo com realizado; prontidão alternativa abona previsto inteiro. São contas diferentes sob o nome abono (`app/ponto/motor_calculo_v2.py:2606-2622`; `app/ponto/services/dia_pago.py:393-401`; `app/folha/export.py:1100-1106`). |
| Dias × horas no resumo | Com FM online, horas são substituídas, mas `dias_abono` permanece do motor; no ramo DP, ambos vêm do DP. Divergência numérica real por isso: **[nao verificado]** (`app/relatorios/cartao_pela_celula.py:93-123`, `186-190`; `app/relatorios/pdf_espelho.py:478`). |
| Turno × dia civil | Abono da grade usa sua `data`; períodos do DP recebem mapa de jornada, com alternativa pela data local da entrada. Crédito parcial usa `Ausencia.data`, e atraso parcial usa data local da entrada, sem esse mapa. Caso concreto de desencontro: **[nao verificado]** (`app/ponto/services/dia_pago.py:128-132`, `390-396`; `app/ponto/services/fechamento.py:1217`; `app/ponto/services/credito_parcial.py:27-28`, `32-46`). |
| “Retrato” × insumo atual | O retrato chama `grade_do_fechamento`, que lê célula/ata da janela; não lê uma grade congelada dentro do FM (`app/ponto/services/dia_pago.py:464`; `app/folha/export.py:329-338`). |

4. **Alcance e proteção da 09/2026**

O recalculador regrava competência já lavrada: salva FM aberto, atualiza os demais status e relavra DiaPago. Aprovação não impede essa atualização calculada; a proteção de `FM.save()` é ultrapassada pelo `QuerySet.update()` desse escritor (`app/ponto/services/fechamento.py:1137-1191`; `app/ponto/models.py:492-503`).

| Entrada | Caminho até a regravação |
|---|---|
| Botão e CLI | Chamam o recalculador; CLI sem `--apply` retorna antes (`app/ponto/views.py:1255`; `app/ponto/management/commands/recalcular_fechamento.py:110-120`). |
| Batida criada ou retratada | Acionam evento; ele resolve a competência do fato e recalcula após commit (`app/ponto/registro_batida.py:156-180`, `283`; `app/ponto/services/fechamento.py:1809-1824`). |
| HE, folga aceita e resposta validada | Chamam a mesma porta por evento (`app/ponto/portas/he.py:202`; `app/chamados/services/validacao.py:386`, `471`). |
| Regeneração de célula/vínculo | Pode recalcular as competências tocadas; porta explícita de exportada chama recalculador e retrato (`app/ponto/portas/celula.py:483-496`; `app/ponto/portas/regen_exportada.py:184`, `193`). |
| Backfill | Não exportada: leitura com lavratura de DP; exportada: retrato (`app/ponto/management/commands/lavrar_dias_pagos.py:104-123`). |
| Comandos de obra | Também passam pelo recalculador; alguns fazem ensaio com rollback (`app/ponto/management/commands/aplicar_09_corte_b.py:212`; `app/ponto/management/commands/celula_veredito_velho.py:149-157`; `app/ponto/management/commands/desvio_o68b.py:93-99`; `app/ponto/management/commands/plano_b_no_dinheiro.py:109-114`). |

**A guarda vigente é específica:** `GUARDA_COMPETENCIA[(9,2026)]`. Quando há escrita, inclusive `somente_leitura=True` com `lavrar_dias_pagos=True`, exclui antes do laço os colaboradores das empresas consideradas exportadas. O retrato tem guarda equivalente antes de calcular (`app/ponto/services/fechamento.py:36-43`, `539-564`; `app/ponto/services/dia_pago.py:449-460`).

“Exportada” vem de `competencia_entregue`, por empresa; não é simplesmente o status do FM. A recusa geral de recalcular qualquer exportada foi retirada, e a tabela específica não contém 10/2026 nem 11/2026 (`app/ponto/services/fechamento.py:1496-1515`, `1537`, `485-498`, `36-43`).

Não há cron regular declarado para recalcular nem para o backfill. Existe caminho indireto: cron `detectar_par_relampago --apply --retratar` → retratação → evento → recalculador. Execução efetiva ou agenda externa: **[nao verificado]** (`app/config/crons.py:1043-1051`, `1148-1155`, `605`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/registro_batida.py:283`).

**Contrafactual solicitado:** mudada a função `por_dia_da_grade`, recalcular propagaria o novo mapa para FM e DP; retratar propagaria para DP. Na 09 das empresas exportadas, ambos são barrados pelas guardas acima; leitura pura continua podendo devolver outro número sem alterar o gravado (`app/ponto/services/fechamento.py:1060-1062`, `1212`, `539-564`, `1759-1761`; `app/ponto/services/dia_pago.py:451-464`).

A proteção **não cobre toda escrita possível**: `restaurar_fechamento` atualiza campos fornecidos sem consultar a guarda, e `lavrar` não a verifica internamente. Restauração copia snapshot; não aplica automaticamente uma regra nova (`app/ponto/services/fechamento.py:1373-1393`; `app/ponto/services/dia_pago.py:288-292`, `359-360`; `app/ponto/management/commands/aplicar_09_corte_b.py:265-268`).

5. **Contraexemplo que deve permanecer abonado**

`atestado` até 15 dias é ABONA na semente, assim como férias e licença-maternidade. Num dia obrigatório sem trabalho, classificado como ABONA pelo catálogo vigente, o motor soma o previsto/60 e a grade soma o previsto em minutos; suprimir esse abono contrariaria a classificação (`app/ponto/catalogo/ausencias.py:95`, `99-101`; `app/ponto/motor_calculo_v2.py:2606-2622`; `app/ponto/services/dia_pago.py:393-396`).

O contraexemplo inclui **tipo criado pelo cadastro com `efeito='abona'`**, mesmo ausente da semente: ele entra em `abonam_vigentes()` pelo campo, não pelo nome (`app/ponto/models.py:565-582`; `app/ponto/catalogo/ausencias.py:249-251`, `308-310`).

_5625 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
