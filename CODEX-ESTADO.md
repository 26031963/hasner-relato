# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 09:28** · publicado em **12/11/2026 09:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 09:28 · sessao b63c886da997 · - · - · origem=Ronald (tmux)

- **`bater_ponto.html` não é o template da rota `/ponto/bater/`.** A URL `bater/` chama `views.bater_ponto`; `bater-v2/` chama a mesma view com `ui_v2=True` (`app/ponto/urls.py:11-12`). A view renderiza `ponto/bater_ponto_v2.html` no ramo v2; no outro ramo usa `_template_triagem(context)`, que pode devolver `ponto/triagem/apto.html` (`app/ponto/views.py:225-235`, `app/ponto/views.py:356-358`).

- **As referências executáveis literais encontradas ao arquivo apagado são verificadores de fonte:** `app/scripts/check_a_lei_batida.py:20-23` e `app/tools/check_frontend_urls.py:29`. O patch fornecido remove exatamente essas entradas, além de apagar o template; não altera `ponto/views.py`, `ponto/urls.py` nem `sw.js` (anexo `diff_v6_vs_pre.patch`, hunks desses arquivos). Referências dinâmicas não identificadas pela busca literal: [nao verificado].

- **O template apagado nunca esteve nessa lista de precache.** `STATIC_CACHE` contém somente `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png` e `/static/icons/icon-512.png` (`app/static/sw.js:3-8`). O `cache.addAll` usa essa lista (`app/static/sw.js:10-13`). A remoção de `bater_ponto.html` não remove entrada alguma dela; o patch não modifica `sw.js` (anexo `diff_v6_vs_pre.patch`).

- **Não há redirect offline para `bater_ponto.html`.** O fallback do SW procura a própria request e devolve `Response.error()` se não encontra cópia (`app/static/sw.js:29-30`, `app/static/sw.js:45-50`). O redirect após sucesso offline do apto aponta para `core:dashboard`, assim como seu link “Ir para o início” (`app/templates/ponto/triagem/apto.html:115`, `app/templates/ponto/triagem/apto.html:292-299`). O patch mantém o destino `core:dashboard` no hunk `@@ -263,41 +270,41 @@`.

- **A navegação HTML retorna antes do trecho que gravaria home/bater no cache.** `mode === 'navigate'` executa fetch com fallback e retorna (`app/static/sw.js:45-50`); o `comCache` com `cache.put` aparece depois (`app/static/sw.js:67-79`). Assim, esse `cache.put` não é atingido pelas navegações usuais; aparência efetiva do erro de rede no Safari: [nao verificado].

- **A fila offline usa IndexedDB, não uma chave de localStorage:** banco `juliani-offline`, versão `2`; store `batidas_pendentes`, chave primária `id` com `autoIncrement` (`app/static/pwa.js:50-54`). O escritor insere com `add` (`app/static/pwa.js:107-122`); sync lê por `getAll` (`app/static/pwa.js:126-134`), remove após sucesso (`app/static/pwa.js:158-164`) e também após HTTP 400 (`app/static/pwa.js:166-174`); indicador lê `count` (`app/static/pwa.js:184-192`).

- **Os chamadores de escrita pre-v6 são:** v2 no catch de rede (`app/templates/ponto/bater_ponto_v2.html:333`), v2 explicitamente offline (`app/templates/ponto/bater_ponto_v2.html:346`), apto offline (`app/templates/ponto/triagem/apto.html:282`) e os dois caminhos do template apagado, sem renderizador encontrado (`app/templates/ponto/bater_ponto.html:543`, `app/templates/ponto/bater_ponto.html:597`; cadeia de renderização em `app/ponto/views.py:356-358`).

- **O SW pede sincronização; quem acessa a fila é pwa.js.** O SW envia `SYNC_BATIDAS` aos clientes abertos (`app/static/sw.js:112-125`); pwa.js recebe a mensagem e chama `sincronizarBatidas` (`app/static/pwa.js:15-16`). Também dispara sync ao voltar online e no bootstrap online (`app/static/pwa.js:320`, `app/static/pwa.js:328`).

- **Na v6, nomes e identidade da fila permanecem; muda a espera da escrita.** O patch mantém `indexedDB.open('juliani-offline', 2)` e criação de `batidas_pendentes` no hunk `@@ -45,9 +45,32 @@`; troca a gravação direta pela função `_transacao`, com `complete`, `error` e `abort`, no hunk `@@ -55,37 +78,80 @@`; limita o gap antes do `add` e mostra aviso enquanto o armazenamento permanece pendente no hunk `@@ -104,23 +170,138 @@`. Os leitores/deletores abaixo de `sincronizarBatidas` não são alterados por esse patch (mesmo hunk termina imediatamente antes dessa função).

- **Linhas v6 dos consumidores inalterados, calculadas pelo deslocamento `+181` do patch:** `sincronizarBatidas` começa em `app/static/pwa.js:307`, `getAll` fica em `:313`, deletes em `:343` e `:353`, `count` em `:371`; pre-v6 correspondentes `:126`, `:132`, `:162`, `:172`, `:190` (anexo `diff_v6_vs_pre.patch`, hunk `@@ -308,6 +489,9 @@`, e `app/static/pwa.js:126-190`).

## 12/11 09:28 · sessao 29bcabb8275e · - · - · origem=Ronald (tmux)

- A home do colaborador renderizada é `core/dashboard_colaborador_v2.html` (`app/core/views.py:270-271`); ela estende `base.html` (`app/templates/core/dashboard_colaborador_v2.html:1`).
- Seus cards são links HTML nativos: Bater (`:31`), Espelho (`:56`), Escala (`:60`), Confirmar Batidas (`:66`), Chamados (`:78`), Justificar (`:82`), Ausência (`:88`), Holerites (`:100`), Benefícios (`:104`), Férias (`:110`), todos em `app/templates/core/dashboard_colaborador_v2.html`. Não registram handlers em `pwa.js`; a navegação efetiva no iPhone é [nao verificado].
- Os botões “×” e “Mais tarde” do modal usam `onclick` inline para remover `onb-modal`, independentes de `pwa.js` (`app/templates/core/dashboard_colaborador_v2.html:126`, `:138`); modal presente/bloqueando cliques no aparelho é [nao verificado].
- A home antiga também usa link nativo para Bater (`app/templates/core/dashboard_colaborador.html:39`) e fecha modal por `onclick` inline (`app/templates/core/dashboard_colaborador.html:187`, `:199`).
- `pwa.js` carrega com `defer` em ambas as cascas (`app/templates/base.html:20`; `app/templates/base_app.html:20`). Ajuda/Minha conta têm registro próprio no script inline da casca (`app/templates/base.html:341`, `:345-348`, `:364-370`); dropdowns de `base_app` também têm script separado (`app/templates/base_app.html:99`, `:121-127`).
- `hasner-ui.js` intercepta clique somente em `.dropdown-toggle`; não é um interceptador universal dos cards (`app/static/js/hasner-ui.js:14-32`). O `<body>` das duas cascas é simples, sem `hx-boost` (`app/templates/base.html:91`; `app/templates/base_app.html:83`).
- Os botões diretamente criados por `pwa.js` são o convite de notificações: fechar (`app/static/pwa.js:358-364`) e ligar avisos (`app/static/pwa.js:365-374`). Seu nascimento depende de a inicialização ultrapassar `await calcularESalvarGap()` (`app/static/pwa.js:323-340`). A v6 troca essa espera por `_noTeto` no hunk do patch `app/static/pwa.js @@ -323,7 +507,8`.
- O relógio da home possui script próprio: offline chama fallback local e retorna (`app/templates/core/dashboard_colaborador_v2.html:143-160`, particularmente `:153`); `AbortController` está dentro de `try` (`:154-157`).
- Optional chaining já existia no `pwa.js` pré-v6: `event.data?.type` (`app/static/pwa.js:16`), `req.result?.valor ?? null` (`:78`), extração CSRF (`:140`, `:245`, `:261`, `:403`). Compatibilidade exata com a versão iOS do aparelho é [nao verificado].
- A inicialização assíncrona não é top-level `await`: é uma IIFE `async` (`app/static/pwa.js:323-346`). As chamadas a câmera introduzidas pela v6 estão em funções executadas sob `_noTeto`, conforme os hunks fornecidos; sua execução real no Safari é [nao verificado].

## 12/11 09:27 · sessao d20c4f297a46 · - · - · origem=Ronald (tmux)

Dois achados introduzidos:

1. **Feriado pode trocar a geradora sem mudança de vínculo.** O serviço percorre todos os vínculos do colaborador e regenera as mesmas datas por cada um. Com vínculos sobrepostos declarados, vence a ordem de iteração. (`ponto/services/cadastro_futuro.py:49`, `ponto/services/cadastro_futuro.py:22`, `ponto/services/cadastro_futuro.py:30`) A porta substitui a geradora pela EC recebida quando cobre a data. (`ponto/portas/celula.py:263`) Para esse evento, selecionar datas agrupadas pela geradora atual.

2. **Foto parcial pode produzir fase errada no 12x36.** A sincronização materializa folgas somente em datas com célula `gerada`. (`escala/folgas.py:607`, `escala/folgas.py:623`) Se a única folga do calendário no mês tiver célula editada, ou estiver além da janela de células existentes, ela desaparece da alimentação. O cálculo então pode retornar à âncora, embora a foto mensal devesse determinar a fase. (`escala/models.py:1280`, `escala/models.py:1289`, `escala/models.py:1353`) Preservar células editadas e não criar células fora da janela permite materializar a `FolgaDia` necessária ao cálculo mensal.

Reprodução dinâmica sem banco: **[nao verificado]**. O decorator de `_hpd_pares` foi excluído dos achados por ser preexistente, conforme informação do agente principal. **[nao verificado por diff]**

## 12/11 09:27 · sessao 3d52e2f9914e · - · - · origem=Ronald (tmux)

Não identifiquei erro de compatibilidade ORM nos arquivos novos: campos e tipos da lavra correspondem ao modelo (`ponto/services/vigia_cadastro_futuro.py:38-46`, `inteligencia/models.py:87-96`), os receivers são carregados (`core/apps.py:8-10`) e o HAIKU lê snapshots (`api/views_mensageria.py:647-648`).

Pontos pendentes concretos:

- Cron continua reescrevendo; o novo teste acusa explicitamente essa pendência (`ponto/portas/celula.py:870-877`, `ponto/tests/test_cadastro_futuro_sem_banco.py:619-628`).
- Falhas de regeneração ainda podem deixar cadastro salvo e futuro antigo: `executar_vinculo` captura a exceção do suplemento (`colaboradores/services/vinculo.py:324-332`); `salvar_tipo` salva antes de capturar a propagação (`escala/services/cadastro_tipo.py:306-323`). O dublê usa transações sem rollback (`ponto/tests/test_cadastro_futuro_sem_banco.py:218`).
- `lavrar()` não tem teste executável nos novos casos; o fio do command é verificado apenas por AST (`ponto/services/vigia_cadastro_futuro.py:38-46`, `ponto/tests/test_cadastro_futuro_sem_banco.py:583-597`).
- O fake ignora ordenação, e o teste HAIKU oferece somente um snapshot; escolher a medição mais recente não está demonstrado (`ponto/tests/test_cadastro_futuro_sem_banco.py:105`, `:575-580`).
- Os contadores novos não fornecem `rotulo`, embora o contrato documentado do endpoint espere esse campo (`ponto/services/vigia_cadastro_futuro.py:33-35`, `api/views_mensageria.py:630-631`).

Comportamento Django real, rollback e custo em frota: [nao verificado]. Nenhum arquivo editado nesta revisão.

## 12/11 09:11 · sessao 3d52e2f9914e · - · - · origem=Ronald (tmux)

A escrita futura do cron está em `ponto/portas/celula.py:870-877`; o command apenas delega em `ponto/management/commands/gerar_celulas.py:309-313`. Alterar só o command exigiria contornar a operação existente; o escritor único está declarado em `core/portas.py:107-113` e cobrado por `ponto/tests/test_chokepoint_celula_gate.py:48-85`.

O leitor HAIKU adequado é `api/views_mensageria.py:627-646`: `api_mensageria_saude` entrega snapshots prontos. Molde: `escala/services/vigencia_impossivel.py:65-74`, incluindo `total=None, sem_medida=True` antes da primeira lavra. URL existente: `api/urls.py:16`.

Django, freezegun e pytest estão ausentes no interpretador consultado; execução ORM permanece [nao verificado]. Não executar `config.settings.ci`: cria diretórios temporários fora de `app/` (`config/settings/ci.py:25-33`) e configura PostgreSQL (`config/settings/ci.py:6-17`).

Execuções observadas, sem alterações de arquivo ou banco:

- `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_reverter_regen_sem_banco ponto.tests.test_reverter_exportada_sem_banco -v`: **36 OK**, 9,824 s. Contratos sem Django: `ponto/tests/test_reverter_regen_sem_banco.py:1-5`; I/O mockado no segundo: `ponto/tests/test_reverter_exportada_sem_banco.py:157-159`.
- `core/tests/test_medida_turno_do_par.py`: **35 OK**, 41,107 s, usando exatamente a adaptação documentada em `core/tests/test_medida_turno_do_par.py:20-68` (`SimpleTestCase` → `unittest.TestCase` em memória).
- `escala/tests/test_chokepoint_escala_gate.py` + `colaboradores/tests/test_chokepoint_cadastro_gate.py`: **12 OK**, 9,980 s, mesma substituição de base em memória; corpos originais preservados (`escala/tests/test_chokepoint_escala_gate.py:53-81`, `colaboradores/tests/test_chokepoint_cadastro_gate.py:50-90`).
- Cinco métodos puramente estáticos de `ponto/tests/test_chokepoint_celula_gate.py`: **5 OK**, 3,387 s (`:50-61`, `:87-97`). Os dois métodos que importam a porta real não foram executados (`:63-85`) [nao verificado].

Padrão para testes novos sem banco: compilar função real por AST com namespace controlado e trocar somente imports necessários; exemplo existente em `colaboradores/tests/test_ui_painel_mostra_o_app.py:74-98`. Helper de substituição reversível de módulos em `:25-42`; mutação de fonte somente em memória em `colaboradores/tests/ui_painel_fontes.py:38-53`.

Nenhum arquivo foi editado.

## 12/11 09:10 · sessao d20c4f297a46 · - · - · origem=Ronald (tmux)

| Caminho | Classificação | Evidência e encaixe |
|---|---|---|
| Feriado salvo/editado/excluído | **E** | Trabalho calculado consulta feriado do lugar do vínculo; alteração pode mudar `trabalha` futuro. (`escala/models.py:1369`, `escala/models.py:1394`) Hoje o receiver só reavalia chamados da data nova; exclusão não tem receiver. (`chamados/signals.py:322`, `chamados/signals.py:326`) Capturar data anterior no `pre_save`; regenerar futuras existentes nas datas anterior/nova no `post_save` e na data excluída no `post_delete`, pela porta comum e sem estender horizonte. |
| `Posto.opera_em_feriado` | **N**, para trabalho/geradora/DNA das células existentes | A flag do posto define somente o padrão ao **criar vínculo**; vínculo existente guarda sua própria `trabalha_em_feriado`. (`escala/models.py:904`, `escala/models.py:910`) O cálculo da célula lê essa flag do vínculo e o município, sem ler `opera_em_feriado`. (`escala/models.py:1389`, `escala/models.py:1394`) Há outro leitor da flag para cobrança do posto, fora desses campos. (`colaboradores/services/feriado_do_posto.py:60`) |
| `reclassificar_base` | **N**, para trabalho/geradora/DNA | Salva exclusivamente `tipo_base`. (`escala/services/cadastro_tipo.py:133`) Trabalho é calculado por ciclo/foto/âncora/feriado, e os marcos usam horários, overrides e modo de intervalo. (`escala/models.py:816`, `escala/models.py:1271`, `escala/models.py:326`) O produtor de DNA não inclui nem consulta `tipo_base`. (`ponto/management/commands/gerar_celulas.py:83`) Isso não prova ausência de efeitos no motor financeiro, que permanece fora desta classificação. (`ponto/motor_calculo_v2.py:3647`) |
| `TipoEscalaAdmin` | **E** | É `ModelAdmin` simples, sem encaminhamento de gravação/propagação. (`escala/admin.py:8`) Encaminhar `save_model` à porta de cadastro de template, preservando campos do admin; chamar apenas `super().save_model` mantém o buraco. A propagação existente fica em `_propagar_regeneracao_template`. (`escala/services/cadastro_tipo.py:327`) |
| `trocar_calendario_de_folga` | **E**, para tornar a porta autossuficiente | Atualmente salva somente FK e trilha; quem materializa é a view individual ou o lote. (`colaboradores/services/vinculo.py:434`, `colaboradores/views.py:239`, `colaboradores/views.py:245`, `escala/views.py:408`) O cálculo do dia usa a foto `FolgaDia`, não a FK. (`escala/models.py:1280`, `escala/models.py:1310`) |
| `salvar_calendario_de_folga` | **E**, para aplicar a edição às futuras existentes | Grava definição/tamanhos e retorna sem materializar foto; `semana_dia` tem retorno precoce. (`escala/folgas.py:548`, `escala/folgas.py:566`, `escala/folgas.py:574`) Reutilizar `datas_que_o_calendario_manda`. (`escala/folgas.py:43`) |

Para os dois calendários, o suplemento deve reconciliar apenas fotos futuras da janela com células existentes, preservando `FolgaDia` manual, e regenerar pela porta comum com `estender_horizonte=False`. Chamar `gerar_foto_folgas` de mês inteiro ampliaria a escrita sobre o passado: ela apaga/recria as fotos do mês e relavra o mês inteiro. (`escala/folgas.py:175`, `escala/folgas.py:180`, `escala/folgas.py:201`)

REDs sugeridos sem banco: feriado criado/removido/movido com células existentes nas duas datas; admin mudando horário com vínculo que começa amanhã; troca e edição de calendário movendo a folga de amanhã, preservando manual e célula editada. A execução desses testes permanece **[nao verificado]**. Nenhum arquivo foi editado. **[nao verificado por diff]** [nao verificado]

## 12/11 09:04 · sessao 100e569d9f3b · FIO-E0-V4-E3-LEITORES · censo · origem=Code

**Brief FIO-E0-V4-E3-LEITORES — somente censo.** Base informada: `087fe758a0488739ab22885a2f090b312b658a92`; identidade Git e pouso da E4B nesta cópia: [nao verificado]. As referências abaixo são da árvore lida; os anexos transcritos no pedido não foram conferidos contra arquivos originais [nao verificado].

**0. Portão de arquivo**

Há dois resultados distintos; o construtor precisa consumir o ampliado, porque o censo encontrou leitores além dos nomes iniciais. O calendário decide a oferta de “Resolver dia”; o copiloto publica vitalidade e retenção do dia. (`app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:476`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:101`)

| Recorte comparado com LASTRO fornecido | Arquivos em comum |
|---|---|
| Recorte nominal: seis caminhos MUDA, cinco respondedores, complemento 11–12 e testes identificados abaixo | **0 em comum**; estado da obra LASTRO [nao verificado]. | [nao verificado]
| Censo ampliado dos leitores de dia e testes das mesmas funções | `app/colaboradores/services/calendario.py`; `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py`; `app/ponto/services/dia_do_colab.py`; `app/core/tests/test_medida_turno_do_par.py`. Presença na lista LASTRO: anexo fornecido [nao verificado]. |
| Registro necessário ao PRONTO 1 | `app/docs/RELATO.md`, também listado no LASTRO fornecido [nao verificado]. |
| Ambos os recortes × caminhos de `bin/` | **0 em comum**. As referências encontradas em `bin/` descrevem o consumo do censo; não são os leitores de negócio listados aqui. (`bin/fio_estado.py:44`, `bin/tests/test_fio_estado.sh:38`) |

Lastro dos quatro arquivos do recorte ampliado:

- `app/colaboradores/services/calendario.py::contexto_calendario`: lê C6 e constrói `pode_ultimo_recurso`; isso participa da porta administrativa do dia. (`app/colaboradores/services/calendario.py:468`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738`)
- `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py`: importa e exercita `contexto_calendario`; é teste do mesmo serviço, embora sua pergunta específica seja ausência/status. (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:14`, `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:43`)
- `app/ponto/services/dia_do_colab.py::dia_do_colab` e `::_pna_do_dia`: expõem C6 e `pergunta_no_app.do_dia` ao copiloto. (`app/ponto/services/dia_do_colab.py:62`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:107`)
- `app/core/tests/test_medida_turno_do_par.py`: contém o sítio `tarefa_do_dia.py::instancias_do_turno` no universo da medição. Isso é acoplamento de teste, não demonstração de que esse arquivo precise ser editado. (`app/core/tests/test_medida_turno_do_par.py:380`)

**Decisão de portão:** não usar o “0” nominal como autorização para um diff que inclua esses arquivos adicionais. A colisão de escrita efetiva depende do diff final, ainda [nao verificado]. O aval fornecido determina espera/serialização quando houver arquivo em comum [nao verificado].

**1. Censo por AST e leitura dos sítios**

O script ao fim percorre Python sem importar a aplicação, resolve chamadas, aliases e reexports estáticos, imprime os chamadores e aponta operações candidatas a escrita. Reflexão dinâmica e cobertura completa de efeitos transitivos permanecem [nao verificado].

| Autoridade/sítio | O que responde; origem da resposta |
|---|---|
| `app/chamados/services/tarefa_do_dia.py:562::tarefa_do_dia` | Juiz do dia: ações por instância, dono, porta e desconhecimento explícito; delega à carga. (`app/chamados/services/tarefa_do_dia.py:32`, `app/chamados/services/tarefa_do_dia.py:564`, `app/chamados/services/tarefa_do_dia.py:572`) |
| `app/chamados/services/tarefa_do_dia.py:515::CargaDaTarefa.tarefa` | Calcula pelo corpo do mesmo juiz e acrescenta portas/verbos carregados do painel. (`app/chamados/services/tarefa_do_dia.py:523`, `app/chamados/services/tarefa_do_dia.py:528`, `app/chamados/services/tarefa_do_dia.py:532`) |
| `app/chamados/services/tarefa_do_dia.py:164::_tarefa_pura` | Decide exigibilidade lendo célula, lâmpada, teto, cobertura, decisões e efeitos; não é leitor externo. (`app/chamados/services/tarefa_do_dia.py:168`, `app/chamados/services/tarefa_do_dia.py:185`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:306`) |
| `app/chamados/services/tarefa_do_dia.py:538::_portas_da_tarefa` | Projeta portas das ações já julgadas; fila residual não cria tarefa. (`app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:545`, `app/chamados/services/tarefa_do_dia.py:553`) |
| `app/chamados/juizes.py:1612::porta_alcancavel_do_dia` — C17 | Responde alcançabilidade por fila ou pergunta viva no app; deriva essa resposta sem ler `tarefa_do_dia`. (`app/chamados/juizes.py:1615`, `app/chamados/juizes.py:1646`, `app/chamados/juizes.py:1657`) |
| `app/chamados/juizes.py:862::pergunta_viva` — C6 | Vitalidade de uma pergunta; usa o corpo `_pergunta_viva_calc` e memo por instância. Não responde sozinho a exigibilidade completa do dia. (`app/chamados/juizes.py:863`, `app/chamados/juizes.py:889`, `app/chamados/juizes.py:1389`; `app/core/juizes.py:112`) |
| `app/chamados/juizes.py:1494::fio_vivo` — C15 | Redução de C6 sobre a coleção recebida; o universo pertence ao chamador. (`app/chamados/juizes.py:1499`, `app/chamados/juizes.py:1500`, `app/chamados/juizes.py:1503`) |
| `app/chamados/models.py:3366::PerguntaDisputa.viva` | Property que delega a C6; o SQL `q_vivas` é apenas pré-filtro generoso. (`app/chamados/models.py:3373`, `app/chamados/models.py:3376`) |
| `app/chamados/models.py:3035::PerguntaDisputa.estado` | Rótulo que combina carimbos e C6; não lê tarefa. (`app/chamados/models.py:3042`, `app/chamados/models.py:3045`) |

**Escritores de tarefa:** o contrato é **0 escritores persistentes**; a função não possui tabela de estado de tarefa. As operações `update` encontradas no módulo alteram os dicionários locais de saída, não registros ORM. Isso não certifica ausência de escrita em toda dependência transitiva. (`app/docs/JUIZES.md:84`; `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:205`, `app/chamados/services/tarefa_do_dia.py:249`, `app/chamados/services/tarefa_do_dia.py:333`, `app/chamados/services/tarefa_do_dia.py:553`)

Os consumidores atuais de produção de `tarefa_do_dia` encontrados são:

| Arquivo::função | Leitura |
|---|---|
| `app/chamados/management/commands/backfill_fio_morto_e4b.py::ato_do_dia` | Lê tarefa para planejar atos do passivo; não é escritor de tarefa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:82`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`) |
| `app/chamados/management/commands/diff_e3_sombra.py::Command.comparar` | Lê carga e tarefa para comparar projeções dos três consumidores C17. (`app/chamados/management/commands/diff_e3_sombra.py:115`, `app/chamados/management/commands/diff_e3_sombra.py:125`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |

As chamadas diretas de teste à função estão em `test_e2_tarefa_do_dia`, `test_e3_c8c_orm` e `test_e3_portas_tarefa`; o script também imprime chamadas à carga e candidatos ao método `.tarefa`, incluindo wrappers de teste. (`app/chamados/tests/test_e2_tarefa_do_dia.py:139`, `app/chamados/tests/test_e2_tarefa_do_dia.py:321`, `app/chamados/tests/test_e3_c8c_orm.py:45`, `app/chamados/tests/test_e3_portas_tarefa.py:19`)

Os **três consumidores de produção de C17** são exatamente os três abaixo; a catraca atual exige sua permanência e deverá mudar junto com a migração. (`app/chamados/tests/test_e3_pendentes_app.py:19`, `app/chamados/tests/test_e3_pendentes_app.py:145`)

| Caminho MUDA/adicional | Sítio; saída atual | Derivação atual |
|---|---|---|
| Fio | `app/chamados/services/fio.py::montar_chamados_fio`: cartões, formulários, explicação, rótulos | C6, crédito, projeção de regularização e desenlace; não tarefa. (`app/chamados/services/fio.py:163`, `app/chamados/services/fio.py:238`, `app/chamados/services/fio.py:375`, `app/chamados/services/fio.py:390`) |
| Tela admin — fio/modal | `app/chamados/views.py::fio_colaborador`; `::modal_fio_colaborador._resp_modal_full` | Consomem a montagem acima; modal habilita explicação. (`app/chamados/views.py:677`, `app/chamados/views.py:889`) |
| Tela admin — drawer/ficha | `app/colaboradores/views.py::painel_colaborador`; `app/colaboradores/services/ficha.py::ficha_do_colab` | Drawer lê projeção do app; ficha deriva dono pelo verbo e pendência por texto/respostas sem veredito. (`app/colaboradores/views.py:940`, `app/colaboradores/services/ficha.py:154`, `app/colaboradores/services/ficha.py:159`, `app/colaboradores/services/ficha.py:169`) |
| Tela admin — calendário | `app/colaboradores/services/calendario.py::contexto_calendario`; `::resolver_dia_oferecido` | Deriva oferta de ação com janela, resposta, C6 e status do dia. (`app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:467`, `app/colaboradores/services/calendario.py:738`) |
| API/app | `app/api/views.py::api_chamados_v2`; `::api_disputa_perguntas` | Leem `consulta_do_app`, não tarefa. (`app/api/views.py:992`, `app/api/views.py:2530`) |
| Badge dos dois kernels | `app/api/views.py::api_me`; `app/api/views_core.py::api_me` | Leem `projecao_do_app`. (`app/api/views.py:315`, `app/api/views_core.py:303`) |
| Badge/PWA dashboard | `app/core/views.py::dashboard` | Conta pendentes da consulta do app; template desenha essa contagem. (`app/core/views.py:252`, `app/templates/core/dashboard_colaborador_v2.html:78`) |
| Badge da navbar do colaborador | `app/core/context_processors.py::navbar_context._badge_colab` | Conta chamados na fila, unidade diferente das perguntas pendentes dos kernels. (`app/core/context_processors.py:185`, `app/core/context_processors.py:187`) |
| Fonte compartilhada API/badge | `app/chamados/services/pergunta_no_app.py::perguntas_do_app`; `::consulta_do_app`; `::projecao_do_app` | C6 → carimbo sem resposta + disputa aberta → contagem; não tarefa. (`app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:90`) |
| `o_que_segura` | `app/folha/fora_da_folha.py::o_que_segura` | Lê `_fio_do_dia`/`_porta`, que chegam a C17. (`app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:225`) |
| Fora da folha | `app/folha/fora_da_folha.py::_porta`; `::_fio_do_dia`; `::fora_da_folha` | C17 em `:67`; fio vem da resposta dessa porta. (`app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:178`) |
| Cadastro realidade | `app/escala/services/cadastro_realidade.py::beco_em_lote` | Lê C17 e examina vivas/âncoras/cadastro; não tarefa. (`app/escala/services/cadastro_realidade.py:442`, `app/escala/services/cadastro_realidade.py:487`, `app/escala/services/cadastro_realidade.py:488`) |
| Vigia/contador | `app/chamados/services/vigia_fio.py::censo_alcance` | C17 em `:635`; depois decide futuro, baldes e causas até contar fio morto. (`app/chamados/services/vigia_fio.py:542`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:695`) |
| Vigia/cron | `app/chamados/management/commands/vigiar_fio_morto.py::Command.handle` | Executa os censos; é consumidor e publicador da medição. (`app/chamados/management/commands/vigiar_fio_morto.py:174`, `app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/config/crons.py:1721`) |
| Copiloto | `app/ponto/services/dia_do_colab.py::dia_do_colab`; `::_pna_do_dia` | Publica C6 e retenção própria de `pergunta_no_app.do_dia`. (`app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:101`, `app/ponto/services/dia_do_colab.py:108`) |

Há ainda um caminho indireto de contador administrativo: navbar → partição de verbos/crédito, e uma lavra por comando. Migrar a fonte exige conferir também essa testemunha, sem executar sua escrita nesta obra de censo. (`app/core/context_processors.py:84`, `app/core/context_processors.py:90`, `app/core/management/commands/lavrar_badge_navbar.py:63`, `app/core/management/commands/lavrar_badge_navbar.py:91`)

**Cinco respondedores: origem e destino da certidão proposta**

| Respondedor atual | Quem chama | Para onde deve ir a pergunta sobre o dia |
|---|---|---|
| `fio.py::montar_chamados_fio`, ramo `todas_respondidas` | Atendimento, fio admin, modal e parcial. (`app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:446`) | Projeção de C19 para tarefa; complemento 12 consulta C6/C15 para vitalidade. O ramo atual usa timestamp cru. (`app/chamados/services/fio.py:340`; `app/chamados/services/tarefa_do_dia.py:32`; `app/chamados/juizes.py:1503`) |
| `raiox.py::creditos_perguntas` | `particionar_verbos` e montagem do fio. (`app/chamados/services/verbos.py:34`, `app/chamados/services/fio.py:390`) | Consumidores da tarefa leem a decisão C19; retirar a derivação por `respondida_em` do papel de respondedor do dia. (`app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`; `app/docs/JUIZES.md:78`) |
| `relatorios_disputa.py::classificar` | `coletar`. (`app/chamados/relatorios_disputa.py:47`) | Categorias de espera projetam ações/donos C19; fechamento continua C7. Hoje classifica por C6 + timestamp. (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`; `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:78`) |
| `cobranca.py::_calados` | `linhas_para_a_pauta`. (`app/chamados/services/cobranca.py:257`) | Pendência do colaborador vem de C19; canal e SLA continuam perguntas próprias. Hoje usa `q_vivas`, resposta nula e disputa aberta. (`app/chamados/services/cobranca.py:228`, `app/chamados/services/cobranca.py:243`, `app/chamados/services/cobranca.py:246`) |
| `_card_atendimento.html` | Duas listas do atendimento. (`app/templates/chamados/meu_atendimento.html:127`, `app/templates/chamados/meu_atendimento.html:144`) | Receber projeção pronta do servidor; retirar derivações de documento/formulário/ação do dia. (`app/templates/chamados/partials/_card_atendimento.html:65`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:203`; `app/docs/JUIZES.md:78`) |

A certidão remove o papel de respondedor e o código sem chamadores no mesmo commit; não significa apagar serviços inteiros nem C6. O registro histórico também usa “cinco respondedores” para outra enumeração de autoridades, que não deve ser confundida com os cinco sítios do aval. (`CLAUDE.md:191`; `app/core/juizes.py:153`; `app/chamados/services/tarefa_do_dia.py:491`)

**Testes a carregar no brief** [nao verificado]

| Frente | Testes existentes | [nao verificado]
|---|---|
| Tarefa, portas e DIFF | `app/chamados/tests/test_e2_tarefa_do_dia.py:139`; `app/chamados/tests/test_e3_portas_tarefa.py:32`; `app/chamados/tests/test_e3_diff_sombra.py:53`; `app/chamados/tests/test_e3_c8b_sem_banco.py:117`; `app/chamados/tests/test_e3_c8c_orm.py:45` |
| API/badge/catraca | `app/chamados/tests/test_e3_pendentes_app.py:145`; `app/chamados/tests/test_e3_app_orm.py:28`; `app/chamados/tests/test_e3_v4_orm.py:20`; `app/core/tests/test_dashboard_badge.py:40`; `app/api/tests/test_api_me_badge.py:36`; `app/core/tests/test_badge_navbar_consome_particao.py:1` |
| Fio e explicação | `app/chamados/tests/test_fio_explicacao.py:24`; `app/chamados/tests/test_selo_identidade_fio_explicacao.py:72`; `app/chamados/tests/test_fio_ancora_destino.py:58`; `app/chamados/tests/test_fio_le_o_juiz.py:102` |
| Relatório/cartão | `app/chamados/tests/test_relatorio_pendencias.py:35`; `app/chamados/tests/test_o318_card_nao_oferece_o_que_a_porta_recusa.py:103`; `app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:124` |
| Vigia/folha/cadastro | `app/chamados/tests/test_n1_alimentacao_unica.py:77`; `app/chamados/tests/test_o261e_f5_alcance.py:100`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:84`; `app/chamados/tests/test_o261_vigia_fio.py:799`; `app/folha/tests/test_haiku_fora_da_folha.py:65`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:163` |
| Admin | `app/colaboradores/tests/test_selo_situacao_e_lampada.py:118`; `app/colaboradores/tests/test_ficha_do_colab.py:39`; calendário e medição do par citados no portão. |

**JS/SW e CLASSE A:** há service worker no caminho PWA: registro em `pwa.js`, interceptação de GET e estratégias de rede/cache em `sw.js`. O `badge` do push nesse arquivo é um ícone. Não foi demonstrada necessidade de modificar JS/SW para a migração do servidor [nao verificado]. Se o diff tocar esses arquivos, aplica-se a CLASSE A e o smoke nas duas cascas exigidos pelo aval fornecido [nao verificado]. (`app/static/pwa.js:14`, `app/static/sw.js:33`, `app/static/sw.js:45`, `app/static/sw.js:82`, `app/static/sw.js:94`)

**2. CASO-7445: confirmação e juiz correto**

**Item 11 confirmado no código.** O mapa é por cartão; o cache é por disputa; o preenchimento do cache recebe as perguntas do cartão corrente. O primeiro cartão com lista não vazia pode definir a explicação reutilizada nos demais. O explicador aceita a lista fornecida e usa `fio_vivo(ps)` para dizer “Tudo resolvido”. (`app/chamados/services/fio.py:240`, `app/chamados/services/fio.py:305`, `app/chamados/services/fio.py:312`; `app/chamados/services/explicador.py:121`, `app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:140`)

O diff deve alimentar `por_que_nao_fecha` com **todas as perguntas da disputa**, compartilhando suas instâncias; chave e universo precisam coincidir. C15 já declara que o universo é responsabilidade do chamador. O RED 34390/disputa 7445/duas vivas é dado do anexo; reprodução nesta cópia [nao verificado]. (`app/chamados/juizes.py:1500`; `app/chamados/tests/test_selo_identidade_fio_explicacao.py:93`)

**Item 12 confirmado no código.** `todas_respondidas` é `all(p.respondida_em ...)`, e o modal usa esse resultado no aviso de fechamento sem anuência. Pergunta morta por célula pode, portanto, continuar sendo tratada como ausência de resposta nesse cálculo. (`app/chamados/services/fio.py:340`; `app/templates/chamados/partials/modal_fio.html:334`)

O aviso deve consultar a vitalidade por C6/C15 no universo do cartão; C7 continua decidindo se a disputa pode fechar, incluindo sua guarda de resposta sem veredito. Não basta substituir o cálculo por `not fio_vivo(...)`: pergunta viva respondida e pergunta viva sem resposta exigem projeções diferentes. O RED 35602/pergunta 51407/`celula_absolveu` vem do anexo; reprodução [nao verificado]. (`app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1494`; `app/chamados/models.py:1850`, `app/chamados/models.py:1866`)

**Registro:** `JUIZES.md` registra C7 e C19 e menciona C6; não contém verbete independente com os endereços literais C6/C15. O próprio documento manda consultar o registro vivo, onde constam `pergunta_viva` e `fio_vivo`. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:78`, `app/docs/JUIZES.md:90`; `app/core/juizes.py:278`, `app/core/juizes.py:287`)

**3. DECISÕES técnicas, sem juiz novo**

1. **Quem responde o dia:** C19 permanece a autoridade. C17 só pode virar fachada depois do DIFF; a regra já resolve os quatro conflitos históricos. (`app/docs/RELATO.md:515`, `app/docs/RELATO.md:518`)

2. **Fila sem pergunta, fila A/pergunta B, verbos A/B e isento residual:** conservar as respostas de C19 e medir a mudança nos consumidores. As expectativas já estão escritas; sua incidência na frota atual está [aguarda sonda] [nao verificado]. (`app/chamados/tests/test_e3_portas_tarefa.py:32`, `app/chamados

[truncado em 20000 de 40250 caracteres]

## 12/11 09:04 · sessao 082aa6f15257 · - · - · origem=Ronald (tmux)

Brief de censo, sem construção. Hash declarado: `087fe758a0488739ab22885a2f090b312b658a92`; correspondência com `main` e pouso da E4B: [nao verificado — cópia sem `.git`]. Nenhum arquivo criado, editado ou apagado; nenhuma consulta a banco ou log executada. [nao verificado — operações desta sessão]

**0 — Portão de arquivo.** O resultado depende de distinguir arquivos da troca de dependências inventariadas: os três consumidores diretos de C17 continuam sendo folha, vigia e cadastro. (`app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`)

| Cruzamento | Resultado |
|---|---|
| Arquivos explícitos dos seis MUDA, cinco respondedores, itens 11–12 e testes encontrados × LASTRO fornecido | **0 em comum**. [nao verificado — resultado reproduzível pelo script abaixo; lista LASTRO recebida no prompt] | [nao verificado]
| Mesmo conjunto × `bin/` | **0 em comum**. [nao verificado — resultado reproduzível pelo script abaixo] |
| Censo ampliado de consumidores de C6/C15 e da cadeia do copiloto × LASTRO | **4 arquivos em comum**, relacionados abaixo. [nao verificado — pertinência à obra LASTRO vem do anexo fornecido] |

Os quatro arquivos do censo ampliado são:

| Arquivo em comum | Sítio encontrado | Implicação para o construtor |
|---|---|---|
| `app/api/views_mensageria.py` | `api_mensageria_dia_do_colab` → `dia_do_colab`. (`app/api/views_mensageria.py:1970`) | Casca do copiloto; incluir no controle de consumidores. |
| `app/ponto/services/dia_do_colab.py` | `dia_do_colab` lê C6 e publica `ainda_viva`. (`app/ponto/services/dia_do_colab.py:71`) | A resposta do copiloto ainda passa por C6. |
| `app/colaboradores/services/calendario.py` | `contexto_calendario` lê C6 para o último recurso. (`app/colaboradores/services/calendario.py:476`) | Dependência dos baldes atuais do vigia. (`app/chamados/services/vigia_fio.py:265`) |
| `app/ponto/services/cartorio.py` | `_julgar_colab_corpo` lê `fio_vivo`. (`app/ponto/services/cartorio.py:1389`; `app/ponto/services/cartorio.py:1646`) | Escritor de fatos que consome C15; presença no censo não exige sua edição. |

Portão estrito sem colisão; qualquer ampliação do diff para esses quatro arquivos precisa voltar ao cruzamento com LASTRO. Isso não comprova disponibilidade de vaga ou teto dois. [nao verificado — diff futuro, vaga e execução concorrente não fornecidos]

**1 — Censo por AST.** Rodei o script ao fim por `python3 -B` e stdin: **26 definições-alvo, 491 ocorrências de chamada, sendo 127 de produção e 364 em 111 arquivos de teste; zero erros de parse**. São ocorrências, não 491 leitores de dia. [nao verificado — saída de execução em memória, sem relatório persistido] [nao verificado]

O script resolve imports, reexports, apelidos de módulo, atribuições simples e `getattr` literal. O reexport de `models.py` é necessário para encontrar leitores como relatório e app. (`app/chamados/models.py:4083`; `app/chamados/relatorios_disputa.py:4`; `app/chamados/services/pergunta_no_app.py:48`)

Dispatch dinâmico, atribuições em tupla e chamadas de ordem superior não ficam provados por esse AST. Os **214 candidatos** `.viva`, `.estado` e `.q_vivas` incluem modelos diferentes; não são uma contagem de leitores C6. [nao verificado — limite da análise estática]

| Autoridade/sítio | O que responde | Fonte atual / escrita |
|---|---|---|
| `app/chamados/services/tarefa_do_dia.py::tarefa_do_dia` | Ações por instância de marco, dono e porta; `nao_sei` explícito. | Juiz que fica; exige `agora`, delega à carga. **Zero escritor de tarefa**. (`app/chamados/services/tarefa_do_dia.py:562`; `app/docs/JUIZES.md:78`) |
| Mesmo arquivo, `CargaDaTarefa.tarefa` | Monta a resposta pura e acrescenta portas. | Lê insumos, decisões, efeitos e painel. (`app/chamados/services/tarefa_do_dia.py:515`; `app/chamados/services/tarefa_do_dia.py:523`; `app/chamados/services/tarefa_do_dia.py:529`) |
| Mesmo arquivo, `_tarefa_pura`, `_pela_decisao`, `_portas_da_tarefa` | Exigibilidade, última decisão e apresentação das portas. | Corpos internos do mesmo juiz. (`app/chamados/services/tarefa_do_dia.py:116`; `app/chamados/services/tarefa_do_dia.py:164`; `app/chamados/services/tarefa_do_dia.py:538`) |
| `app/chamados/juizes.py::porta_alcancavel_do_dia` | Acesso por chamado da célula na fila ou pergunta viva no app com âncora na fila. | Deriva C17; **não lê tarefa**. (`app/chamados/juizes.py:1612`; `app/chamados/juizes.py:1647`) |
| `app/chamados/juizes.py::pergunta_viva` / `_pergunta_viva_calc` | Vida de uma pergunta. | C6; memo por instância e regra própria do domínio de vida. (`app/chamados/juizes.py:862`; `app/chamados/juizes.py:1389`) |
| `app/chamados/juizes.py::fio_vivo` | Alguma pergunta viva na coleção recebida. | Redução C15 sobre C6; universo pertence ao chamador. (`app/chamados/juizes.py:1494`; `app/chamados/juizes.py:1500`) |

Os `.update` encontrados no módulo da tarefa são atualizações de dicionários em memória, não escritores ORM. (`app/chamados/services/tarefa_do_dia.py:205`; `app/chamados/services/tarefa_do_dia.py:214`; `app/chamados/services/tarefa_do_dia.py:249`; `app/chamados/services/tarefa_do_dia.py:252`; `app/chamados/services/tarefa_do_dia.py:333`; `app/chamados/services/tarefa_do_dia.py:553`)

Os consumidores externos diretos da tarefa encontrados são:

| Sítio | Superfície | Leitura |
|---|---|---|
| `app/chamados/management/commands/backfill_fio_morto_e4b.py::ato_do_dia` | Planejamento dos atos E4B. | Lê tarefa e também examina reabertura do credor. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`) |
| `app/chamados/management/commands/diff_e3_sombra.py::Command.comparar` | Instrumento da sombra. | Abre carga, lê C17 pelo consumidor existente e compara com tarefa. (`app/chamados/management/commands/diff_e3_sombra.py:115`; `app/chamados/management/commands/diff_e3_sombra.py:124`; `app/chamados/management/commands/diff_e3_sombra.py:125`) |

Os seis leitores MUDA se desdobram nestes sítios:

| Leitor / `arquivo::função` | O que responde hoje | Deriva o dia ou lê tarefa? |
|---|---|---|
| **Fio** — `app/chamados/services/fio.py::montar_chamados_fio` | Portas e avisos dos cartões de colaborador/admin. | Combina `.viva`, clique morto, crédito e carimbo cru; **não lê tarefa**. (`app/chamados/services/fio.py:283`; `app/chamados/services/fio.py:340`; `app/chamados/services/fio.py:390`) |
| `app/chamados/views.py::meu_atendimento` | Tela do colaborador. | Chama montagem com `so_janela=True`. (`app/chamados/views.py:569`) |
| `app/chamados/views.py::fio_colaborador` | Tela admin do fio. | Chama montagem do fio. (`app/chamados/views.py:677`) |
| `app/chamados/views.py::modal_fio_colaborador._resp_modal_full` e `fio.py::contexto_parcial_fio` | Modal e atualização após ação. | Chamam montagem com explicação. (`app/chamados/views.py:889`; `app/chamados/services/fio.py:446`) |
| **Tela admin** — `app/colaboradores/views.py::painel_colaborador` | “No app dele agora”. | Lê `projecao_do_app`, sem tarefa. (`app/colaboradores/views.py:941`; `app/templates/colaboradores/partials/painel_colaborador.html:190`) |
| **API/app** — `app/api/views.py::api_me` e `app/api/views_core.py::api_me` | Pendências e badge do app. | Leem `projecao_do_app`, sem tarefa. (`app/api/views.py:316`; `app/api/views_core.py:304`) |
| `app/api/views.py::api_chamados_v2` e `api_disputa_perguntas` | Lista e questionários pendentes/histórico. | Leem `consulta_do_app`, sem tarefa. (`app/api/views.py:993`; `app/api/views.py:2531`) |
| `app/chamados/services/pergunta_no_app.py::perguntas_do_app` | Universo de perguntas vivas do app. | Pré-filtro SQL e C6. (`app/chamados/services/pergunta_no_app.py:49`; `app/chamados/services/pergunta_no_app.py:58`) |
| Mesmo arquivo, `consulta_do_app` | Pendentes versus histórico. | Deriva pelo carimbo de resposta e disputa aberta. (`app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/pergunta_no_app.py:71`) |
| Mesmo arquivo, `projecao_do_app` | Contagem e prioridade do badge. | Conta pendentes e chamados não lidos; sem tarefa. (`app/chamados/services/pergunta_no_app.py:90`; `app/chamados/services/pergunta_no_app.py:98`) |
| **Badge/PWA** — `app/core/views.py::dashboard` | Badge do colaborador. | Conta `consulta_do_app`; sem tarefa. (`app/core/views.py:253`; `app/templates/core/dashboard_colaborador_v2.html:78`) |
| `app/core/context_processors.py::navbar_context._badge_colab` | Badge da navbar. | Conta fila; outro sítio a cobrir no censo. (`app/core/context_processors.py:185`) |
| **o_que_segura** — `app/folha/fora_da_folha.py::o_que_segura` | Dias retidos, fio, frase do que destrava e chamados acessíveis. | Lê C17 por `_fio_do_dia` e `_porta`; frase local `DESTRAVA`. (`app/folha/fora_da_folha.py:203`; `app/folha/fora_da_folha.py:221`; `app/folha/fora_da_folha.py:225`; `app/folha/fora_da_folha.py:40`) |
| Mesmo arquivo, `_porta`, `_fio_do_dia`, `fora_da_folha` | Porta e agrupamento dos retidos. | Leem C17; sem tarefa. (`app/folha/fora_da_folha.py:67`; `app/folha/fora_da_folha.py:77`; `app/folha/fora_da_folha.py:178`) |
| **vigia_fio** — `app/chamados/services/vigia_fio.py::censo_alcance` | `fio_morto_alcance` por dia retido. | C17, depois baldes e causas locais; sem tarefa. (`app/chamados/services/vigia_fio.py:635`; `app/chamados/services/vigia_fio.py:654`; `app/chamados/services/vigia_fio.py:695`) |
| Mesmo arquivo, `censo`, `fio_do_dia`, `dono_fora_do_fio` | Outro universo do vigia, vida da coleção e “bola com alguém”. | C15 e composição de resolver dia, ausência, folga validada e lista admin. (`app/chamados/services/vigia_fio.py:166`; `app/chamados/services/vigia_fio.py:306`; `app/chamados/services/vigia_fio.py:414`; `app/chamados/services/vigia_fio.py:431`) |
| `app/escala/services/cadastro_realidade.py::beco_em_lote` | Beco C11: célula acusadora, pergunta viva e ausência de porta. | Alimentação C17 e decisão C17; sem tarefa. (`app/escala/services/cadastro_realidade.py:476`; `app/escala/services/cadastro_realidade.py:484`; `app/escala/services/cadastro_realidade.py:488`) |

Os cinco respondedores nomeados no aval continuam com estas derivações:

| Respondedor | Resposta própria e quem o consome |
|---|---|
| `app/chamados/services/fio.py::montar_chamados_fio` | `todas_respondidas` por carimbo; consumido pelo aviso do modal. (`app/chamados/services/fio.py:340`; `app/templates/chamados/partials/modal_fio.html:334`) |
| `app/chamados/services/raiox.py::creditos_perguntas` | Filtra lavra e separa respondidas/não respondidas pelo carimbo; consumido por fio e partição de verbos. (`app/chamados/services/raiox.py:135`; `app/chamados/services/raiox.py:139`; `app/chamados/services/fio.py:390`; `app/chamados/services/verbos.py:34`) |
| `app/chamados/relatorios_disputa.py::classificar` | C6 decide as vivas, mas carimbo decide colaborador/supervisão; consumido por `coletar`. (`app/chamados/relatorios_disputa.py:18`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/relatorios_disputa.py:47`) |
| `app/chamados/services/cobranca.py::_calados` | `q_vivas` + sem resposta + disputa aberta; produz contadores por pessoa para a pauta. (`app/chamados/services/cobranca.py:228`; `app/chamados/services/cobranca.py:234`; `app/chamados/services/cobranca.py:257`) |
| `app/templates/chamados/partials/_card_atendimento.html` | Oferece formulário por `.viva`; “aguardando a supervisão” por `respondida_em`. (`app/templates/chamados/partials/_card_atendimento.html:77`; `app/templates/chamados/partials/_card_atendimento.html:94`; `app/templates/chamados/partials/_card_atendimento.html:101`) |

`raiox.py::pergunta_sem_acao_possivel` também participa: fio combina vida com esse leitor da lavra. Ele responde materialização da pergunta, não diretamente tarefa do dia. (`app/chamados/services/raiox.py:153`; `app/chamados/services/raiox.py:162`; `app/chamados/services/fio.py:284`)

Outros consumidores que precisam constar da certidão/avaliação de alcance:

| Sítio | Caminho atual |
|---|---|
| `app/chamados/services/aviso_pendencia.py::perguntas_pendentes_do_colab` | Consulta do app. (`app/chamados/services/aviso_pendencia.py:9`) |
| `app/chamados/services/cobranca.py::_pendentes` | Consulta do app. (`app/chamados/services/cobranca.py:48`) |
| `app/colaboradores/services/ficha.py::ficha_do_colab` | Perguntas do app. (`app/colaboradores/services/ficha.py:166`) |
| Mesmo arquivo, `ficha_para_o_copiloto` | Dia do colaborador. (`app/colaboradores/services/ficha.py:280`) |
| `app/folha/retrato_folha.py::montar_linhas` | `fora_da_folha`. (`app/folha/retrato_folha.py:1157`) |
| `app/ponto/services/pautas_dos_discordantes.py::dias_sem_fio` | `_fio_do_dia`. (`app/ponto/services/pautas_dos_discordantes.py:77`) |
| `app/chamados/management/commands/vigiar_fio_morto.py::Command.handle` | Ambos os censos do vigia; comando do encadeamento de cron. (`app/chamados/management/commands/vigiar_fio_morto.py:175`; `app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/config/crons.py:1721`) |

Para C6, além dos sítios acima, o inventário de chamadas explícitas encontrou os consumidores abaixo. Todos **leem vida de pergunta**, não `tarefa_do_dia`; estar nesta lista não os transforma automaticamente em arquivos do diff. As referências são os próprios sítios de chamada:

```text
app/chamados/juizes.py:1486::pergunta_sem_lastro_ou_viva
app/chamados/juizes.py:1491::perguntas_vivas_de
app/chamados/juizes.py:1503::fio_vivo
app/chamados/juizes.py:1573::alimentar_portas
app/chamados/juizes.py:1674::perguntas_respondidas_a_validar
app/chamados/juizes.py:2336::_dias_das_perguntas_vivas
app/chamados/management/commands/auditar_invariantes_chamados.py:202::_contraprova_f1t
app/chamados/management/commands/auditar_invariantes_chamados.py:540::_i6_pares
app/chamados/management/commands/auditar_invariantes_chamados.py:858::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:894::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1653::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1710::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1769::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1782::Command.handle
app/chamados/management/commands/disparar_perguntas_competencia.py:109::contador_fio_mudo
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:449::Command._descarimbados
app/chamados/management/commands/reabrir_orfas_enterradas.py:83::Command.handle
app/chamados/models.py:368::ChamadoColaborador._vivas_entre
app/chamados/models.py:1510::ChamadoColaborador._universo_da_ressuscitacao
app/chamados/models.py:1897::DisputaSupervisao.fechar
app/chamados/models.py:3045::PerguntaDisputa.estado
app/chamados/models.py:3140::PerguntaDisputa.carimbar
app/chamados/models.py:3376::PerguntaDisputa.viva
app/chamados/reconciliador.py:399::lavrar_sem_turno_aberto
app/chamados/reconciliador.py:662::perguntas_absolvidas_pela_celula
app/chamados/reconciliador.py:759::_viva_em_competencia_aberta
app/chamados/reconciliador.py:1087::veredito_orfa
app/chamados/reconciliador.py:1595::reatar_pergunta_orfa
app/chamados/services/acoes_chamado.py:355::montar_contexto_detalhe
app/chamados/services/competencia_trancada.py:123::medir
app/chamados/services/competencia_trancada.py:133::medir
app/chamados/services/competencia_trancada.py:143::medir
app/chamados/services/disputa_emissao.py:1821::reabrir_credor_de_pergunta_viva
app/chamados/services/disputa_emissao.py:1966::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/disputa_emissao.py:2048::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/fio.py:606::processar_post_fio
app/chamados/services/fio.py:782::_costura_nao_mata_o_fio
app/chamados/services/fio.py:961::_colisao_ao_datar
app/chamados/services/fio.py:2129::_migrar_sob_trava
app/chamados/services/fio.py:2134::_migrar_sob_trava
app/chamados/services/pergunta_no_app.py:170::_marco_ja_perguntado
app/chamados/services/relavra_via.py:242::desfazer_carimbo_sem_lastro
app/chamados/services/tarefa_do_dia.py:491::CargaDaTarefa._ler_vida
app/chamados/services/vigia_fio.py:755::viva_em_disputa_fechada
app/chamados/services/vigia_fio.py:813::carimbos_sem_lastro_por_dono
app/chamados/views.py:1242::responder_pergunta_disputa
app/chamados/views.py:1257::responder_pergunta_disputa
app/chamados/views.py:1417::reabrir_disputa_supervisao
app/api/views.py:2608::api_disputa_responder
```

C15 é lido nestes outros sítios; `_pergunta_viva_calc` é chamado pela casca memoizada em dois caminhos. (`app/chamados/juizes.py:889`; `app/chamados/juizes.py:894`)

```text
app/chamados/juizes.py:2389::disputa_viva_do_chamado
app/chamados/management/commands/encerrar_perguntas_com_fato.py:83::Command.handle
app/chamados/models.py:1854::DisputaSupervisao.pode_fechar
app/chamados/services/disputa_emissao.py:1422::_ja_perguntou
app/chamados/services/explicador.py:137::por_que_nao_fecha
app/chamados/services/limbo_folgas.py:203::filhos_no_defeito
app/folha/previa.py:144::_material
app/ponto/management/commands/emitir_furo_retroativo.py:98::Command.handle
app/ponto/management/commands/reconciliar_grade.py:460::Command._relavrar_orfa
app/ponto/management/commands/supra_juiz.py:215::Command.handle
app/ponto/services/cartorio.py:1389::_julgar_colab_corpo
app/ponto/services/cartorio.py:1646::_julgar_colab_corpo
```

O inventário também inclui os respondedores históricos `cobranca_viva_do_dia` e `motivos_a_perguntar`, porque aparecem no registro antigo da E3. Os consumidores são:

```text
app/chamados/models.py:390::ChamadoColaborador._plano_pergunta_viva
app/chamados/models.py:749::ChamadoColaborador._gate_premissa_do_fechamento
app/ponto/services/cartorio.py:1655::_julgar_colab_corpo
app/ponto/services/entrada_sincronizada.py:29::resolver_cobranca_da_entrada
app/ponto/services/triagem_batida.py:590::_autoresolve_f2_f4
app/chamados/management/commands/disparar_perguntas_competencia.py:115::contador_fio_mudo
app/chamados/services/fabrica_da_celula.py:207::perguntar_pela_celula
app/chamados/services/fabrica_da_celula.py:242::motivos_que_o_evento_deixou
```

Essas duas funções respondem cobrança viva e motivos de emissão; a lista documental antiga não é idêntica aos cinco sítios do aval fornecido. Não apagar C6 como efeito colateral da certidão dos respondedores de dia. (`app/chamados/juizes.py:2157`; `app/chamados/services/fabrica_da_celula.py:92`; `app/core/juizes.py:153`)

Os testes a incluir no diff/catraca, conforme a mudança realizada, têm estes pontos de apoio: [nao verificado]

| Cobertura | Arquivos/sítios existentes |
|---|---|
| Catraca de consumidores C17 | Hoje permite exatamente três sítios; precisa acompanhar a retirada deles. (`app/chamados/tests/test_e3_pendentes_app.py:19`; `app/chamados/tests/test_e3_pendentes_app.py:145`) |
| Quatro divergências C17/tarefa | Fila sem pergunta, A/B, verbos e isento; fixtures da lei existentes, sem demonstração de que sejam os casos reais da sonda. (`app/chamados/tests/test_e3_portas_tarefa.py:32`; `app/chamados/tests/test_e3_portas_tarefa.py:40`; `app/chamados/tests/test_e3_portas_tarefa.py:49`; `app/chamados/tests/test_e3_portas_tarefa.py:57`) |
| API, dashboard e badge | Matriz ORM e dashboard. (`app/chamados/tests/test_e3_app_orm.py:28`; `app/chamados/tests/test_e3_app_orm.py:47`; `app/core/tests/test_dashboard_badge.py:31`; `app/api/tests/test_api_me_badge.py:30`) |
| Aviso, cobrança e badge juntos | Mesma lista pendente atual. (`app/chamados/tests/test_e3_v4_orm.py:20`) |
| Fio admin/colaborador | Vida em disputa fechada e exclusão de morta. (`app/chamados/tests/test_fio_le_o_juiz.py:105`; `app/chamados/tests/test_fio_le_o_juiz.py:115`; `app/chamados/tests/test_fio_le_o_juiz.py:121`) |
| Folha e cadastro | Leitura da porta e contraexemplos do beco. (`app/folha/tests/test_haiku_fora_da_folha.py:93`; `app/escala/tests/test_f3_beco1_folga_na_lis

[truncado em 20000 de 48613 caracteres]

## 12/11 09:03 · sessao b5511a9cf7ff · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · censo · origem=Code

O código confirma portas que pulam vínculos futuros, mas corrige duas premissas do diagnóstico: **`ate=hoje` não limita sozinho a regeneração a hoje**, porque a porta estende o horizonte; **o cron atual reescreve células futuras existentes**, quando encontra vínculo ativo que cobre o dia. (`app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:870`)

O caso col60, vínculo 1564 e células 13–16/11 com geradora 1567 é evidência recebida no anexo, sem remedição [nao verificado]. A correspondência da cópia com o hash informado e o pouso da E4B também não puderam ser certificados por Git [nao verificado]. [nao verificado]

**a) Portas, chamadores e janelas**

Nesta tabela, **H** significa: a regeneração recebe `[desde, ate]` e, por padrão, substitui `ate` por `max(ate, fim da competência corrente da empresa)`. Não existe guarda `d <= hoje` no laço que escreve o DNA. (`app/ponto/portas/celula.py:187`, `app/ponto/portas/celula.py:219`)

| Porta / função | Quem chega nela | Janela e guarda | Célula futura existente |
|---|---|---|---|
| `executar_vinculo` — escala/template/âncora do vínculo | View `vincular_escala`; chat; realocação; comando de fase. (`app/colaboradores/views.py:423`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/escala/regua_defesa.py:321`, `app/ponto/management/commands/censo_fase_12x36.py:423`) | Pede `[data_inicio, hoje]` **somente se `data_inicio <= hoje`**. (`app/colaboradores/services/vinculo.py:316`, `app/colaboradores/services/vinculo.py:321`) | Início amanhã pula essa chamada. A criação também é ignorada pelos dois signals de regeneração; pode haver relavra indireta se copiar calendário padrão ou reparentar folgas. (`app/escala/signals.py:54`, `app/escala/signals.py:104`, `app/colaboradores/services/vinculo.py:290`, `app/escala/signals.py:191`) |
| `marcar_trabalha_em_feriado` | Comando `sincronizar_feriado_admin`, com aplicação. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`) | Desde `max(início do vínculo, início da competência anterior)`; guarda **`desde <= hoje`**; pede até hoje, depois H. (`app/colaboradores/services/vinculo.py:849`, `app/colaboradores/services/vinculo.py:854`, `app/colaboradores/services/vinculo.py:855`, `app/colaboradores/services/vinculo.py:858`) | Vínculo futuro é salvo, mas pula regeneração; vínculo já iniciado alcança o futuro de H. (`app/colaboradores/services/vinculo.py:848`, `app/colaboradores/services/vinculo.py:855`) |
| `reavaliar_pela_troca_de_municipio` | `_evento_do_municipio`, por `atualizar_posto` ou backfill do município. (`app/colaboradores/services/estrutura.py:60`, `app/colaboradores/services/estrutura.py:69`, `app/colaboradores/services/estrutura.py:93`) | Examina vínculos do posto, mas exclui quem **não cobre hoje**; pede `[hoje, hoje]`, depois H. (`app/colaboradores/services/vinculo.py:900`, `app/colaboradores/services/vinculo.py:902`, `app/colaboradores/services/vinculo.py:904`) | Exclui vínculo que começa amanhã. Regenera o futuro alcançado do vínculo corrente, com `recalculo=False`. (`app/colaboradores/services/vinculo.py:902`, `app/colaboradores/services/vinculo.py:905`) |
| `_propagar_regeneracao_template` | `salvar_tipo`, pelo wizard ou aceite do limbo. (`app/escala/services/cadastro_tipo.py:320`, `app/escala/views_wizard.py:108`, `app/chamados/services/limbo_folgas.py:92`) | EC ativas do tipo; desde `max(início EC, início competência anterior)`; guarda **`desde <= hoje`**; até hoje, depois H. (`app/escala/services/cadastro_tipo.py:334`, `app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:338`, `app/escala/services/cadastro_tipo.py:348`) | Pula EC futura. Nas demais, regenera futuro alcançado; dinheiro fica sem recálculo inline e fechamento é invalidado. (`app/escala/services/cadastro_tipo.py:350`, `app/escala/services/cadastro_tipo.py:358`) |
| `_ec_vigencia_regenera_celulas` | `post_save EscalaColaborador`; por exemplo, `encerrar_vinculo` salva início/fim pela instância. (`app/escala/signals.py:34`, `app/colaboradores/services/vinculo.py:800`) | Ignora criação/raw. Desde menor início anterior/novo; até maior fim anterior/novo/hoje; depois H. (`app/escala/signals.py:54`, `app/escala/signals.py:60`, `app/escala/signals.py:67`, `app/escala/signals.py:71`) | Regenera futuras alcançadas; não tem guarda `<= hoje`. (`app/escala/signals.py:71`) |
| `_ec_ancora_regenera_celulas` | `post_save EscalaColaborador`. (`app/escala/signals.py:79`) | Ignora criação/raw e âncora nula/igual. Desde `max(início competência, início EC)`; até `max(fim competência, fim EC, hoje)`. (`app/escala/signals.py:104`, `app/escala/signals.py:108`, `app/escala/signals.py:114`, `app/escala/signals.py:117`) | Regenera futuras alcançadas; não tem guarda `<= hoje`. (`app/escala/signals.py:117`) |
| `gerar_foto_folgas` → `_gerar_foto_folgas_no_ato` | Herança de calendário no vínculo; view individual; aplicação por tipo; cron `lavrar_foto_folgas`. (`app/colaboradores/services/vinculo.py:164`, `app/colaboradores/views.py:245`, `app/escala/views.py:411`, `app/escala/management/commands/lavrar_foto_folgas.py:170`) | Mês civil inteiro, depois H; foto e relavra no mesmo `atomic`. (`app/escala/folgas.py:162`, `app/escala/folgas.py:201`, `app/escala/folgas.py:203`, `app/escala/folgas.py:276`) | Regenera futuras geradas e pode criar faltantes. Apaga/recria **FolgaDia de calendário**, preservando manual; não apaga CelulaDia. (`app/escala/folgas.py:175`, `app/escala/folgas.py:180`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:263`) |
| `lancar_folga` / `alterar_folga` | Plano individual; declaração do dia. (`app/escala/views_plano_folgas.py:131`, `app/escala/views_plano_folgas.py:136`, `app/ponto/services/declarar_dia.py:191`) | Pedem `[dia, dia]` por `relavrar_celulas_da_foto`, **com H habilitado**. (`app/escala/folgas.py:314`, `app/escala/folgas.py:357`, `app/escala/folgas.py:276`) | Regeneram futuras alcançadas; não se limitam necessariamente ao dia informado. (`app/ponto/portas/celula.py:187`, `app/ponto/portas/celula.py:263`) |
| `aplicar_folgas_em_lote` | View de lote do Plano. (`app/escala/views_plano_folgas.py:185`) | Recusa datas anteriores ao início da competência corrente; pede mínimo..máximo gravados, depois H. (`app/escala/folgas.py:379`, `app/escala/folgas.py:385`, `app/escala/folgas.py:395`) | Regenera futuras alcançadas. (`app/escala/folgas.py:395`, `app/ponto/portas/celula.py:263`) |
| `reparentar_folga` | Signal de criação/mudança de vigência; comando de reparentamento. (`app/escala/signals.py:191`, `app/escala/management/commands/reparentar_folgas.py:34`) | Pede `[fd.data, fd.data]`, depois H. Para competência passada, `so_rejulgar` esvazia intervalo e desliga H. (`app/escala/folgas.py:500`, `app/escala/folgas.py:502`, `app/escala/folgas.py:267`, `app/escala/folgas.py:276`) | Regenera futuro alcançado. O alvo vem de `vinculo_do_dia_carregado`, portanto também depende da geradora da célula. (`app/escala/folgas.py:470`, `app/escala/folgas.py:500`) |
| `aplicar_folga_calendario` | View de aplicação por tipo. (`app/escala/views.py:376`) | EC ativas; fotos do mês corrente/próximo e relavra explícita do primeiro dia corrente ao último do próximo. (`app/escala/views.py:392`, `app/escala/views.py:411`, `app/escala/views.py:413`, `app/escala/views.py:416`) | Regenera e pode criar futuras nessa janela, que pode ultrapassar a competência corrente. (`app/escala/views.py:416`, `app/ponto/portas/celula.py:259`) |
| `aplicar_no_vinculo` / `desfazer`, ESCALA-AUTO | Comando `ajustar_escalas_auto`. (`app/escala/management/commands/ajustar_escalas_auto.py:39`, `app/escala/management/commands/ajustar_escalas_auto.py:95`) | Aplicação exige início `> hoje`; desfazer começa em `max(início, amanhã)`; fim é a última célula existente, no mínimo início, depois H. (`app/escala/services/escala_auto_executor.py:57`, `app/escala/services/escala_auto_executor.py:100`, `app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:140`, `app/escala/services/escala_auto_executor.py:143`) | Já alcança futuras existentes. (`app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:143`) |
| `corrigir_escala_retroativa::_aplicar` | Comando de correção retroativa. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:164`) | Piso declarado ou início; topo `min(novo fim ou hoje, hoje)`; guarda piso<=topo; depois H. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:227`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`) | Pode regenerar futuro por H; piso futuro pula a chamada. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:228`) |

A porta comum **não apaga a célula fora da vigência**: se ela é gerada por aquele vínculo, ainda acesa, não lavrada e nenhum outro vínculo cobre o dia, grava `trabalha=False` e DNA sem marcos. Havendo outro vínculo cobrindo, esse ramo preserva a célula; a regeneração pelo sucessor precisa alcançá-la. (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:242`, `app/ponto/portas/celula.py:243`)

Dentro da vigência, a porta cria faltantes ou reescreve geradas diferentes em trabalho/geradora/DNA, guardando `dna_anterior`, `regenerada_em` e `regeneracoes`; vínculo sem vigência declarada é recusado e competência lavrada é protegida. (`app/ponto/portas/celula.py:146`, `app/ponto/portas/celula.py:250`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:271`)

Outras portas necessárias ao perímetro:

| Caminho | O que o código faz |
|---|---|
| Editor de calendário | `salvar_calendario_de_folga` grava calendário/tamanhos e retorna, sem materializar fotos ou chamar regeneração; a view chama esse serviço. (`app/escala/folgas.py:548`, `app/escala/folgas.py:566`, `app/escala/folgas.py:574`, `app/escala/views.py:356`) |
| Troca do calendário do vínculo | `trocar_calendario_de_folga` somente salva a FK; a view individual completa o ato gerando mês corrente/próximo. (`app/colaboradores/services/vinculo.py:446`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`) |
| Cadastro/edição/importação de feriado | Saves vêm das views; o signal agenda somente `reavaliar_dia` da **data nova**; esse serviço encerra chamados, sem regenerar DNA. (`app/core/views_config.py:68`, `app/core/views_config.py:81`, `app/core/views_config.py:141`, `app/chamados/signals.py:330`, `app/chamados/signals.py:336`, `app/chamados/services/feriado_reavaliar.py:77`, `app/chamados/services/feriado_reavaliar.py:84`) |
| Exclusão de feriado | View apaga; `post_delete` foi omitido deliberadamente porque a reavaliação existente só fecha chamados. Não há relavra de célula nesse caminho. (`app/core/views_config.py:198`, `app/chamados/signals.py:322`) |
| Troca de `Colaborador.posto` | Evento de célula removido por lei do posto do vínculo do dia; não confundir com mudar município desse posto. (`app/colaboradores/services/colaborador.py:47`, `app/colaboradores/services/colaborador.py:61`) |
| `Posto.opera_em_feriado` | A porta salva o campo, sem regeneração; seu comando é `marcar_feriado_do_posto`. (`app/colaboradores/services/estrutura.py:80`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132`) |
| Admin Django de template | `TipoEscalaAdmin` é `ModelAdmin` sem encaminhamento explícito a `salvar_tipo`; não considerar a propagação desse caminho comprovada. EC é somente leitura no admin. (`app/escala/admin.py:8`, `app/escala/admin.py:13`) |
| Reclassificação técnica de template | `reclassificar_base` salva somente `tipo_base`, sem propagação; o chamador encontrado é o ensaio de reclassificação. (`app/escala/services/cadastro_tipo.py:134`, `app/ponto/management/commands/diff_reclassificar_partido.py:204`) |
| Fechamentos por `update` | `fechar_vigencia`, usado também pelo desligamento, não atravessa os signals de `save`; a chamada não inclui regeneração. (`app/colaboradores/services/vinculo.py:616`, `app/colaboradores/services/vinculo.py:817`, `app/colaboradores/services/desligamento.py:124`) |
| Recusa de aceite / snapshot | `reverter_para_snapshot` apaga vínculos ausentes do snapshot e restaura os demais por `update`; não chama regeneração. Chamador: ações do chamado. (`app/colaboradores/services/vinculo.py:945`, `app/colaboradores/services/vinculo.py:955`, `app/chamados/services/acoes_chamado.py:148`) |

**Escrita implícita:** excluir EC pode zerar `CelulaDia.escala_geradora` por `SET_NULL`, inclusive numa célula editada; os caminhos explícitos encontrados são restauração de snapshot e desfazer ajuste automático. Isso não é regeneração e precisa constar no perímetro de preservação humana. (`app/escala/models.py:1692`, `app/colaboradores/services/vinculo.py:945`, `app/colaboradores/services/vinculo.py:1021`)

Correções operacionais restantes:

- `regenerar_celulas_dia` delega `[dia,dia]` com H desligado; usada por `aplicar_col900` e seu ensaio; reversão pela mesma porta de célula. (`app/ponto/portas/celula.py:734`, `app/ponto/management/commands/aplicar_col900.py:232`, `app/ponto/management/commands/diff_col900.py:69`, `app/ponto/management/commands/aplicar_col900.py:184`)
- `regenerar_em_exportada` segmenta a janela por cobertura do cadastro e estende H apenas para EC ativa; tem chamador no tratamento do passivo validado. (`app/ponto/portas/regen_exportada.py:145`, `app/ponto/portas/regen_exportada.py:174`, `app/chamados/services/passivo_validado.py:438`)
- `curar_residuo_bug115` chama o gerador em `[d,d]`. (`app/ponto/management/commands/curar_residuo_bug115.py:135`)
- Veredito/ata e carimbos são outras operações da mesma porta, sem serem propagação de cadastro/DNA; o save genérico fica em `salvar_e_emitir` e os insumos em lote em `carimbar_insumos_em_lote`. (`app/ponto/portas/celula.py:74`, `app/ponto/portas/celula.py:905`, `app/ponto/portas/celula.py:1108`)

**Cron, começando pelo sítio pedido:** `gerar_celulas` roda às 05:50, sem argumentos de janela. Cria faltantes; pula editadas; no passado só enriquece DNA nulo; em **hoje/futuro** recalcula e salva trabalho, geradora e DNA quando diferem. Não grava os carimbos de regeneração nesse ramo. Seu universo é EC `ativa=True`, com cobertura do dia; sem EC ativa cobrindo o dia, esse laço não conserta a célula órfã. (`app/config/crons.py:426`, `app/ponto/portas/celula.py:829`, `app/ponto/portas/celula.py:849`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`)

**b) Proteção da edição humana**

A distinção é **`origem`**, não `veredito_via`. `editar_dia` exige autor/motivo, grava `origem='editada'`, `editada_em`, `editada_por` e trilha; a repetição idêntica não move carimbo. A regeneração só reescreve `gerada`, o cron pula `editada` e a reversão exige `gerada`. (`app/ponto/portas/celula.py:559`, `app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:609`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:777`)

`trocar_plantao` chama `editar_dia` para os dois participantes. A ligação dessas operações a uma tela humana concreta não foi encontrada no censo estático [nao verificado]. (`app/ponto/portas/celula.py:675`)

As guardas de futuro em `rejulgar` e `_o_chao_do_dia_mudou` limitam **julgamento**, não a escrita do DNA; não são as guardas a remover para este bug. (`app/ponto/portas/celula.py:300`, `app/ponto/portas/celula.py:431`)

**c) Janela corrente e leitores**

Não há parâmetro “gerar N dias à frente”. `_janela_default` toma a **união das competências correntes das empresas ativas**: menor início, maior fim. O corte vem de `Empresa.dia_inicio_competencia`, default 21, interpretado por `ponto/janelas.py`. (`app/ponto/management/commands/gerar_celulas.py:281`, `app/ponto/management/commands/gerar_celulas.py:290`, `app/colaboradores/models.py:46`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:60`)

Portanto, dias à frente = `fim - hoje`; o número real depende do cadastro [aguarda sonda]. Com corte 21, em 12/11 a janela terminaria em 20/11, oito dias à frente; o corte real da frota não foi consultado [nao verificado]. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`)

O cron anterior de foto de folgas usa competências **corrente e seguinte** e pode materializar meses mais amplos; não confundir essa janela com o default de `gerar_celulas`. (`app/escala/management/commands/lavrar_foto_folgas.py:74`, `app/escala/management/commands/lavrar_foto_folgas.py:76`)

A autoridade já está registrada. Se a célula nomeia geradora fora da vigência, `vinculo_do_dia` devolve `None`, sem escolher o vínculo novo. Isso explica o efeito observado no anexo sem precisar de outro juiz. (`app/core/juizes.py:1304`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:282`)

Leitores com janela que comprovadamente admite futuro:

- Calendário: aceita mês/semana/quinzena/competência/custom sem teto em hoje; carrega geradoras e consulta o vínculo do dia. (`app/colaboradores/services/calendario.py:159`, `app/colaboradores/services/calendario.py:314`, `app/colaboradores/services/calendario.py:389`)
- Grade: percorre toda a janela recebida e consulta o previsto pela autoridade. (`app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:377`, `app/escala/services/leitor_celula.py:411`)
- Semana do posto: vai até domingo e lê `CelulaDia.trabalha`. (`app/escala/services/semana_do_posto.py:51`, `app/escala/services/semana_do_posto.py:85`)
- Métodos do vínculo leem a célula existente antes do cálculo/template: trabalho, marcos, pausas e extra declarada. (`app/escala/models.py:1092`, `app/escala/models.py:1122`, `app/escala/models.py:1187`, `app/escala/models.py:1232`)

Censo de chamadas à família `vinculo_do_dia` — incluindo carregadores, aliases e as cascas `escala_vigente` / `_esc_vigente_do_dia`. Receberem futuro efetivamente em produção, além dos caminhos acima, permanece [nao verificado].

```text
app/api/views.py:589
app/api/views_core.py:575
app/api/views_mensageria.py:976
app/chamados/detector_anomalias.py:146
app/chamados/juizes.py:317,515,1929,2122
app/chamados/services/carga_pergunta_viva.py:162
app/chamados/services/disputa_emissao.py:819,909
app/chamados/services/fila_por_causa.py:173
app/chamados/services/limbo_folgas.py:321
app/chamados/services/validacao.py:901
app/colaboradores/services/calendario.py:389
app/colaboradores/services/feriado_do_posto.py:103
app/colaboradores/services/ficha.py:129
app/core/feriados.py:151
app/core/regua_cct.py:329
app/escala/alimentacao.py:317,330,358
app/escala/folgas.py:470
app/escala/proposta_pdf.py:36
app/escala/services/cadastro_realidade.py:528,779
app/escala/services/escala_certa.py:31
app/escala/services/previsto_cego.py:96
app/escala/services/propositor.py:448
app/escala/servico_jornada.py:63,66,233,362,544
app/escala/utils.py:988,1311,2194,2212,2260
app/folha/export.py:359
app/ponto/calculador/alimentacao.py:196,432
app/ponto/portas/celula.py:584
app/ponto/precedencia.py:349,510,537
app/ponto/propositor.py:334
app/ponto/registro_batida.py:29,44
app/ponto/services/cartorio.py:875,1768,1903,2035
app/ponto/services/cobertura.py:278
app/ponto/services/cxr_na_lista.py:101
app/ponto/services/declarar_dia.py:58
app/ponto/services/esmeril_espelho.py:93,154
app/ponto/services/espelho.py:286,724,1065
app/ponto/services/fechamento.py:125,191,948
app/ponto/services/flip_auto.py:66
app/ponto/services/furos_vetados.py:24
app/ponto/services/teto_na_lista.py:77
app/ponto/services/triagem_batida.py:149,402
app/ponto/services/veredito_celula.py:41
app/ponto/services/vigia_de_hora.py:47,71
app/ponto/supra_juiz.py:161,172
app/ponto/turnos.py:788,1960,2099,2166,2218,2361,3058
app/relatorios/dia_da_frota.py:42
app/

[truncado em 20000 de 35161 caracteres]

## 12/11 09:03 · sessao 40e96c6fe9a6 · MAPA-20 · verificador · origem=Code

**REPROVADO.** Há bloqueantes nos itens 1, 4 e 6. O destino registrado para esta reprovação é congelar o MAPA-20 e seguir para AUDITORIA-DE-TRAVAS, sem volta 6. (`app/docs/BACKLOG.md:1916`)

1. **Desenho — bloqueia: N ainda depende de regex sobre prosa.** A lista guarda fontes e âncoras; o montador procura números depois de `:` ou `;`, aceitando número sem ponto, e transforma a quantidade encontrada em N. (`bin/mapa20_lista.json:7`, `bin/mapa20_lista.json:11`, `bin/mapa20_montar.py:37`, `bin/mapa20_montar.py:96`)

   Contraexemplo confirmado em memória: `PRONTO E3: 1. medir fila; 2 minutos depois repetir sonda` produz **N=2**; “2 minutos” virou segundo item. A citação existe, mas não garante que a contagem corresponda aos itens de PRONTO. (`bin/mapa20_montar.py:97`, `bin/mapa20_montar.py:100`)

   O selo apenas reproduz essa decisão e compara bytes; portanto também aceita a saída com esse N. Obra presente na lista, sem fontes, vira PRONTO AUSENTE corretamente. (`bin/mapa20_valida.py:38`, `bin/mapa20_valida.py:49`, `bin/mapa20_montar.py:245`)

2. **Quem escreve e quem lê — sem segundo escritor identificado no fluxo examinado.**

   - **JSON:** o montador lê diretamente; o teste também lê a lista real. O validador chega à lista indiretamente, chamando `montar`. (`bin/mapa20_montar.py:226`, `bin/tests/test_mapa20_valida.sh:416`, `bin/mapa20_valida.py:38`)
   - **Saída:** o montador escreve o temporário; o raspador lê esse arquivo e escreve outro temporário; o selo lê o raspado; o passo 3 copia os bytes aprovados para `MAPA-20.md`. (`bin/mapa20_montar.py:313`, `bin/relato.sh:105`, `bin/relato.sh:108`, `bin/relato.sh:161`)
   - **Retido:** fica somente se igual ao HEAD público e se a raspagem atual não mudar seus bytes; caso contrário, é retirado. (`bin/relato.sh:117`, `bin/relato.sh:120`, `bin/relato.sh:126`)

   Os caminhos executáveis examinados usam a lista e o temporário montado, sem leitura de `logs/MAPA-20/mapa.md` ou `montador_dados.json`; existência de outro leitor fora da cópia: **[nao verificado]**. (`bin/mapa20_montar.py:28`, `bin/mapa20_montar.py:226`, `bin/relato.sh:101`)

3. **Prova por item — existência é conferida; pertinência não.**

   Trecho que aparece em duas linhas, inclusive entre RELATO e RELATO-ARQUIVO, não conta. Duas ocorrências na mesma linha também não contam. (`bin/mapa20_montar.py:215`, `bin/mapa20_montar.py:216`)

   **Fila — vínculo semântico depende da revisão da lista.** `_prova` não recebe obra nem item: um trecho único `E4C OUTRA OBRA SEM PROVA` é aceito como ponteiro e pode ser associado a qualquer item. Contraexemplo confirmado em memória; o teste também associa o mesmo log aos seis itens e exige `6 de 6`. Isso demonstra o limite de existência declarado, sem demonstrar falsidade nas provas atuais. (`bin/mapa20_montar.py:201`, `bin/mapa20_montar.py:268`, `bin/tests/test_mapa20_valida.sh:136`)

   Commit inexistente não incrementa X; `log:` lexicalmente fora de `logs/` é recusado; prova de item acima do total da fonte é recusada. Pertinência efetiva das provas de produção: **[nao verificado]**. (`bin/mapa20_montar.py:205`, `bin/mapa20_montar.py:211`, `bin/mapa20_montar.py:266`)

4. **CONGELA-O-20 — bloqueia: linha indentada escapa.** O extrator ignora linhas que não começam exatamente com `|`. Contraexemplo confirmado em memória, dentro dos marcadores: ` | NOVA | achado | aberta |` passa sem marca. (`bin/mapa20_montar.py:160`, `bin/mapa20_montar.py:161`)

   A configuração aponta para **7027fb34e**, consulta o BACKLOG dessa base e extrai o mesmo bloco OBRAS nos dois lados. A correspondência histórica do commit ao momento correto do aval é **[nao verificado]**. (`bin/mapa20_lista.json:3`, `bin/mapa20_montar.py:178`, `bin/mapa20_montar.py:181`)

   Renomear para ID ausente da base, sem marca, é recusado; remover uma linha passa, pois a regra compara apenas IDs presentes agora. A remoção não constitui ID novo nessa implementação. (`bin/mapa20_montar.py:181`, `bin/mapa20_montar.py:183`)

   **Fila — marca é substring na linha inteira.** `| NOVA | sem POS-20 | aberta |` passa, assim como marca na prosa de outra coluna. O teste legitima marca na descrição; não há exigência implementada de coluna específica nem interpretação de negação. (`bin/mapa20_montar.py:183`, `bin/tests/test_mapa20_valida.sh:210`)

5. **Fim do programa — sem achado aritmético.** A lista fixa E4B 18–19; o montador valida intervalo e declaração, retira esses IDs do conjunto regular e conta suas provas separadamente. Para total 26, ficam denominadores 24 e 2. (`bin/mapa20_lista.json:12`, `bin/mapa20_montar.py:134`, `bin/mapa20_montar.py:271`, `bin/mapa20_montar.py:273`)

   A prova do item 25 pode aparecer acima do denominador publicado 24: os IDs originais continuam limitados pelo total 26, preservando a numeração após excluir 18–19. (`bin/mapa20_lista.json:11`, `bin/mapa20_lista.json:13`, `bin/mapa20_montar.py:266`)

6. **rc — bloqueia: erro operacional de consulta de commit vira prova ausente.** Qualquer retorno não zero de `git cat-file` resulta em `None`. Contraexemplo confirmado em memória: retorno 128 por erro operacional reduz X e permite continuar a montagem, em vez de produzir rc 2. (`bin/mapa20_montar.py:211`, `bin/mapa20_montar.py:268`, `bin/mapa20_montar.py:274`)

   **Fila:** JSON com `obras: [null]` provoca `AttributeError` e rc 2, embora o defeito seja de dado; erro de `git show`/`cmp` na retenção também pode terminar rc 1 sem elevar para 2. (`bin/mapa20_montar.py:240`, `bin/mapa20_montar.py:309`, `bin/relato.sh:117`, `bin/relato.sh:183`)

   Não encontrei `except` que declare montagem parcial válida: montagem precede escrita, e exceções explícitas interrompem com 1/2. Falha durante `write` pode deixar arquivo parcial, mas retorna 2 e o publicador o retém. (`bin/mapa20_montar.py:302`, `bin/mapa20_montar.py:313`, `bin/mapa20_montar.py:315`, `bin/relato.sh:102`)

7. **Selo — há mordida real, com integração parcial.** As partes (a)/(b) copiam e executam o montador real. Resultados literais detectariam inversão global de X; provas ausentes e remoção do log detectariam contagem indevida; `RAIZ-F1` detectaria retirada completa da restrição de obra. (`bin/tests/test_mapa20_valida.sh:79`, `bin/tests/test_mapa20_valida.sh:119`, `bin/tests/test_mapa20_valida.sh:202`, `bin/tests/test_mapa20_valida.sh:218`)

   **Fila:** o publicador usa montador e raspador duplos, além de alterar o selo. Não prova integração integral com os componentes reais. (`bin/tests/test_mapa20_valida.sh:324`, `bin/tests/test_mapa20_valida.sh:352`, `bin/tests/test_mapa20_valida.sh:353`)

   Falta caso de obra **citada, mas excluída**: mover O169 de `fora` para `obras`, sem fontes, passa no runtime como PRONTO AUSENTE, porque a autorização procura apenas token na prosa. A parte (d) barraria essa alteração no JSON real, mas não exercita essa aceitação. (`bin/mapa20_montar.py:241`, `bin/mapa20_montar.py:245`, `bin/tests/test_mapa20_valida.sh:420`, `bin/tests/test_mapa20_valida.sh:422`)

   Execução atual da suíte e resultado 33/33 mutantes: **[nao verificado]**. [nao verificado]

8. **Publicação — sem vazamento demonstrado nas entradas fornecidas.** O mapa não copia o trecho de RELATO: publica somente `arquivo:linha`; os trechos ficam no JSON usado para localizar a referência. (`bin/mapa20_lista.json:8`, `bin/mapa20_montar.py:216`, `bin/mapa20_montar.py:276`)

   O mapa passa pela raspagem antes do selo; alteração desses bytes provoca retenção, e o passo 3 copia somente o aprovado. A eficácia com os nomes reais do banco e o conteúdo efetivamente publicado permanecem **[nao verificado]**. (`bin/relato.sh:105`, `bin/mapa20_valida.py:49`, `bin/mapa20_valida.py:55`, `bin/relato.sh:161`)

## 12/11 09:02 · sessao 69c26974a319 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**O bloqueante da c11 está curado na cópia:** a recusa permanece em `buracos`, com rótulo próprio, e não entra em `explicados`. Não encontrei novo bloqueante no delta examinado (`app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:159`, `app/escala/services/semana_do_posto.py:163`). **Pouso da obra continua sem certificação:** par `simular_folha` da competência 11 e efeito `abona = 0` [nao verificado].

**1. Cura e contraexemplo**

`_fato_do_dia` relança `TipoSemFaixa`; a grade captura especificamente essa exceção e copia `erro.rotulo` para o buraco e a célula. Somente o retorno normal diferente de `'trabalho'` preenche `explicado_por` e `explicados` (`app/escala/services/semana_do_posto.py:187`, `app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:161`).

Para **um único dia previsto de col17, sem batida, com `fantasma_xyz`, sem extras**, o resultado é:

| Saída | Resultado e evidência |
|---|---|
| `buracos` | Uma entrada com colaborador, data e `recusa='Tipo sem faixa: fantasma_xyz'` (`app/ponto/tests/test_lastro_c11_sem_banco.py:273`). |
| `explicados` | `[]` (`app/ponto/tests/test_lastro_c11_sem_banco.py:275`). |
| Célula | `recusa` preenchida; `explicado_por` ausente (`app/ponto/tests/test_lastro_c11_sem_banco.py:277`). |
| `contador_por_posto` | Conserva o posto: `buracos=1`, `explicados=0`, `extras=0`, `com_par=0` (`app/escala/services/semana_do_posto.py:246`, `app/escala/services/semana_do_posto.py:251`). |
| API **sem posto** | `mostrando=1`; `postos` contém essa linha. Não existe chave `dias_sem_explicacao` nesse ramo (`app/api/views_mensageria.py:2085`, `app/ponto/tests/test_lastro_c12_sem_banco.py:104`). |
| API **com posto** | `pessoas=1`, `dias_sem_explicacao=1`, `bateram_sem_previsao=0`, `pares_provaveis=[]`; `explicados` ausente (`app/api/views_mensageria.py:2120`, `app/ponto/tests/test_lastro_c12_sem_banco.py:98`). |

Precisão: `contador_por_posto` não recebe argumento `posto`; os dois modos pertencem à API (`app/escala/services/semana_do_posto.py:230`, `app/api/views_mensageria.py:2082`, `app/api/views_mensageria.py:2113`).

**Há mordida da cura:** `recusa_explicada` modifica o helper real para voltar a retornar `erro.rotulo`; reexecutei e obtive **sete falhas por asserção**, incluindo contagem zerada, posto omitido e proposta ausente (`app/ponto/tests/test_lastro_c12_sem_banco.py:33`, `app/ponto/tests/test_lastro_c12_sem_banco.py:98`, `app/ponto/tests/test_lastro_c12_sem_banco.py:104`, `app/ponto/tests/test_lastro_c12_sem_banco.py:115`).

**2. Quem escreve e quem lê**

| Campo | Escritor e leitores encontrados |
|---|---|
| `buraco.recusa` | A grade escreve diretamente `erro.rotulo`. Propostas leem data/id/nome do buraco; contador e API contam a lista, sem consultar `recusa`. Leitores específicos do rótulo localizados são os testes (`app/escala/services/semana_do_posto.py:159`, `app/escala/services/semana_do_posto.py:206`, `app/escala/services/semana_do_posto.py:214`, `app/escala/services/semana_do_posto.py:251`, `app/api/views_mensageria.py:2121`, `app/ponto/tests/test_lastro_c12_sem_banco.py:128`). |
| `celula.recusa` | A grade escreve; não localizei leitor produtivo específico. Os contratos leem valor e identidade; a API não serializa as células (`app/escala/services/semana_do_posto.py:157`, `app/ponto/tests/test_lastro_c11_sem_banco.py:277`, `app/ponto/tests/test_lastro_c12_sem_banco.py:129`, `app/api/views_mensageria.py:2120`). |
| `explicado_por` | A grade escreve apenas no ramo normal explicativo. Não localizei leitor produtivo específico; testes exigem ausência na recusa e presença no atestado (`app/escala/services/semana_do_posto.py:162`, `app/ponto/tests/test_lastro_c11_sem_banco.py:278`, `app/ponto/tests/test_lastro_c12_sem_banco.py:145`). |
| `grade.explicados` | A grade continua produzindo a lista; o contador lê seu comprimento. O teste existente com atestado também lê a lista (`app/escala/services/semana_do_posto.py:163`, `app/escala/services/semana_do_posto.py:252`, `app/escala/tests/test_semana_do_posto.py:138`). |
| `contador.explicados` | Continua sendo **inteiro**, publicado nas linhas da API sem posto; não foi removido (`app/escala/services/semana_do_posto.py:252`, `app/api/views_mensageria.py:2090`). |
| `API detalhe.explicados` | A chave foi retirada do detalhe; os testes agora cobram sua ausência (`app/api/views_mensageria.py:2116`, `app/ponto/tests/test_lastro_c11_sem_banco.py:286`). |

Na mensageria, o caminho encontrado é **transporte integral**: `core_client` devolve o JSON; `ferramentas` o embrulha e incorpora ao contexto; `copiloto` serializa o contexto. Não encontrei acesso obrigatório a `explicados`, nem `.get('explicados', [])` que passe a produzir vazio nesse caminho (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:879`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`).

**Não identifiquei quebra de leitor da cópia pela remoção.** O detalhe também deixa de transportar o rótulo individual da recusa: entrega contagens e pares, sem buracos/células. Consumidores externos e interpretação efetiva pelo modelo: [nao verificado] (`app/api/views_mensageria.py:2120`, `app/api/views_mensageria.py:2123`).

**3. Propostas, contador e ESMERIL-SEMÂNTICA**

Ambos tratam o recusado como buraco comum: o contador conserva e conta o dia; propostas cruzam qualquer buraco com extras da mesma data. Isso corresponde ao contrato atual de buraco: **previsto sem batida nem faixa explicativa reconhecida** (`app/escala/services/semana_do_posto.py:129`, `app/escala/services/semana_do_posto.py:206`, `app/escala/services/semana_do_posto.py:246`).

Contraexemplo adicional ensaiado: col17 recusado e col18 com batida sem previsão no mesmo dia produzem uma proposta, confiança `alta`, e contador `(buracos, com_par, explicados)=(1,1,0)` (`app/ponto/tests/test_lastro_c12_sem_banco.py:109`, `app/ponto/tests/test_lastro_c12_sem_banco.py:120`).

Não encontrei leitor que necessite distinguir a recusa **para essa contagem ou hipótese**: a proposta não certifica precedência, e o copiloto recebe instrução explícita de apresentar o par como hipótese sujeita à confirmação da supervisão (`app/escala/services/semana_do_posto.py:196`, `mensageria/nucleo/copiloto.py:280`). A proposta perde o diagnóstico `recusa`, pois copia somente seus campos selecionados; essa limitação é observável, mas não reproduz a classificação falsa de “dia explicado” (`app/escala/services/semana_do_posto.py:212`, `app/escala/services/semana_do_posto.py:219`).

**4. Outros chamadores de `_fato_do_dia`**

No censo da cópia, encontrei **uma única chamada produtiva**, dentro de `grade_do_posto`, imediatamente protegida por `except TipoSemFaixa`. Propostas, contador e API chegam pela grade; não chamam diretamente o helper. Não localizei chamador produtivo adicional que passe a quebrar pelo relançamento (`app/escala/services/semana_do_posto.py:155`, `app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:201`, `app/escala/services/semana_do_posto.py:245`, `app/api/views_mensageria.py:2113`).

**5. Inversão dos testes e alcance dos mutantes** [nao verificado]

A inversão é coerente: C5 exige exceção no helper; C11 exige buraco rotulado, explicados vazio e contagem 1. O atestado continua explicado no teste de vizinhos, evitando selar “tudo vira buraco” (`app/ponto/tests/test_lastro_c5_sem_banco.py:500`, `app/ponto/tests/test_lastro_c11_sem_banco.py:273`, `app/ponto/tests/test_lastro_c11_sem_banco.py:285`, `app/ponto/tests/test_lastro_c12_sem_banco.py:139`).

O carregador compila funções do fonte real. C12 executa helper, grade, propostas, contador e o corpo da view; substitui alimentação ORM, limites da semana, busca, token, decorator e `JsonResponse`. Portanto, não é apenas uma asserção sobre saída pronta de duplo (`app/ponto/tests/test_lastro_c5_sem_banco.py:108`, `app/ponto/tests/test_lastro_c5_sem_banco.py:112`, `app/ponto/tests/test_lastro_c12_sem_banco.py:70`, `app/ponto/tests/test_lastro_c12_sem_banco.py:88`).

| Mutante reexecutado | Resultado | O que sela |
|---|---|---|
| `recusa_explicada` | RC 1; 7 falhas | Separação semântica dos canais (`app/ponto/tests/test_lastro_c12_sem_banco.py:33`). |
| `buraco_sem_rotulo` | RC 1; 3 falhas | Transporte do rótulo no buraco (`app/ponto/tests/test_lastro_c12_sem_banco.py:42`, `app/ponto/tests/test_lastro_c12_sem_banco.py:128`). |
| `celula_sem_rotulo` | RC 1; 2 falhas | Transporte do rótulo na célula (`app/ponto/tests/test_lastro_c12_sem_banco.py:48`, `app/ponto/tests/test_lastro_c12_sem_banco.py:129`). |
| `api_explica_recusa` | RC 1; 3 falhas | **Ausência da chave na API**, não uma quarta prova da classificação: apenas recoloca `g['explicados']` (`app/ponto/tests/test_lastro_c12_sem_banco.py:49`, `app/ponto/tests/test_lastro_c12_sem_banco.py:99`). |

Reexecutei os cinco harnesses: **62 testes, OK, RC 0**; dois contratos C11 são repetidos pelo `load_tests` de C12 (`app/ponto/tests/test_lastro_c12_sem_banco.py:152`).

Também obtive **RED com sete falhas**, reconstruindo somente em memória os blocos produtivos anteriores a partir do delta fornecido. Isso reproduziu as falhas cobradas pelos contratos atuais; identidade da reconstrução com o objeto Git c11: [nao verificado] (`app/ponto/tests/test_lastro_c12_sem_banco.py:98`, `app/ponto/tests/test_lastro_c11_sem_banco.py:260`, `app/ponto/tests/test_lastro_c11_sem_banco.py:273`).

O `--red-c11` histórico não pôde ser reproduzido: seu `git show` falhou no carregamento nesta cópia sem repositório. HTTP/ORM completos e RED contra o hash original: [nao verificado] (`app/ponto/tests/test_lastro_c12_sem_banco.py:26`).

**6. Classes de reprovação**

| Classe | Verificação, gravidade e contraexemplo |
|---|---|
| Segundo juiz | **Sem achado novo:** li a autoridade e a delegação; faixa continua por indexação explícita, e o helper consulta A2 (`app/ponto/precedencia.py:108`, `app/escala/services/semana_do_posto.py:186`). |
| Segundo escritor fora da porta | **Sem achado novo:** as escritas examinadas montam dicionários de resposta, sem persistência nesse fluxo (`app/escala/services/semana_do_posto.py:157`, `app/escala/services/semana_do_posto.py:172`). |
| Relógio fora da borda | **Sem achado novo:** o delta usa os limites recebidos; a leitura de hoje permanece na função de borda existente (`app/escala/services/semana_do_posto.py:54`, `app/escala/services/semana_do_posto.py:135`). |
| Derivação paralela | **Sem achado novo:** previsto vem de `CelulaDia`; realizado usa `batidas_apuraveis`/`parear_turnos`; faixa vem de A2 (`app/escala/services/semana_do_posto.py:84`, `app/escala/services/semana_do_posto.py:106`, `app/escala/services/semana_do_posto.py:186`). |
| Selo sem mordida | **Não procede para a cura:** o mutante que restaura o canal errado derruba contagens e propostas reais do harness; permanece o limite de integração descrito acima (`app/ponto/tests/test_lastro_c12_sem_banco.py:38`, `app/ponto/tests/test_lastro_c12_sem_banco.py:98`, `app/ponto/tests/test_lastro_c12_sem_banco.py:115`). |
| Campo com dois significados | **Bloqueante anterior curado:** recusa e faixa reconhecida têm destinos separados. O contraexemplo do único previsto agora conserva um buraco (`app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:162`, `app/ponto/tests/test_lastro_c11_sem_banco.py:273`). |
| `except` que engole | **Fila preexistente:** `RuntimeError` na A2 ainda vira `'trabalho'`; candidato previsto sem batida aparece como buraco comum, sem diagnóstico da falha. O contrato preserva expressamente isso (`app/escala/services/semana_do_posto.py:189`, `app/ponto/tests/test_lastro_c5_sem_banco.py:502`). |
| Fallback | **Fila preexistente:** falha/vazio do cadastro ainda seleciona `CATALOGO`. Contraexemplo: efeito de `abono` alterado no cadastro, seguido de falha de leitura, volta ao efeito congelado. Ocorrência produtiva: [nao verificado] (`app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:299`). |

## 12/11 09:01 · sessao 6fe9daac1672 · - · - · origem=Ronald (tmux)

Item 4 — CONGELA:

- **Bloqueia:** linha nova indentada escapa. `_ids_do_bloco` ignora tudo que não começa exatamente com `|`; contraexemplo dentro dos marcadores: ` | NOVA | achado | aberta |` é aceito sem marca. Confirmado em memória com `_congela` real e entradas substituídas. `bin/mapa20_montar.py:160-166`.
- **Aceita marca na prosa de qualquer coluna:** `| NOVA | sem POS-20 | aberta |` e `| NOVA | cita CONGELA-O-20 de outra obra | aberta |` passam porque a condição usa substring na linha inteira. Confirmado em memória. O teste também legitima marca na descrição. Se o requisito exigir marca atribuída à obra, **bloqueia**; se bastar ocorrência literal na linha, o comportamento atende essa regra literal. `bin/mapa20_montar.py:182-183`; `bin/tests/test_mapa20_valida.sh:210-211`.
- Renomear `VELHA` para `VELHA-NOME-NOVO` sem marca dá `Falha`/rc 1; remover a linha passa. Isso decorre de comparar apenas IDs presentes agora contra os antigos; não há guarda de remoção. Confirmado em memória. `bin/mapa20_montar.py:181-186`.
- A configuração usa **7027fb34e** e consulta `git show <base>:app/docs/BACKLOG.md`; os dois lados passam pelo mesmo extrator dos marcadores OBRAS. `bin/mapa20_lista.json:3`; `bin/mapa20_montar.py:155-183`. A correspondência histórica desse commit ao momento do aval é **[nao verificado]**.
- O BACKLOG da cópia declara “linha nova no bloco OBRAS sem POS-20 nem citação do aval = rc 1”. `app/docs/BACKLOG.md:2027`.

Item 6 — rc:

- **Bloqueia:** falha operacional de `git cat-file` vira prova ausente, permitindo mapa válido com X menor. `_git` só converte falha de executar o processo em `Operacao`; qualquer retorno não zero do processo vira `None` na prova. Contraexemplo confirmado em memória: retorno 128 com erro de leitura de objeto ⇒ `_prova(..., "commit:7027fb34e", [])` retorna `None`; o chamador apenas deixa de contar o item e continua. `bin/mapa20_montar.py:148-152`; `bin/mapa20_montar.py:211`; `bin/mapa20_montar.py:268-275`.
- **Fila:** schema inválido é chamado de operação. Contraexemplo confirmado em memória: JSON com `obras: [null]` chega a `o.get`, gera `AttributeError` e `main` retorna 2; é falha de dado. `bin/mapa20_montar.py:230-240`; `bin/mapa20_montar.py:309-311`.
- **Fila:** na retenção, erro operacional de `git show` ou `cmp` cai no mesmo caminho de ausência/diferença e não eleva `MAPA_RC` para 2. Com montagem rc 1 e falha operacional ao consultar HEAD, o script pode retirar o mapa e terminar rc 1. `bin/relato.sh:103`; `bin/relato.sh:117-128`; `bin/relato.sh:183`.
- Não encontrei `except` que aceite montagem parcial como válida: montagem inteira precede a abertura da saída; montador e selo distinguem suas exceções declaradas, e o publicador conserva rc 2 nas falhas explicitamente tratadas. `bin/mapa20_montar.py:301-317`; `bin/mapa20_valida.py:37-55`; `bin/relato.sh:101-113`.
- A promessa “nada escrito” não é atômica diante de falha de escrita: abrir com `w` pode truncar e `write` pode falhar deixando arquivo parcial, porém retorna 2 e o publicador o retém. **Fila**, sem caminho identificado que publique esse parcial como válido. `bin/mapa20_montar.py:312-317`; `bin/relato.sh:102-104`.

## 12/11 09:01 · sessao 726789790717 · - · - · origem=Ronald (tmux)

Achado bloqueia — N ainda é decidido por regex sobre texto livre: `ITEM` aceita número sem ponto depois de `:`/`;`, e `_itens_do_aval` transforma essas ocorrências em N (`bin/mapa20_montar.py:37`, `bin/mapa20_montar.py:96`). Contraexemplo executado em memória: fonte `PRONTO E3: 1. medir fila; 2 minutos depois repetir sonda`, chave `E3`, âncora `PRONTO E3:` → N=2; “2 minutos” virou item (`bin/mapa20_montar.py:97`). A lista guarda fontes/âncoras, sem enumerar os itens que constituem N (`bin/mapa20_lista.json:7`, `bin/mapa20_lista.json:11`, `bin/mapa20_lista.json:22`).

Provas — trecho encontrado em duas linhas não conta; também não conta repetido duas vezes na mesma linha, pois são contadas ocorrências (`bin/mapa20_montar.py:215`). Trecho único de linha alheia conta: `_prova` não recebe obra nem número do item (`bin/mapa20_montar.py:201`). Contraexemplo executado em memória: RELATO contendo apenas `E4C: OUTRA-OBRA-SEM-PROVA`, ponteiro `relato:OUTRA-OBRA-SEM-PROVA` → `app/docs/RELATO.md:1`, utilizável em qualquer item (`bin/mapa20_montar.py:216`, `bin/mapa20_montar.py:268`). Isso demonstra o limite de existência, explicitamente assumido pelo montador; pertinência das provas reais fornecidas permanece [nao verificado]. O teste também permite um mesmo log nos seis itens e exige 6 de 6 (`bin/tests/test_mapa20_valida.sh:136`, `bin/tests/test_mapa20_valida.sh:139`).

Commit inexistente resulta em `None`, sem incrementar X; log lexicalmente fora de `logs/` gera `Falha`; número de prova acima do total da fonte gera `Falha` (`bin/mapa20_montar.py:205`, `bin/mapa20_montar.py:211`, `bin/mapa20_montar.py:266`).

E4B — sem achado aritmético: itens 18–19 são fixados na lista, validados contra a declaração da fonte, removidos do conjunto regular e contados separadamente (`bin/mapa20_lista.json:12`, `bin/mapa20_montar.py:134`, `bin/mapa20_montar.py:271`, `bin/mapa20_montar.py:273`). A prova 25 exceder o denominador publicado 24 é consequência de preservar a numeração original dos 26 itens após retirar 18–19; o limite validado para IDs continua sendo o total da fonte (`bin/mapa20_lista.json:11`, `bin/mapa20_lista.json:13`, `bin/mapa20_montar.py:266`, `bin/mapa20_montar.py:274`).

## 12/11 09:01 · sessao 5708cda2aee1 · - · - · origem=Ronald (tmux)

Item 7: os casos centrais usam o montador real: a fixture copia `mapa20_montar.py`/`mapa20_valida.py` do repositório e executa esses arquivos (`bin/tests/test_mapa20_valida.sh:79`, `:100`, `:250`).

- Inverter X globalmente seria detectado pelos resultados literais `1 de 6`, `1 de 4`, `0 de 3`, `0 de 2` (`bin/tests/test_mapa20_valida.sh:119`).
- Contar prova ausente seria detectado pela fixture com log ausente, trecho duplicado, trecho inexistente e commit inexistente, pelos resultados literais e pela remoção posterior do log (`bin/tests/test_mapa20_valida.sh:50`, `:59`, `:119`, `:218`).
- Remover totalmente a restrição de obra seria detectado pelo caso `RAIZ-F1`, esperado rc 1 (`bin/tests/test_mapa20_valida.sh:202`).
- A parte do publicador usa duplo explícito de montador, duplo de raspador e altera o selo para marcar a segunda montagem; não constitui integração integral com os componentes reais (`bin/tests/test_mapa20_valida.sh:323`, `:352`, `:353`).

**Fila — cobertura de obra excluída mas citada no aval:** o negativo usa somente `RAIZ-F1`, ausente do aval sintético; não tenta mover `O169`, citado como “já pousada”, para `obras` (`bin/tests/test_mapa20_valida.sh:26`, `:202`). O runtime aceita nome encontrado como token na prosa do aval; movendo O169 de `fora` para `obras` com `fontes: []`, ele vira PRONTO AUSENTE (`bin/mapa20_montar.py:241`, `:245`). A parte (d) detectaria essa alteração no JSON real, mas não testa essa aceitação pelo runtime (`bin/tests/test_mapa20_valida.sh:419`, `:422`).

A suíte não foi executada porque cria/remove arquivos e repositórios; resultado corrente GREEN e 33/33 mutantes: [nao verificado] (`bin/tests/test_mapa20_valida.sh:18`, `:69`, `:77`, `:81`).

## 12/11 09:00 · sessao 3b33739374a1 · - · - · origem=Ronald (tmux)

RED adicional concluído **contra a c11 reconstruída do delta fornecido, somente em memória**: **7 testes, 7 falhas por asserção, RC 1**, sem erro de carregamento. Os casos reproduzem contador/API zerados, proposta ausente, recusa fora dos buracos e helper sem levantar a exceção. (`app/ponto/tests/test_lastro_c12_sem_banco.py:98`, `app/ponto/tests/test_lastro_c12_sem_banco.py:104`, `app/ponto/tests/test_lastro_c12_sem_banco.py:115`, `app/ponto/tests/test_lastro_c12_sem_banco.py:127`, `app/ponto/tests/test_lastro_c11_sem_banco.py:260`, `app/ponto/tests/test_lastro_c11_sem_banco.py:273`)

Inverti literalmente os blocos executáveis indicados pelo delta: tratamento na grade, retorno do helper e chave `explicados` da API; o carregamento e execução utilizaram o harness existente. (`app/escala/services/semana_do_posto.py:154`, `app/escala/services/semana_do_posto.py:187`, `app/api/views_mensageria.py:2122`, `app/ponto/tests/test_lastro_c5_sem_banco.py:112`)

Identidade dessa reconstrução com o objeto Git `1bca7dc22`: **[nao verificado]**. O replay histórico continua indisponível nesta cópia; seu mecanismo exige `git show`. (`app/ponto/tests/test_lastro_c12_sem_banco.py:26`)

## 12/11 09:00 · sessao 3b33739374a1 · - · - · origem=Ronald (tmux)

A inversão é coerente: o helper passa a levantar `TipoSemFaixa`, e a grade guarda o rótulo em `recusa`, conservando o dia em `buracos`, sem `explicado_por` nem entrada em `explicados`. Os testes C5/C11 exigem esses contratos. (`app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:187`, `app/ponto/tests/test_lastro_c5_sem_banco.py:500`, `app/ponto/tests/test_lastro_c11_sem_banco.py:273`)

Os quatro mutantes atingem código real compilado da árvore, com alterações exclusivamente em memória; o carregador lê o arquivo e compila os nós AST selecionados. (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`, `app/ponto/tests/test_lastro_c5_sem_banco.py:108`, `app/ponto/tests/test_lastro_c5_sem_banco.py:112`)

| Ensaio reexecutado | Resultado | Evidência |
|---|---|---|
| Cinco harnesses C5/C7/C10/C11/C12 | 62 testes, OK, RC 0 | `app/ponto/tests/test_lastro_c12_sem_banco.py:152` inclui novamente dois contratos C11 |
| C12 isolado | 7 testes, OK, RC 0 | `app/ponto/tests/test_lastro_c12_sem_banco.py:95`, `app/ponto/tests/test_lastro_c12_sem_banco.py:152` |
| `recusa_explicada` | RC 1, 7 falhas por asserção | Mutação do helper real: `app/ponto/tests/test_lastro_c12_sem_banco.py:33`; cobrança de contagem: `app/ponto/tests/test_lastro_c12_sem_banco.py:98` |
| `buraco_sem_rotulo` | RC 1, 3 falhas por asserção | `app/ponto/tests/test_lastro_c12_sem_banco.py:42`, `app/ponto/tests/test_lastro_c12_sem_banco.py:128` |
| `celula_sem_rotulo` | RC 1, 2 falhas por asserção | `app/ponto/tests/test_lastro_c12_sem_banco.py:48`, `app/ponto/tests/test_lastro_c12_sem_banco.py:129` |
| `api_explica_recusa` | RC 1, 3 falhas por asserção | `app/ponto/tests/test_lastro_c12_sem_banco.py:49`, `app/ponto/tests/test_lastro_c12_sem_banco.py:99` |

A mordida semântica principal é `recusa_explicada`: devolver `erro.rotulo` no canal normal faz novamente `dias_sem_explicacao=0`, omite o posto do contador e retira a proposta; as asserções correspondentes falham. (`app/ponto/tests/test_lastro_c12_sem_banco.py:38`, `app/ponto/tests/test_lastro_c12_sem_banco.py:98`, `app/ponto/tests/test_lastro_c12_sem_banco.py:104`, `app/ponto/tests/test_lastro_c12_sem_banco.py:115`)

O quarto mutante sela a **remoção da chave da resposta**, não a classificação: ele apenas recoloca `'explicados': g['explicados']`, inclusive quando a lista está vazia. Não deve ser apresentado como uma quarta prova independente da cura semântica. (`app/ponto/tests/test_lastro_c12_sem_banco.py:49`)

C12 executa grade/helper/propostas/contador e o corpo inteiro da view; substitui alimentação ORM, limites da semana, busca, token, decorator e `JsonResponse`. A2 executa a implementação real alimentada por linhas simuladas; somente o teste de identidade substitui explicitamente `fatos_do_dia`. HTTP/ORM completos permanecem [nao verificado]. (`app/ponto/tests/test_lastro_c12_sem_banco.py:70`, `app/ponto/tests/test_lastro_c12_sem_banco.py:76`, `app/ponto/tests/test_lastro_c12_sem_banco.py:88`, `app/ponto/tests/test_lastro_c5_sem_banco.py:193`, `app/ponto/tests/test_lastro_c5_sem_banco.py:232`, `app/ponto/tests/test_lastro_c12_sem_banco.py:125`)

O contrato C11 da API isoladamente avalia apenas o dicionário final, mas C12 cobre também a execução da função real nos dois ramos. (`app/ponto/tests/test_lastro_c11_sem_banco.py:279`, `app/ponto/tests/test_lastro_c12_sem_banco.py:93`, `app/ponto/tests/test_lastro_c12_sem_banco.py:95`, `app/ponto/tests/test_lastro_c12_sem_banco.py:101`)

O replay `--red-c11` falhou com **7 erros de carregamento**, não asserções: `git show` retornou 128 porque esta cópia não contém repositório Git. O RED histórico relatado pelo construtor não foi reproduzido aqui; permanece [nao verificado]. O carregador exige esse objeto Git. (`app/ponto/tests/test_lastro_c12_sem_banco.py:26`)

## 12/11 08:59 · sessao b5ce655b1b89 · - · - · origem=Ronald (tmux)

Não encontrei quebra nova nos leitores da cópia nem chamador produtivo adicional de `_fato_do_dia`: a única chamada produtiva está na grade, imediatamente protegida pela captura específica (`app/escala/services/semana_do_posto.py:155`, `app/escala/services/semana_do_posto.py:156`). Consumidores externos: [nao verificado].

| Campo | Escritor | Leitores e efeito |
|---|---|---|
| `buraco.recusa` | Grade copia `erro.rotulo` diretamente (`app/escala/services/semana_do_posto.py:159`). | Nenhum leitor produtivo localizado que examine esse campo. Propostas leem data/id/nome do buraco; contador e API contam a lista (`app/escala/services/semana_do_posto.py:206`, `app/escala/services/semana_do_posto.py:214`, `app/escala/services/semana_do_posto.py:251`, `app/api/views_mensageria.py:2121`). Testes examinam valor/identidade (`app/ponto/tests/test_lastro_c11_sem_banco.py:273`, `app/ponto/tests/test_lastro_c12_sem_banco.py:128`). |
| `celula.recusa` | Grade copia `erro.rotulo` (`app/escala/services/semana_do_posto.py:157`). | Nenhum leitor produtivo específico localizado; API usa somente quantidade de linhas, sem serializar células (`app/api/views_mensageria.py:2120`). Testes leem o campo (`app/ponto/tests/test_lastro_c11_sem_banco.py:277`, `app/ponto/tests/test_lastro_c12_sem_banco.py:129`). |
| `explicado_por` | Apenas ramo normal, `fato != 'trabalho'` (`app/escala/services/semana_do_posto.py:161`, `app/escala/services/semana_do_posto.py:162`). | Nenhum leitor produtivo específico localizado. Contratos exigem ausência na recusa e presença no atestado (`app/ponto/tests/test_lastro_c11_sem_banco.py:278`, `app/ponto/tests/test_lastro_c12_sem_banco.py:144`, `app/ponto/tests/test_lastro_c12_sem_banco.py:145`). |
| `grade.explicados` | Apenas ramo normal reconhecido (`app/escala/services/semana_do_posto.py:163`). | Contador lê comprimento e publica inteiro homônimo; testes existentes verificam lista do atestado (`app/escala/services/semana_do_posto.py:252`, `app/escala/tests/test_semana_do_posto.py:138`). |
| `API detalhe.explicados` removido | Resposta atual termina sem essa chave (`app/api/views_mensageria.py:2116`, `app/api/views_mensageria.py:2130`). | Nenhum leitor tipado produtivo localizado. Mensageria transporta JSON integral; não acessa chave nem fornece default vazio (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:879`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`). |

A distinção permanece na grade; o resumo da API não transporta o rótulo individual, porque entrega somente contagens e pares. Isso limita o que a mensageria poderá narrar, mas não identifiquei leitor que passe a quebrar ou interpretar ausência de `explicados` como lista vazia (`app/api/views_mensageria.py:2120`, `app/api/views_mensageria.py:2121`, `app/api/views_mensageria.py:2123`, `mensageria/nucleo/copiloto.py:379`). Interpretação efetiva pelo modelo: [nao verificado].

Propostas e contador tratam recusa como buraco comum: ambos são “previsto sem batida nem faixa explicativa reconhecida”. O contador mantém o posto e conta o dia; propostas produzem hipótese quando existe extra no mesmo dia, sem resolver precedência (`app/escala/services/semana_do_posto.py:129`, `app/escala/services/semana_do_posto.py:206`, `app/escala/services/semana_do_posto.py:207`, `app/escala/services/semana_do_posto.py:246`, `app/escala/services/semana_do_posto.py:251`). O copiloto manda explicitamente apresentar par como hipótese e exigir confirmação da supervisão; não encontrei segundo significado incompatível nesses consumidores (`mensageria/nucleo/copiloto.py:280`, `mensageria/nucleo/copiloto.py:284`).

Contraexemplo conferido por leitura: único previsto sem batida e `fantasma_xyz` resulta em um buraco com rótulo, `explicados=[]`, célula com `recusa` sem `explicado_por`; contador conserva linha com `buracos=1`, `extras=0`, `explicados=0`, `com_par=0`; API sem posto conserva o posto, e detalhe publica `dias_sem_explicacao=1` (`app/escala/services/semana_do_posto.py:156`, `app/escala/services/semana_do_posto.py:159`, `app/escala/services/semana_do_posto.py:246`, `app/escala/services/semana_do_posto.py:251`, `app/api/views_mensageria.py:2083`, `app/api/views_mensageria.py:2090`, `app/api/views_mensageria.py:2121`).

**Fila preexistente — except/fallback:** `RuntimeError` na autoridade ainda vira `'trabalho'`, e portanto buraco comum sem recusa. Contraexemplo concreto: falha de consulta ao julgar candidato previsto sem batida. Está explicitamente preservado e testado; não é introdução da cura (`app/escala/services/semana_do_posto.py:189`, `app/escala/services/semana_do_posto.py:190`, `app/ponto/tests/test_lastro_c5_sem_banco.py:503`).

## 12/11 08:56 · sessao 925bd426ff97 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

A recusa permanece em `buracos`, com `erro.rotulo` em campo próprio no buraco e na célula ([código](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/services/semana_do_posto.py:156)).

Entrega em [LASTRO_C12.txt](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C12.txt:8): censo, comandos, RED na c11, 62 execuções GREEN, quatro mutantes C12 e dez C11 falhando por asserção, além do ruff aprovado.

Par financeiro da competência 11: **[nao verificado]**.

## 12/11 08:56 · sessao a53ca1c97fa3 · - · - · origem=Ronald (tmux)

Censo de leitores concluído por AST em memória (`python3 -B`, sem importar Django ou escrever arquivo). O inventário completo de chamadas e o cruzamento LASTRO foram enviados nas mensagens anteriores. A execução de cada leitor em produção permanece [não verificado].

- A autoridade já existe: `app/core/juizes.py:1304-1305`. A célula com geradora fora da vigência devolve `None`, sem escolher o vínculo novo: `app/escala/alimentacao.py:269-286`.
- Há leitores que admitem futuro: calendário aceita mês, semana, quinzena, competência e intervalo customizado sem teto em hoje (`app/colaboradores/services/calendario.py:159-212`), carrega células/geradoras (`:314-316`) e chama a autoridade (`:389`); `grade_da_celula` percorre toda a janela recebida (`app/escala/services/leitor_celula.py:350-412`); semana do posto pode incluir dias futuros e lê `CelulaDia.trabalha` (`app/escala/services/semana_do_posto.py:51-56`, `:85-87`).
- O cron não tem horizonte fixo de N dias. Sua janela é a união das competências correntes das empresas ativas: `app/ponto/management/commands/gerar_celulas.py:281-291`. Roda às 05:50 sem argumentos: `app/config/crons.py:426`. O corte vem de `Empresa.dia_inicio_competencia`, default 21 (`app/colaboradores/models.py:46`), interpretado por `app/ponto/janelas.py:10-63`. Horizonte real = `(fim - hoje).days` [aguarda sonda].
- Interseção direta com LASTRO: `app/api/views_mensageria.py:976`; `app/colaboradores/services/calendario.py:315,389`; `app/escala/utils.py:988,1311,2183,2192,2194,2212,2260`; `app/folha/export.py:359`; `app/ponto/precedencia.py:313,320,349,510,537`; `app/ponto/services/cartorio.py:875,1768,1903,2035`; `app/ponto/services/espelho.py:286,625,724,1054,1065`; `app/ponto/services/fechamento.py:125,190,191,656,948`; `app/relatorios/pdf_espelho.py:307,358,571`. A identificação desses caminhos como LASTRO provém do anexo fornecido [não verificado quanto ao estado externo da obra].
- MAPA-20: `app/docs/BACKLOG.md:1916` nomeia `bin/mapa20_montar.py`; `:1900` cita publicação por `bin/relato.sh`. O publicador desta cópia não inclui MAPA-20 nos pares publicados (`bin/relato.sh:103-117`) nem no `git add` (`:121`). Estado externo da obra [não verificado].
- Lei já resolve evento imediato, cron vigia, escritor único, célula irretroativa e exceção formal: `CLAUDE.md:149-156`, `:171-178`. Não identifiquei necessidade de juiz novo; as perguntas do domínio já têm autoridades registradas (`app/core/juizes.py:1304-1305`). O aval fornecido já determina alcance apenas de futuros existentes na janela, preservação de editadas e proibição de ampliar passado [não verificado fora do anexo].

Perguntas de lei sugeridas para consolidação: **0**.

## 12/11 08:56 · sessao 563c7d44c06f · - · - · origem=Ronald (tmux)

Censo executado por AST em memória, com o script integral já enviado; não importou Django nem código da cópia. Resultado: 13 definições, 403 ocorrências de chamada — 94 em produção e 309 em 97 arquivos de teste; zero erros de parse. Essas contagens são da execução do script enviado, sem arquivo persistido [nao verificado]. [nao verificado]

`models.py` reexporta `pergunta_viva`; desconsiderar esse reexport perderia leitores reais. `app/chamados/models.py:4083`.

As 94 chamadas de produção seguem abaixo. Cada linha conserva o alvo e o sítio `arquivo:linha::função`.

```text
pergunta_viva:
app/api/views.py:2608::api_disputa_responder
app/chamados/juizes.py:1486::pergunta_sem_lastro_ou_viva
app/chamados/juizes.py:1491::perguntas_vivas_de
app/chamados/juizes.py:1503::fio_vivo
app/chamados/juizes.py:1573::alimentar_portas
app/chamados/juizes.py:1674::perguntas_respondidas_a_validar
app/chamados/juizes.py:2336::_dias_das_perguntas_vivas
app/chamados/management/commands/auditar_invariantes_chamados.py:202::_contraprova_f1t
app/chamados/management/commands/auditar_invariantes_chamados.py:540::_i6_pares
app/chamados/management/commands/auditar_invariantes_chamados.py:858::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:894::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1653::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1710::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1769::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1782::Command.handle
app/chamados/management/commands/disparar_perguntas_competencia.py:109::contador_fio_mudo
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:449::Command._descarimbados
app/chamados/management/commands/reabrir_orfas_enterradas.py:83::Command.handle
app/chamados/models.py:368::ChamadoColaborador._vivas_entre
app/chamados/models.py:1510::ChamadoColaborador._universo_da_ressuscitacao
app/chamados/models.py:1897::DisputaSupervisao.fechar
app/chamados/models.py:3045::PerguntaDisputa.estado
app/chamados/models.py:3140::PerguntaDisputa.carimbar
app/chamados/models.py:3376::PerguntaDisputa.viva
app/chamados/reconciliador.py:399::lavrar_sem_turno_aberto
app/chamados/reconciliador.py:662::perguntas_absolvidas_pela_celula
app/chamados/reconciliador.py:759::_viva_em_competencia_aberta
app/chamados/reconciliador.py:1087::veredito_orfa
app/chamados/reconciliador.py:1595::reatar_pergunta_orfa
app/chamados/relatorios_disputa.py:18::classificar
app/chamados/services/acoes_chamado.py:355::montar_contexto_detalhe
app/chamados/services/competencia_trancada.py:123::medir
app/chamados/services/competencia_trancada.py:133::medir
app/chamados/services/competencia_trancada.py:143::medir
app/chamados/services/disputa_emissao.py:1821::reabrir_credor_de_pergunta_viva
app/chamados/services/disputa_emissao.py:1966::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/disputa_emissao.py:2048::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/fio.py:238::montar_chamados_fio
app/chamados/services/fio.py:606::processar_post_fio
app/chamados/services/fio.py:782::_costura_nao_mata_o_fio
app/chamados/services/fio.py:961::_colisao_ao_datar
app/chamados/services/fio.py:2129::_migrar_sob_trava
app/chamados/services/fio.py:2134::_migrar_sob_trava
app/chamados/services/pergunta_no_app.py:58::perguntas_do_app
app/chamados/services/pergunta_no_app.py:170::_marco_ja_perguntado
app/chamados/services/relavra_via.py:242::desfazer_carimbo_sem_lastro
app/chamados/services/tarefa_do_dia.py:491::_carga_da_tarefa_cls.CargaDaTarefa._ler_vida
app/chamados/services/vigia_fio.py:755::viva_em_disputa_fechada
app/chamados/services/vigia_fio.py:813::carimbos_sem_lastro_por_dono
app/chamados/views.py:1242::responder_pergunta_disputa
app/chamados/views.py:1257::responder_pergunta_disputa
app/chamados/views.py:1417::reabrir_disputa_supervisao
app/colaboradores/services/calendario.py:476::contexto_calendario
app/ponto/services/dia_do_colab.py:71::dia_do_colab

_pergunta_viva_calc:
app/chamados/juizes.py:889::pergunta_viva
app/chamados/juizes.py:894::pergunta_viva

fio_vivo:
app/chamados/juizes.py:2389::disputa_viva_do_chamado
app/chamados/management/commands/encerrar_perguntas_com_fato.py:83::Command.handle
app/chamados/models.py:1854::DisputaSupervisao.pode_fechar
app/chamados/services/disputa_emissao.py:1422::_ja_perguntou
app/chamados/services/explicador.py:137::por_que_nao_fecha
app/chamados/services/limbo_folgas.py:203::filhos_no_defeito
app/chamados/services/vigia_fio.py:166::fio_do_dia
app/folha/previa.py:144::_material
app/ponto/management/commands/emitir_furo_retroativo.py:98::Command.handle
app/ponto/management/commands/reconciliar_grade.py:460::Command._relavrar_orfa
app/ponto/management/commands/supra_juiz.py:215::Command.handle
app/ponto/services/cartorio.py:1389::_julgar_colab_corpo
app/ponto/services/cartorio.py:1646::_julgar_colab_corpo

porta_alcancavel_do_dia:
app/chamados/services/vigia_fio.py:635::censo_alcance
app/escala/services/cadastro_realidade.py:488::beco_em_lote
app/folha/fora_da_folha.py:67::_porta

tarefa_do_dia:
app/chamados/management/commands/backfill_fio_morto_e4b.py:87::ato_do_dia
app/chamados/management/commands/diff_e3_sombra.py:125::Command.comparar

carga_da_tarefa:
app/chamados/management/commands/diff_e3_sombra.py:115::Command.comparar
app/chamados/services/tarefa_do_dia.py:569::tarefa_do_dia

CargaDaTarefa.tarefa:
app/chamados/services/tarefa_do_dia.py:572::tarefa_do_dia

cobranca_viva_do_dia:
app/chamados/models.py:390::ChamadoColaborador._plano_pergunta_viva
app/chamados/models.py:749::ChamadoColaborador._gate_premissa_do_fechamento
app/ponto/services/cartorio.py:1655::_julgar_colab_corpo
app/ponto/services/entrada_sincronizada.py:29::resolver_cobranca_da_entrada
app/ponto/services/triagem_batida.py:590::_autoresolve_f2_f4

motivos_a_perguntar:
app/chamados/management/commands/disparar_perguntas_competencia.py:115::contador_fio_mudo
app/chamados/services/fabrica_da_celula.py:207::perguntar_pela_celula
app/chamados/services/fabrica_da_celula.py:242::motivos_que_o_evento_deixou

montar_chamados_fio:
app/chamados/services/fio.py:446::contexto_parcial_fio
app/chamados/views.py:569::meu_atendimento
app/chamados/views.py:677::fio_colaborador
app/chamados/views.py:889::modal_fio_colaborador._resp_modal_full

creditos_perguntas:
app/chamados/services/fio.py:390::montar_chamados_fio
app/chamados/services/verbos.py:34::particionar_verbos

pergunta_sem_acao_possivel:
app/chamados/services/fio.py:284::montar_chamados_fio

relatorios_disputa.classificar:
app/chamados/relatorios_disputa.py:47::coletar

cobranca._calados:
app/chamados/services/cobranca.py:257::linhas_para_a_pauta
```

Escritores ORM dentro de `tarefa_do_dia.py`: nenhum candidato encontrado pelos nomes `save/create/update/delete/bulk_create/bulk_update/get_or_create/update_or_create`, salvo seis `.update` de dicionários em memória. `app/chamados/services/tarefa_do_dia.py:205`, `:214`, `:249`, `:252`, `:333`, `:553`. O carregador monta os insumos, e a tarefa retorna o resultado: `app/chamados/services/tarefa_do_dia.py:361`, `:523`, `:572`.

Candidatos relevantes a leitura indireta C6:

```text
app/api/views.py:2543::api_disputa_perguntas._item -> p.estado
app/chamados/management/commands/arquivar_competencia_encerrada.py:158::Command._disputa_viva -> p.viva
app/chamados/services/explicador.py:72::explicar_pergunta -> p.estado
app/chamados/services/fio.py:614::processar_post_fio -> _p.estado
app/chamados/services/vigia_fio.py:311::dono_fora_do_fio -> p.estado
app/chamados/services/cobranca.py:229::_calados -> PerguntaDisputa.q_vivas
app/chamados/services/fio.py:224::montar_chamados_fio -> PerguntaDisputa.q_vivas
app/chamados/services/janela_colab.py:63::chamados_visiveis_colab -> PerguntaDisputa.q_vivas
app/chamados/services/pergunta_no_app.py:50::perguntas_do_app -> PerguntaDisputa.q_vivas
app/chamados/services/raiox.py:74::creditos_perguntas -> PerguntaDisputa.q_vivas
app/chamados/services/vigia_fio.py:748::viva_em_disputa_fechada -> PerguntaDisputa.q_vivas
app/ponto/services/veredito_celula.py:181::fechar_o_fio_do_dia -> PerguntaDisputa.q_vivas
```

`q_vivas` declara contenção generosa, não igualdade com o C6, e encaminha quem precisa da resposta exata a `pergunta_viva`/`.viva`. `app/chamados/models.py:3328`, `:3340`, `:3343`. O template lê `.viva`, mas também carimbos crus nos ramos de resposta. `app/templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:155`.

O gate **ampliado**, incluindo todos os consumidores de C6/fio_vivo, cruza o anexo LASTRO nestes três arquivos: `app/colaboradores/services/calendario.py:476`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/cartorio.py:1389`. A presença dos três no anexo é dado fornecido pelo usuário [nao verificado]. Isso precisa ficar separado do gate estrito dos arquivos MUDA+respondedores+testes; não faz desses três automaticamente arquivos a alterar.

Os 309 sítios de chamada em testes, agrupados por arquivo, abaixo. As funções completas são impressas pelo `TEST_FILE` do script integral enviado. [nao verificado]

```text
app/api/tests/test_api_mensageria_pendencias.py:78 [1]
app/chamados/tests/test_adesao_por_colab.py:61 [1]
app/chamados/tests/test_b2d_resposta_visivel.py:76,99 [2]
app/chamados/tests/test_bug117_marco_no_espelho.py:128,135,143,153 [4]
app/chamados/tests/test_c2_endpoints_janela.py:37,45 [2]
app/chamados/tests/test_c7_red_minimos.py:124 [1]
app/chamados/tests/test_clique_morto.py:67,76,91,92,99,109 [6]
app/chamados/tests/test_competencia_trancada.py:127,134,156,201 [4]
app/chamados/tests/test_contract_pergunta_viva.py:56 [1]
app/chamados/tests/test_cq_c8.py:37,39,40 [3]
app/chamados/tests/test_e1_entradas.py:152 [1]
app/chamados/tests/test_e1_v10.py:229,239 [2]
app/chamados/tests/test_e1_v15c2_descarimbo_efeito.py:44,65,79 [3]
app/chamados/tests/test_e1_v6_horizonte.py:470,476,538,550 [4]
app/chamados/tests/test_e1_v9.py:171,202 [2]
app/chamados/tests/test_e2_pergunta_so_ao_c6.py:95,103 [2]
app/chamados/tests/test_e2_tarefa_do_dia.py:139,321,443,446,523,547,611,613,613,683,684,684,883,884,895,907,908,909,910,933,936,1134,1169,1170 [24]
app/chamados/tests/test_e3_c8c_orm.py:40,45,46,57,65,66,66,76,90 [9]
app/chamados/tests/test_e3_portas_tarefa.py:19,109,120,121,122,124 [6]
app/chamados/tests/test_e4a2_nunca_bateu.py:158 [1]
app/chamados/tests/test_e4a_relogio_injetado.py:34,35 [2]
app/chamados/tests/test_e4b_c11_orm.py:145,169 [2]
app/chamados/tests/test_e4b_c12_orm.py:61,93 [2]
app/chamados/tests/test_e4b_c4_orm.py:473 [1]
app/chamados/tests/test_e4b_c6_orm.py:80,81,82,83 [4]
app/chamados/tests/test_exibicao_so_viva.py:76 [1]
app/chamados/tests/test_f1c4_3_emissores.py:78 [1]
app/chamados/tests/test_fechar3_retorno_do_gate.py:100,108,139 [3]
app/chamados/tests/test_fio_ancora_destino.py:60,68,79 [3]
app/chamados/tests/test_fio_explicacao.py:33 [1]
app/chamados/tests/test_fio_le_o_juiz.py:103 [1]
app/chamados/tests/test_hx_pergunta_viva_topo.py:43 [1]
app/chamados/tests/test_hx_tipo_pelo_marco.py:161,163 [2]
app/chamados/tests/test_invariantes_disputa.py:145 [1]
app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:151 [1]
app/chamados/tests/test_limbo_nao_fecha_com_filho_no_defeito.py:85,127 [2]
app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:104,119 [2]
app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150,238,265 [3]
app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:98,98 [2]
app/chamados/tests/test_o261_fabrica_le_a_lampada.py:57 [1]
app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98,99,104,111,184 [5]
app/chamados/tests/test_o261_lavra_le_a_lampada.py:138,163 [2]
app/chamados/tests/test_o261_pergunta_viva_teto.py:55,60,65,89 [4]
app/chamados/tests/test_o261_recusa_fala.py:105,127,132,177,183 [5]
app/chamados/tests/test_o261_resposta_vazia.py:113 [1]
app/chamados/tests/test_o261_vigia_fio.py:134,135,238,241 [4]
app/chamados/tests/test_o261e_f1_porta_encerramento.py:75,436,446 [3]
app/chamados/tests/test_o261e_f2_fila.py:246 [1]
app/chamados/tests/test_o261e_f2_porta_carimbo.py:668,731 [2]
app/chamados/tests/test_o261e_f2_ramo_e_i20.py:186,202,237,260 [4]
app/chamados/tests/test_o261e_f5b_juiz_porta.py:81,106,188,189 [4]
app/chamados/tests/test_o318_card_nao_oferece_o_que_a_porta_recusa.py:102,222,237 [3]
app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:123,219,230 [3]
app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:177,183 [2]
app/chamados/tests/test_orfa_sem_candidato.py:70 [1]
app/chamados/tests/test_orfas_pelo_evento.py:102,110,354,378 [4]
app/chamados/tests/test_particionar_verbos.py:39 [1]
app/chamados/tests/test_pergunta_do_dia_em_curso.py:89,90,92,99,289,316,457,507 [8]
app/chamados/tests/test_pergunta_no_app.py:85 [1]
app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:54 [1]
app/chamados/tests/test_pergunta_viva.py:18,22,27,36,42,47,51,60,67,72,77 [11]
app/chamados/tests/test_porta_veredito_pergunta.py:427,428,438,454 [4]
app/chamados/tests/test_profile_e53.py:86 [1]
app/chamados/tests/test_pvmuda_carimbo_em_voo.py:76,90,116,125,127,275,281,282 [8]
app/chamados/tests/test_reabrir_orfas_enterradas.py:103 [1]
app/chamados/tests/test_reabrir_pendente_pelo_juiz.py:67,68 [2]
app/chamados/tests/test_reconciliador_c1_escopo.py:48 [1]
app/chamados/tests/test_relatorio_pendencias.py:35,39,43,47,53,59 [6]
app/chamados/tests/test_relatorio_pendencias_lei.py:22,31 [2]
app/chamados/tests/test_resposta_nao_e_fantasma.py:86 [1]
app/chamados/tests/test_selo_celula_absolveu.py:72,85,117 [3]
app/chamados/tests/test_selo_defer_json_creditos.py:80,94 [2]
app/chamados/tests/test_selo_emissor_fio_mudo.py:269 [1]
app/chamados/tests/test_selo_get_nao_escreve.py:88,101 [2]
app/chamados/tests/test_selo_i6_ouve_a_lei.py:114,130 [2]
app/chamados/tests/test_selo_i8_fio_solto.py:95,119,167,179 [4]
app/chamados/tests/test_selo_i8b_resolvedora_cruzada.py:83 [1]
app/chamados/tests/test_selo_identidade_fio_explicacao.py:78,82 [2]
app/chamados/tests/test_selo_juizes_orfaos.py:73,79 [2]
app/chamados/tests/test_selo_memo_pergunta_viva.py:86,87,88,108,109,110,112,124,126,129,132,137,140,148,150,153,156,164,166,174,176,177 [22]
app/chamados/tests/test_snc2_toque_em_lote.py:85,95,98,144,146,148,161,163,179,181,195,197,220,234,238 [15]
app/chamados/tests/test_t34_furo_sob_limbo.py:55 [1]
app/chamados/tests/test_tranca_de_verdade.py:52,59,82,87 [4]
app/chamados/tests/test_viva_delega_ao_juiz.py:42 [1]
app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:56 [1]
app/core/tests/test_selo_performance.py:480,482 [2]
app/escala/tests/test_f3_beco1_folga_na_lista.py:80,148,173,191,193 [5]
app/ponto/tests/test_bug114_fechador_de_dois_dias.py:239 [1]
app/ponto/tests/test_cobparc_marco_no_periodo.py:386 [1]
app/ponto/tests/test_cods_do_ato.py:124 [1]
app/ponto/tests/test_cxr_tela_le_o_motor.py:367,368 [2]
app/ponto/tests/test_fechar3_troca_escala_gate.py:152,203,204,253,254 [5]
app/ponto/tests/test_hx_cartorio_ressuscita.py:56,65 [2]
app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:102 [1]
app/ponto/tests/test_o261_cobrado_le_o_fio.py:134,147,161,195,309,325,349,419 [8]
app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84,91,150,175,184 [5]
app/ponto/tests/test_x1_folga_sem_adesao.py:325,391 [2]
```

O script não encontrou chamadas desses alvos em arquivos `bin/`; isso é resultado da execução sem relatório persistido [nao verificado]. Referências textuais em shell/documentação não equivalem a arquivo compartilhado nem a chamada Python provada.

Limites: `.viva/.estado` de modelos diferentes aparecem entre os 214 candidatos; não declarar 214 leitores C6. Dispatch dinâmico, atribuições em tupla e consumidores de ordem superior não estão provados [nao verificado]. AST Python não julga HTML/JS: o ramo do template requer leitura textual; o script imprime o restante para inspeção.

_4122 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
