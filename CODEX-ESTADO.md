# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 23:49** · publicado em **09/11/2026 23:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 23:49 · sessao 9e374e71ccea · - · - · origem=Ronald (tmux)

A sessão perguntou os temas dos cinco achados: leitores/rc e lista vazia em `preverif1.q:11-18`, pré-requisito em `preverif1.q:29-32`, e RED na base em `construtor1.q:4-11`. Portanto, “não perguntado” não explica esses temas. As linhas dos anexos aqui contam desde a primeira linha do conteúdo, excluindo `== anexo ... ==`.

| Achado | Classificação sustentada | Trechos literais e limites |
|---|---|---|
| 1 | **(c)** para a cronologia concreta; condição já percebida | `.q`: “pouso antes do pre-requisito (CORTES.md:305)” (`preverif1.q:30-31`). `.r`: “A condição exige medição do FIO MUDO e C11 pousado; o registro exige medição com C11 no ar.” e “apresentar essa evidência antes de liberar pouso” (`preverif1.r`, linha física [nao verificado]). A resposta não tinha prova do horário; não é silêncio sobre o requisito. `triagem_preverif1.txt:3` encaminha o selo à **SESSÃO, `!`**, sem demonstrar descarte da cronologia. | [nao verificado]
| 2 | **Ambiguidade entre (a) da conclusão integrada e fatos já vistos; descarte específico [nao verificado]** | `.q`: “QUEM ESCREVE e QUEM LE [...] rc [...] linhas de saida [...] Todos supoem o MESMO significado” (`preverif1.q:11-14`). `.r`: “Leitor operacional específico: [nao verificado]” para VIGIA FIO; “O pipeline também continua calculando esperados somente de `CRONS`, deixando ENCs fora desse universo” (`preverif1.r`, linhas físicas [nao verificado]). O censo também descreveu o placar raspando somente FABRICA, saúde aceitando 0/2 e pipeline CRONS versus ENC. Logo, dizer que ninguém viu esses mecanismos seria falso; porém não foi formulado o achado integrado L-062. `triagem_preverif2.txt:5` descarta **unidade mista**, não ausência de leitor/alarme. |
| 3 | **(b)** como falha de encaminhamento/fechamento da sessão, com ressalva sobre “descarte” | `.q`: “Algum leitor de JUIZES_POR_VARREDURA quebra [...] com a tupla vazia” (`preverif1.q:15-16`). `.r`: “Vazio saudável vira ‘vigia falhou’” e “Correção mínima: emitir representação explícita do universo vazio” (`preverif1.r`, linha física [nao verificado]). Triagem literal: “3 PROCEDENTE -> SESSAO, `!` [...] le universo vazio como ‘vigia falhou’” (`triagem_preverif1.txt:4`). Isso é reconhecimento e atribuição à sessão, **não descarte literal**. O único resumo final de descarte fornecido é “Descartes da triagem: todos fila.” (`opus_final1.txt:7`); motivo individual do descarte [nao verificado]. |
| 4 | **(c)**: prova em PROD | `.q`: censo pede quem conta a lista e leitores (`censo1.q:9,13`). `.r`: “preservar uma medição explícita do comando durante a verificação da retirada” (`preverif1.r`, linha física [nao verificado]). Triagem: “e o pos-pouso mede escritas_por_cron(['disparar_perguntas_competencia']) com o nome explicito” (`triagem_preverif1.txt:4`). A exigência foi vista e acolhida; execução em PROD não está demonstrada. | [nao verificado]
| 5 | **(c)**: RED Django em log/base | `.q`: “Mostre o RED (rodando na base) antes da cura.” (`construtor1.q:11`, confirmar contagem física [nao verificado]). `.r`: “A bateria Django abaixo foi tentada, mas nenhum módulo iniciou: faltou Django”; “execução ORM [...] [nao verificado]” (`construtor1.r`, linhas físicas [nao verificado]). Isso distingue RED de fonte alegado de RED Django não executado. Triagem específica do RED Django em log [nao verificado]. |

Cuidados na consolidação:

- `triagem_preverif2.txt:7` descarta **frescor pela meia-noite do host**; não comprova descarte da medição de 06:35 anterior ao C11.
- `triagem_preverif2.txt:5` descarta **unidade “perguntas” versus pauta**; não comprova descarte de L-062.
- Não há suporte para **(d) NENHUM viu**: os temas e limitações aparecem nas respostas acima.
- O achado 2 exige critério explícito: os mecanismos foram vistos; a conclusão integrada não foi registrada. Atribuir todo ele a (a) ou todo ele a (b), sem essa distinção, excede os anexos. [nao verificado]

## 09/11 23:48 · sessao a8631007fb8c · O169-FECHADA · censo · origem=Code

A premissa está registrada como **L-125**, e a cura da C20-O169 está presente no serviço. Porém, “nunca fecha com premissa viva” **não vale como garantia global**: existem fechamentos genéricos que alcançam cluster sem consultar sua assinatura. Além disso, o banco atual não distingue causalmente fechamento por idade de fechamento por lastro. (`app/docs/LEIS.md:146`; `app/ponto/services/cluster_espurio.py:53`; `app/chamados/models.py:727`; `app/chamados/models.py:292`)

Censo estático. Execução atual da suíte, execução dos mutantes, ocorrências concretas no banco e consumidores externos ao checkout: **[nao verificado]**. [nao verificado]

**1. Caminhos que encerram ou fazem desaparecer**

O cluster canônico nasce como `modulo_origem='cluster_batida_espuria'`, categoria `batida_espuria`. Ele não pertence ao universo normal de furo; portanto, a guarda genérica de premissa de `reconciliar()` retorna autorização sem consultar `FATO_CLUSTER_ESPURIO`. A guarda de perguntas vivas continua podendo impedir o encerramento. (`app/ponto/services/cluster_espurio.py:11`; `app/ponto/services/cluster_espurio.py:132`; `app/ponto/services/cluster_espurio.py:135`; `app/chamados/models.py:1244`; `app/chamados/catalogo/modulos.py:235`; `app/chamados/models.py:727`; `app/chamados/models.py:685`)

| Caminho | Condição e dependência temporal |
|---|---|
| **Serviço específico** | Seleciona todos os chamados vivos do módulo/colaborador e fecha somente os cuja célula do próprio dia demonstra premissa morta. Não recebe horizonte no seletor de fechamento. Conta apenas retorno verdadeiro de `reconciliar()`. (`app/ponto/services/cluster_espurio.py:53`; `app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:71`; `app/ponto/services/cluster_espurio.py:76`; `app/ponto/services/cluster_espurio.py:123`) |
| **Evento do cartório** | Com `apply_` e células julgadas, chama o serviço específico. O serviço exige colaborador ativo e escala ativa sem término; esse recorte limita quem é examinado, mas não impõe idade aos chamados examinados. (`app/ponto/services/cartorio.py:992`; `app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1005`; `app/ponto/services/cluster_espurio.py:117`) |
| **Detector manual** | `--apply` somente produz escrita sem `--alarme`; delega ao mesmo serviço. O cron declarado usa `--alarme`, portanto não fecha. (`app/ponto/management/commands/detectar_cluster_espurio.py:34`; `app/ponto/management/commands/detectar_cluster_espurio.py:46`; `app/config/crons.py:598`) |
| **Transição humana: resposta livre e fio** | Gestor informa status válido; chama `transicionar()`, sem condição de idade ou assinatura cluster. Há autorização contextual e guardas de justificativa/disputa/pergunta. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:711`; `app/chamados/services/fio.py:714`; `app/chamados/services/fio.py:719`; `app/chamados/models.py:135`; `app/chamados/models.py:148`) |
| **Outras ações HTTP** | Aprovar exceção e autorizar geofence também transicionam para resolvido sem filtro de origem nesses caminhos. Exceção usa `criado_em` para montar a data da batida, não como prescrição. Recusar geofence exige batida vinculada ao contexto; o contexto normal do cluster não fornece essa ligação. (`app/chamados/views.py:426`; `app/chamados/views.py:540`; `app/chamados/services/acoes_chamado.py:36`; `app/chamados/services/acoes_chamado.py:70`; `app/chamados/services/acoes_chamado.py:222`; `app/chamados/services/acoes_chamado.py:257`; `app/chamados/services/acoes_chamado.py:277`; `app/ponto/services/cluster_espurio.py:145`) |
| **Decisão administrativa do dia** | Busca todos os vivos por `data_turno` ou FK da célula e chama `reconciliar()`. A data seleciona o dia decidido; não há teto de idade nem consulta à assinatura cluster nesse fechamento. (`app/ponto/services/veredito_celula.py:148`; `app/ponto/services/veredito_celula.py:155`; `app/ponto/services/veredito_celula.py:201`) |
| **Competência trancada** | Parte dos vivos da empresa, seleciona pela competência e aplica exceções; encerra pela via `competencia_trancada`, sem consultar assinatura cluster. Depende da competência/data, não de `--dias`. (`app/chamados/services/competencia_trancada.py:82`; `app/chamados/services/competencia_trancada.py:88`; `app/chamados/services/competencia_trancada.py:96`; `app/chamados/services/competencia_trancada.py:135`; `app/chamados/services/competencia_trancada.py:184`; `app/chamados/models.py:1639`) |
| **Desligamento** | Todos os `NAO_FECHADOS` do colaborador recebem `encerrar()`, sujeito à guarda de perguntas. A data de demissão determina quando executar, sem idade do chamado. O cron efetiva demissões cuja data chegou. (`app/colaboradores/services/desligamento.py:150`; `app/colaboradores/services/desligamento.py:155`; `app/chamados/models.py:1614`; `app/chamados/models.py:1617`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`; `app/config/crons.py:440`) |
| **Signal de disputa fechada** | Disputa existente com `fechada_em` e chamado-pai ainda não encerrado chama `reconciliar()`, sem idade. Cluster como pai nessa FK no banco: **[nao verificado]**. (`app/chamados/signals.py:28`; `app/chamados/signals.py:33`; `app/chamados/signals.py:42`) |
| **Justificativa aprovada** | Origem viva apontada por `chamado_origem` recebe `retratar()`, sem filtro de módulo/idade nesse sítio. Cluster nessa FK: **[nao verificado]**. (`app/ponto/services/justificativa.py:33`; `app/ponto/services/justificativa.py:42`; `app/ponto/services/justificativa.py:46`) |
| **Materialização** | Pode reconciliar destino e clones por FK/IDs, protegendo irmãs respondidas sem consumo; há também fechamento de destino quando ausência já foi decidida. Cluster nessas ligações: **[nao verificado]**. (`app/chamados/services/materializacao.py:581`; `app/chamados/services/materializacao.py:761`; `app/chamados/services/materializacao.py:771`; `app/chamados/services/materializacao.py:773`) |
| **Regularização externa** | Chama `reg.chamado.encerrar()` por FK. O fluxo normal cria origem `regularizacao_externa`; ligação divergente com cluster: **[nao verificado]**. (`app/chamados/services/regularizacao_ext.py:201`; `app/chamados/services/regularizacao_ext.py:205`; `app/chamados/services/regularizacao_ext.py:246`) |
| **`retratar_chamados_lei_cala` manual** | Aceita `--modulo` livre. Com cluster, seleciona vivos cujo dia é anterior a hoje e cuja classificação de falta não acusa falta; `--apply` chama `retratar()`, sem consultar assinatura cluster. Coexistência concreta dessas condições com assinatura viva: **[nao verificado]**. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:24`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:33`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:40`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:45`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`) |
| **`marcar_feriado_do_posto` manual** | Parte da fila, seleciona pelo vínculo do dia e feriado alcançado, e reconcilia ao declarar posto fechado. Carrega calendário de hoje−180 até hoje: existe dependência temporal indireta no universo consultado. Não consulta assinatura cluster nesse fechamento. (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:92`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:102`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:108`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:114`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`) |

A tranca é disparada pelo ato de fechamento do período, diretamente ou por subprocesso `encerrar_por_competencia_trancada --apply`; não é um cron por desenho. (`app/ponto/services/fechamento.py:1420`; `app/ponto/services/fechamento.py:1428`; `app/chamados/services/competencia_trancada.py:281`; `app/chamados/services/competencia_trancada.py:290`; `app/config/crons.py:1273`)

Há também caminhos que **retiram da exibição sem encerrar**:

| Caminho | Condição temporal |
|---|---|
| Arquivo individual | POST autorizado; não exige idade, embora o motivo padrão diga “Silenciado por idade (30d)”. (`app/chamados/views_arquivar.py:24`; `app/chamados/views_arquivar.py:35`; `app/chamados/views_arquivar.py:37`) |
| Arquivo em lote | Exige `dias>=1`, seleciona por verbo/área e `criado_em < agora-dias`; silencia. (`app/chamados/views_arquivar.py:102`; `app/chamados/views_arquivar.py:125`; `app/chamados/views_arquivar.py:135`; `app/chamados/views_arquivar.py:140`) |
| `arquivar_prescritos` | Mesmo corte de idade por criação, default 30 dias, sem véu/resgate; motivo “Prescrito por idade”. Cron declarado desligado. (`app/chamados/management/commands/arquivar_prescritos.py:32`; `app/chamados/management/commands/arquivar_prescritos.py:53`; `app/chamados/management/commands/arquivar_prescritos.py:85`; `app/config/crons.py:1450`) |
| Competência encerrada | Vivos sem véu, dia do fato até `--corte`, sem disputa viva; silencia por dez anos. É ato manual. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:112`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:274`; `app/config/crons.py:1356`) |
| Sem dia irrecuperável | Escopo DIA, sem dia recuperável, criação anterior à competência corrente; silencia por cinco anos. (`app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:51`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:58`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:62`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:88`) |
| Isentos | Todos os vivos de isento sem véu, sem corte de idade; signals acionam o serviço. (`app/chamados/services/silencio_isentos.py:24`; `app/chamados/services/silencio_isentos.py:27`; `app/chamados/services/silencio_isentos.py:43`; `app/chamados/signals.py:274`; `app/chamados/signals.py:301`) |

Esses silêncios não escrevem `status_local`. O painel exclui silêncio ainda vigente; a partição de verbos também classifica silêncio vencido como histórico, fora da lista padrão. (`app/chamados/services/arquivo.py:154`; `app/chamados/services/arquivo.py:165`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:18`; `app/chamados/services/verbos.py:48`)

Exclusão física encontrada: reset de **todos** os chamados, condicionado a banco de ensaio e confirmação; a porta recusa produção pela flag/nome do banco. Outros apagadores de cluster em produção: **[nao verificado]**. (`app/ponto/management/commands/zerar_dados_operacionais.py:71`; `app/ponto/management/commands/zerar_dados_operacionais.py:84`; `app/chamados/models.py:563`; `app/chamados/models.py:571`; `app/ponto/registro_batida.py:326`)

Nos caminhos examinados, cartório interno/grade/lastro excluem cluster pelos universos de furo; o reconciliador genérico de premissa não coleta seu fato e, portanto, conserva. (`app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:1280`; `app/ponto/management/commands/reconciliar_grade.py:207`; `app/ponto/services/lastro.py:387`; `app/chamados/reconciliador.py:262`; `app/chamados/reconciliador.py:301`; `app/chamados/catalogo/premissa.py:127`)

**2. Entrada de `dias`, `ref`, `hoje` e horizonte**

- O comando define `--dias=14`, obtém `ref=timezone.localdate()` e passa ambos a `pendencias()` ou `reconciliar_colab()`. Seu universo de colaboradores vem de escalas ativas sem término e colaboradores ativos, opcionalmente `--colab`. (`app/ponto/management/commands/detectar_cluster_espurio.py:29`; `app/ponto/management/commands/detectar_cluster_espurio.py:35`; `app/ponto/management/commands/detectar_cluster_espurio.py:36`; `app/ponto/management/commands/detectar_cluster_espurio.py:46`; `app/ponto/management/commands/detectar_cluster_espurio.py:50`)
- `dias_com_cluster()` consulta células entre `ref-(dias-1)` e `ref`, inclusivamente. Esse resultado alimenta achados, lista e abertura. (`app/ponto/services/cluster_espurio.py:44`; `app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/cluster_espurio.py:100`; `app/ponto/services/cluster_espurio.py:107`)
- O fechamento chama `_vivos_que_a_premissa_matou(colab)`, sem `dias`, `ref` ou `hoje`; lê todos os vivos e consulta suas próprias datas. Sem dia forte recuperável, o chamado nem entra nos candidatos. (`app/ponto/services/cluster_espurio.py:53`; `app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:68`; `app/ponto/services/cluster_espurio.py:71`; `app/ponto/services/cluster_espurio.py:108`)
- Assinatura presente conserva; célula/insumos ausentes conservam como `None`; medição sem assinatura permite fechar. O juiz de dia sem trabalho prevalece sobre a ausência de insumos. (`app/ponto/services/cluster_espurio.py:25`; `app/ponto/services/cluster_espurio.py:29`; `app/ponto/services/cluster_espurio.py:33`; `app/ponto/services/cluster_espurio.py:74`; `app/chamados/catalogo/premissa.py:127`)
- `agora`/julgamento anterior participam de `na_lista`, usado na abertura; não entram em `a_fechar`. (`app/ponto/services/cluster_espurio.py:101`; `app/ponto/services/cluster_espurio.py:104`; `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:108`)

**Resposta direta:** um dia de 15+ dias com assinatura viva não fecha pelo seletor específico. Pode encerrar por caminhos genéricos, como transição humana, desligamento ou tranca; esses caminhos não impõem uma consulta à assinatura cluster. Ocorrência concreta hoje no banco: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/chamados/services/acoes_chamado.py:315`; `app/colaboradores/services/desligamento.py:155`; `app/chamados/models.py:1639`)

**3. RED da C20-O169 e mutante**

O teste é `ClusterNaoFechaPorIdadeTest.test_RED_dia_de_15_dias_com_assinatura_NAO_fecha_por_idade`, em `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74`.

Ele cria chamado para **dia do fato** `hoje−15`, com ata assinada; consulta `pendencias`, executa `reconciliar_colab` e recarrega o estado. Afirma que o chamado não está em `a_fechar` e continua em `VIVOS`. Não envelhece `criado_em`. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:43`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:65`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:75`)

O mutante **“incluir em `a_fechar` todo chamado cujo dia antecede `hoje-(dias-1)`”** fica vermelho na asserção que exige ausência da lista, mesmo que um gate posterior impeça a escrita. Se fechar pelo serviço, também viola a asserção de sobrevivência. Essa é uma conclusão estática; execução efetiva do mutante: **[nao verificado]**. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:76`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:77`)

Limites do selo:

- Mutante exclusivo no `Command.handle` não é exercitado por esse teste, que chama o serviço diretamente. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:60`; `app/ponto/management/commands/detectar_cluster_espurio.py:45`)
- Mutante baseado na idade de `criado_em` não encontra uma criação envelhecida nessa fixture. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:43`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:65`)
- Existe contraprova: dia de 15 dias **sem** assinatura deve fechar, impedindo uma cura que conserve tudo fora do horizonte. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:80`)

O pouso de 05/11 e o RED histórico de três falhas estão documentados; os logs originais não foram confirmados nesta leitura. (`app/docs/BACKLOG.md:1790`; `app/docs/RELATO.md:7243`; logs originais: **[nao verificado]**)

**4. Definição de `fechados_por_idade` pela função real**

**Não existe discriminador causal suficiente no fechamento atual.** `reconciliar()` grava via `sistema`; o núcleo carimba estado/data/via, e `estado_chamado` registra somente status anterior, novo e via. Um mutante que selecione pela idade e chame o mesmo gate produz esses mesmos registros. (`app/chamados/models.py:52`; `app/chamados/models.py:54`; `app/chamados/models.py:60`; `app/chamados/models.py:242`; `app/chamados/models.py:292`; `app/chamados/models.py:654`; `app/chamados/catalogo/motor.py:95`)

O contador atual mede **fechamentos efetivos**, usando o retorno real de `ch.reconciliar()`. A decisão de premissa existe antes dele, mas não é persistida como motivo do fechamento. Logo, esse contador não permite separar idade de lastro retrospectivamente. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/ponto/services/cluster_espurio.py:123`; `app/chamados/models.py:292`)

Também não basta contar encerrados fora do horizonte: o teste exige fechamento legítimo por lastro aos 15 dias. Consultar hoje a assinatura de um encerrado identifica uma inconsistência presente, mas não prova o fato existente no instante do encerramento. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:80`; `app/ponto/services/cluster_espurio.py:15`; `app/chamados/models.py:292`)

Há uma cegueira adicional para um vigia baseado somente em `pendencias`: o fechamento examina apenas `VIVOS`, enquanto `_sem_chamado` considera existente um chamado de qualquer estado. Um encerrado indevidamente pode deixar de aparecer tanto em `a_fechar` quanto em `a_abrir`. (`app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:81`; `app/ponto/services/cluster_espurio.py:107`)

Para que o número represente **causa histórica**, falta registrar no ato a decisão produzida pela autoridade real; imprimir zero apenas porque o seletor atual não usa idade não constitui medição independente contra o mutante. Essa conclusão decorre da diferença entre a decisão calculada e a trilha gravada. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/chamados/models.py:292`)

| Campo/trilha | Escritores e leitores encontrados |
|---|---|
| `via_encerramento` | Núcleo escreve no terminal e limpa ao voltar a vivo; catálogo significa **quem/como encerrou**, não idade/lastro. (`app/chamados/models.py:245`; `app/chamados/models.py:249`; `app/chamados/catalogo/motor.py:86`) |
| `admin` | Reabertura considera fato novo desde encerramento; vigia de furos reconhece decisão humana. (`app/chamados/models.py:1342`; `app/ponto/services/furos_sem_cobranca.py:78`) |
| `cascade` | Bloqueia renascimento automático e retorno de pai de adesão. (`app/chamados/models.py:1007`; `app/chamados/services/adesao.py:90`) |
| `competencia_trancada` | UI trata como arquivo; serviço usa para reabrir ao destrancar; resolvedor de perguntas evita interpretar como realidade regularizada. (`app/chamados/views_arquivar.py:64`; `app/chamados/views_arquivar.py:165`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/services/disputa_emissao.py:1788`; `app/chamados/services/disputa_emissao.py:1815`) |
| Via preenchida | I16 exige via em terminal, sem verificar idade versus lastro; comando de reversão fotografa a via. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1069`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:135`) |
| `estado_chamado` | Histórico apresenta; vigia lê transições e compara última trilha ao banco. (`app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:28`; `app/chamados/services/vigia_loop.py:114`) |

`motivo_fechamento` pertence à **DisputaSupervisao**, não ao chamado. A porta da disputa escreve/limpa; ide

[truncado em 20000 de 25199 caracteres]

## 09/11 23:46 · sessao 0dbebf065a8e · - · - · origem=Ronald (tmux)

Complemento confirmado:

- Os fechamentos internos do cartório em `app/ponto/services/cartorio.py:1499`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1533`, `app/ponto/services/cartorio.py:1611` e `app/ponto/services/cartorio.py:1626` **excluem cluster**: `chs` vem de `chamados_que_falam_de_batida` (`app/ponto/services/cartorio.py:1280`), que filtra `FURO_MODULES` (`app/ponto/services/cartorio.py:117`); lista/vivos derivam dele (`app/ponto/services/cartorio.py:1316`, `app/ponto/services/cartorio.py:1345`); conjunto contém somente quatro módulos sem cluster (`app/chamados/catalogo/furo_modulos.py:15`).
- O fechamento adiado de `app/ponto/services/cartorio.py:974` vem desse mesmo universo por `_adiados_pela_lampada` (`app/ponto/services/cartorio.py:1493`, `app/ponto/services/cartorio.py:1496`).
- `app/ponto/services/lastro.py:400` exclui cluster pelo filtro explícito `modulo_origem='batida_ausente'` (`app/ponto/services/lastro.py:387`).
- `_retratar` pelo absolvido da célula exclui cluster: filtro `MODULOS_DE_TURNO` (`app/chamados/reconciliador.py:996`), cuja tupla só tem três módulos de turno (`app/chamados/reconciliador.py:336`).
- `_retratar` por premissa genérica conserva cluster, pois não coleta seu fato (`app/chamados/reconciliador.py:262`, `app/chamados/reconciliador.py:290`, `app/chamados/reconciliador.py:301`, `app/chamados/catalogo/premissa.py:127`).
- Os dois chamadores de `encerrar_com_retencao` excluem cluster: bloqueio de entrada (`app/ponto/services/justificativa.py:76`, `app/ponto/services/justificativa.py:87`) e cascata de quatro módulos de turno (`app/ponto/services/justificativa.py:55`, `app/ponto/services/justificativa.py:394`, `app/ponto/services/justificativa.py:400`).
- A **origem da justificativa** pode alcançar qualquer módulo pelo FK: justificativa aprovada e origem viva → `retratar(ch)`; não há filtro de cluster (`app/ponto/services/justificativa.py:33`, `app/ponto/services/justificativa.py:42`, `app/ponto/services/justificativa.py:46`). Existência real de cluster nesse FK: [não verificado].
- `reconciliar_grade` exclui cluster por `FURO_MODULES`, além de só fornecer fatos de falta/turno/intervalo (`app/ponto/management/commands/reconciliar_grade.py:207`, `app/ponto/management/commands/reconciliar_grade.py:298`).
- Reconciliadores de geofence/vínculo não fornecem fato cluster, portanto catálogo conserva eventual cluster que chegue a eles (`app/ponto/services/geofence.py:411`, `app/ponto/management/commands/reconciliar_vinculo.py:49`, `app/chamados/catalogo/premissa.py:127`).
- `processar_alertas_turno` fornece apenas FATO_TURNO_ABERTO antes de retratar, também conservando cluster (`app/ponto/management/commands/processar_alertas_turno.py:207`, `app/ponto/management/commands/processar_alertas_turno.py:216`, `app/chamados/catalogo/premissa.py:127`).

**Novo caminho manual relevante:** `retratar_chamados_lei_cala` aceita `--modulo` livre, default batida_ausente (`app/chamados/management/commands/retratar_chamados_lei_cala.py:24`); filtra VIVOS desse módulo (`app/chamados/management/commands/retratar_chamados_lei_cala.py:33`), exige dia anterior a hoje e `classificar_falta(colab,D)[1] is None` (`app/chamados/management/commands/retratar_chamados_lei_cala.py:40`), e com `--apply` chama `retratar` sem consultar assinatura cluster (`app/chamados/management/commands/retratar_chamados_lei_cala.py:54`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`). Pode selecionar cluster com `--modulo cluster_batida_espuria`; coexistência concreta de assinatura viva e classificação sem falta: [não verificado].

**Gatilho competência trancada:** sem cron por desenho (`app/config/crons.py:1273`). `trancar_periodo` chama `disparar_no_ato` quando criou/retrancou período (`app/ponto/services/fechamento.py:1411`, `app/ponto/services/fechamento.py:1420`); até limite executa `encerrar` inline, acima dispara subprocesso `encerrar_por_competencia_trancada --apply` (`app/chamados/services/competencia_trancada.py:281`, `app/chamados/services/competencia_trancada.py:290`).

## 09/11 23:46 · sessao 2e2e8d3d1af2 · - · - · origem=Ronald (tmux)

Complemento Q1:

- **Desligamento fecha cluster por via administrativa**, sem consultar a premissa: universo todos os chamados não fechados do colaborador, `app/colaboradores/services/desligamento.py:150-155`; `encerrar()` só consulta motor e pergunta viva antes de gravar, sem gate de premissa, `app/chamados/models.py:1609-1619`. Idade do chamado não entra. Data de demissão entra no gatilho: futura fica agendada, `app/colaboradores/services/desligamento.py:39-57,82`; cron seleciona `data_demissao <= hoje`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37-40`, executa a mesma cascata em `:57`; agenda em `app/config/crons.py:440`. Entrada pela view em `app/colaboradores/views.py:1423-1434`.

- **Reset apaga cluster em ensaio**: `app/ponto/management/commands/zerar_dados_operacionais.py:27` exige banco de ensaio; `:71-76` exige `--confirmar` para escrever; `:84` chama apagador de todos os chamados. Porta `app/chamados/models.py:563-571` repete guarda e executa `.all().delete()`. Guarda recusa sem `BANCO_DE_ENSAIO` ou com nome de banco `saas_hasner`, `app/ponto/registro_batida.py:316-328`. Sem idade/janela.

- **`marcar_feriado_do_posto` pode tentar reconciliar cluster**: candidatos vêm de `qs_na_fila`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:102-109`; esse universo exclui silenciados pelo relógio, `app/chamados/services/painel.py:188-190`. Usa calendário carregado de hoje−180 até hoje, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:92-94`, vinculação do próprio dia e feriado em `:107-115`; tenta `reconciliar()` em `:140`. Gate real continua exigindo premissa: `app/chamados/models.py:682-683`.

- **Cobertura recebe cluster, mas não fecha por esses ramos**: PK genérico em `app/ponto/services/cobertura.py:93-95` e universo genérico do dia em `:225-230`; porém condições para chamar reconciliação só passam fatos de falta/turno/intervalo, `:117-122,310-317`. Cluster usa outro fato, `app/chamados/catalogo/premissa.py:52`; ausência desse fato conserva em `:127-130`.

- **Materialização por FK**: `app/chamados/services/materializacao.py:581-582` tenta reconciliar `q.chamado_destino`, sem filtro de origem nesse sítio, quando ausência não está aberta. Alcançar cluster depende da ligação; reconciliação ainda usa gate real, `app/chamados/models.py:651-654,682-683`.

- **Regularização externa por FK**: `app/chamados/services/regularizacao_ext.py:201-205` chama `reg.chamado.encerrar()` sem filtro de origem. Fluxo normal cria chamado novo com origem `regularizacao_externa` e grava a FK, `:246-257`; ligação com cluster no banco **[nao verificado]**.

- **POSTs genéricos de ação podem alcançar cluster**: aprovação de exceção roteia por ação+permissão, `app/chamados/views.py:426-427`, serviço só exclui encerrados `app/chamados/services/acoes_chamado.py:21-26` e transiciona resolvido em `:70`. Autorização geofence roteia por ação+gestor, `app/chamados/views.py:540-541`, só exclui encerrados em `app/chamados/services/acoes_chamado.py:176-177` e transiciona em `:222`. Recusa geofence exige uma batida pertencente ao contexto, `:257-265`, e transiciona em `:277`; contexto normal de cluster não grava `batida_id`, `app/ponto/services/cluster_espurio.py:145`. Sem fechamento por idade nesses trechos; aprovação de exceção usa data de criação para fabricar timestamp da batida em `app/chamados/services/acoes_chamado.py:35-38`.

- **Excluídos do cluster normal por filtro explícito**: agenda `app/chamados/services/acoes_chamado.py:88-89`; aceite de escala `:130-131`; ausência decidida `app/ponto/services/ausencia.py:709`; pós-vínculo `app/colaboradores/services/pos_vinculo.py:36`; auditoria de órfãos `app/chamados/management/commands/auditar_chamados_orfaos.py:34-36`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:28`.

- **GLPI detalhe não sincroniza fechamento local nesse caminho**: chamado local sem ticket redireciona em `app/chamados/views.py:220-222`; ramo remoto só busca ticket/followups em `:244-246` e renderiza em `:262`. Outros escritores GLPI de fechamento local não encontrados: **[nao verificado]**.

## 09/11 23:44 · sessao 0dbebf065a8e · - · - · origem=Ronald (tmux)

Censo somente leitura. A conclusão “15+ dias com premissa viva nunca fecha por qualquer caminho” **não é sustentada globalmente**: o serviço específico protege a idade, mas há portas genéricas que encerram cluster sem consultar `FATO_CLUSTER_ESPURIO` (`app/ponto/services/cluster_espurio.py:66`, `app/chamados/models.py:727`, `app/chamados/services/competencia_trancada.py:181`, `app/colaboradores/services/desligamento.py:150`).

**Caminho específico protegido**

- Evento: depois de lavrar/julgar células, se `apply_` e houver `_julgadas`, cartório chama `_cluster(c, hoje)` (`app/ponto/services/cartorio.py:992`, `app/ponto/services/cartorio.py:1001`).
- Manual: `--apply` efetivo apenas sem `--alarme`; chama `reconciliar_colab(colab, ref, dias)` (`app/ponto/management/commands/detectar_cluster_espurio.py:34`, `app/ponto/management/commands/detectar_cluster_espurio.py:45`).
- Serviço exige colaborador ativo e escala ativa sem data_fim; pode excluir colaboradores que mudaram de situação/escala, mas não restringe idade dos chamados (`app/ponto/services/cluster_espurio.py:117`).
- `a_fechar` procura TODOS VIVOS daquele colaborador/módulo, obtém o dia individual e consulta a célula desse dia, sem horizonte (`app/ponto/services/cluster_espurio.py:66`, `app/ponto/services/cluster_espurio.py:71`).
- Sem célula/insumos → `None` → conservado; assinatura presente → True → vivo; assinatura ausente ou juiz `dia_sem_trabalho` → False → candidato ao fechamento (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/cluster_espurio.py:74`, `app/chamados/catalogo/premissa.py:127`).
- Só incrementa fechados quando `ch.reconciliar()` efetivamente retorna True (`app/ponto/services/cluster_espurio.py:123`).

**Portas genéricas que podem fechar cluster**

| Caminho | Condição/recorte temporal e ressalva |
|---|---|
| Resposta livre gestor | Recebe qualquer status válido e chama `transicionar`; sem condição de idade (`app/chamados/services/acoes_chamado.py:313`). |
| Fio/resposta rápida DP | Busca chamado por pk/colaborador, exige autorização contextual e transiciona status recebido; sem idade (`app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:719`). |
| Signal disputa encerrada | Se disputa não foi criada agora, tem `fechada_em`, chamado-pai não terminal: `ch.reconciliar()`; sem idade própria (`app/chamados/signals.py:28`, `app/chamados/signals.py:41`). Vinculação real de cluster como pai no banco: [não verificado]. |
| Decisão administrativa do dia | Reconcilia TODOS chamados vivos do dia, encontrados por data_turno ou FK célula; depende do dia para selecionar, sem limite de idade (`app/ponto/services/veredito_celula.py:145`, `app/ponto/services/veredito_celula.py:201`). |
| Competência trancada | Parte de TODOS VIVOS da empresa e seleciona competência pelo juiz; exclui fora-da-tranca/resgatados/pergunta viva externa; chama `encerrar_por_competencia_trancada`, que não consulta assinatura cluster (`app/chamados/services/competencia_trancada.py:88`, `app/chamados/services/competencia_trancada.py:96`, `app/chamados/services/competencia_trancada.py:135`, `app/chamados/services/competencia_trancada.py:181`, `app/chamados/models.py:1628`). |
| Desligamento | Todos `NAO_FECHADOS` do colaborador recebem `encerrar()`, depois do tratamento das disputas; data_demissao determina o ato, sem idade do chamado (`app/colaboradores/services/desligamento.py:125`, `app/colaboradores/services/desligamento.py:150`). |
| Materialização de perguntas | Reconcilia destino e clones informados em `chamados_relacionados_ids`; protege irmãs respondidas sem consumo, sem filtro módulo/idade (`app/chamados/services/materializacao.py:761`, `app/chamados/services/materializacao.py:773`). Existência de vínculo cluster nessas posições: [não verificado]. |
| Ausência materializada | Pergunta-irmã com destino e ausência não ABERTA chama `destino.reconciliar()`, sem filtro de módulo nesse ponto (`app/chamados/services/materializacao.py:580`). Vínculo cluster real: [não verificado]. |
| Regularização externa | Se `reg.chamado_id`, confirma materialização, anexa nota e chama `encerrar()` sem filtro módulo/idade (`app/chamados/services/regularizacao_ext.py:197`). Vínculo cluster real: [não verificado]. |
| Exclusão em ensaio | `zerar_dados_operacionais` chama apagador de TODOS chamados; só aceita banco de ensaio, recusando produção por flag/nome (`app/ponto/management/commands/zerar_dados_operacionais.py:84`, `app/chamados/models.py:563`, `app/ponto/registro_batida.py:316`). |

A proteção genérica de premissa em `reconciliar()` é exclusivamente de FURO: retorna True imediatamente quando `not self.e_chamado_de_furo()` (`app/chamados/models.py:717`, `app/chamados/models.py:727`). Cluster nasce com categoria `batida_espuria`, módulo `cluster_batida_espuria`; nem módulo nem categoria constam dos universos de furo (`app/ponto/services/cluster_espurio.py:132`, `app/ponto/services/cluster_espurio.py:135`, `app/chamados/catalogo/modulos.py:235`, `app/chamados/models.py:1244`, `app/chamados/models.py:3664`). Portanto um `reconciliar()` genérico pode fechar o cluster canônico mesmo com assinatura viva, desde que a guarda das perguntas permita (`app/chamados/models.py:682`, `app/chamados/models.py:685`).

O reconciliador genérico NÃO é fechador automático de premissa cluster: identifica seu fato, mas não tem coletor `FATO_CLUSTER_ESPURIO` nos ramos implementados, passando fatos vazios ao catálogo, que conserva (`app/chamados/reconciliador.py:262`, `app/chamados/reconciliador.py:290`, `app/chamados/reconciliador.py:301`, `app/chamados/catalogo/premissa.py:127`).

**Caminhos que fazem sair da fila sem fechar**

| Caminho | Condição temporal |
|---|---|
| `arquivar_prescritos` | `--dias` default30; `criado_em < agora-dias`, vivo, sem silêncio/resgate, verbos validar/cobrar/decidir; grava motivo “Prescrito por idade” e silêncio (`app/chamados/management/commands/arquivar_prescritos.py:32`, `app/chamados/management/commands/arquivar_prescritos.py:47`, `app/chamados/management/commands/arquivar_prescritos.py:53`, `app/chamados/management/commands/arquivar_prescritos.py:84`). Cron DESLIGADO (`app/config/crons.py:1450`). |
| Arquivo individual | POST autorizado; nenhuma exigência efetiva de idade, embora motivo default diga “Silenciado por idade (30d)” (`app/chamados/views_arquivar.py:24`, `app/chamados/views_arquivar.py:35`). |
| Arquivo em lote | Exige dias>=1; filtra idade por criado_em, verbos/área e ausência de silêncio (`app/chamados/views_arquivar.py:101`, `app/chamados/views_arquivar.py:125`, `app/chamados/views_arquivar.py:135`). |
| Competência encerrada | VIVOS sem véu; dia fato <= `--corte`, sem disputa viva; cluster tem escopo DIA, portanto usa dia do fato; silêncio por 10 anos (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:112`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:165`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:274`, `app/chamados/catalogo/modulos.py:115`). Não é cron por desenho (`app/config/crons.py:1356`). |
| Sem dia irrecuperável | Escopo DIA sem dia apurável, criado_em anterior ao início da competência corrente; silêncio5anos (`app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:51`, `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:62`, `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:88`). |
| Isentos | TODOS VIVOS de isento, sem véu, inclusive cluster; não depende da idade, ignora resgate; prazo padrão a partir de agora (`app/chamados/services/silencio_isentos.py:24`, `app/chamados/services/silencio_isentos.py:39`). |

Todos esses usam `arquivo.silenciar`, que grava exclusivamente campos de silêncio/atualização e trilha, sem status (`app/chamados/services/arquivo.py:154`, `app/chamados/services/arquivo.py:165`).

Silêncio por ausência **não alcança cluster**: universo é CALADOS_POR_ISENCAO + justificativa, com guarda explícita de pertencimento (`app/chamados/services/silencio_ausencia.py:21`, `app/chamados/services/silencio_ausencia.py:60`, `app/chamados/catalogo/modulos.py:235`).

Quarentena: há cortes documentados para excluir por período sem fechar/silenciar e migrar isentos a vigia; código implementado correspondente em `app/chamados` não foi localizado [não verificado]. Referências documentais: `app/docs/CORTES.md:347`, `app/docs/CORTES.md:348`, `app/docs/CORTES.md:349`, `app/docs/CORTES.md:352`. O filtro atual de fila examinado exclui silêncio por prazo, sem chamada à quarentena (`app/chamados/services/painel.py:185`).

O selo de 15 dias no serviço específico não impede essas portas genéricas por si só; cobertura delas pelo selo: [não verificado].

## 09/11 23:44 · sessao 9dcf337f6e82 · - · - · origem=Ronald (tmux)

Q4 — o banco atual **não distingue causalmente “fechou por idade” de “fechou por lastro” quando ambos passam por `ch.reconciliar()`**: ele grava `status_local`, `resolvido_em` e `via_encerramento`, sendo a via de `RECONCILIAR` sempre `sistema`; a trilha `estado_chamado` contém somente status anterior/novo e via. Um mutante que seleciona pela idade e chama o mesmo gate produz os mesmos campos e trilha. (`app/chamados/models.py:52`, `app/chamados/models.py:54`, `app/chamados/models.py:60`, `app/chamados/models.py:228`, `app/chamados/models.py:242`, `app/chamados/models.py:292`, `app/chamados/models.py:654`, `app/chamados/catalogo/motor.py:95`)

- O contador existente de fechamento conta retorno verdadeiro do gate real, sem copiar regra: `if ch.reconciliar(): fechados += 1`. Ele não informa a causa. (`app/ponto/services/cluster_espurio.py:123`)
- A causa atual do seletor é conhecida antes do fechamento: `_leitura_da_celula` do próprio dia → `_fato` trivalente → `premissa_morta` → `a_fechar`; entretanto essa decisão e seus insumos não são gravados no fechamento. (`app/ponto/services/cluster_espurio.py:71`, `app/ponto/services/cluster_espurio.py:74`, `app/ponto/services/cluster_espurio.py:76`, `app/ponto/services/cluster_espurio.py:108`, `app/chamados/models.py:292`)
- Portanto, `fechados_por_idade` como contagem histórica comprovada exige evidência causal persistida no ato; imprimir zero porque o seletor atual não contém idade seria uma afirmação sobre o código, não uma medição independente capaz de denunciar o mutante. Essa é uma conclusão do contraste entre o seletor e o formato persistido. (`app/ponto/services/cluster_espurio.py:53`, `app/chamados/models.py:292`)
- Contar encerrados cujo próprio dia ainda possui assinatura, usando a função real, identificaria uma inconsistência atual; não provaria o motivo histórico, pois não há fotografia da leitura no fechamento. (`app/ponto/services/cluster_espurio.py:15`, `app/ponto/services/cluster_espurio.py:74`, `app/chamados/models.py:292`)
- `reconciliaria()` já oferece a decisão real de fechamento em modo somente leitura e compartilha `_decisao_do_fechamento` com `reconciliar()`; serve para contar elegibilidade presente, não fechamento ocorrido nem sua causa. (`app/chamados/models.py:651`, `app/chamados/models.py:657`)

Escritor e leitores dos campos existentes:

| Campo/trilha | Escritor/leitor e significado |
|---|---|
| `via_encerramento` | `_gravar_estado` escreve via no terminal, apaga ao voltar a vivo; catálogo define quem/como encerrou, com `admin`, `sistema`, `cascade`, `cancelado`, `superado`, `legado`, `competencia_trancada`. (`app/chamados/models.py:245`, `app/chamados/models.py:249`, `app/chamados/catalogo/motor.py:86`) |
| `resolvido_em` | Núcleo carimba quando deixa de ser vivo, preserva em resolvido→fechado e limpa ao voltar a vivo. (`app/chamados/models.py:242`, `app/chamados/models.py:248`) |
| Via `admin` | Reabertura exige fato novo desde `resolvido_em`; vigia de furos interpreta chamado superador fechado por admin como decisão humana. (`app/chamados/models.py:1342`, `app/ponto/services/furos_sem_cobranca.py:78`) |
| Via `cascade` | Bloqueia renascimento automático e reabertura de pai de adesão. (`app/chamados/models.py:1007`, `app/chamados/services/adesao.py:90`) |
| Via `competencia_trancada` | UI trata como arquivo, desarquiva por reabertura humana e inclui na lista de arquivados; resolvedor de perguntas explicitamente evita interpretar essa via como realidade resolvida. (`app/chamados/views_arquivar.py:64`, `app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:165`, `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/services/disputa_emissao.py:1812`) |
| Via `competencia_trancada` | Serviço encontra encerrados dessa via para desarquivar no destrancamento. (`app/chamados/services/competencia_trancada.py:259`) |
| Via não vazia | Auditor I16 só exige via em chamado terminal; não verifica lastro versus idade. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1069`) |
| `estado_chamado` | Escritor registra status+via antes do save; histórico apresenta descrição; vigia de loop lê transições/atores; vigia trilha×banco compara último status registrado ao banco. (`app/chamados/models.py:292`, `app/chamados/services/historico.py:86`, `app/chamados/services/vigia_loop.py:28`, `app/chamados/services/vigia_loop.py:114`) |
| Migração histórica | Backfill deriva `admin`/`cancelado`/`superado` onde consegue e usa `legado` no restante; tampouco deriva idade/lastro. (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:28`) |

Não confundir com `motivo_fechamento`: esse campo pertence à **DisputaSupervisao**, com motivos `veredito`, `vazia`, `desligamento`, `admin_encerra`, `competencia_trancada`; não é o motivo de encerramento do ChamadoColaborador. (`app/chamados/models.py:1778`, `app/chamados/models.py:1793`)

- `DisputaSupervisao.fechar` escreve motivo e trilha; reaberturas admin/sistema limpam; a porta também lê para idempotência. (`app/chamados/models.py:1881`, `app/chamados/models.py:1935`, `app/chamados/models.py:1940`, `app/chamados/models.py:2152`, `app/chamados/models.py:2176`)
- Leitores fora de testes: vigia conta disputas `vazia`; command de reabrir órfãs inclui motivo na fotografia para reversão; declaração da porta documenta escritor único. Nenhum desses leitores interpreta o campo como idade/lastro de cluster. (`app/chamados/services/vigia_fio.py:731`, `app/chamados/services/vigia_fio.py:734`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:132`, `app/core/portas.py:424`)

Q6 — resultados do grep literal solicitado:

| Arquivo:linha | Conteúdo protetor |
|---|---|
| `app/docs/LEIS.md:146` | **L-125 CLUSTER NÃO FECHA POR IDADE**: próprio dia a qualquer idade; só assinatura ausente medida ou dia sem trabalho mata; célula ausente/ata sem insumos conservam, salvo juiz de dia sem trabalho; cita C20-O169, corte 11/10 opção b, aplicação 05/11, cura `e76e0419`, selo sete casos e RED em `logs/c20_o169/red.log`; registra pendência `trabalha=False` com `ata={}` e contador de não medidos. |
| `app/docs/DOSSIES.md:54` | Ordem expressa: migrar `detectar_cluster_espurio` para dentro do julgamento da ata. |
| `app/docs/DOSSIES.md:57` | RED de idempotência por porta e medição na sombra por cron. |
| `app/docs/DOSSIES.md:58` | Selos: zero escritas fora da porta, zero juízes por varredura, zero pendências de chamado. |
| `app/docs/DOSSIES.md:59` | Proíbe retirar apply/executar sem provar na sombra que evento resolve tudo; proíbe nova autoridade e fallback. |
| `app/docs/DOSSIES.md:61` | LEI-AKITA: regra por dono, evento da célula, censo de leitores de cada campo movido. |
| `app/docs/CORTES.md:416` | Juiz compartilhado `dia_sem_trabalho`: `trabalha is False` ou tipo de ata preenchido diferente de trabalho; limbo cobra; cluster está entre os quatro emissores obrigados a perguntar. |
| `app/docs/CORTES.md:348` | Quarentena vigente por colaborador+período: juiz por data, item sai da cobrança/perguntas, **nada se apaga, fecha ou silencia**, emissores continuam; levantar devolve automaticamente. |
| `app/docs/CORTES.md:347` | Corte anterior de quarentena, explicitamente substituído pelo da linha 348. |
| `app/docs/CORTES.md:352` | Quarentena de isento sem trilha desde admissão; item sem dia entra somente em janela aberta. |
| `app/docs/CORTES.md:380` | Obra O261 mantém quarentena fora da fila pelo juiz. |
| `app/docs/CORTES.md:208` | “Quarentena” de fabricação/testes vermelhos por >15 min; contexto de esteira, distinto do chamado de cluster. |

A busca literal não encontrou `O169` em CORTES/DOSSIES, nem `detectar_cluster_espurio` em CORTES, nem quarentena em LEIS; como ausência não possui linha própria, registrar como resultado do grep, com completude absoluta `[nao verificado]`.

## 09/11 23:43 · sessao 2e2e8d3d1af2 · - · - · origem=Ronald (tmux)

**3. RED de 15 dias e alcance do mutante**

- Arquivo:teste: `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74`, `ClusterNaoFechaPorIdadeTest.test_RED_dia_de_15_dias_com_assinatura_NAO_fecha_por_idade`; data congelada em `2026-10-26 15:00:00` na linha 38.
- `_caso(15, ASSINADA)` cria chamado cujo **dia do fato** é `hoje - 15 dias`; não envelhece `criado_em`: `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:64-68`; o chamado guarda `contexto_json.data_turno`: `:43-46`.
- O helper consulta `pendencias()['a_fechar']`, executa `reconciliar_colab`, recarrega o registro e verifica participação em `VIVOS`: `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56-62`.
- Asserções: chamado fora de `a_fechar` (`:76`) e permanece vivo (`:77`).
- Mutante que faz dia do fato anterior ao horizonte entrar em `a_fechar` fica vermelho na primeira asserção, mesmo que `ch.reconciliar()` posteriormente impeça a gravação: confronto entre `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59,76` e `app/ponto/services/cluster_espurio.py:108,123-125`.
- **Limite:** mutante implantado exclusivamente no `Command.handle`, sem alterar o serviço, não é exercitado por esse RED: o helper chama serviço diretamente em `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59-60`; o caminho manual do comando fica em `app/ponto/management/commands/detectar_cluster_espurio.py:45-48`. Mutante baseado em idade de `criado_em`, em vez de `data_turno`, também não está reproduzido nessa fixture: `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:43-46,65-66`.
- Contraprova útil já existente: dia com 15 dias e ata sem assinatura **deve fechar**, `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:80-83`; evita conservação indiscriminada por idade.
- História declarada: RED “3 FAIL em a_fechar”, cura e suite documentados em `app/docs/RELATO.md:7242-7248`; entrada fechada em `app/docs/BACKLOG.md:1790`. Logs originais `logs/c20_o169/red.log` e execução efetiva do mutante nesta sessão: **[nao verificado]**.

**5. Saída atual e consumidores**

| Sítio | Texto/contrato efetivamente consumido |
|---|---|
| `app/ponto/management/commands/detectar_cluster_espurio.py:52` | stdout por achado: `  [DRY] %s %s orfas=%s missing=%d` |
| `app/ponto/management/commands/detectar_cluster_espurio.py:58` | stdout apply: `APPLY: %d chamado(s) abertos, %d reconciliado(s)` |
| `app/ponto/management/commands/detectar_cluster_espurio.py:60` | stdout dry: `DRY: %d pendencia(s) que o evento nao resolveu` |
| `app/ponto/management/commands/detectar_cluster_espurio.py:62-64` | stderr alarme: `VIGIA detectar_cluster_espurio: %d pendencia(s) que o evento nao resolveu (esperado 0): %s`, depois `SystemExit(2)` |
| `app/config/crons.py:598` | agenda 06:24 com `--alarme`, grava `cluster_espurio.log`, depende de `reconciliar_grade` |
| `app/config/crons.py:1875-1879` | redireciona stdout+stderr com `>> LOGS/log 2>&1`; `LOGS` é `/home/ronald/saas-hasner/logs`: `:19-20` |
| `bin/cron_run.sh:11-13` | executa e consome **RC**, grava JSON com `nome`, `emp`, `ini`, `fim`, `dur`, `exit`; não raspa o texto stdout |
| `bin/cron_run.sh:22-25,32-35` | RC 0 e 2 carimbam sucesso operacional; outros avisam quebra |
| `app/core/services/crons_quebrados.py:30-47` | lê JSONL do envelope; avalia `exit`, não saída textual do detector; 0/2 aceitos em `:16` |
| `bin/placar_code.sh:477-480` | raspa saída de **cron_quebrado**, regex `^crons_quebrados=\([0-9]*\)$`; não é a saída de detectar_cluster_espurio |
| `bin/placar_code.sh:484-486` | importa `config.crons.juizes_por_varredura()` e conta tamanho; não consome stdout do detector |
| `app/ponto/tests/test_varredura_cluster_evento.py:78-84` | chama detector com stdout/stderr `StringIO`, sem ler conteúdo; exige exit 2, ausência de escrita e depois ausência de alarme |
| `app/ponto/tests/test_varredura_cluster_evento.py:88-95` | verifica nome fora de juízes de varredura, papel vigia e args `--alarme` sem `--apply`; não verifica formato |
| `app/core/contratos_estruturais.py:262-270` | contrato do chamado referencia `config.crons.JUIZES_POR_VARREDURA` e pendentes; nota sobre O169. Não fornece parser de saída ali |
| `bin/gerar_estado.py:184-190` | ESTADO pode incorporar últimas 40 linhas do painel tmux, como texto bruto; publicação específica da saída do detector por esse painel **[nao verificado]** |

Não encontrei parser direto de `cluster_espurio.log`, do prefixo `VIGIA detectar_cluster_espurio` ou das linhas `DRY/APPLY` em busca textual ampla da árvore; como ausência não tem `arquivo:linha`, registro como **[nao verificado]**. A adição de linha própria preservando linhas existentes e RCs é compatível com os consumidores comprovados acima; consumidores externos ao checkout: **[nao verificado]**.

Documentação divergente já presente: `app/docs/CRONS.md:31` ainda registra `06:24 detectar_cluster_espurio --apply`, enquanto declaração executável usa `--alarme` em `app/config/crons.py:598`.

Selos vizinhos leem o **fonte**, não saída: `app/ponto/tests/test_guardas_par_relampago.py:115-126` proíbe strings de escrita em Batida e exige docstring “batida gravada NAO se toca”; `app/ponto/tests/test_detectar_cluster_espurio.py:8,56-57,73-74` importa/testa `dias_com_cluster`.

## 09/11 22:58 · sessao b8554cce33ca · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alterado somente [test_e4a2_disparar_e_vigia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-225337/app/chamados/tests/test_e4a2_disparar_e_vigia.py:61): lista fechada SQL, execução dos callbacks dentro da captura e mutante local `on_commit` (`:61`, `:320`, `:351`).

- **RED antes:** 24 falhas nos casos novos; reprodução com o coletor antigo em memória falha em `WITH … UPDATE` (`:241`, `:256`).
- **Depois:** `Ran 3 tests — OK`, incluindo rejeição dos seis mutantes do coletor (`:264`).
- **Não rodaram:** os três testes ORM, incluindo comando com/sem alarme e mutantes UPDATE síncrono/on_commit (`:304`, `:333`, `:351`). Execução e resultados desses caminhos: **[nao verificado]**. Django/freezegun indisponíveis no Python consultado; nenhuma instalação realizada. [nao verificado]

<details>
<summary>Diff aplicado</summary> [nao verificado]

```diff
--- a/app/chamados/tests/test_e4a2_disparar_e_vigia.py
+++ b/app/chamados/tests/test_e4a2_disparar_e_vigia.py
@@ -10,7 +10,7 @@
 from unittest.mock import Mock
 
 from django.core.management import call_command
-from django.db import connection
+from django.db import connection, transaction
 from django.test import SimpleTestCase, TestCase
 from django.test.utils import CaptureQueriesContext
 from freezegun import freeze_time
@@ -59,8 +59,17 @@
 
 
 def escritas_sql(consultas):
-    """DML de qualquer tabela; SAVEPOINT/RELEASE e leituras nao entram."""
-    return [q['sql'] for q in consultas if re.match(r'^\s*(INSERT|UPDATE|DELETE)\b', q['sql'], re.I)]
+    """Lista fechada de leitura/controle; SQL desconhecido conta como escrita."""
+    escritas = []
+    for consulta in consultas:
+        sql = re.sub(r'\A(?:\s+|--[^\r\n]*(?:\r\n?|\n|$)|/\*.*?\*/)*', '', consulta['sql'], flags=re.S)
+        leitura = re.match(r'^(SELECT|SAVEPOINT|RELEASE\s+SAVEPOINT|ROLLBACK\s+TO\s+SAVEPOINT|SET|SHOW)\b',
+                           sql, re.I)
+        if re.match(r'^WITH\b', sql, re.I):
+            leitura = not re.search(r'\b(INSERT|UPDATE|DELETE|MERGE)\b', sql, re.I)
+        if not leitura:
+            escritas.append(consulta['sql'])
+    return escritas
 
 
 class DispararEVigiaFonteTest(SimpleTestCase):
@@ -219,10 +228,63 @@
 
 
 class DispararEVigiaSQLTest(SimpleTestCase):
+    def _verificar_coletor(self, coletor):
+        leituras = [
+            'SELECT 1', ' select 1', 'SAVEPOINT "s1"', 'RELEASE SAVEPOINT "s1"',
+            'ROLLBACK TO SAVEPOINT "s1"', 'SET TIME ZONE UTC', 'SHOW TIME ZONE',
+            'release\n savepoint "s1"', 'rollback\tto\n savepoint "s1"',
+            '-- c\n /* outro */ SELECT 1', '/* c */ -- outro\r\n SHOW TIME ZONE',
+            'WITH x AS (SELECT 1) SELECT * FROM x',
+        ]
+        escritas = [
+            'INSERT INTO tabela VALUES (1)', '\n update tabela SET campo=1', '\tDELETE FROM tabela',
+            'WITH x AS (SELECT 1) UPDATE tabela SET campo=1',
+            'with x as (select 1) insert into tabela select * from x',
+            'WITH x AS (DELETE FROM tabela RETURNING *) SELECT * FROM x',
+            'WITH x AS (SELECT 1) MERGE INTO tabela USING x ON false WHEN MATCHED THEN DO NOTHING',
+            '/* c */ UPDATE tabela SET campo=1',
+            '-- c\n /* outro */ DELETE FROM tabela',
+            'TRUNCATE tabela', 'COPY tabela FROM STDIN', 'DO $$ BEGIN END $$', 'CALL rotina()',
+            'BEGIN', 'COMMIT', 'ROLLBACK', 'RELEASE outra_coisa', 'ROLLBACK TO outra_coisa', [nao verificado]
+            'SELECTED 1', 'SAVEPOINTED s1', 'RELEASE SAVEPOINTED s1',
+            'ROLLBACK TO SAVEPOINTED s1', 'SETTING x', 'SHOWCASE x',
+            '', ' -- so comentario', '/* comentario sem fim',
+        ]
+        for sql in leituras + escritas:
+            self.assertEqual(coletor([{'sql': sql}]), [sql] if sql in escritas else [], sql)
+
+    def test_MORDE_WITH_comentarios_e_SQL_desconhecido(self):
+        self._verificar_coletor(escritas_sql)
+
     def test_coletor_reconhece_DML_e_ignora_transacao_e_leitura(self):
         dml = ['INSERT INTO tabela VALUES (1)', '\n update tabela SET campo=1', '\tDELETE FROM tabela']
         leituras = ['SELECT 1', 'SAVEPOINT "s1"', 'RELEASE SAVEPOINT "s1"', 'ROLLBACK TO SAVEPOINT "s1"']
         self.assertEqual(escritas_sql([{'sql': sql} for sql in leituras + dml]), dml)
+
+    def test_mutantes_do_coletor_sao_rejeitados(self):
+        fonte = Path(__file__).read_text(encoding='utf-8')
+        no = next(n for n in ast.parse(fonte).body if isinstance(n, ast.FunctionDef) and n.name == 'escritas_sql')
+        fonte = ast.get_source_segment(fonte, no)
+        mutantes = {
+            'lista_aberta': ('if not leitura:',
+                             "if not leitura and re.match(r'^(INSERT|UPDATE|DELETE)\\b', sql, re.I):"),
+            'WITH_escritor_liberado': ("leitura = not re.search(r'\\b(INSERT|UPDATE|DELETE|MERGE)\\b', sql, re.I)",
+                                      'leitura = True'),
+            'MERGE_esquecido': ('INSERT|UPDATE|DELETE|MERGE', 'INSERT|UPDATE|DELETE'),
+            'comentarios_nao_removidos':
+                ("sql = re.sub(r'\\A(?:\\s+|--[^\\r\\n]*(?:\\r\\n?|\\n|$)|/\\*.*?\\*/)*', '', consulta['sql'], flags=re.S)",
+                 "sql = consulta['sql'].lstrip()"),
+            'prefixo_sem_fronteira': ('|SHOW)\\b', '|SHOW)'),
+            'ignora_minusculas': ('re.I', '0'),
+        }
+        self._verificar_coletor(escritas_sql)
+        for nome, (antes, depois) in mutantes.items():
+            with self.subTest(mutante=nome):
+                self.assertIn(antes, fonte)
+                ns = {'re': re}
+                exec(compile(fonte.replace(antes, depois), '<mutante_sql_%s>' % nome, 'exec'), ns)
+                with self.assertRaises(AssertionError):
+                    self._verificar_coletor(ns['escritas_sql'])
 
 
 class DispararEVigiaBancoTest(TestCase):
@@ -256,12 +318,13 @@
                 opts = dict(ini=dia_celula.isoformat(), fim=self.dia.isoformat(), alarme=alarme,
                             stdout=out, stderr=StringIO())
                 with CaptureQueriesContext(connection) as consultas:
-                    if alarme:
-                        with self.assertRaises(SystemExit) as cm:
-                            call_command('disparar_perguntas_competencia', **opts)
-                        self.assertEqual(cm.exception.code, 2)
-                    else:
-                        self.assertIsNone(call_command('disparar_perguntas_competencia', **opts))
+                    with self.captureOnCommitCallbacks(execute=True):
+                        if alarme:
+                            with self.assertRaises(SystemExit) as cm:
+                                call_command('disparar_perguntas_competencia', **opts)
+                            self.assertEqual(cm.exception.code, 2)
+                        else:
+                            self.assertIsNone(call_command('disparar_perguntas_competencia', **opts))
                 self.assertEqual(escritas_sql(consultas), [], out.getvalue())
                 self.assertIn('VIGIA FIO MUDO: 1', out.getvalue())
                 self.assertIn('FABRICA-POR-EVENTO: o julgamento deixou 1 ', out.getvalue())
@@ -277,12 +340,34 @@
             return ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')
 
         with CaptureQueriesContext(connection) as consultas:
-            self.assertEqual(mutante(), 1)
+            with self.captureOnCommitCallbacks(execute=True):
+                self.assertEqual(mutante(), 1)
         dml = escritas_sql(consultas)
         self.assertGreaterEqual(len(dml), 1)
         self.assertEqual(self._contagens(), antes)
         with self.assertRaises(AssertionError):
             self.assertEqual(dml, [], 'o mesmo veto a DML deve rejeitar o mutante local')
+
+    def test_MORDE_o_mesmo_coletor_acusa_UPDATE_em_on_commit(self):
+        from chamados.models import ChamadoColaborador
+        from chamados.tests.test_selo_emissor_fio_mudo import EmissorPedeDoChamadoMudoTest
+        _, ch = EmissorPedeDoChamadoMudoTest._mudo(self, 'E4A2 MUTANTE ON COMMIT') [nao verificado]
+        antes = self._contagens()
+
+        def mutante():
+            transaction.on_commit(lambda: ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante on_commit'))
+
+        with CaptureQueriesContext(connection) as consultas:
+            with self.captureOnCommitCallbacks(execute=True) as callbacks:
+                mutante()
+                self.assertEqual(escritas_sql(consultas), [], 'a escrita deve estar adiada ate a saida do contexto')
+        self.assertEqual(len(callbacks), 1)
+        dml = escritas_sql(consultas)
+        self.assertGreaterEqual(len(dml), 1)
+        self.assertEqual(ChamadoColaborador.objects.get(pk=ch.pk).titulo, 'mutante on_commit')
+        self.assertEqual(self._contagens(), antes)
+        with self.assertRaises(AssertionError):
+            self.assertEqual(dml, [], 'o mesmo veto a DML deve rejeitar o mutante adiado')
```

</details>

Comando executado para reproduzir RED e depois os três testes puros, incluindo todos os seis mutantes (`:256`, `:264`): [nao verificado]

```bash
sql_test() {
  MODO="$1" PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import os
import re
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4a2_disparar_e_vigia.py')
nodes = [n for n in ast.parse(p.read_text()).body
         if isinstance(n, (ast.FunctionDef, ast.ClassDef))
         and n.name in ('escritas_sql', 'DispararEVigiaSQLTest')]
ns = dict(ast=ast, re=re, Path=Path, __file__=str(p.resolve()),
          SimpleTestCase=unittest.TestCase)
exec(compile(ast.Module(body=nodes, type_ignores=[]), str(p), 'exec'), ns)
cls = ns['DispararEVigiaSQLTest']
if os.environ['MODO'] == 'red':
    ns['escritas_sql'] = lambda qs: [q['sql'] for q in qs
        if re.match(r'^\s*(INSERT|UPDATE|DELETE)\b', q['sql'], re.I)]
    suite = unittest.TestSuite([cls('test_MORDE_WITH_comentarios_e_SQL_desconhecido')]) [nao verificado]
else:
    suite = unittest.defaultTestLoader.loadTestsFromTestCase(cls) [nao verificado]
raise SystemExit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}
sql_test red
sql_test green
```

Saída da reprodução (`:256`, `:264`):

```text
RED:   Ran 1 test — FAILED (failures=1)
GREEN: Ran 3 tests — OK
```

Comandos dos testes ORM pendentes, para o ambiente autorizado da suite (`:304`, `:333`, `:351`); **não executados**: [nao verificado]

```bash
teste_banco=chamados.tests.test_e4a2_disparar_e_vigia.DispararEVigiaBancoTest

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_fio_mudo_conta_sem_escrever_com_e_sem_alarme" --verbosity 2
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_MORDE_o_mesmo_coletor_acusa_UPDATE_sem_mudar_contagens" --verbosity 2
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_MORDE_o_mesmo_coletor_acusa_UPDATE_em_on_commit" --verbosity 2
```

## 09/11 22:44 · sessao 9e0b7679dda1 · FIO-E0-V4-E4A-2 · verificador · origem=Code

As curas locais estão presentes; o **RED continua parcial**, e o relógio único está garantido na borda, mas não em todas as funções chamadas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/escala/servico_jornada.py:368`).

Identidade Git, ancestralidade e diffs exatos contra as duas bases: **[nao verificado]**, pois a cópia não tem `.git`. Os 140 testes informados não foram reexecutados; banco, logs e condição efetiva de pouso: **[nao verificado]**. [nao verificado]

**1. Itens destinados à volta 2**

| Item | Veredito e evidência |
|---|---|
| **1** | **CONFORME.** Ambas as asserções exigem zero, preservando as autoridades e acrescentando o histórico da retirada (`app/escala/tests/test_termometro_regua_prazo.py:12`; `app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`). |
| **4** | **PROCEDENTE — cura parcial.** AST ampliado e captura da chamada completa estão presentes; permanecem escapes por SQL não reconhecido e callbacks adiados. Correção mínima: ampliar a classificação SQL e executar callbacks dentro da captura, com mutantes correspondentes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`). |
| **5** | **CONFORME.** O ciclo acusador compara o total de `PerguntaDisputa` antes/depois, além de ausência de vivas, contador igual a um e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:275`). |
| **6a** | **CONFORME.** A docstring aponta `consumir_eventos_celula --semear` e esclarece o consumo posterior; o consumidor realmente retorna depois de semear (`app/chamados/management/commands/disparar_perguntas_competencia.py:8`; `app/chamados/management/commands/consumir_eventos_celula.py:28`). |
| **7** | **CONFORME na cura delimitada; parcial no caminho completo.** `_viva(p, hoje=hoje)` está corrigido. `_ja_perguntou → fio_vivo` continua sem esse argumento; existe ainda releitura de instante em `classificar_falta`. Correção mínima da invariável global: encaminhar dia/instante também por esses caminhos (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:125`; `app/chamados/juizes.py:1434`; `app/escala/servico_jornada.py:368`). |
| **8a** | **CONFORME na legenda prevista; PROCEDENTE no leitor operacional.** O log explica pergunta **OU Pauta DP** e existência do fato sem validação humana; o placar conserva o rótulo “perguntas”. Correção mínima: esclarecer a unidade também na saída do placar e na linha complementar `VIGIA`, preservando o prefixo raspado (`app/chamados/management/commands/disparar_perguntas_competencia.py:140`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `bin/placar_code.sh:390`). |
| **8b** | **CONFORME no comentário.** Agora declara janela GLOBAL, sem empresa, corte 21, coerente com a chamada e o default (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/ponto/janelas.py:50`). Inscrição da fila nominal prometida: **[nao verificado]**. |
| **8c** | **CONFORME na borda.** Uma captura produz `agora`, sua conversão produz `hoje`, e a janela recebe esse mesmo dia; a garantia não se estende integralmente às dependências (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/escala/servico_jornada.py:368`). |
| **8d** | **CONFORME.** O literal completo está na saída e é exigido pelo teste (`app/chamados/management/commands/disparar_perguntas_competencia.py:136`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`). |

Os dois comentários de sessão também foram ajustados para passado (`app/config/crons.py:2044`; `app/config/crons.py:2046`; `app/config/crons.py:2353`). Introdução de defeito funcional novo pelas curas: **[nao verificado]**; os limites concretos identificados estão discriminados abaixo.

**2. Quem escreve e quem lê**

| Valor | Produtor e consumidores |
|---|---|
| **`agora`** | Produzido por `timezone.now()` na borda. Recebido por `_ontem_em_janela`, fábrica da célula, barreira parcial e teto do FIO MUDO (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| **`hoje`** | Produzido por `localdate(agora)`. Alimenta competência, ontem/piso/exclusão de hoje, fábrica, teto e vitalidade direta (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/chamados/management/commands/disparar_perguntas_competencia.py:69`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| **`hoje=` de `pergunta_viva`** | `models` reexporta o juiz; o argumento entra na chave do memo, desce ao cálculo, à janela viva e à testemunha de materialização. O outro escritor explícito desse argumento é a porta de relavra (`app/chamados/models.py:4083`; `app/chamados/juizes.py:852`; `app/chamados/juizes.py:856`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1406`; `app/chamados/services/relavra_via.py:242`). |
| **Instante na barreira parcial** | `parcial_so_lista` encaminha `agora` a `houve_fato_novo`; este consulta carimbos e compara os marcos nos dois extremos. O fallback `now()` existe, mas não é usado quando recebe o instante da borda (`app/ponto/services/cartorio.py:400`; `app/ponto/services/cartorio.py:345`; `app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:417`). |
| **Instante no teto** | A fábrica encaminha para `marcos_vencidos`, que delega a `marcos_com_teto`; este compara o instante recebido, sem reler o relógio (`app/chamados/services/fabrica_da_celula.py:26`; `app/ponto/services/cartorio.py:843`; `app/ponto/services/cartorio.py:858`). O fallback `localtime()` da fábrica não é tomado com `agora` fornecido (`app/chamados/services/fabrica_da_celula.py:89`). |
| **Instante em ontem** | `_ontem_em_janela` compara instantes e encaminha `agora − 2 tetos` a `dia_encerrado` (`app/ponto/services/vigia_de_hora.py:52`; `app/ponto/services/vigia_de_hora.py:58`). |

**PROCEDENTE — ainda há duas famílias de releitura.**

- `_ja_perguntou → fio_vivo → pergunta_viva(p)` omite `hoje`, permitindo `date.today()` na janela viva. A preexistência desse caminho consta do anexo; confirmação independente contra a base: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`; `app/chamados/juizes.py:577`).
- Mesmo `_viva(..., hoje=hoje)` pode alcançar `classificar_falta`, diretamente ou pela testemunha materializada; essa função não recebe instante e consulta `_tz.now()` para o teto do turno. Portanto `hoje=` controla a janela civil, não todo o tempo usado pelo juiz (`app/chamados/juizes.py:1393`; `app/chamados/juizes.py:681`; `app/chamados/juizes.py:770`; `app/escala/servico_jornada.py:270`; `app/escala/servico_jornada.py:368`).

O teste novo exercita fragmentos da borda e a chamada de `_viva` com `Mock`; não executa esse caminho interno do juiz. Correção mínima probatória: acrescentar cenário com o juiz real e relógio que avance após a captura (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:125`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:145`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:147`).

**CONFORME nos consumidores diretos quanto a UTC/local.** O dia vem de conversão local, com São Paulo configurado; os consumidores de `agora` inspecionados comparam instantes conscientes de fuso, sem extrair hora civil crua (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/ponto/services/vigia_de_hora.py:55`; `app/ponto/services/cartorio.py:858`). Igualdade entre o dia do processo usado por `date.today()` e o dia Django em produção: **[nao verificado]**.

As janelas têm significados distintos, explicitamente no código:

| Universo | Limites |
|---|---|
| FIO MUDO | `[ini, fim]`, inclusivo (`app/chamados/management/commands/disparar_perguntas_competencia.py:93`). |
| Célula | `[_piso, fim]`, exclui hoje; competência corrente pode baixar o piso até anteontem e excluir ontem ainda atendido pelo vigia de hora (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:71`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`). |
| Janela viva da pergunta | Da véspera do início da competência anterior até o fim da corrente, inclusivamente (`app/chamados/juizes.py:597`; `app/chamados/juizes.py:603`). |
| Fato novo | `(desde, até]`; batidas candidatas em `[data−1, data+2)` (`app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:350`; `app/ponto/services/cartorio.py:352`). |
| Marco vencido | Inclui igualdade: `marco + teto <= agora` (`app/ponto/services/cartorio.py:858`). |

**PROCEDENTE — apresentação da janela permanece incompleta:** imprime somente `ini/fim`, sem o piso e as exclusões efetivas da célula. Correção mínima: explicitar também esse universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:153`; `app/chamados/management/commands/disparar_perguntas_competencia.py:70`).

| Saída | Quem escreve / quem lê |
|---|---|
| **VIGIA FIO MUDO** | Escreve `len(_mudos)`, após seleção de um motivo por passagem e dedup da chave. Leem os testes de relatório, selo, isolamento por colaborador, célula e teto (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:136`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:437`). Parser operacional específico: **[nao verificado]**. |
| **FIO DECIDIDO** | Escreve o contador incrementado por existência de linha no domínio do fato, sem filtro de validação humana. Leem o selo e o teste sintético do relatório (`app/chamados/management/commands/disparar_perguntas_competencia.py:130`; `app/chamados/management/commands/disparar_perguntas_competencia.py:140`; `app/chamados/models.py:2753`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:155`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:202`). Parser operacional específico: **[nao verificado]**. |
| **LEGENDA** | Escrita pelo comando; exigida literalmente pelo teste de relatório (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`). Leitura operacional dessa linha: **[nao verificado]**. |
| **FABRICA-POR-EVENTO** | Conta motivos, inclusive falta de Pauta DP. O placar raspa somente o inteiro; testes raspam/exigem o mesmo prefixo, e o selo shell permite literalmente esse `sed` e arquivo (`app/chamados/services/fabrica_da_celula.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `bin/placar_code.sh:388`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74`; `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; `app/chamados/tests/test_fabrica_por_evento.py:83`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:196`; `bin/tests/test_placar_conta_o_retido_do_lastro.sh:500`). |

**PROCEDENTE — os leitores não compartilham integralmente a semântica.** O placar descarta a legenda e publica `perguntas_que_o_julgamento_nao_fez`; os registros operacionais também dizem “perguntas”, embora a unidade possa ser pauta. Correção mínima: esclarecer a unidade nesses leitores, sem alterar o prefixo contratado (`bin/placar_code.sh:388`; `bin/placar_code.sh:390`; `app/docs/RELATO.md:27`; `app/docs/BACKLOG.md:1872`; `app/chamados/services/fabrica_da_celula.py:211`).

O frescor do log usa meia-noite do host, independentemente do dia capturado pelo comando; equivalência dos fusos em produção: **[nao verificado]** (`bin/placar_code.sh:389`; `app/chamados/management/commands/disparar_perguntas_competencia.py:39`). O vigia de execução usa rc, aceitando 0/2 para carimbo; não interpreta essas legendas (`bin/cron_run.sh:22`; `bin/cron_run.sh:32`).

**3. Mordida do RED SQL**

**CONFORME para UPDATE ORM síncrono.** A captura envolve `call_command` inteiro, com e sem alarme, exigindo um alvo em cada caminho. O mutante atualiza exatamente uma linha usando `.objects` e o mesmo `connection`, encontra DML mesmo com contagens inalteradas e demonstra a rejeição pelo veto (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:253`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:266`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:267`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:280`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:284`).

Esse mutante prova o **coletor**, separadamente; não é uma mutação injetada no `handle`. Para escrita ORM comum na mesma execução síncrona, a configuração CI possui somente `default`; outra conexão/thread efetivamente usada nesta execução: **[nao verificado]** (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:13`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:261`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `app/config/settings/ci.py:6`).

**PROCEDENTE — callback adiado escapa.** O teste é `TestCase` e não executa callbacks dentro da captura; a própria cópia documenta que eles ficam pendentes e que isso já ocultou defeito. Há sinais reais com escrita agendada, embora a leitura atual não acione seus `post_save` (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:228`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/ponto/tests/test_selo_chokepoint_escrita.py:197`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:17`; `app/chamados/signals.py:227`; `app/chamados/signals.py:238`).

Um mutante `transaction.on_commit(lambda: evento(...))` não encontra nome proibido na catraca, e a escrita de `evento` só ocorreria depois; sua aprovação pela suíte inteira é **[nao verificado]**. Correção mínima: executar `captureOnCommitCallbacks(execute=True)` dentro da captura SQL e acrescentar esse mutante (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/core/observ.py:134`).

| Classe SQL | Resultado do filtro |
|---|---|
| INSERT/UPDATE/DELETE simples, SQL bruto e minúsculas | **Reconhece**, desde que sejam o primeiro token após espaços (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:223`). |
| `bulk_create` emitindo INSERT | **Reconhece pelo SQL**; também está proibido no AST (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:41`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| DML precedido por `WITH` ou comentário | **Não reconhece. PROCEDENTE.** Correção mínima: classificar esses formatos ou rejeitar SQL não reconhecido como leitura/controle permitido (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| `SELECT … FOR UPDATE` | **Ignora.** Não altera linha por si, mas a prova também não garante ausência de travas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| SAVEPOINT/RELEASE/ROLLBACK TO | **Ignora intencionalmente**; DML comum dentro do savepoint continua reconhecível (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:62`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:224`). |
| TRUNCATE/DO/CALL | **Não reconhece** pelo padrão atual; efeitos de escrita desses formatos ficam fora da garantia (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |

**4. Asserções novas passam vazias?**

**CONFORME — zero juízes é uma asserção efetiva de cadastro.** É igualdade numérica sobre a função que percorre as entradas e filtra por papel, acompanhada das autoridades específicas. Não é um laço que deixa de executar; também não prova sozinho o comportamento dos comandos (`app/config/crons.py:2356`; `app/escala/tests/test_termometro_regua_prazo.py:12`; `app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`). A existência de exatamente uma entrada do disparar, com alarme e sem apply, recebe prova própria (`app/chamados/tests/test_selo_emissor_fio_mudo.py:194`).

**CONFORME — o total prende a tabela pertinente e um alvo real.** O ciclo cria chamado/célula dentro da janela, executa o comando e exige FIO MUDO 1/rc 2; a comparação total impede aceitar uma pergunta nova simplesmente porque nasceu morta (`app/chamados/tests/test_selo_emissor_fio_mudo.py:255`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:260`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`). UPDATE ou criação seguida de exclusão podem conservar o total; por isso essa prova complementa o RED SQL (`app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265`).

**5. Classes, restringindo a atribuição ao delta**

A delimitação Git exata do delta permanece **[nao verificado]**; não atribuo como novidade os remanescentes que o próprio anexo declarou preexistentes.

| Classe | Veredito |
|---|---|
| **Segundo juiz** | **Não identificado — CONFORME no trecho inspecionado.** O comando consulta as autoridades compartilhadas de fábrica, vitalidade, parcial, teto e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/chamados/management/commands/disparar_perguntas_competencia.py:130`). |
| **Segundo escritor fora da porta** | **Não identificado no comando atual.** Após consultas, altera coleções locais e imprime; garantia universal indireta: **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:135`; `app/chamados/management/commands/disparar_perguntas_competencia.py:164`). |
| **Relógio fora da borda** | **Sim no caminho chamado; introdução pelo delta [nao verificado].** A borda foi corrigida, mas persistem `date.today()` e `_tz.now()` internos (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/juizes.py:577`; `app/escala/servico_jornada.py:368`). |
| **Derivação paralela de fonte única** | **Não identificada — CONFORME para janela/teto.** Delega às autoridades existentes (`app/chamados/manag

[truncado em 20000 de 21207 caracteres]

## 09/11 22:42 · sessao 5c4bd2bc64d4 · - · - · origem=Ronald (tmux)

**Itens 1 e 5 — CONFORME.** As duas contagens agora exigem zero e preservam as asserções de autoridade (`app/escala/tests/test_termometro_regua_prazo.py:12`, `:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`, `:148`). Não são laços sobre universo vazio: comparam numericamente o resultado da função que percorre as entradas cadastradas e filtra pelo papel (`app/config/crons.py:1787`, `:1798`, `:2356`). Isso sela classificação; não prova sozinho comportamento de cada cron (`app/escala/tests/test_termometro_regua_prazo.py:19`; `app/config/crons.py:2356`).

A contagem total do ciclo prende a tabela pertinente, `PerguntaDisputa`, antes/depois, além de ausência de vivas, alvo contado igual a um e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:269`, `:271`, `:273`, `:274`, `:275`). O fixture cria chamado/célula, e o comando visita esse universo; portanto não depende de ausência de alvo (`app/chamados/tests/test_selo_emissor_fio_mudo.py:255`, `:260`; `app/chamados/management/commands/disparar_perguntas_competencia.py:93`). Não detecta atualização que conserve contagem, nem criação seguida de exclusão: é garantia complementar ao RED SQL (`app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265`).

**Item 4 — CONFORME quanto ao UPDATE ORM síncrono; PROCEDENTE quanto à abrangência declarada.** A catraca AST ganhou as portas previstas e métodos ORM, incluindo `bulk_create`; seus mutantes exercitam esses nomes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`, `:84`, `:87`). A captura cerca `call_command` inteiro, com/sem alarme, e exige um alvo em cada caminho (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:253`, `:258`, `:261`, `:264`, `:266`, `:267`).

O mutante usa `ChamadoColaborador.objects.filter(...).update(...)`, exige uma linha atualizada, captura com o mesmo `connection`, verifica DML presente e contagens iguais e demonstra que a asserção “DML vazio” rejeita o resultado (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`, `:279`, `:280`, `:281`, `:283`, `:284`). Prova a mordida do coletor nesse UPDATE; não injeta o mutante no `handle` (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `:261`).

**Conexão/caminho atual:** a captura importa `django.db.connection`, e os três modelos são `models.Model`; as operações verificadas usam `.objects` sem alias explícito (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:13`, `:238`, `:277`; `app/chamados/models.py:27`, `:1710`, `:2406`). CI declara uma conexão `default` (`app/config/settings/ci.py:6`). A chamada é direta e síncrona, sem despacho para thread no teste; `threading.local()` do cartório é armazenamento de contexto, não criação de thread (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:261`; `app/ponto/services/cartorio.py:32`, `:34`). O caminho atual consulta e imprime; a fábrica consultada retorna motivos, e o dedup retorna vitalidade (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:125`, `:130`, `:135`; `app/chamados/services/fabrica_da_celula.py:213`, `:216`; `app/chamados/services/disputa_emissao.py:1412`, `:1419`). Execução efetiva em outra conexão/thread: **[nao verificado]**.

**Escape por `on_commit` — PROCEDENTE.** O teste é `TestCase` e não executa callbacks pendentes dentro da captura (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:228`, `:258`, `:265`). A própria cópia explica que callbacks ficam em `connection.run_on_commit` no `TestCase` e que essa diferença já ocultou defeito (`app/ponto/tests/test_selo_chokepoint_escrita.py:197`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:17`). Há signal real que agenda escrita posterior, embora o comando atual não chame a porta de escrita que o acordaria (`app/chamados/signals.py:227`, `:233`, `:238`; `app/chamados/management/commands/disparar_perguntas_competencia.py:130`). Correção mínima: executar `captureOnCommitCallbacks(execute=True)` **dentro** de `CaptureQueriesContext` e acrescentar o mutante deferido; o escritor `evento` insere em `LogAuditoria` (`app/core/observ.py:133`, `:134`).

**Filtro SQL — PROCEDENTE parcial.** Só reconhece comandos cujo primeiro token, após espaços, seja INSERT/UPDATE/DELETE, ignorando caixa (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:61`, `:63`).

- `bulk_create` emitindo INSERT, SQL bruto simples e minúsculas: cobertos pelo padrão; minúsculas já têm caso explícito (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:223`).
- `SAVEPOINT`, `RELEASE` e `ROLLBACK TO SAVEPOINT`: ignorados deliberadamente; não são DML (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:62`, `:224`).
- `WITH ... UPDATE ... RETURNING ... SELECT`, comentário antes do UPDATE, `TRUNCATE`, `DO`/`CALL`: não reconhecidos pelo padrão; `cursor`/`execute` também não são vetados no AST (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`, `:63`).
- `SELECT ... FOR UPDATE`: ignorado; não altera linha por si, mas o RED também não sela ausência de travas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`).

Extraí e executei somente `escritas_sql` via AST em `python3 -B`, sem Django/DB: confirmou reconhecimento de UPDATE minúsculo/INSERT e rejeição ausente para CTE com UPDATE, comentário, SELECT FOR UPDATE, TRUNCATE e SAVEPOINT; fundamento reproduzível na expressão de `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`. Correção mínima: rejeitar SQL não classificado como leitura/controle permitido e testar CTE, comentários e callback deferido (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:222`, `:258`).

**Classes:** segundo escritor no comando atual, **não identificado** no caminho inspecionado; garantia universal **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:125`, `:130`, `:135`). Selo/RED sem mordida suficiente, **sim**, pelas duas lacunas concretas acima, sem negar a mordida comprovada do UPDATE síncrono (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:258`, `:276`).

## 09/11 22:41 · sessao 4056e2502796 · - · - · origem=Ronald (tmux)

Curas e limites:

- **6a CONFORME:** a docstring indica `consumir_eventos_celula --semear` e esclarece “semeia e retorna; consumo na rodada seguinte” (`app/chamados/management/commands/disparar_perguntas_competencia.py:8`). O consumidor confirma o retorno antes de consumir (`app/chamados/management/commands/consumir_eventos_celula.py:28`; `app/chamados/management/commands/consumir_eventos_celula.py:31`).
- **7 CONFORME na correção prevista, parcial no callgraph inteiro:** `_viva(p, hoje=hoje)` recebe agora o dia da borda (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`). Continua `_ja_perguntou` sem dia/instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:125`), que chama `fio_vivo` (`app/chamados/services/disputa_emissao.py:1419`), que chama `pergunta_viva(p)` sem hoje (`app/chamados/juizes.py:1434`), permitindo `date.today()` (`app/chamados/juizes.py:577`). Preexistência desses trechos foi declarada no anexo fornecido; comprovação Git independente [nao verificado].
- **Relógio adicional não mencionado na triagem:** mesmo a chamada corrigida `_viva(..., hoje=hoje)` pode alcançar `classificar_falta` (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1393`). Essa função não recebe instante (`app/escala/servico_jornada.py:270`) e relê `_tz.now()` no teto do turno (`app/escala/servico_jornada.py:368`). Também há chamadas por `_pergunta_materializada_de_fato` (`app/chamados/juizes.py:681`; `app/chamados/juizes.py:770`). Correção mínima para a invariável global: propagar `agora` a esse leitor e `hoje` ao caminho `fio_vivo`; introdução desse defeito pelo delta [nao verificado].
- **8b CONFORME:** comentário agora diz explicitamente janela GLOBAL, sem empresa, corte 21 (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`). A implementação chama `janela_atual(hoje)` sem empresa (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`); o default é efetivamente 21 (`app/ponto/janelas.py:50`; `app/ponto/janelas.py:63`). Linha de backlog na cópia [nao verificado].
- **8c CONFORME na borda:** um único `timezone.now()` produz `agora`; `hoje=timezone.localdate(agora)`; competência recebe esse mesmo hoje (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`). Garantia em todas as funções chamadas continua parcial pelos relógios acima (`app/chamados/juizes.py:577`; `app/escala/servico_jornada.py:368`).

Mapa de escritores/leitores:

| Valor | Escritor | Leitores e significado |
|---|---|---|
| `agora` | Borda do comando (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`) | `_ontem_em_janela` recebe com hoje (`app/chamados/management/commands/disparar_perguntas_competencia.py:79`); fábrica recebe ambos (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`); parcial recebe instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`); teto do fio recebe ambos (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| `hoje` | Conversão local do mesmo instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`) | Competência (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`), ontem/piso/filtro `<hoje` (`app/chamados/management/commands/disparar_perguntas_competencia.py:69`), fábrica/teto (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`), vitalidade direta (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`). |
| `hoje=` de pergunta_viva | Chamador acima; padrão `None` (`app/chamados/juizes.py:839`) | Integra chave do memo (`app/chamados/juizes.py:852`; `app/chamados/juizes.py:864`), é repassado ao cálculo (`app/chamados/juizes.py:856`), controla janela viva (`app/chamados/juizes.py:1389`) e testemunha materializada (`app/chamados/juizes.py:1406`). |
| `hoje` dentro da janela viva | Argumento ou `date.today()` independente (`app/chamados/juizes.py:577`) | Chama janela corrente e anterior (`app/chamados/juizes.py:578`; `app/chamados/juizes.py:597`), aceita inclusivamente da véspera do início anterior ao fim corrente (`app/chamados/juizes.py:603`). |

Os consumidores diretos de `agora` toleram representação UTC aware: comparam instantes e subtraem durações, sem extrair `.date()`, `.hour` ou `.time()` cru nesses caminhos (`app/ponto/services/vigia_de_hora.py:52`; `app/ponto/services/vigia_de_hora.py:58`; `app/ponto/services/cartorio.py:858`; `app/ponto/turnos.py:2116`; `app/ponto/turnos.py:2124`). A parcial encaminha o instante a `houve_fato_novo`, que usa intervalo `(desde, ate]` e compara carimbos (`app/ponto/services/cartorio.py:400`; `app/ponto/services/cartorio.py:345`; `app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:365`). Seu teste de marco novo usa `marcos_vencidos` nos dois instantes recebidos (`app/ponto/services/cartorio.py:417`). Nenhum defeito novo por representar `agora` em UTC foi encontrado nesses consumidores (`app/ponto/services/cartorio.py:858`; `app/ponto/turnos.py:2124`).

A configuração local declara São Paulo e `USE_TZ=True` (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`). O fallback `date.today()` é o dia do processo, enquanto a borda usa o fuso corrente Django; igualdade desses dias no host de produção [nao verificado] (`app/chamados/juizes.py:577`; `app/chamados/management/commands/disparar_perguntas_competencia.py:39`).

As janelas não são idênticas por desenho: fio mudo usa `[ini,fim]` (`app/chamados/management/commands/disparar_perguntas_competencia.py:93`); célula usa `[_piso,fim]` e exclui hoje, podendo estender piso até anteontem e excluir ontem ainda atendido pela vigia de hora (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`). O teto de marco inclui igualdade em `marco+30min <= agora` (`app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:858`). `_ontem_em_janela` inclui marco vencendo exatamente à meia-noite (`app/ponto/services/vigia_de_hora.py:53`). A linha `janela` ainda imprime somente a solicitada; não explicita o piso/exclusões da célula (`app/chamados/management/commands/disparar_perguntas_competencia.py:153`). Correção mínima de apresentação: imprimir também o universo efetivo da célula (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`).

O teste novo de relógio prova encaminhamento da borda e `hoje=` por fragmentos com mocks, incluindo o cenário UTC/local em dias diferentes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:125`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:148`). Não executa a função real `pergunta_viva` nessa prova: avalia sua chamada com `Mock`; portanto não detecta as releituras internas acima (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:145`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:147`).

Derivação paralela de janela/teto: **não identificada no trecho inspecionado**; o comando usa as autoridades existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/services/fabrica_da_celula.py:26`; `app/ponto/services/cartorio.py:843`). Sem Git, restrição dessa conclusão estritamente aos hunks do delta [nao verificado].

## 09/11 22:41 · sessao df6029862e30 · - · - · origem=Ronald (tmux)

**8a — CONFORME à cura local prevista.** A legenda declara pergunta **OU Pauta DP** e a barreira FIO DECIDIDO declara somente existência da linha, sem validação humana; as duas mudanças estão presas por asserções literais (`app/chamados/management/commands/disparar_perguntas_competencia.py:140`, `app/chamados/management/commands/disparar_perguntas_competencia.py:157`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:202`). O domínio consultado realmente não exige validação humana: usa colaborador, motivo, data, instância, `conversa=False` e exclusão das vias fora do fato (`app/chamados/models.py:2753`).

**8d — CONFORME.** A linha contém o literal determinado, incluindo “dia ACUSADO pela lei” e “este lote nao as escreve”; o teste exige a linha inteira (`app/chamados/management/commands/disparar_perguntas_competencia.py:136`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`).

**Quem escreve / quem lê as saídas:**

| Valor | Escritor e significado | Leitores encontrados |
|---|---|---|
| `VIGIA FIO MUDO` | `len(_mudos)`, uma tupla elegível colaborador/dia/motivo por chamado visitado, com dedup global da chave; não conta todas as perguntas potencialmente faltantes de cada chamado (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`, `app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `app/chamados/management/commands/disparar_perguntas_competencia.py:136`). | Testes do selo: `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:127`, `:156`, `:173`, `:210`, `:274`, `:284`; colab: `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; célula: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136`, `:155`, `:164`; teto: `app/chamados/tests/test_pergunta_do_dia_em_curso.py:432`, `:437`; RED: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`, `:266`. Parser operacional específico em `app/` ou `bin/`: **[nao verificado]**. |
| `FIO DECIDIDO` | `_bar['fato_ja_existe']`, incrementado após `linhas_dos_fatos(...).exists()` (`app/chamados/management/commands/disparar_perguntas_competencia.py:130`, `:140`). | Selo com linha materializada e validada: `app/chamados/tests/test_selo_emissor_fio_mudo.py:149`, `:155`; relatório sintético com contador zero: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:184`, `:202`. Parser operacional específico: **[nao verificado]**. |
| `LEGENDA` | Explica unidades mistas e significado da barreira (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`). | Asserção literal do relatório: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`. Parser operacional específico: **[nao verificado]**. |
| `FABRICA-POR-EVENTO` | `len(deixou)` conta motivos faltantes; no limbo exportado a falta é de Pauta DP (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:154`; `app/chamados/services/fabrica_da_celula.py:205`, `:214`). | Parser operacional raspa somente o inteiro em `bin/placar_code.sh:388`; exige log modificado desde meia-noite do host em `:389`; reemite o campo `perguntas_que_o_julgamento_nao_fez` em `:390`. Regex de testes: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74`, `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`. Asserções: `app/chamados/tests/test_fabrica_por_evento.py:83`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127`, `:133`, `:161`, `:392`, `:396`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:196`, `:267`. Contrato do sed e do arquivo: `bin/tests/test_placar_conta_o_retido_do_lastro.sh:500`, `:501`, `:507`. |

**PROCEDENTE — a semântica não se propaga completamente ao leitor operacional.** A legenda esclarece o log, mas o placar descarta essa legenda e conserva campo nomeado “perguntas” para quantidade que inclui motivos de Pauta DP (`app/chamados/services/fabrica_da_celula.py:211`, `bin/placar_code.sh:388`, `bin/placar_code.sh:390`). A linha complementar `VIGIA` também continua chamando tudo de pergunta (`app/chamados/management/commands/disparar_perguntas_competencia.py:160`). Correção mínima: conservar o prefixo consumido e esclarecer no rótulo operacional e nessa linha que a unidade é “motivos pendentes: pergunta ou pauta”. Atribuição deste resíduo como novidade do delta: **[nao verificado]**; a triagem já reconhecia a unidade mista.

**Fluxo do log — CONFORME no código.** Cadastro associa o comando a `fio_mudo.log`; o encadeado transporta o caminho e redireciona stdout e stderr; o teste prende log, `--alarme` e ausência de `--apply` (`app/config/crons.py:1521`, `bin/encadeado.sh:58`, `bin/encadeado.sh:64`, `bin/encadeado.sh:102`, `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:25`). O vigia de falhas do runner interpreta **rc**, não estas linhas; 0/2 recebem carimbo e não abrem falha de execução (`bin/cron_run.sh:22`, `bin/cron_run.sh:32`). Existência/frescor desse log em produção: **[nao verificado]**.

**Docs operacionais continuam descrevendo o contador como perguntas**, inclusive o crescimento histórico, sem a unidade Pauta DP (`app/docs/RELATO.md:27`, `app/docs/BACKLOG.md:1872`). O registro da condição de pouso continua exigindo zero do emissor com C11 no ar; não é prova de medição realizada (`app/docs/RELATO.md:26`, `app/docs/BACKLOG.md:1871`).

**Classes no trecho inspecionado:**

- **Segundo juiz — não identificado:** primeiro universo delega integralmente à regra compartilhada; segundo usa ata/teto, parcial, vitalidade e domínio do fato existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:100`, `:106`, `:110`, `:125`, `:130`; `app/chamados/services/fabrica_da_celula.py:209`).
- **Segundo escritor — não identificado no comando:** coleções locais e saída após consultas; emissor real segue em função distinta, que chama os escritores existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `:164`; `app/chamados/services/fabrica_da_celula.py:133`, `:145`). Garantia universal indireta cabe à inspeção RED/árvore de chamadas: **[nao verificado]** por este explorer.
- **ESMERIL-SEMANTICA — cura local conforme; resíduo operacional procedente**, conforme campo “perguntas” descrito acima (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`, `bin/placar_code.sh:390`).

**Itens 2/3 da sessão permanecem visíveis, sem confundir com curas volta 2:** exceção de disparar continua em `bin/tests/test_papel_prazo_nao_deriva.sh:303`, e o teste a reprova fora da tupla em `:325`; placar ainda trata ausência de membros impressos como falha em `bin/placar_code.sh:209`. A triagem explicitamente os destinou à sessão, não ao construtor volta 2 [dado fornecido; sem arquivo local].

**Linhas de fila prometidas:** inscrição de `E4A2-RELOGIO-FIO-VIVO` e `E4A2-JANELA-POR-EMPRESA` no BACKLOG desta cópia **[nao verificado]**; o registro geral da obra está em `app/docs/BACKLOG.md:1871`, e o comentário somente anuncia a fila de janela por empresa em `app/chamados/management/commands/disparar_perguntas_competencia.py:29`.

## 09/11 22:24 · sessao b40cd15d984c · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alterados cinco arquivos, nos itens da volta 2:

- [disparar_perguntas_competencia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/management/commands/disparar_perguntas_competencia.py:36): único `agora/hoje`, janela usando esse dia e `_viva(..., hoje=hoje)`. Reparo documentado em `:8`, janela global em `:28`, texto literal em `:136` e legenda em `:157`. A linha FABRICA permaneceu idêntica em bytes (`:154`).
- [test_e4a2_disparar_e_vigia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/tests/test_e4a2_disparar_e_vigia.py:39): AST ampliado; captura DML da chamada inteira com dois alvos distintos, com/sem alarme (`:242`); mesmo coletor aplicado ao mutante ORM `UPDATE` (`:270`). [nao verificado]
- [test_selo_emissor_fio_mudo.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/tests/test_selo_emissor_fio_mudo.py:266): mantém “nenhuma viva” e exige contagem total antes/depois igual.
- [test_termometro_regua_prazo.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/escala/tests/test_termometro_regua_prazo.py:19) e [test_selfie_ausente_juiz.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/ponto/tests/test_selfie_ausente_juiz.py:148): exigem zero juízes, com o histórico solicitado.

RED antes: o teste do relatório falhou exigindo a linha literal do brief: `FAILED (failures=1)`. Depois, os nove corpos de testes executados por AST/unittest passaram: `Ran 9 tests ... OK`. Essa execução inclui os mutantes de escrita/opções (`test_e4a2_disparar_e_vigia.py:74`), relógio (`:153`) e relatório/alarme (`:211`). [nao verificado]

Comando usado para esses testes e seus mutantes, sem Django: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4a2_disparar_e_vigia.py')
a = ast.parse(p.read_text())
a.body = [
    n for n in a.body
    if not (
        isinstance(n, ast.ImportFrom)
        and (n.module.startswith('django') or n.module == 'freezegun')
    )
    and not (
        isinstance(n, ast.ClassDef)
        and any(isinstance(b, ast.Name) and b.id == 'TestCase' for b in n.bases)
    )
]
ns = {'__file__': str(p.resolve()), 'SimpleTestCase': unittest.TestCase}
exec(compile(a, str(p), 'exec'), ns)
suite = unittest.TestSuite( [nao verificado]
    unittest.defaultTestLoader.loadTestsFromTestCase(c)
    for c in ns.values()
    if isinstance(c, type)
    and c is not unittest.TestCase
    and issubclass(c, unittest.TestCase)
)
r = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(not r.wasSuccessful())
PY
```

Os testes reais pelo Django, a captura ORM, a mordida do `UPDATE` e a suíte completa ficaram **[nao verificado]**. A tentativa dos `SimpleTestCase` parou em `No module named 'django'`. O ruff também ficou **[nao verificado]**: a tentativa retornou `No module named ruff`. [nao verificado]

Comandos pendentes para o Code, incluindo o mutante ORM:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4a2_disparar_e_vigia \
  chamados.tests.test_selo_emissor_fio_mudo \
  escala.tests.test_termometro_regua_prazo \
  ponto.tests.test_selfie_ausente_juiz

PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache \
  chamados/management/commands/disparar_perguntas_competencia.py \
  chamados/tests/test_e4a2_disparar_e_vigia.py \
  chamados/tests/test_selo_emissor_fio_mudo.py \
  escala/tests/test_termometro_regua_prazo.py \
  ponto/tests/test_selfie_ausente_juiz.py
```

## 09/11 22:10 · sessao 636b02405ee3 · FIO-E0-V4-E4A-2 · verificador · origem=Code

[nao verificado] — identidade Git, ancestralidade, diff integral e relação exata dos arquivos tocados: a cópia fornecida não contém `.git`. Execução da suíte e condição de pouso em produção: [nao verificado]. [nao verificado]

1. **QUEM ESCREVE / QUEM LÊ — CONFORME para rc e dependência; PROCEDENTE nas consequências da classificação.**

| Valor | Definição/produtor | Leitores e significado |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Tupla vazia em `app/config/crons.py:2032` | Alimenta papéis em `app/config/crons.py:2164`; exceção estrutural em `app/core/contratos_estruturais.py:264`; igualdade no tabuleiro em `app/core/tests/test_contract_tabuleiro.py:26`; selo de host em `bin/tests/test_papel_prazo_nao_deriva.sh:325` e `bin/tests/test_papel_prazo_nao_deriva.sh:328`. |
| `PAPEL_DO_CRON` | Disparar declarado `vigia` em `app/config/crons.py:2168` | Consulta por `papel()` em `app/config/crons.py:2344`; juízes derivados em `app/config/crons.py:2356`; tabuleiro em `app/core/management/commands/gerar_diagrama.py:242`; subgrafo VIGIAS em `app/core/management/commands/gerar_diagrama.py:397`; selo de prazo em `bin/tests/test_papel_prazo_nao_deriva.sh:247`. |
| Juízes derivados | Compreensão sobre todas as entradas em `app/config/crons.py:2356` | Medição padrão em `app/chamados/services/vigia_loop.py:82`; placar em `bin/placar_code.sh:484`, incluído no TOTAL em `bin/placar_code.sh:555`; desenho em `app/core/management/commands/gerar_diagrama.py:244`. |
| Argv ENC | `--comp-corrente --alarme` em `app/config/crons.py:1521` | Transportado em `app/config/crons.py:1850`; serializado em `app/core/management/commands/crons.py:40`; executado em `bin/encadeado.sh:64` e `bin/encadeado.sh:102`; selado em `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26` e `app/chamados/tests/test_selo_emissor_fio_mudo.py:193`. |
| Rc | Com alarme, qualquer universo pendente produz 2; sem alarme termina normalmente em `app/chamados/management/commands/disparar_perguntas_competencia.py:160` | Runner captura/grava em `bin/cron_run.sh:11` e `bin/cron_run.sh:13`; aceita 0/2 para carimbo em `bin/cron_run.sh:22` e recuperação em `bin/cron_run.sh:32`; corrente acumula rc em `bin/encadeado.sh:65`; saúde aceita 0/2 em `app/core/services/crons_quebrados.py:46`; pipeline distingue alarme em `app/core/management/commands/pipeline_placar.py:75`. |
| `FABRICA-POR-EVENTO` | Quantidade de motivos pendentes e pares colaborador/dia em `app/chamados/management/commands/disparar_perguntas_competencia.py:153` | Parser operacional em `bin/placar_code.sh:388`; parsers de testes em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74` e `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; asserções em `app/chamados/tests/test_fabrica_por_evento.py:83`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:115`. |
| `VIGIA FIO MUDO` | `len(_mudos)` em `app/chamados/management/commands/disparar_perguntas_competencia.py:135` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:173`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:437`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:114`. Leitor operacional específico: [nao verificado]. |
| `ALVOS` e distribuição | Mesmo conjunto `_mudos`, sem limite, em `app/chamados/management/commands/disparar_perguntas_competencia.py:147` e `app/chamados/management/commands/disparar_perguntas_competencia.py:157` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:181`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `app/chamados/tests/test_fabrica_pela_ata.py:51`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:116`. Leitor operacional específico: [nao verificado]. |
| `[BARRADO]` | Quatro contadores em `app/chamados/management/commands/disparar_perguntas_competencia.py:138` | A barreira parcial é conferida em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:174`; o teste de relatório injeta barreiras zeradas em `app/chamados/tests/test_e4a2_disparar_e_vigia.py:105`. Parser operacional da linha: [nao verificado]. |

**CONFORME — rc 2 libera o dependente.** O runner cria o arquivo de fim também em rc 2; a corrente espera sua existência. `reconciliar_perguntas_orfas` depende desse gatilho e mede seu próprio universo, sem interpretar os contadores do disparar. “Fim” significa conclusão, não ausência de pendências (`bin/cron_run.sh:19`; `bin/cron_run.sh:22`; `bin/encadeado.sh:89`; `app/config/crons.py:1526`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29`).

**PROCEDENTE — não há uniformidade completa.** A classificação vazia produz incompatibilidades nos testes e falso diagnóstico no placar, detalhados abaixo. O pipeline também continua calculando esperados somente de `CRONS`, deixando ENCs fora desse universo (`app/escala/tests/test_termometro_regua_prazo.py:19`; `bin/placar_code.sh:209`; `app/core/management/commands/pipeline_placar.py:40`; `app/core/management/commands/pipeline_placar.py:108`).

2. **TUPLA VAZIA / VEREDITOS — PROCEDENTE.**

- **Duas asserções ainda exigem exatamente 1 juiz.** Correção mínima: exigir zero, mantendo as verificações das respectivas autoridades (`app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`).
- **O selo de host rejeita a exceção remanescente.** A exceção do disparar permanece, mas sua ausência na lista acrescenta falha. Apagá-la isoladamente também quebra o caso que exige sua isenção. Correção mínima: depois de comprovada a condição de saída, retirar a exceção e adaptar sua prova específica, preservando o caso que acusa outro cron (`bin/tests/test_papel_prazo_nao_deriva.sh:303`; `bin/tests/test_papel_prazo_nao_deriva.sh:325`; `bin/tests/test_papel_prazo_nao_deriva.sh:388`; `bin/tests/test_papel_prazo_nao_deriva.sh:390`).
- **Vazio saudável vira “vigia falhou”.** `escritas_por_cron()` devolve `universo=0, crons={}`; o shell imprime somente membros e interpreta ausência de linhas como falha. Correção mínima: emitir representação explícita do universo vazio, distinguindo-o de erro de execução (`app/chamados/services/vigia_loop.py:93`; `app/chamados/services/vigia_loop.py:98`; `bin/placar_code.sh:207`; `bin/placar_code.sh:209`).
- **Disparar desaparece da medição padrão de escritas.** Isso decorre da seleção pelos juízes vivos; não comprova ausência de escrita sem trilha. Correção mínima: preservar uma medição explícita do comando durante a verificação da retirada (`app/chamados/services/vigia_loop.py:64`; `app/chamados/services/vigia_loop.py:80`).

**CONFORME — a célula chamado × “um juiz por pergunta” não muda de cor.** `C()` tem `verde=False`; essa célula não o sobrescreve; `verdes()` conta flags, não listas vazias. O aval contrário à reclassificação para produzir verde permanece, assim como a frase 19/20 do R6 (`app/core/contratos_estruturais.py:62`; `app/core/contratos_estruturais.py:262`; `app/core/contratos_estruturais.py:270`; `app/core/contratos_estruturais.py:1055`; `app/core/placar_estrutural.py:440`).

**CONFORME — os leitores estruturais principais toleram vazio.** A derivação usa `sorted` e compreensão; o desenho usa `len` e laço; o selo compara listas e verifica exceções, sem indexar um primeiro juiz (`app/config/crons.py:2356`; `app/core/management/commands/gerar_diagrama.py:256`; `app/core/management/commands/gerar_diagrama.py:264`; `app/core/tests/test_contract_tabuleiro.py:26`; `app/core/tests/test_selo_contratos_estruturais.py:81`).

Os demais testes de retirada conferem ausência de nomes específicos e continuam compatíveis com vazio: `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:243`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:200`; `app/chamados/tests/test_varredura_isentos_evento.py:98`; `app/chamados/tests/test_varredura_feriado_evento.py:106`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:97`; `app/ponto/tests/test_varredura_lote_prazo.py:38`; `app/ponto/tests/test_varredura_fantasmas_evento.py:61`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:277`; `app/ponto/tests/test_vigia_de_hora.py:234`; `app/ponto/tests/test_vigia_intervalo_ausente.py:27`; `app/ponto/tests/test_par_relampago_pelo_turno.py:80`; `app/ponto/tests/test_orfas_vigia.py:115`; `app/ponto/tests/test_varredura_cluster_evento.py:90`; `app/ponto/tests/test_vigia_reconciliar_vinculo.py:26`; `app/ponto/tests/test_c2_vinculo_vira_lista.py:114`; `app/ponto/tests/test_varredura_classe_b_ausencia.py:35`. O teste da medição padrão exige igualdade com a lista viva, inclusive vazia (`app/chamados/tests/test_escritas_por_cron.py:65`).

3. **ESCRITA REMANESCENTE / CHAMADORES — CONFORME no caminho de domínio inspecionado; garantia universal [nao verificado].**

O comando conserva consultas e impressão; termina após o alarme. O parser oferece somente `ini`, `fim`, `comp-corrente`, `alarme` e `colab`; o ENC operacional já passa `--comp-corrente --alarme` (`app/chamados/management/commands/disparar_perguntas_competencia.py:21`; `app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:92`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `app/config/crons.py:1521`).

Há efeitos indiretos de **observabilidade**: `_ata_do_dia` pode chamar `acender_sem_ata` ou `registrar_engolido`; estes fazem logging e, no segundo caso, incrementam contador em memória. A escrita ORM de auditoria pertence a outra função, `evento()` (`app/chamados/services/disputa_emissao.py:895`; `app/chamados/services/disputa_emissao.py:908`; `app/chamados/services/declaracao_texto.py:63`; `app/core/observ.py:73`; `app/core/observ.py:94`; `app/core/observ.py:134`).

Outro chamador executável passando as opções removidas, escrita de domínio por importação ou sinal alcançado nessa execução: **[nao verificado]**. As opções antigas presentes na base incorporada são texto histórico; há também comentário hipotético antigo, não chamada executável (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:165`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:218`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:5`).

4. **RED NOVO — PROCEDENTE: mordida parcial.**

O AST proíbe somente `perguntar`, `_rodar`, `atomic`, `RuntimeError` e as três opções. Os mutantes de escrita testados repetem essa lista (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:55`).

Ele aceita nomes que efetivamente escrevem neste domínio: `criar_ou_atualizar_disputa`, `gerar_perguntas_disputa`, `PerguntaDisputa.abrir`, `.save()`, `.create()`, `.update()` e `reabrir_sistema`. As portas reais geram perguntas, inserem linhas e salvam reabertura (`app/chamados/services/disputa_emissao.py:662`; `app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3019`; `app/chamados/models.py:2177`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`).

Um mutante concreto é inserir, antes de `_mudos.append`, `ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')`. O AST não acusa; o teste de banco compara apenas contagens; o teste de relatório executa somente o sufixo iniciado no primeiro `stdout.write`. Assim, essas verificações não observam tal alteração de estado (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:24`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:94`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:140`). Aprovação desse mutante pela suíte inteira: **[nao verificado]**.

**Correção mínima:** interceptar SQL durante a chamada completa e rejeitar DML, mantendo as contagens; ampliar a catraca para as portas reais identificadas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:146`; `app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3019`).

**CONFORME — parser e alarme têm mordida específica.** Há rejeição das três opções, combinações dos dois universos com/sem alarme e mutações de condição, rc e prefixo (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:76`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:98`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:123`).

5. **INVERSÃO DOS TESTES — CONFORME em vários contratos; PROCEDENTE numa garantia enfraquecida.** [nao verificado]

Relação exata dos arquivos tocados: **[nao verificado]**. Os contratos atuais relacionados são:

| Teste | Garantia atual | [nao verificado]
|---|---|
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:78` | Célula: zero perguntas, contador exato e rc 2. FIO MUDO: zero e rc 2 em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132`; retratada conserva uma linha em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:156`; parcial mantém zero e rc 0 em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:172`. |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py:184` | Exatamente um ENC, alarme e ausência de apply. Sem chave conserva contagem em `app/chamados/tests/test_selo_emissor_fio_mudo.py:205`. |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py:435` | Marco vencido contado e nenhuma pergunta criada; o helper não verifica rc explicitamente em `app/chamados/tests/test_pergunta_do_dia_em_curso.py:422`. |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19` | Preserva formato de seis posições, log e argv; exige alarme e rejeita apply. |
| `app/ponto/tests/test_varredura_lote_prazo.py:62` | Exige exatamente zero juízes e papel vigia. |
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:34` | Conserva contagem de perguntas; confere alvos e distribuição em `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`. |
| `app/chamados/tests/test_fabrica_por_evento.py:76` | Conserva contagem e exige contador da fábrica. |
| `app/chamados/tests/test_fabrica_pela_ata.py:33` | Conserva contagem antes de devolver o relatório. |
| `app/chamados/tests/test_e4a2_disparar_e_vigia.py:146` | Conserva quatro contagens e verifica rc com/sem alarme; limitação quanto a UPDATE descrita no item 4. |

**PROCEDENTE — “nenhuma viva” não equivale a “nenhuma criada”.** No ciclo acusador, a nova asserção aceita uma pergunta criada já morta. Correção mínima: acrescentar comparação de contagem total antes/depois, além de ausência de vivas e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:270`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:272`).

6. **LEGIBILIDADE SITUACIONAL — PROCEDENTE.**

**CONFORME parcialmente:** o docstring identifica vigia, dois universos, rc e emissor por evento; o comentário aponta a fábrica. Isso atende parte da fronteira “quem manda aqui” (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`; `app/chamados/management/commands/disparar_perguntas_competencia.py:13`; `CLAUDE.md:41`; `CLAUDE.md:168`).

**PROCEDENTE — falta a ponte de reparo do estoque.** O comentário transforma estoque pendente em diagnóstico, mas não aponta a porta existente. Correção mínima: indicar `consumir_eventos_celula --semear` e esclarecer que a semeadura retorna antes do consumo, exigindo consumo posterior (`app/chamados/management/commands/disparar_perguntas_competencia.py:83`; `app/chamados/management/commands/consumir_eventos_celula.py:14`; `app/chamados/management/commands/consumir_eventos_celula.py:28`).

**PROCEDENTE — comentários contraditórios.** O cadastro anuncia a saída para vigia e, logo depois, afirma que o FIO MUDO segue escrevendo; a docstring do contador ainda apresenta disparar como emissor. Correção mínima: marcar explicitamente esses trechos como situação anterior ou atualizar sua descrição (`app/config/crons.py:2044`; `app/config/crons.py:2046`; `app/config/crons.py:2353`).

A condição legal de saída continua preservada no corte e no selo; não foi apagada desses lugares. Perda de outra história normativa sem cópia equivalente: **[nao verificado]** (`app/docs/CORTES.md:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:307`).

7. **CLASSES QUE JÁ REPROVARAM OBRA.**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando registrado | **CONFORME no trecho inspecionado:** consulta regra compartilhada da fábrica, `parcial_so_lista`, pergunta viva e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:80`; `app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `app/chamados/management/commands/disparar_perguntas_competencia.py:105`; `app/chamados/management/commands/disparar_perguntas_competencia.py:129`). |
| Segundo escritor fora da porta | **CONFORME no caminho de domínio inspecionado**, com limite probatório do RED apontado no item 4 (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`). |
| Relógio fora da borda | **PROCEDENTE:** `_viva(p)` não recebe `_hoje`; pode alcançar `date.today()`. `_ja_perguntou` também chega a `fio_vivo`, que não injeta hoje. Correção mínima: propagar o dia capturado na borda pelos leitores (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `app/chamados/management/commands/disparar_perguntas_competencia.py:124`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:577`; `app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`). |
| Derivação paralela de fonte única | **CONFORME no teto e janela:** chama funções existentes; há problemas de parâmetros/semântica abaixo (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:109`). |
| Selo/RED sem mordida suficiente | **PROCEDENTE**, para escritores alternativos e UPDATE (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:143`). |
| Ordem de trava | **[nao verificado]** quanto à execução e concorrência. O comando inspecionado faz seleção e relatório, sem novo bloco transacional (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:134`). |
| Pouso antes do pré-requisito | **[nao verificado]** quanto ao pouso efetivo. A condição exige medição do FIO MUDO e C11 pousado; o registro exige medição com C11 no ar. A chamada ao escritor no consumidor prova código local, não implantação. Correção mínima: apresentar essa evidência antes de liberar pouso (`app/docs/CORTES.md:305`; `app/docs/RELATO.md:26`; `app/chamados/reconciliador.py:1387`). |
| Campo com dois significados | **PROCEDENTE**, nos casos seguintes (`app/chamados/management/commands/disparar_perguntas_competencia.py:139`; `app/chamados/management/commands/disparar_perguntas_competencia.py:153`; `app/chamados/services/fabrica_da_celula.py:211`). |

**PROCEDENTE — unidade:** `FABRICA-POR-EVENTO` diz “perguntas”, mas pode contar falta de Pauta DP; `FIO DECIDIDO` decorre de existência de linha do fato, sem exigir validação humana. Correção mínima: explicar as unidades e ajustar a legenda da barreira, preservando o prefixo consumido pelo placar (`app/chamados/services/fabrica_da_celula.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:129`; `app/chamados/management/commands/disparar_perguntas_competencia.py:139`; `app/chamados/models.py:2753`; `bin/placar_code.sh:388`).

**PROCEDENTE — corte por empresa:** o comentário promete corte por empresa, mas `janela_atual` é chamada sem empresa; nesse caso o corte é 21. Correção mínima: tornar o universo explicitamente global ou calcular a janela por empresa usando a mesma autoridade (`app/chamados/management/commands/disparar_perguntas_competen

[truncado em 20000 de 21489 caracteres]

## 09/11 22:08 · sessao d88a5eaae8fc · - · - · origem=Ronald (tmux)

3. **CONFORME quanto à retirada da escrita de domínio:** o comando termina após consultas, contadores e eventual `SystemExit(2)`; as opções restantes são `ini/fim/comp-corrente/alarme/colab` (`app/chamados/management/commands/disparar_perguntas_competencia.py:21-32,70-163`). Os helpers examinados consultam a ata, o juiz e o domínio do fato; o escritor continua separado em `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:15-54,67-130,151-190,205-236`).

- A chamada indireta `_ata_do_dia` pode produzir observabilidade: `acender_sem_ata` só chama `logging.warning`; `registrar_engolido` modifica contador em memória e chama `logging.error`. Nenhuma delas chama o escritor de `LogAuditoria`, que é outra função (`app/chamados/services/disputa_emissao.py:884-910`; `app/chamados/services/declaracao_texto.py:63-72`; `app/core/observ.py:73-82,94-138`).
- Os signals inspecionados são registrados no `ready` e respondem a `pre_save/post_save`; o comando não executa essas operações. Ausência universal de efeitos em toda dependência de inicialização Django: **[nao verificado]** (`app/chamados/apps.py:8-10`; `app/chamados/signals.py:178-188,227-250`).
- O cron real passa somente `--comp-corrente --alarme` (`app/config/crons.py:1521`). As chamadas inspecionadas não passam opções removidas (`app/chamados/tests/test_selo_emissor_fio_mudo.py:58-65,260-261,292-293`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:61-66`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:34-41`; `app/chamados/tests/test_fabrica_por_evento.py:76-84`; `app/chamados/tests/test_fabrica_pela_ata.py:34-41`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198-199`; `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:84-90`).
- Strings antigas restantes são história/base de teste, não chamadores atuais: BASE embutida conserva as três opções; TICKETS relata atos passados (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:165-166,218-223`; `app/docs/TICKETS.md:1116,1120`). O `--apply` de `test_cobparc5...:294` é encaminhado a outro comando, `emitir_furo_retroativo` (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:278-294`).

6. **PROCEDENTE — documentação situacional incompleta/contraditória.**

- O novo docstring identifica vigia, finalidade e escritor por evento; os comentários apontam a fábrica e preservam as guardas relevantes (`app/chamados/management/commands/disparar_perguntas_competencia.py:1-15,54-67,83-85,102-109,127-129`).
- Falta uma ponte para o reparo do estoque no texto do comando; a porta existente é `consumir_eventos_celula --semear`, que apenas semeia e retorna — consumir exige execução posterior normal (`app/chamados/management/commands/disparar_perguntas_competencia.py:1-19,83-85`; `app/chamados/management/commands/consumir_eventos_celula.py:14-15,25-31`). Correção mínima: citar essa sequência no docstring.
- O comentário novo anuncia VIGIA, mas a continuação ainda afirma “segue ESCREVENDO — não há evento”; a função de contagem também afirma que disparar emite pergunta (`app/config/crons.py:2044-2047,2348-2354`). Correção mínima: atualizar as frases ao estado novo, conservando a história como passado.
- A história de teto por instante e emissão por evento continua na fábrica; a exceção/condição de saída continua no corte. Não demonstrei lei viva apagada sem outra localização (`app/chamados/services/fabrica_da_celula.py:1-11`; `app/docs/CORTES.md:305`; BASE histórica: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:174-190`).

7. Classes:

- **Segundo juiz registrado: CONFORME no trecho examinado.** Seleção chama as autoridades existentes de motivos, vivo, parcial e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:15,99,105,109,124,129`).
- **Segundo escritor: CONFORME estaticamente**, com limite de inicialização acima (`app/chamados/management/commands/disparar_perguntas_competencia.py:70-163`; `app/chamados/services/fabrica_da_celula.py:133-147,177-190`).
- **Relógio fora da borda: PROCEDENTE, preexistente.** O comando chama `_viva(p)` sem `_hoje`; o juiz pode alcançar `_dentro_da_janela_viva`, cujo fallback é `date.today()`. `_ja_perguntou` também chega a `fio_vivo` sem transportar o dia (`app/chamados/management/commands/disparar_perguntas_competencia.py:99,124`; `app/chamados/juizes.py:1388-1389,569-578,1425-1434`; `app/chamados/services/disputa_emissao.py:1412-1419`). Correção mínima: propagar o dia capturado por ambas as cadeias. Há ainda duas leituras independentes de relógio na própria borda, antes/depois dos imports (`app/chamados/management/commands/disparar_perguntas_competencia.py:38,49-51`).
- **Derivação paralela de fonte única: CONFORME quanto a teto/motivos**, delegados à fábrica e ao cartório (`app/chamados/management/commands/disparar_perguntas_competencia.py:15,80,109`; `app/chamados/services/fabrica_da_celula.py:15-29`; `app/ponto/services/cartorio.py:831-859`).
- **Ordem de trava: CONFORME no comando**, que não abre transação/trava no corpo atual; efeitos concorrentes concretos: **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:34-163`).
- **Pouso antes do pré-requisito: [nao verificado], não liberado.** O corte exige medição e C11 pousado; o RELATO concretiza “`--apply` achar 0 com C11 no ar”. O dado fornecido não prova essa rodada (`app/docs/CORTES.md:305`; `app/docs/RELATO.md:26`). A implementação do consumidor/C11 na cópia não comprova implantação (`app/chamados/reconciliador.py:1384-1387`).
- **Semântica de janela: PROCEDENTE, preexistente.** `--comp-corrente` chama `janela_atual` sem empresa, portanto usa corte default 21 e aplica uma janela conjunta, embora o comentário diga incluir corte por empresa (`app/chamados/management/commands/disparar_perguntas_competencia.py:24-27,38,70,92`; `app/ponto/janelas.py:36-51,60-63`). Correção mínima: explicitar universo de corte padrão ou calcular janela por empresa conforme contrato autorizado.
- **Inclusividade e dia do turno: CONFORME no recorte explícito.** INI/FIM são inclusivos; o primeiro universo estende o piso e exclui hoje/ontem ainda coberto pelo vigia; o segundo usa a data da célula; vencimento é por instante e usa o turno encadeado (`app/chamados/management/commands/disparar_perguntas_competencia.py:68-80,92,101,109`; `app/ponto/services/cartorio.py:831-859`). A linha “janela” imprime a solicitada, sem revelar o piso ampliado do primeiro universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:69-70,152`).
- **Unidade do contador: PROCEDENTE, preexistente.** `FABRICA-POR-EVENTO` diz “perguntas”, mas o helper também retorna motivos de Pauta DP ausente no limbo exportado (`app/chamados/management/commands/disparar_perguntas_competencia.py:153-156`; `app/chamados/services/fabrica_da_celula.py:205-216`). `[BARRADO] FIO DECIDIDO` conta existência de linha do fato, sem provar decisão humana (`app/chamados/management/commands/disparar_perguntas_competencia.py:127-140`). Correção mínima: corrigir as legendas preservando o prefixo consumido pelo placar (`bin/placar_code.sh:388-390`).

## 09/11 22:07 · sessao e643b06f0f26 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE — RED tem mordida parcial.** O verificador proíbe somente `perguntar`, `_rodar`, `atomic`, `RuntimeError` e três opções; não proíbe outros escritores (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:24-44`). As mutações testadas repetem precisamente essa lista (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:55-74`).

   Os nomes ignorados alcançam escrita real: `criar_ou_atualizar_disputa` agrega/cria e gera perguntas (`app/chamados/services/disputa_emissao.py:662-682`); `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444-452`); a porta insere por `.save(force_insert=True)` (`app/chamados/models.py:3013-3019`); `reabrir_sistema` salva campos (`app/chamados/models.py:2163-2177`).

   Experimento em memória: extrair `violacoes` por AST e fornecer mutantes com esses nomes, `.save()`, `.create()`, `.reabrir_sistema()` ou `.update()` retornou conjunto vazio. Reprodução independente desse resultado na suíte completa: **[nao verificado]**. [nao verificado]

   Mutante concreto: inserir imediatamente antes de `_mudos.append(...)` uma chamada `ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')`; o verificador aceita, e a seleção oferece `ch` nesse ponto (`app/chamados/management/commands/disparar_perguntas_competencia.py:96-133`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:24-44`). O novo teste de banco compara somente quantidades de quatro tabelas, deixando alteração de estado sem mudança de cardinalidade invisível (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:140-162`). O teste de relatório executa apenas o sufixo iniciado no primeiro `stdout.write`, portanto também ignora essa inserção anterior (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:91-109`).

   **Correção mínima:** durante a chamada completa, interceptar SQL e reprovar qualquer DML; ampliar a catraca AST para os escritores canônicos efetivos. Contagens iguais devem continuar como garantia complementar (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:140-162`).

   **CONFORME — alarme e parser possuem mordida explícita.** Há combinações célula/FIO MUDO com e sem chave, rc esperado, mensagem e seis mutantes de relatório/alarme; há rejeição das três opções removidas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:76-85,98-130`). O teste contra `FONTE_BASE` usa fonte incorporado; autenticidade dessa reprodução perante o commit original: **[nao verificado]** (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:51-53,165`).

5. **CONFORME, quanto aos contratos visíveis — não viraram apenas texto.** Lista exata dos arquivos tocados pelo commit: **[nao verificado]**; os contratos atuais relacionados são: [nao verificado]

| Arquivo | Força atual |
|---|---|
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py` | Célula: zero perguntas, contador exato e rc 2 (`:78-83`). FIO MUDO concorde: zero e rc 2 (`:132-139`). Retratada: conserva exatamente uma linha e rc 2 (`:141-157`). Limbo/parcial: zero e rc apropriado (`:159-176`). |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py` | Exatamente um ENC, com alarme e sem apply (`:184-198`). Sem chave: contagem de perguntas igual, além do texto (`:205-213`). Ciclo: nenhuma pergunta viva e rc 2; lei calada: contagem igual e rc 0 (`:266-282`). |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py` | Marco vencido é contado e perguntas continuam inexistentes (`:435-438`). Helper roda sem alarme e não captura/verifica rc explicitamente (`:422-428`). |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py` | Mantém seis posições, log absoluto, serialização exata; exige alarme e proíbe apply (`:19-42`). |
| `app/ponto/tests/test_varredura_lote_prazo.py` | Igualdade exata `len(...) == 0`, mais papel `vigia` (`:62-63`). |
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py` | Helper exige contagem de perguntas igual; casos exigem distribuição e cardinalidade dos alvos (`:34-54`). |
| `app/chamados/tests/test_fabrica_por_evento.py` | Contador da fábrica e contagem de perguntas igual (`:82-84`). |
| `app/chamados/tests/test_fabrica_pela_ata.py` | Helper exige contagem igual antes de devolver relatório (`:34-41`). |
| `app/chamados/tests/test_e4a2_disparar_e_vigia.py` | Novo teste verifica quatro contagens e rc com/sem alarme, mas não detecta UPDATE; ressalva procedente do item 4 (`:140-162`). |

**Ressalva localizada:** “nenhuma viva” no ciclo permite, isoladamente, criar pergunta já morta; não equivale a contagem total igual (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266-272`). Correção mínima: também comparar contagens totais antes/depois nesse cenário (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266-272`).

## 09/11 22:06 · sessao 2e3d14447032 · - · - · origem=Ronald (tmux)

Achados de bloqueio:

- **PROCEDENTE — dois testes ainda exigem 1 juiz.** A lista agora vazia é incompatível com `assertEqual(len(...), 1)` em `app/escala/tests/test_termometro_regua_prazo.py:19` e `app/ponto/tests/test_selfie_ausente_juiz.py:148`. Correção mínima: atualizar ambos para zero, preservando as asserções de autoridade e ausência do cron.
- **PROCEDENTE — selo de host fica vermelho.** A exceção de `disparar_perguntas_competencia` permanece em `bin/tests/test_papel_prazo_nao_deriva.sh:303`; a condição de saída permanece em `:307`; o selo rejeita exceção cujo cron saiu da lista em `:325-326` e devolve erro em `:413`. Correção mínima: retirar a exceção após comprovar a condição, ajustar também a prova específica que exige essa exceção em `:388-389`.
- **PROCEDENTE — lista vazia vira falso “vigia falhou”.** `escritas_por_cron()` retorna `universo=0, crons={}` para lista vazia, pela construção em `app/chamados/services/vigia_loop.py:80-99`. O placar imprime apenas entradas de `crons`, e seu `grep` sem linhas aciona `escritas_por_cron=? (vigia falhou)` em `bin/placar_code.sh:204-209`. Correção mínima: representar explicitamente universo vazio/zero sem diagnosticar falha.

Mapa de valores:

| Valor | Escritor/definição | Leitores |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Tupla em `app/config/crons.py:2032`; disparar só aparece como comentário histórico em `:2044` | Construção de papéis em `:2164`; exceção da matriz em `app/core/contratos_estruturais.py:264`; testes de igualdade e verde em `app/core/tests/test_contract_tabuleiro.py:26,43-44`; host em `bin/tests/test_papel_prazo_nao_deriva.sh:325,328` |
| `PAPEL_DO_CRON` | Vigia declarado em `app/config/crons.py:2168` | `papel()` em `:2344`; juízes derivados em `:2356`; gerador de tabuleiro em `app/core/management/commands/gerar_diagrama.py:242`; subgrafo de vigias em `:397`; testes em `app/core/tests/test_contract_tabuleiro.py:16,19,26`; classificação de prazo no host em `bin/tests/test_papel_prazo_nao_deriva.sh:247` |
| `juizes_por_varredura()` | Deriva por papel sobre todas as entradas em `app/config/crons.py:2356` | `escritas_por_cron` em `app/chamados/services/vigia_loop.py:81-82`; placar em `bin/placar_code.sh:484-486`, soma ao TOTAL em `:555-556`; diagrama em `app/core/management/commands/gerar_diagrama.py:244,256,264`; contagens fixas conflitantes nos dois testes acima |
| Argv ENC | `--comp-corrente --alarme` em `app/config/crons.py:1521` | `corrente()` transporta args em `:1850`; comando serializa em `app/core/management/commands/crons.py:40-41`; shell recebe e executa em `bin/encadeado.sh:58-64,102-104`; testes em `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26-27` e `app/chamados/tests/test_selo_emissor_fio_mudo.py:193-198` |
| Rc 0/2 | Sem alarme termina normalmente; com alarme sai 2 por qualquer universo em `app/chamados/management/commands/disparar_perguntas_competencia.py:160-163` | Runner grava em `bin/cron_run.sh:11-13`, carimba fim em `:22-25`, aceita como recuperado em `:32-35`; corrente continua acumulando rc em `bin/encadeado.sh:64-68`; saúde aceita 0/2 em `app/core/services/crons_quebrados.py:16,46`; pipeline distingue alarme em `app/core/management/commands/pipeline_placar.py:75-79` |
| `FABRICA-POR-EVENTO` | `len(deixou)` e pares distintos em `app/chamados/management/commands/disparar_perguntas_competencia.py:153-155` | Parser operacional em `bin/placar_code.sh:388-390`; parsers de teste em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74` e `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; asserções em `app/chamados/tests/test_fabrica_por_evento.py:83` e `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127,133,161,392,396` |
| `VIGIA FIO MUDO` | `len(_mudos)` em `app/chamados/management/commands/disparar_perguntas_competencia.py:135-136` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:84,127,156,173,210,271,281`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; teste por substring em `app/chamados/tests/test_pergunta_do_dia_em_curso.py:432,437`; leitores operacionais específicos encontrados: [nao verificado] |
| `ALVOS` / distribuição | `app/chamados/management/commands/disparar_perguntas_competencia.py:147-148,157-158` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:75-76,113-114,181-182`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49-54`, `app/chamados/tests/test_fabrica_pela_ata.py:51,64,71`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`; leitor operacional específico: [nao verificado] |
| `[BARRADO]` | Quatro barreiras em `app/chamados/management/commands/disparar_perguntas_competencia.py:138-142` | Leitor operacional/parser específico: [nao verificado] |

**CONFORME — dependente e rc mantêm contrato.** `reconciliar_perguntas_orfas` é dependente pelo gatilho em `app/config/crons.py:1526`; o shell espera existência do arquivo de fim em `bin/encadeado.sh:84-94`; rc 2 também produz esse arquivo em `bin/cron_run.sh:22-25`. Seu comando só mede seu próprio universo, sem ler stdout ou rc do disparar, em `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29-36`. Portanto a ampliação do motivo de rc 2 é compatível com esses leitores; fim significa conclusão, não zero pendências (`bin/cron_run.sh:19-25`).

**CONFORME — célula não muda de cor.** O construtor `C()` usa `verde=False` por padrão em `app/core/contratos_estruturais.py:62`; a célula chamado/juiz não passa outro valor em `:262-270`. O numerador conta flags, não tamanho de exceções, em `:1053-1055`. O aval proibindo verde por reclassificação permanece em `:270`, e a cópia R6 ainda declara 19/20 em `app/core/placar_estrutural.py:440`. Esvaziar a tupla não a torna verde (`app/core/contratos_estruturais.py:62,262-270,1055`).

**PROCEDENTE documental — comentário contraditório.** A declaração diz que disparar saiu e virou vigia em `app/config/crons.py:2044`, mas a continuação diz que “segue ESCREVENDO” e não tem evento em `:2046`. A docstring de `juizes_por_varredura()` também apresenta disparar como emissor em `:2353`. Correção mínima: tornar explícito que esses trechos descrevem o passado.

**CONFORME com limitação de representação — geradores.** O tabuleiro usa todas as entradas e aceita lista vazia por `len` e laço em `app/core/management/commands/gerar_diagrama.py:242-265`; o subgrafo VIGIAS usa só `CRONS` em `:396-397`, portanto não passa a mostrar esse ENC individualmente. A função de lista usa compreensão e `sorted`, sem índice nem `max`, em `app/config/crons.py:2356`; os defeitos de vazio localizados estão nas asserções e no placar citados.

Execução dos testes/selos e comprovação da condição de pouso em produção: **[nao verificado]**. [nao verificado]

## 09/11 22:03 · sessao 3d32616f2541 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Cura aplicada: o comando conta os dois universos, remove as opções de escrita e sai 2 somente com `--alarme` e pendência. [comando:21](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:21), [saída:160](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:160) [nao verificado]

Arquivos tocados e motivo:

| Arquivo | Alteração |
|---|---|
| [disparar_perguntas_competencia.py:134](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:134) | Retirada do escritor e do rollback; diagnóstico `VIGIA FIO MUDO`. |
| [test_e4a2_disparar_e_vigia.py:24](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:24) | RED, base integral, mutantes, parser e teste ORM das quatro contagens. | [nao verificado]
| [e4a2_provas.py:34](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/e4a2_provas.py:34) | Execução das provas de fonte sem Django. |
| [test_selo_emissor_fio_mudo.py:205](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_selo_emissor_fio_mudo.py:205) | Contagem sem criação; cron e ciclo com alarme. |
| [test_jv4a_disparar_vigia_da_celula.py:132](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132) | FIO MUDO e limbo contam; retratada é preparada pelo evento. |
| [test_jv4a_elo_encadeado_tem_log.py:26](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26) | Argv exige alarme e rejeita apply. |
| [test_pergunta_do_dia_em_curso.py:434](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_pergunta_do_dia_em_curso.py:434) | Marco vencido é contado sem pergunta. |
| [test_cobparc5_parcial_sem_periodo_so_lista.py:198](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198) | Chamada com alarme. |
| [test_colab_nao_vaza_fio_mudo.py:35](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:35) | Recorte por colaborador e contagem persistida igual. |
| [test_fabrica_pela_ata.py:34](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_fabrica_pela_ata.py:34) | Ata conta sem criação pelo lote. |
| [test_fabrica_por_evento.py:76](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_fabrica_por_evento.py:76) | Conserva escritor do evento; lote não altera contagem. |
| [test_selo_disparo_nao_cobra_desligado.py:88](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:88) | Chamada com alarme. |
| [test_varredura_lote_prazo.py:62](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/ponto/tests/test_varredura_lote_prazo.py:62) | Exige zero juízes e papel vigia. |

O RED anterior à cura falhou nos sete itens proibidos. Depois, passaram **17 provas de fonte via unittest**; os **14 mutantes** foram rejeitados com rc 1. A base integral permanece no teste. [RED:48](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:48), [base:165](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:165), [runner:80](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/e4a2_provas.py:80) [nao verificado]

Comandos reproduzíveis do RED, provas posteriores e de cada mutante:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas base
# rc 1: sete violações

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas selos
# Ran 17 tests — OK

for m in import_perguntar chamada_perguntar rodar atomic runtime_error \
         opcao_apply opcao_com_push opcao_limite alarme_and \
         alarme_sem_chave alarme_so_celula alarme_so_mudo rc_zero emite
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas mutante "$m"
done
# rc 1 para cada mutante
```

Ruff local retornou `All checks passed!` nos 13 arquivos, com `check --no-cache` e as regras de [ruff.toml:1](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/ruff.toml:1).

A bateria Django abaixo foi tentada, mas nenhum módulo iniciou: faltou Django no import de [manage.py:11](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/manage.py:11). Portanto, execução ORM e serialização via `call_command` ficaram **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_selo_emissor_fio_mudo \
  chamados.tests.test_jv4a_elo_encadeado_tem_log \
  chamados.tests.test_jv4a_disparar_vigia_da_celula \
  chamados.tests.test_pergunta_do_dia_em_curso \
  chamados.tests.test_cobparc5_parcial_sem_periodo_so_lista \
  chamados.tests.test_colab_nao_vaza_fio_mudo \
  chamados.tests.test_fabrica_pela_ata \
  chamados.tests.test_fabrica_por_evento \
  chamados.tests.test_selo_disparo_nao_cobra_desligado \
  chamados.tests.test_e4a2_disparar_e_vigia \
  core.tests.test_contract_tabuleiro \
  ponto.tests.test_varredura_lote_prazo \
  --settings=config.settings.ci --verbosity=2
```

O brief integral não foi localizado; aderência a decisões ausentes dele: **[nao verificado]**. Certificação pela suite Code e pelo opus final: **[nao verificado]**. [nao verificado]

## 09/11 21:56 · sessao f21c98f8a5d3 · E4A1-C6-X-PASSADO-SEM-TURNO · sonda · origem=Code

```text
sonda_leitura: sonda1.py [2c1dcdffa761bf36] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-09T21:53:44.006061-03:00
DONO aberta universo 2 classes {'desfaz_pelo_julgamento': 2}
  exemplos(pk_pergunta, colab, empresa, dia) []
DONO exportada universo 187 classes {'sem_celula': 1, 'dia_sem_trabalho': 6, 'colab_nao_ativo': 132, 'desfaz_pelo_julgamento': 48}
  exemplos(pk_pergunta, colab, empresa, dia) []
ERROS 0
```

**1.** A saída acima é a fornecida no pedido. A tentativa de executar `python3 -B sonda1.py` falhou por arquivo inexistente; o fonte da sonda não foi disponibilizado na cópia. Portanto, a identidade das funções, da ordem e dos argumentos da sonda está **[nao verificado]**.

No código, a sequência é:

- Resolver `agora` e `hoje`, usando respectivamente `timezone.now()` e `timezone.localdate(agora)` quando ausentes: `app/chamados/services/fabrica_da_celula.py:160–161`.
- Avaliar, por curto-circuito: `cel.data < hoje`, situação do **colaborador da célula** igual a `'ativo'`, e `not dia_sem_trabalho(cel)`: `app/chamados/services/fabrica_da_celula.py:172`.
- Chamar `vencidos_no_instante(cel.colaborador, cel, hoje, agora)`; se retornar `None`, chamar `pauta_cadastro_sem_turno(cel)` e retornar `[]`, antes da coleta e do desfazimento: `app/chamados/services/fabrica_da_celula.py:173–180`.
- Dentro de `vencidos_no_instante`, chamar `marcos_vencidos(colab, cel, cel.data, agora)`; retornar `None` somente quando `venc` e `pend` estão vazios e a data é passada: `app/chamados/services/fabrica_da_celula.py:25–29`.

Há diferença entre os universos do vigia e da coleta do julgamento: o vigia filtra carimbo desfazível, disputa aberta e C6 vivo; a coleta do julgamento restringe também ao colaborador da célula, `data_evento=cel.data` e `conversa=False`, passando antes pelas guardas de `motivos_a_perguntar`. Referências: `app/chamados/services/vigia_fio.py:806–818`; `app/chamados/services/fabrica_da_celula.py:85–98`, `app/chamados/services/fabrica_da_celula.py:121–129`.

O vigia chama `pergunta_viva(p)` sem `hoje` explícito; a porta chamada pelo julgamento usa `pergunta_viva(p, hoje=hoje)`. Referências: `app/chamados/services/vigia_fio.py:813`; `app/chamados/services/relavra_via.py:242`. Os argumentos usados pela sonda estão **[nao verificado]**.

**2.** Sim, o vigia devolve **PKs de `PerguntaDisputa`**, acrescentando `p.pk` às listas por dono: `app/chamados/services/vigia_fio.py:804–818`.

`p.celula` é uma FK armazenada e anulável, separada de `data_evento`; não é uma busca automática pela célula dessa data. O filtro do vigia não exige igualdade entre `p.celula.data` e `p.data_evento`. Logo, esse universo pode admitir vínculo com outro dia ou ausência de vínculo. Referências: `app/chamados/models.py:2414–2425`; `app/chamados/services/vigia_fio.py:806–807`.

Já a coleta do julgamento encontra a pergunta pelo **`data_evento`**, sem exigir que sua FK aponte para a célula julgada: `app/chamados/services/fabrica_da_celula.py:121–129`. Portanto, não há garantia, nesse leitor, de que `p.celula` seja a célula cujo julgamento coletaria a pergunta.

A saída fornecida mostra **1 `sem_celula`, em `exportada`**, e nenhuma ocorrência dessa classe em `aberta`. Sem o fonte da sonda, o significado exato desse rótulo — FK nula ou ausência da célula procurada por outra chave — está **[nao verificado]**.

**3.** Para perguntas que permaneçam vivas pelo C6, os leitores incluem:

- **C6 e propriedades:** o C6 pode retornar vivo apesar do carimbo por resposta, célula acusando, classificação da falta ou ausência de lastro; `.viva` delega ao C6 e `.estado` consulta o C6 antes de tratar o carimbo como resolvido. Referências: `app/chamados/juizes.py:1341–1353`, `app/chamados/juizes.py:1372–1406`; `app/chamados/models.py:3041–3047`, `app/chamados/models.py:3375`.
- **Telas do colaborador:** `perguntas_do_app` filtra pelo C6; `consulta_do_app` separa pendentes e histórico; a API serve essas listas. Referências: `app/chamados/services/pergunta_no_app.py:44–74`; `app/api/views.py:2560–2562`, `app/api/views.py:2589–2601`.
- **Telas administrativas:** o relatório classifica pelas perguntas vivas; o banner do drawer lê `perguntas_vivas_de`. Referências: `app/chamados/relatorios_disputa.py:18–23`; `app/colaboradores/services/drawer.py:77–82`.
- **Contadores:** dashboard e badge contam as pendentes de `consulta_do_app`; a materialização conta `perguntas_vivas_de`; o vigia conta os carimbos vivos por dono e imprime `MATERIALIZADA_VIVA`. Referências: `app/core/views.py:252–253`; `app/api/views_core.py:313–316`; `app/chamados/services/materializacao.py:1313–1314`; `app/chamados/services/vigia_fio.py:847–851`; `app/chamados/management/commands/vigiar_fio_morto.py:198–199`.

A saída fornecida não apresenta a classe `PASSADO_SEM_TURNO_nao_desfaz` nem exemplos dela; a presença de casos concretos dessa classe nessas telas e contadores está **[nao verificado]**.

## 09/11 21:48 · sessao a038309c1c18 · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Alterado somente [chamados/services/fabrica_da_celula.py:13](chamados/services/fabrica_da_celula.py:13): inscrição na docstring, com entradas, saídas e invariantes citando as funções. Nenhum código executável mudou.

RED antes da alteração, no [selo existente](core/tests/test_selo_mapa_contrato.py:41):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest core.tests.test_selo_mapa_contrato.SeloMapaContratoTest.test_MORDE_bolha_nova_sem_contrato_e_vermelha -v
```

```text
AssertionError: Lists differ: ['chamados/services/fabrica_da_celula.py'] != []
FAILED (failures=1)
```

Após a alteração:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning -m unittest core.tests.test_selo_mapa_contrato -v
```

```text
Ran 4 tests
OK
```

[Parser real](core/mapa/contrato.py:180):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from core.mapa import contrato
r = 'chamados/services/fabrica_da_celula.py'
f, t = contrato.sem_contrato()
print('arquivo_nas_faltas:', r in f)
print('campos_faltando:', contrato.faltando(contrato.do_arquivo(r)))
print('sem_contrato:', len(f), 'universo:', t)
assert r not in f
PY
```

```text
arquivo_nas_faltas: False
campos_faltando: []
sem_contrato: 38 universo: 69
```

Ruff no arquivo alterado:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache chamados/services/fabrica_da_celula.py
```

```text
All checks passed!
```

Mutantes dos quatro campos, em memória, exercitando o [mesmo selo](core/tests/test_selo_mapa_contrato.py:41):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning - <<'PY'
import ast, io, unittest
from unittest.mock import patch
from core.mapa import contrato

rel = 'chamados/services/fabrica_da_celula.py'
original = contrato._arvore
for campo in ('bolha', 'entra', 'sai', 'invariante'):
    arvore = original(rel)
    arvore.body[0].value.value = ast.get_docstring(arvore).replace(
        '@' + campo + ' ', '@removido ')
    with patch.object(contrato, '_arvore',
                      side_effect=lambda r: arvore if r == rel else original(r)):
        suite = unittest.defaultTestLoader.loadTestsFromName( [nao verificado]
            'core.tests.test_selo_mapa_contrato.SeloMapaContratoTest.'
            'test_MORDE_bolha_nova_sem_contrato_e_vermelha')
        resultado = unittest.TextTestRunner(stream=io.StringIO()).run(suite) [nao verificado]
    assert len(resultado.failures) == 1 and not resultado.errors
    assert rel in resultado.failures[0][1]
    print('MORTO @' + campo + ': FAILED (failures=1)')
PY
```

```text
MORTO @bolha: FAILED (failures=1)
MORTO @entra: FAILED (failures=1)
MORTO @sai: FAILED (failures=1)
MORTO @invariante: FAILED (failures=1)
```

Suite inteira: [não verificado]. [nao verificado]

_3258 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
