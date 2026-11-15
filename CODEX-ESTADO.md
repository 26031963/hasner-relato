# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 10:26** · publicado em **15/11/2026 10:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 10:26 · sessao f08063655a32 · - · - · origem=Ronald (tmux)

Chamadores e falha:

- Único chamador direto produtivo: `executar_cancelamento_admin`, em `app/ferias/services.py:683`, dentro do `atomic` de `app/ferias/services.py:674`; antes trava agendamento (`app/ferias/services.py:636`) e período (`app/ferias/services.py:639`). A função acrescenta `atomic` interno (`app/ponto/services/ausencia.py:1416`) e trava Ausencias (`app/ponto/services/ausencia.py:1417`).
- Chamadas diretas em testes: `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `:191`, `:200`. O arquivo `selo_hj091.py:26` apenas enumera o nome, não chama.
- Entradas produtivas indiretas: individual (`app/ferias/views.py:396`), correção concluída (`app/ferias/views.py:444`) e lote (`app/ferias/views.py:605`), pelo adaptador `app/ferias/views.py:557–561`. Também existe a simulação do command `listar_ferias_dp`, que chama o service em `app/ferias/management/commands/listar_ferias_dp.py:100`, sob outro `atomic` em `:98`.
- `evento` usa seu próprio savepoint (`app/core/observ.py:133`), engole falhas (`:105`, `:87–91`) e retorna `None` (`:139`). A nova checagem lança `RuntimeError` dentro do atomic interno (`app/ponto/services/ausencia.py:1428–1434`), desfazendo UPDATE e trilhas já gravadas nessa chamada (`:1423–1434`). A exceção sai sem alcançar invalidação (`:1435`).
- No chamador produtivo, também desfaz a devolução de saldo já salva (`app/ferias/services.py:680–683`); não chega à gravação de status/obs (`:684–689`) nem ao refresh/retorno (`:690–691`). As três views capturam apenas `AgendamentoNaoCancelavel` (`app/ferias/views.py:397`, `:445`, `:606`), portanto `RuntimeError` escapa; resposta HTTP concreta [não verificado]. No lote, itens anteriores já concluíram seus próprios atomics; o item falho aborta a continuação do loop (`app/ferias/views.py:600–611`; `app/ferias/services.py:674–691`). O command simulado captura a exceção e produz `ERRO:RuntimeError:...`, depois recarrega o agendamento (`app/ferias/management/commands/listar_ferias_dp.py:112–115`).

Travas:

- No fluxo principal, a nova trava respeita a ordem declarada agendamento → período → ausencia (`app/ferias/services.py:629–639`; `app/ponto/services/ausencia.py:1417–1419`). Editar ausência genericamente não é contraparte sobre férias: a guarda recusa esse tipo (`app/ponto/services/ausencia.py:579`, `:92–98`).
- Há uma inversão plausível **entre linhas de Ausencia** contra a reversão do backfill. O novo SELECT herda ordenação por `-data, colaborador__nome` (`app/ponto/models.py:829`), pois não sobrescreve `order_by` (`app/ponto/services/ausencia.py:1417–1419`). A reversão abre atomic e atualiza uma linha por vez na ordem do arquivo (`app/ponto/services/ausencia.py:1647–1649`, UPDATE em `:1535–1536`), sem travar agendamento/período. O apply monta o arquivo a partir de linhas por PK (`:1605–1606`), preserva essa ordem na classificação (`:1477`, `:1511`) e grava `vincular` (`:1617`). Com A1 de data anterior e A2 posterior: cancelamento pode travar A2 e esperar A1; reversão pode atualizar/travar A1 e esperar A2. A existência do escritor e as ordens estão no código; deadlock efetivamente ocorrido [não verificado].
- `colaborador__nome` também implica JOIN no SELECT e, sem `of=('self',)`, pode ampliar o conjunto bloqueado ao colaborador: inferência do ORM a partir de `app/ponto/models.py:829` e `app/ponto/services/ausencia.py:1417–1419`; SQL efetivo [não verificado].

## 15/11 10:23 · sessao faed775e58a7 · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

Há uma falha de competência no novo caminho de `desfazer`: uma saída após a meia-noite pode recalcular a competência seguinte e deixar velha a competência do turno. A rejeição em massa também continua podendo gravar o ato sem a trilha prometida. (`app/ponto/services/retratar_humano.py:212`; `app/ponto/services/fechamento.py:1567`, `1809`, `627`; `app/ponto/services/ausencia.py:1418`; `app/core/observ.py:139`)

Execução dos testes, identidade criptográfica da cópia e mudança efetiva em produção: **[nao verificado]**. O material fornecido contém código e anexos, sem linhas de produção que permitam medir pessoas, competências ou valores afetados: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos**

O censo abaixo identifica os caminhos encontrados na cópia; completude contra escrita dinâmica, SQL montado em execução ou processos externos: **[nao verificado]**. O próprio censo existente declara limitações de inferência. (`app/core/censo_escritas.py:8`, `42`, `47`)

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `Ausencia.status` | Criação; `mudar_estado`; rejeição bulk de férias. (`app/ponto/services/ausencia.py:440`, `926`, `938`, `1418`) | Cobertura para folha exige `aprovada`; cobrança admite qualquer não rejeitada. Estado aberto sai de `ABERTAS`. (`app/ponto/turnos.py:2511`; `app/ponto/services/ausencia.py:866`) |
| `Ausencia.data` | Criação e edição. (`app/ponto/services/ausencia.py:438`, `603`) | Data inicial da cobertura; também serve como dia inicial para crédito parcial, contagens e apresentação. (`app/ponto/turnos.py:2797`; `app/ponto/services/credito_parcial.py:27`; `app/ferias/models.py:149`) |
| `Ausencia.data_fim` | `derivar_data_fim`; criação/edição; derivação no modelo, persistida por transição ou rederivação. (`app/ponto/services/ausencia.py:152`, `439`, `604`, `936`, `1659`; `app/ponto/models.py:917`) | Fim **inclusivo**; vazio abre somente tipos `TIPOS_EM_ABERTO`; nos demais vale um dia. (`app/ponto/turnos.py:2786`) |
| `Ausencia.tipo` | Criação e edição. (`app/ponto/services/ausencia.py:438`, `602`) | Decide catálogo/efeito, abertura do intervalo e classificação parcial; o fechamento consulta esses julgamentos. (`app/ponto/models.py:850`; `app/ponto/turnos.py:2793`; `app/ponto/services/fechamento.py:681`, `694`) |
| `Ausencia.minutos` | Entrada da criação; edição; modelo deriva previsto da falta aprovada quando zero; transição/rederivação persistem. (`app/ponto/services/ausencia.py:439`, `612`, `936`, `1659`; `app/ponto/models.py:887`) | Minutos, convertidos em horas por `/60`; falta de intervalo é total rateado pelos dias; crédito parcial fica no dia inicial. (`app/ponto/models.py:875`; `app/folha/export.py:118`, `121`; `app/ponto/services/credito_parcial.py:27`) |
| `Ausencia.dias_corridos` | Entrada; edição; modelo deriva `(fim−início)+1`. (`app/ponto/services/ausencia.py:438`, `605`; `app/ponto/models.py:903`, `909`) | Duração civil inclusiva para derivar fim e apresentar/diagnosticar; não encontrei leitura direta desse campo no cálculo mensal. (`app/ponto/models.py:912`; `app/ponto/services/dia_decidido.py:621`; `app/ponto/services/vigia_ausencia.py:135`; `app/ponto/services/fechamento.py:638`, `683`, `874`) |
| `Ausencia.aprovada_em` | Criação aprovada; modelo completa em **aprovada ou rejeitada**; `decidir` aceita instante fornecido pelo chamador; rederivação pode completá-lo. (`app/ponto/services/ausencia.py:445`, `1026`, `1659`; `app/ponto/models.py:882`) | Instante de decisão para histórico e frescor, com fallback para criação; apresentação converte para hora local. (`app/ponto/services/dia_decidido.py:508`; `app/ponto/services/cartorio.py:385`; `app/templates/ponto/partials/painel_ausencia.html:275`) |
| `FechamentoMensal.derivado_em` | Recálculo aberto por `save`; demais estados por `update`; restauração põe NULL por padrão; invalidação põe NULL. (`app/ponto/services/fechamento.py:1143`, `1189`, `1391`, `1656`) | Carimbo de derivação; NULL reúne “nunca derivado” e “invalidado”. Contador/listagem padrão olham competência corrente. (`app/ponto/services/fechamento.py:1679`, `1692`, `1702`; `app/ponto/management/commands/projecoes_vencidas.py:49`) |
| `Justificativa.status` | Nascimento na API, tela, cron e regularização; decisão/materialização no serviço; lotes na tela; rejeição no reconciliador; regularização pode voltar a pendente. (`app/api/views.py:1999`; `app/ponto/views.py:1638`, `1799`, `1878`; `app/ponto/management/commands/processar_alertas_turno.py:394`; `app/ponto/services/justificativa.py:205`, `283`, `383`; `app/chamados/reconciliador.py:532`; `app/ponto/services/regularizacao.py:106`) | Estados próprios `pendente/aprovada/rejeitada`; cálculo lê somente aprovada pelo **dia do turno**. (`app/ponto/models.py:130`, `144`, `159`; `app/ponto/services/fechamento.py:660`) |
| `Batida.retratada_em` | `retratar_batida` grava `now()` por `anotar`; desfazer grava NULL pela mesma porta. (`app/ponto/registro_batida.py:254`, `276`; `app/ponto/services/retratar_humano.py:207`) | Presença exclui da apuração; instante também governa prazo humano de sete dias e frescor. (`app/ponto/turnos.py:2876`; `app/ponto/services/retratar_humano.py:53`, `109`; `app/chamados/veredito_lavrado.py:180`) |
| `LogAuditoria.acao/valor_antes/valor_depois` | Persistência runtime por `evento`; `registrar_log` e `evento_kw` adaptam argumentos. O novo bulk usa essa porta. (`app/core/observ.py:133`, `146`; `app/core/auditoria.py:22`; `app/ponto/services/ausencia.py:1419`) | Ação textual e JSON sem esquema comum; cada domínio interpreta seu payload. Desfazer lê última ação; relatórios humanos leem motivo; flips aceitam formatos históricos diferentes. (`app/core/models.py:296`, `300`; `app/ponto/services/retratar_humano.py:35`, `51`, `95`, `247`; `app/ponto/services/flip_batida.py:46`, `73`) |

Os principais leitores adicionais encontrados, agrupados para tornar o alcance conferível, são:

| Família | Sítios leitores |
|---|---|
| Ausência: geometria, precedência, dinheiro e lavra | `app/ponto/turnos.py:2531`, `2554`, `2600`, `2677`, `2715`, `2753`, `2786`, `2827`, `3076`, `3138`; `app/ponto/precedencia.py:273`, `337`; `app/ponto/services/fechamento.py:638`, `683`, `694`, `874`; `app/folha/export.py:70`, `87`, `117`, `140`, `160`; `app/ponto/services/efeito_ausencia.py:47`; `app/ponto/services/credito_parcial.py:26`; `app/ponto/services/cartorio.py:383`, `1356`; `app/ponto/signals.py:123`. |
| Ausência: chamados e cobrança | `app/chamados/services/materializacao.py:79`, `626`, `662`; `app/chamados/services/silencio_ausencia.py:66`, `78`, `94`; `app/chamados/juizes.py:479`, `1801`, `2380`; `app/chamados/services/historico.py:129`; `app/chamados/services/foto_e4b.py:251`, `285`; `app/chamados/services/adesao.py:69`; `app/chamados/services/vigia_fio.py:308`. |
| Ausência: grade, espelho e calendário | `app/escala/utils.py:1174`; `app/escala/servico_jornada.py:347`; `app/escala/services/cadastro_realidade.py:603`; `app/escala/services/furos_diarios.py:222`; `app/ponto/services/espelho.py:582`; `app/colaboradores/services/calendario.py:484`, `532`; `app/api/views.py:1566`. |
| Ausência: férias, relatórios e inteligência | `app/ferias/models.py:147`; `app/ferias/services.py:147`; `app/relatorios/views.py:197`, `277`, `304`; `app/inteligencia/views.py:71`, `232`, `516`; `app/inteligencia/resumo.py:39`; `app/inteligencia/management/commands/calcular_scores.py:12`; `app/inteligencia/management/commands/gerar_alertas.py:55`. |
| Ausência: serviços operacionais e vigias | `app/ponto/services/dia_decidido.py:448`, `481`, `508`, `550`, `614`; `app/ponto/services/vigia_ausencia.py:59`, `104`, `135`, `215`; `app/ponto/services/vigia_ausencia_veredito.py:46`; `app/ponto/services/alertas_ausencia.py:48`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`; `app/ponto/services/afastado_avisa.py:39`, `127`; `app/ponto/services/aviso_da_batida.py:88`; `app/colaboradores/services/situacao_exibida.py:63`. |
| Ausência: telas/API | `app/ponto/views.py:1005`, `2079`, `2190`, `2230`, `2293`, `2370`, `2615`, `2705`, `2801`, `2853`, `2916`; `app/ponto/views_tipos_ausencia.py:24`; `app/api/views.py:1056`, `1853`, `1865`, `3076`; `app/api/views_mensageria.py:744`, `763`, `1034`, `1073`, `1246`; `app/colaboradores/services/detalhe.py:104`; `app/colaboradores/views.py:2927`; `app/ponto/admin.py:44`. |
| Ausência: templates | `app/templates/ponto/ausencias.html:150`; `app/templates/ponto/partials/painel_ausencia.html:51`, `123`, `152`, `275`, `310`; `app/templates/ponto/espelho.html:191`; `app/templates/ponto/partials/painel_fechamento.html:55`; `app/templates/ponto/historico_atestados.html:39`; `app/templates/chamados/fio_colaborador.html:62`; `app/templates/colaboradores/detalhe.html:74`; `app/templates/relatorios/afastados.html:40`. |
| Ausência: comandos de diagnóstico/cura | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:99`, `142`, `167`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `38`, `49`; `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:36`; `app/ponto/management/commands/fechar_chamados_ausencia.py:34`; `app/ponto/management/commands/reconciliar_grade.py:401`; `app/ponto/management/commands/processar_alertas_turno.py:97`; `app/ponto/management/commands/processar_alertas_avancados.py:115`; `app/ponto/management/commands/detectar_ausencias.py:819`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:33`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`; `app/ponto/management/commands/diff_calculador.py:408`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `642`, `1469`. |
| Justificativa: cálculo e operação | `app/ponto/services/efeito_ausencia.py:40`; `app/escala/utils.py:1197`; `app/relatorios/management/commands/diff_extrato_parcial.py:42`; `app/ponto/views.py:1001`, `1707`, `1739`; `app/api/views.py:2095`; `app/core/views.py:205`, `244`, `1191`; `app/chamados/views.py:596`; `app/ponto/utils_turno_orfao.py:53`; `app/templates/ponto/partials/painel_justificativa.html:47`. |
| Batida retratada: apuração e projeções | `app/ponto/turnos.py:1602`, `1864`, `2221`, `2353`, `2469`, `2876`, `2915`; `app/ponto/motor_calculo_v2.py:671`; `app/ponto/services/cartorio.py:381`, `1204`; `app/chamados/reconciliador.py:217`; `app/chamados/juizes.py:758`, `817`; `app/chamados/services/materializacao.py:640`; `app/colaboradores/services/calendario.py:240`, `621`; `app/core/services/painel_op.py:93`; `app/escala/services/propositor.py:478`; `app/escala/services/meio_periodo.py:160`; `app/ferias/situacao.py:279`. |
| Batida retratada: demais consumidores | `app/ponto/registro_batida.py:109`, `141`; `app/api/views.py:676`; `app/api/views_core.py:667`; `app/colaboradores/services/ficha.py:145`; `app/colaboradores/services/pos_vinculo.py:39`; `app/colaboradores/services/senha_provisoria.py:25`; `app/chamados/services/fila_por_causa.py:191`; `app/chamados/services/desvio_escala.py:60`; `app/chamados/services/limbo_folgas.py:38`, `157`; `app/ponto/services/passivo_s84.py:54`; `app/ponto/services/adesao.py:7`, `35`, `58`; `app/ponto/services/piso_intervalo.py:41`; `app/ponto/services/flip_batida.py:15`; `app/ponto/services/flip_auto.py:229`, `287`; `app/ponto/services/lastro.py:262`; `app/ponto/services/faltas_de_hoje.py:55`; `app/ponto/services/deslogados.py:161`. |
| Auditoria: leitores genéricos e outros domínios | `app/core/views.py:705`; `app/chamados/services/historico.py:86`; `app/escala/services/regeneracao.py:58`; `app/chamados/models.py:404`; `app/chamados/reconciliador.py:480`; `app/chamados/juizes.py:1300`; `app/chamados/services/vigia_loop.py:32`, `116`. |

**Os significados divergem nos seguintes pontos:**

- **Fim vazio:** o help do modelo diz indeterminado; o juiz restringe essa interpretação aos tipos em aberto. O histórico de dias decididos usa sempre `data_fim or data`; o copiloto chama qualquer fim vazio de “sem data de volta”. São leituras incompatíveis para uma mesma linha aberta. (`app/ponto/models.py:751`; `app/ponto/turnos.py:2791`; `app/ponto/services/dia_decidido.py:552`, `558`; `app/api/views_mensageria.py:1077`)
- **Status:** há leitores que ainda procuram `Ausencia.status == 'pendente'`, embora os estados abertos sejam `aguardando_documento/aguardando_decisao`. (`app/api/views.py:1061`; `app/templates/ponto/espelho.html:191`; `app/ponto/models.py:701`; `app/ponto/services/ausencia.py:866`)
- **Duração aberta:** o histórico identifica aberto por `dias_corridos` vazio/zero; a criação permite afastamento aberto com o default de um dia. (`app/ponto/services/dia_decidido.py:621`; `app/ponto/services/ausencia.py:216`, `438`)
- **Contagem de faltas:** férias conta registros aprovados cujo início cai no período aquisitivo; folha expande e rateia os dias do intervalo. Uma falta de três dias é um registro para aquele contador. (`app/ferias/models.py:147`; `app/folha/export.py:114`, `118`, `140`)
- **Carimbo decisório:** `save` completa `aprovada_em` também para rejeição; o bulk de férias altera apenas estado e atualização, conservando eventual carimbo/decisor da aprovação anterior. (`app/ponto/models.py:882`; `app/ponto/services/ausencia.py:1418`)
- **Dia civil versus turno:** o evento usa data local do timestamp; a apuração atribui o turno à entrada. Essa divergência alcança o novo desfazer. (`app/ponto/services/fechamento.py:1567`, `1809`, `627`; `app/ponto/services/retratar_humano.py:212`)
- **Auditoria:** antes/depois não possuem esquema universal; os leitores de flip já precisam aceitar string, dict e NULL. O novo payload de rejeição é dict de estado. (`app/core/models.py:300`; `app/ponto/services/flip_batida.py:46`; `app/ponto/services/ausencia.py:1423`)

O fuso configurado é `America/Sao_Paulo`, com `USE_TZ=True`; os timestamps usados pela competência são convertidos para data local. Não identifiquei uma divergência nova de unidade minutos/horas ou de fim inclusivo nesses caminhos. (`app/config/settings/base.py:93`; `app/ponto/services/fechamento.py:1567`; `app/ponto/models.py:875`; `app/ponto/turnos.py:2799`)

**2. Escritores de Ausencia fora dos sítios alterados**

Não identifiquei produtor runtime explícito fora da porta que, em seu caminho normal, escape das invalidações agora acrescentadas. A porta declarada é `services/ausencia.py`, o admin é somente leitura e os chamadores de férias passam pelos helpers. Isso não certifica escrita dinâmica externa: **[nao verificado]**. (`app/core/portas.py:486`; `app/ponto/admin.py:37`; `app/ferias/models.py:444`, `451`; `app/ferias/services.py:683`, `725`)

Há estas distinções necessárias:

- O **escritor físico do modelo** continua fora do diff: `Ausencia.save` deriva minutos, duração, fim e carimbo sem invalidar autonomamente. Sua cobertura depende de quem o chama. (`app/ponto/models.py:878`; `app/ponto/services/ausencia.py:938`, `1659`)
- A migração histórica escreve status diretamente e sua reversão também; nesses sítios não há invalidação. Isso é código de migração, sem evidência de execução nesta produção. (`app/ponto/migrations/0057_a4_backfill_pendente.py:43`, `63`)
- A exclusão de ensaio não invalida e exige banco de ensaio. (`app/ponto/services/ausencia.py:1643`)
- A FK de agendamento tem `CASCADE`; a exclusão individual do modelo chama o helper antes de apagar. Um bypass por exclusão bulk do pai não recebe esse override, mas não identifiquei chamador runtime desse bypass. (`app/ponto/models.py:796`; `app/ferias/models.py:451`)
- Testes escrevem diretamente, inclusive por bulk; isso não demonstra escritor de produção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:79`, `81`; `app/ponto/tests/test_hj091_ferias_derivadas.py:945`)

O backfill de origem altera somente `agendamento_ferias_id`; não altera os insumos financeiros enumerados. (`app/ponto/services/ausencia.py:1514`, `1525`)

**3. Juiz duplicado, derivação paralela, escritor e relógio**

A nova função delega os dias a `dias_da_ausencia`, depois delega a invalidação; esta delega as competências a `competencias_do_fato`, que adapta `competencias_do_intervalo`. Não introduz outra fórmula de cobertura ou de competência. (`app/ponto/services/ausencia.py:142`, `149`; `app/ponto/services/fechamento.py:1649`, `1594`)

Ela calcula, porém, **um envelope entre o menor início e o maior fim**. Mover uma ausência entre intervalos distantes invalida também competências do buraco entre eles. É invalidação conservadora além da união exata dos fatos; não é nova apuração financeira. (`app/ponto/services/ausencia.py:145`; `app/ponto/services/fechamento.py:1656`)

A rejeição bulk permanece um segundo mecanismo de transição em relação à máquina `mudar_estado`, mas já existia no diff-base anexado; a alteração acrescenta fotografia, trilha e invalidação. O log continua passando pelo escritor existente. (`app/ponto/services/ausencia.py:909`, `1412`, `1418`, `1419`; `app/core/observ.py:133`)

**Há relógio novo fora da borda:** `_acordar_projecao` chama `localdate()` internamente, sem parâmetro `hoje/agora`; passa a executar isso inclusive em criação, reversão, espelho e rederivação. Uma decisão também pode chamar o helper duas vezes e obter tetos distintos numa virada de dia. (`app/ponto/services/ausencia.py:137`, `551`, `942`, `1021`, `1055`, `1306`, `1664`)

A premissa “competência futura não tem projeção gravada” não é imposta pelo recálculo, que recebe mês/ano explícitos. Se houver projeção futura de ausência aberta, o teto em hoje a deixa fresca; existência desse caso em produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:130`, `142`; `app/ponto/services/fechamento.py:448`, `898`; `app/ponto/management/commands/recalcular_fechamento.py:78`)

**4. Transações, travas e efeitos antes do commit** [nao verificado]

| Caminho | Ordem e consequência |
|---|---|
| Cancelamento admin de férias | `atomic` → trava agendamento → trava período → rejeita/invalida → salva cancelamento e espelha/apaga/invalida. As novas escritas no fechamento ficam dentro da transação do cancelamento. (`app/ferias/services.py:636`, `639`, `674`, `683`, `689`; `app/ferias/models.py:448`) |
| Cancelamento DP | Salva período e agendamento dentro de `atomic`; o `save` chega ao espelho e à nova invalidação. (`app/ferias/services.py:651`; `app/ferias/models.py:448`; `app/ponto/services/ausencia.py:1306`) |
| Remarcação | Trava agendamento/período; apaga dias antigos e invalida; salva nova janela, criando espelho e invalidando; tudo no mesmo `atomic`. (`app/ferias/services.py:706`, `707`, `725`, `729`; `app/ponto/services/ausencia.py:551`, `1395`) |
| Curador de minutos | A edição e sua nova preparação de invalidação rodam sob trava da ausência e `atomic` do comando. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `139`, `169`) |
| Regularização | Grava justificativa e invalida no mesmo `atomic`; busca e julgamento da órfã acontecem antes dele, sem trava nesse fluxo. (`app/ponto/services/regularizacao.py:52`, `61`, `89`, `107`, `113`) |
| Desfazer retratação | Salva/anota primeiro; o recálculo é registrado em `on_commit`. Dentro de transação espera o commit; em autocommit pode executar durante a chamada. (`app/ponto/services/retratar_humano.py:207`, `213`; `app/ponto/registro_batida.py:256`; `app/ponto/services/fechamento.py:1824`) |

`invalidar_fechamento` tem savepoints e captura exceções: sua falha permite conservar o ato, podendo deixar projeção fresca ou invalidação parcial entre competências. **A nova preparação não possui essa proteção:** importação, relógio e expansão dos dias rodam antes de entrar no helper protegido; uma exceção ali pode desfazer o ato no chamador transacional, ou escapar depois de um `save` já confirmado em autocommit. Ocorrência real: **[nao verificado]**. (`app/ponto/services/fechamento.py:1648`, `1651`, `1661`; `app/ponto/services/ausencia.py:133`, `142

[truncado em 20000 de 27838 caracteres]

## 15/11 10:23 · sessao 0aa7421a8dab · SITUACAO-DESLIGADO-SEM-PORTA · verificador · origem=Code

1. **Selfie: o onboarding continua sendo concluído pelo model.** `salvar_campos(..., ['foto'])` chama `Colaborador.save(update_fields=...)`; quando não há etapas pendentes, o override acrescenta `onboarding_pendente` e `onboarding_completo`. As etapas verificadas são os timestamps de câmera e GPS, e a selfie não os atribui nesse request (`app/core/views.py:1063`; `app/colaboradores/services/colaborador.py:108`; `app/colaboradores/models.py:406`; `app/colaboradores/models.py:420`).

   O receiver de `post_save` de **Colaborador** encontrado é `_isentos_post_save_colaborador`: ele ignora `update_fields`, verifica `raw`/`isento_ponto` e agenda o silenciamento em `on_commit`. Portanto, nesse receiver, trocar save completo por parcial não altera a condição de execução (`app/chamados/signals.py:270`).

   **Há diferenças de persistência além de `situacao`:** `atualizado_em`, declarado `auto_now=True`, fica fora do save parcial. A normalização de CPF e a geração de matrícula ausente continuam acontecendo em memória dentro de `save`, mas esses campos também ficam fora de `update_fields`. A própria view não atribui outro campo de cadastro antes de salvar a foto; os dois campos de onboarding são a ampliação explícita feita pelo model (`app/colaboradores/models.py:381`; `app/colaboradores/models.py:417`; `app/colaboradores/models.py:427`; `app/core/views.py:1048`; `app/colaboradores/services/colaborador.py:108`).

   A resposta de sucesso continua sendo `{'ok': True}`, consumida pela tela para registrar a permissão de câmera em outra requisição. Impacto observado dessas diferenças em usuários de produção: `[nao verificado]` (`app/core/views.py:1066`; `app/templates/core/onboarding.html:223`).

2. **O teste rejeita o save completo descrito e não passa por ausência de chamadas.** O patch é aplicado na classe `Colaborador`, envolve o POST, registra `kwargs.get('update_fields')` antes de executar o método original e rejeita qualquer chamada com `None` (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:237`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:249`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:254`).

   Se `FieldFile.save(save=True)` chama `instance.save()` sem argumentos, esse patch captura a chamada e registra `None`, fazendo o teste falhar. O despacho interno de `FieldFile` não está disponível no fonte local; sua confirmação e a execução efetiva contra **77dfa94fa** ficam `[nao verificado]`. A interceptação e a assertiva estão em `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239` e `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:254`.

   Os escapes sugeridos não produzem aprovação vazia:

   - Redirect/HTML faz `r.json()` falhar; JSON sem `ok=True` reprova; nenhuma chamada capturada reprova (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:251`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:252`).
   - Não há middleware de onboarding nas listas declaradas; o gate de plataforma libera UA ausente, compatível com esse `Client()` (`app/config/settings/base.py:53`; `app/config/settings/saas.py:67`; `app/core/middleware.py:272`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:234`).
   - O teste usa `MEDIA_ROOT` temporário; falha de storage/save vira `ok=False` na view e reprova a assertiva (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:248`; `app/core/views.py:1067`).

   **Limite da cobertura:** ele permite `update_fields=[]` ou outros campos que não incluam `situacao`; não exige `'foto'`, não identifica o colaborador interceptado e não relê a foto no banco. Também não exercita a conclusão das etapas: a fixture marca `onboarding_completo=True`, mas não fornece timestamps de câmera/GPS (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:52`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:253`).

3. **Não encontrei chamador de produção com `situacao` em `tocar`.** Os usos encontrados de `tocar` levam `atualizado_em` (`app/core/views.py:66`; `app/core/views.py:148`; `app/api/views.py:2483`; `app/api/views.py:2501`; `app/api/views.py:2752`; `app/api/views.py:2763`; `app/api/views_core.py:1013`; `app/api/views_core.py:1070`; `app/api/views_core.py:1081`).

   Os dois chamadores de `gravar` que passam `situacao` em `campos` usam operação `'situacao'`: lançamento de INSS e reversor de afastamento. Outra operação seria recusada por `_conferir` antes da guarda, pois só `'situacao'` autoriza esse campo (`app/ponto/views.py:2472`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/colaboradores/services/colaborador.py:38`; `app/colaboradores/services/colaborador.py:93`).

4. **Não identifiquei inversão prévia Disputa/Chamado/Pergunta → Colaborador nos chamadores atuais abaixo.** Todos os `salvar_inteiro` entram no levantamento porque escrevem `situacao` independentemente da operação (`app/colaboradores/services/colaborador.py:115`).

   | Chamador que escreve `situacao` | Transação e trava anterior à porta |
   |---|---|
   | `vincular_posto` → `salvar_inteiro('posto')` | Sem `atomic` local; nenhuma trava prévia no caminho (`app/colaboradores/views.py:310`; `app/colaboradores/views.py:329`). |
   | `editar_colaborador` → `salvar_inteiro('cadastro')` | `atomic` começa imediatamente antes da porta; nenhuma trava prévia nesse bloco (`app/colaboradores/views.py:1343`; `app/colaboradores/views.py:1345`). |
   | `transferir_posto` → `salvar_inteiro('posto')` | Dentro de `atomic`; antes da porta há apenas atribuições (`app/colaboradores/views.py:1387`; `app/colaboradores/views.py:1391`). |
   | `executar_desligamento` → `salvar_inteiro('situacao')` | Dentro de `atomic`; primeira gravação do bloco. A cascata de disputas/chamados vem depois (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:164`; `app/colaboradores/services/desligamento.py:186`). |
   | `lancar_ausencia` → `gravar(situacao='afastado')` | Sem `atomic` externo local; o bloco próprio de `criar_ausencia` já terminou quando a view grava situação (`app/ponto/views.py:2452`; `app/ponto/views.py:2472`; `app/ponto/services/ausencia.py:478`; `app/ponto/services/ausencia.py:512`). |
   | `reverter_situacao_afastado` → `gravar(situacao=SITUACAO_VOLTA)` | Sem `atomic` local nem trava anterior no comando (`app/ponto/management/commands/reverter_situacao_afastado.py:54`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`). |

   O `gate_disputa` apenas consulta e delega; não mantém transação/trava. No desligamento ele é isento e delega diretamente (`app/colaboradores/mixins.py:300`; `app/colaboradores/mixins.py:307`; `app/colaboradores/mixins.py:313`; `app/colaboradores/views.py:1425`).

   Não encontrei `salvar_campos`/`atualizar_em_lote` de produção levando `situacao`; os usos encontrados levam foto, onboarding, cadastro ou aparelho (`app/core/views.py:1065`; `app/core/views.py:1094`; `app/core/views.py:1140`; `app/colaboradores/views.py:1218`; `app/colaboradores/management/commands/importar_telefones_nexti.py:113`; `app/ponto/views.py:1964`; `app/colaboradores/fcm.py:61`; `app/colaboradores/management/commands/backfill_plataforma_fossil.py:73`; `app/colaboradores/management/commands/normalizar_identificadores.py:109`).

   O ramo **agendado** usa operação `'situacao'`, mas grava somente data e motivo, sem escrever situação (`app/colaboradores/services/desligamento.py:110`).

   Assim, adquirir `trava_na_ordem(colab.pk)` antes da guarda respeitaria a ordem nesses caminhos, **desde que o mesmo `atomic` abranja guarda e gravação**. A ordem implementada é Colaborador → Disputa → Chamado → Pergunta. A guarda atual consulta sem essa trava e salva depois; a janela permanece no código (`app/chamados/models.py:2744`; `app/chamados/models.py:2753`; `app/chamados/models.py:2766`; `app/chamados/models.py:2770`; `app/colaboradores/services/colaborador.py:71`; `app/colaboradores/services/colaborador.py:98`). Ausência de deadlock em execução concorrente: `[nao verificado]`.

   **`executar_desligamento` não trava explicitamente a linha antes de gravar desligado:** abre `atomic`, atribui os campos e chama `salvar_inteiro`; para novo valor `'desligado'`, a guarda retorna antes de consultar o banco (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/colaborador.py:57`). Os chamadores UI e cron também não fazem trava prévia (`app/colaboradores/views.py:1431`; `app/colaboradores/views.py:1437`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`). Efeitos físicos da trava implícita do `UPDATE`, sem banco: `[nao verificado]`.

## 15/11 10:23 · sessao 9f18f348c188 · E4C-F1 · construtor · origem=Code

Entrega **parcial**: 14 unidades migradas, E parcial e `_retratada_com_trilha` pausada por duas perguntas de lei. O selo zero permanece RED. HEAD conferido: `714a92a612907cd94acdf77395b2e8f39fd4c537`. [Entrega completa](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:1).

As linhas antigas são os acessos indicados no preparo; as novas são as definições atuais. [Mapa reconferido](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:6).

| Unidade | Antiga → nova | Destino aplicado | [nao verificado]
|---|---|---|
| `_retratada_com_trilha` | 288 → 358 | Pausada: L2 |
| `_slot_esperado` | 348,360 → 417 | D01; alimenta o derivador F4 |
| `_pergunta_materializada_de_fato` | 679,691,707 → 737 | E/DP/A06; escolha pausada |
| `_chave_memo_pv` | 852–854 → 900 | D06: ato e efeitos |
| `_resposta_concorda_com_a_luz` | 1356 → 1399 | D01 |
| `projecao_temporal_da_pergunta` | 1385–1386 → 1447 | DP até o instante + E |
| `pergunta_sem_lastro_ou_viva` | 1496 → 1558 | P/E |
| `respondida_sem_texto` | 1528 → 1591 | D01/conservação |
| `q_respondida_sem_texto` | 1548 → 1659 | Subquery/conservação |
| `q_respondida_com_texto` | 1556 → 1664 | Subquery/conservação |
| `perguntas_corrigiveis` | 1715 → 1823 | D01/P/E |
| `perguntas_respondidas_a_validar` | 1724 → 1830 | D01/P |
| `declarou_hora` | 1750 → 1845 | D01 |
| `delta_da_resposta` | 1897 → 1997 | D01 |
| `classificar_resposta_validacao` | 2001,2006 → 2103 | D01 |
| `motivo_implausivel_validacao` | 2023 → 2125 | D01 |

A leitura mecânica foi criada em [atos_pergunta.py:19](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/services/atos_pergunta.py:19). A06, conservação E4b, assinatura A05 e sondador da guarda de escritor foram preservados; A03/A17/A18/A20 permanecem fora da migração desta fatia. [Exceções](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:24).

Os seis testes F1 cobrem fonte no ato, projeção contraditória, reabertura, invalidação do memo, passivo e A06. Entraram também os nove casos `DecisoesTest`, o selo AST com apelidos e seu mutante. [Provas e comandos individuais](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:49). [nao verificado]

Resultados locais:

- RED contra a base: **4 falhas de asserção**, com passivo/A06 preservados. [Registro](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_red.txt:1).
- Fonte aplicada: **27 passam; 1 falha**, no selo zero pelos dois resíduos publicados. [Registro](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_memoria.txt:1). [nao verificado]
- **31 mutantes mordidos**, sem contar erros de import como mordida. [Resultados](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_mutantes.txt:1), [comando de cada mutante](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:69).
- `py_compile` executado sem gerar bytecode. Ruff indisponível, rc 127. [Compilação](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_compile.txt:1), [ruff](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_ruff.txt:1).

O [censo por unidade](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:7) relaciona escritores e leitores; o [esmeril semântico](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:38) distingue instante do ato, hora trabalhada, dia do turno e dia civil. Comparações PostgreSQL, contagens reais DP/Batida/Ausencia/FechamentoMensal e reconstrução histórica integral dos efeitos: **[nao verificado]**; os testes ORM ficaram preparados para a sessão. [Contrato ORM](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/test_e4c_f1_orm.py:44). [nao verificado]

As duas perguntas de lei são:

1. Qual fato substitui C na materialização de **escolha**, preservando a distinção entre validação e efeito? [Ramo preservado](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/juizes.py:786).
2. Qual fato conserva o **fechamento obsoleto auditado**, distinguindo-o da remoção do motivo? [Unidade preservada](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/juizes.py:358).

**PERGUNTA DE POUSO ANTECIPADA:** a mão do Ronald precisa definir esses dois fatos antes do selo zero; depois, Code/suite e opus final certificam. Necessidade adicional em kernel de batida, dinheiro ou tela: **[nao verificado]**. [Pendências de pouso](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:110). [nao verificado]

LEI-AKITA: origem=chamados/juizes.py (16 unidades), testemunha=DecisaoPergunta (sequencia ato_em,pk) + juiz registrado do destino, RED=test_e4c_f1_juizes_le_o_ato.F1MemoriaTest+DecisoesTest+SeloF1Test (selo zero RED), quem-mais-le=censo em chamados/tests/e4c_f1_entrega.txt:7, juizes novos=0, padrao=PARALLEL-CHANGE

## 15/11 10:21 · sessao 71fb7e8253e2 · - · - · origem=Ronald (tmux)

Q4 — censo independente, somente leitura:

| Escrita de `situacao` | `atomic` no chamador | Trava prévia de Disputa/Chamado/Pergunta |
|---|---|---|
| `vincular_posto → salvar_inteiro(..., 'posto')` | Sem `atomic` local | Nenhuma no caminho até a porta — `app/colaboradores/views.py:310`, `app/colaboradores/views.py:319`, `app/colaboradores/views.py:329`. |
| `editar_colaborador → salvar_inteiro(..., 'cadastro')` | Sim; porta é a primeira operação no bloco | Nenhuma — `app/colaboradores/views.py:1343`, `app/colaboradores/views.py:1345`. |
| `transferir_posto → salvar_inteiro(..., 'posto')` | Sim; antes da porta só atribui posto/empresa | Nenhuma — `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1391`. O decorator consulta disputa e delega, sem transação/trava — `app/colaboradores/mixins.py:300`, `app/colaboradores/mixins.py:307`, `app/colaboradores/mixins.py:313`. |
| `executar_desligamento → salvar_inteiro(..., 'situacao')` | Sim; primeira escrita no bloco | Nenhuma antes da gravação — `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`. Cascata de disputas/chamados vem depois — `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:186`. |
| `lancar_ausencia → gravar(..., {'situacao': 'afastado'})` | Sem `atomic` externo local | `criar_ausencia` roda antes, mas seu bloco próprio já terminou antes de a view escrever situação — `app/ponto/views.py:2400`, `app/ponto/views.py:2452`, `app/ponto/views.py:2472`, `app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:479`. |
| `reverter_situacao_afastado → gravar(..., {'situacao': SITUACAO_VOLTA})` | Sem `atomic` local | Nenhuma — `app/ponto/management/commands/reverter_situacao_afastado.py:54`, `app/ponto/management/commands/reverter_situacao_afastado.py:62`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. |

Não encontrei chamador de produção de `salvar_campos` ou `atualizar_em_lote` levando `situacao`; o inventário positivo acima cobre os escritores localizados, incluindo todo `salvar_inteiro`, que sempre guarda e grava situação independentemente da operação — `app/colaboradores/services/colaborador.py:115`, `app/colaboradores/services/colaborador.py:118`, `app/colaboradores/services/colaborador.py:119`. Não surgiram chamadores dessas portas em `scripts/` ou `bin/` [nao verificado por execução].

O caminho agendado do executor usa operação `'situacao'`, mas grava somente data e motivo; portanto não entra na guarda atual — `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:110`, `app/colaboradores/services/colaborador.py:94`.

**Conclusão estática:** não localizei inversão prévia dessas quatro entidades nos chamadores existentes. Adquirir `trava_na_ordem(colab.pk)` antes da guarda, com a mesma transação abrangendo a gravação, respeita a ordem nos caminhos examinados; a ordem efetiva é Colaborador, Disputa, Chamado, Pergunta — `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`. Ausência de deadlock em execução concorrente [nao verificado].

**Executor:** não faz `select_for_update`/`trava_na_ordem` do colaborador antes de gravar desligado. Entra no `atomic`, atribui os campos e chama `salvar_inteiro`; além disso, novo valor desligado faz a guarda retornar sem sequer consultar a linha — `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/colaborador.py:57`, `app/colaboradores/services/colaborador.py:118`. A gravação antecede as travas da cascata; efeitos físicos das travas implícitas do `UPDATE`, sem banco [nao verificado].

Os dois chamadores localizados do executor também não pretravam: view carrega por `get_object_or_404` e chama executor; cron filtra e chama executor no laço — `app/colaboradores/views.py:1431`, `app/colaboradores/views.py:1437`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`. O decorator isento da view delega diretamente — `app/colaboradores/views.py:1425`, `app/colaboradores/mixins.py:301`.

## 15/11 10:21 · sessao 7811028f26d1 · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem executar testes nem acessar banco/log: [nao verificado]

- Nenhum chamador de produção encontrado passa `situacao` em `tocar`; os usos encontrados passam somente `atualizado_em`: `app/api/views.py:2483`, `app/api/views.py:2501`, `app/api/views.py:2752`, `app/api/views.py:2763`, `app/api/views_core.py:1013`, `app/api/views_core.py:1070`, `app/api/views_core.py:1081`, `app/core/views.py:66`, `app/core/views.py:148`.
- Os únicos `gravar` de produção com `situacao` em `campos` usam operação `'situacao'`: lançamento INSS, `app/ponto/views.py:2472`; reversor, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. Outra operação seria recusada antes da guarda: somente `'situacao'` autoriza esse campo em `app/colaboradores/services/colaborador.py:29`, `app/colaboradores/services/colaborador.py:38`, e `_conferir` precede a guarda em `app/colaboradores/services/colaborador.py:93`.
- `salvar_inteiro` grava `situacao` qualquer que seja a operação; portanto os usos `'posto'` e `'cadastro'` entram no censo de corrida: `app/colaboradores/services/colaborador.py:112`.

| Chamador que escreve `situacao` | Atomic e trava anterior de Disputa/Chamado/Pergunta |
|---|---|
| `vincular_posto` → `salvar_inteiro(..., 'posto')` | Sem atomic local e sem trava prévia no caminho até a chamada: `app/colaboradores/views.py:310`, `app/colaboradores/views.py:319`, `app/colaboradores/views.py:329`. |
| `editar_colaborador` → `salvar_inteiro(..., 'cadastro')` | Atomic começa imediatamente antes; nenhuma trava prévia nesse bloco: `app/colaboradores/views.py:1343`, `app/colaboradores/views.py:1345`. A view começa com leitura simples: `app/colaboradores/views.py:1243`. |
| `transferir_posto` → `salvar_inteiro(..., 'posto')` | Atomic precede apenas atribuições e a chamada: `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1391`. O decorator não adquire trava nem atomic: `app/colaboradores/mixins.py:300`, `app/colaboradores/mixins.py:307`, `app/colaboradores/mixins.py:313`. |
| `executar_desligamento` efetivo → `salvar_inteiro(..., 'situacao')` | Atomic começa em `app/colaboradores/services/desligamento.py:128`; primeira gravação do bloco em `app/colaboradores/services/desligamento.py:133`. Cascata sobre disputas/chamados vem depois: `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:186`. |
| `lancar_ausencia` → `gravar(..., {'situacao': 'afastado'})` | Sem atomic externo local na view: `app/ponto/views.py:2399`, `app/ponto/views.py:2407`. `criar_ausencia` retorna antes da chamada: `app/ponto/views.py:2452`, `app/ponto/views.py:2472`; seu atomic de criação envolve `a.save()` e já terminou no retorno: `app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:512`. |
| Reversor → `gravar(..., {'situacao': SITUACAO_VOLTA})` | Sem atomic/trava anterior no comando: `app/ponto/management/commands/reverter_situacao_afastado.py:54`, `app/ponto/management/commands/reverter_situacao_afastado.py:61`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. `quem_ja_voltou` usa consulta simples: `app/ponto/services/afastado_avisa.py:126`. |

Não encontrei chamador de produção de `salvar_campos`/`atualizar_em_lote` que inclua `situacao`; os usos são foto/onboarding/cadastro/aparelho: `app/core/views.py:1065`, `app/core/views.py:1094`, `app/core/views.py:1140`, `app/colaboradores/views.py:1218`, `app/colaboradores/management/commands/importar_telefones_nexti.py:113`, `app/ponto/views.py:1964`, `app/colaboradores/fcm.py:61`, `app/colaboradores/management/commands/backfill_plataforma_fossil.py:73`, `app/colaboradores/management/commands/normalizar_identificadores.py:109`.

Logo, **não identifiquei inversão D/Ch/P → Colaborador nesses chamadores atuais** se a porta passasse a chamar `trava_na_ordem(colab.pk)` antes da guarda, mantendo guarda e save no mesmo atomic; a aquisição central efetivamente começa por Colaborador e segue Disputa → Chamado → Pergunta: `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`. A definição SaaS não habilita `ATOMIC_REQUESTS`: `app/config/settings/saas.py:83`; configuração efetivamente carregada em produção: `[nao verificado]`.

`executar_desligamento` **não chama trava explícita da linha de Colaborador antes de gravar desligado**: abre atomic, atribui e salva diretamente, `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`. Para novo valor `'desligado'`, a guarda retorna antes de consultar a linha: `app/colaboradores/services/colaborador.py:57`, `app/colaboradores/services/colaborador.py:58`. O save emite a escrita antes da cascata; a trava implícita do `UPDATE` não equivale à leitura prévia por `select_for_update` que o usuário propõe: `app/colaboradores/services/colaborador.py:119`, `app/chamados/models.py:2744`.

Os chamadores indiretos do executor também não adquirem trava antes: UI faz leitura simples e chama executor, com `gate_disputa(isento=True)` (`app/colaboradores/views.py:1425`, `app/colaboradores/views.py:1431`, `app/colaboradores/views.py:1437`; bypass em `app/colaboradores/mixins.py:301`); cron usa queryset comum e chama executor (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`).

Detalhe adicional: `Colaborador.save` pode chamar `trava_na_ordem` para gerar matrícula ausente, mas é trava de outro Colaborador da faixa, sem D/Ch/P pedidos, e acontece dentro do save, depois da guarda da porta: `app/colaboradores/models.py:427`, `app/colaboradores/models.py:436`, `app/colaboradores/services/colaborador.py:118`.

## 15/11 10:21 · sessao 8386020a0f5c · - · - · origem=Ronald (tmux)

1. A selfie continua chamando `Colaborador.save`, agora via `salvar_campos(..., ['foto'])`; portanto a conclusão automática continua: se `pk` existe e não há etapas pendentes, o model acrescenta `onboarding_pendente` e `onboarding_completo` ao `update_fields`. As etapas são `camera_concedida_em` e `gps_concedido_em`, não a existência da foto (`app/core/views.py:1063-1065`; `app/colaboradores/services/colaborador.py:102-108`; `app/colaboradores/models.py:406-426`).

2. O único receiver de `post_save` de **Colaborador** encontrado na cópia é `_isentos_post_save_colaborador`: ele ignora `update_fields`, retorna para `raw` ou não isento e agenda `silenciar_do_colab` em `on_commit`. Não possui dependência de save completo (`app/chamados/signals.py:270-281`; registro em `app/chamados/apps.py:8-10`).

3. Há diferença além de impedir regravação de `situacao`: `atualizado_em` é `auto_now=True` e não entra mais no save da selfie. Além disso, `save` continua normalizando `cpf` e gerando `matricula_hasner` quando faltante, mas esses campos não pertencem a `['foto']`, nem são acrescentados pelo override. Logo deixam de persistir como efeito colateral desse pedido; a atribuição de onboarding foi explicitamente preservada (`app/colaboradores/models.py:381`; `app/colaboradores/models.py:415-439`; `app/colaboradores/services/colaborador.py:108`). Efeito real dessas diferenças sobre usuários de produção: `[nao verificado]`.

4. Antes da gravação, a própria view apenas recupera `request.user.colaborador`, decodifica o POST e atribui a foto; não atribui contato, permissões ou outro campo de cadastro. As permissões são outra view, com seu próprio save parcial (`app/core/views.py:1048-1065`; `app/core/views.py:1073-1095`). Não há middleware de onboarding nas listas declaradas; o gate de plataforma libera UA ausente, compatível com o `Client()` sem UA do teste (`app/config/settings/base.py:53-66`; `app/config/settings/saas.py:67-74`; `app/core/middleware.py:272-274`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:234`).

5. O teste não passa vazio: exige resposta JSON com `ok=True`, lista `chamadas` não vazia, e examina **todas** as chamadas capturadas, rejeitando `update_fields=None` e presença de `situacao`. Redirect/HTML faz `r.json()` falhar; JSON de erro falha na primeira assert; ausência de save falha na segunda (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:250-255`). Erros da view são devolvidos com `ok=False`, inclusive erros de storage/save (`app/core/views.py:1067-1068`).

6. O teste isola upload usando diretório temporário e `override_settings(MEDIA_ROOT=media)`; o backend declarado em SaaS é `FileSystemStorage`. Falha ao criar diretório/storage não é convertida em verde (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:248-251`; `app/config/settings/saas.py:109-112`). A execução real sob esses settings: `[nao verificado]`.

7. Pelo caminho esperado de `FieldFile.save(save=True)`, o save completo seria `instance.save()` e o patch de classe interceptaria essa chamada: `original = Colaborador.save` é guardado antes, e `mock.patch.object(Colaborador, 'save', espia)` envolve o POST. Assim a versão anexa `save=True` produziria `None` em `kwargs.get('update_fields')`, rejeitado pela assert (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:237-254`; caminho antigo fornecido no anexo). **Limite:** a implementação do Django não está presente na cópia acessível; confirmação desse dispatch interno e execução contra 77dfa94fa: `[nao verificado]`.

8. O teste é adequado para detectar save completo, mas não demonstra literalmente “só a foto”: permite qualquer conjunto de campos que não contenha `situacao`, não confere PK das chamadas e não relê `foto` do banco. Também não exercita a conclusão das etapas, pois a fixture só estabelece `onboarding_completo=True`, deixando GPS/câmera sem timestamps (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:52-58`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239-255`; `app/colaboradores/models.py:375-376`).

## 15/11 10:19 · sessao aa799e8e7d78 · - · - · origem=Ronald (tmux)

Censo estático, somente leitura, sem banco e sem executar testes. A evidência abaixo identifica os acessos encontrados; exaustividade contra SQL dinâmico ou execução externa é [nao verificado]. [nao verificado]

**Escritores de Ausencia**

| Campo | Escritores de produção |
|---|---|
| `tipo`, `data` | Nascimento em `app/ponto/services/ausencia.py:437`/`:483`/`:516`; edição em `app/ponto/services/ausencia.py:602`/`:603`/`:613`. |
| `data_fim` | Nascimento e edição derivados por `derivar_data_fim`, `app/ponto/services/ausencia.py:152`, `:289`, `:439`, `:604`; o modelo completa fim ausente em `app/ponto/models.py:912`/`:917`; `mudar_estado` persiste também essa derivação, `app/ponto/services/ausencia.py:936`; rederivação pelo save completo, `app/ponto/services/ausencia.py:1659`. |
| `dias_corridos` | Entrada criação/edição em `app/ponto/services/ausencia.py:438`/`:605`; modelo deriva do intervalo inclusivo em `app/ponto/models.py:903`/`:909`/`:911`; transição persiste em `app/ponto/services/ausencia.py:936`; rederivação em `:1659`. |
| `minutos` | Entrada criação/edição em `app/ponto/services/ausencia.py:439`/`:612`; modelo soma previsto para falta aprovada sem minutos, `app/ponto/models.py:887`/`:895`/`:897`; transição persiste em `app/ponto/services/ausencia.py:936`; rederivação em `:1659`. |
| `status` | Criação, `app/ponto/services/ausencia.py:440`; transição, `:926`/`:938`; rejeição bulk de férias, `:1420`. |
| `aprovada_em` | Criação aprovada, `app/ponto/services/ausencia.py:443`/`:445`; modelo auto-carimba tanto aprovada como rejeitada, `app/ponto/models.py:882`; transição inclui persistência, `app/ponto/services/ausencia.py:936`; decisão aceita instante da borda, `:1026`/`:1027`/`:1037`; rederivação pode completar carimbo, `:1659`. |
| Delete | Espelho inativo e encurtado em `app/ponto/services/ausencia.py:1303` e `:1333`; apagar férias em `:1393`; ensaio em `:1647`; CASCADE da FK de agendamento declarado em `app/ponto/models.py:796`/`:797`. |

A busca explícita e o censo AST existente não encontraram escritor runtime de `Ausencia` fora da porta; o admin é somente leitura, `app/ponto/admin.py:37`/`:38`. O censo reconhece escrituras nomeadas e também declara limitações de inferência, `app/core/censo_escritas.py:8`, `:42`, `:47`. Não use isso como prova absoluta contra escrita dinâmica.

Exceções históricas: migration `0057_a4_backfill_pendente.py` escreve status diretamente em `app/ponto/migrations/0057_a4_backfill_pendente.py:43` e desfaz em `:63`; não invalida fechamento nesses sítios. O delete de ensaio também não invalida, mas exige banco de ensaio, `app/ponto/services/ausencia.py:1643`/`:1646`/`:1647`. O backfill de origem modifica apenas FK, `app/ponto/services/ausencia.py:1524`/`:1525`; não modifica os sete campos pedidos.

**Leitores diretos e leitores centrais**

| Arquivo e pontos | Campos e uso |
|---|---|
| `app/ponto/models.py:720`, `:729`, `:741`, `:852`, `:872`, `:876`, `:882`, `:887`, `:892`, `:903` | Estado aberto/aprovável, tipo/efeito, representação, minutos→horas, carimbo e derivação de intervalo/minutos. |
| `app/ponto/turnos.py:2516`, `:2531`, `:2554`, `:2600`, `:2677`, `:2715`, `:2753`, `:2772`, `:2786`, `:2827`, `:3076`, `:3138`, `:3158` | Autoridades de efeito, interseção, cobertura, parcial, afastamento, sobreposição e carregamento em lote: `status`, `tipo`, `data`, `data_fim`, `minutos`. |
| `app/ponto/precedencia.py:273`, `:337` | `status`, `tipo`, `data`, `data_fim`; filtro e expansão própria de candidatos à precedência. |
| `app/ponto/services/fechamento.py:638`, `:681`, `:694`, `:873` | Ausências aprovadas, cobertura, classificação parcial e rateio de falta; consome `status`, `tipo`, `data`, `data_fim`, `minutos` pelas autoridades. Não encontrei leitura direta de `dias_corridos` ou `aprovada_em` no cálculo mensal. |
| `app/folha/export.py:70`, `:87`, `:117`, `:118`, `:121`, `:140`, `:160` | `tipo`, `status`, datas e minutos; expansão inclusiva e rateio do total pelos dias inteiros do registro. |
| `app/ponto/services/efeito_ausencia.py:47`, `:48`, `:49`, `:50`, `:53`, `:55` | Efeito aprovado, tipo e parcial/minutos; crédito no dia inicial. |
| `app/ponto/services/credito_parcial.py:26`, `:27`, `:28` | Mesmo universo aprovado, tipo/minutos/data para crédito parcial por dia. |
| `app/ponto/services/dia_decidido.py:448`, `:481`, `:493`, `:508`, `:520`, `:550`, `:558`, `:587`, `:614`, `:619` | Estado/tipo/minutos, carimbo decisório, decisões por dia e histórico; lê também `dias_corridos` em `:621` e `:622`. |
| `app/ponto/services/cartorio.py:383`, `:385`, `:1356` | Interseção de datas e carimbo `aprovada_em` para fato novo; cobertura para lavra. |
| `app/ponto/signals.py:123`, `:124` | Tipo/datas para dias a relavrar. |
| `app/ponto/services/vigia_ausencia.py:59`, `:92`, `:104`, `:108`, `:115`, `:120`, `:135`, `:136`, `:215`, `:218` | Estado, tipo, datas e dias_corridos nos diagnósticos. |
| `app/ponto/services/vigia_ausencia_veredito.py:46`, `:53`, `:54` | Aprovação, cobertura, tipo e parcial. |
| `app/ponto/services/alertas_ausencia.py:48`, `:50` | Cobertura aprovada e tipos médicos. |
| `app/chamados/services/materializacao.py:79`, `:83`, `:626`, `:627`, `:630`, `:662`, `:669` | Cobertura, tipo, data e rejeição para absorver/devolver perguntas. |
| `app/chamados/services/silencio_ausencia.py:66`, `:78`, `:94` | Cobertura, estado aberto/aprovado, tipo e data do chamado. |
| `app/chamados/juizes.py:479`, `:1801`, `:2380` | Cobertura/status aprovado; data original por PK. |
| `app/chamados/services/historico.py:129`, `:132` | Tipo/data para evento histórico. |
| `app/chamados/services/foto_e4b.py:251`, `:285`, `:287`, `:288` | Fotografa campos concretos e lê tipo/datas para alcance; aberto fica alcance desconhecido. |
| `app/ferias/models.py:147` | Tipo literal falta, estado aprovado e data inicial para contar faltas do PA. |
| `app/ferias/services.py:147`, `:152`, `:155`, `:156` | Tipo férias, status e data para confrontar espelho com agendamento. |
| `app/relatorios/views.py:197`, `:205`, `:277`, `:292`, `:304` | Estado/tipo/datas para dias médicos e corte civil do relatório. |
| `app/inteligencia/views.py:71`, `:84`, `:232`, `:239`, `:516` | Data inicial e agregados de tipo; `sem_ausencia` usa relação por data inicial. |
| `app/inteligencia/resumo.py:39`, `:42`, `:43`, `:44` | Ordenação por data e contagem de registros aprovados por efeito/tipo. |
| `app/inteligencia/management/commands/calcular_scores.py:12` | Data inicial, status não rejeitado e tipo; contagem de registros. |
| `app/inteligencia/management/commands/gerar_alertas.py:55` | Mesmo padrão de contagem por início no mês civil. |
| `app/colaboradores/services/detalhe.py:104` | Data inicial no mês civil e ordenação. |
| `app/colaboradores/views.py:2927`, `:2928` | Tipo/data/data_fim para afastamento atual. |
| `app/api/views.py:1056`, `:1061`, `:1853`, `:1865`, `:1866`, `:1867`, `:1868`, `:1870`, `:3076`, `:3086` | Estado, tipo, data e minutos: botão de anexo, listagem e guarda de upload. |
| `app/api/views_mensageria.py:744`, `:745`, `:746`, `:748`, `:749`, `:763`, `:1034`, `:1035`, `:1073`, `:1075` | Estado/tipo/datas em contagens e itens do copiloto. |
| `app/ponto/views.py:1005`, `:2079`, `:2190`, `:2230`, `:2293`, `:2334`, `:2370`, `:2615`, `:2638`, `:2648`, `:2705`, `:2722`, `:2724`, `:2766`, `:2801`, `:2811`, `:2853`, `:2877`, `:2916` | Listagens, agrupamento, guardas dos atos, formulário/antes/depois e histórico médico; vários dos sete campos. |
| `app/ponto/views_tipos_ausencia.py:24` | Contagem por tipo gravado. |
| `app/ponto/admin.py:44`, `:45`, `:48` | Exibição/filtro de tipo, data, minutos e estado. |

**Demais consumidores pela autoridade de cobertura**, portanto leitores indiretos de status/tipo/datas, e do parcial/minutos quando indicado:

- Grade: `app/escala/utils.py:1174`, `:1175`; falta do dia em `app/escala/servico_jornada.py:347`, `:348`; cadastro/realidade em `app/escala/services/cadastro_realidade.py:603`; furos em `app/escala/services/furos_diarios.py:222`.
- Espelhos/calendário: `app/ponto/services/espelho.py:582`, `:588`; `app/colaboradores/services/calendario.py:484`, `:532`; API em `app/api/views.py:1566`.
- Cobrança e elegibilidade: `app/chamados/services/adesao.py:69`; `app/chamados/services/vigia_fio.py:308`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`; `app/colaboradores/services/situacao_exibida.py:63`; `app/api/views_mensageria.py:1246`.
- Afastamento/aviso: `app/ponto/services/afastado_avisa.py:39`, `:127`, `:129`; `app/ponto/services/aviso_da_batida.py:88`.
- Comandos: `app/ponto/management/commands/reconciliar_grade.py:401`; `processar_alertas_turno.py:97`; `processar_alertas_avancados.py:115`; `supra_juiz.py:216`; `vigia_b5_ausencia.py:68`; `emitir_furo_retroativo.py:99`; `emitir_turno_aberto_retroativo.py:108`; `backfill_b5_insumos.py:72`; `detectar_ausencias.py:819`; `rejulgar_ausencia_em_folga.py:33`; `ausencias_sem_efeito_na_celula.py:44`; `diff_calculador.py:408`.
- Auditoria de chamados: `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `:642`, `:659`, `:1469`.
- Curadores que leem campos diretamente: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:99`, `:121`, `:142`, `:147`, `:167`, `:169`; `curar_ausencia_sem_fim.py:30`, `:38`, `:49`, `:56`; `ausencia_bloqueante_sem_fim.py:27`, `:39`; `reabrir_chamados_ausencia_aberta.py:36`, `:43`; `fechar_chamados_ausencia.py:34`.

**Templates leitores diretos**

`app/templates/ponto/ausencias.html:150`, `:159`, `:163`, `:177`; `app/templates/ponto/partials/painel_ausencia.html:51`, `:123`, `:124`, `:125`, `:152`, `:155`, `:157`, `:159`, `:163`, `:275`, `:293`, `:310`, `:313`; `app/templates/ponto/espelho.html:191`; `app/templates/ponto/partials/painel_fechamento.html:55`, `:56`; `app/templates/ponto/historico_atestados.html:39`; `app/templates/chamados/fio_colaborador.html:62`; `app/templates/colaboradores/detalhe.html:74`; `app/templates/relatorios/afastados.html:40`. O instante de aprovação do drawer é convertido via `localtime`, `app/templates/ponto/partials/painel_ausencia.html:275`.

**Divergências comprovadas, herdadas salvo nota**

1. `data_fim=NULL` não significa universalmente aberto: help do campo diz aberto, `app/ponto/models.py:751`/`:753`; juiz só abre tipos `TIPOS_EM_ABERTO`, `app/ponto/turnos.py:2791`/`:2793`/`:2796`.
2. API ainda testa status inexistente `pendente`, `app/api/views.py:1061`; espelho idem, `app/templates/ponto/espelho.html:191`; estados reais em `app/ponto/models.py:701`–`:704`.
3. `decididos_do_periodo` reduz toda linha sem fim a um dia, inclusive tipo aberto, `app/ponto/services/dia_decidido.py:552`/`:558`/`:560`; cobertura abre até a borda fornecida, `app/ponto/turnos.py:2793`.
4. Histórico identifica aberto pelo número de dias None/0, `app/ponto/services/dia_decidido.py:622`; criar afastamento permite/defaulta `dias_corridos=1`, `app/ponto/services/ausencia.py:216`/`:438`. A listagem do copiloto chama qualquer fim nulo de “sem data de volta”, `app/api/views_mensageria.py:1077`.
5. Faltas no PA são quantidade de registros iniciados dentro dele, `app/ferias/models.py:147`–`:150`; folha expande e rateia o intervalo inclusivo, `app/folha/export.py:114`/`:118`/`:121`/`:140`. Range de três dias é um registro para o PA; início anterior ao PA fica fora desse contador.
6. `aprovada_em` é carimbo de decisão, inclusive rejeição no save, `app/ponto/models.py:882`; rejeição bulk de férias não move esse carimbo nem decisor, `app/ponto/services/ausencia.py:1420`. Novo diff acrescenta trilha, mas mantém esse contraste de semântica.
7. Modelo deriva minutos de falta **antes** de completar fim ausente, `app/ponto/models.py:887`/`:892`/`:897` versus `:912`/`:917`. Uma falta aprovada com `dias_corridos>1`, sem fim e minutos zero, rederivada nesse estado, soma só o primeiro dia antes de ganhar o range; defeito herdado do save.
8. A redução da invalidação para tipo de dia único sem fim está coerente com os insumos financeiros encontrados: juiz devolve um dia, `app/ponto/turnos.py:2796`, e rateio usa a contagem desse juiz, `app/folha/export.py:118`/`:121`. Não achei dependência financeira de competências entre esse dia e hoje; prova dinâmica [nao verificado].

## 15/11 10:19 · sessao 8b0fcd9b754a · - · - · origem=Ronald (tmux)

**Achado principal: o novo evento de desfazer herda uma competência civil que pode ser diferente da competência do turno.** `desfazer` passa `batida.timestamp`; a porta converte o instante em data local e recalcula apenas uma competência. O fechamento, porém, atribui o turno ao dia da entrada e busca a saída no dia seguinte. Assim, uma S em 21/10 de turno iniciado em 20/10 pode acordar novembro e conservar outubro com a foto anterior; ocorrência real no dado fornecido: [nao verificado]. (`app/ponto/services/retratar_humano.py:212`; `app/ponto/services/fechamento.py:1567`; `app/ponto/services/fechamento.py:1595`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:624`)

**FechamentoMensal.derivado_em**

- Escritores: derivação de registro aberto, `timezone.now()` e `save`; derivação de registro aprovado, `update`; restauração de snapshot, NULL por padrão mas aceita carimbo explícito; invalidação, NULL e atualização de `atualizado_em`. (`app/ponto/services/fechamento.py:1143`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1189`; `app/ponto/services/fechamento.py:1391`; `app/ponto/services/fechamento.py:1653`)
- Leitores: universo de projeções vencidas, contador e comando de exposição; NULL significa nunca derivado ou invalidado, sem distinguir os dois motivos. O contador padrão olha somente a competência corrente por empresa. (`app/ponto/models.py:454`; `app/ponto/services/fechamento.py:1692`; `app/ponto/services/fechamento.py:1702`; `app/ponto/management/commands/projecoes_vencidas.py:49`)
- O hash numérico exclui `derivado_em` e `atualizado_em`; a mera invalidação não altera esse hash. (`app/ponto/portas/regen_exportada.py:35`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:71`)
- O novo desfazer pode, pelo evento, recalcular números de competência exportada e lavrar DiaPago. Não há emissão de TXT na porta do evento; o download do TXT existente entrega a cópia gravada. Mudança efetiva de hash/TXT/dinheiro em produção: [nao verificado]. (`app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1191`; `app/folha/views.py:549`)

**Justificativa.status**

- Semântica declarada: `pendente`, `aprovada`, `rejeitada`; `data_turno` é DateField do turno. A unicidade condicional usa `pendente`. (`app/ponto/models.py:130`; `app/ponto/models.py:144`; `app/ponto/models.py:159`; `app/ponto/models.py:189`)
- Escritores de nascimento pendente: API, formulário web, cron de alerta e regularização; os caminhos que reaproveitam pendente salvam tipo/descrição sem mudar status. (`app/api/views.py:1989`; `app/api/views.py:1999`; `app/ponto/views.py:1624`; `app/ponto/views.py:1638`; `app/ponto/management/commands/processar_alertas_turno.py:394`; `app/ponto/services/regularizacao.py:91`)
- Escritores de transição: aprovação DP, rejeição DP, materialização de saída, aprovação em lote, rejeição em lote, reconciliador pendente→rejeitada e regularização existente sem saída→pendente. Aprovação/materialização/lote aprovado já invalidam o fechamento; a regularização ganhou essa invalidação no diff. (`app/ponto/services/justificativa.py:205`; `app/ponto/services/justificativa.py:283`; `app/ponto/services/justificativa.py:383`; `app/ponto/views.py:1799`; `app/ponto/views.py:1878`; `app/chamados/reconciliador.py:532`; `app/ponto/services/regularizacao.py:105`; `app/ponto/services/justificativa.py:214`; `app/ponto/views.py:1807`)
- Leitores de cálculo: fechamento, efeito de ausência/espelho, grade e comando de comparação; todos filtram `aprovada` e usam `data_turno` inclusiva entre as pontas. (`app/ponto/services/fechamento.py:660`; `app/ponto/services/efeito_ausencia.py:40`; `app/escala/utils.py:1197`; `app/relatorios/management/commands/diff_extrato_parcial.py:42`)
- Leitores operacionais/apresentação: fila/histórico web, painel de fechamento, API, dashboard, atendimento e bloqueio de turno; pendente com saída informada significa regularização aguardando DP. (`app/ponto/views.py:1707`; `app/ponto/views.py:1739`; `app/ponto/views.py:1001`; `app/api/views.py:2095`; `app/core/views.py:205`; `app/core/views.py:244`; `app/core/views.py:1191`; `app/chamados/views.py:596`; `app/ponto/utils_turno_orfao.py:53`; `app/templates/ponto/partials/painel_justificativa.html:47`; `app/templates/chamados/meu_atendimento.html:89`)
- Todo save de Justificativa já acorda o cartório por signal, mas esse signal não invalida FechamentoMensal: são projeções distintas. (`app/ponto/signals.py:161`; `app/ponto/signals.py:165`)
- A regularização grava e invalida dentro do mesmo atomic; `invalidar_fechamento` usa savepoints próprios, engole falhas e não impede commit do ato se a invalidação falhar. (`app/ponto/services/regularizacao.py:89`; `app/ponto/services/regularizacao.py:107`; `app/ponto/services/regularizacao.py:113`; `app/ponto/services/fechamento.py:1648`; `app/ponto/services/fechamento.py:1661`)

**Batida.retratada_em**

- É instante nullable: preenchido exclui a batida da geometria/apuração; NULL a inclui independentemente de aprovação DP. Retratar grava `timezone.now()` por `anotar`; desfazer grava NULL pela mesma porta. (`app/ponto/models.py:65`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:276`; `app/ponto/services/retratar_humano.py:207`)
- A porta física aceita `retratada_em` entre campos anotáveis; aplica save e trilha. O diff não introduz escrita direta fora dela. (`app/ponto/registro_batida.py:205`; `app/ponto/registro_batida.py:237`; `app/ponto/registro_batida.py:254`)
- Leitores principais de apuração/geometria: `turnos`, motor, cartório, pareamento/reconciliador, materialização/prova de chamado, adesão, calendário, dashboards, escala e férias. (`app/ponto/turnos.py:1602`; `app/ponto/turnos.py:1614`; `app/ponto/turnos.py:1864`; `app/ponto/turnos.py:1902`; `app/ponto/turnos.py:2221`; `app/ponto/turnos.py:2353`; `app/ponto/turnos.py:2384`; `app/ponto/turnos.py:2469`; `app/ponto/turnos.py:2876`; `app/ponto/turnos.py:2915`; `app/ponto/motor_calculo_v2.py:671`; `app/ponto/services/cartorio.py:1204`; `app/chamados/reconciliador.py:217`; `app/chamados/juizes.py:758`; `app/chamados/juizes.py:817`; `app/chamados/services/materializacao.py:640`; `app/chamados/services/adesao.py:61`; `app/colaboradores/services/calendario.py:240`; `app/colaboradores/services/calendario.py:621`; `app/core/services/painel_op.py:93`; `app/escala/services/propositor.py:478`; `app/escala/services/meio_periodo.py:160`; `app/ferias/situacao.py:279`)
- Outros leitores runtime explícitos: deduplicação/registro, API, última batida/ficha/pós-vínculo/senha provisória, fila por causa, desvio de escala, limbo de folgas, passivo, adesão, piso de intervalo, flip, lastro, faltas de hoje e deslogados. (`app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:141`; `app/api/views.py:676`; `app/api/views_core.py:667`; `app/colaboradores/services/ficha.py:145`; `app/colaboradores/services/pos_vinculo.py:39`; `app/colaboradores/services/senha_provisoria.py:25`; `app/chamados/services/fila_por_causa.py:191`; `app/chamados/services/desvio_escala.py:60`; `app/chamados/services/limbo_folgas.py:38`; `app/chamados/services/limbo_folgas.py:157`; `app/ponto/services/passivo_s84.py:54`; `app/ponto/services/adesao.py:7`; `app/ponto/services/adesao.py:35`; `app/ponto/services/adesao.py:58`; `app/ponto/services/piso_intervalo.py:41`; `app/ponto/services/flip_batida.py:15`; `app/ponto/services/flip_auto.py:229`; `app/ponto/services/flip_auto.py:287`; `app/ponto/services/lastro.py:262`; `app/ponto/services/faltas_de_hoje.py:55`; `app/ponto/services/deslogados.py:161`; `app/ponto/services/deslogados.py:174`)
- Leitores do instante, em vez de mera presença: prazo humano de sete dias e frescor de fatos/vereditos. Não há divergência de unidade: ambos usam datetime, mas respondem perguntas diferentes. (`app/ponto/services/retratar_humano.py:48`; `app/ponto/services/retratar_humano.py:53`; `app/ponto/services/retratar_humano.py:75`; `app/ponto/services/retratar_humano.py:109`; `app/ponto/services/cartorio.py:381`; `app/chamados/veredito_lavrado.py:180`)
- Comandos também leem esse campo, incluindo correção retroativa, passivo, censos e migração de datas. Estes são código operacional da árvore, não evidência de execução em produção. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:95`; `app/ponto/management/commands/passivo_disputa_retro.py:161`; `app/ponto/management/commands/diff_passivo_s84.py:80`; `app/ponto/management/commands/vigia_plantio_overnight.py:73`; `app/ponto/management/commands/diff_calculador.py:271`; `app/ponto/management/commands/diff_janela_he.py:94`; `app/ponto/management/commands/e6_piso_autopsia.py:72`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:73`)
- O evento novo agenda cálculo por `transaction.on_commit`; em autocommit o callback executa na chamada, e dentro de atomic aguarda o commit. A própria porta protege import/chamada e o agendador protege falhas de cálculo. (`app/ponto/registro_batida.py:177`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1818`; `app/ponto/services/fechamento.py:1823`)

**LogAuditoria ação/antes/depois**

- A persistência tem um escritor: `core.observ.evento`, com savepoint próprio; `registrar_log` e `evento_kw` apenas adaptam argumentos. O novo rejeitar reutiliza esse escritor, sem criar segunda persistência de log. (`app/core/observ.py:94`; `app/core/observ.py:124`; `app/core/observ.py:133`; `app/core/auditoria.py:22`; `app/core/observ.py:146`; `app/ponto/services/ausencia.py:1417`)
- `acao` é texto de até 64 caracteres, com choices não validados por create; antes/depois são JSON sem schema declarado. O ator implícito vem da requisição contextual ou do processo fora dela. (`app/core/models.py:296`; `app/core/models.py:300`; `app/core/observ.py:108`; `app/core/observ.py:116`)
- Leitores relacionados ao diff: desfazer exige que a última ação da batida seja `retratar_batida_admin`; tooltip/relatório de motivos leem `valor_depois`; flips leem antes/depois. (`app/ponto/services/retratar_humano.py:35`; `app/ponto/services/retratar_humano.py:51`; `app/ponto/services/retratar_humano.py:72`; `app/ponto/services/retratar_humano.py:93`; `app/ponto/services/retratar_humano.py:95`; `app/ponto/services/retratar_humano.py:228`; `app/ponto/services/retratar_humano.py:243`; `app/ponto/services/flip_batida.py:66`; `app/ponto/services/flip_batida.py:73`)
- Divergência histórica de formato já tratada: flip aceita payload string `E`/`S`, dict ou NULL. O novo rejeitar escreve dict de status; não achei leitor de payload de status de Ausencia além do teste novo. O curador apenas detecta qualquer trilha posterior. (`app/ponto/services/flip_batida.py:46`; `app/ponto/services/flip_batida.py:69`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:166`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- Leitores genéricos e de outros domínios: UI de auditoria, histórico de chamados, regeneração de escala, retenção/reconciliação, juízes e vigia de loops. Eles usam ações/payloads próprios, não o novo dict de rejeição de férias. (`app/core/views.py:705`; `app/chamados/services/historico.py:86`; `app/escala/services/regeneracao.py:58`; `app/escala/services/regeneracao.py:61`; `app/chamados/models.py:404`; `app/chamados/reconciliador.py:480`; `app/chamados/juizes.py:1300`; `app/chamados/services/vigia_loop.py:32`; `app/chamados/services/vigia_loop.py:116`)

Esta é análise estática; execução dos testes, estado real de banco, logs e alteração efetiva de dinheiro/hash/TXT permanecem [nao verificado]. [nao verificado]

## 15/11 10:18 · sessao ebfe9f78f481 · - · - · origem=Ronald (tmux)

A função pura real é:

```python
def dono_da_divergencia(
    horas_do_dia,
    em_cadastro_x_realidade,
    folga_nao_definida=False,
    sem_vinculo_no_dia=False,
    sem_vinculo_em_dia_de_trabalho=False,
    batida_fora_de_turno=False,
    fase_deslocada=False,
):
```

Ela recebe insumos, **não recebe colaborador/data e não verifica se existe divergência**. Retorna sempre `CADASTRO`, `BATIDA` ou `ESTRUTURA`; por exemplo, horas vazias e demais insumos falsos retornam `ESTRUTURA`. `app/ponto/management/commands/e6_oraculo.py:53`, `:98`, `:110`.

A montagem por colaborador e competência é:

```python
InsumosDoDono(
    colaborador, ini, fim, hoje, espelho=None, *,
    agora, dias_de_fase=None, cadastro=None,
    sem_motor=False, cxr=None,
)
```

Seu método `.dono(data)` responde por aquele colaborador/data chamando o juiz puro com batidas, realizado, fase e cadastro reais. Também não verifica existência de divergência. `app/ponto/management/commands/e6_oraculo.py:128`, `:182`.

`sem_motor=True` com CXR sem cobertura pode devolver **`None = sem resposta`**, nunca `sem_divergencia`. A lavra distingue conjunto vazio conhecido de ausência de cobertura. `app/ponto/management/commands/e6_oraculo.py:142`, `:189`; `app/ponto/services/cxr_na_lista.py:165`, `:182`.

No comando E6, a existência de divergência é verificada **antes** de chamar `.dono(data)`: falta de linha, isenção/ausência e falta de lavratura ficam fora da comparação; zero/zero, diferença explicada pela janela declarada e diferença de até dez minutos não chegam ao dono. Só diferenças restantes recebem dono. `app/ponto/management/commands/e6_oraculo.py:334`, `:339`, `:342`, `:351`, `:354`, `:365`, `:378`.

Logo, não há resposta especial do juiz “quando não há divergência”: ele não faz essa pergunta. Inventar `None -> sem_divergencia` produziria classificação falsa. `app/ponto/management/commands/e6_oraculo.py:98`, `:110`, `:193`.

Para P3, consultar o dono real pode usar `InsumosDoDono(c, ini, fim, hoje, agora=agora).dono(data)`; quando precisa da lista do motor, a própria montagem chama `espelho_do_colab(..., agora=self.agora)`. É o mesmo caminho da medição E6 e das pautas. `app/ponto/management/commands/e6_oraculo.py:162`, `:195`; `app/ponto/management/commands/pautas_discordantes.py:82`, `:109`.

O comando de medição está explicitamente autorizado a chamar o motor; a proibição documentada recai sobre leitores de tela. `app/ponto/management/commands/pautas_discordantes.py:9`.

Não há função pronta observada que produza `sem_divergencia` nesse caminho. Caso o script precise imprimir essa categoria, deve aplicar os gates reais do comando E6 e deixar os dias sem julgamento identificados como erros, sem chamar ausência de julgamento de concordância. `app/ponto/management/commands/e6_oraculo.py:334`, `:346`, `:365`.

## 15/11 10:18 · sessao da5e5106c4ab · - · - · origem=Ronald (tmux)

P4: **“sem obra” para todas as classes, no sentido de zeragem demonstrável pelo dado fornecido**; E4B tem atos candidatos, mas o próprio comando escolhe a cura pela tarefa real e exige recenso para declarar `resolvido`. `app/chamados/management/commands/backfill_fio_morto_e4b.py:12`, `:82`, `:368`.

| Classe e quantidade do anexo [nao verificado] | Atribuição sustentada |
|---|---|
| c — 11 | **sem obra**. O classificador exige pergunta respondida sob terminal; a reabertura E4B retorna `None` para respondida. `app/chamados/services/vigia_fio.py:694`; `app/chamados/services/disputa_emissao.py:1842`. |
| a/em_analise — 10 | **sem obra**. Estado vivo explica o véu, mas não identifica o ato que falta nem qual obra o produz. `app/chamados/services/vigia_fio.py:683`; `app/chamados/services/vigia_fio.py:689`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`. |
| a/aberto — 7 | **sem obra**, pela mesma insuficiência: classificação por estado não determina ato corretor. Mesmas referências. |
| a/registrado — 2 | **sem obra**. `registrado` é VIVO, porém não COBRANCA; o relato reconhece “PRONTO nao transforma registrado em acao”. `app/chamados/catalogo/motor.py:24`; `:29`; `app/docs/RELATO.md:8823`. |
| e/terminal — 4 | **sem obra com zeragem demonstrada; candidata E4B**. Ato concreto: `_executar_dia` chama `reabrir_credor_de_pergunta_viva`; esta chama `credor.reabrir()`. Falta medir se os quatro passam todas as recusas. `app/chamados/management/commands/backfill_fio_morto_e4b.py:132`; `app/chamados/services/disputa_emissao.py:1872`. |
| e/sem_viva — 2 | **sem obra**. O rótulo só prova ausência de pergunta viva; E4B pode planejar perguntar, registrar decisão, materializar, rejulgar ou nenhuma ação. `app/chamados/services/vigia_fio.py:704`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`; `:114`. |
| ANTIGO/pergunta_morreu — 6 | **sem obra**. O antigo distingue apenas presença de `ps`; não informa o ato necessário. `app/chamados/services/vigia_fio.py:449`. Identidades/carga desses seis não fornecidas [nao verificado]. |
| ANTIGO/sem_pergunta — 1 | **sem obra**, mesma limitação. `app/chamados/services/vigia_fio.py:450`. Identidade/carga desse dia não fornecida [nao verificado]. |

Para os quatro e/terminal, o caminho E4B só reabre quando: pergunta sem resposta e viva, âncora existente e fora de VIVOS, carga sem limbo/exportada e `recusa_da_reabertura()` sem motivo. Recusas são reais, portanto o rótulo terminal não garante êxito. `app/chamados/services/disputa_emissao.py:1842`, `:1844`, `:1848`, `:1855`, `:1860`, `:1869`, `:1872`.

A GRADE não permite completar o vínculo ausente:

- E4c migra leitores e E4d para a projeção; esses atos não são, por si, reparação dos dias atuais. `app/docs/BACKLOG.md:1968`.
- E4B declara classes a–e, mas o programa não escolhe atos pelo rótulo e mede o resultado. `app/docs/BACKLOG.md:1977`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:12`, `:368`.
- E3-RESTO-R3 remete ao lote LU e aos achados descartados do pouso, sem vincular essas classes atuais. `app/docs/BACKLOG.md:768`.
- RAIZ trata o agregado raiz/filhos; ZUMBI trata carimbadores sem C7 e fechamento por beco. Nenhuma dessas linhas identifica os sete antigos ou garante sua zeragem. `app/docs/BACKLOG.md:1988`, `:2015`.

Os detalhes F1–F9 de `e4c_reconfere_v2.md` não foram encontrados na cópia; atribuir ato específico a uma fatia seria [nao verificado].

## 15/11 10:18 · sessao ae04dabb3731 · - · - · origem=Ronald (tmux)

**Achados de transação e auditoria**

- A trilha nova das férias não é obrigatória: `rejeitar_ausencias_de_ferias` ignora o retorno de `evento`; este retorna `None` quando falha, preservando a rejeição e permitindo trilha incompleta. O próprio domínio já tem `_trilha`, que exige o registro e desfaz a escrita sem ele. `app/ponto/services/ausencia.py:1418-1425`; `app/core/observ.py:94-139`; `app/ferias/services.py:460-468`.
- O cancelamento admin executa a nova rejeição/invalidação dentro de `atomic`, após travar agendamento e período. Cancelamento DP chega à nova invalidação pelo `save → _sync_ausencia → espelhar_ferias`; remarcação chega por apagar e depois salvar; exclusão do período chega pelo `delete` individual dos agendamentos. `app/ferias/services.py:636-639`; `app/ferias/services.py:651-657`; `app/ferias/services.py:674-689`; `app/ferias/services.py:706-731`; `app/ferias/services.py:870-877`; `app/ferias/models.py:444-454`.
- `invalidar_fechamento` protege suas consultas/escritas com savepoints e engole exceções; falhar ali mantém o ato, podendo deixar carimbo fresco ou invalidar apenas competências anteriores à falha. A nova `_acordar_projecao` calcula datas fora dessa proteção, portanto exceções nessa preparação propagam e podem desfazer o ato transacional; em autocommit, o `save` anterior já pode estar gravado. Ocorrência real: [nao verificado]. `app/ponto/services/fechamento.py:1640-1665`; `app/ponto/services/ausencia.py:133-149`; `app/ponto/services/ausencia.py:515-551`; `app/ponto/services/ausencia.py:938-947`.
- A regularização grava justificativa e invalida dentro do mesmo `atomic`; sua falha protegida não desfaz justificativa. Não há `select_for_update` nesse fluxo: busca da batida e julgamento de órfã acontecem antes do `atomic`. `app/ponto/services/regularizacao.py:50-89`; `app/ponto/services/regularizacao.py:102-114`; `app/ponto/services/fechamento.py:1648-1665`.
- A rejeição bulk fotografa linhas sem trava e depois repete o filtro `status != rejeitada` no UPDATE; em interleaving com outro escritor, pode registrar trilhas para linhas que esta chamada não alterou, com “antes” desatualizado. O chamador admin trava agendamento/PA, mas o helper não trava Ausencia; ocorrência em produção: [nao verificado]. `app/ponto/services/ausencia.py:1412-1423`; `app/ferias/services.py:636-639`.
- O cartório de Ausencia e Batida já roda sincronamente durante `save/delete`, portanto antes do commit quando existe transação; o consumidor de Ausencia é `on_commit`. O diff acrescenta invalidação após os sinais, não transfere esses efeitos anteriores para depois do commit. `app/ponto/signals.py:85-93`; `app/ponto/signals.py:131-152`; `app/ponto/signals.py:250-284`; `app/ponto/services/ausencia.py:938-943`.
- `desfazer` usa `anotar`, grava e audita, depois chama a porta de evento; o recálculo real fica em `on_commit`, com exceções engolidas. Esse recálculo pode reescrever campos calculados de fechamento exportado e lavrar DiaPago; portanto a chamada nova ultrapassa mero `derivado_em=NULL`, embora use escritor existente. `app/ponto/services/retratar_humano.py:207-213`; `app/ponto/registro_batida.py:237-265`; `app/ponto/registro_batida.py:177-185`; `app/ponto/services/fechamento.py:1790-1824`; `app/ponto/services/fechamento.py:1160-1201`.

**Testes: mutantes sobreviventes por caso** [nao verificado]

Todos os números abaixo são de `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py`.

| Caso | Mutante que não detecta |
|---|---|
| Esticar fim, 77–89 | Invalidar somente o range novo: este contém o anterior; encolhimento deixando competência abandonada não é exercitado. |
| Idempotência, 91–98 | Comparar somente `data_fim` em `_derivados`; mudanças exclusivas de minutos/status/aprovada_em deixam carimbo fresco. `app/ponto/services/ausencia.py:1656-1664`. |
| Criar falta, 103–109 | Invalidar somente criação já aprovada; criação aberta fica sem invalidação. |
| Retornar existente, 111–119 | Não invalidar retorno que reabre rejeitada; a fixture retorna a mesma aprovada. |
| Aberta sem fim, 121–127 | Invalidar somente competências das duas pontas, perdendo intermediárias: fixture atravessa apenas competências 10 e 11. |
| Reverter rejeição, 132–140 | Em `mudar_estado`, invalidar somente quando estado anterior é rejeitada; outras transições ficam descobertas. |
| Rejeitar férias, 161–177 | Gerar cinco trilhas para o mesmo `objeto_id`, todas com status esperado; só exige quantidade e conjuntos de status. Também não exige ator. |
| Apagar férias, 179–184 | Ignorar `exceto`; o teste não fornece esse argumento. | [nao verificado]
| Espelhar cancelado, 186–194 | Invalidar somente ramo inativo; remoção de dias fora da nova janela ativa fica descoberta. `app/ponto/services/ausencia.py:1328-1335`. |
| Regularização, 199–212 | Invalidar competência da saída, em vez do início: 04/11 e 05/11 pertencem à mesma competência da fixture. |
| Desfazer, 217–241 | Chamar evento sem restaurar `aprovada/retratada_em`; não há assert desses campos após desfazer. Também não testa commit/rollback: mocka a entrada `recalcular_por_evento`. | [nao verificado]

Não identifiquei caso principal vazio por leitura: férias exigem cinco linhas, regularização exige `pendente`, recálculo de retratação exige uma chamada antes do desfazer. Entretanto, regularização não cria ausências intermediárias: com início ontem, o primeiro dia do laço já é hoje e interrompe. `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:155`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:211`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:235`; `app/ponto/services/regularizacao.py:119-122`.

A base não cria escala/célula; os casos de falta não testemunham dinheiro, marcos ou turno noturno definido por escala. Nenhum novo teste verifica valor financeiro, TXT, exportada, rollback, concorrência ou falha de trilha. `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:44-52`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:77-241`. Execução da suíte: [nao verificado].

## 15/11 10:18 · sessao 24df9e7abeeb · SITUACAO-DESLIGADO-SEM-PORTA · verificador · origem=Code

**Não considero a cura fechada:** permanece um save completo fora da porta, na selfie, e a guarda permite uma intercalação entre consulta e gravação. Ambos podem regravar situação antiga sem `executar_reativacao` (`app/core/views.py:1061`; `app/colaboradores/models.py:439`; `app/colaboradores/services/colaborador.py:118`).

A análise abaixo é estática. Execução dos testes na base, ocorrência dos cenários em produção e valores financeiros efetivamente alterados: **[nao verificado]**. [nao verificado]

**1. Escritores de `Colaborador.situacao` e passagem pela guarda**

| Escritor de produção encontrado | Depois do diff |
|---|---|
| Lançamento INSS: grava `afastado` (`app/ponto/views.py:2472`). | **Passa**, por `gravar` → `_guarda_da_linha` (`app/colaboradores/services/colaborador.py:94`). |
| Reversor de afastamento: grava `ativo` (`app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/ponto/services/afastado_avisa.py:101`). | **Passa** pela mesma guarda; o universo inicial contém apenas `afastado` (`app/ponto/services/afastado_avisa.py:126`). |
| Desligamento efetivo: atribui `desligado` e salva inteiro (`app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/desligamento.py:133`). | **Chama a guarda**, mas ela retorna antes de consultar a linha porque o valor proposto já é `desligado` (`app/colaboradores/services/colaborador.py:57`). |
| Reativação: atribui `ativo`, limpa demissão e usa `salvar_reativacao` (`app/colaboradores/services/desligamento.py:242`; `app/colaboradores/services/desligamento.py:247`). | **Não passa**, deliberadamente; essa porta faz save completo direto (`app/colaboradores/services/colaborador.py:136`). |
| Vinculação de posto: save completo transporta também a situação do objeto (`app/colaboradores/views.py:329`). | **Passa**, por `salvar_inteiro` (`app/colaboradores/services/colaborador.py:118`). |
| Edição cadastral: save completo (`app/colaboradores/views.py:1345`). | **Passa**, pelo mesmo ponto (`app/colaboradores/services/colaborador.py:118`). |
| Transferência de posto: save completo (`app/colaboradores/views.py:1391`). | **Passa**, pelo mesmo ponto (`app/colaboradores/services/colaborador.py:118`). |
| **Selfie do onboarding:** `colab.foto.save(..., save=True)` implica save completo do Colaborador (`app/core/views.py:1048`; `app/core/views.py:1061`). | **Não passa pela porta.** O `save()` do modelo termina em `super().save()` sem essa guarda (`app/colaboradores/models.py:415`; `app/colaboradores/models.py:439`). |
| Admissão: cria pessoa com `ativo` (`app/colaboradores/views.py:1185`; `app/colaboradores/views.py:1208`). | **Não passa**: criação direta na porta; não é transição de uma linha desligada existente (`app/colaboradores/services/colaborador.py:144`). |
| Comando `smoke_chamados`: cria Colaborador pelo serviço, usando situação padrão (`app/chamados/management/commands/smoke_chamados.py:78`). | **Não passa**: mesma criação; padrão `ativo` no modelo (`app/colaboradores/models.py:323`; `app/colaboradores/services/colaborador.py:146`). |

As demais modalidades ficam assim:

- `salvar_campos` protege quando `situacao` está nos nomes; `atualizar_em_lote` protege quando está nos kwargs (`app/colaboradores/services/colaborador.py:106`; `app/colaboradores/services/colaborador.py:131`). Não localizei chamador produtivo atual dessas duas modalidades escrevendo situação: **[nao verificado quanto à inexistência absoluta]**.
- **`save()` direto continua capaz de tirar de desligado**, porque a guarda não está no modelo. A selfie é o sítio concreto que conserva esse risco com objeto antigo (`app/core/views.py:1061`; `app/colaboradores/models.py:439`).
- **`objects...update(situacao=...)` e `bulk_update(..., ['situacao'])` continuam sem proteção própria**: o diff protege wrappers, não instala manager/queryset protegido no modelo (`app/colaboradores/services/colaborador.py:123`; `app/colaboradores/models.py:283`). Sítio produtivo concreto dessas formas sobre situação, ou raw SQL correspondente: **não localizado [nao verificado quanto à inexistência absoluta]**.
- Há ainda um escape na própria assinatura: `gravar` valida `campos + tocar`, mas só guarda se situação estiver em `campos`. Portanto `tocar=['situacao']` pode persistir uma situação previamente atribuída sem guarda; chamador atual desse formato: **[nao verificado]** (`app/colaboradores/services/colaborador.py:92`; `app/colaboradores/services/colaborador.py:94`; `app/colaboradores/services/colaborador.py:98`).
- `salvar_reativacao` pode ser chamada diretamente: não verifica procedência, valor proposto ou trilha. O chamador encontrado usa o executor correto, mas a exclusividade é convenção de chamada (`app/colaboradores/services/colaborador.py:136`; `app/colaboradores/services/desligamento.py:247`).
- Admin está fechado por `SoLeitura`; signals examinados gravam somente onboarding, sem transportar situação (`app/colaboradores/admin.py:48`; `app/core/admin.py:59`; `app/colaboradores/signals.py:102`; `app/colaboradores/signals.py:119`). A migração de isenção também salva apenas `isento_ponto` (`app/colaboradores/migrations/0043_isento_ponto.py:14`).

**Sim, há possibilidade de sair de desligado sem executar reativação.** Além dos escapes acima, existe a sequência concorrente: guarda lê ativo → outro ato desliga → save completo grava ativo. A consulta e a escrita estão separadas, sem bloqueio nessa porta (`app/colaboradores/services/colaborador.py:71`; `app/colaboradores/services/colaborador.py:118`; `app/colaboradores/services/colaborador.py:119`). Reprodução concorrente: **[nao verificado]**.

**2. Caminho legítimo que a guarda passa a recusar**

Não identifiquei esse caminho entre os examinados; a reativação explícita foi migrada à porta separada (`app/colaboradores/views.py:1417`; `app/colaboradores/services/desligamento.py:247`).

- Vínculo novo **já recusava desligado**. A “reativação” dentro de `vinculo.py` altera `EscalaColaborador.ativa`, não `Colaborador.situacao` (`app/colaboradores/services/vinculo.py:91`; `app/colaboradores/services/vinculo.py:276`; `app/colaboradores/services/vinculo.py:283`).
- Pós-vínculo reconcilia células, furos e adesão; cadastro futuro regenera células (`app/colaboradores/services/pos_vinculo.py:73`; `app/colaboradores/services/pos_vinculo.py:85`; `app/ponto/services/cadastro_futuro.py:44`).
- Importação de contatos salva telefone/email; admissão rejeita CPF existente e cria pessoa nova ativa (`app/colaboradores/management/commands/importar_telefones_nexti.py:113`; `app/colaboradores/views.py:1133`; `app/colaboradores/views.py:1208`).
- Agendamento/revisão pelo fluxo de desligamento grava data e motivo, sem escrever situação; portanto essa escrita não aciona a guarda nova (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:110`).
- Porta específica de **cancelamento de desligamento agendado**: **não localizada [nao verificado]**. A limpeza explícita encontrada de `data_demissao` pertence à reativação (`app/colaboradores/services/desligamento.py:243`).

Isso não elimina a mudança funcional da tela de ausências descrita no item 5 (`app/ponto/views.py:2441`).

**3. `salvar_inteiro`: objeto novo, custo e erro de tela**

A premissa “consulta em TODO save completo” é excessiva: a função chama a guarda sempre, mas `pk=None` e proposta `situacao='desligado'` retornam antes da consulta (`app/colaboradores/services/colaborador.py:68`; `app/colaboradores/services/colaborador.py:57`; `app/colaboradores/services/colaborador.py:118`).

| Chamador encontrado | Efeito |
|---|---|
| Vincular posto: carrega objeto existente e salva (`app/colaboradores/views.py:319`; `app/colaboradores/views.py:329`). | Recusa é capturada e apresentada por toast/resposta de erro (`app/colaboradores/views.py:333`; `app/colaboradores/views.py:350`; `app/colaboradores/views.py:353`). |
| Editar cadastro: carrega existente e salva (`app/colaboradores/views.py:1243`; `app/colaboradores/views.py:1345`). | Se houver desligamento entre carga e guarda, o novo `ValueError` pode escapar como **500**, sem tratamento de formulário nesse fluxo (`app/colaboradores/views.py:1343`; `app/colaboradores/views.py:1355`). |
| Transferir posto: carrega existente e salva (`app/colaboradores/views.py:1374`; `app/colaboradores/views.py:1391`). | Mesma possibilidade de **500** por objeto antigo; o fluxo não captura essa recusa (`app/colaboradores/views.py:1387`; `app/colaboradores/views.py:1405`). |
| Efetivador: laço chama executor, que salva inteiro (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:43`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`). | **Sem SELECT adicional da guarda**, pois já propõe desligado (`app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/colaborador.py:57`). |

Não encontrei chamada de `salvar_inteiro` com objeto novo ou outro laço grande: **[nao verificado quanto à inexistência absoluta]**. Os 500 acima são possibilidades demonstradas pelo fluxo; ocorrência real: **[nao verificado]**.

**4. `esta_desligado(SimpleNamespace(situacao=novo))`**

**Funciona para o juiz atual.** Ele lê exclusivamente `getattr(colaborador, 'situacao', None) == 'desligado'`; não acessa demissão, nome ou qualquer outro atributo. Não ocorre `AttributeError` por ausência desses atributos (`app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/colaborador.py:57`).

**5. Universo novo de `lancar_ausencia`**

**Sim, muda além do desligado:** também recusa pessoa com situação gravada `ferias`. O queryset admite apenas `ativo`/`afastado`, e agora o POST precisa pertencer a ele (`app/ponto/views.py:2404`; `app/ponto/views.py:2441`).

Os choices atuais são **`ativo`, `afastado`, `ferias`, `desligado`** (`app/colaboradores/models.py:295`). Antes, pelo diff fornecido, o POST buscava qualquer PK; agora a recusa acontece antes de criar qualquer tipo de ausência, não apenas INSS (`app/ponto/views.py:2443`; `app/ponto/views.py:2452`). Logo, uma ausência válida para pessoa marcada `ferias`, antes alcançável por POST, passa a ser recusada nessa tela; ocorrência desse uso em produção: **[nao verificado]**.

**6. Alcance de `registrar_log`**

A distinção decisiva é **`request=None` versus objeto de request com `user=None`**. O primeiro já funcionava pelo curto-circuito; o segundo antes falhava no acesso a `.is_authenticated` e agora alcança `evento` (`app/core/auditoria.py:18`; `app/core/auditoria.py:24`).

- Chamadas com `None` em backfills/correções continuam no mesmo comportamento: município, escala retroativa e carimbo por batida (`app/colaboradores/management/commands/backfill_municipio_posto.py:96`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:198`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:213`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:101`).
- Comandos de credenciais também já passam `None` (`app/colaboradores/management/commands/diagnostico_login.py:30`; `app/colaboradores/management/commands/regerar_senhas_iniciais.py:70`; `app/core/credenciais.py:43`).
- **Efetivador passa a gravar o log final `Desligamento:`**, pois usa `_ReqSistema(user=None, META={})` (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:63`; `app/colaboradores/services/desligamento.py:229`). A trilha de credencial desse ato já passava `None`, portanto essa parte não é nova (`app/colaboradores/services/desligamento.py:136`).
- **`calcular_scores --motivo` também muda se não existir superusuário ativo**: a busca devolve `None`, mas ainda fabrica request e chama auditoria (`app/inteligencia/management/commands/calcular_scores.py:121`; `app/inteligencia/management/commands/calcular_scores.py:125`; `app/chamados/services/validacao.py:98`).
- Outros requests sintéticos estão em vínculo, regeneração exportada, regularização e validação; passam a registrar se chegarem com usuário nulo/sem atributo de autenticação. Ocorrência concreta dessa condição: **[nao verificado]** (`app/colaboradores/services/vinculo.py:19`; `app/ponto/portas/regen_exportada.py:41`; `app/ponto/services/regularizacao.py:146`; `app/chamados/services/validacao.py:94`).
- Request verdadeiro **sem `META` continua falhando**, antes de chegar à correção de usuário (`app/core/auditoria.py:18`; `app/core/auditoria.py:26`).

Há testes que contam logs: total zero da porta, retrato de idempotência dos turnos e trilha filtrada do vínculo (`app/colaboradores/tests/test_porta_colaborador.py:189`; `app/ponto/tests/test_selo_idempotencia_porta_turno.py:59`; `app/colaboradores/tests/test_trilha_do_vinculo.py:78`). A contagem do efetivador muda intencionalmente; quebra desses outros selos: **[nao verificado]**. O teste novo exige justamente o log antes ausente (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:200`).

**7. Classes de reprovação**

- **Segundo juiz:** a guarda nova não duplica o predicado registrado; consulta `esta_desligado` nos dois lados (`app/colaboradores/services/colaborador.py:56`; `app/colaboradores/services/colaborador.py:60`; `app/core/juizes.py:271`).
- **Segundo escritor:** permanece o save implícito da selfie; portanto a exclusividade da porta não está fechada (`app/core/views.py:1061`; `app/colaboradores/models.py:439`).
- **Relógio fora da borda:** o diff não acrescenta relógio à guarda. Permanecem leituras internas em `julgar_desligamento`, no executor e na reativação; o cron captura `hoje`, mas o juiz ainda lê seu próprio dia (`app/colaboradores/services/desligamento.py:40`; `app/colaboradores/services/desligamento.py:60`; `app/colaboradores/services/desligamento.py:155`; `app/colaboradores/services/desligamento.py:255`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`).
- **Derivação paralela:** a guarda usa o juiz, mas permanece a convivência entre situação gravada e situação exibida derivada do afastamento real (`app/colaboradores/services/situacao_exibida.py:56`; `app/colaboradores/services/situacao_exibida.py:63`; `app/ponto/views.py:2472`).
- **Dois significados:** `situacao` mistura estado cadastral com lâmpada de afastamento; `data_demissao` representa tanto intenção agendada quanto saída efetivada. Os leitores distinguem esses casos de maneiras diferentes: juiz de desligado lê só estado; efetivador combina data vencida e estado; rescisão lê somente data (`app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/folha/export.py:60`). Portanto não há uma interpretação uniforme de “desligado” em todos esses leitores.

**RED contra `77dfa94fa`, reconstruído estaticamente pelo diff:**

| Resultado esperado na base | Testes | [nao verificado]
|---|---|
| **Falham: sete.** Cinco testes de recusa, POST para desligado e trilha do efetivador. | `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:76`, `:83`, `:89`, `:96`, `:106`, `:173`, `:200`. |
| **Passam: quatro.** Reativação, transições do vivo, save mantendo desligado e INSS de ativo. São controles de preservação. | `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:115`, `:122`, `:133`, `:188`. |

Há mordida estática, mas não prova executada aqui: **[nao verificado]**. Além disso:

- Na base, o teste conjunto falha primeiro em `salvar_campos`; não chega à tentativa de lote (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:109`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:111`).
- O teste chamado “reativação ... idempotente” chama o ato **uma vez**. O executor registra evento por criação direta, portanto esse teste não certifica duas chamadas sem duplicata (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:117`; `app/colaboradores/services/historico_vinculo.py:25`).
- O teste de objeto velho desliga **antes** da guarda; não cobre desligamento entre consulta e save, nem o caminho da selfie (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:100`; `app/colaboradores/services/colaborador.py:118`; `app/core/views.py:1061`).

**8. LEI-AKITA, padrão e cortes**

`Policy Enforcement Point` **existe** literalmente no glossário. A consulta ao juiz seguida de recusa corresponde ao padrão, mas sua cobertura está incompleta pelos escapes do item 1 (`app/docs/GLOSSARIO.md:32`; `app/colaboradores/services/colaborador.py:56`; `app/core/views.py:1061`).

**`juizes novos=0` é compatível com as funções acrescentadas:** `DesligadoSemReadmissao` é uma classe de exceção; `salvar_reativacao` é uma porta de persistência. Nenhuma delas cria outro predicado para responder “está desligado?” (`app/colaboradores/services/colaborador.py:45`; `app/colaboradores/services/colaborador.py:136`; `app/core/juizes.py:271`). O texto real da linha LEI-AKITA do commit: **[nao verificado]**.

Não localizei `LEI-DESLIGADO-NAO-TRAVA`, `BUG 96`, reativação ou situação no `CORTES.md` da cópia; a frase literal alegada pelo código fica **[nao verificado]**. Os ponteiros que a alegam estão em `app/core/juizes.py:268` e `app/colaboradores/services/desligamento.py:26`. O ticket BUG 96 documenta futuro recusado/agendado, não demonstra sozinho a exclusividade de reativação (`app/docs/TICKETS.md:394`).

Os cortes encontrados sobre desligamento preservam resposta já dada para eventual rescisão e impedem reabrir disputa de desligado; não identifiquei contradição direta do diff com essas duas frases (`app/docs/CORTES.md:393`; `app/docs/CORTES.md:444`; `app/colaboradores/services/desligamento.py:165`).

**9. Dinheiro**

**Sim, pode mudar entradas e resultados da folha; mudança efetiva de número em produção: [nao verificado].** Não há alteração direta de fórmula nos sítios do diff, mas conservar `desligado` em vez de permitir `afastado` muda:

- o universo do recálculo, que condiciona a inclusão do desligado à data de demissão (`app/ponto/services/fechamento.py:439`);
- retenção versus informativo de rescisão (`app/folha/export.py:1206`; `app/folha/export.py:1225`);
- as ausências que chegam a ser criadas para pessoas fora da lista (`app/ponto/views.py:2441`; `app/ponto/views.py:2452`).

A classificação de saída do TXT pela rescisão continua dependendo de `data_demissao <= fim_janela`, independentemente de situação (`app/folha/export.py:54`). Assim, **“nenhum número muda” não está demonstrado** pelos dados fornecidos **[nao verificado]**.

## 15/11 10:14 · sessao 823b62bae5b0 · - · - · origem=Ronald (tmux)

1. **Escritores efetivos encontrados na cópia, fora dos testes** [nao verificado]

| Sítio | Escrita de `situacao` | Guarda depois do diff |
|---|---|---|
| `app/ponto/views.py:2472` | `gravar(..., {'situacao':'afastado'})`, no lançamento INSS | Sim, `app/colaboradores/services/colaborador.py:94` |
| `app/ponto/management/commands/reverter_situacao_afastado.py:73` | `gravar(..., situacao=SITUACAO_VOLTA)`; alvo vem de quem estava afastado | Sim; alvo em `app/ponto/services/afastado_avisa.py:126` |
| `app/colaboradores/services/desligamento.py:129` e `:133` | Atribui `'desligado'`; `salvar_inteiro` | Chama guarda, mas `'desligado'` retorna antes de consultar banco (`app/colaboradores/services/colaborador.py:57`) |
| `app/colaboradores/services/desligamento.py:242` e `:247` | Reativação: ativo, demissão limpa | **Não**: usa `salvar_reativacao`, cujo corpo salva diretamente (`app/colaboradores/services/colaborador.py:136`) |
| `app/colaboradores/views.py:329` | Save completo na vinculação de posto; leva a situação do objeto | Sim (`app/colaboradores/services/colaborador.py:118`) |
| `app/colaboradores/views.py:1345` | Save completo na edição cadastral; leva a situação do objeto | Sim (`app/colaboradores/services/colaborador.py:118`) |
| `app/colaboradores/views.py:1391` | Save completo na transferência; leva a situação do objeto | Sim (`app/colaboradores/services/colaborador.py:118`) |
| **`app/core/views.py:1061`** | **`colab.foto.save(..., save=True)` no onboarding de selfie: persistência completa implícita do Colaborador** | **Não passa pela porta.** O objeto vem de `request.user.colaborador` (`app/core/views.py:1048`); o modelo salva sem guarda (`app/colaboradores/models.py:415`–`:439`). Consequência de `FieldFile.save(save=True)` inferida pela API; reprodução de execução [nao verificado]. |
| `app/colaboradores/views.py:1185` e `:1208` | Criação com `'ativo'` | Não: `criar_colaborador` faz `objects.create` diretamente (`app/colaboradores/services/colaborador.py:144`) |
| `app/chamados/management/commands/smoke_chamados.py:78` | Criação de fixture por comando, situação default `'ativo'` | Não: mesma criação; default no modelo (`app/colaboradores/models.py:323`) |

**API da porta:** `salvar_campos` protege quando `situacao` aparece nos nomes (`app/colaboradores/services/colaborador.py:106`); `atualizar_em_lote` protege quando aparece nos kwargs (`:131`); não encontrei chamador produtivo atual dessas duas formas escrevendo `situacao`, além dos sítios enumerados. Universo/suposições do censo existente: `app/core/censo_escritas.py:6`, `:43`, `:54`.

**Bypasses:**

- `Colaborador.save()` direto não recebe a guarda; o override só normaliza CPF, onboarding e matrícula, chegando a `super().save` (`app/colaboradores/models.py:415`, `:439`). Logo `save()` direto pode remover desligado sem executar reativação; existe o sítio implícito da selfie acima (`app/core/views.py:1061`).
- `Colaborador.objects...update(situacao=...)` e `bulk_update(..., ['situacao'])` não foram protegidos no manager/modelo: a guarda está somente nos wrappers (`app/colaboradores/services/colaborador.py:90`, `:123`; definição do modelo sem manager customizado em `app/colaboradores/models.py:283`). Chamador concreto produtivo dessas formas sobre `situacao` não encontrado; ocorrência real [nao verificado].
- `salvar_reativacao` é pública e não exige prova do ato, não valida valor nem grava trilha por si; salva qualquer objeto recebido (`app/colaboradores/services/colaborador.py:136`). Seu único chamador produtivo encontrado é o executor (`app/colaboradores/services/desligamento.py:247`).
- Também existe corrida **SELECT → UPDATE**: a guarda lê sem lock e depois salva; desligamento concorrente após a leitura pode ser sobrescrito (`app/colaboradores/services/colaborador.py:59`, `:71`, `:98`, `:119`, `:133`). Reprodução concorrente [nao verificado].
- Admin não é escritor: `ColaboradorAdmin(SoLeitura, ...)` (`app/colaboradores/admin.py:48`) e o mixin recusa add/change/delete (`app/core/admin.py:59`).
- Signals de Colaborador encontrados só alteram onboarding, via escrita parcial (`app/colaboradores/signals.py:101`, `:118`); não transportam situação. Comando efetivador chama o executor existente (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`). Raw SQL produtivo de `situacao` não encontrado; existência fora da cópia [nao verificado].

2. **Não identifiquei caminho legítimo da cópia que precise ressuscitar desligado pelas quatro portas comuns.** Readmissão/reativação explícita foi migrada à exceção (`app/colaboradores/views.py:1417`, `app/colaboradores/services/desligamento.py:247`).

- Vínculo novo já recusa desligado antes deste diff (`app/colaboradores/services/vinculo.py:91`); “reativar existente” nesse serviço significa **EscalaColaborador**, não situação da pessoa (`app/colaboradores/services/vinculo.py:259`, `:276`, `:283`).
- Cadastro futuro escreve células por sua porta, não Colaborador (`app/ponto/services/cadastro_futuro.py:31`, `:44`).
- Pós-vínculo reavalia células, furos e adesão, não situação cadastral (`app/colaboradores/services/pos_vinculo.py:73`, `:85`, `:89`).
- Importação disponível de telefones salva apenas telefone/email, logo não ativa a guarda (`app/colaboradores/management/commands/importar_telefones_nexti.py:113`).
- Admissão cria pessoa nova ativa e rejeita CPF já existente (`app/colaboradores/views.py:1133`, `:1185`, `:1208`).
- Revisão do agendamento pelo fluxo de desligar ainda grava data/motivo sem escrever situação (`app/colaboradores/services/desligamento.py:107`–`:111`); portanto não aciona a nova guarda.
- Não encontrei porta específica de cancelamento de desligamento agendado; tal fluxo legítimo existente [nao verificado]. A única limpeza explícita de `data_demissao` encontrada é a reativação (`app/colaboradores/services/desligamento.py:243`). O chamado `desligamento_revisao` é criado após a efetivação, não uma rotina que desfaça situação (`app/colaboradores/services/desligamento.py:192`, `:208`).

3. **Chamadores de `salvar_inteiro`:**

- `vincular_posto`: objeto existente carregado em `app/colaboradores/views.py:319`, save em `:329`; captura `Exception` em `:333`, devolvendo toast/erro (`:350`, `:353`).
- `editar_colaborador`: objeto existente (`app/colaboradores/views.py:1243`), save em `:1345`, sem captura genérica nesse trecho; caso a guarda detecte desligamento concorrente, pode devolver **500** ao usuário em vez de erro de formulário (`:1343`–`:1355`). Execução concreta [nao verificado].
- `transferir_posto`: objeto existente (`app/colaboradores/views.py:1374`), save em `:1391`, sem captura no fluxo; mesma possibilidade de **500** por objeto stale (`:1387`–`:1405`). Execução concreta [nao verificado].
- Executor de desligamento: save em `app/colaboradores/services/desligamento.py:133`; alcançado em laço pelo cron (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:43`, `:57`). **Aqui não há SELECT extra da guarda**, porque o executor já atribuiu `'desligado'` (`app/colaboradores/services/desligamento.py:129`) e a guarda retorna antes de iterar o queryset (`app/colaboradores/services/colaborador.py:57`–`:59`).

Não encontrei `salvar_inteiro` com objeto novo na cópia; a função ainda pula a guarda para `pk=None` (`app/colaboradores/services/colaborador.py:68`). O rascunho novo da admissão não é salvo por ela (`app/colaboradores/views.py:1096`; `app/colaboradores/services/colaborador.py:149`). Portanto a premissa “leitura no banco em **TODO** save completo” é excessiva: ocorre a chamada da guarda, mas novo ou situação proposta desligado não executam aquela leitura (`app/colaboradores/services/colaborador.py:57`, `:68`, `:118`).

## 15/11 10:14 · sessao b5d8736cfd18 · - · - · origem=Ronald (tmux)

6. **O efeito é global, mas depende do tipo de request.**

- `request=None` já alcançava `evento` antes: o `and` curto-circuitava o acesso a `request.user`; agora continua assim. Chamadas explícitas: `app/colaboradores/management/commands/backfill_municipio_posto.py:96`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:198` e `:213`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:101`; definição atual em `app/core/auditoria.py:18`.
- Comandos de credencial também passam `None`: `app/colaboradores/management/commands/diagnostico_login.py:30`; `app/colaboradores/management/commands/regerar_senhas_iniciais.py:70`; chegam ao adaptador por `app/core/credenciais.py:40`. Portanto **não passam a gravar por causa deste diff**; já gravavam pelo caminho de ator nulo (`app/core/observ.py:94`, `:118`, `:134`).
- O efetivador usa objeto verdadeiro com `user=None`, `META={}`: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `:63`. Antes o acesso a `.is_authenticated` falhava; agora o log final `Desligamento:` alcança `evento`, com usuário nulo (`app/core/auditoria.py:24`; `app/colaboradores/services/desligamento.py:229`). **A trilha de credencial desse mesmo desligamento já usava `None`**, portanto não é nova (`app/colaboradores/services/desligamento.py:135`).
- Outro caso comprovável: `calcular_scores --motivo` busca o primeiro superusuário ativo e monta request mesmo se a busca devolver `None`; nessa condição passa a gravar `score_recalculado` (`app/inteligencia/management/commands/calcular_scores.py:121`; `app/chamados/services/validacao.py:94`).
- Outros requests sintéticos: `_ReqDoAutor` (`app/colaboradores/services/vinculo.py:19`), `_ReqDaPorta` (`app/ponto/portas/regen_exportada.py:41`), `_ReqShim` (`app/ponto/services/regularizacao.py:146`) e `_req_para_trilha` (`app/chamados/services/validacao.py:94`). O efeito novo existe se carregarem usuário nulo/sem `is_authenticated`; ocorrência concreta desses estados em produção, além dos caminhos acima, **[nao verificado]**. A porta da batida já fabrica um usuário com `is_authenticated=False`, portanto esse fallback já funcionava (`app/ponto/registro_batida.py:227`).
- A alteração não resolve request sintético verdadeiro sem `META`: o acesso continua direto e a exceção continua capturada (`app/core/auditoria.py:18`, `:26`).
- Há selos contando logs: total zero da porta do colaborador (`app/colaboradores/tests/test_porta_colaborador.py:189`); total no retrato de idempotência do cron de turnos (`app/ponto/tests/test_selo_idempotencia_porta_turno.py:59`, `:96`); lista filtrada na idempotência do calendário (`app/colaboradores/tests/test_trilha_do_vinculo.py:78`, `:141`). Isso exige revisão dos universos; quebra efetiva desses testes **[nao verificado]**. O novo teste exige a existência do log do efetivador anteriormente ausente (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:200`).

9. **Existe potencial de mudança nos resultados; valores efetivos em produção [nao verificado].**

- Conservar `desligado` em vez de permitir `afastado` muda o universo do recálculo: não desligados entram; desligados dependem de `data_demissao >= início da janela` (`app/ponto/services/fechamento.py:439`). O recálculo produz `FechamentoMensal` (`app/ponto/services/fechamento.py:448`).
- A mesma diferença muda retenção e informativo: desligado retorna motivos vazios; afastado segue ao cálculo dos dias (`app/folha/export.py:1206`). O informativo de rescisão faz o recorte oposto (`app/folha/export.py:1225`).
- O crivo de âncora exige `situacao='ativo'` (`app/folha/export.py:806`).
- A regra de saída do TXT pela rescisão permanece baseada em **data**, independentemente de situação: `data_demissao <= fim_janela` (`app/folha/export.py:54`, `:866`). Não há alteração direta dessa fórmula nos sítios anexados; afirmar “nenhum número muda” não é sustentado pelos leitores acima.
- O filtro novo também impede criar ausências que antes o POST podia criar para estados fora da lista (`app/ponto/views.py:2441`, `:2452`); impacto monetário concreto dessas ausências recusadas **[nao verificado]**.

## 15/11 10:13 · sessao de2eba907074 · - · - · origem=Ronald (tmux)

Itens 7–8, análise estática:

- **Juiz duplicado: não no diff.** `_recusar_volta_de_desligado` consulta `esta_desligado` tanto para o novo valor quanto para a linha gravada; o predicado permanece somente `getattr(..., 'situacao', None) == 'desligado'`. O juiz já está registrado. `app/colaboradores/services/colaborador.py:56`, `app/colaboradores/services/colaborador.py:60`, `app/colaboradores/services/desligamento.py:32`, `app/core/juizes.py:271`.
- **`DesligadoSemReadmissao` é exceção; `salvar_reativacao` é porta, não juiz.** A primeira apenas declara uma subclasse de `ValueError`; a segunda apenas confere a operação e executa `save()`. Contudo, esta porta pública não verifica que foi chamada por `executar_reativacao` nem produz a trilha por si; a disciplina depende do chamador. `app/colaboradores/services/colaborador.py:45`, `app/colaboradores/services/colaborador.py:136`, `app/colaboradores/services/desligamento.py:247`, `app/colaboradores/services/desligamento.py:251`.
- **Guarda não atômica:** a consulta da situação e o `save()` são operações separadas; a porta não usa bloqueio da linha. Uma intercalação “guarda lê ativo → outro escritor desliga → primeiro salva ativo” continua possível em termos do código. O RED do objeto velho cobre desligamento antes da guarda, não durante esse intervalo. `app/colaboradores/services/colaborador.py:71`, `app/colaboradores/services/colaborador.py:118`, `app/colaboradores/services/colaborador.py:119`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:100`.
- **Relógio:** o diff não adiciona leitura de relógio aos guardas. Há leituras preexistentes fora da borda: `julgar_desligamento` chama `date.today()` duas vezes; o executor chama `timezone.now()` e a reativação chama `localdate()`. O cron já captura `hoje`, mas não o fornece ao juiz. Isso contrasta com o desenho documentado de Clock Injection. `app/colaboradores/services/desligamento.py:40`, `app/colaboradores/services/desligamento.py:60`, `app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/desligamento.py:255`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/docs/PLANO-PADROES.md:92`.
- **Derivação paralela:** o guarda novo não replica o predicado de desligado; chama o juiz. O campo continua tendo a dívida anterior de situação gravada versus situação derivada: a apresentação consulta o juiz de afastamento, enquanto a tela grava `afastado`; o registro de juízes documenta explicitamente essa convivência e os 104 leitores ainda não migrados. `app/colaboradores/services/colaborador.py:56`, `app/colaboradores/services/situacao_exibida.py:56`, `app/colaboradores/services/situacao_exibida.py:63`, `app/ponto/views.py:2472`, `app/core/juizes.py:470`.

**RED contra a base, reconstruída pelo diff fornecido — execução real [nao verificado]:**

- **Sete testes deveriam falhar:** cinco testes da guarda, o POST para desligado e a trilha do cron. As operações antigas não levantavam a exceção esperada; o POST antigo buscava qualquer PK; `user=None` interrompia a auditoria antes do evento. `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:76`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:83`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:89`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:96`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:106`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:173`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:200`.
- **Quatro testes deveriam passar na base:** reativação, vivo transitando, desligado sendo salvo sem mudar situação, e afastamento INSS de ativo. São controles de preservação, não demonstração individual do defeito. `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:115`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:122`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:133`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:188`.
- **Mordida parcial:** no teste conjunto `salvar_campos_e_lote`, a primeira `assertRaises` já falha na base; a chamada de lote não chega a executar. O teste chamado “reativação ... idempotente” executa a reativação somente uma vez. Não há teste do atalho `salvar_reativacao` isolado nem da concorrência descrita acima. `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:109`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:111`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:117`.

**LEI-AKITA e cortes:**

- `padrao=Policy Enforcement Point` existe literalmente no glossário e descreve aplicação de decisão recebida de um juiz; essa parte corresponde ao guarda consultando `esta_desligado`. `juizes novos=0` também corresponde às novas funções do diff, que não criam outro predicado de desligado. `app/docs/GLOSSARIO.md:32`, `app/colaboradores/services/colaborador.py:56`, `app/core/juizes.py:271`.
- A **linha real LEI-AKITA do commit** não está no anexo nem há histórico Git nesta cópia: **[nao verificado]**. [nao verificado]
- Não localizei `LEI-DESLIGADO-NAO-TRAVA`, `BUG 96`, `reativacao` ou `situacao` em `app/docs/CORTES.md`; portanto não consigo confirmar a frase de corte alegada pelo próprio código: **[nao verificado]**. Os ponteiros que alegam sua existência estão em `app/core/juizes.py:268` e `app/colaboradores/services/desligamento.py:26`.
- Os cortes de desligamento que efetivamente aparecem dizem que resposta já dada segue viva, porque pode valer na rescisão, e que disputa de desligado não reabre. O diff fornecido não altera essas regras. `app/docs/CORTES.md:393`, `app/docs/CORTES.md:444`, `app/colaboradores/services/desligamento.py:165`.

## 15/11 10:09 · sessao ae03da037804 · HE-REVOGADA-NA-PORTA · verificador · origem=Code

A correção resolve os dois bloqueios identificados: `NAO` sobre recusa revogada deixa de ser `no_op`, e autorização em lote deixa de barrar essa linha. Há, porém, duas ressalvas materiais: os testes novos não comprovam relavratura efetiva, e “DIFF gravado” no serviço não significa dinheiro persistido conferido após o commit. (`app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:587`, `app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/services/autorizacao_he_periodo.py:94`)

O “antes” abaixo vem do patch fornecido. Execução Django na base e no commit, identidade Git da cópia e impacto quantitativo em produção: **[nao verificado]**. O dado fornecido não traz linhas `colNN` nem valores financeiros de produção: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e ESMERIL-SEMÂNTICA**

| Campo | Quem escreve em produção | Quem consome diretamente |
|---|---|---|
| `estado` | `_gravar`, criação e atualização. (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:75`) | Porta: antes/trilha, idempotência, retorno da ponta pequena, guarda da revogação e guarda do lote; modelo: `__str__`; motor; gestão; montador do retrato. (`app/ponto/portas/he.py:61`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:395`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:485`, `app/ponto/portas/he.py:587`, `app/ponto/models.py:1361`, `app/ponto/motor_calculo_v2.py:1417`, `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:107`, `app/ponto/services/he_pendente_lavrado.py:145`) |
| `origem` | `_gravar`, criação e atualização; recebe `admin` de `decidir_he` e `sistema` da ponta pequena. (`app/ponto/portas/he.py:68`, `app/ponto/portas/he.py:76`, `app/ponto/portas/he.py:208`, `app/ponto/portas/he.py:368`) | Antes/trilhas, proteção contra revogar decisão administrativa e mensagem da guarda do lote. (`app/ponto/portas/he.py:62`, `app/ponto/portas/he.py:396`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:591`) |
| `revogada_em` | Nulo na criação; `_gravar` limpa; `_marcar_revogada` preenche e persiste. (`app/ponto/models.py:1335`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:398`, `app/ponto/portas/he.py:400`) | Juiz `vigentes()`, antes da redecisão e trilha da revogação. O helper consulta o juiz. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:63`, `app/ponto/portas/he.py:409`, `app/ponto/portas/he.py:148`) |
| `revogada_motivo` | Vazio na criação; `_gravar` limpa; `_marcar_revogada` preenche. (`app/ponto/models.py:1340`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:399`) | Não encontrei consumo explícito do atributo armazenado na lógica do produto. A trilha recebe o argumento `motivo`. A ferramenta de sombra inventaria e processa esse campo, com banco explicitamente restrito à sombra. (`app/ponto/portas/he.py:409`, `bin/sombra_regras_pessoais.py:125`, `bin/sombra_mascara_pessoal.py:10`, `bin/sombra_mascara_pessoal.py:44`, `bin/sombra_selo_pessoal.py:74`) |
| `minutos_na_decisao` | `_gravar`, criação e atualização; a revogação preserva a foto. (`app/ponto/portas/he.py:67`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:400`) | Antes/trilhas, retornos idempotentes, retornos da revogação e agregação da gestão. (`app/ponto/portas/he.py:61`, `app/ponto/portas/he.py:203`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:395`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:477`, `app/ponto/portas/he.py:484`, `app/ponto/portas/he.py:490`, `app/ponto/portas/he.py:493`, `app/ponto/portas/he.py:494`, `app/ponto/services/gestao_he.py:108`) |
| `minutos_na_revogacao` | Nulo na criação; `_gravar` limpa; `_marcar_revogada` preenche. (`app/ponto/models.py:1343`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:399`) | Não encontrei consumo explícito do atributo armazenado na lógica do produto; a trilha recebe `int(minutos)`. (`app/ponto/portas/he.py:410`) |

As consultas que hidratam objetos completos também carregam os seis campos, mesmo sem consumir todos: quatro consultas cruas na porta e os mapas dos dois comandos. (`app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:580`, `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`)

Os demais leitores de produção recebem projeções desses valores:

- **Motor:** carrega decisões vigentes por `_DHE`; consulta autorização para dia sem previsto e para o tratamento das pontas de entrada/saída. Não usa a foto dos minutos para pagar. (`app/ponto/motor_calculo_v2.py:1417`, `app/ponto/motor_calculo_v2.py:1456`, `app/ponto/motor_calculo_v2.py:1653`, `app/ponto/motor_calculo_v2.py:1697`, `app/ponto/motor_calculo_v2.py:1739`)
- **Gestão:** `estado_por_dia` e `_decisoes` consultam vigentes; `enriquecer` publica estados, pendências e agregados; calendário, filtros, seleção de lote e contador da Central consomem essas projeções. (`app/ponto/services/gestao_he.py:87`, `app/ponto/services/gestao_he.py:105`, `app/ponto/services/gestao_he.py:141`, `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:278`, `app/ponto/services/gestao_he.py:281`, `app/ponto/services/gestao_he.py:312`, `app/ponto/services/gestao_he.py:332`, `app/ponto/services/gestao_he.py:405`, `app/ponto/services/gestao_he.py:475`)
- **Template:** espigas, badges, calendário, oferta de autorização e tabela de apoio leem estado/agregados publicados pela gestão. (`app/templates/ponto/gestao_he.html:109`, `app/templates/ponto/gestao_he.html:124`, `app/templates/ponto/gestao_he.html:125`, `app/templates/ponto/gestao_he.html:219`, `app/templates/ponto/gestao_he.html:220`, `app/templates/ponto/gestao_he.html:256`, `app/templates/ponto/gestao_he.html:258`, `app/templates/ponto/gestao_he.html:267`, `app/templates/ponto/gestao_he.html:273`, `app/templates/ponto/gestao_he.html:314`)
- **Folha:** `porta_export.medir` usa `estado_por_dia`; qualquer decisão vigente presente, inclusive `nao`, libera aquele dia da trava específica de HE. Ausência acima do limite entra na trava quando habilitada. (`app/folha/porta_export.py:230`, `app/folha/porta_export.py:232`, `app/folha/porta_export.py:682`, `app/folha/porta_export.py:690`)
- **Lavrador:** fornece mapa vigente ao montador, que pode revogar pela porta antes de ler `estado`. (`app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/services/he_pendente_lavrado.py:143`, `app/ponto/services/he_pendente_lavrado.py:145`)
- **Comando da ponta pequena:** consulta existência de vigentes, pula essas chaves e chama a porta para as restantes. (`app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:141`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`)
- **Serviço de período:** delega a escrita ao lote e lê o efeito pela autoridade do motor; não consulta diretamente `DecisaoHE.objects`. (`app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`)

**Semântica:** os campos conservam seus contratos: `estado` é a resposta, vigência é outra dimensão; `origem` identifica quem escreveu o estado atual; os dois campos de minutos são fotos de instantes diferentes. (`app/ponto/models.py:1267`, `app/ponto/models.py:1278`, `app/ponto/models.py:1283`, `app/ponto/models.py:1290`, `app/ponto/models.py:1345`)

Há uma ambiguidade preexistente na apresentação: todo `nao` aparece como “ciência”, inclusive recusa automática, porque a agregação não distingue origem. Além disso, `NAO` humano sobre **recusa vigente do sistema** continua `no_op`, preservando origem e minutos antigos. Portanto “com ciência” não comprova ciência humana. (`app/templates/ponto/gestao_he.html:125`, `app/templates/ponto/gestao_he.html:314`, `app/ponto/services/gestao_he.py:107`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:203`)

Ao reviver, os três campos da revogação são limpos; o histórico depende da trilha anterior, cuja gravação é best-effort. Esse comportamento já está no escritor. (`app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:406`, `app/ponto/portas/he.py:411`)

**2. Censo de consultas fora de `.vigentes()`**

Encontrei quatro consultas executáveis de leitura, todas na porta:

| Sítio | Com `revogada_em` preenchido |
|---|---|
| `decidir_he` | Trava a linha crua; `_vigente` devolve `None`; não aplica idempotência pelo estado antigo; atualiza a mesma linha e limpa a revogação. (`app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:83`) |
| `recusar_ponta_pequena` | Depois das guardas de minutos/limite, não considera já decidido; dry-run sinaliza recusa; apply revive a linha como recusa do sistema. (`app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:344`, `app/ponto/portas/he.py:357`, `app/ponto/portas/he.py:366`) |
| `revogar_recusa_do_sistema` | Origem administrativa retorna “decisão do admin”; origem sistema revogada retorna “já revogada”, sem escrita. (`app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:478`, `app/ponto/portas/he.py:483`) |
| `autorizar_em_lote` | Trava a linha crua; revogada não aciona a guarda de estado; segue para `decidir_he`. (`app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:587`, `app/ponto/portas/he.py:592`) |

`objects.create` é escrita. O `objects.filter` no texto `como_reverter` é instrução impressa, não consulta executada naquele comando. (`app/ponto/portas/he.py:66`, `app/ponto/management/commands/recusar_ponta_pequena.py:219`)

**3. Matriz antes → depois**

Pressupostos: autorização/permissão e motivo válidos; minutos positivos; autorização em lote acima do limite; ponta pequena dentro do limite; revogação acima do limite. As rejeições dessas entradas permanecem nas mesmas guardas. (`app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:178`, `app/ponto/portas/he.py:184`, `app/ponto/portas/he.py:190`, `app/ponto/portas/he.py:335`, `app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:491`, `app/ponto/portas/he.py:574`)

`=` significa mesmo resultado antes e depois; “troca” atualiza a linha existente. Referências por porta constam na última linha.

| Linha existente | `decidir NAO` | `decidir AUT` | `recusar_em_lote` | `autorizar_em_lote` | `recusar_ponta_pequena` | `revogar_recusa_do_sistema` |
|---|---|---|---|---|---|---|
| Sem linha | cria = | cria = | recusa = | autoriza = | cria sistema = | “sem decisão” = |
| Vigente admin `nao` | `no_op` = | troca = | `no_op` = | erro = | já decidido = | “decisão do admin” = |
| Vigente admin autorizado | troca = | `no_op` = | troca para `nao` = | `no_op` = | já decidido = | “decisão do admin” = |
| Vigente sistema `nao` | `no_op` = | troca = | `no_op` = | erro = | já decidido = | revoga = |
| Revogada sistema `nao` | **`no_op` → revive admin** | troca/revive = | **`no_op` → recusa contabilizada** | **erro → autoriza/revive** | revive sistema = | “já revogada” = |
| Fontes | `app/ponto/portas/he.py:195`, `:197`, `:206` | `app/ponto/portas/he.py:197`, `:206` | `app/ponto/portas/he.py:262`, `:267`, `:270` | `app/ponto/portas/he.py:580`, `:587`, `:592`, `:597` | `app/ponto/portas/he.py:343`, `:355`, `:366` | `app/ponto/portas/he.py:470`, `:475`, `:478`, `:497` |

Para linha **não revogada**, não identifiquei mudança de resultado de negócio: o juiz atual equivale precisamente à nulidade usada anteriormente nas portas do sistema. Há consulta adicional quando existe linha. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:148`)

A correção de `decidir_he` vale genericamente para qualquer linha revogada, independentemente da origem; o caminho normal de revogação, porém, só admite recusa do sistema. Existência de outras combinações em produção: **[nao verificado]**. (`app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:485`)

**4. Chamadores e prévia/confirmar**

- **View de um dia:** `NAO` sobre revogada muda de “já estava assim” para “Trocado”, com chamada ao recalculador. (`app/ponto/views.py:532`, `app/ponto/views.py:539`, `app/ponto/views.py:542`, `app/ponto/portas/he.py:227`)
- **View de recusa em lote:** esse dia passa de `no_op` para `recusados`. (`app/ponto/views.py:617`, `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:267`, `app/ponto/portas/he.py:270`)
- **Ato de período:** recusa revogada acima do limite passa de erro para autorização, tanto na prévia quanto no confirmar. (`app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:592`)
- **Comandos:** para o mesmo estado inicial, mantêm o resultado; seus mapas já usam vigentes. A substituição por `_vigente` acrescenta leitura, preservando o predicado anterior. (`app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`, `app/ponto/models.py:1267`)
- **Gestão:** já exibia revogada como `sem_decisao`; agora os gestos conseguem concluí-la. Após a escrita, estado e agregados vêm das decisões vigentes. A trava específica da folha também passa a reconhecer a decisão recém-gravada. (`app/ponto/services/gestao_he.py:278`, `app/ponto/services/gestao_he.py:311`, `app/folha/porta_export.py:232`, `app/folha/porta_export.py:686`)

**A igualdade entre os dois DIFFs continua protegida pelo código:** ambos executam `_ato`, medindo antes, autorizando e medindo depois; a prévia desfaz a transação; confirmar compara o dicionário exato e desfaz tudo se divergir. A revogada entra nesse mesmo caminho. (`app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`, `app/ponto/services/autorizacao_he_periodo.py:287`, `app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:343`, `app/ponto/services/autorizacao_he_periodo.py:346`)

A tela transporta exatamente `r['diff']`; cada medição cria autoridade/motor que consulta as decisões vigentes, sem reutilizar o mapa anterior à escrita. (`app/ponto/views.py:720`, `app/ponto/views.py:723`, `app/ponto/services/autorizacao_he_periodo.py:96`, `app/ponto/services/espelho.py:1111`, `app/ponto/motor_calculo_v2.py:1400`)

**Limite importante:** essa guarda compara o efeito no motor após gravar `DecisaoHE`. Não lê `DiaPago`/`FechamentoMensal` depois da relavratura. Esta roda por callback de commit e pode falhar sem desfazer a decisão; também pode ser barrada pela guarda de competência. Portanto, “DIFF mostrado == dinheiro efetivamente persistido” não está demonstrado por essa comparação. (`app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/fechamento.py:561`, `app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1824`)

**5. Checklist e selos**

- **Segundo juiz:** não nasce outro predicado; `_vigente` delega ao modelo. Não encontrei registro específico de HE em `core/juizes.py`; chamar esse juiz de “registrado” permanece **[nao verificado]**. (`app/ponto/portas/he.py:148`, `app/ponto/models.py:1267`, `app/core/juizes.py:7`)
- **Segundo escritor fora da porta:** não aparece no diff. Estado continua em `_gravar`; revogação em `_marcar_revogada`. (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:84`, `app/ponto/portas/he.py:400`)
- **Relógio fora da borda:** nenhuma leitura nova. `vigentes()` testa nulidade; o relógio existente continua no escritor da revogação. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:398`)
- **Derivação paralela:** o helper não calcula minutos nem reproduz o predicado de vigência. (`app/ponto/portas/he.py:146`, `app/ponto/portas/he.py:148`)
- **Ordem de trava:** os quatro usos vêm depois de `select_for_update`; as três portas individuais têm `atomic`, e o lote tem bloco por dia. O helper acrescenta `exists()` sobre o mesmo PK, sem travar outra entidade. Quando o lote chega a `decidir_he` com linha existente, há duas consultas adicionais de vigência. Não identifiquei inversão nova de ordem. (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:293`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:417`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:573`, `app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:592`)
- **Campo com dois significados:** não identifiquei novo significado nos seis campos; permanecem as ressalvas de apresentação de “ciência” e de “gravado” descritas acima. (`app/ponto/models.py:1306`, `app/ponto/models.py:1314`, `app/ponto/models.py:1337`, `app/ponto/models.py:1345`, `app/templates/ponto/gestao_he.html:314`, `app/ponto/services/autorizacao_he_periodo.py:94`)

Resultado **esperado por análise estática**, sem certificação de execução:

| Teste novo | Base do patch | Cópia atual | Fonte | [nao verificado]
|---|---|---|---|
| RED_1 | Falha: `no_op=True` | Passa: revive | `app/ponto/tests/test_he_revogada_na_porta.py:57`, `app/ponto/portas/he.py:197` |
| RED_1b | Falha: `(True, True)` | Passa: `(False, True)` | `app/ponto/tests/test_he_revogada_na_porta.py:78` |
| RED_1c | Falha: contabiliza `no_op` | Passa: contabiliza recusa | `app/ponto/tests/test_he_revogada_na_porta.py:89`, `app/ponto/portas/he.py:267` |
| RED_2 | Falha: guarda do lote barra | Passa: autoriza | `app/ponto/tests/test_he_revogada_na_porta.py:100`, `app/ponto/portas/he.py:587` |
| MORDE_3 | Já passa | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:113` |
| MORDE_4 | Já passa | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:125` |

Os dois MORDE preservam comportamento anterior; não precisam ficar vermelhos na base. MORDE_3 exige a mensagem específica da guarda, evitando passar simplesmente por falta de permissão. (`app/ponto/tests/test_he_revogada_na_porta.py:114`)

A alegação “relavrou o dia” excede a mordida dos testes novos: o recalculador está mockado; `call_count` comprova chamada. Não há caso novo de prévia/confirmar com revogada nem conferência financeira após commit. (`app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:97`)

Quanto aos selos existentes:

- **O273:** o helper respeita o juiz, limpeza e escritores existentes. O censo textual exclui integralmente `he.py`; portanto esse selo, sozinho, não fiscaliza a política interna da porta. (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:445`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:473`)
- **O214:** permanecem os escritores e os três chamadores de `acima_do_limite`; não identifiquei contradição estática introduzida. (`app/ponto/tests/test_o214_ponta_pequena.py:296`, `app/ponto/tests/test_o214_ponta_pequena.py:313`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`)
- **`test_tela_gestao_he*`:** não identifiquei expectativa contrariada pelo diff; resultado executado: **[nao verificado]**.
- **Atomicidade:** executei somente `python3 -B bin/atomicidade_da_porta.py app`, que lê AST. Retornou **rc=1, 35 travas, 25 cobertas, 10 violações**, nenhuma em `he.py`. O estágio de produção do shell reprova esse retorno. (`bin/atomicidade_da_porta.py:55`, `bin/atomicidade_da_porta.py:97`, `bin/atomicidade_da_porta.py:103`, `bin/tests/test_atomicidade_da_porta.sh:28`)
- As ocorrências foram em `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`, `app/ferias/services.py:485`, `app/ferias/services.py:495`, `app/fe

[truncado em 20000 de 22645 caracteres]

## 15/11 10:05 · sessao 4a7b06c0b5f1 · - · - · origem=Ronald (tmux)

Auditoria estática dos selos e checklist:

- **Juiz:** `_vigente` chama exclusivamente `DecisaoHE.objects.vigentes().filter(pk=_ja.pk).exists()`; o único predicado continua `revogada_em__isnull=True`, no modelo. Não nasce segundo predicado nem derivação de minutos. `app/ponto/portas/he.py:130`, `app/ponto/portas/he.py:148`, `app/ponto/models.py:1265`.
- **Registro:** não achei autoridade HE/DecisaoHE em `core/juizes.py`; o registro diz que suas autoridades ficam em `JUIZES[familia][pergunta]`. Distinguir “reusa o juiz já existente no modelo” de “juiz HE registrado”: a segunda afirmação é **[não verificado]**. `app/core/juizes.py:7`, `app/ponto/models.py:1265`.
- **Escritor:** o diff não acrescenta escrita; `_gravar` cria/atualiza e limpa os três campos de revogação, `_marcar_revogada` marca e deixa trilha. Os atos continuam chamando esses escritores. `app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:366`, `app/ponto/portas/he.py:398`, `app/ponto/portas/he.py:497`.
- **Relógio:** o helper novo não lê relógio; `vigentes()` verifica nulidade e não datas. O relógio explícito continua no escritor já existente `_marcar_revogada`, `timezone.now()`. `app/ponto/portas/he.py:146`, `app/ponto/models.py:1267`, `app/ponto/portas/he.py:398`.
- **Trava:** todos os usos novos de `_vigente` vêm depois de travar a linha. `decidir_he`, `recusar_ponta_pequena` e `revogar_recusa_do_sistema` têm `@transaction.atomic`; `autorizar_em_lote` tem bloco por item e chama novamente `decidir_he` para travar a mesma linha. A mudança adiciona uma consulta `exists()` por helper com linha existente; no lote que chega a `decidir_he`, são duas consultas adicionais sobre a mesma linha. Não aparece outra entidade travada pelo helper. `app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:293`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:417`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:573`, `app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:592`.

RED novos, **inferência estática contra a base descrita no patch; execução Django [não verificado]**:

| Teste | Base descrita | Cópia atual esperada | Evidência | [nao verificado]
|---|---|---|---|
| RED_1 | Falha: devolve `no_op=True` para `NAO` revogado | Passa: atualiza e limpa marca | `app/ponto/tests/test_he_revogada_na_porta.py:50`; `app/ponto/portas/he.py:197`; `app/ponto/portas/he.py:206` |
| RED_1b | Falha: `(True,True)` em vez de `(False,True)` | Passa: uma atualização e segundo clique idempotente | `app/ponto/tests/test_he_revogada_na_porta.py:68`; `app/ponto/portas/he.py:197` |
| RED_1c | Falha: `(recusados=0,no_op=1)` | Passa: `(1,0)` | `app/ponto/tests/test_he_revogada_na_porta.py:83`; `app/ponto/portas/he.py:262`; `app/ponto/portas/he.py:267` |
| RED_2 | Falha: guarda crua do lote devolve erro | Passa: linha revogada não barra autorização | `app/ponto/tests/test_he_revogada_na_porta.py:92`; `app/ponto/portas/he.py:586` |
| MORDE_3 | Já passa na base: recusa admin vigente barra lote | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:106`; `app/ponto/portas/he.py:587` |
| MORDE_4 | Já passa na base: repetição vigente é `no_op` | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:118`; `app/ponto/portas/he.py:197` |

- Os dois MORDE não serem vermelhos na base é esperado: preservam comportamento anterior. MORDE_3 confere a mensagem específica da guarda, evitando verde por falta de permissão. O helper `_admin` configura a permissão RBAC correta. `app/ponto/tests/test_he_revogada_na_porta.py:114`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:66`.
- **Limitação material:** `assert _rec.call_count == 1` comprova que houve chamada, não que `DiaPago`/fechamento foi relavrado, nem qual competência mudou: o recalculador está mockado. Os testes novos tampouco exercitam serviço prévia/confirmar ou competência exportada. `app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:97`.
- Os testes novos herdam `TestCase`; isoladamente não provam execução da porta fora de transação. Essa lacuna é coberta pelo selo existente em `TransactionTestCase`, que chama `decidir_he` e `recusar_em_lote` diretamente. `app/ponto/tests/test_he_revogada_na_porta.py:32`, `app/ponto/tests/test_atomicidade_porta_he.py:31`, `app/ponto/tests/test_atomicidade_porta_he.py:44`, `app/ponto/tests/test_atomicidade_porta_he.py:66`.

Selos existentes:

- Não encontrei expectativa nesses arquivos que o diff, por si, contrarie. O273 exige escritores únicos, limpeza de marca, ausência de estados novos e leitores por `vigentes()`; o helper satisfaz essas condições. Reproduzi somente a varredura textual de leitores de O273, sem Django: lista vazia. `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:395`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:445`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:460`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:534`.
- O214 continua encontrando `_gravar` nos dois chamadores e continua encontrando exatamente os mesmos três chamadores de `acima_do_limite`; o diff não adiciona limites literais executáveis nem altera templates. `app/ponto/tests/test_o214_ponta_pequena.py:296`, `app/ponto/tests/test_o214_ponta_pequena.py:313`, `app/ponto/tests/test_o214_ponta_pequena.py:325`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`.
- **O shell de atomicidade já ficaria vermelho na cópia atual por ocorrências fora do diff.** Executei apenas `python3 -B bin/atomicidade_da_porta.py app`, ferramenta que lê/analisa AST; resultado: `total=35 sob_atomic=25 arquivos=17 VIOLACAO=10`. O shell marca vermelho quando esse varredor retorna erro. `bin/atomicidade_da_porta.py:47`, `bin/atomicidade_da_porta.py:55`, `bin/atomicidade_da_porta.py:97`, `bin/tests/test_atomicidade_da_porta.sh:28`.
- As dez ocorrências reportadas foram `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`, `app/ferias/services.py:485`, `app/ferias/services.py:495`, `app/ferias/services.py:636`, `app/ferias/services.py:639`, `app/pautas/services.py:351`, `app/ponto/services/ausencia.py:1517`; nenhuma em `he.py`. São resultados do critério sintático do selo, não demonstração de falta efetiva de transação herdada de chamador. `bin/atomicidade_da_porta.py:70`, `bin/atomicidade_da_porta.py:76`.
- Não execute o próprio shell sob esta autorização: ele cria diretório/arquivos temporários e os apaga. `bin/tests/test_atomicidade_da_porta.sh:41`, `bin/tests/test_atomicidade_da_porta.sh:42`, `bin/tests/test_atomicidade_da_porta.sh:43`, `bin/tests/test_atomicidade_da_porta.sh:64`.

## 15/11 10:05 · sessao cde97262f688 · - · - · origem=Ronald (tmux)

Chamadores e efeitos, por leitura estática:

- `views.decidir_he` chama a porta por alias `_porta`; sobre recusa revogada + pedido `NAO`, passa de mensagem “já estava assim” a “Trocado”, com minutos do clique e recalcular: `app/ponto/views.py:520`, `app/ponto/views.py:532`, `app/ponto/views.py:539`, `app/ponto/views.py:542`; a nova decisão revive a linha com origem admin: `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:83`.
- `views.recusar_he_em_lote` passa de contar `no_op` a contar `recusados` para esse mesmo caso: `app/ponto/views.py:617`, `app/ponto/views.py:618`, `app/ponto/views.py:619`; contador da porta em `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:267`, `app/ponto/portas/he.py:270`.
- `autorizacao_he_periodo._ato` passa de erro nomeado a autorização efetiva sobre recusa revogada acima do limite: `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/portas/he.py:574`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:592`.
- `lavrar_he_pendente` e comando `recusar_ponta_pequena` mantêm resultados para as mesmas linhas; apenas sofrem consulta extra de vigência quando a porta chega a ela. Seus mapas já usam `.vigentes()`: `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`; predicado antigo de nulidade equivale ao juiz atual `app/ponto/models.py:1265`.
- A gestão já mostrava recusa revogada como `sem_decisao`, por consultar somente vigentes e aplicar esse default: `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:278`. O diff permite concluir os gestos antes bloqueados; após conclusão, estado e colunas são consultados vivos: `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:250`, `app/ponto/services/gestao_he.py:311`.

Prévia versus confirmar:

- Recusa revogada entra no universo do período porque `estado_por_dia` exclui revogadas; `itens_sem_decisao` recolhe os dias `sem_decisao` acima do limite: `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:401`, `app/ponto/services/gestao_he.py:405`; `_pedido_de_periodo` usa esse universo nos dois passos: `app/ponto/views.py:668`, `app/ponto/views.py:703`, `app/ponto/views.py:741`.
- Ambos executam `_ato`, que mede antes, autoriza pela mesma porta, mede depois e subtrai: `app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`, `app/ponto/services/autorizacao_he_periodo.py:287`.
- A prévia desfaz tudo via rollback; confirmar compara o dicionário exato e levanta exceção dentro do atomic em divergência: `app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:323`, `app/ponto/services/autorizacao_he_periodo.py:343`, `app/ponto/services/autorizacao_he_periodo.py:346`, `app/ponto/services/autorizacao_he_periodo.py:352`.
- Cada `_medir` pede autoridade nova; ela cria motor e o motor carrega decisões vigentes por instância: `app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/autorizacao_he_periodo.py:96`, `app/ponto/services/espelho.py:1111`, `app/ponto/motor_calculo_v2.py:1400`, `app/ponto/motor_calculo_v2.py:1416`. Assim não identifiquei cache de decisão anterior que faça prévia ignorar a autorização recém-gravada dentro da transação. Confirmação executada com cenário revogado e valores reais: [não verificado].
- O valor mostrado é o mesmo `r['diff']` serializado e carregado no form: `app/ponto/views.py:720`, `app/ponto/views.py:723`, `app/templates/ponto/gestao_he_periodo_previa.html:50`, `app/templates/ponto/gestao_he_periodo_previa.html:90`.
- Atenção semântica: “gravado” nesta guarda significa resultado do motor após gravar `DecisaoHE`, não comparação com `DiaPago` depois dos callbacks de commit; `_medir` só consulta `autoridade_do_periodo`: `app/ponto/services/autorizacao_he_periodo.py:88`, `app/ponto/services/autorizacao_he_periodo.py:94`. Competência exportada é rotulada e gera pauta quando houve autorização: `app/ponto/services/autorizacao_he_periodo.py:288`, `app/ponto/services/autorizacao_he_periodo.py:299`.

Matriz: assumir autor autorizado, motivo válido, minutos positivos; para lote autorizar minutos acima do limite, para ponta pequena minutos dentro do limite, para revogar minutos acima. Guardas desses domínios em `app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:178`, `app/ponto/portas/he.py:184`, `app/ponto/portas/he.py:190`, `app/ponto/portas/he.py:335`, `app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:491`, `app/ponto/portas/he.py:574`.

| Linha | decidir NAO | decidir AUT | recusar lote | autorizar lote | ponta pequena | revogar |
|---|---|---|---|---|---|---|
| Sem linha | cria→igual | cria→igual | recusa→igual | autoriza→igual | cria sistema→igual | sem decisão→igual |
| Vigente admin NAO | no_op→igual | troca→igual | no_op→igual | erro→igual | já decidido→igual | admin→igual |
| Vigente admin AUT | troca→igual | no_op→igual | recusa→igual | no_op→igual | já decidido→igual | admin→igual |
| Vigente sistema NAO | no_op→igual | troca→igual | no_op→igual | erro→igual | já decidido→igual | revoga→igual |
| Revogada sistema NAO | **no_op→troca/revive admin** | troca→igual | **no_op→recusa** | **erro→troca AUT** | revive sistema→igual | já revogada→igual |

Referências da matriz: `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:366`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:478`, `app/ponto/portas/he.py:497`, `app/ponto/portas/he.py:587`, `app/ponto/portas/he.py:592`.

Selos inspecionados:

- O214 exige conjunto exato das chamadoras de `acima_do_limite`; helper novo não altera esse conjunto: `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:542`, `app/ponto/portas/he.py:148`.
- O273 censo de leitores crus exclui explicitamente `ponto/portas/he.py` e `ponto/models.py`; helper está dentro da exclusão: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:473`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:476`.
- O273 marca escritor único também exclui a porta; diff não introduz atribuição fora dela: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:438`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:441`.
- Não executei testes nem modifiquei arquivos. Verde/vermelho empírico: [não verificado]. [nao verificado]

## 15/11 10:05 · sessao 2c6c74fa406d · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem executar testes: [nao verificado]

**Escritores de produção**

| Campo | Todos os sítios encontrados |
|---|---|
| `estado` | `_gravar`: criação `app/ponto/portas/he.py:66`, atualização `:75`, persistência `:84`. Recebe admin de `decidir_he` `:206` e sistema de `recusar_ponta_pequena` `:366`. |
| `origem` | `_gravar`: criação `app/ponto/portas/he.py:68`, atualização `:76`, persistência `:84`; origem admin passada em `:208`, sistema em `:368`. |
| `minutos_na_decisao` | `_gravar`: criação `app/ponto/portas/he.py:67`, atualização `:75`, persistência `:84`; nunca atualizado pela revogação `:398`. |
| `revogada_em` | Default nulo do modelo `app/ponto/models.py:1335`; `_gravar` limpa em `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve relógio em `:398`, persiste em `:400`. |
| `revogada_motivo` | Default vazio `app/ponto/models.py:1340`; `_gravar` limpa `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve `:399`, persiste `:400`. |
| `minutos_na_revogacao` | Default nulo `app/ponto/models.py:1343`; `_gravar` limpa `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve `:399`, persiste `:400`. |

Não apareceu escritor de aplicação fora de `app/ponto/portas/he.py`; o suposto admin do Django mencionado em `app/ponto/models.py:1261` não está registrado no arquivo `app/ponto/admin.py:1` a `:48`. A completude contra referências dinâmicas arbitrárias é [não verificado].

**Leitores diretos dos valores**

- `estado`: antes da escrita/trilha `_gravar` `app/ponto/portas/he.py:61`; idempotência `:197`; retorno de ponta pequena `:355`; trilha da revogação `:395`, `:408`; guarda da revogação `:485`; guarda/erro do lote `:587`, `:591`; `__str__` `app/ponto/models.py:1361`; motor `app/ponto/motor_calculo_v2.py:1417`; mapa da gestão `app/ponto/services/gestao_he.py:86`; agregação da gestão `:107`, `:109`; montagem do retrato `app/ponto/services/he_pendente_lavrado.py:145`.
- `origem`: antes/trilha `_gravar` `app/ponto/portas/he.py:62`; antes/trilha revogação `:396`, `:408`; guarda de autoria da revogação `:475`; mensagem de erro do lote `:591`. Nenhum template encontrado lê origem diretamente; a gestão agrupa apenas por estado em `app/ponto/services/gestao_he.py:107`.
- `revogada_em`: antes/trilha `_gravar` `app/ponto/portas/he.py:63`; juiz `app/ponto/models.py:1267`; trilha após revogar `app/ponto/portas/he.py:409`. `_vigente` pergunta ao juiz, sem reler o atributo em Python `:148`.
- `minutos_na_decisao`: antes/trilha `_gravar` `app/ponto/portas/he.py:61`; retorno de idempotência `:203`; retorno de ponta pequena `:355`; antes/trilha revogação `:395`, `:408`; retornos de revogação `:477`, `:484`, `:490`, `:493`, foto `:494`; soma de decisão por estado na gestão `app/ponto/services/gestao_he.py:108`.
- `revogada_motivo` e `minutos_na_revogacao`: não encontrei leitor explícito do valor armazenado no produto; o log usa os argumentos `motivo` e `int(minutos)`, não os atributos recém-gravados, em `app/ponto/portas/he.py:409` e `:410`. A ferramenta de máscara inventaria `revogada_motivo` em `bin/sombra_regras_pessoais.py:125`; seu processamento genérico está em `bin/sombra_mascara_pessoal.py:44` e o selo em `bin/sombra_selo_pessoal.py:74`.

**Leitores indiretos e telas**

- Motor carrega somente vigentes por alias `_DHE`, `app/ponto/motor_calculo_v2.py:1417`; lê autorização em `:1456`, `:1653`, com efeito nos clipadores `:1697`, `:1739`. Não usa os snapshots para pagar.
- Gestão `estado_por_dia` consulta vigentes em `app/ponto/services/gestao_he.py:87`; `_decisoes` soma snapshots das vigentes em `:105`. `enriquecer` lê ambos `:249`, `:250`, publica estado por dia `:278`, contagens pendentes `:281`, `:285`, e somas/contagens de decisões `:312`.
- Gestão propaga estado ao calendário `app/ponto/services/gestao_he.py:141`; filtra estados `:332`; prepara seleção de lote somente sem decisão `:405`; Central considera ausente no mapa vigente como pendente `:475`, `:477`.
- Template `app/templates/ponto/gestao_he.html`: espiga por estado `:109`; badges `:124`, `:125`; snapshots agregados `:219`, `:220`; oferta de autorização individual `:256`; tooltip/cores/LED/risco `:258`, `:259`, `:267`, `:273`; tabela de pontas `:314`.
- Folha usa o mesmo mapa vigente em `app/folha/porta_export.py:682` a `:686`. Qualquer estado presente libera aquele dia no filtro, inclusive `nao`, em `:232`; ausência de decisão acima do limite entra na trava `:230` a `:234`.
- Comando lavrador fornece mapa vigente de objetos `app/ponto/management/commands/lavrar_he_pendente.py:48`; `apurar` consome estado `app/ponto/services/he_pendente_lavrado.py:145`, podendo antes revogar pela porta `:143`.
- Comando recusa pequena carrega existência de vigentes `app/ponto/management/commands/recusar_ponta_pequena.py:120`, pula essas chaves `:141`, chama porta nas restantes `:170`.
- Serviço de período não lê campos/ORM de `DecisaoHE`; delega lote `app/ponto/services/autorizacao_he_periodo.py:277` e mede antes/depois via autoridade do motor `:271`, `:280`, `:94`.

**Censo ORM fora de `.vigentes()`**

Só quatro consultas executáveis encontradas, todas na porta e todas travando a linha crua:

| Consulta | Com linha revogada |
|---|---|
| `app/ponto/portas/he.py:195` | Obtém/trava a linha; `_vigente` devolve `None` `:196`; não faz idempotência por estado; `_gravar(_ja,...)` revive a mesma linha e limpa revogação `:206`, `:83`. |
| `app/ponto/portas/he.py:343` | Obtém/trava; não considera já decidido `:344`; dry-run retorna sucesso sem escrita `:357`; apply revive pelo `_gravar` `:366`. |
| `app/ponto/portas/he.py:470` | Obtém/trava; para sistema revogada retorna `ja revogada` `:478`, `:483`; se origem admin retorna antes `decisao do admin` `:475`. |
| `app/ponto/portas/he.py:580` | Obtém/trava; `_vigente` devolve `None` `:586`; não barra lote por estado; segue para `decidir_he` `:592`. |

`DecisaoHE.objects.create` `app/ponto/portas/he.py:66` é escrita. `DecisaoHE.objects.filter(...)` em `app/ponto/management/commands/recusar_ponta_pequena.py:219` é texto de instrução de reversão, não consulta executada ali.

**ESMERIL-SEMÂNTICA**

- O contrato distingue estado da decisão e vigência: `NAO` é decisão `app/ponto/models.py:1283`, mas linha revogada não vale segundo `:1267`. A porta passa a respeitar isso em suas quatro políticas `app/ponto/portas/he.py:196`, `:344`, `:478`, `:586`.
- `origem` é autoria do estado atual, não último clique humano `app/ponto/models.py:1290` e `:1294`. Uma recusa **vigente do sistema**, seguida de `NAO` humano, continua `no_op`, conserva origem sistema e snapshot antigo `app/ponto/portas/he.py:197` a `:204`. Isso é preexistente, não mudança do diff.
- Consequentemente, “com ciência” na tela não prova autoria humana: template rotula todo estado `nao` assim em `app/templates/ponto/gestao_he.html:125`, `:220`, `:314`, e a agregação não separa origem em `app/ponto/services/gestao_he.py:107`. A descrição “o admin autorizou N minutos” `:96` não deve ser usada para inferir autoria dos `nao`.
- `minutos_na_decisao` continua sempre foto, nunca pendência atual `app/ponto/models.py:1278`; a gestão soma essa foto como decisão vigente `app/ponto/services/gestao_he.py:108`, enquanto os minutos vivos dos atos vêm do retrato `:388`. `minutos_na_revogacao` é a outra foto, na revogação `app/ponto/models.py:1343`.
- Na revivificação, o motivo/minutos da revogação deixam a linha atual `_gravar` `app/ponto/portas/he.py:83`; sua história está no log anterior de revogação `:406`, sujeito ao best-effort com exceção logada `:411`. O diff não cria esse comportamento.

_5577 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
