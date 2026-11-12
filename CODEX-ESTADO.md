# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 21:54** · publicado em **11/11/2026 21:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 21:54 · sessao 2b55b7c8a142 · FIO-E0-V4-E4B · censo · origem=Code

Auditoria primeiro: as perguntas identificam o **Codex como verificador**, mas não identificam quem as redigiu; autoria de quem perguntou e identidade de quem decidiu a triagem: **[nao verificado]** (`preverif_c4.q:1`; `preverif_c6.q:1`; `preverif_c7.q:1`). O recibo registra “FILA solicitada / sem cura nesta volta”, sem identificar o solicitante (`app/docs/FIO-E0-V4-E4B-c8.txt:90`). A regra exige motivo para descarte e auditoria prévia de perguntador e triador (`CLAUDE.md:372-384`).

A justificativa “HERDADO, nao regressao” pertence ao **ciclo C↔Cel**; atribuí-la aos achados abaixo seria **[nao verificado]** (`triagem_c9_c10.txt:40`). Para `.q/.r`, contei as linhas dos anexos desde a primeira, incluindo linhas vazias; correspondência com a numeração física dos originais: **[nao verificado]**.

1. **BLOQUEANTE 1 — (b): visto e colocado em FILA.**

   **Pergunta:** “Caminhos que fecham chamado SEM passar pelas duas bordas humanas […] Algum fecha o chamado com pergunta viva da exportada sem que ninguem a entregue?” (`preverif_c6.q:8`).

   **Resposta:** “Fechamentos futuros fora das duas bordas sem entrega E4B | **Não curado — FILA de alcance.**” (`preverif_c7.r:24`).

   **Triagem/motivo registrado:** delimitação às duas bordas; o recibo preserva explicitamente o fechamento direto sem coletor (`app/docs/FIO-E0-V4-E4B-c8.txt:90`, `:93`). Justificativa adicional demonstrando que essas categorias nunca ancoram pergunta viva: **[nao verificado]**. As transições humanas diretas continuam na cópia (`app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`; `app/colaboradores/views.py:2068`).

2. **BLOQUEANTE 2 — (a): tema presente; contorno pelo `--hoje` não apontado.**

   **Pergunta:** “apply so em competencia aberta […] comp 10 sem apply” (`preverif_c4.q:10`); “reconferencia antes da escrita” (`preverif_c4.q:20`).

   **Resposta sobre o contorno pelo argv:** **ausente**. O Codex havia visto o problema anterior: “**comp 10 sem `!` não possui impedimento próprio**”; portanto, não cabe dizer que ignorou toda a proteção da competência 10 (`preverif_c4.r:9`).

   A classe **(a)** corresponde especificamente à autorização contornável: a cópia compara a competência com a janela de `hoje`, substituível pelo argumento, enquanto `agora` vem do relógio (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218-230`). A revalidação posterior verifica entrega; execução concreta do contorno: **[nao verificado]** (`app/chamados/management/commands/backfill_fio_morto_e4b.py:301-305`).

3. **FILA a — (b): leitores temporais divergentes vistos e mantidos em FILA.**

   **Pergunta:** “nenhum data_evento cru restante no caminho do S143?” (`preverif_c7.q:5`).

   **Resposta:** “A recusa de exportada lê `q.data_evento` […] sem concluir a migração integral — **FILA preexistente**.” (`preverif_c7.r:29`).

   **Triagem/motivo registrado:** “Leitores crus **preexistentes** de data_evento: FILA”; exaustividade declarada não verificada (`app/docs/FIO-E0-V4-E4B-c8.txt:96`). A divergência permanece: recusa pelo atributo cru, agrupador pelo juiz temporal, descartando `None` (`app/chamados/services/disputa_emissao.py:1835-1836`; `app/chamados/services/fecho_runtime.py:114-116`).

4. **FILA b — (b): perda de novas recusas na Pauta existente vista e mantida em FILA.**

   **Pergunta:** “Pauta DP […] idempotente por marcador+ancora” (`preverif_c6.q:6`); depois, “Cada achado do preverif_c6.r […] nao curado” (`preverif_c7.q:5`).

   **Resposta:** “Reentrega não acrescenta novos IDs à Pauta existente | **Não curado — FILA.** […] a nota S143 conserva o rastro individual.” (`preverif_c7.r:23`).

   **Triagem:** “Reentrega nao agrega novos IDs: continue na Pauta viva”, sob “FILA solicitada” (`app/docs/FIO-E0-V4-E4B-c8.txt:90`, `:92`). Motivo adicional do triador: **[nao verificado]**; a preservação da nota é observação da resposta, sem assinatura de triagem (`preverif_c7.r:23`). O `continue` permanece (`app/chamados/services/fecho_runtime.py:128-130`).

5. **FILA c — (e): NAO PERGUNTADO quanto à estabilidade posterior contra o reconciliador.**

   **Pergunta específica sobre terminar em `em_analise` e depois executar o reconciliador:** **ausente**. Há pergunta sobre caminhos do reconciliador e outra sobre falso verde, mas sem esse ensaio de estabilidade (`preverif_c6.q:8`; `preverif_c7.q:9`). [nao verificado]

   **Resposta sobre esse flap:** **ausente** nos anexos fornecidos **[nao verificado: histórico além dos anexos]**.

   O teste exige `em_analise`, confere a trilha e termina sem uma passagem posterior pelo reconciliador; isso permite constatar a cobertura ausente por leitura, sem afirmar que o flap ocorreu (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211-229`). Ocorrência do flap: **[nao verificado]**.

6. **FILA d — (a): tema dos tetos presente; ressalva de desenho não apontada.**

   **Pergunta:** “Teto de queries: o teste mede o caminho real ou um proxy?” (`preverif_c4.q:24`). [nao verificado]

   **Resposta:** “**O teto global mede caminho real**” (`preverif_c4.r:100`). Reconhecimento dos aumentos e da “PERGUNTA DE DESENHO” aberta: **ausente** nessa resposta.

   A ressalva é legível na cópia: expectativas 213/269 com `[nao verificado]` e pergunta de desenho aberta (`app/ponto/tests/test_selo_chokepoint_escrita.py:192-195`, `:207-209`). Porém, o opus generalizou: **27→28 e 59→60 estão descritos como medidos pela sessão**, não como não verificados (`app/ponto/tests/test_selo_chokepoint_escrita.py:122`, `:132`). A classe refere-se à omissão dessa ressalva documental; certificação independente dos números: **[nao verificado]**.

7. **FILA e — (c): fora do alcance quanto à execução histórica do RED.**

   **Trecho apresentado na pergunta:** “mutantes retorno_antecipado e sem_pauta morrem nos dois RED” (`preverif_c6.q:3`).

   **Resposta que comprove reexecução após c7–c9:** **ausente**. A afirmação “não re-rodado” aparece no opus, mas os logs dessas execuções não foram fornecidos para conferência (`opus_final_c10.r:4`; execução posterior: **[nao verificado]**).

   Há uma limitação estática verificável: o roteiro c9 inclui c8/c7, mas o retorno de c7 não incorpora o ramo padrão c6 que contém `retorno_antecipado`. Isso não exclui execução independente pela sessão (`app/chamados/tests/e4b_c6_verificar.py:320-333`, `:388-389`).

8. **FILA f — (e): NAO PERGUNTADO sobre conciliação dos padrões documentais.**

   **Pergunta sobre `append-only` × `Single Writer Principle` × `Parallel Change`: ausente. Resposta correspondente: ausente.** A c4 inclusive declara “Diff (sem app/docs)”; isso não equivale a uma pergunta sobre coerência dos rótulos (`preverif_c4.q:6`).

   A cópia mostra E4B com `Parallel Change / Expand-Contract`; a linha 1889 pertence a outra obra (`app/docs/BACKLOG.md:1875`, `:1889`). O recibo c4 registra substituição de `append-only`, enquanto c8/c9 declaram `Single Writer Principle` (`app/docs/FIO-E0-V4-E4B-c4.txt:33`; `app/docs/FIO-E0-V4-E4B-c8.txt:5`; `app/docs/FIO-E0-V4-E4B-c9.txt:5`). O merge específico descrito pelo opus e o conteúdo do `recibo_c6.txt`: **[nao verificado]**.

Contagem por classe, nos oito itens acima: **(a) 2 | (b) 3 | (c) 1 | (d) 0 | (e) 2.**

## 11/11 21:53 · sessao 60e1f9eb6616 · - · - · origem=Ronald (tmux)

Inventário direto de chamadas humanas e gates de mudança de estado, somente leitura. Caminho humano de todas as cascatas de fato abaixo: `[nao verificado]` onde não há chamador de borda indicado.

### Fechamentos pedidos diretamente por humano

| Caminho / porta | Escritor de status | Coletor | Prova S143 + Pauta |
|---|---|---|---|
| Detalhe → aprovar exceção | `app/chamados/views.py:426` → `app/chamados/services/acoes_chamado.py:70`, `resolvido` | Não | SEM PROVA |
| Detalhe → aceitar/recusar agenda | `app/chamados/views.py:460` → `app/chamados/services/acoes_chamado.py:108`, `resolvido` | Não | SEM PROVA |
| Detalhe → aceitar/recusar escala | `app/chamados/views.py:463` → `app/chamados/services/acoes_chamado.py:149`, `resolvido` | Não | SEM PROVA |
| Detalhe e fio → autorizar geofence, com/sem advertência | `app/chamados/views.py:540`; `app/chamados/services/fio.py:533` → `app/chamados/services/acoes_chamado.py:222`, `resolvido` | Não | SEM PROVA S143/Pauta; fluxo normal `app/chamados/tests/test_s127_fluxo_geofence.py:31`, `:38` |
| Detalhe e fio → recusar geofence | `app/chamados/views.py:543`; `app/chamados/services/fio.py:544` → `app/chamados/services/acoes_chamado.py:277`, `resolvido` | Não | SEM PROVA S143/Pauta; retratação/fecho normal `app/chamados/tests/test_geofence_validar_e_recusar.py:87` |
| Avaliar benefício | `app/colaboradores/views.py:2046` → `:2068`, `resolvido` se aprovar, `fechado` senão | Não | SEM PROVA |
| Detalhe → resposta livre / dropdown | `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:316` | Sim | `app/chamados/tests/test_e4b_c6_orm.py:90`, `:100`, `:148`, `:151` |
| Fio → resposta rápida com conteúdo / dropdown | `app/chamados/services/fio.py:708`, `:719`, `:722` | Sim | `app/chamados/tests/test_e4b_c6_orm.py:96`, `:156` |
| Command `fechar_chamados_meio_periodo --apply` | `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `:109` → `app/chamados/models.py:454`, `fechado` | Não | SEM PROVA S143/Pauta |

Os chamadores do coletor usam import local em `app/chamados/services/acoes_chamado.py:315` e `app/chamados/services/fio.py:721`; o coletor chama `transicionar` em `app/chamados/services/fecho_runtime.py:150` e `:156`. A outra chamada interna é `Chamado.fechar` em `app/chamados/models.py:454`.

`smoke_chamados` contém chamadas `transicionar` em `app/chamados/management/commands/smoke_chamados.py:134`, `:147`, `:163`, porém recusa banco cujo nome não contém `test` (`:30`) e reverte o atomic ao fim (`:39`); portanto não é caminho produtivo de persistência.

### Outras mudanças humanas diretas

| Caminho | Gate / mudança | Prova específica de S143/Pauta |
|---|---|---|
| Geofence antes da decisão, quando `registrado` | `app/chamados/services/acoes_chamado.py:229`, `:233`: `em_analise` | Fora do fecho; fluxo registrado dos três botões em `app/chamados/tests/test_geofence_validar_e_recusar.py:121` |
| Abrir registro de ausência | `app/chamados/views.py:1494`, `:1513` → `analisar`: `em_analise` | Não fecha; nota em `:1516` |
| Desarquivar encerrado por tranca | `app/chamados/views_arquivar.py:64`, `:70` → `reabrir(humano=True)`: `em_analise` | SEM PROVA S143/Pauta |
| Reabrir disputa como admin | `app/chamados/services/fio.py:581` → `app/chamados/services/acoes_disputa.py:259`, `:270` → `reabrir(humano=True)` | SEM PROVA S143/Pauta |
| Reabrir competência / devolver fila | `app/ponto/services/fechamento.py:1447` → `app/chamados/services/competencia_trancada.py:265` → `reabrir(humano=True)` | SEM PROVA S143/Pauta |
| Substituir aceite antigo de escala ao aplicar vínculo | `app/colaboradores/services/vinculo.py:384`, `:387` → `encerrar`: `fechado` | SEM PROVA S143/Pauta |
| Confirmar regularização externa | `app/chamados/services/regularizacao_ext.py:197`, `:205` → `encerrar`: `fechado` | SEM PROVA S143/Pauta |
| Desligamento | `app/colaboradores/services/desligamento.py:139`, `:142`, `:155` → disputa fecha; pai reconcilia; restantes encerram | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:272` |

Arquivar comum não altera `status_local`; a porta declara e executa silêncio em `app/chamados/views_arquivar.py:24`, `:37`. O `admin_encerra` da **Disputa** existe em `app/chamados/models.py:1943`, mas seu chamador produtivo não foi encontrado: `[nao verificado]`. Não foi localizado registro `ModelAdmin` de Chamado; como ausência sem linha demonstrativa, `[nao verificado]`. API não possui chamada direta a `Chamado.transicionar` localizada; há fechamento de **Disputa** no upload de foto (`app/api/foto_recebida.py:80`) e o signal reconcilia pai (`app/chamados/signals.py:18`, `:42`).

### Censo global por gate, incluindo cascatas e commands

Todos os destinos são decididos no motor: `app/chamados/catalogo/motor.py:99` (humana), `:111` (`RECONCILIAR`), `:112` (`ANALISAR`), `:113` (`REABRIR`), `:114` (`ENCERRAR`), `:116` (`CANCELAR`).

- **`reconciliar` → resolvido**, gate `app/chamados/models.py:641`, escrita `:654`. Chamadores produtivos: `app/chamados/signals.py:42`; `app/chamados/services/adesao.py:115`, `:150`; `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/services/limbo_folgas.py:101`, `:283`, `:286`; `app/chamados/services/materializacao.py:582`, `:771`; `app/chamados/reconciliador.py:523`; `app/chamados/services/silencio_ausencia.py:101`; `app/ponto/services/cartorio.py:986`, `:1511`, `:1524`, `:1545`, `:1623`, `:1638`; `app/ponto/services/cobertura.py:122`, `:317`; `app/ponto/services/lastro.py:400`; `app/ponto/services/veredito_celula.py:202`; `app/ponto/services/ausencia.py:694`; `app/ponto/services/triagem_batida.py:597`; `app/ponto/services/entrada_sincronizada.py:31`; `app/ponto/services/cluster_espurio.py:124`; `app/colaboradores/services/pos_vinculo.py:42`. Commands: `app/chamados/management/commands/auditar_chamados_orfaos.py:67`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`; `app/ponto/management/commands/curar_residuo_bug115.py:151`; `app/ponto/management/commands/detectar_ausencias.py:158`, `:824`, `:834`, `:845`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`.
- **`reabrir` → em_analise**, gate `app/chamados/models.py:1362`, escrita `:1442`. Chamadores: `app/chamados/views_arquivar.py:70`; `app/chamados/services/acoes_disputa.py:270`; `app/chamados/services/competencia_trancada.py:265`; `app/chamados/services/adesao.py:92`; `app/chamados/services/disputa_emissao.py:1849`, `:2009`; `app/chamados/models.py:1019` (renascimento por premissa). Commands: `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`; `app/ponto/management/commands/detectar_ausencias.py:301`; `app/ponto/management/commands/reconciliar_grade.py:530`.
- **`analisar` → em_analise**, gate `app/chamados/models.py:800`, escrita `:806`. Chamadores: `app/chamados/views.py:1513`; `app/chamados/services/disputa_emissao.py:1085`, `:1159`, `:1178`, `:1223`, `:1252`, `:1379`; `app/ponto/services/regularizacao.py:137`; command `app/chamados/management/commands/geofence_registrados_para_analise.py:32` (só aplica após `:27`).
- **`encerrar` → fechado**, gate `app/chamados/models.py:1605`, escrita `:1619`. Chamadores: `app/chamados/reconciliador.py:496`; `app/chamados/services/regularizacao_ext.py:205`; `app/colaboradores/services/desligamento.py:155`; `app/colaboradores/services/vinculo.py:387`.
- **`cancelar` → cancelado**, gate `app/chamados/models.py:773`, escrita `:788`. Commands: `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`.
- **`encerrar_por_competencia_trancada` → fechado**, gate `app/chamados/models.py:1622`, escrita `:1639`; chamador `app/chamados/services/competencia_trancada.py:184`; dispara no ato de trancar em `app/ponto/services/fechamento.py:1427` ou command `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`.

### Fatores da matriz A

Os estados pedidos não são classes disjuntas: destino/container é uma dimensão, exportada/aberta é outra, limbo é outra; o motivo limbo prevalece sobre exportada quando coexistem (`app/chamados/services/disputa_emissao.py:1835`, `:1839`, `:1841`).

- Listagem = destino deste chamado OU, **sem destino**, pai da disputa deste chamado (`app/chamados/models.py:344`). Pai com destino em outro chamado não pertence à lista desse pai (`:345`).
- Toda transição humana passa pelo motor e guards de justificativa, disputa, vivas ancoradas e furo (`app/chamados/models.py:123`, `:135`, `:153`, `:165`, `:181`). A guarda de disputa pai é própria e antecede a guarda de destino (`:153`).
- Guarda humana de viva na aberta recusa antes de S143 (`app/chamados/models.py:165`); prova do gate por destino e estado preservado em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:193`.
- A guarda de ancoradas usa competência entregue **is False** e viva no pós-estado; exportada e dia indecidível ficam fora (`app/chamados/models.py:319`, `:327`, `:349`).
- Aceito o fecho, status/campos/trilha saem pelo núcleo (`app/chamados/models.py:228`, `:229`, `:292`, `:296`); via humana é `admin` (`app/chamados/catalogo/motor.py:95`).
- S143 roda para save que escreve estado encerrado e preserva fecho se seu savepoint falhar (`app/chamados/signals.py:90`, `:96`, `:104`, `:115`, `:117`).
- Recusa exportada/limbo escreve nota no credor (`app/chamados/services/disputa_emissao.py:1855`) e só se coleta no contexto do chamado atual (`app/chamados/services/fecho_runtime.py:18`); sem coletor, não há Pauta dessa entrega (`:19`).
- Com coletor, existe Pauta por competência datável, exceto conversa/sem dia e competência já com marcador vivo (`app/chamados/services/fecho_runtime.py:114`, `:128`, `:132`).
- Ausência de listagem não impede S128 sobre outras perguntas de destino nem toda cascata de disputa; o universo S128 é separado (`app/chamados/services/disputa_emissao.py:1904`, `:1938`).
- “Abriu/reabriu após fecho” pode acontecer em terminal→terminal: prova atual acaba em `em_analise`, com trilhas `fechado`, `em_analise` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `:222`, `:225`). Reconciliador posterior e flap: SEM PROVA.

### Categorias dos caminhos sem coletor

| Caminho | Nascimento normal | Pode excluir ancoragem pelo código? |
|---|---|---|
| Aprovar exceção | `batida_fora_horario` / `horario_nao_previsto`, `app/ponto/services/triagem_batida.py:336` | Não: ação não guarda categoria (`app/chamados/services/acoes_chamado.py:21`, `:25`); a view só exige papel (`app/chamados/views.py:426`). |
| Agenda | `agenda_aceite` / `proposta`, módulo `agenda_dia`, `app/colaboradores/views.py:2363` | Criador não cria pergunta; derivador por mapa a exclui (`app/chamados/services/disputa_emissao.py:211`; mapa `app/chamados/models.py:3664`). Ação guarda categoria (`app/chamados/services/acoes_chamado.py:88`). Banco legado: não sei pelo código. |
| Escala | `escala_aceite` / `mudanca`, `app/colaboradores/services/vinculo.py:393` | Mesma situação do mapa; ação guarda categoria (`app/chamados/services/acoes_chamado.py:130`). Banco legado: não sei pelo código. |
| Autorizar/recusar geofence | Normal `auditoria_clt` / `gps_fora_geofence`, `app/ponto/services/geofence.py:361` | Ações não guardam categoria (`app/chamados/services/acoes_chamado.py:165`, `:243`); não se pode excluir categorias outras. |
| Benefício | Módulo `beneficio`, categoria omitida, `app/colaboradores/views.py:1978` | Avaliação lê FK `sol.chamado_origem` sem guarda de categoria (`app/colaboradores/views.py:2066`); banco só responde qual FK efetiva. |
| Meio período | Command filtra módulos `intervalo_pendente`, `batida_fora_escala_leve` (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:30`, `:56`) | Não filtra categoria; categorias existentes dos alvos: não sei pelo código. |
| Substituição de aceite de escala | Filtra categoria `escala_aceite`, `app/colaboradores/services/vinculo.py:384` | Criador normal acima; relações existentes: não sei pelo código. |
| Desligamento | Fecha chamados `NAO_FECHADOS` de qualquer categoria (`app/colaboradores/services/desligamento.py:150`) | Não exclui ancoragem; o próprio código prevê resposta viva que segura chamado (`:147`). |
| Regularização externa | Fecha `reg.chamado` por FK (`app/chamados/services/regularizacao_ext.py:201`) | Categoria/ancoragem dessa FK: não sei pelo código. |
| Commands/cascatas genéricas | Gates sem whitelist de categoria (`app/chamados/models.py:641`, `:773`, `:1605`) | Não existe exclusão geral por categoria nesses gates. |

A porta genérica de pergunta aceita `chamado_destino` (`app/chamados/models.py:2776`, `:2794`); `resolvedora_valida` permite conversa, sem dia e módulo não-DIA (`app/chamados/juizes.py:2197`, `:2207`, `:2211`). Portanto exclusão do derivador automático **não prova impossibilidade de ancoragem** pela porta genérica.

Os três nascimentos produtivos de disputa localizados escolhem pai `disputa_supervisao`: núcleo comum (`app/chamados/services/disputa_emissao.py:568`, `:587`), agrupador manual (`:1105`, `:1123`) e âncora ausente (`:1340`, `:1358`). Isso demonstra o nascimento desses caminhos; não demonstra categoria de toda FK histórica em produção.

## 11/11 21:51 · sessao b3dfb46b440f · - · - · origem=Ronald (tmux)

PORTA B — matriz fatorada, somente leitura. Não executei testes; resultados de execução nesta sessão são `[nao verificado]`. [nao verificado]

**Juiz e relógio**

- O juiz registrado de “competência de hoje” é `ponto/janelas.py::janela_atual`: `app/docs/JUIZES.md:98`, `app/core/juizes.py:179`, `app/core/juizes.py:329`. A função responde à janela que contém a **data recebida**, respeitando o corte da empresa: `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`.
- O backfill pergunta a esse juiz, porém com `hoje` derivado de `--hoje`; o instante real `agora` é capturado separadamente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`, `:219`, `:226`.
- “Entregue” é outra pergunta: K6 aponta a `folha/export.py::competencia_entregue`, cujo predicado é ExportacaoDominio vigente e não invalidada; pagamento externo não é um campo consultado: `app/core/juizes.py:177`, `:327`; `app/folha/export.py:580`.
- Dentro do ato, há revalidação de entrega, não de competência corrente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:299`. A composição chamada resolve a competência **do dia do plano**, depois consulta K6: `app/chamados/services/vigia_fio.py:834`.
- A cerca não conhece relógio nem competência de hoje: confere alcance determinado, `comp == alvo` e `entregue is False`: `app/chamados/services/foto_e4b.py:145`.

**Forma exata das combinações**

Sejam `R(e)` a competência de `janela_atual(agora.date(), e)`, `A(e)` a competência de `janela_atual(hoje, e)` e `C` a competência pedida. A guarda permite continuar se, e somente se, `C == A(e)` para **cada empresa visível selecionada**; basta uma divergência para recusar o comando antes do censo: `app/chamados/management/commands/backfill_fio_morto_e4b.py:221`. Essa condição combina-se com a tabela de entrega e a tabela de empresa abaixo; assim cobre o produto de argv × estado × empresa sem assumir cortes de PROD `[nao verificado]`.

| `--hoje` | `--competencia`, relativa ao relógio real | Resultado da guarda de corrente | Prova existente |
|---|---|---|---|
| Ausente | Corrente `C=R(e)` para todas as empresas selecionadas | Passa; `hoje=agora.date()`: `backfill_fio_morto_e4b.py:219`, `:226` | Há teste com corrente simulada, mas o helper sempre fornece uma string `hoje`: `test_e4b_classe_e_memoria.py:646`; caso argv realmente ausente: **SEM PROVA** | [nao verificado]
| Ausente | Anterior | Recusa quando diferente de `R(e)`: `backfill_fio_morto_e4b.py:228` | RED testa `10/2026` contra corrente simulada `11/2026`: `test_e4b_c4v7_memoria.py:131` |
| Ausente | Exportada | “Exportada” não determina corrente; se `C != R(e)`, recusa; se igual, passa para entrega: `backfill_fio_morto_e4b.py:228`, `:253` | Exportada simulada corrente sai BANG sem porta: `test_e4b_classe_e_memoria.py:701`; argv ausente + janela real: **SEM PROVA** |
| Ausente | Paga fora do sistema | Rótulo não lido; se `C != R(e)`, recusa; se igual, passa para entrega: `backfill_fio_morto_e4b.py:228`, `:253`; `folha/export.py:580` | **SEM PROVA** específica de pagamento externo |
| Passado | Corrente real | Passa somente se o passado ainda cai em `C=A(e)` para todas; caso contrário recusa: `backfill_fio_morto_e4b.py:226` | **SEM PROVA** com janela real e passado |
| Passado | Anterior real | **Passa** se argv retrocede à janela dessa anterior; depois aplica o filtro de entrega: `backfill_fio_morto_e4b.py:219`, `:226`, `:257` | Teste existente exige aplicação de comp09 com agora 25/09 e argv 15/09: `test_e4_backfill.py:97`, `:109`, `:142`; ele prova a aceitação pretendida pelo teste, não a recusa exigida pelo aval | [nao verificado]
| Passado | Exportada | Passa a guarda se argv faz `C=A(e)`; entregue=True barra escrita depois: `backfill_fio_morto_e4b.py:226`, `:257`, `:302` | Entrega no ato é coberta, mas não essa combinação temporal exata: `test_e4_backfill.py:160`, `:223`; **SEM PROVA** de corrente real |
| Passado | Paga fora do sistema / entregue=False | Passa se argv faz `C=A(e)`; pagamento externo não muda K6; torna-se elegível a executar portas: `backfill_fio_morto_e4b.py:226`, `:257`, `:259`, `:313`; `folha/export.py:580` | **SEM PROVA** específica |
| Futuro | Corrente real | Passa somente se o futuro ainda cai em `C=A(e)` para todas; se cruzou a janela, recusa: `backfill_fio_morto_e4b.py:226` | **SEM PROVA** com janela real e futuro |
| Futuro | Anterior real | Regra continua `C=A(e)`; o código não compara a data com agora: `backfill_fio_morto_e4b.py:218`, `:219`, `:226` | **SEM PROVA** específica |
| Futuro | Exportada | Mesma igualdade contra argv; True barra escrita depois: `backfill_fio_morto_e4b.py:226`, `:257` | **SEM PROVA** temporal específica |
| Futuro | Paga fora do sistema / entregue=False | Mesma igualdade contra argv; False permite continuar, sem leitura de pagamento: `backfill_fio_morto_e4b.py:226`, `:257`; `folha/export.py:580` | **SEM PROVA** específica |
| Qualquer | Competência futura escolhida pelo próprio argv | Passa se `C=A(e)` para todas; não há comparação com `R(e)`: `backfill_fio_morto_e4b.py:226` | O RED usa janela fixa e não testa futuro via argv: `test_e4b_classe_e_memoria.py:588`; **SEM PROVA** |

Na tabela, os nomes curtos correspondem a `app/chamados/management/commands/backfill_fio_morto_e4b.py`, `app/chamados/tests/...` e `app/folha/export.py`.

| Estado de entrega, depois de a guarda aceitar | Resultado | Prova que morde |
|---|---|---|
| True no plano | `bang:exportada`; fora de `executaveis`: `app/chamados/management/commands/backfill_fio_morto_e4b.py:257`, `:259` | `app/chamados/tests/test_e4b_classe_e_memoria.py:701` |
| False no plano e no ato | Elegível a aplicar; exige ato em `_PORTA`, ausência de limbo/BANG e aprovação da cerca; pode seguir sem porta ou recusar por outros efeitos: `app/chamados/management/commands/backfill_fio_morto_e4b.py:259`, `:308`, `:313`, `:327`, `:366` | Apply/idempotência simulados: `app/chamados/tests/test_e4b_classe_e_memoria.py:681`; ORM aplica anterior real usando argv recuado: `app/chamados/tests/test_e4_backfill.py:142` |
| None no plano | `bang:indeterminada`; fora de `executaveis`: `app/chamados/management/commands/backfill_fio_morto_e4b.py:257`, `:259` | Cerca None: `app/chamados/tests/test_e4b_foto_memoria.py:24`; caso None do comando no plano: **SEM PROVA** específica |
| False no plano → True no ato | Rollback do savepoint antes da porta; BANG exportada: `app/chamados/management/commands/backfill_fio_morto_e4b.py:301` | Spy exige nenhuma entrada do dia em `_executar_dia`: `app/chamados/tests/test_e4_backfill.py:229`; mutante: `app/chamados/tests/test_e4b_c4v7_memoria.py:442` |
| False no plano → None no ato | Rollback antes da porta; BANG indeterminada: `app/chamados/management/commands/backfill_fio_morto_e4b.py:302` | **SEM PROVA** específica do comando |
| Linha alterada alcança True / None / outra competência | Rollback pela cerca: `app/chamados/services/foto_e4b.py:157`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:327` | `app/chamados/tests/test_e4b_foto_memoria.py:18`, `:24`, `:73` |

`competencia_do_dia_entregue` só devolve None quando `dia is None`; para dia presente converte K6 em bool. O plano lê `dia` com `date.fromisoformat`, portanto None na coluna de entrega do plano não é um estado normalmente produzido por esse carregamento; None de alcance da foto continua possível e tratado: `app/chamados/services/vigia_fio.py:834`, `:841`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:251`; `app/chamados/services/foto_e4b.py:157`.

| `--empresa` | Universo/efeito |
|---|---|
| Ausente | Guarda percorre todas visíveis; todas devem concordar com `C`; corte de cada empresa pode diferir: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`; `app/ponto/janelas.py:36` |
| Uma ou várias visíveis | Guarda apenas selecionadas; uma divergência recusa todo comando: `app/chamados/management/commands/backfill_fio_morto_e4b.py:224`, `:228` |
| Apenas inexistente/invisível | Nenhuma empresa passa na guarda; censo e complemento fazem interseção com visíveis, produzindo universo vazio, sem executar portas: `app/chamados/management/commands/backfill_fio_morto_e4b.py:224`, `:164`; `app/chamados/services/vigia_fio.py:566`; validação explícita de pk solicitado: **SEM PROVA** |
| Visíveis com cortes que geram competências diferentes no mesmo `hoje` | Uma única `C` não satisfaz todas; recusa na primeira divergência: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`, `:228`; cortes: `app/ponto/janelas.py:50` |

Empresas visíveis são ativas e excluem CNPJ diagnóstico: `app/colaboradores/queries.py:78`. Testes de `--empresa` ausente, seleção mista e cortes divergentes: **SEM PROVA** localizada; o helper de memória fixa empresa `[2]` e janela única: `app/chamados/tests/test_e4b_classe_e_memoria.py:585`, `:588`, `:646`.

**Ponto da prova que não morde o bloqueante**

O RED `test_RED_apply_so_competencia_corrente_falante` só troca `competencia`; usa `janela_atual=lambda *a: (..., 20/11/2026)`, indiferente à data argv. Seu mutante remove a comparação `if comp != corrente`, cobrindo a existência da comparação, sem distinguir relógio real de `--hoje`: `app/chamados/tests/test_e4b_c4v7_memoria.py:131`, `:449`; `app/chamados/tests/test_e4b_classe_e_memoria.py:588`.

Mais forte: a fixture ORM congela agora em 25/09, documenta que isso é comp10, injeta 15/09 para aplicar comp09 e exige escrita; há expectativa de teste favorável ao comportamento contornável: `app/chamados/tests/test_e4_backfill.py:27`, `:97`, `:109`, `:144`, `:147`.

Dependências de uma volta B, para a consolidação: guarda de produção em `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`; expectativa temporal do helper ORM em `app/chamados/tests/test_e4_backfill.py:97`; RED corrente e mutante em `app/chamados/tests/test_e4b_c4v7_memoria.py:131`, `:449`; double de janela e passagem de argv em `app/chamados/tests/test_e4b_classe_e_memoria.py:588`, `:646`. A cerca e K6 não respondem competência de hoje e não precisam receber essa pergunta para caracterizar B: `app/chamados/services/foto_e4b.py:145`; `app/folha/export.py:556`.

## 11/11 21:51 · sessao ccb68bd8192e · - · - · origem=Ronald (tmux)

Somente leitura; não executei testes. [nao verificado]

- **FILA c — confirmação limitada:** `test_i_terminal_para_terminal_passa` termina exigindo `em_analise` e trilha `['fechado', 'em_analise']`; não chama reconciliador depois desse estado. O trecho encerra verificando nota e `materializada_em=None`. Portanto o teste prova a reabertura, não mede o flap posterior. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `:222`, `:225`, `:226`, `:229`) O recibo c4 já nomeia esse teste, mas não documenta ensaio posterior contra o reconciliador. (`app/docs/FIO-E0-V4-E4B-c4.txt:121`)

- **FILA d — confirmado no código, com distinção:** 27→28 e 59→60 estão descritos como **medidos pela sessão**, não `[nao verificado]`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:117`, `:122`, `:127`, `:132`) Os valores 213 e 269 estão mantidos como tetos; sua expectativa após cura aparece `[nao verificado]`, e o custo da carga canônica continua `PERGUNTA DE DESENHO`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:184`, `:192`, `:193`, `:196`, `:204`, `:207`, `:208`, `:210`) Logo não generalizar o rótulo `[nao verificado]` aos quatro deltas.

- **FILA e — execução histórica não verificável; roteiro explica lacuna:** `retorno_antecipado` está definido com assertion-alvo `"S128 nao carimbou a morta"`. (`app/chamados/tests/e4b_c6_verificar.py:45`) `ensaios(c9)` inclui c8, c8 inclui c7, porém c7 retorna somente casos/mutantes C7 e não inclui o ramo padrão c6, onde estão `retorno_antecipado` e `sem_pauta`. (`app/chamados/tests/e4b_c6_verificar.py:320`, `:325`, `:328`, `:332`) `--orm --c9` usa esse roteiro. (`app/chamados/tests/e4b_c6_verificar.py:388`) A ausência de rerun independente depois c7–c9 continua **[nao verificado]**; `c6_orm.log:15,38` é apenas referência do anexo opus, não log disponível para inspeção.

- **FILA f — a referência do opus diverge da cópia:** E4B está em `app/docs/BACKLOG.md:1875`, com `padrao=Parallel Change / Expand-Contract, fase BACKFILL`; `:1889` é outra obra. O recibo c4 registra literalmente `"antes de substituir append-only"` e a substituição para Parallel Change. (`app/docs/FIO-E0-V4-E4B-c4.txt:33`, `:9`) Os recibos c8/c9 declaram `Single Writer Principle`. (`app/docs/FIO-E0-V4-E4B-c8.txt:5`, `app/docs/FIO-E0-V4-E4B-c9.txt:5`) Assim há evidência de três rótulos ao longo dos documentos, mas **não** de `append-only` na linha E4B atual. Conteúdo específico do `recibo_c6.txt` e merge que preservaria append-only: **[nao verificado]**.

Para classificar pelos anexos: FILA c/f não são nomeadas especificamente nas perguntas fornecidas; d tem tema de teto explicitamente na c4 (`preverif_c4.q`, item 8); e envolve prova histórica/RED, com tema genérico nas c6–c9 e alegações de execução nos cabeçalhos. Atribuir c/e/f a falha de leitura sem separar pergunta e prova seria extrapolação. [nao verificado: classificação final depende da numeração literal dos anexos pelo agente principal]

## 11/11 21:50 · sessao 2039fd776329 · - · - · origem=Ronald (tmux)

Caminhos abaixo relativos à raiz, todos somente lidos.

| FILA | Cruza A/B? | Constatação e prova |
|---|---|---|
| (a) `q.data_evento` cru versus dia canônico | **A e B** no reabridor compartilhado: S143 chama `reabrir_credor_de_pergunta_viva` (`app/chamados/services/disputa_emissao.py:1945`); o reabridor avalia toda carga usando `competencia_do_dia_entregue(q.data_evento, ...)` (`:1835`). A chamada do backfill ao mesmo reabridor deve ser citada pelo agente B. | O juiz canônico devolve `None` para conversa (`app/chamados/juizes.py:332`); agrupador pula `None` (`app/chamados/services/fecho_runtime.py:114`). Assim, recusa coletada de conversa/sem dia não gera Pauta (`:115`). Testes confirmam a exclusão no agrupador, **não** a coerência ponta a ponta com o reabridor: `app/chamados/tests/test_e4b_c6_memoria.py:173`; ORM conversa `app/chamados/tests/test_e4b_c6_orm.py:173`. |
| (b) uma Pauta viva por competência | **A**, mesma função `_escrever_pautas` (`app/chamados/services/fecho_runtime.py:92`). Fora do contexto runtime, coletor não recebe nada (`:18`). | Encontrada qualquer Pauta viva marcador+âncora, dá `continue`, sem incluir nova pergunta, chamado ou motivo (`:128`). Prova atual **consagra** essa dedup: teste entrega segunda pergunta `pk=999999` de outro colaborador/chamado na mesma competência e exige total 1 (`app/chamados/tests/test_e4b_c6_orm.py:205`, `:211`, `:214`). Não há assert de incorporação da segunda recusa nesse caso: **SEM PROVA** (`:212`). |
| (c) test_i acaba em `em_analise` | **A**, núcleo de estado e S143: teste invoca `_gravar_estado('fechado', via=ENCERRAR)` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:221`). | O teste exige estado final `em_analise`, trilha `['fechado','em_analise']` e nota “Reaberto: pergunta viva” (`:222`, `:225`, `:226`). Não roda reconciliador depois dessa reabertura; flap posterior **SEM PROVA** (corpo `:211`–`:228`). |
| (d) tetos aumentados | **A**, porque custo vem da mesma carga canônica do resolver/S143; os comentários produtivos apontam para a carga `chamado._ancoradas()` (`app/chamados/services/disputa_emissao.py:1938`). Nenhum cruzamento direto com `--aplicar` demonstrado. | Tetos 28 e 60 estão em `app/ponto/tests/test_selo_chokepoint_escrita.py:124`, `:134`; comentários atribuem +1 `_ancoradas` por S143 (`:121`, `:131`). Tetos 213 e 269 constam `:196`, `:210`, com “Esperado ... [nao verificado]” e “PERGUNTA DE DESENHO” (`:192`, `:193`, `:207`, `:208`). As medições narradas nos comentários não são execução conferida nesta cópia: `[nao verificado]`. |
| (e) mutante retorno antecipado | **A**, muta exatamente `_resolver_perguntas_por_chamado_resolvido`, substituindo `registrar_recusa` por retorno `{0,0}` (`app/chamados/tests/e4b_c6_verificar.py:45`). | O alvo é “S128 nao carimbou a morta”; testes ORM assertam mortas carimbadas, órfã fechada, chamado fechado, 0 reabrir e 1 Pauta (`app/chamados/tests/test_e4b_c6_orm.py:115`, `:118`, `:120`, `:122`, `:126`). `sem_pauta` também existe e troca retorno do agrupador por `return 0` (`app/chamados/tests/e4b_c6_verificar.py:48`). Reexecução ORM pós-c7–c9: `[nao verificado]`; `c6_orm.log` ausente no inventário local. O recibo c10 **narra** protocolo memória c9 com 17 ensaios após GREEN (`app/chamados/tests/e4b_c10_entrega.txt:75`), não prova ORM pós-c9 (`:5`). |
| (f) três padrões / merge BACKLOG | **Não cruza campo ou função de A/B**: são metadados de obra. | **Divergência do anexo com a cópia:** linha E4B da cópia é `app/docs/BACKLOG.md:1875`, já `Parallel Change / Expand-Contract, fase BACKFILL`; linha `:1889` é outro BO. `append-only` está na obra mãe `FIO-E0-V4-E4-ESCRITORES` (`:1866`). Recibos c8/c9 registram `Single Writer Principle` (`app/docs/FIO-E0-V4-E4B-c8.txt:5`; `app/docs/FIO-E0-V4-E4B-c9.txt:5`). `recibo_c6` não encontrado; conteúdo dele `[nao verificado]`. |

Prova de isolamento do passivo já existente: o ORM prepara chamado encerrado, chama save/S143, resolver direto e `fechar_beco`, e exige nenhuma Pauta (`app/chamados/tests/test_e4b_c6_orm.py:159`). Isso cobre esses atos concretos; não cobre todos os chamadores de reconciliador/fantasmas citados pelo aval (`:167`–`:170`).

Dependências para o pai consolidar, sem prescrever implementação:

1. Estado/dia/competência e fronteira do runtime precedem a revisão das células de prova: reabridor compartilhado (`app/chamados/services/disputa_emissao.py:1805`), agrupador (`app/chamados/services/fecho_runtime.py:92`), duas bordas atuais (`app/chamados/tests/test_e4b_c6_orm.py:90`).
2. Depois, prova de cada porta humana e reexecução dos mesmos mutantes sobre a árvore final: catálogo dos mutantes (`app/chamados/tests/e4b_c6_verificar.py:44`); testes de transições e trilha (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`).
3. Tetos só podem ser avaliados sobre a árvore final; a cópia registra os quatro tetos e duas perguntas em aberto (`app/ponto/tests/test_selo_chokepoint_escrita.py:124`, `:134`, `:193`, `:208`).

Arquivos de prova diretamente implicados por (a)–(e): `app/chamados/tests/test_e4b_c6_memoria.py:92`, `app/chamados/tests/test_e4b_c6_orm.py:148`, `app/chamados/tests/e4b_c6_verificar.py:44`, `app/chamados/tests/test_e4b_c6_protocolo.py:128`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `app/ponto/tests/test_selo_chokepoint_escrita.py:124`. Fechar a lista com os arquivos de B e dos demais caminhos de A exige as respostas dos outros agentes `[nao verificado]`.

Possível pergunta de lei a **não confundir com bug técnico**: quando já existe Pauta viva marcador+competência, a recusa posterior precisa constar nessa Pauta, ou a primeira entrega basta? Código atual pula a posterior (`app/chamados/services/fecho_runtime.py:128`); teste exige só unicidade (`app/chamados/tests/test_e4b_c6_orm.py:214`); o aval fornecido pede Pauta ancorada na competência, mas não especifica acumulação `[nao verificado]`.

## 11/11 21:50 · sessao 8d0587f714b2 · - · - · origem=Ronald (tmux)

Evidências da cópia, somente leitura:

- **B1:** adaptador em `app/chamados/services/acoes_chamado.py:316` e `app/chamados/services/fio.py:722`; demais transições diretas nos cinco pontos de `acoes_chamado.py:70/108/149/222/277` e `app/colaboradores/views.py:2068`. Coleta exige contexto (`app/chamados/services/fecho_runtime.py:18-20`), instalado pelo adaptador (`:153-159`). Registro de fila literal: “fechar() fora das duas bordas continua na transicao direta e sem coletor instalado nao entrega Pauta” (`app/docs/FIO-E0-V4-E4B-c8.txt:93`).
- **B2:** `--hoje` aceito (`app/chamados/management/commands/backfill_fio_morto_e4b.py:199`); relógio real em `:218`, data substituível em `:219`; autorização compara competência com `janela_atual(hoje, empresa)` (`:226-230`). Revalidação posterior consulta somente entrega (`:301-305`). Cerca exige alvo e `entregue=False`, sem conferir competência corrente real (`app/chamados/services/foto_e4b.py:145-163`). Execução do bypass: [nao verificado].
- **FILA a:** recusa usa `q.data_evento` cru (`app/chamados/services/disputa_emissao.py:1835-1836`); ausência de dia retorna `None` (`app/chamados/services/vigia_fio.py:834-835`), que satisfaz `is not False`. Agrupador usa juiz temporal e pula `None` (`app/chamados/services/fecho_runtime.py:114-116`); juiz devolve `None` para conversa (`app/chamados/juizes.py:332-336`). Registro de fila literal: “Leitores crus preexistentes de data_evento: FILA” (`app/docs/FIO-E0-V4-E4B-c8.txt:96`).
- **FILA b:** Pauta viva com marcador/âncora causa `continue` (`app/chamados/services/fecho_runtime.py:128-130`). Registro literal: “Reentrega nao agrega novos IDs: continue na Pauta viva” (`app/docs/FIO-E0-V4-E4B-c8.txt:92`).
- **Âncora de B1:** `return p.chamado_destino_id or (d.chamado_id if d is not None else None)` (`app/chamados/services/fio.py:117`).

Os registros acima estão sob “6. FILA solicitada / sem cura nesta volta” (`app/docs/FIO-E0-V4-E4B-c8.txt:90`); identificação pessoal de quem solicitou essa fila: [nao verificado].

## 11/11 21:50 · sessao 648439db674a · - · - · origem=Ronald (tmux)

Auditoria normativa:

- `CLAUDE.md:363-365`: “Sem a hipotese de quem pergunta”; exige QUEM ESCREVE/QUEM LE.
- `CLAUDE.md:372-374`: achado descartado, inclusive “fila”, exige “1 linha de motivo” e anexo ao opus.
- `CLAUDE.md:379-384`: classificação por achado e auditoria “PRIMEIRO quem perguntou e quem triou”.
- Autoria pessoal/modelo de quem escreveu cada `.q` e decidiu cada triagem: **[nao verificado]**. Os cabeçalhos identificam obra, papel e voltas, sem assinatura; o recibo não identifica automaticamente o triador. `app/docs/FIO-E0-V4-E4B-c8.txt:1` identifica apenas “recibo do construtor”.

Evidência local de descarte:

- `app/docs/FIO-E0-V4-E4B-c8.txt:90`: “6. FILA solicitada / sem cura nesta volta”.
- `:92`: “Reentrega nao agrega novos IDs: continue na Pauta viva”.
- `:93`: “fechar() fora das duas bordas continua na transicao direta e sem coletor instalado nao entrega Pauta”.
- `:96`: “Leitores crus preexistentes de data_evento: FILA [...] Exaustividade [nao verificado].”
- `app/docs/FIO-E0-V4-E4B-c9.txt:93`: sete comparações condicionais permanecem em fila, guards/FKs/ciclos concretos não verificados.
- `:103`: prova concorrente, criação concorrente do contador, suíte e opus em fila. [nao verificado]

A linha fornecida `triagem_c9_c10.txt:40` descarta **o ciclo C↔Cel da c9**, como “HERDADO, nao regressao”, sob “CONGELA-O-20: sem numero de celula/fio morto”. Não aplicar esse motivo automaticamente a B1/FILA a,b: essa atribuição não consta da linha.

Padrões e divergência de localização:

- **Na cópia atual**, E4B está em `app/docs/BACKLOG.md:1875`, com “padrao=Parallel Change / Expand-Contract, fase BACKFILL”.
- **Na cópia atual**, `app/docs/BACKLOG.md:1889` é BO-0611-05-RESTO-DA-CLASSE, não E4B.
- `app/docs/FIO-E0-V4-E4B-c4.txt:33`: “R5: assert do padrao da linha E4B deu rc=1 antes de substituir append-only”.
- `app/docs/FIO-E0-V4-E4B-c4.txt:9` e `:102`: Parallel Change / Expand-Contract, BACKFILL.
- `app/docs/FIO-E0-V4-E4B-c8.txt:5` e `app/docs/FIO-E0-V4-E4B-c9.txt:5`: Single Writer Principle.
- `recibo_c6.txt` e arquivos `.q/.r` não encontrados como arquivos locais na busca; usar os anexos do prompt e numeração própria explicitada. Merge e procedência “append-only do main” alegados pelo opus: **[nao verificado]** pela cópia.

## 11/11 21:50 · sessao 4902a7374e30 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVADO.** Há bloqueantes de coerência entre leitores e de significado de `folga`; a classificação incondicional de `troca_de_plantao` também não preserva as condições do corte existente. (`app/escala/utils.py:1220`; `app/ponto/status_dia.py:141`; `app/ponto/precedencia.py:215`; `app/docs/CORTES.md:187`)

A análise abaixo é da cópia e dos anexos fornecidos. Execuções RED/GREEN/mutante, identidade Git dos commits e situação efetiva da produção: **[nao verificado]**. Não rodei suíte nem acessei banco/log. [nao verificado]

1. **Cumprimento do aval**

| Exigência | Avaliação | Evidência e limite |
|---|---|---|
| Mapear cada tipo ativo, um a um | **Cumprido em parte** | Os 17 códigos enumerados no censo fornecido estão explícitos; o mapa também cobre todos os 17 `Ausencia.TIPO_CHOICES`, as 17 sementes e os três históricos indicados. Cobertura de outro cadastro ativo não apresentado: **[nao verificado]**. (`app/ponto/precedencia.py:53`; `app/ponto/models.py:661`; `app/ponto/migrations/0052_seed_tipoausencia.py:9`) |
| Justificar cada faixa em uma linha no RELATO | **Cumprido em parte** | Há comentários individuais no mapa. Registro correspondente da c3 no RELATO: **não achei [nao verificado]**. (`app/ponto/precedencia.py:53`; `app/ponto/precedencia.py:83`) |
| Recusa nomeada fora do mapa | **Cumprido** | `TipoSemFaixa(ValueError)`, mensagem com código e acesso sem default; não herda de `VetoDePrecedencia`. (`app/ponto/precedencia.py:96`; `app/ponto/precedencia.py:100`) |
| Contador por tipo ativo, mesmo sem lançamento | **Cumprido** | Percorre cadastro, filtra `ativo`, chama a autoridade real e captura especificamente `TipoSemFaixa`; publica quantidade, códigos, “Esperado 0” e URL. (`app/ponto/services/vigia_ausencia.py:144`; `app/ponto/services/vigia_ausencia.py:155`; `app/ponto/services/vigia_ausencia.py:168`) |
| Contador com caso que morde | **Cumprido no código do teste** | Tipo fantasma ativo, nenhuma `Ausencia`, exige 1; inativação exige 0. Execução: **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:108`) |
| RED pk 5356 | **Cumprido em parte** | Persiste ausência aprovada `folga`, chama A2 real e exige faixa/rótulo corretos. Não atravessa grade/lavra/tela, onde restam divergências. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92`; `app/escala/utils.py:1220`; `app/ponto/status_dia.py:141`) |
| RED tipo fantasma | **Cumprido no código do teste** | Chamada real exige exceção com código. Execução: **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:13`) |
| Mutante “default de volta” | **Cumprido em parte** | As asserções distinguem o mutante: fantasma deixa de levantar e contador deixa de contar. Os dois failures informados são compatíveis com isso; comando/log efetivo: **não achei [nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/services/vigia_ausencia.py:163`) |
| PROIBIDO campo novo de cadastro | **Cumprido no diff fornecido** | A implementação consulta os campos existentes `codigo/ativo`; não acrescenta faixa ao modelo. (`app/ponto/services/vigia_ausencia.py:160`; `app/ponto/models.py:577`; `app/ponto/models.py:607`) |
| PROIBIDO fallback | **Cumprido na origem; em parte nos efeitos** | `faixa_de` recusa. Capturas antigas agora transformam essa recusa em `False`, `trabalho` ou mapas vazios; detalhado abaixo. (`app/ponto/precedencia.py:103`; `app/ferias/situacao.py:95`; `app/escala/services/semana_do_posto.py:178`; `app/ponto/services/espelho.py:644`) |
| PROIBIDO juiz novo | **Cumprido no diff fornecido** | A classificação continua na A2 registrada; vigia consulta essa autoridade. Restam derivações anteriores divergentes. (`app/core/juizes.py:186`; `app/ponto/services/vigia_ausencia.py:162`; `app/escala/utils.py:1220`) |
| PROIBIDO tocar exportada | **Cumprido quanto ao diff fornecido** | As alterações produtivas são classificação e medição; não incluem operação sobre exportada. Invariância financeira/TXT após eventual pouso: **[nao verificado]**. O fechamento tem regras próprias de cobertura/efeito. (`app/ponto/precedencia.py:100`; `app/ponto/services/vigia_ausencia.py:155`; `app/ponto/services/fechamento.py:693`; `app/folha/export.py:4`) |

O requisito final de suíte inteira e opus final continua pendente conforme o enunciado; comprovação local: **[nao verificado]**. Isso é pendência de **PRONTO**, além dos bloqueantes de código desta preverificação. [nao verificado]

2. **Quem escreve, quem lê e propagação**

| Elemento | Quem escreve/define | Quem lê |
|---|---|---|
| `_FAIXA_DE_TIPO` | Literal em `app/ponto/precedencia.py:52` | Único acesso produtivo encontrado: `faixa_de`, em `app/ponto/precedencia.py:103`. |
| `faixa_de` | Definição em `app/ponto/precedencia.py:100` | Quatro chamadas produtivas: `app/ponto/precedencia.py:406`, `app/ponto/precedencia.py:522`, `app/ponto/precedencia.py:543`, `app/ponto/services/vigia_ausencia.py:162`. Patch temporário em teste: `app/ponto/tests/test_lastro_faixa_sem_default.py:144`. |
| `TipoSemFaixa` | Classe em `app/ponto/precedencia.py:96`; lançada em `app/ponto/precedencia.py:105` | Captura específica no vigia: `app/ponto/services/vigia_ausencia.py:163`; testes de recusa e mock: `app/ponto/tests/test_lastro_faixa_sem_default.py:16`, `app/ponto/tests/test_lastro_faixa_sem_default.py:140`, `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. |
| `_nome_do_tipo` | Definição em `app/ponto/precedencia.py:108` | Veto em `app/ponto/precedencia.py:492`; teste em `app/ponto/tests/test_lastro_faixa_sem_default.py:59`. Retorna display ou código cru, sem consultar faixa: `app/ponto/precedencia.py:122`, `app/ponto/precedencia.py:128`. |
| `tipos_ativos_sem_faixa` | Derivação local em `app/ponto/services/vigia_ausencia.py:158`; publicação em `app/ponto/services/vigia_ausencia.py:168` | Agregados em `app/ponto/services/vigia_ausencia.py:231`; revisão em `app/ponto/services/vigia_ausencia.py:259`; API em `app/api/views_mensageria.py:1417`; teste em `app/ponto/tests/test_lastro_faixa_sem_default.py:90`. |

**Todas as chamadas produtivas de `faixa_de` encontradas:**

| Chamada | Tipo fora do mapa |
|---|---|
| `fatos_do_periodo`: `app/ponto/precedencia.py:406` | Propaga, interrompendo a **janela inteira**. Classifica todas as ausências antes de ordenar; outra ausência conhecida mais forte não evita a recusa. (`app/ponto/precedencia.py:404`; `app/ponto/precedencia.py:421`) |
| `veto_de_lancamento`: `app/ponto/precedencia.py:522` | Propaga antes do laço das folgas. Um veto por sobreposição pode acontecer antes da consulta à faixa. (`app/ponto/precedencia.py:488`; `app/ponto/precedencia.py:531`) |
| Texto do veto: `app/ponto/precedencia.py:543` | Sem captura local. Um código desconhecido estável já teria recusado na chamada anterior, portanto não chega aqui. (`app/ponto/precedencia.py:522`; `app/ponto/precedencia.py:539`) |
| Vigia: `app/ponto/services/vigia_ausencia.py:162` | Captura `TipoSemFaixa`, acrescenta o código e publica o problema. Outras exceções propagam. (`app/ponto/services/vigia_ausencia.py:163`; `app/ponto/services/vigia_ausencia.py:168`) |

Import local do vigia: `app/ponto/services/vigia_ausencia.py:157`. Outro chamador direto por apelido/import local: **não achei**. Chamada construída dinamicamente: **[nao verificado]**.

As chamadas diretas de testes são fantasma, enumeração do mapa, choices, sementes, controle `abono` e delegação do wrapper mockado; neste último, a referência real foi guardada antes do patch. (`app/ponto/tests/test_lastro_faixa_sem_default.py:17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:40`; `app/ponto/tests/test_lastro_faixa_sem_default.py:47`; `app/ponto/tests/test_lastro_faixa_sem_default.py:54`; `app/ponto/tests/test_lastro_faixa_sem_default.py:136`; `app/ponto/tests/test_lastro_faixa_sem_default.py:141`)

**Depois da chamada indireta:**

| Leitor | Captura e resultado |
|---|---|
| `fatos_do_dia` | Delega ao período e propaga. (`app/ponto/precedencia.py:281`) |
| Calendário | Warning com traceback e relançamento; não entrega calendário degradado. (`app/colaboradores/services/calendario.py:507`; `app/colaboradores/services/calendario.py:515`) |
| Espelho, inclusive alias `_fatos_periodo` | Captura genérica, warning e continuação com ambos os mapas vazios, pois a primeira atribuição não terminou. Toda a janela perde essa alimentação. (`app/ponto/services/espelho.py:644`; `app/ponto/services/espelho.py:646`; `app/ponto/services/espelho.py:651`) |
| Palavra do espelho | Recebe `_fatos_do_dia=None` e pula a tradução de palavra/cor/veredito. (`app/ponto/services/espelho.py:870`; `app/ponto/services/espelho.py:339`) |
| Cartório | `aplicar_precedencia_na_celula` propaga antes de carimbar; `julgar_celula` captura, loga e retorna o resultado anterior, sem atualizar esse carimbo. (`app/ponto/services/cartorio.py:1985`; `app/ponto/services/cartorio.py:2032`; `app/ponto/services/cartorio.py:2040`) |
| Situação de férias | Captura silenciosamente e retorna `False` para descanso; pode omitir data do aviso de véspera. (`app/ferias/situacao.py:93`; `app/ferias/situacao.py:108`) |
| Semana do posto | Captura silenciosamente e retorna **`trabalho`**; candidato previsto sem batida vai para **buracos**, em vez de explicado por `atestado`. (`app/escala/services/semana_do_posto.py:175`; `app/escala/services/semana_do_posto.py:151`) |
| Status do dia | Propaga na alimentação da lavra e nos caminhos sem lavra. (`app/ponto/status_dia.py:181`; `app/ponto/status_dia.py:298`; `app/ponto/status_dia.py:304`) |
| Folha sem furo | Propaga quando consulta a janela das lâmpadas apagadas. (`app/folha/services/sem_furo.py:20`; `app/folha/services/sem_furo.py:25`) |
| Dia do colaborador | Propaga localmente. (`app/ponto/services/dia_do_colab.py:15`) |
| Deslogados / faltas de hoje / abriu e não bateu | Propagam localmente, quando alcançam a consulta. (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`) |
| Detector / reavaliação feriado-folga | Propagam localmente. (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`) |

Os tradutores do veto capturam somente `VetoDePrecedencia`; `TipoSemFaixa` escapa, sem virar “sobreposição”. As chamadas de folgas não têm essa captura local. (`app/ponto/services/ausencia.py:382`; `app/ponto/services/declarar_dia.py:121`; `app/ferias/models.py:438`; `app/ferias/views.py:255`; `app/escala/folgas.py:310`; `app/escala/folgas.py:336`)

**Sim, há degradação agora alcançável:** `trabalho`, `False` e mapas vazios. As capturas são anteriores e foram deliberadamente mantidas pelo brief; não atribuo ao diff a criação dessas capturas. Elas passam a receber a nova recusa. (`app/escala/services/semana_do_posto.py:178`; `app/ferias/situacao.py:95`; `app/ponto/services/espelho.py:650`; `app/ponto/precedencia.py:105`)

O contador segue a entrega existente: revisão → escritor semanal → pauta; comando e cron existentes. (`app/ponto/services/vigia_ausencia.py:259`; `app/pautas/semanal.py:202`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23`; `app/config/crons.py:739`)

Quanto aos insumos: `TipoAusencia.codigo/ativo` entram pela UI e pela porta `gravar_tipo_ausencia`, além da semeadura; `Ausencia.tipo` é escrito na criação/edição da porta existente. A porta de cadastro **continua sem validar faixa**, conforme a exclusão explícita do brief. (`app/ponto/views_tipos_ausencia.py:67`; `app/ponto/views_tipos_ausencia.py:89`; `app/ponto/services/ausencia.py:1301`; `app/ponto/migrations/0052_seed_tipoausencia.py:33`; `app/ponto/services/ausencia.py:342`; `app/ponto/services/ausencia.py:498`)

3. **Mapa, significado e precedência**

A regra operacional é esta: férias vencem todas; afastamento vence atestado/feriado/folga/trabalho; atestado vence feriado/folga/trabalho; folga vence somente trabalho. Empates de faixa conservam a ordem de entrada na ordenação. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:421`)

| Tipo | Faixa atribuída | Justificativa escrita no mapa |
|---|---|---|
| `ferias` | férias | Precedência própria. (`app/ponto/precedencia.py:53`) |
| `afastamento_inss` | afastamento | Afastamento acima de atestado. (`app/ponto/precedencia.py:54`) |
| `suspensao` | afastamento | Afasta comparecimento. (`app/ponto/precedencia.py:55`) |
| `licenca_maternidade` | afastamento | Segue licenças. (`app/ponto/precedencia.py:56`) |
| `licenca_paternidade` | afastamento | Segue licenças. (`app/ponto/precedencia.py:57`) |
| `licenca_luto` | afastamento | Analogia às licenças já mapeadas. (`app/ponto/precedencia.py:58`) |
| `atestado` | atestado | Fato humano. (`app/ponto/precedencia.py:59`) |
| `atestado_inss` | atestado | Fato humano, independentemente do pagador. (`app/ponto/precedencia.py:60`) |
| `abon_hr` | atestado | Abono parcial. (`app/ponto/precedencia.py:61`) |
| `abono` | atestado | Abono. (`app/ponto/precedencia.py:62`) |
| `atraso` | atestado | Ausência parcial lançada. (`app/ponto/precedencia.py:63`) |
| `aviso_home_office` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:64`) |
| `declaracao` | atestado | Comparecimento. (`app/ponto/precedencia.py:65`) |
| `declaracao_acompanhante_sem_ab` | atestado | Fato humano independentemente de abono. (`app/ponto/precedencia.py:66`) |
| `declaracao_de_acompanhante` | atestado | Acompanhante. (`app/ponto/precedencia.py:67`) |
| `desligamento` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:68`) |
| `falta` | atestado | Fato humano mesmo descontando. (`app/ponto/precedencia.py:69`) |
| `reciclagem` | atestado | Lançada como ausência. (`app/ponto/precedencia.py:70`) |
| `reducao_de_7_dias_do_aviso` | atestado | Redução lançada. (`app/ponto/precedencia.py:71`) |
| `saida_antecipada` | atestado | Ausência parcial lançada. (`app/ponto/precedencia.py:72`) |
| `treinamento` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:73`) |
| `atestado_medico` | atestado | Código histórico. (`app/ponto/precedencia.py:74`) |
| `obito` | atestado | Código histórico. (`app/ponto/precedencia.py:75`) |
| `outros` | atestado | Código histórico. (`app/ponto/precedencia.py:76`) |
| `folga_compensatoria` | folga | A própria folga do dia. (`app/ponto/precedencia.py:79`) |
| `feriado_folga` | folga | A própria folga do dia. (`app/ponto/precedencia.py:80`) |
| `folga` | folga | Folga lançada como ausência. (`app/ponto/precedencia.py:81`) |
| `folga_abonada` | folga | Folga independentemente de abono. (`app/ponto/precedencia.py:82`) |
| `troca_de_plantao` | folga | Presume representar o dia cedido. (`app/ponto/precedencia.py:83`) |

**Contradições e escolhas relevantes:**

- **`troca_de_plantao`: achei condição de negócio omitida.** O mapa presume dia cedido para qualquer ocorrência. O corte exige par cedido/assumido, confirmação administrativa e dois anexos; o tipo de ausência é pedido até confirmação. A A2 exclui somente rejeitadas e não verifica essas condições. Portanto, a equivalência incondicional a folga é uma hipótese de negócio adicional, embora tenha sido mandada pelo brief. Confirmação/natureza dos lançamentos raspados: **[nao verificado]**. (`app/ponto/precedencia.py:83`; `app/docs/CORTES.md:187`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:406`)

- **`licenca_luto`: escolha substantiva explícita.** Sai do comportamento anterior de atestado e passa a vencer atestado; o catálogo confirma licença paga de dois dias, mas não estabelece essa superioridade. O brief escolhe a analogia; fundamento adicional dessa precedência: **não achei [nao verificado]**. (`app/ponto/precedencia.py:58`; `app/ponto/catalogo/ausencias.py:98`; `app/ponto/precedencia.py:46`)

- **`desligamento`: contradição comprovada apenas pelo nome, não achei.** O comentário só repete “lançado como ausência”. Não há descrição específica desse código no catálogo/choices examinados; tratá-lo como término do vínculo seria inferência **[nao verificado]**. Sua classificação vence feriado/folga/trabalho. (`app/ponto/precedencia.py:68`; `app/ponto/catalogo/ausencias.py:87`; `app/ponto/models.py:661`; `app/ponto/precedencia.py:46`)

- **`aviso_home_office`: tensão semântica, sem prova de faixa errada.** O modelo diz “cumprido em home office”; o catálogo o mantém como ausência abonada. Trabalho efetivamente prestado versus dispensa de comparecimento nos eventos fornecidos: **[nao verificado]**. A faixa escolhida suprime trabalho/feriado/folga na A2. (`app/ponto/models.py:678`; `app/ponto/catalogo/ausencias.py:104`; `app/ponto/precedencia.py:64`; `app/ponto/precedencia.py:421`)

- **`atraso` e `saida_antecipada`: parcialidade não prova outra faixa.** O catálogo os declara parciais; efeito financeiro, duração e precedência são perguntas diferentes. A faixa atestado preserva o comportamento anterior desses códigos, mas a A2 não representa por essa faixa o restante trabalhável do dia. (`app/ponto/catalogo/ausencias.py:9`; `app/ponto/catalogo/ausencias.py:75`; `app/ponto/catalogo/ausencias.py:90`; `app/ponto/precedencia.py:63`; `app/ponto/precedencia.py:72`; `app/ponto/precedencia.py:406`)

- **`folga`, `folga_abonada`, `troca_de_plantao`: mudança operacional real.** Agora perdem para feriado e ganham somente de trabalho; anteriormente recebiam atestado pelo default informado no diff. O status sem lavra passa a `folga`, e o reavaliador passa a poder selecionar a cobrança como espúria. (`app/ponto/precedencia.py:81`; `app/ponto/precedencia.py:46`; `app/ponto/status_dia.py:312`; `app/chamados/services/feriado_reavaliar.py:33`)

Nenhuma dessas faixas, inclusive `afastamento`, passa a autorizar atravessar FolgaDia humana: a permissão continua exclusiva de férias. (`app/ponto/precedencia.py:521`; `app/ponto/precedencia.py:539`)

4. **Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | **Não achei juiz novo no diff. Achei decisão paralela anterior ainda divergente:** A2 atribui folga; a grade classifica toda ausência aprovada não-falta como `ausencia`, sem consultar a faixa. (`app/core/juizes.py:186`; `app/ponto/precedencia.py:81`; `app/escala/utils.py:1210`; `app/escala/utils.py:1220`) |
| Segundo escritor fora da porta | **Não achei introduzido.** A2 devolve fatos; carimbo continua pela porta da célula. O contador usa o escritor semanal existente. (`app/ponto/services/cartorio.py:1985`; `app/ponto/portas/celula.py:1076`; `app/ponto/services/vigia_ausencia.py:260`) |
| Relógio fora da borda | **Não achei relógio novo.** Testes DB congelam tempo e passam `HOJE`; há leituras anteriores quando data não é fornecida ao vigia. (`app/ponto/tests/test_lastro_faixa_sem_default.py:62`; `app/ponto/tests/test_lastro_faixa_sem_default.py:89`; `app/ponto/services/vigia_ausencia.py:58`; `app/ponto/services/vigia_ausencia.py:258`) |
| Derivação paralela de fonte única | **Achei.** Grade → fatos do status → lavra produz `ausencia`; ramo sem lavra lê A2 e produz `folga`. O carimbo de precedência posterior só altera `ata`, não corrige `status_dia`. (`app/escala/utils.py:1220`; `app/ponto/status_dia.py:189`; `app/ponto/status_dia.py:141`; `app/ponto/status_dia.py:312`; `app/ponto/portas/celula.py:1084`) |
| Selo/RED sem mordida | **Não achei ausência total de mordida.** Fantasma e contador distinguem o default. **Achei cobertura insuficiente da cura entre leitores:** pk 5356 testa somente A2. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/tests/test_lastro_faixa_sem_default.py:100`) |
| Campo com dois significados — ESMERIL | **Achei, concretamente.** `tipo_do_dia='folga'` significa tanto código de uma ausência humana quanto marcador genérico da folga automática. O predicado exclui esse valor para reconhecer folga humana. (`app/ponto/precedencia.py:81`; `app/ponto/precedencia.py:410`; `app/ponto/precedencia.py:

[truncado em 20000 de 24752 caracteres]

## 11/11 21:46 · sessao 363a49956f66 · - · - · origem=Ronald (tmux)

Achados principais para consolidação:

1. **Divergência estrutural entre A2 e grade/lavra: achei.** `folga`, `folga_abonada` e `troca_de_plantao` agora pertencem à faixa `folga`; A2 publica faixa e rótulo correspondentes. (`app/ponto/precedencia.py:81`, `app/ponto/precedencia.py:421`, `app/ponto/precedencia.py:430`) Entretanto, `_marcar_ausencias` consulta cobertura aprovada e transforma todo tipo diferente de `falta` em `tipo_dia='ausencia'`, sem consultar a faixa. (`app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2753`, `app/escala/utils.py:1188`, `app/escala/utils.py:1210`, `app/escala/utils.py:1220`) O cartório monta essa grade e passa seu `tipo_dia` para a lavra; sem batida, o status resultante é `ausencia`. (`app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1843`, `app/ponto/status_dia.py:189`, `app/ponto/status_dia.py:141`, `app/ponto/portas/celula.py:963`) Já o ramo sem lavra lê faixa `folga` e responde `folga`. (`app/ponto/status_dia.py:308`) Não achei sobreposição posterior que corrija isso: nos dois builders, FolgaDia entra antes da ausência e depois vem somente isenção antes dos demais cálculos. (`app/escala/utils.py:806`, `app/escala/utils.py:1844`, `app/escala/utils.py:1850`) É uma divergência demonstrável por leitura estática; reprodução integrada está **[nao verificado]**. A coerência entre leitores é exigida pela L-099. (`app/docs/LEIS.md:122`)

2. **ESMERIL-SEMANTICA: achei colisão concreta de `tipo_do_dia='folga'`.** A2 usa esse valor tanto para o código da `Ausencia` quanto para a folga genérica da escala; `folga_humana_ou_feriado` exclui expressamente `tipo_do_dia='folga'` para reconhecer a genérica. (`app/ponto/precedencia.py:81`, `app/ponto/precedencia.py:409`, `app/ponto/precedencia.py:428`, `app/ponto/precedencia.py:215`) Para uma `Ausencia(tipo='folga')` sem FolgaDia, o predicado retorna `False`; para `folga_abonada`, retorna `True`. (`app/ponto/precedencia.py:215`, `app/ponto/precedencia.py:437`) O status consulta esse predicado no veredito `indefinida`, cuja decisão distingue `folga` de `escala_suspeita`. (`app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:177`) Execução desse cenário está **[nao verificado]**.

3. **`troca_de_plantao`: hipótese de negócio acrescentada.** O comentário afirma que qualquer lançamento desse tipo representa o dia cedido. (`app/ponto/precedencia.py:83`) O corte exige ato com par cedido/assumido, confirmação administrativa e dois documentos; o tipo de ausência representa pedido até confirmação. (`app/docs/CORTES.md:187`) A2 exclui apenas rejeitadas e não verifica essas condições antes de classificar a ocorrência. (`app/ponto/precedencia.py:298`, `app/ponto/precedencia.py:406`) Assim, a classificação incondicional elimina distinção exigida pelo corte; natureza e confirmação dos lançamentos raspados estão **[nao verificado]**.

4. **Efeito de cada grupo do mapa:** férias vencem todas as outras faixas; afastamento vence atestado/feriado/folga/trabalho; atestado vence feriado/folga/trabalho; folga vence somente trabalho e perde para feriado. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:421`) Os grupos efetivamente declarados são:
   - `ferias`: `ferias`. (`app/ponto/precedencia.py:53`)
   - `afastamento`: `afastamento_inss`, `suspensao`, `licenca_maternidade`, `licenca_paternidade`, `licenca_luto`. (`app/ponto/precedencia.py:54`)
   - `atestado`: `atestado`, `atestado_inss`, `abon_hr`, `abono`, `atraso`, `aviso_home_office`, `declaracao`, `declaracao_acompanhante_sem_ab`, `declaracao_de_acompanhante`, `desligamento`, `falta`, `reciclagem`, `reducao_de_7_dias_do_aviso`, `saida_antecipada`, `treinamento`, `atestado_medico`, `obito`, `outros`. (`app/ponto/precedencia.py:59`)
   - `folga`: `folga_compensatoria`, `feriado_folga`, `folga`, `folga_abonada`, `troca_de_plantao`. (`app/ponto/precedencia.py:79`)

5. **Limites semânticos dos outros exemplos:** efeito financeiro, duração e parcialidade são perguntas distintas de precedência; o catálogo não fornece conversão automática desses atributos em faixa. (`app/ponto/catalogo/ausencias.py:9`, `app/ponto/catalogo/ausencias.py:75`, `app/ponto/precedencia.py:48`) Portanto, `atraso` e `saida_antecipada` serem parciais/SUPRIME não prova que devam receber outra faixa. (`app/ponto/catalogo/ausencias.py:90`, `app/ponto/precedencia.py:63`, `app/ponto/precedencia.py:72`) `aviso_home_office` é descrito como aviso “cumprido em home office”, mas mantido como ausência abonada; o sentido real dos eventos está **[nao verificado]**. (`app/ponto/models.py:678`, `app/ponto/catalogo/ausencias.py:104`) Para `desligamento`, fundamentação específica além do comentário genérico do mapa: **não achei**; sua natureza nos dados está **[nao verificado]**. (`app/ponto/precedencia.py:68`) `licenca_luto` recebe afastamento por analogia às demais licenças; o catálogo confirma licença paga de dois dias, mas não estabelece que deva vencer atestado. (`app/ponto/precedencia.py:58`, `app/ponto/catalogo/ausencias.py:98`)

6. **RED não cobre essas divergências.** A fixture cria diretamente a ausência `folga` e chama somente `fatos_do_dia`; não atravessa grade, status lavrado ou renderização. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92`) O setup não cria cadastro ativo de `folga`, e a semeadura também não o contém. (`app/ponto/tests/test_lastro_faixa_sem_default.py:67`, `app/ponto/migrations/0052_seed_tipoausencia.py:9`) Isso não invalida a mordida da classificação A2, mas deixa o comportamento dos demais leitores sem cobertura nesse teste. (`app/ponto/tests/test_lastro_faixa_sem_default.py:100`)

## 11/11 21:45 · sessao 0e55e745c27f · - · - · origem=Ronald (tmux)

Censo fechado por ocorrências estáticas na cópia; não achei chamada adicional por apelido de import nem import local além do vigia. Chamadas construídas dinamicamente: [nao verificado].

| Símbolo | Quem escreve/define | Quem lê |
|---|---|---|
| `_FAIXA_DE_TIPO` | Literal único em `app/ponto/precedencia.py:52` | Apenas `faixa_de`, por indexação direta, em `app/ponto/precedencia.py:103`. |
| `faixa_de` | Função em `app/ponto/precedencia.py:100` | Produção: `app/ponto/precedencia.py:406`, `:522`, `:543`; import local `app/ponto/services/vigia_ausencia.py:157`, chamada `:162`. |
| `TipoSemFaixa` | Classe `ValueError`, independente de `VetoDePrecedencia`, em `app/ponto/precedencia.py:96` | Lançada em `app/ponto/precedencia.py:105`; capturada especificamente pelo contador em `app/ponto/services/vigia_ausencia.py:163`; testes em `app/ponto/tests/test_lastro_faixa_sem_default.py:15`, `:16`, `:140` e `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. |
| `_nome_do_tipo` | Função em `app/ponto/precedencia.py:108` | Única chamada de produção no texto de veto, `app/ponto/precedencia.py:492`; teste `app/ponto/tests/test_lastro_faixa_sem_default.py:59`. Não consulta mais faixa: retorna display ou código cru (`app/ponto/precedencia.py:122`, `:128`). |
| `tipos_ativos_sem_faixa` | Derivado de todos os tipos cadastrados, filtrando ativo e perguntando à autoridade real (`app/ponto/services/vigia_ausencia.py:144`, `:159`, `:160`, `:162`, `:168`) | `linhas`, `acesos`, `total_aceso` em `app/ponto/services/vigia_ausencia.py:231`; revisão em `:259`; API `app/api/views_mensageria.py:1417`; testes `app/ponto/tests/test_lastro_faixa_sem_default.py:90`. |

Cada chamada de produção de `faixa_de`:

- `app/ponto/precedencia.py:406`: ausência desconhecida interrompe **a janela inteira**, sem captura local; nem mesmo outra ausência conhecida de faixa mais forte evita a recusa, pois todos os tipos são classificados antes da ordenação (`app/ponto/precedencia.py:404`, `:421`).
- `app/ponto/precedencia.py:522`: desconhecido recusa antes de percorrer folgas; propaga. Pode haver veto de sobreposição anterior, que ocorre antes da consulta de faixa (`app/ponto/precedencia.py:488`, `:522`).
- `app/ponto/precedencia.py:543`: chamada para texto do veto, sem captura local; desconhecido estável já teria recusado em `:522`, portanto não alcança este ramo (`app/ponto/precedencia.py:522`, `:539`).
- `app/ponto/services/vigia_ausencia.py:162`: captura **somente** `TipoSemFaixa`, agrega o código e publica quantidade/frases/URL; outras exceções propagam (`app/ponto/services/vigia_ausencia.py:163`, `:168`, `:175`).

Chamadas diretas de testes: fantasma real (`app/ponto/tests/test_lastro_faixa_sem_default.py:17`); classificação enumerada (`:40`); choices (`:47`); sementes (`:54`); controle conhecido (`:136`); wrapper do mock chamando a referência real guardada (`:141`). A recusa é esperada só no caso fantasma; nos demais controles reais inesperados ela falha o teste; o wrapper gera a recusa proposital de `abono`, e o vigia deve capturá-la (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`, `:139`, `:144`, `:147`).

Leitores indiretos e degradações:

| Leitor | Resultado para desconhecido na janela |
|---|---|
| `fatos_do_dia` | Delega e propaga (`app/ponto/precedencia.py:281`). |
| Calendário | Captura, warning com traceback e relança; não entrega calendário degradado (`app/colaboradores/services/calendario.py:507`, `:515`, `:519`). |
| Espelho | Captura genérica, warning e segue com **ambos mapas vazios**, porque a primeira atribuição falhou antes de retornar (`app/ponto/services/espelho.py:644`, `:648`, `:651`). `_fatos_do_dia` fica `None` (`:870`); tradução de palavra/cor/veredito é pulada (`:331`, `:339`, `:344`). Isso afeta toda a janela carregada, não só o dia desconhecido (`:648`). |
| Cartório | `aplicar_precedencia_na_celula` propaga antes de carimbar (`app/ponto/services/cartorio.py:1985`, `:1986`). `julgar_celula` captura, loga e retorna o julgamento anterior (`:2026`, `:2035`, `:2040`); precedência da ata não é atualizada nesse passo (`:1985`). |
| Férias | Captura silenciosamente e retorna `False` para descanso (`app/ferias/situacao.py:93`, `:96`); pode omitir o aviso de véspera de repouso (`:108`, `:110`). |
| Semana do posto | Captura silenciosamente e retorna **`trabalho`** (`app/escala/services/semana_do_posto.py:175`, `:179`); candidato previsto sem batida entra em **buracos**, antes explicado pela faixa `atestado` (`:151`, `:152`, `:157`). |
| Status do dia | Propaga nos pontos de alimentação (`app/ponto/status_dia.py:181`, `:298`, `:304`). |
| Folha sem furo | Propaga quando existem datas com lâmpadas apagadas (`app/folha/services/sem_furo.py:20`, `:25`). |
| Dia do colaborador | Propaga localmente (`app/ponto/services/dia_do_colab.py:15`). |
| Deslogados, faltas de hoje, abriu e não bateu | Propagam localmente (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`). |
| Detector e reavaliação feriado/folga | Propagam localmente (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`). |

Tradutores do veto capturam somente `VetoDePrecedencia`; `TipoSemFaixa` escapa desses tradutores sem ser rotulada sobreposição: `app/ponto/services/ausencia.py:382`, `app/ponto/services/declarar_dia.py:121`, `app/ferias/models.py:438`, `app/ferias/views.py:255`. Folgas delegam sem essa captura: `app/escala/folgas.py:310`, `:336`.

A entrega do contador usa a revisão/escritor existentes (`app/ponto/services/vigia_ausencia.py:259`, `:260`; `app/pautas/semanal.py:214`), comando existente (`app/ponto/management/commands/vigiar_familia_ausencia.py:23`, `:28`) e cron existente (`app/config/crons.py:739`). API direta publica inclusive as linhas zeradas e prefere a frase longa (`app/api/views_mensageria.py:1422`); outra API usa alias `_rev_dp` e transforma falha genérica de medição em erro explícito (`app/api/views_mensageria.py:2209`, `:2215`, `:2216`).

Há docstrings antigas dizendo “CINCO” no comando e API, embora o consumo seja genérico por linhas (`app/ponto/management/commands/vigiar_familia_ausencia.py:3`, `:16`, `:28`; `app/api/views_mensageria.py:1405`, `:1424`).

## 11/11 21:45 · sessao e61313ac5334 · - · - · origem=Ronald (tmux)

- **RED com mordida real:** `pk=5356`, tipo `folga`, aprovada, persiste uma `Ausencia` e chama `fatos_do_dia` sem mock; exige tipo/faixa `folga` e rótulo `Folga`, recusando `Atestado`. Não achei aprovação por motivo alheio à faixa nesse teste. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92-106`; `app/ponto/precedencia.py:405-430`)

- **Limite desse RED:** a fixture não cadastra `TipoAusencia('folga')`, não cria escala/célula e não passa por calendário/UI ou pela porta de criação. `folga` também não integra a semeadura. Portanto prova o contrato da A2 para a linha histórica, mas não valida o cadastro ativo nem um conflito concreto com feriado/trabalho. (`app/ponto/tests/test_lastro_faixa_sem_default.py:66-106`; `app/ponto/migrations/0052_seed_tipoausencia.py:9-27`; `app/ponto/precedencia.py:298-303`)

- **Fantasma e contador têm mordida independente:** chamada real `faixa_de('fantasma_xyz')` exige exceção e código; outro teste cria tipo ativo real, exige zero `Ausencia`, contador igual a 1 e código nas frases; inativar exige zero. Não são provas sustentadas apenas por mock. (`app/ponto/tests/test_lastro_faixa_sem_default.py:13-17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:108-121`)

- **Mutante default:** inferência pelo código: `_FAIXA_DE_TIPO.get(tipo, 'atestado')` faz falhar a recusa do fantasma e o contador real, porque este depende de capturar `TipoSemFaixa`; isso explica dois testes falhos se somente o módulo novo foi executado. O comando e a composição exata dessa execução permanecem **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16-17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/services/vigia_ausencia.py:157-168`)

- **Mock com finalidade delimitada:** o teste de autoridade intercepta somente `abono`, já existente no mapa e no cadastro; exige que o contador chame a autoridade e observe sua recusa. Esse teste mata o mutante “contador lê o dict”, pela ausência da chamada e/ou pelo contador zero; isoladamente não provaria a recusa real, já coberta nos testes anteriores. (`app/ponto/tests/test_lastro_faixa_sem_default.py:132-148`)

- **Semeadura não fica inteiramente invisível:** o teste exige igualdade exata entre tipos ativos e `seed.TIPOS` antes de exigir zero; outro teste percorre todos os tipos da seed pela `faixa_de` real. A seed cria tipos com `ativo=True`; o teste de autoridade exige explicitamente a presença de `abono`. (`app/ponto/tests/test_lastro_faixa_sem_default.py:49-54`; `app/ponto/tests/test_lastro_faixa_sem_default.py:123-136`; `app/ponto/migrations/0052_seed_tipoausencia.py:30-37`)

- **Teste anterior não foi apagado:** o calendário agora exige a recusa nomeada para o tipo cadastrado `desconto_teste`; continua chamando o calendário real. O retorno anterior de falta/resumo não é mais verificado nesse caso, pois a leitura interrompe antes. (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:43-60`)

- **Relógio novo solto: não achei:** classe DB inteira congelada e contador recebe `HOJE`; os testes simples não leem relógio. Existem relógios anteriores em `contadores` e `revisao_semanal`, acionados somente quando não recebem data; essas linhas não são acrescentadas pelo diff fornecido. (`app/ponto/tests/test_lastro_faixa_sem_default.py:62-64`; `app/ponto/tests/test_lastro_faixa_sem_default.py:87-90`; `app/ponto/services/vigia_ausencia.py:58`; `app/ponto/services/vigia_ausencia.py:258`)

- **Contador ligado à entrega existente:** aparece entre as oito linhas e entra nos `acesos`/`total_aceso`; revisão semanal entrega todas as linhas ao escritor semanal existente. O teste vizinho atualiza o número esperado para oito. Não achei segundo escritor introduzido por esse contador. (`app/ponto/services/vigia_ausencia.py:167-175`; `app/ponto/services/vigia_ausencia.py:228-233`; `app/ponto/services/vigia_ausencia.py:257-262`; `app/ponto/tests/test_a6_vigia_familia_ausencia.py:124-130`)

- **Achado B2 da reprova anterior:** há agora fixture explícita e testes que distinguem o comportamento curado; afirmar que toda a classe de fixture defeituosa foi eliminada exige o RED anterior e seu log, ausentes na cópia examinada **[nao verificado]**. Os testes atuais sustentam cura do caso, não uma prova universal sobre fixtures. (`app/ponto/tests/test_lastro_faixa_sem_default.py:66-106`)

## 11/11 21:19 · sessao 3c18a0449066 · PERGUNTA-DUPLA · construtor · origem=Code

Criado somente [test_pergunta_dupla_saida_noturna.py](/home/ronald/wt-codex-pergunta-dupla-211041/app/chamados/tests/test_pergunta_dupla_saida_noturna.py:1). O ensaio usa API real, relógio congelado por etapa, signals preservados e observador aditivo de nascimento; exige exatamente uma pergunta `orfao_14h`, não conversa, em D, incluindo linhas carimbadas na contagem (`chamados/tests/test_pergunta_dupla_saida_noturna.py:85,120,135,188,234`).

**Esperado no HEAD: VERDE.** Não encontrei produtor alcançável que entregue D+1 para essa saída ausente; não fabriquei contexto para obter RED. Execução Django e mordida do mutante: **[nao verificado]**, pois banco está vedado nesta construção. Sintaxe e compilação em memória passaram; isso não certifica comportamento (`chamados/tests/test_pergunta_dupla_saida_noturna.py:1`).

Censo dos escritores e da origem do dia:

| Escritor real | Valor e semântica |
|---|---|
| Detector `orfao_14h` | `data` vem de `resolver_data_evento` → juiz da batida: **D** (`chamados/detector_anomalias.py:41`; `escala/servico_jornada.py:508`). |
| Fábrica da célula | `contexto[motivo].data` recebe o dia da célula: **D** (`chamados/services/fabrica_da_celula.py:172`). |
| Pendentes adicionais | Consulta `dia_do_turno_da_batida`; grava contexto quando cria pergunta: **D** (`chamados/services/disputa_emissao.py:283,292,296`). |
| Alertas de turno | Grava `tm.data_turno` e timestamp da entrada; **D** vem do turno materializado (`ponto/management/commands/processar_alertas_turno.py:316,349`). |
| Alertas avançados | `ontem` é seleção pelo calendário, mas consulta trabalho, turno materializado e juiz da aberta antes de gravar: **D** neste cenário (`ponto/management/commands/processar_alertas_avancados.py:49,58,63,127`). |
| Detector de ausências/cartório | Grava o dia previsto da passada/célula e `horario_previsto`; não converte uma saída civil em dia do turno (`ponto/management/commands/detectar_ausencias.py:138,706`). |
| Retroativo de turno aberto | Grava `t.data_turno` e timestamp da entrada; **D**. É ato sem agenda de cron (`ponto/management/commands/emitir_turno_aberto_retroativo.py:88,128`; `config/crons.py:934`). |
| Reconciliador c2 | Copia o dia recebido do fio; não faz nova conversão pelo pareador. O chamador prioriza `p.data_evento` (`chamados/reconciliador.py:690,811,920,1567`). |
| Reconciliador de grade | Grava o dia da grade; cron está em sombra, sem `--apply` (`ponto/management/commands/reconciliar_grade.py:517`; `config/crons.py:462`). |
| Web: entrada bloqueada | `pendentes` contém dia **civil da entrada**, D para E às 19h. Exige outra tentativa de E bloqueada, ausente deste ensaio (`ponto/views.py:275,279,301`; `ponto/services/triagem_batida.py:180`). |

Os escritores civis encontrados não demonstram D+1 para a saída faltante:

- `data_batida` do detector descreve batida **existente** em folga ou fora da escala (`chamados/detector_anomalias.py:174,220`).
- A API grava `data_turno=localdate(batida.timestamp)` para desvio **leve**, categoria fora do mapa de regularização de saída (`api/views_core.py:710,717`; `chamados/models.py:3664`).
- `saida_sem_entrada` tem a data corrigida pelo juiz nas duas APIs e na triagem; cobra a entrada ausente e exige S existente (`api/views_core.py:741,747`; `api/views.py:747,753`; `ponto/services/triagem_batida.py:729,739`).
- Os contextos de justificativa copiam data informada ou `j.data_turno`; suas categorias não constam do mapa de saída (`api/views.py:1948,1968,1978`; `ponto/services/justificativa.py:297,302`; `chamados/models.py:3664`).

Quem escreve e quem lê os campos envolvidos:

| Campo | Escrita | Leitura |
|---|---|---|
| Pergunta: `motivo`, `conversa`, `data_evento` | `_nascer` recebe motivo/conversa; `fio.gravar_dia_do_fato` atribui a data; INSERT em `save` (`chamados/models.py:3014,3017`; `chamados/services/fio.py:800`). | Identidade/dedup, fábrica, fio e plantio (`chamados/models.py:2753`; `chamados/services/fabrica_da_celula.py:148`; `chamados/services/fio.py:1630`; `chamados/services/materializacao.py:280`). |
| `DisputaSupervisao.contexto[motivo].data` | Produtores acima; agregador persiste nascimento/merge (`chamados/services/disputa_emissao.py:559,562,590`). | Gerador, derivação do destino e reconciliador (`chamados/services/disputa_emissao.py:191,372`; `chamados/reconciliador.py:1560`). |
| `ChamadoColaborador.contexto_json`, cinco chaves | Produtores acima; atualização pela porta de contexto (`chamados/services/contexto.py:102`). | Parser, repasse sem origem e derivação do destino (`chamados/data_evento.py:16`; `chamados/services/disputa_emissao.py:1232,145`). |

Os dois sítios ainda preservam uma data recebida: leitura crua do contexto e parser sem origem; `dia_do_turno_da_celula` apenas declara o valor (`chamados/services/disputa_emissao.py:372,451,1232`; `ponto/turnos.py:2939`).

Comando para o Code:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_pergunta_dupla_saida_noturna --settings=config.settings.ci
```

Mutante descrito, sem aplicação: em `chamados/services/disputa_emissao.py:451`, trocar `_dtc_g(_dia_p)` por `_dtc_g(_dia_p + datetime.timedelta(days=1))`. Executar o mesmo comando; o repasse conserva D, enquanto o gerador entrega D+1. O assert deve reprovar as duas chaves; mordida executada **[nao verificado]** (`chamados/tests/test_pergunta_dupla_saida_noturna.py:33,240`).

Texto para o RELATO: **cura do +1 presente nesta cópia** (`escala/servico_jornada.py:508`); estado efetivamente “no ar” **[nao verificado]**. Atribuição histórica dos pares e situação atual da batida 147824 continuam **[nao verificado]**, no passivo BATIDA/histórico, L-099. Certificação permanece com Code e opus final.

## 11/11 21:07 · sessao 4be47f121636 · BUG-ORFAO-COM-PAR · construtor · origem=Code

Arquivo: [test_bug_orfao_com_par_ensaio.py:1](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:1). Classe candidata nomeada com `RED`; ensaio pela API real, relógio por passo e captura da pilha de nascimento ([arquivo:84](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:84), [arquivo:122](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:122)).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_bug_orfao_com_par_ensaio --settings=config.settings.ci
```

Expectativa estática no HEAD:

| ASSERT | Esperado | Por quê |
|---|---|---|
| A, após E | Zero perguntas nascidas, inclusive carimbadas | Turno ainda em curso: `escala/servico_jornada.py:385`. |
| B, 06:30:48 | Zero nascimentos após cartório e reconciliação | Fábrica exige marco vencido; S vence às 07:30: `chamados/services/fabrica_da_celula.py:69`, `ponto/services/cartorio.py:882`. |
| C, após S | Um turno fechado ancorado em D; zero nascimentos acumulados | Seleção pelo dia do turno: `ponto/turnos.py:1992`. |

**Execução comportamental no HEAD: [nao verificado].** Não executei Django porque esta obra proíbe banco. Não afirmo RED nem “classe não reproduz no HEAD”; essa conclusão depende da suite. [nao verificado]

**Mutante descrito, sem aplicação:** emitir `orfao_14h` imediatamente após E, dispensando vencimento, em `api/views_core.py:758`, alcançando `chamados/services/disputa_emissao.py:444`. Executar o mesmo comando na cópia mutante; ASSERT A deve falhar. Morte efetiva: [nao verificado]. Especificação em [arquivo:20](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:20).

| Campo lido | Quem escreve | Quem lê em produção |
|---|---|---|
| Pergunta: `motivo` | `chamados/models.py:3014`, INSERT `:3017` | `chamados/juizes.py:941` |
| Pergunta: `data_evento` | `chamados/services/fio.py:800` | `chamados/services/fabrica_da_celula.py:148` |
| Pergunta: `materializada_em` | Carimbo `chamados/models.py:3099`; limpeza `:3208`, `:3266` | C6 `chamados/juizes.py:1370`; E2 `chamados/reconciliador.py:629` |
| Batida: `timestamp`, `tipo` | `ponto/registro_batida.py:157` | `ponto/turnos.py:1217` |
| Célula: `ata` | Cartório `ponto/services/cartorio.py:1837` → porta `ponto/portas/celula.py:1033` | `chamados/juizes.py:965` |

Certificação permanece com o Code e o opus final. [nao verificado]

## 11/11 21:04 · sessao ae9a66b9c290 · DATA-EVENTO-CRU · censo · origem=Code

Numeração dos anexos: primeira linha do conteúdo = 1, incluindo linhas vazias e excluindo `== anexo ==`. Em **b**, considero a triagem da própria resposta; descarte posterior pela sessão é **[nao verificado]**.

| Achado | Letra | Trecho q | Trecho r | Motivo em 1 linha |
|---|---|---|---|---|
| **1 — templates sem ponto** (`opus_final.r:3`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Reconhece `with` e aponta `attr` como escape (`preverif_catraca.r:9`, `:11`) | A cobertura dos escapes foi perguntada; `regroup` e `dictsort` ficaram sem identificação. |
| **2 — dict de filtro em variável** (`opus_final.r:4`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?” (`preverif_catraca_v2.q:1`) | “não localizei”; “As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`) | A exclusão de chaves foi expressamente questionada; o dict usado como filtro não foi identificado. |
| **3 — isenção ampla de return** (`opus_final.r:5`) | **e** | A v3 informa “sobrecontagem admitida” do retorno e pergunta sua estabilidade (`preverif_catraca_v3.q:2`, `:6`) | “`return 'data_evento'`, continua contado”; depois bloqueia a compensação (`preverif_catraca_v3.r:5`, `:19`, `:25`) | A pergunta examinava a sobrecontagem; a isenção defeituosa pertence à cura posterior, registrada na [volta 4](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7968). |
| **4 — comentário multilinha** (`opus_final.r:6`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Recomenda “retirar comentários antes de contar”, mencionando `{# ... #}` (`contrato_catraca.r:40`) | A revisão do regex estava no alcance; a resposta não distinguiu a semântica de comentário em uma linha da multilinha. |
| **5 — ESCRITA pelo nome da chamada** (`opus_final.r:7`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (... destino de escrita, DESTINO_KW)?” (`preverif_catraca_v2.q:1`) | “Leitor real novo omitido pelas exclusões novas: não localizei”; demonstra apenas a regressão do subscrito (`preverif_catraca_v2.r:14`) | As exclusões de escrita foram perguntadas; a isenção indistinta pelo nome do método não foi apontada. |
| **6 — total no ESTADO** (`opus_final.r:8`) | **e** | Pergunta “Quem escreve e quem le o JSON” e colisões com enumeradores (`preverif_catraca.q:7`) | “Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**” (`preverif_catraca.r:36`) | Nenhuma q cobra explicitamente o produtor do total no ESTADO; a resposta menciona a publicação sem verificá-la. |
| **7 — código e JSON sobem juntos** (`opus_final.r:9`) | **b** | Pergunta se acrescentar leitor fica vermelho e se a catraca “so desce” (`preverif_catraca.q:3`) | “aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior” (`preverif_catraca.r:7`) | O Codex viu e tratou como limite, deixando-o fora da lista final de bloqueantes (`preverif_catraca.r:7`, `:40`). |

**Contagem da classificação acima:** **a = 4; b = 1; c = 0; d = 0; e = 2. Total = 7** (`opus_final.r:3–9`).

A promessa do achado 6 está em [RELATO.md:7950](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7950) nesta cópia; [RELATO.md:7966](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7966) registra a volta 3.

## 11/11 21:03 · sessao 8e89de2fb777 · - · - · origem=Ronald (tmux)

| Achado | Letra sugerida | Evidência |
|---|---|---|
| 1 | **a** | `contrato_catraca.q:1`: “o regex de template deixa escapar algum leitor”; `preverif_catraca.q:4`: “quais formas de ler data_evento o varredor nao ve”. A resposta enumera `with` e `attr`, sem `regroup`/`dictsort` (`preverif_catraca.r:9,11,20`). |
| 2 | **a** | `preverif_catraca_v2.q:1`: “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?”; resposta: “não localizei [...] As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`). A pergunta cobre a exclusão; a resposta não identifica o dict de filtro. |
| 3 | **e**, com distinção temporal | A última pergunta trata a **sobrecontagem** de `return 'data_evento'` (`preverif_catraca_v3.q:2,4,6`). O Codex encontra compensação e a bloqueia (`preverif_catraca_v3.r:19,27,29`). A isenção ampla de `return` do opus é defeito da cura posterior, sem pergunta de revisão correspondente nos anexos; a passagem v3→v4 registra “PROCEDE e curado” (`app/docs/RELATO.md:7968`). Não classificar **b**: o achado visto pelo Codex foi acolhido, não descartado (`app/docs/RELATO.md:7968`). |

Numeração considerada: primeira linha do conteúdo de cada anexo = 1; linhas vazias incluídas.

Para o achado 2, a resposta inicial já reconhecia escapes por `**` (`preverif_catraca.r:11,16`), mas não há trecho demonstrando descarte pela triagem do caso específico do dict literal guardado em variável **[nao verificado]**.

## 11/11 20:57 · sessao ef30d638d860 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Implementados mapa explícito, recusa `TipoSemFaixa` e contador por tipo ativo. A certificação pelo Code/suite e opus final permanece **[nao verificado]**. (`ponto/precedencia.py:52`, `:96`; `ponto/services/vigia_ausencia.py:155`)

Arquivos alterados:

- [precedencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/precedencia.py:48): comentário, mapa, exceção independente de `VetoDePrecedencia` e último recurso de `_nome_do_tipo` devolvendo o código cru.
- [vigia_ausencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/services/vigia_ausencia.py:155): `tipos_ativos_sem_faixa`, consultando `faixa_de` com `try/except TipoSemFaixa`; frases com códigos e URL da tela.
- [test_lastro_faixa_sem_default.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/tests/test_lastro_faixa_sem_default.py:11): testes novos, incluindo fixture pk 5356, fantasma, inativação, sementes, choices e consulta à autoridade. [nao verificado]
- [test_a6_vigia_familia_ausencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/tests/test_a6_vigia_familia_ausencia.py:124): expectativa de oito linhas.
- [test_calendario_ausencia_pela_autoridade.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52): preservado o caso que dependia do default; agora exige a recusa nomeada.

Mapa final:

| Faixa | Códigos | Fonte |
|---|---|---|
| `ferias` | `ferias` | `ponto/precedencia.py:53` |
| `afastamento` | `afastamento_inss`, `suspensao`, `licenca_maternidade`, `licenca_paternidade`, `licenca_luto` | `ponto/precedencia.py:54` |
| `atestado` | `atestado`, `atestado_inss`, `abon_hr`, `abono`, `atraso`, `aviso_home_office`, `declaracao`, `declaracao_acompanhante_sem_ab`, `declaracao_de_acompanhante`, `desligamento`, `falta`, `reciclagem`, `reducao_de_7_dias_do_aviso`, `saida_antecipada`, `treinamento`, `atestado_medico`, `obito`, `outros` | `ponto/precedencia.py:59` |
| `folga` | `folga_compensatoria`, `feriado_folga`, `folga`, `folga_abonada`, `troca_de_plantao` | `ponto/precedencia.py:79` |

Cada entrada tem justificativa de uma linha. A conferência por grep e AST não encontrou faltantes entre os 17 choices e as 17 sementes. (`ponto/precedencia.py:52`; `ponto/models.py:661`; `ponto/migrations/0052_seed_tipoausencia.py:9`)

A execução Django parou por ausência de Django, antes dos testes (`manage.py:11`). RED/GREEN de integração, inclusive pk 5356, estão **[nao verificado]**. Em checagem isolada, com dependências simuladas, passaram três testes puros e o contrato do contador; ambos os mutantes foram mordidos. (`ponto/tests/test_lastro_faixa_sem_default.py:13`, `:19`, `:56`, `:132`)

Comando RED, para o HEAD/base informado **com somente o teste novo aplicado**; execução nesse HEAD **[nao verificado]**: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 CONGELAR_EM='2026-11-11 12:00:00' \
python3 manage.py test ponto.tests.test_lastro_faixa_sem_default \
--settings=config.settings.ci --noinput
```

Comando GREEN para o Code; resultado **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 CONGELAR_EM='2026-11-11 12:00:00' \
python3 manage.py test \
ponto.tests.test_lastro_faixa_sem_default \
ponto.tests.test_a6_vigia_familia_ausencia \
ponto.tests.test_a2_precedencia \
colaboradores.tests.test_calendario_ausencia_pela_autoridade \
--settings=config.settings.ci --noinput
```

Mutante **default de volta**, somente em memória:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - <<'PY'
import django
django.setup()
from django.core.management import call_command
from ponto import precedencia as p
p.faixa_de = lambda tipo: p._FAIXA_DE_TIPO.get(tipo, 'atestado')
call_command('test', 'ponto.tests.test_lastro_faixa_sem_default', interactive=False)
PY
```

O teste do fantasma ficou vermelho na checagem isolada. O contador do fantasma também deve ficar vermelho na integração **[nao verificado]**. O caso pk 5356 continua verde nesse mutante porque `folga` permanece explicitamente mapeada. (`ponto/tests/test_lastro_faixa_sem_default.py:13`, `:92`, `:108`; `ponto/precedencia.py:81`)

Mutante **contador lê o dict**, somente em memória:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - <<'PY'
import django
django.setup()
import inspect
from django.core.management import call_command
from ponto import precedencia as p
from ponto.services import vigia_ausencia as v
src = inspect.getsource(v.contadores)
src = src.replace('faixa_de(_t.codigo)', '_FAIXA_DE_TIPO[_t.codigo]')
src = src.replace('except TipoSemFaixa:', 'except KeyError:')
v.__dict__['_FAIXA_DE_TIPO'] = p._FAIXA_DE_TIPO
exec(compile(src, v.__file__, 'exec'), v.__dict__)
call_command('test', 'ponto.tests.test_lastro_faixa_sem_default.LastroFaixaSemDefaultTest.test_MORDE_contador_pergunta_a_autoridade_em_vez_do_dict', interactive=False)
PY
```

Esse mutante ficou vermelho na checagem isolada porque deixou de consultar `faixa_de('abono')`; integração **[nao verificado]**. (`ponto/tests/test_lastro_faixa_sem_default.py:132`)

Escritores e leitores dos campos envolvidos:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `TipoAusencia.codigo` | UI gera na criação; porta insere; seed inicializa. (`ponto/views_tipos_ausencia.py:89`; `ponto/services/ausencia.py:1310`; `ponto/migrations/0052_seed_tipoausencia.py:33`) | UI, catálogo, vigia e novo contador. (`ponto/views_tipos_ausencia.py:31`; `ponto/catalogo/ausencias.py:250`; `ponto/services/vigia_ausencia.py:144`, `:162`) |
| `TipoAusencia.ativo` | Checkbox da UI → porta; seed inicializa. (`ponto/views_tipos_ausencia.py:80`; `ponto/services/ausencia.py:1305`; `ponto/migrations/0052_seed_tipoausencia.py:36`) | Catálogo e novo contador. (`ponto/catalogo/ausencias.py:249`; `ponto/services/vigia_ausencia.py:160`) |
| `Ausencia.tipo` | Porta de criação/edição; fixtures dos testes. (`ponto/services/ausencia.py:342`, `:418`, `:498`; `ponto/tests/test_lastro_faixa_sem_default.py:96`) | Precedência histórica, nomeação e vigia existente. (`ponto/precedencia.py:128`, `:298`, `:406`; `ponto/services/vigia_ausencia.py:105`) |
| Faixa | Definição no mapa; resultado do dia montado pela precedência. (`ponto/precedencia.py:52`, `:429`) | `faixa_de` lê o mapa; fatos, veto e contador consultam essa autoridade. (`ponto/precedencia.py:103`, `:406`, `:522`, `:543`; `ponto/services/vigia_ausencia.py:162`) |

As escritas novas de cadastro/inativação são fixtures de teste. (`ponto/tests/test_lastro_faixa_sem_default.py:111`, `:119`)

Nos quatro leitores preservados, uma recusa durante a chamada capturada produz:

- **Espelho:** warning e continuidade com mapas vazios, podendo ficar sem palavras da janela. (`ponto/services/espelho.py:644`)
- **Cartório:** na aplicação posterior da precedência, registra a exceção e retorna `out`. (`ponto/services/cartorio.py:2032`)
- **Situação de férias:** devolve `False` para descanso. (`ferias/situacao.py:93`)
- **Semana do posto:** devolve `trabalho`. (`escala/services/semana_do_posto.py:175`)

## 11/11 20:49 · sessao 683fb9683207 · CASO-0711-HJ003 · sonda · origem=Code

**A 52340 já existia antes da criação da S. O dado também registra `celula_absolveu` antes da S. Portanto, ele não demonstra nem “emissor ignorou um par disponível” nem “pergunta continuou cobrando após o par”.** O nascimento precede o log de emissão; a cobrança vigente depende do juiz, não apenas da existência da linha. (`app/chamados/models.py:3017`, `app/chamados/models.py:3025`, `app/chamados/juizes.py:1428`.) Valores históricos transcritos da sonda, sem conferência independente: [nao verificado].

**1) Intervalo de nascimento sustentado**

| Limite | Evidência |
|---|---|
| **Superior: não depois do log exibido como 31/10/2026 19:00:41.** | Linha fornecida `LOG objeto=52340 modelo=PerguntaDisputa acao=pergunta_emitida ...`. Na cópia, o INSERT acontece em `app/chamados/models.py:3017`; o log, depois dele, em `app/chamados/models.py:3025`. O relógio do log é `auto_now_add`, `app/core/models.py:307`. Valor histórico: [nao verificado]. |
| **Inferior: não determinado pelo pacote.** | A E tem `criado_em=31/10 19:00:40`, mas falta provar que aquela execução causou o nascimento da pergunta. Coincidência de segundo e usuário não fornece essa causalidade. [nao verificado]. |
| **Limite superior adicional, mais fraco: já existia no processamento de 01/11 06:30:48.** | Campo fornecido `P52340.materializada_em`. É escrito separadamente do nascimento, em `app/chamados/models.py:3099`. Valor histórico: [nao verificado]. |

Assim, **nasceu antes de 01/11 07:00:33**, criação fornecida da 143189; o intervalo estreito **19:00:40–19:00:41 não está provado**, nem o instante exato do INSERT. Valores históricos: [nao verificado].

`data_evento=31/10` não fornece limite de nascimento: a porta grava ali o **dia do fato**, por `gravar_dia_do_fato`, não o relógio da inserção. (`app/chamados/models.py:3016`, `app/chamados/services/fio.py:790`, `app/chamados/services/fio.py:800`.)

O log identifica **usuário 415 no contexto da emissão**, porque `registrar_log(None, ...)` pode herdar o usuário da requisição. Ele não identifica exclusivamente API ou signal/cartório executado dentro dela. (`app/chamados/models.py:3025`, `app/core/auditoria.py:22`, `app/core/observ.py:108`, `app/core/observ.py:115`.) Rota e chamador históricos: [nao verificado].

Os registros `modelo=CelulaDia objeto=52340` e `modelo=CelulaDia objeto=7279` não demonstram julgamento da célula 133672; os totais de ações na janela também não individualizam a pergunta. Conteúdo histórico fornecido: [nao verificado].

**2) Quem cria e quem materializa**

Há duas cadeias relevantes na cópia; **a sonda não permite escolher qual delas emitiu historicamente a 52340**. [nao verificado].

| Cadeia | Caminho |
|---|---|
| **API do kernel** | `/api/ponto/bater/` aponta para `views_core` (`app/api/urls_core.py:5`, `app/api/urls_core.py:14`). Detecta anomalias **antes** da batida (`app/api/views_core.py:420`), grava pela porta (`app/api/views_core.py:645`) e depois chama a emissão (`app/api/views_core.py:758`). |
| **Signal → cartório → fábrica** | `post_save(Batida)` → julgamento (`app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`) → fábrica (`app/ponto/services/cartorio.py:1032`) → faltantes vencidos da ata (`app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`) → disputa e gerador (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`). |
| **Convergência do nascimento** | Gerador → `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444`) → porta do fato (`app/chamados/models.py:2789`) → INSERT e log (`app/chamados/models.py:3017`, `app/chamados/models.py:3025`). |

Na fábrica, **`orfao_14h` também significa “marco final S faltante”**: a tradução é `('fim','S') → 'orfao_14h'`. O nome não prova que aquele emissor mediu quatorze horas. (`app/chamados/services/declaracao_texto.py:44`, `app/chamados/services/fabrica_da_celula.py:71`.)

Para a via observada, o caminho atual é:

`retratar_perguntas_absolvidas_por_celula` → `p.carimbar(VIA_CELULA_ABSOLVEU, usuário_sistema)` → atribuição do carimbo → consulta ao C6 → persistência. (`app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:595`, `app/chamados/models.py:3129`, `app/chamados/models.py:3139`, `app/chamados/models.py:3152`.)

**`materializada_em` preenchida com resposta e validação nulas é compatível com absolvição automática sem plantio.** Esse chamador não pede assinatura de resposta/validação; a porta só as preenche quando recebe esse pedido e existe texto. (`app/chamados/reconciliador.py:595`, `app/chamados/models.py:3130`, `app/chamados/models.py:3134`.) Logo, o carimbo fornecido não prova batida criada nem resposta humana; atribuir a execução histórica exatamente a esse fluxo permanece [nao verificado].

**Todos os escritores atuais do carimbo de `PerguntaDisputa`, incluindo limpezas:**

| Escritor/porta | Operação |
|---|---|
| `_escrever_carimbo` | Atribui instante, autor e via juntos. `app/chamados/models.py:3099`, `app/chamados/models.py:3100`, `app/chamados/models.py:3101`. |
| `carimbar` | Usa o escritor acima e salva somente se C6 aceitar a pergunta como morta. `app/chamados/models.py:3129`, `app/chamados/models.py:3139`, `app/chamados/models.py:3152`. |
| `carimbar_identidade` | Usa o mesmo escritor e salva pela porta de identidade. `app/chamados/models.py:3183`, `app/chamados/models.py:3185`, `app/chamados/models.py:3190`. |
| `_desfazer_em_memoria` | Restaura o carimbo anterior quando o ato falha ou é recusado. `app/chamados/models.py:3162`. |
| `descarimbar` | Limpa os três campos; persistência opcional. `app/chamados/models.py:3208`, `app/chamados/models.py:3212`. |
| `descarimbar_em_lote` | Segundo sítio de escrita direta: limpa os três campos por UPDATE. `app/chamados/models.py:3264`, `app/chamados/models.py:3266`. |

Os chamadores produtivos dessas portas são:

| Operação | Chamadores |
|---|---|
| Carimbar | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:595`; `app/chamados/services/materializacao.py:566`, `app/chamados/services/materializacao.py:1282`, `app/chamados/services/materializacao.py:1436`; `app/chamados/services/disputa_emissao.py:1863`, `app/chamados/services/disputa_emissao.py:1937`. |
| Carimbar identidade | `app/chamados/models.py:2061`, `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:929`, `app/chamados/services/materializacao.py:1104`. |
| Descarimbar individualmente | `app/chamados/models.py:1506` — simulação sem salvar —, `app/chamados/models.py:3298`, `app/chamados/models.py:3315`, `app/chamados/models.py:3322`; `app/chamados/services/relavra_via.py:246`. |
| Descarimbar em lote | `app/chamados/models.py:1566`; `app/chamados/services/disputa_emissao.py:1630`. |

O selo declara esses dois escritores diretos e distingue o homônimo de **RegularizacaoExterna**, que não escreve a pergunta. Também declara limitações da análise estática; portanto, isto não certifica escritores externos ao código da cópia. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`.)

**3) Depois da S: leitores, absolvição e motivo de não reescrever**

A cadeia da S alcança **a data local e a véspera**; portanto, não existe nesse signal um recorte que necessariamente abandone 31/10. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:135`.)

| Leitor/caminho após a S | Decisão na cópia |
|---|---|
| **Cartório/grade** | Carrega universo apurável em janela ampliada e monta a grade. `app/ponto/services/cartorio.py:1142`, `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`. Lavra ata e veredito: `app/ponto/services/cartorio.py:1837`, `app/ponto/services/cartorio.py:1839`. |
| **Fábrica: perguntas já carimbadas** | Pode coletar carimbadas e pedir retirada de carimbo sem lastro. `app/chamados/services/fabrica_da_celula.py:148`, `app/chamados/services/fabrica_da_celula.py:156`, `app/chamados/services/fabrica_da_celula.py:209`. |
| **Reconciliador síncrono da batida** | Roda depois do cartório e chama E2 antes do reatamento. `app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:1432`, `app/chamados/reconciliador.py:1433`. |
| **E2: célula absolveu** | Seleciona somente perguntas **sem materialização**, sem validação e com célula julgada não acusatória; exige C6 morto. `app/chamados/reconciliador.py:629`, `app/chamados/reconciliador.py:630`, `app/chamados/reconciliador.py:637`, `app/chamados/reconciliador.py:661`. |
| **C1: sem turno aberto** | Retorna imediatamente se já materializada. `app/chamados/reconciliador.py:366`. |
| **Reatamento de órfãs** | Exige destino nulo, disputa aberta e **materialização nula**. `app/chamados/reconciliador.py:1501`, `app/chamados/reconciliador.py:1503`, `app/chamados/reconciliador.py:1504`. |
| **Consumidor da lâmpada** | Chama E2 e encerramento de órfãs. `app/chamados/reconciliador.py:1365`, `app/chamados/reconciliador.py:1375`. O universo de órfãs também exige **materialização nula**. `app/chamados/reconciliador.py:1049`. |
| **Predicado da lâmpada** | Exige fato do marco provado, pergunta viva e encerramento temporal; então pode gravar fantasma e auditoria. `app/chamados/reconciliador.py:1084`, `app/chamados/reconciliador.py:1086`, `app/chamados/reconciliador.py:1090`, `app/chamados/reconciliador.py:1147`. |
| **Retirada de carimbo sem lastro** | Aceita materializadas fora das vias de identidade; só descarimba quando C6 diz **viva**. Carimbo com lastro permanece. `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:242`, `app/chamados/services/relavra_via.py:246`. |

**Isso explica por que os tratadores que exigem materialização nula não voltariam a carimbar a ficha fornecida: ela já estava carimbada às 06:30:48. Não demonstra defeito desses tratadores.** Filtro: `app/chamados/reconciliador.py:629`; estado histórico: [nao verificado].

O decisor da cobrança é C6:

- `orfao_14h` consulta a lâmpada `S/hf`, lendo DNA e ata da célula. (`app/chamados/juizes.py:924`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:965`.)
- Com carimbo e lâmpada própria acesa, a pergunta fica morta **antes** de examinar outros furos do dia. (`app/chamados/juizes.py:1428`.)
- `celula_absolveu` não é identidade terminal; célula acusatória pode fazê-la continuar viva. (`app/chamados/models.py:2510`, `app/chamados/juizes.py:1434`.)
- O rótulo `estado` consulta C6 antes de chamar a pergunta materializada de resolvida. (`app/chamados/models.py:3044`, `app/chamados/models.py:3046`.)

Portanto, **“por que não absolveu quando a S chegou?” contém uma premissa não demonstrada**: existe registro de absolvição anterior; falta provar cobrança posterior. Ata, DNA, veredito da célula 133672, resultado de C6 e execução dos consumidores não aparecem no pacote. [nao verificado].

A disputa continuar aberta também não prova vitalidade dessa pergunta: C7 consulta o conjunto inteiro de perguntas e respostas sem veredito. (`app/chamados/models.py:1848`, `app/chamados/models.py:1853`.)

**4) Classe, RED e mutante**

**A classe causal da 52340 ainda não pode ser fechada pelo pacote.** Nascer antes da S elimina a explicação “ignorou aquela S já existente”, mas não prova sozinho cobrança prematura: falta confirmar qual turno/marco a emissão estava cobrando. [nao verificado].

Há uma razão adicional para não presumir essa identidade histórica: o próprio código documenta que `resolver_data_evento` anteriormente gravava o dia da saída esperada, D+1, enquanto hoje devolve o dia do turno. Isso não prova qual versão emitiu a 52340. (`app/escala/servico_jornada.py:498`, `app/escala/servico_jornada.py:501`, `app/escala/servico_jornada.py:509`.) Versão efetivamente executada em 31/10: [nao verificado].

**Classe a ensaiar:** emissão de cobrança sem premissa temporal vigente no nascimento, contrariando **juiz único/escritor único**, com identidade do turno mantida pela fonte única. Esses padrões já existem no desenho. (`app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:12`.) Atribuição dessa classe à 52340: [nao verificado].

O contrato temporal existente é concreto: `classificar_falta` cala antes do fim quando encontra vínculo/tipo; a fábrica só cobra faltantes vencidos, pelo instante encadeado do marco +30 minutos. (`app/escala/servico_jornada.py:385`, `app/escala/servico_jornada.py:388`, `app/chamados/services/fabrica_da_celula.py:69`, `app/ponto/services/cartorio.py:882`.)

**Ensaio mínimo proposto, ainda não executado: [nao verificado].**

| Relógio, nesta ordem | Ação e observação exigidas |
|---|---|
| **D 19:00** | Célula/vínculo noturno 19→07; registrar **somente E** pelo endpoint do kernel, deixando os signals reais rodarem. Exigir zero nascimento de pergunta de saída para o fim ainda futuro. Entrada real: `app/api/urls_core.py:14`, `app/api/views_core.py:645`. |
| **D+1 06:30** | Executar cartório/reconciliação reais, ainda sem S. Exigir novamente zero nascimento indevido. Teto real: `app/ponto/services/cartorio.py:882`, `app/chamados/services/fabrica_da_celula.py:69`. |
| **D+1 07:00** | Registrar S pela mesma porta; deixar julgamento e reconciliação reais ocorrerem. `app/ponto/signals.py:135`, `app/ponto/signals.py:215`. |
| **Após S e após vencimento aplicável** | Exigir um turno fechado ancorado em D e nenhuma pergunta indevida **nascida em qualquer etapa**, inclusive uma que depois tenha sido absolvida. Pareamento/seleção: `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1995`. |

Se a reprodução depender do agregado anterior, acrescentar a disputa aberta e seu contexto **com evidência do conteúdo**, sem inventar uma pergunta anterior para obter vermelho. Contexto histórico necessário: [nao verificado].

**Mutante pertinente proposto:** depois de gravar E, inserir no caminho de emissão um `orfao_14h` para D apenas porque o turno abriu, dispensando a autoridade temporal. O ponto observado é a emissão posterior à gravação, `app/api/views_core.py:758`, chegando a `app/chamados/services/disputa_emissao.py:444`. A primeira asserção deve mordê-lo; execução e morte efetiva do mutante: [nao verificado].

O mutante anterior de recorte civil da S não testa esse nascimento anterior à S. E verificar apenas “nenhuma pergunta viva ao final” deixaria passar nascimento falso seguido de absolvição. Adequação ao contrato proposto: [nao verificado].

**Não há RED demonstrado nesta leitura**: há especificação do ensaio. Uma fixture 19→07 que já passe na cópia não pode ser apresentada como reprodução vermelha da 52340. [nao verificado].

**Escritores e leitores dos demais campos usados**

A tabela identifica os sítios relevantes a esta análise; o censo integral de escritores do carimbo está acima.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Log: `acao`, `modelo`, `objeto_id`, `usuario`, `ator` | `app/core/observ.py:118`, `app/core/observ.py:123`, `app/core/observ.py:134`. | Leitura histórica deste pacote: sonda fornecida [nao verificado]. |
| Log: `criado_em` | Django, `auto_now_add`: `app/core/models.py:307`. | Histórico: `app/chamados/services/historico.py:90`, `app/chamados/services/historico.py:92`. |
| Pergunta: `motivo`, `disputa` | Nascimento: `app/chamados/models.py:3014`. | Marco: `app/chamados/juizes.py:941`; universo por disputa: `app/chamados/reconciliador.py:628`. |
| Pergunta: `data_evento` | `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016`. | `app/chamados/reconciliador.py:1088`, `app/chamados/services/fabrica_da_celula.py:148`. |
| Pergunta: `celula` | Nascimento: `app/chamados/models.py:3014`; rechaveamento: `app/chamados/services/fio.py:1038`. | `app/chamados/juizes.py:944`. |
| Pergunta: `chamado_destino` | Nascimento: `app/chamados/models.py:3014`; costura: `app/chamados/services/fio.py:925`. | `app/chamados/reconciliador.py:1501`. |
| Pergunta: `materializada_em`, `materializada_por` | Portas e chamadores enumerados acima. | C6: `app/chamados/juizes.py:1370`; elegibilidade: `app/chamados/reconciliador.py:629`; auditoria do autor: `app/chamados/reconciliador.py:1145`. |
| Pergunta: `via_resolucao` | Carimbo: `app/chamados/models.py:3101`; troca isolada de via: `app/chamados/models.py:3275`, `app/chamados/services/relavra_via.py:125`. | `app/chamados/juizes.py:1370`, `app/chamados/juizes.py:1422`. |
| Pergunta: `respondida_em`, `resposta_colab` | Resposta: `app/chamados/services/disputa_emissao.py:1467`; automática: `app/chamados/services/disputa_emissao.py:1652`; assinatura pelo carimbo: `app/chamados/models.py:3132`. | `app/chamados/juizes.py:1369`, `app/chamados/juizes.py:1401`. |
| Pergunta: `validada_em` | Validação: `app/chamados/services/validacao.py:134`; carimbo: `app/chamados/models.py:3135`; limpezas: `app/chamados/models.py:1566`, `app/chamados/services/disputa_emissao.py:1480`. | `app/chamados/juizes.py:1385`, `app/chamados/reconciliador.py:630`. |
| Batida: `timestamp`, `tipo` | `app/ponto/registro_batida.py:157`. | Ordenação: `app/ponto/turnos.py:1217`; datas do julgamento: `app/ponto/signals.py:111`; papel: `app/ponto/turnos.py:359`. |
| Batida: `criado_em` | Django, `auto_now_add`: `app/ponto/models.py:79`. | Guarda de chegada: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`. |
| Célula: `dna` | Geração: `app/ponto/portas/celula.py:260`; edição: `app/ponto/portas/celula.py:604`; regeneração: `app/ponto/portas/celula.py:875`. | `app/chamados/juizes.py:948`. |
| Célula: `ata`, lâmpadas, `veredito` | Montagem/lavra: `app/ponto/services/cartorio.py:1837`; escrita: `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:1035`. | Lâmpadas: `app/chamados/juizes.py:965`; veredito: `app/chamados/juizes.py:1432`. |
| Disputa: `fechada_em` | Porta `fechar`: `app/chamados/models.py:1855`. | Elegibilidade: `app/chamados/reconciliador.py:1503`; fechamento condicionado: `app/chamados/reconciliador.py:611`. |

## 11/11 20:45 · sessao b601eef00d07 · - · - · origem=Ronald (tmux)

O nascimento está provado **antes da S**, mas o limite inferior estrito não está provado. O único emissor de log `pergunta_emitida` na cópia está em `app/chamados/models.py:3025`, executado **depois** do `p.save(force_insert=True)` em `app/chamados/models.py:3017`. O `criado_em` do log é `auto_now_add`, em `app/core/models.py:307`. Portanto, com o LOG fornecido, a inserção aconteceu **até 31/10/2026 19:00:41**, antes da criação da S em 01/11 07:00:33. Afirmar o intervalo estreito **[19:00:40,19:00:41]** exigiria provar que a pergunta nasceu especificamente da execução da E; a coincidência com o log da foto e o usuário 415 apenas corrobora essa hipótese. Limite inferior: **[nao verificado]**.

`data_evento=31/10` não serve de relógio de nascimento: é o dia do fato, gravado pela porta do turno em `app/chamados/models.py:3016`; a data do turno é resolvida em `app/chamados/models.py:2875`. `materializada_em` também não é nascimento: seu escritor é separado, em `app/chamados/models.py:3099`.

O log identifica uma **request autenticada no contexto do usuário 415**, pelo encadeamento `app/chamados/models.py:3025` → `app/core/auditoria.py:22` → `app/core/observ.py:108`–`118`, com contexto instalado em `app/core/middleware.py:78`. A rota HTTP específica e a distinção **emissão pelo signal/cartório versus emissão posterior pela API** continuam **[nao verificado]**: ambos podem herdar esse mesmo usuário. O dado não contém URL, descrição completa do evento, contexto da disputa nem callstack.

Cadeias existentes:

- API → detector: `app/api/views.py:434` (clone `app/api/views_core.py:420`) → `app/chamados/detector_anomalias.py:32` → universo apurável e pareador `app/ponto/utils_turno_orfao.py:28`, `:31`. API emite por `app/api/views.py:764` → `app/chamados/services/disputa_emissao.py:662`, `:670`/`:682` → `:444` → porta `app/chamados/models.py:2789` → nascimento `:3017` e log `:3025`.
- Signal de Batida → `app/ponto/signals.py:131`, `:135` → julgamento da véspera e dia local (`:111`–`:112`) → cartório `:93` → fábrica `app/ponto/services/cartorio.py:1032` → faltantes vencidos da ata `app/chamados/services/fabrica_da_celula.py:64`, `:69`, `:71` → emissão em `:172`, `:174` → mesma porta.
- Materialização pela absolvição da célula: `app/chamados/reconciliador.py:584` → usuário automático `:587` → **único chamador produtivo da via `celula_absolveu`** em `:595` → `PerguntaDisputa.carimbar` em `app/chamados/models.py:3103` → atribuição em `:3129`/`:3099` → C6 `:3139` → save `:3152`.

`materializada_em` com R/V nulas é compatível com uma **pergunta retirada da cobrança sem resposta e sem plantio**, pois o escritor da absolvição chama `carimbar` sem argumento `resposta` (`app/chamados/reconciliador.py:595`). A porta só assina resposta/validação quando há pedido explícito e texto (`app/chamados/models.py:3130`–`:3137`). Também não pede lavra de veredito, explicando a compatibilidade com lavra vazia; prova da execução histórica exata, além da via registrada: **[nao verificado]**.

Todos os escritores diretos atuais de `PerguntaDisputa.materializada_em`:

| Escritor direto | Operação |
|---|---|
| `app/chamados/models.py:3099` | `_escrever_carimbo`: instante, autor e via juntos. |
| `app/chamados/models.py:3264`–`:3266` | `descarimbar_em_lote`: limpa os três campos. |

Confirmei por AST em memória, reutilizando apenas as funções de análise do selo `app/chamados/tests/test_o261e_f2_porta_carimbo.py:65`–`:202`, sem importar a aplicação. A atribuição de `app/chamados/services/regularizacao_ext.py:198` é homônima de **RegularizacaoExterna**, não da pergunta; o próprio selo a distingue em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`–`:49`. Limites dessa análise estática estão declarados em `:111`–`:115`.

Todas as portas e chamadores produtivos que chegam a esses escritores:

| Porta/categoria | Chamadores |
|---|---|
| `carimbar` → atribuição `models.py:3129`, persistência `:3152` | `models.py:3287` (fantasma); `reconciliador.py:405` (sem turno aberto), `:595` (célula absolveu); `services/materializacao.py:566` (absorção por ausência), `:1282` (materializar uma), `:1436` (pendentes aprovadas); `services/disputa_emissao.py:1863` (destino resolvido), `:1937` (sem credor). Todos sob `app/chamados/`. |
| `carimbar_identidade` → atribuição `models.py:3183`/`:3185`, save `:3190` | `models.py:3064` (admin decidiu), `:2061` (motivo retratado); `services/fio.py:1359` (duplicata); `services/materializacao.py:929` (paridade recusada), `:1104` (colisão/fato ocupado). |
| Reversão em memória | `models.py:3162`, chamada ao desfazer carimbo recusado/falho em `:3141`, `:3144`, `:3155`. |
| `descarimbar` → `models.py:3208`, save opcional `:3212` | `models.py:1506` (**simulação, sem save**), `:3298` (reabrir fantasma), `:3315` (resposta nova), `:3322` (competência reaberta); `services/relavra_via.py:246` (carimbo sem lastro). |
| `descarimbar_em_lote` → `models.py:3266` | `models.py:1566` (ressuscitar perguntas); `services/disputa_emissao.py:1630` (reabrir para correção). |

O primeiro lugar que explica por que a absolvição não volta a tratar a pergunta depois da S é seu próprio universo: `app/chamados/reconciliador.py:629` exige `materializada_em IS NULL`. Com o carimbo de 06:30:48, a 52340 fica de fora. O leitor `lavrar_sem_turno_aberto` também retorna imediatamente quando o carimbo existe, em `app/chamados/reconciliador.py:366`–`:367`. Isso explica a seleção na cópia; o caminho efetivamente percorrido em produção depois da S continua **[nao verificado]**.

## 11/11 20:45 · sessao e7647ffdc603 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

O default alcança também leituras históricas; removê-lo exige tratar explicitamente os códigos antigos e os leitores que hoje capturam exceções. A origem é `app/ponto/precedencia.py:79`; a leitura histórica chega a ela em `app/ponto/precedencia.py:273–279` e `:381`.

Hash informado: `29e201d0ca2c9a6534ece9880e4a989a629f9987`. HEAD **[nao verificado]**: esta cópia não contém `.git`. Os números de produção abaixo vêm exclusivamente da sonda fornecida; sua reprodução no banco está **[nao verificado]**.

**1. Os 17 tipos ativos fora do mapa**

A regra escrita põe ausências humanas na faixa `atestado`, expressamente incluindo abono e falta; abre uma exceção para ausência que **é a própria folga**. Isso é classificação de precedência, não diagnóstico médico nem efeito financeiro. (`app/ponto/precedencia.py:48–63`)

Os campos não determinam uma faixa por si: `efeito` responde pagamento; `fim` responde duração; `medico` responde relatórios. O mapa, por exemplo, mantém `atestado_inss` em `atestado` e licenças em `afastamento`. (`app/ponto/catalogo/ausencias.py:9–12`, `:75–82`; `app/ponto/models.py:589–590`; `app/ponto/precedencia.py:54–59`)

O universo de 17 códigos e seus atributos é o da sonda fornecida **[nao verificado]**. Na tabela, “alternativa” indica uma interpretação que o material não permite resolver apenas pelo nome.

| Tipo | Faixa indicada e justificativa |
|---|---|
| `abon_hr` | **atestado**: o comentário cita esse código entre fatos humanos que vencem feriado/folga/jornada. (`app/ponto/precedencia.py:48–50`) |
| `abono` | **atestado**: abono é expressamente incluído nessa faixa; pagar não o transforma em férias ou folga. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:102`) |
| `atraso` | **atestado**, enquanto lançamento de ausência parcial: a regra geral cobre o fato humano; duração de um dia/parcialidade não define outra precedência. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:90`) |
| `aviso_home_office` | **atestado** pelo contrato atual de ausência abonada; **trabalho** é defensável se representar efetivo trabalho remoto. Decide a natureza do lançamento, ausente na sonda **[nao verificado]**. O código o chama de aviso “cumprido em home office” e o mantém como ausência abonada. (`app/ponto/models.py:678`; `app/ponto/catalogo/ausencias.py:104`; `app/ponto/precedencia.py:48–50`) |
| `declaracao` | **atestado**: comparecimento médico parcial, abonado, sem exceção própria no mapa. (`app/ponto/catalogo/ausencias.py:96`; `app/ponto/precedencia.py:48–50`) |
| `declaracao_acompanhante_sem_ab` | **atestado** pela regra do fato humano; descontar não significa `trabalho` nem identifica afastamento. O significado específico além dos atributos fornecidos está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:9–12`) |
| `declaracao_de_acompanhante` | **atestado** pela mesma regra; `medico` classifica relatórios, não cria precedência própria. (`app/ponto/precedencia.py:48–50`; `app/ponto/models.py:589–590`) |
| `desligamento` | **atestado** pela regra geral aplicada a uma `Ausencia`; **afastamento** é defensável se representar dispensa/suspensão de comparecimento. Se significa término do vínculo, enquadrá-lo numa dessas faixas pode sequer representar o fato correto. Qual desses sentidos foi cadastrado está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`, `:54–57`) |
| `falta` | **atestado**: está explicitamente nomeada na regra geral, apesar de descontar. A faixa não deve virar `trabalho` por causa do desconto. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:88`) |
| `folga` | **folga**: a exceção escrita diz que folga lançada como ausência “é a folga do dia”. (`app/ponto/precedencia.py:60–63`) |
| `folga_abonada` | **folga**, se o nome descreve descanso lançado; **atestado**, se “folga” foi usado para abonar ausência em jornada mantida. Decide descanso versus justificativa, não o atributo `abona`; o conteúdo dos lançamentos está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`, `:60–63`; `app/ponto/catalogo/ausencias.py:10`) |
| `licenca_luto` | **atestado** pela regra geral; **afastamento** é defensável por analogia às outras licenças já mapeadas. O catálogo/form declaram licença paga de dois dias, mas não resolvem essa distinção de precedência. (`app/ponto/catalogo/ausencias.py:98`; `app/ponto/forms.py:20`; `app/ponto/precedencia.py:48–57`) |
| `reciclagem` | **atestado** enquanto ausência abonada; **trabalho** se for treinamento obrigatório efetivamente trabalhado. A sonda não informa obrigação, jornada ou realização **[nao verificado]**. A única regra de faixa aplicável explicitamente é a geral. (`app/ponto/precedencia.py:48–50`) |
| `reducao_de_7_dias_do_aviso` | **atestado** pela regra geral; **afastamento** é defensável se representar dispensa integral de comparecimento durante o aviso. A configuração fornecida não resolve essa escolha nem prova seu fundamento CLT **[nao verificado]**. (`app/ponto/precedencia.py:48–57`) |
| `saida_antecipada` | **atestado**, enquanto ausência parcial lançada: o catálogo a distingue por parcialidade/duração, sem faixa própria. (`app/ponto/catalogo/ausencias.py:91`; `app/ponto/precedencia.py:48–50`) |
| `treinamento` | **atestado** no contrato atual, que o cadastra como ausência abonada; **trabalho** se representar serviço efetivamente prestado. Decide a natureza real do evento, não “empresa paga”; isso está **[nao verificado]** na sonda. (`app/ponto/catalogo/ausencias.py:103`; `app/ponto/models.py:674`; `app/ponto/precedencia.py:48–50`) |
| `troca_de_plantao` | **folga no dia cedido / trabalho no dia assumido**, quando há troca confirmada: o corte exige um par e confirmação administrativa. Para uma linha isolada que seja apenas pedido, a equivalência a qualquer desses lados está **[nao verificado]**; tratá-la indistintamente como `atestado` perpetuaria a ambiguidade. (`app/docs/CORTES.md:187`) |

Não há, no material fornecido, fundamentação CLT suficiente para fechar juridicamente os sentidos ambíguos dos códigos personalizados. Essa fundamentação permanece **[nao verificado]**; os campos não autorizam inventar uma conversão automática para faixa. (`app/ponto/catalogo/ausencias.py:9–12`, `:75–82`; `app/ponto/models.py:589–590`)

**2. Histórico, chamadas e exceções**

A sonda informa `troca_de_posto` inativo sem usos e os órfãos `atestado_medico:4`, `obito:1`, `outros:1`; datas/status desses seis registros estão **[nao verificado]**.

**Sim, esses códigos são alcançáveis hoje.** `fatos_do_periodo` consulta diretamente `Ausencia`, sem cruzar cadastro ou atividade, excluindo somente rejeitadas; depois classifica cada ocorrência com `faixa_de`. Não existe limite a “dia atual”. (`app/ponto/precedencia.py:273–279`, `:336–339`, `:381`)

Censo das chamadas executáveis diretas:

| Sítio | Uso | Se `faixa_de` levantar |
|---|---|---|
| `app/ponto/precedencia.py:79` | Único leitor de `_FAIXA_DE_TIPO`; atualmente aplica o default. | É a origem proposta da recusa. |
| `app/ponto/precedencia.py:103` | Último recurso para nomear tipo, após tentar `get_tipo_display`. | Propaga: o `except` anterior não envolve esta linha. (`:96–103`) |
| `app/ponto/precedencia.py:381` | Classifica cada ausência da janela. | Interrompe a montagem da janela; não há captura local. (`:379–416`) |
| `app/ponto/precedencia.py:497` | Verifica se o lançamento atravessa folga humana. | Propaga antes de percorrer as folgas. (`:496–508`) |
| `app/ponto/precedencia.py:518` | Monta o texto de veto sobre folga humana. | Propaga; um desconhecido já recusado em `:497` não chega aqui. (`:514–518`) |

Não foram encontradas chamadas diretas adicionais em testes; o teste de abono chega indiretamente pela A2, e os testes de veto chegam pela porta pública. (`app/ponto/tests/test_a2_precedencia.py:395`, `:410`, `:420`) Cobertura de chamadas construídas dinamicamente está **[nao verificado]**.

Os leitores históricos afetados, com o tratamento local da exceção:

| Leitor | Resultado |
|---|---|
| `fatos_do_dia` | Delega e propaga. (`app/ponto/precedencia.py:256`) |
| Calendário | Loga e relança: pode falhar a leitura da janela histórica. (`app/colaboradores/services/calendario.py:507–519`) |
| Espelho | Loga e continua com mapas de fatos/decisões vazios: pode perder palavras da janela. (`app/ponto/services/espelho.py:644–651`) |
| Status do dia | Propaga na alimentação da lavra e nos ramos sem lavra. (`app/ponto/status_dia.py:181`, `:298`, `:304`) |
| Cartório | A aplicação propaga; `julgar_celula` captura e registra a falha da precedência. (`app/ponto/services/cartorio.py:1985`, `:2032–2039`) |
| Situação de férias | Captura e devolve `False` para descanso. (`app/ferias/situacao.py:93–99`) |
| Semana do posto | Captura e devolve `trabalho`. (`app/escala/services/semana_do_posto.py:175–179`) |
| Indicador de folha sem furo | Propaga ao consultar os fatos das datas com lâmpadas apagadas. (`app/folha/services/sem_furo.py:20–26`) |
| Dia do colaborador | Propaga. (`app/ponto/services/dia_do_colab.py:15`) |
| Deslogados / faltas de hoje / abriu e não bateu | Propagam localmente. (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`) |
| Detector de ausências / reavaliação feriado-folga | Propagam localmente. (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`) |

Chamadores do veto capturam **somente `VetoDePrecedencia`**: criação de ausência traduz para `AusenciaInvalida(codigo='sobreposicao')`; declaração devolve texto; férias traduzem para `ValidationError` ou lista de erros. As duas chamadas em folgas propagam. (`app/ponto/services/ausencia.py:379–383`; `app/ponto/services/declarar_dia.py:119–122`; `app/ferias/models.py:434–439`; `app/ferias/views.py:253–256`; `app/escala/folgas.py:310`, `:336`)

Portanto, uma exceção nova independente escapa desses tradutores; fazê-la herdar do veto sem adaptação a rotularia como **sobreposição** na criação de ausência. (`app/ponto/services/ausencia.py:382–383`)

Os demais testes com chamadas indiretas aos fatos/veto estão nestes sítios; uma exceção inesperada falha o teste, enquanto os testes de veto esperam a exceção específica: [nao verificado]

- `app/ponto/tests/test_bug107_justificar_falta.py:84`; `app/ponto/tests/test_o198_falta_em_dia_sem_jornada.py:104`; `app/ponto/tests/test_bo_atestado_range_absolve_tudo.py:133`.
- `app/ponto/tests/test_grade_folga_juiz_sitios.py:74`; `app/ponto/tests/test_precedencia_pela_celula.py:77`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:53`; `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:43`.
- `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:72`; `app/colaboradores/tests/test_calendario_um_juiz.py:118`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:64`.
- `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:139`; `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:55`.

Já existe teste que exige **warning + propagação** quando a A2 falha no calendário: `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:264–276`.

**3. Casa do contador e caso que morde**

A casa é **`vigia_ausencia.contadores`**, que já compara cadastro/código, entrega `catalogo_diverge_do_cadastro` e `tipos_fora_do_catalogo` e reúne os acesos. (`app/ponto/services/vigia_ausencia.py:50–59`, `:142–184`, `:207–212`)

A entrega semanal também existe: revisão → pauta → comando → cron. Não precisa nascer outro vigia. (`app/ponto/services/vigia_ausencia.py:227–242`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23–29`; `app/config/crons.py:739–740`)

O placar existente publica “esperado 0” no mesmo domínio:

- ausência aprovada sem efeito na célula: `bin/placar_code.sh:413–416`;
- “nunca bateu” em dia coberto, explicitamente precedência: `bin/placar_code.sh:417–419`;
- ausência sem célula com vínculo: `bin/placar_code.sh:420`;
- ausência bloqueante sem fim, DP/cadastro: `bin/placar_code.sh:243–247`.

O molde é o teste que cria divergência real, exige `n=1` e nome do código, depois verifica acordo → zero. (`app/ponto/tests/test_a6_vigia_familia_ausencia.py:102–119`)

Para esta obra, o caso deve contar **tipo ativo sem faixa mesmo sem nenhuma Ausencia**; mapear ou inativar deve zerá-lo. A unidade é tipo, não lançamento. O molde também exige presença das linhas em zero e pauta somente para contador aceso. (`app/ponto/tests/test_a6_vigia_familia_ausencia.py:124–132`, `:178–193`) Esse novo caso e sua execução estão **[nao verificado]**.

O mutante “default voltou” precisa ser mordido por chamada real de `faixa_de(tipo_fantasma)` esperando recusa; apenas contar chaves ausentes do mapa não prova que o default morreu. A origem atual do comportamento é `app/ponto/precedencia.py:79`; RED/GREEN/mutante novos estão **[nao verificado]**.

**4. Escritores, leitores e descoberta pelo admin**

| Estado | Escritores e leitores comprovados |
|---|---|
| `TipoAusencia.codigo` | UI gera código somente na criação; porta faz o INSERT. Edição pela UI preserva o código. (`app/ponto/views_tipos_ausencia.py:83–94`; `app/ponto/services/ausencia.py:1301–1310`) |
| `TipoAusencia.ativo` | UI lê checkbox; porta aplica `campos` e salva. (`app/ponto/views_tipos_ausencia.py:67–80`; `app/ponto/services/ausencia.py:1305–1308`) |
| Leitores diretos do cadastro | UI lista todos e conta usos por código; catálogo seleciona ativos e chaveia por código; vigia carrega todos para comparação. (`app/ponto/views_tipos_ausencia.py:24–50`; `app/ponto/catalogo/ausencias.py:249–251`; `app/ponto/services/vigia_ausencia.py:143`) |
| Escrita inicial do cadastro | Seed cria código e `ativo=True`. (`app/ponto/migrations/0052_seed_tipoausencia.py:30–37`) |
| `Ausencia.tipo` | Criação monta `kwargs`, instancia e salva; edição atribui `ausencia.tipo` e salva. (`app/ponto/services/ausencia.py:341–345`, `:385`, `:418`, `:498–509`) |
| Férias | Espelhamento chama a mesma porta com `tipo='ferias'`. (`app/ponto/services/ausencia.py:1202–1207`) |

As portas canônicas das duas entidades estão registradas no mesmo serviço, com selo de escritores externos e allowlist vazia. (`app/core/portas.py:481–487`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:23–26`, `:53–57`)

Os chamadores de criação incluem UI, API, materialização de chamados, veredito da célula e checagem de ausência; regularização/abandono usam o wrapper `garantir_ausencia`. (`app/ponto/views.py:2415`, `:2536`; `app/api/views.py:1805`; `app/chamados/services/materializacao.py:490`, `:526`; `app/ponto/services/veredito_celula.py:336`; `app/ponto/services/checar_ausencia.py:46`; `app/ponto/services/ausencia.py:453–461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`)

`Ausencia.tipo` é lido para validação e edição, efeito/documento, cobertura/range, precedência, grade, fechamento, exportação, vigia e resumo. (`app/ponto/services/ausencia.py:193–195`, `:474–475`, `:611–628`; `app/ponto/models.py:728–729`, `:840–842`; `app/ponto/turnos.py:2534`, `:2600`, `:2774`; `app/ponto/precedencia.py:381`; `app/escala/utils.py:1189`; `app/ponto/services/fechamento.py:693–701`; `app/folha/export.py:88–91`; `app/ponto/services/vigia_ausencia.py:104`; `app/inteligencia/resumo.py:43–44`) Censo fechado de todos os acessos dinâmicos e templates a esse campo: **[nao verificado]**.

**Hoje o admin não descobre falta de faixa no save.** A UI valida nome, efeito e duplicidade; a porta persiste sem perguntar à precedência. O signal apenas invalida cache. (`app/ponto/views_tipos_ausencia.py:61–95`; `app/ponto/services/ausencia.py:1301–1310`; `app/ponto/models.py:1235–1238`)

Se mudar **somente** `faixa_de`, o cadastro continuará salvando; a descoberta pode ocorrer já no primeiro lançamento, por `veto_de_lancamento:497`, ou na primeira leitura dos fatos. Não necessariamente será uma leitura visível ao admin, pois há capturas que continuam o processamento. (`app/ponto/services/ausencia.py:377–383`; `app/ponto/precedencia.py:497`; `app/ponto/services/espelho.py:645–651`; `app/ponto/services/cartorio.py:2032–2039`)

O encaixe para evitar isso é: **recusa nasce em `faixa_de`; a porta de cadastro consulta essa autoridade antes de criar/ativar, e a UI traduz a recusa nomeadamente**. Não copiar o mapa para a UI. A porta existente para isso é `gravar_tipo_ausencia`; essa validação ainda não existe. (`app/ponto/precedencia.py:76–79`; `app/ponto/services/ausencia.py:1301–1310`) Implementação desse encaixe: **[nao verificado]**.

Não colocar essa exigência como condição de registrar batida: a lei proíbe barrá-la, e o caminho existente grava antes de acionar fechamento, capturando falhas posteriores. O vigia precisa expor o problema mesmo quando esse processamento continua. (`CLAUDE.md:143–144`; `app/ponto/registro_batida.py:157–160`, `:170–183`)

A referência antiga do anexo a uma guarda em `models.py:884` **não corresponde à cópia**: nessa linha há o laço de derivação de minutos; o `save` lido não valida faixa. (`app/ponto/models.py:868–908`)

**5. Contraexemplos e dinheiro**

- **Rótulo depende do default hoje:** desconhecido vencedor recebe faixa `atestado`, e o resultado usa `ROTULOS[faixa]`, produzindo “Atestado”. (`app/ponto/precedencia.py:66–79`, `:403–405`)
- **`TRAVESSA_A_FOLGA` não libera folga:** contém somente `ferias`; tanto `atestado` quanto `folga` produzem `False`. A troca não autoriza atravessar outra folga humana. (`app/ponto/precedencia.py:496–518`)
- **A mudança não é apenas cosmética:** `folga` passa a perder para feriado; muda faixa/rótulo, status nos ramos sem lavra e seleção de cobrança espúria por folga. (`app/ponto/precedencia.py:46`, `:396–405`; `app/ponto/status_dia.py:308–316`; `app/chamados/services/feriado_reavaliar.py:33–39`)
- **Dinheiro usa outra pergunta:** nesse caminho, fechamento lê cobertura e `efeito`; `ABONA` justifica o dia e efeito diferente de `DESCONTA` suprime previsto. Há inclusive tratamento atual de efeito desconhecido `None`, independente do default de faixa. (`app/ponto/services/fechamento.py:675–701`)
- **Export lê o lavrado:** não recalcula a folha para emitir o TXT. (`app/folha/export.py:4–9`)
- **Exportada não é trava financeira geral:** o recálculo deixou de ser recusado por estar exportada; existe guarda específica para setembro/2026. O TXT anterior preserva conteúdo/hash ao ser invalidado. (`app/ponto/services/fechamento.py:485–498`, `:36–43`, `:558–566`; `app/folha/models.py:143–162`)

Portanto, trocar **somente a faixa** de `folga` não muda diretamente a regra de abono citada; afirmar dinheiro zero em toda competência exportada exigiria medição que a sonda não contém. Impacto monetário, competências atingidas e números de frota: **[nao verificado]**.

## 11/11 20:45 · sessao 9c60ffe32f88 · - · - · origem=Ronald (tmux)

**O RED anterior, com as duas batidas previamente cadastradas e relógio depois da S, não cobre o nascimento durante o turno aberto.** A fábrica decide vencimento por instante (`cartorio.py:882`) e a API detecta anomalias antes da escrita (`app/api/views.py:434`, `:654`); o teste deve conservar essa ordem.

Caminho real disponível na cópia:

1. API executa `detectar_anomalias(colab)` antes de registrar a batida: `app/api/views.py:434`.
2. Detector geométrico lê `batidas_apuraveis` e `parear_turnos`, selecionando turnos abertos: `app/ponto/utils_turno_orfao.py:28-35`; `app/chamados/detector_anomalias.py:31-36`.
3. Na E, API consulta turno aberto e `classificar_falta` ainda antes da escrita: `app/api/views.py:633-645`. Na S, faz a consulta correspondente: `:605-628`.
4. Registra a batida pela porta: `app/api/views.py:654-668`.
5. Depois da escrita, reaproveita os motivos detectados anteriormente e chama `criar_ou_atualizar_disputa`: `app/api/views.py:757-766`.
6. A porta chama o gerador tanto para disputa nova quanto existente: `app/chamados/services/disputa_emissao.py:669-682`.
7. Gerador traduz o motivo recebido em pergunta e chama `PerguntaDisputa.abrir`; não reaplica o teto do fim nessa chamada: `app/chamados/services/disputa_emissao.py:312-315`, `:444-452`.

**Isso não identifica o chamador histórico da pk52340:** API e signal/cartório podem executar no ato da E. `pergunta_emitida`, usuário415 e coincidência de segundo não trazem stack, origem nem contexto. Atribuição exclusiva à API ou ao signal: **[nao verificado]**.

Guardas existentes que um RED temporal precisa exercitar:

- `classificar_falta` cala antes do fim, **desde que encontre vínculo e tipo de escala**: `app/escala/servico_jornada.py:385-389`.
- Fábrica lê faltantes da ata e só permite os marcos vencidos: `app/chamados/services/fabrica_da_celula.py:64-71`.
- Vencimento é instante do marco encadeado +30min: `app/ponto/services/cartorio.py:879-883`.
- DNA/marcos/vínculo históricos da célula133672 e contexto da disputa no nascimento não estão no dado. Qual guarda falhou ou foi contornada para emitir às19:00:41: **[nao verificado]**.

**RED mínimo proposto, ainda não executado [nao verificado]:**

- Fixture com célula/vínculo noturno real 19→07; se necessário à reprodução, disputa aberta com pergunta anterior, conservando o agregado histórico.
- Relógio D19:00: registrar somente E pela API/porta real; deixar os signals reais rodarem. Assert: nenhum `orfao_14h` para o fim desse turno ainda futuro.
- Avançar para D+1 06:30: executar cartório/reconciliador reais. Assert: continua sem cobrança do fim ainda futuro.
- Avançar para D+1 07:00: registrar S pela mesma porta; deixar julgamento e reconciliação reais rodarem.
- Assert: um turno fechado ancorado em D, nenhum `orfao_14h` indevido nascido para esse turno. Não basta assertar ausência de pergunta **viva** ao final: isso aprovaria nascimento falso seguido de absolvição automática. Proposta **[nao verificado]**.

**Mutante pertinente proposto [nao verificado]:** permitir ao emissor criar `orfao_14h` a partir de um turno apenas aberto, sem obedecer ao teto do fim. A asserção imediatamente após E deve falhar. O mutante de “ignorar S em D+1 por recorte civil” do relatório anterior não explica a emissão comprovadamente anterior à S; sua participação neste caso é **[nao verificado]**.

Cuidado ao nomear a classe: **emissão prematura por desobediência ao juiz temporal** é hipótese testável; determinar o ramo histórico responsável continua **[nao verificado]**. “Não viu um par já disponível” é incompatível com nascimento às19:00 do dia anterior à criação da S, conforme dado fornecido **[nao verificado: sem arquivo:linha da sonda na cópia]**.

O teste existente `test_detector_noturno_s122` não cobre esse RED: seu helper replica `ultima.tipo` (`app/api/tests/test_detector_noturno_s122.py:20-25`), e o cenário19→07 testa esse helper local, sem endpoint, signals, cartório ou ordem temporal completa (`:27-33`).

_4037 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
