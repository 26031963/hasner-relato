# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 18:23** · publicado em **07/11/2026 18:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 18:23 · sessao 3a1ea14e2abb · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: cura parcial.** O veto de mérito foi centralizado, mas permanecem divergências de universo, caminhos com ordem de trava invertida, leitores que confundem conversa com turno e diagnósticos descartados pelos comandos. (`app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1162`; `app/chamados/services/texto.py:35`; `app/ponto/services/veredito_celula.py:177`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`)

Identidade independente do commit/pai, execução da suíte informada, RED→GREEN histórico e ocorrência produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

**1. NAO PROCEDE segundo escritor produtivo de duplicata localizado; PROCEDENTE ressalva na declaração do mérito.**

O caminho produtivo encontrado é único:

`reapontar_resolvedora` / `_migrar_sob_trava` / `backfill.aplicar` → `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade(VIA_DUPLICATA)`. Todos consultam `plano_de_retratacao`, cujo veto é `merito_perdedor`; não localizei cópia produtiva desse veto nem UPDATE direto marcando Pergunta como duplicata. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:1068`; `app/chamados/services/fio.py:1005`)

`retratada_em` pertence a **Batida**. A Pergunta sai do fato pela via `duplicata` ou `motivo_retratado`; remover motivo é outro ato, sem eleição de canônica. (`app/ponto/models.py:67`; `app/chamados/models.py:2926`; `app/chamados/models.py:2031`)

No contraexemplo p1/p2/p3, com universo comum e p1 canônica:

| Escritor/caminho | Resultado |
|---|---|
| Migração | p1 permanece; p2 conserva `admin_decidiu` e resposta, aparece em `recusadas`; p3 vira duplicata. (`app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1360`; `app/chamados/services/fio.py:1361`) |
| Porta `retratar_para_canonica([p2,p3],p1)` | Preserva p2, declara seu mérito e retrata p3. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1049`; `app/chamados/services/fio.py:1051`) |
| Backfill/dedup apply | O mérito de p2 recusa o **apply inteiro**: p1/p2/p3 ficam como estavam; retratadas=0. Compatível com CLASSE-NA-CATRACA (3). (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`) |
| Colisão ao datar p1 | Recusa sem retratar ninguém; se p1 ganhou a data nessa tentativa, volta a NULL. Uma outra linha encerrada também basta para recusar antes do exame de mérito. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`) |

**Ressalva:** se a canônica é BATIDA sem episódio, o plano retorna antes de examinar mérito. A perdedora de mérito permanece protegida, mas não aparece em `merito_recusado`: aparece somente a PK da canônica em `canonica_sem_episodio`. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/services/fio.py:1033`)

**2. PROCEDENTE: há linhas preservadas sem resolver a colisão e caminhos sem garantia de resolvedora no destino.**

| Chamador | Linha preservada e linha `p`; declaração |
|---|---|
| Migração, `p` canônica | Havendo outra intocável no destino, `p` permanece no velho; ninguém é retratado; a recusa de `p` é declarada. (`app/chamados/services/fio.py:1346`; `app/chamados/services/fio.py:1350`) |
| Migração, canônica externa | Pode retratar `p` sem mover a canônica para o novo. Declara duplicatas/recusas, mas não garante uma resolvedora ancorada no novo. (`app/chamados/services/fio.py:1328`; `app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1361`) |
| Colisão ao datar | Mérito, ausência de episódio, outra canônica ou linha encerrada recusam; `p` volta sem dia e as outras ficam intocadas. Não nasce uma segunda identidade datada nessa tentativa. (`app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`) |
| Reapontamento de `p` já datada | Não verifica colisão: trava o colaborador, valida o destino e salva. Pode conservar duas perguntas do fato no destino sem declarar colisão. (`app/chamados/services/fio.py:854`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:868`) |
| Backfill/dedup apply | Mérito provoca rollback integral. Ausência de episódio é tratada como contador informativo: as linhas podem continuar juntas, com retorno de sucesso. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`) |
| DRY | Não escreve. O dedup conta perdedoras do horizonte sem consultar o plano; o backfill lê do plano somente mérito. A apresentação não corresponde integralmente ao que será retratado. (`app/chamados/management/commands/dedup_perguntas_fato.py:72`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`) |

**Contraexemplo residual concreto:** L sem episódio no velho; A do episódio E no novo; mesmo motivo/dia; ambas vivas. `do_fato(E)` inclui L, mas `universo_da_linha(L)` retorna somente L. A migração não vê A como conflito, migra L e declara sucesso, deixando L e A no novo. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1334`; `app/chamados/services/fio.py:1355`)

Quando a canônica está fora dos dois chamados, a migração pode deixar o novo sem pergunta viva daquele fato; o teste “sem mérito” exige justamente retratar p2 e p3, mantendo p1 validada fora deles. O chamador `reabrir` registra o resultado e mantém o velho fechado; não procura outra resolvedora para cada recusa. (`app/chamados/tests/test_e1_v9.py:175`; `app/chamados/tests/test_e1_v9.py:179`; `app/chamados/tests/test_e1_v9.py:183`; `app/chamados/models.py:1397`; `app/chamados/models.py:1408`; `app/chamados/models.py:1409`)

Resolvedora efetivamente costurada a **outro fato** por esses contraexemplos: **[nao verificado]**. A lacuna demonstrada é duplicidade ou ausência de ancoragem, conforme os ramos acima.

**3. NAO PROCEDE a antiga inversão no corpo de `remover_motivo` e nos três pontos de justificativa; PROCEDENTE permanência da classe.**

`remover_motivo` agora adquire Colaborador; depois Disputas explícita/abertas; Chamados pais/vivos/destinos; finalmente todas as Perguntas listadas. Relê as precondições antes dos carimbos e do UPDATE da Disputa. (`app/chamados/models.py:1996`; `app/chamados/models.py:1998`; `app/chamados/models.py:2001`; `app/chamados/models.py:2031`; `app/chamados/models.py:2035`)

A implementação de `trava_na_ordem` segue Colaborador → Disputa → Chamado → Pergunta. Os receivers de Posto e âncora delegam à mesma porta; não têm escritor alternativo da remoção. Chamadas diretas entram no mesmo método. (`app/chamados/models.py:2525`; `app/chamados/models.py:2532`; `app/chamados/models.py:2545`; `app/chamados/models.py:2549`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/tests/test_porta_remocao_motivo.py:255`)

**Limite:** o atomic da remoção termina antes de `self.fechar`. Sem atomic externo são duas transações; o teste do sinal fornece atomic externo e não demonstra essa chamada avulsa como ato indivisível. (`app/chamados/models.py:2035`; `app/chamados/models.py:2042`; `app/chamados/tests/test_e1_v9.py:276`)

Justificativa passou a incluir as Disputas abertas nos três pontos; a aprovação antecipa essa aquisição à trava da própria Justificativa. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:80`; `app/ponto/services/justificativa.py:129`; `app/ponto/services/justificativa.py:140`)

**Outro caminho invertido:** alterar texto de Chamado encerrado, com via diferente de `competencia_trancada`, faz atomic → **UPDATE Chamado** → S143 → aquisição de **Colaborador/Disputas**. O receiver não exige mudança de status nem filtra `update_fields`. Portanto permanece Chamado → Colaborador/Disputa. Portas de contexto e vínculos também salvam Chamado sob atomic sem antecipação local. (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/contexto.py:75`; `app/chamados/services/vinculos.py:86`)

**Outra falha no apply:** `_univ` usa a primeira PK como chave. Grupos α=`[L,Aα,Aα2]` e β=`[L,Bβ]`, com L menor PK compartilhada, sobrescrevem um universo. As travas podem cobrir somente L/Bβ, mas o laço posterior ainda processa α e retrata Aα2 quando Aα respondida vence. Aα2 — e eventual Disputa exclusiva de α — ficam fora da antecipação prometida. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:212`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:214`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:222`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`)

Há ainda INSERT/UPDATE sob atomic sem trava local em `Chamado.criar`, `PerguntaDisputa.save` e `ressuscitar_perguntas`. Isso demonstra alcance insuficiente de A/A2; não demonstra, sozinho, que todos os respectivos chamadores chegam destravados. (`app/chamados/models.py:1077`; `app/chamados/models.py:3151`; `app/chamados/models.py:1530`)

**4. NAO PROCEDE a antiga união α/β pela expansão de L; PROCEDENTE divergência residual entre universos.**

| Entrada | Retorno |
|---|---|
| BATIDA sem episódio L | `universo_da_linha(L)=[L]`. (`app/chamados/services/fio.py:1162`) |
| BATIDA com episódio E | Linhas de E/motivo, independentemente do dia, mais legadas sem episódio do **dia da linha consultada**, acrescentando a própria linha. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1166`) |
| MARCO | Dia/motivo/instância derivada, mais a própria linha. Nascimento e leitura usam a mesma derivação de instância. (`app/chamados/models.py:2610`; `app/chamados/models.py:3168`; `app/chamados/models.py:3175`; `app/chamados/services/fio.py:1166`) |
| Conversa | O helper não tem ramo geral próprio: pode consultar linhas factuais e acrescentar a conversa; BATIDA sem origem retorna somente ela. Os consumidores atuais evitam essa mistura: grupo exclui conversa e migração usa explicitamente `[p]`. (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1329`) |

`universo_do_grupo` expande cada linha inicial, une por PK e ordena. L não expande novamente pelo dia; desapareceu o contraexemplo específico L+A(α)+B(β). (`app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1152`; `app/chamados/services/fio.py:1162`)

**Persistem duas assimetrias:**

- L isolada não vê E, enquanto E vê L: causa a duplicidade da migração descrita no item 2. (`app/chamados/services/fio.py:1162`; `app/chamados/models.py:2577`)
- Mesmo episódio E com A(d1), A2(d2) e L2(d2) respondida: `universo_da_linha(A)` encontra A/A2, mas não L2; `universo_do_grupo([A,A2])` expande A2 e encontra L2. Migração e backfill podem escolher canônicas diferentes para o mesmo episódio. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1336`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`)

**5. PROCEDENTE: o censo de `data_evento` continua incompleto.**

Os escritores declaram dois significados: `abrir` recebe dia do turno e fixa `conversa=False`; `abrir_conversa` lê o dia civil e fixa `True`; `_nascer` persiste. Reapontamento preenche NULL; rechaveamento altera dia existente. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:957`)

Os filtros novos corrigem os leitores nomeados no diff, inclusive o I22 composto: conversa deixou de satisfazer seu `any` factual. **NAO PROCEDE a permanência daquela cegueira específica.** (`app/chamados/juizes.py:77`; `app/chamados/juizes.py:941`; `app/chamados/services/fabrica_da_celula.py:127`; `app/chamados/services/relavra_via.py:154`; `app/chamados/services/pergunta_no_app.py:93`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1045`)

Entretanto, permanecem decisores fora do censo:

| Leitor | Consequência |
|---|---|
| `fechar_o_fio_do_dia` | Seleciona por data sem excluir conversa; fecha por admin e registra SILENCIO. Decidir turno D pode silenciar conversa emitida em D sobre outro assunto. (`app/ponto/services/veredito_celula.py:177`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:192`) |
| C6 `_pergunta_viva_calc` | Consulta janela e `classificar_falta` pelo dia civil; pode julgar a vitalidade da conversa pela falta de outro turno. (`app/chamados/juizes.py:1207`; `app/chamados/juizes.py:1211`) |
| `_pergunta_materializada_de_fato` | Consulta janela/falta pelo mesmo campo sem discriminador. (`app/chamados/juizes.py:664`; `app/chamados/juizes.py:671`) |
| Invariante F1 | Inclui conversas, escolhe a primeira data de pergunta e julga a falta desse dia; a conversa pode determinar a data examinada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1651`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`) |
| I21/I21a e F3 | Usam cobertura de ausência/falta sobre a data sem excluir conversa, alterando a contagem dos invariantes. (`app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1777`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`) |
| `limbo_folgas.filhos_no_defeito` | Uma conversa do dia civil pode segurar outro filho pelo `fio_vivo`. (`app/chamados/services/limbo_folgas.py:201`; `app/chamados/services/limbo_folgas.py:204`) |

Não exige flag corrompida: o emissor cria conversa legítima com o motivo original quando não encontra dia factual. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/disputa_emissao.py:439`)

O censo também reconhece que `data_do_fato_de` compara civil com turno e pode causar recusa de materialização com escrita de `recusa_motivo`. Portanto “nenhum planta, retrata nem silencia” não equivale a ausência de decisão/escrita. (`app/chamados/services/fio.py:1106`; `app/chamados/services/fio.py:1109`; `app/chamados/services/materializacao.py:1189`; `app/chamados/services/materializacao.py:1210`)

**6. NAO PROCEDE a antiga classificação de NULL como abaixo; PROCEDENTE divergência de contagem e apresentação.**

| Retorno | Quem escreve | Quem lê |
|---|---|---|
| `retratadas` | Soma retratações efetivas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`) | Ambos os comandos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`) |
| `abaixo_do_horizonte` | Perdedoras **datadas** fora do horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:235`) | Backfill imprime; dedup somente exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
| `perdedora_sem_dia` | Perdedoras NULL. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`) | Dedup somente classifica como não recusa; nenhum dos dois resumos apresenta a lista. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
| `canonica_sem_episodio` | Plano devolve a **PK da canônica**, não as perdedoras bloqueadas. (`app/chamados/services/fio.py:1031`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`) | Mesma classificação de não recusa, sem apresentação nos resumos. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
| `sem_episodio_intocada` | Plano devolve perdedoras legadas. (`app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:245`) | Mesma omissão de apresentação. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`) |
| `merito_recusado` | Plano; apply acumula. (`app/chamados/services/fio.py:1036`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`) | Migração, colisão, DRY backfill e ambos os applies. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:903`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:421`; `app/chamados/management/commands/dedup_perguntas_fato.py:95`) |

**Não é uma partição das linhas:** L canônica sem episódio bloqueando A1/A2 produz somente `[L]`; A1/A2 ficam fora de todas essas listas de destino. L também pode aparecer nos dois contadores de episódio, sendo canônica de um grupo e perdedora de outro; a deduplicação é por lista, não entre categorias. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`)

**DRY ≠ apply:** L canônica legada + A(E) dá um excedente no DRY do dedup e zero retratada no apply; o motivo fica escondido na saída. O DRY backfill igualmente descarta do plano os resultados sem episódio. (`app/chamados/management/commands/dedup_perguntas_fato.py:73`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`)

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal do apply:** qualquer recusa acumulada causa rollback e zera os contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`)

**7. PROCEDENTE insuficiência dos selos fora do LIMITE declarado; NAO PROCEDE ausência geral de mordida.**

| Catraca | Burla adicional |
|---|---|
| A | SQL literal com `FOR  UPDATE`, dois espaços, escapa da substring exata; não depende de concatenação ou tabela dinâmica. (`app/chamados/tests/test_e1_v7_catracas.py:129`) |
| A2 | Alias `gravar=self.save; gravar()` e decorador `@transaction.atomic` escapam: procura `ast.With` e nomes de chamadas. Além disso, para na primeira trava, mesmo que ela adquira só Colaborador. (`app/chamados/tests/test_e1_v7_catracas.py:319`; `app/chamados/tests/test_e1_v7_catracas.py:323`; `app/chamados/tests/test_e1_v7_catracas.py:327`) |
| B | `rec: list=[p for p in ps if p.ok]` seguido do juiz escapa: propagação considera `Assign`, não `AnnAssign`. Alias local do juiz também escapa. (`app/chamados/tests/test_e1_v7_catracas.py:181`; `app/chamados/tests/test_e1_v7_catracas.py:194`;

[truncado em 20000 de 29192 caracteres]

## 07/11 18:19 · sessao 1c46dadb09e8 · - · - · origem=Ronald (tmux)

**PROCEDENTE a perda de universo em `_univ`.** Os grupos α=`[L,Aα,Aα2]` e β=`[L,Bβ]`, ambos começando pela PK de L, colidem na chave do dicionário; β sobrescreve α. Assim, `pks_c` pode conter somente L/Bβ, deixando Aα, Aα2 e eventual Disputa exclusiva de α fora das travas explícitas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:212`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:214`)

O processamento posterior continua percorrendo **todos** os grupos. Com Aα respondida, ela vence; Aα2 é planejada para retratação, e L fica intocada por falta de episódio. A escrita alcança Aα2 sem antecipar sua trava pela porta: `aplicar → retratar_para_canonica → retratar_duplicata → carimbar_identidade → save`. Isso contradiz a afirmação de que o universo inteiro foi travado antes dos vetos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:222`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:1068`; `app/chamados/models.py:2923`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:207`)

Reprodução em banco: **[nao verificado]**.

## 07/11 18:19 · sessao 348184f1c6dc · - · - · origem=Ronald (tmux)

**3 — PROCEDENTE residual de ordem; NAO PROCEDE permanência da inversão no corpo de `remover_motivo`.** Ele trava Colaborador, depois Disputas explícita/abertas, Chamados pais/vivos/destinos e todas as Perguntas antes de qualquer carimbo; relê as precondições sob trava. (`app/chamados/models.py:1995`; `app/chamados/models.py:1998`; `app/chamados/models.py:2001`; `app/chamados/models.py:2031`). A implementação da porta adquire cada tabela nessa ordem. (`app/chamados/models.py:2525`; `app/chamados/models.py:2532`; `app/chamados/models.py:2545`; `app/chamados/models.py:2549`).

Posto e âncora somente consultam e delegam a essa porta; seus UPDATE anteriores são de Posto/Escala, fora das quatro tabelas. Produção tem esses dois chamadores; chamadas diretas estão nos testes. (`app/colaboradores/signals.py:71`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:155`; `app/escala/signals.py:161`; `app/chamados/tests/test_porta_remocao_motivo.py:255`).

**Ressalva concreta:** o atomic de `remover_motivo` termina antes de `self.fechar`; sem atomic externo, remoção e fechamento ficam em transações diferentes. O teste do sinal adiciona atomic externo e não cobre essa separação. (`app/chamados/models.py:2035`; `app/chamados/models.py:2042`; `app/chamados/tests/test_e1_v9.py:276`).

**Justificativa — NAO PROCEDE a permanência da omissão apontada:** os três pontos incluem `abertas_do_colaborador=True`; aprovação chama a antecipação antes da trava da justificativa. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:80`; `app/ponto/services/justificativa.py:129`; `app/ponto/services/justificativa.py:140`).

**Outro caminho realmente invertido:** `reescrever_texto(ch)` em Chamado terminal, via diferente de `competencia_trancada`, texto alterado e sem trava externa: atomic → UPDATE Chamado → S143 → porta que adquire Colaborador/Disputas. O receiver não exige mudança de status nem examina `update_fields`. **Chamado → Colaborador/Disputa continua possível.** (`app/chamados/services/texto.py:31`; `app/chamados/services/texto.py:35`; `app/chamados/signals.py:81`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1683`). Contexto e vínculos também salvam Chamado sob atomic sem antecipação. (`app/chamados/services/contexto.py:75`; `app/chamados/services/contexto.py:100`; `app/chamados/services/vinculos.py:86`).

Outras escritas sob atomic sem trava local: INSERT de `Chamado.criar`, `PerguntaDisputa.save` e descarimbo em lote de `ressuscitar_perguntas`; este último depois pode reabrir Disputa. Isso demonstra escritores implícitos fora do alcance de A/A2, sem afirmar que todos os chamadores chegam destravados. (`app/chamados/models.py:1077`; `app/chamados/models.py:1530`; `app/chamados/models.py:1560`; `app/chamados/models.py:2147`; `app/chamados/models.py:3151`).

**7 — PROCEDENTE insuficiência dos selos fora dos limites declarados; NAO PROCEDE ausência geral de mordida.**

| Selo | Burla confirmada em execução pura do detector |
|---|---|
| A | SQL literal com tabela protegida e `FOR  UPDATE` — dois espaços — retorna `[]`: busca substring exata. (`app/chamados/tests/test_e1_v7_catracas.py:129`) |
| A2 | `gravar=self.save; gravar()` sob atomic retorna `[]`; só examina nomes de chamadas. Decorador `@transaction.atomic` também fica fora da procura por `ast.With`. (`app/chamados/tests/test_e1_v7_catracas.py:319`; `app/chamados/tests/test_e1_v7_catracas.py:323`) |
| B | `rec: list=[p for p in ps if p.ok]; canonica_do_grupo(rec)` retorna `[]`; só propaga `ast.Assign`. `juiz=canonica_do_grupo; juiz(recorte)` também escapa. (`app/chamados/tests/test_e1_v7_catracas.py:181`; `app/chamados/tests/test_e1_v7_catracas.py:194`; `app/chamados/tests/test_e1_v7_catracas.py:202`) |
| C | SQL UPDATE direto pelo cursor não pertence à lista de escritores; o único limite declarado de C é helper externo. (`app/chamados/tests/test_e1_v7_catracas.py:34`; `app/chamados/tests/test_e1_v7_catracas.py:214`; `app/chamados/tests/test_e1_v7_catracas.py:238`) |
| D | `p.carimbar_identidade("duplicata",u)` retorna `[]`; também `carimba=p.carimbar_identidade; carimba(P.VIA_DUPLICATA,u)`. A constante vale literalmente `"duplicata"`, mas o detector exige chamada com nome `carimbar_identidade` e argumento AST chamado `VIA_DUPLICATA`. (`app/chamados/models.py:2364`; `app/chamados/tests/test_e1_v7_catracas.py:280`) |

A2 aceita parar na primeira trava, mesmo que ela trave somente Colaborador e depois haja Pergunta → Disputa. O próprio mutante “curado” só insere `trava_na_ordem(self.colaborador_id)`, insuficiente para provar a ordem inteira. (`app/chamados/tests/test_e1_v7_catracas.py:327`; `app/chamados/tests/test_e1_v9.py:544`). Fluxo/helper estão declarados como limites, mas alias de método, decorador atomic e suficiência do conjunto travado não estão. (`app/chamados/tests/test_e1_v7_catracas.py:38`).

**Tracker — NAO PROCEDE que ainda registre só primeira tabela.** Agora deduplica por `(tabela,pk)`; para as consultas da porta, a primeira coluna é efetivamente PK. O UPDATE usual sonda o WHERE e remove os parâmetros do SET. (`app/chamados/tests/test_e1_v9.py:60`; `app/chamados/tests/test_e1_v9.py:80`; `app/chamados/tests/test_e1_v9.py:85`; `app/chamados/models.py:2533`). Os mutantes exigem detectar segunda Disputa e UPDATE sem Colaborador. (`app/chamados/tests/test_e1_v9.py:318`; `app/chamados/tests/test_e1_v9.py:328`).

**PROCEDENTE excesso de alcance se apresentado como todas as aquisições:** ele não intercepta INSERT, DELETE ou FOR NO KEY UPDATE; `vistas` não reseta quando uma transação/savepoint libera trava. Seu limite cita apenas executemany, FK do INSERT e ordem PK, e os testes usam atomic externo. (`app/chamados/tests/test_e1_v9.py:47`; `app/chamados/tests/test_e1_v9.py:51`; `app/chamados/tests/test_e1_v9.py:70`; `app/chamados/tests/test_e1_v9.py:85`; `app/chamados/tests/test_e1_v9.py:277`). Correspondência da sonda prévia com locks efetivamente obtidos depois de espera concorrente: **[nao verificado]**.

Executei os 30 testes puros dos detectores v7 + `CatracaAlcanceTest` v9 em memória, substituindo `SimpleTestCase` por `unittest.TestCase` e retirando somente imports entre testes: **30 OK**; as varreduras atuais A/B/C/D/A2 resultaram vazias. Resultado dessa execução: **[nao verificado por arquivo persistido]**. Ela verifica mordida sintética atual; RED→GREEN histórico e testes Django: **[nao verificado]**. [nao verificado]

**Relógio — NAO PROCEDE permanência do defeito novo localizado:** classes de banco v8/v9 estão congeladas; os testes puros usam datas literais e mocks. (`app/chamados/tests/test_e1_v8.py:46`; `app/chamados/tests/test_e1_v8.py:100`; `app/chamados/tests/test_e1_v8.py:130`; `app/chamados/tests/test_e1_v9.py:149`; `app/chamados/tests/test_e1_v9.py:221`; `app/chamados/tests/test_e1_v9.py:266`; `app/chamados/tests/test_e1_v9.py:337`; `app/chamados/tests/test_e1_v9.py:443`).

## 07/11 18:19 · sessao 1c46dadb09e8 · - · - · origem=Ronald (tmux)

**Itens 1/2/4/6 — consolidação só por código.**

1. **NAO PROCEDE segundo escritor produtivo de `VIA_DUPLICATA` localizado.** A escrita está em `fio.retratar_duplicata`, que chama `carimbar_identidade`; seu único chamador produtivo é `retratar_para_canonica`. Os consumidores são reapontamento, migração e apply do backfill. (`app/chamados/services/fio.py:1068`; `app/chamados/services/fio.py:1050`; `app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:274`)

   `retratada_em` pertence a **Batida**, não representa a retratação da Pergunta para uma canônica; a retratação da Batida passa por `retratar_batida`, e seu desfazimento por `anotar`. (`app/ponto/models.py:67`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`)

   **Contraexemplo nominal do aval:** migração mantém p1 canônica, preserva p2 com mérito e declara p2 recusada, retrata p3. O apply identifica o mesmo mérito, mas desfaz o ato inteiro: p1/p2/p3 permanecem como estavam, contadores zerados. Essa diferença decorre do envelope de tudo ou nada, não de cópia do veto. (`app/chamados/services/fio.py:1336`; `app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1360`; `app/chamados/services/fio.py:1361`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:290`)

2. **PROCEDENTE persistência de resultados sem eliminar as duas linhas, e declaração incompleta pelos comandos.**

   - Datar p com colisão e mérito/sem episódio recusa falando; restaura `p.data_evento=None` e não retrata as outras. Se p não é canônica ou alguma outra já está encerrada, também recusa antes de escrever. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:852`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:903`)
   - Na migração, p canônica com outra linha intocável no destino permanece no velho e sai recusada. Se a canônica é externa, p pode virar duplicata sem mover a canônica para o novo; o retorno registra duplicata/recusa, mas não garante uma resolvedora ancorada no novo. (`app/chamados/services/fio.py:1328`; `app/chamados/services/fio.py:1346`; `app/chamados/services/fio.py:1350`; `app/chamados/services/fio.py:1361`)
   - **Contraexemplo residual:** L sem episódio no velho, A de episódio E no destino, mesmo motivo/dia. A identidade consultada por E inclui L, porém `universo_da_linha(L)` devolve somente L; a migração não vê A como conflito, migra L e retorna sucesso, deixando L e A no destino. O reapontamento de pergunta já datada também não examina colisões: apenas trava colaborador, valida o destino e salva. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1334`; `app/chamados/services/fio.py:1355`; `app/chamados/services/fio.py:854`; `app/chamados/services/fio.py:868`)
   - O chamador `reabrir` mantém o velho fechado e apenas anexa nota/evento com o resultado da migração; não trata individualmente recusas nem procura outra resolvedora. (`app/chamados/models.py:1397`; `app/chamados/models.py:1400`; `app/chamados/models.py:1408`; `app/chamados/models.py:1409`)
   - No apply, linha sem episódio pode impedir retratação sem constituir recusa; duas linhas permanecem e o retorno é de sucesso com contador informativo. Os comandos descartam esse diagnóstico ao apresentar o resultado. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:242`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/dedup_perguntas_fato.py:97`)

3. **Item 4 — cura do contraexemplo L+A(α)+B(β), mas PROCEDENTE divergência residual dos universos.**

   - L BATIDA sem episódio: `universo_da_linha` retorna `[L]`. Episódio E: retorna todas as linhas daquele episódio/motivo, independentemente do dia, mais legadas sem episódio do dia da linha consultada. MARCO: usa dia/motivo e a instância derivada do motivo, acrescentando a própria linha caso ausente. (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1166`; `app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/models.py:2610`)
   - `universo_do_grupo` exclui conversas e linhas fora do fato, expande cada linha inicial, une por PK e ordena. A legada encontrada durante a expansão não é expandida novamente; portanto o contraexemplo específico que unia α e β pela legada foi eliminado. (`app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1152`; `app/chamados/services/fio.py:1162`)
   - **Divergência estática:** mesmo episódio E com A(d1), A2(d2), e legada L2(d2) respondida. `universo_da_linha(A)` encontra A/A2, mas não L2; `universo_do_grupo([A,A2])` expande também A2 e inclui L2. Migração iniciada por A pode escolher canônica diferente de backfill/DRY/abrir para d2. A chave BATIDA permite a diferença de dias; não é necessário inventar outro episódio. (`app/chamados/models.py:2577`; `app/chamados/models.py:2579`; `app/chamados/services/fio.py:1149`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1332`; `app/chamados/services/fio.py:1336`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`)
   - Conversa passada diretamente a `universo_da_linha` não recebe tratamento geral próprio: salvo BATIDA sem origem, busca linhas factuais por `do_fato` e depois acrescenta a conversa. `universo_do_grupo` a exclui; a migração evita esse caso por ramo explícito `[p]`. **Não localizado consumidor produtivo que entregue conversa ao helper nessa forma.** (`app/chamados/services/fio.py:1162`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1167`; `app/chamados/services/fio.py:1146`; `app/chamados/services/fio.py:1329`)
   - **NAO PROCEDE defeito MARCO multi-instância demonstrado:** o nascimento rejeita instância diferente daquela derivada do motivo; o leitor usa a mesma derivação. Duas pausas distintas continuam explicitamente reservadas à E2. (`app/chamados/models.py:3168`; `app/chamados/models.py:3175`; `app/chamados/models.py:2610`; `app/chamados/services/declaracao_texto.py:48`)

4. **Item 6 — PROCEDENTE diferença entre contadores, DRY e apresentação do apply.**

   | Valor | Escritor | Leitor produtivo |
   |---|---|---|
   | `retratadas` | Soma escritas efetivas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`) | Backfill e dedup. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`) |
   | `abaixo_do_horizonte` | Perdedoras datadas fora do horizonte; saída deduplica PKs. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:235`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`) | Backfill imprime quantidade; dedup apenas exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:430`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `perdedora_sem_dia` | Perdedoras NULL. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`) | Dedup apenas exclui das recusas; CLI backfill não imprime. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:429`) |
   | `canonica_sem_episodio` | Plano devolve **PK da canônica**, não das perdedoras bloqueadas. (`app/chamados/services/fio.py:1030`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`) | Apenas classificação de não recusa no dedup. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `sem_episodio_intocada` | PKs das perdedoras legadas. (`app/chamados/services/fio.py:1037`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:245`) | Apenas classificação de não recusa no dedup. (`app/chamados/management/commands/dedup_perguntas_fato.py:92`) |
   | `merito_recusado` | Plano/porta; apply acumula. (`app/chamados/services/fio.py:1033`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`) | Migração, colisão, DRY backfill e ambos CLIs apply. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:903`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:421`; `app/chamados/management/commands/dedup_perguntas_fato.py:95`) |

   A perdedora NULL deixou de ser contada como abaixo: **NAO PROCEDE a permanência desse defeito específico**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:235`)

   **Contador não particiona as linhas:** com L canônica sem episódio e A1/A2 perdedoras datadas no horizonte, o plano devolve somente `canonica_sem_episodio=[L]`; A1/A2 não aparecem como retratadas, sem dia, abaixo ou sem episódio intocadas. A PK L pode aparecer nos dois contadores sem episódio em grupos diferentes, como canônica de um e perdedora de outro. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1032`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:231`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:244`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`)

   O DRY do dedup conta toda perdedora no horizonte, sem consultar o plano: L canônica legada + A(E) produz DRY excedentes=1 e apply excedentes=0; o motivo desaparece da apresentação. O DRY backfill consulta o plano somente para `merito_recusado`, omitindo os resultados sem episódio. (`app/chamados/management/commands/dedup_perguntas_fato.py:72`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`; `app/chamados/management/commands/dedup_perguntas_fato.py:96`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:373`)

   Se a canônica é legada, o plano retorna antes de examinar mérito: uma perdedora `admin_decidiu` com resposta fica protegida, porém o mérito não é declarado como tal. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1033`)

Execução em banco e ocorrência produtiva destes contraexemplos: **[nao verificado]**.

## 07/11 18:18 · sessao 770621486b10 · - · - · origem=Ronald (tmux)

**Itens 5 e 9: PROCEDENTE a permanência de leitores que confundem dia civil de conversa com dia de turno; o censo continua incompleto.** As migrações enumeradas no diff existem, mas não cobrem os leitores abaixo. (`app/chamados/services/fio.py:1100`; `app/chamados/services/fio.py:1106`)

Contraexemplos fortes:

- **Escrita que silencia conversa por coincidência de dia civil:** `fechar_o_fio_do_dia` seleciona perguntas por `data_evento=data`, usando `q_vivas`, sem `conversa=False`; chama `fechar_por_admin` e registra SILENCIO. `q_vivas` tampouco exclui conversa. Assim, decidir um turno em D alcança uma conversa emitida em D sobre outro fato, deixando `admin_decidiu` e trilha de decisão daquele dia. (`app/ponto/services/veredito_celula.py:177`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:192`; `app/chamados/models.py:3074`; `app/chamados/models.py:3088`; `app/chamados/models.py:2797`)
- **O próprio C6 ainda julga conversa pela falta de seu dia civil:** `_pergunta_viva_calc` chega a `_dentro_da_janela_viva(p.data_evento)` e `classificar_falta(colab,p.data_evento)` sem discriminar conversa. Com carimbo derivado e sem resposta, pode ressuscitar pela falta de um turno que não é o assunto da conversa. O juiz de materialização também lê esse dia como turno. (`app/chamados/juizes.py:1207`; `app/chamados/juizes.py:1211`; `app/chamados/juizes.py:664`; `app/chamados/juizes.py:671`)
- **Invariante F1:** lê todas as resolvedoras, escolhe a primeira `data_evento`, antes de consultar a célula do chamado, e decide a absolvição pela lei daquela data. Uma conversa morta e muda pode determinar o dia civil escolhido e ocultar o furo do chamado. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1651`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1664`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1665`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1682`)
- **I21/I21a:** a consulta inclui conversa; a cobertura de ausência usa seu `data_evento` civil. Uma ausência aprovada naquele dia pode mandar uma resposta invisível de conversa para o piso, sem resolver o assunto da conversa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1571`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1578`; `app/chamados/management/commands/auditar_invariantes_chamados.py:160`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`)
- **F3:** universo por motivo e data, sem conversa, seguido de `classificar_falta` pelo dia civil. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1777`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1803`)
- **Recusa/retém outro fato:** `limbo_folgas.filhos_no_defeito` consulta todas as perguntas do dia e usa `fio_vivo` para segurar o filho; uma conversa daquele dia civil basta. O calendário também marca `_perg_viva[data_evento]`, influenciando a oferta de “Resolver dia”, sem excluir conversa. (`app/chamados/services/limbo_folgas.py:201`; `app/chamados/services/limbo_folgas.py:204`; `app/colaboradores/services/calendario.py:455`; `app/colaboradores/services/calendario.py:473`; `app/colaboradores/services/calendario.py:477`)

Não depende de corrupção da flag: existem conversas legítimas para motivos genéricos quando não há dia do fato; a porta é chamada com o motivo original. A âncora também nasce explicitamente como conversa. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/services/disputa_emissao.py:1272`)

Mapa de campos/valores:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `data_evento` | Fato: `abrir` exige dia e fixa `conversa=False`; conversa: `abrir_conversa` usa `localdate()` e fixa `True`; `_nascer` cria; reapontamento preenche NULL; rechaveamento altera dia existente. (`app/chamados/models.py:2641`; `app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:957`) | Identidade/horizonte distinguem conversa; resolvedora também. O diagnóstico `data_do_fato_de` compara os dois dias e materialização pode recusar e gravar DATA_EM_DISPUTA: esse leitor está declarado no censo. Os leitores acima não estão e ainda tratam civil como turno. (`app/chamados/models.py:2611`; `app/chamados/services/fio.py:1113`; `app/chamados/juizes.py:1976`; `app/chamados/services/fio.py:87`; `app/chamados/services/materializacao.py:1189`; `app/chamados/services/materializacao.py:1210`) |
| `conversa` | Duas portas fixam o valor; `_nascer` persiste e transmite ao proxy do juiz. (`app/chamados/models.py:2643`; `app/chamados/models.py:2654`; `app/chamados/models.py:2685`; `app/chamados/models.py:2752`) | Sem identidade de fato: filtros/índices e derivador de chave respeitam; resolvedora não compara seu civil ao turno. Não é respeitado pelos leitores acima. (`app/chamados/models.py:2611`; `app/chamados/models.py:3165`; `app/chamados/models.py:3213`; `app/chamados/juizes.py:1976`) |
| `via_resolucao` | `_escrever_carimbo`; descarimbo em lote; exceção `encerrar_por_fato_no_espelho`; relavra. (`app/chamados/models.py:2834`; `app/chamados/models.py:2995`; `app/chamados/models.py:3004`; `app/chamados/services/relavra_via.py:125`) | C6 lê identidade/terminalidade, modelo exclui vias fora do fato, mérito lê `admin_decidiu` mais resposta, retratador lê idempotência. `admin_decidiu` sozinho não elimina resposta pendente; duplicata carimbada elimina antes da resposta. (`app/chamados/juizes.py:1155`; `app/chamados/juizes.py:1160`; `app/chamados/juizes.py:1180`; `app/chamados/models.py:2611`; `app/chamados/services/fio.py:1005`; `app/chamados/services/fio.py:1059`) |
| `retratada_em` | **Campo de Batida**, escrito por `retratar_batida` via `anotar`, e limpo pelo desfazer humano também via `anotar`. Não é campo de Pergunta. (`app/ponto/models.py:67`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Batida fora da apuração; juiz exige testemunha viva; timestamp alimenta frescor da lavra. A retratação da Pergunta é sua via terminal, não esse timestamp. (`app/ponto/turnos.py:2849`; `app/chamados/juizes.py:726`; `app/chamados/juizes.py:781`; `app/chamados/veredito_lavrado.py:140`; `app/chamados/models.py:2926`) |
| `recusa_motivo` | Duplicata via `carimbar_identidade`; recusa humana; justificativa da resposta; pendência; recusa de materialização. (`app/chamados/services/fio.py:1069`; `app/chamados/models.py:2921`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/materializacao.py:847`; `app/chamados/services/materializacao.py:1097`; `app/chamados/services/materializacao.py:1099`) | É texto de proveniências diferentes, dívida já declarada; explicador mostra; I18/I19 usam presença como dono; devolução de validação copia para ato; flip automático extrai batida vizinha por regex. Portanto não tem significado único de “recusa por mérito da retratação”. (`app/chamados/services/validacao.py:42`; `app/chamados/services/explicador.py:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`; `app/chamados/services/validacao.py:208`; `app/ponto/services/flip_auto.py:132`) |
| `batida_origem` | Derivada no nascimento a partir de `contexto.ultima_batida.id`, com família e colaborador conferidos; `_nascer` aceita/propaga a origem e inserção preenche quando ausente. (`app/chamados/models.py:2696`; `app/chamados/models.py:2698`; `app/chamados/models.py:3177`; `app/chamados/models.py:3190`; `app/chamados/models.py:3192`; `app/chamados/models.py:3196`) | Chave do episódio em `do_fato`, índice e agrupamento; `_sem_episodio` distingue acervo. Contexto é mutável e a coluna preserva o episódio de nascimento. (`app/chamados/models.py:2574`; `app/chamados/models.py:2579`; `app/chamados/models.py:3218`; `app/chamados/services/fio.py:1083`; `app/chamados/services/fio.py:1012`; `app/chamados/models.py:3185`) |
| `Disputa.motivos/contexto` | Kernel abre/agrega motivos e funde contexto; métodos removem/respondem motivos, anotam/renovam contexto, reabrem; emissores manuais complementam motivos. (`app/chamados/services/disputa_emissao.py:541`; `app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:562`; `app/chamados/services/disputa_emissao.py:589`; `app/chamados/models.py:1936`; `app/chamados/models.py:2033`; `app/chamados/models.py:2074`; `app/chamados/models.py:2092`; `app/chamados/models.py:2129`; `app/chamados/services/disputa_emissao.py:1267`) | Emissão usa motivo/data/contexto e última batida; materialização e juiz usam contexto como fallback de data; derivador de episódio lê última batida só no nascimento; auditor verifica coerência e catálogo. `motivos` é escopo agregado, não lista de perguntas vivas. (`app/chamados/services/disputa_emissao.py:345`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/services/materializacao.py:343`; `app/chamados/services/materializacao.py:472`; `app/chamados/juizes.py:370`; `app/chamados/models.py:3185`; `app/chamados/management/commands/auditar_disputas.py:88`; `app/colaboradores/services/drawer.py:75`) |

**Relógio fora da borda ainda existe no código atingido:** embora testes novos congelem, portas ainda leem `now` internamente e `abrir_conversa` usa `localdate`; lavrar lê `timezone.now` e `expira_em` usa `date.today`. A afirmação de inexistência global dessa reincidência não se sustenta; se o recorte for somente fixtures novas, o agente de selos verifica os decoradores. (`app/chamados/models.py:2654`; `app/chamados/models.py:2797`; `app/chamados/veredito_lavrado.py:228`; `app/chamados/veredito_lavrado.py:112`)

Não demonstrei segundo juiz **registrado** duplicado por esta volta; afirmar inexistência semântica geral permanece **[nao verificado]**.

## 07/11 18:18 · sessao 4c6d1511766f · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Cura aplicada: toda chamada estática aos dois pareadores, em qualquer função do motor/modelo/serviço DiaPago, passa a contar fora da composição do juiz. Cada chamada publica `arquivo::funcao`, sítio e linha; o gap permanece separado ([instrumento](/home/ronald/wt-codex-motor-le-o-turno-instrumento-181155/app/core/medida_turno_do_par.py:405)). [nao verificado]

Medição anterior à edição: exatamente estas duas portas:

| Arquivo:linha | Função | Destino |
|---|---|---|
| `ponto/motor_calculo_v2.py:503` | `turnos_via_autoridade` | `parear_turnos` |
| `ponto/motor_calculo_v2.py:882` | `MotorBase._turnos_do_juiz` | `turnos_de_batidas` |

DiaPago está definido em `ponto/models.py:281`. A lavratura importa o modelo em `ponto/services/dia_pago.py:163` e monta o dia por `_dia_de` em `:174`; a grade é importada em `:443` e usada em `:464`.

Resultados executados em memória:

- Original: **35 testes, rc=0**; contadores **9/85**, registros e linhas preservados (`core/tests/test_medida_turno_do_par.py:553`, `:569`).
- Antes da cura: M1/M6 produziram falhas de asserção. Após a cura, sobre fontes reais: **M1 e M6 → 10/86; M7 → 9/85** (`core/tests/test_medida_turno_do_par.py:739`, `:748`, `:760`).
- Os **14 mutantes saíram rc=1 por asserção**. Corpo do runner: `core/tests/test_medida_turno_do_par.py:19`; comando individual de cada execução:

| Mutante | rc | Comando em arquivo:linha |
|---|---:|---|
| composicao | 1 | `core/tests/test_medida_turno_do_par.py:73` |
| linhas | 1 | `core/tests/test_medida_turno_do_par.py:74` |
| gap | 1 | `core/tests/test_medida_turno_do_par.py:75` |
| classificar | 1 | `core/tests/test_medida_turno_do_par.py:76` |
| P1-linha-unica | 1 | `core/tests/test_medida_turno_do_par.py:77` |
| P2-len1 | 1 | `core/tests/test_medida_turno_do_par.py:78` |
| Q1-lines10 | 1 | `core/tests/test_medida_turno_do_par.py:79` |
| Q2-len2 | 1 | `core/tests/test_medida_turno_do_par.py:80` |
| Q3-tuple | 1 | `core/tests/test_medida_turno_do_par.py:81` |
| Q4-self | 1 | `core/tests/test_medida_turno_do_par.py:82` |
| R1-tres-funcoes | 1 | `core/tests/test_medida_turno_do_par.py:83` |
| R2-portas-fixas | 1 | `core/tests/test_medida_turno_do_par.py:84` |
| R3-so-motor | 1 | `core/tests/test_medida_turno_do_par.py:85` |
| R4-conta-composicao | 1 | `core/tests/test_medida_turno_do_par.py:86` |

**Campos novos: nenhum.** `censo_fontes` escreve `chave`, `criterio`, `sitio` e `linhas` em `core/medida_turno_do_par.py:412`. A catraca lê identidade/sítio/critério em `core/tests/test_medida_turno_do_par.py:563`, linhas em `:571`, e os testes MORDE conferem a publicação em `:781`.

## 07/11 18:13 · sessao 6364bb5ec91d · MOTOR-LE-O-TURNO-INSTRUMENTO · censo · origem=Code

As referências aos anexos contam linhas desde o início de cada bloco fornecido. A confirmação dos commits posteriores nesta cópia-base é [nao verificado]. [nao verificado]

1. **O1 — portas novas fora das funções fixas**

   **Pergunta:** “O instrumento cumpre o criterio LITERAL? Algum item conta algo fora de A1/B1/nomeados, ou deixa de contar?”; “Cite algum mutante do instrumento que passaria em todos os testes.” (`v1.q:7`, `v1.q:10`) [nao verificado]

   **Resposta:** diagnóstico das novas chamadas M1/M6: **ausente**. O trecho correlato trata outra forma de pareador novo: “Um `range(..., 2)` novo sobe contador e dispara RED.” (`v1.r:34`; `triagem_opus1.txt:2`)

   **Triagem:** descarte específico de M1/M6: **ausente**. A avaliação geral foi: “1 NAO PROCEDE no criterio (9/85, 9/89 reproduzidos); a garantia incompleta e os itens 2 e 4.” (`triagem_v1.txt:2`)

   **Classe (a):** omissão de contagem e mordida estavam perguntadas; o Codex examinou o incremento por `range` novo e deixou passar o incremento por chamada nova à autoridade. (`v1.q:7`, `v1.q:10`; `v1.r:34`; `triagem_opus1.txt:2`)

2. **O2 — leitor host e publicação no ESTADO ausentes**

   **Pergunta:** cobrança explícita dos itens contratuais “leitor `bin/*_estado.py` + linha no ESTADO”: **ausente**. Houve a pergunta correlata: “QUEM ESCREVE e QUEM LE cada valor que o diff grava ou le”. (`v1.q:15`; `triagem_opus1.txt:3`)

   **Resposta:** diagnóstico da entrega contratual faltante: **ausente**. O Codex registrou apenas: “Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**.” (`v1.r:104`) [nao verificado]

   **Triagem:** descarte explícito da obrigação contratual: **ausente**. Sobre o inventário de leitores: “8 tabela recebida, sem achado.” (`triagem_v1.txt:9`)

   **Classe (d):** a obrigação específica de entregar a publicação host não foi perguntada nem diagnosticada; “publicação não verificada” não constitui esse achado. A restrição do construtor a `app/` não impedia a leitura pelo verificador. (`v1.q:15`; `v1.r:104`; `triagem_opus1.txt:3`; `CLAUDE.md:341`, `CLAUDE.md:351`)

3. **FILA — E2 troca quatro identidades B1 no rebase, conservando o total**

   **Pergunta:** sobre essa troca concreta: **ausente**. A pergunta semelhante era sobre outro registro: “COMPOSICAO_REGISTRADA congela a lista inteira? Uma troca de identidade com o mesmo tamanho e cobrada?” (`v2.q:6`)

   **Resposta:** identificação dos quatro B1 trocados: **ausente**. A proteção geral de identidade foi reconhecida: “A igualdade percorre todos os itens dos dois contadores: A1, B1 e os três nomeados, com identidade completa e listas inteiras.” (`v3.r:154`)

   **Triagem:** descarte anterior desse evento: **ausente**. O enquadramento final diz: “ao rebasear, 4 itens B1 trocam de identidade com total igual -- a E2 classifica, nao copia”. (`triagem_opus1.txt:4`)

   **Classe (c), quanto ao evento concreto:** faltam as quatro identidades e as árvores comparadas; a classe geral de troca de identidade já estava reconhecida, portanto não cabe afirmar modo-comum sobre ela. (`v3.r:154`; `triagem_opus1.txt:4`; identidades concretas: [nao verificado])

4. **FILA — LINHAS_REGISTRADAS absolutas**

   **Pergunta:** “Linha mudada por edicao NAO relacionada do motor reprova (custo esperado) e a mensagem diz o que fazer?” (`v3.q:7`)

   **Resposta:** “**PROCEDENTE o custo de deslocamento:** uma edição sem relação com o negócio reprova se deslocar ocorrências registradas.” (`v3.r:156`)

   **Triagem:** “custo de deslocamento e o esperado e a mensagem diz o que fazer.” O opus manteve: “LINHAS_REGISTRADAS absolutas (custo de vermelho falso, nunca verde falso)”. (`triagem_v3.txt:7`; `triagem_opus1.txt:4`) [nao verificado]

   **Classe (b):** o Codex viu o custo e a triagem o aceitou expressamente como esperado, sem tratá-lo como bloqueador. (`v3.r:156`; `triagem_v3.txt:7`)

5. **FILA — M2, M4 e M5 declarados no NAO_VE**

   **Pergunta:** referência individual a M2, M4 ou M5: **ausente**. O tema correlato estava presente: “Distinga o que o NAO_VE declara do que some calado.” (`v3.q:6`)

   **Resposta:** identificação individual desses mutantes: **ausente**. Sobre os limites examinados, o Codex escreveu: “**Não enumera especificamente `for-target`, `with-as` e chaves de `dict` como buracos do gap.**” (`v3.r:152`)

   **Triagem:** “Q5 PROCEDENTE (b)+(d) TEXTO”; a mesma linha registra que a perda da parcela continua cobrada pela catraca numérica e por `LINHAS_REGISTRADAS`. A vinculação disso aos nomes M2/M4/M5 não aparece; o opus fornece somente “M2/M4/M5 declarados no NAO_VE”. (`triagem_v3.txt:6`; `triagem_opus1.txt:4`)

   **Classes individuais — M2: [nao verificado]; M4: [nao verificado]; M5: [nao verificado].** Sem os enunciados dos três mutantes, não há base para escolher entre descarte de achado já visto, classe (b), e limitação fora do alcance, classe (c), nem para associá-los aos exemplos de v3(b). (`triagem_opus1.txt:4`; `v3.r:152`)

## 07/11 18:11 · sessao 3de4b0f69961 · - · - · origem=Ronald (tmux)

Minha consolidação mais defensável:

- **O1 — (a).** O tema estava em v1.q: “Algum item conta algo fora de A1/B1/nomeados, ou deixa de contar?” e “Cite algum mutante do instrumento que passaria em todos os testes.” A resposta avaliou “Pareador novo” como “Um `range(..., 2)` novo”, sem diagnosticar nova chamada ao pareador fora das funções fixas. Não há descarte desse achado específico na triagem; ela não o recebeu. `[nao verificado]` [nao verificado]
- **O2 — (d), com distinção expressa.** A entrega contratual do leitor host não foi perguntada explicitamente nem diagnosticada como faltante. A resposta “Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**” registra uma incerteza, não o achado O2; portanto não basta para atribuir (b). “bin/ fora do alcance do construtor” também não demonstra impossibilidade do verificador. `[nao verificado]` [nao verificado]
- **E2, quatro identidades B1 no rebase — (c) para o evento concreto.** As quatro identidades e as árvores comparadas não constam do dado. v2.q(b) pergunta identidade da composição; v3.r(c) já reconhece “identidade completa” nos registros dos contadores. Não afirmar que ninguém viu a classe geral; não afirmar que o rebase ocorreu depois das verificações. `[nao verificado]`
- **LINHAS_REGISTRADAS absolutas — (b).** v3.q(c): “Linha mudada por edicao NAO relacionada do motor reprova (custo esperado) e a mensagem diz o que fazer?” v3.r(c): “PROCEDENTE o custo de deslocamento”. Triagem: “custo de deslocamento e o esperado e a mensagem diz o que fazer.” Foi visto e aceito como custo esperado, não falha bloqueadora. `[nao verificado]`
- **M2/M4/M5 — correspondência individual indeterminada.** O dado só informa “M2/M4/M5 declarados no NAO_VE”; não fornece suas definições. **(c) apenas conforme esse enquadramento do opus**, sem confirmar que sejam os escapes de v3(b). Mapear M2/M4/M5 a `for`, `with`, `setattr` etc. inventaria informação. `[nao verificado]`

Para O2, se usar (d), explique que o modo comum é **a ausência da entrega host como diagnóstico contratual**, apesar da menção à publicação não verificada. `[nao verificado]`

## 07/11 18:11 · sessao ee5afc1cc41e · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

O1 corrigido em [core/medida_turno_do_par.py:414](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:414): cada chamada resolvida vira uma porta, independentemente do nome da função; chamadas na composição ficam excluídas. A herança por gap continua separada em `core/medida_turno_do_par.py:422`.

Antes da edição, a medição encontrou somente:

| Chave | Linha | Sítio |
|---|---:|---|
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 503 | `porta_parear_turnos` |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` | 882 | `porta_turnos_de_batidas` |

Fontes: `ponto/motor_calculo_v2.py:503` e `:882`.

`DiaPago` está definido em `ponto/models.py:281`; a lavratura o importa em `ponto/services/dia_pago.py:163`. O escopo inclui também grade (`dia_pago.py:443` → `folha/export.py`), fechamento (`dia_pago.py:449` → `ponto/services/fechamento.py`) e janelas (`dia_pago.py:713` → `ponto/janelas.py`). A declaração completa está em `core/medida_turno_do_par.py:75`.

Resultados locais do runner de [core/tests/test_medida_turno_do_par.py:19](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:19):

- **Antes da cura:** M1 e M6 deram RED por `AssertionError: 10 != 9`; M7 passou. Casos em `core/tests/test_medida_turno_do_par.py:759`, `:762` e `:766`.
- **Depois:** original **rc=0, 35 testes OK**. M1/M6 exigem **10/86**; M7 preserva **9/85** (`core/tests/test_medida_turno_do_par.py:752` e `:780`).
- **13/13 mutantes: rc=1 por asserção, sem erros de execução**, usando os comandos de `core/tests/test_medida_turno_do_par.py:73`.
- **9/85, REGISTRO e LINHAS_REGISTRADAS preservados**, cobrados em `core/tests/test_medida_turno_do_par.py:552` e `:568`.

Para reproduzir sem arquivos auxiliares, em `app/`:

```bash
rodar_turno() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$@" <<'PY'
import os, pathlib, shlex, sys
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
source = p.read_text()
runner = source.split("<<'PY'\n", 1)[1].split('\n  PY', 1)[0]
runner = '\n'.join(line[2:] for line in runner.splitlines())
mutant = sys.argv[1] if len(sys.argv) > 1 else ''
os.environ.pop('MUTANTE', None)
sys.argv = [str(p)]
if mutant:
    command = next(line for line in source.splitlines()
                   if line.startswith('  MUTANTE=' + mutant + ' '))
    os.environ['MUTANTE'] = mutant
    sys.argv.append(shlex.split(command)[-1])
exec(compile(runner, '<runner-docstring>', 'exec'))
PY
}

rodar_turno                       # original: rc=0
rodar_turno composicao            # rc=1
rodar_turno linhas                # rc=1
rodar_turno gap                   # rc=1
rodar_turno classificar           # rc=1
rodar_turno P1-linha-unica        # rc=1
rodar_turno P2-len1               # rc=1
rodar_turno Q1-lines10            # rc=1
rodar_turno Q2-len2               # rc=1
rodar_turno Q3-tuple              # rc=1
rodar_turno Q4-self               # rc=1
rodar_turno O1-funcoes-fixas      # rc=1: 10 != 9
rodar_turno O1-ancoras-motor      # rc=1: 10 != 9
rodar_turno O1-sem-composicao     # rc=1: 9 != 10
```

Não há campos novos na saída; os itens reutilizam `chave`, `criterio`, `sitio` e `linhas` (`core/medida_turno_do_par.py:420`).

| Dado acrescentado/reutilizado | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `_ARQUIVOS_DIAPAGO` | `core/medida_turno_do_par.py:75` | mesmo arquivo`:416` |
| `PORTAS_NOMEADAS` | `core/medida_turno_do_par.py:82` | mesmo arquivo`:414` |
| Item da porta: quatro campos existentes | `core/medida_turno_do_par.py:420` | `core/tests/test_medida_turno_do_par.py:753`, `:562`, `:569` |
| Contadores com essas portas | `core/medida_turno_do_par.py:435` | `core/tests/test_medida_turno_do_par.py:752` |

Certificação pelo Code e opus final: **[nao verificado]**.

## 07/11 17:34 · sessao 9e854daaf1c8 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Q1–Q4 acrescentados: [11 ocorrências](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:606), [três desconhecidas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:635), [Tuple interna](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:654) e [outro receptor](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:685). Q5 corrigido no [NAO_VE](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:52) e no [docstring de `_gap`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:257).

A comparação AST antes/depois confirmou detector, catraca e registros intactos; execução e comparação ficaram somente em memória: [nao verificado].

Os resultados abaixo foram observados localmente, sem artefato histórico persistido nem certificação: [nao verificado]. Cada link aponta o comando exato; todos usam o heredoc do [runner](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21).

| Execução / comando | Resultado |
|---|---|
| [Original](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21) | rc=0; 28 testes OK | [nao verificado]
| [`MUTANTE=composicao`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:66) | rc=1; 1 falha por asserção |
| [`MUTANTE=linhas`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:67) | rc=1; 3 falhas por asserção |
| [`MUTANTE=gap`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:68) | rc=1; 9 falhas por asserção |
| [`MUTANTE=classificar`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:69) | rc=1; 1 falha por asserção |
| [`MUTANTE=P1-linha-unica`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:70) | rc=1; 1 falha por asserção |
| [`MUTANTE=P2-len1`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:71) | rc=1; 2 falhas por asserção |
| [`MUTANTE=Q1-lines10`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:72) | rc=1; falta a linha 12 |
| [`MUTANTE=Q2-len2`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:73) | rc=1; `LookupError not raised` |
| [`MUTANTE=Q3-tuple`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:74) | rc=1; asserção contra falsa herança |
| [`MUTANTE=Q4-self`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:75) | rc=1; `LookupError not raised` |

## 07/11 17:19 · sessao 6f9c34fbaa6e · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**As curas corrigem os contraexemplos da triagem, mas a proteção varia: P1 congela integralmente a árvore atual; P2/P3 têm regras gerais no código com cobertura ainda parcial; P4 mantém escapes; P5 remove o relato histórico, sem garantir documentação futura.** (`app/core/tests/test_medida_turno_do_par.py:546`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:286`, `app/core/medida_turno_do_par.py:316`, `app/core/tests/test_medida_turno_do_par.py:55`)

O histórico `133cd78a..35969e9d` não pôde ser conferido: a cópia não contém `.git`. O confronto abaixo usa a fonte atual e a triagem fornecida. **[nao verificado]**

Executei o original e cada mutante isoladamente, em memória: **original 28/28; P1–P5 28/28**. Conferi também as asserções HOST sobre cada fonte mutante. São os testes deste instrumento, pelo runner adaptado de `SimpleTestCase` para `unittest`, sem execução Django/produtiva; resultados sem artefato persistido: **[nao verificado]**. O procedimento está documentado em `app/core/tests/test_medida_turno_do_par.py:20`.

**P1 — fecha as linhas da árvore atual; não fecha a generalização para outras cardinalidades.** Agora há igualdade integral entre registro e publicação, incluindo A1, B1 e nomeados. O mutante anterior “usar `def` quando houver uma ocorrência” enfrenta essa igualdade. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:546`)

Mutante sobrevivente: substituir exatamente este fragmento em `app/core/medida_turno_do_par.py:389`:

```python
'criterio': criterion, 'linhas': lines
```

por:

```python
'criterio': criterion, 'linhas': lines[:10]
```

O maior registro atual tem dez ocorrências; a fixture específica cobra duas. Assim, o mutante preserva a árvore registrada e trunca uma função futura com onze ocorrências. (`app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:587`)

Teste que deveria pegá-lo, ampliando `test_MORDE_linhas_a1_b1_publicam_ocorrencia_e_nao_def`, em `app/core/tests/test_medida_turno_do_par.py:570`:

```python
sources = _mini()
sources['novo.py'] = 'def novo(entrada):\n' + '    entrada.date()\n' * 11
result = m.censo_fontes(sources)
item = next(i for i in result['derivacoes_de_dia']['itens']
            if i['chave'] == 'novo.py::novo')
self.assertEqual(list(range(2, 13)), item['linhas'])
```

**P2 — o código fecha a classe “qualquer quantidade de desconhecidas”; a suíte acrescentou somente o caso de duas.** A diferença de conjuntos continua correta, mas ainda pode regredir para uma guarda limitada sem os testes perceberem. (`app/core/medida_turno_do_par.py:316`, `app/core/tests/test_medida_turno_do_par.py:616`)

Mutante sobrevivente: substituir a guarda em `app/core/medida_turno_do_par.py:316` por:

```python
if 0 < len(inherited - recognized) <= 2:
```

Teste que deveria pegá-lo, estendendo `app/core/tests/test_medida_turno_do_par.py:616`:

```python
sources = _mini()
sources[m._MOTOR] += '        data_jornada = _prev_dj\n' * 3
with self.assertRaisesRegex(LookupError, r'linhas \[12, 13, 14\]'):
    m.censo_fontes(sources)
```

O original aponta as três linhas; o mutante deixa passar e mantém somente a ocorrência reconhecida da fixture. Resultado observado em memória: **[nao verificado]**.

**P3 — fecha o casamento recursivo de Tuple/List sem `starred` na implementação; a suíte não protege todas essas formas.** O falso cruzamento original foi corrigido por associação posicional recursiva. A recursão ensaiada usa lista interna; falta a variante com tupla interna. (`app/core/medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:626`)

Mutante sobrevivente: substituir a linha de `app/core/medida_turno_do_par.py:265` por:

```python
yield from ([(left, right)] if isinstance(left, ast.Tuple) else bindings(left, right))
```

Teste que deveria pegá-lo, ampliando `app/core/tests/test_medida_turno_do_par.py:626`:

```python
sources = _mini()
sources[m._MOTOR] += (
    '        outro, (data_jornada, ultimo) = (_prev_dj, (x, _prev_dj))\n'
)
result = m.censo_fontes(sources)
self.assertEqual([11], result['sitios_nomeados'][-1]['linhas'])
```

Aqui `data_jornada` recebe `x`. O original aceita; o mutante mistura as posições da tupla interna e lança `LookupError`. Resultado observado em memória: **[nao verificado]**.

**P4 — amplia a detecção, mas não fecha a classe de fontes, destinos e bindings.** Alias transitivo, atributo de origem e subscrito literal de destino ganharam implementação e testes; outros bindings continuam ausentes. (`app/core/medida_turno_do_par.py:269`, `app/core/medida_turno_do_par.py:279`, `app/core/medida_turno_do_par.py:286`, `app/core/tests/test_medida_turno_do_par.py:654`)

Mutante sobrevivente: substituir o ramo de `Attribute` em `app/core/medida_turno_do_par.py:281` por:

```python
or (isinstance(n, ast.Attribute) and n.attr == '_prev_dj' and isinstance(n.value, ast.Name) and n.value.id == 'self')
```

A cobertura de origem por atributo usa `self._prev_dj`. Falta outro receptor. (`app/core/tests/test_medida_turno_do_par.py:657`, `app/core/tests/test_medida_turno_do_par.py:659`, `app/core/tests/test_medida_turno_do_par.py:662`)

Teste que deveria pegá-lo: [nao verificado]

```python
sources = _mini()
sources[m._MOTOR] += '        data_jornada = estado._prev_dj\n'
with self.assertRaisesRegex(LookupError, r'linhas \[12\]'):
    m.censo_fontes(sources)
```

**P5 — fecha o texto histórico apontado, não uma classe executável.** O docstring atual prescreve execuções que “devem” falhar; não conserva o antigo relato de autoria, duração e execuções concluídas. (`app/core/tests/test_medida_turno_do_par.py:55`, `app/core/tests/test_medida_turno_do_par.py:64`)

Para cumprir literalmente o pedido de mutante **do instrumento**, acrescente após a frase de `app/core/medida_turno_do_par.py:19` esta linha, ainda dentro do docstring:

```text
Registro da execucao: suite Django oficial aprovada em producao. [nao verificado]
```

Isso sobrevive porque as verificações HOST conferem execução/imports, sem autenticar essa alegação documental. A aprovação produtiva escrita pelo mutante é **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:826`)

Um teste documental como o seguinte pegaria **esse texto**, sem provar a veracidade geral da documentação: [nao verificado]

```python
self.assertNotIn('Registro da execucao:', m.__doc__)
```

A limitação é precisamente essa: P5 exige revisão de alegações e proveniência; um teste literal fecha apenas a frase escolhida. Autenticação geral dessas alegações pela suíte: **[nao verificado]**. [nao verificado]

**a) PROCEDENTE como possibilidade de contaminação; NAO PROCEDE como falha demonstrada no motor da cópia.** `aliases` só cresce, inclui `data_jornada` após a herança reconhecida e não elimina marcas após sobrescritas. Ordem, ramos e alcance são limitações expressamente declaradas. (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:277`, `app/core/medida_turno_do_par.py:290`)

Estes exemplos, acrescentados após a herança reconhecida, podem acusar herança na última atribuição sem que ela carregue `_prev_dj`; isso decorre de `source` consultar o conjunto acumulado. (`app/core/medida_turno_do_par.py:279`, `app/core/medida_turno_do_par.py:300`)

```python
data_jornada = x
data_jornada = data_jornada
```

```python
anterior = _prev_dj
anterior = x
data_jornada = anterior
```

```python
if cond:
    anterior = _prev_dj
else:
    anterior = x
    data_jornada = anterior
```

**Sim: `[2408]` é a única atribuição reconhecida no motor da cópia.** A origem inicial é `data_entrada`; a atribuição direta sob `_gap_h` está em `:2408`; depois ocorre `_prev_dj = data_jornada`. `recognized` exige literalmente `data_jornada = _prev_dj`, sem consultar os aliases. (`app/ponto/motor_calculo_v2.py:2403`, `app/ponto/motor_calculo_v2.py:2408`, `app/ponto/motor_calculo_v2.py:2409`, `app/core/medida_turno_do_par.py:312`)

A contaminação pode causar `LookupError` no **censo**; `_gap` recebe AST e é chamado pela instrumentação. Não há demonstração de parcela errada ou exceção no cálculo produtivo. (`app/core/medida_turno_do_par.py:329`, `app/core/medida_turno_do_par.py:399`; produção: **[nao verificado]**)

**b) PROCEDENTE — permanecem escapes, e nem todos estão declarados especificamente.**

| Forma | Veredito e comportamento | Evidência |
|---|---|---|
| `data_jornada, *outro = (x, _prev_dj)` | **NAO PROCEDE como escape:** acusa conservadoramente, embora o valor herdado vá para `outro`. | `app/core/medida_turno_do_par.py:259`; `app/core/tests/test_medida_turno_do_par.py:645` |
| `for data_jornada in [_prev_dj]: pass` | **PROCEDENTE:** `For` não entra em `assignments`. | `app/core/medida_turno_do_par.py:286` |
| `with cm(_prev_dj) as data_jornada: pass` | **PROCEDENTE:** `With` não entra em `assignments`. | `app/core/medida_turno_do_par.py:286` |
| `data_jornada = copiar(_prev_dj)` | **NAO PROCEDE como escape:** encontra a fonte dentro da chamada; já ensaiado. | `app/core/medida_turno_do_par.py:279`; `app/core/tests/test_medida_turno_do_par.py:599` |
| `setattr(self, 'data_jornada', _prev_dj)` | **PROCEDENTE:** chamada isolada não é atribuição selecionada. | `app/core/medida_turno_do_par.py:286` |
| `global`/`nonlocal` com atribuição direta no corpo examinado | **NAO PROCEDE como escape automático:** a declaração não esconde o `Assign`; alcance é ignorado. | `app/core/medida_turno_do_par.py:34`; `app/core/medida_turno_do_par.py:286` |
| Escritor em função aninhada usando `global`/`nonlocal` | **PROCEDENTE:** `_own` não percorre seu corpo; a chamada não propaga efeitos para `_gap`. | `app/core/medida_turno_do_par.py:70`; `app/core/medida_turno_do_par.py:284` |
| `dados = {'data_jornada': _prev_dj}` | **PROCEDENTE como destino omitido:** chave de `Dict` não é destino reconhecido. Pode marcar `dados` para usos posteriores. | `app/core/medida_turno_do_par.py:269`; `app/core/medida_turno_do_par.py:295` |
| `dados[chave] = _prev_dj` | **PROCEDENTE:** somente chave literal `'data_jornada'` é reconhecida. | `app/core/medida_turno_do_par.py:274` |

`NAO_VE` declara fluxo/alcance, origem interprocedural, chamadas dinâmicas e conservadorismo no desempacotamento. **Não enumera especificamente `for-target`, `with-as` e chaves de `dict` como buracos do gap.** (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:35`, `app/core/medida_turno_do_par.py:36`, `app/core/medida_turno_do_par.py:52`)

**c) NAO PROCEDE cobertura parcial de `LINHAS_REGISTRADAS`.** A igualdade percorre todos os itens dos dois contadores: A1, B1 e os três nomeados, com identidade completa e listas inteiras. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

**PROCEDENTE o custo de deslocamento:** uma edição sem relação com o negócio reprova se deslocar ocorrências registradas. A mensagem é **“linhas mudaram: registrar no MESMO commit”**: informa a ação, embora não nomeie `LINHAS_REGISTRADAS`. Não é toda edição alheia que reprova; precisa alterar o retrato comparado. (`app/core/tests/test_medida_turno_do_par.py:17`, `app/core/tests/test_medida_turno_do_par.py:550`)

**d) NAO PROCEDE a permanência do antigo relato histórico; PROCEDENTE uma promessa comportamental excessiva.** “Alias dentro do if exige LookupError” não cobre, por exemplo, alias introduzido por `for`; a seleção não inclui esse binding. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:286`)

A reescrita abaixo pode retirar a parcela gap sem lançar a exceção prometida; a catraca numérica ainda cobra a queda quando aplicada. (`app/core/medida_turno_do_par.py:286`, `app/core/medida_turno_do_par.py:401`, `app/core/tests/test_medida_turno_do_par.py:497`)

```python
if 0 <= _gap_h <= 6:
    for anterior in [_prev_dj]:
        data_jornada = anterior
```

**FILA:** as referências históricas continuam qualificadas como informadas/anexadas. Sua proveniência e a contemporaneidade exigida por “MESMO commit” não são comprovadas pela suíte; o próprio texto explicita “sem consultar git”. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:5`, `app/core/tests/test_medida_turno_do_par.py:18`)

**e) Classes que já reprovaram a obra.**

| Classe | Veredito nesta revisão | Evidência |
|---|---|---|
| Segundo juiz de negócio | **NAO PROCEDE no instrumento:** identifica estruturas AST e publica medidas; não decide a jornada. | `app/core/medida_turno_do_par.py:329`, `app/core/medida_turno_do_par.py:416` |
| Segundo escritor fora da porta | **NAO PROCEDE no instrumento:** lê fontes e retorna estruturas. | `app/core/medida_turno_do_par.py:427` |
| Relógio fora da borda | **NAO PROCEDE no instrumento:** imports limitados a `ast`/`os`, conferidos pelo teste HOST. | `app/core/medida_turno_do_par.py:21`, `app/core/tests/test_medida_turno_do_par.py:834` |
| Derivação paralela de fonte única | **FILA:** portas e herança permanecem no motor; são objeto da medição. | `app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2408` |
| Selo/RED sem mordida | **PROCEDENTE quanto à cobertura incompleta**, pelos mutantes; **NAO PROCEDE como ausência total de mordida:** incrementos novos são cobrados. | `app/core/tests/test_medida_turno_do_par.py:690`, `app/core/tests/test_medida_turno_do_par.py:700`; sobrevivência: **[nao verificado]** |
| Campo com dois significados | **NAO PROCEDE como divergência de unidade demonstrada:** linhas representam ocorrências, contadores representam funções mais sítios. **FILA** para o rótulo “sem teto ou catraca” de `CLASSIFICAR`, cuja identidade pode reprovar por falta de registro. | `app/core/medida_turno_do_par.py:10`, `app/core/tests/test_medida_turno_do_par.py:12`, `app/core/tests/test_medida_turno_do_par.py:552` |

**f) QUEM ESCREVE / QUEM LÊ.** Inventário dos mecanismos relacionados às curas; exaustividade perante o delta histórico: **[nao verificado]**.

| Valor | Quem escreve | Quem lê |
|---|---|---|
| Pares posicionais de `bindings` | `app/core/medida_turno_do_par.py:264`, `:267` | Construção de atribuições em `app/core/medida_turno_do_par.py:289` |
| Resultado de `destination` | `app/core/medida_turno_do_par.py:270` | Seleção de herança em `app/core/medida_turno_do_par.py:300` |
| `aliases` | Semente em `app/core/medida_turno_do_par.py:277`; expansão em `:295` | `source` em `app/core/medida_turno_do_par.py:280`; convergência em `:296` |
| `assignments` | `app/core/medida_turno_do_par.py:289` | Propagação em `app/core/medida_turno_do_par.py:292`; seleção em `:299` |
| `inherited` | `app/core/medida_turno_do_par.py:301` | Guarda/diagnóstico em `app/core/medida_turno_do_par.py:316`, `:319` |
| `recognized` | `app/core/medida_turno_do_par.py:315` | Guarda e retorno em `app/core/medida_turno_do_par.py:316`, `:320` |
| Linhas lexicais A1/B1 | `app/core/medida_turno_do_par.py:194`, `:217`, `:389` | Contadores em `app/core/medida_turno_do_par.py:406`; igualdade em `app/core/tests/test_medida_turno_do_par.py:547` |
| Linhas nomeadas | `app/core/medida_turno_do_par.py:399`, `:403` | Contadores em `app/core/medida_turno_do_par.py:406`; igualdade em `app/core/tests/test_medida_turno_do_par.py:549` |
| `LINHAS_REGISTRADAS` | `app/core/tests/test_medida_turno_do_par.py:227` | `app/core/tests/test_medida_turno_do_par.py:547` |
| `NAO_VE` | `app/core/medida_turno_do_par.py:31` | Publicação em `app/core/medida_turno_do_par.py:408`, `:423`; igualdade em `app/core/tests/test_medida_turno_do_par.py:543` |
| Fontes/expectativas dos novos ensaios | `app/core/tests/test_medida_turno_do_par.py:618`, `:627`, `:643`, `:655`, `:677` | Chamadas e asserções em `app/core/tests/test_medida_turno_do_par.py:621`, `:637`, `:651`, `:670`, `:679` |
| Instruções de execução documental | `app/core/tests/test_medida_turno_do_par.py:55` | Autenticador automático de execução histórica: **[nao verificado]** |

## 07/11 17:14 · sessao a57c29d65425 · - · - · origem=Ronald (tmux)

**P1: fecha as linhas da árvore atual, não toda cardinalidade futura.** A igualdade compara todos os itens de ambos contadores, incluindo A1, B1 e os três nomeados; não verifica somente presença. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

Mutante exato em `app/core/medida_turno_do_par.py:389`:

```python
# antes
'criterio': criterion, 'linhas': lines

# depois
'criterio': criterion, 'linhas': lines[:10]
```

Executado em memória: **28/28 passam**; contraprova com onze chamadas publica dez linhas. Resultado sem artefato persistido: **[nao verificado documentalmente]**. O maior registro atual tem dez ocorrências e a fixture específica cobra duas. (`app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:587`)

Teste que deveria matá-lo, ampliando `test_MORDE_linhas_a1_b1_publicam_ocorrencia_e_nao_def` em `app/core/tests/test_medida_turno_do_par.py:570`:

```python
sources = _mini()
sources['novo.py'] = 'def novo(entrada):\n' + '    entrada.date()\n' * 11
result = m.censo_fontes(sources)
item = next(i for i in result['derivacoes_de_dia']['itens']
            if i['chave'] == 'novo.py::novo')
self.assertEqual(list(range(2, 13)), item['linhas'])
```

**c) NAO PROCEDE cobertura parcial.** A1 consta em `:227-232`, B1 em `:236-354`, os nomeados nos dois contadores em `:233-235` e `:355-357`; a compreensão não filtra critérios e compara o registro inteiro. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

**c) PROCEDENTE custo de deslocamento.** Uma edição alheia ao negócio que acrescente linhas antes dos sítios registrados muda a igualdade e reprova; o próprio contrato anuncia essa cobrança. A mensagem diz **“linhas mudaram: registrar no MESMO commit”**: informa a ação, embora não nomeie `LINHAS_REGISTRADAS`. (`app/core/tests/test_medida_turno_do_par.py:17`, `app/core/tests/test_medida_turno_do_par.py:550`)

**P5: fecha a ocorrência documental apontada.** O antigo relato de autoria/data/duração/resultados externos foi removido; agora o texto prescreve seis execuções e afirma que **devem** falhar. Não declara que foram executadas. (`app/core/tests/test_medida_turno_do_par.py:55`, `app/core/tests/test_medida_turno_do_par.py:64`)

**d) NAO PROCEDE permanência daquele “Registro da execução”. FILA quanto à história externa.** Permanecem base informada e referência histórica anexada, cuja proveniência a suíte não certifica; são referências qualificadas, não resultados apresentados como medidos pela sessão atual. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:5`; proveniência: **[nao verificado]**)

Não existe mutante lógico do instrumento que seja contraexemplo pertinente à retirada de um relato do docstring dos testes; isso seria outra propriedade. Reintroduzir história no docstring sobreviveria porque não há leitor que autentique documentação, mas não demonstraria falha da correção executável de P1–P4. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:826`; autenticação documental: **[nao verificado]**)

**f), parcela sob minha revisão:**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `LINHAS_REGISTRADAS` | Constante registrada no teste | Igualdade de todos os itens | [nao verificado]
| | `app/core/tests/test_medida_turno_do_par.py:227` | `app/core/tests/test_medida_turno_do_par.py:547` |
| Linhas lexicais A1/B1 | `_scan`, depois publicação de `criteria` | Contadores e igualdade real |
| | `app/core/medida_turno_do_par.py:194`, `app/core/medida_turno_do_par.py:217`, `app/core/medida_turno_do_par.py:389` | `app/core/medida_turno_do_par.py:406`, `app/core/tests/test_medida_turno_do_par.py:549` |
| Linhas nomeadas | `_gap` ou chamadas resolvidas, depois `named` | Contadores e igualdade real |
| | `app/core/medida_turno_do_par.py:399`, `app/core/medida_turno_do_par.py:403` | `app/core/medida_turno_do_par.py:406`, `app/core/tests/test_medida_turno_do_par.py:549` |
| Fixture de linhas deslocadas | Fonte `novo.py`, prefixo de 0/40 linhas | Comparação exata dos sítios |
| | `app/core/tests/test_medida_turno_do_par.py:572`, `app/core/tests/test_medida_turno_do_par.py:584` | `app/core/tests/test_medida_turno_do_par.py:592` |
| Instruções RED externo | Texto do docstring | Execução humana; autenticador automático não identificado |
| | `app/core/tests/test_medida_turno_do_par.py:57` | **[nao verificado]** |

## 07/11 17:14 · sessao f6a7f5a9c8d2 · - · - · origem=Ronald (tmux)

**a) PROCEDENTE como limitação declarada; NAO PROCEDE como falha demonstrada no motor corrente.** O ponto fixo inclui `data_jornada` após a atribuição reconhecida e nunca remove aliases; ignora ordem, sobrescritas e ramos. Assim, exemplos abaixo levantam falso positivo, embora a última atribuição não herde `_prev_dj`. (`app/core/medida_turno_do_par.py:277`, `app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:300`; declaração em `app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:55`)

```python
# Acrescentados após a forma reconhecida:
data_jornada = x
data_jornada = data_jornada
```

```python
anterior = _prev_dj
anterior = x
data_jornada = anterior
```

```python
if cond:
    anterior = _prev_dj
else:
    anterior = x
    data_jornada = anterior
```

Executei `_gap` sobre a AST viva de `MotorBase.calcular_mes` e obtive `[2408]`; resultado sem artefato persistido: **[nao verificado documentalmente]**. A fonte possui exatamente uma atribuição direta `data_jornada = _prev_dj`, sob comparação de `_gap_h`; `recognized` exige essa forma literal e não consulta aliases. A contaminação não cria outra parcela reconhecida no motor disponível. (`app/ponto/motor_calculo_v2.py:2404`, `app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`, `app/ponto/motor_calculo_v2.py:2409`; `app/core/medida_turno_do_par.py:312`)

**b) PROCEDENTE quanto a escapes restantes.**

| Forma | Resultado estrutural | Evidência |
|---|---|---|
| `data_jornada, *outro = (x, _prev_dj)` | Capturada conservadoramente, mesmo sem herança para `data_jornada` | `app/core/medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:645` |
| `for data_jornada in [_prev_dj]: pass` | Some: `For` não entra em `assignments` | `app/core/medida_turno_do_par.py:286` |
| `with cm(_prev_dj) as data_jornada: pass` | Some: `With` não entra em `assignments` | `app/core/medida_turno_do_par.py:286` |
| `data_jornada = copiar(_prev_dj)` | Capturada; já cobrada | `app/core/medida_turno_do_par.py:279`, `app/core/tests/test_medida_turno_do_par.py:599` |
| `setattr(self, 'data_jornada', _prev_dj)` | Some: chamada não constitui atribuição selecionada | `app/core/medida_turno_do_par.py:286` |
| `global`/`nonlocal` + atribuição direta | Declaração não altera seleção da atribuição; alcance não é resolvido | `app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:286` |
| `dados = {'data_jornada': _prev_dj}` | Some como destino nomeado; chave de `Dict` não é destino reconhecido | `app/core/medida_turno_do_par.py:269` |
| `dados[chave] = _prev_dj` | Some se `chave` é variável, ainda que valha `'data_jornada'` | `app/core/medida_turno_do_par.py:274` |

`NAO_VE` declara fluxo/alcance, origem interprocedural, chamadas dinâmicas e conservadorismo sem casamento posicional. Não enumera expressamente os buracos `for`/`with`/chave de dicionário do gap. A promessa “outras reescritas” do docstring excede o detector mostrado. (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:35`, `app/core/medida_turno_do_par.py:36`, `app/core/medida_turno_do_par.py:55`, `app/core/medida_turno_do_par.py:255`)

Mutantes candidatos enviados para validação integral pelo pai:

```python
# P2: substitui a guarda em :316
if 0 < len(inherited - recognized) <= 2:
```

Deveria reprovar com **três** heranças desconhecidas; o teste atual usa duas. (`app/core/tests/test_medida_turno_do_par.py:616`)

```python
# P3: acrescenta ao if de bindings em :260
and len(target.elts) <= 2
```

Deveria reprovar com casamento correto de três posições, por exemplo `data_jornada, outro, ultimo = (x, _prev_dj, y)`. A fixture atual cobre apenas pares. (`app/core/tests/test_medida_turno_do_par.py:626`)

```python
# P4: substitui o ramo Attribute de source em :281
or (isinstance(n, ast.Attribute) and n.attr == '_prev_dj'
    and isinstance(n.value, ast.Name) and n.value.id == 'self')
```

Deveria reprovar exigindo `LookupError` para `data_jornada = estado._prev_dj`; a cobertura existente usa `self._prev_dj`. (`app/core/tests/test_medida_turno_do_par.py:657`, `app/core/tests/test_medida_turno_do_par.py:659`, `app/core/tests/test_medida_turno_do_par.py:662`)

## 07/11 17:10 · sessao 7114a41c4856 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Curas implementadas:

- **P1:** linhas reais A1/B1 e nomeadas congeladas por igualdade, com mensagem “registrar no MESMO commit” (`core/tests/test_medida_turno_do_par.py:227`, `:546`).
- **P2:** duas heranças desconhecidas exigem `LookupError` listando `[12, 13]` (`core/tests/test_medida_turno_do_par.py:616`).
- **P3/P4:** casamento posicional recursivo, fallback conservador, fontes Name/Attribute, aliases transitivos e destino Subscript (`core/medida_turno_do_par.py:259`). Alias **dentro** do `if _gap_h` exige `LookupError`, conforme declaração (`:255`) e teste (`core/tests/test_medida_turno_do_par.py:675`).
- **P5:** retirado o bloco histórico; mantidos runner e seis comandos de mutação (`core/tests/test_medida_turno_do_par.py:20`).

Os registros mantêm **9/85** e a parcela atual do gap em **[2408]** (`core/tests/test_medida_turno_do_par.py:77`, `:235`).

Resultados observados em memória nesta sessão; comprovação persistida das execuções: **[nao verificado]**. Cada link aponta o comando, usando o mesmo heredoc do runner:

| Execução | Resultado | Comando |
|---|---|---|
| Original | **28/28 OK, rc=0** | [runner](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21) |
| composição | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:58) |
| linhas | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:59) |
| gap | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:60) |
| classificar | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:61) |
| P1-linha-unica | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:62) |
| P2-len1 | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:63) |

Quem escreve e quem lê os novos registros e intermediários:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `LINHAS_REGISTRADAS` | Registro literal, `core/tests/test_medida_turno_do_par.py:227` | Igualdade real, mesmo arquivo `:547` |
| `aliases` | Inicialização e propagação, `core/medida_turno_do_par.py:277`, `:295` | Detector de fonte e ponto fixo, mesmo arquivo `:280`, `:296` |
| `assignments` / pares posicionais | Coleta e `bindings`, `core/medida_turno_do_par.py:259`, `:289` | Propagação e seleção de heranças, mesmo arquivo `:292`, `:299` |
| `inherited` ampliado | Seleção, `core/medida_turno_do_par.py:301` | Guarda e diagnóstico, mesmo arquivo `:316`, `:319` |

Certificação pelo Code/opus final: **[nao verificado]**.

## 07/11 17:03 · sessao 0601c3de7f56 · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**As curas 2 e 5b fecham os defeitos específicos apontados; 3 e 4 ainda deixam classes abertas.** A composição ganhou comparação integral, `CLASSIFICAR` ganhou leitor, mas encontrei mutantes sobreviventes para linhas e guarda de herança. (`app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:415`, `app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:456`)

A comparação histórica `c2058690..133cd78a` permanece **[nao verificado]**: a cópia não disponibiliza `.git`. As conclusões abaixo confrontam a fonte disponível com a triagem fornecida. **[nao verificado quanto ao delta histórico]**

Executei em memória, sem Django e com bytecode desabilitado: original **22/22 OK**; cada mutante abaixo, separadamente, **22/22 OK**. O procedimento corresponde ao runner documentado; os resultados desta sessão não têm artefato persistido com arquivo:linha: **[nao verificado documentalmente]**. (`app/core/tests/test_medida_turno_do_par.py:18`, `app/core/tests/test_medida_turno_do_par.py:24`)

| Item procedente da v1 | Veredito da cura |
|---|---|
| **2 — composição expansível** | **Fecha a classe específica “membros mudam sem registro” na suite:** compara a lista inteira da medida real. Não prova legitimidade semântica dos corpos já excluídos. (`app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:372`; `app/core/medida_turno_do_par.py:343`) |
| **3 — linhas falsas** | **Fecha o mutante anterior, não a classe:** precisão cobrada numa fixture com duas ocorrências por função; na medida real, apenas presença de linhas. (`app/core/tests/test_medida_turno_do_par.py:437`, `app/core/tests/test_medida_turno_do_par.py:441`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:412`) |
| **4 — herança desaparece por forma** | **Amplia a proteção, sem fechar a classe:** procura determinados alvos e o nome literal `_prev_dj`, sem propagar aliases nem casar posições de tuplas. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:261`) |
| **5b — `CLASSIFICAR` sem leitor** | **Fecha o defeito “leitor zero”:** igualdade integral entre registro e publicação calculada. (`app/core/tests/test_medida_turno_do_par.py:226`, `app/core/tests/test_medida_turno_do_par.py:415`) |

**Mutante sobrevivente do item 3**, na publicação lexical em `app/core/medida_turno_do_par.py:351`:

```python
# original
'criterio': criterion, 'linhas': lines

# mutante
'criterio': criterion,
'linhas': lines if len(lines) > 1 else [node.lineno]
```

A fixture mantém suas duas ocorrências; funções com ocorrência única passam a publicar o `def`. Isso erra **A1 real**, apontando `_alinhar_por_papel:502` em vez do `range:520`, e **B1 real**, apontando `_dia_da_batida:47` em vez de `localdate:50`. (`app/core/tests/test_medida_turno_do_par.py:449`, `app/core/tests/test_medida_turno_do_par.py:450`; `app/escala/utils.py:502`, `app/escala/utils.py:520`; `app/chamados/juizes.py:47`, `app/chamados/juizes.py:50`)

**Mutante sobrevivente do item 4**, na guarda em `app/core/medida_turno_do_par.py:278`:

```python
# original
if inherited - recognized:

# mutante
if len(inherited - recognized) == 1:
```

Os nove subcasos atuais têm exatamente **uma atribuição desconhecida** por variante, inclusive os casos que também conservam uma reconhecida. Falta cobrar duas desconhecidas. (`app/core/tests/test_medida_turno_do_par.py:458`, `app/core/tests/test_medida_turno_do_par.py:459`, `app/core/tests/test_medida_turno_do_par.py:468`, `app/core/tests/test_medida_turno_do_par.py:469`)

Com esse mutante, renomear `_gap_h` e duplicar `data_jornada = _prev_dj` na fixture devolveu normalmente **2/2**, retirando a parcela gap; resultado em memória sem artefato persistido: **[nao verificado documentalmente]**. A parcela depende das linhas devolvidas por `_gap`. (`app/core/medida_turno_do_par.py:361`, `app/core/medida_turno_do_par.py:363`)

**a) Guarda de `_gap`: classificação desdobrada.**

- **NAO PROCEDE — falso positivo demonstrado na fonte atual:** a herança presente está sob a comparação reconhecida e corresponde à atribuição simples esperada. Execução produtiva: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:274`)
- **NAO PROCEDE — escape das formas diretas ensaiadas:** tupla com `_prev_dj`, atributo **de destino**, walrus e `augassign` são submetidos à exigência de `LookupError`. (`app/core/tests/test_medida_turno_do_par.py:462`, `app/core/tests/test_medida_turno_do_par.py:463`, `app/core/tests/test_medida_turno_do_par.py:464`, `app/core/tests/test_medida_turno_do_par.py:465`, `app/core/tests/test_medida_turno_do_par.py:475`)
- **PROCEDENTE — falso estrutural possível:** `data_jornada, outro = (data_entrada, _prev_dj)` acusa herança, embora `_prev_dj` alimente `outro`. A guarda cruza quaisquer alvos compatíveis com qualquer ocorrência desse nome no RHS. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:261`)
- **PROCEDENTE — escapes restantes:** `anterior = _prev_dj; data_jornada = anterior`, `data_jornada = self._prev_dj` e `dados['data_jornada'] = _prev_dj` escapam à seleção. Respectivamente: falta propagação, origem é `Attribute`, destino é `Subscript`. (`app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`)

O desaparecimento isolado da parcela ainda enfrenta a catraca numérica; falha do detector não equivale automaticamente à aprovação da suite. (`app/core/tests/test_medida_turno_do_par.py:359`, `app/core/tests/test_medida_turno_do_par.py:364`)

**b) NAO PROCEDE — composição congelada apenas por tamanho.** A igualdade cobra todos os membros e sua ordem; troca de identidade mantendo tamanho é rejeitada. **FILA:** conteúdo, uso compartilhado e legitimidade de cada função já composta continuam sem prova semântica; a exclusão permanece integral por função. (`app/core/tests/test_medida_turno_do_par.py:368`; `app/core/medida_turno_do_par.py:343`)

**c) PROCEDENTE — cobertura exata limitada à fixture.** O teste cobre A1/B1 sintéticos, inclusive deslocamento +40. Na medida real, `REGISTRO` compara chave/critério/sítio, sem linhas, e depois exige somente listas não vazias. (`app/core/tests/test_medida_turno_do_par.py:444`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:412`)

**d) PROCEDENTE — existem fatos documentais que a suite não prova.** Autoria “do construtor”, data, duração `30.656s` e resultados das quatro execuções externas são afirmações históricas, não conclusões verificadas pelas asserções. Sua falsidade também está **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:58`, `app/core/tests/test_medida_turno_do_par.py:59`, `app/core/tests/test_medida_turno_do_par.py:60`)

“MESMO commit” é exigência textual: os testes comparam a árvore corrente, sem demonstrar contemporaneidade no histórico. **NAO PROCEDE** atribuir ao docstring uma certificação Django oficial: ela está explicitamente marcada `[nao verificado]`. (`app/core/tests/test_medida_turno_do_par.py:364`, `app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:415`, `app/core/tests/test_medida_turno_do_par.py:64`)

**e) Classes que já reprovaram a obra.**

- **NAO PROCEDE nos dois arquivos examinados:** segundo juiz de negócio, segundo escritor persistente ou relógio fora da borda. O instrumento lê fontes, analisa AST e retorna estruturas; seus imports são `ast`/`os`. (`app/core/medida_turno_do_par.py:21`, `app/core/medida_turno_do_par.py:378`, `app/core/medida_turno_do_par.py:389`)
- **FILA — derivação paralela já existente:** as portas e a herança continuam no motor, medidas positivamente; não foram curadas por esta instrumentação. (`app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:59`)
- **PROCEDENTE quanto à garantia incompleta do RED:** os dois mutantes acima sobrevivem à cobertura atual. **NAO PROCEDE** chamar toda a mordida de decorativa: novos pareador e derivador lexical têm incremento e rejeição exigidos. (`app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:456`, `app/core/tests/test_medida_turno_do_par.py:490`, `app/core/tests/test_medida_turno_do_par.py:498`)
- **FILA de nomenclatura:** `CLASSIFICAR` é chamado “sem teto ou catraca”, mas sua identidade pode reprovar a suite; o próprio texto esclarece que a cobrança é de registro. Divergência de unidade entre escritor e leitor: **NAO PROCEDE** na comparação mostrada. (`app/core/tests/test_medida_turno_do_par.py:12`, `app/core/tests/test_medida_turno_do_par.py:14`, `app/core/tests/test_medida_turno_do_par.py:415`)

**f) QUEM ESCREVE / QUEM LÊ os valores envolvidos nas curas.**

| Valor | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `inherited` | Seleção de atribuições, `app/core/medida_turno_do_par.py:263` | Diferença e diagnóstico, `app/core/medida_turno_do_par.py:278` |
| `recognized` | Reconhecimento sob `_gap_h`, `app/core/medida_turno_do_par.py:277` | Guarda e linhas retornadas, `app/core/medida_turno_do_par.py:278`, `app/core/medida_turno_do_par.py:282` |
| Linhas/erro do gap | Retorno ou exceção, `app/core/medida_turno_do_par.py:279` | Construção do nomeado, `app/core/medida_turno_do_par.py:361`; teste de exceção, `app/core/tests/test_medida_turno_do_par.py:475` |
| `composed` | Fechamento, `app/core/medida_turno_do_par.py:335` | Exclusão, `app/core/medida_turno_do_par.py:343`; guarda de nomeado absorvido, `app/core/medida_turno_do_par.py:358` |
| `composicao` / `COMPOSICAO_REGISTRADA` | Publicação, `app/core/medida_turno_do_par.py:384`; registro, `app/core/tests/test_medida_turno_do_par.py:79` | Igualdade integral, `app/core/tests/test_medida_turno_do_par.py:368` |
| `a_classificar_pela_regra` / `CLASSIFICAR` | Cálculo, `app/core/medida_turno_do_par.py:377`; registro, `app/core/tests/test_medida_turno_do_par.py:226` | Igualdade integral, `app/core/tests/test_medida_turno_do_par.py:415` |
| Linhas A1/B1 | Ocorrências AST, `app/core/medida_turno_do_par.py:191`, `app/core/medida_turno_do_par.py:214`; publicação, `app/core/medida_turno_do_par.py:351` | Presença na medida real, `app/core/tests/test_medida_turno_do_par.py:412`; exatidão na fixture, `app/core/tests/test_medida_turno_do_par.py:454` |
| Fonte com chamada morta / variantes | Injeção em memória, `app/core/tests/test_medida_turno_do_par.py:389`, `app/core/tests/test_medida_turno_do_par.py:458` | `censo`/`censo_fontes`, `app/core/tests/test_medida_turno_do_par.py:395`, `app/core/tests/test_medida_turno_do_par.py:476` |
| “Registro da execução” documental | Texto estático, `app/core/tests/test_medida_turno_do_par.py:58` | Leitor automático que comprove sua proveniência: **[nao verificado]** |

## 07/11 17:02 · sessao 93157a80dbed · - · - · origem=Ronald (tmux)

**Item 3 — PROCEDENTE; cura fecha o mutante anterior, não a classe.** A comparação exata de A1/B1 cobre somente `novo.py` sintético, com **duas ocorrências por função**, nos deslocamentos 0/+40. Na medida real, o registro compara identidade sem linhas e só exige linhas não vazias. (`app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:448`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:412`)

Mutante sobrevivente no instrumento, substituindo a publicação em `app/core/medida_turno_do_par.py:351`:

```python
'criterio': criterion,
'linhas': lines if len(lines) > 1 else [node.lineno]
```

A fixture preserva suas duas ocorrências; uma função com uma ocorrência publica o `def`. Exemplos reais: `_alinhar_por_papel` publica 502 em vez do `range` em 520; `_dia_da_batida` publica 47 em vez de `localdate` em 50. (`app/core/tests/test_medida_turno_do_par.py:437`, `app/core/tests/test_medida_turno_do_par.py:441`, `app/escala/utils.py:502`, `app/escala/utils.py:520`, `app/chamados/juizes.py:47`, `app/chamados/juizes.py:50`)

Executei a suite inteira com o runner em memória, `python3 -B`, trocando somente `SimpleTestCase` por `unittest.TestCase`: baseline **22 testes OK**, mutante acima **22 testes OK**, sem arquivos persistidos; resultado desta execução não dispõe de arquivo:linha e deve ser marcado **[nao verificado]** se a exigência de referência for aplicada também à evidência de ferramenta. A infraestrutura Django oficial permanece **[nao verificado]**. [nao verificado]

**d — PROCEDENTE quanto a fatos que a suite não prova; falsidade não demonstrada.** O docstring afirma uma execução histórica “do construtor”, data, duração de 30.656s e quatro resultados externos RED. São declarações documentais; os testes não comprovam autoria, data, duração nem proveniência dessa execução. (`app/core/tests/test_medida_turno_do_par.py:58`, `app/core/tests/test_medida_turno_do_par.py:59`, `app/core/tests/test_medida_turno_do_par.py:60`, `app/core/tests/test_medida_turno_do_par.py:63`)

O runner é fornecido, e a certificação Code/opus e suite oficial são explicitamente `[nao verificado]`; portanto não há ali alegação de certificação oficial. (`app/core/tests/test_medida_turno_do_par.py:18`, `app/core/tests/test_medida_turno_do_par.py:64`)

“MESMO commit” é uma exigência textual: as asserções comparam valores e listas da árvore corrente; não consultam histórico nem demonstram que atualização e código pertencem ao mesmo commit. (`app/core/tests/test_medida_turno_do_par.py:364`, `app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:415`)

**f — linhas novas/endurecidas.** `_scan` escreve linhas das ocorrências A1 e B1; `censo_fontes` as publica; o teste real lê apenas presença e a fixture lê igualdade exata. (`app/core/medida_turno_do_par.py:191`, `app/core/medida_turno_do_par.py:214`, `app/core/medida_turno_do_par.py:350`, `app/core/tests/test_medida_turno_do_par.py:412`, `app/core/tests/test_medida_turno_do_par.py:454`)

## 07/11 17:01 · sessao b4fd11f2739c · - · - · origem=Ronald (tmux)

**Item 4: a cura fecha as formas diretas ensaiadas, mas não a classe inteira.** A guarda procura destino `Name/Attribute` chamado `data_jornada` e ocorrência literal `Name('_prev_dj')` na expressão; não acompanha aliases nem associa elementos de desempacotamento. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`)

- **NAO PROCEDE — falso na cópia atual:** `_gap` retorna `[2408]`; a atribuição real está dentro da comparação reconhecida de `_gap_h`. Isso verifica a fonte, não uma execução produtiva com banco/log. (`app/ponto/motor_calculo_v2.py:2405`, `app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`; execução produtiva: [nao verificado])
- **NAO PROCEDE — escape das formas diretas testadas:** tupla, atributo de destino, walrus e augassign contendo `_prev_dj` literal lançam `LookupError`, conforme exigido pelos respectivos casos. (`app/core/tests/test_medida_turno_do_par.py:459`, `app/core/tests/test_medida_turno_do_par.py:462`, `app/core/tests/test_medida_turno_do_par.py:463`, `app/core/tests/test_medida_turno_do_par.py:464`, `app/core/tests/test_medida_turno_do_par.py:465`, `app/core/tests/test_medida_turno_do_par.py:475`)
- **PROCEDENTE — falso estrutural:** `data_jornada, outro = (data_entrada, _prev_dj)` dispara a guarda embora `data_jornada` receba `data_entrada`: qualquer destino compatível é cruzado com qualquer ocorrência de `_prev_dj` no RHS. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:261`)
- **PROCEDENTE — escapes restantes:** `data_jornada = self._prev_dj`, `dados['data_jornada'] = _prev_dj` e `anterior = _prev_dj; data_jornada = anterior` retornam `[]`: o primeiro perde o nome literal de origem, o segundo perde o tipo permitido de destino e o terceiro exige propagação inexistente nessa guarda. (`app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`, `app/core/medida_turno_do_par.py:282`)

**Mutante sobrevivente do instrumento**, na guarda: (`app/core/medida_turno_do_par.py:278`)

```python
# antes
if inherited - recognized:
# mutante
if len(inherited - recognized) == 1:
```

Executei os **22 métodos**, em memória, com `SimpleTestCase` substituído por `unittest.TestCase`: **0 falhas, 0 erros**; resultado da execução sem artefato persistido: **[nao verificado]**. O runner corresponde à substituição documentada. (`app/core/tests/test_medida_turno_do_par.py:24`)

Com esse mutante, renomear `_gap_h` e duplicar a herança na fixture retorna normalmente **2/2**, retirando a parcela nomeada; resultado em memória: **[nao verificado]**. Os nove subcasos atuais têm exatamente uma atribuição não reconhecida, inclusive aqueles que acrescentam outra atribuição reconhecida; falta um caso com duas desconhecidas. (`app/core/tests/test_medida_turno_do_par.py:458`, `app/core/tests/test_medida_turno_do_par.py:468`, `app/core/tests/test_medida_turno_do_par.py:469`, `app/core/tests/test_medida_turno_do_par.py:475`; publicação da parcela: `app/core/medida_turno_do_par.py:361`, `app/core/medida_turno_do_par.py:363`)

## 07/11 17:01 · sessao 2a612d8fe253 · - · - · origem=Ronald (tmux)

- **2 — cura fecha a classe indicada na triagem:** o teste compara os 47 membros da lista inteira, por identidade e ordem; não compara só tamanho. Troca de um membro mantendo 47 foi rejeitada em memória por `_composicao`. A lista nasce em `app/core/tests/test_medida_turno_do_par.py:79`; igualdade em `app/core/tests/test_medida_turno_do_par.py:368`; aplicação à medida real em `app/core/tests/test_medida_turno_do_par.py:372`.
- **Limite semântico — FILA:** continua existindo exclusão integral de qualquer função alcançada pelo fechamento, inclusive por chamada morta. Congelar os membros detecta mudança da lista; não prova que cada corpo ou uso compartilhado pertence à autoridade. O fechamento está em `app/core/medida_turno_do_par.py:330`; exclusão em `app/core/medida_turno_do_par.py:343`; o teste deliberadamente aceita exclusão transitiva em `app/core/tests/test_medida_turno_do_par.py:597`.
- **b — NAO PROCEDE:** acusação de que troca de identidade com mesmo tamanho não seria cobrada. A igualdade integral cobra isso diretamente. `app/core/tests/test_medida_turno_do_par.py:368`.
- **5b — cura fecha a classe “leitor zero”:** `CLASSIFICAR` agora tem leitor que exige igualdade integral com a publicação calculada. Seu escritor permanece o registro estático em `app/core/tests/test_medida_turno_do_par.py:226`; leitor em `app/core/tests/test_medida_turno_do_par.py:415`. O cálculo é escrito por `difference`/`classify` em `app/core/medida_turno_do_par.py:373` e publicado em `app/core/medida_turno_do_par.py:381`.
- **FILA de nomenclatura:** o docstring chama `CLASSIFICAR` de “sem teto ou catraca”, embora sua igualdade possa reprovar alterações; as linhas seguintes explicam que é cobrança de registro, sem promover candidatos aos contadores. `app/core/tests/test_medida_turno_do_par.py:12`; `app/core/tests/test_medida_turno_do_par.py:14`; `app/core/tests/test_medida_turno_do_par.py:415`.
- **Quem escreve/lê composição:** fechamento escreve `composed` em `app/core/medida_turno_do_par.py:335`; exclusão lê em `app/core/medida_turno_do_par.py:343`; guarda de nomeado absorvido lê em `app/core/medida_turno_do_par.py:358`; publicação escreve `composicao` em `app/core/medida_turno_do_par.py:384`; teste lê em `app/core/tests/test_medida_turno_do_par.py:368`.
- **Quem escreve/lê expectativa:** `COMPOSICAO_REGISTRADA` é escrito em `app/core/tests/test_medida_turno_do_par.py:79` e lido em `app/core/tests/test_medida_turno_do_par.py:368`; o instrumento não importa esse registro, pois seus únicos imports são `ast` e `os`. `app/core/medida_turno_do_par.py:21`.

Mutante do instrumento sobrevivente específico dessas duas curas: **[nao verificado]**. Não executei a suite inteira; executei a prova direta da troca de identidade 47→47. **[nao verificado quanto à suite inteira]** [nao verificado]

## 07/11 16:58 · sessao d959e2a6a2ad · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Entrega para certificação, com as quatro curas:

| Cura/teste | Mutante que derruba | [nao verificado]
|---|---|
| [Composição congelada + MORDE](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:423) | Chamada nova sob `if False` no juiz; exige registro no MESMO commit. | [nao verificado]
| [Linhas exatas A1/B1](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:432) | `'linhas': [node.lineno]`, publicando o `def`. |
| [Gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:456) | Renomear `_gap_h`; remover a guarda faz o teste falhar. | [nao verificado]
| [Registro CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:414) | Trocar identidade mantendo a quantidade. |

Execução em memória: **22 testes OK; quatro mutantes externos RED, rc=1**. [Registro](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:58) [nao verificado]

[Runner da suite](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:18) e comandos individuais: [composição](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:52), [linhas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:53), [gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:54), [CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:55). [nao verificado]

Herança não reconhecida agora lança `LookupError` com “rever detector”; destino transitivo ignorado está declarado em `NAO_VE`. [Guarda](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:278), [limitação](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:39).

Certificação Code/opus e execução Django oficial: **[nao verificado]**.

## 07/11 16:49 · sessao bc9f57e9f38b · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**Há achados PROCEDENTES na exclusão por composição e na cobertura dos testes.** A medição atual reproduz 9/85, mas isso não prova que toda derivação exigida continuará contada após mudanças de forma. (`app/core/medida_turno_do_par.py:324`, `app/core/medida_turno_do_par.py:350`; `app/core/tests/test_medida_turno_do_par.py:31`)

A comparação histórica `ebe80893..c2058690` está **[nao verificado]**: esta cópia não disponibiliza o histórico Git.

1. **Critério literal — contagem atual correta; garantia incompleta.**

   **NAO PROCEDE** a acusação de promover A2/B2 ao contador: a composição dos contadores é exatamente A1 + nomeados e B1 + nomeados; as diferenças amplas são publicadas separadamente. Reproduzi A1=6, A2=15, B1=82, B2=171; nomeados=3; contadores=9/85; diferenças=9/89. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:358`, `app/core/medida_turno_do_par.py:360`)

   Os três nomeados correspondem às chamadas em `app/ponto/motor_calculo_v2.py:503` e `app/ponto/motor_calculo_v2.py:882`, e à herança em `app/ponto/motor_calculo_v2.py:2408`; entram positivamente nos dois contadores. (`app/core/medida_turno_do_par.py:57`, `app/core/medida_turno_do_par.py:345`, `app/core/medida_turno_do_par.py:349`)

   **PROCEDENTE:** existem maneiras de deixar de contar sem remover a derivação: composição expansível, descrita no item 2, e reconhecimento frágil do gap, descrito no item 4. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:324`)

2. **Allowlist e composição — distinguir duas coisas.**

   **NAO PROCEDE** chamar `REGISTRO` ou `SITIOS_NOMEADOS` de allowlist de funções: o primeiro é esperado da suite; o segundo acrescenta parcelas. Nenhum deles dispensa nominalmente uma função produtiva da varredura. (`app/core/tests/test_medida_turno_do_par.py:296`; `app/core/medida_turno_do_par.py:335`, `app/core/medida_turno_do_par.py:349`)

   Existem exclusões nominais de **universo**: diretórios em `_FORA`, diretórios ocultos e arquivos `test_*.py`; elas correspondem ao molde. (`app/core/medida_turno_do_par.py:26`, `app/core/medida_turno_do_par.py:225`, `app/core/medida_turno_do_par.py:227`; `app/core/medida_leitor_do_dia.py:247`, `app/core/medida_leitor_do_dia.py:249`)

   **PROCEDENTE — a composição funciona como isenção estrutural expansível.** Qualquer chamada AST alcançada pelo fechamento inclui sua função em `composed`; depois essa função inteira fica fora dos quatro critérios. Não há verificação de execução, exclusividade de uso ou responsabilidade pela pergunta do juiz. (`app/core/medida_turno_do_par.py:311`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Prova em memória: uma função externa contendo `entrada.date()` conta B1=1; acrescentar ao juiz apenas `if False: leitor(None)` faz B1=0, sem retirar a derivação externa. O algoritmo percorre essa chamada morta e aplica a exclusão integral. (`app/core/medida_turno_do_par.py:65`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Excluir a implementação do próprio juiz é compatível com “fora do juiz”; equiparar **todo chamado transitivo**, inclusive morto e compartilhado, à implementação autorizada é uma extensão do critério. Autorização explícita dessa extensão no aval fornecido: **[nao verificado]**. A suite consagra a exclusão transitiva, mas não demonstra sua legitimidade semântica. (`app/core/tests/test_medida_turno_do_par.py:416`, `app/core/tests/test_medida_turno_do_par.py:429`)

3. **MORDE — há mordida local, mas existe mutante sobrevivente.**

   **NAO PROCEDE** dizer que todos são decorativos: os testes de pareador e dia novos verificam incremento e rejeição por `SUBIU`. (`app/core/tests/test_medida_turno_do_par.py:322`, `app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:330`, `app/core/tests/test_medida_turno_do_par.py:333`)

   A cobertura efetiva dos 13 `test_MORDE` é esta:

   | Teste | O que efetivamente cobra | [nao verificado]
   |---|---|
   | Candidato amplo | Publicação de novos A2/B2 sem subir os contadores. `app/core/tests/test_medida_turno_do_par.py:310` |
   | Pareador novo | Um `range(..., 2)` novo sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:318` |
   | Dia novo | Alias de `entrada` seguido de `.date()` sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:328` |
   | Número curado | Diminui diretamente o resultado; testa `_catraca`, sem curar fonte nem exercitar detector. `app/core/tests/test_medida_turno_do_par.py:336` |
   | Âncora renomeada | Renomear cada função nomeada provoca exceção. `app/core/tests/test_medida_turno_do_par.py:344` |
   | Sintaxe inválida | Exige `SyntaxError` e identificação do arquivo. `app/core/tests/test_medida_turno_do_par.py:353` |
   | Leitura/travessia | Testa erro de `open` e raiz inexistente; não injeta falha intermediária de travessia ou encoding. `app/core/tests/test_medida_turno_do_par.py:358` |
   | Linhas/alias | Desloca fonte e verifica linhas **dos nomeados**; usa os aliases já presentes na fixture. `app/core/tests/test_medida_turno_do_par.py:374` |
   | Cura dos nomeados | Remove corpos, conserva âncoras e exige zero. `app/core/tests/test_medida_turno_do_par.py:383` |
   | Critérios lexicais | Exemplos de `zip`, `append`, `localdate` e `.date()` civil. `app/core/tests/test_medida_turno_do_par.py:394` |
   | Composição | Exige excluir auxiliar chamado e contar função independente do mesmo arquivo. `app/core/tests/test_medida_turno_do_par.py:416` |
   | Aninhamento | Exige atribuir B1 à função interna. `app/core/tests/test_medida_turno_do_par.py:433` |
   | Autoridade ausente | Remove arquivos inteiros e exige exceção. `app/core/tests/test_medida_turno_do_par.py:441` |

   **PROCEDENTE — mutante do instrumento que passa todos:** em `app/core/medida_turno_do_par.py:332`, substituir `'linhas': lines` por `'linhas': [node.lineno]`. Ele publica a linha do `def` em lugar da ocorrência: por exemplo, aponta `app/escala/utils.py:502`, embora o `range` esteja em `app/escala/utils.py:520`. A suite exige apenas linhas não vazias para A1/B1; a comparação precisa de deslocamento cobre os nomeados. (`app/core/tests/test_medida_turno_do_par.py:301`, `app/core/tests/test_medida_turno_do_par.py:380`)

   Esse mutante passou nos **18 métodos**, executados em memória com `SimpleTestCase` substituído por `unittest.TestCase`. A execução pela infraestrutura Django oficial está **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:26`, `app/core/tests/test_medida_turno_do_par.py:257`)

4. **Falha silenciosa — erros de arquivo propagam; detecção incompleta pode retornar normalmente.**

   **NAO PROCEDE** para erros reportados por `os.walk`, leitura/encoding, `SyntaxError`, autoridade ausente, raízes ausentes e funções âncora ausentes: os caminhos propagam exceção ou lançam `LookupError`. (`app/core/medida_turno_do_par.py:221`, `app/core/medida_turno_do_par.py:272`, `app/core/medida_turno_do_par.py:273`, `app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:337`, `app/core/medida_turno_do_par.py:374`)

   **PROCEDENTE para contagem parcial por forma:** renomeei somente `_gap_h` para `_intervalo_h` na fixture, mantendo a mesma herança. O censo retornou normalmente e ambos os contadores caíram de 3 para 2. O detector exige aquele identificador e, sem correspondência, simplesmente não acrescenta o nomeado. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:344`; fixture em `app/core/tests/test_medida_turno_do_par.py:249`)

   A catraca de números atuais acusaria essa queda; portanto o resultado parcial do **instrumento** não equivale a passagem silenciosa pela **suite**. (`app/core/tests/test_medida_turno_do_par.py:269`)

   **FILA:** destinos transitivos sem registro são ignorados, sem distinguir dependência local ausente de chamada externa não resolvida; somente as raízes têm presença exigida. (`app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:314`)

5. **ESMERIL-SEMANTICA — nome ambíguo, sem leitor mostrado usando outra unidade.**

   **NAO PROCEDE** uma divergência demonstrada entre escritor e leitor: `numero` mede parcelas; `itens` contém essas parcelas; `funcoes` projeta suas chaves **com repetição**; `linhas` contém ocorrências AST dentro da parcela. A suite compara essas mesmas relações. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:294`, `app/core/tests/test_medida_turno_do_par.py:295`)

   **FILA de nomenclatura:** `funcoes` não é conjunto de funções únicas. Uma função B1 que também contém um nomeado aparece duas vezes, multiplicidade declarada pelo instrumento. Nenhum consumidor mostrado interpreta essa lista como conjunto único. (`app/core/medida_turno_do_par.py:10`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:295`)

6. **Universo — nenhum outro desvio encontrado frente a `producao`.**

   **NAO PROCEDE:** reproduzi listas idênticas, com 755 arquivos, entre os dois `producao`. As regras de seleção correspondem e a suite compara as listas completas. (`app/core/medida_turno_do_par.py:224`; `app/core/medida_leitor_do_dia.py:246`; `app/core/tests/test_medida_turno_do_par.py:449`)

   `e6_piso_autopsia` não é excluído por `producao` do molde: sua exclusão pertence ao estágio `NAO_E_LEITOR` daquele outro detector. Portanto sua inclusão aqui não constitui desvio de `producao`. (`app/core/medida_leitor_do_dia.py:240`, `app/core/medida_leitor_do_dia.py:282`; `app/core/tests/test_medida_turno_do_par.py:452`)

   A atribuição histórica da diferença 83→82 exclusivamente a `bin/` consta da documentação da obra; recomposição independente do censo antigo: **[nao verificado]**. (`app/core/medida_turno_do_par.py:5`, `app/core/tests/test_medida_turno_do_par.py:4`)

7. **Juiz novo / segundo escritor / relógio / derivação paralela.**

   **NAO PROCEDE nos dois arquivos examinados:** o instrumento importa `ast`/`os`, lê fontes e devolve estruturas em memória; não escreve dados de negócio nem consulta relógio. (`app/core/medida_turno_do_par.py:21`, `app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:374`)

   **FILA de negócio:** as portas do motor e a herança continuam presentes, agora medidas como RED nomeado. A receita `turnos_de_batidas` copia batidas, prepara parâmetros e chama o pareador; o gap continua escrevendo `data_jornada`. (`app/ponto/turnos.py:1515`, `app/ponto/turnos.py:1533`; `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:57`)

8. **QUEM ESCREVE / QUEM LÊ os valores públicos e insumos.**

   A autoridade de negócio vem do registro vivo; `JUIZES.md` se declara lookup. O instrumento lê esse registro, não o Markdown. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`; `app/core/juizes.py:196`; `app/core/medida_turno_do_par.py:232`)

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | Fontes e árvores AST | Leitura em `app/core/medida_turno_do_par.py:374`; parse em `app/core/medida_turno_do_par.py:272` | Indexação em `app/core/medida_turno_do_par.py:277` |
   | Autoridade | `JUIZES`, `app/core/juizes.py:196` | `_autoridade`, `app/core/medida_turno_do_par.py:232`; raízes em `app/core/medida_turno_do_par.py:275` |
   | Composição | Fechamento em `app/core/medida_turno_do_par.py:311`; publicação em `app/core/medida_turno_do_par.py:365` | Exclusão em `app/core/medida_turno_do_par.py:324`; teste em `app/core/tests/test_medida_turno_do_par.py:429` |
   | `chave`, `criterio`, `linhas` lexicais | `app/core/medida_turno_do_par.py:331` | Montagem em `app/core/medida_turno_do_par.py:349`; validação em `app/core/tests/test_medida_turno_do_par.py:296` |
   | `sitio`, nomeados e suas `linhas` | `app/core/medida_turno_do_par.py:342` | Ambos os contadores em `app/core/medida_turno_do_par.py:349`; testes em `app/core/tests/test_medida_turno_do_par.py:365` |
   | `numero` | `len(items)`, `app/core/medida_turno_do_par.py:350` | `_catraca`, `app/core/tests/test_medida_turno_do_par.py:266` |
   | `itens` | Lexicais + nomeados, `app/core/medida_turno_do_par.py:349` | Consistência/identidade em `app/core/tests/test_medida_turno_do_par.py:294` |
   | `funcoes` | Projeção com multiplicidade, `app/core/medida_turno_do_par.py:351` | Comparação em `app/core/tests/test_medida_turno_do_par.py:295` |
   | `a_classificar_pela_regra` | Diferença de chaves em `app/core/medida_turno_do_par.py:354` | Testes em `app/core/tests/test_medida_turno_do_par.py:303` |
   | `criterios` | Tamanhos das listas, `app/core/medida_turno_do_par.py:363` | Testes em `app/core/tests/test_medida_turno_do_par.py:288` |
   | `universo_arquivos`, `ilegiveis` | `len(trees)` e constante `[]`, `app/core/medida_turno_do_par.py:366` | Testes em `app/core/tests/test_medida_turno_do_par.py:290` |
   | `nao_ve` | Constante em `app/core/medida_turno_do_par.py:31` | Comparação em `app/core/tests/test_medida_turno_do_par.py:299` |
   | `esperado_no_fim` | Constante zero em `app/core/medida_turno_do_par.py:352` | Teste em `app/core/tests/test_medida_turno_do_par.py:300`; a catraca atual lê outro teto em `app/core/tests/test_medida_turno_do_par.py:265` |
   | Rótulo `contador` | `app/core/medida_turno_do_par.py:350`, `app/core/medida_turno_do_par.py:360` | Consumidor externo **[nao verificado]** |
   | Esperados numéricos e `REGISTRO` | `app/core/tests/test_medida_turno_do_par.py:31` | Cat­raca e identidade em `app/core/tests/test_medida_turno_do_par.py:265`, `app/core/tests/test_medida_turno_do_par.py:296` |
   | `CLASSIFICAR` estático | `app/core/tests/test_medida_turno_do_par.py:131` | Nenhum leitor localizado; os testes leem a classificação calculada em `app/core/tests/test_medida_turno_do_par.py:305` |

   Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**. [nao verificado]

_2781 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
