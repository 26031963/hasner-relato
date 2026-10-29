# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 16:41** · publicado em **29/10/2026 16:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 29/10 15:56 · sessao 4b77cd4c0b0c · - · - · origem=Ronald (tmux)

- **Levanta véu:** `desarquivar_chamado` chama `arquivo.levantar(...resgatado=True)` em `app/chamados/views_arquivar.py:75`; antes, chama `ch.reabrir()` somente quando `via_encerramento == competencia_trancada`, em `app/chamados/views_arquivar.py:64` / `:68`.
- **Reabre sem levantar:** `ChamadoColaborador.reabrir`, `app/chamados/models.py:1023`, grava estado em `:1089` e ressuscita perguntas em `:1098`; `_gravar_estado` persiste campos sem véu (`app/chamados/models.py:193`, `:231`).
- **Outra porta sem levantar:** `transicionar`, `app/chamados/models.py:122` / `:183` / `:260`; permite `resolvido→em_analise` e admin sair de fechado (`app/chamados/catalogo/motor.py:102`, `:150`). Entradas: `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:315`; `processar_post_fio`, `app/chamados/services/fio.py:715`.
- **Outros levantadores:** `_ressuscitar_por_resposta`, `app/chamados/services/materializacao.py:663`, com `resgatado=True`; `reconciliar_por_ausencia._levantar`, `app/chamados/services/silencio_ausencia.py:72`, com `resgatado=False`.
- `arquivo.levantar` limpa `silenciado_ate`, autor e motivo em `app/chamados/services/arquivo.py:201` e salva em `:208`.

Chamadores de `reabrir()` que não chamam `levantar`:

| Função/caminho | Chamada |
|---|---|
| `_renascer_por_premissa_viva` | `app/chamados/models.py:770` |
| `acao_reabrir_disputa_admin` | `app/chamados/services/acoes_disputa.py:266` |
| `competencia_trancada.reabrir` | `app/chamados/services/competencia_trancada.py:258` |
| `sincronizar_adesao` | `app/chamados/services/adesao.py:92` |
| `resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1606` |
| `reabrir_orfas_enterradas.Command.handle` | `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` |
| `reabrir_chamados_ausencia_aberta.Command.handle` | `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` |
| `reconciliar_grade.Command._emitir_furo` | `app/ponto/management/commands/reconciliar_grade.py:519` |
| `detectar_ausencias.Command.handle` | `app/ponto/management/commands/detectar_ausencias.py:301` |

`analisar` e `reconciliar` não reabrem encerrados: `app/chamados/catalogo/motor.py:112` e `:111`.

## 29/10 15:55 · sessao cb10afb99300 · - · - · origem=Ronald (tmux)

Escritores diretos de produção:

| Função | Escrita | `resposta_colab` vazia com carimbo? |
|---|---|---|
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1248` | Não: rejeita vazia em `:1219–1221`. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1393` | Função permite `texto=''`; caller atual fornece constante não vazia (`app/api/foto_recebida.py:25`, `:48`). |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1409` | Não: guarda em `:1407`. |
| `resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1572`, `:1649` | Primeiro exige texto (`:1570–1571`); **segundo não exige texto** (`:1648–1651`). |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:513` | Nova atribuição exige texto (`:511–512`); save inclui campo mesmo sem texto, preservando eventual legado (`:528–530`). |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1192` | Não: exige texto (`:1190–1191`). |

Zeradores: `ChamadoColaborador.ressuscitar_perguntas` grava `None` em `app/chamados/models.py:1163` (universo sem resposta: `:1115`); `reabrir_para_correcao` zera carimbo e texto em `app/chamados/services/disputa_emissao.py:1358`, persistido em `:1377`.

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

_131 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
