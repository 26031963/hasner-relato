# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 20:51** · publicado em **05/11/2026 20:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 20:51 · sessao 35b27ecbba74 · - · - · origem=Ronald (tmux)

Complemento dos commands solicitados:

| Caminho | Atomic e escritas comprovadas |
|---|---|
| `marcar_abandono_provisorio.handle` | Atomic cobre **o comando inteiro** em `app/ponto/management/commands/marcar_abandono_provisorio.py:22`; User `get_or_create` na linha 30 → loop de colaboradores nas linhas 35/43 → loop de dias na linha 70 → `garantir_ausencia` na linha 76. Signals síncronos de Ausencia entram nessa transação: `app/ponto/signals.py:143`/147. Sem `--apply`, rollback explícito na linha 98; com `--apply`, não há esse rollback. Ordem dos colaboradores não traz `order_by` nesse queryset, linha 35. |
| `escalonar_chamados_supervisao`, N2 | Atomic `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`: `escalar_urgencia` linha 132 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria por `models.py:524`/`app/core/observ.py:134` → `anotar_contexto` na linha 138 → Chamado SAVE `app/chamados/services/contexto.py:77`. N3 escreve contexto de Disputa na linha 159 e de Chamado na linha 163 **fora desse atomic**. |
| `triagem_batida.ramo_solicitar_anuencia` | Atomic `app/ponto/services/triagem_batida.py:368`: escala linha 369 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria via linha 524 → renova contexto na linha 371 → Chamado SAVE `app/chamados/services/contexto.py:102`. Followup é anexado na linha 351, antes desse bloco. |
| `curar_residuo_bug115._ambiguo` | Atomic `app/ponto/management/commands/curar_residuo_bug115.py:120`: renovar contexto linha 121 → Chamado SAVE `app/chamados/services/contexto.py:102` → LogAuditoria via comando linha 124 e `app/core/observ.py:134`. Outros ramos geram/julgam célula nas linhas 135/143, fora desse atomic. |
| `reconciliar_grade._relavrar_orfa` | Atomic `app/ponto/management/commands/reconciliar_grade.py:472`: renovar contexto linha 473 → Chamado SAVE `app/chamados/services/contexto.py:102`; retirar contexto linha 474 → Chamado SAVE `contexto.py:124`. Trilha `evento_kw` na linha 477 ocorre depois do bloco. |
| `lavrar_placar_situacional.handle` | Atomic `app/core/management/commands/lavrar_placar_situacional.py:101`: loop PlacarSituacional `update_or_create` linha 105 → DELETE de PlacarSituacional linha 110. Empresa apenas consultada na linha 100. Não aparece segunda entidade escrita nesse bloco. |

Dois comandos são barrados em produção pelo código:

- `zerar_dados_operacionais`: exige ensaio em `app/ponto/management/commands/zerar_dados_operacionais.py:27`; a guarda recusa se `BANCO_DE_ENSAIO` não estiver ativo **ou** o banco for `saas_hasner`, em `app/ponto/registro_batida.py:326`. No atomic da linha 80, sequência das chamadas explícitas: Followup DELETE linha 83 → Chamado DELETE linha 84 → AlertaGeofence DELETE linha 85 → AlertaTurno DELETE linha 87 → PushSubscription DELETE linha 88 → Ausencia DELETE linha 90 → FechamentoMensal DELETE linha 93 → Justificativa DELETE linha 95 → Batida DELETE linha 96 → ImportacaoAEJ DELETE linha 97 → PingGeo DELETE linha 98. Deleções reais nos helpers: `app/chamados/services/nota.py:141`, `app/chamados/models.py:539`, `app/ponto/services/alerta_turno.py:32`, `app/ponto/services/ausencia.py:1274`, `app/ponto/services/fechamento.py:1752`, `app/ponto/registro_batida.py:345`. Signals de Ausencia/Batida podem intercalar escritas nessas chamadas; ordem SQL interna das cascatas [não verificado].
- `gerar_batidas_smoke_motor`: guarda de ensaio em `app/ponto/management/commands/gerar_batidas_smoke_motor.py:45`, mais exigência de host `juliani_db_test` nas linhas 47/48. Atomic da linha 106 chama `semear` na linha 107 → Batida `bulk_create` em `app/ponto/registro_batida.py:335` → trilha na linha 336. Não é caminho executável no banco `saas_hasner` pela guarda citada.

A referência `app/chamados/models.py:849` afirma, em comentário, ausência de atomic envolvendo laços sobre colaboradores; o comando `marcar_abandono_provisorio` efetivamente tem esse formato nas linhas 22,35,43,76. Para a análise de ordem, usar o fluxo executável do comando.

## 05/11 20:50 · sessao e492c32c6d22 · - · - · origem=Ronald (tmux)

Censo ponto/escala, somente por leitura do código; `I` abaixo significa INSERT, `U` UPDATE/save de existente, `D` DELETE. Não executei banco, Django nem comandos da aplicação. Ordem SQL interna e frequência de execução em produção: [não verificado]. [nao verificado]

**`select_for_update` encontrado nestes domínios**

| Arquivo:linha | Função | Modelo | Argumentos | Avaliação |
|---|---|---|---|---|
| `app/ponto/portas/he.py:174` | `decidir_he` | DecisaoHE | `()` | `.first()`; atomic decorator `:137` |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | DecisaoHE | `()` | `.first()`; atomic `:268` |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | DecisaoHE | `()` | `.first()`; atomic `:392` |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | DecisaoHE | `()` | `.first()` `:556`; atomic por item `:548` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa | `()` | `.get()`; atomic `:1793`; outro explorer cobre expansão |

`autorizar_em_lote` volta a chamar `decidir_he` depois da primeira trava (`app/ponto/portas/he.py:562`), portanto toma a mesma entidade novamente (`:174`), dentro do mesmo atomic do dia (`:548`).

**Advisory**

`app/ponto/services/vigia_de_hora.py:79`, `rodar`: chave `7180000 + int(empresa)` (`:97`), `SELECT pg_try_advisory_lock(%s)` (`:99`), retorna sem processar se ocupado (`:100`), julga células enquanto segura a trava (`:131`), libera em finally com `pg_advisory_unlock` (`:136`). Não há `atomic` abrangendo o corpo dessa função (`:98`, `:102`, `:134`); o advisory abrange várias transações das portas chamadas, não deve virar uma transação única.

**Escritas existentes sob atomic direto ou por porta chamada**

| Modelo | Escrita | Atomic/caminho |
|---|---|---|
| CelulaDia | `portas/celula.py:239`, `:270` U | `regenerar_celulas_vinculo`, atomic `:212` |
| CelulaDia | `portas/celula.py:578` U | `reverter_regeneracao_dia`, atomic `:569` |
| CelulaDia | `portas/celula.py:682` U | `salvar_e_emitir`, atomic `:681` |
| CelulaDia | `portas/celula.py:645`, `:655` U | `gerar_celulas_periodo`; comando `management/commands/gerar_celulas.py:310` atomic, chamada `:311` |
| CelulaDia | `portas/celula.py:885` bulk U | `carimbar_insumos_em_lote`; não possui atomic próprio (`:868`, `:885`); transação depende do chamador [não verificado neste censo] |
| FuroDiario | `portas/celula.py:906` update_or_create, `:914` D | `gravar_furos`, atomic `:904` |
| DecisaoHE | `portas/he.py:84` U; criação `:66` | `_gravar`, chamado por `decidir_he:181`/`recusar_ponta_pequena:341` sob decorators `:137`/`:268` |
| DecisaoHE | `portas/he.py:375` U | `_marcar_revogada`, chamada `:472` sob decorator `:392` |
| Justificativa | `services/regularizacao.py:106` U | `regularizar_turno_orfao`, atomic `:89` |
| Batida | `registro_batida.py:254` U | `anotar`, quando chamado pelo atomic externo: `veredito_celula.py:204`, flip `:295`; comando `flip_automatico.py:65`, chamada `:68`; outros callers ficam com raiz |
| FechamentoMensal | `services/fechamento.py:1640` U | `invalidar_fechamento`, atomic `:1635` |
| FechamentoMensal | `services/fechamento.py:1597` U | `invalidar_previsto`, quando receiver de EscalaColaborador é executado em atomic do emissor (`escala/signals.py:124`, `:133`) |
| FechamentoMensal | `services/fechamento.py:1128` U/save, `:1144` U | `recalcular_fechamento_mes` SEM atomic próprio; externos listados adiante |
| DiaPago | `services/dia_pago.py:536` bulk U, `:540` U, `:543` I | `retratar_exportada`, atomic `:532` |
| DiaPago | `services/dia_pago.py:669`, `:671` U | `apurar_furo_da_exportada`, atomic `:667` |
| DiaPago | `services/dia_pago.py:357` D, `:358` bulk I | `lavrar`, sem atomic próprio; via recalcular sob atomic externo |
| FolgaDia | `escala/folgas.py:342` U / `:333` D | `_foto_do_dia`; chamados sob atomic de `alterar_folga:354`/`:356`, `aplicar_folgas_em_lote:382`/`:389` |
| FolgaDia | `escala/folgas.py:493` U | `reparentar_folga`, atomic `:490` |
| FolgaDia | `escala/folgas.py:183` D, `:184` bulk I | `_gerar_foto_folgas_no_ato`, atomic em `gerar_foto_folgas:162`, chamada `:163` |
| FolgaCalendario | `escala/folgas.py:560`, `:564`, `:569` save | `salvar_calendario_de_folga`, atomic `:555`; primeiro save pode ser I conforme `cal is None:556` |
| FolgaCalendarioTamanho | `escala/folgas.py:565` D, `:571` update_or_create | mesmo atomic `:555` |
| EscalaColaborador | `colaboradores/services/vinculo.py:160` U | `trocar_calendario_de_folga`, chamado pela view atomic `escala/views.py:407`, chamada `:409` |
| EscalaColaborador | `colaboradores/services/vinculo.py:615` U, `:1010` I | `ajustar_vinculo_pelo_sistema:1008`, chamado em `escala_auto_executor.py:103` sob atomic `:101` |
| EscalaColaborador | `colaboradores/services/vinculo.py:1020` D, `:1021` U; ou `:1023` fechar_vigencia e `:1025` I | `desfazer_ajuste_do_sistema`, chamado em `escala_auto_executor.py:142` sob atomic `:141` |

Os paths nessa tabela levam prefixo `app/`.

**Decorador deslocado que invalida leitura ingênua do grep**

`app/escala/services/cadastro_tipo.py:41` `@transaction.atomic` decora **`_hpd_pares` em `:43`**, helper puro. `salvar_tipo` começa em `:140` e salva em `:306`; não está protegido por esse decorator. `_propagar_regeneracao_template` começa em `:327`; suas portas têm atomics internos, mas não há atomic abrangente de `salvar_tipo` estabelecido por `:41`.

**Signals síncronos e entidades escritas**

| Emissor | Receiver | Efeito síncrono |
|---|---|---|
| Batida post_save | `app/ponto/signals.py:35` `_nucleo_post_save_batida` | `_recompute_around:26` → TurnoMaterializado D/I em `app/ponto/nucleo.py:58`, `:59`, SQL DELETE `:80`; EventoCelula I condicional `:66`→`:93`→`portas/celula.py:673` |
| Batida post_delete | `app/ponto/signals.py:49` | mesmo recompute via `:50` |
| EscalaColaborador post_save | `app/ponto/signals.py:73` | TurnoMaterializado D/I, EventoCelula I via `_recompute_escala:64` |
| Batida post_save | `app/ponto/signals.py:132` | cartório via `:135`→`:93`; escreve CelulaDia/EventoCelula, e condicionalmente chamados/disputas/perguntas pelo fluxo abaixo |
| Batida post_delete | `app/ponto/signals.py:139` | cartório via `:140` |
| Ausencia post_save | `app/ponto/signals.py:144` | cartório via `:147` |
| Ausencia post_delete | `app/ponto/signals.py:151` | cartório via `:152` |
| Justificativa post_save | `app/ponto/signals.py:162` | cartório via `:165` |
| Batida post_save | `app/ponto/signals.py:207` | `reconciliar_chamados_colab:215`; depois `fechar_adesao_com_batida:234`, entidades de chamados; raiz expande |
| EscalaColaborador post_save | `app/escala/signals.py:35` `_ec_vigencia_regenera_celulas` | regenera CelulaDia via `:71`, e marca ChamadoColaborador órfão via `portas/celula.py:283` |
| EscalaColaborador post_save | `app/escala/signals.py:80` `_ec_ancora_regenera_celulas` | mesma porta via `:117` |
| EscalaColaborador post_save | `app/escala/signals.py:125` `_ec_invalida_previsto` | FechamentoMensal U via `:133`→`services/fechamento.py:1597` |
| EscalaColaborador post_save | `app/escala/signals.py:143` `_ec_revalidar_disputas_ancora` | DisputaSupervisao/PerguntaDisputa e chamadas subsequentes pela porta `d.remover_motivo:161`; raiz expande |
| EscalaColaborador post_save | `app/escala/signals.py:173` `_ec_folgas_seguem_a_pessoa` | FolgaDia U via `:191`→`folgas.py:493`, depois CelulaDia/Chamado pela re-lavra `:500` |

`EscalaColaborador pre_save` `app/escala/signals.py:19` captura estado anterior; não escreve outra entidade (`:20` em diante).

**Fora da transação emissora por `on_commit`**

- Ausencia post_save/post_delete consumidor `app/ponto/signals.py:276`, `:283`: `_consumidor_ausencia` agenda callback em `:272`; dentro dele devolve perguntas (`:262`), reconcilia (`:269`) e fecha chamado (`:271`).
- Posto post_save geofence `app/ponto/signals.py:292`: reavaliar chamados `:306` é callback agendado em `:307`.
- Dinheiro de batida/HE/regeneração: `app/ponto/services/fechamento.py:1808` agenda recalcular; a chamada `:1794` ocorre no callback. Não juntar FechamentoMensal/DiaPago ao atomic original por essas chamadas.
- Regeneração agenda rejunção/falta espúria no callback `app/ponto/portas/celula.py:473`, funções em `:469`, `:472`.

**Catálogo de sequências para consolidação**

1. **PE01 — `salvar_e_emitir`**: atomic `app/ponto/portas/celula.py:681`; **CelulaDia U `:682` → EventoCelula I `:673`**, chamada `:684`. Segunda entidade é INSERT, não segunda trava comprovada de linha existente.

2. **PE02 — `recompute_turnos`**: atomic `app/ponto/nucleo.py:57`; **TurnoMaterializado D SQL `:80` → TurnoMaterializado I `:59` → EventoCelula I `portas/celula.py:673`**, quando `fechados` (`nucleo.py:65`), chamada `:66` e seleção ordenada por data `:92`. Não escreve CelulaDia; ela é somente consultada em `:92`.

3. **PE03 — `regenerar_celulas_vinculo`**: atomic `app/ponto/portas/celula.py:212`; **CelulaDia U `:239`/`:270` (ou I `:253`) → ChamadoColaborador U** via `_marcar_cobrancas_orfas:283` → `app/ponto/management/commands/gerar_celulas.py:256` `renovar_contexto`. Esse é caminho concreto **CelulaDia antes de Chamado**. Ordem das células é calendário crescente (`:214`, `:215`, `:275`); queryset de chamados órfãos não declara `.order_by` (`gerar_celulas.py:244`).

4. **PE04 — `alterar_folga`**: atomic `app/escala/folgas.py:354`; **FolgaDia U `:342`/D `:333` → PE03 CelulaDia → Chamado** pela chamada `:357`→`:272`. Pode realizar cartório depois, ainda no atomic externo.

5. **PE05 — `aplicar_folgas_em_lote`**: atomic `app/escala/folgas.py:382`; **FolgaDia U/D/I**, ordem da lista fornecida (`:384`, escrita `:389`) → **PE03 CelulaDia → Chamado** (`:395`→`:272`).

6. **PE06 — `gerar_foto_folgas`**: atomic `app/escala/folgas.py:162`; **FolgaDia D `:183` → FolgaDia I `:184` → PE03 CelulaDia → Chamado**, chamada re-lavra `:203`→`:272`.

7. **PE07 — `lancar_folga`**: atomic `app/escala/folgas.py:309`; **FolgaDia get_or_create `:312` → PE03 CelulaDia → Chamado** via `:314`. Se FolgaDia já existe, `get_or_create` não é save existente aqui; não marcar como trava U comprovada.

8. **PE08 — `reparentar_folga`**: atomic `app/escala/folgas.py:490`; **FolgaDia U `:493` → LogAuditoria I por `evento:494` → PE03 CelulaDia → Chamado** pela re-lavra `:500`. As folgas externas percorrem `.order_by('data')` `:515`.

9. **PE09 — calendário editor**: atomic `app/escala/folgas.py:555`; **FolgaCalendario save `:560`/`:564`/`:569` → FolgaCalendarioTamanho D `:565` ou update_or_create `:571`**. A ordem dos tamanhos é do dict recebido (`:570`), dependente do input.

10. **PE10 — view `aplicar_folga_calendario`**: atomic `app/escala/views.py:407`; **EscalaColaborador U** via `:409`→`colaboradores/services/vinculo.py:160`, **signals síncronos acima** → **PE06 FolgaDia/CelulaDia/Chamado** via `views.py:411`, `:412`, nova re-lavra `:416`. Ordem entre signal em `ponto.signals` e em `escala.signals` exige configuração de carregamento; não fixei cross-module.

11. **PE11 — `_escrever_folga` em `declarar_dia`**: atomic `app/ponto/services/declarar_dia.py:188`; **PE07 FolgaDia/CelulaDia/Chamado** via `:189` → **PerguntaDisputa fechamento admin** em `services/veredito_celula.py:177` via `declarar_dia.py:204` → **Chamado reconciliar** `veredito_celula.py:183` → RespostaChamado I via nota `:185`.

12. **PE12 — `veredito_celula`, ramo trabalhou**: atomic `app/ponto/services/veredito_celula.py:204`; **Batida U** via `flip_tipo:295`→`services/flip_batida.py:30`→`registro_batida.py:254`, ou Batida I via `veredito_celula.py:301`→`registro_batida.py:157` → **signals Batida**, PE02 e cartório → **TurnoMaterializado D/I** recompute explícito `veredito_celula.py:308` → **PerguntaDisputa → Chamado**, fechar fio `:325`→`:177`/`:183` → **CelulaDia → EventoCelula**, lavra `:330`→`portas/celula.py:817`→`:682`/`:673` → auditoria `veredito_celula.py:334`. Ramos e número de flips/batidas dependem do dado (`:290`, `:291`, `:299`).

13. **PE13 — `veredito_celula`, faltou/justificado**: mesmo atomic `:204`; **Ausencia I** via `:315` → signal cartório Ausencia `signals.py:144` → depois fechar fio **PerguntaDisputa → Chamado** `veredito_celula.py:325` → **CelulaDia/EventoCelula** `:330`. Não chamar Ausencia INSERT de trava de uma linha existente.

14. **PE14 — `regularizar_turno_orfao`**: atomic `app/ponto/services/regularizacao.py:89`; **Justificativa I `:91` ou U `:106`** → signal cartório Justificativa `signals.py:162` → **Ausencia get_or_create** via `regularizacao.py:118` (I quando criada, signals) → **ChamadoColaborador U analisar `:137`** → RespostaChamado I nota `:139` → auditoria `:145` em diante. Não é uma simples sequência Justificativa→Ausencia→Chamado porque os signals se intercalam em cada save.

15. **PE15 — `regenerar_em_exportada`**: atomic `app/ponto/portas/regen_exportada.py:169`; **PE03 CelulaDia → Chamado** via `:174` → cartório pós-regeneração, ainda sob atomic externo (`portas/celula.py:402`) → **FechamentoMensal U** `services/fechamento.py:1128`/`:1144`, chamado `regen_exportada.py:179` → **DiaPago D/I** `services/dia_pago.py:357`/`:358` via `fechamento.py:1185` e `:1261` → **DiaPago U** via `regen_exportada.py:188`→`dia_pago.py:536`/`:540` → LogAuditoria I `regen_exportada.py:199`.

16. **PE16 — `aplicar_no_vinculo` executor escala**: atomic `app/escala/services/escala_auto_executor.py:101`; **TipoEscala I `cadastro_tipo.py:416`**, AgendaBloco I `:424` por derivação `executor.py:102`→`:84`; **EscalaColaborador U** fechar vigência `colaboradores/services/vinculo.py:615`, chamada `:1008` → **EscalaColaborador I `:1010`**, via executor `:103`, com signals → **PE03 CelulaDia → Chamado** executor `:104` → HistoricoVinculo I `:107`.

17. **PE17 — `desfazer` executor**: atomic `app/escala/services/escala_auto_executor.py:141`; **EscalaColaborador D/U** `colaboradores/services/vinculo.py:1020`/`:1021` ou U/I `:1023`/`:1025`, chamada executor `:142` → **PE03 CelulaDia → Chamado** chamada `:143` → HistoricoVinculo I `:145`.

18. **PE18 — HE individual**: **DecisaoHE SELECT FOR UPDATE `portas/he.py:174` → U `:84` ou I `:66` → LogAuditoria I por `:94`**. FechamentoMensal/DiaPago estão no callback de `on_commit` (`:202`→`fechamento.py:1808`), não nesta transação.

19. **PE19 — ato HE período**: atomic `services/autorizacao_he_periodo.py:321` (prévia rollback `:323`) ou `:343` (confirmar); **DecisaoHE** via `_ato:277`→`portas/he.py:555`→`:562`→PE18; depois **Pauta I** somente exportada com autorizados (`autorizacao_he_periodo.py:299`, chamada `:300`, escritor `:244`), e sua expansão deve vir do censo pautas da raiz. O antes/depois `_medir` `:271`, `:280` não é o recalcular fechamento gravado.

20. **PE20 — designar cobertura**: atomic `app/ponto/services/cobertura.py:99`; **OrdemSubstituicao I `:100` → ChamadoColaborador reconciliar `:122` → RespostaChamado I `:132`**. `declarar_cobertura_passada` tem atomic `:283`, **OrdemSubstituicao I `:289` → Pauta existente marcar feita `:305` → Chamado reconciliar `:317` → RespostaChamado I `:321`**, ou sem substituto Pauta I `:333`.

**Cartório: subgrafo fundamental para os signals**

`app/ponto/services/cartorio.py:948` chama corpo. Nele, **Chamado/Pergunta antes de CelulaDia** pode ocorrer por `reconciliar:1488`, `:1501`, `:1522`, `:1600`; `ressuscitar_perguntas:1622`; emissor `:1740`/`:1778`; espelhar marcos `:1789`/`:1798`; só então **CelulaDia U** `carimbar:1822`→`portas/celula.py:849`→`:682`, **EventoCelula I** `:673`.

Depois da primeira lavra, pode ocorrer **CelulaDia antes de Pergunta/Chamado/Colaborador**: cartório `_e2(c):959`, reconciliar `:963`, cluster `:994`, fábrica `perguntar_pela_celula:997`; depois nova lavra pela releitura `:1038`. O ramo `nunca_bateu` grava célula `:1264` e retorna `:1268`; portanto não permite assumir que toda execução passa no mesmo conjunto de passos.

Esse subgrafo só pertence à transação emissora quando ela existe: callbacks `on_commit` já foram separados acima. Há ordens de **operações** nos dois sentidos, mas contagem de **primeira aquisição** de locks requer considerar travas já tomadas antes no mesmo caminho; não equiparar “save depois” a “primeira trava depois”.

**Commands com atomic externo chamando cálculo gravado**

Todos permitem **FechamentoMensal U `app/ponto/services/fechamento.py:1128`/`:1144` → DiaPago D/I `app/ponto/services/dia_pago.py:357`/`:358`** dentro da transação externa, ao tomar o ramo de escrita:

- `app/ponto/management/commands/desvio_o68b.py:93`, chamada cálculo `:94`.
- `app/ponto/management/commands/diff_reclassificar_partido.py:160`/`:198`, chamadas `:161`/`:206`.
- `app/ponto/management/commands/celula_veredito_velho.py:133`, cálculo `:149`.
- `app/ponto/management/commands/folga_que_sumiu.py:69`, cálculo `:70`.
- `app/ponto/management/commands/plano_b_no_dinheiro.py:109`, cálculo `:110`.
- `app/ponto/management/commands/aplicar_09_corte_b.py:197`, cálculo `:212`.
- `app/ponto/portas/regen_exportada.py:169`, cálculo `:179`, já PE15.

O rollback deliberado de sonda não apaga a aquisição durante a execução (`autorizacao_he_periodo.py:321`/`:323`; commands com respectivos blocos citados). Não executei essas sondas. [nao verificado]

**Limitações para raiz registrar**

- Os dados fornecidos não escolhem os ramos `if` dos caminhos; execução concreta, linhas realmente existentes e contagens de produção: [não verificado].
- Querysets múltiplos sem `order_by` não têm ordem de linhas estabelecida no código: órfãos `gerar_celulas.py:244`, perguntas fechamento admin `veredito_celula.py:174`, chamados `:148`/`:155`.
- Decisões HE em lote seguem lista recebida, `portas/he.py:546`, sem ordenação global.
- TipoEscala salvar não ganha atomic pelo decorator `cadastro_tipo.py:41`; esse pertence ao helper `:43`.
- Atomics internos da regeneração terminam em `portas/celula.py:348`; cartório `:402` ocorre depois. Ele compartilha o atomic apenas se existir wrapper externo, como PE04–PE08, PE10, PE15–PE17.
- `PeriodoFechado` `services/fechamento.py:1403`/`:1425` e FechamentoMensal `:1436` não têm transação própria nesse caminho; `reabrir_periodo` declara isso em `:1419`.
- `update_or_create` está presente em `portas/celula.py:906` e `escala/folgas.py:571`; a implementação Django/SQL interno não foi executada ou inspecionada aqui: [não verificado].
- INSERT de filho não comprova, por si só, trava de linha pai no instante previsto; efeito FK/constraints, triggers e plano PostgreSQL: [não verificado].

## 05/11 20:50 · sessao 18cfca111f7f · - · - · origem=Ronald (tmux)

Censo do recorte atribuído, somente por leitura do código. Os IDs abaixo identificam unidades de transação; INSERT está separado de escrita de linha existente. Receivers de ponto/chamados precisam ser expandidos pelo explorador responsável antes de transformar estas sequências em contagens globais. Os pontos de disparo estão em `app/ponto/services/justificativa.py:193`, `app/ferias/models.py:436`, `app/colaboradores/services/colaborador.py:61`.

**Correção:** `ChamadoColaborador.criar()` não implica a trava explícita de Colaborador de `abrir()`; não atribuir essa trava a C11. A trava explícita pertence a `app/chamados/models.py:866`.

### Selects explícitos do recorte

Todos os argumentos de `select_for_update()` abaixo são vazios:

| Local | Função | Modelo | Avaliação |
|---|---|---|---|
| `app/colaboradores/models.py:431` | `Colaborador.save` | Colaborador | `values_list(...).first()` em `:435-436`; atomic `:429`; **super.save em `:439` fica depois desse atomic** |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | AplicacaoConvencao | `.first()` `:116`; decorator atomic `:100` |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | SolicitacaoFerias | `.first()` `:720`; atomic `:717` |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `Command._rodar` | LoteHolerite, alias L `:51` | `.get()`; atomic `:53` |
| mesmo arquivo `:124` | `Command._rodar` | LoteHolerite | `.get()`; outro atomic `:123` |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | ChamadoColaborador | `.first()` |
| mesmo arquivo `:70` | `_encerrar_bloqueio_de_entrada` | ChamadoColaborador | `.order_by('-criado_em').first()` `:73` |
| mesmo arquivo `:114` | `travar_origem_antes` | ChamadoColaborador | `list(...order_by('pk').values_list(...))` |
| mesmo arquivo `:127` | `aprovar_justificativa_dp` | Justificativa | `.first()` `:129`; atomic `:125` |
| mesmo arquivo `:262` | `rejeitar_justificativa_dp` | Justificativa | `.first()` `:264`; atomic `:258` |
| mesmo arquivo `:329` | `materializar_saida_dp` | Justificativa | `.get()`; atomic `:327` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa | `.get()`; atomic `:1793` |

### Signals do recorte

Registro de colaboradores: `app/colaboradores/apps.py:8`. Registro de core: `app/core/apps.py:9`.

| Emissor | Receiver | Outra entidade escrita |
|---|---|---|
| Setor / post_save | `app/colaboradores/signals.py:10` | Group.save `:21`, ou Group.get_or_create `:25`; Group–Permission M2M pelo helper `:30` → `app/colaboradores/mixins.py:397`; volta a atualizar Setor `signals.py:32` |
| Setor / post_delete | `app/colaboradores/signals.py:36` | Group.delete `:43` |
| Posto / post_save | `app/colaboradores/signals.py:51` | DisputaSupervisao.remover_motivo `:77`, quando Posto existente está ativo e disputa casa com filtro `:71`; expansão do método pertence ao domínio chamados |
| Batida / post_save | `app/colaboradores/signals.py:87` | Colaborador via `_gravar_colab` `:102` → `app/colaboradores/services/colaborador.py:61`; somente criada, onboarding incompleto e usuário já logou, guards `signals.py:94`, `:97`, `:99` |
| DeviceColaborador / post_save | `app/colaboradores/signals.py:106` | Colaborador via `:119` → `services/colaborador.py:61`; guards criada/incompleto `signals.py:113`, `:116` |
| user_logged_in | `app/core/signals.py:14` | chama invalidar_sessoes_anteriores `:20`; Session em `app/core/views.py:30`; ocorrência dentro de atomic não demonstrada neste recorte [não verificado] |
| user_logged_in | `app/core/signals.py:40` | atribui sessão de request `:48`; persistência dentro da mesma transação [não verificado] |

`AgendamentoFerias.save()` **é override, não receiver**: primeiro `super().save()` em `app/ferias/models.py:436`, depois `_sync_ausencia()` `:437`; helper `:384-385` delega a `espelhar_ferias`. Este insere Ausencia por `app/ponto/services/ausencia.py:1186` → `:418`; apaga dias removidos em `:1197`; agendamento cancelado apaga existentes em `:1174`.

### Unidades de férias

O override acima introduz signals de Ausencia no meio dos passos seguintes. Essa expansão é necessária para a ordem completa. `app/ferias/models.py:437`, `app/ponto/services/ausencia.py:418`, `:1174`.

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| F1 `executar_cancelamento_admin`; views `app/ferias/views.py:365`, `:407`, `:559` | `app/ferias/services.py:352` | PeriodoAquisitivo **UPDATE** `:355` → Ausencia **UPDATE** chamada `:356`, escritor `app/ponto/services/ausencia.py:1267` → AgendamentoFerias **UPDATE** `services.py:362` → Ausencia **DELETE** pelo override/sync `models.py:437`, `ausencia.py:1174` |
| F2 `remarcar_agendamento`; view `app/ferias/views.py:668` | `app/ferias/services.py:399` | AgendamentoFerias **UPDATE** `:406` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:409` |
| F3 `corrigir_datas_agendamento`; view `app/ferias/views.py:490` | `app/ferias/services.py:375` | Ausencia **DELETE** chamada `:385`, escritor `ausencia.py:1254` → AgendamentoFerias **UPDATE** `services.py:387` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:389` |
| F4 `registrar_gozo_historico`; view `app/ferias/views.py:210` | `app/ferias/services.py:277` | AgendamentoFerias **INSERT** `:298` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:302` |
| F5 `executar_agendamento`; views `app/ferias/views.py:298`, `:617`, wrapper `:263` | `app/ferias/services.py:315` | AgendamentoFerias **INSERT** `:321` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:326` |
| F6 `agendar_ferias_porta` | `app/ferias/services.py:156` | chamada F5 `:157` → PeriodoAquisitivo **UPDATE adicional** `:165`, se dias vendidos |
| F7 view `avaliar_solicitacao` | `app/ferias/views.py:717` | SolicitacaoFerias **S4U** `:718` → SolicitacaoFerias **UPDATE** `app/ferias/services.py:422` → AgendamentoFerias **INSERT** `:447` → sync de Ausencia → PeriodoAquisitivo **UPDATE** `:450`, somente ramo com materialização |
| F8 `excluir_periodo_aquisitivo` | `app/ferias/services.py:482` | AgendamentoFerias **DELETE queryset** `:483` → PeriodoAquisitivo **DELETE** `:484` |

Não atribuir `AgendamentoFerias.delete()` `app/ferias/models.py:439` ao `QuerySet.delete()` de F8: o chamador efetivo é `periodo.agendamentos.all().delete()` `app/ferias/services.py:483`; execução do override por esse queryset [não verificado].

Escritas atômicas de uma entidade existente: `editar_periodo_aquisitivo`, atomic `app/ferias/services.py:470`, PeriodoAquisitivo.save `:475`; sincronização de ciclos sob atomic `app/ferias/views.py:164`, chamada `:180` → PeriodoAquisitivo.update `app/ferias/services.py:266`.

**Não unir cancelar_agendamento_dp numa transação:** Periodo.save `app/ferias/services.py:335`, Agendamento.save `:337`; função `:330` e view `app/ferias/views.py:321` não delimitam atomic; transação envolvente [não verificado].

### Unidades de justificativa e regularização

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| J1 view `app/ponto/views.py:1917` → `aprovar_justificativa_dp` | `app/ponto/services/justificativa.py:125` | Chamado **S4U por pk** chamada `:126` → escritor `:114`; Justificativa **S4U** `:127`; Batida **INSERT condicional** chamada `:173`; Justificativa **UPDATE** `:193`; FechamentoMensal **UPDATE** chamada `:200` → `app/ponto/services/fechamento.py:1640`; Chamado origem **S4U** chamada `justificativa.py:202` → `:38`; Chamado bloqueio **S4U** chamada `:203` → `:70` |
| J2 view `app/ponto/views.py:1929` → `rejeitar_justificativa_dp` | `app/ponto/services/justificativa.py:258` | Chamado **S4U por pk** chamada `:261` → `:114`; Justificativa **S4U** `:262`; Justificativa **UPDATE** `:272`; efeitos B5 ficam após saída do atomic, início `:274` |
| J3 view `app/ponto/views.py:3136` → `materializar_saida_dp` | `app/ponto/services/justificativa.py:327` | Chamado **S4U por pk** chamada `:328` → `:114`; Justificativa **S4U** `:329`; Batida **INSERT** chamada `:346`; Justificativa **UPDATE** `:371`; FechamentoMensal **UPDATE** chamada `:375` → fechamento `:1640`; Chamados fechamento `justificativa.py:385`, origem `:393` |
| J4 view `aprovar_lote_justificativas` | `app/ponto/views.py:1793`, por item | Chamado **S4U por pk** chamada `:1794` → justificativa `:114`; Justificativa **S4U** `views.py:1795`; Justificativa **UPDATE** `:1800`; FechamentoMensal **UPDATE** chamada `:1806` → fechamento `:1640`; Batida **INSERT condicional** chamada `views.py:1816`; Justificativa **UPDATE** `:1833`; Chamado origem **S4U** chamada `:1838` → justificativa `:38` |
| J5 `regularizar_turno_orfao`; web `app/ponto/views.py:3205`, API `app/api/views.py:2877` | `app/ponto/services/regularizacao.py:89` | Justificativa **INSERT** `:91` ou, quando existente sem saída, **UPDATE** `:106`; Ausencia **INSERT** chamada `:118` → `app/ponto/services/ausencia.py:461` → `:418`; Chamado.analisar `regularizacao.py:137`; nota `:139`; LogAuditoria `:150` |

Os saves/INSERTs de Justificativa e Batida disparam signals antes do passo textual seguinte; não chamar J1/J4 de ordem final de Fechamento vs Batida sem expandi-los. Os locais efetivos são `app/ponto/services/justificativa.py:173`, `:193`, `app/ponto/views.py:1800`, `:1816`.

`invalidar_fechamento` faz atomic de leitura `app/ponto/services/fechamento.py:1632` e atomic por competência `:1635`; escrita efetiva `:1640`. Sob J1/J3/J4 esses atomics são aninhados aos atomics dos chamadores citados.

### Colaboradores / Empresa

Escritores: Colaborador.save por campos `app/colaboradores/services/colaborador.py:61`, `:69`; save inteiro `:76`; update queryset `:88`. Empresa.update `app/colaboradores/services/estrutura.py:27`. HistoricoVinculo INSERT `app/colaboradores/services/historico_vinculo.py:25`. User.save de acesso `app/core/credenciais.py:114`.

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| C1 command `normalizar_identificadores` | `app/colaboradores/management/commands/normalizar_identificadores.py:106` | Colaborador **UPDATE** chamada `:109` → escritor `services/colaborador.py:88`; depois Empresa **UPDATE** chamada `:112` → `services/estrutura.py:27` |
| C2 view `editar_colaborador` | `app/colaboradores/views.py:1340` | Colaborador **UPDATE** chamada `:1342` → escritor `services/colaborador.py:76`; User **UPDATE** `views.py:1345`, se CPF mudou e há usuário |
| C3 service `executar_reativacao`; view `app/colaboradores/views.py:1417` | `app/colaboradores/services/desligamento.py:209` | Colaborador **UPDATE** chamada `:214` → escritor `services/colaborador.py:76`; User **UPDATE condicional** chamada `desligamento.py:216` → `core/credenciais.py:114`; HistoricoVinculo **INSERT** chamada `desligamento.py:219` → escritor `services/historico_vinculo.py:25` |
| C4 service `executar_desligamento` imediato | `app/colaboradores/services/desligamento.py:103` | Colaborador **UPDATE** chamada `:108` → escritor `colaborador.py:76`; User **UPDATE condicional** chamada `desligamento.py:111` → credenciais `:114`; HistoricoVinculo **INSERT** chamada `desligamento.py:114`; EscalaColaborador **UPDATE** chamada `:123` → `services/vinculo.py:816` → `:615`; DisputaSupervisao.fechar `desligamento.py:142`, por pk `:139`; Chamado.encerrar `:155`; Chamado **INSERT** chamada `.criar` `:163` |
| C5 desligamento agendado | `app/colaboradores/services/desligamento.py:83` | Colaborador **UPDATE** chamada `:85` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `desligamento.py:88` |
| C6 view `posto_add_colab` | `app/colaboradores/views.py:643` | Colaborador **UPDATE** chamada `:645` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `views.py:647` |
| C7 view `posto_remove_colab` | `app/colaboradores/views.py:668` | Colaborador **UPDATE** chamada `:670` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `views.py:672` |
| C8 view `transferir_posto` | `app/colaboradores/views.py:1384` | Colaborador **UPDATE** chamada `:1388` → `colaborador.py:76`; HistoricoVinculo **INSERT** chamada `views.py:1393` |
| C9 view admitir | `app/colaboradores/views.py:1175` | User **INSERT** `:1176`; Colaborador **S4U condicional** `app/colaboradores/models.py:431`; Colaborador **INSERT** chamada `views.py:1182` → `services/colaborador.py:93` → `models.py:439` |
| C10 view `avaliar_solicitacao_beneficio` | `app/colaboradores/views.py:2065` | Chamado.transicionar `:2068`; nota/FollowUp `:2071`; Beneficio **INSERT condicional** chamada `:2075` → `services/beneficios.py:11`; SolicitacaoBeneficio **UPDATE** chamada `views.py:2086` → `services/beneficios.py:32` |
| C11 view `agenda_enviar_aceite` | `app/colaboradores/views.py:2358` | Chamado **INSERT** chamada `:2363`; AgendaDia **UPDATE** chamada propor_agenda `:2378`; escritor no domínio escala |
| C12 `bootstrap_tenant` | `app/tenants/bootstrap.py:25` | Empresa **INSERT** chamada `:27`; Setor **INSERT** chamada `:33`, com receiver Group/M2M/Setor descrito acima; repete por competência `:32`; User **INSERT** `:34` |

Em C9, **o atomic termina antes de `app/colaboradores/views.py:1209`**. O save adicional `:1215` e Histórico `:1218` não estão nesse bloco.

Escritas atômicas adicionais de EscalaColaborador:

- `executar_vinculo`, atomic `app/colaboradores/services/vinculo.py:187`: fecha vigências em `:205`, `:232` → UPDATE `:615`; absorve posteriores `:244` → UPDATE `:753`; EscalaColaborador existente save `:274` ou INSERT `:278`; copia folga `:275`/`:289` → save `:160`. Expansão de signals do save compete ao domínio escala.
- `trocar_calendario_de_folga`, atomic `:443`, save `:445`, LogAuditoria `:448`.
- `fechar_vigencia`, atomic `:614`, update `:615`, LogAuditoria `:621`.
- `reabrir_vigencia_impossivel`, atomic `:690`, update `:691`, LogAuditoria `:697`, HistoricoVinculo INSERT chamada `:701` → `historico_vinculo.py:13`.
- `absorver_vigencias_posteriores`, atomic `:752`, update `:753`, LogAuditoria `:763`, HistoricoVinculo INSERT chamada `:771` → `historico_vinculo.py:13`.
- `encerrar_vinculo`, atomic `:796`, save `:799`, LogAuditoria `:802`.
- `gravar_regua`, atomic `:989`, update `:991`, LogAuditoria `:998`.

Outras escritas sob atomic no recorte:

- Command regerar_senhas_iniciais, atomic `app/colaboradores/management/commands/regerar_senhas_iniciais.py:64`, chama resetar_para_cpf `:70`; escritor User.save em `app/core/credenciais.py:55` ou `:71`, conforme função.
- Command marcar_feriado_do_posto, atomic `app/colaboradores/management/commands/marcar_feriado_do_posto.py:129`, Posto.save chamada `:131` → `services/estrutura.py:82`; receiver Posto pode escrever Disputa/Chamado. O loop de Chamado começa em **outro atomic por chamado**, command `:139`, reconciliar `:140`.
- Command lavrar_placar_situacional, atomic `app/core/management/commands/lavrar_placar_situacional.py:101`, PlacarSituacional.update_or_create `:105`, delete de outros `:110`.

### Holerite / AplicacaoConvencao

| ID | Atomic | Sequência |
|---|---|---|
| H1 publicação `motor.processar` | `app/holerite/motor.py:385` | LoteHolerite **UPDATE** `:409` se lote recebido, ou **INSERT** `:400`; Holerite **UPDATE condicional** `:461` para anexos; Holerite **INSERT** via arquivo.save `:483`; LoteHolerite **UPDATE** `:487` |
| H2 `validar_e_enviar`; view `app/holerite/views.py:618` | decorator `app/holerite/servicos.py:144` | `enviar` chamada `:155`, laço por holerite `:100`: push `:109`; pode Colaborador **UPDATE** por token inválido e PushSubscription **UPDATE** por endpoint morto; depois Holerite **UPDATE** `:113`, `:121` ou `:129`; LogAuditoria **INSERT** `:134` |
| H3 command backfill_empresa_folha | `app/holerite/management/commands/backfill_empresa_folha.py:55` | Holerite **UPDATE** `:56` |
| A1 AplicacaoConvencao.cadastrar | decorator `app/core/services/aplicacao_convencao.py:100` | AplicacaoConvencao **S4U** `:115`; UPDATE `:132` ou INSERT `:120`; LogAuditoria INSERT chamada `_trilha` `:133` → evento `:94` → `app/core/observ.py:134` |
| A2 AplicacaoConvencao.desativar | decorator `app/core/services/aplicacao_convencao.py:137` | AplicacaoConvencao **UPDATE** `:155`; LogAuditoria INSERT chamada `:156` → evento `:94` → observ `:134` |

Em H1, o command faz **três transações separadas**: command `app/holerite/management/commands/processar_lote_holerite.py:53` (S4U `:54`, save `:68`); publicação chamada `:89`, atomic no motor `:385`; finalização command atomic `:123` (S4U `:124`, save `:146`). Não unir a primeira e terceira com a publicação.

Expansão de H2:

1. FCM `_push` `app/holerite/servicos.py:38` → `app/chamados/fcm_utils.py:106` → erro token inválido chama limpeza `:93`.
2. `_limpar_token` chama `app/chamados/fcm_utils.py:41` → `app/colaboradores/fcm.py:61` → Colaborador.update `app/colaboradores/services/colaborador.py:88`.
3. Fallback web `_push` `app/holerite/servicos.py:42` → PushSubscription.save `app/ponto/webpush.py:66` quando erro contém 410/404 `:59`.
4. Holerite.update em `app/holerite/servicos.py:113`, `:121`, `:129`.

O laço de H2 permite `Holerite da pessoa anterior → Colaborador/PushSubscription da pessoa seguinte`, pois o laço está em `app/holerite/servicos.py:100`, push `:109`, update `:113`. Quais ramos ocorreram não está no dado fornecido [não verificado].

### Ausencia / Fechamento e limites úteis para consolidação

- `criar_ausencia` constrói objeto novo em `app/ponto/services/ausencia.py:385` e faz **INSERT** dentro do atomic `:417`, save `:418`. Não classificar esse local como UPDATE de Ausencia existente.
- `garantir_ausencia` delega à criação com retorno de existente em `app/ponto/services/ausencia.py:461`; não demonstrou UPDATE do existente.
- Writes de Ausencia existente em `app/ponto/services/ausencia.py:508`, `:536`, `:819`, `:843`, `:911`, `:1114`, `:1124`, `:1132`, `:1280`, `:1292` não possuem atomic próprio no método; só entram numa transação compartilhada quando há chamador externo demonstrado.
- Ausencia.decidir invalida Fechamento em `app/ponto/services/ausencia.py:930`; escritor Fechamento.update em `app/ponto/services/fechamento.py:1640`. Sem atomic externo que cubra as duas operações, não contar automaticamente `Ausencia → Fechamento` na mesma transação.
- Ausencia.save só deriva campos e grava em `app/ponto/models.py:906`; não localizei nele escrita direta de Colaborador.
- LogAuditoria sempre nasce pelo INSERT `app/core/observ.py:134`, dentro de atomic próprio `:133`; se chamado sob atomic externo, é etapa da transação externa.
- A única ordem de duas entidades **existentes** envolvendo Empresa que confirmei neste recorte é `Colaborador → Empresa`, C1, `normalizar_identificadores.py:109`, `:112`. Ordem inversa [não verificado].
- O par AgendamentoFerias/PeriodoAquisitivo tem ambas as ordens textuais: F1 `Periodo :355 → Agendamento :362`; F2 `Agendamento :406 → Periodo :409`, todos em `app/ferias/services.py`. F1 não depende de INSERT para demonstrar a ordem; F2 também não. Os sinais/sync podem introduzir outras entidades intermediárias `app/ferias/models.py:437`.
- Ordem entre linhas tocadas por um UPDATE queryset não é expressa pelo Python; exemplos `app/colaboradores/services/vinculo.py:615`, `:753`, `app/ponto/services/ausencia.py:1267`. Execução real de linhas e IDs atingidos [não verificado].

## 05/11 20:48 · sessao 35b27ecbba74 · - · - · origem=Ronald (tmux)

Censo independente, somente leitura:

**24 chamadas reais de `select_for_update`**, aferidas por AST fora de `tests/`, arquivos `test*` e migrations; todos argumentos vazios, salvo `of=('self', 'disputa')` em `app/chamados/services/relavra_via.py:221`. Não há argumentos `nowait`, `skip_locked` ou `no_key` nessas chamadas. [não verificado: chamadas geradas dinamicamente/dependências externas]

| Arquivo:linha | Função | Modelo |
|---|---|---|
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100` | `_rodar` | ChamadoColaborador |
| `app/chamados/models.py:674` | `_recarregar_estado_se_mudou` | `type(self)`; ChamadoColaborador neste método |
| `app/chamados/models.py:866` | `abrir` | Colaborador |
| `app/chamados/services/abertura.py:164` | `abrir_chamado_para_colaborador` | Colaborador |
| `app/chamados/services/disputa_emissao.py:467` | `_abrir_ou_agregar_disputa` | DisputaSupervisao |
| `app/chamados/services/disputa_emissao.py:1156` | `criar_disputa_ancora_de_chamado` | DisputaSupervisao |
| `app/chamados/services/fio.py:762` | `_costura_nao_mata_o_fio` | `type(chamado)` |
| `app/chamados/services/relavra_via.py:221` | `desfazer_carimbo_sem_lastro` | PerguntaDisputa **e DisputaSupervisao**, aquisição na mesma consulta |
| `app/colaboradores/models.py:431` | `save` | Colaborador |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | AplicacaoConvencao |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | SolicitacaoFerias |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `_rodar` | LoteHolerite, alias `L` importado na linha 51 |
| `app/holerite/management/commands/processar_lote_holerite.py:124` | `_rodar` | LoteHolerite |
| `app/ponto/portas/he.py:174` | `decidir_he` | DecisaoHE |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | DecisaoHE |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | DecisaoHE |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | DecisaoHE |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:70` | `_encerrar_bloqueio_de_entrada` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:114` | `travar_origem_antes` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:127` | `aprovar_justificativa_dp` | Justificativa |
| `app/ponto/services/justificativa.py:262` | `rejeitar_justificativa_dp` | Justificativa |
| `app/ponto/services/justificativa.py:329` | `materializar_saida_dp` | Justificativa |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa |

`relavra_via.py:221` não permite afirmar ordem PerguntaDisputa→DisputaSupervisao: é uma consulta com dois alvos em `of`; a ordem interna de aquisição está [não verificado]. O `select_related('disputa','celula')` da linha 222 não inclui CelulaDia no `of`. `models.py:673` e `fio.py:761` condicionam o uso de `select_for_update` a `connection.in_atomic_block`.

**Advisory e configurações**

- `app/ponto/services/vigia_de_hora.py:99`: `SELECT pg_try_advisory_lock(%s)`; unlock na linha 136.
- `app/chamados/reconciliador.py:1224`: `SELECT pg_try_advisory_lock(%s)`; unlock na linha 1231; chave por schema calculada em `app/chamados/reconciliador.py:1205`.
- Nenhuma ocorrência executável de `LOCK TABLE` ou advisory `xact_lock` encontrada na varredura da cópia. [não verificado: dependências externas/SQL dinâmico]
- Os dicionários de banco `app/config/settings/saas.py:83` e `app/config/settings/prod.py:9` não definem `ATOMIC_REQUESTS`; não apareceu declaração desse parâmetro na árvore pesquisada. Configuração efetiva de produção [não verificado].
- Padrão de `DJANGO_SETTINGS_MODULE`: SaaS em `app/manage.py:9` e `app/config/wsgi.py:14`; variáveis externas podem substituir o padrão, portanto perfil efetivo [não verificado].

**Domínio residual Pautas: quatro caminhos de UPDATE de entidades existentes**

Todos passam por trilha inserida em LogAuditoria (`app/pautas/services.py:369` → `app/core/observ.py:133`/134), entre Pauta e ContadorPauta.

| Caminho/entrada | Sequência executada dentro do atomic |
|---|---|
| `marcar_lida`, atomic `app/pautas/services.py:203`; view `app/pautas/views.py:95` | Pauta UPDATE `services.py:215` → LogAuditoria INSERT via `services.py:217` → ContadorPauta UPDATE `services.py:354`, chamado na linha 218 |
| `marcar_feita`, atomic `services.py:232`; view `views.py:103` | Pauta UPDATE `services.py:247` → LogAuditoria INSERT via linha 250 → ContadorPauta UPDATE linha 354, chamado na linha 251 |
| `marcar_feita_pelo_sistema`, atomic `services.py:255` | Pauta UPDATE `services.py:272` → LogAuditoria INSERT via linha 275 → ContadorPauta UPDATE linha 354, chamado na linha 276 |
| `cancelar`, atomic `services.py:280`; view `views.py:123` | Pauta UPDATE `services.py:294` → LogAuditoria INSERT via linha 296 → ContadorPauta UPDATE linha 354, chamado na linha 297 |

ContadorPauta pode nascer por `get_or_create` na linha 352, mas recebe UPDATE subsequente na linha 354.

Se INSERT também for considerado na sequência de escritas: `escrever`, atomic `app/pautas/services.py:152`, faz Pauta INSERT linha 180 → LogAuditoria INSERT por linha 186 → ContadorPauta UPDATE linha 354 por chamada linha 188. Entradas: view `app/pautas/views.py:76`, resposta `views.py:112` → service `services.py:194`. Command `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133` envolve cabeçalho e filhas num atomic externo e chama `escrever` nas linhas 134 e 139.

Contagem local estrita de atualizações: **Pauta antes de ContadorPauta = 4 caminhos; ContadorPauta antes de Pauta = 0 caminhos**; os quatro caminhos e referências estão acima. Ao contar o INSERT de `escrever`, a primeira direção ganha um quinto caminho.

**Relatórios e Mensageria**

- `app/relatorios/lote_espelho.py:102`: atomic de `pedir` contém somente INSERT de LoteEspelho na linha 103. O UPDATE de lote antigo chamado por `vigiar` está antes do bloco (`lote_espelho.py:96`/97 → linha 75).
- Não encontrei atomic declarado em Mensageria; suas escritas sem atomic explícito incluem `mensageria/nucleo/acoes.py:21`, `management/commands/cruzar.py:37`/46, `enviar_outbox.py:71`/77 e `coletar_sinais.py:28`. Inclusão em transação por configuração efetiva externa [não verificado].

**Scripts operacionais fora de app**

Não omitir se “produção” abranger ferramentas de reversão:

- `bin/restore_relavratura_10_2026.py:69`: atomic, itera arquivo externo da linha 32; FechamentoMensal SAVE existente linha 81 → FechamentoMensal DELETE linha 83 → DiaPago DELETE linha 85 → DiaPago bulk INSERT linha 86. Ordem entre FechamentoMensal individuais depende da lista externa [não verificado].
- `bin/restore_o263_comp10.py:40`: atomic; FechamentoMensal SAVE linha 50 → FechamentoMensal DELETE linha 52 → DiaPago DELETE linha 54 → DiaPago bulk INSERT linha 56. Dados vêm do arquivo externo linha 24; asserts de banco SaaS nas linhas 17/18.
- `bin/restore_badge_linha_09_10.py:59`: atomic; somente DiaPago DELETE linha 61 → DiaPago bulk INSERT linha 63.

Esses dois restores dão **FechamentoMensal antes de DiaPago = 2 caminhos; sentido inverso = 0**, sem incluir cascatas de DELETE não avaliadas.

Ferramentas explicitamente de banco sombra:

- `bin/sombra_mascara.py:38`/39 exige banco sombra; atomic linha 51; Colaborador UPDATE linha 59 → User UPDATE linha 60 → repetições Colaborador/User linhas 66/70/72/76 → PushSubscription UPDATE linha 84 → RegistroPDFAuditavel UPDATE linha 88.
- `bin/sombra_mascara_pessoal.py:10` exige sombra; atomic linha 16; modelos determinados pelas tabelas ordenadas de regras em `bin/sombra_regras_pessoais.py:49`,70,60,66,79,80,81,138; UPDATEs/bulk_updates em `sombra_mascara_pessoal.py:20`,25,33,36,41,59,61. Não confundir com caminho de aplicação de produção.
- `bin/sondas_rollback.py:49` envolve código `SONDA_SRC` dinâmico executado na linha 53, sempre termina com `raise _Rollback` linha 61. Entidades/ordens do payload [não verificado].

**Registro de signals e ordem**

Ordem declarada no perfil base: core → colaboradores → ponto → escala → chamados (`app/config/settings/base.py:15`,16,19,20,21). Ordem declarada no SaaS: colaboradores → chamados → escala → ponto → core (`app/config/settings/saas.py:37`,38,39,40,49; composição final linha 53).

Importação dos módulos:
`app/core/apps.py:9`, `app/colaboradores/apps.py:8`, `app/ponto/apps.py:8`, `app/escala/apps.py:9`, `app/chamados/apps.py:10`.

Perfil carregado, registro efetivo e interferência de imports prévios [não verificado]. Portanto não converter ordem de aplicativos declarada automaticamente em ordem efetiva comprovada de disparo entre módulos.

Signal adicional registrado por aliases fora de `signals.py`: TipoAusencia post_save/post_delete, `app/ponto/models.py:1233`/1234, receiver `_invalida_cat_ausencia` linha 1235, só `_CAT.invalidar_cache()` linha 1236; sem escrita de outra entidade nesse corpo. Não encontrei `m2m_changed`, `.connect()` manual de signals ou `Signal()` próprio no código de produção pesquisado. [não verificado: registros externos/dinâmicos]

**Alertas de abrangência**

AST detectou **181 declarações/blocos atomic** em `app/` de produção. Vários usam aliases e escapam de busca literal por `transaction.atomic`; em Chamados, inspecionar especialmente:

- `app/chamados/services/acoes_chamado.py:102`,140;
- `app/chamados/services/regularizacao_ext.py:121`,152,235;
- `app/chamados/services/materializacao.py:1142`;
- `app/chamados/services/fio.py:837`;
- `app/chamados/services/disputa_emissao.py:408`,466,898,1148;
- `app/chamados/models.py:274`,290,616,744,1542,1558,1846.

A cópia não contém `.git` e `git rev-parse HEAD` falhou; correspondência ao HEAD/hash informado [não verificado].

## 05/11 20:11 · sessao 4823c9d8907d · HJ-INGESTAO-0511 · censo · origem=Code

HJ-INGESTAO-0511, padrão=n/a (medida). Inventário: **80 unidades; 50 com colaborador e dia; 30 puladas**. Conferi sintaxe e contagem em memória; execução contra banco e resultados de produção permanecem `[nao verificado]`.

Assinaturas conferidas na cópia:

| Item | Assinatura / modelo real | Prova |
|---|---|---|
| a | `status_do_dia_lavrado(colaborador, data, *, agora=None)` | `app/ponto/status_dia.py:201` |
| b | `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)` | `app/ponto/management/commands/e6_oraculo.py:53` |
| b, alimentação | `InsumosDoDono.__init__(self, colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; `dono(self, data)` | `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:172` |
| c | `classificar_export(empresa, mes, ano, fechamentos=None)` | `app/folha/export.py:746` |
| d | `dias_que_seguram(empresa, mes, ano)` | `app/folha/fora_da_folha.py:102` |
| e | `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` | `app/chamados/juizes.py:1341` |
| f | ORM de `ChamadoColaborador`; campos `modulo_origem` e `status_local`; `data_do_chamado(modulo_origem, contexto_json)` | `app/chamados/models.py:21`; `app/chamados/models.py:40`; `app/chamados/models.py:46`; `app/chamados/catalogo/modulos.py:219` |

Em **b**, o caminho é exatamente `espelho_do_colab → InsumosDoDono → dono`, usado pelo comando. Os insumos vêm das batidas apuráveis no envelope ampliado, dos turnos carregados, do resultado do espelho, das lavras de fase e do cadastro do dia; a sonda não monta outra classificação. `app/ponto/management/commands/e6_oraculo.py:142`; `app/ponto/management/commands/e6_oraculo.py:148`; `app/ponto/management/commands/e6_oraculo.py:154`; `app/ponto/management/commands/e6_oraculo.py:164`; `app/ponto/management/commands/e6_oraculo.py:172`; `app/ponto/management/commands/e6_oraculo.py:304`; `app/ponto/management/commands/e6_oraculo.py:316`; `app/ponto/management/commands/e6_oraculo.py:366`.

Em **c/d**, uma memória local preserva a única execução real do classificador por empresa/competência, inclusive sua exceção: `dias_que_seguram` importa e chama esse mesmo classificador internamente. A competência vem de `janela_atual`; corte diferente de 21 produz erro em b/c/d. `app/folha/fora_da_folha.py:115`; `app/folha/fora_da_folha.py:119`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`.

Cole o `exec` abaixo no `manage.py shell` já aberto. `processadas` significa unidades sem erro; as outras categorias são `com_erro` e `puladas`, exclusivas entre si. A leitura usa o instante da execução, sem reconstrução histórica do banco. `[nao verificado]`

===SONDA===
```python
exec(r'''
import sys
sys.dont_write_bytecode = True
import datetime as dt
import json
import logging
from unittest.mock import patch
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

# Inventario literal do anexo fornecido; fonte textual [nao verificado].
UNIDADES = [
    ("HJ-001-U1", 836, "2026-10-13"),
    ("HJ-001-U2", 836, "2026-10-14"),
    ("HJ-002-U1", 909, "2026-10-16"),
    ("HJ-003-U1", 407, "2026-10-21"),
    ("HJ-004-U1", 789, "2026-10-11"),
    ("HJ-004-U2", 789, None),
    ("HJ-005-U1", 42, "2026-10-01"),
    ("HJ-005-U2", 42, "2026-10-08"),
    ("HJ-005-U3", 42, "2026-10-09"),
    ("HJ-005-U4", 42, "2026-10-15"),
    ("HJ-005-U5", 42, "2026-10-22"),
    ("HJ-006-U1", 226, "2026-10-21"),
    ("HJ-007-U1", 253, None),
    ("HJ-008-U1", 283, None),
    ("HJ-009-U1", 504, "2026-10-21"),
    ("HJ-010-U1", 255, "2026-10-03"),
    ("HJ-011-U1", 709, "2026-10-22"),
    ("HJ-012-U1", 239, "2026-10-07"),
    ("HJ-012-U2", 239, "2026-10-12"),
    ("HJ-013-U1", 43, "2026-10-10"),
    ("HJ-013-U2", 43, "2026-10-12"),
    ("HJ-013-U3", 43, "2026-10-26"),
    ("HJ-014-U1", 956, "2026-10-16"),
    ("HJ-014-U2", 956, "2026-10-22"),
    ("HJ-015-U1", 39, "2026-10-15"),
    ("HJ-016-U1", 297, "2026-09-21"),
    ("HJ-016-U2", 297, "2026-10-06"),
    ("HJ-016-U3", 297, "2026-10-13"),
    ("HJ-016-U4", 297, "2026-10-20"),
    ("HJ-016-U5", 297, "2026-10-27"),
    ("HJ-017-U1", 923, "2026-09-27"),
    ("HJ-017-U2", 923, None),
    ("HJ-018-U1", 130, "2026-10-05"),
    ("HJ-019-U1", 29, "2026-10-15"),
    ("HJ-020-U1", 189, "2026-09-21"),
    ("HJ-020-U2", 189, "2026-10-19"),
    ("HJ-021-U1", 204, "2026-09-21"),
    ("HJ-022-U1", 502, "2026-10-09"),
    ("HJ-023-U1", 222, "2026-10-12"),
    ("HJ-023-U2", 256, "2026-10-12"),
    ("HJ-024-U1", 55, "2026-10-03"),
    ("HJ-025-U1", 270, None),
    ("HJ-026-U1", 129, None),
    ("HJ-027-U1", 292, "2026-09-21"),
    ("HJ-027-U2", 292, "2026-10-09"),
    ("HJ-028-U1", 174, "2026-10-25"),
    ("HJ-028-U2", 174, "2026-10-26"),
    ("HJ-029-U1", 33, "2026-10-03"),
    ("HJ-029-U2", 33, "2026-10-04"),
    ("HJ-030-U1", 94, "2026-10-16"),
    ("HJ-031-U1", 247, "2026-10-24"),
    ("HJ-032-U1", 163, None),
    ("HJ-033-U1", 960, None),
    ("HJ-034-U1", 873, "2026-09-21"),
    ("HJ-034-U2", 873, "2026-09-26"),
    ("HJ-035-U1", 949, None),
    ("HJ-036-U1", 207, None),
    ("HJ-037-U1", 332, None),
    ("HJ-038-U1", 446, None),
    ("HJ-039-U1", 89, None),
    ("HJ-040-U1", 92, None),
    ("HJ-041-U1", 231, None),
    ("HJ-042-U1", 214, None),
    ("HJ-043-U1", 235, None),
    ("HJ-044-U1", 246, None),
    ("HJ-045-U1", 250, None),
    ("HJ-046-U1", 673, None),
    ("HJ-047-U1", 194, None),
    ("HJ-048-U1", 219, None),
    ("HJ-049-U1", 288, "2026-10-04"),
    ("HJ-050-U1", 523, "2026-10-25"),
    ("HJ-050-U2", 523, "2026-11-07"),
    ("HJ-051-U1", 123, None),
    ("HJ-052-U1", 1023, None),
    ("HJ-052-U2", 78, None),
    ("HJ-053-U1", None, None),
    ("HJ-054-U1", None, None),
    ("HJ-055-U1", None, None),
    ("HJ-056-U1", 270, None),
    ("HJ-056-U2", None, None),
]
assert len(UNIDADES) == len({u[0] for u in UNIDADES}) == 80
for uid, cid, iso in UNIDADES:
    assert cid is None or type(cid) is int
    if iso is not None:
        assert dt.date.fromisoformat(iso).isoformat() == iso

def erro(exc):
    # Sem mensagem SQL, nomes ou documentos na saida.
    return {"ERRO": type(exc).__name__,
            "sqlstate": getattr(exc, "sqlstate", None)
                        or getattr(getattr(exc, "__cause__", None), "pgcode", None)}

def somente_select(execute, sql, params, many, context):
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_fora_da_leitura")
    return execute(sql, params, many, context)

def leitura(fn):
    if connection.vendor != "postgresql" or connection.in_atomic_block:
        raise RuntimeError("exige_postgresql_sem_transacao_previa")
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION READ ONLY")
        with connection.execute_wrapper(somente_select):
            with connection.cursor() as cur:
                cur.execute("SELECT current_schema()")
                if cur.fetchone()[0] != "juliani":
                    raise RuntimeError("schema_diferente_de_juliani")
            return fn()

def tentar(fn):
    try:
        return True, leitura(fn)
    except Exception as exc:
        return False, erro(exc)

def uma_vez(memo, chave, fn):
    if chave not in memo:
        try:
            memo[chave] = (True, fn())
        except Exception as exc:
            memo[chave] = (False, exc)
    ok, valor = memo[chave]
    if not ok:
        raise valor
    return valor

def tem_erro(valor):
    if isinstance(valor, dict):
        return "ERRO" in valor or any(tem_erro(x) for x in valor.values())
    if isinstance(valor, (list, tuple)):
        return any(tem_erro(x) for x in valor)
    return False

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, default=str), flush=True)

def executar():
    from colaboradores.models import Colaborador
    from escala.models import CelulaDia
    from ponto.status_dia import status_do_dia_lavrado
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from ponto.services.espelho import espelho_do_colab
    from ponto.janelas import corte_da_empresa, janela_atual
    from folha import export
    from folha.fora_da_folha import dias_que_seguram
    from chamados.juizes import porta_alcancavel_do_dia
    from chamados.models import ChamadoColaborador
    from chamados.catalogo.modulos import data_do_chamado

    agora = timezone.now()
    hoje = timezone.localdate(agora)
    real_classificar = export.classificar_export
    memo_export, memo_dias = {}, {}
    totais = {"processadas": 0, "com_erro": 0, "puladas": 0}

    # Proxy apenas em memoria: a chamada interna de d reutiliza c.
    # app/folha/fora_da_folha.py:115 e :119.
    def classificar_uma_vez(empresa, mes, ano, fechamentos=None):
        if fechamentos is not None:
            raise RuntimeError("sonda_exige_fechamentos_gravados")
        return uma_vez(
            memo_export, (empresa.pk, mes, ano),
            lambda: real_classificar(empresa, mes, ano),
        )

    def competencia(c, dia):
        corte = corte_da_empresa(c.empresa)
        if corte != 21:
            raise ValueError("corte_da_empresa_diverge_de_21")
        ini, fim = janela_atual(dia, c.empresa)
        return ini, fim, fim.month, fim.year

    def dono(c, dia, comp):
        ini, fim, mes, ano = comp
        # Mesmo caminho E6: e6_oraculo.py:304, :316 e :366.
        esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
        ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
        return {"dono": ins.dono(dia)}

    def balde(c, comp):
        ini, fim, mes, ano = comp
        itens = export.classificar_export(c.empresa, mes, ano)
        achados = [i for i in itens if i["colaborador_id"] == c.pk]
        if len(achados) > 1:
            raise ValueError("mais_de_um_item_do_colaborador")
        cab = {"empresa_id": c.empresa_id, "mes": mes, "ano": ano,
               "inicio": ini, "fim": fim}
        if not achados:
            return {**cab, "estado": "sem item no universo"}
        i = achados[0]
        return {**cab, "status": i["status"], "motivo": i["motivo"],
                "travas_aprovacao": i["travas_aprovacao"]}

    def segura(c, dia, comp):
        ini, fim, mes, ano = comp
        universo = uma_vez(
            memo_dias, (c.empresa_id, mes, ano),
            lambda: dias_que_seguram(c.empresa, mes, ano),
        )
        itens = [i for i in universo["itens"]
                 if i["colaborador_id"] == c.pk]
        if not itens:
            return {"estado": "indeterminado",
                    "motivo": "sem item no universo", "segura": None}
        if c.pk not in universo["dias"]:
            if c.pk in universo["furo"]:
                raise RuntimeError("furo_sem_dias_carregados")
            return {"segura": False, "dias": [],
                    "escopo": "fora do universo furo_espelho",
                    "motivo_export": itens[0]["motivo"]}
        motivos, dias = universo["dias"][c.pk]
        encontrados = [x for x in dias if x["data"] == dia]
        return {"segura": bool(encontrados),
                "dias": encontrados, "motivos_competencia": motivos}

    def porta(c, dia):
        try:
            cel = CelulaDia.objects.only("pk").get(
                colaborador_id=c.pk, data=dia)
        except CelulaDia.DoesNotExist:
            return "sem celula"
        return {"celula_id": cel.pk,
                "porta": porta_alcancavel_do_dia(cel.pk)}

    def chamados(c, dia):
        encontrados, sem_dia, erros = [], [], []
        qs = ChamadoColaborador.objects.filter(
            colaborador_id=c.pk
        ).only("pk", "modulo_origem", "contexto_json", "status_local").order_by("pk")
        for ch in qs:
            try:
                raw = data_do_chamado(ch.modulo_origem, ch.contexto_json)
                if raw is None or raw == "":
                    sem_dia.append(ch.pk)
                    continue
                data = dt.date.fromisoformat(str(raw)[:10])
                if data == dia:
                    encontrados.append({
                        "id": ch.pk, "tipo": ch.modulo_origem,
                        "status_local": ch.status_local,
                    })
            except Exception as exc:
                erros.append({"chamado_id": ch.pk, **erro(exc)})
        saida = {"chamados": encontrados, "sem_dia_catalogado_ids": sem_dia}
        return {**saida, "ERRO": erros} if erros else saida

    with schema_context("juliani"), patch.object(
        export, "classificar_export", classificar_uma_vez
    ), patch.object(timezone, "now", return_value=agora):
        for uid, cid, iso in UNIDADES:
            linha = {"uid": uid, "col": cid, "dia": iso}
            if cid is None or iso is None:
                motivos = []
                if cid is None:
                    motivos.append("nao casado")
                if iso is None:
                    motivos.append("sem dia")
                emitir({**linha, "estado": "indeterminado",
                        "motivo": "; ".join(motivos)})
                totais["puladas"] += 1
                continue

            dia = dt.date.fromisoformat(iso)
            linha["instante_leitura"] = agora.isoformat()
            linha["data_futura_na_leitura"] = dia > hoje
            ok_c, c = tentar(lambda: Colaborador.objects.select_related(
                "empresa").get(pk=cid))
            if not ok_c:
                for letra in "abcdef":
                    linha[letra] = {**c, "dependencia": "colaborador"}
            else:
                ok_comp, comp = tentar(lambda: competencia(c, dia))
                chamadas = {
                    "a": lambda: status_do_dia_lavrado(c, dia, agora=agora),
                    "e": lambda: porta(c, dia),
                    "f": lambda: chamados(c, dia),
                }
                if ok_comp:
                    chamadas = {
                        **chamadas,
                        "b": lambda: dono(c, dia, comp),
                        "c": lambda: balde(c, comp),
                        "d": lambda: segura(c, dia, comp),
                    }
                for letra in "abcdef":
                    if letra not in chamadas:
                        linha[letra] = {**comp, "dependencia": "competencia"}
                    else:
                        ok, valor = tentar(chamadas[letra])
                        linha[letra] = valor

            if tem_erro(linha):
                totais["com_erro"] += 1
            else:
                totais["processadas"] += 1
            emitir(linha)

    soma = sum(totais.values())
    assert soma == 80
    emitir({"resumo": {**totais, "soma": soma}})

# Evita escrita pelos handlers de arquivo durante as chamadas.
# Configuracao da copia: app/config/settings/saas.py:138 e :162.
nivel_anterior = logging.root.manager.disable
try:
    logging.disable(sys.maxsize)
    executar()
finally:
    logging.disable(nivel_anterior)
''')
```
===FIM===

A ligação abaixo é **por classe sugerida**. As referências comprovam o escopo documental da obra; o enquadramento atual de cada unidade continua `[nao verificado]`. “Cond.” indica uma condição ainda não demonstrada. “Sem obra” significa ausência de associação sustentada ao recorte `backlog_obras.txt` fornecido. `[nao verificado]`

Preservo as três unidades não classificadas e a unidade futura do censo; a unidade futura continua elegível para chamadas na sonda porque possui colaborador e data. `[nao verificado]`

| uid | grupo | obra | prova |
|---|---|---|---|
| HJ-001-U1 | G2 | O261-E-CELULA-20-UM-DONO; F3-PORTA-E-CRON-PRAZO, cond. | `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:1739`; caso `[nao verificado]` |
| HJ-001-U2 | G2 | O261-E-CELULA-20-UM-DONO; F3-PORTA-E-CRON-PRAZO, cond. | `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:1739`; caso `[nao verificado]` |
| HJ-002-U1 | G2 | O261-E-CELULA-20-UM-DONO, cond. | `app/docs/BACKLOG.md:859`; caso `[nao verificado]` |
| HJ-003-U1 | G2 | O261-E-CELULA-20-UM-DONO, cond. | `app/docs/BACKLOG.md:859`; caso `[nao verificado]` |
| HJ-004-U1 | G3 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-004-U2 | G3 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U1 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U2 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-005-U3 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U4 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-005-U5 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-006-U1 | G1 | sem obra | Tela e natureza do “pendente” `[nao verificado]` |
| HJ-007-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-008-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-009-U1 | G1 | sem obra | Origem do cartão amarelo `[nao verificado]` |
| HJ-010-U1 | G1 | sem obra | Porta/tela concreta `[nao verificado]` |
| HJ-011-U1 | G1 | C20-FIO-MUDO, se faltar emissão de pergunta | `app/docs/BACKLOG.md:1762`; caso `[nao verificado]` |
| HJ-012-U1 | G5 | sem obra | Classe específica de QRF/saída `[nao verificado]` |
| HJ-012-U2 | G5 | sem obra | Decisão e eventual defeito de feriado `[nao verificado]` |
| HJ-013-U1 | G1 | sem obra | Tipo da retratação e tela `[nao verificado]` |
| HJ-013-U2 | G5 | sem obra | Decisão de feriado/diária `[nao verificado]` |
| HJ-013-U3 | G1 | sem obra | Tipo da retratação e tela `[nao verificado]` |
| HJ-014-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-014-U2 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-015-U1 | G4 | sem obra | Ato de ausência e elo sem efeito `[nao verificado]` |
| HJ-016-U1 | G3 | O276 | `app/docs/BACKLOG.md:1517`; estado atual `[nao verificado]` |
| HJ-016-U2 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U3 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U4 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U5 | G5 | O214 | `app/docs/BACKLOG.md:1134`; autorização individual `[nao verificado]` |
| HJ-017-U1 | G3 | O232 | `app/docs/BACKLOG.md:1157`; caso `[nao verificado]` |
| HJ-017-U2 | G3 | O232 | `app/docs/BACKLOG.md:1157`; dia individual `[nao verificado]` |
| HJ-018-U1 | G5 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; estado atual `[nao verificado]` |
| HJ-019-U1 | G3 | O259, quanto ao par | `app/docs/BACKLOG.md:1445`; divergência individual `[nao verificado]` |
| HJ-020-U1 | G5 | BO-ADMIN-2710, quanto à auditoria sem ação | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; caso `[nao verificado]` |
| HJ-020-U2 | G5 | BO-ADMIN-2710, quanto à auditoria sem ação | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; caso `[nao verificado]` |
| HJ-021-U1 | G1 | O267 | `app/docs/BACKLOG.md:1508`; dias responsáveis `[nao verificado]` |
| HJ-022-U1 | G1 | O267 | `app/docs/BACKLOG.md:1508`; dias responsáveis `[nao verificado]` |
| HJ-023-U1 | G5 | sem obra | Causa individual da diferença entre competências `[nao verificado]` |
| HJ-023-U2 | G5 | sem obra | Causa individual da diferença entre competências `[nao verificado]` |
| HJ-024-U1 | G1 | sem obra | Classe herdada do censo; contradição concreta `[nao verificado]` |
| HJ-025-U1 | G5 | H12-DEMISSAO-NA-JANELA | `app/docs/BACKLOG.md:1741`; data de desligamento `[nao verificado]` |
| HJ-026-U1 | G3 | O259, quanto ao par | `app/docs/BA

[truncado em 20000 de 23935 caracteres]

## 05/11 20:11 · sessao 7ebe33d2515d · H12H06 · verificador · origem=Code

A v7 fecha a guarda copiada, a falha durante `write` e a lacuna de teste da trava da entrega. Permanecem dois bloqueantes: **a limpeza pode apagar reversão de ato confirmado quando a consulta de confirmação falha**, e **o ciclo C→A/A→C continua alcançável**. (`app/ponto/services/ausencia.py:518`, `app/ponto/services/ausencia.py:585`, `app/ponto/services/ausencia.py:708`, `app/ponto/services/ausencia.py:744`; `app/ponto/views.py:2638`; `app/chamados/models.py:866`.)

Revisão estática, somente leitura. Execução da suíte, execução dos mutantes citados na triagem, identidade Git/hash, ocorrência dos interleavings e cumprimento dos requisitos em produção: **[nao verificado]**. [nao verificado]

**1. Bloqueantes da v6 e todas as entradas da triagem**

| Entrada | Situação em v7 e avaliação |
|---|---|
| PROCEDENTE 1 — guarda copiada; bloqueante 1 da v6 | **Fechado.** A reposição ficou privada por convenção, perdeu a guarda de motivo e tem um único chamador direto de produção localizado: a porta. Motivo e ator são exigidos nela. (`app/ponto/services/ausencia.py:518`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:630`, `app/ponto/services/ausencia.py:736`.) |
| PROCEDENTE 2 — arquivo registrado depois do `write`; parte do bloqueante 3 | **Fechado para a falha apontada.** O caminho entra em `escrito` antes de `write`; uma exceção em escrita ou fechamento já permite a limpeza. O teste provoca especificamente falha em `write`. (`app/ponto/services/ausencia.py:707`, `app/ponto/services/ausencia.py:708`, `app/ponto/services/ausencia.py:709`; `app/ponto/tests/test_h06_reversao_guardas.py:626`.) |
| PROCEDENTE 3 — exceção depois do commit; restante do bloqueante 3 | **Parcial.** A consulta bem-sucedida preserva o arquivo confirmado, mas qualquer `DatabaseError` vira `False` e autoriza apagá-lo. A afirmação da triagem de que esse erro significa necessariamente transação externa condenada não procede. (`app/ponto/services/ausencia.py:585`, `app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:744`.) |
| PROCEDENTE 4 — prova da trava da entrega; bloqueante 4 | **Fechado para o mutante apontado.** O teste agora chama `registrar` em outra conexão enquanto a porta segura a empresa e exige `lock timeout`. Trocar somente a trava de `registrar` por PK inexistente elimina essa espera. Resultado executado do mutante: **[nao verificado]**. (`app/folha/models.py:181`; `app/ponto/tests/test_h06_reversao_guardas.py:865`, `app/ponto/tests/test_h06_reversao_guardas.py:976`.) |
| PROCEDENTE 5 — trilha individual da reversão | **Fechado como cobertura estática do contraexemplo.** A reposição exige retorno da trilha; o teste novo faz somente as trilhas individuais falharem e cobra exceção e rollback dos minutos. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:534`; `app/ponto/tests/test_h06_reversao_guardas.py:603`.) |
| PROCEDENTE 6 — guarda de ator reduzida a `por is None` | **Fechado como cobertura estática.** O teste usa `User` sem PK, que distingue a guarda atual daquela versão enfraquecida. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:630`; `app/ponto/tests/test_h06_reversao_guardas.py:619`.) |
| NÃO CURADO — ciclo C→A/A→C; bloqueante 2 | **Aberto.** A porta mantém C→A; a aprovação mantém uma transação envolvendo o UPDATE de A e o signal que pode pedir C. Estar o lado da UI fora do delta não corrige a incompatibilidade. (`app/ponto/services/ausencia.py:662`, `app/ponto/services/ausencia.py:665`; `app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:1052`; `app/ponto/signals.py:147`; `app/chamados/models.py:866`.) |
| DESCARTE A — substituição externa do pathname | **Procede como não bloqueante demonstrado nesta obra.** A limpeza continua apagando por pathname, sem conferir identidade do arquivo. Um produtor que substitua esse caminho e sua ocorrência: **[nao verificado]**. (`app/ponto/services/ausencia.py:708`, `app/ponto/services/ausencia.py:746`.) |
| DESCARTE B — rollback de `atomic` externo | **Procede para os chamadores CLI atuais; não como garantia geral da função.** Eles não abrem transação externa. A porta, entretanto, aceita aninhamento e retorna sem vincular a remoção do arquivo a um rollback posterior do chamador. A alegação adicional sobre todo `DatabaseError` continua incorreta. (`app/ponto/management/commands/rederivar_falta_declarada.py:93`, `app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/management/commands/rederivar_falta_declarada.py:144`; `app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:657`, `app/ponto/services/ausencia.py:748`.) |
| DESCARTE C — tipos do arquivo e escapes AST | **Parcialmente procede.** Na reversão, bytes alterados precisam encontrar o mesmo hash na trilha antes de escrever; porém aplicar aceita o corpo fornecido pelo produtor e a validação não tipa todos os valores. O selo também continua sem acompanhar variáveis, `**kwargs` e conversões. Isso não demonstra um segundo produtor incorreto atual, mas impede chamar esses contratos de completos. (`app/ponto/services/ausencia.py:634`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:687`, `app/ponto/services/ausencia.py:693`; `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:30`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:43`.) |
| DESCARTE D — rateio, 480, cadastro atual, relógios e formato lista | **Procede como delimitação desta cura; não como fechamento desses pontos.** Permanecem rateio uniforme, limiar 480, corte atual, relógios internos e exigência de arquivo em dict. (`app/folha/export.py:121`; `app/ponto/turnos.py:2580`; `app/ponto/janelas.py:50`; `app/ponto/models.py:873`; `app/ponto/signals.py:122`; `app/ponto/services/ausencia.py:644`.) |
| DESCARTE E — requisitos do pouso | **Procede para separar construção de aplicação.** Não comprova autorização de pouso: as medidas/preservação exigidas continuam documentadas; cumprimento: **[nao verificado]**. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`.) |

**2. Arquivo e exceções**

`_ato_h06_confirmado` **lê a mesma trilha**: delega à mesma consulta por ação aplicada, Empresa e `valor_depois.hash` usada pela porta. Não encontrei cópia de juiz registrado nem outra persistência decidindo o consumo. Entretanto, a função acrescenta uma interpretação incorreta: **consulta indisponível = ato não confirmado**. (`app/ponto/services/ausencia.py:578`, `app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:687`.)

Além disso, a consulta verifica **visibilidade da linha nessa conexão**, não confirmação definitiva de uma transação externa. O nome e a docstring prometem mais do que a consulta estabelece quando há aninhamento. (`app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:586`, `app/ponto/services/ausencia.py:657`.)

| Caminho/interleaving | Consequência |
|---|---|
| `write` ou fechamento falha; rollback ocorre; consulta responde sem linha | O arquivo criado entra na limpeza e é apagado, se `unlink` funcionar. (`app/ponto/services/ausencia.py:708`, `app/ponto/services/ausencia.py:709`, `app/ponto/services/ausencia.py:744`, `app/ponto/services/ausencia.py:746`.) |
| Falha de trilha individual ou foto final divergente | A exceção sai do `atomic`; a limpeza consulta a trilha e remove o arquivo se a aplicação não estiver registrada. (`app/ponto/services/ausencia.py:732`, `app/ponto/services/ausencia.py:738`, `app/ponto/services/ausencia.py:741`.) |
| Commit confirma; callback levanta; consulta funciona | A linha aplicada é encontrada e o arquivo permanece. Esse é o caso escrito no teste novo. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:744`; `app/ponto/tests/test_h06_reversao_guardas.py:950`.) |
| **Commit confirma; ocorre exceção na saída; consulta da trilha também levanta `DatabaseError`** | **Pode apagar o arquivo do ato confirmado.** O helper devolve `False`, e o `except` executa `unlink`. Não há verificação de que o erro veio de uma transação ainda aberta ou condenada. Ocorrência: **[nao verificado]**. (`app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:744`, `app/ponto/services/ausencia.py:746`.) |
| Porta retorna normalmente dentro de transação externa; chamador depois desfaz | **Pode deixar arquivo sem ato confirmado.** A limpeza já não está em execução. Esse chamador não aparece nos caminhos CLI atuais. (`app/ponto/services/ausencia.py:657`, `app/ponto/services/ausencia.py:748`; `app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/management/commands/rederivar_falta_declarada.py:144`.) |
| Rollback ocorre, mas `unlink` levanta | **Pode deixar arquivo de ato não confirmado**, propagando a falha da limpeza no lugar da exceção original. Não há tratamento desse erro. Ocorrência: **[nao verificado]**. (`app/ponto/services/ausencia.py:741`, `app/ponto/services/ausencia.py:746`, `app/ponto/services/ausencia.py:747`.) |
| Arquivo já existia antes de `open('xb')` | A recusa acontece antes de registrar o caminho em `escrito`; esse caminho não manda apagar o arquivo existente. (`app/ponto/services/ausencia.py:707`, `app/ponto/services/ausencia.py:708`.) |

O consumo da reversão é **lógico pela trilha**: reverter lê o arquivo e lavra `h06_ato_revertido`; não o apaga. Uma cópia com bytes idênticos possui o mesmo hash e está sujeita ao mesmo consumo. (`app/ponto/services/ausencia.py:636`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:692`, `app/ponto/services/ausencia.py:710`.)

**3. Chamadores e guardas espelhadas**

Não localizei chamador direto de produção de `_repor_minutos_de_reversao` fora de `porta_do_ato_h06`. Os testes acessam o símbolo para interceptar a chamada; o underscore é uma convenção, não uma restrição de acesso em runtime. (`app/ponto/services/ausencia.py:518`, `app/ponto/services/ausencia.py:736`; `app/ponto/tests/test_h06_reversao_guardas.py:223`, `app/ponto/tests/test_h06_reversao_guardas.py:1004`.)

Não resta cópia externa das guardas **do ato H06** de motivo, ator, entrega, identidade, minutos esperados, empresa atual ou consumo. O comando resolve usuário, faz o censo e entrega ambas as direções à porta. A validação de tipo/minutos da edição comum continua sendo seu contrato preexistente; a exigência de trilha da reposição refere-se à linha individual, não duplica a decisão de consumo. (`app/ponto/management/commands/rederivar_falta_declarada.py:88`, `app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/management/commands/rederivar_falta_declarada.py:144`; `app/ponto/services/ausencia.py:480`, `app/ponto/services/ausencia.py:534`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:675`, `app/ponto/services/ausencia.py:679`, `app/ponto/services/ausencia.py:681`, `app/ponto/services/ausencia.py:692`.)

O ciclo ainda possível usa as **mesmas C e A**:

1. Uma reversão antiga referencia uma A que depois foi editada, rejeitada e reaberta, preservando o PK. As portas permitem essas mudanças. (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:1314`, `app/ponto/services/ausencia.py:1336`, `app/ponto/services/ausencia.py:1358`; `app/ponto/views.py:2731`.)
2. A UI aprova essa A dentro de `atomic`, executa seu UPDATE e pode alcançar, pelo signal/cartório/emissor, `ChamadoColaborador.abrir`, que pede C explicitamente. (`app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:1052`; `app/ponto/signals.py:85`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1777`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`.)
3. Entre o UPDATE de A e esse pedido de C, a reversão obtém E e C e espera A. A guarda que recusaria a identidade obsoleta só acontece depois da aquisição de A. (`app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:662`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:675`.)

Portanto, o `atomic` preserva o agrupamento das escritas, mas não estabelece ordem compatível entre os dois caminhos. Execução desse interleaving: **[nao verificado]**. (`app/ponto/services/ausencia.py:657`; `app/ponto/views.py:2638`.)

**4. Quem escreve e quem lê**

| Campo/valor | Quem produz ou escreve | Quem lê |
|---|---|---|
| `LogAuditoria.valor_depois.hash` | A porta calcula SHA-256 dos bytes e fornece `hash` a `evento`; o escritor efetivo de `valor_depois` é `evento`. (`app/ponto/services/ausencia.py:573`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:713`; `app/core/observ.py:134`.) | `_trilha_do_ato_h06`, tanto para aplicado/consumido quanto para a limpeza. (`app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:687`, `app/ponto/services/ausencia.py:692`.) |
| Ações `h06_ato_aplicado` / `h06_ato_revertido` | Constantes do serviço; a porta escolhe a ação e lavra dentro da transação. (`app/ponto/services/ausencia.py:550`, `app/ponto/services/ausencia.py:551`, `app/ponto/services/ausencia.py:657`, `app/ponto/services/ausencia.py:710`.) | As consultas de aplicação prévia e consumo; a limpeza consulta somente aplicado. (`app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:687`, `app/ponto/services/ausencia.py:692`.) |
| Payload de auditoria `{ato, arquivo, linhas}` | A porta fornece UUID, pathname e PKs; `evento` persiste. (`app/ponto/services/ausencia.py:713`; `app/core/observ.py:136`.) | A autorização de reversão consulta o **hash**, não compara esses três campos nem exige o pathname original. (`app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:693`.) |
| Nome e bytes do arquivo | O comando monta nome com empresa/competência/selo/PID/UUID; a porta serializa JSON ordenado, cria exclusivamente e escreve. (`app/ponto/management/commands/rederivar_falta_declarada.py:116`; `app/ponto/services/ausencia.py:634`, `app/ponto/services/ausencia.py:707`.) | A reversão lê bytes, interpreta JSON, valida forma e calcula hash; o tratamento de exceção pode apagar o pathname. (`app/ponto/services/ausencia.py:636`, `app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:746`.) |
| Corpo: `ato`, `empresa_id`, `mes`, `ano`, `mes_intacto` | UUID e parâmetros montados no comando. (`app/ponto/management/commands/rederivar_falta_declarada.py:118`.) | Empresa dirige trava/pertencimento; mes/ano dirigem entrega; mes intacto/ano dirigem fotos; UUID vai à trilha. (`app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:681`, `app/ponto/services/ausencia.py:704`, `app/ponto/services/ausencia.py:713`.) |
| Corpo: `pk`, identidade, `minutos_antes`, `grade` | O comando captura PK, identidade e minutos; `grade` é soma das respostas do juiz diário no censo. (`app/ponto/management/commands/rederivar_falta_declarada.py:61`, `app/ponto/management/commands/rederivar_falta_declarada.py:119`; `app/ponto/services/ausencia.py:549`, `app/ponto/services/ausencia.py:554`.) | PK seleciona A; identidade e minutos são guardas; grade é pós-condição de aplicar e estado esperado de reverter; minutos anteriores são repostos na volta. (`app/ponto/services/ausencia.py:652`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:675`, `app/ponto/services/ausencia.py:679`, `app/ponto/services/ausencia.py:725`, `app/ponto/services/ausencia.py:736`.) |
| `Ausencia.minutos` | Criação recebe valor; edição zera/atribui; `save` deriva a falta aprovada sem número pelo juiz diário; reposição atribui o anterior. (`app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:532`; `app/ponto/models.py:877`, `app/ponto/models.py:885`.) | Censo, guardas, pós-condição, trilhas individuais, rateio, financeiro e cobertura/apresentação. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/management/commands/rederivar_falta_declarada.py:69`; `app/ponto/services/ausencia.py:679`, `app/ponto/services/ausencia.py:725`, `app/ponto/services/ausencia.py:731`; `app/folha/export.py:121`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`; `app/ponto/turnos.py:2600`; `app/templates/ponto/ausencias.html:163`.) |
| `Ausencia.colaborador/tipo/data/data_fim/dias_corridos/status/chamado_origem` | A criação recebe esses insumos; edição altera tipo/período; a máquina escreve status. Materializadores fornecem tipo e chamado de origem. (`app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:352`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:1041`; `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`.) | Censo seleciona universo; identidade compara colaborador/período/tipo/status; dias são usados na edição e competências tocadas. `chamado_origem` não integra a identidade reconferida. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; `app/ponto/services/ausencia.py:549`, `app/ponto/services/ausencia.py:565`, `app/ponto/services/ausencia.py:675`, `app/ponto/services/ausencia.py:722`.) |
| Ator e motivo | O comando resolve usuário e passa opções; a porta exige PK e motivo e os entrega às trilhas. (`app/ponto/management/commands/rederivar_falta_declarada.py:88`, `app/ponto/management/commands/rederivar_falta_declarada.py:122`; `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:630`, `app/ponto/services/ausencia.py:712`.) | `evento` transforma o ator em usuário persistido; trilhas incluem descrição/motivo e antes/depois. (`app/core/observ.py:118`, `app/core/observ.py:134`; `app/ponto/services/ausencia.py:535`, `app/ponto/services/ausencia.py:731`.) |
| `ExportacaoDominio`: empresa/competência/usuário/nome/conteúdo/hash/linhas/reexportação | A view fornece nome e TXT; `registrar` calcula hash e indicador de reexportação e cria o registro sob trava da Empresa; `gerado_em` é automático. (`app/folha/views.py:280`, `app/folha/views.py:297`; `app/folha/models.py:80`, `app/folha/models.py:178`, `app/folha/models.py:181`, `app/folha/models.py:186`.) | O juiz de entrega consulta empresa/competência e vigência; download lê conteúdo/nome. A porta H06 usa esse juiz, sem escrever ED. (`app/folha/export.py:598`; `app/folha/views.py:554`, `app/folha/views.py:561`; `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:684`.) |
| ED: invalidação/motivo/substituta | `invalidar` escreve os três campos; não adquire a nova trava da Empresa nesse caminho. (`app/folha/models.py:143`, `app/folha/models.py:158`, `app/folha/models.py:162`.) | Vigência/juiz de entrega consideram os registros e a invalidação do vigente. (`app/folha/models.py:134`; `app/folha/export.py:598`, `app/folha/export.py:600`.) |
| Foto da competência intacta | O comando `carimbo_gravado` lê campos de `FechamentoMensal` e produz o dict; H06 não escreve esse fechamento. (`app/ponto/management/commands/carimbo_gravado.py:34`, `app/ponto/management/commands/carimbo_gravado.py:53`; `app/ponto/services/ausencia.py:722`, `app/ponto/services/ausencia.py:736`.) | A porta compara fotos; o comando imprime hashes delas. Essa foto não lê bytes de `ExportacaoDominio.conteudo`. (`app/ponto/services/ausencia.py:704`, `app/ponto/services/ausencia.py:738`; `app/ponto/management/commands/rederivar_falta_declarada.py:130`.) |

No restante do diff, o mapa passa a produzir somente tipos; o predicado compartilhado produz `rescisao`; a lista produz `cobre_parte`. Seus leitores são, respectivamente, materialização, contador/HTML e duração no template. (`app/chamados/models.py:3007`; `app/chamados/services/materializacao.py:468`; `app/folha/export.py:60`; `app/ponto/services/fechamento_lista.py:109`; `app/ponto/views.py:1215`; `app/templates/po

[truncado em 20000 de 26926 caracteres]

## 05/11 20:04 · sessao 573c13515fd0 · F-346 · censo · origem=Code

HASH LIDO = e9f3ab0e21bb353456cb8abd99ffa97af696ff04

## 1 QUEM ESCREVE e QUEM LÊ

**Limite da evidência.** O hash acima identifica a cópia indicada no pedido; sua confirmação independente como objeto Git é [nao verificado]. O dado fornecido não contém registros de agendamentos, ausências, células ou exportações; ocorrência, quantidade e colaboradores afetados em produção são [nao verificado].

**Achado central:** a remarcação grava as novas datas sem apagar as ausências da janela anterior. Seu `save()` chama o espelhamento, mas esse espelhamento consulta somente a janela **nova**; portanto não alcança as linhas antigas que ficaram fora dela. A correção administrativa possui essa limpeza explícita. Isso sustenta o defeito por leitura estática; sua ocorrência em produção é [nao verificado]. (`app/ferias/services.py:374–387`; `app/ferias/services.py:393–409`; `app/ferias/models.py:434–437`; `app/ponto/services/ausencia.py:1165–1169`, `1195–1198`.)

**Escritores das datas de `AgendamentoFerias`.** Os nomes reais são `data_inicio`, `data_fim` e `dias`; a associação à pessoa passa por `periodo_aquisitivo`. (`app/ferias/models.py:299–304`.)

| Escritor efetivo | Campos escritos e entradas encontradas |
|---|---|
| `registrar_gozo_historico` | Cria início/fim recebidos, `dias=fim−início+1`, status pelas datas e autoria; salva e atualiza concedidos/vendidos do período aquisitivo. Entrada: criação de período com gozo histórico. (`app/ferias/services.py:270–303`; `app/ferias/views.py:190–209`.) |
| `executar_agendamento` | Calcula fim inclusivo a partir de início/dias; cria início, fim, dias, aviso, autoria e observação. Bloqueia duplicata por período aquisitivo/início, excluindo cancelados. Entradas: individual, lote e `agendar_ferias_porta`, também utilizada pelo intake/chat. (`app/ferias/services.py:306–327`, `128–165`; `app/ferias/views.py:259`, `298`, `607–617`; `app/core/intake.py:32–34`; `app/core/views_ia.py:668–684`.) |
| `corrigir_datas_agendamento` | Substitui início/fim/dias, recalcula aviso/status e concedidos do período; apaga o trecho antigo fora do novo antes do `save()` completo. Entrada: `editar_datas_admin`. (`app/ferias/services.py:366–389`; `app/ferias/views.py:489–490`.) |
| `remarcar_agendamento` | Substitui início/fim/dias, recalcula aviso/status e concedidos; salva com `update_fields`. Não chama o apagador da janela antiga. Entrada: `editar_agendamento`, incluindo a chamada apontada na obra. (`app/ferias/services.py:393–409`; `app/ferias/views.py:633–668`.) |
| `aprovar_solicitacao` | Cria agendamento com início/fim desejados e dias da solicitação, aviso e autoria; executa `full_clean()` e `save()`. Entrada: avaliação da solicitação pendente, cuja view trava a solicitação com `select_for_update`. (`app/ferias/services.py:413–450`; `app/ferias/views.py:712–729`.) |

O `save()` do modelo executa `_guard_sobreposicao`, grava e chama `_sync_ausencia`; não executa `clean()`/`full_clean()` nesse método. A guarda pula cancelado, atualização sem datas e datas efetivamente inalteradas; quando há mudança, delega ao veto de precedência. (`app/ferias/models.py:387–437`.)

Na edição, a guarda exclui do veto **todas** as ausências de férias da pessoa cuja `data` esteja no intervalo antigo. A seleção não verifica se a descrição contém o identificador daquele agendamento. (`app/ferias/models.py:415–430`.)

**Outros escritores que interferem no espelho, embora não escrevam datas:**

- Cancelamento DP e cancelamento administrativo escrevem `status='cancelado'`; o administrativo também rejeita as ausências da janela antes de salvar. O `save()` dispara o mesmo espelhamento. (`app/ferias/services.py:330–362`; `app/ferias/models.py:434–437`.)
- `alinhar_status_por_data`, chamado pelo comando `sincronizar_status_ferias`, escreve somente status; o comando exige `--apply` para aplicar e exclui cancelados. (`app/ferias/services.py:518–522`; `app/ferias/management/commands/sincronizar_status_ferias.py:20–44`.)
- Pedido de confirmação e resposta do colaborador salvam confirmação/carimbo e também passam pelo `save()` do modelo. (`app/ferias/services.py:487–505`; `app/ferias/models.py:434–437`.)
- O Django admin de férias registra `Afastamento`; a porta indicada na obra é uma view administrativa da aplicação. A API de férias examinada consulta e devolve agendamentos. Outro escritor dinâmico de datas por API/signal/command é [nao verificado]. (`app/ferias/admin.py:2–14`; `app/ferias/views.py:633–668`; `app/api/views.py:2237–2270`.)

**Quem chama `apagar_ausencias_de_ferias`, com quais guardas.** A função real está em **`ponto/services/ausencia.py`**, importada pela irmã em `ferias/services.py`. Ela filtra colaborador, tipo `ferias` e `data` inclusiva; `exceto` preserva a janela informada. Não verifica status, autoria, descrição, origem do agendamento ou competência. (`app/ponto/services/ausencia.py:1246–1254`; `app/ferias/services.py:372`.)

| Chamador encontrado | Guarda e alcance |
|---|---|
| `corrigir_datas_agendamento` | Guarda intervalo anterior, executa em `atomic` e passa `exceto=(nova_ini,nova_fim)`. Sua view exige POST, permissão `editar_ferias`, agendamento não cancelado, justificativa de pelo menos cinco caracteres, dias positivos e saldo. O serviço não repete essas guardas da view. (`app/ferias/services.py:366–389`; `app/ferias/views.py:437–469`.) |
| `AgendamentoFerias.delete` | Apaga férias do intervalo atual, sem `exceto`; o método não acrescenta guarda de status/permissão. (`app/ferias/models.py:439–443`.) |

A remarcação tem outra guarda de entrada: POST, permissão, status exatamente `agendado`, dias positivos e saldo. A view testa status, sem consultar `pode_editar`/`ja_iniciou`; essas propriedades também consideram a data atual. (`app/ferias/views.py:636–665`; `app/ferias/models.py:321–328`.)

**A remarcação cria a ausência nova?** Ela **tenta materializar os dias novos** por `remarcar_agendamento → AgendamentoFerias.save → _sync_ausencia → espelhar_ferias → criar_ausencia`. Cada dia ausente nasce como `tipo='ferias'`, aprovada, um dia, zero minutos, autoria do agendamento e descrição com seu identificador. Dias já encontrados são mantidos; dias recusados não nascem e geram Pauta DP. Portanto “criou todos os dias novos” depende dos dados e é [nao verificado]. (`app/ferias/services.py:406–407`; `app/ferias/models.py:379–385`, `434–437`; `app/ponto/services/ausencia.py:1182–1200`, `341–349`, `385–387`.)

**Campos materializados e seus leitores.**

| Registro/campos | Escrita e leitura atuais |
|---|---|
| Período aquisitivo: `dias_concedidos`, `status` | Correção e remarcação substituem a contribuição dos dias antigos pela dos novos e reaplicam status; saldo de remarcação é consultado pela view. (`app/ferias/services.py:376–389`, `400–409`; `app/ferias/views.py:660–663`; `app/ferias/models.py:216–222`.) |
| Ausência: pessoa, tipo, `data`, `data_fim`, `dias_corridos`, minutos, status, autoria e descrição | Espelhamento cria por dia; cobertura lê pessoa/tipo/status/datas e desempata por data/PK. A ausência também permite intervalo próprio. (`app/ponto/services/ausencia.py:1186–1191`; `app/ponto/models.py:743–774`; `app/ponto/turnos.py:2509–2557`, `2753–2769`.) |
| Grade: `tipo_dia`, `ausencia_tipo`, `ausencia_label`, `ausencia_id` | `_marcar_ausencias` lê cobertura aprovada e preenche esses campos. (`app/escala/utils.py:1190–1244`.) |
| Célula: `ausencia`, impressão/ata e precedência | Cartório lê cobertura de cobrança e carimba precedência separadamente; o leitor da célula lê o resultado lavrado. A FK da ausência usa `SET_NULL`. (`app/ponto/services/cartorio.py:1303–1348`, `1935–1957`; `app/escala/services/leitor_celula.py:380–412`; `app/escala/models.py:1792–1798`.) |

**Leitores de Ausência de férias — caminhos de negócio e apresentação.**

| Área | Leitores e uso |
|---|---|
| Cobertura e conflito | `_pelo_efeito`, `ausencias_do_periodo`, `ausencia_cobre`, `cobertura_ausencia_periodo`, `dias_da_ausencia`, `ausencias_sobrepostas` e `ausencias_vigentes` consultam/expandem as linhas; precedência e checagem de lançamento usam esse universo. (`app/ponto/turnos.py:2509–2557`, `2733–2815`, `3071–3087`; `app/ponto/precedencia.py:443–462`; `app/ponto/services/checar_ausencia.py:19–36`.) |
| Espelho | Carrega cobertura para cobrança e para folha, decisões e fatos; usa ausência/ícone e palavra do dia. (`app/ponto/services/espelho.py:579–588`, `646–647`, `845–872`; `app/ponto/services/dia_decidido.py:332–364`; `app/templates/ponto/espelho.html:165`, `191`.) |
| PDF do espelho | Usa o serviço do espelho e seus dias; também consome efeitos de ausência para cálculo/apresentação. (`app/relatorios/pdf_espelho.py:332`, `406`, `596–602`.) |
| Calendário e grade | Calendário consulta cobertura de cobrança, cobertura de folha e decisões; a grade marca ausências aprovadas. O template lê o objeto/rótulo recebido. (`app/colaboradores/services/calendario.py:484`, `531–532`; `app/escala/utils.py:1177–1244`; `app/templates/colaboradores/partials/_calendario_grade.html:97`.) |
| Célula e cartório | Cartório consulta cobertura, detecta fato novo e grava a precedência; sinais de salvar/apagar ausência acionam julgamento. O suprajuiz compara cobertura, realizado e cobrança. (`app/ponto/services/cartorio.py:355`, `1306`, `1947–1956`; `app/ponto/signals.py:115–152`; `app/ponto/supra_juiz.py:400`, `531`.) |
| Motor e fechamento | `efeitos_da_ausencia` consulta aprovadas e cobertura por dia. Fechamento monta conjuntos/mapas e alimenta o motor; o motor usa o mapa de ausência integral para zerar atraso/saída antecipada. Espelho e PDF também alimentam esse mapa. (`app/ponto/services/efeito_ausencia.py:28–49`; `app/ponto/services/fechamento.py:629`, `664–692`, `742`; `app/ponto/motor_calculo_v2.py:805`, `1085–1092`; `app/ponto/services/espelho.py:1133`; `app/relatorios/pdf_espelho.py:406`.) |
| Folha/exportação | `ausencias_na_janela`, expansão de datas e rateio leem aprovadas por tipo/interseção. Férias ficam fora da rubrica 8932; a retenção de férias com batida usa diretamente **AgendamentoFerias**, por `ferias_na_janela`. (`app/folha/export.py:56–107`, `135–195`, `453–480`; `app/ponto/catalogo/ausencias.py:290`.) |
| Chamados: materialização e reconciliação | `_coberto_por_ausencia` lê a cobertura vencedora; o reconciliador usa essa composição. Devolução de perguntas também consulta cobertura. (`app/chamados/services/materializacao.py:55–81`, `627–642`; `app/chamados/reconciliador.py:822`.) |
| Chamados: juízes e silêncio | `_celula_furada`, seleção da via da disputa, `reconciliar_por_ausencia`, vigia do fio e adesão consultam cobertura; reavaliação de feriado consulta precedência. (`app/chamados/juizes.py:431–448`, `1474`; `app/chamados/services/silencio_ausencia.py:51–77`; `app/chamados/services/vigia_fio.py:299–308`; `app/chamados/services/adesao.py:52–68`; `app/chamados/services/feriado_reavaliar.py:27–33`.) |
| Chamados: evento e histórico | Sinais reconciliam silêncio/devolução/fechamento após commit; histórico lê tipo, data e registrador. (`app/ponto/signals.py:250–284`; `app/chamados/services/historico.py:118–134`; `app/templates/chamados/fio_colaborador.html:60–62`.) |
| Telas de ausência e decisão | Painel de fechamento, painel de justificativa, listagem, consulta por registro, ações de decisão/documento e linha do tempo leem as ausências. (`app/ponto/views.py:1002–1012`, `1680–1682`, `2015–2034`, `2077`, `2180–2188`, `2272`, `2306`, `2335`, `2573`, `2596–2606`, `2660–2679`, `2718`, `2751`; `app/ponto/services/dia_decidido.py:521–615`.) |
| API/app | Espelho v2 usa cobertura; GET de ausências lista as últimas trinta; documento consulta a ausência da pessoa. Mensageria também consulta cobertura para disponibilidade. (`app/api/views.py:1574–1584`, `1861–1889`, `3092`; `app/api/views_mensageria.py:1215–1243`.) |
| Relatórios e histórico em lote | Absenteísmo lê aprovadas por interseção e expande os dias; lote/histórico usa `linha_do_tempo`. O relatório médico usa seleção de tipos médicos, cuja inclusão concreta de férias no cadastro vivo é [nao verificado]. (`app/relatorios/views.py:197–215`, `277–292`; `app/ponto/services/atestados_lote.py:54–57`; `app/ponto/services/dia_decidido.py:558–585`.) |
| Inteligência e detalhe da pessoa | Painel, ranking e PDF consultam ausências por data; “sem ausência” usa a relação reversa. Resumo e detalhe mostram registros recentes. Scores/alertas consultam ausências, excluindo rejeitadas e tipos abonados. (`app/inteligencia/views.py:71–84`, `232–243`, `516–539`; `app/inteligencia/resumo.py:39–45`; `app/colaboradores/services/detalhe.py:104–107`; `app/inteligencia/management/commands/calcular_scores.py:7–16`; `app/inteligencia/management/commands/gerar_alertas.py:55–61`; `app/templates/colaboradores/detalhe.html:63–72`.) |
| Vigias e outros consumidores | Contadores de ausência, ausência versus veredito, deslogados, faltas de hoje, furos diários e cadastro versus realidade consultam as linhas ou cobertura. Admin de Ausência é somente leitura; contagem por tipo também lê o acervo. (`app/ponto/services/vigia_ausencia.py:59–120`, `215–220`; `app/ponto/services/vigia_ausencia_veredito.py:46–53`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`; `app/escala/services/furos_diarios.py:222`; `app/escala/services/cadastro_realidade.py:550`; `app/ponto/admin.py:37–48`; `app/ponto/views_tipos_ausencia.py:24`.) |

**Leitores em comandos**, além dos caminhos anteriores: detecção, alertas, reconciliação, auditoria, backfill e diagnóstico consultam cobertura/ausência nos seguintes sítios. (`app/ponto/management/commands/detectar_ausencias.py:812–815`; `app/ponto/management/commands/processar_alertas_turno.py:97`; `app/ponto/management/commands/processar_alertas_avancados.py:115`; `app/ponto/management/commands/reconciliar_grade.py:401`; `app/ponto/management/commands/supra_juiz.py:216`; `app/ponto/management/commands/vigia_b5_ausencia.py:68`; `app/ponto/management/commands/backfill_b5_insumos.py:72`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`; `app/ponto/management/commands/diff_calculador.py:408`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `642`, `659`, `1466`.)

Também leem ausência os comandos de reabertura por ausência aberta, rejulgamento em folga, ausência sem efeito na célula, bloqueante sem fim, rejulgamento de status por tipo, cura de fim e contagem para zeragem. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:33`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41–44`; `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27–29`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:23–32`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–31`; `app/ponto/management/commands/zerar_dados_operacionais.py:49`.)

**Leitores das datas do agendamento, distintos dos leitores de Ausência:** consultas de férias hoje/janela/início e divergência de status; situação/avisos; listagem, saldo, seção da pessoa e confirmações; disponibilidade/retorno; atendimento; API/mensageria; bloqueio/aviso de batida; folha; sincronização, tripwire e alertas de retorno. (`app/ferias/services.py:47–108`; `app/ferias/situacao.py:115–117`, `138–164`, `246–280`, `304–343`; `app/ferias/views.py:14–73`, `98–111`, `745–749`, `830–891`; `app/colaboradores/views.py:2843–2853`, `2895–2896`; `app/chamados/views.py:626–629`; `app/api/views.py:2251–2270`; `app/api/views_mensageria.py:900–901`, `1045–1046`; `app/ponto/utils.py:88–89`, `197–198`; `app/ponto/services/aviso_da_batida.py:73–74`; `app/folha/export.py:473–480`; `app/ferias/management/commands/sincronizar_status_ferias.py:33–44`; `app/ferias/management/commands/tripwire_status_ferias.py:27–29`; `app/ferias/management/commands/alertar_retornos.py:20`.)

## 2 JUIZ ou LEI

O lookup manda consultar o registro vivo; o registro distingue cobertura, precedência, férias hoje, saldo, fracionamento e aviso. A regra específica F-346 disponível é a frase do pedido; registro documental adicional desse aval na cópia é [nao verificado]. (`app/docs/JUIZES.md:10–16`; `app/core/juizes.py:239–257`.)

| Pergunta | Autoridade atual |
|---|---|
| Que ausência cobre o dia? | `ausencia_cobre` e sua irmã por período, `cobertura_ausencia_periodo`. Para folha, somente aprovada; para cobrança, não rejeitada. Bordas inclusivas; vence maior `(data,pk)`. (`app/core/juizes.py:178–179`, `241`; `app/ponto/turnos.py:2509–2557`, `2733–2769`.) |
| Qual fato vence o dia? | `fatos_do_dia → fatos_do_periodo`, que lê Ausência não rejeitada e aplica precedência. Essa leitura não confere as datas atuais de `AgendamentoFerias`. (`app/core/juizes.py:174`; `app/ponto/precedencia.py:242–279`, `335–396`.) |
| A pessoa está de férias hoje? | `em_gozo_hoje → ferias_na_janela`: lê **AgendamentoFerias** pelas datas e exclui cancelado, inclusive quando o status temporal está atrasado. (`app/core/juizes.py:250`; `app/ferias/services.py:47–68`.) |
| O lançamento pode entrar sobre outro fato? | `veto_de_lancamento`, chamado pelo `save()` do agendamento; exclusões de ausências antigas são fornecidas pelo modelo. (`app/core/juizes.py:242`; `app/ferias/models.py:415–437`; `app/ponto/precedencia.py:435–469`.) |

**Para o espelho há duas respostas efetivamente consumidas:** cobertura de cobrança para ausência/ícone e cobertura de folha para o veredito exibido. `veredito_do_dia` prioriza cobertura aprovada antes dos fatos de precedência. (`app/ponto/services/espelho.py:582–588`, `845–872`; `app/ponto/services/dia_decidido.py:332–364`.)

**Para a célula também há respostas distintas:** a grade marca cobertura de folha; o cartório associa cobertura de cobrança e, separadamente, grava a resposta de `fatos_do_dia` em `ata.precedencia`. O leitor da célula consulta a ata lavrada. Logo não cabe reduzir todos esses campos a uma consulta única de `em_gozo_hoje`. (`app/escala/utils.py:1195–1244`; `app/ponto/services/cartorio.py:1306`, `1935–1957`; `app/escala/services/leitor_celula.py:380–412`; `app/ferias/services.py:57–68`.)

A competência vem do corte cadastrado da empresa; corte 21 produz janela de 21 a 20. Para competência exportada, a regra consolidada preserva o `FechamentoMensal` contra caminho genérico e reserva a alteração à porta de exceção com suas provas; “entregue/importada/paga” em um caso concreto é [nao verificado]. (`app/ponto/janelas.py:10–21`, `36–63`; `app/docs/LEIS.md:123`, `125`; `CLAUDE.md:398–414`.)

## 3 CONTRA-EXEMPLOS PELA REGRA DO AVAL

- **Borda:** quando início novo coincide com fim antigo, esse dia pertence às duas janelas. A irmã o preserva pelo `exceto` inclusivo; uma afirmação de que “todo o antigo desaparece” contrariaria essa função. (`app/ponto/services/ausencia.py:1250–1254`.)
- **Sobreposição parcial ou redução:** o espelhamento mantém linhas existentes no novo intervalo, mas a remarcação não alcança as pontas antigas retiradas. O problema é o trecho antigo fora do novo, não a mera existência de linhas na interseção. (`app/ponto/services/ausencia.py:1165–1184`; `app/ferias/services.py:393–409`.)
- **Depois de competência 21→20 entregue:** remarcação e apagador não consultam exportação; espelhamento declara que período fechado não recusa a materialização. Isso não demonstra alteração automática do fechamento/TXT, sujeita à regra própria. (`app/ferias/services.py:393–410`; `app/ponto/services/ausencia.py:1152–1153`, `1246–1254`; `CLAUDE.md:403–414`.)
- **Ausência de férias editada à mão:** se sua `data` estiver no trecho antigo retirado, a irmã a seleciona mesmo com autoria, descrição, status ou fim diferentes. Se tiver virado um intervalo maior, o apagador continua selecionando pela data inicial, sem recortar sua cobertura. Preservação especial de lançamento manual é [nao verificado]. (`app/ponto/services/ausencia.py:1250–1254`; `app/ponto/turnos.py:2552

[truncado em 20000 de 28558 caracteres]

## 05/11 20:03 · sessao ee623b37cf5a · - · - · origem=Ronald (tmux)

Provas de cobertura na cópia, todas em `app/docs/BACKLOG.md`:

| Classe | Obra/documentação | Linha |
|---|---|---:|
| Tela × status do dia | E4-CALENDARIO-L1 | 822 |
| API do app × status | E4-APP-MAPA | 851 |
| Aprovação × entrada no TXT | O252 | 1440 |
| Retrato, filtros e fonte dos números | O256 | 1066 |
| Dias que seguram a folha | O267 | 1508 |
| Pergunta viva sob chamado terminal | O261-E-CELULA-20-UM-DONO | 859 |
| Porta alcançável | O261-E-F5B-JUIZ-PORTA-ALCANCAVEL | 860 |
| col292, 21/09 | O261-COMPLEMENTO-[nome] | 782 |
| Fio morto, incluindo col446 | O261-FIO-MORTO-PELO-RETRATO | 963 |
| Indefinida e dono CADASTRO | BO-[nome]-DESTRAVA | 968 |
| Beco de folga e âncora | F3-PORTA-E-CRON-PRAZO | 1739 |
| Rejulgamento do mês e reatamento na fila | F3-BECO-1-VOLTA-3 | 1764 |
| Emissão de pergunta de batida ausente | C20-FIO-MUDO | 1762 |
| col923, borda transnoite | O232 | 1157 |
| col297, 21/09, par acima do teto | O276 | 1517 |
| Juiz do par e horas | O259 | 1445 |
| Vínculo válido no dia | O266 | 1507 |
| col33, 03/10 | ATA-X-TURNO | 1651 |
| col288, 04/10 | ATA-X-TURNO-ENTRADA | 1652 |
| col247, 24/10 | JUIZ-DO-PAR-DUAS-ENTRADAS | 753 |
| col42, 08/15/22-10; col789, 11/10 | COBERTURA-PARCIAL-UM-JUIZ | 763 |
| Resposta/materialização, col94 e col129 | FIO-LE-O-JUIZ | 771 |
| Pendência de auditoria; col130, 05/10 | REGISTRADO-NAO-E-VIVO | 757 |
| Feriado e vínculo antigo | BO-FERIADO-VINCULO-ANTIGO | 1451 |
| Decisão/exibição de HE | O214 | 1134 |
| Demissão fora da janela | H12-DEMISSAO-NA-JANELA | 1741 |
| Achados administrativos de col42/789/129/247/130 | BO-ADMIN-2710 | 754 |

O recorte `backlog_obras.txt` do prompt não contém linhas próprias para E4-CALENDARIO-L1, ATA-X-TURNO, JUIZ-DO-PAR-DUAS-ENTRADAS, COBERTURA-PARCIAL-UM-JUIZ, FIO-LE-O-JUIZ e REGISTRADO-NAO-E-VIVO. Se a coluna obra exigir exclusivamente itens literalmente listados nesse recorte, usar uma obra presente que cubra a classe ou “sem obra”. [nao verificado]

Desdobramentos importantes: HJ-004-U1 sugere G4 pela ocorrência nominal de 11/10; HJ-004-U2 sugere G3 pelo ciclo/marcos. HJ-016-U1 sugere G3 pela ocorrência nominal de 21/09; U2–U5 sugerem G5. As associações novas por UID continuam inferências do texto fornecido. `app/docs/BACKLOG.md:763`; `app/docs/BACKLOG.md:1517`; [nao verificado]

HJ-050-U2 deve permanecer entre as 80 unidades da sonda: possui colaborador e data literal, embora o censo anterior a tenha excluído como futura. HJ-056-U2 pula chamadas pela identidade não casada, mas a classe textual ainda sugere G5. [nao verificado]

## 05/11 20:03 · sessao 813f6cff7bff · - · - · origem=Ronald (tmux)

- `classificar_export(empresa, mes, ano, fechamentos=None)` retorna lista de itens com `colaborador_id`, `status`, `motivo`, `detalhe`, `travas_aprovacao`; filtra os fechamentos gravados por empresa/competência. `app/folha/export.py:746`; `app/folha/export.py:751`; `app/folha/export.py:766`
- O balde deve sair de `status` e `motivo` do item filtrado pelo ID. Ausência de item precisa aparecer como “sem item no universo”, pois o universo parte de `FechamentoMensal`, não de todos os colaboradores. `app/folha/export.py:766`; `app/folha/export.py:815`
- Os motivos de exclusão, na ordem, são `rescisao_modulo_proprio`, `sem_codigo_dominio`, `ferias_com_batida`, `cadastro_zero`, `furo_espelho`, `nao_homologado`; caso contrário, `status="entra"`. `app/folha/export.py:819`; `app/folha/export.py:840`; `app/folha/export.py:846`; `app/folha/export.py:850`; `app/folha/export.py:933`; `app/folha/export.py:942`; `app/folha/export.py:948`
- `dias_que_seguram(empresa, mes, ano)` **já chama** `classificar_export` e devolve os mesmos itens em `retorno["itens"]`. Usar essa lista para c evita uma segunda chamada real do classificador. `app/folha/fora_da_folha.py:102`; `app/folha/fora_da_folha.py:119`; `app/folha/fora_da_folha.py:138`
- `retorno["dias"][col_id]` contém `(motivos, lista_de_dias)` para os colaboradores cujo motivo do export é `furo_espelho`. Cada dia contém `data`, `motivo`, `veredito`, `celula_id`; a comparação é `item["data"] == dia`. `app/folha/fora_da_folha.py:128`; `app/folha/fora_da_folha.py:137`; `app/folha/export.py:1167`; `app/folha/export.py:1203`
- Para c e d terem tratamento independente sem repetir o classificador, pode capturar/memorizar em memória a execução original de `classificar_export`, incluindo exceção, e fazer a importação interna de d ler essa mesma chamada por `patch.object`; nenhuma escrita em cache persistente é necessária. A possibilidade técnica decorre da importação local em d. `app/folha/fora_da_folha.py:115`; `app/folha/fora_da_folha.py:119` [nao verificado quanto à execução da sonda]
- A competência correta vem de `ini, fim = janela_atual(dia, empresa)`, depois `mes=fim.month`, `ano=fim.year`; ela respeita o corte efetivo da empresa. `app/ponto/janelas.py:60`; `app/ponto/janelas.py:63`
- `21→20` ocorre quando `corte_da_empresa(empresa) == 21`; o código admite corte de 2 a 28. Para não afirmar corte não medido, a sonda pode exigir `corte==21` e imprimir erro se divergir. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:50`
- As implementações lidas de c e d fazem consultas e constroem resultados em memória; c se declara leitura e d se apresenta como ferramenta de leitura. Garantia transitiva de todos os importadores e configurações de logging permanece `[nao verificado]`. `app/folha/export.py:747`; `app/folha/fora_da_folha.py:2`; `app/folha/fora_da_folha.py:118`; `app/folha/fora_da_folha.py:131`
- Atenção à privacidade: os itens crus de c incluem `nome` e `codigo_dominio`; filtre os campos antes de imprimir. `app/folha/export.py:815`; `app/folha/export.py:817`

## 05/11 20:03 · sessao 782dba7830af · - · - · origem=Ronald (tmux)

Assinaturas conferidas:

- `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)`: `app/ponto/management/commands/e6_oraculo.py:53`.
- `InsumosDoDono.__init__(self, colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`: `app/ponto/management/commands/e6_oraculo.py:128`.
- `InsumosDoDono.dono(self, data)`: `app/ponto/management/commands/e6_oraculo.py:172`.

O caminho exato usado pelo comando é este; `espelho_do_colab` também tem assinatura conferida em `app/ponto/services/espelho.py:1194`:

```python
# app/ponto/management/commands/e6_oraculo.py:304
esp = espelho_do_colab(c, HOJE, ini, fim, agora=AGORA)
# app/ponto/management/commands/e6_oraculo.py:316
ins = InsumosDoDono(c, ini, fim, HOJE, espelho=esp, agora=AGORA)
# app/ponto/management/commands/e6_oraculo.py:366
dono = ins.dono(dia)
```

Insumos reais, sem composição alternativa:

- Horas atribuídas pelo oráculo: `batidas_apuraveis` no envelope `ini 00h −12h` até `fim+1d 00h +14h`, com `minutos_do_oraculo`; `app/ponto/management/commands/e6_oraculo.py:142`.
- Turnos: `turnos_do_colab(c, ini−1d, fim+1d)`, carregado sob demanda; `app/ponto/management/commands/e6_oraculo.py:148`.
- Cadastro × realidade: `resultado.dias_cadastro_x_realidade` do espelho passado; se ausente, o próprio carregador chama `espelho_do_colab`; `app/ponto/management/commands/e6_oraculo.py:154`.
- Fase: `dias_de_fase_as_of(c.pk, c.empresa_id, ini, fim, hoje)`, leitura das lavras que cobriram os dias; `app/ponto/management/commands/e6_oraculo.py:164`; `app/escala/services/fase_12x36.py:117`.
- Cadastro prioritário: `cadastro_do_dia_carregado(c.pk, data)`; `app/ponto/management/commands/e6_oraculo.py:173`; `app/ponto/supra_juiz.py:166`.
- Batida fora de turno: lê `realizado_do_dia(..., turnos=turnos).aberto` e confirma entrada órfã no universo apurável; `app/ponto/management/commands/e6_oraculo.py:186`.

A chamada `ins.dono(dia)` já encaminha esses insumos a `dono_da_divergencia`; não montar seus argumentos manualmente nem substituir horas por batidas do dia civil. `app/ponto/management/commands/e6_oraculo.py:175`

O caminho real do espelho passa por `autoridade_do_periodo`, que monta o motor em memória; portanto, “nunca recalcule” deve ser atendido sem inventar cálculo paralelo nem chamar portas de recálculo persistente, preservando a chamada exigida do E6. `app/ponto/services/espelho.py:1227`; `app/ponto/services/espelho.py:971`

Não executei consultas ao banco nem alterei arquivos. [nao verificado] [nao verificado]

## 05/11 20:00 · sessao 2ed80c37b402 · - · - · origem=Ronald (tmux)

Leitores principais e autoridades, todos somente inspecionados no código:

- **Juiz de cobertura:** `ausencia_cobre(colab, data_ref, para=...)` lê `Ausencia.colaborador`, `status`, `data`, `data_fim`, `tipo`, `pk`; inclui dia único ou intervalo com ambas bordas inclusivas, permite intervalo aberto somente para `TIPOS_EM_ABERTO` e desempata por maior `(data, pk)`. `para='folha'` aceita somente aprovada; `para='cobranca'` exclui rejeitada. A irmã `cobertura_ausencia_periodo` aplica a mesma regra para todos os dias do intervalo. — `app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2753`.
- **Registro vivo:** cobertura da folha e suspensão da cobrança apontam ambas para `ausencia_cobre`; dias ocupados apontam para `cobertura_ausencia_periodo`; precedência aponta para `fatos_do_dia`; férias hoje aponta para `em_gozo_hoje`. — `app/core/juizes.py:174`, `app/core/juizes.py:178`, `app/core/juizes.py:239`, `app/core/juizes.py:250`.
- **Outra pergunta, outra fonte:** `em_gozo_hoje`/`ferias_na_janela` consultam **AgendamentoFerias**, por interseção inclusiva das datas e exclusão de cancelado; não consultam `Ausencia`. Logo alteração só do agendamento pode mudar esse resultado sem mudar cobertura do espelho. — `app/ferias/services.py:47`, `app/ferias/services.py:51`, `app/ferias/services.py:57`, `app/ferias/services.py:63`.

**Espelho, cartão, grade e célula**

- Espelho carrega cobertura de cobrança para objeto/ícone e cobertura de folha para a palavra do dia. Campos exibidos: `ausencia`, `ausencia_icone`, `_cobertura_folha`; carrega também fatos de precedência e decisões de ausência. — `app/ponto/services/espelho.py:579`, `app/ponto/services/espelho.py:588`, `app/ponto/services/espelho.py:646`, `app/ponto/services/espelho.py:845`, `app/ponto/services/espelho.py:870`.
- `dia_decidido.veredito_do_dia` começa pela cobertura aprovada; se houver retorna `veredito_de(cobertura)` antes de analisar os fatos A2. A documentação explicita que precedência responde ao fato vencedor e pode enxergar ausência ainda não aprovada. — `app/ponto/services/dia_decidido.py:332`, `app/ponto/services/dia_decidido.py:362`.
- PDF do cartão chama o próprio espelho e recorta seu `dias`; não tem regra própria de férias para a palavra/linha desenhada. — `app/relatorios/pdf_espelho.py:596`, `app/relatorios/pdf_espelho.py:602`.
- Grade `_marcar_ausencias` consulta cobertura `para='folha'`, lê tipo/rótulo/pk/parcialidade e escreve `tipo_dia='ausencia'`, `ausencia_tipo`, `ausencia_label`, `ausencia_id`, cobrindo inclusive folga; falta recebe ramo próprio. — `app/escala/utils.py:1177`, `app/escala/utils.py:1195`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/escala/utils.py:1241`.
- Cartório lê `ausencia_cobre(..., para='cobranca')` para cada célula e põe status da cobertura na impressão dos insumos. Separadamente `aplicar_precedencia_na_celula` consulta `fatos_do_dia` e carimba `ata.precedencia`. — `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1935`, `app/ponto/services/cartorio.py:1947`.
- Leitor da célula lê o `tipo_dia` já lavrado na ata; transporta cobertura parcial e período da ata. Não atualiza esses campos consultando a ausência atual. — `app/escala/services/leitor_celula.py:380`, `app/escala/services/leitor_celula.py:401`, `app/escala/services/leitor_celula.py:412`.
- A2 `fatos_do_dia` delega para `fatos_do_periodo`; este consulta Ausencias não rejeitadas com interseção, expande por dia, ordena fatos pela `PRECEDENCIA` e retorna tipo/faixa/rótulo/suprimidos. Ele enxerga ausência remanescente antiga independentemente das datas atuais de AgendamentoFerias. — `app/ponto/precedencia.py:242`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:335`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`.
- Signals de `Ausencia.post_save/post_delete` julgam seus dias no cartório; `_dias_ausencia` usa os dias do juiz, corta no horizonte de hoje e, para intervalos longos, prioriza dias com célula existente. `julgar_celula` retorna sem julgar célula futura ou inexistente. — `app/ponto/signals.py:115`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/services/cartorio.py:1960`, `app/ponto/services/cartorio.py:1974`.
- O `supra_juiz` lê cobertura/grade para acusar `FATO_EM_AUSENCIA` quando há realizado em dia aprovado e `COBRANCA_EM_DIA_COBERTO` quando há chamado vivo sobre cobertura integral. — `app/ponto/supra_juiz.py:400`, `app/ponto/supra_juiz.py:531`.

**Motor e folha**

- `efeitos_da_ausencia` lê todas as aprovadas do período para crédito parcial e cobertura aprovada por dia para conjuntos de dias justificados/suprimidos/integrais; efeito vem de `a.efeito`. É consumido por espelho e PDF. — `app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:43`, `app/ponto/services/efeito_ausencia.py:49`, `app/ponto/services/espelho.py:1048`, `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`.
- Fechamento consulta `ausencias_do_periodo(...,'folha')`, usa cobertura aprovada por dia para abono/supressão e injeta `dias_com_ausencia_inteira` no motor. — `app/ponto/services/fechamento.py:629`, `app/ponto/services/fechamento.py:664`, `app/ponto/services/fechamento.py:679`, `app/ponto/services/fechamento.py:684`, `app/ponto/services/fechamento.py:742`.
- Motor recebe mapas prontos; não consulta Ausencia para esse efeito. Para dia integralmente coberto, zera atraso e saída antecipada e acrescenta alerta com tipo da Ausencia. Espelho e PDF injetam o mesmo mapa. — `app/ponto/motor_calculo_v2.py:805`, `app/ponto/motor_calculo_v2.py:1085`, `app/ponto/motor_calculo_v2.py:1092`, `app/ponto/services/espelho.py:1133`, `app/relatorios/pdf_espelho.py:406`.
- Export `ausencias_na_janela` lê aprovadas dos tipos solicitados por interseção; férias estão explicitamente fora de `8932`. Já retenção `dias_ferias_com_batida` consulta **AgendamentoFerias** por `ferias_na_janela`, recorta à competência e cruza batidas apuráveis. São duas fontes distintas usadas para perguntas distintas no mesmo domínio folha. — `app/folha/export.py:60`, `app/folha/export.py:76`, `app/folha/export.py:182`, `app/ponto/catalogo/ausencias.py:290`, `app/folha/export.py:453`, `app/folha/export.py:473`, `app/folha/export.py:480`.
- O cadastro estático do catálogo marca férias como efeito `ABONA`; efeito vivo pode ser alterado pelo cadastro TipoAusencia, portanto efeito vigente concreto de produção permanece não verificado. — `app/ponto/catalogo/ausencias.py:101`, `app/ponto/catalogo/ausencias.py:249`; produção `[nao verificado]`.

**Chamados**

- `_coberto_por_ausencia` usa a ausência vencedora para cobrança e decide se cala pergunta; aceita integral ou parcial sem período. É a composição usada na reconciliação. — `app/chamados/services/materializacao.py:55`, `app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:81`, `app/chamados/reconciliador.py:822`.
- `_celula_furada` consulta cobertura por efeito pedido e trata integral como não furada; seleção da via da disputa consulta a cobertura e retorna `VIA_AUSENCIA_DIA` se aprovada. — `app/chamados/juizes.py:431`, `app/chamados/juizes.py:448`, `app/chamados/juizes.py:1474`.
- `reconciliar_por_ausencia` consulta cobertura de cobrança na data do fato, suspende quando status está em `ABERTAS`; `vigia_fio` usa a mesma leitura para balde `aguarda_decisao_de_ausencia`. — `app/chamados/services/silencio_ausencia.py:51`, `app/chamados/services/silencio_ausencia.py:65`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/vigia_fio.py:308`.
- Adesão ignora colaborador com cobertura não-falta hoje; histórico mostra tipo/data/registrador das Ausencias criadas na janela. — `app/chamados/services/adesao.py:68`, `app/chamados/services/historico.py:129`.
- `detectar_ausencias._afastamento_cobre` consulta `ausencia_cobre(...,'cobranca')`; `feriado_reavaliar` consulta A2; detector de anomalias usa bloqueio por **AgendamentoFerias** mediante `bloquear_se_em_ferias`. — `app/ponto/management/commands/detectar_ausencias.py:812`, `app/chamados/services/feriado_reavaliar.py:27`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/detector_anomalias.py:111`.
- Signals de ausência também executam, após commit, devolução de perguntas absorvidas, reconciliação de silêncio do colaborador e fechamento de chamados da ausência decidida. — `app/ponto/signals.py:250`, `app/ponto/signals.py:259`, `app/ponto/signals.py:265`, `app/ponto/signals.py:272`.

**Censo complementar para consolidar com leitura do root**

- Relatórios de absenteísmo e anual consultam Ausencias por interseção e expandem dias pelo juiz. — `app/relatorios/views.py:197`, `app/relatorios/views.py:205`, `app/relatorios/views.py:277`, `app/relatorios/views.py:292`.
- Outros leitores diretos genéricos que podem aceitar férias: inteligência `views.py:71,239,516`, `resumo.py:39`; colaboradores detalhe `services/detalhe.py:104`; APIs `views.py:1861`; vigia ausência `ponto/services/vigia_ausencia.py:59,108,115,120,215`; decisões `ponto/services/dia_decidido.py:542,579`; cartório `ponto/services/cartorio.py:355`. — referências dadas nas respectivas linhas.
- Comandos leitores por cobertura: `app/ponto/management/commands/processar_alertas_turno.py:97`, `processar_alertas_avancados.py:115`, `reconciliar_grade.py:401`, `supra_juiz.py:216`, `vigia_b5_ausencia.py:68`, `backfill_b5_insumos.py:72`, `emitir_furo_retroativo.py:99`, `emitir_turno_aberto_retroativo.py:108`, `ausencias_sem_efeito_na_celula.py:44`, `diff_calculador.py:408`; auditoria de chamados `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `:642`, `:659`, `:1466`.
- Outros comandos com consulta direta de ausência: `app/inteligencia/management/commands/gerar_alertas.py:55`, `calcular_scores.py:12`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:33`, `rejulgar_ausencia_em_folga.py:56`, `ausencia_bloqueante_sem_fim.py:27`, `rejulgar_status_por_tipo.py:23`, `curar_ausencia_sem_fim.py:29`, `zerar_dados_operacionais.py:49`. Apenas os locais foram conferidos pelo `rg`, efeitos específicos ainda `[nao verificado]`.

## 05/11 20:00 · sessao e8e517383aa1 · - · - · origem=Ronald (tmux)

Censo de escritores de datas de `AgendamentoFerias` na cópia:

| Escritor efetivo | Campos e fluxo | Entradas |
|---|---|---|
| `registrar_gozo_historico` | Constrói `data_inicio=gi`, `data_fim=gf`, `dias=(gf-gi).days+1`, `status=status_por_data`; chama `ag.save()`; atualiza dias concedidos/vendidos do PA. Verifica coerência de intervalo e teto de venda; não chama `full_clean` (`app/ferias/services.py:270`, `app/ferias/services.py:278`, `app/ferias/services.py:289`, `app/ferias/services.py:298`). | `criar_periodo`, quando marcado gozo histórico e recebidas datas; chama serviço (`app/ferias/views.py:190`, `app/ferias/views.py:197`, `app/ferias/views.py:209`). |
| `executar_agendamento` | Deriva fim inclusivo `inicio+dias-1`; guarda duplicata por PA/início, excluindo cancelado; `objects.create` grava início/fim/dias/aviso/autoria; atualiza PA (`app/ferias/services.py:306`, `app/ferias/services.py:313`, `app/ferias/services.py:316`, `app/ferias/services.py:321`). | Individual via wrapper `_executar_agendamento` (`app/ferias/views.py:259`, `app/ferias/views.py:298`); lote, com revalidação antes de cada execução (`app/ferias/views.py:607`, `app/ferias/views.py:613`, `app/ferias/views.py:617`); intake via `agendar_ferias_porta` (`app/ferias/services.py:128`, `app/ferias/services.py:152`, `app/ferias/services.py:157`), registrado como escritor do intake (`app/core/intake.py:32`, `app/core/intake.py:34`) e chamado no “sim” do chat (`app/core/views_ia.py:668`, `app/core/views_ia.py:684`). |
| `corrigir_datas_agendamento` | Atualiza datas/dias/aviso/status, recalcula dias concedidos, limpa `antigo \ novo` e chama `ag.save()` completo (`app/ferias/services.py:366`, `app/ferias/services.py:374`, `app/ferias/services.py:376`, `app/ferias/services.py:385`, `app/ferias/services.py:387`). | `editar_datas_admin` (`app/ferias/views.py:421`, `app/ferias/views.py:489`). |
| `remarcar_agendamento` | Atualiza datas/dias/aviso/status e PA; chama `ag.save(update_fields=[data_inicio,data_fim,dias,aviso_emitido,status])`; não chama limpeza do intervalo antigo (`app/ferias/services.py:393`, `app/ferias/services.py:399`, `app/ferias/services.py:401`, `app/ferias/services.py:406`). | `editar_agendamento`, porta apontada na obra (`app/ferias/views.py:633`, `app/ferias/views.py:667`). |
| `aprovar_solicitacao` | Materializa início/fim desejados e dias da solicitação em novo agendamento; chama `full_clean()` e `save()`; atualiza PA (`app/ferias/services.py:413`, `app/ferias/services.py:427`, `app/ferias/services.py:437`, `app/ferias/services.py:446`). | Avaliação com POST/permissão e bloqueio `select_for_update` da solicitação pendente (`app/ferias/views.py:712`, `app/ferias/views.py:717`, `app/ferias/views.py:728`). |

`data_inicio`, `data_fim` e `dias` são os nomes reais dos campos (`app/ferias/models.py:302`).

O cron `sincronizar_status_ferias` lê datas e escreve apenas status, por `alinhar_status_por_data`; exige `--apply`, exclui cancelados e usa `status_por_data` (`app/ferias/management/commands/sincronizar_status_ferias.py:20`, `app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/sincronizar_status_ferias.py:38`, `app/ferias/management/commands/sincronizar_status_ferias.py:44`, `app/ferias/services.py:518`).

O Django admin deste módulo registra somente `Afastamento`; não há registro de `AgendamentoFerias` nesse arquivo (`app/ferias/admin.py:2`, `app/ferias/admin.py:4`). A API de férias examinada consulta e devolve datas/dias/status (`app/api/views.py:2251`, `app/api/views.py:2257`); não grava nesse fluxo (`app/api/views.py:2270`). Não encontrei outro escritor por signal/API/command na pesquisa global; ausência absoluta de outro escritor dinâmico: [nao verificado].

**Limpeza irmã, chamadores e guardas**

A função real está em `ponto/services/ausencia.py::apagar_ausencias_de_ferias`, não em `ferias/services.py`; filtra pessoa, tipo `ferias`, data inicial/final inclusivas, exclui opcionalmente o novo intervalo e executa `delete`; não filtra status, descrição, autoria ou PK de agendamento (`app/ponto/services/ausencia.py:1246`, `app/ponto/services/ausencia.py:1250`, `app/ponto/services/ausencia.py:1252`, `app/ponto/services/ausencia.py:1254`).

Os dois chamadores encontrados são:

1. `corrigir_datas_agendamento`, dentro de `atomic`, guardando as datas anteriores antes de alterá-las e passando `exceto=(nova_ini,nova_fim)` (`app/ferias/services.py:374`, `app/ferias/services.py:375`, `app/ferias/services.py:385`). A porta `editar_datas_admin` exige POST, permissão `editar_ferias`, status diferente de cancelado, justificativa com ao menos cinco caracteres, dias positivos e saldo suficiente (`app/ferias/views.py:437`, `app/ferias/views.py:449`, `app/ferias/views.py:452`, `app/ferias/views.py:454`, `app/ferias/views.py:463`, `app/ferias/views.py:465`).
2. `AgendamentoFerias.delete`, passando o intervalo atual sem `exceto`; o método não acrescenta guarda própria de status/permissão (`app/ferias/models.py:439`, `app/ferias/models.py:441`, `app/ferias/models.py:443`).

`remarcar_agendamento` não chama essa função; sua porta exige POST/permissão, status exatamente `agendado`, dias positivos e saldo (`app/ferias/views.py:636`, `app/ferias/views.py:649`, `app/ferias/views.py:657`, `app/ferias/views.py:661`).

Apesar do comentário “ainda não iniciado”, a view de remarcação testa somente status; não consulta `pode_editar` nem `ja_iniciou`, propriedades que incluem a data atual (`app/ferias/views.py:648`, `app/ferias/views.py:649`, `app/ferias/models.py:322`, `app/ferias/models.py:327`). Isso é diferença observável de regra atual, sem proposta de mudança (`app/ferias/views.py:649`, `app/ferias/models.py:324`).

**Novo intervalo e prova do defeito por leitura**

`AgendamentoFerias.save` chama guarda de sobreposição, grava e chama `_sync_ausencia` mesmo quando recebe `update_fields` (`app/ferias/models.py:434`, `app/ferias/models.py:435`, `app/ferias/models.py:437`). `_sync_ausencia` delega a `espelhar_ferias` (`app/ferias/models.py:379`, `app/ferias/models.py:384`). `espelhar_ferias` consulta existentes somente entre as datas **atuais** do agendamento, calcula todos os dias inclusivos desse intervalo e cria os que faltam por `criar_ausencia`, tipo férias, aprovada, um dia, descrição com PK do agendamento (`app/ponto/services/ausencia.py:1165`, `app/ponto/services/ausencia.py:1168`, `app/ponto/services/ausencia.py:1177`, `app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1186`). Portanto a remarcação tenta criar a Ausência nova por esse escritor; dias recusados pelo veto não são criados e produzem Pauta DP (`app/ponto/services/ausencia.py:1193`, `app/ponto/services/ausencia.py:1199`, `app/ponto/services/ausencia.py:1204`).

O laço de remoção opera sobre o conjunto `existentes` carregado apenas no novo intervalo; não alcança Ausências no antigo intervalo que ficou fora do novo (`app/ponto/services/ausencia.py:1165`, `app/ponto/services/ausencia.py:1195`). Esse é o fundamento estático do defeito alegado, sem comprovação de ocorrência em produção [nao verificado].

`espelhar_ferias` reconhece agendado/em_gozo/concluido como ativos; cancelado remove as Ausências encontradas somente no intervalo atual (`app/ponto/services/ausencia.py:1164`, `app/ponto/services/ausencia.py:1172`).

**Guarda compartilhada já existente**

`save` não chama `clean` ou `full_clean`; chama `_guard_sobreposicao` (`app/ferias/models.py:434`). A guarda pula cancelado e atualização sem campos de data, ou sem mudança efetiva nas datas (`app/ferias/models.py:394`, `app/ferias/models.py:397`, `app/ferias/models.py:400`). Ao editar datas, reúne todas as Ausências férias da pessoa no intervalo anterior e as exclui do veto; não confirma posse pelo PK gravado na descrição (`app/ferias/models.py:418`, `app/ferias/models.py:423`, `app/ferias/models.py:428`). O veto é `ponto.precedencia.veto_de_lancamento` (`app/ferias/models.py:413`, `app/ferias/models.py:428`).

Consequência para Ausência férias alterada manualmente: a limpeza irmã alcança qualquer linha férias cuja `data` caia em `antigo \ novo`, e a guarda também exclui do veto as linhas férias com `data` dentro do antigo; ambas selecionam por pessoa/tipo/data, sem elo explícito ao agendamento (`app/ponto/services/ausencia.py:1250`, `app/ferias/models.py:423`). A função não restaura conteúdo de linhas existentes no novo intervalo: qualquer linha já encontrada nessa data faz a criação ser pulada (`app/ponto/services/ausencia.py:1182`).

**Sítios irmãos relevantes**

- O par direto é `corrigir_datas_agendamento` versus `remarcar_agendamento`: ambos substituem início/fim/dias/status e recalculam PA; apenas a correção limpa o antigo com exceção do novo (`app/ferias/services.py:376`, `app/ferias/services.py:385`, `app/ferias/services.py:400`, `app/ferias/services.py:406`).
- `excluir_periodo_aquisitivo` apaga agendamentos via `periodo.agendamentos.all().delete()`, enquanto a limpeza explícita mora no `AgendamentoFerias.delete` de instância (`app/ferias/services.py:479`, `app/ferias/services.py:483`, `app/ferias/models.py:439`). A view permite exclusão apenas se não houver agendamento em agendado/em_gozo/concluido; cancelados sobreviventes são o universo restante (`app/ferias/views.py:813`, `app/ferias/views.py:814`, `app/ferias/views.py:818`). Isso é assimetria de porta de exclusão; efeitos concretos de ORM não comprovados em execução [nao verificado].
- Entidade irmã `Ausencia`: `editar_ausencia_registro` muda tipo/data/fim/dias e salva sem guardar intervalo anterior (`app/ponto/services/ausencia.py:496`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:508`); o signal pós-save julga os dias derivados da instância já alterada (`app/ponto/signals.py:115`, `app/ponto/signals.py:124`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`). O `Ausencia.save` examinado também não guarda nem emite intervalo anterior (`app/ponto/models.py:866`, `app/ponto/models.py:892`, `app/ponto/models.py:906`). É um sítio da mesma classe de perda de intervalo anterior para releitura; ocorrência em produção e efeito final dependente de outros caminhos [nao verificado].
- `AfastamentoAdmin` edita início e retornos diretamente; o modelo irmã não contém materialização de Ausência ou limpeza correlata no trecho completo da classe, portanto não há evidência suficiente para equiparar seu mecanismo ao defeito F-346 (`app/ferias/admin.py:12`, `app/ferias/admin.py:13`, `app/ferias/models.py:488`, `app/ferias/models.py:527`).

## 05/11 20:00 · sessao 91c11f014c03 · - · - · origem=Ronald (tmux)

Aval F-346/HJ-INGESTAO-E-F346: não localizei registro correspondente na cópia; a regra específica disponível é a frase fornecida no pedido [nao verificado]. O piloto documenta censo antes do construtor, leitura apenas e resposta sem desenho/código em `app/docs/RELATO.md:4-9`; referência histórica do piloto em `app/docs/PROMPTS.md:1878-1882`.

A função irmã efetiva é `ponto/services/ausencia.py::apagar_ausencias_de_ferias`, localizada em `app/ponto/services/ausencia.py:1246`; `ferias/services.py` a importa e chama na correção em `app/ferias/services.py:372,385-386`. Ela seleciona pelo colaborador, tipo `ferias` e `data` inclusiva no intervalo antigo, preservando a janela nova mediante `exceto`; não filtra status, descrição, autor ou identificador do agendamento e não consulta competência em `app/ponto/services/ausencia.py:1250-1254`.

Contraexemplos e candidatos RED, todos sintéticos; resultado executado [nao verificado]:

| Cadastro + estado + ato | Comportamento atual / expectativa fundamentada |
|---|---|
| Agendamento futuro, ausência diária antiga, remarcação totalmente disjunta | `remarcar_agendamento` troca as datas e salva, sem limpar o intervalo anterior em `app/ferias/services.py:393-409`; o espelhamento consulta apenas o intervalo novo em `app/ponto/services/ausencia.py:1165-1169`. Pela limpeza da irmã, o intervalo antigo fora do novo é o conjunto selecionado para exclusão em `app/ponto/services/ausencia.py:1250-1254`. |
| Intervalos antigo e novo parcialmente sobrepostos | A irmã preserva exatamente a interseção pelo `exclude` inclusivo em `app/ponto/services/ausencia.py:1252-1253`; o espelhamento conserva linhas já existentes no novo em `app/ponto/services/ausencia.py:1182-1184`. Hoje a remarcação deixa também o pedaço antigo que saiu em `app/ferias/services.py:393-409`. |
| Novo início igual ao antigo fim | O dia comum está incluído nas duas pontas e é preservado pelo `exceto` em `app/ponto/services/ausencia.py:1250-1253`; criação dos dias novos inclui `data_fim` em `app/ponto/services/ausencia.py:1178-1181`. |
| Novo intervalo com menos dias, mesmo início ou mesmo fim | A irmã seleciona a ponta retirada, preservando os dias do novo intervalo em `app/ponto/services/ausencia.py:1250-1254`; o serviço de remarcação não realiza essa seleção em `app/ferias/services.py:393-409`. |
| Ausência de férias manual dentro do intervalo antigo, editada para range maior | A limpeza identifica a linha pela **data inicial**, não pela cobertura completa nem pela proveniência em `app/ponto/services/ausencia.py:1250-1254`; apagar a linha pode remover sua cobertura fora da janela antiga, pois a cobertura do range é considerada em `app/ponto/precedencia.py:335-339`. Regra especial que preserve a edição manual [nao verificado]. |
| Ausência manual ou de outro agendamento na interseção | `_guard_sobreposicao` exclui todas as ausências de férias cuja `data` esteja no intervalo antigo, sem verificar vínculo de origem em `app/ferias/models.py:415-430`; o espelho pula qualquer linha existente daquela data, sem verificar status em `app/ponto/services/ausencia.py:1165-1184`. Propriedade inequívoca da linha [nao verificado]. |
| Célula já gerada no intervalo antigo, data futura | Ausência excluída emite `post_delete`, que chama julgamento em `app/ponto/signals.py:150-152`; o cartório devolve `None` para data futura ou célula inexistente em `app/ponto/services/cartorio.py:1974-1978`. Assim, exclusão da Ausência não demonstra atualização imediata da célula futura em `app/ponto/signals.py:150-152` e `app/ponto/services/cartorio.py:1974-1978`. |
| Célula antiga, dia passado | Exclusão de Ausência chama `_julgar_dias` e `cartorio.julgar_celula` em `app/ponto/signals.py:85-93,150-152`; há reconciliação de chamados após commit em `app/ponto/signals.py:250-272,282-284`. Resultado materializado real [nao verificado]. |
| Remarcação para depois de competência 21→20 entregue | Janela depende do cadastro e do juiz, não do mês civil em `app/ponto/janelas.py:10-21,36-63`; nem remarcação nem limpeza consultam exportação em `app/ferias/services.py:393-410` e `app/ponto/services/ausencia.py:1246-1254`. O espelhamento expressamente não recusa período fechado em `app/ponto/services/ausencia.py:1152-1153`. A L-092 consta em `app/docs/LEIS.md:125`, mas há atualização de regra em `CLAUDE.md:398-414`; efeito sobre FechamentoMensal/TXT real [nao verificado]. |
| Agendamento cancelado submetido pela view de remarcação | A view recusa qualquer status diferente de `agendado` em `app/ferias/views.py:648-650`; a porta de correção também recusa cancelado em `app/ferias/views.py:452-453`. O serviço isolado não tem essa guarda e recalcula status pelas datas em `app/ferias/services.py:393-407`; uso direto em produção [nao verificado]. |
| Agendamento `agendado`, mas início já passado por status atrasado | A view verifica apenas status em `app/ferias/views.py:648-650`, enquanto a property `pode_editar` exige também `not ja_iniciou` em `app/ferias/models.py:321-328`. Copiar a guarda da irmã mudaria essa diferença; autorização para mudar critério [nao verificado]. |
| Dois agendamentos do mesmo colaborador no mesmo período | Nova escrita passa pelo veto via `save` em `app/ferias/models.py:387-437`; duas linhas legadas já sobrepostas não têm sua origem distinguida pelo apagador em `app/ponto/services/ausencia.py:1250-1254`. A criação normal bloqueia mesma data de início não cancelada no PA em `app/ferias/services.py:315-323`. Resultado com linhas legadas sobrepostas [nao verificado]. |
| Novo intervalo ocupado por outro fato humano | O veto levanta exceção nomeando o fato existente em `app/ponto/precedencia.py:435-469`, chamado pelo `save` em `app/ferias/models.py:427-437`; se a materialização diária recusar, não cria aquela Ausência e encaminha Pauta em `app/ponto/services/ausencia.py:1185-1200`. |

A guarda adicional da irmã está na **view**, não no apagador: correção permite qualquer status vivo, exige permissão e justificativa mínima de cinco caracteres em `app/ferias/views.py:449-457`; remarcação exige status `agendado`, quantidade positiva e saldo disponível em `app/ferias/views.py:648-663`. A correção deliberadamente não usa `full_clean`, limitando recusa a colisão e integridade em `app/ferias/views.py:474-490`.

Hash recalculado da árvore [nao verificado]: não há `.git` nesta cópia; `bin/arvore_hash.sh:27-28` usa índice temporário e Git, incompatível com executar em leitura estrita. O hash `e9f3ab0e21bb353456cb8abd99ffa97af696ff04` é o rótulo fornecido no pedido [nao verificado].

## 05/11 19:44 · sessao b9319945ac3f · C20-JUIZ-3 · verificador · origem=Code

**Os três casos novos mordem os mutantes pontuais por análise estática.** Os resultados de execução dos testes e dos mutantes são **[não verificado]**. As asserções relevantes estão em `app/chamados/tests/test_c20_juiz3_outbox.py:142`, `app/chamados/tests/test_c20_juiz3_outbox.py:227` e `app/chamados/tests/test_c20_juiz3_outbox.py:329`.

1. **Qual asserção cai em cada mutante**

   - **Tirar `falhas=falhas` da chamada de `retratar_premissa_morta_colab`:** a exceção do pai continua capturada, mas não entra na lista do consumidor; o parâmetro assume `None`. Sem outra falha, cai primeiro `assertEqual(r['falhas'], 1)`, pois o contador fica zero. Também deixam de valer a tentativa gasta e o erro com prefixo `premissa ch=<pai>`. (`app/chamados/reconciliador.py:1360`; `app/chamados/reconciliador.py:1373`; `app/chamados/reconciliador.py:1381`; `app/chamados/reconciliador.py:1424`; `app/chamados/reconciliador.py:1445`; `app/chamados/tests/test_c20_juiz3_outbox.py:329`)

   - **Tirar o atomic próprio de `criar`, mantendo o corpo:** o `RuntimeError` simulado ocorre na chamada do emissor, depois do create e da trilha. Sem o bloco que recebe essa exceção, a marca de rollback esperada não é produzida por esse caminho; cai `assertTrue(transaction.get_rollback())`. O rejulgamento do cartório está neutralizado nessa classe. (`app/chamados/models.py:1055`; `app/chamados/models.py:1060`; `app/chamados/tests/test_c20_juiz3_outbox.py:90`; `app/chamados/tests/test_c20_juiz3_outbox.py:138`; `app/chamados/tests/test_c20_juiz3_outbox.py:142`)

   - **Tirar o atomic próprio de `recompute_turnos`, mantendo o corpo:** DELETE e `bulk_create` já substituíram o recorte quando o emissor lança a exceção. Sem o bloco que restaura o recorte, cai a asserção de existência do turno original com `entrada=e` e `saida IS NULL`. (`app/ponto/nucleo.py:57`; `app/ponto/nucleo.py:59`; `app/ponto/nucleo.py:66`; `app/chamados/tests/test_c20_juiz3_outbox.py:224`; `app/chamados/tests/test_c20_juiz3_outbox.py:227`)

   O teste de falhas comprova as condições da primeira passada; não executa uma segunda passada para provar a releitura prometida na docstring. (`app/chamados/tests/test_c20_juiz3_outbox.py:313`; `app/chamados/tests/test_c20_juiz3_outbox.py:333`)

2. **`needs_rollback`, limpeza e possibilidade de falso positivo**

   O `finally` **não suprime uma falha da asserção**: ele somente executa `set_rollback(False)`, sem `return` ou captura de `AssertionError`. Porém, limpar a marca não restaura as linhas já escritas; portanto esse caso prova a marcação da transação, não o rollback efetivamente ocorrido. (`app/chamados/tests/test_c20_juiz3_outbox.py:141`; `app/chamados/tests/test_c20_juiz3_outbox.py:145`)

   A contenção entre casos depende do teardown normal de `TestCase`: a base efetivamente herda dele, mas sua implementação não está na cópia examinada. Contaminação observada entre casos e execução desse teardown são **[não verificado]**. O caso também não exercita `criar` em autocommit real, pois permanece dentro do `TestCase`. (`app/chamados/tests/test_c20_juiz3_outbox.py:23`; `app/chamados/tests/test_c20_juiz3_outbox.py:37`; `app/chamados/tests/test_c20_juiz3_outbox.py:132`)

   **Para a remoção pontual do atomic, não identifiquei caminho de passagem no fluxo mostrado.** Há, contudo, uma possibilidade estrutural de falso positivo: outro caminho, depois do `assertFalse` inicial, marcar rollback e lançar um `RuntimeError` aceito pelo `assertRaises`. O teste não verifica a mensagem da exceção nem exige que o mock do emissor tenha sido chamado. Esse caminho alternativo concreto não foi demonstrado **[não verificado]**. (`app/chamados/tests/test_c20_juiz3_outbox.py:137`; `app/chamados/tests/test_c20_juiz3_outbox.py:142`)

3. **Alcance do patch de `_veredito_premissa`**

   **Sim, ele também alcança `_premissa_viva`**, porque essa função consulta `_veredito_premissa` no mesmo módulo. O mock, entretanto, só lança para o PK do pai; para os demais chamados, delega ao original. (`app/chamados/reconciliador.py:304`; `app/chamados/reconciliador.py:308`; `app/chamados/tests/test_c20_juiz3_outbox.py:321`; `app/chamados/tests/test_c20_juiz3_outbox.py:327`)

   Os gates examinados em `models.py` usam `premissa_regularizada`; quando precisam do catálogo, importam `avaliar` diretamente. Esse caminho não consulta o símbolo mockado no reconciliador. (`app/chamados/models.py:648`; `app/chamados/models.py:701`; `app/chamados/models.py:1268`; `app/chamados/models.py:1275`)

   A atribuição da falha fica especialmente amarrada pela última asserção: ela exige `premissa ch=<pai>` em **todos** os eventos pendentes encontrados. O laço de premissa grava esse prefixo; os tratamentos de falha da célula, de disputa e do consumidor usam outros formatos. Assim, uma falha genérica em outro caminho pode satisfazer o contador, mas não basta para satisfazer o teste inteiro. (`app/chamados/tests/test_c20_juiz3_outbox.py:333`; `app/chamados/reconciliador.py:1448`; `app/chamados/reconciliador.py:1003`; `app/chamados/reconciliador.py:1165`; `app/chamados/reconciliador.py:1369`)

4. **Classes de reprovação no delta V2**

   **Não identifiquei introdução dessas classes no delta fornecido.** A alteração executável de produção é a interpolação de `premissa` na saída; os trechos de produção alterados em `criar` e no núcleo são explicativos. (`app/chamados/management/commands/consumir_eventos_celula.py:31`; `app/chamados/models.py:1048`; `app/ponto/nucleo.py:70`)

   - **Segundo juiz ou derivação paralela:** continua a consulta ao catálogo pelo coletor existente e a retratação por `_retratar`. (`app/chamados/reconciliador.py:256`; `app/chamados/reconciliador.py:301`; `app/chamados/reconciliador.py:1437`; `app/chamados/reconciliador.py:1443`)
   - **Segundo escritor fora da porta:** os produtores usam `emitir_evento_celula`; o DELETE permanece no arquivo declarado como porta de `TurnoMaterializado`. (`app/chamados/models.py:1059`; `app/ponto/nucleo.py:91`; `app/ponto/portas/celula.py:643`; `app/core/portas.py:117`)
   - **Relógio ou ordem de trava:** o acréscimo de saída não lê relógio nem adquire trava; a trava do command continua envolvendo `_rodar`. (`app/chamados/management/commands/consumir_eventos_celula.py:19`; `app/chamados/management/commands/consumir_eventos_celula.py:33`)
   - **Selo sem mordida:** a contagem alterada permanece uma igualdade exata, `len(salvos)+1`, seguida de verificação de nenhuma emissão adicional na repetição. Isso não constitui aumento de teto de desempenho; execução do selo 17 é **[não verificado]**. (`app/ponto/tests/test_o_outbox_celula.py:111`; `app/ponto/tests/test_o_outbox_celula.py:116`)

   O DELETE cru continua declarado **INCOMPLETO quanto ao censo AST**; a docstring não fornece cobertura adicional. (`app/ponto/nucleo.py:75`; `app/ponto/nucleo.py:80`)

5. **Quem lê a nova saída**

   - **Cron/log:** a configuração agenda o command a cada minuto e encaminha stdout/stderr para `consumir_eventos_celula.log`. Isso demonstra o destino configurado, não um parser do campo. (`app/config/crons.py:394`; `app/config/crons.py:1829`)
   - **Testes:** um teste executa o command normal com `StringIO`, mas verifica o estado da pergunta; outro inspeciona somente “outra execucao”, ramo anterior à linha nova. Os casos C20 chamam diretamente a função Python. (`app/chamados/tests/test_orfas_pelo_evento.py:209`; `app/chamados/tests/test_orfas_pelo_evento.py:211`; `app/ponto/tests/test_o_outbox_celula.py:466`; `app/chamados/management/commands/consumir_eventos_celula.py:21`; `app/chamados/tests/test_c20_juiz3_outbox.py:240`)
   - **Envelope:** `cron_run.sh` executa o comando e registra duração/exit em JSON próprio; sua decisão usa o código de saída. (`bin/cron_run.sh:11`; `bin/cron_run.sh:13`; `bin/cron_run.sh:22`)
   - **Docs:** há relato de contagens históricas atribuídas ao log, sem contrato de leitura do novo campo. (`app/docs/RELATO.md:1120`)

   Parser específico de `por premissa morta`, asserção textual desse campo, leitores externos e crontab efetivamente instalado: **[não verificado]**.

## 05/11 19:42 · sessao 10a5b029df2a · - · - · origem=Ronald (tmux)

Na volta 2, não encontrei introdução das classes citadas: a mudança executável de produção é a inclusão de `r['premissa']` na saída; os outros trechos de produção alterados pelo delta são comentários/docstring. A conclusão se limita ao delta fornecido e à cópia, sem execução. (`app/chamados/management/commands/consumir_eventos_celula.py:31`; `app/chamados/models.py:1048`; `app/ponto/nucleo.py:70`)

- **Segundo juiz / derivação paralela:** o contexto da obra mantém a consulta a `_veredito_premissa`, que consulta o catálogo, e a retratação passa por `_retratar`; os testes novos não acrescentam autoridade de produção. (`app/chamados/reconciliador.py:247`, `app/chamados/reconciliador.py:1437`, `app/chamados/reconciliador.py:1443`)
- **Segundo escritor fora da porta:** os produtores chamam `emitir_evento_celula`, onde está o INSERT do evento; o DELETE continua em `ponto/nucleo.py`, porta declarada de `TurnoMaterializado`. (`app/chamados/models.py:1059`, `app/ponto/nucleo.py:91`, `app/ponto/portas/celula.py:643`, `app/core/portas.py:114`)
- **Relógio fora da borda:** o delta V2 não acrescenta leitura de relógio ao código de produção; os testes novos pertencem às classes com `freeze_time`. (`app/chamados/management/commands/consumir_eventos_celula.py:31`, `app/chamados/tests/test_c20_juiz3_outbox.py:83`, `app/chamados/tests/test_c20_juiz3_outbox.py:149`, `app/chamados/tests/test_c20_juiz3_outbox.py:232`)
- **Selo sem mordida:** a mudança do teste antigo continua cobrando uma igualdade exata: `eventos == len(salvos) + 1`; cobra também nenhuma emissão na repetição. O teto numérico do selo 17 e sua execução nesta cópia permanecem **[não verificado]**. (`app/ponto/tests/test_o_outbox_celula.py:107`, `app/ponto/tests/test_o_outbox_celula.py:111`, `app/ponto/tests/test_o_outbox_celula.py:116`)
- **Ordem de trava:** o delta V2 não adiciona trava; no contexto completo, o consumidor chama a porta por chamado e só envolve a marcação dos eventos em atomic próprio, após os julgamentos. (`app/chamados/reconciliador.py:1354`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1370`)

O DELETE cru invisível ao censo continua **INCOMPLETO**; a volta 2 documenta o limite, sem corrigir sua cobertura. (`app/ponto/nucleo.py:70`, `app/ponto/nucleo.py:75`, `app/ponto/nucleo.py:80`)

## 05/11 19:42 · sessao 9667632ea367 · - · - · origem=Ronald (tmux)

- **Destino da linha:** command imprime o novo campo em `app/chamados/management/commands/consumir_eventos_celula.py:32–35`; cron está declarado a cada minuto com `log='consumir_eventos_celula.log'` em `app/config/crons.py:394`; render redireciona stdout/stderr ao arquivo em `app/config/crons.py:1829–1830`.
- **Testes:** `app/chamados/tests/test_orfas_pelo_evento.py:209` executa o command normal com `StringIO`, mas verifica estado da pergunta, sem inspecionar saída (`:210–211`). `app/ponto/tests/test_o_outbox_celula.py:464–466` inspeciona somente “outra execucao”, ramo que retorna antes da linha nova (`app/chamados/management/commands/consumir_eventos_celula.py:19–22`). Os testes C20 chamam diretamente a função Python (`app/chamados/tests/test_c20_juiz3_outbox.py:240–243`).
- **Envelope e medidores:** `bin/cron_run.sh:11–13` executa o command e registra JSON próprio com duração/exit; `:22–35` decide pelo exit. Vigia lê esse JSON (`app/core/services/crons_quebrados.py:30–46`), assim como o medidor de duração (`bin/crons_duracao.py:61–75`); esses caminhos não analisam a frase acrescentada.
- **Docs:** `app/docs/RELATO.md:1120` registra contagens históricas e cita esse log; não define parser/formato para o novo campo.
- **Limite:** busca por nome do command/log e fragmentos da linha não encontrou parser específico nem asserção da expressão nova; leitores externos, crontab efetivamente instalado e ausência universal de parsers são **[não verificado]**.

## 05/11 19:11 · sessao 5816bfa46ce5 · H12H06 · verificador · origem=Code

A v6 fecha os contraexemplos de trava sobre a Ausencia errada e de UPDATE anterior à trava da Empresa. Ainda permanecem guarda copiada, um ciclo possível entre escritores e falhas no ciclo de vida do arquivo de reversão. (`app/ponto/tests/test_h06_reversao_guardas.py:412`, `app/ponto/tests/test_h06_reversao_guardas.py:867`; `app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:730`; `app/ponto/views.py:2638`; `app/chamados/models.py:866`.)

Revisão estática. Execução da suíte Django, mutantes de produção, identidade Git/hash da cópia, configuração efetiva do banco, ocorrência dos interleavings e cumprimento dos pré-requisitos em produção: **[nao verificado]**. [nao verificado]

**1. Achados da v5 e triagem**

| Linha da triagem | Situação em 3c6e39a5 |
|---|---|
| PROCEDENTE 1 — trava de Ausencia errada | **Fechado para o mutante apontado.** A porta para na foto anterior à escrita; as sondas agora disputam as três PKs reais. Travar `pk=-1` e carregar A sem trava deixa a sonda de A passar e reprova. (`app/ponto/services/ausencia.py:693`; `app/ponto/tests/test_h06_reversao_guardas.py:850`, `app/ponto/tests/test_h06_reversao_guardas.py:867`, `app/ponto/tests/test_h06_reversao_guardas.py:886`.) |
| PROCEDENTE 2 — ciclos por FK | **Parcial.** `NO KEY UPDATE` remove as arestas que dependiam exclusivamente de `KEY SHARE` da FK. Não elimina o ciclo explícito C→A contra A→C descrito abaixo. (`app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/views.py:2638`; `app/chamados/models.py:866`.) |
| PROCEDENTE 3 — helper escondia UPDATE anterior à Empresa | **Fechado para UPDATE ORM na conexão corrente, dentro da porta.** A contagem começa antes de executar a implementação da porta. Permanecem limites de conexão e reconhecimento textual do SQL. (`app/ponto/tests/test_h06_reversao_guardas.py:411`, `app/ponto/tests/test_h06_reversao_guardas.py:414`, `app/ponto/tests/test_h06_reversao_guardas.py:422`.) |
| PROCEDENTE 4 — motivo/usuário no comando | **Parcial.** O comando apenas resolve o usuário; a porta exige motivo e PK do ator. Entretanto, a exigência de motivo continua repetida na reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:88`; `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:528`.) |
| PROCEDENTE 5 — trilha individual ignorada | **Fechado no código nas duas direções.** Retorno `None` agora levanta exceção tanto ao aplicar quanto ao repor, dentro da transação do ato. A nova prova de falha individual exercita somente aplicar. (`app/ponto/services/ausencia.py:533`, `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:721`; `app/ponto/tests/test_h06_reversao_guardas.py:590`.) |
| PROCEDENTE 6 — arquivo órfão | **Parcial.** Há limpeza após falha posterior à criação completa, mas o arquivo só entra em `escrito` depois de `write` e fechamento. A limpeza também confunde exceção ao sair do `atomic` com ausência de confirmação. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:698`, `app/ponto/services/ausencia.py:730`.) |
| PROCEDENTE 7 — PK repetido | **Fechado para duplicação.** Não fecha validação dos tipos dos valores. (`app/ponto/services/ausencia.py:631`, `app/ponto/services/ausencia.py:635`; `app/ponto/tests/test_h06_reversao_guardas.py:603`.) |
| PROCEDENTE 8 — AST, expressão e import com `as` | **Fechado para os exemplos apontados.** Agora alcança inteiros dentro da expressão e apelidos de `ImportFrom`; permanecem outros caminhos de transportar o literal. (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:30`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:38`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:79`.) |
| DESCARTE 1 — elegibilidade no censo | **Procede como separação entre censo e decisão sobre o estado atual.** O comando seleciona o universo; a porta reconfere identidade, minutos e empresa. Isso não torna a porta autossuficiente diante de um produtor arbitrário de `ato`. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:118`; `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:670`.) |
| DESCARTE 2 — relógios preexistentes | **Procede quanto à autoria do delta; não significa problema curado.** O relógio novo nomeia arquivo na borda; os relógios internos continuam alcançáveis. (`app/ponto/management/commands/rederivar_falta_declarada.py:46`; `app/ponto/models.py:873`; `app/ponto/signals.py:122`; `app/ponto/services/cartorio.py:1965`.) |
| DESCARTE 3 — rateio uniforme | **Procede como fora da cura proposta; permanece aberto.** O leitor continua dividindo o total igualmente pelos dias. (`app/folha/export.py:118`, `app/folha/export.py:121`; `app/docs/PROMPTS.md:1861`.) |
| DESCARTE 4 — pré-requisitos do pouso | **Procede para separar construção de pouso.** Não autoriza considerar o pouso aprovado: medidas 09/10/11 e preservação da 09 continuam exigidas. Cumprimento: **[nao verificado]**. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`.) |
| DESCARTE 5 — motivo na reposição seria outro contrato | **Não procede sob a proibição literal de guarda copiada.** As duas funções exigem o mesmo predicado, na mesma cadeia de reversão. Ser um escritor de exceção não faz essa pergunta mudar. (`app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:725`.) |
| NÃO CURADO — ciclo com UI | **Aberto, com correção da fundamentação.** A edição simples não mostra transação envolvente; a aprovação de irmãos mostra. Existe contraexemplo com reversão obsoleta e aprovação da mesma A, detalhado no item 2. (`app/ponto/views.py:2791`, `app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:654`; `app/chamados/models.py:866`.) |

Os demais achados nomeados na v5 ficam assim:

| Achado da v5 | Situação atual |
|---|---|
| Reversão sobre outra identidade/empresa; aplicar usando empresa antiga | **Fechados no código para os contraexemplos apresentados.** A comparação ocorre com C/A travados, antes da escrita. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:670`.) |
| Sobrescrita do arquivo | **Fechado.** Nome contém PID/UUID; `xb` recusa arquivo existente. (`app/ponto/management/commands/rederivar_falta_declarada.py:116`; `app/ponto/services/ausencia.py:696`; `app/ponto/tests/test_h06_reversao_guardas.py:236`.) |
| Concorrência/ranges/atomicidade | **Parcial.** Consulta todas as competências e envolve as escritas em `atomic`; compatibilidade global e arquivo externo à transação permanecem problemáticos. (`app/ponto/services/ausencia.py:559`, `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:673`, `app/ponto/services/ausencia.py:696`.) |
| Leitores H12 divergentes | **Fechado no código.** Exportação/lista usam o mesmo predicado; contador e HTML leem `rescisao`. (`app/folha/export.py:60`, `app/folha/export.py:875`; `app/ponto/services/fechamento_lista.py:109`; `app/ponto/views.py:1215`; `app/templates/ponto/_fech_linha.html:2`.) |
| Rollback na segunda reposição | **Fechado como caso estático.** O teste executa a primeira reposição real e falha na segunda. Resultado executado: **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:218`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:725`.) |
| Parcialidade 480 versus financeiro | **Aberto, preexistente.** Cobertura compara com 480; leitores financeiros ainda usam presença de minutos. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| Cadastro atual decidindo passado | **Aberto, separado no aval.** Corte vem do cadastro atual. (`app/ponto/janelas.py:50`; `app/docs/PROMPTS.md:1861`.) |
| Arquivos antigos | **Aberto como compatibilidade.** Formato antigo em lista continua recusado; existência de arquivos afetados: **[nao verificado]**. (`app/ponto/services/ausencia.py:633`.) |
| Status/data_fim/contador/travas | **Parcial.** Os casos existentes permanecem; a nova sonda fecha a falsa proteção de A, mas não prova a linha real travada por `registrar`. (`app/ponto/tests/test_h06_reversao_guardas.py:326`, `app/ponto/tests/test_h06_reversao_guardas.py:343`, `app/ponto/tests/test_h06_reversao_guardas.py:388`, `app/ponto/tests/test_h06_reversao_guardas.py:557`, `app/ponto/tests/test_h06_reversao_guardas.py:867`, `app/ponto/tests/test_h06_reversao_guardas.py:251`.) |

O protocolo de hash continua dependendo da trava da Empresa: consultar aplicado/consumido e lavrar a ação ocorre dentro dela; não há unicidade própria do hash no modelo de auditoria. Reuso é recusado com motivo, inclusive R1 após reversão e reaplicação por R2. (`app/ponto/services/ausencia.py:580`, `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:676`, `app/ponto/services/ausencia.py:685`, `app/ponto/services/ausencia.py:699`; `app/core/models.py:309`; `app/ponto/tests/test_h06_reversao_guardas.py:454`.)

**2. Efeito da mudança das travas**

Localizei dois chamadores de `travar_entrega`: `ExportacaoDominio.registrar` e a porta H06. Ambos entram em `atomic` antes da chamada. (`app/folha/models.py:179`, `app/folha/models.py:181`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:647`.)

| Escritor/par | Efeito |
|---|---|
| `registrar` × porta; porta × porta | **Continuam serializados na Empresa.** Ambos pedem `NO KEY UPDATE`, modos conflitantes entre si. (`app/folha/export.py:564`, `app/folha/export.py:570`; `app/folha/models.py:181`; `app/ponto/services/ausencia.py:647`.) |
| `invalidar` | Não chama `travar_entrega`; atualiza invalidação/motivo/substituta da ED existente. Continua podendo tornar K6 falso, mas não cria nem desinvalida uma entrega. Não ganhou serialização na Empresa. (`app/folha/models.py:143`, `app/folha/models.py:154`, `app/folha/models.py:162`; `app/folha/export.py:598`, `app/folha/export.py:600`.) |
| Admissão | Mantém Cmax `FOR UPDATE` para matrícula e INSERT de Cnovo dentro do atomic externo. A FK de Empresa passa a avançar durante a trava E da porta/entrega; disputa pelo próprio Cmax continua se ele for alvo H06. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`; `app/colaboradores/models.py:321`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`; `app/ponto/services/ausencia.py:651`.) |
| Transferência/edição cadastral | O UPDATE da mesma C continua conflitando com a porta. A checagem da FK da Empresa destino deixa de esperar a trava inicial E, eliminando a aresta do ciclo apresentado na v5. (`app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`; `app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`.) |
| `ChamadoColaborador.abrir` | **Continua serializado com a porta pela mesma C.** Seu `FOR UPDATE` explícito permanece antes do dedup/nascimento. (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:932`; `app/ponto/services/ausencia.py:651`.) |
| INSERT que apenas referencia C | Deixa de esperar a trava inicial C da porta pela FK. Isso não libera `abrir`, que pede uma trava explícita antes do INSERT. (`app/ponto/services/ausencia.py:649`, `app/ponto/services/ausencia.py:651`; `app/ponto/models.py:743`; `app/chamados/models.py:866`.) |
| CelulaDia → FK Ausencia | A checagem da FK passa durante a trava inicial A. O ciclo que dependia exclusivamente de célula segurando `KEY SHARE A` perde essa aresta; UPDATE da mesma célula continua sendo disputa de escrita. (`app/ponto/services/ausencia.py:654`; `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:810`; `app/ponto/tests/test_h06_reversao_guardas.py:877`.) |
| Edição simples de Ausencia pela UI | Continua disputando o UPDATE de A. A view e o serviço não abrem `atomic` próprio; sob autocommit, não se pode presumir retenção da trava A durante todo o signal. Transação efetiva em produção: **[nao verificado]**. (`app/ponto/views.py:2748`, `app/ponto/views.py:2791`; `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:508`; `app/config/settings/saas.py:83`.) |
| Signals da Ausencia | Continuam síncronos para o cartório. Se houver emissão, alcançam `abrir`, readquirindo C em **FOR UPDATE**. Portanto, “as três linhas ficam NO KEY durante todo o ato” não está demonstrado pela foto anterior ao save. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`; `app/ponto/tests/test_h06_reversao_guardas.py:819`.) |

**Sim, pares antes serializados incidentalmente deixaram de serializar:** trava E × FK E da admissão/transferência; trava C da porta × INSERT que apenas referencia C; trava A da porta × FK A da célula. Essa liberação é precisamente o efeito pretendido de `no_key=True`, não preservação universal da serialização anterior. (`app/folha/export.py:564`; `app/ponto/services/ausencia.py:649`; `app/ponto/tests/test_h06_reversao_guardas.py:877`.)

**Permanece um ciclo possível nas mesmas linhas, independente de FK**, neste contraexemplo estático:

1. Existe R1 de uma A anteriormente falta/aprovada. Depois, pelas portas de produção, o DP pode editar a mesma A para um tipo parcial, cancelar e reverter a rejeição com documento; a reabertura preserva o PK e pode produzir `aguardando_decisao`. (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:994`; `app/ponto/views.py:2731`; `app/ponto/services/ausencia.py:1305`, `app/ponto/services/ausencia.py:1312`, `app/ponto/services/ausencia.py:1322`, `app/ponto/services/ausencia.py:1344`.)
2. A aprovação UI entra em `atomic`, atualiza essa A e mantém sua trava. Com célula existente, cobertura parcial, resto faltante e ramo emissor habilitado, o signal pode pedir C `FOR UPDATE` por `abrir`. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1762`, `app/ponto/services/cartorio.py:1777`; `app/chamados/models.py:866`.)
3. Intercalada entre o UPDATE A e o pedido C, a reversão obsoleta obtém E e C e espera **a mesma A**. A UI segura A e espera **a mesma C**. A guarda de identidade só vem depois da obtenção de A. (`app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:664`.)

É **C→A contra A→C**; `NO KEY UPDATE` continua conflitando com esses locks de escrita. Abortar um participante não elimina a inversão. Existência dessa preparação e execução do deadlock em produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/services/ausencia.py:1038`; `app/chamados/models.py:866`.)

**3. Porta única e arquivo**

Entrega, identidade, empresa atual, minutos esperados e consumo estão centralizados nos caminhos atuais: aplicar e reverter chamam a mesma função; as decisões ficam nela. O comando não mantém cópia dessas guardas. (`app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/management/commands/rederivar_falta_declarada.py:144`; `app/ponto/services/ausencia.py:657`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:681`.)

A unicidade literal permanece incompleta: motivo é exigido pela porta e pela reposição. `repor_minutos_de_reversao` também continua gravável diretamente com motivo, embora seu único chamador de produção localizado seja a porta. Não encontrei segundo caminho atual de produção usando essa possibilidade. (`app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:532`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:725`.)

Sobre o arquivo:

- **Arquivo preexistente:** `xb` falha antes de adicioná-lo à lista de limpeza; esse caminho não apaga a sentinela de outro ato. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:698`; `app/ponto/tests/test_h06_reversao_guardas.py:236`.)
- **Falha em `write`/fechamento:** pode deixar arquivo criado, pois `escrito.append` ainda não ocorreu. O teste novo provoca falha da lavra depois da criação completa, não esse caso. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:697`, `app/ponto/services/ausencia.py:698`; `app/ponto/tests/test_h06_reversao_guardas.py:581`.)
- **Arquivo substituído no pathname:** a limpeza guarda somente o caminho, sem conferir identidade do arquivo antes de `unlink`. Se houver substituição externa, pode apagar o substituto; nenhum escritor atual que faça essa substituição foi demonstrado. Ocorrência: **[nao verificado]**. (`app/ponto/services/ausencia.py:698`, `app/ponto/services/ausencia.py:732`.)
- **Exceção depois do commit:** o `except BaseException` engloba a saída do `atomic`. O save registra `on_commit`; uma interrupção como `KeyboardInterrupt` nesse callback não é capturada pelo `engolir`, que captura somente `Exception`. Nesse contraexemplo, o banco já confirmou e a porta ainda apaga a reversão. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:730`; `app/ponto/signals.py:254`, `app/ponto/signals.py:260`, `app/ponto/signals.py:272`; `app/core/observ.py:90`.)
- **Atomic externo:** a função não exige transação externa final. Se retornar após um savepoint e o chamador depois desfizer a transação externa, o arquivo fica fora desse rollback. Os chamadores CLI atuais não abrem esse atomic externo. (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:734`; `app/ponto/management/commands/rederivar_falta_declarada.py:93`, `app/ponto/management/commands/rederivar_falta_declarada.py:122`.)

Na saída normal, o arquivo de ato confirmado permanece, como necessário para a reversão. O defeito é tratar toda exceção na saída do `atomic` como prova de que o ato não confirmou. (`app/ponto/services/ausencia.py:730`, `app/ponto/services/ausencia.py:734`.)

**4. Quem escreve e quem lê**

| Campo/valor | Quem produz/escreve | Quem lê |
|---|---|---|
| `Ausencia.minutos` | Criação recebe o valor; edição atribui número ou zero; `Ausencia.save` soma o previsto; transição persiste a derivação; reversão repõe o anterior. (`app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:1037`, `app/ponto/services/ausencia.py:531`; `app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/models.py:887`.) | Censo, guarda e pós-checagem; rateio; financeiro/crédito/efeitos; cobertura e apresentação. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/management/commands/rederivar_falta_declarada.py:69`; `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:714`; `app/folha/export.py:121`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/turnos.py:2600`; `app/templates/ponto/ausencias.html:163`.) |
| `Ausencia.tipo` | Materializadores fornecem tipo; criação e edição gravam. (`app/chamados/services/materializacao.py:468`, `app/chamados/services/materializacao.py:485`, `app/chamados/services/materializacao.py:521`; `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`.) | Censo, identidade, derivação, catálogo/efeito e cobertura parcial. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; 

[truncado em 20000 de 35057 caracteres]

## 05/11 19:09 · sessao 9e2282fbfa3a · C20-JUIZ-3 · verificador · origem=Code

**O diff conecta os dois produtores ao consumidor existente, sem demonstrar juiz ou escritor novo; porém os testes deixam passar a perda da atomicidade própria e do tratamento de falhas.** Há também uma promessa incorreta sobre a atomicidade da trilha. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/reconciliador.py:1360`, `app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/core/observ.py:105`)

Referências abaixo são da cópia fornecida. Identidade Git, ancestralidade, execução da suíte, execução dos mutantes e ocorrência produtiva dos cenários: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `EventoCelula.celula_id` | `emitir_evento_celula`, copiando `cel.pk`. (`app/ponto/portas/celula.py:643`) | Vigia de absolvição e semeadura procuram pendente por célula. (`app/chamados/reconciliador.py:1262`, `app/chamados/reconciliador.py:1284`, `app/chamados/reconciliador.py:1297`) |
| `EventoCelula.colaborador_id` | Mesma porta, copiando `cel.colaborador_id`. (`app/ponto/portas/celula.py:643`) | Consumidor agrupa/carrega colaborador; vigia constrói `_com_evento`. (`app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1344`, `app/chamados/management/commands/reconciliar_chamados.py:62`) |
| `EventoCelula.data` | Mesma porta, copiando `cel.data`. (`app/ponto/portas/celula.py:643`) | O consumidor **não restringe o trabalho à data do evento**: percorre o colaborador inteiro. Outro leitor produtivo específico desse campo: **[nao verificado]**. (`app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1432`) |
| `processado_em` | Nasce nulo; consumidor grava `agora` no sucesso. (`app/escala/models.py:1859`, `app/chamados/reconciliador.py:1380`) | Elegibilidade, contador de mortos, proteção do UPDATE e retenção. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`, `app/chamados/reconciliador.py:1372`, `app/chamados/reconciliador.py:1387`) |
| `tentativas` | Default zero; consumidor incrementa com `F` na falha. (`app/escala/models.py:1860`, `app/chamados/reconciliador.py:1374`) | Separação entre pendentes vivos e mortos. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`) |
| `erro` | Default vazio; consumidor grava falhas concatenadas, limitadas a 500 caracteres. Leitor produtivo específico: **[nao verificado]**. (`app/escala/models.py:1861`, `app/chamados/reconciliador.py:1374`) | **[nao verificado]** |
| `TurnoMaterializado` | Núcleo monta linhas, apaga recorte e faz `bulk_create`. (`app/ponto/nucleo.py:48`, `app/ponto/nucleo.py:58`, `app/ponto/nucleo.py:59`) | O novo `RETURNING` lê dia/entrada/saída; a diferença entre identidades abertas antigas e novas determina emissão. (`app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:79`, `app/ponto/nucleo.py:80`) |
| `ChamadoColaborador.status_local` | Nascimento por `abrir`/`criar`; transições por `_gravar_estado`. O consumidor chega pela sequência `_retratar → reconciliar → _gravar_estado`. (`app/chamados/models.py:932`, `app/chamados/models.py:1054`, `app/chamados/models.py:222`, `app/chamados/reconciliador.py:523`, `app/chamados/models.py:620`) | Seleção de vivos, motor de transição, signals e painel. (`app/chamados/reconciliador.py:1433`, `app/chamados/models.py:639`, `app/chamados/signals.py:70`, `app/chamados/services/painel.py:211`) |
| `LogAuditoria/fechamento_recusado` | Gates de premissa/pergunta chamam `_registrar_recusa`; este deduplica e delega o INSERT a `core.observ.evento`. (`app/chamados/models.py:730`, `app/chamados/models.py:408`, `app/chamados/models.py:386`, `app/chamados/models.py:391`, `app/core/observ.py:134`) | Próprio deduplicador, consulta genérica de auditoria e testes novos. (`app/chamados/models.py:386`, `app/core/views.py:705`, `app/chamados/tests/test_c20_juiz3_outbox.py:239`) |
| Retorno `premissa` | Inicializado em zero, incrementado pelo retorno da função extraída e devolvido no dicionário. (`app/chamados/reconciliador.py:1327`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1388`) | Testes leem; **o command consumidor não imprime essa chave**, embora imprima `retratados`. Outro leitor produtivo: **[nao verificado]**. (`app/chamados/tests/test_c20_juiz3_outbox.py:254`, `app/chamados/management/commands/consumir_eventos_celula.py:32`) |

Os chamadores do INSERT são `salvar_e_emitir`, nascimento por `abrir`, nascimento por `criar`, recompute e semeadura; todos delegam à mesma porta. Os dois novos produtores não inserem `EventoCelula` diretamente. (`app/ponto/portas/celula.py:654`, `app/chamados/models.py:941`, `app/chamados/models.py:1058`, `app/ponto/nucleo.py:90`, `app/chamados/reconciliador.py:1286`)

**Não identifiquei escritor explícito novo fora da porta no caminho alterado:** turno permanece em `ponto/nucleo.py`, e estado do chamado em `chamados/models.py`, ambos autorizados. `EventoCelula` e `LogAuditoria` não têm entrada própria em `PORTAS`; seus donos estão declarados no modelo do evento e em `core.observ.evento`. A manutenção de processamento pelo consumidor escreve campos distintos dos identificadores inseridos pela porta. (`app/core/portas.py:117`, `app/core/portas.py:233`, `app/core/portas.py:38`, `app/escala/models.py:1851`, `app/core/observ.py:96`, `app/chamados/reconciliador.py:1374`)

**2. Equivalência do DELETE e outros escritores**

Para colaborador persistido, modelo e configuração presentes, **o conjunto apagado é o mesmo**: colaborador por PK e intervalo inclusivo de `data_turno`. O `RETURNING` não limita a exclusão aos abertos: apaga todo o recorte e depois seleciona, no resultado, entrada presente/saída ausente. (`app/ponto/nucleo.py:78`, `app/ponto/nucleo.py:80`; filtro anterior no diff fornecido)

A classe atual é simples, sem manager ou override de exclusão; suas FKs são **saintes**, para colaborador e batidas. Elas não significam apagar colaborador/batidas ao excluir o turno. Dependente entrante ou receiver específico de exclusão de `TurnoMaterializado` não foi localizado; cobertura de registros dinâmicos externos: **[nao verificado]**. (`app/ponto/models.py:1196`, `app/ponto/models.py:1201`, `app/ponto/models.py:1204`, `app/ponto/models.py:1206`)

Logo, **não há perda concreta de signal/cascata de saída demonstrada nesta cópia**, mas o SQL não conserva genericamente o mecanismo de exclusão do ORM: usa diretamente a conexão padrão e executa o DELETE. Equivalência sob outro roteador/alias ou dependentes adicionados depois: **[nao verificado]**. (`app/ponto/nucleo.py:74`, `app/ponto/nucleo.py:76`, `app/ponto/nucleo.py:78`)

Além do núcleo, existe exclusão indireta por cascata ao apagar colaborador ou batida. A porta de batida chama `.delete()`, e seu receiver recompõe depois da exclusão; portanto um turno já eliminado pela cascata pode não estar no `RETURNING`. Esse detector não garante evento para tal caminho. (`app/ponto/models.py:1202`, `app/ponto/models.py:1205`, `app/ponto/models.py:1207`, `app/ponto/registro_batida.py:291`, `app/ponto/signals.py:48`, `app/ponto/nucleo.py:80`)

**Há perda de cobertura do censo:** o novo `cur.execute` fica invisível ao reconhecedor de escritas ORM usado pelo gate de turno. Isso não cria outro escritor, mas enfraquece a capacidade do selo de enxergar esse DELETE. (`app/ponto/nucleo.py:77`, `app/core/censo_escritas.py:364`, `app/core/censo_escritas.py:377`, `app/ponto/tests/test_chokepoint_turno_gate.py:35`)

**3. Atomicidade e chamadores de recompute**

**Sim:** `criar` envolve chamado e evento no mesmo `atomic(savepoint=False)`; recompute envolve DELETE, recriação e emissão no mesmo `atomic`. Não aparece nesses blocos caminho normal de commit do evento elegível com rollback do respectivo chamado/materializado, ou o inverso. (`app/chamados/models.py:1053`, `app/chamados/models.py:1058`, `app/ponto/nucleo.py:57`, `app/ponto/nucleo.py:66`)

Existem estados sem evento por desenho: módulo fora da lista, chamado sem célula, recompute sem identidade aberta removida e fechamento em dia sem célula. Portanto a cobertura não é “qualquer nascimento/qualquer recompute”. (`app/chamados/models.py:1056`, `app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:89`)

A garantia também não engloba necessariamente a **escrita de origem**: batida/vínculo podem persistir enquanto recompute e evento falham, pois os receivers engolem a exceção; flip salva a alteração antes da recomposição. Isso não separa evento de materializado dentro do atomic do núcleo. (`app/ponto/signals.py:29`, `app/ponto/signals.py:67`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/flip_batida.py:39`)

Chamadores produtivos encontrados por busca textual — execução de `git grep`/completude histórica: **[nao verificado]**:

| Chamador | Atomic externo local |
|---|---|
| Save/delete de batida, via `_recompute_around` | Não abre atomic no wrapper. (`app/ponto/signals.py:19`, `app/ponto/signals.py:26`, `app/ponto/signals.py:48`) |
| Save de vínculo, via `_recompute_escala` | Não abre atomic no wrapper. (`app/ponto/signals.py:53`, `app/ponto/signals.py:64`) |
| Alertas de turno | Sem atomic externo local. (`app/ponto/management/commands/processar_alertas_turno.py:32`, `app/ponto/management/commands/processar_alertas_turno.py:67`) |
| Alertas avançados | Sem atomic externo local. (`app/ponto/management/commands/processar_alertas_avancados.py:19`, `app/ponto/management/commands/processar_alertas_avancados.py:57`) |
| Materialização explícita | Sem atomic externo local. (`app/ponto/management/commands/materializar_turnos.py:16`, `app/ponto/management/commands/materializar_turnos.py:24`) |
| Flip | Sem atomic externo local. (`app/ponto/services/flip_batida.py:10`, `app/ponto/services/flip_batida.py:39`) |
| Veredito humano | Dentro do atomic iniciado antes. (`app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:308`) |

**A prosa nova promete mais que o código:** “create, trilha e INSERT juntos ou nenhum” não vale para falha da trilha, porque `evento()` tem savepoint próprio e engole a exceção. Chamado e evento podem persistir sem a trilha de nascimento. Também é incorreto dizer que as guardas anteriores “não escrevem nada”: recusas/vetos podem gravar auditoria antes do create. (`app/chamados/models.py:1050`, `app/chamados/models.py:1175`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/chamados/models.py:1034`, `app/chamados/models.py:1038`)

**4. Classes de reprovação**

| Classe | Incorre? |
|---|---|
| Segundo juiz duplicando autoridade | **Não demonstrado.** O produtor detecta mudança no materializado; o consumidor continua perguntando `_veredito_premissa`, que delega ao catálogo registrado. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1437`, `app/chamados/reconciliador.py:301`, `app/core/juizes.py:263`) |
| Segundo escritor fora da porta | **Não demonstrado no diff.** Novas emissões delegam à porta existente; transição continua no gate do chamado. (`app/ponto/nucleo.py:90`, `app/chamados/models.py:1058`, `app/chamados/models.py:620`) |
| Relógio fora da borda | **Nenhuma leitura nova demonstrada.** Os trechos acrescentados trabalham com estado/dias recebidos; o relógio do consumidor já é capturado na entrada. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1325`, `app/chamados/reconciliador.py:1437`) |
| Derivação paralela de fonte única | **Não demonstrada para a decisão da premissa.** A diferença de conjuntos produz notificação; não substitui o julgamento existente. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1437`) |
| Selo/RED sem mordida | **Parcialmente sim:** atomic próprio e propagação de falhas têm mutantes sobreviventes previstos; DELETE cru sai do alcance do censo. (`app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/tests/test_c20_juiz3_outbox.py:196`, `app/chamados/reconciliador.py:1447`, `app/core/censo_escritas.py:377`) |
| Ordem de trava | Não há nova trava explícita de colaborador/disputa no consumidor; o fechamento trava o chamado pela porta existente. Deadlock efetivo: **[nao verificado]**. (`app/chamados/reconciliador.py:1311`, `app/chamados/models.py:668`) |
| Pouso antes de pré-requisito | **[nao verificado]**: o material entregue não prova sequência de pousos, suíte ou leitura final; essas condições estão documentadas. (`app/docs/PISTA.md:108`, `app/docs/PISTA.md:113`) |

**5. Extração e `tranca_em_lote`**

No caminho antigo, **a extração preserva ordem, universo, decisão, exceção e retorno**: perguntas absolvidas, órfãs, premissa; vivos do colaborador ordenados por PK; mesmo juiz, mesma retratação; exceção logada/ignorada; soma final mantida. (`app/chamados/reconciliador.py:1411`, `app/chamados/reconciliador.py:1420`, `app/chamados/reconciliador.py:1432`, `app/chamados/reconciliador.py:1443`, `app/chamados/reconciliador.py:1445`; comparação com o diff fornecido)

O consumidor acrescenta deliberadamente `falhas`: exceções por chamado entram na lista e impedem marcar o lote como processado. Um `False` por retenção ou gate **não é exceção** e não impede processamento do evento. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1447`, `app/chamados/reconciliador.py:1373`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:523`)

**Sim, chama fora de `tranca_em_lote`.** Esse contexto é memo em `ContextVar`, não lock SQL, e está expressamente reservado a leitor que não escreve trilha; o consumidor executa escritas. (`app/chamados/reconciliador.py:1360`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:994`)

**6. Janela criada pelo skip do vigia**

- **Tentativas esgotadas:** não mascaram por si: saem de `eventos_pendentes`; são contadas como eventos mortos e entram no alarme. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`, `app/chamados/management/commands/reconciliar_chamados.py:118`)
- **Processado sem retratar:** sem outro pendente do colaborador, volta ao julgamento do vigia. Recusa/retenção legítima fica como MEDIDA. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:79`)
- **Evento de outro dia:** exclui o colaborador inteiro da contagem, mas o consumidor também julga todos os seus vivos; o dia diferente, sozinho, não prova abandono. (`app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1432`)
- **Há uma janela por fotografia:** `_com_evento` é calculado antes do laço; consumo posterior não atualiza esse conjunto nesta execução. Ocorrência/duração produtiva: **[nao verificado]**. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:71`)
- **Há mascaramento prolongável:** órfã adiada mantém evento pendente sem gastar tentativas; enquanto existir, exclui também a premissa do contador. O consumidor ainda tenta julgá-la, mas idade/quantidade de pendentes são somente MEDIDA, fora da soma de alarme. Isso não prova julgamento bem-sucedido. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1375`, `app/chamados/management/commands/reconciliar_chamados.py:113`, `app/chamados/management/commands/reconciliar_chamados.py:118`)

**7. Os testes mordem? Mutantes** [nao verificado]

**Mordem comportamento real:** emissão, exclusão de módulo estranho, fechamento/repetição/preservação do aberto, pai retratado, recusa do furo e reentrega idempotente. (`app/chamados/tests/test_c20_juiz3_outbox.py:99`, `app/chamados/tests/test_c20_juiz3_outbox.py:109`, `app/chamados/tests/test_c20_juiz3_outbox.py:135`, `app/chamados/tests/test_c20_juiz3_outbox.py:146`, `app/chamados/tests/test_c20_juiz3_outbox.py:155`, `app/chamados/tests/test_c20_juiz3_outbox.py:242`, `app/chamados/tests/test_c20_juiz3_outbox.py:271`)

Dois mutantes relevantes têm sobrevivência prevista por inspeção; execução: **[nao verificado]**:

1. **Retirar `falhas=falhas` da chamada nova.** Caminhos felizes permanecem; exceção por chamado é engolida sem avisar o consumidor, que pode marcar o evento processado. Os erros injetados pelos testes novos são no INSERT do evento, não nesse julgamento. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1445`, `app/chamados/reconciliador.py:1380`, `app/chamados/tests/test_c20_juiz3_outbox.py:123`, `app/chamados/tests/test_c20_juiz3_outbox.py:194`)
2. **Retirar os atomics próprios de `criar`/recompute, reindentando os corpos.** A base usa `TestCase`, e os dois testes de falha ainda fornecem atomic externo. Assim, não distinguem proteção própria de proteção do chamador; em autocommit, o mutante permitiria estado persistido antes da falha do evento. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/tests/test_c20_juiz3_outbox.py:37`, `app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/tests/test_c20_juiz3_outbox.py:196`)

**8. Contradição com aval/PROIBIDOS**

Não aparece no diff fornecido nova reclassificação, alteração da nota/listas do contrato, juiz novo ou fallback novo; o caminho principal realiza evento em `criar`/recompute e consumo pelo O-OUTBOX existente. Isso **não equivale a certificar cobertura completa ou fechamento do contrato**. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/reconciliador.py:1360`, `app/core/contratos_estruturais.py:263`, `app/core/contratos_estruturais.py:269`)

**Ressalva do “selo 17”:** a cópia já contém teto **18** para `_gravar_estado (sem pergunta)`, enquanto a prosa diz 17. O arquivo desse teto não está no diff anexado; portanto não atribuo essa subida à obra. Origem histórica e conformidade desse 18 com o aval literal: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`, `app/docs/RELATO.md:25`)

Os pontos concretos que impedem um aceite sem ressalvas são a prova insuficiente de atomicidade/retry, a exclusão do DELETE do alcance do censo e a promessa falsa sobre a trilha; o novo contador `premissa` também fica ausente da saída produtiva do command. (`app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/reconciliador.py:1447`, `app/core/censo_escritas.py:377`, `app/core/observ.py:105`, `app/chamados/management/commands/consumir_eventos_celula.py:32`)

## 05/11 19:07 · sessao b8013929f9c6 · - · - · origem=Ronald (tmux)

O contraexemplo de **reversão obsoleta × aprovação UI** fecha a lacuna das mesmas linhas, como inferência estática:

1. R1 foi emitido para A falta/aprovada; posteriormente o DP edita **a mesma A** para um tipo parcial e minutos abaixo de 480, cancela e reverte a rejeição com documento. A reabertura preserva o PK e usa `estado_inicial`, que com documento devolve `aguardando_decisao`. A edição de tipo da aprovada, isoladamente, **não** a reabre, pois `rejulgar_status_pelo_tipo` só atua em ABERTAS. (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:744`, `app/ponto/services/ausencia.py:994`; `app/ponto/views.py:2725`, `app/ponto/views.py:2731`; `app/ponto/services/ausencia.py:1297`, `app/ponto/services/ausencia.py:1305`, `app/ponto/services/ausencia.py:1312`, `app/ponto/services/ausencia.py:1322`, `app/ponto/services/ausencia.py:1344`.)

2. A aprovação UI abre atomic, atualiza A e mantém essa trava durante o signal. Com célula existente, tipo parcial vigente, cobertura parcial com resto faltante, data no horizonte e sem chamado vivo, o cartório pode emitir o chamado do resto e pedir **FOR UPDATE C**. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/ponto/turnos.py:2598`; `app/ponto/services/cartorio.py:1676`, `app/ponto/services/cartorio.py:1762`, `app/ponto/services/cartorio.py:1777`, `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`.)

3. Intercale a reversão de R1 depois do UPDATE A e antes desse pedido C: a porta obtém E e **NO KEY UPDATE C**, depois espera A. A UI segura A e espera C. A guarda de identidade da reversão só será avaliada **depois** de obter A, portanto não evita o ciclo. (`app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:664`, `app/ponto/services/ausencia.py:665`.)

Logo permanece um ciclo de travas explícitas **porta C→A × aprovação UI A→C**, com o mesmo C e o mesmo PK A; a troca `FOR UPDATE`→`NO KEY UPDATE` não elimina conflito com o `FOR UPDATE C` do gate de chamados nem com o UPDATE A. O arquivo obsoleto é suficiente; não depende de H06 censar uma Ausencia aberta. As condições descritas e a execução do deadlock em produção são [nao verificado]. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/chamados/models.py:866`; `app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:1038`.)

## 05/11 19:06 · sessao b8013929f9c6 · - · - · origem=Ronald (tmux)

Censo em `app/**/*.py`: os únicos chamadores de `travar_entrega` são `ExportacaoDominio.registrar` e `porta_do_ato_h06`; ambos abrem `atomic` antes da chamada. (`app/folha/models.py:179`, `app/folha/models.py:181`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:647`.) Não apareceu outro comando chamando essa função na pesquisa; exaustividade além da cópia: [nao verificado].

| Caminho | Efeito da v6 |
|---|---|
| `registrar` × porta; porta × porta | Continuam serializados pela mesma Empresa em `NO KEY UPDATE`; Colaborador e Ausencia também passam a `NO KEY UPDATE`. (`app/folha/export.py:570`; `app/folha/models.py:181`; `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`.) |
| `registrar` × admissão/transferência para a Empresa | A proteção anteriormente incidental por conflito com a checagem FK deixa de existir: o modo novo admite `KEY SHARE`. O teste novo exige justamente isso. (`app/folha/export.py:564`, `app/folha/export.py:570`; `app/ponto/tests/test_h06_reversao_guardas.py:885`.) |
| Admissão | Mantém atomic externo, trava Cmax para matrícula e depois insere Cnovo com FK Empresa. A mudança em E elimina a espera da FK de E contra a porta; a disputa pelo próprio Cmax continua, se for alvo da porta. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`; `app/colaboradores/models.py:321`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`; `app/ponto/services/ausencia.py:651`.) |
| Transferência/edição cadastral | Mantêm UPDATE de C no atomic; a mudança de empresa referencia E destino. A porta continua disputando a mesma linha C, mas sua trava E deixa a checagem da FK avançar. (`app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`; `app/colaboradores/services/colaborador.py:76`; `app/ponto/services/ausencia.py:651`.) |
| `ChamadoColaborador.abrir` | Continua tomando **FOR UPDATE C**, portanto continua conflitando com a trava C da porta. Não perdeu sua serialização de dedup/nascimento. (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:932`; `app/ponto/services/ausencia.py:651`.) |
| INSERTs que apenas referenciam C | Deixam de ser barrados pela trava inicial C da porta; isso não equivale a liberar `abrir`, que toma trava explícita forte antes do INSERT. (`app/ponto/services/ausencia.py:649`, `app/ponto/services/ausencia.py:651`; `app/chamados/models.py:866`, `app/chamados/models.py:932`.) |
| Cartório/Celula→Ausencia | A trava inicial A da porta deixa avançar a checagem FK da célula para A; o cartório atribui a FK e salva a célula em atomic. O antigo ciclo que dependia **somente** de `KEY SHARE A` versus `FOR UPDATE A` perde essa aresta. (`app/ponto/services/ausencia.py:654`; `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:815`; `app/ponto/tests/test_h06_reversao_guardas.py:885`.) |
| `ExportacaoDominio.invalidar` | Não chama a trava E; atualiza apenas invalidação/motivo/substituta em ED. O juiz K6 passa a falso quando o vigente está invalidado, mas `invalidar` não cria nem desinvalida ED. A v6 não acrescentou uma nova serialização para ele. (`app/folha/models.py:143`, `app/folha/models.py:154`, `app/folha/models.py:158`, `app/folha/models.py:162`; `app/folha/export.py:598`, `app/folha/export.py:600`.) |

A alegação “a porta fica NO KEY nas três linhas durante todo o ato” é excessiva: seu save dispara `post_save`; quando o cartório alcança emissão, chama `ChamadoColaborador.abrir`, que readquire C em `FOR UPDATE`. O teste de `KEY SHARE` para na foto **anterior** à escrita e não examina esse modo posterior. Alcançabilidade efetiva desse ramo para uma falta específica de produção: [nao verificado]. (`app/ponto/services/ausencia.py:693`, `app/ponto/services/ausencia.py:711`, `app/ponto/services/ausencia.py:725`; `app/ponto/signals.py:143`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`; `app/ponto/tests/test_h06_reversao_guardas.py:819`, `app/ponto/tests/test_h06_reversao_guardas.py:885`.)

**Correção importante à triagem sobre o ciclo UI A→C.** A view de edição simples não abre `atomic`, e o serviço também não: chama `Ausencia.save()` diretamente. O perfil SaaS da cópia não declara `ATOMIC_REQUESTS`. Sob autocommit, não se pode presumir que o UPDATE A permaneça travado durante o signal posterior; portanto o ciclo dessa view, sozinho, não está demonstrado. Transação efetiva em produção: [nao verificado]. (`app/ponto/views.py:2747`, `app/ponto/views.py:2791`; `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:508`; `app/ponto/models.py:908`; `app/config/settings/saas.py:83`.)

Há um caminho UI com atomic explícito: aprovação de irmãos envolve o laço em `atomic`, salva Ausencia e pode chegar a C pelo signal. Ele tem ordem potencial **A→C**, inversa à porta **C→A**; a troca para `NO KEY` não elimina conflito entre esses locks de escrita/explícitos. Porém a seleção desse caminho é por estados ABERTAS, enquanto H06 censa falta aprovada; afirmar ciclo executável nas **mesmas linhas** exige apresentar o interleaving que supera essa diferença e habilita o ramo emissor. Esse interleaving e sua execução permanecem [nao verificado]. (`app/ponto/views.py:2603`, `app/ponto/views.py:2613`, `app/ponto/views.py:2615`, `app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/chamados/models.py:866`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/management/commands/rederivar_falta_declarada.py:55`.)

Os ciclos v5 admissão/transferência por FK de E e célula por FK de A foram tratados no modo inicial das travas; uma garantia global de ausência de ciclos, incluindo upgrades e escritores transacionais, não foi demonstrada pelos testes novos, que sondam a porta antes do primeiro save. (`app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/tests/test_h06_reversao_guardas.py:819`, `app/ponto/tests/test_h06_reversao_guardas.py:875`, `app/ponto/tests/test_h06_reversao_guardas.py:885`.)

_1985 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
