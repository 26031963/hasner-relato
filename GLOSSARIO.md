# GLOSSARIO — hasner-ponto

_O98 (glossario do dominio). Arquivo criado em 27/10 pelo aval `GLOSSARIO-TERMOS-TECNICOS`: so documento,
nao toca nucleo; nenhuma lei nova. O nome da casa e ALIAS COSMETICO do termo tecnico; quando os dois
divergirem, vale o termo tecnico e a lei citada._

## Termo tecnico <- alias da casa

| termo tecnico | alias da casa | onde mora no codigo |
|---|---|---|
| Single Source of Truth / Aggregate Root | celula soberana / fonte unica (alias usado nos cortes do Ronald FIO-LE-O-JUIZ 27/10 17:3x e SMOKE-FIO-NO-AR-ANTES 23:3x; o mesmo padrao, nao padrao novo) | `escala/models.py::CelulaDia`; escritor `ponto/portas/celula.py::gerar_celulas_periodo` (excecao formal `regenerar_celulas_vinculo`) |
| Single Writer Principle | juiz unico / escritor unico | `core/juizes.py` (tabela em `app/docs/JUIZES.md`); batida: `ponto/registro_batida.py::registrar_batida` |
| sinal derivado do estado, reconciliacao level-triggered (o oposto, edge-triggered, e o fio morto) | lampada | `ponto/services/cartorio.py::lampadas_do_dia`, `ponto/services/lastro.py::marcos_acesos`; o fio morto: `chamados/management/commands/vigiar_fio_morto.py` |
| Architectural Invariants + Fitness Functions | contratos estruturais | `core/contratos_estruturais.py::total` |
| Architecture Conformance Test (armadilha: selar a FORMA e nao o SIGNIFICADO) | selo | `bin/tests/test_*.sh` (host) e `*/tests/test_selo_*.py`; selo por AST, nunca por texto |
| Event-driven architecture | crons virando evento | `config/crons.py::JUIZES_POR_VARREDURA` (o que ainda e varredura); signals do cartorio `ponto/signals.py` |
| Transactional Outbox | O-OUTBOX | a construir (`app/docs/PLANO-PADROES.md`) |
| Parallel Change / Expand-Contract | EXPAND / BACKFILL / migrar leitores / CONTRACT (aval E4-EXPAND-CONTRACT; corte Ronald 09/11 21:2x, L-127) | E4a-1 (fase EXPAND): `chamados/services/fabrica_da_celula.py::pauta_cadastro_sem_turno` grava o fato novo ao lado; nenhum leitor muda de fonte ate a fase de leitores; CONTRACT so depois (E4b) |
| Clock Injection | hoje/agora da borda (relogio lido UMA vez na borda e passado; corte Ronald 09/11 21:2x, L-128) | `chamados/services/fabrica_da_celula.py::perguntar_pela_celula(cel, hoje=, agora=)`, `chamados/services/vigia_fio.py::censo(hoje=)`; selos `chamados/tests/test_e4a_relogio_injetado.py`, `core/tests/test_selo_teste_sem_relogio.py` |
| Strangler Fig Pattern | construir ao lado e trocar inteira | — (conduta: raia/copia, L-107; troca de motor, L-110) |
| Shadow mode / Parallel run | sombra | `bin/sombra.sh`; settings `config/settings/sombra.py` |
| Merge queue | comboio / pouso | — (conduta: CLAUDE.md 7b POUSO-EM-COMBOIO, L-105/L-107); `bin/deploy.sh` |
| Backfill / Reprocessing | rejulgar o passivo | `ponto/services/cartorio.py::julgar_celula` / `julgar_colab` com `forcar=True` |
| CQRS com projecoes reconstruiveis | espelho, DiaPago, fechamento, retrato e pilulas | `ponto/services/espelho.py::espelho_do_colab`, `ponto/models.py::DiaPago`, `ponto/models.py::FechamentoMensal`; reconstrucao: L-121 / O-PROJECOES |
| Duplicated business logic / logic drift | derivacao paralela | — (proibida pela LEI-AKITA 2; caca por selo de AST) |
| Bitemporal data | vinculo do dia x vinculo de hoje | `escala/alimentacao.py::vinculo_do_dia` / `vinculo_do_dia_carregado`; passo 2: O-BITEMPORAL-2 |
| Immutable event log + compensating event | batida so retratada | `ponto/models.py::Batida.retratada_em`; universo `ponto/turnos.py::batidas_apuraveis` |
| ADR (Architecture Decision Record) | LEIS / CORTES | `app/docs/LEIS.md`, `app/docs/CORTES.json` (formato ADR para lei nova: L-122) |
| idempotencia pela chave do fato | dedup do fato | `chamados/models.py::ChamadoColaborador.abrir` (dedup + nascimento serializados por colaborador) |
| Identity Map | a instancia pedida / memo por identidade | `chamados/juizes.py::um_colaborador_por_identidade` (o memo da tranca mora no dict da rodada de `chamados/juizes.py::tranca_em_lote` -- SIM do toque + usuario de sistema, SNC2 05/11; sitio `chamados/management/commands/vigiar_fio_morto.py` abre `with tranca_em_lote():`); sitios: `chamados/services/pergunta_no_app.py::perguntas_do_app` / `_marco_ja_perguntado` / `medir`, `chamados/juizes.py::alimentar_portas` / `_no_app`, `chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula`, `chamados/services/vigia_fio.py::viva_em_disputa_fechada` / `carimbos_sem_lastro_por_dono` / `censo_alcance` / `censo`, `chamados/juizes.py::alimentar_portas(celula_ids, colabs)` (a semente entra ANTES do laco das perguntas; quem passa: `folha/fora_da_folha.py::_alimentar` com os retidos de `fora_da_folha` e o colaborador de `o_que_segura`, e `censo_alcance` com o in_bulk do universo, que sai de `dias_que_seguram` sem alimentar: a porta e alimentada UMA vez por rodada -- N1-ALIMENTACAO-UNICA 05/11), `colaboradores/services/calendario.py::contexto_calendario`, `chamados/services/fio.py::migrar_resolvedoras_vivas`, `chamados/relatorios_disputa.py::coletar`, `chamados/juizes.py::_dias_das_perguntas_vivas`, `chamados/services/competencia_trancada.py::medir`, `chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido`, `chamados/reconciliador.py::encerrar_orfas_pela_lampada` / `orfas_pela_lampada_sem_evento` / `semear_eventos_celula` / `reconciliar_perguntas_orfas_colab`, `chamados/models.py::ChamadoColaborador._vivas_entre` / `_universo_da_ressuscitacao`, `reabrir_orfas_enterradas`; aval Ronald N1-IDENTITY-MAP 04/11 18:0x. CELULA DO ATO (LAVRAR-HE-PENDENTE-ESCALA 04/11): `escala/alimentacao.py::celulas_do_ato` + `escala/models.py::EscalaColaborador._celulas_do_ato_para` (os 4 leitores da celula do dia sem `celulas=` leem o dict do ato), ligado em `ponto/services/fechamento.py::apurar_por_fatia` |
| Offline Fallback (network-first: sem rede, a copia do cache; sem copia, o erro de rede de sempre -- nunca `undefined` no `respondWith`) | fallback do SW | `static/sw.js::doCacheOuErroDeRede`; selo `core/tests/test_sw_offline_fallback.py` |
