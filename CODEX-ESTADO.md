# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 16:20** · publicado em **07/11/2026 16:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 16:20 · sessao 504cc4b15e21 · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura permanece parcial. PROCEDENTES as classes (1), (2) e (4); NAO PROCEDE a permanência dos defeitos enumerados nas classes (3) e (5). Persistem falhas de alcance dos selos e uma cegueira nova no I22.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2019`; `app/chamados/juizes.py:932`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/services/fio.py:1172`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

Identidade independente do commit/pai, ocorrência dos contraexemplos em produção, reprodução em banco, RED→GREEN histórico e suíte inteira `rc=0`: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: as portas ainda podem escolher canônicas diferentes, por divergência de universo.**

O contraexemplo anterior do horizonte foi corrigido: `aplicar` escolhe a canônica antes de recortar as perdedoras que pode escrever. **NAO PROCEDE sua permanência nessa forma.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`)

Entretanto, `universo_do_grupo` não é idempotente. Contraexemplo estático: mesmo colaborador, motivo e dia no horizonte; `L` sem episódio, muda e menor PK; `A` do episódio α, muda; `B` do episódio β, respondida. `do_fato(α)` entrega `[L,A]`, e `abrir(α)` segura `L`. Episódios distintos são fatos distintos. (`app/chamados/models.py:2565`; `app/chamados/models.py:2567`; `app/chamados/models.py:2691`; `app/chamados/models.py:2692`; `app/chamados/services/fio.py:1240`)

O agrupador também entrega `[L,A]`, mas `aplicar` expande esse grupo novamente. Ao visitar `L`, sem episódio, `universo_do_grupo` chama o ramo de `do_fato` por dia, que inclui `B`. O apply passa a escolher `B` e retrata `A`: **a expansão de uma linha legada uniu episódios diferentes**. (`app/chamados/services/fio.py:1120`; `app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1079`; `app/chamados/services/fio.py:1081`; `app/chamados/models.py:2571`; `app/chamados/models.py:2598`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Assim, DRY e apply podem divergir; abrir e migração usam diretamente `do_fato`, enquanto backfill/dedup aplicam a expansão adicional. **NAO PROCEDE segundo implementador da preferência resposta/menor PK localizado; PROCEDENTE derivação paralela do universo entregue ao juiz.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:359`; `app/chamados/management/commands/dedup_perguntas_fato.py:70`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1255`; `app/chamados/services/fio.py:984`)

**2. Classe (2) — PROCEDENTE pelas ordens residuais; NAO PROCEDE outro `select_for_update` explícito de produção localizado para as quatro entidades.**

As quatro aquisições explícitas estão em `trava_na_ordem`. Os dois caminhos apontados na volta anterior receberam antecipação das travas. (`app/chamados/models.py:2513`; `app/chamados/models.py:2520`; `app/chamados/models.py:2533`; `app/chamados/models.py:2537`; `app/chamados/models.py:290`; `app/chamados/services/materializacao.py:676`)

Persistem, porém:

- **Pergunta → Disputa:** `remover_motivo` abre atomic, carimba a Pergunta — com `save` real — e depois salva a Disputa, sem antecipar a ordem. Os receivers de reativação de Posto e calibração de âncora chegam diretamente a esse método. (`app/chamados/models.py:2011`; `app/chamados/models.py:2019`; `app/chamados/models.py:2911`; `app/chamados/models.py:2023`; `app/colaboradores/signals.py:71`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:155`; `app/escala/signals.py:161`)
- **Chamado → Disputa por chamadas sucessivas ao próprio dono:** a aprovação de justificativa trava Colaborador e Chamados sem incluir as Disputas abertas. Depois, o fechamento da origem chama `retratar`, chega ao reconciliar e adquire as Disputas abertas. Um Chamado elegível ao fechamento e outra Disputa aberta da mesma pessoa bastam para alcançar essa ordem. (`app/ponto/services/justificativa.py:123`; `app/ponto/services/justificativa.py:134`; `app/ponto/services/justificativa.py:211`; `app/ponto/services/justificativa.py:39`; `app/ponto/services/justificativa.py:44`; `app/chamados/reconciliador.py:523`; `app/chamados/models.py:655`; `app/chamados/models.py:694`)

Portanto, centralizar a aquisição não garante a ordem global entre chamadas: uma chamada posterior pode adquirir linhas de tabela anterior ainda não travadas. (`app/ponto/services/justificativa.py:123`; `app/chamados/models.py:694`)

**3. Classe (3) — NAO PROCEDE a permanência dos defeitos anteriores enumerados.**

O atomic engloba o processamento; as travas precedem a conferência do horizonte empresarial, mérito e exportada. `_ent` agora é reiniciado por colaborador. O horizonte da escrita dos grupos e o veto dos `sem_ato` também são lidos nesse envelope. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:208`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`)

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal:** qualquer recusa levanta exceção dentro do atomic; o retorno posterior zera os contadores. `abaixo_do_horizonte` é informativo, e o dedup corretamente o exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:278`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `app/chamados/management/commands/dedup_perguntas_fato.py:91`)

**NAO PROCEDE segundo escritor do backfill localizado:** os commands preparam o DRY e encaminham a escrita para `aplicar`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:401`; `app/chamados/management/commands/dedup_perguntas_fato.py:89`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Exclusão mútua contra entrega concorrente da competência: **[nao verificado]**; `competencia_entregue` faz leitura simples, o que não invalida, por si, a conferência literal exigida dentro do atomic. (`app/folha/export.py:581`)

**4. Classe (4) — PROCEDENTE: significado declarado, mas censo incompleto e leitores de decisão incompatíveis.**

Pergunta de fato usa **dia do turno**, com `conversa=False`; conversa usa **dia civil da emissão**, com `conversa=True`. O proxy e `resolvedora_valida` agora respeitam essa distinção. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O censo lista `_dia_da_disputa`, `dias_do_fato`, prévia, copiloto e diagnóstico, mas conclui “Nenhum deles escreve”. Essa conclusão não considera os consumidores que decidem a escrita. (`app/chamados/services/fio.py:1034`)

Há provas concretas:

- **Leitor censado alimenta escrita:** `dias_do_fato` inclui datas das conversas; o arquivamento usa seu resultado para decidir pelo corte e depois silenciar o Chamado. Conversa encerrada não impede o caminho, pois a guarda procura pergunta viva. (`app/chamados/juizes.py:209`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288`)
- **Escritor fora do censo:** `fabrica_da_celula.perguntar` seleciona por motivo/data sem distinguir conversa e chama a porta que pode descarimbar a pergunta. Os filtros posteriores também não excluem conversa. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/relavra_via.py:179`; `app/chamados/services/relavra_via.py:234`; `app/chamados/services/relavra_via.py:244`)
- **Decisores fora do censo:** `dia_da_pergunta_trancado` transforma essa data em competência e pode matar a pergunta antes da resposta; `_dias_das_perguntas_vivas` transforma a data civil em fato usado por `competencia_do_chamado`. (`app/chamados/juizes.py:932`; `app/chamados/juizes.py:945`; `app/chamados/juizes.py:1141`; `app/chamados/juizes.py:2037`; `app/chamados/juizes.py:2058`; `app/chamados/services/competencia_trancada.py:92`)

Também falta o leitor de classificação `pergunta_no_app.retencao`, que pode tomar a conversa civil como pergunta do dia do furo. (`app/chamados/services/pergunta_no_app.py:92`)

**5. Classe (5) — NAO PROCEDE no leitor de “novas perguntas” localizado.**

Ele lê `data_evento` da Pergunta e compara com o dia local da aplicação da 0052. Ausência do registro ou dia nulo alarma; o escape dos fatos anteriores está declarado. Não usa `Disputa.aberta_em`. (`app/chamados/services/fio.py:1158`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1168`; `app/chamados/services/fio.py:1172`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência geral de mordida.**

| Selo | Burla que permanece |
|---|---|
| A — trava | O detector reconhece SQL literal `FOR UPDATE`, mas a varredura só o chama se houver `select_for_update` no arquivo. Um arquivo contendo somente SQL cru passa. O mutante testa o detector isolado, sem esse filtro. (`app/chamados/tests/test_e1_v7_catracas.py:120`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/tests/test_e1_v7_catracas.py:277`) |
| A — trava | `lock = qs.select_for_update; lock()` não é chamada de atributo nem o `getattr` reconhecido. (`app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:116`) |
| B — canônica | Argumento nomeado recortado escapa: o detector examina somente `n.args[0]`. `min`, `sorted()[0]` e QuerySet filtrado continuam no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:185`; `app/chamados/tests/test_e1_v7_catracas.py:26`) |
| C — escrita | `gravar = p.save; gravar()` escapa: há resolução de alias de importação, mas não dessa atribuição. Helper externo segue no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:208`; `app/chamados/tests/test_e1_v7_catracas.py:217`; `app/chamados/tests/test_e1_v7_catracas.py:28`) |

A/B percorrem `app` com exclusões; C examina os dois commands. Esse alcance não equivale a uma garantia semântica sobre todo o repositório. (`app/chamados/tests/test_e1_v7_catracas.py:37`; `app/chamados/tests/test_e1_v7_catracas.py:43`; `app/chamados/tests/test_e1_v7_catracas.py:194`)

Os mutantes têm expectativas que mordem as formas sintéticas escolhidas, inclusive homônimos e aliases. **Não demonstram o caminho completo**, como mostra o SQL cru filtrado antes do detector. (`app/chamados/tests/test_e1_v7_catracas.py:270`; `app/chamados/tests/test_e1_v7_catracas.py:277`; `app/chamados/tests/test_e1_v7_catracas.py:340`; `app/chamados/tests/test_e1_v7_catracas.py:383`)

Quanto aos REDs v8:

- Canônica exige duas retratações, preservação de p1, restauração do índice e idempotência. Porém usa uma única batida e não chama efetivamente abrir/migração; não cobre a legada que conecta episódios distintos. (`app/chamados/tests/test_e1_v8.py:50`; `app/chamados/tests/test_e1_v8.py:65`; `app/chamados/tests/test_e1_v8.py:72`; `app/chamados/tests/test_e1_v8.py:77`)
- Ordem exige presença das tabelas e ausência de violações, mas o observador registra somente a primeira operação por **tabela**. Uma aquisição posterior de outra PK desaparece; uma consulta registrada também não prova que encontrou linha. (`app/chamados/tests/test_e1_v8.py:108`; `app/chamados/tests/test_e1_v8.py:121`; `app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)
- Conversa verifica nascimento aceito e fato cruzado recusado isoladamente; não testa a composição do I22. (`app/chamados/tests/test_e1_v8.py:138`; `app/chamados/tests/test_e1_v8.py:148`)

Execução Django desses REDs e comprovação histórica da mordida: **[nao verificado]**.

**7. Escritores e leitores — PROCEDENTE incompatibilidade residual de datas e imprecisão de `abaixo_do_horizonte`.**

| Campo/estado | Quem escreve | Quem lê / significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, pelas portas de carimbo/descarimbo; lote tem UPDATE; exceções existentes são `encerrar_por_fato_no_espelho` e `relavrar`. (`app/chamados/models.py:2818`; `app/chamados/models.py:2983`; `app/chamados/models.py:2992`; `app/chamados/services/relavra_via.py:123`) | C6, identidade e backfill leem via de resolução/identidade. Duplicata é terminal antes da resposta; a documentação corrigida concorda com isso. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:2569`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/models.py:2893`) |
| “retratada” | É estado representado pela via: dedup grava `VIA_DUPLICATA`; remoção de motivo grava `VIA_MOTIVO_RETRATADO`. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2019`) | `fora_do_fato` exclui da identidade; C6 torna terminal quando carimbada. A resposta permanece histórica. (`app/chamados/models.py:2922`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2894`) |
| `data_evento` | Nascimento; preenchimento no reapontamento; rechaveamento. (`app/chamados/models.py:2739`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade/horizonte usam turno; leitores do item 4 ainda recebem dia civil sem distinguir. **Significado incompatível entre consumidores.** (`app/chamados/models.py:2599`; `app/chamados/services/fio.py:1043`; `app/chamados/juizes.py:932`) |
| `conversa` | As portas fixam `False`/`True`; `_nascer` persiste e agora transmite ao proxy. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/models.py:2740`) | Identidade, índice e resolvedora usam “sem fato”. Os leitores do item 4 ignoram o discriminador. (`app/chamados/models.py:2599`; `app/chamados/models.py:3207`; `app/chamados/juizes.py:1969`) |
| `abaixo_do_horizonte` | `aplicar` acumula perdedoras fora de `q_do_horizonte`; retorno deduplica PKs. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`) | CLI chama de perdedoras do acervo abaixo; dedup corretamente não interpreta como recusa. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:419`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |

O último significado é mais amplo que o anunciado: uma BATIDA perdedora com `data_evento=None` também entra em `abaixo_do_horizonte`. O agrupador permite esse NULL, embora o censo o distinga como `batida_sem_dia`. **Não escreve a linha, mas classifica “sem dia” como “abaixo”.** (`app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1111`; `app/chamados/services/fio.py:1148`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`)

**8. Reincidências — PROCEDENTES universo paralelo, ordem de trava, cobertura insuficiente e relógio não injetado; NAO PROCEDE segundo juiz de preferência ou segundo escritor do backfill demonstrado.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2023`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/services/fio.py:978`; `app/chamados/management/commands/dedup_perguntas_fato.py:90`)

O relógio residual alcança **teste novo**: apesar de declarar todos os instantes literais/injetados, ele chama `ch.fechar` sem congelamento, chegando a `timezone.now()`. As novas fixtures também criam Disputa com `aberta_em=auto_now_add`. Portanto contar zero `now()` literal no diff não demonstra injeção. (`app/chamados/tests/test_e1_v8.py:12`; `app/chamados/tests/test_e1_v8.py:106`; `app/chamados/models.py:450`; `app/chamados/models.py:244`; `app/chamados/tests/test_e1_v8.py:104`; `app/chamados/models.py:1715`)

**NAO PROCEDE subida dos cinco TETOS sem frase C8:**

| Cenário | Teto / aumento | Referência |
|---|---:|---|
| Estado sem pergunta | 27 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 59 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Uma cobrança fecha | 208 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 262 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Três perguntas, sem cruzar | 10 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:193` |

A justificativa é compatível com **duas consultas adicionais por entrada**: Disputas e Chamados. Isso satisfaz a exigência documental da C8, mas não prova tempo ou contenção constantes: o conjunto de linhas bloqueadas cresce com Disputas abertas e Chamados vivos. Medições citadas nos comentários: **[nao verificado]**. (`PLANO_PISCADA.md:10`; `app/chamados/models.py:2518`; `app/chamados/models.py:2520`; `app/chamados/models.py:2528`; `app/chamados/models.py:2533`)

A diferença de mérito anteriormente levada a desenho permanece: migração retrata perdedoras sem o veto de `admin_decidiu` com resposta que o backfill aplica. (`app/chamados/services/fio.py:1265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`)

**9. `resolvedora_valida` — PROCEDENTE cegueira do I22; NAO PROCEDE transformação automática da entrada factual em conversa ou cegueira equivalente no I8b.**

`abrir` força `conversa=False`, e o proxy preserva esse valor. A mudança não desvia essa porta para o retorno antecipado da conversa. (`app/chamados/models.py:2631`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O contraexemplo do **I22 não exige flag corrompida**: Chamado vivo com perguntas de fato de outro turno e uma conversa legítima apontada para ele. A conversa agora devolve `True`; o `any(...)` fica verdadeiro e o I22 deixa de acusar que nenhuma pergunta **de fato** resolve aquele turno. (`app/chamados/models.py:2642`; `app/chamados/juizes.py:1969`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1043`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

O **I8b continua examinando cada pergunta viva individualmente**: a conversa irmã não impede que uma pergunta factual cruzada seja rejeitada pelo juiz. Os REDs novos cobrem a recusa individual, mas não a composição que mascara o I22. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`; `app/chamados/management/commands/auditar_invariantes_chamados.py:896`; `app/chamados/tests/test_e1_v8.py:148`)

## 07/11 16:17 · sessao f802bf7d346e · - · - · origem=Ronald (tmux)

**2 — PROCEDENTE ordem implícita residual; NAO PROCEDE outro `select_for_update` explícito de produção localizado.**

As quatro aquisições explícitas estão no dono: `app/chamados/models.py:2513`, `:2520`, `:2533`, `:2537`. Os dois caminhos antigos foram antecipados: `_gravar_estado` em `app/chamados/models.py:290` e ausência rejeitada em `app/chamados/services/materializacao.py:676`.

Permanecem dois contraexemplos de código:

1. **Pergunta → Disputa sem trava prévia:** `DisputaSupervisao.remover_motivo` abre atomic em `app/chamados/models.py:2011`, carimba a Pergunta em `:2019`, cujo save ocorre em `:2911`, e depois salva a Disputa em `:2023`. Não chama `trava_na_ordem`. Entradas reais: reativação de Posto em `app/colaboradores/signals.py:77` e calibração de âncora em `app/escala/signals.py:161`. Os receivers consultam as disputas e chamam diretamente, sem antecipar essas travas (`app/colaboradores/signals.py:71`; `app/escala/signals.py:155`).

2. **Chamado → Disputa por chamadas sucessivas ao próprio dono:** aprovação de justificativa chama `travar_origem_antes` dentro do atomic (`app/ponto/services/justificativa.py:134`, `:135`). O helper trava Colaborador e Chamados **sem** `abertas_do_colaborador=True` (`:123`). Depois `encerrar_chamado_de_origem` (`:211`) trava novamente somente Colaborador/origem (`:39`) e chama `retratar` (`:44`). Este alcança `reconciliar` (`app/chamados/reconciliador.py:523`), que chama `_decisao_do_fechamento` (`app/chamados/models.py:634`), `_recarregar_estado_se_mudou` (`:655`) e finalmente adquire as Disputas abertas em `:694`, depois dos Chamados já adquiridos. Premissa suficiente: origem viva sem retenção e outra Disputa aberta da mesma pessoa. Há variante no bloqueio de entrada (`app/ponto/services/justificativa.py:76`, `:83`). Reprodução/concorrência real: **[nao verificado]**.

**6 — PROCEDENTE alcance insuficiente; NAO PROCEDE ausência geral de mordida.**

Burla importante **não declarada**: A reconhece SQL literal com tabela e `FOR UPDATE` (`app/chamados/tests/test_e1_v7_catracas.py:120`), mas o pipeline só chama o detector se o arquivo contiver o texto `select_for_update` (`:238`). Um arquivo contendo apenas `cursor.execute('SELECT ... FROM "chamados_perguntadisputa" FOR UPDATE')` passa a catraca. O mutante chama diretamente `travas_fora`, sem passar pelo filtro que o excluiria (`:277`).

Outras burlas:

- A: método ligado, `lock = qs.select_for_update; lock()`, não é chamada de atributo nem `getattr` reconhecido (`app/chamados/tests/test_e1_v7_catracas.py:100`, `:116`).
- B: recorte passado por argumento nomeado não é examinado; só `n.args[0]` é lido (`:185`). `min`, `sorted()[0]` e QuerySet filtrado seguem no limite declarado (`:26`).
- C: método ligado, `gravar = p.save; gravar()`, não tem resolução transitiva da atribuição, só nome/alias de import/getattr (`:208`, `:217`). Helper externo também segue no limite declarado (`:28`).
- A/B só percorrem `app` e excluem diretórios/nome de testes; C só dois commands (`:35`, `:38`, `:364`). [nao verificado]

Executei em memória os 22 testes das três classes com `python3 -B`, substituindo apenas `django.test.SimpleTestCase` por `unittest.TestCase`, sem Django, banco ou arquivos escritos: 22 passaram, incluindo os três zeros e mutantes existentes. Essa execução não é a suíte Django nem prova PostgreSQL; por exigência de referência para afirmações, resultado sem arquivo persistido: **[nao verificado]**. Os próprios testes e suas expectativas estão em `app/chamados/tests/test_e1_v7_catracas.py:232`, `:297`, `:362`.

REDs v8 têm mordida direcionada por leitura do código:

- Canônica verifica p1, duas retratações, p1 intocada, restauração do índice e idempotência (`app/chamados/tests/test_e1_v8.py:63`, `:72`, `:75`, `:77`, `:78`). Porém chama `do_fato + canonica_do_grupo`, sem invocar efetivamente `abrir` nem migração (`:65`, `:66`).
- Ordem exige tabelas e ausência de violações (`:108`, `:109`, `:110`, `:121`, `:122`), mas herda observador que registra só a primeira operação por **tabela**, inclusive consulta sem provar linhas retornadas (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`, `:46`). Não é prova de ordem por PK nem bloqueio real de todas as linhas.
- Conversa comprova nascimento com dia civil e destino antigo (`app/chamados/tests/test_e1_v8.py:138`, `:142`), com controle factual recusado (`:148`). Não testa todos os defeitos estruturais de I8b/I22.

**8 — TETOS: NAO PROCEDE subida sem C8; PROCEDENTE ressalva sobre “custo constante”.**

Cinco aumentos têm frase escrita ao lado: 27/+2 (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`), 59/+2 (`:118`), 208/+4 (`:161`), 262/+8 (`:162`), 10/+2 (`:193`). C8 exige precisamente justificativa ao lado (`PLANO_PISCADA.md:10`).

A decomposição de +2 por entrada é compatível com uma consulta das Disputas e outra dos Chamados (`app/chamados/models.py:2519`, `:2532`). É número fixo de consultas por entrada; o número de linhas examinadas/travadas depende das Disputas abertas e Chamados vivos (`:2518`, `:2528`). Tempo/contenção e medições citadas nos comentários: **[nao verificado]**.

**Relógio residual também existe no arquivo novo**, não apenas nas linhas antigas descartadas pela triagem: `test_e1_v8.py:12` declara todos os instantes literais/injetados, mas o novo teste chama `ch.fechar` sem congelamento (`app/chamados/tests/test_e1_v8.py:106`), que alcança `_gravar_estado` (`app/chamados/models.py:454`, `:450`) e `timezone.now()` (`:244`). Fixtures novas criam Disputa (`app/chamados/tests/test_e1_v8.py:49`, `:104`, `:136`, `:147`) cujo `aberta_em` usa `auto_now_add=True` (`app/chamados/models.py:1715`). Efeito no resultado: **[nao verificado]**.

## 07/11 16:17 · sessao bb8915b7b890 · - · - · origem=Ronald (tmux)

1. **Classe (1) — PROCEDENTE: permanece divergência de universo, agora introduzida por `universo_do_grupo`.** A identidade BATIDA é por episódio, e dois episódios no mesmo dia são fatos distintos. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/dedup_perguntas_fato.py:54`)

   Contraexemplo estático: mesmo colaborador/motivo/dia acima do horizonte; `L` legada sem episódio, muda e menor PK; `A` do episódio α, muda; `B` do episódio β, respondida. `do_fato(α)` retorna `[L,A]`, porque seu OR acrescenta somente as legadas sem episódio daquele dia; `abrir(α)` escolhe `L`. (`app/chamados/models.py:2565`; `app/chamados/models.py:2567`; `app/chamados/models.py:2691`; `app/chamados/models.py:2692`; `app/chamados/services/fio.py:984`)

   `grupos_de_colisao` detecta α com a legada e entrega `[L,A]`. `aplicar` chama novamente `universo_do_grupo([L,A])`; a iteração sobre `L`, sem episódio, chama `do_fato(..., batida_origem_id=None)`, que consulta **todas** as perguntas daquele motivo/dia, incluindo `B`. Assim o apply escolhe `B` e retrata `A`, embora a porta α escolhesse `L`. `universo_do_grupo` não é idempotente, e reaplicar expansão alarga o fato. (`app/chamados/services/fio.py:1116`; `app/chamados/services/fio.py:1120`; `app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1076`; `app/chamados/services/fio.py:1079`; `app/chamados/services/fio.py:1081`; `app/chamados/models.py:2571`; `app/chamados/models.py:2598`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

   O DRY escolhe diretamente sobre os PKs fornecidos pelo agrupador, enquanto o apply expande de novo: portanto DRY/apply também podem informar/escolher canônicas diferentes. A migração permanece com o universo direto de `do_fato`, que não faz essa segunda expansão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:358`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:359`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:70`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1255`)

   Ocorrência de produção e reprodução em banco: **[nao verificado]**.

   **NAO PROCEDE o contraexemplo anterior do horizonte:** a canônica agora vem antes de restringir a escrita ao horizonte; tampouco localizei outro implementador de preferência por resposta/PK fora de `canonica_do_grupo`. Os consumidores centrais delegam a escolha. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:267`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1255`; `app/chamados/models.py:2692`)

2. **Classe (3) — NAO PROCEDE a permanência dos defeitos anteriores enumerados.** O atomic engloba o laço; as travas precedem a conferência do horizonte, mérito e exportada; o cache `_ent` nasce novamente para cada colaborador. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:208`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`)

   Qualquer recusa provoca exceção dentro do atomic e retorno com contadores zerados; `abaixo_do_horizonte` é separado das recusas, e o dedup o exclui corretamente desse conjunto. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:278`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `app/chamados/management/commands/dedup_perguntas_fato.py:91`)

   **NAO PROCEDE segundo escritor do backfill localizado:** os dois commands encaminham a execução à mesma `aplicar`; as escritas do apply estão nela, por `DecisaoPergunta.registrar`, `retratar_duplicata` e `evento`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:401`; `app/chamados/management/commands/dedup_perguntas_fato.py:89`; `app/chamados/management/commands/dedup_perguntas_fato.py:90`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:267`)

   A conferência de exportada é uma leitura simples da autoridade; exclusão mútua contra exportação concorrente não foi demonstrada. Isso é **[nao verificado]**, não prova de falha no requisito literal de reler dentro do atomic depois da trava. (`app/folha/export.py:581`; `app/folha/export.py:586`)

3. **Item 7 — PROCEDENTE imprecisão no significado de `abaixo_do_horizonte`.** O escritor coloca ali todo perdedor que **não satisfaz** `q_do_horizonte`, inclusive BATIDA com `data_evento=None`; não somente datas anteriores ao horizonte. O agrupador BATIDA admite data nula porque sua exclusão de NULL é por `batida_origem`, e basta outra linha do episódio estar no horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1043`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1110`; `app/chamados/services/fio.py:1111`)

   Exemplo estático: mesma batida/motivo; `p_null` muda com data nula, `p_datada` respondida no horizonte. `p_null` vai a `abaixo_do_horizonte`, enquanto o próprio censo a distingue como `batida_sem_dia`. A documentação e a saída do command chamam todas essas linhas de perdedoras abaixo do horizonte. A linha continua intocada, mas a classificação é mais ampla que o significado anunciado. (`app/chamados/services/fio.py:1148`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:419`)

4. **Item 8 — PROCEDENTE derivação paralela de universo; NAO PROCEDE segundo juiz de preferência ou segundo escritor de backfill demonstrado.** A preferência segue centralizada, mas o novo expansor de grupos produz universo diferente da porta/migração no contraexemplo acima. (`app/chamados/services/fio.py:978`; `app/chamados/services/fio.py:1070`; `app/chamados/models.py:2691`; `app/chamados/services/fio.py:1248`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`)

5. **Cobertura do RED de canônica — morde a regressão específica anterior; não cobre o novo contraexemplo.** A fixture usa uma única batida, compartilhada por todas as perguntas; não tem legada sem episódio nem segundo episódio. Ela exige retratar as duas perdedoras, preservar a canônica, reinstalar índice e obter zero retratações na segunda aplicação. Execução/RED→GREEN histórico: **[nao verificado]**. (`app/chamados/tests/test_e1_v8.py:50`; `app/chamados/tests/test_e1_v8.py:54`; `app/chamados/tests/test_e1_v8.py:63`; `app/chamados/tests/test_e1_v8.py:72`; `app/chamados/tests/test_e1_v8.py:75`; `app/chamados/tests/test_e1_v8.py:77`; `app/chamados/tests/test_e1_v8.py:78`)

6. **Questão de desenho da triagem anterior permanece:** `_migrar_sob_trava` retrata as perdedoras sem consultar `merito_perdedor`; o backfill veta expressamente `admin_decidiu` com resposta. A mudança atual não resolve essa diferença entre portas, embora ambas usem a mesma preferência de canônica. (`app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`)

## 07/11 16:17 · sessao d0b6bdfa5b99 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE: o censo está incompleto e há consumidores que decidem/escrevem com o dia civil da conversa.** O contrato atual separa turno (`abrir`, `conversa=False`) e emissão civil (`abrir_conversa`, `conversa=True`). (`app/chamados/models.py:2631`; `app/chamados/models.py:2635`; `app/chamados/models.py:2642`)

- `dias_do_fato`, já censado, alimenta uma escrita: o comando de arquivamento escolhe alvos pelo dia devolvido e pelo corte, chama `_aplicar` e silencia o chamado. Conversa encerrada não impede esse caminho: a guarda verifica existência de pergunta viva. Logo a frase “Nenhum deles escreve” não cobre a cadeia de decisão. (`app/chamados/juizes.py:209`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:113`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:141`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `app/chamados/services/fio.py:1040`)
- Outro leitor ausente do censo: `fabrica_da_celula.perguntar` seleciona perguntas pelo dia, sem excluir conversa, e chama `desfazer_carimbo_sem_lastro`. A porta relê pela elegibilidade/carimbo, consulta C6 e descarimba; esses filtros também não excluem conversa. Assim uma conversa carimbada pode entrar na manutenção do fato do turno cuja data coincida com sua emissão civil. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/relavra_via.py:179`; `app/chamados/services/relavra_via.py:234`; `app/chamados/services/relavra_via.py:240`; `app/chamados/services/relavra_via.py:244`)
- Também faltam no censo `dia_da_pergunta_trancado`, que calcula competência de `data_evento` sem discriminador e mata a pergunta antes da resposta; e `_dias_das_perguntas_vivas`, que transforma a data civil em fato usado por `competencia_do_chamado`. (`app/chamados/juizes.py:932`; `app/chamados/juizes.py:945`; `app/chamados/juizes.py:955`; `app/chamados/juizes.py:1141`; `app/chamados/juizes.py:2037`; `app/chamados/juizes.py:2040`; `app/chamados/juizes.py:2058`; `app/chamados/juizes.py:2062`)
- `pergunta_no_app.retencao` pode rotular uma conversa como pergunta viva do mesmo dia do furo. Esse consumidor não diferencia conversa no teste da data. (`app/chamados/services/pergunta_no_app.py:92`)

5. **NAO PROCEDE a reincidência no contador “nova pergunta”.** Ele lê `data_evento` da pergunta, compara com o dia local da aplicação da 0052 e declara o escape dos fatos anteriores. Não depende de `Disputa.aberta_em`. (`app/chamados/services/fio.py:1158`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1168`; `app/chamados/services/fio.py:1171`; `app/chamados/services/fio.py:1172`)

9. **PROCEDENTE para a cegueira do I22; NAO PROCEDE desvio automático das perguntas abertas pela porta de fato.** `abrir` força `conversa=False`, e `_nascer` passa esse discriminador ao proxy. A alteração não transforma essa entrada em conversa. (`app/chamados/models.py:2631`; `app/chamados/models.py:2673`)

O contraexemplo do I22 não exige dado malformado: chamado vivo com uma pergunta de fato de outro turno e uma conversa legítima apontada para ele. Antes, ambas podiam falhar na comparação; agora a conversa retorna `True`, e o `any(...)` do I22 torna-se verdadeiro, escondendo que nenhuma pergunta **de fato** resolve aquele turno. A conversa legítima pode receber destino pela própria porta pública. (`app/chamados/models.py:2635`; `app/chamados/models.py:2642`; `app/chamados/juizes.py:1969`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1043`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1047`)

**NAO PROCEDE a mesma cegueira individual no I8b:** ele examina cada pergunta viva separadamente; a pergunta de fato cruzada continua sendo rejeitada pelo juiz, independentemente da conversa irmã. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`; `app/chamados/management/commands/auditar_invariantes_chamados.py:895`; `app/chamados/management/commands/auditar_invariantes_chamados.py:897`; `app/chamados/juizes.py:2009`)

Os REDs novos verificam conversa aceita e fato cruzado recusado isoladamente; não verificam a composição de resolvedoras no I22. (`app/chamados/tests/test_e1_v8.py:135`; `app/chamados/tests/test_e1_v8.py:143`)

Ocorrência desses estados em produção, reprodução no banco e execução dos testes: **[nao verificado]**. [nao verificado]

## 07/11 15:47 · sessao d55aaa0b4dac · FIO-E0-V4-E2 · verificador · origem=Code

**Não atesto equivalência irrestrita.** O predicado ficou centralizado, mas há contraexemplos entre lote e chamadas separadas com Empresas divergentes em cache; também existe um caminho condicional de escrita pela tarefa. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:975`, `app/chamados/juizes.py:990`, `app/core/utils.py:18`)

Suíte, deltas efetivamente executados, RED anterior, estado de produção e sequência de pouso: **[nao verificado]**. [nao verificado]

**1) Quem escreve, quem lê e significado**

| Dado | Escritor → leitor; significado |
|---|---|
| `PeriodoFechado.fechado_em` | Nasce com `auto_now_add`; `trancar_periodo` renova ao retrancar uma competência reaberta. O juiz usa como **instante inferior exclusivo** do toque. Também é comparado com `resgatado_em` pelo encerrador de chamados. (`app/ponto/models.py:926`, `app/ponto/services/fechamento.py:1401`, `app/ponto/services/fechamento.py:1408`, `app/chamados/juizes.py:999`, `app/chamados/services/competencia_trancada.py:104`) |
| `PeriodoFechado.reaberto_em` | `reabrir_periodo` grava o instante; retrancar limpa. `periodo_trancado` e `competencia_trancada` leem **NULL versus não NULL**, não um horizonte temporal; `ativo` não decide. Outros leitores diretos usam o mesmo filtro. (`app/ponto/services/fechamento.py:1410`, `app/ponto/services/fechamento.py:1432`, `app/ponto/services/fechamento.py:1463`, `app/ponto/services/fechamento.py:1473`, `app/chamados/juizes.py:1894`, `app/chamados/services/competencia_trancada.py:311`) |
| `LogAuditoria.acao`, `modelo`, `objeto_id`, `descricao` | A porta `registrar_log` encaminha para `evento`, que cria a linha. São verbo/modelo textuais, ID inteiro sem FK e texto livre. Para cobrar: `cobrar_dia_manual/Colaborador/colab.pk`, com `dia=<data>`; resolver: `veredito_celula/CelulaDia/cel.pk`; desarquivar: `desarquivar_chamado/ChamadoColaborador/ch.pk`. O leitor reconhece exatamente essas combinações. (`app/core/auditoria.py:22`, `app/core/observ.py:134`, `app/core/models.py:296`, `app/chamados/services/cobrar_dia.py:37`, `app/ponto/services/veredito_celula.py:334`, `app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:137`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1199`) |
| `LogAuditoria.criado_em`, `usuario` | Instante automático e FK anulável; `evento` resolve o usuário pelo ator explícito/request. O juiz exige `criado_em > fechado_em`, usuário não NULL e diferente de sistema. **Não verifica papel ADMIN**: “humano” nesse predicado significa esses dois filtros. (`app/core/models.py:295`, `app/core/models.py:307`, `app/core/observ.py:108`, `app/core/observ.py:118`, `app/chamados/juizes.py:1141`) |
| `ChamadoColaborador.celula` | FK anulável da célula de origem. O nascimento obtém a célula pelo colaborador/dia do fato; `apontar_celula` preenche somente ponteiro ainda NULL. O juiz lê o vínculo **atual**, exigindo também que o chamado pertença ao colaborador consultado. (`app/chamados/models.py:48`, `app/chamados/models.py:1090`, `app/chamados/models.py:1119`, `app/chamados/models.py:1191`, `app/chamados/services/vinculos.py:92`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1192`) |
| `CelulaDia.colaborador`, `data` | FK da pessoa e `DateField`. Geração, regeneração e edição humana criam pela pessoa e pelo dia percorrido/recebido. O juiz consulta IDs/datas em lote e depois confere o **par exato**, eliminando os cruzamentos extras do `__in`. (`app/escala/models.py:1681`, `app/escala/models.py:1685`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:849`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1167`) |

Os instantes seguem a configuração `USE_TZ=True`, `America/Sao_Paulo`; o juiz não converte fuso nem escolhe o dia: recebe uma data pronta. A janela usa o corte empresarial, inclui início/fim e adota 21 quando falta Empresa, limitado a 2–28. Configuração efetiva de produção: [nao verificado]. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/juizes.py:991`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`)

| Chave/parâmetro | Escritor → leitor; significado |
|---|---|
| `('empresa', id)` | `dias_trancados` grava/lê a Empresa, inclusive `None`; alimenta o corte da janela. (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:991`) |
| `('tranca', emp, mes, ano)` | O mesmo corpo grava/lê `PeriodoFechado` ou `None`; mês/ano vêm do **fim da competência**, não do mês civil do dia. (`app/chamados/juizes.py:992`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:997`) |
| `('toque', colab, dia, desde)` | `_admin_tocou_os_dias_depois` lê a chave e grava somente `True`; distingue pessoa, data e instante de tranca. (`app/chamados/juizes.py:1100`, `app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1116`) |
| `('sistema',)` | O helper obtém o PK de sistema e o distribui pelos memos pendentes; a consulta da trilha o exclui. (`app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1112`, `app/chamados/juizes.py:1142`) |
| `_tarefa_pura(trancado=)` | A carga fornece pertencimento a `self._trancado`; significa **competência trancada e ausência de toque humano posterior**, não apenas existência de `PeriodoFechado`. O corpo puro somente consome esse booleano. (`app/chamados/services/tarefa_do_dia.py:168`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`) |

**Não é correto afirmar que todo `data_evento` significa dia do turno.** Perguntas do fato exigem `DiaDoTurno`; conversas gravam o dia civil da criação. A tarefa seleciona `conversa=False`, enquanto C6/wrapper da tranca não fazem essa distinção. É uma dualidade anterior ao diff, mas o novo docstring generaliza demais ao chamar todo `data_evento` de dia do turno. (`app/chamados/models.py:2477`, `app/chamados/models.py:2494`, `app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/juizes.py:930`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`)

**2) Lote versus avulso**

O avulso novo é literalmente lote de um. Repetições preservam posições; empresas/competências possuem chaves distintas; células e chamados são reconduzidos ao par correto. Com entradas coerentes e estado estável durante a leitura, essas estruturas preservam o predicado. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:962`, `app/chamados/juizes.py:992`, `app/chamados/juizes.py:1167`, `app/chamados/juizes.py:1192`)

O menor `desde` está correto: a consulta traz um superconjunto, e **cada etapa** reaplica `> desde` do item. Um registro exatamente no instante da tranca não conta. (`app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`)

A equivalência **para todo objeto aceito** falha nestas bordas:

- Dois colaboradores com mesmo `empresa_id`, memos separados e objetos Empresa com cortes 10/21: o lote reaproveita a primeira Empresa para ambos; chamadas separadas usam suas respectivas Empresas. Com dia 15/11 e tranca somente em novembro, reproduzi em memória **lote `[False, False]`; separados `[False, True]`**. Ocorrência em produção: [nao verificado]. (`app/chamados/juizes.py:975`, `app/chamados/juizes.py:979`, `app/chamados/juizes.py:990`, `app/ponto/janelas.py:24`)
- `SimpleNamespace(empresa_id=E, empresa=None)`: o corpo antigo anexado usava `None`, portanto corte default 21; o novo interpreta como Empresa faltante e busca E, podendo mudar competência e resultado. A comparação histórica depende do diff fornecido; execução de `e2dc08e2`: [nao verificado]. (`app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:991`, `app/ponto/janelas.py:50`)
- NS sem atributo `empresa` agora pode ser atendido pela busca; Empresa inexistente é omitida por `in_bulk`, vira `None` e segue pela janela default. Isso substitui o acesso direto anterior ao atributo/FK, inclusive seus possíveis erros. Execução ORM dessas bordas: [nao verificado]. (`app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`)

Os selos novos não cobrem essas bordas: usam uma Empresa, uma competência e colaboradores reais; o NS envolve pergunta/disputa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1146`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1186`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1198`)

**3) Quantidade e ordem de queries**

Para um colaborador válido, sistema existente e caches equivalentes, a sequência nominal permanece: Empresa → período → sistema → cobrar → células → resolver → chamados → desarquivar, interrompendo quando possível. Cada etapa necessária corresponde a uma consulta; o caminho frio completo chega a oito. A comparação com a base é estática pelo diff; deltas executados: [nao verificado]. (`app/chamados/juizes.py:984`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:1110`, `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1198`)

**Não são as mesmas queries SQL:** saíram `contains/exists` do cobrar; agora são carregadas descrição/data e a seleção ocorre em Python. O custo de linhas/memória não é certificado pela contagem de queries. (`app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1156`)

Há diferenças possíveis: NS com `empresa=None` ganha consulta de Empresa; o carregamento por `in_bulk` alimenta o memo, mas não aquece a FK `colab.empresa`, podendo deslocar uma consulta para um acesso posterior. (`app/chamados/juizes.py:940`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`)

`test_snc2` conta somente queries em `core_logauditoria` e `auth_user`; exige o delta de quatro consultas de trilha e igualdade do custo de sistema. **Não verifica total nem ordem de todas as queries.** (`app/chamados/tests/test_snc2_toque_em_lote.py:99`, `app/chamados/tests/test_snc2_toque_em_lote.py:107`, `app/chamados/tests/test_snc2_toque_em_lote.py:109`)

**4) Estado entre chamadas**

Empresa/tranca continuam com `lote=False`, no request ou colaborador; toque/sistema podem usar `LOTE_DA_TRANCA`. Os contextos são restaurados em `finally`. `da_vez` é local, mas seus objetos são copiados para os memos existentes, agora compartilhando referências entre colaboradores. (`app/chamados/juizes.py:970`, `app/chamados/juizes.py:971`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:996`, `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1055`, `app/core/middleware.py:78`, `app/core/middleware.py:82`)

**Não há NAO de toque gravado no `_tranca_memo`.** Só `True` entra nessa chave. (`app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1115`, `app/chamados/juizes.py:1116`)

Há, porém, **estado novo na carga**: `self._trancado` conserva o resultado durante sua vida. Reutilizar a mesma carga após toque/reabertura mantém o resultado anterior; `.tarefa()` não consulta novamente o juiz. Isso é uma fotografia da carga, distinta do memo de toque. (`app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`)

**5) Caminho da tarefa e posição do degrau**

No módulo inspecionado, a tranca entra por `dias_trancados`, vira `self._trancado` e chega ao corpo puro por booleano; não encontrei outro caminho direto de leitura da tranca. (`app/chamados/services/tarefa_do_dia.py:364`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`)

A ordem é `fora_vinculo → isento → trancado → sem_celula`. Portanto preserva as duas primeiras prioridades, mas muda casos existentes: **trancado sem célula passa de `sem_celula/nao_sei=True` para `competencia_trancada/nao_sei=False`**. Também antecede ata sem lâmpadas, teto, ausência e decisões. (`app/chamados/services/tarefa_do_dia.py:177`, `app/chamados/services/tarefa_do_dia.py:180`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:196`, `app/chamados/services/tarefa_do_dia.py:200`, `app/chamados/services/tarefa_do_dia.py:255`)

**6) Estado atual e horizonte**

Sim: o docstring declara expressamente tranca/toque como estado atual, sem corte por `agora`; a consulta compara a trilha somente com `fechado_em`. (`app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/services/tarefa_do_dia.py:306`, `app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`)

O selo de horizonte continua mordendo decisões, efeitos, retratação e passivo posteriores; não ficou vazio. Sua lista textual não acrescentou nominalmente a tranca, embora remeta ao docstring completo da carga; também não cobre a interação entre horizonte e tranca. Logo ele não certifica uma reconstrução histórica integral da tarefa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/tests/test_e2_tarefa_do_dia.py:714`, `app/chamados/tests/test_e2_tarefa_do_dia.py:732`, `app/chamados/tests/test_e2_tarefa_do_dia.py:750`, `app/chamados/tests/test_e2_tarefa_do_dia.py:785`)

**7) Classes de reprovação**

| Classe | Resultado |
|---|---|
| Segundo juiz/derivação paralela | Não apareceu um segundo corpo: C6 → avulso → lote; tarefa → lote. `_C20` está registrado e a frase da TRAVA existe. A equivalência universal exigida na declaração, contudo, tem as ressalvas do item 2. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`, `app/chamados/services/tarefa_do_dia.py:364`, `app/core/juizes.py:292`, `app/docs/CORTES.md:425`, `CLAUDE.md:276`) |
| Escrita pelo leitor | **Caminho condicional encontrado:** toque pendente chama `get_user_sistema`, cujo `get_or_create` cria usuário se faltar. O helper é anterior, mas a tarefa agora o alcança. Migration semeia sistema; existência em produção: [nao verificado]. (`app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1110`, `app/core/utils.py:18`, `app/core/migrations/0003_user_sistema.py:6`) |
| Relógio fora da borda | Os blocos novos recebem datas/instantes; não acrescentam leitura do relógio. A tarefa exige `agora` e confere compatibilidade da carga. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`, `app/chamados/services/tarefa_do_dia.py:416`, `app/chamados/services/tarefa_do_dia.py:420`) |
| Caso14 sem mordida | Tem respostas fixas para trancado, O44, cobrar/resolver, sistema/nulo/antes/outro e reabertura. Não é apenas comparação entre implementações. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1086`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1092`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1098`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1122`) |
| Selo14 sem mordida | Verifica resultado fixo e delegação por mock; cobre um universo homogêneo. Pré-cria sistema, portanto não morde a escrita condicional acima. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1156`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1201`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1203`) |
| Caso10PelaForma sem mordida | Detecta seus mutantes explícitos, mas é parcial: `getattr(c, 'via_encerramento')` e `filter(**{'via_encerramento': ...})` escapam; presença do import basta para sua exigência de “perguntar ao juiz”. Esses escapes não foram encontrados na tarefa real. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1225`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1235`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1250`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1255`, `app/chamados/services/tarefa_do_dia.py:364`) |
| Ordem de trava | Os blocos alterados não acrescentam aquisição explícita de trava; permanece a ressalva do possível `get_or_create`. (`app/chamados/juizes.py:961`, `app/chamados/juizes.py:1094`, `app/chamados/services/tarefa_do_dia.py:362`, `app/core/utils.py:18`) |
| Pouso antes do pré-requisito/RED anterior | **[nao verificado]**. | [nao verificado]
| Campo com dois significados | `data_evento` distingue fato/turno e conversa/dia civil; o diff não cria essa dualidade, mas seu docstring não a ressalva. (`app/chamados/models.py:2477`, `app/chamados/models.py:2494`, `app/chamados/juizes.py:930`) |

**8) Zero chamador — L-111**

Não encontrei sítio tocado que tenha ficado sem chamador de produção: `_empresa_ja_carregada`, os dois helpers de toque, `dias_trancados`, `_tarefa_pura` e a fábrica da carga possuem chamadas. O avulso conserva C6, models, reconciliador e emissor. (`app/chamados/juizes.py:977`, `app/chamados/juizes.py:1003`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:932`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:399`, `app/chamados/services/tarefa_do_dia.py:409`, `app/chamados/juizes.py:1261`, `app/chamados/models.py:1798`, `app/chamados/reconciliador.py:657`, `app/chamados/reconciliador.py:737`, `app/chamados/services/disputa_emissao.py:1691`)

## 07/11 15:44 · sessao 2159758aa197 · - · - · origem=Ronald (tmux)

Q2 — **equivalência universal não demonstrada e falsa para alguns objetos aceitos antes**:

- O avulso novo é literalmente `dias_trancados([(colab, data_evento)])[0]`; não há segunda implementação entre as formas novas. `app/chamados/juizes.py:932`.
- Repetições preservam índices; empresas e competências separam as chaves; o produto cartesiano da consulta de células é filtrado novamente pelo par exato; chamados são reconduzidos ao colaborador e célula exatos. `app/chamados/juizes.py:962`, `app/chamados/juizes.py:992`, `app/chamados/juizes.py:1162`, `app/chamados/juizes.py:1167`, `app/chamados/juizes.py:1192`.
- Cortes `desde` diferentes estão corretamente tratados para datetimes comparáveis: buscar `> min(desde)` traz um superconjunto e cada etapa verifica novamente `> desde` do item; igualdade não toca. `app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`.
- **Contraexemplo de compatibilidade antiga:** `SimpleNamespace(pk=1, empresa_id=E, empresa=None)`, empresa E real com corte 10, dia 15/11. Antes, conforme diff fornecido, `memo[empresa]=colab.empresa=None` e janela default 21 ⇒ competência 11. Agora `None` vira “empresa faltante”, `in_bulk` carrega E ⇒ competência 12. Havendo tranca somente em 11, resultados divergem. Caminho novo: `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:991`; default e competências: `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:50`. Reprodução em PROD [nao verificado].
- Dois colaboradores/NS com mesmo `empresa_id`, memos separados, objetos `empresa` com cortes distintos: o primeiro objeto entra em `da_vez`, o segundo é ignorado e recebe o primeiro; avulsos isolados usariam seus objetos próprios. `app/chamados/juizes.py:975`, `app/chamados/juizes.py:979`, `app/chamados/juizes.py:990`.
- Empresa inexistente: agora `in_bulk` não fornece objeto, grava `None`, usa default 21 e consulta período; o antigo acesso direto à FK poderia lançar `DoesNotExist`. NS sem atributo `empresa` antes poderia lançar `AttributeError`; agora pode carregar Empresa. `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`; comportamento ORM executado [nao verificado].
- Os testes de equivalência novos usam uma empresa, três dias consecutivos da mesma competência e cortes iguais; NS envolve a pergunta/disputa, **o colaborador continua modelo real**. Não cobrem os contraexemplos acima, empresas/competências múltiplas ou `desde` diferentes. `app/chamados/tests/test_e2_tarefa_do_dia.py:1147`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1186`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1198`.

Q3 — **contagem/ordem de etapas nominal preservada para um modelo válido e sistema existente; SQL não é idêntico**:

| Etapa | Queries por singleton quando necessária |
|---|---:|
| Empresa ausente do memo e FK não carregada | 1 |
| Período ausente do memo | 1 |
| Sistema ausente do memo | 1 SELECT se usuário existe |
| Cobrar | 1 |
| Células, se Cobrar não tocou | 1 |
| Resolver, se há célula | 1 |
| Chamados, se Resolver não tocou | 1 |
| Desarquivar, se há chamado elegível | 1 |

Fontes: `app/chamados/juizes.py:984`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:1110`, `app/core/utils.py:18`, `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1198`.

- Sem tranca, sem `desde` ou com SIM de toque memorizado, respectivos estágios cessam antes. `app/chamados/juizes.py:998`, `app/chamados/juizes.py:1097`, `app/chamados/juizes.py:1101`.
- Há mudança de SQL: antigo `descricao__contains + exists` passa a carregar descrição/instante e conferir em Python. `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1156`; comparação com base somente pelo diff fornecido [nao verificado].
- `test_snc2` conta **somente `core_logauditoria` e `auth_user`**, cobra `lg5-lg1=4`, igualdade de auth entre 1 e 5 e vantagem sobre avulso. Não confere total nem ordem de todas as queries. `app/chamados/tests/test_snc2_toque_em_lote.py:99`, `app/chamados/tests/test_snc2_toque_em_lote.py:107`, `app/chamados/tests/test_snc2_toque_em_lote.py:109`, `app/chamados/tests/test_snc2_toque_em_lote.py:111`.
- Exceção concreta a “mesmas queries”: NS com `empresa=None` antes não consultava Empresa, agora consulta. `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`.
- FK não carregada: novo `in_bulk` alimenta memo, mas não atribui `colab.empresa`; acesso posterior a essa FK pode fazer query que o antigo descriptor já teria aquecido. `app/chamados/juizes.py:940`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`. Execução [nao verificado].
- Avaliação do wrapper mudou de ordem: novo obtém disputa/colaborador antes de `data_evento`; antigo diff obtinha data primeiro. Se esses campos estiverem deferred/lazy, a ordem pode mudar. `app/chamados/juizes.py:932`; base executada [nao verificado].

Q4 — **`da_vez` local, sem NAO de toque no memo; existe novo snapshot da carga**:

- Empresa/tranca usam `lote=False`; request ou colaborador continuam donos. Toque/sistema podem usar `LOTE_DA_TRANCA`; reset é em `finally`. `app/chamados/juizes.py:971`, `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1028`, `app/chamados/juizes.py:1031`, `app/chamados/juizes.py:1055`.
- `da_vez` nasce local; valores são copiados aos memos existentes. O dicionário não vaza, mas referências Empresa/PeriodoFechado passam a ser compartilhadas entre memos de colaboradores distintos dentro da chamada. `app/chamados/juizes.py:970`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:996`.
- Somente `memo[k]=True` é escrito para toque; negativos ficam apenas no resultado transitório. `app/chamados/juizes.py:1094`, `app/chamados/juizes.py:1115`, `app/chamados/juizes.py:1116`.
- A tarefa agora conserva `self._trancado` durante a vida da carga, incluindo o resultado derivado “não houve toque”; chamadas subsequentes de `carga.tarefa` não consultam novamente o juiz. Reutilizar essa carga depois de toque/reabertura preserva o snapshot anterior. Isso não é NAO no `_tranca_memo`, mas é estado novo na instância da carga. `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`.
- O usuário de sistema pode ser criado pelo helper, apesar do docstring dizer que ausência lança erro; pré-existência em deploy é pressuposto, não prova fornecida. `app/core/utils.py:12`, `app/core/utils.py:15`, `app/core/utils.py:18`. O selo aquece esse usuário explicitamente. `app/chamados/tests/test_e2_tarefa_do_dia.py:1157`.

Q8 — **nenhum helper novo ficou sem chamador**:

- `_empresa_ja_carregada` → lote; `_admin_tocou_os_dias_depois` → lote; `_toque_humano_nos_dias` → admin; lote → avulso e carga. `app/chamados/juizes.py:977`, `app/chamados/juizes.py:1003`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:932`, `app/chamados/services/tarefa_do_dia.py:367`.
- Avulso mantém C6, models, reconciliador e emissor. `app/chamados/juizes.py:1261`, `app/chamados/models.py:1798`, `app/chamados/reconciliador.py:657`, `app/chamados/reconciliador.py:737`, `app/chamados/services/disputa_emissao.py:1691`.
- Nomes privados antigos desapareceram da implementação; a única ocorrência antiga encontrada foi a prosa histórica já dispensada pelo usuário. `app/chamados/tests/test_snc2_toque_em_lote.py:4`.

A cópia não possui `.git`; confirmação independente do hash/base e resultados de execução dos testes [nao verificado]. [nao verificado]

## 07/11 15:44 · sessao c5568a330d88 · - · - · origem=Ronald (tmux)

Q5–Q7, leitura estática:

- A tarefa consulta a tranca somente por `dias_trancados`, forma o conjunto `(colab.pk, dia)` e entrega seu pertencimento como `trancado` ao corpo puro. Não há leitura direta dos modelos/carimbos da tranca nesse módulo. `app/chamados/services/tarefa_do_dia.py:364`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`.
- A prioridade efetiva é `fora_vinculo` → isento → trancado → sem célula. Assim, tranca preserva as duas primeiras respostas, mas transforma `sem_celula/nao_sei=True` em `competencia_trancada/nao_sei=False`; também antecede ata sem lâmpadas, teto, ausência e decisão. Isso corresponde ao novo degrau proposto, com efeito inclusive em dia sem célula. `app/chamados/services/tarefa_do_dia.py:177`, `app/chamados/services/tarefa_do_dia.py:180`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:196`, `app/chamados/services/tarefa_do_dia.py:200`.
- A carga declara expressamente tranca e toque como estado atual, sem corte por `agora`; a chamada não passa `agora` e o juiz usa somente `fechado_em` e `criado_em > desde`. `app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/services/tarefa_do_dia.py:306`, `app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`.
- O selo de horizonte continua exercitando decisão, efeito, retratação e passivo posteriores; portanto não fica vazio. Porém não cobre interação dessas situações com a nova tranca, e sua lista nominal de limites não menciona tranca, embora remeta ao docstring completo da carga. `app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/tests/test_e2_tarefa_do_dia.py:714`, `app/chamados/tests/test_e2_tarefa_do_dia.py:732`, `app/chamados/tests/test_e2_tarefa_do_dia.py:750`, `app/chamados/tests/test_e2_tarefa_do_dia.py:785`.
- Não há segundo corpo para o predicado: avulso delega ao lote; C6 usa o avulso e tarefa usa o lote. Registro `_C20` existe e a frase exigida pela trava também. `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`, `app/chamados/services/tarefa_do_dia.py:364`, `app/core/juizes.py:292`, `app/docs/CORTES.md:425`. Conformidade com a condição histórica de autoridade anteriormente única exige a comparação fornecida e censo dos demais leitores; somente o registro não prova isso. `CLAUDE.md:273`.
- Há caminho condicional de escrita herdada agora alcançado pela tarefa: tranca com toque pendente chama `get_user_sistema()`, que usa `get_or_create`, apesar de seu docstring dizer que levantaria `DoesNotExist`. Se faltar o usuário, o leitor cria-o. Migration e command semeiam o usuário; existência real em produção [nao verificado]. `app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1110`, `app/core/utils.py:15`, `app/core/utils.py:18`, `app/core/migrations/0003_user_sistema.py:6`, `app/core/management/commands/garantir_user_sistema.py:12`.
- O selo de zero escritas não monta tranca; o Selo14 garante o usuário antes da captura. Logo esses selos não mordem a falta dessa precondição. `app/chamados/tests/test_e2_tarefa_do_dia.py:878`, `app/chamados/tests/test_e2_tarefa_do_dia.py:889`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1156`.
- Caso14 possui respostas fixas para trancado, O44, cobrar/resolver, sistema/nulo/antes/outro e reabertura; não é mera comparação entre duas implementações. `app/chamados/tests/test_e2_tarefa_do_dia.py:1086`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1092`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1098`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1122`.
- Selo14 verifica resultado fixo e delegação mediante mock, além de igualdade de contagens 1×1/2×3. Seu universo usa uma empresa e uma competência: não certifica empresas, competências ou cortes heterogêneos. `app/chamados/tests/test_e2_tarefa_do_dia.py:1146`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1178`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1201`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1203`.
- Caso10PelaForma morde seus sete mutantes explícitos, mas é parcial: reconhece `Name/Attribute/import/keyword`; não reconhece `getattr(c,'via_encerramento')` nem `qs.filter(**{'via_encerramento': ...})`, e exige apenas presença do import do juiz. Confirmei esses escapes executando somente o helper AST extraído em memória (`python3 -B`), sem importar a aplicação ou acessar banco. Nenhum desses caminhos foi encontrado na tarefa real. `app/chamados/tests/test_e2_tarefa_do_dia.py:1225`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1235`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1250`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1255`.
- Não há aquisição de trava nova nos blocos alterados do juiz/tarefa; ordem operacional de pouso, RED anterior e suíte verde não são demonstrados pelo material inspecionado. `app/chamados/juizes.py:961`, `app/chamados/juizes.py:1094`, `app/chamados/services/tarefa_do_dia.py:362`; execução/pouso [nao verificado].

## 07/11 15:43 · sessao 1a1b36c36a82 · - · - · origem=Ronald (tmux)

Evidências para Q1:

- **PeriodoFechado**: `fechado_em` e `reaberto_em` são instantes (`DateTimeField`), não dias; o primeiro nasce por `auto_now_add`, o segundo aceita NULL (`app/ponto/models.py:926`, `app/ponto/models.py:931`). `trancar_periodo` cria a linha e, ao retrancar uma reaberta, grava `timezone.now()` em `fechado_em` e limpa `reaberto_em`; `reabrir_periodo` grava `timezone.now()` em `reaberto_em` (`app/ponto/services/fechamento.py:1401`, `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1410`, `app/ponto/services/fechamento.py:1432`). O leitor `periodo_trancado` considera viva a linha com `reaberto_em IS NULL`, sem comparar instante de reabertura nem `ativo`; `competencia_trancada` usa o mesmo filtro (`app/ponto/services/fechamento.py:1457`, `app/ponto/services/fechamento.py:1463`, `app/ponto/services/fechamento.py:1473`). `dias_trancados` lê `fechado_em` como corte inferior exclusivo do toque (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`). Outro leitor usa o mesmo `>` para `resgatado_em` (`app/chamados/services/competencia_trancada.py:104`).

- **Fuso/unidades**: configuração da cópia é `America/Sao_Paulo`, `USE_TZ=True` (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`). A janela recebe `date`, faz competência pelo corte empresarial e inclui os dias do início e do fim; corte default 21, limitado a 2–28 (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`). Nenhuma conversão de fuso ocorre dentro de `dias_trancados`; ele recebe o dia já escolhido (`app/chamados/juizes.py:963`, `app/chamados/juizes.py:991`). A configuração efetivamente usada em produção é [nao verificado].

- **LogAuditoria**: `acao`/`modelo` são strings; `objeto_id` inteiro sem FK; `descricao` texto; `usuario` FK anulável; `criado_em` instante automático (`app/core/models.py:295`, `app/core/models.py:298`, `app/core/models.py:299`, `app/core/models.py:307`). A porta `registrar_log` encaminha para `evento`; este resolve o ator e cria a linha, copiando os argumentos (`app/core/auditoria.py:8`, `app/core/auditoria.py:22`, `app/core/observ.py:108`, `app/core/observ.py:118`, `app/core/observ.py:134`). A escolha `acao` não restringe os verbos gravados por `create`, reconhecido no comentário do modelo (`app/core/models.py:296`).

- **Os três escritores da trilha**: cobrar grava `cobrar_dia_manual`, `Colaborador`, PK do colaborador e `dia=%s` com o `data` recebido, antes do cartório (`app/chamados/services/cobrar_dia.py:37`). Resolver grava `veredito_celula`, `CelulaDia`, PK da célula ou 0 sem célula; a data textual não é usada pelo leitor desse ato (`app/ponto/services/veredito_celula.py:334`). Desarquivar chama `arquivo.levantar`, que grava a trilha somente quando houve mudança; `_trilha` usa `ChamadoColaborador`, PK do chamado e ator passado (`app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:193`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:137`).

- **O leitor da trilha** reconhece exatamente esses três pares ação/modelo; cobrar compara substring `dia=` + `data.isoformat()`, resolver associa o ID à célula atual do par, desarquivar associa o ID ao chamado atual do mesmo colaborador e célula (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1155`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1199`). Humano significa somente usuário não NULL e diferente do usuário sistema; não há checagem de papel ADMIN nesse leitor (`app/chamados/juizes.py:1141`). `get_user_sistema` usa `get_or_create`, podendo escrever se o usuário não existir; isso já integra o caminho antigo informado no diff (`app/core/utils.py:18`).

- **ChamadoColaborador.celula** é FK anulável para a célula que originou o chamado (`app/chamados/models.py:46`). No nascimento, `_celula_de` deriva o dia via `dia_do_fato` e busca por colaborador/data; `abrir`/`criar` passam essa célula à criação (`app/chamados/models.py:1191`, `app/chamados/models.py:1201`, `app/chamados/models.py:1207`, `app/chamados/models.py:1090`, `app/chamados/models.py:1119`). Para chamado existente, a porta `apontar_celula` escreve somente se o ponteiro ainda é NULL (`app/chamados/services/vinculos.py:92`). O novo leitor verifica também `ChamadoColaborador.colaborador_id`; uma célula apontada por chamado de outro colaborador não basta (`app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1192`).

- **CelulaDia.colaborador/data** são FK e `DateField`, unidade pessoa/dia (`app/escala/models.py:1681`, `app/escala/models.py:1685`). Os escritores de geração/regeneração e edição humana criam com o colaborador do vínculo e o dia recebido/percorrrido (`app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:849`). O leitor em lote consulta produto de IDs/datas, mas filtra novamente o par exato em Python (`app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1167`).

- **Atenção ao contrato de `data_evento`**: pergunta datada exige `DiaDoTurno`; conversa grava o dia civil da criação por `localdate()` (`app/chamados/models.py:2470`, `app/chamados/models.py:2477`, `app/chamados/models.py:2487`, `app/chamados/models.py:2494`). `DiaDoTurno` é `NewType` de `date`, sem distinção em runtime (`app/core/dia_do_turno.py:13`). C6 e wrapper da tranca não filtram `conversa`, portanto a afirmação “todo data_evento é dia do turno” não vale universalmente; essa dualidade antecede o diff (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:1256`).

Contraexemplos para equivalência universal com o corpo antigo anexado:

- `SimpleNamespace(empresa_id=id_real, empresa=None)` antes passava None à janela, usando corte 21; agora busca a Empresa e pode usar outro corte. NS sem `empresa` antes levantava AttributeError, agora busca (`app/chamados/juizes.py:936`, `app/chamados/juizes.py:982`, `app/ponto/janelas.py:50`; corpo antigo: anexo `e2_juiz.diff` [nao verificado para linha local]).
- FK com empresa inexistente antes acessava `colab.empresa`; agora `in_bulk` pode omitir o ID, grava None e segue com corte default, devolvendo False se não houver período (`app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`, `app/ponto/janelas.py:50`, `app/ponto/services/fechamento.py:1463`). A ocorrência desses objetos em produção é [nao verificado].

## 07/11 15:24 · sessao c9a5bc337859 · - · - · origem=Ronald (tmux)

[prova/1_viva.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/1_viva.out:1)
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

[prova/2_tmp.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2_tmp.out:1)
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

[prova/2b_tmpdir.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2b_tmpdir.out:1)
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

[prova/3_git.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3_git.out:1)
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

[prova/3b_commit.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3b_commit.out:1)
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-152358/index.lock': Read-only file system
rc=128
```

[prova/4_raiz.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/4_raiz.out:1)
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-152358/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

[prova/5_home.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/5_home.out:1)
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

[prova/6_docker.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6_docker.out:1)
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

[prova/6b_sock.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6b_sock.out:1)
```text
rc=7
```

[prova/7_tcp_local.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7_tcp_local.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/46597: Operation not permitted
rc=1
```

[prova/7c_unix.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7c_unix.out:1)
```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
PermissionError: [Errno 1] Operation not permitted
rc=1
```

[prova/7b_tcp_ext.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7b_tcp_ext.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

[prova/8_sobrevive.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/8_sobrevive.out:1)
```text
lancado
rc=0
```

## 07/11 14:57 · sessao 512712426e5e · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura continua parcial. Permanecem PROCEDENTES as classes (1), (2) e (4); a classe (3) melhorou, mas não cumpre integralmente a conferência por item sob trava. A classe (5) foi corrigida.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2665`; `app/chamados/services/fio.py:1120`)

Confirmação independente do hash/pai, RED→GREEN histórico, suíte inteira `rc=0`, bloqueios executados no banco e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: ainda há recorte anterior à canônica.**

`aplicar` escolhe sobre `_no_fato(fam)`, que já filtrou o horizonte. Entretanto, a identidade BATIDA é `(batida_origem_id, motivo)`, sem dia, e `do_fato` consulta o episódio inteiro sem esse corte. Portanto as portas continuam podendo entregar canônicas diferentes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/services/fio.py:1055`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:2562`)

Contraexemplo de código, no acervo anterior à implantação do índice: mesmo episódio/motivo, `p1` respondida abaixo do horizonte e `p2/p3` mudas acima dele. `abrir` e a migração escolhem `p1`; backfill/dedup escolhem `p2` e retratam `p3`. A regra de preferência é a mesma; o universo entregue a ela diverge. Reprodução em banco: **[nao verificado]**. (`app/chamados/models.py:2683`; `app/chamados/models.py:2684`; `app/chamados/services/fio.py:984`; `app/chamados/services/fio.py:1196`; `app/chamados/services/fio.py:1203`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

Existe ainda diferença sem atravessar o horizonte: `do_fato` incorpora, pelo OR, a pergunta legada **sem episódio** do mesmo dia; o agrupamento BATIDA usado pelo backfill restringe-se à chave do episódio. Essa linha legada também pode ser a canônica excluída. (`app/chamados/models.py:2560`; `app/chamados/models.py:2563`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`)

**NAO PROCEDE a permanência do contraexemplo antigo das reutilizáveis:** agora a declaração escolhe primeiro sobre o fato inteiro e somente depois confere destino/reutilização. (`app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:246`; `app/chamados/services/declaracao_texto.py:267`)

**2. Classe (2) — PROCEDENTE pelas travas implícitas e pela ordem posterior.**

**NAO PROCEDE aquisição explícita adicional localizada** das quatro entidades por `select_for_update`: as quatro consultas estão em `trava_na_ordem`, na ordem declarada. A busca fora de `app` encontrou detector e fixtures sintéticas, não outra aquisição de produção; construção dinâmica sem os nomes pesquisados permanece **[nao verificado]**. (`app/chamados/models.py:2508`; `app/chamados/models.py:2515`; `app/chamados/models.py:2528`; `app/chamados/models.py:2532`; `bin/atomicidade_da_porta.py:63`; `bin/tests/test_atomicidade_da_porta.sh:53`)

Persistem dois caminhos concretos:

- **Chamado → Disputa por S143.** `fechar → transicionar → _aplicar → _gravar_estado` pode entrar sem `_recarregar_estado_se_mudou`. O escritor trava somente Colaborador, salva Chamado e dispara S143; o resolver adquire depois as Disputas abertas. Um chamado sem impedimento próprio, mas com outra Disputa aberta da mesma pessoa, alcança essa sequência. (`app/chamados/models.py:447`; `app/chamados/models.py:199`; `app/chamados/models.py:442`; `app/chamados/models.py:286`; `app/chamados/models.py:291`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/juizes.py:2083`)
- **Pergunta → Disputa na ausência rejeitada.** O atomic chama `reabrir_para_correcao`, que faz UPDATE de Pergunta; depois chama `reabrir_sistema`, que salva Disputa. Não há antecipação dessas travas nesse envelope. (`app/chamados/services/materializacao.py:672`; `app/chamados/services/materializacao.py:673`; `app/chamados/services/disputa_emissao.py:1516`; `app/chamados/models.py:2973`; `app/chamados/services/materializacao.py:677`; `app/chamados/models.py:2130`)

O sítio antigo de `_recarregar_estado_se_mudou` foi corrigido, mas essa correção não cobre todas as entradas no escritor de estado. S88/S143/S144 continuam executando ações síncronas durante os saves. (`app/chamados/models.py:689`; `app/chamados/models.py:442`; `app/chamados/signals.py:42`; `app/chamados/signals.py:92`; `app/chamados/signals.py:140`)

**3. Classe (3) — PROCEDENTE, parcial; NAO PROCEDE mistura de escrita e recusa no retorno normal.**

Mérito é relido dentro do atomic, depois da trava. Exportação também é consultada nesse envelope; contudo `_ent` fica fora do laço e reaproveita a resposta entre colaboradores da mesma competência, sem nova consulta após a trava de cada um. O congelamento contra entrega concorrente não está demonstrado por essas travas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:194`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:200`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:219`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:238`; `app/folha/export.py:581`)

A divergência do horizonte por empresa é conferida **antes** do atomic. Para `sem_ato`, há veto explícito sob trava; para grupos, o horizonte funciona como filtro que pode esvaziar o universo e provocar `continue`, sem registrar `fora_do_horizonte`. Isso não implementa integralmente o veto anunciado por item. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:216`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:225`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:229`)

**NAO PROCEDE o defeito anterior de escrita persistida acompanhada de recusa:** há atomic externo e a exceção é levantada dentro dele; o retorno de recusa ocorre após rollback e zera os contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:264`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:266`)

`handle` faz mais que chamar `aplicar`: prepara/imprime o DRY e traduz recusas em saída. **NAO PROCEDE outra escrita do acervo nele ou no dedup:** os dois encaminham o apply à mesma função; as alterações de carimbo continuam delegadas a `retratar_duplicata`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:296`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:371`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:384`; `app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

**4. Classe (4) — PROCEDENTE: a declaração mudou, mas leitores permanecem incompatíveis.**

O significado declarado é **dia do turno com `conversa=False`**; conversa conserva o **dia civil da emissão**. O novo `q_do_horizonte` incorpora o discriminador. (`app/chamados/services/fio.py:1031`; `app/chamados/services/fio.py:1036`; `app/chamados/models.py:2637`)

O próprio nascimento ainda entrega o dia civil da conversa a `resolvedora_valida`, usando proxy sem `conversa`. Esse juiz presume conversa apenas quando a data é nula e compara a data recebida com o turno do chamado. Conversa emitida hoje para chamado de dois dias antes pode ser recusada como costura cruzada. (`app/chamados/models.py:2661`; `app/chamados/models.py:2665`; `app/chamados/models.py:2666`; `app/chamados/models.py:2668`; `app/chamados/juizes.py:1966`; `app/chamados/juizes.py:1968`; `app/chamados/juizes.py:1985`; `app/chamados/juizes.py:2001`)

Outros leitores sem `conversa=False`, `do_fato` ou `q_do_horizonte`:

| Leitor | Uso incompatível |
|---|---|
| Juiz do dia do chamado | Incorpora datas de todas as perguntas ao dia inferido da Disputa. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:209`) |
| Prévia da folha | Agrupa pela data civil e apresenta a resposta como material do dia da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:67`; `app/folha/previa.py:95`) |
| Copiloto do dia | Inclui conversas civis nas perguntas daquele dia. (`app/ponto/services/dia_do_colab.py:64`; `app/ponto/services/dia_do_colab.py:71`; `app/ponto/services/dia_do_colab.py:99`) |
| Diagnóstico do fio | Compara `data_evento` ao dia do fato sem distinguir conversa. (`app/chamados/services/fio.py:86`; `app/chamados/services/fio.py:88`) |

**5. Classe (5) — NAO PROCEDE no contador atual.**

“Nova” passou a usar `data_evento` da Pergunta contra o dia local da aplicação da 0052. Registro ausente ou dia nulo alarma; o escape de pergunta posterior sobre turno anterior está declarado. O RED fixa a aplicação da migração e exige que pergunta nova em Disputa velha conte como nova. (`app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1115`; `app/chamados/services/fio.py:1119`; `app/chamados/services/fio.py:1120`; `app/chamados/tests/test_e1_v6_horizonte.py:228`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência generalizada de mordida.**

A/B varrem Python sob `app`, com exclusões; C examina somente os dois commands. Os esperados são zero, sem allowlist explícita. Isso não equivale a cobrir semanticamente toda a classe no repositório. (`app/chamados/tests/test_e1_v7_catracas.py:26`; `app/chamados/tests/test_e1_v7_catracas.py:30`; `app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:176`; `app/chamados/tests/test_e1_v7_catracas.py:218`; `app/chamados/tests/test_e1_v7_catracas.py:268`)

| Catraca | Formas que escapam ao detector |
|---|---|
| A — trava | `getattr(..., "select_for_update")`, `get_for_update`, SQL cru; importação do módulo como `from chamados import models as m`. Alias direto do modelo e queryset em variável são detectados nos casos cobertos. (`app/chamados/tests/test_e1_v7_catracas.py:65`; `app/chamados/tests/test_e1_v7_catracas.py:79`; `app/chamados/tests/test_e1_v7_catracas.py:191`) |
| B — canônica | Alias importado da função; `.filter/.exclude` no QuerySet; `qs = do_fato(...); qs.first()`; `min`/`sorted()[0]`. (`app/chamados/tests/test_e1_v7_catracas.py:19`; `app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:113`; `app/chamados/tests/test_e1_v7_catracas.py:135`) |
| C — escrita | Alias de escritor, `getattr(p, "save")()` e helper chamado fora dos dois arquivos examinados. (`app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:157`) |

As dispensas também usam apenas o nome da função: outra função chamada `trava_na_ordem`, `canonica_do_grupo` ou `aplicar` pode ocultar infração, sem comprovar arquivo/classe da autoridade. Logo zero allowlist nominal não garante sítio único. (`app/chamados/tests/test_e1_v7_catracas.py:83`; `app/chamados/tests/test_e1_v7_catracas.py:121`; `app/chamados/tests/test_e1_v7_catracas.py:157`)

Os mutantes mordem as formas antigas escolhidas. Os REDs também corrigem as vacuidades específicas: p1 validada/materializada vence p2 muda; D exige `count()==1`; o `TransactionTestCase` tenta NOWAIT em segunda conexão, com seis linhas bloqueadas e dois controles livres, mais variante sem flag. Execução desses casos no banco: **[nao verificado]**. (`app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`; `app/chamados/tests/test_e1_v6_horizonte.py:405`; `app/chamados/tests/test_e1_v6_horizonte.py:454`; `app/chamados/tests/test_e1_v5_ordem_trava.py:481`; `app/chamados/tests/test_e1_v5_ordem_trava.py:504`; `app/chamados/tests/test_e1_v5_ordem_trava.py:515`)

Permanece uma limitação distinta: o observador de ordem registra somente a primeira aquisição de cada **tabela**, ignorando outra PK da mesma tabela adquirida posteriormente. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)

**7. Escritores, leitores e significados — PROCEDENTE a incompatibilidade de `data_evento`; NAO PROCEDE uniformidade total.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| `via_resolucao` | Carimbo por `_escrever_carimbo`, pelas portas normal/identidade/descarimbo; lote tem UPDATE próprio. Exceções declaradas: `relavrar` e `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2810`; `app/chamados/models.py:2865`; `app/chamados/models.py:2902`; `app/chamados/models.py:2973`; `app/chamados/services/relavra_via.py:123`; `app/chamados/models.py:2983`) | C6, `q_vivas`, identidade do fato, backfill e materializador. Representa via de encerramento/identidade, não necessariamente plantio. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3058`; `app/chamados/models.py:2564`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/services/materializacao.py:438`) |
| `materializada_em` | Mesmas portas do carimbo. Duplicata preserva instante/autor anteriores quando já existem. (`app/chamados/models.py:2812`; `app/chamados/models.py:2894`; `app/chamados/models.py:2974`) | Vitalidade, elegibilidade do materializador, identificação da absorção e instante do SILENCIO legado. Não significa sempre “batida criada”, nem sempre instante da retratação; o help do campo continua mais estreito. (`app/chamados/juizes.py:1210`; `app/chamados/services/materializacao.py:1363`; `app/chamados/services/materializacao.py:624`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/models.py:3103`) |
| `duplicata` / motivo retratado | `retratar_duplicata` e `remover_motivo`, pela porta de identidade. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2014`) | `VIAS_FORA_DO_FATO`, C6, índices e guardas de descarimbo: terminalidade da identidade; resposta permanece histórica. (`app/chamados/models.py:2364`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2918`; `app/chamados/models.py:3193`) |
| `admin_decidiu` | `fechar_por_admin`, pela porta de identidade. Backfill acrescenta SILENCIO à sequência; não grava novamente essa via. (`app/chamados/models.py:2777`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`) | C6 e backfill distinguem decisão humana; mérito perdedor é essa via com resposta. (`app/chamados/juizes.py:1173`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`) |
| `data_evento` | Nascimento; preenchimento de órfã no reapontamento; rechaveamento. (`app/chamados/models.py:2731`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade, horizonte, competência, materialização e leitores do dia. A incompatibilidade turno/conversa permanece nos consumidores do item 4. (`app/chamados/models.py:2562`; `app/chamados/services/fio.py:1036`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:118`; `app/chamados/services/materializacao.py:345`; `app/chamados/juizes.py:1966`) |
| `status_local` | `_gravar_estado` atribui/grava; nascimento define estado inicial. (`app/chamados/models.py:228`; `app/chamados/models.py:291`; `app/chamados/models.py:1048`) | `_recarregar_estado_se_mudou`, motor e juízes leem estado persistido do Chamado. **NAO PROCEDE mudança de significado causada pelo diff**; o problema restante é ordem de aquisição. (`app/chamados/models.py:690`; `app/chamados/models.py:692`; `app/chamados/catalogo/motor.py:24`; `app/chamados/models.py:257`) |

Há ainda documentação contraditória: `carimbar_identidade` diz que duplicata respondida pode continuar viva, mas o C6 atual mata `VIAS_QUE_VENCEM_A_RESPOSTA` antes de examinar a resposta. (`app/chamados/models.py:2885`; `app/chamados/models.py:2887`; `app/chamados/juizes.py:1148`; `app/chamados/juizes.py:1153`)

**8. Reincidências e sete TETOS.**

**PROCEDENTES:** seleção sobre universo paralelo, ordem de trava, campo com dois domínios e relógio fora da borda nos testes. As fixtures ainda criam Disputa com `auto_now_add`, chamam fechamento que usa `now()` sem injeção e usam `localdate()/now()` no selo de desempenho. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2637`; `app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1710`; `app/chamados/tests/test_e1_v6_horizonte.py:174`; `app/chamados/models.py:2777`; `app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/tests/test_selo_chokepoint_escrita.py:262`)

**NAO PROCEDE segundo escritor do backfill demonstrado nesta volta; NAO PROCEDE selo inteiramente sem mordida.** Persistem insuficiências de cobertura descritas acima. (`app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`)

**NAO PROCEDE subida sem frase C8:** os sete números têm justificativa ao lado. (`PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

| Cenário | Teto / aumento | Fonte |
|---|---:|---|
| `_gravar_estado` sem pergunta | 25 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 57 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Reconciliar recusa | 14 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:119` |
| Uma cobrança fecha | 204 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 254 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Uma cobrança fica | 142 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:180` |
| Três cobranças ficam | 211 / +3 | `app/ponto/tests/test_selo_chokepoint_escrita.py:187` |

A decomposição é compatível com quantidade fixa de consultas **por entrada** na trava, não com tempo/contenção constantes: cresce o universo de linhas bloqueadas. O último +3 admite decomposição não medida, apesar da justificativa geral de +2 por entrada. Medições citadas nos logs e causalidade exata desse delta: **[nao verificado]**. (`app/chamados/models.py:2513`; `app/chamados/models.py:2515`; `app/chamados/models.py:2523`; `app/chamados/models.py:2528`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

**9. Efeito colateral — NAO PROCEDE apagamento físico; PROCEDENTE perda de efetividade sem veto de mérito.**

A retratação não apaga resposta, validação ou `DecisaoPergunta`: altera via/carimbo/autor/recusa. Porém tira a linha da identidade e pode matar sua resposta pendente antes da validação. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2894`; `app/chamados/models.py:2898`; `app/chamados/models.py:2902`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`)

Contraexemplo abaixo do horizonte: `p1` respondida/validada fora do destino; `x=p2` respondida com `admin_decidiu`, sem validação nem célula, no destino; `p3` muda viva no chamado velho. `x` pode estar viva pela resposta. A migração escolhe `p1`, retrata `x` e `p3` e não migra nenhuma delas para o destino. A resposta de `x` perde sua pendência efetiva; a decisão fica histórica. (`app/chamados/juizes.py:1153`; `app/chamados/juizes.py:1165`; `app/chamados/services/fio.py:1181`; `

[truncado em 20000 de 20638 caracteres]

## 07/11 14:55 · sessao c4396d29c20e · - · - · origem=Ronald (tmux)

Busca complementada fora de `app`, incluindo raiz, `bin` e scripts: **NAO PROCEDE aquisição explícita adicional localizada**; o resultado de produção encontrado é um detector AST, não uma trava. (`bin/atomicidade_da_porta.py:63`)

Os `select_for_update` restantes fora de `app` aparecem em fontes sintéticas de teste sobre `Linha`. (`bin/tests/test_atomicidade_da_porta.sh:53`; `bin/tests/test_atomicidade_da_porta.sh:70`; `bin/tests/test_atomicidade_da_porta.sh:75`; `bin/tests/test_atomicidade_da_porta.sh:80`)

Não surgiram candidatos adicionais por `get_for_update`, `getattr`, SQL `FOR UPDATE`/`FOR SHARE` ou `LOCK TABLE`; ausência por construção dinâmica que não contenha esses nomes: **[nao verificado]**.

## 07/11 14:54 · sessao 675064e115a1 · - · - · origem=Ronald (tmux)

Itens 6/8, somente leitura:

- **PROCEDENTE — catracas incompletas.** A/B varrem somente Python sob `app`, excluindo diretórios e arquivos; C limita-se aos dois commands. Não cobrem literalmente todo o repositório. (`app/chamados/tests/test_e1_v7_catracas.py:26`; `app/chamados/tests/test_e1_v7_catracas.py:30`; `app/chamados/tests/test_e1_v7_catracas.py:144`)
- **NAO PROCEDE — ausência de mordida nas formas antigas.** Executei os 16 métodos dessas três classes em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 16 passaram; os detectores retornaram A/B/C vazios para 749 fontes. Essa execução não constitui suite Django, banco nem RED→GREEN histórico. (`app/chamados/tests/test_e1_v7_catracas.py:165`; `app/chamados/tests/test_e1_v7_catracas.py:209`; `app/chamados/tests/test_e1_v7_catracas.py:261`)
- **PROCEDENTE — burlas demonstradas em memória:** A deixa passar `getattr(...,"select_for_update")`, `get_for_update`, SQL cru e `from chamados import models as m; m.PerguntaDisputa.objects.select_for_update()`. B deixa passar alias importado da canônica, `canonica_do_grupo(do_fato(...).filter(...))` e `qs=do_fato(...); qs.first()`. C deixa passar alias de `retratar_duplicata`, `getattr(p,"save")()` e helper externo. (`app/chamados/tests/test_e1_v7_catracas.py:79`; `app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:113`; `app/chamados/tests/test_e1_v7_catracas.py:135`; `app/chamados/tests/test_e1_v7_catracas.py:157`)
- **PROCEDENTE — exceções apenas por nome.** Qualquer função homônima `trava_na_ordem`, `canonica_do_grupo` ou `aplicar` recebe a dispensa, sem provar arquivo/classe da autoridade. Não há allowlist explícita, mas isso não certifica sítio único. (`app/chamados/tests/test_e1_v7_catracas.py:83`; `app/chamados/tests/test_e1_v7_catracas.py:121`; `app/chamados/tests/test_e1_v7_catracas.py:157`)

Os três REDs solicitados têm asserções concretas: p1 validada/materializada versus p2 muda e ordem reversa; D exige exatamente uma linha; teste real usa segunda thread/conexão, NOWAIT, seis linhas bloqueadas e dois controles livres, com variante sem flag. Resultado no banco: **[nao verificado]**. (`app/chamados/tests/test_e1_v6_horizonte.py:405`; `app/chamados/tests/test_e1_v6_horizonte.py:445`; `app/chamados/tests/test_e1_v5_ordem_trava.py:448`; `app/chamados/tests/test_e1_v5_ordem_trava.py:481`; `app/chamados/tests/test_e1_v5_ordem_trava.py:504`; `app/chamados/tests/test_e1_v5_ordem_trava.py:515`)

**PROCEDENTE — ordem ainda observada por tabela:** o rastreador ignora aquisições posteriores de outras PKs da mesma tabela; o novo teste de bloqueio não elimina essa limitação geral. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)

**PROCEDENTE — relógio fora da borda permanece.** Fixtures criam Disputa com `auto_now_add`; casos chamam `fechar_por_admin`, que usa `now()` sem injeção. A fixture dos tetos também usa `localdate()` e `now()`. (`app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1710`; `app/chamados/tests/test_e1_v6_horizonte.py:174`; `app/chamados/models.py:2777`; `app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/tests/test_selo_chokepoint_escrita.py:262`)

**NAO PROCEDE — subida calada dos sete tetos:** todos têm justificativa ao lado, como exige C8. Valores/deltas: 25/+2, 57/+2, 14/+2, 204/+4, 254/+8, 142/+2, 211/+3. (`PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:118`; `app/ponto/tests/test_selo_chokepoint_escrita.py:119`; `app/ponto/tests/test_selo_chokepoint_escrita.py:161`; `app/ponto/tests/test_selo_chokepoint_escrita.py:162`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

A implementação sustenta quantidade fixa de consultas por entrada, não tempo/contenção constantes: o universo de linhas bloqueadas cresce. O último +3 admite decomposição não medida, apesar da justificativa geral +2/entrada; causalidade exata desse delta e medições dos logs: **[nao verificado]**. (`app/chamados/models.py:2508`; `app/chamados/models.py:2513`; `app/chamados/models.py:2523`; `app/chamados/models.py:2528`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

## 07/11 14:54 · sessao ec76314e5c3c · - · - · origem=Ronald (tmux)

**1. PROCEDENTE — universo recortado antes da canônica permanece no backfill.** `aplicar` lê `_no_fato(fam)` e só então chama `canonica_do_grupo`; `_no_fato` já excluiu o passado pelo horizonte. A identidade BATIDA não inclui data, enquanto `do_fato` busca o episódio inteiro sem horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/services/fio.py:1055`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:2562`)

Contraexemplo deduzido do código: mesmo episódio/motivo BATIDA, `p1` respondida abaixo H, `p2/p3` mudas acima H; `abrir` e migração escolhem `p1`, mas backfill/dedup escolhem `p2` e retratam `p3`. O índice parcial não impede a linha abaixo H; a colisão acima H é justamente o acervo tratado antes do índice. Ocorrência e reprodução no banco: **[nao verificado]**. (`app/chamados/models.py:2683`; `app/chamados/models.py:2684`; `app/chamados/services/fio.py:1196`; `app/chamados/services/fio.py:1203`; `app/chamados/services/fio.py:984`; `app/chamados/models.py:3200`)

Outro universo diferente: `do_fato` inclui a linha antiga **sem episódio** do mesmo dia pelo OR; agrupamento/backfill BATIDA usa exclusivamente `(batida_origem_id,motivo)`, podendo excluir essa canônica mesmo sem atravessar H. (`app/chamados/models.py:2560`; `app/chamados/models.py:2563`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`)

**NAO PROCEDE o contraexemplo antigo das reutilizáveis:** seleção passou a consultar o fato inteiro, escolher e depois conferir reutilização/destino. (`app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:246`; `app/chamados/services/declaracao_texto.py:267`)

**NAO PROCEDE novo juiz em `reapontar_resolvedora`:** as irmãs retratadas vêm de `_colisao_ao_datar`, que recompõe `[p]+outras`, chama a canônica e recusa quando `p` perde ou existe outra encerrada. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:889`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`)

**4. PROCEDENTE — leitores ainda tratam conversa civil como turno.** A declaração atual limita dia do turno a `conversa=False`, mantendo dia civil em `abrir_conversa`. (`app/chamados/services/fio.py:1031`; `app/chamados/models.py:2632`; `app/chamados/models.py:2637`)

O caso mais forte é o próprio nascimento: `_nascer` passa também o dia civil da conversa a `resolvedora_valida`, num proxy sem discriminador; o juiz presume conversa apenas quando a data é nula e compara ao turno do chamado. Conversa criada hoje para chamado D−2 pode levantar `ResolvedoraCruzada`. (`app/chamados/models.py:2661`; `app/chamados/models.py:2665`; `app/chamados/models.py:2666`; `app/chamados/models.py:2668`; `app/chamados/juizes.py:1966`; `app/chamados/juizes.py:1968`; `app/chamados/juizes.py:1985`; `app/chamados/juizes.py:2001`)

Outros leitores sem discriminação:

- Fio marca divergência comparando dia civil ao turno. (`app/chamados/services/fio.py:86`; `app/chamados/services/fio.py:88`)
- Juiz do dia do chamado incorpora datas de todas as perguntas, inclusive conversas, tanto singular quanto lote. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:209`; `app/chamados/juizes.py:214`)
- Prévia da folha agrupa perguntas por `data_evento` e apresenta resposta de conversa como material do dia da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:67`; `app/folha/previa.py:95`; `app/folha/previa.py:142`)
- Copiloto do dia reúne conversas civis em `perguntas_deste_dia`. (`app/ponto/services/dia_do_colab.py:64`; `app/ponto/services/dia_do_colab.py:71`; `app/ponto/services/dia_do_colab.py:99`)

**5. NAO PROCEDE no contador atual:** usa `data_evento` da Pergunta e dia local de aplicação da 0052; ausência da migração ou dia nulo alarma. O escape de turno antigo contado como acervo está declarado. (`app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1115`; `app/chamados/services/fio.py:1119`; `app/chamados/services/fio.py:1120`)

**9. NAO PROCEDE apagamento físico; PROCEDENTE retirada de efetividade sem veto de mérito.** A retratação preserva texto da resposta, validação e decisões: só salva via/carimbo/autor/recusa. Porém `duplicata` sai da identidade e mata a pergunta antes da resposta. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2894`; `app/chamados/models.py:2898`; `app/chamados/models.py:2902`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`)

O próprio backfill define `admin_decidiu` com resposta como mérito e explica que torná-la duplicata retira a decisão do fato; `_migrar_sob_trava` faz essa troca sem chamar o veto. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:126`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/services/fio.py:1212`; `app/chamados/services/fio.py:1213`)

Contraexemplo abaixo H: canônica `p1` respondida/validada fora do destino; `x=p2` respondida, `admin_decidiu`, sem validação nem célula no destino; `p3` muda viva no velho. `x` pode continuar viva antes da migração, pois resposta sem concordância comprovada com luz mantém vitalidade; migração escolhe `p1`, retrata `x` e `p3`, e nada migra para o destino. A decisão de `x` fica histórica, sem a proteção exigida no backfill. Reprodução executada e ocorrência em produção: **[nao verificado]**. (`app/chamados/juizes.py:1100`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1153`; `app/chamados/juizes.py:1165`; `app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1203`; `app/chamados/services/fio.py:1204`; `app/chamados/services/fio.py:1212`)

## 07/11 14:54 · sessao c4396d29c20e · - · - · origem=Ronald (tmux)

**Classe (2): PROCEDENTE.** A centralização de `select_for_update` não eliminou aquisições posteriores por `save`/`UPDATE`; há dois caminhos concretos restantes. (`app/chamados/models.py:291`; `app/chamados/services/materializacao.py:672`)

1. **Chamado → Disputa via S143.** `Chamado.fechar()` chama `transicionar()` e `_aplicar()`, sem passar por `_recarregar_estado_se_mudou`. `_gravar_estado` trava somente o Colaborador e salva o Chamado; seu `post_save` chama S143, cujo resolver só então trava todas as Disputas abertas do colaborador. (`app/chamados/models.py:447`; `app/chamados/models.py:128`; `app/chamados/models.py:442`; `app/chamados/models.py:286`; `app/chamados/models.py:291`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1683`)

   Contraexemplo de código: chamado administrativo sem perguntas próprias, sem justificativa pendente e sem Disputa própria impeditiva, com **outra** Disputa aberta da mesma pessoa. A guarda S127 consulta somente `chamado=chamado`; essa outra Disputa não impede a transição, mas será travada pelo resolver depois do UPDATE do Chamado. Reprodução em banco: **[nao verificado]**. (`app/chamados/juizes.py:2083`; `app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/services/disputa_emissao.py:1683`)

2. **Pergunta → Disputa na devolução por ausência rejeitada.** `devolver_absorvidas_da_ausencia_rejeitada` abre atomic e chama `reabrir_para_correcao(..., desfazer_absorcao=True)`; este chega ao UPDATE de Pergunta em `descarimbar_em_lote`. Depois, no mesmo atomic externo, chama `d.reabrir_sistema`, que salva a Disputa. Esses envelopes não antecipam a trava do Colaborador/Disputa. (`app/chamados/services/materializacao.py:672`; `app/chamados/services/materializacao.py:673`; `app/chamados/services/disputa_emissao.py:1516`; `app/chamados/models.py:2973`; `app/chamados/services/materializacao.py:676`; `app/chamados/models.py:2130`)

**NAO PROCEDE a permanência dos quatro `select_for_update` explícitos fora do sítio único nos arquivos de produção examinados:** as quatro aquisições localizadas estão em `trava_na_ordem`, na sequência Colaborador, Disputa, Chamado, Pergunta. Isso não certifica a ordem das escritas acima. (`app/chamados/models.py:2508`; `app/chamados/models.py:2515`; `app/chamados/models.py:2528`; `app/chamados/models.py:2532`)

**O sítio citado na v7 foi corrigido:** `_recarregar_estado_se_mudou` agora antecipa Disputas abertas e Chamados vivos antes de ler `status_local`; a insuficiência é a existência de outras entradas no escritor de estado. (`app/chamados/models.py:689`; `app/chamados/models.py:690`; `app/chamados/models.py:442`)

**Parte do item 7 — `status_local`: NAO PROCEDE divergência de significado demonstrada.** O campo armazena o estado do Chamado; `_gravar_estado` atribui o destino, a leitura recarrega a instância quando o estado persistido difere e os conjuntos oficiais distinguem vivo, encerrado e terminal. A migração da trava mantém esse significado. (`app/chamados/models.py:52`; `app/chamados/models.py:228`; `app/chamados/models.py:690`; `app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:30`)

**Limite do selo de ordem:** o observador registra apenas a primeira aquisição de cada **tabela**, portanto não demonstra que uma segunda Disputa/Chamado da mesma pessoa, ainda não travada, não foi adquirida depois da Pergunta. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`)

**Sinais continuam síncronos:** S88 reconcilia o pai, S143 resolve perguntas e S144 chama o materializador durante `post_save`; a correção da entrada por `_recarregar` não transforma esses sinais em garantia global de ordem. (`app/chamados/signals.py:41`; `app/chamados/signals.py:92`; `app/chamados/signals.py:140`)

## 07/11 14:52 · sessao cc914c55e5a6 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Baixa — v5.r #5: FECHA os dois escapes apontados.** O detector agora reconhece `/usr/bin/python3`; o laço remove `-I` da ocorrência `k`, individualmente. Na reprodução em memória, o wrapper atual ficou limpo, as sete retiradas foram detectadas e ambas as ocorrências de uma linha sintética foram mordidas. O MORDE absoluto detectou `/python3 -c`, pelo motivo declarado. Não identifiquei regressão nesse trecho; isso continua sendo reconhecimento textual, sem cobrir comandos montados por variável (`bin/tests/test_codex_pergunta.sh:56-70`).

2. **Informativo — selo vizinho marcado CURADO: FECHA a quebra de âncora.** As duas âncoras do mutante existem exatamente uma vez; a substituição em memória produz shell sintaticamente válido, desativa a porta posterior e insere a consulta antes do slot (`bin/tests/test_disjuntor.sh:1495-1502`, `:1780-1783`; `bin/codex_pergunta.sh:342`, `:370-371`). O cenário cobra a consequência correta: três triagens durante a espera devem impedir a chamada; consultar somente antes permite perder essa mudança (`bin/tests/test_disjuntor.sh:1419-1432`). A chamada antecipada usa `python3` sem `-I`, mas o caso executa o wrapper mutado diretamente, sem aplicar FORMA-PY-ISOLADO: não há morte incidental por esse selo (`bin/tests/test_disjuntor.sh:1504`, `:1782`). Mordida efetiva do cenário completo: **[nao verificado]**.

3. **Informativo — v5.r #6: FECHA as correções documentais marcadas CURADO.**

   - **Retornos:** L-126 agora distingue pré-check/revalidação com rc 2 do inventário final proibido ou falho com rc 3, conforme o código (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:183`, `:237-239`, `:386-392`, `:413-426`).
   - **Vaga por repositório:** corresponde à derivação pelo `git-common-dir` de `CODEX_VIVO` e ao `flock`; clones distintos continuam com vagas distintas (`app/docs/LEIS.md:147`; `CLAUDE.md:352`; `bin/codex_pergunta.sh:56`, `:71-76`, `:232-233`).
   - **“Não remove a wt”:** descreve corretamente o wrapper, que imprime a remoção recomendada; continua removendo temporários e sidecars (`CLAUDE.md:352`; `bin/codex_pergunta.sh:259`, `:376-383`, `:417`).
   - **Proibições:** a exceção do gitfile raiz e a classificação de autoload/symlink no conjunto tocado correspondem ao inventário. Hardlinks são acrescentados explicitamente a esse conjunto (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:152`, `:160-177`). O hash do gitfile é uma fotografia tomada no pré-check e comparada depois (`bin/codex_pergunta.sh:242`, `:388`, `:414`).
   - **Opus:** a distinção entre guarda mecânica e conduta permanece correta: o fluxo executa Codex, inventaria e termina sem consultar aprovação opus (`CLAUDE.md:351-353`; `bin/codex_pergunta.sh:394-448`).

4. **Baixa, condicional — DESCARTADO #3: o motivo sustenta apenas um descarte limitado.** A ausência declarada de `PYTHONDONTWRITEBYTECODE` naquela sessão não refuta a diferença ambiental apontada: o wrapper usa `-I`, sem `-B`, e os auxiliares importam módulos locais (`bin/codex_pergunta.sh:96-102`, `:278`, `:371`; `bin/raspar_publico.py:15-16`; `bin/disjuntor.py:123-124`, `:671-672`). Estar no `.gitignore` não impede a escrita de bytecode (`.gitignore:2-3`). Portanto, sustenta “nenhum efeito concreto demonstrado”, mas não “efeitos ambientais equivalentes”; geração efetiva no host e ambiente de produção: **[nao verificado]**.

5. **Informativo — DESCARTADOS #2c e #7 se sustentam como ausência de defeito demonstrado.** Pergunta, anexo e sonda relativos são entradas deliberadamente resolvidas pelo cwd, conforme o próprio contrato do wrapper (`bin/codex_pergunta.sh:68`, `:86-87`, `:98`, `:275`, `:306-308`). `_IRC` torna-se agregado após conferir o gitfile, mas seu consumidor pergunta apenas se houve falha; `_INV1` e `.inventario` também são consumidos como vazio/status/diagnóstico, sem exigir TSV puro nesses caminhos (`bin/codex_pergunta.sh:386-392`, `:413-418`).

6. **Média condicional / baixa de cobertura — as FILAS continuam procedentes; o delta não as fecha.**

   - **PATH — média condicional:** executáveis continuam resolvidos por nome. “Sem `.`” sozinho não exclui componentes relativos ou vazios; PATH efetivo de produção: **[nao verificado]** (`bin/codex_pergunta.sh:125`, `:268`, `:394`).
   - **HOOK_STOP_RAIZ — média condicional:** continua aceitando raiz relativa e usando-a para caminhos de estado. Ser variável do chamador delimita o vetor, sem eliminá-lo (`bin/disjuntor.py:60`, `:119`, `:247-249`; `bin/hook_stop_fila1.py:30-35`).
   - **`py_iso_nomes` — baixa de cobertura:** a triagem caracteriza corretamente a limitação. A fixture instala wrapper e B3 falso; o caso exige a mensagem específica da guarda. Sem ela, a ausência do raspador oferece outra saída rc 4. Isso distingue a guarda, mas não demonstra vazamento nem envio cru ao Codex (`bin/tests/test_codex_pergunta.sh:74-76`, `:117-120`, `:404-407`; `bin/codex_pergunta.sh:302`, `:311`).
   - **Duas listas — média preexistente:** `_nomes` roda no subshell da pergunta e novamente no pai para anexos/sonda; sem override, pode extrair duas fotografias. O cleanup do pai também não conhece o primeiro temporário (`bin/codex_pergunta.sh:259`, `:265-268`, `:285-286`, `:302`).
   - **Relógio B3 — preexistente:** a chamada continua sem fornecer `agora`, chegando ao fallback `time.time()` (`bin/disjuntor.py:1710`, `:1042`, `:989`, `:980`, `:877`).

7. **Informativo — limite da verificação.** Executei somente verificações em memória e análise sintática; as suítes integrais criam/removem arquivos e não foram executadas sob a regra de leitura (`bin/tests/test_codex_pergunta.sh:18-19`, `:74-81`; `bin/tests/test_disjuntor.sh:39-41`, `:1492-1502`). Resultado integral dos selos, prova real e efeitos em produção: **[nao verificado]**.

## 07/11 14:45 · sessao dc04abcadd48 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Informativo — 1b fechado para o vetor declarado.** As sete chamadas diretas usam `python3 -I`: inventário inicial, nomes, duas raspagens, B3, revalidação e inventário final. Não há outro lançamento Python direto escondido por variável ou continuação no wrapper atual (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`). Isso fecha a importação de `app/hashlib.py`, `app/subprocess.py` ou `app/json.py` pelo cwd nessas chamadas; não demonstra isolamento geral dos processos do host (`bin/codex_pergunta.sh:96`).

2. **Média, condicional e preexistente — ainda existem dependências do ambiente/cwd.**

   - O inventário executa `git` pelo `PATH` herdado; o shell também resolve executáveis como `bash`, `timeout` e `codex` pelo nome. Com componente relativo ou `.` no `PATH`, arquivo executável plantado em `<wt>/app` pode rodar no host. O `PATH` de produção é **[nao verificado]** (`bin/codex_pergunta.sh:125`, `:268`, `:394`).
   - O B3 aceita `HOOK_STOP_RAIZ`, inclusive relativo, e usa essa raiz para ler/gravar estado; o fornecedor do topo também aceita a variável. Portanto, esse caminho pode depender do cwd mesmo sob `-I` (`bin/disjuntor.py:60`, `:119`, `:247`, `:274`; `bin/hook_stop_fila1.py:30`).
   - Pergunta, anexos e sonda relativos dependem deliberadamente do cwd do chamador (`bin/codex_pergunta.sh:68`, `:86`, `:87`, `:275`, `:306`, `:308`). Comportamento interno do executável externo `codex`, além dos argumentos fornecidos: **[nao verificado]** (`bin/codex_pergunta.sh:394`).

3. **Baixa — “rodam iguais sob `-I`” precisa de qualificação.** Os imports locais necessários continuam disponíveis: o raspador insere seu próprio `bin` antes de importar `nome_no_texto`; o B3 faz isso antes de importar `orcamento_contexto` e `hook_stop_fila1`. Não aparece import de `<wt>/app` nesses caminhos (`bin/raspar_publico.py:15`; `bin/disjuntor.py:123`, `:671`; `bin/nome_no_texto.py:26`).

   Entretanto, `-I` muda o tratamento das variáveis de inicialização, inclusive `PYTHONDONTWRITEBYTECODE`; sem `-B`, os imports locais podem passar a gerar bytecode. Criação efetiva no host é **[nao verificado]**. A equivalência funcional dos imports está sustentada pelo código; igualdade de todos os efeitos ambientais, não (`bin/codex_pergunta.sh:96`, `:102`, `:278`, `:371`). O Python dos auxiliares citados realmente roda em containers (`bin/relato_nomes.sh:18`; `bin/sonda_leitura.sh:235`, `:239`).

4. **Média — os testes têm mordida, mas `py_iso_nomes` tem uma causa incidental.** [nao verificado]

   - **`pre`:** o mutante deixa passar a primeira varredura; a revalidação ainda isolada recusa depois. Morre por deixar de apresentar o motivo exigido para o pré-check, o que distingue corretamente as fases (`bin/tests/test_codex_pergunta.sh:379`, `:382`, `:113`; `bin/codex_pergunta.sh:386`).
   - **`reval`:** planta durante a espera da trava e exige recusa antes do Codex; retirar `-I` dessa fase permite ultrapassá-la (`bin/tests/test_codex_pergunta.sh:197`, `:204`, `:206`, `:385`; `bin/codex_pergunta.sh:386`, `:394`).
   - **`fim`:** cobra rc 3, motivo “motor v2”, ausência de sidecar e presença do módulo plantado no inventário. A expectativa corresponde ao defeito declarado (`bin/tests/test_codex_pergunta.sh:117`, `:389`, `:394`).
   - **`nomes`:** a fixture copia somente o wrapper e cria um B3 falso; não instala o raspador. Sem `-I`, `json.py` permite passar `_nomes`, mas a chamada seguinte continua terminando rc 4 porque falta `raspar_publico.py`. O mutante morre pela mensagem “sem a lista de nomes” ausente, sem demonstrar envio de dado cru ou chamada ao Codex (`bin/tests/test_codex_pergunta.sh:69`, `:70`, `:378`, `:398`, `:401`, `:499`; `bin/codex_pergunta.sh:311`). Execução integral desses casos: **[nao verificado]**.

5. **Baixa — FORMA cobre a escrita atual, não toda grafia possível.** O detector reconhece as sete chamadas atuais e acusa a retirada individual de `-I`; porém exclui `/` como delimitador anterior, deixando escapar `/usr/bin/python3`, e não interpreta comandos montados por variável (`bin/tests/test_codex_pergunta.sh:56`, `:59`, `:63`). Também promete mordida por ocorrência, mas percorre linhas e remove apenas a primeira ocorrência de cada linha (`bin/tests/test_codex_pergunta.sh:54`, `:61`, `:62`). Essas formas escapáveis não estão nos sete lançamentos atuais (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

6. **Média — documentação corrigida em parte, com novas generalizações excessivas.**

   - **L-126:** junta pré-check, revalidação e fim sob “proibido ou falha = rc 3”; os dois primeiros recusam com **rc 2**, e somente o fim converte para **rc 3** (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:183`, `:239`, `:392`, `:426`).
   - **CLAUDE:** limita rc 3 ao inventário final e atribui corretamente o descarte ao Code; o comando de remoção é apenas impresso (`CLAUDE.md:351`; `bin/codex_pergunta.sh:417`).
   - **Teto:** “1 Codex construtor por vez” precisa dizer **por repositório selecionado**. A vaga deriva do `git-common-dir` de `CODEX_VIVO`; clones distintos têm vagas distintas (`CLAUDE.md:352`; `app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:56`, `:72`, `:76`, `:233`).
   - **“Não apaga nada”:** correto se significar “não remove a wt”; literalmente, o wrapper apaga temporários e sidecars (`CLAUDE.md:352`; `bin/codex_pergunta.sh:259`, `:376`, `:381`).
   - **Proibições:** o gitfile raiz é exceção explícita; symlink/autoload são classificados no conjunto de alterações, não incondicionalmente em toda entrada da base (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:152`, `:160`, `:170`, `:176`, `:414`).
   - A ausência de guarda de opus e do “1 opus” está descrita corretamente como conduta (`CLAUDE.md:353`; `bin/codex_pergunta.sh:393`, `:442`, `:448`).

7. **Informativo — escritores, leitores e significados.**

   | Campo/arquivo | Quem escreve → quem lê |
   |---|---|
   | `_INV0` | stdout **e stderr** do inventário → teste de vazio e diagnóstico da recusa (`bin/codex_pergunta.sh:237`, `:238`, `:239`). |
   | `_IRC0` | status da atribuição anterior → teste explícito de sucesso; separado do conteúdo (`bin/codex_pergunta.sh:237`, `:238`). |
   | `_INV1` | stdout/stderr do inventário, acrescido de `FALHOU` se necessário → teste de vazio (`bin/codex_pergunta.sh:386`, `:389`). |
   | `.inventario` | inventário final; wrapper acrescenta alteração do gitfile → filtro da `.recusa` e asserções dos testes (`bin/codex_pergunta.sh:413`, `:414`, `:418`; `bin/tests/test_codex_pergunta.sh:255`, `:394`). |
   | `_IRC` | inicialmente status do Python; depois status **agregado**, podendo virar 3 pelo gitfile → decisão de recusa e diagnóstico (`bin/codex_pergunta.sh:413`, `:414`, `:415`, `:417`). |
   | Lista de nomes | extração/escrita pelo auxiliar, ou caminho fornecido por `CODEX_NOMES_JSON` → validação de tamanho e leitura pelo raspador (`bin/relato_nomes.sh:11`, `:13`, `:21`; `bin/codex_pergunta.sh:266`, `:270`; `bin/raspar_publico.py:20`). |

   `_INV1` e `.inventario` carregam registros **e diagnósticos**, portanto não são listas TSV puras; os consumidores mostrados usam vazio/status ou marcadores explícitos (`bin/codex_pergunta.sh:386`, `:389`, `:413`, `:418`). `_IRC` tem a transição de significado indicada acima; não deve ser tratado como status bruto do Python após a conferência do gitfile (`bin/codex_pergunta.sh:414`).

8. **Média, preexistente — “lista UMA vez” não descreve o fluxo default.** A primeira `_nomes` roda dentro de `$(_pergunta_raspada)`; suas atribuições não retornam ao pai. Com anexo/sonda, o pai chama `_nomes` novamente e, sem override, pode extrair outra lista: duas fotografias temporais, embora ambas passem pela mesma porta (`bin/codex_pergunta.sh:245`, `:263`, `:265`, `:268`, `:285`, `:302`). O primeiro temporário também não fica identificado em `DADOS` para o cleanup do pai (`bin/codex_pergunta.sh:259`).

9. **Checklist das classes — sem aprovação ampla implícita.**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | Não identifiquei duplicação: inventário único com três consumidores; raspagem usa `nome_no_texto`; B3 consulta a decisão existente (`bin/codex_pergunta.sh:105`, `:237`, `:386`, `:413`; `bin/raspar_publico.py:16`, `:31`, `:36`; `bin/disjuntor.py:1042`). |
   | Segundo escritor fora da porta | Não identifiquei novo escritor desses artefatos na v5; produção mais ampla **[nao verificado]** (`bin/codex_pergunta.sh:237`, `:413`, `:414`; `bin/relato_nomes.sh:21`). |
   | Relógio fora da borda | Permanece no B3: `--b3-parada` não fornece `agora`, e a decisão lê `time.time()` como fallback. Preexistente; v5 não altera isso (`bin/disjuntor.py:1710`, `:1042`, `:989`, `:980`, `:877`). |
   | Derivação paralela | Inventário e predicado de nomes permanecem únicos; as duas capturas de nomes são a ressalva do item 8 (`bin/codex_pergunta.sh:105`, `:268`, `:302`; `bin/raspar_publico.py:16`). |
   | Selo/RED sem mordida | Ressalvas dos itens 4–5; execução completa **[nao verificado]** (`bin/tests/test_codex_pergunta.sh:56`, `:69`, `:401`). |
   | Ordem de trava | Vaga → gate/slot → resposta → estado B3; triagem também toma resposta antes de estado. Não identifiquei inversão nova nesses caminhos (`bin/codex_pergunta.sh:232`, `:342`, `:348`, `:368`, `:371`; `bin/disjuntor.py:786`, `:818`). |
   | Pouso antes do pré-requisito | Suite no hash, prova real, primeiro uso de negócio e pouso **[nao verificado]**; a exigência de suite antes do opus permanece documental (`CLAUDE.md:375`). | [nao verificado]
   | Campo com dois significados | Ressalvas do item 7; continua também a separação entre raiz de pausa/LOGD e raiz da vaga (`bin/codex_pergunta.sh:76`, `:88`, `:191`). |

## 07/11 14:42 · sessao 27743e1766f0 · - · - · origem=Ronald (tmux)

1. **Média — L-126 amplia indevidamente o `rc 3`.** O texto novo junta “pre-check, revalidação e fim” e depois afirma “proibido ou falha = rc 3” (`app/docs/LEIS.md:147`). No pre-check, qualquer saída não vazia ou código não zero termina por `_recusa2`, portanto **rc 2** (`bin/codex_pergunta.sh:183`, `:237`, `:238`, `:239`); revalidação também termina **rc 2** (`:386`, `:389`, `:392`). Só a recusa final converte em **rc 3** (`:413`, `:415`, `:426`). CLAUDE limita essa promessa ao inventário **no fim** e está correto (`CLAUDE.md:351`).

2. **Média — “1 Codex construtor por vez” precisa dizer “por repositório”.** A vaga deriva de `git-common-dir` do `CODEX_VIVO` selecionado (`bin/codex_pergunta.sh:56`, `:72`, `:76`), e o `flock` prende esse arquivo (`:232`, `:233`). Assim, as frases de `CLAUDE.md:352` e `app/docs/LEIS.md:147` descrevem corretamente worktrees do mesmo repo, mas não um teto global entre clones distintos. É a **fila antiga 3a** do anexo, não defeito introduzido pela v5. A pausa/LOGD continuam na raiz do script, enquanto a vaga vem do repositório vivo: duas fronteiras diferentes (`bin/codex_pergunta.sh:88`, `:191`, `:193`, `:76`), correspondentes à fila antiga 3b.

3. **Baixa — proibição documental é mais ampla que a implementação.** A classificação de symlink/autoload/`.git*` percorre apenas `tocados` (`bin/codex_pergunta.sh:170`, `:171`, `:176`); symlink idêntico ao blob/modo da base não entra por esse motivo (`:160`). O arquivo `.git` da raiz é podado da varredura (`:151`, `:152`) e conferido separadamente (`:242`, `:414`). A formulação irrestrita “.git em qualquer nível [...] symlink [...] = PROIBIDO” merece explicitar o universo de alterações e a exceção do gitfile raiz (`app/docs/LEIS.md:147`). Existência de symlink rastreado nessa base: **[nao verificado]**.

4. **Média, preexistente — lista de nomes não é obtida “UMA vez”.** Essa promessa está em `bin/codex_pergunta.sh:263`, mas a primeira `_nomes` roda dentro de `$(_pergunta_raspada)` (`:276`, `:285`, `:286`); suas atribuições `DADOS`/`NOMES` ficam no subshell (`:265`, `:266`). Com anexo/sonda, `_nomes` roda novamente no pai (`:302`), que inicialmente tinha `DADOS=""` (`:245`). Sem `CODEX_NOMES_JSON`, isso produz duas extrações por `relato_nomes.sh` (`:267`, `:268`), podendo haver duas fotografias temporais. O temporário inicial também não fica disponível ao cleanup do pai (`:259`). Não é um novo escritor: ambas passam pela mesma porta, cuja extração/escrita está em `bin/relato_nomes.sh:8`, `:11`, `:13`, `:18`, `:21`.

5. **Informativo — correções documentais centrais procedem.** A remoção da wt é apenas instruída no texto da recusa; o comando não é executado (`bin/codex_pergunta.sh:417`). “Descartar a wt é conduta do Code” está correto (`CLAUDE.md:351`, `:352`). O fluxo não exige certificado opus nem controla construtor opus antes/depois da chamada (`bin/codex_pergunta.sh:393`, `:404`, `:442`, `:448`), coerente com `CLAUDE.md:353`. A frase “o wrapper não apaga nada” deve ser entendida como **a wt**: há remoção de temporários e sidecars (`bin/codex_pergunta.sh:259`, `:376`, `:381`).

6. **Classes restantes, por inspeção:** inventário permanece em uma única definição com três consumidores (`bin/codex_pergunta.sh:105`, `:237`, `:386`, `:413`); raspagem continua perguntando ao mesmo `nome_no_texto` para mascarar/conferir (`bin/raspar_publico.py:16`, `:31`, `:36`); B3 continua perguntando à decisão existente (`bin/disjuntor.py:1039`, `:1042`). Não identifiquei juiz de negócio duplicado nem novo escritor fora dessas portas nos trechos auditados; o registro de autoridades permanece em `app/core/juizes.py:170`, com papel documental explicado em `app/docs/JUIZES.md:10`. Ordem: vaga → gate/slot → resposta → estado B3 (`bin/codex_pergunta.sh:232`, `:342`, `:348`, `:368`, `:371`); triagem também toma resposta antes de estado (`bin/disjuntor.py:779`, `:786`, `:789`, `:804`, `:818`). Não encontrei inversão nova nesses caminhos. Execução da prova real, suite no hash, pouso e atendimento dos pré-requisitos: **[nao verificado]**.

## 07/11 14:42 · sessao 0e16194bbd08 · - · - · origem=Ronald (tmux)

1. **1b: fechado para o vetor declarado de import Python; isolamento geral do host não demonstrado.** Os sete lançamentos diretos usam `python3 -I`: inventários em `bin/codex_pergunta.sh:237`, `:386`, `:413`; validação de nomes em `:270`; raspagem em `:278`, `:311`; B3 em `:371`. O inventário ainda resolve `git` pelo `PATH` herdado (`bin/codex_pergunta.sh:124-125`); outros executáveis também são chamados pelo nome, inclusive `bash` (`:268`) e `timeout`/`codex` (`:394`, `:401`). Se o `PATH` contiver componente relativo/`.` e o cwd for `<wt>/app`, executável plantado ali permanece vetor possível; o `PATH` efetivo de produção é **[nao verificado]**.

2. **Dependências de cwd deliberadas e restantes.** Pergunta, anexos e sonda podem ser argumentos relativos, usados sem mudança global de cwd (`bin/codex_pergunta.sh:67-68`, `:86-87`, `:275`, `:306`, `:308`). Além disso, `HOOK_STOP_RAIZ` continua sendo lido pelo B3, inclusive se relativo (`bin/disjuntor.py:60`, `:118-119`), e pelo módulo que fornece o topo (`bin/hook_stop_fila1.py:30-34`). O ramo pode então ler/gravar estado relativo ao cwd (`bin/disjuntor.py:247-274`). `-I` não elimina leituras explícitas de `os.environ`; isso não é variável `PYTHON*` (`bin/disjuntor.py:60`).

3. **`raspar_publico.py`: imports locais continuam disponíveis sob `-I`.** Importa `json`, `os`, `sys` antes de inserir explicitamente a própria pasta; importa depois o juiz `nome_no_texto` (`bin/raspar_publico.py:11-16`). O juiz importa somente `re` (`bin/nome_no_texto.py:26`). Os nomes e textos entram por argumentos/arquivos com UTF-8 explícito (`bin/raspar_publico.py:19-28`), e a saída raspada também (`:40`). Nenhuma leitura explícita de `PYTHON*` nesse caminho foi encontrada nesses módulos; a equivalência executada com/sem `-I` é **[nao verificado]**.

4. **B3: a decisão segue o juiz existente, com imports locais explicitamente restaurados.** `main` encaminha `--b3-parada` para `b3_parada` (`bin/disjuntor.py:1709-1710`), que passa por `estado_b3` → `julga_b3` → `decide_b3` (`:1042`, `:989`, `:980`). `_abre` insere a própria pasta antes de importar `orcamento_contexto` (`:122-125`); `_topo_consistente` faz o mesmo para `hook_stop_fila1` (`:671-672`). Ambos têm somente imports de biblioteca padrão no topo (`bin/orcamento_contexto.py:26-31`; `bin/hook_stop_fila1.py:20-34`). Não foi localizado import de código em `<wt>/app` nesse ramo; `_no_relato`, que carregaria `app/core/esteira_vigia.py`, pertence a outro caminho (`bin/disjuntor.py:179-188`, `:227`).

5. **Python dos auxiliares fica no container, conforme o comentário novo.** `relato_nomes.sh` executa Python por `docker exec saas_core` (`bin/relato_nomes.sh:17-19`); `sonda_leitura.sh` por `docker run ... python manage.py` (`bin/sonda_leitura.sh:235-240`). A árvore escolhida por `SONDA_ARVORE` influencia o Python do container, não um interpretador Python do host (`bin/sonda_leitura.sh:232-239`). A ausência de efeitos específicos do executável externo `codex` antes do sandbox é **[nao verificado]**: o wrapper só fornece sua chamada/configuração (`bin/codex_pergunta.sh:394-402`).

6. **Caveat de precisão do comentário “rodam iguais”.** `-I` pretende mudar a resolução inicial de imports e o tratamento das variáveis de inicialização Python (`bin/codex_pergunta.sh:96-102`); portanto “iguais” deve significar o funcionamento esperado com os imports locais restaurados, não igualdade de todo efeito ambiental. Em particular, ignorar `PYTHONDONTWRITEBYTECODE` pode permitir criação de bytecode dos módulos locais importados — inferência, efeito executado **[nao verificado]** (`bin/raspar_publico.py:15-16`; `bin/disjuntor.py:123-124`, `:671-672`).

7. **Lista de nomes: comentário “UMA vez” já não descreve o fluxo default.** `_pergunta_raspada` roda em substituição de comando (`bin/codex_pergunta.sh:285-286`); sua `_nomes` cria `DADOS`/`NOMES` nesse subshell (`:265-268`). As atribuições não atualizam o `DADOS` externo, inicializado vazio (`:245`). Havendo anexo/sonda, `_nomes` é chamado novamente no processo externo (`:301-302`), podendo extrair uma segunda lista do banco. A promessa “UMA vez” (`:263`) não vale nesse cenário sem `CODEX_NOMES_JSON`; não é novidade introduzida pelo diff v5.

## 07/11 14:41 · sessao c0b04a894150 · - · - · origem=Ronald (tmux)

1. **Média — mutante `py_iso_nomes` morre com causa incidental.** O cenário monta somente o wrapper e um B3 falso; não instala `raspar_publico.py` nem `nome_no_texto.py` (`bin/tests/test_codex_pergunta.sh:68-70`). Retirado `-I` pelo mutante (`bin/tests/test_codex_pergunta.sh:499`), `app/json.py` encerra o Python com sucesso (`bin/tests/test_codex_pergunta.sh:378`, `:399`), permitindo passar `_nomes` (`bin/codex_pergunta.sh:270`). A execução então termina **rc 4 porque falta o raspador** (`bin/codex_pergunta.sh:311`); o caso morre pela mensagem “sem a lista de nomes” ausente, verificada em `bin/tests/test_codex_pergunta.sh:113`, `:401`. Não comprova vazamento nem chamada ao Codex; a execução completa do teste permanece [nao verificado].

2. **Informativa — os outros três casos correspondem à classe declarada.** `pre` planta antes da primeira varredura; sem isolamento, o pre-check passa silenciosamente, mas a revalidação isolada ainda rejeita, mudando o motivo exigido (`bin/tests/test_codex_pergunta.sh:379-382`; `bin/codex_pergunta.sh:237-239`, `:386-392`). `reval` planta durante a espera e, sem isolamento nesse sítio, permite chamar o Codex (`bin/tests/test_codex_pergunta.sh:197-206`, `:385-388`; `bin/codex_pergunta.sh:386-398`). `fim` planta o módulo junto com alteração do motor e cobra rc 3, motivo e presença do módulo no inventário (`bin/tests/test_codex_pergunta.sh:389-395`, `:117-122`; `bin/codex_pergunta.sh:413-426`). Resultado integral da suíte [nao verificado].

3. **Informativa — FORMA cobre as sete chamadas atuais.** Sonda somente em memória confirmou sete ocorrências e detecção ao remover isoladamente `-I` de cada uma: `bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`; implementação do detector em `bin/tests/test_codex_pergunta.sh:56-63`. Não há Python host adicional direto, variável ou continuação escondida nesses lançamentos (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

4. **Baixa — promessa genérica da FORMA excede sua implementação.** A regex não detecta `/usr/bin/python3` nem comando montado por variável; ela exclui `/` como delimitador anterior (`bin/tests/test_codex_pergunta.sh:56`). Além disso, promete mordida por ocorrência (`:54-55`), mas o laço percorre linhas e o `sed` troca somente a primeira ocorrência por linha (`:61-63`). Essas grafias não estão entre os sete lançamentos atuais, mas o selo não assegura a promessa para futuras formas de escrita (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

## 07/11 14:28 · sessao 1cbdb9d63fe3 · FIO-E0-V4-E2 · leitura · origem=Code

1. **TRANCA-DE-VERDADE / `competencia_trancada`:** arquivo por chamado, mediante `status_local` + `via_encerramento`; não grava célula: app/ponto/services/fechamento.py:1415. Disparador: app/ponto/services/fechamento.py:1421. Escritores: app/chamados/services/competencia_trancada.py:184 → app/chamados/models.py:1580 → app/chamados/models.py:226, app/chamados/models.py:243, app/chamados/models.py:283. Pergunta sem novo carimbo: app/chamados/services/competencia_trancada.py:178. **O44 desarquivar:** app/chamados/views_arquivar.py:70 → app/chamados/models.py:1399 → app/chamados/models.py:247; levantamento do véu e trilha: app/chamados/views_arquivar.py:77 → app/chamados/services/arquivo.py:201, app/chamados/services/arquivo.py:208, app/chamados/services/arquivo.py:209. Toque por colab+dia: app/chamados/services/cobrar_dia.py:37; leitura: app/chamados/juizes.py:955.

2. **NOT READABLE** pelo `leitor_celula`: app/escala/services/leitor_celula.py:418, app/escala/services/leitor_celula.py:428, app/escala/services/leitor_celula.py:445. Leitor mais próximo do arquivo: app/chamados/views_arquivar.py:165. Leitor mais próximo da condição colab+dia: app/chamados/juizes.py:920, app/chamados/juizes.py:948, app/chamados/juizes.py:955.

3. **Nenhuma leitura direta ou transitiva encontrada:** app/chamados/services/tarefa_do_dia.py:294, app/chamados/services/tarefa_do_dia.py:332, app/chamados/services/tarefa_do_dia.py:345, app/chamados/services/tarefa_do_dia.py:355, app/chamados/services/tarefa_do_dia.py:378, app/chamados/services/tarefa_do_dia.py:400. Leitores da tranca fora desse caminho: app/chamados/juizes.py:948; app/chamados/models.py:1011.

4. **Leitores de `via_encerramento='competencia_trancada'`:** app/chamados/views_arquivar.py:64, app/chamados/views_arquivar.py:165; app/chamados/services/disputa_emissao.py:1662; app/chamados/services/competencia_trancada.py:259. **Outras leituras de `via_encerramento`:** app/chamados/models.py:681, app/chamados/models.py:974, app/chamados/models.py:1309, app/chamados/models.py:1342; app/chamados/services/adesao.py:90; app/ponto/services/furos_sem_cobranca.py:69, app/ponto/services/furos_sem_cobranca.py:70; app/chamados/management/commands/reabrir_orfas_enterradas.py:135; app/chamados/management/commands/auditar_invariantes_chamados.py:1068.

_2756 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
