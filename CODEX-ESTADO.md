# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 16:57** · publicado em **27/11/2026 16:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 16:57 · sessao f39976049fa4 · FERIADO-UM-JUIZ · construtor · origem=Code

Implementei a c5 na cópia. O controle passou com **40 testes sem banco e 41 estruturais**. Os 10 mutantes da c5 passaram no controle e falharam como previsto quando aplicada a mutação. `ponto/tests/feriado_um_juiz_mutantes.py:47`, `:283`, `:337`

- **Achado 6 — selos.** Atualizei os cinco endereços da catraca e comparei as expressões B1 com as registradas na c3. A partição agora grava o dia corrigido no mapa e o relê com `_dia_de` antes de consultar a certeza da escala e a cláusula da rubrica. Carreguei as fontes da c4 em memória e reproduzi as três falhas por asserção. Os três selos passaram na c5. `core/tests/test_medida_turno_do_par.py:289`, `:329`, `:565`, `ponto/calculador/regras.py:425`, `ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:58`
- **Achado 4 — competência e ciclo.** A borda consulta o juiz do feriado nos dois dias do corte. O RED mostrou que a c4 pôs as 4 h comuns do dia 21 na competência 11. Na c5, a 11 recebe as 4 h de feriado do dia 20 e a 12 recebe as 4 h comuns do dia 21. Na troca 6x1 → 12x36, o RED mostrou 4 h indevidas de HE100 feriado na véspera. Na c5, cada dia recebe 4 h comuns. Os dois casos rodaram com as mesmas batidas na c4 e na c5. `ponto/services/fechamento.py:155`, `ponto/calculador/regras.py:410`, `ponto/tests/test_feriado_um_juiz.py:607`, `:618`
- **Achado 5 — três gravações.** Um `atomic` por colaborador cobre a criação ou atualização do mensal e as duas lavras. As capturas existentes marcam rollback. Em memória, a falha injetada na segunda lavra deixou mensal e primeira lavra atualizados na c4. Na c5, os três voltaram aos valores anteriores. A falha na primeira lavra também foi testada. A competência sem pares consegue gravar o ajuste do oráculo com rubricas vazias. **`except` no diff lógico: 0 adicionados e 0 removidos**, conferidos por AST e `git diff -w`. `ponto/services/fechamento.py:928`, `:1276`, `:1340`, `ponto/calculador/alimentacao.py:360`, `ponto/services/dia_pago.py:311`, `ponto/tests/test_feriado_um_juiz.py:43`, `:147`

**Comandos em `app/`** — `base_c4` reproduz quatro falhas por asserção. O executor retorna `0` quando encontra o resultado esperado. `ponto/tests/feriado_um_juiz_mutantes.py:337`

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes "$1"
}

prova base_c4
prova controle
prova estruturais

prova c5_endereco_folha_antigo
prova c5_dia_sem_leitura_da_lavra

prova c5_so_feriado_depois_do_corte
prova c5_dobra_pela_fatia_de_origem
prova c5_12x36_lavrado_na_vespera

prova c5_gravacoes_sem_atomic_comum
prova c5_motor_sem_sinal_de_rollback
prova c5_oraculo_sem_sinal_de_rollback
prova c5_exige_mapa_sem_pares
prova c5_recusa_lavra_sem_rubricas
```

**Recorte para a sessão na VM.** O teste Django cobre mensal aberto e aprovado, com falha no `bulk_create` do oráculo. Execução com banco, recorte na VM, suíte inteira e parecer do Opus [nao verificado]. `ponto/tests/test_feriado_um_juiz_porta.py:462`, `docs/RELATO.md:1841`, `docs/LEIS.md:147`

```bash
bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto core folha relatorios"
```

## 27/11 16:57 · sessao f92e1d4a9b06 · - · - · origem=Ronald (tmux)

**Mapa.** Censo estático confirmou 20 chamadas a `criar`, incluindo um smoke manual, e 18 a `abrir`. O selo de escritores percorreu 774 arquivos em memória, com quatro verificações aprovadas e nenhum escritor de `status_local` em runtime fora das portas declaradas. O selo exclui testes e migrações. [test_selo_status_so_pelas_portas.py](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_selo_status_so_pelas_portas.py:340)

- `criar(cls, *, colaborador, status='aberto', **campos)` insere sempre que passa motor, absolvição pela célula, veto e urgência. Quando `status='aberto'`, o motor pode escolher `registrado`. Sem dedup nem reabertura. Recusas preexistentes devolvem `None`. `app/chamados/models.py:1120-1166`.
- `abrir(cls, *, colaborador, defaults=None, **lookup)` serializa pelo colaborador, procura correspondente, tenta renascer se encerrado e devolve `(existente, False)` mesmo quando recusou renascer. Procura filho agrupado sob pai vivo. Se chegar ao nascimento passa motor, célula e veto, grava via `get_or_create` e devolve `(ch, criado)`. Recusas preexistentes de nascimento devolvem `(None, False)`. `app/chamados/models.py:899,968-1047`.
- Fora dos dois `INSERT`s (`app/chamados/models.py:1037,1161`), `_gravar_estado` é o escritor runtime da coluna (`:213,240,304-308`), chamado por `_aplicar` (`:469`), `reconciliar` (`:674`), `cancelar` (`:854`), `analisar` (`:872`), `reabrir` (`:1508`), `encerrar` (`:1793`) e `encerrar_por_competencia_trancada` (`:1813`). `:375,390` são simulação em memória. Migrações históricas alteram estado em `app/chamados/migrations/0038_a3_registrado_backfill.py:10,15` e `0039_a3_via_backfill.py:24,33`.
- Reabertura fora de nascer/renascer existe por `reabrir`, que leva `fechado`/`resolvido` a `em_analise`, e pelo POST humano → `transicionar_com_pauta` → `transicionar` → `_aplicar`. No segundo, o motor permite ao `admin_ti` sair de `fechado` inclusive para `aberto`, `em_analise` ou `registrado`, todos `VIVOS`. As telas do detalhe, fio e modal ocultam a troca usual de status em `fechado`, mas a aceitação no backend está no motor. `app/chamados/models.py:140,211,466-469,1428,1505-1518`; `app/chamados/services/fecho_runtime.py:175,181`; `app/chamados/catalogo/motor.py:24,98-106,113,139-156`; `app/templates/chamados/detalhe_local.html:375`, `fio_colaborador.html:254`, `partials/modal_fio.html:682`.

**Chamadores de `criar`.**

| Sítio | Módulo e origem |
|---|---|
| `app/api/views.py:2189` | `app_colaborador`, solicitação humana |
| `app/chamados/services/abertura.py:47` | `modulo`, humano, GET com default `chamados` e catálogo pode sobrescrever (`app/chamados/views.py:150,197`, `abertura.py:34`) |
| `app/chamados/services/abertura.py:172` | `modulo`, humano DP, `tipo` do formulário ou `mensagem_dp` default do helper (`app/chamados/views.py:136-140`, `abertura.py:124`) |
| `app/colaboradores/views.py:2026,2412` | `beneficio`, `agenda_dia`, ações humanas |
| `app/chamados/services/regularizacao_ext.py:247` | `regularizacao_externa`, ação humana (`app/colaboradores/views.py:3032`, `app/chamados/views_regularizacao.py:54` via `regularizacao_ext.py:319`) |
| `app/colaboradores/services/vinculo.py:412` | `escala_colaborador`, oferta de aceite em ação humana (`:388`) |
| `app/ponto/utils.py:40,103` | `colaborador_sem_posto`, `colaborador_em_ferias`, automatismos de ponto |
| `app/ponto/views.py:310` | `entrada_bloqueada_turno_aberto`, automatismo da batida |
| `app/api/views.py:720`, `app/api/views_core.py:714` | `batida_fora_escala_leve`, automatismo da batida |
| `app/ponto/services/triagem_batida.py:107,336,459` | `acesso_bloqueado`, `batida_fora_horario` nos dois últimos, automatismos da batida |
| `app/ponto/services/geofence.py:361` | `gps_ausente` ou `gps_fora_geofence` (`:344`), automatismo da batida |
| `app/ponto/services/justificativa.py:411` | `justificativa`, efeito de rejeição humana DP (`app/ponto/views.py:1956`) |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `turno_aberto_24h`, cron (`app/config/crons.py:363`) |
| `app/colaboradores/services/desligamento.py:195` | `desligamento_revisao`, efeito de UI ou cron |
| `app/chamados/management/commands/smoke_chamados.py:124` | `smoke`, diagnóstico manual (`app/config/crons.py:1460`) |

**Chamadores de `abrir`.**

| Sítio | Módulo e origem |
|---|---|
| `app/chamados/views.py:974` | `mensagem_dp`, humano |
| `app/api/views.py:1985` | `justificativa_colab`, wrapper da solicitação humana |
| `app/ponto/services/ausencia.py:805` | `ausencia_colab`, efeito da solicitação humana (`app/api/views.py:1861`, `app/ponto/views.py:2718`) |
| `app/chamados/services/disputa_emissao.py:582` | `disputa_supervisao`, misto. Declaração humana `:620,641` e sistema `:648,676`, chamado por APIs de batida, fábrica e crons (`app/api/views.py:768`, `app/api/views_core.py:762`, `app/chamados/services/fabrica_da_celula.py:172`, `app/ponto/management/commands/reconciliar_grade.py:424`) |
| `app/chamados/services/disputa_emissao.py:1118,1353` | `disputa_supervisao_manual`. Órfãos misto (`app/chamados/services/acoes_disputa.py:261`, `app/chamados/services/cobrar_dia.py:49`, `app/chamados/services/pergunta_no_app.py:263`, crons `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:803`); âncora humana (`acoes_disputa.py:228`) |
| `app/chamados/services/adesao.py:75` | `sem_previsao_escala`, sistema (`app/escala/management/commands/apurar_furos_diarios.py:17`, `app/config/crons.py:1536`) |
| `app/ponto/views.py:3191` | `ping_geo_fora`, automatismo do ping |
| `app/escala/regua_defesa.py:51` | `regua_degenerada`, crons `app/ponto/management/commands/detectar_ausencias.py:601`, `reconciliar_grade.py:495` |
| `app/ponto/services/cluster_espurio.py:196` | `cluster_batida_espuria` (`:11`), cron `app/config/crons.py:598` |
| `app/ponto/management/commands/detectar_ausencias.py:191,511,272,694` | `sem_previsao_escala` nos dois primeiros, `ancora_limbo`, `batida_ausente` (`:29`), cron `app/config/crons.py:364` |
| `app/ponto/management/commands/reconciliar_grade.py:507` | `batida_ausente` (`:54`), cron `app/config/crons.py:462` |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | `turno_aberto`, cron `app/config/crons.py:362` |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | `turno_aberto_24h`, comando retroativo |
| `app/chamados/reconciliador.py:1032` | `batida_ausente` (`:957`), reatar órfã automático (`:925,1776`) |

**Juiz e predicados.** `esta_desligado(colaborador)` compara somente `getattr(colaborador, 'situacao', None) == 'desligado'`. Situação do objeto, sem relógio, data ou vínculo. Agendamento muda data e motivo, efetivação grava situação, reativação muda para ativo e limpa a data. `app/colaboradores/services/desligamento.py:23-32,107-133,235-247`; `app/core/juizes.py:283`.

- `_renascer_por_premissa_viva` barra `situacao != 'ativo'`, incluindo afastado e férias, e barra `via_encerramento == VIA_CASCADE`, um fato do chamado que pode sobreviver à readmissão. Adesão compara a situação exata em `:67`, mas `:91` testa via de encerramento. `app/chamados/models.py:1070-1085`; `app/chamados/services/adesao.py:67,91-93`. Reemissão do reconciliador usa a versão mais ampla `!= 'ativo'`. `app/chamados/reconciliador.py:942`.
- `recusa_da_reabertura` testa veto de dia, admin sem fato novo, premissa regularizada, agrupamento, irmão e motor, sem predicado de desligado ou teste direto de `VIA_CASCADE`. O veto testa módulo, dia e vínculo do emissor. `app/chamados/models.py:1192-1211,1384-1426`.
- Estado exato, com filtros locais adicionais, aparece no Cartório `app/ponto/services/cartorio.py:1803,1868`, furos vetados `app/ponto/services/furos_vetados.py:62` e I5a `app/chamados/management/commands/auditar_invariantes_chamados.py:659-670`. O I5a conta `COBRANCA`, que exclui `registrado`, e roda pelo auditor agendado às 07:18. `app/chamados/catalogo/motor.py:24-32`; `app/config/crons.py:542`.
- Cron agendado usa data preenchida `<= hoje` mais situação diferente. Cadastro do dia no Supra Juiz usa `dia > data_demissao` quando `vinculo_do_dia` retornou `None`. Elegibilidade de turno usa data nula ou posterior ao fim. Rescisão do TXT usa data `<= fim_janela` e retenção/informativo leem o juiz canônico. São decisões com universos distintos. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-40`; `app/ponto/supra_juiz.py:170-175,184-210`; `app/ponto/utils.py:177-180`; `app/folha/export.py:54-60,1215-1237`.

**Reabertura humana e trilha.**

- S93 admin sai do fio e valida POST `motivo_reabertura` e `justificativa_admin` de pelo menos 20 caracteres. Reabre a disputa, cria pergunta de conversa e então chama `ch.reabrir(humano=True)` sem entregar autor ou motivo e sem conferir o retorno. A nota recebe `request.user` e o log da disputa usa `registrar_log(request, ...)`. `app/chamados/views.py:675,955`; `app/chamados/services/fio.py:717-719`; `app/chamados/services/acoes_disputa.py:274-315,338`; `app/core/auditoria.py:8-25`.
- Desarquivar usa `reabrir(humano=True)` quando a via é tranca e registra o ator em resgate/ocorrência, sem campo de motivo. Reabrir período exige POST `motivo`, grava `PeriodoFechado.reaberto_motivo` e autor, mas o chamado recebe só `humano=True` e a trilha do lote passa o ator. `app/chamados/views_arquivar.py:52-88`; `app/ponto/views.py:1548-1575`; `app/ponto/services/fechamento.py:1456-1472`; `app/chamados/services/competencia_trancada.py:264,275,294-300`.
- O detalhe despacha `novo_status` mesmo sem `conteudo`. No fio, o `conteudo` é nota de resposta, exigida neste ramo. O `transicionar` grava `respondido_por` e o evento `estado_chamado` com o usuário. A própria `reabrir` não recebe motivo nem usuário e `_gravar_estado` é chamado sem `user`; em request o escritor do log capta `request.user`. `app/chamados/views.py:548`; `app/chamados/services/acoes_chamado.py:292-301,322-331`; `app/chamados/services/fio.py:862-876`; `app/chamados/models.py:242-244,304-308,469,1428,1508`; `app/core/observ.py:94-138`.
- Recusa cabe no `LogAuditoria`, JSON com porta, colaborador e módulo, por `core.observ.evento`. Já há ações de recusa de nascer/reabrir para outros motivos, com trio equivalente e dedup de ciclo, no veto de dia. `FollowupChamado` exige um chamado e serve apenas quando já existe objeto. `app/core/models.py:285-307`; `app/core/observ.py:94-138`; `app/chamados/models.py:1221-1254,1842`; `app/chamados/services/nota.py:75-89`.
- O retorno de recusa precisa ser consumido pelos chamadores. A abertura humana e criadores de disputa já desreferenciam ou registram followup sobre o resultado sem testar `None`. `app/chamados/views.py:141-143,196-210`; `app/chamados/services/abertura.py:194-202`; `app/chamados/services/disputa_emissao.py:598-606,1134-1142,1369-1376`; `app/chamados/services/nota.py:87-107`.

## 27/11 16:55 · sessao e83cdc28581f · - · - · origem=Ronald (tmux)

## Item 4 — pergunta respondida sem veredito

- O desligamento efetivo percorre apenas disputas abertas, pula as que têm resposta sem veredito, fecha as restantes pela porta `FECHOU_DESLIGAMENTO` e depois tenta `encerrar` cada chamado em `NAO_FECHADOS`. A disputa recusa resposta sem veredito antes de julgar as vivas, inclusive sob trava. Para as vivas que passam, a porta de desligamento compartilha com `admin_encerra` o carimbo `admin_decidiu`, a linha `DecisaoPergunta.SILENCIO` e a trilha. O signal tenta reconciliar o pai. `encerrar` ainda pode recusar pela pergunta viva ancorada. `app/colaboradores/services/desligamento.py:164`, `:182`; `app/chamados/models.py:2123`, `:2175`, `:2184`, `:2199`, `:1779`, `:429`; `app/chamados/signals.py:29`, `:43`.
- O censo de 13/11 ficou velho quanto à definição. A lista usa `DecisaoPergunta.composta`, exige **instante e texto** pelo juiz L-130, sem validação nem materialização, e exclui dia trancado salvo `ignorar_tranca=True`. A guarda do chamado busca perguntas ancoradas também em disputas fechadas, universo que o vigia conhece e que o laço atual do desligamento pula. `app/chamados/models.py:2000`, `:2020`, `:2041`, `:361`; `app/chamados/juizes.py:1644`; `app/chamados/services/vigia_fio.py:746`; `app/colaboradores/services/desligamento.py:164`.
- A via própria de desligamento precisa encerrar **respondidas e mudas**, sem validação de mérito. O registro de 13/11 já substitui expressamente o corte de 28/10 que mandava manter a respondida viva. LU-11 confirma as mudas, LU-12 diz que só o chamado reabre. O valor literal da nova via ainda não está na lista de choices, que ocupa campo de 24 caracteres. `app/docs/BACKLOG.md:732`, `:1211`; `app/docs/PROMPTS.md:2222`; `app/docs/CORTES.md:485`, `:545`, `:546`; `app/chamados/models.py:2836`, `:2872`.
- O molde é **o ramo legado** `VIA_COMPETENCIA_TRANCADA` com carimbo no degrau anterior à resposta do C6 e seu espelho SQL. A tranca atual mata por leitura e grava Pauta informativa ao DP. A via antiga continua legível e tem sua reabertura própria. `app/chamados/models.py:2835`, `:3774`, `:3388`, `:3703`; `app/chamados/juizes.py:1507`, `:1514`; `app/chamados/services/competencia_trancada.py:126`, `:133`, `:137`, `:202`, `:236`, `:241`, `:280`.
- A lista `VIAS_QUE_VENCEM_A_RESPOSTA` afeta C6 e SQL. Acrescentar apenas às identidades não vence o retorno antecipado da resposta. `carimbar` aplica o carimbo com `resposta=None` e `lavrar_veredito=False` por padrão e pode usar o C6 para uma via terminal fora de `vias_que_encerram_por_identidade`. `carimbar_identidade` exige pertinência à sua lista. O juízo de lastro é independente e, sem ramo explícito, entende escolha carimbada como fato, hora/data-hora exige prova, texto exige completude/retratação. Não confundir terminalidade com `VIAS_FORA_DO_FATO`, que rege identidade, dedup e índices. `app/chamados/juizes.py:729`, `:756`, `:758`, `:864`, `:1514`, `:1521`, `:1541`; `app/chamados/models.py:2821`, `:2835`, `:3414`, `:3434`, `:3462`, `:3484`, `:3511`, `:3544`, `:3036`, `:3067`, `:3947`, `:3953`; `app/chamados/veredito_lavrado.py:262`.
- A reabertura permitida de chamado hoje dispara `ressuscitar_perguntas`. Seu universo inclui perguntas mudas carimbadas, simula descarimbar, limpa os campos e pode chamar `DisputaSupervisao.reabrir_sistema`. Isso deve ser coberto para LU-12. `reabrir_admin` hoje pode abrir diretamente a disputa e plantar conversa antes de tentar o chamado. `app/chamados/models.py:1517`, `:1549`, `:1582`, `:1599`, `:1739`, `:1749`, `:2396`, `:2433`; `app/chamados/services/acoes_disputa.py:296`; `app/docs/CORTES.md:546`.
- Mostrar como informativa precisa contemplar `estado` (`RESOLVIDA` para morta carimbada), `estado_aplicacao_pergunta` (hora respondida carimbada cai em `aplicada` fora das duas vias da tabela atual), o card com fallback do choice e o explicador, que lê lastro e estados de recusa. A fila normal já lê C6 e SQL. `app/chamados/models.py:3361`; `app/chamados/services/materializacao.py:868`, `:890`, `:904`; `app/chamados/services/fio.py:466`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/_card_atendimento.html:195`; `app/chamados/services/explicador.py:123`, `:131`; `app/chamados/juizes.py:1863`.
- Validar e recusar têm guardas de resposta/validação e de disputa fechada quando há cartão esperado. Há validação sem cartão em API foto e comando. Aceitar folga ocorre antes do teste de `materializada_em`. Conferir que a via nova não vire decisão na rota direta. O `post_save` da disputa também agenda retomar regularizações já confirmadas, cujo materializador pode chamar `registrar_batida`. Incidência em produção [nao verificado]. `app/chamados/services/validacao.py:747`, `:756`, `:777`, `:813`, `:948`, `:980`, `:999`; `app/api/foto_recebida.py:93`; `app/chamados/management/commands/validar_classe_a.py:132`; `app/chamados/signals.py:122`, `:149`, `:252`; `app/chamados/services/regularizacao_ext.py:78`, `:219`.

## Demais ramos que leem a via

| Ramo atual | Efeito de uma via nova |
|---|---|
| Projeção e chave do memo | Memo incorpora o valor, projeção anterior ao carimbo limpa a via salvo `fora_do_fato`. `app/chamados/juizes.py:901`, `:1486`. |
| Ressuscitação, descarimbos e re-lavra | Portas gerais poupam `fora_do_fato`; uma via nova selecionada pode ser removida. Re-lavra só troca `declaracao`/`completude_turno`. `desfazer_carimbo_sem_lastro` admite candidata fora de identidades e só limpa se C6 der viva. `app/chamados/models.py:1554`, `:1678`, `:1723`, `:3570`, `:3653`, `:3659`; `app/chamados/services/disputa_emissao.py:1689`, `:1710`; `app/chamados/services/relavra_via.py:118`, `:155`, `:181`, `:242`. |
| Rótulos/escritores particulares | Admin e fato-espelho podem trocar uma via diferente. Fantasma tenta pelo C6. Nova resposta reabre apenas `VIAS_SEM_LASTRO`, competência reabre somente a via de tranca. A antiga inversa de fantasma saiu. `app/chamados/models.py:2325`, `:3377`, `:3667`, `:3678`, `:3683`, `:3699`, `:3706`. |
| Absorção, inversão e seleção por duplicata | Nova fica fora de absorção e fila de paridade, dentro da seleção que só exclui duplicata. O ramo do cartório exige que todas sejam duplicata. `app/chamados/models.py:4540`; `app/chamados/services/materializacao.py:275`, `:1202`; `app/chamados/services/fabrica_da_celula.py:149`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/cartorio.py:1705`. |
| Mérito, retratação, dedup e identidade | Proteção de mérito é só admin, no-op de retratação só duplicata. Se ficar fora de `VIAS_FORA_DO_FATO`, nova permanece nos filtros de identidade e dedup. `app/chamados/services/fio.py:1260`, `:1327`, `:1407`, `:1684`, `:1767`, `:2208`, `:2244`, `:2266`, `:2312`; `app/chamados/services/disputa_emissao.py:337`, `:409`, `:428`, `:446`; `app/chamados/services/tarefa_do_dia.py:262`; `app/api/foto_recebida.py:45`; `app/chamados/models.py:3036`, `:3067`, `:3947`, `:3953`. |
| Comandos | I6 acusa desconhecida se atingir `_i6_pares` e faltar no piso; F1t pode classificar como `carimbo/celula`; I9 só acusa via vazia. Relavra avulsa chama toda não derivada de “via de DECISAO”. Backfill só apanha `admin_decidiu` em seu filtro de decisão. Fecho-espelho só toca `SEM_HISTORIA`. `app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `:204`, `:435`, `:462`, `:474`, `:546`, `:961`, `:1740`; `app/chamados/management/commands/relavrar_via_i6.py:58`, `:88`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`, `:364`, `:432`, `:540`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34`, `:43`, `:67`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`. |
| Transporte e trilha | Composta, snapshots, refresh, histórico e contagens abertas aceitam o valor, sem branch próprio. `app/chamados/models.py:2186`, `:3589`, `:4507`; `app/chamados/reconciliador.py:1287`; `app/chamados/services/disputa_emissao.py:2182`; `app/chamados/services/materializacao.py:307`, `:1245`, `:1786`; `app/chamados/services/passivo_validado.py:69`, `:148`, `:578`; `app/chamados/services/relavra_via.py:123`, `:245`; `app/chamados/services/validacao.py:377`, `:471`; `app/chamados/management/commands/bug_par_103.py:67`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:59`. |

`I5a` é o contador mais próximo de vivos de desligado. Percorre `VIVOS` e só conta `COBRANCA` com `situacao` crua, portanto `registrado` fica fora. O vigia roda às 07:18 e o placar depende dele. O contador literal `vivos_de_desligado` está no backlog. `app/chamados/management/commands/auditar_invariantes_chamados.py:643`, `:659`, `:670`; `app/chamados/catalogo/motor.py:24`, `:29`; `app/config/crons.py:542`, `:747`, `:2194`; `app/docs/BACKLOG.md:732`, `:733`.

## 27/11 16:54 · sessao aa0d1df7f86a · - · - · origem=Ronald (tmux)

**3. Auditoria S89**

- O nascimento está em `app/colaboradores/services/desligamento.py:195`, depois da cascata de disputas e chamados em `:164` e `:182`. Entra por `ChamadoColaborador.criar`, com `area='dp_rh'`, `categoria='auditoria_clt'`, `subcategoria='registrar_resposta_oficial'`, `modulo_origem='desligamento_revisao'`, urgência normal e JSON de data, motivo, autor e instante (`:205`, `:208`, `:209`, `:216`). O catálogo marca módulo de auditoria, e `criar` traduz o estado inicial para `registrado`. Esse estado pertence a `VIVOS` e `NAO_FECHADOS` e fica fora de `COBRANCA`. `app/chamados/catalogo/natureza.py:18`, `app/chamados/models.py:1113`, `app/chamados/models.py:1130`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:134`.
- A view `desligar_colaborador` julga e chama o executor. O cron seleciona demissão vencida de cadastro ainda não desligado, imprime em dry e, em apply, revalida e chama o mesmo executor com autor de request nulo. A agenda declara `--apply` às 06:06, papel `agenda`. `app/colaboradores/views.py:1464`, `app/colaboradores/views.py:1472`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:66`, `app/config/crons.py:440`, `app/config/crons.py:2309`.
- O push remete ao detalhe local, com descrição inteira, followups e ação regular de resposta/status. O fio e o modal gerenciais leem todos os chamados do colaborador, mostram descrição truncada e timeline. O painel lateral lê JSON e timeline. A lista do colaborador exclui o módulo interno. `app/colaboradores/services/desligamento.py:219`, `app/chamados/push_utils.py:64`, `app/chamados/urls.py:14`, `app/chamados/views.py:363`, `app/chamados/views.py:550`, `app/templates/chamados/detalhe_local.html:97`, `app/templates/chamados/detalhe_local.html:112`, `app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:322`, `app/chamados/services/fio.py:228`, `app/chamados/views.py:678`, `app/chamados/views.py:890`, `app/templates/chamados/fio_colaborador.html:231`, `app/templates/chamados/partials/modal_fio.html:487`, `app/chamados/views.py:347`, `app/templates/chamados/partials/painel_chamado_turno.html:35`, `app/chamados/services/janela_colab.py:36`, `app/chamados/services/janela_colab.py:74`. Entrega do push em produção [nao verificado].
- Leitores de catálogo e contagem. `data_demissao`, escopo dia e dono DP em `app/chamados/catalogo/modulos.py:141`. Exceção da tranca em `:265`, consumida em `app/chamados/models.py:1107` e `app/chamados/services/competencia_trancada.py:113`. Rótulo e ação de registro no desenlace `app/chamados/services/desenlace.py:30`, `:85`, comentário RH `app/chamados/services/verbos.py:176`. Painel aceita drill `registrado` e conta a fila de cobrança em `app/chamados/services/painel.py:189`, `:212`, `:307`, `:316`. Modal ativos e Inteligência seguem a fila em `app/chamados/views.py:901` e `app/inteligencia/views.py:101`. Lente Registros agrupa e conta por módulo em `app/chamados/views.py:1482` e `app/chamados/services/verbos.py:199`, `:225`. Placar Equipe conta resolução assinada por staff e validação em `app/chamados/services/verbos.py:242`, `:253`, `:263`. API Mensageria lista o registro sem verbo e conta no balde `registro_auditoria`, e a visão do colaborador lê total de vivos. `app/api/views_mensageria.py:390`, `:452`, `:459`, `:1043`, `:1082`. Vigia do fio admite registro com célula no agregado de vivos e contabiliza célula acesa só de módulo fora da lista de furos como fora do vocabulário, sem alarmá-la. `app/chamados/services/vigia_fio.py:322`, `:337`, `:343`, `:401`.
- `HistoricoVinculo` já recebe o desligamento agendado e efetivo pelo escritor existente. Tem observação limitada a 400, data, motivo e autor. A edição do cadastro apresenta a observação, o painel de vínculo lê seis linhas e o fio completo a apresenta no tooltip do histórico. `app/colaboradores/models.py:548`, `:577`, `:579`, `app/colaboradores/services/historico_vinculo.py:20`, `app/colaboradores/services/desligamento.py:112`, `:138`, `app/templates/colaboradores/editar.html:179`, `app/colaboradores/views.py:306`, `:317`, `app/inteligencia/resumo.py:51`, `app/templates/chamados/fio_colaborador.html:88`.
- `LogAuditoria` também já recebe o agendado e o efetivo. O primeiro dentro do atomic, o segundo via `registrar_log` após o bloco. O modelo comporta ref do colaborador, JSON de antes/depois, usuário e ator de sistema. A tela genérica requer `ver_saude_sistema`, semeada para TI. Cadastro tem `editar_cadastro` semeada para DP e expõe o histórico, logo armazenar JSON sozinho não garante que o informativo chegue ao DP. `app/colaboradores/services/desligamento.py:118`, `:227`, `app/core/auditoria.py:8`, `:22`, `app/core/observ.py:94`, `:133`, `app/core/models.py:285`, `:300`, `app/core/views.py:686`, `app/colaboradores/mixins.py:363`, `:370`, `:375`, `app/colaboradores/views.py:1259`.
- O S88 mencionado pelo escritor é a revisão de roadmap que motivou o chamado S89. O signal também se chama S88 Defesa 1 e tenta resolver o pai após fechar a disputa pela porta do chamado. O aval item 2 substitui a decisão de nascimento e a exceção antiga de contar S89 à parte. `app/colaboradores/services/desligamento.py:191`, `app/chamados/signals.py:19`, `:43`, `app/docs/RELATO.md:4243`, `aval.txt:1` (anexo).
- Os testes atuais exigem S89 e o seu payload, preservação da respondida e via `admin_decidiu` para a muda. Precisam de revisão conforme aval e LU-11. `app/colaboradores/tests/test_desligar_guards.py:73`, `:93`, `:117`, `:120`, `app/docs/CORTES.md:545`.

**6. Medição**

- O nome `vivos_de_desligado` aparece como condição de pronto do backlog. O equivalente mais próximo em execução é I5a, que percorre vivos e conta apenas `COBRANCA` com comparação crua `situacao == 'desligado'`. Perde `registrado`, portanto o S89 normal, e não tem trilha de recusas no universo. É o `auditar_invariantes_chamados` das 07:18, papel `vigia`, imprime a medida em stdout e no HTML. O exit 2 atual só observa F1/F2. `app/docs/BACKLOG.md:732`, `:733`, `app/chamados/management/commands/auditar_invariantes_chamados.py:643`, `:659`, `:670`, `:1867`, `:1886`, `app/config/crons.py:542`, `:2194`.
- Encaixe de desenho para o contador solicitado é o mesmo vigia I5a, com uma medida de tentativas cujas trilhas vêm de `LogAuditoria` e a medida de estoque usando `VIVOS` e `esta_desligado`. Existe molde de trilha `nascimento_recusado`/`reabertura_recusada` com módulo, colaborador e porta, deduplicado por chave e ciclo. Essa deduplicação faz a quantidade de linhas diferir da quantidade de tentativas quando a chave se repete. `app/chamados/management/commands/auditar_invariantes_chamados.py:637`, `app/chamados/models.py:1221`, `:1232`, `:1254`, `app/colaboradores/services/desligamento.py:23`, `app/chamados/catalogo/motor.py:24`.
- O outro contador da família, `desligados_retidos`, conta colaborador × competência retido pelo export e passa ao `vigiar_fio_morto`/carimbo. É universo distinto do estoque de chamados e da trilha. Se o contador novo precisar de publicação no ESTADO, há o molde de `vigiar_fio_morto` com carimbo e leitor `fio_estado`, e o placar operacional já tem leitor do carimbo para duas chaves. `app/folha/fora_da_folha.py:166`, `app/chamados/services/vigia_fio.py:565`, `:580`, `app/chamados/management/commands/vigiar_fio_morto.py:243`, `:275`, `:289`, `bin/fio_estado.py:122`, `:192`, `app/core/management/commands/pipeline_placar.py:85`, `:135`. O vigia de esteira trabalha com arquivos de fila e estado de fatias. Os juízes por varredura são dívida que só encolhe. `app/core/esteira_vigia.py:1`, `app/config/crons.py:1985`.
- Lei a esclarecer para o selo de zero: uma reabertura humana autorizada entra em `em_analise`, logo conta como vivo de desligado. O aval fornece simultaneamente a exceção humana e o selo zero. Unidade, eventual exclusão e momento desse selo em relação à reabertura [nao verificado]. `aval.txt:1` (anexo), `app/chamados/models.py:1428`, `app/chamados/catalogo/motor.py:24`.

## 27/11 16:45 · sessao d019348cce5d · CODEX-CONSTROI-MOTOR · construtor · origem=Code

Implementei a cura14/P4 em `core/esteira_vigia.py` e `core/tests/test_vigia_ccm.py`. O mesmo juiz CCM atende a guarda prévia e o reteste. Se a guarda recusa, o vigia avisa Pauta e RELATO com a primeira linha do juiz e retorna antes de Git, deploy e gravação de `ultimo_revert`. [esteira_vigia.py:748](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:748) [esteira_vigia.py:1320](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1320)

Na recusa tardia, o vigia tenta desfazer o revert. O deploy compensatório só roda se o Git retornar zero, e `revert desfeito` exige sucesso dos dois comandos. Uma falha em qualquer etapa aparece na Pauta e no RELATO, com teste para cada caso. A guarda depende de `_rodar_testes` devolver `None`, o que ocorre depois de escrever o log de recusa. [esteira_vigia.py:1339](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1339) [test_vigia_ccm.py:189](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:189) [esteira_vigia.py:753](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:753) [nao verificado]

**Pré-verificação.** O diff passou em `git diff --check`. A base, a cura e os três mutantes compilam, com 25 `ExceptHandler` e um único `argv` do CCM. A guarda usa o `agora` já recebido. `deve_reverter` lê `ultimo_revert` para aplicar o teto. `passada` regrava o estado que leu antes de chamar `auto_revert`, então preserva o valor carregado na recusa prévia e pode sobrescrever um carimbo novo se a tentativa chegou ao reteste. [esteira_vigia.py:749](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:749) [esteira_vigia.py:1076](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1076) [esteira_vigia.py:1319](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1319) [esteira_vigia.py:1548](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1548) [esteira_vigia.py:1657](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1657) [LEIS.md:97](/home/ronald/wt-codex-codex-constroi-motor-163727/app/docs/LEIS.md:97) [nao verificado]

**Pista pendente.** O ambiente disponível é a VM. A Pista exige rede e `logs/CODEX-CONSTROI-MOTOR/cura14/red_p4.log` está em caminho ignorado. O log ficou pendente. O RED na base, o verde na cura e a mordida dos mutantes seguem [nao verificado]. A certificação cabe ao Code e ao opus final. [/etc/hostname:1](/etc/hostname:1) [CLAUDE.md:129](/home/ronald/wt-codex-codex-constroi-motor-163727/CLAUDE.md:129) [.gitignore:53](/home/ronald/wt-codex-codex-constroi-motor-163727/.gitignore:53) [LEIS.md:147](/home/ronald/wt-codex-codex-constroi-motor-163727/app/docs/LEIS.md:147)

### Comandos para o Code na Pista

Rodar a partir de `app/`, no ambiente de teste da Pista. A função carrega a variante em memória e usa os testes da cópia. Os alvos dos mutantes correspondem às três asserções pedidas. [manage.py:7](/home/ronald/wt-codex-codex-constroi-motor-163727/app/manage.py:7) [test_vigia_ccm.py:146](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:146) [test_vigia_ccm.py:162](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:162) [test_vigia_ccm.py:172](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:172) [nao verificado]

```bash
set -o pipefail
vigia=core/esteira_vigia.py
classe=core.tests.test_vigia_ccm.AutoRevertCCMTest
recusa="$classe.test_auto_revert_juiz_recusa_antes_do_revert"
ordem="$classe.test_auto_revert_ordem_juiz_antes_do_git"
desfaz="$classe.test_auto_revert_reteste_recusado_desfaz"

p4() {
    PYTHONDONTWRITEBYTECODE=1 python3 -c '
import sys
from core import esteira_vigia as vigia
from manage import main

exec(compile(sys.stdin.read(), vigia.__file__, "exec"), vars(vigia))
sys.argv[0] = "manage.py"
main()
' test --settings=config.settings.ci --noinput -v 2 "$@"
}

# RED na base. Capturar esta saída em logs/CODEX-CONSTROI-MOTOR/cura14/red_p4.log.
git show 0e0497a1ce54c3de18be24660cf8a35ec9de6680:app/core/esteira_vigia.py \
    | p4 "$recusa" "$ordem" "$desfaz"

# Cura.
p4 core.tests.test_vigia_ccm < "$vigia"

# juiz_depois_do_revert
sed -e '/^    # O revert restaura/,/^        return .sem revert: reteste seria recusado/{H;d;}' \
    -e '/^        return .revert nao aplicou./G' "$vigia" \
    | p4 "$recusa" "$ordem"

# recusa_segue
sed '/^def auto_revert(/,/^def explicar_vermelho(/s/^    if juiz.returncode != 0:$/    if False:/' \
    "$vigia" | p4 "$recusa"

# none_mantem_revert
sed "/^    desfecho_recusa = None$/,/^    memoria\[/s/^        if subprocess.run(.*'HEAD'.*returncode != 0:$/        if False:/" \
    "$vigia" | p4 "$desfaz"
```

## 27/11 16:44 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

A recusa tardia apura o resultado da compensação, emite Pauta e RELATO e então grava `ultimo_revert`. [core/esteira_vigia.py:1339] [core/esteira_vigia.py:1347] [core/esteira_vigia.py:1350]

## 27/11 16:43 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

A compensação agora precede a persistência, e o sucesso exige retorno zero de Git e deploy. O teste cobre separadamente os dois códigos de falha e os respectivos avisos. [core/esteira_vigia.py:1339] [core/tests/test_vigia_ccm.py:189] [nao verificado]

A gravação do estado ainda precede os avisos. Se ela lançar `OSError`, a compensação terá sido tentada, mas Pauta e RELATO podem ficar sem registro. [core/esteira_vigia.py:823] [core/esteira_vigia.py:1347]

## 27/11 16:42 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

O pré-verificador confirmou por AST 25 `ExceptHandler` na base e na cura, um único argv do CCM e a leitura de relógio já existente. A guarda prévia retorna antes de Git, deploy e gravação. [core/esteira_vigia.py:749] [core/esteira_vigia.py:1304] [core/esteira_vigia.py:1322] [nao verificado]

`deve_reverter` lê `ultimo_revert` para aplicar o teto. A recusa prévia preserva o valor carregado. Quando a tentativa chega ao reteste, `passada` pode sobrescrever o carimbo novo ao regravar o estado lido antes de chamar `auto_revert`. [core/esteira_vigia.py:1076] [core/esteira_vigia.py:1338] [core/esteira_vigia.py:1542] [core/esteira_vigia.py:1630] [core/esteira_vigia.py:1651]

Na recusa tardia, uma falha na gravação do estado pode impedir o rollback. Os retornos do Git e do deploy compensatórios ficam sem conferência, permitindo anunciar `revert desfeito` se um dos comandos falhar. [core/esteira_vigia.py:823] [core/esteira_vigia.py:1339] [core/esteira_vigia.py:1341] [nao verificado]

O diff cobre os efeitos pedidos nos três selos. A execução na Pista e a mordida dinâmica seguem [nao verificado]. [core/tests/test_vigia_ccm.py:144] [core/tests/test_vigia_ccm.py:160] [core/tests/test_vigia_ccm.py:170]

## 27/11 16:23 · sessao df030164fcae · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c11 põe o gatilho do feriado acima da região rolável na ordem de pintura da célula. O template isola o empilhamento da célula mensal e usa `z-index:1` na ação e `z-index:0` na região. A célula segue com 132 px, e as marcações continuam no fim da região rolável, como na c10. A guarda B1 do histórico permanece no JS. [_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Ação e região:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Ordem das marcações:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132) · [Guarda B1:27](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:27)

Os **104 px** vêm da sonda. Ela posiciona a ação sobre o conteúdo da célula para deixar pouco espaço vertical no drawer de 128 px. No s11, o retângulo cabe na célula e na viewport, mas a sonda também exige que o centro receba o clique. A correção trata a sobreposição com a grade rolável da c10. Qual elemento recebeu o clique no s11 é **[não verificado]**. [Posicionamento:277](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:277) · [Medição:291](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:291)

O **RED** é o próprio teste que acusou as quatro falhas do s11. O mutante novo `drawer-sem-empilhamento-da-acao` volta `isolation` e `z-index` a `auto` só no drawer, repondo o empilhamento da c10. A revisão estática confirmou que a sonda, as asserções, os esperados e os mutantes anteriores são idênticos aos da c10. Contagem no diff **0 `except`, 0 `catch`**. [Mutante:78](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:78) · [Teste RED:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

**Recorte para a sessão na VM**, com o smoke de clique e histórico, a renderização da grade e os selos do popover. [Uso de `--only`:13275](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/docs/RELATO.md:13275)

```bash
PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --only "core.tests.test_smoke_clique_feriado_vira_trabalho colaboradores.tests.test_feriado_botao_cortado core.tests.test_selo_popover_acao"
```

**Comando do mutante** no ambiente de testes da VM. [Seleção do mutante:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=drawer-sem-empilhamento-da-acao \
  python manage.py test --settings=config.settings.ci --noinput \
  core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
```

Resultado da c11 em Chromium e falha do mutante pelo motivo esperado **[não verificado]**. A certificação fica com a suite e o opus final. [nao verificado]

## 27/11 16:16 · sessao b845a7dd5be1 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **K1 continua aberto.** O turno partido de `10:00–14:00 + 14:30–18:20` leva **470 min à HE100 feriado** na promoção. O corte do K1 exige **440 min de HE100 feriado e 30 min de intra indenizada**. O laço do motor que apura a supressão percorre `periodos`, e a promoção soma os minutos de `periodos_ft`. O teste continua como `expectedFailure`. (`app/ponto/motor_calculo_v2.py:3078–3118`, `app/ponto/services/fechamento.py:993–997`, `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`, `app/docs/RELATO.md:255`)
2. **A certeza da escala ainda pode decidir quantos minutos chegam à partição.** Com previsto zero, empresa sujeita à janela e sem autorização, a L-097 conserva o par de folga com escala incerta e zera o par com escala certa. O teste que cobra a mesma quantidade nas duas rubricas está em `skip`. A precedência entre esse bloqueio e o aval literal continua pendente. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1562–1597`, `app/ponto/tests/test_feriado_um_juiz_porta.py:499–515`)
3. **A vigência protege a classificação antiga da 10, mas o aval também veda tocar exportada.** A guarda específica cobre a 09. Nas demais, o evento pode regravar o mensal e as duas lavras. Na 11 e seguintes, isso já usa a regra nova mesmo se houver TXT entregue. O POST pode gerar outra fotografia a partir do cálculo online. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:36–50,600–639,1189–1223,1294–1328,1829–1855`, `app/folha/views.py:214–219,269–275,302–307`)
4. **A data e o vínculo ainda podem divergir.** Reproduzi em memória o turno do dia 21 cair na competência anterior quando o feriado é no dia 20. Também reproduzi na partição 4 h de HE100 feriado para um par cujo dia do turno pertence a 12x36, usando o contexto 6x1 que trouxe o par e a data herdada da véspera. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2522,2537–2545`, `app/ponto/calculador/regras.py:410–431`)
5. **As três gravações ainda podem divergir.** O `atomic` protege o `delete` e o `bulk_create` de uma versão de `DiaPago`. O mensal é salvo antes, e as falhas das duas lavras são capturadas separadamente. Uma falha pode deixar o mensal novo com uma ou ambas as lavras antigas. (`app/ponto/services/dia_pago.py:355–363`, `app/ponto/services/fechamento.py:1166–1223,1267–1270,1328–1332`)
6. **Três testes estruturais falham na própria cópia.** Executei os testes de AST em memória. A catraca de endereços registra cinco sítios divergentes, incluindo `folha/export.py:505` onde o censo encontra `:514`. Os dois testes do dia da lavra rejeitam a atribuição `dia_jornada = dia_turno`, embora a linha seguinte atualize o mapa. (`app/core/tests/test_medida_turno_do_par.py:286–289,326–336,565–569`, `app/folha/export.py:514`, `app/ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:46–71`, `app/ponto/calculador/regras.py:420–430`)
7. **Pouso e apply [nao verificado].** O aval exige medir antes os 75 dias de 42 colaboradores como hora comum, excluir os intermitentes e entregar reversão, antes × depois por colaborador e DIFF do TXT 11. A prova desses itens, a suíte inteira neste hash e o pouso de E1 antes do K1 estão [nao verificado]. O K1 tem aval separado e condicionado ao DIFF de frota por rubrica antes do pouso. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016`, `CLAUDE.md:448–467`)

## Ver3, achado por achado

| Achado do ver3 | Situação na c4 |
|---|---|
| **1. TXT perde a primeira fatia, `cadastro_zero` ou resumo perde a rubrica** | **Curado no caso reproduzido.** Emissão, homologação e resumo do cartão usam `eventos_do_fechamento_da_competencia`. A 11 admite FT promovida com `horas_trabalhadas=0` quando a rubrica está cadastrada e o colaborador está sujeito ao ponto. O comparador acusa a rubrica ausente com HE100 bruta igual. O PDF discrimina a parcela no resumo. (`app/folha/export.py:388–394,750–760,924–926,1023–1032`, `app/relatorios/cartao_pela_celula.py:89–101,250–277`, `app/relatorios/pdf_espelho.py:771–779`, `app/ponto/tests/test_feriado_um_juiz.py:56–99`) |
| **2. Exportada fora da guarda pode mudar** | **Curado para a promoção retroativa da FT incerta até a 10. Aberto para a proteção do gravado.** A fixture da 10 força `entra`, cadastra apenas a rubrica de feriado e chama `registrar` diretamente. Nesse caso, a fotografia anterior tem zero linhas de eventos. O teste compara seis campos do mensal, campos escolhidos das lavras e o registro anterior. A reemissão pelo POST e a 11 já entregue ficam sem essa cobertura. (`app/ponto/calculador/regras.py:393–409`, `app/ponto/tests/test_feriado_um_juiz_porta.py:151–160,239–296`, `app/folha/export.py:539–548,750–765`, `app/folha/views.py:214–219,269–275,302–307`) |
| **3. Oráculo e DIFF redecidem a dobra** | **Curado para a HE100 feriado normal alimentada a partir da 11.** O oráculo recebe os minutos dos períodos da fatia, inclusive zero com calendário injetado vazio. `periodo.reler` também passa a carregar os minutos do período. O DIFF usa a mesma projeção FT nos dois lados, então `4,00 = 4,00` prova a ligação da partição com o comando. O caminho do oráculo ainda calcula intra e, fora da projeção promovida, noturno e splits com a régua do recorte. (`app/ponto/calculador/alimentacao.py:429–446,518–523,571–597`, `app/ponto/calculador/periodo.py:764–769,824–843`, `app/ponto/calculador/regras.py:793–817,909–932`, `app/ponto/management/commands/diff_calculador.py:244–257,403–415`) |
| **4. Split bruto sobrescrito ou acima do total** | **Curado no recorte da FT promovida.** A c4 preserva o campo bruto, limita o split aos minutos trabalhados e usa a união da janela no plantão longo com prorrogação. O controle em memória de 21h a 23h do dia seguinte deu **1.500 min de split**, com adicional legal de **1.500 ÷ 0,875 min**. A correção do bruto em outros caminhos fica fora desse ramo. (`app/ponto/calculador/regras.py:432–447`, `app/ponto/motor_calculo_v2.py:82–118,151–161`, `app/ponto/tests/test_feriado_um_juiz.py:331–365`, `app/ponto/tests/test_feriado_um_juiz_porta.py:572–581`) |
| **5. K1 e dia do turno** | **K1 aberto. Data parcialmente curada.** Há asserções de data e rubrica nas duas lavras para dois turnos e troca de praça. A correção do mapa é condicional ao ciclo, ao feriado e à presença do dia na fatia. O corte de data e a classificação original do período já aconteceram quando ela roda. (`app/ponto/tests/test_feriado_um_juiz_porta.py:380–394,455–497`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/motor_calculo_v2.py:2537–2574`) |
| **6. Soma, idempotência e falha da relavra** | **Curado o rollback de uma versão. Paridade parcial.** O teste sem promoção exige **8,04 h** no mensal para `241 + 241 min`. A soma diária desses minutos arredonda para **8,03 h**. Com uma FT promovida de 10 segundos, mesmo aparecendo como `0,00 h` de feriado no teste, o mensal passa para **8,03 h** nas horas comuns. Duas execuções bem-sucedidas podem repetir os valores, mas recriam linhas e carimbos. A prova de concorrência e de atomicidade entre mensal e versões está [nao verificado]. (`app/ponto/services/fechamento.py:1005–1013`, `app/ponto/services/dia_pago.py:344–363,552–565`, `app/ponto/models.py:373–387`, `app/ponto/tests/test_feriado_um_juiz_porta.py:396–453`) |

## Aval, vigência e alcance

- **A regra principal entrou.** Em 6x1, a FT já apurada consulta `_feriado_dobra_no_dia` pelo dia do turno. Com dobra devida, a FT incerta vira HE100 feriado. Com escala certa, `feriado_vira_folga` separa HE100 feriado de folga trabalhada a 100%. O diff fornecido preserva `motor_calculo_v2.py` e `CICLOS_FERIADO_SIMPLES`. O intermitente é excluído por base ou ciclo e seu motor injeta calendário vazio. Um período da própria fatia 12x36 fica fora da promoção. A emissão e a troca de vínculo ainda admitem os casos dos bloqueios 3 e 4. A cláusula cadastrada da CCT no caminho regular 12x36 continua no código. (`diff.txt:1`, `app/ponto/calculador/regras.py:314–322,367–377,410–431`, `app/ponto/motor_calculo_v2.py:941–978,3686–3698,3740`, `app/core/regua_cct.py:466–479`)
- **A partir da competência 11/2026**, mudam a promoção, o consumo da HE100 feriado no TXT e no cartão, a projeção nova para o oráculo, o ajuste noturno da FT promovida e o arredondamento em mensal com promoção positiva. Até a 10, a partição e a emissão históricas conservam o critério anterior. A borda final muda já na 11 e a inicial só na 12. O `atomic` da lavra, a comparação do cartão e a exibição no PDF valem para todas as chamadas. `periodo.reler` também mudou sem parâmetro de competência. (`app/folha/export.py:385–394,924–926`, `app/ponto/calculador/regras.py:386–447`, `app/ponto/services/fechamento.py:223–230,989–1013,1294–1305`, `app/ponto/services/dia_pago.py:358–362`, `app/ponto/calculador/periodo.py:539,764–769`, `app/relatorios/cartao_pela_celula.py:266–273`, `app/relatorios/pdf_espelho.py:771–779`)
- **Transportar a rubrica da fatia para o TXT cabe no aval.** Corrigir a data da rubrica preserva o objetivo enquanto usa o turno que já foi julgado. O filtro de batidas da borda atua **antes** do motor e pode alterar a apuração de um turno comum. A união altera também `horas_noturnas`, que sai como adicional no TXT. O arredondamento condicionado à promoção altera horas comuns de outros dias do mesmo mensal. Esses efeitos vão além da redistribuição entre HE100 feriado e folga trabalhada. A faixa de impacto na frota está [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:154–172,339–348,998–1013`, `app/folha/export.py:453–462`, `CLAUDE.md:274–277`)
- **A 10 fica igual ao gravado?** O corte preserva a regra para os mesmos insumos. Igualdade com o gravado real, com ambas as lavras e com o TXT vigente [nao verificado]. A própria cópia registra que a 10 foi paga à mão, sem TXT, e saiu da guarda. A condição “10 exportada” está na fixture e na pergunta. A L-113 permite recálculo genérico fora da guarda, enquanto o aval da obra exige preservar exportadas. (`app/ponto/services/fechamento.py:36–50,600–639`, `app/ponto/tests/test_feriado_um_juiz_porta.py:239–289`, `app/docs/LEIS.md:125,155`, `CLAUDE.md:458–470`)

## Escritores, leitores e invariantes

| Valor | Quem escreve e quem lê |
|---|---|
| **HE100 feriado, HE100 total e folga trabalhada** | O v2 escreve os minutos dos períodos normais. `particionar_ft` produz as três listas em memória. O fechamento soma e grava o mensal, e `lavrar` grava as linhas diárias. TXT e cartão leem o mensal passado a eles. A emissão calcula HE100 comum como HE100 total menos a parte de feriado. `horas_extras_100_feriado` agrega feriado normal e FT promovida, sem guardar a origem no mensal. No dia, `folga_feriado_min` identifica só a FT promovida. (`app/ponto/motor_calculo_v2.py:328–344`, `app/ponto/calculador/regras.py:392–452`, `app/ponto/services/fechamento.py:984–1006,1193–1199`, `app/ponto/services/dia_pago.py:173–186,235–257`, `app/folha/export.py:434–460`, `app/relatorios/cartao_pela_celula.py:90–101`) |
| **`horas_trabalhadas` e previsto** | O mensal e a lavra somam períodos normais, órfãos e FT comum. O campo exclui FT com rubrica própria. `pago_do_dia` acrescenta folga trabalhada e o detalhe da FT promovida, portanto responde a uma pergunta diferente. O export lê `horas_trabalhadas` para classificação e prontidão. O previsto do fechamento vem da grade da célula e o realizado limita o abono, sem entrar na soma do previsto. Há uma exceção preexistente na grade por turno do intermitente convocado, que copia o realizado para o previsto. (`app/ponto/services/fechamento.py:1005–1013,1075–1093`, `app/ponto/services/dia_pago.py:258–271,388–402,892–900`, `app/folha/export.py:329–338,853–858,924–926,1058–1074`, `app/escala/services/leitor_celula.py:399–423`, `app/escala/utils.py:2099–2124`) |
| **Split noturno e adicional** | O v2 escreve o split bruto e os minutos noturnos legais. A partição escreve `FolgaEmFeriado` com o split de relógio e o adicional legal da promoção. Mensal, lavra e projeção leem esses valores. O TXT emite o adicional legal em evento próprio. O split de HE100 é subconjunto da HE100. (`app/ponto/motor_calculo_v2.py:1013–1049`, `app/ponto/calculador/regras.py:380–383,432–447`, `app/ponto/services/fechamento.py:998–1004`, `app/ponto/services/dia_pago.py:244–252`, `app/ponto/calculador/alimentacao.py:369–388`, `app/folha/export.py:438–461`) |
| **Rubricas do oráculo e `lavrar(rubricas=...)`** | `alimentacao` projeta a partição e lê os valores da primeira fatia para a dobra normal. `do_dia` devolve `valores` e `nao_decide`. Desde a 10 existe a versão `oraculo`; da 11 em diante ela recebe a partição nova. A mesma `lavrar` valida donos declarados, monta a base com os períodos e substitui os campos decididos. A soma lê por versão. (`app/ponto/calculador/alimentacao.py:353–389,518–523,571–597`, `app/ponto/calculador/regras.py:787–817,954–961`, `app/ponto/services/fechamento.py:1294–1328`, `app/ponto/services/dia_pago.py:50–76,311–346,552–565`) |
| **Data, contexto da fatia e `_FERIADO_CLASSIFICADO_NO_MENSAL`** | O motor escreve o dia do turno por entrada crua e o mapa da jornada por período. A composição guarda `(motor, vínculo)` por identidade e por dia. A partição lê os dois e também escreve no mapa da jornada. Lavra, oráculo e DIFF leem esse mapa. A data civil é a resposta de contingência quando falta mapeamento. O literal `(2026, 11)` mora no export e é lido por emissão, classificação, partição, fechamento e DIFF. Competência é o rótulo da janela da empresa, vigência do vínculo é quem cobre cada data, e o corte da regra é um terceiro valor. Chamadas avulsas sem competência têm comportamento próprio. (`app/ponto/motor_calculo_v2.py:911–922,963–978,2513–2522`, `app/ponto/services/fechamento.py:223–243,335–350,377–382`, `app/ponto/calculador/regras.py:386–431,679–686`, `app/ponto/services/dia_pago.py:109–132`, `app/folha/export.py:385–394,924–926`, `app/ponto/management/commands/diff_calculador.py:112–152,203–215`, `app/ponto/janelas.py:10–21,54–63`, `app/escala/alimentacao.py:205–264`) |

**Juízes, portas e leitores.** F3 continua no motor, e o auxiliar do oráculo empresta seu método. O segundo escritor introduzido é **em memória**, no mapa `dia_da_jornada`. A borda chama o juiz de turnos para retirar batidas e depois entrega ao motor um conjunto diferente para novo pareamento. A partição é disjunta por ocorrência, e suas três listas somam a FT que chegou até ela. Isso não garante que todos os minutos tenham chegado à competência certa nem que mensal, dia e TXT usem a mesma fotografia. O cartão exige a existência do mensal gravado e então recalcula online. O PDF usa o mesmo resumo na janela de competência. O POST do TXT também calcula online, e o comparador `cartao_x_txt` usa o **mensal gravado** como referência. Já `diff_calculador` recalcula ou intercepta a apuração em memória. O DIFF entre arquivos entregues lê os conteúdos armazenados. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:301–322,392–452`, `app/ponto/services/fechamento.py:339–348,1766–1792`, `app/relatorios/cartao_pela_celula.py:69–76,250–273,324–350`, `app/relatorios/pdf_espelho.py:628`, `app/folha/views.py:214–219,269–275`, `app/ponto/management/commands/diff_calculador.py:114–152`, `app/folha/haiku_export.py:79–107`)

**Relógio e travas.** O diff não adiciona leitura de relógio de parede. O corte usa `(ano, mes)` e o fechamento já captura `_agora_da_passada` uma vez. Não identifiquei inversão explícita nova. A entrega trava Empresa, e a revisão específica segue Empresa → Colaborador → Batida. O evento genérico roda em `on_commit` sem essas travas. A transação local da lavra não serializa entrega, mensal e as duas versões. (`diff.txt:1`, `app/ponto/services/fechamento.py:600–604,678–680,1829–1855`, `app/folha/views.py:102–104,132–135`, `app/folha/export.py:570–576`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/ponto/services/dia_pago.py:358–362`)

## Contraexemplos e controles adicionais

- **20/21.** Em memória, com dois turnos `20/11 18–22h` e `21/11 03–07h` e feriado **no dia 20**, a 11 recebeu **4 h de feriado e 4 h comuns no dia 20**, e a 12 recebeu zero. Com feriado em **21/10**, a 10 recebeu as 8 h comuns e a 11 recebeu zero. A fixture de borda cobre feriado em **21/11**, quando os dois lados do filtro já atuam. Um único turno `20/11 22h → 21/11 07h` com feriado só no dia 21 ficou inteiro na 11, como hora comum. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2545`, `app/ponto/tests/test_feriado_um_juiz.py:391–425`)
- **Municipal × estadual e troca no meio do mês.** Londrina e Cambé compartilham um feriado estadual do PR. Um municipal de Cambé alcança só o vínculo de Cambé. O carregador resolve a praça pelo vínculo de cada dia. Na troca **6x1 → 12x36**, a partição pode impedir a troca de contexto pelo ciclo de destino e conservar a dobra já decidida pelo 6x1 de origem. Reproduzi esse ramo com 240 min e confirmação de que a data lavrada ficou na véspera. Ocorrência na frota [nao verificado]. (`app/core/feriados.py:57–67,135–167`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/tests/test_feriado_um_juiz_porta.py:470–497,595–615`)
- **Intermitente e desligado no dia 20.** Um FT intermitente por base ou ciclo fica em trabalhadas na partição. Para desligamento em 20/11 com corte 21, o colaborador ainda entra no universo de apuração da 11. A grade considera o fato lavrado no dia da demissão, e o classificador manda as verbas para a rescisão antes de avaliar o feriado emitível. Assim, 4 h no mensal podem coexistir com zero linha no TXT mensal por rescisão. O valor no módulo de rescisão está [nao verificado]. (`app/ponto/calculador/regras.py:367–377`, `app/ponto/services/fechamento.py:489–506`, `app/ponto/janelas.py:10–21`, `app/escala/services/leitor_celula.py:379–398`, `app/folha/export.py:54–60,878–893,924–926`)

## Selos e `except`

- **35 controles sem banco passaram. Os 33 mutantes sem banco mordem código de produção:** todos passaram no controle e provocaram falha de asserção no alvo mutado. Há ainda **11 mutantes de porta**, dez com alvo em produção e um de fixture que exige erro de unicidade. A execução dos 11 e os REDs contra os commits-base estão [nao verificado]. O mutante `endereco-c2` altera o registro do teste, e o controle correspondente já falha nesta cópia. (`app/ponto/tests/feriado_um_juiz_mutantes.py:42–219,273–318`, `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`)
- **`except` no diff fornecido: 0 adicionados e 0 removidos, inclusive em testes.** Os `except` do comando de DIFF, da alimentação e das duas lavras já estão no contexto do diff. `assertRaisesRegex` e `finally` nos testes não são cláusulas `except`. (`diff.txt:1`, `app/ponto/management/commands/diff_calculador.py:130–140`, `app/ponto/calculador/alimentacao.py:429–455,552–560`, `app/ponto/services/fechamento.py:1267–1270,1328–1332`, `app/ponto/tests/test_feriado_um_juiz.py:20–40`)

## 27/11 16:15 · sessao 080e3483b92e · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c10 coloca as marcações no fim da área rolável dos feriados com ação oferecida no modo mês. A célula mantém os **132 px**, e o botão fica acima da região rolável. [Grade:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Grade:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Grade:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132)

**Causa provável.** Na ficha `420px 1fr`, o anexo mede uma célula de **102 × 132 px** e deixa **no máximo 70,41 px** abaixo do botão, descontados os 3 px de margem e os 6 px de padding e borda inferiores. A região da c9 podia ter ações e faixas depois de `17:00`. Chegar ao máximo (**145 de 145 px**) alinhava o fim de todo esse conteúdo, e `17:00` continuava invisível. O anexo traz o resultado agregado da sonda. A caixa de `17:00` e o ponto exato do recorte ficam **[nao verificado]**. `s10_falha.txt:4` · [Ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) · [Grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138) · [Sonda:428](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:428)

**RED e mutantes.** O RED é o smoke existente na c9. O s10 registrou **19 testes, 1 falha**, em `ultimaVisivelNoFim`. O diff de teste acrescenta duas entradas CSS e **0 `except`/`catch`**. A sonda, a asserção, os valores esperados e o teste B1 de histórico ficaram idênticos byte a byte. A execução da c10 e dos mutantes fica com o Code **[nao verificado]**. `s10_falha.txt:3–9` · [Mutantes:76](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:76) · [Smoke e B1:545](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:545) [nao verificado]

```bash
# RED na c9. Na c10, o mesmo comando exercita a cura.
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci "$alvo"

# M1 — restaura o fluxo em bloco da c9
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=rolagem-em-bloco python manage.py test --settings=config.settings.ci "$alvo"

# M2 — mantém o grid e devolve as marcações à ordem anterior
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=marcacoes-antes-dos-detalhes python manage.py test --settings=config.settings.ci "$alvo"
```

**Leitores.** A ficha e a aba Espelho do drawer carregam o calendário mensal. A fita de diagnóstico usa `layout='fita'`, `relatorios/furos.html` inclui a grade sem `layout`, e o Espelho de Ponto avulso tem grade semanal própria. A mudança de CSS vale somente para `layout='mes'` com a ação de feriado oferecida ao usuário. [Ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) · [Aba Espelho:134](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:134) · [Calendário:67](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/calendario_partial.html:67) · [Fita:35](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [Furos:42](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/relatorios/furos.html:42) · [Espelho avulso:97](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/espelho.html:97)

**Recorte.** O `--only` literal do s10 está **[nao verificado]**. O anexo traz a contagem, e este recorte foi reconstruído para 19 testes. `s10_falha.txt:7` [nao verificado]

```bash
--only "colaboradores.tests.test_feriado_botao_cortado colaboradores.tests.test_ui_pilulas_quebram_linha core.tests.test_selo_popover_acao core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_smoke_retratar_1366"
```

## 27/11 16:05 · sessao 1a2ab4657839 · FERIADO-UM-JUIZ · construtor · origem=Code

A c4 está na cópia sobre `29488e3bb`. Em memória, os **9 REDs falharam na c3**, com 12 falhas de asserção contando os subcasos. Na c4, passaram **35 testes e 33 mutantes**. O diff acrescenta **0 `except`**. Os testes de porta com banco e a certificação pelo Code e pelo opus final ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:261`, `ponto/tests/feriado_um_juiz_mutantes.py:279`)

| Item | RED e prova na c3 | Cura e leitores |
|---|---|---|
| **A** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_cartao_le_a_rubrica_emitida` e `::test_RED_c4_cartao_x_txt_acusa_feriado_ausente_com_he100_igual`. O TXT emitiu `400`, o cartão mostrou `0 h` e o contador devolveu `{}` ao perder a rubrica com HE100 bruta igual. (`ponto/tests/test_feriado_um_juiz.py:56`, `:72`) | Cartão, montagem do TXT e homologação leem os mesmos eventos do mensal. O PDF discrimina HE100 feriado e o contador confere a rubrica e o total. App e ficha leem o total. (`folha/export.py:388`, `:753`, `:1026`, `relatorios/cartao_pela_celula.py:89`, `:266`, `relatorios/pdf_espelho.py:773`, `api/views.py:1663`, `colaboradores/services/ficha.py:205`) |
| **B** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_vigencia_conserva_ft_incerta_em_trabalhadas_ate_outubro`. Na c3, outubro classificou os 240 min como HE100 feriado. O teste de porta `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_evento_na_10_entregue_conserva_a_vigencia_e_a_11_aberta_promove` cobre mensal, duas lavras, TXT vigente e controle na 11 aberta. Execução da porta [nao verificado]. (`ponto/tests/test_feriado_um_juiz.py:300`, `ponto/tests/test_feriado_um_juiz_porta.py:239`) | Partição, lavras e DIFF usam a vigência do TXT. Até outubro, a partição segue a seleção por escala certa e a cláusula histórica da base. O recálculo genérico L-113 segue disponível nas competências entregues a partir da 11. A reemissão nessas competências está [nao verificado]. (`folha/export.py:385`, `ponto/calculador/regras.py:395`, `ponto/services/fechamento.py:989`, `:1305`, `:1829`, `ponto/management/commands/diff_calculador.py:152`, `59600163c:app/ponto/services/fechamento.py:958`) |
| **C** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_alimentacao_le_a_dobra_normal_da_primeira_fatia` e `::test_RED_c4_periodo_normal_respeita_calendario_injetado_vazio`. Na c3, o oráculo respondeu `0`, `4` e `4 h` nos casos que pediam `4`, `0` e `0 h`. A releitura também inventou 240 min com calendário vazio. (`ponto/tests/test_feriado_um_juiz.py:216`, `:232`) | A alimentação da lavra `oraculo`, o DIFF e a releitura por período leem os minutos decididos pelo motor que apurou o turno. A prova integrada de mensal, lavras e TXT está [nao verificado]. (`ponto/calculador/alimentacao.py:519`, `:584`, `ponto/calculador/periodo.py:768`, `ponto/management/commands/diff_calculador.py:411`, `ponto/tests/test_feriado_um_juiz_porta.py:298`) |
| **D** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_par_de_21h_a_23h_conta_a_uniao_do_noturno`. A c3 deu **1.560 min** de split. A c4 deu **1.500 min** e adicional legal de **1.500 ÷ 0,875 min**. (`ponto/tests/test_feriado_um_juiz.py:353`) | A promoção mede a união da janela com o medidor existente, desconta os intervalos e entrega split e adicional ao mensal, às duas lavras e ao DIFF. A prova integrada está [nao verificado]. (`ponto/calculador/regras.py:433`, `ponto/services/fechamento.py:998`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:353`, `ponto/tests/test_feriado_um_juiz_porta.py:572`) |
| **E** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_data_e_certeza_seguem_o_turno_de_cada_par` e `::test_RED_c4_turnos_distintos_na_borda_20_21_entram_cada_um_na_sua_competencia`. A c3 pôs ambas as parcelas na véspera. Na borda, deixou os dois pares na 11 e nenhum na 12. (`ponto/tests/test_feriado_um_juiz.py:367`, `:391`) | A partição resolve vínculo, certeza, cláusula e data da lavra pelo turno. Entre competências já na vigência, a entrada do motor usa o pareador existente para retirar da janela ampliada o turno fechado e distinto que pertence à competência vizinha. As duas lavras e o DIFF leem o resultado. Os casos de porta com troca de praça estão [nao verificado]. (`ponto/calculador/regras.py:412`, `ponto/services/fechamento.py:154`, `:226`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:361`, `ponto/tests/test_feriado_um_juiz_porta.py:455`) |
| **F** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_competencia_sem_feriado_conserva_arredondamento_da_base`. Executadas as atribuições dos dois hashes com 241 + 241 min, a base deu **8,04 h** e a c3 **8,03 h**. `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_delete_e_bulk_create_usam_a_mesma_transacao` mostrou a perda da linha anterior na c3. (`59600163c:app/ponto/services/fechamento.py:922`, `:1008`, `29488e3bb:app/ponto/services/fechamento.py:964`, `ponto/tests/test_feriado_um_juiz.py:13`) | Sem promoção, o mensal volta a **8,04 h**. Com promoção positiva, arredonda uma vez. O RED existente conserva as asserções e agora injeta uma promoção fracionária na fixture. As duas versões de `lavrar` executam `delete` e `bulk_create` na mesma transação. Rollback no banco [nao verificado]. (`ponto/services/fechamento.py:1008`, `ponto/tests/test_feriado_um_juiz_porta.py:396`, `:435`, `ponto/services/dia_pago.py:360`) |
| **G** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_l097_certeza_so_decide_rubrica_com_ou_sem_autorizacao` está com o `skip` literal. A chamada isolada ao motor deu **240/0 min** para incerta/certa sem autorização e **240/240 min** com autorização. (`ponto/tests/test_feriado_um_juiz_porta.py:499`, `ponto/motor_calculo_v2.py:1562`, `:1585`) | G fica pendente da decisão de lei. K1 conserva o `expectedFailure` e as asserções da c3. (`ponto/tests/test_feriado_um_juiz_porta.py:381`, `:499`) |

**Comandos a partir de `app/`**. `base_c3` carrega os fontes do hash em memória e exige falhas de asserção. Os cinco comandos com `--settings` dependem da pista com banco e ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:43`, `:169`, `:279`)

```bash
m() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes "$@"; }

m base_c3
m controle
m c4_cartao_rejulga_ciclo
m c4_contador_ignora_feriado
m c4_ignora_vigencia
m c4_alimentacao_rejulga_dobra
m c4_periodo_rejulga_dobra
m c4_split_bruto_do_noturno_longo
m c4_adicional_bruto_do_noturno_longo
m c4_lavra_na_data_herdada
m c4_certeza_no_dia_herdado
m c4_pad_conserva_turnos_externos
m c4_lavra_sem_atomic

m c4_fechamento_ignora_competencia --settings=config.settings.ci
m c4_mensal_le_noturno_bruto --settings=config.settings.ci
m c4_lavra_le_noturno_bruto --settings=config.settings.ci
m c4_troca_de_praca_le_vinculo_herdado --settings=config.settings.ci
m c4_arredonda_fora_da_promocao --settings=config.settings.ci
```

**Recorte para o Code** (`bin/suite.sh:295`)

```bash
bash ../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto folha relatorios escala core colaboradores api"
```

Medição dos 75 dias e 42 colaboradores, DIFF do TXT 11 de frota, reversão e apply L-092 [nao verificado].

## 27/11 16:04 · sessao ab2be411f222 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**B1 — Histórico HTMX.** O componente instala os listeners uma vez por documento e devolve o painel ao `details` antes do snapshot. O RED `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_historico_htmx_do_feriado_e_de_inverter_retratar` exercita Feriado e Inverter/Retratar, conta as inscrições após a segunda execução do script e verifica os snapshots e o DOM ao Voltar e Avançar. [popover-acao.js:28](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:28) [popover-acao.js:177](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:177) [teste:546](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:546)

**Prova antes da cura.** Li o blob de `c44771b4a` com `git show` e executei o JS duas vezes no Duktape, com DOM simulado. O base registrou **2 inscrições por evento e 1 órfão** no ponto do snapshot. A cura registrou **1 inscrição por evento e 0 órfãos**. Retirar a guarda voltou a duplicar as inscrições. Retirar o fechamento voltou a deixar o órfão. A reprodução em Chromium fica [nao verificado]. [popover-acao.js:160](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:160) [mutantes:77](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:77)

**B2 — Ficha real a 1366 px.** `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas` agora abre a rota da ficha, exige `colaboradores/detalhe.html` e espera o swap HTMX. A sonda verifica a coluna de 420 px e o `overflow:hidden` antes de medir viewport, recorte, ancoragem e submit. O MORDE `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_MORDE_painel_recortado_na_ficha_real` injeta recorte e exige que a sonda o acuse mesmo com o painel dentro da viewport. A leitura do hash-base confirmou a lacuna do cenário sintético. A mordida e a geometria da ficha real ficam [nao verificado], portanto a cura geométrica condicional segue em aberto. [ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) [ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) [sonda:235](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235) [MORDE:549](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:549) [rota:676](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:676)

A tentativa do RED por `manage.py` parou no import de `django.core`, antes de iniciar a suíte. O clique na tela de produção e a certificação pelo Code e pelo opus ficam [nao verificado]. `360 px` e `base_app` ficam na lista do pouso conforme o aval fornecido. [manage.py:11](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [cenários:641](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:641) [nao verificado]

**Inclusões e diff.** `base.html` e `base_app.html` são as duas inclusões diretas. Os consumidores são Feriado, Inverter/Retratar e Resolver dia. O diff tem **0 `except` e 0 `catch` adicionados ou removidos**. [base.html:440](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base.html:440) [base_app.html:194](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base_app.html:194) [grade:96](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:96) [grade:133](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:133) [grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138)

**Comandos dos mutantes para o Code**, a partir de `app/`, no ambiente de teste. Cada invocação abaixo deve falhar. [mutantes:72](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:72) [seleção:543](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:543) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
smoke=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
b1="$smoke.test_historico_htmx_do_feriado_e_de_inverter_retratar"
b2="$smoke.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas"

SMOKE_FERIADO_MUTANTE=historico-sem-guarda python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=historico-sem-fechar-painel python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=pagina-sem-ficha python3 manage.py test --settings=config.settings.ci "$b2"
SMOKE_FERIADO_MUTANTE=painel-recortado python3 manage.py test --settings=config.settings.ci "$b2"
```

**Recorte `--only` para o Code.** [suite.sh:26](/home/ronald/wt-codex-feriado-botao-cortado-112417/bin/suite.sh:26) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 ../bin/suite.sh \
  --dir /home/ronald/wt-codex-feriado-botao-cortado-112417 \
  --only "core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_selo_popover_acao core.tests.test_smoke_retratar_1366"
```

## 27/11 16:04 · sessao 5e1b420eecd8 · PAR-PELO-TURNO · construtor · origem=Code

A c1v14 está na cópia. O recorte em memória passou **65 testes**, e os vizinhos de `core` e `chamados` passaram **63 testes**. A execução canônica dos 18 registros e a medição dos tetos de **254/318** ficam **[nao verificado]**. O `--only` da cópia usa rede e a rota local usa Docker, ambos fora do escopo autorizado. `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:53`, `bin/suite.sh:320`, `bin/suite.sh:551`

### Grupos 1–4

| Grupo | Cura e prova |
|---|---|
| 1. Fixture dos REDs | Os cenários de fechamento usam um `Colaborador` real, sem salvar. Os nove casos chegaram às asserções originais e passaram em memória. Comparei por AST as 42 asserções dos três arquivos. O mutante `fixture` reproduziu `KeyError: 7` com o dublê que exige a FK. `app/ponto/tests/test_par_c1v8_reds.py:20`, `app/ponto/tests/test_par_pelo_turno_c1v10.py:30`, `app/ponto/tests/test_par_pelo_turno_c1v11.py:63`, `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:154` |
| 2. Data civil | `_data_do_turno` usa `data_local(saida.timestamp)` e preserva a condição da segunda virada. O selo intacto passou e rejeitou o mutante `data`, que repõe a conversão da c1v13. Folha e espelho também leem a geometria do pareador. `app/ponto/turnos.py:289`, `app/ponto/tests/test_contract_no_batida_date.py:232`, `app/ponto/services/fechamento.py:636`, `app/ponto/services/espelho.py:1035` |
| 3. Custo da escrita | A carga reutiliza o DNA da `CelulaDia`. Os três pontos de economia identificados no código são a recomposição do núcleo, a grade do cartório e a carga de `dias_encerrados`. Os REDs `dna` e `dna-grade` pegam a releitura e a perda do repasse. A atribuição das **+3 queries históricas** e os totais pós-cura são **[nao verificado]**. Os tetos seguem intactos. `app/ponto/nucleo.py:40`, `app/ponto/services/cartorio.py:1182`, `app/ponto/services/cartorio.py:1205`, `app/escala/utils.py:1957`, `app/ponto/turnos.py:1958`, `app/ponto/tests/test_par_pelo_turno_c1v14.py:19`, `app/ponto/tests/test_selo_chokepoint_escrita.py:491` |
| 4. Sem vínculo | O guarda usa o vínculo do dia julgado, já resolvido na segmentação. O selo legado devolveu `('S', 'orfao_14h', 'SAIU')` com persistência em memória. O RED com vínculo só na entrada passou. `cadastro` e `dono` derrubaram os respectivos testes. O juiz de chamados também lê `classificar_falta`. `app/ponto/turnos.py:2055`, `app/escala/servico_jornada.py:214`, `app/escala/tests/test_selo_b5c_universo_da_jornada.py:108`, `app/ponto/tests/test_par_c1v8_reds.py:202`, `app/chamados/juizes.py:746` |

### Grupo 5. Selos estruturais

| Selo | Caminho exigido pelo AST | Mutante |
|---|---|---|
| `falta_um_significado` | As duas emissões chegam à única aplicação da folha. A chamada preserva `agora` e `ate`, que faltavam no literal antigo. `app/relatorios/tests/test_falta_um_significado.py:144`, `app/relatorios/pdf_espelho.py:255` | `folha` aplica a contagem no PDF. |
| `pdf_le_o_juiz_do_dia` | PDF → espelho → marcas da janela → projeção por PK do dia julgado. A volta do agrupamento local continua vedada. `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:74`, `app/ponto/turnos.py:3049` | `dia` reintroduz `_dj` por data civil. |
| `f1c_contrato` | O PDF consome a grade retornada pelo espelho, que chama `grade_espelho_janela`. `app/escala/tests/test_f1c_contrato.py:42`, `app/ponto/services/espelho.py:1286` | `grade` monta uma grade própria. |
| `regua_do_periodo_vem_da_janela` | O PDF recebe `escala` e `tipo_escala` de `autoridade_do_periodo`, que consulta `vinculo_do_dia`. `app/ponto/tests/test_regua_do_periodo_vem_da_janela.py:274`, `app/ponto/services/espelho.py:1076` | `vinculo` escolhe a escala no PDF. |
| `turno_cruza_meia_noite` | O chamado continua consultando a regra do previsto. A completude lê `cross_meianoite` do par efetivo, inclusive na segunda virada. `app/escala/tests/test_turno_cruza_meia_noite.py:43`, `app/chamados/juizes.py:2324`, `app/ponto/turnos.py:332` | `noturno` compara horas no guarda e `chamado` repete `hf <= hi`. |

Cada selo passou na fonte correspondente e rejeitou seu mutante com `rc 1`, tanto antes da obra (`59600163c`) como na c1v14. Na catraca vizinha, atualizei exclusivamente as coordenadas de quatro registros. Três já divergiam na c1v13. O mutante `catraca` voltou a derrubar a igualdade de linhas. **`except` adicionados no diff e nos arquivos novos: 0.** `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:76`, `app/core/tests/test_medida_turno_do_par.py:292`, `app/core/tests/test_medida_turno_do_par.py:342`, `app/core/tests/test_medida_turno_do_par.py:554`

### Comandos

Em `app/`, recorte em memória e mutantes. O mutante `fixture` termina no erro de FK simulado. Os demais falham por asserção. `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:179`

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v14_verificacao "$@"
}
prova
for alvo in fixture data dna dna-grade cadastro dono catraca; do
  prova --alvo "$alvo" --mutante
done
for alvo in folha dia grade vinculo noturno chamado; do
  prova --alvo "$alvo" --antes
  prova --alvo "$alvo" --antes --mutante
  prova --alvo "$alvo"
  prova --alvo "$alvo" --mutante
done
```

Recorte canônico dos 18 registros e vizinhos para o Code **[nao verificado]**. `bin/suite.sh:26`

```bash
labels=(
  ponto.tests.test_par_c1v8_reds
  ponto.tests.test_par_pelo_turno_c1v10
  ponto.tests.test_par_pelo_turno_c1v11
  ponto.tests.test_par_pelo_turno_c1v12
  ponto.tests.test_par_pelo_turno_c1v13
  ponto.tests.test_par_pelo_turno_c1v14
  ponto.tests.test_par_pelo_turno_c1v6
  ponto.tests.test_contract_no_batida_date
  ponto.tests.test_selo_chokepoint_escrita
  ponto.tests.test_c1_dias_encerrados
  ponto.tests.test_regua_do_periodo_vem_da_janela
  escala.tests.test_selo_b5c_universo_da_jornada
  escala.tests.test_noturno_completo_intervalo
  escala.tests.test_sobra_na_borda_nao_vira_intervalo
  escala.tests.test_f1c_contrato
  escala.tests.test_turno_cruza_meia_noite
  relatorios.tests.test_falta_um_significado
  relatorios.tests.test_pdf_le_o_juiz_do_dia
  relatorios.tests.test_pdf_sem_regra_propria
  relatorios.tests.test_pdf_nao_calcula
  relatorios.tests.test_par_pelo_turno_c1v6_pdf
  chamados.tests.test_e3_c8b_sem_banco
  core.tests.test_medida_turno_do_par
)
PYTHONDONTWRITEBYTECODE=1 bash ../bin/suite.sh \
  --dir /home/ronald/wt-codex-par-pelo-turno-111717 \
  --only "${labels[*]}"
```

## 27/11 15:54 · sessao aff965f9ab2a · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**REPROVADO no recorte desktop de 1366 px.** Há dois bloqueios: o histórico HTMX pode restaurar um painel órfão no `body`, e o smoke de página mede o calendário numa página de ensaio sem o layout da ficha real. O drawer estabilizado tem prova geométrica no ensaio. O clique do Ronald na tela continua [nao verificado]. `app/static/js/popover-acao.js:115` `app/static/js/popover-acao.js:131` `app/core/tests/vendor/htmx-1.9.10.min.js:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:32` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:119` `app/templates/colaboradores/detalhe.html:87` `app/docs/LEIS.md:99`

A revisão foi só leitura, sobre a cópia e os anexos. O `Ran 9 OK` às 15:47 é o resultado informado. A seleção dos nove testes, suas medidas brutas, o clique no calendário real de 20/11 e a identidade Git da cópia, que veio sem `.git`, ficam [nao verificado]. [nao verificado]

### 1. Página, drawer e MORDEs

- **O diff implementa a cura nas duas superfícies.** A grade conserva os 132 px, põe o gatilho antes das marcações, dá rolagem ao conteúdo abaixo e permite quebrar o rótulo. A ficha e a aba Espelho recebem a mesma grade. O JS mede o gatilho e o painel, move o painel para o `body` e limita a posição à viewport e à altura visível do drawer. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/detalhe.html:215` `app/templates/core/_drawer_generico.html:134` `app/static/js/popover-acao.js:32` `app/static/js/popover-acao.js:45` `app/static/js/popover-acao.js:131`
- **A medida do Chromium é real nos cenários montados.** A fixture exige `trabalha=False`, oferta `True` e quatro batidas. A sonda faz hit-test no gatilho, chama `.click()` no alvo, espera o `toggle` e mede os retângulos. Cobra o painel dentro das quatro bordas com margem de 8 px, hit-test em cinco pontos, distância de até 16 px sem sobreposição de área e botão de submit atingível. No drawer, usa a aba Espelho real com respostas do Client Django e espera a transição terminar. Na página, injeta o fragmento direto no `main` e pula a grade `420px 1fr`, o contêiner com `overflow:hidden` e o carregamento HTMX da ficha. A geometria da ficha real a 1366 px fica [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:112` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:146` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:183` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:205` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:247` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:275` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:459` `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:202` `app/templates/colaboradores/detalhe.html:215`
- **Os cinco MORDEs automáticos do feriado têm falha específica.** `viewport-do-host` executa a 1365 e exige a divergência de largura. `gatilho-ausente` remove o `summary`. `drawer-fechado` fecha o drawer antes da medida. `excecao-na-medicao` exige a mensagem da exceção injetada. `painel-sobre-gatilho` exige painel ainda dentro da viewport, distância zero e ancoragem reprovada. Cada caso exige a falha correspondente em `_confere_medidas`. O de viewport verifica a sensibilidade à largura errada. Os demais mutantes opt-in, inclusive recorte, distância, foco, fechamento e encaixe lateral, têm execução individual [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:57` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:248` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:419` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:441` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:476` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:555`
- **Os selos vizinhos têm alcances distintos.** A varredura acusa o exemplo estrutural ruim. O MORDE de geometria exige estouro real de “Resolver dia” com `left:0` e sem o JS. O de Retratar exige que haja painel medido com a rota `/retratar/` e que nenhum painel medido estoure. A regressão unitária de quatro batidas cobra ordem, conteúdo e estilos do HTML. `app/core/tests/test_selo_popover_acao.py:88` `app/core/tests/test_selo_popover_acao.py:185` `app/core/tests/test_smoke_retratar_1366.py:167` `app/colaboradores/tests/test_feriado_botao_cortado.py:71`

### 2. Consumidores, fechamento e regressão HTMX

- **Inclusões diretas do script:** `app/templates/base.html:440` e `app/templates/base_app.html:194`. Os consumidores efetivos são **Tornar dia de trabalho**, **Inverter tipo / Retratar** e **Resolver dia / Cobrar este dia**. A grade entra no calendário mensal da ficha e do Espelho, na fita do drawer de diagnóstico e em `relatorios/furos.html`. Furos passa só `dias` e `resumo` com `only`, então recebe o novo invólucro da grade, sem os popovers de ação. `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/templates/colaboradores/partials/_calendario_grade.html:138` `app/templates/ponto/partials/_veredito_dia.html:17` `app/templates/ponto/partials/_veredito_dia.html:25` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/relatorios/furos.html:42`
- **Entradas do drawer:** lista de colaboradores, Benefícios, lista e Raio-X do Retrato da Folha, e a porta direta “Resolver dia no calendário” do Retrato. O diagnóstico em fita também entra pelo Painel Situacional e por Pendências de Disputa. `app/templates/colaboradores/partials/_lista_tabela.html:16` `app/templates/colaboradores/beneficios_lista.html:86` `app/templates/folha/_retrato_lista.html:61` `app/templates/folha/_retrato_raiox.html:68` `app/folha/retrato_folha.py:1019` `app/templates/folha/_retrato_o_que_fazer.html:73` `app/templates/colaboradores/partials/_ps_linha.html:4` `app/templates/colaboradores/partials/pendencias_disputa.html:9`
- **Fechar e substituir fragmento.** `fecharDrawer()` emite `drawer:fechou`. O JS devolve o painel ao pai, restaura estilo e `tabindex`, desliga o observador e evita devolver foco ao gatilho do drawer que fecha. O smoke cobra isso. Num `outerHTML` do calendário ou `innerHTML` do drawer que remove o gatilho, o observador fecha o painel original. Os sete listeners são instalados por execução do script, fora de `abre()` e `posiciona()`. A troca por HTMX fica [nao verificado] em navegador. `app/templates/core/_drawer_generico.html:17` `app/templates/core/_drawer_generico.html:71` `app/templates/colaboradores/calendario_partial.html:42` `app/static/js/popover-acao.js:101` `app/static/js/popover-acao.js:140` `app/static/js/popover-acao.js:156` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:342`
- **Bloqueio — Voltar/Avançar no histórico HTMX.** A paginação da lista usa `hx-push-url` e a mesma lista abre o drawer do colaborador. Com um popover aberto, o HTMX guarda o HTML do `body`, incluindo o painel `fixed` separado do `details`. Ao restaurar esse snapshot, o observador só conhece o nó original. A cópia no `body` fica sem dono e fora do alcance de `fecha()`. A restauração também recria os scripts do `body`. Cada nova execução do componente instala outro conjunto de listeners globais, pois a inicialização carece de guarda. É uma regressão possível também em **Resolver dia** e **Inverter/Retratar**. O caminho é demonstrável pelo código. A reprodução em navegador fica [nao verificado]. `app/templates/colaboradores/partials/_lista_tabela.html:16` `app/templates/colaboradores/partials/_lista_tabela.html:54` `app/templates/base.html:19` `app/templates/base.html:440` `app/core/tests/vendor/htmx-1.9.10.min.js:1` (`Zt`, `Qt`, `er`, `ar`, `Ue`, `Nt`) `app/static/js/popover-acao.js:101` `app/static/js/popover-acao.js:108` `app/static/js/popover-acao.js:131` `app/static/js/popover-acao.js:140` `app/static/js/popover-acao.js:156`

### 3. Porta de escrita

O formulário mantém o POST nativo para `ponto:feriado_vira_trabalho`, com token CSRF e middleware. A view exige login, POST e `tem_acao(..., 'editar_escalas')`, valida os campos e relê a mesma oferta do calendário. A escrita passa por `editar_dia(..., so_se_folga=True)`, que confere `trabalha is False` sob trava e grava célula e trilha no mesmo `atomic`. O template só usa os dois predicados recebidos para renderizar e formatar a célula. O smoke mede o submit e confere método e rota. A submissão pelo Chromium fica [nao verificado]. `app/templates/ponto/partials/_feriado_vira_trabalho.html:7` `app/config/settings/base.py:58` `app/ponto/urls.py:59` `app/ponto/views.py:3584` `app/ponto/views.py:3607` `app/ponto/views.py:3622` `app/ponto/portas/celula.py:642` `app/ponto/portas/celula.py:665` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:275`

### 4. Lista do pouso — `base_app` e 360 px [nao verificado]

- **Ficha a 360 px:** a primeira coluna fixa de 420 px já excede a largura da tela. O calendário começa na segunda coluna, fora do enquadramento inicial. `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:201`
- **Drawer e `base_app` a 360 px:** a grade mensal conserva sete colunas e células de 132 px. Recorte efetivo, ancoragem e acionabilidade a 360 px ficam [nao verificado]. `base_app` tem cenários sintéticos a 1366, com o bootstrap PWA desativado. A ficha real estende `base.html`. Ficam na lista do pouso conforme o aval anexado, sem pedido de nova volta por esses itens. `app/templates/colaboradores/partials/_calendario_grade.html:64` `app/templates/colaboradores/detalhe.html:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:537`

### 5. Quem escreve e quem lê

| Campo ou valor | Escritor → leitores |
|---|---|
| `feriado_vira_trabalho_oferecido` | `contexto_calendario` compõe feriado aplicável + célula existente com `trabalha=False`. A grade, a view POST e o smoke leem o mesmo campo. `app/colaboradores/services/calendario.py:343` `app/colaboradores/services/calendario.py:791` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/ponto/views.py:3624` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:467` |
| Feriado e `CelulaDia.trabalha` | Configuração/importação gravam `Feriado` e `feriados_do_colab` fornece as datas ao calendário. Geração, regeneração, reversão e edição escrevem o estado da célula pela porta. O calendário lê a célula carregada e a edição relê sob trava. `app/core/views_config.py:60` `app/core/views_config.py:144` `app/core/feriados.py:135` `app/ponto/portas/celula.py:293` `app/ponto/portas/celula.py:642` `app/ponto/portas/celula.py:866` `app/ponto/portas/celula.py:952` `app/colaboradores/services/calendario.py:791` |
| `pode_editar_dia`, `pode_veredito`, `volta_url` | View do calendário e serviço do drawer de diagnóstico montam o contexto a partir de `tem_acao` e da rota da ficha. A grade consome as flags e repassa a URL aos formulários. A view POST consulta a permissão novamente. `app/colaboradores/mixins.py:322` `app/colaboradores/views.py:1992` `app/colaboradores/views.py:2007` `app/colaboradores/services/drawer.py:131` `app/colaboradores/services/drawer.py:156` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/ponto/views.py:3607` |
| `layout`, `dias`, `data`, `cal_offset`, `status`, `veredito_dia`, `palavra_dia`, marcações, badges e oferta de Resolver dia | Os chamadores passam `mes`/`fita`. O serviço compõe as projeções e a grade as lê. A porta de batida grava `pk`/`tipo`/`timestamp` na criação e o flip altera `tipo` via `anotar`. A grade e o smoke leem esses valores. `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/colaboradores/services/calendario.py:639` `app/colaboradores/services/calendario.py:754` `app/colaboradores/services/calendario.py:809` `app/colaboradores/services/calendario.py:838` `app/ponto/registro_batida.py:170` `app/ponto/services/flip_batida.py:41` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469` |
| POST `colab`, `dia`, `volta`, `motivo`, CSRF | O partial escreve os três valores ocultos e renderiza o token. O administrador preenche `motivo`. Middleware lê CSRF, a view lê e valida os campos, `resposta_acao` recebe a volta saneada e a porta recebe o motivo. `app/templates/ponto/partials/_feriado_vira_trabalho.html:8` `app/config/settings/base.py:58` `app/ponto/views.py:3584` `app/ponto/views.py:3598` `app/ponto/views.py:3614` `app/ponto/views.py:3632` |
| Escrita indireta do POST | `editar_dia` grava `trabalha`, `dna`, `escala_geradora`, `origem`, `editada_em` e `editada_por`. A porta lê o antes/depois e registra autor, ato e motivo via `evento_kw`. Calendário lê `trabalha`, o juiz do vínculo lê a geradora, e cartório, fase e veredito leem `editada_em`. `app/ponto/portas/celula.py:569` `app/ponto/portas/celula.py:641` `app/ponto/portas/celula.py:658` `app/core/observ.py:134` `app/colaboradores/services/calendario.py:793` `app/escala/alimentacao.py:228` `app/ponto/services/cartorio.py:411` `app/escala/services/fase_12x36.py:190` `app/chamados/veredito_lavrado.py:194` |
| Estado do painel, evento e medidas | O navegador fornece retângulos, área, foco e estilos computados. `abre` guarda `atual`, pai/irmão, estilo e `tabindex`. `posiciona` escreve o CSS temporário e `fecha` restaura. O drawer escreve `display`/`transform` e emite o evento que o JS lê. A sonda gera as medidas, o driver CDP as transporta e `_confere_medidas` as valida. `app/static/js/popover-acao.js:32` `app/static/js/popover-acao.js:106` `app/static/js/popover-acao.js:140` `app/templates/core/_drawer_generico.html:71` `app/static/js/popover-acao.js:168` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:98` `app/core/tests/smoke_feriado_chromium.js:44` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` |

### 6. Classes de reprovação

**Segundo juiz, segundo escritor, relógio fora da borda, derivação paralela de negócio e campo com dois significados:** zero introduzidos no diff fornecido. Oferta e permissão continuam com os donos acima, o `agora` do POST sai uma vez da view e segue para calendário e edição, e a mudança de produção escreve estado de apresentação. **Selo sem mordida:** os cinco controles têm falhas específicas. A lacuna é de cobertura da ficha real e do histórico HTMX. A trava de `so_se_folga` permanece na porta, e o smoke humano pré-pouso está [nao verificado]. `app/colaboradores/services/calendario.py:791` `app/ponto/views.py:3607` `app/ponto/views.py:3622` `app/ponto/portas/celula.py:642` `app/static/js/popover-acao.js:115` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:32` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:441` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:555` `app/docs/LEIS.md:99`

### 7. `except` no diff

**0 cláusulas Python `except` adicionadas, 0 removidas.** Também há **0 `catch` JavaScript adicionados ou removidos**. A rejeição da sonda e as falhas do driver viram `motivo`, que a asserção exige ser nulo. O `catch` de `htmx.process` no drawer já existia. `app/colaboradores/tests/test_feriado_botao_cortado.py:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:356` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` `app/core/tests/test_selo_popover_acao.py:124` `app/core/tests/test_smoke_retratar_1366.py:54` `app/core/tests/smoke_feriado_chromium.js:68` `app/templates/core/_drawer_generico.html:112`

## 27/11 15:44 · sessao 4c95a5211276 · PAR-PELO-TURNO · leitura · origem=Code

Comparei a cópia `84f8a88d4` com a cópia local da base `59600163c`, arquivo a arquivo. As linhas abaixo são do HEAD, salvo quando marco `base`. Os 18 registros do anexo correspondem a **17 métodos**, porque `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` falhou em dois subtestes. A execução da suíte na base é [nao verificado]. `app/ponto/tests/test_par_c1v8_reds.py:175–186`

### 9 ERROR — fixture compartilhada da obra

Os três arquivos de teste foram adicionados pela obra e usam `_ler_fechamento`. A fixture fornece um `SimpleNamespace(pk=7, id=7)` e força a busca do fechamento gravado a retornar `None`. O serviço então tenta construir `FechamentoMensal(colaborador=colab, ...)`, cuja FK exige uma instância de `Colaborador`. Essa incompatibilidade está na fixture nova. A construção já existia na base. `app/ponto/tests/test_par_c1v8_reds.py:24,67–100` · `app/ponto/models.py:398–407` · `app/ponto/services/fechamento.py:885–890` · `base:app/ponto/services/fechamento.py:910–915` · `app/ponto/tests/test_par_pelo_turno_c1v10.py:10` · `app/ponto/tests/test_par_pelo_turno_c1v11.py:11`

O `KeyError: 7` do anexo sai do `[colab.pk]` aplicado ao dicionário retornado. O serviço captura exceções por colaborador e pode devolver o dicionário sem preencher essa chave. O anexo mostra essa consequência em todos os nove registros. A primeira exceção capturada na pista é [nao verificado]. **Classificação dos nove: erro de fixture antes das asserções listadas.** A cura demonstrável é alimentar a FK com um `Colaborador` compatível, preservando os REDs. `app/ponto/tests/test_par_c1v8_reds.py:95–100` · `app/ponto/services/fechamento.py:1133–1135,1302–1309`

| Teste | Ponto do erro e expectativa que ficou sem julgamento | [nao verificado]
|---|---|
| `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` — 23/10 | Chamada na linha 183. A competência deve ter `0,0 h` e saldo `0,0` com e sem o par posterior. `app/ponto/tests/test_par_c1v8_reds.py:175–186` |
| `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` — 25/10 | Mesma chamada e mesmos valores, com o fato posterior além do contexto do mapa. `app/ponto/tests/test_par_c1v8_reds.py:173–186` |
| `test_3b_o_ultimo_dia_com_marca_e_movimento_da_competencia` | Chamada na linha 196. Espera **1 turno aberto** e saldo **menor que zero**. `app/ponto/tests/test_par_c1v8_reds.py:194–198` |
| `test_5_a_folha_preserva_as_oito_horas_em_21_de_outubro` | Chamada na linha 249. Espera **8,0 h** em 21/10. `app/ponto/tests/test_par_c1v8_reds.py:241–250` |
| `test_a_folha_fecha_o_par_na_segunda_virada_sem_levar_as_marcas_vizinhas` | Chamada na linha 41. Espera **26,0 h**, **0 abertos**, só os PKs `[1, 2]` e ambos atribuídos a `DIA`. `app/ponto/tests/test_par_pelo_turno_c1v10.py:30–46` |
| `test_retratacao_apos_a_leitura_conserva_o_mesmo_conjunto_na_folha` | Chamada na linha 75. Espera uma leitura `[[1, 2]]`, **2,0 h** e **0 abertos**. `app/ponto/tests/test_par_pelo_turno_c1v10.py:48–78` |
| `test_a_segunda_virada_chega_separadamente_a_cada_leitor` | Chamada na linha 177. Espera `[1, 2]` na fatia da folha e também na leitura independente do espelho. `app/ponto/tests/test_par_pelo_turno_c1v11.py:166–182` |
| `test_fatia_recebe_os_dois_pks_de_um_par_julgado_em_d` | A asserção da carga inicial vem antes da chamada que falha na linha 91. No fechamento, espera `[1, 2]` no motor, **26,0 h** e **0 abertos**. `app/ponto/tests/test_par_pelo_turno_c1v11.py:79–94` |
| `test_universo_da_porta_na_folha_e_no_espelho` | Chamada na linha 72. Com a porta vazia, espera **0,0 h**, nenhuma marca no espelho e duas chamadas à porta com os mesmos limites. `app/ponto/tests/test_par_pelo_turno_c1v11.py:63–77` |

### 2 FAIL — teto de consultas no fechamento de chamados pela batida

| Teste | Falha observada | [nao verificado]
|---|---|
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | O chamado fechou. **257 consultas**, teto **254**. `app/ponto/tests/test_selo_chokepoint_escrita.py:218,343–354,491–494` |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | Os três chamados fecharam. **321 consultas**, teto **318**. `app/ponto/tests/test_selo_chokepoint_escrita.py:254,343–354,496–499` |

**Testes e tetos idênticos na base. A obra alterou o caminho medido.** O `post_save` da batida recompõe turnos via `turnos_do_colab`. O cartório também chega a essa função por `dias_encerrados`. No HEAD, a carga lê as batidas por limites de instante e usa a janela de contexto alterada. O anexo comprova `+3` nos dois cenários. A origem de cada consulta extra e o total da base sob o mesmo ambiente são [nao verificado]. **Os selos estão certos. A correção é no custo do caminho de escrita**, com os tetos atuais como referência. `app/ponto/tests/test_selo_chokepoint_escrita.py:480–499` · `app/ponto/signals.py:19–36,85–93,137–140` · `app/ponto/nucleo.py:40–47` · `app/ponto/services/cartorio.py:1182–1186` · `app/ponto/turnos.py:1885–1909,2213–2215` · `base:app/ponto/turnos.py:1850–1876` · `PLANO_PISCADA.md:8–15`

### 4 FAIL — selos estruturais do PDF

Os quatro testes já existiam na base e estão idênticos no HEAD. A obra mudou o coletor do PDF para chamar `espelho_do_colab(..., janela_exata=True)`. As falhas abaixo são de expectativas sobre a forma do código. Cada contrato continua rastreável no caminho delegado. `base:app/relatorios/pdf_espelho.py:187–194,307,543,628` · `app/relatorios/pdf_espelho.py:178–182,221–245,255`

| Teste | Causa no HEAD e contrato | [nao verificado]
|---|---|
| `test_MORDE_ha_UM_sitio_que_aplica_a_folha_e_os_dois_caminhos_passam_por_ele` | O `assertIn` procura a chamada literal com quatro argumentos. A chamada única agora passa também `agora=agora, ate=agora`. Os dois modos de emissão chegam ao mesmo coletor, e `folha_manda` continua com **um** ponto de aplicação dos totais. A expectativa textual ficou desatualizada. `app/relatorios/tests/test_falta_um_significado.py:157–178` · `app/relatorios/pdf_espelho.py:255,277,663,710` · `app/relatorios/cartao_pela_celula.py:346` |
| `test_MORDE_o_pdf_consome_o_juiz_e_nao_regra_propria` | O `assertIn('dia_das_batidas', fonte)` perdeu as menções que já eram comentários na base. No HEAD, o PDF recebe as linhas do espelho. O espelho agrupa as marcas pelo mapa de PKs de `marcas_da_janela_para_exibicao`, produzido pelo pareador. A regra continua sendo **imprimir no dia julgado para o PK**. `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:84–96` · `base:app/relatorios/pdf_espelho.py:484,535,613` · `app/relatorios/pdf_espelho.py:221–245` · `app/ponto/services/espelho.py:586–591,1243–1245` · `app/ponto/turnos.py:3044–3061` |
| `test_renders_usam_o_contrato` | O contador da string `grade_espelho_janela` no PDF caiu de **1 para 0**, ante mínimo **1**. A chamada está no ramo `janela_exata` do espelho e o PDF consome a grade retornada. A exigência de usar a grade compartilhada continua satisfeita pelo caminho delegado. `app/escala/tests/test_f1c_contrato.py:42–44,68–70` · `base:app/relatorios/pdf_espelho.py:193,543` · `app/ponto/services/espelho.py:1283–1287,1322–1325` · `app/relatorios/pdf_espelho.py:221–222,240–244` |
| `test_RED_os_oito_pedem_o_vinculo_ao_juiz_da_casa` | A busca AST aceita duas chamadas diretas no corpo do coletor. A base tinha `vinculo_do_dia_carregado`. No HEAD, `_espelho` chama `autoridade_do_periodo`, que chama `vinculo_do_dia`, e o coletor usa `autoridade.esc` e `autoridade.tipo_escala`. O PDF aparece em `calados` pelo alcance da busca. O contrato do RED continua sendo **todos os leitores consultarem a autoridade do vínculo**. `app/ponto/tests/test_regua_do_periodo_vem_da_janela.py:197–221,260–272,300–309` · `base:app/relatorios/pdf_espelho.py:307` · `app/ponto/services/espelho.py:1076–1077,1242` · `app/relatorios/pdf_espelho.py:221–224,261–262` |

### 2 FAIL — par noturno completo e cadastro

- **`test_SEM_escala_o_veredito_NAO_muda`** — Teste idêntico na base. A fixture cria **E 18:52 e S 06:52 do dia seguinte**, sem criar vínculo. Na base, o guarda retornava `False` sem escala e a classificação seguia para o fallback que espera `('S', 'orfao_14h', 'SAIU')`. A obra antecipou o guarda e passou a silenciar pelo par fechado que cruza o dia. A pista recebeu `(None, None, None)`. **Mudança de domínio causada pela obra. Pelo contrato disponível, a cura é no código para o caso inteiramente sem vínculo.** O RED da obra que aceita ausência de vínculo **na saída** cria vínculo para a entrada. Revogação do contrato legado para o caso inteiramente sem escala é [nao verificado]. `app/escala/tests/test_selo_b5c_universo_da_jornada.py:108–117` · `base:app/escala/servico_jornada.py:232–239,361–366,490–492` · `app/escala/servico_jornada.py:214–220,318–322` · `app/ponto/tests/test_par_c1v8_reds.py:200–221`
- **`test_o_juiz_do_chamado_e_o_noturno_completo_perguntam_a_regra`** — Teste idêntico na base. Falha a segunda busca literal por `turno_cruza_meia_noite(hi, hf)`. A primeira, no juiz do chamado, passou. A obra substituiu a checagem do horário previsto em `_turno_noturno_completo` pela leitura de `Turno.cross_meianoite` e pelo intervalo inclusivo `data_turno <= data_evento <= data_saida`. **A segunda expectativa estrutural ficou desatualizada.** O RED da obra exige `True` para o par fechado na segunda virada com vínculo noturno, diurno ou ausente na saída. `app/escala/tests/test_turno_cruza_meia_noite.py:43–48` · `app/chamados/juizes.py:2316–2324` · `base:app/escala/servico_jornada.py:234–239` · `app/escala/servico_jornada.py:214–220` · `app/ponto/turnos.py:318–334` · `app/ponto/tests/test_par_c1v8_reds.py:200–221`

### 1 FAIL — conversão de data dentro do pareador

- **`test_nenhum_ofensor_novo`** — Teste idêntico na base. A obra adicionou `fim = timezone.localtime(saida.timestamp)` seguido de `fim.date() > d` em `_data_do_turno`. O visitante AST rastreia variáveis atribuídas a partir de `.timestamp` e marca o `.date()`. Aplicado aos dois arquivos, o detector aponta essa linha apenas no HEAD. **O selo está certo para a forma que protege. A cura é no código**, usando `data_local` para a data civil da saída e preservando a condição da segunda virada. O valor esperado de `novos` continua `set()`. `app/ponto/tests/test_contract_no_batida_date.py:184–216,232–235` · `base:app/ponto/turnos.py:248–322` · `app/ponto/turnos.py:44–47,287–301`

## 27/11 15:35 · sessao 0c713d148d50 · INVENTARIO-PELA-REGRA · construtor · origem=Code

Corrigi o instrumento do AUS-05. `_EmissorMudo._espelhar_marcos` tem a assinatura do emissor real e retorna `False`, o valor do caminho sem escrita. A comparação com o HEAD confirmou que os 35 casos C2 receberam somente mudanças de docstring, preservando as asserções de ESC-12, AUS-03 e FOL-14. [app/folha/tests/_celulas.py:11] [app/ponto/management/commands/detectar_ausencias.py:544] [app/ponto/management/commands/detectar_ausencias.py:558]

A bateria ficou com **50 casos etiquetados**. CMP-06 a CMP-11 e CMP-15 têm `@unittest.skip("lei: ...")` e duas respostas candidatas. As frentes da tabela são escolhas técnicas de leitura, sujeitas à revisão do Code. A menção a intermitente não veio acompanhada de cadastro, batidas e gabarito entre os 15 casos do L4. [app/folha/tests/_inventario_c3_mutantes.py:74] [app/folha/tests/test_inventario_competencia.py:247] [app/folha/tests/test_inventario_competencia.py:408] [nao verificado]

O RED isolado do AUS-05 reproduziu o `AttributeError` ao retirar o método em memória. O RED do predicado de rescisão apontou CMP-12 em 01/2027, CMP-13 em 01/2027 e CMP-14 em 12/2026. Para o movimento normal, os testes cobram matrícula, competência e valor em centésimos de hora. O L4 não informa o código dessa rubrica, e o escritor atual não oferece o evento de horas normais. A execução Django do recorte fica com Code e opus final. [app/folha/tests/_inventario_c3_mutantes.py:45] [app/folha/tests/_inventario_c3_mutantes.py:146] [app/folha/tests/test_inventario_competencia.py:132] [app/folha/export.py:439] [nao verificado] [nao verificado]

### Diff por arquivo

| Arquivo | Linhas `+/-` | Mudança |
|---|---:|---|
| [folha/tests/_celulas.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/_celulas.py:11) | +3/−0 | Método mudo |
| [ponto/tests/test_inventario_escala.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) | +36/−12 | 12 docstrings |
| [ponto/tests/test_inventario_ausencia.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) | +27/−9 | 9 docstrings |
| [folha/tests/test_inventario_folha.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130) | +42/−14 | 14 docstrings |
| [folha/tests/test_inventario_competencia.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_competencia.py:1) | +444/−0 | 15 casos CMP |
| [folha/tests/_inventario_c3_mutantes.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/_inventario_c3_mutantes.py:1) | +185/−0 | Ensaios em memória |

**`except` no diff: 0**, contando inclusões e remoções nos seis arquivos acima.

### RED e mutantes

Na pasta `app`, esta função invoca os ensaios sem ORM. [app/folha/tests/_inventario_c3_mutantes.py:159]

```bash
c3() { PYTHONDONTWRITEBYTECODE=1 python3 folha/tests/_inventario_c3_mutantes.py "$1"; }
```

| Comando | Resultado observado | Referência |
|---|---|---|
| `c3 controle` | rc 0 | `app/folha/tests/_inventario_c3_mutantes.py:174` |
| `c3 red-emissor` | rc 1, método ausente na chamada do cartório | `app/folha/tests/_inventario_c3_mutantes.py:49` |
| `c3 sem-palavra-chave` | rc 1, rejeita `reaberto=` | `app/folha/tests/_inventario_c3_mutantes.py:51` |
| `c3 retorno-true` | rc 1, sinaliza escrita indevida | `app/folha/tests/_inventario_c3_mutantes.py:54` |
| `c3 sem-etiqueta` | rc 1, acusa a etiqueta ausente em CMP-06 | `app/folha/tests/_inventario_c3_mutantes.py:84` |
| `c3 sem-skip` | rc 1, acusa o `skip` ausente em CMP-06 | `app/folha/tests/_inventario_c3_mutantes.py:96` |
| `c3 corte-trocado` | rc 1, acusa 20/11 atribuído a dezembro | `app/folha/tests/_inventario_c3_mutantes.py:118` |
| `c3 movimento-outra-competencia` | rc 1, acusa movimento na competência errada | `app/folha/tests/_inventario_c3_mutantes.py:139` |
| `c3 movimento-minutos-a-menos` | rc 1, encontra 1100 centésimos onde espera 1200 | `app/folha/tests/_inventario_c3_mutantes.py:140` |
| `c3 movimento-em-feriado` | rc 1, rejeita o valor na rubrica de feriado | `app/folha/tests/_inventario_c3_mutantes.py:141` |
| `c3 red-rescisao` | rc 1, acusa as três exclusões pelo predicado real | `app/folha/tests/_inventario_c3_mutantes.py:146` |

### Recorte para o Code

Na raiz da cópia, com os quatro módulos dentro de `--only`. Execução neste turno [nao verificado]. [app/docs/RELATO.md:10801]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only "ponto.tests.test_inventario_escala ponto.tests.test_inventario_ausencia folha.tests.test_inventario_folha folha.tests.test_inventario_competencia"
```

### Etiquetas

| Classe | Família | Frente | Nascente |
|---|---|---|---|
| ESC-01 | escala | espelho | `app/escala/utils.py:2284` |
| ESC-02 | escala | espelho | `app/escala/utils.py:2284` |
| ESC-03 | escala | espelho | `app/escala/alimentacao.py:228` |
| ESC-04 | escala | espelho | `app/ponto/turnos.py:2971` |
| ESC-05 | escala | espelho | `app/ponto/motor_calculo_v2.py:1049` |
| ESC-06 | escala | espelho | `app/ponto/motor_calculo_v2.py:961` |
| ESC-07 | escala | espelho | `app/ponto/precedencia.py:259` |
| ESC-10 | escala | espelho | `app/ponto/motor_calculo_v2.py:3535` |
| ESC-11 | escala | espelho | `app/ponto/motor_calculo_v2.py:3538` |
| ESC-12 | escala | espelho | `app/ponto/services/dia_pago.py:239` |
| ESC-13 | escala | espelho | `app/ponto/services/dia_pago.py:250` |
| ESC-15 | escala | espelho | `app/ponto/services/espelho.py:1200` |
| AUS-01 | ausencia_ferias | espelho | `app/ponto/motor_calculo_v2.py:3596` |
| AUS-02 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:396` |
| AUS-03 | ausencia_ferias | espelho | `app/escala/utils.py:1262` |
| AUS-04 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:193` |
| AUS-05 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:346` |
| AUS-06 | ausencia_ferias | ferias | `app/ferias/services.py:504` |
| AUS-07 | ausencia_ferias | ferias | `app/folha/export.py:90` |
| AUS-08 | ausencia_ferias | ferias | `app/ferias/services.py:49` |
| AUS-10 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:396` |
| FOL-01 | folha_export | txt | `app/folha/export.py:439` |
| FOL-02 | folha_export | txt | `app/folha/export.py:441` |
| FOL-03 | folha_export | txt | `app/folha/export.py:446` |
| FOL-04 | folha_export | txt | `app/folha/export.py:443` |
| FOL-05 | folha_export | txt | `app/folha/export.py:447` |
| FOL-06 | folha_export | espelho | `app/ponto/motor_calculo_v2.py:3603` |
| FOL-07 | folha_export | txt | `app/folha/export.py:215` |
| FOL-08 | folha_export | txt | `app/folha/export.py:752` |
| FOL-09 | folha_export | txt | `app/folha/export.py:399` |
| FOL-10 | folha_export | txt | `app/folha/export.py:739` |
| FOL-11 | folha_export | txt | `app/ponto/janelas.py:15` |
| FOL-12 | folha_export | txt | `app/ponto/services/dia_pago.py:109` |
| FOL-13 | folha_export | txt | `app/folha/export.py:525` |
| FOL-14 | folha_export | txt | `app/folha/views.py:277` |
| CMP-01 | escala | espelho | `app/ponto/janelas.py:15` |
| CMP-02 | escala | txt | `app/folha/export.py:443` |
| CMP-03 | escala | txt | `app/ponto/services/fechamento.py:979` |
| CMP-04 | escala | espelho | `app/ponto/motor_calculo_v2.py:3603` |
| CMP-05 | escala | espelho | `app/ponto/motor_calculo_v2.py:3811` |
| CMP-06 | escala | txt | `app/core/regua_cct.py:477` |
| CMP-07 | escala | txt | `app/ponto/motor_calculo_v2.py:961` |
| CMP-08 | escala | txt | `app/ponto/motor_calculo_v2.py:961` |
| CMP-09 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-10 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-11 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-12 | folha_export | txt | `app/folha/export.py:875` |
| CMP-13 | folha_export | txt | `app/folha/export.py:875` |
| CMP-14 | folha_export | txt | `app/folha/export.py:875` |
| CMP-15 | ausencia_ferias | ferias | `app/folha/export.py:90` |

## 27/11 15:22 · sessao 92a042f4758d · FERIADO-UM-JUIZ · verificador · origem=Code

**REPROVADO.** A c2/c3 corrige a emissão da HE100 de feriado no TXT da 11 quando o vínculo termina em 12x36. Ainda há quatro bloqueios de dinheiro: K1 continua pagando a dobra sobre 470 minutos, o recálculo genérico pode mudar exportadas fora da guarda, o resumo do cartão pode perder as 4 h que o TXT emite e a lavra `oraculo` ainda pode divergir em feriado de trabalho normal após troca de vínculo. (`app/folha/export.py:744–749`, `app/ponto/services/fechamento.py:564–600,948–965`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286`, `app/relatorios/cartao_pela_celula.py:89–101`, `app/ponto/calculador/alimentacao.py:423–446,559–577`)

## Aval literal e alcance

- **A promoção principal está implementada.** Para cada período de folga trabalhada, `particionar_ft` usa o motor que apurou a fatia e consulta `_feriado_dobra_no_dia` pelo dia do turno do par. Em 6x1, com dobra devida, escala incerta vai para HE100 feriado. Com escala certa, `feriado_vira_folga=True` vai para HE100 feriado e `False` conserva folga trabalhada, também a 100%. Sem dobra, a partição manda a folga certa para a rubrica de folga e a incerta para trabalhadas. O corte por ciclo mantém 12x36 fora da promoção, e o corte por base ou ciclo exclui intermitentes. O diff preserva o arquivo do motor, o conjunto `CICLOS_FERIADO_SIMPLES` e a cláusula de CCT já usada no caminho regular do 12x36. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `diff.txt:1`, `app/ponto/calculador/regras.py:366–414`, `app/ponto/motor_calculo_v2.py:941–978,3740,3764–3767,3810–3822`, `app/core/regua_cct.py:473–479`, `app/folha/export.py:445–452`)
- **“Escala certa só escolhe a rubrica” vale dentro da partição.** Antes dela, a L-097 ainda pode zerar uma FT certa sem autorização, com previsto zero e empresa na janela. Com os mesmos 240 min de entrada, o bloqueio preservou 240 min para escala incerta e zerou o caso de escala certa na reprodução em memória. A incidência nos 75 dias é [nao verificado]. **RED:** passar a mesma FT 6x1 pela porta, com e sem autorização, variando apenas a certeza da escala. Se o aval exigir a mesma quantidade também antes da partição, há uma decisão de lei sobre a L-097. (`app/ponto/motor_calculo_v2.py:1562–1597,2558–2574`, `app/ponto/calculador/regras.py:389–409`, `CLAUDE.md:274–283`)
- **O motor por fatia já existia.** O acréscimo de `apurar_por_fatia` é guardar `(motor, vínculo)` por identidade do período FT. Esse vínculo com a origem cabe no aval. Também cabe ajustar a emissão e a homologação da 11 para transportar a rubrica já classificada. Há deltas além do literal: o arredondamento muda `horas_trabalhadas` em mês sem feriado, a alimentação da lavra `oraculo` muda também em dias normais de intermitentes e a promoção do fechamento alcança competências anteriores à 11. O TXT lê a partir de 11 qualquer HE100 feriado positiva do mensal, inclusive em um mensal 12x36 sem informação de proveniência dessa parcela. O comportamento inscrito no motor para 12x36 permanece, mas a camada de emissão mudou. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:299–342,948–970,1246–1276`, `app/ponto/calculador/alimentacao.py:456,529–570`, `app/folha/export.py:383–385,425–428,744–749,1023–1028`, `CLAUDE.md:274–277`)

## Laudo c1, achado por achado

| Achado da c1 | Situação em c2/c3 e RED |
|---|---|
| **1. TXT perde a primeira fatia ou retém em `cadastro_zero`.** | **Curado na montagem e homologação da 11 em diante.** Há testes para 6x1 → 12x36 e para só 4 h de FT promovida, com previsto e `horas_trabalhadas` zerados e rubrica homologada. Eles passam por classificação, montagem e serialização, com fechamento gravado e calculado em leitura. **Procedente no resumo:** reproduzimos em memória TXT `[400]`, HE100 bruta `4,00` e `total_extras=0,00` no cartão. O app e a ficha repassam esse total. O contador `cartao_x_txt` compara HE100 bruta, deixando a rubrica feriado e `total_extras` fora da comparação. **RED:** no mesmo caso, exigir 4 h no resumo e na linha 0200, e fazer o contador acusar supressão da rubrica com HE100 bruta igual. (`app/folha/export.py:744–757,921–923,995–1000,1023–1037`, `app/ponto/tests/test_feriado_um_juiz_porta.py:164–177,215–227`, `app/relatorios/cartao_pela_celula.py:89–101,249–274,320–346`, `app/api/views.py:1660–1669`, `app/colaboradores/services/ficha.py:198–205`) |
| **2. Exportada fora da guarda pode mudar.** | **Procedente.** A guarda específica cobre setembro de 2026. Fora dela, a L-113 permite o recálculo genérico sob suas condições. O aval desta fatia veda tocar exportada. O evento retroativo continua chegando ao mensal e às duas lavras com a classificação nova. O POST do TXT usa o fechamento calculado em leitura, inclusive onde a guarda impede escrita do recálculo, e pode registrar uma nova fotografia. **RED:** entregar uma competência fora da guarda, disparar evento retroativo e exigir igualdade do mensal, das duas lavras e do TXT vigente. Exercitar também a reemissão na 09 guardada e na 11 entregue, com a 11 aberta como controle. Alteração efetiva em produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:36–50,564–600,948–965,1118–1185,1246–1276,1780–1806`, `app/folha/views.py:214–219,269–275,302–307`, `app/folha/models.py:173–188`) |
| **3. Oráculo e DIFF redecidem feriado.** | **Curado para FT projetada e para o intermitente no caminho alimentado.** F3 continua no motor. O auxiliar avulso empresta o próprio método, e a lavra FT recebe a partição. O DIFF usa a mesma projeção nos dois lados, portanto `4,00 = 4,00` prova a ligação, enquanto a classificação é cobrada pelos testes da porta. **Procedente fora de FT:** o oráculo ainda recompõe a dobra por dia, usa o flag de um motor montado no início com o template de referência do fim e pode cair na consulta nacional. O caminho de `periodo.py` também recompõe com os insumos que recebeu. Em memória, um par normal 6x1 deu 240 min no motor e zero em `do_dia` com o flag falso do 12x36 final. **RED:** feriado em dia normal da primeira fatia num 6x1 → 12x36 e no sentido inverso, com igualdade do mensal, das lavras e do TXT. Cobrir também calendário oficial injetado vazio. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:297–310,360–363,727–748,818–829`, `app/ponto/calculador/alimentacao.py:423–446,559–577`, `app/ponto/calculador/periodo.py:828–845`, `app/ponto/management/commands/diff_calculador.py:110–148,231–253,399–409`, `app/ponto/tests/test_feriado_um_juiz_porta.py:229–270`) |
| **4. Segundo escritor do split e split acima do total.** | **Curada a sobrescrita do período bruto e imposto o teto da promoção.** O split segue numa parcela própria, mensal e lavras a leem, e o noturno legal do dia promovido vem dos períodos apurados. **Procedente a exatidão do noturno longo:** reproduzimos 21h → 23h do dia seguinte, com prorrogação. A promoção retorna 1.560 min de split, embora a união da janela seja 1.500 min. A sobreposição compensa a hora diurna inicial, e o adicional legal também carrega o excesso. O teste atual começa às 22h e cobra o teto. **RED:** exigir 1.500 min de split e `1.500/0,875` min de adicional legal no caso de 21h. Incidência na frota [nao verificado]. (`app/ponto/calculador/regras.py:379–414`, `app/ponto/calculador/alimentacao.py:361–387`, `app/ponto/services/dia_pago.py:223–225,244–257`, `app/ponto/motor_calculo_v2.py:82–118,151–161,1027–1049`, `app/ponto/tests/test_feriado_um_juiz.py:196–216`, `app/ponto/tests/test_feriado_um_juiz_porta.py:242–248,343–351`) |
| **5. K1 e dia do turno.** | **K1 procedente.** A c3 conservou a asserção e marcou a falha como esperada. **Dia parcialmente curado:** o juiz de dobra agora recebe o dia do turno do par. A certeza, a cláusula da rubrica e a linha lavrada ainda usam a jornada herdada, que pode ser a véspera com intervalo de até 6 h entre turnos distintos. O teste mensal soma a competência inteira e aceita as 4 h mesmo se caírem na data errada. **RED:** nos pares 18h–22h na véspera e 03h–07h no feriado, exigir 4 h comuns na primeira linha e 4 h de feriado na segunda, em ambas as versões. Repetir com praça e certeza diferentes, e com a virada na borda da competência. (`app/ponto/calculador/regras.py:390–409`, `app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2574`, `app/ponto/services/dia_pago.py:126–132,244–257`, `app/ponto/tests/test_feriado_um_juiz.py:176–194`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286,298–324`) |
| **6. Soma, idempotência e falha da relavra.** | **Curado o caso 241 + 241 min:** agora soma e arredonda uma vez, chegando a 8,03 h em vez de 8,04 h. O teste repete a apuração e confere as somas das duas versões. Esse próprio RED comprova delta fora de feriado. **Procedente a falha parcial:** o mensal é salvo antes das lavras, cada erro é capturado separadamente e `lavrar` exclui as linhas antes de recriá-las. A repetição bem-sucedida conserva números no cenário testado e recria as linhas. **REDs:** cobrar paridade com a base num mês sem feriado para demonstrar o delta de escopo. Injetar falha no `bulk_create` depois do `delete` e exigir a preservação do conjunto anterior. Intercalar duas relavras e, em outro teste, primeira entrega e evento retroativo, conferindo mensal, versões e TXT vigente. (`app/ponto/services/fechamento.py:963–965,1118–1222,1246–1283,1780–1806`, `app/ponto/services/dia_pago.py:344–359,549–562`, `app/ponto/tests/test_feriado_um_juiz_porta.py:288–296`, `app/ponto/models.py:380–387`) |

Os controles pedidos de **véspera de um mesmo turno**, **municipal versus nacional**, **folga com abono** e **BO-B(3)** estão no código. O partido da véspera espera 7 h comuns e zero feriado. O municipal só alcança a praça pertinente, com nacionais nas duas praças, e o teste espera 12 h de feriado mais 4 h comuns. A folga promovida conserva 4 h e abono zero, e a ausência num dia com 480 min previstos e 240 realizados abona 240. BO-B(3) com escala certa e `feriado_vira_folga=False` conserva 4 h em folga trabalhada. O teste da virada com **dois turnos distintos** ainda precisa cobrar a data de cada linha, conforme o item 5. (`app/core/feriados.py:57–67,135–167`, `app/ponto/services/dia_pago.py:386–401`, `app/ponto/tests/test_feriado_um_juiz_porta.py:110–136,298–324,366–409`)

## Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **Períodos, minutos, calendário e dias** | O v2 escreve `minutos_trabalhados`, pontas efetivas, intervalos, extras brutas, `periodos`/`periodos_ft` e o mapa da jornada. O carregador de feriados resolve o município do vínculo de cada data. `apurar_por_fatia` compõe os mapas e grava em memória o contexto por identidade, lido por `particionar_ft` no fechamento e no DIFF. Escala certa lê LIMBO e A2/A8. `feriado_vira_folga` lê o cadastro `trabalha_em_feriado`, ciclo e calendário da praça. Há três valores distintos: dia civil da entrada, dia do turno do juiz e dia da jornada lavrada. Os dois últimos podem divergir por herança. (`app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2607,3353–3389`, `app/core/feriados.py:135–167`, `app/ponto/services/fechamento.py:314–346,672–673,950–952`, `app/ponto/management/commands/diff_calculador.py:137–148`, `app/escala/services/escala_certa.py:19–51`, `app/escala/models.py:1384–1406`, `app/ponto/services/dia_pago.py:126–132`) |
| **Trabalhadas, folga, HE50, HE100 e noturno** | O fechamento escreve `horas_trabalhadas` com períodos normais, órfãos e FT comum. FT certa vai para `horas_folga_trabalhada`. FT promovida entra em `horas_extras_100_feriado`, na HE100 total e nas extras totais. O split da promoção é somado à HE100 noturna. `lavrar` lê os mesmos períodos e listas para a versão `motor`. O TXT lê o mensal, calcula HE100 comum como total menos feriado, emite folga e feriado por rubrica e emite o adicional noturno à parte. `horas_trabalhadas` também alimenta classificação, travas de aprovação, prontidão e resumo. (`app/ponto/motor_calculo_v2.py:294–344`, `app/ponto/services/fechamento.py:941–965,1124–1155,1180–1188`, `app/ponto/services/dia_pago.py:173–186,223–271`, `app/folha/export.py:425–457,850–851,921–923,1059–1063`, `app/relatorios/cartao_pela_celula.py:93–120`) |
| **`rubricas_ft`, `lavrar(rubricas=...)` e leitores diários** | `alimentacao` projeta a partição por dia e recompõe as contribuições dos períodos normais naquele dia. `do_dia` lê a projeção e ainda apura intra e outras rubricas conforme o ramo. O fechamento passa `rubricas=` à mesma `lavrar` para a versão `oraculo`, desde 10/2026. A porta valida os donos declarados na chamada e substitui os campos fornecidos sobre a base já montada. `soma_da_competencia` agrega por versão. Para horas pagas do dia, `pago_do_dia` soma trabalhadas, folga e `detalhe.folga_feriado_min`. O detalhe é só da FT promovida. Assim a HE100 feriado de um período normal, cujos minutos já estão em trabalhadas, entra uma vez nas horas pagas. Espelho, app, calendário e testemunha do export leem essas linhas ou suas projeções. (`app/ponto/calculador/alimentacao.py:353–388,559–582`, `app/ponto/calculador/regras.py:747–778,914–918`, `app/ponto/services/fechamento.py:1246–1278`, `app/ponto/services/dia_pago.py:50–76,244–257,311–359,549–562,889–927`, `app/ponto/services/espelho.py:820–831`, `app/api/views.py:1527–1555`, `app/colaboradores/services/calendario.py:685–689`, `app/folha/porta_export.py:113–124`) |

A partição é **disjunta por ocorrência**: cada período FT entra em uma lista. Na rubrica do TXT, feriado e HE100 comum separam o total, e o split noturno é subconjunto. Isso sustenta a conservação no golden e no caso de 241 + 241 min. A igualdade **mensal = soma de cada lavra** permanece aberta nos contraexemplos de K1 e de feriado normal com troca de vínculo. A igualdade numérica também pode conviver com uma data errada na lavra. (`app/ponto/calculador/regras.py:389–414`, `app/ponto/services/fechamento.py:948–965`, `app/ponto/services/dia_pago.py:244–267,549–562`, `app/folha/export.py:425–452`, `app/ponto/tests/test_feriado_um_juiz_porta.py:69–92,272–308`)

## Selos, K1 e pouso [nao verificado]

- **Classes estruturais.** A promoção chama F3 e produz um valor em memória, sem novo escritor persistente. A derivação paralela que sobra é a alimentação do feriado normal do oráculo, descrita no item 3. O diff introduz zero leituras de relógio e zero inversões de trava. A corrida preexistente continua relevante para o dinheiro novo: a entrega trava a empresa, a revisão específica usa Empresa → Colaborador → Batida e consulta K6, e o recálculo genérico por evento roda em `on_commit` sem essa serialização. (`diff.txt:1`, `app/ponto/calculador/regras.py:389–414,818–829`, `app/ponto/services/dia_pago.py:344–359`, `app/folha/views.py:103,132–135`, `app/folha/export.py:561–567`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/ponto/services/fechamento.py:1780–1834`)
- **Os mutantes têm mordida no código.** Na cópia, sem gravação, rodamos **26 controles e 22 mutantes sem banco**. Os controles passaram e cada mutante provocou falha de asserção na função alterada em memória. Dos seis mutantes adicionais de porta, cinco alteram produção e um altera a fixture para provocar erro de unicidade. A execução dos seis e o RED contra o commit-base são [nao verificado]. O selo de endereço `endereco-c2` morde o registro de linha, com escopo de numeração. A cópia é um `git archive` e o modo `base` requer `git show`. (`app/ponto/tests/feriado_um_juiz_mutantes.py:38–143,169–223`, `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`, `CLAUDE.md:375–379`)
- **O `expectedFailure` de K1 deixa passar dinheiro errado.** O teste força a rota `MotorTurnoPartido` para `10:00–14:00 + 14:30–18:20` em feriado de folga e cobra 440 min de HE100 feriado, 440 de HE100 total e 30 min de intra no mensal e nas duas somas. A reprodução em memória deu 470 min de HE100 na promoção, zero intra no motor e 30 min de intra no `do_dia` com pausa de 30 e mínimo de 60. O laço de intra entre pares percorre `resultado.periodos`, e a FT fica em `periodos_ft`. O agregado do job informado não identifica qual asserção falhou. **RED:** tornar esse teste obrigatório sob o `!` separado do K1 e cobrar as três rubricas nas duas lavras, no mensal e na emissão com rubricas configuradas. O registro da dependência põe E1 antes de K1 e condiciona o pouso à prova de frota da 11 por rubrica. Pouso efetivo e ocorrência produtiva desse caso [nao verificado]. (`app/ponto/tests/test_feriado_um_juiz_porta.py:138–148,272–286`, `app/ponto/motor_calculo_v2.py:3044–3061,3078–3118`, `app/ponto/services/fechamento.py:953–960,985`, `app/ponto/calculador/regras.py:768–778`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016`)
- **Dos 40, a obra acrescenta um.** K1 é o único `expectedFailure` adicionado pelo diff. O censo do recorte na cópia encontrou 25 marcas diretas, incluindo K1, e 15 marcas temporárias de cron com prazo até 27/11. Isso coincide com o total informado para `1127142942_29488e3b`, `Ran 16274`, `OK`, 32 skips e 40 falhas esperadas. Identidades no log do job [nao verificado]. K1 tem tag e item de fila, mas a marca é incondicional. Prazo e aval para silenciar K1 [nao verificado]. (`diff.txt:1`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286`, `app/core/tests/marca_cron15.py:27–65`, `app/docs/BACKLOG.md:2005`, `CLAUDE.md:197–201,274–277`)

**Falta para o pouso e o apply.** Fechar os REDs procedentes e o pré-requisito K1. Certificar a suíte **inteira** no hash que vai pousar e fazer o opus final de mão única depois dela. O recorte verde informado cobre apenas os rótulos listados. Para a 11, comprovar que está aberta, a medição anterior dos **75 dias/42 colaboradores** como hora comum e a exclusão dos intermitentes. Antes do apply, gerar reversão por colaborador em `logs/` e guardar os hashes das exportadas vigentes e os valores gravados que a obra pode alcançar. No mesmo marco, publicar a prova de **frota** depois do apply, com antes × depois por colaborador, horas e rubrica, DIFF do TXT 11, hashes antes/depois das exportadas e trilha em `PENDENTES` com reversão. O corte separado do K1 exige ainda DIFF de frota por rubrica **antes** do pouso e `PAREI` se houver mudança fora de feriado de folga. Essas evidências operacionais, o pouso e o apply são [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/PROMPTS.md:3016`, `CLAUDE.md:129,260–287,295–299,421–431,448–467`)

## 27/11 15:21 · sessao ce7b3625b60e · - · - · origem=Ronald (tmux)

**REPROVADO.** K1 continua no caminho de produção. A promoção soma 470 minutos brutos de HE100 feriado no turno partido. A apuração de intra percorre apenas `resultado.periodos`, de modo que o caso de folga fica fora do laço. O `expectedFailure` mascara essa falha na pista. É o único `expectedFailure` adicionado pelo diff, e a identidade dos outros 39 no job fica [nao verificado]. **RED:** exigir 440 min de HE100 e 30 min de intra no mensal e nas duas lavras, com o teste normal. `app/ponto/motor_calculo_v2.py:3059–3118`, `app/ponto/services/fechamento.py:953–960,985`, `app/ponto/tests/test_feriado_um_juiz_porta.py:138–147,272–287`, `app/docs/RELATO.md:252–255`, `app/docs/PROMPTS.md:3016`.

- **F3 e segundo juiz.** A partição consulta o método registrado do motor e reaproveita a instância da fatia. O oráculo e o DIFF passam a consumir a partição da FT. Dias sem projeção, porém, julgam o feriado de novo com a régua de um motor montado com o template final e a data inicial, além de manterem fallback nacional. Em memória, um período real de MotorComercial 6x1 deu 240 min de HE100 feriado. O mesmo par em `do_dia`, alimentado com flag falso do vínculo final, deu 0 h. **RED:** 6x1 → 12x36 e sequência inversa, com feriado em dia normal, exigir mensal e ambas as lavras iguais. `app/core/juizes.py:320`, `app/ponto/motor_calculo_v2.py:941–978,1000–1011,3380–3388`, `app/ponto/calculador/regras.py:297–310,360–363,727–748,818–829`, `app/ponto/calculador/alimentacao.py:423–446,559–577`, `app/ponto/management/commands/diff_calculador.py:110–148,231–253,399–409`.
- **Data do turno e data da lavra.** A c2 corrige a data usada para decidir a dobra. A data usada para lavrar e escolher rubrica pode continuar na véspera pela herança de até seis horas. O RED de dois turnos distintos hoje soma o mês. **RED:** separar turnos `18–22` e `03–07`, exigir nas duas lavras a HE100 no dia do segundo turno e testar a borda da competência e escalas certas divergentes. `app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2574,3067–3075`, `app/ponto/calculador/regras.py:389–401`, `app/ponto/services/dia_pago.py:126–132,244–257`, `app/ponto/tests/test_feriado_um_juiz.py:176–194`, `app/ponto/tests/test_feriado_um_juiz_porta.py:299–313`.
- **Partição e noturno.** A partição tem três destinos disjuntos por ocorrência, reaproveita o split válido, calcula o faltante pela função do motor e limita o split da FT promovida à HE100. O valor bruto do período fica preservado. O noturno legal deriva da fatia nas projeções com promoção. O acesso direto a `motor.prorrogacao_noturna_pos5h` contorna a fronteira declarada da v3 e passa pelo filtro atual do selo, que procura `getattr` com default. **RED estrutural:** reconhecer acesso ao flag no motor desempacotado de `contexto_por_periodo` e exigir a porta declarada. `app/ponto/calculador/regras.py:379–414`, `app/ponto/calculador/nucleo.py:314–350`, `app/ponto/calculador/alimentacao.py:361–387`, `app/ponto/services/dia_pago.py:244–257`, `app/ponto/tests/test_s5b_porta_unica.py:273–292,393–400`.
- **Escopo e controles.** Já havia um motor por fatia, e o mapa novo preserva seu contexto para a FT. BO-B(3) certa com `feriado_vira_folga=False` permanece em folga trabalhada, e há controles de mudança de praça, nacional versus municipal, folga com abono e abono residual. O v2 preserva a exceção CCT existente no dia normal 12x36, e a promoção da FT exclui o ciclo. Intermitente por base ou por ciclo fica fora no caminho de produção. L-097 continua capaz de zerar os minutos antes da partição para folga com escala certa e sem autorização. Reproduzi em memória os mesmos 240 min de FT 6x1 saindo com 0 min quando certa e 240 min quando incerta sob a guarda L-097. **RED:** controle com e sem autorização na janela da guarda, conforme a delimitação do aval. `app/ponto/services/fechamento.py:299–321`, `app/ponto/calculador/regras.py:366–401`, `app/ponto/calculador/alimentacao.py:529–573`, `app/core/regua_cct.py:473–479`, `app/ponto/motor_calculo_v2.py:1562–1597,3694–3698,3740,3764–3767`, `app/ponto/tests/test_feriado_um_juiz_porta.py:101–113,253–270,315–329,366–409`.
- **Idempotência e soma.** O arredondamento único cura o caso 241 + 241, inclusive em folga comum, que ultrapassa o universo literal do feriado. O teste cobre repetição sequencial dos valores. O mensal segue gravado antes das lavras, e a substituição por versão ainda faz `delete` seguido de `bulk_create`. **RED:** injetar falha entre eles e checar conservação do conjunto anterior, depois intercalar evento retroativo e exportação. `app/ponto/services/fechamento.py:963–965,1118–1172,1180–1222,1246–1283`, `app/ponto/services/dia_pago.py:344–359`, `app/ponto/tests/test_feriado_um_juiz_porta.py:289–298`.
- **Mordida.** Rodei sem escrita e sem banco 26 controles verdes e 22 mutações de produção com controle verde e depois falha de uma asserção. A mutação da fixture espera erro de unicidade e altera só o teste. Mutantes de porta e RED contra a base [nao verificado]. No DIFF, as duas colunas de FT leem a mesma projeção, e `> 0,02` deixa diferenças de um centésimo fora da contagem de divergências. `app/ponto/tests/feriado_um_juiz_mutantes.py:37–140,187–224`, `app/ponto/management/commands/diff_calculador.py:231–253,399–409,503–527,546–572`.

## 27/11 15:20 · sessao 71639322d2dc · FERIADO-BOTAO-CORTADO · construtor · origem=Code

Alterei apenas [core/tests/test_smoke_clique_feriado_vira_trabalho.py](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516).

1. Mantive os nove cenários de 1366 px nas duas cascas, incluindo lateral, sem espaço e batida. [Cenários desktop](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:517)
2. **Lista do pouso** — saíram os quatro casos de 360 px (`base.html` e `base_app.html`, página e drawer), `esperado_360`, o retorno antecipado da sonda, `test_MORDE_painel_fora_da_viewport` e `painel-fora-360`. Ficou um comentário com o aval `BOTAO-POUSA-DESKTOP-2711-1510`. [Comentário do aval](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516) [nao verificado]
3. `test_MORDE_painel_sobre_o_gatilho` seleciona a página de `base.html` a 1366 px. A regex exige painel dentro da viewport, distância zero e `painelEncostado: False`. O deslocamento do mutante foi preservado. [Mutação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:252) · [Regex](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:453) · [Filtro](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:526)
4. Ajustei também `viewport-do-host`, que dependia da diferença entre 360 e 1366 px. Agora ele mede a 1365 px, e cada cenário confere a largura pedida. [Mutação de largura](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:419) · [Largura esperada](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:535)

Conferi a sintaxe, o diff e as asserções RED com medidas sintéticas. **A mordida real de `painel-sobre-gatilho` a 1366 px e o resultado da suíte no Chromium ficam [nao verificado].** [Asserções do smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429) [nao verificado]

**Comandos para o Code**, a partir de `app/`. Os dois comandos com mutante devem produzir RED no teste principal. [Entrada do smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:438) · [Configuração de CI](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/config/settings/ci.py:6) [nao verificado]

```bash
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
principal="$alvo.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas"

smoke_feriado() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python3 -B manage.py test "$1" --noinput
}

SMOKE_FERIADO_MUTANTE= smoke_feriado "$alvo"
SMOKE_FERIADO_MUTANTE=painel-sobre-gatilho smoke_feriado "$principal"
SMOKE_FERIADO_MUTANTE=viewport-do-host smoke_feriado "$principal"
```

_8735 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
