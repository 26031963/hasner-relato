# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 18:17** · publicado em **30/10/2026 18:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 18:17 · sessao 412d5bddd613 · - · - · origem=Ronald (tmux)

O campo do chamado é **`ChamadoColaborador.status_local`**, com terminais `resolvido`, `fechado`, `cancelado`, `superado`; `registrado` pertence a `VIVOS`. Há um núcleo físico de transição, `_gravar_estado`, que atribui o campo e salva, mas ele não veta encerramento por `pergunta_viva`: `app/chamados/models.py:46`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/models.py:192`, `app/chamados/models.py:231`.

**Portas e condição de encerramento**

| Porta | Terminal e condição | C6 antes da escrita? | Prova |
|---|---|---|---|
| `transicionar`, chamado por `fechar` | `resolvido`/`fechado`, permitido pelo motor/perfil; fechado recusa justificativa pendente; encerrados recusam disputa aberta viva; resolvido de furo exige premissa regularizada | **Sim, limitado:** `disputa_viva_do_chamado` pega a primeira disputa aberta e consulta `fio_vivo` das perguntas dela. Não enumera todas as perguntas cujo destino é o chamado | `app/chamados/models.py:122`, `app/chamados/models.py:130`, `app/chamados/models.py:142`, `app/chamados/models.py:165`, `app/chamados/models.py:262`, `app/chamados/juizes.py:1817`, `app/chamados/juizes.py:1821`, `app/chamados/juizes.py:1111` |
| `reconciliar` | VIVOS → `resolvido`; recarrega status do banco; furo só fecha com premissa regularizada para folha, pai agrupador vivo ou outra cobrança viva do dia; sem dia permite com trilha | Não como veto geral; outro juiz: premissa/célula/folha | `app/chamados/models.py:434`, `app/chamados/models.py:463`, `app/chamados/models.py:470`, `app/chamados/models.py:484`, `app/chamados/models.py:489`, `app/chamados/models.py:495`, `app/chamados/models.py:498` |
| `cancelar` | VIVOS → `cancelado` | Não | `app/chamados/models.py:527`, `app/chamados/models.py:532`, `app/chamados/models.py:537`, `app/chamados/catalogo/motor.py:116` |
| `superar` | VIVOS → `superado`; superador diferente de si e não terminal | Não; sem chamador produtivo declarado | `app/chamados/models.py:543`, `app/chamados/models.py:547`, `app/chamados/models.py:560`, `bin/esmeril_chamado_censo.py:143` |
| `encerrar` | aberto/em análise/registrado/resolvido → `fechado`, via cascade | Não | `app/chamados/models.py:1288`, `app/chamados/models.py:1297`, `app/chamados/catalogo/motor.py:114` |
| `encerrar_por_competencia_trancada` | VIVOS → `fechado`, via competência trancada | Não no método; o serviço mede a competência antes | `app/chamados/models.py:1300`, `app/chamados/models.py:1305`, `app/chamados/models.py:1310`, `app/chamados/services/competencia_trancada.py:158`, `app/chamados/services/competencia_trancada.py:160` |

`_gravar_estado` relavra perguntas depois do save quando a transição cruza a fronteira ENCERRADOS; isso é efeito posterior, não guarda anterior: `app/chamados/models.py:231`, `app/chamados/models.py:243`, `app/chamados/models.py:246`.

**Todos os sítios produtivos de chamada encontrados para transições terminais**

Nas linhas que chamam `reconciliar`, o terminal é sempre `resolvido`, sujeito à guarda comum acima; nas que chamam `encerrar`, é `fechado`: `app/chamados/catalogo/motor.py:111`, `app/chamados/catalogo/motor.py:114`.

| Chamador / disparador / condição própria | Sítio(s) |
|---|---|
| Signal de disputa fechada, pai existente e ainda não ENCERRADO | `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42` |
| Retratação canônica: retém irmã respondida sem consumo e justificativa alheia pendente no ato de aprovar; depois reconcilia | `app/chamados/reconciliador.py:439`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525` |
| Chamador da retratação: reconciliação por colaborador, premissa julgada morta; evento da batida e cron | `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1363`, `app/chamados/reconciliador.py:1369` |
| Chamador da retratação: célula absolveu chamado de turno; consumidor do outbox | `app/chamados/reconciliador.py:933`, `app/chamados/reconciliador.py:951`, `app/chamados/reconciliador.py:955` |
| Fecho forçado com retenção, usado pela aprovação de justificativa e ajuste retroativo | `app/chamados/reconciliador.py:487`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:498`; chamadores: `app/ponto/services/justificativa.py:79`, `app/ponto/services/justificativa.py:385` |
| Desligamento: todos NAO_FECHADOS do colaborador → `encerrar` | `app/colaboradores/services/desligamento.py:132`, `app/colaboradores/services/desligamento.py:137` |
| Nova escala/aceite: chamados vivos anteriores de categoria escala_aceite → `encerrar` | `app/colaboradores/services/vinculo.py:373`, `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387` |
| Regularização externa materializada, havendo chamado → `encerrar` | `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:201`, `app/chamados/services/regularizacao_ext.py:205` |
| Serviço de competência trancada: aplicar, trancada, com ações → porta específica | `app/chamados/services/competencia_trancada.py:156`, `app/chamados/services/competencia_trancada.py:160`, `app/chamados/services/competencia_trancada.py:177` |
| Adesão: filho furo no período é agrupado sob pai; reconcilia | `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115` |
| Adesão estoque: pai nunca_bateu com batida, só com fechar e sem dry | `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:144`, `app/chamados/services/adesao.py:148`, `app/chamados/services/adesao.py:150` |
| Adesão por evento batida/pós-vínculo: batida não retratada na janela do pai | `app/colaboradores/services/pos_vinculo.py:17`, `app/colaboradores/services/pos_vinculo.py:36`, `app/colaboradores/services/pos_vinculo.py:41`, `app/colaboradores/services/pos_vinculo.py:42` |
| Aceitar sugestão de folga: grava template e fecha LIMBO-FOLGAS vivos daquele template | `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`, `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:101` |
| Agrupar furo sob pai LIMBO, ainda não agrupado | `app/chamados/services/limbo_folgas.py:214`, `app/chamados/services/limbo_folgas.py:217`, `app/chamados/services/limbo_folgas.py:221`; detector chama em `app/ponto/management/commands/detectar_ausencias.py:756` |
| Silêncio por ausência: ausência aprovada e premissa morta, com aplicar | `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:100`, `app/chamados/services/silencio_ausencia.py:101` |
| Ausência do chamado saiu de ABERTAS; evento fecha, cron de ausência virou vigia | `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:676`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:705`, `app/ponto/services/ausencia.py:709` |
| Materialização de resposta ausência: fecha destino só quando ausência não está ABERTA | `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:538`, `app/chamados/services/materializacao.py:539` |
| Materialização: destino e clones; retém irmãs respondidas sem consumo | `app/chamados/services/materializacao.py:679`, `app/chamados/services/materializacao.py:701`, `app/chamados/services/materializacao.py:702`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:713`, `app/chamados/services/materializacao.py:717` |
| Entrada sincronizada: cobrança batida_ausente do dia e todos os marcos acesos | `app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:28`, `app/ponto/services/entrada_sincronizada.py:29`, `app/ponto/services/entrada_sincronizada.py:31` |
| Registro de batida na tela: E fecha falta; S fecha turno da entrada, recusa outro turno ainda aberto; premissa_morta | `app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:535`, `app/ponto/services/triagem_batida.py:580`, `app/ponto/services/triagem_batida.py:593`, `app/ponto/services/triagem_batida.py:597` |
| Cobertura designada, origem viva e premissa_morta | `app/ponto/services/cobertura.py:114`, `app/ponto/services/cobertura.py:117`, `app/ponto/services/cobertura.py:122` |
| Cobertura passada: chamados vivos do dia, premissa_morta, sobrevivente por último | `app/ponto/services/cobertura.py:309`, `app/ponto/services/cobertura.py:310`, `app/ponto/services/cobertura.py:311`, `app/ponto/services/cobertura.py:318` |
| Admin decide dia: fecha perguntas por admin; chamados vivos por ordem do sobrevivente | `app/ponto/services/veredito_celula.py:160`, `app/ponto/services/veredito_celula.py:177`, `app/ponto/services/veredito_celula.py:182`, `app/ponto/services/veredito_celula.py:183` |
| Cartório, aplicar: todos os marcos do chamado acenderam | `app/ponto/services/cartorio.py:1153`, `app/ponto/services/cartorio.py:1163` |
| Cartório, aplicar: marcos cobrados não existem mais | `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1174`, `app/ponto/services/cartorio.py:1175` |
| Cartório, aplicar: COBRANCA_EM_DIA_SEM_TRABALHO ou COBRANCA_SEM_FURO; sobrevivente por último | `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196` |
| Cartório, aplicar: fio sem viva, sem resposta escrita, todas as perguntas VIA_DUPLICATA | `app/ponto/services/cartorio.py:1258`, `app/ponto/services/cartorio.py:1261`, `app/ponto/services/cartorio.py:1263`, `app/ponto/services/cartorio.py:1265`, `app/ponto/services/cartorio.py:1266` |
| Lastro horário: julgar retorna com_lastro, aplicar, recarrega e exige VIVOS | `app/ponto/services/lastro.py:300`, `app/ponto/services/lastro.py:304`, `app/ponto/services/lastro.py:306`, `app/ponto/services/lastro.py:310`, `app/ponto/services/lastro.py:317` |
| Feriado/folga espúria: cron e evento compartilham serviço | `app/chamados/services/feriado_reavaliar.py:8`, `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:36`, `app/chamados/services/feriado_reavaliar.py:43`, `app/chamados/services/feriado_reavaliar.py:47` |
| Cluster espúrio: colaborador ativo com escala ativa sem fim; chamados cuja premissa morreu | `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/cluster_espurio.py:46`, `app/ponto/services/cluster_espurio.py:67`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:80` |
| Detector ausências: limpa LIMBO antigo | `app/ponto/management/commands/detectar_ausencias.py:158` |
| Detector ausências: afastamento / cobertura / entrada coletados | `app/ponto/management/commands/detectar_ausencias.py:812`, `app/ponto/management/commands/detectar_ausencias.py:816`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:826`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:837` |
| Detector intervalo: premissa morta por ausência do dia em datas_furo; dry usa reconciliaria | `app/ponto/management/commands/detectar_intervalo_ausente.py:115`, `app/ponto/management/commands/detectar_intervalo_ausente.py:128`, `app/ponto/management/commands/detectar_intervalo_ausente.py:134` |
| Marcar posto fechado em feriado: alvos de feriado | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:116`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` |
| Cura BUG115: ata sem furo | `app/ponto/management/commands/curar_residuo_bug115.py:150`, `app/ponto/management/commands/curar_residuo_bug115.py:151` |
| Auditoria órfãos: categoria disputa_supervisao sem disputa aberta, com --fechar | `app/chamados/management/commands/auditar_chamados_orfaos.py:34`, `app/chamados/management/commands/auditar_chamados_orfaos.py:39`, `app/chamados/management/commands/auditar_chamados_orfaos.py:58`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67` |
| Meio período: --apply, superuser ativo para assinar; `fechar` default fechado | `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` |
| Aviso régua migrado à lista C7, --apply → cancelar | `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:36`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` |
| Revisão vínculo migrada à lista C8, --apply → cancelar | `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:44`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:52`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| Tela avaliação benefício: aprovar → resolvido; outra ação → fechado | `app/colaboradores/views.py:2055`, `app/colaboradores/views.py:2068` |
| Tela aprovação exceção, agenda aceita/recusada, escala aceita/recusada, geofence autorizado/recusado → resolvido | `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277` |
| Tela resposta livre: gestor, novo_status dentre choices; transicionar limita | `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315` |
| Tela fio: chamado + conteúdo, RBAC, novo_status dentre choices | `app/chamados/services/fio.py:703`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:713`, `app/chamados/services/fio.py:715` |
| Smoke operacional: transições válidas, reabertura e recusas | `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163` |

**Nascimento já terminal e migrações são exceções à passagem física pelo núcleo**

`criar(status=...)` preserva qualquer status diferente de aberto, passa ao `objects.create` e não chama `_gravar_estado`; o smoke exerce nascimento `fechado` e `resolvido`. `abrir` monta `_def` com estado inicial do motor, mas depois incorpora defaults fornecidos pelo chamador antes do get_or_create: `app/chamados/models.py:818`, `app/chamados/models.py:827`, `app/chamados/models.py:851`, `app/chamados/management/commands/smoke_chamados.py:122`, `app/chamados/management/commands/smoke_chamados.py:126`, `app/chamados/management/commands/smoke_chamados.py:146`, `app/chamados/management/commands/smoke_chamados.py:158`, `app/chamados/models.py:702`, `app/chamados/models.py:704`, `app/chamados/models.py:735`.

A migração 0039 grava diretamente cancelado → superado quando prova irmão único nos módulos de furo, mesmo colaborador/data_turno; sua reversa grava superado → cancelado. Ambas passam fora de núcleo e C6: `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

**Modelos relacionados**

`DisputaSupervisao` usa `fechada_em`, não um status terminal; `aberta` é `fechada_em is None`. A porta `fechar` consulta efetivamente C6 para todas as perguntas e recusa vivas no fluxo normal; exceções nomeadas `desligamento`/`admin_encerra` fecham mudas com ato de admin, e todas recusam respostas sem veredito. A gravação física fica em `fechar`: `app/chamados/models.py:1504`, `app/chamados/models.py:1530`, `app/chamados/models.py:1558`, `app/chamados/models.py:1568`, `app/chamados/models.py:1577`, `app/chamados/models.py:1579`, `app/chamados/models.py:1586`, `app/chamados/models.py:1590`, `app/chamados/models.py:1595`.

`RegularizacaoExterna` é entidade auxiliar ligada a chamado, com status próprios aguardando/confirmada/recusada/materializada/cancelada. Seu escritor materializada é `materializar_regularizacao_externa`; recusada volta à supervisão e deixa o chamado aberto. Não confundir esses valores com terminais de ChamadoColaborador: `app/chamados/models.py:2744`, `app/chamados/models.py:2745`, `app/chamados/models.py:2763`, `app/chamados/models.py:2772`, `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:200`, `app/chamados/services/regularizacao_ext.py:261`, `app/chamados/services/regularizacao_ext.py:266`.

A allowlist do esmeril ainda documenta bypass de fechamento de disputa no desligamento, mas o código atual já o migrou para `DisputaSupervisao.fechar`; o comentário da allowlist está desatualizado: `bin/esmeril_chamado_censo.py:64`, `app/colaboradores/services/desligamento.py:140`, `app/colaboradores/services/desligamento.py:147`, `app/colaboradores/services/desligamento.py:150`.

Método executado: AST read-only de todos os Python da árvore para atribuição/AnnAssign/AugAssign a status_local, kwargs em verbos ORM e bulk_update/setattr literais; leitura dos merges de dict e rastreamento manual das portas/chamadores. O AST literal sozinho não encontra `_def`/`**lookup`, execução dinâmica, SQL externo, comandos feitos em shell/produção, plugins ou dados ausentes do snapshot; exaustividade dessas superfícies externas permanece `[nao verificado]`. O varredor existente também declara suas limitações de atribuição e balde não atribuível: `app/core/censo_escritas.py:6`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:49`, `bin/esmeril_chamado_censo.py:99`, `bin/esmeril_chamado_censo.py:113`.

## 30/10 18:17 · sessao 454c1076e906 · - · - · origem=Ronald (tmux)

Concordo com os dois bloqueios identificados:

- **Ata `concorde` sem batida pode continuar produzindo falta antes do encerramento.** `_vv` desliga `futuro` em `app/colaboradores/services/calendario.py:822`; `_classificar` chega a `falta` em `:148` e descarta o `ok` de `concorde` sem batida em `:176`. O comentário “veredito lavrado decide” não corresponde ao comportamento desse ramo (`:820`).
- **Régua suspeita impede o rótulo da folha no dia passado cego.** `regua_suspeita` vence `retido_folha` em `app/colaboradores/services/calendario.py:139`; a seleção do rótulo da folha só ocorre para status `sem_certidao` em `:899`. Assim, mesmo recebendo `sem_celula`, a tela pode continuar dizendo `escala_suspeita` (`:834`, `:913`).

Censo dos campos diretamente afetados:

| Campo | Escritor | Leitores |
|---|---|---|
| `_agora` / `hoje` | `contexto_calendario`, `app/colaboradores/services/calendario.py:258` | `dias_encerrados` (`:712`), recortes `_em_curso`/`_sem_fim` (`:715`, `:723`), retenção (`:726`), `futuro` (`:822`), oferta de resolver (`:945`). |
| Motivo de não encerramento | `_dia_encerrado_motivo_puro`, `app/ponto/turnos.py:2088`; publicação em `dias_encerrados`, `:2139` | Calendário: `_em_curso` e `_sem_fim`, `app/colaboradores/services/calendario.py:714`. |
| Veredito persistido | `CelulaDia.lavrar_veredito` delega à porta, `app/escala/models.py:1775` | Calendário consulta e indexa em `app/colaboradores/services/calendario.py:525`; folha consulta em `app/folha/export.py:1188` e julga em `:1215`. |
| `retido_folha` | Motivo produzido por `motivos_retencao_celula`, `app/folha/export.py:1200`; calendário apenas filtra `sem_celula`, `app/colaboradores/services/calendario.py:727` | `_classificar`, `:141`; rótulo, `:899`. |
| `status` | `_classificar`, `app/colaboradores/services/calendario.py:108`; chamada única em `:814` | Grade (`app/templates/colaboradores/partials/_calendario_grade.html:69`), resumo (`calendario.py:949`), oferta resolver (`:204`), portas `status_do_dia`/`contar_por_status` (`:219`, `:231`). |
| `status_rotulo` | `app/colaboradores/services/calendario.py:899` | Grade, `app/templates/colaboradores/partials/_calendario_grade.html:99`; ramo cego novo em `:107`. |
| `dia_pago` / rubricas | Calendário lê `_linhas_do_periodo` em `app/colaboradores/services/calendario.py:385`, `_rubricas_do_dia` em `:800`; publicação `:922` | `_classificar` recebe desvio em `:827`; badges em `:854`; a mudança não escreve `DiaPago`. |

Custo adicional por montagem, inferido dos caminhos SQL: **uma consulta de vínculos sempre**, apesar da alimentação de turnos e células (`app/ponto/turnos.py:2130`). Havendo dias passados sem fim e sem veredito, mais uma chamada à folha (`app/colaboradores/services/calendario.py:726`), que consulta células (`app/folha/export.py:1188`) e o carregador de vínculos (`:1194`); esse carregador possui a consulta principal e eventual consulta de geradoras faltantes (`app/escala/alimentacao.py:116`, `:125`). Tempo e número efetivo nesta execução: **[não verificado]**.

O custo se multiplica pelos calendários efetivamente montados: perfil (`app/colaboradores/views.py:1946`), drawer (`app/colaboradores/services/drawer.py:121`), painel de furos dentro do laço de colaboradores (`app/relatorios/furos.py:139`, `:184`) e vigia pela porta de um dia (`app/chamados/services/vigia_fio.py:294`; `app/colaboradores/services/calendario.py:213`).

O relógio novo é compartilhado entre encerramento e retenção (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`), mas a montagem da palavra continua chamando `autoridade_do_periodo` e `folha_manda` sem passar esse instante (`:676`, `:689`). Portanto, “uma borda de relógio” está comprovada apenas para a parte nova; toda a montagem compartilhar o mesmo relógio: **[não verificado]**.

O template dá precedência à `palavra_dia` sobre o `status_rotulo` (`app/templates/colaboradores/partials/_calendario_grade.html:98`). Não provei um caso alcançável em que isso esconda o rótulo cego novo: **[não verificado]**.

## 30/10 18:16 · sessao 2ef9bfc27055 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**Veredito: REPROVADA como cumprimento integral do aval das 15:3x.** O caso principal foi corrigido, mas a resposta da folha ainda pode desaparecer na classificação, e o rótulo continua sendo escrito pelo calendário. (`app/docs/PROMPTS.md:1681`; `app/colaboradores/services/calendario.py:141`; `app/colaboradores/services/calendario.py:899`)

1. **P2 — A retenção é consultada, mas pode ser descartada sem vínculo ativo.** Considere dia passado, célula `trabalha=True`, DNA sem `hf`, nenhum veredito e vínculo histórico encerrado cobrindo a data. O juiz de encerramento considera os vínculos históricos; a folha retém essa célula como `sem_celula`. Porém o calendário busca exclusivamente `ativa=True`: sem vínculo ativo, mantém `eh_trabalho=None`. O novo `retido_folha` só é lido dentro de `trabalho is True`; o resultado vira `sem_dados`, com “Sem dados”, apesar da resposta autoritativa já carregada. (`app/ponto/turnos.py:2130`; `app/escala/alimentacao.py:253`; `app/folha/export.py:1237`; [calendario.py:351](/home/ronald/codex_ws/38fe9889eed26fb04bf16edb63c20fb9a625a2ff/app/colaboradores/services/calendario.py:351); `app/colaboradores/services/calendario.py:768`; `app/colaboradores/services/calendario.py:783`; `app/colaboradores/services/calendario.py:134`; `app/colaboradores/services/calendario.py:158`)

   A guarda por vínculo ativo já existia na base; a insuficiência desta obra é conectar o novo resultado da folha atrás dessa guarda. A correção precisa preservar A2 e usar a autoridade do dia, sem promover `retido_folha` a sobrescritor indiscriminado de status. (`6c37ad84:app/colaboradores/services/calendario.py:341`; `6c37ad84:app/colaboradores/services/calendario.py:748`; `app/colaboradores/services/calendario.py:124`; `app/colaboradores/services/calendario.py:834`)

2. **P2 — Há fonte única do motivo, mas ainda não do rótulo exigido.** `motivos_retencao_celula` devolve `data/motivo/celula_id/veredito`; a frase “Dia sem julgamento do cartorio…” nasce em `ROTULO_CEGO_FOLHA`, no calendário. Portanto a tela lê o **código** da autoridade e escreve sua própria interpretação textual; não lê um rótulo fornecido pela mesma autoridade da folha, como pede o aval. Isso não constitui outro juiz de retenção, mas deixa incompleto o contrato literal do rótulo compartilhado. (`app/folha/export.py:1203`; [calendario.py:64](/home/ronald/codex_ws/38fe9889eed26fb04bf16edb63c20fb9a625a2ff/app/colaboradores/services/calendario.py:64); `app/colaboradores/services/calendario.py:726`; `app/colaboradores/services/calendario.py:899`; `app/docs/PROMPTS.md:1681`)

3. **P3 — O censo deixou vocabulário antigo vivo.** O calendário agora usa `previsto` também para hoje e para turno de dia anterior ainda não encerrado, mas a legenda entregue ao copiloto continua definindo “Previsto” como “dia futuro previsto pela escala”. O teste dessa legenda compara os nomes dos itens, não suas explicações; essa contradição passa. (`app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/legenda_calendario.py:18`; `app/colaboradores/legenda_calendario.py:45`; `app/colaboradores/tests/test_legenda_calendario.py:14`)

O aval das **13:3x está atendido no caminho coberto**: o calendário pergunta ao juiz existente, reaproveita seus turnos e células, mantém `previsto` enquanto o turno não encerrou e respeita veredito já lavrado. O tratamento distinto de passado `sem_previsto` corresponde ao complemento das 15:3x; não classificaria essa distinção, isoladamente, como juiz novo ou teto indevido por data. (`app/colaboradores/services/calendario.py:712`; `app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:722`; `app/colaboradores/services/calendario.py:822`; `app/docs/PROMPTS.md:1676`; `app/docs/PROMPTS.md:1681`)

O circuito dos campos afetados é este:

| Campo | Quem escreve/produz | Quem lê |
|---|---|---|
| `ativa`, vigência e `tipo_escala` do vínculo | Portas de criação, encerramento e correção: `app/colaboradores/services/vinculo.py:797`, `:832`, `:842` | Juiz do vínculo: `app/escala/alimentacao.py:237`; carga do encerramento: `app/ponto/turnos.py:2130`; calendário: `app/colaboradores/services/calendario.py:351` |
| Horários do template; `dna.marcos` | Cadastro: `app/escala/services/cadastro_tipo.py:223`; congelamento: `app/ponto/management/commands/gerar_celulas.py:81` | `marcos_do_dia`: `app/escala/models.py:1094`; `marcos_turno`/`fim_turno`: `app/escala/servico_jornada.py:69`, `:92` |
| `CelulaDia.trabalha/dna/escala_geradora` | Porta da célula: `app/ponto/portas/celula.py:247`, `:260` | Calendário: `app/colaboradores/services/calendario.py:785`, `:829`; juiz do vínculo: `app/escala/alimentacao.py:237`; folha: `app/folha/export.py:1237` |
| `veredito/via/em/protestos` | `lavrar_veredito`: `app/ponto/portas/celula.py:691` | `_ver_map` e classificador: `app/colaboradores/services/calendario.py:525`, `:806`, `:830`; retenção: `app/folha/export.py:1215` |
| `Turno.data_turno/batidas/aberto` | Geometria do turno: `app/ponto/turnos.py:310` | Índice do calendário: `app/colaboradores/services/calendario.py:316`; alimentação do juiz: `:713`; classificação: `:826` |
| Motivo de não encerramento; retorno booleano ou `motivos=True` | Mesmo corpo puro: `app/ponto/turnos.py:2097`; adaptação do retorno: `:2139` | `_em_curso` e `_sem_fim`: `app/colaboradores/services/calendario.py:714`, `:722` |
| `futuro` | Campo interno montado em `app/colaboradores/services/calendario.py:822` | `_classificar`: `app/colaboradores/services/calendario.py:135`, `:155` |
| Motivo da retenção → `_cego_folha` → `retido_folha` | Folha: `app/folha/export.py:1200`; transporte: `app/colaboradores/services/calendario.py:727`, `:834` | Classificador: `app/colaboradores/services/calendario.py:141`; composição do rótulo: `:899` |
| `status/status_rotulo` | `_classificar` e montagem: `app/colaboradores/services/calendario.py:189`, `:898` | Grade: `app/templates/colaboradores/partials/_calendario_grade.html:99`; resumo: `app/colaboradores/services/calendario.py:949`; portas curtas: `:228`, `:239`; Resolver dia: `:205` |
| `veredito_dia/palavra_dia/cor_dia` | Trilha separada de `dia_decidido.do_dia`: `app/ponto/services/dia_decidido.py:399`; transporte: `app/colaboradores/services/calendario.py:887` | Template, antes dos ícones de status: `app/templates/colaboradores/partials/_calendario_grade.html:68`, `:98` |

A mudança alcança calendário da ficha, drawer, relatório de furos e, indiretamente, o vigia que consulta “Resolver dia”; portanto o custo e a alteração de status não ficam restritos à página individual. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/chamados/services/vigia_fio.py:294`)

Quanto aos **relógios**:

- O novo caminho está coerente: captura `_agora` uma vez, deriva `hoje` desse instante, passa `_agora` ao juiz e `hoje` à retenção. (`app/colaboradores/services/calendario.py:258`; `:712`; `:726`)
- O juiz continua combinando turno aberto vivo com fim previsto; o fim vem do DNA quando disponível, com encadeamento dos marcos através da meia-noite. A aberta expira pela janela do template mais a margem existente de quatro horas. (`app/ponto/turnos.py:1981`; `:1997`; `:2097`; `app/escala/servico_jornada.py:81`)
- Isso não torna todos os relógios da montagem únicos: o prazo de desfazer captura outro `now()`, e a trilha da palavra chama autoridades próprias. São caminhos já existentes, não prova de regressão deste diff. (`app/colaboradores/services/calendario.py:343`; `:676`; `:689`; `app/relatorios/cartao_pela_celula.py:40`)

O **custo acrescentado por montagem** é sempre um SELECT de vínculos em `dias_encerrados`; os turnos e células já carregados são reaproveitados. Havendo passado `sem_previsto` sem veredito, soma-se uma chamada à folha, com consulta de células e carga de vínculos. Não há uma consulta à folha por dia. (`app/ponto/turnos.py:2129`; `:2130`; `:2132`; `app/colaboradores/services/calendario.py:724`; `app/folha/export.py:1188`; `:1194`)

Em CPU, há percurso dos dias com seleção entre vínculos, além dos turnos; o filtro `r['data'] in _sem_fim` ainda usa lista, podendo acrescentar trabalho quadrático no ramo cego. No relatório de frota, esse custo se repete por calendário montado. (`app/ponto/turnos.py:2132`; `:2136`; `app/escala/alimentacao.py:253`; `app/colaboradores/services/calendario.py:727`; `app/relatorios/furos.py:184`)

**Os testes mordem os casos centrais, mas não certificam esses pontos restantes.** [nao verificado]

- Há vínculo real, antes do turno, meia-noite, turno encadeado além de ontem, depois do fim, veredito lavrado e comparação com a folha. A sentinela no rótulo verifica o consumo pelo template nos três layouts. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:29`; `:42`; `:47`; `:57`; `:61`; `:65`; `:92`; `:104`)
- A fixture permanece com vínculo ativo e sem criação de batidas; não cobre o achado 1 nem a entrada nova `turnos=` com conteúdo. C1 possui batidas, mas suas chamadas não exercitam esse argumento. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20`; `app/ponto/tests/test_c1_dias_encerrados.py:28`; `:51`; `:66`)
- A sentinela prova que o template lê o campo; não prova que o rótulo veio da autoridade da folha. O teste de cobertura também admite que A2 decide antes da cobertura que ele aparenta verificar. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`; `:134`)
- O teste de custo limita o **delta** do ramo cego a duas consultas e compara um contra oito dias; custo comum adicional passa nessa comparação. O selo geral mantém teto de 55 consultas e mede o segundo acesso aquecido. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/core/tests/test_selo_performance.py:249`; `:332`)

Na avaliação isolada dos corpos reais, em memória e com dependências substituídas, confirmei `sem_celula` na folha e `sem_dados` no classificador quando `eh_trabalho=None`; isso verifica a perda do motivo, sem constituir reprodução completa com Django/banco. (`app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:134`; `:158`) Resultado integral da suíte nesta árvore e incidência em produção: **[não verificado]**.

## 30/10 18:14 · sessao 0e744b1d9c96 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**REPROVADA: o aval das 15:3x ainda não fecha.** O calendário consulta a folha, mas pode descartar o motivo recebido e exibir outra palavra. O aval exige que esse dia leia o rótulo do mesmo juiz da folha (`app/docs/PROMPTS.md:1681`; `app/colaboradores/services/calendario.py:726`, `:141`, `:158`).

1. **Dia histórico com vínculo encerrado válido perde o rótulo da folha.** Contraexemplo: dia passado, vínculo encerrado cuja vigência cobre o dia, nenhum vínculo ativo, célula `trabalha=True`, DNA contendo `hi` sem `hf`, `veredito=None`. A autoridade aceita esse vínculo histórico; o calendário procura somente `ativa=True` e deixa `eh_trabalho=None` (`app/escala/alimentacao.py:237`, `:250`; `app/colaboradores/services/calendario.py:351`, `:768`, `:783`).

   Nesse caso, `dias_encerrados` responde `sem_previsto`; a folha responde `sem_celula`; o calendário recebe o motivo, mas `_classificar` só lê `retido_folha` dentro de `trabalho is True`. Com `None`, retorna **`sem_dados`**, publicando “Sem dados”. Confirmei esse retorno executando o corpo real de `_classificar`, extraído por AST em memória (`app/ponto/turnos.py:2102`, `:2130`; `app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:727`, `:134`, `:141`, `:158`, `:899`).

2. **A régua suspeita também esconde o motivo recebido.** Com trabalho previsto, sem batida, sem fim e sem veredito, `regua_suspeita=True` vence `retido_folha='sem_celula'`: retorna **`escala_suspeita`**. O template mostra a explicação da régua, sem o motivo da folha. A precedência já existia, mas a integração acrescentada não satisfaz o rótulo exigido nesse cruzamento (`app/colaboradores/services/calendario.py:139`, `:141`, `:727`; `app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/docs/PROMPTS.md:1681`).

3. **Há leitor de significado não migrado.** `previsto` agora abrange hoje e dias anteriores ainda não encerrados, mas a legenda fornecida ao copiloto continua dizendo **“dia futuro previsto pela escala”**. O teste da legenda compara apenas os nomes, portanto não acusa esse significado errado (`app/colaboradores/services/calendario.py:714`, `:822`; `app/colaboradores/legenda_calendario.py:18`; `app/api/views_mensageria.py:1764`; `app/colaboradores/tests/test_legenda_calendario.py:14`).

O caminho principal das 13:3x está implementado pela autoridade existente: a montagem entrega turnos, células e instante a `dias_encerrados`; o núcleo continua verificando turno vivo e fim previsto. Não há juiz temporal novo nesse trecho nem limite artificial a hoje/ontem (`app/docs/PROMPTS.md:1676`; `app/colaboradores/services/calendario.py:712`; `app/ponto/turnos.py:2097`, `:2105`, `:2137`).

O rastro dos campos lidos ou alterados pelo diff é este:

| Campo ou alimentação | Quem escreve/produz | Quem lê |
|---|---|---|
| `CelulaDia.trabalha`, `dna.marcos`, `escala_geradora` | Geração/regeneração pela porta da célula (`app/ponto/portas/celula.py:6`, `:247`, `:595`). | `eh_dia_trabalho`, `marcos_do_dia`, seleção do vínculo; folha lê `trabalha` (`app/escala/models.py:1066`, `:1094`; `app/escala/alimentacao.py:237`; `app/folha/export.py:1237`). |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `lavrar_veredito`, recebendo julgamento do cartório/admin (`app/ponto/portas/celula.py:647`, `:671`, `:689`). | Calendário monta `_ver_map`; retentor lê veredito/protestos e marcos da ata (`app/colaboradores/services/calendario.py:525`; `app/folha/export.py:1215`, `:1228`, `:1124`). |
| Vínculo: `ativa`, `data_inicio`, `data_fim`, `tipo_escala` | Cadastro e portas de vínculo; encerramento grava `ativa=False` e `data_fim` (`app/colaboradores/services/vinculo.py:167`, `:780`, `:797`). | Calendário escolhe ativo; juíza escolhe vínculo do dia; folha verifica cobertura (`app/colaboradores/services/calendario.py:351`; `app/ponto/turnos.py:2130`, `:2137`; `app/folha/export.py:1193`). |
| Turnos: `data_turno`, `aberto`, batidas | `turnos_do_colab`/pareador; batidas entram pela porta e retratadas ficam fora da apuração (`app/ponto/registro_batida.py:94`; `app/ponto/turnos.py:1848`, `:1858`). | Calendário reutiliza a lista já montada; juíza agrupa por dia e consulta turno vivo (`app/colaboradores/services/calendario.py:316`, `:713`; `app/ponto/turnos.py:2132`, `:2097`). |
| `_motivo_aberto`, `_em_curso`, `_sem_fim` | Resposta da juíza e seleção transitória da montagem (`app/colaboradores/services/calendario.py:712`, `:714`, `:722`). | Formação de `futuro` e seleção da consulta à folha (`app/colaboradores/services/calendario.py:724`, `:822`). |
| Motivo/data da retenção → `_cego_folha` → `retido_folha` | Juiz `motivos_retencao_celula`; montagem filtra somente `sem_celula` (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:727`). | `_classificar` e composição de `status_rotulo`, sujeitos aos escapes acima (`app/colaboradores/services/calendario.py:141`, `:834`, `:899`). |
| `futuro`, `status`, `status_rotulo` | Montagem produz `futuro`; `_classificar` produz status; montagem compõe rótulo (`app/colaboradores/services/calendario.py:814`, `:822`, `:898`). | Grade, resumo, `status_do_dia`, `contar_por_status` e decisão de oferecer Resolver dia (`app/templates/colaboradores/partials/_calendario_grade.html:69`, `:99`; `app/colaboradores/services/calendario.py:204`, `:219`, `:231`, `:948`). |
| `resolver_dia_oferecido` | Juiz existente, após status pronto (`app/colaboradores/services/calendario.py:199`, `:944`). | Template e vigia do fio; portanto a mudança repercute além da cor da tela (`app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`). |

A montagem também é consumida pela ficha, drawer e painel de furos; as portas curtas recompõem o calendário para responder status ou disponibilidade da ação (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/colaboradores/services/calendario.py:213`, `:225`).

**Relógios:** `_agora` e `hoje` vêm do mesmo instante; esse instante segue à juíza e essa data segue ao retentor. A folha mantém seu próprio critério de dia passado e a exceção do dia da demissão, agora explicitamente consultados pelo calendário (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`; `app/folha/export.py:1211`, `:1237`).

O encerramento lê o fim do DNA, com `agora >= fim`; a aberta expira pelo template, com margem padrão de quatro horas e comparação estrita `agora > expiração`. Essa diferença é herdada e está documentada no próprio juiz; não nasceu neste diff (`app/ponto/turnos.py:1992`, `:1997`, `:2007`, `:2039`, `:2102`).

**Custo por montagem:** há **uma consulta adicional incondicional** para todos os vínculos do colaborador, reaproveitando turnos e células. Havendo candidatos cegos, acrescenta-se uma chamada à folha pelo intervalo entre o primeiro e o último candidato, com consultas de células e vínculos. Portanto, nesse caminho, são **+1 consulta no caso comum e até +3 com retenção**, por montagem; tempo real e custo total em produção **[nao verificado]** (`app/ponto/turnos.py:2129`, `:2130`, `:2132`; `app/colaboradores/services/calendario.py:722`, `:726`; `app/folha/export.py:1188`, `:1194`).

**Os testes mordem parte relevante, mas não esses escapes:** [nao verificado]

- Há casos reais para antes do turno, meia-noite, turno encadeado anterior a ontem, falta depois do fim, veredito lavrado, folga, ausência e isenção (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:65`, `:112`, `:123`, `:132`, `:146`).
- O RED cego verifica a premissa na juíza e na folha; o template é renderizado com sentinela em `status_rotulo`. Isso verifica leitura efetiva do campo (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:85`, `:97`, `:104`).
- A fixture usa vínculo ativo e não cruza régua suspeita: deixa passar os dois escapes do rótulo. O teste de custo compara 0/1/8 cegos, mas subtrai uma base que já contém a consulta adicional fixa (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23`, `:29`, `:169`, `:177`).
- A propriedade C1 com células alimentadas não contém batidas para provar o pareamento com essa alimentação; o novo caminho `turnos=` também não recebe uma lista não vazia nos casos novos (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19`, `:42`).

Há ainda **defeito herdado de contrato**, separado das alterações: quando `turnos` é omitido, `dias_encerrados` encaminha células com chave `(colaborador_id, data)` a `turnos_do_colab`, que exige chave `data` e a embrulha novamente. O cartório usa esse caminho. O calendário evita esse repasse ao fornecer os turnos, mas a inconsistência continua na árvore (`app/ponto/turnos.py:2129`, `:2132`, `:1852`, `:1928`; `app/ponto/services/cartorio.py:853`).

**Suite, RED contra a base e smoke deste hash: [nao verificado].** O bloqueador demonstrado é o motivo da folha recebido e descartado pelo classificador; consultar a autoridade não basta enquanto o leitor continua substituindo sua palavra (`app/colaboradores/services/calendario.py:727`, `:134`, `:139`, `:158`; `app/docs/PROMPTS.md:1681`).

## 30/10 18:14 · sessao fd8816f3fb67 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**Veredito: o defeito principal está corrigido, mas eu não certificaria o cumprimento integral dos avais.** A árvore consulta a juíza existente e trata o passado sem fim previsto; ainda faltam o contrato literal do rótulo compartilhado e testes que protejam a dependência da folha. (`app/colaboradores/services/calendario.py:712`, `:714`, `:726`, `:899`; `app/docs/PROMPTS.md:1681`)

**O que está errado ou incompleto:**

1. **O rótulo continua sendo escrito pela tela.** A folha devolve `data`, `motivo`, `celula_id` e `veredito`. O calendário aproveita `motivo='sem_celula'`, mas cria a frase `ROTULO_CEGO_FOLHA` e monta `status_rotulo` localmente. Portanto, compartilha o julgamento da retenção, mas não lê um rótulo produzido pela mesma autoridade, como pede literalmente o aval 15:3x. Isso não constitui outro juiz de retenção; constitui um contrato de apresentação ainda local. (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:64`, `:727`, `:899`; `app/docs/PROMPTS.md:1681`)

2. **Os testes não protegem essa dependência da folha.** Por inspeção, substituir a consulta por `_cego_folha = {d: 'sem_celula' for d in _sem_fim}` preservaria as expectativas: o caso cego espera esse código fixo; os casos de veredito, folga, ausência e isenção escapam por outros ramos; o custo cairia para zero e continuaria passando. Mutante não executado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`, `:113`, `:124`, `:133`, `:147`, `:177`; `app/colaboradores/services/calendario.py:722`)

3. **A prova de custo cobre somente uma parcela.** O teste desconta a montagem sem dias cegos da própria árvore. Assim, esconde a consulta adicional incondicional de vínculos feita por `dias_encerrados`. Também não mede CPU: a filtragem das retenções procura cada data em uma lista, com custo potencial `O(R × K)`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:169`, `:177`; `app/ponto/turnos.py:2130`; `app/colaboradores/services/calendario.py:722`, `:727`)

4. **Um consumidor do significado de `previsto` ficou para trás.** A legenda entregue ao copiloto ainda explica “dia futuro previsto pela escala”, embora agora `previsto` inclua hoje e turnos iniciados em datas anteriores. O teste da legenda compara os nomes dos rótulos, não essa descrição; ele não pega a contradição. (`app/colaboradores/legenda_calendario.py:18`; `app/api/views_mensageria.py:1764`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/tests/test_legenda_calendario.py:14`)

**O núcleo dos dois avais está presente.** A pergunta usa `[data_ini, data_fim]` inteiro, com os mesmos turnos já carregados: não há recorte novo em hoje/ontem. O passado `sem_previsto` sai de `_em_curso`; somente os candidatos sem veredito consultam a folha. A retenção entra como fato em `_classificar`, preservando a precedência humana e o veredito, em vez de sobrescrever o resultado depois. (`app/colaboradores/services/calendario.py:712`, `:714`, `:722`, `:834`; `:124`, `:141`, `:183`)

Quem escreve e quem lê os campos envolvidos:

| Campo | Escritor/produtor | Leitores relevantes |
|---|---|---|
| `CelulaDia.trabalha`, `dna`, `escala_geradora` | Geração/regeneração pela porta; `montar_dna` produz o blob. (`app/ponto/portas/celula.py:243`, `:260`, `:593`, `:612`; `app/ponto/management/commands/gerar_celulas.py:73`) | `marcos_do_dia`, `vinculo_do_dia`, calendário e retenção. (`app/escala/models.py:1094`; `app/escala/alimentacao.py:237`; `app/colaboradores/services/calendario.py:785`, `:829`; `app/folha/export.py:1237`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos` | Porta `lavrar_veredito`. (`app/ponto/portas/celula.py:691`) | Calendário carrega `_ver_map`, impede previsão sobre decisão lavrada e classifica; folha lê veredito/protestos. (`app/colaboradores/services/calendario.py:525`, `:530`, `:822`, `:830`; `app/folha/export.py:1215`, `:1228`) |
| Turnos: `data_turno`, `batidas`, `aberto` | Pareador produz `Turno`; `turnos_do_colab` fornece a janela. (`app/ponto/turnos.py:321`, `:1190`, `:1848`) | Calendário agrupa e reaproveita na juíza; também determina presença e turno aberto. (`app/colaboradores/services/calendario.py:316`, `:713`, `:731`, `:825`) |
| `_agora`, `hoje` | Contexto captura o instante e deriva a data local. (`app/colaboradores/services/calendario.py:258`) | Juíza, folha, `futuro` e oferta de resolução. (`app/colaboradores/services/calendario.py:712`, `:726`, `:822`, `:945`) |
| Motivo de não encerramento | `_dia_encerrado_motivo_puro`; `dias_encerrados` publica motivo ou booleano. (`app/ponto/turnos.py:2097`, `:2105`, `:2139`) | Calendário deriva `_em_curso` e `_sem_fim`. (`app/colaboradores/services/calendario.py:714`, `:722`) |
| Motivo de retenção / `_cego_folha` | Folha escreve o motivo; calendário filtra a resposta. (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:727`) | Fato `retido_folha`, classificador e rótulo. (`app/colaboradores/services/calendario.py:834`, `:141`, `:899`) |
| `futuro` | Contexto: data futura ou dia ainda em curso sem veredito. (`app/colaboradores/services/calendario.py:822`) | Somente os ramos do classificador. É um fato transitório, não campo persistido. (`app/colaboradores/services/calendario.py:135`, `:155`, `:814`) |
| `status` | `_classificar`; contexto publica o resultado. (`app/colaboradores/services/calendario.py:189`, `:898`) | Grade, resumo, portas de consulta e `resolver_dia_oferecido`. (`app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/colaboradores/services/calendario.py:204`, `:228`, `:239`, `:949`) |
| `status_rotulo` | Contexto, usando constantes locais e motivo da folha. (`app/colaboradores/services/calendario.py:899`) | Grade, inclusive o novo fallback. (`app/templates/colaboradores/partials/_calendario_grade.html:99`, `:105`, `:107`) |
| `resolver_dia_oferecido` | Juiz existente lê o status e carimba cada dia. (`app/colaboradores/services/calendario.py:199`, `:945`) | Template e vigia; portanto a mudança de status também alcança a oferta de ação e sua contagem. (`app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`) |

Os chamadores que recebem esses efeitos são perfil, drawer, relatório de furos e vigia pela porta de um dia; `status_do_dia` e `contar_por_status` também remontam o contexto. (`app/colaboradores/views.py:1945`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/colaboradores/services/calendario.py:213`, `:225`, `:236`)

**Relógios:** `_agora` é compartilhado pela decisão de encerramento e pelo cálculo de `hoje`; a folha recebe esse `hoje` explicitamente. O fim previsto vem dos marcos encadeados, inclusive atravessando mais de uma meia-noite; turno aberto vivo pode adiar o encerramento. O segundo `now()` do calendário serve ao prazo de desfazer retratação, outra finalidade. (`app/colaboradores/services/calendario.py:258`, `:343`, `:712`, `:726`; `app/escala/servico_jornada.py:80`; `app/ponto/turnos.py:2097`)

**Custo adicional por montagem:** uma consulta de vínculos sempre; havendo candidatos cegos, mais uma consulta de células e uma de vínculos na folha, salvo saídas antecipadas. Turnos e células são reutilizados na primeira consulta à juíza, evitando recarga por dia. O custo ocorre novamente em cada montagem das portas curtas. O total efetivamente medido neste hash permanece **[não verificado]**. (`app/ponto/turnos.py:2129`; `app/folha/export.py:1174`, `:1184`, `:1188`, `:1194`; `app/colaboradores/services/calendario.py:213`, `:225`)

Os testes **mordem os resultados principais**: antes do turno, madrugada, depois do fim, turno encadeado além de ontem, decisão lavrada, passado cego e consumo do campo pelo template mediante sentinela. O teste C1 também distingue DNA `10–14` de template `19–07`. Não cobrem, porém, a dependência obrigatória da folha nem o caminho `turnos=` com batidas nessa nova prova C1. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:65`, `:92`, `:108`; `app/ponto/tests/test_c1_dias_encerrados.py:62`)

Há ainda um defeito de contrato no caminho global: sem `turnos=`, `dias_encerrados` repassa células com chave `(colaborador_id, data)` a uma função que espera chave `data`, criando chaves aninhadas no adaptador. O calendário evita esse caminho passando turnos; o cartório o utiliza. A comparação indica que é herdado da base, não introduzido nesta alteração **[não verificado por execução]**. (`app/ponto/turnos.py:2113`, `:2132`, `:1852`, `:1928`; `app/ponto/services/cartorio.py:853`)

Execução das suítes neste hash: **[não verificado]**. A análise foi estática; os testes de integração criam e atualizam registros. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20`, `:165`, `:170`)

## 30/10 18:14 · sessao d4daaabd58b0 · - · - · origem=Ronald (tmux)

Contraprova confirmada por execução das funções reais extraídas por AST, em memória, sem Django/DB/escrita:

```text
_classificar(trabalho=True, sem_batida=True,
             futuro=False, veredito='concorde') -> falta
_classificar(..., veredito='coberta')            -> falta
_classificar(..., futuro=True, veredito=None)   -> previsto
```

Qualquer `_vv` cancela a proteção temporal (`app/colaboradores/services/calendario.py:822`); o ramo sem batida escolhe falta (`:148`), e o mapa `concorde -> ok` é deliberadamente apagado por ausência de batida (`:176`). O teste de veredito existente cobre somente `nunca_bateu`, portanto não pega esse caso (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`).

O estado `concorde` com turno não encerrado e sem batida nasce do próprio escritor: executei também `supra_juiz.classificar_dia` real para dia 14/10, hoje 15/10, adesão anterior, previsto 720, realizado 0, batidas 0 e `turno_encerrado=False`; resultado `[]`. `derivar_veredito([])` real devolveu `concorde`. Referências: `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:349`, `app/escala/models.py:1592`. O cartório normal repassa essa proteção temporal e lavra os códigos (`app/ponto/services/cartorio.py:1080`, `:1117`, `:1409`). Assim, **o cenário mais forte é ontem com turno noturno ainda em curso**; o calendário pode acusar falta quando a juíza ainda nega encerramento. Hoje antes do primeiro marco tem guarda adicional do cartório, por isso não alego nascimento medido desse estado em produção (`app/ponto/services/cartorio.py:1048`).

Também fiz mutação AST que remove apenas `retido_folha` do classificador. O caso cego muda de `sem_certidao` para `falta`; portanto a asserção nova discrimina esse ramo (`app/colaboradores/services/calendario.py:141`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`). Régua suspeita, batida presente, A2 e isento permanecem iguais com essa mutação, por precedência (`app/colaboradores/services/calendario.py:124`, `:137`, `:139`, `:159`).

Suite Django, RED contra o hash base e mutação da renderização: [não verificado]. [nao verificado]

## 30/10 18:13 · sessao 0564966d58eb · - · - · origem=Ronald (tmux)

Dois pontos para consolidar, sem execução de Django/banco:

1. **Cumprimento literal incompleto do rótulo compartilhado.** O calendário consulta o juiz da folha, mas ele retorna `data/motivo/celula_id/veredito`, sem rótulo (`app/folha/export.py:1203`). A frase “Dia sem julgamento…” nasce numa constante nova do calendário (`app/colaboradores/services/calendario.py:64`) e é composta localmente (`app/colaboradores/services/calendario.py:899`). Portanto há SSOT do **motivo**, mas não um rótulo produzido pelo juiz da folha. O aval exige a leitura desse rótulo (`app/docs/PROMPTS.md:1681`). Não é correto chamar isso de novo juiz de retenção: a decisão vem de `motivos_retencao_celula` (`app/colaboradores/services/calendario.py:726`).

2. **O motivo calculado pode ser descartado quando há batida.** `_sem_fim` e `_cego_folha` não restringem o universo a dias sem batida (`app/colaboradores/services/calendario.py:722`, `app/colaboradores/services/calendario.py:727`), mas `retido_folha` só participa do ramo sem batida e de trabalho (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:141`). Para um passado com par fechado, DNA sem `hf` e nenhum veredito, o juiz pode retornar `sem_previsto` (`app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2103`), enquanto a folha retém `sem_celula` (`app/folha/export.py:1237`). A classificação com batidas segue `aberto/irregular/ok` (`app/colaboradores/services/calendario.py:159`), e o motivo só aparece no rótulo se o status final for `sem_certidao` (`app/colaboradores/services/calendario.py:899`). O teste novo usa dias sem batida (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:82`); cenário completo com batidas **[não verificado]**. Avaliado em memória pelo corpo real de `_classificar`: `retido_folha='sem_celula'` com `tem_batida=True, celula='ok'` retorna `ok`; com `celula='irregular'`, retorna `irregular`.

Não encontrei regressão demonstrável na trilha `palavra_dia/veredito_dia/cor_dia`: ela permanece ligada ao derivador anterior (`app/colaboradores/services/calendario.py:887`), cujo ramo normal devolve `trabalhou` sem palavra (`app/ponto/services/dia_decidido.py:396`, `app/ponto/services/dia_decidido.py:426`). Essa divergência conceitual é herdada, não introduzida pelo diff.

## 30/10 18:13 · sessao 20d699d43c5a · - · - · origem=Ronald (tmux)

O núcleo novo mantém a autoridade existente: `dias_encerrados` chama `_dia_encerrado_motivo_puro`, e a condição temporal continua `agora >= fim` depois de verificar turno aberto; não identifiquei juiz novo nesse trecho. `app/ponto/turnos.py:2088`, `app/ponto/turnos.py:2105`, `app/ponto/turnos.py:2137`.

Há dois relógios herdados: encerramento pelo DNA congelado (`fim_turno` → `ec.marcos_do_dia`) e expiração da aberta pelo template, com margem padrão de quatro horas e comparação estrita `agora > expiração`. O diff alimenta o DNA, mas não muda a segunda regra. `app/escala/servico_jornada.py:68`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2007`.

A montagem captura um único `now`, deriva dele `hoje`, passa o instante ao juiz e a data à folha; essa borda está coerente. `app/colaboradores/services/calendario.py:258`, `app/colaboradores/services/calendario.py:712`, `app/colaboradores/services/calendario.py:726`.

Custo incremental estático: um SELECT de todos os vínculos com `tipo_escala` por montagem; turnos e células já carregados são reaproveitados. No ramo cego, uma chamada à retenção acrescenta os SELECTs de células e vínculos por envelope, não por dia. Não medi tempo nem consultas em execução. `app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2138`, `app/folha/export.py:1188`, `app/folha/export.py:1194`, `app/escala/alimentacao.py:116`.

Escritores relevantes:

- `trabalha`, `dna`, `escala_geradora`: geração e regeneração na porta da célula. `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:612`.
- `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`: `lavrar_veredito`; o método do modelo delega. `app/ponto/portas/celula.py:689`, `app/ponto/portas/celula.py:695`, `app/escala/models.py:1775`.
- `ativa`, `data_fim`: encerramento pela porta do vínculo. `app/colaboradores/services/vinculo.py:780`, `app/colaboradores/services/vinculo.py:797`.

Leitores relevantes:

- DNA: `EscalaColaborador.marcos_do_dia`; fechamento temporal: `fim_turno` e o juiz. `app/escala/models.py:1094`, `app/escala/servico_jornada.py:92`, `app/ponto/turnos.py:2102`.
- Veredito: calendário monta `_ver_map`; folha lê diretamente `CelulaDia.veredito` e `trabalha`. `app/colaboradores/services/calendario.py:525`, `app/folha/export.py:1215`, `app/folha/export.py:1237`.
- Motivo da folha: calendário filtra `sem_celula`, alimenta `retido_folha` e compõe o rótulo. `app/colaboradores/services/calendario.py:727`, `app/colaboradores/services/calendario.py:834`, `app/colaboradores/services/calendario.py:899`.

Dívida herdada, distinta do diff: o carregador aceita células com chave `(colaborador_id, data)` e as passa diretamente ao `turnos_do_colab` quando não recebe `turnos`; este exige chave `data` e a embrulha novamente. O cartório usa exatamente esse caminho. Isso pode fazer o vínculo do pareamento ignorar a geradora da célula. O teste novo com células alimentadas não contém batidas para exercitar essa diferença geométrica. `app/ponto/turnos.py:2129`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928`, `app/ponto/services/cartorio.py:853`, `app/ponto/tests/test_c1_dias_encerrados.py:62`.

O contraexemplo enviado pelo pai — vínculo encerrado válido no dia, sem vínculo ativo, célula de trabalho sem fim e sem veredito — é compatível com a autoridade canônica, que aceita vínculo encerrado dentro da vigência; a seleção ativa do calendário perde esse vínculo. `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`, `app/colaboradores/services/calendario.py:351`.

## 30/10 18:13 · sessao 0b3b2c996662 · - · - · origem=Ronald (tmux)

Achados do calendário:

1. **O rótulo da folha ainda se perde sem escala ativa.** Contraexemplo: vínculo histórico `ativa=False`, com vigência cobrindo dia passado; célula `trabalha=True`, DNA sem `hf`, sem veredito. O calendário busca somente vínculo ativo (`app/colaboradores/services/calendario.py:351`), deixa `eh_trabalho=None` (`:768`, `:783`), enquanto `dias_encerrados` carrega todos os vínculos (`app/ponto/turnos.py:2130`) e identifica `sem_previsto` (`:2102-2104`). O retentor devolve `sem_celula` (`app/folha/export.py:1237-1238`), mas `_classificar` só consulta `retido_folha` dentro de `trabalho is True` (`app/colaboradores/services/calendario.py:134-144`): com `None`, cai em `sem_dados` (`:151-158`) e publica “Sem dados”, sem motivo da folha (`:899-901`). Medi o corpo real `_classificar`, extraído por AST e executado somente em memória: esses fatos devolvem `sem_dados`. Não executei Django/banco [nao verificado].

2. **Outro filtro anterior engole o motivo da folha:** `regua_suspeita` vence `retido_folha` (`app/colaboradores/services/calendario.py:139-144`). O rótulo então vem da régua (`:913`), embora o novo carregador tenha recebido `sem_celula` (`:727`). A avaliação da régua pode devolver suspeita a partir dos campos persistidos (`app/escala/regua_defesa.py:620-633`). A precedência existente da régua é explícita; considero necessário distinguir este caso do contraexemplo anterior antes de acusar violação do aval [nao verificado quanto ao alcance literal do aval].

3. **Leitor sem migração:** o status `previsto` passou a abranger hoje e dias passados não encerrados (`app/colaboradores/services/calendario.py:714-715`, `:822`), mas o catálogo da legenda continua descrevendo-o como “dia futuro previsto pela escala” (`app/colaboradores/legenda_calendario.py:18`). Essa descrição é servida ao copiloto pela API (`app/api/views_mensageria.py:1759-1767`). O teste da legenda compara apenas o conjunto de nomes, ignorando `quer_dizer` (`app/colaboradores/tests/test_legenda_calendario.py:14-21`).

Escritores/leitores relevantes:

- `CelulaDia.veredito`, `veredito_via`, `veredito_em`, `protestos`: escritor canônico `lavrar_veredito` (`app/ponto/portas/celula.py:647-649`, `:691-697`). O calendário lê em lote para `_ver_map` (`app/colaboradores/services/calendario.py:525-530`); a folha lê célula/veredito/protestos (`app/folha/export.py:1188`, `:1215-1238`).
- `CelulaDia.trabalha`: o calendário lê pela autoridade `ec.eh_dia_trabalho` (`app/colaboradores/services/calendario.py:785-786`; `app/escala/models.py:1066-1073`) e diretamente como `celula_trabalha` (`app/colaboradores/services/calendario.py:829`). A folha usa o mesmo campo no reconhecimento do cego (`app/folha/export.py:1237`).
- `CelulaDia.dna.marcos`: `marcos_do_dia` lê a célula alimentada; sem mapa, consulta o banco (`app/escala/models.py:1094-1107`). O novo caminho encaminha esse mapa a `fim_turno` (`app/ponto/turnos.py:2102`).
- `_motivo_aberto`, `_em_curso`, `_sem_fim`, `_cego_folha`, `futuro`, `retido_folha`: dados transitórios escritos pela montagem (`app/colaboradores/services/calendario.py:712-727`, `:822`, `:834`); `_classificar` consome os dois últimos (`:135`, `:141`).
- `status`: escritor `_classificar`, publicação no dia (`app/colaboradores/services/calendario.py:814-835`, `:898`). Consumidores: grade (`app/templates/colaboradores/partials/_calendario_grade.html:69-107`), resumo (`app/colaboradores/services/calendario.py:948-965`), portas `status_do_dia`/`contar_por_status` (`:219-239`) e `resolver_dia_oferecido` (`:199-205`, `:944-945`). O vigia do fio lê essa última porta (`app/chamados/services/vigia_fio.py:294-299`).
- `status_rotulo`: escritor da montagem (`app/colaboradores/services/calendario.py:899-901`); grade lê o campo, inclusive no ramo genérico alterado (`app/templates/colaboradores/partials/_calendario_grade.html:99-107`).

Custo e relógios:

- `_agora` e `hoje` derivam do mesmo instante; `_agora` é passado à juíza e `hoje` ao retentor (`app/colaboradores/services/calendario.py:258-259`, `:712-713`, `:726`). Existe outro `timezone.now()` para prazo de desfazer, pergunta distinta (`:343-347`).
- Há **+1 consulta incondicional por montagem**, para todos os vínculos (`app/ponto/turnos.py:2130`). O ramo cego acrescenta uma chamada ao retentor (`app/colaboradores/services/calendario.py:724-726`), que consulta células e vínculos (`app/folha/export.py:1188`, `:1194`); o carregador de vínculos pode fazer segunda consulta por geradoras faltantes quando `mais` existe, mas este chamador não o fornece (`app/escala/alimentacao.py:123-127`, `:153-154`).
- O teste novo mede somente a diferença entre ramo com/sem cegos; sua base já contém a consulta incondicional (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177-183`). Não certifica custo total nem o aumento na frota. A rota de furos monta um calendário por card/colaborador (`app/relatorios/furos.py:165-188`).
- O selo geral mantém teto 55 (`app/core/tests/test_selo_performance.py:249`); execução atual e custo real de produção [nao verificado].

Testes: [nao verificado]

- Mordem hoje antes do turno, meia-noite e turno encadeado anterior a ontem (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42-59`), fim já passado (`:61-63`), ata já lavrada (`:65-76`, `:112-121`), cego com vínculo ativo (`:91-110`), folga/ausência/isento (`:123-153`) e custo incremental do ramo (`:155-183`).
- A sentinela do tooltip verifica efetivamente que o template lê `status_rotulo` (`:107-110`).
- A fixture só tem vínculo ativo com tipo (`:23-30`); não cobre o contraexemplo de vínculo histórico, a régua suspeita nem a semântica da legenda. Execução da suite [nao verificado]. [nao verificado]

## 30/10 18:12 · sessao c4020d80c4b7 · - · - · origem=Ronald (tmux)

Achei uma lacuna concreta: **a resposta da folha pode ser carregada e descartada**. Sem vínculo ativo, `ec=None` (`app/colaboradores/services/calendario.py:351`), `eh_trabalho` permanece `None` (`:768`, `:783`). Mesmo com célula passada `trabalha=True`, sem veredito e sem fim previsto, a folha retorna `sem_celula` (`app/folha/export.py:1237-1238`), `_cego_folha` captura (`app/colaboradores/services/calendario.py:722-727`), mas `_classificar` só lê `retido_folha` dentro de `trabalho is True` (`:134-144`); o dia cai em `sem_dados` (`:158`), com rótulo genérico (`:899-901`). O caso precisa entrar no harness; execução dinâmica desse cenário [não verificado].

Mapa dos campos:

| Campo | Escritor | Leitores |
|---|---|---|
| `CelulaDia.veredito/veredito_via/veredito_em/protestos` | `app/ponto/portas/celula.py:691-695`; wrapper `app/escala/models.py:1775-1783` | calendário `app/colaboradores/services/calendario.py:525-530`, `:806`, `:830`; folha `app/folha/export.py:1215-1238` |
| `CelulaDia.trabalha/dna/escala_geradora` | regeneração `app/ponto/portas/celula.py:260`; dia `:532` | marcos `app/escala/models.py:1094-1108`; trabalho no calendário `app/colaboradores/services/calendario.py:785-786`; retenção `app/folha/export.py:1237` |
| Fim previsto | derivado por `app/escala/servico_jornada.py:89-93`, lendo marcos `:69-85` | juiz `app/ponto/turnos.py:2102-2105` |
| Motivo de não encerramento | `app/ponto/turnos.py:2097-2105`, lote `:2137-2139` | `_em_curso` e `_sem_fim`, `app/colaboradores/services/calendario.py:714-723` |
| `futuro` | `app/colaboradores/services/calendario.py:822` | `_classificar`, `:135`, `:155`; campo interno, sem `dia_futuro` nesse fluxo |
| `retido_folha` | código da autoridade `app/folha/export.py:1200-1204`; transporte `app/colaboradores/services/calendario.py:727`, `:834` | `_classificar`, `:141` |
| `status/status_rotulo` | `_classificar`, `app/colaboradores/services/calendario.py:189`, saída `:898-901` | grade `app/templates/colaboradores/partials/_calendario_grade.html:99-107`; resumo `app/colaboradores/services/calendario.py:949-965`; resolver `:205`, `:945`; portas curtas `:225-239` |
| `veredito_dia/palavra_dia/cor_dia` | `app/ponto/services/dia_decidido.py:399-436`; transporte calendário `app/colaboradores/services/calendario.py:887-896` | grade `app/templates/colaboradores/partials/_calendario_grade.html:68`, `:98`; espelho possui transporte próprio `app/ponto/services/espelho.py:355`; API `app/api/views.py:1401-1403`; PDF `app/relatorios/pdf_espelho.py:919-920` |

A grade prioriza palavra não `em_aberto` antes de desenhar `status`, portanto verificar apenas o dicionário de status não cobre toda apresentação (`app/templates/colaboradores/partials/_calendario_grade.html:97-107`).

O rótulo `ROTULO_CEGO_FOLHA` é apresentação local (`app/colaboradores/services/calendario.py:64`), mas o código `sem_celula` vem efetivamente do juiz da folha (`app/folha/export.py:1214`, `:1238`); essa composição, isoladamente, não constitui novo julgamento.

## 30/10 18:12 · sessao d50b322d0446 · - · - · origem=Ronald (tmux)

O núcleo atende aos dois avais nos casos cobertos: usa a janela inteira da autoridade existente (`app/colaboradores/services/calendario.py:712`), exclui apenas o passado `sem_previsto` de `_em_curso` (`:714`), pergunta uma vez à folha pelos dias cegos (`:722`) e injeta a resposta como fato no único `_classificar` (`:141`, `:834`).

Pontos para consolidar:

- **Rótulo compartilhado parcialmente:** a autoridade escreve `motivo='sem_celula'` em `app/folha/export.py:1237`; o calendário lê esse motivo em `app/colaboradores/services/calendario.py:727`, mas escreve sua própria frase em `:64` e `:899`. Isso é SSOT para a retenção, apresentação local para o rótulo; a folha não retorna uma frase de apresentação (`app/folha/export.py:1200`).
- **Lacuna possível do aval 15:3x:** a retenção é calculada para todo passado `sem_previsto` sem veredito (`app/colaboradores/services/calendario.py:722`), mas só governa status no ramo de trabalho sem batida, depois de isento e régua suspeita (`:133`–`:144`). Exemplo estrutural: régua suspeita + passado sem fim + sem veredito continua `escala_suspeita`, embora a folha retenha `sem_celula` (`:139`; `app/folha/export.py:1237`). A precedência A2 é deliberada nos testes (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:124`, `:133`); não equiparar essa precedência a um defeito automaticamente.
- **Custo de CPU escondido:** `_sem_fim` é lista ordenada (`app/colaboradores/services/calendario.py:722`), e `r['data'] in _sem_fim` é executado para cada retenção (`:727`): O(R×K). O teste de custo só mede quantidade de queries (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:172`, `:175`, `:179`), portanto não morde esse crescimento.
- **Custo por montagem:** a consulta nova `dias_encerrados` carrega todos os vínculos uma vez (`app/ponto/turnos.py:2130`); reutiliza células e turnos (`:2129`, `:2132`). Havendo passado sem fim, folha faz carga de células e vínculos (`app/folha/export.py:1188`, `:1194`), com atalhos para isento e janela vazia (`:1174`, `:1184`). Há custo adicional linear na janela no juiz e na folha (`app/ponto/turnos.py:2136`; `app/folha/export.py:1207`).
- **Relógios:** `_agora` escreve `hoje` e alimenta o juiz (`app/colaboradores/services/calendario.py:258`, `:259`, `:712`); folha recebe `hoje` explicitamente (`:726`), evitando seu default `localdate()` (`app/folha/export.py:1179`). Existe outro `timezone.now()` para prazo de desfazer, finalidade independente (`app/colaboradores/services/calendario.py:343`).

Escritores/leitores centrais:

| Campo | Escritor | Leitores relevantes |
|---|---|---|
| `CelulaDia.veredito/veredito_via/veredito_em` | Porta `lavrar_veredito`: `app/ponto/portas/celula.py:692`; fachada `app/escala/models.py:1775`; cartório chama em `app/ponto/services/cartorio.py:1409`, humano em `app/ponto/services/veredito_celula.py:330` | calendário carrega `app/colaboradores/services/calendario.py:525`; decide `_vv` em `:806`, `:822`, `:830`; folha lê em `app/folha/export.py:1215`, `:1237` |
| motivo de não encerramento | `_dia_encerrado_motivo_puro`: `app/ponto/turnos.py:2097`–`:2105`; carregador publica em `:2139` | calendário `app/colaboradores/services/calendario.py:714`, `:722` |
| motivo de retenção | `app/folha/export.py:1200`, `:1237` | calendário `app/colaboradores/services/calendario.py:727`, `:834`, `:899` |
| `futuro` | calendário `app/colaboradores/services/calendario.py:822` | `_classificar` `:135`, `:155` |
| `status` | `_classificar` retorna `app/colaboradores/services/calendario.py:189`; contexto publica em `:898` | resumo `:949`; Resolver dia `:204`, `:945`; grade `app/templates/colaboradores/partials/_calendario_grade.html:69` |
| `status_rotulo` | `app/colaboradores/services/calendario.py:899` | grade `app/templates/colaboradores/partials/_calendario_grade.html:99`, `:105`, `:107` |

O novo fallback do template realmente lê `status_rotulo` (`app/templates/colaboradores/partials/_calendario_grade.html:107`), e a sentinela do teste evita que uma frase literal coincidente passe (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`). Não executei testes de banco, para preservar a exigência somente leitura. [não verificado]

## 30/10 18:12 · sessao 5c1f28a1ae15 · - · - · origem=Ronald (tmux)

Não confirmei bloqueador novo nos diffs de `servico_jornada.py`/`turnos.py`: a assinatura de `fim_turno` só recebe alimentação e repassa ao mesmo cálculo; o booleano público continua `_m is None`, e `motivos=True` apenas expõe a causa. (`app/escala/servico_jornada.py:89-93`; `app/ponto/turnos.py:2088-2105`; `app/ponto/turnos.py:2137-2139`)

- **Custo acrescentado por montagem do calendário:** uma consulta de vínculos; células e turnos já carregados são reutilizados, sem consulta de DNA por dia. (`app/colaboradores/services/calendario.py:712-713`; `app/ponto/turnos.py:2129-2138`; `app/escala/models.py:1094-1108`)
- **Raio global:** `dias_encerrados` também alimenta o cartório, o supra-juiz, o emissor retroativo e `par_completo_da_janela`. No cartório, o resultado entra na impressão de insumos e pode alterar o pulo/rejulgamento. (`app/ponto/services/cartorio.py:853`; `app/ponto/services/cartorio.py:1082-1092`; `app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/turnos.py:2150`)
- **Defeito herdado relevante:** quando `dias_encerrados` recebe `celulas={(colaborador_id, data): celula}` sem `turnos=`, repassa esse mapa diretamente a `turnos_do_colab`; este exige `{data: celula}` e reconstrói a chave como `(_cid, _d)`, produzindo chaves aninhadas. Isso pode fazer o pareamento perder a geradora declarada pela célula. O cartório usa justamente esse caminho. (`app/ponto/turnos.py:2113`; `app/ponto/turnos.py:2132`; `app/ponto/turnos.py:1852`; `app/ponto/turnos.py:1928-1932`; `app/ponto/services/cartorio.py:853`) **Já existia na base**, conforme comparação somente leitura. [não verificado por execução]
- **Novo teste morde o repasse do DNA e a confusão entre contratos plural/singular:** DNA `10:00–14:00` contra template `19:00–07:00`, consultado às 15h. Contudo, não cria batidas e não testa `turnos=`, portanto não cobre o defeito de alimentação do pareamento acima. (`app/ponto/tests/test_c1_dias_encerrados.py:62-77`)

Escritores do DNA, `trabalha` e `escala_geradora`: geração/regeneração pela porta; leitores relevantes: `marcos_do_dia`, `vinculo_do_dia`, `fim_turno` e o juiz de encerramento. (`app/ponto/portas/celula.py:243-265`; `app/ponto/portas/celula.py:595-614`; `app/escala/models.py:1094-1108`; `app/escala/alimentacao.py:237-254`; `app/escala/servico_jornada.py:68-93`; `app/ponto/turnos.py:2095-2105`)

## 30/10 18:12 · sessao d0172544555c · - · - · origem=Ronald (tmux)

Não encontrei juiz novo no diff de autoridade: os motivos continuam saindo de `_dia_encerrado_motivo_puro`, e o lote apenas expõe motivo ou booleano (`app/ponto/turnos.py:2088`, `:2137-2139`).

Há uma falha relevante **preexistente ainda presente**: `dias_encerrados` declara `celulas={(colaborador_id,data):celula}` (`app/ponto/turnos.py:2113`) e repassa o mapa cru para `turnos_do_colab` (`:2132`), cujo contrato é `{data:celula}` (`:1852`). Este monta novamente a chave composta (`:1928`), produzindo chave aninhada; `vinculo_do_dia` procura a chave simples composta (`app/escala/alimentacao.py:237`). O cartório alimenta exatamente o formato composto (`app/ponto/services/cartorio.py:853`). O BACKLOG já registra a dívida (`app/docs/BACKLOG.md:790`).

O novo teste de alimentação não morde esse defeito: cria célula sem `escala_geradora` (`app/ponto/tests/test_c1_dias_encerrados.py:62-63`) e, portanto, não confronta a geradora declarada com outro vínculo concorrente; compara a célula persistida com a mesma célula do banco (`:64-74`).

Escritores e leitores principais:

- `CelulaDia.veredito/protestos`: escritor canônico `lavrar_veredito` (`app/ponto/portas/celula.py:647-649`, `:691-697`); cartório chama em `app/ponto/services/cartorio.py:981`, `:1409`; folha lê em `app/folha/export.py:1215-1238`; calendário usa ausência de veredito em `app/colaboradores/services/calendario.py:723`.
- `dna/trabalha/escala_geradora`: geração escreve em `app/ponto/portas/celula.py:595-614`; regeneração em `:247-260`; DNA é lido por `EscalaColaborador.marcos_do_dia` (`app/escala/models.py:1094-1108`), através de `fim_turno` (`app/escala/servico_jornada.py:69`, `:92`) e do juiz (`app/ponto/turnos.py:2102`).
- `Turno.data_turno/aberto`: `_mk` produz (`app/ponto/turnos.py:316-322`); `turno_aberto_vivo` lê (`:1987-1993`); o juiz usa em `:2097-2101`.

Relógios:

- Calendário captura um instante e deriva dele a data local (`app/colaboradores/services/calendario.py:258-259`), passa o instante ao juiz (`:712`) e a data à folha (`:726`).
- Juiz confronta `agora >= fim` (`app/ponto/turnos.py:2105`); aberta expira por janela do template acrescida de quatro horas (`:1997-2011`), enquanto fim previsto lê DNA (`:2102`; distinção herdada declarada em `:2039-2045`).
- Folha ainda usa o dia civil, `d < hoje` (`app/folha/export.py:1213`, `:1237`).
- Fuso configurado: `America/Sao_Paulo`, `USE_TZ=True` (`app/config/settings/base.py:93-95`).

Custo adicional estático por montagem: uma consulta obrigatória de vínculos (`app/ponto/turnos.py:2130`); turnos e células fornecidos evitam novas cargas (`:2129`, `:2132`, `:2138`). Havendo `_sem_fim`, uma chamada à folha (`app/colaboradores/services/calendario.py:725-727`), normalmente duas consultas (`app/folha/export.py:1188`, `:1194`), ou nenhuma nos retornos antecipados de isento/janela colapsada (`:1174-1187`). O teste mede somente esse delta e limita a duas consultas (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:169-183`); custo absoluto da montagem e duração: [não verificado].

A API singular continua buscando vínculo/DNA no banco mesmo quando recebe células alimentadas (`app/ponto/turnos.py:2079-2080`). O teste novo compara alimentação com o mesmo dado persistido, sem enfrentar divergência entre alimentação e banco (`app/ponto/tests/test_c1_dias_encerrados.py:62-74`). Execução da suíte: [não verificado]; não rodei fixtures que escrevem banco, respeitando somente leitura.

## 30/10 18:12 · sessao d4daaabd58b0 · - · - · origem=Ronald (tmux)

Auditoria estática dos testes, sem executar Django nem escrever arquivos. Resultado de execução/RED contra base/mutação real: [não verificado]. [nao verificado]

- **Os casos centrais têm asserções discriminantes:** hoje antes do turno, virada da meia-noite, turno encadeado até dois dias depois e falta após fim previsto; há também veredito já lavrado contra o “previsto”. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:65`.
- **O complemento das 15:3x tem caso correto com vínculo e célula trabalhando sem `hf`.** Ele verifica simultaneamente folha=`sem_celula`, tela=`sem_certidao`, motivo no rótulo e renderização nas três disposições. A sentinela verifica leitura efetiva de `status_rotulo`, evitando texto literal coincidente. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:82`, `:91`, `:97`, `:104`, `:108`.
- **Precedências estão cobertas parcialmente:** veredito lavrado, folga humana, ausência pendente e isento. O teste da ausência declara corretamente que não distingue cobrança por todas as ausências de cobrança só pelas aprovadas: a A2 decide antes. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:112`, `:123`, `:132`, `:146`.
- **O teste de custo não certifica custo total por montagem.** Subtrai uma montagem igualmente afetada pela mudança: overhead comum desaparece. A query de escalas sempre executada por `dias_encerrados` fica fora desse teto; mede-se apenas acréscimo do ramo cego, com tolerância de duas queries. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:169`, `:177`, `:183`; `app/ponto/turnos.py:2130`.
- **Não há mordida demonstrada da nova alimentação `turnos=` com lista não vazia.** A fixture nova de calendário não cria batidas; o novo cenário C1 alimenta células, sem criar batidas. As propriedades C1 anteriores carregam turnos internamente, sem passar `turnos=`. Assim, esta bateria nova não certifica reaproveitamento de turnos nem impede recarga indevida nesse caminho. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19`, `:42`; `app/ponto/tests/test_c1_dias_encerrados.py:26`, `:53`; `app/ponto/turnos.py:2132`.
- **O novo teste C1 certifica DNA diferente do template e os dois formatos de alimentação para a porta singular**, com um instante adversarial 15h entre fim do DNA às 14h e turno do template 19–07h. Referências: `app/ponto/tests/test_c1_dias_encerrados.py:61`, `:66`, `:73`, `:77`.
- **Limites de interface:** render sentinela só exercita o ramo sem palavra/ausência. O template prioriza ausência e `palavra_dia` antes do ramo de `status_rotulo`. Também não há caso novo sem `hf` com régua suspeita, que precede a retenção no classificador. Isso é lacuna de cobertura; violação desses ramos ao aval depende da interpretação literal que o pai está auditando. Referências: `app/templates/colaboradores/partials/_calendario_grade.html:97`, `:98`, `:107`; `app/colaboradores/services/calendario.py:139`, `:141`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:91`.

Não afirmaria “testes não mordem” em bloco: as asserções dos casos principais são fortes, mas **custo total, alimentação de turnos e certificação de execução permanecem sem prova nesta leitura**. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:43`, `:92`, `:177`; execução: [não verificado].

## 30/10 18:12 · sessao f9c2b7bc127d · - · - · origem=Ronald (tmux)

Os testes novos cobrem os casos centrais sem mocks: vínculo real, antes do turno, meia-noite, turno encadeado de dois dias, depois do fim, veredito já lavrado, dia sem fim previsto e comparação com a folha. A fixture antiga sem vínculo foi corrigida. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:29`, `:42`, `:47`, `:57`, `:61`, `:65`, `:82`, `:92`.

Mordem também a ligação do rótulo ao template: os três layouts recebem uma sentinela em `status_rotulo`, impedindo um `title` literal de passar. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104`.

Lacunas concretas:

- **Alimentação de turnos não vazios sem prova direta.** A fixture nova e seus casos não criam batidas; C1 cria batidas, mas suas chamadas nunca passam `turnos=`. Portanto não testam diretamente a nova entrada de turnos não vazios usada pelo calendário. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20`; `app/ponto/tests/test_c1_dias_encerrados.py:28`, `:35`, `:51`, `:66`, `:77`; `app/colaboradores/services/calendario.py:712`.
- **Teste de cobertura não discrimina a cobertura da cobrança.** A própria documentação admite que A2 decide antes e deixa passar o mutante “cobrança só considera aprovadas”. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:134`.
- **Custo comum não preso pelo teste novo.** Ele compara apenas o acréscimo de um e oito dias cegos contra uma base; trabalho extra igual nos três cenários passa. O teto geral continua em 55 queries, medindo o segundo acesso aquecido. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/core/tests/test_selo_performance.py:249`, `:332`, `:401`.

Custo estrutural acrescentado por montagem:

- Sempre **+1 SELECT de vínculos**; turnos e células já carregados são reaproveitados. `app/ponto/turnos.py:2129`, `:2130`, `:2132`.
- Havendo passado `sem_previsto` sem veredito, uma chamada adicional ao juiz da folha, com SELECT de células e carga de vínculos; não ocorre por dia. `app/colaboradores/services/calendario.py:722`; `app/folha/export.py:1188`, `:1194`.
- CPU cresce com dias × vínculos, além dos turnos: o juiz percorre os dias e filtra candidatos por dia. `app/ponto/turnos.py:2132`; `app/escala/alimentacao.py:253`.

A suspeita inicial de divergência pela ordenação dos vínculos **não procede**: `vinculo_do_dia` escolhe por `max(ativa, data_inicio)`. `app/escala/alimentacao.py:254`.

Execução e resultado verde dos testes nesta auditoria: **[não verificado]**; realizei somente leitura. [nao verificado]

## 30/10 18:12 · sessao 54a8d86a7323 · - · - · origem=Ronald (tmux)

Os testes cobrem os casos funcionais principais: hoje antes do turno, madrugada seguinte, fim já vencido, turno encadeado além de ontem e veredito lavrado (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42-76`). O caso cego confirma vínculo/trabalho, motivo real `sem_celula`, status `sem_certidao` e leitura de `status_rotulo` nos três layouts, inclusive por sentinela (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:92-110`).

Há três limitações concretas:

- **O teste não morde a dependência do juiz da folha.** As asserções esperam sempre o motivo fixo `sem_celula`, enquanto a sentinela verifica apenas que o template lê o campo (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97-110`). Por inspeção, substituir a chamada à folha por `_cego_folha = {d: 'sem_celula' for d in _sem_fim}` preservaria esses resultados; o custo cairia a zero e também satisfaria `<=2`. Trata-se de conclusão estática, mutante não executado (`app/colaboradores/services/calendario.py:722-727`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177-183`).

- **“Custo 0 no caso comum” vale apenas para a retenção adicional, não para o diff completo.** O calendário sempre chama `dias_encerrados`, que sempre consulta todos os vínculos, mesmo recebendo turnos e células previamente carregados (`app/colaboradores/services/calendario.py:712-713`; `app/ponto/turnos.py:2129-2132`). O teste desconta sua própria base e, portanto, não captura esse custo incondicional de uma consulta por montagem; mede somente o acréscimo do ramo cego (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:169-183`). A retenção acrescenta uma consulta de células e uma carga de vínculos (`app/folha/export.py:1188-1194`).

- **O rótulo não vem integralmente do juiz da folha.** A folha devolve `data`, código `motivo`, `celula_id` e `veredito`; a frase é inventada localmente em `ROTULO_CEGO_FOLHA` e montada por `contexto_calendario` (`app/folha/export.py:1200-1204`; `app/colaboradores/services/calendario.py:64`; `app/colaboradores/services/calendario.py:899-901`). O template lê corretamente esse campo no ramo residual (`app/templates/colaboradores/partials/_calendario_grade.html:107`). A exigência literal do aval é “a tela lê o rótulo do mesmo juiz da folha” (`app/docs/PROMPTS.md:1681`).

A cobertura pendente é explicitamente um selo de precedência da A2, não um RED do novo ramo de retenção; o próprio teste documenta essa limitação (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133-144`).

Não executei testes porque usam `TestCase`, criam registros e atualizam células, incompatível com a instrução somente leitura (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:17-30`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:165-173`).

## 30/10 18:12 · sessao b602562ce5c4 · - · - · origem=Ronald (tmux)

Avais literais: `app/docs/PROMPTS.md:1676` (13:3x) e `:1681` (15:3x). O histórico de reprovação A1–A5 está em `app/docs/RELATO.md:8`; ele descreve árvore anterior, portanto não prova defeito remanescente no hash atual.

Os testes novos mordem os principais casos solicitados: [nao verificado]

- Antes do turno: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`; meia-noite: `:57`; turno estendido além de ontem: `:47`; falta após fim: `:61`.
- Passado sem fim/veredito: vínculo real criado em `:29`, premissa `sem_previsto` verificada em `:85`, retenção real da folha verificada em `:97`, status e motivo em `:99`.
- Veredito lavrado vence em `:112`; folga lançada em `:123`; ausência pendente em `:132`; isento passado em `:146`.
- O template é efetivamente renderizado nos três layouts e recebe sentinela no campo `status_rotulo`: `:104–110`. Isso impede teste verde por frase literal independente do campo. [nao verificado]
- Custo compara 0/1/8 dias cegos, exige quantidade real de `sem_certidao`, igualdade do acréscimo e teto de duas queries: `:169–183`. **Não mede custo total da montagem nem custo temporal**; captura apenas a diferença em consultas causada pelo ramo cego.
- O teste de ausência admite corretamente sua limitação: A2 decide antes da cobertura, logo não certifica o universo de cobrança: `:134–138`. [nao verificado]

Há lacunas de cobertura, sem constituírem isoladamente bug comprovado:

- A nova alimentação `turnos=` não é exercitada pelos testes C1: `app/ponto/tests/test_c1_dias_encerrados.py:28`, `:66`, `:77`; a implementação aceita esse caminho em `app/ponto/turnos.py:2132`. Não há prova nova para divergência de pareamento entre janelas.
- A propriedade janela/singular compartilha o mesmo núcleo, `app/ponto/turnos.py:2079`, `:2137`. A bateria independente da regra permanece em `app/ponto/tests/test_juiz_dia_encerrado.py:78–158`; não confundir equivalência entre carregadores com certificação da regra.
- O dia da demissão, explicitamente citado na reprovação anterior (`app/docs/RELATO.md:8`), não tem caso novo; o retentor o excepciona em `app/folha/export.py:1211`, `:1237`.
- Não há assert novo para efeito sobre `resolver_dia_oferecido`; esse juiz lê o status em `app/colaboradores/services/calendario.py:204`. Os novos testes verificam status, rótulo, precedência e custo, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42–183`.

Não executei testes, respeitando somente leitura. Resultado verde/vermelho desta árvore: **[nao verificado]**. [nao verificado]

## 30/10 17:43 · sessao f9f2f71f3459 · CALENDARIO-HOJE · verificador · origem=Code

**REPROVADO para certificar a volta 7:** o teste de alimentação não cobre a correção singular que descreve: chama a singular sem `celulas`. Há também incompatibilidades de dict na cópia, mas atribuí-las ao último commit seria indevido. (`app/ponto/tests/test_c1_dias_encerrados.py:56`, `app/ponto/tests/test_c1_dias_encerrados.py:70`; introdução no delta: [não verificado])

A cópia não contém histórico Git utilizável; não consegui executar `git diff 6c37ad84 HEAD` nem `git show HEAD`. Portanto, igualdade de resultados com o main e identificação exata dos testes alterados no último commit são **[não verificado]**.

1. **Chamadores e diferenças de resultado**

Para **cada linha abaixo**, a comparação histórica com `6c37ad84` permanece **[não verificado]**. Estes são os caminhos presentes na cópia:

| Chamador | Entrada atual |
|---|---|
| Fechamento → `dia_encerrado` | Dict `{data: célula}`, recortado pelo pad. (`app/ponto/services/fechamento.py:354`, `app/ponto/services/fechamento.py:361`) |
| Vigia de hora → `dia_encerrado` | Dict `{(colab,data): célula}`. (`app/ponto/services/vigia_de_hora.py:57`) |
| Flip automático → `dia_encerrado` | Sem `celulas`. (`app/ponto/services/flip_auto.py:285`) |
| Detector de ausências → `dia_encerrado` | Sem `celulas`. (`app/ponto/management/commands/detectar_ausencias.py:473`) |
| Reconciliador → `dia_encerrado_motivo` | Sem `celulas`. (`app/chamados/reconciliador.py:1041`) |
| Cartório → `dias_encerrados` | Dict `{(colab,data): célula}`, sem `turnos`. (`app/ponto/services/cartorio.py:853`) |
| Comando supra juiz → `dias_encerrados` | Sem alimentação. (`app/ponto/management/commands/supra_juiz.py:108`) |
| Emissor retroativo → `dias_encerrados` | Sem alimentação. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`) |
| Calendário → `dias_encerrados` | Dict `{(colab,data): célula}` **e** turnos previamente carregados. (`app/colaboradores/services/calendario.py:712`, `app/colaboradores/services/calendario.py:713`) |
| `par_completo_da_janela` → `dias_encerrados` | Sem alimentação; o retorno filtra os dias admitidos pelo juiz de par completo. (`app/ponto/turnos.py:2150`, `app/ponto/turnos.py:2153`) |

A entrada que discrimina a correção singular é: **DNA 10–14, template 19–07, sem batidas, consulta às 15h**. A singular atual consulta os marcos separadamente e encerra; reintroduzir o repasse do seu dict `{data}` à parte pura faz a busca por `(colab,data)` falhar e usar o template, retornando `relogio`/`False`. Isso demonstra o efeito da mutação, não uma diferença comprovada contra o main. (`app/ponto/turnos.py:2079`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1095`, `app/escala/models.py:1097`, `app/escala/models.py:1108`; comparação histórica: [não verificado])

2. **Contratos de `celulas`**

| Função | Forma exigida e encaminhamento |
|---|---|
| `dias_encerrados` | `{(colab,data): célula}`; correto para vínculo e julgamento, **errado no repasse ao pareador quando falta `turnos=`**. (`app/ponto/turnos.py:2113`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2137`) |
| `_dia_encerrado_motivo_puro` | `{(colab,data): célula}`; encaminha ao cálculo do fim. (`app/ponto/turnos.py:2094`, `app/ponto/turnos.py:2102`) |
| `fim_turno` | Mesmo contrato de `marcos_turno`; encaminhamento correto. (`app/escala/servico_jornada.py:90`, `app/escala/servico_jornada.py:92`) |
| `marcos_turno` | Mesmo contrato de `ec.marcos_do_dia`: `{(colab,data): célula}`. (`app/escala/servico_jornada.py:67`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1083`) |
| `turnos_do_colab` | `{data: célula}`; converte internamente para chave composta. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928`) |
| `vinculo_do_dia` | `{(colab,data): célula}`; procura exatamente essa chave. (`app/escala/alimentacao.py:209`, `app/escala/alimentacao.py:237`) |

**Achado procedente:** a plural passa o mapa composto diretamente a `turnos_do_colab`. A conversão interna produz `(colab,(colab,data))`; o resolvedor perde a geradora da célula e pode escolher outro vínculo pelo fallback. A chamada do cartório e o teste C1 alimentado percorrem esse caminho; o calendário o evita fornecendo `turnos=`. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:1928`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:254`, `app/ponto/services/cartorio.py:853`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/colaboradores/services/calendario.py:713`)

Esse defeito do cartório está explicitamente registrado como **preexistente**; não o considero regressão comprovada desta volta. (`app/docs/BACKLOG.md:790`; comprovação pelo histórico Git: [não verificado])

Há ainda forma incompatível na vigia: ela entrega chave composta à singular, que a repassa ao pareador de chave simples. Ocorrência e efeito em produção: **[não verificado]**. (`app/ponto/services/vigia_de_hora.py:57`, `app/ponto/turnos.py:2079`, `app/ponto/turnos.py:1852`)

3. **Os testes mordem?** [nao verificado]

O alcance abaixo decorre da leitura e de sondas em memória das funções; execução da suíte e dos mutantes completos: **[não verificado]**. [nao verificado]

| Teste | Mutação que pega | Mutação que deixa passar | [nao verificado]
|---|---|---|
| C1 alimentado | Forçar template em lugar do DNA: às 15h exige encerramento pelo DNA 10–14. | **Reintroduzir o repasse singular indevido**, pois a singular é chamada sem alimentação. (`app/ponto/tests/test_c1_dias_encerrados.py:63`, `app/ponto/tests/test_c1_dias_encerrados.py:70`, `app/ponto/tests/test_c1_dias_encerrados.py:73`) |
| Mesmo C1 | Passar `{}` aos marcos, causando ausência da chave e fallback ao template. | **Remover o encaminhamento de `celulas`**: com `None`, o método consulta a célula persistida e encontra o mesmo DNA. (`app/escala/models.py:1094`, `app/escala/models.py:1097`, `app/escala/models.py:1108`, `app/ponto/tests/test_c1_dias_encerrados.py:62`) |
| Mesmo C1 | Verifica o resultado temporal do DNA. | Perder a geradora durante o pareamento: a fixture não declara geradora nem cria batidas. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`) |
| Ausência pendente | A2 deixar de reconhecer a pendente abonadora. | Restringir a cobertura de cobrança a aprovadas: A2 já retorna `ausencia` antes da cobertura. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:138`, `app/colaboradores/services/calendario.py:124`) |
| Custo | Crescimento entre um e oito dias cegos; incremento superior a duas queries. | Consultas adicionais comuns aos cenários normal e cego, pois a base é subtraída. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:179`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:183`) |
| Render com sentinela | `title` ausente ou substituído por frase fixa. | Só exige a substring no HTML; não verifica sua localização. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:105`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`) |

Para selar especificamente a volta 7, falta exercer a **singular alimentada com `{data: célula}`**, com DNA diferente do template, e exigir encerramento às 15h. O teste atual descreve essa proteção, mas chama outra entrada. (`app/ponto/tests/test_c1_dias_encerrados.py:56`, `app/ponto/tests/test_c1_dias_encerrados.py:63`, `app/ponto/tests/test_c1_dias_encerrados.py:70`)

## 30/10 17:42 · sessao 659bb6fb3747 · - · - · origem=Ronald (tmux)

Correção confirmada: o teste C1 **não detecta a remoção do repasse de `celulas`** em `turnos.py:2138`, `turnos.py:2102`, `servico_jornada.py:92` ou `servico_jornada.py:69`. Sem alimentação, `marcos_do_dia` consulta a célula persistida e encontra o mesmo DNA criado pelo teste. (`app/escala/models.py:1094`, `app/escala/models.py:1097`, `app/escala/models.py:1099`; `app/ponto/tests/test_c1_dias_encerrados.py:62`)

Ele detecta **forçar a leitura do template** — por exemplo, omitir `ec` ou passar `{}` como alimentação dos marcos — porque às 15h DNA 10–14 encerrou e template 19–07 não. (`app/escala/servico_jornada.py:68`, `app/escala/servico_jornada.py:71`; `app/escala/models.py:1095`, `app/escala/models.py:1108`; `app/ponto/tests/test_c1_dias_encerrados.py:73`)

Também **não detecta reintroduzir o repasse singular indevido**: a comparação chama singular sem `celulas`. (`app/ponto/tests/test_c1_dias_encerrados.py:70`)

O teste de custo limita somente o **incremento do ramo cego** a duas consultas e compara um versus oito dias; consultas adicionais comuns à base escapam pela subtração. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:179`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:183`)

Mutantes e suíte não foram executados; o alcance acima foi estabelecido por leitura do código. [não verificado quanto à execução] [nao verificado]

## 30/10 17:42 · sessao e49045cf3803 · - · - · origem=Ronald (tmux)

Achado procedente de contrato na porta **plural**: `dias_encerrados` recebe `{(cid,data): CelulaDia}`, mas, sem `turnos=`, repassa esse mapa diretamente a `turnos_do_colab`, que exige `{data: CelulaDia}`. — `app/ponto/turnos.py:2113`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:1852`.

| Função | Forma exigida e caminho atual |
|---|---|
| `dias_encerrados` | `{(cid,data): cel}`; preserva corretamente para vínculo e julgamento, mas repassa erradamente ao carregador de turnos. — `app/ponto/turnos.py:2129`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2137` |
| `_dia_encerrado_motivo_puro` | `{(cid,data): cel}`, repassado a `fim_turno`. — `app/ponto/turnos.py:2094`, `app/ponto/turnos.py:2102` |
| `fim_turno` | Mesmo contrato de `marcos_turno`; encaminhamento correto. — `app/escala/servico_jornada.py:89`, `app/escala/servico_jornada.py:92` |
| `marcos_turno` | Mesmo contrato de `ec.marcos_do_dia`: `{(cid,data): cel}`. — `app/escala/servico_jornada.py:67`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1083`, `app/escala/models.py:1095` |
| `turnos_do_colab` | `{data: cel}`; transforma em `{(cid,data): cel}` para resolver o vínculo. — `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928` |
| `vinculo_do_dia` | `{(cid,data): cel}`; procura exatamente essa chave. — `app/escala/alimentacao.py:209`, `app/escala/alimentacao.py:237` |

A forma errada produz `{(cid,(cid,data)): cel}` após a transformação de `turnos_do_colab`; a geradora desaparece para o resolvedor, que passa a escolher o vínculo por vigência, atividade e início. — `app/ponto/turnos.py:1928`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:253`.

Chamadas concretas atingidas: cartório fornece o mapa plural sem `turnos=`, assim como o novo teste de propriedade alimentada. O calendário fornece `turnos=` e evita esse ramo. — `app/ponto/services/cartorio.py:853`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`, `app/colaboradores/services/calendario.py:712`.

A singular está coerente: seu mapa `{data: cel}` vai somente a `turnos_do_colab`; vínculo e marcos são carregados separadamente. — `app/ponto/turnos.py:2052`, `app/ponto/turnos.py:2079`.

O novo teste alimentado não detecta o erro plural: cria célula **sem `escala_geradora`**, não cria batidas e compara o encerramento ditado pelo DNA; portanto, não distingue perder a geradora durante o pareamento. Ele detecta deixar de fornecer o DNA ao julgamento plural, pois às 15h exige encerramento pelo DNA 10–14, enquanto o template é 19–07. — `app/ponto/tests/test_c1_dias_encerrados.py:58`, `app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:73`, `app/ponto/tests/test_juiz_dia_encerrado.py:49`.

Verificação somente em memória: extraí por AST as funções reais de escolha de vínculo e confirmei que, com célula nomeando A e outro vínculo B ativo mais recente, o mapa correto escolhe A e o mapa duplicado escolhe B. O teste de integração desse cenário não foi executado. — `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:254`; execução de integração [não verificado].

A introdução desse defeito especificamente no último commit ou em relação ao main não pode ser comprovada pela cópia sem histórico Git. [não verificado] [nao verificado]

_679 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
