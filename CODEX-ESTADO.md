# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 08:54** · publicado em **07/11/2026 08:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 08:54 · sessao 6b0be6c97974 · - · - · origem=Ronald (tmux)

Achados confirmados na cópia, sem execução em banco:

1. **Reentrada no índice pela ressuscitação.** `_universo_da_ressuscitacao` inclui perguntas materializadas sem texto, sem excluir `duplicata` ou `motivo_retratado`; remove o carimbo na cópia e pergunta ao C6 se ela viveria. Se a célula acusa o dia e o marco está apagado, a antiga pode voltar mesmo havendo substituta para o mesmo fato. `ressuscitar_perguntas` então executa `UPDATE via_resolucao=''`, reintroduzindo a linha no índice sem `fato_livre_para` nem `_nascer`. Referências: `app/chamados/models.py:1449`, `:1470`, `:1473`, `:1524`, `:2852`, `:3060`. Caminhos: reabertura em `app/chamados/models.py:1417`; cartório em `app/ponto/services/cartorio.py:1622`; desarquivamento em `app/chamados/views_arquivar.py:86`.

2. **Reentrada no índice por `fechar_por_admin`.** Uma duplicata respondida pode continuar viva porque a resposta precede a regra da via; `fechar_o_fio_do_dia` seleciona `q_vivas` e troca a via para `admin_decidiu`, entrando novamente no índice enquanto a canônica permanece. Referências: `app/chamados/juizes.py:1152`, `:1164`, `:1173`; `app/chamados/models.py:2935`, `:2946`, `:2675`, `:2797`, `:3061`; `app/ponto/services/veredito_celula.py:174`. Também há chamadas diretas em `app/chamados/services/validacao.py:282` e `:317`.

3. **Identidade BATIDA ainda tem dois significados.** `_nascer` passa o episódio ao dedup; `fato_livre_para` não passa `p.batida_origem_id` e consulta motivo/dia/instância. Assim, dois episódios GPS distintos, permitidos pelo índice no mesmo dia, podem impedir um rechaveamento legítimo. Referências: `app/chamados/models.py:2582`, `:2513`, `:2496`, `:3064`.

4. **Outro derivador antigo mata episódios distintos.** `migrar_resolvedoras_vivas` identifica conflito somente por motivo e data; canoniza e retrata uma das linhas, mesmo quando são BATIDAS de episódios diferentes. Caminho produtivo: `ChamadoColaborador.reabrir`, ramo `irmao_vivo`. Referências: `app/chamados/services/fio.py:1013`, `:1017`, `:1019`; `app/chamados/models.py:1387`, `:1390`, `:3064`.

Inventário de escrita:

| Campo | Escritores encontrados |
|---|---|
| `batida_origem` | `_nascer` repassa o episódio, `app/chamados/models.py:2579`; `_chave_de_identidade` preenche na inserção, `:3026`; derivador lê o contexto e confere o dono, `:3041`–`:3045`. |
| `instancia_marco` | `_chave_de_identidade`, `app/chamados/models.py:3023`. Aceita valor explícito não vazio; não o reconfere com o motivo, `:3023`. |
| `colaborador` | `_chave_de_identidade`, `app/chamados/models.py:3017`. Igualdade com a disputa é conferida somente na inserção, `:3007`. |
| `conversa` | `abrir`/`abrir_conversa` passam `False`/`True`, `app/chamados/models.py:2529`, `:2540`; persistência em `:2629`. |
| `data_evento` | Nascimento em `app/chamados/models.py:2629`; datar pergunta sem dia em `app/chamados/services/fio.py:841`, `:863`; rechaveamento em `:913`, `:919`. |
| `via_resolucao` | `_escrever_carimbo`, `app/chamados/models.py:2713`; gravações por `carimbar :2762`, `carimbar_identidade :2797`, `descarimbar :2805`; lote por `.update() :2852`; encerramento por fato `:2862`; relavra direta em `app/chamados/services/relavra_via.py:123`, `:125`. |

Não encontrei criação produtiva de `PerguntaDisputa` fora de `_nascer`, cujo `cls.objects.create` está em `app/chamados/models.py:2629`; completude absoluta diante de escritores dinâmicos: **[nao verificado]**. Não encontrei `bulk_create` de perguntas; o `bulk_update` de `app/chamados/veredito_lavrado.py:207` usa os campos da lavra, não as chaves de identidade (`app/chamados/veredito_lavrado.py:57`). Buscas em `bin/`, `scripts/`, `ops/`, `eval/` e `fatias_agendadas/` não localizaram outro escritor desses campos; chamadas indiretas/genéricas: **[nao verificado]**. O leitor explícito de perguntas encontrado em `bin/` somente conta linhas: `bin/censo_folha.py:28`.

Leitores dos campos novos:

- Episódio: `app/chamados/models.py:2467`, `:2471`, `:2472`, `:3064`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `:87`.
- Instância: `app/chamados/models.py:2496`, `:3059`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`.
- Conversa: `app/chamados/models.py:2472`, `:2497`, `:2511`, `:3023`, `:3026`, `:3060`, `:3065`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`.
- Colaborador materializado: `app/chamados/models.py:2497`, `:2509`; `app/chamados/services/fio.py:853`, `:901`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `:142`, `:161`, `:176`.

Leitores produtivos de `data_evento`, agrupados:

- Núcleo: `app/chamados/juizes.py:266`, `:363`, `:623`, `:659`, `:932`, `:1199`, `:1525`, `:1965`, `:2031`; `app/chamados/services/materializacao.py:279`, `:344`, `:473`, `:517`, `:533`, `:621`, `:1256`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/services/fio.py:87`, `:777`, `:839`, `:904`, `:1014`.
- Emissão/declaração/validação: `app/chamados/services/disputa_emissao.py:392`, `:408`, `:433`, `:821`, `:963`, `:1293`, `:1389`, `:1707`; `app/chamados/services/declaracao_texto.py:237`, `:330`, `:337`, `:550`, `:625`; `app/chamados/services/validacao.py:268`, `:425`, `:476`, `:489`, `:645`.
- Reconciliação: `app/chamados/reconciliador.py:370`, `:944`, `:1040`, `:1072`, `:1142`, `:1293`, `:1543`, `:1553`, `:1628`, `:1641`.
- Outros serviços/telas: `app/chamados/canal.py:65`; `app/chamados/services/aviso_pendencia.py:20`; `cobranca.py:83`; `competencia_trancada.py:107`, `:252`; `explicador.py:70`, `:126`; `fabrica_da_celula.py:118`, `:137`; `historico.py:108`; `limbo_folgas.py:202`; `passivo_validado.py:113`, `:519`; `pergunta_no_app.py:92`, `:109`; `relavra_via.py:101`, `:152`; `vigia_fio.py:816`; `app/chamados/templatetags/chamados_extras.py:31`; `app/api/views.py:1184`, `:2664`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176`.
- Comandos: `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `:139`, `:144`; `backfill_veredito_lavrado.py:111`; `dedup_perguntas_fato.py:54`, `:69`; `disparar_perguntas_competencia.py:213`; `encerrar_perguntas_com_fato.py:74`; `migrar_data_evento_dia_do_turno.py:79`, `:126`, `:186`, `:306`, `:315`; `reabrir_orfas_enterradas.py:68`, `:86`, `:117`; `auditar_invariantes_chamados.py:63`, `:148`, `:282`, `:325`, `:892`, `:914`, `:957`, `:1662`, `:1775`, `:1801`; `app/ponto/management/commands/mapa_divergencia.py:272`; `app/ponto/management/commands/vigia_plantio_overnight.py:86`.

Leitores de `via_resolucao`, além dos próprios escritores:

- `app/chamados/juizes.py:648`, `:659`, `:814`, `:1147`, `:1173`; `app/chamados/models.py:2474`, `:2498`, `:2937`, `:2939`, `:3061`, `:3066`; `app/chamados/reconciliador.py:1133`, `:1144`; `app/chamados/services/materializacao.py:438`, `:623`, `:665`, `:1080`; `passivo_validado.py:141`, `:515`; `relavra_via.py:116`, `:153`, `:179`, `:230`; `fio.py:958`; `fabrica_da_celula.py:119`; `explicador.py:95`; `competencia_trancada.py:251`; `app/ponto/services/cartorio.py:1599`; `app/ponto/services/flip_auto.py:129`.
- Comandos: `app/chamados/management/commands/auditar_invariantes_chamados.py:202`, `:433`, `:542`, `:955`, `:1707`; `backfill_decisoes_pergunta.py:96`, `:127`, `:178`; `dedup_perguntas_fato.py:74`; `encerrar_perguntas_com_fato.py:33`, `:42`; `migrar_data_evento_dia_do_turno.py:52`; `reabrir_orfas_enterradas.py:67`, `:129`; `relavrar_via_i6.py:58`, `:88`.

Semântica temporal: `abrir` declara dia do turno, mas conversa grava dia civil do nascimento no mesmo campo; os leitores frequentemente não consultam `conversa`. Referências: `app/chamados/models.py:2523`, `:2534`, `:2540`; leitura da janela em `app/chamados/veredito_lavrado.py:229`. A janela viva é inclusiva nas duas pontas, `app/chamados/juizes.py:589`; a expiração reproduz essa inclusão em `app/chamados/veredito_lavrado.py:116`. `via_pelo_fato` usa o dia civil inteiro para provar ausência de batidas, deliberadamente, e depois consulta o turno com a mesma data: `app/chamados/juizes.py:1451`, `:1462`, `:1463`, `:1481`; `batidas_apuraveis` inclui início e fim, `app/ponto/turnos.py:2868`, `:2870`.

## 07/11 08:53 · sessao 20696c111c9b · - · - · origem=Ronald (tmux)

Achados confirmados na cópia, sem execução de banco ou testes. [nao verificado]

1. **O backfill não cumpre “data_evento NULL ... saem ... ANTES do índice”.** `fora_do_indice()` apenas lista MARCO sem dia e BATIDA sem origem; `aplicar()` recebe somente colisões e admin sem ato, sem reparar nem retratar esses dois baldes. A guarda da migration exclui expressamente as chaves NULL e permite subir os índices com essas linhas restantes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:114`, `:149`, `:263`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `:35`)

2. **A proteção de competência exportada pode ignorar participantes de colisão BATIDA.** A chave BATIDA não inclui dia; o teste constrói colisão com dois dias diferentes. `competencia_de()` calcula a competência de cada pergunta, mas `handle()` classifica o grupo inteiro apenas pela competência de `pks[0]`. Se a primeira estiver aberta e outra exportada, `paradas_apply` não protege esta última e `aplicar()` pode retratá-la. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `:137`, `:235`, `:243`, `:263`; `app/chamados/tests/test_e1_v5_portas_backfill.py:141`)

3. **O comando faz a verificação da exportação antes das travas e não a repete no apply.** `handle()` consulta `competencia_entregue`; `aplicar()` abre seus próprios atomics por grupo e por pergunta sem consultar novamente a competência. Uma alteração concorrente entre essas etapas não é coberta pelo código mostrado; ocorrência real em produção [nao verificado]. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`, `:259`, `:158`, `:174`)

4. **F — “a primeira fica” diverge deliberadamente do código e do selo.** `canonica_do_grupo()` ordena por PK, prefere a primeira com `resposta_colab.strip()` não vazia e somente na ausência de resposta conserva o menor PK. Portanto, uma pergunta posterior respondida retrata a primeira não respondida. Entre respondidas, vence o menor PK; espaços em branco não contam como resposta. O backfill usa essa autoridade no DRY e no apply; o teste exige justamente a substituição da primeira pela segunda respondida. (`app/chamados/services/fio.py:943`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:165`, `:233`; `app/chamados/tests/test_e1_v5_portas_backfill.py:119`)

**D — mordida dos quatro arquivos novos**

| Selo | O que verifica | Mutante de produção que escapa |
|---|---|---|
| `test_e1_v5_familia.py` | Família obrigatória e cobertura; campos dos constraints do **modelo**; listas congeladas da migration. (`:40`, `:44`, `:78`, `:91`) | Remover `instancia_marco` dos `fields` da **AddConstraint da migration**, preservando o modelo e as listas: esse selo não inspeciona os campos da operação migratória. Também não compara os conectivos e exclusões da condição: `_motivos_da_condicao()` extrai somente `motivo__in`. (`:62`, `:84`, `:91`; migration `:53`) |
| `test_e1_v5_indice.py` | Corrida crua MARCO/BATIDA, episódios distintos, motivos distintos, conversa/duplicata, porta sobre legado sem origem. (`:62`, `:69`, `:84`, `:90`, `:96`, `:110`) | Remover `instancia_marco` da chave real do índice passa por todos esses casos: nenhuma fixture cria duas instâncias diferentes no mesmo dia/motivo. Também não há caso para exclusão por `motivo_retratado`. (`:64`, `:71`, `:96`, `:106`) |
| `test_e1_v5_ordem_trava.py` | Ordem das tabelas vistas nos quatro caminhos isolados; dois mutantes simulados. (`:88`, `:95`, `:103`, `:112`, `:121`, `:135`) | Desligar o signal do cartório, ou retirar a fabricação no ciclo, não torna vermelho o teste de `analisar`: ele exige apenas a trava do Chamado. A fixture não informa célula ou contexto de dia; o signal só chama o cartório quando encontra dia. (`:77`, `:88`; `app/chamados/signals.py:176`) |
| `test_e1_v5_portas_backfill.py` | Recusa de colisão nas duas portas; ato de fechamento; colisões MARCO/BATIDA, idempotência, exportada simulada, ordem. (`:56`, `:70`, `:81`, `:119`, `:137`, `:150`, `:161`, `:173`) | Remover `ato_por` das novas gravações passa: os testes conferem somente `ato`/`ato_em`. Também passam ausência de tratamento para NULL e verificação de exportação somente da primeira competência, pois faltam esses casos. (`:87`, `:156`, `:141`, `:164`) | [nao verificado]

A guarda `_guarda()` poderia virar `noop` sem ser exercitada pelos novos testes: eles retiram/devolvem os índices diretamente por `schema_editor`, sem executar a migration 0053 sobre acervo sujo. (`app/chamados/tests/acervo_sem_indice.py:18`; `app/chamados/tests/test_e1_v5_portas_backfill.py:95`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:29`)

O coletor de travas deduplica **por tabela**, sem identificar linha e sem reiniciar por transação. Assim, um segundo Colaborador travado depois de Pergunta, ou uma segunda Disputa depois de Chamado, pode ficar invisível se essa tabela já apareceu. Também captura somente a primeira tabela do `FROM`, não todas as tabelas potencialmente alcançadas por um SELECT com joins. (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`, `:38`, `:43`, `:45`)

**G — migration 0053**

- A mesma classe `Migration` contém `RunPython(_guarda)` e as duas `AddConstraint`, sem `atomic=False`. Pelo padrão do Django/PostgreSQL isso corresponde a uma transação de migration; confirmação do executor instalado e da execução real [nao verificado]. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:42`)
- A guarda faz somente consultas agregadas, sem INSERT/UPDATE/DELETE. Portanto, ela própria não produz eventos de FK pendentes que expliquem `pending trigger events`; o ambiente real e eventuais triggers externos [nao verificado]. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:29`)
- Guarda e índices usam a **mesma função `_no_fato()`**, com as mesmas listas congeladas. A guarda acrescenta `data_evento IS NOT NULL` e `batida_origem IS NOT NULL`; isso corresponde às chaves que podem colidir num índice UNIQUE com NULLs distintos. Não há divergência local de `conversa`, motivo ou vias entre guarda e condição. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:25`, `:32`, `:35`, `:54`, `:59`)
- O helper de teste executa `SET CONSTRAINTS ALL IMMEDIATE` antes do DDL, mas a migration não executa DML anterior que exija essa drenagem. A necessidade concreta no banco de produção [nao verificado]. (`app/chamados/tests/acervo_sem_indice.py:18`; migration `:29`)

**A — censo de `DecisaoPergunta.ato/ato_em/ato_por`**

Escritor físico único: `registrar()` constrói as instâncias e faz `bulk_create`. (`app/chamados/models.py:3517`)

Produtores dos valores:

- Fechamento da disputa: SILENCIO, carimbo da pergunta e usuário. (`app/chamados/models.py:1878`)
- Retratação de motivo: RETRATACAO, instante do ato e ator. (`app/chamados/models.py:1998`)
- Resposta humana: RESPOSTA, `_agora` compartilhado com `respondida_em`, usuário. (`app/chamados/services/disputa_emissao.py:1369`)
- Reabertura/recusa: ato recebido, instante único e `por`. (`app/chamados/services/disputa_emissao.py:1478`)
- Resposta automática e resposta carimbada pela trilha: RESPOSTA e instante recebido, sem autor informado. (`app/chamados/services/disputa_emissao.py:1519`, `:1540`)
- Validação e devolução: VALIDACAO com `validada_em`; DEVOLUCAO com novo `timezone.now()`; autor recebido. (`app/chamados/services/validacao.py:139`, `:205`)
- Absorção: ABSORCAO com o mesmo instante do carimbo e usuário. (`app/chamados/services/materializacao.py:558`)
- Fechamento do fio: SILENCIO com `materializada_em` e autor. (`app/ponto/services/veredito_celula.py:182`)
- Backfill: SILENCIO com `materializada_em` e `materializada_por_id`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:181`)
- Projeção legada: RESPOSTA/VALIDACAO com seus carimbos originais; validação conserva `validada_por_id`, resposta não informa autor. (`app/chamados/models.py:3505`)

Leitores:

- Ordenação por `ato_em`, depois PK. (`app/chamados/models.py:3463`)
- Correspondência de legado por `(pergunta, ato, ato_em)`. (`app/chamados/models.py:3501`)
- Histórico da tela lê somente RESPOSTA e atos com foto. (`app/chamados/models.py:3082`)
- Fio faz prefetch de `ato_por`; template mostra ato, instante e autor. (`app/chamados/services/fio.py:231`; `app/templates/chamados/partials/modal_fio.html:208`)
- Backfill verifica existência de qualquer SILENCIO/VALIDACAO, sem correspondência de instante. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:125`, `:179`)

**Possível ESMERIL em `ato`:** SILENCIO é apresentado como “Silencio encerrado pela porta”, e o fechamento da disputa recusa respostas pendentes antes desse ato. Já `fechar_o_fio_do_dia()` percorre `q_vivas()`, cujo universo inclui resposta não vazia, e grava SILENCIO mesmo assim. O enum passa a cobrir tanto ausência de resposta quanto encerramento administrativo de pergunta respondida. (`app/chamados/models.py:3439`, `:1848`, `:2918`; `app/ponto/services/veredito_celula.py:174`)

## 07/11 08:53 · sessao 184f8267eb13 · - · - · origem=Ronald (tmux)

Achados para C/D/E, por leitura estática; execução concorrente e ocorrência em produção [nao verificado].

1. **A ordem global continua violada no fechamento de disputa.** `DisputaSupervisao.fechar` abre atomic em `app/chamados/models.py:1866`; nas exceções, grava Pergunta primeiro (`:1875` → `fechar_por_admin`, `:2675` → `carimbar_identidade`, `:2797`); depois grava Disputa (`:1890`). O post_save pede `ch.reconciliar()` (`app/chamados/signals.py:41-42`), que chama `_recarregar_estado_se_mudou` (`app/chamados/models.py:627-628`, `:649`) e só então trava Colaborador (`:686`). Mesmo sem perguntas vivas, a sequência Disputa UPDATE → Colaborador permanece quando o pai está vivo (`app/chamados/signals.py:33`).

2. **Aprovação/materialização de justificativa começa pelo Chamado.** `aprovar_justificativa_dp` abre atomic e chama `travar_origem_antes` (`app/ponto/services/justificativa.py:125-126`), cuja trava é Chamado FOR UPDATE (`:114`); depois `encerrar_chamado_de_origem` (`:202`) trava Chamado novamente (`:38`) e chama `retratar` (`:42`) → `_retratar` (`app/chamados/reconciliador.py:522-523`) → `reconciliar` → Colaborador (`app/chamados/models.py:649`, `:686`). `materializar_saida_dp` tem a mesma entrada (`app/ponto/services/justificativa.py:327-329`) e chama o fechamento da origem (`:393`).

3. **Validação de folga grava Pergunta antes de chegar ao Colaborador.** Ramo de folga já declarada: atomic (`app/chamados/services/validacao.py:280`) → `_carimbar_validacao` (`:281`) → save UPDATE Pergunta (`:137`) → `fechar_o_fio_do_dia` (`:289`) → `c.reconciliar` (`app/ponto/services/veredito_celula.py:190`) → trava Colaborador (`app/chamados/models.py:686`). Ramo de nova folga também grava a validação primeiro (`app/chamados/services/validacao.py:307-309`) antes de declarar o dia.

4. **A própria função tocada `fechar_o_fio_do_dia` não estabelece a ordem prometida.** Percorre e grava Perguntas (`app/ponto/services/veredito_celula.py:174-177`) antes de reconciliar Chamados (`:189-190`), que adquire Colaborador (`app/chamados/models.py:686`). Seus chamadores usam atomic envolvendo a função: `veredito_celula` (`app/ponto/services/veredito_celula.py:211`, `:332`) e declaração de folga (`app/ponto/services/declarar_dia.py:188`, `:204`). Uma trava anterior eventualmente adquirida por efeitos de cada ramo não é garantida por essa função; cenário explícito da validação já acima (`app/chamados/services/validacao.py:280-289`).

5. **Comando de reata mantém Chamado → Colaborador.** `reatar_beco_folga_ao_limbo` abre atomic e trava pai Chamado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99-100`); chama reata (`:105`) → `reatar_perguntas_ao_limbo` → `reapontar_resolvedora` (`app/chamados/services/limbo_folgas.py:243`) → trava Colaborador (`app/chamados/services/fio.py:853`, caso sem nova data).

6. **O ciclo literal analisar → cartório → fábrica ainda admite Chamado antes de Disputa.** O novo `_gravar_estado` trava Colaborador e depois grava Chamado (`app/chamados/models.py:285`, `:290`). O post_save com dia conhecido chama cartório (`app/chamados/signals.py:177-180`), cujo final chama fábrica (`app/ponto/services/cartorio.py:997`). A fábrica pode chamar `perguntar` (`app/chamados/services/fabrica_da_celula.py:176`) → `criar_ou_atualizar_disputa` (`:139`) → `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:642`) → Disputa FOR UPDATE (`:511`). Também pode reler Pergunta e Disputa juntas (`app/chamados/services/fabrica_da_celula.py:166` → `app/chamados/services/relavra_via.py:221`). Portanto, colocar Colaborador primeiro resolve aquela inversão de duas tabelas no trecho, mas não implementa a ordem literal Colaborador → Disputa → Chamado → Pergunta no ciclo inteiro (`app/chamados/models.py:279-290`; caminhos citados).

7. **O selo novo não percorre o ciclo do aval.** A fixture de Chamado omite célula e data (`app/chamados/tests/test_e1_v5_ordem_trava.py:77-79`); o teste de analisar só executa essa fixture (`:88-93`). O signal só chama cartório com dia conhecido (`app/chamados/signals.py:177-180`), e `dia_do_fato(fortes=True)` devolve None sem célula/data (`app/chamados/juizes.py:93-124`). Assim, um mutante que retire a trava prévia somente quando o Chamado possui célula/dia não é exercitado pelo teste analisar; os ramos reais com cartório/fábrica também ficam fora dessa fixture (`app/chamados/tests/test_e1_v5_ordem_trava.py:77-93`).

8. **O detector de trava confunde tabela já vista com linha já travada.** `travas` registra somente a primeira ocorrência por nome de tabela (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45-47`). Mutante: travar Colaborador A, depois Chamado, depois Colaborador B; a segunda trava de colaborador é descartada e o detector não denuncia a inversão por outra linha (`:45-47`, `:57-65`). O parser de FOR UPDATE também só lê o primeiro `FROM`, sem interpretar tabelas de JOIN/`OF` (`:43-44`); a produção tem precisamente uma trava conjunta de Pergunta+Disputa (`app/chamados/services/relavra_via.py:221-224`).

Os testes novos de ordem exercitam apenas analisar, nascer reapontando duplicata, reapontar e rechavear (`app/chamados/tests/test_e1_v5_ordem_trava.py:88-119`); os caminhos de fechar disputa, validar folga e aprovar justificativa acima não são chamadas desses quatro casos (`mesmo arquivo:88-119`).

## 07/11 08:52 · sessao 3374abd0dab0 · V2-I2 · sonda · origem=Code

**A volta 2 fecha a diferença em 27/09: +420 minutos. Ela ainda não identifica uma mudança de campo que causou o zero gravado, o autor dessa mudança ou o dono conclusivo L-099.** [nao verificado]

1. **Dia e campo explicativo**

| Dia / célula | Previsto hoje | DP gravado | Diferença |
|---|---:|---:|---:|
| 27/09/2026 / 33835 | 420 | 0 | **+420** |
| Total informado | 10920 | 10500 | **+420** |

A tabela reproduz a única divergência emitida pela sonda fornecida. [nao verificado]

- **`trabalha=True`** permite calcular a jornada; a função retorna zero quando `eh_dia_trabalho` não retorna exatamente `True`. Não foi fornecido o valor histórico de `trabalha` usado na lavratura do DP. ([utils.py:2287](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2287); [nao verificado])
- **DNA:** os marcos visíveis explicam os 420 atuais: 15–23 são 480 minutos, descontados 60 de pausa. **A troca 19–20 → 20–21 acrescenta zero minutos**, pela mesma aritmética. ([utils.py:2293](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2293), [utils.py:2326](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2326))
- **Geradora e vigência:** a seleção atual procura a geradora nomeada pela célula e rejeita o dia fora de suas datas. O resultado atual de 420 indica que essa seleção passou; porém, o raspado não fornece o PK, as datas nem seus estados históricos. ([alimentacao.py:269](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/alimentacao.py:269); [nao verificado])

Portanto, **o DNA explica o valor atual; nenhum dos quatro campos está demonstrado como causa histórica de 0 → 420**. [nao verificado]

Há uma diferença decisiva entre os leitores: a grade usada no fechamento prefere **`ata.minutos_previstos`**, inclusive quando vale **zero**, e só chama `minutos_previstos_do_dia` se esse campo for `None`. O mapa destinado ao DP inclui somente dias classificados como `trabalho` ou `ausencia`. Assim, também precisam ser investigados **ATA e classificação do dia**, além dos quatro campos propostos. ([leitor_celula.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/services/leitor_celula.py:380), [dia_pago.py:387](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/services/dia_pago.py:387))

2. **Escritor, momento e disparador**

O escritor compatível com os marcadores apresentados é **`regenerar_celulas_vinculo`**: reatribui `trabalha`, `escala_geradora` e `dna`, guarda `dna_anterior`, atualiza `regenerada_em` e incrementa o contador. Sua condição exige diferença em **qualquer um** dos três campos; não prova que todos mudaram. ([celula.py:263](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:263), [celula.py:271](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:271))

O marcador informado é **23/10/2026 13:04:42.382279 UTC**, contador **2**. A atribuição desse ato concreto ao escritor e ao seu chamador permanece [nao verificado].

A auditoria pertinente é **`acao='regenerar_celulas_vinculo'`, `modelo='EscalaColaborador'`, `objeto_id=PK da geradora`**; registra autor opcional, motivo e janela. Os logs fornecidos são apenas `veredito_celula` de **33829–33834**, sem a regeneração de **33835**. **Quem disparou: [nao verificado].** ([celula.py:334](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:334); [nao verificado])

O marcador também não certifica toda escrita posterior: existem caminhos que alteram campos sem atualizá-lo. ([celula.py:243](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:243), [celula.py:865](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:865))

3. **Ordem temporal**

| Marco informado | UTC |
|---|---|
| Célula `regenerada_em` | 23/10 13:04:42.382279 |
| FM `atualizado_em` | 29/10 10:26:49.276449 |
| DP `lavrado_em` | 29/10 10:26:49.281559 |
| Célula `julgada_em` / `insumos_em` atuais | 02/11 07:25:34.015313 |

Valores e ordenação derivados do raspado. [nao verificado]

**O DP é posterior à regeneração registrada e ao carimbo do FM, mas anterior ao julgamento atual da célula.** Portanto, a cronologia fornecida não sustenta “DP anterior à regeneração”. [nao verificado]

O campo temporal do DP nesta cópia chama-se **`lavrado_em`**, com `auto_now=True`; não `atualizado_em`. ([models.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/models.py:380))

4. **Dono L-099 e prova faltante**

**Dono conclusivo: [nao verificado].** A L-099 exige coerência com o cadastro existente; regeneração, isoladamente, não demonstra cadastro errado. ([LEIS.md:122](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/docs/LEIS.md:122))

O juiz aplica esta prioridade: flags de cadastro ausente/indefinido → **CADASTRO**; batida fora de turno ou quantidade ímpar → **BATIDA**; fase deslocada ou Cadastro × Realidade → **CADASTRO**; restante → **ESTRUTURA**. `furo_apurado` e `veredito='furo'` não são argumentos desse juiz. **Zero batidas é par**; com as demais flags falsas, retorna **ESTRUTURA**. ([e6_oraculo.py:53](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:98))

Falta exatamente:

- **Autoria:** PK da geradora e auditoria completa da regeneração de 23/10, com `usuario`, `ator`, descrição e valores anteriores/posteriores. ([celula.py:334](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:334))
- **Causa do zero:** ATA/grade consumida em 29/10, sobretudo `minutos_previstos` e `tipo_dia`, mais os estados históricos de trabalha/geradora/vigência. A ATA atual está truncada e seu julgamento é posterior ao DP. ([leitor_celula.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/services/leitor_celula.py:380); [nao verificado])
- **Dono:** `horas_do_dia` do oráculo e os seis indicadores efetivos: `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho`, `batida_fora_de_turno`, `fase_deslocada` e `em_cadastro_x_realidade`. Os indicadores da célula não substituem essa alimentação. ([e6_oraculo.py:172](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:172))

## 07/11 08:52 · sessao 9beb35ca1c56 · - · - · origem=Ronald (tmux)

1. **Bloqueia — mistura de dia UTC e local permanece.** `esta_stale` usa `timezone.localdate(agora)` para comparar a expiração, mas `p.julgado_em.date()` para decidir se a lavra antecedeu a virada; a varredura duplica essa combinação (`app/chamados/veredito_lavrado.py:198`, `app/chamados/veredito_lavrado.py:201`, `app/chamados/services/raiox.py:42`). Contraexemplo reproduzido apenas em memória, extraindo a função por AST: expiração `2026-11-21`, julgamento `2026-11-21 01:30 UTC` (=20/11 22:30 SP), agora `21/11 09:00 SP`, sem marcos → `False`, embora o julgamento local anteceda a expiração (`app/chamados/veredito_lavrado.py:198`). O teste novo exercita apenas `agora` em UTC, com julgamento às 20h locais, portanto não cobre esse caso (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`, `app/chamados/tests/test_e2_tarefa_do_dia.py:637`).

2. **Bloqueia — lavra do futuro continua aceita no passado.** `esta_stale` não exige `julgado_em <= agora`; sem expiração e sem marco posterior ao julgamento, devolve `False` mesmo com julgamento em 2036 e agora em 2026 (`app/chamados/veredito_lavrado.py:196`, `app/chamados/veredito_lavrado.py:203`). A carga aceita esse veredito como vigente (`app/chamados/services/tarefa_do_dia.py:297`). A própria fixture “fresco” grava `D+3650`, escondendo esse defeito temporal (`app/chamados/tests/test_e2_tarefa_do_dia.py:129`). Contraexemplo reproduzido sem banco por extração AST da função (`app/chamados/veredito_lavrado.py:187`).

3. **Fila — os relógios do juiz e da expiração não têm o mesmo contrato.** `expira_em` usa a data local recebida; `lavrar` fornece `timezone.localdate(p.julgado_em)` (`app/chamados/veredito_lavrado.py:115`, `app/chamados/veredito_lavrado.py:245`). Entretanto, o juiz efetivamente chamado por `lavrar` usa `_dentro_da_janela_viva`, que continua lendo `datetime.date.today()` (`app/chamados/veredito_lavrado.py:235`, `app/chamados/juizes.py:563`, `app/chamados/juizes.py:661`). Caso demonstrativo: SO em UTC, 21/11 01:30 UTC =20/11 22:30 SP: juiz usa janela de 21/11, expiração usa janela de 20/11 (`app/chamados/juizes.py:564`, `app/chamados/veredito_lavrado.py:117`). Fuso efetivo do SO em produção: [nao verificado]. O selo examina somente funções diretas nomeadas e não alcança esse helper (`app/chamados/tests/test_e2_tarefa_do_dia.py:743`).

4. **Fila — chamadores não migrados para `ate`.** Há dois: `_mdl_s2([colaborador.pk])` no fio e a chamada no teste de concordância (`app/chamados/services/fio.py:248`, `app/chamados/services/fio.py:249`, `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`). Não causam `TypeError`, porque `ate` continua opcional (`app/chamados/veredito_lavrado.py:125`). No fio, o resultado chega a `_sem_acao`, mas essa função ignora `marcos` e simplesmente lê o veredito, portanto a ausência de `ate` não muda essa decisão hoje; deixa cinco queries sem uso efetivo nessa decisão (`app/chamados/services/fio.py:284`, `app/chamados/services/raiox.py:152`, `app/chamados/services/raiox.py:174`, `app/chamados/services/raiox.py:183`). No teste, a varredura usa `ate=agora` e o comparador lê o banco inteiro com outro relógio, deixando de comparar precisamente o mesmo recorte temporal (`app/chamados/services/raiox.py:35`, `app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`).

5. **Fila — migração de assinaturas realizada nos chamadores executáveis encontrados.** Produção: `lavrar` e backfill fornecem `hoje`; tarefa e auditor fornecem `marcos, agora` a `esta_stale`; tarefa, auditor e varredura fornecem `ate` (`app/chamados/veredito_lavrado.py:245`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/services/tarefa_do_dia.py:296`, `app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1161`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`, `app/chamados/services/raiox.py:36`). Chamadas antigas nos testes novos são intencionais, sob `assertRaises(TypeError)` (`app/chamados/tests/test_e2_tarefa_do_dia.py:648`). Demais ocorrências textuais antigas identificadas são comentários/docstrings, não execução (`app/chamados/services/fio.py:814`, `app/core/portas.py:319`).

6. **Fila — mapa dos escritores e leitores dos marcos.** Todos os marcos abaixo são instantes de registro/alteração/trilha, não o dia do turno ou necessariamente o instante da ocorrência da batida; `marcos_do_lote` os agrega por colaborador sem limitar datas de células ao horizonte (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:165`, `app/chamados/veredito_lavrado.py:176`).

   - `Batida.criado_em`: `auto_now_add`, criado pela porta de batida (`app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`); leitor `marcos_do_lote` (`app/chamados/veredito_lavrado.py:151`).
   - `Batida.retratada_em`: porta `retratar_batida` usa `timezone.now()`; desfazer retratação limpa por `anotar` (`app/ponto/registro_batida.py:274`, `app/ponto/services/retratar_humano.py:207`); leitor (`app/chamados/veredito_lavrado.py:154`).
   - `CelulaDia.gerada_em`: `auto_now_add` (`app/escala/models.py:1702`).
   - `CelulaDia.regenerada_em`: regeneração e desfazer regeneração pela porta (`app/ponto/portas/celula.py:274`, `app/ponto/portas/celula.py:792`).
   - `CelulaDia.editada_em`: edição humana pela porta, com `agora=timezone.now()` (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:607`).
   - `CelulaDia.veredito_em`: porta, `agora or timezone.now()` (`app/ponto/portas/celula.py:1032`).
   - `CelulaDia.julgada_em`: porta `carimbar`, com `agora` obrigatório recebido (`app/ponto/portas/celula.py:1047`, `app/ponto/portas/celula.py:1060`).
   - Os cinco marcos de célula são lidos conjuntamente (`app/chamados/veredito_lavrado.py:167`).
   - `PerguntaDisputa.materializada_em`: atribuição central `_escrever_carimbo`, com `quando` recebido ou lido na porta; descarimbo também limpa o campo (`app/chamados/models.py:2658`, `app/chamados/models.py:2683`, `app/chamados/models.py:2802`); leitor (`app/chamados/veredito_lavrado.py:176`).
   - `PerguntaDisputa.respondida_em`: resposta pelo relógio do ato; resposta automática/trilha usam instante recebido; carimbar pode preencher e desfazer/reabrir pode limpar/restaurar (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/models.py:2691`, `app/chamados/models.py:2722`); leitor (`app/chamados/veredito_lavrado.py:180`).

   Com `ate`, cada campo usa limite inclusivo `<= ate`; sem `ate`, lê todos os valores atuais. Para invalidar a lavra, o marco precisa ser estritamente `> julgado_em` (`app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`, `app/chamados/veredito_lavrado.py:205`). O limite não reconstrói versões históricas sobrescritas: lê os campos atuais filtrados (`app/chamados/veredito_lavrado.py:167`).

7. **Fila — `janela_expira_em` tem escritores declarados e expiração inclusiva.** `lavrar` calcula e atribui a data e salva o trio; o backfill calcula separadamente, atribui o trio e persiste pela porta de lote; desfazer restaura a fotografia em memória (`app/chamados/veredito_lavrado.py:243`, `app/chamados/veredito_lavrado.py:247`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/chamados/veredito_lavrado.py:261`). Leitores principais: `esta_stale`, varredura e diagnóstico/recorte do backfill (`app/chamados/veredito_lavrado.py:198`, `app/chamados/services/raiox.py:42`, `app/chamados/management/commands/backfill_veredito_lavrado.py:121`). Expira no próprio dia (`<= hoje`), enquanto a janela viva do juiz inclui as duas pontas (`app/chamados/veredito_lavrado.py:198`, `app/chamados/juizes.py:589`). `expira_em` recebe uma **data local**; `esta_stale` recebe um **instante** e converte para data local; o backfill lê `hoje` e `agora` em chamadas separadas, potencialmente atravessando a meia-noite (`app/chamados/veredito_lavrado.py:109`, `app/chamados/veredito_lavrado.py:192`, `app/chamados/management/commands/backfill_veredito_lavrado.py:94`).

8. **Fila — pendência E1 não impede execução por assinatura.** `lavrar` continua lendo o relógio e está explicitamente excluído do selo como borda; `models.py` chama a assinatura preservada (`app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:244`, `app/chamados/tests/test_e2_tarefa_do_dia.py:722`, `app/chamados/models.py:2709`). O fio continua chamando `marcos_do_lote` sem `ate`, aceito pelo valor padrão; o efeito atual é o descrito no achado 4 (`app/chamados/services/fio.py:249`, `app/chamados/veredito_lavrado.py:125`). Nenhum teste Django foi executado nesta auditoria; comportamento de banco/produção permanece [nao verificado].

## 07/11 08:52 · sessao b52aa7feb371 · - · - · origem=Ronald (tmux)

1. **B3 realizado — fila, sem bloqueio demonstrado neste recorte.** A carga chama o leitor único com cadastro recarregado, células e vínculos completos; `_tarefa_pura` recebe o resultado e retorna sem ações para `fora_vinculo` e isento. O juiz de dia sem trabalho continua delegado pela ponte do suprajuiz. **Caso:** demissão D−1, admissão D+1, isento e demissão D com/sem fato estão explicitamente confrontados por Caso13. `app/chamados/services/tarefa_do_dia.py:276`, `:315`, `:171`, `:174`, `:184`; `app/ponto/supra_juiz.py:65`; `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:562`, `:569`, `:576`, `:584`.

2. **Todas as formas de `_celulas_da_ata` receberam o campo — fila.** Com lâmpadas, escreve `acesa_lavrada=l_.get('acesa')`; sem lâmpadas, cada célula agregada recebe `None`. Não existe ramo `sem_ancora` ou retorno antecipado correspondente nesta função; `sem_ancora` é transportado no dia. **Caso:** ata agregada mantém ignorância; ata com `acesa=None` produz `missing=False`, mas conserva `acesa_lavrada=None`. `app/escala/services/leitor_celula.py:114`, `:119`, `:137`, `:160`, `:174`, `:421`; `app/escala/tests/test_grade_alimentada.py:57`.

3. **Censo da lâmpada — fila.** O produtor `lampadas_do_dia` escreve True/None/False no ramo de DNA sem coluna, e valores booleanos nos demais ramos; a dispensa força True sem batida e o teto força None para marco pendente. `ata_do_dia` inclui a lista na ata. O novo campo `acesa_lavrada` é somente projeção em memória, lida pela tarefa e tratada como ignorância quando None. **Caso:** `acesa=True,luz=None,dispensada=...` significa dispensa, não batida ocorrida; a tarefa apenas cala a cobrança. `app/ponto/services/cartorio.py:638`, `:642`, `:644`, `:677`, `:689`, `:702`, `:752`, `:774`, `:881`; `app/escala/services/leitor_celula.py:137`; `app/chamados/services/tarefa_do_dia.py:81`, `:95`, `:211`, `:214`.

4. **Censo de `fora_vinculo` — fila.** O leitor escreve esse tipo a partir de admissão, demissão e fato lavrado; a tarefa, declaração, materialização e fechamento o consomem. **Caso:** admissão é inclusiva; antes dela um fato permite conservar o dia; demissão D sem fato é fora, D com fato permanece; depois da demissão o fato não vence a condição. Portanto “fato vence cadastro dos dois lados” não deve ser interpretado como permissão geral para fatos posteriores à demissão. `app/escala/services/leitor_celula.py:385`, `:389`, `:393`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/declaracao_texto.py:112`; `app/chamados/services/materializacao.py:239`; `app/ponto/services/fechamento.py:1036`.

5. **Censo de isenção — fila.** A fonte é `Colaborador.isento_ponto`; o juiz lê somente a flag. A tela Passe Livre grava pela porta de cadastro; a migration histórica também escreve a flag. O leitor publica `isento_ponto`, aplica o overlay de apresentação e a tarefa utiliza o valor publicado. **Caso:** o overlay pode mudar `missing` para False sem mudar `acesa_lavrada`; isso não cria cobrança porque a tarefa retorna antes pelo gate de isento. `app/colaboradores/models.py:294`; `app/core/constants.py:13`; `app/colaboradores/views.py:175`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/migrations/0043_isento_ponto.py:13`; `app/escala/services/leitor_celula.py:360`, `:441`, `:445`; `app/escala/utils.py:858`; `app/chamados/services/tarefa_do_dia.py:174`.

6. **Igualdade alimentada: nenhum contraexemplo válido encontrado; selo incompleto — fila.** Com o mesmo cadastro, células completas e vínculos completos do mesmo estado, os caminhos convergem para o mesmo cálculo; o mapa é recortado à janela e a carga efetivamente fornece a lista inteira de vínculos. **Caso:** o selo cobre somente um vínculo, células existentes e demissão; não demonstra múltiplos vínculos, ausência de vínculo/célula ou admissão. Um contraexemplo baseado em dois vínculos com mesma `data_inicio` seria inválido: existe unicidade `(colaborador,data_inicio)`. `app/escala/services/leitor_celula.py:361`, `:364`, `:372`, `:406`, `:411`; `app/ponto/turnos.py:2988`; `app/escala/tests/test_grade_alimentada.py:26`, `:33`, `:44`; `app/escala/models.py:1004`.

7. **Caso13 e grade não passam por ausência total de exercício — fila.** Caso13 possui contra-casos que exigem cobrança, além dos casos que exigem silêncio; o selo da grade exige igualdade, ausência de consultas às duas tabelas e triestado explícito. Entretanto Caso13 verifica comportamento, sem impedir uma futura cópia da regra que produza os mesmos resultados. **Caso:** remover os gates atuais quebra os casos silenciosos; simplesmente calar tudo quebra `test_vinculo_vigente_segue_cobrado` e demissão com fato. `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:576`, `:591`; `app/escala/tests/test_grade_alimentada.py:51`, `:52`, `:60`.

## 07/11 08:51 · sessao a5fd7da30510 · - · - · origem=Ronald (tmux)

- **Dono conclusivo: [nao verificado].** A L-099 cobra coerência determinística e idempotente com o cadastro existente; não permite atribuir a divergência ao cadastro apenas porque houve regeneração. (`app/docs/LEIS.md:122`)

- **Juiz exato, nesta ordem:** `folga_nao_definida` ou qualquer flag de ausência de vínculo → **CADASTRO**; `batida_fora_de_turno` → **BATIDA**; quantidade ímpar de `horas_do_dia` → **BATIDA**; `fase_deslocada` → **CADASTRO**; presença em Cadastro × Realidade → **CADASTRO**; resto → **ESTRUTURA**. (`app/ponto/management/commands/e6_oraculo.py:98`)

- **`furo_apurado=True`, `veredito='furo'`, `tem_entrada=False` e `tem_saida=False` não são argumentos desse juiz.** Assim, eles não fecham a classificação BATIDA. (`app/ponto/management/commands/e6_oraculo.py:53`)

- **Se forem realmente zero batidas no universo do E6**, zero é par e o ramo de paridade não resulta BATIDA; com todas as demais flags falsas, resulta ESTRUTURA. O código declara expressamente esse caso. (`app/ponto/management/commands/e6_oraculo.py:72`; `app/ponto/management/commands/e6_oraculo.py:106`; `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143`)

- **O previsto atual de 420, se produzido pela função indicada sobre os insumos reais, implica que a seleção atual encontrou vínculo e tipo de escala e que `eh_dia_trabalho` retornou exatamente `True`.** A função retornaria zero antes de chegar à geometria caso essas guardas falhassem. Isso não demonstra ausência de defeito de folga. (`app/escala/utils.py:2281`; `app/escala/utils.py:2287`; `app/escala/models.py:1241`)

**Falta exatamente, para reproduzir a decisão do juiz em 27/09:**

1. `horas_do_dia` efetivamente agrupadas pelo oráculo, derivadas de `batidas_apuraveis`, incluindo a janela ampliada; os indicadores da célula não substituem esse vetor. (`app/ponto/management/commands/e6_oraculo.py:141`)
2. `batida_fora_de_turno`, calculado com os turnos reais: exige realizado aberto e órfã real no universo apurável. (`app/ponto/management/commands/e6_oraculo.py:186`)
3. Resultado de `cadastro_do_dia_carregado(270, 2026-09-27)`: `folga_nao_definida`, `sem_vinculo_no_dia` e `sem_vinculo_em_dia_de_trabalho`. (`app/ponto/management/commands/e6_oraculo.py:172`; `app/ponto/supra_juiz.py:166`)
4. Pertinência de 27/09 à lista `dias_cadastro_x_realidade` do resultado do motor. (`app/ponto/management/commands/e6_oraculo.py:154`)
5. `fase_deslocada` pela lavra as-of usada no julgamento, ou o resultado falso dessa consulta. (`app/ponto/management/commands/e6_oraculo.py:164`)

Para auditar o cadastro bruto desses resultados: PK da geradora e suas datas de vigência; definição de folga do template e `FolgaDia` do mês. A seleção usa a geradora e rejeita datas fora de sua vigência; o defeito 6x1 depende de `folga_dia_semana` e existência de folga no mês. (`app/escala/alimentacao.py:269`; `app/escala/models.py:1241`)

## 07/11 08:51 · sessao 6689704095a9 · - · - · origem=Ronald (tmux)

- O escritor que corresponde à assinatura `dna_anterior` preenchido + `regenerada_em` + contador é `regenerar_celulas_vinculo`: ele reatribui **trabalha, escala_geradora e dna juntos**, guarda DNA anterior e atualiza carimbos. Isso não prova que os três valores tenham mudado: a condição aceita diferença em qualquer um deles. (`app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:271`)
- A regeneração por dia delega ao mesmo escritor; a descrição da auditoria distingue o rótulo “pela correcao do DIA”. (`app/ponto/portas/celula.py:734`)
- A auditoria pertinente usa **`acao='regenerar_celulas_vinculo'`, `modelo='EscalaColaborador'`, `objeto_id=ec.pk`**, com usuário opcional, motivo, janela e número de alterações; sua gravação é best-effort. (`app/ponto/portas/celula.py:334`, `app/ponto/portas/celula.py:342`, `app/ponto/portas/celula.py:350`)
- Os logs raspados contêm apenas `veredito_celula` de **33829 a 33834**; não contêm a regeneração de **33835**, nem a trilha do seu vínculo. Assim, **autor, disparador e ato concreto em 23/10 permanecem [nao verificado]**.
- Para obter a autoria, falta **ID do vínculo** e a linha completa de auditoria dessa ação na janela da regeneração: timestamp, `usuario`, `ator`, descrição e `valor_antes/valor_depois`. A vinculação ao usuário/processo é feita por `evento_kw` → `evento`, com fallback de request ou `sistema:<comando>`. (`app/core/observ.py:108`, `app/core/observ.py:117`, `app/core/observ.py:134`, `app/core/observ.py:146`)
- O marcador não certifica sozinho a última escrita de todos os campos: o gerador pode reescrever hoje/futuro sem atualizá-lo; o ramo fora da vigência grava `trabalha=False`/DNA sem carimbos; enriquecimento de DNA nulo também não os atualiza. (`app/ponto/portas/celula.py:237`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:865`)
- A edição humana normal deixaria `origem='editada'`, `editada_em` e `editada_por`; a reversão normal limparia `dna_anterior` e avançaria contador/carimbo. (`app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:786`)
- **Vigência não é campo da célula:** o signal observa `EscalaColaborador.data_inicio/data_fim` e chama regeneração quando mudam. Falta a trilha do vínculo para atribuir uma alteração histórica dessa vigência. (`app/escala/signals.py:25`, `app/escala/signals.py:56`, `app/escala/signals.py:71`)
- `dna_anterior` guarda somente a versão imediatamente anterior, sem valores anteriores de `trabalha`/geradora nem histórico completo. A mudança visível de pausa **19–20 → 20–21** não revela qual desses outros campos mudou. (`app/escala/models.py:1714`, `app/escala/models.py:1726`)

## 07/11 08:51 · sessao 0ffdc017cf7f · - · - · origem=Ronald (tmux)

1. **B4 realizado para o mutante declarado — fila.** O detector inclui `ponto_fechamentomensal`, a prova negativa o usa e o mutante injeta uma consulta real a `FechamentoMensal.objects.exists()`; a prova positiva exige encontrá-la. Não é comparação vazia: a fixture cria colaborador e vínculo antes da carga. Caso: `Caso10CompetenciaExportada`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:77`, `:85`, `:507`, `:518`, `:533`, `:539`, `:544`. Execução Django [nao verificado].

2. **O selo de relógio admite leitura fora da borda — bloqueia a certificação ampla.** A detecção exige chamada sem argumentos **e sem keywords**, além de reconhecer apenas cinco nomes literais. Os contraexemplos `timezone.localdate(value=None)` e `clock = timezone.now; return clock()` retornaram `[]` quando executei somente o helper AST extraído, em memória. Ambos escapam ao selo; os mutantes existentes só exercitam grafias simples. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:724`, `:735`, `:737`, `:755`, `:760`. O helper também ignora funções que deixem de ser `FunctionDef` de primeiro nível: `:743`.

3. **Caso13 morde a regressão B3 comportamental — fila.** Os testes exigem razões específicas e ausência de ações para demissão anterior, admissão posterior e isenção; há contracaso com fato na demissão exigindo `perguntar`, além de vínculo vigente. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:562`, `:569`, `:576`, `:584`, `:591`. A igualdade lote/casca compartilha o mesmo carregador e não demonstra, sozinha, uso do leitor único: `app/chamados/services/tarefa_do_dia.py:351`, `app/chamados/tests/test_e2_tarefa_do_dia.py:599`.

4. **O novo selo temporal cobre dois casos reais, mas não todo o contrato — fila.** Ele compara fato das 22h com perguntas às 21h/23h e usa instante UTC cuja data local ainda é D; essas expectativas distinguem retirada do limite temporal e retorno a `agora.date()`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:623`, `:627`, `:630`, `:637`, `:641`. Não exercita nesse selo os demais marcos nem igualdade exata com `ate`: o fato alterado é apenas `CelulaDia.veredito_em`, em `:630`.

5. **Lavra futura continua aceita, inclusive pelas fixtures — bloqueia vigência histórica estrita.** A fixture geral grava `julgado_em=D+3650`; `esta_stale` não verifica `julgado_em > agora`, podendo devolver `False` para uma lavra que ainda não existia. Caso demonstrável pelo corpo: veredito `nao`, lavra futura, janela `None`, marcos `{}` ⇒ `False`; a carga a aceita em `_lavra`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:127`, `app/chamados/veredito_lavrado.py:196`, `:203`, `:207`, `app/chamados/services/tarefa_do_dia.py:297`.

6. **`test_grade_alimentada` não certifica a equivalência em todo caso — fila.** A fixture contém um único vínculo e apenas a forma de ata com `lampadas`; o teste cobre filtragem de célula externa, demissão e triestado, mas não múltiplos vínculos, colaborador sem vínculo, admissão ou ata agregada. Referências: `app/escala/tests/test_grade_alimentada.py:20`, `:26`, `:28`, `:33`, `:44`, `:57`. O resultado efetivo para esses casos adicionais cabe à auditoria do leitor [nao verificado neste subtrabalho].

7. **Não classificar a convivência dos cinco respondedores como juiz clandestino — fila E3 declarada.** O registro autoriza `_C19`, declara a pureza e mantém explicitamente os cinco respondedores até E3; a tabela repete o contrato. Referências: `app/core/juizes.py:150`, `:154`, `:285`; `app/docs/JUIZES.md:73`, `:80`. Para dia sem trabalho, a nova tarefa chama o adaptador que delega ao juiz registrado, sem copiar o predicado: `app/chamados/services/tarefa_do_dia.py:184`, `app/ponto/supra_juiz.py:65`, `:69`, `:71`; autoridade em `app/docs/JUIZES.md:59`.

8. **0514(3), universo da sombra inteiro, não está demonstrado — bloqueia sua certificação.** A API percorre `[ini,fim]` e a carga recebe colaboradores explicitamente; isso não prova que o chamador da sombra seleciona toda célula do horizonte. Referências: `app/escala/services/leitor_celula.py:375`, `:377`, `:444`, `app/chamados/services/tarefa_do_dia.py:340`. Não encontrei script/artefato da sombra na árvore pesquisada; seleção efetiva e execução do universo [nao verificado].

## 07/11 08:49 · sessao f13e743f1403 · V2-I2 · sonda · origem=Code

**27/09 é o candidato visível aos +420 minutos, mas a sonda não fecha a atribuição por dia nem o dono L-099.** Ela informa FM=10500 e soma dos DP motor=10500; os 10920 aparecem no enunciado, sem resultado novo por dia: todos os `DP_atual` estão `None`. [nao verificado]

**1) Dias e mudanças observáveis**

Na tabela, “anterior” significa exclusivamente `dna_anterior`, não o DNA usado para calcular o DP gravado. O modelo guarda apenas uma versão anterior, não toda a sequência de regenerações. (`app/escala/models.py:1714`)

| Dias | DNA anterior → atual, no trecho fornecido | DP gravado |
|---|---|---|
| **27/09** | 15–23, pausa **19–20 → 20–21** | **0**. [nao verificado] |
| 21, 22, 25, 26, 28 e 29/09; 02, 04, 05, 06, 09, 11, 12, 13, 16, 18, 19 e 20/10 | Mesma mudança de pausa; entrada e saída permanecem 15–23 | 420 em cada dia. [nao verificado] |
| 23 e 30/09; 07 e 14/10 | `marcos=null`, tipo **313** → 15–23, pausa 20–21 | 420 em cada dia. [nao verificado] |
| 24/09; 01, 08 e 15/10 | 15–23, pausa 19–20 → `marcos=null`, tipo **397** | 0, exceto **01/10=None**. [nao verificado] |
| 03, 10 e 17/10 | `marcos=null`, tipo **397** → 15–23, pausa 20–21 | 420 em cada dia. [nao verificado] |

**A mudança de pausa, isoladamente, acrescenta zero minutos:** tanto 19–20 como 20–21 descontam 60 de uma jornada de 480, resultando em 420. Essa é a aritmética implementada pelo previsto. (`app/escala/utils.py:2293`; `app/escala/utils.py:2326`)

Em **27/09**, portanto, o contraste relevante é **DP gravado=0 versus DNA atual com geometria de 420**; não é aumento da duração entre os dois DNAs. [nao verificado] Para afirmar que o previsto atual desse dia é 420, faltam `trabalha=True` e vínculo válido: a função verifica essas condições antes de calcular os marcos, e `eh_dia_trabalho` lê `cel.trabalha`. (`app/escala/utils.py:2281`; `app/escala/models.py:1094`)

Os horários de regeneração apresentados são:

| Células | Última regeneração registrada, UTC | Contador |
|---|---|---|
| **27/09** | **23/10/2026 13:04:42.382279** | 2. [nao verificado] |
| Demais dias, exceto os três abaixo | 23/10/2026, entre 13:04:42.372643 e 13:04:42.414014 | 2. [nao verificado] |
| 03/10 | 23/10/2026 16:49:42.903745 | 4. [nao verificado] |
| 10/10 | 23/10/2026 16:49:42.910592 | 4. [nao verificado] |
| 17/10 | 23/10/2026 16:49:42.917070 | 4. [nao verificado] |

Todas aparecem com `origem='gerada'` e `editada_em=None`; a sonda não identifica a pessoa responsável. [nao verificado] Isso é compatível com regeneração: esse escritor preserva esses campos e registra separadamente DNA anterior, horário e contador. (`app/ponto/portas/celula.py:263`; `app/ponto/portas/celula.py:271`)

**2) Escritores e histórico**

| Operação | Escritor efetivo e consequência |
|---|---|
| `gerar_celulas` | O comando reexporta a porta; `gerar_celulas_periodo` cria trabalha/geradora/DNA. (`app/ponto/management/commands/gerar_celulas.py:13`; `app/ponto/portas/celula.py:848`) |
| **Regeneração registrada nas células** | `regenerar_celulas_vinculo` copia DNA para `dna_anterior`, grava o novo, atualiza horário e incrementa contador. É o escritor correspondente aos marcadores apresentados. (`app/ponto/portas/celula.py:271`) |
| Regeneração de um dia | Delega ao mesmo escritor; os marcadores não distinguem esse chamador da regeneração por vínculo. (`app/ponto/portas/celula.py:734`) |
| Edição humana | `editar_dia` grava `origem='editada'`, `editada_em` e `editada_por`. Essa assinatura não aparece na sonda. (`app/ponto/portas/celula.py:599`) [nao verificado] |
| Outros caminhos | O gerador pode atualizar hoje/futuro sem atualizar os marcadores de regeneração; o ramo fora da vigência também altera trabalha/DNA sem esses marcadores. Logo, eles não certificam toda escrita posterior. (`app/ponto/portas/celula.py:865`; `app/ponto/portas/celula.py:237`) |
| Reversão | Restaura DNA anterior, mas **limpa** `dna_anterior` e avança contador/horário. (`app/ponto/portas/celula.py:783`) |

**Quem disparou cada alteração permanece desconhecido.** A regeneração recebe autor opcional e tenta registrá-lo na auditoria; a célula não tem campo `regenerada_por`. (`app/ponto/portas/celula.py:334`; `app/escala/models.py:1718`) A auditoria desses atos não foi fornecida. [nao verificado]

**Não foi possível conferir o `git log` do HEAD:** esta cópia não contém `.git`; presença, datas e ancestralidade dos commits no hash informado ficam [nao verificado]. A evidência documental local permite apenas estas distinções: [nao verificado]

- A **R4 de 27/09** corrigiu a leitura da **pausa do realizado** para célula com marcos nulos; preservou o fallback de `marcos_do_dia`. (`app/escala/models.py:1138`; `app/escala/models.py:1151`)
- O previsto atual ainda conserva esse fallback quando não encontra pausas; o código registra explicitamente a colisão R4 × F1. (`app/escala/utils.py:2307`; `app/escala/utils.py:2317`; `app/escala/utils.py:2335`)
- Há registro de alimentação `celulas/folgas` em **26/09**, incluindo correção de DNA alimentado que ficava invisível. (`app/escala/utils.py:2257`)
- A cura documentada especificamente como **PREVISTO** é de **02/10**, para descontar todas as pausas, com identificadores `fecb9a67` e `07d89f21`; sua inclusão no histórico do HEAD não pôde ser conferida. (`app/docs/TICKETS.md:246`) [nao verificado]

**3) Dono L-099**

**Dono conclusivo dos +420: [nao verificado].** A L-099 exige coerência com o cadastro existente; regeneração, sozinha, não prova cadastro errado nem defeito estrutural. (`app/docs/LEIS.md:122`)

O juiz existente usa folga indefinida, ausência de vínculo, batida fora de turno, paridade, fase deslocada e presença em Cadastro × Realidade; esses insumos não acompanham a sonda. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:113`) [nao verificado]

Para concluir faltam:

- **Previsto novo por dia**, com identificação da execução que produziu 10920, esclarecendo também `DP_prev=None` em 01/10. [nao verificado]
- **`trabalha`, geradora, vigências e DNAs completos**, para aplicar as guardas do previsto. (`app/escala/utils.py:2281`)
- **Trilha dos atos**, para identificar autor, motivo e versões intermediárias. (`app/escala/models.py:1714`; `app/ponto/portas/celula.py:334`)
- **Insumos do juiz de dono e histórico Git**, para fechar respectivamente L-099 e a relação com alguma cura commitada. (`app/ponto/management/commands/e6_oraculo.py:113`) [nao verificado]

## 07/11 08:47 · sessao 5a0fbffa6c33 · - · - · origem=Ronald (tmux)

| Operação | Escrita e prova |
|---|---|
| `gerar_celulas` | O command reexporta as funções da porta em `app/ponto/management/commands/gerar_celulas.py:10-13` e chama `gerar_celulas_periodo` em `:309-311`. |
| Nascimento da célula | `gerar_celulas_periodo` grava `origem='gerada'`, `trabalha`, geradora e DNA em `app/ponto/portas/celula.py:848-852`; `gerada_em` é `auto_now_add`, em `app/escala/models.py:1702`. A regeneração também pode criar célula ausente em `app/ponto/portas/celula.py:258-260`. |
| Reescrita registrada da sonda | `regenerar_celulas_vinculo` restringe a célula existente a `origem='gerada'` e exige alteração de trabalha/geradora/DNA em `app/ponto/portas/celula.py:263`; guarda o DNA anterior, troca o DNA, registra horário e incrementa contador em `:271-277`. Não altera `origem`, `gerada_em` ou `editada_em` nesse ramo, conforme os campos salvos em `:276-277`. |
| Regeneração por dia | `regenerar_celulas_dia` usa exatamente o escritor acima, com horizonte desabilitado, em `app/ponto/portas/celula.py:734-737`. Portanto os marcadores da célula não distinguem esse chamador do ato por vínculo. |
| Edição humana | `editar_dia` exige autor e motivo em `app/ponto/portas/celula.py:559-562`; grava `origem='editada'`, `editada_em`, `editada_por`, trabalha/geradora/DNA em `:599-609`, e trilha antes/depois em `:610-618`. A regeneração comum não alcança essa origem, pela condição em `:263`. |
| Reversão | `reverter_regeneracao_dia` restaura DNA anterior, **limpa** `dna_anterior`, incrementa contador e muda horário em `app/ponto/portas/celula.py:783-795`. A trilha recebe ambos os DNAs e autor em `:797-803`. |
| Outra escrita sem esses marcadores | No gerador, passado só recebe DNA se estiver nulo (`app/ponto/portas/celula.py:856-861`); hoje/futuro pode mudar trabalha/geradora/DNA **sem** guardar anterior ou atualizar regeneração (`:865-871`). O ramo fora de vigência da própria regeneração também muda trabalha/DNA sem esses marcadores (`:237-245`). |

`dna_anterior` é apenas a versão imediatamente anterior, e o histórico completo pertence ao `LogAuditoria`, conforme `app/escala/models.py:1714-1717`. Assim, `regeneracoes=2/4` não permite reconstruir as duas/quatro versões intermediárias.

A trilha da regeneração recebe `autor` opcional e registra ação `regenerar_celulas_vinculo`, vínculo, intervalo, motivo e quantidade em `app/ponto/portas/celula.py:331-349`. A célula não contém um campo `regenerada_por`: contém horário/contador/DNA anterior em `app/escala/models.py:1718-1729`, enquanto o campo de pessoa disponível é `editada_por` em `:1732-1735`. Quem executou os atos apresentados na sonda permanece `[nao verificado]`.

Há vários disparadores possíveis: alteração de vigência (`app/escala/signals.py:71-73`), alteração de âncora (`:117-119`) e edição de template (`app/escala/services/cadastro_tipo.py:307-320`, `:348-350`). Os dois signals e a propagação de template mostrados chamam a regeneração sem fornecer `autor`, conforme esses mesmos trechos; portanto até a trilha pode ter usuário nulo. A sonda não identifica qual desses disparadores produziu cada ato `[nao verificado]`.

## 07/11 08:47 · sessao e5b1b97e3c3d · - · - · origem=Ronald (tmux)

**Histórico Git: não verificável nesta cópia.** A raiz não contém `.git`; não consultei outro checkout. Logo, hash/autoria/data/ancestralidade de commits no HEAD e “cura commitada em 27/09” ficam **[nao verificado]**. [nao verificado]

O código/documentação local permitem distinguir:

- **R4 de 27/09** corrigiu a leitura da **pausa do realizado**, quando uma célula existente tem `dna.marcos=null`: o template fornecia pausa inexistente. A implementação criou leitor próprio; preservou o fallback de `marcos_do_dia`, pois os testes exigiam esse comportamento para folga. Prova: `app/escala/models.py:1138-1158`, fallback atual em `app/escala/models.py:1124-1133`. A documentação registra R4 fechada em 28/09, com +75,12 h em 27 colaboradores: `app/docs/BACKLOG.md:1077`. Isso não demonstra relação com os +420 de col270.
- **O previsto não recebeu integralmente essa guarda R4.** A fonte declara que era o leitor faltante; hoje lê todas as pausas, mas, quando elas retornam vazio, continua descontando `hii/hfi` devolvidos por `marcos_do_dia`. A colisão entre R4 e F1 é explicitamente registrada: `app/escala/utils.py:2307-2340`.
- A cura documentada de **PREVISTO** é de **02/10**, para descontar **duas pausas**, com identificadores `fecb9a67` e `07d89f21`; o próprio registro diz que a guarda R4 ficou fora: `app/docs/TICKETS.md:246`. Esses identificadores são evidência documental; sua presença no git log do HEAD continua **[nao verificado]**.
- O comentário registra que a alimentação `celulas/folgas` entrou em **26/09**, para custo e DNA alimentado invisível: `app/escala/utils.py:2257-2266`. Não prova por si só data/commit no histórico.

**L-099 e limite de atribuição:**

- Lei: coerência, determinismo e idempotência **com o cadastro que tem**; donos ESTRUTURA/CADASTRO/BATIDA, somente ESTRUTURA vira cura de código: `app/docs/LEIS.md:122`.
- O juiz pede dados não fornecidos na sonda: folga não definida, vínculo válido, batida fora de turno, paridade, fase deslocada e presença em `dias_cadastro_x_realidade`. Executável em `app/ponto/management/commands/e6_oraculo.py:98-110`; montagem dos insumos descrita em `:113-125`.
- Portanto, não é válido chamar o delta de CADASTRO apenas porque há regeneração, nem ESTRUTURA por resíduo sem carregar os insumos. Com ausência de DP novo por dia e sem `trabalha`/DNA integral/vigências/batidas/CxR, dono conclusivo dos +420 = **[nao verificado]**.

Uma distinção útil para o laudo: leitor da célula pode priorizar `ata.minutos_previstos` e só consultar a autoridade quando ausente (`app/escala/services/leitor_celula.py:382-395`), enquanto `minutos_previstos_do_dia` verifica vínculo, `trabalha` e marcos (`app/escala/utils.py:2277-2295`). A sonda não trouxe ata nem execução nova por dia.

## 07/11 08:30 · sessao 980fb726544f · MOTOR-LE-O-TURNO-INSTRUMENTO · censo · origem=Code

**Os critérios AST dão números diferentes; não escolhi um deles.** No escopo da obra, obtive **A1=1, A2=8, B1=17 e B2=18 funções**. São contagens sintáticas: A2 inclui adaptadores que recebem pares, e B2 inclui datas de competência ou prazo. Portanto, o número de **violações semânticas** continua **[não verificado]**. Exemplos dessas diferenças: `app/ponto/adaptador_motor.py:29-43`, `app/ponto/services/fechamento.py:1567-1569`.

| Universo da execução | A1 | A2 | B1 | B2 |
|---|---:|---:|---:|---:|
| Escopo da obra | 1 | 8 | 17 | 18 |
| Resto da árvore inventariada | 5 | 7 | 66 | 154 |
| Árvore inteira inventariada | 6 | 15 | 83 | 172 |
| Somente `app/` | 6 | 15 | 82 | 171 |
| Árvore, reaproveitando a exclusão de medição concreta do molde | 5 | 14 | 83 | 171 |
| Somente `app/`, com essa exclusão | 5 | 14 | 82 | 170 |

Os totais acima são resultados da execução reproduzida ao fim; sua equivalência à definição semântica do contrato é **[não verificado]**. A exclusão de medição concreta é `e6_piso_autopsia::handle`, declarada em `app/core/medida_leitor_do_dia.py:282-283`, com pareamento em `app/ponto/management/commands/e6_piso_autopsia.py:94-95`.

1. **Como o molde define universo, critério e `NAO_VE`.**

O universo de STATUS é todo `.py` sob a raiz `app/`, por `os.walk`, excluindo diretórios `tests`, `migrations`, `staticfiles`, `node_modules`, `logs`, `media`, `__pycache__`, diretórios ocultos e arquivos começados por `test_`. Management não é excluído em bloco. VALOR usa outra abrangência: a lista fechada `LEITORES`. `app/core/medida_leitor_do_dia.py:32-45`, `:151-152`, `:240-251`, `:388-394`.

STATUS conta **funções** que produzem pelo menos duas palavras distintas do vocabulário e têm alguma produção sob escolha. Produção significa atribuição, retorno ou ramo de expressão condicional nas formas explicitadas; função aninhada pertence a si própria. O vocabulário vem das chaves AST de `ROTULO_STATUS`, acrescidas dos paralelos. `app/core/medida_leitor_do_dia.py:160-190`, `:195-236`.

VALOR conta **sítios de aritmética** sobre atributos de dinheiro: `BinOp`, `AugAssign` ou agregadores reconhecidos, incluindo algumas formas de `getattr` literal. Assim, `numero` mistura funções de STATUS com sítios de VALOR; não significa “número de funções” uniformemente. `app/core/medida_leitor_do_dia.py:70-115`, `:358`, `:390-401`.

As exceções têm categorias explícitas; previsão e presença são condicionadas ao vocabulário produzido. O molde também conserva quantidade de literais dentro de cada função, para uma decisão nova numa função já contada poder alterar a catraca. `app/core/medida_leitor_do_dia.py:254-277`, `:317-321`, `:374-382`, `:406-409`.

O `NAO_VE` publicado enumera: indireção sem literal; decisões booleanas ou de relógio que não viram palavra; função de uma palavra; HTML/JS; vocabulário novo; arquivo fora da lista de VALOR; `BoolOp` que o detector de VALOR não atravessa; e mudança que reutiliza a mesma quantidade de literais. `app/core/medida_leitor_do_dia.py:342-351`.

Falhas de leitura ou parse entram em `ilegiveis`; o resultado as publica, em vez de tratá-las como ausência de achados. A fonte ilegível do vocabulário levanta `LookupError`. Embora a documentação diga “ast/os”, esta cópia também importa `io`. `app/core/medida_leitor_do_dia.py:22-24`, `:163-173`, `:363-368`, `:412-415`.

2. **Critérios propostos e suas diferenças.**

Os quatro critérios abaixo são propostas de aproximação AST, **[não verificado como critério oficial]**. Cada contador soma uma função uma vez; preserva todas as linhas que casaram. Não há isenção da função inteira por ela também consultar o juiz: isso esconderia fallbacks existentes em `app/ponto/services/dia_pago.py:128-132` e `app/ponto/motor_calculo_v2.py:2942-2955`.

| Critério | Forma AST contada | Limitação principal |
|---|---|---|
| **A1 — seleção posicional** | `Call` de `range(início, fim, 2)` ou `zip` cujos dois primeiros argumentos são fatias com passo literal 2. | Também encontra pausas e índices de marcos; não reconhece todo pareador por estado. Exemplo: `app/escala/utils.py:496`, `:520`. |
| **A2 — seleção ou recomposição** | A1, mais `append(Tuple/List)` com primeira posição marcada lexicalmente como entrada e segunda como saída, propagando marcas por atribuições e iterações locais. | Encontra recomposição, mas também adaptadores de pares recebidos. Exemplos: `app/ponto/motor_calculo_v2.py:688`, `app/ponto/adaptador_motor.py:43`. |
| **B1 — extração com origem lexical** | `.date()` sem argumentos, `localdate(x)` ou `data_local(x)`, quando o operando contém ou herda nomes/atributos temporais declarados no script. | A marca lexical não prova tipo nem pertença; aliases sem essas marcas escapam. Exemplo capturado: `app/escala/detector_proposta.py:57-58`. |
| **B2 — extração civil ampla** | As mesmas chamadas de B1, sem exigir marca lexical. | Inclui calendário de hoje, conversão de formato, competência e prazo. Exemplos: `app/colaboradores/models.py:824-825`, `app/chamados/veredito_lavrado.py:182-185`. |

A cláusula literal de A, **“sem receber pares do juiz”**, não é demonstrada por esses padrões locais. A2 deliberadamente expõe esse limite: `periodos_de_turnos` recebe turnos e só lê suas pontas, mas casa com sua forma sintática. Portanto, interpretar A2=15 como quinze pareadores independentes seria **[não verificado]**; o contraexemplo está em `app/ponto/adaptador_motor.py:13-19`, `:29-44`.

Para excluir o juiz e sua composição, o script parte de `turnos_do_colab` e `parear_turnos` e calcula o fechamento de chamadas estáticas resolvidas por nome/import. Não exclui `turnos.py` inteiro. A composição entre turno e juiz da batida existe na chamada `papel_por_minuto_da_ata → periodos_do_dia`. `app/core/juizes.py:196`, `app/ponto/turnos.py:1580`, `:1631`, `:1937`, `:1990`.

Esse fechamento também tem limite: chamadas por parâmetros, métodos de objetos e resolução dinâmica não ficam provadas. A completude da exceção de composição é **[não verificado]**; o script publica esse limite em `NAO_VE`.

O inventário principal percorre a árvore inteira, mantendo comandos de medição para não confundir localização com uso. Também fornece o recorte `app/` do molde e a variante que reaproveita sua exclusão concreta de `e6_piso_autopsia`. Não exclui todo management: o molde também não faz isso. `app/core/medida_leitor_do_dia.py:240-251`, `:282-283`.

3. **Cobertura dos sítios nomeados no aval.**

| Sítio | Contado? | Evidência e limite |
|---|---|---|
| **MOTOR-PASSA-O-TURNO-A-VOLTA / resíduo 573** | **B1 e B2** em `MotorBase._julgar_pontualidade`; A1/A2 não identificam especificamente essa pergunta. | Passa `e.date()` e as pontas do período a `cumpriu_a_volta_do_intervalo`. O defeito “período em vez de turno” não exige construir par novo: é falso negativo de A como cobertura da obra. `app/ponto/motor_calculo_v2.py:2117-2119`. O 573 está registrado na documentação, sem reprodução de produção aqui: `app/docs/BACKLOG.md:779`; número atual **[não verificado]**. |
| **col882 turno aberto** | O comportamento do caso não é demonstrado pelos contadores. | Os calculadores retornam cedo sem saída; isso não prova quais pares as cinco batidas dadas produzirão. `app/ponto/motor_calculo_v2.py:2752-2755`, `:2879-2882`, `:3217-3220`. Execução desse caso **[não verificado]**. A2 também não reconhece isoladamente `append((entrada, None))`, forma presente em `app/ponto/calculador/alimentacao.py:88`, `:111`. |
| **JUIZ-DO-PAR-DUAS-ENTRADAS** | **Não em A1/A2 nas duas portas.** `turnos_via_autoridade` entra em B1/B2 por outra razão. | Uma porta chama `turnos_de_batidas`; outra chama `parear_turnos` com alimentação própria. Divergência entre receitas não exige `range(...,2)` nem `append((e,s))` local: falso negativo de A. `app/ponto/motor_calculo_v2.py:867-882`, `:475-506`. O B dessa porta é apenas a extração para consulta em `:470-474`. |
| **Partido `:3012 × :2446`** | **B1 e B2** nas funções relevantes. | Nesta cópia, o mecanismo verificável está no fallback civil e herança por gap em `app/ponto/motor_calculo_v2.py:2403-2408`, e nos fallbacks do partido em `:2942`, `:2955`. A âncora `:3012` agora é comentário. `app/ponto/motor_calculo_v2.py:3012`. |
| **`dia_pago._dia_de`** | **B1 e B2.** | Depois de tentar o mapa, retorna a data civil da entrada. `app/ponto/services/dia_pago.py:128-132`. |
| **`_dia_da_jornada_do_par`** | Não apareceu como função no inventário AST. | O nome está na documentação, mas sua implementação nesta cópia é **[não verificado]**. `app/docs/BACKLOG.md:1821`. |
| **Split de `calculador/alimentacao.py:73-108`** | **A2 sim; A1/B1/B2 não.** | Lê períodos, dia do mapa e pontas efetivas; depois divide o período pelos intervalos. É falso positivo de A2 para “pareamento de batidas cruas”; a ausência em A1 não é falso negativo dessa definição. `app/ponto/calculador/alimentacao.py:73-83`, `:106-108`, `:128-158`. |

B também não identifica, como regra autônoma, a herança de dia por gap: a função atual é contada por conter extrações civis, mas a decisão de `:2405-2408` não tem detector próprio. Adicionar outra decisão desse tipo numa função já contada pode preservar N. `app/ponto/motor_calculo_v2.py:2403-2408`. O molde evita parte desse problema mantendo uma medida interna de sítios, além do número de funções. `app/core/medida_leitor_do_dia.py:320-321`, `:406-409`.

4. **Falsos positivos prováveis, além do split.**

| Critério | Sítio | Por que provavelmente excede a pergunta |
|---|---|---|
| A1/A2 | `app/escala/utils.py:496` | Forma pares S→E de pausa/papel, não pares laborais E→S. |
| A1/A2 | `app/escala/utils.py:520` | Enumera posições de pausas dos marcos. |
| A1/A2 | `app/ponto/turnos.py:810-812` | Lê pares de pausa já marcados no turno. |
| A1/A2 | `app/ponto/management/commands/vigiar_motor_autoridade.py:75-86` | Soma marcas internas de turnos recebidos. |
| A2 | `app/ponto/adaptador_motor.py:29-44` | Converte pontas de turnos recebidos. |
| A2 | `app/ponto/motor_calculo_v2.py:164-172` | Une intervalos já fornecidos. |
| A2 | `app/ponto/motor_calculo_v2.py:1883-1888` | Recorta intervalos recebidos à janela. |
| B1/B2 | `app/ponto/motor_calculo_v2.py:97-98` | Percorre faixas civis para cálculo noturno; pertença do turno não é demonstrada por essa extração. |
| B1/B2 | `app/ponto/motor_calculo_v2.py:470-474` | Amplia consulta de DNA/marcos. |
| B1/B2 | `app/ponto/janelas.py:84-85` | Responde competência fechada. |
| B1/B2 | `app/ponto/services/fechamento.py:1567-1569` | Normaliza `date`/`datetime` de um fato. |
| B2 | `app/folha/pdf_export.py:43-47` | Converte formato `AAAAMMDD`. |
| B2 | `app/colaboradores/models.py:821-825` | Obtém mês atual para custo de transporte. |
| B2 | `app/chamados/veredito_lavrado.py:182-185` | Compara prazo de expiração e lavratura. |
| B1/B2 | `bin/disjuntor.py:1512-1539` | O homônimo `timestamp` é de mensagens do assistente, não de batidas de ponto. |

5. **Script integral.**

A execução foi por `python3 -B - .`, com o texto abaixo em stdin, na raiz informada; não foi criado arquivo de script. Registro persistido dessa execução: **[não verificado]**. A saída integral vem depois do código. `@NN` indica a linha do nó contado; o número após a função é a linha da definição.

```python
import ast
import os

FUN = (ast.FunctionDef, ast.AsyncFunctionDef)
SKIP = {'tests', 'migrations', 'staticfiles', 'node_modules',
        'logs', 'media', '__pycache__'}
E = {'entrada', 'efetivo_entrada', 'ent', 'e', '_e', '_ent', 'e_ts'}
S = {'saida', 'efetivo_saida', 'sai', 's', '_s', 's_', 's_ts'}
T = {'timestamp', 'ts', 'ts_entrada', 'ts_saida', 'instante',
     'quando', 'inicio', 'fim'}
NAO_VE = (
    'Sem tipos nem execucao: nomes/atributos podem ser homonimos.',
    'Fluxo intrafuncao insensivel a ordem, ramos, sobrescritas e alcance.',
    'Nao prova origem interprocedural dos argumentos nem pares do juiz.',
    'Chamadas dinamicas, getattr calculado e aliases entre modulos escapam.',
    'Composicao: so fechamento de chamadas estaticas resolvidas; self/metodos '
    'em objetos e parametros nao sao resolvidos.',
    'Nao ve HTML/JS, codigo gerado, codigo em strings ou corpo de modulo.',
    'B nao ve toda regra de meia-noite sem extracao civil nem heranca por gap.',
    'A1 ve so passo 2; A2 acrescenta tuplas E/S: nao e censo semantico exato.',
    'Nao distingue exibicao, competencia, prazo, pausa e par laboral.',
)

def own(node):
    yield node
    for ch in ast.iter_child_nodes(node):
        if not isinstance(ch, FUN + (ast.ClassDef, ast.Lambda)):
            yield from own(ch)

def dotted(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        head = dotted(node.value)
        return head + '.' + node.attr if head else ''
    return ''

def targets(node):
    return {n.id for n in ast.walk(node)
            if isinstance(n, ast.Name) and isinstance(n.ctx, ast.Store)}

def aliases(nodes, package):
    out = {}
    for node in nodes:
        if isinstance(node, ast.Import):
            for a in node.names:
                out[a.asname or a.name.split('.')[0]] = (
                    a.name if a.asname else a.name.split('.')[0])
        elif isinstance(node, ast.ImportFrom):
            base = node.module or ''
            if node.level:
                prefix = package.split('.')
                base = '.'.join(prefix[:len(prefix)-node.level+1]
                                + ([base] if base else []))
            for a in node.names:
                if a.name != '*':
                    out[a.asname or a.name] = base + '.' + a.name
    return out

def units(tree):
    def visit(node, prefix=''):
        for ch in ast.iter_child_nodes(node):
            if isinstance(ch, FUN + (ast.ClassDef,)):
                name = prefix + ch.name
                if isinstance(ch, FUN):
                    yield name, ch
                yield from visit(ch, name + '.')
            else:
                yield from visit(ch, prefix)
    return visit(tree)

def tags(node, env):
    if node is None:
        return set()
    out = set()
    for n in own(node):
        word = None
        if isinstance(n, ast.Name):
            word = n.id
            out |= env.get(word, set())
        elif isinstance(n, ast.Attribute):
            word = n.attr
        elif (isinstance(n, ast.Call) and dotted(n.func) == 'getattr'
              and len(n.args) > 1 and isinstance(n.args[1], ast.Constant)):
            word = n.args[1].value
        elif (isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute)
              and n.func.attr == 'get' and n.args
              and isinstance(n.args[0], ast.Constant)):
            word = n.args[0].value
        elif isinstance(n, ast.Subscript) and isinstance(n.slice, ast.Constant):
            word = n.slice.value
        if word in E:
            out.add('E')
        if word in S:
            out.add('S')
        if word in T:
            out.add('T')
    return out

def environment(nodes):
    env = {}
    while True:
        before = {k: frozenset(v) for k, v in env.items()}
        for n in nodes:
            pairs = []
            if isinstance(n, ast.Assign):
                pairs = [(t, n.value) for t in n.targets]
            elif isinstance(n, ast.AnnAssign) and n.value is not None:
                pairs = [(n.target, n.value)]
            elif isinstance(n, (ast.For, ast.AsyncFor, ast.comprehension)):
                pairs = [(n.target, n.iter)]
            for dst, src in pairs:
                if (isinstance(dst, (ast.Tuple, ast.List))
                        and isinstance(src, (ast.Tuple, ast.List))
                        and len(dst.elts) == len(src.elts)):
                    bindings = zip(dst.elts, src.elts)
                else:
                    bindings = [(dst, src)]
                for left, right in bindings:
                    val = tags(right, env)
                    for key in targets(left):
                        env.setdefault(key, set()).update(val)
        if before == {k: frozenset(v) for k, v in env.items()}:
            return env

def step2(n):
    if isinstance(n, ast.Call) and dotted(n.func) == 'range':
        return (len(n.args) == 3 and isinstance(n.args[2], ast.Constant)
                and n.args[2].value == 2)
    if isinstance(n, ast.Call) and dotted(n.func) == 'zip':
        return len(n.args) >= 2 and all(
            isinstance(a, ast.Subscript) and isinstance(a.slice, ast.Slice)
            and isinstance(a.slice.step, ast.Constant)
            and a.slice.step.value == 2 for a in n.args[:2])
    return False

def scan(node, imports):
    nodes = list(own(node))
    env = environment(nodes)
    found = {'A1': [], 'A2': [], 'B1': [], 'B2': []}
    for n in nodes:
        if step2(n):
            found['A1'].append(n.lineno)
        if isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute):
            if n.func.attr == 'append' and n.args:
                pair = n.args[0]
                if (isinstance(pair, (ast.Tuple, ast.List)) and len(pair.elts) >= 2
                        and 'E' in tags(pair.elts[0], env)
                        and 'S' in tags(pair.elts[1], env)):
                    found['A2'].append(n.lineno)
        if not isinstance(n, ast.Call):
            continue
        call = dotted(n.func)
        head, dot, tail = call.partition('.')
        call = imports.get(head, head) + (dot + tail if dot else '')
        last = call.split('.')[-1]
        arg = None
        if (isinstance(n.func, ast.Attribute) and n.func.attr == 'date'
                and not n.args and not n.keywords):
            arg = n.func.value
        elif last in {'localdate', 'data_local'}:
            arg = n.args[0] if n.args else next(
                (k.value for k in n.keywords if k.arg == 'value'), None)
        if arg is not None:
            found['B2'].append(n.lineno)
            if tags(arg, env):
                found['B1'].append(n.lineno)
    found['A2'] += found['A1']
    return {k: sorted(set(v)) for k, v in found.items()}

def census(root, extra=None, emit=True):
    log = print if emit else lambda *args: None
    files, errors, records, modules = [], [], {}, {}
    for directory, dirs, names in os.walk(root):
        dirs[:] = sorted(d for d in dirs if d not in SKIP
                         and not d.startswith('.'))
        for name in sorted(names):
            if name.endswith('.py') and not name.startswith('test_'):
                files.append(os.path.relpath(os.path.join(directory, name), root))
    for rel in sorted(files):
        try:
            with open(os.path.join(root, rel), encoding='utf-8-sig') as fh:
                tree = ast.parse(fh.read(), filename=rel)
        except (SyntaxError, UnicodeError, OSError) as exc:
            errors.append((rel, type(exc).__name__))
            continue
        path = rel[:-3].replace(os.sep, '.')
        module = path[4:] if path.startswith('app.') else path
        if module.endswith('.__init__'):
            module = module[:-9]
            package = module
        else:
            package = module.rpartition('.')[0]
        imp = aliases(own(tree), package)
        modules[module] = imp
        for name, node in units(tree):
            records[module + '.' + name] = (rel, name, node, imp, module, package)
    if extra is not None:
        tree = ast.parse(extra, filename='<mutante em memoria>')
        for name, node in units(tree):
            records['mutante.' + name] = (
                '<mutante em memoria>', name, node, {}, 'mutante', '')
    def resolve(call, rec):
        rel, name, node, imp, module, package = rec
        local = dict(imp)
        local.update(aliases(own(node), package))
        raw = dotted(call.func)
        head, dot, tail = raw.partition('.')
        if head in local:
            return local[head] + (dot + tail if dot else '')
        if dot:
            return module + '.' + raw
        prefix = name.split('.')
        for i in range(len(prefix), -1,

[truncado em 20000 de 46612 caracteres]

## 07/11 08:19 · sessao 48640afec29c · - · - · origem=Ronald (tmux)

Fora do escopo da obra, os candidatos A encontrados exigem distinguir par laboral de pausa/papel:

- `app/escala/utils.py::_papeis_por_alternancia:477`: `zip(_ks[1:-1:2], _ks[2:-1:2])` forma pares S→E de pausa e produz papéis, não períodos E→S (`app/escala/utils.py:496-498`). Recebe `locs, consumed`; `_match_marcos` o chama quando `turno is None` (`app/escala/utils.py:585-598`). Um critério genérico “zip de fatias passo 2” o contará; um critério restrito a pares laborais E→S não o contará.
- `app/escala/services/cadastro_tipo.py::_hpd_pares:43`: monta tuplas de minutos (`app/escala/services/cadastro_tipo.py:57`), mas recebe campos `hi`/`hf` previamente nomeados, não infere quais horários se correspondem (`app/escala/services/cadastro_tipo.py:49`). Falso positivo provável de “append tupla de horários”.
- `app/ponto/services/flip_auto.py::pares_mesmo_tipo:31`: monta tuplas de duas batidas (`app/ponto/services/flip_auto.py:45`), porém exige tipos iguais (`app/ponto/services/flip_auto.py:43`): detector E/E ou S/S, fora de A.
- `app/ponto/adaptador_motor.py::periodos_de_turnos:13`: monta `(e_ts, s_ts)` (`app/ponto/adaptador_motor.py:43`), lendo `t.entrada` e `t.saida` dos turnos recebidos (`app/ponto/adaptador_motor.py:29-38`): excluir por proveniência.

B inequívocos fora da obra:

| Função | Evidência e motivo |
|---|---|
| `app/ponto/services/credito_parcial.py::dia_do_periodo:32` | A própria docstring declara dia de pertença; retorna `localtime(p.entrada).date()` (`:33-36`). |
| `app/ponto/services/espelho.py::somar_periodos:51` | Deriva dia da entrada e recorta somas pela competência (`:84-86`). |
| `app/ponto/services/espelho.py::montar_dias:365` | Deriva dia da entrada para contar turnos na competência (`:426-434`). |
| `app/ponto/services/esmeril_espelho.py::assinaturas_do_colab:79` | Atribui datas a períodos com grandes descontos/HE por `localtime(p.entrada).date()` (`:170-177`). |
| `app/escala/detector_proposta.py::medir_padrao:49` | `lt = localtime(b.timestamp)`; agrupa em `por_dia[lt.date()]` (`:56-58`). |
| `app/escala/regua_defesa.py::diagnostico_regua:118` | `localdate(b.timestamp)` e recuo de um dia por corte horário; agrupa o dia-turno próprio (`:154-160`). |
| `app/escala/regua_defesa.py::entradas_fora_do_inicio:503` | Agrupa batidas por `localdate(b.timestamp)` (`:519-521`). |
| `app/ponto/services/regularizacao.py::regularizar_turno_orfao:19` | Atribui literalmente `data_turno = localtime(batida_e.timestamp).date()` e calcula dias ausentes (`:73-75`). |
| `app/chamados/services/disputa_emissao.py::_data_furo_da_grade:42` | Extrai timestamp de evento, deriva `ref` por data local e o oferece à grade como dia do turno-furo (`:44-68`). |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:876` | Deriva data da batida-origem, classifica falta e usa como fallback de `data_evento` (`:1037-1041`). |

Outros sítios B relevantes à revisão:

- `app/escala/utils.py::montar_grade_prevista:668` atribui órfãs à data civil de suas batidas (`app/escala/utils.py:790-799`).
- `app/escala/utils.py::montar_grade_prevista_periodo_por_turno:1298` ancora sweep intermitente e seleciona batidas por `.date()` (`app/escala/utils.py:1501`, `:1521`, `:1531`, `:1544-1555`); atribui sobras à data civil (`:1835-1855`).
- `app/folha/porta_export.py::dias_fora_do_dia_pago:26` usa data local da entrada (`app/folha/porta_export.py:104`); `medir:313` usa `_p.entrada.date()` (`app/folha/porta_export.py:538`). A função é medição: eventual exclusão precisa ser explícita.
- `app/chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite:627` recebe turnos do juiz (`:643-646`), mas compara datas civis das duas pontas e aceita `dref` em qualquer delas (`:651-654`). Excluir por presença de chamada ao juiz pode ocultar esse comportamento.

Composição comprovada do juiz:

`_turnos_do_colab_puro` chama `papel_por_minuto_da_ata` (`app/ponto/turnos.py:1937`), que importa e chama `ponto.juiz_batida.periodos_do_dia` (`app/ponto/turnos.py:1580`, `:1631`). Este usa `_instante_da_luz` (`app/ponto/juiz_batida.py:241-242`), cuja decisão de meia-noite ocorre por `replace` e `± timedelta(days=1)`, sem `.date()` (`app/ponto/juiz_batida.py:97-102`). É uma exceção de composição sustentada pelo código.

Para B lexical, usar sementes AST `.timestamp`, `.entrada`, `.saida`, além de parâmetros com nomes explícitos `timestamp`, `ts_entrada`, `ts_saida`, `entrada`, `saida`, e propagar por atribuição intrafunção; isso captura o alias `lt` (`app/escala/detector_proposta.py:57-58`). Não excluir função porque contém apenas um `Name data_turno`: ele pode ser justamente o alvo da derivação (`app/ponto/services/regularizacao.py:73`).

Falsos positivos prováveis:

- Competência: `app/ponto/janelas.py::competencia_fechada:75`, data local seguida de `janela_atual` (`:84-85`).
- Data da última batida para diagnóstico/classificação, sem declarar turno: `app/ponto/services/dias_em_aberto.py::passivo_da_competencia:77` (`:123-132`).
- Datas civis usadas para detectar cruzamento de meia-noite, já recebendo pares do juiz: `app/chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite:627` (`:643-654`).
- Busca de candidatos do dia local e véspera, sem escolher um dia único: `app/ponto/registro_batida.py::_casa_marco_proprio:35` (`:42-49`).

## 07/11 08:18 · sessao b219e9ce20f7 · - · - · origem=Ronald (tmux)

Auditoria do escopo, somente leitura:

| Sítio | Classe candidata | Evidência |
|---|---|---|
| `app/ponto/calculador/nucleo.py::minutos_do_oraculo:42` | A e B | O ramo sem `pares` agrupa instantes em turnos e forma pares por índices consecutivos, `range(0, len(_pares), 2)`, em `:157-228` e `:245-247`; atribui o dia por `_e.date()` em `:141` e `tu[0].date()` em `:237`. Receber `pares` opcionalmente não elimina o ramo que pareia sozinho. |
| `app/ponto/calculador/nucleo.py::minutos_do_oraculo._dentro_do_envelope:159` | B | Escolhe envelopes pelas datas civis de `a` e `b`, em `:180`. |
| `app/ponto/motor_calculo_v2.py::_periodos_pelo_marco:534` | A discutível; B claro | Recebe `trio` já pareado em `:558`, substitui períodos usando `periodos_do_dia` em `:616`, `:636-640`, `:685-688`; usa fallback civil do mapa em `:686-687`. Critério A que exclua todo recebimento de pares deixará passar essa substituição; critério A que conte qualquer construção de tupla entrada/saída contará também adaptadores legítimos. |
| `app/ponto/motor_calculo_v2.py::MotorBase._janela_do_dia:1490` | B | `dia = entrada.date()` determina o dia de consulta de empresa e marcos em `:1528-1536`. |
| `app/ponto/motor_calculo_v2.py::MotorBase._julgar_pontualidade:2012` | B | Consulta marcos por `e.date()` em `:2053`; constrói a referência por entrada e comparação de horário em `:2194-2201`; fallback por `localizar(entrada).date()` em `:2210`. |
| `app/ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2265` | B | Lê o mapa com fallback civil em `:2403` e altera `data_jornada` por herança de período anterior quando gap está entre 0 e 6 horas, em `:2405-2408`. |
| `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2750` | B | Decide feriado e dobra pelo calendário de `e`, em `:2796` e `:2809`. |
| `app/ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:2899` | B | Mantém fallbacks civis `_ent.date()` em `:2942` e `_eft.date()` em `:2955`, embora consulte o juiz em `:2921-2923`. |
| `app/ponto/motor_calculo_v2.py::MotorComercial.calcular_periodo:3215` | B | Decide feriado e dobra por `e.date()`, em `:3264` e `:3268`. |
| `app/ponto/motor_calculo_v2.py::MotorComercial.calcular_mes:3308` | B | Mapa de dia tem fallback `localizar(p.entrada).date()` em `:3326`; há outras classificações civis em `:3447`, `:3465`, `:3490-3495`. |
| `app/ponto/services/dia_pago.py::_dia_de:109` | B | Após tentar o mapa em `:128-131`, retorna `timezone.localtime(p.entrada).date()` em `:132`. |
| `app/ponto/services/fechamento.py::recalcular_fechamento_mes:448` | B | Agrupa períodos órfãos pelo calendário da entrada em `:838-842`; classifica folga/feriado pelo calendário da entrada em `:934-947`. |
| `app/ponto/calculador/alimentacao.py::segmentos_do_motor:35` | Excluir de A e B semântico | Recebe períodos em `:76`, lê dia do mapa em `:73-82`, recebe pontas em `:83`, divide par já existente pelos intervalos em `:128-158`; não casa batidas cruas nem deriva dia civil. Detector de `append(tuple)` produzirá falso positivo. |

Para os nomes do aval:

- **MOTOR-PASSA-O-TURNO-A-VOLTA / resíduo 573:** a chamada verificável é `MotorBase._julgar_pontualidade`, passando `e.date()` e as duas pontas do período a `cumpriu_a_volta_do_intervalo`, em `app/ponto/motor_calculo_v2.py:2117-2119`. B por `.date()` captura a função; A por pareamento cru não captura o problema de perguntar por período em vez de turno. O número de produção 573 não foi verificado.
- **JUIZ-DO-PAR-DUAS-ENTRADAS:** existem duas receitas: `_turnos_do_juiz` chama `turnos_de_batidas` em `app/ponto/motor_calculo_v2.py:867-882`, enquanto `turnos_via_autoridade` chama `parear_turnos` com parâmetros `AUT_*`, papel e marcos próprios em `:453-506`. Critério que apenas procura chamada de juiz não captura divergência de alimentação entre receitas. Divergência concreta em produção não foi verificada.
- **col882 turno aberto:** os três calculadores de período retornam cedo quando `saida is None`, em `app/ponto/motor_calculo_v2.py:2752-2755`, `:2879-2882`, `:3217-3220`. Isso não comprova como as cinco batidas do aval chegam pareadas; execução desse caso não foi verificada.
- **Partido `:3012 × :2446`:** essas âncoras estão deslocadas: nesta cópia `:3012` é comentário sobre DNA e `:2446` inicializa `folga_pelo_dia`. Os mecanismos relevantes verificáveis estão em `app/ponto/motor_calculo_v2.py:2403-2408` e `:2942-2955`.
- **`_dia_da_jornada_do_par`:** não localizado em arquivos Python. O nome aparece apenas em documentação, por exemplo `app/docs/BACKLOG.md:1821` e `app/docs/RELATO.md:35`; implementação e enquadramento AST [não verificado].
- **Split `alimentacao.py:73-108`:** recebe dia e pontas do motor; a divisão efetiva está hoje em `app/ponto/calculador/alimentacao.py:128-158`. Excluí-lo de A semântico é coerente com receber um par já existente; não é falso negativo de pareamento cru.

Prováveis falsos positivos de B amplo:

- Iteração de faixas noturnas por dias civis: `app/ponto/motor_calculo_v2.py:97-98`.
- Datas usadas para ampliar a consulta de marcos/DNA, sem atribuir o dia de um período: `app/ponto/motor_calculo_v2.py:470-474`.
- Datas dos limites de competência: `app/ponto/services/fechamento.py:655-664`.
- Conversor de data usado para competência do fato: `app/ponto/services/fechamento.py::_so_a_data:1558`, operação em `:1569`, consumidor em `:1573`.
- Contagem declarada de batidas por dia de calendário: `app/ponto/calculador/nucleo.py:35`, implementação em `:117` e `:235`; a mesma função também contém derivações de pertença reais em `:141` e `:237`.

## 07/11 06:01 · sessao 761aee8c9215 · ESMERIL-ORACULO-NOTURNA · leitura · origem=Code

**O achado principal é: nesta cópia, calculador e motor usam o mesmo juiz `he_noturna`, mas entregam pontas diferentes: efetivas no calculador, cruas no motor.** Portanto, a diferença demonstrável não é “conta diária versus conta por período”. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/regras.py:827`, `app/ponto/motor_calculo_v2.py:1015`)

Os valores da sonda são o dado fornecido; sua reprodução com os insumos e a versão executada em produção permanece **[nao verificado]**.

**1. Quem decide o campo gravado no oráculo**

O fechamento monta `_rub_o = rubricas_por_dia(...)` e chama a mesma `lavrar`, passando `rubricas=_rub_o` e `versao='oraculo'`. Essa chamada **não passa `do_motor`**, cujo padrão é `()`. (`app/ponto/services/fechamento.py:1271`, `app/ponto/services/fechamento.py:1288`, `app/ponto/services/fechamento.py:1294`, `app/ponto/services/dia_pago.py:140`)

A sequência executada é:

- Primeiro, `lavrar` acumula o valor do motor: `p.minutos_extra_100_noturna / 60`. (`app/ponto/services/dia_pago.py:184`; também no ramo `ft_feriado`, `app/ponto/services/dia_pago.py:252`)
- `_DONOS_DECLARADOS` reúne campos presentes em `valores`, em `nao_decide`, nos mapas entregues e em `do_motor`. Isso verifica **cobertura de responsabilidade**; não escolhe precedência entre valores. (`app/ponto/services/dia_pago.py:60`, `app/ponto/services/dia_pago.py:64`, `app/ponto/services/dia_pago.py:75`)
- Com `rubricas`, campo sem dono provoca `ValueError`, antes da gravação. (`app/ponto/services/dia_pago.py:312`, `app/ponto/services/dia_pago.py:316`)
- O laço percorre `Rubricas.valores`: se o campo pertence a `CAMPOS_DIA` e não está em `do_motor`, executa **`_L[_campo] = float(_valor or 0)` — substitui, não soma**. (`app/ponto/services/dia_pago.py:324`, `app/ponto/services/dia_pago.py:329`)
- `nao_decide` apenas registra o motivo no detalhe; não substitui o valor previamente acumulado. (`app/ponto/services/dia_pago.py:331`)
- As linhas resultantes são gravadas na versão recebida. (`app/ponto/services/dia_pago.py:335`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`)

**Logo, quando `valores` contém `horas_extras_100_noturna`, quem decide o número final do oráculo é `regras.do_dia`, por essa substituição.** Se vier somente em `nao_decide`, permanece o acumulado anterior; `do_motor` protegeria explicitamente esse acumulado, mas não é passado nessa chamada. (`app/ponto/calculador/regras.py:838`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:331`, `app/ponto/services/fechamento.py:1277`)

**2. Onde e como o calculador calcula HE100 noturna diária**

`segmentos_do_motor` agrupa os períodos por `resultado.dia_da_jornada[id(p)]`; sem esse mapa, exclui o período e conta `sem_dia_da_jornada`. Não reparte automaticamente o turno entre os dias civis atravessados. (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:80`)

Para cada período, monta `he_pontas_por_dia` com:

- `efetivo_entrada` e `efetivo_saida`;
- `minutos_extra_50` e `minutos_extra_100` **já calculados pelo motor**;
- os intervalos do motor, reunidos por dia. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:107`)

`rubricas_por_dia` passa esses insumos a `regras.do_dia`; esta chama `he_noturna` **uma vez por período**, soma os retornos de 100% e escreve `_s100 / 60` em `horas_extras_100_noturna`. A quantidade diária de HE100 que o calculador calcula separadamente não alimenta esse split. (`app/ponto/calculador/alimentacao.py:526`, `app/ponto/calculador/regras.py:784`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:832`, `app/ponto/calculador/regras.py:839`)

As regras do juiz compartilhado são:

| Aspecto | Regra executada |
|---|---|
| Ancoragem do trecho extra | Parte da saída de **cada período** e recua `HE50 + HE100` minutos trabalhados, saltando intervalos e limitado pela entrada. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:406`, `app/ponto/motor_calculo_v2.py:440`) |
| Janela | Horário local; interseções com **22–24 e 00–05** nas datas civis atravessadas pelo recorte. (`app/ponto/motor_calculo_v2.py:89`, `app/ponto/motor_calculo_v2.py:97`, `app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`) |
| Após 05h | Com prorrogação ligada, estende 00–05 até o fim recebido se **o próprio recorte extra** tocar 00–05 e continuar após 05h. O início noturno do turno completo, sozinho, não basta. (`app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:441`) |
| Intervalos | Desconta o noturno consumido pelos intervalos dentro do recorte. (`app/ponto/motor_calculo_v2.py:442`) |
| Parcela de 100% | Multiplica o noturno encontrado por `extra_100_min / total_extra`. (`app/ponto/motor_calculo_v2.py:447`, `app/ponto/motor_calculo_v2.py:448`) |
| Hora reduzida | **O split não aplica `60/52,5`**: retorna minutos de relógio. A redução pertence à rubrica distinta `horas_noturnas`. (`app/ponto/motor_calculo_v2.py:409`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:451`, `app/ponto/calculador/regras.py:88`, `app/ponto/calculador/regras.py:849`) |

O calculador obtém o flag de prorrogação uma vez, por `get_motor_cct(c, ini, tipo_escala=te)`, e o passa aos dias. A resolução posterior do vínculo diário não recalcula esse flag. (`app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/alimentacao.py:404`, `app/ponto/calculador/alimentacao.py:492`, `app/ponto/calculador/alimentacao.py:528`)

**3. Onde e como o motor calcula no feriado em dobra**

O juiz da dobra é `_feriado_dobra_no_dia`: `feriado_em_dobra and eh_feriado(dia)`. Seu contrato atribui o plantão ao dia de início, incluindo a cauda após meia-noite. (`app/ponto/motor_calculo_v2.py:928`, `app/ponto/motor_calculo_v2.py:934`)

No ramo geométrico `Motor12x36`, o teste usa `e.date()` e coloca a duração líquida inteira em HE100/feriado; depois chama `_aplicar_he_noturna`. Isso depende do flag de dobra, não apenas do nome da classe. (`app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:2803`, `app/ponto/motor_calculo_v2.py:2809`, `app/ponto/motor_calculo_v2.py:2855`)

Nos recálculos mensais comercial e partido, a dobra é distribuída **em cada período**, com os minutos trabalhados daquele período, seguida de `_aplicar_he_noturna`. No comercial, o agrupamento usa `data_turno` do juiz, com fallback à data local da entrada. (`app/ponto/motor_calculo_v2.py:3097`, `app/ponto/motor_calculo_v2.py:3104`, `app/ponto/motor_calculo_v2.py:3325`, `app/ponto/motor_calculo_v2.py:3354`, `app/ponto/motor_calculo_v2.py:3361`)

`_aplicar_he_noturna` entrega ao juiz **`periodo.entrada/saida`**, as quotas HE50/100, o flag do motor e `intervalos_do_motor`; grava o retorno de 100% em `minutos_extra_100_noturna`. Portanto, janela, recuo, prorrogação e ausência de fator reduzido são os mesmos descritos em (2), mas aplicados às pontas cruas. (`app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:1020`)

As pontas efetivas ficam em campos separados: a janela calcula `e/s`, e `_carimbar_segmento` grava `efetivo_entrada/efetivo_saida` sem substituir as pontas originais. (`app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:1840`)

**4. Diferenças candidatas para os dois turnos**

**Turno 22:50 → 07:04:** a diferença concreta é ancorar a mesma quota extra na saída efetiva ou na saída crua; isso desloca a interseção com 05h. Também pode haver diferença de flag: o motor é construído por fatia, enquanto o calculador consulta a CCT uma vez para sua chamada. A ocorrência dessas condições nos colaboradores é **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/motor_calculo_v2.py:1015`, `app/ponto/services/fechamento.py:279`, `app/ponto/calculador/alimentacao.py:390`)

Exemplo **condicional**: se a saída efetiva fosse 05:00, a crua 07:04 e a prorrogação estivesse desligada, o motor gastaria aproximadamente 124 minutos da quota extra depois de 05h, fora da janela estrita; o oráculo ancoraria antes. Essa ordem de grandeza é compatível com a diferença fornecida de aproximadamente 2,06 h, mas **marco 05:00, flag, pausas e reprodução exata são [nao verificado]**. A mecânica está no recuo e na interseção. (`app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:405`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:440`)

**Turno da tarde → cerca de 23h:** uma saída efetiva às 23:00 e crua às 23:06 permitiria 60 versus 66 minutos após 22h, compatíveis com 1,00 versus 1,10 h. Esses horários exatos são apenas exemplo: **[nao verificado]**. Prorrogação após 05h não explica isoladamente esse exemplo, pois a interseção relevante seria 22–24. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/motor_calculo_v2.py:1015`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:115`)

**Hora reduzida e “diário versus período” não são diferenças entre os dois splits nesta cópia:** ambos chamam a mesma função por período, sem multiplicador reduzido. O comentário de `dia_pago.py` que ainda descreve ancoragens distintas não corresponde às chamadas executáveis atuais. (`app/ponto/calculador/regras.py:827`, `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:451`, `app/ponto/services/dia_pago.py:307`)

Não há elementos para escolher qual regra deveria prevalecer nem para fechar a causa dos dois números observados: **[nao verificado]**.

**5. Leitores e registro de juiz**

Nas rotas identificadas abaixo, os leitores usam **`motor`**, pelos defaults das APIs de leitura. A execução efetiva dessas rotas em produção é **[nao verificado]**. (`app/ponto/services/dia_pago.py:550`, `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:833`)

| Canal | Chamada encontrada |
|---|---|
| Espelho | `app/ponto/services/espelho.py:410` |
| Calendário | `app/colaboradores/services/calendario.py:290` |
| App/API | `app/api/views.py:1457` |
| Cartão | `app/relatorios/cartao_pela_celula.py:143` |
| Extrato parcial | `app/relatorios/views.py:487` |
| Gestão de HE | `app/ponto/services/gestao_he.py:256` |
| Autorização de HE | `app/ponto/services/autorizacao_he_periodo.py:174` |

O selo/porta de exportação usa soma com default `motor` e consultas explicitamente filtradas por `motor`. O TXT lê `FechamentoMensal`. (`app/folha/porta_export.py:388`, `app/folha/porta_export.py:284`, `app/folha/porta_export.py:589`, `app/folha/porta_export.py:651`, `app/folha/export.py:711`, `app/folha/export.py:777`)

Há, porém, leitores operacionais que **podem alcançar `oraculo`**; portanto, “ninguém lê” seria amplo demais:

| Comando/script | Leitura |
|---|---|
| `lavrar_dias_pagos` | Aceita `--versao`; conta linhas da versão pedida. (`app/ponto/management/commands/lavrar_dias_pagos.py:52`, `app/ponto/management/commands/lavrar_dias_pagos.py:140`) |
| `apurar_furo_da_exportada` | Consulta todas as versões e lê versão/colaborador/furo; sem seleção explícita, processa versões com NULL. (`app/ponto/management/commands/apurar_furo_da_exportada.py:59`, `app/ponto/management/commands/apurar_furo_da_exportada.py:61`, `app/ponto/management/commands/apurar_furo_da_exportada.py:99`) |
| Selo de idempotência | Lê todas as versões, incluindo a versão na chave. (`bin/r5_idempotencia_frota.py:65`, `bin/r5_idempotencia_frota.py:67`) |
| Snapshot de reversão | Fotografa todas as versões e campos selecionados. (`bin/snapshot_badge_linha_09_10.py:50`, `bin/snapshot_badge_linha_09_10.py:60`) |
| Relavraturas específicas | Capturam todas as versões para reversão. (`bin/relavrar_o118_col221.py:39`, `bin/relavrar_o119_col221.py:38`, `bin/relavrar_o120_col221_09.py:68`) |

A execução desses comandos/scripts em produção é **[nao verificado]**. `diff_calculador` compara cálculos vivos e, com `--contra-gravado`, `FechamentoMensal`; não é leitor do `DiaPago` oráculo gravado. (`app/ponto/management/commands/diff_calculador.py:45`, `app/ponto/management/commands/diff_calculador.py:62`, `app/ponto/management/commands/diff_calculador.py:245`)

**O `JUIZES.md` não registra um juiz específico para HE noturna por dia.** Registra cálculo genericamente em `motor_calculo_v2.py` e dobra de feriado em `CICLOS_FERIADO_SIMPLES`; declara-se lookup e aponta ao registro vivo. Isso é distinto da existência da função compartilhada `he_noturna` no código. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:29`, `app/docs/JUIZES.md:73`, `app/ponto/motor_calculo_v2.py:409`)

## 07/11 05:59 · sessao 939e50b04f0b · - · - · origem=Ronald (tmux)

Motor, com referências para consolidação:

- `_aplicar_he_noturna` passa **`periodo.entrada/saida` crus**, os totais HE50/100, o flag de prorrogação e `intervalos_do_motor` ao juiz compartilhado; atribui `minutos_extra_100_noturna` se o retorno for não zero. `app/ponto/motor_calculo_v2.py:1012-1022`.
- As pontas cruas nascem no `PeriodoCalculo`; a janela produz `e/s` e o carimbo guarda esses valores em **outros campos**, `efetivo_entrada/efetivo_saida`, sem substituir `entrada/saida`. `app/ponto/motor_calculo_v2.py:2751`, `:2759-2765`, `:1760-1763`, `:1840-1842`. Comercial segue o mesmo desenho em `:3216`, `:3224-3238`.

Regra efetiva do split:

1. Soma HE50+HE100; reconstrói a ponta extra **a partir da saída**, recuando por minutos trabalhados e pulando intervalos. `app/ponto/motor_calculo_v2.py:432-440`, `:392-406`.
2. Mede noturno nessa ponta e subtrai seus intervalos; reparte pela proporção HE50/HE100. `app/ponto/motor_calculo_v2.py:441-451`.
3. Janela local: 22–24 + 00–05, percorrendo os dias civis atravessados; não corta o cálculo ao dia civil do feriado. `app/ponto/motor_calculo_v2.py:89-117`.
4. Prorrogação: se o flag estiver ligado e **o recorte passado ao juiz** tocar 00–05 e continuar após05, estende até o fim. Como o recorte é `inicio_extra → fim`, a condição é da ponta extra, não automaticamente da jornada inteira. `app/ponto/motor_calculo_v2.py:109-111`, `:440-441`.
5. HE noturna retorna **minutos de relógio**, sem multiplicação por60/52,5. A redução aparece no adicional noturno (`calcular_noturnas`) e no fator1 quando afastada, em outro cálculo. `app/ponto/motor_calculo_v2.py:432-451`, `:983-992`. A soma mensal HE100noturna divide os minutos por60. `:342-344`.

Feriado em dobra:

- O juiz devolve `feriado_em_dobra and eh_feriado(dia)`; o contrato documenta pertencimento ao dia de início do plantão. `app/ponto/motor_calculo_v2.py:928-934`.
- No 12x36 geométrico, testa `e.date()`, atribui a duração líquida inteira a HE100/feriado e depois chama `_aplicar_he_noturna`. `app/ponto/motor_calculo_v2.py:2761-2785`, `:2809-2813`, `:2855`.
- No comercial e partido, o recálculo mensal usa `data_turno` do juiz, com fallback à data local da entrada; distribui a dobra em **cada período**, pelos minutos trabalhados daquele período, e faz split individual. `app/ponto/motor_calculo_v2.py:884-895`, `:2941-2945`, `:3092-3114`, `:3325-3330`, `:3350-3376`.
- A regra padrão da fábrica exclui `tipo_ciclo='12x36'` da dobra; CCT pode reativá-la por cláusula cadastrada. `app/ponto/motor_calculo_v2.py:3623`, `:3693-3694`; `app/core/regua_cct.py:477-478`.
- Prorrogação e afastamento da redução chegam da régua/cadastro; valores aplicados aos colaboradores concretos são `[nao verificado]`. Fluxo: `app/core/regua_cct.py:449-472`; default legal por ciclo em `:153-182`.

Diferença verificável frente ao calculador: este chama o mesmo juiz com `he_pontas`; o agente do calculador identificou que essas pontas vêm dos campos efetivos, enquanto o motor usa os crus. A chamada compartilhada e soma por período estão em `app/ponto/calculador/regras.py:824-839`; lado motor cru em `app/ponto/motor_calculo_v2.py:1015`.

Exemplos **condicionais**, sem afirmar os insumos reais:

- Se a saída efetiva fosse05:00, a crua07:04 e a prorrogação estivesse desligada, o motor gastaria124min da quota extra na cauda após05; esses minutos deslocariam o início do recorte e não entrariam na janela estrita. O oráculo ancorado05:00 teria outra interseção. Isso é compatível com uma diferença próxima de2,06h, pelo recuo e janela citados; saída efetiva05:00 e flag concretos são `[nao verificado]`. `app/ponto/motor_calculo_v2.py:392-406`, `:109-116`, `:440-448`.
- Se a saída efetiva fosse23:00 e a crua23:06, haveria60 contra66min após22, compatível com1,00 contra1,10h; esses dois horários exatos e os demais insumos são `[nao verificado]`. `app/ponto/motor_calculo_v2.py:103-116`, `:435-448`.

Não usar os comentários sobre correções anteriores como prova de qual árvore gerou a sonda; a versão executada e os insumos completos da medição são `[nao verificado]`.

## 07/11 05:59 · sessao e6f623b28100 · - · - · origem=Ronald (tmux)

Leitores encontrados na cópia; a execução efetiva em produção é `[nao verificado]`.

| Canal | O que lê |
|---|---|
| Espelho | `linhas_do_periodo` sem versão explícita: `app/ponto/services/espelho.py:410`. |
| Calendário | Mesma API sem versão explícita: `app/colaboradores/services/calendario.py:290`. |
| App/API | Mesma API sem versão explícita: `app/api/views.py:1457`. |
| Cartão | `soma_da_competencia` sem versão explícita: `app/relatorios/cartao_pela_celula.py:143`. |
| Extrato parcial | `soma_do_periodo` sem versão explícita: `app/relatorios/views.py:487`. |
| Gestão de HE | `soma_do_periodo` sem versão explícita: `app/ponto/services/gestao_he.py:256`. |
| Autorização de HE | `soma_do_periodo` sem versão explícita: `app/ponto/services/autorizacao_he_periodo.py:174`. |

Esses leitores selecionam **motor**, porque os defaults são `versao='motor'`: `app/ponto/services/dia_pago.py:550`, `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:833`; os filtros efetivos ficam em `app/ponto/services/dia_pago.py:559`, `app/ponto/services/dia_pago.py:784`, `app/ponto/services/dia_pago.py:856`.

O selo/porta de exportação também usa motor: soma default em `app/folha/porta_export.py:388`; filtros explícitos em `app/folha/porta_export.py:284`, `app/folha/porta_export.py:589`, `app/folha/porta_export.py:651`. `dias_fora_do_dia_pago` aceita versão, mas seu default é motor, e a chamada da porta recebe o mapa motor já carregado: `app/folha/porta_export.py:26`, `app/folha/porta_export.py:597`.

O TXT lê **FechamentoMensal**, em `app/folha/export.py:711` e `app/folha/export.py:777`. A função de eventos recebe um fechamento: `app/folha/export.py:402`.

Há, entretanto, comandos/scripts capazes de ler **oraculo**, portanto “ninguém lê” seria absoluto demais:

- `lavrar_dias_pagos`: aceita `--versao`, default motor, e conta linhas da versão pedida: `app/ponto/management/commands/lavrar_dias_pagos.py:52`, `app/ponto/management/commands/lavrar_dias_pagos.py:140`. É ferramenta por ato, conforme registro em `app/config/crons.py:1015`.
- `apurar_furo_da_exportada`: `_universo` consulta **todas** as versões e lê `versao`, colaborador e `furo_apurado`: `app/ponto/management/commands/apurar_furo_da_exportada.py:59`, `app/ponto/management/commands/apurar_furo_da_exportada.py:61`. Sem `--versao`, seleciona todas as versões com NULL: `app/ponto/management/commands/apurar_furo_da_exportada.py:78`, `app/ponto/management/commands/apurar_furo_da_exportada.py:99`; passa cada versão à porta em `app/ponto/management/commands/apurar_furo_da_exportada.py:131`.
- Selo operacional de idempotência lê todas as versões: `bin/r5_idempotencia_frota.py:65`; usa versão na chave e lê trabalhadas/realizados: `bin/r5_idempotencia_frota.py:67`.
- Snapshot de reversão fotografa todas as versões e campos: `bin/snapshot_badge_linha_09_10.py:50`, `bin/snapshot_badge_linha_09_10.py:60`.
- Snapshots incorporados às relavraturas capturam todas as versões: `bin/relavrar_o118_col221.py:39`, `bin/relavrar_o119_col221.py:38`, `bin/relavrar_o120_col221_09.py:68`.

`diff_calculador` compara cálculos vivos, e `--contra-gravado` usa **FechamentoMensal**; isso não constitui leitura da versão oraculo de DiaPago: `app/ponto/management/commands/diff_calculador.py:45`, `app/ponto/management/commands/diff_calculador.py:62`, `app/ponto/management/commands/diff_calculador.py:245`.

O **JUIZES.md não registra juiz específico para “HE noturna por dia”**: contém o endereço genérico de cálculo em `app/docs/JUIZES.md:29` e o de dobra de feriado em `app/docs/JUIZES.md:73`. A tabela declara ser lookup e aponta ao registro vivo: `app/docs/JUIZES.md:7`. No registro vivo, feriado/dobra aponta a `CICLOS_FERIADO_SIMPLES` (`app/core/juizes.py:280`), e a pergunta de totais de competência aponta a `folha/export.py::eventos_do_fechamento` (`app/core/juizes.py:698`, `app/core/juizes.py:711`); essas entradas não nomeiam um juiz de HE noturna diária.

## 07/11 05:59 · sessao a5699bf2ca46 · - · - · origem=Ronald (tmux)

Achado central: o calculador usa **a mesma função `he_noturna` do motor**, porém com pontas efetivas, enquanto `_aplicar_he_noturna` entrega pontas cruas. Não há base para atribuir os valores observados a regras independentes de janela ou hora reduzida. (`app/ponto/calculador/regras.py:824`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/motor_calculo_v2.py:1014`)

1. **Fluxo do calculador:** `rubricas_por_dia` recebe `insumos`; estes são montados por `insumos_do_calculador`, que chama `segmentos_do_motor`. O fechamento usa essa porta para construir `_rub_o`. (`app/ponto/calculador/alimentacao.py:305`; `app/ponto/calculador/alimentacao.py:314`; `app/ponto/services/fechamento.py:1269`; `app/ponto/services/fechamento.py:1271`)

2. **Dia de atribuição:** `segmentos_do_motor` obtém o dia por `resultado.dia_da_jornada[id(p)]`; sem esse mapa o período é excluído e contado como `sem_dia_da_jornada`. Portanto a agregação é por dia da jornada, não repartição automática no dia civil. (`app/ponto/calculador/alimentacao.py:73`; `app/ponto/calculador/alimentacao.py:78`; `app/ponto/calculador/alimentacao.py:80`)

3. **Pontas e quantidade de HE:** usa `p.efetivo_entrada/efetivo_saida` e copia `p.minutos_extra_50/minutos_extra_100` do motor. Não usa a quantidade diária de HE recalculada por `do_dia` para decidir o split. Copia também os intervalos do motor para o mapa diário. (`app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:101`; `app/ponto/calculador/alimentacao.py:102`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/calculador/alimentacao.py:107`)

4. **Conta diária:** `rubricas_por_dia` passa as pontas, intervalos e flag de prorrogação a `regras.do_dia`. Esta chama `motor_calculo_v2.he_noturna` uma vez por período, soma `_b`, e escreve `val['horas_extras_100_noturna'] = _s100 / 60`. (`app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/alimentacao.py:526`; `app/ponto/calculador/alimentacao.py:528`; `app/ponto/calculador/regras.py:824`; `app/ponto/calculador/regras.py:827`; `app/ponto/calculador/regras.py:832`; `app/ponto/calculador/regras.py:839`)

5. **Ancoragem temporal:** `he_noturna` toma `saida` como fim, recua `extra50 + extra100` minutos trabalhados, saltando intervalos e limitado pela entrada, e mede o trecho resultante. Portanto o trecho extra fica no fim de cada período; a janela de prorrogação é julgada a partir desse **trecho extra**, não necessariamente do turno completo. (`app/ponto/motor_calculo_v2.py:392`; `app/ponto/motor_calculo_v2.py:395`; `app/ponto/motor_calculo_v2.py:406`; `app/ponto/motor_calculo_v2.py:432`; `app/ponto/motor_calculo_v2.py:435`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:441`)

6. **Janela/prorrogação:** converte para horário local; percorre as datas civis do recorte e intersecta 00–05 e 22–24. Com prorrogação habilitada, se `max(inicio, 00h) < 05h` e o fim é posterior às 05h, estende a janela 00–05 até o fim recebido. Assim, um trecho extra que só começa depois das 05h não ganha prorrogação por o turno original ter começado às 22h. (`app/ponto/motor_calculo_v2.py:89`; `app/ponto/motor_calculo_v2.py:97`; `app/ponto/motor_calculo_v2.py:100`; `app/ponto/motor_calculo_v2.py:103`; `app/ponto/motor_calculo_v2.py:109`; `app/ponto/motor_calculo_v2.py:110`; `app/ponto/motor_calculo_v2.py:112`)

7. **Intervalos e repartição:** desconta o noturno consumido por intervalos e reparte o resultado na proporção `extra100 / total_extra`. Se a dobra deixou toda a HE em 100%, todo o noturno medido nesse trecho é HE100 noturna. (`app/ponto/motor_calculo_v2.py:442`; `app/ponto/motor_calculo_v2.py:447`; `app/ponto/motor_calculo_v2.py:448`)

8. **Hora reduzida:** `he_noturna` não recebe nem aplica `hora_reduzida_afastada`, tampouco multiplica por `60/52,5`; o resultado do split é em minutos de relógio. O fator aparece na rubrica distinta `horas_noturnas`: `noturnas_do_dia` multiplica o relógio por 1 ou `FATOR_HORA_NOTURNA`. (`app/ponto/motor_calculo_v2.py:409`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:448`; `app/ponto/motor_calculo_v2.py:451`; `app/ponto/calculador/regras.py:88`; `app/ponto/calculador/regras.py:89`; `app/ponto/calculador/regras.py:849`)

9. **Origem do flag no calculador:** chama `get_motor_cct(c, ini, tipo_escala=te)` uma vez e obtém `_cct_pro` por `cct_do_motor`; é esse flag que passa a todos os dias. A resolução do vínculo por dia muda tipo, intermitência, intervalo cadastrado e folgas, sem recalcular `_cct_pro`. O fechamento entrega como referência o vínculo do último dia coberto na janela. Portanto uma divergência de flag entre o motor que calculou uma fatia e o calculador é hipótese verificável no código, mas ocorrência nestes colaboradores é `[nao verificado]`. (`app/ponto/calculador/alimentacao.py:390`; `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:404`; `app/ponto/calculador/alimentacao.py:492`; `app/ponto/calculador/alimentacao.py:496`; `app/ponto/calculador/alimentacao.py:499`; `app/ponto/calculador/alimentacao.py:528`; `app/ponto/services/fechamento.py:851`; `app/ponto/services/fechamento.py:852`)

10. **Diferenças candidatas para os dois casos:** motor ancora o recuo em `periodo.saida` e limita por `periodo.entrada`; calculador usa `efetivo_saida/efetivo_entrada`. Uma saída efetiva diferente muda a quantidade de HE que cruza 22h, podendo explicar qualitativamente 1,10 versus 1,00; pode também deslocar o início do trecho em relação às 05h e mudar a prorrogação. Flag diferente pode afetar o turno que atravessa 05h, mas não explica isoladamente um turno que termina por volta de 23h. Pontas efetivas, pausas, flags e subdivisão em períodos destes casos não constam do dado: reconstruir exatamente 5,16/3,10 ou 1,00/1,10 é `[nao verificado]`. (`app/ponto/motor_calculo_v2.py:1015`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:110`)

11. **Dobra do calculador:** classifica pelo dia da jornada recebido, devolvendo todo o trabalhado quando `_feriado_dobra_no_dia(dia)` é verdadeiro. Porém esse total diário não alimenta o split: este continua usando as HE dos períodos do motor. (`app/ponto/calculador/regras.py:348`; `app/ponto/calculador/regras.py:351`; `app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:784`; `app/ponto/calculador/regras.py:827`; `app/ponto/calculador/regras.py:832`)

Atenção à leitura dos comentários: afirmam que os dois caminhos têm as mesmas pontas e “a troca não muda número”, mas os argumentos executados diferem entre efetivos e crus; citar o código executável nesse ponto. (`app/ponto/calculador/alimentacao.py:52`; `app/ponto/calculador/alimentacao.py:54`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/motor_calculo_v2.py:1015`)

## 07/11 05:18 · sessao 2ab7f98e2983 · PERTENCA-DO-MINUTO-PELO-TURNO · censo · origem=Code

**A cópia usa três critérios diferentes:** dia do turno/jornada no agrupamento mensal; data civil da entrada em decisões por período e no fechamento; instantes do recorte para o noturno. Assim, um período pode receber a **linha diária correta**, mas carregar uma **rubrica escolhida pelo dia civil errado**. (`app/ponto/motor_calculo_v2.py:2403–2408`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/services/fechamento.py:934–949`; `app/ponto/services/dia_pago.py:128–132`)

As referências abaixo são às linhas **da cópia consultada**. Resultados efetivamente pagos em produção, incidência por colaborador e execução dos cenários nesta auditoria: **[não verificado]**.

**O juiz existente e seu alcance.** `core/juizes.py` registra expressamente “a que turno (`data_turno`) pertence a batida?” em `turnos_do_colab`; a pergunta financeira distinta — “que horas do turno que cruza a meia-noite contam como feriado?” — permanece em `SEM_JUIZ`. (`app/core/juizes.py:196`; `app/core/juizes.py:153`; `app/core/juizes.py:329–331`)

| Função/contrato | Entrada, saída e relógio |
|---|---|
| `turnos_do_colab(colab, ini, fim, celulas=None)` | Recebe colaborador e datas; carrega batidas/células/vínculos e devolve turnos com `data_turno` em `[ini,fim]`. O objeto contém `data_turno`, entrada, saída, batidas, aberto e cruzamento de meia-noite. (`app/ponto/turnos.py:41`; `app/ponto/turnos.py:1848–1857`; `app/ponto/turnos.py:1913`) |
| `parear_turnos(...)` | Recebe batidas com timestamp **aware**, tipo E/S, marcos e eventual papel da ata; ordena pelo instante e forma turnos. É o pareador puro usado pelas portas superiores. (`app/ponto/turnos.py:1190–1215`) |
| `turnos_de_batidas(...)` | Porta usada pelo motor com batidas já carregadas: copia objetos, interpreta timestamp naive como horário local e chama `parear_turnos`. (`app/ponto/turnos.py:1515–1533`) |
| `_data_do_turno(entrada, vespera, saida, cont_max_s)` | Parte da data **local** da entrada, ou da saída órfã. Pode devolver a véspera quando os marcos anteriores cruzam meia-noite, a entrada está antes do fim e longe do início, e a saída cabe no envelope anterior. `_mk` publica isso em `Turno.data_turno`. (`app/ponto/turnos.py:283–322`) |
| Relógio da atribuição | Usa timestamps recebidos, `timezone.localtime` e marcos; não recebe `agora`. A pergunta separada sobre turno aberto vivo recebe `agora` e pode usar `timezone.now()`. (`app/ponto/turnos.py:43–46`; `app/ponto/turnos.py:246`; `app/ponto/turnos.py:1998`; `app/ponto/turnos.py:2024`) |

Portanto, existe juiz de **batida → turno/dia**, com associação pelo pareamento. Um contrato geral **instante arbitrário sem batida → turno** é **[não verificado]**; não é o domínio registrado dessa porta. (`app/core/juizes.py:196`; `app/ponto/turnos.py:1193`)

**Censo da pertença no motor.**

| Sítio/função | Qual “dia” usa e origem | Quem chama; resultado e leitor imediato |
|---|---|---|
| `_dia_do_turno` | Lê `Turno.data_turno` e monta `{instante localizado de cada E: data_turno}`. Consulta `_turnos_do_juiz`, que usa `turnos_de_batidas`. (`app/ponto/motor_calculo_v2.py:867–895`) | Chamado pelos cálculos mensais Base, Partido e Comercial; alimenta os agrupadores abaixo. (`app/ponto/motor_calculo_v2.py:2348`; `app/ponto/motor_calculo_v2.py:2922`; `app/ponto/motor_calculo_v2.py:3325`) |
| `MotorBase.calcular_mes`: escolha de `data_jornada` | Primeiro mapa do juiz; sem chave, **data civil local da entrada**. Depois aplica decisão própria: período iniciado de 0 a 6 horas após a saída anterior herda o dia anterior. Só saída real alimenta `_prev_fim`. (`app/ponto/motor_calculo_v2.py:2400–2423`) | O fechamento chega por `apurar_por_fatia → motor.calcular_mes`. `data_jornada` filtra a competência e alimenta FT e mapa por período. (`app/ponto/services/fechamento.py:286–291`; `app/ponto/motor_calculo_v2.py:2429–2432`; `app/ponto/motor_calculo_v2.py:2448–2461`) |
| `MotorBase.calcular_mes`: folga trabalhada | Pergunta `EscalaColaborador.eh_dia_trabalho(data_jornada)`; sem esse ramo, usa `_folga_pelo_dia_previsto(data_jornada, dias_folga)`. Grava somente o booleano `_folga_do_par[instante]` **antes** de chamar `calcular_periodo`; grava o mapa `id(periodo) → data_jornada` depois. (`app/ponto/motor_calculo_v2.py:2445–2461`) | Separa `resultado.periodos_ft` de `resultado.periodos`; a janela lê o memo de folga, e o fechamento lê as listas. (`app/ponto/motor_calculo_v2.py:2468–2494`; `app/ponto/motor_calculo_v2.py:1526`; `app/ponto/services/fechamento.py:933`) |
| `_folga_pelo_dia_previsto` | Recebe o dia escolhido pelo chamador; usa `weekday()` e a previsão injetada. Não consulta o juiz de turno. (`app/ponto/motor_calculo_v2.py:936–954`) | Chamado pelo laço Base para classificação e motivo da FT. O **texto** ainda extrai o dia da semana da entrada civil, podendo discordar do número. (`app/ponto/motor_calculo_v2.py:2453`; `app/ponto/motor_calculo_v2.py:2474–2484`) |
| `_feriado_dobra_no_dia` / `eh_feriado` | Não escolhem a pertença: recebem `dia`. O primeiro aplica `feriado_em_dobra && eh_feriado(dia)`; o segundo lê feriados injetados ou `is_feriado`. O contrato escrito exige o dia de início do plantão. (`app/ponto/motor_calculo_v2.py:928–967`) | Chamados pelos quatro escritores de dobra: 12x36 por período, Comercial por período e os dois agrupadores mensais. Cada chamador escolhe o dia que entrega. (`app/ponto/motor_calculo_v2.py:2809`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:3092`; `app/ponto/motor_calculo_v2.py:3350`) |
| `MotorTurnoPartido.calcular_mes`: agrupamento | Reconsulta o juiz; normais usam `dia_do_turno.get(instante, entrada.date())`; FT recebe o mesmo critério. Esse mapa é mesclado em `resultado.dia_da_jornada`; para chaves regravadas, prevalece sobre a herança do Base. (`app/ponto/motor_calculo_v2.py:2922–2959`) | O agrupamento de trabalhado, dobra e HE deste override usa **somente períodos normais**. FT ganha mapa, mas não passa por essas contas. (`app/ponto/motor_calculo_v2.py:2944–2950`; `app/ponto/motor_calculo_v2.py:3081–3084`) |
| Partido: dobra e HE | Feriado/dobra pelo dia agrupado. A dobra já é escrita em **cada período**, com split em cada um. HE diária usa esse dia para previsto e limite; HE semanal usa esse dia para semana, exclusão de feriado e escolha da última jornada. (`app/ponto/motor_calculo_v2.py:3081–3114`; `app/ponto/motor_calculo_v2.py:3128–3160`; `app/ponto/motor_calculo_v2.py:3169–3191`) | Chamado pelo cálculo mensal; escreve campos dos períodos que as propriedades de `ResultadoMes`, o fechamento e DiaPago somam. (`app/ponto/motor_calculo_v2.py:324–344`; `app/ponto/services/fechamento.py:919–924`; `app/ponto/services/dia_pago.py:180–184`) |
| `MotorComercial.calcular_mes` | Reconsulta o juiz, com fallback civil, para **normais**; zera HE individual e recalcula dobra/HE pelo dia agrupado. FT fica fora desse reset. (`app/ponto/motor_calculo_v2.py:3325–3350`) | Dobra em cada período + split; HE diária pelos marcos de `dia`, lançada no último período. Leitores: mesmas propriedades, fechamento e DiaPago. (`app/ponto/motor_calculo_v2.py:3354–3361`; `app/ponto/motor_calculo_v2.py:3396–3423`; `app/ponto/services/fechamento.py:919–924`; `app/ponto/services/dia_pago.py:180–184`) |
| `_aplicar_teto_pontualidade` | Recebe mapa `id(periodo) → dia`; agrupa por ele e **expõe/mescla** `resultado.dia_da_jornada`. Não reconstrói o dia nesse agrupamento. (`app/ponto/motor_calculo_v2.py:1073–1084`) | Chamado ao final dos três cálculos mensais; o fechamento mescla os mapas das fatias e passa o resultado à lavratura. (`app/ponto/motor_calculo_v2.py:2718`; `app/ponto/motor_calculo_v2.py:3195`; `app/ponto/motor_calculo_v2.py:3508`; `app/ponto/services/fechamento.py:311–316`; `app/ponto/services/fechamento.py:1217`) |

**Decisões por período, recorte e noturno — inclusive os sítios mínimos pedidos.**

| Sítio/função | Critério efetivo na cópia; chamada e resultado |
|---|---|
| `Motor12x36.calcular_periodo` | `e` vem da **entrada efetiva** da janela. Feriado e dobra usam **`e.date()`**, sem ler `data_jornada`. HE comum consulta `_jornada_do_dia(e)` e o limite com `e`. O laço Base chama o método antes de associar o período ao mapa. (`app/ponto/motor_calculo_v2.py:2759–2760`; `app/ponto/motor_calculo_v2.py:2796–2841`; `app/ponto/motor_calculo_v2.py:2458–2460`) |
| `MotorComercial.calcular_periodo` | Também decide feriado/dobra por **`e.date()`**, da entrada efetiva. Escreve HE100/feriado e chama split. O mensal corrige os normais por agrupamento; os FT preservam esse julgamento individual. (`app/ponto/motor_calculo_v2.py:3224–3238`; `app/ponto/motor_calculo_v2.py:3264–3294`; `app/ponto/motor_calculo_v2.py:3327–3338`) |
| `_previsto_do_dia`, `_jornada_do_dia`, `_intervalo_descontado_no_dia` | Recebendo datetime, extraem **`quando.date()`**; não consultam mapa de turno. Recebendo date, usam a própria data. Por isso o ramo individual e o mensal podem perguntar por dias diferentes. (`app/ponto/motor_calculo_v2.py:1227–1253`; `app/ponto/motor_calculo_v2.py:1273–1277`; `app/ponto/motor_calculo_v2.py:1313`; `app/ponto/motor_calculo_v2.py:2835`; `app/ponto/motor_calculo_v2.py:3128`) |
| `_janela_do_dia` / `_janela_do_par` | Critério **misto**: folga pelo memo por instante; depois **`entrada.date()`** para empresa, marcos, extra declarada e autorização. A exceção L-084 do dia consulta `data_turno` no mapa e seus marcos. Os clipadores recebem essa decisão e produzem o par efetivo usado no trabalhado/noturno. (`app/ponto/motor_calculo_v2.py:1526–1536`; `app/ponto/motor_calculo_v2.py:1582–1594`; `app/ponto/motor_calculo_v2.py:1646–1653`; `app/ponto/motor_calculo_v2.py:1760–1763`) |
| `_dia_sem_previsto_fora_da_janela` | Recebe **`data_jornada`** do Base. Usa esse dia para feriado, empresa, previsto, autorização e escala certa; pode zerar trabalhado, noturno e HE e colapsar o segmento efetivo. Não deriva outro dia. (`app/ponto/motor_calculo_v2.py:2461`; `app/ponto/motor_calculo_v2.py:1448–1487`) |
| `minutos_noturnos`: janela, teto e prorrogação | Usa **instantes localizados**, percorrendo datas civis entre início e fim; cria 00–05 e 22–24. Recortes acima de 26h retornam zero. Havendo interseção com 00–05, a prorrogação vai até **`fim`**, sem teto em `s2_ini`; portanto os segmentos podem sobrepor. Não consulta juiz de turno. (`app/ponto/motor_calculo_v2.py:89–118`) |
| `minutos_noturnos_dos_intervalos` | Mede pausas dentro do recorte recebido: união de intervalos, cortes nas horas locais e presença noturna segundo prefixos de `minutos_noturnos`; limita o desconto ao noturno total. É chamado pelo total noturno e pelo split. (`app/ponto/motor_calculo_v2.py:162–196`; `app/ponto/motor_calculo_v2.py:985`; `app/ponto/motor_calculo_v2.py:442`) |
| `_inicio_do_trecho_extra` → `he_noturna` | Recua do **instante final** pela quantidade de minutos extras trabalhados, descontando pausas. Depois mede **`inicio_extra → fim` isoladamente**: esse trecho decide sua própria prorrogação, sem herdar a janela do par inteiro. Reparte o noturno entre HE50/100 proporcionalmente. (`app/ponto/motor_calculo_v2.py:392–406`; `app/ponto/motor_calculo_v2.py:430–451`) |
| `_aplicar_he_noturna`: split do par | Passa **`periodo.entrada/saida` brutos**, não `efetivo_entrada/saida`; lê intervalos e escreve os subconjuntos noturnos. Chamado pelos cálculos individuais 12x36/Comercial e pelas dobras/HE mensais Partido/Comercial. (`app/ponto/motor_calculo_v2.py:1012–1022`; `app/ponto/motor_calculo_v2.py:2855`; `app/ponto/motor_calculo_v2.py:3294`; `app/ponto/motor_calculo_v2.py:3104`; `app/ponto/motor_calculo_v2.py:3160`; `app/ponto/motor_calculo_v2.py:3191`; `app/ponto/motor_calculo_v2.py:3361`; `app/ponto/motor_calculo_v2.py:3423`) |
| `calcular_noturnas` / `_escrever_noturnas` | O total noturno usa o **par efetivo recebido**, desconta intervalos e aplica o fator. Os três cálculos de período escrevem por essa porta; isso difere do par bruto entregue ao split. (`app/ponto/motor_calculo_v2.py:983–1005`; `app/ponto/motor_calculo_v2.py:2792`; `app/ponto/motor_calculo_v2.py:2877–2897`; `app/ponto/motor_calculo_v2.py:3260`) |

Nos marcadores aproximados do pedido, a cópia contém: prorrogação/teto em `minutos_noturnos:93–112`, medição da cauda em `he_noturna:440–443`, split bruto em `_aplicar_he_noturna:1012–1018`, classificação FT em `2448–2457` e dobra mensal do partido em `3092–3104`. (`app/ponto/motor_calculo_v2.py:93–112`; `app/ponto/motor_calculo_v2.py:440–443`; `app/ponto/motor_calculo_v2.py:1012–1018`; `app/ponto/motor_calculo_v2.py:2448–2457`; `app/ponto/motor_calculo_v2.py:3092–3104`)

O helper **`_dia_da_jornada_do_par` do patch retirado não integra o caminho presente**: entre o predicado de dobra e o de folga não há esse método; os dois escritores individuais continuam passando `e.date()`. O armazenamento por instante feito pelo Base é `_folga_do_par`, um booleano, enquanto o dia por período é gravado depois da chamada. (`app/ponto/motor_calculo_v2.py:914–936`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:2457–2460`)

Há ainda duas atribuições complementares: o crédito de dia aberto agrupa pelo mapa e pelos turnos do juiz, mas transfere dos períodos calculados apenas trabalhado/fora da janela; banco e reflexo de DSR misturam agrupamento por jornada com seleções de extras por entrada civil. (`app/ponto/motor_calculo_v2.py:2514–2560`; `app/ponto/motor_calculo_v2.py:2577–2579`; `app/ponto/motor_calculo_v2.py:2636–2644`; `app/ponto/motor_calculo_v2.py:3442–3447`; `app/ponto/motor_calculo_v2.py:3465`; `app/ponto/motor_calculo_v2.py:3487–3495`)

**Fechamento e DiaPago.**

| Sítio/função | Dia usado, chamador e destino |
|---|---|
| `apurar_por_fatia` | Seleciona batidas por **data civil local**, lida de `turnos.data_local`, com contexto de ±1 dia. Chama o motor e mescla `dia_da_jornada` preservando os objetos/períodos. É chamado pelo recalculador e pelos leitores da autoridade do período. (`app/ponto/services/fechamento.py:203–204`; `app/ponto/services/fechamento.py:244–262`; `app/ponto/services/fechamento.py:286–316`; `app/ponto/services/fechamento.py:754–756`) |
| `recalcular_fechamento_mes`: filtro `_ft_certa` | Pergunta `escala_certa_no_dia` pela **data civil local de `p.entrada`**, sem mapa de jornada. A decisão determina se há adicional de FT ou apenas hora trabalhada. (`app/ponto/services/fechamento.py:928–935`; `app/ponto/services/fechamento.py:965–985`) |
| Mesmo recalculador: promoção `_ft_feriado` | Novamente usa **data civil local da entrada** para obter vínculo e perguntar `feriado_vira_folga`. Promove toda a hora a HE100/feriado; apenas lê o split noturno já escrito pelo motor. (`app/ponto/services/fechamento.py:944–964`) |
| `dia_pago._dia_de(p,mapa)` | Sem entrada: `None`. Com mapa e valor para `id(p)`: dia do mapa. Sem mapa, mapa vazio, chave ausente ou valor `None`: **data civil local da entrada**. Não chama diretamente o juiz. (`app/ponto/services/dia_pago.py:109–132`) |
| `dia_pago.lavrar` | Chama `_dia_de` nos quatro conjuntos: normais, FT certa, FT feriado e FT sem escala. Recebe listas já classificadas pelo fechamento **e** `resultado.dia_da_jornada`; grava a linha em `DiaPago.data`. Logo, mudar a chave diária aqui não reclassifica a rubrica anteriormente escolhida pelo civil. (`app/ponto/services/dia_pago.py:173–184`; `app/ponto/services/dia_pago.py:234–265`; `app/ponto/services/dia_pago.py:345–360`; `app/ponto/services/fechamento.py:1201–1217`) |
| O85, períodos sem vínculo | O fechamento seleciona dias órfãos por batidas civis e atribui cada período órfão à **data civil local da entrada**, sem ler seu mapa; entrega minutos já datados à lavratura. (`app/ponto/services/fechamento.py:793–796`; `app/ponto/services/fechamento.py:838–843`; `app/ponto/services/fechamento.py:1204`; `app/ponto/services/dia_pago.py:269–272`) |

**Quem lê esses resultados.**

| Leitor | Caminho e eventual nova decisão de dia |
|---|---|
| Fechamento | Soma propriedades de `ResultadoMes`: HE somente dos normais; noturno dos normais + FT. Depois aplica sua partição FT civil. (`app/ponto/motor_calculo_v2.py:321–344`; `app/ponto/services/fechamento.py:918–985`) |
| Espelho — dinheiro diário | Carrega `DiaPago` por `data` e expõe `pago_h`/rubricas. Essas rubricas já chegam com a pertença e classificação descritas acima. (`app/ponto/services/dia_pago.py:854–856`; `app/ponto/services/espelho.py:406–410`; `app/ponto/services/espelho.py:805–813`) |
| Espelho — geometria e resumo | Batidas/cards usam o juiz de batidas. Entretanto, `somar_periodos(..., dentro=...)` ainda recorta competência pela **entrada civil**, sem mapa de jornada. (`app/ponto/services/espelho.py:450–460`; `app/ponto/services/espelho.py:574–577`; `app/ponto/services/espelho.py:82–86`) |
| `porta_export` | `dias_fora_do_dia_pago` pede o resultado à autoridade, mas reagrupa trabalhado por **entrada civil**, comparando com `DiaPago.data`. Pode acusar divergência diária de uma cauda que DiaPago colocou no dia anterior. (`app/folha/porta_export.py:74–78`; `app/folha/porta_export.py:101–118`) |
| PDF do espelho | Chama `espelho_do_colab`; desenha `pago_h` e rubricas diárias. O total passa por `folha_manda`, que consulta o fechamento/soma lavrada. (`app/relatorios/pdf_espelho.py:596–602`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/pdf_espelho.py:901–914`; `app/relatorios/cartao_pela_celula.py:323–342`) |
| Exportação e PDF do TXT | Eventos leem os campos do fechamento; splits HE noturna não são eventos adicionais separados. O PDF do TXT traduz o conteúdo exportado e suas datas, sem atribuir novamente minutos a turnos. (`app/folha/export.py:420–447`; `app/folha/pdf_export.py:54–65`; `app/folha/pdf_export.py:111–137`; `app/folha/pdf_export.py:173–185`) |

**Contra-exemplos e sítios que discordam.** Os casos abaixo são fixtures/assertivas da cópia ou deduções estáticas explicitadas; sua execução nesta auditoria é **[não verificado]**.

| Caso | Pertença e sítio que erraria/divergiria |
|---|---|
| Madrugada sem entrada noturna anterior: escala 21–05, E **02/09 00:57**, saídas 02:29/05:00 | O teste põe batidas e card em **01/09**. Os escritores individuais por `e.date()`, o fechamento FT civil e o fallback de `_dia_de` perguntariam por **02/09**. (`app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:118–129`; `app/ponto/motor_calculo_v2.py:2796–2809`; `app/ponto/motor_calculo_v2.py:3264–3268`; `app/ponto/services/fechamento.py:935–949`; `app/ponto/services/dia_pago.py:132`) |
| 12x36: d19–d+1 04:30 / d+1 05:30–07; só **d+1 feriado** | Base herda **d**, mas `Motor12x36.calcular_periodo` dobra a cauda pelo civil **d+1**. O teste caracterizador espera **90 min** indevidos segundo a regra do teste vizinho, que exige zero e está `expectedFailure`. (`app/ponto/motor_calculo_v2.py:2405–2408`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/tests/test_o280_bateria_da_lei.py:1549–1577`) |
| Mesmo 12x36, só **d feriado** | O caracterizador espera **570 min** de dobra, deixando a cauda de 90 fora. O dia herdado é d, mas o escritor individual usa d+1. O noturno da cauda também fica zero porque é medido separadamente. A exigência de prorrogação entre esses pares está expressamente condicionada à pergunta aberta do F7g; não cabe tratá-la como regra já resolvida. (`app/ponto/tests/test_o280_bateria_da_lei.py:1579–1626`; `app/ponto/motor_calculo_v2.py:2792–2809`) |
| Partido 21–00 / 03–07 | O segundo par entra civilmente em d+1, mas pertence a d. A dobra dos **normais já usa o juiz e alcança cada par**. Se os pares estiverem em FT, o fechamento civil pode promover o segundo ao feriado errado, ou deixar de promovê-lo quando o feriado é d; o filtro de escala certa pode igualmente consultar o dia errado. Essa última conclusão é dedução do fluxo. (`app/ponto/tests/test_o280_bateria_da_lei.py:1758–1781`; `app/ponto/motor_calculo_v2.py:2942–2955`; `app/ponto/motor_calculo_v2.py:3097–3104`; `app/ponto/services/fechamento.py:934–950`) |
| Partido com vão **7h**: d19–23 / d+1 06–10

[truncado em 20000 de 21998 caracteres]

_2682 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
