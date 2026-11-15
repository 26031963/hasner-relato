# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 10:19** · publicado em **15/11/2026 10:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Há duas lacunas semânticas concretas:

1. **Escolha:** VALIDACAO não testemunha materialização. T distingue `validada_sem_efeito` (`chamados/services/tarefa_do_dia.py:169`). O materializador da escolha devolve sucesso sem Batida (`chamados/services/materializacao.py:1069`), depois carimba e lavra (`:1283`), sem registrar DP de materialização. SILENCIO testemunha fechamento humano (`chamados/models.py:1988`); ABSORCAO testemunha ausência vinculada (`chamados/services/materializacao.py:569`). Nenhum deles cobre toda escolha.

   Pergunta: **qual testemunha factual substitui C na materialização de escolha, preservando a distinção entre validação e efeito?** Pausar esse ramo da unidade E.

2. **Retratação auditada antiga:** `_retratada_com_trilha` exige C mais auditoria `retratar_pergunta_obsoleta` (`chamados/juizes.py:291`). RETRATACAO atual significa remoção do motivo, seguida de identidade (`chamados/models.py:2110`, `:2120`, `:2125`). Nenhum escritor produtivo atual desse nome de auditoria apareceu; acervo real [nao verificado].

   Pergunta: **qual fato conserva o fechamento obsoleto auditado e o distingue da remoção do motivo?** A06 pode conservar identidade aplicável; substituir universalmente por RETRATACAO + log não tem equivalência demonstrada. Pausar essa unidade fora de A06.

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhum arquivo editado.

As 16 unidades no HEAD informado:

| Unidade | Linha antiga preparo → atual | Leitura atual relevante |
|---|---:|---|
| `_retratada_com_trilha` | 285? → 285 | C:291; auditoria:294-296 |
| `_slot_esperado` | 341? → 344 | R:351,363; delega ao plantio:374-376 |
| `_pergunta_materializada_de_fato` | 662 → 665 | C/VIA:682,694,710 |
| `_chave_memo_pv` | 821 → 824 | R/V/C/VIA:855-857 |
| `_resposta_concorda_com_a_luz` | 1330? → 1333 | R:1359 |
| `projecao_temporal_da_pergunta` | 1378 → 1381 | R/V/C/VIA:1388-1398 |
| `pergunta_sem_lastro_ou_viva` | 1492? → 1495 | C:1499 |
| `respondida_sem_texto` | 1525? → 1528 | R:1531 |
| `q_respondida_sem_texto` | 1548 → 1544 | R SQL:1551 |
| `q_respondida_com_texto` | 1556 → 1554 | R SQL:1559 |
| `perguntas_corrigiveis` | 1712? → 1716 | R SQL:1718 |
| `perguntas_respondidas_a_validar` | 1721? → 1723 | R ordenação:1727 |
| `declarou_hora` | 1747? → 1738 | R:1753 |
| `delta_da_resposta` | 1894? → 1890 | R:1900 |
| `classificar_resposta_validacao` | 1998? → 1996 | R:2004,2009 |
| `motivo_implausivel_validacao` | 2020? → 2017 | R:2026 |

As interrogações indicam que o preparo forneceu linha do acesso, sem linha de declaração; não apresentar essas antigas como medidas.

**Escritores/fatos, compartilhados pelas unidades D01/D03/D06:**

- R humano: `chamados/services/disputa_emissao.py:1481`, `:1482`; mesma testemunha `_agora` em DP RESPOSTA `:1525`; nova resposta limpa V `:1494`.
- R automático: `chamados/services/disputa_emissao.py:1673`, `:1674`; DP mesmo instante `:1677`.
- R reparado por trilha: `chamados/services/disputa_emissao.py:1695`; DP instante da testemunha `:1698`.
- Nascimento com R/V recebido: `chamados/models.py:3102` chama conservação e `registrar`.
- V: `chamados/services/validacao.py:159`, `:160`; DP mesmo instante e autor `:164`.
- DEVOLUÇÃO: limpa V `chamados/services/validacao.py:224`, `:225`; DP `:230`.
- REABERTURA/RECUSA: `chamados/services/disputa_emissao.py:1631` registra foto e instante `:1632-1637`; porta do lote limpa projeção `:1648`. RECUSA humana entra nessa porta pelo fluxo `chamados/services/validacao.py:626`.
- C e VIA têm atribuição única em memória: `chamados/models.py:3180-3184`; identidade usa a porta `:3265-3268`.
- SILÊNCIO: DP com instante do C em `chamados/models.py:1988-1990`.
- RETRATAÇÃO: DP em `chamados/models.py:2120-2124`, identidade em `:2125`.
- ABSORÇÃO: DP com mesmo `agora` do carimbo `chamados/services/materializacao.py:567-572`.
- Escrita física DP: `chamados/models.py:4086`, `:4096`; ordenação canônica `:4032`; conservação `:4057-4083`.
- Efeito Batida: registro `chamados/services/materializacao.py:933-939`; vínculo, inclusive reuso, `:952-956`; `timestamp`, `retratada_em`, `criado_em` são campos distintos em `ponto/models.py:23`, `:67`, `:79`.

**Quem lê os valores migrados, por unidade:**

- `_retratada_com_trilha`: E em `chamados/juizes.py:800`, `:818`, `:821`; lote em `chamados/services/carga_pergunta_viva.py:194`.
- `_slot_esperado`: E `chamados/juizes.py:755`; lote `chamados/services/carga_pergunta_viva.py:119`; explicador `chamados/services/explicador.py:29`; vigia `chamados/services/vigia_carimbo.py:110`; classe do lote `chamados/services/validacao.py:907`.
- E: P `chamados/juizes.py:1486`; lampada/lavra `chamados/veredito_lavrado.py:263`; materialização `chamados/services/materializacao.py:1239`, `:1433`; raiox `chamados/services/raiox.py:191`; explicador `chamados/services/explicador.py:93`; backfill `chamados/management/commands/backfill_veredito_lavrado.py:105`.
- Memo: `chamados/juizes.py:893`, `:905`.
- Concordância: P `chamados/juizes.py:1432`.
- Projeção temporal: P `chamados/juizes.py:1403`; slot `:350`.
- Sem lastro: corrigíveis `chamados/juizes.py:1719`; leitor de declaração `chamados/services/declaracao_texto.py:240`.
- Respondida sem texto: porta e vigia `chamados/services/fio.py:187`, `:1602`; `chamados/services/vigia_fio.py:629`; admin `chamados/juizes.py:1706`.
- Face SQL com texto: `chamados/models.py:3453-3454`; fila `chamados/juizes.py:1725`. Outros consumidores diretos da face sem texto fora da declaração: [nao verificado].
- Corrigíveis: `chamados/models.py:2282`; `chamados/services/acoes_disputa.py:96`; `chamados/views.py:1419`; template tag `chamados/templatetags/chamados_tags.py:202`.
- Fila a validar: `chamados/services/validacao.py:956`; `api/views_mensageria.py:540`.
- Declarou hora: `chamados/services/relavra_via.py:97`; `chamados/services/validacao.py:185`; `chamados/services/materializacao.py:555`.
- Delta: classificação `chamados/juizes.py:2010`; lote `chamados/services/validacao.py:899`.
- Classificação: tags `chamados/templatetags/chamados_tags.py:147`, `:174`; mensageria `api/views_mensageria.py:544`; comando `chamados/management/commands/validar_classe_a.py:63`.
- Implausível: validação `chamados/services/validacao.py:429`; tag `chamados/templatetags/chamados_tags.py:22`.
- Reexportação histórica das autoridades: `chamados/models.py:4226`.

**Esmeril semântico:**

- `ato_em` é instante do ato (`chamados/models.py:3984-3992`, `:4016`), ordenado com `pk` (`:4032`). R/V humanos usam o mesmo instante da projeção (`disputa_emissao.py:1474-1482`, `:1525`; `validacao.py:159-166`).
- Ambiente aplica `USE_TZ=True`, fuso `America/Sao_Paulo` (`config/settings/base.py:93`, `:95`). “Todo datetime está materializado em UTC em memória” não é garantido pelo campo; [nao verificado]. Não confundir representação/fuso com equivalência de instante.
- Instante do ato não é hora declarada do trabalho. Texto `data_hora` é convertido em horário local consciente (`chamados/juizes.py:363-369`). Para `hora`, a data sai da cadeia de marcos do turno (`:299-334`) e da função de plantio (`:374-376`).
- `data_evento` é dia do turno; conversa guarda dia civil e é excluída dessa interpretação (`chamados/juizes.py:337-341`). Preservar essa separação.
- C não equivale a efeito: registro, vínculo, timestamp do trabalho e instante de criação da Batida são distintos (`materializacao.py:933-956`; `ponto/models.py:23`, `:40`, `:79`). Marco D06 deve captar efeito/retratação e lavra; só o último timestamp do trabalho não basta.
- T atual está em `chamados/services/tarefa_do_dia.py:140-185`, não nas antigas linhas do preparo. Passivo em `:152-153`; devolução `:163-164`; validação sem efeito `:169-170`.

**Dois riscos concretos para a construção:**

1. `_slot_esperado` delega à F4 ainda leitora de projeção (`chamados/juizes.py:374-376`); precisa fornecer os valores do ato ao delegado sem alterar F4.
2. Guarda de escritor chama `respondida_sem_texto` com `SimpleNamespace` sem relação DP (`chamados/services/fio.py:1598-1605`). Além disso, A05 aplica R/V/C em memória antes de chamar P (`chamados/models.py:3212-3222`). Preservar essas exceções expressamente; uma leitura irrestrita de `p.decisoes` quebraria esses caminhos.

## 15/11 09:58 · sessao 9ff5783795fe · ESPINHA-NA-FRENTE · sonda · origem=Code

**NOVO: 248 dias sem porta; ANTIGO: 7 dias de fio morto.** A saída fornecida mede 248, enquanto o aval anexo menciona 247. Todos os números abaixo vêm exclusivamente dessa saída, sem arquivo numerado para conferência. **[nao verificado]**

O NOVO lista também os dias com porta ou responsável: apenas `sem_porta` incrementa `fio_morto_alcance`. [vigia_fio.py:658](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:714).

**Nenhum plano apresentado permite atribuir zeragem integral aos recortes atuais.** E4c migra leitores; E4d interrompe a projeção; E4B declara cobertura geral das classes a–e e deixa os itens 18–19 para o fim. O anexo E4c enumera arquivos por F1–F9, sem associar essas causas à zeragem. `e4c_reconfere_v2.md:7–15`; [BACKLOG.md:1968](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1968), [BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977).

E3-RESTO-R3 remete ao lote LU; RAIZ trata a reação conjunta de perguntas e disputas. Essas linhas também não identificam quais dias desta saída zerariam. [BACKLOG.md:768](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:768), [BACKLOG.md:1988](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1988).

Para atribuir a cura, faltam:

- **a:** identificar a ação alcançável que faltou; o mapa registra que o PRONTO não transforma `registrado` em ação. [RELATO.md:8823](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:8823).
- **c:** ligar a resposta sob terminal ao ato reparador; `reabrir_credor` retorna sem ato para pergunta respondida. [disputa_emissao.py:1842](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1842).
- **d:** diagnosticar a retenção e o ato correspondente; a definição da classe exige medida antes de cura. [BACKLOG.md:859](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:859).
- **e/terminal:** conferir a carga do credor e o resultado do reparo E4B; outra pergunta exportada, limbo ou recusa podem impedir a reabertura. [disputa_emissao.py:1855](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1855).
- **e/sem viva e ANTIGO:** vincular cada dia ao ato executável e medir o resultado; o backfill distingue `tarefa_nada` e dias que continuam sem porta. [backfill_fio_morto_e4b.py:117](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:117), [backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:368).

As contas fecham pelo dado fornecido. **[nao verificado]**

- NOVO sem porta: **212 + 11 + 10 + 7 + 2 + 4 + 2 = 248**.
- NOVO completo: **1.560 + 248 + 104 + 65 + 19 + 5 + 248 = 2.249**.
- ANTIGO morto: **6 + 1 = 7**. Universo completo de lâmpadas: **2.381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2.914**.
- Interseção dos **7 antigos com os 2.249 novos: 6**, todos classificados `porta_fila` no NOVO. Interseção entre os **dois contadores de mortos: 0**.
- **“Sem obra” com zeragem demonstrada: 248 NOVOS + 7 ANTIGOS = 255 dias distintos entre os alarmes.** A sonda não identifica individualmente quais antigos compõem cada causa.

Na tabela, **E/A = `exportada=True` / `exportada=False`**; todas essas contagens são do dado fornecido. **[nao verificado]** “Sem obra” significa sem zeragem atribuível demonstrada pelos planos acima.

| Universo · rota/causa/detalhe | E/A | Código classificador | Obra que zera |
|---|---:|---|---|
| NOVO · `porta_app/-/-` | 0/1.560 | [tarefa:634](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:634) | Fora do contador morto |
| NOVO · `porta_fila/-/-` | 0/248 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `porta_pergunta/-/-` | 0/104 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `decisao_do_dia_pendente/-/-` | 0/65 | [vigia:306](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:306) | Fora do contador morto |
| NOVO · `fio_do_admin/-/-` | 0/19 | [vigia:313](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:313) | Fora do contador morto |
| NOVO · `aguarda_decisao_de_ausencia/-/-` | 0/5 | [vigia:308](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:308) | Fora do contador morto |
| NOVO · `sem_porta/d/-` | 0/212 | [vigia:696](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:696) | **sem obra** |
| NOVO · `sem_porta/c/-` | 0/11 | [vigia:694](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:694) | **sem obra** |
| NOVO · `sem_porta/a/em_analise` | 0/10 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/aberto` | 0/7 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/registrado` | 0/2 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/e/pergunta_viva_sob_chamado_terminal` | 0/4 | [vigia:706](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:706) | **sem obra**; candidata E4B |
| NOVO · `sem_porta/e/sem_pergunta_viva` | 0/2 | [vigia:704](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:704) | **sem obra** |
| ANTIGO · `pergunta_morreu` | 0/6 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |
| ANTIGO · `sem_pergunta_nenhuma` | 0/1 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |

## 15/11 09:57 · sessao ccf03c37b60d · - · - · origem=Ronald (tmux)

Confirmo a leitura conservadora: **não há plano que demonstre zeragem dos recortes atuais por causa**; há obras candidatas e cobertura parcial documentada. `app/docs/RELATO.md:8820`, `app/docs/RELATO.md:8823`, `app/docs/RELATO.md:8826`, `app/docs/RELATO.md:8827`.

- **a/registrado:** o mapa anterior declara “PRONTO nao transforma registrado em acao”. `app/docs/RELATO.md:8823`.
- **c/respondida sob terminal:** o reparo de credor exclui respondidas; o código retorna sem ato quando `respondida_em` existe. `app/docs/RELATO.md:8827`; `app/chamados/services/disputa_emissao.py:1842`.
- **d:** o mapa registra a lacuna “folha retem, tarefa nada”; não demonstra cura pela E3 ou E4B. `app/docs/RELATO.md:8820`; `app/docs/RELATO.md:610`.
- **e/terminal:** E4B é candidata, mas pode recusar por limbo, outra pergunta exportada na carga ou recusa da reabertura; `exportada=False` do dia não basta. `app/chamados/services/disputa_emissao.py:1855`, `app/chamados/services/disputa_emissao.py:1860`, `app/chamados/services/disputa_emissao.py:1869`.
- **e/sem viva e ANTIGO:** falta diagnóstico que vincule cada dia ao ato reparador; o comando determina atos por tarefa e mede depois se o dia permaneceu sem porta. `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:368`.

Recomendo escrever **“sem obra com zeragem demonstrada”**, evitando afirmar ausência de obra registrada: E4B cobre genericamente classes a–e, E4c migra leitores e E4d interrompe projeção. `app/docs/BACKLOG.md:1977`; `app/docs/BACKLOG.md:1968`.

Subtotal conservador: **248 NOVO + 7 ANTIGO = 255 incidências sem zeragem atribuível demonstrada** pelo material disponível. Os números atuais vêm exclusivamente do enunciado. `[nao verificado]`

## 15/11 09:56 · sessao 3cff7206f1ea · - · - · origem=Ronald (tmux)

- **E4c/E4d não têm zeragem por causa demonstrada nos anexos.** BACKLOG define E4c como migração dos leitores DIVERGE e selo; E4d como parar a projeção. O inventário F1–F9 do anexo identifica arquivos, não um reparo que zere cada classe. `app/docs/BACKLOG.md:1968`; anexo `e4c_reconfere_v2.md`, itens F1–F9.
- **E4B é a obra candidata ao passivo a–e**, explicitamente; porém a mesma linha informa apenas que itens 18–19 ficam para o fim do programa. Não demonstra que estes 248 dias desaparecerão. `app/docs/BACKLOG.md:1977`.
- **O próprio comando exige classificação adicional:** deriva atos da tarefa; sem ato executável retorna `tarefa_nada`/`nao_sei`; retira essas linhas da execução; depois mede quais continuam sem porta. A saída fornecida não contém esses atos/resultados por dia. `app/chamados/management/commands/backfill_fio_morto_e4b.py:109`; `:117`; `:262`; `:368`.
- **Classe d tem lacuna documentada:** a definição do contador não equivale ao “sair de d” do aval; exige medida antes de cura. RELATO também registra ausência de decisão na E3 para o estado retido cuja tarefa não exige nada. `app/docs/BACKLOG.md:859`; `app/docs/RELATO.md:610`.
- **E3-RESTO-R3 remete ao lote LU**, mas sua linha não relaciona LU-01..07/LU3 às causas a/c/d/e nem promete zerá-las. `app/docs/BACKLOG.md:768`.
- **RAIZ-DA-DISPUTA trata reação conjunta de raiz e filhos**, destino e órfãos, mas não contém associação dos 248/7 dias atuais a uma fatia que os zere. `app/docs/BACKLOG.md:1988`; `app/docs/GRADE-MESTRA.md:108`.

Classificação conservadora: **“sem obra com zeragem comprovada”** para os 248 NOVOS e os 7 ANTIGOS; E4B pode ser indicada como candidata, sem subtrair dias do subtotal “sem obra”. Para atribuição verificável falta plano por ramo/ato e resultado do DRY sobre os dias fornecidos. `app/docs/BACKLOG.md:1977`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`; `:368`.

## 15/11 09:56 · sessao 95f73f751961 · - · - · origem=Ronald (tmux)

- **NOVO:** `alcance_universo` reúne dias retidos pela Folha, por colaborador/data, nas competências enumeradas; não é o contador de mortos. `app/chamados/services/vigia_fio.py:584–600`. Apenas `sem_porta` incrementa `fio_morto_alcance`. `app/chamados/services/vigia_fio.py:714–718`.
- **Classificações NOVO:** `a` = presença de chamado vivo do dia ou âncora viva fora da fila, detalhe com status; `c` = pergunta viva respondida com destino terminal; `d` = nenhuma pergunta viva, dia fora da lista e colaborador na lista; `e/sem_pergunta_viva` e `e/pergunta_viva_sob_chamado_terminal` são residuais posteriores. `app/chamados/services/vigia_fio.py:679–707`.
- **Rotas publicadas:** `porta_fila`/`porta_pergunta` vêm da tarefa em `app/chamados/services/tarefa_do_dia.py:628–632`; `porta_app`, em `app/chamados/services/tarefa_do_dia.py:634`; o vigia apenas as lê e conta. `app/chamados/services/vigia_fio.py:658–665`.
- **Baldes com ação nomeada:** decisão pendente, ausência aberta e fio do admin são classificados respectivamente em `app/chamados/services/vigia_fio.py:306–307`, `:308–310`, `:313–314`; entram antes das causas de `sem_porta`. `app/chamados/services/vigia_fio.py:670–678`.
- **ANTIGO:** parte de chamados vivos com célula, restringe o vocabulário e exige lâmpada acesa; primeiro separa fio vivo, hoje/futuro e os baldes com dono. `app/chamados/services/vigia_fio.py:334–379`, `:412–448`. No residual, `ps` não vazio gera `pergunta_morreu`; vazio gera `sem_pergunta_nenhuma`. `app/chamados/services/vigia_fio.py:449–450`.
- A sonda fornecida permite **6 `pergunta_morreu` + 1 `sem_pergunta_nenhuma` = 7**, todos abertos; não imprime qual célula pertence a qual dessas duas causas. Atribuição individual permanece **[nao verificado]**.
- As **6 células antigas presentes no NOVO estão todas em `porta_fila` no dado fornecido**. Logo, a interseção dos 7 antigos com o universo NOVO de 2249 é **6**; com os **248 `sem_porta` é 0**, conforme as linhas fornecidas **[nao verificado]**.
- A partição antiga `com_alvo=0`, `sem_alvo=5`, `sem_pergunta_na_porta=2` também soma 7, mas é classificação distinta e não equivale às causas a–e. `app/chamados/services/vigia_fio.py:467–480`.
- Para eventual conferência do universo completo antigo: **2381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2914**, conforme agregados fornecidos **[nao verificado]**; o código declara a identidade em `app/chamados/services/vigia_fio.py:104–107`.
- Exportação do ANTIGO compõe a janela do dia com `competencia_entregue`; não presume o mês civil. `app/chamados/services/vigia_fio.py:843–865`.

## 15/11 09:53 · sessao 698c79408f3e · ESPINHA-NA-FRENTE · leitura · origem=Code

**“Nada” abaixo significa nenhum pedido adicional demonstrado pelo recorte descrito.** O anexo fornece alvos da E4c; o BACKLOG descreve CONTRACT e fatos da E4d, mas o diff final de cada pouso é **[nao verificado]**. `e4c_reconfere_v2.md:7–15` (anexo); `app/docs/BACKLOG.md:1968–1975`.

Cláusulas usadas: **SMOKE** = BUG 73, duas cascas; PRODUCAO-MAXIMA 5 permite smoke posterior somente quando resta TELA, com reversão pronta. **BATIDA** = kernel espera Ronald. **DINHEIRO** = aberta preaprovada com reversão anterior, hash das exportadas intacto e prova de frota posterior; gravado exportado exige `!` e REGEN-EM-EXPORTADA. **PARADA 2 / §4** = dados ESCALA/VÍNCULO, motor e apagar/voltar arquivo usado em prod exigem `!`. **CADASTRO** = regra variável precisa de cadastro pela UI. `CLAUDE.md:202–203`, `CLAUDE.md:143–144`, `CLAUDE.md:265–268`, `CLAUDE.md:406–417`, `CLAUDE.md:43–44`; `producao_maxima.txt:1` (anexo).

**Dinheiro não pode receber “exportada intacta” por presunção:** validação/materialização alcançam o fechamento da competência do fato; a cópia persiste também outros status e tem guarda específica para 09/2026. Competências e valores efetivamente atingidos por cada futura fatia: **[nao verificado]**. `app/chamados/services/validacao.py:470–472`; `app/ponto/services/fechamento.py:1809–1810`, `app/ponto/services/fechamento.py:1137–1162`, `app/ponto/services/fechamento.py:36–43`.

| fatia | pede | cláusula | arquivo:linha |
|---|---|---|---|
| **E4c F1 — juízes** | **Smoke: nada direto**, sem template/JS/view no alvo. **Batida/dinheiro:** migra predicados e fila de validação; efeito efetivo sobre esses caminhos **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA/DINHEIRO se alterar o caminho executor; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:7` (anexo); `app/chamados/juizes.py:1528–1559`, `app/chamados/juizes.py:1722–1724` |
| **E4c F2 — modelos** | **Smoke: nada direto. Batida: conferir com Ronald o recorte de `_nascer`**, pertencente ao caminho de emissão alcançado pela batida; estado/seleção/apresentação isolados não demonstram esse efeito. **Dinheiro: [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; §4; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:8` (anexo); `app/chamados/models.py:2922`, `app/chamados/models.py:3124–3131`; `app/chamados/services/fio.py:1161–1167` |
| **E4c F3 — validação/verbos/painel/ações** | **Smoke: nada direto. Batida: Ronald se mudar validação → materialização. Dinheiro:** validação agenda recálculo da competência **do fato**, aberta ou exportada conforme o caso; exportada gravada pede `!`. **Escala: `!` se atingir aceite de folga**, que chama `declarar_dia` → `lancar_folga`. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/validacao.py:346–347`, `app/chamados/services/validacao.py:460–487`; `app/ponto/services/declarar_dia.py:179–193`; `app/chamados/services/materializacao.py:932–940` |
| **E4c F4 — emissão/materialização/passivo** | **Smoke: nada direto. Batida: Ronald no recorte que alterar plantio. Dinheiro:** materialização → `registrar_batida` → fechamento por evento; passivo exportado tem caminho explícito **plantio → REGEN → Pauta DP**. **`!` para executar retificação exportada e regeneração de DNA/células.** Competências atingidas **[nao verificado]**. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/materializacao.py:932–940`, `app/chamados/services/materializacao.py:1012–1024`; `app/chamados/services/passivo_validado.py:433–449`; `app/ponto/portas/regen_exportada.py:87–104` |
| **E4c F5 — fio/reconciliador e leitores** | **Smoke: nada direto. Batida: Ronald para o recorte do reconciliador/contrato no caminho da batida. Dinheiro:** POST do fio pode chamar materialização, alcançando competência do fato; tarefa/vigia/apresentação isolados não demonstram movimento. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito; impacto real [nao verificado].** | BATIDA; DINHEIRO 1–2; PARADA 2; CADASTRO | `app/ponto/signals.py:206–215`; `app/chamados/reconciliador.py:1429–1442`; `app/chamados/services/fio.py:698–708`, `app/chamados/services/fio.py:1165` |
| **E4c F6 — views/API/canal/tags** | **Smoke:** Meu Atendimento, detalhe local, modal do fio e prévia/resultado do lote; nas duas cascas onde compartilhados. **API:** preservar fases públicas e adicionar exigibilidade; isso não demonstra alteração da API de bater ponto. **Batida/dinheiro:** seleção do lote alcança validação → materialização/recálculo; Ronald se esse caminho mudar, `!` se mover gravado exportado. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/views.py:550`, `app/chamados/views.py:632–633`, `app/chamados/views.py:927`; `app/chamados/views_cobrar.py:169–208`, `app/chamados/views_cobrar.py:353–386`; `app/api/urls.py:67–73`; `app/docs/BACKLOG.md:1999` |
| **E4c F7 — quatro partials** | **Smoke de Ronald:** Meu Atendimento/card, detalhe/pergunta, modal do fio e drawer do lote; **duas cascas** conforme o grafo. Pode ficar para depois **se** o diff conservar apenas apresentação/leitura, com reversão pronta. **Kernel direto: nada. Dinheiro:** nenhuma escrita demonstrada pelo recorte de apresentação; os formulários acionam portas existentes. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE + PRODUCAO-MAXIMA 5; PARADA 2; CADASTRO | `app/templates/chamados/meu_atendimento.html:1`, `app/templates/chamados/meu_atendimento.html:127`; `app/templates/chamados/detalhe_local.html:310`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/templates/chamados/partials/_lote_preview.html:21–26`; `app/core/context_processors.py:210` |
| **E4c F8 — ponto/calendário/ficha/prévia** | **Smoke:** calendário do perfil, cabeçalho do fio e Prévia da Folha se a mudança alcançar essas saídas; template/JS futuro **[nao verificado]**. **Batida: Ronald no recorte `flip_auto`**: leitura alimenta cron com `--apply` → `flip_tipo` → `registro_batida.anotar`. **Dinheiro:** prévia é leitura; alteração monetária decorrente do flip **[nao verificado]**. **`!` se houver apply que regrave célula/ata; motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; SMOKE se front; DINHEIRO se houver efeito; PARADA 2 / §4; CADASTRO | `app/colaboradores/views.py:1948–1965`; `app/chamados/views.py:918–930`; `app/folha/previa.py:28–31`; `app/ponto/services/flip_auto.py:128–140`; `app/config/crons.py:707`; `app/ponto/services/flip_batida.py:12–39`; `app/ponto/services/cartorio.py:951–956` |
| **E4c F9 — comandos** | **Smoke: nada. Migrar leitores não implica executar apply.** Se o pouso incluir `validar_classe_a --apply`, chama validação real: **Ronald para batida**, DINHEIRO na competência do fato e **`!` se alcançar gravado exportado**. Outros comandos escrevem atos/carimbos; universo/competências dos applies **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/management/commands/validar_classe_a.py:85–100`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:323–331`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:53–68`; `app/chamados/management/commands/relavrar_via_i6.py:96–102` |
| **E4d — CONTRACT** | **Arquivos do futuro corte: [nao verificado].** Conferência funcional identificável: fio e atendimento ao parar projeção. **Smoke obrigatório** depende de tocar front. **Kernel/dinheiro/escala/vínculo:** conservação dos efeitos após desligar projeção **[nao verificado]**; não cabe declarar “nada”. **Motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA/DINHEIRO conforme alcance; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1968`; `app/chamados/models.py:2525–2527`, `app/chamados/models.py:3982–3992`; `app/templates/chamados/partials/modal_fio.html:217–229`; `app/templates/chamados/partials/_card_atendimento.html:189–192` |
| **E4d — FATO-A03, reabertura** | **Smoke candidato:** fio, “Reabertura administrativa”; obrigatório se editar esse front. Porta atual escreve disputa/chamado/pergunta. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1970`; `app/chamados/services/acoes_disputa.py:235–274`; `app/templates/chamados/partials/modal_fio.html:377–390` |
| **E4d — FATO-A05, assinatura** | **Tela/casca, kernel, dinheiro, competência e arquivos do pouso: [nao verificado].** O registro apenas nomeia “ramo de assinatura”; não sustenta escolher uma assinatura de escala como alvo. **Motor, apagar arquivo e cadastro novo: [nao verificado].** | SMOKE/BATIDA/DINHEIRO/PARADA 2/CADASTRO: enquadramento depende do sítio ainda não identificado | `app/docs/BACKLOG.md:1971`; [nao verificado] |
| **E4d — FATO-A06, identidade/duplicata** | **Smoke candidato:** atendimento mostrando repetida/via; obrigatório se editar front. Porta atual grava identidade/carimbo/motivo. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1972`; `app/chamados/models.py:3251–3273`; `app/chamados/services/fio.py:1384–1401`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A17, pendência técnica** | **Smoke candidato:** pendência no fio/card. **Batida/dinheiro:** fato está dentro do materializador; se a troca alterar prosseguimento para plantio, espera Ronald e pode alcançar fechamento da competência do fato, inclusive exportada → `!`. Delta real **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1973`; `app/chamados/services/materializacao.py:851–857`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:222–226`; `app/templates/chamados/partials/_card_atendimento.html:189` |
| **E4d — FATO-A18, recusa técnica** | **Smoke candidato:** recusa/motivo no fio/card. **Batida/dinheiro:** mesma fronteira materializadora; mudança na recusa pode mudar plantio e recálculo da competência do fato. Ronald para kernel; **`!` se mover gravado exportado**; impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1974`; `app/chamados/services/materializacao.py:1089–1113`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A20, conferência de data** | **Smoke candidato:** fio, “Datas em disputa / Conferir data”. **Batida/dinheiro:** conferência libera gate materializador → plantio → fechamento da competência do fato; Ronald para kernel, **`!` se alcançar gravado exportado**. A regra “só o par conferido” já está decidida; implementação/impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/services/validacao.py:660–709`; `app/chamados/views.py:1292–1314`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/chamados/services/materializacao.py:1199–1220`; `app/docs/CORTES.md:440` |
| **Retirada física posterior — campos legados** | Está registrada **depois da série**, separada de parar projeção. **Smoke/kernel/dinheiro e arquivos da migration: [nao verificado].** Remover campo não demonstra apagar arquivo de prod; **se apagar/voltar arquivo usado, `!`**. **Cadastro novo: nada descrito.** | PARADA 2; demais cláusulas dependem do diff | `app/docs/BACKLOG.md:1969`; `CLAUDE.md:265–268` |

_5570 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
