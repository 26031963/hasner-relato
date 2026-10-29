# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 16:44** · publicado em **29/10/2026 16:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 16:44 · sessao c8da773fe4fd · o261 · censo · origem=Code

**O código usa critérios diferentes para “respondida”.** `respondida` e o desempate de `estado` usam apenas o carimbo; o juiz `pergunta_viva` exige **carimbo e texto** para dar precedência à resposta; `respostas_sem_veredito` aceita **carimbo ou texto**. Portanto, resposta vazia carimbada pode aparecer como respondida e impedir fechamento. (`app/chamados/models.py:2318`, `app/chamados/models.py:2112`, `app/chamados/models.py:2115`, `app/chamados/juizes.py:1020`, `app/chamados/models.py:1428`)

Nas tabelas, **NÃO** identifica explicitamente leitores cujo **teste local** não distingue `resposta_colab == ''` de resposta preenchida. Quando existe seleção anterior por vivacidade, ela está indicada separadamente; o juiz de vivacidade consulta o texto. (`app/chamados/juizes.py:1020`)

**Modelo, juízes e classificadores centrais**

| Leitor | O que decide | Vazia ≠ preenchida? |
|---|---|---|
| `PerguntaDisputa.respondida` — `app/chamados/models.py:2318` | Retorna se recebeu resposta, exclusivamente por `respondida_em is not None`. | **NÃO.** Vazia carimbada retorna verdadeiro. |
| `PerguntaDisputa.estado` — `app/chamados/models.py:2108` | Prioriza validação/materialização; nos ramos pendente/respondida, decide exclusivamente pela presença do carimbo. (`app/chamados/models.py:2112`, `app/chamados/models.py:2115`) | **NÃO no desempate pendente/respondida.** |
| `pergunta_viva` / `_pergunta_viva_calc` — `app/chamados/juizes.py:1005` | Carimbo **e** texto após `strip()` ativam a precedência da fala; a pergunta continua para decisão do admin, salvo as exceções de validação, tranca, decisão administrativa e concordância com a luz. (`app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1028`, `app/chamados/juizes.py:1032`) | **SIM.** Vazia carimbada não entra no ramo da fala. (`app/chamados/juizes.py:1020`) |
| Property `viva` — `app/chamados/models.py:2273` | Delega a `pergunta_viva`. | **SIM, por delegação.** (`app/chamados/juizes.py:1020`) |
| `q_vivas` — `app/chamados/models.py:2246` | No pré-filtro SQL, `_resposta_viva` exige carimbo, texto diferente de `''` e não nulo; esse ramo participa da seleção de potencialmente vivas. (`app/chamados/models.py:2247`, `app/chamados/models.py:2257`) | **SIM para `''`.** Esse teste SQL não aplica `strip()`. (`app/chamados/models.py:2246`) |
| `pendentes_do_colaborador` — `app/chamados/models.py:1381` | Seleciona disputas abertas com pergunta viva **sem carimbo**, para pendência/badge do colaborador. (`app/chamados/models.py:1383`, `app/chamados/models.py:1384`) | **NÃO no corte de pendência.** Texto preenchido sem carimbo continua candidato; vazia carimbada é excluída. (`app/chamados/models.py:1383`) |
| `respostas_sem_veredito` — `app/chamados/models.py:1428` | Considera resposta ainda sem validação/materialização quando existe **carimbo OU texto**; normalmente exclui dia trancado. (`app/chamados/models.py:1429`, `app/chamados/models.py:1430`, `app/chamados/models.py:1431`, `app/chamados/models.py:1432`) | **NÃO quando há carimbo:** vazia e preenchida entram. **SIM quando o carimbo é nulo:** texto preenchido ainda entra. |
| `pode_fechar` — `app/chamados/models.py:1439` | Só libera fechamento sem respostas sem veredito e sem fio vivo. | **Com carimbo, NÃO:** resposta vazia sem validação/materialização também bloqueia pela lista anterior. (`app/chamados/models.py:1430`) |
| `DisputaSupervisao.fechar` — `app/chamados/models.py:1472` | Consulta respostas sem veredito e recusa fechamento quando existem. (`app/chamados/models.py:1473`) | Mesmo **OU**: **NÃO com carimbo; SIM pelo texto quando carimbo nulo.** (`app/chamados/models.py:1430`, `app/chamados/models.py:1431`) |
| `perguntas_corrigiveis` — `app/chamados/juizes.py:1116` | Seleciona respostas que podem ser corrigidas/reabertas: carimbo presente e pergunta sem lastro ou viva. (`app/chamados/juizes.py:1117`, `app/chamados/juizes.py:1094`) | **NÃO no requisito “respondida”.** Há filtro adicional de lastro/vivacidade. |
| `perguntas_respondidas_a_validar` — `app/chamados/juizes.py:1123` | Monta fila global de validação: carimbo presente, texto diferente de `''`, potencialmente viva e viva pelo juiz; ordena pelo carimbo. (`app/chamados/juizes.py:1124`, `app/chamados/juizes.py:1126`, `app/chamados/juizes.py:1127`) | **SIM.** Exclui `''` explicitamente. |
| `irmas_respondidas_sem_consumo` — `app/chamados/juizes.py:1471` | Encontra respostas creditadas ao chamado, sem validação/materialização, para preservar o fio administrativo. (`app/chamados/juizes.py:1473`, `app/chamados/juizes.py:1474`) | **SIM.** Exige carimbo e texto após `strip()`. (`app/chamados/juizes.py:1477`) |
| Relatório `classificar` — `app/chamados/relatorios_disputa.py:17` | Entre as vivas: alguma sem carimbo → `AGUARDA_COLAB`; todas carimbadas → `AGUARDA_SUPERVISAO`; nenhuma viva → `PRONTA_FECHAR`. (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:20`, `app/chamados/relatorios_disputa.py:22`) | **NÃO no corte colaborador/supervisão.** A seleção anterior por vivacidade é distinta. |

**Cobrança, contadores, painel e decisões de fluxo**

| Leitor | O que decide | Vazia ≠ preenchida? |
|---|---|---|
| Preparação do fio — `app/chamados/services/fio.py:336` | Calcula `todas_respondidas` com `all(p.respondida_em)`; libera solicitar correção se todas respondidas, disputa aberta e limite disponível. (`app/chamados/services/fio.py:338`) | **NÃO.** |
| Regularização pelo fio — `app/chamados/services/fio.py:654` | Bloqueia regularização manual se alguma pergunta **viva** não tem carimbo. (`app/chamados/services/fio.py:655`) | **NÃO nesse gate.** |
| `acao_regularizar_disputa` — `app/chamados/services/acoes_disputa.py:130` | Gate equivalente: pergunta viva sem carimbo impede prosseguir para regularização/materialização. (`app/chamados/services/acoes_disputa.py:131`, `app/chamados/services/acoes_disputa.py:139`) | **NÃO nesse gate.** |
| `acao_reabrir_disputa` — `app/chamados/services/acoes_disputa.py:95` | Conta pendentes por carimbo nulo; combina com corrigíveis para decidir entre já pendente, nenhuma resposta corrigível e reabrir. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/acoes_disputa.py:103`) | **NÃO na contagem nem no requisito de resposta das corrigíveis.** (`app/chamados/juizes.py:1116`) |
| `reabrir_por_colab` — `app/chamados/models.py:1780` | Sem corrigíveis, retorna `sem_resposta_para_corrigir`; com corrigíveis, reabre e incrementa contador. (`app/chamados/models.py:1781`, `app/chamados/models.py:1784`) | **NÃO no requisito de resposta**, herdado de corrigíveis. (`app/chamados/juizes.py:1116`) |
| Filtro `respostas_que_o_reabrir_apaga` — `app/chamados/templatetags/chamados_tags.py:188` | Conta corrigíveis para informar quantas respostas a reabertura devolverá ao colaborador. | **NÃO no requisito de resposta**, pelo mesmo juiz. (`app/chamados/juizes.py:1116`) |
| `perguntas_pendentes_do_colab` — `app/chamados/services/aviso_pendencia.py:13` | Retorna perguntas vivas sem carimbo; alimenta aviso de que texto livre não respondeu ao questionário. (`app/chamados/services/aviso_pendencia.py:17`, `app/chamados/services/aviso_pendencia.py:21`) | **NÃO no teste de pendência.** |
| `_pendentes` / `checar_cobranca` — `app/chamados/services/cobranca.py:47` | Reutiliza a lista anterior; decide se pode cobrar, conta perguntas/disputas e impede nova cobrança durante prazo vigente. (`app/chamados/services/cobranca.py:48`, `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:88`, `app/chamados/services/cobranca.py:89`) | **NÃO no teste de resposta**, herdado de `aviso_pendencia`. (`app/chamados/services/aviso_pendencia.py:13`) |
| `_calados` — `app/chamados/services/cobranca.py:229` | Carimbo nulo, `q_vivas` e disputa aberta formam universo de pessoas caladas; conta vencidos e sem canal. (`app/chamados/services/cobranca.py:230`, `app/chamados/services/cobranca.py:243`, `app/chamados/services/cobranca.py:246`, `app/chamados/services/cobranca.py:248`) | **NÃO no corte “calado”.** Não exige texto vazio. |
| `recobrar_questionario` — `app/chamados/services/disputa_emissao.py:1445` | Conta pendentes por carimbo nulo para resultado/trilha da recobrança. (`app/chamados/services/disputa_emissao.py:1450`, `app/chamados/services/disputa_emissao.py:1453`) | **NÃO.** |
| `becos.classificar` — `app/chamados/services/becos.py:29` | Pergunta sem carimbo e destino não resolvido conta como visível ao colaborador; sem essas perguntas e com pai resolvido, classifica beco, sujeito a `pode_fechar`. (`app/chamados/services/becos.py:30`, `app/chamados/services/becos.py:33`) | **NÃO no corte de visibilidade.** O fechamento consulta outra autoridade. |
| Censo `furo_sem_canal` — `app/chamados/services/furo_sem_canal.py:80` | Pergunta com carimbo nulo conta como canal aberto da célula; conta células acusatórias sem esse canal nem chamado vivo. (`app/chamados/services/furo_sem_canal.py:81`, `app/chamados/services/furo_sem_canal.py:85`, `app/chamados/services/furo_sem_canal.py:95`) | **NÃO.** |
| `creditos_perguntas` — `app/chamados/services/raiox.py:136` | Divide perguntas selecionadas em respondidas e não respondidas por `p[3]`, que é `respondida_em`; credita cada conjunto ao destino vivo ou ao pai. (`app/chamados/services/raiox.py:137`, `app/chamados/services/raiox.py:138`, `app/chamados/services/raiox.py:144`, `app/chamados/services/raiox.py:149`) | **NÃO nessa divisão.** Há pré-filtro por `q_vivas` e exclusão de materializada de fato. (`app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`) |
| `particionar_verbos` — `app/chamados/services/verbos.py:31` | Usa os créditos para atribuir verbo e contar chamados; o catálogo dá precedência a resposta para `admin_validar`, depois pergunta para `colab`. (`app/chamados/services/verbos.py:44`, `app/chamados/services/verbos.py:53`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/catalogo/natureza.py:50`) | **NÃO acrescenta distinção textual** à divisão dos créditos. (`app/chamados/services/raiox.py:137`) |
| `placar_aceite` — `app/chamados/services/verbos.py:306` | Conta colaboradores que responderam depois da cobrança mais recente, hoje e em sete dias. (`app/chamados/services/verbos.py:308`, `app/chamados/services/verbos.py:315`, `app/chamados/services/verbos.py:317`, `app/chamados/services/verbos.py:319`) | **NÃO.** A consulta traz colaborador e carimbo, sem texto. (`app/chamados/services/verbos.py:309`) |
| Histórico — `app/chamados/services/historico.py:99` | Carimbo gera evento “colaborador respondeu”; vazio troca apenas o conteúdo exibido para `sem resposta`. (`app/chamados/services/historico.py:105`, `app/chamados/services/historico.py:109`, `app/chamados/services/historico.py:113`) | **NÃO para gerar evento de resposta; SIM apenas na legenda.** |
| `explicar_pergunta` — `app/chamados/services/explicador.py:72` | `.estado` pendente → colaborador/cobrar; respondida → admin/validar ou recusar. (`app/chamados/services/explicador.py:73`, `app/chamados/services/explicador.py:77`, `app/chamados/services/explicador.py:79`) | **NÃO no desempate**, herdado de `.estado`. (`app/chamados/models.py:2112`) |
| `_gaveta_de` — `app/chamados/services/fila_por_causa.py:215` | Disputa aberta com resposta sem veredito vai para `respondido_a_decidir`. (`app/chamados/services/fila_por_causa.py:216`) | **NÃO com carimbo; SIM para texto sem carimbo**, pelo **OU** do modelo. (`app/chamados/models.py:1430`, `app/chamados/models.py:1431`) |
| Competência trancada — `app/chamados/services/competencia_trancada.py:113` | Consulta respostas sem veredito ignorando tranca; conta e encaminha para decisão do DP. (`app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:128`) | Mesmo **OU**: **vazia carimbada entra**. (`app/chamados/models.py:1430`) |
| `fila_validar_classificada` — `app/chamados/services/validacao.py:867` | Classifica e conta a fila global de respondidas; calcula idade pelo carimbo. (`app/chamados/services/validacao.py:868`, `app/chamados/services/validacao.py:870`, `app/chamados/services/validacao.py:875`) | **SIM no fluxo padrão**, que exclui `''`; um `qs` fornecido substitui a seleção padrão. (`app/chamados/juizes.py:1124`, `app/chamados/services/validacao.py:867`) |

Os consumidores seguintes propagam essas decisões, sem criar outro teste textual: [nao verificado]

| Consumidor | Efeito e distinção herdada |
|---|---|
| Dashboard e APIs de início — `app/core/views.py:253`, `app/api/views.py:325`, `app/api/views_core.py:314` | Contam `pendentes_do_colaborador`; APIs exibem badge `Responder DP`. **NÃO distinguem texto no corte de pendência herdado.** (`app/api/views.py:327`, `app/api/views_core.py:316`, `app/chamados/models.py:1383`) |
| Painel — `app/chamados/services/painel.py:228`; lente — `app/chamados/services/verbos.py:168`; fila — `app/chamados/services/fila.py:55` | Exibem/filtram/contam pelos verbos derivados dos créditos. **NÃO acrescentam distinção textual.** (`app/chamados/services/painel.py:229`, `app/chamados/services/verbos.py:180`, `app/chamados/services/fila.py:60`, `app/chamados/services/raiox.py:137`) |
| Cobrança em massa — `app/chamados/views_cobrar.py:77` | Seleciona apenas verbo `cobrar`. **NÃO acrescenta distinção textual.** (`app/chamados/views_cobrar.py:78`, `app/chamados/services/raiox.py:138`) |
| Arquivamento — `app/chamados/views_arquivar.py:115`, `app/chamados/management/commands/arquivar_prescritos.py:50` | Usa verbos para selecionar alvos e contar por categoria. **NÃO acrescenta distinção textual.** (`app/chamados/views_arquivar.py:117`, `app/chamados/management/commands/arquivar_prescritos.py:60`) |
| Navbar — `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/context_processors.py:90` | Deriva badge administrativo de Validar + Decidir; **NÃO acrescenta distinção textual**. (`app/core/management/commands/lavrar_badge_navbar.py:73`, `app/chamados/services/verbos.py:24`) |
| `na_fila` — `app/chamados/services/na_fila.py:19` | Conta por colaborador/empresa os chamados com verbo de fila. **NÃO acrescenta distinção textual.** (`app/chamados/services/na_fila.py:23`) |
| Mensageria — `app/api/views_mensageria.py:395`, `app/api/views_mensageria.py:453`, `app/api/views_mensageria.py:1142` | Consome a mesma partição por verbo. **NÃO acrescenta distinção textual** à classificação herdada. (`app/chamados/services/verbos.py:31`, `app/chamados/services/raiox.py:137`) |
| Tela de pendências — `app/colaboradores/views.py:2997`; exportação do relatório — `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | Apresentam categorias para validar/cobrar; o command exporta JSON. **NÃO distinguem vazio no desempate herdado colaborador/supervisão.** (`app/colaboradores/views.py:2999`, `app/colaboradores/views.py:3001`, `app/chamados/management/commands/relatorio_pendencias_disputa.py:18`, `app/chamados/relatorios_disputa.py:20`) |

**Views, templates e API do colaborador**

| Leitor | O que decide | Vazia ≠ preenchida? |
|---|---|---|
| POST HTML — `app/chamados/views.py:1280` | Calcula `ja_respondida` e passa ao core; muda indicação de correção na auditoria e no fio. (`app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1286`, `app/chamados/services/disputa_emissao.py:1312`) | **NÃO sobre a resposta persistida.** |
| Recobrança HTML — `app/chamados/views.py:1416` | Busca sem carimbo, retém vivas e decide recobrar ou informar que não há pergunta viva para cobrar. (`app/chamados/views.py:1417`, `app/chamados/views.py:1421`, `app/chamados/views.py:1431`) | **NÃO no filtro de resposta.** Vivacidade é outro filtro. |
| Validação inline — `app/chamados/views_cobrar.py:169` | Entre as vivas, carimbo presente seleciona perguntas para validar. (`app/chamados/views_cobrar.py:208`) | **NÃO no corte de respondida.** |
| Fallback da validação inline — `app/chamados/views_cobrar.py:175` | Sem itens para validar, busca carimbo nulo e informa que aguarda o colaborador; pode apontar resposta existente no chat. (`app/chamados/views_cobrar.py:176`, `app/chamados/views_cobrar.py:180`, `app/chamados/views_cobrar.py:192`) | **NÃO no filtro.** |
| API de chamados — `app/api/views.py:1187` | `all(p.respondida_em)` participa de `pode_solicitar_correcao`, junto do limite de reaberturas. (`app/api/views.py:1184`) | **NÃO.** |
| API de perguntas — `app/api/views.py:2621` | Usa `.estado` para separar `respondidas` de `perguntas` e publicar `aguarda_admin`. (`app/api/views.py:2622`, `app/api/views.py:2624`) | **NÃO no desempate de estado.** A lista anterior vem de `perguntas_do_app`. (`app/chamados/models.py:2112`, `app/api/views.py:2604`) |
| Destino da API — `app/api/views.py:2643` | Escolhe pergunta que não esteja `RESPONDIDA` para ancorar o chamado de destino. | **NÃO no desempate herdado de estado.** (`app/chamados/models.py:2112`) |
| POST API — `app/api/views.py:2688` | Calcula `ja_respondida` e passa ao mesmo core. (`app/api/views.py:2695`) | **NÃO sobre o texto persistido.** Recusar a **nova entrada** vazia é outra guarda. (`app/api/views.py:2690`) |
| `_pergunta_card` — `app/templates/chamados/partials/_pergunta_card.html:24` | Alterna aguardando validação/aguardando colaborador, depois de priorizar validada. (`app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:25`) | **NÃO.** |
| `_pergunta_card` — `app/templates/chamados/partials/_pergunta_card.html:29` | Carimbo habilita apresentação da resposta e fluxo de Validar/Rejeitar quando ainda não validada. (`app/templates/chamados/partials/_pergunta_card.html:31`, `app/templates/chamados/partials/_pergunta_card.html:43`) | **NÃO.** |
| `modal_fio` — `app/templates/chamados/partials/modal_fio.html:182` | Mostra fluxo da pergunta cega somente quando não carimbada. | **NÃO.** |
| `modal_fio` — `app/templates/chamados/partials/modal_fio.html:210` | Alterna resposta/validação/correção versus aguardando resposta do colaborador. (`app/templates/chamados/partials/modal_fio.html:212`, `app/templates/chamados/partials/modal_fio.html:311`) | **NÃO.** |
| `modal_fio` — `app/templates/chamados/partials/modal_fio.html:327` | `todas_respondidas` alterna Cobrar versus Regularizar disputa e fechar. (`app/templates/chamados/partials/modal_fio.html:342`, `app/templates/chamados/partials/modal_fio.html:345`) | **NÃO**, herdado de `all(carimbo)`. (`app/chamados/services/fio.py:336`) |
| `modal_fio`, explicação — `app/templates/chamados/partials/modal_fio.html:433` | Mostra aguardando colaborador ou ação do admin conforme estado projetado pelo explicador. (`app/templates/chamados/partials/modal_fio.html:434`) | **NÃO no desempate herdado de `.estado`.** (`app/chamados/services/explicador.py:72`, `app/chamados/models.py:2112`) |
| Card do colaborador — `app/templates/chamados/partials/_card_atendimento.html:94` | Na pergunta viva, mostra resposta enviada e aguardando supervisão. (`app/templates/chamados/partials/_card_atendimento.html:99`, `app/templates/chamados/partials/_card_atendimento.html:101`) | **NÃO nesse recibo.** |
| Card do colaborador — `app/templates/chamados/partials/_card_atendimento.html:155` | Alterna Enviar/Reenviar resposta. | **NÃO.** |
| Card do colaborador — `app/templates/chamados/partials/_card_atendimento.html:384` | Pergunta de hora carimbada recebe selo Enviado com resposta/data. (`app/templates/chamados/partials/_card_atendimento.html:385`) | **NÃO.** |
| Card do colaborador — `app/templates/chamados/partials/_card_atendimento.html:135` | Texto existente mostra “Você respondeu… escolha de novo para corrigir”; carimbo apenas acrescenta data. (`app/templates/chamados/partials/_card_atendimento.html:137`) | **SIM, só pelo texto.** |
| Card do colaborador — `app/templates/chamados/partials/_card_atendimento.html:176` | Na pergunta não viva, texto existente mostra “Sua resposta”. (`app/t

[truncado em 20000 de 40904 caracteres]

## 29/10 16:43 · sessao 6b7e1b35c615 · justif · leitura · origem=Code

**1. Sítios que gravam `Justificativa.status='aprovada'`**

O censo encontrou **três escritores explícitos**, todos por atribuição seguida de `save()`. Não encontrei aprovação explícita por `update()`, `create()` ou `get_or_create(defaults=...)`. (`app/ponto/services/justificativa.py:85`, `app/ponto/services/justificativa.py:262`, `app/ponto/views.py:1787`)

| Função / gravação | Dentro de `transaction.atomic`? | Efeito direto sobre chamados |
|---|---|---|
| `aprovar_justificativa_dp()` — atribuição em `app/ponto/services/justificativa.py:85`, `save()` em `:88` | **Sim**, bloco em `app/ponto/services/justificativa.py:23`, com `select_for_update()` em `:24`. | Não altera os ponteiros. Chama followup que lê `chamado_origem` e apenas anexa ocorrência. Depois procura o chamado mais recente do colaborador de módulo **`entrada_bloqueada_turno_aberto`** e chama `encerrar()`, sem filtrar pelo vínculo com a justificativa. (`app/ponto/services/justificativa.py:112`, `app/chamados/utils.py:7`, `app/chamados/utils.py:21`, `app/ponto/services/justificativa.py:121`, `app/ponto/services/justificativa.py:127`) |
| `materializar_saida_dp()` — atribuição em `app/ponto/services/justificativa.py:262`, `save()` em `:265` | **Sim**, bloco em `app/ponto/services/justificativa.py:224`, com `select_for_update()` em `:225`. | Não altera os ponteiros. Encerra chamados do mesmo colaborador/dia, nos módulos `turno_aberto`, `turno_aberto_24h`, `batida_ausente`, `saida_sem_entrada`, em `NAO_FECHADOS`. Usa `contexto_json.data_turno`, com fallback para data de criação quando essa chave está nula; não seleciona por `chamado_origem` ou `justificativa_relacionada`. (`app/ponto/services/justificativa.py:274`, `app/ponto/services/justificativa.py:276`, `app/ponto/services/justificativa.py:285`) |
| `aprovar_lote_justificativas()` — atribuição em `app/ponto/views.py:1787`, `save()` em `:1790` | **Não no corpo/decorador da função**; o decorador é `login_required`. As configurações de banco de produção não habilitam `ATOMIC_REQUESTS`. (`app/ponto/views.py:1762`, `app/ponto/views.py:1784`, `app/config/settings/prod.py:9`, `app/config/settings/saas.py:83`) | Não manipula diretamente chamados ou seus vínculos. Invalida o fechamento e pode registrar uma saída, produzindo os efeitos indiretos da batida. (`app/ponto/views.py:1795`, `app/ponto/views.py:1799`, `app/ponto/views.py:1806`) |

Nos três casos, o `save()` também aciona `_cartorio_post_save_justificativa()`, que rejulga `data_turno` por `_julgar_dias()` → `cartorio.julgar_celula()`. Esse receiver não chama diretamente o reconciliador de chamados. (`app/ponto/signals.py:161`, `app/ponto/signals.py:165`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`)

Há ainda uma **porta genérica pelo Django admin**: `JustificativaAdmin` herda de `admin.ModelAdmin`, sem restrição local de edição; `status` admite `aprovada`. A implementação herdada da gravação e sua atomicidade não estão na árvore `app/` examinada **[não verificado]**. (`app/ponto/admin.py:18`, `app/ponto/admin.py:19`, `app/ponto/models.py:132`, `app/ponto/models.py:159`)

**2. Métodos do próprio `ChamadoColaborador` e seus gates**

O catálogo define `ENCERRADOS = ('resolvido', 'fechado')`; `cancelado` e `superado` são terminais adicionais. (`app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`)

| Método | Destino a partir de `aberto`/`em_analise` | O que pergunta antes de escrever |
|---|---|---|
| `transicionar()` | `resolvido` ou `fechado` | Motor/perfil; justificativa pendente **somente para `fechado`**; disputa viva para ambos; para `resolvido` de furo, `premissa_regularizada()` — padrão `para='cobranca'`, com fallback legado sem dia. (`app/chamados/models.py:125`, `app/chamados/models.py:130`, `app/chamados/models.py:142`, `app/chamados/models.py:165`) |
| `fechar()` | Por padrão `fechado`, aceita outro destino | Delega a `transicionar()`, herdando os gates acima. (`app/chamados/models.py:262`, `app/chamados/models.py:264`) |
| `reconciliar()` | `resolvido` | Reconfere o estado no banco, consulta o motor `RECONCILIAR` e o gate da premissa. O argumento `novo_status` não determina o destino: a escrita usa o resultado do motor. (`app/chamados/models.py:434`, `app/chamados/models.py:440`, `app/chamados/models.py:444`, `app/chamados/models.py:463`, `app/chamados/catalogo/motor.py:111`) |
| `encerrar()` | `fechado` | Somente motor `ENCERRAR`; não pergunta justificativa, disputa ou regularização. (`app/chamados/models.py:1196`, `app/chamados/models.py:1201`, `app/chamados/catalogo/motor.py:114`) |
| `encerrar_por_competencia_trancada()` | `fechado` | Exige estado em `VIVOS` e motor `ENCERRAR`; o próprio método não consulta a tranca da competência. (`app/chamados/models.py:1208`, `app/chamados/models.py:1211`, `app/chamados/models.py:1214`) |

O gate de `reconciliar()` permite encerrar se não for furo; se não houver dia identificável, com trilha; se `premissa_regularizada(para='folha')` ou agrupamento sob pai vivo responder verdadeiro; ou se existir outra cobrança viva de furo para o mesmo colaborador/dia, como deduplicação. Fora disso, recusa. (`app/chamados/models.py:484`, `app/chamados/models.py:488`, `app/chamados/models.py:495`, `app/chamados/models.py:498`, `app/chamados/models.py:525`)

**Se `justificativa_relacionada.status == 'aprovada'`, `premissa_regularizada(para='folha')` retorna `True` imediatamente**, antes de consultar dia, célula ou turno; esse primeiro ramo independe de `para`. (`app/chamados/models.py:923`, `app/chamados/models.py:937`, `app/chamados/models.py:938`)

Para completar os terminais: `cancelar()` consulta o motor e escreve `cancelado`; `superar()` consulta o motor e exige sucessor existente, diferente de si e não terminal, gravando vínculo e estado em `atomic`. (`app/chamados/models.py:527`, `app/chamados/models.py:532`, `app/chamados/models.py:543`, `app/chamados/models.py:547`, `app/chamados/models.py:557`)

Os escritores internos são `_aplicar()` → `_gravar_estado()`. Não têm gate próprio de regularização; o núcleo exige via válida para terminal e salva estado/trilha dentro de `atomic`. (`app/chamados/models.py:257`, `app/chamados/models.py:260`, `app/chamados/models.py:204`, `app/chamados/models.py:226`, `app/chamados/models.py:231`)

**3. Origem de `vivos`, `_com_fio` e do `cobrado` sem protesto**

`_julgar_colab_corpo()` carrega `chs = chamados_que_falam_de_batida(c)`. Essa função consulta os chamados do colaborador cujo módulo pertence a `FURO_MODULES`, faz prefetch de `perguntas_que_resolvem` e agrupa pela data declarada pelo catálogo do módulo. (`app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:85`, `app/ponto/services/cartorio.py:94`, `app/ponto/services/cartorio.py:96`)

Para cada célula:

- `lista = chs.get(data.isoformat(), [])`. (`app/ponto/services/cartorio.py:1040`)
- `vivos` filtra `lista` por `status_local in VIVOS`; aqui `VIVOS` inclui **`aberto`, `em_analise` e `registrado`**. (`app/ponto/services/cartorio.py:1069`, `app/chamados/catalogo/motor.py:24`)
- `_com_fio` filtra `vivos` por `fio_vivo(x.perguntas_que_resolvem.all())`. `fio_vivo()` significa existir pelo menos uma pergunta para a qual `pergunta_viva()` responde verdadeiro. (`app/ponto/services/cartorio.py:1074`, `app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1111`)

Portanto, naquele bloco, **`cods` vazio e pelo menos um chamado vivo com fio vivo** fazem `_vd = 'cobrado'`. Apenas o status vivo do chamado não basta. (`app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1141`, `app/ponto/services/cartorio.py:1142`)

**4. Quem encerra o `turno_aberto` após a aprovação**

**Não há encerramento dedicado de `j.chamado_origem` simplesmente porque a justificativa foi aprovada.** No fluxo comum, esse ponteiro serve para anexar uma ocorrência; o encerramento explícito procura `entrada_bloqueada_turno_aberto`. Existem, porém, os caminhos condicionais abaixo. (`app/chamados/utils.py:7`, `app/chamados/utils.py:21`, `app/ponto/services/justificativa.py:121`, `app/ponto/services/justificativa.py:127`)

| Caminho existente | Condição e encerrador |
|---|---|
| **Materialização retroativa** | Aprova e chama `encerrar()` nos chamados do colaborador/dia, incluindo `turno_aberto`; independe do ponteiro `chamado_origem`. (`app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:274`, `app/ponto/services/justificativa.py:285`) |
| **Batida de saída registrada na aprovação** | O signal de Batida chama `reconciliar_chamados_colab()`. Para turno aberto, a coleta precisa concluir que o turno correspondente deixou de estar aberto; então `_retratar()` chama `reconciliar()`, podendo reter por perguntas respondidas aguardando consumo. Na aprovação individual, a batida pode ocorrer antes da gravação de `aprovada`. (`app/ponto/services/justificativa.py:69`, `app/ponto/services/justificativa.py:85`, `app/ponto/views.py:1806`, `app/ponto/signals.py:214`, `app/chamados/reconciliador.py:287`, `app/chamados/reconciliador.py:1208`, `app/chamados/reconciliador.py:435`, `app/chamados/reconciliador.py:456`) |
| **Cartório acionado pelo save da justificativa** | Chama `reconciliar()` por marcos cobrados acesos, marcos inexistentes, protestos `COBRANCA_EM_DIA_SEM_TRABALHO`/`COBRANCA_SEM_FURO`, ou fio integralmente duplicado sem respostas. A aprovação permitir o gate não substitui esses critérios do chamador. (`app/ponto/signals.py:165`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1175`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196`, `app/ponto/services/cartorio.py:1261`, `app/ponto/services/cartorio.py:1266`) |
| **Consumidor de eventos da célula** | A escrita da célula emite evento; o consumidor retrata chamados de turno somente se a célula tiver veredito que não acusa e nenhum marco pendente, passando por `_retratar()` → `reconciliar()`. (`app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:642`, `app/chamados/reconciliador.py:1145`, `app/chamados/reconciliador.py:800`, `app/chamados/reconciliador.py:823`, `app/chamados/reconciliador.py:825`, `app/chamados/reconciliador.py:456`) |
| **Disputa vinculada encerrada** | O signal da disputa tenta `ch.reconciliar()`; o evento necessário é o fechamento da disputa. (`app/chamados/signals.py:28`, `app/chamados/signals.py:32`, `app/chamados/signals.py:42`) |
| **Ação humana** | Pode chamar `transicionar()`; a aprovação satisfaz o coletor da guarda de resolução, mas não executa a ação humana. (`app/chamados/services/acoes_chamado.py:315`, `app/chamados/models.py:167`, `app/chamados/models.py:937`) |

O reconciliador periódico também chama a coleta que exige a morte da premissa física do turno; não seleciona o chamado simplesmente pela aprovação da justificativa. (`app/chamados/management/commands/reconciliar_chamados.py:52`, `app/chamados/reconciliador.py:287`, `app/chamados/reconciliador.py:1208`)

O caso automático expõe a lacuna: o cron cria `turno_aberto` com contexto contendo data, entrada e posto, **sem `marcos_faltantes`**; cria a justificativa `esqueci_saida` com `chamado_origem` e aponta o vínculo inverso quando ela nasce. Sem marcos nomeados, os testes de marcos acesos/inexistentes não o encerram. (`app/ponto/management/commands/processar_alertas_turno.py:348`, `app/ponto/management/commands/processar_alertas_turno.py:396`, `app/ponto/management/commands/processar_alertas_turno.py:399`, `app/ponto/management/commands/processar_alertas_turno.py:423`, `app/ponto/services/lastro.py:171`, `app/ponto/services/lastro.py:111`)

Assim, **aprovação sem saída materializada pode deixá-lo vivo**: se não há protestos e o fio permanece vivo, o dia vira `cobrado`; esse veredito acusa e impede a absolvição pelo consumidor da célula. O `True` de `premissa_regularizada()` autoriza uma tentativa efetiva de encerramento, mas não a dispara. (`app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1142`, `app/escala/models.py:1735`, `app/chamados/reconciliador.py:823`, `app/chamados/models.py:440`, `app/chamados/models.py:495`, `app/chamados/models.py:937`)

A rotina `fechar_cobranca_com_lastro` não cobre esse módulo: sua consulta filtra exclusivamente `batida_ausente`. (`app/ponto/services/lastro.py:216`, `app/ponto/services/lastro.py:217`)

## 29/10 16:43 · sessao ff3f10a49a1a · o261 · censo · origem=Code

**Há vários caminhos que fecham sem consultar a pergunta dependente. A proteção não está centralizada em `reconciliar()` nem em `encerrar()`; ela existe em dois executores específicos.** A transição humana também consulta outra relação: a disputa cujo **pai** é o chamado, não todas as perguntas cujo `chamado_destino` é ele. (`app/chamados/models.py:434`, `app/chamados/models.py:453`, `app/chamados/models.py:1192`, `app/chamados/juizes.py:1817`)

Os terminais considerados são **`resolvido`, `fechado`, `cancelado`, `superado`**. (`app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`)

Abaixo, “sítio” identifica a chamada que executa a transição; entradas que delegam para esses executores aparecem separadamente. A eficácia em execução/banco não foi verificada. [não verificado]

**Como interpretar a consulta e a auditoria**

- **Sim\***: chama `irmas_respondidas_sem_consumo()`. Ela consulta `chamado_destino=ch`, `respondida_em IS NOT NULL`, `validada_em IS NULL`, **mas também exige `materializada_em IS NULL` e texto não vazio**; inclui ainda perguntas creditadas ao pai quando o destino não está vivo. Portanto, não cobre integralmente o predicado mais amplo do pedido. (`app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1474`, `app/chamados/juizes.py:1477`)
- **Não**: não há essa consulta antes da transição no caminho indicado. A consulta do núcleo a perguntas dependentes ocorre **depois** do `save()`, para relavrar o veredito; não impede o fechamento. (`app/chamados/models.py:231`, `app/chamados/models.py:246`)
- Todas as transições pelo núcleo tentam gravar **`LogAuditoria.acao='estado_chamado'`**, com `valor_depois.status_local` e `valor_depois.via`. As ações adicionais estão indicadas nas tabelas. (`app/chamados/models.py:227`, `app/chamados/models.py:229`, `app/core/observ.py:134`)
- `motivo` passado a `cancelar()`/`superar()` vira conteúdo de nota; não é acrescentado ao registro `estado_chamado`. (`app/chamados/models.py:538`, `app/chamados/models.py:561`, `app/chamados/models.py:229`)

| Porta / escritor | Estado e `via` literal | Consulta dependente antes? |
|---|---|---|
| `_gravar_estado()` — `app/chamados/models.py:185`; atribuição em `:192`, persistência em `:231` | Estado e via recebidos; exige via válida para terminal (`:197`, `:204`) | **Não**; consulta posterior em `:246` |
| `_aplicar()` — `app/chamados/models.py:257` | Destino recebido; `via='admin'` (`:260`; `app/chamados/catalogo/motor.py:95`) | **Não**; o próprio método exige que o chamador rode as guardas (`:258`) |
| `transicionar()` / `fechar()` — `app/chamados/models.py:122`, `:262` | `resolvido`/`fechado`; `via='admin'` (`:183`, `:260`; `app/chamados/catalogo/motor.py:145`) | **Não pelo destino**; guarda por disputa-pai (`app/chamados/models.py:147`; `app/chamados/juizes.py:1817`) |
| `reconciliar()` — `app/chamados/models.py:434` | `resolvido`; `via='sistema'` (`:444`; `app/chamados/catalogo/motor.py:95`, `:111`) | **Não**; passa pelo motor e pela premissa do dia (`app/chamados/models.py:467`, `:470`) |
| `encerrar()` — `app/chamados/models.py:1192` | `fechado`; `via='cascade'` (`:1201`; `app/chamados/catalogo/motor.py:95`, `:114`) | **Não** (`app/chamados/models.py:1196`) |
| `encerrar_por_competencia_trancada()` — `app/chamados/models.py:1204` | `fechado`; `via='competencia_trancada'` (`:1211`, `:1214`; `app/chamados/catalogo/motor.py:92`) | **Não** (`app/chamados/models.py:1208`) |
| `cancelar()` — `app/chamados/models.py:527` | `cancelado`; `via='cancelado'`; `motivo=''` por padrão (`:527`, `:537`; `app/chamados/catalogo/motor.py:95`, `:116`) | **Não** (`app/chamados/models.py:531`) |
| `superar()` — `app/chamados/models.py:543` | `superado`; `via='superado'`; `motivo=''` por padrão (`:543`, `:560`; `app/chamados/catalogo/motor.py:95`, `:117`) | **Não**; verifica o chamado sobrevivente e o motor (`app/chamados/models.py:547`, `:549`) |

O fechamento por `reconciliar()` pode acrescentar `acao='fechamento_sem_dia_do_fato'`; ou `acao='fechamento_por_dedup'` com **`motivo='cobranca_viva_do_dia'`**. Quando recusa, usa `acao='fechamento_recusado'` com **`motivo='dia_furado_para_a_folha'`**. Essas regras não são a consulta de dependentes. (`app/chamados/models.py:491`, `:505`, `:508`, `:516`, `:522`)

**Executores com consulta de dependentes**

Todos os sítios seguintes usam `resolvido / sistema / estado_chamado` pela porta comum. (`app/chamados/models.py:444`, `:227`; `app/chamados/catalogo/motor.py:95`, `:111`)

| Função / sítio | Consulta antes? | Ação adicional literal |
|---|---|---|
| `reconciliador._retratar()` — `app/chamados/reconciliador.py:456` | **Sim\***, chamada em `:435`; retorna antes do fechamento em `:454` | Na retenção: **`retratacao_retida_irma_orfa`** (`:449`) |
| `materializacao._reconciliar_destino_e_clones()._fecha()` — `app/chamados/services/materializacao.py:711` | **Sim\*** em `:702`; aplicada ao destino em `:714` e a cada clone em `:721` | Na retenção: **`destino_retido_irma_orfa`** (`:704`) |

Entradas que herdam essa proteção por delegação:

| Entrada | Delegação / ação adicional |
|---|---|
| `retratar()` | `_retratar()` — `app/chamados/reconciliador.py:479` |
| `retratar_chamados_absolvidos_por_celula()` | `_retratar()` — `app/chamados/reconciliador.py:800` |
| `reconciliar_chamados_colab()` | `_retratar()` — `app/chamados/reconciliador.py:1214` |
| `consumir_eventos_celula()` | `retratar_chamados_absolvidos_por_celula()` — `app/chamados/reconciliador.py:1145` |
| Command `reconciliar_chamados` | Reconciliação por colaborador e consumo de eventos — `app/chamados/management/commands/reconciliar_chamados.py:52`, `:62` |
| Command `consumir_eventos_celula` | Consumidor — `app/chamados/management/commands/consumir_eventos_celula.py:31` |
| Command `retratar_chamados_lei_cala` | `retratar()` — `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` |
| `reavaliar_chamados_geofence()` | `retratar()` em `app/ponto/services/geofence.py:419`; ação adicional **`reconciliar_geofence`** em `:422` |
| Command `reconciliar_vinculo` | `retratar()` em `app/ponto/management/commands/reconciliar_vinculo.py:68`; ação **`reconciliar_vinculo`** em `:74` |
| Command `processar_alertas_turno` | `retratar()` — `app/ponto/management/commands/processar_alertas_turno.py:216` |
| Command `reconciliar_grade` | `retratar()` — `app/ponto/management/commands/reconciliar_grade.py:300` |
| Command `reconciliar_fantasmas` | `retratar()` em `app/ponto/management/commands/reconciliar_fantasmas.py:279`; ação **`reconciliar_fantasma`** em `:289` |
| Command `supra_juiz` | Alias `_retr()` em `app/ponto/management/commands/supra_juiz.py:248`, condicionado pelo ramo em `:243` |
| Signal `_reconciliador_post_save_batida` | `reconciliar_chamados_colab()` — `app/ponto/signals.py:215` |

**Fechamentos por `reconciliar()` SEM a consulta dependente**

**Todas as linhas desta tabela são “não”**, salvo a última, que tem proteção alternativa. Usam `resolvido / sistema / estado_chamado`; a guarda comum é a premissa do dia, não a pergunta dependente. (`app/chamados/models.py:440`, `:444`, `:470`, `:474`)

| Função / sítio — evidência do caminho sem a consulta | Ação adicional do fechamento |
|---|---|
| Signal `disputa_fechada_fecha_chamado_pai()` — `app/chamados/signals.py:28`, `:42` | Trilha comum; nota `fechado_pela_disputa` em `:45` |
| `_materializar_ausencia_de_resposta()` — `app/chamados/services/materializacao.py:539` | Consulta e carimba perguntas **da disputa/data**, não todas as dependentes (`:507`, `:528`) |
| `sincronizar_adesao()`, filhos agrupados — `app/chamados/services/adesao.py:102`, `:115` | Trilha comum; nota `agrupado_adesao` em `:118` |
| `sincronizar_adesao()`, pai com batida — `app/chamados/services/adesao.py:142`, `:150` | Trilha comum; nota `adesao_fechada_por_batida` em `:152` |
| `limbo_folgas.aceitar_sugestao()` — `app/chamados/services/limbo_folgas.py:98`, `:101` | **`editar`** no `TipoEscala` em `:94`; CH usa trilha comum |
| `limbo_folgas.agrupar_furo()` — `app/chamados/services/limbo_folgas.py:216`, `:221` | Contexto `defeito_cadastro='folga_nao_definida'` em `:220`; CH usa trilha comum |
| `feriado_reavaliar.encerrar()` — `app/chamados/services/feriado_reavaliar.py:43`, `:47` | **`reconciliar_feriado`** em `:53` |
| `silencio_ausencia.reconciliar_por_ausencia()` — `app/chamados/services/silencio_ausencia.py:93`, `:101` | **`reconciliar_ausencia_lancada`** em `:108`; pode levantar silêncio em `:107` |
| Command `auditar_chamados_orfaos` — `app/chamados/management/commands/auditar_chamados_orfaos.py:38`, `:67` | Trilha comum; testa ausência de disputa aberta ligada ao pai (`:38`) |
| `fechar_adesao_com_batida()` — `app/colaboradores/services/pos_vinculo.py:42` | Trilha comum; nota em `:45` |
| `resolver_cobranca_da_entrada()` — `app/ponto/services/entrada_sincronizada.py:31` | Trilha comum; nota em `:36` |
| `_autoresolve_f2_f4()` — `app/ponto/services/triagem_batida.py:597` | Trilha comum; nota em `:602` |
| `designar_cobertura()` — `app/ponto/services/cobertura.py:114`, `:122` | Trilha comum; nota em `:132` |
| `declarar_cobertura_passada()` — `app/ponto/services/cobertura.py:310`, `:318` | Trilha comum; notas em `:324`, `:327` |
| `cartorio._julgar_colab_corpo()`, marcos acesos — `app/ponto/services/cartorio.py:1162`, `:1163` | Trilha comum |
| Mesma função, marcos inexistentes — `app/ponto/services/cartorio.py:1173`, `:1175` | Trilha comum; nota `marco_inexistente` em `:1182` |
| Mesma função, dia sem trabalho/sem furo — `app/ponto/services/cartorio.py:1188`, `:1196` | Trilha comum |
| `lastro.fechar()` — `app/ponto/services/lastro.py:306`, `:317` | Trilha comum; nota `lastro_encontrado` em `:322` |
| `cluster_espurio.reconciliar_colab()` — `app/ponto/services/cluster_espurio.py:79`, `:80` | Trilha comum |
| `fechar_chamado_da_ausencia()` — `app/ponto/services/ausencia.py:676`, `:678` | Trilha comum; nota `ausencia_decidida` em `:684` |
| `veredito_celula.fechar_o_fio_do_dia()` — `app/ponto/services/veredito_celula.py:174`, `:183` | Antes fecha perguntas por colaborador/data como admin (`:177`); chamador registra **`veredito_celula`** em `:333` |
| Command `detectar_ausencias.handle()`, limbo corrigido — `app/ponto/management/commands/detectar_ausencias.py:155`, `:158` | Trilha comum |
| Mesmo command, `_fechar_por_afastamento()` — `app/ponto/management/commands/detectar_ausencias.py:811` | Trilha comum; nota em `:816` |
| Mesmo command, `_fechar_por_cobertura()` — `app/ponto/management/commands/detectar_ausencias.py:821` | Trilha comum; nota em `:826` |
| Mesmo command, `_fechar_chamado()` — `app/ponto/management/commands/detectar_ausencias.py:832` | Trilha comum; nota em `:836` |
| Command `detectar_intervalo_ausente.handle()` — `app/ponto/management/commands/detectar_intervalo_ausente.py:134` | Trilha comum quando não está em dry-run (`:134`) |
| Command `curar_residuo_bug115._ata_sem_furo()` — `app/ponto/management/commands/curar_residuo_bug115.py:150`, `:151` | Trilha comum; nota em `:157` |
| Command `marcar_feriado_do_posto._marcar()` — `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` | Trilha comum; ação contextual **`marcar_feriado_do_posto`** no posto em `:118` |
| `cartorio._julgar_colab_corpo()`, fio duplicata — `app/ponto/services/cartorio.py:1266` | **Olha dependentes, mas não pelo predicado solicitado**: lê `perguntas_que_resolvem` em `:1260`, recusa qualquer texto em `:1263`, exige todas duplicatas em `:1265` |

No último ramo, `respondida_em` preenchida com texto vazio não é barrada pelo teste de texto. Além disso, os três fechamentos anteriores do cartório acontecem **antes** dessa proteção alternativa. (`app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, `:1263`)

Entradas adicionais para esses executores incluem: `fechar_cobranca_com_lastro → lastro.fechar()`; `detectar_cluster_espurio → reconciliar_colab()`; `apurar_furos_diarios → sincronizar_adesao(fechar=True)`; e `reavaliar_ausencias_feriado → feriado_reavaliar.encerrar()`. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`, `app/ponto/management/commands/detectar_cluster_espurio.py:46`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:32`)

`fechar_o_fio_do_dia()` também é usado pela declaração do dia; fechar chamados de ausência decidida delega ao executor da tabela. Nenhum desses atalhos acrescenta a consulta pelo destino. (`app/ponto/services/declarar_dia.py:198`, `app/ponto/services/veredito_celula.py:183`, `app/ponto/services/ausencia.py:713`)

**Transições humanas SEM consulta pelo destino**

Todas passam por `transicionar()` e recebem `via='admin'`, ação comum `estado_chamado`. A guarda de disputa é por `DisputaSupervisao.chamado=ch`, com perguntas dessa disputa. (`app/chamados/models.py:147`, `:183`, `:260`; `app/chamados/juizes.py:1817`, `:1820`)

| Função / sítio | Estado terminal | Ação adicional literal |
|---|---|---|
| `acao_aprovar_excecao()` — `app/chamados/services/acoes_chamado.py:70` | `resolvido` | **`aprovar_excecao`** em `:76` |
| `acao_responder_agenda()` — `app/chamados/services/acoes_chamado.py:108` | `resolvido` | `acao` em `:120`: **`aceitar_agenda` / `recusar_agenda`** (`app/chamados/views.py:460`) |
| `acao_responder_escala()` — `app/chamados/services/acoes_chamado.py:149` | `resolvido` | `acao` em `:161`: **`aceitar_escala` / `recusar_escala`** (`app/chamados/views.py:463`) |
| `acao_autorizar_geofence()` — `app/chamados/services/acoes_chamado.py:222` | `resolvido` | **`autoriz_geo`** em `:223` |
| `acao_recusar_geofence()` — `app/chamados/services/acoes_chamado.py:277` | `resolvido` | **`recusar_geo`** em `:278`; na batida, **`recusar_batida_fora_do_posto`** em `:269`; motivo livre obrigatório em `:254` |
| `acao_resposta_livre()` — `app/chamados/services/acoes_chamado.py:315` | Destino do POST (`:287`) | **`status`** em `:318`; havendo conteúdo, **`responder`** em `:295` |
| `processar_post_fio()` — `app/chamados/services/fio.py:715` | Destino informado, validado pelas choices (`:713`) | Trilha comum; nota `mensagem_dp` em `:712` |
| `avaliar_solicitacao_beneficio()` — `app/colaboradores/views.py:2068` | `resolvido` se aprovar; senão `fechado` | Trilha comum; motivo/resposta livres em `:2057`, `:2069` |
| Command `fechar_chamados_meio_periodo` — `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` | `fechado`, default da porta (`app/chamados/models.py:262`) | Resolução com **`MOTIVO='meio periodo declarado'`** (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:31`, `:110`) |
| Command `smoke_chamados._bloco_b_transicoes()` — `app/chamados/management/commands/smoke_chamados.py:134` | Destinos da matriz (`:130`) | Trilha comum |

As entradas HTTP dessas ações estão em `detalhe_chamado_local`, `fio_colaborador` e `modal_fio_colaborador`; elas delegam aos executores acima. (`app/chamados/views.py:427`, `:433`, `:461`, `:464`, `:541`, `:544`, `:547`, `:674`, `:954`)

**Cascatas, tranca e cancelamentos SEM consulta dependente**

| Função / sítio | Estado / via / ação | Motivo |
|---|---|---|
| `executar_desligamento()` — `app/colaboradores/services/desligamento.py:132`, `:137` | `fechado / cascade / estado_chamado`, pela porta em `app/chamados/models.py:1201` | Nota **`Colab desligado em {data_demissao} - fechamento automatico.`** (`app/colaboradores/services/desligamento.py:131`, `:139`) |
| `vinculo._executar_aceite()` — `app/colaboradores/services/vinculo.py:384`, `:387` | `fechado / cascade / estado_chamado` (`app/chamados/models.py:1201`) | Sem motivo passado no laço (`app/colaboradores/services/vinculo.py:387`) |
| `aprovar_justificativa_dp()` — `app/ponto/services/justificativa.py:121`, `:127` | `fechado / cascade / estado_chamado`; ação **`aprovar`** na Justificativa (`:108`) | Sem motivo passado a `encerrar()` (`:127`) |
| `materializar_saida_dp()` — `app/ponto/services/justificativa.py:274`, `:285` | `fechado / cascade / estado_chamado`; ação **`mat_saida_retro`** na Justificativa (`:293`) | Motivo recebido, incorporado à nota (`:287`, `:288`) |
| `competencia_trancada.encerrar()` — `app/chamados/services/competencia_trancada.py:174`, `:177` | `fechado / competencia_trancada / estado_chamado`; adicional **`encerrar_por_competencia_trancada`** (`:41`, `:47`, `:183`) | Competência e empresa na auditoria (`:186`, `:187`) |
| Command `fechar_chamados_vinculo_na_lista` — `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` | `cancelado / cancelado / estado_chamado` (`app/chamados/models.py:537`; `app/chamados/catalogo/motor.py:116`) | Literal formatado abaixo (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`) |
| Command `fechar_avisos_regua_na_lista` — `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` | `cancelado / cancelado / estado_chamado` (`app/chamados/models.py:537`; `app/chamados/catalogo/motor.py:116`) | Literal formatado abaixo (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`) |

Os dois motivos de cancelamento são:

- `'A revisao de vinculo virou linha da lista Cadastro x Realidade (C8: %s). O detector nao abre mais chamado; a correcao e no cadastro, pela lista.'` (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`)
- `'O aviso de cadastro do termometro virou linha da lista Cadastro x Realidade (C7: %s). O termometro nao abre mais chamado; a correcao e no cadastro, pela lista.'` (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`)

O command da tranca delega ao serviço acima; o serviço também pode ser disparado pelo ato de trancar a competência. (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`, `app/chamados/services/competencia_trancada.py:33`, `:292`)

**Fechamentos indiretos pela disputa**

`DisputaSupervisao.fechar()` consulta respostas sem veredito e perguntas **da própria disputa**, salva o fechamento e dispara o signal que pode resolver o chamado-pai. Não consulta todas as perguntas externas que apontam para esse chamado. O CH recebe `resolvido / sistema / estado_chamado`; a disputa registra **`acao='disputa_fechada'`**. (`app/chamados/models.py:1428`, `:1472`, `:1481`, `:1494`, `:1495`; `app/chamados/signals.py:42`)

| Sítio indireto | `motivo_fechamento` |
|---|---|
| `lavrar_sem_turno_aberto()` — `app/chamados/reconciliador.py:414` | Escolhido pela porta |
| `retratar_perguntas_absolvidas_por_celula()` — `app/chamados/reconciliador.py:567` | Escolhido pela porta |
| `encerrar_orfas_pela_lampada()` — `app/chamados/reconciliador.py:964`; consumidor em `:1148` | Escolhido pela porta |
| `criar_disputa_a_partir_de_chamados_orfaos()` — `app/chamados/services/disputa_emissao.py:1083` | Escolhido pela porta |
| `resolver_perguntas_por_chamado_resolvido()` — `app/chamados/services/disputa_emissao.py:1662` | Escolhido pela porta |
| `_materializar_ausencia_de_resposta()` — `app/chamados/services/materializacao.py:543` | Escolhido pela porta |
| `_fechar_disputa_se_quitada()` — `app/chamados/services/materializacao.py:1250` | Escolhido pela porta |
| `becos.fechar_beco()` — `app/chamados/services/becos.py:52` | Escolhido pela porta |
| Command `encerrar_perguntas_com_fato` — `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Escolhido pela porta |
| `entrada_sincronizada.resolver_cobranca_da_entrada()` — `app/ponto/services/entrada_sincronizada.py:59` | Escolhido pela porta |
| `resolver_disputas_de_foto()` — `app/api/foto_recebida.py:80` | Escolhido pela porta |
| `competencia_trancada.encerrar()` — `app/chamados/services/competencia_trancada.py:197` | **`competencia_trancada`** (`app/chamados/models.py:1359`) |
| `executar_desligamento()` — `app/colaboradores/services/desligamento.py:150` | **`desligamento`** (`app/chamados/models.py:1357`); os CH já são encerrados antes, em `app/colaboradores/services/desligamento.py:137` |
| Porta `DisputaSupervisao.admin_encerra()` — `app/

[truncado em 20000 de 23393 caracteres]

## 29/10 16:42 · sessao 0a8223573f6c · - · - · origem=Ronald (tmux)

Não existe encerramento direto de `j.chamado_origem` no fluxo comum de aprovação: ele só recebe uma ocorrência por `criar_followup_sistema_justificativa`; o encerramento explícito nesse fluxo procura outro chamado, pelo módulo `entrada_bloqueada_turno_aberto`. (`app/ponto/services/justificativa.py:111`, `app/ponto/services/justificativa.py:121`, `app/ponto/services/justificativa.py:127`, `app/chamados/utils.py:7`, `app/chamados/utils.py:21`)

Os caminhos existentes que podem encerrar `turno_aberto` são condicionais:

- **Materialização retroativa da saída:** `materializar_saida_dp` aprova a justificativa e percorre chamados do colaborador/data, incluindo `turno_aberto`, chamando `encerrar()`; não usa `chamado_origem` para localizar o alvo. (`app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:272`, `app/ponto/services/justificativa.py:276`, `app/ponto/services/justificativa.py:285`)
- **Batida materializada durante aprovação:** aprovação individual pode registrar saída antes de aprovar a justificativa; aprovação em lote pode registrar saída depois; o `post_save` de Batida chama `reconciliar_chamados_colab`. Esse reconciliador só chega a `_retratar` se sua própria coleta disser que a premissa morreu; para turno aberto consulta a existência do turno correspondente, não a aprovação da justificativa. (`app/ponto/services/justificativa.py:34`, `app/ponto/services/justificativa.py:69`, `app/ponto/services/justificativa.py:85`, `app/ponto/views.py:1787`, `app/ponto/views.py:1799`, `app/ponto/views.py:1806`, `app/ponto/signals.py:206`, `app/ponto/signals.py:214`, `app/chamados/reconciliador.py:264`, `app/chamados/reconciliador.py:286`, `app/chamados/reconciliador.py:1208`, `app/chamados/reconciliador.py:1214`)
- **Cartório disparado pelo save da justificativa:** `post_save(Justificativa)` rejulga o dia; no julgamento, pode chamar `reconciliar()` por marcos cobrados todos acesos, marcos inexistentes, protestos `COBRANCA_EM_DIA_SEM_TRABALHO`/`COBRANCA_SEM_FURO`, ou fio composto exclusivamente de perguntas duplicatas. Esses critérios do chamador são necessários além do gate do modelo. (`app/ponto/signals.py:161`, `app/ponto/signals.py:165`, `app/ponto/signals.py:89`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1174`, `app/ponto/services/cartorio.py:1175`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196`, `app/ponto/services/cartorio.py:1260`, `app/ponto/services/cartorio.py:1265`, `app/ponto/services/cartorio.py:1266`)
- **Consumidor de eventos da célula:** a lavra de veredito/ata enfileira evento; o consumidor chama `retratar_chamados_absolvidos_por_celula`, incluindo `turno_aberto`, somente se o veredito não acusa e não há marcos pendentes; `_retratar` ainda pode reter por respostas de irmãs aguardando consumo. O comando está configurado a cada minuto, com dreno adicional no reconciliador de quinze minutos. (`app/ponto/portas/celula.py:630`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:642`, `app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:697`, `app/chamados/reconciliador.py:336`, `app/chamados/reconciliador.py:792`, `app/chamados/reconciliador.py:800`, `app/chamados/reconciliador.py:823`, `app/chamados/reconciliador.py:825`, `app/chamados/reconciliador.py:435`, `app/chamados/reconciliador.py:454`, `app/chamados/reconciliador.py:1145`, `app/config/crons.py:375`, `app/config/crons.py:376`, `app/chamados/management/commands/reconciliar_chamados.py:60`, `app/chamados/management/commands/reconciliar_chamados.py:62`)
- **Fechamento da disputa vinculada:** signal da disputa fechada tenta `ch.reconciliar()`; a aprovação torna verdadeiro o coletor quando existe `justificativa_relacionada`, mas o evento necessário aqui continua sendo o fechamento da disputa. (`app/chamados/signals.py:28`, `app/chamados/signals.py:32`, `app/chamados/signals.py:41`, `app/chamados/signals.py:42`, `app/chamados/models.py:495`, `app/chamados/models.py:937`, `app/chamados/models.py:938`)
- **Transição humana:** a resposta livre pode chamar `transicionar(novo_status, user)`; a aprovação satisfaz o coletor usado pela guarda de resolução, sem executar essa transição por si. (`app/chamados/services/acoes_chamado.py:315`, `app/chamados/models.py:165`, `app/chamados/models.py:167`, `app/chamados/models.py:183`, `app/chamados/models.py:937`)

Para o chamado automático concreto, o contexto inicial contém apenas `data_turno`, `entrada_timestamp` e `posto`; a justificativa criada recebe `tipo='esqueci_saida'`, `status='pendente'` e `chamado_origem`, e o chamado recebe o ponteiro inverso. Portanto, inicialmente faltam os `marcos_faltantes` necessários para o encerramento por `marcos_acesos`. (`app/ponto/management/commands/processar_alertas_turno.py:335`, `app/ponto/management/commands/processar_alertas_turno.py:348`, `app/ponto/management/commands/processar_alertas_turno.py:391`, `app/ponto/management/commands/processar_alertas_turno.py:396`, `app/ponto/management/commands/processar_alertas_turno.py:398`, `app/ponto/management/commands/processar_alertas_turno.py:399`, `app/ponto/management/commands/processar_alertas_turno.py:423`, `app/ponto/services/lastro.py:75`, `app/ponto/services/lastro.py:170`, `app/ponto/services/lastro.py:171`)

Se a aprovação não produzir saída e o chamado continuar com fio vivo, pode permanecer aberto: sem protestos e com `_com_fio`, o cartório escreve `cobrado`; esse veredito não permite o caminho de absolvição da célula. O retorno `True` de `premissa_regularizada(para='folha')` só autoriza um chamador que efetivamente tente encerrar. (`app/ponto/services/justificativa.py:34`, `app/ponto/services/justificativa.py:85`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1141`, `app/ponto/services/cartorio.py:1142`, `app/chamados/reconciliador.py:823`, `app/chamados/models.py:434`, `app/chamados/models.py:440`, `app/chamados/models.py:444`, `app/chamados/models.py:495`, `app/chamados/models.py:937`, `app/chamados/models.py:938`)

A rede `fechar_cobranca_com_lastro` não é encerrador desse módulo: sua consulta filtra exclusivamente `batida_ausente`. (`app/ponto/services/lastro.py:216`, `app/ponto/services/lastro.py:217`)

## 29/10 16:42 · sessao 54ff1da61f71 · - · - · origem=Ronald (tmux)

Métodos do próprio `ChamadoColaborador`:

| Método | Mudança | Gate antes da escrita |
|---|---|---|
| `transicionar()` | Vivo → `resolvido`/`fechado` | Consulta motor/perfil; para `fechado`, barra justificativa pendente; para ambos, barra disputa viva; para `resolvido` de furo, exige `premissa_regularizada()` com parâmetro padrão `cobranca` (há fallback legado sem dia). `app/chamados/models.py:122`, `app/chamados/models.py:130`, `app/chamados/models.py:142`, `app/chamados/models.py:165`. |
| `fechar()` | Mesmo comportamento | Delega diretamente a `transicionar()`. `app/chamados/models.py:262`. |
| `reconciliar()` | Vivo → `resolvido` | Recarrega estado se divergir do banco; consulta motor `RECONCILIAR`; pergunta `_gate_premissa_do_fechamento()`. O argumento `novo_status` não é usado; o destino vem do motor. `app/chamados/models.py:434`, `app/chamados/models.py:453`, `app/chamados/catalogo/motor.py:111`. |
| `encerrar()` | Vivo → `fechado` | Apenas motor `ENCERRAR`; não pergunta justificativa, disputa ou regularização da premissa. `app/chamados/models.py:1192`, `app/chamados/catalogo/motor.py:114`. |
| `encerrar_por_competencia_trancada()` | Vivo → `fechado` | Exige estado em `VIVOS` e motor `ENCERRAR`; o método não verifica se a competência está trancada. `app/chamados/models.py:1204`. |
| `_aplicar()` / `_gravar_estado()` | Escritores internos | `_aplicar()` delega ao núcleo; `_gravar_estado()` não pergunta regularização, exige via válida para terminal e salva com trilha em `atomic`. `app/chamados/models.py:185`, `app/chamados/models.py:204`, `app/chamados/models.py:226`, `app/chamados/models.py:257`. |

`cancelar()` e `superar()` também retiram vivos de circulação, para `cancelado` e `superado`; tecnicamente esses estados estão em `TERMINAIS`, não em `ENCERRADOS`, que contém apenas `resolvido`/`fechado`. `cancelar()` consulta somente o motor; `superar()` também exige sucessor existente, diferente do próprio chamado e não terminal, gravando o vínculo e o estado em `atomic`. `app/chamados/models.py:527`, `app/chamados/models.py:543`, `app/chamados/catalogo/motor.py:30`.

O gate de `reconciliar()` permite fechar quando:

- Não é chamado de furo. `app/chamados/models.py:484`.
- Não consegue determinar o dia: permite, registrando `fechamento_sem_dia_do_fato`. `app/chamados/models.py:488`.
- `premissa_regularizada(para='folha')` é verdadeira ou está agrupado sob pai vivo. `app/chamados/models.py:495`.
- Existe outra cobrança viva de furo do mesmo colaborador/dia: permite como deduplicação com trilha. `app/chamados/models.py:498`.
- Fora dessas condições, recusa e registra `fechamento_recusado`. `app/chamados/models.py:511`.

`premissa_regularizada(para='folha')` responde **`True` imediatamente** quando `justificativa_relacionada.status == 'aprovada'`, antes de consultar dia, célula, turno ou parâmetro `para`. `app/chamados/models.py:937`.

Sobre o fechamento após aprovação:

- A aprovação DP comum não encerra `j.chamado_origem`; apenas cria nota nesse chamado. O helper lê o ponteiro e anexa ocorrência, sem transição de estado. `app/ponto/services/justificativa.py:111`, `app/chamados/utils.py:7`, `app/chamados/utils.py:21`.
- O fechamento explícito da aprovação DP comum procura outro módulo: `entrada_bloqueada_turno_aberto`, por colaborador, e chama `encerrar()`. Portanto não alcança `modulo_origem='turno_aberto'`. `app/ponto/services/justificativa.py:121`.
- **Existe caminho explícito na materialização retroativa:** `materializar_saida_dp()` aprova a justificativa e encerra chamados de módulos incluindo `turno_aberto`, por colaborador e data do contexto, com fallback para criação quando contexto sem data; não usa `chamado_origem`. `app/ponto/services/justificativa.py:259`, `app/ponto/services/justificativa.py:274`, `app/ponto/services/justificativa.py:276`, `app/ponto/services/justificativa.py:285`.
- **Existe caminho indireto pela batida gerada:** aprovação DP, quando tem saída informada e entrada, chama `registrar_batida`; o signal da batida chama `reconciliar_chamados_colab()`, que pode retratar o chamado se seu turno físico deixou de estar aberto; `_retratar()` chama `reconciliar()` em `atomic`, salvo retenção por perguntas respondidas sem consumo. Esse evento pode ocorrer antes da gravação de `j.status='aprovada'`. `app/ponto/services/justificativa.py:34`, `app/ponto/services/justificativa.py:69`, `app/ponto/services/justificativa.py:85`, `app/ponto/signals.py:206`, `app/ponto/signals.py:214`, `app/chamados/reconciliador.py:264`, `app/chamados/reconciliador.py:1208`, `app/chamados/reconciliador.py:435`, `app/chamados/reconciliador.py:455`.
- O signal de `Justificativa` somente rejulga o dia; não chama diretamente o reconciliador de chamados. `app/ponto/signals.py:161`.
- O cartório pode fechar chamados com todos os marcos nomeados acesos, ou marcos tornados inexistentes, ou protestos `COBRANCA_EM_DIA_SEM_TRABALHO`/`COBRANCA_SEM_FURO`, ou fio inteiramente duplicado, sempre via `reconciliar()`. `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1174`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1265`.
- Contudo, o `turno_aberto` automático do cron nasce com `data_turno`, `entrada_timestamp` e `posto`, sem `marcos_faltantes`; por isso não entra no fechamento por marcos acesos ou inexistentes. `app/ponto/management/commands/processar_alertas_turno.py:335`, `app/ponto/management/commands/processar_alertas_turno.py:348`, `app/ponto/services/lastro.py:170`, `app/ponto/services/lastro.py:110`.
- A justificativa automática nasce `esqueci_saida`, `pendente`, com `chamado_origem`, e o cron aponta `chamado.justificativa_relacionada` somente quando a justificativa foi criada; a porta preserva ponteiro previamente existente. `app/ponto/management/commands/processar_alertas_turno.py:389`, `app/ponto/management/commands/processar_alertas_turno.py:402`, `app/ponto/management/commands/processar_alertas_turno.py:423`, `app/chamados/services/vinculos.py:81`.
- A aprovação dispensa lâmpadas na ata, mas um chamado vivo com fio pode manter o dia `cobrado`; o consumidor de eventos só retrata chamado quando o veredito da célula não acusa e não tem marcos pendentes. Portanto ele não garante fechamento desse cenário. `app/ponto/services/cartorio.py:600`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1141`, `app/chamados/reconciliador.py:814`, `app/chamados/reconciliador.py:823`.
- O reconciliador periódico também exige a morte da premissa física antes de chamar `_retratar()`, não usa a aprovação da justificativa como seletor; aprovação sem saída materializada pode deixar o turno aberto e não alcançar essa chamada, apesar de o gate individual já permitir o fechamento. `app/chamados/reconciliador.py:264`, `app/chamados/reconciliador.py:287`, `app/chamados/reconciliador.py:1208`, `app/chamados/models.py:937`.

A conclusão precisa é: **não há fechamento dedicado, pelo vínculo `chamado_origem`, disparado simplesmente pela aprovação; há caminhos condicionais pela batida/cartório/célula e o fechamento explícito da ferramenta de saída retroativa.** `app/chamados/utils.py:7`, `app/ponto/signals.py:161`, `app/ponto/services/justificativa.py:121`, `app/ponto/services/justificativa.py:274`.

## 29/10 16:42 · sessao 89489b6f5983 · - · - · origem=Ronald (tmux)

Foram encontrados **3 sítios explícitos de aprovação** de `ponto.models.Justificativa` em produção: `app/ponto/services/justificativa.py:85`, `app/ponto/services/justificativa.py:262` e `app/ponto/views.py:1787`.

| Função e gravação | `transaction.atomic` | Efeito sobre chamados |
|---|---|---|
| `aprovar_justificativa_dp`, definida em `app/ponto/services/justificativa.py:16`; atribui em `:85` e salva em `:88`. | Sim, bloco iniciado em `app/ponto/services/justificativa.py:23`; `select_for_update` em `:24`. | Não altera `chamado_origem` nem `justificativa_relacionada`. Depois do bloco, chama o followup em `app/ponto/services/justificativa.py:112`: o helper lê `justificativa.chamado_origem` e anexa somente uma ocorrência (`app/chamados/utils.py:7`, `:21`). Tenta encerrar o chamado mais recente do colaborador com módulo **`entrada_bloqueada_turno_aberto`**, via `.encerrar()`, sem condição de vínculo com a justificativa (`app/ponto/services/justificativa.py:121`, `:123`, `:127`). |
| `materializar_saida_dp`, definida em `app/ponto/services/justificativa.py:214`; atribui em `:262` e salva em `:265`. | Sim, bloco iniciado em `app/ponto/services/justificativa.py:224`; `select_for_update` em `:225`. | Não altera os vínculos. Seleciona chamados do mesmo colaborador, mesmo `contexto_json.data_turno` ou fallback de `criado_em`, módulos `turno_aberto`, `turno_aberto_24h`, `batida_ausente`, `saida_sem_entrada`, estados `NAO_FECHADOS`, e chama `.encerrar()` em cada um; não depende de `chamado_origem` nem de `justificativa_relacionada` (`app/ponto/services/justificativa.py:274`, `:276`, `:277`, `:279`, `:281`, `:284`, `:285`). |
| `aprovar_lote_justificativas`, definida em `app/ponto/views.py:1763`; atribui em `:1787` e salva em `:1790`. | Não há bloco ou decorador `atomic` nessa função; o decorador é `login_required` (`app/ponto/views.py:1762`, `:1763`, `:1784`, `:1790`, `:1839`). Os bancos de produção e SaaS também não habilitam `ATOMIC_REQUESTS` (`app/config/settings/prod.py:9`, `app/config/settings/saas.py:83`). | O corpo não consulta, altera ou encerra chamados; invalida fechamento e pode materializar a batida de saída (`app/ponto/views.py:1795`, `:1798`, `:1806`, `:1815`, `:1839`). |

**Efeito comum indireto:** os três `save()` disparam o receiver `_cartorio_post_save_justificativa`, que chama `_julgar_dias` para `instance.data_turno`; `_julgar_dias` chama `cartorio.julgar_celula` (`app/ponto/signals.py:161`, `:162`, `:165`, `:85`, `:93`). Esse receiver não chama diretamente o reconciliador de chamados; a chamada `reconciliar_chamados_colab` pertence ao receiver de **Batida** (`app/ponto/signals.py:206`, `:207`, `:214`, `:215`).

As outras construções de justificativa examinadas criam/regravam `pendente`: `create` na view (`app/ponto/views.py:1635`, `:1642`), `create` na API (`app/api/views.py:2059`, `:2064`), `get_or_create` da regularização (`app/ponto/services/regularizacao.py:91`, `:99`), `get_or_create` do cron (`app/ponto/management/commands/processar_alertas_turno.py:391`, `:398`) e `update_or_create` da triagem (`app/ponto/services/triagem_batida.py:258`, `:266`). Não encontrei `update`, `create` ou `get_or_create/defaults` explícito que escreva `aprovada`; os únicos literais de atribuição encontrados são os três acima (`app/ponto/services/justificativa.py:85`, `:262`, `app/ponto/views.py:1787`).

**Ressalva do Django admin:** `JustificativaAdmin` herda diretamente de `admin.ModelAdmin`, sem restrição local de edição ou `readonly_fields` (`app/ponto/admin.py:18`, `:19`, `:20`, `:22`); o campo `status` oferece `aprovada` e é editável no modelo (`app/ponto/models.py:130`, `:132`, `:159`). Portanto há uma porta genérica de edição/criação pelo admin, além dos três escritores explícitos; o método herdado de Django e seu bloco transacional não estão no código `app/` inspecionado **[não verificado]**.

## 29/10 16:41 · sessao 03a4f5633c08 · - · - · origem=Ronald (tmux)

Censo externo concluído; nenhum arquivo alterado.

Todos os sítios abaixo gravam pelo núcleo `_gravar_estado`, que audita `acao='estado_chamado'` e `depois={'status_local': novo, 'via': via}` (`app/chamados/models.py:192`, `app/chamados/models.py:227`). As vias literais são `admin`, `sistema`, `cascade`, `cancelado`, `superado` (`app/chamados/catalogo/motor.py:88`, `app/chamados/catalogo/motor.py:95`).

| Função/sítio externo | Guarda de pergunta dependente | Via / registro adicional |
|---|---|---|
| `avaliar_solicitacao_beneficio` → `transicionar('resolvido' if aprovar else 'fechado')` — `app/colaboradores/views.py:2046`, `app/colaboradores/views.py:2068` | **Não** consulta pelo FK `chamado_destino`; a guarda humana consulta disputa cujo `chamado=este`, em `app/chamados/models.py:147`, `app/chamados/juizes.py:1817` | `admin`; nota `tipo='beneficio_avaliado'` — `app/colaboradores/views.py:2071` |
| `executar_desligamento` → `encerrar()` — `app/colaboradores/services/desligamento.py:137` | **Não**; seleção dos chamados e fechamento direto em `app/colaboradores/services/desligamento.py:132` | `cascade`; nota `tipo='desligamento_cascata'`, texto `Colab desligado em {data_demissao} - fechamento automatico.` — `app/colaboradores/services/desligamento.py:131`, `app/colaboradores/services/desligamento.py:139` |
| `_executar_aceite` → `encerrar()` — `app/colaboradores/services/vinculo.py:373`, `app/colaboradores/services/vinculo.py:387` | **Não**; fecha chamados vivos da categoria `escala_aceite` — `app/colaboradores/services/vinculo.py:384` | `cascade`; não há motivo de fechamento nesse laço — `app/colaboradores/services/vinculo.py:384` |
| `aprovar_justificativa_dp` → `encerrar()` — `app/ponto/services/justificativa.py:16`, `app/ponto/services/justificativa.py:127` | **Não**; procura último `entrada_bloqueada_turno_aberto` do colaborador — `app/ponto/services/justificativa.py:121` | `cascade`; log adicional `acao="aprovar"` em Justificativa — `app/ponto/services/justificativa.py:108` |
| `materializar_saida_dp` → `encerrar()` — `app/ponto/services/justificativa.py:214`, `app/ponto/services/justificativa.py:285` | **Não**; filtra colaborador, dia e módulos `turno_aberto`, `turno_aberto_24h`, `batida_ausente`, `saida_sem_entrada` — `app/ponto/services/justificativa.py:274` | `cascade`; nota `tipo='turno_fechado_retroativo'` — `app/ponto/services/justificativa.py:287`; log adicional `'mat_saida_retro'` — `app/ponto/services/justificativa.py:293` |
| `fechar_chamados_vinculo_na_lista.Command.handle` → `cancelar()` — `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` | **Não**; prova presença na lista C8, sem pergunta — `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35` | `cancelado`; motivo literal formatado: `'A revisao de vinculo virou linha da lista Cadastro x Realidade (C8: %s). O detector nao abre mais chamado; a correcao e no cadastro, pela lista.'` — `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| `fechar_avisos_regua_na_lista.Command.handle` → `cancelar()` — `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` | **Não**; prova presença na lista C7 — `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27` | `cancelado`; motivo `'O aviso de cadastro do termometro virou linha da lista Cadastro x Realidade (C7: %s). O termometro nao abre mais chamado; a correcao e no cadastro, pela lista.'` — `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` |
| `fechar_chamados_meio_periodo.Command.handle` → `fechar()` — `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` | **Não** pelo FK dependente; passa pela guarda humana — `app/chamados/models.py:264`, `app/chamados/juizes.py:1817` | `admin`; resolução começa com literal `MOTIVO='meio periodo declarado'` — `app/ponto/management/commands/fechar_chamados_meio_periodo.py:31`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110` |

Todos os seguintes chamam `reconciliar()` cru, **sem veto próprio da dependente respondida/não validada**; o modelo chama somente `_decisao_do_fechamento` → `_gate_premissa_do_fechamento` antes de gravar via `sistema` (`app/chamados/models.py:440`, `app/chamados/models.py:444`, `app/chamados/models.py:470`):

| Função/sítio | Nota / literal adicional |
|---|---|
| `fechar_adesao_com_batida` — `app/colaboradores/services/pos_vinculo.py:42` | `tipo='adesao_fechada_por_batida'` — `app/colaboradores/services/pos_vinculo.py:45` |
| `resolver_cobranca_da_entrada` — `app/ponto/services/entrada_sincronizada.py:31` | `tipo='entrada_sincronizada_resolveu'` — `app/ponto/services/entrada_sincronizada.py:36` |
| `_autoresolve_f2_f4` — `app/ponto/services/triagem_batida.py:597` | `tipo='batida_resolveu'` — `app/ponto/services/triagem_batida.py:602` |
| `designar_cobertura` — `app/ponto/services/cobertura.py:122` | `tipo='cobertura_designada'` — `app/ponto/services/cobertura.py:132` |
| `declarar_cobertura_passada` — `app/ponto/services/cobertura.py:318` | `tipo='cobertura_designada'` ou `'coberto_a_confirmar'` — `app/ponto/services/cobertura.py:324`, `app/ponto/services/cobertura.py:327` |
| `_julgar_colab_corpo`, marcos acesos — `app/ponto/services/cartorio.py:1163` | Sem nota nesse bloco — `app/ponto/services/cartorio.py:1164` |
| `_julgar_colab_corpo`, marcos inexistentes — `app/ponto/services/cartorio.py:1175` | `tipo='marco_inexistente'` — `app/ponto/services/cartorio.py:1182` |
| `_julgar_colab_corpo`, dia sem trabalho/sem furo — `app/ponto/services/cartorio.py:1196` | Sem nota nesse bloco — `app/ponto/services/cartorio.py:1197` |
| `lastro.fechar` — `app/ponto/services/lastro.py:317` | `tipo='lastro_encontrado'` — `app/ponto/services/lastro.py:322` |
| `cluster_espurio.reconciliar_colab` — `app/ponto/services/cluster_espurio.py:80` | Sem motivo/nota no laço — `app/ponto/services/cluster_espurio.py:79` |
| `fechar_chamado_da_ausencia` — `app/ponto/services/ausencia.py:678` | `tipo='ausencia_decidida'` — `app/ponto/services/ausencia.py:684` |
| `detectar_ausencias.Command.handle`, limbo corrigido — `app/ponto/management/commands/detectar_ausencias.py:158` | Sem nota nesse sítio — `app/ponto/management/commands/detectar_ausencias.py:155` |
| `_fechar_por_afastamento` — `app/ponto/management/commands/detectar_ausencias.py:811` | `tipo='fechado_por_afastamento'` — `app/ponto/management/commands/detectar_ausencias.py:816` |
| `_fechar_por_cobertura` — `app/ponto/management/commands/detectar_ausencias.py:821` | `tipo='fechado_por_cobertura'` — `app/ponto/management/commands/detectar_ausencias.py:826` |
| `_fechar_chamado` — `app/ponto/management/commands/detectar_ausencias.py:832` | `tipo='fechado_por_batida'` — `app/ponto/management/commands/detectar_ausencias.py:836` |
| `detectar_intervalo_ausente.Command.handle` — `app/ponto/management/commands/detectar_intervalo_ausente.py:134` | Sem nota nesse sítio — `app/ponto/management/commands/detectar_intervalo_ausente.py:134` |
| `curar_residuo_bug115.Command._ata_sem_furo` — `app/ponto/management/commands/curar_residuo_bug115.py:151` | `tipo='lastro_ata_sem_furo'` — `app/ponto/management/commands/curar_residuo_bug115.py:157` |
| `marcar_feriado_do_posto.Command._marcar` — `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` | `tipo='feriado_do_posto'` — `app/colaboradores/management/commands/marcar_feriado_do_posto.py:131` |

Duas particularidades:

- `cartorio._julgar_colab_corpo` **olha dependentes por regra alternativa** no sítio `app/ponto/services/cartorio.py:1266`: carrega `x.perguntas_que_resolvem` (`app/ponto/services/cartorio.py:1260`), recusa qualquer texto de resposta (`app/ponto/services/cartorio.py:1263`) e exige todas com `VIA_DUPLICATA` (`app/ponto/services/cartorio.py:1265`). Não implementa o triplo exato `respondida_em != null`, `validada_em == null`, FK destino: resposta carimbada com texto vazio escapa desse veto (`app/ponto/services/cartorio.py:1263`).
- `fechar_o_fio_do_dia` fecha perguntas vivas por colaborador/data com `fechar_por_admin`, depois chama `reconciliar()` nos chamados (`app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:177`, `app/ponto/services/veredito_celula.py:183`). **Não veta** resposta dependente aguardando validação; o escritor da pergunta grava `via_resolucao=VIA_ADMIN_DECIDIU` e `materializada_em`, sem validar (`app/chamados/models.py:2131`). Nota `tipo='dia_decidido_pelo_admin'` (`app/ponto/services/veredito_celula.py:186`); chamador `veredito_celula` registra `acao='veredito_celula'` (`app/ponto/services/veredito_celula.py:333`).

Os seguintes usam `retratar()`, portanto **SIM, por delegação**: `_retratar` chama `irmas_respondidas_sem_consumo` antes de reconciliar (`app/chamados/reconciliador.py:435`, `app/chamados/reconciliador.py:456`). A consulta filtra FK destino, respondida e não validada, **mais** `materializada_em IS NULL` e resposta textual não vazia (`app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1477`). Via `sistema`, nota `tipo='retratado'`; retenção audita `'retratacao_retida_irma_orfa'` (`app/chamados/reconciliador.py:449`, `app/chamados/reconciliador.py:473`).

| Chamador externo de `retratar` | Log adicional |
|---|---|
| `reavaliar_chamados_geofence` — `app/ponto/services/geofence.py:419` | `'reconciliar_geofence'` — `app/ponto/services/geofence.py:422` |
| `reconciliar_vinculo.Command.handle` — `app/ponto/management/commands/reconciliar_vinculo.py:68` | `'reconciliar_vinculo'` — `app/ponto/management/commands/reconciliar_vinculo.py:74` |
| `processar_alertas_turno` — `app/ponto/management/commands/processar_alertas_turno.py:216` | Apenas núcleo/unidade retratar nesse bloco — `app/ponto/management/commands/processar_alertas_turno.py:216` |
| `reconciliar_grade.Command.handle` — `app/ponto/management/commands/reconciliar_grade.py:300` | Apenas núcleo/unidade retratar nesse bloco — `app/ponto/management/commands/reconciliar_grade.py:300` |
| `reconciliar_fantasmas.Command.handle` — `app/ponto/management/commands/reconciliar_fantasmas.py:279` | `'reconciliar_fantasma'` — `app/ponto/management/commands/reconciliar_fantasmas.py:289` |
| `supra_juiz.Command.handle`, alias `_retr` — `app/ponto/management/commands/supra_juiz.py:248` | Texto dinâmico por `motivo_retratacao`, sem argumento `motivo`; condição `executar` e trava desativada (`False`) — `app/ponto/management/commands/supra_juiz.py:41`, `app/ponto/management/commands/supra_juiz.py:243` |

Sobre Justificativa: **aprovar não encerra explicitamente `j.chamado_origem`**. O helper chamado após aprovação só acrescenta followup ao origem (`app/ponto/services/justificativa.py:112`, `app/chamados/utils.py:7`, `app/chamados/utils.py:21`). Entretanto, o `j.save()` dispara `_cartorio_post_save_justificativa`, que rejulga o dia e pode fechá-lo pelos sítios do cartório acima (`app/ponto/services/justificativa.py:88`, `app/ponto/signals.py:161`, `app/ponto/signals.py:165`, `app/ponto/signals.py:93`). A aprovação em lote também salva e dispara esse receiver (`app/ponto/views.py:1787`, `app/ponto/views.py:1790`). O fechamento explícito da aprovação individual seleciona outro alvo por módulo/colaborador, sem usar `chamado_origem` (`app/ponto/services/justificativa.py:121`).

## 29/10 16:40 · sessao f8afdfacdddc · - · - · origem=Ronald (tmux)

Censo em `app/chamados/`, excluindo modelos/catalogo, testes e migrações. As portas comuns e seus literais ficam em `models.py`; abaixo os sítios chamadores e os valores adicionais. [nao verificado]

O único juiz anterior ao fechamento que consulta explicitamente a FK dependente é `irmas_respondidas_sem_consumo`: filtra `respondida_em__isnull=False`, `validada_em__isnull=True`, `materializada_em__isnull=True`, `chamado_destino=ch` **ou crédito ao pai da disputa**, e exige texto não vazio. Portanto é uma guarda mais estreita que o predicado literal do pedido: `app/chamados/juizes.py:1439`, `:1471–1477`.

| Função/sítio que altera CH via porta | Dependente antes? | Valores adicionais |
|---|---|---|
| `reconciliador._retratar`, chamada `ch.reconciliar()` `app/chamados/reconciliador.py:456` | **Sim**, juiz em `:435`; recusa retorna antes em `:454` | Sucesso usa trilha comum `estado_chamado`; nota `tipo='retratado'` `:471–473`. Recusa: `acao='retratacao_retida_irma_orfa'` `:449`, `depois={'perguntas_pendentes': ...}` `:453`. |
| `services.materializacao._reconciliar_destino_e_clones._fecha`, `alvo.reconciliar()` `app/chamados/services/materializacao.py:711` | **Sim**, `:702`; aplica ao destino `:714` e clones `:721` | Recusa `acao='destino_retido_irma_orfa'` `:704`; valores `perguntas_pendentes`, `gatilho_pergunta_pk` `:708–709`; sucesso só trilha comum. |
| signal `disputa_fechada_fecha_chamado_pai`, `ch.reconciliar()` `app/chamados/signals.py:42` | **Não**, corpo `:28–45`; só checa CH e disputa fechada | Nota `tipo='fechado_pela_disputa'` `:45`; trilha comum. |
| `services.materializacao._materializar_ausencia_de_resposta`, `q.chamado_destino.reconciliar()` `app/chamados/services/materializacao.py:539` | **Não**. Itera perguntas da disputa/data `:507–508`, valida/materializa essas perguntas `:509–530`; não consulta todas dependentes do CH | Pergunta grava `via_resolucao` escolhida entre `VIA_DECLARACAO` / `VIA_AUSENCIA_DIA` `:526–527`; CH trilha comum. |
| `services.adesao.sincronizar_adesao`, filhos `f.reconciliar()` `app/chamados/services/adesao.py:115` | **Não**, seleção/contexto `:102–115` | Contexto `agrupado_em` `:112`; nota `tipo='agrupado_adesao'` `:118`; trilha comum. |
| Mesma função, pai `pai.reconciliar()` `app/chamados/services/adesao.py:150` | **Não**, consulta pais e batidas `:142–150` | Nota `tipo='adesao_fechada_por_batida'` `:152`; trilha comum. |
| `services.limbo_folgas.aceitar_sugestao`, `ch.reconciliar()` `app/chamados/services/limbo_folgas.py:101` | **Não**, seleção por template `:98–101` | Log adicional `acao='editar'`, modelo `TipoEscala` `:94`; nota `tipo='folga_gravada_limbo'` `:103`; trilha comum CH. |
| `services.limbo_folgas.agrupar_furo`, `furo.reconciliar()` `app/chamados/services/limbo_folgas.py:221` | **Não**, corpo `:216–225` | Contexto `agrupado_em`, `defeito_cadastro='folga_nao_definida'` `:219–220`; nota `tipo='agrupado_limbo'` `:225`; trilha comum. |
| `services.feriado_reavaliar.encerrar`, `ch.reconciliar()` `app/chamados/services/feriado_reavaliar.py:47` | **Não**, corpo `:43–56` | `acao='reconciliar_feriado'` `:53`; nota `tipo='falta_espuria_feriado'` `:51`; trilha comum. |
| `services.silencio_ausencia.reconciliar_por_ausencia`, `ch.reconciliar()` `app/chamados/services/silencio_ausencia.py:101` | **Não**, consulta ausência e premissa `:93–101` | `acao='reconciliar_ausencia_lancada'` `:108`; nota `tipo='falta_redundante_ausencia'` `:105`. Pode levantar silêncio com `acao='levantar_silencio_ausencia'` `:72`, motivo textual `:107`. |
| `services.regularizacao_ext.materializar_regularizacao_externa`, `reg.chamado.encerrar()` `app/chamados/services/regularizacao_ext.py:205` | **Não**, trecho final `:197–205` | Nota `tipo='regularizacao_confirmada'` `:203–204`; trilha comum da porta. |
| `services.competencia_trancada.encerrar`, `ch.encerrar_por_competencia_trancada()` `app/chamados/services/competencia_trancada.py:177` | **Não**, seleciona IDs medidos e encerra `:174–177` | `via='competencia_trancada'` constante `:41`; `acao='encerrar_por_competencia_trancada'` constante `:47`, usada `:183`; nota `tipo='competencia_trancada'` `:182`. |
| command `auditar_chamados_orfaos.Command.handle`, `ch.reconciliar()` `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | **Não**, só exige ausência de disputa aberta ligada ao pai `:38–43` | Nota `tipo='auditoria_orfaos'` `:72–75`; trilha comum. |

Portas humanas: todas as chamadas abaixo passam por `transicionar`, cuja guarda examina apenas disputa com `chamado=ch` e suas perguntas (`app/chamados/models.py:142–155`; `app/chamados/juizes.py:1817–1822`), **não perguntas externas com `chamado_destino=ch`**.

| Função | Escrita terminal | Log adicional literal |
|---|---|---|
| `acao_aprovar_excecao` | `resolvido`, `app/chamados/services/acoes_chamado.py:70` | `aprovar_excecao` `:76` |
| `acao_responder_agenda` | `resolvido`, `app/chamados/services/acoes_chamado.py:108` | `acao` variável `:120`; domínio `aceitar_agenda`/`recusar_agenda` na view `app/chamados/views.py:460–461` |
| `acao_responder_escala` | `resolvido`, `app/chamados/services/acoes_chamado.py:149` | `acao` variável `:161`; domínio `aceitar_escala`/`recusar_escala` na view `app/chamados/views.py:463–464` |
| `acao_autorizar_geofence` | `resolvido`, `app/chamados/services/acoes_chamado.py:222` | `autoriz_geo` `:223`; motivo de Advertencia é resolução do POST ou texto montado `:188–196` |
| `acao_recusar_geofence` | `resolvido`, `app/chamados/services/acoes_chamado.py:277` | `recusar_geo` `:278`; retratação da batida `recusar_batida_fora_do_posto` `:269`; motivo é POST obrigatório `:254–256` |
| `acao_resposta_livre` | `novo_status` do POST, `app/chamados/services/acoes_chamado.py:315` | `status` `:318`; se conteúdo, `responder` `:295` |
| `processar_post_fio` | `novo_status`, `app/chamados/services/fio.py:715` | Sem log específico nesse ramo `:702–731`; nota `tipo='mensagem_dp'` `:712` |
| command smoke `_bloco_b_transicoes` | destino da matriz, `app/chamados/management/commands/smoke_chamados.py:130–134` | Só trilha comum; `:147` e `:163` têm destinos vivos e não são fechadores (`:144`, `:158`) |

Roteadoras das ações humanas: `detalhe_chamado_local` delega em `app/chamados/views.py:427`, `:433`, `:461`, `:464`, `:541`, `:544`, `:547`; `fio_colaborador` em `:674`; `modal_fio_colaborador` em `:954`. O fio também roteia as ações geofence `app/chamados/services/fio.py:539–540`.

Rotas delegadas que herdam **SIM** de `_retratar`: wrapper `retratar` `app/chamados/reconciliador.py:477–479`; `retratar_chamados_absolvidos_por_celula` `:800`; `reconciliar_chamados_colab` `:1214`; consumidor de evento de célula `:1145`; command `reconciliar_chamados` `app/chamados/management/commands/reconciliar_chamados.py:52`, `:62`; command `retratar_chamados_lei_cala` `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`. Nenhum deles acrescenta outro literal de `LogAuditoria.acao` no fechamento; `_retratar` escreve nota de `tipo='retratado'` (`app/chamados/reconciliador.py:471–473`).

Fechamentos indiretos de disputa que acionam o signal do CH, logo **não fazem a consulta por dependente do CH**: a porta de disputa consulta suas próprias perguntas (`app/chamados/models.py:1472–1489`), e o signal chama `reconciliar()` (`app/chamados/signals.py:42`).

- `reconciliador.lavrar_sem_turno_aberto` `app/chamados/reconciliador.py:414`.
- `retratar_perguntas_absolvidas_por_celula` `app/chamados/reconciliador.py:567`.
- `encerrar_orfas_pela_lampada` `app/chamados/reconciliador.py:964`; entrada pelo consumidor `:1148`.
- `services.disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos` `app/chamados/services/disputa_emissao.py:1083`.
- `resolver_perguntas_por_chamado_resolvido` `app/chamados/services/disputa_emissao.py:1662`.
- `services.materializacao._materializar_ausencia_de_resposta` `app/chamados/services/materializacao.py:543`.
- `_fechar_disputa_se_quitada` `app/chamados/services/materializacao.py:1250`.
- `services.becos.fechar_beco` `app/chamados/services/becos.py:52`; resolucao literal em `:18`; callers `fechar_beco_da_disputa` `:73`, receiver `app/chamados/signals.py:195`, command `app/chamados/management/commands/fechar_becos_disputa.py:57`.
- `services.competencia_trancada.encerrar` `app/chamados/services/competencia_trancada.py:197–198`, passa `motivo=FECHOU_COMPETENCIA_TRANCADA`.
- command `encerrar_perguntas_com_fato.handle` `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.

Todos os indiretos sem motivo explícito deixam a porta escolher `vazia`/`veredito` (`app/chamados/models.py:1489`), geram `acao='disputa_fechada'` (`:1495`) e o CH recebe a trilha comum pelo signal (`app/chamados/signals.py:42`). A guarda de disputa pode ser forte para suas próprias perguntas, mas não cobre uma pergunta de outra disputa apontando para este CH (`app/chamados/models.py:1481`; `app/chamados/juizes.py:1474`).

## 29/10 16:40 · sessao cdf481adedda · - · - · origem=Ronald (tmux)

Recorte auditado: `models.py`, `juizes.py`, `relatorios_disputa.py`, `templatetags/` e `services/`. Não encontrei `AGENTS.md` nos ancestrais ou dentro de `app/` consultados. [não verificado]

Convenção: **“Não” significa que o teste de respondida/pendente indicado não consulta conteúdo**. Quando há `pergunta_viva` antes/depois, identifico separadamente: esse juiz consulta texto e pode mudar o universo recebido (`app/chamados/juizes.py:1020`).

**Leitores centrais**

| Leitor | O que decide | Distingue `''` de preenchida? |
|---|---|---|
| `PerguntaDisputa.estado` | Depois de priorizar validação/materialização, rotula `PENDENTE` quando timestamp é nulo e `RESPONDIDA` quando presente. `app/chamados/models.py:2108`, `:2112`, `:2115`. | **Não** no ramo pendente/respondida: só timestamp. `app/chamados/models.py:2112`, `:2115`. |
| `PerguntaDisputa.respondida` | Booleano de resposta recebida. `app/chamados/models.py:2318`. | **Não**: só `respondida_em is not None`. `app/chamados/models.py:2318`. |
| `PerguntaDisputa.viva` | Delega ao juiz, sem regra própria. `app/chamados/models.py:2273`. | **Sim**, por delegação a `pergunta_viva`. `app/chamados/models.py:2273`; `app/chamados/juizes.py:1020`. |
| `_pergunta_viva_calc` / `pergunta_viva` | Fala do colaborador entra no ramo que preserva a pergunta viva para decisão/concordância somente quando há **timestamp e texto após strip**; validação, tranca e decisão administrativa também influenciam. `app/chamados/juizes.py:1006`, `:1010`, `:1018`, `:1020`, `:1028`, `:1032`; wrapper `:795`. | **Sim**: timestamp com `''` não entra no ramo da fala. `app/chamados/juizes.py:1020`. |
| `_chave_memo_pv` | Invalida memo do juiz quando timestamp/texto mudam. Não é classificador autônomo. `app/chamados/juizes.py:768`, `:769`, `:791`. | **Sim** na chave: conteúdo é incluído literalmente. `app/chamados/juizes.py:769`. |
| `PerguntaDisputa.q_vivas` | Pré-filtro SQL de perguntas potencialmente vivas; ramo `_resposta_viva` requer timestamp presente e texto diferente de vazio/nulo. `app/chamados/models.py:2246`, `:2247`, `:2257`, `:2258`. | **Sim**, para `''`; SQL não aplica `.strip()`. `app/chamados/models.py:2246`, `:2247`. |
| `DisputaSupervisao.pendentes_do_colaborador` | Badge/lista de disputas abertas com pergunta viva sem timestamp. `app/chamados/models.py:1381`, `:1383`, `:1384`, `:1385`. | **Não** no teste de pendência: texto preenchido com timestamp nulo entra; timestamp presente com texto vazio fica fora. O filtro posterior de vitalidade é distinto. `app/chamados/models.py:1383`, `:1384`. |
| `DisputaSupervisao.respostas_sem_veredito` | Lista respostas que impedem fechamento quando não validadas nem materializadas; considera **timestamp OU texto**. `app/chamados/models.py:1428`, `:1429`, `:1430`, `:1431`. | **Não quando timestamp já existe**: vazia e preenchida entram igualmente. **Sim quando timestamp é nulo**, pois texto preenchido ainda entra. `app/chamados/models.py:1430`, `:1431`. |
| `DisputaSupervisao.pode_fechar` | Libera fechamento somente sem respostas sem veredito e sem fio vivo. `app/chamados/models.py:1439`. | Herda o comportamento misto acima; timestamp com vazio ainda bloqueia via `respostas_sem_veredito`. `app/chamados/models.py:1430`, `:1431`, `:1439`. |
| `DisputaSupervisao.fechar` | Recusa fechamento em qualquer porta se a lista de respostas sem veredito não estiver vazia. `app/chamados/models.py:1472`, `:1473`, `:1474`. | Mesmo comportamento: **não distingue quando timestamp existe**. `app/chamados/models.py:1430`, `:1431`, `:1472`. |
| `perguntas_corrigiveis` | Seleciona respostas que podem voltar a pendentes/reabrir: timestamp presente mais ausência de lastro ou vitalidade. `app/chamados/juizes.py:1116`, `:1117`. | **Não no requisito de resposta**; vazio com timestamp é candidato. O segundo juiz pode variar por conteúdo. `app/chamados/juizes.py:1116`, `:1117`; `:1094`. |
| `perguntas_respondidas_a_validar` | Fila global IA/lote, ordenada pela resposta: timestamp presente, texto diferente de `''`, `q_vivas` e juiz vivo. `app/chamados/juizes.py:1123`, `:1124`, `:1126`, `:1127`. | **Sim**: exclui `''` explicitamente; a exclusão SQL não remove espaços. `app/chamados/juizes.py:1124`. |
| `irmas_respondidas_sem_consumo` | IDs de perguntas respondidas ainda não validadas/materializadas creditadas ao chamado; o docstring descreve pendências. `app/chamados/juizes.py:1471`, `:1473`, `:1474`, `:1477`. | **Sim**: exige timestamp e `.strip()` não vazio. `app/chamados/juizes.py:1473`, `:1477`. |
| `relatorios_disputa.classificar` | Dentro das vivas, qualquer timestamp nulo produz `AGUARDA_COLAB`; caso contrário, `AGUARDA_SUPERVISAO`; sem vivas, `PRONTA_FECHAR`. `app/chamados/relatorios_disputa.py:17`, `:18`, `:20`, `:21`, `:22`. | **Não no teste colab/supervisão**; o juiz de vitalidade é uma etapa anterior que distingue. `app/chamados/relatorios_disputa.py:17`, `:20`. |

**Serviços de apresentação, cobrança e contagem**

| Leitor | O que decide | Distingue vazio? |
|---|---|---|
| Preparação do fio | Define `todas_respondidas` por `all(timestamp)` e libera solicitar correção se todas respondidas, disputa aberta e teto disponível. `app/chamados/services/fio.py:336`, `:337`, `:338`. | **Não**. `app/chamados/services/fio.py:336`. |
| Regularização pelo fio | Bloqueia fechamento/manual materialização se qualquer pergunta viva estiver sem timestamp. `app/chamados/services/fio.py:654`, `:655`, `:664`. | **Não no gate de anuência**; usa somente timestamp depois de obter vivas. `app/chamados/services/fio.py:654`. |
| `acao_regularizar_disputa` | Gate equivalente da outra casca: bloqueia pergunta viva sem timestamp e chama materialização das validadas. `app/chamados/services/acoes_disputa.py:130`, `:131`, `:139`. | **Não no gate**. `app/chamados/services/acoes_disputa.py:130`. |
| `acao_reabrir_disputa` | Conta pendentes por timestamp nulo, consulta corrigíveis e escolhe “já pendente” / “nenhuma pergunta” / reabrir. `app/chamados/services/acoes_disputa.py:95`, `:97`, `:98`, `:99`, `:100`, `:103`. | **Não na contagem**; corrigíveis também não exige texto como requisito de resposta. `app/chamados/services/acoes_disputa.py:95`; `app/chamados/juizes.py:1116`. |
| `reabrir_por_colab` | Quando não há corrigíveis retorna `sem_resposta_para_corrigir`; quando há, reabre e incrementa contador. `app/chamados/models.py:1780`, `:1781`, `:1782`, `:1784`, `:1785`. | **Não como requisito de resposta**, por delegação a corrigíveis. `app/chamados/models.py:1780`; `app/chamados/juizes.py:1116`. |
| Filtro `respostas_que_o_reabrir_apaga` | Mostra quantidade de respostas que a ação de reabrir devolverá ao colaborador. `app/chamados/templatetags/chamados_tags.py:184`, `:188`. | **Não como requisito de resposta**, delega a corrigíveis. `app/chamados/templatetags/chamados_tags.py:188`; `app/chamados/juizes.py:1116`. |
| `perguntas_pendentes_do_colab` | Seleciona perguntas vivas sem timestamp; alimenta aviso de texto livre. `app/chamados/services/aviso_pendencia.py:13`, `:17`, `:21`. | **Não no teste pendente**; vitalidade é avaliada separadamente. `app/chamados/services/aviso_pendencia.py:13`. |
| `cobranca._pendentes` / `checar_cobranca` | Reutiliza seleção acima; decide se pode cobrar, conta perguntas/disputas e dias, bloqueia nova cobrança enquanto prazo corre. `app/chamados/services/cobranca.py:47`, `:48`, `:82`, `:86`, `:88`, `:89`, `:91`. | **Não no teste de resposta**, herdado de `aviso_pendencia.py:13`. |
| `cobranca._calados` | Conta pessoas cobradas/caladas e sem canal usando timestamp nulo e `q_vivas`, disputa aberta, canal e vencimento. `app/chamados/services/cobranca.py:229`, `:230`, `:243`, `:246`, `:248`. | **Não** no filtro de silêncio: não exige texto vazio. `app/chamados/services/cobranca.py:229`. |
| `recobrar_questionario` | Conta pendentes por timestamp nulo para retorno/trilha da recobrança; renova prazo e anexa nota. `app/chamados/services/disputa_emissao.py:1444`, `:1445`, `:1450`, `:1453`, `:1454`. | **Não**. `app/chamados/services/disputa_emissao.py:1445`. |
| `becos.classificar` | Pergunta sem timestamp e destino não resolvido conta como visível ao colab; ausência dessas e pai resolvido permite classificar beco, sujeito a `pode_fechar`. `app/chamados/services/becos.py:29`, `:30`, `:31`, `:33`. | **Não no teste de visibilidade**; o gate de fechamento é separado. `app/chamados/services/becos.py:30`, `:33`. |
| Censo `furo_sem_canal` | Timestamp nulo significa pergunta aberta/canal para célula; conta células acusatórias sem esse canal nem chamado vivo. `app/chamados/services/furo_sem_canal.py:80`, `:81`, `:85`, `:86`, `:90`. | **Não**. `app/chamados/services/furo_sem_canal.py:81`. |
| `verbos` — resultado da cobrança | Conta colaboradores que responderam após cobrança mais recente, hoje e últimos sete dias, usando data do timestamp. `app/chamados/services/verbos.py:306`, `:308`, `:315`, `:317`, `:318`, `:319`. | **Não**: não consulta texto. `app/chamados/services/verbos.py:306`, `:309`. |
| `historico._eventos_dos_chamados` | Timestamp gera evento “colaborador respondeu”; texto vazio recebe rótulo “sem resposta”, mas o evento continua sendo resposta. `app/chamados/services/historico.py:99`, `:105`, `:106`, `:109`, `:112`, `:113`. | **Não para classificar evento respondido**; **sim apenas no texto exibido**, pelo fallback. `app/chamados/services/historico.py:99`, `:109`. |
| `explicador.explicar_pergunta` | Usa `.estado`: pendente → dono colaborador/cobrar; respondida → dono admin/validar ou recusar. `app/chamados/services/explicador.py:72`, `:73`, `:74`, `:75`, `:77`, `:78`, `:79`. | **Não** no ramo respondida/pendente herdado de `.estado`. `app/chamados/models.py:2112`, `:2115`; `app/chamados/services/explicador.py:72`. |
| `fila_por_causa._gaveta_de` | Disputa aberta com `respostas_sem_veredito()` entra em `respondido_a_decidir`. `app/chamados/services/fila_por_causa.py:214`, `:215`, `:216`. | **Não quando há timestamp**, sim para texto sem timestamp, herdando o OR do modelo. `app/chamados/models.py:1430`, `:1431`. |
| Medição de competência trancada | Usa respostas sem veredito ignorando tranca para contar e encaminhar à pauta/decisão do DP. `app/chamados/services/competencia_trancada.py:113`, `:126`, `:127`, `:128`. | Mesmo comportamento misto do OR: **timestamp com vazio é aceito**. `app/chamados/models.py:1430`, `:1431`. |
| `fila_validar_classificada` | Usa fila global de respondidas, classifica e conta perguntas/chamados; calcula idade pelo timestamp. `app/chamados/services/validacao.py:867`, `:868`, `:870`, `:875`, `:877`, `:878`. | **Sim no fluxo padrão**, porque a fila padrão exclui `''`; parâmetro `qs` pode substituir essa seleção. `app/chamados/services/validacao.py:867`; `app/chamados/juizes.py:1124`. |
| `marcador_do_placar` | Timestamp máximo participa do ETag/invalidação de atualização; não decide respondida/pendente. `app/chamados/services/painel.py:66`, `:67`, `:68`. | **Não**, mas fora do núcleo classificatório solicitado. `app/chamados/services/painel.py:67`. |

**Validação/materialização e guardas que leem estado de resposta**

| Leitor | O que decide | Distingue vazio? |
|---|---|---|
| `validar_pergunta` | Timestamp ausente retorna `sem_resposta`/aguardar colab. `app/chamados/services/validacao.py:356`, `:357`, `:358`. | **Não nesse gate**: timestamp com vazio passa. Materialização possui guarda posterior por texto. `app/chamados/services/validacao.py:356`; `app/chamados/services/materializacao.py:769`. |
| `recusar_pergunta` | Timestamp ausente significa `ja_pendente`; presente permite devolver resposta/reabrir. `app/chamados/services/validacao.py:530`, `:532`, `:540`. | **Não no gate**; apenas nota registra `(vazia)` como apresentação. `app/chamados/services/validacao.py:530`, `:546`. |
| `materializar_perguntas_validadas_da_disputa` | Seleciona prontas com timestamp + validação presentes e sem materialização. `app/chamados/services/materializacao.py:1286`, `:1287`, `:1288`, `:1289`, `:1292`. | **Não no queryset**; unidade recusa texto vazio depois. `app/chamados/services/materializacao.py:1287`, `:769`. |
| Unidade `_materializar_uma` | Texto vazio após strip significa sem anuência; recusa e não planta batida. `app/chamados/services/materializacao.py:769`, `:776`, `:778`. | **Sim**, independente do timestamp. `app/chamados/services/materializacao.py:769`. |
| `estado_aplicacao_pergunta` | Não gera estado/badge de aplicação se não for hora ou não houver texto. `app/chamados/services/materializacao.py:415`, `:416`. | **Sim**. `app/chamados/services/materializacao.py:415`. |
| Materialização de ausência e granular | Só completa timestamp/validação ausentes quando há texto; sem texto preserva ausência desses carimbos. `app/chamados/services/materializacao.py:511`, `:512`, `:514`; `:1190`, `:1191`, `:1194`. | **Sim**: usa `.strip()`. `app/chamados/services/materializacao.py:511`, `:1190`. |
| Resolver perguntas por chamado resolvido | Protege fala com texto+timestamp sem lastro contra fechamento automático; se resolve, completa carimbos somente quando há texto. `app/chamados/services/disputa_emissao.py:1558`, `:1559`, `:1560`, `:1570`, `:1571`, `:1574`. | **Sim**. `app/chamados/services/disputa_emissao.py:1558`, `:1570`. |
| Mesmo resolvedor, ramo de escolha sem credor | Depois de juiz considerar pergunta morta, preenche timestamp nulo sem testar texto nesse ramo. `app/chamados/services/disputa_emissao.py:1641`, `:1643`, `:1648`, `:1649`. | **Não nesse teste**; é guarda de escritor, não leitor UI. `app/chamados/services/disputa_emissao.py:1648`. |
| `declaracao_texto` — colab | Timestamp+texto igual à hora retorna `ja_declarada`; caso contrário passa `bool(timestamp)` ao core para indicar correção. `app/chamados/services/declaracao_texto.py:363`, `:364`, `:365`. | **Sim** para idempotência (exige conteúdo igual); **não** na classificação de correção enviada ao core. `app/chamados/services/declaracao_texto.py:363`, `:365`. |
| `declaracao_texto` — emissão/admin | Só escreve resposta pelo core quando timestamp ausente; presente segue direto para validar resposta existente. `app/chamados/services/declaracao_texto.py:632`, `:638`, `:641`. | **Não no gate de já respondida**. `app/chamados/services/declaracao_texto.py:632`. |
| `responder_automaticamente` | Preserva resposta textual existente; só responde automaticamente a pergunta sem texto. `app/chamados/services/disputa_emissao.py:1390`, `:1391`, `:1392`. | **Sim**, só texto. `app/chamados/services/disputa_emissao.py:1390`. |
| `carimbar_resposta_pela_trilha` | Só repara limbo texto sem timestamp; timestamp presente ou texto vazio viram no-op. `app/chamados/services/disputa_emissao.py:1407`, `:1408`, `:1409`. | **Sim**. `app/chamados/services/disputa_emissao.py:1407`. |

**Leitores que usam só texto e influenciam resposta/pendência**

| Leitor | Decisão | Vazio diferente? |
|---|---|---|
| Universo de ressurreição/cartório | Seleciona apenas materializadas com texto vazio/nulo para voltar à fila, respeitando luz do marco. `app/chamados/models.py:1112`, `:1113`, `:1115`, `:1119`. | **Sim**; SQL testa `''`/nulo, não espaços. `app/chamados/models.py:1115`. |
| `canonica_do_grupo` | Na deduplicação, pergunta com texto vence; empate usa menor PK. `app/chamados/services/fio.py:825`, `:826`, `:827`. | **Sim**, com strip. `app/chamados/services/fio.py:826`. |
| `resposta_display` | Texto vazio retorna `''`; preenchido resolve nome/rótulo de dropdown ou devolve texto. `app/chamados/models.py:2323`, `:2324`, `:2325`, `:2345`. | **Sim**, apresentação. `app/chamados/models.py:2323`. |
| `leitura_da_resposta` | Sem texto não classifica plausibilidade nem gera frase; preenchida chama classificador. `app/chamados/templatetags/chamados_tags.py:128`, `:129`, `:133`. | **Sim**, com strip. `app/chamados/templatetags/chamados_tags.py:128`. |
| `classificar_resposta_validacao` | Escolha sem texto não tem classe; preenchida classifica A/B; horas passam pelo parser de delta. `app/chamados/juizes.py:1403`, `:1404`, `:1405`, `:1406`, `:1409`. | **Sim**. `app/chamados/juizes.py:1404`; hora vazia falha em `:1316`, `:1317`. |
| `classe_do_lote` | Escolha vazia é lote D; hora ilegível/vazia também D; outros tipos têm regras próprias. `app/chamados/services/validacao.py:799`, `:804`, `:805`, `:806`, `:810`, `:811`. | **Sim** para escolha/hora; dropdown posto sempre C nesse ramo, mesmo vazio. `app/chamados/services/validacao.py:802`, `:803`, `:805`, `:810`. |
| `declarou_hora` | Vazia não é declaração; preenchida só é declaração se não for escape maiúsculo/marca descartada. `app/chamados/juizes.py:1152`, `:1153`, `:1154`, `:1155`. | **Sim**. `app/chamados/juizes.py:1153`. |
| `_slot_esperado`, `delta_da_resposta`, `_resposta_concorda_com_a_luz`, `motivo_implausivel_validacao` | Usam conteúdo de hora para derivar lastro/slot, classificar delta/concordância ou plausibilidade; vazio não produz hora comparável. `app/chamados/juizes.py:332`, `:339`, `:983`, `:995`, `:996`, `:1299`, `:1316`, `:1317`, `:1425`, `:1436`. | **Sim por parsing**, não pelo timestamp. Referências anteriores. |

Não confundir `services/fio.py:608`: esse leitor de `.estado` só verifica `VALIDADA/RESOLVIDA` para gravar trilha após a ação, sem decidir respondida/pendente (`app/chamados/services/fio.py:608`, `:609`). `_fechar_disputa_se_quitada` conta **vivas** e delega fechamento ao `pode_fechar`, sem nova regra por timestamp (`app/chamados/services/materializacao.py:1231`, `:1233`, `:1250`).

## 29/10 16:39 · sessao 67ace01ddde8 · - · - · origem=Ronald (tmux)

Leitores diretos no escopo de crons/auditoria/reconciliador:

| Leitor | Decisão/efeito | Distingue `''` de preenchida? |
|---|---|---|
| `lavrar_sem_turno_aberto` | Texto preenchido impede resolução automática pela via `sem_turno_aberto`; após outras guardas, materializa e pode fechar disputa. | **SIM**, pelo `.strip()`. `app/chamados/reconciliador.py:365`, `app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:413`. |
| `veredito_orfa` | Texto preenchido impede classificar a órfã como `encerra`/`teto`; consumidor só encerra quando recebe `ENCERRA`. | **SIM**, `.strip()`. `app/chamados/reconciliador.py:878`, `app/chamados/reconciliador.py:928`, `app/chamados/reconciliador.py:932`, `app/chamados/reconciliador.py:940`. |
| `perguntas_mudas_com_resposta` | Seleciona perguntas sem texto tipado e com resposta registrada no fio do mesmo dia; produz dados para fila administrativa. | **SIM**, igualdade literal `resposta_colab=''`; ignora carimbo. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/canal.py:72`, `app/chamados/canal.py:75`. |
| `dedup_perguntas_fato.handle` | Texto determina o rótulo de relatório “com resposta” versus “menor id”; escolha efetiva da canônica delegada a `canonica_do_grupo`. | **SIM**, `.strip()`. `app/chamados/management/commands/dedup_perguntas_fato.py:71`, `app/chamados/management/commands/dedup_perguntas_fato.py:72`, `app/chamados/management/commands/dedup_perguntas_fato.py:83`. |
| `curar_perguntas_em_limbo.handle` | Define limbo como carimbo nulo com texto não vazio; valida universo medido, busca nota para carimbar e encaminha demais para reabertura. | **SIM** no universo, por `exclude(resposta_colab='')`; guarda adicional de reabertura usa **somente carimbo**, sem distinguir vazio localmente. `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:56`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. |
| Auditor `_contraprova_i21a` | Confere se piso I21a possui ausência aprovada e resposta registrada; carimbo ausente produz alarme. | **NÃO no predicado**: apenas `not p.respondida_em`; recebe universo já filtrado por texto. `app/chamados/management/commands/auditar_invariantes_chamados.py:177`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1571`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1581`. |
| Auditor I2/I2p | Carimbo presente com disputa aberta retira pergunta de violação I2 e declara piso “RESPONDIDAS”. | **NÃO**: texto vazio e preenchido recebem a mesma isenção pelo carimbo. `app/chamados/management/commands/auditar_invariantes_chamados.py:586`, `app/chamados/management/commands/auditar_invariantes_chamados.py:591`, `app/chamados/management/commands/auditar_invariantes_chamados.py:593`, `app/chamados/management/commands/auditar_invariantes_chamados.py:595`. |
| Auditor I20 | Conta/alarma novos meios-estados: texto preenchido com carimbo nulo, excluindo PKs explicitamente cicatrizados. | **SIM**, `exclude(resposta_colab='')`. `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1102`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`. |
| Auditor I21/I21a | Conta respostas sem validação/materialização, invisíveis porque destino e pai saíram de vivos; ausência aprovada move para piso I21a. | **SIM**, exige carimbo e texto diferente de `''`. `app/chamados/management/commands/auditar_invariantes_chamados.py:1568`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1571`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1576`. |
| Auditor C2 | Conta “pergunta morta-por-célula” sem materialização/validação/resposta por carimbo. | **NÃO**: carimbo nulo basta para esse aspecto; texto não é examinado. `app/chamados/management/commands/auditar_invariantes_chamados.py:1597`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1598`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1599`. |
| Auditor F1 | Pula chamado se alguma pergunta está viva; também pula quando alguma pergunta possui texto, preservando como decisão do admin. | **SIM**, `.strip()`; texto preenchido afasta o alarme independentemente de testar carimbo nesse ramo. `app/chamados/management/commands/auditar_invariantes_chamados.py:1648`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1649`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1651`. |

Consumidores indiretos com decisão relevante:

- `_retratar` barra a retratação de chamado quando `irmas_respondidas_sem_consumo(ch)` devolve respostas pendentes; retorna `False`, com trilha deduplicada. A distinção vazio/preenchido vem do juiz chamado, e não de teste local. `app/chamados/reconciliador.py:435`, `app/chamados/reconciliador.py:436`, `app/chamados/reconciliador.py:446`, `app/chamados/reconciliador.py:454`; o juiz exige carimbo e texto `.strip()` em `app/chamados/juizes.py:1473`, `app/chamados/juizes.py:1477`.
- Auditor I12/I12r usa o mesmo juiz para tirar chamado da violação e colocá-lo no piso de resposta aguardando validação; **SIM** por delegação. `app/chamados/management/commands/auditar_invariantes_chamados.py:983`, `app/chamados/management/commands/auditar_invariantes_chamados.py:984`, `app/chamados/management/commands/auditar_invariantes_chamados.py:987`, `app/chamados/juizes.py:1473`, `app/chamados/juizes.py:1477`.
- `encerrar_orfas_pela_lampada` barra fechamento da disputa quando `respostas_sem_veredito()` devolve pendência e incrementa `disputas_barradas`; **SIM** por delegação, mas a autoridade aceita carimbo **OU** texto como evidência de resposta. `app/chamados/reconciliador.py:956`, `app/chamados/reconciliador.py:958`, `app/chamados/reconciliador.py:963`, `app/chamados/models.py:1430`, `app/chamados/models.py:1431`.
- `validar_classe_a` busca a gaveta `respondido_a_decidir`, reúne `respostas_sem_veredito()`, classifica e valida classe A; usa a autoridade acima, **SIM** por delegação, com o mesmo **OU**. `app/chamados/management/commands/validar_classe_a.py:44`, `app/chamados/management/commands/validar_classe_a.py:54`, `app/chamados/management/commands/validar_classe_a.py:63`, `app/chamados/management/commands/validar_classe_a.py:66`, `app/chamados/management/commands/validar_classe_a.py:100`, `app/chamados/models.py:1430`, `app/chamados/models.py:1431`.
- `lavrar_furo_sem_canal` publica/conta “com pergunta aberta” a partir de `services.furo_sem_canal.medir`; a referência ao carimbo na docstring não constitui outro predicado executável no command. `app/chamados/management/commands/lavrar_furo_sem_canal.py:15`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:30`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:35`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:48`.

Uso auxiliar, fora da classificação respondida/pendente: `veredito_lavrado` lê `Max(respondida_em)` para invalidar veredito quando insumo mudou; **NÃO** distingue texto vazio/preenchido, mas não decide que a pergunta está respondida nesse ponto. `app/chamados/veredito_lavrado.py:167`, `app/chamados/veredito_lavrado.py:168`, `app/chamados/veredito_lavrado.py:187`, `app/chamados/veredito_lavrado.py:189`.

## 29/10 16:39 · sessao 4a34da157bb4 · - · - · origem=Ronald (tmux)

Censo da minha área, somente leitura. “Não” abaixo significa que **o teste local de respondida/pendente não distingue `resposta_colab == ''` de texto preenchido**; seleção anterior por `pergunta_viva` pode aplicar outras regras, identificadas separadamente. [nao verificado]

| Leitor | O que decide | Distingue vazio? |
|---|---|---|
| `app/chamados/views.py:1280` | Calcula `ja_respondida` para o POST HTML e passa ao core (`:1283`). O core usa o booleano para acrescentar “correcao” na auditoria e selecionar “Resposta corrigida” no fio (`app/chamados/services/disputa_emissao.py:1286`, `:1312`). | **Não**: apenas `p.respondida_em is not None` (`app/chamados/views.py:1280`). |
| `app/chamados/views.py:1416` | Busca perguntas sem carimbo, retém vivas (`:1417`), conta (`:1418`) e, inexistindo corrigíveis, decide recobrar (`:1421–1425`) ou informar que não há pergunta viva para cobrar (`:1431–1433`). | **Não no filtro de respondida**: apenas NULL (`:1416`); a vivacidade é outro filtro (`:1417`). |
| `app/chamados/views_cobrar.py:169` | `validar_inline`: seleciona entre vivas as que têm carimbo para encaminhar à validação (`:201–208`); calcula remanescentes (`:214`). | **Não no teste de respondida**: apenas `_p.respondida_em` (`:169`); há seleção prévia por `perguntas_vivas_de` (`:168–169`). |
| `app/chamados/views_cobrar.py:175–176` | Sem itens para validar, busca perguntas sem carimbo e responde “Aguardando o COLABORADOR”, com quantidade (`:192–197`); pode informar respostas existentes no chat (`:180–195`). | **Não** no filtro; nenhuma condição sobre o texto tipado (`:175–176`). |
| `app/api/views.py:1184–1188` | Libera `pode_solicitar_correcao` se todas as perguntas do chamado têm carimbo e ainda há menos de duas reaberturas. | **Não**: `all(p.respondida_em ...)` (`:1187`). |
| `app/api/views.py:2621–2624` | `api_disputa_perguntas`: separa `respondidas` de `perguntas` e publica `aguarda_admin`. | **Não na classificação**: usa `p.estado` (`:2621–2624`), cujo desempate pendente/respondida depende só do carimbo (`app/chamados/models.py:2112`, `:2115`). Universo anterior vem de `perguntas_do_app` (`app/api/views.py:2603–2604`). |
| `app/api/views.py:2642–2644` | Escolhe chamado de destino de uma pergunta que não esteja no estado RESPONDIDA para ancorar a resposta da API. | **Não na classificação**: `p.estado != RESPONDIDA` (`:2643`), derivado pelo carimbo (`app/chamados/models.py:2112`, `:2115`). |
| `app/api/views.py:2688` | Calcula `ja_respondida` no POST API; passa ao mesmo core (`:2694–2695`), com efeitos de trilha citados acima. | **Não** sobre o valor persistido (`:2688`). A recusa de **nova entrada** vazia em `:2690–2692` é diferente de examinar `p.resposta_colab`. |
| `app/templates/chamados/partials/_pergunta_card.html:24` | Exibe “aguardando validacao” versus “aguardando colaborador” (`:25`), depois de priorizar validada (`:23`). | **Não** (`:23–25`). |
| `app/templates/chamados/partials/_pergunta_card.html:29` | Exibe resposta, data e ações Validar/Rejeitar para pergunta carimbada ainda não validada (`:31–43`, `:92–117`). | **Não**: guarda é só `perg.respondida_em` (`:29`); texto exibido por `resposta_display` (`:31`). |
| `app/templates/chamados/partials/modal_fio.html:182` | Exibe correção sugerida e ações da pergunta cega somente se não carimbada (`:183–197`). | **Não** (`:182`). |
| `app/templates/chamados/partials/modal_fio.html:210` | Alterna resposta e seu fluxo de validação/correção (`:212–309`) versus “Aguardando resposta do colaborador” (`:311–312`). | **Não**: só carimbo (`:210`), mesmo exibindo texto cru (`:212`). |
| `app/templates/chamados/partials/modal_fio.html:327` | Na disputa aberta, alterna aviso e botão Cobrar (`:334`, `:342–344`) versus formulário “Regularizar disputa e fechar” (`:345–352`). | **Não**: `chamado.todas_respondidas` vem de `all(p.respondida_em ...)` (`app/chamados/services/fio.py:335–338`). |
| `app/templates/chamados/partials/_card_atendimento.html:94` | Na pergunta viva (`:77`), mostra “Resposta enviada”, data e “Aguardando a supervisao conferir” (`:99–101`). | **Não** no recibo: só carimbo (`:94`). |
| `app/templates/chamados/partials/_card_atendimento.html:155` | Alterna botão “Reenviar resposta” versus “Enviar resposta”. | **Não** (`:155`). |
| `app/templates/chamados/partials/_card_atendimento.html:384` | Para pergunta de hora carimbada, exibe selo “Enviado” com resposta/data (`:385`). | **Não** (`:384–385`). |
| `app/templates/chamados/partials/_card_atendimento.html:135` | Mostra “Você respondeu… Escolha de novo para corrigir” no formulário de hora (`:136–138`). | **Sim**, só texto: `{% if p.resposta_colab %}` (`:135`); carimbo só acrescenta data (`:137`). |
| `app/templates/chamados/partials/_card_atendimento.html:176` | Na pergunta não viva (`:173`), mostra “Sua resposta” somente quando existe texto (`:177`). | **Sim**, só texto (`:176`). |

Leitores de apresentação/preenchimento, sem nova decisão própria de respondida/pendente:

- `app/templates/chamados/partials/_card_atendimento.html:108`, `:142`, `:148`: usam texto para marcar opção de escolha, preencher data/hora ou selecionar posto/escala. Tratam o vazio de forma distinta no conteúdo do formulário, sem classificar respondida por isso.
- `app/api/views.py:1173–1175` e `:2632–2634`: serializam texto, display e carimbo; não rejeitam ou reclassificam texto vazio nessas linhas.
- `app/api/views_mensageria.py:567–570`: serializa texto, carimbo e dias de espera. A fila que recebe **distingue vazio**: chama `perguntas_respondidas_a_validar` (`:539–540`), cujo queryset exige carimbo e exclui `resposta_colab=''` (`app/chamados/juizes.py:1123–1124`).
- `app/chamados/views_cobrar.py:290–293`: monta amostra da proposta de validação com `resposta_colab`; a seleção foi delegada a `fila_validar_classificada` (`:287–288`).
- `app/templates/chamados/partials/_lote_preview.html:24`: imprime `x.pergunta.resposta_colab`; não há condição local para vazio nessa célula.

Ocorrência sem decisão observável: `app/api/views.py:1007–1010` constrói `_pend` com `respondida_em__isnull=True`; `_pend` não tem outro uso nesse arquivo, e a expressão termina em queryset lazy (`:1010`). Portanto não é contador/filtro consumido pela tela nessa implementação. A ausência de outro uso foi conferida por busca textual no arquivo [não verificável por uma única referência de linha].

## 29/10 16:39 · sessao 39bff95d7513 · - · - · origem=Ronald (tmux)

Censo dos escritores extras, em só leitura:

| Sítio | Mudança terminal | Consulta dependente antes? | via / motivo / auditoria |
|---|---|---|---|
| `backfill()` | `cancelado → superado` por `.update()` em `app/chamados/migrations/0039_a3_via_backfill.py:24` | **Não**: corpo consulta apenas usuários e chamados irmãos (`app/chamados/migrations/0039_a3_via_backfill.py:12`, `app/chamados/migrations/0039_a3_via_backfill.py:20`). | `via_encerramento='superado'`; `superado_por_id=irm[0].pk`; não grava motivo/LogAuditoria no corpo (`app/chamados/migrations/0039_a3_via_backfill.py:24`). |
| `desfazer()` | `superado → cancelado` por `.update()` (`app/chamados/migrations/0039_a3_via_backfill.py:33`) | **Não**: função inteira só obtém o modelo e executa dois updates (`app/chamados/migrations/0039_a3_via_backfill.py:31`). | Preserva via no primeiro update e depois limpa **todas** para `''`; não grava motivo/LogAuditoria (`app/chamados/migrations/0039_a3_via_backfill.py:33`, `app/chamados/migrations/0039_a3_via_backfill.py:34`). |

As duas funções são forward/reverse da mesma migração (`app/chamados/migrations/0039_a3_via_backfill.py:39`).

Nascimento terminal possível, separado de transição:

- `ChamadoColaborador.criar(status=...)` aceita estado inicial informado e grava diretamente no `create`; só transforma o default `'aberto'`, portanto pode nascer terminal sem passar por `_gravar_estado` (`app/chamados/models.py:805`, `app/chamados/models.py:814`, `app/chamados/models.py:834`).
- Chamador encontrado com esse comportamento: `smoke_chamados.Command._bloco_b_transicoes._novo()` repassa `status=status`; o laço usa as origens da matriz, inclusive terminais, e há chamada explícita `_novo("fechado")` (`app/chamados/management/commands/smoke_chamados.py:123`, `app/chamados/management/commands/smoke_chamados.py:126`, `app/chamados/management/commands/smoke_chamados.py:130`, `app/chamados/management/commands/smoke_chamados.py:146`).
- Trilha desse nascimento: `acao='estado_chamado'`, `depois.via='criar'`; `via_encerramento` continua default `''` (`app/chamados/models.py:48`, `app/chamados/models.py:835`, `app/chamados/models.py:889`, `app/chamados/models.py:892`).

Não encontrei escritores adicionais por atributo, `setattr`, subscript/dicionário `status_local`, kwargs ORM ou SQL/shell/JS fora desses e do núcleo `models.py:192`; limite: busca estática, sem execução [não verificado]. O próprio contrato do repo declara como único escritor de transição `self.status_local = novo`, excluindo migrações/testes da varredura (`app/chamados/tests/test_contract_status_nucleo.py:9`, `app/chamados/tests/test_contract_status_nucleo.py:16`, `app/chamados/tests/test_contract_status_nucleo.py:23`).

## 29/10 16:24 · sessao 6010c1d1cba5 · grupo1 · verificador · origem=Code

- **Classe 3 — `app/ponto/services/dias_em_aberto.py:112`:** `passivo_da_competencia` lê `_tz_ato.now()` por colaborador, dentro do serviço, sem receber o instante do chamador (`:78`).
- **Classe 3 — `app/ponto/services/he_pendente_lavrado.py:93`:** `apurar` consulta o relógio a cada colaborador; sua assinatura não recebe o instante do ato (`:71`).
- **Classe 3 — `app/ponto/services/veredito_celula.py:217`:** `_grade` lê `timezone.now()` a cada simulação. Baseline (`:219`) e candidatos (`:273`, `:281`) recebem instantes independentes.
- **Classe 3 — `app/ponto/management/commands/e6_oraculo.py:141`:** o leitor compartilhado `InsumosDoDono.cxr` captura o relógio internamente; o construtor não recebe instante (`:118`). Também é consumido pelo serviço `app/ponto/services/teto_na_lista.py:71`.

Total por classe: 1=0; 2=0; 3=4; 4=0; 5=0; 6=0; 7=0.

## 29/10 16:23 · sessao 913e999675d7 · - · - · origem=Ronald (tmux)

Não confirmei achados independentes das classes 5 ou 7.

O retorno à DP é discriminado pelo caso comportamental em `app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:257` e pelo selo em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:454`.

O selo do instante verifica explicitamente argumento ausente, `None` literal e assinatura obrigatória; não promete fiscalizar a origem de todo relógio (`app/escala/tests/test_selo_grade_pede_o_instante.py:24`, `:38`, `:71`). Aceitar `agora=timezone.now()` isoladamente não demonstra classe 5, pois bordas também capturam o instante.

O corte exige RED de 24x48 (`app/docs/CORTES.md:356`), mas pouso efetivo antes desse requisito permanece [nao verificado].

## 29/10 16:23 · sessao 104676660d1d · - · - · origem=Ronald (tmux)

Não confirmei achados novos das classes 1, 2, 4 ou 6.

- O seletor consulta o corpo puro do juiz registrado, sem reproduzir o predicado de encerramento: `app/escala/utils.py:566-570`; `app/ponto/turnos.py:2083-2105`. Os filtros de turno único e fechado já constavam na closure substituída, conforme o diff fornecido.
- O retratador continua escrevendo pela porta `retratar_batida`: `app/ponto/management/commands/detectar_par_relampago.py:217-226`. As alterações do diff nesse fluxo propagam `agora`: `app/ponto/management/commands/detectar_par_relampago.py:235-239`, `267-272`, `289`, `309`.
- Há diferenças preexistentes entre a alimentação do detector e da grade: detector lê quatro marcos pela função `marcos_turno` (`app/chamados/detector_anomalias.py:197`; `app/escala/servico_jornada.py:68-77`), enquanto a grade monta todas as pausas (`app/escala/utils.py:1577-1581`). Não atribuí esse problema ao diff.

## 29/10 16:23 · sessao ffccd173bc3f · - · - · origem=Ronald (tmux)

Achados de classe 3:

- `app/ponto/services/dias_em_aberto.py:112`: `passivo_da_competencia` lê `_tz_ato.now()` dentro do laço de colaboradores (`:106`); sua assinatura (`:78`) não recebe instante.
- `app/ponto/services/he_pendente_lavrado.py:93`: `apurar` lê `_tz_ato.now()` por colaborador (`:91`), em vez de receber o instante do ato; assinatura em `:71`.
- `app/ponto/services/veredito_celula.py:217`: `_grade` lê `timezone.now()` em cada simulação; baseline (`:219`) e alternativas (`:273`, `:281`) podem ser julgados em instantes distintos.
- `app/ponto/management/commands/e6_oraculo.py:141`: `InsumosDoDono.cxr` lê relógio dentro do carregador compartilhado; seu construtor (`:118`) não recebe instante. Há consumidores externos ao command em `app/ponto/services/teto_na_lista.py:71` e `app/ponto/management/commands/pautas_discordantes.py:82`.

Candidato adicional, menos inequívoco: `app/folha/porta_export.py:318`, leitor `medir` com relógio próprio e assinatura sem instante (`:313`); além disso, `hoje = _hoje()` (`:338`) consulta outro relógio (`:23`). Depende de considerar `medir` serviço interno ou borda do ato.

## 29/10 16:21 · sessao 094a77c4a3e8 · grupo1 · verificador · origem=Code

- **Classe 3 — `app/ponto/services/veredito_celula.py:217`:** `_grade` lê `timezone.now()` a cada simulação; baseline e candidatos recebem instantes diferentes nas chamadas de `:219`, `:273` e `:281`.

- **Classe 3 — `app/ponto/services/he_pendente_lavrado.py:93`:** `apurar` lê o relógio dentro do laço de colaboradores (`:91`), em vez de receber o instante da execução.

- **Classe 3 — `app/ponto/services/dias_em_aberto.py:112`:** `passivo_da_competencia` captura outro instante por colaborador dentro do laço (`:106`).

- **Classe 3 — `app/ponto/management/commands/e6_oraculo.py:141`:** o carregador compartilhado `InsumosDoDono.cxr` lê o relógio internamente; seu construtor não recebe `agora` (`:118`).

- **Classe 3 — `app/folha/porta_export.py:318`:** o leitor `medir` captura seu próprio instante; o comando chama esse leitor por empresa, sem transmitir um instante comum (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`, `:88`).

- **Classe 3 — `app/relatorios/pdf_espelho.py:1040`:** o gerador informacional captura o instante internamente; a geração em lote o chama por colaborador (`app/relatorios/services.py:250`).

- **Classe 3 — `app/relatorios/pdf_espelho.py:1086`:** o gerador auditável também captura outro instante internamente; o lote chega a ele por `app/relatorios/services.py:237` e `:70`.

- **Classe 3 — `app/relatorios/pdf_espelho.py:593`:** passa a data de `agora` como `data_fim` de `ate_ontem`, mas omite `hoje=`; o helper lê `timezone.localdate()` (`app/relatorios/cartao_pela_celula.py:40`). A chamada em `app/relatorios/pdf_espelho.py:287` também omite esse parâmetro, misturando o instante recebido com o relógio da execução.

Total por classe: 1=0; 2=0; 3=8; 4=0; 5=0; 6=0; 7=0.

## 29/10 16:20 · sessao 46356997b8f3 · - · - · origem=Ronald (tmux)

Não confirmei defeitos novos das classes 1, 4, 6 ou 7.

O seletor reutiliza o julgamento existente: `app/escala/utils.py:566` e `app/escala/utils.py:569` chamam `_dia_encerrado_puro`, cujo corpo delega ao mesmo predicado de encerramento em `app/ponto/turnos.py:2085`.

A condição de quatro marcos do detector (`app/chamados/detector_anomalias.py:218`) corresponde ao contrato atual de `marcos_turno`, que monta entrada, primeira pausa e saída (`app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:75`, `app/escala/servico_jornada.py:77`). A ausência da segunda pausa nesse serviço é anterior ao diff fornecido.

Não há evidência verificável de pouso ou violação de pré-requisito nos trechos alterados examinados. [não verificado] [nao verificado]

## 29/10 16:19 · sessao 8622147100f1 · - · - · origem=Ronald (tmux)

Nenhum achado confirmado de classe 5: a regressão `turno=None` no detector é distinguida pelo caso comportamental C923, que exige `('grave', '07:30', 119.0)` (app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:251–257).

Duas limitações verificáveis dos selos, sem concluir defeito do conjunto:

- O selo do instante aceita `agora=timezone.now()`: verifica somente presença do argumento e `None` literal (app/escala/tests/test_selo_grade_pede_o_instante.py:37–39). Pode sustentar classe 5 se a promessa cobrada for impedir relógio fora da borda; a promessa AST específica, porém, é apenas ausência de `agora=` (mesmo arquivo:9–11).
- O tripwire aceita nome posteriormente sobrescrito ou seletor em ramo morto: `_nomes_do_seletor` reúne nomes de qualquer atribuição contendo a chamada, sem analisar fluxo (app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:473–476). O teste comportamental C923 ainda pega a volta integral à DP (app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:251–257).

Não encontrei omissões de `agora=` nas chamadas de produção aos novos contratos obrigatórios por varredura AST em memória; isso é evidência da inspeção, sem linha única no commit. [nao verificado]

## 29/10 15:56 · sessao e4097e916b79 · o261 · censo · origem=Code

Escritores de `respondida_em`:

| Função | Arquivo:linha | Pode carimbar com `resposta_colab` vazia? |
|---|---|---|
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1248` | Não; rejeita vazia em `:1220`. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1393` | **Sim**, não valida `texto`; o chamador atual fornece constante preenchida — `app/api/foto_recebida.py:25`, `:48`. |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1409` | Não; guarda em `:1407`. |
| `resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1572` e `:1649` | Primeiro exige texto; **segundo permite vazio**, sem guarda em `:1648`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:513` | Só atribui carimbo com texto (`:511`); porém salva o campo mesmo sem texto, preservando eventual carimbo anterior (`:528`). |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1192` | Não cria carimbo sem texto; guarda em `:1190`. |

Também escrevem **`None`**: `ChamadoColaborador.ressuscitar_perguntas` — `app/chamados/models.py:1163`; `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1358`, persistência em `:1377`. Esta última também esvazia a resposta (`:1358`).

Portas de reabertura/desarquivamento:

| Porta | Efeito sobre o véu |
|---|---|
| `ChamadoColaborador.reabrir` — `app/chamados/models.py:1023` | **Não chama `levantar`**: muda estado (`:1089`) e ressuscita perguntas (`:1098`), preservando `silenciado_ate`; campos persistidos em `app/chamados/models.py:193`. |
| `transicionar` — `app/chamados/models.py:183`, `:260` | **Não levanta**; permite reabertura humana conforme `app/chamados/catalogo/motor.py:102`, `:150`. |
| `desarquivar_chamado` — `app/chamados/views_arquivar.py:52` | **Chama `levantar`**, com `resgatado=True` (`:75`). Chama `reabrir` somente para encerramento por competência trancada (`:64`, `:68`). |
| `_ressuscitar_por_resposta` | **Chama `levantar`**, com `resgatado=True` — `app/chamados/services/materializacao.py:663`. |
| `reconciliar_por_ausencia._levantar` | **Chama `levantar`**, com `resgatado=False` — `app/chamados/services/silencio_ausencia.py:72`. |

Reabertura administrativa e reabertura de competência passam por `reabrir`, **sem levantar o véu**: `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/competencia_trancada.py:258`. Quem efetivamente limpa `silenciado_ate` é `arquivo.levantar` — `app/chamados/services/arquivo.py:201`, persistência em `:208`.

_137 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
