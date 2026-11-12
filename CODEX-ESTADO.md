# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 00:19** · publicado em **12/11/2026 00:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 00:19 · sessao 9443b128960e · - · - · origem=Ronald (tmux)

Os 3 FAIL de impressão são causados por duplos que declaram `tipo_dia='ausencia'` sem fornecer as chaves que c6 passou a ler (`ponto/services/cartorio.py:1237`; `ponto/tests/test_e3_nb_impressao_memoria.py:365`, `:401`; `chamados/tests/test_e3_pos_suite_sem_banco.py:139`).

- Reproduzi os 3 FAIL com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria chamados.tests.test_e3_pos_suite_sem_banco -q`: 31 testes, 3 FAIL. [nao verificado]
- Em diagnóstico exclusivamente em memória, fornecer `ausencia_tipo`/`ausencia_id` aos duplos com cobertura fez os mesmos 31 testes passarem, preservando todas as asserções. A grade real fornece essas chaves (`escala/utils.py:1228`).
- Não há frase do aval autorizando mudar o hash B5u. Inverter essas asserções seria indevido. A decisão da sessão “Cobrança e dinheiro passaram a ler a CHAVE de cobertura” sustenta atualizar as entradas dos testes; isso precisa ser registrado como adaptação do contrato, sem mudar o comportamento caracterizado (`ponto/services/cartorio.py:1227`).

`dia_sem_trabalho` é regressão de código: a exceção parcial passou a alcançar qualquer rótulo (`chamados/juizes.py:455`). Cura sugerida: falta continua dia sem trabalho; folga só entra na exceção parcial quando há chave de cobertura. Isso preserva o teste antigo com carimbo espúrio (`chamados/tests/test_emis_ausencia_parcial_cobra.py:50`) e o P1 com folga humana parcial real (`ponto/tests/test_lastro_c5_sem_banco.py:609`). Mantenha `ausencia_parcial_cobra_o_resto` lendo apenas o carimbo, pois o contrato da ata antiga exige isso (`ponto/tests/test_lastro_c5_sem_banco.py:621`).

CATRACA: o censo em memória encontrou as mesmas identidades e quatro deslocamentos no registro (`core/tests/test_medida_turno_do_par.py:247`):

| Sítio | Antes → c6 atual |
|---|---|
| `escala/utils.py::montar_grade_prevista_periodo_por_turno` | `[1480,1500,1510,1523,1534,1820]` → `[1484,1504,1514,1527,1538,1824]` |
| `ponto/services/cartorio.py::_julgar_colab_corpo` | `[1191,1344]` → `[1198,1351]` |
| `ponto/services/espelho.py::autoridade_do_periodo` | `[1029]` → `[1032]` |
| `ponto/services/fechamento.py::_so_a_data` | `[1569]` → `[1568]` |

Recalcule esses números depois das curas; a igualdade é cobrada em `core/tests/test_medida_turno_do_par.py:539`. Nenhum arquivo foi escrito por este explorer.

## 12/11 00:18 · sessao ef9964a52222 · - · - · origem=Ronald (tmux)

Mapa na c6, por leitura estática; execução com banco não realizada [não verificado].

- Origem: `faixa_de` levanta `TipoSemFaixa` em `ponto/precedencia.py:105`; `fatos_do_periodo` chama para cada ausência em `ponto/precedencia.py:409`.
- Grade: `_marcar_ausencias` consulta o período sem captura em `escala/utils.py:1205`; builder chama o overlay em `escala/utils.py:1852`; contrato dos renders chama o builder em `escala/utils.py:2341`.
- PDF: `_coletar_dados_espelho` chama a grade sem captura em `relatorios/pdf_espelho.py:475`; PDF informacional propaga em `relatorios/pdf_espelho.py:1046`; PDF mensal propaga em `relatorios/pdf_espelho.py:1093`. O traceback fornecido confirma a interrupção desse coletor.
- Tela: o primeiro caminho pelo builder tem captura geral em `ponto/services/espelho.py:569`, mas a consulta independente de A2 em `ponto/services/espelho.py:649` recaptura e levanta especificamente em `ponto/services/espelho.py:651`. Propaga por `espelho_do_colab` em `ponto/services/espelho.py:1265`, tela web em `ponto/views.py:158` e API em `api/views.py:1354`.
- Cartório: julgamento consulta o builder sem captura em `ponto/services/cartorio.py:1168`, chamado em `ponto/services/cartorio.py:990`. Outra leitura direta, `aplicar_precedencia_na_celula`, consulta A2 em `ponto/services/cartorio.py:1992`, mas seu chamador captura e registra em `ponto/services/cartorio.py:2042`.
- Folha: **não há propagação direta por essa grade**. `grade_do_fechamento` lê a ata por `grade_da_celula` em `folha/export.py:335` e `folha/export.py:338`; prontidão consome essa grade em `folha/export.py:1095`. Falha anterior na lavratura pode impedir atualização, mas impacto efetivo não medido [não verificado].
- Semana do posto: `_fato_do_dia` recaptura e levanta em `escala/services/semana_do_posto.py:178`; sua leitura ocorre em `escala/services/semana_do_posto.py:151`.

Forma **já existente** que conserva o nome sem derrubar seu leitor: vigia captura `TipoSemFaixa`, acumula os códigos e devolve o contador `tipos_ativos_sem_faixa` em `ponto/services/vigia_ausencia.py:163` e `ponto/services/vigia_ausencia.py:168`. Não encontrei representação de dia recusado nas grades/PDF; os únicos tratamentos específicos de produção aparecem nos locais acima [não verificado quanto a mecanismos indiretos].

Proposta restritiva: manter `faixa_de` recusando, manter `test_05` intacto e registrar a decisão pendente sobre representação da recusa no PDF. Usar apenas o contador como justificativa para continuar julgando o dia inventaria uma resposta que ele não fornece (`ponto/services/vigia_ausencia.py:168`).

Férias é regressão: contrato explícito exige leitura sem exceção (`ferias/situacao.py:1`, `ferias/situacao.py:22`, `ferias/situacao.py:38`); c6 introduziu `raise` em `ferias/situacao.py:96`, que viola o selo AST em `ferias/test_a3_situacao_de_ferias.py:739`. A estrutura existente `_aviso` permite preservar a recusa nomeada sem derrubar o leitor (`ferias/situacao.py:64`): capturar o erro, anexar aviso vermelho com seu texto nos caminhos de situação/agendamento, sem concluir descanso para o dia não julgado (`ferias/situacao.py:245`, `ferias/situacao.py:322`). Isso requer transportar avisos ao helper; não exige campo de modelo. Nenhum arquivo foi alterado.

## 11/11 23:53 · sessao 9bd2d7f7b5cd · FIO-E0-V4-E4B · verificador · origem=Code

A pré-verificação tem pendências: o selo aceita formas concretas de escrita fora da porta; há caminho estático para Pauta por limbo sem competência exportada; e a explicação dos tetos não fecha a aritmética do cenário com três chamados. (app/chamados/tests/e4b_c12_ast.py:247; app/chamados/services/disputa_emissao.py:1841; app/chamados/services/fecho_runtime.py:119; app/ponto/tests/test_selo_chokepoint_escrita.py:210)

Não achei no material fornecido a tabela comparativa da corrente da manhã e do cartório emp. 2/3/4 exigida pelo aval; resultado desse DIFF, ORM, concorrência e identidade Git independente permanecem **[não verificado]**. A própria entrega distingue o snapshot textual da identidade Git e declara ORM/PROD não verificados. (app/docs/FIO-E0-V4-E4B-c12.txt:3; app/docs/FIO-E0-V4-E4B-c12.txt:4)

**1. Censo**

Não achei outro escritor corrente de **fechamento/resolução** fora da porta e dos nove sítios internos nominais. O corpo nominal inclui o gravador e os gates; o selo procura chamadas adicionais fora desse corpo. Isso não prova ausência de reflexão ou SQL arbitrário. (app/chamados/tests/e4b_c12_ast.py:18; app/chamados/tests/e4b_c12_ast.py:220; app/chamados/tests/e4b_c12_ast.py:249; completude dinâmica **[não verificado]**)

Para a pergunta literal sobre **qualquer escrita de status**, existem chamadores não terminais fora da porta e da tabela S/E. `analisar` e `reabrir` chegam ao gravador, mas não constam de `GATES`; seus eventos levam a `em_analise`, portanto não são fechadores/resolvedores do aval. (app/chamados/models.py:807; app/chamados/models.py:1443; app/chamados/tests/e4b_c12_ast.py:14; app/chamados/catalogo/motor.py:112)

| Operação fora da porta | Chamadores encontrados |
|---|---|
| `analisar` | app/chamados/views.py:1513; app/ponto/services/regularizacao.py:137; app/chamados/services/disputa_emissao.py:1085, :1159, :1178, :1223, :1252, :1379; app/chamados/management/commands/geofence_registrados_para_analise.py:32 |
| `reabrir` | app/chamados/models.py:1020; app/chamados/views_arquivar.py:70; app/ponto/management/commands/reconciliar_grade.py:530; app/ponto/management/commands/detectar_ausencias.py:302; app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49; app/chamados/services/disputa_emissao.py:1851, :2011; app/chamados/services/competencia_trancada.py:266; app/chamados/services/acoes_disputa.py:270; app/chamados/services/adesao.py:93; app/chamados/management/commands/reabrir_orfas_enterradas.py:172 |

As migrations escrevem fora da porta: 0038 altera aberto/registrado; 0039 faz o backfill de estado/via. Elas estão nominalizadas no selo separado. A aplicação anterior em PROD e a afirmação de tabela vazia em instalação nova aparecem como comentário, sem prova correspondente no dado raspado. (app/chamados/migrations/0038_a3_registrado_backfill.py:10; app/chamados/migrations/0038_a3_registrado_backfill.py:15; app/chamados/migrations/0039_a3_via_backfill.py:24; app/chamados/migrations/0039_a3_via_backfill.py:33; app/chamados/tests/test_e4b_c12_selo.py:93; app/chamados/tests/test_e4b_c12_selo.py:97)

O selo principal só percorre `app/` e exclui migrations e testes. Não achei outro fechador terminal literal nas buscas de views/API, cascatas, comandos e scripts externos; censo irrestrito de tasks dinâmicas, aliases interprocedurais e SQL: **[não verificado]**. (app/chamados/tests/e4b_c12_ast.py:11; app/chamados/tests/e4b_c12_ast.py:34)

**2. Equivalência dos sítios**

O ramo `ato=` executa o callable sem lhe acrescentar argumentos e devolve seu resultado exato. Assim, os `user` externos não transformam `reconciliar()` em ato humano nem `encerrar()` deixa de usar sua via original. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:169; app/chamados/models.py:655; app/chamados/models.py:1620)

A comparação reversível em memória recuperou o AST dos arquivos anteriores, descontados os imports, contra o snapshot textual usado pelo runner; equivalência contra o commit Git real **[não verificado]**. O runner carrega essa base textual. (app/chamados/tests/e4b_c12_verificar.py:17; app/chamados/tests/e4b_c12_verificar.py:18)

Na tabela, “booleano” significa que a seleção, condição ou retorno continua usando o booleano original; “descartado” significa que o chamador não consome o retorno. A preservação desse retorno está no ramo da porta. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:169)

| Sítio | Callable/operação e uso mantidos | Código |
|---|---|---|
| S01 | `reconciliar()`, booleano | app/chamados/management/commands/auditar_chamados_orfaos.py:68 |
| S02–S04 | `transicionar(destino, usuário)`, retorno descartado; sem `ato=` | app/chamados/management/commands/smoke_chamados.py:135, :148, :164 |
| S05 | `encerrar()`, booleano | app/chamados/reconciliador.py:497 |
| S06 | `reconciliar()`, booleano | app/chamados/reconciliador.py:524 |
| S07–S08 | `reconciliar()`, booleano | app/chamados/services/adesao.py:116, :151 |
| S09 | `encerrar_por_competencia_trancada()`, booleano | app/chamados/services/competencia_trancada.py:185 |
| S10 | `reconciliar()`, booleano | app/chamados/services/feriado_reavaliar.py:48 |
| S11 | `reconciliar()`, booleano | app/chamados/services/limbo_folgas.py:102 |
| S12–S13 | `reconciliar()`, booleano; conserva rollback entre tentativas | app/chamados/services/limbo_folgas.py:284, :286, :287 |
| S14 | `reconciliar()`, descartado | app/chamados/services/materializacao.py:583 |
| S15 | `reconciliar()`, booleano devolvido | app/chamados/services/materializacao.py:772 |
| S16 | `encerrar()`, descartado | app/chamados/services/regularizacao_ext.py:206 |
| S17 | `reconciliar()`, booleano | app/chamados/services/silencio_ausencia.py:102 |
| S18 | `reconciliar()`, booleano | app/chamados/signals.py:43 |
| S19 | `reconciliar()`, booleano | app/colaboradores/management/commands/marcar_feriado_do_posto.py:141 |
| S20 | `encerrar()`, booleano combinado com status | app/colaboradores/services/desligamento.py:156 |
| S21 | `reconciliar()`, booleano | app/colaboradores/services/pos_vinculo.py:43 |
| S22 | `encerrar()`, descartado | app/colaboradores/services/vinculo.py:388 |
| S23 | `cancelar(motivo=...)`, booleano; motivo e `ch=ch` conservados | app/escala/management/commands/fechar_avisos_regua_na_lista.py:48 |
| S24 | `reconciliar()`, booleano | app/ponto/management/commands/curar_residuo_bug115.py:152 |
| S25 | `reconciliar()`, descartado | app/ponto/management/commands/detectar_ausencias.py:159 |
| S26–S28 | `reconciliar()`, booleano | app/ponto/management/commands/detectar_ausencias.py:825, :835, :846 |
| S29 | `reconciliar()`, booleano; dry continua em `reconciliaria()` | app/ponto/management/commands/detectar_intervalo_ausente.py:139 |
| S30 | `fechar(sistema, resolucao=...)`, `None` descartado; defaults capturam `ch/dia/te` | app/ponto/management/commands/fechar_chamados_meio_periodo.py:110 |
| S31 | `cancelar(motivo=...)`, booleano; motivo e `ch=ch` conservados | app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58 |
| S32 | `reconciliar()`, booleano | app/ponto/services/ausencia.py:695 |
| S33–S38 | `reconciliar()`, booleano nas seleções/condições | app/ponto/services/cartorio.py:987, :1512, :1525, :1546, :1624, :1639 |
| S39 | `reconciliar()`, booleano | app/ponto/services/cluster_espurio.py:125 |
| S40 | `reconciliar()`, atribuído a `_fechou` | app/ponto/services/cobertura.py:123 |
| S41 | `reconciliar()`, booleano | app/ponto/services/cobertura.py:318 |
| S42 | `reconciliar()`, booleano; serviço continua devolvendo chamado/`None` | app/ponto/services/entrada_sincronizada.py:32 |
| S43 | `reconciliar()`, booleano | app/ponto/services/lastro.py:401 |
| S44 | `reconciliar()`, booleano | app/ponto/services/triagem_batida.py:598 |
| S45 | `reconciliar()`, booleano | app/ponto/services/veredito_celula.py:203 |
| S46 | `transicionar('em_analise', user)`, descartado; sem `ato=` | app/chamados/services/acoes_chamado.py:238 |

E01–E09 descartam o retorno da porta; não achei consumidor desses sítios confundindo contagem de Pautas com booleano ou objeto. (app/chamados/models.py:455; app/chamados/services/acoes_chamado.py:71, :226, :283, :110, :152, :322; app/chamados/services/fio.py:722; app/colaboradores/views.py:2069)

**A equivalência de ordem não é absoluta em S30.** Antes, a porta interna de `fechar()` entregava a Pauta e depois vinha a nota; agora a reentrada interna reutiliza o coletor, a nota é escrita e o wrapper externo entrega a Pauta depois. Estado continua antes da nota, mas Pauta e nota trocam de ordem, e a nota passa a integrar o atomic externo da porta. (app/chamados/models.py:455; app/chamados/models.py:458; app/chamados/services/fecho_runtime.py:173; app/chamados/services/fecho_runtime.py:168; app/ponto/management/commands/fechar_chamados_meio_periodo.py:110)

Não achei outra troca entre escritas antigas dos sítios; equivalência operacional com ORM e concorrência: **[não verificado]**. (app/docs/FIO-E0-V4-E4B-c12.txt:10)

**3. Porta, reentrada e atomicidade**

| Caso | Comportamento do código |
|---|---|
| Reentrada imediata do mesmo chamado | Executa o gate usando o coletor atual; não abre outro atomic nem entrega outro lote. (app/chamados/services/fecho_runtime.py:158; app/chamados/services/fecho_runtime.py:173) |
| Outro chamado aninhado | Abre seu atomic e coletor; restaura o anterior antes de entregar seu lote. (app/chamados/services/fecho_runtime.py:161; app/chamados/services/fecho_runtime.py:167) |
| A→B→A | A comparação é somente com o chamado do coletor corrente; o segundo A, enquanto B está corrente, abre outro lote. Não existe mapa global de chamados ativos. (app/chamados/services/fecho_runtime.py:156; app/chamados/services/fecho_runtime.py:159) |
| Exceção que sai de `ato()` | Restaura o contexto e pula `_escrever_pautas`; a exceção sai pelo atomic da porta. (app/chamados/services/fecho_runtime.py:164; app/chamados/services/fecho_runtime.py:168) |
| Falha na entrega da Pauta | Propaga fora do S143 e sai pelo mesmo atomic do ato. (app/chamados/services/fecho_runtime.py:161; app/chamados/services/fecho_runtime.py:168) |
| Chamador captura essa falha | A captura externa ocorre depois da saída do atomic da porta; o receiver S88 é exemplo de chamador que captura. (app/chamados/signals.py:42; app/chamados/signals.py:47) |
| Chamador já em atomic | A porta pede atomic com savepoint padrão; os gates automáticos internos continuam com `savepoint=False`. (app/chamados/services/fecho_runtime.py:161; app/chamados/models.py:651; app/chamados/models.py:1613) |
| Autocommit | O atomic externo da porta delimita ato e entrega; não se pode chamar seu custo de SAVEPOINT/RELEASE em todos os contextos. (app/chamados/services/fecho_runtime.py:161; comportamento efetivo do backend **[não verificado]**) |

Há caminhos de **transição sem Pauta**:

- O S143 captura falhas do resolver. O coletor só incorpora as recusas depois de o contexto e o savepoint saírem sem erro; portanto uma falha pode preservar o fechamento e descartar a coleta. Isso é consequência explícita do best-effort existente. (app/chamados/signals.py:116; app/chamados/signals.py:118; app/chamados/services/fecho_runtime.py:35; app/chamados/services/fecho_runtime.py:40)
- Pergunta sem dia de turno não ancora Pauta; via `competencia_trancada` também sai antes do processamento normal do resolver. (app/chamados/services/fecho_runtime.py:120; app/chamados/services/fecho_runtime.py:122; app/chamados/services/disputa_emissao.py:1901)
- Sem recusa coletada, não há entrega. (app/chamados/services/fecho_runtime.py:118; app/chamados/services/fecho_runtime.py:128)

A porta também **não exige uma transição bem-sucedida para escrever**: entrega o lote independentemente de `resultado`, inclusive `False`. O teste de memória exige justamente esse contrato. Nos gates produtivos inventariados, não achei um caso comprovado de Pauta sem nenhuma transição no ato; essa garantia não é imposta pela porta. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:168; app/chamados/tests/test_e4b_c12_memoria.py:27; app/chamados/tests/test_e4b_c12_memoria.py:31)

O retorno antecipado de `_escrever_pautas` só retira o atomic vazio quando nenhuma recusa tem âncora. O atomic externo continua cobrindo o ato; não achei mudança de rollback ou reentrada causada por esse retorno. (app/chamados/services/fecho_runtime.py:128; app/chamados/services/fecho_runtime.py:130; app/chamados/services/fecho_runtime.py:161)

**4. Quem escreve, quem lê e significado**

O quadro abaixo identifica escritores e famílias de leitores encontradas; não certifico censo irrestrito de todos os leitores dinâmicos/SQL. **[não verificado]**

| Campo/valor | Escritores | Leitores identificados |
|---|---|---|
| `status_local` | Atribuição e persistência em app/chamados/models.py:228, :296; nascimento em :1096; simulação restaurada em :356, :371; migrations citadas no item 1 | Gates/tripwire em app/chamados/models.py:165, :219, :221; S143 em app/chamados/signals.py:97, :105; destino em app/chamados/juizes.py:760; fio em app/chamados/services/fio.py:389, :432; fila em app/chamados/services/na_fila.py:17; painel em app/chamados/services/painel.py:213; cartório em app/ponto/services/cartorio.py:1358, :1616; cascatas em app/colaboradores/services/desligamento.py:153, :156 e app/colaboradores/services/vinculo.py:386 |
| `status_local`, outros consumidores | Mesmo escritor acima | app/chamados/reconciliador.py:247, :840; app/ponto/management/commands/detectar_ausencias.py:153, :194, :259; app/api/views.py:320, :1044, :1972; app/api/views_core.py:309; app/chamados/views.py:976, :1477; app/colaboradores/services/calendario.py:437; app/relatorios/views.py:745, :920; app/relatorios/pdf_chamados.py:116; app/chamados/management/commands/auditar_invariantes_chamados.py:573, :1068 |
| `status_local`, apresentação | Mesmo escritor acima | app/templates/chamados/detalhe_local.html:68; app/templates/chamados/detalhe.html:36; app/templates/chamados/fio_colaborador.html:167; app/templates/chamados/partials/_lista_chamados.html:33; app/templates/chamados/partials/_card_atendimento.html:13; app/templates/chamados/partials/modal_fio.html:98; app/templates/chamados/partials/painel_chamado_turno.html:18; app/templates/relatorios/historico_chamados.html:121 |
| `via_encerramento` | app/chamados/models.py:245, :249; backfill em app/chamados/migrations/0039_a3_via_backfill.py:24 | app/chamados/models.py:715, :1008, :1343, :1376; app/chamados/views_arquivar.py:64, :165; app/ponto/services/furos_sem_cobranca.py:69; app/chamados/services/disputa_emissao.py:1874, :1901; app/chamados/services/competencia_trancada.py:194, :260, :271; app/chamados/services/adesao.py:91; app/chamados/management/commands/auditar_invariantes_chamados.py:1070; app/chamados/management/commands/reabrir_orfas_enterradas.py:135 |
| `resolvido_em` | app/chamados/models.py:244, :248 | app/chamados/models.py:243, :715, :1343, :1347, :1376; app/chamados/services/disputa_emissao.py:1861; app/chamados/services/painel.py:65, :245, :249; app/chamados/services/verbos.py:230, :237; app/chamados/services/fila.py:109; app/chamados/views_arquivar.py:166; app/chamados/management/commands/auditar_invariantes_chamados.py:574; app/chamados/management/commands/reabrir_orfas_enterradas.py:134; app/templates/relatorios/historico_chamados.html:127; app/templates/chamados/detalhe_local.html:88; app/templates/chamados/detalhe.html:56 |
| `Pauta.ancora_id/texto` | E4B monta em app/chamados/services/fecho_runtime.py:58, :120; INSERT delegado em app/pautas/services.py:180; resposta copia âncora em :199 | Dedup em app/chamados/services/fecho_runtime.py:136; lista/âncora em app/pautas/services.py:322, :336; API em app/pautas/views.py:40, :47; apresentação em app/templates/core/_copiloto.html:405, :570 e app/templates/colaboradores/detalhe.html:119; suporte em app/pautas/management/commands/suporte_hasner.py:109 |
| `_RECUSAS` | Default, `set`, `reset` e atualização do mapa em app/chamados/services/fecho_runtime.py:13, :20, :35, :39, :40, :163, :167, :179, :183 | Somente o runtime: app/chamados/services/fecho_runtime.py:18, :30, :156 |
| `PerguntaDisputa.via_resolucao/materializada_em` | Atribuição conjunta em app/chamados/models.py:3098; persistem `carimbar` em :3155, identidade em :3191, descarimbo em :3213, lote em :3265; troca isolada de via em :3276 | C6 em app/chamados/juizes.py:662, :674, :689, :1342, :1368, :1374, :1405; estado/SQL em app/chamados/models.py:3044, :3056, :3351; materializador em app/chamados/services/materializacao.py:435, :439, :626, :1090, :1373; validação em app/chamados/services/validacao.py:188, :461, :472 |
| Carimbo da pergunta, outros consumidores | Mesmas portas acima | app/chamados/services/disputa_emissao.py:1908, :2040; app/chamados/reconciliador.py:353, :367, :1144, :1495; app/chamados/veredito_lavrado.py:203; app/chamados/services/fio.py:1101, :1168, :1244, :1351, :1434, :1854; app/chamados/services/passivo_validado.py:21, :64, :140; app/chamados/services/relavra_via.py:42, :58, :66, :181, :244; app/chamados/services/fabrica_da_celula.py:149, :171; app/chamados/services/tarefa_do_dia.py:227; app/ponto/services/cartorio.py:1623 |
| Carimbo da pergunta, tela/auditoria/reparo | Mesmas portas acima | app/chamados/services/painel.py:67; app/chamados/services/raiox.py:135; app/chamados/services/historico.py:103; app/chamados/views.py:1257; app/templates/chamados/partials/_card_atendimento.html:182; app/chamados/management/commands/auditar_invariantes_chamados.py:433, :470, :955, :1710; app/chamados/management/commands/auditar_disputas.py:71, :150; app/chamados/management/commands/reabrir_orfas_enterradas.py:67, :129, :142; app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70, :147; app/chamados/management/commands/relavrar_via_i6.py:32, :58; app/chamados/management/commands/backfill_decisoes_pergunta.py:101, :314, :325, :330 |

Os significados relevantes não são todos intercambiáveis:

- `ENCERRADOS` é resolvido/fechado; `TERMINAIS` acrescenta cancelado/superado; `NAO_FECHADOS` inclui resolvido. A porta coleta por `ENCERRADOS`, não por todo terminal. (app/chamados/catalogo/motor.py:30; app/chamados/catalogo/motor.py:31; app/chamados/catalogo/motor.py:32; app/chamados/services/fecho_runtime.py:158)
- `resolvido_em` preserva o instante anterior em resolvido→fechado, mas `via_encerramento` é sobrescrita. Portanto o par não necessariamente descreve o mesmo ato terminal; isso já está no núcleo existente. (app/chamados/models.py:242; app/chamados/models.py:245)
- A competência da Pauta usa o mês do **fim da janela**, com corte empresarial; não usa simplesmente o mês civil da pergunta. As datas inicial e final representam o intervalo corte…corte−1. (app/chamados/services/fecho_runtime.py:124; app/chamados/services/fecho_runtime.py:125; app/ponto/janelas.py:10; app/ponto/janelas.py:24)
- Conversa usa dia civil em `data_evento`, e o juiz devolve `None` para seu dia de turno. O runtime respeita isso; `_vivas_entre` ainda consulta competência pelo `data_evento` cru. Essa divergência é preexistente e impede afirmar uniformidade total. (app/chamados/juizes.py:332; app/chamados/services/fecho_runtime.py:120; app/chamados/models.py:364)
- `materializada_em` não prova plantio de batida: a porta de identidade grava `admin_decidiu` sem plantio, embora o help text ainda diga “batida criada + chamado fechado”. O C6 lê via e carimbo juntos. (app/chamados/models.py:3396; app/chamados/models.py:3170; app/chamados/juizes.py:1368)
- `_RECUSAS` contém identidades e motivos, sem grandeza temporal. O runtime não acrescenta relógio próprio; os relógios alcançados continuam nos escritores de estado/carimbo/contador. O fuso configurado é São Paulo, com `USE_TZ=True`. (app/chamados/services/fecho_runtime.py:19; app/chamados/models.py:244; app/chamados/models.py:3186; app/pautas/services.py:362; app/pautas/services.py:366; app/confi

[truncado em 20000 de 31158 caracteres]

## 11/11 23:49 · sessao 78d66b49c516 · - · - · origem=Ronald (tmux)

**Item 4 — escritores/leitores e semântica**

- `status_local`: escritor persistente canônico `_gravar_estado`, atribuição em `app/chamados/models.py:228`, `save` em `:296`; INSERT de nascimento em `:1096`; simulação com restauração em `:356`/`:371`. Os gates usam o motor existente, e seus significados continuam separados: VIVOS, COBRANCA, ENCERRADOS, TERMINAIS e NAO_FECHADOS em `app/chamados/catalogo/motor.py:24-32`; autoridades registradas em `app/core/juizes.py:273-279`.
- Leitores de `status_local`, por famílias: gate/tripwire `app/chamados/models.py:165`, `:219`, `:221`, `:364`; receiver `app/chamados/signals.py:97`, `:105`; C6/destino `app/chamados/juizes.py:760`; fio/fila `app/chamados/services/fio.py:389`, `:432`, `app/chamados/services/na_fila.py:17`; painel `app/chamados/services/painel.py:213-224`; cartório `app/ponto/services/cartorio.py:1358`, `:1616`; encerramentos/cascatas `app/colaboradores/services/desligamento.py:153`, `:156`, `app/colaboradores/services/vinculo.py:386`; emissores/reconciliadores `app/ponto/management/commands/detectar_ausencias.py:153`, `:194`, `:259`, `app/chamados/reconciliador.py:247`, `:840`; API/telas `app/api/views.py:320`, `:1044`, `:1972`, `app/api/views_core.py:309`, `app/chamados/views.py:976`, `:1477`, `app/colaboradores/services/calendario.py:437`; relatórios `app/relatorios/views.py:745`, `:920`, `app/relatorios/pdf_chamados.py:116`; auditorias `app/chamados/management/commands/auditar_invariantes_chamados.py:573`, `:1068`; templates `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/fio_colaborador.html:167`, `app/templates/chamados/partials/_lista_chamados.html:33`, `app/templates/chamados/partials/_card_atendimento.html:13`, `app/templates/chamados/partials/modal_fio.html:98`, `app/templates/chamados/partials/painel_chamado_turno.html:18`, `app/templates/relatorios/historico_chamados.html:121`. Catálogo acima resume famílias; exaustividade de leitores, sobretudo SQL/dinâmicos, **[não verificado]**.
- `via_encerramento`: escrito em `app/chamados/models.py:245`, limpo em `:249`; significado “quem/como deixou de ser vivo”, fonte em `app/chamados/catalogo/motor.py:86-96`. Leitores: `app/chamados/models.py:1008`, `:1343`; `app/chamados/views_arquivar.py:64`, `:165`; `app/ponto/services/furos_sem_cobranca.py:69-70`; `app/chamados/services/disputa_emissao.py:1874`, `:1901`; `app/chamados/services/competencia_trancada.py:260`; `app/chamados/services/adesao.py:91`; auditoria em `app/chamados/management/commands/auditar_invariantes_chamados.py:1070`; relatório de reparo em `app/chamados/management/commands/reabrir_orfas_enterradas.py:135`.
- `resolvido_em`: instante da primeira saída de vivo; `resolvido→fechado` preserva carimbo, em `app/chamados/models.py:242-244`; reabertura limpa em `:248`. Leitores: proteção contra reabertura `app/chamados/models.py:1343-1347`; dedup de nota `app/chamados/services/disputa_emissao.py:1861`; janela civil/deslizante inclusiva `>=` em `app/chamados/services/painel.py:235-252`; `app/chamados/services/verbos.py:230`, `:237`; `app/chamados/services/fila.py:109`; ordenação `app/chamados/views_arquivar.py:166`; auditoria `app/chamados/management/commands/auditar_invariantes_chamados.py:574`; templates `app/templates/relatorios/historico_chamados.html:127`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/chamados/detalhe.html:56`.
- `Pauta.ancora_id/texto`: E4B monta texto em `app/chamados/services/fecho_runtime.py:58-83`, competência em `:120-126`; único INSERT delegado em `app/pautas/services.py:180-185`; resposta copia âncora em `:199`. Semântica empresa+competência documentada em `app/pautas/models.py:34`, `:84-89`. Leitores: dedup `app/chamados/services/fecho_runtime.py:136-137`; lista/chip `app/pautas/services.py:322`, `:336`; API `app/pautas/views.py:40-41`, `:47`; UI `app/templates/core/_copiloto.html:405`, `:570-573`; ficha `app/templates/colaboradores/detalhe.html:119`; suporte `app/pautas/management/commands/suporte_hasner.py:109-113`. Competência usa mês do **fim** da janela, com intervalo inclusivo corte..corte−1 em `app/ponto/janelas.py:10-32`, `:60-63`.
- `_RECUSAS`: somente em `app/chamados/services/fecho_runtime.py`: default `:13`, leituras `:18`, `:30`, `:156`, atribuições/restaurações `:35`, `:39`, `:163`, `:167`, `:179`, `:183`; significado `{chamado:pk, perguntas:{pk:(pergunta,motivo)}}` em `:19-20`, `:34`, `:162`; não contém instante/unidade temporal.
- `PerguntaDisputa.materializada_em/via_resolucao`: atribuição central conjunta em `app/chamados/models.py:3098-3102`; persistência pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` e lote em `:3155`, `:3191`, `:3213`, `:3265-3267`; exceção nominal troca apenas via no fato já materializado em `:3276-3277`. C7 chama `fechar_por_admin` em `:1922`, usando o próprio instante no registro em `:1926`; S143 chama `carimbar` em `app/chamados/services/disputa_emissao.py:2060`.
- Leitores principais desses campos: C6 `app/chamados/juizes.py:662`, `:674`, `:689`, `:1342`, `:1368`, `:1374`, `:1405`; estado/SQL de vivas `app/chamados/models.py:3044`, `:3056`, `:3351-3353`; materializador `app/chamados/services/materializacao.py:435`, `:439`, `:626-633`, `:1090`, `:1373`; validação `app/chamados/services/validacao.py:188-190`, `:461`, `:472`; S143 `app/chamados/services/disputa_emissao.py:1908`, `:2040`; reconciliador `app/chamados/reconciliador.py:353`, `:367`, `:1144`, `:1495`; veredito lavrado `app/chamados/veredito_lavrado.py:203-204`; fio `app/chamados/services/fio.py:1101`, `:1168`, `:1244`, `:1351`, `:1434`, `:1854`; passivo `app/chamados/services/passivo_validado.py:21`, `:64`, `:140-142`; relavra `app/chamados/services/relavra_via.py:42`, `:58`, `:66`, `:181`, `:244-252`; fábrica `app/chamados/services/fabrica_da_celula.py:149`, `:171`; tarefa `app/chamados/services/tarefa_do_dia.py:227-228`; cartório `app/ponto/services/cartorio.py:1623`; painel/raiox/histórico `app/chamados/services/painel.py:67`, `app/chamados/services/raiox.py:135`, `app/chamados/services/historico.py:103`; tela `app/chamados/views.py:1257`, `app/templates/chamados/partials/_card_atendimento.html:182`, `:186`; auditorias `app/chamados/management/commands/auditar_invariantes_chamados.py:433`, `:470`, `:955`, `:1710`, `app/chamados/management/commands/auditar_disputas.py:71`, `:150`; reparadores `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:129`, `:142`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70-72`, `:147`, `app/chamados/management/commands/relavrar_via_i6.py:32`, `:58-68`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:101`, `:314`, `:325`, `:330`.

**Achados para itens 4/5**

1. **Pauta sem exportada é permitida pelo código.** Limbo sozinho produz `recusa:credor_carrega_limbo:...`; o sufixo exportada é opcional em `app/chamados/services/disputa_emissao.py:1835-1846`; S143 coleta todo esse prefixo em `:1948-1952`; `_escrever_pautas` exige dia válido, sem exigir exportada no motivo, em `app/chamados/services/fecho_runtime.py:119-126`, `:139-141`. A origem humana `resolvido→fechado`, agora coletada em `:173-184`, permite alcançar isso sem guarda de origem viva em `app/chamados/models.py:165`. A carga inclui irmã limbo mesmo morta, porque `app/chamados/services/disputa_emissao.py:1798-1801` usa `_ancoradas`, cujo filtro não exige viva em `app/chamados/models.py:344-347`; a pergunta alvo pode continuar viva pela célula furada em `app/chamados/juizes.py:1378-1381`. Ocorrência PROD e execução ORM **[não verificado]**.
2. **Dois usos de data preexistentes:** gate lê `data_evento` cru em `app/chamados/models.py:364`; runtime usa juiz que devolve `None` para conversa em `app/chamados/services/fecho_runtime.py:120`, `app/chamados/juizes.py:332-336`. Assim, dia civil de conversa pode ser tratado como exportado na guarda e não ancorar Pauta. Não é mudança nova no diff fornecido; ocorrência **[não verificado]**.
3. **Ambiguidade documental preexistente:** `materializada_em` diz “batida criada + chamado fechado” em `app/chamados/models.py:3394-3396`, mas `carimbar_identidade` registra decisões sem plantio em `:3170-3191`; C6 lê via+carimbo como identidade em `app/chamados/juizes.py:1360-1369`. Não achei novo significado introduzido pelo diff nesses campos.
4. **Relógio novo no runtime: não achei.** O alcance novo passa pelos relógios preexistentes do núcleo/carimbo em `app/chamados/models.py:244`, `:3125`, `:3186` e contador em `app/pautas/services.py:362`, `:366`; timezone configurado `America/Sao_Paulo`/`USE_TZ=True` em `app/config/settings/base.py:93`, `:95`.
5. **Juiz duplicado no runtime: não achei:** composição chama juiz do dia e janela em `app/chamados/services/fecho_runtime.py:113`, `:116`, `:120-125`, sem regra própria de vitalidade; autoridades registradas em `app/core/juizes.py:273-292`.
6. Ordem canônica existente C→D→Ch→P em `app/chamados/models.py:2630-2694`; nova entrega acrescenta contador DP→âncoras ordenadas após ato em `app/chamados/services/fecho_runtime.py:130-141`, `app/pautas/services.py:344-352`. Ciclo concorrente concreto causado pela ampliação **[não verificado]**; não certificar ordem global de lotes multicollaboradores sem medição/inspeção do atomic externo.

## 11/11 23:48 · sessao 1d0a0301f85d · - · - · origem=Ronald (tmux)

**Item 2.** Contra o snapshot textual, reverter os 46 enxertos recupera o AST anterior dos 30 arquivos, descontados os imports; o runner também usa esse snapshot, carregado em `app/chamados/tests/e4b_c12_verificar.py:17-18`. Identidade Git da base e comparação independente de commits: [não verificado]; a cópia não contém repositório Git [não verificado].

O ramo `ato=` devolve exatamente o resultado do callable, inclusive `False` e `None`; `user` e `novo_status` externos não são repassados ao método original, que conserva seus argumentos (`app/chamados/services/fecho_runtime.py:157-169`). `reconciliar`, `encerrar`, `encerrar_por_competencia_trancada` e `cancelar` devolvem booleanos; `fechar` não possui retorno explícito (`app/chamados/models.py:642`, `app/chamados/models.py:656`, `app/chamados/models.py:1606`, `app/chamados/models.py:1621`, `app/chamados/models.py:1641`, `app/chamados/models.py:774`, `app/chamados/models.py:793`, `app/chamados/models.py:452`).

- S01–S13: mesmos métodos/argumentos e mesmos usos booleanos; S02–S04 descartam retorno; S12/S13 preservam as duas tentativas e rollback intermediário (`app/chamados/management/commands/auditar_chamados_orfaos.py:68`; `app/chamados/management/commands/smoke_chamados.py:135`, `:148`, `:164`; `app/chamados/reconciliador.py:497`, `:524`; `app/chamados/services/adesao.py:116`, `:151`; `app/chamados/services/competencia_trancada.py:185`; `app/chamados/services/feriado_reavaliar.py:48`; `app/chamados/services/limbo_folgas.py:102`, `:284`, `:287`).
- S14–S25: S14/S16/S22/S25 descartam resultado; S15 devolve o booleano; demais usam condição booleana; S23 mantém motivo completo e captura `ch=ch` (`app/chamados/services/materializacao.py:583`, `:772`; `app/chamados/services/regularizacao_ext.py:206`; `app/chamados/services/silencio_ausencia.py:102`; `app/chamados/signals.py:43`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:141`; `app/colaboradores/services/desligamento.py:156`; `app/colaboradores/services/pos_vinculo.py:43`; `app/colaboradores/services/vinculo.py:388`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:48`; `app/ponto/management/commands/curar_residuo_bug115.py:152`; `app/ponto/management/commands/detectar_ausencias.py:159`).
- S26–S38: usos booleanos preservados, inclusive seleção das comprehensions e ramo dry; S30 conserva argumentos e descarta `None`; S31 conserva motivo e captura `ch=ch` (`app/ponto/management/commands/detectar_ausencias.py:825`, `:835`, `:846`; `app/ponto/management/commands/detectar_intervalo_ausente.py:139`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`; `app/ponto/services/ausencia.py:695`; `app/ponto/services/cartorio.py:987`, `:1512`, `:1525`, `:1546`, `:1624`, `:1639`).
- S39–S46: usos booleanos preservados; S40 mantém `_fechou`; S46 descarta retorno e conserva a transição não terminal (`app/ponto/services/cluster_espurio.py:125`; `app/ponto/services/cobertura.py:123`, `:318`; `app/ponto/services/entrada_sincronizada.py:32`; `app/ponto/services/lastro.py:401`; `app/ponto/services/triagem_batida.py:598`; `app/ponto/services/veredito_celula.py:203`; `app/chamados/services/acoes_chamado.py:238`).
- E01–E09 descartam retorno da porta, portanto não há conversão indevida de contagem em booleano nesses chamadores (`app/chamados/models.py:455`; `app/chamados/services/acoes_chamado.py:71`, `:226`, `:283`, `:110`, `:152`, `:322`; `app/chamados/services/fio.py:722`; `app/colaboradores/views.py:2069`).

**Ressalva S30:** a ordem entre a Pauta existente de E01 e a nota muda. Antes, `fechar` escrevia Pauta pela porta interna e depois nota; com wrapper externo, a reentrada interna retorna sem escrever Pauta, a nota nasce e só depois o wrapper entrega Pauta (`app/chamados/models.py:455-458`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; `app/chamados/services/fecho_runtime.py:173-176`, `:168`). Não achei troca na ordem entre as escritas antigas de estado e nota; equivalência operacional ORM [não verificado].

**Item 7.** A primeira chamada pula a disputa porque a irmã morta tem resposta sem veredito; essa lista exige `materializada_em is None` e texto/resposta (`app/colaboradores/services/desligamento.py:140-143`; `app/chamados/models.py:1843-1847`). S20 encerra o chamado; S143 poupa a viva e carimba a irmã morta com `resposta='validar'`, removendo o impedimento da disputa (`app/colaboradores/services/desligamento.py:156`; `app/chamados/services/disputa_emissao.py:1966-1987`).

Na segunda chamada, a disputa entra em `fechar(..., motivo=desligamento)`; C7 chama `fechar_por_admin` nas vivas e grava `admin_decidiu`/`materializada_em` por `carimbar_identidade` (`app/colaboradores/services/desligamento.py:143`; `app/chamados/models.py:1919-1927`; `app/chamados/models.py:3065`; `app/chamados/models.py:3186-3191`). Esse enterro ocorre **antes** do save da disputa e fora de S143/porta do Chamado; o save da disputa pode depois acionar S88, mas a pergunta já foi carimbada (`app/chamados/models.py:1937`; `app/chamados/signals.py:19-43`).

O teste alterado ainda exige viva sem `materializada_em` antes da segunda chamada, verifica Pauta idêntica depois e passa a exigir a via C7 esperada; não achei ocultação de enterro na primeira chamada (`app/chamados/tests/test_e4b_c12_orm.py:168-179`). Resultado ORM e demonstração pareada contra c11 real: [não verificado]. O teste não afirma explicitamente `materializada_em is not None` após a segunda chamada (`app/chamados/tests/test_e4b_c12_orm.py:174-179`).

**Item 8.** O mecanismo de +2 por entrada **aninhada e não reentrante** é coerente com o `atomic()` padrão; isso inclui entrada cujo gate devolve `False` (`app/chamados/services/fecho_runtime.py:158-169`). Sem recusas, o retorno anterior ao atomic remove o savepoint vazio de `_escrever_pautas` (`app/chamados/services/fecho_runtime.py:118-130`).

A explicação “2 entradas por chamado” não bate com +8 para três chamados: duas vezes três entradas, a +2 cada, seriam +12; +8 corresponde a quatro entradas (`app/ponto/tests/test_selo_chokepoint_escrita.py:210`). O código permite uma entrada por chamado no loop e outra por disputa/pai fechado no E2: E2 roda antes do loop, fecha disputas distintas e dispara S88; depois o loop ainda pode entrar para um chamado já fechado pelo signal (`app/ponto/services/cartorio.py:983-987`; `app/chamados/reconciliador.py:609-615`; `app/chamados/signals.py:34-43`). Logo, “N chamados + um pai de disputa” explicaria +4/+8; contagem efetiva dessa execução [não verificado].

“Nunca por item” só é defensável para o custo do savepoint por entrada. A porta é chamada dentro de loops por chamado; com recusas, `_escrever_pautas` percorre perguntas, acessa suas relações e janela, depois executa lock/dedup/escrita por competência (`app/ponto/services/cartorio.py:987`; `app/chamados/services/fecho_runtime.py:119-141`). Totais observados 217/277 e alegação de diff SQL exclusivamente SAVEPOINT/RELEASE: [não verificado]; constam como declarações nos comentários e no plano, sem a medição no dado raspado (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`; `PLANO_PISCADA.md:142`).

## 11/11 23:47 · sessao 7ca0371690e3 · - · - · origem=Ronald (tmux)

Itens 1 e 6 — somente leitura.

**Censo**

Não achei escritor corrente de fechamento/resolução de `ChamadoColaborador` fora da porta e do corpo nominal; os nove sítios internos estão em `app/chamados/tests/e4b_c12_ast.py:18`, e o teste cobra ausência de outros escritores em `app/chamados/tests/test_e4b_c12_selo.py:22`. Completude contra reflexão/SQL arbitrário: [não verificado].

A leitura literal de “todo escritor de status” encontra **20 chamadores não terminais fora da porta**, omitidos pelo conjunto `GATES`; `analisar` e `reabrir` escrevem em `app/chamados/models.py:807` e `app/chamados/models.py:1443`, mas seus nomes não constam de `app/chamados/tests/e4b_c12_ast.py:14`. O motor limita esses eventos ao destino `em_analise`, portanto não são fechadores/resolvedores do aval (`app/chamados/catalogo/motor.py:112`).

- `analisar`: `app/chamados/views.py:1513`; `app/ponto/services/regularizacao.py:137`; `app/chamados/services/disputa_emissao.py:1085`, `:1159`, `:1178`, `:1223`, `:1252`, `:1379`; `app/chamados/management/commands/geofence_registrados_para_analise.py:32`.
- `reabrir`: `app/chamados/models.py:1020`; `app/chamados/views_arquivar.py:70`; `app/ponto/management/commands/reconciliar_grade.py:530`; `app/ponto/management/commands/detectar_ausencias.py:302`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`; `app/chamados/services/disputa_emissao.py:1851`, `:2011`; `app/chamados/services/competencia_trancada.py:266`; `app/chamados/services/acoes_disputa.py:270`; `app/chamados/services/adesao.py:93`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`.

As migrations escrevem fora da porta: 0038 em `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; 0039 em `app/chamados/migrations/0039_a3_via_backfill.py:24`, `:33`. São nominalizadas em `app/chamados/tests/test_e4b_c12_selo.py:97`. Aplicação prévia/vazio de tenant novo só aparece como alegação de comentário e não está comprovada pelo dado raspado (`app/chamados/tests/test_e4b_c12_selo.py:93`).

Não achei escrita terminal literal em scripts externos de `bin`/`mensageria` nem SQL literal de `status_local`; completude dessa ausência é [não verificado]. O selo restringe a varredura a `app`, pois `ROOT` nasce em `app/chamados/tests/e4b_c12_ast.py:11`, e ainda exclui migrations, diretórios de teste e arquivos com prefixos `test_`/`tests_` (`app/chamados/tests/e4b_c12_ast.py:34`).

**Mordida e buracos**

Os testes existentes cobrem botão direto, update, aliases, getattr literal, loop, save e bulk_update literal (`app/chamados/tests/test_e4b_c12_selo.py:48`, `:64`, `:77`). Executei o runner somente em memória, com `python3 -B`/`PYTHONDONTWRITEBYTECODE=1`: **17 testes, OK**, sem banco; registro persistente desse resultado [não verificado].

Experimentos somente em memória mostraram:

| Forma | Resultado do AST | Explicação no código |
|---|---|---|
| `import chamados.models as m; ...encerrar()` | Morde | Resolve imports/atributos (`app/chamados/tests/e4b_c12_ast.py:109`, `:190`) |
| `C.custom.filter(...).update(status_local=...)` | Morde | Update aceita tipo desconhecido (`app/chamados/tests/e4b_c12_ast.py:253`) |
| `bulk_update(..., ['status_local'])` | Morde | Campo literal reconhecido (`app/chamados/tests/e4b_c12_ast.py:259`) |
| `getattr(c, 'encer' + 'rar')()` | Passa | Só resolve string `Constant` (`app/chamados/tests/e4b_c12_ast.py:114`) |
| Método herdado: `class Proxy(C): ... self.encerrar()` | Passa | Não resolve herança; compara tipo exatamente com TARGET (`app/chamados/tests/e4b_c12_ast.py:127`, `:249`) |
| `bulk_update(..., ['status_' + 'local'])` | Passa | Soma faz união dos fragmentos, não concatenação (`app/chamados/tests/e4b_c12_ast.py:161`) |
| `update(**{'status_local':'fechado'})` | Passa | Ignora keyword com `arg=None` (`app/chamados/tests/e4b_c12_ast.py:241`) |
| `save(False, False, None, ['status_local'])` | Passa | Só examina keyword `update_fields` e atribuição anterior (`app/chamados/tests/e4b_c12_ast.py:264`) |
| `transicionar_com_pauta(..., ato=c.encerrar())` | Passa | Autoriza qualquer AST dentro de `ato`; a chamada seria avaliada antes da entrada da porta (`app/chamados/tests/e4b_c12_ast.py:246`) |

O selo de migration pega update literal e atribuição, conforme seus dois adversariais (`app/chamados/tests/test_e4b_c12_selo.py:133`). Entretanto, **não pega bulk_update com fields posicional ou keyword `fields`, getattr literal, `save(update_fields=...)`, nem `**kwargs`**, pois a regra só reconhece chamadas de atributo com keyword `status_local`/`defaults`, ou atribuição explícita (`app/chamados/tests/test_e4b_c12_selo.py:112`).

Há um buraco adicional mais forte: inserir **uma nova escrita terminal literal dentro de `0039.backfill`** mantém o selo verde, porque os achados são um conjunto apenas de `(arquivo, função)`; a nova operação não muda esse conjunto (`app/chamados/tests/test_e4b_c12_selo.py:115`, `:119`, `:131`). Reproduzi em memória acrescentando `C.objects.update(status_local='fechado')` dentro dessa função; registro persistente [não verificado].

A descoberta de migrations ainda depende de nomes `0*.py`, excluindo uma futura `1000_*.py` (`app/chamados/tests/test_e4b_c12_selo.py:126`).

## 11/11 23:44 · sessao d83266f85771 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

C6 aplicada: cobertura separada do rótulo, mapas legados preservados, efeito de tipos inativos mantido e versão 5 com horizonte `2026-11`. [Relato, censo e escritores/leitores](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6.r:1). [nao verificado]

22 testes GREEN; RED na base `f218d86a7` e na c5; 15 mutantes mortos, incluindo os nove anteriores. [Comandos e resultados](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6_execucoes.r:4).

Sem commit, pela proibição inicial. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6.r:66). Certificação da suíte completa e Opus final: [nao verificado]. [nao verificado]

## 11/11 23:23 · sessao 2ecf1a0154a7 · FIO-E0-V4-E4B · construtor · origem=Code

C12 aplicada. A [entrega com tabela, leitores/escritores e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:6) registra os 46 sítios migrados. [nao verificado]

Validação local:

- [15 testes novos verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1185) e [106 de regressão verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:730). [nao verificado]
- [55 mutantes: exatamente um subTest vermelho por sítio](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:470).
- [Base: 46 falhas esperadas](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1162).

ORM D3/D5 preparado, sem execução: **[não verificado]**. Sem commit, conforme a [proibição da obra](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:4). Certificação permanece com Code/suite/opus. [nao verificado]

## 11/11 23:01 · sessao e5ede8a32f5b · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; ORM não executado [não verificado].

- Reutilizar `RecusaS143RuntimeTest._cenario`: cria exportação, fechamento e pergunta viva com assertions reais (`chamados/tests/test_e4b_c6_orm.py:45`).
- D3 deve testar `resolvido → fechado`: matriz permite (`chamados/catalogo/motor.py:102`), guarda humana de viva só roda para origem `VIVOS` (`chamados/models.py:165`), e save terminal dispara S143 (`chamados/signals.py:96`). Resultado ORM: [não verificado].
- Vínculo: tornar categoria `escala_aceite` e chamar `_executar_aceite` com template do vínculo existente; fechamento precede criação (`colaboradores/services/vinculo.py:384`, `colaboradores/services/vinculo.py:393`).
- Desligamento: `_cenario(limbo=True)` mantém disputa mista aberta pela resposta sem veredito (`chamados/models.py:1843`); cascata pula essa disputa e alcança `encerrar` (`colaboradores/services/desligamento.py:139`, `colaboradores/services/desligamento.py:155`).
- Regularização: preparar disputas fechadas sem carimbar a pergunta, verificar C6 viva, então materializar reg confirmada. Guard só barra disputas abertas (`chamados/services/regularizacao_ext.py:58`), mas coleta S143 usa `_ancoradas()` também de disputas fechadas (`chamados/services/disputa_emissao.py:1940`). C6 não consulta `disputa.fechada_em` (`chamados/juizes.py:1330`). Existência desse acervo em produção: [não verificado].
- Cron: chamar `Command()._fechar_chamado(ch, entrada)` com chamado não-furo da fixture; gate permite premissa não-furo (`chamados/models.py:728`), sítio preserva booleano (`ponto/management/commands/detectar_ausencias.py:842`).
- Signal: fechar outra disputa sem perguntas cujo pai seja `ch`, conservando a disputa da viva; receiver reconcilia pai (`chamados/signals.py:28`).
- API: chamado `batida_ausente` no dia aberto, marco E08 aceso na célula e viva em outro dia exportado. Lookup é real (`chamados/juizes.py:2105`), juiz lê célula gravada (`ponto/services/lastro.py:185`).
- Cartório: cobrar marco antigo E07 num dia cuja grade contém E08/S17; manter viva em outro dia exportado. Ramo `marcos_inexistentes → reconciliar` exercita `_julgar_colab_corpo` sem adiar fechamento (`ponto/services/cartorio.py:1523`); gate consulta mesmo juiz (`chamados/models.py:1298`).

Para mutantes ORM, seguir enxerto em memória já existente, substituindo somente função/sítio e preservando globais (`chamados/tests/e4b_c11_verificar.py:78`, `chamados/tests/e4b_c11_verificar.py:90`). Execução e comandos novos dependem dos nomes finais [não verificado].

## 11/11 22:57 · sessao 65a24b6b5e82 · FIO-E0-V4-E4B · sonda · origem=Code

JSON integral fornecido, sem reexecução independente: não localizei `sonda_mig0039.py` na cópia. [não verificado]

```json
{"aplicada_em": "2026-08-23 04:09:15.485039+00:00", "chamados_superado": 107, "mig0039_aplicada": 1, "migrations_chamados_posteriores": 14} [nao verificado]
```

**(a)** O acionamento registrado é `RunPython(backfill, desfazer)`: `backfill` na aplicação efetiva da 0039; `desfazer` na reversão dela, com dependência da 0038. ([0039_a3_via_backfill.py:38](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:38), [0039_a3_via_backfill.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:39))

“Somente podem rodar por migrate/rollback” seria excessivo: testes chamam ambas diretamente. ([test_esmeril2_migracoes_reversas.py:36](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:36), [test_esmeril2_migracoes_reversas.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:39))

**(b)** No fluxo de tenant novo implementado, **zero**: a tabela nasce por `CreateModel`, sem inserções; as migrations de dados anteriores apenas atualizam registros existentes. ([0001_initial.py:16](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0001_initial.py:16), [0023_rebaixar_urgencia_resolvidos.py:4](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4), [0024_rebaixar_urgencia_resolvidos_regressao.py:5](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5), [0038_a3_registrado_backfill.py:9](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0038_a3_registrado_backfill.py:9))

O provisionamento cria o tenant com schema automático antes do bootstrap administrativo. ([models.py:21](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/models.py:21), [services.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:11), [services.py:13](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:13)) Contagem medida num tenant novo de produção: [não verificado].

**(c)** Não encontrei importação ou chamada dessas funções em código de produção fora de migrations na busca estática da cópia. Ausência em execução real: [não verificado]. O import direto encontrado está em teste; as referências produtivas encontradas à 0039 são comentários. ([test_esmeril2_migracoes_reversas.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:11), [cluster_espurio.py:134](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:134), [cluster_espurio.py:136](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:136))

Atribuir os 107 `superado` à execução da 0039, apenas com esse JSON: [não verificado].

## 11/11 22:57 · sessao 7e1f793c05ad · - · - · origem=Ronald (tmux)

No fluxo de tenant novo implementado, a 0039 encontra **0 `ChamadoColaborador`**: a tabela é criada em `app/chamados/migrations/0001_initial.py:16`, sem inserção de registros; as migrações de dados anteriores apenas alteram objetos existentes (`app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5`, `app/chamados/migrations/0038_a3_registrado_backfill.py:9`).

O provisionamento cria `Cliente` antes do bootstrap (`app/tenants/services.py:11`, `app/tenants/services.py:13`), com criação automática do schema habilitada (`app/tenants/models.py:14`, `app/tenants/models.py:21`). O bootstrap posterior cria Empresa, Setores e usuário administrador (`app/tenants/bootstrap.py:27`, `app/tenants/bootstrap.py:32`, `app/tenants/bootstrap.py:34`). O teste de scaffolding também contém a asserção `ChamadoColaborador.objects.count() == 0` (`app/tenants/tests.py:13`).

Isso descreve o fluxo da cópia; não executei testes nem medi um tenant novo em produção. O dado fornecido não contém essa contagem de tenant novo. [não verificado] [nao verificado]

## 11/11 22:57 · sessao f6699bf6e06d · - · - · origem=Ronald (tmux)

A 0039 registra `backfill` e `desfazer` como operação direta/reversa de `RunPython`; depende da 0038. `app/chamados/migrations/0039_a3_via_backfill.py:38-39`.

Não cabe afirmar que as funções **somente podem** executar via migrate/rollback: o teste importa a migration e chama ambas diretamente. `app/chamados/tests/test_esmeril2_migracoes_reversas.py:11`, `:36`, `:39`, `:47`, `:55`.

Na busca estática de toda a cópia, incluindo arquivos ocultos/ignorados e excluindo `.git`, não encontrei importação ou chamada dessas funções em código de produção fora migrations; ausência absoluta em execução é [não verificado]. As referências produtivas encontradas são comentários: `app/ponto/services/cluster_espurio.py:134`, `:136`, `:139`; `app/chamados/models.py:798`. A migration seguinte apenas declara a dependência: `app/chamados/migrations/0040_a27_recusa_motivo.py:9`.

Não encontrei arquivo `sonda_mig0039.py` nem ocorrência do hash `273df217b8f8d27b` na cópia; execução independente da sonda e conferência desse hash são [não verificado].

## 11/11 22:53 · sessao 0524c0e7b362 · FIO-E0-V4-E4B · censo · origem=Code

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 11/11 22:50 · sessao f576ee34ec90 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

Não reexecutei a sonda; reproduzo integralmente o JSON raspado fornecido, sem validação independente em banco ou logs. [nao verificado]

```json
{
  "celulas_cobertas_sem_ata": 3,
  "celulas_por_tipo_e_ata_tipo_dia": {
    "folga|ausencia": 9,
    "troca_de_plantao|ausencia": 12
  },
  "dias_cobertos_folga_comp11": 24,
  "parcial_por_tipo_e_ata_tipo_dia": {},
  "tipos_inativos_com_ausencias": {
    "troca_de_posto": 0
  }
}
```

**(a)** No recorte solicitado da comp 11, o consolidado fornecido indica **24 células cobertas**: **21 com ata `ausencia`** (9 de `folga` + 12 de `troca_de_plantao`), **0 com ata `folga`**, **0 com outro `tipo_dia`** e **3 sem ata**. O mapa de parciais está vazio: **0 com `cobertura_parcial` no resultado fornecido**. [nao verificado]

**(b)** Nenhum tipo inativo aparece com contagem positiva de ausências; a única entrada fornecida é **`troca_de_posto`: 0**. [nao verificado]

**(c)** **A mudança isolada da grade de `ausencia` para `folga` não provoca relavratura de uma ata já carimbada e com veredito.** O próprio comentário declara que `tipo_dia` da grade não integra a impressão; com impressão igual e `forcar=False`, o código incrementa `pulados` e executa `continue`. [app/ponto/services/cartorio.py:190](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:190), [app/ponto/services/cartorio.py:1395](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1395)

O mecanismo de atualização por mudança de regra é:

- `REGRA_VERSOES` declara `((1, '2026-09'), (2, '2026-10'), (3, '2026-10'), (4, '2026-10'))`; `versao_para` escolhe a maior versão cujo horizonte alcança a competência — portanto, **4 para a comp 11**. [app/ponto/services/cartorio.py:132](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:132), [app/ponto/services/cartorio.py:248](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:248)
- A versão entra na impressão como `regra:N`. Uma nova versão aplicável muda esse insumo; `forcar=True` também permite atravessar o pulo por impressão igual. [app/ponto/services/cartorio.py:489](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:489), [app/ponto/services/cartorio.py:1399](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1399)
- Quando há julgamento e `apply_=True`, a ata recebe `tipo_dia` do dia da grade, passa por `lavrar_veredito` e é carimbada com a impressão. [app/ponto/services/cartorio.py:728](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:728), [app/ponto/services/cartorio.py:1836](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1836)

O JSON não informa a impressão nem a versão gravada nessas 21 células; sua elegibilidade individual para rejulgamento permanece [nao verificado].

## 11/11 22:47 · sessao 041b3efca22e · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVO a c5.** Encontrei três regressões bloqueantes: perda de valores quando o tipo é inativado; parcial que deixa de cobrar o restante do turno; folga aprovada que pode virar `nunca_bateu`. A propriedade “grade == status” também não está garantida para atas antigas já carimbadas. Referências: `app/ponto/services/dia_pago.py:390` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`, `:1275`, `:1395`.

Referências sem indicação são da **base**; **c5** indica a linha resultante do patch. Os contraexemplos abaixo são construídos a partir do código; ocorrência e impacto quantitativo em produção: **[nao verificado]**.

**1. Quem escreve, quem lê e o significado dos campos**

| Campo/valor | Quem escreve ou define | Quem lê, inclusive fora do diff |
|---|---|---|
| `faixa` | `_FAIXA_DE_TIPO` define a faixa; `faixa_de` consulta; A2 escolhe a vencedora. `app/ponto/precedencia.py:52`, `:100`, `:421` | Grade nova, status, predicado de folga humana, semana do posto, reavaliação de chamados, diagnóstico do colaborador e carimbo. `app/escala/utils.py:1224` **c5**; `app/ponto/status_dia.py:308`; `app/ponto/precedencia.py:212`; `app/escala/services/semana_do_posto.py:177`; `app/chamados/services/feriado_reavaliar.py:36`; `app/ponto/services/dia_do_colab.py:132`; `app/ponto/services/cartorio.py:1988` |
| `tipo_do_dia` | A2 escreve o código vencedor; c5 distingue folga automática por `folga_da_escala`. `app/ponto/precedencia.py:413`, `:432` **c5** | Tradutor, status, deslogados, faltas de hoje, sem-furo, diagnóstico e carimbo. `app/ponto/services/dia_decidido.py:371` **c5**; `app/ponto/status_dia.py:311`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/services/cartorio.py:1987` |
| `tipo_dia` | Builders/overlays escrevem a classificação da grade; cartório serializa; porta persiste; leitor reconstrói da ata. `app/escala/utils.py:1801`, `:1809`; `app/escala/utils.py:1224` **c5**; `app/ponto/services/cartorio.py:728`; `app/ponto/portas/celula.py:995`; `app/escala/services/leitor_celula.py:397` | Dinheiro, status, cobrança parcial, suprajuiz, ramo sem adesão e espelho. `app/ponto/services/dia_pago.py:397` **c5**; `app/ponto/status_dia.py:189`; `app/chamados/juizes.py:438`; `app/ponto/supra_juiz.py:304`; `app/ponto/services/cartorio.py:1230`; `app/ponto/services/espelho.py:679` |
| `ausencia_tipo` | Grade obtém o código da cobertura para folha; c5 transporta grade → ata → grade lida. `app/escala/utils.py:1188`, `:1189`; `app/ponto/services/cartorio.py:765` **c5**; `app/escala/services/leitor_celula.py:436` **c5** | Novo leitor financeiro consulta o efeito vigente desse código. `app/ponto/services/dia_pago.py:390` **c5** |
| `ausencia_id`, `ausencia_label` | Grade escreve PK/display da cobertura; c5 transporta as chaves pela ata. Falta recebe ID no ramo próprio. `app/escala/utils.py:1189`, `:1216`; `app/escala/utils.py:1226` **c5**; `app/ponto/services/cartorio.py:765` **c5** | O transporte preserva as chaves, mas o link do calendário citado nas notas usa outro objeto: cobertura atual → `dia.ausencia.id/label`. `app/colaboradores/services/calendario.py:485`, `:673`; `app/templates/colaboradores/partials/_calendario_grade.html:97` |
| Mapas `abono/previsto/realizado` por dia | `por_dia_da_grade`, em minutos, indexados por `data`. `app/ponto/services/dia_pago.py:364`, `:367`; implementação nova em `:387` **c5** | Fechamento soma; lavratura copia para `DiaPago`; prontidão nova soma; retrato de exportada e testemunha da porta também consultam. `app/ponto/services/fechamento.py:1062`, `:1212`; `app/ponto/services/dia_pago.py:288`, `:464`; `app/folha/export.py:1098` **c5**; `app/folha/porta_export.py:139` |
| Totais do fechamento | Rubricas horárias vêm de `resultado`; os três totais de minutos vêm dos mapas; previsto/dias/carimbo são gravados no mesmo bloco. `app/ponto/services/fechamento.py:1002`, `:1062`, `:1083` | Prontidão com snapshot, exportação de eventos e conferências de lavratura. `app/folha/export.py:1045`, `:1076`, `:402`; `app/ponto/services/dia_pago.py:550`; `app/folha/porta_export.py:143` |
| `prontidao` | Calculada por `folha.export.prontidao`; retorna percentual e previsto em horas. `app/folha/export.py:1025`, `:1050` | Snapshot lê os totais persistidos; caminho sem snapshot passa a consultar os mapas. `app/folha/export.py:1045`; `app/folha/export.py:1098` **c5** |
| `TipoSemFaixa` | `faixa_de` levanta recusa nomeada para código fora do mapa. Já existe na c3. `app/ponto/precedencia.py:96`, `:100` | Vigia conta; três capturas passam a propagar; cartório ainda captura genericamente. `app/ponto/services/vigia_ausencia.py:163`; `app/ferias/situacao.py:95` **c5**; `app/escala/services/semana_do_posto.py:178` **c5**; `app/ponto/services/espelho.py:651` **c5**; `app/ponto/services/cartorio.py:2035` |
| `tipos_ativos_sem_faixa` | Vigia percorre **todos os tipos ativos**, mesmo sem lançamentos, perguntando a `faixa_de`. `app/ponto/services/vigia_ausencia.py:144`, `:160`, `:168` | API de contadores e revisão da família. `app/api/views_mensageria.py:1416`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23` |

**Não supõem todos o mesmo significado.** A2 escolhe o fato mais forte; cobertura para folha escolhe a ausência aprovada mais recente. Portanto `tipo_dia` pode refletir a faixa de um fato enquanto `ausencia_tipo/id/label` identificam outro lançamento. Essa separação é exercitada pelo próprio teste de sobreposição da c5. `app/ponto/precedencia.py:421`; `app/ponto/turnos.py:2516`, `:2753`; `app/ponto/tests/test_lastro_c5_sem_banco.py:261` **c5**.

Datas de cobertura são inclusivas; o parcial noturno usa minutos desde o início do turno, podendo ultrapassar 1440, e inclui ambas as bordas. Os mapas financeiros mantêm a data recebida da grade; não convertem para o dia civil das batidas. `app/ponto/turnos.py:2540`, `:2638`, `:2641`, `:2647`; `app/ponto/services/dia_pago.py:390`, `:288`.

B2 separa a folga automática na origem da A2, mas `tipo_dia='folga'` continua agrupando folga humana e automática. Os leitores de cobrança que ainda interpretam esse rótulo causam os bloqueios abaixo. `app/ponto/precedencia.py:413` **c5**; `app/escala/utils.py:1224` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`.

**2. Dinheiro: preservação limitada e regressão concreta**

**BLOQUEIA — a ata nova perde valores quando o tipo sai do catálogo ativo.**

Contraexemplo: tipo `folga`, efeito ABONA; ausência aprovada; jornada prevista de 480 minutos; batidas pareadas cobrindo 180 minutos, por exemplo 08–11. A ata antiga era `ausencia`; a nova é `folga` com `ausencia_tipo='folga'`. Enquanto o efeito está disponível, ambas produzem abono 300, previsto 480 e realizado 180. Se o tipo for inativado, o catálogo deixa de retorná-lo e a ata nova passa a produzir **três mapas vazios**. `app/escala/utils.py:1224` **c5**; `app/ponto/services/dia_pago.py:390`, `:397` **c5**; `app/ponto/catalogo/ausencias.py:249`, `:293`.

A inativação é permitida pela porta; o signal invalida o cache, e o modelo declara que inativar preserva o histórico. O vigia de faixas não acusa esse caso, pois só conta tipos ativos sem mapa. `app/ponto/services/ausencia.py:1301`; `app/ponto/models.py:607`, `:1235`; `app/ponto/services/vigia_ausencia.py:160`.

A prova executada em memória confirmou os resultados abaixo para P=480/C=180; não exercitou banco ou persistência. As condições executadas correspondem a `app/ponto/services/dia_pago.py:387` da base e `:390` **c5**.

| Efeito disponível | Base, ata antiga `ausencia` | c5, ata nova `folga` |
|---|---:|---:|
| `abona`, `suprime` ou `desconta` | 300 / 480 / 180 | 300 / 480 / 180 |
| `None` | 300 / 480 / 180 | mapas vazios |

Alcance dessa regressão:

- **Com snapshot:** prontidão continua lendo o número gravado até sua invalidação/recálculo. **Sem snapshot:** uma janela composta só desse dia passa de 8 horas previstas/100% para `cego=True`, motivo `sem_escala`. `app/folha/export.py:1045`, `:1078`; `app/folha/export.py:1098`, `:1108` **c5**.
- **Fechamento e DiaPago:** recálculo soma os mapas; lavratura copia seus minutos. `app/ponto/services/fechamento.py:1046` **c5**; `app/ponto/services/dia_pago.py:288`.
- **Exportada:** o helper alterado também é chamado por `retratar_exportada`, que atualiza ou zera esses campos. Portanto “não tocar exportada” não está assegurado apenas por não editar esse chamador. A execução depende da guarda existente. `app/ponto/services/dia_pago.py:451`, `:464`, `:481`, `:495`.
- **Falha do catálogo:** `_do_banco` captura exceções e `vigente` usa a semente; códigos como `folga` e `troca_de_plantao` não estão nessa semente. Isso oferece outro caminho para efeito ausente. Falha real nesse caminho: **[nao verificado]**. `app/ponto/catalogo/ausencias.py:87`, `:253`, `:278`.

**O que preserva:** para dias anteriormente classificados como `ausencia`, com código ainda encontrável e efeito válido, c5 conserva P−C/P/C, inclusive nos SUPRIME/DESCONTA que o pedido mandou manter. Na prontidão sem snapshot, a contribuição antiga P passa a `(P−C)+C`, preservando o percentual nesse universo. `app/ponto/services/dia_pago.py:394` **c5**; `app/folha/export.py:1100`; `app/folha/export.py:1098` **c5**.

Não encontrei mudança direta nas fórmulas das rubricas horárias emitidas pelo TXT: elas continuam lendo os campos horários do fechamento. Isso não elimina a mudança de retenção descrita abaixo. `app/folha/export.py:402`; `app/folha/export.py:1241`.

Valores, batidas, atas e snapshots dos 24 dias reais mencionados no raspado: **[nao verificado]**.

**3. Classes de reprovação**

| Classe | Conclusão |
|---|---|
| Segundo juiz | Não identifiquei nova função que clone A2, cobertura ou previsto: a grade consulta A2 e os consumidores financeiros passam a compartilhar o helper. Permanecem leitores com regra incompatível, detalhados abaixo. Autoridades registradas: `app/core/juizes.py:184`, `:187`, `:190`, `:290`; costura c5: `app/escala/utils.py:1204`; `app/folha/export.py:1098` **c5**. |
| Segundo escritor fora da porta | Não encontrei novo escritor persistente no delta: `ata_do_dia` monta o dicionário; a lavra e a porta existentes persistem. `app/ponto/services/cartorio.py:765` **c5**; `app/ponto/services/cartorio.py:1839`; `app/ponto/portas/celula.py:995`, `:899`. |
| Relógio fora da borda | Não há nova leitura de relógio nos acréscimos do patch. Existe leitura preexistente de `localdate()`/`now()` em `julgar_celula`, mesmo com `agora=` recebido: **fila**. Contraexemplo: replay com `agora` de outro dia usa a data real nas guardas. `app/ponto/services/cartorio.py:1997`, `:2002`, `:2023`, `:2026`. |
| Derivação paralela | A soma financeira fica concentrada no helper; entretanto, leitores de `tipo_dia` mantêm interpretações próprias da cobertura. São os bloqueios parcial/sem adesão abaixo. `app/ponto/services/fechamento.py:1046` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`. |
| Campo com dois significados | `folga` passa a representar também ausência aprovada com efeito financeiro e possível cobertura parcial, enquanto leitores ainda o tratam como descanso integral. `app/escala/utils.py:1224` **c5**; `app/chamados/juizes.py:456`; `app/ponto/services/cartorio.py:1230`. |
| Default/except que engole | A recusa de faixa é nominal, mas a aplicação final de precedência ainda a captura genericamente; há também a captura/fallback do catálogo que alcança o novo leitor financeiro. `app/ponto/precedencia.py:105`; `app/ponto/services/cartorio.py:2033`; `app/ponto/catalogo/ausencias.py:253`, `:278`. |
| Ordem de trava | O delta não acrescenta locks/transações; save e evento continuam no mesmo `atomic`. Integração concorrente e ordem efetiva de locks: **[nao verificado]**. `app/ponto/portas/celula.py:899`. |
| Pouso antes do pré-requisito | Pouso executado: **[nao verificado]**. Suite inteira e opus final desta volta: **[nao verificado]**. A ordem exigida é suite verde → opus final → pouso. `CLAUDE.md:377`, `:381`. | [nao verificado]

**BLOQUEIA — parcial passa a calar o restante do turno.**

Contraexemplo: `folga_compensatoria` cadastrada como ABONA e `parcial=True`; ausência aprovada de 120 minutos, período 08–10; turno 08–17; marco das 17 apagado. Esse cadastro é aceito pela porta e pelo juiz de parcialidade. `app/ponto/services/ausencia.py:1301`; `app/ponto/catalogo/ausencias.py:250`; `app/ponto/turnos.py:2600`.

A c5 escreve `tipo_dia='folga'`, mantendo `cobertura_parcial=True`. Porém `ausencia_parcial_cobra_o_resto` exige literalmente `ausencia`; passa a False, e `dia_sem_trabalho` passa a True. O suprajuiz também deixa de reconhecer o dia como dia de cobrança. A prova em memória confirmou a mudança False → True no juiz de dia sem trabalho. `app/escala/utils.py:1224`, `:1234` **c5**; `app/chamados/juizes.py:438`, `:456`; `app/ponto/supra_juiz.py:304`.

**BLOQUEIA — folga integral aprovada pode virar `nunca_bateu` e reter folha.**

Contraexemplo: colaborador ativo, nenhuma batida apurável na vida; célula `trabalha=True`; folga aprovada integral em 15/10/2026, empresa com corte 21; primeiro julgamento ou julgamento forçado. Na base, a grade `ausencia` satisfaz `_coberto_inteiro_nb`; na c5, a grade `folga` deixa de satisfazê-lo. Fora do horizonte de competência 11, `sem_trabalho` também é False, e o ramo força `nunca_bateu`. `app/ponto/services/cartorio.py:1197`, `:1230`, `:1241`, `:1275`; `app/ponto/services/cartorio.py:270`, `:281`.

Esse veredito acusa e retém a folha mesmo sem protestos: muda a elegibilidade de pagamento/exportação, embora os mapas do dia possam continuar iguais. `app/escala/models.py:1767`; `app/folha/export.py:1241`, `:1251`, `:1262`.

**FILA preexistente — recusa pode ser engolida depois da lavra.**

Uma ausência aberta já persistida, de tipo sem faixa, pode ficar fora da cobertura de folha; o overlay retorna sem perguntar A2. A aplicação final de precedência encontra o tipo e sua exceção é capturada após o julgamento. Existência dessa linha em produção: **[nao verificado]**. `app/ponto/turnos.py:2516`; `app/escala/utils.py:1202`; `app/ponto/services/cartorio.py:2026`, `:2033`; `app/ponto/precedencia.py:105`.

Não atribuo esse exemplo à criação normal: o caminho com veto consulta a precedência antes. `app/ponto/services/ausencia.py:377`; `app/ponto/precedencia.py:522`.

**O RED sem banco não cobre esses caminhos reais.**

O harness executa funções e trechos AST, mas substitui ORM, calendário, cadastro e relógio. Em particular: `vigente()` vira dicionário; o teste de ata fabrica a célula diretamente; o fechamento executa só o trecho até `_dias_inc`; a prontidão recebe `previsto_em=None`. `app/ponto/tests/test_lastro_c5_sem_banco.py:7`, `:169`, `:307`, `:354`, `:240` **c5**.

Assim, ele não exercita inativação/cache/signals, guardas da impressão, `julgar_colab` completo, persistência pela porta, eventos, transações, snapshot nem retenção real. Esses caminhos estão fora dos trechos chamados pelos testes citados; suas implementações reais estão em `app/ponto/models.py:1235`; `app/ponto/services/cartorio.py:1246`, `:1395`; `app/ponto/portas/celula.py:899`; `app/folha/export.py:1045`, `:1241`.

**4. Ata antiga lida pelo leitor novo**

As três chaves ausentes **continuam ausentes**. O transporte só copia quando `campo in ata`; não consulta a cobertura atual. `app/escala/services/leitor_celula.py:436` **c5**.

Quem decide é:

- **Classificação:** `ata.tipo_dia`; se ausente/vazio, o leitor usa `cel.trabalha`. `app/escala/services/leitor_celula.py:397`.
- **Dinheiro sem slug:** o ramo legado de `por_dia_da_grade`. Ata `ausencia`, P480/C180, mantém 300/480/180; ata `folga` sem slug continua fora dos mapas. `app/ponto/services/dia_pago.py:397` **c5**.
- **Status já lavrado:** a coluna persistida, quando disponível e fora do andamento do turno. `app/ponto/status_dia.py:287`.

**BLOQUEIA a propriedade B1 integral — ata antiga pode permanecer divergente.** Contraexemplo: folga humana aprovada, ata antiga `ausencia`, status persistido `ausencia`, impressão atual e nenhum fato novo. A grade viva c5 passa a `folga`; a ata/status continuam antigos porque o cartório pula impressão igual. O patch não altera `REGRA_VERSOES`, e `tipo_dia` da grade não integra essa impressão. `app/ponto/services/cartorio.py:132`, `:192`, `:1395`; `app/escala/utils.py:1224` **c5**; `app/ponto/status_dia.py:287`.

Isso preserva o dinheiro legado da ata, mas não prova “grade == status no mesmo dia” em todo caminho. O teste de B1 chama status sem lavra e a função de status da lavra diretamente, sem passar pela guarda real. `app/ponto/tests/test_lastro_c5_sem_banco.py:228`, `:245` **c5**.

**5. B3: efeitos fora da A2**

**FILA — há mudança concreta de classe fora da A2.** Contraexemplo: troca `aguardando_decisao`, dia previsto de trabalho, nenhuma batida, ata com lâmpada apagada. Antes, A2 classificava como folga e `dias_com_furo` não incluía a data; c5 exclui a troca, A2 responde trabalho e o contador passa a incluir a data como furo. `app/ponto/precedencia.py:301` **c5**; `app/folha/services/sem_furo.py:20`, `:26`.

Simultaneamente, a cobertura de cobrança continua incluindo a troca aberta e pode suspender a cobrança antes de consultar a ata. Deslogados e faltas de hoje também a excluem pela cobertura. Isso mantém respostas distintas entre os indicadores. `app/ponto/turnos.py:2518`; `app/chamados/juizes.py:476`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`.

A mudança do contador é consequência demonstrável; que ela seja indesejada: **[nao verificado]**. Mudança de valor financeiro causada especificamente por B3: **[nao verificado]**. A cobertura de folha já exige aprovação, independentemente dessa alteração na A2. `app/ponto/turnos.py:2516`.

**6. Leitores que não migraram ou mantêm regra própria**

| Sítio | Resultado |
|---|---|
| `app/chamados/juizes.py:438` e consumidores, inclusive `app/chamados/services/declaracao_texto.py:119` | **Bloqueia:** parcial só é reconhecida sob `ausencia`; contraexemplo 08–10/turno até 17 acima. |
| `app/ponto/supra_juiz.py:304` | **Bloqueia:** mesmo requisito literal para cobrar o restante da parcial. |
| `app/ponto/services/cartorio.py:1230` | **Bloqueia:** cobertura integral aprovada só reconhecida sob `ausencia/falta`; contraexemplo sem adesão na competência 10 acima. |
| `app/escala/utils.py:1210`, `:1213` | **Fila preexistente:** falta continua com ramo próprio por slug. Falta aprovada em folga conserva folga na grade, enquanto A2 admite o fato em faixa de atestado. `app/ponto/precedencia.py:69`, `:406`; `app/ponto/status_dia.py:311`. |
| `app/ponto/services/cartorio.py:2035` | **Fila preexistente:** captura genérica ainda alcança `TipoSemFaixa`; contraexemplo de ausência aberta já persistida acima. |
| `app/colaboradores/services/calendario.py:485`, `:673` | **Fila de evidência:** o link citado nas notas nasce da cobertura atual, não das novas chaves da ata. Transportar ID/label não prova sozinho o clique no calendário. `app/templates/colaboradores/partials/_calendario_grade.html:97`. |

No censo de `tipo_do_dia`, não encontrei outra comparação produtiva direta com `folga` além do tradutor alterado: os demais selecionam trabalho, leem faixa ou transportam o código. Li `app/ponto/services/dia_decidido.py:370`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/status_dia.py:308`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/services/cartorio.py:1987`. Isso não elimina as regressões dos leitores de **`tipo_dia`** listadas acima.

## 11/11 22:43 · sessao 1c91f963fa17 · - · - · origem=Ronald (tmux)

Prova AST executada em memória, rc=0, sem escrita. Ordem: `(abono, previsto, realizado)`, P=480/C=180 (`ponto/services/dia_pago.py:364`, base; `:390-406`, c5).

| Efeito simulado | Base, ata antiga | c5, ata nova | c5, ata antiga |
|---|---|---|---|
| `abona` | 300/480/180 | 300/480/180 | 300/480/180 |
| `None` | 300/480/180 | mapas vazios | 300/480/180 |

Ata antiga: `tipo_dia='ausencia'`, sem slug. Ata nova: `tipo_dia='folga', ausencia_tipo='folga'` (`ponto/services/cartorio.py:728`, base; `:765-767`, c5). Ocorrência produtiva [nao verificado].

## 11/11 22:42 · sessao 1c91f963fa17 · - · - · origem=Ronald (tmux)

**Achado bloqueia — dinheiro depende do cadastro atual, apesar da ata congelada.** A c5 passa a consultar `efeito_vigente(ausencia_tipo)` e exclui uma ata `tipo_dia='folga'` quando esse efeito é `None` (`ponto/services/dia_pago.py:390-399`, c5). O catálogo só carrega tipos ativos e devolve `None` para código ausente (`ponto/catalogo/ausencias.py:249`, `:293-295`).

Contraexemplo de código, **ocorrência em produção [nao verificado]**: escala prevê 480 min; ausência `folga` aprovada; batidas cobrem 180 min. Na base, a ata `ausencia` gera **abono=300, previsto=480, realizado=180** (`ponto/services/dia_pago.py:388-404`, base). Na c5, a nova ata `folga` com slug conserva esses números enquanto o tipo estiver ativo; depois de inativá-lo, os três mapas ficam vazios (`ponto/services/dia_pago.py:390-399`, c5). A inativação é permitida pela porta, invalida o cache e o próprio modelo promete preservar histórico (`ponto/services/ausencia.py:1301-1308`; `ponto/models.py:607-608`, `:1235-1238`).

A mesma regressão pode ocorrer quando `_do_banco()` captura uma falha e `vigente()` usa a semente congelada, que não contém os três tipos acrescentados somente ao cadastro do teste (`ponto/catalogo/ausencias.py:253-254`, `:278`; `ponto/tests/test_lastro_c5_sem_banco.py:166-169`, c5). Falha real de banco nesse caminho [nao verificado].

**Alcance do achado:**

- Prontidão **com snapshot** conserva o número armazenado; **sem snapshot** consulta o novo helper e, numa janela composta só desse dia, muda de previsto 8 h / 100% para `cego=True, motivo='sem_escala'` (`folha/export.py:1045-1080`, base; `:1098-1109`, c5).
- Recalcular fechamento soma os mapas e grava os totais; lavrar copia seus minutos para `DiaPago` (`ponto/services/fechamento.py:1046-1063`, c5; `ponto/services/dia_pago.py:288-292`, base).
- A exportada **também alcança o helper alterado**: `retratar_exportada()` lê-o e atualiza ou zera os três campos; sua guarda restringe apenas as competências declaradas em `GUARDA_COMPETENCIA` (`ponto/services/dia_pago.py:451-464`, `:481-495`, base). Portanto o patch não oferece isolamento geral de exportadas por construção (`ponto/services/dia_pago.py:464`, base).
- A testemunha `_gravado_x_grade()` também usa o helper alterado para distinguir divergências (`folha/porta_export.py:133-145`, base).

**Preservação demonstrável, com limite:** para ausência que antes era `tipo_dia='ausencia'`, cujo código continua encontrável com qualquer dos três efeitos válidos, a c5 preserva **P−C/P/C**, incluindo `feriado_folga`: todos os efeitos fazem `_ausencia=True` (`ponto/services/dia_pago.py:390-406`, c5). Na prontidão antiga, ausência contribuía com P de abono e zero de realizado; na nova contribui com `(P−C)+C=P`, preservando o percentual nesse universo (`folha/export.py:1100-1111`, base; `:1098-1110`, c5). Garantia na frota [nao verificado].

**Ata antiga:** sem `ausencia_tipo/id/label`, o leitor simplesmente não acrescenta essas chaves; não busca ausência atual (`escala/services/leitor_celula.py:436-438`, c5). Se seu `tipo_dia` é `ausencia`, o helper preserva o contrato anterior mesmo sem slug; se é `folga`, continua excluída. Quem decide é o ramo por rótulo de `por_dia_da_grade()` (`ponto/services/dia_pago.py:397-399`, c5).

**Sem mudança direta encontrada no TXT:** o emissor de eventos lê rubricas horárias do fechamento, não os três mapas nem o rótulo; isento retorna lista vazia (`folha/export.py:402-449`, `:417-419`, base). Isso não afasta mudanças de retenção/classificação por outros leitores [nao verificado].

**Selo insuficiente para o achado:** o teste substitui `vigente()` por um dicionário em memória; não exercita consulta real, cache, signal de inativação ou falha do catálogo (`ponto/tests/test_lastro_c5_sem_banco.py:166-169`, c5). Seus testes de prontidão passam `previsto_em=None`, deixando o ramo de snapshot fora dessa prova (`ponto/tests/test_lastro_c5_sem_banco.py:240-244`, c5). A prova AST em memória confirmou o contrato antigo sem slug e o descarte da ata nova com efeito ausente; integração ORM e ocorrência produtiva [nao verificado].

## 11/11 22:42 · sessao cb5fe192ae3f · - · - · origem=Ronald (tmux)

Referências sem sufixo são da base; “c5” indica linha resultante do patch.

**1. Bloqueia: leitor financeiro/retentor não migrou de `ausencia` para `folga`.**

`ponto/services/cartorio.py:1230` define cobertura integral por `tipo_dia in ('ausencia','falta')`. O patch muda folga humana aprovada para `folga` em `escala/utils.py:1224` c5. O cartório continua com a regra anterior.

Contraexemplo: colaborador ativo, admitido antes da janela, nenhuma batida apurável na vida, célula `trabalha=True`, previsto 480, ausência `folga` aprovada inteira em 15/10/2026 numa empresa com corte 21, primeiro julgamento ou julgamento forçado. Na base, `_coberto_inteiro_nb=True`; na c5, False. `sem_trabalho` também é False porque seu horizonte começa na competência 11 (`cartorio.py:270`, `:281-285`, `:1241-1242`). A condição em `cartorio.py:1275-1277` passa a forçar `nunca_bateu`. Esse veredito acusa (`escala/models.py:1767-1768`) e retém a folha mesmo sem protestos (`folha/export.py:1241-1252`). Ocorrência produtiva: [nao verificado].

**2. Bloqueia B1 integral: a cura não alcança automaticamente atas antigas já carimbadas.**

O patch não altera `REGRA_VERSOES` (`ponto/services/cartorio.py:132`). A grade e seu `tipo_dia` não entram na impressão; isso é explicitado em `cartorio.py:190-193`. O julgamento normal pula célula com impressão igual (`cartorio.py:1395-1401`); o ramo sem adesão também (`:1246-1252`).

Contraexemplo: célula antiga de folga humana aprovada, com ata `tipo_dia='ausencia'`, `status_dia='ausencia'`, impressão atual e nenhuma mudança de insumo. Grade viva c5 passa a `folga`; leitor da ata conserva `ausencia` (`escala/services/leitor_celula.py:397-428`); leitor de status prioriza a coluna já lavrada (`ponto/status_dia.py:287-289`). Portanto “grade == status no mesmo dia” só está demonstrado para nova lavra. O teste usa `status_sem_lavra` e chama `status_da_lavra` diretamente (`ponto/tests/test_lastro_c5_sem_banco.py:228-254` c5); não executa a guarda real do cartório. Ocorrência produtiva: [nao verificado].

**3. Ata antiga: ausência das novas chaves é mantida, não preenchida.**

O transporte c5 usa `if campo in ata` (`escala/services/leitor_celula.py:436-438` c5). Logo ausência de `ausencia_tipo`, `ausencia_id`, `ausencia_label` deixa essas chaves ausentes no dia. O leitor não consulta cobertura atual. `tipo_dia` continua vindo da ata ou, se ausente/vazio, de `cel.trabalha` (`leitor_celula.py:397-398`).

Quem decide dinheiro sem slug é `por_dia_da_grade`: seu ramo c5 preserva o contrato `trabalho/ausencia` (`ponto/services/dia_pago.py:397-400` c5). Ata antiga `ausencia`, P480/C180 continua abono300, previsto480, realizado180; ata antiga `folga` sem slug continua fora desses mapas. Não há evidência de ata antiga produzida pela base representando ausência humana com `tipo_dia='folga'`: [nao verificado].

Nova lavra substitui as chaves do cartório; a porta preserva apenas `precedencia` de outra operação (`ponto/portas/celula.py:101-104`, `:991-1006`). Não há segundo escritor persistente introduzido pelo transporte: o cartório monta dicionário (`cartorio.py:720-762` base; acréscimo c5 `:762-767`), e o caminho existente chama `cel.lavrar_veredito` (`cartorio.py:1837-1843` base).

**4. Fila/limitação da recusa: permanece captura genérica que engole `TipoSemFaixa`.**

`julgar_celula` chama `aplicar_precedencia_na_celula` dentro de `except Exception`, registra log e devolve resultado (`ponto/services/cartorio.py:2032-2040` base; c5 `:2038-2046`). Essa função chama A2 (`:1984-1985` base), cujo `faixa_de` levanta a recusa (`ponto/precedencia.py:100-105`).

Contraexemplo lógico: ausência já persistida, aberta, de tipo ativo sem mapa, não sendo `troca_de_plantao`, em dia julgado. Cobertura `para='folha'` só admite aprovada (`ponto/turnos.py:2516-2519`), então `_marcar_ausencias` pode retornar antes de perguntar A2 (`escala/utils.py:1202-1203`); o cartório pode lavrar normalmente e só a aplicação final de precedência encontra o tipo e tem a exceção engolida. Existência desse cadastro/linha em produção: [nao verificado]. A criação normal com `se_existir='veto'` já chama `veto_de_lancamento`, que passa por `faixa_de` (`ponto/services/ausencia.py:377-383`, `ponto/precedencia.py:522`); portanto não afirmar que toda criação normal permite o exemplo.

**5. O selo sem banco não exercita os riscos acima.**

O harness extrai funções e trechos AST, substitui ORM, relógio e módulos (`ponto/tests/test_lastro_c5_sem_banco.py:7-10`, `:86-93`, `:158-211` c5). O teste de ata chama `ata_do_dia`, fabrica `NS(ata=..., trabalha=True, veredito='coberta')` e passa diretamente ao leitor (`:307-326` c5). Não chama `julgar_colab`, `julgar_celula`, `lavrar_veredito`, `carimbar`, `salvar_e_emitir`, transação, evento, signals ou a guarda da impressão. O teste do fechamento compila somente `_dentro` até `_dias_inc` (`:354-373` c5); não alcança persistência nem o `except Exception` real do fechamento (`ponto/services/fechamento.py:1087-1099` base). A prontidão do harness sempre fabrica `previsto_em=None` (`test_lastro_c5_sem_banco.py:237-242` c5); snapshot não é exercitado, embora exista o ramo real em `folha/export.py:1045-1080`.

**6. Travas, relógio e porta.**

Não encontrei novo relógio ou nova trava nos acréscimos de transporte, rótulo e mapas do patch; o caminho existente usa o relógio recebido ao montar grade (`ponto/services/cartorio.py:1161`) e persiste por `carimbar`/porta (`:1839`, `:1850` aproximadamente; linha exata final desse segundo ponto não reconferida: [nao verificado]). O relógio avulso preexistente ignora parcialmente `agora=`: `julgar_celula` lê `localdate()` e `now()` nas guardas (`cartorio.py:2002`, `:2023`) antes de passar `agora` ao lote (`:2026`). Como não foi introduzido pelo patch, classificaria como fila preexistente; seu alcance concreto nesta obra: [nao verificado].

A porta faz save e evento no mesmo atomic (`ponto/portas/celula.py:899-907`). Os testes c5 não provam ordem de locks, atomicidade ou integração de eventos; ordem de trava nova/invertida: [nao verificado]. Suite inteira, opus final e pré-requisitos de pouso executados nesta volta: [nao verificado], conforme as próprias notas fornecidas.

## 11/11 22:42 · sessao 5516f8ea04bc · - · - · origem=Ronald (tmux)

Achado confirmado — **bloqueia: parcial perde cobrança ao virar `folga`**. O cadastro permite `parcial=True` para qualquer tipo pela porta genérica (`app/ponto/services/ausencia.py:1301`); o catálogo entrega essa propriedade (`app/ponto/catalogo/ausencias.py:250`) e o juiz considera parcial qualquer tipo assim cadastrado com `0 < minutos < 480` (`app/ponto/turnos.py:2585`). Contraexemplo sintético: `folga_compensatoria`, ABONA, parcial=True; ausência aprovada de 120 minutos, período 08–10; jornada 08–17, marco 17 apagado. A c5 passa a escrever `tipo_dia='folga'`, conservando `cobertura_parcial=True` (`app/escala/utils.py:1224`, c5). Entretanto, o juiz registrado só reconhece parcial se `tipo_dia=='ausencia'` (`app/chamados/juizes.py:438`); assim `dia_sem_trabalho` passa a True (`app/chamados/juizes.py:456`) e o supra deixa de considerar dia de cobrança (`app/ponto/supra_juiz.py:304`). Ocorrência desse cadastro em produção: [nao verificado].

B3 — **fila: universos continuam diferentes fora da A2**. A c5 exclui troca não aprovada da A2 (`app/ponto/precedencia.py:301`, c5), mas a cobertura de cobrança continua incluindo qualquer não rejeitada (`app/ponto/turnos.py:2518`). Uma troca aberta de dia inteiro, em jornada prevista sem batidas, continua calando `_celula_furada` antes de consultar a ata (`app/chamados/juizes.py:476`). Também continua excluída dos indicadores de falta/deslogados pela cobertura (`app/ponto/services/faltas_de_hoje.py:59`; `app/ponto/services/deslogados.py:163`). A A2 já responde trabalho, enquanto esses leitores suspendem a cobrança. Essa distinção antecede a c5; regressão financeira causada por ela: [nao verificado].

B3 — mudança concreta de classe no contador: com a mesma troca aberta, célula de trabalho e lâmpada apagada, o dia deixa de ser considerado limpo e passa a furo porque `dias_com_furo` consulta a A2 atual e aceita apenas `tipo_do_dia=='trabalho'` (`app/folha/services/sem_furo.py:20`). Portanto, retirar a troca da A2 alcança leitores fora dos testes B3; alteração de valor financeiro por esse contador: [nao verificado].

B2 — não encontrei outro leitor produtivo comparando diretamente `tipo_do_dia=='folga'` além do tradutor alterado; os demais selecionam trabalho, transportam o código ou leem a faixa (`app/ponto/services/dia_decidido.py:370`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/status_dia.py:308`; `app/ponto/services/cartorio.py:1987`). Esse censo não elimina o achado dos leitores de **tipo_dia** acima (`app/chamados/juizes.py:438`; `app/ponto/supra_juiz.py:304`).

Recusa engolida remanescente — `julgar_celula` captura toda exceção de `aplicar_precedencia_na_celula` e apenas registra log, incluindo `TipoSemFaixa` (`app/ponto/services/cartorio.py:2033`). A chamada acontece depois de `julgar_colab`, que pode ter aplicado a lavra (`app/ponto/services/cartorio.py:2026`). As três capturas corrigidas pela c5 não cobrem essa sequência real (`app/ferias/situacao.py:95`, c5; `app/escala/services/semana_do_posto.py:178`, c5; `app/ponto/services/espelho.py:651`, c5).

## 11/11 22:38 · sessao 17dfcca52004 · - · - · origem=Ronald (tmux)

Censo adicional: 41 chamadas a `reconciliar/cancelar/encerrar/encerrar_por_competencia_trancada`; `pautas/views.py:123` é falso positivo de outro domínio. As referências abaixo são linhas do HEAD; o diff c11 fornecido não altera estes arquivos/sites. [não verificado quanto à identidade Git da cópia, que não contém `.git`]

| Sítio | Função | Disparador comprovado |
|---|---|---|
| `chamados/management/commands/auditar_chamados_orfaos.py:67` | `Command.handle` | humano comando, guarda `--fechar` em `:58` |
| `chamados/reconciliador.py:496` | `encerrar_com_retencao` | humano UI, `ponto/services/justificativa.py:87`, `:400` |
| `chamados/reconciliador.py:523` | `_retratar` | cron `config/crons.py:394` → `reconciliador.py:1368`, `:1372`; signal de batida `ponto/signals.py:215`; humano comando `ponto/management/commands/reconciliar_fantasmas.py:280` |
| `chamados/services/adesao.py:115` | `sincronizar_adesao` | cron/comando `escala/management/commands/apurar_furos_diarios.py:17`, `config/crons.py:1512` |
| `chamados/services/adesao.py:150` | `sincronizar_adesao` | mesmo caminho, `fechar=True` em `escala/management/commands/apurar_furos_diarios.py:17` |
| `chamados/services/competencia_trancada.py:184` | `encerrar` | humano comando `chamados/management/commands/encerrar_por_competencia_trancada.py:29`; ato de trancar `ponto/services/fechamento.py:1428`; não cron por desenho `config/crons.py:1276` |
| `chamados/services/feriado_reavaliar.py:47` | `encerrar` | signal `chamados/signals.py:334`; porta da célula `ponto/portas/celula.py:500` |
| `chamados/services/limbo_folgas.py:101` | `aceitar_sugestao` | humano UI `chamados/views.py:433` |
| `chamados/services/limbo_folgas.py:283` | `agrupar_furo` | cron/comando `escala/management/commands/apurar_furos_diarios.py:19`; `ponto/management/commands/detectar_ausencias.py:761` |
| `chamados/services/limbo_folgas.py:286` | `agrupar_furo` | mesmo caminho; segunda tentativa após rollback, `:284` |
| `chamados/services/materializacao.py:582` | `_materializar_ausencia_de_resposta` | confirmação/materialização de resposta; chamada humana `chamados/services/validacao.py:469` [demais gatilhos não verificados] |
| `chamados/services/materializacao.py:771` | `_reconciliar_destino_e_clones._fecha` | humano UI `chamados/services/fio.py:670`, `chamados/services/acoes_disputa.py:139`; humano comando `chamados/management/commands/backfill_fio_morto_e4b.py:147` |
| `chamados/services/regularizacao_ext.py:205` | `materializar_regularizacao_externa` | humano UI `chamados/views_regularizacao.py:217`; API colaborador `api/views.py:3000`, `api/views_core.py:1322`; signal de disputa `chamados/signals.py:133` → `regularizacao_ext.py:224` |
| `chamados/services/silencio_ausencia.py:101` | `reconciliar_por_ausencia` | signal `ponto/signals.py:265`–`:269`; humano comando `ponto/management/commands/reavaliar_ausencias_lancadas.py:54` |
| `chamados/signals.py:42` | `disputa_fechada_fecha_chamado_pai` | signal `post_save` de disputa, `:18` |
| `colaboradores/management/commands/marcar_feriado_do_posto.py:140` | `Command._marcar` | humano comando `config/crons.py:1331` |
| `colaboradores/services/desligamento.py:155` | `executar_desligamento` | humano UI `colaboradores/views.py:1434`; cron `config/crons.py:440` → `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57` |
| `colaboradores/services/pos_vinculo.py:42` | `fechar_adesao_com_batida` | signal `ponto/signals.py:234`; humano UI via `colaboradores/services/vinculo.py:330` → `pos_vinculo.py:88`; humano comando `colaboradores/management/commands/corrigir_escala_retroativa.py:237` |
| `colaboradores/services/vinculo.py:387` | `_executar_aceite` | humano UI `colaboradores/views.py:423`; chat `colaboradores/services/troca_pelo_chat.py:75`; humano comando `ponto/management/commands/censo_fase_12x36.py:423` |
| `escala/management/commands/fechar_avisos_regua_na_lista.py:47` | `Command.handle` | humano comando, `--apply` em `:42` |
| `ponto/management/commands/curar_residuo_bug115.py:151` | `Command._ata_sem_furo` | humano comando `config/crons.py:1458` |
| `ponto/management/commands/detectar_ausencias.py:158` | `Command.handle` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:824` | `Command._fechar_por_afastamento` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:834` | `Command._fechar_por_cobertura` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:845` | `Command._fechar_chamado` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_intervalo_ausente.py:138` | `Command.handle` | humano comando; cron usa `--alarme`, `config/crons.py:491`, que força dry em `detectar_intervalo_ausente.py:107` |
| `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` | `Command.handle` | humano comando, `--apply` em `:52` |
| `ponto/services/ausencia.py:694` | `fechar_chamado_da_ausencia` | signal `ponto/signals.py:266`, `:271` → `ausencia.py:729` |
| `ponto/services/cartorio.py:986` | `julgar_colab` | cron `config/crons.py:616`; outros chamadores [não verificado] |
| `ponto/services/cartorio.py:1511` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1524` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1545` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1623` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1638` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cluster_espurio.py:124` | `reconciliar_colab` | cartório `ponto/services/cartorio.py:1016`; humano comando `ponto/management/commands/detectar_cluster_espurio.py:46` |
| `ponto/services/cobertura.py:122` | `designar_cobertura` | humano UI `ponto/views.py:3048` |
| `ponto/services/cobertura.py:317` | `declarar_cobertura_passada` | chamador de produção não encontrado no `rg` por nome; disparador [não verificado] |
| `ponto/services/entrada_sincronizada.py:31` | `resolver_cobranca_da_entrada` | API colaborador `api/views.py:803`, `api/views_core.py:798` |
| `ponto/services/lastro.py:400` | `fechar_pelo_marco_colab` | cron consumidor `config/crons.py:394` → `chamados/reconciliador.py:1383` |
| `ponto/services/triagem_batida.py:597` | `_autoresolve_f2_f4` | UI colaborador `ponto/services/triagem_batida.py:753`, `ponto/views.py:349` |
| `ponto/services/veredito_celula.py:202` | `fechar_o_fio_do_dia` | humano UI `ponto/services/veredito_celula.py:346`, `ponto/services/declarar_dia.py:206`, `chamados/services/validacao.py:296` |

Os alvos dos 41 sites são: `reconciliar` → `resolvido`, pelo gate em `chamados/models.py:641`, `:654`; `encerrar` → `fechado`, `models.py:1605`, `:1619`; `cancelar` → decisão `CANCELAR`, `models.py:783`, `:788`; tranca → `fechado`, `models.py:1632`, `:1639`. O catálogo declara `resolvido/fechado/cancelado/superado` terminais e não declara um estado literal `encerrado`, em `chamados/catalogo/motor.py:19`, `:30`, `:31`.

**Provas e limites da classificação:**

- Todos os `reconciliar/encerrar` genéricos acima ficam **sem-prova**: a guarda de pergunta viva ignora competência entregue cujo resultado seja diferente de `False`, em `chamados/models.py:364`, `:365`; a documentação diz explicitamente que exportada fica fora em `:328`. O S143 percorre `_ancoradas()` e chama a reabertura em `chamados/services/disputa_emissao.py:1938`, `:1945`, registrando recusas em `:1946`, `:1950`.
- O site de tranca tem **não quanto à recusa S143**: escreve `via=VIA_COMPETENCIA_TRANCADA` em `chamados/models.py:1639`; o resolver retorna antes do laço de reabertura em `chamados/services/disputa_emissao.py:1899`, `:1900`. Também mede vivas pelas duas relações antes de selecionar chamados em `chamados/services/competencia_trancada.py:139`, `:142`, `:146`.
- Os dois sites de `cancelar` têm **não quanto à recusa S143**: escrevem `cancelado`, que não pertence a `ENCERRADOS` (`chamados/catalogo/motor.py:30`, `:31`); o receiver retorna em `chamados/signals.py:96`. Isso não prova ausência de pergunta viva exportada.
- `chamados/migrations/0039_a3_via_backfill.py:24` escreve `superado`; `:33` escreve `cancelado`. `RunPython` liga ambos à migração em `:39`; `.update()` não emite o `post_save`, conforme `chamados/signals.py:95`. Não excluir essas linhas do inventário fora de testes.
- `smoke_chamados.py:134` é um site dinâmico que pode alcançar terminal; `:147` só reabre, por destinos literais de `:144`; `:163` só tenta destinos de reabertura/mesmo aberto listados em `:158`. O comando recusa banco sem `test` no nome em `:32`–`:35` e força rollback em `:39`–`:44`.

**Leitores comuns / resolver:** os saves de `_gravar_estado` nomeiam `status_local` em `chamados/models.py:229`; S143 filtra encerrado e coluna salva em `chamados/signals.py:96`, `:104`, chama o resolver em `:116`. Outros leitores principais: rebaixa urgência em `signals.py:70`; C6 lê destino encerrado em `chamados/juizes.py:760`; fila/painel em `chamados/services/painel.py:155`, `:217`, `:218`; fio em `chamados/services/fio.py:432`, `:771`. `_retratar` ainda chama o resolver diretamente em `chamados/reconciliador.py:536`, além do signal.

**Obstáculos para trechos mínimos:**

| Grupo | Obstáculo comprovado |
|---|---|
| Todos os automáticos | Trocar por `fechar()` ativa perfil e guards humanos: `chamados/models.py:130`, `:136`, `:148`, `:181`; perde a via do evento original porque `_aplicar` escreve `TRANSICAO_HUMANA`, `:449`, `:450`. |
| Todos os `if reconciliar()` | `transicionar_com_pauta` devolve número de Pautas, inclusive zero após fechar, em `chamados/services/fecho_runtime.py:159`; não pode substituir como retorno booleano. `reconciliar` devolve falso/verdadeiro em `models.py:652`, `:655`. |
| Reconciliar | Preservar premissa, dedup e reata em `chamados/models.py:682`–`:692`; fechamento humano recusa sem reatar em `:165`–`:171`. |
| Desligamento | Atomic externo `colaboradores/services/desligamento.py:103`; disputa fecha antes do chamado em `:139`–`:155`; cron pode não ter user em `:70`–`:75`. |
| Regularização externa | Atomic externo `chamados/services/regularizacao_ext.py:121`; status do lote salvo antes do chamado em `:197`–`:205`; retomada pode vir de signal/on_commit `chamados/signals.py:133`–`:145`. |
| LIMBO agrupamento | Atomic e ordem global `chamados/services/limbo_folgas.py:270`–`:274`; savepoint e segunda tentativa `:277`–`:288`; idempotência inicial `:252`. |
| Materialização | Pergunta em voo e atomic do materializador `chamados/services/materializacao.py:751`–`:754`; retenção de irmãs antes do fechamento `:762`–`:771`. |
| Lastro | Fecho + nota no mesmo atomic `ponto/services/lastro.py:399`–`:402`. |
| Cobertura | Atomic externo `ponto/services/cobertura.py:99`, `:283`; user existe na assinatura `:22`, `:246`. |
| Marcar feriado | Autor pode ser `None` no lookup `colaboradores/management/commands/marcar_feriado_do_posto.py:127`; atomic por chamado em `:139`. |
| Tranca | Via própria com efeito de não resolver, `chamados/models.py:1639`, `chamados/services/disputa_emissao.py:1899`; não migrar ingenuamente para humano. |

A solução mínima que preserve comportamento dos automáticos exige permitir à porta existente envolver a execução do gate original, preservando retorno e via; uma simples troca de chamada por `fechar()` altera guardas e idempotência pelos pontos acima. Isso é proposta de implementação [não verificado], sem necessidade demonstrada de juiz novo.

_4068 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
