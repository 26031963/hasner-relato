# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 11:13** · publicado em **26/11/2026 11:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 11:13 · sessao a976abdc32aa · - · - · origem=Ronald (tmux)

Censo dos 16 sítios atribuídos. Nas linhas com A e C, a origem de cada entrega está determinada pelo ramo indicado.

| Sítio | Origem da entrega sob a trava |
|---|---|
| `app/chamados/reconciliador.py:558` `retratar_com_motivo` | **A** no gate de `ch.reconciliar`, acionado em `:606`. Perguntas e donos com Empresa relidos em `app/chamados/models.py:364` e julgados em `app/chamados/models.py:387`. O resolvedor de `:616` tem caminhos A e C. A retenção em `:588` passa `ignorar_tranca=True` em `app/chamados/juizes.py:2214`. |
| `app/chamados/reconciliador.py:629` `retratar_perguntas_absolvidas_por_celula` | **A**. Trava C em `:679`, materializa perguntas com `disputa__colaborador__empresa` em `:681` e consulta C20 em `:695`. A chamada de `trava_na_ordem` em `:691` acrescenta as outras travas. |
| `app/chamados/reconciliador.py:1236` `encerrar_orfas_pela_lampada` | **B**, com FK inicialmente fria. O Colaborador de `:1499` é preservado em `:1268`, via `app/chamados/juizes.py:1279`, embora a pergunta seja nova. O primeiro C20 que lê a Empresa aquece a mesma instância em `app/chamados/juizes.py:1117`. O próximo `atomic` adquire C em `:1267` com essa FK em cache da iteração anterior. `:1299`, `:1306` e `:1307` ficam após o `atomic` de cada item. |
| `app/chamados/services/acoes_disputa.py:105` `corrigiveis_sob_a_trava` | **C**. A segunda consulta de `perguntas_corrigiveis`, disparada em `:116`, carrega P em `app/chamados/juizes.py:1853` depois de `:112`. O C6 só é chamado para as carimbadas via `app/chamados/juizes.py:1595`, com dono lazy em `app/chamados/juizes.py:1041`. |
| `app/chamados/services/acoes_disputa.py:343` `acao_corrigir_resposta` | **D** sob `:423`. A pergunta de `:425` chega ao core em `:442` com `exige_viva=False`, `app/chamados/services/disputa_emissao.py:1471` e `:1517`. A hora de `:380` fica fora dos escapes de `app/chamados/services/materializacao.py:1169`. A validação de `:454` vem depois da saída do `atomic`. |
| `app/chamados/services/becos.py:38` `fechar_beco` | **C** no C7 de `:60` depois do `d.refresh_from_db()` integral em `:59`. O reverso é lido de novo em `app/chamados/models.py:2052` e `:2097`, com Empresa fria. O resolver em `:58` tem caminhos A/C. `executar_sob_cerca` executa o callback dentro do bloco, `app/chamados/services/fio.py:1571` e `:1577`. |
| `app/chamados/services/declaracao_texto.py:308` `declarar_hora_colab` | **C** na candidata de `:391`, carregada por `:252` e `app/chamados/services/fio.py:2144` depois de `:363`. Colaborador lazy, Empresa fria no primeiro C6 de `:256` via `app/chamados/juizes.py:1595`. A costura de `:398` pode aquecer a Empresa dentro do mesmo C em `app/chamados/services/fio.py:930`. A releitura de `:369` alimenta a guarda de horário. |
| `app/chamados/services/declaracao_texto.py:669` `aplicar_declaracao` | **C** na candidata de `:698`, mesma carga de `:252` e `app/chamados/services/fio.py:2144`, depois de `:685`. O caminho de reuso do nascimento também pode chegar com Empresa aquecida sob C, `app/chamados/models.py:3185` e `:3258`, `app/chamados/services/fio.py:930`. `:738` fica fora do `atomic` local. |
| `app/chamados/services/disputa_emissao.py:483` `_abrir_ou_agregar_disputa` | **D** nos chamadores identificados. O filtro `status_local__in=VIVOS` em `:586` exclui o renascimento terminal de `app/chamados/models.py:987`, conforme `app/chamados/catalogo/motor.py:24` e `:30`. O módulo não tem chave de data em `app/chamados/catalogo/modulos.py:144`. Os contextos de produção não passam `data_turno` no topo, de modo que o signal de `app/chamados/signals.py:215` não chama cartório. `:641`, `app/chamados/services/fabrica_da_celula.py:172`, `app/api/views.py:761`, `app/api/views_core.py:755`, `app/ponto/services/triagem_batida.py:734`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`. |
| `app/chamados/services/disputa_emissao.py:1316` `criar_disputa_ancora_de_chamado` | **C/A** pelo `analisar` de `:1393`, sob `:1345`. `app/chamados/models.py:869` e `:308` salvam o estado. O signal síncrono `app/chamados/signals.py:215` aceita `data_inicio` do `ancora_limbo`, `app/chamados/catalogo/modulos.py:131`, `app/ponto/management/commands/detectar_ausencias.py:297`. Se houver julgamento da célula, `app/ponto/services/cartorio.py:1701` relê P sem Empresa (C), e `:1026` pode acionar E2 com a carga A de `app/chamados/reconciliador.py:681`. A conversa de `:1385` não entrega um dia de fato. |
| `app/chamados/services/disputa_emissao.py:1471` `responder_pergunta_disputa_core` | **C**. `:1494` recarrega integralmente P depois de `:1490`; `:1519` usa essa P, com a relação lazy em `app/chamados/juizes.py:1041`. A ausência em `:1596` fica depois do `atomic` local e tem trava própria em `app/chamados/services/materializacao.py:1075`. |
| `app/chamados/services/disputa_emissao.py:1985` `resolver_perguntas_por_chamado_resolvido` | **A/C**. Depois de `:2001`, a consulta de `:2074` recebe P ancoradas carregadas com Empresa por `app/chamados/models.py:364` e P de `:2038` sem Empresa. O mapa de `:2071` reaproveita o dono das ancoradas quando há a mesma PK. P adicionais são lidas em `:2167` e `app/chamados/models.py:2052`. `:2055` copia um memo do cartório, sem substituir o dono da P. |
| `app/chamados/services/fio.py:621` `processar_post_fio` | **C**. Sob `:750`, P e Colaborador sem Empresa em `:751`, C6 em `:758`. O render de `:755` e `:759` também relê P em `:296` e julga em `:316` e `:402`. A listagem do anexo inclui outros ramos, como `:782` e `:824`, fora desse `atomic`. |
| `app/chamados/services/fio.py:901` `_costura_nao_mata_o_fio` | **B** no histórico demonstrável. `app/chamados/signals.py:370` carrega o Colaborador numa chamada pós-commit; `app/chamados/reconciliador.py:1669` e `:1672` ligam-no à P e `:866`, alcançada por `:1741`, aquece a FK antes do `reapontar` de `:1794`. A trava `:923` é reentrante nesse caminho. `:930` e `:936` leem a mesma P. Com FK originalmente fria, `:930` a aquece sob C antes de C20. |
| `app/chamados/services/fio.py:959` `reapontar_resolvedora` | **B** no histórico acima, em relação à primeira trava `:1044`. Também **C** no ramo de colisão `:1050`, com outras perguntas de `:1109` e `:2144`, julgadas em `:1115`. |
| `app/chamados/services/fio.py:2402` `migrar_resolvedoras_vivas` | **C** nas primeiras entregas. Depois de `:2446`, as P são outras consultas em `:2478` e `:2482`, com mapa vazio em `:2477` e carga do dono em `app/chamados/juizes.py:1041`. `:2525` pode entregar a mesma instância aquecida pelo C20 sob a mesma trava, `app/chamados/juizes.py:1117`. |

## 26/11 11:12 · sessao 37a822ff5230 · - · - · origem=Ronald (tmux)

| Sítio | Entrega ao C20 sob a trava indicada | Classe |
|---|---|---|
| `limbo_folgas.agrupar_furo` | Depois de `limbo_folgas.py:275`, as perguntas de `models.py:364–366` chegam com Empresa em cache a `models.py:387`. Os fechos de `limbo_folgas.py:284,287` repetem a coleta pelo gate de `models.py:400,708`. | A, com A/C no S143 (`disputa_emissao.py:2038–2042,2071–2074`) |
| `materializacao._materializar_ausencia_de_resposta` | `q` é relida em `materializacao.py:1076`, mas o reverso de `materializacao.py:1032,1048,1058` a prende à mesma `disputa`. O Colaborador é acessado em `materializacao.py:1038` antes de C em `materializacao.py:1075`. A criação síncrona da ausência pode aquecer a Empresa antes, via `ponto/services/ausencia.py:483,529,562`, `ponto/signals.py:85–93,143–147` e `cartorio.py:1016,1225,2114`. Iterações também reutilizam o dono aquecido pela primeira irmã em `juizes.py:1117` depois que o atomic por irmã termina. O C6 de `materializacao.py:1104` passa por `models.py:3445`. | B, com C se a FK chegar fria |
| `materializacao.materializar_perguntas_validadas_da_disputa` | `p` é relida após `materializacao.py:2278` em `materializacao.py:2282`, mas a chamada de `materializacao.py:2291` repassa a disputa de entrada. A pré-triagem de `acoes_disputa.py:169` pode aquecer seu dono pelo reverso de `juizes.py:1600–1601,1041,1117`. Uma iteração anterior também pode aquecê-lo e liberar C. O plantio usa esse mesmo `disputa.colaborador` em `materializacao.py:1581–1582,1698–1699`, preservado em `ponto/registro_batida.py:170–177`, e o sinal de `ponto/signals.py:206–215` chega ao mapa semeado em `reconciliador.py:1672` e a C20 em `reconciliador.py:1741,866,869`. O carimbo direto de pergunta já validada para em `juizes.py:1498`. | B |
| `passivo_validado.reparar` | Depois de `passivo_validado.py:399`, as perguntas e Empresas são relidas em `passivo_validado.py:400–404` e repassadas ao plantio por `passivo_validado.py:409,491,585`. A cauda `passivo_validado.py:447` usa outro atomic e pode abrir a própria trava em `models.py:2123`. | A na trava de `:399` |
| `relavra_via.desfazer_carimbo_sem_lastro` | A consulta `relavra_via.py:236–239` sucede `relavra_via.py:230` e junta só disputa e célula. A primeira leitura da Empresa acontece pelo C20, em `relavra_via.py:242` → `juizes.py:1041,1104–1107`. | C |
| `validacao.devolver_instante_sem_texto_l130` | Pergunta e Empresa carregadas depois de C em `validacao.py:465–467`. C20 via `validacao.py:468,434`. | A |
| `validacao._aceitar_a_folga` | O fecho principal de `validacao.py:563` e o de `validacao.py:588` via `declarar_dia.py:167,206` levam perguntas carregadas em `veredito_celula.py:180–181` e julgadas em `veredito_celula.py:193`, com FK fria, ou as perguntas e a Empresa de `models.py:364–387` no fecho de `veredito_celula.py:208`. Os carimbos `validacao.py:556,596` escrevem via de identidade (`models.py:3347,3491–3520`). | C/A |
| `validacao.validar_pergunta` | A pergunta de `validacao.py:730` sofre `refresh_from_db()` em `validacao.py:746` após C de `validacao.py:745`. A Empresa pode ser aquecida sob C pelo K6 de `validacao.py:782` → `materializacao.py:835`, antes da passagem ao plantio e aos sinais de batida do caminho `validacao.py:815,819`. Folga em `validacao.py:779` tem as entregas acima. `validacao.py:798` agenda `on_commit` (`fechamento.py:1848`), e `validacao.py:830` fica após o atomic local. | A/C |
| `validacao.validar_lote_classe_a` | A pergunta é relida em `validacao.py:1157–1158` após C de `validacao.py:1156`. `validacao.py:1162` calcula classe do lote sem passar por C20 (`validacao.py:1265–1301`), e a entrega ocorre pelo caminho `validacao.py:1165` → `acoes_disputa.py:21` → `validacao.py:746,815,819,830`. O atomic de `validacao.py:1153` abrange também essa cauda. | A/C |
| `views_cobrar.validar_inline` | Guarda `views_cobrar.py:224` usa `_d` relida em `views_cobrar.py:222`, após `views_cobrar.py:218`. Disputa de `fio.py:148–149,153–155`, perguntas via reverso `juizes.py:1600–1601` e Empresa inicialmente fria. `views_cobrar.py:226` carrega a pergunta pelo processo de validação e mantém o atomic externo até a cauda. | C na guarda, A/C na validação |
| `cluster_espurio.tratar_legado_encerrado_sem_lastro` | `cluster_espurio.py:281` relê o Chamado depois de `cluster_espurio.py:279`. O Colaborador direto de `models.py:1110` vai ao K7. O C20 vem das perguntas de `juizes.py:2523–2526`, chamadas por `juizes.py:2545` e `models.py:1109`, ou da ressuscitação em `models.py:1517,1549–1555,1599,1676` e migração em `models.py:1481` → `fio.py:2478–2484`. Nesses seletores o primeiro acesso a Empresa é frio. | C |
| `declarar_dia._escrever_folga` | O fecho `declarar_dia.py:206` carrega perguntas em `veredito_celula.py:180–181`, depois de C em `declarar_dia.py:190`, com FK fria para `veredito_celula.py:193`. A relavra síncrona de `declarar_dia.py:191` → `escala/folgas.py:333,337` → `ponto/portas/celula.py:392,479` usa `ec.colaborador` acessado depois de C. O seletor anterior do vínculo não faz join no colaborador (`escala/servico_jornada.py:46–48,64`, `escala/alimentacao.py:148–153`). Cartório e fecho de chamados têm as entregas frescas de `reconciliador.py:681–695`, `models.py:364–387` e frias de `cartorio.py:145–146,1424`. | C/A |
| `justificativa.encerrar_chamado_de_origem` | Após `justificativa.py:41`, `justificativa.py:42,46` repassa o Chamado. As perguntas com Empresa do gate vêm de `models.py:364–387`. A retenção de `reconciliador.py:451` usa `ignorar_tranca=True` em `juizes.py:2214`. | A, com A/C no S143 |
| `justificativa._encerrar_bloqueio_de_entrada` | Após `justificativa.py:80`, `justificativa.py:81,87` repassa o Chamado. A retenção ignora C20 (`juizes.py:2214`), e `reconciliador.py:543` chama `models.py:1779,1791,364–387` com Empresa em cache fresco. | A, com A/C no S143 |
| `veredito_celula.fechar_o_fio_do_dia` | Perguntas em `veredito_celula.py:180–181` são novas depois da trava C de `veredito_celula.py:176` e antes da segunda trava `veredito_celula.py:185`, que acrescenta Disputa e Chamado. C6 em `veredito_celula.py:193` acessa o dono lazy com Empresa fria. Fechos de `veredito_celula.py:208` carregam Empresa por `models.py:364–387` e abrem S143. | C/A |
| `veredito_celula.veredito_celula` | A view carrega `ponto/views.py:3497` e reutiliza o mesmo Colaborador em vários dias em `ponto/views.py:3518–3519`. A batida `veredito_celula.py:328` preserva a instância em `registro_batida.py:170–177`. Depois que o cartório aquece `c.empresa` numa data (`cartorio.py:1225,1783`), a batida numa data posterior pode reutilizar esse cache anterior à nova trava de `veredito_celula.py:231`. O sinal `ponto/signals.py:206–215` percorre `reconciliador.py:1581,1672,1741,866,869`, que entrega a mesma instância ao C20. `veredito_celula.py:352` também tem o caminho C/A do fecho direto. | B |

Em `materializacao._materializar_ausencia_de_resposta`, `materializacao.py:1151,1154,1155` acontece fora dos atomics de `materializacao.py:1074`. O chamador principal chama a absorção ao sair de seu próprio atomic (`disputa_emissao.py:1568,1596`). Em `materializacao.materializar_perguntas_validadas_da_disputa`, `materializacao.py:2293` também acontece depois do atomic local por pergunta. A trava de um chamador pode persistir nesses trechos, como na validação inline e em lote (`views_cobrar.py:214–226`, `validacao.py:1153–1165,830`).

Nos 16, B demonstrável em `_materializar_ausencia_de_resposta`, `materializar_perguntas_validadas_da_disputa` e `veredito_celula`. Execuções efetivas e frequência em produção [nao verificado].

## 26/11 11:12 · sessao 952a9a3ed210 · - · - · origem=Ronald (tmux)

`models.py` abaixo é `app/chamados/models.py`. As referências abreviadas de comandos ficam em `app/chamados/management/commands/`.

| Sítio | Instância e entrega |
|---|---|
| `app/api/foto_recebida.py:_validar_pelo_dono` | **C, com A na cascata.** A validação `:93` está sob a segunda aquisição `:90`. A primeira, `:73`, cobre a resposta automática, que relê e escreve sem C6. `validacao.py:726–730,746` carrega nova Pergunta sob C, usando a disputa antiga só como filtro. A cauda `validacao.py:830` → `materializacao.py:2293,2137` → `juizes.py:1600–1601` lê o colaborador da disputa recém-carregada. Empresa fria para o motivo foto de tipo escolha, com A no fecho via `_ancoradas`. `app/chamados/services/disputa_emissao.py:1746–1763`, `app/chamados/catalogo/perguntas.py:94–97`, `app/chamados/services/validacao.py:234,798`, `app/chamados/services/materializacao.py:825–835,1361,1772`, `models.py:364–365`. |
| `backfill_fio_morto_e4b.py:_executar_dia` | **A/C.** `:163` só roda no ramo `:161`. `:166,:169` herdam o C exterior de `:361–362,:404` também quando o ramo `:161` é pulado. A fábrica alcança Pergunta relida com `disputa` e `celula`, Empresa fria, em `app/chamados/services/fabrica_da_celula.py:207–209` → `app/chamados/services/relavra_via.py:230,236–242`. No plantio, a disputa semente sai de `:136–137` e a Pergunta é relida em `app/chamados/services/materializacao.py:2282`. A cauda `:2293,:2137` chama `app/chamados/juizes.py:1600–1601` ainda sob C exterior. Empresa pode estar fria ou ter sido acessada sob C no intervalo `materializacao.py:825–835`; o gate de chamado também carrega Empresa sob C em `models.py:364–365`. `:149–153` tem C6 em `disputa_emissao.py:1941` antes do `:163` e sob o C exterior, com Empresa fria inicialmente. |
| `backfill_fio_morto_e4b.py:handle` | **A/C.** `:361–362` mantém C no replanejamento e na execução. `:390` → `:90` envia ao lote o cadastro com Empresa relido em `app/chamados/services/tarefa_do_dia.py:528,554–555,837`; `:92–94` → `app/chamados/services/disputa_emissao.py:1941` usa Pergunta recém-carregada sem Empresa. `:397` repete o inventário de `:263`; `:404` chama `_executar_dia` acima. `:440` → `:81` → `app/chamados/services/vigia_fio.py:580–581,614` → `app/chamados/juizes.py:1751–1757` e `tarefa_do_dia.py:528,555` releem Empresa para os donos do plano, além de ler outros colaboradores no alcance global. O anexo omite `:406`, que pode executar deliberadamente o callback do beco sob C. `app/chamados/services/foto_e4b.py:51–54,75–80`, `app/chamados/signals.py:235–240`, `app/chamados/services/becos.py:35,86–92`. As fotos em `:365,:384,:407,:435` não entregam suas Perguntas ao C20. `foto_e4b.py:235–239,254–280,326–333`. |
| `backfill_fio_morto_e4b.py:_travar_colaboradores` | **C.** `:244` → `:252` → `:216–221` → `app/chamados/services/disputa_emissao.py:1917` → `models.py:1549–1555,1599`. A Consulta de ressuscitação carrega Pergunta, Disputa e Célula sob C. Colaborador lazy em `app/chamados/juizes.py:1041`, Empresa fria no primeiro par. |
| `estoque_l130_instante_sem_texto.py:handle` | **A.** Sob `:78`, Pergunta com Empresa em `:97–99` → `app/chamados/services/validacao.py:434`. `:105` delega e relê novamente com Empresa em `validacao.py:465,467–468`. |
| `reatar_beco_folga_ao_limbo.py:_rodar` | **D.** O pai relido sob `:112–113` está na fila, portanto em `COBRANCA`. A chamada `:118` → `app/chamados/services/limbo_folgas.py:238–244` não passa data. A colisão de data é pulada e a guarda de costura retorna antes de C6 para destino vivo. `app/chamados/services/painel.py:188–189`, `app/chamados/catalogo/motor.py:29–31`, `app/chamados/services/fio.py:924–936,959,1022,1049,1064`. |
| `validar_classe_a.py:handle` | **A/C.** Trava em `:124`, Pergunta de reclassificação em `:125`, classificador de resposta em `:128` sem C20. `:132` passa a disputa prévia somente como filtro. A cauda da validação e as cascatas ainda rodam no `atomic` exterior de `:118`. Pergunta nova `app/chamados/services/validacao.py:726–730,746`; Colaborador lazy `:234,798`; C20 via `:830` → `app/chamados/services/materializacao.py:2293,2137` → `app/chamados/juizes.py:1600–1601`. Empresa fria quando ainda não acessada, cache pós-C quando o intervalo lê K6 ou `_ancoradas` carrega o grafo no fecho. `validar_classe_a.py:37–42`, `juizes.py:2131–2149`, `materializacao.py:825–835`, `models.py:364–365`. |
| `models.py:ChamadoColaborador.abrir` | **C.** `:971–972` → `:988` → `:1085` → `:1517` → `:1636,1676` → `:1549–1555,1599`. A Pergunta da ressuscitação carrega pós-C, com Colaborador lazy e Empresa fria no primeiro par em `app/chamados/juizes.py:1041`. Redirecionamento para irmão em `models.py:1481` faz o mesmo com Perguntas de `app/chamados/services/fio.py:2478–2484`, sob `:2446`. A guarda direta de `models.py:1075` usa K7 em `:1099–1110`. |
| `models.py:ChamadoColaborador.ressuscitar_perguntas` | **C.** `:1636` é triagem antes de `:1655`; `:1676` reavalia o universo de `:1549–1555,1599`, cujo mapa começa sem sementes. Empresa fria no primeiro par, reutilização possível dentro do próprio laço. `models.py:1562,1570–1574`, `app/chamados/juizes.py:1041,1117,1275–1283`. |
| `models.py:DisputaSupervisao.fechar` | **B demonstrável**, com C se a FK segue fria e A quando a foto veio de C exterior. `:2097` carrega Perguntas via reverso `self.perguntas`; `.disputa` leva ao mesmo `self` nas guardas `:2116` e `:2138`. `:2130` atualiza campos de estado da disputa. O C20 de triagem preenche a Empresa antes de `:2123`; caso externo concreto em `encerrar_perguntas_com_fato.py:82–89`, que faz `fio_vivo` no mesmo d antes de chamar `fechar`. Caminho A via E2 em `app/chamados/reconciliador.py:679,681–683,707,719`. `models.py:2045,2107,2688–2691`, `app/chamados/juizes.py:1041,1117`. |
| `models.py:DisputaSupervisao.remover_motivo` | **D no escopo local, C com C exterior retido.** `:2253,2259` têm o `atomic` que termina após `:2296`; o carimbo de identidade `:2292` grava sem C6 (`:3491–3521`). `:2303` fica fora do `atomic` local e `fechar` tem aquisição própria em `:2123`. Em chamada por `app/colaboradores/services/estrutura.py:66–77` e `app/colaboradores/signals.py:71–77`, o `atomic` exterior retém C para a cauda, cuja disputa nasce do queryset simples do receiver. A entrada `app/colaboradores/views.py:893–913` → `estrutura.py:82–86` não abre esse `atomic` exterior. `models.py:2097,2107,2116`. |
| `models.py:DisputaSupervisao.reabrir_por_colab` | **C.** `:2469` → `:2481` chama novamente o juiz de `app/chamados/juizes.py:1853–1854`, que carrega Pergunta pelo manager global. `:1595` chama C6 para a materializada. Colaborador lazy em `:1041`, Empresa fria. |
| `models.py:PerguntaDisputa._nascer` | **A.** C em `:3123` via `:2898`; `ex` vem do universo pós-C `:3185` → `app/chamados/services/fio.py:2144–2145,1776–1778` → `models.py:2995–3001`, com `disputa__colaborador`, ou do novo `get` em `:3235`. No ramo de destino terminal, `:3258` → `fio.py:930` lê Empresa sob C antes de entregar a mesma Pergunta em `:936`. Destino vivo retorna em `fio.py:926–927`; nascimento novo em `models.py:3292–3297` não passa por essa guarda. |

## 26/11 11:10 · sessao 5986b5cfd949 · O30-F3-CONSTRUIR · verificador · origem=Code

**O dinheiro muda quando a cura grava uma falta aprovada sem fim e com minutos zerados.** A c5 entrega o fim ao `Ausencia.save` antes da soma. Com isso, a R8 lê o intervalo inteiro, enquanto a base somava apenas o primeiro dia nessa chamada. Também ficaram duas regras para proteger o anexo compartilhado e uma janela de concorrência entre a consulta ao K6 e a exportação. Quantidade de linhas e valor em produção **[nao verificado]**. (`app/ponto/services/ausencia.py:1924`, `app/ponto/services/ausencia.py:1934`, `app/ponto/services/ausencia.py:1942`, `app/ponto/models.py:901`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:618`, `app/ponto/services/ausencia.py:312`, `app/ponto/services/ausencia.py:1175`, `app/folha/models.py:174`)

A cópia corresponde aos trechos c5 inspecionados. Ela veio sem `.git`, então a identidade do commit e a igualdade integral de `0191ece77..aa32babb3` com o anexo ficam **[nao verificado]**. O diff colado também omite a remoção da tag e da biblioteca narrada pelo construtor. Conferi o estado final do template e as referências na cópia. (`app/ponto/services/ausencia.py:1942`, `app/templates/ponto/partials/painel_ausencia.html:1`, `app/ponto/views.py:2103`)

## 1. PROCEDE — escritores e leitores

O registro vivo atribui a cobertura e os dias ocupados a `ponto/turnos.py`, os minutos previstos a `escala/utils.py`, a exigência do documento a `estado_inicial` e os estados abertos a `ABERTAS`. Para dinheiro, o recorte é a janela da empresa e o juiz da entrega é o K6. (`app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:58`, `app/docs/JUIZES.md:131`, `app/core/juizes.py:193`, `app/core/juizes.py:259`, `app/core/juizes.py:349`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `data_fim` | `criar_ausencia` calcula e insere, `editar_ausencia_registro` calcula e grava, `rederivar_fim` grava o fim conferido, `Ausencia.save` preenche fim ausente, `mudar_estado` persiste a derivação do modelo. (`app/ponto/services/ausencia.py:425`, `:569`, `:660`, `:740`, `:763`, `:1102`, `:1942`; `app/ponto/models.py:918`) | Cobertura, sobreposição e expansão por dia, cálculo de minutos no modelo, insumo e invalidação da folha, rateio de faltas, signal do cartório, recorte do lote, histórico e painel. (`app/ponto/turnos.py:2540`, `:2555`, `:2796`, `:2851`, `:2870`; `app/ponto/models.py:906`; `app/ponto/services/ausencia.py:119`, `:189`, `:1924`; `app/folha/export.py:63`, `:87`, `:117`; `app/ponto/signals.py:115`; `app/ponto/services/atestados_lote.py:55`; `app/ponto/services/dia_decidido.py:681`; `app/templates/ponto/partials/painel_ausencia.html:159`) |
| `minutos` | Entrada de criação e edição, derivação R8 de falta aprovada com zero no modelo, persistência pelas portas de estado e cura. H06 também chega pela edição. (`app/ponto/services/ausencia.py:569`, `:771`, `:1102`, `:1943`; `app/ponto/models.py:901`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:164`) | Juiz de parcialidade, insumo da folha, rateio e total de faltas, cartão/histórico/CSV, API e painel. (`app/ponto/turnos.py:2617`; `app/ponto/services/ausencia.py:119`; `app/folha/export.py:121`, `:140`; `app/ponto/services/fechamento.py:893`; `app/ponto/services/dia_decidido.py:551`, `:578`; `app/ponto/services/atestados_lote.py:149`; `app/api/views.py:1879`; `app/templates/ponto/partials/painel_ausencia.html:160`) |
| `dias_corridos` | Criação e edição recebem a duração, `Ausencia.save` ajusta pelo intervalo preenchido, estado e cura persistem a coluna. O espelho de férias nasce pela criação com um dia. (`app/ponto/services/ausencia.py:569`, `:764`, `:1102`, `:1561`, `:1943`; `app/ponto/models.py:917`) | Modelo e comando de cura leem a duração para propor fim. Histórico usa a duração gravada quando o fim é nulo. Painel mostra a duração e a usa no checkbox. CSV recebe `dias` do histórico. (`app/ponto/models.py:917`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`; `app/ponto/services/ausencia.py:1924`; `app/ponto/services/dia_decidido.py:686`; `app/templates/ponto/partials/painel_ausencia.html:158`, `:194`; `app/ponto/services/atestados_lote.py:149`) |
| `aprovada_em` e `atualizado_em` | Criação de aprovada e decisão explícita escrevem `aprovada_em`. O modelo o preenche quando falta em aprovada ou rejeitada. `atualizado_em` é `auto_now` e entra nos `update_fields` da edição, decisão com período, rejeição de férias e cura. (`app/ponto/services/ausencia.py:576`, `:772`, `:1253`, `:1266`, `:1668`, `:1943`; `app/ponto/models.py:834`, `:838`, `:896`) | `quem_quando` alimenta cartão, histórico e CSV. O painel lê `aprovada_em` e o cartório consulta os dois carimbos para detectar fato novo. H06 lê `atualizado_em` na reversão. (`app/ponto/services/dia_decidido.py:558`, `:573`; `app/ponto/services/atestados_lote.py:152`; `app/templates/ponto/partials/painel_ausencia.html:310`; `app/ponto/services/cartorio.py:401`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`) |
| `documento_referencia`, `documento` e os seis campos de atestado | Referência no INSERT, com `SET_NULL` na exclusão da raiz. Arquivo na criação, em `anexar_documento` e em `_reabrir`. Os seis campos entram no INSERT ou no `UPDATE` de `_gravar_dados_atestado`, chamado pelas portas de criação, edição, anexo e reabertura. (`app/ponto/models.py:815`; `app/ponto/services/ausencia.py:326`, `:572`, `:575`, `:580`, `:652`, `:664`, `:774`, `:1180`, `:1183`, `:1472`, `:1476`) | Raiz e irmãs na porta, gate de aprovação, comando de rejulgamento, drawer, visualização protegida, aprovar/rejeitar e lista. Lote lê a raiz para extensão, metadados CSV/PDF e bytes PDF/ZIP. Histórico lê tipo e presença do arquivo na raiz. API, troca de plantão e rótulo do dia também têm leitores do arquivo próprio. (`app/ponto/services/ausencia.py:283`, `:803`, `:869`, `:890`, `:1122`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, `:38`; `app/ponto/views.py:2103`, `:2148`, `:2787`, `:2863`; `app/ponto/views_atestado.py:30`; `app/templates/ponto/ausencias.html:170`; `app/templates/ponto/partials/painel_ausencia.html:71`, `:99`, `:125`, `:162`; `app/ponto/services/atestados_lote.py:107`, `:119`, `:178`, `:255`; `app/ponto/services/dia_decidido.py:477`, `:555`, `:674`, `:688`; `app/api/views.py:1061`; `app/ponto/portas/celula.py:726`, `:746`) |

## 2. PROCEDE — classes com resíduos

| Classe | Veredito |
|---|---|
| **Segundo juiz** | **PROCEDE.** `servir_documento_ausencia`, aprovar, rejeitar e a lista ainda resolvem o elo por conta própria. Em `A → B → C`, o drawer recusa `A` porque `B` também aponta para outra linha. A entrega protegida tenta servir o arquivo de `B`, podendo devolver bytes ou 404. Também há duas regras para substituir arquivo compartilhado. A guarda da raiz só considera `aprovada`. A guarda de irmãs considera `aprovada` e `rejeitada`, só em anexo próprio. Uma filha aberta pode substituir o arquivo de uma raiz rejeitada se nenhuma filha estiver decidida. O alcance desse caso no estoque fica **[nao verificado]**. (`app/ponto/services/ausencia.py:283`, `:312`, `:1171`, `:1175`; `app/ponto/views.py:2103`, `:2148`, `:2787`, `:2863`; `app/templates/ponto/ausencias.html:170`) |
| **Segundo escritor fora da porta** | **NAO PROCEDE na cópia.** A varredura estática de produção com `detalhar_saves=True` retornou zero escritas atribuídas a `Ausencia` fora do serviço e do modelo. O admin é de leitura e `SET_NULL` é a ação declarada da FK. (`app/core/censo_escritas.py:396`, `:455`; `app/ponto/admin.py:38`; `app/ponto/models.py:815`, `:892`; `app/ponto/services/ausencia.py:1943`) |
| **Relógio fora da borda** | **PROCEDE como resíduo.** A leitura nova da c4 saiu de `rederivar_fim`. A cura ainda passa pelo `_tz.now()` do modelo se faltar `aprovada_em`, pelo `auto_now` e, quando o insumo anterior tinha fim nulo, pelo `localdate()` de `_acordar_projecao`. O `post_save` síncrono do cartório também lê `localdate()`. O default de mês/ano do lote é lido na view e entregue explicitamente ao serviço. (`app/ponto/services/ausencia.py:177`, `:190`, `:1943`; `app/ponto/models.py:838`, `:896`; `app/ponto/signals.py:115`, `:143`; `app/ponto/views_atestado.py:103`; `app/ponto/services/atestados_lote.py:35`) |
| **Derivação paralela** | **PROCEDE para o fim residual.** A cópia c4 de **minutos, dias corridos, carimbo e validação** saiu da cura. No caminho com fim proposto preenchido, o modelo usa esse fim. A fórmula do **fim** continua em três lugares, `derivar_data_fim`, o `elif` de `Ausencia.save` e a prévia do comando. Uma alteração na função real pode fazer o dry anunciar outro fim. O duplo cálculo no caminho da cura com fim preenchido **NAO PROCEDE**. (`app/ponto/services/ausencia.py:198`, `:1924`, `:1942`; `app/ponto/models.py:918`, `:926`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:37`) |
| **Selo ou RED sem mordida** | **PROCEDE nas fronteiras.** Os 46 testes sem banco passaram. Rodei `d5_ignora_fim_cercado`, os cinco mutantes `c5_` do trecho final, `save_integral`, `d4_append` e `d4_atribuicao_em_ramo`. Todos deram `FAIL` por asserção. `d1_zip_le_filha` e `d1_historico_le_filha` também deram `FAIL`, mas a fixture injeta uma filha diretamente e os dois alimentadores de produção selecionam só raízes. O selo de referência verifica atribuição direta e `.update()` com keyword literal dentro do serviço. O do painel verifica presença textual e atribuição a `raiz`, sem renderizar nem exigir que a view entregue a raiz correta. REDs dependentes de commits e suíte com banco **[nao verificado]**. (`app/ponto/tests/mutantes_atestado_f3.py:134`, `:204`, `:218`, `:230`, `:380`; `app/ponto/tests/test_atestado_f3_contrato.py:241`, `:257`, `:273`, `:624`, `:735`, `:747`; `app/ponto/services/dia_decidido.py:624`; `app/ponto/services/atestados_lote.py:50`; `app/ponto/views.py:2042`) |
| **Ordem de trava** | **NAO PROCEDE** uma inversão demonstrada no grupo do documento. O caminho trava raiz, registro solicitado e, quando necessário, filhas por PK. **PROCEDE** a janela de concorrência com exportação. A cura trava `Ausencia`, consulta K6 e só depois salva. A emissão do TXT pode registrar uma exportação entre a consulta e o save, pois o registro da exportação usa outro conjunto de linhas, sem exclusão comum com a cura. Ocorrência em produção **[nao verificado]**. (`app/ponto/services/ausencia.py:296`, `:1122`, `:1921`, `:1934`, `:1943`; `app/folha/export.py:588`; `app/folha/views.py:270`, `:299`; `app/folha/models.py:174`) |
| **Pouso antes do pré-requisito** | **PROCEDE como pendência de verificação.** A mudança de leitura pressupõe estoque sem cadeia. O anexo relata zero cadeias de segunda mão, sem trazer a sonda primária. Uma cadeia legada pode derrubar o drawer ou abortar um comando de acervo depois de outras linhas já processadas. A migration `0079` está na cópia e o deploy prevê migrar antes do reload. Aplicação da migration, suíte inteira verde, cadeia em produção e pouso efetivo **[nao verificado]**. (`app/ponto/services/ausencia.py:290`, `:1921`; `app/ponto/views.py:2103`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`; `app/ponto/migrations/0079_ausencia_atestado_dados.py:8`; `bin/deploy.sh:316`; `CLAUDE.md:398`) |
| **ESMERIL-SEMANTICA** | **PROCEDE para fim nulo.** O checkbox chama `NULL` com `dias_corridos > 1` de indeterminado. A cura usa exatamente essa forma como fim faltante quando o tipo admite fim, e a cobertura trata a falta sem fim como dia único até a cura. Para `data_fim` preenchida, **NAO PROCEDE** a divergência inclusiva/exclusiva nos caminhos auditados. Modelo, cobertura e cerca incluem a última data. A parcial noturna ancora `hora_ini` e `hora_fim` no dia de início do turno. A R8 caminha pelas datas do intervalo e pergunta o previsto da jornada ancorada em cada data, inclusive quando ela cruza meia-noite. (`app/templates/ponto/partials/painel_ausencia.html:194`; `app/ponto/models.py:751`, `:918`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`; `app/ponto/turnos.py:2555`, `:2644`, `:2851`; `app/ponto/services/ausencia.py:1934`; `app/escala/utils.py:2320`) |

## 3. NAO PROCEDE — leitor órfão pela L-111

A busca por `_fixar_raiz_documento` deixou só a lápide. A busca por `ausencia_tags`, pelo antigo uso da tag e por imports correspondentes deu zero na cópia. O único render do partial entrega `raiz` no contexto, e o partial lê essa variável diretamente. Isso cobre o vínculo estático entre view e template. Renderização nas duas cascas **[nao verificado]**. (`app/docs/LEIS.md:137`, `app/ponto/services/ausencia.py:304`, `app/ponto/views.py:2103`, `app/ponto/views.py:2108`, `app/templates/ponto/partials/painel_ausencia.html:1`, `app/templates/ponto/partials/painel_ausencia.html:71`)

## 4. PROCEDE — mudança de insumo de folha

- **Chamador:** o único chamador de produção localizado é `curar_ausencia_sem_fim --apply`, com `somente_sem_fim=True`. A configuração o declara fora do pipeline, como ato de acervo. Sem `--apply`, o comando só imprime. Os signals são consumidores do `save`. Frequência real de execução, execução manual anterior e agenda instalada **[nao verificado]**. (`app/config/crons.py:769`, `app/config/crons.py:1302`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`)
- **Universo:** falta aprovada, `data_fim IS NULL`, `dias_corridos > 1` e fim retornado pela função real. O número gravado de minutos só muda se era zero e a soma prevista for positiva. A diferença **c5 versus base** exige minutos zerados e previsto positivo depois do primeiro dia. Mesmo com minutos positivos já gravados, a cura amplia o intervalo que a folha usa no rateio. `dias_corridos` já coincide com a duração do fim derivado nesse recorte. Um fim já preenchido é preservado na chamada genérica e é excluído da chamada do comando. A coluna sozinha não identifica a origem desse fim. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `:48`; `app/ponto/services/ausencia.py:198`, `:1924`, `:1942`; `app/ponto/models.py:901`, `:917`; `app/folha/export.py:113`, `:121`; `app/ponto/tests/test_atestado_f3_contrato.py:747`)
- **Exportadas:** se qualquer dia do intervalo cair em exportação vigente pelo K6, a porta retorna antes de gravar a ausência inteira, inclusive quando os demais dias pertencem a uma competência aberta. A `GUARDA_COMPETENCIA` atua em outro ponto e na cópia só lista 09/2026. Para uma cura admitida, `_acordar_projecao` invalida o carimbo. O fechamento posterior pode consumir o intervalo e os minutos novos pelo rateio, se o catálogo classificar o tipo como desconto. A janela de concorrência do item 2 impede afirmar proteção atômica entre exportar e curar. Valores gravados, valor em reais e efeito em TXT de produção **[nao verificado]**. (`app/ponto/services/ausencia.py:1930`, `:1945`; `app/chamados/services/vigia_fio.py:814`; `app/folha/export.py:580`, `:138`; `app/ponto/services/fechamento.py:36`, `:578`, `:893`, `:1676`; `CLAUDE.md:422`)

## 5. PROCEDE — sonda de leitura entregue

O script lê o schema `juliani`, recorte que o backup da sombra restaura. Use `python3 -` a partir de `app/`, com o bloco na entrada padrão. `colNNN` usa o PK com pelo menos três dígitos. A saída conta cada ausência uma vez por competência tocada e mostra 11/12 de 2026 e todas as competências efetivamente entregues pelo K6, com zeros por empresa. `fim_gravado` é o cenário de chamada direta, separado de `cura_sem_fim`, que é o caminho do comando. As linhas barradas aparecem com a proposta simulada. O script desativa a abertura dos logs de arquivo antes do `django.setup()`. Execução na sombra **[nao verificado]**. (`bin/sombra.sh:244`, `bin/sombra_mascara.py:64`, `app/config/settings/sombra.py:47`, `app/config/settings/sombra.py:60`, `app/config/settings/saas.py:136`, `app/config/settings/saas.py:160`, `app/ponto/services/dia_pago.py:702`, `app/folha/export.py:556`, `app/ponto/services/ausencia.py:1924`)

```python
import os
import sys
from collections import Counter, defaultdict

sys.dont_write_bytecode = True
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.sombra")

import django
from django.conf import settings

assert settings.HASNER_SOMBRA is True
assert settings.DATABASES["default"]["NAME"] == "sombra"
assert settings.DATABASES["default"]["HOST"] == "sombra_db"
settings.LOGGING_CONFIG = None
django.setup()

from django.db import connection, transaction
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from escala.utils import minutos_previstos_do_dia
from folha.export import competencia_entregue
from folha.models import ExportacaoDominio
from ponto.models import Ausencia
from ponto.services.ausencia import derivar_data_fim
from ponto.services.dia_pago import competencias_do_intervalo
from ponto.turnos import dias_da_ausencia

meses_alvo = {(11, 2026), (12, 2026)}
modos = ("cura_sem_fim", "fim_gravado")
totais = defaultdict(Counter)

with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database()")
        assert cursor.fetchone() == ("sombra",)

    empresas = list(Empresa.objects.order_by("pk").values_list("pk", flat=True))
    triplas = ExportacaoDominio.objects.order_by().values_list(
        "empresa_id", "mes", "ano"
    ).distinct()
    entregues = {tripla for tripla in triplas if competencia_entregue(*tripla)}
    recorte = entregues | {
        (empresa, mes, ano) for empresa in empresas for mes, ano in meses_alvo
    }

    ausencias = (
        Ausencia.objects.filter(tipo="falta", status="aprovada", dias_corridos__gt=1)
        .select_related("colaborador__empresa", "documento_referencia")
        .order_by("pk")
    )
    for a in ausencias.iterator(chunk_size=500):
        fim = a.data_fim or derivar_data_fim(a.tipo, a.data, a.dias_corridos, False)
        if fim is None or fim < a.data:
            continue

        empresa = a.colaborador.empresa
        comps = {
            (empresa.pk, mes, ano)
            for mes, ano in competencias_do_intervalo(a.data, fim, empresa)
        }
        exibidas = comps & recorte
        if not exibidas:
            continue

        modo = "cura_sem_fim" if a.data_fim is None else "fim_gravado"
        entregues_no_intervalo = comps & entregues
        referencia_invalida = (
            a.documento_referencia_id is not None
            and a.documento_referencia.documento_referencia_id is not None
        )
        dias = dias_da_ausencia(a.tipo, a.data, fim, a.data, fim)
        previsto = (
            [minutos_previstos_do_dia(a.colaborador, dia) for dia in dias]
            if not a.minutos else []
        )
        minutos_r8 = sum(previsto) if previsto else a.minutos
        muda_minutos = minutos_r8 != a.minutos
        muda_dias = len(dias) != a.dias_corridos
        muda_fim = fim != a.data_fim
        afetada = muda_minutos or muda_dias or muda_fim
        delta_c5 = sum(previsto[1:]) if modo == "cura_sem_fim" else 0
        passaria = not referencia_invalida and not entregues_no_intervalo

        for chave in exibidas:
            n = totais[chave, modo]
            n["candidatas"] += 1
            n["mudam_fim"] += muda_fim
            n["mudam_dias_corridos"] += muda_dias
            n["mudam_minutos"] += muda_minutos
            n["minutos_c5_diferem_da_base"] += delta_c5 != 0
            n["afetadas_barradas_k6"] += bool(afetada and entregues_no_intervalo)
            n["afetadas_ref_invalida"] += bool(afetada and referencia_invalida)
            n["afetadas_passariam"] += bool(afetada and passaria)

        if afetada:
            rotulos = lambda chaves: ",".join(
                f"{mes:02d}/{ano}"
                for _, mes, ano in sorted(chaves, key=lambda k: (k[2], k[1]))
            ) or "-"
           

[truncado em 20000 de 21177 caracteres]

## 26/11 11:08 · sessao 86c45000af0e · R1-SEM-DONO-2611 · leitura · origem=Code

Nas seis chaves, o E6 selecionou `minutos_realizados` e calculou **piso − realizado**. O teto é a soma dos vãos dos trechos e só entra na classificação de faixa quando há trecho ímpar. As seis chaves têm `impares=false`, `guarda_janela=false` e dono `ESTRUTURA`. A correspondência com os deltas publicados do R1 e a identidade da foto lida estão **[nao verificado]**. `SONDA1/roda.out:15–21` · `app/ponto/management/commands/e6_oraculo.py:263–269,339–404` · `app/ponto/calculador/nucleo.py:235–254`

## 1. Ramos que produziram os seis deltas

O E6 recebeu os instantes apuráveis em hora local, sem `pares` nem `envelopes`. Nesse ramo, `brutas` conta por data civil, `horas` agrupa pela data da primeira batida do trecho, o piso soma os pares por posição e o teto soma `última − primeira` de cada trecho. No espelho, o realizado agrupa pelo `data_turno` do juiz. Um carimbo de 60 confirma a passagem pelo ramo de desconto da janela declarada. Um carimbo de zero deixa em aberto qual ramo do realizado forneceu o saldo. `app/ponto/management/commands/e6_oraculo.py:149–154` · `app/ponto/calculador/nucleo.py:157–159,178–254` · `app/ponto/services/espelho.py:625–628,689–690,801,835–837` · `app/ponto/turnos.py:719–771`

| Dia de 2026 | Realizado · brutas · piso / teto · delta em min | O que fecha pelo dado e o que falta medir |
|---|---|---|
| **col114 15/11** | **499 · 4 · 439,847646 / 499,385183 · −59,152354 → −59** | O E6 tem um trecho com dois pares, `09:58→15:05` e `16:05→18:17`. Teto menos piso dá **59,537537 min**. O realizado coincide com as bordas `09:58→18:17` truncadas ao minuto e traz janela **0**. As bordas efetivamente escolhidas e o reconhecimento de `15:05→16:05` como pausa pelo juiz estão **[nao verificado]**. Ler `data_turno`, `_fim_fechado`, a pausa valorizada e `borda_posicional`. `SONDA1/roda.out:15` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:542–546,605–622,719–771` |
| **col130 29/10** | **335 · 1 · 671,905433 / 729,936250 · +336,905433 → +337** | O E6 soma `18:56→00:33` e `01:31→07:06` no trecho chaveado em **29/10**. Três pontas são de 30/10. O espelho descontou **60** de janela. Repondo esses 60, o realizado fica em 395 e sobra uma diferença de **276,905433 min** para o piso. A atribuição e o fim contado de cada turno em 29 e 30/10 estão **[nao verificado]**. `SONDA1/roda.out:16` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:285–324,733–771` |
| **col220 10/11** | **337 · 1 · 430,420683 / 486,063217 · +93,420683 → +93** | O E6 soma `21:54→03:35` e `04:31→06:00` no trecho chaveado em **10/11**. Três pontas são de 11/11. O espelho descontou **60** de janela. Repondo esses 60, o realizado fica em 397 e sobra **33,420683 min** para o piso. O papel da ata para `03:35`, `04:31` e `06:00`, o fim contado e o dia de cada turno estão **[nao verificado]**. `SONDA1/roda.out:17` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:609–622,733–771,1940–1944` |
| **col874 22/10** | **667 · 3 · 382,822567 / 382,822567 · −284,177433 → −284** | O E6 soma apenas `15:45→22:07` na chave de 22/10. A terceira batida civil foi agrupada em chave anterior. O espelho descontou **60** de janela e ainda computou 667, acima da duração do único par do E6 na chave. O destino da terceira batida, as bordas que entraram nos 667 e a contribuição de cada turno estão **[nao verificado]**. `SONDA1/roda.out:18` · `app/ponto/calculador/nucleo.py:215–247` · `app/ponto/turnos.py:719–771` |
| **col989 23/10** | **721 · 2 · 567,370050 / 567,370050 · −153,629950 → −154** | O E6 soma um único par, `08:32→17:59`. O realizado com janela **0** excede em **153,629950 min** todo o trecho do E6. As mesmas duas bordas contadas uma vez não produzem 721. A borda efetiva, eventual turno adicional e sua presença nos cartões estão **[nao verificado]**. `SONDA1/roda.out:19` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:719–771` · `app/ponto/services/espelho.py:465–474,778–801` |
| **col996 10/11** | **443 · 5 · 1026,385217 / 1116,385217 · +583,385217 → +583** | O E6 abre dois trechos na mesma chave. O primeiro soma `06:57→13:00` e `14:30→15:50`, com vão interno de **90 min**. A batida de `21:16` entra **mais de 14 h** após `06:57`, com gap de cerca de **5 h 26** desde `15:50`. Assim, o corte vem do **vão total** com quatro pontas, o que explica `cortes=false`. O segundo trecho soma `21:16→06:59` na chave de **10/11**. O delta tem a ordem de grandeza desse par inteiro. As pontas efetivamente contadas nos 443 e o destino do par noturno no juiz estão **[nao verificado]**. `SONDA1/roda.out:20` · `app/ponto/calculador/nucleo.py:215–254` · `app/ponto/turnos.py:719–771` |

## 2. Encaixe nas obras

- **Pausa.** Col114 15/11 tem o contraste aritmético da família **C**. Em col130, col220 e col874, **C** aparece no ramo do espelho como desconto comprovado de 60, com diferença residual em todos os três. A L-032 e o corte de **14/09** regem a pausa valorizada e a janela declarada. `INTERVALO-PRE-ASSINALADO-CONTRATO` V2 trata a dedução no motor e a intrajornada pela L-031/L-032. A aplicação dessa obra aos deltas está **[nao verificado]**. `SONDA1/roda.out:15–18` · `app/docs/LEIS.md:79–80` · `app/ponto/turnos.py:707–717,756–771` · `app/docs/BACKLOG.md:2554`
- **Dia e pareamento.** Col130, col220, col874 e col996 mostram o problema de **comparar chaves civis e de turno**, família **B**, com a chave do E6 demonstrada e a do juiz **[nao verificado]**. Para col996, o **corte por vão** do lado E6 também tem família expressamente registrada em `ORACULO-E6-43-DEFEITOS`. A L-085 rege o dia do plantão. `MOTOR-LE-O-TURNO-DIA-DO-PAR` trata a herança de 0–6 h na lavra/FT. `NOTURNO-A-JANELA-NO-DIA-DO-TURNO-2411-0210` trata a âncora da janela de HE pela L-085/L-097. `PAR-PELO-TURNO-V6-2311-1958` trata a separação da jornada pelo teto `previsto + 4 h` do pareador. O corte comprovado de col996 é o **vão de 14 h do E6**. O nexo de cada delta com essas obras do motor ou do pareador está **[nao verificado]**. `SONDA1/roda.out:16–18,20` · `app/docs/LEIS.md:115–117,154` · `app/docs/GRADE-MESTRA.md:303` · `app/docs/BACKLOG.md:1989,1991,2020` · `app/docs/CORTES.md:646` · `app/ponto/calculador/nucleo.py:215–224` · `app/ponto/motor_calculo_v2.py:1545–1567,2415–2429`
- **Geometria, previsão e abono.** Col989 tem separação de geometria a apurar, família **A**. O encaixe em **D** depende de encontrar a diferença entre os turnos somados e os cartões. A **L-115** é o corte aplicável se aparecer borda posicional após pausa fechada. `O327` trata falha de lavratura do oráculo de folha, `E1-PREVISAO-24H` trata previsão inválida e `ABONADO-COM-SAIDA-ANTECIPADA` trata desconto de pontualidade em dia coberto pelo abono, pelo corte `ABONADO-COM-SAIDA-NA-ZONA-INVIOLAVEL`. Esses três mecanismos estão **[nao verificado]** nas seis chaves. Em todas elas, a fonte selecionada foi o realizado. `SONDA1/roda.out:15–20` · `app/docs/LEIS.md:71,93,153` · `app/docs/BACKLOG.md:1793,1837,2006` · `app/docs/RELATO.md:7039` · `app/ponto/services/espelho.py:465–474,689–690,778–801`

## 3. Sonda única de leitura

O bloco é autocontido dentro de um Django já inicializado em sombra. Usa a janela visual do espelho para o juiz de turno declarado em `core/juizes.py`, e imprime o alvo e as datas vizinhas. Fixei `hoje` e `agora` nos valores do rodapé fornecido. A execução e a equivalência da foto estão **[nao verificado]**. O bloco não chama o wrapper que prepara credenciais e grava trilha. `SONDA1/roda.out:21` · `app/config/settings/sombra.py:49–60` · `app/core/juizes.py:215–216` · `app/ponto/services/espelho.py:625–649,1298–1302,1330–1332` · `bin/sonda_leitura.sh:126–142,235–250`

```python
import datetime as dt
import json
import logging
import sys
from contextlib import ExitStack

sys.dont_write_bytecode = True

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from ponto import turnos as juiz
from ponto.calculador.nucleo import minutos_do_oraculo
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.services.dia_pago import _dia_de
from ponto.services.espelho import espelho_do_colab

assert getattr(settings, "HASNER_SOMBRA", False) is True
assert not connection.in_atomic_block

ALVOS = (
    (114, "2026-11-15"), (130, "2026-10-29"), (220, "2026-11-10"),
    (874, "2026-10-22"), (989, "2026-10-23"), (996, "2026-11-10"),
)
HOJE = dt.date(2026, 11, 26)
AGORA = dt.datetime.fromisoformat("2026-11-26T11:01:55.389818-03:00")
UM_DIA = dt.timedelta(days=1)


def emitir(**dados):
    print(json.dumps(dados, ensure_ascii=True, default=str))


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def meia_noite(data):
    return timezone.make_aware(dt.datetime.combine(data, dt.time.min))


def min_entre(inicio, fim):
    return (fim - inicio).total_seconds() / 60


def medir(c, alvo):
    chave = f"col{c.pk} {alvo}"
    dias = (alvo - UM_DIA, alvo, alvo + UM_DIA)
    ini, fim = janela_fechamento(11, 2026, c.empresa)
    esp = espelho_do_colab(c, HOJE, ini, fim, agora=AGORA)
    vis_ini, vis_fim = esp["data_ini"], esp["data_fim"]
    linhas = {d["data"]: d for d in esp["dias"]}
    traco = {}

    # Observa os trechos e o papel efetivos, sem refazer o corte ou o pareamento.
    def observar(frame, evento, retorno):
        if evento != "return":
            return
        if frame.f_code is minutos_do_oraculo.__code__:
            traco["trechos"] = frame.f_locals["turnos"]
        if frame.f_code is juiz._turnos_do_colab_puro.__code__:
            traco["papel"] = frame.f_locals["_papel"]
            traco["batidas_juiz"] = frame.f_locals["bs"]

    with ExitStack() as restaurar:
        restaurar.callback(sys.setprofile, sys.getprofile())
        sys.setprofile(observar)
        ins = InsumosDoDono(c, ini, fim, HOJE, espelho=esp, agora=AGORA)
        turnos = juiz.turnos_do_colab(c, vis_ini, vis_fim)

    v = ins.oraculo
    apelidos = {}
    ecos = {b.pk for b in traco["batidas_juiz"] if getattr(b, "_eco_flush", False)}

    def batida(b):
        if b is None:
            return None
        return {
            "id": apelidos.setdefault(b.pk, f"b{len(apelidos) + 1}"),
            "instante_local": local(b.timestamp),
            "tipo": b.tipo,
            "papel_ata": juiz._papel_da_ata(b, traco["papel"]),
            "eco_flush": b.pk in ecos,
            "is_intervalo": b.is_intervalo,
            "intra_dur": bool(getattr(b, "_intra_dur", False)),
            "intra_declarado": bool(getattr(b, "_intra_declarado", False)),
        }

    emitir(chave=chave, competencia=(ini, fim), janela_visual=(vis_ini, vis_fim))
    brutas = juiz.batidas_apuraveis(
        c, meia_noite(alvo - 2 * UM_DIA), meia_noite(alvo + 3 * UM_DIA)
    )
    for b in brutas:
        emitir(chave=chave, batida_apuravel=batida(b))

    anterior = None
    for trecho in traco["trechos"]:
        if any(t.date() in dias for t in trecho):
            pares = list(zip(trecho[::2], trecho[1::2]))
            emitir(
                chave=chave, dia_e6=trecho[0].date(),
                instantes=[local(t) for t in trecho],
                pares=[(local(a), local(z), min_entre(a, z)) for a, z in pares],
                piso_trecho=sum(min_entre(a, z) for a, z in pares),
                teto_trecho=min_entre(trecho[0], trecho[-1]),
                fronteira_anterior=None if anterior is None else {
                    "n_pontas": len(anterior),
                    "gap_min": min_entre(anterior[-1], trecho[0]),
                    "vao_min": min_entre(anterior[0], trecho[0]),
                },
            )
        anterior = trecho

    celulas = celulas_do_periodo(
        [c.pk], min(vis_ini, dias[0]), max(vis_fim, dias[-1])
    )
    cels_vis = {k: cel for k, cel in celulas.items() if vis_ini <= k[1] <= vis_fim}
    geradoras = {cel.escala_geradora_id for cel in cels_vis.values() if cel.escala_geradora_id}
    escalas = sorted(
        escalas_do_periodo(c, vis_ini, vis_fim, mais=geradoras),
        key=lambda ec: ec.data_inicio,
    )

    for dia in dias:
        k = dia.isoformat()
        cel = celulas.get((c.pk, dia))
        de = linhas.get(dia)
        emit_cel = None if cel is None else {
            "trabalha": cel.trabalha, "veredito": cel.veredito,
            "marcos": (cel.dna or {}).get("marcos"), "dna": cel.dna,
        }
        ec = vinculo_do_dia(c.pk, dia, escalas, cels_vis) if de is not None else None
        te = ec.tipo_escala if ec is not None else None
        livre = bool(te and te._intervalo_livre())
        intervalo = ec.intervalo_do_dia(dia, celulas=cels_vis) if te else (None, None)
        do_dia = [t for t in turnos if t.data_turno == dia]
        real = juiz.realizado_dos_turnos(do_dia, dia, livre=livre, intervalo=intervalo)
        pos = juiz._borda_posicional(do_dia)
        selecionado = minutos_do_espelho(de)
        piso = v.minutos.get(k, 0.0)
        delta = None if selecionado[0] is None else piso - selecionado[0]
        emitir(
            chave=chave, dia=dia, celula=emit_cel,
            vinculo_id=ec.pk if ec is not None else None,
            livre=livre, intervalo_declarado=intervalo,
            realizado_juiz=real._asdict() if de is not None else None,
            borda_L115=None if pos is None else {
                "entrada": batida(pos[0].entrada), "saida": batida(pos[1]),
                "pausa": [batida(b) for b in pos[2]],
            },
            espelho=None if de is None else {
                **{campo: de.get(campo) for campo in (
                    "minutos_realizados", "pago_h", "realizado_janela_descontada",
                    "realizado_sem_turno", "realizado_aberto", "isencao", "veredito_dia",
                )},
                "ausencia_presente": de.get("ausencia") is not None,
                "cobertura_folha_presente": de.get("_cobertura_folha") is not None,
            },
            selecionado=selecionado, piso=piso, teto=v.envelope.get(k, 0.0),
            pausa_e6=v.pausas.get(k, 0.0), horas=v.horas.get(k, []),
            brutas=v.brutas.get(k, 0), impar=k in v.impares, corte=k in v.cortes,
            delta=delta, delta_arredondado=round(delta) if delta is not None else None,
            guarda_janela=(delta is not None and diferenca_e_a_janela_declarada(delta, de)),
        )

        for t in do_dia:
            isolado = None
            if de is not None and pos is None:
                isolado = juiz.realizado_dos_turnos(
                    [t], dia, livre=livre, intervalo=intervalo
                )._asdict()
            emitir(
                chave=chave, data_turno=t.data_turno, aberto=t.aberto,
                cruza_meia_noite=t.cross_meianoite,
                entrada=batida(t.entrada), saida=batida(t.saida),
                fim_contado=batida(juiz._fim_fechado(t)),
                batidas_turno=[batida(b) for b in t.batidas],
                pausas=[(local(a), local(z)) for a, z in juiz.intervalos_do_turno(t)],
                abaixo_do_piso=[
                    (local(a), local(z)) for a, z in juiz.pausas_abaixo_do_piso(t)
                ],
                realizado_turno_isolado=isolado,
            )

    resultado = esp["resultado"]
    mapa = getattr(resultado, "dia_da_jornada", None) or {}
    dia_cartao = {id(p): d for d, linha in linhas.items() for p in linha["turnos"]}
    for ft, periodos in ((False, resultado.periodos), (True, resultado.periodos_ft)):
        for p in periodos:
            d_cartao = dia_cartao.get(id(p))
            d_mapa = mapa.get(id(p))
            d_civil = timezone.localtime(p.entrada).date()
            if {d_cartao, d_mapa, d_civil}.intersection(dias):
                emitir(
                    chave=chave, periodo_motor={
                        "ft": ft, "dia_cartao": d_cartao, "dia_mapa": d_mapa,
                        "dia_para_lavra": _dia_de(p, mapa),
                        "entrada": local(p.entrada), "saida": local(p.saida),
                        "efetivo_entrada": local(p.efetivo_entrada),
                        "efetivo_saida": local(p.efetivo_saida),
                        "minutos_trabalhados": p.minutos_trabalhados,
                        "minutos_fora_janela": p.minutos_fora_janela,
                        "intra_descontada": p.intra_descontada,
                    },
                )


with schema_context("juliani"), transaction.atomic(), ExitStack() as restaurar:
    restaurar.callback(logging.disable, logging.root.manager.disable)
    logging.disable(logging.CRITICAL)
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY")
        cursor.execute(
            "SELECT current_database(), current_schema(), "
            "current_setting('transaction_isolation'), "
            "current_setting('transaction_read_only'), txid_current_snapshot()::text"
        )
        banco, schema, isolamento, leitura, snapshot = cursor.fetchone()
    assert (banco, schema, isolamento, leitura) == ("sombra", "juliani", "repeatable read", "on")
    emitir(banco=banco, schema=schema, snapshot=snapshot, hoje=HOJE, agora=local(AGORA))
    for pk, data in ALVOS:
        medir(Colaborador.objects.select_related("empresa").get(pk=pk), dt.date.fromisoformat(data))
```

## 4. Dia → mecanismo → obra → total dos 18

| Dia | Mecanismo demonstrado ou localizado | Obra dona | Quantos dos 18 a obra zera, somando o mapa |
|---|---|---|---|
| **col114 15/11** | **C**, diferença entre piso e vão interno com janela do espelho zero. Origem da valorização **[nao verificado]**. `SONDA1/roda.out:15` · `app/ponto/turnos.py:719–771` | **[nao verificado]** | **[nao verificado]** |
| **col130 29/10** | **B** na chave do E6 e **C** no desconto de 60. Origem do saldo **[nao verificado]**. `SONDA1/roda.out:16` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:760–771` | **[nao verificado]** | **[nao verificado]** |
| **col220 10/11** | **B** na chave do E6 e **C** no desconto de 60. Origem do saldo **[nao verificado]**. `SONDA1/roda.out:17` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:760–771` | **[nao verificado]** | **[nao verificado]** |
| **col874 22/10** | **B** na migração de uma batida civil no E6 e **C** no desconto de 60. Geometria que completa o saldo **[nao verificado]**. `SONDA1/roda.out:18` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:719–771` | **[nao verificado]** | **[nao verificado]** |
| **col989 23/10** | **A**, o par do E6 sozinho não fecha os 721 min. Participação de **D** **[nao verificado]**. `SONDA1/roda.out:19` · `app/ponto/turnos.py:719–771` · `app/ponto/services/espelho.py:465–474,778–801` | **[nao verificado]** | **[nao verificado]** |
| **col996 10/11** | **B** e corte por vão com dois trechos na chave do E6. `SONDA1/roda.out:20` · `app/ponto/calculador/nucleo.py:215–247` | `ORACULO-E6-43-DEFEITOS` registra o ramo do instrumento. Obra que resolve este delta **[nao verificado]**. `app/docs/GRADE-MESTRA.md:303` | O mapa relaciona **2 A**, **2 B** e **2 candidatas C** ao instrumento. Esta sonda acrescenta **1 ocorrência do ramo de corte**. Total que a obra zera **[nao verificado]**. `DOSSIE/r.md:27–34` · `SONDA1/roda.out:20` |

Nenhuma das seis chaves sustenta abrir **classe nova** com este dado. Para uma classe que a sonda venha a demonstrar, o padrão depende do produtor divergente. O GLOSSÁRIO dá **Single Source of Truth / um juiz por pergunta** para par, dia ou pausa julgados em fontes distintas, **CQRS com projeções reconstruíveis** quando a divergência está na projeção, e **Architecture Conformance Test** para selar o result

[truncado em 20000 de 20282 caracteres]

## 26/11 11:07 · sessao 35915bcc7391 · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

1. **bloqueia | Verif2-4 procedente. Cura parcial na origem.** A casca, o mapa por PK e `CargaDaJanela` passam a usar a carga histórica comum. Com os mesmos insumos, cada turno publicado concorda com o mapa em todas as suas PKs, e o mapa inclui a saída de 25/10 quando apresenta o turno de 23/10. A igualdade foi obtida também descartando turnos inteiros quando uma de suas PKs recebe outro dia. Isso muda a geometria entregue pelo juiz registrado, além de unificar a carga. `app/core/juizes.py:216`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:2941`, `app/ponto/turnos.py:3007`, `app/ponto/turnos.py:3119`

2. **bloqueia | L-085 na virada de vínculo e de competência.** Reproduzido em memória com o pareador real, sem ata/DNA. Vínculo 19h–07h até 23/10, vínculo 08h–17h a partir de 24/10, corte em 24/10 e uma única S em 24/10 às 07h. O segmento noturno põe a órfã em **23/10**, dentro da jornada da véspera. O diurno reivindica a mesma PK em **24/10**. O mapa conserva a última atribuição e a F3 elimina a órfã de 23/10 da lista publicada. Na criação dessa marca, o cartório recebe 23 e 24 por vizinhança, o núcleo não gera a linha de 23 e o evento agenda **somente 11/2026**. Também reproduzi a perda de um **par fechado** com vínculo 08h–17h até 23/10, 19h–07h com pausa 22h–23h desde 24/10, E em 23/10 às 22h e S em 24/10 às 21h. A F3 publica só a órfã de 24/10, embora a E continue no mapa em 23/10. `app/docs/LEIS.md:117`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1916`, `app/ponto/turnos.py:2949`, `app/ponto/turnos.py:3007`, `app/ponto/turnos.py:3025`, `app/ponto/signals.py:124`, `app/ponto/nucleo.py:48`, `app/ponto/services/fechamento.py:1832`, `app/ponto/janelas.py:24`

3. **bloqueia | Verif2-5a procedente. A âncora civil saiu do signal, mas o resultado ainda depende da publicação do item 2.** No caminho normal, o núcleo compara a versão anterior com a gravada, reaproveita os turnos atuais e guarda os dias para o cartório. No `post_delete`, o juiz reinsere a marca em memória para montar a versão anterior. A vizinhança do cartório passa a ser a véspera de cada dia afetado mais o próprio dia. Há uma regressão separada no `pre_save`: o novo `SELECT` da versão anterior fica fora das proteções dos receivers. Com falha injetada nessa consulta, a exceção escapou antes do `save` de `anotar`. `app/ponto/signals.py:20`, `app/ponto/signals.py:27`, `app/ponto/signals.py:30`, `app/ponto/signals.py:124`, `app/ponto/signals.py:153`, `app/ponto/signals.py:160`, `app/ponto/turnos.py:3025`, `app/ponto/registro_batida.py:276`

4. **bloqueia | Verif2-5b procedente. Criação e retratação migraram. Exclusão e falha parcial ficam abertas.** Na criação e na retratação, a porta passa os dias julgados ao fechamento, que usa o corte da empresa e deduplica as competências no callback pós-commit. Outro caso reproduzido em memória, com um único vínculo 19h–07h e corte em 24/10, é uma S órfã `offline_pendente` de 24/10 às 07h. Ela é apurável e pertence a **23/10, competência 10**. Rejeitá-la pela rota real dispara núcleo e cartório, mas `apagar` não agenda fechamento. A guarda da criação institucional e do flip ainda consulta **11/2026** pelo instante civil. Num segundo ensaio, com dias 23 e 24 no callback, injetei falha no recálculo de outubro. Novembro nem foi tentado, pois o `try` envolve o laço inteiro. `app/ponto/registro_batida.py:158`, `app/ponto/registro_batida.py:174`, `app/ponto/registro_batida.py:195`, `app/ponto/registro_batida.py:303`, `app/ponto/registro_batida.py:307`, `app/ponto/views.py:475`, `app/ponto/views.py:496`, `app/ponto/turnos.py:2863`, `app/ponto/services/flip_batida.py:21`, `app/ponto/janelas.py:84`, `app/ponto/services/fechamento.py:1832`

5. **bloqueia | Dois caminhos ainda atribuem o dia fora da carga da autoridade.** **C6** chama o mesmo corpo puro com outro recorte. Reproduzi com vínculo 19h–07h, E em 25/10 às 05h e S em 26/10 às 06h. Para `[24/10,24/10]`, o C6 vê só a E e apresenta turno aberto de 24/10. A carga histórica vê a S, fecha em 25/10 e deixa a janela do dia 24 sem aquele turno. Com corte 25, a divergência atravessa as competências 10 e 11. **Motor mensal** também mantém carga e atribuição próprias. Lê só um dia de margem, pareia com `turnos_de_batidas`, indexa a entrada pelo instante e usa a data civil quando falta a chave. Com corte 24, a carga da competência 10 sequer vê a S de 25/10 do golden de 24h10. **`fechamento.py:1241`** repassa o mapa do motor à lavratura. O segundo derivador está a montante dessa linha. Efeito final na lavra [nao verificado]. `app/chamados/services/carga_pergunta_viva.py:165`, `app/chamados/services/carga_pergunta_viva.py:173`, `app/chamados/juizes.py:447`, `app/escala/servico_jornada.py:403`, `app/ponto/services/fechamento.py:641`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:2961`, `app/ponto/services/fechamento.py:1241`, `app/ponto/services/dia_pago.py:173`

6. **bloqueia | O insumo explícito `celulas=` ainda permite respostas diferentes.** A casca singular recebe atas frescas da montagem da grade. Mapa, lote e evento usam células gravadas. Além disso, a F3 sobrepõe apenas as chaves fornecidas, preservando a célula do banco quando a chave falta, inclusive dentro da janela fornecida. O contrato existente dá outro sentido à chave ausente: a célula não existe naquele dia. Isso deixa dois conjuntos de insumos para a pergunta que deveria ter resposta única. `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:2909`, `app/ponto/turnos.py:2987`, `app/ponto/turnos.py:3025`, `app/ponto/turnos.py:3119`, `app/escala/utils.py:1934`, `app/escala/utils.py:1952`, `app/escala/alimentacao.py:14`, `app/escala/alimentacao.py:240`

7. **bloqueia | REDs, mutantes e catraca.** Os dez arquivos locais conferem com os blobs-base do anexo. O SHA do commit [nao verificado]. O `test_juiz_unico_eventos.py` citado no relato está ausente da árvore e do diff entregue. Os cinco REDs, oito GREENs e três mortes por asserção relatados [nao verificado]. Em memória, executei o censo real com as funções relevantes da F3 reconstruídas. **9 → 9 pareadores, 81 → 78 derivações e B1 78 → 75.** As asserções reais de número e composição falharam contra a catraca protegida. Saem `signals.py:23` e `:111`. A carga `turnos.py:3026 → :2979` entra na composição e sai de B1. `dia_do_turno_da_batida` continua em B1, `:2989 → :2929`, assim como `par_completo`, `:2217 → :2156`. A composição ganha `_carregar_dias_das_batidas`, `_dia_das_batidas_puro`, `batidas_apuraveis` e `marcos_dna_periodo`, e perde `escalas_do_periodo`. O R6 ainda declara **18/20**, com a célula de juiz único vermelha. `app/core/tests/test_medida_turno_do_par.py:108`, `app/core/tests/test_medida_turno_do_par.py:338`, `app/core/tests/test_medida_turno_do_par.py:508`, `app/core/tests/test_medida_turno_do_par.py:546`, `app/core/tests/test_medida_turno_do_par.py:562`, `app/core/placar_estrutural.py:443`, `app/core/contratos_estruturais.py:225`

8. **bloqueia | O selo perdeu a mordida no carregador que o C6 ainda usa.** Isolei a troca em memória e rodei o verificador AST real. Ao substituir `batidas_apuraveis(None)` por `Batida.objects`, ele acusa **1** ocorrência no histórico e **0** no recorte do C6. Na base acusava **1** no recorte. Os dois mutantes reancorados cobrem o histórico. O C8b também passou de duas chamadas à entrada pública `turnos` para duas chamadas diretas ao carregador recortado, deixando sem aquela asserção o cache da entrada pública. `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:132`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:195`, `app/chamados/tests/test_e3_c8c_sem_banco.py:104`, `app/chamados/tests/test_e3_c8b_sem_banco.py:522`, `app/chamados/services/carga_pergunta_viva.py:173`, `app/ponto/turnos.py:3119`

9. **nota | Escritores, leitores e ordem de trava.**

   - **`Turno.data_turno`.** `_mk` publica a resposta de `_data_do_turno`. O mapa a copia e `recompute_turnos` a materializa. C6 e motor também chegam ao pareador pelos caminhos do item 5. `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`, `app/ponto/turnos.py:2949`, `app/ponto/nucleo.py:48`, `app/chamados/services/carga_pergunta_viva.py:182`, `app/ponto/motor_calculo_v2.py:882`
   - **`TurnoMaterializado.data_turno`.** `recompute_turnos` é o escritor direto localizado. Faz `DELETE` do recorte e `bulk_create` em `atomic`. Os gravadores de batida continuam em `registro_batida.py`; o diff ali altera o agendamento. A exclusão indireta por FK é herdada. Leem a materialização a seleção de turno atual e o painel, o reconciliador, o juiz da via do chamado, o flip, os alertas e os comandos de auditoria. `app/core/portas.py:103`, `app/core/portas.py:118`, `app/ponto/registro_batida.py:170`, `app/ponto/registro_batida.py:276`, `app/ponto/nucleo.py:54`, `app/ponto/nucleo.py:61`, `app/ponto/nucleo.py:84`, `app/ponto/models.py:1214`, `app/ponto/selecao_periodo.py:270`, `app/core/services/painel_op.py:222`, `app/chamados/reconciliador.py:97`, `app/chamados/reconciliador.py:174`, `app/chamados/juizes.py:1939`, `app/ponto/services/flip_batida.py:37`, `app/ponto/management/commands/processar_alertas_turno.py:76`, `app/ponto/management/commands/processar_alertas_turno.py:172`, `app/ponto/management/commands/processar_alertas_avancados.py:58`, `app/ponto/management/commands/verificar_nucleo.py:24`, `app/chamados/management/commands/auditar_invariantes_chamados.py:229`, `app/chamados/management/commands/auditar_invariantes_chamados.py:342`, `app/chamados/management/commands/auditar_invariantes_chamados.py:610`, `app/chamados/management/commands/auditar_invariantes_chamados.py:990`
   - **Mapa `{pk: dia}` e competência.** `_dia_das_batidas_puro` publica o mapa. Leem o adaptador unitário, a API, o espelho e seu PDF, o painel, a adesão, férias, o vigia e a comparação antes/depois do evento. `julgamento_do_salvamento` grava os dias no objeto em memória. Cartório e porta de batida os leem, e o callback do fechamento os converte em pares mês/ano e chama o recálculo. HE, veredito de célula e validação de chamados seguem passando a data de seus fatos. `app/ponto/turnos.py:2929`, `app/ponto/turnos.py:2941`, `app/ponto/turnos.py:3025`, `app/ponto/turnos.py:3050`, `app/api/views.py:1444`, `app/api/views.py:2042`, `app/ponto/services/espelho.py:465`, `app/relatorios/pdf_espelho.py:596`, `app/core/services/painel_op.py:330`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/ponto/signals.py:124`, `app/ponto/registro_batida.py:198`, `app/ponto/services/fechamento.py:1832`, `app/ponto/portas/he.py:230`, `app/ponto/portas/celula.py:503`, `app/chamados/services/validacao.py:628`, `app/chamados/services/validacao.py:798`
   - **Ordem.** Núcleo → cartório → reconciliador, com a ordem de trava Colaborador → Disputa → Chamado → Pergunta. O novo `SELECT` é simples. O núcleo monta as linhas antes do `atomic` de substituição, sem serializar a fotografia por colaborador. Resultado de saves/retratações concorrentes [nao verificado]. `app/ponto/signals.py:20`, `app/ponto/signals.py:50`, `app/ponto/signals.py:153`, `app/ponto/signals.py:228`, `app/ponto/nucleo.py:45`, `app/ponto/nucleo.py:61`, `app/chamados/models.py:2939`, `app/chamados/models.py:2948`, `app/chamados/models.py:2961`, `app/chamados/models.py:2965`

10. **nota | Os 51 sítios do censo.** Confirmei **50 chamadas em produção**, incluindo aliases, mais o registro `_T1`. `_TT2` é outro ponteiro no registro para a mesma função. As chamadas abaixo recebem a nova resposta da autoridade. Mudança efetiva de cada um dos 50 consumidores na frota, PKs e linhas materializadas nas competências 10 e 11 [nao verificado]. A reprodução do item 2 demonstra mudança no valor retornado e na entrada da materialização. `app/core/juizes.py:216`, `app/core/juizes.py:748`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:3007`, `app/ponto/nucleo.py:48`

   - **API e chamados.** `app/api/views.py:1447`, `app/chamados/detector_anomalias.py:157`, `app/chamados/juizes.py:447`, `app/chamados/juizes.py:2303`, `app/chamados/models.py:1378`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/materializacao.py:399`, `app/chamados/services/materializacao.py:513`
   - **Colaboradores e escala.** `app/colaboradores/services/drawer.py:47`, `app/colaboradores/services/feriado_do_posto.py:473`, `app/escala/detector_proposta.py:65`, `app/escala/regua_defesa.py:361`, `app/escala/services/escala_auto.py:87`, `app/escala/services/impacto_troca.py:18`, `app/escala/servico_jornada.py:249`, `app/escala/servico_jornada.py:403`, `app/escala/utils.py:1627`, `app/escala/utils.py:1952`
   - **Ponto.** `app/ponto/nucleo.py:48`, `app/ponto/turno_leitura.py:89`, `app/ponto/turno_leitura.py:95`, `app/ponto/turnos.py:780`, `app/ponto/turnos.py:2043`, `app/ponto/turnos.py:2101`, `app/ponto/turnos.py:2131`, `app/ponto/turnos.py:3191`, `app/ponto/views.py:396`, `app/ponto/views.py:1613`, `app/ponto/views.py:3356`, `app/ponto/services/bordas_realizado.py:66`, `app/ponto/services/esmeril_espelho.py:102`, `app/ponto/services/espelho.py:628`, `app/ponto/services/passivo_s84.py:82`, `app/ponto/services/regularizacao.py:61`, `app/ponto/services/teto_na_lista.py:68`, `app/ponto/services/triagem_batida.py:553`, `app/ponto/services/vigia_pareador_celula.py:97`
   - **Comandos e relatórios.** `app/ponto/management/commands/censo_fase_12x36.py:180`, `app/ponto/management/commands/detectar_ausencias.py:337`, `app/ponto/management/commands/detectar_par_relampago.py:101`, `app/ponto/management/commands/dna_x_batida_real.py:69`, `app/ponto/management/commands/e6_oraculo.py:159`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/marcar_abandono_provisorio.py:46`, `app/ponto/management/commands/verificar_nucleo.py:29`, `app/ponto/management/commands/vigia_plantio_overnight.py:38`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`, `app/relatorios/pdf_espelho.py:213`, `app/relatorios/pdf_espelho.py:451`

11. **nota | Tipos, bordas, relógio e `except`.** `Batida.timestamp` é instante. `Turno.data_turno` e a coluna materializada são datas de jornada. O mapa conserva a exceção herdada: uma PK sem turno reclamante recebe dia civil. `contexto_json['data_turno']` da batida fora de escala e `Justificativa.data_turno` continuam carregando dia civil nos fluxos citados pelo verif2. A competência e o pad de apresentação incluem as duas datas-limite. O segmento pareia por intervalo de instantes semiaberto. O diff acrescenta **zero `except`** e nenhuma leitura direta de relógio no novo carregador. Permanecem o `date.today()` no cadastro do intervalo e as leituras de relógio dentro do cartório. Efeito de `date.today()` sobre `Turno.data_turno` [nao verificado]. `app/ponto/models.py:23`, `app/ponto/models.py:1215`, `app/ponto/turnos.py:1919`, `app/ponto/turnos.py:2941`, `app/ponto/turnos.py:3007`, `app/ponto/janelas.py:10`, `app/api/views.py:717`, `app/api/views_core.py:711`, `app/ponto/services/regularizacao.py:73`, `app/ponto/services/fechamento.py:680`, `app/ponto/signals.py:30`, `app/ponto/registro_batida.py:195`, `app/ponto/services/fechamento.py:1832`, `app/ponto/juiz_batida.py:129`, `app/ponto/juiz_batida.py:274`, `app/ponto/services/cartorio.py:2084`, `app/ponto/services/cartorio.py:2105`

12. **fila | Verif2-7 e 9 seguem procedentes.** Adesão reativa compara o dia do mapa aos limites inclusivos do pai. O cron aceita qualquer batida apurável do colaborador. Quanto ao custo, a leitura estática dá **4–5 consultas** na casca histórica com marcas no pad, **1** com pad vazio e **3–4 adicionais** no lote com a ata já carregada. O caminho normal do `save` compartilha uma carga histórica entre os três consumidores do evento, pode parear duas versões e acrescenta um `SELECT` para capturar a versão anterior quando aplicável. O cartório e a adesão ainda podem abrir cargas próprias. SQL efetivo, teto do chokepoint, latência e impacto em produção [nao verificado]. `app/colaboradores/services/pos_vinculo.py:37`, `app/chamados/services/adesao.py:143`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/ponto/turnos.py:2953`, `app/ponto/turnos.py:2987`, `app/ponto/turnos.py:3119`, `app/escala/alimentacao.py:148`, `app/ponto/signals.py:20`, `app/ponto/signals.py:124`, `app/ponto/services/cartorio.py:1149`, `app/ponto/tests/test_selo_chokepoint_escrita.py:343`

**Veredito final — REPROVA.** A F3 perde geometria na troca de vínculo, mantém respostas paralelas em C6 e no motor mensal, deixa uma exclusão apurável sem evento de fechamento e falha na catraca congelada. `app/ponto/turnos.py:3007`, `app/chamados/services/carga_pergunta_viva.py:165`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/registro_batida.py:307`, `app/core/tests/test_medida_turno_do_par.py:508`

## 26/11 11:06 · sessao f9d3e5e1594c · - · - · origem=Ronald (tmux)

1. **Bloqueia | A sobreposição de segmentos ainda desloca a órfã da véspera.** Reproduzi em memória com o pareador e `vinculo_do_dia` reais. Vínculo noturno 19h–07h até 23/10, diurno 08h–17h desde 24/10, corte 24, sem DNA nem ata, e uma única S em 24/10 às 07h. O segmento noturno atribui a S a 23/10. O diurno atribui a mesma PK a 24/10, sobrescreve o mapa e o filtro F3 descarta o turno de 23/10. Na criação, o cartório visita 23 e 24 pela regra de vizinhança, o núcleo recebe só o turno de 24 e o evento agenda 11/2026. Produção [nao verificado]. `app/ponto/turnos.py:285–298`, `app/ponto/turnos.py:1253–1302`, `app/ponto/turnos.py:1977–2002`, `app/ponto/turnos.py:3001–3009` na base e hunk F3 `+2950`, `app/ponto/signals.py:124`, `app/ponto/services/fechamento.py:1831`, `app/ponto/janelas.py:10–33`.

2. **Bloqueia | C6 conserva outro caminho para o mesmo veredito.** O lote chama o corpo puro com batidas do próprio recorte. O avulso chama `turnos_do_colab`. Reproduzi com 19h–07h, E em 25/10 às 05h e S em 26/10 às 06h, corte 25. O histórico fecha o par em 25/10, competência 11. O recorte C6 `[24/10,24/10]` enxerga só a entrada e devolve aberto em 24/10, competência 10. A diferença chega a `_turno_tem_saida` e `classificar_falta`. `app/chamados/services/carga_pergunta_viva.py:165–184`, `app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/juizes.py:447–449`, `app/escala/servico_jornada.py:249–259`, `app/escala/servico_jornada.py:403–415`, `app/ponto/turnos.py:285–309`, `app/ponto/turnos.py:1492–1498`.

3. **Bloqueia | A lavra ainda recebe o dia derivado no motor.** O motor usa a carga de batidas limitada a ±1 dia da competência, deduplica e monta mapa por instante de entrada e tipo gravado. Na falta da chave usa o civil. Para o par de 24h10 com E em 23/10 e S em 25/10, corte 24, o evento pode agendar outubro enquanto o cálculo de outubro sequer carrega a S. `fechamento.py:1241` e `:1311` repassam a resposta do motor à lavra. Resultado final do gravado [nao verificado]. `app/ponto/services/fechamento.py:641–653`, `app/ponto/motor_calculo_v2.py:867–894`, `app/ponto/motor_calculo_v2.py:2961`, `app/ponto/motor_calculo_v2.py:3346`, `app/ponto/services/fechamento.py:1241`, `app/ponto/services/fechamento.py:1311`, `app/ponto/services/dia_pago.py:126–132`, `app/ponto/services/dia_pago.py:173–177`.

4. **Nota | Escritores diretos e censo.** O censo AST da base localizou escrita de `Batida` apenas em `app/ponto/registro_batida.py:170,176,274,311,355`. O núcleo cria `TurnoMaterializado` e apaga o recorte via SQL na porta, com exclusão indireta herdada pelas FKs `CASCADE`. A varredura conferiu 50 chamadas diretas ou por alias, mais `_T1`. Calendário, status e painel também são consumidores via lote. Variação efetiva dos consumidores em produção [nao verificado]. `app/ponto/nucleo.py:48`, `app/ponto/nucleo.py:59`, `app/ponto/nucleo.py:80`, `app/ponto/models.py:1214`, `app/ponto/models.py:1217`, `app/ponto/models.py:1219`, `app/core/portas.py:118`, `app/core/juizes.py:216`, `app/colaboradores/services/calendario.py:252`, `app/ponto/status_dia.py:273`, `app/core/services/painel_op.py:327–333`.

5. **Fila | A guarda do chokepoint interpreta a competência pelo timestamp civil.** Para uma S órfã de 24/10 às 07h num vínculo noturno estável 19h–07h, corte 24, o pareador devolve 23/10 e o novo evento agenda 10/2026. A guarda das gravações institucionais e do flip consulta 11/2026 pelo civil. É um caminho herdado fora da autorização de alteração do chokepoint nesta fatia. `app/ponto/turnos.py:285–298`, `app/ponto/janelas.py:75–86`, `app/ponto/registro_batida.py:147–162`, `app/ponto/services/flip_batida.py:21–24`, `app/chamados/services/materializacao.py:857–859`.

## 26/11 11:03 · sessao f3d1e6c7b2e6 · O327-C11-93 · leitura · origem=Code

Na competência **11/2026**, o anexo registra **10 cortes** nos sete PKs. Pelo dia do **mapa interno do motor**, **3** estão dentro da competência e **7** estão fora. Os três de dentro são do `col900` e têm `par_fechado_positivo=false`. A atribuição desses cortes pela chamada direta a `turnos_do_colab` e a perda de minutos trabalhados estão [nao verificado]. `cortados.jsonl:1–7`, `DONO-2611/r.md` (sonda, `ler_motor`), `app/ponto/motor_calculo_v2.py:2362–2365`.

### Quem decide o dia e por que corta

1. **O juiz decide `data_turno`.** `core/juizes.py` aponta para `ponto.turnos.turnos_do_colab`. Essa função lê batidas, células e vínculos, pareia por segmentos de vínculo e devolve os turnos pelo `data_turno`. Na regra da madrugada, `_data_do_turno` pode atribuir o turno à véspera se os marcos indicarem cruzamento de meia-noite e, havendo saída, ela couber no envelope da jornada da véspera. `app/core/juizes.py:214–216`, `app/ponto/turnos.py:1850–1920,1961–2002,216–231,285–324`.
2. **O `MotorBase` decide o `data_jornada` usado no corte.** Ele monta seu mapa com `turnos_de_batidas`, usando as batidas daquela fatia, e indexa `data_turno` pelo instante localizado das `E`. O laço começa com esse dia ou, sem chave, com a data civil da entrada. Se houve saída no período anterior e o intervalo até a entrada atual está entre **0 e 6 horas, inclusive**, `_prev_dj` prevalece. O estado do período anterior é atualizado antes da guarda, inclusive quando esse período é cortado. `app/ponto/motor_calculo_v2.py:867–895,2362–2373,2397–2398,2415–2448`.
3. **A guarda aplica os limites da fatia.** O fechamento recorta a fatia pela competência, vigência, início da operação e dias atribuídos pelo vínculo. A entrada do motor aceita batidas da véspera e do dia seguinte. Depois de montar os períodos, o motor executa `continue` se `data_jornada < data_ini` ou `data_jornada > data_fim`, antes de calcular e registrar o período em `dia_da_jornada`. Nos dez cortes do anexo, `dia_usado` coincide com `dia_juiz` do mapa interno. A herança como causa dos cortes observados está [nao verificado]. `app/ponto/services/fechamento.py:204–230,244–311`, `app/ponto/motor_calculo_v2.py:2445–2448,2473–2476,1090–1092`, `cortados.jsonl:1–7`.

### Dono pela L-085 e pela L-099

| Classe solicitada | Dono e leitura do anexo |
|---|---|
| **Dia do juiz dentro da competência e período cortado** | **ESTRUTURA**, no critério do corte. Pelo mapa interno, são **3 ocorrências**, em `28/10`, `15/11` e `16/11`, todas do `col900` e sem par fechado positivo. O `BATIDA` de `28/10` é o dono da divergência diária registrada pelo E6. `cortados.jsonl:6`, `app/docs/LEIS.md:117,122`, `app/ponto/management/commands/e6_oraculo.py:98–110,182–195`. |
| **Dia do juiz fora da competência** | **Corte legítimo, sem dono de divergência na 11.** A jornada pertence à competência que contém seu dia de início. Pelo mapa interno, são **7 ocorrências**, seis em `20/10` e uma em `21/11`. `cortados.jsonl:1–7`, `app/docs/LEIS.md:117,122`, `app/ponto/janelas.py:54–57`. |

A sonda abaixo observa os dois `continue` no fechamento em modo de leitura. Ela consulta `turnos_do_colab` com dias de borda e procura o instante exato da entrada entre as batidas dos turnos devolvidos. Quando o juiz devolve zero ou mais de um dia para o instante, a linha e o total registram `[nao verificado]`. A execução da sonda e a confirmação independente do hash estão [nao verificado]. `app/ponto/services/fechamento.py:468–475,1152–1160,1215–1225`, `app/ponto/motor_calculo_v2.py:2445–2448`, `app/ponto/turnos.py:1834–1852,1997–2002`.

```python
import sys

_pyc_anterior = sys.dont_write_bytecode
sys.dont_write_bytecode = True
try:
    import json
    import logging
    from collections import Counter, defaultdict
    from contextlib import ExitStack

    from django.conf import settings
    from django.db import connection, transaction
    from django.utils import timezone

    from colaboradores.models import Colaborador
    from ponto.janelas import janela_fechamento
    from ponto.motor_calculo_v2 import MotorBase, localizar
    from ponto.services import fechamento
    from ponto.turnos import janela_com_pad, turnos_do_colab

    def sonda():
        mes, ano = 11, 2026
        pks = (90, 274, 704, 755, 761, 900, 984)  # cortados.jsonl:1–7
        desconhecido = "[nao verificado]"

        # app/config/settings/sombra.py:13–14,47–51,60
        if (
            getattr(settings, "HASNER_SOMBRA", False) is not True
            or connection.settings_dict["NAME"] != "sombra"
            or connection.settings_dict["HOST"] != "sombra_db"
            or getattr(connection, "schema_name", None) != "juliani"
        ):
            raise RuntimeError("Abra o shell da sombra no schema juliani.")

        codigo_fechamento = fechamento.recalcular_fechamento_mes.__code__
        codigo_fatia = fechamento.apurar_por_fatia.__code__
        codigo_motor = MotorBase.calcular_mes.__code__
        cortes = defaultdict(list)

        def ler_motor(frame, evento, argumento):
            # app/ponto/motor_calculo_v2.py:2415–2448
            if evento == "line" and frame.f_lineno in (2446, 2448):
                local = frame.f_locals
                vinculo = local["escala_colaborador"]
                entrada = localizar(local["entrada"])
                cortes[local["colaborador_id"]].append({
                    "entrada": entrada,
                    "saida": localizar(local["saida"]),
                    "data_turno_mapa_do_motor": local["_dia_turno_juiz"].get(entrada),
                    "dia_da_jornada_motor": local["data_jornada"],
                    "fatia": {
                        "inicio": local["data_ini"],
                        "fim": local["data_fim"],
                        "vinculo_id": vinculo.pk,
                        "vinculo_inicio": vinculo.data_inicio,
                        "vinculo_fim": vinculo.data_fim,
                    },
                    "linha_do_corte": frame.f_lineno,
                })
            return ler_motor

        def rastrear(frame, evento, argumento):
            if evento != "call" or frame.f_code is not codigo_motor:
                return None

            # app/ponto/services/fechamento.py:306–311,774–775,847
            # As subclasses podem chamar MotorBase por super().
            pai = frame.f_back
            while pai is not None and pai.f_code is not codigo_fechamento:
                if pai.f_code is codigo_fatia:
                    if pai.f_back is not None and pai.f_back.f_code is codigo_fechamento:
                        return ler_motor
                    return None
                pai = pai.f_back
            return None

        saidas = []
        totais = Counter(
            perda_estrutural_l085=0,
            corte_legitimo=0,
            dia_do_juiz_nao_verificado=0,
        )
        pares_fechados_positivos_dentro = 0

        with ExitStack() as sessao:
            sessao.callback(logging.disable, logging.root.manager.disable)
            logging.disable(sys.maxsize)
            sessao.enter_context(transaction.atomic(durable=True))

            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cursor.execute("SELECT current_database(), current_schema()")
                if cursor.fetchone() != ("sombra", "juliani"):
                    raise RuntimeError("Banco ou schema fora da sombra esperada.")

            colabs = Colaborador.objects.select_related("empresa").in_bulk(pks)
            faltantes = sorted(set(pks).difference(colabs))
            if faltantes:
                raise RuntimeError(f"PKs ausentes na sombra: {faltantes}")

            # app/ponto/services/fechamento.py:559–560,1152–1160,1215–1225
            with ExitStack() as observacao:
                observacao.callback(sys.settrace, sys.gettrace())
                sys.settrace(rastrear)
                leituras = fechamento.recalcular_fechamento_mes(
                    mes, ano, colaborador_ids=pks,
                    somente_leitura=True, lavrar_dias_pagos=False,
                )

            faltantes = sorted(set(pks).difference(leituras))
            if faltantes:
                raise RuntimeError(f"Fechamento de leitura sem retorno para: {faltantes}")

            for pk in pks:
                colab = colabs[pk]
                inicio, fim = janela_fechamento(mes, ano, colab.empresa)
                cortes_pk = cortes.get(pk, ())
                dias_por_instante = defaultdict(set)

                if cortes_pk:
                    # app/ponto/turnos.py:1834–1852,1997–2002
                    juiz_inicio, juiz_fim = janela_com_pad(inicio, fim)
                    for turno in turnos_do_colab(colab, juiz_inicio, juiz_fim):
                        if turno.data_turno is not None:
                            for batida in turno.batidas:
                                instante = localizar(batida.timestamp)
                                dias_por_instante[instante].add(turno.data_turno)

                for corte in cortes_pk:
                    entrada, saida = corte["entrada"], corte["saida"]
                    dias = sorted(dias_por_instante.get(entrada, ()))
                    dia_juiz = dias[0] if len(dias) == 1 else None
                    dentro = inicio <= dia_juiz <= fim if dia_juiz is not None else None
                    par_fechado_positivo = saida is not None and saida > entrada

                    # app/docs/LEIS.md:117,122
                    if dentro is None:
                        classe = "dia_do_juiz_nao_verificado"
                        dono = desconhecido
                    elif dentro:
                        classe = "perda_estrutural_l085"
                        dono = "ESTRUTURA"
                        pares_fechados_positivos_dentro += int(par_fechado_positivo)
                    else:
                        classe = "corte_legitimo"
                        dono = None
                    totais[classe] += 1

                    saidas.append({
                        **corte,
                        "pk": pk,
                        "entrada": timezone.localtime(entrada).isoformat(),
                        "saida": timezone.localtime(saida).isoformat() if saida is not None else None,
                        "data_turno_juiz": dia_juiz if dia_juiz is not None else desconhecido,
                        "datas_turno_candidatas": dias,
                        "janela_fechamento": [inicio, fim],
                        "dia_juiz_dentro_da_competencia": dentro,
                        "par_fechado_positivo": par_fechado_positivo,
                        "classe_do_corte": classe,
                        "dono_do_corte_l099": dono,
                    })

                if not cortes_pk:
                    saidas.append({
                        "pk": pk,
                        "janela_fechamento": [inicio, fim],
                        "n_periodos_cortados": 0,
                    })

        for linha in saidas:
            print(json.dumps(linha, default=str, sort_keys=True))
        print(json.dumps({
            "competencia": "2026-11",
            "total_pks": len(pks),
            "total_periodos_cortados": sum(totais.values()),
            "por_classe": dict(totais),
            "pares_fechados_positivos_dentro": pares_fechados_positivos_dentro,
        }, sort_keys=True))

    sonda()
finally:
    sys.dont_write_bytecode = _pyc_anterior
```

## 26/11 11:02 · sessao c8a671674d30 · O30-F3-CONSTRUIR · construtor · origem=Code

A cura c5 está na cópia sobre `8d48f1f35393dcd86345b5c866bfd4978d668181`. `rederivar_fim` calcula o fim uma vez, confere o intervalo e entrega a mesma data ao `Ausencia.save`. O modelo deriva minutos, dias corridos e carimbo. O teste executa o `save` do modelo com persistência em memória e confirma **1.440 minutos** pela R8 (`0 + 480 + 480 + 480`). [ausencia.py:1915](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1915) · [models.py:892](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/models.py:892) · [test_atestado_f3_contrato.py:735](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:735)

- **L-111.** Removi `_fixar_raiz_documento`, seus dois chamadores e `d1_fixar_aceita_filha`. A lápide ficou junto à trava que resolve a raiz. [ausencia.py:296](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:296) · [mutantes_atestado_f3.py:198](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:198)
- **Painel.** A view chama `raiz_documento` uma vez. O template usa a raiz recebida no contexto. Removi a tag e sua biblioteca. [views.py:2095](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2095) · [painel_ausencia.html:71](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:71)
- **Verificação local.** Passaram 46 testes de contrato sem banco e 11 testes do selo de escritores com `unittest.TestCase` no lugar de `SimpleTestCase`. Alinhei também o teste de integração de três dias à R8. Integração com banco, renderização real do painel e produção [nao verificado]. A certificação cabe ao Code e ao Opus. [test_atestado_f3_contrato.py:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) · [test_lote7_selo_escritor_acorda.py:296](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:296) · [test_lote7_fato_de_ausencia_acorda_projecao.py:618](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:618) · [LEIS.md:147](/home/ronald/wt-codex-o30-f3-construir-010143/app/docs/LEIS.md:147) [nao verificado]

### Diff, RED e mutantes

O diff passou em `git diff --check`. Cada `FAIL` abaixo saiu com `rc=1` por asserção e zero `ERROR`. O RED histórico `red_c2_c3` deu `ERROR` de importação também ao executar o runner da base c4. [mutantes_atestado_f3.py:66](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66) · [mutantes_atestado_f3.py:216](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:216) · [atestados_lote.py:19](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:19) [nao verificado]

```bash
git diff 8d48f1f35393dcd86345b5c866bfd4978d668181 -- .

export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest -q ponto.tests.test_atestado_f3_contrato
python3 -m ponto.tests.mutantes_atestado_f3 red_c4                       # 5 FAIL
python3 -m ponto.tests.mutantes_atestado_f3 d5_ignora_fim_cercado         # FAIL: 28/09 no lugar de 26/09
python3 -m ponto.tests.mutantes_atestado_f3 c5_minutos_usam_fim_anterior  # FAIL: 0 != 1440
python3 -m ponto.tests.mutantes_atestado_f3 c5_rederivar_save_base       # FAIL: selo AST
python3 -m ponto.tests.mutantes_atestado_f3 c5_rederivar_timezone_now    # FAIL: selo AST
python3 -m ponto.tests.mutantes_atestado_f3 c5_regrava_referencia         # FAIL: selo da escrita da referência
python3 -m ponto.tests.mutantes_atestado_f3 c5_painel_recalcula_raiz      # FAIL: selo do template
python3 -m ponto.tests.mutantes_atestado_f3 save_integral                 # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 d4_append                    # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 d4_atribuicao_em_ramo         # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 red_c3_d1                    # 2 FAIL
python3 -m ponto.tests.mutantes_atestado_f3 red_c3_d5                    # 2 FAIL
```

## 26/11 11:00 · sessao 48cb3acf6f01 · R1-SEM-DONO-2611 · leitura · origem=Code

```python
import json
from collections import Counter
from datetime import date

from django.apps import apps
from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    batida_fora_de_turno,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.services.espelho import espelho_do_colab
from ponto.supra_juiz import cadastro_do_dia_carregado


NV = "[nao verificado]"
MES, ANO = 11, 2026

# PUBLICADO.md, cabeçalho e §1, identifica a foto de referência como sombra de 26/11. [nao verificado]
# A data da referência é usada como hoje. A hora original e a identidade da
# foto conectada ficam [nao verificado] até serem informadas pela sessão.
# app/ponto/management/commands/e6_oraculo.py:284-297
HOJE = date(2026, 11, 26)
AGORA_DA_RODADA = None  # datetime com fuso, se a sessão tiver o instante original
FOTO_LIDA = NV         # identificador do dump, se a sessão o tiver

# PUBLICADO.md §3 traz as classes das 18 chaves. Os deltas inteiros dessas [nao verificado]
# chaves estão [nao verificado]. As referências abaixo são as citadas no anexo.
# Para col200 e col382, PUBLICADO.md §2 traz espelho × piso–teto. O comando [nao verificado]
# arredonda o delta separadamente dos operandos.
# app/ponto/management/commands/e6_oraculo.py:362-363,401-404
DELTA_PUBLICADO = {
    (112, "2026-10-21"): NV,  # depois_c11.txt:14
    (112, "2026-10-22"): NV,  # depois_c11.txt:15
    (200, "2026-11-03"): NV,  # depois_c11.txt:23
    (382, "2026-11-11"): NV,  # depois_c11.txt:36
    (114, "2026-11-08"): NV,  # depois_c11.txt:18-19
    (252, "2026-11-18"): NV,  # depois_c11.txt:26-32
    (252, "2026-11-19"): NV,  # depois_c11.txt:26-32
    (884, "2026-11-08"): NV,  # depois_c11.txt:41
    (922, "2026-11-01"): NV,  # depois_c11.txt:43-46
    (922, "2026-11-04"): NV,  # depois_c11.txt:43-46
    (302, "2026-11-20"): NV,  # depois_c11.txt:34
    (996, "2026-10-28"): NV,  # depois_c11.txt:47
    (114, "2026-11-15"): NV,  # depois_c11.txt:18-19
    (130, "2026-10-29"): NV,  # depois_c11.txt:20
    (220, "2026-11-10"): NV,  # depois_c11.txt:25
    (874, "2026-10-22"): NV,  # depois_c11.txt:40
    (989, "2026-10-23"): NV,  # depois_c11.txt:43-46
    (996, "2026-11-10"): NV,  # depois_c11.txt:48-49
}
DONO_PUBLICADO = "ESTRUTURA"  # PUBLICADO.md §1, §3 e r.md, censo das 18 chaves [nao verificado]


def sonda():
    assert len(DELTA_PUBLICADO) == 18

    # app/config/settings/sombra.py:13-14,29,49-60
    # bin/sombra_mascara.py:32-38
    assert getattr(settings, "HASNER_SOMBRA", False) is True
    assert settings.DATABASES["default"]["NAME"] == "sombra"
    assert connection.settings_dict["NAME"] == "sombra"
    assert connection.schema_name == "juliani"
    assert not connection.in_atomic_block and connection.get_autocommit()

    # app/ponto/management/commands/e6_oraculo.py:284-297
    # CLAUDE.md:147-148
    agora = timezone.localtime(AGORA_DA_RODADA or timezone.now())
    origem_agora = (
        "informado pela sessão" if AGORA_DA_RODADA is not None else "execução da sonda"
    )

    Colaborador = apps.get_model("colaboradores", "Colaborador")
    FechamentoMensal = apps.get_model("ponto", "FechamentoMensal")
    pks = {pk for pk, _ in DELTA_PUBLICADO}
    linhas = []

    with transaction.atomic():
        # app/chamados/tests/cef_c3_sonda.py:16-22
        # bin/sombra_db.sh:45-47
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema(), pg_current_snapshot()::text")
            banco, schema, snapshot_mvcc = cursor.fetchone()
        assert banco == "sombra" and schema == "juliani"

        # Universo de empresas ativas e fechamentos da competência.
        # app/ponto/management/commands/e6_oraculo.py:292-305,314
        colabs = Colaborador.objects.select_related("empresa").in_bulk(pks)
        fechamentos = set(
            FechamentoMensal.objects.filter(
                mes=MES, ano=ANO, colaborador_id__in=pks
            ).values_list("colaborador_id", flat=True)
        )

        por_colab = {}
        for pk, colab in colabs.items():
            # Mesmas chamadas e mesmo objeto espelho passado ao carregador.
            # app/ponto/management/commands/e6_oraculo.py:303-320,327-330
            ini, fim = janela_fechamento(MES, ANO, colab.empresa)
            espelho = espelho_do_colab(colab, HOJE, ini, fim, agora=agora)
            ins = InsumosDoDono(colab, ini, fim, HOJE, espelho=espelho, agora=agora)
            dias_espelho = {
                str(linha.get("data"))[:10]: linha for linha in (espelho.get("dias") or [])
            }
            por_colab[pk] = (ins, dias_espelho)

        for (pk, k), publicado in DELTA_PUBLICADO.items(): [nao verificado]
            dia = date.fromisoformat(k)
            colab = colabs.get(pk)
            ins, dias_espelho = por_colab.get(pk, (None, {}))
            de = dias_espelho.get(k)
            campos = de if de is not None else {}

            # app/ponto/services/espelho.py:801,829-837,863-874
            # app/ponto/management/commands/e6_oraculo.py:236-239,263-269,339-363
            isencao = campos.get("isencao")
            ausencia = campos.get("ausencia") is not None if de is not None else None
            leitura_espelho = minutos_do_espelho(de)
            m_esp, _ = leitura_espelho
            veredito = ins.oraculo if ins is not None else None
            piso = veredito.minutos.get(k, 0.0) if veredito is not None else None
            teto = veredito.envelope.get(k, 0.0) if veredito is not None else None
            delta = piso - m_esp if piso is not None and m_esp is not None else None
            guarda = diferenca_e_a_janela_declarada(delta, de) if delta is not None else None
            delta_inteiro = round(delta) if delta is not None else None

            # app/ponto/management/commands/e6_oraculo.py:98-110,149-154,156-212,393-404
            # app/ponto/supra_juiz.py:179-191
            horas = veredito.horas.get(k, []) if veredito is not None else None
            cadastro = dict.fromkeys((
                "folga_nao_definida",
                "sem_vinculo_no_dia",
                "sem_vinculo_em_dia_de_trabalho",
            ))
            entrada_solta = paridade = fase = cxr = dono = None
            if ins is not None:
                cadastro = cadastro_do_dia_carregado(pk, dia)
                entrada_solta = batida_fora_de_turno(colab, dia, ins.turnos())
                paridade = "impar" if len(horas) % 2 else "par"
                fase = k in ins.fase()
                cxr = k in ins.cxr()
                dono = ins.dono(dia)

            # Saídas do laço do R1, na ordem do command. A sonda também lê
            # os campos de diagnóstico das chaves que ficam fora da lista.
            # app/ponto/management/commands/e6_oraculo.py:303-305,331-378
            exclusao = None
            if colab is None:
                exclusao = "colaborador_ausente"
            elif not colab.empresa.ativa:
                exclusao = "empresa_inativa"
            elif pk not in fechamentos:
                exclusao = "sem_fechamento_da_competencia"
            elif not ins.ini <= dia <= ins.fim:
                exclusao = "fora_da_janela_de_fechamento"
            elif de is None:
                exclusao = "dia_sem_linha_no_espelho"
            elif isencao or ausencia:
                exclusao = "dia_isento_ou_ausencia"
            elif m_esp is None:
                exclusao = "dia_sem_lavratura_no_espelho"
            elif m_esp == 0 and piso == 0:
                exclusao = "dia_sem_trabalho_ambos"
            elif guarda:
                exclusao = "esp_desconta_janela_declarada"
            elif abs(delta) <= 10:
                exclusao = "bate_ate_10min"

            chave = f"col{pk} {k}"
            linhas.append({
                "chave": chave,
                "linha_no_espelho": de is not None,
                "isencao": isencao,
                "ausencia_presente": ausencia,
                "minutos_realizados": campos.get("minutos_realizados"),
                "pago_h": campos.get("pago_h"),
                "minutos_do_espelho": leitura_espelho,
                "piso_min": piso,
                "teto_min": teto,
                "horas": horas,
                "brutas": veredito.brutas.get(k, 0) if veredito is not None else None,
                "impares": k in veredito.impares if veredito is not None else None,
                "cortes": k in veredito.cortes if veredito is not None else None,
                "realizado_janela_descontada": campos.get("realizado_janela_descontada"),
                "guarda_janela": guarda,
                "delta_antes_de_arredondar_min": delta,
                "delta_arredondado_min": delta_inteiro,
                "delta_publicado_min": publicado, [nao verificado]
                "delta_bate_publicado": (
                    delta_inteiro == publicado [nao verificado]
                    if delta_inteiro is not None and publicado != NV else NV [nao verificado]
                ),
                **cadastro,
                "entrada_solta_apuravel": entrada_solta,
                "paridade_de_horas": paridade,
                "fase_deslocada_as_of": fase,
                "em_cadastro_x_realidade": cxr,
                "ins_dono": dono,
                "dono_r1": dono if exclusao is None else None,
                "exclusao_r1": exclusao,
            })

    assert len(linhas) == len({linha["chave"] for linha in linhas}) == 18
    for linha in linhas:
        print(json.dumps(linha, ensure_ascii=False, allow_nan=False))

    # O R1 conta dono nas divergências que passaram pelo laço.
    # app/ponto/management/commands/e6_oraculo.py:364-381,427-439
    contagem = Counter(linha["dono_r1"] for linha in linhas if linha["exclusao_r1"] is None)
    print(json.dumps({
        "chaves_conferidas": len(linhas),
        "banco": banco,
        "schema": schema,
        "snapshot_mvcc": snapshot_mvcc,
        "foto_lida": FOTO_LIDA,
        "foto_de_referencia": "sombra de 26/11/2026 (PUBLICADO.md, cabeçalho)", [nao verificado]
        "hoje": HOJE.isoformat(),
        "agora_local": agora.isoformat(),
        "origem_agora": origem_agora,
        "contagem_por_dono": {
            dono: contagem[dono] for dono in ("ESTRUTURA", "CADASTRO", "BATIDA")
        },
        "chaves_fora_do_r1": [
            {"chave": linha["chave"], "motivo": linha["exclusao_r1"]}
            for linha in linhas if linha["exclusao_r1"] is not None
        ],
        "chaves_com_dono_diferente_do_publicado": [
            linha["chave"] for linha in linhas
            if linha["exclusao_r1"] is None and linha["dono_r1"] != DONO_PUBLICADO
        ],
        "chaves_cujo_delta_nao_bate_o_publicado": [
            linha["chave"] for linha in linhas if linha["delta_bate_publicado"] is False
        ],
        "chaves_com_confronto_do_delta_nao_verificado": [
            linha["chave"] for linha in linhas if linha["delta_bate_publicado"] == NV
        ],
    }, ensure_ascii=False, allow_nan=False))


# Execução na sombra [nao verificado].
sonda()
```

## 26/11 10:57 · sessao 6fd2e2a15712 · O327-C11-93 · leitura · origem=Code

Os 95 PKs fornecidos chegaram ao ramo 1. Na saída raspada, todos têm `v.minutos` vazio, zero bloqueios na janela e 11 campos sem dono. A sonda anterior mediu a condição da guarda por `_DONOS_DECLARADOS`. A causa por PK e a ocorrência efetiva da exceção em produção estão [nao verificado]. `recusados.txt:1–95`, `app/ponto/calculador/alimentacao.py:449,467–475`, `app/ponto/services/dia_pago.py:50–76,312–321`.

### 1. Caminhos até `v.minutos` vazio

O universo admite quem tem situação diferente de `desligado` ou quem está desligado com demissão a partir do início da janela da empresa. A seleção dispensa testes de admissão, vigência, célula e batida. Já `rubricas_por_dia` passa **sempre** `pares=` ao núcleo. Nesse ramo, uma chave de `v.minutos` só nasce quando chega uma tupla com entrada e saída. As batidas avulsas alimentam o diagnóstico, e uma entrada sem saída só marca o dia como ímpar. Um par entregue ao núcleo com duração zero ainda criaria uma chave. `app/ponto/services/fechamento.py:448–465,640–653,1289–1300`, `app/ponto/calculador/alimentacao.py:449`, `app/ponto/calculador/nucleo.py:109–155`.

| Caminho | O que o código faz |
|---|---|
| **Sem batida apurável, inclusive no envelope** | A porta exclui retratadas. O fechamento busca também a véspera e o dia seguinte, então zero no recorte civil da janela por si só é insuficiente. Sem batidas no envelope não há par no motor. `app/ponto/turnos.py:2921–2948`, `app/ponto/services/fechamento.py:640–653`, `app/ponto/motor_calculo_v2.py:897–912,2362–2373`, `app/ponto/adaptador_motor.py:28–45`. |
| **Vínculo fora da janela ou nenhuma fatia executada** | O acumulador começa vazio. A fatia é limitada pela vigência, pelo início da operação e pela atribuição da célula. Pode ser pulada por inteiro, ou rodar sem receber as batidas que ficaram fora de sua cobertura. No O85, a recuperação de dias órfãos, quando admitida, segue por `orfaos_por_dia`, fora dos períodos que alimentam o calculador. O caso sem vínculo algum tem tratamento cadastral explícito. `app/ponto/services/fechamento.py:200–225,225–311,787–863,1293–1305`, `app/escala/alimentacao.py:148–159,205–216,269–286`. |
| **Sem célula** | A falta da célula desvia o pareamento da ata para a autoridade de geometria. A fatia ainda pode apurar pela vigência. No recorte de sobreposição em que já há dias atribuídos pela célula, um dia sem dono entra pela vigência única. A grade sem células dentro do vínculo tem uma saída própria, que pode deixar o previsto e o realizado sem resposta enquanto o motor ainda consegue parear. `app/ponto/motor_calculo_v2.py:558–576,611–638`, `app/ponto/services/fechamento.py:228–290,1044–1069,1111–1123`, `app/escala/models.py:1080–1107`. |
| **Ausência cobrindo** | A cobertura da folha usa ausências aprovadas. Ela alimenta justificação, supressão do previsto ou desconto segundo o tipo. O abono integral zera atraso e saída antecipada e preserva o trabalho apurado. A cobertura chamada pelo fechamento atua sobre esses insumos, enquanto as batidas já foram lidas em uma porta própria. A ausência pode explicar o dia sem par ou a falta de previsto, mas sua presença isolada não prova por que o par faltou. `app/ponto/turnos.py:2528–2537,2741–2753`, `app/ponto/services/fechamento.py:652–660,700–721,744–775`, `app/ponto/motor_calculo_v2.py:1095–1122`. |
| **Empresa ou regime** | A empresa define o corte da competência e a aplicação da janela de HE. CLT, CCT ausente e regime vazio têm caminhos que devolvem motor. Intermitente também tem motor e apura pares fechados. `CicloSemMotor` na montagem da fatia interrompe o cálculo antes da chamada de rubricas. Uma janela de HE aplicável pode colapsar o par, que então segue pelo caminho de segmento descartado. `app/ponto/janelas.py:38–57`, `app/core/regua_cct.py:351–373,428–489`, `app/ponto/motor_calculo_v2.py:1389–1412,3589–3605,3657–3676`, `app/ponto/services/fechamento.py:744–775,1327–1332`. |
| **Batida apurável sem par fechado** | O motor restringe a lista a E/S e remove duplicatas antes de parear. Saída órfã vira anomalia sem período. Entrada em aberto pode chegar ao núcleo com `saida=None`. O motor pode creditar uma parte já fechada a um período que segue aberto. A sonda mede esse crédito separadamente, pois o segmento aberto enviado ao núcleo ainda fica sem saída. `app/ponto/motor_calculo_v2.py:897–912,2362–2373,2527–2580`, `app/ponto/adaptador_motor.py:28–45`, `app/ponto/calculador/alimentacao.py:83–113`, `app/ponto/calculador/nucleo.py:139–147`. |
| **Período cortado por dia ou descartado pela alimentação** | O motor corta o período cujo dia de jornada caiu fora da fatia. A alimentação pula períodos sem mapa do dia, fechados sem carimbo e pares efetivos com `saída <= entrada`. O bloqueio HE100 sem previsto também colapsa o par, mas, se publicado dentro da janela, ganha um item de rubrica com zero. Esse último mecanismo explica `v.minutos` vazio com rubrica, e está ausente nas 95 linhas fornecidas. `app/ponto/motor_calculo_v2.py:2415–2448,1479–1505`, `app/ponto/calculador/alimentacao.py:73–123,459–475`, `recusados.txt:1–95`. |

### 2. Dono e decisão sobre a recusa

- **A guarda está correta para a chamada medida.** Com `{}` e 11 campos sem dono, `dia_pago.py:317` deve recusar antes de substituir as linhas da versão oráculo. Se faltam os fatos necessários para afirmar trabalho ou uma rubrica, inventar zero seria incorreto. Se já há trabalho apurado, abono, falta decidida ou outro fato afirmado, a guarda continua correta e a perda desse fato na composição é o que precisa ser explicado. O escritor já tem entradas próprias para esses fatos e para o ajuste mensal. `app/ponto/services/dia_pago.py:50–76,173–186,234–292,312–321,348–361`, `app/docs/LEIS.md:61,65,113,122`.
- **O E6 julga divergência de minutos por dia.** A recusa por campos sem dono é da chamada inteira. O E6 só chama o juiz depois de encontrar um dia comparável com diferença acima de dez minutos e excluir a diferença explicada pela janela declarada. Em um dia divergente, vínculo ausente ou folga indefinida têm precedência como `CADASTRO`, batida solta ou ímpar pode dar `BATIDA`, e fase lavrada ou cadastro × realidade podem dar `CADASTRO`. `ESTRUTURA` é o resultado residual de uma divergência comparável. A sonda aplica esse mesmo filtro por dia e chama `InsumosDoDono.dono()`. O dono individual das divergências está [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110,128–154,182–195,328–379`, `app/docs/LEIS.md:122`.
- **O corte legítimo usa o dia do juiz e a célula soberana.** Pela L-085, um turno do juiz fora da competência fica na competência a que pertence. Se um par fechado com dia do juiz dentro da fatia é descartado porque a herança de 0–6 h o levou para fora, a perda é estrutural. A resposta NOTURNO-A também manda preservar o trabalhado após o marco quando a saída na madrugada fecha um turno que não cruza. A L-097 classifica a HE desse trecho. Ausência aprovada e vínculo fora da vigência mantêm o tratamento que o cadastro e a célula sustentam. A presença desses casos entre os 95 está [nao verificado]. `app/docs/LEIS.md:71,115,117,121–122`, `app/docs/PROMPTS.md:2928`, `app/escala/alimentacao.py:269–286`, `app/ponto/motor_calculo_v2.py:936–951,2415–2448`, `app/ponto/janela_he.py:173–191`.

A sonda usa a janela de cada empresa, intercepta as duas lavras em memória e imprime uma linha JSON por PK, seguida da contagem. O total por caminho admite sobreposição quando duas condições foram observadas no mesmo PK. A execução da sonda e a confirmação independente do hash da cópia estão [nao verificado]. `app/ponto/services/fechamento.py:574–577,640–653,1215–1225,1289–1320`, `app/ponto/services/dia_pago.py:359–360`.

```python
import sys

_pyc_anterior = sys.dont_write_bytecode
sys.dont_write_bytecode = True

import json
import logging
from collections import Counter, defaultdict
from contextlib import ExitStack
from datetime import datetime, time, timedelta
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.alimentacao import (
    celulas_do_periodo,
    escalas_do_periodo,
    vinculo_cobre_a_data,
    vinculo_do_dia,
)
from ponto.calculador import alimentacao
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.motor_calculo_v2 import MotorBase, localizar
from ponto.services import dia_pago, fechamento
from ponto.services.espelho import espelho_do_colab
from ponto.turnos import (
    ausencias_vigentes,
    batidas_apuraveis,
    cobertura_ausencia_periodo,
    dias_da_ausencia,
)


MES, ANO = 11, 2026
UM_DIA = timedelta(days=1)
# recusados.txt:1–95
PKS = tuple(map(int, """
041 053 066 075 076 083 090 128 142 144 181 186 199 209 210 216
225 234 236 257 268 271 274 291 298 312 317 318 379 391 427 440
499 518 534 574 607 638 641 642 644 645 646 647 649 650 663 667
674 675 677 678 697 699 704 717 718 751 752 755 758 761 832 883
900 915 921 938 966 973 980 984 1009 1016 1025 1027 1033 1043
1049 1053 1054 1055 1056 1057 1058 1059 1060 1061 1062 1063
1064 1065 1066 1067 1068
""".split()))


def donos_e6(colab, ini, fim, dias, agora):
    # app/ponto/management/commands/e6_oraculo.py:328–379
    hoje = timezone.localtime(agora).date()
    espelho = espelho_do_colab(colab, hoje, ini, fim, agora=agora)
    insumos = InsumosDoDono(colab, ini, fim, hoje, espelho=espelho, agora=agora)
    linhas = {str(d.get("data"))[:10]: d for d in espelho.get("dias") or ()}
    donos = {}
    saidas = Counter()

    for dia in dias:
        chave = dia.isoformat()
        linha = linhas.get(chave)
        if linha is None:
            saidas["sem_linha_no_espelho"] += 1
            continue
        if linha.get("isencao") or linha.get("ausencia") is not None:
            saidas["isento_ou_ausencia"] += 1
            continue
        minutos_espelho, _ = minutos_do_espelho(linha)
        if minutos_espelho is None:
            saidas["sem_lavratura_no_espelho"] += 1
            continue
        minutos_e6 = insumos.oraculo.minutos.get(chave, 0.0)
        diferenca = minutos_e6 - minutos_espelho
        if minutos_espelho == minutos_e6 == 0:
            saidas["sem_trabalho_em_ambos"] += 1
        elif diferenca_e_a_janela_declarada(diferenca, linha):
            saidas["janela_declarada"] += 1
        elif abs(diferenca) <= 10:
            saidas["ate_10_minutos"] += 1
        else:
            donos[chave] = insumos.dono(dia)
    return donos, dict(saidas)


def caminhos_da_recusa(obs, apuracao, motor):
    if obs is None or "campos_sem_dono" not in obs:
        return ["rubricas_ou_guarda_nao_observadas"]
    if obs["ramo"] != 1:
        return [f"ramo_{obs['ramo']}_na_sombra"]

    caminhos = []
    cnt = obs["segmentos"]
    if obs["n_batidas_envelope"] == 0:
        caminhos.append("sem_batida_apuravel_no_envelope")
    if apuracao["n_fatias"] == 0:
        caminhos.append("sem_fatia_executada")
    elif obs["n_batidas_envelope"] and motor["batidas_nas_fatias"] == 0:
        caminhos.append("batidas_fora_da_entrada_das_fatias")
    if motor["batidas_nas_fatias"] and motor["batidas_apos_filtro_motor"] == 0:
        caminhos.append("batidas_filtradas_no_motor")
    if motor["batidas_apos_filtro_motor"] and motor["periodos_montados"] == 0:
        caminhos.append("pareador_sem_periodo")
    if obs["cortes_da_fatia"]:
        caminhos.append("periodo_cortado_pelo_dia_da_fatia")
    for motivo in (
        "turno_aberto",
        "sem_dia_da_jornada",
        "sem_carimbo_do_motor",
        "par_invertido_pela_janela",
        "par_bloqueado_sem_previsto",
    ):
        if cnt.get(motivo):
            caminhos.append(motivo)
    return caminhos or ["outro [nao verificado]"]


def sonda():
    # app/config/settings/sombra.py:13–14,29,47–51,60
    if (
        getattr(settings, "HASNER_SOMBRA", False) is not True
        or connection.settings_dict["NAME"] != "sombra"
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.schema_name != "juliani"
    ):
        raise RuntimeError("Abra o shell da sombra no schema juliani.")
    if (MES, ANO) in fechamento.GUARDA_COMPETENCIA:
        raise RuntimeError("Competencia presente em GUARDA_COMPETENCIA.")
    if len(PKS) != 95 or len(set(PKS)) != 95:
        raise ValueError("Lista de PKs diferente de recusados.txt.")

    codigo_fechamento = fechamento.recalcular_fechamento_mes.__code__
    codigo_fatia = fechamento.apurar_por_fatia.__code__
    codigo_motor = MotorBase.calcular_mes.__code__
    codigo_rubricas = alimentacao.rubricas_por_dia.__code__

    apuracoes, observacoes = {}, {}
    motores = defaultdict(Counter)
    cortes = defaultdict(list)
    pk_em_fatia = None

    def ler_motor(frame, evento, retorno):
        locais = frame.f_locals
        # app/ponto/motor_calculo_v2.py:2415–2448
        if evento == "line" and frame.f_lineno in (2446, 2448):
            entrada, saida = locais["entrada"], locais["saida"]
            dia_juiz = locais["_dia_turno_juiz"].get(localizar(entrada))
            cortes[pk_em_fatia].append({
                "dia_usado": locais["data_jornada"],
                "dia_juiz": dia_juiz,
                "fatia": [locais["data_ini"], locais["data_fim"]],
                "par_fechado_positivo": saida is not None and saida > entrada,
                "juiz_dentro_da_fatia": (
                    dia_juiz is not None
                    and locais["data_ini"] <= dia_juiz <= locais["data_fim"]
                ),
            })
        if evento == "return" and retorno is not None:
            # app/ponto/motor_calculo_v2.py:2362–2373,2733–2737
            motores[pk_em_fatia].update({
                "batidas_nas_fatias": len(locais["batidas"]),
                "batidas_apos_filtro_motor": len(locais["batidas_limpas"]),
                "periodos_montados": len(locais["periodos"]),
            })
        return ler_motor

    def ler_fatia(frame, evento, retorno):
        nonlocal pk_em_fatia
        if evento == "return":
            if retorno is not None:
                # app/ponto/services/fechamento.py:283–311,338–341
                apuracoes[pk_em_fatia] = {
                    "n_fatias": len(retorno.regua_por_fatia),
                    "dias_cobertos": sorted(retorno.dias_cobertos),
                    "motores": sorted(frame.f_locals["_classes_observadas"])
                    if "_classes_observadas" in frame.f_locals else [],
                }
            pk_em_fatia = None
        return ler_fatia

    def ler_rubricas(frame, evento, retorno):
        if evento == "return" and isinstance(retorno, dict):
            # app/ponto/calculador/alimentacao.py:449–475,533
            locais = frame.f_locals
            chamador = frame.f_back.f_locals
            pk = locais["c"].pk
            itens, bloqueios = locais["_itens"], locais["_bloq"]
            n_minutos = len(locais["v"].minutos)
            ramo = (
                (3 if len(itens) > n_minutos else 0)
                if retorno else (2 if itens else 1)
            )
            observacoes[pk] = {
                "ramo": ramo,
                "rubricas_objeto": retorno,
                "n_dias_rubricas": len(retorno),
                "n_dias_v_minutos": n_minutos,
                "n_bloqueios_na_janela": len(bloqueios),
                "n_batidas_envelope": len(locais["batidas_locais"] or ()),
                "segmentos": dict(locais["insumos"]["contadores"]),
                "cortes_da_fatia": cortes.get(pk, []),
                "dias_previsto_suprimido_por_ausencia": sorted(
                    chamador["_datas_suprimidas"]
                ),
                "dias_abono_integral": sorted(chamador["_dias_aus_inteira"]),
                "grade": Counter(
                    d.get("tipo_dia") or "sem_tipo"
                    for d in (chamador.get("_dias_g") or ())
                ),
            }
        return ler_rubricas

    def rastrear(frame, evento, argumento):
        if evento != "call":
            return None
        codigo = frame.f_code
        pai = frame.f_back
        if codigo is codigo_fatia and pai.f_code is codigo_fechamento:
            nonlocal pk_em_fatia
            pk_em_fatia = frame.f_locals["colab"].pk
            frame.f_trace_lines = False
            return ler_fatia
        if codigo is codigo_motor and pk_em_fatia is not None:
            return ler_motor
        if codigo is codigo_rubricas and pai.f_code is codigo_fechamento:
            frame.f_trace_lines = False
            return ler_rubricas
        return None

    def interceptar_lavra(colab, mes, ano, *, versao, rubricas=None, **dados):
        # app/ponto/services/fechamento.py:1223–1261,1301–1320
        # app/ponto/services/dia_pago.py:50–76,312–321,359–360
        if versao == "motor":
            return 0
        obs = observacoes[colab.pk]
        if versao != "oraculo" or (mes, ano) != (MES, ANO):
            raise ValueError("Chamada de lavra fora do escopo da sonda.")
        if rubricas is not obs.pop("rubricas_objeto"):
            raise ValueError("Lavra recebeu outro objeto de rubricas.")
        fontes = (
            "faltas_por_dia", "abono_por_dia",
            "previsto_por_dia", "realizado_por_dia",
        )
        mapas = [dados[campo] for campo in fontes]
        donos = dia_pago._DONOS_DECLARADOS(
            rubricas, *mapas, do_motor=dados.get("do_motor", ()),
        )
        obs["campos_sem_dono"] = sorted(set(dia_pago.CAMPOS_DIA) - donos)
        obs["recusa_317"] = bool(obs["campos_sem_dono"])
        obs["dias_por_mapa_da_folha"] = {
            campo: None if mapa is None else len(mapa)
            for campo, mapa in zip(fontes, mapas)
        }
        obs["n_furos_afirmaveis"] = len(dados.get("furos_do_motor") or ())
        obs["minutos_orfaos_o85"] = sum(
            (dados.get("orfaos_por_dia") or {}).values()
        )
        obs["trabalho_positivo_em_periodos"] = any(
            (p.minutos_trabalhados or 0) > 0
            for campo in ("periodos", "ft_certa", "ft_feriado", "ft_sem_escala")
            for p in (dados.get(campo) or ())
        )
        return 0

    linhas = []
    total_caminhos, total_contextos = Counter(), Counter()
    with ExitStack() as sessao:
        sessao.callback(logging.disable, logging.root.manager.disable)
        logging.disable(sys.maxsize)
        sessao.enter_context(transaction.atomic(durable=True))
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco ou schema fora da sombra esperada.")

        universo = set(
            fechamento.universo_da_competencia(MES, ANO)
            .filter(pk__in=PKS).values_list("pk", flat=True)
        )
        colabs = Colaborador.objects.select_related("empresa").in_bulk(PKS)
        agora = timezone.now()

        with ExitStack() as observacao:
            observacao.enter_context(patch.object(dia_pago, "lavrar", interceptar_lavra))
            observacao.callback(sys.settrace, sys.gettrace())
            sys.settrace(rastrear)
            fechamento.recalcular_fechamento_mes(
                MES, ANO, colaborador_ids=PKS,
                somente_leitura=True, lavrar_dias_pagos=True,
            )

        for pk in PKS:
            linha = {"pk": pk, "col": f"col{pk:03d}", "no_universo": pk in universo}
            colab = colabs.get(pk)
            if colab is None:
                linha["caminhos"] = ["pk_ausente_na_sombra"]
                linhas.append(linha)
                total_caminhos.update(linha["caminhos"])
                continue

            # app/ponto/janela

[truncado em 20000 de 25542 caracteres]

## 26/11 10:53 · sessao bce89212e720 · R1-SEM-DONO-2611 · censo · origem=Code

# R1-SEM-DONO-2611 · censo da competência 11

**Dono pelo E6: ESTRUTURA nas 18 chaves.** O mapa tira as 18 da lista ESTRUTURA de 26/11. A obra responsável pela cura é uma atribuição diferente. A fonte selecionada pelo E6 para o número do espelho, os insumos individuais do dono e o nexo de cada mecanismo com o delta de 26/11 estão **[nao verificado]**. [Anexo r.md §1 e §3, `depois_c11.txt:12–49`] [e6_oraculo.py:342–380](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:342)

## Caminho dos dois números em cada linha

| Número | Sítio produtor e ponto de separação |
|---|---|
| **E6** | `InsumosDoDono` lê só os instantes locais de `batidas_apuraveis` e chama `minutos_do_oraculo(ts)` sem `pares` ou `envelopes`. O núcleo corta a sequência por gap ou vão, chaveia cada trecho pela data civil do primeiro instante e soma os pares por posição, em minutos. O comando lê `e6.get(k, 0.0)`. `app/ponto/management/commands/e6_oraculo.py:149–154,350` · `app/ponto/calculador/nucleo.py:109–110,186–225,235–254` |
| **Espelho, fonte prioritária** | `montar_dias` lê `turnos_do_colab` na janela visual e publica o resultado de `realizado_do_dia` em `minutos_realizados`. Aqui a sequência ganha papel da ata, marcos e vínculo do dia. O realizado agrupa por `Turno.data_turno`, valoriza a pausa e soma os turnos. Zero numérico nessa fonte tem prioridade. `app/ponto/services/espelho.py:267–294,625–628,689–690,801,1298–1302` · `app/ponto/turnos.py:719–771,1940–1944,1977–2002` · `app/ponto/management/commands/e6_oraculo.py:263–265` |
| **Espelho, quando o realizado é nulo** | `montar_dias` publica `pago_h` a partir do `DiaPago` de versão `motor`. O fechamento entrega períodos e mapa do dia a `lavrar`, que grava as rubricas. `pago_do_dia` soma trabalhadas, folga trabalhada e folga-feriado. O espelho arredonda em horas, e o E6 converte por ×60. Com as duas fontes nulas, o comando exclui a chave. Os períodos do motor corrente também alimentam os cartões e a lista Cadastro × Realidade. `app/ponto/services/fechamento.py:1224–1251` · `app/ponto/services/dia_pago.py:173–178,235–272,359–360,833–856,890–898` · `app/ponto/services/espelho.py:420–426,435–474,829–830,1330–1331` · `app/ponto/management/commands/e6_oraculo.py:162–172,263–269,342–349` |

A regra efetiva do dono roda nesta ordem: **CADASTRO** por folga indefinida, sem vínculo ou sem vínculo em dia de trabalho → **BATIDA** por turno aberto com entrada solta apurável ou contagem ímpar em `horas[k]` → **CADASTRO** por fase deslocada lavrada ou presença do dia na lista do motor → **ESTRUTURA** no resíduo. A contagem relevante é a atribuída pelo oráculo à chave, inclusive zero, e o marcador `IMPAR` do trecho é apurado à parte. Essa precedência explica o dono publicado. Os valores dos predicados por chave estão **[nao verificado]**. `app/ponto/management/commands/e6_oraculo.py:98–110,174–212,378–404` · `app/docs/LEIS.md:122`

## Dia → mecanismo → dono → obra

As letras remetem ao ponto de separação descrito abaixo, após os caminhos comuns acima. “Família registrada” indica o encaixe do achado disponível. A atribuição do **delta R1 de 26/11** à obra permanece **[nao verificado]** onde a tabela indica candidatura. [Anexo r.md §1 e §3] `app/ponto/management/commands/e6_oraculo.py:342–378`

| Dia de 2026 | Mecanismo ou evidência disponível | Dono R1 | Obra |
|---|---|---|---|
| **col112 21/10** | **A · Pareamento.** A leitura anterior traz `17:03,07:01` no E6 e `06:57→17:03` no pareador cru. Período efetivo usado no R1 **[nao verificado]**. `RELATO.md:13115–13117` · `depois_c11.txt:14–15` | ESTRUTURA | Família registrada em `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, “pareamento posicional”. Nexo R1 **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col112 22/10** | **A · Pareamento.** A leitura anterior traz `17:02,06:56` no E6 e `07:01→17:02` no pareador cru. Período efetivo usado no R1 **[nao verificado]**. `RELATO.md:13116–13117` · `depois_c11.txt:14–15` | ESTRUTURA | `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, mesma família. Nexo R1 **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col200 03/11** | **B · Chave da madrugada.** Há um par civil de 03/11 atribuído ao turno de 02/11 e uma colisão nominal às 02:00. Qual deles entra no `420 × 598–720` **[nao verificado]**. `RELATO.md:189,2360` · `depois_c11.txt:23` | ESTRUTURA | A chave do par tem escopo em `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` e na família de madrugada registrada em `EV-NA-BRECHA-1611`. Obra do delta de 03/11 **[nao verificado]**. `BACKLOG.md:823,1989,1991` · `GRADE-MESTRA.md:303` |
| **col382 11/11** | **B · Chave da madrugada.** O teste nominal dá o par `00:02→07:51` civil de 11/11 ao turno de 10/11. Sua contribuição para `437 × 854–949` **[nao verificado]**. `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` · `depois_c11.txt:36` | ESTRUTURA | Mesmas famílias registradas. `P0-[nome]-RELAVRA-10` inclui o colaborador, com efeito em 11/11 **[nao verificado]**. `BACKLOG.md:823,1989,1991,2303` · `GRADE-MESTRA.md:303` |
| **col114 08/11** | **C · Comparação de pausa.** Consta do recorte anterior de intervalo, que ficou incompleto. Par, piso e janela descontada **[nao verificado]**. `RELATO.md:13118–13119` · `depois_c11.txt:18–19` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:13118–13119` |
| **col252 18/11** | **C · Comparação de pausa.** O colaborador tem família documentada em outras datas. Pausa curta nesta chave **[nao verificado]**. `RELATO.md:2390,3096` · `depois_c11.txt:26–32` | ESTRUTURA | Candidata `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, “pausa L-032”. Encaixe nesta data **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col252 19/11** | **C · Comparação de pausa.** Par, piso e janela descontada nesta chave **[nao verificado]**. `RELATO.md:2390,3096` · `depois_c11.txt:26–32` | ESTRUTURA | Mesma candidata registrada. Encaixe nesta data **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col884 08/11** | **C · Comparação de pausa.** Aparece no recorte de intervalo e no levantamento “sem nome”, entre as batidas fora de turno. Ramo que produziu o delta **[nao verificado]**. `RELATO.md:3097,13118–13119` · `depois_c11.txt:41` | ESTRUTURA | `EV-NA-BRECHA-1611` cobre a investigação “SEM NOME 12”. Obra da cura ou classe nova **[nao verificado]**. `BACKLOG.md:823` |
| **col922 01/11** | **D · Turnos somados e cartões exibidos.** Diagnóstico nominal anterior, registrado “a confirmar”. Nexo com o R1 **[nao verificado]**. `RELATO.md:2926,3097` · `depois_c11.txt:43–46` | ESTRUTURA | Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611`, “funcao que soma x que desenha”. `BACKLOG.md:823,1469,2529` |
| **col922 04/11** | **D · Turnos somados e cartões exibidos.** Mesmo diagnóstico nominal. Turno ou período responsável pelo delta R1 **[nao verificado]**. `RELATO.md:2926,3097` · `depois_c11.txt:43–46` | ESTRUTURA | Mesma candidata `O54` / `E3-geometria`, via `EV-NA-BRECHA-1611`. Atribuição R1 **[nao verificado]**. `BACKLOG.md:823,1469,2529` |
| **col302 20/11** | **E · Origem do zero no E6.** Piso zero e espelho positivo. Trecho, chave e fonte do positivo **[nao verificado]**. `depois_c11.txt:34` · `app/ponto/management/commands/e6_oraculo.py:350–372` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `MOTOR-LE-O-TURNO` registra outras chaves com esse sintoma. `BACKLOG.md:2234` · [Anexo r.md §1] |
| **col996 28/10** | **E · Origem do zero no espelho.** Zero selecionado pelo leitor e piso E6 positivo. Fonte do zero **[nao verificado]**. `depois_c11.txt:47` · `app/ponto/management/commands/e6_oraculo.py:263–269,370–372` | ESTRUTURA | `O54` / `E1-sem-previsao` acompanha o contador. Obra da cura ou classe nova **[nao verificado]**. `BACKLOG.md:1469` · `app/core/espelho_verdade.py:60–61` |
| **col114 15/11** | **F · Separação específica [nao verificado].** O recorte anterior de intervalo nomeia 08/11. `depois_c11.txt:18–19` · `RELATO.md:13118` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col130 29/10** | **F · Separação específica [nao verificado].** A leitura anterior deixou o caso fora do envelope. `RELATO.md:2926` · `depois_c11.txt:20` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:2926` |
| **col220 10/11** | **F · Separação específica [nao verificado].** A leitura anterior deixou o caso fora do envelope. `RELATO.md:2926` · `depois_c11.txt:25` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:2926` |
| **col874 22/10** | **F · Separação específica [nao verificado].** `depois_c11.txt:40` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col989 23/10** | **F · Separação específica [nao verificado].** `depois_c11.txt:43–46` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col996 10/11** | **F · Separação específica [nao verificado].** `depois_c11.txt:48–49` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |

## A · Pareamento por posição e pareamento do turno

O primeiro ponto observado de separação em **col112 21/10 e 22/10** é a escolha das pontas. O produtor de `Veredito.minutos` soma posições consecutivas do trecho, e `e6_oraculo` lê o acumulado por dia. No realizado, `_mk` produz o `Turno` com entrada, saída e `data_turno`, e `realizado_dos_turnos` lê essas pontas, incluindo o último par fechado de um turno aberto quando existir. O motor monta seus períodos em `calcular_mes` e pode substituir os do dia pelas pontas julgadas pela ata. A leitura anterior de col112 usou `parear_turnos` **sem marcos/ata**. Os períodos do motor e os turnos do espelho na rodada de 26/11 estão **[nao verificado]**. `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/management/commands/e6_oraculo.py:350` · `app/ponto/turnos.py:312–324,609–622,719–741,1997–2000` · `app/ponto/motor_calculo_v2.py:632–645,669–688,2362–2370` · `app/docs/RELATO.md:13115–13117`

**Escritores e leitores.** `registrar_batida` grava as pontas de origem, e `retratar_batida` muda a participação no universo de apuração. O cartório lavra a ata, `papel_por_minuto_da_ata` a lê para o pareador, e os turnos alimentam o realizado e a guarda de batida solta do E6. Do lado do motor, os períodos seguem para os cartões e para `DiaPago`. `app/ponto/registro_batida.py:118–127,162–180,287–301` · `app/ponto/turnos.py:1867–1871,1940–1944,2940–2948` · `app/ponto/services/cartorio.py:1907–1909` · `app/ponto/management/commands/e6_oraculo.py:198–212` · `app/ponto/services/espelho.py:435–474` · `app/ponto/services/dia_pago.py:173–178`

**Leis e padrão.** L-002 manda ler a autoridade, L-030 guarda a origem da batida, L-115 define a saída posicional no caso de pausa fechada e L-114 dá ao E6 o papel de termômetro. O padrão para corrigir a família registrada é **Single Source of Truth / um juiz por pergunta**, com **Architecture Conformance Test** sobre o par e o dia produzidos. `EV-NA-BRECHA-1611` registra o instrumento `ORACULO-E6-43-DEFEITOS`, cuja frase inclui “pareamento posicional 11”. `app/docs/LEIS.md:60,78,153–154` · `app/docs/CORTES.md:455,461` · `app/docs/GLOSSARIO.md:11–15` · `app/docs/PLANO-PADROES.md:9,53–65` · `app/docs/BACKLOG.md:823` · `app/docs/GRADE-MESTRA.md:303`

## B · Dia civil do trecho, dia do turno e dia do período pago

A separação pode ser lida em três locais. O **E6** chaveia pelo primeiro instante civil do trecho. O **realizado** soma pelo `data_turno` produzido por `_data_do_turno`, que consulta as bordas da véspera. O **motor** tem um mapa do juiz para a janela de HE e um caminho de agregação que pode herdar `_prev_dj` após um período fechado com gap de até seis horas. O fechamento passa `resultado.dia_da_jornada` à lavra, e `dia_pago._dia_de` usa esse mapa quando há chave para o período. Cada mudança de chave precisa ser seguida em D e na véspera para fechar o delta. `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:285–324,719–721` · `app/ponto/motor_calculo_v2.py:936–951,1545–1588,2415–2429` · `app/ponto/services/fechamento.py:1241` · `app/ponto/services/dia_pago.py:126–132`

**Escritores e leitores.** `_mk` produz `Turno.data_turno`. O realizado e `dia_das_batidas` consomem essa chave. O motor monta e publica seu mapa de saída, lido pela lavra e pela testemunha da exportação. A colisão registrada de col200 tem outra origem, `colisoes_tipo_oposto` e a porta de gravação. Sua participação no par comparado em 03/11 está **[nao verificado]**. `app/ponto/turnos.py:312–324,719–721,2951–2982` · `app/ponto/motor_calculo_v2.py:1085–1092,2415–2429,2950–2978` · `app/ponto/services/fechamento.py:1241` · `app/folha/porta_export.py:101–121` · `app/ponto/registro_batida.py:94–127,162–180` · `app/docs/RELATO.md:2360`

**Leis, padrão e obras.** L-085 dá a cauda ao dia de início do plantão. L-097 e L-084 regem a janela e a avaliação das pontas cruas. O corte `NOTURNO-A-JANELA-NO-DIA-DO-TURNO-2411-0210` fixa a âncora da janela. O padrão é **Single Source of Truth / um juiz por pergunta** para o dia, com **CQRS com projeções reconstruíveis** para o valor já lavrado. `app/docs/LEIS.md:115–117,142` · `app/docs/CORTES.md:329,646` · `app/docs/GLOSSARIO.md:11–12,25–26` · `app/docs/PLANO-PADROES.md:11,28–34`

- `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` registra “janela de HE ancora no dia do turno”. `MOTOR-LE-O-TURNO-DIA-DO-PAR` registra a herança de 0–6 h na lavra/FT. A obra-mãe `MOTOR-LE-O-TURNO` manda que “motor e DiaPago leem par e dia SO de core/juizes.py::turnos_do_colab”. O defeito de atribuição da madrugada no **instrumento** já está na família registrada em `EV-NA-BRECHA-1611`. Qual desses caminhos explica os dois deltas da tabela está **[nao verificado]**. `app/docs/BACKLOG.md:823,1989,1991,2234` · `app/docs/GRADE-MESTRA.md:303`
- `P0-[nome]-RELAVRA-10` registra a relavra dos dez colaboradores, incluindo col200 e col382. `COMP11-RELAVRA-FROTA` registra a relavra da frota. A contribuição de cada ato para estas duas chaves está **[nao verificado]**. `app/docs/BACKLOG.md:2303–2304` · `app/ponto/services/fechamento.py:1215–1241`

## C · Pausa observada, piso e janela declarada

O E6 deixa o vão entre pares fora da soma. O realizado lê `intervalos_do_turno`, que só reconhece pausa marcada de pelo menos 15 min. Na ausência de pausa reconhecida, o ramo aplicável de intervalo fixo desconta a janela declarada que intercepta o turno. Esse ramo produz `janela_descontada`, que o espelho publica como `realizado_janela_descontada`. O E6 lê o carimbo e exclui a comparação quando `abs((E6 − espelho) − janela) <= 10`, com janela diferente de zero. A discriminação entre pausa abaixo do piso, par/turno diferente e ausência de carimbo nas quatro chaves está **[nao verificado]**. `app/ponto/calculador/nucleo.py:243–254` · `app/ponto/turnos.py:495–499,542–546,756–771` · `app/ponto/services/espelho.py:837` · `app/ponto/management/commands/e6_oraculo.py:215–239,350–358`

**Escritores e leitores.** A célula guarda a régua e a ata. As portas de célula e o cartório são os sítios de geração, regeneração e lavra pertinentes. O espelho lê `vinculo_do_dia` e `intervalo_do_dia`. As marcas de pausa vêm do pareamento, lidas por `intervalos_do_turno`, pelo realizado e pelo motor. O leitor direto do carimbo de desconto na comparação é `diferenca_e_a_janela_declarada`. A guarda de rajada e a impressão da ata têm obras próprias, com participação nos quatro deltas **[nao verificado]**. `app/ponto/portas/celula.py:112,899` · `app/ponto/services/cartorio.py:1907–1909` · `app/ponto/services/espelho.py:286–294,837` · `app/ponto/turnos.py:502–546,798–815` · `app/ponto/motor_calculo_v2.py:2563,3006` · `app/ponto/management/commands/e6_oraculo.py:236–239` · `app/docs/BACKLOG.md:2538,2558,2560`

**Leis e padrão.** L-032 rege a pré-assinalação e o intervalo batido dentro do envelope. O corte de 14/09 está transcrito no realizado, com regra para a janela fixa e para a pausa livre. O corte D1 vigente trata da guarda do casador. São pontos distintos do cálculo. O padrão da cura do instrumento é **Single Source of Truth / um juiz por pergunta**, com **Architecture Conformance Test** sobre qual pausa foi reconhecida e qual desconto foi publicado. A obra registrada cita “pausa L-032 12”. A leitura nominal anterior de col252 cita 09, 10, 11, 13 e 17/11. `app/docs/LEIS.md:60,80,136` · `app/docs/CORTES.md:601` · `app/ponto/turnos.py:707–717` · `app/docs/GLOSSARIO.md:11–15` · `app/docs/PLANO-PADROES.md:9,61–65` · `app/docs/BACKLOG.md:823` · `app/docs/GRADE-MESTRA.md:303` · `app/docs/RELATO.md:2390,3096`

## D · Soma do realizado e desenho dos cartões

Em **col922 01/11 e 04/11**, o diagnóstico nominal anterior é “realizado soma turno que a tela nao exibe”, a confirmar. A separação no código está entre `turnos_do_colab → realizado_do_dia → minutos_realizados` e `resultado.periodos + periodos_ft → índice por entrada → instante → PK → dia → dia.turnos`. Período sem dia nessa ponte fica fora de `dia.turnos`. O template e os leitores do papel consomem a lista. O E6 consome o número selecionado no leitor, sem somar os cartões. Assim, a diferença entre soma e desenho, mesmo quando reproduzida, requer uma segunda comparação para demonstrar o delta com o E6. `app/docs/RELATO.md:2926,3097` · `app/ponto/services/espelho.py:435–474,625–628,670,689–690,778–801` · `app/templates/ponto/espelho.html:194–203` · `app/relatorios/pdf_espelho.py:902,965` · `app/ponto/management/commands/e6_oraculo.py:263–269`

**Escritores, leitores e leis.** O motor produz `periodos` e `periodos_ft`. `montar_dias` produz os campos `turnos` e `minutos_realizados` do espelho. O realizado tem ainda leitores no estado do dia e na soma `total_relogio`. L-002 rege a fonte comum dos leitores, `CARTAO-E-ESPELHO` manda o PDF desenhar o mesmo `dias`, e L-095 rege o valor pago. O padrão é **CQRS com projeções reconstruíveis**, com **Single Source of Truth** para a geometria usada por cada projeção. `O54` registra o programa. `EV-NA-BRECHA-1611` registra a leitura, e `PRIORIDADE-ESPELHO-VERDADE-E-TRES-CORTES` mantém `E3-geometria` “a confirmar” para col922. O item histórico `E3-geometria` registra a entrega `O65`. `app/ponto/motor_calculo_v2.py:2508–2510` · `app/ponto/services/espelho.py:778–801,930–932` · `app/ponto/status_dia.py:190` · `app/docs/LEIS.md:60,113,142` · `app/docs/CORTES.md:341` · `app/docs/GLOSSARIO.md:11,25` · `app/docs/PLANO-PADROES.md:11,28–34` · `app/docs/BACKLOG.md:823,1469,1481,2529` · `app/core/espelho_verdade.py:76–78`

## E · Proveniência dos dois zeros

- **col302 20/11.** A classe publicada exige piso E6 exatamente zero e espelho positivo. O sítio do zero é `e6.get(k, 0.0)` sobre a soma posicional. O zero pode vir da chave ausente ou de uma soma sem minutos positivos. O produtor do positivo, a chave dos trechos vizinhos e eventual correspondência com os casos de `MOTOR-LE-O-TURNO` estão **[nao verificado]**. `depois_c11.txt:34` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/management/commands/e6_oraculo.py:350–372` · `app/docs/BACKLOG.md:2234`
- **col996 28/10.** O sítio de seleção do zero é `minutos_do_espelho`. A primeira origem possível é o `RealizadoDoDia.minutos` numérico, por exemplo quando há turno do dia e a soma fecha em zero. Sem turno do dia, o realizado devolve `None`, e o leitor passa ao `pago_h`. Para esse segundo caminho participar do R1, o pago precisa ser numérico. Linha de `DiaPago` ausente ou com todas as rubricas de dinheiro zeradas devolve `None`. A origem efetiva do zero está **[nao verificado]**. `depois_c11.txt:47` · `app/ponto/turnos.py:719–721,733–771` · `app/ponto/services/dia_pago.py:890–898` · `app/ponto/management/commands/e6_oraculo.py:263–269,342–347`

**Escritores, leitores, leis e padrão.** O censo dos campos é o dos caminhos comuns. Em particular, `DiaPago.lavrar` é a porta de escrita das rubricas de trabalho, `linhas_do_periodo` e `pago_do_dia` as leem para o espelho, e PDF e relatóri

[truncado em 20000 de 28032 caracteres]

## 26/11 10:47 · sessao 3af52882fa76 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

Implementei a F3 na cópia `a4d3675c7a187493c006eb291ee40580a3ea7a77`. Os cinco REDs falharam na base por asserção. Na cura, os oito testes em memória passaram, incluindo troca de vínculo, retratação e mudança de competência. Os três mutantes voltaram a falhar por asserção. A certificação fica com o Code e o opus final. `ponto/tests/test_juiz_unico_eventos.py:61`

## Cura

- `turnos_do_colab`, `dia_das_batidas` e `CargaDaJanela` usam o carregador histórico da F2. O mapa inclui as PKs de cada turno apresentado, inclusive a saída de 25/10 quando a janela termina em 23/10. Se dois segmentos reivindicam a mesma PK em dias diferentes, vale a precedência que o mapa F2 já aplicava. A publicação de turnos respeita essa atribuição. `ponto/turnos.py:1850` `ponto/turnos.py:2891` `ponto/turnos.py:2941` `ponto/turnos.py:3007` `ponto/turnos.py:3119`
- O juiz compara a geometria antes e depois do evento. O núcleo reaproveita os turnos julgados, e o cartório e o fechamento reaproveitam os dias afetados pelo `save`. A exclusão reintroduz a marca em memória para julgar a versão anterior. O fechamento usa o corte da empresa e chama o recálculo uma vez por competência afetada, após o commit. `ponto/turnos.py:3025` `ponto/signals.py:20` `ponto/signals.py:30` `ponto/signals.py:124` `ponto/nucleo.py:40` `ponto/registro_batida.py:195` `ponto/registro_batida.py:303` `ponto/services/fechamento.py:1831`
- A contagem AST ficou em **zero `except` novos**, incluindo testes. No chokepoint, o diff se limita às chamadas e à preparação do agendamento. Os selos permitidos foram reancorados na carga histórica. `ponto/registro_batida.py:174` `ponto/registro_batida.py:195` `ponto/registro_batida.py:303` `ponto/tests/test_contract_no_batida_date.py:447` `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:178` `ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:195` `chamados/tests/test_e3_c8b_sem_banco.py:522` `chamados/tests/test_e3_c8c_sem_banco.py:104`

## RED → GREEN

| Caso | Saída na base | Resultado na cura |
|---|---|---|
| R4 · par de 24h10 | `Items in the second set but not the first: datetime.date(2026, 10, 23)` | A saída aparece no turno de 23/10 em `[23/10, 23/10]` e na competência 10. O mapa e o lote concordam. `ponto/tests/test_juiz_unico_eventos.py:137` |
| R4 · saídas órfãs | `datetime.date(2026, 10, 24) != datetime.date(2026, 10, 25) : PK 6` | A PK 6 recebe 25/10 em `[25/10, 25/10]`, `[24/10, 26/10]` e na competência 11. `ponto/tests/test_juiz_unico_eventos.py:156` |
| R5a · `post_save` | Cartório chamou `[24/10, 25/10]`. O núcleo iniciou em `23/10`, com âncora civil. | O cartório julga 23/10 e o núcleo ancora a recomputação na jornada de 23/10. `ponto/tests/test_juiz_unico_eventos.py:172` |
| R5a · `post_delete` | As mesmas duas asserções falharam após remover a saída. | O julgamento anterior à exclusão recupera a jornada de 23/10. `ponto/tests/test_juiz_unico_eventos.py:178` |
| R5b · corte em 24/10 | `Expected: recalcular_fechamento_mes(10, 2026, ...)` / `Actual: recalcular_fechamento_mes(11, 2026, ...)` | Agenda 10/2026. O caso adicional com mudança de dono agenda 10 e 11, e a retratação agenda a competência anterior. `ponto/tests/test_juiz_unico_eventos.py:196` `ponto/tests/test_juiz_unico_eventos.py:202` `ponto/tests/test_juiz_unico_eventos.py:211` |

O comando da cura retornou `Ran 8 tests` e `OK`. Os dublês cobrem ORM, despacho, gravação do núcleo e execução agendada do fechamento. `ponto/tests/test_juiz_unico_eventos.py:61`

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest -v ponto.tests.test_juiz_unico_eventos
```

## Mutantes

`f3 base` retornou `rc=1`, com 10 falhas nos cinco casos. `f3 recorte` retornou `rc=1`, com cinco falhas de R4. `f3 signal` retornou `rc=1`, com quatro falhas de R5a. `f3 evento` retornou `rc=1`, com duas falhas de R5b. O comando aplica cada substituição somente em memória. `ponto/tests/test_juiz_unico_eventos.py:153` `ponto/tests/test_juiz_unico_eventos.py:172` `ponto/tests/test_juiz_unico_eventos.py:196`

```sh
f3() {
  F3_MODO="$1" PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast
import os
import subprocess
import unittest
from importlib.machinery import SourceFileLoader
from pathlib import Path
from unittest.mock import patch

modo = os.environ["F3_MODO"]
raiz = Path.cwd()
base_hash = "a4d3675c7a187493c006eb291ee40580a3ea7a77"
turnos = "ponto/turnos.py"
signal = "ponto/signals.py"
registro = "ponto/registro_batida.py"
fechamento = "ponto/services/fechamento.py"

def base(arquivo):
    return subprocess.check_output(["git", "show", base_hash + ":app/" + arquivo], text=True)

def trocar(fonte, antes, depois):
    assert fonte.count(antes) == 1, antes
    return fonte.replace(antes, depois, 1)

def casca(fonte):
    no = next(n for n in ast.parse(fonte).body if getattr(n, "name", "") == "turnos_do_colab")
    return ast.get_source_segment(fonte, no)

r4 = ["r4_par_24h10", "r4_saidas_orfas"]
r5a = ["r5a_save", "r5a_delete"]
if modo == "base":
    fontes = {f: base(f) for f in (turnos, signal, registro, fechamento)}
    casos = r4 + r5a + ["r5b_evento"]
elif modo == "recorte":
    s = Path(turnos).read_text()
    fontes = {turnos: trocar(s, casca(s), casca(base(turnos)))}
    casos = r4
elif modo == "signal":
    s = Path(signal).read_text()
    s = trocar(s, "        if dias:\n            recompute_turnos",
               "        from ponto.turnos import data_local\n"
               "        dias = (data_local(batida.timestamp),)\n"
               "        if dias:\n            recompute_turnos")
    s = trocar(s, "    ontem = datetime.timedelta(days=1)",
               "    from ponto.turnos import data_local\n"
               "    dias = (data_local(batida.timestamp),)\n"
               "    ontem = datetime.timedelta(days=1)")
    fontes, casos = {signal: s}, r5a
else:
    assert modo == "evento"
    s = Path(registro).read_text()
    fontes = {registro: trocar(s, "dias_da_jornada=dias_do_salvamento(batida, criada=criada)",
                               "dias_da_jornada=None")}
    casos = ["r5b_evento", "r5b_mudanca_do_dono_agenda_duas_competencias"]

fontes = {raiz / f: s.encode() for f, s in fontes.items()}
ler_real = SourceFileLoader.get_data

def ler(loader, caminho):
    p = Path(caminho)
    return fontes[p] if p in fontes else ler_real(loader, caminho)

prefixo = "ponto.tests.test_juiz_unico_eventos.JuizUnicoEventosTest.test_"
with patch.object(SourceFileLoader, "get_data", ler):
    suite = unittest.defaultTestLoader.loadTestsFromNames([prefixo + c for c in casos]) [nao verificado]
    raise SystemExit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}

f3 base
f3 recorte
f3 signal
f3 evento
```

## Custo e catraca

- **Por chamada singular**, a estimativa estática é **4–5 consultas** com marcas na janela e **1** quando o pad está vazio. A leitura de marcas percorre o histórico apurável. Em lote, turnos e mapa dividem **3–4 consultas históricas** e reutilizam o vínculo da ata já lido. Chamadas singulares independentes abrem cargas independentes. `ponto/turnos.py:2953` `ponto/turnos.py:2987` `ponto/turnos.py:3070` `ponto/turnos.py:3119` `escala/alimentacao.py:148`
- **Por `save`**, o caminho normal dos receivers faz uma carga histórica compartilhada. Criação, retratação e edição de insumo podem executar dois pareamentos sobre essa carga. Edição de insumo em PK existente, ou `save` completo dessa PK, acrescenta **1 consulta** para ler a versão anterior. O núcleo reaproveita a geometria. Cada dia adicional julgado pode aumentar o custo do cartório. SQL efetivo, latência e teto do chokepoint ficam **[nao verificado]**. `ponto/signals.py:20` `ponto/signals.py:30` `ponto/signals.py:124` `ponto/turnos.py:3025` `ponto/nucleo.py:40` `ponto/tests/test_selo_chokepoint_escrita.py:343`
- **Catraca protegida.** O censo passou de **81 → 78 derivações**, com **9 pareadores**. B1 passou de **78 → 75**. `_recompute_around:23` e `_dias_batida:111` saem. `_carregar_dias_das_batidas:3026 → 2979` entra na composição do juiz e sai de B1. `dia_do_turno_da_batida:2989 → 2929` e `par_completo:2217 → 2156` continuam em B1. A composição ganha `_carregar_dias_das_batidas`, `_dia_das_batidas_puro`, `batidas_apuraveis` e `marcos_dna_periodo`, e perde `escalas_do_periodo`. O registro congelado ainda acusa o delta. `core/tests/test_medida_turno_do_par.py:108` `core/tests/test_medida_turno_do_par.py:110` `core/tests/test_medida_turno_do_par.py:336` `core/tests/test_medida_turno_do_par.py:508` `ponto/turnos.py:2979`

## Impacto nos 51 sítios

Há mudança observada na resposta da autoridade que esses sítios consultam. O espelho usa essa resposta para as datas de saídas órfãs, a API usa para agrupar turnos por dia, e o núcleo usa para materializar `data_turno`. A mudança efetiva de cada saída consumidora nas competências 10 e 11 está **[nao verificado]**. `ponto/tests/test_juiz_unico_eventos.py:137` `ponto/services/espelho.py:628` `api/views.py:1447` `ponto/nucleo.py:47`

A varredura encontrou 50 chamadas diretas ou por alias, além do registro `_T1`. Sítios expostos à mudança de resposta:

- **API e autoridade** — `api/views.py:1447`, `core/juizes.py:216`.
- **Chamados** — `chamados/detector_anomalias.py:157`, `chamados/juizes.py:447,2303`, `chamados/models.py:1378`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:145`, `chamados/services/disputa_emissao.py:722`, `chamados/services/materializacao.py:399,513`.
- **Colaboradores e escala** — `colaboradores/services/drawer.py:47`, `colaboradores/services/feriado_do_posto.py:473`, `escala/detector_proposta.py:65`, `escala/regua_defesa.py:361`, `escala/services/escala_auto.py:87`, `escala/services/impacto_troca.py:18`, `escala/servico_jornada.py:249,403`, `escala/utils.py:1627,1952`.
- **Ponto** — `ponto/nucleo.py:48`, `ponto/turno_leitura.py:89,95`, `ponto/turnos.py:780,2043,2101,2131,3185`, `ponto/views.py:396,1613,3356`, `ponto/services/bordas_realizado.py:66`, `ponto/services/esmeril_espelho.py:102`, `ponto/services/espelho.py:628`, `ponto/services/passivo_s84.py:82`, `ponto/services/regularizacao.py:61`, `ponto/services/teto_na_lista.py:68`, `ponto/services/triagem_batida.py:553`, `ponto/services/vigia_pareador_celula.py:97`.
- **Comandos e relatórios** — `ponto/management/commands/censo_fase_12x36.py:180`, `ponto/management/commands/detectar_ausencias.py:337`, `ponto/management/commands/detectar_par_relampago.py:101`, `ponto/management/commands/dna_x_batida_real.py:69`, `ponto/management/commands/e6_oraculo.py:159`, `ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `ponto/management/commands/marcar_abandono_provisorio.py:46`, `ponto/management/commands/verificar_nucleo.py:29`, `ponto/management/commands/vigia_plantio_overnight.py:38`, `relatorios/management/commands/vinculo_do_dia_divergentes.py:124`, `relatorios/pdf_espelho.py:213,451`.

## Sonda de sombra para o Code

Executar o texto no shell Django do envelope de sombra com `PYTHONDONTWRITEBYTECODE=1`. A sonda chama as duas versões reais de `turnos_do_colab`, usa o corte de cada empresa e captura em memória o lote que `recompute_turnos` entregaria ao ORM. A comparação inclui as linhas já gravadas na sombra e usa multiplicidade. A sintaxe foi conferida. Resultado da execução na sombra **[nao verificado]**. `ponto/janelas.py:55` `ponto/turnos.py:1850` `ponto/nucleo.py:40` `ponto/models.py:1208`

```python
from collections import Counter, defaultdict
import json
import subprocess
from unittest.mock import patch

from colaboradores.models import Colaborador
from ponto import nucleo, turnos
from ponto.janelas import janela_fechamento
from ponto.models import TurnoMaterializado

hash_base = "a4d3675c7a187493c006eb291ee40580a3ea7a77"
fonte = subprocess.check_output(["git", "show", hash_base + ":app/ponto/turnos.py"])
base = {"__name__": "ponto.turnos_base_f3", "__package__": "ponto"}
exec(compile(fonte, "ponto/turnos.py@" + hash_base, "exec"), base)
campos = ("data_turno", "entrada_id", "saida_id", "aberto",
          "cross_meianoite", "dt_fim_previsto")

def foto(juiz, colab, ini, fim):
    pares = juiz(colab, ini, fim)
    dias = defaultdict(set)
    for turno in pares:
        for batida in turno.batidas:
            dias[batida.pk].add(turno.data_turno)
    rows = []
    with patch.object(nucleo, "_apagar_recorte_devolvendo_abertos", return_value=set()), \
         patch.object(TurnoMaterializado.objects, "bulk_create", side_effect=rows.extend):
        nucleo.recompute_turnos(colab, ini, fim, turnos=pares)
    return dias, Counter(tuple(getattr(row, f) for f in campos) for row in rows)

def emitir(**linha):
    print(json.dumps(linha, default=str, sort_keys=True))

def contagem(linhas):
    return [{"dados": dados, "n": n} for dados, n in sorted(linhas.items(), key=str)]

totais = {m: Counter() for m in (10, 11)}
for c in Colaborador.objects.select_related("empresa").order_by("pk").iterator(chunk_size=200):
    fotos, dias_base, dias_cura = {}, defaultdict(set), defaultdict(set)
    for m in totais:
        ini, fim = janela_fechamento(m, 2026, c.empresa)
        pk_base, mat_base = foto(base["turnos_do_colab"], c, ini, fim)
        pk_cura, mat_cura = foto(turnos.turnos_do_colab, c, ini, fim)
        for pk in pk_base:
            dias_base[pk].update(pk_base[pk])
        for pk in pk_cura:
            dias_cura[pk].update(pk_cura[pk])
        fotos[m] = ini, fim, pk_base, pk_cura, mat_base, mat_cura

    for m, (ini, fim, pk_base, pk_cura, mat_base, mat_cura) in fotos.items():
        for pk in sorted(pk_base.keys() | pk_cura.keys()):
            if dias_base[pk] != dias_cura[pk]:
                totais[m]["pks"] += 1
                emitir(comp=m, colab=c.pk, pk=pk, base=sorted(dias_base[pk]),
                       cura=sorted(dias_cura[pk]))
        gravado = Counter(TurnoMaterializado.objects.filter(
            colaborador=c, data_turno__range=(ini, fim)).values_list(*campos))
        if mat_base != mat_cura or gravado != mat_cura:
            saiu, entrou = mat_base - mat_cura, mat_cura - mat_base
            totais[m].update(saidas_base_cura=sum(saiu.values()),
                             entradas_base_cura=sum(entrou.values()))
            emitir(comp=m, colab=c.pk, tipo="materializacao", sai=contagem(saiu),
                   entra=contagem(entrou), sai_do_gravado=contagem(gravado - mat_cura),
                   entra_no_gravado=contagem(mat_cura - gravado))

for m, total in totais.items():
    emitir(comp=m, total=dict(total))
```

**Fora da F3.** O C6 de chamados ainda chama o corpo puro com seu recorte próprio. O mapa interno do motor mensal segue em sua porta. A suíte Django com banco, os valores lavrados em produção e o comportamento sob concorrência estão **[nao verificado]**. `chamados/services/carga_pergunta_viva.py:165` `ponto/motor_calculo_v2.py:867` `ponto/services/fechamento.py:1241`

## 26/11 10:46 · sessao 78751ae7cf11 · ORDEM-PELO-R1 · leitura · origem=Code

**`MOTOR-LE-O-TURNO` tem o maior escopo nominal, com 12 dos 38 dias ESTRUTURA da comp. 11.** O mapa fecha em **19 chaves com uma obra identificada, 1 chave com duas candidatas e 18 SEM DONO**. Quantos dias cada obra efetivamente zera no R1 de 26/11 está **[nao verificado]**. O E6 pode deixar de listar uma chave por acordo de minutos, mudança de dono ou saída da comparação. `depois_c11.txt:12–49` [BACKLOG.md:2234](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2234) [e6_oraculo.py:331](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:331)

## 1. Dia → obra e prova

Os caminhos `depois_c11.txt` são as referências do PUBLICADO fornecido. A fonte escolhida pelo E6 para o valor do espelho em cada chave está **[nao verificado]**. O comando prioriza `minutos_realizados`, usa `pago_h` quando o primeiro campo é nulo e publica a classe a partir dos valores comparados. `app/ponto/management/commands/e6_oraculo.py:263–269,342–372,401–404,450–451`

**Provas de código usadas na tabela**

- **Chave do turno.** O BACKLOG enumera “11 dia-colab em que oraculo E pareador dao [] e o espelho tem 103..418 min” e registra separadamente “col841 2026-11-16”. O E6 chama `minutos_do_oraculo(ts)` sem pares e agrupa pela data civil do primeiro instante do trecho. O espelho carrega `turnos_do_colab` e soma pela `data_turno`. A leitura r4 localizou esses caminhos e deixou a causa individual dos 11 **[nao verificado]**. `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:2198,2203,13103–13114` `app/ponto/management/commands/e6_oraculo.py:149–154,350` `app/ponto/calculador/nucleo.py:235–247` `app/ponto/services/espelho.py:625–628,689–690,801` `app/ponto/turnos.py:719–721,733–771`
- **Pausa curta.** A linha **`EV-NA-BRECHA-1611`** registra **`ORACULO-E6-43-DEFEITOS`** na GRADE. O RELATO nomeia “pausa abaixo do piso com intervalo pre-assinalado L-032” para col252 e dá as cinco datas. O E6 soma os pares por posição e deixa o vão entre pares fora da soma. O realizado do espelho exige 15 minutos para reconhecer a pausa batida e, na ausência de pausa reconhecida, desconta a janela fixa declarada quando ela se aplica. O E6 ainda tem uma guarda para excluir a diferença explicada pela janela declarada. O valor dessa guarda por chave na rodada de 26/11 está **[nao verificado]**. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:2355,2390,3096` `app/ponto/calculador/nucleo.py:243–253` `app/ponto/turnos.py:499,542–546,756–771` `app/ponto/management/commands/e6_oraculo.py:236–239,350–358`
- **Janela de HE.** A linha **`NOTURNO-A-JANELA-NO-DIA-DO-TURNO`** diz “janela de HE ancora no dia do turno”. O motor ancora os marcos nesse dia e aplica a L-084 às pontas cruas. O RELATO documenta os deltas dos pares de col922 e col1010. O efeito sobre a fonte do valor ou a transição de dono no R1 está **[nao verificado]**. `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:205` `app/ponto/motor_calculo_v2.py:1543–1588` `app/ponto/janela_he.py:41–61` `app/ponto/management/commands/e6_oraculo.py:162–172,182–195,263–269`

| Dia de 2026 | Classe do PUBLICADO | Obra | Prova | [nao verificado]
|---|---|---|---|
| col37 14/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. Caso nominal. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13114` |
| col110 12/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. Caso nominal. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13111` |
| col113 30/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também tem teste nominal da janela do par na madrugada de 31/10. O efeito desse teste sobre o zero do E6 está **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13112` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:61–69` |
| col113 01/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. O vínculo dessa medição com o zero do E6 está **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13113,13200–13207` |
| col200 29/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. O mapa de DiaPago também registra folga trabalhada gravada em D+1. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13103,13204–13207` `app/ponto/motor_calculo_v2.py:2416–2424` |
| col200 02/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13104,13200–13207` |
| col218 14/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13105,13200–13207` |
| col296 02/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13106,13200–13207` |
| col302 20/11 | `e6_zero_esp_trabalho` | **SEM DONO** | Classe publicada. Causa e obra por chave **[nao verificado]**. `depois_c11.txt:34` `app/ponto/management/commands/e6_oraculo.py:350–372` |
| col382 10/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também tem teste nominal da janela do par civil de 11/11, atribuído a 10/11. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13107` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` |
| col516 23/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A sonda anterior registra saída órfã às 07:01. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13108` |
| col727 10/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. O mapa de DiaPago também registra folga trabalhada gravada em D+1. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13109,13204–13207` `app/ponto/motor_calculo_v2.py:2416–2424` |
| col841 16/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Caso nominal da chave do dia. O pareador cru deu BATIDA na sonda v3. A v3b com `turnos_do_colab` manteve ESTRUTURA. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:2157,2203,13110` `app/ponto/management/commands/e6_oraculo.py:98–110,182–195` |
| col996 28/10 | `esp_zero_e6_trabalho` | **SEM DONO** | `O54`, etapa `E1-sem-previsao`, acompanha o contador. A causa do zero nesta chave está **[nao verificado]**. `depois_c11.txt:47` `app/docs/BACKLOG.md:1469,1303` `app/core/espelho_verdade.py:60–61` `app/ponto/management/commands/e6_oraculo.py:263–269,370–372` |
| col114 08/11 | `diverge_10_60` | **SEM DONO** | Citado entre os dias de intervalo na sonda anterior. Mecanismo específico e obra **[nao verificado]**. `depois_c11.txt:18–19` `app/docs/RELATO.md:13118–13119` |
| col114 15/11 | `diverge_10_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:18–19` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col252 09/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 10/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 11/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 13/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 17/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 18/11 | `diverge_10_60` | **SEM DONO** | Pausa curta nesta data **[nao verificado]**. `depois_c11.txt:26–32` `app/docs/RELATO.md:2390,3096` |
| col252 19/11 | `diverge_10_60` | **SEM DONO** | Pausa curta nesta data **[nao verificado]**. `depois_c11.txt:26–32` `app/docs/RELATO.md:2390,3096` |
| col884 08/11 | `diverge_10_60` | **SEM DONO** | A chave aparece em “sem nome” e no recorte de intervalo. Causa e obra **[nao verificado]**. `depois_c11.txt:41` `app/docs/RELATO.md:3097,13118–13119` |
| col112 21/10 | `diverge_acima_60` | **SEM DONO** | A sonda registra pares diferentes entre E6 e pareador cru na chave. A inclusão nominal na família de madrugada do oráculo está **[nao verificado]**. `depois_c11.txt:14–15` `app/docs/RELATO.md:13115–13117` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:235–247` |
| col112 22/10 | `diverge_acima_60` | **SEM DONO** | A sonda registra pares diferentes entre E6 e pareador cru na chave. A inclusão nominal na família de madrugada do oráculo está **[nao verificado]**. `depois_c11.txt:14–15` `app/docs/RELATO.md:13115–13117` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:235–247` |
| col130 29/10 | `diverge_acima_60` | **SEM DONO** | A leitura anterior deixou col130 fora do envelope. Causa e obra **[nao verificado]**. `depois_c11.txt:20` `app/docs/RELATO.md:2926` |
| col200 03/11 | `diverge_acima_60` | **SEM DONO** | A NOTURNO-A documenta um par civil de 03/11 cujo turno é de 02/11. Há também uma colisão nominal de batidas às 02:00. O nexo de qualquer um dos achados, ou da relavra, com o delta de 03/11 está **[nao verificado]**. `depois_c11.txt:23` `app/docs/RELATO.md:189,2360` `app/docs/BACKLOG.md:2304` `app/ponto/motor_calculo_v2.py:936–951` `app/ponto/registro_batida.py:118–127` |
| col220 10/11 | `diverge_acima_60` | **SEM DONO** | A leitura anterior deixou col220 fora do envelope. Causa e obra **[nao verificado]**. `depois_c11.txt:25` `app/docs/RELATO.md:2926` |
| col382 11/11 | `diverge_acima_60` | **SEM DONO** | O teste da NOTURNO-A atribui o par civil de 11/11 ao turno de 10/11. `P0-[nome]-RELAVRA-10` cita o colaborador. Efeito sobre a chave R1 de 11/11 **[nao verificado]**. `depois_c11.txt:36` `app/docs/BACKLOG.md:2303–2304` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` |
| col874 22/10 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:40` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col922 29/10 | `diverge_acima_60` | **Duas candidatas** `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` e `MOTOR-LE-O-TURNO` | **NOTURNO-A** cobre o par cru liberado pela L-084, `+174,6` min no motor frente a `−175` no R1. **MOTOR-LE-O-TURNO** cobre o mapa de dia, com 29/10 no juiz e 27/10 no resultado do motor para o par documentado. A contribuição de cada caminho ao delta R1 está **[nao verificado]**. `depois_c11.txt:43` `app/docs/BACKLOG.md:1989,1996,2234` `app/docs/RELATO.md:194,205,290` `app/ponto/motor_calculo_v2.py:1543–1588,2416–2424` `app/ponto/services/dia_pago.py:126–132` |
| col922 01/11 | `diverge_acima_60` | **SEM DONO**. Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611` | “realizado soma turno que a tela nao exibe”, registrado **a confirmar**. O código soma turnos do juiz e monta os cartões por outro caminho, a partir dos períodos do motor. Nexo com o delta R1 **[nao verificado]**. `depois_c11.txt:43–46` `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:435–436,464–474,689–690,801` `app/templates/ponto/espelho.html:194` |
| col922 04/11 | `diverge_acima_60` | **SEM DONO**. Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611` | Mesmo diagnóstico nominal de soma e exibição, **a confirmar**. Nexo com o delta R1 **[nao verificado]**. `depois_c11.txt:43–46` `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:435–436,464–474,689–690,801` `app/templates/ponto/espelho.html:194` |
| col989 23/10 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:43–46` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col996 10/11 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:48–49` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col1010 05/11 | `diverge_acima_60` | `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` | Janela de HE. O teste nominal põe a saída prevista em D+1 e conta cru o par da madrugada. O DIFF mede `+294,8` min no motor e o R1 mostra `−295`. `depois_c11.txt:49` `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:205` `app/ponto/janela_he.py:41–61` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:93–107` |
| col909 11/11 | `diverge_acima_60/FORA` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Família “corte por vao”. O anexo traz `CORTE_do_oraculo` e `−586` min. O núcleo marca o corte que deixa um trecho ímpar e soma só os pares fechados. A chave também consta na lista BATIDA de `HJ-092`. O nexo desse segundo registro com o delta, e o delta após corrigir o corte, estão **[nao verificado]**. `depois_c11.txt:42` `app/docs/BACKLOG.md:797,823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:3936` `app/ponto/calculador/nucleo.py:215–225,236–247` `app/ponto/management/commands/e6_oraculo.py:393–404` |

Nos cinco dias de col252, `[nome]-NAO-DESFAZ-O-PAPEL` e `[nome]-NA-IMPRESSAO` também aparecem no registro da **guarda da rajada e do marco vazio na ata**. O RELATO deixa o resíduo D1 em pergunta de desenho. O efeito dessas obras no delta do E6 está **[nao verificado]**. `app/docs/BACKLOG.md:2538,2558,2560` `app/docs/RELATO.md:2355,2390`

## 2. Por obra, estado e pré-requisitos

| ID exato | Dias ESTRUTURA no escopo e dias que zera | Estado e pré-requisitos registrados |
|---|---|---|
| `MOTOR-LE-O-TURNO` | **12 nominais** da classe `e6_zero_esp_trabalho`. Também candidata para a chave compartilhada col922 29/10. Zera **[nao verificado]**. `app/docs/BACKLOG.md:1996,2234` `app/docs/RELATO.md:2157,2198` | **Registrada**, fila 1 após E2 pousar com E3. O instrumento anterior à Fatia 1 consta como pousado. A ordem de código é Fatia 1 **PAR**, depois Fatia 2 **DIA**, com motor e `DiaPago` lendo o par e o dia da autoridade do turno. O BACKLOG exige RED pela regra antes, DIFF 10/11 por dono e Pauta DP para exportada. `app/docs/BACKLOG.md:2234–2235` `app/docs/GRADE-MESTRA.md:105` `app/core/juizes.py:748` `app/ponto/motor_calculo_v2.py:867–884,2416–2424` `app/ponto/services/dia_pago.py:109–132` |
| `ORACULO-E6-43-DEFEITOS` | **6 cobertos pela família documentada**, cinco de pausa de col252 e o corte de col909. Zera **[nao verificado]**. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:2390,3096` `depois_c11.txt:42` | **Registrada** na GRADE como instrumento. O ID da linha que a registra no BACKLOG é `EV-NA-BRECHA-1611`. O pré-requisito declarado é `CONTRATO-DE-INSTRUMENTO` quando chegar a vez. Os pontos de cálculo são o agrupamento e a soma posicional para a pausa, e o corte por vão para col909. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:215–225,235–253` |
| `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` | **1 chave exclusiva do mecanismo**, col1010 05/11, e **1 compartilhada**, col922 29/10. Zera **[nao verificado]**. `depois_c11.txt:43,49` `app/docs/RELATO.md:194,205` | **No ar** conforme BACKLOG. O escopo autorizado é a âncora da janela na comp. 11 em diante. O cálculo usa o mapa do juiz, o marco de saída em D+1 quando aplicável e a L-084 nas pontas cruas. O BACKLOG registra dependência de `MOTOR-LE-O-TURNO` para B-I28/v0815_7. A atualização do gravado depende de relavra por evento ou botão. `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:143,270` `app/ponto/motor_calculo_v2.py:936–951,1543–1588` `app/ponto/janela_he.py:41–61` |
| `EV-NA-BRECHA-1611` e `O54` | **2 candidaturas sem dono confirmado**, col922 01/11 e 04/11. `EV-NA-BRECHA-1611` é a leitura da soma e da exibição. `O54`, etapa `E3-geometria`, é a fatia apontada pelo diagnóstico. Zera **[nao verificado]**. `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:3097` | **EV registrada como leitura**, com i1/i2/i3b aguardando `todos.fim` da PREPARA na linha. **O54 em curso** na linha do programa, com portas sequenciais. O registro de código marca a entrega histórica de `E3-geometria` como feita pela `O65`. A pendência nominal é confirmar, nas duas chaves, o turno que entra na soma e qual período chega ao cartão. `app/docs/BACKLOG.md:823,1469,1481` `app/core/espelho_verdade.py:67–78` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:464–474,689–690,801` |
| `R1-SEM-DONO-2611` | **18 chaves da comp. 11 neste mapa**, agrupadas abaixo. Zera **[nao verificado]**. `depois_c11.txt:12–49` | **Registrada**, investigação por família, sem vaga, conforme o BACKLOG fornecido e o PUBLICADO §5. Linha desse ID na cópia **[nao verificado]**. Pré-requisito de código por família **[nao verificado]**. | [nao verificado]

**Pré-requisitos da vaga.** O anexo BACKLOG põe a ordem após o voo e os pré-requisitos de E4d. A linha local de `ORDEM-PELO-R1-2611` está **[nao verificado]**. Na cópia, `E4C-F5-INSTANTE` e `PORTA-CONFERE-TRAVA` estão registrados antes da E4d, e `E4D-CONTRATO-ROTA-B` registra a matriz respondida e o brief como próximo passo. `app/docs/BACKLOG.md:830,1985,2155` `app/docs/RELATO.md:98,13237`

## 3. SEM DONO por classe

- **`e6_zero_esp_trabalho` — 1.** col302 20/11. `depois_c11.txt:34` `app/docs/BACKLOG.md:2234`
- **`esp_zero_e6_trabalho` — 1.** col996 28/10. `depois_c11.txt:47` `app/core/espelho_verdade.py:60–61`
- **`diverge_10_60` — 5.** col114 08/11 e 15/11, col252 18/11 e 19/11, col884 08/11. `depois_c11.txt:18–19,26–32,41` `app/docs/RELATO.md:2390,3097,13118`
- **`diverge_acima_60` — 11.** col112 21/10 e 22/10, col130 29/10, col200 03/11, col220 10/11, col382 11/11, col874 22/10, col922 01/11 e 04/11, col989 23/10, col996 10/11. `depois_c11.txt:14–15,20,23,25,36,40,43–46,48–49` `app/docs/RELATO.md:189,2926,3097,13115–13117`
- **`diverge_acima_60/FORA` — 0 SEM DONO.** A única chave, col909 11/11, tem o corte documentado. `depois_c11.txt:42` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:215–225`

## 26/11 10:42 · sessao 16453a22ec76 · ABONO-X-SAIDA-ANTECIPADA · leitura · origem=Code

## 1. Qual valor entra como parcela paga da `Ausencia` parcial?

**LEI-NOVA.** As fontes fixam os minutos declarados como crédito do atraso de 17/09, o período declarado como critério de cobertura e o déficit da linha `ausencia` como abono projetado pela grade. Para o **teto líquido da obra**, falta a regra que escolhe entre esses valores ou exige coincidência com a ponta coberta. A escolha muda o valor do desconto e fica fora de **DEFINIÇÃO TÉCNICA** de chave, aceite ou encaixe. A chave do dia de início da jornada já está decidida pela L-085 e pelo corte da parcial noturna. `r.md:54,72` (anexo) · `app/ponto/services/credito_parcial.py:1–5,15–29` · `app/ponto/services/dia_pago.py:372–396` · `app/docs/CORTES.md:462,473` · `app/docs/LEIS.md:117` · `CLAUDE.md:285–291`

## 2. Como o crédito exclusivo do atraso convive com o teto líquido?

**LEI-NOVA.** A L-093 manda preservar a proporção quando seu teto reduz a soma. O corte de 17/09 manda abater o atraso do próprio dia. Falta a precedência entre as duas operações e a regra de como cumprir o abatimento quando o teto líquido já comporta toda a pontualidade. A escolha muda as rubricas e fica fora de **DEFINIÇÃO TÉCNICA** de chave, aceite ou encaixe. `r.md:73` (anexo) · `app/docs/LEIS.md:111` · `app/ponto/services/credito_parcial.py:1–5,50–61` · `CLAUDE.md:285–291`

**Conta literal de `CORTES.md:558`.** “a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo”. Com `P−T=120`, a frase manda a **Justificativa aprovada pagar 120 min**. O `B=60` dado é o valor de crédito parcial do exemplo. Usando esse `B` na conta do dossiê, sobram `120−60=60 min` no total. **A linha 558 sozinha fixa nenhum dos dois rateios, nem `15/45` nem `0/60`.** `app/docs/CORTES.md:558` · `app/docs/PROMPTS.md:2312` · `r.md:54,73` (anexo)

| Opção do dossiê | Conta em minutos de atraso / saída antecipada |
|---|---|
| Só teto líquido proporcional | `A+S=120`, fator `60/120=0,5`, resultado **15/45**. `r.md:54,73` (anexo) · `app/docs/LEIS.md:111` |
| Crédito primeiro no atraso | `min(30,60)=30` abate todo o atraso. Os `30` restantes cobrem parte dos `90` de saída, resultado **0/60**. `r.md:73` (anexo) · `app/ponto/services/credito_parcial.py:50–61` |
| Caminho atual da cópia, com o crédito na chave do próprio dia | O teto bruto é `120` e mantém `30/90`. O abatimento posterior resulta em **0/90** no fechamento e no DiaPago `motor`. `app/ponto/motor_calculo_v2.py:1187–1213` · `app/ponto/services/fechamento.py:1013–1026` · `app/ponto/services/dia_pago.py:173–195` |

## Leis e cortes candidatos, por pergunta

Na cópia, `CORTES.md:462` e `:473` são de **27/10**. O corte **JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO** é de **14/11**, na linha `:558`. O texto do crédito de **17/09** está registrado na docstring do serviço e identificado pelo mesmo nome em `TICKETS.md`. `app/docs/CORTES.md:462,473,558` · `app/ponto/services/credito_parcial.py:1–10` · `app/docs/TICKETS.md:1108`

| Candidata e trecho literal | 1. Valor da parcela paga | 2. Precedência e rateio |
|---|---|---|
| **L-093** — “`atraso + saida antecipada` do DIA nunca passam de `(previsto do dia - trabalhado do dia)`” e “se mantem a PROPORCAO em que elas estavam”. `app/docs/LEIS.md:111` | **não responde porque** define o teto por previsto e trabalhado, sem escolher `B`. | **não responde porque** fixa a proporção da redução pelo teto e deixa aberta a ordem em relação ao crédito. No exemplo, seu teto literal de `120` já comporta `30+90`. |
| **L-084** — “entrada **E** saida, as DUAS, a mais de **3 h** dos seus marcos = o TURNO INTEIRO esta em outro horario” e “UMA ponta longe desconta normalmente”. `app/docs/LEIS.md:116` | **não responde porque** decide quando os marcos descrevem o dia para julgar pontualidade. | **não responde porque** a distância aos marcos decide a incidência do julgamento, sem ordenar crédito e teto. |
| **L-085** — “A JORNADA PERTENCE AO DIA DE INICIO” e “hora depois da meia-noite herda o dia em que o plantao COMECOU”. `app/docs/LEIS.md:117` | **não responde porque** fixa a chave da jornada e deixa o valor de `B` em aberto. | **não responde porque** fixa o dia da conta e deixa a ordem dentro do dia em aberto. |
| **L-006 e L-024** — “regra que varia por cliente/praca/empresa nasce como cadastro pela UI, com nome e leitor” e “A GRADE E FONTE UNICA DE PREVISTO”. `app/docs/LEIS.md:64,75` | **não responde porque** disciplinam o cadastro e a fonte de `P`, sem escolher a fonte da parcela paga. | **não responde porque** cadastro e fonte de `P` deixam em aberto a ordem dos abatimentos. |
| **L-039 e L-088** — “FALTA TEM UM SIGNIFICADO = decidida” e “falta e dia previsto sem trabalho e sem justificativa”. `app/docs/LEIS.md:87,120` | **não responde porque** definem falta e furo, sem quantificar abono parcial. | **não responde porque** definem a classificação do dia, sem distribuir desconto entre as pontas. |
| **L-089** — “ausencia que abona/desconta so alcanca dia que a escala previa; em folga nao gera efeito nem consome saldo”. `app/docs/LEIS.md:121` | **não responde porque** delimita os dias elegíveis e deixa o valor de `B` em aberto. | **não responde porque** a elegibilidade do dia deixa o rateio em aberto. |
| **L-073 e tolerância de 14/11** — “so valem comparar INSTANTE ou exigir FATO ENCERRADO” e “5 min por marcacao com teto de 10 no dia”. `app/docs/LEIS.md:100` · `app/docs/CORTES.md:521` | **não responde porque** tratam do momento de julgar e da tolerância de marcação. | **não responde porque** nenhum desses tetos ordena a aplicação do crédito parcial. |
| **L-097 emendada e corte do trabalhado real** — “o trecho apos o marco de saida CONTA no trabalhado e no noturno” e “o teto da L-093 usa o trabalhado REAL do dia (ANTES da janela de HE da L-097)”. `app/docs/LEIS.md:115` · `app/docs/PROMPTS.md:14` | **não responde porque** definem `T`, sem definir `B`. | **não responde porque** definem a base trabalhada do teto e deixam em aberto a aplicação do crédito. |
| **17/09 — CREDITO-NO-PROPRIO-DIA** — “a declaracao parcial paga abate o atraso DO PROPRIO DIA, nunca de outro” e “o dia conta pelas batidas e os minutos viram credito”. `app/ponto/services/credito_parcial.py:1–5` · `app/docs/TICKETS.md:1108` | **não responde porque** os minutos declarados são a opção escrita para o crédito histórico do atraso. O texto não escolhe a parcela que entra no teto líquido das duas pontas. | **não responde porque** manda abater o atraso do próprio dia, mas não diz como compor esse abatimento com o teto líquido nem como tratar a antecipada. |
| **B-B21** — “as horas batidas contam e o abono cobre so o previsto que as batidas nao cobriram; o dia nunca passa de previsto + HE decidida”. `app/docs/CORTES.md:88,420` · `app/ponto/services/dia_pago.py:372–396` | **não responde porque** fixa o abono da linha `ausencia` na grade. A aplicação desse valor a toda parcial elegível para o teto fica em aberto. | **não responde porque** limita o abono pelo que faltou no dia, sem ordenar abatimento e rateio. |
| **BO-ADMIN-2710 e COBERTURA-PARCIAL-PERIODO** — “FATO_EM_AUSENCIA so com batida DENTRO do periodo declarado”, “o periodo de ausencia PARCIAL e cadastro” e “ponta pelo tipo e inferencia, a declaracao nao diz a ponta”. `app/docs/CORTES.md:455,458,462–463,466,477` | **não responde porque** decide a cobertura pelo período declarado. A recusa de inferir a ponta pelo tipo não estabelece coincidência com a ponta como requisito financeiro. | **não responde porque** julgar uma batida ou um marco dentro do período deixa em aberto a precedência entre crédito e teto. |
| **COBERTURA-ABONO-DO-PERIODO** — “pousa agora so a parte da AUSENCIA parcial […] que nao muda o abono do motor” e “o periodo cruza a meia-noite e pertence ao dia de INICIO do turno”. `app/docs/CORTES.md:473` | **não responde porque** separa a cobertura da `Ausencia` do pagamento da `Justificativa` e fixa a chave da parcial noturna. | **não responde porque** separar as obras e fixar o dia da jornada deixa a ordem financeira em aberto. |
| **PARCIAL-BORDA-V2-2511** — “a dispensa do MARCO PREVISTO fica como hoje (periodo_da_parcial_no_dia inclusivo)” e “S no minuto do inicio e E no minuto do fim estao FORA, E no inicio e S no fim estao DENTRO”. `app/docs/CORTES.md:648` | **não responde porque** decide a borda de cobertura, sem quantificar parcela paga. | **não responde porque** a borda da batida não ordena os abatimentos. |
| **ABONADO-COM-SAIDA-NA-ZONA-INVIOLAVEL** — “ausencia que explica o dia INTEIRO zera atraso/saida antecipada; parcial so se os minutos ja saem de juiz existente”. `app/docs/RELATO.md:7039` · `app/ponto/motor_calculo_v2.py:1102–1122` | **não responde porque** exige juiz existente para a parcial, sem identificar qual valor desse juiz entra no teto líquido. | **não responde porque** fixa zero nas duas pontas para a inteira, sem regra de precedência para a parcial. |
| **JUSTIFICATIVA-PAGA-O-PERIODO-B / B-NAO-FECHA** — “a justificativa aprovada vira efeito PARCIAL pela MESMA porta da Ausencia” e “motor_calculo_v2 le o periodo pago”. `app/docs/CORTES.md:479,496` | **não responde porque** o texto dá uma porta de leitura para `Justificativa`, sem escolher `B` da `Ausencia` parcial. | **não responde porque** o aval de leitura do período pago deixa a ordem das duas operações em aberto. |
| **JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO** — “a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo” e “O motor e o periodos_justificados leem o MESMO juiz: um numero so.” A própria linha diz “substitui o aval JUSTIFICATIVA-PELA-SOMA das 18:08”. `app/docs/CORTES.md:558` · `app/docs/PROMPTS.md:2295,2312` | **não responde porque** quantifica o pagamento de `Justificativa` aprovada e não estende essa fórmula à `Ausencia` parcial. | **não responde porque** fixa o total pago pela justificativa e não determina a precedência nem o par de rubricas. |
| **CATALOGO-SAIDA-ANTECIPADA-DESCONTA** — “saida_antecipada = DESCONTA em minutos”. `app/docs/CORTES.md:345` | **não responde porque** define o efeito do tipo e a unidade do desconto. | **não responde porque** o efeito `DESCONTA` não fixa a prioridade do crédito nem o valor final da saída. |
| **Contrato da obra no dossiê** — “A soma esperada nesse contrato é `min(A + S, max(0, P − T − B))`.” Contrato aprovado independente do dossiê [nao verificado]. `r.md:54` (anexo) | **não responde porque** recebe `B` como entrada e deixa expressamente a aplicação a toda parcial para a pergunta 1. | **não responde porque** a fórmula entrega uma soma. A seção 7 apresenta os dois pares para essa mesma soma. `r.md:73` (anexo) |
| **Célula soberana / L-020** — “dia + marcos sao DADO com DNA congelado, irretroativo”. O ADR diz “O VALOR do dia NAO entra na celula: fica no `DiaPago`”. `app/docs/LEIS.md:71` · `app/docs/ADR/0002-celula-e-o-dia.md:8–10` | **não responde porque** fixa a autoridade do dia e dos marcos, sem escolher o valor da parcial. | **não responde porque** a autoridade dos dados do dia deixa a fórmula financeira em aberto. |
| **Juiz único / L-001 e L-002** — “1 JUIZ por pergunta” e “tela, PDF, TXT, badge e contador leem a MESMA autoridade”. `CLAUDE.md:17–22,179` · `app/docs/LEIS.md:59–60` · `app/docs/PLANO-PADROES.md:53–62` | **não responde porque** exige uma autoridade para o valor de `B`, sem selecionar qual valor ela deve entregar. | **não responde porque** exige uma decisão comum aos leitores, sem escolher a ordem da conta. |
| **Escritor único / L-007 e L-095** — “campo de estado tem 1 escritor canonico + tripwire” e “e lavrado **uma vez**, pelo motor v2, no evento”. `app/docs/LEIS.md:65,113` · `CLAUDE.md:179` | **não responde porque** fixa a autoridade da escrita, sem escolher o insumo financeiro. | **não responde porque** lavrar uma vez não determina qual dos dois pares deve ser lavrado. |
| **Lâmpada 4a / L-061** — “CELULA/ATA por marco = o que aconteceu (a LAMPADA)” e “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga”. `CLAUDE.md:170–174` · `app/docs/LEIS.md:95` | **não responde porque** fixa a leitura do fato lavrado por marco, sem converter cobertura em minutos pagos para esse teto. | **não responde porque** ler a lâmpada não fixa a precedência entre crédito e redução proporcional. |
| **L-110 e L-117** — “o codigo de hoje **nao e gabarito** do codigo novo” e “Lei nova so nasce de **corte LITERAL do Ronald**”. `app/docs/LEIS.md:136,138` | **não responde porque** o comportamento atual prova a fonte do crédito histórico e a regra de conduta exige corte literal para a escolha ainda aberta. | **não responde porque** o comportamento atual prova a sequência da cópia e a regra de conduta exige corte literal para a composição ainda aberta. |

A identidade da cópia com o hash informado e os valores efetivamente emitidos em TXT entregue ficam [nao verificado].

## 26/11 10:41 · sessao e840e4e72b81 · CLEAR-NAO-PARA-A-NOITE · verificador · origem=Code

Os cinco arquivos da cópia conferem com os hashes de conteúdo do lado novo do diff. A identidade Git de `HEAD` e do pai, a execução integral dos selos e o comportamento em produção ficam **[nao verificado]**. Conferi a sintaxe dos dois selos e reproduzi em memória os contraexemplos de handoff repetido, retorno com handoff antigo, fallback de CLEAR e contador com linha truncada. O selo integral cria e apaga arquivos. `bin/tests/test_orcamento_contexto.sh:32`, `bin/tests/test_orcamento_contexto.sh:611`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1250`, `bin/disjuntor.py:2121`.

## Achados da v1.r

| Achado | O que a volta 2 resolve |
|---|---|
| Janela de 23:00 a 08:00 ou 30 min sem resposta | **Em parte.** A janela continua incluindo 23:00 e excluindo 08:00, agora com selo nas quatro bordas. De dia continua exigindo **mais de** 30 min desde a fala selecionada, sem verificar se ela responde a uma pergunta. `bin/orcamento_contexto.py:89`, `bin/orcamento_contexto.py:113`, `bin/tests/test_orcamento_contexto.sh:516`. |
| Disparo diurno atravessava a entrada da noite em silêncio | **Resolve o caminho apontado.** Com topo neutro e sem handoff elegível, há anúncio próprio uma vez. Com handoff elegível, sai o recado de fim de marco. Com `PAREI` no topo ou DIFF/apply detectado, esses ramos têm prioridade. O AVISO efetivamente escrito no RELATO fica **[nao verificado]**. `bin/hook_stop_fila1.py:1082`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1118`. |
| Instrução de handoff faltava nas entradas tardias e só havia um recado de fim de marco | **Em parte.** As entradas agora pedem o handoff e o recado se repete a cada `mtime` elegível. O hook usa o `mtime` do arquivo como prova de marco, inclusive quando o mesmo arquivo ainda satisfaz o corte. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1106`, `bin/hook_stop_fila1.py:1110`. |
| Troca de `PAREI` e fim de marco passavam antes da guarda de DIFF/apply | **Em parte.** A guarda agora precede os dois. Com a trava detectada, há um `siga` próprio para concluir o ato, que também carrega a instrução de handoff e AVISO. A sondagem libera a trava antes da decisão e da emissão. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1096`, `bin/disjuntor.py:410`, `bin/hook_stop_fila1.py:1439`. |
| Retorno diurno perdia novo pedido por causa de `fim_visto` e herdava o tempo de espera | **Em parte.** O retorno limpa `fim_visto` e `clear_parei_desde`, e com topo neutro um handoff elegível volta a provocar o pedido. O caminho direto de `PAREI` pode reaproveitar handoff diurno, e uma leitura inconclusiva também dispara a transição de volta. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1161`, `bin/hook_stop_fila1.py:1184`, `bin/orcamento_contexto.py:95`. |
| Dois classificadores de fala e duas aberturas da trilha do orçamento no mesmo laço | **Resolve.** ESCADA e janela usam `ultima_fala`. `ATENDIDO` e `CLEAR-NOITE` são lidos na mesma abertura em `disjuntor.ler`. A ESCADA passa a aceitar `enqueue` como entrada, inclusive se houver `remove` posterior. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1394`, `bin/disjuntor.py:1409`, `bin/orcamento_contexto.py:105`, `bin/disjuntor.py:2115`. |
| Classificação das linhas de transcript | **Em parte.** Slash e eco bash iniciados por `<`, `enqueue` vazio e `enqueue` de sidechain ficam excluídos. Imagem sem texto e resposta representada por `tool_result` ainda ficam excluídas. Um `enqueue` válido seguido de `remove` continua valendo por regra explícita. O formato real dessas entradas fica **[nao verificado]**. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1355`, `bin/disjuntor.py:1360`, `bin/tests/fixtures/orcamento_transcript.jsonl:1`. |
| Transcript ilegível ou sem instante e contenção de exceções | **Em parte.** `OSError` e última fala reconhecida sem instante dão `None` de dia. JSON truncado ainda é pulado e pode deixar uma fala antiga decidir a ausência. A janela horária ainda decide antes de ler o arquivo. `bin/orcamento_contexto.py:99`, `bin/orcamento_contexto.py:106`, `bin/disjuntor.py:1376`, `bin/hook_stop_fila1.py:1234`. |
| Zero ambíguo, futuro aceito e rótulo do contador | **Em parte.** `OSError` e data inválida numa linha reconhecida agora renderizam `nao sei`, o futuro é excluído e o rótulo diz “recados”. Linha curta ou com o evento fora da terceira coluna ainda pode virar zero. A trilha ainda grava hora local sem offset, interpretada como local pelo leitor. O fuso efetivo dos processos em produção fica **[nao verificado]**. `bin/disjuntor.py:2121`, `bin/disjuntor.py:2134`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2421`, `bin/hook_stop_fila1.py:887`. |
| Selo aceitava dia vazio, `siga` repetido e 08:00 dentro da noite | **Resolve esses três buracos.** O caso diurno exige a frase inteira, o caso `PAREI` exige o `ALLOW` comum no segundo Stop e o caso de borda exige `None` às 08:00 com fala recente. `test_disjuntor.sh` agora acrescenta fala recente e fixa a hora para os casos diurnos. Outras lacunas do selo constam abaixo. `bin/tests/test_orcamento_contexto.sh:519`, `bin/tests/test_orcamento_contexto.sh:529`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_disjuntor.sh:87`. |
| Registro no 7b, `PROMPTS.md` e config | **Não resolve a entrega documental.** O 7b da cópia contém a regra geral anterior e `PROMPTS.md` termina sem o registro deste aval. O diff fornecido preserva a config, que já chama o handoff no `PreCompact`. `CLAUDE.md:343`, `app/docs/PROMPTS.md:3005`, `.claude/settings.json:14`, `.claude/settings.json:25`. |

## 1. Escritores, leitores e ESMERIL-SEMÂNTICA

O escritor em operação é `_orcamento_travado`, no Stop. Ele roda sob a trava do estado, salva por `os.replace` e depois apenda os eventos. O disjuntor lê `ultima_medida` do mesmo JSON, sem consumir esses campos. Os selos semeiam `clear_parei_desde` diretamente para simular espera. `bin/hook_stop_fila1.py:903`, `bin/hook_stop_fila1.py:972`, `bin/disjuntor.py:2165`, `bin/tests/test_orcamento_contexto.sh:197`, `bin/tests/test_disjuntor.sh:212`.

| Campo | Escrita, leitura e significado |
|---|---|
| `noite` | O Stop grava `True` ao armar ou processar disparo na janela. Lê na saída da janela. Marca que o disparo esteve na conduta sem Ronald, por horário ou ausência. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1222`. |
| `noite_anunciada` | O Stop grava no disparo noturno, no fim de marco ou no anúncio de entrada. Lê para suprimir o anúncio de entrada. É um registro de recado emitido por esses ramos. A troca de `PAREI` e o aviso de DIFF também carregam a instrução e deixam o campo intacto. `bin/hook_stop_fila1.py:1093`, `bin/hook_stop_fila1.py:1103`, `bin/hook_stop_fila1.py:1112`, `bin/hook_stop_fila1.py:1118`, `bin/hook_stop_fila1.py:1222`. |
| `noite_clear_visto` | O ramo de `PAREI` noturno lê e grava `True` ao **pedir** a troca. Suprime outro pedido no mesmo trecho noturno, mesmo se o topo mudar e depois voltar a `PAREI`. `bin/hook_stop_fila1.py:1077`, `bin/hook_stop_fila1.py:1101`. |
| `adiado_noite` | O ramo DIFF lê e grava a string do motivo para suprimir repetição. Apaga na primeira observação fora desse ramo. A string identifica o motivo, sem identificar o ato que tomou a trava. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1100`. |
| `fim_visto` | O ramo diurno lê e grava `True` quando **pede** `PAREI` após handoff elegível. A flag prova emissão do pedido. `bin/hook_stop_fila1.py:1184`. |
| `fim_dia_em` | O ramo diurno grava o epoch `agora` junto com `fim_visto`. A noite lê no corte de handoff e a volta incorpora em `handoff_min`. É o instante do Stop que pediu a linha. O caminho diurno que recebe o par diretamente libera sem gravá-lo. `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1161`, `bin/hook_stop_fila1.py:1186`. |
| `handoff_min` | A noite e o adiamento diurno gravam `agora`. A volta grava o máximo com `fim_dia_em`. Os leitores são fim noturno, par diurno e fim diurno. A unidade continua epoch em segundos, com limite inferior inclusivo sobre `st_mtime`. As escritas usam o instante dos Stops. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1184`. |
| `clear_parei_desde` | O ramo diurno grava o primeiro Stop com o par apto a liberar e zera em adiamento. O mesmo ramo lê para expirar e o fechamento do ciclo lê para medir o clear. O sentido é o início da espera válida pelo `/clear`. `bin/hook_stop_fila1.py:1032`, `bin/hook_stop_fila1.py:1129`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1161`. |
| `sem_handoff_visto` | O ramo diurno lê e grava ao emitir a primeira cobrança de handoff para `PAREI` sem o par. Marca a cobrança emitida. `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1135`. |
| `adiado` | O ramo diurno lê e grava o motivo textual da guarda, DIFF/apply ou agente de fundo, para suprimir recado repetido pelo mesmo motivo. `bin/hook_stop_fila1.py:936`, `bin/hook_stop_fila1.py:1145`. |

Na volta, o Stop remove todos esses campos salvo `handoff_min`, que atualiza. O marco novo descarta o disparo inteiro. As unidades dos campos de tempo são coerentes. As ambiguidades procedentes são usar o instante do pedido como testemunha do handoff consumido, usar `mtime` como identidade de marco e tratar o `None` de presença **ou** leitura inconclusiva como “Ronald presente de novo”. `bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1186`, `bin/orcamento_contexto.py:95`.

## 2. Juízes em `bin/`

Há um classificador de fala, `disjuntor._fala_do_ronald`, e um juiz de janela, `orcamento_contexto.janela_sem_ronald`. Os outros leitores de `type == 'user'` encontrados usam a linha para identificar o prompt inicial do verificador B3 ou o comando `/clear` para o piso. O leitor de `queue-operation` para fala é o classificador compartilhado. `bin/disjuntor.py:649`, `bin/disjuntor.py:1342`, `bin/disjuntor.py:1350`, `bin/orcamento_contexto.py:95`, `bin/orcamento_contexto.py:289`.

A centralização mudou dois critérios da janela. A cauda cai de 4 MiB para 2.000.000 bytes e “última” passa do maior timestamp para a última fala na ordem do arquivo. Em memória, uma linha de 1 min atrás seguida no arquivo por outra de 40 min abriu ausência. O próprio selo também contém o `user` sintético `Async agent launched successfully...`, que o leitor de agentes identifica como retorno do harness e o juiz de fala aceita como texto. Sem timestamp, essa entrada sobrepõe a fala anterior e dá `None` na janela diurna. Ocorrência desses formatos em produção **[nao verificado]**. `bin/orcamento_contexto.py:65`, `bin/disjuntor.py:469`, `bin/disjuntor.py:1352`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1376`, `bin/orcamento_contexto.py:111`, `bin/tests/test_disjuntor.sh:235`.

## 3. Repetição, trava e liberação

- **Repetição confirmada para o mesmo handoff.** O Stop aceita `mtime >= corte` e grava o seu próprio `agora` como próximo corte. Um `mtime` igual ao corte pode ser aceito de novo. Um `mtime` no futuro gerou dois `siga` consecutivos em memória, sem escrita nova de handoff. O cursor avançou nos dois Stops, mas o marco e o recado eram os mesmos. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:999`, `bin/hook_stop_fila1.py:1110`.
- **Sai `siga` com `sombra.lock` tomada.** É o recado específico para concluir o ato em curso. Ele proíbe abrir outro DIFF/apply e inclui instrução de handoff e AVISO. Há também uma corrida para o recado normal, pois a guarda solta o `flock` compartilhado antes de salvar e imprimir. `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1114`, `bin/disjuntor.py:421`, `bin/hook_stop_fila1.py:1439`.
- **O Stop pode encerrar de noite.** Quando o orçamento retorna `None`, o fluxo comum pode dar `ALLOW` silencioso. O ramo noturno normal não dá `CLEAR-LIBERA`. Já o fallback de exceção dá `CLEAR-LIBERA` com `PAREI`, handoff recente e guarda livre, sem consultar a janela. Reproduzi esse fallback em memória. `bin/hook_stop_fila1.py:132`, `bin/hook_stop_fila1.py:1125`, `bin/hook_stop_fila1.py:1234`, `bin/hook_stop_fila1.py:1250`, `bin/hook_stop_fila1.py:1436`, `bin/hook_stop_fila1.py:1480`.

## 4. Transições e aval

| Transição | Saída e aderência |
|---|---|
| Dia → noite, disparo ativo e topo neutro | **Atende no fluxo nominal.** Sem handoff elegível, um `siga` pede handoff e AVISO. Com handoff elegível, o recado assume fim de marco. Depois, sem novidade, segue o caminho comum. `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1118`, `bin/hook_stop_fila1.py:1480`. |
| Noite → dia, retorno | **Parcial.** Registra `CLEAR-VOLTA`, limpa as marcas anteriores e, com topo neutro, volta a pedir `PAREI` quando o handoff é elegível. O corte usa os Stops anteriores, portanto um handoff anterior à fala de retorno pode qualificar. Transcript inconclusivo também aciona a volta. Uma fala dentro de 23:00–08:00 mantém a regra horária. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1184`, `bin/orcamento_contexto.py:95`. |
| DIFF/apply começa e acaba de noite | **Parcial.** Se o Stop observa a trava tomada, sai um `siga` de conclusão e os seguintes caem no fluxo comum enquanto o motivo persiste. Ao observar a trava livre, apaga `adiado_noite` e reavalia topo, handoff e anúncio. Dois atos separados por uma folga que nenhum Stop observou compartilham a supressão. A corrida da sondagem impede garantir a ordem até a emissão. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1100`, `bin/disjuntor.py:421`. |
| `PAREI` de dia atravessa para a noite | **Parcial.** Com a guarda livre, sai `siga` pedindo a troca por AVISO. Reproduzi em memória a sequência em que o par já havia liberado de dia pela rota direta, sem `fim_dia_em`. Após a troca, o mesmo handoff produziu um falso fim de marco noturno. Se o `PAREI` persistir até a volta, o mesmo par pode liberar de dia novamente. `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1161`. |
| Disparo pendente entra à noite | **Atende quando os impedimentos cessam.** Com DIFF/apply ou outro impedimento reconhecido, mantém `PENDENTE`. No primeiro Stop elegível, pode armar mesmo com a medida abaixo do limiar e sai `siga` de AVISO, handoff e fila. `bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1205`, `bin/hook_stop_fila1.py:1218`. |
| Marco de `/clear` novo à noite | **Fecha o ciclo antigo.** A classificação `ATENDIDO` ou `SEGUIU` ocorre antes da janela. Se carregou `clear_parei_desde` de dia, o intervalo inclui o tempo noturno e pode registrar clear tardio. Com novo gatilho elegível, arma outro ciclo noturno no mesmo Stop. Sem gatilho, segue o fluxo comum. O registro comprova que o hook observou o marco. A execução do handoff antes do ato fica **[nao verificado]**. `bin/hook_stop_fila1.py:1025`, `bin/hook_stop_fila1.py:1059`, `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1221`. |

## 5. `disjuntor.ler`

- **`nao sei` é parcial.** `OSError` e uma linha reconhecida como `CLEAR-NOITE` com data inválida viram `None`. `lixo hora CLEAR-NOITE`, com três palavras, é descartada antes da validação e deu `0` na sonda em memória. Uma trilha existente e legível sem eventos na janela também dá zero. `FileNotFoundError` indica caminho ausente no momento da leitura, inclusive se a trilha tiver sido removida. `bin/disjuntor.py:2113`, `bin/disjuntor.py:2121`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2422`.
- **Há teto superior.** A janela implementada é `[agora − 86400, agora]`. O código inclui a borda inferior que o comentário descreve como exclusiva. `bin/disjuntor.py:2105`, `bin/disjuntor.py:2111`, `bin/disjuntor.py:2132`.
- **A `TRILHA` principal preserva os filtros e contagens para arquivo estável**, inclusive o tratamento de erro e a aceitação de linhas futuras. A mudança unifica as quatro leituras desse laço. O censo ainda abre `TRILHA` separadamente para outra medida. `bin/disjuntor.py:2115`, `bin/disjuntor.py:2124`, `bin/disjuntor.py:2136`, `bin/disjuntor.py:2155`, `bin/disjuntor.py:2237`.

## 6. Mordida do selo, seção 7

| Caso | O que derruba e o que ainda passa |
|---|---|
| **(a) Fala e janela** | Derruba rejeitar `enqueue` válido e incluir 08:00 na noite. Uma cópia do juiz com outro nome e os mesmos resultados passa. A fala digitada da amostra fica atrás do `enqueue`. `bin/tests/test_orcamento_contexto.sh:494`, `bin/tests/test_orcamento_contexto.sh:497`, `bin/tests/test_orcamento_contexto.sh:509`, `bin/tests/test_orcamento_contexto.sh:519`. |
| **(b) Dia** | Derruba saída vazia, frase alterada e evento `CLEAR-NOITE` diurno. O zero numérico do contador fica sem medição neste caso. `bin/tests/test_orcamento_contexto.sh:529`, `bin/tests/test_orcamento_contexto.sh:542`. |
| **(c) Disparo e contador** | Derruba remover o handoff da mensagem, contar o futuro ou transformar em número o erro de leitura amostrado. Remover o corte inferior ou ignorar a linha truncada de três palavras passa. `bin/tests/test_orcamento_contexto.sh:534`, `bin/tests/test_orcamento_contexto.sh:549`, `bin/disjuntor.py:2121`. |
| **(d) Marcos e volta** | Derruba perder um dos dois recados ou repetir no Stop intermediário da amostra. Passa reaproveitar um mesmo `mtime` futuro. O assert da volta também aceita a mensagem diurna sem o `PAREI` no sufixo, pois confere só o prefixo. `bin/tests/test_orcamento_contexto.sh:560`, `bin/tests/test_orcamento_contexto.sh:573`. |
| **(d2) Dia → noite → dia** | Derruba silêncio de entrada, anúncio repetido e `fim_visto` antigo impedindo a volta com handoff novo. Remover o AVISO do fim do anúncio ainda passa pelo prefixo exigido. `bin/tests/test_orcamento_contexto.sh:576`, `bin/tests/test_orcamento_contexto.sh:584`. |
| **(g) DIFF/apply** | Derruba troca de `PAREI` sob trava e repetição do recado de DIFF. Um mutante que aplica a guarda noturna **só se `_eh_clear(linha)`** passa, pois o teste sempre põe `PAREI` no topo durante a trava. Com topo neutro e handoff elegível, esse mutante dá `siga` de fim de marco sob trava. `bin/tests/test_orcamento_contexto.sh:586`, `bin/tests/test_orcamento_contexto.sh:596`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1110`. |
| **(e) `PAREI` noturno** | O mutante `if True` chega ao hook selecionado por `local H` e cai no passo 2 se repetir a troca. O caso não exercita o fallback de exceção nem o `PAREI` reaparecendo após topo neutro. O `siga-aviso` do selo vem do `ALLOW` na trilha. `bin/tests/test_orcamento_contexto.sh:112`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_orcamento_contexto.sh:614`, `bin/tests/_saida_do_stop.sh:13`, `bin/hook_stop_fila1.py:1250`. |
| **(f) Ausência de dia** | Derruba remover a ausência diurna ou usar a última linha do assistente como fala. A troca de `>` por `>=` no limiar de 30 min passa pelos valores amostrados. `bin/tests/test_orcamento_contexto.sh:513`, `bin/tests/test_orcamento_contexto.sh:623`, `bin/orcamento_contexto.py:113`. |

## 7. Classes de reprovação

**Procedem** mordida incompleta do selo, ambiguidade na transição de retorno e prova insuficiente do pré-requisito de handoff. A ordem local da guarda melhorou, mas a trava de DIFF/apply fica livre entre sondagem e emissão. Pouso ou deploy efetivo **[nao verificado]**. `bin/tests/test_orcamento_contexto.sh:586`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1110`, `bin/disjuntor.py:421`, `bin/hook_stop_fila1.py:1439`.

**Não procedem no diff** segundo juiz registrado, segundo escritor dos campos fora da porta ou relógio novo fora da borda. A derivação paralela da fala e a abertura duplicada de `orcamento.log` dentro de `disjuntor.ler` foram removidas. O timestamp do append co

[truncado em 20000 de 21557 caracteres]

## 26/11 10:40 · sessao 03ce12260be3 · O327 · leitura · origem=Code

```sh
#!/bin/sh
# Executar na raiz da copia. Entrada do manage.py em app/manage.py:7–18.
# Execucao na sombra e correspondencia com as 93 recusas do relato [nao verificado].
cd app || exit 1

exec python -B -c '
import logging
import os
import sys
from collections import Counter
from contextlib import ExitStack
from unittest.mock import patch


def sonda():
    from django.conf import settings
    from django.db import connection, transaction
    from django_tenants.utils import schema_context
    from ponto.calculador import alimentacao
    from ponto.services import dia_pago, fechamento

    mes, ano = 11, 2026
    # Banco e host literais em app/config/settings/sombra.py:29,47–51,60.
    # Schema juliani em app/config/settings/sombra.py:13–14.
    assert settings.HASNER_SOMBRA is True
    assert connection.settings_dict["NAME"] == "sombra"
    assert connection.settings_dict["HOST"] == "sombra_db"
    assert (mes, ano) not in fechamento.GUARDA_COMPETENCIA  # app/ponto/services/fechamento.py:36–50,578–595

    codigo_rubricas = alimentacao.rubricas_por_dia.__code__
    observadas = {}
    visitas = Counter()
    linhas = []

    def observar_rubricas(frame, evento, retorno):
        if evento != "return" or frame.f_code is not codigo_rubricas or not isinstance(retorno, dict):
            return

        locais = frame.f_locals
        pk = locais["c"].pk
        visitas[pk] += 1

        # Le os itens e bloqueios da chamada real. app/ponto/calculador/alimentacao.py:449,467–475,514–533
        inicio, fim = locais["ini"], locais["fim"]
        itens = locais["_itens"]
        n_minutos = len(locais["v"].minutos)
        bloqueados_na_janela = locais["_bloq"]
        bloqueados_publicados = {
            item["dia"]
            for item in (getattr(locais["resultado"], "dias_sem_previsto_fora_da_janela", None) or ())
            if item.get("dia") is not None
        }
        n_adicionados = len(itens) - n_minutos

        # 0 = com rubricas, sem bloqueado acrescentado aos itens
        # 1 = itens vazios: v.minutos vazio e zero bloqueados na janela
        # 2 = itens presentes, todos fora da janela
        # 3 = com rubricas e pelo menos um bloqueado acrescentado aos itens
        if retorno:
            ramo = 3 if n_adicionados else 0
        else:
            ramo = 2 if itens else 1

        dias = (
            (fim - inicio).days + 1 - len(retorno),
            len(retorno),
            n_minutos,
            len(itens) - len(retorno),
            len(bloqueados_na_janela),
            len(bloqueados_publicados - bloqueados_na_janela),
            n_adicionados,
        )
        observadas[pk] = (retorno, ramo, dias)

    def interceptar_lavra(colab, mes_lavra, ano_lavra, *, versao, rubricas=None, **dados):
        # Intercepta as duas chamadas. app/ponto/services/fechamento.py:1223–1261,1301–1320
        if versao == "motor":
            return 0

        assert versao == "oraculo" and (mes_lavra, ano_lavra) == (mes, ano)
        retorno, ramo, dias = observadas[colab.pk]
        assert rubricas is retorno

        # Avalia a guarda pura com os quatro mapas recebidos.
        # app/ponto/services/dia_pago.py:50–76,135–141,312–321
        sem_dono = set()
        if rubricas is not None:
            donos = dia_pago._DONOS_DECLARADOS(
                rubricas,
                dados["faltas_por_dia"],
                dados["abono_por_dia"],
                dados["previsto_por_dia"],
                dados["realizado_por_dia"],
                dados.get("do_motor", ()),
            )
            sem_dono = set(dia_pago.CAMPOS_DIA) - donos

        linhas.append((colab.pk, ramo, len(sem_dono), dias))
        return 0

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            assert cursor.fetchone() == ("sombra", "juliani")

        # app/ponto/services/fechamento.py:448–465
        universo = set(fechamento.universo_da_competencia(mes, ano).values_list("pk", flat=True))
        assert universo

        with ExitStack() as controles:
            controles.enter_context(patch.object(dia_pago, "lavrar", new=interceptar_lavra))
            controles.callback(sys.setprofile, sys.getprofile())
            sys.setprofile(observar_rubricas)
            # O fechamento monta os insumos e chama rubricas_por_dia.
            # app/ponto/services/fechamento.py:896–921,1152–1168,1215–1225,1289–1300
            leitura = fechamento.recalcular_fechamento_mes(
                mes, ano, somente_leitura=True, lavrar_dias_pagos=True,
            )

        assert set(leitura) == universo
        assert visitas == Counter({pk: 1 for pk in universo})
        assert len(linhas) == len(universo)
        assert {pk for pk, _, _, _ in linhas} == universo

    # colNNN usa o PK, conforme a convencao de app/ponto/management/commands/censo_fase_12x36.py:263.
    # Linha individual: colNNN ramo campos_sem_dono, seguida das sete contagens de dias.
    # Dias: sem rubrica na janela, com rubrica, em v.minutos, itens fora da janela,
    # bloqueados na janela, bloqueados fora da janela, bloqueados acrescentados aos itens.
    for pk, ramo, campos, dias in sorted(linhas):
        print(f"col{pk:03d}", ramo, campos, *dias)

    # Total: 0 universo processados recusados recusados_com_11_campos.
    print(0, len(universo), len(linhas),
          sum(campos > 0 for _, _, campos, _ in linhas),
          sum(campos == 11 for _, _, campos, _ in linhas))

    # Por ramo: 1 ramo colaboradores recusados recusados_com_11_campos,
    # seguido das somas das sete contagens de dias na mesma ordem.
    for ramo in range(4):
        grupo = [(campos, dias) for _, r, campos, dias in linhas if r == ramo]
        print(1, ramo, len(grupo),
              sum(campos > 0 for campos, _ in grupo),
              sum(campos == 11 for campos, _ in grupo),
              *(sum(dias[i] for _, dias in grupo) for i in range(7)))


# Evita a criacao do diretorio e a abertura dos arquivos de log no bootstrap.
# app/config/settings/sombra.py:27 e app/config/settings/saas.py:118–119,136–143,160–167
os.environ["DJANGO_SETTINGS_MODULE"] = "config.settings.sombra"
logging.disable(sys.maxsize)
with patch.object(os, "makedirs"):
    from config.settings import sombra
sombra.LOGGING_CONFIG = None

from manage import main

sys.argv = [
    "manage.py", "shell", "--settings=config.settings.sombra",
    "--no-imports", "--verbosity=0",
    "--command=from __main__ import sonda\nsonda()",
]
main()
'
```

## 26/11 10:39 · sessao b12082e302b7 · - · - · origem=Ronald (tmux)

A seção 7 corrige a mordida contra 08:00 inclusivo, saída diurna vazia e repetição da troca de `PAREI` pela mutação `if True`. O último caso testa o arquivo mutado porque a variável local `H` fica visível à chamada de `_roda` pelo escopo dinâmico do Bash. O teste final de liberação diurna reutiliza a árvore do mutante e chama o hook original. `bin/tests/test_orcamento_contexto.sh:112`, `:519`, `:529`, `:598`, `:617`, `:620`.

| Caso | Mordida e lacuna |
|---|---|
| (a) | Cobra `enqueue` válido, filtros, arquivo ausente e limites horários. Rejeitar todo `user` cujo conteúdo seja lista passa, pois a lista da fixture contém `tool_result` e é negativa. Um juiz duplicado sob outro nome também passa se produzir os mesmos resultados no exemplo. `bin/tests/test_orcamento_contexto.sh:494`, `:497`, `:502`, `:509`, `:519`. |
| (b) | Compara o recado diurno inteiro e a ausência de `CLEAR-NOITE`. O zero numérico diurno ainda fica sem assert. `bin/tests/test_orcamento_contexto.sh:529`, `:532`, `:541`. |
| (c) | Exige handoff, AVISO, evento, contagem, teto futuro, `None` com erro de abertura e timestamp inválido em linha com quatro tokens. Passa remover o corte inferior ou devolver zero para linha truncada com três tokens. `bin/tests/test_orcamento_contexto.sh:534`, `:549`, `:553`, `:555`, `bin/disjuntor.py:2121`. |
| (d) | Cobra dois marcos noturnos separados por um ALLOW comum, o evento de volta e a reabertura diurna. Os asserts de prefixo aceitam item incorreto no fim do recado e omissão do pedido literal de `PAREI` no recado diurno. `bin/tests/test_orcamento_contexto.sh:560`, `:563`, `:568`, `:573`. |
| (d2) | Cobra anúncio na entrada, ausência de repetição e novo fim diurno. O assert do anúncio termina no comando de handoff e aceita a omissão de AVISO e continuidade da fila. `bin/tests/test_orcamento_contexto.sh:576`, `:578`, `:580`, `:583`. |
| (g) | Cobra prioridade do recado da trava com topo `PAREI`, ausência de repetição durante a mesma tomada e troca quando ela solta. Não separa topo neutro com handoff novo nem disparo pendente. A primeira resposta é explicitamente um `siga` com a trava tomada. `bin/tests/test_orcamento_contexto.sh:586`, `:589`, `:590`, `:593`, `:596`. |
| (e) | Cobra troca, depois ALLOW comum, ausência de `CLEAR-LIBERA` e queda precisa do `if True` no segundo passo. Deixa de fora falha contida e nova escrita do `PAREI` na mesma noite. `bin/tests/test_orcamento_contexto.sh:598`, `:604`, `:617`. |
| (f) | Cobra ausência de dia apesar do último assistente recente. Trocar `>` por `>=` no limite de 30 min passa com exemplos de 25 e 40 min. `bin/tests/test_orcamento_contexto.sh:514`, `:623`, `:631`, `bin/orcamento_contexto.py:114`. |

O helper converte um ALLOW real, sem saída do hook, em `siga-aviso` a partir da trilha. `bin/tests/_saida_do_stop.sh:13`, `:15`, `bin/hook_stop_fila1.py:1480`. O selo do disjuntor restaura fala recente sintética e hora diurna padrão para C1 e ajusta âncoras dos dois mutantes da Escada. `bin/tests/test_disjuntor.sh:87`, `:98`, `:121`, `:129`, `:1723`. A sintaxe dos dois scripts passou em `bash -n`. Execução completa **[nao verificado]**, pois os scripts escrevem e apagam arquivos. `bin/tests/test_orcamento_contexto.sh:32`, `:551`, `:611`.

Em memória, `disjuntor.ler` devolveu `0` para trilha ausente, vazia, somente futura e linha `lixo hora CLEAR-NOITE` truncada. Devolveu `None` para `OSError` e a mesma linha com um quarto token. Contou a linha exatamente em `agora-86400` e exatamente em `agora`. `bin/disjuntor.py:2105`, `:2113`, `:2121`, `:2125`, `:2132`, `:2134`, `:2153`. Os contadores da `TRILHA` mantêm a semântica com arquivo estável. `_censo_24h` ainda reabre `TRILHA` para a contagem própria. `bin/disjuntor.py:2136`, `:2161`, `:2192`, `:2237`.

Em memória, o leitor de presença ainda declarou ausência com assistente datado de 40 min antes seguido de linha `user` truncada. Também declarou ausência com fala de 5 s antes seguida no arquivo por fala de 40 min antes. Última fala reconhecida sem timestamp e fala exatamente aos 30 min retornaram `None`. O leitor novo toma a última fala pela ordem do arquivo e reduz a janela lida de 4 MiB para 2.000.000 bytes em relação à v1. `bin/disjuntor.py:1370`, `:1376`, `:1391`, `bin/orcamento_contexto.py:65`, `:111`, `:118`.

O recado novo de DIFF/apply inclui instrução de handoff e AVISO no topo com a trava tomada. O script de handoff relata a trava e prossegue até escrever `HANDOFF-SESSAO.md`. `bin/hook_stop_fila1.py:1070`, `:1091`, `:1097`, `bin/handoff_sessao.sh:18`, `:251`, `:318`, `:327`.

## 26/11 10:39 · sessao b3f1530d333c · - · - · origem=Ronald (tmux)

- A duplicação do juiz de fala foi resolvida. B4 e CLEAR usam `_fala_do_ronald` via `ultima_fala`. Os outros leitores de `user` em `bin/` extraem o prompt do verificador B3 e o marco `/clear`. A Escada passa a considerar `enqueue`, inclusive quando seguido de `remove`. `bin/disjuntor.py:649`, `bin/disjuntor.py:1342`, `bin/disjuntor.py:1365`, `bin/disjuntor.py:1394`, `bin/disjuntor.py:1402`, `bin/orcamento_contexto.py:105`, `bin/orcamento_contexto.py:289`.
- Classificação da v1 parcialmente corrigida. Slash, bash iniciado por `<`, vazio e sidechain enfileirado são excluídos. Imagem sem texto e `AskUserQuestion` com `tool_result` continuam excluídos. O harness demonstrado no selo `Async agent launched successfully...` é aceito pelo predicado se chegar sem flags. A nova seleção escolhe a última linha em ordem de arquivo e limita a cauda a 2 MB. Em memória, gravei fala de 1 min e depois fala de 40 min e a janela abriu pela antiga. Formatos em produção [nao verificado]. `bin/disjuntor.py:472`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1365`, `bin/orcamento_contexto.py:113`, `bin/tests/test_disjuntor.sh:235`.
- Dois `except ValueError` novos em `ultima_fala`. Timestamp ilegível da última fala reconhecida produz `None` de dia. JSON truncado é pulado e uma cauda antiga ainda pode produzir ausência, confirmado em memória. OSError de dia já produzia `None`, e a janela do relógio precede o I/O. O hook interpreta esse mesmo `None` como volta comprovada, grava `CLEAR-VOLTA` e apaga as travas por estado. `bin/disjuntor.py:1376`, `bin/orcamento_contexto.py:95`, `bin/orcamento_contexto.py:107`, `bin/hook_stop_fila1.py:1071`.
- Contador parcialmente corrigido. OSError e linha reconhecida com timestamp ruim produzem `None` e o painel mostra `nao sei`. `lixo hora CLEAR-NOITE` e `CLEAR-NOITE despedida` produzem zero, pois falham na identificação antes do parse. ENOENT só mostra caminho ausente no ato. Trilha válida sem eventos na janela também produz zero legítimo. O intervalo efetivo inclui os dois extremos e exclui futuro. Para arquivo principal estável os filtros e contagens se mantêm, inclusive futuro e efeito de OSError. Cada trilha é aberta uma vez no laço de agregação, e o censo continua com leitura própria da TRILHA. `bin/disjuntor.py:2105`, `bin/disjuntor.py:2113`, `bin/disjuntor.py:2121`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2237`, `bin/disjuntor.py:2422`.
- Pendência documental permanece. A regra geral no 7b e o PreCompact automático já existem. O aval específico segue ausente da cópia de `PROMPTS.md`. Os cinco hashes de blob conferem com o anexo. Identidade Git do commit/pai, selo integral e comportamento de produção [nao verificado]. `CLAUDE.md:343`, `app/docs/PROMPTS.md:3005`, `.claude/settings.json:25`, `bin/tests/test_orcamento_contexto.sh:32`.

## 26/11 10:38 · sessao 09732ae72b93 · O30-F3-CONSTRUIR · construtor · origem=Code

A cura D1–D5 está na cópia sobre `0191ece77c2383771232eb4c449cc33dfc20055b`. O `HEAD` foi conferido e a entrega ficou no workspace sem commit, conforme a proibição expressa de `git commit` e de escrita em `.git`. [Base dos REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:18)

**Prova local.** Passaram 44 testes de contrato sem banco e 37 testes estruturais com `SimpleTestCase` substituído por `unittest.TestCase`. Os cinco comandos RED contra a c3 e os 14 mutantes terminaram com `rc=1` pelas asserções esperadas, sem erros de execução. O diff acrescentou **0 `except`**, incluindo testes, e passou em `git diff --check`. Três testes estruturais dependentes de Django, a integração e a concorrência real no PostgreSQL ficam [nao verificado]. A certificação fica com o Code e o Opus. [Contrato F3](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:128) · [Mutantes](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:190) · [Selo estrutural](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:296) [nao verificado]

### Decisões e REDs

| Item | Decisão e falha na c3 |
|---|---|
| **D1** | Criar, fixar e reabrir recusam referência que já tem referência. O RED mantém `C → B → A`, comprova a recusa e compara os bytes do ZIP com a entrega protegida pelo vínculo válido `B → A`. [Porta](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:283) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:551) |
| **D2** | A porta trava raiz, linha pedida e irmãs antes de autorizar a troca própria. Na c3, uma irmã aberta recebia `200` mesmo quando uma irmã aprovada ou rejeitada lia o arquivo compartilhado. [Porta](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1181) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:791) |
| **D3** | O upload próprio em ausência decidida devolve `409`, com ou sem comprovante gravado. O RED da aprovada sem arquivo exige zero gravações e confirma que o DP pode anexar. Na c3, o upload próprio devolvia `200`. [Registro da decisão](/home/ronald/wt-codex-o30-f3-construir-010143/app/docs/RELATO.md:7112) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:769) |
| **D4** | O selo usa AST e exige, para `update_fields=<nome>`, lista ou tupla literal atribuída uma vez no mesmo escopo, sem mutação. Na c3, `append` e atribuição só dentro de `if` passavam sem achado. [Selo](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:85) · [REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:316) |
| **D5** | O rejulgamento consulta o documento da raiz nos dois sentidos. A cura persiste via `save_base` o fim conferido pela cerca e preserva a duração legada da falta. O RED da c3 conferiu `26/09` e encontrou `28/09` gravado. [Comando](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/management/commands/rejulgar_status_por_tipo.py:21) · [Cura](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1923) · [REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:688) |

### Comandos de reprodução

Execute em `app/`. Cada chamada do módulo de mutantes deve retornar `1`. [Seletores e alvos](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66)

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest -q ponto.tests.test_atestado_f3_contrato

for caso in \
  red_c3_d1 d1_insert_achata d1_insert_ignora_raiz_relida d1_fixar_aceita_filha \
    d1_reabrir_achata d1_zip_le_filha d1_historico_le_filha \
  red_c3_d2 d2_so_linha_pedida \
  red_c3_d3 d3_permite_primeiro_arquivo \
  red_c3_d4 d4_append d4_atribuicao_em_ramo \
  red_c3_d5 d5_rejulga_le_filha d5_inverso_le_filha \
    d5_ignora_fim_cercado d5_minutos_usam_fim_novo
do
  python3 -m ponto.tests.mutantes_atestado_f3 "$caso"
done
```

### Arquivos alterados

- Portas e leitores. [ausencia.py:283](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:283) · [atestados_lote.py:99](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:99) · [dia_decidido.py:653](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/dia_decidido.py:653) · [views_atestado.py:30](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:30) · [painel_ausencia.html:2](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:2) · [ausencia_tags.py:6](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/templatetags/ausencia_tags.py:6) · [api/views.py:3087](/home/ronald/wt-codex-o30-f3-construir-010143/app/api/views.py:3087) · [rejulgar_status_por_tipo.py:21](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/management/commands/rejulgar_status_por_tipo.py:21)
- Censo e testes. [censo_escritas.py:399](/home/ronald/wt-codex-o30-f3-construir-010143/app/core/censo_escritas.py:399) · [test_lote7_selo_escritor_acorda.py:37](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:37) · [test_atestado_f3_contrato.py:85](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:85) · [test_atestado_f3.py:151](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:151) · [test_anexar_ausencia_colab.py:74](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_anexar_ausencia_colab.py:74) · [mutantes_atestado_f3.py:66](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66) [nao verificado]

**Entrega protegida.** `servir_documento_ausencia` usa `a.documento_referencia or a`, suficiente sob a invariante de um elo. A sonda fornecida registra **0 cadeias legadas** de profundidade ≥ 2. Produção [nao verificado]. [ponto/views.py:2147](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2147)

**Fila.** Itens 9, 12 e 14 do `r_v3.txt`. Nome do arquivo no LogAuditoria e na ficha admin, storage sem tenant, vigência aberta anterior ao recorte, `TIPOS_EXIGEM_DOC` cru, JSON E4b, fuso de `decidido_em`, prazo de reabrir, cron de prazo sem releitura e mês civil como padrão do lote. A máscara da sombra fica com a sessão. [Anexo fornecido, itens 9, 12 e 14]

_8349 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
