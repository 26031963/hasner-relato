# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 10:38** · publicado em **07/11/2026 10:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 10:38 · sessao 58b366715336 · FIO-E0-V4-E1 · verificador · origem=Code

**A. PROCEDENTE: ainda há significados e consultas de identidade divergentes.** O índice usa a instância persistida e o episódio; parte das portas continua usando a posição fixa do motivo ou disputa+motivo+dia (`app/chamados/models.py:2508`; `app/chamados/services/disputa_emissao.py:321`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:53`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:58`).

O censo abaixo identifica escritores e cadeias leitoras encontrados; exaustividade de acessos dinâmicos, execução dos testes e incidência em produção: **[nao verificado]**. [nao verificado]

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade` e `descarimbar`; também `descarimbar_em_lote`, `encerrar_por_fato_no_espelho` e a porta de relavratura (`app/chamados/models.py:2720`; `app/chamados/models.py:2884`; `app/chamados/models.py:2893`; `app/chamados/services/relavra_via.py:123`). | Identidade: `do_fato`, `linhas_dos_fatos`, `_no_fato`, retratação e backfill (`app/chamados/models.py:2486`; `app/chamados/models.py:2510`; `app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:993`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:90`). Vitalidade/materialização: C6, `q_vivas`, absorção e relavratura (`app/chamados/juizes.py:1147`; `app/chamados/juizes.py:1173`; `app/chamados/models.py:2968`; `app/chamados/services/materializacao.py:623`; `app/chamados/services/relavra_via.py:179`). **Não há exclusão uniforme**: a fábrica exclui somente `duplicata`, deixando `motivo_retratado` segurar emissão (`app/chamados/services/fabrica_da_celula.py:119`). |
| `batida_origem` da Pergunta | `_nascer` e `_chave_de_identidade`, pelo derivador que lê `ultima_batida.id`; a 0052 deixa o acervo NULL (`app/chamados/models.py:2591`; `app/chamados/models.py:3057`; `app/chamados/models.py:3072`; `app/chamados/migrations/0052_pergunta_batida_origem.py:19`). | `do_fato`, `fato_livre_para`, `_colisao_ao_datar`, migrador, agrupamento e índice (`app/chamados/models.py:2483`; `app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:3095`). **Não há consenso operacional**: o emissor pode barrar antes de consultar episódio; `do_fato` também agrega acervo sem episódio pelo dia (`app/chamados/services/disputa_emissao.py:321`; `app/chamados/models.py:2482`). `_achar_batida_origem(chamado)` é outra função, referente à entrada do turno, e não lê essa coluna (`app/chamados/services/materializacao.py:109`). |
| `data_evento` | Nascimento datado/conversa; posteriormente, `reapontar_resolvedora` e `rechavear_data_evento` (`app/chamados/models.py:2541`; `app/chamados/models.py:2552`; `app/chamados/models.py:2641`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`). | Identidade e competência: lote, agrupamento, backfill e índice (`app/chamados/models.py:2508`; `app/chamados/services/fio.py:1046`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:107`; `app/chamados/models.py:3090`). Plantio/ausência: `materializacao` (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:473`; `app/chamados/services/materializacao.py:517`). Validação, C6, lavra, reconciliação e apresentação (`app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:433`; `app/chamados/juizes.py:1199`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/reconciliador.py:1543`; `app/chamados/services/fio.py:87`). **Dois significados explícitos**: dia do turno no fato e dia da pergunta na conversa; leitores de identidade que não filtram `conversa` os misturam (`app/chamados/models.py:2552`; `app/chamados/services/disputa_emissao.py:394`). |
| `instancia_marco` | `_chave_de_identidade` deriva somente quando vazia; valor explícito é preservado. A 0051 preenche posições fixas (`app/chamados/models.py:3054`; `app/chamados/migrations/0051_decisao_pergunta.py:27`). | Índice/agrupamento usam a coluna persistida; `linhas_dos_fatos`, alcançada pelas portas e migrador, substitui-a por `instancia_do_motivo(m)` (`app/chamados/models.py:3090`; `app/chamados/services/fio.py:1016`; `app/chamados/models.py:2508`; `app/chamados/services/fio.py:1107`). **Divergência comprovada**: a coluna admite `I1/I2`, mas a consulta procura `int_ida/int_volta`; o próprio derivador declara que ainda não carrega índice da pausa (`app/chamados/tests/test_e1_v5_indice.py:124`; `app/chamados/tests/test_e1_v5_indice.py:125`; `app/chamados/services/declaracao_texto.py:45`; `app/chamados/services/declaracao_texto.py:50`). |
| `colaborador` da Pergunta | Derivado e conferido contra a disputa **na inserção**; preenchimento histórico pela 0051 (`app/chamados/models.py:3038`; `app/chamados/models.py:3048`; `app/chamados/migrations/0051_decisao_pergunta.py:25`). | Portas, agrupamento, backfill e desfazer usam a coluna (`app/chamados/models.py:2509`; `app/chamados/services/fio.py:936`; `app/chamados/services/fio.py:1044`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:100`; `app/chamados/services/relavra_via.py:221`). C6, validação, fábrica e materialização continuam alcançando a pessoa pela disputa (`app/chamados/juizes.py:665`; `app/chamados/services/validacao.py:421`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/materializacao.py:282`). O nascimento exige igualdade; preservação universal após escritas dinâmicas: **[nao verificado]**. |
| `conversa` | `abrir=False`, `abrir_conversa=True`, persistidos por `_nascer`; acervo recebe default `False` na 0051 (`app/chamados/models.py:2541`; `app/chamados/models.py:2552`; `app/chamados/models.py:2642`; `app/chamados/migrations/0051_decisao_pergunta.py:99`). | Porta, derivador, migrador, agrupamento e índices distinguem conversa (`app/chamados/models.py:2523`; `app/chamados/models.py:2566`; `app/chamados/models.py:3054`; `app/chamados/services/fio.py:1104`; `app/chamados/services/fio.py:1029`; `app/chamados/models.py:3091`). **Outros dedups não distinguem**, relacionados em C (`app/chamados/services/disputa_emissao.py:322`; `app/chamados/services/declaracao_texto.py:237`). Identificação correta das conversas antigas: **[nao verificado]**; a migração não as classifica individualmente (`app/chamados/migrations/0051_decisao_pergunta.py:99`). |
| `chamado_destino` | Nascimento/reuso em `_nascer` e reapontamento pela porta do fio (`app/chamados/models.py:2613`; `app/chamados/models.py:2642`; `app/chamados/services/fio.py:869`). As atribuições da simulação de costura e de `_vivas_entre` são em memória (`app/chamados/services/fio.py:780`; `app/chamados/services/fio.py:784`; `app/chamados/models.py:361`). | Costura/E4-c, migrador, materialização, C6, relavratura, declaração, reconciliação e listagem (`app/chamados/services/fio.py:857`; `app/chamados/services/fio.py:1095`; `app/chamados/services/materializacao.py:1254`; `app/chamados/juizes.py:744`; `app/chamados/services/relavra_via.py:101`; `app/chamados/services/declaracao_texto.py:324`; `app/chamados/reconciliador.py:1487`; `app/chamados/services/fio.py:80`). **O desvio está em usá-lo como identidade**: `_derivar_chamado_destino` trata chamado+motivo+carimbo nulo como impedimento do mesmo fato (`app/chamados/services/disputa_emissao.py:214`; `app/chamados/services/disputa_emissao.py:226`). |

Há ainda leitores externos dos valores projetados: API, calendário, prévia, histórico, explicador, cobrança e comandos de auditoria/migração (`app/api/views.py:2606`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/chamados/services/historico.py:108`; `app/chamados/services/explicador.py:95`; `app/chamados/services/cobranca.py:83`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:130`).

**Achado adicional do backfill — PROCEDENTE.** Uma `admin_decidiu` sem ato e sem resposta pode integrar colisão com uma irmã respondida. A irmã respondida vence; o primeiro laço retrata a `admin_decidiu`; o segundo a pula porque a via agora é `duplicata`. Assim, esse caminho termina sem escrever seu SILENCIO (`app/chamados/services/fio.py:985`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:131`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:142`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:145`). Existência desse caso no col90: **[nao verificado]**.

**B. NAO PROCEDE para a recusa sequencial corrigida; PROCEDENTE para canônica desatualizada.**

Todos os chamadores encontrados de `retratar_duplicata`:

| Chamador | Referência |
|---|---|
| `reapontar_resolvedora` | `app/chamados/services/fio.py:868` |
| `migrar_resolvedoras_vivas` | `app/chamados/services/fio.py:1122` |
| `dedup_perguntas_fato.handle` | `app/chamados/management/commands/dedup_perguntas_fato.py:83` |
| `backfill_decisoes_pergunta.aplicar` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:131` |
| Testes diretos | `app/chamados/tests/test_e1_v5_portas_backfill.py:159`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:390`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:398` |

- **NAO PROCEDE a antiga antecipação em `reapontar_resolvedora`:** E4-c e costura são aceitos antes da retratação; recusa retorna antes dela. Retratação e gravação da substituta permanecem no mesmo `atomic` (`app/chamados/services/fio.py:844`; `app/chamados/services/fio.py:857`; `app/chamados/services/fio.py:863`; `app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:871`).
- **PROCEDENTE a janela do migrador:** captura as vivas do destino sem travar o conjunto; quando a canônica já está no destino, pula o reapontamento e retrata diretamente a fonte. O chamador `reabrir` não envolve esse ramo em transação própria (`app/chamados/services/fio.py:1095`; `app/chamados/services/fio.py:1113`; `app/chamados/services/fio.py:1122`; `app/chamados/models.py:1387`).
- **Contraexemplo concorrente permitido pelo código:** depois da consulta do fato, a canônica sai por `remover_motivo`; o migrador então retrata a fonte contra a instância antiga. `remover_motivo` não toma a trava do colaborador nesse ato, e `retratar_duplicata` não confere a canônica atual nem igualdade de chave (`app/chamados/models.py:2000`; `app/chamados/models.py:2008`; `app/chamados/services/fio.py:993`; `app/chamados/services/fio.py:1002`). Resultado possível: nenhuma linha permanece na identidade do fato; ocorrência efetiva: **[nao verificado]**.
- **A mesma classe alcança os commands:** dedup escolhe canônica antes do `atomic`; backfill relê os PKs travados, mas não reaplica `_no_fato` antes de escolher a canônica (`app/chamados/management/commands/dedup_perguntas_fato.py:66`; `app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:128`).

**C. PROCEDENTE: sobrevivem estes agrupadores/gatilhos com identidade diferente.**

| Sítio | Diferença para a 0053 |
|---|---|
| `linhas_dos_fatos` → `do_fato` → portas/migrador | Deriva instância fixa pelo motivo, em vez de receber a instância persistida (`app/chamados/models.py:2508`; `app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:1107`). |
| `do_fato`, BATIDA | Une episódio conhecido ao acervo sem episódio do mesmo dia; sem episódio, retorna à chave por dia (`app/chamados/models.py:2482`; `app/chamados/models.py:2488`). |
| `gerar_perguntas_disputa`, ramo BATIDA | Disputa+motivo, sem episódio e sem excluir conversa (`app/chamados/services/disputa_emissao.py:321`). |
| `gerar_perguntas_disputa`, ramos datados | Disputa+motivo+dia; exclui as vias, mas não conversa nem instância (`app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:410`). |
| `_derivar_chamado_destino` | Chamado+motivo+carimbo nulo, sem chave completa do fato (`app/chamados/services/disputa_emissao.py:226`). |
| `_ja_perguntou` | Disputa+motivo+dia antes de consultar `fio_vivo`; sem os filtros de identidade (`app/chamados/services/disputa_emissao.py:1297`; `app/chamados/services/disputa_emissao.py:1304`). |
| `_candidata_reusavel` | Disputa+motivo+dia antes do predicado de reuso (`app/chamados/services/declaracao_texto.py:236`). |
| `fabrica_da_celula.motivos_a_perguntar` | Colaborador+dia, reduzido a conjunto de motivos; exclui só duplicata. Uma retratada/conversa pode calar emissão (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`). |
| `disparar_perguntas_competencia` | Colaborador+motivo+dia, sem vias/conversa/instância; abandona como fato existente (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`). |
| `_marco_ja_perguntado` | Colaborador+dia+motivos; qualquer pergunta morta pode representar marco encerrado, sem os filtros do índice (`app/chamados/services/pergunta_no_app.py:109`). |
| `insumos_em_regularizacao` | Agrupa motivo+dia após restringir à instância fixa do derivador; não representa instâncias distintas (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/declaracao_texto.py:550`). |

O caso BATIDA tem consequência direta: a disputa aberta é reutilizada e o gerador é chamado novamente, mas seu filtro disputa+motivo cala a segunda batida antes da porta (`app/chamados/services/disputa_emissao.py:515`; `app/chamados/services/disputa_emissao.py:666`; `app/chamados/services/disputa_emissao.py:321`). O teste alterado fecha a disputa entre as batidas e não cobre esse caminho (`app/chamados/tests/test_a2_data_evento_batida.py:33`; `app/chamados/tests/test_a2_data_evento_batida.py:42`).

**D. PROCEDENTE: a ordem literal ainda não é “SEMPRE”.**

- **Pergunta antes de Colaborador:** dedup abre `atomic` e retrata sem tomar a trava do colaborador; chega ao `save` da Pergunta por `carimbar_identidade` (`app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/services/fio.py:1002`; `app/chamados/models.py:2812`).
- **Pergunta antes de Disputa:** `Disputa.fechar`, no encerramento administrativo/desligamento, carimba perguntas antes de atualizar a disputa (`app/chamados/models.py:1880`; `app/chamados/models.py:1895`).
- **Pergunta antes de Chamado:** `fechar_o_fio_do_dia` carimba perguntas e depois reconcilia chamados; o reconciliar pode travar/atualizar Chamado (`app/ponto/services/veredito_celula.py:180`; `app/ponto/services/veredito_celula.py:192`; `app/chamados/models.py:687`; `app/chamados/models.py:285`).
- **Validação:** atualiza Pergunta antes de uma devolução que pode reabrir Disputa no mesmo `atomic` (`app/chamados/services/validacao.py:417`; `app/chamados/services/validacao.py:422`; `app/chamados/services/validacao.py:451`; `app/chamados/services/validacao.py:218`; `app/chamados/models.py:2124`).
- **Desfazer:** toma Colaborador primeiro, mas usa uma única consulta `FOR UPDATE OF self, disputa`; não estabelece separadamente Disputa→Pergunta. Ordem efetiva dessas duas aquisições no banco: **[nao verificado]** (`app/chamados/services/relavra_via.py:226`; `app/chamados/services/relavra_via.py:230`).

**NAO PROCEDE nos chamadores de produção encontrados a acusação de `trava_do_colaborador` fora de transação.** Os chamadores de `fechar_o_fio_do_dia` e `travar_origem_antes` estão sob `atomic`; o helper não cria sua própria transação (`app/ponto/services/veredito_celula.py:214`; `app/ponto/services/declarar_dia.py:188`; `app/chamados/services/validacao.py:281`; `app/ponto/services/justificativa.py:131`; `app/ponto/services/justificativa.py:264`; `app/ponto/services/justificativa.py:333`; `app/ponto/views.py:1792`; `app/chamados/models.py:2456`).

**E. PROCEDENTE parcialmente: há mordida real, mas também provas insuficientes.**

| Teste | Aval | [nao verificado]
|---|---|
| `test_e1_v5_chave_familia`, dedup | **NAO PROCEDE “vazio”**: cria fatos, executa command e confere vias persistidas; MORDE remove o índice para montar acervo duplicado (`app/chamados/tests/test_e1_v5_chave_familia.py:54`; `app/chamados/tests/test_e1_v5_chave_familia.py:67`; `app/chamados/tests/test_e1_v5_chave_familia.py:73`). |
| Migração, episódios distintos | **PROCEDENTE a insuficiência**: só exige ausência de duplicatas; migrador sem ação satisfaria essas asserções. Não exige destino final nem `migradas` (`app/chamados/tests/test_e1_v5_chave_familia.py:82`; `app/chamados/tests/test_e1_v5_chave_familia.py:88`). |
| Migração, mesmo episódio | **PROCEDENTE a cobertura parcial**: prova retratação da irmã, mas não a canônica efetivamente no destino (`app/chamados/tests/test_e1_v5_chave_familia.py:91`; `app/chamados/tests/test_e1_v5_chave_familia.py:98`). |
| Migração, destino recusado | **PROCEDENTE a limitação do mock**: substitui integralmente `reapontar_resolvedora`; prova reação ao retorno False, não recusa real/atomicidade (`app/chamados/tests/test_e1_v5_chave_familia.py:108`). |
| `OrdemDeTravaDaValidacaoEDoDesfazerTest` | **Não é vazio para Colaborador antes**: exige Pergunta na captura e possui mutantes removendo a trava. **É insuficiente para o aval completo**: filtra deliberadamente todas as violações que não sejam “sem o Colaborador antes” (`app/chamados/tests/test_e1_v5_ordem_trava.py:231`; `app/chamados/tests/test_e1_v5_ordem_trava.py:249`; `app/chamados/tests/test_e1_v5_ordem_trava.py:253`; `app/chamados/tests/test_e1_v5_ordem_trava.py:259`). |
| Capturador de travas | **PROCEDENTE a cegueira**: registra apenas a tabela do `FROM` e elimina repetições por tabela. Não captura a Disputa adicional do `FOR UPDATE OF self, disputa`, nem distingue novas linhas da mesma tabela (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`; `app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `app/chamados/tests/test_e1_v5_ordem_trava.py:43`; `app/chamados/services/relavra_via.py:230`). |
| `ColisaoAoDatarTest` MORDE | **NAO PROCEDE “vazio/mock”**: usa destino real de outro dia, exige recusa, irmã preservada e uma linha no fato. Morde a antiga retratação antecipada (`app/chamados/tests/test_e1_v5_portas_backfill.py:137`; `app/chamados/tests/test_e1_v5_portas_backfill.py:142`; `app/chamados/tests/test_e1_v5_portas_backfill.py:146`). |
| `FatoRetratadoVoltaTest` RED datado | **NAO PROCEDE “vazio”**: passa por emissão→remoção→emissão reais e exige PK nova. Cobre o ramo de intervalo; não o outro filtro dos motivos com destino (`app/chamados/tests/test_e1_volta2.py:271`; `app/chamados/tests/test_e1_volta2.py:274`; `app/chamados/tests/test_e1_volta2.py:284`; `app/chamados/services/disputa_emissao.py:410`). |
| Instâncias `I1/I2` | **PROCEDENTE a lacuna**: testa somente inserções cruas; não passa pelas portas que substituem a instância pela posição do motivo (`app/chamados/tests/test_e1_v5_indice.py:124`; `app/chamados/models.py:2508`). |

Resultado de execução desses testes: **[nao verificado]**. [nao verificado]

**F. PROCEDENTE para `--ate` e para exportada sem PAREI.**

- **`--ate` fraciona grupo BATIDA:** data não pertence à chave, mas o filtro é aplicado antes da agregação; a busca dos PKs reutiliza a mesma base recortada (`app/chamados/services/fio.py:1017`; `app/chamados/services/fio.py:1045`; `app/chamados/services/fio.py:1049`).
- **Contraexemplo estático:** três irmãs do episódio, duas antes do corte e a terceira respondida depois. A canônica global respondida fica fora; o command retrata uma irmã usando outra canônica local. Se houver apenas uma irmã dentro e outra fora, nem detecta colisão (`app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:985`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- **`--col

[truncado em 20000 de 23734 caracteres]

## 07/11 10:35 · sessao 308a91c03dbd · - · - · origem=Ronald (tmux)

A/C — **PROCEDENTE: os leitores ainda não têm uma identidade única compatível com o índice.**

- **`instancia_marco` tem dois alcances.** O índice aceita a instância persistida (`app/chamados/models.py:3090`); o escritor preserva instância explícita e só deriva quando vazia (`app/chamados/models.py:3054`). Já `linhas_dos_fatos` sempre impõe a posição fixa derivada do motivo, sem receber a instância da pergunta (`app/chamados/models.py:2508`; `app/chamados/services/declaracao_texto.py:42`). Portanto `fato_livre_para` e `_colisao_ao_datar` podem consultar `int_ida` ao tratar uma pergunta `I2` (`app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`), e `migrar_resolvedoras_vivas` pode retratar fatos de instâncias diferentes (`app/chamados/services/fio.py:1107`; `app/chamados/services/fio.py:1122`).
- **BATIDA ainda sofre dedup por disputa+motivo.** `gerar_perguntas_disputa` descarta qualquer motivo não datado se já existe na disputa, sem episódio nem `conversa=False` (`app/chamados/services/disputa_emissao.py:321`). O conjunto datado contém os motivos de destino e os intervalos (`app/chamados/services/disputa_emissao.py:26`). A emissão repetida reutiliza a disputa aberta e volta a chamar esse gerador (`app/chamados/services/disputa_emissao.py:515`; `app/chamados/services/disputa_emissao.py:666`), portanto uma segunda batida com `gps_ausente` na mesma disputa é calada antes da porta. O teste alterado fecha a disputa entre as batidas (`app/chamados/tests/test_a2_data_evento_batida.py:33`; `app/chamados/tests/test_a2_data_evento_batida.py:42`).
- **Fallback BATIDA por dia permanece deliberadamente diferente do índice.** Com episódio informado, `do_fato` une o episódio às linhas antigas sem origem daquele dia; sem episódio, usa exclusivamente a chave por dia (`app/chamados/models.py:2482`; `app/chamados/models.py:2488`). Isso permite que uma linha antiga sem episódio retenha episódios distintos do mesmo dia; o teste o exige explicitamente (`app/chamados/tests/test_e1_v5_indice.py:111`).
- **Outros gatilhos/leitores de identidade divergente:**
  - Ramos datados de `gerar_perguntas_disputa`: disputa+motivo+dia, sem conversa/instância (`app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:410`).
  - `_derivar_chamado_destino`: chamado+motivo+carimbo nulo; o comentário chama isso de “MESMO FATO”, mas não confere instância/conversa (`app/chamados/services/disputa_emissao.py:214`; `app/chamados/services/disputa_emissao.py:226`).
  - `_ja_perguntou`: disputa+motivo+dia sem excluir vias/conversa/instância antes do `fio_vivo`; chamado pelo agrupador e pelo cron (`app/chamados/services/disputa_emissao.py:1297`; `app/chamados/services/disputa_emissao.py:813`; `app/chamados/management/commands/disparar_perguntas_competencia.py:173`).
  - `_candidata_reusavel`: disputa+motivo+dia sem vias/conversa/instância (`app/chamados/services/declaracao_texto.py:236`).
  - `fabrica_da_celula.motivos_a_perguntar`: colab+dia e conjunto de motivos, excluindo só duplicata; retratada/conversa podem calar a volta (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`).
  - Cron `disparar_perguntas_competencia`: colab+motivo+dia sem exclusões; classifica como fato já existente e abandona (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`).
  - Diagnóstico `_marco_ja_perguntado`: colab+dia+motivos sem vias/conversa/instância; qualquer morta satisfaz “marco encerrado” (`app/chamados/services/pergunta_no_app.py:109`).
  - `insumos_em_regularizacao` agrupa por motivo+dia, após restringir à instância fixa de `linhas_dos_fatos`; também não representa I1/I2 (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/declaracao_texto.py:550`).

Inventário A dos quatro campos delegados:

| Campo | Escritores de produção/históricos | Leitores |
|---|---|---|
| `instancia_marco` | `_chave_de_identidade`, preservando explícito (`app/chamados/models.py:3054`); backfill histórico (`app/chamados/migrations/0051_decisao_pergunta.py:27`) | `linhas_dos_fatos` (`app/chamados/models.py:2508`), constraint (`app/chamados/models.py:3090`), chave/grupos (`app/chamados/services/fio.py:1016`; `app/chamados/services/fio.py:1047`), guarda/índice histórico (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:53`) |
| `batida_origem` **da PD** | `_nascer` (`app/chamados/models.py:2591`), `_chave_de_identidade` (`app/chamados/models.py:3057`), ambos pelo derivador de contexto que valida colab (`app/chamados/models.py:3069`); migração cria NULL (`app/chamados/migrations/0052_pergunta_batida_origem.py:19`) | `do_fato` (`app/chamados/models.py:2483`), `fato_livre_para` (`app/chamados/models.py:2525`), constraint (`app/chamados/models.py:3095`), `_colisao_ao_datar` (`app/chamados/services/fio.py:890`), chave/null/grupos (`app/chamados/services/fio.py:1017`; `app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1042`), migrar (`app/chamados/services/fio.py:1108`), migração0053 (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:35`) |
| `conversa` | `abrir=False` (`app/chamados/models.py:2541`), `abrir_conversa=True` (`app/chamados/models.py:2552`), persistência `_nascer` (`app/chamados/models.py:2642`); acervo defaultFalse (`app/chamados/migrations/0051_decisao_pergunta.py:99`) | `do_fato`/lote (`app/chamados/models.py:2484`; `app/chamados/models.py:2509`), `fato_livre_para` (`app/chamados/models.py:2523`), nascimento/trava (`app/chamados/models.py:2566`; `app/chamados/models.py:2586`), derivador de identidade (`app/chamados/models.py:3054`), constraints (`app/chamados/models.py:3091`; `app/chamados/models.py:3096`), fio `_no_fato`/migrar (`app/chamados/services/fio.py:1029`; `app/chamados/services/fio.py:1104`), migração0053 (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:26`) |
| `colaborador` **denormalizado da PD** | `_chave_de_identidade` deriva/valida contra disputa só na inserção (`app/chamados/models.py:3038`; `app/chamados/models.py:3048`); migração0051 (`app/chamados/migrations/0051_decisao_pergunta.py:25`) | lote/porta (`app/chamados/models.py:2509`; `app/chamados/models.py:2521`), reapontar/colisão/rechavear/migrar (`app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:889`; `app/chamados/services/fio.py:936`; `app/chamados/services/fio.py:1107`), grupos (`app/chamados/services/fio.py:1044`), backfill (`app/chamados/management/commands/backfill_decisoes_pergunta.py:62`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:100`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:139`), desfazer (`app/chamados/services/relavra_via.py:221`) |

`_achar_batida_origem` é função legada que recebe chamado, não leitora da nova coluna PD (`app/chamados/services/materializacao.py:109`). Na cópia não há outros escritores de produção encontrados desses quatro campos além dos acima; exaustividade de acessos dinâmicos [nao verificado].

F — **PROCEDENTE para `--ate`; `--colab` não fraciona um grupo íntegro por origem.**

- A chave BATIDA é batida+motivo (`app/chamados/services/fio.py:1017`), mas o código filtra linhas por data **antes** de formar o grupo e continua usando a base recortada para os PKs (`app/chamados/services/fio.py:1045`; `app/chamados/services/fio.py:1049`). Contraexemplo estático: três irmãs do mesmo episódio, duas antes do corte e a terceira respondida depois; a canônica global respondida some do cálculo, e uma das anteriores é retratada pela canônica local (`app/chamados/management/commands/dedup_perguntas_fato.py:66`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- `--colab` é aplicado da mesma forma (`app/chamados/services/fio.py:1043`), porém uma BATIDA íntegra pertence a uma pessoa, e o derivador valida a origem contra o colaborador (`app/chamados/models.py:3076`). Há criação explícita por `**campos` preservando origem informada sem validação correspondente, portanto integridade universal dessa relação [nao verificado] (`app/chamados/models.py:2591`; `app/chamados/models.py:3057`).
- `dedup_perguntas_fato --apply` não consulta competência exportada nem emite PAREI: decide e grava diretamente (`app/chamados/management/commands/dedup_perguntas_fato.py:48`; `app/chamados/management/commands/dedup_perguntas_fato.py:81`). Também não toma trava do colaborador antes de retratar (`app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- O backfill usa outra proteção: verifica qualquer membro exportado (`app/chamados/management/commands/backfill_decisoes_pergunta.py:209`) e recusa antes de aplicar (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`); essa proteção não envolve o command dedup (`app/chamados/management/commands/dedup_perguntas_fato.py:35`).

E — limites dos testes da chave: [nao verificado]

- Os testes dedup exercitam escrita real e conferem vias persistidas; o MORDE remove o índice para montar duas irmãs (`app/chamados/tests/test_e1_v5_chave_familia.py:73`). Não são vazios para distinguir episódio e dia (`app/chamados/tests/test_e1_v5_chave_familia.py:67`).
- O RED de migração dos episódios distintos só verifica ausência de duplicata; uma migração inteiramente recusada também satisfaz essas asserções, pois não verifica destino ou `out['migradas']` (`app/chamados/tests/test_e1_v5_chave_familia.py:82`).
- O RED de recusa usa mock da porta e cobre apenas a reação do chamador ao retorno False (`app/chamados/tests/test_e1_v5_chave_familia.py:108`).
- A prova I1/I2 testa inserções cruas, sem exercitar `abrir`, `fato_livre_para`, reapontar ou migrar, onde a instância é perdida (`app/chamados/tests/test_e1_v5_indice.py:121`).
- Execução ou resultado dos testes [nao verificado]. [nao verificado]

## 07/11 10:34 · sessao edabd9a5f904 · - · - · origem=Ronald (tmux)

B — **NAO PROCEDE no caminho sequencial corrigido de reapontar**: `resolvedora_valida` e `_costura_nao_mata_o_fio` aceitam antes da retratação; recusa retorna sem retratar. A retratação e o save da substituta pertencem ao mesmo `atomic`. (`app/chamados/services/fio.py:844`, `:857`, `:860`, `:867`, `:871`.)

B — **PROCEDENTE quanto a caminho concorrente por canônica desatualizada**, inferência estática; ocorrência em produção [nao verificado]:

- `migrar_resolvedoras_vivas` captura `vivas_destino`, consulta o fato e escolhe canônica sem travar o conjunto; quando a canônica já está no destino, pula o reapontamento e retrata a fonte diretamente. (`app/chamados/services/fio.py:1095`, `:1107`, `:1112`, `:1113`, `:1122`.)
- O chamador real `reabrir` também não abre `atomic` ao redor desse ramo. (`app/chamados/models.py:1338`, `:1387`, `:1390`.)
- Entre a consulta de identidade e a retratação, a canônica pode sair por `remover_motivo`; essa porta retrata perguntas antes de qualquer trava de colaborador. A fonte depois pode virar duplicata de uma linha que já saiu do fato. (`app/chamados/models.py:2000`, `:2001`, `:2008`; `app/chamados/services/fio.py:1122`.)
- O backfill também relê por PK, mas não reaplica o predicado do fato: se a canônica do censo já saiu do fato, ainda pode ser escolhida. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:127`, `:128`, `:131`.)
- O dedup lê grupo e escolhe canônica antes do `atomic`, sem reler dentro. (`app/chamados/management/commands/dedup_perguntas_fato.py:66`, `:68`, `:81`, `:83`.)
- `retratar_duplicata` não verifica a existência atual da canônica no fato nem igualdade das chaves. (`app/chamados/services/fio.py:989`, `:993`, `:1002`.)

Chamadores runtime de `retratar_duplicata`: `reapontar_resolvedora` (`app/chamados/services/fio.py:868`); `migrar_resolvedoras_vivas` (`app/chamados/services/fio.py:1122`); `dedup_perguntas_fato.handle` (`app/chamados/management/commands/dedup_perguntas_fato.py:83`); `backfill_decisoes_pergunta.aplicar` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:131`). Chamadas diretas em testes: `app/chamados/tests/test_e1_v5_portas_backfill.py:159`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:390`, `:398`.

E — **NAO PROCEDE acusação de vazio nos casos concretos principais**:

- Dedup MORDE remove o índice, cria duplicatas e verifica `via_resolucao` recarregada do banco. (`app/chamados/tests/test_e1_v5_chave_familia.py:73`, `:75`, `:77`; leitura em `:54`.)
- ColisaoAoDatar MORDE monta destino de outro dia real, chama a porta sem mock e exige recusa, irmã preservada e uma linha no fato. Morde a antiga retratação anterior ao E4-c. (`app/chamados/tests/test_e1_v5_portas_backfill.py:133`, `:139`, `:142`, `:146`, `:148`.)
- FatoRetratadoVolta DATADO executa emissor, remoção e emissor reais, exigindo nova PK. Cobre o ramo de intervalo sem destino; não cobre a outra consulta de dedup dos motivos que precisam de destino. (`app/chamados/tests/test_e1_volta2.py:264`, `:271`, `:274`, `:282`, `:284`; `app/chamados/services/disputa_emissao.py:393`, `:410`.)

E — **PROCEDENTE como limitação de cobertura**:

- RED de migração com episódios distintos só exige ausência de duplicatas; não exige migração efetiva nem destino final. Uma implementação sem ação satisfaria essas asserções. (`app/chamados/tests/test_e1_v5_chave_familia.py:82`, `:87`, `:88`, `:89`.)
- MORDE da migração comprova a irmã retratada, mas não confere a canônica no destino. (`app/chamados/tests/test_e1_v5_chave_familia.py:91`, `:97`, `:98`.)
- RED de recusa mocka integralmente `reapontar_resolvedora`; prova o fluxo de retorno do migrador, não a recusa real ou sua atomicidade. O teste concreto ColisaoAoDatar fornece evidência complementar dessa classe. (`app/chamados/tests/test_e1_v5_chave_familia.py:108`, `:110`, `:111`; `app/chamados/tests/test_e1_v5_portas_backfill.py:142`.)

C — leitores adicionais encontrados e encaminhados ao explorador de identidade: fábrica usa `colab+dia→motivos` e só exclui duplicata (`app/chamados/services/fabrica_da_celula.py:118`, `:119`, `:123`, `:126`); command de disparo usa `colab+motivo+dia` sem vias/conversa/instância (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`); candidata reusável usa `disputa+motivo+dia` (`app/chamados/services/declaracao_texto.py:236`); dedup do emissor usa `disputa+motivo+dia` (`app/chamados/services/disputa_emissao.py:1297`).

Não executei testes nem acessei banco/log; resultado de execução [nao verificado]. [nao verificado]

## 07/11 10:33 · sessao f3e0660912ad · - · - · origem=Ronald (tmux)

D — **PROCEDENTE**: `dedup_perguntas_fato --apply` abre `atomic` e chama `retratar_duplicata` sem travar Colaborador (`app/chamados/management/commands/dedup_perguntas_fato.py:81-83`). A chamada chega ao `UPDATE` da Pergunta por `carimbar_identidade` (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2812`); o `save` da Pergunta apenas abre outro `atomic`, sem pegar Colaborador (`app/chamados/models.py:3031-3041`).

D — **PROCEDENTE para a ordem completa**, apesar de vários caminhos agora pegarem Colaborador primeiro:

- `Disputa.fechar` no ramo administrativo carimba Pergunta antes de atualizar Disputa (`app/chamados/models.py:1877-1880`; `app/chamados/models.py:1895`).
- `fechar_o_fio_do_dia` carimba Pergunta antes de reconciliar Chamado (`app/ponto/services/veredito_celula.py:180`; `app/ponto/services/veredito_celula.py:192-193`); o reconciliar pode chegar à trava/UPDATE de Chamado (`app/chamados/models.py:687`; `app/chamados/models.py:277-285`).
- `_aceitar_a_folga` carimba Pergunta antes de chamar esse fechamento (`app/chamados/services/validacao.py:284`; `app/chamados/services/validacao.py:292`).
- `validar_pergunta` atualiza Pergunta por `_carimbar_validacao` antes de `devolver_validacao_recusada`, que pode reabrir Disputa ainda dentro do mesmo `atomic` (`app/chamados/services/validacao.py:417-422`; `app/chamados/services/validacao.py:451-453`; `app/chamados/services/validacao.py:217-218`; UPDATE da Disputa: `app/chamados/models.py:2124`).
- `desfazer_carimbo_sem_lastro` faz uma SELECT que trava **Pergunta e Disputa juntas**, por `select_for_update(of=('self', 'disputa'))`; não estabelece separadamente Disputa → Pergunta (`app/chamados/services/relavra_via.py:230-233`). A ordem efetiva de aquisição dessas duas linhas no banco é **[nao verificado]**.

D — **NÃO PROCEDE nos chamadores de produção encontrados quanto a `trava_do_colaborador` fora de transação**: o helper exige transação e avalia imediatamente a SELECT (`app/chamados/models.py:2450-2457`). Os três chamadores de `fechar_o_fio_do_dia` o invocam dentro de `atomic` (`app/ponto/services/veredito_celula.py:214`, `:337`; `app/ponto/services/declarar_dia.py:188`, `:206`; `app/chamados/services/validacao.py:281`, `:292`). Os quatro chamadores de `travar_origem_antes` também (`app/ponto/services/justificativa.py:131-132`, `:264-267`, `:333-334`; `app/ponto/views.py:1792-1793`). O helper não contém guarda explícita de `in_atomic_block`; uso externo diverso é **[nao verificado]** (`app/chamados/models.py:2456-2457`).

E — **PROCEDENTE quanto à cobertura insuficiente da ordem global**, sem vacuidade total: os testes novos da validação/desfazer exigem que Pergunta apareça na captura (`app/chamados/tests/test_e1_v5_ordem_trava.py:249`; `:259`) e possuem mutantes que removem a trava do Colaborador (`:252-255`; `:262-265`). Porém descartam deliberadamente violações da subordem entre Disputa/Chamado/Pergunta: `_sem_colab_antes` filtra somente a mensagem `"sem o Colaborador antes"` (`:230-231`), conforme a própria docstring (`:201-204`).

E — **PROCEDENTE quanto às cegueiras do capturador**: `travas()` registra somente a tabela do `FROM` para SELECT FOR UPDATE (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`; `:43-47`), ignorando a Disputa também travada pelo JOIN de `relavra_via.py:230`. Além disso, elimina repetições por **tabela**, embora a reentrância dependa da linha (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `:45-47`). O teste de fechar fio não monta Chamado (`:162-168`), e o teste de fechar disputa monta disputa vazia (`:170-174`): não cobrem os ramos acima com Pergunta → Chamado ou Pergunta → Disputa.

Execução dos testes, incidência em produção e ocorrência efetiva de deadlock: **[nao verificado]**. [nao verificado]

## 07/11 10:15 · sessao 3be797b389d4 · FIO-E0-V4-E1 · verificador · origem=Code

**Não aprovo a árvore atual:** há recusa com escrita persistida, inversão de trava e consumidores que ainda usam uma identidade diferente da declarada por família. Fontes: `app/chamados/services/fio.py:849`, `app/chamados/services/validacao.py:417`, `app/chamados/services/disputa_emissao.py:321`.

A comparação dos dois diffs ficou **[nao verificado]**: `git diff` falhou porque esta cópia não contém metadados Git. Portanto, não atribuo os achados especificamente ao delta da rodada 1. Execução da suíte, ocorrência em produção e quantidades de passivo: **[nao verificado]**. [nao verificado]

**Achados**

1. **BLOQUEIA — reapontamento recusado pode deixar outra pergunta retratada.**  
   `reapontar_resolvedora` resolve a colisão em `app/chamados/services/fio.py:849`; `_colisao_ao_datar` já retrata as outras perguntas em `app/chamados/services/fio.py:893`. Só depois são verificados o dia e o estado do destino, em `app/chamados/services/fio.py:856` e `app/chamados/services/fio.py:858`.

   **Contraexemplo:** P é antiga, sem dia e sem resposta; Q é mais nova, datada D e viva. P vence como canônica pelo menor PK (`app/chamados/services/fio.py:979`). Ofereça destino de D+7: Q vira `duplicata`, o juiz recusa o destino (`app/chamados/juizes.py:2000`), e a função restaura somente a data de P e retorna `False`, sem rollback (`app/chamados/services/fio.py:859`). O retorno normal confirma a retratação dentro do `atomic` (`app/chamados/services/fio.py:844`).

   Os selos exercitam sucesso e recusa **anterior** à retratação; falta a recusa posterior por destino inválido ou terminal. Fontes: `app/chamados/tests/test_e1_v5_portas_backfill.py:106`, `app/chamados/tests/test_e1_v5_portas_backfill.py:119`.

2. **BLOQUEIA — validação de HORA ainda trava Pergunta antes de Colaborador.**  
   O caminho começa no `atomic` de `validar_pergunta`, chama `_carimbar_validacao` e executa UPDATE da Pergunta antes de adquirir Colaborador. Fontes: `app/chamados/services/validacao.py:417`, `app/chamados/services/validacao.py:418`, `app/chamados/services/validacao.py:137`.

   Depois, no mesmo ato, segue para materialização → registro da batida → reconciliação → aquisição de Colaborador. Caminho: `app/chamados/services/validacao.py:446` → `app/chamados/services/materializacao.py:1213` → `app/chamados/services/materializacao.py:918` → `app/ponto/registro_batida.py:157` → `app/ponto/signals.py:215` → `app/chamados/reconciliador.py:1427` → `app/chamados/reconciliador.py:1450` → `app/chamados/reconciliador.py:523` → `app/chamados/models.py:686`.

   **Contraexemplo concorrente:** T1 valida HORA, segura Pergunta P e pede Colaborador C. T2 fecha o fio do dia, segura C e pede UPDATE de P. O caminho inverso está em `app/ponto/services/veredito_celula.py:175`, `app/ponto/services/veredito_celula.py:180` e `app/chamados/models.py:2812`. A ocorrência efetiva de deadlock em produção é **[nao verificado]**.

3. **BLOQUEIA pelo “SEMPRE” literal — a ordem completa também se inverte depois do Colaborador.**

   | Caminho | Contraexemplo e inversão |
   |---|---|
   | `DisputaSupervisao.fechar` | `admin_encerra` com pergunta muda viva grava Pergunta antes do UPDATE da Disputa: `app/chamados/models.py:1878`, `app/chamados/models.py:1895`. |
   | `fechar_o_fio_do_dia` | Dia com pergunta e chamado vivos grava Pergunta antes de reconciliar Chamado: `app/ponto/services/veredito_celula.py:180`, `app/ponto/services/veredito_celula.py:192`. |
   | Reapontamento com colisão | Retrata Pergunta antes de adquirir Chamado: `app/chamados/services/fio.py:894`, `app/chamados/services/fio.py:858`, `app/chamados/services/fio.py:769`. |
   | `desfazer_carimbo_sem_lastro` | Trava Pergunta e Disputa conjuntamente, sem adquirir Colaborador nessa entrada: `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`. Pode depois reabrir a Disputa: `app/chamados/services/relavra_via.py:246`. |

   Adquirir Colaborador primeiro nos três primeiros caminhos serializa as entradas conformes daquela pessoa, mas essas sequências continuam diferentes da ordem integral prometida em `app/chamados/models.py:2451`.

4. **BLOQUEIA — emissor de BATIDA ainda deduplica por disputa/motivo.**  
   `gerar_perguntas_disputa` interrompe pelo `exists()` de `(disputa,motivo)` antes de chegar à porta. GPS e foto estão nesse ramo, pois não pertencem a `MOTIVOS_DATADOS_SET`. Fontes: `app/chamados/services/disputa_emissao.py:26`, `app/chamados/services/disputa_emissao.py:321`, `app/chamados/models.py:3262`.

   **Contraexemplo:** B1 e B2, ambas sem GPS, na mesma disputa: a pergunta de B1 impede a de B2. A chave declarada admite ambos os episódios: `(batida_origem,motivo)`. A entrada de produção volta a chamar o emissor em `app/chamados/services/disputa_emissao.py:662`; o índice está em `app/chamados/models.py:3095`.

   O selo de episódios distintos cria as duas linhas diretamente, sem atravessar esse emissor; portanto, não morde essa falha. Fonte: `app/chamados/tests/test_e1_v5_indice.py:84`.

5. **BLOQUEIA — migração de resolvedoras trata episódios distintos como duplicatas.**  
   `migrar_resolvedoras_vivas` compara apenas motivo e dia; depois escolhe uma canônica e retrata a outra. Fontes: `app/chamados/services/fio.py:1043`, `app/chamados/services/fio.py:1047`, `app/chamados/services/fio.py:1049`.

   **Contraexemplo:** perguntas vivas `gps_ausente` de B1 e B2, no mesmo dia, em chamados diferentes. A reabertura redirecionada migra as resolvedoras e elimina uma cobrança legítima como duplicata. O chamador de produção está em `app/chamados/models.py:1387`; a identidade por episódio está em `app/chamados/models.py:2484`.

   O executor antigo de dedup conserva a mesma derivação por colaborador/motivo/dia, também sem episódio, conversa ou instância. Um `--apply --ate` abrangendo esses fatos pode repetir o erro. Fontes: `app/chamados/management/commands/dedup_perguntas_fato.py:57`, `app/chamados/management/commands/dedup_perguntas_fato.py:68`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`.

6. **BLOQUEIA — pergunta datada retratada ainda impede o fato que retorna.**  
   Os dedups de intervalo e de motivos com chamado-destino fazem `exists()` sem excluir `VIAS_FORA_DO_FATO`. Fontes: `app/chamados/services/disputa_emissao.py:391`, `app/chamados/services/disputa_emissao.py:407`.

   **Contraexemplo:** uma disputa conserva outro motivo; `intervalo_saida` é removido e sua pergunta é retratada. O intervalo volta a ser cobrado no mesmo dia: o emissor encontra a linha retratada e para antes da porta. A retratação ocorre em `app/chamados/models.py:2001`; o contrato “fato voltou → pergunta nova” está em `app/chamados/models.py:1993`.

   O teste existente cobre o retorno de `posto_desativado`, que passa pelo dedup não datado já corrigido; não cobre esses dois ramos datados. Fontes: `app/chamados/tests/test_e1_volta2.py:244`, `app/chamados/services/disputa_emissao.py:322`.

7. **FILA — selo de trava confunde tabela com linha.**  
   O coletor descarta toda segunda aquisição da mesma tabela e observa somente a primeira tabela do `FROM`. Fontes: `app/chamados/tests/test_e1_v5_ordem_trava.py:29`, `app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `app/chamados/tests/test_e1_v5_ordem_trava.py:45`.

   **Contraexemplo:** `Colaborador A → Chamado A → Colaborador B`. A última aquisição desaparece de `seq`, porque a tabela Colaborador já foi vista. O mesmo coletor não registra separadamente a Disputa adquirida no JOIN de `FOR UPDATE OF self,disputa`. Fontes: `app/chamados/tests/test_e1_v5_ordem_trava.py:46`, `app/chamados/services/relavra_via.py:221`.

   O caso de fechamento usa disputa vazia e deixa de exercer a inversão com perguntas vivas do achado 3. Fonte: `app/chamados/tests/test_e1_v5_ordem_trava.py:170`.

**A — campos: escritores, leitores e significado**

| Campo | Escrita encontrada | Leitura e significado |
|---|---|---|
| `batida_origem` | Nascimento por `_nascer` e `_chave_de_identidade`; deriva o ID do contexto e confere o colaborador. `app/chamados/models.py:2591`, `app/chamados/models.py:3057`, `app/chamados/models.py:3076`. | Episódio de BATIDA em `do_fato`, índice e backfill. Há divergência nos achados 4/5. `app/chamados/models.py:2484`, `app/chamados/models.py:3095`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`. |
| `data_evento` | Nascimento; reapontamento quando ausente; rechaveamento explícito. `app/chamados/models.py:2641`, `app/chamados/services/fio.py:841`, `app/chamados/services/fio.py:943`. | **Dois significados discriminados por `conversa`:** dia do turno para fato; dia civil em que foi feita para conversa. Plantio lê como dia do fato; resolvedora compara turno; lavra deriva janela. `app/chamados/models.py:2535`, `app/chamados/models.py:2546`, `app/chamados/services/materializacao.py:344`, `app/chamados/juizes.py:1965`, `app/chamados/veredito_lavrado.py:95`. |
| `instancia_marco` | Derivada no nascimento quando factual e vazia. `app/chamados/models.py:3054`. | Posição do marco, pelo mapa de quatro motivos; lida pelo domínio do fato e índice MARCO. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2508`, `app/chamados/models.py:3090`. |
| `colaborador` | Inserção exige igualdade com o colaborador da Disputa. `app/chamados/models.py:3048`. | Identidade lê a coluna; consumidores também chegam pela Disputa. O significado declarado é a mesma pessoa. `app/chamados/models.py:2509`, `app/chamados/services/materializacao.py:522`. |
| `conversa` | `_nascer`; `abrir` fixa `False`, `abrir_conversa` fixa `True`. `app/chamados/models.py:2541`, `app/chamados/models.py:2552`, `app/chamados/models.py:2641`. | Conversa fica fora do domínio do fato e dos índices. `app/chamados/models.py:2509`, `app/chamados/models.py:3091`, `app/chamados/models.py:3096`. |
| `via_resolucao` | `_escrever_carimbo`; descarimbo em lote; encerramento por fato no espelho; relavra. `app/chamados/models.py:2720`, `app/chamados/models.py:2882`, `app/chamados/models.py:2893`, `app/chamados/services/relavra_via.py:123`. | C6, filtro SQL, domínio do fato, índices e backfill. `app/chamados/juizes.py:1147`, `app/chamados/models.py:2968`, `app/chamados/models.py:2486`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`. |
| `DecisaoPergunta.ato` | `registrar`, com `bulk_create`; recebe resposta, validação, devolução, recusa/reabertura, absorção, retratação e silêncio dos respectivos escritores. `app/chamados/models.py:3550`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/validacao.py:139`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:2003`, `app/ponto/services/veredito_celula.py:185`. | Histórico, recuperação da projeção, checks e backfill distinguem o tipo do ato. `app/chamados/models.py:3113`, `app/chamados/models.py:3532`, `app/chamados/models.py:3497`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:125`. |
| `DecisaoPergunta.via_antes` | **Não é campo do modelo.** Há variável local para auditoria e via anterior embutida no texto da retratação. `app/chamados/models.py:3476`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:167`, `app/chamados/models.py:2006`. | Não há coluna estruturada `via_antes` entre os campos declarados. `app/chamados/models.py:3476`. |

A competência do backfill é `(empresa, mês, ano)`; `_exportada` passa essa mesma ordem para `competencia_entregue`. O grupo BATIDA é barrado se **qualquer membro** estiver em competência exportada. Fontes: `app/chamados/management/commands/backfill_decisoes_pergunta.py:145`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`, `app/folha/export.py:556`.

O intervalo do adaptador de competências é inclusivo `[de,ate]`; nesse backfill a consulta é de um único dia, via `competencia_do_fato`. Fontes: `app/ponto/services/fechamento.py:1567`, `app/ponto/services/fechamento.py:1595`.

**A — funções solicitadas**

| Função | Quem chama / o que lê e escreve |
|---|---|
| `_colisao_ao_datar` | Chamada pelo reapontamento; lê domínio do fato, canônica e vitalidade; retrata outras linhas. `app/chamados/services/fio.py:850`, `app/chamados/services/fio.py:883`, `app/chamados/services/fio.py:894`. |
| `reapontar_resolvedora` | Consumida por modelo, reconciliador, declaração e limbo; escreve destino/data e lavra. `app/chamados/models.py:431`, `app/chamados/reconciliador.py:944`, `app/chamados/services/declaracao_texto.py:330`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/fio.py:865`. |
| `carimbar` | Reconciliação, materialização e resposta leem seu resultado; consulta C6 e grava o carimbo, com resposta/validação opcionais. `app/chamados/reconciliador.py:586`, `app/chamados/services/materializacao.py:1268`, `app/chamados/services/disputa_emissao.py:1781`, `app/chamados/models.py:2762`, `app/chamados/models.py:2775`. |
| `carimbar_identidade` | Fechamento administrativo, dedup e retratação; grava carimbo/recusa sem rejulgar C6. `app/chamados/models.py:2687`, `app/chamados/services/fio.py:997`, `app/chamados/models.py:2008`, `app/chamados/models.py:2812`. |
| `descarimbar` / lote | Volta da resolução e reabertura; limpam os três campos juntos; o lote exclui vias fora do fato no próprio UPDATE. `app/chamados/services/relavra_via.py:231`, `app/chamados/models.py:2830`, `app/chamados/models.py:2882`. |
| `encerrar_por_fato_no_espelho` | Chamado pelo executor específico; escreve somente a via, com guarda de fora do fato na instância. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/models.py:2891`, `app/chamados/models.py:2893`. |
| `fechar_por_admin` | Disputa, aceite de folga e fio do dia; delega ao carimbo de identidade. `app/chamados/models.py:1880`, `app/chamados/services/validacao.py:285`, `app/ponto/services/veredito_celula.py:180`, `app/chamados/models.py:2687`. |
| `DisputaSupervisao.fechar` | Lê respostas pendentes/vitalidade; fecha mudas, registra SILENCIO e grava fechamento. `app/chamados/models.py:1849`, `app/chamados/models.py:1859`, `app/chamados/models.py:1883`, `app/chamados/models.py:1895`. |
| `travar_origem_antes` | Aprovação, rejeição, materialização de justificativa e view; adquire Colaborador antes dos Chamados. `app/ponto/services/justificativa.py:132`, `app/ponto/services/justificativa.py:267`, `app/ponto/services/justificativa.py:334`, `app/ponto/views.py:1793`, `app/ponto/services/justificativa.py:111`. |
| `validacao` | `validar_pergunta` carimba e materializa; folga passa pelo declarador. A entrada HORA conserva o achado 2. `app/chamados/services/validacao.py:365`, `app/chamados/services/validacao.py:313`, `app/chamados/services/validacao.py:418`. |
| `veredito_celula` | Adquire Colaborador na entrada e fecha o fio no ato do veredito. `app/ponto/services/veredito_celula.py:216`, `app/ponto/services/veredito_celula.py:337`. |
| `fechar_o_fio_do_dia` | Consumido pelo veredito e declarador; fecha perguntas, registra SILENCIO e reconcilia chamados. `app/ponto/services/veredito_celula.py:185`, `app/ponto/services/veredito_celula.py:192`, `app/ponto/services/declarar_dia.py:206`. |
| `declarar_dia` | Percorre os dias; folga adquire Colaborador e fecha fio; falta delega ao veredito. `app/ponto/services/declarar_dia.py:165`, `app/ponto/services/declarar_dia.py:190`, `app/ponto/services/declarar_dia.py:206`, `app/ponto/services/declarar_dia.py:215`. |
| `reatar_beco_folga_ao_limbo` | Executor adquire Colaborador antes do Chamado; usa a porta de reapontamento. `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:104`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:105`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`. |
| `relavra_via` | Relavra apenas vias derivadas; escreve via e lavra. Desfazer carimbo relê Pergunta/Disputa travadas e pode reabrir a Disputa. `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:246`. |
| `backfill._exportada` | Consumida na classificação dos grupos e decisão de recusar apply; delega à autoridade de exportação. `app/chamados/management/commands/backfill_decisoes_pergunta.py:235`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:255`. |

**B — escritores fora da porta e identidade vazia**

A criação produtiva encontrada converge em `_nascer`; os dois escritores de data existente estão em `fio`. O UPDATE em lote de carimbo permanece na porta, e o `bulk_update` encontrado atualiza a lavra. Fontes: `app/chamados/models.py:2641`, `app/chamados/services/fio.py:865`, `app/chamados/services/fio.py:949`, `app/chamados/models.py:2882`, `app/chamados/veredito_lavrado.py:207`. Ausência absoluta de escrita por SQL dinâmico: **[nao verificado]**.

Não encontrei conversão produtiva `conversa=True → False`; as portas fixam o valor no nascimento. **BATIDA factual pode nascer com `instancia_marco=''`**, coerentemente com seu índice por episódio; os quatro motivos MARCO recebem a posição do mapa. Fontes: `app/chamados/models.py:2541`, `app/chamados/models.py:2552`, `app/chamados/models.py:3054`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:3095`.

As guardas individuais de terminalidade leem a instância recebida, enquanto o lote filtra o estado no banco. A proteção contra instância antiga após retratação concorrente permanece **[nao verificado]**; fontes da diferença: `app/chamados/models.py:2802`, `app/chamados/models.py:2828`, `app/chamados/models.py:2891`, `app/chamados/models.py:2883`.

**D — fixtures**

Na fixture das órfãs, reaproveitar a linha emitida e restaurá-la viva preserva a prova local: ela exige uma muda encerrada, disputa barrada identificando a respondida e resposta ainda sem materialização. Fontes: `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:86`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:92`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:107`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:116`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:129`.

Os testes de acervo duplicado continuam montando duplicatas sem índice e exigindo sua reinstalação depois da cura. Fontes: `app/chamados/tests/test_e1_v5_portas_backfill.py:167`, `app/chamados/tests/test_e1_v5_portas_backfill.py:189`, `app/chamados/tests/test_e1_v5_portas_backfill.py:205`. Comparação antes/depois dos aproximadamente 30 módulos e eventual perda de outras mordidas: **[nao verificado]**.

**E — seis tetos**

A justificativa de **um SELECT FOR UPDATE por entrada** corresponde aos três pontos de aquisição; não há laço nesses trechos. Fontes: `app/chamados/models.py:285`, `app/chamados/models.py:686`, `app/chamados/services/fio.py:768`, `app/chamados/models.py:2456`.

Isso não significa uma aquisição por transição inteira: `cancelar` pode passar pelos dois helpers, e a costura de pergunta existente ocorre dentro da emissão por motivo. Fontes: `app/chamados/models.py:758`, `app/chamados/models.py:764`, `app/chamados/models.py:2611`, `app/chamados/services/disputa_emissao.py:312`.

Os tetos publicados são 20, 52, 12, 192, 208 e 8; a re-medição dos números citados nos comentários ficou **[nao verificado]**. Fontes: `app/ponto/tests/test_selo_chokepoint_escrita.py:116`, `:118`, `:119`, `:161`, `:187`, `:193`.

**F — mordida dos selos**

Há asserções úteis sobre índice real, famílias, preservação das linhas, idempotência e recusa de exportada. Fontes: `app/chamados/tests/test_e1_v5_indice.py:62`, `app/chamados/tests/test_e1_v5_familia.py:40`, `app/chamados/tests/test_e1_v5_portas_backfill.py:189`, `app/chamados/tests/test_e1_v5_portas_backfill.py:247`.

Escapam os mutantes concretos dos achados: manter o dedup antigo no emissor, comp

[truncado em 20000 de 21365 caracteres]

## 07/11 10:11 · sessao fa977c4870a5 · - · - · origem=Ronald (tmux)

Achados confirmados no código atual; pertencimento ao delta `[nao verificado]`, pois a cópia não tem `.git`.

1. **Bloqueia — emissor ainda deduplica BATIDA por disputa/motivo.** `gerar_perguntas_disputa` interrompe em `app/chamados/services/disputa_emissao.py:321-324`; `gps_ausente` não pertence aos datados (`:26`, `app/chamados/models.py:3262`). Contraexemplo: B1 e B2 no mesmo dia, ambas sem GPS, mesma disputa: a primeira pergunta impede a segunda, embora a identidade declarada permita dois episódios (`app/chamados/models.py:3095`). Entrada real: `criar_ou_atualizar_disputa` chama o emissor em `app/chamados/services/disputa_emissao.py:662`. O teste de episódios distintos cria linhas diretamente, sem atravessar esse emissor (`app/chamados/tests/test_e1_v5_indice.py:84-88`).

2. **Bloqueia — segundo dedup destrói episódio distinto.** `migrar_resolvedoras_vivas` identifica conflito apenas por motivo e dia (`app/chamados/services/fio.py:1043-1044`), escolhe canônica e retrata outra (`:1046-1050`). Contraexemplo: perguntas vivas `gps_ausente` de B1 e B2, mesmo dia, em chamados diferentes; reabrir o antigo migra suas resolvedoras e declara um episódio distinto “duplicata”. Cadeia real: `ChamadoColaborador.reabrir` → migração (`app/chamados/models.py:1387-1391`). Diverge da chave `(batida_origem,motivo)` (`app/chamados/models.py:3095`).

3. **Bloqueia — dedups datados conservam a pergunta retratada como impedimento.** Os `exists()` de intervalo (`app/chamados/services/disputa_emissao.py:391-393`) e motivos com destino (`:407-409`) não excluem `VIAS_FORA_DO_FATO`. Contraexemplo: retirar `intervalo_saida` de disputa ainda aberta retrata a pergunta (`app/chamados/models.py:2001-2009`); o fato retorna naquele dia e o emissor para antes da porta. Contraria a semântica explícita “fato voltou → pergunta nova” (`app/chamados/models.py:2347-2349`). O dedup não datado já exclui essas vias (`app/chamados/services/disputa_emissao.py:322-324`).

4. **Bloqueia — confirmação independente do pouso antes do requisito.** P sem dia, menor PK, e Q datada viva: `canonica_do_grupo` escolhe P (`app/chamados/services/fio.py:979-981`); `_colisao_ao_datar` retrata Q (`:893-894`). Depois o destino de D+7 é recusado (`app/chamados/juizes.py:2000`), e `reapontar_resolvedora` retorna normalmente, restaurando apenas o dia em memória de P (`app/chamados/services/fio.py:859-862`). O `atomic` aberto em `:844` confirma Q como duplicata mesmo sem completar a costura.

Mapa de identidade e escrita:

| Campo | Escritor | Leitor/semântica |
|---|---|---|
| `batida_origem` | Nascimento: `app/chamados/models.py:3057-3058`; derivação pelo ID do contexto, validado contra colaborador em `:3072-3076`; porta em `:2591-2594` | `do_fato` por episódio em `:2484`; índice em `:3095`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`. Emissor e migração divergem nos achados 1/2. |
| `instancia_marco` | Apenas nascimento em `app/chamados/models.py:3054-3056` | Quatro posições em `app/chamados/services/declaracao_texto.py:42-57`; leitura em `app/chamados/models.py:2508`; índice em `:3090`. BATIDA factual nasce normalmente com `''`; os quatro motivos MARCO recebem posição. |
| `conversa` | Nascimento em `app/chamados/models.py:2641-2642`; portas fixam False em `:2541` ou True em `:2552` | Fato exclui conversa em `:2509`, índices em `:3091/:3096`. Não encontrei escritor de produção que converta True→False `[nao verificado]` quanto a SQL dinâmico fora da análise estática. |
| `colaborador` | Inserção em `app/chamados/models.py:3048-3053`, exigindo igualdade com disputa | Identidade em `:2509`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`; consumidores também usam colaborador da disputa, por exemplo `app/chamados/services/materializacao.py:522`. |
| `data_evento` | Nascimento em `app/chamados/models.py:2641`; reapontar em `app/chamados/services/fio.py:841/:865`; rechavear em `:943/:949` | Dia do turno para fato (`app/chamados/models.py:2535-2541`), dia civil para conversa (`:2546-2552`). Plantio lê em `app/chamados/services/materializacao.py:344-349`; juiz de destino em `app/chamados/juizes.py:1965`; janela inclusive em `:589`. |
| `via_resolucao` | `_escrever_carimbo` em `app/chamados/models.py:2720-2724`; lote em `:2882-2884`; encerramento em `:2893-2894`; relavra em `app/chamados/services/relavra_via.py:123-125` | C6 em `app/chamados/juizes.py:1147/:1173`; SQL em `app/chamados/models.py:2968-2970`; exclusão da identidade em `:2486/:2509`; índices em `:3092/:3097`. |
| `DecisaoPergunta.ato` | `registrar`, único `bulk_create`, em `app/chamados/models.py:3548-3550` | Histórico em `:3113-3119`; recuperação da projeção em `:3532-3544`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:125/:180`. |
| `DecisaoPergunta.via_antes` | **Campo inexistente** no modelo (`app/chamados/models.py:3476-3489`) | Há variável de auditoria no backfill (`app/chamados/management/commands/backfill_decisoes_pergunta.py:167/:172`) e via anterior embutida em `ato_motivo` da retratação (`app/chamados/models.py:2006-2007`). |

Não encontrei criação de produção de `PerguntaDisputa` fora de `_nascer` (`app/chamados/models.py:2641`). Os `bulk_update` de perguntas encontrados atualizam a lavra (`app/chamados/veredito_lavrado.py:207`), e o lote que muda via passa por `descarimbar_em_lote` (`app/chamados/models.py:2882-2884`). Ausência absoluta de escritor dinâmico `[nao verificado]`.

Risco adicional a confirmar: guardas `fora_do_fato()` leem a instância recebida, sem recarga sob trava (`app/chamados/models.py:2743/:2802/:2828/:2891`). Uma instância carregada antes de retratação concorrente pode sobrepor a via terminal; o interleaving completo de produção `[nao verificado]`.

## 07/11 10:11 · sessao d454790e366b · - · - · origem=Ronald (tmux)

**bloqueia — validação HORA ainda prende Pergunta antes de Colaborador.**  
`validar_pergunta` abre transação e chama `_carimbar_validacao` antes de qualquer trava do colaborador (`app/chamados/services/validacao.py:417-418`); o primeiro ato é UPDATE da pergunta (`app/chamados/services/validacao.py:137`). Depois materializa a resposta (`app/chamados/services/validacao.py:446` → `app/chamados/services/materializacao.py:1211-1213` → `app/chamados/services/materializacao.py:918`), cuja batida aciona reconciliação (`app/ponto/signals.py:207-215`). Um destino cuja premissa morreu chega a `_retratar` (`app/chamados/reconciliador.py:1450`, `:522-523`) e trava Colaborador (`app/chamados/models.py:684-687`).

Contraexemplo: T1 valida HORA, segura Pergunta P e depois pede Colaborador C; T2 executa `fechar_o_fio_do_dia`, segura C (`app/ponto/services/veredito_celula.py:175`) e pede UPDATE de P (`:180` → `app/chamados/models.py:2812`). Há ciclo estático; ocorrência real em produção `[nao verificado]`.

**bloqueia — colisão retrata antes de verificar se o reapontamento pode acontecer.**  
`reapontar_resolvedora` chama `_colisao_ao_datar` (`app/chamados/services/fio.py:849-850`), que retrata outras perguntas (`:893-894`), antes de conferir a resolvedora e o estado do destino (`:856-858`). A recusa apenas restaura `p.data_evento` e retorna False (`:859-862`); o atomic não recebe rollback (`:844`).

Contraexemplo: P antiga, sem dia, canônica; Q mais nova e viva, já datada D; destino com dia incompatível D+10. Q ganha `duplicata`; o juiz recusa o destino; P continua sem dia e Q permanece retratada. A recusa produz escrita persistente e deixa o fato D sem sua linha anterior (`app/chamados/services/fio.py:883-894`, `:859-862`).

**bloqueia pelo pedido literal — a ordem completa ainda se inverte após Colaborador.**

- `Disputa.fechar(admin_encerra)` grava perguntas (`app/chamados/models.py:1877-1880`) antes do UPDATE da Disputa (`:1895`); seu signal depois reconcilia o Chamado-pai (`app/chamados/signals.py:41-42`). Contraexemplo: disputa com uma pergunta muda viva.
- `fechar_o_fio_do_dia` grava perguntas (`app/ponto/services/veredito_celula.py:180`) antes de reconciliar chamados (`:192-193`). Contraexemplo: dia com pergunta e chamado vivos.
- Colisão ao datar grava Pergunta (`app/chamados/services/fio.py:893-894` → `app/chamados/models.py:2812`) antes de travar Chamado (`app/chamados/services/fio.py:858`, `:769`).
- `desfazer_carimbo_sem_lastro` trava Pergunta e Disputa conjuntamente sem Colaborador (`app/chamados/services/relavra_via.py:217-224`), descarimba (`:231`) e pode atualizar Disputa (`:246` → `app/chamados/models.py:2124`).

O Colaborador previamente adquirido nos três primeiros caminhos serializa entradas conformes da mesma pessoa, mas as sequências não cumprem a ordem literal solicitada (`app/chamados/models.py:2451-2455`).

**fila — selo de trava não cobre os contraexemplos e confunde tabela com linha.**  
O teste de fechar disputa usa disputa vazia (`app/chamados/tests/test_e1_v5_ordem_trava.py:170-174`); não exerce `admin_encerra` com pergunta viva. Os demais casos cobrem analisar, nascimento, reapontamento simples, rechaveamento, fio e justificativa (`:88-119`, `:162-185`), deixando a validação HORA acima fora do selo.

O coletor elimina repetições por **tabela** (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45-47`). Mutante não detectado: após Colaborador A → Chamado A, adquirir Colaborador B; a tabela Colaborador já consta de `vistas`. Também registra somente o primeiro FROM do SELECT (`:29`, `:44`), deixando invisível a trava adicional da Disputa num `FOR UPDATE OF self,disputa` (`app/chamados/services/relavra_via.py:221-222`).

**Mapa solicitado, recorte de funções/travas.**

| Função | Leitura/escrita e consumidores |
|---|---|
| `reapontar_resolvedora` | Lê identidade/destino; escreve `data_evento`, `chamado_destino` e lavra (`app/chamados/services/fio.py:839-866`). Consumidores: modelo (`app/chamados/models.py:431`), reconciliador (`app/chamados/reconciliador.py:944`, `:1628`, `:1641`), declaração (`app/chamados/services/declaracao_texto.py:330`), limbo (`app/chamados/services/limbo_folgas.py:243`, `:276`). |
| `carimbar` | Escreve trio do carimbo e resposta/validação opcionais; consulta C6 antes do save (`app/chamados/models.py:2751-2775`). |
| `carimbar_identidade` | Escreve trio e recusa opcionais, sem C6; bloqueia retorno de linha fora do fato (`app/chamados/models.py:2800-2812`). |
| `descarimbar` / lote | Escrevem trio vazio; recusam/excluem linhas fora do fato (`app/chamados/models.py:2825-2834`, `:2874-2884`). |
| `fechar_por_admin` | Delega ao carimbo de identidade (`app/chamados/models.py:2673-2687`); chamado pelo fechamento de disputa (`:1880`), validação de folga (`app/chamados/services/validacao.py:285`, `:321`) e fio do dia (`app/ponto/services/veredito_celula.py:180`). |
| `encerrar_por_fato_no_espelho` | Porta em `app/chamados/models.py:2886`; gatilho em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`. |
| `travar_origem_antes` | Colaborador antes dos Chamados (`app/ponto/services/justificativa.py:111`, `:120`); chamadas nas aprovações/rejeição/materialização (`:132`, `:267`, `:334`) e view (`app/ponto/views.py:1793`). |
| `veredito_celula` | Atomic e Colaborador na entrada (`app/ponto/services/veredito_celula.py:214-216`); fecha fio no mesmo ato (`:337`). |
| `declarar_dia` | Percorre dias (`app/ponto/services/declarar_dia.py:165-168`); folga trava Colaborador (`:188-190`) e fecha fio (`:206`); falta delega ao veredito (`:215`). |
| `reatar_beco_folga_ao_limbo` | Colaborador antes do pai Chamado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99-105`); reversão usa reapontamento (`:127`). |
| `relavra_via` | `relavrar` escreve via e lavra diretamente (`app/chamados/services/relavra_via.py:123-126`), dentro do escritor Q5 declarado (`app/docs/DOSSIES.md:46`); `desfazer_carimbo_sem_lastro` tem a inversão descrita acima (`app/chamados/services/relavra_via.py:217-246`). |

A lei de escopo literal consta em `app/docs/LEIS.md:62`; a exigência de selo que morde, em `app/docs/LEIS.md:97`. A comparação histórica dos diffs e execução concorrente permanecem `[nao verificado]`.

## 07/11 10:11 · sessao 01179be24412 · - · - · origem=Ronald (tmux)

D/E/F — leitura estática, sem banco nem execução da suíte `[nao verificado]`; comparação dos ~30 módulos com versões anteriores indisponível porque a cópia não tem `.git` `[nao verificado]`. [nao verificado]

1. **Bloqueia — recusa tardia perde a pergunta datada; selos não mordem.** `_colisao_ao_datar` retrata outras perguntas antes da validação final da resolvedora: `app/chamados/services/fio.py:849`, `:856`, `:859`, `:893`. Contraexemplo: `p` antiga sem dia é canônica; `q` viva já ocupa o fato; destino oferecido é de outro dia. `q` vira duplicata, a resolvedora é recusada e `p` continua sem dia; o retorno normal não desfaz a retratação. Os testes exercitam sucesso e recusa anterior à retratação, sem recusa posterior: `app/chamados/tests/test_e1_v5_portas_backfill.py:106`, `:119`.

2. **Fila — selo de trava confunde tabela já vista com linha já travada.** O coletor descarta qualquer segunda trava da mesma tabela: `app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45`. Contraexemplo reproduzido usando o próprio helper, extraído por AST, sem banco: `Colaborador(pk11) → Chamado(pk22) → Colaborador(pk33)` produz `violacoes=[]`. Logo, o mutante que pega outro colaborador depois do chamado escapa; os casos usam um único colaborador: `:73`, `:151`.

3. **E — justificativa correta por entrada, limitada por chamada.** Cada entrada nos três helpers acrescenta um `SELECT FOR UPDATE`: `app/chamados/models.py:285`, `:686`, `:2456`; `app/chamados/services/fio.py:768`. Não há laço nesses trechos. Entretanto, uma transição pode entrar duas vezes (`cancelar`: `models.py:758`, `:764`), e costura ocorre por pergunta existente (`models.py:2611`), dentro da emissão por motivo (`app/chamados/services/disputa_emissao.py:312`, `:426`). Os seis números publicados nos tetos não foram re-medidos `[nao verificado]`.

4. **D — restaurar a órfã viva preserva a prova local.** A fixture reutiliza a pergunta emitida, remove os carimbos e fixa a célula acusando: `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:86`, `:92`, `:107`. As asserções exigem a muda encerrada, a disputa barrada com identificação da respondida e a respondida ainda sem materialização: `:116`, `:118`, `:120`, `:124`, `:129`. Essa montagem impede o caso de passar apenas porque a pergunta já estava morta. A guarda direta também continua cobrada: `:164`.

5. **F — mordidas reais presentes.** Corrida MARCO exige uma linha e um `IntegrityError`: `app/chamados/tests/test_e1_v5_indice.py:62`; episódios distintos e instâncias distintas são aceitos: `:84`, `:121`. Família ausente é cobrada tanto no catálogo de motivos quanto no catálogo de perguntas: `app/chamados/tests/test_e1_v5_familia.py:40`, `:71`. Backfill exige preservação das linhas, idempotência e reinstalação do índice: `app/chamados/tests/test_e1_v5_portas_backfill.py:189`; competência exportada exige saída 3 e nenhuma retratação: `:247`, `:267`.

As leis pertinentes declaram origem única, testemunha pela autoridade, medição real e escritor único: `app/docs/LEIS.md:59`, `:60`, `:61`, `:65`; o dossiê exige censo por campo e portas com prova: `app/docs/DOSSIES.md:46`, `:57`, `:61`.

## 07/11 09:14 · sessao 7c456cbb062c · FIO-E0-V4-E2 · verificador · origem=Code

1. **Bloqueia — apelido do módulo `time` escapa.** `_do_time` exige literalmente `f.value.id == 'time'`; `import time as _clock` não passa pelo tratamento de `ImportFrom`. (app/chamados/tests/test_e2_tarefa_do_dia.py:768; app/chamados/tests/test_e2_tarefa_do_dia.py:783)

   Mutante inserido **em memória** no início de `tarefa_do_dia` e, separadamente, de `esta_stale`:

   ```python
   import time as _clock
   import datetime
   agora = datetime.datetime.fromtimestamp(
       _clock.time(), datetime.timezone.utc)
   ```

   Nos dois módulos completos, o detector retornou `[]`, embora o mutante substitua o instante recebido pelo relógio corrente. Isso viola o contrato de relógio pela borda. (app/chamados/tests/test_e2_tarefa_do_dia.py:778; app/chamados/services/tarefa_do_dia.py:345; app/chamados/veredito_lavrado.py:187; app/core/juizes.py:152)

2. **Bloqueia — `None` indireto também escapa.** `_sem_instante` reconhece ausência de argumento e `ast.Constant(None)`; uma variável contendo `None` é aceita como instante. (app/chamados/tests/test_e2_tarefa_do_dia.py:762)

   Mutante executado em memória no módulo real: atribuir `instante = None` em `esta_stale` e substituir `timezone.localdate(agora)` por `timezone.localdate(instante)`. O detector retornou `[]`; nesse ramo, a conversão recebe `None` e lê o relógio, comportamento explicitamente reconhecido pelo próprio selo. (app/chamados/veredito_lavrado.py:198; app/chamados/tests/test_e2_tarefa_do_dia.py:741; app/chamados/tests/test_e2_tarefa_do_dia.py:766)

**Falsos positivos atuais: nenhum na execução pedida.** Extraí o detector por AST e executei sobre os arquivos completos: tarefa → `[]`; veredito excluindo `lavrar` → `[]`; veredito sem exclusão → `[('lavrar', 245)]`. O autoteste `test_MORDE_mutantes` também passou nessa execução isolada. (app/chamados/tests/test_e2_tarefa_do_dia.py:755; app/chamados/tests/test_e2_tarefa_do_dia.py:803; app/chamados/tests/test_e2_tarefa_do_dia.py:811; app/chamados/veredito_lavrado.py:245)

**Triagem:** as duas grafias anteriores — `now(tz)` e importação de `now` com apelido — estão cobertas, mas encerrar a cobertura do selo deixa passar os bloqueios 1 e 2. (app/chamados/tests/test_e2_tarefa_do_dia.py:778; app/chamados/tests/test_e2_tarefa_do_dia.py:785)

Não identifiquei fundamento adicional para promover os demais itens a bloqueio: a ausência de validação de `agora` naive e os dois relógios continuam presentes; a permanência dos leitores antigos está declarada no escopo da E2. (app/chamados/services/tarefa_do_dia.py:349; app/chamados/tests/test_selo_concordancia_varredura_stale.py:56; app/chamados/tests/test_selo_concordancia_varredura_stale.py:58; app/core/juizes.py:154) Ocorrência em produção e execução integrada Django: [nao verificado].

## 07/11 09:09 · sessao 9c3f52998fa8 · FIO-E0-V4-E2 · verificador · origem=Code

1. **Bloqueia — #3 cura os escapes anteriores, mas o selo ainda aceita leitura do relógio.** `_sem_instante` considera qualquer primeiro argumento diferente de `None` como instante; isso confunde **fuso** com **instante** (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

   **Caso executado em memória contra o detector real:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   O detector devolve `[]`, embora a expressão leia o relógio. Também passa `from django.utils.timezone import now as clock; clock()`: o import usa `ast.alias`, enquanto a busca cobre `Name`/`Attribute`, e `clock` não pertence à lista reconhecida (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `app/chamados/tests/test_e2_tarefa_do_dia.py:754`; `app/chamados/tests/test_e2_tarefa_do_dia.py:767`; `app/chamados/tests/test_e2_tarefa_do_dia.py:770`).

   Portanto, **não considero #3 encerrado**. Os mutantes novos mordem suas grafias declaradas, mas não sustentam a garantia do selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:739`; `app/chamados/tests/test_e2_tarefa_do_dia.py:795`).

2. **Fila — #1 e #2 estão curados nos dois predicados; igualdade e `None` não escapam.** Para os mesmos objetos, marcos e instante aware, as comparações coincidem (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:42`; `app/chamados/services/raiox.py:45`).

   | Caso, sem outro motivo de invalidação | Resultado dos dois |
   |---|---|
   | `julgado_em == agora` | Fresca: a existência é inclusiva. |
   | `julgado_em > agora`, expiração `None` | Stale: o futuro é rejeitado antes da expiração. |
   | Expiração em D+1; julgamento D 22:30 local, armazenado em UTC; consulta D+1 09h | Stale: usa a data local do julgamento. |
   | Julgamento no próprio dia da expiração | Não cai novamente pelo relógio: a comparação é `<`. |
   | Marco igual ao julgamento | Não invalida: exige `>`. |

   Esses resultados foram reproduzidos com os corpos extraídos em memória, substituindo consultas e conversão de fuso; execução integrada Django: **[nao verificado]** (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/services/raiox.py:40`).

3. **Fila — `agora` naive continua sem validação na borda.** `tarefa_do_dia` rejeita somente `None`; com julgamento aware e `agora` naive, a nova comparação levanta `TypeError`. Reproduzi esse caso no corpo isolado; não há retorno conservador de stale (`app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/veredito_lavrado.py:196`).

   A cópia configura `USE_TZ=True`; chamador real passando naive: **[nao verificado]**. Isso é uma lacuna de contrato/validação, sem evidência para promovê-la a regressão de produção (`app/config/settings/base.py:95`).

4. **Fila — o teste de concordância ainda compara instantes diferentes.** O delta alinha os marcos e o juízo do segundo caminho, mas `_confere` executa a varredura antes de capturar seu próprio `agora`; a varredura captura outro relógio internamente (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`; `app/chamados/services/raiox.py:35`).

   **Caso:** julgamento em `t`, expiração `None`, sem marcos; relógio da varredura em `t−1µs`, relógio do juízo em `t+1µs`. A primeira responde stale; o segundo, fresca. Não demonstra divergência da lei: demonstra que o selo não fixa a mesma entrada temporal (`app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — os testes funcionais novos mordem os mutantes pretendidos, com lacunas específicas.** [nao verificado]

   - **Noite:** trocar somente `localdate(p.julgado_em)` por `.date()` muda o caso isolado de stale para fresca, contrariando a expectativa do teste. Ele cobre o caminho da tarefa; não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `app/chamados/tests/test_e2_tarefa_do_dia.py:652`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:202`).
   - **Futuro na tarefa:** retirar `> agora` aceita a lavra das 22h às 21h; o contracaso das 23h impede invalidar indiscriminadamente toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `app/chamados/tests/test_e2_tarefa_do_dia.py:660`; `app/chamados/tests/test_e2_tarefa_do_dia.py:662`).
   - **Futuro na concordância:** retirar a guarda de uma implementação quebra a igualdade; retirar das duas deixa ambas vazias, mas quebra a exigência de exatamente um stale (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:61`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:99`).

   Falta caso específico de igualdade com `agora` e de julgamento noturno na varredura; os casos novos usam horários estritamente anteriores/posteriores (`app/chamados/tests/test_e2_tarefa_do_dia.py:659`; `app/chamados/tests/test_e2_tarefa_do_dia.py:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

6. **Fila — escritores e leitores dos dois campos: concordância decisória, dívida de significado preservada.**

   | Papel | Sítio |
   |---|---|
   | Produz valores novos | `lavrar`: instante corrente e expiração calculada pela data local desse instante (`app/chamados/veredito_lavrado.py:245`; `app/chamados/veredito_lavrado.py:246`). |
   | Exceção de backfill | Captura dia/instante no início e atribui os campos às linhas selecionadas (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`). |
   | Persiste/restaura | Porta de lote e restauração da foto (`app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:262`). |
   | Decide vigência | `esta_stale` e varredura, com as mesmas desigualdades (`app/chamados/veredito_lavrado.py:196`; `app/chamados/services/raiox.py:40`). |
   | Transporta/compara | Foto da lavra, comparação do backfill e foto de reversão (`app/chamados/veredito_lavrado.py:254`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/services/passivo_validado.py:83`). |

   Os saves de abertura/carimbo, troca de via e costura persistem o resultado de `lavrar`; não calculam outra expiração (`app/chamados/models.py:2572`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:124`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:903`).

   **Caso da dívida #9:** evento remoto recebe `hoje`, porque o cálculo começa em `max(hoje, data_evento)`. Isso contradiz “determinístico por data_evento”, mas não cria laço: após lavrar, data local do julgamento iguala a expiração e não satisfaz `<`. Também torna incorreto o comentário “data futura de novo” (`app/chamados/models.py:2402`; `app/chamados/veredito_lavrado.py:115`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:200`; `app/chamados/veredito_lavrado.py:202`).

7. **Fila — futuro stale não cria relavra em laço pelo mesmo motivo; leitores antigos continuam diferentes da tarefa.** O cron seleciona os stale uma vez e chama `lavrar` uma vez por alvo. A lavratura substitui o timestamp futuro pelo instante corrente; a rodada seguinte captura novo `agora` (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:35`).

   **Caso com relógio normal:** borda da tarefa em `t0`, lavratura concorrente em `t1>t0`, SELECT depois. A tarefa passa a responder `veredito_sem_lavra_vigente`, sem relavrar; ela não conserva a lavra anterior para reconstruir aquele instante (`app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:137`).

   Raiox/fio antigos leem diretamente o veredito preenchido, sem validar os dois campos. Portanto uma lavra futura `sim` ainda pode esconder pergunta materializada nesses caminhos até a manutenção; a auditoria apenas pula o stale (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:183`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`). Ocorrência e volume em produção: **[nao verificado]**.

8. **Fila — não encontrei fundamento para promover #9 ou #13 a bloqueio de produção; #3 permanece bloqueador.** #13 continua incompleto: o juiz lê `date.today()`, enquanto a expiração recebe a data local do carimbo. O efeito depende da execução e do fuso efetivo, que o dado fornecido não demonstra (`app/chamados/juizes.py:563`; `app/chamados/juizes.py:661`; `app/chamados/veredito_lavrado.py:246`). Fuso efetivo, execução da suíte Django e impacto real em produção: **[nao verificado]**.

## 07/11 09:08 · sessao 23fa3c758f64 · - · - · origem=Ronald (tmux)

1. **Bloqueia — #3 permanece incompleto: argumento de fuso é confundido com instante.** `_sem_instante` só marca chamada vazia ou primeiro argumento/keyword `value` literalmente `None`; a função chamada também é excluída da busca por nomes (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `:759`; `:760`; `:765`; `:770`).

   **Caso executado contra o helper real, extraído por AST em memória:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   Resultado do detector: `[]`. Executei também a expressão: retorna instante aware, lendo efetivamente o relógio. O argumento é fuso, não instante. Portanto o selo continua aceitando leitura banal do relógio (`app/chamados/tests/test_e2_tarefa_do_dia.py:759`; `:767`).

   Os casos `timezone.localdate(timezone=datetime.timezone.utc)` e `timezone.localtime(timezone=datetime.timezone.utc)` também retornam `[]` no detector, pela mesma omissão do `value`; execução real dessas APIs Django aqui é **[nao verificado]**, pois Django não está instalado (`app/requirements.txt:13`; detector em `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

2. **Fila — os três escapes da volta 1 foram efetivamente curados, e seus mutantes mordem.** Executei em memória o `test_MORDE_mutantes` extraído por AST, usando `unittest.TestCase`; todas as assertions passaram, incluindo `localdate(None)`, `localtime(value=None)`, alias por atribuição, helper `_hoje` e chamada no módulo. Isso confirma as grafias concretamente listadas, sem corrigir o escape do item 1 (`app/chamados/tests/test_e2_tarefa_do_dia.py:795`; `:799`; `:800`; `:801`; `:802`; `:803`; `:805`).

   **Outro escape executado:** `from django.utils.timezone import now as clock; def f(): return clock()` produz `[]`: o import usa `ast.alias`, enquanto o detector só pesquisa `ast.Name`/`ast.Attribute`, e `clock` não consta de `_RELOGIO` (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `:754`; `:770`).

3. **Fila — `test_julgado_a_noite_e_a_data_local_da_lavra` morde o mutante `.date()` no caminho da tarefa.** O teste grava julgamento local D 22:30, expiração D+1 e consulta D+1 09:00; exige ausência de lavra vigente. A carga aplica `esta_stale`, e validação sem entrada em `_lavra` produz exatamente essa razão (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `:651`; `:652`; `app/chamados/services/tarefa_do_dia.py:298`; `:135`; `:137`).

   **Caso reproduzido em memória com a função real extraída por AST**, `julgado_em` convertido para UTC e um adaptador de `localdate` com `ZoneInfo('America/Sao_Paulo')`: atual retorna `True`; substituindo somente `timezone.localdate(p.julgado_em)` por `p.julgado_em.date()`, retorna `False`. A configuração da cópia usa São Paulo e `USE_TZ=True` (`app/chamados/veredito_lavrado.py:202`; `app/config/settings/base.py:93`; `:95`). O teste não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:651`).

4. **Fila — os dois testes de futuro mordem a remoção da guarda `> agora`.** No teste da tarefa, expiração permanece `None` pela fixture, julgamento passa a D 22h, consulta D 21h exige ignorância e D 23h exige validade; isso impede atribuir a primeira queda à expiração e impede um mutante que invalide toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `:658`; `:659`; `:660`; `:661`; `:662`).

   **Caso reproduzido em memória com a função real:** às 21h, atual `True`, remoção apenas de `or p.julgado_em > agora` dá `False`; às 23h, atual `False` (`app/chamados/veredito_lavrado.py:196`).

   O teste de concordância usa julgamento uma hora futuro e expiração 30 dias futura; exige exatamente um stale. Remover a guarda de só uma implementação quebra a igualdade; remover das duas mantém igualdade vazia, mas quebra a cardinalidade exigida (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `:98`; `:99`; `:61`; `app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — igualdade com `agora` está correta no código, mas sem mordida específica nos testes novos.** Com expiração `None`, marcos `{}` e julgamento exatamente igual a `agora`, reproduzi `esta_stale=False`; alterando `>` para `>=`, reproduzi `True`. Os casos novos usam 22h contra 21h/23h e futuro de uma hora, sem igualdade (`app/chamados/veredito_lavrado.py:196`; `:198`; `:204`; `app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `:659`; `:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

Execução efetiva da suíte Django e passagem desses testes no banco: **[nao verificado]**. [nao verificado]

## 07/11 09:07 · sessao 9cfeaba4b92d · - · - · origem=Ronald (tmux)

1. **Fila — os leitores decisórios concordam para os mesmos insumos e instante aware.** `esta_stale` e a varredura usam as mesmas quatro comparações: julgamento futuro `> agora`; expiração `<= data local de agora`; julgamento local `< expiração`; marco `> julgamento`. Igualdade de julgamento com `agora` é aceita; marco igual não invalida; `janela_expira_em=None` desliga somente o motivo relógio, não a rejeição de julgamento futuro (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:42`; `app/chamados/services/raiox.py:45`).

2. **Fila — censo de quem escreve e lê os dois campos.** Escrita de valores novos: `lavrar` carimba um instante e calcula a expiração pela data local desse mesmo instante; backfill carimba o instante inicial do comando e calcula pela data local capturada no início. Restauração de foto recoloca os valores anteriores; a porta de lote persiste o trio já calculado (`app/chamados/veredito_lavrado.py:245`; `app/chamados/veredito_lavrado.py:246`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:95`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`; `app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:262`).

   Os demais escritores encontrados persistem o resultado de `lavrar`: reapontamento em `abrir`, carimbo, troca de via, costura de resolvedora e rechaveamento do dia (`app/chamados/models.py:2572`; `app/chamados/models.py:2574`; `app/chamados/models.py:2709`; `app/chamados/models.py:2711`; `app/chamados/services/relavra_via.py:124`; `app/chamados/services/relavra_via.py:125`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:851`; `app/chamados/services/fio.py:903`; `app/chamados/services/fio.py:904`).

   Leitores decisórios diretos são `esta_stale` e a varredura; leitores adicionais transportam/comparam valores: foto da lavra, contadores do backfill e foto para reversão do passivo (`app/chamados/veredito_lavrado.py:196`; `app/chamados/services/raiox.py:31`; `app/chamados/veredito_lavrado.py:254`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/management/commands/backfill_veredito_lavrado.py:135`; `app/chamados/services/passivo_validado.py:65`; `app/chamados/services/passivo_validado.py:83`).

3. **Fila — dívida #9 permanece, sem laço demonstrado.** O modelo promete expiração determinística por evento, mas `expira_em` parte de `max(hoje,data_evento)`: evento já fora da janela recebe a data de hoje. Após lavrar, `data_local(julgado_em)==janela_expira_em`; portanto a comparação estrita `<` impede repetir a invalidação só pelo relógio. O comentário que promete “data futura de novo” está errado nesse caso, mas a guarda continua funcional (`app/chamados/models.py:2402`; `app/chamados/veredito_lavrado.py:115`; `app/chamados/veredito_lavrado.py:119`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:200`; `app/chamados/veredito_lavrado.py:202`).

4. **Fila — `agora` naive continua sem contrato validado na borda.** A API pública rejeita apenas `None`; um julgamento aware comparado a `agora` naive falha em `p.julgado_em > agora`. A configuração usa `USE_TZ=True`; aceitação intencional de naive não está documentada nesses contratos (`app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/veredito_lavrado.py:196`; `app/config/settings/base.py:95`). Chamador de produção fornecendo naive: **[nao verificado]**.

5. **Fila — #13 não está curado, mas dano de produção não é demonstrado pelo dado.** O juiz usado por `lavrar` continua consultando `date.today()`, enquanto a expiração usa a data local do instante carimbado; configurações diferentes de fuso entre relógio do SO e Django podem produzir julgamentos e expirações baseados em dias distintos (`app/chamados/juizes.py:563`; `app/chamados/juizes.py:661`; `app/chamados/veredito_lavrado.py:237`; `app/chamados/veredito_lavrado.py:246`). A cópia configura `America/Sao_Paulo`; fuso efetivo do SO em produção: **[nao verificado]** (`app/config/settings/base.py:93`).

6. **Fila — rejeitar julgamento futuro não cria repetição no cron pelo mesmo instante antigo.** O cron seleciona uma vez os stale e chama `lavrar` uma vez por alvo; `lavrar` substitui o julgamento pelo relógio corrente. A seleção da varredura lê as linhas antes de capturar `agora`, portanto, com relógio normal e valores escritos por `lavrar`, o julgamento já lido precede a borda da varredura (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:31`; `app/chamados/services/raiox.py:35`). Quantidade efetiva de julgamentos futuros no acervo: **[nao verificado]**.

## 07/11 09:07 · sessao 7af3b96e1193 · - · - · origem=Ronald (tmux)

1. **Fila — o teste de concordância ainda usa dois instantes.** `_confere` executa a varredura antes de capturar seu próprio `agora`; a varredura lê o relógio internamente. Portanto, o comentário “MESMA borda” vale para marcos/juízo do segundo caminho, não entre varredura e juízo (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`; `app/chamados/services/raiox.py:35`).

   **Caso:** sem marcos e expiração `None`, fixe `julgado_em=t`; devolva `t−1µs` para o relógio da varredura e `t+1µs` para o relógio de `_confere`. A varredura devolve stale e a lei devolve fresca, embora ambos os predicados estejam corretos para seus próprios instantes (`app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`). O teste novo com futuro de uma hora não exercita essa fronteira (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

2. **Fila — a mudança não demonstra relavra em laço com relógio normal.** O cron calcula uma lista de stale, aplica teto e chama `lavrar` uma vez para cada alvo; não reavalia a mesma lista durante a rodada. `lavrar` substitui o timestamp futuro pelo `timezone.now()` corrente, e a próxima varredura captura novo `agora` (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:60`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:35`).

   **Caso:** uma linha com julgamento amanhã passa a entrar na rodada de hoje; depois da gravação, sem outro insumo posterior, deixa de ser futura e não volta por esse motivo. Comportamento efetivo de linhas futuras em produção: **[nao verificado]** (`app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:45`; `app/chamados/veredito_lavrado.py:245`).

3. **Fila — telas antigas continuam lendo o campo mesmo stale; não acionam a nova regra nem relavram.** `creditos_perguntas` usa diretamente `veredito_lavrado='sim'` no filtro SQL, e `pergunta_sem_acao_possivel` retorna o campo preenchido sem chamar `esta_stale`. A decisão de aceitar o campo até a próxima rodada está declarada nesse módulo (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:183`; `app/chamados/services/raiox.py:115`; `app/chamados/services/raiox.py:117`).

   **Caso:** uma lavra futura `sim` ainda pode esconder uma pergunta materializada nessas leituras até o cron corrigir o campo. Já a tarefa exclui essa lavra e retorna `veredito_sem_lavra_vigente`; a mudança não torna todos os leitores observadores do mesmo instante (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:184`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:135`; `app/chamados/services/tarefa_do_dia.py:137`).

4. **Fila — concorrência normal pode produzir ignorância transitória na tarefa, sem escrever.** A tarefa recebe um `agora` fixo, mas consulta as versões atuais das perguntas; se uma lavra for gravada depois desse `agora` e antes do SELECT, a nova guarda a exclui (`app/chamados/services/tarefa_do_dia.py:267`; `app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:245`).

   **Caso:** borda `t0`, cron grava `julgado_em=t1>t0`, SELECT da tarefa encontra esse campo. A tarefa responde `veredito_sem_lavra_vigente`, `nao_sei=True`, e não relavra. Isso segue a regra declarada de não aceitar lavra posterior; o código não conserva a lavra anterior para reconstruí-la nesse instante (`app/chamados/services/tarefa_do_dia.py:137`; `app/chamados/services/tarefa_do_dia.py:252`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:245`).

5. **Fila — auditoria reconhece futuro como stale, mas apenas pula essa amostra.** Não há relavra nesse ramo: o auditor usa um `agora` para marcos e juízo e executa `continue` quando `esta_stale` retorna verdadeiro (`app/chamados/management/commands/auditar_invariantes_chamados.py:1160`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`).

   **Caso:** lavra futura deixa de ser confrontada com o juiz real nessa execução, sendo classificada como stale conhecido. O cron só trata perguntas cujo `chamado_destino` pertence ao universo vivo; não se deve interpretar o comentário “o leitor re-lavra” como garantia para toda pergunta pulada pelo auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:1154`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1166`; `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:57`).

6. **Fila — a divergência de fuso do juiz antigo permanece condicional.** `_dentro_da_janela_viva` lê `date.today()`, enquanto o carimbo de expiração recebe `timezone.localdate(julgado_em)`; a cópia declara `TIME_ZONE='America/Sao_Paulo'` e `USE_TZ=True`. O fuso efetivo do processo/SO e uma divergência em produção não estão demonstrados pelo dado (`app/chamados/juizes.py:563`; `app/chamados/veredito_lavrado.py:246`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`). **[nao verificado]**

## 07/11 08:59 · sessao 9681a0007b55 · FIO-E0-V4-E1 · verificador · origem=Code

**Não daria o aval.** A cópia ainda permite reentrada de duplicatas nos índices, não resolve os fatos sem chave antes da migration e não estabelece a ordem global de travas exigida. (`app/chamados/models.py:1524`, `app/chamados/models.py:2675`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:263`, `app/ponto/services/veredito_celula.py:174`)

A cópia não contém `.git`: o diff `c2833d85..d62f5b96` e a atribuição de cada problema a esse diff ficaram **[nao verificado]**. O parecer abaixo verifica o código presente. Execução da suíte, DRY real, migration aplicada, correção efetiva de col90 e ocorrência dos cenários em produção: **[nao verificado]**.

**A. Escritores, leitores e significado**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.batida_origem` | `_nascer` repassa o episódio; `_chave_de_identidade` preenche na inserção. O derivador lê `ultima_batida.id` e confere o colaborador da batida. (`app/chamados/models.py:2579`, `app/chamados/models.py:3026`, `app/chamados/models.py:3041`) | Dedup por episódio e índice BATIDA; agrupamento e contagem de chave ausente no backfill. (`app/chamados/models.py:2467`, `app/chamados/models.py:2472`, `app/chamados/models.py:3064`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:87`) |
| `data_evento` | Nascimento; reapontamento de pergunta sem dia; rechaveamento. (`app/chamados/models.py:2629`, `app/chamados/services/fio.py:841`, `app/chamados/services/fio.py:863`, `app/chamados/services/fio.py:913`, `app/chamados/services/fio.py:919`) | Identidade MARCO, juízes, plantio, ausência, janela, reconciliação, apresentação e backfill. Referências detalhadas abaixo. (`app/chamados/models.py:2496`, `app/chamados/services/materializacao.py:344`, `app/chamados/veredito_lavrado.py:229`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:144`) |
| `instancia_marco` | `_chave_de_identidade`; aceita valor explícito não vazio. A migration histórica também preenche pelo motivo. (`app/chamados/models.py:3023`, `app/chamados/migrations/0051_decisao_pergunta.py:27`) | `linhas_dos_fatos`, índice MARCO e backfill. (`app/chamados/models.py:2496`, `app/chamados/models.py:3059`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`) |
| `colaborador` | `_chave_de_identidade`, derivado da disputa e conferido somente na inserção; preenchimento histórico pela migration. (`app/chamados/models.py:3007`, `app/chamados/models.py:3017`, `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Dedup, travas, índice e competência do backfill. Outros consumidores continuam lendo o colaborador pela disputa. (`app/chamados/models.py:2497`, `app/chamados/models.py:2509`, `app/chamados/models.py:3059`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:137`, `app/ponto/services/veredito_celula.py:175`) |
| `conversa` | `abrir=False`, `abrir_conversa=True`, persistidos em `_nascer`; acervo histórico recebe `False`. (`app/chamados/models.py:2529`, `app/chamados/models.py:2540`, `app/chamados/models.py:2629`, `app/chamados/migrations/0051_decisao_pergunta.py:98`) | Dedup, liberação de identidade, derivação da chave, índices e backfill. (`app/chamados/models.py:2472`, `app/chamados/models.py:2497`, `app/chamados/models.py:2511`, `app/chamados/models.py:3023`, `app/chamados/models.py:3060`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`) |
| `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar`, `carimbar_identidade` e `descarimbar`; `.update()` de `descarimbar_em_lote`; encerramento por fato; relavra direta. (`app/chamados/models.py:2713`, `app/chamados/models.py:2762`, `app/chamados/models.py:2797`, `app/chamados/models.py:2805`, `app/chamados/models.py:2852`, `app/chamados/models.py:2862`, `app/chamados/services/relavra_via.py:123`) | Vitalidade, lastro, identidade, materialização, fábrica, reabertura e auditorias. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1173`, `app/chamados/models.py:2498`, `app/chamados/services/materializacao.py:623`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/competencia_trancada.py:251`) |

Os consumidores localizados de `data_evento`, além do próprio nascimento/dedup, são:

- **Julgamento e materialização:** `app/chamados/juizes.py:266`, `app/chamados/juizes.py:363`, `app/chamados/juizes.py:623`, `app/chamados/juizes.py:659`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1199`, `app/chamados/juizes.py:1525`, `app/chamados/juizes.py:1965`; `app/chamados/services/materializacao.py:279`, `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:621`; `app/chamados/veredito_lavrado.py:229`.
- **Emissão, declaração, validação e reconciliação:** `app/chamados/services/disputa_emissao.py:392`, `app/chamados/services/disputa_emissao.py:408`, `app/chamados/services/disputa_emissao.py:821`, `app/chamados/services/disputa_emissao.py:963`, `app/chamados/services/disputa_emissao.py:1293`; `app/chamados/services/declaracao_texto.py:237`, `app/chamados/services/declaracao_texto.py:337`, `app/chamados/services/declaracao_texto.py:550`, `app/chamados/services/declaracao_texto.py:625`; `app/chamados/services/validacao.py:268`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:489`, `app/chamados/services/validacao.py:645`; `app/chamados/reconciliador.py:370`, `app/chamados/reconciliador.py:1040`, `app/chamados/reconciliador.py:1072`, `app/chamados/reconciliador.py:1293`, `app/chamados/reconciliador.py:1543`.
- **Serviços e apresentação:** `app/chamados/canal.py:65`; `app/chamados/services/aviso_pendencia.py:20`; `app/chamados/services/cobranca.py:83`; `app/chamados/services/competencia_trancada.py:107`; `app/chamados/services/explicador.py:70`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/historico.py:108`; `app/chamados/services/limbo_folgas.py:202`; `app/chamados/services/passivo_validado.py:113`; `app/chamados/services/pergunta_no_app.py:92`; `app/chamados/services/relavra_via.py:101`; `app/chamados/services/vigia_fio.py:816`; `app/chamados/templatetags/chamados_extras.py:31`; `app/api/views.py:1184`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176`.
- **Comandos:** `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/dedup_perguntas_fato.py:54`; `app/chamados/management/commands/disparar_perguntas_competencia.py:213`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`; `app/chamados/management/commands/auditar_invariantes_chamados.py:148`; `app/ponto/management/commands/mapa_divergencia.py:272`; `app/ponto/management/commands/vigia_plantio_overnight.py:86`.

Outros leitores de `via_resolucao` incluem a fotografia da reconciliação/materialização, relavra, explicador, cartório, flip e comandos de auditoria/dedup/reabertura. (`app/chamados/reconciliador.py:1133`, `app/chamados/services/materializacao.py:665`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/relavra_via.py:179`, `app/chamados/services/explicador.py:95`, `app/ponto/services/cartorio.py:1599`, `app/ponto/services/flip_auto.py:129`, `app/chamados/management/commands/auditar_invariantes_chamados.py:202`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`)

Para `DecisaoPergunta.ato/ato_em/ato_por`, o escritor físico é `registrar()` por `bulk_create`; os produtores dos valores são os seguintes. (`app/chamados/models.py:3517`)

| Produtor | Valores escritos |
|---|---|
| Fechamento da disputa | `SILENCIO`, carimbo da pergunta, usuário. (`app/chamados/models.py:1878`) |
| Remoção de motivo | `RETRATACAO`, instante do ato, ator. (`app/chamados/models.py:1998`) |
| Resposta humana | `RESPOSTA`, mesmo instante da projeção, usuário. (`app/chamados/services/disputa_emissao.py:1369`) |
| Reabertura/recusa | Ato recebido, instante único, `por`. (`app/chamados/services/disputa_emissao.py:1478`) |
| Respostas automáticas | `RESPOSTA`, instante recebido, autor omitido. (`app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/disputa_emissao.py:1540`) |
| Validação/devolução | `VALIDACAO` com `validada_em`; `DEVOLUCAO` com novo instante; autor recebido. (`app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:205`) |
| Absorção | `ABSORCAO`, instante compartilhado com carimbo, usuário. (`app/chamados/services/materializacao.py:560`) |
| Fechamento do fio | `SILENCIO`, `materializada_em`, autor. (`app/ponto/services/veredito_celula.py:182`) |
| Backfill | `SILENCIO`, `materializada_em`, `materializada_por_id`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:181`) |
| Projeção legada | `RESPOSTA`/`VALIDACAO`, carimbos originais; autor preservado somente para validação. (`app/chamados/models.py:3505`) |

Os leitores ordenam por `(ato_em, pk)`, identificam projeção legada por igualdade exata de `(pergunta, ato, ato_em)` e montam o histórico de respostas/fotos. O template exibe ato, instante e autor. Já o detector `admin_decidiu_sem_ato` aceita **qualquer** `SILENCIO/VALIDACAO` anterior, sem correspondência de instante. São critérios diferentes. (`app/chamados/models.py:3463`, `app/chamados/models.py:3501`, `app/chamados/models.py:3082`, `app/chamados/services/fio.py:231`, `app/templates/chamados/partials/modal_fio.html:208`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:125`)

O mapa de chamadas das funções pedidas é:

| Função | Chamadores encontrados |
|---|---|
| `do_fato` | `fato_livre_para`, `_nascer`, projeção/propostas de declaração e classificador da migração do dia. (`app/chamados/models.py:2513`, `app/chamados/models.py:2582`, `app/chamados/services/declaracao_texto.py:434`, `app/chamados/services/declaracao_texto.py:594`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:152`) |
| `fato_livre_para` | Reapontamento que data; rechaveamento. (`app/chamados/services/fio.py:849`, `app/chamados/services/fio.py:908`) |
| `_nascer` | `abrir` e `abrir_conversa`. (`app/chamados/models.py:2529`, `app/chamados/models.py:2540`) |
| `_chave_de_identidade` | `save`, somente em inserção. (`app/chamados/models.py:3007`) |
| `trava_do_colaborador` | Estado/releitura do Chamado, identidade/nascimento, três entradas de emissão, costura/reapontamento/rechaveamento e apply do backfill. (`app/chamados/models.py:285`, `app/chamados/models.py:686`, `app/chamados/models.py:2510`, `app/chamados/models.py:2555`, `app/chamados/services/disputa_emissao.py:510`, `app/chamados/services/disputa_emissao.py:948`, `app/chamados/services/disputa_emissao.py:1211`, `app/chamados/services/fio.py:768`, `app/chamados/services/fio.py:853`, `app/chamados/services/fio.py:901`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:163`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:175`) |
| `_gravar_estado` | `_aplicar`, reconciliar, cancelar, analisar, reabrir, encerrar e encerrar por competência. (`app/chamados/models.py:444`, `app/chamados/models.py:631`, `app/chamados/models.py:764`, `app/chamados/models.py:782`, `app/chamados/models.py:1408`, `app/chamados/models.py:1569`, `app/chamados/models.py:1589`) |
| `_recarregar_estado_se_mudou` | Decisão de fechamento, cancelar e os dois encerramentos. (`app/chamados/models.py:649`, `app/chamados/models.py:758`, `app/chamados/models.py:1563`, `app/chamados/models.py:1579`) |
| `reapontar_resolvedora` | Gate do Chamado; reemissão/reata; declaração; limbo; migração de resolvedoras; reversão do comando de reata. (`app/chamados/models.py:431`, `app/chamados/reconciliador.py:944`, `app/chamados/reconciliador.py:1628`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/declaracao_texto.py:330`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/services/fio.py:1023`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`) |
| `rechavear_data_evento` | Apply e reversão da migração do dia. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:284`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`) |
| `_costura_nao_mata_o_fio` | Dedup do nascimento e reapontamento. (`app/chamados/models.py:2599`, `app/chamados/services/fio.py:856`) |
| `fechar_o_fio_do_dia` | Veredito da célula, declaração de folga e aceite de folga já declarada. (`app/ponto/services/veredito_celula.py:332`, `app/ponto/services/declarar_dia.py:204`, `app/chamados/services/validacao.py:289`) |
| Backfill `handle/aplicar` | `handle` mede/classifica e chama `aplicar`; este retrata pela porta do fio e registra decisões pela porta da sequência. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:263`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:167`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:181`) |

**Não pressupõem todos o mesmo significado.** Os desvios encontrados são:

1. **BATIDA muda de unidade entre portas.** `_nascer` deduplica passando o episódio; `fato_livre_para` omite `p.batida_origem_id` e cai em motivo/dia/instância. Dois episódios distintos permitidos pelo índice no mesmo dia podem bloquear um rechaveamento legítimo. (`app/chamados/models.py:2582`, `app/chamados/models.py:2513`, `app/chamados/models.py:2496`, `app/chamados/models.py:3064`)

2. **Dois outros consumidores conservam a identidade antiga.** `migrar_resolvedoras_vivas` considera duplicação apenas por motivo/dia e pode retratar episódios diferentes. O gerador também barra outra pergunta BATIDA pelo par disputa/motivo antes de chegar ao novo dedup. (`app/chamados/services/fio.py:1013`, `app/chamados/services/fio.py:1019`, `app/chamados/services/disputa_emissao.py:26`, `app/chamados/services/disputa_emissao.py:321`)

3. **Instância explícita e instância consultada podem divergir.** A inserção aceita instância fornecida não vazia, mas `linhas_dos_fatos` consulta a instância derivada do motivo. Nenhum emissor atual passando instância alternativa foi localizado; ocorrência produtiva **[nao verificado]**. (`app/chamados/models.py:3023`, `app/chamados/models.py:2496`)

4. **`data_evento` tem dois significados declarados:** dia do turno para fato; dia civil do nascimento para conversa. O tipo é `NewType`, sem conversão temporal. Leitores como a lavra da janela não discriminam `conversa`. (`app/chamados/models.py:2523`, `app/chamados/models.py:2534`, `app/chamados/models.py:2540`, `app/core/dia_do_turno.py:13`, `app/chamados/veredito_lavrado.py:229`)

5. **As bordas temporais localizadas:** a identidade compara datas exatas; a janela viva inclui ambas as pontas; a expiração ocorre quando `janela_expira_em <= hoje`. A atribuição da batida ao dia delega ao juiz do turno, inclusive para o noturno. (`app/chamados/models.py:2496`, `app/chamados/juizes.py:589`, `app/chamados/veredito_lavrado.py:116`, `app/chamados/veredito_lavrado.py:182`, `app/ponto/turnos.py:2917`)

6. **`SILENCIO` também recebe pergunta respondida.** O fechamento do fio percorre `q_vivas`, que inclui resposta não vazia, e grava `SILENCIO`. No fechamento da disputa, resposta sem veredito é recusada antes desse ato. O mesmo enum cobre situações diferentes. (`app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:182`, `app/chamados/models.py:2935`, `app/chamados/models.py:1848`, `app/chamados/models.py:3439`)

A completude absoluta diante de SQL construído dinamicamente e aliases não atribuíveis ficou **[nao verificado]**.

**B. Há outros caminhos que podem violar os índices**

- **Ressurreição de duplicata/retratada — bloqueador.** O universo seleciona materializadas sem texto, sem excluir `duplicata/motivo_retratado`; limpa a via numa cópia para consultar o C6. Havendo substituta no fato e célula que ainda acusa, a antiga pode ser escolhida. O `.update(via_resolucao='')` reintroduz essa linha no índice, sem consultar identidade. Caminhos: reabrir, cartório e desarquivar. (`app/chamados/models.py:1449`, `app/chamados/models.py:1470`, `app/chamados/models.py:1473`, `app/chamados/models.py:1524`, `app/chamados/models.py:2852`, `app/chamados/models.py:1417`, `app/ponto/services/cartorio.py:1622`, `app/chamados/views_arquivar.py:86`)

- **Fechamento administrativo de duplicata respondida — bloqueador.** A resposta pode manter a duplicata viva; `fechar_o_fio_do_dia` a seleciona e `fechar_por_admin` troca `duplicata` por `admin_decidiu`. Essa via participa do índice, enquanto a canônica continua presente. A gravação é `save(update_fields=...)`, sem `fato_livre_para` nem dedup de nascimento. Também há chamadas diretas no aceite de folga. (`app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1164`, `app/chamados/models.py:2935`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/models.py:2675`, `app/chamados/models.py:2797`, `app/chamados/models.py:3061`, `app/chamados/services/validacao.py:282`, `app/chamados/services/validacao.py:317`)

Não localizei outro nascimento produtivo fora do `cls.objects.create` de `_nascer`. O `bulk_update` encontrado em `veredito_lavrado` grava campos da lavra; a relavra direta de via restringe sua entrada a vias derivadas. Isso não elimina os dois caminhos de reentrada acima. (`app/chamados/models.py:2629`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:125`)

**C. A ordem de trava “SEMPRE” continua falsa**

| Caminho | Ordem encontrada |
|---|---|
| `DisputaSupervisao.fechar` | Atomic → UPDATE Pergunta nas exceções → UPDATE Disputa → signal → reconciliar pai → Colaborador. Mesmo sem perguntas, permanece Disputa antes do Colaborador quando o pai está vivo. (`app/chamados/models.py:1866`, `app/chamados/models.py:1875`, `app/chamados/models.py:1890`, `app/chamados/signals.py:41`, `app/chamados/models.py:649`, `app/chamados/models.py:686`) |
| Aprovar justificativa DP | Atomic → Chamado `FOR UPDATE` → encerrar origem → retratar/reconciliar → Colaborador. Materializar saída DP usa a mesma entrada. (`app/ponto/services/justificativa.py:125`, `app/ponto/services/justificativa.py:114`, `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:42`, `app/chamados/reconciliador.py:522`, `app/chamados/models.py:686`, `app/ponto/services/justificativa.py:327`, `app/ponto/services/justificativa.py:393`) |
| Validar folga já declarada | Atomic → `_carimbar_validacao` → UPDATE Pergunta → fechar fio → reconciliar Chamado → Colaborador. Nova folga também grava a validação primeiro. (`app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:137`, `app/chamados/services/validacao.py:289`, `app/ponto/services/veredito_celula.py:190`, `app/chamados/models.py:686`, `app/chamados/services/validacao.py:307`) |
| `fechar_o_fio_do_dia` | Grava Perguntas antes de reconciliar Chamados, cuja porta adquire Colaborador. A função não estabelece a trava inicial; seus chamadores incluem atomics externos. (`app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:189`, `app/chamados/models.py:686`, `app/ponto/services/veredito_celula.py:211`, `app/ponto/services/declarar_dia.py:188`) |
| Reatar beco ao limbo | Atomic → pai Chamado `FOR UPDATE` → reatar perguntas → reapontar → Colaborador. (`app/chamados/managem

[truncado em 20000 de 32847 caracteres]

## 07/11 08:56 · sessao 3deb80690dc1 · V2-I2 · sonda · origem=Code

**A volta 3 identifica o disparador da regeneração e mostra lavras posteriores a ela. Ainda não recupera a ata consumida em 29/10 nem prova qual ato mudou o previsto para 420.** [nao verificado]

1. **Ata na lavratura e ata atual**

Hoje, a sonda mostra **`minutos_previstos=420`, `tipo_dia='trabalho'`**, com quatro marcos 15/20/21/23, todos apagados; a grade reproduz 420/trabalho. [nao verificado]

Em **29/10 10:26**, está demonstrado no material fornecido **DP previsto=0**; **ata prevista=0 e `tipo_dia` histórico continuam [nao verificado]**. DP zero não permite reconstruí-los: o mapa aceita somente `trabalho`/`ausencia`, e uma linha criada por outra rubrica ou furo começa com zero e pode permanecer sem contribuição do mapa de previsto. (`app/ponto/services/dia_pago.py:167`, `app/ponto/services/dia_pago.py:279`, `app/ponto/services/dia_pago.py:288`, `app/ponto/services/dia_pago.py:387`)

O escritor é esta cadeia: **`ata_do_dia` monta os campos → `lavrar_veredito` substitui a ata diferente → `carimbar` persiste a lavra com os carimbos**. (`app/ponto/services/cartorio.py:704`, `app/ponto/services/cartorio.py:1800`, `app/ponto/portas/celula.py:1027`, `app/ponto/portas/celula.py:1060`)

**02/11 é compatível com a reescrita, mas não está provado como o instante de 0→420.** `julgada_em` pode avançar por outros campos do julgamento; a auditoria apresentada guarda veredito/via/protestos/status, sem ata anterior/posterior. (`app/ponto/portas/celula.py:1055`, `app/ponto/portas/celula.py:982`, `app/ponto/portas/celula.py:1037`)

2. **Ata velha sem rejulgar, ou julgamento com célula nova?**

O raspado registra, nesta ordem: regeneração em **23/10 13:04:42**, lavra **`cobrado→cobrado` às 13:04:43**, lavra **`furo→concorde` em 29/10 10:12:50**, DP às **10:26**, julgamento atual em **02/11**. Portanto, há evidência de lavras após a regeneração e antes do DP. [nao verificado]

Isso **contraria a explicação simples “foi julgada antes de regenerar e nunca rejulgada”**. Pela cópia, ata diferente provoca nova lavra mesmo mantendo o veredito; repetir `cobrado` não significa ausência de julgamento. (`app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1013`, `app/ponto/portas/celula.py:1027`; [nao verificado])

Mas **“julgou previsto zero com a célula nova” ainda não está demonstrado**: faltam os campos da ata dessas lavras e a grade que os produziu. Os eventos não mostram quais chaves mudaram. (`app/ponto/services/cartorio.py:704`, `app/ponto/portas/celula.py:1037`; [nao verificado])

3. **Quem disparou a regeneração de 23/10**

O ato registrado é **`executar_vinculo`, `usuario=846`, `ator=None`, no vínculo 1449**, com regeneração de 61 células entre 21/09 e 20/11 às **13:04:42.483907**; em seguida, há o registro de vínculo **ASG CAB TARD** pelo mesmo usuário. Esse é o ato correspondente ao carimbo da célula fornecido. Identidade nominal do usuário: **[nao verificado]**.

A associação tem suporte no código: `executar_vinculo` passa `request.user` como autor e esse motivo à regeneração; a porta registra o autor na auditoria do vínculo. (`app/colaboradores/services/vinculo.py:320`, `app/ponto/portas/celula.py:334`)

4. **Dono L-099 ou “relógio da ata”?**

**Nenhum dos dois está fechado como explicação causal deste +420.** A cronologia comprova julgamento atual posterior ao FM/DP; sozinha, não comprova que esse julgamento alterou o previsto. [nao verificado]

“Relógio da ata” não é uma saída de `dono_da_divergencia`: o juiz retorna **CADASTRO, BATIDA ou ESTRUTURA**, sem receber datas de FM/DP/julgamento. Faltam seus insumos efetivos para classificar este caso. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:172`)

A analogia com APPLY 11 também não fecha a causa: o trecho citado explica **realizado de dia em curso versus ata D+1**, e declara que o **previsto cobre a competência inteira**; não certifica este previsto de 27/09. (`app/ponto/management/commands/lavrar_dias_pagos.py:16`, `app/ponto/management/commands/lavrar_dias_pagos.py:23`)

**Falta exatamente:**

- **Ata e grade efetivamente consumidas em 29/10 10:26**, com previsto, tipo, presença da chave no mapa de previsto e carimbo do julgamento anterior. [nao verificado]
- **Antes/depois das atas nas lavras de 23/10, 29/10 e 02/11**, para localizar a mudança; para explicar eventual zero, a grade produzida naquele julgamento, seus insumos históricos e a versão executada. [nao verificado]
- **`horas_do_dia` e os seis indicadores efetivos do juiz**: folga indefinida, os dois de ausência de vínculo, batida fora de turno, fase deslocada e Cadastro × Realidade. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:172`)

## 07/11 08:55 · sessao 5824c396a471 · - · - · origem=Ronald (tmux)

- **Escritor da ATA:** `ata_do_dia` copia `tipo_dia` e `minutos_previstos` da grade (`app/ponto/services/cartorio.py:704–706`); o julgamento entrega essa ATA à lavra (`app/ponto/services/cartorio.py:1800–1807`); a porta substitui `cel.ata` quando diferente (`app/ponto/portas/celula.py:995`, `1027–1028`); `carimbar` salva a lavra junto de `julgada_em` e `insumos_em` (`app/ponto/services/cartorio.py:1821–1823`; `app/ponto/portas/celula.py:1060–1067`).

- **`julgada_em=02/11` não prova que previsto mudou nesse julgamento.** Esse relógio avança se a impressão, ausência, entrada/saída ou qualquer campo lavrado mudar; pode andar sem mudar `minutos_previstos`. (`app/ponto/portas/celula.py:1055–1067`)

- **A auditoria fornecida não preserva ATA anterior/posterior.** O evento guarda veredito, via, protestos e status; não guarda `ata`, `minutos_previstos`, `tipo_dia`, `impressao` ou julgamento anterior. Assim não reconstitui o previsto/tipo consumido na lavratura do DP nem identifica precisamente o ato de 0→420. (`app/ponto/portas/celula.py:982–983`, `1037–1040`)

- **Há evidência de julgamento após a regeneração**, não apenas julgamento anterior esquecido: o raspado tem `23/10 13:04:43 cobrado→cobrado` e `29/10 10:12:50 furo→concorde`, antes do DP de 10:26. [nao verificado] Na cópia, ATA diferente com o mesmo veredito provoca nova lavra; quando ATA/veredito/via/protestos são idênticos, só mudança de status emite descrição própria “status do dia…”, não “veredito cobrado→cobrado”. (`app/ponto/portas/celula.py:995`, `1013–1025`, `1027–1041`) Logo o evento repetindo `cobrado` é compatível com mudança da ATA, sem revelar quais chaves mudaram; aplicar esse detalhe ao binário de outubro exige comprovar versão então executada. [nao verificado]

- **A cópia atual força julgamento dos dias passados tocados antes do dinheiro:** regeneração chama `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:352–356`), que chama `julgar_celula(...forcar=True)` (`app/ponto/portas/celula.py:422–440`) antes de `recalcular_por_evento` (`app/ponto/portas/celula.py:445–458`). O próprio comentário documenta correção da ordem em 23/10, sem certificar horário de deploy nem execução desse fluxo para col270. (`app/ponto/portas/celula.py:411–421`)

- **O ato `lavrar_status_competencia` de 31/10 não explica mudança de previsto/tipo:** o caminho só passa status, sem ATA, e não move o veredito quando apenas status muda. (`app/ponto/services/cartorio.py:875–891`; `app/ponto/portas/celula.py:1013–1025`)

- **`carimbar_precedencia` também não explica troca de previsto/tipo:** altera exclusivamente a chave `precedencia` da ATA existente. (`app/ponto/portas/celula.py:1075–1082`)

Falta exatamente uma fotografia histórica/versionada da ATA de 33835 em **29/10 antes do DP**, incluindo `minutos_previstos`, `tipo_dia`, `julgada_em` e `impressao`; fotografias antes/depois dos julgamentos relevantes para datar 0→420; e os insumos/grade produzidos naquele julgamento, com versão de código então executada, para decidir por que um julgamento posterior à regeneração poderia ter produzido zero. [nao verificado]

## 07/11 08:55 · sessao ff601d32099a · - · - · origem=Ronald (tmux)

O “relógio da ata” **não é uma saída do juiz L-099**: `dono_da_divergencia` só retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`, usando flags e paridade; não recebe FM, DP, ata ou `julgada_em`. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`)

- O juiz exige `horas_do_dia`, `em_cadastro_x_realidade`, `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho`, `batida_fora_de_turno` e `fase_deslocada`; sua alimentação oficial está em `InsumosDoDono.dono`. (`app/ponto/management/commands/e6_oraculo.py:172`)
- `orfas=[]`, quatro marcos ausentes e realizado zero na ata atual não fornecem esse conjunto. Dono concreto L-099: **[nao verificado]**.
- Se todas as flags forem falsas e `horas_do_dia=[]`, o resultado será `ESTRUTURA`; é conclusão condicional, não classificação comprovada deste caso. (`app/ponto/management/commands/e6_oraculo.py:98`)

A ordem informada — FM/DP em 29/10 e julgamento atual em 02/11 — demonstra julgamento atual posterior ao gravado, **mas não demonstra por que o previsto era zero nem qual alteração mudou o número**. [nao verificado]

A explicação de `lavrar_dias_pagos.py:16–26` trata especificamente de **realizado de dia em voo**: motor já lê batidas do dia, ata só nasce em D+1; o mesmo texto diz que previsto cobre a competência inteira. Portanto, esse comentário sustenta a possibilidade geral de fontes com relógios diferentes; não certifica que este **previsto de 27/09** seja a mesma situação normal documentada. (`app/ponto/management/commands/lavrar_dias_pagos.py:16`, `app/ponto/management/commands/lavrar_dias_pagos.py:23`)

**Cuidado com o pressuposto ATA histórica = 0:** DP previsto zero **não prova** ata prevista zero nem `tipo_dia` histórico:

1. A grade prefere `ata.tipo_dia` e preserva `ata.minutos_previstos=0`, chamando a função somente quando previsto é `None`. (`app/escala/services/leitor_celula.py:380`)
2. O mapa de previsto só inclui `tipo_dia in ('trabalho','ausencia')`. (`app/ponto/services/dia_pago.py:387`)
3. A linha DP começa com todos os campos em zero; o mapa de previsto só acrescenta valores quando tem a chave do dia. Uma linha criada por outra rubrica/furo pode manter previsto zero mesmo se o tipo a excluiu do mapa. (`app/ponto/services/dia_pago.py:167`, `app/ponto/services/dia_pago.py:279`, `app/ponto/services/dia_pago.py:288`)

Falta, portanto, **ATA/grade efetivamente consumida em 29/10**, com previsto e tipo, e seu carimbo de julgamento; versões/intervenções até 02/11; e os insumos efetivos do juiz para fechar L-099. A sonda fornecida não apresenta esses estados históricos. [nao verificado]

## 07/11 08:55 · sessao 20b7ebf0bf10 · FIO-E0-V4-E2 · verificador · origem=Code

**REPROVADO para o conjunto pedido.** B3 e B4 foram corrigidos, mas permanecem falhas no relógio local, na aceitação de lavra futura e na cobertura do selo de relógio (`app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/tests/test_e2_tarefa_do_dia.py:507`; `app/chamados/veredito_lavrado.py:201`; `app/chamados/tests/test_e2_tarefa_do_dia.py:735`).

1. **Bloqueia — 0514(2): a correção de fuso ficou pela metade.** `esta_stale` converte `agora` para data local, mas compara a expiração com `p.julgado_em.date()`, sem conversão local; a varredura repete essa mistura (`app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:201`; `app/chamados/services/raiox.py:42`).

   **Caso reproduzido em memória:** expiração em **21/11/2026**, julgamento em **21/11 01:30 UTC = 20/11 22:30 São Paulo**, consulta em **21/11 09:00 São Paulo**, sem marcos. A função retorna `False`: mantém vigente uma lavra anterior à virada local. O novo selo testa `agora` em UTC, mas não essa virada de `julgado_em` (`app/chamados/veredito_lavrado.py:201`; `app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `app/chamados/tests/test_e2_tarefa_do_dia.py:637`).

2. **Bloqueia — 0514(2): limitar os marcos não impede ler uma lavra futura.** `esta_stale` não exige `julgado_em <= agora`; a carga aceita qualquer `sim`/`nao` que passe nessa função (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:203`; `app/chamados/services/tarefa_do_dia.py:297`).

   **Caso reproduzido em memória:** `veredito_lavrado='nao'`, `julgado_em=2036`, `agora=2026`, expiração `None`, marcos `{}` ⇒ `esta_stale=False`. A fixture geral continua chamando de “fresco” um julgamento em `D+3650`, portanto legitima esse futuro em vez de testar existência da lavra no instante consultado (`app/chamados/veredito_lavrado.py:207`; `app/chamados/tests/test_e2_tarefa_do_dia.py:127`).

3. **Bloqueia — o selo “relógio só na borda” deixa passar leituras reais.** O detector reconhece somente cinco nomes e somente chamadas sem argumentos nem keywords (`app/chamados/tests/test_e2_tarefa_do_dia.py:724`; `app/chamados/tests/test_e2_tarefa_do_dia.py:735`).

   **Casos executados contra o helper AST, em memória:** `timezone.now()` foi detectado; `timezone.localdate(None)` e `clock = timezone.now; clock()` devolveram `[]`. Também ficam fora os relógios em helpers externos às três funções examinadas. Os mutantes existentes mordem as grafias simples declaradas, mas não sustentam a garantia ampla do nome do selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:743`; `app/chamados/tests/test_e2_tarefa_do_dia.py:755`; `app/chamados/tests/test_e2_tarefa_do_dia.py:760`).

4. **Bloqueia a certificação — 0514(3), universo completo, permanece [nao verificado].** A grade percorre todas as datas de `[ini,fim]`, inclusive produzindo `sem_celula`; entretanto a carga recebe de fora os colaboradores e o horizonte. Isso não demonstra que a sombra selecionou **toda célula do horizonte** (`app/escala/services/leitor_celula.py:375`; `app/escala/services/leitor_celula.py:394`; `app/chamados/services/tarefa_do_dia.py:340`).

   **Caso que falta:** confronto entre o universo de células do horizonte e as chaves efetivamente avaliadas pela sombra. O dado fornecido não contém essa execução/certificação: **[nao verificado]**.

5. **Fila — B3 realizado no caminho alterado.** A carga recarrega o cadastro e alimenta `grade_da_celula`; `_tarefa_pura` recebe o dia projetado e a isenção, encerra `fora_vinculo` e isento sem ações, e pergunta “dia sem trabalho” ao juiz existente por meio do adaptador (`app/chamados/services/tarefa_do_dia.py:276`; `app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/tarefa_do_dia.py:174`; `app/ponto/supra_juiz.py:69`).

   **Caso:** Caso13 confronta demissão anterior, admissão posterior, isenção, demissão no próprio dia com/sem fato e vínculo vigente. Esses casos distinguem o B3 antigo e também impedem uma implementação que simplesmente silencie tudo (`app/chamados/tests/test_e2_tarefa_do_dia.py:555`; `app/chamados/tests/test_e2_tarefa_do_dia.py:562`; `app/chamados/tests/test_e2_tarefa_do_dia.py:569`; `app/chamados/tests/test_e2_tarefa_do_dia.py:576`; `app/chamados/tests/test_e2_tarefa_do_dia.py:584`; `app/chamados/tests/test_e2_tarefa_do_dia.py:591`).

6. **Fila — censo de `acesa_lavrada`: todas as formas foram contempladas, mas “acesa” não significa necessariamente batida ocorrida.**

   | Valor/caminho | Quem escreve | Quem lê e caso |
   |---|---|---|
   | `acesa` original | O cartório produz `True`, `None` ou `False`; também produz lâmpadas pelo casamento posicional e pelo ramo sem DNA (`app/ponto/services/cartorio.py:638`; `:642`; `:644`; `:677`; `:689`). | `ata_do_dia` incorpora a lista à ata (`app/ponto/services/cartorio.py:702`). |
   | Dispensa | Força `acesa=True`, `luz=None`, com motivo de dispensa (`app/ponto/services/cartorio.py:774`). | **Caso:** ausência dispensada acende sem ninguém bater; a tarefa cala a cobrança pelo `True` (`app/ponto/services/cartorio.py:752`; `app/chamados/services/tarefa_do_dia.py:211`). |
   | Marco pendente | O teto transforma `False` em `None` (`app/ponto/services/cartorio.py:881`). | A tarefa conserva ignorância e marca `nao_sei` (`app/chamados/services/tarefa_do_dia.py:214`). |
   | Forma com lâmpadas | `_celulas_da_ata` publica literalmente `l_.get('acesa')`; chave ausente também vira `None` (`app/escala/services/leitor_celula.py:137`). | `_lampadas_do_dia` transporta o valor para as instâncias (`app/chamados/services/tarefa_do_dia.py:81`; `:95`). |
   | Forma agregada | Cada célula agregada recebe `acesa_lavrada=None`, com tipo/hora desconhecidos (`app/escala/services/leitor_celula.py:171`). | Sem instâncias identificáveis, dia de trabalho resulta em `ata_sem_lampadas`, `nao_sei=True` (`app/chamados/services/tarefa_do_dia.py:185`). |

   `acesa_lavrada` é projeção em memória; o overlay de isenção pode mudar `missing` para `False` sem mudar esse valor original. A tarefa usa o gate de isento antes de cobrar, portanto essa diferença não cria cobrança (`app/escala/services/leitor_celula.py:137`; `app/escala/services/leitor_celula.py:441`; `app/escala/utils.py:858`; `app/chamados/services/tarefa_do_dia.py:174`).

7. **Fila — censo de `fora_vinculo` e isenção: significados distintos, coerentes no caminho novo.**

   - **`tipo_dia='fora_vinculo'`:** escrito pela grade a partir de admissão, demissão e fato lavrado; lido pela tarefa, declaração, materialização e fechamento. Admissão é inclusiva; antes dela o fato pode preservar o dia. Na demissão, o próprio dia exige fato; depois da demissão, fato não vence a exclusão. O fato considerado é minuto realizado ou contagem lavrada, não mera existência de uma lâmpada `True` (`app/escala/services/leitor_celula.py:385`; `:389`; `:393`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/declaracao_texto.py:112`; `app/chamados/services/materializacao.py:239`; `app/ponto/services/fechamento.py:1036`).
   - **Isenção:** a fonte é a flag cadastral; o juiz `isento_ponto` lê somente essa flag. A tela Passe Livre grava pela porta de cadastro, e a migration histórica semeou a flag. A grade publica `isento_ponto`; a carga o passa como `isento` à tarefa (`app/colaboradores/models.py:294`; `app/core/constants.py:13`; `app/colaboradores/views.py:175`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/migrations/0043_isento_ponto.py:13`; `app/escala/services/leitor_celula.py:445`; `app/chamados/services/tarefa_do_dia.py:317`).

   **Caso de significado:** `fora_vinculo` aqui trata da vida cadastral do empregado; escolher a escala do dia é outra pergunta, cuja autoridade registrada é `vinculo_do_dia`. O novo caminho não substitui essa autoridade por uma escolha própria (`app/escala/services/leitor_celula.py:389`; `app/docs/JUIZES.md:110`; `app/escala/utils.py:2194`).

8. **Fila — censo dos marcos: o limite foi aplicado a todos os campos enumerados.** São instantes de registro/alteração, agregados por colaborador; não são o dia civil nem o dia do turno da ocorrência (`app/chamados/veredito_lavrado.py:151`; `:165`; `:176`).

   | Marco | Escritor/origem | Leitor |
   |---|---|---|
   | `Batida.criado_em` | `auto_now_add`; criação pela porta de batida (`app/ponto/models.py:79`; `app/ponto/registro_batida.py:157`). | `app/chamados/veredito_lavrado.py:151` |
   | `Batida.retratada_em` | Retratação com `timezone.now()`; desfazer pode limpar (`app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`). | `app/chamados/veredito_lavrado.py:154` |
   | `CelulaDia.gerada_em` | `auto_now_add` (`app/escala/models.py:1702`). | `app/chamados/veredito_lavrado.py:167` |
   | `regenerada_em` | Porta de célula, regeneração/desfazer (`app/ponto/portas/celula.py:274`; `:792`). | `app/chamados/veredito_lavrado.py:167` |
   | `editada_em` | Edição humana pela porta (`app/ponto/portas/celula.py:601`; `:607`). | `app/chamados/veredito_lavrado.py:168` |
   | `veredito_em` | Porta, com `agora` recebido ou relógio interno (`app/ponto/portas/celula.py:1032`). | `app/chamados/veredito_lavrado.py:167` |
   | `julgada_em` | `carimbar`, com `agora` obrigatório recebido (`app/ponto/portas/celula.py:1047`; `:1060`). | `app/chamados/veredito_lavrado.py:168` |
   | `materializada_em` | `_escrever_carimbo`; a porta recebe `quando` ou lê o relógio; descarimbo também limpa (`app/chamados/models.py:2658`; `:2683`; `:2802`). | `app/chamados/veredito_lavrado.py:176` |
   | `respondida_em` | Resposta com relógio do ato; resposta automática/trilha com instante recebido; carimbar também pode preencher, e desfazer restaura (`app/chamados/services/disputa_emissao.py:1340`; `:1346`; `:1516`; `:1537`; `app/chamados/models.py:2691`; `:2722`). | `app/chamados/veredito_lavrado.py:180` |

   **Caso de fronteira:** com `ate`, cada marco entra por `<= ate`, inclusivo; só invalida uma lavra se for estritamente `> julgado_em`. Sem `ate`, entram todos os valores atuais. Isso filtra os campos atuais, sem reconstruir versões sobrescritas; não constitui uma fotografia histórica completa (`app/chamados/veredito_lavrado.py:137`; `:140`; `:205`).

9. **Fila — `janela_expira_em` continua com dois significados.** O modelo o descreve como data determinística por `data_evento`, mas `expira_em` começa em `max(hoje, data_evento)`: para evento já expirado, devolve o dia da consulta/lavra (`app/chamados/models.py:2399`; `app/chamados/veredito_lavrado.py:115`).

   **Caso reproduzido em memória:** evento **21/05/2026** ⇒ expiração **07/11/2026** quando `hoje=07/11`; o mesmo evento ⇒ **08/11/2026** quando `hoje=08/11`. Essa dívida já está declarada como `EXPIRA-EM-ANDA-COM-HOJE`, e o diff não a cura (`app/chamados/veredito_lavrado.py:119`; `app/chamados/management/commands/backfill_veredito_lavrado.py:24`).

   **Escrita/leitura:** `lavrar` atribui e salva o trio; o backfill calcula e atribui, persistindo pela porta de lote; desfazer restaura a foto. Os leitores decisórios são `esta_stale` e a varredura; o backfill também compara o campo. Outros caminhos persistem o resultado já lavrado no mesmo save (`app/chamados/veredito_lavrado.py:245`; `:247`; `:253`; `:261`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `:140`; `:143`; `app/chamados/services/raiox.py:42`; `app/chamados/models.py:2710`; `app/chamados/services/fio.py:904`).

10. **Fila — chamadores: nenhuma quebra de assinatura encontrada; duas chamadas continuam sem `ate`.**

    - `expira_em`: os chamadores executáveis fornecem `hoje`; `esta_stale`: tarefa, auditor e testes fornecem `marcos, agora`. As duas chamadas sem argumentos novos no selo são deliberadas, sob `assertRaises(TypeError)` (`app/chamados/veredito_lavrado.py:245`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`; `app/chamados/tests/test_e2_tarefa_do_dia.py:648`).
    - **Não migrado:** alias `_mdl_s2` no fio. Não quebra porque `ate=None` permanece válido; atualmente os marcos chegam a `pergunta_sem_acao_possivel`, que ignora o argumento. São cinco consultas sem efeito nessa decisão (`app/chamados/services/fio.py:248`; `:249`; `:284`; `app/chamados/services/raiox.py:174`; `:183`; `app/chamados/veredito_lavrado.py:125`).
    - **Não migrado:** teste de concordância. Não quebra a assinatura, mas compara varredura limitada a um instante com marcos sem limite e outro relógio; pode divergir se houver mudança entre as leituras (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `:58`; `:59`; `app/chamados/services/raiox.py:35`).
    - `grade_da_celula` preserva os três argumentos antigos e acrescenta alimentação opcional; os chamadores antigos não precisam migrar. Isso inclui folha, calendário, relatórios, inteligência, espelho, emissão e materialização (`app/escala/services/leitor_celula.py:350`; `app/folha/export.py:338`; `app/colaboradores/services/calendario.py:334`; `app/relatorios/furos.py:132`; `app/inteligencia/calculadores.py:42`; `app/ponto/services/espelho.py:519`; `app/chamados/services/disputa_emissao.py:83`; `app/chamados/services/materializacao.py:233`).

11. **Fila — equivalência da grade: nenhum contraexemplo válido encontrado com alimentação completa do mesmo estado; o selo não prova “todo caso”.** Os caminhos usam o mesmo mapa recortado, cadastro e cálculo; a carga fornece todos os vínculos. Múltiplos vínculos continuam submetidos ao juiz registrado, que prioriza a geradora e depois a vigência (`app/escala/services/leitor_celula.py:361`; `:364`; `:372`; `:411`; `app/ponto/turnos.py:2988`; `app/escala/alimentacao.py:269`; `:285`).

    **Casos:** célula fora da janela é descartada; sem célula retorna `sem_celula`; sem vínculo, o previsto calculado retorna zero; admissão/demissão passam pelo mesmo predicado antes do cálculo. Alimentação incompleta não tem igualdade garantida — `{}` significa que não existe célula carregada naquele dia (`app/escala/services/leitor_celula.py:364`; `:389`; `:394`; `app/escala/utils.py:2256`; `app/escala/alimentacao.py:240`).

    O selo tem **um vínculo**, células existentes e demissão; não exercita múltiplos vínculos, colaborador sem vínculo, admissão ou forma agregada. A igualdade e a ausência de consultas são verificações úteis, mas insuficientes para a universalidade solicitada (`app/escala/tests/test_grade_alimentada.py:26`; `:33`; `:44`; `:51`; `:57`).

12. **Fila — mordida dos demais selos, por inspeção.**

    | Selo | Caso que distingue o mutante | Limite |
    |---|---|---|
    | **Caso10 MORDE** | Injeta consulta real a `FechamentoMensal`; o mesmo detector usado na proibição exige encontrá-la. B4 foi curado (`app/chamados/tests/test_e2_tarefa_do_dia.py:507`; `:521`; `:533`; `:539`; `:544`). | Não depende de o fechamento existir: a consulta já basta para violar a proibição (`app/chamados/tests/test_e2_tarefa_do_dia.py:539`). |
    | **Caso13** | Razões específicas e contracasos cobrados impedem silêncio geral (`app/chamados/tests/test_e2_tarefa_do_dia.py:560`; `:582`; `:597`). | Prova comportamento; uma cópia equivalente da regra também passaria. Lote e casca compartilham implementação (`app/chamados/tests/test_e2_tarefa_do_dia.py:609`; `app/chamados/services/tarefa_do_dia.py:352`). |
    | **LavraNoRelogioDaBorda** | Fato das 22h não vence às 21h e vence às 23h; consulta UTC preserva o dia local (`app/chamados/tests/test_e2_tarefa_do_dia.py:630`; `:634`; `:641`). | Exercita apenas `CelulaDia.veredito_em` e a conversão de `agora`; não cobre o julgamento UTC do achado 1 (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `:630`). |
    | **`test_grade_alimentada`** | Compara resultado, consultas e triestado explícito (`app/escala/tests/test_grade_alimentada.py:51`; `:52`; `:60`). | Cobertura limitada aos dados do achado 11 (`app/escala/tests/test_grade_alimentada.py:20`). |

    Esses testes não passam por ausência total de exercício; execução efetiva da suíte Django nesta auditoria: **[nao verificado]**. [nao verificado]

13. **Fila — classes arquiteturais e restrição E1.** A derivação paralela de staleness permanece na varredura, expressamente declarada; ela já reproduz o erro de fuso do juiz. Há também relógio fora da borda no juiz original `_dentro_da_janela_viva`, que continua lendo `date.today()`, enquanto a expiração agora usa data local recebida (`app/chamados/services/raiox.py:17`; `:42`; `app/chamados/juizes.py:563`; `app/chamados/veredito_lavrado.py:245`).

    **Caso:** se o SO estiver em UTC, às **21/11 01:30 UTC = 20/11 22:30 São Paulo**, o juiz pode usar a janela de 21/11 e o carimbo de expiração a de 20/11. O fuso do SO em produção é **[nao verificado]**; os dois contratos diferentes estão no código (`app/chamados/juizes.py:564`; `:583`; `:661`; `app/chamados/veredito_lavrado.py:117`).

    A convivência dos cinco respondedores antigos com `tarefa_do_dia` está autorizada até E3; não constitui, por si, juiz clandestino. O caminho novo delega dia sem trabalho ao juiz registrado (`app/core/juizes.py:150`; `app/docs/JUIZES.md:73`; `:80`; `app/ponto/supra_juiz.py:69`).

    `lavrar` continua lendo o relógio, mas foi explicitamente declarado borda de escrita; sua assinatura preservada atende `models.py`. O fio sem `ate` também continua executável, com o efeito descrito no achado 10. Não há nesses dois pontos quebra nova de assinatura (`app/chamados/tests/test_e2_tarefa_do_dia.py:722`; `app/chamados/veredito_lavrado.py:227`; `:244`; `app/chamados/models.py:2709`; `app/chamados/services/fio.py:249`).

    A tarefa continua dependente dos campos e da sequência da E1; pouso e pré-requisitos efetivamente aplicados são **[nao verificado]**. Não identifiquei nova escrita fora da porta nem nova aquisição de trava no caminho alterado; o backfill persiste pela porta de lote (`app/chamados/services/tarefa_do_dia.py:280`; `:289`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/chamados/veredito_lavrado.py:223`).

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

_2700 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
