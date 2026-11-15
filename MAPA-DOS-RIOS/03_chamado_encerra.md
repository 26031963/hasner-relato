# MAPA DOS RIOS — ChamadoColaborador ENCERRA

Convencao: caminhos abaixo relativos a `app/`. Cada linha da matriz representa um par origem → destino; portas alternativas ficam na mesma linha. `ENCERRADOS` significa `resolvido/fechado`; `TERMINAIS` acrescenta `cancelado/superado`. (`chamados/catalogo/motor.py:30`, `chamados/catalogo/motor.py:31`)

## 1. RESUMO

1. Ha **7 estados declarados**, sem estado literal `encerrado`. (`chamados/catalogo/motor.py:19`)
2. O encerramento runtime admite **11 pares origem → destino**, realizados por **21 combinacoes de par/porta**: humana 8, reconciliar 3, encerrar 4, competencia trancada 3 e cancelar 3. (`chamados/catalogo/motor.py:99`, `chamados/catalogo/motor.py:111`, `chamados/catalogo/motor.py:114`, `chamados/catalogo/motor.py:116`, `chamados/catalogo/motor.py:153`, `chamados/models.py:1677`)
3. Sao **5 portas decisorias** e **1 atalho**, `fechar`; o escritor runtime da transicao e `_gravar_estado`. (`chamados/models.py:128`, `chamados/models.py:452`, `chamados/models.py:642`, `chamados/models.py:820`, `chamados/models.py:1652`, `chamados/models.py:1669`, `chamados/models.py:296`)
4. O censo abaixo enumera **50 sitios externos de encerramento**: R=34, E=4, T=1, C=2, H=8, atalho `fechar`=1; o encaminhamento interno de `fechar` acrescenta um sitio, sem acrescentar transicao. (`chamados/models.py:455`; sitios discriminados na secao 2)
5. A migration 0039 acrescenta **2 pares historicos**: `cancelado → superado` e a reversa. (`chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)
6. L2: **0 escritores externos encontrados no runtime; 4 sitios de estado em migracoes**, dos quais 2 pertencem ao encerramento. Os selos excluem migracoes. (`chamados/models.py:296`, `chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`, `chamados/tests/test_selo_status_so_pelas_portas.py:34`)
7. `cancelar` e `encerrar` tambem perguntam pela pergunta viva; a descricao “sem guard” de `encerrar` nao descreve seu corpo atual. (`chamados/models.py:833`, `chamados/models.py:1654`, `chamados/models.py:1664`)
8. L3: **2 terminais sem saida runtime; 0 sem saida considerando tambem ida/reversa das migracoes**. Ocorrencias e frequencias em producao: **[nao verificado]**. (`chamados/catalogo/motor.py:104`, `chamados/catalogo/motor.py:106`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

## 2. TABELA DE TRANSICOES

**Portas e juizes usados na matriz:**

- **H — `transicionar`**, tambem alcancada por `fechar`: `pode_transicionar` consulta o motor; verifica disputa viva para ambos os destinos, ancoradas quando a origem e viva, justificativa pendente para `fechado`, furo/premissa para `resolvido`. (`chamados/models.py:119`, `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:165`, `chamados/models.py:181`, `chamados/models.py:455`)
- **R — `reconciliar`**: `_decisao_do_fechamento` consulta motor, premissa, conversa do pai e pergunta viva, nessa ordem. (`chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689`)
- **E — `encerrar`**: motor `ENCERRAR` e pergunta viva. (`chamados/models.py:1661`, `chamados/models.py:1664`)
- **T — `encerrar_por_competencia_trancada`**: exige origem viva, motor `ENCERRAR` e pergunta viva. A verificacao da tranca esta no servico chamador. (`chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684`, `chamados/services/competencia_trancada.py:95`, `chamados/services/competencia_trancada.py:192`)
- **C — `cancelar`**: motor `CANCELAR` e pergunta viva. (`chamados/models.py:830`, `chamados/models.py:833`)
- **Nucleo comum**: antes da atribuicao, `_gravar_estado` ainda possui uma guarda que levanta erro se a travessia vivo → terminal deixar pergunta viva ancorada. (`chamados/models.py:219`, `chamados/models.py:224`, `chamados/models.py:225`, `chamados/models.py:228`)

**Persistencia comum P:** `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; rebaixamento de urgencia e preservacao da original quando aplicavel. H tambem escreve `respondido_por`; C escreve esse autor somente quando recebido. O carimbo existente e preservado entre encerrados. (`chamados/models.py:228`, `chamados/models.py:229`, `chamados/models.py:230`, `chamados/models.py:243`, `chamados/models.py:246`, `chamados/models.py:835`)

**Trilha comum:** tentativa de `LogAuditoria.acao='estado_chamado'`, com antes/depois, via e ator, antes do `save`, dentro do atomic. Essa trilha e best-effort. O wrapper coleta recusas S143 e escreve Pauta para destinos em `ENCERRADOS`; `cancelado` passa pelo ramo sem esse coletor. (`chamados/models.py:280`, `chamados/models.py:292`, `chamados/models.py:296`, `core/observ.py:98`, `core/observ.py:105`, `chamados/services/fecho_runtime.py:158`, `chamados/services/fecho_runtime.py:168`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") |
|---|---|---|---|---|---|---|---|
| 1 | aberto → resolvido | H: `chamados/models.py:132`, `chamados/models.py:181`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689` | H `transicionar`/`fechar`; R `reconciliar`; nascentes H/R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; H via `admin`, R via `sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 2 | em_analise → resolvido | H: `chamados/models.py:132`, `chamados/models.py:181`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689` | H `transicionar`/`fechar`; R `reconciliar`; nascentes H/R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 3 | registrado → resolvido | H somente `admin_ti`: `chamados/catalogo/motor.py:153`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689`; guardas H acima | H `transicionar`/`fechar`; R `reconciliar`; nascentes parametrizadas H e nascentes R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_humana_espelha_pode_transicionar` (`:27`); `::test_selo_sistema` (`:107`) |
| 4 | fechado → resolvido | H somente `admin_ti`: `chamados/catalogo/motor.py:150`, `chamados/catalogo/motor.py:153`; disputa e premissa: `chamados/models.py:154`, `chamados/models.py:181` | H `transicionar`/`fechar`; entradas parametrizadas H abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; via `admin`; preserva carimbo existente; autor e trilha (`chamados/models.py:243`, `chamados/models.py:450`) | `chamados/signals.py:97`; `chamados/services/painel.py:221` | `chamados/tests/test_motor_invariantes.py::test_fechado_so_sai_por_reabrir_ou_admin_ti` (`:61`) |
| 5 | aberto → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; T acrescenta nota/evento no servico (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:210`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/views_arquivar.py:64` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`); `chamados/tests/test_competencia_trancada.py::test_MORDE_chamado_de_dia_trancado_em_decidir_sai_pelo_ato_de_trancar` (`:68`) |
| 6 | em_analise → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; trilha comum e complemento T (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/views_arquivar.py:164` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 7 | registrado → fechado | H: `chamados/catalogo/motor.py:105`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; trilha comum e complemento T (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/services/painel.py:222` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 8 | resolvido → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`; E: `chamados/models.py:1661`; guarda de pergunta retorna cedo fora de VIVOS (`chamados/models.py:417`) | H `transicionar`/`fechar`; E `encerrar`; nascentes H/E abaixo; T recusa essa origem (`chamados/models.py:1677`) | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; preserva carimbo existente; via `admin` ou `cascade`; trilha comum (`chamados/models.py:243`, `chamados/models.py:450`, `chamados/models.py:1666`) | `chamados/signals.py:97`; `chamados/services/painel.py:222` | `chamados/tests/test_encerrar_gate.py::test_resolvido_para_fechado` (`:20`); `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`) |
| 9 | aberto → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota `cancelamento` se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:836`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`); `chamados/tests/test_o261e_f1_porta_encerramento.py::test_g_RED_cancelar_com_viva_recusa` (`:101`) |
| 10 | em_analise → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`) |
| 11 | registrado → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`) |
| 12 | cancelado → superado — migracao | Modulo duplicavel, data presente e irmao unico elegivel (`chamados/migrations/0039_a3_via_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:16`, `chamados/migrations/0039_a3_via_backfill.py:20`, `chamados/migrations/0039_a3_via_backfill.py:23`) | `backfill`, registrado em `RunPython` (`chamados/migrations/0039_a3_via_backfill.py:39`) | `queryset.update`, `chamados/migrations/0039_a3_via_backfill.py:24` | `status_local`, via `superado`, `superado_por_id`; essa escrita nao carimba `resolvido_em` nem chama a trilha runtime (`chamados/migrations/0039_a3_via_backfill.py:24`) | `chamados/services/historico.py:145`, `chamados/services/historico.py:178` | `chamados/tests/test_esmeril2_migracoes_reversas.py::test_0039_superado_ida_volta_idempotente` (`:33`); `::test_0039_irmao_alheio_nao_barra_nem_e_escolhido` (`:42`) |
| 13 | superado → cancelado — reversa | Filtro `status_local='superado'` (`chamados/migrations/0039_a3_via_backfill.py:33`) | `desfazer`, reversa do `RunPython` (`chamados/migrations/0039_a3_via_backfill.py:39`) | `queryset.update`, `chamados/migrations/0039_a3_via_backfill.py:33` | Estado `cancelado`, limpa `superado_por`; depois limpa via de todos os chamados (`chamados/migrations/0039_a3_via_backfill.py:33`, `chamados/migrations/0039_a3_via_backfill.py:34`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_esmeril2_migracoes_reversas.py::test_0039_superado_ida_volta_idempotente` (`:33`) |

### Nascentes diretas da matriz

**R — 34 sitios, todos pedindo `resolvido`:**

- `_retratar`: `chamados/reconciliador.py:524`; signal `disputa_fechada_fecha_chamado_pai`: `chamados/signals.py:43`.
- `fechar_adesao_com_batida`: `colaboradores/services/pos_vinculo.py:43`; comando, `_marcar`: `colaboradores/management/commands/marcar_feriado_do_posto.py:141`.
- `julgar_colab` e `_julgar_colab_corpo`, **6 sitios**: `ponto/services/cartorio.py:1011`, `ponto/services/cartorio.py:1539`, `ponto/services/cartorio.py:1552`, `ponto/services/cartorio.py:1573`, `ponto/services/cartorio.py:1651`, `ponto/services/cartorio.py:1666`.
- `detectar_ausencias`, **4 sitios**: `handle`, `_fechar_por_afastamento`, `_fechar_por_cobertura`, `_fechar_chamado`: `ponto/management/commands/detectar_ausencias.py:159`, `ponto/management/commands/detectar_ausencias.py:826`, `ponto/management/commands/detectar_ausencias.py:837`, `ponto/management/commands/detectar_ausencias.py:849`.
- `sincronizar_adesao`, **2 sitios**: `chamados/services/adesao.py:116`, `chamados/services/adesao.py:151`.
- `aceitar_sugestao` e duas tentativas de `agrupar_furo`, **3 sitios**: `chamados/services/limbo_folgas.py:102`, `chamados/services/limbo_folgas.py:284`, `chamados/services/limbo_folgas.py:287`.
- `_materializar_ausencia_de_resposta` e `_fecha`, **2 sitios**: `chamados/services/materializacao.py:588`, `chamados/services/materializacao.py:777`.
- `designar_cobertura` e `declarar_cobertura_passada`, **2 sitios**: `ponto/services/cobertura.py:123`, `ponto/services/cobertura.py:319`.
- `fechar_pelo_marco_colab`: `ponto/services/lastro.py:401`; `resolver_cobranca_da_entrada`: `ponto/services/entrada_sincronizada.py:32`.
- `reconciliar_colab`: `ponto/services/cluster_espurio.py:190`; `fechar_chamado_da_ausencia`: `ponto/services/ausencia.py:758`.
- `fechar_o_fio_do_dia`: `ponto/services/veredito_celula.py:203`; `_autoresolve_f2_f4`: `ponto/services/triagem_batida.py:598`.
- Comandos `_ata_sem_furo` e `handle`: `ponto/management/commands/curar_residuo_bug115.py:155`, `ponto/management/commands/detectar_intervalo_ausente.py:139`.
- `reconciliar_por_ausencia`: `chamados/services/silencio_ausencia.py:102`; `feriado_reavaliar.encerrar`: `chamados/services/feriado_reavaliar.py:48`; comando `handle`: `chamados/management/commands/auditar_chamados_orfaos.py:68`.

**E — 4 sitios, todos pedindo `fechado`:**

- `encerrar_com_retencao`: `chamados/reconciliador.py:497`.
- `_executar_aceite`: `colaboradores/services/vinculo.py:404`.
- `executar_desligamento`: `colaboradores/services/desligamento.py:187`.
- `materializar_regularizacao_externa`: `chamados/services/regularizacao_ext.py:206`.

**T — 1 sitio:** `competencia_trancada.encerrar`, pedindo `fechado`; acrescenta nota e evento especificos apos sucesso. (`chamados/services/competencia_trancada.py:209`, `chamados/services/competencia_trancada.py:210`, `chamados/services/competencia_trancada.py:215`)

**C — 2 sitios:** comandos `fechar_chamados_vinculo_na_lista` e `fechar_avisos_regua_na_lista`, ambos pedindo `cancelado` e passando motivo. (`ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `escala/management/commands/fechar_avisos_regua_na_lista.py:48`)

**H — 8 sitios externos sem `ato`:**

- `acao_aprovar_excecao`, `acao_responder_agenda`, `acao_responder_escala`, `acao_autorizar_geofence`, `acao_recusar_geofence`: pedem `resolvido`. (`chamados/services/acoes_chamado.py:71`, `chamados/services/acoes_chamado.py:110`, `chamados/services/acoes_chamado.py:155`, `chamados/services/acoes_chamado.py:229`, `chamados/services/acoes_chamado.py:286`)
- `acao_resposta_livre` e `processar_post_fio`: destino parametrizado, conferido contra `STATUS_CHOICES`, depois julgado pela porta. (`chamados/services/acoes_chamado.py:322`, `chamados/services/acoes_chamado.py:325`, `chamados/services/fio.py:719`, `chamados/services/fio.py:722`)
- `avaliar_solicitacao_beneficio`: `resolvido` ao aprovar, `fechado` no outro ramo. (`colaboradores/views.py:2072`)

**Atalho `fechar` — 1 sitio externo:** comando `fechar_chamados_meio_periodo`; chama `ch.fechar`, que encaminha internamente para H e acrescenta nota quando recebe resolucao. (`ponto/management/commands/fechar_chamados_meio_periodo.py:110`, `chamados/models.py:455`, `chamados/models.py:458`)

**Envelopes excluidos do encerramento:** `_chamado_para_decisao` pede somente `em_analise`; no smoke, uma chamada percorre a matriz, outra reabre e outra testa recusas. O smoke recusa banco sem `test` no nome e reverte sua transacao. (`chamados/services/acoes_chamado.py:241`, `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:135`, `chamados/management/commands/smoke_chamados.py:148`, `chamados/management/commands/smoke_chamados.py:164`)

### Nascentes anteriores: commands, crons e eventos

**Commands que chegam indiretamente a R:**

- Por `retratar → _retratar → reconciliar`: `retratar_chamados_lei_cala`, `reconciliar_grade`, `reconciliar_vinculo`, `reconciliar_fantasmas` e `_resolver_chamado_do_turno` de `processar_alertas_turno`. (`chamados/management/commands/retratar_chamados_lei_cala.py:59`, `ponto/management/commands/reconciliar_grade.py:309`, `ponto/management/commands/reconciliar_vinculo.py:68`, `ponto/management/commands/reconciliar_fantasmas.py:280`, `ponto/management/commands/processar_alertas_turno.py:216`, `chamados/reconciliador.py:547`, `chamados/reconciliador.py:524`)
- Por servicos especificos: `fechar_chamados_ausencia`, `reavaliar_ausencias_lancadas`, `detectar_cluster_espurio`, `apurar_furos_diarios` e `processar_cartorio`. (`ponto/management/commands/fechar_chamados_ausencia.py:44`, `ponto/management/commands/reavaliar_ausencias_lancadas.py:54`, `ponto/management/commands/detectar_cluster_espurio.py:81`, `escala/management/commands/apurar_furos_diarios.py:17`, `ponto/management/commands/processar_cartorio.py:65`)
- `consumir_eventos_celula` chama o consumidor, que executa retratacoes e fechamento por lastro. (`chamados/management/commands/consumir_eventos_celula.py:31`, `chamados/reconciliador.py:1369`, `chamados/reconciliador.py:1373`, `chamados/reconciliador.py:1384`)
- `celula_veredito_velho` pode rejulgar pelo cartorio e conservar o resultado com `--aplicar`; sem aplicar, provoca rollback. (`ponto/management/commands/celula_veredito_velho.py:144`, `ponto/management/commands/celula_veredito_velho.py:156`, `ponto/management/commands/celula_veredito_velho.py:162`)

**T tambem nasce no ato de trancar:** `trancar_periodo → disparar_no_ato`; o servico executa diretamente ou dispara o comando com `--apply`. O comando tambem permite execucao sob demanda. (`ponto/services/fechamento.py:1428`, `chamados/services/competencia_trancada.py:307`, `chamados/services/competencia_trancada.py:315`, `chamados/services/competencia_trancada.py:322`, `chamados/management/commands/encerrar_por_competencia_trancada.py:29`)

**Agendamentos que possuem caminho de escrita lido:**

- `processar_alertas_turno`, a cada 5 minutos: turno fechado chama `_resolver_chamado_do_turno`, depois `retratar`. (`config/crons.py:362`, `ponto/management/commands/processar_alertas_turno.py:88`, `ponto/management/commands/processar_alertas_turno.py:216`)
- `detectar_ausencias`, a cada 5 minutos: quatro sitios R e chamada de agrupamento. (`config/crons.py:364`, `ponto/management/commands/detectar_ausencias.py:159`, `ponto/management/commands/detectar_ausencias.py:762`, `ponto/management/commands/detectar_ausencias.py:826`, `ponto/management/commands/detectar_ausencias.py:837`, `ponto/management/commands/detectar_ausencias.py:849`)
- `consumir_eventos_celula`, a cada minuto: caminhos de retratacao e lastro acima. (`config/crons.py:394`, `chamados/reconciliador.py:1369`, `chamados/reconciliador.py:1384`)
- `processar_cartorio --apply`: seis sitios R no cartorio e chamada ao reconciliador de cluster. (`config/crons.py:616`, `ponto/management/commands/processar_cartorio.py:65`, `ponto/services/cartorio.py:1042`)
- `apurar_furos_diarios`, encadeado depois de `processar_cartorio`: chama adesao com `fechar=True`. (`config/crons.py:1527`, `escala/management/commands/apurar_furos_diarios.py:17`)

**Agendamentos de vigilancia:** `reconciliar_fantasmas`, `reavaliar_ausencias_lancadas`, `fechar_chamados_ausencia`, `reconciliar_vinculo`, `detectar_intervalo_ausente` e `detectar_cluster_espurio` estao configurados com `--alarme`; esse modo impede seus ramos de fechamento. (`config/crons.py:392`, `config/crons.py:396`, `config/crons.py:397`, `config/crons.py:489`, `config/crons.py:491`, `config/crons.py:598`, `ponto/management/commands/reconciliar_fantasmas.py:153`, `ponto/management/commands/reavaliar_ausencias_lancadas.py:26`, `ponto/management/commands/fechar_chamados_ausencia.py:37`, `ponto/management/commands/reconciliar_vinculo.py:29`, `ponto/management/commands/detectar_intervalo_ausente.py:108`, `ponto/management/commands/detectar_cluster_espurio.py:69`)

`reconciliar_grade` esta agendado sem `--apply`; a retratacao exige `apply`. `reconciliar_chamados` e `fechar_cobranca_com_lastro` estao como alarmes/leitores. (`config/crons.py:462`, `ponto/management/commands/reconciliar_grade.py:305`, `config/crons.py:395`, `chamados/management/commands/reconciliar_chamados.py:118`, `config/crons.py:731`, `ponto/management/commands/fechar_cobranca_com_lastro.py:77`)

**Entradas por evento ou tela, citando apenas os sitios das outras entidades:**

- Batida e ausencia: `ponto/signals.py:215`, `ponto/signals.py:234`, `ponto/signals.py:269`, `ponto/signals.py:271`.
- API de entrada: `api/views.py:791`, `api/views_core.py:785`.
- Declaracao/validacao do dia: `ponto/services/declarar_dia.py:206`, `chamados/services/validacao.py:319`.
- Justificativa, para E com retencao: `ponto/services/justificativa.py:87`, `ponto/services/justificativa.py:400`.
- Celula/feriado: `ponto/portas/celula.py:538`, `chamados/signals.py:335`.
- Disputa fechada, para R: `chamados/signals.py:19`, `chamados/signals.py:43`.

### Provas complementares das guardas

- **H recusa pergunta viva:** `chamados/tests/test_o261e_f1_porta_encerramento.py::test_e_RED_transicionar_com_viva_por_destino_levanta` (`chamados/tests/test_o261e_f1_porta_encerramento.py:193`).
- **R recusa, registra e deduplica a recusa:** `chamados/tests/test_o261e_f1_porta_encerramento.py::test_a_RED_reconciliar_com_viva_por_destino_recusa_com_trilha_idempotente` (`chamados/tests/test_o261e_f1_porta_encerramento.py:82`).
- **E recusa pergunta viva:** `chamados/tests/test_o261e_f1_porta_encerramento.py::test_f_RED_encerrar_com_viva_devolve_False_sem_raise` (`chamados/tests/test_o261e_f1_porta_encerramento.py:107`).
- **T, dry/apply e segunda passada:** `chamados/tests/test_competencia_trancada.py::test_MORDE_passivo_dry_depois_apply_e_a_segunda_passada_e_zero` (`chamados/tests/test_competencia_trancada.py:106`).
- **Nucleo impede contorno da guarda:** `chamados/tests/test_o261e_f1_porta_encerramento.py::test_i_RED_tripwire_no_nucleo` (`chamados/tests/test_o261e_f1_porta_encerramento.py:200`).
- **Selo testa mutantes de escrita:** atribuicao, update/create, defaults, save, bulk/setattr/SQL e funcao externa. (`chamados/tests/test_selo_status_so_pelas_portas.py:405`, `chamados/tests/test_selo_status_so_pelas_portas.py:411`, `chamados/tests/test_selo_status_so_pelas_portas.py:421`, `chamados/tests/test_selo_status_so_pelas_portas.py:440`, `chamados/tests/test_selo_status_so_pelas_portas.py:449`, `chamados/tests/test_selo_status_so_pelas_portas.py:466`)

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 12 pares: 11 runtime e 1 historico

- **#1, #2, #3 — 3 pares para `resolvido` desde vivo:** H combina motor, disputa, ancoradas e premissa; R combina motor, premissa, conversa e pergunta viva. (`chamados/models.py:132`, `chamados/models.py:154`, `chamados/models.py:166`, `chamados/models.py:183`, `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689`)
- **#4 — 1 par `fechado → resolvido`:** excecao de perfil no motor, disputa viva e premissa de furo. (`chamados/catalogo/motor.py:150`, `chamados/catalogo/motor.py:153`, `chamados/models.py:154`, `chamados/models.py:181`)
- **#5, #6, #7 — 3 pares para `fechado` desde vivo:** H combina motor, justificativa, disputa e ancoradas; E combina motor/pergunta; T combina selecao por tranca no servico, origem viva, motor/pergunta na porta. (`chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`, `chamados/models.py:1661`, `chamados/models.py:1664`, `chamados/services/competencia_trancada.py:95`, `chamados/services/competencia_trancada.py:192`, `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684`)
- **#8 — 1 par `resolvido → fechado`:** H combina motor, justificativa e disputa; na nascente `encerrar_com_retencao`, ha ainda `retencao_da_retratacao` antes de E. (`chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/reconciliador.py:493`, `chamados/reconciliador.py:497`)
- **#9, #10, #11 — 3 pares para `cancelado`:** motor e `_gate_pergunta_viva`, alem da guarda do escritor quando cruza vivo → terminal. (`chamados/models.py:830`, `chamados/models.py:833`, `chamados/models.py:221`, `chamados/models.py:224`)
- **#12 — 1 par historico `cancelado → superado`:** filtros de estado/modulo, presenca da data e unicidade do irmao elegivel. (`chamados/migrations/0039_a3_via_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:16`, `chamados/migrations/0039_a3_via_backfill.py:20`, `chamados/migrations/0039_a3_via_backfill.py:23`)

Na nascente `_retratar`, a retencao acrescenta outro julgamento antes de R: irmas respondidas sem consumo ou justificativa alheia pendente. (`chamados/reconciliador.py:447`, `chamados/reconciliador.py:452`, `chamados/reconciliador.py:519`)

A composicao de pergunta viva tambem consulta competencia entregue, `pergunta_viva`, cobranca sobrevivente e `resolvedora_valida`; quando aprovada, pode reapontar perguntas antes de encerrar. (`chamados/models.py:364`, `chamados/models.py:368`, `chamados/models.py:390`, `chamados/models.py:392`, `chamados/models.py:437`)

### L2 ESCRITOR FORA DA PORTA — 4 sitios persistentes de estado em migracoes; 0 encontrados no runtime

1. `aberto → registrado`, fora do recorte de encerramento: `queryset.update` em `chamados/migrations/0038_a3_registrado_backfill.py:10`.
2. Reversa `registrado → aberto`, fora do recorte de encerramento: `queryset.update` em `chamados/migrations/0038_a3_registrado_backfill.py:15`.
3. **`cancelado → superado`**, sem `_gravar_estado`: `queryset.update` em `chamados/migrations/0039_a3_via_backfill.py:24`.
4. **`superado → cancelado`**, sem `_gravar_estado`: `queryset.update` em `chamados/migrations/0039_a3_via_backfill.py:33`.

Ha ainda **4 sitios exclusivos de escrita de via**, sem mudanca de estado: preencher `cancelado`, preencher `admin`, preencher `legado` e limpar todas as vias na reversa. (`chamados/migrations/0039_a3_via_backfill.py:25`, `chamados/migrations/0039_a3_via_backfill.py:27`, `chamados/migrations/0039_a3_via_backfill.py:28`, `chamados/migrations/0039_a3_via_backfill.py:34`)

No runtime, as escritas declaradas sao o nucleo da transicao e as duas portas de nascimento. `_vivas_entre` faz duas atribuicoes temporarias, restauradas em `finally`; o `queryset.update` proximo escreve somente urgencia. (`chamados/models.py:296`, `chamados/models.py:1018`, `chamados/models.py:1142`, `chamados/models.py:356`, `chamados/models.py:371`, `chamados/models.py:637`)

O **zero encontrado no runtime** tem limite: o selo exclui migracoes e declara que receptores ou parametros genericos nao resolvidos pelo AST podem escapar. (`chamados/tests/test_selo_status_so_pelas_portas.py:15`, `chamados/tests/test_selo_status_so_pelas_portas.py:17`, `chamados/tests/test_selo_status_so_pelas_portas.py:34`)

### L3 ESTADO ALCANCAVEL SEM SAIDA — 2 no runtime; 0 incluindo migracoes e reversas

1. **`cancelado`: terminal absoluto declarado no runtime.** Chega por `cancelar`, pelas duas nascentes C enumeradas; o motor recusa saida humana inclusive para `admin_ti` e seus mapas de sistema nao oferecem saida. Entretanto, a migration permite `cancelado → superado`. (`chamados/models.py:835`, `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `chamados/catalogo/motor.py:104`, `chamados/catalogo/motor.py:111`, `chamados/catalogo/motor.py:142`, `chamados/migrations/0039_a3_via_backfill.py:24`)
2. **`superado`: terminal absoluto declarado, mantido como acervo.** Chega pela migration 0039; a porta runtime `superar` foi retirada. A reversa da migration oferece `superado → cancelado`. (`chamados/catalogo/motor.py:106`, `chamados/models.py:841`, `chamados/models.py:845`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

`resolvido` e `fechado` possuem saida por `reabrir`; `fechado` tambem permite transicao humana por `admin_ti`. Portanto nao pertencem aos becos sem saida runtime. (`chamados/catalogo/motor.py:113`, `chamados/catalogo/motor.py:150`, `chamados/catalogo/motor.py:153`)

## 4. O QUE NAO FOI LIDO OU FICOU INCERTO

- **Execucao e frequencia em producao, migracoes efetivamente aplicadas, crontab instalado e resultado atual dos testes:** **[nao verificado]**. As contagens apresentadas sao de codigo e sitios, sem acesso a banco/log.
- **Correspondencia criptografica da copia com o hash informado:** **[nao verificado]**; nao havia repositorio Git reconhecido no diretorio consultado.
- **Nascimento diretamente em terminal:** `criar` preserva status diferente de `aberto`, e `abrir` permite defaults sobreporem o estado calculado. O exercicio concreto localizado foi no smoke, restrito e revertido; ocorrencia persistente em producao por essas entradas: **[nao verificado]**. (`chamados/models.py:987`, `chamados/models.py:1110`, `chamados/models.py:1142`, `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:147`)
- **As tres arestas `SUPERAR` do motor nao demonstram nascentes runtime:** permanecem na tabela do motor, mas a porta foi retirada; por isso nao foram convertidas em transicoes de producao na matriz. (`chamados/catalogo/motor.py:117`, `chamados/models.py:841`)
- **Autor recebido pelo wrapper com `ato`:** o wrapper executa `ato()` sem repassar `user`; o autor eventual da trilha pode vir do contexto de request/processo. Autor efetivamente registrado em cada ocorrencia: **[nao verificado]**. (`chamados/services/fecho_runtime.py:157`, `chamados/services/fecho_runtime.py:165`, `core/observ.py:110`, `core/observ.py:117`)
- **Trilha infalivel e estado final apos todos os receivers:** **[nao verificado]**. `evento` tolera falha, e o `save` pode disparar reentrada que reabre o chamado; uma transicao para terminal nao prova permanencia nele. (`core/observ.py:98`, `core/observ.py:139`, `chamados/models.py:274`, `chamados/models.py:296`)
- **Escrita por metaprogramacao arbitraria ou receptor cujo tipo nao se resolve:** **[nao verificado]**, conforme limite expresso do selo. (`chamados/tests/test_selo_status_so_pelas_portas.py:17`)