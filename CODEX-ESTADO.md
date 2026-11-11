# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 10:10** · publicado em **11/11/2026 10:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 10:10 · sessao 1a4869192285 · - · - · origem=Ronald (tmux)

Censo direto, somente leitura; testes não executados, estado verde atual [nao verificado]. [nao verificado]

**Consulta e leitores**

- `consulta_do_app` deriva listas novas, preserva ordem por ID e separa `pendentes` por `respondida_em is None` **e** `disputa.fechada_em is None`; demais perguntas vivas vão ao histórico. Fonte anterior: `perguntas_do_app → q_vivas + pergunta_viva(C6)`. `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/models.py:3326`, `app/chamados/juizes.py:839`.
- **7 sítios de consumo em produção**: dois badges (`app/api/views_core.py:314`, `app/api/views.py:325`), GET chamados (`app/api/views.py:1005`), GET perguntas (`app/api/views.py:2543`), dashboard (`app/core/views.py:253`), aviso de pendência (`app/chamados/services/aviso_pendencia.py:9`), cobrança (`app/chamados/services/cobranca.py:48`).
- A consulta não contém `status_local__in=VIVOS`; esse filtro pertence à contagem de **chamados com followup DP não lido**, nos badges. `VIVOS = aberto, em_analise, registrado`, diferente de `COBRANCA`. `app/api/views_core.py:309`, `app/api/views.py:320`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`.

**Escritores diretos dos campos que separam pendentes/histórico**

| Campo/operação | Sítios |
|---|---|
| Resposta do colaborador | `responder_pergunta_disputa_core`: atribui e salva `respondida_em`. `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1508` |
| Resposta automática | `responder_automaticamente`. `app/chamados/services/disputa_emissao.py:1653` |
| Recuperação de carimbo pela trilha | `carimbar_resposta_pela_trilha`. `app/chamados/services/disputa_emissao.py:1674` |
| Carimbo com resposta já textual | `PerguntaDisputa.carimbar`, quando `resposta` solicitado e texto presente. `app/chamados/models.py:3130`, `app/chamados/models.py:3152` |
| Voltar a pendente | `reabrir_para_correcao`: monta `respondida_em=None`, executa `qs.update(**campos)` ou delega ao descarimbo em lote. `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629` |
| Ressuscitar perguntas | `ChamadoColaborador.ressuscitar_perguntas` delega a `descarimbar_em_lote(...respondida_em=None)`; este faz update dinâmico. `app/chamados/models.py:1566`, `app/chamados/models.py:3264` |
| Nascimento da pergunta | `_nascer` recebe `**campos`, constrói e insere; inclui assinaturas recebidas. Campo nullable por definição. `app/chamados/models.py:2448`, `app/chamados/models.py:3014` |
| Fechar disputa | `DisputaSupervisao.fechar`: `fechada_em=now`, seguido de save. `app/chamados/models.py:1932` |
| Reabrir disputa | `reabrir_admin` e `reabrir_sistema`: `fechada_em=None`, seguido de save. `app/chamados/models.py:2149`, `app/chamados/models.py:2173` |
| Nascer disputa aberta | Default nullable; criações do emissor. `app/chamados/models.py:1757`, `app/chamados/services/disputa_emissao.py:587`, `app/chamados/services/disputa_emissao.py:1123`, `app/chamados/services/disputa_emissao.py:1358` |

Atribuições de `respondida_em` que **não persistem**: simulação em cópia (`_universo_da_ressuscitacao`) e restauração da memória quando carimbar recusa. `app/chamados/models.py:1508`, `app/chamados/models.py:3163`.

Vitalidade C6 também depende de `validada_em`, tranca da competência, `via_resolucao`, `materializada_em`, texto da resposta, célula/ata/veredito e testemunha do fato; o conjunto de escritores transitivos de todos esses insumos não foi encerrado neste recorte [nao verificado]. As dependências aparecem em `app/chamados/juizes.py:1324`. O registro existente reconhece oito módulos-porta de `PerguntaDisputa`, incluindo modelo, emissor, materialização, fio, reconciliador, lavra, relavra e validação. `app/core/portas.py:393`.

**Não lido do colaborador**

- Campo real: `FollowupChamado.lido_colaborador_em`, nullable. Criar nota sem carimbo faz nascer uma fonte não lida. `app/chamados/models.py:1673`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:103`.
- Escritor do carimbo: `nota.marcar_lidas`, usando mapa dinâmico `LADOS`; atualiza apenas registros ainda nulos. `app/chamados/services/nota.py:59`, `app/chamados/services/nota.py:126`.
- **3 chamadores com lado colaborador**: GET nativo marca **todos** followups DP daquele colaborador; endpoint web de um chamado; endpoint web em lote. `app/api/views.py:1050`, `app/chamados/views.py:1135`, `app/chamados/views.py:1150`.
- Chamador com lado DP modifica **outro campo**, `lido_dp_em`. `app/chamados/views.py:880`, `app/chamados/services/nota.py:59`.
- Leitores em produção: dois badges; serialização nativa `novo`/contador por followup; `_nao_lido` web/contador por chamado; duas posições no card de atendimento. `app/api/views_core.py:311`, `app/api/views.py:322`, `app/api/views.py:1128`, `app/chamados/views.py:603`, `app/templates/chamados/partials/_card_atendimento.html:51`, `app/templates/chamados/partials/_card_atendimento.html:59`.
- Outros atos que eliminam fonte: `nota.apagar_em_ensaio` apaga notas sob guarda de banco de ensaio; excluir o chamado faz cascade da FK. `app/chamados/services/nota.py:136`, `app/chamados/models.py:1668`.
- Registro da porta de criação/leitura/apagamento: `app/core/portas.py:239`.

**`status_local`: escritores que alteram entrada/saída de VIVOS**

- Campo/default `'aberto'`. `app/chamados/models.py:52`.
- **Escritor persistente de transição**: `_gravar_estado`, atribuição em `:228`, `save(update_fields=campos)` em `:296`. `app/chamados/models.py:228`, `app/chamados/models.py:296`.
- Gates locais que delegam ao núcleo: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. `app/chamados/models.py:450`, `app/chamados/models.py:654`, `app/chamados/models.py:788`, `app/chamados/models.py:806`, `app/chamados/models.py:1442`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`.
- **Nascimento idempotente**: `abrir` monta `_def['status_local']`, agrega defaults e passa ao `get_or_create`. `app/chamados/models.py:938`, `app/chamados/models.py:971`.
- **Nascimento direto**: `criar` passa `status_local=status` ao `objects.create`. A construção anterior `_nascente` é só memória. `app/chamados/models.py:1070`, `app/chamados/models.py:1095`.
- **Simulação sem persistência**: `_vivas_entre` aplica e restaura o status em memória. `app/chamados/models.py:356`, `app/chamados/models.py:371`.
- Registro existente da porta de ciclo: `app/core/portas.py:218`.
- Migrações de dados, fora do runtime: `aberto↔registrado` e `cancelado↔superado`. `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

**Contratos já presentes, sem executar**

- Selo AST de escritores de status permite precisamente `_gravar_estado`, `_vivas_entre`, `criar`, `abrir`; distingue memória e persistência, examina update/create/defaults/bulk/setattr/SQL e declara limites de resolução de tipos. `app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:340`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:449`.
- Selo da nota: nenhum escritor fora da porta e idempotência do marcar lidas. `app/chamados/tests/test_porta_nota_do_fio.py:44`, `app/chamados/tests/test_porta_nota_do_fio.py:207`.
- Matriz ORM compara endpoint de perguntas, dashboard e **ambos** kernels, incluindo dicionário completo do badge; prova que vermelho conta chamados e não followups. `app/chamados/tests/test_e3_app_orm.py:28`, `app/chamados/tests/test_e3_app_orm.py:62`.
- Contratos de leitores: aviso/cobrança/badges mesma pendente; lista nativa pendentes + histórico; frescor após tranca/toque/reabertura. `app/chamados/tests/test_e3_v4_orm.py:20`, `app/chamados/tests/test_e3_v4_orm.py:42`, `app/chamados/tests/test_e3_v4_orm.py:101`.
- Outros leitores de teste explícitos da consulta: `app/chamados/tests/test_esmeril2_left_join.py:19`, `app/chamados/tests/test_fio_le_o_juiz.py:267`, `app/core/tests/test_dashboard_badge.py:45`; contratos AST em `app/chamados/tests/test_contract_pergunta_viva.py:68`, `app/chamados/tests/test_pergunta_no_app.py:168`; ferramentas de mutação em `app/chamados/tests/e3_mutantes.py:23`.

## 11/11 10:08 · sessao 984214e9a557 · - · - · origem=Ronald (tmux)

Censo de autoridades e decisões, somente leitura:

1. **Não há juiz registrado para o rótulo/count “o que o app mostra ao colaborador agora”.** O registro de chamados contém C6 `pergunta_viva`, C13 `retencao`, C15 `fio_vivo`, C19 `tarefa_do_dia`, C20 `dia_da_pergunta_trancado`, mas nenhum para o badge ou `consulta_do_app`. `app/core/juizes.py:272`, `app/core/juizes.py:278`, `app/core/juizes.py:285`, `app/core/juizes.py:287`, `app/core/juizes.py:291`, `app/core/juizes.py:292`.

2. **A tabela `JUIZES.md` também não declara esse juiz.** Ela se define como lookup, aponta o registro vivo em `core/juizes.py`, declara que o código vivo manda e lista os juízes de chamados sem badge. `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:59`, `app/docs/JUIZES.md:78`.

3. **Autoridade existente da seleção apresentada:** `consulta_do_app(colab)` retorna exatamente `{'pendentes': [pergunta], 'historico': [pergunta]}`; lê `perguntas_do_app(colab, em_lote=True)` e separa pendentes por `respondida_em is None and disputa.fechada_em is None`, preservando ordem por id. O serviço declara que API, badge dos dois kernels e dashboard contam a mesma lista. `app/chamados/services/pergunta_no_app.py:3`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:64`, `app/chamados/services/pergunta_no_app.py:69`, `app/chamados/services/pergunta_no_app.py:75`.

4. **A vitalidade continua pertencendo ao C6:** `perguntas_do_app` usa `PerguntaDisputa.q_vivas()`, identidade do colaborador e `pergunta_viva`; `consulta_do_app` apenas particiona a apresentação. `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:49`, `app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:58`, `app/core/juizes.py:278`.

5. **`estado_app` não é este problema:** no situacional significa “sem device ativo”, “instalou, nunca bateu”, “bateu e parou” ou “ativo”; não descreve perguntas, mensagens ou badge. `app/colaboradores/services/situacional.py:191`.

Decisões técnicas sugeridas para o construtor — propostas, não implementação:

| Candidata | Pró e limite com evidência |
|---|---|
| **Wrapper de projeção junto de `consulta_do_app`, em `chamados/services/pergunta_no_app.py` — recomendação** | Mantém seleção e apresentação próximas, sem depender de uma view HTTP; pode receber `colab`, chamar uma vez `consulta_do_app`, mover para si a query não lidos e devolver listas + números + badge. A seleção já mora nesse serviço por ser o leitor do universo do app; os kernels hoje repetem query e regra. `app/chamados/services/pergunta_no_app.py:64`, `app/api/views_core.py:305`, `app/api/views_core.py:313`, `app/api/views.py:316`, `app/api/views.py:324`. |
| Acrescentar badge à própria `consulta_do_app` | Menor número de funções, mas altera contrato de retorno e adiciona query de não lidos a todos os consumidores. Há testes de igualdade **exata** com as duas chaves e teto de nove queries; portanto é candidata mais invasiva. `app/chamados/tests/test_e3_v4_orm.py:85`, `app/chamados/tests/test_e3_v4_orm.py:91`, `app/chamados/tests/test_e3_v4_orm.py:98`. |
| Novo módulo de apresentação em `chamados/services/` | Nome próprio separa a projeção da seleção e pode conservar o contrato atual; acrescenta arquivo e indireção. Existência/aceitação desse arquivo no escopo literal do anexo precisa ser tratada como decisão técnica de implementação, não lei nova. Base: `app/chamados/services/pergunta_no_app.py:61`; restrição de escopo do anexo: **[nao verificado]**. |
| Helper puro que recebe duas contagens | Facilita testar prioridade/cor, mas sozinho não atende o painel sem alguém produzir a query não lidos. Para evitar query própria no painel, precisaria de outro montador, ou da função pública aceitando `colab`. Query atual: `app/api/views_core.py:305`; proibição do anexo: **[nao verificado]**. |
| Função em `api/views_core.py` ou `api/views.py` | Permite extrair a regra no local atual, mas o painel passaria a importar camada HTTP e ainda seria preciso escolher um dos kernels como dono; os dois possuem hoje a regra. `app/api/views_core.py:313`, `app/api/views.py:324`. |

**Forma recomendada do retorno do wrapper**, como proposta compatível: `{'pendentes': [...], 'historico': [...], 'questionarios_pendentes': int, 'chamados_nao_lidos': int, 'chamados_badge': {'count': int, 'cor': str, 'rotulo': str} | None}`. Não calcular novamente rótulo nem count nos leitores; `None` continua representando ausência de badge. Contrato atual dos kernels: `app/api/views_core.py:314`, `app/api/views_core.py:316`, `app/api/views_core.py:318`, `app/api/views_core.py:320`; contrato de listas: `app/chamados/services/pergunta_no_app.py:75`. O nome da função e o shape interno são decisão técnica, não comprovados como já escolhidos: **[nao verificado]**.

**Cuidados concretos com os selos existentes:**

- Matriz E3 testa os dois kernels, dashboard, endpoint, duas perguntas na mesma disputa e prioridade do amarelo sobre vermelho; vermelho conta chamados, não followups. `app/chamados/tests/test_e3_app_orm.py:28`, `app/chamados/tests/test_e3_app_orm.py:38`, `app/chamados/tests/test_e3_app_orm.py:43`, `app/chamados/tests/test_e3_app_orm.py:47`, `app/chamados/tests/test_e3_app_orm.py:62`.
- Mutantes atuais procuram literalmente `len(consulta_do_app(... )['pendentes'])` nos dois kernels; a extração deve atualizar esses alvos, preservando a mordida contra contar disputas. `app/chamados/tests/e3_mutantes.py:23`, `app/chamados/tests/e3_mutantes.py:25`.
- Contrato AST exige que `api_disputa_perguntas` continue lendo `consulta_do_app`, e que a consulta continue delegando a `perguntas_do_app`/C6. Mantendo wrapper separado e endpoints de perguntas intactos, esse contrato pode permanecer. `app/chamados/tests/test_contract_pergunta_viva.py:62`, `app/chamados/tests/test_contract_pergunta_viva.py:68`, `app/chamados/tests/test_contract_pergunta_viva.py:75`.
- A chamada `api/views.py:1004` alimenta o agrupamento por chamado usando pendentes + histórico, depois marca notas do DP como lidas; a chamada `:2542` alimenta perguntas, histórico e `em_disputa`, sem calcular badge. Extração apenas da regra não exige alterá-las. `app/api/views.py:1004`, `app/api/views.py:1040`, `app/api/views.py:1050`, `app/api/views.py:2542`, `app/api/views.py:2571`, `app/api/views.py:2573`, `app/api/views.py:2581`.

**Golden atual:** existem três perguntas por colaborador/dia sobre presença de pergunta no app, ligadas a `dia_do_colab`; não existe nesse arquivo caso do rótulo/count “agora”. `eval/golden.py:35`, `eval/golden.py:184`, `eval/golden.py:187`, `eval/oraculo.py:93`. A inclusão de uma pergunta “agora” é requisito do anexo, não execução nesta cópia: **[nao verificado]**.

**Perguntas de lei: 0**, conforme o anexo que já fixa fonte, prioridade, cores existentes, contagem por pergunta e ausência de badge; a autoridade de vitalidade continua C6. `app/chamados/services/pergunta_no_app.py:61`, `app/api/views_core.py:315`, `app/api/views_core.py:317`, `app/core/juizes.py:278`; aval/“Nenhuma lei nova” do anexo: **[nao verificado]**.

## 11/11 10:08 · sessao 950c845836fc · - · - · origem=Ronald (tmux)

3. View/contexto/contador:

- Única view encontrada renderizando o partial: `painel_colaborador(request, cid)`, protegida por login e ação `ver_cadastro`; rota `<int:cid>/painel/`. (`app/colaboradores/views.py:914`, `app/colaboradores/views.py:917`, `app/colaboradores/views.py:988`, `app/colaboradores/urls.py:48`)
- Mensagens chegam como `chamados`: `ChamadoColaborador.objects.filter(colaborador=colab).prefetch_related('followups').order_by('-criado_em')[:5]`. O comentário diz “abertos”, mas o filtro executável não restringe status. (`app/colaboradores/views.py:936`, `app/colaboradores/views.py:937`)
- A view passa `chamados`; não passa `consulta_do_app`, `pendentes`, `historico`, contagem de perguntas ou rótulo do app. Os outros dados explícitos são colaborador, escala, retrato/portas/legenda, total de batidas, batidas do mês, mês atual e CSRF. (`app/colaboradores/views.py:988`)
- Badge atual da aba Mensagens é `{{ chamados|length }}`, condicionado a haver chamados; portanto conta os até cinco chamados carregados, sem distinguir leitura, perguntas ou status. (`app/templates/colaboradores/partials/painel_colaborador.html:39`, `app/colaboradores/views.py:937`)
- Aba Mensagens contém formulário HTMX de mensagem rápida e include `msgs_lista.html`; a lista mostra assunto/data do chamado e até cinco followups por chamado, com conteúdo truncado em 80 caracteres. (`app/templates/colaboradores/partials/painel_colaborador.html:188`, `app/templates/colaboradores/partials/painel_colaborador.html:198`, `app/templates/colaboradores/partials/msgs_lista.html:4`, `app/templates/colaboradores/partials/msgs_lista.html:7`, `app/templates/colaboradores/partials/msgs_lista.html:10`)
- Após enviar, `abrir_chamado_rapido` cria/reusa chamado `mensagem_dp` em `VIVOS`, anexa nota, envia push e devolve somente `msgs_lista.html`, com a mesma consulta limitada a cinco. O `hx-target="#msgs-lista"` não atualiza o botão da aba. (`app/chamados/views.py:973`, `app/chamados/views.py:976`, `app/chamados/views.py:986`, `app/chamados/views.py:989`, `app/chamados/views.py:997`, `app/chamados/views.py:1000`, `app/templates/colaboradores/partials/painel_colaborador.html:189`)

5. Cascas e grafo:

- Correção da premissa: o GET do painel usa **fetch + innerHTML**, depois `htmx.process(corpo)`; HTMX processa os controles injetados. (`app/templates/core/_drawer_generico.html:19`, `app/templates/core/_drawer_generico.html:26`, `app/templates/core/_drawer_generico.html:33`)
- `base.html → core/_drawer_generico.html` e `base_app.html → core/_drawer_generico.html`; ambas carregam HTMX. (`app/templates/base.html:399`, `app/templates/base_app.html:154`, `app/templates/base.html:19`, `app/templates/base_app.html:19`)
- Fluxo da lista: `colaboradores/lista.html` estende `base.html` e inclui `_lista_tabela.html`; clique da linha chama `abrirPainelColab`, que chama `abrirDrawer(..., '/colaboradores/'+cid+'/painel/', 1100)`. (`app/templates/colaboradores/lista.html:1`, `app/templates/colaboradores/lista.html:57`, `app/templates/colaboradores/partials/_lista_tabela.html:16`, `app/templates/core/_drawer_generico.html:89`, `app/templates/core/_drawer_generico.html:104`)
- Há ramo legado `#col-painel`, também por fetch, com inserção in-place. (`app/templates/core/_drawer_generico.html:95`, `app/templates/core/_drawer_generico.html:99`)
- Fluxo do retrato: `folha/retrato_folha.html` estende `base.html`, inclui `_retrato_lista.html`; o nome abre a mesma rota, levando `mes/ano`. A versão parcial do retrato também inclui a lista. (`app/templates/folha/retrato_folha.html:1`, `app/templates/folha/retrato_folha.html:53`, `app/templates/folha/_retrato_lista.html:61`, `app/templates/folha/_retrato_parcial.html:7`)
- Fluxo Raio-X: barra abre drawer com `raiox_href`; `retrato_raiox` renderiza `_retrato_raiox.html`, cujas linhas abrem a mesma rota do painel com `mes/ano`. (`app/templates/folha/_retrato_barra.html:71`, `app/templates/folha/_retrato_barra.html:73`, `app/folha/views_retrato.py:510`, `app/templates/folha/_retrato_raiox.html:68`, `app/templates/folha/_retrato_raiox.html:70`)
- O partial do painel não estende casca; inclui `_cabecalho_drawer.html`, condicionalmente `_retrato_o_que_fazer.html`, e `msgs_lista.html`. O bloco do retrato inclui `_led.html` e `_retrato_legenda.html`. (`app/templates/colaboradores/partials/painel_colaborador.html:1`, `app/templates/colaboradores/partials/painel_colaborador.html:8`, `app/templates/colaboradores/partials/painel_colaborador.html:26`, `app/templates/colaboradores/partials/painel_colaborador.html:198`, `app/templates/folha/_retrato_o_que_fazer.html:36`, `app/templates/folha/_retrato_o_que_fazer.html:71`)
- Os hosts encontrados do painel usam `base.html` fixo; a infraestrutura de drawer existe nas duas cascas. A seleção global `base_tpl` depende de `modo_app`, mas não altera `{% extends 'base.html' %}` fixo desses hosts. (`app/templates/colaboradores/lista.html:1`, `app/templates/folha/retrato_folha.html:1`, `app/templates/base_app.html:154`, `app/core/context_processors.py:205`, `app/core/context_processors.py:210`)

Selo existente/duas cascas:

- Há testes do painel real via client GET, do bloco do retrato e do cabeçalho compartilhado; não constituem selo API × painel. (`app/colaboradores/tests/test_retrato_no_drawer.py:151`, `app/colaboradores/tests/test_retrato_no_drawer.py:167`, `app/colaboradores/tests/test_retrato_no_drawer.py:182`, `app/ponto/tests/test_drawer_ausencia_no_molde.py:81`)
- A casa já tem padrão de smoke renderizando as duas cascas, com verificação dos templates realmente usados e sessão `via_app/plataforma_sessao`; esse exemplo mede o espelho, não o painel. (`app/core/tests/test_smoke_chromium.py:151`, `app/core/tests/test_smoke_chromium.py:155`, `app/core/tests/test_smoke_chromium.py:160`, `app/core/tests/test_smoke_chromium.py:172`)
- Para o construtor: renderizar somente a resposta de `/colaboradores/<cid>/painel/` duas vezes não demonstra duas cascas, pois é partial sem extends; o selo deve medir a integração com cada casca que inclui o drawer. (`app/colaboradores/views.py:988`, `app/templates/colaboradores/partials/painel_colaborador.html:1`, `app/templates/base.html:399`, `app/templates/base_app.html:154`)

## 11/11 10:05 · sessao f575c8ee2b9b · O169-FECHADA · verificador · origem=Code

**A volta 5 ainda não cumpre integralmente o aval:** o ramo trancado fica parado sem relavrar a via, e o apply usa uma instância carregada antes da trava. O aval exige relavra no primeiro caso; a função apenas pula essas linhas. (`app/docs/CORTES.md:435`; `app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:264`; `app/ponto/services/cluster_espurio.py:267`)

1. **Escritores e leitores dos campos.**

   | Campo/valor | Quem escreve | Quem lê |
   |---|---|---|
   | `status_local` | O caminho novo é tratamento → `_renascer_por_premissa_viva` → `reabrir` → `_gravar_estado`. O núcleo atribui e salva; `models.py` é porta declarada. (`app/ponto/services/cluster_espurio.py:273`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:296`; `app/core/portas.py:218`) | Contador seleciona encerrados; relatório mostra o estado; painel conta estados; vigia compara estado persistido com auditoria. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:251`; `app/chamados/services/painel.py:213`; `app/chamados/services/vigia_loop.py:117`) |
   | `via_encerramento` | Mesmo núcleo: terminal recebe a via; renascimento limpa para `''`. (`app/chamados/models.py:245`; `app/chamados/models.py:249`) | Contador seleciona `RECONCILIAR`; renascimento recusa cascata; desarquivamento e reabertura de competência distinguem a via de tranca. (`app/ponto/services/cluster_espurio.py:112`; `app/chamados/models.py:1007`; `app/chamados/views_arquivar.py:64`; `app/chamados/services/competencia_trancada.py:259`) |
   | `resolvido_em` | `_gravar_estado` carimba encerramento, preserva encerrado→encerrado e limpa no renascimento. (`app/chamados/models.py:242`; `app/chamados/models.py:248`) | Guarda contra desfazer decisão administrativa sem fato novo; estatísticas de resolvidos. (`app/chamados/models.py:1342`; `app/chamados/services/fila.py:109`) |
   | Followup/nota | Renascimento chama `nota.anexar_ocorrencia`, porta declarada de `FollowupChamado`, com tipo `renascido`. (`app/chamados/models.py:1021`; `app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`) | Histórico lê conteúdo e autor; detalhe/painel também consomem followups. (`app/chamados/services/historico.py:80`; `app/chamados/views.py:352`; `app/chamados/services/painel.py:362`) |
   | `LogAuditoria`: `estado_chamado` e ação nova | Núcleo emite `estado_chamado`; tratamento emite `reabrir_legado_cluster_o169`; ambos usam o escritor `core.observ.evento`. (`app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:218`; `app/ponto/services/cluster_espurio.py:277`; `app/core/observ.py:94`; `app/core/observ.py:134`) | Histórico e vigia leem `estado_chamado`. O teste lê especificamente a ação nova; o histórico filtra apenas suas ações declaradas. (`app/chamados/services/historico.py:44`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`) |
   | `CelulaDia.ata` | Cartório produz a ata; persistência passa pela porta da célula, inclusive lavra e precedência. O tratamento novo apenas lê. (`app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1033`; `app/ponto/portas/celula.py:1076`; `app/core/portas.py:112`) | `_leitura_da_celula`, compartilhada pelo contador, abertura e fechamento, lê órfãs/lâmpadas. (`app/ponto/services/cluster_espurio.py:27`; `app/ponto/services/cluster_espurio.py:47`; `app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:126`) |
   | `PeriodoFechado` | `trancar_periodo` e `reabrir_periodo`, no módulo declarado. O diff não o escreve. (`app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1437`; `app/core/portas.py:501`) | A casca do chamado consulta `competencia_trancada`; C20 consulta `periodo_trancado`. Ambos reconhecem a tranca por `reaberto_em IS NULL`; `ativo` é documental. (`app/chamados/models.py:1044`; `app/ponto/services/fechamento.py:1464`; `app/ponto/services/fechamento.py:1480`) |

   **Não há segundo escritor introduzido pelo diff para esses estados:** as alterações passam pelos escritores acima. O teste que escreve diretamente `CelulaDia.ata` é preparação de fixture, não o tratamento produtivo. (`app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:56`)

   Os valores persistidos são `RECONCILIAR → 'sistema'` e tranca → `'competencia_trancada'`; `relavrar_via_tranca` é ação do relatório. (`app/chamados/catalogo/motor.py:88`; `app/chamados/catalogo/motor.py:92`; `app/chamados/catalogo/motor.py:95`; `app/ponto/services/cluster_espurio.py:219`)

2. **Juízes e derivações.**

   A função nova não implementa outro predicado de assinatura, universo, dia, competência ou recusa: usa contador, `dia_do_fato`, casca de tranca e `recusa_da_reabertura`. A porta continua responsável pelo veto e demais guardas. (`app/ponto/services/cluster_espurio.py:245`; `app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:251`; `app/ponto/services/cluster_espurio.py:254`; `app/chamados/models.py:1015`)

   **Há duas semânticas preexistentes que precisam permanecer explícitas:** C20, registrado como “dia trancado”, desconta toque humano posterior; `_dia_em_competencia_trancada`, usada pelo relatório e renascimento, consulta a competência trancada sem esse desconto. Portanto a coluna não significa exatamente o C20 de `JUIZES.md`. (`app/docs/JUIZES.md:86`; `app/core/juizes.py:292`; `app/chamados/juizes.py:1033`; `app/chamados/juizes.py:1041`; `app/chamados/models.py:1038`)

   Também pode haver diferença entre **o dia cuja assinatura foi contada** e **a competência usada na tranca**: o primeiro usa `dia_do_fato(fortes=True)`; `competencia_do_chamado` escolhe o fato mais recente, podendo incluir ausência e perguntas vivas. Isso é reaproveitamento de autoridade existente, mas pode classificar outra competência daquela sugerida pela coluna `dia`. (`app/ponto/services/cluster_espurio.py:114`; `app/ponto/services/cluster_espurio.py:251`; `app/chamados/juizes.py:2275`; `app/chamados/juizes.py:2283`; `app/chamados/juizes.py:2290`)

3. **Relógio: existe leitura decisória indireta fora da borda.**

   Não há `now/today` novo diretamente no tratamento. Contudo, inclusive no DRY, há caminho alcançável: tranca → competência → perguntas vivas → C6 sem `hoje` → `_dentro_da_janela_viva` → `date.today()`. A consulta de perguntas não restringe o módulo do chamado. (`app/ponto/services/cluster_espurio.py:249`; `app/chamados/models.py:1043`; `app/chamados/juizes.py:2286`; `app/chamados/juizes.py:2264`; `app/chamados/juizes.py:2267`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:577`)

   O apply também pode alcançar esse caminho ao ressuscitar perguntas. Os carimbos automáticos de persistência são outra coisa, preexistente. A ocorrência dessas configurações nos três chamados de produção é **[nao verificado]**. (`app/chamados/models.py:1451`; `app/chamados/models.py:1509`; `app/chamados/models.py:68`; `app/chamados/models.py:1674`)

4. **Travas, deadlock e TOCTOU.**

   - `abrir`: `atomic → trava C → leitura → renascer`. O novo tratamento: `atomic → leitura de chamado+colaborador → trava C → contador → renascer`. Portanto não são sequências equivalentes: o objeto usado pela porta nasceu antes da trava. O contrato de `trava_na_ordem` manda reler depois. (`app/chamados/models.py:902`; `app/chamados/models.py:905`; `app/chamados/models.py:906`; `app/chamados/models.py:922`; `app/ponto/services/cluster_espurio.py:266`; `app/ponto/services/cluster_espurio.py:267`; `app/chamados/models.py:2638`)
   - `resposta_tardia` também não possui a sequência proposta: lê estado/célula e chama renascimento diretamente; o escritor adquire as travas posteriormente. (`app/chamados/services/resposta_tardia.py:35`; `app/chamados/services/resposta_tardia.py:44`; `app/chamados/services/resposta_tardia.py:46`; `app/chamados/models.py:290`)
   - **TOCTOU concreto:** enquanto espera C, o colaborador pode passar de ativo para desligado. A releitura do contador não filtra situação nem substitui o colaborador em cache; o renascimento pode continuar vendo `ativo`. `reabrir` só atualiza determinados campos quando detecta mudança de status, sem renovar esse colaborador. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:267`; `app/ponto/services/cluster_espurio.py:270`; `app/chamados/models.py:1005`; `app/chamados/models.py:1373`)
   - `--esperados` valida somente a fotografia inicial. Sob trava verifica pertinência individual, não igualdade do universo inteiro; entradas desaparecidas são puladas e novas entradas não abortam. Cada linha tem sua transação, portanto falha posterior pode deixar aplicações anteriores commitadas. (`app/ponto/services/cluster_espurio.py:259`; `app/ponto/services/cluster_espurio.py:263`; `app/ponto/services/cluster_espurio.py:266`; `app/ponto/services/cluster_espurio.py:270`)
   - Não encontrei inversão nova demonstrada nesse corpo: a leitura anterior é comum, e o núcleo adquire C→D→Chamado. Ausência universal de deadlock é **[nao verificado]**. A trava C também não torna atômica a leitura de `PeriodoFechado`, escrito antes dos ganchos que tratam chamados. (`app/ponto/services/cluster_espurio.py:267`; `app/chamados/models.py:290`; `app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1428`)

5. **Campos com dois significados.**

   - **`pks`/`numero`: coerentes nos leitores encontrados.** `numero=len(pks)`; tratamento usa os PKs e alarme usa o número. `universo.total` possui outro denominador: todos os encerrados pela via selecionada, antes de exigir assinatura. (`app/ponto/services/cluster_espurio.py:126`; `app/ponto/services/cluster_espurio.py:127`; `app/ponto/services/cluster_espurio.py:149`; `app/ponto/services/cluster_espurio.py:246`; `app/ponto/management/commands/detectar_cluster_espurio.py:100`)
   - **`trancada`: mesma função da guarda de renascimento, em dados estáveis.** Pode ficar temporalmente velha, pois define a ação antes do apply; linhas inicialmente trancadas sequer são reavaliadas dentro dele. (`app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:264`; `app/chamados/models.py:1009`)
   - **`aplicado/aplicadas` contam retornos verdadeiros do renascimento**, não tentativas nem todas as escritas. Uma recusa pode escrever auditoria; a recusa por irmão vivo pode migrar perguntas e anexar nota, retornando `False`. (`app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:1017`; `app/chamados/models.py:1415`; `app/chamados/models.py:1427`; `app/chamados/models.py:1436`)
   - **`recusa` é incompleta e pode ficar desatualizada.** Consulta só guardas de `reabrir`; desligado/cascata/tranca são guardas adicionais de renascimento. Após `False`, o tratamento não atualiza o motivo. Assim, desligado pode sair `ativo=False`, `recusa=None`, `aplicado=False`. (`app/ponto/services/cluster_espurio.py:254`; `app/ponto/services/cluster_espurio.py:255`; `app/ponto/services/cluster_espurio.py:274`; `app/chamados/models.py:1005`; `app/chamados/models.py:1007`)

6. **Selos e mutantes.**

   Os dois harnesses exigem `errors == []` e alguma falha de asserção; erro de infraestrutura registrado em `errors` não é aceito como morte, e execução sem falha também não passa. O legado acrescenta controle da fonte intacta. Contudo, aceitam qualquer asserção falhada no caso, inclusive de fixture, sem identificar qual asserção matou. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:286`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:287`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:143`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:144`)

   As mordidas dos oito mutantes do legado são concretas:

   | Mutante | Asserção |
   |---|---|
   | `fechar_por_idade` | Chamado continua VIVO após cron. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175`) |
   | `relavrar_via_em_nao_trancada` | `aplicadas == 1`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:109`) |
   | `reabre_trancada` | Ação e classificação devem ser `relavrar_via_tranca, True`. **Morde o relatório; a porta real ainda barraria reabrir.** (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:132`; `app/chamados/models.py:1009`) |
   | `reabrir_fora_da_porta` | Exige mais uma linha `estado_chamado`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:113`) |
   | `apply_sem_esperados` | Exige aborto para expectativa ausente/diferente. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`) |
   | `dry_escreve` | Exige linha não aplicada, contador zero e foto intacta. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:95`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:98`) |
   | `universo_proprio` | Só o chamado assinado pode entrar. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:154`) |
   | `forca_desligado` | Exige não aplicação e foto intacta. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:164`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:166`) |

   Os 36 mutantes do contador/comando também têm asserções correspondentes:

   | Mutante(s) | Asserção |
   |---|---|
   | `contador_cego`, `recorta_por_idade` | `numero == 1`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:173`) |
   | `pks_divergem_do_numero` | `pks == [1]`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:174`) |
   | `assinatura_invertida` | Sem assinatura conta zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:179`) |
   | `nao_medido_vira_assinatura` | Não medido conta zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:333`) |
   | `sem_filtro_via` | Vias excluídas contam zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:201`) |
   | `via_digitada` | Constante substituída por via sintética continua contando um. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:323`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:326`) |
   | `status_errado`, `sem_filtro_modulo` | Conta exatamente `len(ENCERRADOS)`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:209`) |
   | `sem_filtro_colab` | Recorte do colaborador conta dois. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:352`) |
   | `conta_dias_em_vez_de_chamados` | Três chamados contam três. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:351`) |
   | `mistura_colaboradores` | Assinatura do outro colaborador não serve. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:340`) |
   | `legado_nao_contado`, `vazia_nao_contada`, `nula_nao_contada`, `via_nova_nao_contada` | Igualdade integral de `fora_do_universo`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:225`) |
   | `rotulo_desconhecido_vazio` | Rótulos precisam estar preenchidos. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:232`) |
   | `fora_sem_filtro_colab` | Exclusões respeitam colaborador. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:257`) |
   | `sem_guarda_colaborador_fk` | Assinatura vem do colaborador correto. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:305`) |
   | `consulta_por_par` | Exige `k` consultas. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:319`) |
   | `troca_rotulos_humana_cascata`, `cascata_omite_ajuste_retroativo`, `cancelado_rotulado_desconhecido`, `superado_rotulado_desconhecido`, `legado_rotulado_desconhecido`, `vazia_rotulada_desconhecida`, `nula_rotulada_desconhecida` | Igualdade integral dos rótulos. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:250`) |
   | `produto_cartesiano` | Só células dos pares pedidos são lidas. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:273`) |
   | `recarrega_fk`, `reconsulta_par_da_fk` | Consulta somente o par faltante. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:284`) |
   | `esconde_universo_vazio` | Saída precisa declarar universo vazio. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:377`) |
   | `zero_vira_universo_vazio` | Zero achados não pode anunciar universo vazio. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:385`) |
   | `silencia_contador` | Saída contém o número medido. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:401`) |
   | `alarme_ignora_contador`, `apply_vence_alarme` | Exige `SystemExit`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:391`) |
   | `alarme_so_maior` | Divergência para menos também exige alarme. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:419`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:422`) |

   **Lacunas relevantes:** remover a trava/releitura; usar colaborador desatualizado; trocar a classificação de tranca por uma que desconta toque humano; perder assinatura ou mudar tranca entre medida/apply; falhar após o primeiro PK; ignorar `--colab` no caminho legado; adulterar `recusa`, ator ou motivo da auditoria; tornar a comparação de esperados dependente da ordem. Os cenários novos não instrumentam concorrência, o teste de comando prepara um colaborador, as verificações da trilha são parciais e os applies felizes usam um PK. (`app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:259`; `app/ponto/services/cluster_espurio.py:268`; `app/ponto/services/cluster_espurio.py:270`; `app/ponto/management/commands/detectar_cluster_espurio.py:55`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:106`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:158`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:184`)

   A execução da suíte Django do legado permanece **[nao verificado]**; as mordidas acima são demonstradas pela estrutura dos testes, não por execução contra banco. [nao verificado]

7. **Existe porta que troca via de chamado já encerrado sem reabrir? Sim, em sentido geral.**

   `transicionar('fechado', user)` permite `resolvido → fechado`, passa por `_aplicar` e grava via `admin`; ambos os estados pertencem a `ENCERRADOS`, sem passagem por vivo. O núcleo preserva `resolvido_em`. Portanto a frase absoluta de `PAROU_SEM_PORTA` é excessiva. (`app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:450`; `app/chamados/models.py:242`; `app/ponto/services/cluster_espurio.py:220`)

   **Isso não entrega a operação exigida pelo aval:** relavrar somente a via para `competencia_trancada`, preservando o encerramento. O gate dedicado recusa fora de `VIVOS`; o núcleo tecnicamente aceita escrever via terminal, mas não há no caminho examinado um gate específico dessa relavra. (`app/chamados/models.py:1630`; `app/chamados/models.py:1639`; `app/chamados/models.py:240`; `app/chamados/models.py:245`)

8. **Outros defeitos materiais.**

   - **Trilha obrigatória não é garantida.** `evento` é best-effort, isola e engole falhas, retornando `None`; o tratamento ignora o retorno e marca aplicação. Pode haver chamado reaberto sem a auditoria nova; `estado_chamado` usa o mesmo emissor. (`app/core/observ.py:98`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:292`)
   - **“Nada se escreve” não é universal no apply parado/vazio.** `get_user_sistema()` roda antes de pular trancada

[truncado em 20000 de 21272 caracteres]

## 11/11 10:04 · sessao 0693a9df1757 · FIO-E0-V4-E4B · construtor · origem=Code

O leitor **já existia nesta base**; sua ausência não explica o log fornecido. Causa histórica do `OK`: **[nao verificado]**. [test_e4_backfill.py:209](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:209)

- Alinhei a fonte a `fonte_real`, preservando tupla única, unicidade e patch condicional. [test_e4_backfill.py:212](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:212)
- Acrescentei selo que exige a assertion de entrega, sem `precondicao:`. [protocolo.py:69](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm_protocolo.py:69)
- Memória: protocolo **12 OK**, c4v7 **19 OK**; três mutantes novos caíram por assertion, `rc=1`, sem errors. [recibo:50](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:50)

Arquivos, comandos com rc e comandos GREEN/`--orm` para a sessão estão no [recibo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:110). ORM **[nao verificado]**.

LEI-AKITA com `padrao=n/a (instrumento)` registrada. Sem commit, pela proibição inicial. [recibo:130](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:130) [nao verificado]

## 11/11 10:02 · sessao e62921812cba · FIO-E0-V4-E3 · sonda · origem=Code

[nao verificado]
```text
sonda_leitura: sonda_col1005_base.py [94d2db28b6538739] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

PERG 56283 mat=sim via=celula_absolveu cel_ver=concorde data=2026-11-10
PERG 56384 mat=sim via=celula_absolveu cel_ver=concorde data=2026-11-10
GERADOR_BASE col1005 []
gerador.n n 0
```

## 11/11 10:02 · sessao 021c9279df7a · - · - · origem=Ronald (tmux)

Correção da conclusão sobre relógio: **não cabe afirmar ausência de leitura indireta fora da borda**. O tratamento não lê relógio diretamente, mas pode alcançar `date.today()` em um juiz herdado — `app/ponto/services/cluster_espurio.py:249`, `app/chamados/juizes.py:577`.

Cadeia verificável, inclusive no DRY: `_dia_em_competencia_trancada → competencia_do_chamado → _dias_das_perguntas_vivas → pergunta_viva(p)` sem `hoje → _pergunta_viva_calc → _dentro_da_janela_viva → date.today()` — `app/chamados/models.py:1043`, `app/chamados/juizes.py:2286`, `app/chamados/juizes.py:2264`, `app/chamados/juizes.py:2267`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:856`, `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:577`.

A consulta de perguntas não restringe módulo, portanto alcança chamado de cluster que tenha disputa e perguntas correspondentes; o relógio é lido se a pergunta ultrapassar os retornos anteriores do C6 — `app/chamados/juizes.py:2264`, `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1329`, `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1389`. Existência dessa configuração nos chamados de produção: [nao verificado].

Outra cadeia possível no apply é `reabrir → ressuscitar_perguntas → _universo_da_ressuscitacao → pergunta_viva(_q)` sem `hoje` — `app/chamados/models.py:1451`, `app/chamados/models.py:1543`, `app/chamados/models.py:1509`.

Sem perguntas relacionadas, os caminhos diretos de agrupamento/irmão não mostram relógio; o veto retorna antes do juiz para esse módulo, porque cluster não pertence ao universo declarado — `app/chamados/models.py:815`, `app/chamados/models.py:822`, `app/chamados/juizes.py:2133`, `app/chamados/juizes.py:2137`, `app/chamados/models.py:1137`, `app/ponto/services/furos_vetados.py:79`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`.

Os carimbos `auto_now/auto_now_add` devem ser citados separadamente como persistência preexistente, não confundidos com o relógio decisório do C6 — `app/chamados/models.py:68`, `app/chamados/models.py:1674`.

## 11/11 10:02 · sessao a47458d8e632 · - · - · origem=Ronald (tmux)

A tabela cobre os 36 nomes declarados; todas as linhas abaixo apontam para as asserções em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py`. Os pares nome→teste são declarados em `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`.

| Mutante(s) | Teste / asserção que morde | [nao verificado]
|---|---|
| `contador_cego`, `recorta_por_idade` | `test_RED_resolvido_reconciliar_com_assinatura_conta_um`: `numero == 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:173`. |
| `pks_divergem_do_numero` | Mesmo teste: `pks == [1]` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:174`. |
| `assinatura_invertida` | `test_mesmo_encerramento_sem_assinatura_conta_zero`: `numero == 0` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:179`. |
| `nao_medido_vira_assinatura` | `test_sem_dia_sem_celula_e_ata_nao_medida_nao_sao_assinatura`: `numero == 0` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:333`. |
| `sem_filtro_via` | `test_tres_vias_excluidas_mesmo_com_assinatura`: `numero == 0` para cada via excluída — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:201`. |
| `via_digitada` | `test_via_vem_da_constante_do_evento`: constante alterada para via sintética, `numero == 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:323`, `:326`. |
| `status_errado`, `sem_filtro_modulo` | `test_filtro_exige_modulo_encerrados_e_via`: `numero == len(ENCERRADOS)` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:209`. |
| `sem_filtro_colab` | `test_conta_chamados_e_respeita_colab_opcional`: filtro colab 1 deve contar 2 — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:352`. |
| `conta_dias_em_vez_de_chamados` | Mesmo teste: três chamados devem contar 3, apesar dos pares repetidos — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:351`. |
| `mistura_colaboradores` | `test_mesmo_dia_de_outro_colaborador_nao_prova_assinatura`: `numero == 1`; também exige 2 após assinar ambos — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:340`, `:343`. |
| `legado_nao_contado`, `vazia_nao_contada`, `nula_nao_contada`, `via_nova_nao_contada` | `test_RED_toda_via_fora_do_universo_contada_e_rotulada`: dicionário completo `fora_do_universo == esperado` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:225`. |
| `rotulo_desconhecido_vazio` | Mesmo teste: rótulo de cada via deve ser verdadeiro/preenchido — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:232`. |
| `fora_sem_filtro_colab` | `test_fora_do_universo_respeita_colab_e_universo_vazio`: `{'legado': 1}` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:257`. |
| `sem_guarda_colaborador_fk` | `test_RED_fk_cruzada_busca_colaborador_do_chamado_na_data_da_fk`: `numero == int(assinatura_correta)` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:305`. |
| `consulta_por_par` | `test_RED_k_colaboradores_varios_pares_faltantes_k_consultas`: número de consultas deve ser `k`, não número de pares — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:319`. |
| `troca_rotulos_humana_cascata`, `cascata_omite_ajuste_retroativo`, `cancelado_rotulado_desconhecido`, `superado_rotulado_desconhecido`, `legado_rotulado_desconhecido`, `vazia_rotulada_desconhecida`, `nula_rotulada_desconhecida` | `test_RED_rotulo_fiel_por_via_exata`: igualdade integral do dicionário `excluidos` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:250`. |
| `produto_cartesiano` | `test_RED_celulas_lidas_so_dos_pares_pedidos`: só duas células lidas — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:273`. |
| `recarrega_fk`, `reconsulta_par_da_fk` | `test_RED_reaproveita_fk_carregada_sem_buscar_o_mesmo_par`: lista de células consultadas exatamente `[(2, DIA)]` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:284`. |
| `esconde_universo_vazio` | `test_universo_vazio_imprime_sempre_inclusive_apply_e_alarme`: saída contém `universo vazio` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:377`. |
| `zero_vira_universo_vazio` | `test_zero_achados_com_universo_preenchido_nao_diz_vazio`: saída não contém `universo vazio` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:385`. |
| `silencia_contador` | `test_dry_informa_um_sem_sair_e_sem_depender_de_escala_ativa`: saída contém `"numero": 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:401`. |
| `alarme_ignora_contador`, `apply_vence_alarme` | `test_RED_alarme_com_um_sai_dois_sem_escrever_mesmo_com_apply`: precisa levantar `SystemExit` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:391`. |
| `alarme_so_maior` | `test_alarme_compara_numero_com_esperado`: cenário `numero=0, esperado=1` precisa levantar `SystemExit` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:419`, `:422`. |

**Infraestrutura/vacuidade:** não são aceitas como morte pelo harness: alvo deve existir uma única vez, o caso roda, `errors` precisa ser vazio e `failures` precisa ser não vazio. Portanto erro de infraestrutura faz o selo ficar vermelho; ausência de falha também. O harness não distingue qual asserção falhou, mas os alvos acima têm asserções diretas sobre o comportamento mutado. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:137`, `:142`, `:143`, `:144`.)

## 11/11 10:01 · sessao a47458d8e632 · - · - · origem=Ronald (tmux)

**Selos/mutantes — conclusão estática:** os dois harnesses recusam erro de infraestrutura e exigem `resultado.failures`; uma exceção de import/sintaxe/banco não conta como mutante morto. O legado também roda controle da fonte intacta. Não há caminho explícito de sucesso com zero falhas nos mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:278`, `:286`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:137`, `:143`, `:144`.)

Os **oito mutantes do legado** têm mordidas concretas:

| Mutante | Asserção que o mata |
|---|---|
| `fechar_por_idade` | O chamado de 20 dias deve continuar VIVO após cron (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175`). |
| `relavrar_via_em_nao_trancada` | `aplicadas == 1`, depois status VIVO (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:109`). |
| `reabre_trancada` | O relatório precisa trazer `relavrar_via_tranca, True` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:132`). **Aqui a mordida é na classificação do relatório; a porta real continua recusando a reabertura** (`app/chamados/models.py:1009`). |
| `reabrir_fora_da_porta` | Exatamente mais uma linha `estado_chamado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:113`). O `QuerySet.update` do mutante pula `_gravar_estado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:221`). |
| `apply_sem_esperados` | `abortado` verdadeiro para `None`, vazio e conjunto diferente (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:142`, `:145`). |
| `dry_escreve` | Linha deve ficar `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:95`, `:97`, `:98`). |
| `universo_proprio` | Só o chamado com assinatura entra nos PKs/linhas (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:150`, `:154`, `:155`). |
| `forca_desligado` | `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:164`, `:165`, `:166`). |

**Execução:** rodei apenas os dois módulos puros `test_o169_cluster_encerrado_sem_lastro` e `test_o169_cluster_encerrado_mutantes`, com bytecode desabilitado: 62 testes, OK, incluindo todos os 36 mutantes. Nenhum arquivo de resultado foi criado; a execução não tem evidência arquivo:linha própria [nao verificado]. O contrato declara não usar Django/banco/rede/temporários e executa fontes em namespaces isolados (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:1`, `:31`, `:103`). A suite Django do legado não foi executada [nao verificado].

**Limites reais dos selos:**

- O harness aceita **qualquer** `AssertionError` no caso alvo, inclusive de fixture; não exige uma asserção específica. No legado, controle intacto reduz esse risco; no contador, cada caso normal também existe, mas não há controle intacto dentro da classe de mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:65`, `:72`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:133`, `:148`.)
- O comentário “cada caso abre seu savepoint (`_fixture_setup`)" não corresponde ao mecanismo visível: chama diretamente `.run(resultado)`, dentro de `transaction.atomic()` próprio e rollback `_Desfaz`. O isolamento visível é esse atomic externo; não há chamada explícita `_fixture_setup`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:245`, `:258`, `:260`, `:261`.)
- As notas só são verificadas por **contagem maior**, sem conteúdo/tipo/exatamente uma; a trilha extra só confere via anterior e status posterior, sem ator/motivo completos. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:116`, `:117`, `:118`.)

**Mutações relevantes do diff sem mordida nos testes perguntados:** [nao verificado]

1. Remover/mover a trava ou remover a releitura do contador: todos os casos são sequenciais, sem escritor concorrente/instrumentação de ordem. (`app/ponto/services/cluster_espurio.py:266`, `:268`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:89` a `:205`.)
2. Remover `colaborador_id=o['colab']` do comando legado: o teste de comando prepara apenas um colaborador; a verificação existente do filtro no contrato puro passa pelo caminho normal, sem `legado`. (`app/ponto/management/commands/detectar_cluster_espurio.py:55`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:184`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:164`, `:404`.)
3. Trocar `recusa` por `None` sempre, ou não atualizar a recusa após falha da porta: não há asserção sobre essa chave, inclusive no desligado. (`app/ponto/services/cluster_espurio.py:254`, `:274`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:158`.)
4. Alterar ator/motivo da auditoria ou texto/tipo da nota mantendo contagem: os testes perguntados não mordem esses valores. (`app/ponto/services/cluster_espurio.py:277`, `:281`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:118`.)
5. Trocar aceitação de conjuntos esperados por dependência da ordem: não há APPLY feliz com dois ou mais PKs, em ordem invertida/duplicados. (`app/ponto/services/cluster_espurio.py:259`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:106`, `:131`, `:142`.)
6. Não há cenário de tranca surgindo/desaparecendo entre medição e apply, perda de assinatura no intervalo, ou erro depois de primeiro PK de lote: os cenários perguntados usam dados estáveis e APPLY por um PK. (`app/ponto/services/cluster_espurio.py:249`, `:263`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`, `:127`.)

**Outro defeito concreto:** APPLY com todos os itens trancados, com porta recusando tudo, ou mesmo `esperados=[]`/universo vazio ainda chama `get_user_sistema()` antes do loop; esse helper faz `get_or_create` e pode gravar User se faltar. Logo “NADA se escreve” não é universal. `_foto` não olha User e não captura isso. (`app/ponto/services/cluster_espurio.py:262`, `:264`; `app/core/utils.py:18`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`.)

## 11/11 10:00 · sessao 021c9279df7a · - · - · origem=Ronald (tmux)

2. **Sem juiz novo no corpo do diff:** universo vem de `contador_cluster_encerrado_sem_lastro()['pks']`; assinatura continua em `_leitura_da_celula`; dia usa `dia_do_fato`; tranca usa a mesma casca de `renascer`; recusa delega a `recusa_da_reabertura` — `app/ponto/services/cluster_espurio.py:245`, `app/ponto/services/cluster_espurio.py:249`, `app/ponto/services/cluster_espurio.py:251`, `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:126`.

Há uma diferença **preexistente, agora exposta pelo relatório**: `_dia_em_competencia_trancada` usa a competência do chamado e a existência de `PeriodoFechado` vivo; não desconta toque do admin — `app/chamados/models.py:1038`, `app/chamados/models.py:1044`. C20, declarado como autoridade da tranca do dia, desconta explicitamente toque humano posterior — `app/core/juizes.py:156`, `app/core/juizes.py:161`, `app/chamados/juizes.py:1033`, `app/chamados/juizes.py:1041`. Não chamar essa casca de C20 sem distinguir as perguntas.

Outra diferença relevante: o dia mostrado/medido vem de `dia_do_fato(fortes=True)`, mas a competência usada na tranca vem do fato **mais recente**, incluindo ausência e perguntas vivas; a classificação pode, portanto, tratar outra competência que a do dia cuja assinatura foi contada — `app/ponto/services/cluster_espurio.py:114`, `app/ponto/services/cluster_espurio.py:251`, `app/chamados/models.py:1043`, `app/chamados/juizes.py:2275`, `app/chamados/juizes.py:2283`, `app/chamados/juizes.py:2290`.

3. **Nenhuma leitura explícita nova de relógio** no contador ou tratamento; o comando normal lê `timezone.localdate()` na borda, e `--legado` retorna antes dela — `app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:224`, `app/ponto/management/commands/detectar_cluster_espurio.py:67`, `app/ponto/management/commands/detectar_cluster_espurio.py:70`. Os carimbos automáticos de nota/atualização são leituras de relógio de persistência preexistentes, não uma decisão nova por idade — `app/chamados/models.py:68`, `app/chamados/models.py:1674`. Não confundir `timezone.localdate(b.timestamp)` com leitura do relógio atual: ela converte um instante dado — `app/chamados/juizes.py:53`.

4. **Ordem de trava:** `abrir` faz `atomic → trava do colaborador → primeira leitura → renascer` — `app/chamados/models.py:902`, `app/chamados/models.py:905`, `app/chamados/models.py:906`, `app/chamados/models.py:922`. O novo tratamento faz `atomic → leitura de ch+colaborador → trava → contador → renascer`; a primeira leitura ocorre antes da trava — `app/ponto/services/cluster_espurio.py:266`, `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:268`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:273`. Isso viola a orientação explícita de reler a instância depois da aquisição — `app/chamados/models.py:2638`.

`resposta_tardia` não tem essa sequência: consulta estado e célula e chama diretamente o renascimento; a trava vem posteriormente, quando o escritor grava estado — `app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:46`, `app/chamados/models.py:280`, `app/chamados/models.py:290`.

Não identifiquei uma inversão nova comprovada: antes de adquirir C, o novo corpo só faz leitura comum; o escritor adquire C→D→Ch e a porta de ressuscitação C→D→Ch→P — `app/ponto/services/cluster_espurio.py:267`, `app/chamados/models.py:289`, `app/chamados/models.py:1562`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`. Ausência universal de deadlock é [nao verificado].

**TOCTOU concreto:** a releitura do contador não atualiza `ch` nem seu colaborador já carregado. Se outro ato alterar `situacao`, contexto ou via enquanto espera a trava, `renascer` pode usar o objeto anterior; `reabrir` só refresca campos quando detecta mudança de status — `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:270`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1373`, `app/chamados/models.py:1375`.

`esperados` compara apenas a fotografia inicial; sob a trava só verifica se cada pk ainda pertence ao contador, sem conferir igualdade de conjuntos. Novos candidatos não abortam o lote; candidatos desaparecidos são pulados; a classificação inicial `trancada` também decide quais linhas sequer entram no apply — `app/ponto/services/cluster_espurio.py:259`, `app/ponto/services/cluster_espurio.py:264`, `app/ponto/services/cluster_espurio.py:270`. Não há transação do lote: falha posterior deixa anteriores já commitados — `app/ponto/services/cluster_espurio.py:263`, `app/ponto/services/cluster_espurio.py:266`.

A trava C não serializa necessariamente a mudança de `PeriodoFechado`: `trancar_periodo` grava a tranca antes do gancho que trata os chamados, e `reabrir_periodo` também grava antes do gancho — `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1419`, `app/ponto/services/fechamento.py:1428`, `app/ponto/services/fechamento.py:1441`, `app/ponto/services/fechamento.py:1448`.

5. **`pks` e `numero`: coerentes na implementação:** `pks` enumera chamados com assinatura e `numero=len(pks)`; o alarme compara `numero` ao esperado e o tratamento usa os pks. `universo.total` é outro denominador, anterior ao filtro da assinatura — `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:127`, `app/ponto/services/cluster_espurio.py:149`, `app/ponto/services/cluster_espurio.py:246`, `app/ponto/management/commands/detectar_cluster_espurio.py:100`.

`trancada` significa a mesma chamada que a guarda de `renascer`, em estado estável; pode divergir temporalmente porque a linha foi montada antes do apply — `app/ponto/services/cluster_espurio.py:249`, `app/chamados/models.py:1009`, `app/ponto/services/cluster_espurio.py:263`.

`aplicado/aplicadas` contam somente retorno verdadeiro do renascimento, não efeitos totais ou tentativas — `app/ponto/services/cluster_espurio.py:273`, `app/ponto/services/cluster_espurio.py:274`, `app/ponto/services/cluster_espurio.py:282`. Porta recusada pode mesmo assim escrever trilha de veto ou migrar perguntas/anexar nota e retornar falso — `app/chamados/models.py:1017`, `app/chamados/models.py:1415`, `app/chamados/models.py:1427`, `app/chamados/models.py:1436`.

**`recusa` é incompleta:** relatório consulta só as guardas de `reabrir`; desligado/cascade/tranca pertencem às guardas adicionais de `renascer`; após retorno falso nada atualiza o motivo. Assim desligado pode aparecer `ativo=False`, `recusa=None`, `aplicado=False` — `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:255`, `app/ponto/services/cluster_espurio.py:274`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1009`.

Outros defeitos concretos:

- `get_user_sistema()` roda antes de pular linhas trancadas e fora dos atomics; usa `get_or_create`. Até lote vazio ou totalmente parado pode criar `auth_user` se o sistema faltar — `app/ponto/services/cluster_espurio.py:262`, `app/ponto/services/cluster_espurio.py:264`, `app/core/utils.py:18`.
- A nova trilha obrigatória usa escritor best-effort, cujo retorno pode ser `None`; o tratamento ignora o retorno e conta aplicação mesmo sem a trilha persistida — `app/core/observ.py:96`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/ponto/services/cluster_espurio.py:277`, `app/ponto/services/cluster_espurio.py:282`.
- Descrição da auditoria afirma “encerrado por idade pelo cron velho”, mas o próprio contador declara não distinguir nota/trilha/motivo e incluir decisões administrativas por reconciliar; proveniência por idade de cada pk é [nao verificado] — `app/ponto/services/cluster_espurio.py:93`, `app/ponto/services/cluster_espurio.py:94`, `app/ponto/services/cluster_espurio.py:278`.


## 11/11 10:00 · sessao 5604be92552c · - · - · origem=Ronald (tmux)

O diff respeita os escritores declarados dos campos principais:

- `status_local`, `via_encerramento` e `resolvido_em`: tratamento chama `_renascer_por_premissa_viva`; esta chama `reabrir`, que chama `_gravar_estado`. O núcleo altera e salva os três campos; `models.py` é porta declarada. Não há escrita direta nova desses campos no serviço. `app/ponto/services/cluster_espurio.py:273`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:244`; `app/chamados/models.py:248`; `app/chamados/models.py:296`; `app/core/portas.py:218`.
- Nota de renascimento: `_renascer_por_premissa_viva` usa `nota.anexar_ocorrencia`, escritor declarado de `FollowupChamado`; a porta cria a nota. Leitores incluem histórico, detalhe e painel. `app/chamados/models.py:1021`; `app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`; `app/chamados/services/historico.py:80`; `app/chamados/views.py:352`; `app/chamados/services/painel.py:362`.
- `LogAuditoria`: núcleo emite `estado_chamado`; serviço emite `reabrir_legado_cluster_o169`; ambos passam por `core.observ.evento`. O histórico e os vigias leem `estado_chamado`; a ação nova tem leitor específico no teste. `app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:277`; `app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:28`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`.
- `CelulaDia.ata`: tratamento apenas lê via contador e `_leitura_da_celula`; produtor é cartório, persistência passa pela porta da célula. `app/ponto/services/cluster_espurio.py:245`; `app/ponto/services/cluster_espurio.py:126`; `app/ponto/services/cluster_espurio.py:27`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1033`; `app/core/portas.py:112`.
- `PeriodoFechado`: tratamento apenas consulta indiretamente; escritores de tranca/reabertura são `trancar_periodo`/`reabrir_periodo` em fechamento, módulo declarado. `_dia_em_competencia_trancada` lê `competencia_trancada`, que consulta `reaberto_em__isnull=True`. `app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1437`; `app/core/portas.py:501`; `app/chamados/models.py:1033`; `app/ponto/services/fechamento.py:1473`.

Leitores relevantes dos valores alterados:

- Contador filtra `ENCERRADOS` e via `RECONCILIAR`; relatório lê status/via antes e depois. `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:112`; `app/ponto/services/cluster_espurio.py:251`; `app/ponto/services/cluster_espurio.py:272`; `app/ponto/services/cluster_espurio.py:280`.
- Via de tranca determina desarquivamento e devolução à fila quando período reabre. `app/chamados/views_arquivar.py:64`; `app/chamados/services/competencia_trancada.py:259`.
- `resolvido_em` participa da recusa por fechamento administrativo sem fato novo e das estatísticas. `app/chamados/models.py:1342`; `app/chamados/services/painel.py:245`; `app/chamados/services/fila.py:109`.
- Os valores reais são `RECONCILIAR → 'sistema'` e tranca → `'competencia_trancada'`. `app/chamados/catalogo/motor.py:88`; `app/chamados/catalogo/motor.py:92`; `app/chamados/catalogo/motor.py:95`.

Defeitos e nuance do ramo parado:

- A trilha não é garantida atomicamente: `evento` é best-effort, engole falha e retorna `None`; o tratamento ignora o retorno e marca aplicado. Pode reabrir sem a linha obrigatória do legado; o núcleo usa o mesmo emissor para `estado_chamado`. `app/core/observ.py:98`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:292`.
- `get_user_sistema()` pode criar usuário antes do laço, mesmo universo vazio ou composto somente de linhas trancadas. Logo “nada se escreve” no ramo parado não vale para qualquer entidade do banco se faltar esse usuário. `app/ponto/services/cluster_espurio.py:262`; `app/core/utils.py:18`.
- Não encontrei gate que relavre **somente a via para tranca, preservando o status encerrado**: o gate dedicado recusa qualquer estado fora de `VIVOS`. `app/chamados/models.py:1622`; `app/chamados/models.py:1630`.
- A alegação literal “nenhuma porta troca a via de chamado já encerrado sem reabrir” é excessiva: `resolvido → fechado` é permitido; `transicionar → _aplicar → _gravar_estado` troca via para `admin`, preservando `resolvido_em`, sem estado vivo intermediário. Isso não resolve a relavra solicitada para tranca. `app/ponto/services/cluster_espurio.py:220`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:447`; `app/chamados/models.py:242`.

## 11/11 10:00 · sessao e11873e160bf · FIO-E0-V4-E3 · sonda · origem=Code

Não reexecutei a sonda. O envelope reescreve credenciais, cria/remove arquivos temporários e acrescenta log, contrariando sua regra de não escrever arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:126), [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:216), [bin/sonda_leitura.sh:248](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:248) A identificação desta cópia como HEAD do hash informado não pôde ser confirmada: não há repositório Git reconhecido no diretório. [nao verificado]

Transcrição integral da saída fornecida, sem nova execução. [nao verificado]

```text
sonda_leitura: sonda_x1_comp11_v2.py [c1ce461ce6ce3acb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

PERG pk 54831 col237 2026-11-07 liga=celula data_ev=2026-11-07 mat=sim valid=nao via=celula_absolveu cel_ver=fato_em_ausencia disp_fechada=sim pai=nenhum viva=True motivo=batida_fora_escala_grave
celula.acusa n 76
celula.nao_acusa n 1
pares n 77
pergunta.fora_universo n 1
pergunta.total n 1
pergunta.viva n 1
```

Na saída fornecida não consta traceback, `sonda_nao_verificada` nem linha `CHAMADO_VIVO_X1`. Não há contadores adicionais transcritos; não os completei com zeros. [nao verificado]

Não existe linha PERG com `mat=nao valid=nao` no dado fornecido; portanto, não há caso individual dessa classe para mapear. [nao verificado] O caminho condicional de absolvição seria `qs_perguntas_absolviveis` → `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula`: exige materialização e validação nulas, célula com veredito preenchido que não acusa, pergunta morta pelo juiz e dia não trancado; então chama `carimbar(VIA_CELULA_ABSOLVEU, ...)`. Deixar de acusar sozinho não basta. [app/chamados/reconciliador.py:627](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:627), [app/chamados/reconciliador.py:661](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:661), [app/chamados/reconciliador.py:595](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:595)

Para a **PERG 54831**, com via preenchida e `viva=True` no dado: [nao verificado]

- **O carimbo não garante morte.** `fato_em_ausencia` pertence a `VEREDITOS_ACUSAM`; quando alcança esse ramo, o juiz retorna `True`, mesmo havendo materialização. `celula_absolveu` não integra as vias de identidade que encerram independentemente do fato. [app/escala/models.py:1767](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/escala/models.py:1767), [app/chamados/juizes.py:1372](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/juizes.py:1372), [app/chamados/models.py:2510](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/models.py:2510)

- **O reconciliador não a recarimba nesse estado**, nem se a célula passar a não acusar: sua seleção exige `materializada_em IS NULL`. O reatador de órfãs também exige materialização nula e disputa aberta. [app/chamados/reconciliador.py:629](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:629), [app/chamados/reconciliador.py:1500](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:1500)

- **A cascata S143/S128 também não a recarimba:** o receiver chama o resolvedor, cujo primeiro conjunto exige materialização nula e disputa aberta; o ramo sem credor pula disputas fechadas e exige materialização e validação nulas. [app/chamados/signals.py:115](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:115), [app/chamados/services/disputa_emissao.py:1823](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1823), [app/chamados/services/disputa_emissao.py:1907](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1907), [app/chamados/services/disputa_emissao.py:1916](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1916)

- **S144 também a exclui:** delega a `materializar_pendentes_aprovadas`, que seleciona somente materialização nula. [app/chamados/signals.py:162](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:162), [app/chamados/services/materializacao.py:1418](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/materializacao.py:1418)

- **Existe outro escritor capaz de desfazer o carimbo**, se receber essa pergunta e confirmar sua elegibilidade/vitalidade: `desfazer_carimbo_sem_lastro`, cujo padrão permite reabrir disputa fechada, limpa os três campos do carimbo e reabre a disputa. Porém, o julgamento automático chama essa porta com `reabrir_fechada=False`, excluindo disputas fechadas. Isso impede concluir que “nenhum escritor existe”, mas esses caminhos automáticos não corrigem essa linha no estado informado. [app/chamados/services/relavra_via.py:187](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:187), [app/chamados/services/relavra_via.py:238](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:238), [app/chamados/services/relavra_via.py:246](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:246), [app/chamados/services/relavra_via.py:260](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:260), [app/chamados/services/fabrica_da_celula.py:209](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/fabrica_da_celula.py:209)

O dado não comprova quem gravou o carimbo original, quando o veredito mudou ou se alguma dessas portas foi executada para a PK 54831. No anexo, `pai=nenhum` representa `chamado_destino`, não o chamado-pai da disputa. [nao verificado]

## 11/11 09:55 · sessao eacf68e547f2 · FIO-E0-V4-E3 · sonda · origem=Code

1) Transcrição de todos os contadores e linhas `VIVA`/`CHAMADO_VIVO` fornecidos; execução em produção sem verificação independente: [nao verificado]

```text
VIVA pk 54831 col237 2026-11-07
chamado.total n 1
pares n 77
pergunta.total n 1
pergunta.via.celula_absolveu n 1
pergunta.viva n 1
```

Não aparece linha `CHAMADO_VIVO`, nem contador `chamado.vivo` impresso na saída anexada. [nao verificado]

2) Não aparece `sonda_nao_verificada` nem traceback no dado fornecido. [nao verificado]

3) **Não: os recortes não são equivalentes.** O alcance possível pelo código é maior, pelos seguintes caminhos. (`app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:608`)

- **Perguntas com data ausente ou divergente.** O reconciliador seleciona pelo colaborador da disputa, exige `materializada_em` e `validada_em` nulos e célula vinculada com veredito preenchido que não acusa. Não restringe `data_evento`, `celula__data` nem competência. Assim, perguntas vinculadas às células desses dias, mas com `data_evento=NULL` ou fora dos dias listados para aquele colaborador, ficam fora da sonda e podem entrar como candidatas no reconciliador. (`app/chamados/reconciliador.py:627–642`)

- **Candidata não significa pergunta efetivamente absolvida.** O gerador só entrega a pergunta quando `pergunta_viva(p)` é falsa e o dia não está trancado; o escritor ainda depende do retorno de `carimbar`. Portanto, a diferença de filtros demonstra alcance possível, sem provar que existam registros omitidos elegíveis em produção. (`app/chamados/reconciliador.py:661–672`, `app/chamados/reconciliador.py:595–597`)

- **Chamado-pai sem célula no dia sondado.** Após carimbar, o escritor pode fechar a disputa; `pode_fechar()` consulta todas as perguntas dela, sem recorte de dia. O signal busca o chamado-pai pelo seu PK e tenta reconciliá-lo, sem exigir `celula__data=dia`. Um pai sem célula ou com célula fora dos dias sondados escapa da consulta de chamados da sonda, embora possa ser alcançado por esse caminho. (`app/chamados/reconciliador.py:598–614`, `app/chamados/models.py:1848–1853`, `app/chamados/signals.py:28–42`)

- **Perguntas adicionais pela cascata do pai.** Se o chamado for encerrado, o signal pode chamar o resolvedor, cujo filtro usa `chamado_destino`, materialização nula e disputa aberta, sem limitar `data_evento`. Essas perguntas também podem ficar fora da sonda; a cascata aplica suas próprias guardas antes de carimbar. (`app/chamados/signals.py:96–115`, `app/chamados/services/disputa_emissao.py:1823–1827`, `app/chamados/services/disputa_emissao.py:1837–1865`)

Separadamente das duas funções perguntadas, a rotina irmã `retratar_chamados_absolvidos_por_celula` também alcança chamados vivos de turno sem exigir célula vinculada: determina o dia por `dia_do_fato` e busca a célula por colaborador/data. Esse dia pode vir do contexto, da batida ou da disputa. (`app/chamados/reconciliador.py:996–1004`, `app/chamados/reconciliador.py:1023–1026`, `app/chamados/juizes.py:98–138`)

Existência e quantidade de registros nessas lacunas, causa da pergunta viva e resultado de eventual reconciliação em produção: [nao verificado].

## 11/11 09:37 · sessao 2d68990e4812 · RAIZ-DA-DISPUTA · sonda · origem=Code

**O aval resolve a identidade da pausa por leitura da célula; D8 continua sendo a pergunta de lei aberta sobre BATIDA do acervo sem episódio versus BATIDA com episódio.** (`app/escala/models.py:1652`; `app/chamados/services/fio.py:1274`; complemento do aval recebido: [nao verificado].)

Os números abaixo transcrevem o dado raspado fornecido. Uso **[nao verificado]** para sua proveniência sem arquivo local numerado; as referências de código explicam os critérios, sem certificar novamente a produção. Identidade Git da cópia com o hash informado: **[nao verificado]**.

1. **Preenchimento dos `[aguarda sonda]`**

| Item do censo | Preenchimento |
|---|---|
| **77 colab-dias** | A saída informa **7.057 colab-dias com ambos os lados**, **2.777 sem um dos lados**, **31 com multiplicidade em alguma posição** e **94 com exatamente uma pergunta por posição e metade validada/outra viva**. Correspondência nominal com os antigos 77: **[nao verificado]**. |
| **486 grupos / 848 excedentes** | Não aparecem `D08.grupo`, `D08.grupos`, `D08.excedentes` nem os baldes do plano. Total atual, distribuição retratável/protegida e correspondência com 486/848: **[nao verificado]**. |
| **18 DS com `pode_fechar=True`** | A saída informa **486 DS abertas**, das quais **23 com C7 verdadeiro**. Correspondência com as antigas 18 e causa histórica da permanência abertas: **[nao verificado]**. |
| **51522/51523/col960** | A emissão contém **as duas perguntas**, ambas de **col960**; `caso.col960_confere=1` significa que a conjunção dessa conferência passou. Resposta, validação, materialização, via, DS atual, data, intervalo e participação nos outros agregados: **[nao verificado]**. | [nao verificado]

Os antigos 77, 18 e 486/848 estão documentados, respectivamente, em `app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:1883` e `app/docs/BACKLOG.md:1887`. **94 não comprova crescimento de 77; 23 não comprova crescimento de 18**, porque não foi fornecida a correspondência dos universos. **[nao verificado]**

As **23 DS** emitidas são os PKs **2395, 2721, 3958, 3994, 4001, 4004, 4013, 4015, 4020, 5148, 5155, 5167, 5522, 5934, 6308, 7288, 7290, 7299, 7788, 7959, 7995, 7996 e 8006**. **[nao verificado]**

A interpretação de cada família de contadores é:

| Contadores e números recebidos — **[nao verificado]** | O que medem; o que não medem |
|---|---|
| `D01.colab_dia_candidato=7057`; `falta_lado=2777`; `multiplas_linhas_na_posicao=31`; emissões `identidade_a_conferir` | A sonda agrupa por **colaborador/data**, usando posições derivadas do motivo; multiplicidade conta colab-dias, não perguntas excedentes. Não exige mesma DS nem DS aberta e não certifica intervalo específico. Duas pausas ainda compartilham motivo/posição. (`app/chamados/services/declaracao_texto.py:48`, `:55`; composição da sonda: [nao verificado].) |
| `D01.metade_validada_outra_viva=94` | Entre grupos com uma pergunta de cada posição, testa validação de uma e C6 verdadeiro da outra. Não demonstra que sejam saída/volta **da mesma pausa**, nem tempo de espera ou causa da situação. (`app/chamados/juizes.py:1324`; composição da sonda: [nao verificado].) |
| `D02.validada_sem_lastro=1181`; `D03.carimbo_sem_lastro=4291` | Validação ou materialização presente, respectivamente, com `_pergunta_materializada_de_fato` retornando exatamente `False`. Esse leitor também reconhece encerramentos sem batida; não são contagens de validações inválidas ou plantios fracassados. (`app/chamados/juizes.py:647`, `:662`, `:688`.) |
| `D03.aplicacao`: `aplicada=5641`, `aplicavel=322`, `bloqueada_paridade=182`, `invalida=327`, `ja_satisfeita=76`, `recusada_paridade=322` | Classificações atuais de perguntas **hora com texto**, pelo leitor de aplicação. Não são retornos de execução da porta. `aplicada` decorre do carimbo, excetuada a via de paridade; `ja_satisfeita` exige batida encontrada e aprovada. (`app/chamados/services/materializacao.py:425`, `:432`, `:434`, `:438`, `:442`, `:448`, `:453`, `:457`.) |
| `D03.aplicada_sem_lastro=779` | Interseção entre classificação `aplicada` e lastro `False`. Não demonstra 779 plantios malsucedidos: o leitor chama de aplicada quase toda pergunta carimbada. (`app/chamados/services/materializacao.py:434`, `:442`; `app/chamados/juizes.py:647`.) |
| `D03.reuso_nao_aprovado=102` | Perguntas cujo slot derivável encontra batida não aprovada. Não conta atos históricos de reuso nem batidas distintas. (`app/chamados/juizes.py:235`; `app/chamados/services/materializacao.py:448`, `:859`, `:865`.) |
| `D05.pendencia_com_carimbo=79`; `recusa_com_lastro=5970` | Coexistências atuais de campos: pendência/materialização e texto de recusa/lastro verdadeiro. Não demonstram estados exclusivos ou contradição: pendência pode acompanhar prosseguimento, e recusa é texto persistido. (`app/chamados/models.py:2556`, `:2560`; `app/chamados/services/materializacao.py:850`.) |
| `D05.codigo`: `ABRE_TURNO=2`, `COLISAO=27`, `COMPETENCIA_APROVADA=41`, `DATA_EM_DISPUTA=33`, `NAO_GERA_BATIDA=53`, `OUTRO=5868`, `PARIDADE=418` | Classificação do **texto atual** de recusa pelo primeiro padrão correspondente; `OUTRO` é o residual. Não enumera recusas históricas nem particiona os 94 candidatos por causa. (`app/chamados/catalogo/recusa.py:25`, `:77`, `:81`, `:88`.) |
| `D07.vizinha_retratada=15` | Perguntas hora/texto com slot derivável cuja vizinha retornada tem retratação. Não exige ausência de reuso nem efetivo bloqueio pela guarda; não prova 15 recusas causadas por retratadas. A busca compara tipo sem excluir explicitamente retratadas. (`app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`; composição da sonda: [nao verificado].) |

| Contadores T e números recebidos — **[nao verificado]** | O que medem; o que não medem |
|---|---|
| `T01.resposta_presente=17408`; `T02.validacao_a_invalidar_por_nova_resposta=15239`; `T03.ja_validada=15239`; `T03.sem_resposta=31108` | Presença/ausência dos timestamps. T02 conta validações existentes que uma nova resposta admissível limparia; não mede novas respostas recebidas, invalidações executadas ou admissibilidade futura. (`app/chamados/models.py:2448`; `app/chamados/services/disputa_emissao.py:1470`, `:1480`; `app/chamados/services/validacao.py:393`, `:396`.) |
| `T07.fila_normal=97` | Resposta e validação presentes, materialização ausente: o predicado da fila normal. Não garante sucesso da materialização nem conta pares prontos. (`app/chamados/services/materializacao.py:1369`.) |
| `T08.data_por_conferir=157` | Divergência de data retornada pelo leitor, sem conferência. Não conta recusas efetivamente executadas. (`app/chamados/services/fio.py:40`; `app/chamados/services/materializacao.py:1191`, `:1193`.) |
| `T09.hora_sem_texto=39699` | Perguntas de tipo hora sem texto após `strip`, inclusive fora da fila de materialização. Não significa 39.699 tentativas recusadas. (`app/chamados/services/materializacao.py:829`; filtro da sonda: [nao verificado].) |
| `T10.tipo_pendente=745` | Derivações hora/texto que acrescentaram pendência de tipo não conferido à lista em memória. Não significa que a pendência foi gravada; o caminho pode prosseguir. (`app/chamados/services/materializacao.py:330`, `:386`, `:395`.) |
| `T11.reuso_no_slot=3700` | Perguntas com slot derivável para as quais o leitor encontra batida do mesmo tipo na janela. Não conta reusos executados nem batidas distintas. (`app/chamados/juizes.py:235`, `:243`.) |
| `T12.paridade_sem_lampada=442`; `T13.paridade_com_lampada=368` | Sem reuso encontrado, com vizinha do mesmo tipo e leitor de furo exatamente `False`; separam lâmpada `não True` de lâmpada `True`. T12 inclui lâmpada desconhecida. Não são recusas/carimbos executados nem predizem o resultado da aplicação conjunta do par. (`app/chamados/services/materializacao.py:875`, `:901`, `:924`; conjunção da sonda: [nao verificado].) |
| `T18.absolvivel=1` | Itens produzidos pelo leitor de absolvíveis, nos colaboradores alcançados pelas perguntas carregadas. Aplica C6 e exclui tranca; não conta DS fecháveis ou carimbos executados. (`app/chamados/reconciliador.py:645`, `:661`, `:667`, `:672`.) |
| `T19.encerra=1` | Órfãs do universo próprio cujo veredito corrente retorna `ENCERRA`. Não significa encerramento executado. (`app/chamados/reconciliador.py:1044`, `:1048`, `:1060`, `:1092`.) |
| `T20.DS_abertas=486`; `T20.C7_true=23` | DS com fechamento nulo e, entre elas, as que passam no C7 completo: nenhuma resposta sem veredito e nenhum fio vivo. Não informa por que ficaram abertas, nem equivale aos 486 **grupos** antigos. (`app/chamados/models.py:1842`, `:1848`, `:1853`; universo da sonda: [nao verificado].) |

Os contadores `estado.r*.v*.m*` são combinações de presença dos timestamps **resposta/validação/materialização**, não a propriedade `estado`, o C6 ou uma história de transições. (`app/chamados/models.py:2448`, `:2449`, `:3033`, `:3393`; construção da sonda: [nao verificado].)

| Combinação | Número recebido |
|---|---:|
| `r0.v0.m0` | 11.115 — [nao verificado] |
| `r0.v0.m1` | 19.993 — [nao verificado] |
| `r1.v0.m0` | 972 — [nao verificado] |
| `r1.v0.m1` | 1.197 — [nao verificado] |
| `r1.v1.m0` | 97 — [nao verificado] |
| `r1.v1.m1` | 15.142 — [nao verificado] |

`via.*` conta perguntas pelo **valor atual não vazio** de `via_resolucao`; valores fora de `VIAS` vão para `nao_catalogada`. Não mede atos históricos nem prova lastro. (`app/chamados/models.py:2525`, `:2561`; agrupamento da sonda: [nao verificado].)

| Vias | Números recebidos — **[nao verificado]** |
|---|---|
| `adesao`; `admin_decidiu`; `ausencia_dia`; `cadastro_folga` | 2.296; 3.071; 1.114; 341 |
| `celula_absolveu`; `chamado_encerrado`; `competencia_trancada`; `completude_turno` | 3.644; 7.021; 1.949; 3.380 |
| `declaracao`; `duplicata`; `fantasma`; `fato_ocupado` | 5.425; 4.689; 1.047; 27 |
| `juiz_aprovou`; `legado_sem_lastro`; `nao_catalogada`; `paridade_recusada`; `sem_turno_aberto` | 339; 24; 325; 324; 1.316 |

2. **Falhas e respostas ausentes**

Não há `sonda_nao_verificada` nem `nao_verificado.*` na saída recebida. Portanto, **nenhum ponto de falha foi informado**; não há base para localizar uma exceção em produção. **[nao verificado]**

A ausência de D08 também **não prova falha**. No script anexado, suas chaves só são incrementadas dentro do laço; o leitor pode devolver uma lista vazia. A presença das emissões posteriores de DS e caso não sustenta afirmar aborto naquele laço. (`app/chamados/services/fio.py:1942`, `:1993`; execução e integralidade da saída: [nao verificado].)

Assim, ficam sem resposta **a reconciliação dos 486/848 com o universo real, os baldes de retratação, o alcance de D8 e as divergências numéricas do explicador/relatório contra C7**. Não preencho essas ausências com zero. **[nao verificado]**

Ausência de marcador externo tampouco certifica ausência de falhas internas: o leitor de furo captura exceção e retorna `False`; o gerador de absolvíveis captura exceção e ignora a pergunta. Ocorrência dessas falhas nesta execução: **[nao verificado]**. (`app/chamados/services/materializacao.py:184`; `app/chamados/reconciliador.py:669`.)

3. **AST: já existe a função “estas duas perguntas são o par da mesma pausa”?**

**Não identifiquei esse predicado no AST inspecionado.** Ausência universal por reflexão/código dinâmico: **[nao verificado]**. Existem a representação dos pares na célula e leitores de posições/instâncias, com contratos diferentes:

| Sítio existente | Contrato |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara os pares `hii/hfi` e `hii2/hfi2`; a entidade fornece tipos e ordem canônica. É a identidade dos **marcos**, não uma comparação entre perguntas. (`app/escala/models.py:1652`, `:1654`, `:1659`.) |
| `EscalaColaborador.pausas_do_dia` | Lê todas as pausas completas do DNA; célula existente sem marcos devolve vazio. `intervalo_do_dia` lê somente a primeira. (`app/escala/models.py:1163`, `:1189`, `:1197`, `:1202`.) |
| `grade_da_celula` / `_marcos_faltantes` | Transportam marcos/lâmpadas e retornam faltantes como dia, posição, tipo e hora. (`app/escala/services/leitor_celula.py:114`, `:148`; `app/chamados/services/declaracao_texto.py:86`, `:100`.) |
| `posicao_do_marco` | Primeiro=`inicio`, último=`fim`, intermediário S=`int_ida`, E=`int_volta`. Sozinha não distingue o intervalo. (`app/chamados/services/declaracao_texto.py:75`, `:79`, `:83`.) |
| `tarefa_do_dia._posicoes` / `instancias_do_turno` | Enumeram repetições em memória, incluindo `int_ida:2`/`int_volta:2`, e publicam instância, posição, tipo, hora e lâmpada. (`app/chamados/services/tarefa_do_dia.py:57`, `:65`, `:67`, `:91`, `:101`.) |
| `fato_do_marco_no_espelho` | Para motivos de intervalo, considera os marcos correspondentes de **todas** as pausas; não identifica qual pausa uma pergunta individual cobra. (`app/chamados/juizes.py:911`, `:919`, `:922`.) |

Há uma perda concreta do índice na pergunta persistida: `_chave_de_identidade` deriva `instancia_marco` apenas do motivo e rejeita um valor diferente, como o sufixo `:2`. O comentário reconhece a separação das pausas como pendente. (`app/chamados/models.py:3437`, `:3440`, `:3441`, `:3445`; `app/chamados/services/declaracao_texto.py:49`.)

Quem produz/escreve esses dados:

- **DNA e lâmpadas:** `montar_dna` produz os pares de marcos; `montar_lampadas` produz chave, tipo, hora e instante. (`app/ponto/management/commands/gerar_celulas.py:73`, `:83`, `:92`, `:36`, `:60`, `:68`.)
- **Persistência da célula:** portas de geração, regeneração, edição e reversão. (`app/ponto/portas/celula.py:857`, `:256`, `:272`, `:591`, `:604`, `:786`.)
- **Posição da pergunta:** `save` chama `_chave_de_identidade` no nascimento, que escreve a posição simples. (`app/chamados/models.py:3420`, `:3440`.)
- **Índice da tarefa:** `_posicoes` o produz apenas na estrutura retornada em memória. (`app/chamados/services/tarefa_do_dia.py:65`, `:67`.)

Portanto, o encaixe técnico deve consumir esses marcos da célula e preservar sua identidade até as perguntas; o aval proíbe derivação paralela, juiz novo e gravação de “aguardando metade”. **[nao verificado — determinação do anexo recebido.]**

4. **D1–D8 depois do aval**

| Decisão | O que permanece aberto | Natureza e fundamento |
|---|---|---|
| **D1 — unidade/espera/identidade** | Implementar o ato conjunto e ligar cada pergunta ao intervalo correto da célula. A validação e o lote atuais ainda operam individualmente. (`app/chamados/services/validacao.py:461`, `:463`; `app/chamados/services/materializacao.py:1375`.) | **Decisão fechada; execução técnica aberta.** Par no mesmo ato e estado derivado já registrados; fonte única S133 e L-099 para ausência de cadastro. (`app/docs/BACKLOG.md:1929`, `:1930`; `CLAUDE.md:148`; `app/docs/LEIS.md:122`.) Sem marco, seguir sozinho: **[nao verificado — aval recebido]**. |
| **D2 — resposta/validação/reversão** | Harmonizar resposta humana, timestamps supridos e reversões, preservando atos e trilha. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:3132`, `:3135`; `app/chamados/services/validacao.py:199`.) | **Adequação técnica sob lei escrita**, L-007: escritor canônico e auditoria. (`app/docs/LEIS.md:65`.) |
| **D3 — aplicada versus encerrada** | Adequar leitores que tratam carimbo como aplicação; os 779 são interseção corrente, não diagnóstico histórico. (`app/chamados/services/materializacao.py:434`, `:442`; número: [nao verificado].) | **Adequação técnica sob lei escrita**, testemunha lê autoridade; identidade pode encerrar sem plantar. (`app/docs/LEIS.md:60`; `app/chamados/services/materializacao.py:924`, `:1099`.) |
| **D4 — escritores de via** | Organizar carimbo, descarimbo, troca isolada e relavra sob portas canônicas com trilha. (`app/chamados/models.py:3101`, `:3266`, `:3275`; `app/chamados/services/relavra_via.py:125`.) | **Adequação técnica sob L-007**, sem pergunta de lei nova. (`app/docs/LEIS.md:65`.) |
| **D5 — pendência/recusa** | Adequar consumidores para não inferirem espera da metade desses campos. Os campos têm outros usos atuais. (`app/chamados/services/materializacao.py:850`; `app/chamados/services/disputa_emissao.py:1497`.) | **Adequação técnica**; estado explícito superado e fonte/escritor únicos já escritos. (`app/docs/BACKLOG.md:1930`; `app/docs/LEIS.md:60`, `:65`.) |
| **D6 — fechamento** | Migrar explicador/relatório para C7 e implementar reação da raiz; causalidade histórica das DS continua sem resposta. (`app/chamados/services/explicador.py:137`; `app/chamados/relatorios_disputa.py:20`; histórico: [nao verificado].) | **Autoridade decidida por lei escrita:** C7 único. (`app/docs/CORTES.md:272`; `app/core/juizes.py:279`.) Autor do fecho automático = autor do ato que carimbou a última pergunta, com motivo próprio: **decisão técnica explícita**, [nao verificado — aval recebido]. |
| **D7 — vizinhança/universo/papel** | Corrigir e verificar a guarda pelos leitores existentes; 15 vizinhas retratadas não medem seu efeito causal. (`app/chamados/services/materializacao.py:94`, `:104`; número/causalidade: [nao verificado].) | **Adequação técnica sob leis escritas** L-025 e L-087: universo apurável e papel pelo marco. (`app/docs/LEIS.md:76`, `:119`.) |
| **D8 — acervo sem episódio** | Definir quando BATIDA sem episódio e BATIDA com episódio são o mesmo fato, permitindo retratação entre elas. Alcance nos 486/848 permanece sem medição apresentada. (`app/chamados/services/fio.py:1274`, `:1275`; alcance: [nao verificado].) | **Pergunta de lei aberta.** O comportamento conservador atual mantém `sem_episodio_intocada`; o aval da pausa não fornece resposta para essa identidade. (`app/chamados/services/fio.py:1294`, `:1295`; alcance do aval recebido: [nao verificado].) |

**Perguntas de lei restantes: 1 — D8.** (`app/chamados/services/fio.py:1275`.)

## 11/11 09:35 · sessao ad850049a991 · - · - · origem=Ronald (tmux)

A sonda inline não tem arquivo local disponível; os números abaixo vêm apenas do anexo, portanto sua proveniência por `arquivo:linha` é **[nao verificado]**. Os leitores citados foram conferidos na cópia.

| Contador / saída | O que efetivamente mede / limite |
|---|---|
| `D01.colab_dia_candidato=7057` | Chaves `(colaborador_id,data_evento)` com pelo menos uma pergunta de cada posição, após exclusão de conversa/fora do fato e conferência `instancia_marco == instancia_do_motivo`. A posição vem de `app/chamados/services/declaracao_texto.py:55`. Não certifica intervalo específico; duas pausas ainda compartilham posição/motivo, limitação declarada em `:49`. |
| `D01.falta_lado=2777` | Chaves candidatas que não têm ambos os lados. Não são necessariamente pausas incompletas da célula; é ausência de perguntas em uma posição. **[nao verificado por arquivo:linha: lógica da sonda inline]** |
| `D01.multiplas_linhas_na_posicao=31` | Colab-dias com ambos os lados e cardinalidade diferente de um em pelo menos um lado. Incluídos nos 7057 e excluídos do teste de metade validada. Não conta duplicatas retratáveis ou quantidade de intervalos. **[nao verificado por arquivo:linha: sonda inline]** | [nao verificado]
| `D01.metade_validada_outra_viva=94` | Colab-dias com exatamente uma pergunta por posição e `(validada(a) ∧ C6(b)) ∨ (validada(b) ∧ C6(a))`. C6 vem de `app/chamados/juizes.py:839`; validada retorna morta em `:1325`. Não demonstra 94 pares reais da mesma pausa, nem histórico de espera, devolução ou recusa. |
| `D02.validada_sem_lastro=1181` | Perguntas com validação e `_pergunta_materializada_de_fato is False`. Esse leitor admite encerramentos por identidade e outras testemunhas, e não significa exclusivamente existência de batida nova: `app/chamados/juizes.py:647`, `:662`, `:688`, `:734`. Não prova 1181 validações inválidas. |
| `D03.carimbo_sem_lastro=4291` | Materialização timestamp presente e leitor de lastro `False`; mesma ressalva acima. |
| `D03.aplicacao.*` | Só perguntas `tipo_resposta='hora'` e texto não vazio. Retornos: aplicada 5641; aplicável 322; bloqueada_paridade 182; inválida 327; já_satisfeita 76; recusada_paridade 322. `app/chamados/services/materializacao.py:432`, `:434`, `:438`, `:442`, `:444`, `:448`, `:452`, `:457`. Não são retornos de execução da porta. |
| `D03.aplicada_sem_lastro=779` | Interseção de estado de aplicação `aplicada` e lastro `False`. O estado `aplicada` decorre do carimbo para qualquer via que não seja paridade: `app/chamados/services/materializacao.py:434`, `:438`, `:442`. Não equivale a 779 plantios malsucedidos. |
| `T11.reuso_no_slot=3700`; `D03.reuso_nao_aprovado=102` | Entre perguntas hora/texto com slot derivável, encontro do leitor de reuso; 102 encontram batida não aprovada. Uso real no escritor em `app/chamados/services/materializacao.py:859`, `:865`; UI requer aprovação em `:448`. Não demonstra 3700 reusos históricos. |
| `D05.pendencia_com_carimbo=79` | Campo de pendência não vazio e timestamp de materialização. Pendência pode coexistir com prosseguimento do plantio: `app/chamados/services/materializacao.py:845`, `:850`, `:852`. |
| `D05.recusa_com_lastro=5970` | Texto atual de recusa não vazio com lastro `True`. Não demonstra contradição ou recusa atual impeditiva, porque é texto remanescente e lastro de leitura atual. **[nao verificado por arquivo:linha: conjunção da sonda]** |
| `D05.codigo.*` | Classificação do texto atual de recusa por primeiro padrão textual correspondente; `OUTRO` é fallback. `app/chamados/catalogo/recusa.py:77`, `:81`, `:88`. Contagens: ABRE_TURNO 2; COLISAO 27; COMPETENCIA_APROVADA 41; DATA_EM_DISPUTA 33; NAO_GERA_BATIDA 53; OUTRO 5868; PARIDADE 418. Não é enumeração de atos históricos nem causas exclusivas dos 94 colab-dias. |
| `D07.vizinha_retratada=15` | Para hora/texto com slot derivável, o leitor retorna vizinha do mesmo tipo cuja `retratada_em` está preenchida. Busca anterior/posterior sem filtro de retratação: `app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`. Não exige que a guarda bloqueie; nem que não haja reuso; não prova 15 recusas causadas por retratadas. |
| `T18.absolvivel=1` | Quantidade produzida por `perguntas_absolvidas_pela_celula` nos colaboradores alcançados pela lista de perguntas. O leitor aplica C6 e exclui dia trancado: `app/chamados/reconciliador.py:645`, `:661`, `:667`, `:672`. Não é fechamento executado ou DS fechável. |
| `T19.encerra=1` | Órfãs no universo `materializada=None`, `validada=None`, DS aberta, motivos específicos e dia presente, cujo veredito atual é ENCERRA: `app/chamados/reconciliador.py:1044`, `:1048`, `:1082`, `:1084`, `:1086`, `:1090`, `:1092`. Não é carimbo executado. |
| `T20.DS_abertas=486`; `T20.C7_true=23` | DS com `fechada_em=None`; dentre elas, C7 retorna `True`. C7 exige ausência de resposta sem veredito e fio vivo: `app/chamados/models.py:1842`, `:1848`, `:1853`. Não explica por que não fecharam nem identifica as antigas 18. |
| Caso | Existem exatamente as duas perguntas pedidas, ambas col960, segundo `caso_51522_51523 n2` e `caso.col960_confere n1`. Não foram impressos timestamps, C6, DS, data, marco ou intervalo delas. Estado e pertencimento a 94/486/23/grupos continuam **[nao verificado]**. |

**D08 não foi medido na saída disponibilizada.** Não há `D08.grupo`, `D08.grupos`, `D08.excedentes` nem baldes. As assinaturas do inline são compatíveis com a cópia: `app/chamados/services/fio.py:1225`, `:1259`, `:1905`, `:1923`. `grupos_de_colisao` começa com lista vazia e pode devolvê-la (`:1942`, `:1993`); o `Counter` inline só toca D08 dentro do laço. Assim, ausência de chaves não demonstra falha e não autoriza preencher o censo como zero. Os 486 DS abertas não são evidência dos 486 grupos antigos.

Não aparece `sonda_nao_verificada` ou `nao_verificado.*` no dado. T20, T18/T19 e caso presentes tornam indevida a afirmação de aborto no laço D08 para o inline exatamente mostrado. Qual script exato produziu a saída e por que D08 está ausente: **[nao verificado]**.

**Limite dos próprios leitores:** `_plantio_preenche_furo_na_data` engole exceção e retorna `False` (`app/chamados/services/materializacao.py:184`); o gerador de absolvíveis captura exceção e ignora pergunta (`app/chamados/reconciliador.py:669`). Portanto ausência de `nao_verificado.*` externo não certifica inexistência de falhas internas. Logs não fornecidos: **[nao verificado]**.

**Divergências C7:** explicador calcula somente `not fio_vivo` (`app/chamados/services/explicador.py:137`); relatório publica PRONTA_FECHAR se não houver vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Nenhum número de divergências foi impresso. Não convertê-lo em zero sem esclarecer a instrumentação/saída.

## 11/11 09:35 · sessao 9ad8f4f4c039 · - · - · origem=Ronald (tmux)

Não localizei no AST um predicado que receba **duas `PerguntaDisputa` e responda se pertencem à mesma pausa**. A ausência universal por reflexão, código gerado ou chamadas dinâmicas permanece **[nao verificado]**. A inspeção executada em memória percorreu 838 arquivos Python de runtime em `app/` e `bin/`, com 5.422 funções e nenhum erro de parse; essa execução não tem artefato `arquivo:linha` **[nao verificado por arquivo:linha]**.

O par dos marcos já está representado na própria célula: `CelulaDia.PARES_PAUSA_DNA` contém `('hii','hfi')` e `('hii2','hfi2')`; a entidade atribui S/E e fornece a ordem canônica. Isso identifica **marcos**, sem comparar perguntas. (`app/escala/models.py:1652`, `:1654`, `:1659`, `:1672`.)

| Leitor existente | O que responde |
|---|---|
| `EscalaColaborador.pausas_do_dia` | Todas as pausas do DNA da célula; célula existente sem marcos devolve `()`. Template somente quando falta célula. (`app/escala/models.py:1166`, `:1189`, `:1197`, `:1202`, `:1208`.) |
| `intervalo_do_dia` | Somente a primeira pausa retornada pela irmã acima. (`app/escala/models.py:1163`.) |
| `grade_da_celula` / `_celulas_da_ata` | Marcos e lâmpadas lavrados do dia; expõe tipo/hora, preservando ordem. (`app/escala/services/leitor_celula.py:114`, `:122`, `:148`, `:350`.) |
| `posicao_do_marco` | Primeiro=`inicio`, último=`fim`, intermediário S=`int_ida`, E=`int_volta`. Não identifica sozinho qual intervalo. (`app/chamados/services/declaracao_texto.py:75`, `:79`, `:83`.) |
| `_marcos_faltantes` | Lê a grade e retorna `(dia,pos,tipo,hora)` dos marcos faltantes. Não devolve índice da pausa. (`app/chamados/services/declaracao_texto.py:100`, `:121`, `:130`, `:139`.) |
| `tarefa_do_dia._posicoes` | Enumera repetições em memória: `int_ida`, `int_ida:2`, `int_volta`, `int_volta:2`. (`app/chamados/services/tarefa_do_dia.py:57`, `:65`, `:67`.) |
| `instancias_do_turno` | Publica instância, posição, tipo, hora, lâmpada e data civil a partir do dia do leitor único. (`app/chamados/services/tarefa_do_dia.py:91`, `:96`, `:101`.) |
| `_pos_da_pergunta` | Lê `instancia_marco`, com alternativa pelo motivo; não compara duas perguntas. (`app/chamados/services/tarefa_do_dia.py:106`.) |
| `pausas_dna_periodo` / `pausas_de_celulas` | Leitores em lote das pausas pelas chaves da própria entidade. (`app/escala/utils.py:27`, `:57`, `:111`, `:141`.) |

Há um impedimento concreto à persistência do índice já produzido pela tarefa: o nascimento da pergunta deriva `instancia_marco` somente do motivo e **rejeita valor diferente**, como `int_ida:2`. O próprio comentário reconhece a separação de duas pausas como pendente. (`app/chamados/models.py:3425`, `:3437`, `:3440`, `:3441`, `:3445`.) A tabela motivo→posição também documenta que duas pausas caem no mesmo motivo; `_complementar_marcos_faltantes` consome os marcos, mas descarta `_hora` ao criar a pergunta. (`app/chamados/services/declaracao_texto.py:48`, `:55`; `app/chamados/services/disputa_emissao.py:873`, `:881`.)

Os produtores/escritores relacionados são:

- `montar_dna` produz `hii/hfi` e o segundo par pelas chaves de `PARES_PAUSA_DNA`; `montar_lampadas` produz as lâmpadas com `chave`, tipo, hora e instante. (`app/ponto/management/commands/gerar_celulas.py:73`, `:81`, `:92`, `:94`, `:36`, `:60`, `:68`.)
- A porta da célula persiste geração, regeneração, edição humana e reversão. (`app/ponto/portas/celula.py:816`, `:857`, `:110`, `:256`, `:272`, `:534`, `:591`, `:604`, `:740`, `:786`.)
- `PerguntaDisputa.save` chama `_chave_de_identidade` na inserção; esta escreve a posição simples em `instancia_marco`. (`app/chamados/models.py:3413`, `:3420`, `:3425`, `:3440`.)

Portanto, existem a autoridade dos pares de marcos e leitores de posições/instâncias; a ligação persistida **pergunta→marco específico da pausa** ainda perde o índice nos sítios acima. Não se deve chamar `_posicoes` de juiz “estas duas perguntas são o mesmo par”: seu contrato é enumerar os marcos da ata. (`app/chamados/services/tarefa_do_dia.py:57`, `:67`; `app/chamados/models.py:3437`, `:3445`.)

## 11/11 09:34 · sessao b113ca5b5451 · - · - · origem=Ronald (tmux)

Após o aval, **D1–D7 não têm pergunta de lei pendente; continuam com trabalho técnico de implementação/verificação. D8 mantém a pergunta de lei sobre identidade BATIDA do acervo versus BATIDA com episódio.** O novo aval resolve especificamente a identidade da pausa pela célula; não responde à identidade de D8. (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`; conteúdo do aval recebido: **[nao verificado por arquivo:linha]**.)

| D | Situação após aval | Lei escrita versus decisão técnica; evidência |
|---|---|---|
| **D1** | **Decisão fechada; implementação aberta.** Aplicar o par no mesmo ato, esperar a outra metade em estado derivado; identificar saída/volta do mesmo intervalo pelos marcos da célula. Sem marcos, cada pergunta segue sozinha. | O ato conjunto está em `app/docs/BACKLOG.md:1929`; estado explícito foi superado pelo derivado em `app/docs/BACKLOG.md:1930`. Fonte única de previsto: S133/L-024, `app/docs/LEIS.md:75`; testemunha lê a autoridade: `CLAUDE.md:20`; cadastro não se cura por código: L-099, `app/docs/LEIS.md:122`. O complemento específico do novo aval está apenas no texto recebido: **[nao verificado por arquivo:linha]**. Hoje a validação ainda materializa uma P (`app/chamados/services/validacao.py:461`, `:463`) e o lote itera individualmente (`app/chamados/services/materializacao.py:1375`, `:1376`). |
| **D2** | **Sem pergunta nova; harmonização técnica aberta.** Preservar diferença entre resposta humana, projeção suprida e reversão. | Escritor canônico e trilha são L-007 (`app/docs/LEIS.md:65`). Nova resposta limpa validação (`app/chamados/services/disputa_emissao.py:1468`, `:1480`); devolução também limpa, registrando DEVOLUÇÃO (`app/chamados/services/validacao.py:199`, `:205`); carimbo pode suprir resposta/validação (`app/chamados/models.py:3132`, `:3135`). Como compatibilizar essas portas é decisão técnica sob a lei escrita. |
| **D3** | **Sem pergunta nova; significado e leitores técnicos abertos.** Carimbo terminal não prova plantio. | Testemunhas leem autoridade (`CLAUDE.md:20`; `app/docs/LEIS.md:60`). Há encerramento por identidade sem criar batida (`app/chamados/services/materializacao.py:924`, `:1099`), enquanto o leitor de aplicação chama quase todo carimbo de “aplicada” (`app/chamados/services/materializacao.py:434`, `:438`, `:442`) e histórico o descreve como fato registrado (`app/chamados/services/historico.py:103`, `:104`). Adequação dessas testemunhas é técnica. |
| **D4** | **Sem pergunta nova; convergência técnica aberta.** | Um escritor canônico e exceção com trilha: L-007 (`app/docs/LEIS.md:65`; `CLAUDE.md:32`). Existem atribuição comum (`app/chamados/models.py:3101`), descarimbo em lote (`:3266`), troca isolada (`:3275`, `:3276`) e relavra (`app/chamados/services/relavra_via.py:125`, `:127`). Organizar as portas existentes sob a obrigação já escrita é técnico. |
| **D5** | **Sem pergunta nova; consumidores técnicos abertos.** Não tratar pendência/recusa como “aguarda metade”. | Estado explícito foi superado (`app/docs/BACKLOG.md:1930`); fonte e escritor únicos são lei (`CLAUDE.md:20`, `:32`). Pendência grava motivo (`app/chamados/services/materializacao.py:850`, `:851`); contestação humana também grava motivo (`app/chamados/services/disputa_emissao.py:1497`); COLISÃO pode encerrar identidade (`app/chamados/services/materializacao.py:1093`, `:1099`). Separar esses usos é técnico. |
| **D6** | **Autoridade fechada; migração e causalidade histórica abertas.** | C7 é juiz único por corte expresso (`app/docs/CORTES.md:272`), registrado em `app/core/juizes.py:279`; sua regra completa está em `app/chamados/models.py:1853`. Explicador só consulta fio vivo (`app/chamados/services/explicador.py:137`), relatório publica PRONTA_FECHAR sem vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Migrá-los é técnico. Autor automático = autor do último ato com motivo próprio é decisão técnica explícita no novo aval: **[nao verificado por arquivo:linha]**. Razão histórica de cada DS continuar aberta: **[nao verificado]**. |
| **D7** | **Lei fechada; avaliação/correção técnica aberta.** | Universo apurável exclui retratada: L-025 (`app/docs/LEIS.md:76`); papel vem do marco, não do tipo gravado: L-087 (`app/docs/LEIS.md:119`). A guarda atual lê `Batida.objects` e compara `.tipo` (`app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`). Isso não autoriza remover a guarda sem definir/testar comportamento pela autoridade existente; solução é técnica. |
| **D8** | **Pergunta de lei ainda aberta.** Quando BATIDA do acervo sem episódio representa o mesmo fato de BATIDA com episódio, permitindo retratação entre ambas? | O código declara literalmente a lei aberta e conserva o acervo (`app/chamados/services/fio.py:1274`, `:1275`, `:1294`, `:1295`). O plano também protege mérito (`:1290`, `:1291`) e episódios distintos (`:1287`, `:1292`, `:1293`). O novo aval sobre pares da pausa não responde a essa identidade: **[nao verificado por arquivo:linha quanto ao alcance do anexo]**. |

Referências dos agregados antigos:

- **77 colab-dias**, 48 colabs e 18 na competência 11 estão registrados em `app/docs/BACKLOG.md:1882`.
- **18 DS com pode_fechar=True**, 77 pares e 848 duplicadas aparecem na descrição da raiz em `app/docs/BACKLOG.md:1883`.
- **486 grupos/848 excedentes em disputa aberta** estão registrados em `app/docs/BACKLOG.md:1887`.

Essas linhas documentam a medição anterior; não demonstram equivalência entre seu universo e o da sonda recebida. (`app/docs/BACKLOG.md:1882`, `:1883`, `:1887`; equivalência dos universos: **[nao verificado]**.)

## 11/11 09:26 · sessao d8f1db422342 · FIO-E0-V4-E4B · construtor · origem=Code

Corrigi a fixture em [test_e4b_c4_orm.py:375](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:375). **GREEN ORM e morte ORM de `sem_rollback` continuam [nao verificado].**

1. **A deficiência demonstrada é a falta de protesto cobrável na fixture.** A lavra sem `codigos` grava `protestos=[]` (`ponto/portas/celula.py:1011`); o c2 recusa criar o chamado sem esse protesto (`chamados/reconciliador.py:867`). A assertion exige pergunta pendente **com destino**, portanto sua falha não demonstra ausência de qualquer pergunta (`chamados/tests/test_e4b_c4_orm.py:429`).

   O aviso é impresso em `chamados/services/declaracao_texto.py:71`, chamado pela abstenção em `chamados/services/disputa_emissao.py:911`. O aviso fornecido nomeia 21/09, dia de P90; o vizinho é 02/09 (`chamados/tests/test_e4b_c4_orm.py:21`, `chamados/tests/test_e4_backfill.py:28`). Pelo ponto alcançado na falha fornecida, as checagens de vizinho resolvido e ausência de rollback integral já haviam passado (`chamados/tests/test_e4b_c4_orm.py:426`).

2. **A fixture agora lavra `FURO_PARCIAL` e verifica protesto persistido e ata legível**, incluindo a entrada apagada (`chamados/tests/test_e4b_c4_orm.py:375`). Permanecem a precondição de persistência e a assertion-alvo de P90 (`chamados/tests/test_e4b_c4_orm.py:429`, `chamados/tests/test_e4b_c4_orm.py:434`).

3. **Defeito de produção perdendo escrita aceita: [nao verificado].** A correção ficou nos testes. O novo ensaio executa a chamada extraída da fixture, a lavra real e o corpo inteiro do c2; suas bordas simuladas não demonstram persistência ou sinais ORM ([test_e4b_c4v11_memoria.py:1](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v11_memoria.py:1)). [nao verificado]

Diff central:

```diff
-        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo', ata=self.cel.ata)
+        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo',
+                                   codigos=['FURO_PARCIAL'], ata=self.cel.ata)
```

O diff também acrescenta as precondições em `test_e4b_c4_orm.py:377`, o novo RED com três mutantes em `test_e4b_c4v11_memoria.py:31` e sua inclusão no roteiro em [e4b_c4_verificar.py:68](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:68).

Resultados locais observados:

| Verificação | rc | Resultado / referência |
|---|---:|---|
| RED antes da correção | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| Memória completa após alterações | 0 | 90 testes; comando abaixo | [nao verificado]
| `--selos` | 0 | 40 testes; `e4b_c4_verificar.py:238` | [nao verificado]
| `--orm-preflight` | 0 | 8 enxertos; `e4b_c4_verificar.py:78` |
| `sem_protestos` | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| `protesto_nao_cobravel` | 1 | Mesma assertion; `test_e4b_c4v11_memoria.py:114` |
| `roteiro_sem_fixture` | 1 | Assertion de inclusão; `test_e4b_c4v11_memoria.py:52` |
| `sem_rollback`, memória | 1 | Assertion-alvo de P90; `test_e4b_c4v10_memoria.py:89` |
| 24 mutantes c4v7 | 1 cada | `AssertionError`, sem erros; catálogo em `test_e4b_c4v7_memoria.py:441` |
| GREEN / mutantes ORM | — | [nao verificado] |

Comandos reproduzíveis:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4b_classe_e_memoria \
  chamados.tests.test_e4b_c4_tranca_memoria \
  chamados.tests.test_e4b_c4v7_memoria \
  chamados.tests.test_e4b_c4v10_memoria \
  chamados.tests.test_e4b_c4v11_memoria \
  chamados.tests.test_e4b_c4_base \
  chamados.tests.test_e4b_c4_orm_protocolo -q

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante sem_protestos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante protesto_nao_cobravel
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante roteiro_sem_fixture
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v10_memoria --mutante sem_rollback
```

Para a sessão executar o ORM, primeiro o GREEN isolado, depois `sem_rollback` e o roteiro completo. A assertion esperada do mutante permanece em `chamados/tests/test_e4b_c4_orm.py:435`; os resultados abaixo são [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

E4B_C4_MUTANTE=sem_rollback PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
```

## 11/11 09:19 · sessao 492833d88911 · RAIZ-DA-DISPUTA · censo · origem=Code

**F3/F4 têm oito decisões identificadas e uma pergunta de lei: a identidade entre BATIDA do acervo sem episódio e BATIDA com episódio.** A aplicação conjunta do par, a espera pela outra metade, o estado derivado e a autoridade C7 já têm decisão registrada; suas divergências atuais são de implementação. (`app/chamados/services/fio.py:1275`; `app/docs/BACKLOG.md:1929`, `:1930`; `app/docs/CORTES.md:272`.)

Identidade Git da cópia com o hash informado: **[nao verificado]**. Estado atual de col960 e das perguntas 51522/51523: **[nao verificado; aguarda sonda]**. Os anexos F0/F1 permanecem como base, sem novo levantamento de nascimento, dona ou destino nesta entrega. **[nao verificado por arquivo:linha: referência aos anexos recebidos]**

Os números fornecidos são **77 colab-dias**, **486 grupos/848 excedentes** e **18 DS com `pode_fechar=True`**; não constituem partição por causa, transição ou decisão. A ligação dos 77 e dos 18 à obra está registrada; equivalência dos 486/848 ao universo do código e distribuição de todos esses números: **[aguarda sonda]**. (`app/docs/BACKLOG.md:1882`, `:1883`; chave dos grupos: `app/chamados/services/fio.py:1923`, `:1945`, `:1976`.)

O censo abaixo usa os seguintes prefixos. Cada arquivo da primeira coluna, combinado com cada número das demais colunas, constitui uma referência `arquivo:linha`.

| Prefixo | Caminho |
|---|---|
| C | `app/chamados/` |
| S | `app/chamados/services/` |
| Q | `app/chamados/management/commands/` |
| H | `app/templates/chamados/partials/` |
| P | `app/ponto/` |
| T | `app/chamados/tests/` |

**DENTRO** significa `C/models.py` ou `S/`, conforme o perímetro pedido; **FORA** compreende juízes, reconciliador, commands, views, templates e `bin/`. Essa classificação é por localização, não uma prova de que todo método localizado em `models.py` execute pela raiz DS. (`app/chamados/models.py:1848`, `:3103`; `app/chamados/reconciliador.py:340`.)

Os nomes reais e as distinções de estado são:

| Pedido | Nome real / natureza |
|---|---|
| respondida | Coluna `PerguntaDisputa.respondida_em`; `respondida` é **propriedade**, definida por timestamp não nulo. (`C/models.py:2448`, `:3496`, `:3498`.) |
| validada | Coluna `PerguntaDisputa.validada_em`, acompanhada de autor nas portas. (`C/models.py:2449`; `S/validacao.py:134`, `:135`.) |
| materializada | Coluna `PerguntaDisputa.materializada_em`, acompanhada de autor; seu help text fala em batida criada, mas portas também a usam para encerramento sem plantio. (`C/models.py:3393`, `:3395`; `S/materializacao.py:924`, `:1099`.) |
| via | Coluna `PerguntaDisputa.via_resolucao`. (`C/models.py:2561`.) |
| pendência | Coluna `PerguntaDisputa.pendencia_admin`; não é via de encerramento. (`C/models.py:2556`, `:2558`.) |
| recusa | Coluna `PerguntaDisputa.recusa_motivo`. (`C/models.py:2560`.) |
| pode_fechar | **Método derivado de `DisputaSupervisao`, não coluna nem property**: ausência de resposta sem veredito **e** ausência de fio vivo. (`C/models.py:1848`, `:1853`.) |
| estado | **Propriedade derivada da pergunta**; traduz validação, materialização, resposta e C6. (`C/models.py:3033`, `:3041`, `:3044`, `:3048`.) |

O AST foi executado por `python3 -B` em stdin, com revisão dos fluxos dinâmicos; não foi gravado artefato de execução. **[nao verificado por arquivo:linha quanto à execução]** A unidade das tabelas é **uma linha por `(campo, arquivo)`**, reunindo os sítios desse arquivo. Portanto as contagens abaixo são linhas das tabelas, não funções, atribuições ou registros de produção. **[nao verificado por arquivo:linha; reproduzível pelo script ao fim]**

| Campo | Linhas da tabela |
|---|---:|
| `respondida_em` | 33 |
| `validada_em` | 22 |
| `materializada_em` | 23 |
| `via_resolucao` | 21 |
| `pendencia_admin` | 6 |
| `recusa_motivo` | 12 |
| `pode_fechar`, incluindo derivações concorrentes | 11 |
| Total | 128 |

Nas tabelas, **m** é atribuição em memória; **P** é persistência associada; **W** é inserção/update ou delegação de escrita. A restauração em memória e o proxy de ressuscitação estão separados das escritas persistentes. (`C/models.py:1507`, `:1508`, `:3152`, `:3159`, `:3264`.)

O levantamento de **`respondida_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3132/P3152; **só memória** m1508,3163 | 1564,1844,3045,3048,3127,3131,3348,3498,3924,3931,3932,3933 |
| `S/disputa_emissao.py` — DENTRO | resposta m1468/P1508; correção W1630,1632; automática m1653/P1655; trilha m1674/P1676 | 1619,1670,1712,1856,1929,1946 |
| `S/acoes_disputa.py` — DENTRO | — | 95,130 |
| `S/becos.py` — DENTRO | — | 30 |
| `S/cobranca.py` — DENTRO | — | 228 |
| `S/declaracao_texto.py` — DENTRO | — | 417,419,687 |
| `S/fio.py` — DENTRO | — | 340,660 |
| `S/furo_sem_canal.py` — DENTRO | — | 80 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 669,1369 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot dinâmico83 |
| `S/pergunta_no_app.py` — DENTRO | — | 71 |
| `S/raiox.py` — DENTRO | — | 138 |
| `S/validacao.py` — DENTRO | — | 393,576,581,924 |
| `S/verbos.py` — DENTRO | — | 309,312 |
| `S/vigia_fio.py` — DENTRO | — | 675,783 |
| `app/api/views.py` — FORA | — | 1172,1184,2568,2582,2625,2642 |
| `app/api/views_mensageria.py` — FORA | — | 569,570 |
| `C/juizes.py` — FORA | — | 829,1341,1449,1594,1601,1604,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 177,592,1105,1572,1602 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60 |
| `Q/curar_perguntas_em_limbo.py` — FORA | — | 46,69 |
| `C/relatorios_disputa.py` — FORA | — | 21 |
| `C/veredito_lavrado.py` — FORA | — | 206,208 |
| `C/views.py` — FORA | — | 1280,1416 |
| `C/views_cobrar.py` — FORA | — | 169,175 |
| `P/management/commands/reconciliar_grade.py` — FORA | — | 460 |
| `P/supra_juiz.py` — FORA | — | 545,548 |
| `H/_card_atendimento.html` — FORA | — | 94,100,137,155,384,385 |
| `H/_pergunta_card.html` — FORA | — | 24,29,32 |
| `H/modal_fio.html` — FORA | — | 185,217,220 |
| `bin/censo_folha.py` — FORA | — | filtros28,29 |

O levantamento de **`validada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3135/P3152; **só memória** m1507,3164 | 309,344,1564,1843,3041,3127,3134,3359,3924,3935,3936,3937 |
| `S/disputa_emissao.py` — DENTRO | resposta nova m1480/P1508; correção W1630,1632 | 1470,1471,1620,1917 |
| `S/validacao.py` — DENTRO | `_carimbar_validacao` m134/P137; devolução m199/P202 | 139,147,188,195,345,396,472,573,732,753 |
| `S/acoes_disputa.py` — DENTRO | — | 345 |
| `S/declaracao_texto.py` — DENTRO | — | 227 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 670,1369 |
| `S/painel.py` — DENTRO | — | 67,270,271,272,273 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/verbos.py` — DENTRO | — | 239,246 |
| `S/vigia_carimbo.py` — DENTRO | — | 97 |
| `C/juizes.py` — FORA | — | 829,1325,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 205,1075,1572,1602,1765,1778 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60,100 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67 |
| `C/reconciliador.py` — FORA | — | 627,1048 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `H/_pergunta_card.html` — FORA | — | 20,23,42 |
| `H/modal_fio.html` — FORA | — | 228 |
| `bin/censo_folha.py` — FORA | — | 29 |

O levantamento de **`materializada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3099; portas P3152,3190,3212; lote W3264 | 1483,1843,1925,3043,3046,3056,3126,3182,3183,3284,3296,3313,3350,3352 |
| `S/disputa_emissao.py` — DENTRO | — | 226,762,1070,1089,1323,1621,1823,1917 |
| `S/fabrica_da_celula.py` — DENTRO | — | 170 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 434,532,623,627,632,671,1089,1093,1369,1418 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/raiox.py` — DENTRO | — | 135,175 |
| `S/relavra_via.py` — DENTRO | — | 181,244 |
| `S/validacao.py` — DENTRO | — | 188,190,461,472,490 |
| `C/juizes.py` — FORA | — | 288,662,689,831,1336,1362,1368,1399,1417,1949 |
| `Q/auditar_disputas.py` — FORA | — | 69,148 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 470,538,587,905,955,1075,1156,1572,1602,1765 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,325 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 147 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,88,91,129,142 |
| `C/reconciliador.py` — FORA | — | 366,627,1048,1144,1154,1500 |
| `C/veredito_lavrado.py` — FORA | — | 202,204 |
| `C/views.py` — FORA | — | 1257 |
| `P/management/commands/reconciliar_fantasmas.py` — FORA | — | 211 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `P/services/veredito_celula.py` — FORA | — | 195 |

O levantamento de **`via_resolucao`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3101; portas P3152,3190,3212; lote W3264; `encerrar_por_fato_no_espelho` m3275/P3276 | 1483,1920,2054,2060,2722,2755,3055,3126,3201,3264,3273,3284,3296,3313,3320,3350,3352,3485,3491 |
| `S/relavra_via.py` — DENTRO | **`relavrar` m125/P127** | 118,121,123,153,181,245 |
| `S/competencia_trancada.py` — DENTRO | — | 251 |
| `S/disputa_emissao.py` — DENTRO | — | 327,398,417,436,1927,1942,1945 |
| `S/explicador.py` — DENTRO | — | 95 |
| `S/fabrica_da_celula.py` — DENTRO | — | 148 |
| `S/fio.py` — DENTRO | — | 1099,1166,1243,1350,1431,1852,1888,1910,1954 |
| `S/materializacao.py` — DENTRO | — | 438,623,668,1089 |
| `S/passivo_validado.py` — DENTRO | — | 83,140,141,142,515 |
| `S/tarefa_do_dia.py` — DENTRO | — | 227,228 |
| `C/juizes.py` — FORA | — | 662,674,830,1336,1362 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 202,206,433,542,955,1710 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,314,330,398,504 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31,42,49,66,76 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 70,71,72 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,129,142,160 |
| `Q/relavrar_via_i6.py` — FORA | — | 58,59,62,66,68,88 |
| `C/reconciliador.py` — FORA | — | 1143,1154 |
| `P/services/cartorio.py` — FORA | — | 1610 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `H/_card_atendimento.html` — FORA | — | 186 |

O levantamento de **`pendencia_admin`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; default2556 | — |
| `S/materializacao.py` — DENTRO | `_materializar_resposta_hora` m850/P851 | — |
| `S/explicador.py` — DENTRO | — | 85,90 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 1112 |
| `H/_card_atendimento.html` — FORA | — | 183 |
| `H/modal_fio.html` — FORA | — | 222,225 |

O levantamento de **`recusa_motivo`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `carimbar_identidade` m3188/P3190 | — |
| `S/disputa_emissao.py` — DENTRO | contestação m1497/P1508 | — |
| `S/materializacao.py` — DENTRO | pendência m850/P851; recusa m1102/P1103 | refresh1089; comparação1092 |
| `S/validacao.py` — DENTRO | recusa admin m584/P590 | 147,188,192,208,225,228,582,604 |
| `S/explicador.py` — DENTRO | — | 90 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 535,1075,1112 |
| `P/services/flip_auto.py` — FORA | — | regex132 |
| `H/_card_atendimento.html` — FORA | — | 186 |
| `H/modal_fio.html` — FORA | — | 226 |
| `bin/sombra_mascara_pessoal.py` — FORA | `setattr`57; `bulk_update`62,64 | carregamento51; `getattr`54; comparação56 |
| `bin/sombra_selo_pessoal.py` — FORA | — | SQL77 |

A escrita externa da máscara é efetivamente da pergunta: o inventário inclui `PerguntaDisputa.recusa_motivo`; o programa exige banco `sombra`. Ela não deve ser confundida com escritor ordinário de produção, nem omitida do censo de formas de escrita. (`bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:10`, `:57`, `:62`.)

O levantamento de **`pode_fechar` e decisões concorrentes de fechamento** é:

| Arquivo / fronteira | Definição/derivação | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | juiz `DisputaSupervisao.pode_fechar`1848,1853 | — |
| `S/explicador.py` — DENTRO | bool parcial137; publicação163 | uso local138,139 |
| `C/relatorios_disputa.py` — FORA | projeção concorrente `PRONTA_FECHAR`20 | C6 em18 |
| `S/becos.py` — DENTRO | — | 33,56 |
| `S/competencia_trancada.py` — DENTRO | — | 153,202 |
| `S/desenlace.py` — DENTRO | — | `dict.get`63 |
| `S/disputa_emissao.py` — DENTRO | — | 1278,1963 |
| `S/materializacao.py` — DENTRO | — | 585,1316 |
| `C/reconciliador.py` — FORA | — | 411,611,1170 |
| `P/services/entrada_sincronizada.py` — FORA | — | 58 |
| `H/modal_fio.html` — FORA | — | 435,446 |

As indireções relevantes para interpretar essas tabelas são:

| Forma | Resultado conferido |
|---|---|
| `update(**campos)` | `reabrir_para_correcao` monta resposta/validação nulas e texto vazio em1593; persiste diretamente em1632 ou entrega ao descarimbo em1630. (`S/disputa_emissao.py:1593`, `:1630`, `:1632`.) |
| `update(..., **outros)` | O descarimbo escreve materialização/autor/via e aceita campos adicionais; os chamadores atuais passam resposta/validação. A guarda veta apenas o trio do carimbo em `outros`. (`C/models.py:1566`, `:3255`, `:3266`; `S/disputa_emissao.py:1593`.) |
| `save(update_fields=lista)` | A resposta salva a lista montada em1508; carimbo salva em3152; identidade em3190; descarimbo em3212. A string na lista é mecanismo de persistência, não leitura do valor. (`S/disputa_emissao.py:1508`; `C/models.py:3152`, `:3190`, `:3212`.) |
| `bulk_update` próximo ao domínio | `gravar_lavras_em_lote` escreve somente `veredito_lavrado`, `julgado_em`, `janela_expira_em`; não escreve os seis campos acima. (`C/veredito_lavrado.py:237`, `:250`.) |
| `setattr` genérico | A porta de texto permite título/assunto/descrição; `core.reexibir` altera memória. Não são prova de escrita persistente desses estados. (`S/texto.py:18`, `:25`, `:33`; `app/core/reexibir.py:45`, `:49`.) |
| `F()` | O enumerador inclui `F` como leitor de campo; escrita dos alvos por `F()` não foi identificada nos sítios classificados. Ausência universal por expressões construídas: **[nao verificado]**. |
| SQL | A máscara/selo foram discriminados; o SQL do reconciliador aberto é advisory lock/unlock. SQL dinâmico arbitrário escrevendo os alvos: **[nao verificado]**. (`bin/sombra_selo_pessoal.py:77`; `C/reconciliador.py:1235`, `:1242`.) |
| Signals | `update_fields` é lido como metadado de disparo; o signal emite evento e agenda beco. Não é leitura do conteúdo de cada coluna. (`C/signals.py:227`, `:238`, `:255`, `:257`.) |
| Estado derivado | API serializa `p.estado`; explicador, fio e vigia também o consomem. São leitores indiretos dos campos/juízes já discriminados. (`app/api/views.py:2573`; `S/explicador.py:72`; `S/fio.py:614`; `S/vigia_fio.py:311`.) |

As migrations ficam fora das 128 linhas de runtime:

| Campo | Schema / leitura executável |
|---|---|
| resposta/validação | `C/migrations/0014_perguntadisputa.py:25`, `:26` |
| materialização | `C/migrations/0016_s84_pergunta_chamado_destino.py:23` |
| pendência | `C/migrations/0043_hx_pendencia_admin.py:15` |
| recusa | `C/migrations/0040_a27_recusa_motivo.py:15` |
| via | criação `C/migrations/0033_perguntadisputa_via_resolucao.py:15`; alterações em `0035_via_admin_decidiu.py:15`, `0036_a1_pergunta_celula.py:22`, `0037_a3_estado_honesto.py:31`, `0040_a27_recusa_motivo.py:20`, `0041_b2_via_legado_sem_lastro.py:15`, `0042_hx_via_ausencia_dia.py:15`, `0046_via_celula_absolveu.py:15`, `0048_via_competencia_trancada.py:14`, `0051_decisao_pergunta.py:110` |
| via, leitura de universo | `C/migrations/0053_pergunta_unica_por_familia.py:32` |

Completude contra reflexão, receivers de tipo desconhecido, SQL montado e todos os consumidores transitivos: **[nao verificado]**. O instrumento conserva uma fila de revisão dessas formas; não transforma ausência de literal em prova de ausência.

A matriz seguinte descreve **comportamento da cópia**, não a execução em produção. Os testes foram inspecionados; execução da suíte nesta sessão: **[nao verificado]**. “Sem teste” significa cobertura específica não identificada nesta leitura, não ausência global certificada. **[nao verificado quanto à cobertura global]** [nao verificado]

| ID / porta e entrada | Juiz consultado | Operação | Persistido | Retorno | Prova |
|---|---|---|---|---|---|
| **T01 responder**, entrada admissível | Guardas por tipo/opções; porta de descarimbo | Core de resposta | Texto, `respondida_em`, decisão RESPOSTA; eventual descarimbo | `(True,None)` | `S/disputa_emissao.py:1433`, `:1467`, `:1485`, `:1508`, `:1511`, `:1559`; `T/test_reresposta_invalida_validacao.py:67` |
| **T02 responder novamente**, tinha validação | Mesmas guardas | Invalida assinatura anterior | Nova resposta; validação/autor nulos; histórico conservado | Mesmo contrato | `S/disputa_emissao.py:1470`, `:1480`, `:1511`; `T/test_reresposta_invalida_validacao.py:55` |
| **T03 validar**, sem pergunta/resposta ou já validada; implausível sem confirmação | Filtro PK/DS; carimbos; `motivo_implausivel_validacao` | Recusa antes da escrita | Nada por esses ramos | `nao_encontrada`, `sem_resposta`, `ja_validada`, `implausivel` | `S/validacao.py:385`, `:393`, `:396`, `:404`; sem teste específico |
| **T04 validar hora admissível** | Implausibilidade; materializador individual | Valida **uma P**, antes do laço | Validação/autor e decisão VALIDAÇÃO; efeitos da unidade | `validada` ou `ja_no_espelho` | `S/validacao.py:134`, `:139`, `:435`, `:461`, `:463`, `:515`, `:517`; `T/test_o261_recusa_fala.py:135` |
| **T05 validar**, materialização recusada sem encerramento e com motivo | `_resposta_de_hora`; carimbo nulo; motivo | Devolve validação individual | Limpa validação/autor, mantém resposta, registra DEVOLUÇÃO; pode reabrir DS | `nao_gravou`, `devolvida=True`; repetição pode dar `None` interno | `S/validacao.py:188`, `:199`, `:205`, `:217`, `:473`, `:499`; `T/test_o261_recusa_fala.py:87`, `:148`, `:164` |
| **T06 validar**, recusa já encerrou identidade | Devolução lê materialização | Não desfaz validação | Encerramento permanece | Devolução `False`; resultado final depende dos erros | `S/validacao.py:190`, `:490`; `T/test_o261_recusa_fala.py:190` |
| **T07 materializar lote normal** | Fila por timestamps; unidade; C7 na cauda | Itera **uma P por vez** | Efeitos individuais; eventual fecho da DS | Dict de materializadas, reusos, pendentes, fecho e erros | `S/materializacao.py:1369`, `:1375`, `:1378`, `:1381`; `T/test_o317_passivo_pela_porta.py:188`, `:481` |
| **T08 materializar**, dias divergem sem conferência | `fio.data_do_fato_de` | Recusa nomeada | `recusa_motivo`; nenhuma batida desse ramo | `False` | `S/materializacao.py:1191`, `:1193`, `:1213`, `:1214`; sem teste específico |
| **T09 hora sem texto humano** | Guarda de anuência | Recusa antes de derivar horário | Motivo na P/fio | Ramo `None`; unidade `False` | `S/materializacao.py:829`, `:836`, `:841`, `:1227`; sem teste específico aberto |
| **T10 tipo não conferido** | `entrada_de_marco`, grade/célula | Usa tipo disponível pelo motivo e registra pendência | `pendencia_admin` + motivo; pode prosseguir | Não é recusa obrigatória | `S/materializacao.py:386`, `:395`, `:397`, `:850`, `:851`; sem teste específico |
| **T11 batida já no slot** | `_batida_existente_na_janela` | Reusa; anota origem; reconcilia | Pode alte

[truncado em 20000 de 49203 caracteres]

## 11/11 09:15 · sessao 5d9a4f9c90fa · FIO-E0-V4-E3 · construtor · origem=Code

Cura aplicada somente em `chamados/tests`; os arquivos alterados e novos estão registrados em [e3fix11_execucao.txt:132](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:132). [nao verificado]

O censo dos harnesses ficou assim:

| Executor | Como monta o namespace | Mesmo furo? |
|---|---|---|
| S2/S3 | Dicionário manual; `funcao()` compila somente a função solicitada, sem copiar globais. [helper:14](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e4_apoio.py:14) | Sim; corrigido carregando a constante e o leitor reais. [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147) | [nao verificado]
| `test_e3_nb_impressao_memoria` | Seleção fixa por AST, já incluindo função e constante. [teste:173](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:173) | Não. | [nao verificado]
| `x1_mutantes --sem-banco` | Reutiliza o harness anterior. [executor:77](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:77) | Não. |
| `x1_mutantes`, instalação ORM | Executa definições em `modulo.__dict__`, incluindo função e constante. [executor:100](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:100) | Não pelo código; execução ORM [nao verificado]. |
| `e3_pos_suite_sem_banco` | Invoca o próprio S2/S3; herdava o defeito. [executor:78](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_pos_suite_sem_banco.py:78) | Corrigido pela mesma alteração. |

Os outros testes que selecionam `_julgar_colab_corpo` por AST apenas inspecionam sua estrutura: [test_o219:217](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_o219_versao_da_regra.py:217), [test_e3_isento:112](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_isento_na_impressao.py:112). [nao verificado]

Com `cobertura='aprovada'`, **a expectativa original permanece**: `pulados=1`, sem rejulgamento, preservando impressão e veredito anteriores. O caso usa 07/11/2026, dentro do horizonte; porém a cobertura integral exclui o marcador X1. A impressão está deliberadamente cega à cobertura e a chamada usa `apply_=False`. Portanto, nenhuma assertion precisou ser relaxada. [teste:127](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:127), [cartorio:1229](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1229), [teste:169](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:169). A cobertura visível continua tendo seu caso separado de rejulgamento para `concorde`. [memória:414](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:414) [nao verificado]

O diff da cura acrescenta estas linhas, preservando a assertion original:

```diff
+                ns['HORIZONTE_SEM_ADESAO'] = literais('ponto/services/cartorio.py')['HORIZONTE_SEM_ADESAO']
+                funcao('ponto/services/cartorio.py', 'sem_adesao_no_horizonte', ns)
+                self.assertIn('sem_adesao_no_horizonte', ns.keys(),
+                              'harness deve carregar o leitor real do cartorio')
+                self.assertIn('HORIZONTE_SEM_ADESAO', ns.keys(),
+                              'harness deve carregar a declaracao lida pelo leitor')
+                leitor = ns['sem_adesao_no_horizonte']
+                # Corte default 21: 20/10 ainda e comp10; o dia do caso (07/11) e comp11.
+                self.assertFalse(leitor(dt.date(2026, 10, 20)))
+                self.assertTrue(leitor(dia))
...
+                self.assertEqual(out['nunca_bateu'], 0)
+                self.assertEqual(cel.impressao, legado)
+                self.assertEqual(cel.veredito, 'nunca_bateu')
```

Fonte: [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147). O novo [executor de mutantes:18](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_namespace_mutantes.py:18) remove cada dependência ou substitui o leitor por constantes, somente em memória. [nao verificado]

Cada link abaixo contém o **comando exato**, RC e resultado:

| Execução | RC | Resultado |
|---|---:|---|
| [Suíte solicitada](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:14) | 0 | **31 OK** | [nao verificado]
| [X1 green](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:19) | 0 | 24 OK |
| [X1 sem_horizonte_x1](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:25) | 1 | Assertion; ERROR=0 |
| [X1 horizonte_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:32) | 1 | Assertion; ERROR=0 |
| [X1 literal_incondicional](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:39) | 1 | Assertion; ERROR=0 |
| [X1 sem_fonte_no_gancho](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:46) | 1 | Assertion; ERROR=0 |
| [X1 so_acusador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:53) | 1 | Assertion; ERROR=0 |
| [X1 parcial_tira_marcador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:60) | 1 | Assertion; ERROR=0 |
| [RED sem_leitor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:67) | 1 | Dois subcasos por assertion |
| [sem_declaracao](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:74) | 1 | Dois subcasos por assertion |
| [leitor_constante_true](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:81) | 1 | Dois subcasos por assertion |
| [leitor_constante_false](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:88) | 1 | Dois subcasos por assertion |
| [declaracao_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:95) | 1 | Dois subcasos por assertion |
| [Mutantes S2/S3 existentes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:102) | 1 cada | Assertion; ERROR=0 |
| [Pós-suite e selos](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:114) | 0 | 7+1+15+35 OK | [nao verificado]
| [Ruff](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:121) | 127 | Indisponível |
| [Diff check](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:129) | 0 | Sem apontamentos |

`status_aprovada_tira_marcador` exige ORM e não possui alvo sem banco. [x1_mutantes:122](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:122). Certificação Code/opus final: [nao verificado].

_3766 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
