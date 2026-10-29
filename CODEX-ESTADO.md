# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 15:51** · publicado em **29/10/2026 15:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 15:51 · sessao 1ee182181f58 · - · - · origem=Ronald (tmux)

HEAD lido: `7596ef5e8351bf7473acecafc8348bca7d2cf9d7`. Somente leitura estática; ocorrências reais no banco: **[nao verificado]**.

**(1) Escritores de `respondida_em`**

| Escritor | Grava `resposta_colab` na mesma operação? | Condição |
|---|---|---|
| `responder_pergunta_disputa_core` — `app/chamados/services/disputa_emissao.py:1247–1266` | **Sim.** | Rejeita resposta vazia antes da escrita (`:1219–1221`). |
| `responder_automaticamente` — `app/chamados/services/disputa_emissao.py:1381–1394` | **Sim.** | Recebe texto e instante; não verifica se o texto recebido é vazio. O chamador encontrado fornece texto constante de foto recebida (`app/api/foto_recebida.py:26`, `:42–51`). |
| `carimbar_resposta_pela_trilha` — `app/chamados/services/disputa_emissao.py:1398–1410` | **Não.** | Exige texto já preenchido e carimbo nulo (`:1407`). |
| `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:506–530` | **Não.** | Só atribui instante quando existe texto não vazio (`:511–513`); o `save` inclui o campo mesmo quando permanece nulo (`:528–530`). |
| `_materializar_uma` — `app/chamados/services/materializacao.py:1190–1198` | **Não.** | Só preenche carimbo nulo quando existe texto não vazio (`:1190–1193`). |
| `resolver_perguntas_por_chamado_resolvido`, ramo de destino — `app/chamados/services/disputa_emissao.py:1570–1583` | **Não.** | Exige texto existente para preencher o carimbo (`:1570–1573`). |
| Mesma função, ramo **sem credor vivo** — `app/chamados/services/disputa_emissao.py:1640–1651` | **Não; tampouco exige texto.** | Se o juiz considera a pergunta morta, materializa e preenche `respondida_em` quando nulo (`:1640–1651`). |
| `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1358–1376` | **Apaga ambos.** | Guarda histórico e faz `update(respondida_em=None, resposta_colab='', …)` no mesmo bloco transacional (`:1358`, `:1365–1375`). |
| `ChamadoColaborador.ressuscitar_perguntas` — `app/chamados/models.py:1160–1163` | **Não altera o texto.** | Universo exige texto vazio/nulo e materialização existente; exclui marco já aceso (`:1112–1120`). Limpa `respondida_em` junto dos outros carimbos (`:1162–1163`). |

**Existe caminho automático com carimbo e resposta vazia:** o ramo sem credor vivo não testa nem escreve texto; `resposta_colab` tem default `''` (`app/chamados/services/disputa_emissao.py:1640–1651`; `app/chamados/models.py:1865–1866`). Esse ramo procura disputas abertas do colaborador, exige pai e destinos fora de `VIVOS` e perguntas sem validação/materialização; o filtro efetivo não limita `tipo_resposta` (`app/chamados/services/disputa_emissao.py:1621–1642`).

Os acessos operacionais encontrados às portas incluem:

| Caminho | Evidência |
|---|---|
| Resposta web/API/ação/texto | Chamam o core em `app/chamados/views.py:1283`, `app/api/views.py:2695`, `app/chamados/services/acoes_disputa.py:367`, `app/chamados/services/declaracao_texto.py:365`, `:638`. |
| Manutenção de perguntas em limbo | `curar_perguntas_em_limbo` chama carimbo pela trilha ou reabertura; escrita condicionada a `--apply` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:63–69`). Esse carimbador exige texto (`app/chamados/services/disputa_emissao.py:1407`). |
| Fechamento automático de chamado | `post_save` de chamado encerrado chama o resolvedor (`app/chamados/signals.py:75–86`). |
| Reconciliador/manutenção | Chamam o resolvedor em `app/chamados/reconciliador.py:469`, `:1370`, `app/chamados/services/becos.py:49`, `app/ponto/management/commands/reconciliar_fantasmas.py:280`. |
| Consumidor de eventos | Chama retratação e encerramento de órfãs (`app/chamados/reconciliador.py:1145–1149`). A retratação chega ao resolvedor por `_retratar` (`:800`, `:456–469`); o encerramento fantasma grava apenas via/materialização/autor (`app/chamados/models.py:2170–2173`). |

**Competência trancada não escreve `respondida_em`:** a porta grava somente via, materialização e autor; conserva resposta e carimbo anteriores (`app/chamados/models.py:2143–2148`). O resolvedor retorna imediatamente para chamado encerrado por essa via (`app/chamados/services/disputa_emissao.py:1531–1532`).

Também não escrevem esse campo `fechar_por_admin`, `encerrar_fantasma`, `encerrar_por_fato_no_espelho` e a lavra de “sem turno aberto” (`app/chamados/models.py:2131–2134`, `:2156–2159`, `:2170–2173`; `app/chamados/reconciliador.py:402–408`).

Escritas dinâmicas sem o nome do campo, SQL externo e escritores em testes/migrations: **[nao verificado]**; testes/migrations não li integralmente. [nao verificado]

**(2) `o_que_segura`: motivo, objeto e tela**

A função separa a classificação geral do colaborador dos dias retidos: `classificar_export` fornece `entra/motivo`; `motivos_retencao_celula` fornece `dias` (`app/folha/fora_da_folha.py:188–194`). Portanto, lista de dias vazia não basta para afirmar que entra (`:211–213`).

Os motivos **por dia** são:

| Motivo | Predicado que retém | Objeto/estado cuja mudança é necessária; acesso encontrado |
|---|---|---|
| `sem_celula`, célula inexistente | D passado, vínculo cobrindo D e D diferente da demissão (`app/folha/export.py:1195–1214`). | É necessário produzir/julgar a `CelulaDia`; fechar chamado não consta desse predicado (`:1212–1214`). Tela que garanta criar a célula ausente: **[nao verificado]**. |
| `sem_celula`, célula existente | `veredito is None`, `trabalha is not None`, D passado e não demissão (`app/folha/export.py:1237–1238`). | O objeto lido é a célula, cujo veredito precisa deixar de satisfazer o predicado. Existe decisão de dia no calendário, mas criação/julgamento completo desse caso por essa tela: **[nao verificado]** (`app/ponto/services/veredito_celula.py:323–335`). |
| `espelho_<veredito>` | Célula acusadora, sem isenção integral por protestos não cobráveis e sem isenção por pausa (`app/folha/export.py:1215–1236`). | O predicado lê **veredito/protestos/ata da célula**, não status do chamado. A saída depende de novo estado da célula ou de uma das exceções (`:1227–1236`). |

Os sufixos acusadores declarados são `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu` e `fato_em_ausencia` (`app/escala/models.py:1735–1736`). A retenção aplica o mesmo ramo a todos; não contém uma associação individual “veredito → objeto/tela” (`app/folha/export.py:1215–1236`).

A ação nomeada é escolhida pelo **fio**, separadamente:

| Fio | Objeto/ato nomeado | Tela/view comprovada |
|---|---|---|
| `colab` | Pergunta viva pendente: colaborador responde; depois admin valida (`app/folha/fora_da_folha.py:66–68`, `:40`). | `responder_pergunta_disputa`, por ID da pergunta, exige colaborador dono e pergunta viva (`app/chamados/urls.py:17`; `app/chamados/views.py:1228–1246`). |
| `admin` | Pergunta viva respondida: validar ou recusar (`app/folha/fora_da_folha.py:69–70`, `:39`). | Formulários no fio levam `pergunta_id/chamado_id`; views delegam às ações de validar/recusar (`app/templates/chamados/partials/modal_fio.html:272–288`; `app/chamados/views.py:483–518`). |
| `chamado` | Resolver os chamados vivos ligados à célula; a ferramenta inclui suas PKs (`app/folha/fora_da_folha.py:198–210`). | Retrato oferece modal do fio por **colaborador** (`app/folha/retrato_folha.py:977`, `:1020`). O botão de validação não prova solução universal: exige disputa e perguntas respondidas; sem disputa retorna erro (`app/chamados/views_cobrar.py:160–169`). |
| `nada` | O código declara “SEM FIO”, sem ato específico (`app/folha/fora_da_folha.py:42`, `:73`). | Não há porta pelo fio no mapa do retrato (`app/folha/retrato_folha.py:978`). |

Portas concretas para os fatos/cadastro do dia:

- **Resolver dia no calendário:** formulário recebe colaborador/data e oferece `trabalhou`, `faltou`, `justificado` (`app/templates/colaboradores/partials/_calendario_grade.html:125–126`; `app/templates/ponto/partials/_veredito_dia.html:17–25`). A view exige POST e `regularizar_batida` (`app/ponto/views.py:3284–3313`).
- `trabalhou` pode corrigir tipos e gravar batidas manuais; `faltou/justificado` criam `Ausencia` aprovada; depois fecha fio e lavra veredito administrativo na célula existente (`app/ponto/services/veredito_celula.py:290–329`). A view reconcilia o trecho em seguida (`app/ponto/views.py:3321–3323`).
- **Escala/folga/sem escala:** o retrato aponta para `painel_vinculo`; cadastro aponta para `painel_colaborador` (`app/folha/retrato_folha.py:987–992`).
- **Ausência já ligada a chamado:** `ir_para_ausencia` exige módulo `ausencia_colab`, lê `ausencia_id` e redireciona ao registro (`app/chamados/views.py:1494–1519`). O mapa do retrato não tem ponte por colaborador para essa porta (`app/folha/retrato_folha.py:982–985`).

Além dos dias, o **motivo geral** pode ser:

| Motivo geral | Objeto/acesso comprovado |
|---|---|
| `rescisao_modulo_proprio` | Determinado por demissão; o código remete ao módulo próprio de rescisão (`app/folha/export.py:834–845`). |
| `sem_codigo_dominio` | `Colaborador.codigo_dominio`; edição recebe `codigo_folha` e atribui esse campo (`app/folha/export.py:846–849`; `app/colaboradores/views.py:1331–1334`). |
| `ferias_com_batida` | Conflito entre férias e batidas; porta específica que solta cada caso: **[nao verificado]** (`app/folha/export.py:850–858`). Retrato não oferece porta por tipo férias (`app/folha/retrato_folha.py:993`). |
| `cadastro_zero` | O texto do classificador indica vínculo/escala e recálculo (`app/folha/export.py:933–940`); painel vínculo está no mapa (`app/folha/retrato_folha.py:987–989`). |
| `furo_espelho` | Motivos por célula não vazios; casos detalhados acima (`app/folha/export.py:942–947`). |
| `nao_homologado` | Códigos de eventos fora do catálogo homologado (`app/folha/export.py:948–986`). Tela de rubricas vincula eventos/códigos (`app/folha/views.py:27–48`); porta de manutenção do conjunto homologado: não li. |

Fechar pergunta ou chamado, isoladamente, não consta dos predicados de retenção por dia; eles leem célula, vínculo e exceções (`app/folha/export.py:1193–1238`).

**(3) Universo do `vigia_fio.censo`**

A consulta inicial parte de **`ChamadoColaborador` em `VIVOS`, com `celula_id`**, opcionalmente por empresa; não parte dos dias retidos da folha nem de `qs_na_fila` (`app/chamados/services/vigia_fio.py:183–187`).

Depois separa módulos de furo, carrega células acusadoras e todas as perguntas dessas células (`app/chamados/services/vigia_fio.py:199–203`, `:226–227`, `:242–244`).

- **Chamado vivo silenciado/arquivado:** entra na consulta inicial, pois ela não filtra `silenciado_ate` (`app/chamados/services/vigia_fio.py:184–187`). Já `qs_na_fila` exclui silêncio futuro (`app/chamados/services/painel.py:186–188`).
- Pode contar como `fio_morto_cobrando` mesmo silenciado: o indicador de cobrança usa apenas status `COBRANCA`; o incremento não relê silêncio (`app/chamados/services/vigia_fio.py:199–200`, `:286–289`).
- **Pergunta respondida e destino fechado:** isso sozinho não determina entrada. Se não houver **outro chamado vivo ligado à célula**, o dia fica fora desde a consulta inicial (`app/chamados/services/vigia_fio.py:184–187`).
- Se outro chamado vivo trouxer a célula, a pergunta respondida pode contar como fio vivo: texto não vazio com carimbo, sem validação/tranca e sem decisão administrativa pode permanecer vivo independentemente do destino fechado (`app/chamados/juizes.py:1006–1032`). O censo considera todas as perguntas da célula (`app/chamados/services/vigia_fio.py:242–259`).

Entrar no universo não significa contar como morto: outro vocabulário, pergunta viva, hoje/futuro, decisão de dia oferecida, ausência aberta cobrindo ou folga contestada validada têm saídas próprias (`app/chamados/services/vigia_fio.py:253–285`). O censo também não percorre os dias `sem_celula` da folha: exige chamado com célula e depois célula acusadora (`:184`, `:226–227`; retenção sem célula em `app/folha/export.py:1212–1214`).

**(4) Reabertura e `silenciado_ate`**

| Porta | Muda para vivo? | Limpa silêncio? |
|---|---|---|
| `ChamadoColaborador.reabrir` | `resolvido/fechado → em_analise`, passando pelas guardas; depois ressuscita perguntas (`app/chamados/models.py:1050–1098`; `app/chamados/catalogo/motor.py:113`). | **Não.** O núcleo grava status/carimbos/urgência, sem campos de silêncio (`app/chamados/models.py:193–217`). |
| `transicionar → _aplicar` | A matriz humana permite retorno de resolvido para análise; há autorização ampliada de admin TI (`app/chamados/catalogo/motor.py:99–105`, `:139–156`; `app/chamados/models.py:257–260`). | **Não**, usa o mesmo núcleo (`app/chamados/models.py:193–217`). |
| `_renascer_por_premissa_viva` | Delega a `reabrir`, após guardas de situação, cascade e competência (`app/chamados/models.py:763–770`). `abrir` pode acionar esse renascimento no dedup (`:635`, `:686`). | **Não**, herda a porta anterior. |
| `arquivo.levantar` | **Não altera status.** Grava somente campos de silêncio/resgate/atualização (`app/chamados/services/arquivo.py:199–208`). | **Sim** (`:201–203`). |
| `desarquivar_chamado` | Só chama `reabrir` no ramo de competência trancada; depois levanta o silêncio (`app/chamados/views_arquivar.py:64–77`). | **Sim**, quando chega ao levantamento (`:75–77`). |
| Resgate por resposta | Escolhe destino ou pai e chama `arquivo.levantar` (`app/chamados/services/materializacao.py:655–667`). | **Sim**; esse trecho não muda status. |
| Reconciliação do silêncio de ausência | Levanta conforme estado da ausência/premissa (`app/chamados/services/silencio_ausencia.py:68–75`, `:93–114`). | **Sim**, sem carimbar resgate humano. |

Chamadores de `reabrir` encontrados sem levantamento de silêncio no ato: adesão, ação administrativa da disputa, reabertura de competência, resolvedor de respostas, resposta tardia e comandos de reabertura/reconciliação (`app/chamados/services/adesao.py:92`; `app/chamados/services/acoes_disputa.py:259–266`; `app/chamados/services/competencia_trancada.py:252–265`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/services/resposta_tardia.py:35–47`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:48–53`; `app/ponto/management/commands/reconciliar_grade.py:518–519`; `app/ponto/management/commands/detectar_ausencias.py:300–301`).

**Existe caminho para vivo + silêncio futuro sem exigir quarentena do colaborador:** a própria tela de arquivar chama `arquivo.silenciar`, preservando status e alterando somente campos do chamado; não condiciona o ato à quarentena (`app/chamados/views_arquivar.py:24–42`; `app/chamados/services/arquivo.py:156–171`). Também uma reabertura aceita conserva silêncio anterior, pois `_gravar_estado` não o limpa (`app/chamados/models.py:1087–1089`, `:193–217`). Ocorrência real desse estado: **[nao verificado]**.

**(5) Link desde o retrato até o dia/pergunta/chamado**

**Há navegação indireta por colaborador; não há link específico em cada data bloqueada no trecho lido.** As datas são `span`s; as portas recebem apenas ID do colaborador (`app/templates/folha/_retrato_o_que_fazer.html:43–46`; `app/folha/retrato_folha.py:1010–1024`).

| Etapa | View/template e informação transportada |
|---|---|
| Retrato | Rota → `views_retrato.retrato_folha` (`app/folha/urls.py:23`). |
| Clique no nome | Abre `painel_colaborador` com colaborador, mês e ano (`app/templates/folha/_retrato_lista.html:60–62`; `app/colaboradores/urls.py:48`). |
| Bloco “o que fazer” | View lê a linha **gravada** de `RetratoFolha`; monta textos e portas dessa linha (`app/colaboradores/views.py:975–985`). Partial incluído no painel (`app/templates/colaboradores/partials/painel_colaborador.html:26`). |
| Datas bloqueadas | Exibidas sem links individuais (`app/templates/folha/_retrato_o_que_fazer.html:43–46`). A linha gravada não contém IDs de pergunta/chamado/célula (`app/folha/retrato_folha.py:1177–1188`). |
| Botões do retrato | Fio colab/chamado → modal do fio; escala/folga/sem escala → painel vínculo; cadastro → painel colaborador. Outros tipos podem não ter porta (`app/folha/retrato_folha.py:974–995`). Todos usam somente ID do colaborador (`:1020`). |
| Modal do fio | `modal_fio_colaborador`, por colaborador, sem dia/competência na URL gerada (`app/chamados/urls.py:39`; `app/chamados/views.py:869–889`; `app/folha/retrato_folha.py:1020`). |
| Pergunta/chamado dentro do fio | Forms de validar/recusar levam IDs de pergunta e chamado; cobrança leva chamado (`app/templates/chamados/partials/modal_fio.html:272–288`, `:343`). |

O texto do retrato pode conservar literalmente “chamado N”: é lookup de `DESTRAVA`; a substituição pelos números concretos existe em `o_que_segura`, mas não nesse lookup (`app/folha/retrato_folha.py:1068–1069`; `app/folha/fora_da_folha.py:207–210`).

Há outra navegação: **painel → aba Espelho → calendário → Resolver dia** (`app/templates/colaboradores/partials/painel_colaborador.html:33`, `:180–182`; `app/templates/colaboradores/partials/_calendario_grade.html:125–126`). Porém o carregamento inicial da aba pede `/colaboradores/<id>/calendario/` **sem mês/ano do retrato**; o calendário permite navegar o período depois (`app/templates/core/_drawer_generico.html:132–142`; `app/templates/colaboradores/calendario_partial.html:37–55`).

## 29/10 15:50 · sessao df5ef1ecfe7e · - · - · origem=Ronald (tmux)

Complemento T21 itens 2/5:

- **Existe porta direta “Resolver dia”** no calendário: `templates/colaboradores/partials/_calendario_grade.html:125-126` inclui `ponto/partials/_veredito_dia.html` passando colaborador e dia. O formulário usa `colab`, `data`, `volta`, oferece `trabalhou/faltou/justificado` e tipo de ausência (`templates/ponto/partials/_veredito_dia.html:17-23`). “Cobrar este dia” reutiliza esses campos em outra porta (`:25`).
- URL de decisão: `ponto/urls.py:58`→`veredito_celula_view`; exige POST e permissão `regularizar_batida`, lê `colab`, `datas` ou `data`, valida ISO e opção de veredito, chama serviço por dia (`ponto/views.py:3284-3313`). Em sucesso reconcilia o trecho (`:3321-3323`).
- Essa porta **não significa um único tipo de conserto para cada veredito acusador**. Despacha por fato declarado: `trabalhou` pode inverter tipos via escritor canônico (`ponto/services/veredito_celula.py:290-297`), grava batidas `manual`, com autoridade humana e aprovador (`:298-304`), depois `recompute_turnos(D-1,D+1)` (`:307`). `faltou`/`justificado` criam `Ausencia` de um dia já aprovada (`:309-316`).
- Ao final fecha fio do dia e, se célula existente, chama `cel.lavrar_veredito(via='admin',veredito=...,ator=autor)` (`veredito_celula.py:323-329`). `fechar_o_fio_do_dia` fecha perguntas vivas por colaborador+data e reconcilia chamados vivos (`:174-187`). Caso sem célula: serviço guarda log com objeto_id zero (`:328-335`); criação posterior da célula nesse fluxo: [não verificado].
- **Tela necessária é alcançável desde a ficha**, porém há quebra de contexto temporal: ficha tem aba “Espelho” (`templates/colaboradores/partials/painel_colaborador.html:33,180-182`); JS `trocarAbaColab` carrega `/colaboradores/<cid>/calendario/` **sem mes/ano**, mesmo que o nome do retrato tenha aberto ficha com competência (`templates/core/_drawer_generico.html:132-142`). Assim o carregamento inicial não leva os parâmetros do retrato. Calendário permite navegar mês/ano/modo depois (`templates/colaboradores/calendario_partial.html:37-55`).
- View do calendário exige `ver_cadastro`, carrega colaborador e passa GET para `contexto_calendario`; habilita decisão apenas com `regularizar_batida`, oferecendo tipos vigentes de ausência (`colaboradores/views.py:1935-1962`); rota `colaboradores/urls.py:24`.
- Há também portas por **batida existente** no mesmo calendário: inverter tipo e retratar com motivo/observação (`templates/colaboradores/partials/_calendario_grade.html:121`); estas ações usam a PK da batida, diferente de “Resolver dia”. Cadeia completa dessas duas escritas até rejulgamento: não li.
- **Declaração de folga/falta por lote** também existe como serviço: `ponto/services/declarar_dia.py:147-162` revalida e despacha por dia. Folga chama `lancar_folga` dentro de transação e fecha fio (`:172-201`); falta reutiliza `veredito_celula(...,'faltou')` (`:205-211`). Chat serve preview/aplicação dessa declaração (`core/views_ia.py:703,717,748,756`); execução completa do intake até o serviço: não li.
- **Rubricas**: tela `/rubricas/` (`folha/urls.py:8`) exige `editar_folha`, aceita evento/código e escopo empresa/global e chama `vincular_evento` (`folha/views.py:27-48`). Autoridade cria/atualiza `RubricaDominio` com código, ativo e catálogo (`folha/rubricas.py:17-31`). Catálogo FK só é escolhido quando descrição é inequívoca (`:11-14`); isso é diferente do gate de export, que compara `codigo_fopag.zfill(4)` ao conjunto de códigos homologados (`folha/export.py:968-986`). Porta de importar/manter o conjunto homologado: não li.
- **Vínculo/cadastro**: links do retrato para painel vínculo e cadastro já comprovados (`folha/retrato_folha.py:987-992`). Cadeia de gravação de vínculo, pós-vínculo e rejulgamento será consolidada pelo root; não reli nesta tarefa.

## 29/10 15:48 · sessao df5ef1ecfe7e · - · - · origem=Ronald (tmux)

T21 itens 2 e 5, fonte `7596ef5e8351bf7473acecafc8348bca7d2cf9d7`. Somente leitura; não executei testes/banco/escrita.

**O que `o_que_segura` realmente entrega**

- `folha/fora_da_folha.py:188-193`: consulta janela da competência, obtém classificação geral de `classificar_export` e, separadamente, os dias de `motivos_retencao_celula`. Portanto `motivo` geral e lista `dias` respondem perguntas diferentes; lista vazia não prova `entra`.
- `folha/fora_da_folha.py:198-210`: lista por dia contém `data`, `veredito`, `motivo`, `fio`, texto `destrava` e **PKs dos chamados vivos**. Não inclui PK de pergunta nem célula no retorno.
- `folha/fora_da_folha.py:51-55,66-73`: perguntas são carregadas por `celula_id`; primeiro vence pergunta viva pendente (`colab`); outra pergunta viva→`admin`; sem pergunta viva mas chamado VIVO→`chamado`; restante→`nada`. Associação do chamado é pela célula, não necessariamente pergunta.disputa.chamado.

**Cada condição que coloca um dia em `dias`**

| Motivo | Critério exato | Objeto/estado que precisa mudar para deixar de satisfazer o critério |
|---|---|---|
| `sem_celula`, célula inexistente | D passado, com vínculo cobrindo D, D diferente da demissão; `export.py:1195-1214`. | Produzir `CelulaDia` do dia com julgamento que não retenha; o simples fechamento de chamado não aparece no predicado. Porta/tela específica de julgamento forçado desse dia: [não verificado]. |
| `sem_celula`, célula existente | `veredito is None`, `trabalha is not None`, D passado e não demissão; `export.py:1237-1238`. | Julgar/lavrar veredito da célula. Não basta trocar status de pergunta/chamado: esses objetos não são lidos nesse ramo (`export.py:1237-1238`). |
| `espelho_<veredito>` | Célula acusadora e não inteiramente isenta por protestos não cobráveis e não isenta pela pausa; `export.py:1215,1227-1236`. | Corrigir o fato/cadastro/ata que sustenta a acusação e obter novo julgamento; critério de saída é deixar `VEREDITOS_ACUSAM` ou satisfazer uma das duas exceções1235. |
| Valores possíveis do sufixo | `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`; `escala/models.py:1735-1736`. | Não existe associação individual “um veredito→uma tela” nessa função; escolha depende do fato e fio. |
| Exceção pausa | `protestos == {'FURO_PARCIAL'}` e só pausas apagadas; `export.py:1147`. Exige leitura de papéis sem desalinhamento, ponta aberta ou “não sei”; `1122-1129`. | Não exige preencher pausa para sair da retenção se já cumpre essa exceção. |
| Exclusões gerais | Isento Art.62 sai antes1174-1178; recorta admissão/demissão1180-1187; sem célula **e** sem vínculo não entra1209-1210. | Essas exclusões são parte do universo, não ações sugeridas ao operador. |

Não há restrição `d<hoje` no ramo de veredito acusador (`export.py:1215-1236`); ela existe nos dois ramos `sem_celula` (`1213,1237`).

**Fio→ato e porta existente**

| Fio | O que o código nomeia | Porta comprovada e critérios |
|---|---|---|
| `colab` | Colaborador responde; depois admin valida (`fora_da_folha.py:38-40`). | `chamados:responder_pergunta_disputa`, URL por pergunta (`chamados/urls.py:17`); POST, colaborador autenticado dono da disputa e pergunta viva (`chamados/views.py:1228-1246`). Cobrar é outra ação, não resposta: botão no fio `templates/chamados/partials/modal_fio.html:343`; URL `chamados/urls.py:25`. |
| `admin` | Validar ou recusar resposta (`fora_da_folha.py:39`). | Form por `pergunta_id` e `chamado_id` no fio (`modal_fio.html:272-277`); rejeição `283-288`. Backend delega a `acao_validar_pergunta` (`chamados/views.py:483-501`) ou `acao_recusar_pergunta` (`505-518`). `acoes_disputa.py:19-27` delega ao serviço e recusa sem resposta; erro de gravação é explícito32-41. |
| `chamado` | Resolver os chamados vivos, com números substituídos somente na ferramenta (`fora_da_folha.py:207-210`). | Retrato oferece “Validar no fio” por **colaborador**, não operação de resolver por PK (`retrato_folha.py:977,1020`). `validar_inline` exige disputa aberta vinculada e perguntas vivas respondidas (`views_cobrar.py:160-169`); sem disputa retorna erro167. Portanto esse botão não prova solução para todo fio `chamado` sem pergunta viva. |
| `nada` | “SEM FIO… este dia precisa de alguém para olhar” (`fora_da_folha.py:42`). | Retrato não oferece porta pelo fio (`retrato_folha.py:978`). Uma porta de correção específica por dia nesse caso: [não verificado]. |

“Regularizar disputa” também não é fechamento livre: exige resolução textual e bloqueia qualquer pergunta viva sem resposta; depois materializa validadas (`chamados/services/acoes_disputa.py:118-139`). Fechar o continente por si só não altera o critério de retenção da célula (`folha/export.py:1215-1238`).

**Motivo geral que pode aparecer em `o_que_segura['motivo']` sem ser um dia da lista**

Ordem do classificador:

| Motivo geral / condição | Objeto e porta comprovada |
|---|---|
| `rescisao_modulo_proprio`: `data_demissao != None` (`export.py:840-845`). | Não é pendência a “soltar” pelo chamado; código declara módulo próprio Dominio/TRCT (`834-839`). |
| `sem_codigo_dominio`: código vazio (`846-849`). | `Colaborador.codigo_dominio`; edição cadastral possui tradução `codigo_folha→codigo_dominio` (`colaboradores/views.py:1237,1260`) e atribuição1334. Tela do retrato pode oferecer “Cadastro do colaborador” conforme tipo (`retrato_folha.py:992`). |
| `ferias_com_batida`: mapa de dias com batida dentro de férias (`export.py:850-858`). | Conferir fatos de férias/batida; porta do retrato por tipo `ferias` é explicitamente `None` (`retrato_folha.py:993`). Porta concreta que resolveria cada inconsistência: [não verificado]. |
| `cadastro_zero`: zero horas, batidas apuráveis suficientes e nenhum dia previsto; escreve motivo (`export.py:876-888,910-919,933-940`). | Próprio texto manda cadastrar vínculo/escala e recalcular (`937-938`); retrato tipos escala/folga/sem_escala vão ao painel vínculo (`retrato_folha.py:987-989`). |
| `furo_espelho`: motivos de retenção por célula não vazios (`export.py:942-947`). | Célula/fatos por dia, nos quadros anteriores. |
| `nao_homologado`: códigos não homologados (`948-953`). | Rubricas/catálogo: mapa empresa sobre global (`958-965`); só eventos não zero fora do catálogo, catálogo vazio desliga gate (`968-986`); inclui falta_diurna quando houver datas de falta987-992. Tela listagem rubricas `folha/urls.py:8`; porta concreta de homologação/edição: não li. |

Recalcular fechamento é POST administrativo separado: `ponto/views.py:1226-1252` chama `recalcular_fechamento_mes`. Se isso força rejulgamento da célula em todos esses casos: [não verificado].

**Caminho concreto desde o retrato até pergunta/chamado**

1. URL do retrato é `folha/urls.py:23`; nome da pessoa abre drawer `colaboradores:painel_colaborador` com `mes` e `ano` (`templates/folha/_retrato_lista.html:60-62`); rota do painel em `colaboradores/urls.py:48`.
2. Painel lê **linha já gravada** de `RetratoFolha`, não executa `o_que_segura` (`colaboradores/views.py:973-985`). Só existe bloco com competência válida, empresa e permissão `ver_folha`975.
3. Partial entra no cabeçalho do painel (`templates/colaboradores/partials/painel_colaborador.html:26`). “Datas (ISO)” são `span`s, **sem links por dia** (`templates/folha/_retrato_o_que_fazer.html:43-46`).
4. Linha gravada tem `colab,codigo,status,motivo,dias,datas,dono,tipo,saldo`; não guarda IDs célula/pergunta/chamado (`folha/retrato_folha.py:1177-1188`).
5. Texto “O que destrava” é lookup de `DESTRAVA`, **literal incluindo “chamado N”** (`retrato_folha.py:1051-1055,1068-1069`). A substituição pelos PKs reais ocorre na ferramenta `o_que_segura` (`fora_da_folha.py:207-210`), não nesse drawer.
6. Botões são gerados por `portas_da_linha`: fio colab→“Cobrar no fio”, fio chamado→“Validar no fio”; admin e ninguém não têm porta pelo fio (`retrato_folha.py:974-979`). Tipos escala/folga/sem_escala→painel vínculo; cadastro→painel colaborador; falta/não bate/férias/sem causa→nenhuma (`986-995`).
7. Todos esses endereços usam **apenas ID do colaborador**, sem competência/dia/pergunta/chamado (`retrato_folha.py:1010-1024`), e abrem outro drawer (`_retrato_o_que_fazer.html:60-62`).
8. Modal fio é `chamados/urls.py:39`→`modal_fio_colaborador`; requer gestor e monta chamados do colaborador (`chamados/views.py:869-889`). Não recebe dia nem competência da origem. Contador de mensagens usa competência **atual** (`894-907`).
9. Dentro do fio há cards com formularios por pergunta e chamado (`modal_fio.html:272-287`) e cobrar por chamado343. Assim há caminho indireto até o objeto, mas não link do retrato ao dia/pergunta/chamado específico que o segurou.
10. Ausência tem uma porta por chamado, `chamados:ir_para_ausencia` (`urls.py:44`): exige `modulo_origem='ausencia_colab'`, lê `contexto_json['ausencia_id']`, marca análise e redireciona `/ponto/ausencias/?registro=<id>` (`views.py:1494-1519`). O próprio mapa do retrato declara faltar ponte por colaborador para essa porta (`retrato_folha.py:982-985`).

Escopo não lido: serviço interno de `validar_pergunta`/materialização completa, regras e gravações da tela de ausências/férias, telas internas do catálogo homologado, mecanismo de recalcular→rejulgar. Portanto não afirmo que qualquer clique acima solta automaticamente a folha; o critério final é o novo resultado do classificador/célula (`fora_da_folha.py:191-213`, `export.py:942-953`).

## 29/10 15:48 · sessao 758e5831a301 · grupo1 · verificador · origem=Code

- **Classe 3 — `app/ponto/services/veredito_celula.py:217`:** `_grade` lê `timezone.now()` a cada simulação. Baseline e candidatos podem receber instantes distintos dentro da mesma decisão (`app/ponto/services/veredito_celula.py:219`, `app/ponto/services/veredito_celula.py:273`, `app/ponto/services/veredito_celula.py:281`).

- **Classe 3 — `app/ponto/services/dias_em_aberto.py:112`:** o serviço lê o relógio dentro do laço de colaboradores, em vez de receber um instante comum à apuração (`app/ponto/services/dias_em_aberto.py:78`, `app/ponto/services/dias_em_aberto.py:106`).

- **Classe 3 — `app/ponto/services/he_pendente_lavrado.py:93`:** `apurar` produz um novo instante por colaborador; sua assinatura não recebe o instante do chamador (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:91`).

- **Classe 3 — `app/ponto/management/commands/e6_oraculo.py:141`:** o leitor `InsumosDoDono.cxr` lê seu próprio relógio quando precisa carregar o espelho, sem receber o instante pela construção do objeto (`app/ponto/management/commands/e6_oraculo.py:118`).

- **Classe 3 — `app/folha/porta_export.py:318`:** `medir` captura o relógio dentro do serviço leitor; sua assinatura impede que a borda forneça o instante (`app/folha/porta_export.py:313`).

- **Classe 3 — `app/relatorios/pdf_espelho.py:1040`:** o gerador informacional captura o instante internamente. Quando chamado pelo laço do lote, cada documento recebe um instante próprio (`app/relatorios/services.py:222`, `app/relatorios/services.py:250`).

- **Classe 3 — `app/relatorios/pdf_espelho.py:1086`:** o gerador auditável também captura o instante internamente, inclusive no caminho chamado pelo lote (`app/relatorios/services.py:70`, `app/relatorios/services.py:237`).

- **Classe 3 — `app/escala/utils.py:2289`:** mesmo recebendo `agora`, o contrato lê `localdate()` sem argumento quando `hoje` é omitido. O alinhamento usa o instante recebido, mas o corte visual usa outro relógio (`app/escala/utils.py:2294`, `app/escala/utils.py:2295`).

- **Classe 3 — `app/escala/utils.py:1257`:** o builder agora exige `agora`, mas continua obtendo `ref` do relógio real quando omitida, independentemente do instante recebido (`app/escala/utils.py:1244`).

- **Classe 3 — `app/relatorios/pdf_espelho.py:593`:** o coletor recebe `agora`, mas calcula a data passada ao espelho com `datetime.date.today()`, mantendo uma segunda leitura temporal dentro do leitor (`app/relatorios/pdf_espelho.py:187`).

- **Classe 3 — `app/ponto/management/commands/detectar_par_relampago.py:289`:** `varrer_pares_embutidos` recebe `agora`, mas não o repassa ao carregador de `par_completo`; o caminho termina lendo `timezone.now()` no juiz (`app/ponto/turnos.py:2143`, `app/ponto/turnos.py:2119`). A montagem das janelas no comando também omite o instante já capturado (`app/ponto/management/commands/detectar_par_relampago.py:328`, `app/ponto/management/commands/detectar_par_relampago.py:349`).

- **Classe 5 — `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:445`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:451`:** os selos verificam apenas presença de `turno=` e existência de uma chamada ao seletor. Trocar o argumento por `turno=None` no detector preserva ambas as verificações e restaura a DP sem papel (`app/chamados/detector_anomalias.py:219`, `app/escala/utils.py:585`, `app/escala/utils.py:622`). O RED anterior ao encerramento e o MORDE com saída regular também não distinguem essa regressão (`app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:192`, `app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:218`).

Total por classe: 1=0; 2=0; 3=11; 4=0; 5=1; 6=0; 7=0.

## 29/10 15:48 · sessao 41a4d35fbf65 · - · - · origem=Ronald (tmux)

T21 — itens 3/4. Fonte: cópia `7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app`. Somente leitura estática.

**Universo do vigia versus fila**

- `app/chamados/services/vigia_fio.py:184-187`: `censo` começa em **todos os chamados VIVOS com célula**, opcional empresa; não filtra silêncio, arquivo, competência ou data.
- `app/chamados/catalogo/motor.py:24-31`: VIVOS=`aberto,em_analise,registrado`; COBRANCA=`aberto,em_analise`; ENCERRADOS=`resolvido,fechado`.
- `app/chamados/services/vigia_fio.py:199-203`: separa módulos FURO_MODULES e, por célula, marca `cobra=True` se existe chamado em COBRANCA; silêncio não participa.
- `app/chamados/services/vigia_fio.py:226-227`: só prossegue com célula em VEREDITOS_ACUSAM.
- `app/chamados/services/vigia_fio.py:242-244`: perguntas são **todas as perguntas da célula**, não só as que apontam para o chamado que trouxe a célula.
- `app/chamados/services/vigia_fio.py:153-163`; `app/chamados/juizes.py:1102-1111`: fio vivo é `any(pergunta_viva(p))`.
- `app/chamados/services/vigia_fio.py:253-285`: sai do alarme por outro vocabulário, fio vivo, hoje/futuro, decisão do dia oferecida, ausência ABERTA cobrindo, ou FOLGA_CONTESTA VALIDADA.
- `app/chamados/services/vigia_fio.py:286-289`: restante conta fio_morto; COBRANCA por status basta para fio_morto_cobrando.
- `app/chamados/services/painel.py:186-188`: `qs_na_fila` usa vivos+COBRANCA **e exclui `silenciado_ate > agora`**.

**Consequência:** chamado `aberto/em_analise`, silenciado no futuro, célula acusadora passada e sem as saídas acima pode contar **fio_morto_cobrando**, embora esteja fora de `qs_na_fila`; isso decorre diretamente das diferenças em `vigia_fio.py:184,200,288` versus `painel.py:188`. Se chamado já está resolvido/fechado, ele sozinho não traz a célula ao censo (`vigia_fio.py:184`); outro chamado VIVO na mesma célula pode trazê-la (`:187,226`).

**Pergunta respondida com destino fechado**

- `app/chamados/juizes.py:1006-1016`: validada ou trancada sai antes.
- `app/chamados/juizes.py:1020-1032`: resposta não vazia com respondida_em, sem decisão explícita do admin sobre o dia, continua viva salvo `_resposta_concorda_com_a_luz(p) is True`. **Destino fechado não é guarda desse ramo.**
- `app/chamados/juizes.py:1039-1052`: vias de identidade materializadas e marco já aceso também podem matar a pergunta nos ramos posteriores.
- `app/chamados/juizes.py:1054-1057`: célula acusadora pode manter viva pergunta não encerrada pelos degraus anteriores.
- `app/chamados/services/disputa_emissao.py:1597-1608`: resolvedor procura irmãs respondidas sem consumo, disputa ainda aberta e pai fora de VIVOS; tenta `pai.reabrir()`.
- `app/chamados/models.py:243-249`: cruzar fronteira ENCERRADOS relavra perguntas não validadas apontadas por chamado_destino; não equivale a aceitar resposta nem limpa silêncio.

**Portas de retorno à vida e limpeza do véu**

| porta | efeito / limpa silenciado_ate? |
|---|---|
| `app/chamados/models.py:1023,1087-1098` `reabrir` | Resolvido/fechado → em_analise pelo motor; ressuscita perguntas elegíveis. **Não limpa**. |
| `app/chamados/models.py:185-217` `_gravar_estado` | Limpa resolvido_em/via e restaura urgência quando vivo; lista de campos não contém silêncio. **Não limpa**. |
| `app/chamados/models.py:122-183,257-260`; `app/chamados/catalogo/motor.py:99-103,139-156` `transicionar/_aplicar` | Via humana também permite resolvido→em_analise; admin_ti tem autorização ampliada, salvo terminais absolutos. Usa mesmo núcleo; **não limpa**. |
| `app/chamados/models.py:742,763-770` `_renascer_por_premissa_viva` | Ativo, não cascade, competência não trancada, depois `reabrir`; **não limpa**. |
| `app/chamados/models.py:635,686` `abrir` | Dedup pode acionar renascimento do existente; herda preservação do véu. |
| `app/chamados/services/arquivo.py:176,193-208` `levantar` | Limpa silenciado_ate/por/motivo, opcional resgatado_em. **Não muda status**. |
| `app/chamados/views_arquivar.py:64-77` desarquivar | Se via competencia_trancada, reabre primeiro; depois **levanta véu em todos os casos**, resgatado=True. |
| `app/chamados/services/materializacao.py:655-667` resgate por resposta | Destino ou pai da disputa; se silenciado_ate preenchido, **levanta**, resgatado=True. |
| `app/chamados/services/silencio_ausencia.py:68-75,93-114` | Levanta silêncio por ausência se premissa segue viva após aprovação, ausência deixou de estar pendente/aprovada, ou após reconciliar lastro; **limpa**, resgatado=False. |

Chamadores de `reabrir` encontrados, sem `arquivo.levantar` no trecho do ato:

- `app/chamados/services/adesao.py:92`: pai.
- `app/chamados/services/acoes_disputa.py:259-266`: reabertura administrativa da disputa/pergunta e chamado encerrado.
- `app/chamados/services/competencia_trancada.py:252-265`: competência reaberta.
- `app/chamados/services/disputa_emissao.py:1597-1608`: resposta pendente em disputa aberta.
- `app/chamados/services/resposta_tardia.py:35-47`: célula acusa → `_renascer_por_premissa_viva`.
- `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.
- `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:48-53`.
- `app/ponto/management/commands/reconciliar_grade.py:513-519`: furo persistente.
- `app/ponto/management/commands/detectar_ausencias.py:300-301`: existente encerrado.

**Caminho concreto para vivo+silêncio futuro:** um chamado fechado com véu futuro, cuja célula ainda acusa e cuja reabertura passa nas guardas, recebe `reabrir` de `reconciliar_grade.py:518-519`; `_gravar_estado` preserva silêncio (`models.py:193,211-217`), mas torna em_analise (`catalogo/motor.py:113`); fica fora da fila (`painel.py:188`) e dentro do censo (`vigia_fio.py:184,200`). É possibilidade provada pelo fluxo; existência de ocorrência real `[nao verificado]`.

A reabertura não consulta quarentena nos trechos lidos de `reabrir`/renascimento (`models.py:1023-1099,763-770`); existência de quarentena adicional em chamadores não lidos integralmente `[nao verificado]`.

Busca de atribuições literais de status ativo e de `silenciado_ate` em Python de produção do app não encontrou escrita alternativa do véu: escrita encontrada em `arquivo.py:166,201`. Censo dinâmico/aliases de `.update()` e `bulk_update` não exaustivamente provado; testes/migrations não li. [nao verificado]

## 29/10 15:47 · sessao c03300972490 · - · - · origem=Ronald (tmux)

T21 item 1 — fonte `7596ef5e8351bf7473acecafc8348bca7d2cf9d7`.

**Há um escritor operacional que pode preencher `respondida_em` com resposta vazia:** `resolver_perguntas_por_chamado_resolvido`, ramo sem credor vivo, em `app/chamados/services/disputa_emissao.py:1648-1651`. Ele testa apenas carimbo nulo, grava o instante e salva; não testa nem escreve `resposta_colab`. A resposta tem default `''` em `app/chamados/models.py:1865-1866`.

Censo dos escritores de `respondida_em` em Python operacional, incluindo apagadores:

| Arquivo:linha | Função/caminho | Escreve texto na mesma operação? | Guarda/efeito |
|---|---|---|---|
| `app/chamados/services/disputa_emissao.py:1247-1266` | `responder_pergunta_disputa_core` | **Sim**, texto e instante no mesmo `save`. | Normaliza e rejeita vazia em `1219-1222`; valida tipo; desfaz validação anterior e carimbo sem lastro. |
| `app/chamados/services/disputa_emissao.py:1390-1394` | `responder_automaticamente` | **Sim**, `texto` e `instante` recebidos no mesmo `save`. | Recusa sobrescrever texto existente. Não valida conteúdo não vazio do argumento `texto` nem instante; chamador encontrado fornece constante não vazia. |
| `app/chamados/services/disputa_emissao.py:1407-1410` | `carimbar_resposta_pela_trilha` | **Não**; exige texto já existente. | Só texto não vazio e carimbo nulo; instante vem da nota do fio. |
| `app/chamados/services/materializacao.py:509-530` | `_materializar_ausencia_de_resposta` | **Não**; exige texto já existente para carimbar. | Para irmãs `hora` no mesmo dia da disputa; materializa todas, mas só preenche resposta/validação quando `_tem_resp` (`511-515`). |
| `app/chamados/services/materializacao.py:1190-1198` | `_materializar_uma` | **Não**; exige texto já existente. | Só preenche respondida se texto `.strip()` e carimbo nulo; salva com materialização e veredito. |
| `app/chamados/services/disputa_emissao.py:1570-1585` | `resolver_perguntas_por_chamado_resolvido`, ramo destino | **Não**; exige texto já existente. | `_tem_resp` e carimbo nulo; carimba também validação quando texto existe. |
| `app/chamados/services/disputa_emissao.py:1643-1651` | Mesma função, ramo disputas sem credor | **Não, nem exige texto.** | Carimba materialização/via e `respondida_em` se nulo; pode produzir par `respondida_em != NULL`, `resposta_colab=''`. |
| `app/chamados/services/disputa_emissao.py:1358-1376` | `reabrir_para_correcao` | **Sim: apaga ambos** no mesmo `update`. | Antes arquiva resposta/carimbos em `RespostaAnterior`, no mesmo atomic (`1365-1375`). Opcionalmente limpa absorção. |
| `app/chamados/models.py:1112-1116,1162-1163` | `ChamadoColaborador.ressuscitar_perguntas` | **Não escreve texto**; universo exige texto vazio/nulo. | Limpa respondida, validação, materialização e via; preserva perguntas cujo próprio marco está aceso pelo filtro `1118-1121`. |

O ramo problemático não se restringe por `tipo_resposta`: considera disputas abertas do colaborador (`disputa_emissao.py:1621-1623`), pula pai vivo (`1627-1629`) e destinos vivos (`1631-1633`), seleciona perguntas sem materialização e sem validação (`1634-1635`), pula as ainda vivas pelo juiz (`1640-1642`), depois escreve o carimbo sem verificar texto (`1648-1651`). O comentário o apresenta como tratamento de perguntas de escolha (`1609-1617`), mas o filtro efetivo não limita o tipo (`1634-1635`).

**Quem alcança as portas**

| Porta | Chamadores operacionais encontrados |
|---|---|
| `responder_pergunta_disputa_core` | Web `app/chamados/views.py:1283`; API `app/api/views.py:2695`; ação de disputa `app/chamados/services/acoes_disputa.py:367`; declaração textual `app/chamados/services/declaracao_texto.py:365,638`. |
| `responder_automaticamente` | Upload de foto, `app/api/foto_recebida.py:48`; texto constante `[auto] Foto recebida em upload separado.` em `26`; instante `now()` em `42`. Em seguida valida pela porta `validar_pergunta` em `51`. |
| `carimbar_resposta_pela_trilha` | Manutenção `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-68`; usa `nota.criado_em`, passa só com `--apply` (`63-68`); seleciona texto preenchido sem carimbo em `46`. |
| `reabrir_para_correcao` | Devolução de validação `app/chamados/services/validacao.py:540`; reversão de absorção de ausência `app/chamados/services/materializacao.py:628`; manutenção de limbo `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. |
| `_materializar_ausencia_de_resposta` | Core de resposta para escapes de ausência em `app/chamados/services/disputa_emissao.py:1291-1295`. |
| `_materializar_uma` | Validação individual `app/chamados/services/validacao.py:417`; materialização em lote `app/chamados/services/materializacao.py:1293`; tratamento de passivo `app/chamados/services/passivo_validado.py:516`. |
| `resolver_perguntas_por_chamado_resolvido` | Signal `app/chamados/signals.py:86`; reconciliador `app/chamados/reconciliador.py:469,1370`; tratamento de becos `app/chamados/services/becos.py:49`; manutenção `app/ponto/management/commands/reconciliar_fantasmas.py:280`. |

**Competência e outros encerramentos não equivalem a resposta**

- A função problemática retorna sem agir se chamado foi encerrado pela via competência trancada: `app/chamados/services/disputa_emissao.py:1535-1536`; também pula pergunta de dia trancado no ramo principal em `1550-1551`.
- A porta `PerguntaDisputa.fechar_por_competencia_trancada` grava apenas via/materialização/autor, preservando resposta e seu carimbo: `app/chamados/models.py:2143-2148`.
- A inversa de competência limpa via/materialização/autor, preservando resposta/carimbo: `app/chamados/models.py:2213-2218`; chamador `app/chamados/services/competencia_trancada.py:244-247`.
- `fechar_por_admin` grava via/materialização/autor, sem respondida/texto: `app/chamados/models.py:2131-2134`.
- `encerrar_fantasma` idem: `app/chamados/models.py:2170-2173`; inversa limpa esses campos em `2185-2188`.
- `encerrar_por_fato_no_espelho` só grava via: `app/chamados/models.py:2156-2159`.
- `lavrar_sem_turno_aberto` escreve materialização/autor/via, sem resposta: `app/chamados/reconciliador.py:402-408`.
- Absolvição pela célula idem: `app/chamados/reconciliador.py:548-552`.
- Signal de disputa fechada delega a `materializar_pendentes_aprovadas`: `app/chamados/signals.py:123-135`; a porta grava apenas materialização/autor/via quando fato está materializado, sem carimbar resposta: `app/chamados/services/materializacao.py:1335-1343`.

**Campos similares: mapa dos escritores encontrado na mesma varredura**

| Estado | Escritores, além dos já descritos |
|---|---|
| `validada_em` | `_carimbar_validacao`: `app/chamados/services/validacao.py:131-133`; devolução apaga `188-190`; core de nova resposta apaga `app/chamados/services/disputa_emissao.py:1260-1262`; materializadores/ressuscitação/reabertura já tabelados. |
| `materializada_em` e `via_resolucao` | Recusa de paridade `app/chamados/services/materializacao.py:858-868`; recusa por fato ocupado `1043-1051`; materialização unitária `1149,1177-1185`; duplicata `app/chamados/services/fio.py:844-850`. |
| Limpeza de materialização/via | `reabrir_por_resposta` em `app/chamados/models.py:2203-2208` prepara campos e o core os salva em `disputa_emissao.py:1266-1267`; `desfazer_carimbo_sem_lastro` em `app/chamados/services/relavra_via.py:204-209`. |
| Troca de via com relavra | `app/chamados/services/relavra_via.py:123-126`. |
| Outro modelo com materialização | `RegularizacaoExterna` recebe carimbo em `app/chamados/services/regularizacao_ext.py:198`; não é `PerguntaDisputa`. |
| Snapshot que menciona respondida | `app/chamados/services/passivo_validado.py:64-66,83` lista `respondida_em` para foto de reversão; não encontrei escrita/restauração por essa lista no arquivo. |

Varri `app/**/*.py` com `rg` e AST para atribuições de atributos e argumentos nomeados de `respondida_em`, `resposta_colab`, `materializada_em`, `validada_em`, `via_resolucao`, excluindo testes/migrations do censo operacional. Também busquei esses campos em `*.sql`, `*.sh`, `*.json`; apareceu referência narrativa em `app/docs/CORTES.json:1223`, sem escrita operacional encontrada. Não li integralmente testes/migrations; escrita dinâmica montada sem nomes desses campos, ORM genérico/admin e SQL externo à árvore: **[nao verificado]**. Não executei testes nem banco.

## 29/10 15:47 · sessao 0e812661bb23 · - · - · origem=Ronald (tmux)

Classe 5 — `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:445` e `:451`: os selos verificam somente a existência de `turno=` e de alguma chamada ao seletor, sem conferir que o resultado chega ao matcher. Trocar `turno=_turno_papel` por `turno=None` em `app/chamados/detector_anomalias.py:219` mantém ambos verdes e restaura a DP sem papel (`app/escala/utils.py:585`, `:614`).

A mesma regressão não é distinguida pelos testes comportamentais novos: o RED cobra `ok` antes do encerramento (`app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:190`), e o MORDE verifica o seletor separadamente e uma saída regular que a DP também casa (`:209`, `:218`). Os demais cenários também cobram respostas da DP (`:98`, `:116`, `:134`, `:158`).

Verificação realizada somente em memória: os dois selos executados sobre a mutação passaram; os 16 julgamentos do detector foram reproduzidos com `parear_turnos` e `_match_marcos` extraídos do código, sempre `turno=None`, e coincidiram com as expectativas. Não executei Django nem banco; a execução integral da suíte fica [nao verificado]. [nao verificado]

Recomendo contar como **um achado classe 5**, pois todos deixam sobreviver a mesma regressão.

## 29/10 15:36 · sessao 5f0695ecea1e · - · - · origem=Ronald (tmux)

Respostas literais às três tentativas com `touch` (cada comando retornou código 1). [nao verificado]

1. Workspace:
```text
touch: cannot touch '/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/INTRUSO_COPIA.txt': Read-only file system
```

2. saas-hasner:
```text
touch: cannot touch '/home/ronald/saas-hasner/INTRUSO_VIVA.txt': Read-only file system
```

3. /tmp:
```text
touch: cannot touch '/tmp/INTRUSO_TMP.txt': Read-only file system
```

## 29/10 15:36 · sessao 017a99c90c50 · - · - · origem=Ronald (tmux)

ok.

## 29/10 15:35 · sessao c53af7fdf9ed · - · - · origem=Ronald (tmux)

ok. Pergunta de FULANO.

## 29/10 15:32 · sessao e21096e78255 · - · - · origem=Ronald (tmux)

1. Tentativa no workspace — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/INTRUSO_COPIA.txt: Read-only file system
```

2. Tentativa em `saas-hasner` — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
```

3. Tentativa em `/tmp` — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /tmp/INTRUSO_TMP.txt: Read-only file system
```

Os três comandos terminaram com código `1`; nenhuma criação foi bem-sucedida. [nao verificado]

## 29/10 15:31 · sessao 9a5c280f2d2c · - · - · origem=Ronald (tmux)

ok.

## 29/10 15:28 · sessao 7e4719651533 · - · - · origem=Ronald (tmux)

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 1319444918a3 · - · - · origem=Ronald (tmux)

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 1258252f08e3 · - · - · origem=Ronald (tmux)

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao cdb1b9f8e444 · - · - · origem=Ronald (tmux)

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 126426b53b16 · - · - · origem=Ronald (tmux)

ok
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 3ccabb649915 · - · - · origem=Ronald (tmux)

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:25 · sessao 23341f1d3b2f · - · - · origem=Ronald (tmux)

*Numeração de `anexo_log.txt`: `# ponto.log de hoje...` é a linha 1; linhas vazias contam.*

- **Sim, o POST de atestado do app chegou em 29/10, do user 903**: recebido às **11:49:44,630** (`origem=api_app`); após parse, às **11:49:44,754**, registrou `tipo=atestado arquivo=True`. O acesso confirma `POST /api/ausencias/ → 200` às **11:49:44**. (`anexo_log.txt:2–3,25`)
- **PWA, user 1006**: uploads de atestado às **15:21:15,054** e **15:21:17,447**, `origem=pwa`; os POSTs em `/ponto/ausencias/submeter/` retornaram **302** às **15:21:15** e **15:21:17**. (`anexo_log.txt:4–5,39–40`)
- Na `api_ausencias`, **antes do parse**, `post_recebido` registra origem, user, tamanho declarado, content-type (até 60 caracteres) e teto dos campos; o primeiro acesso a `request.data` vem depois. ([app/api/views.py:1767](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1767), [1772](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1772))
- **Após parse**, `upload_ausencia` registra origem, user, tipo, presença do arquivo, tamanho, content-type e plataforma. ([app/api/views.py:1832](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1832)) Correspondência dos users com `colNN`: **[não verificado]**.

## 29/10 15:22 · sessao 95904850ee23 · - · - · origem=Ronald (tmux)

stdout [nao verificado]:
```text
rc=1
```

stderr [nao verificado]:
```text
/home/ronald/wt-codexpro-2910-1525/bin/sonda_leitura.sh: line 132: /home/ronald/wt-codexpro-2910-1525/logs/.env_leitor.novo: Read-only file system
```

Código de saída do comando solicitado: `1`. [nao verificado]

_115 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
