# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 15:15** · publicado em **13/11/2026 15:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 15:15 · sessao cee2ecca4203 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**BLOQUEANTE — o RED novo prova transporte de `hoje`, mas deixa passar a volta do relógio interno.** Ele espia `folha.export.informativo_rescisao` e verifica somente os argumentos recebidos; não verifica leituras do relógio nem um resultado dependente da data. (`app/folha/tests/test_lei_desligado_nao_trava.py:132`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)

**Contraexemplo concreto:** substituir `hoje = hoje or _tz.localdate()` por `hoje = _tz.localdate()`. O spy continua recebendo `06/10`, embora o interior use `25/10`. A fixture tem células `nunca_bateu`; esse ramo não aplica o filtro `d < hoje`. Portanto, esse mutante escapa da asserção nova. Execução do teste Django completo com esse mutante: **[nao verificado]**. (`app/folha/export.py:1238`; `app/folha/tests/test_lei_desligado_nao_trava.py:37`; `app/folha/export.py:1273`; `app/folha/export.py:1294`)

A cura produtiva **existe para o informativo do lavrador**: captura local em `:48`, passagem em `:65`, transporte pelo retrato em `:1163` e pelo helper em `:1225`, chegando ao laço em `:1228`. O RED morde retirar essa passagem; não morde ignorá-la dentro da autoridade. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1163`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`)

**NAO-BLOQ neste delta — a documentação chama de “borda” uma leitura que continua no interior.** O comentário novo atribui ao Haiku uma leitura “na borda dele”, mas `o_que_segura` omite `hoje`; quem lê é `_dias_sem_decisao`. (`app/folha/retrato_folha.py:1211`; `app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`)

**Contraexemplo concreto:** desligado com vínculo, sem célula em 05/10 e demissão posterior. Uma chamada iniciada em 05/10 às 23:59 que alcance o fallback depois da meia-noite passa a incluir 05/10 pelo teste `d < hoje`. Esse caminho continua aberto; a injeção do lavrador não o alcança. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

| Caminho | Quem lê o relógio; relação com o informativo |
|---|---|
| Lavrador | Lê uma data local e a injeta até `informativo_rescisao`. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/retrato_folha.py:1225`) |
| `montar_linhas` avulso | Aceita `hoje=None`; nessa modalidade cada desligado elegível pode chegar ao fallback interno. (`app/folha/retrato_folha.py:1120`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`) |
| Tela e drawer | Leem o retrato persistido. O relógio da tela escolhe competência; não recalcula o informativo. (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:190`; `app/folha/views_retrato.py:196`; `app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:60`) |
| Haiku `o_que_segura` | Omite `hoje`; leitura efetiva no interior do export. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`) |
| Vigia | Captura data local, mas segue por `dias_que_seguram` e retenção, sem chamar o informativo. Também não transporta seu `hoje` até esse retentor. (`app/chamados/management/commands/vigiar_fio_morto.py:169`; `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:98`) |

Os relógios identificados nesses caminhos usam `localtime(...).date()` ou `localdate()`, com configuração da cópia `America/Sao_Paulo`, `USE_TZ=True`. **Não há divergência UTC demonstrada no `hoje` do informativo.** Configuração efetiva de produção: **[nao verificado]**. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/export.py:1238`)

**NAO-BLOQ — o cabeçalho da bolha omite diferenças entre agendamento e consumação.** “Agendado só grava na data” vale para o valor `situacao='desligado'`; não vale para `data_demissao` e motivo, gravados antecipadamente. A saída “encerra vínculo e abre chamado DP” também precisa distinguir o ramo agendado. (`app/colaboradores/services/desligamento.py:9`; `app/colaboradores/services/desligamento.py:11`; `app/colaboradores/services/desligamento.py:110`)

**Contraexemplo concreto:** agendar amanhã grava data/motivo hoje e retorna antes do encerramento de vínculo e da criação do chamado. O cron depois chama o mesmo executor; não constitui segundo escritor do valor `desligado`. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:126`; `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/desligamento.py:188`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`)

`esta_desligado` corresponde ao predicado inscrito: lê somente `situacao`. Já `julgar_desligamento` recebe POST e lê `date.today()` internamente; “para o juiz, só o colaborador” só descreve corretamente o primeiro. O selo de mapa cobra presença das etiquetas, sem demonstrar a verdade dessas descrições. (`app/colaboradores/services/desligamento.py:7`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:35`; `app/colaboradores/services/desligamento.py:60`; `app/core/tests/test_selo_mapa_contrato.py:41`; `app/core/tests/test_selo_mapa_contrato.py:60`)

**NAO-BLOQ — o rótulo de consultas foi corrigido, mas a documentação atribui a segunda consulta à fonte errada.** O teste agora anuncia corretamente **uma chamada** ao helper mockado. O helper continua consultando colaboradores e, por desligado elegível, células e **vínculos**, não “férias/ausências”. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1351`; `app/folha/retrato_folha.py:1210`; `app/folha/retrato_folha.py:1222`; `app/folha/export.py:1247`; `app/folha/export.py:1253`)

**Contraexemplo concreto:** um desligado sem qualquer férias/ausência ainda percorre as consultas de `CelulaDia` e `EscalaColaborador`. O custo `1+2D` permanece para essas consultas-base; redução do N+1 não foi implementada nesse caminho. Latência produtiva: **[nao verificado]**. (`app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:148`; `app/escala/alimentacao.py:186`)

**D1 tem mordida para a regressão caracterizada.** A expectativa nova `[]` é efetivamente comparada: `retencao` integra `APURACAO`, `_medir` lê `fm.motivos_espelho` e `_confere_barrado` compara essa chave. Restaurar `['sem_celula']` nesse resultado falha contra as duas expectativas novas. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:329`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:361`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:644`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:675`)

A cadeia passa pelo recálculo real, que escreve `motivos_espelho` usando o wrapper do retentor. Isso cobre a retenção materializada do caso D1; execução nesta revisão: **[nao verificado]**. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:604`; `app/ponto/services/fechamento.py:1067`; `app/folha/export.py:1307`)

Os escritores, leitores e significados relevantes ficam assim:

| Campo/valor | Escritor → leitor; significado |
|---|---|
| `hoje` | Lavrador → retrato → informativo → laço; data civil usada nos ramos de célula ausente ou sem veredito. Há fallback interno quando omitido. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`; `app/folha/export.py:1295`) |
| `informativo` | Export produz dias; helper converte para ISO; `montar_linhas` monta a chave; lavrador persiste; drawer lê. Não se confunde com `datas`/`dias` de retenção. (`app/folha/export.py:1228`; `app/folha/retrato_folha.py:1225`; `app/folha/retrato_folha.py:1193`; `app/folha/management/commands/lavrar_retrato_folha.py:76`; `app/templates/folha/_retrato_o_que_fazer.html:60`) |
| `situacao` | Executor grava `desligado` pela porta; juiz lê. Reativação grava `ativo`: unicidade do escritor do **valor desligado** não significa unicidade de escritor do campo inteiro. (`app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:235`) |
| `data_demissao` / motivo | Executor grava antecipadamente ou na consumação; cron lê o agendamento; export lê a data para rescisão e recorte. **A data representa intenção futura ou desligamento consumado**, distinguíveis pela situação. (`app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:130`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/folha/export.py:60`; `app/folha/export.py:1241`) |
| `retencao` de D1 | Fechamento escreve `motivos_espelho`; teste o copia para `retencao` e compara com `MEDIDO`. É lista de motivos, não quantidade de dias. (`app/ponto/services/fechamento.py:1067`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:644`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:675`; `app/folha/export.py:1298`) |
| `desligados_retidos` | Fonte produz lista de ids; vigia agrega seu comprimento por empresa/competência. Há mudança explícita de lista para inteiro; não é contagem global de pessoas únicas. (`app/folha/fora_da_folha.py:144`; `app/folha/fora_da_folha.py:146`; `app/chamados/services/vigia_fio.py:584`; `app/chamados/services/vigia_fio.py:589`) |

O Raio-X **continua sem cura**: seleciona pelo export, recorta a demissão e reconstrói dias com `furos_por_dia`; o preview continua contando motivos como dias. Contraexemplo: três dias com um mesmo motivo aparecem como `1` nesse preview. São resíduos, não novas derivações introduzidas pela injeção de `hoje`. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/folha/views.py:398`; `app/folha/export.py:1298`)

No código produtivo acrescentado, não identifiquei segundo juiz ou segundo escritor fora da porta: permanece a delegação ao mesmo laço e a persistência pelo lavrador. A classe de reprovação nova demonstrada é **RED sem mordida para a propriedade anunciada**. Suíte integral, opus, pouso e medição produtiva de competência 11: **[nao verificado]**. (`app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`; `app/folha/management/commands/lavrar_retrato_folha.py:74`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)

## 13/11 15:15 · sessao 049bf09ab549 · - · - · origem=Ronald (tmux)

Contagem transitiva confirmada: em execução normal com `dias` não vazio, **`alimentar_fila = 2 + b + j`**. As duas cargas principais chamam por `alimentar_portas` e pela primeira `CargaDaTarefa.tarefa`; `b` indica a alimentação extra de `beco_em_lote` quando a lista é carregada e encontra células; `j` é o número de janelas distintas `(ini,fim)` com perguntas vivas usadas nas cargas extras da tarefa. Sem dias, retorna antes das duas cargas. Referências: `app/chamados/services/vigia_fio.py:594`, `app/chamados/juizes.py:1594`, `app/chamados/services/tarefa_do_dia.py:529`, `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:492`, `app/escala/services/cadastro_realidade.py:495`.

A lista chama `_folga_nao_definida_sem_porta` uma vez; ela reúne todas as janelas e chama `beco_em_lote` uma vez. A `ListaDoAdmin` já carregada não repete essa execução. Referências: `app/escala/services/cadastro_realidade.py:873`, `app/escala/services/cadastro_realidade.py:574`, `app/escala/services/cadastro_realidade.py:582`, `app/chamados/services/vigia_fio.py:205`.

Cada `alimentar_fila` também consulta linhas de `PerguntaDisputa` pelos créditos: destino em vivos OU chamado da disputa em vivos; acrescenta `q_vivas()` e exclui materializada com `VL_SIM`; lê cinco campos com `values_list`, sem materializar objetos de `PerguntaDisputa`/`Colaborador`. Referências: `app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:56`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`.

Cargas transitivas adicionais de `PerguntaDisputa`:

- Por bloco de datas e colaborador sem porta em dia passado, calendário consulta `disputa__colaborador=colaborador`, `data_evento__range=(data_ini,data_fim)`, exclui `resposta_colab=''`, com `select_related('disputa__colaborador','celula','chamado_destino')`. Julga `_pv(_q)` sem `agora`. O JOIN também materializa `Colaborador`, embora o mapa depois imponha a instância semeada. Referências: `app/chamados/services/vigia_fio.py:642`, `app/chamados/services/vigia_fio.py:266`, `app/colaboradores/services/calendario.py:128`, `app/colaboradores/services/calendario.py:472`.
- Cada alimentação extra do beco repete o sítio de perguntas A; cada carga extra de tarefa repete o sítio B. Referências: `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:492`.

Cargas transitivas de `Colaborador`, além do núcleo A/B:

| Sítio | Filtro / quantidade |
|---|---|
| `app/folha/export.py:785` | Por empresa×competência: `FechamentoMensal.filter(colaborador__empresa=empresa,mes=mes,ano=ano).select_related('colaborador')`, chamado por `dias_que_seguram` em `app/folha/fora_da_folha.py:130`. |
| `app/folha/fora_da_folha.py:142` | Outra carga por empresa×competência: `Colaborador.filter(pk__in=_ids_furo).select_related('empresa')`. `_ids_furo` sai dos mesmos itens do export, portanto esse conjunto é subconjunto do anterior (`app/folha/fora_da_folha.py:133`). |
| `app/colaboradores/services/calendario.py:474` | JOIN descrito acima, uma carga por bloco; colaborador já conhecido e semeado em `app/colaboradores/services/calendario.py:475`. |
| `app/escala/services/cadastro_realidade.py:485` | Se beco encontra células: `Colaborador.filter(pk__in={ids das células}).select_related('empresa')`. |
| `app/escala/services/cadastro_realidade.py:612` | `_afastado_sem_registro`: queryset `exclude(situacao='desligado')`, opcional empresa; lê `pk` com `situacao='afastado'` em `:616`. O mesmo queryset entra como subquery de ausência em `:615`, por `app/colaboradores/services/situacao_exibida.py:44` e `app/ponto/turnos.py:2715`. |
| `app/escala/services/cadastro_realidade.py:624` | Se houver afastados: `filter(pk__in=pks).select_related('empresa').order_by('pk')`. |
| `app/escala/services/cadastro_realidade.py:185` | Por empresa, lê somente PKs de `filter(pk__in=ids de células nunca_bateu,situacao='desligado')`; loop em `:179`, células em `:181`. |
| `app/escala/services/cadastro_realidade.py:777` | `_vinculos`: `filter(pk__in=ids).select_related('empresa')`, onde IDs são lavra ∪ cadastro (`:895–900`); filtro de empresa ocorre depois da carga (`:907–909`). |
| `app/escala/services/cadastro_realidade.py:1025` | `select_related('empresa').get(pk=cid)`, uma vez por colaborador no memo local; apenas donos divergentes sem C11 utilizável (`:1015–1024`). |
| `app/ponto/supra_juiz.py:177` | Leitura escalar `Colaborador.filter(pk=...).values_list('data_demissao').first()` por dia sem vínculo, alcançada por `um_dono_por_dia → InsumosDoDono.dono`; condição em `:174`, chamada em `app/ponto/management/commands/e6_oraculo.py:183`. Beco fornece cadastro e evita essa rota (`app/escala/services/cadastro_realidade.py:534`). |

Não encontrei outras consultas de `PerguntaDisputa` nas rotas de lista/`InsumosDoDono` inspecionadas; `InsumosDoDono` carrega batidas, turnos, lavras e cadastro, com `sem_motor=True` impedindo chamada ao espelho. Referências: `app/ponto/management/commands/e6_oraculo.py:130`, `app/ponto/management/commands/e6_oraculo.py:156`, `app/ponto/management/commands/e6_oraculo.py:162`, `app/ponto/management/commands/e6_oraculo.py:174`, `app/escala/services/cadastro_realidade.py:1033`.

Quantidade efetivamente executada, cardinalidades e interseções concretas em produção: **[nao verificado]**.

## 13/11 15:15 · sessao c1cfee3aaef7 · - · - · origem=Ronald (tmux)

Levantamento de A e leitores, somente código.

- `alimentar_fila` escreve `verbo` com `particionar_verbos(qs_vivos(), agora=agora)['mapa']`; consulta a fila global com `qs_na_fila(agora=agora).values_list('pk','celula_id')`; descarta pks ausentes de `verbo`; `fila_ids` permanece global, e somente `fila_cel` é recortado pelos ids recebidos. `app/chamados/juizes.py:1557`, `app/chamados/juizes.py:1558`, `app/chamados/juizes.py:1561`, `app/chamados/juizes.py:1562`, `app/chamados/juizes.py:1564`, `app/chamados/juizes.py:1565`, `app/chamados/juizes.py:1567`.
- A fonte `qs_vivos` usa `ChamadoColaborador`, sem filtros opcionais nas chamadas acima, com `status_local__in=VIVOS`; a base declara `select_related('colaborador','colaborador__empresa','colaborador__posto','respondido_por')` e `prefetch_related('followups')`; a partição utiliza projeção por `values_list` e remove prefetch. `app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:100`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/verbos.py:44`.
- A fila acrescenta `status_local__in=COBRANCA` e exclui `silenciado_ate__gt=agora` (ou `timezone.now()` se `None`). `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:189`.
- `verbo` também depende de créditos de perguntas: `PerguntaDisputa.filter(Q(chamado_destino__in=vivo)|Q(disputa__chamado__in=vivo))`, `q_vivas()`, exclusão de `materializada_em` preenchido com `veredito_lavrado=VL_SIM`, projeção de cinco campos; não materializa `Colaborador` nessa projeção. `app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:56`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`.
- `alimentar_portas` escreve `vivas` lendo `PerguntaDisputa.filter(celula_id__in=_ids).select_related('disputa','celula').order_by('pk')`, passando por identity map e `pergunta_viva(p, agora=agora)` em TODAS as linhas carregadas. `app/chamados/juizes.py:1599`, `app/chamados/juizes.py:1600`, `app/chamados/juizes.py:1603`, `app/chamados/juizes.py:1604`.
- Escreve `instancias` inicialmente de `colabs`; `colab` vem de `CelulaDia.filter(pk__in=_ids).values_list('pk','colaborador_id')`; inicia `no_app={}`. `app/chamados/juizes.py:1598`, `app/chamados/juizes.py:1606`, `app/chamados/juizes.py:1607`.

Leituras no censo:

| Chave | Leituras e destino |
|---|---|
| `fila_ids` | Alias `fila_pk` em `app/chamados/services/vigia_fio.py:609`; pertença da âncora na formação do véu em `app/chamados/services/vigia_fio.py:674`. |
| `verbo` | Não há leitura no tronco atual de `censo_alcance`: carga A em `app/chamados/services/vigia_fio.py:608`, e porta B em `app/chamados/services/vigia_fio.py:640`, `app/chamados/services/vigia_fio.py:641`. Ausência global de outros acessos dinâmicos: [não verificado]. |
| `fila_cel` | Mesma situação de `verbo`; a porta do censo vem de B em `app/chamados/services/vigia_fio.py:641`. |
| `vivas` | Alias `perg` em `app/chamados/services/vigia_fio.py:614`; âncoras para carregar status em `app/chamados/services/vigia_fio.py:621`; `ps` do dia em `app/chamados/services/vigia_fio.py:646`; estados de âncoras/véu em `app/chamados/services/vigia_fio.py:670`, `app/chamados/services/vigia_fio.py:673`; causas e detalhes em `app/chamados/services/vigia_fio.py:682`, `app/chamados/services/vigia_fio.py:684`, `app/chamados/services/vigia_fio.py:686`, `app/chamados/services/vigia_fio.py:690`, `app/chamados/services/vigia_fio.py:694`, `app/chamados/services/vigia_fio.py:698`, `app/chamados/services/vigia_fio.py:700`. |
| `colab` | Não é usado pelo wrapper `no_app`: ele passa diretamente o `cid` recebido a `_no_app`, em `app/chamados/services/vigia_fio.py:624`, `app/chamados/services/vigia_fio.py:625`. |
| `no_app` | `_no_app` lê presença/cache e retorna conjunto em `app/chamados/juizes.py:1613`, `app/chamados/juizes.py:1621`; o censo consulta pertença de `p.pk` em `app/chamados/services/vigia_fio.py:700`. |
| `instancias` | Segundo carregamento de perguntas (`perg_todas`) compartilha mapa em `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:619`; `_no_app` usa mapa em `app/chamados/juizes.py:1616`. |

`perg_todas` é variável, não função: repete a query por `celula_id__in=_cels`, `select_related('disputa','celula')`, sem filtro de vitalidade; passa o conjunto ao `dono_fora_do_fio`. `app/chamados/services/vigia_fio.py:617`, `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:663`.

`dono_fora_do_fio` usa essas perguntas em `p.estado == VALIDADA` e `_e_aceite_de_folga(p)`; não recebe o dicionário `alim`. `app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:311`. `p.estado` chama `pergunta_viva(self)` sem `agora` se `validada_em` é nulo; quando preenchido, retorna antes. `app/chamados/models.py:3067`, `app/chamados/models.py:3070`.

`respondida_sem_texto` lê `respondida_em` e `resposta_colab`; `chamado_que_lista` lê `chamado_destino_id`, ou `disputa.chamado_id`. `app/chamados/juizes.py:1518`, `app/chamados/services/fio.py:116`, `app/chamados/services/fio.py:117`.

`_no_app` pode escrever tanto `instancias` quanto `no_app`; busca `Colaborador.filter(pk=colab_id).first()` somente se id não estiver no mapa; escreve os ids devolvidos por `perguntas_do_app(_c)` sem passar `agora`. `app/chamados/juizes.py:1617`, `app/chamados/juizes.py:1618`, `app/chamados/juizes.py:1620`. `perguntas_do_app` consulta por `disputa__colaborador=colab`, aplica `q_vivas()`, `select_related('disputa','celula')`, `order_by('id')`, e julga `pergunta_viva(p)` sem argumentos temporais/carga. `app/chamados/services/pergunta_no_app.py:49`, `app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:58`.

A função de identity map não faz consulta explícita: troca o colaborador pela instância existente; após o consumidor, acrescenta instância que ele tenha carregado. `app/chamados/juizes.py:1179`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1185`.

Cuidado com a contagem por rodada: duas chamadas `alimentar_fila` são as do tronco A/B com universo não vazio: A em `app/chamados/juizes.py:1594`; B preguiçosa, uma vez por carga, em `app/chamados/services/tarefa_do_dia.py:529`, `app/chamados/services/tarefa_do_dia.py:531`. Contudo, a expansão transitiva de `ListaDoAdmin` pode acrescentar outras:

1. `ListaDoAdmin` carrega uma vez se ainda não carregada, chamando `cadastro_realidade.lista`. `app/chamados/services/vigia_fio.py:205`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:216`.
2. `lista` chama `_folga_nao_definida_sem_porta`, que chama `beco_em_lote` uma vez se há janelas. `app/escala/services/cadastro_realidade.py:873`, `app/escala/services/cadastro_realidade.py:582`.
3. `beco_em_lote` sai se não há células; caso contrário chama outro `alimentar_portas` e monta uma carga B por janela distinta com perguntas vivas, consultando tarefas dessas janelas. Em execução completa isso acrescenta `1+J` chamadas de `alimentar_fila`, onde `J` é o número dessas janelas. `app/escala/services/cadastro_realidade.py:483`, `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:492`, `app/escala/services/cadastro_realidade.py:496`, `app/escala/services/cadastro_realidade.py:497`.
4. As células adicionais são ativas, `veredito='indefinida'`, protesto `PREVISTO_COM_DIA_INDEFINIDO` e dentro das janelas; seus colaboradores são carregados por pk com `select_related('empresa')`. `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:479`, `app/escala/services/cadastro_realidade.py:480`, `app/escala/services/cadastro_realidade.py:485`.
5. O censo cria a lista default SEM `agora`; a lista nesse caminho acaba lendo `timezone.now()`. Objeto fornecido pelo chamador pode trazer outro estado/instante, não verificável sem execução. `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/escala/services/cadastro_realidade.py:803`.

Outra carga condicional de perguntas/colaborador no grafo do censo é a do calendário: `_resolver_da_rodada` chama `resolver_dia_oferecido_nas_datas`, que monta calendários em blocos até 366 dias; consulta `PerguntaDisputa` por `disputa__colaborador=colaborador`, `data_evento__range`, exclui `resposta_colab=''`, faz `select_related('disputa__colaborador','celula','chamado_destino')`, e julga `_pv(_q)` sem `agora`. `app/chamados/services/vigia_fio.py:270`, `app/chamados/services/vigia_fio.py:642`, `app/colaboradores/services/calendario.py:114`, `app/colaboradores/services/calendario.py:129`, `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:473`, `app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/calendario.py:476`.

Outros consumidores executáveis localizados fora dos testes: [nao verificado]

- `cadastro_realidade.beco_em_lote`: chama A e lê apenas `vivas` diretamente. `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:496`.
- `juizes.porta_alcancavel_do_dia`: chama A se não recebeu alimentação, e lê `fila_cel`, `verbo`, `vivas`, `fila_ids`, `colab`, além de `no_app`/`instancias` via `_no_app`. `app/chamados/juizes.py:1676`, `app/chamados/juizes.py:1677`, `app/chamados/juizes.py:1678`, `app/chamados/juizes.py:1679`, `app/chamados/juizes.py:1681`, `app/chamados/juizes.py:1682`, `app/chamados/juizes.py:1686`.
- Comando de diagnóstico `diff_e3_sombra.Command.comparar`: chama A, passa à porta C17 e a `casos_do_par`; esta última lê `fila_cel`, `vivas`, `fila_ids`. `app/chamados/management/commands/diff_e3_sombra.py:116`, `app/chamados/management/commands/diff_e3_sombra.py:127`, `app/chamados/management/commands/diff_e3_sombra.py:131`, `app/chamados/management/commands/diff_e3_sombra.py:64`, `app/chamados/management/commands/diff_e3_sombra.py:65`, `app/chamados/management/commands/diff_e3_sombra.py:66`.

Busca textual em todo repositório não localizou outra chamada executável C17 externa ao comando de diagnóstico; resolução/importação dinâmica não foi demonstrada [não verificado].

## 13/11 15:13 · sessao 4e7d28618bda · - · - · origem=Ronald (tmux)

Auditoria de B, somente pelo código:

- **Entrada e universo:** `censo_alcance` abre B com `list(colabs.values())`, menor e maior data do conjunto do censo e o mesmo `agora`; depois chama `carga.tarefa(...)` para cada par do censo (`app/chamados/services/vigia_fio.py:639-641`). `CargaDaJanela` guarda esses colaboradores em `_colabs`, carrega células da janela com padding e as organiza em `_cels`; não recarrega `Colaborador` nessa base (`app/ponto/turnos.py:2973-2992`).
- **Recarga de cadastro:** B executa `Colaborador.objects.select_related('empresa').in_bulk(ids)` se houver IDs, mesmo recebendo instâncias já carregadas (`app/chamados/services/tarefa_do_dia.py:370-372`). B também carrega colaboradores por JOIN na query de perguntas, por `select_related('disputa__colaborador__empresa', ...)` (`app/chamados/services/tarefa_do_dia.py:376-380`).
- **Perguntas de B:** uma query de `PerguntaDisputa` com `colaborador_id__in=ids`, `conversa=False`, `data_evento__gte=self.ini`, `data_evento__lte=self.fim`; `select_related('disputa__colaborador__empresa', 'celula', 'chamado_destino')`; `order_by('colaborador_id', 'pk')`; agrupa em `_perg[(colaborador_id,data_evento)]` (`app/chamados/services/tarefa_do_dia.py:376-383`). A lê por `celula_id__in=_ids`, sem filtro de conversa/data, `select_related('disputa','celula')` e ordem PK (`app/chamados/juizes.py:1599-1604`). Os universos podem se sobrepor, mas não são definidos pelo mesmo predicado (mesmas referências).
- **C6 em B:** `_ler_vida` seleciona candidatas por grade, isenção, tipo do dia, tranca, exportação e exposição fora dos marcos normais; em dia de trabalho cujas perguntas estão todas nos marcos da grade, pula C6; quando necessário, seleciona todas as perguntas daquele dia (`app/chamados/services/tarefa_do_dia.py:467-488`). Semeia `p.disputa.colaborador` com `_cadastro[cid]`, cria `carga_da_vida(self,candidatas)` e chama `pergunta_viva(p, hoje=_hoje(self.agora), carga=carga, agora=self.agora)` apenas para candidatas (`app/chamados/services/tarefa_do_dia.py:481-493`).
- **Comparação de relógio:** A chama `pergunta_viva(p, agora=agora)` para todas as perguntas carregadas (`app/chamados/juizes.py:1600-1604`). O wrapper transforma `hoje=None` em `_hoje(agora)` se houver `agora`, portanto não é correto dizer que A e B necessariamente usam dias civis diferentes; os argumentos/caminhos diferem porque B fornece carga, que desvia do memo avulso (`app/chamados/juizes.py:883-894`). `CargaDaVida` reutiliza o cadastro e a tranca de B; não faz outra query de `PerguntaDisputa` nem de `Colaborador` no próprio corpo (`app/chamados/services/carga_pergunta_viva.py:77-112`).
- **Fila preliminar de B:** além de `alimentar_fila`, o construtor chama `qs_na_fila(_base=ChamadoColaborador.objects.filter(pk__in=ancoras), agora=agora)` para as âncoras das perguntas de B e guarda `_fila` (`app/chamados/services/tarefa_do_dia.py:415-418`). Isso é distinto da alimentação global de portas (mesmas referências; `app/chamados/juizes.py:1558-1567`).
- **Segunda chamada de `alimentar_fila`:** primeira `tarefa()` inicia `_portas` com todas as células de `_cels` dentro de `[ini,fim]`, usando `agora=self.agora`; chamadas seguintes reutilizam `_portas` (`app/chamados/services/tarefa_do_dia.py:427`, `app/chamados/services/tarefa_do_dia.py:529-534`). A chama com `_cels` dos pares exatos do censo (`app/chamados/services/vigia_fio.py:600-608`). Assim os argumentos de célula diferem: B pode incluir dias intermediários de todos os colaboradores; A só células dos pares enumerados (mesmas referências).
- **O recorte de célula só afeta `fila_cel`:** `alimentar_fila` sempre particiona `qs_vivos()` global com `agora`, sempre consulta `qs_na_fila(agora=agora)` global para `fila_ids`, e apenas testa `cel_id in _ids` ao montar `fila_cel` (`app/chamados/juizes.py:1557-1567`). Logo, mesmo estado do banco e mesmo instante implicam mesma definição para `verbo`/`fila_ids`; diferentes listas de célula não tornam esses dois conjuntos recortados (mesma referência). Há leituras separadas e o próprio código trata corrida entre elas; ocorrência real de divergência em produção: **[não verificado]** (`app/chamados/juizes.py:1561-1563`).
- **Autoridade/query de fila:** `qs_vivos()` usa `montar_qs_filtrado(...)` e filtra `status_local__in=VIVOS` (`app/chamados/services/painel.py:153-155`); a base tem `select_related('colaborador','colaborador__empresa','colaborador__posto','respondido_por')`, prefetch followups e anotações/ordenação (`app/chamados/services/painel.py:98-115`). `qs_na_fila` acrescenta `status_local__in=COBRANCA` e exclui `silenciado_ate__gt=agora` (`app/chamados/services/painel.py:188-190`). A partição usa `values_list` e remove prefetch, não materializa os colaboradores desses JOINs (`app/chamados/services/verbos.py:44-46`).
- **Leitura adicional de perguntas por chamada de `alimentar_fila`:** `particionar_verbos` chama `creditos_perguntas(vivo_qs)` (`app/chamados/services/verbos.py:34`). Este lê `PerguntaDisputa` cujo destino ou chamado da disputa está no universo vivo, aplica `q_vivas()`, exclui `materializada_em__isnull=False` junto com `veredito_lavrado=VL_SIM`, e materializa `values_list('chamado_destino_id','disputa_id','disputa__chamado_id','respondida_em','resposta_colab')`; sem `select_related`, sem conversa/data/célula e sem chamada Python de C6 (`app/chamados/services/raiox.py:56-58`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135-144`). Portanto são duas execuções dessa leitura no caminho normal com dias, uma por cada `alimentar_fila` (mesmas referências; `app/chamados/juizes.py:1594`; `app/chamados/services/tarefa_do_dia.py:529-533`).
- **API de B e equivalência das sete chaves:** B guarda os três dados de fila no atributo privado `_portas`; `_portas_da_tarefa` lê `fila_ids`, `verbo`, `fila_cel`, intersecta com portas inteiras das ações exigíveis e publica `rota`, `fila`, `verbos`, `fio` (`app/chamados/services/tarefa_do_dia.py:529-556`). `porta_da_tarefa` retorna somente esses quatro campos quando existe rota (`app/chamados/services/tarefa_do_dia.py:559-565`). B não oferece um equivalente público de `alim` com `vivas`, `colab`, `no_app`, `instancias`; `_perg` não é o conjunto `vivas`, `_cadastro`/`_colabs` não são o mesmo identity map, e as `instancias` públicas de tarefa representam marcos, não instâncias ORM de colaboradores (`app/chamados/services/tarefa_do_dia.py:372-383`, `app/chamados/services/tarefa_do_dia.py:511-534`, `app/chamados/services/tarefa_do_dia.py:184-185`; `app/chamados/juizes.py:1595-1607`).

## 13/11 15:13 · sessao 25297c2a7a00 · BUG-PAR · verificador · origem=Code

**BLOQUEANTE: o delta ainda contém regressão de retorno, divergências tela/porta e caminho que pode regravar competência exportada.** (`app/chamados/services/materializacao.py:171`, `app/chamados/services/materializacao.py:1113`, `app/chamados/services/materializacao.py:686`, `app/ponto/services/fechamento.py:1160`)

Os contraexemplos abaixo são construídos por leitura do código; sua ocorrência em produção, a execução da suíte e a evidência RED/GREEN/mutante permanecem **[não verificado]**. [nao verificado]

1. **BLOQUEANTE — `None` significa tanto “fora do intervalo” quanto “irmã gravada”.**

   Para motivo fora do intervalo, `par_do_intervalo` retorna `(None, None, None)`; `_gravar_irma_do_par` repassa esse último `None`. O chamador interpreta isso como sucesso e afirma que a outra metade gravou. (`app/chamados/services/materializacao.py:171`, `app/chamados/services/materializacao.py:1371`, `app/chamados/services/materializacao.py:1113`)

   **Contraexemplo:** pergunta `orfao_14h`, vizinha do mesmo tipo, sem absolvição por preenchimento de furo, lâmpada acesa. Nenhuma irmã existe ou grava; mesmo assim ocorre `_Reprovada` com “a outra metade do intervalo gravou”, antes do ramo que encerraria pela via `paridade_recusada`. Isso também deixa o item 4 parcialmente descoberto: o texto continua podendo dizer algo que não aconteceu. (`app/chamados/services/materializacao.py:1116`, `app/chamados/services/materializacao.py:1146`, `app/chamados/services/materializacao.py:1170`)

2. **BLOQUEANTE — a função compartilhada ainda não reúne os impedimentos da porta.**

   `estado_aplicacao_pergunta` só consulta `par_do_intervalo` quando encontra conflito de paridade sem absolvição por furo; fora desse ramo retorna `aplicavel`. A porta aplica `DATA_EM_DISPUTA` antes de qualquer ramo de materialização. (`app/chamados/services/materializacao.py:674`, `app/chamados/services/materializacao.py:677`, `app/chamados/services/materializacao.py:686`, `app/chamados/services/materializacao.py:1481`)

   **Contraexemplo:** chão E18:30, S01:00, S06:30; pergunta de volta E02:00 com dia divergente do chamado e `data_conferida_em=None`. Isoladamente a volta alterna corretamente: a tela promete gravação, mas a porta recusa pela data. Portanto, o item 3 não está integralmente curado. (`app/chamados/services/materializacao.py:673`, `app/chamados/services/materializacao.py:686`, `app/chamados/services/materializacao.py:1481`)

   Existe ainda impedimento de competência no escritor que a elegibilidade não consulta: o plantio passa `fato_de_chao=False`; `registrar_batida` recusa fechamento aprovado/exportado. **Contraexemplo:** par que fecha a paridade em competência cujo `FechamentoMensal.status='exportado'`: tela aplicável, clique recusado. (`app/chamados/services/materializacao.py:157`, `app/chamados/services/materializacao.py:1173`, `app/ponto/registro_batida.py:132`, `app/ponto/janelas.py:87`)

3. **BLOQUEANTE — validar pode tocar o gravado da competência 10 exportada, mesmo recusando o plantio.**

   A validação agenda recálculo antes de tentar materializar; o callback chama o escritor mensal, cujo ramo de fechamento não aberto atualiza horas e `atualizado_em`. A guarda temporária cobre somente 09/2026. (`app/chamados/services/validacao.py:543`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:36`)

   **Contraexemplo:** pergunta devolvida pela porta 103, de competência 10 exportada, para colaborador incluído no universo do cálculo. O admin valida; a batida recusa pela competência, mas o recálculo já agendado executa após o commit. Esse caminho não passa pela geração de Pauta DP do lote. Mudança numérica efetiva em produção: **[não verificado]**. (`app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:564`, `app/ponto/registro_batida.py:135`, `app/ponto/services/fechamento.py:1824`, `app/chamados/services/passivo_validado.py:470`)

4. **BLOQUEANTE — a ordem global foi adicionada ao lote, mas continua ausente no laço da disputa.**

   O lote chama `_travar_o_ato` no topo da transação. A primeira fase de `validar_pergunta` também toma a trava canônica, e a porta 103 toma Colaborador → Disputa → Chamado → Pergunta. Entretanto, depois de sair do `atomic`, a validação chama o laço da disputa, que chama `_materializar_uma` sem tomar essa ordem no topo. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/validacao.py:529`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:570`, `app/chamados/services/materializacao.py:1669`)

   **Contraexemplo de deadlock possível:** A entra pelo laço numa pergunta sem marco identificável e grava `pendencia_admin`, adquirindo a linha Pergunta; B entra pelo lote ou porta 103, trava Colaborador e espera essa Pergunta; A prossegue até reconciliar o destino e pede Colaborador. Forma-se P→C / C→P. O `save` individual da pergunta não exige previamente o registro da trava do colaborador. Deadlock observado: **[não verificado]**. (`app/chamados/services/materializacao.py:1080`, `app/chamados/services/materializacao.py:1220`, `app/chamados/models.py:712`, `app/chamados/models.py:2687`, `app/chamados/models.py:2713`, `app/chamados/models.py:3465`)

5. **BLOQUEANTE — o rótulo 103 não chega ao modal no caminho aplicável.**

   O Python coloca “anterior a 18/10” e o dia no detalhe de `aplicavel`, mas o modal imprime esse detalhe somente para `bloqueada_paridade` e `ja_satisfeita`. (`app/chamados/services/materializacao.py:680`, `app/templates/chamados/partials/modal_fio.html:239`)

   **Contraexemplo:** pergunta devolvida com irmã respondida e par válido. O dicionário contém o rótulo; o modal não o apresenta. O histórico não resolve: `respostas_anteriores` ignora `DEVOLUCAO`, pois `ATOS_COM_FOTO` contém apenas reabertura e recusa. O teste novo confere o dicionário, sem renderizar a tela. (`app/chamados/services/materializacao.py:678`, `app/templates/chamados/partials/modal_fio.html:248`, `app/chamados/models.py:3599`, `app/chamados/models.py:3955`, `app/chamados/tests/test_bug_par_projetado.py:512`)

6. **BLOQUEANTE — a trilha da devolução é incompleta e sua foto pode faltar.**

   A porta registra autor/motivo em `DecisaoPergunta` e chama `evento` com antes/depois. Porém, apaga também `materializada_por_id`, omitido das duas fotos. **Contraexemplo:** carimbo antigo assinado pelo usuário 17: a devolução elimina essa assinatura sem registrá-la na foto da porta. O comando guarda esse campo na reversão, mas a porta isolada não oferece a mesma garantia. (`app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:275`, `app/chamados/services/validacao.py:284`, `app/chamados/models.py:3234`, `app/chamados/management/commands/bug_par_103.py:57`)

   **Outro contraexemplo:** falha no INSERT de `LogAuditoria`. `evento` desfaz seu próprio savepoint, engole a falha e retorna `None`; a porta não verifica esse retorno e pode concluir a devolução com `True`. Resta a decisão com motivo/ator, mas sem a foto antes/depois. (`app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:287`)

7. **BLOQUEANTE — a reversão é escrita antes, mas pode ser sobrescrita por outro apply.**

   A ordem normal está correta: fecha o arquivo de reversão antes de chamar a primeira devolução. Entretanto, o nome tem resolução de um segundo e a abertura usa modo `w`. (`app/chamados/management/commands/bug_par_103.py:70`, `app/chamados/management/commands/bug_par_103.py:71`, `app/chamados/management/commands/bug_par_103.py:78`)

   **Contraexemplo:** dois applies no mesmo segundo, inclusive para empresas diferentes, usam o mesmo nome; o segundo trunca a reversão do primeiro. O comando não usa criação exclusiva nem identifica a execução/empresa no nome. Ocorrência real: **[não verificado]**. (`app/chamados/management/commands/bug_par_103.py:19`, `app/chamados/management/commands/bug_par_103.py:70`)

8. **NAO-BLOQ — o item 5 melhora, mas “gravada pela irmã neste ato” continua sendo uma inferência.**

   O lote evita reprocessar a irmã no caso coberto pelo teste, relendo o carimbo. Entretanto, `_pela_irma` significa apenas “estava nulo na instância e agora não está nulo”; não prova autoria, via ou gravação neste ato. (`app/chamados/services/passivo_validado.py:551`, `app/chamados/services/passivo_validado.py:554`, `app/chamados/tests/test_bug_par_projetado.py:368`)

   **Contraexemplo:** outra transação encerra a pergunta por identidade entre a coleta e a trava. O lote conta `plantada` embora esse ato não tenha plantado sua batida. Incidência no raspado: **[não verificado]**. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/passivo_validado.py:553`, `app/chamados/services/passivo_validado.py:555`)

9. **NAO-BLOQ — os selos novos não cobrem todas as garantias anunciadas.**

   O teste de trava verifica C/D/Chamado antes do primeiro INSERT de batida, mas não verifica Pergunta nem os demais caminhos. **Contraexemplo:** manter essas consultas e introduzir uma escrita anterior de Pergunta não é diretamente cobrado pelas asserções. (`app/chamados/tests/test_bug_par_projetado.py:393`, `app/chamados/tests/test_bug_par_projetado.py:399`, `app/chamados/tests/test_bug_par_projetado.py:405`)

   O item 9 ganhou testes dos dois ramos defensivos; o rollback testa `_materializar_uma` com ambas já validadas. **Contraexemplo descoberto pelo teste:** mover a validação da irmã para fora do savepoint de `_validar_o_par` não seria cobrado por esse caso, que não passa por essa validação nem verifica seu desfazimento. Execução e mutante: **[não verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:422`, `app/chamados/tests/test_bug_par_projetado.py:449`, `app/chamados/tests/test_bug_par_projetado.py:454`, `app/chamados/services/validacao.py:450`)

   A allowlist dispensa a função inteira: qualquer leitura indevida adicionada dentro de uma função listada continua excluída pelo selo. Isso inclui o novo julgamento do par e o lote. (`app/chamados/tests/test_contract_leitores_vitalidade.py:108`, `app/chamados/tests/test_contract_leitores_vitalidade.py:121`, `app/chamados/tests/test_contract_leitores_vitalidade.py:341`)

**Situação dos procedentes da triagem, limitada à leitura estática:**

| Item | Resultado |
|---|---|
| 2 | A irmã implausível passou a ser checada pelo helper comum; a equivalência completa tela/porta continua falsa pelo achado 2. (`app/chamados/services/materializacao.py:182`, `app/chamados/services/validacao.py:445`) |
| 3 | Parcial: datas das duas metades entram no helper, mas a tela só o chama no ramo de conflito de paridade. (`app/chamados/services/materializacao.py:188`, `app/chamados/services/materializacao.py:674`) |
| 4 | Remove “colada” e produz falta da metade/distância; há a regressão do achado 1. O aviso traduzido continua genérico quanto à distância. (`app/chamados/catalogo/recusa.py:29`, `app/chamados/services/materializacao.py:1124`, `app/chamados/services/acoes_disputa.py:34`) |
| 5 | Caso comum coberto; ressalva do achado 8. (`app/chamados/services/passivo_validado.py:551`) |
| 6 | Corrigido no topo do lote; não em todos os caminhos de escrita, conforme achado 4. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/materializacao.py:1669`) |
| 9 | Dois testes adicionados; cobertura da validação da irmã incompleta, conforme achado 9. (`app/chamados/tests/test_bug_par_projetado.py:422`, `app/chamados/tests/test_bug_par_projetado.py:437`) |

**BUG-PAR-103: o predicado usa corte exclusivo pelo instante do carimbo**, anterior a `18/10/2026 00:00 America/Sao_Paulo`, equivalente a `03:00 UTC`; não usa o dia do turno. Assim, `18/10 02:59:59 UTC` entra e `03:00 UTC` não entra, satisfeitas as demais condições. (`app/chamados/services/materializacao.py:227`, `app/chamados/services/materializacao.py:255`, `app/chamados/services/materializacao.py:258`, `app/config/settings/base.py:93`)

A lâmpada consultada é a da célula atualmente ligada à pergunta, admitindo `False` e `None`; não é uma fotografia histórica do instante da absolvição. Para intervalo com múltiplas pausas, esse juiz exige todas as lâmpadas correspondentes acesas. A equivalência desse universo com as 103 linhas medidas não pode ser conferida sem os carimbos/células individuais: **[não verificado]**. (`app/chamados/services/materializacao.py:259`, `app/chamados/juizes.py:944`, `app/chamados/juizes.py:960`, `app/chamados/juizes.py:971`)

O contador real calcula `total_aval = universo + len(bug_par_103)` e o comando imprime esse valor; não há 163 fixado. O total efetivamente retornado para a população de produção continua **[não verificado]**. (`app/chamados/services/materializacao.py:320`, `app/chamados/management/commands/bug_par_103.py:61`)

**Escritores e leitores dos estados envolvidos:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `validada_em` / `validada_por` | Validação e devoluções; também preenchimento automático em `carimbar(resposta='validar')` e limpeza por nova resposta. (`app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:201`, `app/chamados/services/validacao.py:270`, `app/chamados/models.py:3160`, `app/chamados/services/disputa_emissao.py:1480`) | Elegibilidade/validação da irmã e seleção do laço. (`app/chamados/services/materializacao.py:180`, `app/chamados/services/validacao.py:451`, `app/chamados/services/materializacao.py:1657`) |
| `materializada_em` / `materializada_por` / carimbo | `_escrever_carimbo`, pelas portas de carimbar/descarimbar; reset em lote. (`app/chamados/models.py:3123`, `app/chamados/models.py:3234`, `app/chamados/models.py:3306`) | Seleção da irmã, predicado 103, contador, tela, prova do par e lote. (`app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:257`, `app/chamados/services/materializacao.py:303`, `app/chamados/services/materializacao.py:654`, `app/chamados/services/validacao.py:454`, `app/chamados/services/passivo_validado.py:552`) |
| `via_resolucao` | Porta conjunta do carimbo; também atualização isolada por fato/relavra. (`app/chamados/models.py:3127`, `app/chamados/models.py:3315`, `app/chamados/services/relavra_via.py:118`) | Predicado 103, tela e explicador. (`app/chamados/services/materializacao.py:257`, `app/chamados/services/materializacao.py:658`, `app/chamados/services/explicador.py:95`) |
| `recusa_motivo` | Materializador, porta de identidade, recusa admin e resposta de escape. (`app/chamados/services/materializacao.py:1079`, `app/chamados/services/materializacao.py:1348`, `app/chamados/models.py:3213`, `app/chamados/services/validacao.py:695`, `app/chamados/services/disputa_emissao.py:1497`) | Classificação do universo, explicador e tradução do aviso. (`app/chamados/services/materializacao.py:241`, `app/chamados/services/explicador.py:90`, `app/chamados/services/acoes_disputa.py:34`) |
| `data_conferida_em` | Conferência da data. (`app/chamados/services/validacao.py:775`) | Helper do par e gate da materialização, em duas conjunções separadas. (`app/chamados/services/materializacao.py:151`, `app/chamados/services/materializacao.py:1481`) |
| `DecisaoPergunta` / motivo 103 | Porta `registrar`, chamada pela devolução. (`app/chamados/models.py:4029`, `app/chamados/services/validacao.py:275`) | Rótulo 103 e histórico, com a lacuna de apresentação do achado 5. (`app/chamados/services/materializacao.py:265`, `app/chamados/models.py:3594`) |

Há dois significados relevantes: `materializada_em` também representa encerramento sem batida; `validada_em` pode resultar de preenchimento automático, além da validação explícita do admin. (`app/chamados/services/materializacao.py:654`, `app/chamados/services/materializacao.py:658`, `app/chamados/models.py:3160`)

Nos caminhos examinados, o plantio continua passando por `registro_batida`, e a hora deriva de `_ts_tipo_do_plantio`; não há segundo escritor/derivador demonstrado nesses trechos. Há relógio interno para o novo `ato_em`, mas ele não decide o corte das 103; divergência de elegibilidade causada por esse relógio é **[não verificado]**. (`app/chamados/services/materializacao.py:195`, `app/chamados/services/materializacao.py:203`, `app/chamados/services/materializacao.py:1172`, `app/chamados/services/validacao.py:276`, `app/chamados/services/materializacao.py:258`)

## 13/11 15:12 · sessao 591bbeaab94c · - · - · origem=Ronald (tmux)

**Cura restrita ao informativo da lavra:** o lavrador captura `timezone.localtime(timezone.now()).date()` uma vez e passa por `montar_linhas(..., hoje=_hoje)` → `_informativo_rescisao(..., hoje=hoje)` → `export.informativo_rescisao(..., hoje=hoje)` → `_dias_sem_decisao(..., hoje)`. Com data fornecida, o fallback `localdate()` não executa. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1163`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`; `app/folha/export.py:1238`)

| Caminho | Quem lê relógio / chega ao informativo? |
|---|---|
| Lavrador | Captura dia local na borda, como acima. (`app/folha/management/commands/lavrar_retrato_folha.py:48`) |
| `montar_linhas` avulso | Default `hoje=None`; helper também aceita `None`; se omitido, cada desligado elegível relê relógio no interior de `_dias_sem_decisao`. Não encontrei outro chamador de produção de `montar_linhas` além do lavrador. (`app/folha/retrato_folha.py:1120`; `app/folha/retrato_folha.py:1200`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`; `app/folha/management/commands/lavrar_retrato_folha.py:65`) |
| Tela / drawer | Não alcança `informativo_rescisao`: lê o JSON persistido. Seu hoje local escolhe competência, sem recomputar informativo. (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:190`; `app/folha/views_retrato.py:196`; `app/colaboradores/views.py:982`; `app/colaboradores/views.py:984`; `app/folha/retrato_folha.py:110`) |
| Haiku `o_que_segura` | Omite `hoje`; o relógio é lido no interior de `export._dias_sem_decisao`, não na borda Haiku. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1228`; `app/folha/export.py:1238`) |
| Vigia | Não alcança `informativo_rescisao`: captura dia local e chama `censo_alcance`, que chama `dias_que_seguram` e a retenção. (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `app/chamados/management/commands/vigiar_fio_morto.py:169`; `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:137`; `app/folha/fora_da_folha.py:98`) |

**NAO-BLOQ no delta, resíduo de relógio interno explicitamente preservado:** a documentação nova diz que Haiku lê relógio “na borda dele”, mas a chamada omite a data e a leitura efetiva ocorre dentro do laço compartilhado. Contraexemplo: um desligado com vínculo, sem célula em 05/10 e demissão posterior; chamada começada em 05/10 às 23:59 que só alcança o fallback após meia-noite passa a incluir 05/10, pelo ramo `d < hoje`. Esse resíduo já está no caminho preexistente, não é cura abrangente de relógio. (`app/folha/retrato_folha.py:1211`; `app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

**Outro limite preexistente:** o `hoje` do lavrador só foi injetado no informativo; a fonte chamada antes dele ainda não o recebe. O vigia também não injeta seu hoje em `dias_que_seguram`. Portanto “uma leitura de relógio na rodada” não vale para toda a cadeia de retenção. Contraexemplo equivalente ao acima para um ativo faz a fonte variar ao atravessar meia-noite. (`app/folha/retrato_folha.py:1157`; `app/folha/retrato_folha.py:1163`; `app/chamados/services/vigia_fio.py:566`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:98`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

**Fuso:** os leitores identificados usam dia civil local por `localtime(...).date()` ou `localdate()`, e a configuração da cópia é `America/Sao_Paulo`, `USE_TZ=True`. Não há divergência UTC demonstrada nesses caminhos do informativo; `timezone.now()` seguido de conversão local não produz dia UTC. Configuração efetiva produtiva e eventual ativação de outro fuso durante execução: **[nao verificado]**. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/export.py:1238`; `app/chamados/management/commands/vigiar_fio_morto.py:169`)

## 13/11 15:12 · sessao a2457deca3e5 · - · - · origem=Ronald (tmux)

- **NAO-BLOQ — inscrição de bolha descreve entradas/saídas de forma incompleta.** `@entra ... para o juiz, so o colaborador` só descreve `esta_desligado`; `julgar_desligamento` também lê POST e relógio interno. `@sai` não distingue executor agendado do imediato. Contraexemplo: agendar amanhã grava `data_demissao`/motivo agora, retorna sem encerrar vínculo e sem abrir chamado DP. (`app/colaboradores/services/desligamento.py:7`; `app/colaboradores/services/desligamento.py:35`; `app/colaboradores/services/desligamento.py:40`; `app/colaboradores/services/desligamento.py:60`; `app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:126`)

- **Não comprovo segundo escritor do valor `desligado`.** O cron agendado julga e chama o mesmo executor; a atribuição produtiva literal identificada ocorre no executor, que persiste pela porta canônica. A frase “agendado só grava na data” é correta apenas para `situacao='desligado'`; é falsa para `data_demissao`, escrita antecipadamente. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; `app/colaboradores/services/desligamento.py:11`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/colaborador.py:73`)

- **`situacao` tem outros escritores legítimos; “um escritor” não vale para o campo inteiro.** Reativação atribui `ativo`, limpa data/motivo e persiste pela mesma porta. `data_demissao` significa data planejada enquanto ativo e data de desligamento quando consumado; o cron lê conjuntamente data e situação para distinguir os estados. (`app/colaboradores/services/desligamento.py:235`; `app/colaboradores/services/desligamento.py:239`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`)

- **NAO-BLOQ — documentação nova erra a segunda consulta do informativo.** Diz “células + férias/ausências”, mas carrega células e vínculos. Contraexemplo: mesmo sem férias/ausências, cada desligado elegível consulta `CelulaDia` e `EscalaColaborador`. (`app/folha/retrato_folha.py:1210`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)

- **O selo de mapa garante presença das etiquetas, não a verdade do contrato.** Um cabeçalho completo com entradas/saídas falsas continua satisfazendo os testes: eles consultam ausência de inscrição e estrutura do parser. (`app/core/tests/test_selo_mapa_contrato.py:41`; `app/core/tests/test_selo_mapa_contrato.py:60`)

Execução de testes, mutantes e ocorrência produtiva: **[nao verificado]**. [nao verificado]

## 13/11 15:12 · sessao 3777adbae6fd · HJ-093 · sonda · origem=Code

Transcrição literal do anexo; `[não verificado]` aplica-se a todas as linhas abaixo quanto à confirmação independente em produção.

```text
JUST 3331 {'id': '3331', 'batida_entrada': '139122', 'data_turno': '2026-10-27', 'tipo': 'esqueci_saida', 'saida_informada': 'None', 'latitude': 'None', 'longitude': 'None', 'status': 'aprovada', 'aprovada_por': '657', 'aprovada_em': '2026-10-28 11:31:15.607936+00:00', 'motivo_rejeicao': '', 'batida_gerada': 'None', 'chamado_origem': '35329', 'criado_em': '2026-10-27 22:20:18.830771+00:00', 'atualizado_em': '2026-10-28 11:31:15.608025+00:00'}
FROTA2 2026-11 com_justificativa 33 realizado0_COM_batida_casada 9 colabs 9 realizado0_SEM_batida 2
FROTA2 2026-10 com_justificativa 134 realizado0_COM_batida_casada 23 colabs 21 realizado0_SEM_batida 5
FROTA2 2026-09 com_justificativa 16 realizado0_COM_batida_casada 3 colabs 2 realizado0_SEM_batida 0
FROTA2 2026-08 com_justificativa 24 realizado0_COM_batida_casada 10 colabs 5 realizado0_SEM_batida 0
CASOS 2026-11 [(140889, 56, '2026-10-25', 'disp', 1, 'luz', 1, 'prev', 720), (127808, 976, '2026-10-26', 'disp', 3, 'luz', 1, 'prev', 660), (135337, 496, '2026-10-22', 'disp', 1, 'luz', 1, 'prev', 720), (140580, 750, '2026-10-26', 'disp', 1, 'luz', 1, 'prev', 720), (141802, 996, '2026-10-26', 'disp', 3, 'luz', 1, 'prev', 440), (131421, 912, '2026-10-23', 'disp', 3, 'luz', 1, 'prev', 660), (121745, 962, '2026-10-25', 'disp', 2, 'luz', 2, 'prev', 440), (129875, 932, '2026-10-27', 'disp', 1, 'luz', 1, 'prev', 720), (143004, 1004, '2026-10-27', 'disp', 3, 'luz', 1, 'prev', 660)]
CASOS 2026-10 [(118689, 56, '2026-10-07', 'disp', 1, 'luz', 1, 'prev', 720), (112074, 567, '2026-10-12', 'disp', 3, 'luz', 1, 'prev', 660), (114134, 206, '2026-09-22', 'disp', 3, 'luz', 1, 'prev', 480), (110059, 341, '2026-10-07', 'disp', 1, 'luz', 1, 'prev', 720), (108830, 340, '2026-10-08', 'disp', 3, 'luz', 1, 'prev', 660), (113955, 438, '2026-09-23', 'disp', 3, 'luz', 1, 'prev', 660), (112352, 347, '2026-09-30', 'disp', 1, 'luz', 1, 'prev', 720), (112358, 347, '2026-10-06', 'disp', 1, 'luz', 1, 'prev', 720), (106510, 922, '2026-09-28', 'disp', 1, 'luz', 1, 'prev', 480), (106526, 922, '2026-10-14', 'disp', 1, 'luz', 1, 'prev', 480), (110352, 389, '2026-09-30', 'disp', 1, 'luz', 1, 'prev', 720), (105323, 789, '2026-10-11', 'disp', 3, 'luz', 1, 'prev', 440), (105365, 938, '2026-09-23', 'disp', 1, 'luz', 1, 'prev', 720), (33812, 564, '2026-09-24', 'disp', 3, 'luz', 1, 'prev', 480), (116944, 358, '2026-10-02', 'disp', 3, 'luz', 1, 'prev', 480), (105667, 931, '2026-09-25', 'disp', 1, 'luz', 1, 'prev', 480), (33879, 310, '2026-09-21', 'disp', 3, 'luz', 1, 'prev', 440), (104743, 228, '2026-10-01', 'disp', 1, 'luz', 1, 'prev', 720), (113727, 468, '2026-10-05', 'disp', 3, 'luz', 1, 'prev', 440), (116515, 829, '2026-09-23', 'disp', 3, 'luz', 1, 'prev', 440), (106125, 331, '2026-10-03', 'disp', 3, 'luz', 1, 'prev', 660), (118347, 643, '2026-09-25', 'disp', 1, 'luz', 1, 'prev', 720), (111910, 396, '2026-09-28', 'disp', 1, 'luz', 1, 'prev', 720)]
CASOS 2026-09 [(24651, 697, '2026-09-01', 'disp', 3, 'luz', 1, 'prev', 540), (24660, 697, '2026-09-10', 'disp', 3, 'luz', 1, 'prev', 540), (21683, 228, '2026-09-14', 'disp', 1, 'luz', 1, 'prev', 720)]
CASOS 2026-08 [(3283, 408, '2026-08-17', 'disp', 3, 'luz', 1, 'prev', 660), (2261, 855, '2026-08-18', 'disp', 3, 'luz', 1, 'prev', 660), (8127, 518, '2026-07-25', 'disp', 3, 'luz', 1, 'prev', 440), (8131, 518, '2026-07-29', 'disp', 3, 'luz', 1, 'prev', 440), (8148, 518, '2026-08-15', 'disp', 3, 'luz', 1, 'prev', 440), (8150, 518, '2026-08-17', 'disp', 3, 'luz', 1, 'prev', 440), (16234, 510, '2026-08-10', 'disp', 3, 'luz', 1, 'prev', 420), (1013, 863, '2026-08-10', 'disp', 3, 'luz', 1, 'prev', 660), (1021, 863, '2026-08-18', 'disp', 3, 'luz', 1, 'prev', 660), (1023, 863, '2026-08-20', 'disp', 3, 'luz', 1, 'prev', 660)]
FIM
```

Sim: o trecho fornecido termina em `FIM`, sem erro exibido. [não verificado]

`realizado0_COM_batida_casada`: realizado zero com alguma lâmpada `luz != None`, indicando batida real casada a marco, conforme a definição fornecida. [não verificado]  
`realizado0_SEM_batida`: realizado zero no dia inteiro justificado/dispensado, sem batida; a dispensa registra `luz=None`. [não verificado]; [cartorio.py:786](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/services/cartorio.py:786)  
`acesa=True` sozinha não distingue os grupos: a dispensa também acende a lâmpada, mantendo `luz=None` e registrando `dispensada`. [cartorio.py:807](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/services/cartorio.py:807)

A JUST `3331` traz tipo `esqueci_saida`, status `aprovada` e turno `2026-10-27`. [não verificado] O tipo significa “Esqueci de bater saída”. [models.py:119](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:119)

Ela nomeia `batida_entrada=139122`; traz `saida_informada=None` e `batida_gerada=None`, sem ID ou horário explícito de marco no recorte. [não verificado] No modelo, esses campos representam, respectivamente, a batida de entrada do turno, a saída declarada e a batida de saída criada após aprovação. [models.py:139](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:139), [models.py:149](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:149), [models.py:167](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:167)

## 13/11 15:12 · sessao ee8f74e6c5d5 · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — RED não prova ausência de relógio interno.** O teste observa somente os argumentos registrados pelo spy em `informativo_rescisao`: exige `hoje=06/10`; não verifica leitura do relógio nem resultado sensível à data. (`app/folha/tests/test_lei_desligado_nao_trava.py:130`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`) **Contraexemplo:** trocar `hoje = hoje or _tz.localdate()` por `hoje = _tz.localdate()` preserva os argumentos vistos pelo spy e restaura o relógio interno. A fixture contém células `nunca_bateu`, cujo ramo não consulta `hoje`; esse teste continuaria passando por inspeção do fluxo. Execução do mutante: **[nao verificado]**. (`app/folha/export.py:1238`; `app/folha/tests/test_lei_desligado_nao_trava.py:37`; `app/folha/export.py:1273`; `app/folha/export.py:1294`)

- **D1: a expectativa nova tem mordida para o defeito caracterizado.** O caso gera células quando ativo, encerra o vínculo por UPDATE e grava `situacao='desligado'`, conservando célula de trabalho. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:756`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:759`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:762`) A medição recalcula o fechamento real e lê `fm.motivos_espelho`; `retencao` integra as chaves efetivamente comparadas, portanto não ficou numa expectativa ignorada. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:604`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:644`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:675`) O escritor chama o wrapper do retentor, que delega a `motivos_retencao_celula`; restaurar `['sem_celula']` nesse fechamento diverge dos novos `[]`, nas duas configurações de HE. Execução nesta revisão: **[nao verificado]**. (`app/ponto/services/fechamento.py:1066`; `app/ponto/services/fechamento.py:1067`; `app/folha/export.py:1307`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:329`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:361`)

- **NAO-BLOQ — documentação nova atribui consultas à fonte errada.** O helper documenta “células + férias/ausências”, mas o laço consulta células e carrega vínculos por `escalas_do_periodo`. **Contraexemplo:** desligado elegível sem férias/ausências ainda percorre essas duas consultas. (`app/folha/retrato_folha.py:1209`; `app/folha/retrato_folha.py:1210`; `app/folha/export.py:1247`; `app/folha/export.py:1252`; `app/folha/export.py:1253`)

- **NAO-BLOQ — cabeçalho da bateria ficou contraditório.** Ainda afirma que nenhum número de `MEDIDO` foi ajustado, enquanto o delta altera as duas retenções de D1 para `[]`. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:11`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:13`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:329`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:361`)

## 13/11 15:10 · sessao 193cd2f6ba0f · - · - · origem=Ronald (tmux)

**BLOQUEANTE — o laço da disputa continua sem trava no topo.** `validar_pergunta` encerra seu `atomic` e chama o laço depois; o laço chama `_materializar_uma` sem `trava_na_ordem` (`app/chamados/services/validacao.py:568`, `:570`; `app/chamados/services/materializacao.py:1663`, `:1669`). Existem entradas diretas por `fio.py:708` e `acoes_disputa.py:141`.

Contraexemplo: pergunta validada, sem carimbo, sem marco identificável. A regularização abre o atomic e grava `pendencia_admin` na Pergunta antes de gravar a batida (`materializacao.py:1508`, `:1075`, `:1080`, `:1173`). O signal da batida chama o reconciliador, que pode pedir a trava do Colaborador (`app/ponto/signals.py:206`, `:215`; `app/chamados/reconciliador.py:1433`, `:594`). Em paralelo, o lote trava Colaborador e espera essa Pergunta (`passivo_validado.py:379`, `:440`; `models.py:2687`, `:2713`): ciclo P→C / C→P possível. Ocorrência em produção: [não verificado].

A guarda `exigir_colaboradores_travados` **não protege esse save individual**: `PerguntaDisputa.save` só verifica o par resposta/carimbo e chama `super().save` (`models.py:3465`, `:3472`, `:3476`); a exigência em `models.py:3301` pertence ao descarimbo em lote.

**BLOQUEANTE — retorno `None` confunde “fora do intervalo” com “irmã gravada”.** `par_do_intervalo` retorna `(None,None,None)` para motivo fora de intervalo (`materializacao.py:172`). `_gravar_irma_do_par` devolve esse `None` (`:1371`, `:1373`), e o chamador entende sucesso, refaz a guarda e levanta “a outra metade [...] gravou” (`:1111`, `:1118`).

Contraexemplo: `orfao_14h`, vizinha do mesmo tipo, plantio que não preenche furo, lâmpada acesa. Agora levanta `_Reprovada` antes do ramo que carimbaria `VIA_PARIDADE_RECUSADA` (`:1146`, `:1170`), embora não exista irmã nem gravação dela.

**NAO-BLOQ — item 5 curado no caso comum, mas “pela irmã NESTE ato” é inferência excessiva.** O lote relê o carimbo e pula a irmã que chegou sem carimbo e agora tem algum (`passivo_validado.py:551`, `:554`). Entretanto qualquer carimbo surgido entre a coleta e a trava satisfaz `_pela_irma`; não verifica via, lastro ou autoria do ato (`:327`, `:379`, `:553`).

Contraexemplo: outra transação absolve a pergunta por identidade antes da trava do lote; o lote conta “plantada” sem batida nova e pode acionar regeneração da exportada (`:555`, `:461`, `:470`). Ocorrência no dado: [não verificado].

**NAO-BLOQ — o selo do item 6 não prova a ordem inteira.** O teste procura Colaborador/Disputa/Chamado antes do primeiro INSERT, mas não verifica Pergunta nem os caminhos do laço fora do lote (`test_bug_par_projetado.py:393`, `:399`, `:405`). Mutante que mantém essas três consultas e introduz uma escrita anterior de Pergunta permanece fora das asserções desse teste. [nao verificado]

**NAO-BLOQ — item 9 tem testes novos, execução e RED/GREEN não verificados.** Há teste de ordem invertida (`test_bug_par_projetado.py:422`) e teste de rollback depois da irmã (`:437`). Este último chama `_materializar_uma`, verifica batidas, `materializada_em` e via, mas não passa pela validação da irmã em `_validar_o_par` nem verifica reversão de `validada_em` (`:454`, `:457`; `validacao.py:451`, `:455`). Um mutante que deixasse a validação da irmã fora do savepoint de `_validar_o_par` não seria cobrado por esse teste. [nao verificado]

## 13/11 15:10 · sessao 0805ee4d3532 · - · - · origem=Ronald (tmux)

Achados demonstráveis:

- **BLOQUEANTE — trilha antes/depois pode faltar após devolução efetiva.** `devolver_paridade_bug_par_103` limpa os campos e registra `DecisaoPergunta`, depois chama `evento` sem conferir retorno (`app/chamados/services/validacao.py:267-286`). `evento` engole falhas em seu próprio savepoint (`app/core/observ.py:105`, `app/core/observ.py:133-139`). **Contraexemplo:** falha no INSERT de `LogAuditoria`; a devolução persiste e retorna `True`, mas não existe a foto antes/depois exigida. A linha `DecisaoPergunta` contém autor, texto e motivo, sem essa foto (`app/chamados/services/validacao.py:275-278`).

- **BLOQUEANTE — trilha incompleta do carimbo apagado.** A porta apaga também `materializada_por_id` (`app/chamados/models.py:3234-3236`), mas a foto antes/depois não contém esse campo (`app/chamados/services/validacao.py:263-285`). **Contraexemplo:** carimbo antigo assinado pelo usuário 17; após devolver, o banco perde essa assinatura e o registro de auditoria não informa quem assinava. O arquivo de reversão do comando inclui o campo (`app/chamados/management/commands/bug_par_103.py:56-58`); a porta isolada não garante isso.

- **NÃO-BLOQ — rótulo 103 não cobre todos os resultados da tela.** A testemunha retorna `invalida` antes de calcular o rótulo e retorna `ja_satisfeita` sem usá-lo (`app/chamados/services/materializacao.py:664-672`). **Contraexemplo:** pergunta devolvida, seguida de batida aprovada no horário antes de renderizar; aparece “batida já registrada”, sem “anterior a 18/10” e dia.

Censo dos campos:

| Campo | Escritor | Leitores relevantes do delta |
|---|---|---|
| `validada_em` / `validada_por` | Validação explícita: `app/chamados/services/validacao.py:136-143`; devoluções: `:201-204`, `:270-272`; simetria automática: `app/chamados/models.py:3160-3163`; resposta nova: `app/chamados/services/disputa_emissao.py:1480-1482`; correção: `:1595` | Elegibilidade da irmã: `app/chamados/services/materializacao.py:180-185`; porta do par: `app/chamados/services/validacao.py:451-455` |
| `materializada_em` / `materializada_por` / `via_resolucao` | Atribuição conjunta: `app/chamados/models.py:3123-3127`; portas: `:3155`, `:3208-3216`, `:3234-3238`; lote: `:3306-3308` | Irmã: `app/chamados/services/materializacao.py:136-139`; predicado 103: `:255-259`; contador: `:303-316`; tela: `:654-662`; lote: `app/chamados/services/passivo_validado.py:551-553` |
| `via_resolucao`, isoladamente | `app/chamados/models.py:3315-3318`; relavra de vias derivadas: `app/chamados/services/relavra_via.py:118-128` | Explicador: `app/chamados/services/explicador.py:95`; demais leituras acima |
| `recusa_motivo` | Materializador: `app/chamados/services/materializacao.py:1079-1080`, `:1335-1349`; recusa admin: `app/chamados/services/validacao.py:695-701`; resposta de escape: `app/chamados/services/disputa_emissao.py:1497-1499`; identidade/deduplicação: `app/chamados/models.py:3213-3216`, `app/chamados/services/fio.py:1399` | Classificação do universo: `app/chamados/services/materializacao.py:241-244`; explicador: `app/chamados/services/explicador.py:90`; tradução de aviso: `app/chamados/services/acoes_disputa.py:32-41` |
| `DecisaoPergunta` | Porta `registrar`: `app/chamados/models.py:4029-4041` | Rótulo 103: `app/chamados/services/materializacao.py:265-269`; preservação da projeção anterior: `app/chamados/models.py:4000-4025` |

Há campos com dois significados: `materializada_em` também representa encerramento sem criar batida, explicitamente discriminado pela tela (`app/chamados/services/materializacao.py:654-661`); `validada_em` representa validação explícita e preenchimento automático por simetria (`app/chamados/services/validacao.py:136-143`; `app/chamados/models.py:3160-3163`). A ocorrência desses estados na população raspada além do informado é **[não verificado]**.

A allowlist permite a função inteira: o selo exclui qualquer ofensor cujo par `(arquivo, função)` esteja listado, sem conferir o propósito de cada leitura (`app/chamados/tests/test_contract_leitores_vitalidade.py:108-124`, `:339-343`). Portanto, acrescentar uma decisão indevida por carimbo dentro dessas funções continuaria autorizado por esse selo; execução/mutante não foi realizado **[não verificado]**.

A casca usa o resultado da porta e não escreve esses campos (`app/chamados/services/acoes_disputa.py:18-64`); o explicador consulta a testemunha (`app/chamados/services/explicador.py:99-102`) e o desenlace lê `gravou=False` (`app/chamados/services/desenlace.py:63-73`).

## 13/11 15:10 · sessao af9660bd35d2 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — exportada 10: validar pode reescrever o gravado, mesmo quando o plantio recusa.** A validação agenda `recalcular_por_evento` antes da materialização (`app/chamados/services/validacao.py:543`); o callback chama o escritor mensal (`app/ponto/services/fechamento.py:1807`), que atualiza inclusive fechamentos aprovados/exportados (`app/ponto/services/fechamento.py:1160`). A guarda temporária contém somente 09/2026 (`app/ponto/services/fechamento.py:36`). **Contraexemplo:** pergunta elegível de competência 10 exportada, colaborador pertencente ao universo mensal: o clique agenda o recálculo, embora a criação da batida recuse pelo fechamento (`app/ponto/registro_batida.py:132`; `app/ponto/janelas.py:87`). Alteração efetiva dos valores em produção: **[não verificado]**.

2. **BLOQUEANTE — tela e porta continuam divergentes para competência fechada e DATA_EM_DISPUTA.** A elegibilidade do par não consulta `competencia_fechada`, enquanto o escritor consulta (`app/chamados/services/materializacao.py:157`; `app/ponto/registro_batida.py:132`). **Contraexemplo:** par golden numa competência com `FechamentoMensal.status='exportado'`: tela anuncia par aplicável; porta recusa criar a primeira batida (`app/chamados/services/materializacao.py:677`; `app/ponto/janelas.py:87`). Para DATA_EM_DISPUTA, a tela só pergunta ao novo juiz quando existe conflito de paridade sem absolvição por furo (`app/chamados/services/materializacao.py:673`), mas a porta aplica o impedimento incondicionalmente (`app/chamados/services/materializacao.py:1479`). **Contraexemplo:** chão E18:30, S01:00, S06:30; volta E02:00 com dia divergente e `data_conferida_em=None`: tela retorna `aplicavel`, porta recusa (`app/chamados/services/materializacao.py:686`; `app/chamados/services/materializacao.py:1481`).

3. **BLOQUEANTE — rótulo BUG-PAR-103 desaparece no caminho aplicável da tela.** O código concatena “anterior a 18/10” ao detalhe de `aplicavel` (`app/chamados/services/materializacao.py:680`), mas o modal só imprime esse detalhe para `bloqueada_paridade` e `ja_satisfeita` (`app/templates/chamados/partials/modal_fio.html:239`). **Contraexemplo:** pergunta devolvida pela porta 103 cuja irmã está respondida e cujo par fecha: o Python produz o rótulo, mas o modal não o apresenta (`app/chamados/services/materializacao.py:678`; `app/templates/chamados/partials/modal_fio.html:248`). O histórico também não salva a apresentação: a porta registra `DEVOLUCAO` com motivo e dia (`app/chamados/services/validacao.py:275`), porém `respostas_anteriores` só inclui respostas superadas e `ATOS_COM_FOTO` (`app/chamados/models.py:3595`), cujo conjunto contém apenas REABERTURA e RECUSA (`app/chamados/models.py:3955`). O teste verifica o dicionário da função, sem renderizar o modal (`app/chamados/tests/test_bug_par_projetado.py:512`).

A leitura confirma que o plantio novo passa por `registrar_batida`; não encontrei segundo escritor de Batida nos caminhos examinados (`app/chamados/services/materializacao.py:1172`; `app/chamados/services/materializacao.py:1193`). Execução de testes e incidência concreta desses contraexemplos em produção: **[não verificado]**.

## 13/11 14:48 · sessao 3f776bdfcbc5 · HJ-092 · sonda · origem=Code

| Pergunta pk | colNNN | Dia | Motivo | Resposta | t1 | t2 | Marco da grade (tipo, hora) | Batida plantada (pk, tipo, hora) | Outro marco preenchido | Referência |
|---|---|---|---|---|---|---|---|---|---|---|
| 22021 | col392 | 2026-07-26 | saida_sem_entrada | 09:00 | E | S | S, 10:00 | 96760, E, 26/07 09:00 | False | [nao verificado] |
| 22626 | col489 | 2026-08-30 | saida_sem_entrada | 08:00 | E | S | S, 07:00 | 87582, S, 31/08 08:00 | False | [nao verificado] |
| 25140 | col518 | 2026-09-07 | intervalo_saida | 12:00 | S | E | E, 12:05 | 96572, E, 07/09 06:31 | False | [nao verificado] |
| 26279 | col107 | 2026-09-13 | saida_sem_entrada | 12:50 | E | S | E, 12:50 | 103763, E, 13/09 12:50 | True | [nao verificado] |
| 27212 | col438 | 2026-09-04 | intervalo_saida | 12:00 | S | E | E, 12:00 | 112546, S, 04/09 12:00 | False | [nao verificado] |
| 27223 | col450 | 2026-09-13 | orfao_14h | 01:00 | S | E | E, 23:00 | 99634, S, 14/09 01:02 | False | [nao verificado] |
| 27227 | col451 | 2026-09-14 | orfao_14h | 18:55 | S | E | E, 19:00 | 110327, S, 14/09 18:55 | False | [nao verificado] |
| 27289 | col499 | 2026-09-15 | saida_sem_entrada | 06:00 | E | S | S, 06:00 | 103792, E, 16/09 06:00 | False | [nao verificado] |
| 27739 | col334 | 2026-08-27 | intervalo_saida | 03:00 | S | E | E, 03:00 | 112409, E, 27/08 19:00 | False | [nao verificado] |
| 34263 | col152 | 2026-09-21 | intervalo_saida | 12:00 | S | E | S, 12:00 | 108960, S, 21/09 14:40 | True | [nao verificado] |
| 39767 | col334 | 2026-10-04 | orfao_14h | 19:00 | S | E | E, 19:00 | 118509, S, 05/10 06:55 | True | [nao verificado] |
| 40729 | col821 | 2026-10-06 | saida_sem_entrada | 19:00 | E | S | S, 19:00 | 127012, E, 06/10 19:00 | False | [nao verificado] |
| 44524 | col993 | 2026-10-15 | intervalo_saida | 12:00 | S | E | E, 12:12 | 144929, E, 15/10 12:00 | True | [nao verificado] |
| 46755 | col996 | 2026-10-21 | saida_sem_entrada | 12:55 | E | S | S, 13:00 | 144654, E, 21/10 12:55 | False | [nao verificado] |
| 56625 | col909 | 2026-11-11 | saida_sem_entrada | 11:00 | E | S | S, 12:00 | 151895, E, 11/11 07:00 | False | [nao verificado] |

Total de linhas `T2`: **15** (esperado: 15). [nao verificado]

A saída termina em `FIM_T2`: **sim**; registra `ERROS 0`: **sim**. [nao verificado]

## 13/11 14:47 · sessao c5530b6ed786 · HJ-093 · sonda · origem=Code

**(a) O zero é compatível com uma saída sem entrada para formar par.** A ata apenas copia `dia['minutos_realizados']`; quem calcula é `ponto.turnos.realizado_do_dia`, chamado pela grade. (`app/ponto/services/cartorio.py:738–740`; `app/escala/utils.py:1879`, `2045–2058`; `app/ponto/turnos.py:774–795`.)

As referências são da cópia fornecida. A correspondência ao HEAD/hash declarado é **[nao verificado]**: não há metadados Git disponíveis para conferir.

Na saída anexada, ATA e GRADE registram `minutos_realizados=0`, três marcos dispensados por justificativa, sem `luz`, e a saída das 18:59. A execução da sonda não foi reproduzida **[nao verificado]**.

Aplicando o código **exclusivamente à batida listada**, o resultado é:

```text
pares E→S completos: []
registro do pareador: (entrada=None, saída=BAT139122 às 18:59:53)
```

O pareador começa sem entrada corrente e, recebendo essa S, cria o registro com `entrada=None`. `_fim_fechado` devolve `None` para ele; o somador o ignora e mantém total zero. **Não é, nesse cenário, o ramo `sem_turno=True`: existe um registro, mas nenhum trecho fechado que some tempo.** (`app/ponto/turnos.py:1222–1223`, `1495–1496`, `609–612`, `733–739`, `771`.)

Os pares da execução histórica são **[nao verificado]**: o anexo não imprime esses pares nem todas as batidas vizinhas que o carregador consulta com margem de datas. (`app/ponto/turnos.py:1862–1866`.)

**(b) A causa demonstrável desse realizado zero é a ausência de par; ele não comprova a classe JUSTIFICATIVA-PAGA-O-PERIODO.** Essa classe trata de quem remunera o período justificado, enquanto o realizado soma tempo entre batidas pareadas. (`app/docs/BACKLOG.md:1061`; `app/ponto/turnos.py:710–718`, `733–771`.)

Nesta cópia, a justificativa aprovada marca o dia como `justificado`; depois, o cartório acende as lâmpadas faltantes com `luz=None` e `dispensada='justificativa'`. Essa operação não acrescenta minutos nem batidas ao pareador. O mapa de papéis exige luz real (`instante_luz`), portanto dispensa sem luz não fornece a entrada ausente. (`app/escala/utils.py:1197–1199`, `1254–1259`; `app/ponto/services/cartorio.py:802–812`; `app/ponto/turnos.py:1638`.)

A condição financeira citada no backlog existe: `min_parcial` acumula Ausências pagas parciais; Justificativas aprovadas entram no conjunto de datas justificadas. No motor, o abono considera dias obrigatórios justificados **sem trabalho reconhecido**. As linhas atuais desse último trecho são **2606–2622**, não as antigas 2539–2556 citadas no backlog. (`app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/motor_calculo_v2.py:2606–2622`.)

O tipo, período e demais campos da justificativa específica são **[nao verificado]**, pois a consulta `just` terminou em `FieldError`. Sua classificação financeira nessa obra também é **[nao verificado]**.

**(c) Transcrição literal das linhas FROTA fornecidas; consulta e execução não reproduzidas [nao verificado]:**

```text
FROTA 2026-11 com_justificativa 33 realizado_0 11 realizado_0_com_lampada_acesa 11 colabs 11
FROTA 2026-10 com_justificativa 134 realizado_0 28 realizado_0_com_lampada_acesa 28 colabs 26
FROTA 2026-09 com_justificativa 16 realizado_0 3 realizado_0_com_lampada_acesa 3 colabs 2
FROTA 2026-08 com_justificativa 24 realizado_0 10 realizado_0_com_lampada_acesa 10 colabs 5
```

Pelos rótulos e listas CASOS, a leitura é: células com justificativa; subconjunto com realizado zero; subconjunto desse zero com lâmpada acesa; colaboradores distintos dos casos. O predicado exato de cada contador é **[nao verificado]**, porque o código da sonda não foi anexado.

O universo informado é restrito a células com ata e lâmpadas, desde 21/06; o filtro efetivamente executado é **[nao verificado]**. Esses números não demonstram quantidade de descontos, horas devidas ou casos da classe do backlog **[nao verificado]**. Tampouco “acesa” significa necessariamente batida: uma dispensa também acende a lâmpada sem luz. (`app/ponto/services/cartorio.py:807–810`.)

São competências, não necessariamente meses civis: com corte 21, novembro abrange 21/10–20/11. O corte efetivo usado pela sonda é **[nao verificado]**. (`app/ponto/janelas.py:10–21`.)

**(d) Existe caminho até os campos de realizado da folha; desconto efetivo não está demonstrado.**

| Consumidor | Caminho no código |
|---|---|
| Grade da ata | Copia `ata['minutos_realizados']`. `app/escala/services/leitor_celula.py:423`. |
| Fechamento mensal | Carrega essa grade, aplica limite diário ao previsto e soma em `FechamentoMensal.minutos_realizados`. `app/folha/export.py:335–338`; `app/ponto/services/dia_pago.py:393–401`; `app/ponto/services/fechamento.py:1061–1064`. |
| DiaPago | Recebe o mapa diário e incorpora `minutos_realizados`. `app/ponto/services/fechamento.py:1212–1213`; `app/ponto/services/dia_pago.py:288–292`. |
| Espelho | Lê a grade da célula e usa o realizado da ata no diagnóstico `_ata_sem_turno`. O realizado cru é recalculado pelo juiz. `app/ponto/services/espelho.py:519`, `891–894`, `675–676`, `783`. |
| Linha paga / PDF | Usa `DiaPago`, `pago_h` e rubricas. `app/ponto/services/espelho.py:406–410`, `811–813`; `app/relatorios/pdf_espelho.py:902`, `981`. |
| Conferência da exportação | Soma diretamente minutos positivos da ata; declara não alimentar o TXT. `app/folha/validacao_pdf.py:38`, `44–54`. |

O TXT emite rubricas de outros campos do fechamento e faltas pela fonte de ausências que descontam; não transforma esse zero diretamente em rubrica de desconto. (`app/folha/export.py:439–452`, `730–744`, `179–185`.)

**Que o fechamento gravado incorporou esse zero, quanto foi pago/descontado e o resultado final de espelho, PDF, DSR ou TXT: [nao verificado].** O anexo mostra os fechamentos abertos, mas não esses valores.

## 13/11 14:46 · sessao 568fd7bfba34 · - · - · origem=Ronald (tmux)

Cadeia concreta para o item (d), na cópia:

- `app/escala/services/leitor_celula.py:423`: `grade_da_celula` copia `int(ata.get('minutos_realizados') or 0)` para o dia da grade.
- `app/folha/export.py:335` e `:338`: `grade_do_fechamento` chama esse leitor.
- `app/ponto/services/fechamento.py:1013` e `:1014`: recalcular fechamento carrega essa grade.
- `app/ponto/services/dia_pago.py:394`: limita o realizado de cada dia ao previsto. `:395-396` só concede abono para `tipo_dia == 'ausencia'`; `:400-401` inclui realizado dos dias de trabalho.
- `app/ponto/services/fechamento.py:1061-1064`: usa essa derivação e soma em `FechamentoMensal.minutos_realizados`.
- `app/ponto/services/fechamento.py:1137-1144`: grava fechamento aberto; `:1185` inclui minutos realizados no update de outros status.
- `app/ponto/services/fechamento.py:1213`: passa o mapa ao lavrador de `DiaPago`; `app/ponto/services/dia_pago.py:288-292` grava seu `minutos_realizados`.

Portanto, **o 0 tem caminho de código até os campos de realizado da folha**; a saída fornecida só mostra status dos fechamentos, sem seus valores. **Que o fechamento gravado do col1004 já incorporou esse 0: [não verificado].**

**Não concluir desconto/valor pago a partir do 0:** as rubricas de exportação são extraídas de horas extras, noturnas, intra, atraso e saída antecipada em `app/folha/export.py:439-452`; o TXT lê os fechamentos gravados em `:719-739` e faltas por `datas_falta` em `:740-744`. Não usa `minutos_realizados` como rubrica direta nesses trechos.

No espelho:

- `app/ponto/services/espelho.py:519-526` lê a grade da célula.
- `:891-894` consome `minutos_realizados` da ata para o aviso `_ata_sem_turno`.
- O campo cru do espelho em `:783` vem do juiz chamado em `:675-676`, não de cópia direta do campo da ata.
- A linha de dinheiro usa `DiaPago`: consulta em `:406-410`; `pago_h` e `rubricas` em `:811-813`.
- O PDF lê `pago_h` em `app/relatorios/pdf_espelho.py:902` e imprime esse valor em `:981`.
- A conferência de exportação lê diretamente o minuto da ata e soma dias/minutos positivos em `app/folha/validacao_pdf.py:44-54`; esse relatório declara que não alimenta o TXT em `:38`.

**Pagamento, desconto, DSR, espelho/PDF final ou exportação efetivos desse caso: [não verificado]**, pois nenhum desses valores aparece no dado anexado.

## 13/11 14:46 · sessao 921173f54253 · - · - · origem=Ronald (tmux)

Para a batida **listada isoladamente** (`S 18:59:53`, id139122), os pares completos são **`[]`**. O pareador começa `cur=None` e, ao receber `S` sem entrada, produz um `Turno(entrada=None, saida=BAT139122, batidas=(BAT139122,), aberto=False, cross_meianoite=False)` — `app/ponto/turnos.py:1222-1223`, `1495-1496`, `312-324`. A data normalmente é27/10 para esse desenho diurno; eventual influência do DNA da véspera não consta no anexo [não verificado].

O cálculo é:

- `escala/utils.py:1879` importa `realizado_do_dia` de `ponto.turnos`.
- `escala/utils.py:1898` pede os turnos; `2045-2049` chama `realizado_do_dia`; `2058` grava o resultado no dia.
- `ponto/turnos.py:774-795` delega a `realizado_dos_turnos`.
- `ponto/turnos.py:609-612`: `_fim_fechado` retorna `None` para turno sem entrada.
- `ponto/turnos.py:733-739`: total inicia0 e ignora turno sem fim fechado; `771` retorna0.
- **Nuance:** com a S órfã presente no dia, `sem_turno=False`; não é o ramo `sem_turno=True` de `719-721`. O zero vem de não haver trecho E→S fechado — `733-739`, `771`.
- Dispensas sem `luz` não viram entradas: mapa de papel só considera `acesa` com `instante_luz` — `1638`; e se abstém quando faltam entrada/saída — `1682-1683`.
- A ata apenas copia o número já calculado — `ponto/services/cartorio.py:738-740`.

`ATA.orfas=[]` não contradiz isso: o casamento a marcos reconhece a S no marco19h; a órfã do pareador significa saída sem entrada — `ponto/turnos.py:1495-1496`.

**Limite:** o anexo não mostra a saída do pareador histórico nem as batidas da janela com pad. `turnos_do_colab` carrega dias vizinhos — `ponto/turnos.py:1862-1866`; a montagem pede uma janela — `escala/utils.py:1898`. Portanto, os pares da execução histórica são **[não verificado]**; o pareamento acima é a consequência do código aplicado exclusivamente às batidas listadas.

## 13/11 14:41 · sessao bfb9aff3c2f8 · HJ-092 · sonda · origem=Code

**A saída classifica 133 como `t1_certo`, 15 como `t2_certo` e deixa 469 indeterminadas. “Certo” aqui é o rótulo da sonda, condicionado ao teste de alternância.** `[nao verificado]` [nao verificado]

Os números abaixo derivam exclusivamente do dado colado, por isso levam `[nao verificado]`. Não localizei `sonda_classe.py` na cópia; seu corpo não consta do texto fornecido, portanto não pude conferir sua implementação nem o hash informado. `[nao verificado]`

**1. Contagem por competência**

Nas colunas de ocupação: P = preenchido, V = vazio, ? = `None`. `[nao verificado]`

| Competência | Total | t1 certo | t2 certo | Sem plantio vivo | Nenhum alterna | Ambos alternam | Outro marco P / V / ? | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|---|
| 2026-08 | 55 | 3 | 1 | 48 | 3 | 0 | 31 / 24 / 0 | `[nao verificado]` |
| 2026-09 | 198 | 44 | 8 | 96 | 50 | 0 | 100 / 77 / 21 | `[nao verificado]` |
| 2026-10 | 221 | 74 | 4 | 98 | 43 | 2 | 81 / 92 / 48 | `[nao verificado]` |
| 2026-11 | 143 | 12 | 2 | 124 | 4 | 1 | 13 / 53 / 77 | `[nao verificado]` |
| **Total** | **617** | **133** | **15** | **366** | **100** | **3** | **225 / 246 / 146** | `[nao verificado]` |

O total “não dá para dizer” é **51 / 146 / 143 / 129**, respectivamente, somando **469**. As classes fecham exatamente as 617 entradas, e o cabeçalho declara `ERROS 0`. `[nao verificado]`

Sobre **627 versus 617**, a saída permite confirmar somente o universo declarado de 617 nesta execução e a diferença aritmética de 10. Ela não identifica os dez `pergunta_id` adicionais nem demonstra quais logs estavam ausentes; portanto não permite reconciliar os dois universos ou classificar os dez restantes. `[nao verificado]`

**2. Limitações do critério e alcance**

Na cópia, `t1` vem do casamento por motivo em `marcos_faltantes`, passando `tipo=None`; `t2` usa a resposta, escolhe o marco mais próximo por distância circular e aceita distância até 240 minutos. Na divergência, retorna `t1`. `app/chamados/juizes.py:549`, `app/chamados/juizes.py:553`, `app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:296`, `app/chamados/services/materializacao.py:310`, `app/chamados/services/declaracao_texto.py:25`.

| Fraqueza | Onde pode errar | Quantos a saída permite delimitar |
|---|---|---|
| **Janela fixa** | Pode cortar vizinhos relevantes ou incluir outro turno. O materializador pode substituir a hora respondida pela hora do marco de intervalo e ajustar o instante por overnight. `app/chamados/services/materializacao.py:412`, `app/chamados/services/materializacao.py:426`. | **251** têm plantio vivo e entram potencialmente na comparação: **148** decididos e **103** ambíguos. Quantos sofreram corte ou contaminação da janela: `[nao verificado]`. |
| **Alternância como prova** | Alternar não demonstra identidade do marco, horário correto ou jornada correta. O pareador considera intervalo e papel informado pela ATA, que pode prevalecer sobre o tipo gravado. `app/ponto/turnos.py:1195`, `app/ponto/turnos.py:1201`. | Os **148** rótulos conclusivos dependem dessa heurística; **100** não alternam em nenhuma hipótese e **3** alternam nas duas. Quantos dos 148 estão efetivamente errados: `[nao verificado]`. |
| **Grade lida hoje** | O leitor consulta a célula/ATA disponível, sem parâmetro de instante histórico. Isso não reconstrói necessariamente o marco que originou `t2` no ato. `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:362`, `app/escala/services/leitor_celula.py:379`. | Até **617** reconstruções de marco/ocupação ficam expostas; **471** receberam booleano de ocupação e **146** receberam `None`. Quantas grades mudaram: `[nao verificado]`. |
| **Resposta do log × atual** | A derivação lê `p.resposta_colab`, e o log registra esse valor naquela chamada. O modelo admite respostas posteriores que superam anteriores. `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:308`, `app/chamados/models.py:3501`. | Até **617** comparações históricas ficam expostas se a sonda recomputa pela resposta atual. A saída não informa quantas respostas mudaram. Há **29** plantados diferentes de `t1`, mas isso não identifica a causa. `[nao verificado]` |
| **Exigir plantio vivo vinculado** | Ausência de vínculo não prova ausência histórica de plantio: `pergunta_origem` aceita nulo e seu contrato inclui plantios anteriores a 06/09. `app/ponto/models.py:40`. | Limita os **366 `sem_plantio_vivo`**; quantos representam ausência real, vínculo antigo ausente ou retratação: `[nao verificado]`. |
| **Ocupação histórica aproximada** | Sem o script, não se conferem os filtros temporais e de validade usados no teste dos 30 minutos. O modelo distingue instante da marca, criação e retratação. `app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:79`. | **225 preenchidos, 246 vazios, 146 desconhecidos** são resultados do indicador. Quantos reproduzem incorretamente a ocupação no ato: `[nao verificado]`. |

Os alcances acima se sobrepõem; não são quantidades de erros demonstrados nem devem ser somados. `[nao verificado]`

**3. Cruzamento com ocupação**

| Classe da sonda | Total | Outro preenchido | Outro vazio | Desconhecido | Fonte |
|---|---:|---:|---:|---:|---|
| `t1_certo` | 133 | **92** | 41 | 0 | `[nao verificado]` |
| `t2_certo` | 15 | 4 | **11** | 0 | `[nao verificado]` |

Entre `t2_certo`, **11/15 = 73,3%** tinham o outro marco vazio. Entre `t1_certo`, **92/133 = 69,2%** tinham o outro preenchido. Isso sustenta uma associação na amostra, condicionada ao critério da sonda. `[nao verificado]`

A candidata **“a grade só vale contra marco ainda vazio” não é sustentada como condição universal**: existem quatro `t2_certo` com outro marco preenchido — **pk26279, pk34263, pk39767 e pk44524**. Além disso, há **41 `t1_certo` com outro vazio**, portanto vazio sozinho não determina que `t2` esteja certo. `[nao verificado]`

O código atual também não exige marco vazio para selecionar `t2`: a lista de marcos contém tipo/hora de todas as lâmpadas, e a seleção filtra hora/tipo, proximidade e tolerância. `app/escala/services/leitor_celula.py:148`, `app/chamados/services/materializacao.py:293`, `app/chamados/services/materializacao.py:299`.

**4. col824 — pk52752 / pk52753 / pk53024**

| Pergunta | Entrada histórica fornecida | Saída fornecida | Fonte |
|---|---|---|---|
| **pk52752** | `CASO 52752 None`; sem valores disponíveis de `t1`/`t2`. | Não aparece nas listas de classificação. Isso não prova inexistência da pergunta nem ausência em outros logs. | `[nao verificado]` |
| **pk52753** | `intervalo_volta`, resposta **06:00**, **t1=E / t2=S**. | Plantado vivo **E**, outro marco **vazio**, `nao_da_para_dizer(nenhum_alterna)`. O plantado coincide com `t1`, mas a sonda não confirma nenhum dos dois. | `[nao verificado]` |
| **pk53024** | `orfao_14h`, resposta **01:01**, **t1=S / t2=E**. | Sem plantio vivo vinculado, ocupação **None**, `nao_da_para_dizer(sem_plantio_vivo)`. Não permite decidir entre `t1` e `t2`. | `[nao verificado]` |

## 13/11 14:40 · sessao f4074b0f4b47 · - · - · origem=Ronald (tmux)

Cálculo exclusivamente das tuplas fornecidas; sem acesso à definição do último campo. `[nao verificado]`

| Competência | t1 certo: preenchido | t1 certo: vazio | t1: desconhecido | t2 certo: preenchido | t2 certo: vazio | t2: desconhecido |
|---|---:|---:|---:|---:|---:|---:|
| 2026-08 | 2 | 1 | 0 | 0 | 1 | 0 |
| 2026-09 | 31 | 13 | 0 | 1 | 7 | 0 |
| 2026-10 | 51 | 23 | 0 | 3 | 1 | 0 |
| 2026-11 | 8 | 4 | 0 | 0 | 2 | 0 |
| Total | 92 | 41 | 0 | 4 | 11 | 0 |

Toda a tabela deriva da saída fornecida. `[nao verificado]`

Os quatro `t2_certo` com outro marco preenchido são pk26279, pk34263, pk39767 e pk44524. `[nao verificado]`

Há 29 plantados diferentes de t1: 16 em 2026-09 e 13 em 2026-10. A divisão por classe é: `[nao verificado]`

- `t1_certo`: 3 — pk31938, pk31942, pk34270. `[nao verificado]`
- `t2_certo`: 4 — pk22626, pk25140, pk27739, pk44524. `[nao verificado]`
- `nenhum_alterna`: 22 — pk20883, pk23194, pk24950, pk25441, pk26108, pk26298, pk26885, pk29236, pk29930, pk33586, pk33606, pk34460, pk35300, pk37621, pk39998, pk40362, pk40757, pk41275, pk41356, pk41656, pk41978, pk43672. `[nao verificado]`
- `ambos_alternam`: 0. `[nao verificado]`

O último campo é maior que 1 em 26 tuplas: 8 `sem_plantio_vivo`, 7 `nenhum_alterna`, 11 `t1_certo`, nenhuma nas outras classes; sua soma em todas as 617 tuplas é 648. Não é possível atribuir esse campo a repetições de log sem o script. `[nao verificado]`

## 13/11 14:40 · sessao dfa9ae20f57a · - · - · origem=Ronald (tmux)

Não localizei `sonda_classe.py` na cópia; o hash e a implementação exata da sonda não puderam ser conferidos. A análise do critério depende da descrição e da saída fornecidas. `[nao verificado]`

| Fraqueza | Evidência e alcance |
|---|---|
| Janela fixa pode cortar turno noturno ou incluir outro turno | O materializador pode ajustar o instante por overnight; intervalo também pode usar hora do marco em vez da resposta. `app/chamados/services/materializacao.py:413`, `app/chamados/services/materializacao.py:426`. Potencialmente **251** perguntas com plantio vivo entram na comparação de sequências: **148** decididas e **103** ambíguas; quantidade efetivamente prejudicada pela janela não consta. `[nao verificado]` |
| Alternância não prova qual marco ocorreu | O pareador distingue intervalo e papel informado pela ATA; o papel pode prevalecer sobre `Batida.tipo`. `app/ponto/turnos.py:1195`, `app/ponto/turnos.py:1201`. As **148** classificações conclusivas são conclusões da heurística; os **100 nenhum_alterna** e **3 ambos_alternam** expõem suas abstenções. Número de conclusões erradas: não mensurável pela saída. `[nao verificado]` |
| Grade atual não reconstrói a grade do ato | O leitor consulta `CelulaDia` por colaborador/data, sem parâmetro de instante histórico. `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:362`. Potencialmente **617** casos expostos; quantidade de grades efetivamente alteradas não consta. Os **146 None** na ocupação não comprovam alteração de grade. `[nao verificado]` |
| Resposta atual pode diferir da registrada no log | A derivação lê `p.resposta_colab`; o log grava a resposta vigente daquela chamada. `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:308`. O modelo reconhece respostas anteriores superadas. `app/chamados/models.py:3501`. Potencialmente **617** expostos; quantidade efetivamente alterada não consta. `[nao verificado]` |
| Ausência de plantio vivo vinculado não prova ausência histórica de plantio | `pergunta_origem` aceita nulo; seu próprio contrato inclui plantio anterior a 06/09 entre os registros sem vínculo. `app/ponto/models.py:40`. Isso limita a interpretação dos **366 sem_plantio_vivo**. `[nao verificado]` |
| Ocupação atual não basta para provar ocupação histórica | Há campos distintos de instante da batida, criação e retratação. `app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:79`. Sem o script, não se confere como esses tempos e o raio de 30 minutos foram empregados. A saída possui **225 True**, **246 False**, **146 None**; erros efetivos não são contáveis. `[nao verificado]` |

O cálculo de `t2` não filtra marco vazio: seleciona entre marcos com hora e tipo válidos, escolhe o mais próximo e aplica a tolerância. `app/chamados/services/materializacao.py:293`. A tolerância é 240 minutos. `app/chamados/services/declaracao_texto.py:25`. Quando ambos divergem, o retorno favorece `t1`. `app/chamados/services/materializacao.py:310`.

Para `col824`, `CASO 52752 None` só mostra ausência de entrada naquele mapeamento fornecido; não prova inexistência da pergunta nem ausência em todos os logs. `pk52753`: entrada histórica `intervalo_volta`, resposta `06:00`, `t1=E`, `t2=S`; saída com plantado `E`, ocupação `False`, classificação `nenhum_alterna`. `pk53024`: entrada histórica `orfao_14h`, resposta `01:01`, `t1=S`, `t2=E`; saída sem plantio vivo e ocupação `None`. `[nao verificado]`

## 13/11 14:38 · sessao 27bbcb37f3da · HJ-092 · sonda · origem=Code

**(a)** Na ATA fornecida da célula **128589 / col824**, **nenhuma lâmpada acesa tem `tipo_real` diferente do marco**: 17:00=`E/E`, 22:00=`S/S`, 23:00=`E/E`. Às 05:00, o marco é `S`, mas está apagado, com `tipo_real=null`; isso não demonstra inversão. A oposição mencionada no laudo anterior não aparece nesta ATA. [nao verificado]

A origem de `tipo_real` é:

- A grade monta `locs` com **`b.tipo`**, o tipo gravado da batida (`app/escala/utils.py:1323`).
- O alinhamento passa esse valor para `_celula_do_marco`, tanto no ramo por papel quanto no ramo por custo (`app/escala/utils.py:619`, `app/escala/utils.py:631`, `app/escala/utils.py:661`).
- A célula guarda separadamente `tipo/papel=tipo_m` e `tipo_real=tipo_real` (`app/escala/utils.py:326–327`).
- O cartório copia `x['tipo_real']` para a lâmpada; não o substitui pelo papel (`app/ponto/services/cartorio.py:711–713`).

Portanto, **`tipo_real` representa o tipo gravado da batida casada**, enquanto `tipo/papel` representa o marco. No anexo, 22:00=`S` coincide com **pk152080**, e 23:00=`E` coincide com **pk152081**. [nao verificado]

**(b)** A sequência e o pareamento **efetivamente exibido em `TURNO_SEM_MARCOS`** são:

| pk | Hora local | Tipo gravado | Resultado exibido |
|---|---|---|---|
| 143310 | 01/11 12:56:03 | E | Turno aberto. [nao verificado] |
| 143421 | 01/11 17:00:04 | E | Turno aberto. [nao verificado] |
| 143443 | 01/11 18:00:05 | E | Entrada do turno fechado. [nao verificado] |
| 152080 | 01/11 22:00:00 | S | Interna: saída para intervalo. [nao verificado] |
| 152081 | 01/11 23:00:00 | E | Interna: volta do intervalo. [nao verificado] |
| 143650 | 02/11 01:01:06 | S | Fecha o turno iniciado em pk143443. [nao verificado] |

**Sim: 17:00 e 18:00 já são entradas gravadas; 18:00 inicia o único turno fechado exibido.** [nao verificado] Uma `E` sucessiva preserva o turno anterior aberto e inicia outro (`app/ponto/turnos.py:1417–1420`). Sem marcos, o par `S22:00→E23:00` é reconhecido como intervalo pela existência da `S` posterior (`app/ponto/turnos.py:393–420`).

Para a célula, a grade chama `turnos_do_colab` e entrega seus turnos a `realizado_do_dia` (`app/escala/utils.py:1898`, `app/escala/utils.py:2045–2049`). O espelho também chama esse wrapper (`app/ponto/services/espelho.py:614`, `app/ponto/services/espelho.py:675–676`). O wrapper chama `parear_turnos` com papel da ATA, marcos e DNA diário (`app/ponto/turnos.py:1939`, `app/ponto/turnos.py:1992–1994`).

Com a ATA fornecida, o marco final apagado impede **esta célula** de emitir papel ao pareador; sem papel, vale o tipo gravado (`app/ponto/turnos.py:1682–1683`, `app/ponto/turnos.py:1382`). Com os marcos 22:00/23:00, essas batidas são absorvidas como intervalo (`app/ponto/turnos.py:1356–1376`).

**(c)** **Sim, a reconstituição fecha em 240 + 121 = 361:**

- **240 min:** pk143443 **18:00 → 22:00** pk152080. [nao verificado]
- **121 min:** pk152081 **23:00 → 01:01 do dia seguinte** pk143650. [nao verificado]

O código calcula **um turno bruto menos a pausa**: `421 − 60 = 361`; elimina segundos antes das diferenças e desconta o intervalo reconhecido (`app/ponto/turnos.py:605–606`, `app/ponto/turnos.py:740–741`, `app/ponto/turnos.py:756–771`). Os turnos abertos sem par fechado não acrescentam minutos (`app/ponto/turnos.py:735–739`).

A sonda não imprime os turnos efetivos de `turnos_do_colab` com toda a alimentação da janela. Para comprovar essa execução específica, faltam seus pares/pks e o mapa efetivo de papéis, incluindo eventuais batidas e células vizinhas. [nao verificado]

**(d) BATIDA — lista do admin:** há entradas consecutivas pk143310/pk143421/pk143443 e nenhuma oposição nas lâmpadas acesas da ATA apresentada [nao verificado]; `tipo_real` preserva o tipo gravado e cada nova `E` abre outro turno (`app/escala/utils.py:1323`, `app/escala/utils.py:619`, `app/ponto/turnos.py:1417–1420`).

_4630 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
