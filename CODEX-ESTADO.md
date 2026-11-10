# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 18:52** · publicado em **10/11/2026 18:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 18:52 · sessao 28e142e5e8be · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Corrigidos os dois pontos: [guarda de donas visitadas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:619) e [dia indeterminado classificado como I7a](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/management/commands/auditar_invariantes_chamados.py:69).

A [entrega VOLTA 4](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:136) registra comandos, RED contra a base, três mutantes mortos, 37 testes de memória e 35 da catraca verdes. O deslocamento de nove linhas foi [registrado com justificativa](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267). [nao verificado]

FALTA certificação ORM/SQL pelo Code e revisão final Opus [nao verificado].

## 10/11 18:51 · sessao d1cb93410c4e · - · - · origem=Ronald (tmux)

Correção da linha âncora: retorno estrangeiro não é estado real nesta cópia. `abrir_conversa` sempre insere pergunta na DS recebida; o dedup canônico só roda para `conversa=False`. Portanto, sucesso da âncora persiste P local; exceção da porta desfaz pai/DS no atomic externo. Push e retorno estão dentro desse atomic. (`app/chamados/models.py:2794`, `:2801`, `:2893`, `:3008`, `:3014`, `:3017`; `app/chamados/services/disputa_emissao.py:1416`, `:1465`, `:1475`, `:1476`)

## 10/11 18:50 · sessao d1cb93410c4e · - · - · origem=Ronald (tmux)

Censo de nascimento: três `DisputaSupervisao.objects.create` no código de aplicação fora de testes/migrations: núcleo, órfãos e âncora; as rotas de declaração e grade delegam ao núcleo. (`app/chamados/services/disputa_emissao.py:643`, `:1217`, `:1452`, `:722`; `app/ponto/management/commands/reconciliar_grade.py:424`)

| Caminho | Operação e persistido | Retorno/push | Prova | [nao verificado]
|---|---|---|---|
| Grade `_disputa` | Chama núcleo com `motivos=[]`, contexto `{'origem':'grade'}`; nascendo, pai e DS ficam sem P porque laço do gerador recebe lista vazia. (`app/ponto/management/commands/reconciliar_grade.py:424`; `app/chamados/services/disputa_emissao.py:643`, `:650`, `:403`) | Retorna `d`; helper não chama push. Não há caller de `_disputa` neste comando; fluxo de `handle` chama `_emitir_furo`, que cria/reabre apenas Chamado e retorna. (`app/ponto/management/commands/reconciliar_grade.py:428`, `:254`, `:488`, `:507`, `:529`, `:531`) | Memória testa núcleo diretamente: `test_bo_disputa_sem_pergunta_memoria.py::test_chamada_grade_motivos_vazios_conta_estrutura`; comando inteiro para este nascimento: AUSENTE. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:421`) |
| Órfãos — elegibilidade | Remove dias na lista do admin antes atomic; vazio retorna None. Dentro atomic trava fecho C/D/Ch/P. Pode complementar perguntas em dona preexistente e analisar Chamado antes de retornar None no segundo filtro. (`app/chamados/services/disputa_emissao.py:1141`, `:1143`, `:1153`, `:1164`, `:1175`, `:1177`, `:1179`, `:1187`) | None pode coexistir com complementos persistidos; não implica ausência de escrita. (`app/chamados/services/disputa_emissao.py:1175`, `:1188`) | Prova específica retorno None com complemento: AUSENTE [nao verificado]. |
| Órfãos — nascimento normal | Seleciona primeira DS aberta; se ausente abre pai manual e cria DS com motivos existentes ou `['orfao_14h']`, contexto origem/IDs e FK pai. Ramo intervalo pergunta por marcos com data parseável; demais por ata, batida-origem ou contexto. (`app/chamados/services/disputa_emissao.py:1190`, `:1194`, `:1198`, `:1217`, `:1220`, `:1232`, `:1242`, `:1262`, `:1288`, `:1325`) | Ao fim pergunta pendentes, consulta `pode_fechar`; permanece aberta se não pode fechar; push se `notificar`, dentro atomic, com DS local/motivos atualizados. (`app/chamados/services/disputa_emissao.py:1364`, `:1372`, `:1383`, `:1385`) | `test_pergunta_no_app.py::test_MORDE_col709_dia_mais_velho_ganha_a_pergunta_do_marco_apagado`, `::test_duas_passadas_uma_pergunta`. (`app/chamados/tests/test_pergunta_no_app.py:89`, `:103`) |
| Órfãos — zero P fora HX | Continua sem pergunta quando marco intervalo inválido, dia do fato ausente, falta de base, juiz sem falta ou veto ausência/fio; motivo pode entrar antes do veto. Se `pode_fechar` e não houve abstenção por ata, persiste DS FECHADA e pai; não desfaz nascimento. (`app/chamados/services/disputa_emissao.py:1244`, `:1300`, `:1327`, `:1329`, `:1332`, `:922`, `:924`, `:926`, `:1376`, `:1378`) | Retorna DS fechada antes do push. Literal “nasce com pergunta ou não nasce”: NÃO. (`app/chamados/services/disputa_emissao.py:1382`, `:1384`) | `test_hx_disputa_muda_sem_ata.py::test_com_ata_e_dia_completo_fecha_como_antes` exige zero P e DS fechada; `test_hx_disputa_muda.py::test_nao_notifica_disputa_muda` exige ausência de push. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63`, `:77`, `:78`; `app/chamados/tests/test_hx_disputa_muda.py:44`, `:50`) |
| Órfãos — sem ata HX | Pai/DS já existem; `_ata_do_dia` chama `_marcos_faltantes`; estado sem_celula com escala vigente acende e recusa perguntas. Mantém DS aberta zero P, acende auto-fecho. Sem escala vigente, gate permite seguir; exceção de leitura também permite seguir. (`app/chamados/services/disputa_emissao.py:1217`, `:990`, `:995`, `:996`, `:1003`, `:1005`, `:1267`, `:1305`, `:1336`, `:1373`, `:1375`) | Se `notificar=True`, tenta push mesmo zero P; teste HX usa `notificar=False`. Literal invariante: NÃO, exceção explicitamente aprovada pelo dado do usuário. (`app/chamados/services/disputa_emissao.py:1383`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:55`) | `test_hx_disputa_muda_sem_ata.py::test_sem_ata_nao_fecha_e_acende` exige zero P/aberta/warning. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, `:58`, `:59`, `:61`) |
| Órfãos — retorno estrangeiro | `_criar_pergunta_se_couber` chama `PerguntaDisputa.abrir` e ignora `(pergunta,criada)`, devolvendo `True`; não transfere motivo nem troca DS local pela dona. Chamados podem ser analisados como se pergunta local tivesse nascido. (`app/chamados/services/disputa_emissao.py:931`, `:941`, `:1252`, `:1271`, `:1314`, `:1317`, `:1345`) | Resultado/push referem-se à DS local; pode fechar local zero P ou conservá-la conforme `pode_fechar`/HX. Literal invariante DEPENDE de outra P local. (`app/chamados/services/disputa_emissao.py:1372`, `:1373`, `:1376`, `:1384`) | Combinação específica com dona estrangeira: AUSENTE [nao verificado]. |
| Órfãos — universo mudou | `_trava_do_repasse` recusa crescimento do conjunto; atomic desfaz tentativa; captura fora grava evento `repasse_recusado`. (`app/chamados/services/disputa_emissao.py:1097`, `:1100`, `:1108`, `:1110`, `:1387`, `:1390`) | `REPASSE_RECUSADO`; outras exceções não são capturadas nesse handler. (`app/chamados/services/disputa_emissao.py:1393`) | Provas específicas podem estar em testes de repasse; não auditadas [nao verificado]. |
| Âncora — nova | Guarda categoria/status e P não materializada por destino; trava universo aberto; abre/reusa pai manual, cria DS com `ancora_ausente`, contexto origem/id do limbo; abre conversa `data_hora` com destino limbo, analisa limbo. (`app/chamados/services/disputa_emissao.py:1410`, `:1412`, `:1417`, `:1425`, `:1433`, `:1452`, `:1455`, `:1465`, `:1469`, `:1471`, `:1473`) | Push da DS local dentro atomic, depois retorna DS. Literal invariante SIM se P ficou nela. (`app/chamados/services/disputa_emissao.py:1475`, `:1476`) | `test_fio_ancora.py::test_01_disparo_gera_pergunta_sem_batida`, `::test_02_idempotente`. (`app/chamados/tests/test_fio_ancora.py:69`, `:78`, `:88`, `:95`) |
| Âncora — estrangeira | Ignora retorno de `abrir_conversa`; DS/pai local persistem mesmo que conversa pertença a outra DS, salvo exceção externa. Analisa limbo e notifica pela local. (`app/chamados/services/disputa_emissao.py:1452`, `:1465`, `:1473`, `:1475`) | Retorna local; literal invariante NÃO se local sem outra P. (`app/chamados/services/disputa_emissao.py:1476`) | AUSENTE [nao verificado]. |
| Declaração helper isolado | `disputa_para_declaracao` chama núcleo com `emitir_perguntas=False`, contexto origem, merge None. Pode persistir pai/DS sem P; não gera nem envia push. (`app/chamados/services/disputa_emissao.py:709`, `:712`, `:714`, `:722`, `:725`, `:726`) | Retorna DS local. Literal invariante NÃO no limite desse helper. (`app/chamados/services/disputa_emissao.py:727`) | `test_porta_disputa_emissao.py::test_declaracao_sem_disputa_aberta_faz_nascer_uma_com_pai_e_trilha`, `::test_MORDE_a_declaracao_nao_manda_push_nem_varre_pergunta`. (`app/chamados/tests/test_porta_disputa_emissao.py:185`, `:189`, `:223`, `:230`, `:231`) |
| Declaração consumidores | Ambos têm atomic externo, trava antes do helper e abrem P depois. `_candidata_reusavel` só retorna canônica na própria DS; estrangeira resulta None e chamada de `abrir` pode devolvê-la. Consumidores não conferem dona nem eliminam DS local. Retornos normais de erro dentro atomic também confirmam escrita anterior. (`app/chamados/services/declaracao_texto.py:348`, `:352`, `:360`, `:250`, `:254`, `:376`, `:412`, `:421`, `:654`, `:656`, `:659`, `:666`, `:683`, `:695`) | Declaração colab retorna dicionário; admin valida depois do atomic usando `disputa=d` local. Sem push do helper. Invariante DEPENDE de P retornada pertencer a d. (`app/chamados/services/declaracao_texto.py:426`, `:696`, `:697`; `app/chamados/services/disputa_emissao.py:712`) | Helper coberto acima; combinação consumidor+canônica estrangeira: AUSENTE [nao verificado]. |
| Fábrica | `perguntar` desfaz carimbo, chama wrapper e chama gerador novamente depois do núcleo; não é outro create direto. Julgamento envolve chamada em silêncio de push. (`app/chamados/services/fabrica_da_celula.py:169`, `:172`, `:174`, `:218`, `:219`) | `perguntar` retorna contagem de desfeitos; pós-geração pode falhar após núcleo já concluído, salvo atomic externo. (`app/chamados/services/fabrica_da_celula.py:175`) | Cobertura específica da segunda geração com falha: AUSENTE [nao verificado]. |
| Smoke | Comando chama núcleo e gera outra vez depois; não há create DS separado. (`app/chamados/management/commands/smoke_chamados.py:99`, `:102`) | Exceção vira contador/problema no comando. (`app/chamados/management/commands/smoke_chamados.py:114`, `:116`) | É bancada do próprio comando; prova da atomicidade pós-núcleo AUSENTE [nao verificado]. |

Push sempre usa `disputa.colaborador`, URL `/meu-atendimento/`, sem FK/ID da dona na URL; não consulta fechamento nem existência de P. `_PUSH_SILENCIADO` suprime antes das chamadas WebPush/FCM; exceptions são registradas e engolidas. Entrega efetiva [nao verificado]. (`app/chamados/services/disputa_emissao.py:1845`, `:1847`, `:1861`, `:1862`, `:1865`, `:1869`, `:1870`, `:1872`)

O código não realiza push por dona em lote: o wrapper usa a DS única retornada pelo núcleo; nascimento manda `nova=True`; agregação chama pendentes e manda somente quando `novos` não vazio. (`app/chamados/services/disputa_emissao.py:765`, `:766`, `:779`, `:780`, `:781`)

## 10/11 18:50 · sessao 5c96eb64a4b6 · - · - · origem=Ronald (tmux)

**Efeitos da dona fechada.** O núcleo salva `motivos`/`contexto` antes de chamar o gerador, preservando diretamente `fechada_em`; o `save` aciona os receivers mesmo sem transição de estado. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`, `:609`)

- O receiver tenta reconciliar o pai quando ele existe e não está em `ENCERRADOS`; pode acrescentar ocorrência de fechamento. (`app/chamados/signals.py:28`, `:33`, `:42`, `:45`)
- Outro receiver agenda `retomar_regularizacoes_confirmadas` em `on_commit`, selecionando regularizações confirmadas sem materialização daquele colaborador. (`app/chamados/signals.py:129`, `:137`, `:144`; `app/chamados/services/regularizacao_ext.py:218`, `:221`, `:224`)
- Outro chama `materializar_pendentes_aprovadas`, que percorre perguntas não materializadas e carimba somente as aprovadas pelo juiz e pela porta. (`app/chamados/signals.py:153`, `:163`; `app/chamados/services/materializacao.py:1418`, `:1427`, `:1431`)
- O persistido adicional exato depende de pai, perguntas e regularizações preexistentes: [nao verificado]. O teste de reuso fechado substitui `abrir_conversa` e o push; não demonstra todos esses efeitos. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:68`, `:81`, `:82`)

**Push e retorno com várias donas.** A recuperação retorna somente a primeira dona processada e a união dos motivos novos das diversas agregações. (`app/chamados/services/disputa_emissao.py:687`, `:695`, `:697`, `:698`)

- Se nasceu: wrapper chama push com todos os motivos e `nova=True`. (`app/chamados/services/disputa_emissao.py:765`, `:766`)
- Se agregou/reusou: wrapper chama primeiro `perguntar_entradas_pendentes` sobre essa única dona; depois envia push se a união `novos` não for vazia. Assim, motivos novos de outras donas podem compor o aviso associado à primeira. (`app/chamados/services/disputa_emissao.py:779`, `:780`, `:781`; `:695`, `:698`)
- O helper de push não verifica `fechada_em`; tenta web push e FCM para o colaborador e usa URL genérica `/meu-atendimento/`, sem ID de dona no endereço. Silêncio contextual suprime ambos. Entrega real: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1847`, `:1855`, `:1861`, `:1865`, `:1869`)
- Órfãos enviam somente com `notificar`, após a tentativa de fecho automático; quando esse fecho ocorre, retornam antes do push. (`app/chamados/services/disputa_emissao.py:1376`, `:1382`, `:1383`, `:1384`)
- A fábrica de perguntas por célula usa `push_disputa_silenciado`; ainda chama o gerador novamente depois do wrapper. (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:217`, `:218`, `:219`)

**Exceção após batida.** A API grava a batida por `registrar_batida` antes de chamar a emissão. O núcleo captura somente `_PerguntaDeOutraDisputa`; exceções do gerador atravessam o `atomic` e impedem o wrapper de chegar ao push. (`app/api/views_core.py:681`, `:794`; `app/chamados/services/disputa_emissao.py:546`, `:547`, `:650`, `:673`, `:765`)

A prova ORM existente cria batida antes da emissão, exige preservação dela, igualdade das contagens de DS/Chamado/Pergunta/log e ausência de push; outro teste falha depois de criar pergunta e exige rollback. Execução ORM nesta sessão: [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py::test_gerador_levanta_desfaz_disputa_pai_e_pergunta_mas_preserva_batida`, linhas `32–53`; `::test_gerador_falha_apos_pergunta_desfaz_as_duas`, linhas `55–66`)

Não há retry genérico nessa captura: quem/quando reemitirá **o mesmo episódio** é [nao verificado]. Existem entradas futuras condicionais: nova chamada API reexecuta detector e emissão; cartório pergunta somente pelas células julgadas naquele ato; cron de foto percorre batidas ainda sem selfie cobrável às 07:26. Isso não demonstra recuperação universal do episódio perdido. (`app/api/views_core.py:433`, `:794`; `app/ponto/services/cartorio.py:992`, `:1007`, `:1008`; `app/chamados/services/fabrica_da_celula.py:207`, `:219`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `:96`; `app/config/crons.py:686`)

**Ciclo.** A cópia mantém recursão na recuperação e não contém guarda de visitadas no corpo; o teste “dona diferente” compara apenas a pergunta retornada com a DS da tentativa atual. Um ciclo A→B→A não encontra limite explícito; persistência final depende da saída/erro e dos `atomic` envolventes. Reprodução com ORM: [nao verificado]. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:676`, `:690`, `:698`)

**Auditor.** I7a/I7b contam somente DS abertas com zero linhas de perguntas; pergunta morta também conta como linha e exclui a DS desses números. (`app/chamados/management/commands/auditar_invariantes_chamados.py:61`, `:63`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:100`, `:111`, `:112`)

O auditor descarta motivos sem resolução/dia; portanto um motivo indeterminado mais outro dia `sem_celula` pode acabar em I7b. `bool(dias)` protege apenas o conjunto totalmente vazio. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:51`, `:53`, `:69`, `:70`)

A cena HX exige DS existente, zero perguntas, aberta e warning; BO a incorpora e exige `(I7a,I7b)=(0,1)`. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:55`, `:56`, `:58`, `:59`, `:61`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:95`, `:97`, `:98`)

**Limite das provas.** As suites de memória declaram que não certificam PostgreSQL, signals nem a suite Django. O arquivo de teste `test_e1_ordem_de_trava.py` citado no comentário do núcleo está AUSENTE nesta cópia; não o usar como prova executável. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:1`, `:3`; `app/chamados/services/disputa_emissao.py:553`; presença do arquivo: [nao verificado])

## 10/11 18:50 · sessao 0425b48d5ffb · - · - · origem=Ronald (tmux)

**Conclusão:** a substituição da exceção de controle e da reentrada cabe no serviço de emissão, sem alterar `chamados/models.py`, desde que o planejamento use a identidade completa da porta e congele cada fato antes da escrita. “Nenhuma exceção possível” não cabe: a porta mantém recusas deliberadas e transacionais. (`app/chamados/services/disputa_emissao.py:673`, `app/chamados/models.py:2840`, `app/chamados/models.py:2917`, `app/chamados/models.py:2954`, `app/chamados/models.py:2819`)

| Questão | Resposta fundamentada |
|---|---|
| Quem responde a dona sem escrever? | `fio.universo_do_fato(colaborador_id, motivo, dia, batida_origem_id=...)`, seguido de `fio.canonica_do_grupo(linhas).disputa`; lista vazia significa nenhuma canônica naquele universo. O universo é explicitamente o mesmo usado pela porta, e a canônica prefere resposta do colaborador, depois menor PK. (`app/chamados/services/fio.py:1784`, `app/chamados/services/fio.py:1794`, `app/chamados/services/fio.py:1831`, `app/chamados/services/fio.py:1225`) |
| `PerguntaDisputa.do_fato` basta? | Não: fornece as linhas iniciais da chave; a porta usa o fecho limitado pelo dia e elege um componente do fato. Há identidade por episódio e acervo sem episódio. (`app/chamados/models.py:2717`, `app/chamados/models.py:2900`, `app/chamados/models.py:2907`, `app/chamados/services/fio.py:1826`) |
| `_candidata_reusavel` responde dona global? | Não: elege no universo completo, mas só devolve a canônica se pertence à disputa recebida e satisfaz a reusabilidade. Pode retornar `None` com canônica existente noutra disputa. (`app/chamados/services/declaracao_texto.py:230`, `app/chamados/services/declaracao_texto.py:250`, `app/chamados/services/declaracao_texto.py:254`) |
| `(colaborador,motivo,dia)` sempre identifica sozinho? | Não para a família BATIDA: episódio conhecido participa da consulta; dois componentes distintos podem existir no mesmo dia. O planejamento deve levar episódio, instância e dia normalizado pela mesma autoridade da porta. (`app/chamados/models.py:2712`, `app/chamados/models.py:2866`, `app/chamados/models.py:2875`, `app/chamados/services/fio.py:1826`) |
| O helper atual de resolução basta? | Não: retorna texto/destino/dia e recusa alguns casos, mas não consulta a canônica global; contém dedup local que retorna `None`. Outras guardas continuam no gerador: catálogo, dedup não datado, hora sem dia e conversa repetida. (`app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:361`, `app/chamados/services/disputa_emissao.py:380`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:415`, `app/chamados/services/disputa_emissao.py:438`, `app/chamados/services/disputa_emissao.py:449`) |
| Leitura prévia sem trava equivale à porta? | Não oferece a proteção que a porta usa: ela trava o colaborador antes do dedup; o teste de corrida exige uma única pergunta e retornos `criada=True/False` para duas entradas concorrentes. (`app/chamados/models.py:2845`, `app/chamados/tests/test_e1_identidade_corrida.py:74`, `app/chamados/tests/test_e1_identidade_corrida.py:81`) |
| Leitura sob C no mesmo atomic resolve tudo? | Protege contra escritores que seguem a mesma trava, mas o plano precisa reproduzir a identidade completa e manter os insumos: a porta recalcula episódio/dia, pode rechavear a canônica e pode recusar. Equivalência integral numa implementação ainda inexistente: [nao verificado]. (`app/chamados/models.py:2866`, `app/chamados/models.py:2875`, `app/chamados/models.py:2940`, `app/chamados/models.py:2954`) |
| Pergunta morta segura identidade? | “Morta” não basta como filtro: a identidade exclui especificamente `duplicata` e `motivo_retratado`; fechamento da disputa e demais vias não estão nessa exclusão. Filtrar previamente por pergunta viva ou disputa aberta divergiria da porta. (`app/chamados/models.py:2393`, `app/chamados/models.py:2754`, `app/chamados/models.py:2921`) |
| Rechaveamento pode desaparecer do desenho? | Não: reuso com dia gravado diferente passa pela porta de rechaveamento; competência desconhecida/exportada, colisão e outras recusas continuam. O “dry” existente roda escrita real em atomic desfeito, portanto não é uma leitura pura. (`app/chamados/models.py:2940`, `app/chamados/services/fio.py:1056`, `app/chamados/services/fio.py:1068`, `app/chamados/services/fio.py:1077`) |

A ordem exige **C → todas as DS necessárias → todos os Chamados necessários → todas as Perguntas necessárias**, com PK crescente por grupo, antes da escrita que possa disparar cascatas; usar apenas chamadas individuais aparentemente ordenadas não certifica a transação inteira. O topo atual inclui DS abertas, DS explicitamente pedidas, pais das abertas e Chamados vivos, mas não inclui automaticamente destino terminal de canônica fechada. No rechaveamento, `_nascer` pede tanto o destino recebido quanto o destino anterior e a pergunta. Num lote, é necessário antecipar esse universo antes da primeira Pergunta. (`app/chamados/models.py:2631`, `app/chamados/models.py:2675`, `app/chamados/models.py:2677`, `app/chamados/models.py:2679`, `app/chamados/models.py:2683`, `app/chamados/models.py:2692`, `app/chamados/models.py:2947`)

Mudanças de desenho necessárias, sem código de produção:

- `disputa_emissao.py::_abrir_ou_agregar_disputa`: planejar cada fato sob C, particionar pelas donas e escrever uma vez; retirar `_PerguntaDeOutraDisputa` como mecanismo de descoberta/reentrada. Hoje a descoberta ocorre depois de salvar a DS e gerar perguntas, seguida de rollback e chamada recursiva. (`app/chamados/services/disputa_emissao.py:607`, `app/chamados/services/disputa_emissao.py:609`, `app/chamados/services/disputa_emissao.py:621`, `app/chamados/services/disputa_emissao.py:673`, `app/chamados/services/disputa_emissao.py:690`)
- `disputa_emissao.py::_resolver_pergunta_por_motivo` e `::gerar_perguntas_disputa`: separar resolução de insumos da decisão de emissão/dedup e consumir o plano congelado; recalcular a data no contexto de cada dona reproduz a condição do ciclo A↔B. Hoje a data vem do contexto da DS usada em cada entrada. (`app/chamados/services/disputa_emissao.py:335`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:434`, `app/chamados/services/disputa_emissao.py:585`, `app/chamados/services/disputa_emissao.py:690`)
- `disputa_emissao.py::criar_ou_atualizar_disputa`: tratar o resultado de várias donas para pendentes e push; hoje o núcleo devolve somente a primeira dona e o conjunto agregado de motivos novos. (`app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:698`, `app/chamados/services/disputa_emissao.py:779`, `app/chamados/services/disputa_emissao.py:781`)
- `auditar_invariantes_chamados.py::_dias_da_disputa_sem_pergunta`: continuar consumindo a fonte comum de resolução caso a assinatura/resultado do helper mude; hoje lê diretamente a posição `[2]`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `app/chamados/management/commands/auditar_invariantes_chamados.py:52`)
- Não mudar `models.py` para contornar identidade, rechaveamento, contrato de saída ou recusas: a API pública já aceita a dona escolhida como `disputa`, preserva a FK da canônica reusada e mantém esses controles. (`app/chamados/models.py:2776`, `app/chamados/models.py:2789`, `app/chamados/models.py:3008`, `app/chamados/models.py:3014`)

Os testes existentes exigem preservar: [nao verificado]

| Exigência | Teste | [nao verificado]
|---|---|
| Canônica do fecho, não filtro local | `app/chamados/tests/test_e1_v13.py:88::test_RED_abrir_alfa_segura_a_canonica_do_fecho`; `app/chamados/tests/test_e1_v13.py:98::test_RED_candidata_reusavel_segura_a_canonica_do_fecho`. |
| Um componente por fato, inclusive episódios/acervo | `app/chamados/tests/test_e1_v13.py:346::test_RED_porta_sem_episodio_e_um_componente`. |
| Rechavear no mesmo ato e idempotência | `app/chamados/tests/test_e1_v15d.py:60::test_RED_R1_reuso_rechaveia_no_mesmo_ato_e_2x_e_o_mesmo_estado`. |
| Recusa sem escrita: sem célula, colisão, competência exportada | `app/chamados/tests/test_e1_v15d.py:84::test_RED_R2_dia_novo_sem_celula_recusa_declarada_sem_escrever`; `:101::test_RED_R3_colisao_com_outra_linha_do_fato_recusa`; `:124::test_RED_R4_competencia_exportada_no_dia_velho_ou_novo_recusa`. |
| Corrida produz uma pergunta e mesmo PK | `app/chamados/tests/test_e1_identidade_corrida.py:74::test_MORDE_duas_na_janela_nascem_UMA_pergunta`; `:81::test_MORDE_a_segunda_recebe_criada_False`. |
| Ordem real linha a linha | `app/chamados/tests/test_e1_v15f.py:117::test_RED_reuso_com_rechave_e_costura_trava_na_ordem`; definição de violação em `app/chamados/tests/test_e1_v9.py:96`. |
| Só `trava_na_ordem` adquire travas | `app/chamados/tests/test_e1_v7_catracas.py:387::test_zero_trava_fora_de_trava_na_ordem`. |
| Canônica eleita sobre universo autorizado | `app/chamados/tests/test_e1_v7_catracas.py:447::test_zero_canonica_fora_do_universo`. |
| Declaração não envia push nem chama gerador | `app/chamados/tests/test_porta_disputa_emissao.py:223::test_MORDE_a_declaracao_nao_manda_push_nem_varre_pergunta`. |
| Urgência normal e contexto preexistente conservado na declaração | `app/chamados/tests/test_porta_disputa_emissao.py:194::test_MORDE_a_urgencia_da_declaracao_e_normal_nao_emergencia`; `:199::test_disputa_aberta_agrega_o_motivo_e_NAO_toca_o_contexto`. |

Não cabe prometer ausência absoluta de exceções, usar `do_fato` sem o universo, tomar `_candidata_reusavel` como dona global, filtrar canônicas por vitalidade, nem usar `rechavear_no_reuso(dry=True)` como leitura pura. Esses atalhos conflitam com os contratos já escritos. (`app/chamados/models.py:2905`, `app/chamados/services/declaracao_texto.py:254`, `app/chamados/models.py:2754`, `app/chamados/services/fio.py:1077`)

Certificação ORM, corrida efetivamente executada, SQL total, efeitos de receivers e validade de uma implementação nova permanecem [nao verificado].

## 10/11 18:41 · sessao cec9bd295d6b · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não liberaria ainda:** a reentrada pode ciclar entre donas; o auditor pode publicar I7b descartando um motivo sem dia; e continuam gerações posteriores à transação do núcleo. (`app/chamados/services/disputa_emissao.py:690`, `:779`, `:294`; `app/chamados/management/commands/auditar_invariantes_chamados.py:51`, `:69`; `app/chamados/services/fabrica_da_celula.py:174`)

1. **Estado dos achados da `preverif1.r`**

   | Achado anterior | Estado atual | Linha que decide |
   |---|---|---|
   | 1. Geração posterior ao núcleo | **Igual.** Principal protegida; pendentes e fábrica continuam depois. | `app/chamados/services/disputa_emissao.py:609`, `:650`, `:779`, `:294`; `app/chamados/services/fabrica_da_celula.py:174` |
   | 2. Retorno estrangeiro ignorado na agregação | **Curado nesse ponto.** Agora detecta e levanta; persistem limitações de reentrada descritas abaixo. | `app/chamados/services/disputa_emissao.py:612`, `:621` |
   | 3. Batida anterior ao atomic; resposta HTTP não garantida | **Igual.** A disputa continua posterior à gravação; o núcleo captura somente a exceção específica de reuso. | `app/api/views_core.py:681`, `:794`; `app/api/views.py:684`, `:794`; `app/chamados/services/disputa_emissao.py:673` |
   | 4. HX preservada | **Igual, corretamente.** Abstenção sem ata e impedimento de fecho continuam. | `app/chamados/services/disputa_emissao.py:1267`, `:1304`, `:1335`, `:1373`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58` |
   | 5. Derivação paralela do dia por motivo | **Curado quanto à divergência anterior.** Auditor chama o helper por motivo; há outra insuficiência com `None`. | `app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:50`, `:51` |
   | 6. Fechada adicional travada depois de Chamado na recuperação | **Curado para o universo de DS.** Recuperação agora antecipa todas as DS. A ordem completa de Ch/P tem ressalva abaixo. | `app/chamados/services/disputa_emissao.py:681`, `:683` |
   | 7. C8 sem certificação dos totais SQL | **Igual.** Justificativas presentes; 145/219 efetivos são [nao verificado]. | `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `:185`, `:193`, `:198` |
   | 8. RED/testes sem mordida suficiente | **Parcial.** Declaração e `all` vazio foram fortalecidos; execução histórica da base nesta cópia é [nao verificado]. | `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:406`, `:411`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:465` |
   | 9. Conflito de leitores do dia | **Curado no trecho antigo por motivo.** A distinção entre zero perguntas e perguntas mortas continua coerente. | `app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `:63`; `app/chamados/relatorios_disputa.py:13`, `:18` |
   | 10. Classes estruturais/metadados | **Parcial.** Saíram a derivação antiga e a lacuna específica de DS; continuam as ressalvas desta revisão. Pouso/pré-requisito: [nao verificado]. | `app/chamados/management/commands/auditar_invariantes_chamados.py:50`; `app/chamados/services/disputa_emissao.py:681`, `:690` |

   A grade ainda pode criar DS sem pergunta passando motivos vazios; a âncora continua tendo nascimento próprio e não examina o retorno de `abrir_conversa`. Esses dois pontos anteriores permanecem. (`app/ponto/management/commands/reconciliar_grade.py:424`, `:425`; `app/chamados/services/disputa_emissao.py:403`, `:1452`, `:1465`)

2. **Agregação, rollback, destino do motivo e reentrada**

   Na agregação, motivos/contexto são salvos e o gerador roda dentro do mesmo `atomic`. Se alguma pergunta devolvida pertence a outra DS, a exceção sai desse bloco: desfaz os updates do ensaio, perguntas novas e alterações transacionais feitas no reuso. A DS aberta preexistente e as perguntas preexistentes permanecem. (`app/chamados/services/disputa_emissao.py:547`, `:607`, `:609`, `:621`, `:673`)

   Depois, a recuperação reentra **na mesma porta** para cada dona. Motivos reusados seguem suas canônicas; os restantes voltam à aberta original. O contexto por motivo é filtrado por dona; chaves globais acompanham todas. A pergunta não troca de FK. (`app/chamados/services/disputa_emissao.py:616`, `:620`, `:688`, `:690`; `app/chamados/models.py:3008`)

   **Pode ficar em duas `DS.motivos`:** se a aberta já continha M antes da tentativa, o rollback restaura esse M; a recuperação acrescenta M à dona e não remove o anterior. Portanto isso não implementa transferência exclusiva de um motivo preexistente. Como `motivos` resume cenários, sem identificar dia/episódio, duas ocorrências do mesmo ID também não provam, isoladamente, duplicação do mesmo fato. (`app/chamados/services/disputa_emissao.py:578`, `:580`, `:621`, `:690`; `app/chamados/models.py:1743`)

   Em retorno normal, o particionamento conserva os motivos recebidos. Se a recuperação levantar, seu `atomic` desfaz as agregações: um motivo novo pode terminar em nenhuma DS porque o ato falhou. O retorno normal representa somente a primeira dona processada, mesmo quando outras também receberam motivos. (`app/chamados/services/disputa_emissao.py:676`, `:687`, `:695`, `:698`)

   **Pode ciclar até `RecursionError`.** O teste “a pergunta é da própria dona” impede apenas nova recuperação nessa situação; não há conjunto de visitadas nem limite para A→B→A. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:690`)

   Reprodução somente em memória, com núcleo/helper/gerador reais: `intervalo_saida`; A resolve D1 e possui pergunta local de D2; B resolve D2 e possui a canônica de D1; contexto recebido vazio preserva as datas distintas. A porta double, estável por dia, devolve B para D1 e A para D2. O dedup local não barra nenhum dos lados, e a execução termina em `RecursionError`. Reprodução com a porta ORM real: [nao verificado]. (`app/chamados/services/disputa_emissao.py:335`, `:361`, `:387`, `:585`, `:612`, `:690`; infraestrutura em `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:73`)

   **Dona fechada:** é selecionada explicitamente, sem filtro de abertura; recebe motivos/contexto, preservando `fechada_em` e a FK da pergunta. Porém seu `save` também aciona receivers: reconciliação do pai, retomada de regularizações e materialização de pendentes podem ocorrer. Os doubles não certificam esses efeitos. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`; `app/chamados/signals.py:28`, `:42`, `:129`, `:144`, `:153`, `:163`)

3. **Ordem de trava**

   Nascimento, agregação e reentrada passam pela chamada de `:566`; recuperação passa por `:683`. Com emissão habilitada, ambas fornecem todas as DS do colaborador, inclusive fechadas. (`app/chamados/services/disputa_emissao.py:564`, `:566`, `:681`, `:683`)

   A primeira tabela efetivamente travada é **Colaborador**, depois DS, Chamado e perguntas explicitamente pedidas, cada grupo ordenado por PK. (`app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692`)

   O QuerySet lazy dos IDs é avaliado em `:2671`, **depois de C e antes de D**. Nesse caminho `_universo` é verdadeiro, logo `:2674` faz curto circuito e não reitera `_pks(disputas)`. A reentrada cria outra consulta sob C já retido; isso não é dupla avaliação daquele mesmo QuerySet. (`app/chamados/models.py:2669`, `:2671`, `:2674`; `app/chamados/services/disputa_emissao.py:564`, `:690`)

   A declaração desliga a emissão e mantém sua trava externa antes da porta; agrupador e âncora continuam com suas entradas próprias. (`app/chamados/services/disputa_emissao.py:726`, `:1153`, `:1425`; `app/chamados/services/declaracao_texto.py:348`, `:352`, `:654`, `:656`)

   **A lacuna antiga D depois de Ch foi corrigida para as DS selecionadas, mas isso não prova toda a ordem por transação.** O topo inclui pais de DS abertas e chamados vivos; não inclui automaticamente destinos terminais de canônicas fechadas. No rechaveamento, `_nascer` pode pedir esse destino e a pergunta posteriormente. (`app/chamados/models.py:2677`, `:2679`, `:2683`, `:2940`, `:2947`, `:2949`, `:2950`)

   Assim, há caminho estático em que dois reusos/rechaveamentos no mesmo laço pedem uma nova linha de Chamado depois da pergunta do primeiro motivo — ou um Chamado de PK menor depois dos já adquiridos. Reprodução completa e deadlock efetivo são [nao verificado]; não declararia a ordem global inteira certificada. (`app/chamados/services/disputa_emissao.py:403`, `:609`, `:650`; `app/chamados/models.py:2947`, `:2950`)

4. **Fidelidade de `_resolver_pergunta_por_motivo`**

   Pelos hunks fornecidos da obra, o bloco extraído preserva os ramos anteriores: texto pelo catálogo; recuperação pela grade; data declarada do motivo; batida pelo turno; intervalo; derivação de destino; dedup; preferência de `_data_ev` sobre fallback. Comparação independente com o objeto Git `6c0517eb9`: [nao verificado]. (`app/chamados/services/disputa_emissao.py:308`, `:313`, `:335`, `:354`, `:358`, `:373`, `:387`)

   Os três `continue` internos tornaram-se `return None`: texto ainda impossível, intervalo sem dia/duplicado e destino duplicado. O gerador converte esse retorno novamente em `continue`. (`app/chamados/services/disputa_emissao.py:329`, `:364`, `:383`, `:435`, `:436`)

   Permanecem fora do helper as guardas de catálogo, dedup não datado, hora sem dia e conversa repetida. Portanto o helper não é, sozinho, uma resposta completa a “essa pergunta nascerá?”. Não identifiquei mudança de motivos emitidos causada pela extração isolada nos hunks apresentados. (`app/chamados/services/disputa_emissao.py:404`, `:415`, `:438`, `:449`)

   O comportamento anterior de dedup com `_data_ev=None` antes de aplicar fallback também foi conservado; a própria paridade espera repetição no cenário sem candidato. (`app/chamados/services/disputa_emissao.py:381`, `:387`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:439`, `:441`)

   O outro consumidor operacional do helper é o auditor: aceita `None`, lê `tupla[2]` e descarta dia `None`. O dia retornado é o do fato/turno resolvido; o dia civil da conversa só é escrito depois, por `abrir_conversa`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `:51`, `:52`, `:53`; `app/chamados/models.py:2801`)

5. **Fonte do dia e resultados de I7a/I7b**

   Sem IDs de órfãos, cada motivo consulta o mesmo helper do gerador. Saiu a precedência antiga “alguma data explícita → ultima_batida → pai/contexto”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:50`)

   **Sobra o ramo próprio dos IDs:** contexto da DS prevalece sobre o pai; IDs não vazios causam retorno antecipado, mesmo se nenhum chamado correspondente for encontrado. Nesse ramo, as fontes continuam sendo `data_do_chamado`, origem com `resolver_data_evento` e contexto sem origem. (`app/chamados/management/commands/auditar_invariantes_chamados.py:34`, `:38`, `:42`, `:44`, `:45`, `:46`)

   | Situação da DS aberta sem nenhuma pergunta | Resultado |
   |---|---|
   | Conjunto de dias vazio | **I7a**, sem chamar `_marcos_faltantes`. |
   | Um dia `sem_celula`, outro `lido` ou `fora_vinculo` | **I7a**. |
   | Conjunto não vazio, todos `sem_celula` | **I7b**. |
   | Helper retorna `None` para todos os motivos | **I7a**. |
   | Helper retorna `None` para um motivo e dia `sem_celula` para outro | **Pode dar I7b**, pois o motivo indeterminado desapareceu do conjunto. |

   As decisões da tabela vêm do descarte de `None` e de `bool(dias) and all(...)`; a última linha é uma insuficiência residual para afirmar que todos os motivos foram explicados pela falta de ata. (`app/chamados/management/commands/auditar_invariantes_chamados.py:51`, `:53`, `:69`, `:70`, `:72`)

   “SEM ata” reutiliza `sem_celula`, que também inclui ata sem lâmpadas. I7a é um grupo residual de responsabilidade ESTRUTURA: seu rótulo “COM ata” não prova existência de ata nos casos sem dia. (`app/chamados/services/declaracao_texto.py:93`, `:123`; `app/chamados/management/commands/auditar_invariantes_chamados.py:65`, `:69`, `:73`)

6. **Selo de derivação reescrito**

   Antes, conforme o diff fornecido, exigia somente a substring `data_pergunta=_data_fallback` no gerador. Agora exige a delegação literal ao helper e exatamente duas chamadas AST a `_derivar_chamado_destino`, ambas com esse argumento. O novo verifica as duas derivações, enquanto uma única substring bastava ao antigo. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:138`, `:139`, `:143`, `:146`, `:148`; versão histórica local: [nao verificado])

   Continua sendo selo estrutural: não demonstra que o gerador utiliza o resultado corretamente. Por exemplo, delegar e depois sobrescrever destino/dia por uma terceira derivação sem data pode passar, porque a inspeção das chamadas ocorre somente dentro do helper. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:139`, `:143`, `:145`; `app/chamados/services/disputa_emissao.py:437`)

   **Não demonstraria “antigo mata, novo deixa passar” usando literalmente o selo antigo sobre esta extração:** ele já reprovaria o controle correto, que deixou de conter aquela substring no gerador. Um mutante semanticamente equivalente antes/depois, com ambos os controles verdes e perda comprovada de mordida, é [nao verificado].

   A sonda chamada “selo original” executa o método **atual** lido do arquivo, não a versão histórica anterior à reescrita. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:346`, `:347`, `:349`, `:355`)

7. **Catraca de linhas do turno**

   O delta fornecido altera somente números dos mesmos dois sítios B1: `[778,779]→[809,810]` e `[1262]→[1293]`. Não acrescenta sítio permitido nem altera o critério; a cópia registra as novas linhas nos mesmos identificadores. (`app/core/tests/test_medida_turno_do_par.py:267`, `:269`, `:270`)

   A catraca verifica separadamente identidade dos sítios e linhas exatas. Os 35 testes passaram nesta sessão com a substituição de `SimpleTestCase` descrita na entrega; isso não certifica o runner Django. (`app/core/tests/test_medida_turno_do_par.py:20`, `:535`, `:541`; certificação Django: [nao verificado])

8. **Testes reais, `--base`, RED e vacuidade** [nao verificado]

   Executam os corpos extraídos dos arquivos por AST, sem reimplementação manual do núcleo/gerador/auditor. São funções recompiladas em outro escopo: ORM, transação, catálogo, juiz da ata e porta de pergunta são doubles. A sonda de trava extrai o método real de `models.py`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:37`, `:70`, `:73`, `:89`, `:183`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:197`)

   **`--base` pertence à suite anterior:** busca emissor `6c0517eb9`, acrescenta helper atual quando ausente e usa auditor atual como sonda. A suite nova oferece `--antes`, com base `f0b2c84fa`, não `--base`. Seus testes de paridade também comparam com f0b, não diretamente com 6c. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:23`, `:29`, `:209`, `:210`, `:214`, `:473`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:26`, `:358`, `:457`)

   Para o caminho **(a)**:

   - `test_a_excecao_do_gerador_desfaz_agregado_e_preserva_batida`: a assertion de `:295` espera `(0,0)`; no fluxo antigo, DS/pai já concluídos antes da exceção produzem `(1,0)`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:295`)
   - `test_a_excecao_apos_inserir_pergunta_desfaz_tambem_a_pergunta`: exige positivamente pergunta antes da exceção; o contador pode continuar zero por essa pergunta existir, mas a contagem direta de tabelas em `:312` deve reprovar o resíduo antigo. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:300`, `:305`, `:311`, `:312`)

   Essa é a mordida indicada pelo código e pelo fluxo anterior anexado; execução dessas assertions contra o objeto Git real da base nesta cópia é [nao verificado].

   A declaração deixou de passar como no-op: exige retorno e gravações positivas. O `all` das travas agora tem guarda de conjunto não vazio. A paridade comportamental exige resultados esperados por cenário antes de comparar os dois geradores; as recusas vazias são deliberadas. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:406`, `:411`, `:412`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:380`, `:433`, `:434`)

   Rodei 25 controles de memória nesta sessão, verdes. Os dois testes restantes de paridade histórica, `--base` e a certificação ORM ficaram [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:284`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:141`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:221`, `:345`, `:357`, `:377`)

9. **Quem escreve, quem lê e significado dos campos**

   | Campo | Escritores | Leitores relevantes |
   |---|---|---|
   | `DS.motivos` | Núcleo; agrupador/âncora; portas de remoção/reabertura. | Gerador, auditor e relatório. (`app/chamados/services/disputa_emissao.py:583`, `:645`, `:1219`, `:1349`, `:1460`; `app/chamados/models.py:1966`, `:2063`, `:2159`; `app/chamados/management/commands/auditar_invariantes_chamados.py:49`; `app/chamados/relatorios_disputa.py:52`) |
   | `DS.contexto`, `motivo.data`, `qtd`, `ultima_em` | Núcleo; pendentes; portas de contexto. Recuperação filtra, mas reentra para gravar. | Texto/data do helper, grade e auditor. (`app/chamados/services/disputa_emissao.py:293`, `:296`, `:599`, `:600`, `:604`, `:646`, `:688`; `app/chamados/models.py:2104`, `:2122`; `app/chamados/services/disputa_emissao.py:308`, `:335`; `app/chamados/management/commands/auditar_invariantes_chamados.py:30`) |
   | `DS.chamado` e contexto do pai | Nascimentos e porta de ocorrência/contexto do chamado. | Grade, auditor, S127 e receivers. (`app/chamados/services/disputa_emissao.py:636`, `:642`, `:647`, `:1221`, `:1456`, `:55`; `app/chamados/management/commands/auditar_invariantes_chamados.py:31`; `app/chamados/juizes.py:2316`; `app/chamados/signals.py:32`) |
   | `DS.fechada_em` | Fecho e reaberturas existentes; esta cura não o escreve diretamente. | Seleção da aberta/dona, auditor e receivers. (`app/chamados/models.py:1932`, `:2149`, `:2173`; `app/chamados/services/disputa_emissao.py:571`, `:575`; `app/chamados/management/commands/auditar_invariantes_chamados.py:63`; `app/chamados/signals.py:129`) |
   | `Pergunta.disputa`, `motivo`, `data_evento`, `chamado_destino`, `conversa`, via | Porta de nascimento; dia pelo ponto canônico; reuso pode rechavear/costurar preservando dona. | Dedup, detector estrangeiro, auditor e leitores do fio. (`app/chamados/models.py:2947`, `:2982`, `:3008`, `:3014`, `:3016`; `app/chamados/services/disputa_emissao.py:361`, `:381`, `:612`, `:613`; `app/chamados/management/commands/auditar_invariantes_chamados.py:63`) |
   | I7a/I7b | Auditor calcula listas e `add` produz número/amostra. | stdout, HTML e execução programada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:73`, `:74`, `:632`, `:1911`, `:1918`; `app/config/crons.py:542`) |

   Os insumos de origem continuam vindo dos produtores existentes: API grava `entrada_sem_saida`/`ultima_batida`; PWA e fábrica fornecem data por motivo; agrupador grava IDs de órfãos; detector transporta entradas pendentes. Censo exaustivo de todos os produtores: [nao verificado]. (`app/api/views_core.py:669`, `:788`; `app/ponto/services/triagem_batida.py:738`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/disputa_emissao.py:1220`; `app/chamados/detector_anomalias.py:50`)

   **Leitores com pressupostos relevantes:** o wrapper recebe uma única DS, mas ela agora pode ser fechada ou apenas a primeira dona de um lote. Mesmo assim chama pendentes e, havendo motivos novos, envia “Nova pendência… Toque para responder”, sem guarda de `fechada_em` nessa função de push. Entrega efetiva de notificação é [nao verificado]. (`app/chamados/services/disputa_emissao.py:695`, `:698`, `:779`, `:781`, `:1845`, `:1859`, `:1861`)

   `data_evento` continua tendo domínio explicitado por `conversa`: turno para fato e dia civil para conversa. O helper não deve ser lido com

[truncado em 20000 de 22660 caracteres]

## 10/11 18:36 · sessao 40458ebcdfa0 · - · - · origem=Ronald (tmux)

**Agregação estrangeira está curada para motivos novos, mas há duas ressalvas reproduzidas: motivo preexistente fica em duas DS; reentrada pode ciclar.** (`app/chamados/services/disputa_emissao.py:578`, `:612`, `:620`, `:690`)

- O ensaio salva motivos/contexto e executa todo o gerador dentro do `atomic`; só depois examina as perguntas devolvidas. A exceção estrangeira desfaz essas gravações, as perguntas novas e alterações transacionais do reuso. A DS aberta preexistente e as perguntas preexistentes permanecem. (`app/chamados/services/disputa_emissao.py:547`, `:607`, `:609`, `:621`, `:673`)
- A recuperação filtra as chaves por motivo, reentra na mesma porta para cada dona e mantém os motivos não reusados na aberta original. As chaves globais do contexto acompanham todas as donas; `contexto_nascimento` é passado inteiro, mas a reentrada seleciona uma DS existente. (`app/chamados/services/disputa_emissao.py:616`, `:620`, `:688`, `:690`, `:694`, `:575`)
- A primeira dona estrangeira vira o retorno da função; outras donas também recebem seus motivos. Portanto o retorno não representa necessariamente todo o lote. (`app/chamados/services/disputa_emissao.py:687`, `:695`, `:698`)
- **Duas DS.motivos:** se a aberta já possuía M antes da chamada, o rollback restaura esse M; a recuperação agrega M à dona e não remove o M antigo. Reproduzi com `Ambiente` atual e gerador real: ambas as DS terminam com `['batida_fora_escala_grave']`. A demonstração usa doubles; ocorrência real em banco [nao verificado]. (`app/chamados/services/disputa_emissao.py:578`, `:580`, `:621`, `:690`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:73`)
- **Ciclo:** não existe conjunto de visitadas, limite nem estabilização na recursão. Dona própria encerra porque não satisfaz `pergunta.disputa_id != disputa_aberta.pk`; isso não impede A→B→A quando cada dona resolve outro dia. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:690`)
- Reproduzi ciclo com **intervalo_saida**, sem episódio, helper/gerador/núcleo reais: A aberta tem contexto M.data=D1 e pergunta local de D2; B fechada tem contexto M.data=D2 e pergunta local de D1. Porta double estável por dia devolve a canônica de D1/B ou D2/A; `_derivar_chamado_destino` devolve `(None,None)`, sem alterar o dia intervalar. `emitir([M], contexto={})` termina em `RecursionError` após 139 chamadas com limite 160. O dedup não barra: cada DS só possui pergunta do dia oposto ao pedido. PostgreSQL [nao verificado]. (`app/chamados/services/disputa_emissao.py:334`, `:360`, `:370`, `:387`, `:434`, `:612`, `:690`)
- A dona fechada é selecionada sem filtro de abertura e recebe apenas `motivos`/`contexto`; seu `fechada_em` e a FK da pergunta ficam preservados. Entretanto salvar DS fechada dispara receivers que não exigem alteração de `fechada_em`: S88 pode reconciliar o pai e a retomada de regularizações agenda callback. Esses efeitos não existem nos doubles da sonda. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`; `app/chamados/signals.py:28`, `:42`, `:129`, `:144`)
- Se a operação termina normalmente, os motivos novos ficam na dona ou na aberta original conforme o particionamento. Exceção posterior na recuperação desfaz sua transação; motivo novo pode terminar em nenhuma DS porque o ato inteiro falhou. Não há garantia de sucesso no ciclo. (`app/chamados/services/disputa_emissao.py:676`, `:687`, `:690`, `:698`)

**Travas do núcleo/reentrada/recuperação:**

- Entrada normal e reentrada passam todas as DS do colaborador, abertas e fechadas, por QuerySet lazy; recuperação agora passa o mesmo universo antes de recursar. A lacuna anterior “fechada adicional adquirida depois de Ch” foi curada para essas DS. (`app/chamados/services/disputa_emissao.py:564`, `:566`, `:681`, `:683`, `:690`)
- Primeira tabela é **Colaborador**, depois avaliação dos IDs, depois DS por PK, depois Ch por PK, depois perguntas explicitamente pedidas. (`app/chamados/models.py:2666`, `:2671`, `:2675`, `:2688`, `:2692`)
- Com `abertas_do_colaborador=True` e colaborador válido, `_universo` é verdadeiro; a condição em 2674 faz curto-circuito e não reitera `_pks(disputas)`. O QuerySet é avaliado em 2671, uma vez nessa chamada, entre C e D. A reentrada cria outro QuerySet sob a mesma transação, mas C já está retido; releitura adicional não é dupla avaliação do mesmo QuerySet entre duas travas. (`app/chamados/models.py:2669`, `:2671`, `:2674`; `app/chamados/services/disputa_emissao.py:564`, `:690`)
- `emitir_perguntas=False` passa a dona explícita, se houver, e abertas; não toma todas as fechadas. Declaração tem trava externa própria antes da porta e da pergunta. (`app/chamados/services/disputa_emissao.py:556`, `:557`, `:726`; `app/chamados/services/declaracao_texto.py:348`, `:352`, `:360`, `:376`)
- **Ressalva da ordem global completa:** todas as DS estão previamente travadas, porém Ch inclui apenas pais das DS **abertas** e Ch **VIVOS**. Destino terminal de canônica fechada não entra automaticamente. (`app/chamados/models.py:2677`, `:2679`, `:2681`, `:2683`)
- No reuso com rechave, `_nascer` pode adquirir `ex.chamado_destino_id` terminal e a pergunta. Como o atomic novo engloba o laço inteiro do gerador, dois reusos/rechaves sucessivos podem pedir Ch terminal novo depois da P do primeiro motivo. Isso é caminho estático plausível; reprodução completa com ORM/portas reais [nao verificado]. Não afirmar ausência global de Ch após P. (`app/chamados/models.py:2940`, `:2947`, `:2949`, `:2950`; `app/chamados/services/disputa_emissao.py:403`, `:609`)
- Reuso sem rechave costura o destino pedido por `_costura_nao_mata_o_fio`; esse destino vem do derivador de Ch vivos, já coberto pelo topo. (`app/chamados/services/disputa_emissao.py:198`, `:200`; `app/chamados/models.py:2980`; `app/chamados/services/fio.py:768`)
- Permanecem gerações posteriores ao núcleo em pendentes e fábrica, sem participação no novo detector estrangeiro. (`app/chamados/services/disputa_emissao.py:779`, `:294`; `app/chamados/services/fabrica_da_celula.py:172`, `:174`)

## 10/11 18:35 · sessao e9fc4319af12 · - · - · origem=Ronald (tmux)

4. **Extração fiel no trecho mostrado do diff da obra.** O antigo bloco da base 6c `disputa_emissao.py:344–424` corresponde ao helper atual `:307–387`; as recusas por texto sem data, intervalo sem dia/duplicado e destino duplicado viraram `return None` em `:329`, `:364`, `:383`. A seleção de `_data_ev` antes do fallback continua em `:387`; o retorno inclui texto/destino/dia em `:388`. A comparação independente com o objeto Git 6c é [nao verificado].

As guardas do gerador continuam fora do helper: motivo ausente do catálogo (`app/chamados/services/disputa_emissao.py:404`), dedup não datado (`:415`), hora sem dia (`:438`) e conversa repetida (`:449`). Portanto o helper não representa sozinho todas as condições de nascimento; a chamada do gerador conserva essas guardas e transforma `None` em `continue` (`:434–443`).

O outro consumidor operacional é o auditor: aceita `None`, acrescenta apenas `resolvida[2]`, e elimina dia `None` (`app/chamados/management/commands/auditar_invariantes_chamados.py:49–53`). O dia é o turno/fato fornecido pelas fontes; conversas recebem dia civil posteriormente na porta `abrir_conversa` (`app/chamados/models.py:2801`), não no helper.

5. **A precedência paralela dos motivos saiu; sobra o ramo próprio dos IDs.** IDs do contexto da DS prevalecem sobre os do pai; IDs não vazios causam retorno antecipado, mesmo sem registros encontrados (`app/chamados/management/commands/auditar_invariantes_chamados.py:34`, `:45`). Nesse ramo os dias vêm de `data_do_chamado`, `resolver_data_evento` com origem ou `data_evento_do_contexto` (`:37–44`). Fora dele, cada motivo chama o helper (`:49–52`).

Resultados por DS aberta sem nenhuma linha de pergunta:

- Dias vazios: I7a; juiz não é chamado pelo curto circuito de `bool(dias)` (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`).
- Dias mistos, um `sem_celula` e outro `lido`/`fora_vinculo`: I7a (`:69–72`).
- Todos os dias retornados `sem_celula`, com conjunto não vazio: I7b (`:69–74`).
- Helper `None` em todos os motivos: I7a; `None` é descartado (`:51–53`).
- **Helper `None` em um motivo e dia `sem_celula` em outro: pode dar I7b.** O motivo recusado não deixa marca de indeterminação no conjunto (`:51–53`, `:69`). Reproduzido localmente com funções atuais e doubles; runtime/produção [nao verificado].

`sem_celula` inclui ata sem lâmpadas, enquanto `fora_vinculo` tem significado distinto; o auditor reutiliza essa autoridade (`app/chamados/services/declaracao_texto.py:91–96`, `:123–126`; `app/chamados/management/commands/auditar_invariantes_chamados.py:70`).

9. Campos e consumidores relevantes:

| Campo | Escritores | Leitores/semântica |
|---|---|---|
| DS.motivos | Núcleo `app/chamados/services/disputa_emissao.py:583`, `:645`; agrupador `:1219`, `:1225`, `:1254`, `:1349`; remoção/reabertura `app/chamados/models.py:1966`, `:2063`, `:2159` | Catálogo define “cenários ativos” em `app/chamados/models.py:1743`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:49`; relatório `app/chamados/relatorios_disputa.py:52`. |
| DS.contexto, motivo.data, qtd, ultima_em | Núcleo `app/chamados/services/disputa_emissao.py:599–607`, `:646`; pendentes `:293–296`; outras portas `app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122` | Helper texto/data `app/chamados/services/disputa_emissao.py:308`, `:335`; grade funde contexto do pai `:54–58`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:30–34`. Helper só monta cópia local em `app/chamados/services/disputa_emissao.py:315–316`. |
| DS.chamado / contexto_json do pai | Núcleo `app/chamados/services/disputa_emissao.py:636`, `:647`; FK aceita SET_NULL `app/chamados/models.py:1749–1752` | Grade `app/chamados/services/disputa_emissao.py:55–58`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:31–34`; S127 `app/chamados/juizes.py:2316`. |
| DS.fechada_em | Fecho/reaberturas `app/chamados/models.py:1932`, `:2149`, `:2173` | Núcleo seleciona aberta em `app/chamados/services/disputa_emissao.py:571` e dona sem filtro em `:575`; auditor só abertas `app/chamados/management/commands/auditar_invariantes_chamados.py:63`. |
| Pergunta.disputa/motivo/destino/data | `_nascer` cria em `app/chamados/models.py:3014`; dia passa por `gravar_dia_do_fato` em `:3016`; reuso retorna existente em `:3008` | Detector lê dona/motivo `app/chamados/services/disputa_emissao.py:612–613`; helper dedup lê dia/motivo/via/conversa `:361–382`; auditor conta ausência de qualquer linha `app/chamados/management/commands/auditar_invariantes_chamados.py:63`. |

“Sem pergunta” significa zero linhas, consistente com relatório (zero em `app/chamados/relatorios_disputa.py:12–14`; só mortas em `:18–20`), fechamento VAZIA/VEREDITO (`app/chamados/models.py:1903`) e bloqueio S127 (`app/chamados/juizes.py:2319–2320`).

10. **Relógio indireto merece ressalva.** O auditor chama `cobra_marcos_da_ata` sem `hoje` (`app/chamados/management/commands/auditar_invariantes_chamados.py:37`); esse predicado lê `_tz.localdate()` (`app/chamados/services/disputa_emissao.py:838`). Já existia na volta 2, portanto não é introdução do delta v3; integra o caminho adicionado pela obra inteira. Não afirmar classificação I7 inteiramente independente do relógio.

Não localizei autoridade duplicada pela extração: o helper conserva os leitores, e o auditor reutiliza `_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:313`, `:354`, `:372`, `:375`; `app/chamados/management/commands/auditar_invariantes_chamados.py:70`). MAPA continua atribuindo dia do chamado a `data_do_chamado` e fechamento a `pode_fechar` (`app/docs/MAPA.md:163`, `:165`); não há divergência específica desses endereços causada pelo helper. Pouso, pré-requisito e certificação são [nao verificado].

## 10/11 18:35 · sessao f6ffe75e9181 · - · - · origem=Ronald (tmux)

6. O selo anterior exigia só a substring `data_pergunta=_data_fallback` dentro do gerador; isso consta no hunk fornecido de `diff_v3.patch` [nao verificado quanto ao arquivo histórico local]. O atual exige a delegação literal ao helper e exatamente duas chamadas AST a `_derivar_chamado_destino`, ambas com argumento `data_pergunta` cujo valor é `_data_fallback`. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:138`, `:143`, `:146`, `:148`)

   Para os dois sítios extraídos, a exigência de encaminhamento ficou mais estrita: o antigo podia deixar uma chamada sem data enquanto outra preservava a substring; o atual verifica ambas. Entretanto continua sendo teste estrutural: pode passar se o gerador mantiver a delegação e depois sobrescrever destino/dia com uma terceira chamada a `_derivar_chamado_destino(disputa, motivo)` sem data. O selo só inspeciona chamadas dentro do helper. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:139`, `:143`, `:145`; desempacotamento real em `app/chamados/services/disputa_emissao.py:436`)

   **Cuidado na comparação pedida:** executar literalmente o selo antigo sobre o código extraído já reprova o controle correto; logo não é uma prova pareada de “controle verde e mutante RED”. Um mutante semanticamente equivalente antes/depois da extração, morto apenas pelo antigo, não foi demonstrado nesta cópia. [nao verificado] [nao verificado]

7. O delta fornecido muda somente os valores das linhas dos mesmos dois identificadores B1: `_orfao_resolvido_cross_meianoite` e `criar_disputa_a_partir_de_chamados_orfaos`; não acrescenta sítio permitido. A cópia registra `[809,810]` e `[1293]`. A catraca mantém verificações separadas da identidade e das linhas. (`app/core/tests/test_medida_turno_do_par.py:269`, `:270`, `:535`, `:541`)

8. Os testes recompilam por AST os corpos lidos dos arquivos, em vez de reimplementar manualmente núcleo/gerador/auditor. Fronteiras ORM, atomic, catálogo, ata e porta de pergunta são doubles. A trava da volta 3 é extraída do método real de `PerguntaDisputa`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:37`, `:70`, `:73`, `:89`, `:183`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:197`)

   `--base` pertence à suite anterior: emissor obtido por `git show` da base 6c, acrescido do helper atual quando ausente, com auditor atual como sonda. A suite nova possui `--antes`, referindo-se a f0b; não possui `--base`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:23`, `:29`, `:209`, `:210`, `:214`, `:473`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:26`, `:457`)

   No caminho (a), a primeira exceção confronta I7a/I7b com `(0,0)` em `:295`; no emissor antigo que já concluiu núcleo antes da exceção, espera-se falha por `(1,0)`. A segunda exceção exige positivamente uma pergunta antes de levantar (`:305`); como a pergunta existente exclui a DS do contador, a assertion decisiva é a contagem direta vazia em `:312`. Execução dessas assertions contra o histórico real 6c nesta cópia: [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:295`, `:300`, `:305`, `:311`, `:312`)

   A vacuidade apontada na declaração foi curada: exige retorno, DS/pai gravados, motivo/contexto/descrição e contagens positivas, além de ausência de geração. A assertion `all(...)` de travas agora tem guarda de não vazio. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:331`, `:406`, `:408`, `:410`, `:411`, `:414`)

   A paridade não passa simplesmente por ambos os geradores devolverem vazio: confronta antes o resultado com dia/pergunta esperado por cenário; três casos de recusa esperam vazio deliberadamente. O teste do selo extraído apenas inspeciona código, sem executar geração. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:380`, `:433`, `:434`, `:345`, `:355`)

   Os modos de mutação exigem controle verde, pelo menos uma falha por assertion e nenhum erro de montagem. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:477`, `:481`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:462`, `:465`)

Execução somente em memória nesta revisão: 23 testes verdes, abrangendo suite anterior, `Volta3Test` e selo extraído; sem banco/Git histórico. [nao verificado quanto à certificação Django/PostgreSQL] [nao verificado]

## 10/11 18:29 · sessao 1510caea65c6 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

VOLTA 3 implementada; cura, comandos e rastreio na [entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:3).

- [27 testes de memória verdes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:136) e [35 da catraca verdes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:184). [nao verificado]
- [RED contra a base: 6 assertions; sete mutantes novos mortos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:142).
- [Registro de linhas ajustado; tetos preservados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:85).

SQL/ORM pelo Code e certificação final pelo Opus: **[não verificado]**. [FALTA](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:244).

## 10/11 18:25 · sessao 39c276452548 · BO-0611-05 · verificador · origem=Code

**A volta 7 cura os três leitores delimitados, preservando a decisão do geofence. Não certificaria o pouso com a prova disponível: falta materializar o BACKLOG das divergências; RED histórico, mensagem do commit e smoke permanecem `[nao verificado]`.** (`app/templates/core/_d2_feed.html:16`; `app/templates/ponto/batidas_offline_pendentes.html:96`; `app/ponto/views.py:485`; `app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`)

**1. LIBERA-B3: cinco itens e três proibidos**

| Exigência | Veredito e sítio |
|---|---|
| **(1)** Extrair o juiz, geofence usá-lo, registrar e leitores perguntarem | **Cumprido no recorte autorizado.** Função em `app/ponto/services/geofence.py:126`, uso pelo geofence em `:269`, registro em `app/core/juizes.py:1201`, filtro em `app/core/templatetags/hasner_filters.py:34`, feeds e contador nos sítios citados acima. |
| **(2)** Censo antes do construtor; divergências externas no BACKLOG | **Parcial.** Censo e brief foram fornecidos; cronologia independente `[nao verificado]`. Código e corte remetem as divergências ao BACKLOG, mas a linha encontrada continua sendo a original, “censo1 em curso”, sem registrar as divergências v7. (`app/core/juizes.py:1200`; `app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`) |
| **(3)** Mutante de nulidade em um leitor fica vermelho | **Cumprido no desenho executável do selo:** controle, mutação isolada, coordenada válida, zero e leitor vizinho; execução Django nesta revisão `[nao verificado]`. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:128`) |
| **(4)** RED contra `da829e9f6`, com log guardado | **Não cumprido como evidência entregue.** Execução e log histórico `[nao verificado]`; o requisito continua registrado. (`CLAUDE.md:28`; `app/docs/CORTES.md:440`) |
| **(5)** Commit LEI-AKITA + L-030 + correções de LEIS | **Parcial.** LEIS corrigidas; conteúdo da mensagem do commit `[nao verificado]`. (`app/docs/LEIS.md:78`; `app/docs/LEIS.md:165`; `CLAUDE.md:63`) |

| Proibido | Veredito |
|---|---|
| Mexer no veredito do geofence | **Cumprido:** a extração preserva o predicado; detalhado no item 2. (`app/ponto/services/geofence.py:145`; `:269`) |
| Fallback de coordenadas | **Cumprido no delta examinado:** juiz não inventa coordenada; ramo sem GPS grava `None` no alerta. (`app/ponto/services/geofence.py:145`; `:275`; `:276`) |
| Tratar `0.0` como coordenada válida | **Cumprido nos três leitores curados; parcial na obra inteira.** Permanecem leitores por nulidade e um selo da obra exige zero como evidência no detector. Essa migração foi destinada à fila pelo aval, mas a fila não está materializada na cópia. (`app/ponto/views.py:485`; `app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`; `app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`) |

**2. Paridade de `tem_gps` com a base**

**Sim, para todos os valores escalares enumerados na pergunta.** `bool(lat and lon)` equivale a `not (not lat or not lon)` para esses valores: qualquer componente falsy torna o par sem GPS; ambos truthy tornam o par com GPS. Não há contraexemplo entre `None`, `''`, zeros numéricos, `Decimal('0')`, `Decimal('0E-7')`, valores válidos, pares parciais e strings. (`app/ponto/services/geofence.py:145`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:72`)

**String não vazia é truthy**, inclusive `"0"`, `"0.0"` e `"abc"`; o juiz preserva a semântica anterior, não valida se a string representa coordenada. Uma string inválida pode falhar posteriormente na conversão numérica; esse comportamento não foi corrigido pela extração. (`app/ponto/services/geofence.py:145`; `app/ponto/services/geofence.py:88`)

A alteração substitui apenas a condição de entrada do ramo: zero numérico continua produzindo `sem_gps`, com coordenadas do alerta nulas, sem reescrever a Batida. (`app/ponto/services/geofence.py:269`; `:274`; `:275`; `:276`)

**3. Leitores, `load`, objetos e falha de render**

Os dois templates perguntam pelo filtro; o contador pergunta diretamente à função. **O filtro não refaz a conta:** importa o módulo e devolve seu resultado. (`app/templates/core/_d2_feed.html:16`; `app/templates/ponto/batidas_offline_pendentes.html:96`; `app/ponto/views.py:485`; `app/core/templatetags/hasner_filters.py:33`; `:34`)

**O `load` é local aos dois consumidores**, portanto independe de qual raiz os renderiza. (`app/templates/core/_d2_feed.html:1`; `app/templates/ponto/batidas_offline_pendentes.html:4`)

Nos caminhos de produção encontrados:

- O feed recebe uma **lista de models `Batida`**, tanto pela célula quanto pelo endpoint de dados; `b` não é dict nem linha de `values()`. (`app/core/cell.py:9`; `:20`; `app/core/views.py:1162`; `:1171`)
- A página offline recebe **QuerySet de `Batida`**; cada `batida` do laço é model. O `values_list()` serve somente ao contador. (`app/ponto/views.py:441`; `:485`; `:489`; `app/templates/ponto/batidas_offline_pendentes.html:53`)

**Dict ou `None` quebram o filtro com `AttributeError`**, porque ele exige `.latitude` e `.longitude`. Os testes usam `SimpleNamespace`, que satisfaz esse contrato; não cobrem dict/None. (`app/core/templatetags/hasner_filters.py:34`; `app/ponto/tests/test_bo0611_05_templates_geo.py:123`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:100`)

Não há proteção local envolvendo o render offline ou os renders do feed. Se a exceção propagar, a resposta offline inteira falha; no feed, falha a resposta do fragmento/JSON, sem necessariamente derrubar o dashboard já aberto. Propagação efetivamente executada pelo Django neste ambiente: **`[nao verificado]`**. (`app/ponto/views.py:488`; `app/core/live_cell.py:22`; `:28`; `app/core/views.py:1171`)

**4. Universo e custo de `com_gps`**

`pendentes` reúne todas as Batidas com `origem='offline_pendente'` e `aprovada=False`, ordenadas por timestamp. **A view não impõe teto, paginação, período ou filtro explícito por empresa/colaborador.** (`app/ponto/views.py:441`; `:446`)

Há um `COUNT` para o total, leitura de todas as coordenadas e contagem em Python, seguida do queryset completo para render. Portanto o custo adicional cresce com o universo inteiro; volume e latência reais **`[nao verificado]`**. (`app/ponto/views.py:484`; `:485`; `:489`; `app/templates/ponto/batidas_offline_pendentes.html:53`)

**Não encontrei regra GPS concorrente nessa página:** `sem_gps` é o complemento do contador pelo juiz, e cada linha usa o mesmo juiz. Cards e linhas são coerentes para um conjunto estável; alterações concorrentes entre as consultas **`[nao verificado]`**. (`app/ponto/views.py:486`; `app/templates/ponto/batidas_offline_pendentes.html:19`; `:23`; `:27`; `:96`)

**5. Leitores remanescentes do censo**

Continuam com decisão ou transformação própria:

| Forma | Sítios atuais |
|---|---|
| Truthiness das duas coordenadas | `app/colaboradores/services/drawer.py:63`; `app/templates/core/dashboard2.html:242`; `:287` |
| Truthiness individual | `app/ponto/services/geofence.py:349`; `:350`; `:391`; `:392`; `app/core/services/painel_op.py:227`; `:228`; `app/colaboradores/painel_cell.py:54`; `:55`; `app/colaboradores/services/situacional.py:199`; `:200` |
| Nulidade conjunta | `app/chamados/views.py:692`; `app/ponto/services/geofence.py:52`; `:425`; `app/ponto/services/vinculo_na_lista.py:49`; `app/colaboradores/views.py:53`; `:337`; `:356`; `:395`; `:2474`; `app/colaboradores/services/detalhe.py:22`; `:53` |
| Nulidade no contexto de chamado | `app/templates/chamados/partials/modal_fio.html:480`; `app/templates/chamados/partials/_relatorio_geo.html:30` |
| Nulidade individual na serialização | `app/ponto/views.py:468`; `:469`; `app/core/views.py:1024`; `:1025` |
| `isNaN` após conversão | `app/static/js/geo-colab-map.js:29`; `:39`; `app/templates/chamados/fio_colaborador.html:313`; `app/templates/chamados/painel_gestao.html:583` |
| Coordenadas copiadas em alerta | `app/ponto/models.py:263`; `app/ponto/management/commands/reconciliar_alertas_geofence.py:42`; `app/templates/colaboradores/detalhe.html:142`; `app/templates/core/dashboard_supervisor.html:90` |

**Há julgamentos próprios na mesma página do feed:** clicar na linha abre o drawer, que decide novamente em JavaScript. Para os floats/nulos emitidos pelo endpoint, inclusive zero numérico, atualmente coincide com o juiz; continua sendo derivação própria. (`app/templates/core/_d2_feed.html:4`; `app/core/views.py:1024`; `:1025`; `app/templates/core/dashboard2.html:242`; `:287`)

**Há alcance sobre alerta/decisão:** vínculo aceita zero por nulidade e calcula distância; o reconciliador consome esse resultado. A reavaliação geofence também seleciona ocorrências por nulidade e pode retratar chamados. Efeito financeiro direto desses leitores **`[nao verificado]`**. (`app/ponto/services/geofence.py:52`; `:58`; `:67`; `app/chamados/reconciliador.py:300`; `app/ponto/services/geofence.py:425`; `:430`; `:440`)

O aval manda registrar essa dívida sem ampliar a volta. A promessa está no corte, mas o BACKLOG disponível só contém a entrada original da BO; registro externo das divergências **`[nao verificado]`**. (`app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`)

**6. Registro, corte e documentação**

**A frase exigida está presente**, e o juiz está registrado. `tem_gps` não duplica `classificar_posicao`: presença de coordenadas e classificação de distância/accuracy/raio são perguntas distintas. (`CLAUDE.md:74`; `app/docs/CORTES.md:442`; `app/core/juizes.py:1191`; `:1201`; `app/ponto/services/geofence.py:95`; `:126`)

Comparação em memória, sem escrita:

- **`MAPA-JUIZES.md` integral confere** com o gerador, inclusive ordenação e cinco juízes de Batida. (`bin/gerar_mapa_juizes.py:17`; `:44`; `:65`; `app/docs/MAPA-JUIZES.md:19`; `:183`)
- **`ARQUITETURA.mmd` integral confere** com a montagem dos blocos reais. (`app/core/management/commands/gerar_diagrama.py:344`; `:911`; `app/docs/ARQUITETURA.mmd:363`)
- **As alterações de `MAPA.md` conferem.** Igualdade integral ficou **`[nao verificado]`** apenas para o contador de portas que exige resolver Django, em `app/docs/MAPA.md:231`. (`app/docs/MAPA.md:93`; `:247`; `app/core/management/commands/gerar_diagrama.py:827`; `app/core/portas_da_tela.py:108`)
- **`JUIZES.md` está incompleto quanto ao novo juiz:** continua lookup sem entrada `tem_gps`; não apresenta regra GPS contraditória. O próprio documento manda acompanhar o registro vivo. (`app/docs/JUIZES.md:7`; `:14`; `:121`; `app/core/juizes.py:1201`)
- **LEIS:78/:165 agora descrevem a cura e a barreira restante**, mantendo L-030 como PELA-METADE. (`app/docs/LEIS.md:78`; `:165`; `app/ponto/registro_batida.py:145`; `:150`)

Os mapas refletem o **registro**, não provam ausência de leitores paralelos: a família continua sem pendentes cadastrados, apesar dos leitores remanescentes declarados. (`app/core/juizes.py:1200`; `:1266`)

**7. Os selos mordem?**

**Sim, por inspeção, os selos críticos possuem controles e mutações efetivas:**

- O novo selo cobre zeros float/int/Decimal/`0E-7`, ausentes, parciais e válidos; a grade exige os dois resultados e reprova nulidade. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:31`; `:65`; `:72`; `:82`)
- O mutante troca **um** leitor, mantém controle válido, faz zero gerar link e exige a mesma asserção vermelha; o leitor vizinho permanece verde. A substituição exige âncora única. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:128`; `app/ponto/tests/test_bo0611_05_templates_geo.py:51`)
- Renderiza **as duas cascas reais**, com montagem de ensaio para o feed e página consumidora para offline; isso não substitui smoke de clique. (`app/ponto/tests/test_bo0611_05_templates_geo.py:31`; `:100`; `:107`)
- Nos mutantes críticos, render e controles ficam fora de `assertRaises`, e não há `subTest` interno engolindo a falha esperada. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:132`; `:137`; `app/ponto/tests/test_bo0611_05_templates_geo.py:190`)
- A saída de offline de `leitores_geo` **não afrouxou a cobertura funcional**: o novo selo extrai a expressão real por AST, testa zero/parcial/mistura, mata nulidade e confere import/redefinição. (`app/ponto/tests/test_bo0611_05_leitores_geo.py:3`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:170`; `:184`; `:193`; `:198`)
- Contrato e teste de API foram fortalecidos: incluem o novo registro e afirmam que zero preservado pelo escritor é lido como ausência. (`app/ponto/tests/test_contract_juiz_batida.py:164`; `app/api/tests/test_l030_batida_pela_porta.py:40`; `:56`)

Na obra inteira, o selo modal deixou de exigir `naoBloquear` quando não existe `hxConfirmar`; a cobertura JS nova proíbe confirmação antes do POST/fila, inclusive com comentário enganoso. As âncoras examinadas existem e são verificadas. (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:138`; `app/ponto/tests/test_l030_forma_js.py:91`; `:95`; `:110`; `app/templates/ponto/bater_ponto_v2.html:331`; `:344`)

Os **14 testes puros executados ficaram verdes**, incluindo mutantes internos; sem log por regra de somente leitura, resultado persistido **`[nao verificado]`**. Render/POST não foram reexecutados porque Django está ausente; os 6672/ruff são resultados informados pela sessão, sem conferência independente **`[nao verificado]`**. [nao verificado]

**8. Quem escreve, quem lê e tipos**

**Escrita efetiva:** `registrar_batida`, nos dois `create`; semeadura também está na porta e exige banco de ensaio. Latitude/longitude são `DecimalField` nullable com sete casas. (`app/ponto/registro_batida.py:152`; `:157`; `:331`; `:333`; `:335`; `app/ponto/models.py:58`; `:59`)

**Encaminham geo à porta:** os dois kernels e a triagem. A sanitização valida com `float(v)` sem substituir o valor original: preserva `0.0`; vazio, nulo e não numérico viram `None`. (`app/api/views.py:648`; `:654`; `app/api/views_core.py:640`; `:646`; `app/ponto/services/triagem_batida.py:701`; `:707`; `app/ponto/registro_batida.py:11`; `:15`; `:17`)

**Leitores do resultado da função:** geofence, filtro dos feeds e contador offline. Há ainda uma chave homônima `tem_gps` produzida por cálculo próprio no drawer e consumida pelo template diagnóstico. (`app/ponto/services/geofence.py:269`; `app/core/templatetags/hasner_filters.py:34`; `app/ponto/views.py:485`; `app/colaboradores/services/drawer.py:63`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:49`)

**Sim, outros leitores aceitam zero como coordenada:** detalhe seleciona por nulidade e emite pontos de mapa; pino calcula distância; vínculo usa zero como evidência. O teste novo da obra exige explicitamente esse último comportamento. (`app/colaboradores/services/detalhe.py:22`; `:53`; `:82`; `app/templates/colaboradores/detalhe.html:231`; `app/colaboradores/views.py:2474`; `:2479`; `app/ponto/services/vinculo_na_lista.py:49`; `:57`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`)

**Decimal/float/string:** zero numérico é falsy; string `"0.0"` é truthy. `floatdot` converte Decimal, float ou string numérica para float e imprime seis casas com ponto; string inválida retorna intacta. É formatação, não juiz de presença. O comportamento do objeto recém-criado antes de recarga do banco permanece **`[nao verificado]`**, pois a porta preserva o tipo recebido. (`app/ponto/services/geofence.py:145`; `app/core/templatetags/hasner_filters.py:22`; `:25`; `:26`; `app/ponto/registro_batida.py:15`)

**9. Os quatro achados procedentes do Opus**

| Achado | Situação atual |
|---|---|
| Zero acendia pino; teste confundia preservação com validade | **Curado nos sítios apontados:** feeds consultam o juiz e API afirma zero como ausência na leitura. Permanecem as divergências externas descritas nos itens 5/8. (`app/templates/core/_d2_feed.html:16`; `app/templates/ponto/batidas_offline_pendentes.html:96`; `app/api/tests/test_l030_batida_pela_porta.py:63`) |
| RED contra a base sem log | **Não demonstrado; `[nao verificado]`.** Testes atuais não comprovam execução histórica contra `da829e9f6`. (`CLAUDE.md:28`) |
| Pouso sem LEI-AKITA/L-030 | **Parcial:** documentação corrigida; mensagem do commit e passagem dos portões **`[nao verificado]`**. (`app/docs/LEIS.md:78`; `CLAUDE.md:63`) |
| LEIS:78/:165 falsas | **Curado:** ambas removem as barreiras GPS/situação e conservam a ressalva de tipo oposto. (`app/docs/LEIS.md:78`; `:165`; `app/ponto/registro_batida.py:150`) |

O smoke col677 nas duas cascas continua sendo requisito separado; sua realização **`[nao verificado]`**. (`app/docs/CORTES.md:440`)

**10. Classes estruturais e commit** [nao verificado]

**Não identifiquei novo juiz duplicando autoridade registrada, novo escritor fora da porta ou relógio introduzido pela extração:** `tem_gps` é puro, as perguntas registradas são distintas e os kernels encaminham à mesma porta. (`app/ponto/services/geofence.py:145`; `app/core/juizes.py:1191`; `:1201`; `app/api/views_core.py:640`; `app/api/views.py:648`)

**Derivação paralela e dois sentidos de “tem geo” permanecem:** zero significa ausência para o juiz e amostra geográfica para leitores por nulidade. É dívida explicitamente adiada pelo aval, inclusive na mesma página do feed; falta seu registro concreto no BACKLOG disponível. (`app/ponto/services/geofence.py:145`; `:52`; `app/templates/core/dashboard2.html:242`; `app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`)

Não encontrei falta de mordida nos selos examinados; **RED histórico, ordem de trava, liberação de vaga, pouso após pré-requisitos e smoke ficam `[nao verificado]`**. Os requisitos constam no corte e no protocolo. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:128`; `app/docs/CORTES.md:440`; `CLAUDE.md:369`)

**Linha LEI-AKITA completa no commit e citação L-030: `[nao verificado]`.** Sem histórico/mensagem do commit, não é possível conferir origem, testemunha, RED, quem-mais-lê, juízes novos e padrão; a forma obrigatória está em `CLAUDE.md:63`. [nao verificado]

## 10/11 18:23 · sessao 4c9560844f07 · - · - · origem=Ronald (tmux)

6. **Registro e corte cumpridos.** `tem_gps` consta em `app/core/juizes.py:1201`; a frase literal `corte Ronald: juiz tem_gps nasce` consta em `app/docs/CORTES.md:442`, exatamente o padrão exigido por `bin/tests/test_juiz_novo_tem_corte.sh:41`. O selo foi inspecionado, sem execução, pois cria temporário e pode reescrever a base (`bin/tests/test_juiz_novo_tem_corte.sh:32`, `:52`, `:57`).

**Não duplica `classificar_posicao`:** esse juiz recebe distância/accuracy/raio e classifica dentro/fora/incerto (`app/ponto/services/geofence.py:95`, `:114`); `tem_gps` recebe latitude/longitude e responde presença (`:126`, `:145`). O registro também distingue explicitamente as duas perguntas (`app/core/juizes.py:1191`, `:1201`).

**Documentação gerada:** comparei em memória, sem escrita:

- `MAPA-JUIZES.md` integral **igual** à saída real de `bin/gerar_mapa_juizes.py:17` até `:65`; as perguntas são ordenadas por `sorted` em `:44`. A ordem atual — tem GPS antes de foto — está correta (`app/docs/MAPA-JUIZES.md:18`, `:19`, `:20`).
- `ARQUITETURA.mmd` integral **igual** à montagem real de `app/core/management/commands/gerar_diagrama.py:911` até `:919`, usando todos os blocos reais. O nó batida vem do comprimento do registro (`:344`, `:346`).
- `MAPA.md`: **todas as linhas conferidas, exceto o contador de portas em `app/docs/MAPA.md:231`**. O ambiente não tem Django; esse contador precisa do resolver (`app/core/portas_da_tela.py:108`, `:112`). Comparei o restante usando `mapa_cobertura(portas=[])`, cuja única diferença foi essa linha; portanto mudanças v7, tabela `app/docs/MAPA.md:93` e seção `:237` até `:248`, batem com o gerador (`app/core/management/commands/gerar_diagrama.py:827`, `:834`, `:850`). Igualdade integral incluindo os números 168/147: **[nao verificado]**.
- `app/docs/JUIZES.md` continua um lookup sem entrada para `tem_gps`; declara que o vivo manda e manda corrigir divergência (`:7`, `:14`) e a tabela termina em `:121`. Está incompleto quanto ao novo juiz; não contém semântica GPS contraditória.

**Registro de pendências não materializado:** o comentário promete que os demais leitores viram BACKLOG (`app/core/juizes.py:1200`, `app/docs/CORTES.md:442`), mas o registro da família permanece vazio (`app/core/juizes.py:1266`). No BACKLOG, identifiquei apenas a linha original da obra, ainda falando em censo em curso e construção futura (`app/docs/BACKLOG.md:1873`); não identifiquei linha específica das divergências geo7 ou BO-0611-05-RESTO-DA-CLASSE **[nao verificado quanto a eventual registro externo]**.

9. **Quatro achados procedentes do Opus:**

- Zero/pino: cura dos dois leitores e teste do escritor pertencem à revisão dos outros agentes; juiz atual declara zero sem GPS (`app/ponto/services/geofence.py:136`, `:145`).
- RED contra `da829e9f6` com log guardado: **[nao verificado]**; não há logs/histórico fornecidos na cópia.
- Linha LEI-AKITA e citação L-030 no commit: **[nao verificado]**; sem `.git`/mensagem do commit. A obrigação é verificável no código do selo, mas isso não comprova o commit (`bin/tests/test_lei_akita.sh:18`, `:22`, `:394`).
- LEIS falsas: **curadas** em `app/docs/LEIS.md:78` e `:165`; ambas removem a alegação de 403 GPS/situação e preservam o resto da L-030 como PELA-METADE por colisão tipo oposto, consistente com `app/ponto/registro_batida.py:138`, `:145`, `:150`.

Smoke de Ronald/col677 nas duas cascas e ordem de pouso após liberação de vaga: **[nao verificado]**; requisitos constam em `app/docs/CORTES.md:440` e despacho ainda futuro em `app/docs/RELATO.md:16`.

10. **Classes estruturais:** nesta extração não aparece novo escritor, leitura de relógio ou trava: `tem_gps` é função pura de dois valores (`app/ponto/services/geofence.py:126`, `:145`), e seu registro não compete com juiz já registrado (`app/core/juizes.py:1191`, `:1201`). Re-derivações remanescentes estão assumidas no comentário, mas não entraram no contador de pendentes (`app/core/juizes.py:1200`, `:1266`); isso é dívida de fonte única ainda presente, cuja migração o aval mandou adiar (`app/docs/CORTES.md:442`). Certificação de toda a obra nas demais classes, commit completo e pré-requisitos históricos: **[nao verificado]**.

## 10/11 18:22 · sessao a76dbf51bc37 · - · - · origem=Ronald (tmux)

- **Novo selo morde por inspeção:** testa zeros float/int/Decimal/`0E-7`, ausentes e parciais, exige ambos os vereditos na grade e rejeita nulidade (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:31`, `:65`, `:72`, `:82`). O filtro é provado por retorno sentinela e argumentos exatos (`:97`).
- **Mutante exigido existe:** cada leitor e cada casca têm controle verde, mutante de nulidade com coordenada válida ainda mostrando link, zero causando a mesma asserção vermelha e leitor vizinho intocado verde (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:128`). A substituição exige exatamente uma ocorrência da guarda; âncora inexistente não passa silenciosamente (`app/ponto/tests/test_bo0611_05_templates_geo.py:51`).
- **Duas cascas são renderizadas:** `CASCAS` enumera `base.html` e `base_app.html`; o loader substitui somente a base e usa templates reais. Feed usa página de ensaio, offline usa sua página consumidora; isso não equivale a smoke de clique em produção (`app/ponto/tests/test_bo0611_05_templates_geo.py:31`, `:100`, `:107`, `:145`).
- **Sem falha engolida pelo `subTest` nos mutantes críticos:** o novo selo e o de templates não abrem `subTest` dentro do método cuja falha é capturada; render e controles ficam fora de `assertRaises` (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:132`, `:137`; `app/ponto/tests/test_bo0611_05_templates_geo.py:190`). Nos testes puros antigos, os `subTest` são externos à asserção esperada (`app/ponto/tests/test_bo0611_05_leitores_geo.py:142`; `app/ponto/tests/test_l030_forma_js.py:88`).
- **Offline sair de `leitores_geo` não afrouxou a cobertura funcional:** o substituto extrai a expressão real de `com_gps` por AST, testa completos/zeros/parciais/mistura, mata nulidade e exige import do juiz sem redefinição (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:170`, `:184`, `:193`, `:198`). O selo antigo explica a transferência e mantém três sítios (`app/ponto/tests/test_bo0611_05_leitores_geo.py:3`, `:23`).
- **Contrato e API foram fortalecidos:** contrato compara o registro completo incluindo a nova autoridade; API continua provando uma escrita e coordenadas preservadas, acrescentando leitura de zero como `False` (`app/ponto/tests/test_contract_juiz_batida.py:164`; `app/api/tests/test_l030_batida_pela_porta.py:40`, `:56`).
- **Mudança do selo modal:** deixou de exigir `naoBloquear` quando não existe chamada `hxConfirmar`; permanece exigindo-o quando existe. A nova cobertura JS proíbe confirmação antes de POST/fila, inclusive com comentário `naoBloquear` enganoso (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:135`; `app/ponto/tests/test_l030_forma_js.py:95`).
- **Executei 14 testes puros, todos verdes**, incluindo controles/mutantes de porta, JS, leitores e detector; os testes correspondentes estão em `app/api/tests/test_l030_selo_porta.py:68`, `app/ponto/tests/test_l030_forma_js.py:71`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:114`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:88`. Resultado desta execução não tem arquivo de log por proibição de escrita: `[nao verificado]`. Django não está instalado neste ambiente; render/POST não foram reexecutados: `[nao verificado]`.
- **Ressalva da obra inteira:** o selo do detector exige que zero gere evidência de vínculo divergente, inclusive `0/0`; é divergência residual explicitamente codificada, anterior à volta 7, não mudança comprovada de semântica nesta volta (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`). Os selos analisados não estão entre os três testes listados em quarentena (`app/core/testes_em_quarentena.txt:6`).

## 10/11 18:22 · sessao 14926ad172a8 · - · - · origem=Ronald (tmux)

3. Os três leitores curados consultam o juiz: `_d2_feed.html:16`, `batidas_offline_pendentes.html:96`, `ponto/views.py:485`; filtro sem regra própria em `core/templatetags/hasner_filters.py:33-34`. O `load` está no próprio arquivo consumidor, então não depende da raiz: `_d2_feed.html:1`; offline:4. Todos os caminhos de produção encontrados entregam model: feed usa `list[Batida]` em `core/cell.py:9-20` e `core/views.py:1162-1171`; offline usa QuerySet de Batida em `ponto/views.py:441-446,489` e itera em template:53. Não há dict/values nesses caminhos. O filtro aceita apenas objetos com atributos; dict ou None causa AttributeError em `hasner_filters.py:34`. Não há catch na view offline ao renderizar (`ponto/views.py:488`) nem no render do feed (`core/live_cell.py:22,27-28`, `core/views.py:1171`). Framework Django instalado/propagação efetivamente executada: [nao verificado]. Se propagar, offline perde a resposta da página; feed perde a resposta do fragmento/JSON, enquanto o dashboard já aberto mostra “Erro ao carregar” no drawer (`dashboard2.html:169-174`), portanto não afirmar que sempre derruba toda a página.

4. Universo de `pendentes`: todas Batidas `origem='offline_pendente', aprovada=False`, ordenadas por timestamp; nenhum teto, paginação, período, filtro explícito por empresa ou colaborador nessa view (`ponto/views.py:441-446`). Há COUNT SQL total e iteração Python completa das coordenadas (`:484-485`), depois queryset completo para render (`:489`, template:53). `sem_gps = total - com_gps` usa a mesma decisão (`:486`); cards e linhas coerentes para mesmo conjunto estável (`template:19,23,27,96-100`). Concorrrência entre consultas e volume real: [nao verificado]. Não há filtros GPS próprios na página inspecionada (`templates/ponto/batidas_offline_pendentes.html:1-127`).

5. Permanecem predicados próprios do censo, por grupo:
- Truthiness das duas: drawer `colaboradores/services/drawer.py:63`; JS dashboard `templates/core/dashboard2.html:242,287`.
- Truthiness individual/serialização: geofence `:349-350,391-392`; painel `core/services/painel_op.py:227-228`; painel cell `colaboradores/painel_cell.py:54-55`; situacional `colaboradores/services/situacional.py:199-200`.
- Nulidade conjunta: `chamados/views.py:692`; `ponto/services/geofence.py:52,425`; `ponto/services/vinculo_na_lista.py:49-50`; `colaboradores/views.py:53,337,356,395,2474`; `colaboradores/services/detalhe.py:22,53`; templates modal:480 e relatório:30.
- Nulidade individual de serialização: `ponto/views.py:468-469`; `core/views.py:1024-1025`.
- `isNaN`: `static/js/geo-colab-map.js:39`; `templates/chamados/fio_colaborador.html:313`; `templates/chamados/painel_gestao.html:583`.
- Cópias AlertaGeofence: `ponto/models.py:263`; comando reconciliar:42; detalhe template:142; supervisor:90.

Há leitores na MESMA página do feed: o clique da linha abre drawer (`_d2_feed.html:4`), usando JSON com floats/nulos (`core/views.py:1024-1025`) e novo julgamento JS (`dashboard2.html:242,287`). Para floats usuais, inclusive zero, o resultado atual coincide; dependência do juiz está ausente nesses dois sítios. Não encontrou segundo julgamento GPS na tela offline (`template:1-127`).

Alimentam alerta/decisão: `geofence.vinculo_contagem` aceita zero por nulidade e calcula distância (`:52,58-61`), `vinculo_diverge` o consome (`:64-67`); `vinculo_na_lista` inclui zero e calcula centroides/distâncias/classificação (`:49-50,57,73-90`), lavra snapshot (`:112-122`) que o geofence lê por `_cadastro_em_revisao` (`geofence.py:70-83`). `reavaliar_chamados_geofence` inclui zero por nulidade (`:425`) e pode retratar chamados conforme distâncias (`:430-440`). Dinheiro diretamente a partir desses leitores: [nao verificado].

BACKLOG: busca por BO-0611-05, tem_gps ou censo_geo7 no arquivo encontrou somente a linha antiga de obra/censo1 (`app/docs/BACKLOG.md:1873`), sem linha específica das divergências v7. Ausência em outros destinos: [nao verificado].

8. Escritor de fato: `ponto/registro_batida.py:152-158`; sanitização `:5-18`, chamada antes da escrita `:105`. Valida via `float(v)` mas preserva valor original (`:15`); `0.0` fica intacto, vazio/nulo/não numérico vira None (`:11-17`). Semeadura também dentro da mesma porta, guardada para ensaio (`:331-335`). Encaminhadores com geo: `api/views.py:648,654-655`; `api/views_core.py:640,646-647`; `ponto/services/triagem_batida.py:701,707-708`. Campos model DecimalField sete casas (`ponto/models.py:58-59`).

Resultado nomeado `tem_gps`: consumidores produção em geofence:269, filtro:34, contador view:485. Outra chave `tem_gps` é produzida com conta própria em drawer:63, lida duas vezes em `templates/colaboradores/partials/_drawer_diagnostico.html:49`; não é chamada ao juiz.

Sim, leitores ainda aceitam zero:
- Detalhe seleciona por nulidade (`colaboradores/services/detalhe.py:22,53`), emite floats (`:82-83`) e oferece link (`templates/colaboradores/detalhe.html:188`) e mapa/pino (`:231-239`).
- `_geo_do_pino` só recusa nulos (`colaboradores/views.py:2474`), calcula distância de zero (`:2479`) e classifica (`:2486-2487`).
- Vínculo e reavaliação descritos acima.
- Teste NOVO da obra exige zero como amostra geográfica numérica no detector (`ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:134-139`).

Juiz aceita strings não vazias como truthy, inclusive `"0.0"`; essa é inferência direta do `bool(latitude and longitude)` (`geofence.py:146`). Model carregado do banco usa DecimalField (`models.py:58-59`); tipo exato de objeto recém-criado antes de refresh pode conservar float/string recebido porque sanitização não converte (`registro_batida.py:15`). Geofence principal avalia zero numérico como sem_gps e gera alerta com coordenadas None (`geofence.py:269-278`), contextos usam truthiness individual (`:349-350,391-392`). `floatdot` converte Decimal/float/string numérica para `float` e formata seis casas com ponto (`hasner_filters.py:22-25`); string inválida retorna intacta (`:26-27`). Logo não valida presença e não resolve `"0.0"` truthy; conversão para float pode perder precisão antes das seis casas (`:25`).

## 10/11 18:13 · sessao 05b0ac66455f · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não liberaria a obra inteira ainda:** restam geração fora da transação do núcleo, reuso estrangeiro ignorado na agregação, derivação paralela do dia no auditor e ampliação de travas na recuperação. (`app/chamados/services/fabrica_da_celula.py:174`; `app/chamados/services/disputa_emissao.py:596`; `app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:659`)

1. **Caminho (a): a geração principal foi protegida, mas ainda existe geração posterior ao commit do núcleo.** Nascimento e agregação chamam o gerador dentro de `atomic`. Porém o wrapper chama `perguntar_entradas_pendentes` depois desse retorno; ela também chama `gerar_perguntas_disputa`. (`app/chamados/services/disputa_emissao.py:534`, `:596`, `:625`, `:748`, `:294`)

   Existe caminho operacional sem transação externa: consumidor → fábrica → núcleo → segunda geração. A fábrica chama o wrapper em `:172` e diretamente o gerador em `:174`; o consumidor só abre seu próximo `atomic` depois dessas chamadas e captura eventual exceção sem desfazer o núcleo concluído. Portanto **sim**, pode haver DS commitada antes de uma geração posterior levantar; isso, sozinho, não demonstra que ela ficou sem pergunta. (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`, `:301`; `app/chamados/reconciliador.py:1387`, `:1388`, `:1392`)

   Censo dos chamadores operacionais diretos:

   | Porta | Chamador | Transação da pergunta |
   |---|---|---|
   | `_abrir_ou_agregar_disputa` | Recuperação recursiva, `app/chamados/services/disputa_emissao.py:659` | Geração dentro dos atomics da recuperação e do núcleo, `:651`, `:596`. |
   | Mesma | `disputa_para_declaracao`, `app/chamados/services/disputa_emissao.py:691` | Desliga geração, `:695`; consumidores criam a pergunta na transação deles. |
   | Mesma | Wrapper, `app/chamados/services/disputa_emissao.py:727` | Principal dentro; pendentes depois, `:748`. |
   | Mesma | Grade, `app/ponto/management/commands/reconciliar_grade.py:424` | Passa motivos vazios, `:425`; pode nascer sem pergunta, pois o laço não executa, `app/chamados/services/disputa_emissao.py:312`. |
   | Mesma | Smoke, `app/chamados/management/commands/smoke_chamados.py:99` | Repete geração em `:102`, mas tudo está no atomic externo com rollback, `:39`, `:43`. |
   | `criar_ou_atualizar_disputa` | APIs, `app/api/views.py:794`; `app/api/views_core.py:794` | Principal no núcleo; ressalva dos pendentes em `app/chamados/services/disputa_emissao.py:748`. |
   | Mesma | PWA, `app/ponto/services/triagem_batida.py:734` | Mesma situação do wrapper. |
   | Mesma | Retro de selfie, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Mesma situação do wrapper. |
   | Mesma | Fábrica, `app/chamados/services/fabrica_da_celula.py:172` | Segunda geração explícita em `:174`. |

   Os consumidores da declaração mantêm DS e pergunta na mesma transação: `declarar_hora_colab` abre atomic, chama a porta e cria pergunta; `aplicar_declaracao` faz o mesmo. (`app/chamados/services/declaracao_texto.py:348`, `:360`, `:376`, `:654`, `:659`, `:666`)

   Os outros nascimentos operacionais são: **agrupador**, DS e perguntas dentro do atomic, preservando os `continue` sem ata; **âncora**, DS e `abrir_conversa` dentro do mesmo atomic. Ambos continuam caminhos próprios; a âncora não inspeciona o retorno estrangeiro da porta. A grade usa o núcleo, sem quarto `objects.create`. (`app/chamados/services/disputa_emissao.py:1115`, `:1186`, `:1216`, `:1238`, `:1276`, `:1307`, `:1385`, `:1421`, `:1434`; `app/ponto/management/commands/reconciliar_grade.py:424`)

   Chamadas diretas adicionais em testes estão em `app/chamados/tests/test_3b_reuso_746.py:23`, `:31`; `test_a2_batida_sem_linha.py:30`; `test_a2_data_evento_batida.py:24`, `:36`, `:43`, `:49`, `:54`; `test_bo_disputa_sem_pergunta.py:29`; `test_e1_fila_v2_2_integracao.py:132`; `test_e1_v6_horizonte.py:268`, `:269`, `:275`, `:451`, `:452`; `test_e1_volta2.py:171`; `test_plantio_noturno_mais1.py:206`, `:213`; `test_regressao_etapa7.py:226`, `:241`, `:244`, `:260`, `:264`; `test_smoke_ciclo_disputa.py:69`, `:70`; `app/ponto/tests/test_selfie_ausente_juiz.py:122`. A execução ORM dessas chamadas é [nao verificado].

2. **Caminho (b): correto no nascimento; incompleto na agregação.** No nascimento, o detector identifica perguntas de outras disputas e levanta dentro do atomic. A captura fica fora dele; depois reentra no ramo existente de agregação para cada dona. Isso desfaz o ensaio antes da agregação definitiva. (`app/chamados/services/disputa_emissao.py:624`, `:628`, `:636`, `:648`, `:651`, `:659`)

   A dona **fechada** é selecionada explicitamente, sem filtro de aberta. O ramo salva somente motivos/contexto, preservando `fechada_em` e a FK da pergunta. Não introduz segundo escritor desses campos fora da porta. (`app/chamados/services/disputa_emissao.py:562`, `:570`, `:591`, `:594`, `:659`; `app/chamados/models.py:3008`)

   **Falha residual:** quando já existe DS aberta, o ramo ignora o retorno estrangeiro do gerador. Uma aberta vazia — possível pela grade — recebe o motivo, enquanto a dona da canônica não recebe essa agregação. A sonda com funções reais extraídas por AST e doubles confirmou aberta com motivo, dona sem ele e I7a=1; PostgreSQL/signals são [nao verificado]. (`app/chamados/services/disputa_emissao.py:564`, `:570`, `:596`, `:597`; `app/ponto/management/commands/reconciliar_grade.py:424`; infraestrutura da sonda em `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:82`)

3. **Batida: ficou fora do atomic novo; sucesso da resposta não está protegido.** As APIs registram a batida antes de chamar a disputa; o atomic novo é interno ao núcleo. O PWA também registra primeiro. (`app/api/views_core.py:681`, `:794`; `app/api/views.py:684`, `:794`; `app/ponto/services/triagem_batida.py:701`, `:734`; `app/chamados/services/disputa_emissao.py:534`)

   O chokepoint insere a batida e protege a etapa posterior de fechamento contra exceções. Já as chamadas de disputa nas APIs não possuem captura envolvente, e o núcleo captura somente `_PerguntaDeOutraDisputa`: uma exceção do gerador pode impedir a resposta normal depois da gravação. Portanto o patch não envolve a batida no rollback novo, mas também não prova “batida nunca barrada” como sucesso HTTP. Persistência efetiva/HTTP em runtime são [nao verificado]. (`app/ponto/registro_batida.py:157`, `:175`, `:179`; `app/api/views_core.py:794`; `app/api/views.py:794`; `app/chamados/services/disputa_emissao.py:648`)

4. **HX-ATA-NAO-SABE: comportamento preservado no código atual; igualdade byte a byte com a base [nao verificado].** A cópia não fornece histórico Git para comparação independente; o patch anexado não declara hunks no agrupador nem no teste HX, enquanto as entregas alegam comparação AST do agrupador — isso não equivale a comparação de bytes. [nao verificado] [nao verificado]

   A abstenção atual continua marcando falta de ata e evitando o fechamento; o teste continua exigindo zero perguntas, disputa aberta e WARNING `auto_fecho_disputa`, com cenários de contraste com ata. (`app/chamados/services/disputa_emissao.py:1234`, `:1235`, `:1272`, `:1273`, `:1303`, `:1304`, `:1342`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, `:58`, `:63`, `:77`)

5. **I7a/I7b: universo e estado da ata estão corretos; a fonte do dia não está inteiramente unificada.** O filtro significa DS aberta com **zero linhas** de pergunta, sem filtro de vitalidade. “SEM ata” usa literalmente `_marcos_faltantes(...)[0] == 'sem_celula'`; dia indeterminável e `fora_vinculo` caem em I7a. Não nasceu outro predicado de presença de ata. (`app/chamados/management/commands/auditar_invariantes_chamados.py:67`, `:73`, `:74`; `app/chamados/services/declaracao_texto.py:91`)

   Para órfãos identificados por IDs, reaproveita os leitores dos ramos correspondentes: `data_do_chamado`, `resolver_data_evento` com batida-origem e `data_evento_do_contexto`. Não troca diretamente pelo dia civil da abertura. (`app/chamados/management/commands/auditar_invariantes_chamados.py:33`; `app/chamados/services/disputa_emissao.py:1232`, `:1268`, `:1295`)

   **Há derivação paralela fora desse ramo:** o gerador pode resolver por `_data_furo_da_grade`, que funde contexto do pai e prioriza `entrada_sem_saida.entrada_id`/`entrada_aberta_em`. O auditor ignora essas fontes e segue para `ultima_batida` ou pai/contexto. Além disso, havendo data explícita em algum motivo, retorna imediatamente, enquanto o gerador resolve separadamente cada motivo. (`app/chamados/services/disputa_emissao.py:53`, `:59`, `:68`, `:70`, `:346`, `:371`; `app/chamados/management/commands/auditar_invariantes_chamados.py:48`, `:50`, `:51`, `:58`)

   A divergência foi reproduzida com AST real e doubles: origem no turno 08/07, contexto com 09/07; fonte do gerador devolve 08/07 e auditor consulta 09/07. Assim pode classificar como I7b pela ata do dia diferente. Ocorrência em produção é [nao verificado]. (`app/chamados/services/disputa_emissao.py:68`, `:76`; `app/chamados/management/commands/auditar_invariantes_chamados.py:58`, `:74`)

6. **QuerySet: uma avaliação no caminho novo; o risco apontado não é dupla avaliação.** `_universo` fica verdadeiro, portanto `if _universo or _pks(disputas)` faz curto-circuito: `_pks` só itera `disputas` em `:2671`. Não há duas avaliações entre as quais uma dona possa nascer nesse caminho. Mesmo fora dele, reiterar o mesmo QuerySet normal pode usar seu cache; comportamento Django em runtime aqui é [nao verificado]. (`app/chamados/services/disputa_emissao.py:553`; `app/chamados/models.py:2669`, `:2671`, `:2674`)

   A entrada inicial pede C → IDs → D por PK → Ch por PK; perguntas entram depois pela porta. (`app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692`)

   **A recuperação deixa uma lacuna na ordem por transação:** primeiro trava donas retornadas + abertas + chamados; a recursão volta a pedir **todas** as DS. Uma fechada adicional pode entrar depois de Ch já travado. A sonda da volta 2 não cobre esse caminho: prepara aberta e substitui o gerador por Mock. (`app/chamados/services/disputa_emissao.py:652`, `:659`, `:551`; `app/chamados/models.py:2688`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:29`, `:133`)

   Isso não demonstra deadlock: C retido serializa escritores conformes. Deadlock efetivo com outro escritor, ou prova global de ausência dele, é [nao verificado]. A disciplina declarada é C primeiro e ordem crescente por tabela. (`app/chamados/models.py:2631`, `:2666`)

7. **Tetos/C8: justificativa corresponde à leitura adicional, mas não certifica os totais.** Existe a seleção de PKs de todas as DS do colaborador, inclusive fechadas, depois da trava C; a justificativa está junto dos dois tetos, como exige C8. (`app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2666`, `:2671`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `:193`; `PLANO_PISCADA.md:10`)

   É uma consulta adicional **por invocação do núcleo**. Linhas transferidas/travadas crescem com o histórico de DS; custo total não é constante. A fábrica também pode chamar a porta várias vezes, em laço por motivo. (`app/chamados/services/disputa_emissao.py:551`; `app/chamados/services/fabrica_da_celula.py:168`, `:172`)

   A sonda varia chamados vivos, mantém duas DS e simula consultas; não mede o SQL completo de `registrar_batida`. Totais finais **145/219 são [nao verificado]**. (`app/chamados/tests/test_bo_disputa_volta2_memoria.py:24`, `:28`, `:30`, `:133`, `:142`)

8. **Os erros do RED informado não provam o bug (a).** ImportError/KeyError do contador provam ausência do símbolo; não observam I7a=1. O teste ORM importa esse símbolo na carga, e os testes de exceção em memória consultam o contador antes das contagens diretas. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:9`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:280`, `:296`)

   O modo `--base` atual usa **emissor antigo + auditor novo como sonda**, resolvendo essa deficiência metodológica. Nesse arranjo, o teste de exceção antes da pergunta confronta I7a com zero em `:280`; o teste após inserção confronta diretamente DS/pai/pergunta/log em `:297`; o teste de profundidade confronta `[1,1]` em `:381`. Reprodução desse modo contra o Git da base nesta cópia é [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:28`, `:201`, `:204`, `:275`, `:285`, `:381`)

   AST recompila os **corpos extraídos da cópia**, sem reimplementação manual do núcleo, gerador ou contador; entretanto ORM, atomic, juiz da ata e porta da pergunta são doubles. Isso testa lógica Python, sem certificar SQL, signals ou concorrência. Os wrappers exigem controle verde e morte por assertion, recusando erro de montagem. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:36`, `:69`, `:88`, `:176`, `:224`, `:449`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:174`)

   Há testes com mordida positiva — pergunta existe antes da falha, lote exige DS=2/PD=3 — e um teste fraco: declaração apenas exige `assert_not_called`, passando se a função virar no-op. A sonda de travas ignora predicados no double e devolve sempre as duas DS, portanto não comprova a seleção SQL. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:336`, `:384`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:74`, `:84`)

9. **Escritores e leitores dos campos centrais:**

   | Campo | Escritores | Leitores relevantes |
   |---|---|---|
   | `DS.motivos` | Núcleo, agrupador, âncora; portas de remoção/reabertura. (`app/chamados/services/disputa_emissao.py:570`, `:620`, `:1146`, `:1194`, `:1318`, `:1429`; `app/chamados/models.py:1966`, `:2063`, `:2159`) | Gerador, auditor e relatório. (`app/chamados/services/disputa_emissao.py:312`; `app/chamados/management/commands/auditar_invariantes_chamados.py:48`; `app/chamados/relatorios_disputa.py:52`) |
   | `DS.contexto` | Núcleo, pendentes, agrupador/âncora, remoção/anotar/renovar. (`app/chamados/services/disputa_emissao.py:591`, `:621`, `:293`, `:1189`, `:1424`; `app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122`) | Fonte da grade, gerador, pendentes e auditor. (`app/chamados/services/disputa_emissao.py:54`, `:273`, `:345`, `:372`, `:387`; `app/chamados/management/commands/auditar_invariantes_chamados.py:29`) |
   | `DS.fechada_em` | Fechar e reabrir admin/sistema. (`app/chamados/models.py:1932`, `:2149`, `:2173`) | Seleção do núcleo, auditor e S127. (`app/chamados/services/disputa_emissao.py:558`, `:562`; `app/chamados/management/commands/auditar_invariantes_chamados.py:69`; `app/chamados/juizes.py:2316`) |
   | `DS.chamado` | Três nascimentos; FK admite SET_NULL. (`app/chamados/services/disputa_emissao.py:622`, `:1190`, `:1425`; `app/chamados/models.py:1749`) | Auditor, fonte da grade, S127 e signal do pai. (`app/chamados/management/commands/auditar_invariantes_chamados.py:30`; `app/chamados/services/disputa_emissao.py:55`; `app/chamados/juizes.py:2316`; `app/chamados/signals.py:32`) |
   | `Pergunta.disputa` | `_nascer`; reuso devolve existente preservando FK. (`app/chamados/models.py:3014`, `:3008`) | Detector estrangeiro, auditor pela relação inversa, S127 e relatório. (`app/chamados/services/disputa_emissao.py:628`; `app/chamados/management/commands/auditar_invariantes_chamados.py:69`; `app/chamados/juizes.py:2319`; `app/chamados/relatorios_disputa.py:12`) |
   | Saída do auditor | `add`, apresentação stdout e HTML. (`app/chamados/management/commands/auditar_invariantes_chamados.py:636`, `:1915`, `:1930`) | Cron pede HTML; placar depende da execução. Leitura humana efetiva é [nao verificado]. (`app/config/crons.py:542`, `:747`) |

   As chaves de origem também têm produtores existentes: API escreve `ultima_batida` e `entrada_sem_saida`; agrupador escreve `chamados_orfaos_ids`; fábrica escreve data por motivo. O auditor lê essas informações parcialmente, conforme o conflito do item 5. (`app/api/views_core.py:669`, `:788`; `app/chamados/services/disputa_emissao.py:1179`, `:1189`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/management/commands/auditar_invariantes_chamados.py:33`, `:48`, `:51`)

   **Pergunta morta contar como linha não conflita com os leitores existentes:** relatório distingue lista vazia de lista sem vivas; fechamento distingue `VAZIA` de `VEREDITO`; S127 bloqueia zero linhas ou fio vivo. O conflito encontrado é a escolha do dia, não esse significado de “sem pergunta”. (`app/chamados/relatorios_disputa.py:12`, `:18`; `app/chamados/models.py:1903`; `app/chamados/juizes.py:2320`)

10. **Classes reprovadoras e metadados.** Estão presentes a **derivação paralela de fonte única** e a **lacuna na ordem de trava da recuperação**; há ainda geração posterior ao núcleo e cobertura incompleta do reuso na agregação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:652`, `:659`, `:596`; `app/chamados/services/fabrica_da_celula.py:174`)

   Não aparece novo predicado concorrente de “SEM ata”, nem segundo escritor de motivos no tratamento novo: ele chama o juiz existente e reentra na porta existente. Também não aparece leitura nova de relógio na classificação I7a/I7b; `ultima_em` continua sendo timestamp da agregação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:74`; `app/chamados/services/disputa_emissao.py:659`, `:587`)

   **Linhas LEI-AKITA dos commits, ordem real de pouso e cumprimento do pré-requisito BO-0611-05: [nao verificado].** O molde exigido inclui origem, testemunha, RED, leitores, juízes e padrão; o ciclo exige suite e revisão antes do pouso. (`CLAUDE.md:62`, `:63`, `:65`; `app/docs/LEIS.md:147`)

   Para fundamentar o pouso, cabem **L-001/L-002** — origem e fonte única —, **L-003** — medição —, **L-007** — escritor único —, **L-030** — batida — e **L-123/L-126** — padrão e certificação. O índice fornecido também aponta **L-050**, cuja fronteira exportada não pode ser reintroduzida. (`app/docs/LEIS.md:59`, `:60`, `:61`, `:65`, `:78`, `:91`, `:144`, `:147`)

   Isso é diferente da cobrança automática por sítio: o selo exige citação quando o hunk cruza função da coluna **PROTEGE**; uma menção ao arquivo em DOSSIES/CORTES não basta para afirmar cobrança mecânica. Resultado desse selo para o pouso é [nao verificado]. (`bin/tests/test_lei_protege_sitio.sh:13`, `:77`, `:83`, `:176`, `:180`)

   Não identifiquei afirmação específica de **LEIS.md/JUIZES.md que a cura torne falsa**; identifiquei código novo contrariando a exigência de fonte única. JUIZES continua sendo lookup, com correção obrigatória se divergir do código; não justifica cadastrar outro juiz para legitimar a derivação paralela. (`app/docs/LEIS.md:60`; `app/docs/JUIZES.md:7`, `:14`; `app/chamados/management/commands/auditar_invariantes_chamados.py:46`)

## 10/11 18:11 · sessao 9d5fc4a2156d · - · - · origem=Ronald (tmux)

Achado principal: há derivação paralela do dia no auditor, com divergência reproduzida em memória. O comentário “Mesma precedencia do gerador” não corresponde integralmente ao código: auditor usa datas explícitas → `ultima_batida` da DS → dia forte do pai/contexto; gerador pode antes usar `_data_furo_da_grade`, que funde contexto do pai e prioriza `entrada_sem_saida.entrada_id`/`entrada_aberta_em`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:46`; `app/chamados/services/disputa_emissao.py:53`; `app/chamados/services/disputa_emissao.py:68`; `app/chamados/services/disputa_emissao.py:346`.)

Executei AST real de `_data_furo_da_grade` e `_dias_da_disputa_sem_pergunta`, com ORM/turno/ata doubles do próprio teste: DS com `entrada_sem_saida.entrada_id` apontando turno 08/07 e `data_turno=09/07`; fonte do gerador respondeu 08/07, auditor respondeu 09/07. Com juiz double `sem_celula` apenas em 09/07, contador publicou `(0,1)`. A cena depende dos doubles; ocorrência em produção [nao verificado]. A divergência decorre das linhas citadas e do fallback final do auditor. (`app/chamados/management/commands/auditar_invariantes_chamados.py:51`; `app/chamados/management/commands/auditar_invariantes_chamados.py:58`; `app/chamados/services/disputa_emissao.py:76`.)

Há outra diferença: se **algum** motivo tem data explícita, auditor retorna imediatamente o conjunto dessas datas e ignora o fallback para motivos sem data; gerador decide data separadamente dentro do laço de motivos. (`app/chamados/management/commands/auditar_invariantes_chamados.py:48`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/services/disputa_emissao.py:371`.)

4. Integridade HX:

- A cópia não possui `.git`; comparação direta byte a byte contra base não foi possível [nao verificado].
- O `diff_obra.patch` fornecido não contém hunk no agrupador nem no teste HX; isso demonstra apenas que o anexo não declara alteração ali. Base independente e igualdade byte a byte [nao verificado]. [nao verificado]
- O código atual mantém a abstenção: `_ata_do_dia` lê `_marcos_faltantes`, com `sem_celula` e escala vigente acende/recusa; agrupador marca `_absteve_por_ata` e evita fecho quando ela estiver marcada. (`app/chamados/services/disputa_emissao.py:958`; `app/chamados/services/disputa_emissao.py:965`; `app/chamados/services/disputa_emissao.py:1234`; `app/chamados/services/disputa_emissao.py:1272`; `app/chamados/services/disputa_emissao.py:1303`; `app/chamados/services/disputa_emissao.py:1342`.)
- O teste atual exige zero perguntas, DS aberta e WARNING `auto_fecho_disputa`; há contraste com ata, dia completo e fechamento. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:77`.)

5. Contadores:

- Universo correto para zero **linhas**: `fechada_em IS NULL` e relação `perguntas IS NULL`; pergunta morta continua excluindo DS desse universo, pois nenhuma coluna de vitalidade é filtrada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:67`.)
- “SEM ata” é literalmente `_marcos_faltantes(...)[0] == 'sem_celula'`, sem novo julgamento de presença de ata; `fora_vinculo` e dia indeterminável ficam no I7a. (`app/chamados/management/commands/auditar_invariantes_chamados.py:70`; `app/chamados/services/declaracao_texto.py:91`.)
- Para IDs dos órfãos, usa leitores existentes: `cobra_marcos_da_ata` + `data_do_chamado`; batida origem + `resolver_data_evento`; senão `data_evento_do_contexto`. São os leitores usados nos ramos correspondentes do agrupador, com a observação de que o auditor chama resolver com motivo fixo `orfao_14h`; resolver aceita os dois motivos e delega ao mesmo turno. (`app/chamados/management/commands/auditar_invariantes_chamados.py:33`; `app/chamados/services/disputa_emissao.py:1232`; `app/chamados/services/disputa_emissao.py:1268`; `app/chamados/services/disputa_emissao.py:1295`; `app/escala/servico_jornada.py:474`.)
- Isso não reproduz literalmente o booleano de abstenção: agrupador passa por regularizabilidade, base, classificação e exceção “sem escala vigente”; contador pergunta apenas estado atual do juiz para todos os IDs existentes. A especificação dada exige estado `sem_celula`, portanto não proponho adicionar os gates ao contador. (`app/chamados/services/disputa_emissao.py:1129`; `app/chamados/services/disputa_emissao.py:1263`; `app/chamados/services/disputa_emissao.py:1298`; `app/chamados/services/disputa_emissao.py:971`; `app/chamados/management/commands/auditar_invariantes_chamados.py:73`.)
- I7a continua violação; I7b sempre INFO, inclusive zero, e HTML usa cor neutra/cabeçalho “numero (I7b: publicado)”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:77`; `app/chamados/management/commands/auditar_invariantes_chamados.py:81`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1915`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1928`.)

9. Inventário dos campos no caminho analisado:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| `DS.motivos` | núcleo agrega/insere (`app/chamados/services/disputa_emissao.py:570`, `:620`); agrupador/âncora (`:1146`, `:1188`, `:1194`, `:1223`, `:1318`, `:1423`, `:1429`); portas aparar/remover/reabrir (`app/chamados/models.py:1966`, `:2063`, `:2159`) | gerador usa lista recebida (`app/chamados/services/disputa_emissao.py:312`); auditor deriva dias dos motivos (`app/chamados/management/commands/auditar_invariantes_chamados.py:48`); relatório imprime rótulos (`app/chamados/relatorios_disputa.py:52`) |
| `DS.contexto` | núcleo merge/nascimento (`app/chamados/services/disputa_emissao.py:577`, `:591`, `:621`); entradas pendentes (`:293`); agrupador/âncora (`:1189`, `:1424`); portas de remoção/anotar/renovar (`app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122`) | gerador/fonte da grade (`app/chamados/services/disputa_emissao.py:54`, `:345`, `:372`, `:387`); entradas pendentes (`:273`); auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:29`) |
| `DS.fechada_em` | fechamento (`app/chamados/models.py:1932`); reaberturas admin/sistema (`:2149`, `:2173`) | núcleo seleciona abertas ou dona explícita (`app/chamados/services/disputa_emissao.py:558`, `:562`); auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`); S127 (`app/chamados/juizes.py:2316`); signal do pai (`app/chamados/signals.py:28`) |
| `DS.chamado` | três nascimentos do emissor (`app/chamados/services/disputa_emissao.py:622`, `:1190`, `:1425`); FK admite SET_NULL (`app/chamados/models.py:1749`) | auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:30`); fonte da grade (`app/chamados/services/disputa_emissao.py:55`); S127 (`app/chamados/juizes.py:2316`); signal encerra pai (`app/chamados/signals.py:32`) |
| `Pergunta.disputa` | `_nascer` insere (`app/chamados/models.py:3014`); reuso retorna `ex` sem mudar FK (`:3008`) | detector estrangeiro (`app/chamados/services/disputa_emissao.py:628`); relação inversa do auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`); S127 (`app/chamados/juizes.py:2319`); relatório (`app/chamados/relatorios_disputa.py:12`) |
| saída auditor | `add` monta total/amostra (`app/chamados/management/commands/auditar_invariantes_chamados.py:636`); stdout/HTML (`:1915`, `:1930`) | cron publica com `--html` (`app/config/crons.py:542`); placar depende da execução do auditor (`app/config/crons.py:747`); leitor humano efetivo [nao verificado] |

Leitores existentes já distinguem zero linhas de perguntas mortas: relatório retorna SEM_PERGUNTA só com lista vazia, PRONTA_FECHAR quando há linhas mas nenhuma viva; fechamento usa `vazia` só para zero linhas; S127 bloqueia zero linhas ou fio vivo. Portanto não há conflito nesse significado do novo contador. (`app/chamados/relatorios_disputa.py:12`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/models.py:1903`; `app/chamados/juizes.py:2320`.)

A derivação paralela de dia descrita acima é o conflito concreto encontrado; exaustividade de todo escritor dinâmico/JSON e leitores externos [nao verificado].

## 10/11 18:10 · sessao 5b97b39adce8 · - · - · origem=Ronald (tmux)

Sonda em memória executada sem criar arquivo, com `python3 -B`, rc=0; usa `Ambiente` e funções extraídas da cópia por AST (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:82`, `:213`).

Resultado confirmado por assertions:

- Dona fechada `pk=1`: motivos continuam `['foto_ausente']`.
- DS aberta inicialmente vazia `pk=2`: recebe `['batida_fora_escala_grave']`.
- Retorno da emissão: DS `pk=2`.
- Pergunta canônica permanece ligada à dona `pk=1`.
- Total DS: 2; contadores `(I7a, I7b) = (1, 0)`.

A causa é o ramo de agregação ignorar o retorno do gerador (`app/chamados/services/disputa_emissao.py:596`) e retornar a DS aberta (`:597`); o detector de dona estrangeira existe apenas no nascimento (`:624`, `:628`, `:636`). Portanto há classe residual de (b) quando a DS aberta já existe vazia; a nova DS não nasce nessa chamada, mas o motivo fica na DS errada e I7a acusa 1 (`app/chamados/services/disputa_emissao.py:564`, `:570`, `:596`).

PostgreSQL, signals e concorrência: [nao verificado].

## 10/11 18:10 · sessao 6a680ef2adf3 · - · - · origem=Ronald (tmux)

Itens 6–8, referências relativas à raiz.

**6. QuerySet e ordem**

- No caminho novo, `_pks(disputas)` executa **uma vez**: o núcleo passa `abertas_do_colaborador=True` e um colaborador, tornando `_universo=True`; a expressão `if _universo or _pks(disputas)` faz curto-circuito. Portanto a ocorrência de `:2674` não reitera o QuerySet nesse caminho. Não existem duas avaliações entre as quais uma dona possa nascer. `app/chamados/services/disputa_emissao.py:553`; `app/chamados/models.py:2669`; `app/chamados/models.py:2671`; `app/chamados/models.py:2674`.
- A sequência inicial é C → leitura de IDs → D ordenadas por pk → Ch ordenados por pk; P entra posteriormente pela porta. `app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`.
- **Ressalva concreta na ordem por transação:** o tratamento de reuso trava somente donas retornadas + abertas + Ch vivos; a recursão amplia novamente para **todas** as DS, inclusive fechadas não retornadas. Havendo outra DS fechada X, a recursão pode adquirir X depois de Ch já adquirido no tratamento. Isso contradiz a ordem literal global por transação. `app/chamados/services/disputa_emissao.py:651`; `app/chamados/services/disputa_emissao.py:653`; `app/chamados/services/disputa_emissao.py:659`; `app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2688`.
- Não decorre automaticamente um deadlock: C continua retido e serializa os escritores que seguem a mesma disciplina. Deadlock real ou escritor concorrente que contorne C: [nao verificado]. A porta declara C primeiro e ordenação crescente por tabela. `app/chamados/models.py:2631`; `app/chamados/models.py:2666`.
- A sonda da volta 2 não cobre essa reentrada: cria DS aberta para entrar em agregação e substitui o gerador por Mock. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:29`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:133`.

**7. Tetos/C8**

- A justificativa corresponde à expressão ORM real: `values_list('pk', flat=True)` de **todas** as DS do colaborador, avaliada após C. `app/chamados/services/disputa_emissao.py:551`; `app/chamados/models.py:2666`; `app/chamados/models.py:2671`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:193`.
- É uma leitura adicional **por chamada do núcleo**; não é custo constante em linhas transferidas/travadas, pois cresce com o histórico de disputas. Também não significa uma única chamada por julgamento: a fábrica chama o núcleo em laço por motivo. `app/chamados/services/disputa_emissao.py:551`; `app/chamados/services/fabrica_da_celula.py:168`; `app/chamados/services/fabrica_da_celula.py:172`.
- A sonda varia quantidade de Ch vivos, mantém duas DS e simula as consultas; prova número de avaliações nesse recorte, não os totais SQL 145/219. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:24`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:28`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:30`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:142`. Totais SQL finais: [nao verificado].
- C8 exige justificativa escrita junto ao número; isso existe. `PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:193`.

**8. Mordida e limites**

- O RED informado de ImportError/KeyError comprova falta do símbolo, **não** a observação de I7a=1 para o bug (a). O teste ORM importa o símbolo durante carga; os testes de exceção em memória consultam o contador antes de conferir as tabelas diretamente. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:9`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:280`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:296`.
- O modo `--base` atual resolve esse problema metodológico ao carregar somente o emissor antigo e manter o auditor atual como sonda. Nesse arranjo, `test_a_excecao_do_gerador_desfaz_agregado_e_preserva_batida` pode falhar por ASSERT em `:280`, observando `(1,0)` contra `(0,0)`; o teste após inserção pode falhar nas contagens `:297`. Isso é prova híbrida explicitada, não execução integral da base. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:28`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:201`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:204`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:275`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:285`.
- Os testes extraem e recompilam **o corpo real do código**, sem reimplementação manual do núcleo/gerador/contador. Criam novos objetos-função em namespace substituído: atomic, ORM, juiz da ata e porta P são doubles. Não certificam signals, SQL nem concorrência PostgreSQL. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:36`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:69`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:88`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:176`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:224`.
- A sonda volta 2 extrai a implementação real de `trava_na_ordem`, mas `Trava.filter` ignora o predicado e o resultado D contém sempre as duas DS fabricadas. Portanto comprova ordem de chamadas e argumentos, não que o SQL efetivamente seleciona o conjunto correto. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:74`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:84`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:97`.
- Não há vacuidade geral nos cenários principais: existe assert positivo de pergunta antes da exceção e o lote exige DS=2/PD=3. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:336`.
- **Teste individual fraco:** `test_declaracao_delega_pergunta_ao_consumidor` só exige `gerar.assert_not_called()`, sem conferir nascimento/retorno; passaria se o wrapper virasse no-op. A checagem estática de settings usa glob sem assert de universo não vazio, embora os arquivos existam nesta cópia. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:384`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:427`.

Executei somente Python `-B`: controles atuais passaram 14+2 testes; mutantes gerador/reuso/contador foram mortos com 5/2/5 assertions e trava repetida/consulta antecipada com 3/1 assertions. Evidência de execução desta sessão sem arquivo persistido: [nao verificado]. Os wrappers exigem controle verde e assertions, recusando erros de montagem. `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:449`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:174`.

Comparação e execução direta da base indisponíveis porque `git show` retorna 128 nesta cópia; não substituí o RED informado por esse erro ambiental. [nao verificado]

## 10/11 18:10 · sessao 5b97b39adce8 · - · - · origem=Ronald (tmux)

1. **Há geração depois do atomic do núcleo.** O núcleo agora gera dentro de `atomic` tanto no nascimento quanto na agregação (`app/chamados/services/disputa_emissao.py:534`, `:596`, `:625`), mas o wrapper ainda chama `perguntar_entradas_pendentes` depois do retorno (`:748`), e essa função chama novamente o gerador (`:294`). Além disso, `fabrica_da_celula.perguntar` chama o wrapper (`app/chamados/services/fabrica_da_celula.py:172`) e depois chama diretamente o gerador (`:174`). Existe um caminho concreto sem atomic externo: consumidor `app/chamados/reconciliador.py:1387` → fábrica `:301` → `:219` → `:172/:174`; o atomic do consumidor só começa depois, em `app/chamados/reconciliador.py:1392`. A exceção da segunda geração é capturada em `:1388`, sem desfazer o núcleo anteriormente concluído. A reprodução SQL/produção é [nao verificado].

2. **Censo de chamadores operacionais diretos.**

   | Porta chamada | Chamador | Situação da pergunta |
   |---|---|---|
   | `_abrir_ou_agregar_disputa` | Recursão de reuso, `app/chamados/services/disputa_emissao.py:659` | Geração no atomic recursivo, dentro do atomic do tratamento `:651/:596`. |
   | `_abrir_ou_agregar_disputa` | `disputa_para_declaracao`, `app/chamados/services/disputa_emissao.py:691` | Desliga geração `:695`; consumidores abaixo constroem pergunta no atomic deles. |
   | `_abrir_ou_agregar_disputa` | Wrapper, `app/chamados/services/disputa_emissao.py:727` | Geração principal dentro `:625/:596`; pendentes depois `:748/:294`. |
   | `_abrir_ou_agregar_disputa` | Grade, `app/ponto/management/commands/reconciliar_grade.py:424` | Passa `motivos=[]` `:425`; o laço do gerador não executa (`app/chamados/services/disputa_emissao.py:312`). Pode nascer DS sem pergunta. |
   | `_abrir_ou_agregar_disputa` | Smoke, `app/chamados/management/commands/smoke_chamados.py:99` | Gera de novo `:102`, mas o `handle` envolve tudo em atomic com rollback `:39/:43`; não é exemplo de DS commitada antes da segunda geração. |
   | `criar_ou_atualizar_disputa` | APIs, `app/api/views.py:794`, `app/api/views_core.py:794` | Geração principal no núcleo; pendentes podem gerar depois (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | PWA, `app/ponto/services/triagem_batida.py:734` | Mesma porta e ressalva dos pendentes (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | Retro de selfie, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Mesma porta e ressalva dos pendentes (`app/chamados/services/disputa_emissao.py:748`). |
   | `criar_ou_atualizar_disputa` | Fábrica, `app/chamados/services/fabrica_da_celula.py:172` | Segunda geração explícita fora do atomic interno `:174`. |

   Os únicos consumidores operacionais de `disputa_para_declaracao` encontrados são `declarar_hora_colab` e `aplicar_declaracao`: abrem atomic em `app/chamados/services/declaracao_texto.py:348/:654`, chamam a porta em `:360/:659` e abrem pergunta em `:376/:666`. Assim DS e pergunta pertencem à mesma transação desses consumidores; o retorno estrangeiro da pergunta nesses consumidores não passa pelo detector novo de nascimento do núcleo (`app/chamados/services/disputa_emissao.py:624`).

3. **Outros nascimentos de DS.** O agrupador cria em `app/chamados/services/disputa_emissao.py:1186`, dentro do atomic `:1115`; perguntas de intervalo, ata, origem e fallback ficam dentro dele (`:1216`, `:1238`, `:1276`, `:1307`). A ausência de ata conserva os `continue` (`:1235`, `:1273`, `:1304`) e não fecha a DS quando `_absteve_por_ata` (`:1342`). A âncora cria DS em `:1421` e chama `PerguntaDisputa.abrir_conversa` em `:1434`, ambas dentro do atomic `:1385`; ignora o retorno da porta `:1434`. A grade já usa o núcleo com lista vazia (`app/ponto/management/commands/reconciliar_grade.py:424`), não um quarto `objects.create`.

4. **Reuso no nascimento atende o mecanismo pedido.** O gerador devolve as perguntas obtidas da porta (`app/chamados/services/disputa_emissao.py:439/:444/:470`). No nascimento, o detector identifica `pergunta.disputa_id != disputa.pk`, distribui motivos por dona e levanta dentro do atomic (`:628/:635/:636`). A captura fica fora desse atomic (`:648`); o tratamento abre outro atomic (`:651`) e reentra no próprio ramo de agregação (`:659`). A dona é selecionada explicitamente sem filtro de aberta (`:562`); o save grava apenas `motivos`/`contexto` (`:570/:591/:594`), preservando `fechada_em` e FK da pergunta. Não há novo escritor de motivos fora do núcleo nesse tratamento (`:659`).

5. **Mas o ramo de agregação ignora a pergunta estrangeira.** Quando já existe DS aberta, o código salva os motivos nela e chama `gerar_perguntas_disputa` sem inspecionar o retorno (`app/chamados/services/disputa_emissao.py:564/:594/:596/:597`). Se essa DS aberta estiver vazia — o caminho da grade permite isso (`app/ponto/management/commands/reconciliar_grade.py:424`) — e a porta devolver canônica de outra DS, ela pode continuar sem pergunta própria e com o motivo, enquanto a dona não recebe a agregação pelo mecanismo novo. O teste de estrangeira ORM só prepara dona fechada e ausência de DS aberta nova (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:70`); a reprodução desse cenário adicional é [nao verificado].

6. **Batida não entrou no atomic novo, mas a resposta pode falhar.** APIs gravam antes de chamar a disputa (`app/api/views_core.py:681/:794`, `app/api/views.py:684/:794`); o atomic novo pertence somente ao núcleo (`app/chamados/services/disputa_emissao.py:534`). PWA também registra antes (`app/ponto/services/triagem_batida.py:701/:734`). `registrar_batida` faz o `Batida.objects.create` em `app/ponto/registro_batida.py:157` e protege a etapa de fechamento posterior com `except Exception` (`:175/:179`). Porém a chamada de disputa das APIs não está protegida por um `try` envolvente (`app/api/views_core.py:794`, `app/api/views.py:794`); o núcleo captura apenas `_PerguntaDeOutraDisputa` (`app/chamados/services/disputa_emissao.py:648`). Logo um erro do gerador pode subir e impedir a resposta normal depois da gravação. Essa propagação já existia quando o wrapper gerava fora do atomic na base; novidade de runtime, estado HTTP efetivo e garantias PostgreSQL são [nao verificado].

## 10/11 17:59 · sessao 3960e7546cc4 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Volta 2 entregue: [ENTREGA, com comandos, RED e mutantes](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:9).

- Linhas registradas: **778, 779 e 1262**. [Registro](core/tests/test_medida_turno_do_par.py:267)
- Trava duplicada removida; tetos **145/219**, com justificativa C8. [Cura](chamados/services/disputa_emissao.py:548), [tetos](ponto/tests/test_selo_chokepoint_escrita.py:180)
- Sonda isolada: **6 → 5 consultas**, igualmente com 1 e 3 furos. [Medida](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:54)

SQL real e suite ORM completa: **[nao verificado]**. [nao verificado]

_3445 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
