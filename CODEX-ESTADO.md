# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 13:56** · publicado em **16/11/2026 13:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 13:56 · sessao f6c24406d92e · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**VEREDITO: REPROVA.** P1/P2 ficaram parcialmente cobertos: o navegador testa o fragmento em um hospedeiro artificial, sem percorrer o carregamento real do painel. O selo pode permanecer verde com esse carregamento quebrado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:247`; `app/templates/colaboradores/vincular.html:143`).

1. **P1/P2 — a alternância é medida, mas a integração real continua descoberta.** O teste confirma a casca renderizada, dispara `change` e compara o resultado; remover `onchange` produz o estado antigo. Há mordida para esse comportamento isolado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:237`, `:201`, `:255`, `:262`).

   Porém, inserir o fragmento antes de `</body>` **não representa sua chegada real**:

   - Na página de vínculos, que estende `base.html`, ele chega por `fetch`, entra via `innerHTML` em `.vinc-painel`, tem scripts executados explicitamente e passa por `htmx.process` (`app/templates/colaboradores/vincular.html:1`, `:147`, `:150`, `:156`, `:161`).
   - No drawer, o destino é `#drawer-geral-corpo`; a abertura altera a transformação do ancestral, e o processamento HTMX acontece depois da inserção (`app/templates/core/_drawer_generico.html:8`, `:16`, `:26`, `:33`).
   - O teste usa apenas `<div id="uvc-painel">`, como HTML inicial. Não chama nenhum desses carregadores. Uma regressão que impeça `carregarPainel` de inserir o fragmento não interfere nessa medição (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:240`; `app/templates/colaboradores/vincular.html:143`).

   Os ancestrais também importam: a tela real possui largura limitada, `overflow:hidden` e área de rolagem; o drawer nasce deslocado para fora da tela. A sonda consulta somente `display` do próprio bloco, sem verificar abertura, área ou recorte por ancestrais. Portanto, “bloco com display” não certifica “painel visível e utilizável” (`app/templates/colaboradores/vincular.html:5`; `app/templates/core/_drawer_generico.html:3`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`).

2. **A prova da cascata também admite falso verde.** O helper religa os estáticos, mas o teste não confirma que o CSS carregou e descarta os erros retornados pelo navegador. Como a alternância usa `hidden` e um handler inline, o resultado esperado pode sobreviver à ausência da folha externa (`app/core/tests/test_smoke_chromium.py:93`, `:123`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`). Carregamento efetivo nesta execução: [nao verificado].

3. **P3 — respondido no ponto levantado.** Exigir HTTP 200 e dois/sete blocos elimina o verde por duas respostas de erro iguais. A medição continua sendo quantidade de queries; bytes são apenas impressos, sem limite ou medição de tempo (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`).

4. **P4 — agora confere argumentos, mas não associa cada retorno à renderização.** A asserção exige que as duas chamadas corretas estejam presentes; permite chamadas adicionais. Como todas recebem a mesma sentinela, esse teste isolado aceita chamadas corretas descartadas e renderização alimentada por outra chamada. Os testes de datas acrescentam cobertura, mas a nova asserção não distingue os retornos por calendário/mês (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:150`, `:154`, `:157`, `:107`, `:114`).

Quanto aos descartes:

- **D1:** a linha citada confirma o problema semântico: vazio resolve o padrão do tipo no POST, enquanto a caixa vazia oculta todas as projeções. A fixture não configura padrão no `TipoEscala`, deixando descoberta a combinação “vazio com padrão não nulo” (`app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição de alterar somente contexto está registrada; já a afirmação de que qualquer solução necessariamente duplicaria regra ou alteraria o POST não está demonstrada pelas linhas citadas (`app/docs/BACKLOG.md:820`; [nao verificado]).
- **D2:** manter os demais links dentro de blocos ocultos corresponde ao desenho implementado; isso não é, por si, link visível indevido. A alegação de aprovação literal depende do anexo, sem o documento original conferido (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`, `:207`; [nao verificado]).
- **D3:** a projeção reutiliza o mesmo `hoje` da agenda. O código configura São Paulo, mas isso não demonstra a igualdade incondicional alegada entre os dois relógios (`app/colaboradores/views.py:280`, `:302`; `app/config/settings/base.py:93`). Inicialização efetiva de `TZ` e igualdade em produção: [nao verificado].
- **D4:** motivo consistente: a fonte calcula a definição do calendário, e essa é a pergunta registrada para a obra; não pretende mostrar a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`; `app/docs/BACKLOG.md:820`).
- **D5:** motivo consistente como descrição de snapshot: um novo GET recalcula agenda e projeção; trocar a caixa apenas alterna blocos já recebidos (`app/colaboradores/views.py:280`, `:294`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`).
- **D6:** “custo constante” só se sustenta como quantidade de queries. Processamento e HTML crescem por calendário; o teste não limita esse crescimento (`app/colaboradores/views.py:299`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:177`). A medição do log e a extrapolação fornecida não foram revalidadas: [nao verificado].

O mapa dos quatro valores é:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `folgas_projecao` | View consulta a fonte, monta e publica o contexto: `app/colaboradores/views.py:304`, `:308`. | Template percorre calendários, meses e datas: `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`. |
| `pode_abrir_calendario` | View consulta `tem_acao(..., 'editar_escalas')`: `app/colaboradores/views.py:309`. | Template condiciona o link: `app/templates/colaboradores/partials/painel_vinculo.html:207`. Destino exige a mesma ação: `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK: `app/templates/colaboradores/partials/painel_vinculo.html:205`. | Handler compara com a caixa e escreve `hidden`: `app/templates/colaboradores/partials/painel_vinculo.html:196`. Testes leem atributo/conteúdo/display: `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `:41`, `:194`. |
| `folga_calendario_id`, formulário | Template produz opções e seleção inicial; sonda altera `.value`: `app/templates/colaboradores/partials/painel_vinculo.html:195`, `:200`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `:203`. | Handler lê a seleção; POST lê o campo e resolve calendário ou padrão: `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:230`, `:236`. |

O `folga_calendario_id` persistente pertence à FK nullable `EscalaColaborador.folga_calendario`, com `SET_NULL` (`app/escala/models.py:890`).

- **Escritores:** cópia do padrão ao vincular; troca explícita usada pela tela individual e pelo lote; criação de vínculos nos ajustes automáticos. A fixture também grava a FK diretamente (`app/colaboradores/services/vinculo.py:161`, `:284`, `:298`, `:461`, `:1092`, `:1113`; `app/colaboradores/views.py:239`; `app/escala/views.py:409`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78`).
- **Leitores:** seleção/ocultação inicial, retorno do POST e trilha (`app/templates/colaboradores/partials/painel_vinculo.html:200`, `:205`; `app/colaboradores/views.py:237`, `:258`; `app/colaboradores/services/vinculo.py:42`, `:459`, `:471`).
- **Outros consumidores:** universo de vínculos, divergência, vigias, materialização e sincronização futura; lista de usuários do calendário pela relação reversa; diagnóstico de vínculos sem plano; comando de lavra (`app/escala/folgas.py:75`, `:99`, `:127`, `:157`, `:600`, `:627`; `app/escala/views.py:300`; `app/api/views_mensageria.py:1223`; `app/escala/management/commands/lavrar_foto_folgas.py:113`, `:170`).

Execução da suíte e resultados de produção: [nao verificado]. [nao verificado]

## 16/11 13:54 · sessao 1062dd8ea6bd · - · - · origem=Ronald (tmux)

D1–D6, com distinção entre código confirmado e justificativas históricas:

- **D1:** o fato citado está correto: vazio no POST resolve `tipo_escala.folga_calendario_padrao`, e passa o resultado ao escritor (`app/colaboradores/views.py:230`, `:236`, `:239`). A tela, por outro lado, associa vazio à opção “padrão do template” e oculta todos os blocos (`app/templates/colaboradores/partials/painel_vinculo.html:198`, `:205`). A fixture não define calendário padrão no tipo; portanto o caso “vazio com padrão não nulo” permanece descoberto (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição “view só contexto” consta do escopo registrado (`app/docs/BACKLOG.md:820`). Ser pré-existente e ser impossível corrigir sem duplicar regra ou violar o aval são justificativas históricas/de desenho, não demonstradas só pelas linhas citadas [nao verificado].

- **D2:** o preload de todos os blocos e a alternância por valor existem (`app/colaboradores/views.py:301`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:204`). Os links permanecem nos blocos ocultos quando há permissão (`app/templates/colaboradores/partials/painel_vinculo.html:205`, `:207`). A aprovação literal desse desenho é relatada no anexo; o documento original da proposta/revisão não foi localizado [nao verificado].

- **D3:** `hoje` vem de `date.today()` e alimenta tanto a agenda anterior quanto a projeção nova (`app/colaboradores/views.py:280`, `:285`, `:302`). O fuso configurado é São Paulo, com `USE_TZ=True` (`app/config/settings/base.py:93`, `:95`). Isso sustenta a alegação de reutilização do relógio existente; a afirmação universal de igualdade com `timezone.localdate()` e a inicialização de `TZ` pelo Django não estão demonstradas pelo código da cópia [nao verificado]. Não há evidência fornecida de divergência em produção [nao verificado].

- **D4:** correto quanto à pergunta implementada: a fonte recebe calendário/ano/mês e calcula a definição, sem ler a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`). A view usa exatamente essa fonte (`app/colaboradores/views.py:304`), coerente com o escopo registrado (`app/docs/BACKLOG.md:820`).

- **D5:** correto como descrição de snapshot: agenda e projeção são montadas durante o GET e entregues no mesmo fragmento (`app/colaboradores/views.py:280`, `:294`, `:304`, `:306`). A alternância apenas escolhe os blocos já entregues (`app/templates/colaboradores/partials/painel_vinculo.html:196`). O painel da página de vínculos pode ser recarregado por novo `fetch` (`app/templates/colaboradores/vincular.html:143`, `:147`).

- **D6:** crescimento linear do trabalho e do HTML é confirmado pelos loops por calendário e por mês (`app/colaboradores/views.py:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`). O teste mede igualdade da quantidade de queries entre duas e sete opções; imprime bytes, sem impor limite de bytes, tempo ou processamento (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`). “~581 bytes” é aritmeticamente coerente com os números do anexo, mas a execução e `green_modulo.log` não foram verificadas [nao verificado]. “140 calendários” não é contagem de produção demonstrada pelo dado fornecido [nao verificado].

Não identifiquei linha citada que contradiga frontalmente o fato central de D1, D4 ou D5; D2 e D3 contêm premissas adicionais sem prova suficiente, e D6 só sustenta custo constante **em queries**, não custo total (`app/colaboradores/views.py:299`, `:304`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:179`).

## 16/11 13:54 · sessao 18019f974ac0 · - · - · origem=Ronald (tmux)

Censo dos quatro campos/contextos:

| Campo | Escritor | Leitor |
|---|---|---|
| `folgas_projecao` | `painel_vinculo` monta `(calendário, [(mês, dias ordenados)])` consultando a fonte e publica no contexto — `app/colaboradores/views.py:304`, `app/colaboradores/views.py:308`. | Template percorre calendários/meses e imprime datas — `app/templates/colaboradores/partials/painel_vinculo.html:204`, `app/templates/colaboradores/partials/painel_vinculo.html:206`. |
| `pode_abrir_calendario` | `painel_vinculo` consulta `tem_acao(request.user, 'editar_escalas')` — `app/colaboradores/views.py:309`. | Template condiciona o link — `app/templates/colaboradores/partials/painel_vinculo.html:207`. O destino exige a mesma ação — `app/escala/views.py:314`, `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK de cada calendário no atributo e determina `hidden` inicial pelo FK salvo — `app/templates/colaboradores/partials/painel_vinculo.html:205`. | `onchange` lê o atributo, compara com o valor da caixa e grava `e.hidden` — `app/templates/colaboradores/partials/painel_vinculo.html:196`. Regex do teste lê PK/hidden/conteúdo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41`; sonda lê atributo e `display` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`. |
| `folga_calendario_id` como formulário | Template produz `select`, marca opção pelo FK salvo e permite vazio — `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/templates/colaboradores/partials/painel_vinculo.html:200`. Usuário/sonda muda `.value` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:203`. | POST lê e resolve PK; vazio resolve `tipo_escala.folga_calendario_padrao` — `app/colaboradores/views.py:230`, `app/colaboradores/views.py:232`, `app/colaboradores/views.py:236`. Sonda seleciona o campo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:190`. |

`folga_calendario_id` persistente corresponde à FK nullable `EscalaColaborador.folga_calendario`, com `on_delete=SET_NULL` — `app/escala/models.py:890`, `app/escala/models.py:891`, `app/escala/models.py:892`.

Escritores persistentes relevantes:

- `_copiar_folga_padrao` preenche somente vínculo sem calendário, atribui a relação e salva — `app/colaboradores/services/vinculo.py:157`, `app/colaboradores/services/vinculo.py:158`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:162`; chamadas após atualizar/criar vínculo — `app/colaboradores/services/vinculo.py:284`, `app/colaboradores/services/vinculo.py:298`.
- `trocar_calendario_de_folga` atribui/salva FK — `app/colaboradores/services/vinculo.py:461`, `app/colaboradores/services/vinculo.py:462`; chamado pela tela individual — `app/colaboradores/views.py:239` — e aplicação em lote — `app/escala/views.py:409`.
- Ajuste automático e desfazimento criam vínculos copiando calendário — `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1092`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1113`.

Leitores persistentes relevantes:

- UI: seleção e bloco inicial — `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`.
- POST: rótulo anterior e JSON de retorno — `app/colaboradores/views.py:237`, `app/colaboradores/views.py:258`.
- Trilha: snapshot e antes/depois da troca — `app/colaboradores/services/vinculo.py:42`, `app/colaboradores/services/vinculo.py:459`, `app/colaboradores/services/vinculo.py:463`, `app/colaboradores/services/vinculo.py:471`.
- Folgas: universo com calendário — `app/escala/folgas.py:75`, `app/escala/folgas.py:79`; divergência — `app/escala/folgas.py:99`; contador de calendário sem datas — `app/escala/folgas.py:126`, `app/escala/folgas.py:129`; materialização — `app/escala/folgas.py:157`, `app/escala/folgas.py:158`; sincronização futura — `app/escala/folgas.py:627`, `app/escala/folgas.py:629`.
- Salvar definição do calendário procura vínculos por essa relação e sincroniza — `app/escala/folgas.py:600`, `app/escala/folgas.py:601`.

A consequência de D1 está no código: mudar a caixa para vazio oculta todos os blocos imediatamente, mas salvar pode persistir o calendário padrão do tipo — `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:239`. A fixture “sem calendário” não configura calendário padrão no tipo, logo não cobre essa combinação — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:58`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:69`.

## 16/11 13:54 · sessao 1d6a3422f25b · - · - · origem=Ronald (tmux)

- **P1/P2 parcialmente atendidos.** O teste executa `change` e mede `display` nas duas cascas, com mutante que remove `onchange`; portanto comprova a alternância do fragmento naquele hospedeiro artificial (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:188`, `:201`, `:255`, `:262`). Não comprova a chegada real: injeta HTML inicial em `#uvc-painel` antes de `</body>` (`:240-250`); a tela real usa `fetch`, `innerHTML`, wrapper `.vinc-painel`, execução dos scripts e `htmx.process` (`app/templates/colaboradores/vincular.html:143-161`). O drawer usa `#drawer-geral-corpo`, abre por transformação e processa HTMX após injeção (`app/templates/core/_drawer_generico.html:3`, `:16-34`).
- **Selo pode ficar verde com integração quebrada.** A sonda só verifica `display` do próprio bloco (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193-195`); não mede ancestrais, posição, área, clipping, abertura nem processamento do fragmento real. O hospedeiro real limita largura e tem `overflow:hidden`/rolagem, ausentes do wrapper artificial (`app/templates/colaboradores/vincular.html:5-11`; teste `:247`). O requisito local exige duas cascas e afirma que o helper é piso, não teto (`CLAUDE.md:202-203`).
- **Cascata sem garantia de CSS carregado.** O helper apenas liga `STATIC_ROOT` e reescreve URLs (`app/core/tests/test_smoke_chromium.py:93-100`); o novo teste descarta os erros retornados (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`). Como `hidden` já funciona pelo navegador e `onchange` é inline, CSS externo ausente pode manter o resultado esperado (`app/templates/colaboradores/partials/painel_vinculo.html:195-205`). Carregamento efetivo em execução: [nao verificado].
- **P3 atendido no ponto relatado.** Exigir HTTP 200 e dois/sete blocos fecha o verde por duas respostas de erro iguais (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174-176`). A métrica continua sendo quantidade de queries: bytes são impressos, sem limite, e CPU/tempo não são medidos (`:177-179`).
- **P4 melhora, mas não fecha o vínculo argumento→retorno→tela.** O teste exige que as duas chamadas corretas estejam presentes no conjunto de chamadas, mas permite chamadas adicionais erradas (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:154-156`). Todas recebem a mesma sentinela, e a comparação agrega datas do bloco, permitindo consultar corretamente sem usar esses retornos e usar retorno de outra chamada (`:150-158`). Não valida quantidade, exclusividade nem correspondência de cada mês/calendário com sua resposta (`:154-158`).
- **Execução dos testes nesta auditoria: [nao verificado].** O próprio caminho de teste cria diretório e arquivos temporários e os apaga; executá-lo contrariaria a restrição somente leitura (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:248-252`; `app/core/tests/test_smoke_chromium.py:94-100`).

## 16/11 13:47 · sessao 25e4b8416f04 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**Parecer: não fecharia esta versão.** O caminho “vazio → calendário padrão” deixa a projeção divergente do calendário salvo; o teste de alternância não executa a interação e o selo novo não cobre as duas cascas. (`app/colaboradores/views.py:235-257`; `app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `CLAUDE.md:203`)

Execução dos testes, smoke, números efetivos de queries e confirmação Git do hash/base: **[nao verificado]**. [nao verificado]

1. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritor → leitores |
   |---|---|
   | `folga_calendarios` | A view produz uma lista ordenada, com `tamanhos` prefetched; a própria view percorre essa lista para projetar e o template percorre para montar opções. (`app/colaboradores/views.py:301-307`; `app/templates/colaboradores/partials/painel_vinculo.html:199-200`) |
   | `folgas_projecao` | A view produz pares calendário/meses, contendo datas ordenadas da fonte; o template consome meses, datas e calendário do link. (`app/colaboradores/views.py:302-308`; `app/templates/colaboradores/partials/painel_vinculo.html:204-207`) |
   | `pode_abrir_calendario` | A view chama `tem_acao('editar_escalas')`; o template condiciona o link. O destino exige a mesma ação por um decorator que chama o mesmo juiz. (`app/colaboradores/views.py:309`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:314-316`; `app/colaboradores/mixins.py:334-346`) |
   | `escala_ativa.folga_calendario_id` | A view carrega a primeira escala `ativa=True`; o template usa o FK para seleção e visibilidade inicial. O POST escreve pela porta `trocar_calendario_de_folga`; a herança inicial também escreve no serviço de vínculo, e operações de cópia preservam o FK. (`app/colaboradores/views.py:268`; `app/templates/colaboradores/partials/painel_vinculo.html:200-205`; `app/colaboradores/services/vinculo.py:151-162`; `app/colaboradores/services/vinculo.py:450-462`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`) |
   | `FolgaCalendario.modo/celulas/tamanhos` | As portas de salvar/criar calendário escrevem esses dados. A fonte lê `modo`, depois `celulas` ou `dias_por_tamanho()`, que percorre `tamanhos.all()`. Editor e listagem também leem a definição. (`app/escala/folgas.py:571-595`; `app/escala/folgas.py:645-661`; `app/escala/folgas.py:52-56`; `app/escala/models.py:1558-1560`; `app/escala/views.py:292-307`; `app/escala/views.py:362-371`) |
   | `hoje` | É variável local, **não contexto enviado ao template**. Já vinha de `date.today()` para a agenda; agora também determina os dois meses da projeção. (`app/colaboradores/views.py:280-288`; `app/colaboradores/views.py:302-318`) |

   Os meses projetados são **meses civis completos**, inclusive dias anteriores a hoje; a fonte retorna `date`, sem hora. Não há recorte por competência 21–20 nesse caminho. (`app/colaboradores/views.py:302-305`; `app/escala/folgas.py:6-15`; `app/escala/folgas.py:18-36`)

   **Não há uniformidade explícita de fuso:** painel e POST usam `date.today()`, enquanto a porta de troca e o salvamento do calendário usam `timezone.localdate()`. Os settings declaram `America/Sao_Paulo` e `USE_TZ=True`; o fuso efetivo do processo e eventual divergência em produção são **[nao verificado]**. (`app/colaboradores/views.py:243`; `app/colaboradores/views.py:280`; `app/colaboradores/services/vinculo.py:465`; `app/escala/folgas.py:599`; `app/config/settings/base.py:93-95`)

   Há duas interpretações do **vazio**: na apresentação significa “nenhuma projeção”; no POST significa “resolver o padrão do tipo”, que pode ser um calendário concreto. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:230-239`)

2. **Derivação fora da fonte**

   Não identifiquei derivação paralela no trecho novo: a view chama `datas_que_o_calendario_manda` e apenas ordena; o template itera e formata. A escolha entre os dois modos continua dentro da fonte. (`app/colaboradores/views.py:300-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-206`; `app/escala/folgas.py:43-56`)

   No GET, cada projeção contém todas as datas retornadas para aquele calendário/mês. Ela representa **a definição do calendário**, sem filtrar vigência do vínculo ou consultar a foto efetiva da pessoa; portanto não certifica os dias materializados ou julgados para ela. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:206`; `app/escala/folgas.py:157-158`; `app/escala/models.py:1069-1096`)

3. **Quando fica velha ou divergente; o que o admin vê**

   | Caminho | Resultado pelo código |
   |---|---|
   | Trocar a caixa sem salvar | Mostra imediatamente a projeção e o link do candidato selecionado; o vínculo só muda no POST. Isso atende à prévia do calendário **selecionado**. (`app/templates/colaboradores/partials/painel_vinculo.html:195-207`; `app/colaboradores/views.py:230-239`) |
   | Salvar calendário explícito | Permanece a projeção calculada no GET; a resposta é somente toast. Não há atualização da definição após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:191-196`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, com padrão não nulo | **Falha:** a caixa continua vazia e todos os blocos ficam ocultos, embora o POST tenha salvo o calendário padrão. Novo GET seleciona esse FK e mostra sua projeção. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:235-239`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, sem padrão | Continua sem calendário e sem projeção visível, coerente com o valor salvo. (`app/colaboradores/views.py:235-239`; `app/templates/colaboradores/partials/painel_vinculo.html:205`) |
   | Sem escala ativa | Exibe “vincule a escala primeiro”, bloco esmaecido e `pointer-events:none`; ainda calcula todas as projeções, inicialmente ocultas. POST retorna 400. Os controles não recebem `disabled`; comportamento por teclado: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:184-211`; `app/colaboradores/views.py:226-229`; `app/colaboradores/views.py:301-305`) |
   | Calendário editado depois | Um painel ainda aberto conserva a definição antiga; alternar a caixa apenas alterna HTML existente. Novo GET recalcula. O editor salva e redireciona à lista; retorno pelo histórico/cache do navegador: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:301-305`; `app/escala/views.py:354-361`) |
   | Virada de mês com painel aberto | Conserva os dois meses do GET; o handler não consulta relógio nem servidor. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/templates/colaboradores/partials/painel_vinculo.html:196`) |

   Na leitura literal de **“sem calendário = sem link”**, também há desvio: com permissão, os links dos calendários oferecidos continuam no HTML, dentro de blocos `hidden`. O teste exige somente ausência de blocos visíveis. (`app/templates/colaboradores/partials/painel_vinculo.html:204-209`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)

4. **Duas cascas e raiz da busca**

   O endpoint entrega um fragmento, sem renderizar uma casca. O caminho direto vem de `vincular.html`, que estende `base.html`: a linha chama `abrirPainelVinculo`, depois `carregarPainel`, que busca **`/colaboradores/vincular/<cid>/painel/` a partir da raiz do host**, injeta o HTML e processa HTMX. (`app/colaboradores/views.py:306`; `app/templates/colaboradores/vincular.html:1`; `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136-161`; `app/config/urls.py:26`; `app/colaboradores/urls.py:12`)

   Existe outro caminho pelo retrato: tipos escala/folga/sem_escala geram a URL do painel e o bloco chama `window.abrirDrawer`. Esse carregador faz `fetch(url)` e está incluído nas duas cascas. Isso demonstra infraestrutura compartilhada; sessão real oferecendo e abrindo o novo painel em `base_app.html`: **[nao verificado]**. (`app/folha/retrato_folha.py:1003-1006`; `app/folha/retrato_folha.py:1066-1075`; `app/templates/folha/_retrato_o_que_fazer.html:73`; `app/templates/core/_drawer_generico.html:11-33`; `app/templates/base.html:399`; `app/templates/base_app.html:154`)

5. **Leis de UI e conduta de front**

   - **L2:** a derivação de datas e a autorização do link usam autoridades existentes. A inconsistência está na resolução do vazio descrita acima. (`app/docs/LEIS-UI.md:23-25`; `app/colaboradores/views.py:304-309`; `app/colaboradores/views.py:235-239`)
   - **L4:** o novo link só é renderizado com `editar_escalas`, mesma autorização exigida pelo destino. (`app/docs/LEIS-UI.md:39-49`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:315`; `app/colaboradores/mixins.py:342`)
   - **L7/L8 e JS por posição:** o trecho acrescentado não traz ajuda explicativa, `title` cru ou diálogo nativo. O handler busca pelo atributo `data-folgas-cal`, dentro do formulário, sem índice ou posição no DOM. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:77-89`; `app/core/selo_js.py:3-7`)
   - **L1/L3/L6/L9/L10/L11:** o trecho novo não acrescenta contador, classificação de estado, filtro, PDF ou ícone; não identifiquei violação nova dessas leis nesse bloco. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:12`; `app/docs/LEIS-UI.md:31`; `app/docs/LEIS-UI.md:68`; `app/docs/LEIS-UI.md:95`; `app/docs/LEIS-UI.md:120`; `app/docs/LEIS-UI.md:174`)
   - **L5:** o botão existente continua sem indicador/trava, mas duração superior a um segundo é **[nao verificado]**; não há base para declarar nova violação por lentidão. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/docs/LEIS-UI.md:56-60`)
   - **Duas cascas:** o selo novo só pede o fragmento; não renderiza ambas nem prova seu grafo, contrariando a conduta explícita. Smoke de clique e eventual execução de verificações JS: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78-83`; `CLAUDE.md:202-203`)

   O `title` cru da agenda e o emoji de “Ver agenda completa” já existem fora das adições apresentadas; não são introduzidos por esta obra. (`app/templates/colaboradores/partials/painel_vinculo.html:226`; `app/templates/colaboradores/partials/painel_vinculo.html:249`)

6. **O teste morde?** [nao verificado]

   - **Fonte: sim, parcialmente.** Exige sentinela não vazia na saída e ausência do dia antigo. Porém o retorno igual para todas as chamadas e a conversão em conjuntos não verificam argumentos, separação por mês, ordem ou duplicações. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41-42`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-91`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:143-150`)
   - **Alternância: não prova funcionamento.** Verifica estado inicial e uma regex do `onchange`; um handler quebrado contendo o texto esperado pode passar. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`)
   - **Sem calendário:** tem proteção contra ausência total de blocos, mas verifica visibilidade, não ausência literal de links. A fixture tem escala ativa e tipo sem padrão; não alcança o caso vazio com padrão concreto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:52-75`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)
   - **Sem ação:** preserva datas não vazias e exige ausência da URL; existe também teste positivo do link. Testa o consumo de um juiz mockado, sem validar navegação de uma conta real sem permissão. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-103`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:131-140`)
   - **Custo:** isoladamente pode passar com duas respostas iguais de erro/redirect, pois não exige status 200 nem projeções nas respostas medidas. Não fixa teto absoluto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Relógio:** a classe está congelada; não depende do dia da execução. O horário escolhido não testa divergência na fronteira de dia/mês entre fusos. RED/GREEN efetivamente executados: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:45`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-108`)

7. **Lista, outros leitores e queries**

   O consumidor do painel apenas itera, portanto não depende de métodos de queryset. O wizard usa o mesmo nome de contexto, mas recebe querysets produzidos por suas próprias views; a mudança local não altera esses produtores. (`app/templates/colaboradores/partials/painel_vinculo.html:199`; `app/templates/escala/_wizard_cal_select.html:4`; `app/escala/views_wizard.py:86`; `app/escala/views_wizard.py:176`; `app/escala/views_wizard.py:197`)

   **Contagem estrutural esperada do carregamento de calendários:** uma consulta de calendários mais uma de `tamanhos` quando há calendários; sem calendários, somente a primeira. O prefetch atende `tamanhos.all()` usado pela fonte. Frente ao queryset anterior das opções, acrescenta uma consulta quando a lista não está vazia. (`app/colaboradores/views.py:299-305`; `app/escala/models.py:1558-1560`; `app/escala/folgas.py:56`)

   A quantidade dessas consultas é constante em relação ao número de calendários, mas há **duas chamadas à fonte por calendário**, além de memória e HTML proporcionais à quantidade oferecida. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-209`)

   **Número total efetivo por request: [nao verificado].** O endpoint também executa a agenda existente, com consultas dependentes dos caminhos de células/fotos; o teste fornecido apenas compara dois totais e imprime números cuja saída não foi fornecida. (`app/colaboradores/views.py:283-294`; `app/escala/models.py:1092-1096`; `app/escala/models.py:1122-1123`; `app/escala/models.py:1286-1288`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:165-167`)

8. **Classes que já reprovaram obras**

   - **Segundo juiz/derivação paralela:** não aparece no trecho novo; datas vêm da fonte declarada e a permissão vem do juiz existente. (`app/colaboradores/views.py:300-309`; `app/escala/folgas.py:43-56`; `app/colaboradores/mixins.py:322-342`)
   - **Segundo escritor fora da porta:** não aparece nas adições; elas consultam e montam contexto, e o JS altera somente visibilidade. (`app/colaboradores/views.py:300-309`; `app/templates/colaboradores/partials/painel_vinculo.html:196`)
   - **Relógio fora da borda:** não foi acrescentado; a view reutiliza seu `hoje`. Permanece a diferença de convenção de fuso apontada no item 1. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/colaboradores/services/vinculo.py:465`)
   - **Selo sem mordida suficiente:** presente na promessa de alternância e na verificação isolada de custo; comprovação histórica de RED: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Campo com dois significados:** presente no vazio da caixa — ausência de projeção na UI versus resolução de calendário padrão no POST — e agravado pela manutenção do HTML após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/views.py:235-257`)

## 16/11 13:45 · sessao 444ae62b7f9f · - · - · origem=Ronald (tmux)

1. **Produtores/leitores e significado**

- `folga_calendarios`: escrito por `painel_vinculo` como lista ordenada de todos os `FolgaCalendario`, com prefetch de `tamanhos`; lido na própria view para produzir projeções e no template para opções `pk/nome` (`app/colaboradores/views.py:301`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- `folgas_projecao`: escrito somente pela view como `(calendário, [(primeiro dia do mês, datas ordenadas da fonte)])`; lido no template para datas e link (`app/colaboradores/views.py:302`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- `pode_abrir_calendario`: view chama `tem_acao(user, 'editar_escalas')`; template consome o booleano; destino usa `@acao_required('editar_escalas')`, cujo corpo chama o mesmo `tem_acao`. Não há segundo juiz de permissão neste trecho (`app/colaboradores/views.py:309`, `app/templates/colaboradores/partials/painel_vinculo.html:207`, `app/escala/views.py:315`, `app/colaboradores/mixins.py:342`).
- `escala_ativa.folga_calendario_id`: carregado da primeira escala `ativa=True`, usado para marcar opção e mostrar bloco inicial. Escritor do POST é `trocar_calendario_de_folga`; a herança inicial `_copiar_folga_padrao` também escreve dentro do serviço de vínculo. O novo JavaScript só troca o bloco conforme o valor escolhido, sem escrever banco (`app/colaboradores/views.py:268`, `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:461`).
- `FolgaCalendario.modo/celulas/tamanhos`: campos definidos como modo `dias_mes` ou `semana_dia`, pares de ocorrência semanal e dias por tamanho de mês. Editor chama `salvar_calendario_de_folga`, que grava modo/células e apaga ou atualiza tamanhos; wizard chama a porta `criar_calendario_de_folga`. A fonte lê `modo/celulas`, ou `dias_por_tamanho()` que lê `tamanhos.all()` (`app/escala/models.py:1539`, `app/escala/models.py:1544`, `app/escala/models.py:1558`, `app/escala/models.py:1566`, `app/escala/views.py:354`, `app/escala/folgas.py:584`, `app/escala/folgas.py:593`, `app/escala/folgas.py:645`, `app/escala/folgas.py:654`, `app/escala/folgas.py:54`).
- `hoje` **não é contexto enviado ao template**: variável local da view, escrita por `_dt.date.today()`, usada na agenda e agora na seleção dos dois meses. As datas são civis, sem hora; a fonte produz `date(ano, mes, dia)`. A projeção cobre meses civis completos, incluindo dias anteriores a hoje, exatamente conforme item 1, e não a competência 21–20 (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:285`, `app/colaboradores/views.py:302`, `app/colaboradores/views.py:306`, `app/escala/folgas.py:15`, `app/escala/folgas.py:36`). A competência tem outro contrato explícito (`app/ponto/janelas.py:10`).
- **Fuso não é uniformizado no código**: view e POST usam `date.today()`, mas troca pelo serviço e salvamento do calendário usam `timezone.localdate()`; settings declaram `America/Sao_Paulo` e `USE_TZ=True`. Se o fuso do processo diferir, a virada de mês pode divergir. Configuração real do processo e divergência em produção: **[nao verificado]** (`app/colaboradores/views.py:243`, `app/colaboradores/views.py:280`, `app/colaboradores/services/vinculo.py:465`, `app/escala/folgas.py:599`, `app/config/settings/base.py:93`).
- **O vazio tem duas leituras de interface/ação**: a seleção vazia oculta todas as projeções; no POST, vazio resolve para `ec.tipo_escala.folga_calendario_padrao`, que pode ser calendário concreto. Após sucesso HTMX só há toast, portanto a interface fica vazia embora tenha sido salvo um calendário padrão (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/colaboradores/views.py:230`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:251`).

2. **Fonte única**

- Não há derivação paralela de folgas no trecho novo: a view chama `datas_que_o_calendario_manda` duas vezes por calendário e só ordena; template só itera e formata. Os dois modos são interpretados exclusivamente pela fonte (`app/colaboradores/views.py:300`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:52`).
- A tela apresenta todas as datas devolvidas para cada mês; não filtra hoje, vigência, foto materializada ou folgas manuais. Isso é coerente com “o calendário manda”, mas não prova os dias efetivamente materializados ou julgados para a pessoa (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:158`, `app/escala/folgas.py:184`, `app/escala/folgas.py:187`).
- O relógio continua lido na borda view, e a fonte recebe ano/mês explicitamente; o diff não acrescenta relógio dentro do resolvedor (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:304`, `app/escala/folgas.py:43`).

7. **Outros leitores e custo**

- Outro leitor com o mesmo nome de contexto é `_wizard_cal_select.html`, mas ele recebe seu próprio queryset de `views_wizard`; a mudança local para lista no painel não o afeta. No painel, o leitor só itera, sem métodos de queryset (`app/templates/escala/_wizard_cal_select.html:4`, `app/escala/views_wizard.py:86`, `app/escala/views_wizard.py:176`, `app/escala/views_wizard.py:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- Custo estrutural do trecho novo: **1 consulta de calendários + 1 prefetch de tamanhos quando há calendários**; sem calendários, só a primeira. Antes, a lista de opções exigia uma consulta. A fonte lê `self.tamanhos.all()` de objetos prefetched, sem `.filter()` que descartaria o cache (`app/colaboradores/views.py:301`, `app/escala/models.py:1558`, `app/escala/folgas.py:56`).
- Constante em número de consultas de calendário para `K>0`; CPU/memória/HTML crescem com `K`, pois há `2K` chamadas à fonte e um bloco por calendário (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- **Número total medido por request: [nao verificado]**. Não confundir a constância relativa a `K` com custo total fixo: a agenda existente chama `eh_dia_trabalho` até 14 vezes e `marcos_do_dia` nos dias de trabalho, cujos caminhos consultam células e, no fallback, fotos (`app/colaboradores/views.py:287`, `app/colaboradores/views.py:289`, `app/colaboradores/views.py:292`, `app/escala/models.py:1092`, `app/escala/models.py:1122`, `app/escala/models.py:1286`).

## 16/11 13:45 · sessao 8e6f7eb5845c · - · - · origem=Ronald (tmux)

3. Caminhos de atualização:

- **Trocar a caixa sem salvar:** o `onchange` mostra imediatamente o bloco do valor escolhido e esconde os demais. Portanto mostra a prévia do candidato, embora o vínculo persistido continue sendo o anterior até o POST; isso corresponde ao requisito “SELECIONADO”. (`app/templates/colaboradores/partials/painel_vinculo.html:195-207`; `app/colaboradores/views.py:230-239`)
- **Salvar calendário explícito:** o POST chama a porta de escrita; a resposta HTMX só dispara toast, enquanto o formulário usa `hx-swap="none"`. A projeção selecionada permanece aquela calculada no GET. Sem alteração concorrente de definição ou virada de mês, ela continua representando o calendário escolhido; esses eventos concretos em produção são [nao verificado]. (`app/colaboradores/views.py:238-256`; `app/templates/colaboradores/partials/painel_vinculo.html:191-207`)
- **Salvar vazio com padrão não nulo:** há divergência demonstrável pelo código. Vazio oculta todos os blocos, mas o POST transforma vazio em `ec.tipo_escala.folga_calendario_padrao` e grava esse calendário. Como a resposta não repõe o HTML, o admin continua vendo a opção vazia, sem projeção/link, embora o vínculo agora tenha calendário. Novo GET selecionará esse padrão e mostrará seu bloco. (`app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/views.py:235-239`; `app/colaboradores/views.py:251-256`; `app/templates/colaboradores/partials/painel_vinculo.html:200`)
- **Salvar vazio sem padrão:** fica sem calendário e sem bloco visível; nesse caso a seleção exibida corresponde ao valor persistido. (`app/colaboradores/views.py:235-239`; `app/templates/colaboradores/partials/painel_vinculo.html:196-205`)
- **Sem escala ativa:** o GET ainda calcula projeções de todos os calendários, mas nenhum bloco é selecionado; o bloco do formulário aparece esmaecido, com “vincule a escala primeiro” e `pointer-events:none`. Não há `disabled` nos controles; bloqueio efetivo por teclado é [nao verificado]. Um POST nessa condição retorna 400 “sem vinculo de escala ativo”. (`app/colaboradores/views.py:268`; `app/colaboradores/views.py:301-305`; `app/templates/colaboradores/partials/painel_vinculo.html:184-211`; `app/colaboradores/views.py:226-229`)
- **Calendário editado depois do GET:** os dias são uma fotografia no HTML, calculada naquele GET; alternar a caixa só alterna blocos existentes, sem consultar a fonte novamente. O editor grava pela porta e redireciona à lista; um novo GET do painel recalcula, enquanto um painel ainda aberto conserva o HTML anterior. Comportamento de voltar pelo histórico/cache do navegador é [nao verificado]. (`app/colaboradores/views.py:301-306`; `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/escala/views.py:354-361`; `app/escala/folgas.py:571-601`)
- **Virada de mês com painel aberto:** os dois meses ficam aqueles calculados no GET; o `onchange` não avança meses nem recalcula datas. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/templates/colaboradores/partials/painel_vinculo.html:196`)

4. Cascas e carregadores:

- O painel é **fragmento AJAX**, sem `extends`; a view devolve diretamente esse partial. Não há render de uma casca no endpoint. (`app/templates/colaboradores/partials/painel_vinculo.html:1`; `app/colaboradores/views.py:306`)
- O caminho direto confirmado é a página `vincular.html`, que estende `base.html`. A linha chama `abrirPainelVinculo`, que chama `carregarPainel`; este faz `fetch('/colaboradores/vincular/' + cid + '/painel/')`, injeta o HTML e executa `htmx.process(p)`. A URL parte da raiz do host, não do caminho atual. (`app/templates/colaboradores/vincular.html:1`; `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136-161`)
- A montagem de rotas confirma `/colaboradores/` + `vincular/<cid>/painel/`. (`app/config/urls.py:26`; `app/colaboradores/urls.py:12`)
- Há também porta dinâmica: `PORTA_DO_TIPO` aponta escala/folga/sem_escala para `colaboradores:painel_vinculo`; `portas_da_linha` resolve a URL; o bloco “o que fazer” chama `window.abrirDrawer` com esse endereço. Esse bloco é incluído condicionalmente pelo painel do colaborador. (`app/folha/retrato_folha.py:1003-1006`; `app/folha/retrato_folha.py:1066-1075`; `app/templates/folha/_retrato_o_que_fazer.html:66-74`; `app/templates/colaboradores/partials/painel_colaborador.html:26`)
- O carregador compartilhado `abrirDrawer` faz `fetch(url)`, injeta por `innerHTML` e processa HTMX. Ele existe nas duas cascas por include de `core/_drawer_generico.html`. Isso prova disponibilidade da infraestrutura nas duas raízes; não prova que uma sessão real de `base_app.html` ofereça a porta ou consiga passar pelas permissões. (`app/templates/core/_drawer_generico.html:11-33`; `app/templates/base.html:399`; `app/templates/base_app.html:154`; `app/colaboradores/views.py:264-265`)
- O teste antigo de duas cascas verifica grafo/includes e presença da função, explicitamente sem renderizar uma página inteira. Render/click real do painel novo nas duas cascas: [nao verificado]. (`app/colaboradores/tests/test_retrato_no_drawer.py:259-277`)

## 16/11 13:45 · sessao cb5f0c45d200 · - · - · origem=Ronald (tmux)

5. No trecho novo, não há diálogo nativo, `title`, ícone/emoji ou ajuda explicativa; o JavaScript busca `data-folgas-cal` dentro de `this.form`, sem posição no DOM. Portanto não identifiquei violação nova de L7/L8/L10/L11 ou JS por posição nesse bloco. O `title` cru da agenda já está fora do diff. (app/templates/colaboradores/partials/painel_vinculo.html:195-209; app/templates/colaboradores/partials/painel_vinculo.html:226; app/docs/LEIS-UI.md:77-89; app/core/selo_js.py:3-7)

O link respeita L4 e usa o mesmo juiz de permissão do destino: contexto chama `tem_acao('editar_escalas')`; `acao_required` também chama `tem_acao`, e o destino exige essa ação. (app/colaboradores/views.py:309; app/colaboradores/mixins.py:322-342; app/escala/views.py:314-316; app/docs/LEIS-UI.md:39-49)

O selo novo não renderiza ambas as cascas nem prova o grafo delas: faz GET apenas do partial. Isso não satisfaz a conduta explícita de selo de front nas duas raízes. Smoke de clique do Ronald e RED/GREEN executados: [nao verificado]. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78-83; CLAUDE.md:202-203; app/docs/CONDUTAS.md:50-68)

6. O teste tem mordidas úteis, mas cobertura incompleta: [nao verificado]

- **Fonte:** a sentinela não vazia precisa aparecer na saída e o dia antigo precisa desaparecer; isso detecta derivação própria ignorando o retorno da fonte. Entretanto não confere argumentos/quantidade das chamadas, separação por mês ou duplicação: `_dias` converte em conjunto e `_fonte` une os meses. Um código que trocasse os rótulos dos meses ou repetisse datas poderia continuar passando. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:32-42; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-91; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:143-150)
- **Alternância:** confere o estado inicial e a presença de um `onchange` contendo `data-folgas-cal`; nunca executa o handler. Um handler com esse texto e comportamento quebrado pode passar. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120)
- **Sem calendário:** impede vacuidade pela ausência completa de blocos, pois exige blocos oferecidos e nenhum visível; não exige ausência literal de links no HTML, apenas blocos ocultos. A fixture possui escala ativa e tipo sem calendário padrão; não cobre vínculo sem ativa nem opção vazia resolvendo um padrão não nulo. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:52-75; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129)
- **Sem ação:** preserva dias não vazios e exige ausência da URL; existe teste positivo da URL, então o conjunto não se sustenta só em ausência. É teste do contexto com juiz mockado, não teste de uma conta real sem permissão navegando ao destino. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-103; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:131-140)
- **Custo:** compara número de queries entre 2 e 7 calendários, mas não verifica status das duas respostas, presença de projeções ou aumento dos blocos. Pode passar em duas respostas iguais de erro/redirect sem projeção. Também não fixa teto absoluto nem prova tempo/memória constantes. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167)
- **Relógio:** a classe inteira congela 10/12/2026 às 15h; não há relógio solto nesses testes. A constante `HOJE` é declarada, mas a expectativa usa `MESES` e literais. Resultado efetivo da execução: [nao verificado]. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:30-31; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:45; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-108)

8. Neste diff não aparece segundo juiz de datas, segundo escritor persistente ou nova leitura de relógio: a view reutiliza `hoje` já lido na borda e chama a fonte única, ordenando o retorno; o template apenas formata datas. Há lacuna real na força do selo de front/custo descrita acima; evidência histórica de RED e smoke é [nao verificado]. (app/colaboradores/views.py:280; app/colaboradores/views.py:300-309; app/templates/colaboradores/partials/painel_vinculo.html:204-207; app/escala/folgas.py:43-56; CLAUDE.md:193-194; CLAUDE.md:202-203)

## 16/11 13:43 · sessao 8c89640a4b0e · BUG-PAR-O-PAR · construtor · origem=Code

**Entrega incompleta: faltam os arrays U60/U103 do anexo `u163.py`.** O JSON não foi inventado; seu teste termina com `FileNotFoundError`. [RELATO.md:5](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:5) [nao verificado]

A cura escrita limita DXG aos PKs congelados, consulta K6 antes das escritas e usa a célula dona na guarda. [materializacao.py:703](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:703), [validacao.py:289](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:289), [materializacao.py:402](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:402)

Arquivos tocados, relativos a `app/`:

```text
chamados/services/universo_bug_par.py
chamados/services/materializacao.py
chamados/services/validacao.py
chamados/services/fio.py
chamados/management/commands/bug_par_103.py
chamados/templatetags/chamados_tags.py
templates/chamados/partials/_pergunta_card.html
templates/chamados/partials/modal_fio.html
chamados/tests/test_universo_bug_par.py
chamados/tests/test_bug_par_volta.py
chamados/tests/test_bug_par_103.py
chamados/tests/test_bug_par_sem_banco.py
chamados/tests/test_bug_par_projetado.py
chamados/tests/test_s139.py
chamados/tests/bug_par_mutantes.py
core/tests/test_medida_turno_do_par.py
docs/JUIZES.md
docs/RELATO.md
```

A entrega detalhada está registrada com referências:

- **Cada RED, `arquivo::teste`, afirmação e mutante:** [RELATO.md:21](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:21). [nao verificado]
- **Comando de cada mutante:** [RELATO.md:46](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:46).
- **Quem escreve e quem lê cada campo/função:** [RELATO.md:64](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:64).
- **Lookup reconciliado:** [JUIZES.md:38](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/JUIZES.md:38).

Medição local, sem certificação: **8 testes OK, 15 mutantes mortos por asserção e 35 testes da catraca OK**. Integração Django, vizinhos O261/O317, dez mutantes de integração e reprodução de 84/19+75 permanecem **[nao verificado]**. [RELATO.md:48](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:48) [nao verificado]

Comandos para a sessão, após fornecer o JSON exato:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_universo_bug_par chamados.tests.test_bug_par_sem_banco chamados.tests.test_bug_par_volta chamados.tests.test_bug_par_103 chamados.tests.test_bug_par_projetado chamados.tests.test_s139 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_o261_vespera_nao_absolve chamados.tests.test_o317_passivo_pela_porta chamados.tests.test_o261_paridade_respeita_a_lampada core.tests.test_medida_turno_do_par chamados.tests.test_e2_tarefa_do_dia chamados.tests.test_e3_portas_tarefa --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_mutantes --django --mutante todos
```

LEI-AKITA: origem=universo congelado pendente do anexo + portão da materialização, testemunha=estado_aplicacao_pergunta e competencia_entregue, RED=blocos (a)-(c) + fora163 + 5644, quem-mais-le=docs/RELATO.md:64, juizes novos=0, padrao=Single Writer Principle.

## 16/11 13:25 · sessao 77f8120193e8 · UI-VINCULO-MOSTRA-O-CALENDARIO · leitura · origem=Code

**(a) As duas leituras existem no código:** a foto deve chegar à célula pela geração/regeneração; grade e precedência também leem `FolgaDia` por cima do previsto, sem alterar `CelulaDia.trabalha`. Já `EC.eh_dia_trabalho` devolve o valor da célula existente. Portanto, gravar a foto não garante, por si só, que todos esses leitores responderão igual. (`app/escala/folgas.py:215-218`; `app/escala/utils.py:1147-1153`; `app/ponto/precedencia.py:356-387`; `app/escala/models.py:1087-1096`.)

**Quem escreve `FolgaDia`.** O módulo autorizado é `app/escala/folgas.py`. (`app/core/portas.py:470-473`.)

| Porta | Escrita e entradas |
|---|---|
| `gerar_foto_folgas` | Apaga a foto de calendário do mês, preserva manuais, recria as datas e chama a relavratura na mesma transação. Entradas: vínculo individual, aplicação ao tipo, herança do padrão e comando `lavrar_foto_folgas --apply`. (`app/escala/folgas.py:150-218`; `app/colaboradores/views.py:239-246`; `app/escala/views.py:408-412`; `app/colaboradores/services/vinculo.py:151-166`; `app/escala/management/commands/lavrar_foto_folgas.py:159-170`.) |
| `sincronizar_foto_futura_do_calendario` | Usa o mesmo escritor com recorte de datas futuras e depois regenera futuras. É chamada ao salvar o calendário ou mudar o calendário do vínculo. (`app/escala/folgas.py:597-642`; `app/colaboradores/services/vinculo.py:459-465`.) |
| `lancar_folga` | `get_or_create` manual seguido de relavratura. Entradas: toggle do Plano e declaração pela supervisão. (`app/escala/folgas.py:329-339`; `app/escala/views_plano_folgas.py:124-132`; `app/ponto/services/declarar_dia.py:179-193`.) |
| `alterar_folga` | `_foto_do_dia` apaga, cria ou altera `tipo`; depois relavra. Entrada: toggle do Plano. (`app/escala/folgas.py:343-382`; `app/escala/views_plano_folgas.py:134-137`.) |
| `aplicar_folgas_em_lote` | Usa `_foto_do_dia`, recusa competência anterior à corrente e relavra o intervalo gravado. Entrada: Plano em lote. (`app/escala/folgas.py:401-421`; `app/escala/views_plano_folgas.py:163-185`.) |
| `reparentar_folga` | Muda o vínculo da folga manual e chama relavratura; calendário é recusado. Na competência passada usa `so_rejulgar`, preservando o DNA. Entradas: signal de criação/vigência e comando de reparentamento. (`app/escala/folgas.py:484-525`; `app/escala/signals.py:185-207`; `app/escala/management/commands/reparentar_folgas.py:22-34`.) |

Há ainda efeitos referenciais declarados: excluir o vínculo apaga suas folgas por `CASCADE`; excluir o calendário deixa `calendario_origem` nulo por `SET_NULL`. (`app/escala/models.py:1585-1591`.)

**Quem escreve `CelulaDia.trabalha`.**

- **Geração:** `gerar_celulas_periodo` calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria a célula com esse valor e DNA. A data presente na foto do próprio vínculo resulta em `False`. Célula existente editada é preservada; passado existente só pode receber DNA ausente; hoje/futuro existente é pulado nessa função. (`app/ponto/portas/celula.py:911-935`; `app/escala/models.py:1280-1288`; `app/escala/models.py:1309-1317`.)
- **Command `gerar_celulas`:** antes da geração, possui suplemento que regenera células existentes de **hoje**. Isso não constitui uma atualização geral das futuras existentes. (`app/ponto/management/commands/gerar_celulas.py:303-325`; `app/ponto/management/commands/gerar_celulas.py:345-350`.)
- **Regeneração:** recalcula pela foto/cadastro; cria ausentes e reescreve existentes divergentes somente com `origem='gerada'`. Guarda DNA anterior, instante e incrementa `regeneracoes`. Também pode desligar células fora da vigência. (`app/ponto/portas/celula.py:229-311`.)
- **Edição humana:** `editar_dia` escreve `trabalha`, DNA, geradora, `origem='editada'`, autor e instante; não incrementa os campos de regeneração. Troca de plantão delega a essa edição. (`app/ponto/portas/celula.py:575-580`; `app/ponto/portas/celula.py:637-647`; `app/ponto/portas/celula.py:670-715`.)
- **Reversão:** `reverter_regeneracao_dia` restaura `trabalha` pelo DNA anterior e incrementa os carimbos de regeneração. (`app/ponto/portas/celula.py:817-837`.)

**Quem lê cada fonte.**

| Consumidor | Fonte efetivamente lida |
|---|---|
| `EC.eh_dia_trabalho` | Existindo célula, retorna `cel.trabalha`; somente sem célula chama o cálculo que consulta a foto. (`app/escala/models.py:1087-1096`.) |
| `EC.marcos_do_dia` | Lê `dna.marcos`; sem marcos utilizáveis, retorna os marcos do template. Não aplica a foto nessa leitura. (`app/escala/models.py:1117-1133`.) |
| Grade/espelho | O overlay consulta `FolgaDia` pelo juiz, põe `tipo_dia='folga'`, limpa as colunas e órfãs **do dicionário da grade**. O espelho obtém seu flag `folga` desse `tipo_dia`. (`app/escala/utils.py:1140-1153`; `app/escala/utils.py:1844`; `app/ponto/services/espelho.py:673-679`; `app/ponto/services/espelho.py:764-771`.) |
| Precedência | Usa `trabalha` como previsto, mas inclui folga lançada válida mesmo com previsto `True`; folga vence trabalho na ordenação. Calendário vale apenas para o vínculo do dia; manual vale para a pessoa. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:235-239`; `app/ponto/precedencia.py:347-397`.) |
| Motor | No caminho com vínculo não intermitente, classifica folga trabalhada pela resposta `False` de `EC.eh_dia_trabalho`; os marcos vêm de `EC.marcos_do_dia`, com fallback ao template. (`app/ponto/motor_calculo_v2.py:2445-2469`; `app/ponto/motor_calculo_v2.py:2290-2291`; `app/ponto/motor_calculo_v2.py:1532-1540`.) |
| Folha | O fechamento monta datas previstas por `EC.eh_dia_trabalho`, alimenta o motor com células e grava os resultados, incluindo FT. O TXT lê os eventos do fechamento. (`app/ponto/services/fechamento.py:724-755`; `app/ponto/services/fechamento.py:918-924`; `app/ponto/services/fechamento.py:965-966`; `app/folha/export.py:439-446`; `app/folha/export.py:725-739`.) |

O dinheiro mostrado no espelho segue outro percurso que seu flag visual de folga: pago/rubricas do dia leem `DiaPago`; totais da competência passam por `folha_manda` e `fechamento_lido`. (`app/ponto/services/espelho.py:805-813`; `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/cartao_pela_celula.py:69-76`; `app/ponto/services/fechamento.py:1759-1761`.)

**Caso das cinco folgas de dezembro.** Para célula já existente, o percurso previsto é:

`gerar_foto_folgas → relavrar_celulas_da_foto → regenerar_futuras_do_cadastro → regenerar_celulas_vinculo → trabalha=False`.

As chamadas estão em `app/escala/folgas.py:215-218`, `app/escala/folgas.py:297-299` e `app/ponto/services/cadastro_futuro.py:43-45`; o cálculo da foto e a escrita estão em `app/escala/models.py:1309-1317` e `app/ponto/portas/celula.py:289-311`.

**O limite concreto encontrado:** a foto solicitada é mensal, mas a relavratura futura seleciona somente células geradas já existentes dentro de `_janela_default(hoje)`, a partir de amanhã, respeitando o vínculo do dia. Essa janela é a união das competências **correntes** das empresas ativas. Não é o mês inteiro pedido a `gerar_foto_folgas`. (`app/escala/folgas.py:180-197`; `app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/management/commands/gerar_celulas.py:290-300`.)

Logo, datas fora dessa janela não chegam à regeneração por esse percurso. **Que esse filtro tenha excluído precisamente as cinco datas relatadas: `[nao verificado]`.** Data do ato, cortes, origem/geradora das células e bloqueios do caso: `[nao verificado]`. `regeneracoes=0` também não prova ausência de chamada: o incremento ocorre na reescrita divergente, após seleção e guardas. (`app/ponto/portas/celula.py:284-311`.)

**(b) Sim: `gerar_foto_folgas` no mês corrente grava/apaga `FolgaDia` anterior a hoje.** A seleção é pelo mês inteiro; `delete` e `bulk_create` não têm corte por hoje. As manuais são preservadas. (`app/escala/folgas.py:180-197`.)

**A célula passada também pode ser tocada:** a função chama relavratura desde o primeiro dia do mês; o ramo passado encaminha `desde..hoje` à regeneração formal. Essa é precisamente a exceção à irretroatividade prevista na seção 4, sujeita às guardas de vigência, lavra e origem gerada. (`app/escala/folgas.py:212-218`; `app/escala/folgas.py:290-296`; `CLAUDE.md:149-156`; `app/ponto/portas/celula.py:147-162`; `app/ponto/portas/celula.py:284-311`.)

A sincronização **futura** preserva passado, mas as views de vincular/aplicar calendário também chamam depois a geração mensal sem esse recorte. Portanto, a proteção da primeira chamada não protege a segunda. (`app/escala/folgas.py:621-639`; `app/colaboradores/views.py:243-246`; `app/escala/views.py:411-412`.)

**DONO: NAO SEI** — o juiz exige insumos além dos três valores relatados; esses insumos do caso estão `[nao verificado]`. Decisão: `app/ponto/management/commands/e6_oraculo.py:98-110`.

## 16/11 13:24 · sessao 0e5369e6a2cb · UI-VINCULO-MOSTRA-O-CALENDARIO · sonda · origem=Code

# Calendarios de folga em uso com grade vazia (modo dias_mes)

UNIVERSO calendarios dias_mes em uso 72 | com grade vazia 68 | vinculos afetados 77. [nao verificado]

| Calendario (pk) | Vinculos ativos | Grades que faltam | Dias que ja tem (por grade) | Quem usa |
|---|---:|---|---|---|
| 69 | 5 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col390/ec929, col392/ec923, col468/ec402, col575/ec925, col825/ec926 [nao verificado] |
| 72 | 2 | fevereiro | 31: [1, 8, 15, 22, 25, 29]; 30: [5, 12, 15, 19, 26] | col309/ec1355, col491/ec1271 [nao verificado] |
| 78 | 2 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col921/ec1141, col1026/ec1581 [nao verificado] |
| 87 | 2 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col235/ec202, col600/ec944 [nao verificado] |
| 122 | 2 | meses de 30; fevereiro | 31: [1, 2, 5, 6, 7, 8, 9, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 26, 27, 28, 29, 30] | col346/ec1383, col610/ec1420 [nao verificado] |
| 138 | 2 | meses de 31; fevereiro | 30: [1, 8, 15, 22, 29] | col476/ec1451, col864/ec1494 [nao verificado] |
| 39 | 1 | fevereiro | 31: [5, 6, 7, 12, 13, 19, 20, 26, 27]; 30: [5, 6, 7, 12, 13, 19, 20, 26, 27] | col216/ec184 [nao verificado] |
| 50 | 1 | meses de 31; fevereiro | 30: [30] | col128/ec910 [nao verificado] |
| 53 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col384/ec1347 [nao verificado] |
| 54 | 1 | meses de 30; fevereiro | 31: [4, 9, 18, 23] | col310/ec1167 [nao verificado] |
| 55 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col204/ec1045 [nao verificado] |
| 56 | 1 | meses de 30; fevereiro | 31: [4, 13, 18, 27] | col962/ec1303 [nao verificado] |
| 57 | 1 | meses de 30; fevereiro | 31: [6, 11, 20, 25] | col577/ec491 [nao verificado] |
| 59 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col349/ec307 [nao verificado] |
| 60 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col587/ec500 [nao verificado] |
| 61 | 1 | meses de 30; fevereiro | 31: [4, 12, 18, 26] | col205/ec1044 [nao verificado] |
| 62 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col214/ec182 [nao verificado] |
| 64 | 1 | meses de 30; fevereiro | 31: [8, 11, 15, 22, 29] | col919/ec1136 [nao verificado] |
| 66 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 22, 23, 30] | col243/ec953 [nao verificado] |
| 68 | 1 | fevereiro | 31: [6, 13, 18, 20, 27]; 30: [3, 10, 17, 22, 24] | col278/ec1350 [nao verificado] |
| 70 | 1 | fevereiro | 31: [6, 11, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col203/ec1409 [nao verificado] |
| 71 | 1 | meses de 31; fevereiro | 30: [4, 11, 15, 18, 25] | col617/ec1354 [nao verificado] |
| 73 | 1 | fevereiro | 31: [2, 9, 15, 16, 23, 30]; 30: [6, 13, 15, 20, 27] | col263/ec1526 [nao verificado] |
| 74 | 1 | fevereiro | 31: [3, 4, 10, 17, 24, 31]; 30: [6, 13, 15, 20, 27] | col922/ec1142 [nao verificado] |
| 76 | 1 | fevereiro | 31: [6, 8, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col968/ec1358 [nao verificado] |
| 77 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 8, 9, 16, 23, 30] | col879/ec1359 [nao verificado] |
| 79 | 1 | fevereiro | 31: [4, 5, 12, 19, 26]; 30: [1, 2, 9, 16, 23, 30] | col327/ec285 [nao verificado] |
| 81 | 1 | fevereiro | 31: [2, 9, 16, 18, 23, 30]; 30: [6, 13, 15, 20, 27] | col522/ec446 [nao verificado] |
| 82 | 1 | meses de 30; fevereiro | 31: [3, 10, 17, 24, 25, 31] | col292/ec1267 [nao verificado] |
| 83 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 9, 15, 16, 23, 30] | col917/ec1134 [nao verificado] |
| 84 | 1 | fevereiro | 31: [6, 13, 20, 25, 27]; 30: [3, 10, 17, 22, 24] | col301/ec265 [nao verificado] |
| 85 | 1 | fevereiro | 31: [4, 7, 14, 21, 28]; 30: [1, 4, 11, 18, 25] | col849/ec1001 [nao verificado] |
| 86 | 1 | fevereiro | 31: [1, 4, 8, 15, 22, 29]; 30: [1, 5, 12, 19, 26] | col382/ec1269 [nao verificado] |
| 89 | 1 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col814/ec979 [nao verificado] |
| 90 | 1 | meses de 30; fevereiro | 31: [4, 7, 14, 20, 26] | col89/ec1039 [nao verificado] |
| 92 | 1 | meses de 30; fevereiro | 31: [2, 6, 12, 18, 24, 31] | col638/ec1306 [nao verificado] |
| 94 | 1 | meses de 30; fevereiro | 31: [5, 12, 18, 19, 26] | col81/ec812 [nao verificado] |
| 95 | 1 | meses de 30; fevereiro | 31: [1, 6, 11, 15, 21, 25, 28] | col900/ec1341 [nao verificado] |
| 96 | 1 | meses de 31; fevereiro | 30: [4, 8, 14, 20, 26] | col107/ec1203 [nao verificado] |
| 98 | 1 | meses de 31; fevereiro | 30: [5, 11, 18, 24, 29] | col114/ec1537 [nao verificado] |
| 101 | 1 | meses de 30; fevereiro | 31: [3, 10, 18, 27] | col829/ec947 [nao verificado] |
| 106 | 1 | meses de 30; fevereiro | 31: [1, 8, 16, 22, 29] | col749/ec1374 [nao verificado] |
| 107 | 1 | meses de 30; fevereiro | 31: [8, 15, 18, 23, 29] | col87/ec802 [nao verificado] |
| 109 | 1 | meses de 30; fevereiro | 31: [6, 13, 20, 27] | col245/ec1381 [nao verificado] |
| 115 | 1 | fevereiro | 31: [4, 11, 18, 20, 25]; 30: [5, 8, 12, 19, 26] | col822/ec1360 [nao verificado] |
| 118 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col43/ec1411 [nao verificado] |
| 123 | 1 | meses de 31; fevereiro | 30: [4, 11, 18, 22, 25] | col865/ec1357 [nao verificado] |
| 124 | 1 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col826/ec967 [nao verificado] |
| 127 | 1 | meses de 30; fevereiro | 31: [2, 10, 25, 28, 29] | col950/ec1321 [nao verificado] |
| 128 | 1 | meses de 30; fevereiro | 31: [2, 9, 11, 16, 23, 30] | col369/ec1313 [nao verificado] |
| 132 | 1 | meses de 30; fevereiro | 31: [5, 12, 19, 25] | col903/ec1460 [nao verificado] |
| 133 | 1 | meses de 30; fevereiro | 31: [4, 6, 13, 20, 27] | col129/ec853 [nao verificado] |
| 134 | 1 | meses de 30; fevereiro | 31: [1, 7, 13, 19, 25] | col789/ec1472 [nao verificado] |
| 135 | 1 | meses de 30; fevereiro | 31: [4, 10, 16, 22, 28] | col967/ec1316 [nao verificado] |
| 136 | 1 | meses de 30; fevereiro | 31: [6, 12, 18, 24, 30] | col446/ec384 [nao verificado] |
| 137 | 1 | meses de 30; fevereiro | 31: [2, 9, 16, 20, 25] | col502/ec1150 [nao verificado] |
| 140 | 1 | fevereiro | 31: [2, 8, 16, 22, 30]; 30: [2, 8, 16, 22, 30] | col899/ec1512 [nao verificado] |
| 141 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col444/ec1513 [nao verificado] |
| 143 | 1 | meses de 31; fevereiro | 30: [11] | col1022/ec1521 [nao verificado] |
| 144 | 1 | meses de 31; fevereiro | 30: [8, 14, 22, 28] | col941/ec1556 [nao verificado] |
| 145 | 1 | meses de 31; fevereiro | 30: [7, 15, 21, 29] | col945/ec1255 [nao verificado] |
| 146 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col985/ec1418 [nao verificado] |
| 149 | 1 | fevereiro | 31: [6, 13, 15, 20, 27]; 30: [7, 14, 15, 21, 28] | col191/ec161 [nao verificado] |
| 150 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col933/ec1184 [nao verificado] |
| 151 | 1 | meses de 31; fevereiro | 30: [1, 5, 12, 19, 26] | col991/ec1425 [nao verificado] |
| 152 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 23, 30] | col992/ec1584 [nao verificado] |
| 153 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col474/ec1270 [nao verificado] |
| 154 | 1 | meses de 30; fevereiro | 31: [1, 10, 15, 24, 29] | col529/ec1038 [nao verificado] |

Leitura em [nao verificado]; nada foi escrito no banco. [nao verificado] Cadastro e do admin (L-099): corrigir pela tela do calendario. [nao verificado]

## 16/11 13:24 · sessao 43aa1fa989b8 · - · - · origem=Ronald (tmux)

- **Célula versus foto:** `EC.eh_dia_trabalho` retorna diretamente `cel.trabalha` se existir célula; consulta cálculo/foto somente sem célula (`app/escala/models.py:1087-1096`). O cálculo puro lê `FolgaDia` do próprio vínculo/mês e responde `False` se a data estiver na foto (`app/escala/models.py:1280-1288`, `app/escala/models.py:1309-1317`). `EC.marcos_do_dia` lê `dna.marcos`; sem marcos, usa template (`app/escala/models.py:1117-1133`).

- **Grade/espelho leem folga por cima:** `_aplicar_folgas_foto` consulta juiz da folga e altera apenas o dicionário da grade: `tipo_dia='folga'`, colunas `None`, órfãs vazias (`app/escala/utils.py:1140-1153`); a grade composta chama esse overlay (`app/escala/utils.py:1844`). O espelho deriva seu flag `folga` desse `tipo_dia` e o publica no dia (`app/ponto/services/espelho.py:673-679`, `app/ponto/services/espelho.py:764-771`).

- **Precedência também sobrepõe:** lê o `trabalha` da célula como `previsto`; mesmo `previsto=True`, uma folga lançada válida entra na lista e vence trabalho pela ordem de precedência (`app/ponto/precedencia.py:347-387`, `app/ponto/precedencia.py:46`). Folga de calendário só vale para o vínculo do dia; manual vale para pessoa (`app/ponto/precedencia.py:235-239`). Espelho carrega esses fatos (`app/ponto/services/espelho.py:646-649`); o compositor responde `FOLGA` quando `tipo_do_dia='folga'` (`app/ponto/services/dia_decidido.py:370-372`).

- **Motor segue célula:** classificação de folga trabalhada pergunta a `EC.eh_dia_trabalho`; só sua resposta `False` aciona FT (`app/ponto/motor_calculo_v2.py:2445-2469`), separado em `periodos_ft` (`app/ponto/motor_calculo_v2.py:2492-2494`). Marcos vêm de EC e DNA, com fallback (`app/ponto/motor_calculo_v2.py:2290-2291`, `app/ponto/motor_calculo_v2.py:1532-1540`; outros consumidores `app/ponto/motor_calculo_v2.py:2686-2691`, `app/ponto/motor_calculo_v2.py:3014-3017`, `app/ponto/motor_calculo_v2.py:3391-3396`). Portanto, célula `True` e folga válida na foto permitem grade/precedência `folga` enquanto esta classificação do motor segue trabalho, pelas condições citadas.

- **Folha:** fechamento monta datas previstas por `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:724-733`), alimenta motor com células e chama `apurar_por_fatia` (`app/ponto/services/fechamento.py:748-756`). Horas normais/extras leem resultado (`app/ponto/services/fechamento.py:918-924`); FT lê períodos reconhecidos e publica `horas_folga_trabalhada` (`app/ponto/services/fechamento.py:925-928`, `app/ponto/services/fechamento.py:965-966`). TXT lê fechamentos recebidos ou persistidos (`app/folha/export.py:717-722`) e seus eventos (`app/folha/export.py:725-739`), incluindo `horas_folga_trabalhada` (`app/folha/export.py:439-446`).

- **Totais/pago do espelho:** totais chamam `folha_manda` (`app/ponto/services/espelho.py:1283-1284`), que chama `totais_da_folha` (`app/relatorios/cartao_pela_celula.py:320-323`); esta chama `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:69-76`), a mesma apuração em modo leitura (`app/ponto/services/fechamento.py:1759-1761`). Pago/rubricas do dia leem `DiaPago` (`app/ponto/services/espelho.py:805-813`).

- **Dono individual:** os insumos necessários para executar `dono_da_divergencia` não estão no dado recebido `[nao verificado]`. O juiz distingue cadastro por flags específicas, depois batida, fase e `em_cadastro_x_realidade`; só o resíduo é estrutura (`app/ponto/management/commands/e6_oraculo.py:98-110`). Causa executada e dono individual do relato não se deduzem de `FolgaDia`, `trabalha=True` e `regeneracoes=0` isoladamente `[nao verificado]`.

## 16/11 13:24 · sessao 325eca3c6fbc · - · - · origem=Ronald (tmux)

O módulo escritor de `CelulaDia` é `app/ponto/portas/celula.py`; o registro de portas aponta somente para ele e o selo tem allowlist vazia (`app/core/portas.py:107-113`; `app/ponto/tests/test_chokepoint_celula_gate.py:19-23,48-54`).

- **Geração normal:** `gerar_celulas_periodo` chama `ec.eh_dia_trabalho_calculado(d)` e cria a célula com `trabalha=valor`, geradora e DNA (`app/ponto/portas/celula.py:911-918`). Célula editada fica; passado existente só recebe DNA ausente, sem mudar `trabalha`; hoje/futuro existente é pulado pela função (`app/ponto/portas/celula.py:919-935`). O command possui suplemento próprio que regenera **hoje**, pela porta, antes da geração (`app/ponto/management/commands/gerar_celulas.py:303-325,345-350`).
- **Regeneração:** lê `eh_dia_trabalho_calculado`, monta DNA e cria ausentes; para existente `origem='gerada'` divergente, altera `trabalha`, geradora e DNA e incrementa `regeneracoes`, guardando DNA anterior e instante (`app/ponto/portas/celula.py:289-311`). Não depende de a data ser futura; é a exceção formal ao passado imutável (`app/ponto/portas/celula.py:112-135`; `CLAUDE.md:149-157`). Recusa vínculo sem vigência declarada e pula competência exportada, salvo ato explícito `apesar_da_lavra` (`app/ponto/portas/celula.py:147-162,218-221,284-288`).
- **Outras escritas de trabalha:** fora da vigência, a regeneração pode zerar `trabalha`, com ramos distintos para futuro e passado (`app/ponto/portas/celula.py:229-282`); `reverter_regeneracao_dia` restaura pelo DNA anterior e incrementa carimbos (`app/ponto/portas/celula.py:817-837`); `editar_dia` grava True/False, DNA, geradora, `origem='editada'`, autor/data, exigindo autor e motivo (`app/ponto/portas/celula.py:597-602,622-647`). Edição não incrementa os campos de regeneração (`app/ponto/portas/celula.py:575-580`). Troca de plantão delega duas edições ao mesmo escritor, cedente False e substituto True (`app/ponto/portas/celula.py:670-679,706-715`).

**Caminho foto → célula:** foto mensal é gravada e relavrada na mesma transação (`app/escala/folgas.py:150-163,193-218`). `eh_dia_trabalho_calculado` consulta `FolgaDia` do vínculo/mês; data presente na foto dá False (`app/escala/models.py:1280-1288,1296-1317`). A relavra chama a regeneração para o passado e `regenerar_futuras_do_cadastro` para futuro (`app/escala/folgas.py:280-300`). O suplemento futuro seleciona apenas células **geradas já existentes**, a partir de amanhã e dentro de `_janela_default`, respeitando o dono do dia (`app/ponto/services/cadastro_futuro.py:26-45`). Essa janela é a união das competências **correntes** das empresas ativas (`app/ponto/management/commands/gerar_celulas.py:290-300`).

Assim, uma foto de dezembro pode ser gravada sem alcançar células de dezembro que estejam fora daquela janela; o dado fornecido não informa dia do ato, cortes das empresas, origem/geradora das células nem se alguma guarda barrou — o motivo concreto é `[nao verificado]`. `regeneracoes=0` não prova que a função nunca foi chamada: o incremento só ocorre no ramo de reescrita divergente (`app/ponto/portas/celula.py:297-311`).

**Portas de cadastro relevantes:** vínculo criado/atualizado copia calendário padrão e gera foto do mês corrente e seguinte (`app/colaboradores/services/vinculo.py:151-166,283-298`); depois regenera passado e futuras existentes (`app/colaboradores/services/vinculo.py:315-340`). Troca do calendário do vínculo sincroniza foto futura quando o FK muda (`app/colaboradores/services/vinculo.py:450-465`). Salvar calendário sincroniza seus vínculos (`app/escala/folgas.py:571-601`). A sincronização específica poupa passado e seleciona células futuras existentes, reconcilia foto futura mensal e regenera pela porta (`app/escala/folgas.py:605-642`). Mudança de vigência e âncora tem signals separados para passado e futuro (`app/escala/signals.py:54-83,111-134`); edição de template também propaga pela regeneração (`app/escala/services/cadastro_tipo.py:332-366`).

**(b): sim, a chamada mensal `gerar_foto_folgas` alcança passado.** Sem `datas_foto`, seleciona foto de calendário pelo mês inteiro, apaga e recria todas as datas do calendário; não há filtro `>= hoje` (`app/escala/folgas.py:180-197`). Chama relavra desde o primeiro dia do mês (`app/escala/folgas.py:212-218`), e o ramo passado regenera de `desde` até **hoje**, sem `min(ate, hoje)` (`app/escala/folgas.py:290-296`). Portanto células passadas geradas podem ser tocadas pela exceção formal, respeitadas as guardas; não são genericamente protegidas dessa chamada pela irretroatividade (`app/ponto/portas/celula.py:284-311`; `CLAUDE.md:149-157`).

## 16/11 13:24 · sessao 02f6ef787e65 · - · - · origem=Ronald (tmux)

`FolgaDia` tem módulo escritor declarado único em `app/core/portas.py:471`: `app/escala/folgas.py`.

| Porta | Escrita e chamada de célula | Entradas |
|---|---|---|
| `gerar_foto_folgas` | Abre transação e chama `_gerar_foto_folgas_no_ato` (`app/escala/folgas.py:150-163`); apaga foto de calendário do mês e recria datas, preservando manual (`app/escala/folgas.py:180-197`); chama relavra (`app/escala/folgas.py:215-218`). | Aplicar calendário ao tipo (`app/escala/views.py:376-412`); vincular folga no colaborador (`app/colaboradores/views.py:212-246`); herdar padrão do template (`app/colaboradores/services/vinculo.py:151-166`, chamadas em `:284` e `:298`); comando `lavrar_foto_folgas --apply` (`app/escala/management/commands/lavrar_foto_folgas.py:159-170`), cron em `app/config/crons.py:425`. |
| `sincronizar_foto_futura_do_calendario` | Seleciona células após hoje (`app/escala/folgas.py:621-624`), reconcilia somente datas futuras dos meses encontrados (`:634-639`) e regenera futuras (`:640-642`). | Salvar calendário (`app/escala/folgas.py:597-601`); trocar calendário do vínculo quando ID muda (`app/colaboradores/services/vinculo.py:459-465`). |
| `lancar_folga` | `get_or_create` e relavra no mesmo atomic (`app/escala/folgas.py:329-339`). | Toggle do Plano (`app/escala/views_plano_folgas.py:124-132`); declaração pela supervisão (`app/ponto/services/declarar_dia.py:179-193`). |
| `alterar_folga` | Chama `_foto_do_dia`, depois relavra (`app/escala/folgas.py:377-382`); o helper apaga, cria ou troca `tipo` (`:343-366`). | Toggle do Plano (`app/escala/views_plano_folgas.py:134-137`). |
| `aplicar_folgas_em_lote` | Chama mesmo helper e relavra os dias gravados, recusando competência anterior à corrente (`app/escala/folgas.py:401-421`). | Plano em lote (`app/escala/views_plano_folgas.py:163-185`). |
| `reparentar_folga` | Recusa calendário (`app/escala/folgas.py:484-485`); salva vínculo novo (`:515-516`) e relavra (`:523-525`). Competência passada usa `so_rejulgar`, sem relavrar DNA (`:521-525`). | Signal de nascimento/mudança de vigência (`app/escala/signals.py:185-207`), via `reparentar_folgas_do_colaborador` (`app/escala/folgas.py:529-541`); comando `reparentar_folgas --apply` (`app/escala/management/commands/reparentar_folgas.py:22-34`). |

**Resposta (b): sim, `gerar_foto_folgas` para mês corrente grava/apaga antes de hoje.** Não há corte por hoje na seleção mensal, no `delete` ou no `bulk_create` (`app/escala/folgas.py:180-197`). A célula passada também é encaminhada à regeneração formal: `relavrar_celulas_da_foto` chama `regenerar_celulas_vinculo(ec, desde, hoje, estender_horizonte=False)` (`app/escala/folgas.py:290-296`). Portanto, a irretroatividade não impede toda alteração: a porta é a exceção formal para passado errado por cadastro (`app/ponto/portas/celula.py:112-119`; `CLAUDE.md:149-156`).

As guardas concretas da regeneração são vínculo sem vigência declarada (`app/ponto/portas/celula.py:153-162`), competência exportada (`:218-221`, `:284-288`), e origem humana: atualização normal exige `cel.origem == 'gerada'` (`:297-311`). Futuro alcançado pela relavra passa pelo suplemento que seleciona células geradas já existentes dentro da janela e do dono correto (`app/ponto/services/cadastro_futuro.py:26-45`).

**Atenção à diferença entre portas:** salvar/trocar calendário usa primeiro a sincronização futura, que preserva passado (`app/escala/folgas.py:621-639`; `app/colaboradores/services/vinculo.py:463-465`), mas as views `vincular_folga` e `aplicar_folga_calendario` também chamam depois `gerar_foto_folgas` para mês corrente e próximo, sem esse corte (`app/colaboradores/views.py:243-246`; `app/escala/views.py:411-412`).

No caso dezembro/FolgaDia existente/célula `True`/regenerações zero, o caminho esperado quando a foto chega depois da célula é `gerar_foto_folgas → relavrar_celulas_da_foto → regenerar_futuras_do_cadastro → regenerar_celulas_vinculo` (`app/escala/folgas.py:215-218`, `:297-299`; `app/ponto/services/cadastro_futuro.py:26-45`). O fato de esse caminho ter sido executado, a data da execução, a janela naquele instante, origem da célula, vigência/dono e eventual bloqueio não são demonstrados pelo dado fornecido: `[nao verificado]`. A divergência normal do ramo principal, quando atualizada, incrementa `regeneracoes` (`app/ponto/portas/celula.py:297-311`), portanto zero sozinho não prova que a função nunca foi chamada.

## 16/11 13:22 · sessao 7e002ba200c0 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: corrigir a exclusividade da função escritora e fechar o aviso/RED literal; item 2: falta a relação individual raspada.** (`app/escala/folgas.py:659`; `app/templates/core/_dialogo.html:130`; `app/escala/tests/test_folga_cadastro.py:30`; relação individual: [nao verificado]).

**Item 1 — editor**

1. **Premissas: PROCEDE, exceto “única porta = função salvar”.**

   - **PROCEDE:** no modo `dias_mes`, aparece uma grade por vez, começando pela de 31: a view ordena 31/30/28, o JS começa com `idx = 0` e substitui o conteúdo de um único `fc-grade`. (`app/escala/views.py:318`; `app/templates/escala/folga_calendario_editar.html:45`; `app/templates/escala/folga_calendario_editar.html:74`; `app/templates/escala/folga_calendario_editar.html:86`).
   - **PROCEDE:** salva com outras grades vazias sem confirmação específica: o formulário submete normalmente; campos ausentes viram listas vazias, entregues à gravação. (`app/templates/escala/folga_calendario_editar.html:9`; `app/templates/escala/folga_calendario_editar.html:59`; `app/escala/views.py:342`; `app/escala/views.py:349`; `app/escala/views.py:356`).
   - **PROCEDE com precisão:** cada `FolgaCalendarioTamanho` guarda **uma** grade; o conjunto comporta 28/30/31, com unicidade por calendário/tamanho. O modelo não exige três registros preenchidos. (`app/escala/models.py:1563`; `app/escala/models.py:1565`; `app/escala/models.py:1570`; `app/escala/models.py:1575`).
   - **NAO PROCEDE a exclusividade da função:** `salvar_calendario_de_folga` atualiza/cria grades; `criar_calendario_de_folga` também as cria. A porta canônica registrada é o **módulo** `escala/folgas.py`. A função salvar também exclui os tamanhos quando muda para `semana_dia`. (`app/escala/folgas.py:594`; `app/escala/folgas.py:659`; `app/core/portas.py:473`; `app/escala/folgas.py:588`).
   - **PROCEDE o criador rápido:** o wizard chama `criar_calendario_de_folga`; o descarte `d <= tam` acontece **no servidor**. (`app/escala/views_wizard.py:192`; `app/escala/folgas.py:658`; `app/escala/folgas.py:661`).
   - **PROCEDE contexto e POST:** `editar_folga_calendario` monta o contexto; o JS serializa os dias selecionados de todos os tamanhos, e a view monta explicitamente as três listas antes de chamar salvar. Grades vazias chegam por ausência de campos, convertida em `[]`. (`app/escala/views.py:362`; `app/escala/views.py:369`; `app/templates/escala/folga_calendario_editar.html:108`; `app/escala/views.py:340`; `app/escala/views.py:356`).

2. **Colisão: NAO PROCEDE bloqueio demonstrado por O148; PROCEDE sobreposição do editor.**

   O O148-EDITAR contempla essa edição, mas consta como **só registrar**, com versionamento aguardando corte; aparece em LEIS entre atos, não como lei vigente. Isso não prova reserva de arquivo em obra ativa. Concorrência efetiva: [nao verificado]. (`app/docs/CORTES.md:101`; `app/docs/CORTES.md:357`; `app/docs/LEIS.md:206`; `app/docs/LEIS.md:313`).

   As leis aplicáveis exigem mesma autoridade para tela/ação, ajuda pelo componente e nenhum diálogo nativo; não demonstram proibição da projeção proposta. (`app/docs/LEIS-UI.md:23`; `app/docs/LEIS-UI.md:77`; `app/docs/LEIS-UI.md:87`).

3. **Condições literais: PROCEDE ressalva.**

   - **Repetir sem `static/js` novo: PROCEDE a viabilidade.** O template já contém JS inline, seleções por tamanho e sincronização dos campos. É possível copiar seleções na tela, filtrando pelo tamanho, e continuar submetendo pela mesma view; implementação concreta: [nao verificado]. (`app/templates/escala/folga_calendario_editar.html:70`; `app/templates/escala/folga_calendario_editar.html:73`; `app/templates/escala/folga_calendario_editar.html:108`).
   - **Componente da casa: PROCEDE sua existência, mas não o par literal pronto.** É `core/_dialogo.html`, incluído pela base, com `hxConfirmar`. Aceita “Salvar assim mesmo” como verbo; o negativo está fixo em **“Cancelar”**, sem parâmetro para “Voltar”. Fechar esse literal apenas pela API atual não procede; adaptação dentro do escopo ainda é [nao verificado]. (`app/templates/base.html:388`; `app/templates/core/_dialogo.html:172`; `app/templates/core/_dialogo.html:191`; `app/templates/core/_dialogo.html:130`).
   - **RED do POST: NAO PROCEDE como vermelho da cura visual.** Já existe teste que envia valores distintos nas três grades e confere a persistência; criar e editar usam a mesma view. O teste não observa explicitamente a chamada à porta. Para provar “pela porta”, cabe observar a função real e conferir persistência; isso reforça o contrato existente. O RED da cura deve morder na marca/aviso ausentes, com completo sem aviso. Execução dos testes: [nao verificado]. (`app/escala/tests/test_folga_cadastro.py:18`; `app/escala/tests/test_folga_cadastro.py:30`; `app/escala/urls.py:35`; `app/escala/urls.py:36`; `app/escala/views.py:356`).

4. **Alcance: PROCEDE escrita ao salvar; extrapolação financeira/exportada não demonstrada.**

   Salvar já atualiza as grades, sincroniza fotos futuras dos vínculos e regenera células futuras com `recalculo=False`. Portanto, o ato do admin produz escrita além da definição das grades, pelo caminho existente. (`app/escala/folgas.py:594`; `app/escala/folgas.py:600`; `app/escala/folgas.py:639`; `app/escala/folgas.py:640`).

   O caminho seleciona datas posteriores a hoje. Impacto real em dinheiro/exportada, migração nova ou necessidade de `!` fora do declarado: [nao verificado]. A lei exige `!` para gravado exportado e alterações de escala/vínculo fora da autorização literal. (`app/escala/folgas.py:623`; `app/ponto/services/cadastro_futuro.py:27`; `CLAUDE.md:265`).

**Item 2 — lista de trabalho**

1. **Premissas/dado suficiente: NAO PROCEDE.** O raspado fornecido contém totais e menciona calendários 51/154, mas não entrega a relação calendário → grades → colaboradores → quantidade de usos. A lista completa e sua ordenação não podem ser conferidas ou reconstruídas desse material. [nao verificado].

2. **Colisão com lei/O148: NAO PROCEDE colisão demonstrada.** Listar pendências de cadastro para o admin é compatível com L-099; O148-EDITAR trata edição/versionamento, exclusão e busca. (`CLAUDE.md:179`; `app/docs/CORTES.md:357`).

3. **Condição que não fecha: PROCEDE falta de evidência individual.** A lista existente no código ordena por nome e coleta todos os vínculos, sem filtro de atividade nesses acessos; não comprova a lista pedida “em uso, ordenada por uso”. Falta o resultado raspado individual da sonda. (`app/escala/views.py:286`; `app/escala/views.py:288`; `app/escala/views.py:300`; resultado individual: [nao verificado]).

4. **Alcance: NAO PROCEDE escrita de negócio demonstrada; há escrita operacional no envelope.** O envelope prevê role/grants e arquivo de ambiente, além de acrescentar trilha em log. O wrapper executa a sonda antes do Codex e raspa a saída. Execução específica, grants reais e efeitos desta sonda: [nao verificado]. (`bin/sonda_leitura.sh:28`; `bin/sonda_leitura.sh:248`; `bin/codex_pergunta.sh:328`; `bin/codex_pergunta.sh:334`).

VEREDITO: DEVOLVE <1: perguntas 1 e 3; 2: perguntas 1 e 3>

## 16/11 13:20 · sessao db8b5d6bc7f1 · - · - · origem=Ronald (tmux)

- **“Salvar é a única função escritora” — NÃO PROCEDE.** `salvar_calendario_de_folga` faz `update_or_create` das grades e as apaga ao trocar para `semana_dia`; `criar_calendario_de_folga` também cria as grades. A porta canônica declarada é o **módulo** `escala/folgas.py`, não exclusivamente a função salvar (`app/escala/folgas.py:588`, `app/escala/folgas.py:594`, `app/escala/folgas.py:645`, `app/escala/folgas.py:659`, `app/core/portas.py:473`).

- **Criador rápido — PROCEDE; corte no servidor.** `wizard_calendario_criar` recebe dias, valida `1..31` e chama `criar_calendario_de_folga`; esta percorre `(31,30,28)` e grava somente `d <= tam` (`app/escala/views_wizard.py:140`, `app/escala/views_wizard.py:179`, `app/escala/views_wizard.py:188`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`).

- **Fonte — PROCEDE com precisão:** cada registro de `FolgaCalendarioTamanho` guarda uma grade; o calendário pode ter registros de 28,30,31, exclusivos por calendário/tamanho; não existe exigência de que os três estejam presentes ou preenchidos (`app/escala/models.py:1563`, `app/escala/models.py:1565`, `app/escala/models.py:1570`, `app/escala/models.py:1575`). `dias_por_tamanho()` lê as linhas existentes, sem completar grades ausentes (`app/escala/models.py:1558`).

- **Contexto e POST — PROCEDE.** A view define 31,30,28, lê os marcados, monta os três elementos e entrega o contexto ao editor (`app/escala/views.py:318`, `app/escala/views.py:362`, `app/escala/views.py:369`). No POST `dias_mes`, percorre os três tamanhos, transforma campos ausentes em listas vazias, valida o intervalo e passa o dicionário completo à porta (`app/escala/views.py:340`, `app/escala/views.py:342`, `app/escala/views.py:349`, `app/escala/views.py:356`).

- **Censo de leitores diretos:** `dias_por_tamanho()` é consumido em produção por `datas_que_o_calendario_manda`, que delega a `folgas_do_mes`; este escolhe o tamanho real, usando 28 para fevereiro bissexto, e devolve vazio quando a grade falta/está vazia (`app/escala/folgas.py:56`, `app/escala/folgas.py:12`, `app/escala/folgas.py:14`). A função de datas alimenta divergência, vigia, geração de foto e sincronização futura (`app/escala/folgas.py:99`, `app/escala/folgas.py:127`, `app/escala/folgas.py:158`, `app/escala/folgas.py:629`). Lista e editor também leem diretamente `cal.tamanhos` (`app/escala/views.py:292`, `app/escala/views.py:362`).

- **RED do POST: a obrigação já existe no código e em teste.** `test_post_cria_calendario_e_3_tamanhos` envia valores distintos nos três campos e confere o dicionário persistido; criar e editar usam a mesma view (`app/escala/tests/test_folga_cadastro.py:18`, `app/escala/tests/test_folga_cadastro.py:30`, `app/escala/urls.py:35`, `app/escala/urls.py:36`). O teste de porta também confere as três grades e idempotência (`app/escala/tests/test_porta_escala.py:95`). Logo um teste apenas dessa condição **não evidencia RED da mudança visual**, por inspeção desses caminhos; resultado de execução é [nao verificado]. Para testemunhar “pela porta”, uma verificação adicional precisaria observar a chamada da função real sem substituir a persistência; seria reforço de contrato, não o RED visual (`app/escala/views.py:354`, `app/escala/views.py:356`, `app/escala/folgas.py:594`).

- **Outros escritores em testes, separados de produção:** preparações com `objects.create` em `app/colaboradores/tests/test_vincular_folga_view.py:31`, `app/colaboradores/tests/test_vincular_folga_view.py:35`, `app/colaboradores/tests/test_vinculo_folga_padrao.py:25`, `app/escala/tests/test_folga_semana_dia.py:65`, `app/escala/tests/test_torneira_folgas.py:99`, `app/escala/tests/test_folga_models.py:11`, `app/escala/tests/test_gerador_folgas.py:32`, `app/escala/tests/test_gerador_folgas.py:34`, `app/escala/tests/test_smoke_clique_folga_calendario.py:112`, `app/escala/tests/test_editor_folga_modo.py:29`, `app/escala/tests/test_folga_aplicar.py:37`, `app/escala/tests/test_foto_folga_continua.py:64`, `app/escala/tests/test_selo_foto_folgas_relavra.py:46`; alteração por instância em `app/escala/tests/test_foto_folga_continua.py:145` e `app/escala/tests/test_foto_folga_continua.py:146`. A migração encontrada cria o esquema, sem carga de dados (`app/escala/migrations/0015_folgacalendario_folgacalendariotamanho.py:29`). A guarda de arquitetura proíbe escritores de produção fora da porta, com allowlist vazia (`app/escala/tests/test_chokepoint_escala_gate.py:27`, `app/escala/tests/test_chokepoint_escala_gate.py:55`).

## 16/11 13:20 · sessao 099ae93ca4b9 · - · - · origem=Ronald (tmux)

- **PROCEDE — item 1 alcança o editor previsto na O148.** O complemento histórico nomeia `editar_folga_calendario` como “EDITAR-NAO-PUBLICA”; O148-EDITAR contempla edição com uso por versão. Referências: `app/docs/O148-COMPLEMENTO.md:15`, `app/docs/CORTES.md:357`.

- **NAO PROCEDE — afirmar que versionamento já seja lei vigente ou que exista reserva de arquivo em obra ativa.** Os registros dizem “só registrar”, “não constrói agora” e “edição por versão espera corte de lei”. O148-EDITAR aparece na seção “CORTES que são ATO, não lei”. Referências: `app/docs/CORTES.md:101`, `app/docs/CORTES.md:357`, `app/docs/BACKLOG.md:819`, `app/docs/LEIS.md:206`, `app/docs/LEIS.md:313`. Concorrência efetiva no mesmo arquivo: **[nao verificado]**.

- **NAO PROCEDE — conflito necessário com leis de UI.** O aviso pode seguir componente da casa, sem diálogo nativo, e consumir o mesmo estado usado pela ação; essas são as exigências aplicáveis. Referências: `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`. A raia normalmente permite templates/static/testes; a exceção nominal da view consta apenas no anexo fornecido **[nao verificado na cópia]**; regra geral: `CLAUDE.md:312`.

- **Achado material — a porta declarada não é o único escritor.** `criar_calendario_de_folga` também cria `FolgaCalendarioTamanho`; o descarte acima do tamanho ocorre no servidor. Referências: `app/escala/folgas.py:645`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`.

- **Achado material — salvar já produz efeitos além da definição das grades.** A porta sincroniza fotos futuras dos vínculos e chama regeneração futura pela porta da célula. A descrição histórica “grava e redireciona sem rematerializar” está superada nesse ponto. Referências: `app/escala/folgas.py:600`, `app/escala/folgas.py:639`, `app/escala/folgas.py:640`; descrição histórica: `app/docs/O148-COMPLEMENTO.md:15`.

## 16/11 12:57 · sessao 24f76b5f4000 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

1) **NAO PROCEDE a objeção às premissas.**

- A fonte existe, recebe `(cal, ano, mes)` e concentra a escolha entre os dois modos; os resolvedores ficam abaixo dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:54`). Há selo contra chamadas dos resolvedores fora desse módulo (`app/escala/tests/test_foto_folga_continua.py:281`).
- Leitores diretos encontrados: `divergencia_foto_x_calendario` (`app/escala/folgas.py:99`), `calendario_com_template_vazio_no_mes` (`app/escala/folgas.py:127`), `gerar_foto_folgas` (`app/escala/folgas.py:158`) e `sincronizar_foto_futura_do_calendario` (`app/escala/folgas.py:629`).
- A rota recebe `cid` inteiro e a view busca `FolgaCalendario.pk=cid` (`app/escala/urls.py:36`, `app/escala/views.py:319`).
- O bloco e a caixa existem. O texto vazio é **“— usar as folgas da escala (padrão do template) —”**, conforme a descrição do aval; não é literalmente “sem calendario” (`app/templates/colaboradores/partials/painel_vinculo.html:186`, `app/templates/colaboradores/partials/painel_vinculo.html:197`).
- `painel_vinculo` escolhe o primeiro vínculo ativo, fornece os calendários e renderiza o partial. A opção selecionada compara o calendário com **`escala_ativa.folga_calendario_id`**, não com o padrão do TipoEscala (`app/colaboradores/views.py:268`, `app/colaboradores/views.py:295`, `app/colaboradores/views.py:297`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).

2) **NAO PROCEDE colisão no escopo declarado.**

- Ler essa fonte atende à testemunha que não recalcula e ao juiz único da tela (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`).
- O grep encontrou referências a outro sítio de `colaboradores/views.py`, a `reparentar_folga` e à porta de troca do calendário; nenhuma exige alteração nesta projeção (`app/docs/LEIS.md:64`, `app/docs/CORTES.md:349`, `app/docs/LEIS.md:189`). Outra referência específica impeditiva: `[nao verificado]`.
- O148-EDITAR permanece **“SO REGISTRAR”**, com edição por versão aguardando corte; abrir a edição existente não implementa esse complemento (`app/docs/CORTES.md:356`, `app/escala/views.py:369`).
- A exceção nominal do anexo cobre o contexto `.py` diante da restrição geral da raia UI (`CLAUDE.md:312`); registro dessa exceção em arquivo: `[nao verificado]`.
- Comparação com a lista BUG-PAR fornecida: nenhum arquivo em comum com os dois alvos declarados; confirmação externa dos arquivos efetivamente em voo: `[nao verificado]`.

3) **NAO PROCEDE impedimento literal; há cuidados obrigatórios de implementação.**

- A fonte aceita cada mês separadamente, sem restringir corrente/próximo; a decisão dos dias permanece integralmente nela (`app/escala/folgas.py:43`, `app/escala/folgas.py:55`).
- **Somente contexto inicial deixaria a projeção velha ao trocar a caixa:** não há atualização no select, o POST usa `hx-swap="none"` e retorna apenas toast (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`). Isso cabe resolver nos alvos autorizados, projetando pela fonte os calendários já fornecidos e alternando sua apresentação no partial (`app/colaboradores/views.py:295`, `app/escala/folgas.py:43`).
- O RED pode comparar HTML com a fonte real; há precedente de GET e inspeção do painel (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`). Para morder derivação paralela, deve também substituir o retorno da fonte por datas sentinela e exigir sua exibição. Execução desse RED/mutante: `[nao verificado]`.
- Ajuda por componente e ausência de diálogo nativo continuam exigíveis (`app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`).

4) **NAO PROCEDE alcance adicional necessário.**

A projeção chama um leitor que devolve datas; abrir a edição por GET não entra no ramo de escrita (`app/escala/folgas.py:52`, `app/escala/views.py:320`, `app/escala/views.py:362`). Não exige alterar as portas que trocam vínculo ou materializam folgas (`app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`). O `!` protege mudança do **dado** de escala/vínculo e do gravado de exportada (`CLAUDE.md:265`). Patch, migração, efeitos financeiros e cumprimento posterior desses limites: `[nao verificado]`.

VEREDITO: EXECUTA

## 16/11 12:55 · sessao 01ed894fced5 · - · - · origem=Ronald (tmux)

Não identifiquei impedimento intrínseco no literal; há uma armadilha de atualização que deve entrar no RED.

- A fonte aceita `(cal, ano, mes)` e retorna `set[date]`; resolve os modos internamente e aceita calendário ausente. Pode ser chamada duas vezes, inclusive na virada de ano, sem derivar folgas fora dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:52`, `app/escala/folgas.py:54`).
- A seleção inicial vem de `escala_ativa.folga_calendario_id`; o vazio já tem o texto “usar as folgas da escala (padrão do template)”. A expressão literal “sem calendario” está na resposta do escritor, não nessa opção (`app/templates/colaboradores/partials/painel_vinculo.html:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`, `app/colaboradores/views.py:250`).
- Uma projeção estática ficaria velha ao mudar/salvar: o select não tem atualização e o formulário usa `hx-swap="none"`; o POST HTMX devolve corpo vazio e toast. O carregamento existente só se repete automaticamente após swap da linha (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`, `app/templates/colaboradores/vincular.html:165`).
- Isso pode ser resolvido nos locais autorizados: a view já disponibiliza todos os calendários; o contexto pode acrescentar os dois retornos da fonte por calendário, e o partial alternar a projeção e o link conforme a seleção, sem calcular folgas. Viabilidade por leitura: `app/colaboradores/views.py:295`, `app/colaboradores/views.py:298`, `app/escala/folgas.py:43`; implementação e desempenho `[nao verificado]`.
- Para o mutante, apenas comparar datas reais não prova dependência da fonte. Proposta: substituir temporariamente o retorno da fonte por datas sentinela durante o GET e exigir essas datas no HTML e chamadas para os dois meses; execução desse RED `[nao verificado]`. O GET do painel já possui precedente de teste com `Client` e inspeção do HTML/contexto (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`).

## 16/11 12:55 · sessao a627924a613d · - · - · origem=Ronald (tmux)

- **Pergunta 2: NAO PROCEDE, no escopo literal do item 1.** Ler a fonte única segue L-002 (`app/docs/LEIS.md:60`) e L2 de UI (`app/docs/LEIS-UI.md:23`). A regra geral de `wt-ui` limita a templates/static/teste (`CLAUDE.md:309`–313); a exceção nominal para contexto na view consta somente do anexo fornecido `[nao verificado em arquivo]`.

- Busca pelos arquivos/funções em `LEIS.md`/`CORTES.md`: `colaboradores/views.py` aparece por **form_empresa**, outro sítio (`app/docs/LEIS.md:64`); `escala/folgas.py` por **reparentar_folga**, outro predicado (`app/docs/CORTES.md:349`). Não encontrei referências exatas a `painel_vinculo`, `editar_folga_calendario`, `escala/urls.py` ou `vincular_folga` nesses dois documentos `[nao verificado: ausência não possui linha positiva]`.

- **O148-EDITAR não exige implementar versões nesta obra.** Está expressamente em “SO REGISTRAR”, com edição por versão aguardando corte (`app/docs/CORTES.md:356`). Item 1 pode apenas apontar à edição existente: GET busca o calendário e renderiza (`app/escala/views.py:319`, `app/escala/views.py:362`, `app/escala/views.py:369`); a escrita fica no ramo POST (`app/escala/views.py:320`, `app/escala/views.py:356`).

- **Pergunta 4: NAO PROCEDE se restrita a exibição/contexto/link.** `!` protege alteração de **dado** de escala/vínculo e gravado de exportada (`CLAUDE.md:265`–268); L-007 protege escritores e trilha (`app/docs/LEIS.md:65`, `app/docs/LEIS.md:189`). O resolver devolve datas sem persistir (`app/escala/folgas.py:43`–56).

- **Ponto para pergunta 3:** a caixa atual não dispara requisição ao mudar; `selected` compara o calendário **salvo** do vínculo (`app/templates/colaboradores/partials/painel_vinculo.html:195`–201). Apenas contexto inicial acompanha esse calendário salvo; acompanhar seleção ainda não salva exigiria mecanismo adicional `[nao verificado: intenção exata do anexo]`.

- O texto existente para ausência é exatamente “— usar as folgas da escala (padrão do template) —”, não a expressão “sem calendário” (`app/templates/colaboradores/partials/painel_vinculo.html:197`).

_6035 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
