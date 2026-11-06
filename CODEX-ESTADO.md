# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:48** · publicado em **06/11/2026 20:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:48 · sessao aac56714e0f6 · - · - · origem=Ronald (tmux)

Censo desta raia: **33 views de escrita acessíveis por POST examinadas: 22 em chamados e 11 em escala**; mais `ir_para_ausencia`, mutação sem guarda HTTP acionada por link, separada da contagem POST (`app/chamados/views.py:1494`, `app/chamados/urls.py:44`). Contagem manual pelos nomes abaixo; ocorrência em produção [nao verificado].

**Inventário das 22:** `abrir_chamado` (147), `adicionar_followup` (274), `detalhe_chamado_local` (359), `fio_colaborador` (659), `modal_fio_colaborador` (869), `abrir_chamado_rapido` (961), `atribuir_chamado` (1008), `conferir_data_pergunta` (1292), `reabrir_disputa_supervisao` (1372), todas em `app/chamados/views.py`; `cobrar_chamado` (24), `cobrar_massa` (61), `validar_inline` (145), `validar_lote` (232), `cobrar_dia_view` (306), `validar_lote_tela` (372), em `app/chamados/views_cobrar.py`; `arquivar_chamado` (24), `desarquivar_chamado` (52), `arquivar_lote` (93), em `app/chamados/views_arquivar.py`; `regularizacao_criar` (42), `regularizacao_add_item` (74), `regularizacao_del_item` (137), `regularizacao_enviar` (154), em `app/chamados/views_regularizacao.py`. Rotas: `app/chamados/urls.py:10`, `:12`, `:14`, `:18`, `:20`, `:21`, `:25`, `:26`, `:27`, `:28`, `:31`, `:32`, `:33`, `:34`, `:37`, `:39`, `:40`, `:41`, `:49`, `:51`, `:52`, `:53`.

**Inventário das 11:** `copiar_tipo_escala` (141), `excluir_tipo_escala` (156), `escala_remove_colab` (256), `editar_folga_calendario` (316), `aplicar_folga_calendario` (376), `corroborar_regua_da_lista` (456), `realocar_regua_da_lista` (463), em `app/escala/views.py`; `wizard_salvar` (98), `wizard_calendario_criar` (140), em `app/escala/views_wizard.py`; `plano_folgas_toggle` (98), `plano_folgas_aplicar` (144), em `app/escala/views_plano_folgas.py`. Rotas: `app/escala/urls.py:9`, `:10`, `:15`, `:17`, `:20`, `:24`, `:25`, `:31`, `:32`, `:33`, `:35`, `:36`, `:37`. `copiar`/`excluir` não restringem o método, mas escrevem ao receber POST (`app/escala/views.py:141`, `:156`).

### Grupos divergentes comprovados

1. **Validar pergunta — detalhe × fio/modal × inline/lote.**
   - Detalhe: `/chamados/local/<id>/`, view `app/chamados/views.py:359`, template `chamados/detalhe_local.html` (`app/chamados/views.py:550`), cartões de pergunta `app/templates/chamados/partials/_pergunta_card.html:70`, `:95`; chama `acao_validar_pergunta` (`app/chamados/views.py:501`) → escritor canônico `validar_pergunta`, `app/chamados/services/validacao.py:343`, via `app/chamados/services/acoes_disputa.py:13`.
   - Fio `/chamados/colaborador/<id>/`, view `app/chamados/views.py:659`, template `chamados/fio_colaborador.html` (`:697`); modal `/chamados/modal/<id>/`, view `:869`, template `chamados/partials/modal_fio.html`, formulários `app/templates/chamados/partials/modal_fio.html:258`, `:274`; ambos → `processar_post_fio`, `app/chamados/services/fio.py:481`, chamadas em views `:674` e `:954`.
   - **Diferença:** detalhe filtra `disputa__fechada_em__isnull=True` (`app/chamados/views.py:493`); fio/modal aceita pergunta viva de disputa fechada, passa `sem_reabrir=True` e registra `validar_viva_em_disputa_fechada` quando o estado ficou VALIDADA/RESOLVIDA (`app/chamados/services/fio.py:591`, `:596`, `:605`, `:610`).
   - Inline `/chamados/validar-inline/<cid>/`, view `app/chamados/views_cobrar.py:145`, botão `app/templates/chamados/partials/_lista_chamados.html:82`: busca somente disputa aberta (`:160`, `:163`), valida todas as vivas respondidas (`:168`, `:208`).
   - Lotes `/chamados/validar-lote/` e `/chamados/validar-lote/tela/`, views `app/chamados/views_cobrar.py:232`, `:372`; templates `app/templates/core/_copiloto.html:16` e `app/templates/chamados/partials/_lote_preview.html:21`: convergem em `validar_lote_classe_a` (`app/chamados/services/validacao.py:648`), mas restringem à classe A (`:688`). A tela grava log agregado adicional `validou_lote_tela` (`app/chamados/views_cobrar.py:383`).
   - **Leitor/gravidade:** sim, potencialmente dia/dinheiro: validação dispara recalcular fechamento no dia do fato e materializar batidas (`app/chamados/services/validacao.py:405`, `:423`, `:430`); espelho lê o pago `DiaPago` (`app/ponto/services/espelho.py:801`). Dano real [nao verificado].

2. **Responder ao chamado / mudar status — detalhe × fio/modal.**
   - Detalhe acima → `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:283`, formulário `app/templates/chamados/detalhe_local.html:385`.
   - Fio/modal acima → ramo `app/chamados/services/fio.py:704`, formulários `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:649`.
   - **Converge nos escritores de estado** `nota.anexar_ocorrencia` (`app/chamados/services/nota.py:75`) e `ChamadoColaborador.transicionar` (`app/chamados/models.py:122`), mas detalhe registra LogAuditoria `responder` e `status` (`app/chamados/services/acoes_chamado.py:295`, `:318`); ramo fio não registra esses logs (`app/chamados/services/fio.py:704` até `:738`).
   - Detalhe aceita transição sem conteúdo no servidor (`app/chamados/services/acoes_chamado.py:313`), fio exige conteúdo para entrar no ramo (`app/chamados/services/fio.py:705`); ambos os formulários HTML exigem texto (`app/templates/chamados/detalhe_local.html:387`, `app/templates/chamados/fio_colaborador.html:261`). Detalhe oferece “Reabrir/aberto”; fio oferece apenas em análise/resolvido/fechado (`app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`).
   - **Leitor:** histórico da própria tela e auditoria (`app/chamados/views.py:372`, `app/templates/chamados/detalhe_local.html:135`); estado tem guardas contra fechar disputa/perguntas vivas e resolver furo ainda irregular (`app/chamados/models.py:142`, `:159`, `:175`).
   - **Dinheiro/dia:** diferença de trilha/opções, sem escritor financeiro direto nesses dois ramos (`app/chamados/services/fio.py:704`, `app/chamados/services/acoes_chamado.py:283`); efeito financeiro efetivo [nao verificado].

3. **Abrir chamado/mensagem DP para colaborador — formulário completo × mensagem rápida.**
   - Completo `/chamados/abrir/`, `app/chamados/views.py:147`, `chamados/form_abertura.html` (`:212`), ramo admin `:77` → `abrir_chamado_para_colaborador`, `app/chamados/services/abertura.py:122`.
   - Rápido `/chamados/colaborador/<cid>/mensagem-rapida/`, rota `app/chamados/urls.py:40`, view `app/chamados/views.py:961`, disparador `app/templates/colaboradores/partials/painel_colaborador.html:188` → `ChamadoColaborador.abrir` em `app/chamados/views.py:973`, depois nota `:986`.
   - **Diferença:** completo cria chamado com assunto/descrição/prazo/tipo/dia e autor no contexto, faz deduplicação por mesma entrada/autor, grava log e toca dois drivers (`app/chamados/services/abertura.py:145`, `:154`, `:165`, `:172`, `:188`, `:194`, `:202`). Rápido reutiliza qualquer vivo `mensagem_dp` da pessoa, assunto fixo, conteúdo como nota, push `push_colaborador`, sem log explícito nessa view (`app/chamados/views.py:973` até `:1003`).
   - **Leitor:** lista de mensagens renderizada em `app/chamados/views.py:997`, template `colaboradores/partials/msgs_lista.html` (`:1000`); fio lê followups (`app/chamados/views.py:372`).
   - **Dinheiro/dia:** não diretamente para `mensagem_dp`; ramos escrevem chamado/nota (`app/chamados/views.py:973`, `app/chamados/services/abertura.py:172`).
   - **Observação de alcançabilidade:** rota usa argumento `colaborador_id`, mas função recebe `cid` (`app/chamados/urls.py:40`, `app/chamados/views.py:961`); execução bem-sucedida desse endpoint [nao verificado].

4. **Cobrar colaborador — central/modal individual × massa × IA.**
   - Individual `/chamados/cobrar/<cid>/`, `app/chamados/views_cobrar.py:24`, botões lista e modal em `app/templates/chamados/partials/_lista_chamados.html:84`, `app/templates/chamados/partials/modal_fio.html:345`.
   - Massa `/chamados/cobrar/massa/`, `app/chamados/views_cobrar.py:61`, `app/templates/chamados/painel_gestao.html:59`, `:65`.
   - Ambos enviam FCM e usam nota, mas o massa filtra somente a fila cobrar, departamento e `muda_a_folha`, um por pessoa (`app/chamados/views_cobrar.py:76`, `:78`, `:91`, `:95`, `:97`); individual não testa VIVOS/fila/dinheiro (`:29` até `:56`). Ambos limitam por nota `[COBRANCA]` no dia (`:36`, `:117`), mas tipos de nota são `cobranca_push` × `cobranca_push_em_massa` (`:55`, `:132`).
   - IA informado pelo pai → `cobrar_colaborador`, `app/chamados/services/cobranca.py:140`: verifica perguntas pendentes e prazo existente (`:155`, `:159`), grava `prazo_sla` nos pais antes do toque (`:176`, `:178`), nota e log (`:181`, `:188`). Individual/massa não renovam prazo (`app/chamados/views_cobrar.py:24`, `:61`).
   - **Leitor:** `linhas_para_a_pauta` em `app/chamados/services/cobranca.py:251`; comentário do prazo liga à pauta após 48h em `:173`. Dinheiro/dia direto não; muda prazo/escalonamento e seleção de quem cobrar (`:170`, `:178`).

5. **Criar regularização externa — colaboradores × painel chamados.**
   - Colaboradores `app/colaboradores/views.py:2948`, template `colaboradores/partials/_form_regularizar.html` (`:2987`) → `criar_regularizacao_externa` (`:2983`) em `app/chamados/services/regularizacao_ext.py:230`.
   - Chamados `/chamados/regularizacao/colab/<id>/criar/`, `app/chamados/views_regularizacao.py:42`, template `chamados/regularizacao_painel.html` (`:34`) → `abrir_lote` (`:54`) em `app/chamados/services/regularizacao_ext.py:309` → converge em `criar_regularizacao_externa` (`:318`); itens por outro POST `app/chamados/views_regularizacao.py:74` → `adicionar_item`, `app/chamados/services/regularizacao_ext.py:322`.
   - **Diferença:** colaboradores exige categoria e itens, não exige motivo preenchido (`app/colaboradores/views.py:2961`, `:2965`, `:2981`); chamados exige motivo mas deixa categoria vazia (`app/chamados/views_regularizacao.py:48`, `:54`). Colaboradores cria itens diretamente (`app/chamados/services/regularizacao_ext.py:240`); chamados deduplica item por get_or_create (`:325`), oferece corrigir/remover/intervalo e veta E==S (`app/chamados/views_regularizacao.py:102`, `:120`, `:124`).
   - **Leitor:** métrica por `motivo_categoria` (`app/chamados/services/regularizacao_ext.py:277`, `:284`); materializador lê ação/intervalo (`:123`, `:167`).
   - **Dinheiro/dia:** sim após confirmação do colaborador, que é obrigatória (`app/chamados/services/regularizacao_ext.py:51`); adicionar gera batidas via `registrar_batida` (`:77`), corrigir altera batida existente (`:153`), remover usa `delete` (`:136`). Não tratar criação do lote como batida já gravada.

6. **Criar calendário de folgas — editor × wizard.**
   - `/escala/folgas/criar/`, `app/escala/views.py:316`, template `app/templates/escala/folga_calendario_editar.html:9` → `salvar_calendario_de_folga` em `app/escala/folgas.py:548`, chamada view `:356`.
   - `/escala/tipos/wizard/calendario/criar/`, `app/escala/views_wizard.py:140`, disparador `app/templates/escala/wizard_tipo_escala.html:160` → `criar_calendario_de_folga`, `app/escala/folgas.py:577`, chamadas `app/escala/views_wizard.py:171`, `:192`.
   - **Diferença:** escritores separados; editor ignora entradas inválidas (`app/escala/views.py:328`, `:335`, `:345`), wizard recusa com 400 (`app/escala/views_wizard.py:165`, `:167`, `:187`). Editor permite grade diferente por tamanho de mês (`app/escala/views.py:340`); wizard replica uma lista aos tamanhos 31/30/28 com clamp (`app/escala/folgas.py:590`).
   - **Leitor:** `datas_que_o_calendario_manda` em `app/escala/folgas.py:43`, foto `:150`, re-lavra `:203`.
   - **Dinheiro/dia:** sim quando aplicado: calendário decide datas de folga, foto re-lavra células (`app/escala/folgas.py:158`, `:203`, `:272`). Valor afetado em produção [nao verificado].

7. **Corroborar cadastro errado — chamado × cadastro versus realidade.**
   - Chamado `/chamados/local/<id>/`, view `app/chamados/views.py:438`, disparador modal `app/templates/chamados/partials/modal_fio.html:559` → `corroborar_regua`, `app/escala/regua_defesa.py:647`, com chamado (`app/chamados/views.py:443`).
   - Lista `/escala/cadastro-x-realidade/regua/<ec>/corroborar/`, view `app/escala/views.py:456`, formulário `app/templates/escala/cadastro_x_realidade.html:28` → mesmo escritor (`app/escala/views.py:459`).
   - **Diferença:** chamado exige RBAC contextual (`app/chamados/views.py:420`) e busca vínculo ativo por tipo do contexto (`:441`); lista usa EC por PK e apenas `_is_gestor` (`app/escala/views.py:447`, `:449`). Só chamado recebe nota de confirmação no fio (`app/escala/regua_defesa.py:670`); ambos gravam régua/trilha pela mesma porta (`:662`).
   - **Leitor:** veto de cobrança `app/escala/regua_defesa.py:641`, rótulo de espelho `:620`; nota no fio `app/chamados/views.py:372`.
   - **Dinheiro/dia:** diferença provada é escopo/permissão/nota; mesmo EC recebe mesmos campos (`app/escala/regua_defesa.py:666`). Dano financeiro por diferença [nao verificado].

8. **Realocar régua — chamado × lista.**
   - Chamado ramo `app/chamados/views.py:449`, formulário `app/templates/chamados/partials/modal_fio.html:568` → `realocar_para_destino`, `app/escala/regua_defesa.py:326`.
   - Lista rota `app/escala/urls.py:17`, view `app/escala/views.py:463`, formulário `app/templates/escala/cadastro_x_realidade.html:28` → `realocar_vinculo_para_destino`, `app/escala/regua_defesa.py:291`.
   - **Converge** nesse núcleo (`app/escala/regua_defesa.py:329`), depois `julgar_vinculo`/`executar_vinculo` (`:316`, `:321`).
   - **Diferença:** destino do chamado vem do contexto armazenado (`:328`); lista usa `ultimo_lavrado()` (`app/escala/views.py:469`). Só chamado anexa ocorrência de realocação (`app/escala/regua_defesa.py:330`). Permissões diferentes iguais ao grupo anterior (`app/chamados/views.py:420`, `app/escala/views.py:447`).
   - **Dinheiro/dia:** sim potencial quando os destinos diferem: troca vínculo a partir de hoje pelo escritor que reescreve células (`app/escala/regua_defesa.py:293`, `:312`, `:321`); destino divergente real [nao verificado].

9. **Efeito transversal fio × modal, mesmo comando.**
   - Fio → `processar_post_fio` (`app/chamados/views.py:667`, `:674`).
   - Modal antes do mesmo escritor marca notas do colaborador como lidas pelo DP (`app/chamados/views.py:877`, `:880`, `:951`, `:954`) → escritor `marcar_lidas`, `app/chamados/services/nota.py:126`, update `:132`.
   - **Leitor:** não lidos da central `app/chamados/services/painel.py:362`, `:366`, badge `app/templates/chamados/partials/_lista_chamados.html:48`.
   - **Dinheiro/dia:** não, grava leitura de mensagens (`app/chamados/services/nota.py:59`, `:132`).

### Convergências reais para o fim do censo

- **Recusar pergunta:** detalhe `app/chamados/views.py:505` → `acao_recusar_pergunta` (`:518`), fio/modal `app/chamados/services/fio.py:621`, `:634` → mesma casca `app/chamados/services/acoes_disputa.py:65` → escritor `recusar_pergunta`, `app/chamados/services/validacao.py:494`. Disparadores: `app/templates/chamados/partials/_pergunta_card.html:112`, `app/templates/chamados/partials/modal_fio.html:287`. Vale ressalva de leitura automática modal acima.
- **Corrigir resposta:** detalhe `app/chamados/views.py:523`, `:536`; fio/modal `app/chamados/services/fio.py:635` → `acao_corrigir_resposta`, `app/chamados/services/acoes_disputa.py:298`, que depois valida pelo canônico (`:375`). Disparador modal `app/templates/chamados/partials/modal_fio.html:298`; disparador detalhe [nao verificado].
- **Autorizar geofence; autorizar com advertência; recusar:** detalhe `app/chamados/views.py:540`, `:543`, modal/fio `app/chamados/services/fio.py:529`, `:541`, `:542` → mesmos `acao_autorizar_geofence` (`app/chamados/services/acoes_chamado.py:165`) e `acao_recusar_geofence` (`:243`), este → `retratar_batida` (`:269`). Disparadores detalhe `app/templates/chamados/detalhe_local.html:285`, `:286`, `:287`; modal `app/templates/chamados/partials/modal_fio.html:501`, `:512`, `:523`. Recusa muda dia/apuração (`app/chamados/services/acoes_chamado.py:269`).
- **Regularizar disputa:** detalhe `app/chamados/views.py:467` → `acao_regularizar_disputa`, `app/chamados/services/acoes_disputa.py:118`; modal/fio mantém orquestração copiada `app/chamados/services/fio.py:642`. Ambas convergem em `materializar_perguntas_validadas_da_disputa`, chamadas `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`; mesmos gates resolução e perguntas vivas sem resposta (`acoes_disputa.py:123`, `:130`; `fio.py:643`, `:656`). Disparadores `app/templates/chamados/detalhe_local.html:317`, `app/templates/chamados/partials/modal_fio.html:350`. Diferença observada: exceção de push é `pass` na casca (`acoes_disputa.py:152`, `:158`) e logger no fio (`fio.py:679`, `:686`), sem divergência de estado demonstrada.
- **Reabrir questionário:** detalhe e modal postam para o mesmo endpoint `reabrir_disputa_supervisao`, `app/chamados/views.py:1372`; disparadores `app/templates/chamados/detalhe_local.html:9`, `app/templates/chamados/partials/modal_fio.html:363`; escritor de reabertura `app/chamados/services/disputa_emissao.py:1362`, chamado via alias em `app/chamados/views.py:1435`.
- **Pedir confirmação de âncora:** detalhe `app/chamados/views.py:455` → `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:178`; fio/modal `app/chamados/services/fio.py:517` → casca `acao_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:197`. Disparador modal `app/templates/chamados/partials/modal_fio.html:639`; disparador detalhe [nao verificado].

### Candidatos que não devem virar “botão atual” sem ressalva

- `arquivar_chamado` e `arquivar_lote` convergem no escritor `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`), mas individual adiciona followup (`app/chamados/views_arquivar.py:44`) e lote não (`:140` até `:148`), com filtro idade/VIVOS/silêncio nulo no lote (`:105`, `:121`, `:135`). Nenhum disparador para arquivar individual/lote encontrado nos templates/static; tela que dispara [nao verificado]. Desarquivar tem botão comprovado (`app/templates/chamados/partials/_lista_chamados.html:85`).
- `reabrir_disputa` em `processar_post_fio` chama casca antiga que não recobra perguntas já pendentes (`app/chamados/services/acoes_disputa.py:95` até `:101`), enquanto endpoint atual recobra vivas (`app/chamados/views.py:1421`, `:1425`). Nenhum formulário atual com valor exato `reabrir_disputa` encontrado; disparador de tela [nao verificado].
- Toggle e aplicar lote do Plano têm portas diferentes e lote recusa competência passada (`app/escala/folgas.py:385`), toggle não tem esse filtro (`app/escala/views_plano_folgas.py:103` até `:137`). São endpoints da mesma tela; não contar como duas telas isoladamente (`app/escala/urls.py:8`, `:9`, `:10`). IA declarar folga é segunda tela: converge em `lancar_folga`, mas fecha também fio do dia (`app/ponto/services/declarar_dia.py:189`, `:204`), ao contrário do Plano (`app/escala/views_plano_folgas.py:131`).
- Wizard criar/editar tipo são duas rotas para a mesma view e template, usando `salvar_tipo` (`app/escala/urls.py:24`, `:25`, `app/escala/views_wizard.py:108`, `app/templates/escala/wizard_tipo_escala.html:15`); separar criação de edição de objeto evita inventar a mesma ação.

## 06/11 20:48 · sessao b826cfa5d075 · - · - · origem=Ronald (tmux)

Censo de `colaboradores`: **33 views POST administrativas**, mais **1 DELETE** (`agenda_bloco_deletar`, fora da contagem POST); `solicitar_beneficio` é autoatendimento e foi excluída; `views_fase.py` só renderiza. Referências: `app/colaboradores/urls.py:9`, `app/colaboradores/urls.py:89`, `app/colaboradores/views.py:1965`, `app/colaboradores/views.py:2261`, `app/colaboradores/views.py:2315`, `app/colaboradores/views.py:2340`, `app/colaboradores/views_fase.py:36`, `app/colaboradores/views_fase.py:118`.

**Achados divergentes comprovados**

1. **Mudar posto — grupo de três caminhos.**

   - `/colaboradores/<cid>/vincular/posto/` → `vincular_posto`, `app/colaboradores/views.py:311`; formulário `app/templates/colaboradores/partials/painel_vinculo.html:112`; escreve somente `posto` em `app/colaboradores/views.py:325` → `salvar_inteiro`, `app/colaboradores/services/colaborador.py:73`; histórico por `registrar_mudanca`, `app/colaboradores/views.py:329`.
   - `/colaboradores/postos/<pid>/colabs/add/` → `posto_add_colab`, `app/colaboradores/views.py:631`; acionador `app/templates/colaboradores/postos.html:97`, drawer `app/templates/colaboradores/partials/painel_posto.html:45`; escreve somente `posto` por `gravar`, `app/colaboradores/views.py:645` → `app/colaboradores/services/colaborador.py:55`; histórico de evento, `app/colaboradores/views.py:647`.
   - `/colaboradores/<cid>/transferir/` → `transferir_posto`, `app/colaboradores/views.py:1366`; formulário `app/templates/colaboradores/editar.html:215`; escreve **posto e empresa**, `app/colaboradores/views.py:1387`; usa `gate_disputa`, `app/colaboradores/views.py:1365`; demais caminhos não possuem esse decorator, `app/colaboradores/views.py:310`, `app/colaboradores/views.py:630`.
   - Convergem no módulo de escrita do colaborador, mas não no comando nem nos campos. `registrar_mudanca` cria campos `tipo/de/para`; `registrar_evento_de_vinculo` cria evento sem esses campos: `app/colaboradores/services/historico_vinculo.py:7`, `app/colaboradores/services/historico_vinculo.py:20`.
   - **Dinheiro/dia: sim, potencial por janela e empresa de exportação:** `classificar_export` filtra empresa do colaborador, `app/folha/export.py:778`; janela usa `colab.empresa`, `app/folha/export.py:337`. Ocorrência concreta em produção [nao verificado].

2. **Desativar/reativar posto — editar cadastro versus botão.**

   - `/colaboradores/postos/<pid>/editar/` → `form_posto`, `app/colaboradores/views.py:714`; checkbox `app/templates/colaboradores/posto_form.html:119`; passa `ativo` em `app/colaboradores/views.py:846` → `atualizar_posto`, `app/colaboradores/services/estrutura.py:64`.
   - `/colaboradores/postos/<pid>/toggle/` → `toggle_posto`, `app/colaboradores/views.py:876`; botão `app/templates/colaboradores/partials/painel_posto.html:67` → `gravar_posto_ativo`, `app/colaboradores/services/estrutura.py:73`.
   - Só o botão veta desativação com colaboradores ativos, `app/colaboradores/views.py:883`. O escritor de edição atribui dados e salva sem esse veto, `app/colaboradores/services/estrutura.py:66`.
   - Leitor: listas de postos disponíveis filtram `ativo=True`, `app/colaboradores/views.py:269`, `app/colaboradores/views.py:1242`. **Dinheiro/dia: não demonstrado diretamente;** esses escritores alteram cadastro do posto, `app/colaboradores/services/estrutura.py:64`, `app/colaboradores/services/estrutura.py:73`; efeito financeiro [nao verificado].

3. **Lançar benefício — individual versus lote; convergência de escritor com validações distintas.**

   - `/colaboradores/beneficios/novo/` → `lancar_beneficio`, `app/colaboradores/views.py:1767`, template `app/templates/colaboradores/beneficio_form.html:12`.
   - `/colaboradores/beneficios/lote/` → `lancar_beneficio_lote`, `app/colaboradores/views.py:1818`, template `app/templates/colaboradores/beneficio_lote.html:17`.
   - Ambos → `criar_beneficio`, `app/colaboradores/services/beneficios.py:9`. Só lote ignora benefício ativo já existente por pessoa/tipo/competência, `app/colaboradores/views.py:1839`; só individual exige campos no servidor, `app/colaboradores/views.py:1779`; competência individual usa `int`, `app/colaboradores/views.py:1798`, lote usa sanitização, `app/colaboradores/views.py:1825`.
   - O decorator individual busca `colaborador_id`, `app/colaboradores/views.py:1766`, mas template e view usam `colaborador`, `app/templates/colaboradores/beneficio_form.html:17`, `app/colaboradores/views.py:1772`; portanto o gate recebe `None` no formulário normal e passa, por `app/colaboradores/mixins.py:305`.
   - Leitores somam valores ativos, `app/colaboradores/views.py:1701`, e exportam cada lançamento, `app/colaboradores/views.py:1897`. **Dinheiro: sim no cadastro/exportação de benefícios; integração desse CSV na folha [nao verificado].**
   - Terceiro produtor do mesmo estado: aprovação `novo_vt`/`novo_va` cria benefício **R$0**, competência atual, descrição fixa e conclui solicitação/chamado, `app/colaboradores/views.py:2066`, `app/colaboradores/views.py:2072`, `app/colaboradores/views.py:2086`; tela `app/templates/colaboradores/painel_solicitacao_beneficio.html:15`.

4. **Regularização externa — divergente, também informado pelo agente de chamados.**

   - Drawer `/colaboradores/colab/<cid>/regularizar/` → `colab_regularizar`, `app/colaboradores/views.py:2948`; template `app/templates/colaboradores/partials/_form_regularizar.html:2`; exige categoria, permite motivo vazio, exige itens, `app/colaboradores/views.py:2961`, `app/colaboradores/views.py:2965`, `app/colaboradores/views.py:2981`; chama `criar_regularizacao_externa`, `app/colaboradores/views.py:2983`.
   - Chamados → `regularizacao_criar`, `app/chamados/views_regularizacao.py:42`; exige motivo, cria lote sem itens/categoria via `abrir_lote`, `app/chamados/views_regularizacao.py:48`, `app/chamados/views_regularizacao.py:54`.
   - Convergem em `app/chamados/services/regularizacao_ext.py:230`, mas drawer cria itens simples, `app/chamados/services/regularizacao_ext.py:240`, enquanto editor usa outra porta e validações; agente de chamados possui os detalhes.

5. **Redefinir senha — ficha administrativa versus cadastro do colaborador.**

   - `/configuracoes/usuarios-ad/ficha/<uid>/acao/` → `ficha_usuario_acao`, `app/core/views_usuarios.py:324`, template `app/templates/core/config/_ficha_usuario.html:25` → `resetar_senha_de_sistema`, `app/core/credenciais.py:185`: grava senha e `last_login=None`, `app/core/credenciais.py:187`.
   - Colaborador reset CPF → `app/colaboradores/views.py:1474` → `resetar_para_cpf`, `app/core/credenciais.py:47`; senha digitada → `app/colaboradores/views.py:1556` → `definir_senha_temporaria`, `app/core/credenciais.py:67`. Ambos marcam `deve_trocar_senha=True`, `app/core/credenciais.py:57`, `app/core/credenciais.py:73`.
   - **Leitor decisivo:** `precisa_trocar_senha` ignora `last_login` quando existe colaborador e lê sua flag, `app/colaboradores/mixins.py:282`. Inferência: staff com colaborador e flag falsa não ganha obrigação de troca pelo reset da ficha de sistema; a ficha permite qualquer `User(is_staff=True)`, `app/core/views_usuarios.py:331`, e perfis administrativos de colaborador tornam o usuário staff, `app/core/credenciais.py:132`. Usuários concretos nessa interseção [nao verificado].
   - Todos auditados pelo mesmo `_trilha`, `app/core/credenciais.py:40`. **Dinheiro/dia: não diretamente**, escritas são senha/flags, `app/core/credenciais.py:54`, `app/core/credenciais.py:187`.

6. **Bloquear/desbloquear acesso — ficha administrativa versus gestão de colaboradores.**

   - Ficha sistema → `app/core/views_usuarios.py:347` → `alternar_ativo_de_sistema`, `app/core/credenciais.py:196`.
   - Gestão colaborador → `toggle_acesso`, `app/colaboradores/views.py:1583`; template `app/templates/colaboradores/gestao_usuarios.html:67` → `alternar_acesso`, `app/core/credenciais.py:84`.
   - Ambos gravam `User.is_active` e auditam, `app/core/credenciais.py:89`, `app/core/credenciais.py:200`; só sistema veta autodesativação, `app/core/views_usuarios.py:348`.
   - Desligamento e reativação são comandos mais amplos que também escrevem acesso absoluto por `definir_acesso`, `app/colaboradores/services/desligamento.py:111`, `app/colaboradores/services/desligamento.py:216`; desligamento altera situação, fecha vínculos/disputas/chamados e abre auditoria, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:122`, `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:154`, `app/colaboradores/services/desligamento.py:163`. Não equiparar desligar a bloquear acesso.

**Convergentes comprovados para o fim do censo**

- **Reset CPF em duas telas:** `app/templates/colaboradores/editar.html:230` e `app/templates/colaboradores/gestao_usuarios.html:60` → mesma rota `app/colaboradores/urls.py:58` → mesma view `app/colaboradores/views.py:1474` → `app/core/credenciais.py:47`.
- **Reativar colaborador em duas telas:** `app/templates/colaboradores/editar.html:287` e `app/templates/colaboradores/partials/painel_colaborador.html:162` → mesma rota `app/colaboradores/urls.py:53` → `app/colaboradores/views.py:1408` → `app/colaboradores/services/desligamento.py:203`.
- **Trocar escala via vínculo e IA:** vínculo → `app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`; IA → `app/colaboradores/services/troca_pelo_chat.py:64`; ambos julgam em `app/colaboradores/services/vinculo.py:50` e executam em `app/colaboradores/services/vinculo.py:167`. IA chama novamente juiz antes de gravar, `app/colaboradores/services/troca_pelo_chat.py:72`.

**Candidato com segundo acionador não localizado**

- Calendário individual → `app/colaboradores/views.py:239`, fotos `app/colaboradores/views.py:245`, histórico `app/colaboradores/views.py:241`; lote por tipo → `app/escala/views.py:409`, fotos `app/escala/views.py:411`, relavra adicional `app/escala/views.py:416`. **Ambos já relavram dentro de `gerar_foto_folgas`**, `app/escala/folgas.py:203`; não declarar ausência de relavra individual. Rota do lote existe, `app/escala/urls.py:37`; template/acionador do lote [nao verificado]. A diferença comprovada é histórico individual, atomicidade externa do lote e relavra adicional do lote, `app/colaboradores/views.py:241`, `app/escala/views.py:407`, `app/escala/views.py:416`.

## 06/11 20:48 · sessao 9b3bd19952a6 · - · - · origem=Ronald (tmux)

Recorte **ponto/férias/folha**: **52 views admin com POST de escrita persistente**: ponto33, férias14, folha5. Excluídos `previa_he_periodo` (rollback obrigatório), fluxos exclusivos do colaborador e `importar_aej` (410). Referências: `app/ponto/urls.py:19`, `app/ponto/views.py:434`, `app/ponto/services/autorizacao_he_periodo.py:320`, `app/ponto/views.py:57`, `app/ferias/urls.py:9`, `app/folha/urls.py:8`. Minha classificação operacional dá **42 ações**, agrupando individual/lote; trata “resolver dia” como uma ação composta, portanto ajustar se o censo global separar seus vereditos. `[nao verificado]`

**Achados divergentes principais**

1. **Aprovar justificativa**  
   A: `/ponto/justificativa/<jid>/aprovar/` → `app/ponto/views.py:1911` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`; form `app/templates/ponto/partials/painel_justificativa.html:51`.  
   B: `/ponto/justificativa/aprovar-lote/` → `app/ponto/views.py:1763` → escrita direta `j.status/save`, `app/ponto/views.py:1796`; form montado em `app/templates/ponto/fila_justificativas.html:176`. Rotas: `app/ponto/urls.py:40`.  
   Difere: individual recusa saída ≤ entrada (`services/justificativa.py:137`), só cria saída se tem entrada (`:137`), sai sem aprovar se já existe batida gerada (`:169`); lote cria saída informada sem exigir `batida_entrada` (`views.py:1808`). Individual encerra bloqueio de entrada (`services/justificativa.py:203`), deixa `LogAuditoria` (`:215`), follow-up (`:219`) e notificações (`:225`); lote encerra somente origem explicitamente (`views.py:1837`) e termina em `:1856`, sem esses passos.  
   Leitor financeiro: `Justificativa(status='aprovada')` compõe `datas_justificadas` no fechamento, `app/ponto/services/fechamento.py:651`. **Muda dinheiro/dia: sim, potencial**, pois aceita/aprova/materializa condições diferentes e a aprovação abona dia no fechamento. Incidência em produção `[nao verificado]`.

2. **Rejeitar justificativa**  
   A `/ponto/justificativa/<jid>/rejeitar/` → view `app/ponto/views.py:1923` → `rejeitar_justificativa_dp`, `app/ponto/services/justificativa.py:247`; template `app/templates/ponto/partials/painel_justificativa.html:58`.  
   B `/ponto/justificativa/rejeitar-lote/` → view/escritor direto `app/ponto/views.py:1859`, escrita `:1875`; template `app/templates/ponto/fila_justificativas.html:188`; rotas `app/ponto/urls.py:42`.  
   Individual usa atomic + lock (`services/justificativa.py:258`), repete como “já processada” (`:265`), cria chamado `justificativa_rejeitada` (`:276`) e follow-up (`:309`). Lote não cria esse chamado nem follow-up; faz get/save sem lock e push (`views.py:1874`). **Dinheiro/dia: não demonstrado para a mesma rejeição**; chamado/corrida/trilha diferem, consequência monetária concreta `[nao verificado]`.

3. **Aprovar fechamento — grupo de três entradas**  
   A `/ponto/fechamento/aprovar/` → `aprovar_fechamento`, `app/ponto/views.py:1257` → `aprovar_lote`, `app/ponto/services/fechamento.py:1307`; template `app/templates/ponto/fechamento.html:209`.  
   B `/ponto/fechamento/aprovar-colaborador/` → `aprovar_colaborador`, `app/ponto/views.py:1461` → `aprovar`, `app/ponto/services/fechamento.py:1319`; template `app/templates/ponto/partials/painel_fechamento.html:86`.  
   C `/folha/exportar/gerar/` → `gerar_dominio_txt`, `app/folha/views.py:102` → mesmo `aprovar_lote` (`:252`); template `app/templates/folha/exportar.html` (linha do form não coletada `[nao verificado]`). Rotas `app/ponto/urls.py:32`, `app/folha/urls.py:14`.  
   A classifica **objetos gravados** (`ponto/views.py:1290`); B classifica `fechamento_lido` (`:1480`, `:1506`); C recalcula em somente leitura (`folha/views.py:207`) e classifica esse resultado (`:212`). A pode dispensar `conta_vazia` só quando selecionado (`ponto/views.py:1304`), B sempre é selecionado (`:1509`), C aplica `TRAVAS_FORA_DO_CARIMBO_DO_TXT` (`folha/views.py:243`). A grava observação (`ponto/views.py:1363`) e tranca período quando sem pulados (`:1366`); B não; C não passa observação e registra Log de aprovação por geração (`folha/views.py:254`) e exportação (`:289`). Escritores A/C fazem `update` (`services/fechamento.py:1316`); B faz save integral (`:1324`).  
   Leitor: o próprio TXT usa `_fechs_lido` para as linhas (`folha/views.py:268`); **muda dinheiro/dia: sim, potencial**, pois as portas podem autorizar pessoas segundo fotos diferentes e C produz rubricas no arquivo. Valor/prod `[nao verificado]`.

4. **Reabrir fechamento**  
   A `/ponto/fechamento/reabrir-colaborador/` → `app/ponto/views.py:1434` → `reabrir`, `app/ponto/services/fechamento.py:1333`; B `/ponto/fechamento/reavaliar-vazios/` → `app/ponto/views.py:1383` → `reabrir_lote`, `app/ponto/services/fechamento.py:1328`; rotas `app/ponto/urls.py:34`.  
   A aceita qualquer aprovado (`views.py:1447`); B só aprovados cujo `classificar_export` gravado retorna `conta_vazia` (`:1409`). Mesmos três campos, escritores save vs update (`services/fechamento.py:1330`, `:1338`), logs distintos (`views.py:1424`, `:1453`). **Dinheiro/dia: não diretamente**; libera novo cálculo, que pode mudar valor `[nao verificado]`.

5. **Cancelar férias — DP versus admin versus massa/concluída**  
   A `/ferias/agendamento/<ag_id>/cancelar/` → `app/ferias/views.py:321` → `cancelar_agendamento_dp`, `app/ferias/services.py:330`; templates `app/templates/ferias/saldo.html:71`, `app/templates/ferias/_secao_colab.html:160`.  
   B `/ferias/agendamento/<ag_id>/cancelar-admin/` → `views.py:341` → `executar_cancelamento_admin`, `services.py:341`; template `_secao_colab.html:162`.  
   C `/ferias/cancelar-admin-massa/` → `views.py:519` → mesmo escritor (`:559`); template `ferias/cancelar_admin_massa.html`, render em `views.py:565`.  
   D `/ferias/agendamento/<ag_id>/corrigir-concluida/` → `views.py:378` → mesmo escritor (`:407`), exige justificativa ≥5 (`:399`). Rotas `app/ferias/urls.py:9`, `:18`.  
   A somente barra `concluido` (`views.py:325`), **não barra já cancelado**; escritor subtrai novamente `dias_concedidos` sem atomic (`services.py:333`). B/C barram via `pode_cancelar_admin`, só agendado/em_gozo (`models.py:335`), e usam atomic (`services.py:352`), rejeitam ausências explicitamente (`:356`), carimbam `obs` (`:360`). B deixa `LogAuditoria` (`views.py:368`), C não chama esse log entre execução e resposta (`:551–565`). D aceita concluído e registra justificativa no Log (`:410`).  
   **Não alegar ausência órfã só no DP:** ambos salvam AgendamentoFerias, cujo save sincroniza Ausencia (`app/ferias/models.py:434`, `:379`).  
   Leitor do saldo: `saldo_para_remarcar` usado por edição (`views.py:661`). **Muda dinheiro/dia: sim**, saldo pode ser devolvido novamente pelo POST de A já cancelado; incidência `[nao verificado]`.

6. **Agendar férias pela solicitação versus agendar individual/lote/chat**  
   Individual `/ferias/periodo/<periodo_id>/agendar/` → `app/ferias/views.py:269` → `_executar_agendamento` (`:298`) → `executar_agendamento`, `app/ferias/services.py:306`; template `app/templates/ferias/agendar.html`, render `views.py:312`, e `_secao_colab.html:60`.  
   Lote `/ferias/agendar-lote/` → view `:569` → mesmo escritor (`:617`), template render `:626`.  
   Solicitação `/ferias/solicitacoes/<sid>/avaliar/` → `views.py:711` → `aprovar_solicitacao`, `services.py:410`, **outro escritor**, `AgendamentoFerias(...).full_clean/save` (`:434–447`); template `ferias/solicitacoes.html`, render `views.py:705`.  
   Solicitação é marcada aprovada **antes** de tentar agendar (`services.py:416`); erro no agendamento vira aviso (`:448`). Seleciona primeiro PA adquirido (`:424`); individual recebe PA e usa juiz `_validar_agendamento` (`views.py:288`) e escritor com guarda de duplicata (`services.py:316`). Chat `agendar_ferias_porta` (`services.py:128`) **converge em executar_agendamento** (`:157`), mas pode vender dias no mesmo atomic (`:161`).  
   **Dinheiro/dia: sim, potencial**, solicitação aprovada pode não gerar gozo e escolha/validação/escritor diferem; Ausencia materializada sai do model save (`models.py:434`), leitor de gozo `ferias_na_janela`, `services.py:47`. Produção `[nao verificado]`.

**Convergências com diferenças nas cascas**

7. **Aprovar ausência individual/lote** converge em `app/ponto/services/ausencia.py:897` via `_a4_decidir`, `app/ponto/views.py:82`.  
   A `/ponto/ausencias/<aid>/aprovar/` → `views.py:2599`, template `app/templates/ponto/partials/painel_ausencia.html:309`; B `/ponto/ausencias/lote/aprovar/` → `views.py:2270`, template `app/templates/ponto/ausencias.html:308`; rotas `app/ponto/urls.py:54`, `:65`.  
   A propaga irmãos (`views.py:2615`), pode confirmar sem lastro (`:2608`) e gravar horas parciais (`:2630`, `:2644`), usa atomic para todos irmãos (`:2641`) e push (`:2646`). B só ids, gate sem confirmação (`:2284`), sem período, sem push (`:2282–2300`). O escritor só aplica `aprovada_em` se recebido (`services/ausencia.py:916`); A manda, B não. Leitor financeiro/grade: período parcial na autoridade (`services/ausencia.py:935`); **dinheiro/dia: sim, potencial**, cobertura do conjunto e período diferem. Incidência `[nao verificado]`.

8. **Rejeitar ausência individual/lote** converge no mesmo `decidir`.  
   A `/ponto/ausencias/<aid>/rejeitar/` → `views.py:2680`, template painel `:330`; B lote → `views.py:2304`, template `ausencias.html:321`. A propaga irmãos (`views.py:2688`), manda `aprovada_em` (`:2702`), push (`:2704`); B só ids e acrescenta motivo em descrição (`:2319`). **Dinheiro/dia: sim, potencial pelo conjunto de dias rejeitados**, não por escritor duplicado (`services/ausencia.py:897`).

9. **Cancelar ausência individual/lote** converge em `decidir` e ambos deixam Log por registro. A `views.py:2720` → `:2734`; B `views.py:2333` → `:2351`. Templates painel `[nao verificado]` / `ausencias.html:278`; rotas `urls.py:56`, `:75`. Mesmos campos/carimbos/status, muda seleção/envelope/log descritivo (`views.py:2357`, `:2739`). **Dinheiro/dia: não identifiquei divergência para mesmo registro**.

10. **Editar datas férias normal/admin** converge em `_trocar_janela`, `app/ferias/services.py:366`, via wrappers `:398`, `:403`.  
    Normal `/ferias/agendamento/<ag_id>/editar/` → `views.py:633`, form `_secao_colab.html:130`; admin `/editar-datas-admin/` → `views.py:421`, form `:146`; rotas `ferias/urls.py:21`. Normal só `status='agendado'` (`views.py:649`; não verifica `pode_editar` nessa view); admin qualquer não cancelado (`:452`) e motivo ≥5 (`:455`), Log só admin (`:497`). Mesma limpeza janela antiga e Ausencia (`services.py:387`), saldo/data/status (`:378`). **Dinheiro/dia: sim por alcance de status permitido**, para o mesmo agendado com mesmos parâmetros converge.

11. **Autorizar HE dia/período** converge em `decidir_he`, `app/ponto/portas/he.py:138` → `_gravar`, `:44`.  
    Dia `/ponto/gestao-he/decidir/` → `views.py:496`, chamada `:529`; form `app/templates/ponto/gestao_he.html:256`; também calendário `[nao verificado]`. Período `/ponto/gestao-he/periodo/autorizar/` → `views.py:725` → `autorizacao_he_periodo.confirmar`, `:751` → `_ato` `services/autorizacao_he_periodo.py:249` → `autorizar_em_lote` `portas/he.py:480` → mesmo `decidir_he`, `:562`; form `gestao_he_periodo_previa.html:79`.  
    Período só dias acima limite (`views.py:665`), não sobrescreve outra decisão (`portas/he.py:557`), exige DIFF igual (`autorizacao_he_periodo.py:343`) e cria Pauta DP se exportado (`:299`). Dia pode trocar decisão, sem essa pauta explícita (`portas/he.py:174`). Leitor dinheiro: porta relavra pelo evento (`portas/he.py:185`), **dinheiro/dia: sim, potencial pela aceitação/recusa diferente e HE autorizada**.

12. **Recusar HE dia/lote** converge em `decidir_he`; lote `app/ponto/views.py:546` → `recusar_em_lote` (`:614`) → `app/ponto/portas/he.py:237`. Forms `app/templates/ponto/gestao_he.html:68`, `:139`. Escrita/trilha iguais (`portas/he.py:181`); lote permite erros por item (`views.py:619`). **Dinheiro/dia: não por recusa inédita**, recusa é padrão; troca de autorizado é outra condição (`portas/he.py:12`).

13. **Lançar ausência / resolver dia justificado ou faltou / chat** converge em `criar_ausencia`, `app/ponto/services/ausencia.py:130`.  
    A `/ponto/ausencias/lancar/` → `views.py:2368` → `criar_ausencia` (`:2415`); template render `:2460`.  
    B `/ponto/celula/veredito/` → `views.py:3312` → `veredito_celula`, `services/veredito_celula.py:191` → criar (`:315`); forms `app/templates/ponto/partials/_veredito_dia.html:17`, `_veredito_lote_barra.html:2`.  
    C chat → `app/ponto/services/checar_ausencia.py:43` → criar (`:46`).  
    B fecha fio (`veredito_celula.py:325`), lavra célula admin (`:330`) e Log (`:334`); A permite minutos/documento/indeterminado/HE100 (`views.py:2417`) e escreve situação afastado para INSS (`:2433`), C não recebe esses campos e proíbe INSS/férias/falta (`checar_ausencia.py:10`). Criar já deixa trilha própria (`services/ausencia.py:440`), portanto não dizer A/C “sem Log”. **Dinheiro/dia: sim, potencial**, parâmetros e lavra diferentes.

14. **Mesmo endpoint em templates distintos**: férias agendar (`agendar.html` render `views.py:312` / `_secao_colab.html:60`), férias cancelar (`saldo.html:71` / `_secao_colab.html:160`); flip batida fila (`app/templates/ponto/flip_fila.html:16`) / calendário (`app/templates/colaboradores/partials/_calendario_grade.html:121`) → `app/ponto/views.py:3260` → `flip_tipo` (ver chamada `:3267`). Veredito dia/lote → mesmo endpoint `views.py:3312`, mesmo loop `:3339`. Calendário mês inclui grade em `calendario_partial.html:62`; drawer diagnóstico também (`_drawer_diagnostico.html:35`). **Convergências, sem divergência adicional demonstrada para mesmo payload**.

**Lista das 52 views para consolidação**

Ponto33: `batidas_offline_pendentes:434`, `decidir_he:496`, `recusar_he_em_lote:546`, `autorizar_he_periodo:725`, `recalcular_fechamento:1226`, `aprovar_fechamento:1257`, `reavaliar_vazios:1383`, `reabrir_colaborador:1434`, `aprovar_colaborador:1461`, `reabrir_periodo:1520`, `aprovar_lote_justificativas:1763`, `rejeitar_lote_justificativas:1859`, `aprovar_justificativa:1911`, `rejeitar_justificativa:1923`, `anexar_documento_ausencia:2133`, `aprovar_ausencia_lote:2270`, `rejeitar_ausencia_lote:2304`, `cancelar_ausencia_lote:2333`, `lancar_ausencia:2368`, `reverter_rejeicao_ausencia:2569`, `aprovar_ausencia:2599`, `editar_periodo_ausencia:2662`, `rejeitar_ausencia:2680`, `cancelar_ausencia:2720`, `editar_ausencia:2751`, `criar_cobertura:3037`, `materializar_saida_retroativa:3106`, `flip_batida_view:3260`, `retratar_batida_view:3274`, `desfazer_retratacao_view:3286`, `veredito_celula_view:3312` (todas `app/ponto/views.py`); `app/ponto/views_tipos_ausencia.py:57`; `app/ponto/views_motivos_retratacao.py:48`.

Férias14: `criar_periodo_aquisitivo:123`, `agendar_ferias:269`, `cancelar_agendamento:321`, `cancelar_agendamento_admin:341`, `corrigir_concluida_admin:378`, `editar_datas_admin:421`, `cancelar_admin_massa:519`, `agendar_lote:569`, `editar_agendamento:633`, `avaliar_solicitacao:711`, `editar_periodo:777`, `excluir_periodo:808`, `agendamento_enviar:830`, `vender_dias:906`, todas `app/ferias/views.py`.

Folha5: `lista_rubricas:27`, `gerar_dominio_txt:102`, `nova_integracao:466`, `editar_integracao:487`, `excluir_integracao:501`, todas `app/folha/views.py`.

## 06/11 20:47 · sessao dc2d4fc85da7 · DUAS-TELAS-E-PWA-OFFLINE · leitura · origem=Code

**O PWA tem fila offline real em IndexedDB. O “zero atrasadas” informado não prova ausência de fila nem perda:** o envio online e o reenvio da fila carregam campos diferentes. (`app/static/pwa.js:107`; `app/static/pwa.js:147`; `app/templates/ponto/bater_ponto_v2.html:330`)

1. **Sem rede: o que acontece**

   O dashboard padrão aponta para `/ponto/bater-v2/`; essa rota renderiza `bater_ponto_v2.html`, que herda `base.html`, carregador de `pwa.js`. (`app/core/views.py:270`; `app/templates/core/dashboard_colaborador_v2.html:31`; `app/ponto/urls.py:12`; `app/ponto/views.py:356`; `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/base.html:20`)

   Na v2, com a tela disponível, o clique desabilita o botão e exige **selfie e GPS antes de verificar a rede**. Sem esses requisitos, mostra aviso, reabilita o botão e não grava. Com eles e `navigator.onLine=false`, chama a gravação offline; concluída a gravação, mostra “salva no celular” e esconde o botão. Se a gravação falhar, mostra “Erro ao registrar” e reabilita o botão. (`app/templates/ponto/bater_ponto_v2.html:285`; `app/templates/ponto/bater_ponto_v2.html:320`; `app/templates/ponto/bater_ponto_v2.html:367`)

   A gravação usa **IndexedDB**, banco `juliani-offline`, versão 2, store `batidas_pendentes`, com `id` autoincremental. Guarda tipo, horário corrigido pelo gap, `timestamp_dispositivo`, gap, GPS e foto; só confirma depois de completar a transação. A fila de batidas não está em localStorage nem no Cache Storage. ([app/static/pwa.js:48](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/pwa.js:48); `app/static/pwa.js:107`)

   O **reenvio é executado pela página**, no evento `online`, na inicialização quando está online ou ao receber `SYNC_BATIDAS` do SW. Exige a meta de colaborador, ordena as pendências e faz POST multipart para `/api/ponto/bater/`. Sucesso com JSON legível remove a pendência; **HTTP 400 também remove**, com aviso apenas no console; outras respostas e exceções conservam o item para outra sincronização. (`app/static/pwa.js:15`; `app/static/pwa.js:126`; `app/static/pwa.js:158`; `app/static/pwa.js:320`)

   O handler Background Sync não envia sozinho: manda mensagem para janelas existentes ou, sem janela, notifica para abrir o app. A gravação da fila não registra a tag `sync-batidas`; portanto, esse handler não comprova sincronização autônoma com o app fechado. (`app/static/sw.js:112`; `app/static/pwa.js:107`; `app/static/pwa.js:323`)

   Há outra rota ativa, `/ponto/bater/`, que normalmente serve `triagem/apto.html`: offline, abre câmera e depois grava na mesma fila; câmera indisponível esconde a confirmação. Não confundir esse caminho com o arquivo `bater_ponto.html`, que não é escolhido pela view atual. (`app/ponto/urls.py:11`; `app/ponto/views.py:225`; `app/ponto/views.py:356`; `app/templates/ponto/triagem/apto.html:236`; `app/templates/ponto/triagem/apto.html:253`; `app/templates/ponto/triagem/apto.html:289`)

   **Abrir a tela sem rede é uma questão separada:** o SW pré-cacheia JS, manifest e ícones; seu ramo de navegação consulta cache, mas não grava HTML nele. Tela disponível desde uma sessão anterior não equivale a abertura offline garantida. (`app/static/sw.js:3`; `app/static/sw.js:45`)

2. **Horários gravados e comparação com APK**

   | Caminho | Instante enviado e gravado |
   |---|---|
   | **PWA v2 online** | O formulário não envia `timestamp_dispositivo`; a API usa `agora` como `timestamp` e deixa o horário do dispositivo nulo. (`app/templates/ponto/bater_ponto_v2.html:102`; `app/templates/ponto/bater_ponto_v2.html:330`; `app/api/hora_do_aparelho.py:135`) |
   | **PWA pela fila** | Envia `timestamp_dispositivo` ISO. O `timestamp` corrigido e o gap guardados localmente **não são enviados** nesse multipart. (`app/static/pwa.js:113`; `app/static/pwa.js:143`) |
   | **PWA pelo POST HTML `/ponto/bater/`** | O formulário não envia horário do dispositivo; o servidor grava `timezone.now()`. (`app/templates/ponto/triagem/apto.html:86`; `app/ponto/services/triagem_batida.py:625`; `app/ponto/services/triagem_batida.py:708`) |
   | **APK na API** | A porta classifica como APK quando recebe `device_id` não vazio e aplica a mesma função de leitura de horário, que aceita epoch em segundos/milissegundos e ISO. Comportamento interno da fila do cliente APK: **[nao verificado]**. (`app/ponto/registro_batida.py:53`; `app/api/hora_do_aparelho.py:72`; `app/api/hora_do_aparelho.py:98`) |

   Na API, horário legível entre **agora e 48 horas atrás** vira `timestamp`; ausente, ilegível, futuro ou anterior à janela faz usar `agora`. Um horário legível descartado continua sendo gravado em `timestamp_dispositivo`. A API entrega ambos à porta única, que cria a `Batida`. ([app/api/hora_do_aparelho.py:124](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/api/hora_do_aparelho.py:124); `app/api/views_core.py:680`; `app/ponto/registro_batida.py:157`)

   **Escritores e leitores relevantes:**

   | Campo | Escrita e leitura |
   |---|---|
   | `timestamp` | API escolhe o efetivo; porta grava; geometria e tela leem. (`app/api/views_core.py:529`; `app/ponto/registro_batida.py:157`; `app/ponto/turnos.py:353`; `app/templates/ponto/batidas_offline_pendentes.html:66`) |
   | `timestamp_dispositivo` | Fila PWA produz; API interpreta e passa à porta. Tela de pendentes lê; `descartadas()` compara com `timestamp`, medindo **descarte do horário**, não atraso de chegada. (`app/static/pwa.js:115`; `app/api/views_core.py:692`; `app/templates/ponto/batidas_offline_pendentes.html:71`; `app/api/hora_do_aparelho.py:178`) |
   | `sincronizado_em` | Campo nullable, **não preenchido por essa chamada da API nem acrescentado pela porta**; a tela de pendentes tenta exibi-lo. (`app/ponto/models.py:77`; `app/api/views_core.py:680`; `app/ponto/registro_batida.py:157`; `app/templates/ponto/batidas_offline_pendentes.html:88`) |
   | `criado_em` | Django preenche com `auto_now_add`; o pareador também o lê para identificar eco de flush. (`app/ponto/models.py:79`; `app/ponto/turnos.py:343`) |
   | `canal` / `origem` | API grava origem `app` para ambos; `device_id` decide `apk`/`pwa`. O juiz de selfie lê origem/canal para decidir o prazo. (`app/api/views_core.py:680`; `app/ponto/registro_batida.py:53`; `app/ponto/registro_batida.py:80`) |

   Assim, **atraso superior a uma hora é mensurável para PWA reenviado pela fila**, usando `criado_em - timestamp_dispositivo`; dentro da janela aceita, também `criado_em - timestamp`. Isso usa criação no banco como referência, não um carimbo explícito de chegada HTTP. Para PWA online sem horário do dispositivo, o instante original não fica disponível; `sincronizado_em` não resolve nessa porta. (`app/ponto/models.py:75`; `app/api/hora_do_aparelho.py:141`; `app/api/views_core.py:680`)

   A fórmula e o universo usados nos números informados — inclusive cobertura de tentativas que nunca chegaram — são **[nao verificado]**. Portanto, atribuir o zero a perda, ausência de tentativas offline ou somente limitação da medida é **[nao verificado]**.

3. **Rede cai durante o POST; duplicação**

   Na **v2 online**, rejeição do `fetch` entra no `catch` e tenta guardar na fila. Porém, o horário é gerado **ao enfileirar depois da falha**, não preservado desde o clique. Não há timeout explícito no POST; enquanto o `fetch` não terminar/rejeitar, esse `catch` não roda. Resposta HTTP não satisfatória, inclusive 5xx, mostra erro e reabilita o botão **sem enfileirar**. (`app/templates/ponto/bater_ponto_v2.html:335`; `app/templates/ponto/bater_ponto_v2.html:341`; `app/templates/ponto/bater_ponto_v2.html:349`; `app/static/pwa.js:109`)

   No **reenvio de item já guardado**, falha de rede ou de leitura do JSON conserva o item; outra sincronização pode reenviá-lo. No POST HTML de `/ponto/bater/`, não existe recuperação equivalente: ele faz `form.submit()` sem `catch` que grave a tentativa. (`app/static/pwa.js:153`; `app/static/pwa.js:178`; `app/templates/ponto/triagem/apto.html:210`)

   **Não há chave dedicada de idempotência enviada pelo PWA**, nem envio do `id` local. Há deduplicação servidor por colaborador/tipo em ±5 minutos do horário efetivo, deduplicação adicional na porta e unicidade de `(colaborador, timestamp)`. Isso protege repetições com dados compatíveis; não garante ausência de duplicata quando a resposta se perde e a nova tentativa ganha outro horário, especialmente fora das janelas. (`app/static/pwa.js:143`; `app/api/views_core.py:549`; `app/ponto/registro_batida.py:106`; `app/ponto/models.py:106`)

4. **Service worker e alcance dos testes** [nao verificado]

   O SW **não intercepta o POST de bater**: retorna imediatamente para qualquer método diferente de GET. O reenvio da fila vai para a API, roteada ao core e à porta de registro. ([app/static/sw.js:35](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/sw.js:35); `Caddyfile:8`; `app/api/urls_core.py:14`; `app/api/views_core.py:680`)

   `test_sw_offline_fallback.py` executa o SW real em Node, com rede/cache simulados e **requisições sempre GET**. Verifica erro de rede sem cache, retorno do cache existente e preferência pela rede. É teste do SW/PWA; não prova POST, criação de cache HTML, IndexedDB ou reenvio de batida. (`app/core/tests/test_sw_offline_fallback.py:25`; `app/core/tests/test_sw_offline_fallback.py:44`; `app/core/tests/test_sw_offline_fallback.py:79`; `app/core/tests/test_sw_offline_fallback.py:89`; `app/core/tests/test_sw_offline_fallback.py:96`)

   `test_batida_offline.py` envia POST direto com Django Client autenticado, horário ISO e **sem `device_id`**: pelo classificador, representa API/canal PWA, não cliente APK. Verifica criação, repetição imediata sem segunda linha, erros de payload e aceitação HTTP de horário antigo. Não executa navegador, fila ou reconexão; o caso antigo só exige HTTP 200, não preservação daquele horário em `timestamp`. Resultado de execução nesta análise: **[nao verificado]**. (`app/ponto/tests/test_batida_offline.py:49`; `app/ponto/tests/test_batida_offline.py:60`; `app/ponto/tests/test_batida_offline.py:68`; `app/ponto/tests/test_batida_offline.py:78`; `app/ponto/tests/test_batida_offline.py:104`; `app/ponto/registro_batida.py:61`)

5. **Veredito: GUARDADA — com tela disponível, requisitos atendidos e transação concluída, a batida offline fica em IndexedDB; existem bloqueios prévios e descarte em HTTP 400, portanto não há garantia universal de conservação.** ([app/static/pwa.js:107](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/pwa.js:107); `app/templates/ponto/bater_ponto_v2.html:325`; `app/static/pwa.js:166`)

   A LEI local L-030 diz que “batida de chão nunca é barrada”; os bloqueios prévios da v2 e a remoção em 400 precisam ser avaliados contra essa regra nos próprios produtores/fila, mantendo a escrita servidor na porta única. Esses são os sítios de origem do problema, não o contador de atrasos. Enquadramento jurídico externo específico da Portaria 671: **[nao verificado]**. (`app/docs/LEIS.md:78`; `app/templates/ponto/bater_ponto_v2.html:285`; `app/static/pwa.js:166`; `app/ponto/registro_batida.py:94`; `CLAUDE.md:17`)

## 06/11 20:47 · sessao 29b41450d58c · - · - · origem=Ronald (tmux)

Achados v2 e MORDE, por inspeção e testes somente em memória: [nao verificado]

| Achado v2 | Estado v3 |
|---|---|
| Linha compartilhada `]; CAD=700`, chamada/import/monkeypatch após `;` | **Fechado:** esses nós permanecem no dump; casos `linha_par`, `decisao`, `monkey` distinguem as alterações. (`bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:382`, `bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:406`) |
| `papel`, `estado`, `id`, chave nova/removida | **Fechado sintaticamente** para chaves não declaradas: apenas valores de chaves declaradas são mascarados; estrutura/chaves ficam no dump. Os casos novos cobrem `estado`, `papel`, adição de chave; não há caso de remoção. (`bin/tests/test_lei_akita.sh:205`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:402`) |
| `for`, `with`, walrus, `del`, import na unicidade | **Fechado para essas formas:** `Name` não `Load` e aliases são contados. Confirmei cada forma em memória: duas ligações, declaração rejeitada. O mecanismo antigo de Name como constante-string desapareceu; valores `Name` não são mascarados. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:155`, `bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:206`) |
| Redeclaração no ALVO | **Fechado para as formas contadas; agora há caso discriminante:** checkout de `for_decl` no ALVO antes de julgar outro commit. (`bin/tests/test_lei_akita.sh:175`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:417`) |
| Erro Git | **Mudou de forma:** `diff` foi eliminado; `show` continua gerando `PADRAO_ERRO`; qualquer erro de `cat-file` vira `None`, qualquer erro da sonda de pai vira `False`. A docstring agora promete especificamente erro de `git show`, mas continua sem distinguir falha operacional dessas sondas. (`bin/tests/test_lei_akita.sh:137`, `bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:216`, `bin/tests/test_lei_akita.sh:222`) |

**Nova lacuna da unicidade:** `ExceptHandler.name` e nomes de captura de `match` não são nós `Name`; o contador não os alcança. Em memória, os dois exemplos abaixo resultaram `_ligacoes(..., 'PAPEL_DO_MODULO') == 1` e `_dado_medido(...) == {'R': frozenset({'numero'})}`. Ocorrem ligações adicionais sem detecção. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:175`)

```python
PAPEL_DO_MODULO = 'medida'
DADO_MEDIDO = {'R': ('numero',)}
R = [{'numero': 1}]

# Alternativa A:
try:
    1 / 0
except Exception as PAPEL_DO_MODULO:
    pass

# Alternativa B:
match 1:
    case PAPEL_DO_MODULO:
        pass
```

A alternativa A ainda apaga o nome ao sair do handler; B troca seu valor por `1`. A garantia de “única ligação” não corresponde ao Python executado. Os casos MORDE não incluem essas formas. (`bin/tests/test_lei_akita.sh:148`, `bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:408`)

**Há cláusulas sem mordida, mesmo aceitando os três mutantes vermelhos informados pelo usuário.** Extraí funções reais, fixture `PL`, os 21 `casos_m` e os dois casos de ALVO. Executei em memória com Git substituído por snapshots de strings; baseline e cada mutante abaixo tiveram **zero divergências nesses 23 casos**. Isso não foi execução integral do shell nem teste com Git real. (`bin/tests/test_lei_akita.sh:360`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:419`)

| Mutante sobrevivente nos 23 casos | Cláusula sem discriminação |
|---|---|
| Retirar `and not isinstance(v.value, bool)` | Exclusão explícita de booleanos. (`bin/tests/test_lei_akita.sh:207`) |
| Trocar `(str, int, float)` por `(str,)` | Aceitação de valores numéricos: positivos existentes são strings; nenhum caso exige `int` ou `float`. (`bin/tests/test_lei_akita.sh:207`, `bin/tests/test_lei_akita.sh:361`, `bin/tests/test_lei_akita.sh:378`) |
| Trocar `if _de_topo(t, k.value) is None:` por `if False:` | Existência/unicidade do contêiner declarado. O teste de ALVO duplicado liga `PAPEL_DO_MODULO`, não `R`. (`bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:384`, `bin/tests/test_lei_akita.sh:417`) |
| Retirar `and None not in v.keys` | Rejeição de `**` na declaração `DADO_MEDIDO`; nenhuma fixture planta declaração com desempacotamento. (`bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:360`, `bin/tests/test_lei_akita.sh:400`) |

Os dois casos retroativos não foram executados nessa simulação; alteram número string e `CAD`, portanto não exercitam as quatro cláusulas acima por inspeção. (`bin/tests/test_lei_akita.sh:423`)

Outras lacunas por inspeção: validação de declaração vazia/malformada, tupla de chaves vazia/tipo errado, múltiplos alvos, contêiner inexistente, import/def/global como ligação adicional **no ALVO**, módulo criado/apagado/inválido, commit raiz, erros de `show`/`cat-file`/pai, chave removida e efeitos semânticos de valores declarados. Não há esses casos no bloco de medida. (`bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:216`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:433`)

O comentário do MORDE ainda diz “por LINHA” e “nem concessão retroativa”, embora a implementação use AST e os casos seguintes exijam concessão retroativa; são resíduos documentais. (`bin/tests/test_lei_akita.sh:350`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:420`)

## 06/11 20:46 · sessao c811294be807 · BO-[nome]-0611-V2 · sonda · origem=Code

**Não fecho nenhum item como “resolvido”: faltam evidências da ação na tela, do resultado posterior no DiaPago e da validação operacional. A sonda fornecida termina com `ERROS {}`; as limitações abaixo são de evidência disponível.** [nao verificado]

Nos blocos, a sequência da célula é **trabalha / veredito / origem / status**. Os estados de produção vêm exclusivamente do texto fornecido e recebem `[nao verificado]`.

**02 — col78**

- **ESTADO HOJE:** entre 21/10 e 05/11, os plantões apresentados estão `true / concorde / gerada / ok`, alternados com `false / concorde / gerada / folga`. Em 06/11: célula #131373 `true / null / gerada / aberto`, `pede_decisao=true`, `resolver_dia=false`. Chamado #34349 resolvido via sistema; pergunta #49403 sem resposta/validação, mas carimbada por `completude_turno`. A FICHA não apresenta DiaPago nem identifica uma correção solicitada por dia/hora/ID. [nao verificado]
- **DONO L-099:** **não atribuível com o dado disponível**. Não há uma divergência específica identificada para separar ESTRUTURA, CADASTRO e BATIDA. O juiz exige insumos adicionais, incluindo cadastro×realidade, vínculo e fase; não basta encontrar uma célula `ok`. (`app/ponto/management/commands/e6_oraculo.py:53`, `:98`, `:110`.)
- **PORTA DA TELA:** a porta que resolveria **a queixa concreta** permanece **[nao verificado]**, porque essa queixa não foi identificada no dado corrigido. A pergunta #49403 carimbada não demonstra que uma correção humana foi executada. [nao verificado]
- **OBRA:** nenhuma das quatro obras pode ser atribuída apenas pelo ID corrigido. Não proponho ID novo sem localizar o fato pendente. **Validação operacional pendente:** identificar a correção alegada e conferir seu resultado no espelho e DiaPago. [nao verificado]

**05 — col456**

- **ESTADO HOJE:** plantões apresentados até 04/11: `true / concorde / gerada / ok`; folgas intermediárias: `false / concorde / gerada / folga`. Em 06/11, célula #134856 `true / cobrado / gerada / irregular`, retenção `espelho_cobrado`, sem Resolver dia. Chamado #37287 aberto; perguntas #54491/#54727 sem resposta, validação ou materialização. Em 31/10, #36201 resolvido e #36202 `registrado`; justificativa #3408 rejeitada. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **ESTRUTURA para a barreira de GPS comprovada no código**: V2 interrompe a tentativa sem coordenadas e a API retorna 403 antes do registro. **Não está demonstrado que essa barreira causou alguma marcação ausente de col456.** (`app/templates/ponto/bater_ponto_v2.html:297`; `app/api/views_core.py:440`, `:454`; causalidade individual [nao verificado].)
- **PORTA DA TELA:** **PORTA NAO EXISTE para o admin retirar essa barreira pelo fluxo analisado**: a condição da API é direta, sem parâmetro de cadastro nesse ramo. A tela de bater ponto existe, mas não é uma ação administrativa que sane o passado. (`app/ponto/urls.py:12`; `app/ponto/views.py:242`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/api/views_core.py:440`.)
- **OBRA:** complementar a pendência existente **L-030 PELA-METADE**, que já nomeia essas barreiras; ela cabe melhor que DST, O10-C, O261 ou o job do lote. Não precisa de ID novo. (`app/docs/LEIS.md:162`.)
- **RESULTADO/VALIDAÇÃO:** remover a barreira deverá permitir alcançar o escritor de batidas; o registro agenda recálculo por evento. Isso não recupera automaticamente uma tentativa histórica não apresentada. O caso exige conferir tentativa, marcação efetivamente gravada, espelho e DiaPago posterior. (`app/ponto/registro_batida.py:157`, `:175`; resultado individual [nao verificado].)

**07 — col836, perguntas #43643 e #44051**

- **ESTADO HOJE:** células #112095/#112096, 13–14/10, `true / indefinida / gerada / escala_suspeita`, retenção `espelho_indefinida`, sem Resolver dia. Ambas responderam `17:00`, foram validadas em 16/10 e têm recusa preenchida, com 192 caracteres. Entretanto, seus carimbos `materializada_em`, via `cadastro_folga`, são de **14/10 e 15/10, anteriores às respostas e à validação**. Chamados #30870/#31151 resolvidos via sistema. DiaPago 0 consta no enunciado; as linhas correspondentes não foram anexadas. [nao verificado]
- **GUARDA comprovada na cópia:** a validação só chama `_materializar_uma` quando `materializada_em is None`; o laço também seleciona somente perguntas sem esse carimbo. A devolução RECUSA-FALA igualmente não age quando já há `materializada_em`. (`app/chamados/services/validacao.py:422`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/validacao.py:182`.)
- **Limite da conclusão:** esses são os ramos atuais que impedem novo plantio nesse estado. **Qual ramo escreveu historicamente a recusa de 192 caracteres é [nao verificado]**: comprimento não identifica motivo. A via `cadastro_folga` corresponde ao agrupamento LIMBO-FOLGAS; não significa, por si, batida plantada. (`app/chamados/models.py:2265`; `app/chamados/juizes.py:1463`.)
- **DONO L-099:** **ESTRUTURA para o percurso sem reprocessamento acessível de pergunta validada/carimbada cujo dia continua acusando**; a correção do cadastro subjacente deve permanecer separada. O dono formal dos dois dias não foi medido. (`app/chamados/services/validacao.py:366`, `:422`; `CLAUDE.md:176`; dono formal [nao verificado].)
- **PORTA DA TELA:** existe Validar no detalhe, mas **não resolve essas duas perguntas hoje**: o template não oferece o botão para pergunta validada e o backend responde `ja_validada`. **PORTA NAO EXISTE nesse fluxo para reprocessar esse estado.** (`app/chamados/views.py:483`, `:501`; `app/templates/chamados/partials/_pergunta_card.html:42`, `:92`; `app/chamados/services/validacao.py:366`.)
- **OBRA:** **complemento O261**, abrangendo carimbo, validação, lastro e saída operacional. Não confundir com o plantio noturno deslocado: esse mecanismo não foi demonstrado aqui. (`app/docs/BACKLOG.md:1450`; mecanismo individual [nao verificado].)
- **VALIDAÇÃO OPERACIONAL pendente:** esclarecer a recusa textual e o lastro do carimbo, executar a resolução adequada pela tela e conferir célula, eventual batida e DiaPago. Nenhum desses resultados posteriores foi fornecido. [nao verificado]

**08 — col617, atestado**

- **ESTADO HOJE:** a FICHA **não mostra atestado pendente atualmente**: #5380 está rejeitado e #5399 aprovado para 05–06/11. Células #122699/#122700 `true / concorde / gerada / ausencia`; chamados #37140/#37319 resolvidos. As perguntas de 05/11 foram carimbadas por `ausencia_dia`; #54517, de 06/11, por `celula_absolveu`. DiaPago não fornecido. [nao verificado]
- **O QUE O ESPELHO MOSTRA ENQUANTO PENDENTE:** o montador separa cobertura de cobrança e cobertura de folha. O juiz da palavra pode mostrar **“Atestado aguardando decisao”**, preservando o veredito: o pedido aberto não concede o abono da aprovação. (`app/ponto/services/espelho.py:582`, `:588`, `:878`; `app/ponto/services/dia_decidido.py:224`, `:420`.)
- **DONO L-099:** **não há divergência atual demonstrada para atribuir um dono**. Aprovação documental é ato administrativo; eventual incoerência persistente entre os leitores após a decisão seria ESTRUTURA. (`CLAUDE.md:176`; divergência do caso [nao verificado].)
- **PORTA DA TELA:** **Ausências → abrir o registro → Aprovar**, com permissão `editar_ausencias`; o botão depende das condições documentais. View chama `decidir`, que escreve status e carimbos do decisor. (`app/ponto/urls.py:65`; `app/ponto/views.py:2599`, `:2603`, `:2643`; `app/templates/ponto/partials/painel_ausencia.html:306`, `:321`, `:324`; `app/ponto/services/ausencia.py:911`, `:913`.)
- **RESULTADO:** o save da ausência provoca julgamento dos dias e a porta grava veredito/status. **A aprovação apenas invalida o fechamento; não grava imediatamente o DiaPago.** O recálculo posterior chama sua lavratura. (`app/ponto/signals.py:93`, `:143`; `app/ponto/portas/celula.py:1030`; `app/ponto/services/ausencia.py:946`; `app/ponto/services/fechamento.py:1640`, `:1184`.)
- **PORTA COMPLEMENTAR para apuração:** botão **Recalcular** no fechamento, cuja view chama `recalcular_fechamento_mes`. Sua execução e resultado neste caso não foram fornecidos. (`app/templates/ponto/fechamento.html:87`; `app/ponto/views.py:1226`, `:1252`; execução [nao verificado].)
- **OBRA:** não atribuir automaticamente a DST: seu escopo declara ausência lançada como dia decidido, sem nova porta de decisão do dia. **Validação pendente:** conferir período aprovado, resultado no espelho e lavratura posterior do DiaPago. (`app/docs/BACKLOG.md:1793`; resultado operacional [nao verificado].)

**09 — col1000, 29–30/10, chamados #37325/#37326**

- **ESTADO HOJE:** células #142904/#142905 `true / furo / gerada / irregular`, `pede_decisao=true`, `resolver_dia=false`, sem retenção apresentada. Chamados `em_analise`, sem resolução/via. Perguntas #54529/#54530, `intervalo_volta`, sem resposta, validação ou materialização, veredito `nao`. O dado apresenta E/S/S em cada dia, sem retorno E do intervalo. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **BATIDA como encaminhamento do retorno ausente**, condicionado à confirmação dos insumos cadastrais. O juiz prioriza folga não definida/ausência de vínculo antes da contagem ímpar; esses insumos por dia não vieram. (`app/ponto/management/commands/e6_oraculo.py:98`, `:106`; dono formal [nao verificado].)
- **PORTA DA TELA:** detalhe `/chamados/local/<id>/` → resposta do colaborador → **Validar pelo admin**. A view chama a autoridade de validação; sem resposta ela recusa `sem_resposta`. (`app/chamados/urls.py:14`; `app/chamados/views.py:359`, `:483`, `:1283`; `app/templates/chamados/partials/_pergunta_card.html:29`, `:92`; `app/chamados/services/validacao.py:363`.)
- **Limite da porta:** o modal **Corrigir** existe, mas fica dentro da condição `respondida_em`; para essas perguntas ainda sem resposta ele só mostra espera. Não há nessa tela uma correção administrativa de horário oferecida **nesse estado**. (`app/templates/chamados/partials/modal_fio.html:212`, `:295`, `:313`.)
- **RESULTADO esperado:** materialização escreve pela porta `registrar_batida`; em intervalo fixo pode usar o horário do marco, não a hora livre informada. A validação agenda recálculo após commit, chegando à lavratura do DiaPago. Aceitação e resultado concreto dependem das guardas e não foram observados. (`app/chamados/services/materializacao.py:404`, `:901`; `app/chamados/services/validacao.py:406`; `app/ponto/services/fechamento.py:1794`, `:1808`, `:1184`; resultado [nao verificado].)
- **OBRA:** tratamento do dado pela porta existente; **complemento O261** se o requisito for dar ao admin saída antes da resposta. Não é a classe DST dos dias sem trabalho. (`app/docs/BACKLOG.md:1450`, `:1793`.)
- **VALIDAÇÃO OPERACIONAL pendente:** obter o fato do intervalo, executar a ação e conferir batida, espelho e DiaPago; apenas mudar o status do chamado não sana o furo. (`app/chamados/models.py:185`; execução [nao verificado].)

**10 — col399, 31/10**

- **ESTADO HOJE:** célula #138663 `false / fato_sem_previsao / gerada / folga_trabalhada`, sem Resolver dia/retenção. Batidas #142321 E 06:20:43 e #142798 S 18:22:41 aprovadas e não retratadas. Pergunta #52088 carimbada por `celula_absolveu`. A troca #5373 é **para 24/10**, aprovada e criada **em 04/11**, não “registrada em 24/10”; não há par/documentos apresentados. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **ESTRUTURA da porta de troca faltante**. O dono formal da divergência cadastral/financeira desse dia não foi medido. (`app/docs/RELATO.md:64`; `app/ponto/management/commands/e6_oraculo.py:98`; dono do dado [nao verificado].)
- **PORTA DA TELA: PORTA NAO EXISTE para confirmar a troca.** O backend O10-B existe: `trocar_plantao` exige os dois lados e documentos, e chama `editar_dia` para cedente e substituto dentro do atomic. Escreve trabalha, DNA, geradora, origem editada, autor/data e trilha. (`app/ponto/portas/celula.py:632`, `:653`, `:662`, `:673`, `:603`.)
- **O QUE FALTA:** view e formulário que chamem essa operação com substituto, duas cartas e motivo. O despacho O10-C registra expressamente ausência de chamador de UI. **Resolver dia não substitui isso**: seu formulário não confirma o par de troca, e “trabalhou” recusa natureza diferente de trabalho. (`app/docs/RELATO.md:64`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/services/veredito_celula.py:221`.)
- **OBRA:** **sim, O10-C**, complemento da porta já construída. DST pode tratar decisão sobre batida em folga, mas não substitui a confirmação documental da troca. (`app/docs/BACKLOG.md:1811`, `:1793`.)
- **RESULTADO/VALIDAÇÃO:** a porta reavalia os dias e solicita recálculo; este grava DiaPago. Falta executar pela futura tela e conferir **os dois lados** no espelho e DiaPago, inclusive repetição idempotente. (`app/ponto/portas/celula.py:422`, `:445`; `app/ponto/services/dia_pago.py:357`; resultado operacional [nao verificado].)

**11/12 — col921, competência 11**

- **ESTADO HOJE:** `batidas=[]`. Em 21–23 e 25–29/10: `true / furo / gerada / irregular`; em 30/10 e 01–05/11: `true / cobrado / gerada / irregular`; em 06/11: `true / cobrado / gerada / previsto`. Folgas 24 e 31/10: `false / concorde / gerada / folga`. Resolver dia falso em todos esses dias. Há perguntas sem resposta/materialização e chamados de batida abertos, além do cadastro #37165 aberto. As seis faltas aprovadas apresentadas pertencem a 14–20/10, competência anterior. DiaPago não fornecido. [nao verificado]
- **CONFERÊNCIA DST:** **não corresponde a `folga_trabalhada sem Resolver dia`**: esse status não aparece na FICHA de col921. Para os dias irregulares da competência corrente, a oferta de Resolver dia é barrada por `pode_ultimo_recurso`; para folga trabalhada, adicionalmente, o status nem integra o conjunto que oferece o botão. (`app/colaboradores/services/calendario.py:94`, `:103`, `:455`, `:738`; estado individual [nao verificado].)
- **DONO L-099:** **CADASTRO como encaminhamento da pendência #37165**, mas sua incidência formal em cada dia é **[nao verificado]**. Zero batidas, isoladamente, não permite chamar o caso de BATIDA pelo ramo de paridade ímpar. (`app/ponto/management/commands/e6_oraculo.py:98`, `:106`, `:110`; chamado [nao verificado].)
- **PORTA DA TELA existente para decisão factual:** **Ausências → Lançar**, escolhendo colaborador, tipo e data; a view cria ausência aprovada pelo admin. Se a decisão for falta, é necessário confirmar primeiro a previsão de trabalho: a autoridade veta falta em folga. (`app/ponto/urls.py:57`; `app/ponto/views.py:2368`, `:2415`; `app/templates/ponto/lancar_ausencia.html:12`, `:26`, `:88`; `app/ponto/precedencia.py:536`.)
- **RESULTADO:** falta decidida pode lavrar status `falta`; ausência aprovada pode lavrar `ausencia`. Isso não demonstra atualização do DiaPago: conferir a lavratura após apuração, pelo percurso de recálculo já indicado. (`app/ponto/status_dia.py:139`, `:141`; `app/ponto/services/fechamento.py:1184`; resultado individual [nao verificado].)
- **OBRA:** não atribuir a DST apenas porque falta o botão. Sanear cadastro e decidir os fatos pelas portas existentes; eventual beco de pergunta/fio cabe em **O261**, depois de identificado. Não há fundamento apresentado para ID novo. (`app/docs/BACKLOG.md:1793`, `:1450`; enquadramento individual [nao verificado].)
- **VALIDAÇÃO OPERACIONAL pendente para ambos:** confirmar cadastro dos dias, executar decisões justificadas e conferir competência 11 no espelho/DiaPago. Os itens continuam abertos no critério solicitado. [nao verificado]

**03 — validar-lote: duração HTTP e recusas são problemas separados**

- **ESTADO HOJE:** os logs agregados fornecidos mostram tentativas com 29, 37, 40 e 42 recusadas; as quatro mais recentes apresentadas têm 40. O `ui_lote.log` mostra respostas **HTTP 200**, inclusive em 40–122 segundos. Não há, nesse anexo, timeout efetivamente registrado. [nao verificado]
- **DONO L-099:** **ESTRUTURA para o processamento longo dentro da requisição**. A view chama o serviço sincronamente antes de renderizar. (`app/chamados/views_cobrar.py:372`, `:381`; `CLAUDE.md:89`.)
- **TIMEOUT:** `gthread` e `timeout=30` estão configurados, mas a própria documentação da cópia distingue worker travado de requisição longa. Portanto, **“passou de 30 segundos, caiu por timeout” não está provado**. (`app/gunicorn.conf.py:19`, `:21`; `app/docs/TICKETS.md:132`; queda concreta [nao verificado].)
- **PORTA DA TELA:** `/chamados/validar-lote/tela/`, formulário HTMX; o resultado distingue recusadas e validadas sem gravação e apresenta motivos por referência. (`app/chamados/urls.py:37`; `app/chamados/views_cobrar.py:372`; `app/templates/chamados/partials/_lote_preview.html:7`, `:21`.)
- **OBRA:** **LOTE-VALIDAR-VIRA-JOB**, já existente para esse endpoint. O job resolve a ocupação da requisição; não elimina recusas de negócio. (`app/docs/BACKLOG.md:1806`; `app/chamados/services/validacao.py:682`.)

**Não é possível distribuir as 40 recusas por causa com o dado anexado.** Faltam as referências daquele POST e o retorno `recusados[].motivo`; `RECUSA03` fornece somente comprimento do texto, motivo da pergunta e via atual. [nao verificado]

Os ramos que recusam são:

| Motivo/ramo da cópia | Referência | Contagem nas 40 |
|---|---|---|
| Caso não encontrado | `app/chamados/services/validacao.py:682` | [nao verificado] |
| Já validada | `app/chamados/services/validacao.py:685` | [nao verificado] |
| Fora da classe A permitida ao lote | `app/chamados/services/validacao.py:688` | [nao verificado] |
| Escape, resposta de posto ou confirmação sem marco | `app/chamados/services/validacao.py:807`, `:809`, `:815` | [nao verificado] |
| Escolha que exige decisão individual | `app/chamados/services/validacao.py:816` | [nao verificado] |
| Resposta vazia, tipo inadequado ou hora ilegível | `app/chamados/services/validacao.py:813`, `:817` | [nao verificado] |
| Sem marco esperado | `app/chamados/services/validacao.py:820` | [nao verificado] |
| AM/PM ou desvio além do envelope | `app/chamados/services/validacao.py:822` | [nao verificado] |
| Batida apurável próxima da hora | `app/chamados/services/validacao.py:827` | [nao verificado] |
| Validação individual não deixou carimbo | `app/chamados/services/validacao.py:712` | [nao verificado] |

O agrupamento possível dos **68 registros anexados**, por **via atual**, é: `admin_decidiu=48`, `declaracao=8`, `paridade_recusada=7`, `ausencia_dia=3`, `fato_ocupado=1`, via vazia=1. **Isso não é decomposição causal das 40 recusas de um POST.** [nao verificado]

Uma recusa antiga pode coexistir com materialização posterior: a validação não limpa necessariamente `recusa_motivo`. O resultado do lote também separa `validadas`, `recusados` e `nao_gravados`. (`app/chamados/services/validacao.py:396`, `:705`, `:715`; `app/chamados/services/materializacao.py:1064`.)

**Validação operacional pendente:** preservar o resultado por referência de uma tentativa, agrupar seus motivos reais e conferir espelho/DiaPago dos itens efetivamente aceitos. O anexo não permite executar essa conferência. [nao verificado]

**Quem escreve e quem lê os campos de estado usados acima**

| Campo | Escritor | Leitor |
|---|---|---|
| Célula `trabalha`, `origem`, DNA | geração/regeneração/edição na porta: `app/ponto/portas/celula.py:259`, `:272`, `:603`, `:849` | autoridade de dia de trabalho: `app/escala/models.py:1094`; edição/regeneração lê origem em `app/ponto/portas/celula.py:263`, `:854` |
| Célula `veredito`, via, data e ata | `lavrar_veredito`: `app/ponto/portas/celula.py:1028` | status: `app/ponto/status_dia.py:113`; decisão humana no calendário: `app/colaboradores/services/calendario.py:413` |
| `status_dia`, `status_rotulo` | mesma lavra, por `_aplicar_status`: `app/ponto/portas/celula.py:961`, `:1033` | D4: `app/ponto/status_dia.py:287`; calendário: `app/colaboradores/services/calendario.py:663`; template: `app/templates/colaboradores/partials/_calendario_grade.html:99` |
| `resolver_dia`, `pede_decisao` | derivados, não estad

[truncado em 20000 de 23383 caracteres]

## 06/11 20:46 · sessao a5ee1a5bb961 · - · - · origem=Ronald (tmux)

Leitores das chaves declaradas:

| Campo | Quem lê e efeito |
|---|---|
| `RESULTADOS.numero` | `placar()` usa sua veracidade na guarda de prova: valor verdadeiro com prova vazia vira `PENDENTE`; valor falso evita esse rebaixamento. A contagem publica o estado resultante. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:700`) `gerar_estado` publica o valor ou “não medido”. (`bin/gerar_estado.py:99`, `bin/gerar_estado.py:116`) O selo R6 lê sua frase, extrai contagens por regex e converte uma delas a inteiro para confrontar o juiz. (`app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `app/core/tests/test_selo_r6_frase_do_juiz.py:22`, `app/core/tests/test_selo_r6_frase_do_juiz.py:25`) |
| `RESULTADOS.prova` | `.strip()` decide rebaixamento para `PENDENTE`, portanto estado e contagem. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:700`) Publicada pelo gerador. (`bin/gerar_estado.py:100`) As provas de R1 e SOMA determinam aprovação do selo de medição única e o conjunto de arquivos citados. (`app/core/tests/test_r1_termometro_uma_medicao.py:52`, `app/core/tests/test_r1_termometro_uma_medicao.py:57`, `app/core/tests/test_r1_termometro_uma_medicao.py:69`) |
| `RESULTADOS.fonte_do_medido` | É glob operacional: o gerador busca caminhos, extrai datas de seus nomes e escolhe a maior; entrega isso a `placar()` e `veredito()`. Decide data/cadência/cor, além do resultado do selo R1. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:84`, `app/core/tests/test_r1_termometro_uma_medicao.py:52`) |
| `RESULTADOS.fonte` | É copiada para o resultado de `placar()`. (`app/core/placar_estrutural.py:645`) O gerador apenas menciona o nome do campo em uma explicação fixa; não consulta seu valor nesse trecho. (`bin/gerar_estado.py:121`) Seu valor-string participa do selo genérico de afirmações fora do ar; pode mudar acusação e RED. (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:75`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:100`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:110`) |
| `RESULTADOS.nota_do_papel` | Copiada por `placar()` e examinada pelo mesmo selo genérico de todos os campos-string; pode mudar o resultado desse selo. (`app/core/placar_estrutural.py:645`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:75`) Leitor nominal que use esse valor para classificar papel: [não verificado]. |
| `ETAPAS.prova` | Decide `FEITO → EM_CURSO`, quantidade de feitos e percentual. (`app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`) Publicada pelo gerador. (`bin/gerar_estado.py:164`) A presença de `TERMOMETRO R1` seleciona quais itens serão fiscalizados pelo selo de medição única; removê-la retira um item do censo. (`app/core/tests/test_r1_termometro_uma_medicao.py:45`, `app/core/tests/test_r1_termometro_uma_medicao.py:64`) O limite está explicitamente confessado no próprio teste. (`app/core/tests/test_r1_termometro_uma_medicao.py:15`) |

Contraexemplo novo confirmado em memória, sem criar arquivos: inserir `contratos_estruturais: 0/999 verdes` no início da docstring de `placar_estrutural.py` altera o primeiro casamento da regex do handoff de `19/20` para `0/999`, enquanto o dump com docstring mascarada permanece idêntico. O leitor pesquisa o texto bruto inteiro e publica o primeiro casamento. (`app/core/placar_estrutural.py:2`, `bin/tests/test_lei_akita.sh:195`, `bin/handoff_sessao.sh:179`, `bin/handoff_sessao.sh:181`) O selo R6 compara exclusivamente `RESULTADOS['R6']['numero']` e não essa leitura do handoff. (`app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `app/core/tests/test_selo_r6_frase_do_juiz.py:22`)

A interpretação “prova/fonte apenas documentam uma medição” não vale para esses leitores: `prova` serve como condição de promoção/rebaixamento e seleção de fiscalização; `fonte_do_medido` seleciona os arquivos que fornecem a data decisória. (`app/core/placar_estrutural.py:647`, `app/core/espelho_verdade.py:211`, `app/core/tests/test_r1_termometro_uma_medicao.py:46`, `bin/gerar_estado.py:51`)

## 06/11 20:46 · sessao 1b79a84b9a49 · - · - · origem=Ronald (tmux)

- **Caminho principal da cópia:** dashboard padrão usa `dashboard_colaborador_v2.html`, cujo botão aponta para `/ponto/bater-v2/`; essa rota renderiza `bater_ponto_v2.html`. (`app/core/views.py:270`; `app/templates/core/dashboard_colaborador_v2.html:31`; `app/ponto/urls.py:12`; `app/ponto/views.py:356-357`)

- **Sem rede, com tela disponível:** `v2Confirmar()` trava o botão, exige selfie e GPS e só então consulta `navigator.onLine`. Sem foto/GPS retorna com aviso e botão reabilitado; havendo requisitos, chama `registrarBatidaOffline`, mostra “salva no celular” e oculta o botão. Falha da gravação mostra “Erro ao registrar” e reabilita o botão. (`app/templates/ponto/bater_ponto_v2.html:285-302`; `app/templates/ponto/bater_ponto_v2.html:320-325`; `app/templates/ponto/bater_ponto_v2.html:367-380`)

- **GUARDADA em IndexedDB:** banco `juliani-offline`, versão 2; object store `batidas_pendentes`, chave `id` autoincremental. Guarda tipo, timestamp corrigido pelo gap, timestamp_dispositivo ISO, gap, GPS e foto Blob; resolve após completar a transação. Não é fila em localStorage ou Cache Storage. (`app/static/pwa.js:48-59`; `app/static/pwa.js:107-123`)

- **Reenvio real:** evento `online`, inicialização da página quando `navigator.onLine`, ou mensagem SW `SYNC_BATIDAS`; exige meta `user-perfil=colaborador`, lê toda fila, ordena e envia multipart para `/api/ponto/bater/`. Sucesso com JSON interpretável exclui item; HTTP 400 também exclui, com aviso somente no console; outras falhas deixam item para próxima sincronização. (`app/static/pwa.js:15-17`; `app/static/pwa.js:126-180`; `app/static/pwa.js:320-328`)

- **Background Sync incompleto:** SW tem handler `sync-batidas`: se houver janela, delega por mensagem; sem janela, apenas notifica para abrir o app. Não existe chamada de registro da tag nos produtores da fila ou na inicialização examinados; tampouco SW faz upload autônomo. (`app/static/sw.js:112-130`; `app/static/pwa.js:107-124`; `app/static/pwa.js:323-346`)

- **Timestamp muda entre caminhos:** v2 online monta `FormData` de formulário com tipo/GPS/foto e envia sem `timestamp_dispositivo`; fila guarda esse campo e o acrescenta ao POST do reenvio. O timestamp corrigido/gap guardados não são acrescentados ao multipart de sincronização; GPS accuracy/capturedAt guardados também não são enviados ali. (`app/templates/ponto/bater_ponto_v2.html:102-107`; `app/templates/ponto/bater_ponto_v2.html:330-338`; `app/static/pwa.js:113-118`; `app/static/pwa.js:143-156`)

- **Queda no POST v2:** rejeição de `fetch` entra no catch e grava fila, gerando o timestamp **nesse momento**, não no clique original; não há AbortController/timeout no POST. HTTP não-ok, inclusive 5xx, mostra erro e reabilita o botão, sem enfileirar. Se servidor gravou e resposta se perdeu, o catch pode guardar nova tentativa com novo timestamp; payload PWA não manda chave explícita de idempotência, nem o `id` local. (`app/templates/ponto/bater_ponto_v2.html:335-365`; `app/static/pwa.js:109-118`; `app/static/pwa.js:143-156`)

- **Outra rota ativa:** `/ponto/bater/` renderiza triagem (`apto.html` nos casos normais), com POST nativo para a própria rota, sem catch/retry/fila na queda durante envio online. Offline declarado abre câmera e depois grava a mesma IndexedDB; falha da câmera esconde confirmar. O formulário também não inclui timestamp_dispositivo. (`app/ponto/urls.py:11`; `app/ponto/views.py:225-235`; `app/ponto/views.py:358`; `app/templates/ponto/triagem/apto.html:86-91`; `app/templates/ponto/triagem/apto.html:210-255`; `app/templates/ponto/triagem/apto.html:289-307`)

- **POST web dessa outra rota:** `ramo_registrar_es` usa `agora=timezone.now()`, entrega esse instante à porta, origem `'app'` e canal calculado sem device_id. Somente `/ponto/batida-offline/` foi aposentado com HTTP 410; `/ponto/bater/` continua roteado. (`app/ponto/services/triagem_batida.py:625-626`; `app/ponto/services/triagem_batida.py:707-719`; `app/ponto/views.py:417-428`; `app/ponto/urls.py:11-13`)

- **Template legado não comprova rota atual:** `bater_ponto.html` tem catch que enfileira rede/HTTP não-ok, mas a view atual escolhe v2 ou triagem. Não atribuir esse tratamento à triagem servida. (`app/templates/ponto/bater_ponto.html:527-556`; `app/ponto/views.py:356-358`)

- **SW não intercepta POST:** retorna de imediato para qualquer método diferente de GET. Navegação HTML usa rede/cache/erro, mas esse ramo não grava a página no cache; instalação pré-cacheia somente JS, manifest e ícones. Assim, não há prova de que seja possível **abrir** a tela v2 pela primeira vez sem rede; a fila descrita pressupõe tela já disponível. (`app/static/sw.js:3-12`; `app/static/sw.js:33-50`; `app/static/sw.js:67-79`)

- **LEI local e origem de eventual cura:** a regra “batida de chão nunca é barrada” e porta única estão em `CLAUDE.md:140-141`; os bloqueios selfie/GPS estão no produtor v2 antes da gravação, e a vulnerabilidade de recebimento/retry está na construção/envio do payload e fila, respectivamente. (`app/templates/ponto/bater_ponto_v2.html:285-325`; `app/templates/ponto/bater_ponto_v2.html:330-365`; `app/static/pwa.js:107-180`)

## 06/11 20:45 · sessao 9290390fa592 · - · - · origem=Ronald (tmux)

- Roteamento da cópia: `Caddyfile:8-9` manda `/api/ponto/*` para `core:8000`; `app/config/urls_core.py:6` inclui `api.urls_core`, que importa `views_core` e registra `ponto/bater/` em `app/api/urls_core.py:5,14`. A outra casca também tem a rota em `app/api/urls.py:67`.

- A API lê `timestamp_dispositivo`, chama `hora_do_aparelho` e recebe `(ts_efetivo, ts_aparelho, motivo)` em `app/api/views_core.py:519-531`. A versão UI faz o mesmo em `app/api/views.py:535-539`.

- A regra aceita epoch em segundos/milissegundos e ISO, converte para datetime aware e devolve `None` quando não consegue ler (`app/api/hora_do_aparelho.py:72-121`). Se o aparelho diz um instante de 0 a 48 horas atrás, esse instante vira o efetivo; se ausente/ilegível, futuro ou mais antigo que 48 horas, o efetivo vira `agora`, mas um horário legível descartado continua retornado como `ts_aparelho` (`app/api/hora_do_aparelho.py:62-67,124-153`).

- Escrita da API: `registrar_batida(colab, ts_efetivo, tipo, 'app', ..., canal=canal_da_batida(device_id), timestamp_dispositivo=ts_aparelho)` em `app/api/views_core.py:680-694` e `app/api/views.py:683-697`; o chokepoint cria `Batida(timestamp=timestamp, origem=origem, **campos)` em `app/ponto/registro_batida.py:152-159`.

- `canal_da_batida` escreve `"apk"` se `device_id` não é vazio e `"pwa"` nos demais casos (`app/ponto/registro_batida.py:53-61`). Portanto, é classificação pelo payload; não uma prova independente do aplicativo efetivamente usado (`app/ponto/registro_batida.py:61`). `origem='app'` é comum aos dois canais na API (`app/api/views_core.py:680-684`).

- `timestamp` é o instante efetivo, `timestamp_dispositivo` é nullable e recebe o instante legível do aparelho; `sincronizado_em` é nullable; `criado_em` tem `auto_now_add=True` (`app/ponto/models.py:23,75-79`). A chamada da API não fornece `sincronizado_em` nem `timestamp_gap_ms`, e o chokepoint não os acrescenta; portanto novas linhas desse caminho deixam esses campos NULL (`app/api/views_core.py:680-694`; `app/ponto/registro_batida.py:104-158`; `app/ponto/models.py:76-79`).

- Consequência para a medida: em uma batida recebida pela API com timestamp do dispositivo legível, é possível medir `criado_em - timestamp_dispositivo`; nos casos aceitos pela janela, também `criado_em - timestamp`. `criado_em` é criação no banco, não um campo explícito de chegada HTTP. Medir por `sincronizado_em` não funciona nessa porta, pois não é preenchido (`app/api/hora_do_aparelho.py:141-153`; `app/api/views_core.py:680-694`; `app/ponto/models.py:75-79`). A fórmula usada para os números de produção informados permanece [nao verificado].

- Leitores concretos: a tela de pendentes mostra `timestamp`, `timestamp_dispositivo`, `timestamp_gap_ms` e `sincronizado_em` (`app/templates/ponto/batidas_offline_pendentes.html:66-90`); `descartadas()` compara `timestamp - timestamp_dispositivo` para contar horários descartados, não atraso de chegada (`app/api/hora_do_aparelho.py:156-185`); `sequencia_relogio_x_jornada()` filtra por `criado_em` e ordena/lê `timestamp` (`app/ponto/turnos.py:2376-2385`); o cron de selfie lê `origem`, `canal` e idade calculada com `timestamp` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66-77`).

- Atenção à terceira porta: o formulário web em `app/ponto/services/triagem_batida.py:708-719` grava `timestamp=agora`, `origem='app'` e canal derivado de `device_id`, mas **não fornece `timestamp_dispositivo`**. Se esse formulário for usado, a medida de instante do dispositivo não é possível pelos campos dessa batida (`app/ponto/services/triagem_batida.py:708-719`; `app/ponto/models.py:75-79`). O agente de PWA deve estabelecer qual template usa qual porta.

- Idempotência no servidor existe por dados, sem token dedicado nessas chamadas: API consulta mesmo colaborador/tipo em ±5 minutos do `ts_efetivo` e retorna `status='ja_registrada'` (`app/api/views_core.py:549-568`); o chokepoint deduplica mesmo tipo em ±60 segundos e devolve uma linha existente para mesma chave `(colaborador, timestamp)`, mesmo se tipo diferente/retratada (`app/ponto/registro_batida.py:106-122`); o modelo impõe `unique_together = [["colaborador", "timestamp"]]` (`app/ponto/models.py:106`). Isso não substitui uma fila nem mantém o instante original quando o cliente faz uma nova tentativa com horário novo (`app/api/views_core.py:529-554`; `app/ponto/models.py:106`).

- `test_batida_offline.py` não exercita navegador, falta de rede, fila, reconexão ou SW: usa `Client.force_login()` e dois POSTs diretos em `/api/ponto/bater/`, com ISO em `timestamp_dispositivo`, sem `device_id` (`app/ponto/tests/test_batida_offline.py:49-66,78-88`). Pelo classificador, esse payload fica no canal PWA (`app/ponto/registro_batida.py:61`). Prova pretendida: payload válido cria uma linha, repetição imediata do mesmo payload resulta em uma linha e `ja_registrada`, payload inválido dá 400 e timestamp de 72 horas continua aceito em HTTP (`app/ponto/tests/test_batida_offline.py:68-109`); não prova persistência offline de APK ou PWA (`app/ponto/tests/test_batida_offline.py:49-109`).

- O backend trata o formato epoch esperado do APK e ISO esperado do PWA pela mesma função (`app/api/hora_do_aparelho.py:72-121`); o comentário associa APK a `device_id`, assinatura Keystore e foto em POST separado (`app/ponto/registro_batida.py:53-59`; `app/api/views_core.py:459-488`). Armazenamento local e gatilhos de reenvio do **cliente APK** não foram comprovados por código nativo nesta cópia: [nao verificado].

## 06/11 20:45 · sessao e1ea6b97aab5 · - · - · origem=Ronald (tmux)

- `test_sw_offline_fallback.py` lê o **sw.js real**, executando-o em Node com `self`, rede e cache simulados (`app/core/tests/test_sw_offline_fallback.py:23`, `:25`, `:35`). O harness sempre envia **GET** (`:44`), para navegação, tela de bater e ramo restante (`:62`). Os testes exigem `Response.error()` sem rede/cache (`:79`), conteúdo do cache quando existe (`:89`) e preferência pela rede disponível (`:96`). Assim, não exercitam POST, IndexedDB, clique, persistência ou reenvio de batida (`:37`, `:44`).

- `test_batida_offline.py` usa **Django Client autenticado por sessão**, enviando diretamente `/api/ponto/bater/` (`app/ponto/tests/test_batida_offline.py:60`). O payload contém `timestamp_dispositivo` e não contém `device_id` (`:49`); ele representa o caminho API/PWA, não o cliente APK, cuja identificação deve ser conferida na porta (`:51`). Testa criação (`:68`), dois envios do mesmo timestamp retornando `ja_registrada` e apenas uma linha (`:78`), tipo/payload inválidos (`:91`, `:95`), timestamp antigo aceito (`:104`), colaborador sem posto (`:111`) e disputa (`:119`). Não executa `pwa.js`, não desconecta navegador e não prova recuperação real de fila (`:60`).

- O teste adicional explicitamente chamado PWA também usa Django Client e payload sem `device_id` (`app/api/tests/test_api_bater_pwa.py:42`, `:47`). Exercita foto/disputa, não funcionamento offline (`:64`, `:78`, `:95`).

- O SW **não intercepta POST**: retorna antes do `respondWith` quando o método não é GET (`app/static/sw.js:35`). Seu evento Background Sync apenas manda `SYNC_BATIDAS` para páginas abertas; sem página, mostra notificação pedindo abertura (`app/static/sw.js:112`, `:116`, `:119`, `:122`).

- A fila existe: IndexedDB `juliani-offline`, store `batidas_pendentes` (`app/static/pwa.js:50`, `:54`), gravação transacional em `registrarBatidaOffline` (`:107`, `:112`, `:120`); há chamadas reais nos templates v2 (`app/templates/ponto/bater_ponto_v2.html:356`, `:374`) e clássico (`app/templates/ponto/bater_ponto.html:543`, `:597`). Portanto, os testes limitados não autorizam concluir perda, e a hipótese de inexistência de fila é contrariada pelo código citado (`app/static/pwa.js:107`).

- A **lei local** L-030 determina “batida de chão nunca é barrada em runtime”, escrita pela porta `registro_batida.py`, sem fabricação pelo motor (`CLAUDE.md:140`, `app/docs/LEIS.md:78`). A referência explícita à Portaria 671 encontrada nesses textos trata de exibição da marcação crua (`CLAUDE.md:164`, `app/docs/LEIS.md:76`). Artigo, vigência e enquadramento jurídico externo da Portaria: **[nao verificado]**.

- Caso apareça necessidade de cura, a regra local manda atuar na fonte (`CLAUDE.md:17`); para perda na captura/fila PWA, os sítios de origem são chamadas dos templates e `registrarBatidaOffline`/`sincronizarBatidas`, mantendo a gravação servidor na porta única (`app/templates/ponto/bater_ponto_v2.html:356`, `app/static/pwa.js:107`, `:126`, `app/ponto/registro_batida.py:94`). Nenhuma cura foi executada **[nao verificado por evidência de alteração; somente leitura nesta tarefa]**.

## 06/11 20:44 · sessao 1209bcebfb4b · - · - · origem=Ronald (tmux)

**08 — col617**

- **Estado hoje:** não há atestado pendente na FICHA: #5380 rejeitado; #5399 aprovado, cobrindo 05–06/11, ambas células `trabalha=true / concorde / gerada / ausencia`; chamados #37140 e #37319 resolvidos via sistema. DiaPago não foi fornecido. [nao verificado]
- **Pendente no espelho:** a cobertura para cobrança inclui ausências não rejeitadas; a cobertura para folha exige aprovação (`app/ponto/turnos.py:2509`). O montador separa as duas (`app/ponto/services/espelho.py:582`, `:588`) e entrega a aberta como `_pendente_do_dia` (`:878`). O juiz da palavra mantém o veredito e mostra **“Atestado aguardando decisao”**, amarelo (`app/ponto/services/dia_decidido.py:420`, `:224`, `:57`). Portanto, aparência de pendência e abono são estados distintos no código.
- **Porta:** Ausências → drawer → Aprovar (`app/ponto/urls.py:65`; `app/ponto/views.py:2067`; `app/templates/ponto/partials/painel_ausencia.html:306`, `:309`, `:321`). Exige `editar_ausencias`, POST e ausência aberta (`app/ponto/views.py:2603`); chama `_a4_decidir` (`:2643`), que chega à decisão e escritor de estado (`app/ponto/services/ausencia.py:897`, `:911`, `:824`, `:835`). Documento anexado leva à decisão do DP (`:779`, `:791`); sem documento obrigatório, não há botão Aprovar (`painel_ausencia.html:324`).
- **Espelho após ação:** save de Ausência julga os dias (`app/ponto/signals.py:143`, `:147`, `:93`); cartório escreve ata, veredito e status (`app/ponto/services/cartorio.py:1800`; `app/ponto/portas/celula.py:1030`). Palavra de ausência aprovada sai da cobertura de folha (`app/ponto/services/dia_decidido.py:428`).
- **DiaPago após ação:** **não prometer recálculo imediato pela aprovação.** `decidir` invalida fechamento (`app/ponto/services/ausencia.py:946`), zerando `derivado_em` (`app/ponto/services/fechamento.py:1636`). O próprio código distingue invalidação de recálculo (`:1606`). O recálculo posterior escreve DiaPago por `fechamento.py:1184` → `dia_pago.py:357`.
- **Dono/obra:** aprovação documental pertence ao admin; defeito de coerência de tela, se demonstrado, é ESTRUTURA pela L-099 (`CLAUDE.md:176`). A sonda atual não demonstra pendência nem divergência entre espelho/DiaPago. [nao verificado] A obra DST declara explicitamente dia com ausência lançada como “SEM porta, dia decidido” (`app/docs/BACKLOG.md:1793`), portanto não abrir Resolver dia para substituir aprovação de atestado.
- **Fechamento:** não fechar: falta DiaPago e validação operacional do percurso. [nao verificado]

**09 — col1000, 29–30/10**

- **Estado hoje:** células #142904/#142905: `true / furo / gerada / irregular`, `resolver_dia=false`, `pede_decisao=true`; chamados #37325/#37326 `em_analise`, sem resolução/via; perguntas #54529/#54530 `intervalo_volta`, sem resposta, validação ou materialização, veredito `nao`. Existem três marcações em cada dia: E/S/S, faltando retorno E do intervalo. DiaPago não fornecido. [nao verificado]
- **Porta atual:** chamado local `/chamados/local/<id>/` (`app/chamados/urls.py:14`; `app/chamados/views.py:359`). O cartão oferece Validar somente após resposta (`app/templates/chamados/partials/_pergunta_card.html:29`, `:42`, `:92`); view encaminha à autoridade (`app/chamados/views.py:483`, `:501`). Sem resposta, o serviço recusa `sem_resposta` (`app/chamados/services/validacao.py:363`).
- **Ação operacional necessária hoje:** colaborador responde ao horário na porta de atendimento (`app/chamados/views.py:1228`, `:1283`), writer `resposta_colab/respondida_em` (`app/chamados/services/disputa_emissao.py:1282`); admin valida. Não inventar hora: ela não veio na sonda. [nao verificado]
- **Existe Corrigir:** modal oferece HH:MM e motivo (`app/templates/chamados/partials/modal_fio.html:295`, `:307`), view local também reconhece a ação (`app/chamados/views.py:523`). Porém o formulário está dentro de `if perg.respondida_em` (`modal_fio.html:212`); sem resposta há somente “Aguardando resposta do colaborador” (`:313`). Assim, **não há nessa tela porta de Corrigir oferecida hoje para essas duas perguntas sem resposta**.
- **O que escreve:** Corrigir preserva declaração anterior em followup, grava pela autoridade de resposta e chama validar (`app/chamados/services/acoes_disputa.py:355`, `:366`, `:375`). Materialização de intervalo fixo usa horário do marco, quando disponível, em vez de hora livre (`app/chamados/services/materializacao.py:404`); batida passa por `registrar_batida` (`:901`). Validação agenda recálculo (`app/chamados/services/validacao.py:406`), que recalcula após commit (`app/ponto/services/fechamento.py:1791`, `:1808`) e grava DiaPago (`:1184`; `app/ponto/services/dia_pago.py:357`).
- **Dono:** falta de retorno é candidata BATIDA: juiz formal prioriza contagem ímpar (`app/ponto/management/commands/e6_oraculo.py:62`). Contudo, `folga_nao_definida` e `sem_vinculo_no_dia` precedem essa regra (`:77`, `:85`); a sonda não traz esses insumos do juiz para 29/30, portanto dono formal não medido [nao verificado]. O chamado #35214 de cadastro aberto, sozinho, não prova sua incidência nestes dois dias. [nao verificado]
- **Obra:** complemento O261 para eventual falta de porta admin antes da resposta/resultado verificável; DST não corresponde aos dias `trabalha=true` fornecidos. Encaixe proposto [nao verificado]; DST cobre batida em folga/feriado (`app/docs/BACKLOG.md:1793`).
- **Fechamento:** não fechar: faltam resposta, ação efetiva, DiaPago posterior e validação operacional. [nao verificado]

**Escritores/leitores adicionais**

| Campo utilizado | Escritor | Leitor |
|---|---|---|
| Ausência.status | `app/ponto/services/ausencia.py:808`, `:824` | cobertura `app/ponto/turnos.py:2514`; cabeçalho `app/templates/ponto/partials/painel_ausencia.html:24` |
| Ausência.aprovado_por/aprovada_em | `app/ponto/services/ausencia.py:913` | auditoria `app/templates/ponto/partials/painel_ausencia.html:257` |
| Pergunta.resposta_colab/respondida_em | `app/chamados/services/disputa_emissao.py:1282` | `app/templates/chamados/partials/_pergunta_card.html:29`; guarda `app/chamados/services/validacao.py:363` |
| Pergunta.validada_em | autoridade `app/chamados/services/validacao.py:396` | `app/templates/chamados/partials/_pergunta_card.html:23` |
| Chamado.status_local/resolvido_em/via | `app/chamados/models.py:195`, `:222` | `app/templates/chamados/detalhe_local.html:68`, `:88` |
| Célula.veredito/status | `app/ponto/portas/celula.py:1030` | `app/ponto/status_dia.py:287`; calendário `app/colaboradores/services/calendario.py:606` |
| DiaPago | `app/ponto/services/fechamento.py:1184` → `app/ponto/services/dia_pago.py:357` | `app/ponto/services/espelho.py:406` |

Não executei validação operacional; análise de código somente leitura. [nao verificado] [nao verificado]

## 06/11 20:43 · sessao f1e9774faab9 · - · - · origem=Ronald (tmux)

**10 — col399, 31/10**

- **Estado da sonda:** célula 138663 `trabalha=false / fato_sem_previsao / gerada / folga_trabalhada`, `resolver_dia=false`, sem retenções; batidas #142321 E 06:20:43 e #142798 S 18:22:41 aprovadas e não retratadas. Só consta Ausência #5373 `troca_de_plantao`, aprovada, **data 24/10**, criada **04/11 16:13:01**; não consta par nem documento. “Registrada só em 24/10” deve ser corrigido para “registrada **para** 24/10”; DiaPago não aparece no dado. [nao verificado]
- **Dono:** **ESTRUTURA da porta faltante**, comprovada pelo código e pelo despacho O10-C; dono formal do dado segundo `dono_da_divergencia` não foi medido pela sonda. O juiz exige também vínculo, fase e cadastro×realidade; paridade par sozinha não decide o dono. (`app/docs/RELATO.md:64`; `app/ponto/management/commands/e6_oraculo.py:98`.)
- **PORTA DA TELA NÃO EXISTE para confirmar a troca.** Na cópia, `trocar_plantao` só aparece na própria definição e na chamada interna de `editar_dia`; nenhuma view chama a operação. O despacho registra expressamente “ZERO chamador de UI” e manda construir view+form com substituto, duas cartas e motivo. (`app/ponto/portas/celula.py:632`; `app/ponto/portas/celula.py:675`; `app/docs/RELATO.md:64`.)
- **O que existe:** backend O10-B: cedente recebe `editar_dia(..., False)` e substituto `editar_dia(..., True)`, no mesmo atomic; recusa lado ausente, mesmo colaborador e falta de carta. A edição escreve `trabalha`, DNA, geradora, `origem='editada'`, autor/data e trilha. (`app/ponto/portas/celula.py:603`; `app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:673`.)
- **Resultado previsto, não observado:** reavalia os dias tocados antes de solicitar recálculo por competência; o recálculo chama `dia_pago.lavrar`, que substitui as linhas da versão da competência. Falhas do julgamento/recálculo são capturadas e logadas, portanto a existência desse fio não prova execução bem-sucedida no caso. (`app/ponto/portas/celula.py:422`; `app/ponto/portas/celula.py:445`; `app/ponto/services/fechamento.py:1184`; `app/ponto/services/dia_pago.py:357`.)
- **Resolver dia não substitui a troca:** formulário oferece apenas trabalhou/faltou/justificado/cobrar; “trabalhou” recusa dia cujo `tipo_dia != trabalho`. `folga_trabalhada` está fora dos status que oferecem o botão. (`app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/services/veredito_celula.py:221`; `app/colaboradores/services/calendario.py:94`.)
- **Obra:** complementar **O10-C**, especificamente SOLO → lista do admin com porta de troca aberta, sem escrita antecipada; nenhuma obra nova. **DIA-SEM-TRABALHO-PORTA** é relacionada à decisão sobre batidas em folga, mas não substitui a confirmação do par de troca. (`app/docs/BACKLOG.md:1811`; `app/docs/BACKLOG.md:1793`.)
- **Fechamento:** não resolvido; faltam porta UI e validação operacional do espelho/DiaPago após a confirmação. (`app/docs/RELATO.md:64`; resultado operacional [nao verificado].)

**11/12 — col921, competência 11**

- **Estado da sonda:** `batidas=[]`; de 21–23 e 25–29/10: `true / furo / gerada / irregular`; 30/10 e 01–05/11: `true / cobrado / gerada / irregular`; 06/11 `true / cobrado / gerada / previsto`; folgas 24 e 31/10 `false / concorde / gerada / folga`. Nenhum desses dias oferece Resolver dia; DiaPago não foi fornecido. Há chamado de cadastro #37165 `sem_previsao_escala / folga_nao_definida / aberto`, chamados de batida abertos e perguntas sem resposta/materialização. [nao verificado]
- **Não é o grupo DST “folga_trabalhada sem Resolver dia”.** O quadro DST documenta 161 dias com batidas e status `folga_trabalhada`; col921 não tem esse status no dado fornecido. O seu bloqueio do Resolver dia decorre da competência corrente, mesmo nos dias `irregular`, porque `pode_ultimo_recurso` exige data anterior ao início da competência atual. (`app/docs/RELATO.md:38`; `app/colaboradores/services/calendario.py:455`; `app/colaboradores/services/calendario.py:738`; dados col921 [nao verificado].)
- **Dono:** há pendência operacional de **CADASTRO** expressa pelo chamado #37165; confirmar que isso é o insumo `folga_nao_definida=True` do juiz e o dono formal de cada dia permanece [nao verificado]. O juiz dá CADASTRO quando falta definição de folga/vínculo; ausência total de batidas, sozinha, não aciona seu ramo BATIDA por paridade ímpar. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:106`; chamado [nao verificado].)
- **Porta factual existente para admin confirmar falta/ausência:** `/ponto/ausencias/lancar/`, view `lancar_ausencia`, formulário de colaborador/tipo/data/duração. Cria Ausência já aprovada, registrador/aprovador = admin. Isso é diferente de fingir disponibilidade do botão Resolver dia. (`app/ponto/urls.py:57`; `app/ponto/views.py:2368`; `app/ponto/views.py:2415`; `app/templates/ponto/lancar_ausencia.html:12`; `app/templates/ponto/lancar_ausencia.html:26`; `app/templates/ponto/lancar_ausencia.html:88`.)
- **Limite:** só lançar falta depois de confirmar que era dia de trabalho; a autoridade veta falta quando a escala do dia responde False. A falta decidida vira status `falta`; ausência aprovada sem batida vira `ausencia`; isso não prova o resultado operacional de col921. (`app/ponto/precedencia.py:536`; `app/ponto/status_dia.py:139`; `app/ponto/status_dia.py:141`.)
- **Obra:** não cabe atribuir col921 ao complemento DST apenas pela ausência do botão. Para dado de escala/falta, execução administrativa das portas existentes; eventual defeito do fio/porta precisa do diagnóstico de cada pergunta, não do rótulo genérico “já OK”. O escopo DST é batida em folga/feriado sem trabalho; ausência lançada é dia decidido. (`app/docs/BACKLOG.md:1793`; `app/docs/RELATO.md:51`.)
- **Fechamento:** não resolvidos; a sonda não demonstra ação na tela nem DiaPago posterior nem validação operacional. [nao verificado]

**Escritores/leitores dos campos usados**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha`, `origem`, DNA e edição humana | porta `editar_dia`, `app/ponto/portas/celula.py:603`; gerar/regenerar são operações do mesmo módulo, `:74` | estado da edição/trilha `:528`; juiz de status e grade, `app/ponto/status_dia.py:272`; “dia de trabalho” é autoridade `EscalaColaborador.eh_dia_trabalho` conforme `CLAUDE.md:152` |
| `veredito`, `veredito_via`, `veredito_em`, `ata` | `lavrar_veredito`, `app/ponto/portas/celula.py:969`, grava `:1028` | regra do status `app/ponto/status_dia.py:113`; resolução humana chama a porta `app/ponto/services/veredito_celula.py:330` |
| `status_dia`, `status_rotulo` | `_aplicar_status` dentro da lavra `app/ponto/portas/celula.py:961`; decisão pura `app/ponto/status_dia.py:113` | D4 lê coluna `app/ponto/status_dia.py:287`; calendário lê D4 `app/colaboradores/services/calendario.py:662`; template desenha `app/templates/colaboradores/partials/_calendario_grade.html:101` |
| `resolver_dia_oferecido` | derivado, não persistido: `app/colaboradores/services/calendario.py:97`; insumo `pode_ultimo_recurso` `:738` | template `app/templates/colaboradores/partials/_calendario_grade.html:125`; vigia lê a mesma resposta pela plural `app/colaboradores/services/calendario.py:117` |
| rubricas `DiaPago` | `dia_pago.lavrar` chamado pelo fechamento `app/ponto/services/fechamento.py:1184`, escrita `app/ponto/services/dia_pago.py:357` | `rubricas_do_dia` `app/ponto/services/dia_pago.py:899`; calendário `app/colaboradores/services/calendario.py:655` |
| `Ausencia.status='aprovada'` por lançamento do admin | view passa a criação ao serviço `app/ponto/views.py:2415` | natureza/status do dia lê efeito/cobertura pela A2 `app/ponto/status_dia.py:298` e `:310` |
| chamado vivo/status exibido no calendário | escritor específico deve vir do outro levantamento [nao verificado] | indexador lê `status_local` `app/colaboradores/services/calendario.py:437` e `:444` |

A validação de dono formal não é possível só com os campos da FICHA: os insumos adicionais do juiz são carregados por `InsumosDoDono`, incluindo cadastro×realidade e fase por dia. (`app/ponto/management/commands/e6_oraculo.py:113`; `app/ponto/management/commands/e6_oraculo.py:154`; `app/ponto/management/commands/e6_oraculo.py:164`.)

## 06/11 20:43 · sessao 0ad1cebba9e1 · - · - · origem=Ronald (tmux)

Evidências de 07:

- As perguntas #43643/#44051 já tinham `materializada_em`, via `cadastro_folga`, antes da resposta e da validação; célula `true/indefinida/gerada/escala_suspeita`, sem Resolver dia, DiaPago 0 somente no enunciado. Tudo é dado fornecido `[nao verificado]`.
- **Guarda atual que impede nova materialização:** `validar_pergunta` chama a unidade somente se `materializada_em is None` (`app/chamados/services/validacao.py:422`); o laço seleciona somente `materializada_em__isnull=True` (`app/chamados/services/materializacao.py:1344`). A devolução RECUSA-FALA também retorna sem agir quando já existe materialização (`app/chamados/services/validacao.py:182`).
- A guarda que escreveu **historicamente** a recusa de 192 caracteres não pode ser identificada pelo comprimento `[nao verificado]`. Não afirmar paridade, geometria ou falta de marco.
- Nova validação individual retorna `ja_validada` (`app/chamados/services/validacao.py:366`); a tela esconde seu botão em pergunta validada (`app/templates/chamados/partials/_pergunta_card.html:42`). Rota de detalhe chama a validação em `app/chamados/views.py:483`; formulário em `app/templates/chamados/partials/_pergunta_card.html:92`.
- `cadastro_folga` significa agrupamento por LIMBO-FOLGAS (`app/chamados/models.py:2265`); a seleção da via exige filho agrupado, sem batida e pai de cadastro `sem_previsao_escala/folga_nao_definida` (`app/chamados/juizes.py:1463`). O problema de cadastro e o beco estrutural precisam ser separados: cadastro não prova sozinho a causa da recusa histórica `[nao verificado]`.
- A classe **“carimbo diz encerrada, validação existe, célula ainda acusa, nenhuma ação reprocessa” cabe como complemento O261**, cujo escopo inclui validada sem plantio e escritores do carimbo (`app/docs/BACKLOG.md:1450`). Não está resolvida pelo dado `[nao verificado]`.

Evidências de 03:

- Porta `/chamados/validar-lote/tela/` em `app/chamados/urls.py:37`; view síncrona chama serviço antes de renderizar (`app/chamados/views_cobrar.py:372`, `:381`). Formulário HTMX em `app/templates/chamados/partials/_lote_preview.html:21`; resultado detalha referências e motivos em `:7-8`.
- `gthread`, quatro threads e `timeout=30` estão em `app/gunicorn.conf.py:19-21`. A documentação da própria cópia registra expressamente que esse timeout não limita uma requisição longa; trata worker travado (`app/docs/TICKETS.md:132`). Caddy não declara timeout específico nessa rota (`Caddyfile:8-18`). Os POST anexados terminam 200; falha por timeout, cancelamento no navegador ou queda de worker não veio demonstrada `[nao verificado]`.
- Obra existente exata: **LOTE-VALIDAR-VIRA-JOB**, aberta, com o mesmo endpoint e finalidade (`app/docs/BACKLOG.md:1806`). Dono ESTRUTURA: trabalho longo está na requisição, enquanto a orientação manda job (`CLAUDE.md:89`).

**Recusas do lote — ramos possíveis, contagem por ramo desconhecida `[nao verificado]`:**

| Motivo | Ramo |
|---|---|
| Referência não encontrada | `app/chamados/services/validacao.py:682-684` |
| Já validada | `app/chamados/services/validacao.py:685-687` |
| Saiu da classe A | `app/chamados/services/validacao.py:688-690` |
| Escape/resposta de posto/confirmação sem marco | `app/chamados/services/validacao.py:807-815` |
| Opção que exige conversa | `app/chamados/services/validacao.py:816` |
| Sem hora legível/tipo inadequado | `app/chamados/services/validacao.py:817-818` |
| Sem marco esperado | `app/chamados/services/validacao.py:820-821` |
| AM/PM ou além da tolerância da folha | `app/chamados/services/validacao.py:822-823` |
| Batida apurável a até 60 minutos | `app/chamados/services/validacao.py:827-829` |
| Validação individual recusou e não ficou carimbo | `app/chamados/services/validacao.py:712-714` |

As **68 RECUSA03** são registros com motivo preenchido, não as referências dos 40 recusados de um POST `[nao verificado]`. Seu agrupamento descritivo por **via atual**, contado no texto fornecido, é: `admin_decidiu=48`, `declaracao=8`, `paridade_recusada=7`, `ausencia_dia=3`, `fato_ocupado=1`, via vazia/foto_ausente=1; soma 68 `[nao verificado]`. Isso **não identifica motivos causais nem contagens por guarda**. O dado não trouxe `refs` do POST, retorno `recusados[].motivo` nem texto `recusa_motivo` `[nao verificado]`.

O campo `motivo` fornecido em RECUSA03 é o motivo da **pergunta** (`orfao_14h`, intervalo etc.), não a razão da recusa; o serviço devolve esta separadamente como `recusados[].motivo` (`app/chamados/services/validacao.py:683`, `:689`, `:714`). Motivo antigo pode coexistir com posterior materialização: a validação não o limpa e a porta só o altera ao receber nova recusa (`app/chamados/services/validacao.py:396`, `app/chamados/services/materializacao.py:1064-1078`).

Escritores/leitores úteis:

- `validada_em/por`: decisão em `_carimbar_validacao` (`app/chamados/services/validacao.py:127-133`); devolução em `:182-190`; leitura na porta em `:366`, no lote em `:685` e na tela em `app/templates/chamados/partials/_pergunta_card.html:23`. Outros carimbos de simetria e reabertura estão declarados no censo do módulo (`app/chamados/services/validacao.py:19-26`).
- `materializada_em/por/via_resolucao`: atribuição conjunta em `PerguntaDisputa._escrever_carimbo` (`app/chamados/models.py:2556-2560`), portas `carimbar/carimbar_identidade/descarimbar` descritas em `:2532-2543`; leitura de estado em `:2499-2505`, guarda de plantio em `app/chamados/services/validacao.py:422`.
- `recusa_motivo`: `_carimbar_recusa` em `app/chamados/services/materializacao.py:1058-1078`; também recusa administrativa em `app/chamados/services/validacao.py:545-547`. Não é campo com escritor único, e o censo declara cinco escritores (`app/chamados/services/validacao.py:42-45`). Leitor para orientação: `app/chamados/services/explicador.py:85-103`.
- `pendencia_admin`: plantio escreve junto ao motivo (`app/chamados/services/materializacao.py:825-826`); explicador lê antes de concluir resolvida (`app/chamados/services/explicador.py:85`).
- `resposta_colab/respondida_em`: `responder_pergunta_disputa_core` (`app/chamados/services/disputa_emissao.py:1247`, `:1282-1283`); validação lê em `app/chamados/services/validacao.py:363`, classificação do lote em `:806-818`.
- Batida resultante: `registrar_batida` chamado em `app/chamados/services/materializacao.py:901-908`; só então passa pelo juiz de lastro e carimbo (`:1207-1256`). Geometria/paridade podem recusar e desfazer a escrita, portanto existência da função não prova resultado operacional (`:1209-1218`).
- DiaPago: validação agenda `recalcular_por_evento` (`app/chamados/services/validacao.py:405-408`), executado após commit (`app/ponto/services/fechamento.py:1791-1808`); recalculo chama `ponto.services.dia_pago.lavrar` em `app/ponto/services/fechamento.py:1184-1221`. Resultado posterior específico e validação operacional não fornecidos `[nao verificado]`.

Outra inconsistência visível: texto fixo do preview promete **30 min** (`app/templates/chamados/partials/_lote_preview.html:14`), mas o envelope real lê a tolerância da folha (`app/chamados/services/validacao.py:749-752`), com comentário registrando redução a 10 (`:731-738`). Pode entrar como complemento da mesma obra para alinhar explicação e comportamento; não explica a quantidade de 40 sem as referências `[nao verificado]`.

## 06/11 20:37 · sessao b86b0bc41185 · L-123-MEDIDA-EM-PY · verificador · origem=Code

**Parecer: reprova a volta 2. Ainda passam mudanças de comportamento pelo juiz de `padrao`: há uma brecha de linha compartilhada e há decisões dentro dos contêineres declarados como dado.** (`bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`, `app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`)

1. **Caminhos de aceite e recusa**

O contraexemplo mais direto é alterar o fechamento de `RESULTADOS`, na linha 624, de `]` para `]; CADENCIA_TERMOMETRO_DIAS = 700`. O `Assign` de `RESULTADOS` torna a linha livre; a segunda atribuição torna a mesma linha presa; o retorno conserva a linha porque não subtrai `preso` de `livre`. A conferência dos hunks aceita essa linha nos dois lados. (`app/core/placar_estrutural.py:624`, `bin/tests/test_lei_akita.sh:200`, `bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:230`)

**Confirmei em memória**, usando as funções reais: a linha 624 permanece livre antes/depois; com `hoje=2026-11-06` e R1 medido em `2026-10-01`, o original retorna AMARELO e o alterado retorna VERDE. O vencimento consulta precisamente essa constante. (`bin/tests/test_lei_akita.sh:194`, `app/core/placar_estrutural.py:668`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:694`)

Outro caminho dispensa instrução executável: trocar o `papel` do R1 de `TERMOMETRO` para `PRINCIPAL`. Ambos são constantes-string elegíveis; a alteração mantém dado puro, retira R1 dos termômetros e permite chegar ao VERDE sem termômetro. Também confirmei AMARELO → VERDE em memória no cenário acima. (`app/core/placar_estrutural.py:61`, `app/core/placar_estrutural.py:121`, `bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:203`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:694`)

Na tabela, “aceita” significa concessão da exceção pelo juiz de `padrao`, satisfeitas as demais condições do arquivo. **Não significa régua inteira verde**: os testes L114 exigem R1 como único termômetro e verificam a borda de sete dias e as cores. (`bin/tests/test_lei_akita.sh:244`, `app/core/tests/test_l114_placar_papel_e_termometro.py:34`, `app/core/tests/test_l114_placar_papel_e_termometro.py:51`, `app/core/tests/test_l114_placar_papel_e_termometro.py:71`)

| Caso | Resultado |
|---|---|
| Constante-string com outro `Assign`, `AnnAssign` ou `AugAssign`, inclusive dentro de função com `global` | **Recusa o uso desse Name como dado puro**: a contagem deixa de ser um. O percurso alcança qualquer escopo. (`bin/tests/test_lei_akita.sh:151`, `bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:185`) |
| Constante-string também ligada por `for`, `with … as`, `import … as` ou walrus; ou removida por `del` | **Aceita indevidamente o Name como constante**, quando essas instruções já existem e só o contêiner muda: `_atribuicoes` não conta esses nós. Isso também afeta a suposta unicidade de `PAPEL_DO_MODULO` e `DADO_MEDIDO`. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:185`) |
| Acrescentar essas instruções em linhas próprias no commit julgado | **Recusa**: esses nós não são docstring nem `Assign` elegível. A linha compartilhada descrita acima continua sendo exceção indevida. (`bin/tests/test_lei_akita.sh:201`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`) |
| `R = X = [...]`, mesmo com ambos os nomes declarados | **Recusa**: exige exatamente um alvo. (`bin/tests/test_lei_akita.sh:202`) |
| `R[...] = ...` ou `R.attr = ...` em linha própria | **Recusa**: exige alvo `Name`. (`bin/tests/test_lei_akita.sh:202`) |
| Dict com chamada, lambda ou Name sem ser constante-string | **Recusa**: esses nós não passam em `_dado_puro`. (`bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:192`) |
| `AnnAssign` executável em linha própria | **Recusa**: somente `Assign` pode ser dado livre. (`bin/tests/test_lei_akita.sh:202`) |
| Chamada, import ou monkeypatch depois de `;` na linha de dado | **Aceita indevidamente**, pela sobreposição entre `livre` e `preso`. (`bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`) |
| Alterar `papel`, `estado`, `prova`, `id` ou `fonte_do_medido` dentro de `RESULTADOS` | **Aceita**, conservada a pureza sintática. Esses campos controlam classificação, rebaixamento, contagem e obtenção da data. (`bin/tests/test_lei_akita.sh:203`, `app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:650`, `app/core/placar_estrutural.py:700`, `bin/gerar_estado.py:47`, `bin/gerar_estado.py:56`) |
| Alterar `estado`/`prova` em `ETAPAS` | **Aceita**; pode alterar estado efetivo, feitos e percentual. (`app/core/espelho_verdade.py:209`, `app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`, `bin/tests/test_lei_akita.sh:203`) |
| Mudar ou remover chave de dict | **Aceita**, se continuar puro. Não há validação de esquema; leitores acessam diretamente chaves como `id`, `estado` e `itens`, podendo mudar resultado ou levantar exceção. (`bin/tests/test_lei_akita.sh:188`, `app/core/placar_estrutural.py:650`, `app/core/espelho_verdade.py:208`, `app/core/espelho_verdade.py:209`) |
| String contendo código | **Aceita como string**. Nas rotas rastreadas, a publicação usa templates fixos com `%`; interpretação por `eval` ou por template fornecido pelo dado: **[não verificado]**. (`bin/tests/test_lei_akita.sh:188`, `bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`, `bin/gerar_estado.py:163`) |

Uso desses contêineres para decidir **dinheiro em produção**: **[não verificado]**. O uso para decidir cor, vencimento e classificação do placar está demonstrado nos leitores acima. (`app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:679`)

2. **Nomes lidos no ALVO**

A autorização vem do ALVO; a pureza e os intervalos vêm separadamente do pai e do commit julgado. Portanto, a retroatividade não transforma diretamente uma chamada dentro do valor em dado puro: esse valor continua sendo examinado na revisão histórica. (`bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:221`, `bin/tests/test_lei_akita.sh:223`)

**A garantia semântica, porém, é maior que a prova disponível.** O ALVO fornece nomes, sem comprovar que eles tinham função de medição no commit antigo. Não há restrição que impeça declarar uma constante de decisão como nome elegível; a validação exige apenas uma tupla não vazia de strings, seguida do teste sintático do `Assign`. Assim, um ALVO que liste `CADENCIA_TERMOMETRO_DIAS` pode autorizar retroativamente sua troca literal. Esse cenário decorre do predicado; sua ocorrência histórica nesta obra é **[não verificado]**. (`bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:668`)

As falhas de contagem de vínculos também permitem que a declaração sintática encontrada pelo selo divirja do valor executado no ALVO. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:163`)

Merge continua filtrando caminhos iguais a pais publicados; os caminhos restantes são comparados com o primeiro pai e julgados dos dois lados. Rename usa `--no-renames`; origem ausente no ALVO recusa. Restauração posterior pode habilitar uma exclusão histórica composta só de linhas livres, conforme a retroatividade estabelecida. (`bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:230`, `bin/tests/test_lei_akita.sh:296`, `bin/tests/test_lei_akita.sh:303`, `bin/tests/test_lei_akita.sh:310`)

3. **O MORDE não morde cada cláusula nova**

O mapeamento abaixo é **por inspeção dos casos**, não resultado de executar versões mutantes. Os casos chamam as funções reais e o shell rejeita qualquer `MORDE_FALHOU`. (`bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:569`)

| Caso existente | Mutante que distingue |
|---|---|
| `so_numero`, `estado`, `comentario`, `doc` | Desligar a concessão; proibir constante-string; retirar liberação de comentário/docstring. (`bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:406`) |
| `na_def` | Liberar corpo de função. (`bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:407`) |
| `tira_def` | Examinar somente o lado novo. (`bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:407`) |
| `com_import`, `com_lambda`, `com_chamada`, `com_nome` | Liberar, respectivamente, import, lambda, chamada ou Name não constante-string. (`bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:408`) |
| `decisao` | Liberar atribuição fora dos nomes declarados. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:409`) |
| `monkey` | Liberar atribuição a atributo em linha própria. (`bin/tests/test_lei_akita.sh:399`, `bin/tests/test_lei_akita.sh:409`) |
| `sem_decl`, `mais_fech`, rótulo instrumento | Dispensar papel `medida`, ignorar outro arquivo de negócio ou estender a exceção a instrumento. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:411`) |
| ALVO sem declaração | Ler declaração exclusivamente em `h`. (`bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:418`) |
| Retroativo número/decisão | Exigir declaração em `h`, ou liberar decisão fora do contêiner. (`bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:431`) |

**`redecl` não prova a unicidade no ALVO.** O caso acrescenta outra declaração em `h`, mas o preparo restaura HEAD à base antes de julgá-lo. Ele já reprova pela linha presa; retirar a checagem de unicidade no ALVO não é distinguido por esse caso. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`)

Faltam casos para: vínculos por `for`/`with`/import/walrus/`del`; reatribuição de constante-string; duplicação de ambos os metadados no ALVO; múltiplos alvos; Subscript; AnnAssign; instrução presa na mesma linha livre; DADO inválido/aninhado; classe/decorador; comentário dentro de função; docstring posterior; diff sem hunk; e dado puro usado como decisão. A matriz atual não planta esses cenários. (`bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:412`)

Os testes antigos cobrem merge/rename com `n/a (instrumento)`, mas não a elegibilidade AST de medida nesses cenários. Os casos novos de medida passam mensagem e arquivos manualmente; seus commits usam mensagem `c`, sem exercitar essa integração pelo universo. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:480`, `bin/tests/test_lei_akita.sh:513`, `bin/tests/test_lei_akita.sh:525`)

Há ainda divergência no limite de erro: `show`/`diff` emitem `PADRAO_ERRO`, mas qualquer erro de `cat-file` vira AST ausente e qualquer erro da sonda de pai vira `False`. Recusam a concessão, mas não cumprem literalmente “erro Git = PADRAO_ERRO”. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:219`)

Execução integral do MORDE e dos dois mutantes vermelhos prometidos: **[não verificado]**. Não executei o shell, cujo preparo cria arquivos e repositórios temporários. (`bin/tests/test_lei_akita.sh:362`, `bin/tests/test_lei_akita.sh:374`, `bin/tests/test_lei_akita.sh:376`)

4. **Os três R1**

`36c90132`, `6ed6adbe` e `a16637ca` tocarem somente linhas dentro de `RESULTADOS`/`ETAPAS`: **[não verificado]**. O histórico Git e os diffs individuais não estão disponíveis nesta cópia **[não verificado]**.

O relato cita as origens, um comparativo de três arquivos e as correções, mas não apresenta os hunks desses commits. Isso não sustenta responder “sim”. (`app/docs/RELATO.md:864`, `app/docs/RELATO.md:866`, `app/docs/RELATO.md:869`)

5. **Quem escreve, quem lê e ESMERIL-SEMÂNTICA**

| Símbolo | Escritores/declarações | Leitores |
|---|---|---|
| `PAPEL_DO_MODULO` | Dois módulos; fixture e alterações do MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:429`) | `_dado_medido`, por AST do ALVO. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:209`) |
| `DADO_MEDIDO` | Dois módulos e fixture MORDE. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:370`) | `_dado_medido` extrai nomes; `_linhas_livres` usa-os para liberar atribuições. (`bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:203`) |
| `RESULTADOS` | Definição de topo; teste L114 reatribui e restaura. Responsabilidade declarada: “quem mede”. (`app/core/placar_estrutural.py:28`, `app/core/placar_estrutural.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:117`, `app/core/tests/test_l114_placar_papel_e_termometro.py:123`) | `placar`; selo de afirmações fora do ar; testes L114, R1 e R6. (`app/core/placar_estrutural.py:644`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:83`, `app/core/tests/test_l114_placar_papel_e_termometro.py:96`, `app/core/tests/test_r1_termometro_uma_medicao.py:41`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`) |
| `ETAPAS` | Definição de topo. Responsabilidade declarada: “quem fecha a fatia”. (`app/core/espelho_verdade.py:19`, `app/core/espelho_verdade.py:35`) | `placar` e teste de costura R1. (`app/core/espelho_verdade.py:206`, `app/core/tests/test_r1_termometro_uma_medicao.py:45`) |

`gerar_estado` executa os módulos, chama as funções e publica suas projeções. O handoff também lê o texto bruto de `placar_estrutural.py` por regex. (`bin/gerar_estado.py:77`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:144`, `bin/gerar_estado.py:145`, `bin/handoff_sessao.sh:179`)

**“Medida/dado” não mantém o mesmo alcance.** Nos comentários, promete número medido e prova; no selo, significa expressão sintaticamente pura dentro de nome autorizado; nos leitores, inclui parâmetros de classificação, descoberta da data, rebaixamento e percentual. `fonte_do_medido`, por exemplo, é um glob operacional, não apenas texto publicado. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:203`, `bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `app/core/espelho_verdade.py:211`)

6. **Classes de reprovação**

- **Segundo juiz:** o fluxo analisado conserva `julgar` e usa um predicado de elegibilidade no mesmo caminho; não demonstra um segundo juiz de negócio. (`bin/tests/test_lei_akita.sh:253`, `bin/tests/test_lei_akita.sh:311`)
- **Derivação paralela:** o selo examina sintaxe e linhas; não introduz nesse fluxo outro cálculo monetário ou de vencimento. Uma duplicação fora desse escopo é **[não verificado]**. (`bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:207`)
- **Selo sem mordida:** há mordida para os casos plantados, mas falta discriminar as brechas demonstradas e a unicidade no ALVO. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:414`)
- **Campo com dois significados / ESMERIL-SEMÂNTICA: reprova.** A autorização chamada “dado medido” alcança conteúdo que decide comportamento, e a classificação por linha ainda libera instrução executável colada ao dado. (`bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:206`, `app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`)

## 06/11 20:37 · sessao 325c532adcd0 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**A V4 corrige a reprovação estática do selo:** a avaliação em memória encontrou `novos=[]` e `curados=[]`, satisfazendo os dois testes perguntados (`app/core/tests/test_selo_teste_sem_relogio.py:67`, `:72`). A garantia de execução dos hooks congelados e dos REDs contra os pais permanece **[nao verificado]**.

1. **Selo: os dois ficam verdes na avaliação estática.**

   | Arquivo de teste | `le_relogio_solto` | Em `soltos()` | Em `lista()` | [nao verificado]
   |---|---:|---|---|
   | API original | 11 | Sim | Sim |
   | API novo | 0 | Não | Não |
   | A2 original | 2 | Sim | Sim |
   | A2 novo | 0 | Não | Não |

   Os valores foram calculados pelas funções reais do selo, em memória. O detector conta chamadas por AST e considera qualquer ocorrência de `freeze_time` suficiente para retornar zero (`app/core/tests/test_selo_teste_sem_relogio.py:28`, `:32`, `:58`, `:62`). Os originais continuam lendo relógio e listados; os novos têm decoração de classe (`app/api/tests/test_api_bater_ponto.py:222`; `app/chamados/tests/test_a2_data_evento_batida.py:31`, `:48`; `app/core/tests/testes_sem_relogio.txt:5`, `:26`; `app/api/tests/test_api_bater_sse_sem_dia.py:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`).

   Na cópia inteira, `soltos()` e `lista()` têm as mesmas 458 entradas. Os três métodos do selo passaram usando `unittest.TestCase` como substituto da base Django, sem executar Django ou acessar banco (`app/core/tests/test_selo_teste_sem_relogio.py:66`). Igualdade byte a byte com a lista de `8050df3f`: **[nao verificado]**.

2. **Congelamento: a posição agora é adequada; cobertura efetiva dos hooks não foi comprovada aqui.**

   O decorador envolve a classe que contém `setUpTestData` na API e a classe que contém `setUp` no A2, além dos respectivos corpos (`app/api/tests/test_api_bater_sse_sem_dia.py:19`, `:22`, `:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:19`, `:25`).

   O comportamento esperado do freezegun para `unittest.TestCase` é cobrir `setUpClass` — incluindo `setUpTestData` —, `setUp` e corpo; essa implementação das dependências não está disponível na cópia: **[nao verificado]**. As versões estão fixadas em Django 5.2.14 e freezegun 1.5.5 (`app/requirements.txt:13`, `:43`).

   Os campos automáticos relevantes chamam-se `criado_em` e usam `auto_now_add`; receberem efetivamente o instante congelado depende daquela cobertura **[nao verificado]** (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`). “Arquivo inteiro congelado” também não significa importação congelada: os imports antecedem a decoração (`app/api/tests/test_api_bater_sse_sem_dia.py:9`, `:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:8`, `:17`).

3. **Mordida preservada dedutivamente; fixtures equivalentes, mas não literalmente iguais.**

   **API:** mantém patch retornando `None`, URL `/api/ponto/bater/`, S três horas retroativa e os três asserts (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:40`, `:41`, `:45`). O patch alcança os imports locais dos dois gêmeos; o código atual trata `None` antes de serializar (`app/api/views.py:776`, `:778`; `app/api/views_core.py:776`, `:778`). Contra o pai descrito com `None.isoformat()`, a mordida continua dedutiva; objeto e execução contra `39324b76`: **[nao verificado]**. O teste simula a resposta `None`, sem construir uma retratação real (`app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:37`).

   **A2:** conserva ID sem linha, timestamp de agosto e os asserts que exigem outro dia, igual ao dia corrente congelado (`app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:28`, `:32`). Hoje a busca exige PK e colaborador; sem linha, o juiz retorna `None`, permitindo a conversa datada por `localdate()` (`app/chamados/services/disputa_emissao.py:379`, `:417`; `app/ponto/turnos.py:2921`; `app/chamados/models.py:2426`). Um pai que derive diretamente do timestamp falharia no primeiro assert; implementação e execução contra `8050df3f`: **[nao verificado]**. A2 chama o helper diretamente, sem patch nem URL (`app/chamados/tests/test_a2_batida_sem_linha.py:30`).

   Os rótulos mudaram e o username API também mudou na cópia; os campos funcionais das fixtures foram preservados (`app/api/tests/test_api_bater_ponto.py:199`, `:206`; `app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:26`; `app/chamados/tests/test_a2_data_evento_batida.py:11`; `app/chamados/tests/test_a2_batida_sem_linha.py:19`). A repetição entre classes não deve colidir sob o isolamento esperado de `TestCase`; ambas usam essa base, mas seu rollback efetivo aqui é **[nao verificado]** (`app/api/tests/test_api_bater_sse_sem_dia.py:20`; `app/chamados/tests/test_a2_batida_sem_linha.py:18`).

4. **Não identifiquei contrato que quebre pelos novos arquivos ou nomes de classe.**

   O runner delega a descoberta ao `DiscoverRunner`; o CRON-15 só recolhe métodos com sua marca; o smoke de portas agrega URLs dos arquivos de teste (`app/core/runner_quarentena.py:72`, `:76`; `app/core/tests/test_marca_cron15.py:37`, `:41`; `app/core/tests/test_contract_smoke_porta.py:48`). Em memória, o censo CRON-15 continuou exatamente 15/15 e o contrato de smoke passou (`app/core/tests/test_marca_cron15.py:48`; `app/core/tests/test_contract_smoke_porta.py:38`). O contrato de imports também varre testes, mas verifica nomes importados, sem exigir as classes antigas (`app/core/tests/test_contract_import_local_resolve.py:62`, `:71`, `:81`). Suíte completa: **[nao verificado]**.

5. **Classes que já reprovaram a obra.**

   - **Selo/RED sem mordida:** a falsa “cura” dos arquivos antigos foi eliminada; a mordida permanece dedutiva. O RED API ainda não exige que o mock tenha sido chamado (`app/core/tests/test_selo_teste_sem_relogio.py:34`, `:73`; `app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:45`).
   - **Relógio fora da borda:** as fixtures agora estão dentro das classes decoradas; os arquivos antigos continuam explicitamente soltos e listados. A cobertura efetiva dos hooks permanece **[nao verificado]** (`app/api/tests/test_api_bater_sse_sem_dia.py:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`; `app/core/tests/testes_sem_relogio.txt:5`, `:26`).
   - **Paridade dos gêmeos:** o contrato AST passou em memória; isso comprova igualdade estrutural, sem substituir duas execuções comportamentais (`app/api/tests/test_contract_paridade_kernels.py:26`, `:34`).

## 06/11 20:35 · sessao ce334d3b3633 · - · - · origem=Ronald (tmux)

- **API: mordida dedutiva preservada.** Patch, POST, tipo S retroativo três horas e asserts foram mantidos (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:41`, `:45`). O atraso cabe na janela de 48 h (`app/api/hora_do_aparelho.py:63`, `:141`) e evita a correção ao vivo de 300 s (`app/api/views_core.py:592`). Sem vínculo na fixture, a correção retroativa não entra (`app/api/tests/test_api_bater_sse_sem_dia.py:23`; `app/api/views_core.py:611`). O fallback sem batidas dá motivo (`app/escala/servico_jornada.py:469`, `:471`, `:545`), permitindo criar SSE (`app/api/views_core.py:649`). O patch alcança os imports locais nos dois gêmeos (`app/api/views.py:776`; `app/api/views_core.py:776`). Atual trata `None`; o pai descrito, com `None.isoformat()`, seria RED (`app/api/views_core.py:778`; `app/api/tests/test_api_bater_sse_sem_dia.py:36`). Objeto Git e execução contra `39324b76`: **[nao verificado]**.

- **A2: a mudança de arquivo preserva o contraexemplo.** Continua fornecendo ID sem linha, timestamp de agosto e exigindo data diferente de agosto e igual ao dia congelado de outubro (`app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:28`, `:32`). Atual busca a linha por PK e colaborador; sem linha, o helper retorna `None` (`app/chamados/services/disputa_emissao.py:379`; `app/ponto/turnos.py:2921`). O motivo é de escolha (`app/chamados/catalogo/perguntas.py:82`), permitindo `abrir_conversa`, que grava `localdate()` (`app/chamados/services/disputa_emissao.py:410`, `:417`; `app/chamados/models.py:2426`). Se o pai deriva diretamente do timestamp, falha no primeiro assert; implementação do pai e execução contra `8050df3f`: **[nao verificado]**.

- **Fixtures equivalentes funcionalmente, não literalmente iguais.** A API mudou rótulos e username, mantendo campos funcionais; CPF/CNPJ repetem (`app/api/tests/test_api_bater_ponto.py:199`, `:206`, `:207`; `app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:26`, `:27`). A2 mudou rótulos, mantendo os demais parâmetros (`app/chamados/tests/test_a2_data_evento_batida.py:12`; `app/chamados/tests/test_a2_batida_sem_linha.py:20`). Ambos herdam `TestCase`; garantia de isolamento depende da implementação Django indisponível, portanto **[nao verificado]** além da declaração de herança (`app/api/tests/test_api_bater_sse_sem_dia.py:20`; `app/chamados/tests/test_a2_batida_sem_linha.py:18`).

- **Limite do RED API:** não há `assert_called` no patch; a passagem efetiva pela guarda é deduzida do fluxo, não medida pelo teste (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:45`). O teste simula retorno `None`, sem construir uma batida retratada real (`app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:37`).

## 06/11 20:35 · sessao fa77ffc8e719 · - · - · origem=Ronald (tmux)

Achado principal: **o selo aceita alteração de decisão dentro de `RESULTADOS`**. Trocar somente `'papel': TERMOMETRO` por `PRINCIPAL` no R1 conserva dado puro, mas remove o único termômetro da seleção e pode trocar AMARELO por VERDE. (`app/core/placar_estrutural.py:121`, `app/core/placar_estrutural.py:61`, `bin/tests/test_lei_akita.sh:174`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:672`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:694`)

Verifiquei a mudança **somente em memória**, executando AST original e AST com esse valor trocado: com `hoje=2026-11-06`, medição R1 `2026-10-01`, original retorna AMARELO/vencido; mutado retorna VERDE/em dia. A explicação está na classificação por `papel` e nas duas listas consultadas pelo veredito. (`app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:690`, `app/core/placar_estrutural.py:694`)

**Limite:** esse contraexemplo passa pelo juiz de `padrao`; o teste existente L114 exige R1 como único termômetro e exige os dois resultados de cor, portanto não afirmar que passa pela régua inteira. (`bin/tests/test_lei_akita.sh:243`, `app/core/tests/test_l114_placar_papel_e_termometro.py:34`, `app/core/tests/test_l114_placar_papel_e_termometro.py:35`, `app/core/tests/test_l114_placar_papel_e_termometro.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:75`)

| Símbolo | Quem escreve | Quem lê |
|---|---|---|
| `PAPEL_DO_MODULO` | Declarações dos módulos; fixture e alterações do MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:429`) | `_dado_medido` via `_atribuicoes`, sintaticamente no AST do ALVO. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:165`, `bin/tests/test_lei_akita.sh:209`) |
| `DADO_MEDIDO` | Declarações dos módulos e fixture MORDE. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:370`) | `_dado_medido` extrai nomes; `_linhas_livres` confere alvo do `Assign`. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:203`) |
| `RESULTADOS` | Definição de topo; teste reatribui e restaura. A responsabilidade declarada é “quem mede”. (`app/core/placar_estrutural.py:28`, `app/core/placar_estrutural.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:117`, `app/core/tests/test_l114_placar_papel_e_termometro.py:123`) | `placar`; selo de afirmação fora do ar; testes L114, R1 e R6. (`app/core/placar_estrutural.py:644`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:83`, `app/core/tests/test_l114_placar_papel_e_termometro.py:36`, `app/core/tests/test_l114_placar_papel_e_termometro.py:96`, `app/core/tests/test_r1_termometro_uma_medicao.py:41`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`) |
| `ETAPAS` | Definição de topo; responsabilidade declarada é “quem fecha a fatia”. (`app/core/espelho_verdade.py:19`, `app/core/espelho_verdade.py:35`) | `placar`; teste de costura da mesma medição R1. (`app/core/espelho_verdade.py:206`, `app/core/tests/test_r1_termometro_uma_medicao.py:45`) |

Os consumidores indiretos executam os módulos e chamam seus leitores: `gerar_estado` usa `placar`, `linha_do_placar`, `veredito` e publica as duas projeções. O handoff também lê texto do arquivo por regex para extrair a frase `contratos_estruturais`. (`bin/gerar_estado.py:77`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:83`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:144`, `bin/gerar_estado.py:145`, `bin/handoff_sessao.sh:179`)

Outros controles inseridos nos contêineres:

- `papel` determina classificação e seleção de resultados; `estado` determina contagens; `numero`/`prova` determinam rebaixamento a PENDENTE. (`app/core/placar_estrutural.py:646`, `app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:672`, `app/core/placar_estrutural.py:700`, `app/core/placar_estrutural.py:701`)
- `fonte_do_medido` é padrão de glob usado para ler nomes de arquivos e derivar a data; `id` indexa o mapa de datas; essa data determina vencimento. Não é apenas prosa de prova. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:52`, `bin/gerar_estado.py:56`, `app/core/placar_estrutural.py:650`, `app/core/placar_estrutural.py:668`)
- Em `ETAPAS`, `estado` e `prova` controlam rebaixamento e percentual; `itens`, `nome` e `porta` são chaves acessadas diretamente. Renomear chave ainda é sintaticamente puro, podendo mudar resultado ou causar exceção. (`app/core/espelho_verdade.py:208`, `app/core/espelho_verdade.py:209`, `app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:215`, `app/core/espelho_verdade.py:217`)
- `fonte_do_medido` adulterado pode mudar o universo de arquivos consultados; demonstrar uma nova data real sem acesso aos logs é **[não verificado]**. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`)
- Nos leitores rastreados, strings entram em templates fixos com `%`, não como expressão Python; não achei interpretação `eval`/`exec` nessas rotas. A ausência fora das rotas rastreadas é **[não verificado]**. (`bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`, `bin/gerar_estado.py:153`, `bin/gerar_estado.py:163`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`)

**ESMERIL:** nos comentários dos módulos, “medida” promete número medido e texto de prova; no selo significa qualquer expressão pura atribuída ao contêiner nomeado; nos leitores, esses contêineres também fornecem parâmetros de classificação, descoberta da data, rebaixamento e percentual. Portanto pureza sintática não garante ausência de decisão. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:651`, `bin/gerar_estado.py:47`, `app/core/espelho_verdade.py:211`)

## 06/11 20:35 · sessao 26d44a2e25e0 · - · - · origem=Ronald (tmux)

Não identifiquei contrato que quebre pela criação dos dois arquivos ou pelos novos nomes de classe; execução da suíte completa: [nao verificado]. [nao verificado]

| Consumidor | Efeito do delta |
|---|---|
| Descoberta da suíte | Labels abrangem `api` e `chamados`; o runner delega a descoberta ao `DiscoverRunner`, sem inventário fechado dos arquivos/classes (`bin/regua.sh:23`; `bin/suite.sh:102`; `app/core/runner_quarentena.py:72`, `:76`). |
| Selo TESTE-SEM-RELOGIO | Enumera nomes `test*.py`, portanto inclui ambos os novos arquivos; avalia seu conteúdo, não exige nomes de classe específicos (`app/core/tests/test_selo_teste_sem_relogio.py:48`, `:53`, `:59`). |
| Lista CRON-15 | Varre arquivos e classes de `api/tests` e `chamados/tests`, mas só coleta métodos decorados com `falha_esperada_cron15`. Os novos métodos não possuem essa marca, portanto não alteram o conjunto comparado (`app/core/tests/test_marca_cron15.py:25`, `:37`, `:41`, `:50`; `app/api/tests/test_api_bater_sse_sem_dia.py:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:25`). |
| Cobertura POST das portas | Agrega URLs dos `test_*.py`; o POST `/api/ponto/bater/` continua presente no novo arquivo, preservando a contribuição do teste movido (`app/core/tests/test_contract_smoke_porta.py:48`, `:51`; `app/api/tests/test_api_bater_sse_sem_dia.py:41`). |
| Imports locais | Varre inclusive testes e verifica os nomes importados; os novos arquivos usam modelos existentes, sem dependência dos nomes das classes antigas (`app/core/tests/test_contract_import_local_resolve.py:62`, `:71`, `:81`; `app/colaboradores/models.py:40`, `:177`, `:219`, `:283`; `app/ponto/models.py:8`; `app/chamados/models.py:2205`). |
| Censo dos contratos | Seleciona somente `test_contract_*.py`; os dois novos nomes ficam fora desse conjunto (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:164`; `app/api/tests/test_api_bater_sse_sem_dia.py:1`; `app/chamados/tests/test_a2_batida_sem_linha.py:1`). |
| P13 `data_evento` | Exclui diretórios `tests`, portanto os arquivos novos não entram no censo de escritores de produção (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:114`, `:115`). |
| Diagrama/cobertura por família | Lê `*/tests/test_*.py`, porém só associa selos que referenciam o registro `core.juizes`; os novos imports não fazem essa ligação (`app/core/management/commands/gerar_diagrama.py:450`, `:458`, `:665`, `:674`, `:708`, `:719`; `app/api/tests/test_api_bater_sse_sem_dia.py:15`; `app/chamados/tests/test_a2_batida_sem_linha.py:13`). |
| Guarda de RELATO | Varre testes, mas ignora fontes sem o literal `RELATO.md`; os arquivos novos não contêm esse literal (`bin/tests/test_relato_guarda_pedido_de_patch.sh:144`, `:146`; `app/api/tests/test_api_bater_sse_sem_dia.py:1`; `app/chamados/tests/test_a2_batida_sem_linha.py:1`). |
| Testes fora do Git | Compara arquivos encontrados com `git ls-files`; estado efetivamente versionado nesta cópia: [nao verificado] (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`). |

O integrador também deriva automaticamente labels de módulos para arquivos em `tests`, permitindo selecionar os dois novos módulos sem cadastro adicional (`app/core/integrador_lote.py:45`, `:57`, `:58`).

## 06/11 20:35 · sessao 8fabf48e1494 · - · - · origem=Ronald (tmux)

Auditoria por inspeção; execução do MORDE e de mutantes: **[não verificado]**.

**Falha adicional crítica:** `livre` prevalece sobre `preso` quando dois nós ocupam a mesma linha. `_linhas_livres` adiciona as faixas dos nós aos dois conjuntos e retorna `livre | (soltas - preso)`, sem subtrair `preso` de `livre`. Assim, terminar o contêiner com `]; CAD = 700` ou `}; funcao()` deixa essa linha livre pelo `Assign` permitido, embora a segunda instrução seja presa; o verificador aceita a alteração por linha. (`bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:230`)

**O MORDE não cobre cada cláusula nova.** Os casos de medida chamam as funções reais, mas recebem mensagens/arquivos montados manualmente; os commits têm mensagem `c`, portanto não exercitam a integração de medida com `falhas_do_universo`. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:414`)

| Caso | Mutante que mata por inspeção |
|---|---|
| `so_numero`, `estado`, `comentario`, `doc` | Recusa incondicional de medida; proibição de constante-string; retirada da liberação de comentário/docstring. (`bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:391`, `bin/tests/test_lei_akita.sh:406`) |
| `na_def` | Liberação de corpo de função. (`bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:407`) |
| `tira_def` | Julgar somente o lado novo, ignorando linhas presas removidas. (`bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:407`) |
| `com_import`, `com_lambda`, `com_chamada`, `com_nome` | Liberar respectivamente import, lambda, chamada ou Name sem ser constante-string. (`bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:396`, `bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:408`) |
| `decisao` | Liberar Assign fora dos nomes declarados. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:409`) |
| `monkey` | Liberar atribuição a Attribute. (`bin/tests/test_lei_akita.sh:399`, `bin/tests/test_lei_akita.sh:409`) |
| `redecl` | Liberar a linha histórica que acrescenta outra declaração; **não demonstra unicidade no ALVO**. O HEAD é restaurado à base após criar cada commit, e a declaração é lida desse HEAD. (`bin/tests/test_lei_akita.sh:74`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:409`) |
| `sem_decl` | Aceitar `PAPEL_DO_MODULO='juiz'` no ALVO. (`bin/tests/test_lei_akita.sh:373`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`) |
| `mais_fech` | Ignorar outro arquivo de negócio. (`bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:410`) |
| `so_numero` com instrumento | Estender a exceção a `n/a (instrumento)`. (`bin/tests/test_lei_akita.sh:411`) |
| ALVO sem declaração | Ler a declaração apenas de `h`, dispensando a do ALVO. (`bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:418`) |
| Retroativo número/decisão | Exigir declaração em `h`; ou liberar constante de decisão fora de DADO. (`bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:429`, `bin/tests/test_lei_akita.sh:431`) |

**Mutantes/lacunas que sobrevivem ou não são discriminados pelos casos existentes:**

- Retirar a unicidade de `PAPEL_DO_MODULO` ou `DADO_MEDIDO` no ALVO: nenhum caso planta ALVO com duplicação; `redecl` falha por linha presa em `h`. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:429`)
- Retirar a contagem de atribuições de constantes-string: a fixture contém constantes atribuídas uma vez; nenhum caso introduz reatribuição. (`bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:371`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:403`)
- Aceitar Assign com múltiplos alvos ou Subscript; liberar AnnAssign; liberar nós presos na mesma linha de dado; aceitar declaração aninhada ou DADO inválido; liberar classe/decorador, comentário dentro de função, docstring posterior, ou diff sem hunk: faltam casos específicos. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:199`, `bin/tests/test_lei_akita.sh:201`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:225`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:411`)
- Não há caso de medida com rebinding por for/with/import/walrus/del, nem leitor que interprete dado como decisão/código/chave. (`bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:372`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:411`)
- O comentário “nem concessão retroativa” ficou contraditório ao teste seguinte, que exige concessão retroativa para número. (`bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:420`, `bin/tests/test_lei_akita.sh:423`)

**Merge/rename/restauração:**

- Universo enumera todos os pais; começa com os caminhos diferentes do primeiro pai e remove os iguais a qualquer outro pai publicado; os restantes passam pelo predicado de medida. (`bin/tests/test_lei_akita.sh:285`, `bin/tests/test_lei_akita.sh:293`, `bin/tests/test_lei_akita.sh:296`, `bin/tests/test_lei_akita.sh:303`, `bin/tests/test_lei_akita.sh:308`, `bin/tests/test_lei_akita.sh:311`)
- `medida_do_commit` compara linhas com o primeiro pai e `--no-renames`, inclusive ambos os lados e contagem zero. (`bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:221`, `bin/tests/test_lei_akita.sh:228`, `bin/tests/test_lei_akita.sh:230`)
- Rename mantém origem e destino; origem ausente no ALVO recusa, mas origem restaurada/declarada posteriormente pode permitir exclusão histórica se só houver linhas livres. Criação que inclua declarações normais, função ou import recusa pelas linhas presas; ausência de arquivo num lado só é tolerada quando a contagem alterada desse lado é zero. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:211`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:230`)
- O MORDE antigo cobre primeiro pai, merge limpo/maligno e renames com `n/a (instrumento)`; não cobre elegibilidade AST de medida nesses cenários. (`bin/tests/test_lei_akita.sh:480`, `bin/tests/test_lei_akita.sh:486`, `bin/tests/test_lei_akita.sh:513`, `bin/tests/test_lei_akita.sh:520`, `bin/tests/test_lei_akita.sh:525`, `bin/tests/test_lei_akita.sh:536`, `bin/tests/test_lei_akita.sh:540`)

**Erro Git:** `show` e `diff` novos emitem `PADRAO_ERRO`; `cat-file -e` com qualquer rc retorna AST ausente, e `rev-parse h^1` com qualquer rc retorna False. Estes últimos recusam a medida, mas não cumprem literalmente “erro de Git = PADRAO_ERRO”. Os erros dessas novas operações não têm injeção específica no MORDE. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:142`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:214`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:219`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:433`)

**Integração e resultado:** o universo real fornece o callback de medida; o shell exige `PADRAO_FIM`; o MORDE precisa produzir `MORDE_OK` e nenhuma linha `MORDE_FALHOU`. A régua percorre `test_*.sh`, e o pre-push passa `PUSH_SHA` ao selo. (`bin/tests/test_lei_akita.sh:311`, `bin/tests/test_lei_akita.sh:565`, `bin/tests/test_lei_akita.sh:568`, `bin/tests/test_lei_akita.sh:569`, `bin/regua.sh:138`, `bin/pre-push.sh:38`)

## 06/11 20:35 · sessao 34ca66140e6e · - · - · origem=Ronald (tmux)

- Os dois decoradores agora estão nas classes; a fixture API está em `setUpTestData` e a fixture A2 em `setUp`, com os corpos dentro das respectivas classes (`app/api/tests/test_api_bater_sse_sem_dia.py:19`, `:22`, `:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:19`, `:25`).

- A cópia fixa `freezegun==1.5.5` e `Django==5.2.14` (`app/requirements.txt:43`, `:13`). O comportamento esperado é o decorador de classe do freezegun envolver `setUpClass` — que inclui `setUpTestData` no Django —, `setUp` e corpo; contudo as implementações das duas dependências não estão instaladas/encontráveis neste ambiente, portanto essa semântica não pôde ser comprovada pelo código disponível: **[nao verificado]**. Nenhuma execução Django/DB foi realizada: **[nao verificado]**.

- Há campos automáticos relevantes: `Colaborador.criado_em = DateTimeField(auto_now_add=True)` e `Batida.criado_em` idem (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`). A afirmação de que recebem o instante congelado depende daquela semântica de biblioteca: **[nao verificado]**. O nome de campo efetivo nesses modelos é `criado_em` (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`).

- Evitar a formulação absoluta “o arquivo inteiro não lê relógio real”: os imports são executados antes da aplicação do decorador; o que o código mostra diretamente é decoração da classe, não congelamento do import do módulo (`app/api/tests/test_api_bater_sse_sem_dia.py:9`, `:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:8`, `:17`). Cobertura de imports, `_pre_setup` do Django, rotinas do runner, bibliotecas ignoradas, threads e relógio externo: **[nao verificado]**.

- As fixtures API **não são literalmente idênticas**: os nomes têm sufixo `2` e o username mudou; os campos de empresa, praça, posto, colaborador, flags e ausência de criação de escala/batida continuam correspondentes (`app/api/tests/test_api_bater_ponto.py:197`; `app/api/tests/test_api_bater_sse_sem_dia.py:21`). Não reproduzir os identificadores pessoais encontrados na cópia (`app/api/tests/test_api_bater_ponto.py:206`; `app/api/tests/test_api_bater_sse_sem_dia.py:26`).

- A2 também muda os rótulos para sufixo `2`; mantém os demais parâmetros de criação e o usuário `sistema` (`app/chamados/tests/test_a2_data_evento_batida.py:11`; `app/chamados/tests/test_a2_batida_sem_linha.py:19`).

- CNPJ e CPF são realmente únicos no modelo; portanto haveria colisão se as linhas fossem compartilhadas no mesmo estado persistente (`app/colaboradores/models.py:43`, `:319`). Ambas as classes novas e as antigas herdam de `django.test.TestCase`, sem substituição local dessa base (`app/api/tests/test_api_bater_sse_sem_dia.py:10`, `:20`; `app/api/tests/test_api_bater_ponto.py:194`; `app/chamados/tests/test_a2_batida_sem_linha.py:9`, `:18`; `app/chamados/tests/test_a2_data_evento_batida.py:5`, `:10`). Isolamento transacional/limpeza entre classes é o comportamento esperado do Django TestCase, mas sua implementação e execução não puderam ser verificadas nesta cópia: **[nao verificado]**.

_2535 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
