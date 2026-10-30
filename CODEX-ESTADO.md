# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 12:57** · publicado em **30/10/2026 13:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 12:57 · sessao 8ab81fa72246 · O261-VETO-A2 · verificador · origem=Code

1. **Não há garantia absoluta: resta um escape concorrente por `analisar()`.** O detector carrega chamados vivos e os entrega à fábrica; esta pode chamar `ch.analisar()` usando a instância antiga. Se outro escritor encerrar o chamado nesse intervalo, `analisar()` decide pelo estado em memória e grava `em_analise` sem consultar veto nem reler o banco. É um caminho automático que permanece fora de `reabrir()`; ocorrência em produção **[não verificado]**. (`app/ponto/management/commands/detectar_ausencias.py:790–796`; `app/chamados/services/disputa_emissao.py:971–981`; `app/chamados/models.py:566–572`, `:192`, `:231`)

   O escape obrigatório do callback está coberto: o receiver usa usuário de sistema, chama o resolver, e este executa `_pai.reabrir()` com o padrão `humano=False`; o veto retorna antes da escrita de estado e da ressurreição de perguntas. (`app/chamados/signals.py:89–92`; `app/chamados/services/disputa_emissao.py:1601–1608`; `app/chamados/models.py:1090–1093`, `:1143–1146`, `:1185–1194`)

   No percurso sequencial com estado atual, os demais eventos automáticos do motor não fazem encerrado → vivo; essa transição pertence a `REABRIR`. (`app/chamados/catalogo/motor.py:110–117`)

2. **sem achado nas três cadeias humanas atuais.** Desarquivar exige login, POST, permissão e acesso à área; reabrir disputa exige autorização sobre o chamado, staff/admin_ti, motivo e justificativa; reabrir competência exige login, POST, `reabrir_periodo` e motivo, repassando `request.user` até o serviço. (`app/chamados/views_arquivar.py:51–68`; `app/chamados/services/fio.py:552–577`; `app/chamados/services/acoes_disputa.py:242–266`; `app/ponto/views.py:1520–1544`; `app/ponto/services/fechamento.py:1415–1416`; `app/chamados/services/competencia_trancada.py:258`)

   Há uma fragilidade local, sem chamador automático demonstrado: o serviço de competência aceita `autor=None`, substitui por usuário de sistema e mesmo assim passa `humano=True`; portanto a classificação humana depende da cadeia acima. Uso automático externo **[não verificado]**. (`app/chamados/services/competencia_trancada.py:228`, `:239`, `:258`)

3. **sem achado nos leitores.** `reabrir()` trata `veto_do_dia` antes de qualquer uso do segundo elemento como chamado. `reabrir_orfas_enterradas` desempacota `_irmao`, mas não o utiliza: qualquer motivo provoca recusa e `continue`. (`app/chamados/models.py:1143–1146`, `:1164–1178`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:105–111`)

4. **Há contraexemplos na deduplicação, inferidos do código; ocorrência em produção [não verificado].**

   **Duplicação concorrente:** duas chamadas diretas podem consultar antes de qualquer uma inserir e ambas registrar a mesma recusa. O helper não serializa consulta/inserção, e o modelo não declara unicidade dessa chave. (`app/chamados/models.py:925–937`; `app/core/observ.py:133–138`; `app/core/models.py:309–316`)

   **Linha extra sem passagem daquele dia:** no nascimento, basta nascer chamado de outro dia do mesmo colaborador/módulo; faltam filtros de dia/célula em `_passou`. Na reabertura, basta renovar o SLA mantendo o chamado encerrado: isso altera `atualizado_em` e habilita outra linha. (`app/chamados/models.py:931–934`, `:378–385`)

   **Ciclo calado por concorrência:** uma recusa consulta a trilha e fica suspensa; ocorre nascimento permitido; a recusa antiga grava sua linha depois. Quando o veto volta, o nascimento fica anterior à última linha e não provoca novo registro. A comparação usa o horário da inserção tardia, sem serialização com o nascimento. (`app/chamados/models.py:925–937`; `app/core/models.py:307`)

   **Custo do helper:** um `SELECT`; existindo linha anterior, mais um `EXISTS`; havendo registro, mais um `INSERT`, além do controle transacional de `evento()`. Os índices declarados não cobrem a chave JSON com a ordenação utilizada; plano e latência reais **[não verificado]**. (`app/chamados/models.py:925–937`; `app/core/observ.py:133–138`; `app/core/models.py:313–316`)

5. **sem achado quanto a novo chamado, nota ou pergunta proibidos.** O novo ramo de veto escreve auditoria e retorna `False` antes dos efeitos de reabertura. O juiz consultado retorna apenas o motivo; a pergunta e as notas administrativas aparecem como contexto preexistente no diff fornecido. (`app/chamados/models.py:1143–1146`, `:937–941`, `:1185–1194`; `app/ponto/services/furos_vetados.py:27–44`; `app/chamados/services/acoes_disputa.py:259–270`; `app/chamados/services/competencia_trancada.py:258–260`)

## 30/10 12:55 · sessao 9ed2fa415911 · - · - · origem=Ronald (tmux)

3. **Sem achado.** `reabrir()` trata `veto_do_dia` antes dos ramos que usam `_irmao` como chamado; portanto a tupla do veto não chega a `_irmao.pk` (`app/chamados/models.py:1143`, `app/chamados/models.py:1144`, `app/chamados/models.py:1164`). `reabrir_orfas_enterradas` desempacota o segundo elemento mas não o usa: qualquer `motivo` provoca `PAI_RECUSA` e `continue` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:107`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:111`).

4. **Há contraexemplos de duplicação e de recusa calada, inferidos do código; ocorrência em produção [não verificado].**

- **Duplicação concorrente:** duas chamadas diretas a `criar()` ou `reabrir()` podem ler a mesma última linha antes de qualquer delas inserir; o helper não serializa consulta e inserção, e `LogAuditoria` não possui unicidade dessa chave (`app/chamados/models.py:925`, `app/chamados/models.py:937`, `app/core/observ.py:133`, `app/core/models.py:313`). O bloqueio do colaborador existe em `abrir()`, mas não cobre esses dois caminhos diretos (`app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/models.py:818`, `app/chamados/models.py:1115`).
- **Duplicação sem passagem pela porta:** após uma recusa de reabertura, basta renovar o prazo do chamado, mantendo-o encerrado; `definir_prazo_sla()` muda `atualizado_em`, e a próxima recusa considera isso `_passou=True` (`app/chamados/models.py:378`, `app/chamados/models.py:385`, `app/chamados/models.py:931`). Existe chamada a essa renovação em `recobrar_questionario()` sem guarda do estado do chamado naquele ponto (`app/chamados/services/disputa_emissao.py:1448`, `app/chamados/services/disputa_emissao.py:1450`).
- **Nascimento de outro dia:** a prova de passagem filtra somente colaborador, módulo e criação posterior; um chamado de outro dia/célula torna a próxima recusa da chave original uma linha nova, sem provar mudança daquele dia vetado (`app/chamados/models.py:925`, `app/chamados/models.py:933`).
- **Recusa calada por interleaving:** uma recusa pode consultar a trilha, ficar suspensa, ocorrer nascimento permitido e só depois gravar sua linha; a próxima recusa compara o nascimento com o horário dessa linha tardia e o ignora por não ser posterior (`app/chamados/models.py:925`, `app/chamados/models.py:934`, `app/chamados/models.py:937`, `app/core/models.py:307`). É contraexemplo possível; reprodução/ocorrência [não verificado].
- **Custo do helper:** uma consulta da última linha; quando encontrada, mais um `EXISTS`; quando emite, mais um `INSERT`, com controle transacional de `evento()` e possível acesso ao usuário lazy (`app/chamados/models.py:925`, `app/chamados/models.py:929`, `app/chamados/models.py:937`, `app/core/observ.py:106`, `app/core/observ.py:133`). Os índices declarados de auditoria não cobrem a chave JSON nem sua ordenação; plano e latência reais [não verificado] (`app/core/models.py:313`).

## 30/10 12:55 · sessao 1f14b6ff2d74 · - · - · origem=Ronald (tmux)

Q1: o escape nomeado do callback está barrado: o receiver automático chama o resolver com usuário sistema (`app/chamados/signals.py:81-92`); o resolver chama `_pai.reabrir()` sem `humano=True` (`app/chamados/services/disputa_emissao.py:1601-1608`); o veto é primeira guarda e retorna `False` antes da escrita/ressuscitação (`app/chamados/models.py:1090-1093,1143-1146,1185-1194`).

Há ressalva à afirmação **“toda reabertura automática”**: `analisar()` consulta apenas o estado da instância e escreve `em_analise`, sem reler banco nem perguntar veto (`app/chamados/models.py:566-572`; `app/chamados/catalogo/motor.py:112`). Contraexemplo por concorrência: detector carrega `batida_ausente` vivo e o entrega à fábrica (`app/ponto/management/commands/detectar_ausencias.py:790-796`); outro escritor encerra; fábrica mantém a instância antiga e chama `ch.analisar()` (`app/chamados/services/disputa_emissao.py:971-981`), que grava vivo diretamente (`app/chamados/models.py:192,231`). `batida_ausente` pertence ao universo (`app/chamados/catalogo/furo_modulos.py:15`; `app/ponto/services/furos_vetados.py:74-81`). Essa possibilidade é anterior ao diff; ocorrência em produção [não verificado].

No percurso sequencial, os outros gates automáticos que chamam `_gravar_estado` não reabrem encerrado: `RECONCILIAR`, `ANALISAR`, `CANCELAR`, `SUPERAR` e `ENCERRAR` têm mapas sem transição de encerrado para vivo; só `REABRIR` tem essa transição (`app/chamados/catalogo/motor.py:110-117`). `transicionar()` pode reabrir, mas os usos encontrados com destino vivo vêm de requisição/ação humana (`app/chamados/services/acoes_chamado.py:313-315`; `app/chamados/services/fio.py:702-715`); não encontrei um escritor automático sequencial de estado encerrado para vivo por `update` fora do gate (`app/chamados/models.py:185-231,429-430`).

## 30/10 12:55 · sessao 0d8cfe19a610 · - · - · origem=Ronald (tmux)

2. **sem achado nas cadeias atuais da cópia.**

- Desarquivar: `login_required`, POST, `gerir_chamados` e recusa por área precedem `reabrir(humano=True)`; o usuário segue para a trilha e a nota (`app/chamados/views_arquivar.py:51`, `:54`, `:56`, `:60`, `:68`, `:75`, `:79`).
- Reabrir disputa: as duas entradas exigem login e gestor; o POST delega ao fio, que verifica `pode_agir_chamado`; o serviço exige `is_staff`/grupo `admin_ti`, motivo válido e justificativa, e passa `request.user` à reabertura administrativa (`app/chamados/views.py:658`, `:662`, `:667`, `:674`, `:868`, `:872`, `:951`; `app/chamados/services/fio.py:560`, `:575`; `app/chamados/services/acoes_disputa.py:242`, `:248`, `:250`, `:256`, `:266`).
- Competência: login, POST, permissão `reabrir_periodo` e motivo obrigatório precedem a chamada com `por=request.user`; o serviço de fechamento repassa esse usuário como `autor` ao serviço que usa `humano=True` (`app/ponto/views.py:1520`, `:1523`, `:1525`, `:1537`, `:1544`; `app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:258`).
- Não localizei chamada automática nessas cadeias; uso dinâmico externo é **[não verificado]**. A função de competência isoladamente não exige humano: aceita `autor=None` e adota usuário de sistema; isso constitui possibilidade de uso futuro, não um chamador automático demonstrado (`app/chamados/services/competencia_trancada.py:228`, `:239`).

5. **sem achado no diff fornecido quanto a novo chamado, nota ou pergunta.** O ramo novo de recusa chama somente a trilha e retorna `False`; a trilha escreve `LogAuditoria` (`app/chamados/models.py:1143`, `:1145`, `:1146`, `:937`; `app/core/observ.py:133`). A pergunta e a nota da reabertura administrativa, assim como a nota da competência e a nota do resgate, já aparecem como contexto anterior no diff fornecido; anterioridade histórica além desse anexo é **[não verificado]** (`app/chamados/services/acoes_disputa.py:259`, `:270`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/views_arquivar.py:79`).

Detalhe adicional de trilha: `via='reabrir'` produz ação `reabertura_recusada`, mas descrição “nascimento recusado (reabrir)”, porque o texto distingue somente `renascer` dos demais valores (`app/chamados/models.py:919`, `:938`).

## 30/10 12:41 · sessao b5030ad3bef7 · O261-VETO-A2 · verificador · origem=Code

1. **Não: o selo não cobre toda reabertura.** Ele reconhece apenas chamadas com a forma `.reabrir(...)`, registra a função e elimina repetições. Acrescentar outra reabertura dentro de função já classificada também passa sem alterar o inventário. (`app/chamados/tests/test_o261_veto_na_porta.py:251`, `:256`, `:275`, `:313`)

   Mutante plausível que escapa:

   ```python
   def automatico(ch):
       ressuscitar = ch.reabrir
       ressuscitar()
   ```

   A chamada usa `ast.Name`, enquanto o detector exige `ast.Attribute`; `getattr(ch, 'reabrir')()` também escapa. (`app/chamados/tests/test_o261_veto_na_porta.py:256`)

   **Existe caminho real omitido:** a resposta livre aceita `novo_status` e chama `transicionar`; o motor permite `resolvido → em_analise`, chegando a `_gravar_estado` sem passar por `.reabrir`. É uma reabertura humana que falta no censo anunciado como completo. (`app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:183`, `:260`)

   Existe ainda chamada indireta por alias: `fechamento.reabrir_periodo` chama `_reabrir_fila`, que reabre chamados internamente. O selo classifica o destino, sem verificar novos chamadores dele. (`app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:258`)

2. **As classes precisam destas ressalvas:**

   | Sítio | Conferência |
   |---|---|
   | `_renascer_por_premissa_viva` | **sem achado**: consulta o veto antes de reabrir. (`app/chamados/models.py:780`, `:784`) |
   | `desarquivar_chamado` | **sem achado**: ação da tela com permissão e tratamento da recusa. (`app/chamados/views_arquivar.py:51`, `:70`) |
   | `competencia_trancada.reabrir` | **Humano na cadeia atual**: POST, permissão e motivo → `reabrir_periodo` → serviço. A função isolada aceita `autor=None` e assume sistema; a classe não garante a origem de futuros chamadores. (`app/ponto/views.py:1523`, `:1525`, `:1536`, `:1544`; `app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:239`) |
   | `acao_reabrir_disputa_admin` | Humano procede: exige administrador, motivo e justificativa. **Há falha no resultado**, descrita abaixo. (`app/chamados/services/acoes_disputa.py:242`, `:245`, `:250`) |
   | `ponto.views.reabrir_colaborador` | **sem achado**: o objeto reaberto é `FechamentoMensal`. (`app/ponto/views.py:1439`, `:1453`; `app/ponto/services/fechamento.py:1317`) |
   | `sincronizar_adesao` | Automatismo, mas reabre `sem_previsao_escala`, fora dos cinco módulos vetados. (`app/chamados/services/adesao.py:74`, `:92`; `app/ponto/services/furos_vetados.py:79`) |
   | `detectar_ausencias.handle` | Esse sítio reabre `ancora_limbo`, também fora do universo. (`app/ponto/management/commands/detectar_ausencias.py:271`, `:301`; `app/ponto/services/furos_vetados.py:79`) |
   | `resolver_perguntas_por_chamado_resolvido` | **sem achado na classe**: reabertura direta provocada pelo processamento da resposta. (`app/chamados/services/disputa_emissao.py:1598`, `:1606`) |
   | `reconciliar_grade._emitir_furo` | **sem achado na classe**: pode reabrir diretamente o encerrado devolvido por `abrir`. (`app/ponto/management/commands/reconciliar_grade.py:499`, `:522`) |
   | `reabrir_orfas_enterradas.handle` | Lote com dry padrão e `--apply`; a classificação não demonstra execução autônoma por cron. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:48`, `:157`, `:161`; `app/config/crons.py:743`) |
   | `reabrir_chamados_ausencia_aberta.handle` | Também é lote dry/apply e reabre `ausencia_colab`, fora dos cinco módulos. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:19`, `:29`, `:49`; `app/ponto/services/furos_vetados.py:79`) |

   **Falha em `acao_reabrir_disputa_admin`:** abre a disputa, cria pergunta, ignora o retorno de `ch.reabrir()` e anuncia sucesso. Contraexemplo: pai encerrado pelo admin, com dia identificado e sem fato novo; `reabrir()` retorna `False`, deixando disputa aberta com pai encerrado. (`app/chamados/services/acoes_disputa.py:256`, `:259`, `:265`, `:295`; `app/chamados/models.py:1075`, `:1080`, `:1123`, `:1128`) Ocorrência em produção: `[nao verificado]`.

3. **sem achado de quebra de leitor/teste pela chave `via`.** O teste que exige uma linha repete apenas `abrir`, mantendo a mesma chave. Os contadores de recusa por célula absolvida usam outro método, cuja chave permanece intacta. (`app/chamados/tests/test_o261_veto_na_porta.py:430`, `:439`; `app/chamados/models.py:942`; `app/ponto/tests/test_o_outbox_celula.py:348`, `:544`)

   A cardinalidade muda intencionalmente: `abrir` e `criar` recusados para o mesmo objeto/módulo/dia/veto podem produzir duas linhas. A auditoria geral conta essas linhas; não exige unicidade por veto. (`app/chamados/models.py:733`, `:842`, `:923`; `app/core/views.py:705`, `:727`) Resultado da suíte: `[nao verificado]`.

4. **Eu retiraria dois itens da FILA para fechar o contrato declarado de A:**

   - **Item 5 — VETO-TRILHA-CICLO.** O método promete deduplicação “enquanto nada muda”, mas consulta todo o histórico. Contraexemplo: `criar` recusado → correção permite `criar` → mesmo veto retorna → novo `criar` recusado sem nova trilha. A primeira linha histórica suprime o novo ciclo. A chave `via` não cura isso. (`app/chamados/models.py:835`, `:840`, `:910`, `:923`, `:926`)

   - **Item 6 — VETO-VIGIA-NO-PLACAR**, para aceitar A com seu vigia operacional. Contraexemplo: chamado vivo em dia vetado, com célula `cobrado`. O vigia novo encontra esse chamado; o contador ligado ao placar consulta somente células `furo` e pode mostrar zero. Assim, a exceção admitida pelo Strangler pode ficar invisível no placar. (`app/ponto/services/furos_vetados.py:61`, `:95`, `:106`; `bin/placar_code.sh:209`) Existência desse estado em produção: `[nao verificado]`.

   **Item 9 continua pendente de pouso, embora esteja como MEDINDO.** O cron declara `--apply`; o comando percorre células passadas sem filtro de exportação; mudança no veto altera a impressão e pode chegar à lavra. Contraexemplo: célula exportada com impressão antiga, cujo veto agora muda, é rejulgada automaticamente. (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:49`, `:61`; `app/ponto/services/cartorio.py:329`, `:1083`, `:1092`, `:1409`) Quantidade afetada e resultado da medida anunciada: `[nao verificado]`.

## 30/10 12:39 · sessao 7a382b3d18df · - · - · origem=Ronald (tmux)

Classificações:

| Sítio | Veredito |
|---|---|
| `_renascer_por_premissa_viva` | **sem achado**: consulta veto antes de `reabrir`. `app/chamados/models.py:780-784`. |
| `desarquivar_chamado` | **sem achado**: POST autenticado, permissão e área; reabre encerramento por tranca e trata recusa. `app/chamados/views_arquivar.py:51-70`. |
| `competencia_trancada.reabrir` | **Humano na cadeia atual**, pois POST+RBAC+motivo da view chama `fechamento.reabrir_periodo`, que chama esse serviço. `app/ponto/views.py:1520-1544`; `app/ponto/services/fechamento.py:1415-1416`; `app/chamados/services/competencia_trancada.py:258`. **Não é garantia da função**: `autor=None` vira usuário sistema. `app/chamados/services/competencia_trancada.py:228,239`. |
| `acao_reabrir_disputa_admin` | **Humano procede**: exige staff/admin_ti, motivo e justificativa; fio delega à ação. `app/chamados/services/acoes_disputa.py:242-256`; `app/chamados/services/fio.py:575-577`. |
| `ponto.views.reabrir_colaborador` | **sem achado**: `_fech.reabrir(f)` recebe `FechamentoMensal`; a função do serviço só altera esse fechamento. `app/ponto/views.py:1439-1453`; `app/ponto/services/fechamento.py:1317-1323`. |
| `sincronizar_adesao` | Automatismo sem consulta ao veto, mas reabre **`sem_previsao_escala`**, fora do universo dos cinco. `app/chamados/services/adesao.py:74-92`; universo: `app/ponto/services/furos_vetados.py:74-81`, `app/chamados/catalogo/furo_modulos.py:15-16`, `app/chamados/reconciliador.py:336-337`. |
| `resolver_perguntas_por_chamado_resolvido` | **sem achado na classe**: resposta esperando consumo provoca reabertura direta do pai. `app/chamados/services/disputa_emissao.py:1598-1608`. |
| `detectar_ausencias.handle` | Automatismo sem consulta ao veto, mas esse sítio reabre **`ancora_limbo`**, fora dos cinco módulos. `app/ponto/management/commands/detectar_ausencias.py:271-301`; universo acima. |
| `reconciliar_grade._emitir_furo` | **sem achado na classe**: depois de `abrir`, se recebeu encerrado, chama `reabrir` diretamente; apenas `None` encerra o caminho. `app/ponto/management/commands/reconciliar_grade.py:499-522`. |
| `reabrir_orfas_enterradas.handle` | **Rótulo “automático” impreciso**: comando de uma vez, dry por padrão e `--apply`; reabre pai por critério do lote. `app/chamados/management/commands/reabrir_orfas_enterradas.py:1,48-49,157-166`; registrado fora de pipeline em `app/config/crons.py:743-750`. |
| `reabrir_chamados_ausencia_aberta.handle` | **Rótulo “automático” impreciso**: dry por padrão, `--apply` explícito, registrado como ato único; ainda reabre somente **`ausencia_colab`**, fora dos cinco. `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:19-20,28-49`; `app/config/crons.py:1394-1395`; universo acima. |

**Falha adicional concreta em `acao_reabrir_disputa_admin`:** classificação humana não garante reabertura efetiva do pai. A ação abre a disputa e cria pergunta **antes** de chamar `ch.reabrir()`, ignora o retorno e anuncia sucesso. `app/chamados/services/acoes_disputa.py:256-266,293-295`; gravação da disputa: `app/chamados/models.py:1773-1785`.

Contraexemplo estático: pai encerrado por admin, com data resolvida e sem fato novo; `recusa_da_reabertura` retorna `admin_sem_fato_novo` e `reabrir()` retorna `False`. A disputa já ficou aberta e a pergunta nova existe, mas o pai permanece encerrado. `app/chamados/models.py:1075-1080,1123-1128`; `app/chamados/services/acoes_disputa.py:256-266`. Ocorrência em produção: `[nao verificado]`.

## 30/10 12:39 · sessao 0414381eb7ad · - · - · origem=Ronald (tmux)

Q1 — **não acha toda reabertura; acha funções contendo a sintaxe `.reabrir(...)`**, sem resolver alvo, aliases ou fluxo entre funções. Também elimina multiplicidade por função (`any`) e pelo `set`; nova chamada dentro de função já classificada passa sem mudar o resultado. [app/chamados/tests/test_o261_veto_na_porta.py:251](app/chamados/tests/test_o261_veto_na_porta.py:251), [app/chamados/tests/test_o261_veto_na_porta.py:275](app/chamados/tests/test_o261_veto_na_porta.py:275).

**Contraexemplo existente:** `acoes_chamado` lê `novo_status` do POST e chama `chamado.transicionar(novo_status, request.user)` quando gestor; o motor permite `resolvido → em_analise`. Essa reabertura passa por `_aplicar → _gravar_estado`, sem `.reabrir` e sem classificação nesse selo. [app/chamados/services/acoes_chamado.py:287](app/chamados/services/acoes_chamado.py:287), [app/chamados/services/acoes_chamado.py:313](app/chamados/services/acoes_chamado.py:313), [app/chamados/catalogo/motor.py:102](app/chamados/catalogo/motor.py:102), [app/chamados/models.py:183](app/chamados/models.py:183), [app/chamados/models.py:260](app/chamados/models.py:260). Existe caminho equivalente no fio. [app/chamados/services/fio.py:713](app/chamados/services/fio.py:713).

**Mutante plausível que escapa:**

```python
def automatico(ch):
    ressuscitar = ch.reabrir
    ressuscitar()
```

O detector exige `ast.Call.func` do tipo `ast.Attribute` com `attr == 'reabrir'`; a chamada acima tem `ast.Name`. `getattr(ch, 'reabrir')()` e `ch._gravar_estado('em_analise')` também escapam pela mesma condição. [app/chamados/tests/test_o261_veto_na_porta.py:256](app/chamados/tests/test_o261_veto_na_porta.py:256). O núcleo aceita o estado recebido e o persiste; não consulta veto. [app/chamados/models.py:192](app/chamados/models.py:192), [app/chamados/models.py:231](app/chamados/models.py:231).

Uma lambda imediata contendo `ch.reabrir()` também escapa: `_corpo_proprio` exclui `Lambda`, enquanto `_funcoes` só registra `FunctionDef`/`AsyncFunctionDef`. [app/chamados/tests/test_o261_veto_na_porta.py:168](app/chamados/tests/test_o261_veto_na_porta.py:168), [app/chamados/tests/test_o261_veto_na_porta.py:182](app/chamados/tests/test_o261_veto_na_porta.py:182).

Há alias real de outra função que reabre internamente: `competencia_trancada.reabrir as _reabrir_fila` em fechamento; o chamador não aparece no censo, embora o destino apareça. [app/ponto/services/fechamento.py:1415](app/ponto/services/fechamento.py:1415), [app/chamados/services/competencia_trancada.py:258](app/chamados/services/competencia_trancada.py:258).

Executei apenas o AST extraído em memória com `python3 -B`: direto detectado; alias, `getattr`, lambda, núcleo e `transicionar` retornaram lista vazia. Resultado de execução sem arquivo persistido: `[nao verificado]`; as condições que determinam esses resultados estão nas linhas citadas acima. [nao verificado]

## 30/10 12:39 · sessao 0865afc7bf13 · - · - · origem=Ronald (tmux)

3. **sem achado de quebra de leitor/teste existente por `valor_depois__via`.** A deduplicação passa a distinguir `abrir` de `criar`; o JSON gravado continua incluindo `via` e os demais campos (`app/chamados/models.py:923-931`). O teste que exige uma linha repete somente `abrir`, portanto a chave nova mantém sua expectativa (`app/chamados/tests/test_o261_veto_na_porta.py:430-439`).

- **Mudança real de cardinalidade:** para o mesmo objeto/módulo/dia/veto, `abrir` seguido de `criar` agora registra duas linhas, pois as entradas passam respectivamente `via='abrir'` e `via='criar'` (`app/chamados/models.py:733`, `app/chamados/models.py:842`, `app/chamados/models.py:923-931`). Não há teste misto exigindo uma linha nas referências localizadas; resultado da execução da suíte: `[nao verificado]`.
- Os testes antigos que contam `nascimento_recusado` sobre célula absolvida exercitam `_trilha_da_recusa`, cuja chave continua módulo/veredito e não recebeu a mudança (`app/ponto/tests/test_o_outbox_celula.py:348-353`, `app/ponto/tests/test_o_outbox_celula.py:544-552`, `app/chamados/models.py:934-949`).
- O histórico do chamado consulta apenas `estado_chamado`, excluindo essas recusas (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86-95`). A tela geral de auditoria conta linhas reais e não pressupõe uma linha por veto; mostrará as duas operações separadas (`app/core/views.py:705-727`).

**FILA VETO-TRILHA-CICLO continua procedente.** Contraexemplo: veto por `VETO_REGUA` em `abrir` → correção permite nascimento → veto idêntico retorna e outro `abrir` é recusado. A nova recusa encontra a primeira linha histórica e retorna sem registro, pois o `exists()` não considera correção, intervalo ou último evento (`app/chamados/models.py:923-926`). Isso contraria a promessa “enquanto nada muda” do próprio método; não demonstra falha do bloqueio do nascimento, que continua retornando antes da criação (`app/chamados/models.py:910`, `app/chamados/models.py:730-735`).

## 30/10 12:07 · sessao 227f3905545f · O261-VETO-A2 · verificador · origem=Code

**REPROVADO para fechamento:** há reabertura automática que contorna o veto e o selo não a detecta; a promessa de cadastro para ATIVO sem vínculo também não está atendida nos fluxos examinados. (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/tests/test_o261_veto_na_porta.py:207`; `app/colaboradores/queries.py:64`)

Correspondência da cópia com HEAD/hash e base d2c37edd: [nao verificado]. Execução da bateria Django, ocorrências em produção, reversão prévia e hashes do gravado exportado: [nao verificado].

Sobre a **pergunta 1**, o registro declara `motivo_do_veto` como juiz; o lookup JUIZES.md manda prevalecer o registro vivo. (`app/core/juizes.py:248`; `app/docs/JUIZES.md:11`; `app/docs/JUIZES.md:14`)

A cadeia dos campos e valores afetados é esta:

| Campo/valor | Quem escreve ou determina | Quem lê no trecho afetado |
|---|---|---|
| Universo dos módulos | `FURO_MODULES` + `MODULOS_DE_TURNO`, unidos por `modulos_do_veto`. (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/reconciliador.py:336`; `app/ponto/services/furos_vetados.py:79`) | Porta, contador e teste dos cinco módulos. (`app/chamados/models.py:889`; `app/ponto/services/furos_vetados.py:95`; `app/chamados/tests/test_o261_veto_na_porta.py:384`) |
| `colaborador`, `modulo_origem`, `contexto_json`, `celula` do chamado | Nascimento por `abrir`/`criar`; `_celula_de` resolve a associação pelo dia. (`app/chamados/models.py:725`; `app/chamados/models.py:735`; `app/chamados/models.py:830`; `app/chamados/models.py:850`; `app/chamados/models.py:974`) | `_veto_da_regua` e, indiretamente, `dia_do_fato`. (`app/chamados/models.py:889`; `app/chamados/models.py:892`; `app/chamados/models.py:895`) |
| Dia do fato | `dia_do_fato(fortes=True)` lê primeiro `celula.data`, depois o contexto conforme o catálogo. (`app/chamados/juizes.py:78`; `app/chamados/juizes.py:89`; `app/chamados/juizes.py:92`) | Porta e contador usam essa mesma autoridade. (`app/chamados/models.py:892`; `app/ponto/services/furos_vetados.py:101`) |
| Vínculo do dia, inclusive `None` | `vinculo_do_emissor` delega a `escala_vigente`, que alimenta `vinculo_do_dia`; geradora e vigência continuam nessa cadeia. (`app/ponto/services/furos_vetados.py:23`; `app/escala/servico_jornada.py:38`; `app/escala/servico_jornada.py:55`; `app/escala/alimentacao.py:196`) | Juiz, cartório, c2, passivo e contador. (`app/ponto/services/furos_vetados.py:65`; `app/ponto/services/cartorio.py:1013`; `app/chamados/reconciliador.py:854`; `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_vetados.py:106`) |
| Template/marcos usados para degeneração | `TipoEscala.marcos_do_dia`; nas fixtures novas, `dar_vinculo` grava template, posto e vínculo ativo desde `desde`. (`app/escala/models.py:326`; `app/chamados/tests/_vinculo_do_dia.py:14`; `app/chamados/tests/_vinculo_do_dia.py:23`) | `regua_degenerada`, chamado pelo juiz e pela função com efeito de cadastro. (`app/escala/regua_defesa.py:23`; `app/escala/regua_defesa.py:26`; `app/ponto/services/furos_vetados.py:40`; `app/escala/regua_defesa.py:46`) |
| `regua_avaliada_em`, confiança, dias baixos e corroborada | Avaliação/corroborar/desfazer usam `gravar_regua`, que persiste os campos. (`app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:719`; `app/escala/regua_defesa.py:765`; `app/colaboradores/services/vinculo.py:911`) | Veto de suspeita lê avaliação/faixa/dias baixos; o relatório também lê `regua_corroborada_em`. (`app/escala/regua_defesa.py:560`; `app/escala/regua_defesa.py:644`; `app/ponto/services/furos_vetados.py:70`) |
| `VETO_DEGENERADA` / `VETO_REGUA` / `None` | Retorno do juiz; não é campo persistido por ele. (`app/ponto/services/furos_vetados.py:40`) | Leitores discriminados na tabela seguinte. (`app/chamados/models.py:896`; `app/ponto/services/cartorio.py:1013`) |
| `status_local` e vitalidade | Nascimento nas duas portas; transição em `_gravar_estado`. As duas escritas históricas da 0038 permanecem na allowlist. (`app/chamados/models.py:735`; `app/chamados/models.py:850`; `app/chamados/models.py:192`; `app/chamados/migrations/0038_a3_registrado_backfill.py:10`; `app/chamados/migrations/0038_a3_registrado_backfill.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:40`) | Dedup, emissores, c2 e contador. (`app/chamados/models.py:685`; `app/ponto/management/commands/detectar_ausencias.py:734`; `app/ponto/management/commands/reconciliar_grade.py:521`; `app/chamados/reconciliador.py:888`; `app/ponto/services/furos_vetados.py:95`) |
| Trilha: ação/modelo/objeto, `modulo_origem`, `dia`, `veto`, `via`, colaborador | `_trilha_do_veto` monta; `evento` grava `LogAuditoria`. (`app/chamados/models.py:915`; `app/chamados/models.py:924`; `app/core/observ.py:134`) | A própria deduplicação e os testes novos consultam o veto persistido. (`app/chamados/models.py:921`; `app/chamados/tests/test_o261_veto_na_porta.py:362`) |
| Impressão e carimbos da célula | Cartório calcula a impressão; `carimbar` persiste impressão, julgada/insumos, ausência e entrada/saída; `lavrar_veredito` escreve veredito/protestos/ata. (`app/ponto/services/cartorio.py:1083`; `app/ponto/portas/celula.py:721`; `app/ponto/portas/celula.py:689`) | A igualdade da impressão decide o pulo; as fontes de células/protestos alimentam os dois leitores de furos. (`app/ponto/services/cartorio.py:1092`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/furos_sem_cobranca.py:61`) |
| Achados e contadores | `furos_vetados_por_regua` e `chamados_vivos_em_dia_vetado` montam os resultados em memória. (`app/ponto/services/furos_vetados.py:69`; `app/ponto/services/furos_vetados.py:108`) | Comando de relatório; desfazer corroboracão filtra pelo vínculo; placar extrai somente o contador antigo. (`app/ponto/management/commands/furos_vetados_por_regua.py:29`; `app/ponto/management/commands/furos_vetados_por_regua.py:44`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`; `bin/placar_code.sh:210`) |

Os leitores de produção do **juiz**, incluindo aliases, têm estes efeitos quando `ec=None` passa a devolver `VETO_DEGENERADA`:

| Leitor | Mudança |
|---|---|
| Porta `_veto_da_regua` | Recusa nascimento por `abrir`/`criar` e renascimento por `_renascer_por_premissa_viva`, havendo dia forte. (`app/chamados/models.py:731`; `app/chamados/models.py:778`; `app/chamados/models.py:838`; `app/chamados/models.py:893`) |
| Cartório `_veto_do_dia` | Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal e pode provocar relavratura. Isso não determina, sozinho, qual será o novo veredito. (`app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1108`) |
| `furos_vetados_por_regua` | Passa a incluir furo sem vínculo, retornando vínculo/corroborada `None`; o comando conta o furo sem contar um vínculo inexistente. (`app/ponto/services/furos_vetados.py:65`; `app/ponto/services/furos_vetados.py:69`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`) |
| `furos_sem_cobranca.medir` | Classifica como porta `veto`, fora do total esperado zero e fora da cobrança pelo reparador. (`app/ponto/services/furos_sem_cobranca.py:30`; `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125`) |
| c2 | Barra nascer/renascer; a costura em chamado já vivo continua dispensando esse veto. (`app/chamados/reconciliador.py:808`; `app/chamados/reconciliador.py:852`; `app/chamados/reconciliador.py:855`) |
| `chamados_vivos_em_dia_vetado` | Passa a listar o vivo sem vínculo, mantendo os chamados sem dia em contador separado. (`app/ponto/services/furos_vetados.py:101`; `app/ponto/services/furos_vetados.py:106`) |

Detector e grade ainda usam as funções antigas antes de pedir a porta; são as cópias nominalmente toleradas pelo lote A. O retroativo lê indiretamente o juiz pelo dry `veto_no_nascimento`, mas já descarta `ec=None` antes desse ponto. (`app/ponto/management/commands/detectar_ausencias.py:600`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`)

Sobre a **pergunta 2: sem achado de intenção “sem vínculo/sem escala” mascarada pela migração**. O caso explícito “sem escala” de F4 não passa pelo helper que acrescenta vínculo; o caso novo sem vínculo cria outro colaborador e exige `(None, False)`. “Tranca sem cadastro” testa a exclusão dos avisos de cadastro da tranca. (`app/chamados/tests/test_selo_f4_fatia4.py:67`; `app/chamados/tests/test_selo_f4_fatia4.py:131`; `app/chamados/tests/test_o261_veto_na_porta.py:395`; `app/chamados/tests/test_tranca_sem_cadastro.py:39`; `app/chamados/tests/test_tranca_sem_cadastro.py:50`)

Há, entretanto, **prova insuficiente de nascimento em dois testes tocados**, preexistente à inserção do helper no diff fornecido: ambos prometem nascimento no nome, mas somente chamam `_ctx_c2` e verificam o dicionário. Contraexemplo: a porta recusar todo nascimento enquanto `_ctx_c2` continua retornando aquele contexto; as asserções continuam satisfeitas. (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`; `app/chamados/tests/test_bug125_cobranca_cega.py:176`)

Sobre a **pergunta 3, há achado: o escritor existe, mas ATIVO sem vínculo não o alcança pelos fluxos normais examinados**.

`vetar_cobranca_se_degenerada` aceita `None` e abre `regua_degenerada` com template ausente; a nova porta não veta esse módulo. Seus chamadores são detector e grade. (`app/escala/regua_defesa.py:45`; `app/escala/regua_defesa.py:51`; `app/chamados/models.py:889`; `app/ponto/management/commands/detectar_ausencias.py:600`; `app/ponto/management/commands/reconciliar_grade.py:487`)

O detector exige vínculo cobrindo hoje e ainda pula `ec=None`; a grade descarta escala ausente antes da emissão; o cartório também exige vínculo antes de chamar o detector. **Contraexemplo:** ATIVO com batidas, empresa em rollout e nenhum vínculo cobrindo hoje/ontem: não chega ao escritor de cadastro; uma tentativa de nascimento na nova porta é recusada pelo juiz puro, sem abrir cadastro. Essa ausência de alcance já está nos trechos anteriores ao diff; não é uma regressão nova demonstrada. (`app/colaboradores/queries.py:64`; `app/ponto/management/commands/detectar_ausencias.py:140`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/services/cartorio.py:1348`; `app/ponto/services/cartorio.py:1446`; `app/ponto/services/furos_vetados.py:40`)

O teste de cadastro prova a função isolada com vínculo e template degenerado; o outro só procura o nome da função no texto dos emissores. Não prova ATIVO sem vínculo chegando automaticamente ao escritor. (`app/ponto/tests/test_f3_regua_defesa.py:49`; `app/ponto/tests/test_f3_regua_defesa.py:53`; `app/ponto/tests/test_f3_regua_defesa.py:61`)

Sobre a **pergunta 4, há dois escapes de reabertura**:

- **HX-RESPOSTA-VISIVEL automático:** o signal de fechamento chama o resolver, que chama `_pai.reabrir()` diretamente. O veto novo está no renascer; `reabrir()` consulta outras recusas e grava a vitalidade sem perguntar esse veto. **Contraexemplo:** pai legado `entrada_bloqueada_turno_aberto`, encerrado por sistema, dia forte, sem vínculo, sem irmão/agrupamento, disputa aberta com resposta pendente creditada ao pai após o destino encerrar. Esse módulo pode ficar fora da guarda de premissa de furo; o pai reabre apesar de `VETO_DEGENERADA`. Ocorrência em produção: [nao verificado]. (`app/chamados/signals.py:89`; `app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:778`; `app/chamados/models.py:984`; `app/chamados/models.py:1078`; `app/chamados/models.py:1158`; `app/chamados/juizes.py:1471`)

- **Grade pode desfazer a recusa do renascimento:** `abrir()` devolve o existente encerrado mesmo quando o renascer recusou; a grade então chama `ch.reabrir()`. A guarda nova `ch is None` não cobre esse retorno. **Contraexemplo:** vínculo saudável pré-carregado pela grade; cadastro muda antes da releitura da porta; a cópia antiga passa, a porta veta o renascimento e devolve o encerrado, e a grade tenta reabrir sem veto. Mantendo célula furada e demais guardas permissivas, esse caminho contorna a recusa. Ocorrência em produção: [nao verificado]. (`app/ponto/management/commands/reconciliar_grade.py:189`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/chamados/models.py:685`; `app/chamados/models.py:896`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/models.py:1039`)

Há ainda estes achados nos sítios criados/tocados:

- **Selo sem cobertura dos escapes acima.** O varredor reconhece vivificação por gestores e construtor+save; não classifica `pai.reabrir()` como escrita. Contraexemplo pelo próprio varredor: `def automatico(pai): pai.reabrir()` produz listas vazias. As allowlists podem continuar verdes com esse escape presente; o MORDE atual não testa essa forma. (`app/chamados/tests/test_o261_veto_na_porta.py:29`; `app/chamados/tests/test_o261_veto_na_porta.py:207`; `app/chamados/tests/test_o261_veto_na_porta.py:209`; `app/chamados/tests/test_o261_veto_na_porta.py:264`)

- **Impressão de `nunca_bateu` não recebe o novo veto.** O ramo calcula `imp0` sem `veto`, compara a igualdade e retorna antes do leitor do juiz. Contraexemplo: célula já carimbada de colaborador sem batidas e sem vínculo, demais insumos iguais; a mudança para `VETO_DEGENERADA` não altera sua impressão e ela continua pulada. (`app/ponto/services/cartorio.py:907`; `app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1003`; `app/ponto/services/cartorio.py:1013`)

- **Trilha deduplicada além do contrato declarado.** A promessa é por operação e “enquanto nada muda”, mas o filtro omite `via` e pesquisa toda a história. Contraexemplo: `abrir` recusado, depois `criar` recusado para a mesma chave: a segunda operação não deixa linha; corrigir e posteriormente voltar ao mesmo veto também encontra a recusa antiga. (`app/chamados/models.py:908`; `app/chamados/models.py:921`; `app/chamados/models.py:927`)

- **Contador novo não chega ao placar existente.** O comando imprime `chamados_vivos_em_dia_vetado`, porém o consumidor captura apenas `furos_vetados_por_regua`. Contraexemplo: surge um vivo vetado sem aumentar o contador antigo; a saída mostrada pelo placar permanece igual. (`app/ponto/management/commands/furos_vetados_por_regua.py:52`; `bin/placar_code.sh:209`)

A relavratura **não fica restrita à competência aberta pelo comando existente**: ele percorre todas as células passadas; no ramo normal, a impressão nova pode romper o pulo e chegar ao carimbo. Isso demonstra alteração possível da célula histórica, não alteração comprovada do gravado financeiro — o hash desse gravado mede `FechamentoMensal`. Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/regen_exportada.py:62`)

Juiz paralelo novo, derivação paralela do vínculo/universo, relógio novo fora da borda e inversão nova de trava: **sem achado** nos trechos acrescentados; a trava do colaborador precede a leitura/dedup, e o veto delega às autoridades citadas. Custo efetivo no request e pré-requisitos operacionais de pouso: [nao verificado]. (`app/chamados/models.py:666`; `app/chamados/models.py:888`; `app/ponto/services/furos_vetados.py:79`)

## 30/10 12:04 · sessao dd342ba74c7a · O261-VETO-A2 · leitura · origem=Code

**Sim: o cron configurado pode relavrar células antigas, inclusive de competência exportada ou trancada. A seleção não tem piso de competência nem guarda de exportação/tranca.** (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/portas/celula.py:647-729`)

Correspondência desta cópia com o hash informado, instalação/execução efetiva dos crons e quantidade de células afetadas em produção: **[nao verificado]**.

**1) Caminho de rejulgamento e comandos**

- No ramo normal, o cartório consulta `motivo_do_veto` pelo vínculo do dia, inclui `veto:VETO_DEGENERADA` na impressão e compara com a gravada. A impressão diferente permite novo julgamento por `classificar_dia`, seguido de lavra e carimbo. (`app/ponto/services/cartorio.py:1011-1014`, `:1083-1108`, `:1406-1425`; `app/ponto/services/cartorio.py:329-330`)
- **Exceção:** o ramo sem adesão, `nunca_bateu`, calcula a impressão sem passar o veto. Portanto, essa alteração isolada não muda necessariamente a impressão de toda célula sem vínculo. (`app/ponto/services/cartorio.py:907`, `:941-943`)
- **Cron:** `processar_cartorio --apply`, diariamente às **06:27 / 06:29 / 06:31**, respectivamente para empresas **2 / 3 / 4**, pela expansão com passo de dois minutos. Varre **todas as células existentes com `data < hoje`**, sem data inicial e sem filtro de situação do colaborador. (`app/config/crons.py:17`, `:29-33`, `:597`, `:1785-1794`; `app/ponto/management/commands/processar_cartorio.py:49-65`)
- Também existe `vigia_de_hora --apply`, a cada dez minutos, cobrindo candidatas de hoje e certos turnos noturnos de ontem, pelo mesmo `julgar_celula`. Eventos de batida, ausência, justificativa e criação/mudança de status de chamado também acionam julgamento unitário. (`app/config/crons.py:348-349`; `app/ponto/services/vigia_de_hora.py:108-130`; `app/ponto/signals.py:131-165`; `app/chamados/signals.py:164-180`)

Há dois comandos relevantes:

| Comando | Argumentos e comportamento |
|---|---|
| `processar_cartorio` | `--empresa N` obrigatório; `--limite N`; `--forcar` ignora impressão igual. Sem `--apply`, julga/reporta sem persistir; com ele, grava. (`app/ponto/management/commands/processar_cartorio.py:25-29`, `:65`) |
| `celula_veredito_velho` | `--mes M --ano A`, opcionais `--empresa N`, `--json`, `--sem-recalculo`, `--so-recalculo`; aplicação por `--aplicar --antes ARQUIVO`. Recorta a janela da competência por empresa até ontem. (`app/ponto/management/commands/celula_veredito_velho.py:37-54`, `:75-80`) |

No segundo, sem `--aplicar`, o ensaio força a relavratura e, por padrão, recalcula o fechamento dentro de transação com **rollback**; `--sem-recalculo` evita esse recálculo. Com `--aplicar`, exige snapshot `--antes` e **desliga o recálculo**, gravando a relavratura forçada. (`app/ponto/management/commands/celula_veredito_velho.py:90-114`, `:133-159`)

**2) Exportada ou trancada: alcança; não há guarda nessa relavratura**

O lote e as portas de lavra/carimbo não consultam `ExportacaoDominio` nem `PeriodoFechado`. `--forcar` também não introduz proteção de competência. (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/services/cartorio.py:1092-1097`; `app/ponto/portas/celula.py:647-729`)

Existe proteção da **decisão administrativa**: `veredito_via='admin'` preserva o veredito humano, mas permite atualizar a ata e depois o carimbo. Existe ainda `PISO_EMISSAO='2026-09'`, que restringe emissão automática de chamados, sem impedir a relavratura da célula. (`app/ponto/portas/celula.py:671-677`, `:708-728`; `app/ponto/services/cartorio.py:190`, `:1319-1327`)

**3) Dinheiro gravado e recálculo**

A relavratura direta escreve campos de `CelulaDia` e pode emitir `EventoCelula`; **não regrava por si só `FechamentoMensal`, `DiaPago` ou o conteúdo/hash de uma exportação existente**. (`app/ponto/services/cartorio.py:1406-1425`; `app/ponto/portas/celula.py:623-642`, `:647-729`)

Quem persiste os valores calculados é `recalcular_fechamento_mes`: salva/atualiza `FechamentoMensal` e chama a lavratura de `DiaPago`, que substitui suas linhas por colaborador/competência/versão. (`app/ponto/services/fechamento.py:1105-1169`; `app/ponto/services/dia_pago.py:357-358`)

Os gatilhos incluem:

- Botão **Recalcular** e command `recalcular_fechamento --mes M --ano A [--empresa N] [--colabs IDs] --apply`; sem `--apply`, esse command apenas fotografa o gravado. Não há cron desse comando no registro. (`app/ponto/views.py:1227-1253`; `app/ponto/management/commands/recalcular_fechamento.py:77-120`; `app/config/crons.py:1071-1078`)
- `recalcular_por_evento`, que agenda em `on_commit` o recálculo do colaborador na competência do fato, por exemplo após registrar batida. (`app/ponto/services/fechamento.py:1766-1792`; `app/ponto/registro_batida.py:160`, `:175-178`)

**Há uma ressalva indireta:** a lavra pode alimentar o consumidor de eventos, que pode fechar disputa; esse fechamento pode retomar regularização já confirmada, criar batida e assim disparar recálculo. A existência desse encadeamento nos casos afetados é **[nao verificado]**. (`app/chamados/reconciliador.py:1296-1305`, `:1118-1120`; `app/chamados/signals.py:97-121`; `app/chamados/services/regularizacao_ext.py:218-224`, `:76-77`; `app/ponto/registro_batida.py:160`)

No escritor financeiro, há **guarda específica para 09/2026 nas empresas exportadas**; exportação não é bloqueio genérico. (`app/ponto/services/fechamento.py:36-43`, `:523-559`)

**Um próximo TXT pode mudar sem recálculo previamente persistido:** gerar TXT calcula o fechamento em modo leitura, monta suas rubricas e cria outro `ExportacaoDominio` com conteúdo/hash próprios; o registro anterior permanece. (`app/folha/views.py:207-212`, `:268-299`; `app/folha/models.py:174-187`)

**4) Campos gravados e reversão**

| Operação | Campos |
|---|---|
| Lavra | `veredito`, `veredito_via`, `veredito_em`, `protestos`; `ata` quando diferente. (`app/ponto/portas/celula.py:669-695`) |
| Carimbo | `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em`, junto dos campos lavrados. (`app/ponto/portas/celula.py:721-728`) |

O julgamento unitário também pode atualizar `ata.precedencia`. Essa relavratura não regenera DNA/origem nem produz `dna_anterior`. (`app/ponto/services/cartorio.py:1563-1568`, `:719-724`)

Há `LogAuditoria`, ação `veredito_celula`, com antes/depois **somente de veredito, via e protestos**. A trilha é best-effort e não constitui reversão completa de ata, impressão, insumos e carimbos. `EventoCelula` também não guarda antes/depois. (`app/ponto/portas/celula.py:657`, `:699-702`; `app/core/observ.py:94-105`; `app/escala/models.py:1800-1815`)

O snapshot exigido por `celula_veredito_velho --aplicar` guarda decisão, protestos, ata, impressão e datas de veredito/julgamento, mas **omite `ausencia`, `tem_entrada`, `tem_saida` e `insumos_em`**: sua cobertura de reversão é parcial. Snapshot efetivamente produzido para esta obra: **[nao verificado]**. (`app/ponto/management/commands/celula_veredito_velho.py:100-114`; `app/ponto/portas/celula.py:721-728`)

## 30/10 12:04 · sessao 1415e2588123 · - · - · origem=Ronald (tmux)

**Q3 — relavratura e dinheiro**

- **Relavrar a célula não chama diretamente o recálculo financeiro.** O cartório lavra veredito/ata e carimba os insumos; as portas persistem campos da célula, auditoria e `EventoCelula`. Referências: `app/ponto/services/cartorio.py:1406-1425`; `app/ponto/portas/celula.py:634-642,647-729`.
- O consumidor desse evento chama retratação dos chamados e encerramento de perguntas órfãs; não chama diretamente `recalcular_por_evento`. Referência: `app/chamados/reconciliador.py:1296-1305`.
- **Não prometer ausência absoluta de efeito indireto:** o consumidor pode fechar disputa (`app/chamados/reconciliador.py:1118-1120`); seu signal agenda retomada de regularizações anteriormente confirmadas (`app/chamados/signals.py:97-121`); a retomada pode criar batida pela porta (`app/chamados/services/regularizacao_ext.py:218-225,70-81`), que dispara recálculo (`app/ponto/registro_batida.py:157-160,175-178`). Existência dessa situação nos dados fornecidos: **[nao verificado]**.

**Quem recalcula e quando**

- `recalcular_por_evento` agenda em `transaction.on_commit` o recálculo de **um colaborador na competência do fato**, por `competencia_do_fato`; captura falhas sem desfazer o fato. Referência: `app/ponto/services/fechamento.py:1766-1795`.
- Seus gatilhos de produção encontrados são: batida (`app/ponto/registro_batida.py:175-178`); decisão de HE (`app/ponto/portas/he.py:201-202`); folga aceita e resposta validada (`app/chamados/services/validacao.py:323-324,399-401`); correção de vínculo, uma chamada por competência tocada (`app/ponto/portas/celula.py:384-399`).
- Também há botão POST com permissão `editar_folha`, chamando o mesmo serviço (`app/ponto/views.py:1227-1253`), e command `recalcular_fechamento --mes --ano [--empresa] [--colabs ids] [--apply] [--motivo-exportada texto]`; sem `--apply`, apenas fotografa o gravado, sem simular recálculo (`app/ponto/management/commands/recalcular_fechamento.py:77-120`).
- O registro declara que `recalcular_fechamento` não tem cron deliberadamente (`app/config/crons.py:1071-1078`). O cron **`pre_fechamento --apply`, às 05:25**, escreve apenas Pauta DP; lê bloqueios e marcos, sem chamar o recálculo financeiro (`app/config/crons.py:387`; `app/folha/management/commands/pre_fechamento.py:36-77,85-112`). Comentários antigos dizendo que “o cron rega o gravado” não correspondem a um cron financeiro identificado nesses caminhos (`app/ponto/registro_batida.py:170-173` versus `app/config/crons.py:1071-1078`).

**O que o recálculo financeiro escreve**

- `FechamentoMensal`: salva os campos derivados quando aberto; quando aprovado, atualiza os campos calculados por `update`, preservando o carimbo de aprovação. Referência: `app/ponto/services/fechamento.py:1105-1158`.
- `DiaPago`: o mesmo serviço chama `dia_pago.lavrar`, com os resultados usados no fechamento; essa lavratura substitui as linhas do colaborador/competência/versão. Referências: `app/ponto/services/fechamento.py:1159-1205`; `app/ponto/services/dia_pago.py:335-358`.
- **Guarda temporária atual:** competência **09/2026**, nas empresas que a exportaram, tem colaboradores excluídos da escrita financeira, inclusive backfill de `DiaPago`; há trilha de recusa. Não é guarda genérica de toda competência exportada. Referências: `app/ponto/services/fechamento.py:36-43,523-559`.
- `celula_veredito_velho --aplicar --antes arquivo` **desliga recálculo financeiro** e grava snapshot da célula antes de rejulgar. Sem `--aplicar`, o ensaio pode executar recálculo dentro de `atomic`, seguido de rollback. Referência: `app/ponto/management/commands/celula_veredito_velho.py:90-114,133-159`.

**TXT e rubricas**

- Relavratura/recálculo não reescrevem o `conteudo`/`hash_sha256` de um TXT antigo nesses caminhos; cada nova exportação cria seu próprio registro, conteúdo e hash. Até a porta de invalidação preserva conteúdo/hash, alterando apenas os campos de invalidação. Referência: `app/folha/models.py:143-187`.
- **Importante:** um **novo Gerar TXT já deriva os números online**, com `somente_leitura=True`, e monta suas linhas/rubricas a partir desses objetos; portanto o próximo TXT pode refletir a célula relavrada **sem que o fechamento gravado tenha sido previamente recalculado**. Referências: `app/folha/views.py:207-212,265-268,280-300`. O mapeamento cadastrado de rubricas é apenas consultado pela montagem (`app/folha/export.py:958-965`).
- Efeito monetário concreto dessa alteração do veto no dado fornecido: **[nao verificado]**.

## 30/10 12:04 · sessao 381499a71b54 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **Reabertura automática escapa do veto pelo HX-RESPOSTA-VISIVEL.** O signal de fechamento chama automaticamente o resolver (`app/chamados/signals.py:75-94`); ele reabre o pai por `_pai.reabrir()` direto quando há resposta pendente creditada (`app/chamados/services/disputa_emissao.py:1598-1608`). O veto novo está somente em `_renascer_por_premissa_viva` (`app/chamados/models.py:778-781`); `reabrir` consulta outras recusas e grava vida sem perguntar o veto (`app/chamados/models.py:1119-1158`).

   **Contraexemplo:** pai legado `entrada_bloqueada_turno_aberto`, encerrado por sistema, dia forte no contexto, sem vínculo no dia, sem agrupamento/irmão vivo; disputa aberta com pergunta respondida e não validada cujo destino acabou de encerrar. O juiz devolveria `VETO_DEGENERADA` (`app/ponto/services/furos_vetados.py:40-41`), mas o resolver chama `reabrir` diretamente (`app/chamados/services/disputa_emissao.py:1606`). Esse módulo não pertence a `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235-241`), portanto, sem categoria/subcategoria legada de furo, também evita a guarda de premissa (`app/chamados/models.py:984-990`, `app/chamados/models.py:1078-1080`). A resposta pendente que ativa o ramo é selecionada sem consultar vínculo/veto (`app/chamados/juizes.py:1471-1477`). Ocorrência em produção: [nao verificado].

2. **Selo não fiscaliza esse escape.** O varredor só trata `update`/`bulk_update` como vivificação (`app/chamados/tests/test_o261_veto_na_porta.py:29`) e identifica pedidos à porta apenas quando a cadeia nomeia o modelo (`app/chamados/tests/test_o261_veto_na_porta.py:207-208`); chamadas `pai.reabrir()` não entram no conjunto de escritas (`app/chamados/tests/test_o261_veto_na_porta.py:209-219`). O caso MORDE de escrita cobre gestores e construtor+save, sem reabertura de instância (`app/chamados/tests/test_o261_veto_na_porta.py:264-297`).

   **Contraexemplo:** `def automatico(pai): pai.reabrir()` produz `([], [])` pelo próprio `varrer_fonte`; conferido executando somente as funções AST em memória, com `python3 -B`, sem Django/banco nem arquivos escritos. A limitação está nas condições citadas (`app/chamados/tests/test_o261_veto_na_porta.py:207-219`).

3. **Grade conserva outra chamada automática direta que pode desfazer a recusa da porta.** `abrir` devolve o chamado encerrado mesmo quando `_renascer_por_premissa_viva` recusou (`app/chamados/models.py:685-688`); `_emitir_furo` então chama `ch.reabrir()` se ele continuar encerrado (`app/ponto/management/commands/reconciliar_grade.py:519-522`). A nova guarda `ch is None` não cobre esse retorno de dedup (`app/ponto/management/commands/reconciliar_grade.py:519-520`).

   **Contraexemplo:** o vínculo saudável recebido em `ec` passa pela cópia de veto (`app/ponto/management/commands/reconciliar_grade.py:486-492`), mas o vínculo relido pela porta é ausente/suspeito (`app/chamados/models.py:896`): a porta recusa o renascimento, devolve o encerrado, e o ramo seguinte tenta reabrir sem veto. O caller pré-carrega o mapa de vínculos antes da emissão (`app/ponto/management/commands/reconciliar_grade.py:187-197`), permitindo divergência se o cadastro mudar entre as leituras; ocorrência concreta em produção e efeito final após signals: [nao verificado].

4. **Trilha prometida por operação não inclui operação na deduplicação.** A docstring promete uma linha por objeto/módulo/dia/veto/operação (`app/chamados/models.py:908-910`), mas a consulta ignora `via` (`app/chamados/models.py:921-922`), embora a grave (`app/chamados/models.py:927-928`).

   **Contraexemplo:** `abrir` recusado e depois `criar` recusado para mesmo colaborador/célula, módulo, dia e motivo: a segunda operação não deixa linha própria (`app/chamados/models.py:921-923`). Também não nasce nova linha depois de correção e posterior regressão ao mesmo veto, pois a busca cobre toda a história (`app/chamados/models.py:921-923`).

Abertura manual atual bloqueada com data: **sem achado concreto**. A UI do DP restringe módulos aos tipos abríveis do catálogo (`app/chamados/services/abertura.py:148-153`); os cinco módulos do universo não têm abertura declarada (`app/chamados/catalogo/modulos.py:105-112`, `app/chamados/catalogo/modulos.py:182-183`). A abertura do colaborador passa contexto sem dia (`app/chamados/services/abertura.py:58-64`), e a API cria `app_colaborador`, fora do universo (`app/api/views.py:2237-2246`). A limitação genérica de `criar` existe porque ele aplica o veto sem distinguir autoria (`app/chamados/models.py:838-841`), mas um fluxo humano atual do universo com dia que a acione está [nao verificado].

## 30/10 12:03 · sessao dfd42513de12 · - · - · origem=Ronald (tmux)

Achei dois problemas de alcance; a ausência de vínculo continua sem chamado de cadastro no fluxo normal, e uma impressão de célula continua sem consultar o veto. (`app/colaboradores/queries.py:64`; `app/ponto/services/cartorio.py:941`)

1. **Cadastro ATIVO sem vínculo: o escritor existe, mas os fluxos automáticos não chegam nele.** `vetar_cobranca_se_degenerada` aceita `ec=None`, chama `regua_degenerada(None, data)` e abre `regua_degenerada`, deduplicado por `tipo_escala_id=None`; esse módulo fica fora do universo vetado pela porta. (`app/escala/regua_defesa.py:45`; `app/escala/regua_defesa.py:51`; `app/chamados/models.py:889`)

   Seus dois chamadores de produção são detector e grade. O detector consulta `colaboradores_em_operacao`, que exige vínculo cobrindo hoje, e ainda pula `ec=None` no laço. A grade descarta `ev=None`/`te=None` antes de chamar `_emitir_furo`. O cartório também exige vínculo para chamar o detector, tanto no furo integral quanto no parcial. (`app/ponto/management/commands/detectar_ausencias.py:98`; `app/colaboradores/queries.py:64`; `app/ponto/management/commands/detectar_ausencias.py:139`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/marcos.py:107`; `app/ponto/management/commands/reconciliar_grade.py:254`; `app/ponto/services/cartorio.py:1347`; `app/ponto/services/cartorio.py:1446`)

   **Contraexemplo:** colaborador ATIVO, empresa em rollout, usuário operacional, com batidas existentes, mas sem qualquer vínculo cobrindo hoje/ontem. O detector o exclui; a grade o inclui pelas batidas, porém não chega ao escritor; uma tentativa direta de nascimento do universo é vetada pelo juiz puro, que não abre cadastro. Portanto a promessa “ativo sem vínculo segue abrindo `regua_degenerada`” não está atendida nesses caminhos. A falha já existia antes do diff apresentado; regressão nova dessa ausência: [nao verificado]. (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:64`; `app/ponto/management/commands/reconciliar_grade.py:84`; `app/ponto/management/commands/reconciliar_grade.py:106`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/chamados/models.py:896`; `app/ponto/services/furos_vetados.py:40`)

   O teste atual prova a função isolada com **vínculo existente e template degenerado**; o outro apenas procura o nome da função no texto dos emissores. Nenhum deles prova o caminho automático de ATIVO sem vínculo. (`app/ponto/tests/test_f3_regua_defesa.py:49`; `app/ponto/tests/test_f3_regua_defesa.py:53`; `app/ponto/tests/test_f3_regua_defesa.py:61`)

2. **Impressão de `nunca_bateu` continua fora do veto.** Sem batida apurável na vida, o cartório entra no ramo `adesao is None`, calcula `imp0` sem `veto=`, pode pular por igualdade e retorna antes da definição/cache de `_veto_do_dia`. (`app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:907`; `app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1003`; `app/ponto/services/cartorio.py:1011`)

   **Contraexemplo:** célula já carimbada de colaborador sem batidas e sem vínculo; todos os demais insumos permanecem iguais. A mudança `ec=None → VETO_DEGENERADA` não entra em `imp0`, e a célula continua sendo pulada. O ramo normal inclui o veto e reage à mudança; esse ramo não. (`app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/furos_vetados.py:40`)

Quanto à relavratura, o próximo `processar_cartorio --apply` percorre todas as células passadas, sem piso de competência; no ramo normal, a nova palavra do veto altera a impressão e ultrapassa o pulo, chegando à lavra/carimbo. Isso prova alteração possível da própria célula exportada, mas **não prova alteração de `FechamentoMensal`, `DiaPago` ou TXT**. Reversão prévia, hash das exportadas e diferença enviada à Pauta DP: [nao verificado]. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/cartorio.py:1409`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/celula.py:721`)

## 30/10 12:03 · sessao 77cd2fb619a1 · - · - · origem=Ronald (tmux)

Migração das fixtures: **sem achado de intenção “sem vínculo/sem escala” mascarada** no censo dos usos de `dar_vinculo`; execução da bateria [nao verificado].

- F4 preservou o caso explícito “sem escala”: `test_MORDE_procedencia_ignora_batida_retratada` cria colaborador sem escala e chama diretamente `procedencia_fina`; não passa pelo helper que adiciona vínculo. `app/chamados/tests/test_selo_f4_fatia4.py:131-139`, `:67-70`.
- `TrancaSemCadastro` testa a natureza dos avisos de cadastro, não ausência de vínculo; o vínculo novo não elimina o cenário da tranca. `app/chamados/tests/test_tranca_sem_cadastro.py:3-12`, `:39-57`.
- Os comentários “sem vínculo no fixture” ficaram desatualizados em `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:57` e `app/chamados/tests/test_resposta_tardia.py:66`; os testes continuam explicitamente montando/verificando a premissa/lâmpada, em `:46-65` e `:63-77`, respectivamente.
- O caso novo sem vínculo usa outro colaborador, não chama `dar_vinculo` e espera `(None, False)` da porta. `app/chamados/tests/test_o261_veto_na_porta.py:395-405`.

**Lacuna de prova nos sítios migrados, preexistente ao diff fornecido:** dois testes prometem nascimento no nome, mas só exercitam construção de contexto: [nao verificado]

1. `app/chamados/tests/test_emissor_nomeia_o_marco.py:88-94`: chama `_ctx_c2`, verifica data e ausência de marcos; não abre chamado.
2. `app/chamados/tests/test_bug125_cobranca_cega.py:176-185`: simula falha da lavra e verifica o dicionário; não abre chamado.

Contraexemplo concreto para ambos: `_ctx_c2` continua produzindo aquele contexto e o emissor/porta passa a recusar todo nascimento; as asserções permanecem satisfeitas. Não é regressão de vacuidade introduzida pela inserção de `dar_vinculo`, conforme o diff anexo [nao verificado quanto ao histórico além do anexo].

## 30/10 12:03 · sessao b5596fdd5cf4 · - · - · origem=Ronald (tmux)

**A relavratura alcança células de competência exportada ou trancada:** o lote seleciona apenas `empresa` e `data < hoje`, percorre todas as células passadas do colaborador e chama `julgar_colab`; não consulta `ExportacaoDominio` nem tranca nesse recorte. (`app/ponto/management/commands/processar_cartorio.py:49-65`)

O julgamento normal incorpora `motivo_do_veto` na impressão e só pula por impressão igual, carimbo/veredito existentes e ausência de `forcar`; em seguida grava pelas portas `lavrar_veredito` e `carimbar`. Não há guarda por competência nesses trechos. (`app/ponto/services/cartorio.py:1006-1014`, `:1083-1097`, `:1406-1425`)

A guarda concreta do escritor é a decisão administrativa: `veredito_via == 'admin'` preserva o veredito, mas permite atualizar a ata. `lavrar_veredito` e `carimbar` não consultam exportação ou tranca. (`app/ponto/portas/celula.py:647-729`)

**Não confundir com regeneração do DNA:** `regenerar_celulas_vinculo` consulta `_competencias_exportadas`, salvo motivo explícito `apesar_da_lavra`; essa guarda pertence à regeneração, não à relavratura descrita acima. (`app/ponto/portas/celula.py:191-202`; `app/ponto/services/cartorio.py:719-728`)

Há piso para **emissão automática de chamados**, não para gravar o julgamento: `PISO_EMISSAO = '2026-09'`; `_fora_do_piso` considera esse horizonte e `toque`, sem consultar tranca/exportação. (`app/ponto/services/cartorio.py:190`, `:232-246`, `:1319-1327`)

**Cron configurado:** `processar_cartorio --apply`, diariamente às 06:27/06:29/06:31 para empresas 2/3/4, pela expansão de `por_empresa=True` e passo padrão 2. A janela é todo o passado com células existentes, sem piso. Instalação/execução efetiva em produção: [nao verificado]. (`app/config/crons.py:17-18`, `:29-33`, `:597`, `:1785-1794`; `app/ponto/management/commands/processar_cartorio.py:49-65`)

Há também `vigia_de_hora --apply` a cada dez minutos, por empresa, cobrindo candidatas de hoje e certos turnos noturnos de ontem; usa o mesmo `julgar_celula` com teto dos marcos. (`app/config/crons.py:348-349`; `app/ponto/services/vigia_de_hora.py:108-130`)

**Comando de relavratura por competência:** `celula_veredito_velho --mes M --ano A [--empresa N]`. Outros argumentos: `--json`, `--sem-recalculo`, `--aplicar`, `--antes`, `--so-recalculo`; recorta `janela_fechamento` até no máximo ontem. (`app/ponto/management/commands/celula_veredito_velho.py:37-54`, `:75-80`)

Sem `--aplicar`, faz ensaio em transação, incluindo recálculo por padrão, e provoca rollback; `--sem-recalculo` evita esse recálculo. Com `--aplicar`, exige `--antes`, desliga obrigatoriamente o recálculo e chama `julgar_colab(..., apply_=True, forcar=True)`. Não há guarda de exportação/tranca antes dessa relavratura. (`app/ponto/management/commands/celula_veredito_velho.py:90-114`, `:133-159`)

O snapshot `--antes` guarda `pk`, colaborador/data, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `julgada_em`, `impressao`; **não inclui** os demais campos que `carimbar` escreve (`ausencia`, `tem_entrada`, `tem_saida`, `insumos_em`), portanto é reversão parcial do estado da célula. (`app/ponto/management/commands/celula_veredito_velho.py:100-114`; `app/ponto/portas/celula.py:721-728`)

**Exceção importante à premissa do impacto:** o ramo `adesao is None` (`nunca_bateu`) monta impressão **sem veto**, portanto a alteração isolada de `motivo_do_veto` não muda necessariamente a impressão de toda célula sem vínculo. A conclusão automática vale para o ramo normal que passa `veto=_veto_do_dia(data)`. (`app/ponto/services/cartorio.py:907`, `:941-943`, `:1083-1088`)

O docstring de `calado()` ainda fala em célula trancada “imutável”, mas seu corpo somente liga o sinal de antirrecursão; não constitui guarda do sweep do cartório. (`app/ponto/services/cartorio.py:58-68`; `app/ponto/management/commands/processar_cartorio.py:49-65`)

## 30/10 12:03 · sessao 2622be1dadf8 · - · - · origem=Ronald (tmux)

Q4 — evidências da cópia, somente leitura:

- O cartório chama `cel.lavrar_veredito(via='cartorio', …, save=False, ata=…)` e depois `carimbar(…, lavrados=_cv)`; portanto a persistência reúne campos da lavra e do carimbo (`app/ponto/services/cartorio.py:1408`, `:1424`; `app/ponto/portas/celula.py:726`).
- A lavra pode escrever **`veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`**. Entrada idêntica retorna sem escrita; se o veredito é humano (`veredito_via='admin'`), cartório preserva a decisão e pode atualizar somente a ata (`app/ponto/portas/celula.py:669`, `:671`, `:687`, `:689`).
- O carimbo escreve **`julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em`**, além dos campos retornados pela lavra; mesma impressão, mesmos insumos e nenhuma lavra nova não atualizam os relógios (`app/ponto/portas/celula.py:716`, `:721`, `:726`).
- A relavratura não é regeneração: `dna`, `origem`, `regenerada_em`, `dna_anterior`, `editada_*` não pertencem à união dessas escritas (`app/ponto/services/cartorio.py:719`; implementação `app/ponto/portas/celula.py:695`, `:726`). O `dna_anterior` é histórico da **regeneração do vínculo**, não backup da ata/veredito (`app/escala/models.py:1682`, `:1694`).
- A ata recebida preserva a chave `precedencia` já existente; demais chaves ausentes na nova ata não são preservadas genericamente (`app/ponto/portas/celula.py:94`, `:660`). O caminho unitário `julgar_celula` também escreve `ata.precedencia` após julgar; isso usa `carimbar_precedencia` (`app/ponto/services/cartorio.py:1563`; `app/ponto/portas/celula.py:732`).
- Existe `LogAuditoria` com ação **`veredito_celula`**, objeto `CelulaDia`: antes/depois guardam **somente `veredito`, `via`, `protestos`**. Não guardam ata, impressão, ausência, indicadores de batida nem timestamps (`app/ponto/portas/celula.py:657`, `:698`). Logo esse registro permite conhecer a decisão anterior, mas **não é reversão completa da célula** (mesmas referências).
- A trilha é **best-effort**: sua falha é registrada/engolida e não garante existência de linha de auditoria para toda alteração; existência efetiva de registros em produção **[nao verificado]** (`app/core/observ.py:94`, `:105`, `:133`).
- A gravação do veredito/ata também emite **`EventoCelula`**, no mesmo `atomic` do save, quando a lavra inclui `veredito` ou `ata`; esse evento tem identificação da célula/colaborador/data e campos de processamento, **sem payload de antes/depois**, portanto não é backup (`app/ponto/portas/celula.py:623`, `:626`, `:634`; `app/escala/models.py:1800`).
- Há precedente de snapshot manual de células: `bin/snapshot_o118_col221.py` coleta `trabalha`, `origem`, `julgada_em`, `dna`, `ata`, `impressao`, `escala_geradora_id` e chamados, limitado ao colaborador 221 desde 21/09/2026; a reversão descrita repõe ata/julgada_em/impressão, não oferece restauração automática genérica de todo cartório (`bin/snapshot_o118_col221.py:12`, `:15`, `:25`). Arquivo de snapshot realmente existente/executado **[nao verificado]**.
- Os scripts nomeados `snapshot_relavratura_10_2026.py`/`restore_relavratura_10_2026.py` fotografam/restauram **FechamentoMensal e DiaPago motor**, não CelulaDia; não confundí-los com reversão da ata/impressão (`bin/snapshot_relavratura_10_2026.py:20`, `:28`, `:34`, `:57`; `bin/restore_relavratura_10_2026.py:69`, `:79`, `:85`).

## 30/10 11:59 · sessao 46ece27c3e68 · O261-VETO-A2 · sonda · origem=Code

Não executei a sonda: o script não foi localizado na cópia. Abaixo, transcrição integral do dado fornecido, sem validação em produção. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_sem_vinculo_exportada.py [926d4e5e0ed50956] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HOJE=2026-10-30 INI=2026-09-20 CEL_TOTAL=39418
SEMVINC|emp=1|comp=2026-10|aberta=21
SEMVINC|emp=2|comp=2026-09|EXPORTADA=52
SEMVINC|emp=2|comp=2026-10|aberta=1272
SEMVINC|emp=2|comp=2026-11|aberta=894
SEMVINC|emp=2|comp=2026-12|aberta=90
SEMVINC|emp=2|comp=2027-01|aberta=22
SEMVINC|emp=3|comp=2026-09|EXPORTADA=11
SEMVINC|emp=3|comp=2026-10|aberta=206
SEMVINC|emp=3|comp=2026-11|aberta=475
SEMVINC|emp=3|comp=2026-12|aberta=110
SEMVINC|emp=3|comp=2027-01|aberta=44
SIT|ativo|EXPORTADA=9
SIT|ativo|aberta=168
SIT|desligado|EXPORTADA=54
SIT|desligado|aberta=2966
EXPORTADA_TOTAL=63
ERROS=0
FIM-DA-SONDA
```

Total de células sem vínculo: **3.197**, pela soma das linhas SEMVINC. [nao verificado]  
Em competência EXPORTADA: **63** — empresa **2**, **2026-09**: **52**; empresa **3**, **2026-09**: **11**. [nao verificado]  
**ERROS=0**, conforme o dado fornecido. [nao verificado]

## 30/10 10:55 · sessao f89de696d0e3 · FIO-MORTO-DONO-NA-LISTA · verificador · origem=Code

O vigia desta árvore **lê o dono lavrado; não o recalcula**. A retirada de `fio_morto` exige a chave `(colaborador, data)` na lista e valor BATIDA/CADASTRO. Permanecem lacunas de observabilidade, relógio, custo e cobertura dos selos descritas abaixo. (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`, `app/chamados/services/vigia_fio.py:314`)

A identidade Git e a completude histórica de `f0d319ba..HEAD`, o resultado da suíte/RED e qualquer impacto quantitativo em produção ficam **[nao verificado]**.

**Premissas pelo código**

- **Dono L-099:** neste fluxo, quem decide é `dono_da_divergencia`: CADASTRO pelas condições de cadastro; BATIDA por batida fora de turno/paridade; depois CADASTRO ou ESTRUTURA. `InsumosDoDono.dono()` monta os insumos e chama esse decisor. Isso não constitui um único registro global atualizado do dono: e6 e pautas calculam em seus atos; a lista lê uma fotografia persistida. (`app/ponto/management/commands/e6_oraculo.py:90`, `app/ponto/management/commands/e6_oraculo.py:146`, `app/ponto/management/commands/e6_oraculo.py:338`, `app/ponto/management/commands/pautas_discordantes.py:104`, `app/ponto/services/teto_na_lista.py:123`)
- **“Está na lista do admin”:** tela e vigia chamam a mesma `lista()`, com empresas visíveis; o vigia ainda cruza seu recorte. Mas essa lista reúne várias origens: cadastro atual, snapshots C e lavra A. Portanto, uma função compartilhada não significa uma única origem persistente. (`app/escala/views.py:490`, `app/chamados/services/vigia_fio.py:254`, `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:575`)
- **Alcance efetivo de “dia na lista”:** `dias_na_lista()` exige assinatura com `data`. Atualmente C9 fornece esse campo; C4/C5 mencionam datas somente em `detalhe`, e A fornece agregados. Assim, o helper alcança C9, não todos os dias mencionados textualmente na tela. (`app/escala/services/cadastro_realidade.py:669`, `app/escala/services/cadastro_realidade.py:312`, `app/escala/services/cadastro_realidade.py:374`, `app/escala/services/cadastro_realidade.py:142`, `app/escala/services/cadastro_realidade.py:617`)
- **Outra lista com nome parecido:** `InsumosDoDono.cxr()` lê `resultado.dias_cadastro_x_realidade` do motor, não `cadastro_realidade.lista()`. Confundir esses universos daria uma premissa falsa. (`app/ponto/management/commands/e6_oraculo.py:136`, `app/ponto/motor_calculo_v2.py:2657`, `app/escala/services/cadastro_realidade.py:502`)

**Escritores e leitores dos campos pedidos**

| Campo/valor | Quem escreve/calcula | Quem lê |
|---|---|---|
| `fio_do_admin` | Inicializado e incrementado pelo censo. (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:320`) | `linhas()`; carimbo integral; seleção genérica de inteiros do materializador. (`app/chamados/services/vigia_fio.py:516`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:208`) |
| `celulas_fio_do_admin` | Censo, limitado por `limite`. (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:321`) | `linhas()` e carimbo integral; o materializador seleciona somente int/bool. (`app/chamados/services/vigia_fio.py:516`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:208`) |
| `fio_do_admin_sem_resposta` | Censo: incrementa por candidato quando a chamada da lista falhou. (`app/chamados/services/vigia_fio.py:217`, `app/chamados/services/vigia_fio.py:260`, `app/chamados/services/vigia_fio.py:264`) | `linhas()`, carimbo e materializador genérico. (`app/chamados/services/vigia_fio.py:516`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:208`) |
| Resultado de `lista()` | Monta assinaturas, agrupa por posto e devolve `postos`. (`app/escala/services/cadastro_realidade.py:593`, `app/escala/services/cadastro_realidade.py:617`, `app/escala/services/cadastro_realidade.py:652`) | Tela/PDF; vigia; encerradores de avisos C7 e chamados C8. (`app/escala/views.py:490`, `app/chamados/services/vigia_fio.py:259`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`) |
| Mapa de `dias_na_lista()` | Extrai `(colab_id, data) → dono_do_dia`, sem recalcular o dono. (`app/escala/services/cadastro_realidade.py:666`, `app/escala/services/cadastro_realidade.py:670`) | `_fio_do_admin()` consulta a chave e aceita BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`) |
| `InsumosDoDono.dono()` e valores BATIDA/CADASTRO/ESTRUTURA | Método chama `dono_da_divergencia()`. (`app/ponto/management/commands/e6_oraculo.py:146`, `app/ponto/management/commands/e6_oraculo.py:155`) | e6, pautas e produtor C9. (`app/ponto/management/commands/e6_oraculo.py:338`, `app/ponto/management/commands/pautas_discordantes.py:104`, `app/ponto/services/teto_na_lista.py:86`) |
| `data` e dono persistidos da C9 | `medir()` escreve `itens[*].data/dono`; `lavrar()` persiste o dicionário em `MetricaSnapshot.detalhe`, chamado pelo comando com `--apply`. (`app/ponto/services/teto_na_lista.py:79`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`) | `ultimo_lavrado()` abre o snapshot; `_teto_do_dia()` copia para `data/dono_do_dia`. (`app/ponto/services/teto_na_lista.py:123`, `app/escala/services/cadastro_realidade.py:310`) |
| Assinatura C9: `data`, `dono_do_dia`, dono de apresentação | `_teto_do_dia()` copia os campos; `lista()` conserva a assinatura e produz rótulo/ação pelo dono. (`app/escala/services/cadastro_realidade.py:312`, `app/escala/services/cadastro_realidade.py:598`, `app/escala/services/cadastro_realidade.py:602`) | Helper lê `data/dono_do_dia`; ação lê o dono; HTML/PDF mostram `detalhe` e dono de apresentação. (`app/escala/services/cadastro_realidade.py:670`, `app/escala/services/cadastro_realidade.py:450`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:711`) |
| `agora`/`hoje` do comando | `agora = timezone.now()`; `hoje = timezone.localdate(agora)`. (`app/chamados/management/commands/vigiar_fio_morto.py:158`) | `hoje` entra no censo, comparações temporais e lista; ambos datam o carimbo. (`app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/management/commands/vigiar_fio_morto.py:196`) |
| `medido_em` do carimbo | Derivado do mesmo `agora`; `_carimbar()` grava JSON e substitui o arquivo. (`app/chamados/management/commands/vigiar_fio_morto.py:196`, `app/chamados/management/commands/vigiar_fio_morto.py:127`, `app/chamados/management/commands/vigiar_fio_morto.py:129`) | ESTADO calcula idade e publica horário; teste verifica formato/offset/data. (`bin/fio_estado.py:74`, `bin/fio_estado.py:112`, `app/chamados/tests/test_o261_vigia_fio.py:793`) |

Os novos testes também leem os três campos do admin, o mapa e os argumentos enviados à lista; a lista é substituída por mock nesses cenários. (`app/chamados/tests/test_o261_vigia_fio.py:541`, `app/chamados/tests/test_o261_vigia_fio.py:565`, `app/chamados/tests/test_o261_vigia_fio.py:588`, `app/chamados/tests/test_o261_vigia_fio.py:603`, `app/chamados/tests/test_o261_vigia_fio.py:615`)

**Censo completo: quem escreve e quem mais lê**

Todos os campos abaixo nascem em `censo()`; o comando recebe o dicionário e preserva **todos**, inclusive listas, no carimbo. O materializador chama outro censo e seleciona automaticamente todos os int/bool para seu antes/depois. (`app/chamados/services/vigia_fio.py:207`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:203`, `app/chamados/management/commands/materializar_passivo_validado.py:208`)

| Campos | Escrita no censo | Leitura específica, além dos consumidores integrais acima |
|---|---|---|
| `chamados_vivos_com_celula`, `celulas_apontadas`, `celulas_so_de_outro_vocabulario` | `app/chamados/services/vigia_fio.py:207` | Log lê os dois primeiros. (`app/chamados/services/vigia_fio.py:489`) |
| `lampadas_acesas`, `cego` | `app/chamados/services/vigia_fio.py:231`, `app/chamados/services/vigia_fio.py:225`, `app/chamados/services/vigia_fio.py:363` | Log, ESTADO; `cego` também alarma. (`app/chamados/services/vigia_fio.py:489`, `bin/fio_estado.py:90`, `app/chamados/management/commands/vigiar_fio_morto.py:80`) |
| `dias_futuros_no_universo`, `fio_morto_de_hoje_pra_frente` | `app/chamados/services/vigia_fio.py:279`, `app/chamados/services/vigia_fio.py:292` | Log/ESTADO; somente o primeiro entra no alarme. (`app/chamados/services/vigia_fio.py:496`, `bin/fio_estado.py:88`, `app/chamados/management/commands/vigiar_fio_morto.py:80`) |
| `fora_do_vocabulario_do_cartorio`, `fora_do_vocabulario_sem_fio_vivo`, `fio_vivo` | `app/chamados/services/vigia_fio.py:282`, `app/chamados/services/vigia_fio.py:284`, `app/chamados/services/vigia_fio.py:287` | Log; `fio_vivo` também no ESTADO. (`app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:517`, `bin/fio_estado.py:91`) |
| `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `celulas_escala_nao_descreve_o_dia` | `app/chamados/services/vigia_fio.py:298`, `app/chamados/services/vigia_fio.py:305`, `app/chamados/services/vigia_fio.py:310`, `app/chamados/services/vigia_fio.py:312` | Log. (`app/chamados/services/vigia_fio.py:506`, `app/chamados/services/vigia_fio.py:509`, `app/chamados/services/vigia_fio.py:511`) |
| `fio_morto`, `sem_pergunta_nenhuma`, `pergunta_morreu`, `fio_morto_cobrando`, `celulas_do_fio_morto` | `app/chamados/services/vigia_fio.py:324`, `app/chamados/services/vigia_fio.py:325`, `app/chamados/services/vigia_fio.py:327`, `app/chamados/services/vigia_fio.py:330` | Log; células no comando; morto/cobrando no alarme e ESTADO. (`app/chamados/services/vigia_fio.py:494`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:81`, `bin/fio_estado.py:86`) |
| `porta_universo_nos_dias_mortos`, `porta_<classe>`, `fio_morto_sem_pergunta_na_porta`, `fio_morto_com_alvo`, `fio_morto_sem_alvo`, `classes_do_fio_morto` | `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:339`, `app/chamados/services/vigia_fio.py:345`, `app/chamados/services/vigia_fio.py:347`, `app/chamados/services/vigia_fio.py:349`, `app/chamados/services/vigia_fio.py:355` | Contadores no log; `com_alvo` também no alarme e comparação genérica do ESTADO. (`app/chamados/services/vigia_fio.py:499`, `app/chamados/services/vigia_fio.py:503`, `app/chamados/management/commands/vigiar_fio_morto.py:81`, `bin/fio_estado.py:72`) |
| `decisao_do_dia_pendente_com_alvo` | `app/chamados/services/vigia_fio.py:362` | Log. (`app/chamados/services/vigia_fio.py:506`) |

O **ESTADO** lê `censo/esperado`, compara genericamente os esperados e publica uma seleção dos campos; `gerar_estado.py` usa esse leitor/compositor. O **placar do materializador** imprime diferenças, sem condicionar sucesso aos valores. (`bin/fio_estado.py:69`, `bin/fio_estado.py:72`, `bin/fio_estado.py:82`, `bin/gerar_estado.py:260`, `app/chamados/management/commands/materializar_passivo_validado.py:230`)

O **placar do cron** guarda nome, início, fim, duração e exit, não contadores. O cron declara os dois alarmes; exit 2 é alarme de negócio e não abre pauta de cron quebrado. As **pautas discordantes** têm apuração própria pelo mesmo juiz do dono, sem ler os contadores desse censo. (`bin/cron_run.sh:13`, `app/config/crons.py:1654`, `bin/cron_run.sh:32`, `app/ponto/management/commands/pautas_discordantes.py:104`, `app/ponto/services/pautas_dos_discordantes.py:158`)

**Checklist de classes**

1. **Segundo juiz que duplica registrado — achado preexistente, agora alcançado:** a C2 fixa competência com corte `21`, enquanto JUIZES registra `janela_atual/corte_da_empresa` e proíbe esse literal. O novo caminho chama essa C2. Não é duplicação do juiz do dono. (`app/escala/services/cadastro_realidade.py:187`, `app/docs/JUIZES.md:57`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:518`)

2. **Segundo escritor fora da porta — não demonstrado nos campos auditados:** os contadores são escritos pelo censo; o dono C9 é persistido por `lavrar()`; o vigia somente extrai e compara o valor. (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:320`, `app/ponto/services/teto_na_lista.py:113`, `app/escala/services/cadastro_realidade.py:670`, `app/chamados/services/vigia_fio.py:266`)

3. **Relógio fora da borda — presente:** comando e carimbo compartilham o instante, mas o calendário chamado pelo censo relê `timezone.now()`. Na dependência nova, C2 usa `date.today()` e C4 omite `dia` ao chamar uma função cujo default relê `localdate()`. Esses dois últimos relógios produzem C2/C4, não o dono C9. (`app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/chamados/management/commands/vigiar_fio_morto.py:196`, `app/chamados/services/vigia_fio.py:294`, `app/colaboradores/services/calendario.py:249`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:350`, `app/colaboradores/services/situacao_exibida.py:45`)

4. **Derivação paralela — dono corrigido neste caminho:** o helper copia o lavrado, sem chamar `InsumosDoDono`. Fora do diff, pautas conserva um pré-filtro separado com chamada parcial ao mesmo juiz antes de `_ins.dono()`. É duplicação da consulta parcial, não outro decisor. (`app/escala/services/cadastro_realidade.py:670`, `app/chamados/services/vigia_fio.py:266`, `app/ponto/management/commands/pautas_discordantes.py:93`, `app/ponto/management/commands/pautas_discordantes.py:104`)

5. **Selo/RED sem mordida suficiente — duas lacunas concretas:** a mistura da partição inclui `fio_do_admin` na soma, mas não cria C9 nem exige esse balde positivo; o teste positivo separado não cobra a soma. O selo temporal verifica formato/offset/data, sem provocar a virada que distinguia a implementação anterior. Execução histórica do RED: **[nao verificado]**. (`app/chamados/tests/test_o261_vigia_fio.py:647`, `app/chamados/tests/test_o261_vigia_fio.py:659`, `app/chamados/tests/test_o261_vigia_fio.py:666`, `app/chamados/tests/test_o261_vigia_fio.py:553`, `app/chamados/tests/test_o261_vigia_fio.py:755`, `app/chamados/tests/test_o261_vigia_fio.py:793`)

6. **Custo — ampliação verificável, duração desconhecida:** a lista roda uma vez por censo com candidato, inclusive em falha. Porém monta todas as assinaturas; C2 percorre células da competência de todo o recorte e chama `periodos_do_dia()` por célula. “Só nos dias mortos” limita o gatilho, não o universo desse trabalho. Tempo e queries em produção: **[nao verificado]**. (`app/chamados/services/vigia_fio.py:250`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:195`, `app/escala/services/cadastro_realidade.py:200`)

7. **Quem-mais-lê/observabilidade — duas lacunas:** o ESTADO não seleciona os três campos novos, embora eles estejam no carimbo e os inteiros entrem no materializador. Além disso, uma exceção na leitura C9 é engolida dentro de `lista()`; o vigia recebe resposta parcial e pode publicar `fio_do_admin_sem_resposta=0`. Os dias permanecem em `fio_morto`, mas a indisponibilidade da C9 fica invisível nesse contador. Os testes de falha substituem a lista inteira, sem exercitar essa falha interna. (`bin/fio_estado.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:208`, `app/escala/services/cadastro_realidade.py:561`, `app/chamados/services/vigia_fio.py:260`, `app/chamados/services/vigia_fio.py:266`, `app/chamados/tests/test_o261_vigia_fio.py:573`)

## 30/10 10:53 · sessao 7e2b21166b22 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **P2 — teste da partição não exercita `fio_do_admin > 0`.** A mistura fabrica dias e somente simula `_oferece`; acrescenta o balde novo à igualdade, mas não fabrica assinatura C9 nem exige valor positivo (`app/chamados/tests/test_o261_vigia_fio.py:647-668`). A empresa da fixture usa CNPJ oculto (`:34`); `empresas_visiveis()` exclui esse prefixo (`app/colaboradores/queries.py:85-86`), o vigia cruza visíveis com o recorte (`app/chamados/services/vigia_fio.py:256-259`) e a lista elimina linhas dessas empresas (`app/escala/services/cadastro_realidade.py:588-589`). Retirar o termo `fio_do_admin` dessa igualdade continuaria passando nessa mistura. O teste com admin positivo cobra os baldes, mas não sua soma (`app/chamados/tests/test_o261_vigia_fio.py:553-571`). RED efetivamente executado: [nao verificado].

2. **P2 — contador de falha não detecta falha interna da fonte C9.** `fio_do_admin_sem_resposta` sobe somente quando `lista()` levanta (`app/chamados/services/vigia_fio.py:259-264`). Entretanto, `lista()` engole exceção de `_teto_do_dia()` e prossegue sem C9 (`app/escala/services/cadastro_realidade.py:561-567`); também pode devolver `None` (`:575-578`), convertido em mapa vazio por `dias_na_lista()` (`:665-671`). A célula permanece corretamente em `fio_morto`, mas ausência de resposta da C9 aparece como `sem_resposta=0`. Os testes injetam exceção na lista inteira (`app/chamados/tests/test_o261_vigia_fio.py:573-588`), sem cobrir falha da C9.

3. **Relógio ainda não é único na rodada.** Comando fixa `agora` e `hoje`, encaminha somente `hoje` ao censo e usa `agora` no carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:158-160`, `:196-197`). Porém, censo chama a porta do calendário sem data de referência (`app/chamados/services/vigia_fio.py:294`); ela chama `contexto_calendario()` (`app/colaboradores/services/calendario.py:200-208`), que lê novamente `timezone.now()` (`:249`) e usa esse outro `hoje` na janela de último recurso (`:569`) e no juiz (`:905`). Essa cadeia antecede o diff fornecido. Na nova dependência da lista, `_jornada_que_mente()` usa `date.today()` (`app/escala/services/cadastro_realidade.py:186-187`, chamada em `:518`); `_afastado_sem_registro()` omite `dia` ao chamar `afastados_hoje()` (`:350`), cuja referência nasce em outro `localdate()` (`app/colaboradores/services/situacao_exibida.py:45`). Esses dois últimos caminhos produzem C2/C4, não o dono da C9 (`app/escala/services/cadastro_realidade.py:216`, `:373`, `:312-313`).

4. **Selo temporal não provoca a fronteira corrigida.** O teste apenas exige formato, offset e igualdade de datas (`app/chamados/tests/test_o261_vigia_fio.py:793-796`); seu executor e cenário não simulam passagem da meia-noite (`:739-753`, `:755-796`). A implementação antiga também satisfaz essas verificações fora dessa fronteira. RED histórico: [nao verificado].

5. **Custo estrutural: um dia candidato pode disparar leitura do recorte inteiro.** A lista é chamada sob demanda e memoizada (`app/chamados/services/vigia_fio.py:250-259`), mas monta todas as assinaturas (`app/escala/services/cadastro_realidade.py:513-575`). Sua C2 percorre vínculos ativos do recorte (`:190-193`) e todas as células de trabalho da competência, chamando `periodos_do_dia()` por célula (`:195-200`). Portanto, “custo só nos dias mortos” descreve o gatilho, não o universo percorrido (`app/chamados/services/vigia_fio.py:319`). Duração e volume de queries em produção: [nao verificado].

Não identifiquei novo cálculo do dono no vigia: ele compara o valor do mapa com BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:266`); o mapa lê `dono_do_dia` da assinatura (`app/escala/services/cadastro_realidade.py:669-670`), escrito pela adaptação C9 da lavra (`:310-313`). O cálculo permanece em `InsumosDoDono.dono()` → `dono_da_divergencia()` (`app/ponto/management/commands/e6_oraculo.py:146-155`), chamado pelo produtor da lavra (`app/ponto/services/teto_na_lista.py:72`, `:86`).

## 30/10 10:53 · sessao 0546f019a554 · - · - · origem=Ronald (tmux)

Achados no caminho de lista/dono:

1. **Relógio fora da borda persiste no caminho novo.** O vigia injeta `hoje` em `lista` (`app/chamados/services/vigia_fio.py:259`), mas a lista chama `_jornada_que_mente(empresas)` sem a data (`app/escala/services/cadastro_realidade.py:518`), e essa função lê `_dt.date.today()` (`app/escala/services/cadastro_realidade.py:186`). C4 também chama `afastados_hoje(colaboradores=qs)` sem `dia` (`app/escala/services/cadastro_realidade.py:350`), cujo fallback lê `timezone.localdate()` (`app/colaboradores/services/situacao_exibida.py:45`). Não muda diretamente o dono C9, mas invalida afirmar que toda a lista deriva do relógio capturado pelo comando.

2. **Custo ampliado para toda a lista, por rodada com candidato morto.** A chamada é memoizada uma vez (`app/chamados/services/vigia_fio.py:250`), porém executa todas as classes C1–C9 e a lavra A (`app/escala/services/cadastro_realidade.py:513`, `:518`, `:527`, `:535`, `:542`, `:549`, `:556`, `:563`, `:570`, `:575`). C2 consulta todas as células de trabalho da competência no universo de vínculos ativos e chama `periodos_do_dia` por célula (`app/escala/services/cadastro_realidade.py:190`, `:195`, `:200`). Não se restringe aos dias mortos que dispararam a consulta. Duração real: [nao verificado].

3. **`fio_do_admin_sem_resposta` não vê falha interna de C9.** `lista` captura erro em `_teto_do_dia`, escreve log e segue (`app/escala/services/cadastro_realidade.py:561`–`:567`); o vigia só marca falha se sua chamada inteira levantar (`app/chamados/services/vigia_fio.py:260`–`:265`). Assim, C9 pode falhar e o contador continuar zero. Os dias permanecem no alarme, pois não haverá dono elegível no mapa (`app/chamados/services/vigia_fio.py:266`).

4. **Pergunta de competência tem uma cópia antiga alcançada pelo novo fluxo.** C2 fixa `periodo_apuracao(hoje.month, hoje.year, 21)` (`app/escala/services/cadastro_realidade.py:187`), embora o registro indique `janela_atual/corte_da_empresa`, expressamente “nunca 21 cravado” (`app/docs/JUIZES.md:55`–`:62`). É pré-existente, não duplicação nova do juiz do dono.

Premissas e escritores/leitores:

- **O dono da divergência tem um decisor neste fluxo:** `dono_da_divergencia`, com precedência CADASTRO por ausência de vínculo/folga, BATIDA por aberto/paridade, depois CADASTRO/ESTRUTURA (`app/ponto/management/commands/e6_oraculo.py:90`–`:100`). `InsumosDoDono.dono` monta insumos e chama esse decisor (`app/ponto/management/commands/e6_oraculo.py:146`–`:155`).
- **Leitores de `InsumosDoDono.dono`:** e6 (`app/ponto/management/commands/e6_oraculo.py:338`); pautas (`app/ponto/management/commands/pautas_discordantes.py:104`); medição C9 (`app/ponto/services/teto_na_lista.py:86`). Os construtores correspondentes estão em e6 `:288`, pautas `:82` e teto `:72`.
- **Pautas ainda tem pré-filtro separado**, chamando diretamente o mesmo juiz com `False` antes de `_ins.dono` (`app/ponto/management/commands/pautas_discordantes.py:93`–`:104`). Não é outro juiz, mas outra montagem parcial da pergunta.
- **Escrita persistente do dono C9:** `medir` põe o retorno em `itens[*]['dono']` (`app/ponto/services/teto_na_lista.py:78`–`:86`); `lavrar` grava o dict em `MetricaSnapshot.detalhe` (`app/ponto/services/teto_na_lista.py:110`–`:116`); comando chama ambos (`app/ponto/management/commands/lavrar_teto_do_dia.py:27`–`:34`).
- **Leitura persistente:** `ultimo_lavrado` busca o snapshot mais recente (`app/ponto/services/teto_na_lista.py:123`–`:129`); `_teto_do_dia` transforma `dono` em `dono_do_dia` e copia a `data` (`app/escala/services/cadastro_realidade.py:310`–`:313`); `lista` usa esse dono para rótulo e ação (`app/escala/services/cadastro_realidade.py:595`–`:607`).
- **O vigia não recalcula o dono:** `dias_na_lista` copia `dono_do_dia` das assinaturas com data (`app/escala/services/cadastro_realidade.py:666`–`:670`); `_fio_do_admin` só verifica a chave e os valores BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:266`).
- **Fonte de lista compartilhada, origens múltiplas:** a tela usa `lista(empresas={visíveis})` (`app/escala/views.py:490`); vigia usa a mesma função e cruza com seu recorte (`app/chamados/services/vigia_fio.py:254`–`:259`). `lista` combina cadastro atual, lavras C e lavra A (`app/escala/services/cadastro_realidade.py:513`–`:575`), e só inclui linhas das empresas selecionadas (`app/escala/services/cadastro_realidade.py:588`–`:589`).
- **Não confundir essa lista com `dias_cadastro_x_realidade` do motor:** o motor escreve relatos por dia (`app/ponto/motor_calculo_v2.py:1593`–`:1610`) e publica o atributo (`app/ponto/motor_calculo_v2.py:2657`); `InsumosDoDono.cxr` lê esse atributo do espelho (`app/ponto/management/commands/e6_oraculo.py:136`–`:143`). A lista do admin se compõe por outro pipeline (`app/escala/services/cadastro_realidade.py:502`–`:575`).
- **Única assinatura com `data` estruturada atualmente é C9:** escritor em `app/escala/services/cadastro_realidade.py:312`. C4 põe dias somente em texto (`app/escala/services/cadastro_realidade.py:372`–`:376`); C5 também (`app/escala/services/cadastro_realidade.py:142`–`:143`); assinaturas A são agregados sem `data` (`app/escala/services/cadastro_realidade.py:617`–`:618`). Logo “dia na lista” operacionalizado pelo helper alcança C9, não todos os dias mencionados em detalhes.
- **Leitor direto de `data` estruturada:** `dias_na_lista` (`app/escala/services/cadastro_realidade.py:669`–`:670`). HTML mostra a data incorporada em `detalhe` e o dono de apresentação (`app/templates/escala/cadastro_x_realidade.html:28`); PDF também (`app/escala/services/cadastro_realidade.py:711`–`:714`).
- **Outros leitores de `lista`, fora do diff:** tela/PDF (`app/escala/views.py:490`–`:495`); fechamento de avisos C7 (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:26`–`:33`); fechamento de chamados C8 (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:30`–`:41`).
- **Relógio independente anterior no produtor C9:** comando captura `timezone.localdate()` (`app/ponto/management/commands/lavrar_teto_do_dia.py:27`), depois `medir` captura outro `timezone.now()` (`app/ponto/services/teto_na_lista.py:49`–`:50`). Isso está fora do diff.
- **Snapshot C9 é o último disponível, sem filtro de idade:** `app/ponto/services/teto_na_lista.py:123`–`:129`. A lista não entrega metadados C9 de frescor ao helper (`app/escala/services/cadastro_realidade.py:310`–`:318`); não é possível concluir frescor de produção: [nao verificado].

Selos relevantes para sua consolidação: teste integração lavra → lista verifica C9 e detalhes (`app/ponto/tests/test_col129_teto_vira_lista.py:70`–`:79`), dono CADASTRO (`:88`–`:95`) e turnos da competência ±1 dia (`:97`–`:114`). Os novos testes do vigia substituem a lista por mock, portanto não exercitam o custo, relógios secundários ou falha interna real da C9 (`app/chamados/tests/test_o261_vigia_fio.py:541`–`:551`).

_471 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
