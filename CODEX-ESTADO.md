# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 09:53** · publicado em **30/10/2026 09:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 09:53 · sessao 3c1b129f7b2c · O261-FIO-MORTO · sonda · origem=Code

Sim: `2026-10-25` aparece com código `C9`, dono `BATIDA`, dono_do_dia `BATIDA` e ação `Regularizar a saida do dia`, no dado fornecido. [nao verificado]

```text
CXR129 codigo=C2 data=None dono=cadastro (lido agora) dono_do_dia=None acao=Revisar a jornada e os marcos do template [nao verificado]
CXR129 codigo=C9 data=2026-08-23 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-06 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-07 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-20 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-10-11 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-10-25 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=A5 data=None dono=cadastro dono_do_dia=None acao=Revisar os horarios do template [nao verificado]
CXR129 codigo=A7 data=None dono=cadastro dono_do_dia=None acao=Revisar os horarios do template [nao verificado]
CXR129_total=9 [nao verificado]
```

## 30/10 09:16 · sessao 9c7726ff84d8 · O261-VETO-NA-PORTA · censo · origem=Code

**Dos 30 caminhos das três tabelas de nascimento/reabertura do anexo, 29 passam por pelo menos uma das três portas; 1 escreve por fora.** As escritas convergem nos quatro pontos abaixo. Contagem por caminho, detalhada na tabela; não é contagem de ocorrências em produção. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

| Sigla usada nas tabelas | Linha da escrita final |
|---|---|
| **A** — `ChamadoColaborador.abrir` | `get_or_create`: `app/chamados/models.py:730` |
| **C** — `ChamadoColaborador.criar` | `objects.create`: `app/chamados/models.py:834` |
| **G** — `ChamadoColaborador._gravar_estado` | `save(update_fields=campos)`: `app/chamados/models.py:231` |
| **X** — por fora | `QuerySet.update(status_local='aberto')`: `app/chamados/migrations/0038_a3_registrado_backfill.py:15` |

Na coluna **A / C / G**, a ordem é `abrir / criar / _gravar_estado`. **Sim*** indica passagem pelo ramo de reabertura: `abrir → _renascer_por_premissa_viva → reabrir → _gravar_estado`. (`app/chamados/models.py:686`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`)

A marca **“copia de veto”** identifica o sítio que mantém bloqueio próprio de régua/célula antes de pedir a escrita. As delegações que atravessam esse sítio estão identificadas como herança. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:626`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:854`)

| Nº | Caminho — arquivo::função | Chamada que conduz à escrita | Escrita final | A / C / G | Marca |
|---:|---|---|---|---|---|
| 1 | `app/chamados/models.py::ChamadoColaborador.abrir` — nascimento | `app/chamados/models.py:730` | **A** | sim / não / não | Veto dentro da própria porta: `app/chamados/models.py:726`. |
| 2 | `app/chamados/models.py::ChamadoColaborador.criar` | `app/chamados/models.py:834` | **C** | não / sim / não | Veto dentro da própria porta: `app/chamados/models.py:822`. |
| 3 | `app/chamados/models.py::ChamadoColaborador.abrir` — dedup encerrado | Renascimento em `app/chamados/models.py:686`; `reabrir` em `app/chamados/models.py:770` | **G** | sim / não / sim | Retorna antes do veto de nascimento: `app/chamados/models.py:688`, `app/chamados/models.py:726`. |
| 4 | `app/chamados/models.py::ChamadoColaborador.reabrir` | `_gravar_estado` em `app/chamados/models.py:1089` | **G** | não / não / sim | Consulta da própria porta: `app/chamados/models.py:1050`. |
| 5 | `app/chamados/models.py::ChamadoColaborador.transicionar` — destino vivo | `_aplicar` em `app/chamados/models.py:183` → `_gravar_estado` em `app/chamados/models.py:260` | **G** | não / não / sim | Guardas de premissa do corpo são para resolução: `app/chamados/models.py:165`. |
| 6 | `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `app/ponto/management/commands/detectar_ausencias.py:439` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **A/G** | sim / não / sim* | **copia de veto**: régua em `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`; célula em `app/ponto/management/commands/detectar_ausencias.py:626`. |
| 7 | `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Detector em `app/ponto/management/commands/emitir_furo_retroativo.py:142` → caminho 6 | **A/G** | sim / não / sim* | Herda a cópia do detector: `app/ponto/management/commands/emitir_furo_retroativo.py:142`. |
| 8 | `app/ponto/management/commands/reconciliar_grade.py::Command.handle → _emitir_furo` | `abrir` em `app/ponto/management/commands/reconciliar_grade.py:499`; `reabrir` adicional em `app/ponto/management/commands/reconciliar_grade.py:520` | **A/G** | sim / não / sim* | **copia de veto**: régua em `app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`; célula em `app/ponto/management/commands/reconciliar_grade.py:494`. |
| 9 | `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral/morta/limbo | Detector em `app/ponto/services/cartorio.py:1358` → caminho 6 | **A/G** | sim / não / sim* | Herda bloqueio do detector. Consulta própria para impressão: `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`. |
| 10 | `app/ponto/services/cartorio.py::_julgar_colab_corpo` — parcial | Detector em `app/ponto/services/cartorio.py:1394` → caminho 6 | **A/G** | sim / não / sim* | Herda bloqueio do detector. Mesma consulta para impressão: `app/ponto/services/cartorio.py:1083`. |
| 11 | `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir` em `app/chamados/reconciliador.py:862` | **A/G** | sim / não / sim* | **copia de veto**: célula em `app/chamados/reconciliador.py:838`; régua em `app/chamados/reconciliador.py:854`, condicionada por `app/chamados/reconciliador.py:852`. |
| 12 | `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `app/ponto/management/commands/processar_alertas_turno.py:141` → `app/ponto/management/commands/processar_alertas_turno.py:316` → `abrir` em `app/ponto/management/commands/processar_alertas_turno.py:335` | **A/G** | sim / não / sim* | Consulta compartilhada pela porta: `app/chamados/models.py:726`. |
| 13 | `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar` em `app/ponto/management/commands/processar_alertas_avancados.py:121` | **C** | não / sim / não | Consulta compartilhada pela porta: `app/chamados/models.py:822`. |
| 14 | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir` em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146` | **A/G** | sim / não / sim* | Consulta antecipada somente no dry: `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:140`. |
| 15 | `app/ponto/views.py::bater_ponto` | `_Chamado.criar` em `app/ponto/views.py:291` | **C** | não / sim / não | Condição própria de entrada bloqueada e dedup: `app/ponto/views.py:275`, `app/ponto/views.py:281`. |
| 16 | `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | `app/chamados/views.py:197` → `criar` em `app/chamados/services/abertura.py:47` | **C** | não / sim / não | Consulta compartilhada pela porta: `app/chamados/models.py:822`. |
| 17 | `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | `app/chamados/views.py:1053` → renascimento em `app/chamados/services/resposta_tardia.py:45` | **G** | não / não / sim | Seleção própria por `VEREDITOS_ACUSAM`: `app/chamados/services/resposta_tardia.py:45`. |
| 18 | `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `app/chamados/views.py:547` → `transicionar` em `app/chamados/services/acoes_chamado.py:315` | **G** | não / não / sim | Passa pelo escritor da transição: `app/chamados/models.py:260`. |
| 19 | `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | `app/chamados/views.py:674`, `app/chamados/views.py:954` → `transicionar` em `app/chamados/services/fio.py:715` | **G** | não / não / sim | Passa pelo escritor da transição: `app/chamados/models.py:260`. |
| 20 | `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 21 | `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | `app/ponto/services/fechamento.py:1416` → `ch.reabrir` em `app/chamados/services/competencia_trancada.py:258` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 22 | `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `app/chamados/services/fio.py:577` → `ch.reabrir` em `app/chamados/services/acoes_disputa.py:266` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 23 | `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir` em `app/chamados/services/disputa_emissao.py:1606` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 24 | `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | **G** | não / não / sim | Pergunta antecipadamente à mesma `recusa_da_reabertura`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/models.py:1050`. |
| 25 | `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | `update` direto em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | **X** | não / não / não | **escritor a migrar**. |
| 26 | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `abrir` em `app/chamados/services/disputa_emissao.py:504` | **A/G** | sim / não / sim* | Chamado-pai `disputa_supervisao`: `app/chamados/services/disputa_emissao.py:507`. |
| 27 | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir` em `app/chamados/services/disputa_emissao.py:908` | **A/G** | sim / não / sim* | Chamado-pai de módulo manual: `app/chamados/services/disputa_emissao.py:911`. |
| 28 | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir` em `app/chamados/services/disputa_emissao.py:1128` | **A/G** | sim / não / sim* | Recusa própria de âncora encerrada: `app/chamados/services/disputa_emissao.py:1109`. |
| 29 | `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | `app/chamados/views.py:136` → `criar` em `app/chamados/services/abertura.py:172` | **C** | não / sim / não | Validação própria de tipo/dia: `app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:152`. |
| 30 | `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa → app/chamados/reconciliador.py::reatar_pergunta_orfa → reemitir_chamado_do_dia` | `app/chamados/services/disputa_emissao.py:409` → `app/chamados/reconciliador.py:1518` → `abrir` em `app/chamados/reconciliador.py:862` | **A/G** | sim / não / sim* | Herda a cópia do c2: `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:854`. |

As entradas convergentes do anexo ficam relacionadas abaixo aos caminhos numerados. Para a escrita delegada indicada, passam por **A: sim; C: não; G: sim***, com as linhas finais **A/G** definidas acima. (`app/chamados/models.py:730`, `app/chamados/models.py:686`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`, `app/chamados/models.py:231`)

| Entrada — arquivo::função | Referência e convergência até os caminhos acima | Escrita final |
|---|---|---|
| `app/ponto/management/commands/processar_cartorio.py::Command.handle` | `app/ponto/management/commands/processar_cartorio.py:65` → corpo via `app/ponto/services/cartorio.py:744` → **9/10** | **A/G** |
| `app/ponto/services/cartorio.py::julgar_celula` | `julgar_colab` em `app/ponto/services/cartorio.py:1560`; emissor em `app/ponto/services/cartorio.py:1561` → **9/10** | **A/G** |
| `app/ponto/services/vigia_de_hora.py::rodar` | `julgar_celula` em `app/ponto/services/vigia_de_hora.py:130` → **9/10** | **A/G** |
| `app/chamados/views_cobrar.py::cobrar_dia_view → app/chamados/services/cobrar_dia.py::cobrar_dia` | `app/chamados/views_cobrar.py:325` → `julgar_celula` em `app/chamados/services/cobrar_dia.py:29` → **9/10** | **A/G** |
| `app/ponto/management/commands/cobrar_furos_sem_cobranca.py::Command.handle → app/ponto/services/furos_sem_cobranca.py::cobrar` | `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30` → `julgar_celula` em `app/ponto/services/furos_sem_cobranca.py:128` → **9/10** | **A/G** |
| `app/ponto/signals.py::_julgar_dias` | `julgar_celula` em `app/ponto/signals.py:93` → **9/10** | **A/G** |
| `app/chamados/signals.py::_cartorio_post_save_chamado` | `julgar_celula` em `app/chamados/signals.py:180` → **9/10**; receiver posterior ao save original: `app/chamados/signals.py:164` | **A/G** |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `julgar_celula` em `app/ponto/portas/celula.py:372` → **9/10** | **A/G** |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `julgar_celula` em `app/colaboradores/services/pos_vinculo.py:72` → **9/10** | **A/G** |
| `app/escala/regua_defesa.py::rejulgar_furos_do_vinculo` | `julgar_colab` em `app/escala/regua_defesa.py:692` → **9/10** | **A/G** |
| Commands de rejulgamento | `app/ponto/management/commands/celula_veredito_velho.py::Command.handle`, chamada em `app/ponto/management/commands/celula_veredito_velho.py:144`; `app/ponto/management/commands/curar_residuo_bug115.py::Command._lavrar`, chamada em `app/ponto/management/commands/curar_residuo_bug115.py:143`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py::Command.handle`, chamada em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71` → **9/10** | **A/G** |
| Scripts fora de `tests/` | `bin/rejulgar_o118_col221.py::<módulo>`, chamada em `bin/rejulgar_o118_col221.py:24`; `bin/r5_idempotencia_frota.py::rodada`, chamada em `bin/r5_idempotencia_frota.py:109` → **9/10** | **A/G** |
| `app/chamados/reconciliador.py::_ctx_c2` | `julgar_celula` em `app/chamados/reconciliador.py:671` → **9/10** | **A/G** |
| Reconciliação por command/signal | `app/chamados/management/commands/reconciliar_chamados.py::Command.handle`, chamada em `app/chamados/management/commands/reconciliar_chamados.py:52`; `app/ponto/signals.py::_reconciliador_post_save_batida`, chamada em `app/ponto/signals.py:215` → cadeia em `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1423`, `app/chamados/reconciliador.py:1518` → **11** | **A/G** |
| `app/ponto/services/cartorio.py::julgar_colab → app/chamados/services/fabrica_da_celula.py::perguntar_pela_celula → perguntar` | `app/ponto/services/cartorio.py:770` → `app/chamados/services/fabrica_da_celula.py:149` → chamadas em `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/services/fabrica_da_celula.py:124` → **26/30** | **A/G** |
| `app/chamados/management/commands/disparar_perguntas_competencia.py::_rodar` | Fábrica em `app/chamados/management/commands/disparar_perguntas_competencia.py:288` → **26/30**, pelas chamadas em `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/services/fabrica_da_celula.py:124` | **A/G** |
| `app/api/views.py::api_bater_ponto` e `app/api/views_core.py::api_bater_ponto` | `app/api/views.py:778`, `app/api/views_core.py:778` → núcleo/perguntas em `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:618` → **26/30** | **A/G** |
| `app/ponto/services/triagem_batida.py::ramo_registrar_es` | `app/ponto/services/triagem_batida.py:651` → mesma cadeia de disputa/perguntas → **26/30**, via `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606` | **A/G** |
| `app/chamados/services/declaracao_texto.py::declarar_hora_colab/aplicar_declaracao` | `app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/declaracao_texto.py:605` → núcleo em `app/chamados/services/disputa_emissao.py:563` → **26** | **A/G** |
| Outros chamadores do agrupador de órfãos | `app/chamados/services/acoes_disputa.py::acao_iniciar_disputa_orfaos`, chamada em `app/chamados/services/acoes_disputa.py:221`; `app/chamados/services/pergunta_no_app.py::perguntar`, chamada em `app/chamados/services/pergunta_no_app.py:159`; `app/chamados/services/cobrar_dia.py::cobrar_dia`, chamada em `app/chamados/services/cobrar_dia.py:44`; `app/ponto/management/commands/processar_alertas_turno.py::_abrir_chamado_critico`, chamada em `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle`, chamada em `app/ponto/management/commands/processar_alertas_avancados.py:158`; `app/ponto/management/commands/detectar_ausencias.py::_abrir_chamado`, chamada em `app/ponto/management/commands/detectar_ausencias.py:791` → **27** | **A/G** |
| Outros chamadores do núcleo | `app/ponto/management/commands/reconciliar_grade.py::_disputa`, chamada em `app/ponto/management/commands/reconciliar_grade.py:416`; `app/chamados/management/commands/smoke_chamados.py::_bloco_a_motivos`, chamada em `app/chamados/management/commands/smoke_chamados.py:99` → **26** | **A/G** |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py::Command.handle` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` → núcleo em `app/chamados/services/disputa_emissao.py:598` → **26** | **A/G** |

Para as marcas, há estas distinções verificadas:

| Consulta anterior | Classificação |
|---|---|
| Cartório: `motivo_do_veto` em `app/ponto/services/cartorio.py:1013` | Alimenta a impressão em `app/ponto/services/cartorio.py:1083`; sua função de detectar queda do veto está documentada em `app/ponto/services/furos_vetados.py:22`. Não marcada como cópia de bloqueio. |
| Passivo: régua/célula em `app/ponto/services/furos_sem_cobranca.py:96`, `app/ponto/services/furos_sem_cobranca.py:98` | Classifica a medição; a cobrança filtra essa classificação em `app/ponto/services/furos_sem_cobranca.py:125`. Que essa leitura se torne apagável pela mudança da porta: **[nao verificado]**. |
| Retroativo de turno: `celula_absolveria` em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134` | Consulta dry que termina sem emissão em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:140`; reutiliza o juiz da porta em `app/chamados/models.py:858`. |
| Órfãs enterradas: `recusa_da_reabertura` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:105` | Reutiliza a mesma função da porta, chamada em `app/chamados/models.py:1050`; não copia suas guardas, conforme `app/chamados/models.py:993`. |
| Resposta tardia: teste de `VEREDITOS_ACUSAM` em `app/chamados/services/resposta_tardia.py:45` | Seleção própria de premissa. Equivalência com o veto de régua/célula a centralizar: **[nao verificado]**. |

| Total — unidade explícita | Quantidade e referência |
|---|---|
| Caminhos das três tabelas principais do anexo | **30**, enumerados acima; entradas convergentes remetem a essas linhas. |
| Passam por pelo menos uma das três portas | **29** — linhas 1–24 e 26–30; escritas em `app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`. |
| Escrevem por fora | **1** — linha 25, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`. |
| **copia de veto**, contando sítios próprios | **3** — detector, grade e c2: `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/chamados/reconciliador.py:854`. |
| Caminhos principais que atravessam essas cópias, incluindo delegações | **7** — linhas 6, 7, 8, 9, 10, 11 e 30; delegações em `app/ponto/management/commands/emitir_furo_retroativo.py:142`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1394`, `app/chamados/services/disputa_emissao.py:409`. |
| **escritor a migrar** | **1** — reverso da migration 0038, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`. |
| Pontos físicos de escrita neste recorte | **4** — três internos às portas e o `update` externo: `app/cha

[truncado em 20000 de 20420 caracteres]

## 30/10 09:15 · sessao 56fb2aebdc3a · O261-NOTURNO · verificador · origem=Code

- **[P2] Falso negativo introduzido** — [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166](/home/ronald/codex_ws/06d7ad061f6eb40555d3dafb6785a8d6067ffb7a/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166): o novo filtro ignora templates válidos como `'no dia {data!s}'.format(data=x)` e `'no dia {data:%d/%m/%Y}'.format(data=x)`, pois `_tem_data` exige a substring exata `{data}`. Reproduzido em memória: antes retornavam `[1]`; agora retornam `[]`, permitindo formatação de data fora do produtor.

## 30/10 09:15 · sessao 44dd85543b31 · O261-VETO-NA-PORTA · leitura · origem=Code

**São relógios diferentes:** as **24 h** são idade da entrada do turno aberto; as **72 h** mudam a urgência. Já o emissor `turno_aberto` nasce em relação ao **fim previsto**, e as **4 h** pertencem à expiração do “turno vivo”. ([turnos.py:3002](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3002), [turnos.py:3013](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3013), [turnos.py:3020](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3020), [processar_alertas_turno.py:140](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:140), [turnos.py:2011](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2011))

1. **`processar_alertas_turno.py` → `turno_aberto`**

   O nascimento passa por `_marco_critico` quando o lembrete é **`pos_20`**, chegando a `ChamadoColaborador.abrir(modulo_origem='turno_aberto')`. Portanto depende do **fim previsto + 20 minutos**, não de entrada aberta há 24 h. ([processar_alertas_turno.py:138](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:138), [processar_alertas_turno.py:316](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:316), [processar_alertas_turno.py:335](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:335))

   Os números vêm de `LEMBRETES_PRE_MIN = {-15,-5}`, `LEMBRETES_POS_MIN = {5,10,20}` e tolerância **240 segundos**. A decisão usa distância absoluta até cada marco: `pos_20` vale entre **16 e 24 minutos depois do fim**, inclusive; depois dessa janela, esse marco não dispara. ([turnos.py:3030](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3030), [turnos.py:3038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3038))

   O fim vem primeiro de **`tm.dt_fim_previsto`**. Na materialização, esse campo é calculado com o `TipoEscala` vigente na data e `janela_turno_de(te, data_turno)`, **sem `ec`**; esse caminho lê os marcos do template, incluindo override do dia. Se o campo estiver vazio, o comando consulta `escala_vigente` e chama `janela_turno_de(..., ec=ec)`, que lê `ec.marcos_do_dia`; esse método lê DNA da célula e, faltando marcos, cai no template. ([processar_alertas_turno.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:100), [nucleo.py:31](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/nucleo.py:31), [selecao_periodo.py:136](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/selecao_periodo.py:136), [selecao_periodo.py:151](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/selecao_periodo.py:151), [models.py:1094](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/models.py:1094))

   **`ec=None`: depende do caminho.** Com `dt_fim_previsto=None`, o fallback executa `if not ec: continue`: **cala**. Com fim materializado preenchido, essa consulta é pulada; a consulta posterior `_ec_phe=None` também **não veta** o nascimento, pois só pula quando existe vínculo com `permite_hora_extra`. Logo não há veto universal por falta de vínculo nesse emissor. ([processar_alertas_turno.py:101](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:101), [processar_alertas_turno.py:107](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:107), [processar_alertas_turno.py:134](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:134))

   As **36 h** são alcance da busca, não prazo de nascimento. O `prazo_sla` gravado também é outro relógio: `prazo_de_cobranca(agora)` acrescenta **20 minutos à emissão**. ([processar_alertas_turno.py:51](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:51), [processar_alertas_turno.py:347](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:347), [motor.py:45](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/motor.py:45))

2. **`processar_alertas_avancados.py` → `turno_aberto_24h`**

   Examina **ontem**, exige vínculo e dia de trabalho, recalcula a materialização e pergunta `turno_aberto_ha`. Esse juiz exige turno aberto com entrada e retorna literalmente **`(agora - entrada.timestamp) / 3600`**: não desconta jornada prevista nem conta desde a saída prevista. ([processar_alertas_avancados.py:22](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:22), [processar_alertas_avancados.py:49](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:49), [processar_alertas_avancados.py:57](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:57), [turnos.py:3000](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3000))

   Os três números são constantes do juiz: **8 h**, **24 h**, **72 h**. N3 exige fim previsto vencido e idade `<24`; sem fim previsto, substitui o vencimento por idade `>=8`. N5 exige **idade `>=24`, mesmo sem ter vencido o fim previsto**. As 72 h só escolhem `emergencia` em vez de `urgente`. ([turnos.py:2987](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2987), [turnos.py:3010](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3010), [turnos.py:3020](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3020))

   O fim previsto passado ao N3 vem de `janela_turno_de(tipo_escala, ontem, ec=ec)`. N3 envia push/alerta; o nascimento do chamado fica no ramo N5, via `ChamadoColaborador.criar`. Se encontrar chamado anterior, atualiza o existente. ([processar_alertas_avancados.py:71](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:71), [processar_alertas_avancados.py:81](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:81), [processar_alertas_avancados.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:100), [processar_alertas_avancados.py:121](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:121))

   **`ec=None`: cala imediatamente**, antes de calcular idade ou acessar `ec.tipo_escala`; não erra por desreferenciar esse `None`. ([processar_alertas_avancados.py:37](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:37))

3. **`emitir_turno_aberto_retroativo.py` → `turno_aberto_24h`**

   Usa **constante local `HORAS_MIN=24`**, sem chamar o juiz de idade: calcula horas desde a entrada e pula se `<24`. O fato vem de `turnos_do_colab`, exigindo turno aberto e entrada. ([emitir_turno_aberto_retroativo.py:48](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48), [emitir_turno_aberto_retroativo.py:88](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88), [emitir_turno_aberto_retroativo.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:100))

   O fim previsto só é lido **depois dos filtros**, para compor a descrição; não decide prazo. A urgência nasce fixa em `urgente`, inclusive para entradas com mais de 72 h. **`ec=None`: incrementa `sem_vinculo` e cala.** ([emitir_turno_aberto_retroativo.py:104](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104), [emitir_turno_aberto_retroativo.py:141](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:141), [emitir_turno_aberto_retroativo.py:152](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:152), [emitir_turno_aberto_retroativo.py:162](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:162))

   Exclui hoje pelo recorte de `fim`; por padrão apenas simula, e o nascimento via `abrir` depende de `--apply`. ([emitir_turno_aberto_retroativo.py:60](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:60), [emitir_turno_aberto_retroativo.py:73](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:73), [emitir_turno_aberto_retroativo.py:131](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:131))

**As 4 h de `turno_aberto_de`:** são o default `margem_horas=4` de `turno_aberto_vivo`/`expiracao_da_aberta`. A aberta deixa de ser viva quando **`agora > fim_previsto + margem`**. A janela é consultada **sem `ec`**. Sem vínculo, `te=None`; sem tipo ou sem fim previsto, essa função não expira a aberta por esse relógio. A carga de `turno_aberto_de` continua limitada às batidas de hoje−2 até hoje. ([turnos.py:1981](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:1981), [turnos.py:1991](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:1991), [turnos.py:2005](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2005), [turnos.py:2182](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2182), [turnos.py:2205](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2205))

**Outro emissor encontrado, de módulo irmão:** o POST de entrada pode criar `entrada_bloqueada_turno_aberto`, com categoria `turno_aberto`. Sua condição temporal é **14 ≤ horas < 96**, constantes locais, sobre idade da entrada arredondada para uma casa decimal. Sem vínculo, a coleta continua com marcos `None`; a ausência de vínculo não cala esse emissor. ([views.py:275](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/views.py:275), [views.py:291](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/views.py:291), [triagem_batida.py:17](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:17), [triagem_batida.py:28](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:28), [triagem_batida.py:149](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:149), [triagem_batida.py:178](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:178), [triagem_batida.py:188](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:188))

**Passar do prazo ainda não garante nascimento:** as portas `abrir` e `criar` podem recusar chamado de turno quando a célula o absolve; retornam respectivamente `(None, False)` e `None`. Isso é veto posterior ao relógio, distinto de `ec=None`. ([models.py:725](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:725), [models.py:821](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:821), [models.py:848](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:848))

## 30/10 09:14 · sessao 1a4581e339fa · - · - · origem=Ronald (tmux)

- **Não encontrei outro nascimento dos módulos exatos** além dos três comandos: os outros locais consultam chamados existentes (`app/ponto/views.py:392`, `app/ponto/management/commands/marcar_abandono_provisorio.py:64`); o `modulo = 'turno_aberto'` dinâmico pertence ao fechador `_autoresolve_f2_f4`, consulta `cobranca_viva_do_dia` e reconcilia o existente (`app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:543`, `app/ponto/services/triagem_batida.py:589`, `app/ponto/services/triagem_batida.py:597`). A busca não constitui prova formal de inexistência de construções dinâmicas adicionais. [nao verificado]

- **As 4 h são expiração de presença viva**, não limiar de nascimento de `turno_aberto_24h`: `turno_aberto_vivo(..., margem_horas=4)` retorna `None` apenas quando `agora > expiracao`; `expiracao_da_aberta` calcula `fim previsto + margem_horas` (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2011`).

- Essa expiração usa `janela_turno_de(tipo_escala, turno.data_turno)` **sem `ec`**; portanto lê `tipo_escala.marcos_do_dia` ou os campos crus `hora_inicio`/`hora_fim`, inclusive seu fallback de exceção (`app/ponto/turnos.py:2008`, `app/ponto/selecao_periodo.py:151`, `app/ponto/selecao_periodo.py:154`, `app/ponto/selecao_periodo.py:156`, `app/ponto/selecao_periodo.py:159`).

- `turno_aberto_de` consulta o vínculo de **hoje** e batidas entre hoje−2 e hoje; sem vínculo, `te=None`, pula leitura de marcos e ainda chama o pareador e `turno_aberto_vivo`; `expiracao_da_aberta` retorna `None` sem tipo, logo não o expira por esse relógio (`app/ponto/turnos.py:2179`, `app/ponto/turnos.py:2182`, `app/ponto/turnos.py:2186`, `app/ponto/turnos.py:2205`, `app/ponto/turnos.py:2213`, `app/ponto/turnos.py:2242`, `app/ponto/turnos.py:2005`).

- **Módulo irmão `entrada_bloqueada_turno_aberto`:** o POST de entrada cria-o quando `tem_bloqueio_turno` e não há regularização pendente (`app/ponto/views.py:275`, `app/ponto/views.py:291`, `app/ponto/views.py:299`). O prazo vem de constantes **14 e 96 h**; a decisão é `14 <= horas < 96` (`app/ponto/services/triagem_batida.py:17`, `app/ponto/services/triagem_batida.py:18`, `app/ponto/services/triagem_batida.py:28`). As horas são `agora − entrada`, arredondadas para uma casa antes da comparação (`app/ponto/services/triagem_batida.py:181`, `app/ponto/services/triagem_batida.py:188`, `app/ponto/services/triagem_batida.py:206`).

- Nesse irmão, **`ec=None` não cala a coleta**: `_mi_tri`/`_mb_tri` permanecem `None` e o código continua pareando e preenchendo `tem_bloqueio_turno` (`app/ponto/services/triagem_batida.py:149`, `app/ponto/services/triagem_batida.py:150`, `app/ponto/services/triagem_batida.py:151`, `app/ponto/services/triagem_batida.py:178`, `app/ponto/services/triagem_batida.py:206`). O aviso de cadastro “Sem escala” também não retorna: gera sinal e continua (`app/ponto/services/triagem_batida.py:92`, `app/ponto/services/triagem_batida.py:123`). A consulta `turno_aberto_de` controla outro campo do contexto e não o predicado de bloqueio (`app/ponto/services/triagem_batida.py:199`, `app/ponto/services/triagem_batida.py:200`, `app/ponto/services/triagem_batida.py:206`).

## 30/10 09:14 · sessao 5f4f8da95cc5 · O261-NOTURNO · verificador · origem=Code

- **[P2] Falso positivo introduzido:** `'Ola {nome}'.format(**{**{'nome': 'Ana'}})` retorna `'Ola Ana'`, mas o detector atual acusa `[1]`; antes retornava `[]`. O novo `all(...)` rejeita o desempacotamento interno sem considerar que o receptor literal não usa `{data}`. [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170](/home/ronald/codex_ws/e8c314e0e07e3af5e4fce78592a704117d2b85c0/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170)

Varredura executada em memória: **nenhum apontamento nos 787 arquivos elegíveis**, conforme o escopo do teste. [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:197](/home/ronald/codex_ws/e8c314e0e07e3af5e4fce78592a704117d2b85c0/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:197)

## 30/10 09:13 · sessao 3a9739218e03 · - · - · origem=Ronald (tmux)

| Caminho | Chamada à porta | Veto próprio no trecho |
|---|---|---|
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:504` | Não há consulta própria de régua/célula no núcleo; delega a abertura. (`app/chamados/services/disputa_emissao.py:419`, `app/chamados/services/disputa_emissao.py:504`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:908` | Filtra chamados órfãos regularizáveis, sem perguntar o veto de régua do novo pai. (`app/chamados/services/disputa_emissao.py:892`, `app/chamados/services/disputa_emissao.py:908`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:1128` | Recusa âncora encerrada; não pergunta veto de régua/célula do novo pai. (`app/chamados/services/disputa_emissao.py:1107`, `app/chamados/services/disputa_emissao.py:1109`, `app/chamados/services/disputa_emissao.py:1128`) |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | `ChamadoColaborador.criar`, `app/chamados/services/abertura.py:172` | Validação de tipo/dia e dedup; não consulta própria de veto de régua. (`app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:152`, `app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`) |
| `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa → reconciliador::reatar_pergunta_orfa → reemitir_chamado_do_dia` | `reatar_pergunta_orfa` em `app/chamados/services/disputa_emissao.py:409` → c2 em `app/chamados/reconciliador.py:1518` → `abrir` em `app/chamados/reconciliador.py:862` | A consulta própria está no c2: célula em `app/chamados/reconciliador.py:838`; `motivo_do_veto` em `app/chamados/reconciliador.py:854`, condicionado por `not _costura_no_vivo` em `app/chamados/reconciliador.py:852`. A função `gerar_perguntas_disputa` delega. |

A fábrica também delega: `perguntar_pela_celula → perguntar` em `app/chamados/services/fabrica_da_celula.py:149`; `perguntar → criar_ou_atualizar_disputa` em `app/chamados/services/fabrica_da_celula.py:122`; perguntas/c2 pela chamada em `app/chamados/services/fabrica_da_celula.py:124`.

O passivo tem consulta própria na **medição**: `motivo_do_veto` em `app/ponto/services/furos_sem_cobranca.py:96` e `celula_veta_cobranca` em `app/ponto/services/furos_sem_cobranca.py:98`. A cobrança filtra a classificação e delega ao cartório em `app/ponto/services/furos_sem_cobranca.py:125` e `app/ponto/services/furos_sem_cobranca.py:128`. Não há base para declarar automaticamente essa medição apagável quando o veto entrar na porta. [nao verificado]

A referência literal da L-111 está em `app/docs/LEIS.md:137`: trata da função que perdeu o último chamador de produção; não permite concluir que todo leitor diagnóstico perde sua função quando a porta passar a vetar. (`app/docs/LEIS.md:137`)

## 30/10 09:13 · sessao f4b9c20ce21e · - · - · origem=Ronald (tmux)

`emitir_turno_aberto_retroativo` usa **idade da entrada**, com **vínculo vigente como requisito separado**: `HORAS_MIN = 24` é constante local; calcula `(agora - localtime(ts)).total_seconds()/3600` e pula somente se `<24`, portanto inclui exatamente 24 h. Não calcula 24 h a partir de duração ou fim previsto. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48`, `:94`, `:100`, `:101`)

- A fonte do fato é `turnos_do_colab(c, ini, fim, celulas=cels)`; exige `t.aberto` e timestamp de entrada. Não chama `turno_aberto_ha`, `turno_aberto_de` ou `margem_horas` nesse caminho. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:68`, `:88`, `:92`, `:94`, `:100`)
- **`ec=None` cala**: consulta `escala_vigente(c,data)` depois do limiar; incrementa `sem_vinculo` e executa `continue`. Não há acesso a `ec.tipo_escala` nesse ramo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `:105`, `:106`, `:107`)
- O pareador pode retornar turnos de dia sem dono: segmentos admitem `_dono=None`; `_marcos_de(None)` retorna `(None,None)` e o segmento segue para `parear_turnos`. Portanto o silêncio por vínculo é explícito no emissor, não impossibilidade de formar o turno. (`app/ponto/turnos.py:1890`, `:1932`, `:1933`, `:1938`, `:1953`, `:1965`, `:1973`)
- A leitura de previsto `hora_fim=(ec.marcos_do_dia(data) or [None,None])[1]` ocorre **depois** de limiar, vínculo, ausência e dedupe; exceção é engolida e resultado serve somente à descrição. Não decide prazo ou nascimento. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:141`, `:143`, `:144`, `:162`)
- Recorta `fim` para ontem se `fim>=hoje`; isso é exclusão de hoje, independente de idade da entrada. O intervalo `ini/fim` vem dos argumentos obrigatórios. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:56`, `:57`, `:70`, `:73`, `:74`)
- Carrega células com constantes `ini-2 dias` e `fim+2 dias`; são alcance de alimentação, não prazo de nascimento. `turnos_do_colab` carrega batidas via `janela_com_pad`, cuja implementação retorna `ini-2 dias,fim+1 dia`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `:87`; `app/ponto/turnos.py:1845`, `:1857`, `:1858`)
- Outros silêncios antes da porta: ausência cobrindo cobrança; `turno_aberto` VIVO do mesmo dia ou `turno_aberto_24h` NÃO FECHADO do mesmo dia. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`, `:118`, `:125`)
- Por padrão é dry-run; com `--apply` chama `ChamadoColaborador.abrir` com módulo/categoria `turno_aberto_24h`, urgência fixa `urgente`. A porta ainda pode vetar por célula absolvida; o comando contabiliza `_ch is None` como `recusado_celula`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:60`, `:131`, `:146`, `:147`, `:150`, `:152`, `:169`; `app/chamados/models.py:726`, `:729`)
- `escala_vigente` delega a `vinculo_do_dia`: geradora da célula fora de `data_inicio..data_fim` retorna `None`; sem geradora escolhe vínculo que cobre a data ou `None`. (`app/escala/servico_jornada.py:55`, `:57`, `:58`; `app/escala/alimentacao.py:237`, `:250`, `:251`, `:253`, `:254`)

## 30/10 09:13 · sessao 89d6d93ce23b · - · - · origem=Ronald (tmux)

| Caminho conferido | Chamada à porta | Escrita final | Consulta própria |
|---|---|---|---|
| `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `handle:439` → `abrir:688` | `app/chamados/models.py:730`; se dedup encerrado, `:686` → `:770` → `:1089` → `:231` | **copia de veto**: régua em `detectar_ausencias.py:600` e `:604`; célula em `:626`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Alias detector `:38`, delegação `:142` → detector `:688` | Mesmas escritas acima (`app/chamados/models.py:730`, `:231`) | Herda detector; chamada delegada em `emitir_furo_retroativo.py:142`. Não há outro gate de régua/célula no corpo lido (`:30`, `:109`, `:127`, `:142`). |
| `app/ponto/management/commands/reconciliar_grade.py::Command._emitir_furo` | `abrir:499`; também `reabrir:520` | `app/chamados/models.py:730` ou `:231` | **copia de veto**: régua em `reconciliar_grade.py:487`, `:491`; célula em `:494`. |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral/morta/limbo | Detector `:1358` → `detectar_ausencias.py:688` | `app/chamados/models.py:730` ou `:231` | Consulta própria a `motivo_do_veto` em `cartorio.py:1013`, usada na impressão em `:1083`; gate efetivo da emissão herdado do detector (`detectar_ausencias.py:600`, `:604`, `:626`). |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — parcial | Detector `:1394` → `detectar_ausencias.py:688` | `app/chamados/models.py:730` ou `:231` | Mesma consulta para impressão (`cartorio.py:1013`, `:1083`); gate herdado do detector. |
| `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir:862` | `app/chamados/models.py:730` ou `:231` | **copia de veto**: célula em `reconciliador.py:838`; régua em `:854`, condicionada a `not _costura_no_vivo` em `:852`. |
| `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `:141` → `:316` → `abrir:335` | `app/chamados/models.py:730` ou `:231` | Herda porta; `_marco_critico` verifica retorno em `processar_alertas_turno.py:317`; ausência própria em `:97`, hora extra em `:135`. |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar:121` | `app/chamados/models.py:834` | Herda porta; ausência própria em `processar_alertas_avancados.py:116`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir:146` | `app/chamados/models.py:730` ou `:231` | `celula_absolveria` é consulta **só dry** (`emitir_turno_aberto_retroativo.py:131`, `:134`, `:140`), reutiliza o juiz da porta (`app/chamados/models.py:855`, `:858`). |
| `app/ponto/views.py::bater_ponto` | `_Chamado.criar:291` | `app/chamados/models.py:834` | Dedup em `app/ponto/views.py:281`; contexto apenas `pendentes/total` em `:280`; porta depende de célula/módulo em `app/chamados/models.py:849`. |
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View `:197` → serviço `criar:47` | `app/chamados/models.py:834` | Herda porta; contexto do serviço em `app/chamados/services/abertura.py:58` não fornece dia. |

**11 caminhos pelas portas, nenhum escritor por fora neste subconjunto**: chamadas documentadas nas linhas acima; escritores comuns em `app/chamados/models.py:730`, `:834`, `:231`.

**Três sítios próprios de bloqueio duplicado**: detector, grade, c2 (`detectar_ausencias.py:600`, `reconciliar_grade.py:487`, `reconciliador.py:854`). Se a contagem for por caminho herdado, explicitar a unidade evita contar quatro vezes o mesmo detector (`emitir_furo_retroativo.py:142`, `cartorio.py:1358`, `:1394`).

A leitura de veto do cartório serve à impressão e ao rejulgamento quando o veto cai (`app/ponto/services/furos_vetados.py:22`, `cartorio.py:1083`); classificá-la como candidato automático a apagar não está demonstrado. A consulta dry reutiliza o juiz existente (`app/chamados/models.py:855`, `:858`).

L-111 determina apagar função que perdeu seu último chamador de produção; o texto está em `CLAUDE.md:341`.

## 30/10 09:13 · sessao 7cb56ed90292 · - · - · origem=Ronald (tmux)

| Caminho | Chamada intermediária | Escrita final | `abrir` / `criar` / `_gravar_estado` |
|---|---|---|---|
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | `views.py:1053` → `resposta_tardia.py:45` → `_renascer_por_premissa_viva` → `reabrir` em `models.py:770` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `views.py:547` → `acoes_chamado.py:315` → `transicionar` → `_aplicar` em `models.py:183` | `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | `views.py:674` / `views.py:954` → `fio.py:715` → `transicionar` → `_aplicar` em `models.py:183` | `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | `fechamento.py:1416` → `competencia_trancada.py:258` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `fio.py:577` → `acoes_disputa.py:266` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `reabrir` em `app/chamados/services/disputa_emissao.py:1606` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | Nenhuma das três portas | `update(status_local='aberto')` em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | não / não / não — **escritor a migrar** |

Sobre as marcas:

- `resposta_tardia.receber` pergunta diretamente se a célula existe e seu veredito está em `VEREDITOS_ACUSAM`, antes de renascer: é seleção própria de premissa/célula; não é consulta a `motivo_do_veto`. (`app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:45`)
- `reabrir_orfas_enterradas` consulta antecipadamente `pai.recusa_da_reabertura()`. É reutilização da mesma função chamada pela porta, não cópia das guardas; os comentários documentam explicitamente essa distinção. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/models.py:993`, `app/chamados/models.py:1048`, `app/chamados/models.py:1050`)
- As transições humanas chamam `_aplicar` e `_gravar_estado`; as guardas de disputa e premissa do corpo de `transicionar` atingem destinos encerrados/resolvido. (`app/chamados/models.py:142`, `app/chamados/models.py:165`, `app/chamados/models.py:183`, `app/chamados/models.py:260`)
- O reverso da migration não filtra módulo; muda todos os `registrado` para `aberto`. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

## 30/10 09:13 · sessao c38edef84295 · - · - · origem=Ronald (tmux)

- **`ec=None` cala antes de qualquer cálculo de prazo**: o vínculo é obtido para **ontem**, e `if not ec: continue` interrompe esse colaborador. Portanto esse caminho não nasce nem erra por desreferenciar `ec`. (`app/ponto/management/commands/processar_alertas_avancados.py:36-41`)

- O comando só examina `data_turno=ontem`, exige dia de trabalho do vínculo, recalcula materialização e lê a primeira linha de `TurnoMaterializado` desse dia. (`app/ponto/management/commands/processar_alertas_avancados.py:22`, `:49-59`)

- **A idade usada para N5 é tempo desde a entrada real**, sem subtrair duração prevista ou fim previsto: `turno_aberto_ha` exige linha existente, `aberto=True`, entrada existente e retorna `(agora - turno.entrada.timestamp).total_seconds() / 3600`. O comando consome esse resultado. (`app/ponto/turnos.py:2992-3002`; `app/ponto/management/commands/processar_alertas_avancados.py:62-66`)

- **24 h** é constante `TURNO_ABERTO_N5_H = 24`; a decisão é `elif horas >= TURNO_ABERTO_N5_H: return 'N5'`. Assim, o nível N5 independe de ter vencido o fim previsto. (`app/ponto/turnos.py:2988`, `:3010-3014`)

- **8 h** é constante `TURNO_ABERTO_N3_SEM_FIM_H = 8`, usada somente como substituição do fim previsto ausente. Com fim previsto, N3 exige `agora > fim_previsto`; em ambos os casos exige menos de 24 h. N3 envia push e registra `pos_50`; não cria chamado neste ramo. (`app/ponto/turnos.py:2987`, `:3010-3014`; `app/ponto/management/commands/processar_alertas_avancados.py:73-89`)

- O fim previsto passado ao juiz vem de `janela_turno_de(tipo_escala, ontem, ec=ec)`, que consulta `ec.marcos_do_dia(data_ref)`; se essa consulta lançar exceção, usa `tipo_escala.hora_inicio/hora_fim`; sem um desses marcos retorna `(None,None)`. Compõe data/hora e leva o fim para o dia seguinte se cruza meia-noite. (`app/ponto/management/commands/processar_alertas_avancados.py:70-72`; `app/ponto/selecao_periodo.py:136-150`)

- **72 h** é constante `TURNO_ABERTO_EMERGENCIA_H = 72`, usada apenas em `urgencia_turno_aberto`: emergência a partir de 72 h, urgente antes. Não é um segundo limiar de nascimento. (`app/ponto/turnos.py:2989`, `:3018-3020`; `app/ponto/management/commands/processar_alertas_avancados.py:119-130`)

- No N5, primeiro procura `turno_aberto` vivo **de ontem** ou `turno_aberto_24h` não fechado **sem filtro de dia**; se encontra, escala o existente. Se não encontra, cria `turno_aberto_24h` com `data_turno=ontem` e timestamp da entrada. (`app/ponto/management/commands/processar_alertas_avancados.py:100-109`, `:120-140`, `:165-174`)

- Além do vínculo e dia de trabalho, há silêncio por isenção, ausência de turno aberto/entrada, chamado existente silenciado e ausência que cobre o dia quando não existe chamado. (`app/ponto/management/commands/processar_alertas_avancados.py:31-39`, `:49-50`, `:64-65`, `:112-117`)

- O nascimento passa por **`ChamadoColaborador.criar`**, não `abrir`; a porta resolve célula e pode retornar `None` se ela absolve o fato. O emissor verifica `_novo is None` e continua antes do push. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `:141-144`; `app/chamados/models.py:805-825`, `:848-851`)

- Não há uso de `turno_aberto_de` nem `margem_horas` no cálculo deste emissor: ele chama `turno_aberto_ha`, `nivel_turno_aberto` e `urgencia_turno_aberto`. (`app/ponto/management/commands/processar_alertas_avancados.py:62-72`, `:119`)

## 30/10 08:57 · sessao aed133b16f47 · O261-NOTURNO · verificador · origem=Code

- **P2 — Ainda pode mostrar dia diferente do texto.** Reproduzi em memória: identidade 21/10, instante 22/10 e pergunta `Confirme o horário da saída no dia 22/10/2026.` → `dia_mostrado` retorna **21/10**. O ponto final quebra a igualdade com o produtor e aciona o fallback; texto ISO com hora em 22/10 também retorna 21/10. A desigualdade não prova que o texto nomeia a identidade. Esse resultado chega ao cabeçalho e à API. (`app/chamados/models.py:2178`; `app/templates/chamados/partials/_card_atendimento.html:90`; `app/api/views.py:1169`). Ocorrência em produção: [não verificado].

- **P2 — O detector ganhou falso negativo em `.format(**dict)`.** Reproduzi `_copias("t.format(**{**ctx})") == []` e `_copias("t.format(**{key: value})") == []`, inclusive quando `ctx` contém `data` ou `key == 'data'`. O filtro considera seguro qualquer `ast.Dict` sem chave constante `'data'`, embora desempacotamentos e chaves dinâmicas possam fornecê-la. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168`).

Executei o detector original em memória sobre a cópia: **zero ocorrências em 787 arquivos**, com os exemplos positivos/negativos passando. O perímetro é `app/`, excluindo testes, migrações e caches; os dois donos são dispensados, e a isenção nominal do espelho fica em `chamados/models.py`. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:142`, `:156`, `:186`, `:195`).

O `select_related('celula')` cobre a relação lida pela propriedade; não encontrei defeito adicional nessa alteração nem no uso de `cached_property`. (`app/api/views.py:1045`; `app/chamados/models.py:2163`, `:2173`).

Correspondência Git ao hash e conteúdo da resposta da volta 3: [não verificado].

## 30/10 08:53 · sessao e05761e8c23b · O261-NOTURNO · verificador · origem=Code

1. **`dia_mostrado` ainda pode contradizer o texto gravado.** Reprodução em memória: `orfao_14h`, célula de 21/10 com saída em 22/10, contexto `data='2026-10-21T19:00:00'`. O produtor grava “saída no dia 2026-10-21T19:00:00”, pois devolve a data bruta quando o parse falha; a propriedade reconstrói “21/10/2026”, a igualdade falha e retorna **22/10**. Esse texto pode ser persistido com `data_evento=21/10`, porque a emissão extrai os primeiros dez caracteres para a identidade. Ocorrência em produção: [nao verificado]. Referências: [query.py:99](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:99), [models.py:2176](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2176), [disputa_emissao.py:347](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/disputa_emissao.py:347), [disputa_emissao.py:389](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/disputa_emissao.py:389).

   **`padrao` e motivo desconhecido não demonstraram esse mesmo erro:** motivo catalogado ignora `padrao`; os quatro motivos com marco estão no catálogo; motivo sem marco retorna a própria `data_evento` antes da comparação. Referências: [query.py:87](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:87), [query.py:64](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:64), [juizes.py:823](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:823), [perguntas.py:34](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/perguntas.py:34).

2. **Há N+1 de célula na API.** O queryset carrega `chamado_destino__celula`, mas a propriedade acessa outra relação, `p.celula`: até um SELECT adicional por pergunta datada com essa FK. Referências: [views.py:1043](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1043), [views.py:1169](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1169), [models.py:2172](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2172).

   Em cobrança/aviso, esse carregamento por pergunta **já acontece no juiz**, antes da propriedade: o helper seleciona apenas `disputa`, e `pergunta_viva` acessa `celula` na chave do memo. A segunda chamada da propriedade é condicional e omite `colaborador`, portanto não executa a query do produtor. Referências: [aviso_pendencia.py:9](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/aviso_pendencia.py:9), [juizes.py:757](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:757), [juizes.py:791](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/juizes.py:791), [models.py:2176](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2176), [query.py:102](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/catalogo/query.py:102).

3. **O card repete o cálculo:** `dia_mostrado` é propriedade sem cache, acessada no cabeçalho e novamente para cada opção. Nesse caminho a célula já vem carregada; repete processamento, sem acrescentar SELECT da FK. Referências: [models.py:2162](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/models.py:2162), [_card_atendimento.html:90](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/partials/_card_atendimento.html:90), [_card_atendimento.html:164](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/partials/_card_atendimento.html:164), [fio.py:222](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/fio.py:222).

4. **A API ainda entrega o dia cru junto do rótulo corrigido.** `data_evento` continua D e `data_evento_label` pode ser D+1; exibição do campo cru pelo cliente nativo: [nao verificado]. O atendimento com `base_app` inclui o mesmo card corrigido; cobrança usa `dia_mostrado`; o PDF lê dias do espelho e marcações, o que não comprova outro leitor da data textual da pergunta. Referências: [views.py:1168](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/api/views.py:1168), [meu_atendimento.html:127](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/templates/chamados/meu_atendimento.html:127), [context_processors.py:210](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/core/context_processors.py:210), [cobranca.py:83](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/services/cobranca.py:83), [pdf_espelho.py:598](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/relatorios/pdf_espelho.py:598), [pdf_espelho.py:872](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/relatorios/pdf_espelho.py:872).

5. **O selo AST mantém falsos negativos.** Reproduções retornam `[]`: `t='dia {data}'; k='{data}'; y=t.replace(k,x)` e `f(pergunta='dia {data}')`. O detector só considera certos pais imediatos do literal e não acompanha variáveis. Além disso, exclui `models.py` inteiro e só percorre `chamados`, deixando substituições fora desse perímetro sem fiscalização. Referências: [test_noturno_le_o_dia_do_instante.py:155](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155), [test_noturno_le_o_dia_do_instante.py:137](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137), [test_noturno_le_o_dia_do_instante.py:181](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:181).

6. **O selo AST mantém falsos positivos.** `t.format(**{'nome': x})` e `'Olá {nome}'.format_map(c)` são marcados sem substituir `data`; `raise E('placeholder {data} residual' + detalhe)` também é marcado, porque qualquer `BinOp` com esse literal conta como template. Referências: [test_noturno_le_o_dia_do_instante.py:150](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150), [test_noturno_le_o_dia_do_instante.py:158](/home/ronald/codex_ws/31b8cec02ab0de6ec4e3ab040d19f884adf4d8a9/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158).

## 30/10 08:51 · sessao f438f9b87014 · - · - · origem=Ronald (tmux)

- **API ainda expõe duas datas diferentes**: `data_evento` permanece o dia de identidade; `data_evento_label` usa `dia_mostrado` (`app/api/views.py:1168-1169`). O endpoint atende o colaborador autenticado (`app/api/views.py:973-979`). Se o cliente nativo usa o campo cru para exibição: **[nao verificado]**.

- **Não confirmei outro leitor público direto por `data_evento`**: o card mostra `dia_mostrado` no cabeçalho e na confirmação (`app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`). O mesmo card é incluído no atendimento (`app/templates/chamados/meu_atendimento.html:126-127`), cuja casca pode ser `base_app.html` (`app/templates/chamados/meu_atendimento.html:1`, `app/core/context_processors.py:210`).

- **Push de cobrança acompanha a propriedade**: os dias vêm de `p.dia_mostrado` (`app/chamados/services/cobranca.py:83`), entram no corpo (`app/chamados/services/cobranca.py:122-136`) e são enviados pelos canais FCM/webpush (`app/chamados/services/cobranca.py:105-113`, `app/chamados/services/cobranca.py:186-187`). O push de abertura da disputa só informa o motivo, sem dia (`app/chamados/services/disputa_emissao.py:1491-1498`).

- **PDF do espelho não oferece prova de outro leitor da data da pergunta**: recebe os dias do espelho, recorta por `d['data']` e ordena por essa chave (`app/relatorios/pdf_espelho.py:592-599`); desenha as marcações pelas batidas (`app/relatorios/pdf_espelho.py:872-878`). Equiparar esse dia de turno ao dia textual da pergunta seria **[nao verificado]**.

- **Ocorrências que não devem virar falso achado de audiência**: o modal mostra `data_evento` via `data_do_fato` (`app/templates/chamados/partials/modal_fio.html:159-180`, `app/chamados/services/fio.py:87-88`), mas exige gestor (`app/chamados/views.py:872-874`); a outra serialização crua é a fila de validação do copiloto (`app/api/views_mensageria.py:523-537`, `app/api/views_mensageria.py:568`).

- **Resíduo de template, sem comprovar leitor válido de pergunta**: `meu_atendimento.html:19` usa `p.data_evento`, mas está dentro do laço `reg` (`app/templates/chamados/meu_atendimento.html:10-19`), e a view não entrega `p` no contexto (`app/chamados/views.py:633-652`).

## 30/10 08:51 · sessao 4195d57157cb · - · - · origem=Ronald (tmux)

- **P2 — N+1 na API permanece.** O queryset carrega `chamado_destino__celula`, mas não `PerguntaDisputa.celula`; ao serializar cada pergunta com `data_evento`, `dia_mostrado` acessa essa outra FK. São até N SELECTs adicionais para N perguntas datadas com `celula_id`, inclusive quando várias apontam à mesma célula. Referências: `app/api/views.py:1043-1045`, `app/api/views.py:1168-1169`, `app/chamados/models.py:1847-1848`, `app/chamados/models.py:2169-2172`.

- **Cobrança/aviso também têm carregamento por pergunta, mas ele já ocorre antes da property.** O helper traz só `disputa` e chama `pergunta_viva`; a chave do memo desse juiz lê `p.celula`, carregando a FK. Logo atribuir uma nova query por pergunta ao acesso posterior de `dia_mostrado` nesses dois laços seria incorreto. Referências: `app/chamados/services/aviso_pendencia.py:9-13`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:791`, `app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/cobranca.py:82-83`.

- **As duas chamadas são condicionais e não são duas queries.** A property sempre executa `dia_mostrado_do_marco`; só chama `texto_pergunta_com_data` quando o dia calculado difere de `data_evento`. Essa chamada omite `colaborador`, portanto não entra no SELECT de `CelulaDia` do produtor. Referências: `app/chamados/models.py:2172`, `app/chamados/models.py:2176-2177`, `app/chamados/catalogo/query.py:102-105`.

- **O cálculo é repetido por acesso no template.** `dia_mostrado` é `@property` sem cache; o card acessa no cabeçalho e uma vez por opção de confirmação. O fio já faz `select_related('celula')`, então nesse caminho há repetição do cálculo, sem novo SELECT da FK. Referências: `app/chamados/models.py:2162-2179`, `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:162-164`, `app/chamados/services/fio.py:222-224`.

- **A cobrança efetiva refaz a leitura inteira das pendentes.** `cobrar_colaborador` chama `checar_cobranca`, que busca pendentes, e depois chama `_pendentes` novamente; ambas passam pelo helper sem `select_related('celula')`. Referências: `app/chamados/services/cobranca.py:45-48`, `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:155`, `app/chamados/services/cobranca.py:163`, `app/chamados/services/aviso_pendencia.py:9-13`.

## 30/10 08:51 · sessao ebdf4394c212 · - · - · origem=Ronald (tmux)

- **Falso negativo:** o detector ignora literal em atribuição, dicionário, retorno e keyword diferente de `padrao`. Reprodução em memória: `t='Confirme dia {data}'; k='{data}'; y=t.replace(k,x)` retorna `[]`; `PerguntaDisputa.objects.create(pergunta='Confirme dia {data}')` também retorna `[]`. A decisão considera apenas o pai imediato do literal e não acompanha variáveis. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155-162`.

- **Falso positivo restante:** `t.format(**{'nome': x})` e `'Olá {nome}'.format_map(c)` retornam `[1]`, embora nenhum substitua `data`; a regra marca qualquer `**kwargs` em `.format` e todo `.format_map`. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150-154`. A prosa `raise E('placeholder {data} residual' + detalhe)` também retorna `[1]`, porque todo `BinOp` é classificado como template. Referência: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158`.

- **Cobertura insuficiente para provar produtor único:** o selo exclui `models.py` inteiro e só percorre arquivos Python sob `chamados`; uma substituição nova em `models.py` escapa mesmo que `_copias` a detectasse. Referências: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137-138`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:175-182`.

## 30/10 08:49 · sessao 5c95096a391b · O261-NOTURNO · verificador · origem=Code

- **O card ainda mistura dias na confirmação de escape.** O cabeçalho usa `p.dia_mostrado`, mas “Falta/Atestado/Folga — dia … — confirmar?” usa `p.data_evento`. Quando o marco cai em D+1, a confirmação anuncia D (`app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`). O componente integra o atendimento do colaborador (`app/templates/chamados/meu_atendimento.html:127`).

- **A cobrança push continua anunciando D.** Coleta `p.data_evento`, monta “Sobre o seu ponto nos dias…” e encaminha ao colaborador por FCM/webpush; portanto pode divergir do dia mostrado no card (`app/chamados/services/cobranca.py:83`, `app/chamados/services/cobranca.py:123`, `app/chamados/services/cobranca.py:133`, `app/chamados/services/cobranca.py:186`, `app/chamados/services/cobranca.py:107`, `app/chamados/services/cobranca.py:112`).

- **Outras respostas da API ainda nomeiam o questionário por D.** `aviso_texto_livre` monta “questionario do(s) dia(s)” usando `p.data_evento`; esse texto retorna no campo `aviso` de dois endpoints (`app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/aviso_pendencia.py:21`, `app/api/views.py:1001`, `app/api/views.py:2279`). Sua exibição efetiva pelo cliente é **[nao verificado]**.

- **`dia_mostrado` pode discordar do texto já gravado.** A propriedade calcula o dia pelo DNA atual; ao reutilizar uma pergunta existente, `abrir` devolve o registro sem atualizar `pergunta`. Assim, uma pergunta antiga cujo texto nomeia D pode ganhar cabeçalho D+1 (`app/chamados/models.py:2172`, `app/chamados/models.py:2062`, `app/chamados/models.py:2084`). O próprio classificador contempla perguntas com texto antigo nomeando D (`app/chamados/services/validacao.py:819`).

- **O detector AST tem falso positivo e falso negativo nos papéis escolhidos.** Qualquer keyword contendo `{data}` é acusado: `raise E(message='placeholder {data} residual')` cai no detector, embora a variante posicional seja aceita como prosa (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166`). Já `t.format(**{'data': x})` escapa: a expansão tem `arg=None`, enquanto a regra exige `arg == 'data'` (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147`). Além disso, a varredura cobre somente `chamados/`, sem garantir exclusividade global (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:133`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171`).

A comparação de texto gravado especificamente com **f0d319ba** permanece **[nao verificado]**. A constante atual contém `Confirme o horario no dia {data}` (`app/chamados/catalogo/query.py:45`); o anexo mostra esse mesmo literal antes da substituição, mas não fornece a versão de f0d319ba **[nao verificado]**.

## 30/10 08:48 · sessao 9f402510778f · - · - · origem=Ronald (tmux)

Achados do item (1):

- **Confirmação do escape ainda mostra D:** `app/templates/chamados/partials/_card_atendimento.html:164` interpola `p.data_evento` no modal, enquanto o cabeçalho usa `p.dia_mostrado` em `:90`. Para marco noturno em D+1, o mesmo card mostra duas datas. O card integra o atendimento do colaborador em `app/templates/chamados/meu_atendimento.html:127`; a casca app disponibiliza o diálogo em `app/templates/base_app.html:145`, e ele exibe a mensagem recebida em `app/templates/core/_dialogo.html:233`.

- **Push de cobrança ainda anuncia D:** `app/chamados/services/cobranca.py:83` coleta os dias de `p.data_evento`; `:123` e `:133` formam “Sobre o seu ponto nos dias…”. `:186`–`:187` enviam esse texto ao colaborador, por FCM em `:107` ou webpush em `:112`. Assim, a cobrança continua apontando D quando o card passa a mostrar D+1 (`app/templates/chamados/partials/_card_atendimento.html:90`).

- **Aviso devolvido ao colaborador ainda identifica o questionário por D:** `app/chamados/services/aviso_pendencia.py:20` coleta `p.data_evento` e `:21`–`:24` monta “questionario do(s) dia(s)…”. Esse aviso entra na resposta das APIs em `app/api/views.py:996`–`:1001` e `:2271`–`:2279`, além da resposta web em `app/chamados/views.py:1119`–`:1124`. A exibição efetiva do campo `aviso` pelo cliente nativo é [não verificado].

Não há outro achado concreto para acrescentar: `api_disputa_perguntas` serve o texto salvo sem campo de data em `app/api/views.py:2622`–`:2635`; o PDF de chamados renderiza descrição e followups em `app/relatorios/pdf_chamados.py:129`–`:147`, sem leitura direta de `PerguntaDisputa.data_evento` nesse trecho.

## 30/10 08:48 · sessao d73817e3db6c · - · - · origem=Ronald (tmux)

Achados no detector:

- **Falso positivo de prosa:** `raise E(message='placeholder {data} residual')` produz `[1]`, embora a variante posicional seja explicitamente considerada prosa e aceita. O teste classifica qualquer `ast.keyword` com `{data}`, independentemente do argumento ou função: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`; caso negativo posicional: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166`.

- **Falso negativo no papel `.format`:** `t.format(**{'data': x})` produz `[]`. A verificação exige `k.arg == 'data'`; na expansão `**`, o AST tem `arg=None`, e o dicionário contém `'data'`, sem o literal `'{data}'` exigido pela outra verificação: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:151`.

- **Falso negativo no papel `%`:** `x = f'no dia {{data}}' % ()` produz `[]`. O literal aparece sob `ast.JoinedStr`, pai não aceito pelo detector: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156`.

- **Falso positivo em concatenação de prosa:** `'placeholder {data} residual' + detalhe` produz `[1]`; a seleção aceita todo `ast.BinOp`, sem restringir ao operador `%` anunciado: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:139`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`.

As reproduções acima executaram somente `_copias`, extraído por AST em memória, sem importar Django ou escrever arquivos. [não verificado por arquivo:linha]

Não encontrei produtor de `{data}` fora de `chamados/`; o teste, porém, só percorre `app/chamados`, portanto não garante ausência global: `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:133`, `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171`.

## 30/10 08:47 · sessao 7a5bab0cc3cc · - · - · origem=Ronald (tmux)

Nenhum achado concreto na guarda.

A chamada em `app/chamados/services/validacao.py:834` corresponde à assinatura `registrar_engolido(logger_nome, operacao, exc, **contexto)` em `app/core/observ.py:73`.

Quando `_ts_tipo_do_plantio` levanta, a classificação retorna `LOTE_B` com `sem_instante_do_plantio=True` (`app/chamados/services/validacao.py:830-835`). A fila preserva o item nessa classe e continua a iteração (`app/chamados/services/validacao.py:885-888`); suas contagens incluem B, enquanto `refs_lote` contém apenas A (`app/chamados/services/validacao.py:895-898`). A validação em massa reclassifica cada pergunta e recusa qualquer classe diferente de A (`app/chamados/services/validacao.py:681-683`).

_423 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
