# RELATORIOS-PLANO — censo dos 15 cards de `/relatorios/` e o que fazer com cada um

> Levantado em **26/09/2026** por leitura de codigo + **medicao em prod READ ONLY**.
> Nenhum codigo de produto mudou. Este arquivo e o unico escrito.
> Fatia: **CENSO-RELATORIOS** (fase 1 = medir, fase 2 = este plano).

## 0. METODO E PROVA DE LEITURA

- **Onde rodou**: `saas_ui` (nao `saas_core`). Motivo medido: `saas_core` roda com
  `HASNER_TENANT_URLCONF=config.urls_core` (docker-compose.yml:30) e `config/settings/saas.py:77`
  transforma isso em `ROOT_URLCONF`; renderizar `base.html` la quebra em `NoReverseMatch` antes de
  dar numero. `saas_ui` nao tem a variavel -> `config.urls` inteiro.
- **Como**: `RequestFactory` + superuser + `transaction.atomic()` + `CaptureQueriesContext`, com
  `raise` no fim para o rollback (CLAUDE.md sec. 6, ZONA INVIOLAVEL). Cada bloco contou as queries
  que **nao** sao `SELECT/SAVEPOINT/RELEASE/BEGIN/SET`: **0 em todos os 27 blocos medidos**.
- **Prova do rollback**: `LogAuditoria`/`Batida`/`RegistroPDFAuditavel` antes == depois em cada
  passada (`562050 / 66823 / 1614` na ultima). Os `+3` de `LogAuditoria` observados entre a 1a e a
  2a passada sao **trafego de prod** (cron `veredito_celula`), nao meus — meus blocos tiveram
  `nao_select=0`.
- **O POST de `espelho_lote` NAO foi medido**: ele ESCREVE (`relatorios/views.py:1172`
  `registrar_log('cartao_lote', ...)` e `relatorios/pdf_base.py:152`
  `RegistroPDFAuditavel.objects.create`). Medi so o GET (formulario). Onde nao medi, esta escrito
  **nao medido** — em nenhum lugar ha estimativa passada por medicao.
- **Janela de USO**: `app/logs/access-ui.log*` guarda **8 dias (19/09 a 26/09)**; o resto foi
  rotacionado (`logs/access_rotacao.log`). Nao ha 30 dias de log. Onde existe trilha em banco com
  janela maior (`LogAuditoria.acao='cartao_lote'`, `RegistroPDFAuditavel`), ela esta citada.
- **Frota medida** (`situacao='ativo'`): **548** — emp2 J.A 411 · emp3 JS/JSP 113 · emp4 RA 21 ·
  emp1 Confiance 3 · emp20/21/29 = 0. Cadastro total 870.

---

## A. INVENTARIO DOS 15 CARDS

Os cards sao os do `templates/relatorios/index.html`: **14 sempre visiveis + 1 so para superuser**
(`index.html:145` `{% if request.user.is_superuser %}` -> Log de Auditoria). **15 no total.**
Dois deles apontam para FORA do app (`ponto:flip_fila`, `core:log_auditoria`).

### A1. Tabela do censo (a que vai para o RELATO)

| # | Card (rotulo do admin) | url · view | FONTE (L-002) | ESCALA medida -> 750 -> 5.000 | USO (8d, 19–26/09) | FOSSIL | SOBREPOSICAO |
|---|---|---|---|---|---|---|---|
| 1 | Auditoria de espelhos | `/relatorios/furos/` · `urls.py:30` · `views.py:1331` | **LE a celula**: `escala/services/leitor_celula.py::grade_da_celula` (ata) + `CelulaDia.veredito` (`furos.py:37` `estado_do_dia`, FUROS-ESTADO-PELO-VEREDITO 22/09). Derivacao propria residual: `furos.py:78::escalas_em_limbo` (pendente `_TT3`, `core/juizes.py:423`) | emp4 (23 colab) 347 q / 0,500 s · emp3 (115) **2.278 q / 3,299 s** -> ~20 q e 28,7 ms **por colab** -> 750: ~15.000 q / ~21 s -> 5.000: ~99.000 q / ~143 s. **N+1 declarado no proprio codigo**: `furos.py:139-141` (`for c in qs:` + comentario "1 query/colab") | **0 hits** | nao | #2 e `pendencias/espelho/` leem o MESMO furo por 3 caminhos; E4 do O54 |
| 2 | Quem cobrar | `/relatorios/ranking-furos/` · `urls.py:31` · `views.py:1416` | **LE** `FuroDiario` (escritor unico = cron `apurar_furos_diarios`) via `escala/services/furos_diarios.py::ranking_colabs`/`universo_empresa` + **juiz do TXT** `folha/export.py::classificar_export` (`views.py:1478-1480`). Derivacao propria: `views.py:1391::_pilulas` (% por posto/praca) e o PDF (`views.py:1498`, pendente `_TT3`, `core/juizes.py:425`) | emp4 38 q / 0,107 s · emp3 129 q / 0,242 s · **emp2 (418) 425 q / 0,712 s** -> ~1,0 q e 1,7 ms/colab -> 5.000: ~5.100 q / ~8,5 s. Sem N+1 de peso | **0 hits** | nao. `FuroDiario` vivo: 14.051 linhas, `max(avaliada_em)=26/09 09:37` nas 3 empresas | #1, `pendencias/*`, tela Fechamento (`templates/ponto/fechamento.html:75` linka "Quem cobrar") |
| 3 | Fila do flip E/S | `/ponto/flip-fila/` · `ponto/urls.py:51` · `ponto/views.py:2558` | `ponto/services/flip_auto.py::avaliar_fila` — decide pelo **tipo gravado** da batida, nao pelo marco | **4.151 q / 5,763 s** na frota inteira (dias=14) -> 5.000: ~38.000 q / ~53 s (linear). Pior N+1 relativo de todos: `ponto_batida` x396, `escala_celuladia` x339, `escala_escalacolaborador` x273 por passada | **0 hits** | nao | **E2/E3 do O54** (o juiz da batida decide pelo marco) — o card perde razao de existir |
| 4 | Cartoes-ponto em lote | `/relatorios/espelho/lote/` · `urls.py:29` · `views.py:1101` | `relatorios/services.py:124::gerar_pdf_espelho_lote_bytes` -> `pdf_espelho.py::gerar_pdf_espelho` (auditavel, com hash) e, quando ele recusa, `gerar_pdf_espelho_informacional_bytes` (pendente `_TT5`, `core/juizes.py:522`). Janela pelo juiz: `folha/export.py::janela_competencia` (services.py:149) | GET (form) 2 q / 0,005 s. **POST nao medido** (escreve `LogAuditoria` + `RegistroPDFAuditavel`) | **121 hits — 44 GET + 77 POST**, 5 usuarios (u28, u651, u652, u653, u657). Trilha em banco: `LogAuditoria.cartao_lote` = **74 em 8d / 75 em 90d** (a trilha nasceu em 17/09), usuarios JDP01 23 · JDP02 20 · ronald_ti 14 · GERENCIAJULIANI 10 · JSP02 8 | nao | **E4 do O54** (cartao == espelho == fechamento == TXT) |
| 5 | Horas por colaborador | `/relatorios/horas/` · `urls.py:11` · `views.py:30` | **LE `FechamentoMensal` gravado** (autoridade para o numero). Mas a **competencia e mes civil** (`mes`/`ano` do GET), nao a janela 21->20: pendente `_TT7` "qual competencia a tela mostra?" em `core/juizes.py:616` — "deveria ler `janela_atual` (K8)" | **603 q / 0,790 s** com 603 linhas (09/2026). **N+1 = 596 queries em `colaboradores_empresa`**, causa exata: `Posto.__str__` (`colaboradores/models.py:187` = `f"{self.nome} — {self.empresa}"`) chamado por `{{ f.colaborador.posto }}` (`templates/relatorios/horas_mes.html:66`), e o `select_related` da view (`views.py:41-45`) nao traz `colaborador__posto__empresa`. -> 750: ~750 q / ~0,98 s -> 5.000: ~5.000 q / ~6,6 s | **0 hits** | nao (FM 07=629, 08=618, 09=603) | tela Fechamento Mensal; **FECHAMENTO-ONLINE (O48/O54-E5)** passo 3 |
| 6 | Inconsistencias do periodo | `/relatorios/inconsistencias/` · `urls.py:12` · `views.py:102` | **LE `FechamentoMensal.inconsistencias`/`turnos_abertos`** gravados. Competencia civil -> mesmo pendente `_TT7` (`core/juizes.py:612`) | **425 q / 0,288 s** (425 linhas). Mesmo N+1 do `Posto.__str__` (`templates/relatorios/inconsistencias.html:42`); aqui o `select_related` (`views.py:116-117`) nem traz `posto__praca` -> 5.000: ~5.000 q / ~3,4 s | **0 hits** | nao (425 de 603 FM de 09 tem `inconsistencias>0` = **70% da frota**) | #5, tela Fechamento, **E4/E5 do O54** |
| 7 | Absenteismo mensal | `/relatorios/absenteismo/` · `urls.py:13` · `views.py:153` | **DERIVA**: laco em `views.py:177-190` chamando `ponto/turnos.py:1041::dias_da_ausencia` — que **nao e o juiz declarado**. O juiz de "que dias a ausencia ocupa?" e `ponto/turnos.py:1000::cobertura_ausencia_periodo` (`core/juizes.py:165`); `dias_da_ausencia` e uma 2a funcao no mesmo arquivo que **replica a regra** sem `_pelo_efeito(para=)` nem o desempate B5-s. Pendente `_TT7` em `core/juizes.py:620` | 5 q / 0,125 s — laco em Python, sem N+1 (`select_related` em `views.py:177`). 5.000: nao medido; cresce com nº de `Ausencia`, nao de colab | **0 hits** | nao (4.029 Ausencia, 3.604 aprovadas) | #8, #10 e a linha do tempo da **O30 F2/F3** ("uma funcao monta a linha do tempo") |
| 8 | Atestados acumulados | `/relatorios/atestados/` · `urls.py:14` · `views.py:222` | **DERIVA** igual ao #7 (`views.py:261-262` `dias_da_ausencia`) + `ponto/catalogo/ausencias.py::abonam_vigentes` (esse e catalogo). Pendente `_TT9` em `core/juizes.py:622`. O template ainda tem regra propria: `{% if r.dias_empresa >= 15 %}` (pendente `_TT3`, `core/juizes.py:475`) | 4 q / 0,038 s. Sem N+1 | **0 hits** | nao (302 atestados aprovados em 2026) | **O30 F3 ATESTADOS LOTE** absorve; #7 |
| 9 | Ferias vencendo | `/relatorios/ferias-vencendo/` · `urls.py:15` · `views.py:306` | **LE o juiz**: `ferias/models.py:205::vencendo` -> `status_de_periodo` (AUS-ESPELHO-1, `views.py:329`). Mas o **template re-deriva**: `{% if p.data_limite_concessao <= hoje %}` (`templates/relatorios/ferias_vencendo.html`, pendente `_TT10`, `core/juizes.py:477`) e a view segue listada em `core/juizes.py:614` | 52 q / 0,057 s (49 linhas) — **N+1 de 49 queries em `colaboradores_empresa`**, mesma causa do #5 (`Posto.__str__`). 5.000: ~1 q/linha, nao medido em numero de periodos | **0 hits** | nao (1.044 `PeriodoAquisitivo`) | `relatorios/services.py::gerar_pdf_ferias_gestao_bytes` (mesmo pendente, `core/juizes.py:610`) e a tela de Ferias |
| 10 | Colaboradores afastados | `/relatorios/afastados/` · `urls.py:16` · `views.py:358` | **LE o juiz** `ponto/turnos.py::afastamentos_no_dia(para='cobranca')` desde A-AFASTADO-JUIZ 20/09 (`views.py:370`); o desempate "ultimo escreve" foi copiado a mao (`views.py:371`). Segue em `core/juizes.py:624` por OUTRA pergunta (duracao) | 16 q / 0,017 s. Sem N+1 | **0 hits** | nao | #7 (mesma `Ausencia`); **O30 F2** |
| 11 | Extrato parcial do mes | `/relatorios/extrato-parcial/` · `urls.py:17` · `views.py:407` | **DERIVA TUDO, em runtime, por colaborador**: `views.py:489` `motor.calcular_mes(...)` via `core/regua_cct.py::get_motor_cct`, com previsto montado no proprio laco (`views.py:470-476` cursor dia a dia chamando `esc.eh_dia_trabalho`). DOIS pendentes de dinheiro: `core/juizes.py:626` (a view) e `:628` (a chamada do motor) — "deveria ler FM / `eventos_do_fechamento` + `janela_competencia`" | praca66 (21 colab) **2.321 q / 2,646 s** · praca4 (27) **3.634 q / 3,966 s** -> ~110–135 q e 126–147 ms **por colab**. Loop: `views.py:456` `for colab in qs_colab`. -> 750: ~101.000 q / **~110 s** -> 5.000: ~675.000 q / **~12 min**. Confere com o O48, que ja chamava leitor de frota de "inviavel online (~65 s)" | **0 hits** | nao | **FECHAMENTO-ONLINE (O48 / O54-E5)** — e literalmente o passo 6 ("leitores de FROTA, inviavel online, cache declarado"); #5 |
| 12 | Beneficios consolidados | `/relatorios/beneficios/` · `urls.py:18` · `views.py:557` | LE `colaboradores.Beneficio` gravado + `Sum`. Competencia civil -> pendente `_TT7` (`core/juizes.py:618`) | 7 q / 0,013 s, **resposta VAZIA** | **0 hits** | **FOSSIL MEDIDO**: `Beneficio` tem **4 linhas no banco inteiro** (2 `ativo`), ultima competencia **05/2026**; para 09/2026 = **0**. O card abre vazio e sempre vai abrir vazio | nenhuma (VA/VT nao tem fonte viva) |
| 13 | Gerador de relatorio (builder) | `/relatorios/builder/` · `urls.py:10` · `views.py:1279` | Le `Colaborador` cru (cadastro) — nao responde pergunta de dia/hora/soma, entao **fora do alcance da L-002**. Acessores em `views.py:1196::_acc_escala` (varre `c.escalas.all()` procurando `ativa`) e `:1205::_acc_sistema` | form 2 q / 0,004 s · **CSV das 870 pessoas 6 q / 0,102 s** (prefetch correto). O unico card que ja escala. 5.000: ~6 q / ~0,6 s | **0 hits** | nao | filtro proprio (empresa/praca/situacao) x **filtro universal L6** |
| 14 | Historico de chamados | `/relatorios/historico-chamados/` · `urls.py:19` · `views.py:651` (PDF em `:827` / `urls.py:20`) | Le `ChamadoColaborador` + followups cru, filtro por `status_local`/`area` literais. Nao pergunta ao motor (`chamados/catalogo/motor.py`) o que esta VIVO — mas tambem nao classifica: **so exibe**. Nao esta no registro de pendentes | vazio 1 q / 0,005 s · **colab 651 (1.148 chamados) 7 q / 0,417 s** (prefetch de followups OK). 5.000: irrelevante (e por colaborador) | **0 hits** | nao (24.634 chamados; `RegistroPDFAuditavel` tipo `historico_chamados` = **3 em toda a historia**) | fio do chamado (`chamados/detalhe_local.html`) |
| 15 | Log de Auditoria (so superuser) | `/auditoria/` · `core/urls.py:32` · `core/views.py:688` | Le `LogAuditoria` cru — e a trilha, e o que ela tem de ser | 5 q / 0,257 s. 562.038 linhas na tabela; a view pagina/filtra. 5.000: nao medido (nao depende de colab) | **1 hit** (u657) | nao | nenhuma |

### A2. Servico · template · parcial · export · permissao · selo — por card

| # | Card | service / modulo | template | parciais | JS proprio | export | permissao (arquivo:linha) | selo/teste que o cobre |
|---|---|---|---|---|---|---|---|---|
| 1 | Auditoria de espelhos | `relatorios/furos.py` (`dataset_furos:129`, `semanas_calendario:183`, `escalas_em_limbo:78`), `relatorios/furos_pdf.py` | `relatorios/furos.html` | `colaboradores/partials/_calendario_grade.html`, `_calendario_legenda.html`, `_geo_modal.html`, `core/_btn_pdf.html` | 0 | **PDF** (`views.py:1383-1384`) | `request.user.is_staff` (`views.py:1333`) — **nao usa `ver_relatorios`** | `relatorios/tests/test_relatorio_janela_da_empresa.py`, `test_furos_juiz.py`, `test_furos_estado_pelo_veredito.py`, `test_furos_irmas.py`, `test_classificar_dia_sem_marco.py` |
| 2 | Quem cobrar | `escala/services/furos_diarios.py`, `folha/export.py::classificar_export`, `views.py::_pilulas:1391` | `relatorios/ranking_furos.html` | `core/_btn_pdf.html` | 1 `<script>` inline (`window.qcFiltro`, pendente `_TT3` em `core/juizes.py:479`) | **PDF** via `_pdf_colaboradores` (`views.py:1498`) | `is_staff` (`views.py:1420`, sem decorator em `:1416`) e **SEM `@login_required`** | `test_ranking_furos_view.py`, `test_quem_cobrar_folha_juiz.py`, `test_hx_qc_fonte_unica.py` |
| 3 | Fila do flip E/S | `ponto/services/flip_auto.py::avaliar_fila` | `ponto/flip_fila.html` | — | `hxConfirmSubmit` inline | — | `tem_acao(user,'regularizar_batida')` (`ponto/views.py:2560`) | **NENHUM** (grep por `flip_fila` em `test_*.py` = 0) |
| 4 | Cartoes em lote | `relatorios/services.py:124`, `relatorios/pdf_espelho.py`, `relatorios/cartao_pela_celula.py`, `relatorios/pdf_base.py` | `relatorios/espelho_lote.html` | `core/_btn_pdf.html` | 1 `<script>` (selecao de colabs, usa `colab-search`/`filtro-search`) | **PDF** (unico arquivo) + trilha `LogAuditoria` | `_requer_staff` = `tem_acao(user,'ver_relatorios')` (`views.py:14-18`) | `test_cauda_trilha_do_cartao.py`, `test_cartao_auditavel.py`, `test_cartao_pela_celula.py`, `test_cartao_cobre_o_periodo.py`, `test_cartao_competencia_pelo_juiz.py`, `test_cartao_nao_perde_batida.py`, `test_cartao_marca_espuria.py`, `ponto/tests/test_contract_espelho_sem_emoji.py` — **o card mais coberto do app** |
| 5 | Horas por colaborador | nenhum (query na view) | `relatorios/horas_mes.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:56`) | `_requer_staff` | so `test_get_sanitizado.py` (sanitiza `mes`/`ano`). **Nenhum selo de numero** |
| 6 | Inconsistencias | nenhum | `relatorios/inconsistencias.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:125`) | `_requer_staff` | **NENHUM** |
| 7 | Absenteismo | `ponto/turnos.py::dias_da_ausencia`, `ponto/catalogo/ausencias.py` | `relatorios/absenteismo.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:191`) | `_requer_staff` | `ponto/tests/test_aus_espelho_2.py:47` — selo de **texto-fonte** (le `relatorios/views.py` como string), nao request |
| 8 | Atestados acumulados | idem #7 | `relatorios/atestados.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:279`) | `_requer_staff` | `relatorios/tests/test_atestados_acumulados_pelo_juiz.py`, `test_get_sanitizado.py` |
| 9 | Ferias vencendo | `ferias/models.py::vencendo`/`status_de_periodo` | `relatorios/ferias_vencendo.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:331`) | `_requer_staff` | `ponto/tests/test_aus_espelho_1.py`, `test_get_sanitizado.py` |
| 10 | Afastados | `ponto/turnos.py::afastamentos_no_dia` | `relatorios/afastados.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:383`) | `_requer_staff` | `ponto/tests/test_afastado_hoje.py` (texto-fonte + import da view) |
| 11 | Extrato parcial | `core/regua_cct.py::get_motor_cct` -> `ponto/motor_calculo_v2.py`, `core/feriados.py`, `escala/models.py::EscalaColaborador` | `relatorios/extrato_parcial.html` | — (nem filtro padrao) | 0 | **CSV** (`views.py:513`) | `_requer_staff` | **NENHUM** — e o card que chama o MOTOR |
| 12 | Beneficios | nenhum | `relatorios/beneficios_consolidado.html` | `core/_filtro_praca.html` | 0 | **CSV** (`views.py:601`) | `_requer_staff` | **NENHUM** |
| 13 | Builder | `views.py::_COLS_COLAB:1208`, `_ORDER_COLAB:1221`, `_pdf_colaboradores:1227` | `relatorios/builder.html` | — | 0 | **CSV + PDF** | `_requer_staff` | **NENHUM** |
| 14 | Historico de chamados | `relatorios/pdf_chamados.py::gerar_pdf_historico`, `relatorios/pdf_base.py` | `relatorios/historico_chamados.html` | `core/_btn_pdf.html` | 1 `<script>` (autocomplete via `colab-search`) | **PDF** (`views.py:827`) | `tem_acao(user,'ver_relatorios')` (`views.py:654`) | `relatorios/tests_pdf_chamados.py` (smoke do PDF), `colaboradores/tests/test_rbac_aliases_s100.py` |
| 15 | Log de Auditoria | nenhum | `core/log_auditoria.html` (`core/views.py:735`, fora do app) | — | — | — | `tem_acao(user,'ver_saude_sistema')` (`core/views.py:690`) | `core/tests/test_contract_trilha.py` |

### A3. Menu e links que apontam para os cards

- **Menu lateral** (unica entrada): `templates/base.html:169` -> `relatorios:index`.
- `templates/ponto/fechamento.html:75` -> **`relatorios:ranking_furos`** ("Quem cobrar", pilula).
- `templates/relatorios/index.html:30` -> **`ponto:flip_fila`**.
- `templates/relatorios/index.html:146` -> `core:log_auditoria` (so superuser).
- `templates/chamados/detalhe_local.html:123` -> `relatorios:baixar_espelho_followup`;
  `:334` -> `relatorios:anexar_espelho_chamado`.
- **Nao ha nenhum link** para `pendencias_cadastro`, `pendencias_espelho`, `espelho_pdf_avulso`,
  `espelho_dinamico` ou `builder` fora do proprio index — as tres primeiras nao tem card nenhum.

### A4. Rotas que existem e NAO sao card (achado: 20 rotas, 15 cards)

`relatorios/urls.py` tem **23** `path()`. Fora dos cards:
- `pendencias/cadastro/` (`urls.py:32` · `views_pend.py:74`) — **sem card e sem link**. Medido:
  emp4 83 q / 0,143 s. Cobertura: `test_pend_cadastro_juiz.py`, `test_pendencias.py`.
- `pendencias/espelho/` (`urls.py:33` · `views_pend.py:87`) — **sem card e sem link**. emp4 30 q /
  0,076 s. Ambas com `is_staff`, **sem `@login_required`**.
- `espelho/avulso/` (`urls.py:28` · `views.py:1006`) — **0 hits em 8 dias**; ja registrada como
  "podem 22, uso 0, sem smoke" em `core/juizes.py:722`.
- `espelho/dinamico/` (`urls.py:34`) — `RedirectView` para `espelho_lote`. **Ninguem a referencia**
  (grep em `templates/` = 0): e compatibilidade de link antigo, candidata a remocao.
- `historico-chamados/pdf/`, `colab-search/`, `filtro-search/`, `espelho/chamado/<id>/anexar/`,
  `espelho/followup/<id>/baixar/` — auxiliares. `colab-search` (151 hits) e `filtro-search`
  (75 hits) sao as rotas MAIS usadas do app em 8 dias, e servem o card #4.

### A5. Codigo morto, duplicado e fossil (com arquivo:linha)

1. **`colaboradores.Beneficio` = tabela fossil**: 4 linhas, ultima competencia 05/2026. O card #12
   (`views.py:557`) e uma tela que nunca tem dado. **MEDIDO**.
2. **Filtro duplicado, linha a linha**, em `horas_mes`: `views.py:38` e `:39` leem `praca` duas
   vezes; `views.py:52-53` aplicam o MESMO `.filter(colaborador__posto__praca_id=praca_id)` duas
   vezes. Inofensivo no SQL, mas e copia literal.
3. **`relatorios/views.py:1196::_acc_escala`** varre `c.escalas.all()` em Python procurando
   `ativa` — com `try/except Exception: pass` engolindo tudo (`:1202`). Regra de "qual escala
   vale" fora de qualquer juiz.
4. **`espelho/dinamico/`** (`urls.py:34`): redirect sem nenhum referenciador.
5. **`RegistroPDFAuditavel` quase-morto por tipo**: 1.614 linhas, **1.605 `espelho_ponto`**, 6
   `outro`, **3 `historico_chamados`**, **0 `extrato_parcial`** — o enum
   (`relatorios/models.py:23-28`) declara `extrato_parcial` e nada nunca gravou com esse tipo.
6. **O caderno da folha sai 99,8% SEM hash CLT — e a causa NAO e recusa, e o botao que o admin
   escolhe.** MEDIDO sobre as **75** linhas de `LogAuditoria.cartao_lote` (a trilha nasceu em
   **17/09 20:33**, entao 75 e tudo o que existe): **1.720 espelhos gerados, 1.716 informacionais,
   23 sem dados**. O corte por filtro: **71 dos 75 lotes usaram PERIODO LIVRE** (`data_ini`/
   `data_fim`), e esse caminho **nunca tenta o auditavel** — `services.py:174` carimba
   `motivo = 'periodo livre'` e vai direto ao informacional: 1.709 gerados, **1.709
   informacionais**. Os outros **4 lotes** foram por competencia: 11 gerados, 7 informacionais,
   **4 auditaveis com hash**. Ou seja: o fallback por RECUSA do auditavel explica 7 espelhos; os
   outros 1.709 sao o admin pedindo datas livres. As duas coisas precisam de cura diferente — a
   primeira e a E2 (furo trava o auditavel), a segunda e a TELA (periodo livre nao deveria custar
   o hash).
7. **Nenhum card usa o filtro universal (L6)**: `core/filtro.py` (`ler:68`, `aplicar:159`) tem UM
   consumidor no sistema inteiro — `templates/holerite/_drawer_celula.html` via
   `core/_filtro_universal.html`. Os 8 cards de lista usam `core/_filtro_praca.html`, com
   empresa+praca so.
8. **RBAC em dois idiomas**: 11 views pedem `tem_acao(user,'ver_relatorios')`; **4 pedem
   `request.user.is_staff`** (`views.py:1333` furos, `views.py:1420` ranking, `views_pend.py:75`,
   `views_pend.py:88`) e **3 dessas nao tem `@login_required`**.

---

## B. FICHA POR CARD

Formato: **destino** · autoridade que ele passa a ler (L-002) · o que muda na tela (lingua de
admin) · o que muda no export · escala em 5.000 · obra de que depende · selo/RED · fatias.

### 1. Auditoria de espelhos — **FUNDIR** (recebe #2 e `pendencias/espelho/`)
- **Autoridade**: segue em `grade_da_celula` + `CelulaDia.veredito`, e passa a receber os periodos
  do dia do **juiz da batida** (O54-E2/E3). `escalas_em_limbo` deixa de classificar por conta
  propria e passa a ler a mesma classe que `pendencias_cadastro` usa.
- **Tela**: vira **a** tela de cobranca. Ganha o **filtro universal** (nome, praca, posto, escala,
  estado) e **periodo livre** no lugar do combo de competencia; o calendario continua igual. Some
  a coluna de "% de furo" (vem do #2, que aposenta) e entra **"Dias com furo" / "Dias devidos"**
  com o rotulo "Precisa cobrar" em vez de "furo".
- **Export**: um so botao **PDF** (`core/_btn_pdf.html`, ja usa) e nada de CSV novo.
- **5.000**: hoje ~20 q e 28,7 ms **por colab** (medido). Obrigatorio: `grade_da_celula` **em lote**
  por empresa+janela (uma consulta de `CelulaDia`, nao uma por pessoa) e **paginacao** dos cards
  (hoje monta todos). PDF da empresa inteira vira **job assincrono** com aviso na tela (L5).
- **Depende de**: O54-E2/E3 (juiz da batida) e do lote de `grade_da_celula`.
- **RED/selo**: emp3 (115 colab) hoje **2.278 q**; depois, **< 30 q** na mesma janela, com a MESMA
  lista de dias por colaborador (diff 0 contra a medicao de hoje). Selo: "nenhuma chamada de
  `grade_da_celula` dentro de laco de colaborador em `relatorios/`".
- **Tamanho**: 3 fatias (lote · filtro universal + periodo livre · absorver #2 e `pendencias`).

### 2. Quem cobrar — **APOSENTAR** (corte dado; ver C)
- Nao recebe autoridade nova. Sai porque a cobranca passa a ser **por excecao** (juiz + Pautas por
  posto + CADASTRO x REALIDADE), nao por ranking de %.
- **Tela**: o card desaparece do index. A pilula de `templates/ponto/fechamento.html:75` passa a
  apontar para **#1** com `?empresa_id=` (o mesmo filtro), porque e de la que o DP vem.
- **Export**: o PDF "quem_cobrar_*.pdf" morre; quem quer lista leva o PDF do #1.
- **5.000**: nao se aplica (medido hoje: ~1 q e 1,7 ms/colab — nao era ele o problema).
- **Depende de**: as Pautas por posto existirem; `pendencias/*` migradas para #1.
- **RED/selo**: rota `relatorios:ranking_furos` fora do `urls.py`, `grep` de `ranking_furos` em
  `templates/` = 0, e `core/juizes.py:425`/`:479` (os dois pendentes `_TT3` dele) **removidos** do
  registro pelo caminho certo (o selo recusa impressao que sumiu).
- **Tamanho**: 1 fatia (com o redirect de `fechamento.html:75`).

### 3. Fila do flip E/S — **APOSENTAR na porta da E3** (corte dado)
- O juiz decide o periodo pelo **marco**, nao pelo `tipo` gravado — a fila humana perde objeto.
- **Tela**: card fora do index. O que sobrar de decisao humana aparece **no dia do colaborador**
  (espelho/calendario), nao numa fila global.
- **Export**: nao tem.
- **5.000**: nao se aplica (mas registre: hoje e **4.151 q / 5,76 s** por abertura).
- **Depende de**: O54-E3 no ar.
- **RED/selo**: `ponto:flip_fila` sem link em `templates/` e o contador
  `periodos_fora_do_juiz` = 0 (ja previsto no O53/O54).
- **Tamanho**: 1 fatia, e ela e' **remocao de link + rota**, nao reescrita.

### 4. Cartoes-ponto em lote — **MANTER e reescrever por dentro** (e o unico card com uso)
- **Autoridade**: `gerar_pdf_espelho` passa a ler os periodos do **juiz da batida** (O54-E2) e o
  informacional deixa de ser um segundo desenho — **cartao == espelho == fechamento == TXT** (E4).
- **Tela**: o formulario ganha o **filtro universal** (hoje ha um seletor proprio alimentado por
  `colab-search`/`filtro-search`) e **periodo livre** ao lado de competencia; e passa a dizer, ANTES
  de gerar, **quantos sairao com hash e quantos sairao informacionais** — hoje o admin so descobre
  no toast depois (`views.py:1188`).
- **Export**: PDF unico, botao padrao (ja conforme L9). Acrescenta **um CSV de conferencia** (quem
  entrou, com hash sim/nao, motivo) porque e disso que a trilha de 17/09 precisa.
- **5.000**: **job assincrono obrigatorio** — POST devolve "gerando, avisamos" (L5) e o PDF fica
  para download; lote sincrono de 5.000 espelhos nao cabe em request. Fatia de 500 paginas por
  arquivo.
- **Depende de**: O54-E2/E4.
- **RED/selo**: **periodo livre passa a gerar espelho COM hash** — hoje `services.py:174` carimba
  `motivo='periodo livre'` e pula o auditavel, e foi assim que **1.709 dos 1.720** espelhos do
  caderno sairam sem hash (MEDIDO, 71 dos 75 lotes). RED: emp4, um lote por datas livres com
  `informacionais=0` na trilha; e cada pagina com o mesmo total do `FechamentoMensal` lido.
- **Tamanho**: 3 fatias (previa honesta · job · filtro universal/periodo livre).

### 5. Horas por colaborador — **FUNDIR com #6 e #11** numa tela so ("Horas da competencia")
- **Autoridade**: `FechamentoMensal` **lido online** (`fechamento_lido`, O48 passo 2) e competencia
  por **`ponto/janelas.py::janela_atual`/`corte_da_empresa`** — nunca mes civil. Mata o pendente
  `_TT7` de `core/juizes.py:616`.
- **Tela**: o combo "Setembro / 2026" vira **"Competencia 21/08 a 20/09"** (a janela da empresa
  escolhida, igual ao #2 de hoje). Colunas: Trabalhadas, Noturnas, Extra 50%, Extra 100%, Atraso,
  Turnos abertos, Status — as tres ultimas vindas de #6 e #11. Filtro universal.
- **Export**: CSV com as mesmas colunas da tela + botao **PDF** padrao (hoje so tem CSV).
- **5.000**: hoje **603 q / 0,79 s** com 603 linhas; o N+1 morre com
  `select_related('colaborador__posto__empresa')` (ou `Posto.__str__` sem `self.empresa`) —
  **1 fatia de 1 linha**. Depois: paginacao de 100 linhas.
- **Depende de**: FECHAMENTO-ONLINE (O48/O54-E5) passo 2 e 3.
- **RED/selo**: a mesma competencia aberta em emp4 (`dia_inicio_competencia != 21`) mostra o mesmo
  intervalo que o #2 mostra hoje; e queries **<= 10** com 603 linhas (medido hoje: 603).
- **Tamanho**: 2 fatias (N+1 + janela pelo juiz; depois a fusao).

### 6. Inconsistencias do periodo — **FUNDIR em #5** (nao sobrevive sozinho)
- **Autoridade**: nao e "inconsistencia", e **furo/dia em aberto** — pergunta do juiz da batida.
- **Tela**: deixa de ser card. Passa a ser **filtro "so com pendencia" dentro de #5**, mais o
  atalho para o dia no #1.
- **Export**: o CSV vai junto do de #5.
- **5.000**: mesmo N+1 do `Posto.__str__` (425 q hoje).
- **Depende de**: #5 e O54-E2.
- **RED/selo**: hoje **425 de 603** linhas de 09/2026 tem `inconsistencias>0` (70% da frota) — o
  selo e que, depois da E2, esse numero caia para o de furos reais e que a tela e o TXT digam o
  mesmo (`furo_so_intervalo` que bloqueia apto = 0, porta da E2).
- **Tamanho**: 1 fatia (dentro da de #5).

### 7. Absenteismo mensal — **REESCREVER** (le o juiz) e depois **FUNDIR** no atalho da O30
- **Autoridade**: trocar `ponto/turnos.py:1041::dias_da_ausencia` por
  **`ponto/turnos.py:1000::cobertura_ausencia_periodo(para=...)`** (o juiz declarado em
  `core/juizes.py:165`) + `ponto/catalogo/ausencias.py::efeito_vigente`; competencia por
  `janela_competencia`. Mata os pendentes `core/juizes.py:620`.
- **Tela**: "Setembro" vira a **competencia da empresa**; a coluna "Dias" passa a dizer
  **"Dias que a ausencia ocupou nesta competencia"** e ganha a coluna **"Efeito"** (paga /
  desconta / suprime) com a palavra do catalogo vigente, nao o codigo do tipo.
- **Export**: CSV com Efeito; PDF padrao junto com a O30 F3.
- **5.000**: hoje 5 q / 0,125 s — cresce com `Ausencia`, nao com colab. Sem N+1. Nao ha obra de
  escala aqui.
- **Depende de**: **O30 F2/F3** ("uma funcao monta a linha do tempo") — e ela que deve ser a fonte.
- **RED/selo**: o atestado de 30 dias comecado no dia 25 conta 6 dias nesta competencia e 24 na
  seguinte, e o total por tipo == `cobertura_ausencia_periodo` da mesma janela (diff 0). Selo:
  `dias_da_ausencia` sem nenhum chamador em `relatorios/`.
- **Tamanho**: 1 fatia.

### 8. Atestados acumulados — **FUNDIR na O30 F3 ("Atestados LOTE")**
- **Autoridade**: idem #7 (`cobertura_ausencia_periodo` + `efeito_vigente`); o `>= 15` do template
  (`core/juizes.py:475`) sai da tela e vira a regra que o catalogo/CCT declara.
- **Tela**: o card morre; o conteudo aparece como **secao "Acumulado no ano (empresa / INSS)"** no
  drawer "Atestados LOTE", com a secao "exige documento e nao tem" que a O30 ja especifica.
- **Export**: PDF+ZIP+CSV da O30 F3 (nao um CSV proprio).
- **5.000**: 4 q hoje; sem problema.
- **Depende de**: **O30 F3**.
- **RED/selo**: os 302 atestados aprovados de 2026 dao o mesmo total na secao e no
  `cobertura_ausencia_periodo`; `atestados_sem_anexo_na_competencia` no payload (ja previsto na O30).
- **Tamanho**: 0 fatias proprias — entra como item da O30 F3.

### 9. Ferias vencendo — **MANTER**, com duas curas
- **Autoridade**: ja le `ferias/models.py::vencendo`/`status_de_periodo`. Falta **tirar a regra do
  template** (`{% if p.data_limite_concessao <= hoje %}`, `core/juizes.py:477`): o rotulo
  "VENCIDA/vencendo" vem do juiz, a tela so pinta.
- **Tela**: a coluna "Limite concessao" ganha **um rotulo ja decidido** ("Vencida" / "Vence em N
  dias"); o campo "dias" (hoje 90 por default) fica explicito como **"Janela de aviso"**. Filtro
  universal.
- **Export**: CSV atual + PDF padrao (hoje o PDF de ferias mora noutro lugar,
  `relatorios/services.py::gerar_pdf_ferias_gestao_bytes`, com o **mesmo pendente**
  `core/juizes.py:610` — os dois passam a ler `status_de_periodo`).
- **5.000**: N+1 de 49 queries por 49 linhas (medido) — mesma cura de uma linha do #5.
- **Depende de**: nada (fatia independente).
- **RED/selo**: um periodo com `data_limite_concessao` no passado e saldo 0 **nao** aparece como
  vencida (o juiz ja diz isso); e a tela e o PDF de gestao dao a mesma lista.
- **Tamanho**: 1 fatia.

### 10. Colaboradores afastados — **FUNDIR** na Linha do Tempo da O30 F2
- **Autoridade**: ja le `afastamentos_no_dia(para='cobranca')`. Tirar a copia do desempate
  (`views.py:371`) e pedir o desempate ao juiz.
- **Tela**: deixa de ser card e vira **filtro "Afastados hoje"** na lista de gente + a linha do
  tempo do colaborador (O30 F2), que e onde o admin vai agir.
- **Export**: o CSV vai junto do da O30.
- **5.000**: 16 q hoje; sem problema.
- **Depende de**: O30 F2.
- **RED/selo**: o numero do card == o numero do painel (a divergencia "12 contra 11" de 20/09 e o
  RED historico); nenhuma linha sem data.
- **Tamanho**: 0 fatias proprias.

### 11. Extrato parcial do mes — **REESCREVER** (e o caso mais grave) e **FUNDIR em #5**
- **Autoridade**: para de chamar `motor.calcular_mes` por colaborador em request. Passa a ler
  **`fechamento_lido(colab, mes, ano)`** (O48 passo 2, funcao pura) por lote, e o previsto do
  **juiz** — nunca o cursor dia a dia de `views.py:470-476`. Mata os dois pendentes de dinheiro
  `core/juizes.py:626` e `:628`.
- **Tela**: o rotulo "Extrato parcial do mes" vira **"Horas ate hoje (competencia aberta)"** e a
  data de corte ("ate 26/09") fica no cabecalho, nao escondida no contexto. Mesmas colunas de #5,
  com a marca "parcial".
- **Export**: CSV igual; nada de PDF novo.
- **5.000**: medido **110–135 q e 126–147 ms por colaborador** -> **~675.000 queries e ~12 minutos**.
  Sem cache declarado isso **nao existe** em 5.000 — e exatamente o passo 6 do O48 ("leitores de
  FROTA, inviavel online, cache com invalidacao no molde do `previsto_em`/`invalidar_previsto`").
  Enquanto o cache nao existir: **job assincrono** e a tela le o resultado.
- **Depende de**: **O48 / O54-E5 (FECHAMENTO-ONLINE)** passos 2, 3 e 6.
- **RED/selo**: col37 09/2026 (153h56) e col39 (saida antecipada 5h) — os REDs que a propria O48
  declara — batendo entre tela, `fechamento_lido` e TXT; e queries por requisicao **< 50** com 750
  colaboradores (medido hoje: ~101.000).
- **Tamanho**: 3 fatias, e **nenhuma antes da porta da E5**.

### 12. Beneficios consolidados — **APOSENTAR** (fossil medido)
- **Autoridade**: nenhuma a ganhar. `Beneficio` tem **4 linhas** no banco e a ultima competencia e
  05/2026; a tela de 09/2026 abre **vazia**.
- **Tela**: card fora do index. Se VA/VT voltar a ser assunto, nasce como **cadastro com leitor**
  (LEI-AKITA 12), nao como relatorio sobre tabela vazia.
- **Export**: o CSV morre com o card.
- **5.000**: nao se aplica.
- **Depende de**: nada.
- **RED/selo**: `Beneficio.objects.count()` colado no commit; rota fora do `urls.py`; pendente
  `core/juizes.py:618` removido do registro.
- **Tamanho**: 1 fatia (junto com #2 e #3, uma fatia so de "index enxuto").

### 13. Gerador de relatorio (builder) — **MANTER** (e o unico que ja escala)
- **Autoridade**: nao responde pergunta de dia/hora/soma — fica fora da L-002. Excecao:
  `_acc_escala` (`views.py:1196`) decide "qual escala vale" com `try/except Exception: pass` mudo (`:1201-1202`); passa a ler o
  vinculo do dia pelo juiz de escala.
- **Tela**: troca o filtro proprio (empresa/praca/situacao/`so_operacao`) pelo **filtro universal**,
  e ganha a coluna "Competencia" so quando o campo escolhido a exigir.
- **Export**: CSV + PDF ja existem; o botao PDF passa a ser o padrao `core/_btn_pdf.html` (hoje
  `builder.html` **nao inclui** o partial — L9).
- **5.000**: **6 q / 0,102 s** para as 870 pessoas (medido). Basta manter o `prefetch_related` e
  paginar a previa.
- **Depende de**: nada.
- **RED/selo**: selo do L9 com allowlist vazia passa a cobrir `builder.html`; e queries **<= 10**
  com 5.000 linhas.
- **Tamanho**: 1 fatia.

### 14. Historico de chamados — **MANTER** (defesa documental; nao mexer no conteudo)
- **Autoridade**: e exibicao de trilha, nao juizo — mas o filtro "Status" usa literais
  (`status_local=...`, `views.py:701`) em vez dos conjuntos do motor
  (`chamados/catalogo/motor.py`: VIVOS/COBRANCA/TERMINAIS/NAO_FECHADOS, que a S5 proibiu escrever
  a mao). Passa a oferecer **"Vivos" / "Encerrados"** vindos do motor.
- **Tela**: o campo de busca de gente vira o **filtro universal**; o resto fica igual (e prova).
- **Export**: PDF auditavel ja existe (`views.py:827`) — **3 registros em toda a historia**; manter
  e deixar visivel, porque e ele que serve em audiencia.
- **5.000**: irrelevante (e por colaborador): 7 q / 0,417 s para o colab com **1.148 chamados**.
- **Depende de**: nada.
- **RED/selo**: o filtro "Vivos" devolve exatamente `NAO_FECHADOS` do motor (nenhuma tupla literal
  na view).
- **Tamanho**: 1 fatia.

### 15. Log de Auditoria — **MANTER como esta**
- E a trilha crua (`core/views.py:688`), so superuser (`ver_saude_sistema`). Nao deve ler juiz
  nenhum: se ela resumir, deixa de ser trilha.
- **Tela/export/escala**: sem mudanca. 5 q / 0,257 s sobre 562.038 linhas.
- **Nao depende de obra nenhuma.**

### B-extra. `pendencias/cadastro/` e `pendencias/espelho/` (sem card)
- **`pendencias/espelho/`**: **FUNDIR em #1** (mesma pergunta, mesma fonte `ranking_colabs`).
- **`pendencias/cadastro/`**: **MANTER e ganhar card** — e a unica tela que diz "CADASTRO x
  REALIDADE" com receita de acao (`relatorios/pendencias.py:7-14` RECEITAS), que e justamente o
  corte de cobranca por excecao do C. Ganha `@login_required` e `ver_relatorios`.

---

## C. CORTES JA DADOS — APLICADOS, NAO REABERTOS

1. **"Quem cobrar" (#2) = APOSENTAR.** A cobranca passa a ser **por excecao**: o juiz acende a
   lampada, as **Pautas por posto** levam ao supervisor, e a tela de **CADASTRO x REALIDADE**
   (`pendencias/cadastro/`) resolve quem o sistema nao consegue apurar. Ranking de % sai.
2. **"Fila do flip E/S" (#3) = APOSENTAR na porta da E3.** O juiz decide pelo **marco**, nao pelo
   tipo gravado; fila humana de flip nao tem objeto depois da E3.
3. **Botao PDF padrao unico (L9, `app/docs/LEIS-UI.md:95`).** Um partial (`core/_btn_pdf.html`),
   rotulo sempre "PDF". Hoje 5 templates de `relatorios/` o usam e **`builder.html` nao** — e o
   `builder` exporta PDF (`views.py:1319`). Nenhum rotulo novo; quem tiver dois PDFs usa
   `core/_quadro_pdf.html`.
4. **Filtro universal de gente (L6, `LEIS-UI.md:68`), periodo livre, cartao = espelho.** O
   aplicador e `core/filtro.py:159`; hoje ele tem **um** consumidor no sistema
   (`holerite/_drawer_celula.html`) e **zero** em `relatorios/`. Todo card de lista que sobreviver
   adota o componente; nenhum reimplementa busca. Competencia deixa de ser combo mes/ano: **periodo
   livre** + a janela da empresa (`janela_atual`/`corte_da_empresa`).
5. **"Atestados LOTE" (O30 F3) absorve o que se sobrepoe**: o card #8 inteiro, a parte de
   documentos do #7 e a linha do tempo que o #10 hoje resume. Uma funcao monta a linha do tempo;
   nenhum calculo proprio de dias ou efeito.
6. **O24 (FECHAMENTO-UI-PORTAS) esta SUPERADO pelo O39** (`app/docs/BACKLOG.md:56`) — a dependencia
   dos cards #5/#6/#11 se escreve como **O39 + O48/O54-E5**, nao como O24.

---

## D. MAPA — card x destino x dependencia x ordem

| # | Card | Destino | Depende de | Ordem | Fatias |
|---|---|---|---|---|---|
| 12 | Beneficios consolidados | **APOSENTAR** (tabela com 4 linhas) | — | **1** | 1 (uma fatia so com #2 e #3: "index enxuto") |
| 2 | Quem cobrar | **APOSENTAR** (corte) | Pautas por posto · `pendencias/*` em #1 | **1** | 1 |
| 3 | Fila do flip E/S | **APOSENTAR** na porta da E3 | O54-E3 | **2** | 1 |
| 5 | Horas por colaborador | **FUNDIR** -> "Horas da competencia" (recebe #6 e #11) | O48/O54-E5 passos 2-3 · O39 | **3** | 2 |
| 6 | Inconsistencias | **FUNDIR em #5** | #5 · O54-E2 | **3** | 1 (dentro de #5) |
| 9 | Ferias vencendo | **MANTER** + tirar regra do template + matar N+1 | — | **3** (independente, pode ir junto) | 1 |
| 13 | Builder | **MANTER** + botao PDF padrao + filtro universal | — | **3** | 1 |
| 1 | Auditoria de espelhos | **FUNDIR** (recebe #2 e `pendencias/espelho/`) + lote + paginacao | O54-E2/E3 | **4** | 3 |
| — | `pendencias/cadastro/` | **MANTER e ganhar card** (CADASTRO x REALIDADE) | #1 | **4** | 1 |
| 7 | Absenteismo mensal | **REESCREVER** (le `cobertura_ausencia_periodo`) e depois fundir | O30 F2/F3 | **5** | 1 |
| 8 | Atestados acumulados | **FUNDIR na O30 F3** | O30 F3 | **5** | 0 (item da O30) |
| 10 | Afastados | **FUNDIR na O30 F2** | O30 F2 | **5** | 0 (item da O30) |
| 4 | Cartoes-ponto em lote | **MANTER, reescrever por dentro** (previa honesta · job · filtro) | O54-E2/E4 | **6** | 3 |
| 11 | Extrato parcial | **REESCREVER** (fechamento lido + cache/job) e fundir em #5 | **O48/O54-E5 passos 2,3,6** | **7** | 3 |
| 14 | Historico de chamados | **MANTER** + status pelo motor + filtro universal | — | **8** | 1 |
| 15 | Log de Auditoria | **MANTER como esta** | — | — | 0 |

**Contagem fechada, cada card em UM balde (15 = 15)**:
- **3 APOSENTADOS**: #2 Quem cobrar · #3 Fila do flip · #12 Beneficios.
- **4 ABSORVIDOS** (deixam de ser card, o conteudo vive noutro lugar): #6 e #11 -> dentro de #5 ·
  #8 e #10 -> dentro da O30 (F3 e F2).
- **2 RECEPTORES** (continuam card e engordam): #1 (recebe #2 e `pendencias/espelho/`) ·
  #5 (recebe #6 e #11).
- **1 REESCRITO e depois absorvido pela O30**: #7 Absenteismo.
- **5 MANTIDOS**: #4 · #9 · #13 · #14 · #15.
- **+1 CARD NOVO**: `pendencias/cadastro/` (CADASTRO x REALIDADE), que hoje existe como rota sem card.

**Index**: 15 -> **8 cards** (#1, #4, #5, #9, #13, #14, #15 + `pendencias/cadastro/`). #7 nao conta
como card porque termina dentro do atalho "Atestados LOTE" da O30.

### D1. Links e menus a redirecionar quando um card sair

| Sai | Quem aponta hoje (arquivo:linha) | Para onde vai |
|---|---|---|
| #2 `relatorios:ranking_furos` | `templates/relatorios/index.html:21` · **`templates/ponto/fechamento.html:75`** ("Quem cobrar", a porta que o DP usa) | `relatorios:furos_espelho?empresa_id=<id>` (card #1) |
| #3 `ponto:flip_fila` | `templates/relatorios/index.html:30` · `templates/ponto/flip_fila.html:16` (`volta=`) | o dia do colaborador (espelho/calendario); a rota sai do `ponto/urls.py:51` |
| #12 `relatorios:beneficios_consolidado` | `templates/relatorios/index.html:116` | sem substituto (o assunto volta como cadastro, se voltar) |
| #6 `relatorios:inconsistencias` | `templates/relatorios/index.html:56` | `relatorios:horas_mes?pendencia=1` (filtro dentro de #5) |
| #8 `relatorios:atestados` | `templates/relatorios/index.html:76` | drawer "Atestados LOTE" (O30 F3) |
| #10 `relatorios:afastados` | `templates/relatorios/index.html:96` | lista de gente com filtro "Afastados hoje" (O30 F2) |
| #11 `relatorios:extrato_parcial` | `templates/relatorios/index.html:106` | `relatorios:horas_mes` (competencia aberta, marca "parcial") |
| `espelho/dinamico/` | **ninguem** (`grep` em `templates/` = 0) | remover a rota (`relatorios/urls.py:34`) |

> Guarda: nenhuma rota sai sem que `grep -rn "<nome_da_rota>" app/templates app/*/views.py` volte 0.
> Foi assim que remover `bin/hasner-integrador-off.service` doeu em 25/09 — arquivo que **prod
> usava** citado noutro lugar.

---

## E. OS 5 ACHADOS QUE MAIS PESAM

1. **13 dos 15 cards tiveram ZERO acesso em 8 dias.** Em 19–26/09 o `access-ui.log` registra
   trafego em **2** cards: Cartoes em lote (44 GET + 77 POST) e Log de Auditoria (1). A pagina
   `/relatorios/` em si teve 50 acessos — **gente entrou no indice 50 vezes e clicou em quase
   nada**. Zero em: Auditoria de espelhos, Quem cobrar, Fila do flip, Horas, Inconsistencias,
   Absenteismo, Atestados, Ferias vencendo, Afastados, Extrato parcial, Beneficios, Builder,
   Historico de chamados (13). A janela e de 8 dias porque o log rotaciona; para o card usado ha
   trilha em banco que confirma (74 lotes em 8 dias, 75 desde 17/09).
2. **Beneficios consolidados e um fossil medido**: `Beneficio` = **4 linhas**, ultima competencia
   05/2026, **0** em 09/2026. A tela do card abre vazia hoje e sempre.
3. **Extrato parcial custa 110–135 queries e 126–147 ms POR COLABORADOR** (medido em 21 e 27
   pessoas) porque chama `motor.calcular_mes` dentro de `for colab in qs_colab`
   (`views.py:456`, `:489`) -> **~675 mil queries e ~12 min em 5.000**. E o unico card que faz o
   MOTOR trabalhar em request, e **nao tem nenhum teste**.
4. **O card mais usado da casa e o menos auditavel, e nao e por recusa**: das 75 geracoes de lote
   (todas desde 17/09), **71 foram por PERIODO LIVRE**, caminho que `services.py:174` manda direto
   ao informacional sem nem tentar o hash CLT — **1.709 de 1.720 espelhos**. Por competencia foram
   4 lotes: 11 espelhos, **4 com hash**. Total: **1.716 de 1.720 (99,8%) sem hash**. Cura dividida:
   E2 para os 7 que o auditavel recusou, TELA para os 1.709 que o admin pediu com datas livres.
5. **O N+1 de tres cards e uma linha de `__str__`**: `Posto.__str__`
   (`colaboradores/models.py:187`) monta `f"{self.nome} — {self.empresa}"`, e `horas_mes` (596 q),
   `inconsistencias` (425 q) e `ferias_vencendo` (49 q) nao trazem `posto__empresa` no
   `select_related`. Uma linha por view.

**Extras que o censo achou e que nao estavam na pergunta**: (a) **4 views usam `is_staff` em vez do
RBAC `ver_relatorios`** e **3 delas nao tem `@login_required`** (`views.py:1420`,
`views_pend.py:75`, `views_pend.py:88`); (b) o **filtro universal (L6)** tem **1 consumidor** no
sistema e **0** em `relatorios/`; (c) **5 cards nao tem selo nenhum** (#3, #6, #11, #12, #13);
(d) as rotas **mais** usadas do app em 8 dias sao os dois autocompletes (`colab-search` 151,
`filtro-search` 75), que servem o card #4.

---

*Medido em 26/09/2026 contra prod em leitura, `saas_ui`, rollback provado. Onde nao ha numero, esta
escrito "nao medido".*
