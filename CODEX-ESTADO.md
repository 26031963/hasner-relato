# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 11:57** · publicado em **12/11/2026 11:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 11:57 · sessao b126c59da370 · - · - · origem=Ronald (tmux)

Censo do delta e dos produtores/leitores:

| Campo | Escritor/origem | Leitores |
|---|---|---|
| `ata.orfas` | `ata_do_dia` copia `dia.orfas` como strings; porta atribui ata e persiste com evento. (`app/ponto/services/cartorio.py:727`, `:736`; `app/ponto/portas/celula.py:1006`, `:1033`, `:1040`, `:899`) | `_orfas_da_ata` conserva horas; ata antiga vira `?`; `grade_da_celula` publica. (`app/escala/services/leitor_celula.py:180`, `:187`, `:190`, `:420`) |
| `orfas_grade` | Adaptador devolve `gd.orfas`, espelho publica; sem grade/marcos devolve vazio. (`app/escala/utils.py:1087`, `:1113`; `app/ponto/services/espelho.py:699`, `:702`, `:854`) | HTML e PDF. (`app/templates/ponto/espelho.html:151`, `:240`; `app/relatorios/pdf_espelho.py:475`) |
| `dias[].data` | Espelho percorre datas da janela e publica a linha. (`app/ponto/services/espelho.py:663`, `:775`) | Indexação de órfãs e recorte PDF; datas dos turnos abertos. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:562`, `:577`) |
| `dias[].batidas` | Juiz atribui data por `pk`; espelho agrupa e publica. (`app/ponto/turnos.py:2945`, `:2953`, `:2955`; `app/ponto/services/espelho.py:579`, `:665`, `:776`) | PDF recupera timestamps classificados e atribui apresentação. (`app/relatorios/pdf_espelho.py:478`, `:567`) |
| `batida.timestamp` | Campo persistido da batida; adaptador transporta como entrada de período. (`app/ponto/models.py:23`; `app/ponto/adaptador_motor.py:37`, `:43`) | Ponte de dia, seleção de órfãs, marca e desenho. (`app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`, `:568`, `:840`) |
| `periodo.entrada`, `periodo.turno_aberto` | Motor cria entrada localizada e marca aberto quando saída é `None`; pareamento vem da autoridade. (`app/ponto/motor_calculo_v2.py:509`, `:530`, `:2751`, `:2753`, `:2878`, `:2880`, `:3216`, `:3218`) | Guarda do resumo PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`, `:516`; `app/ponto/services/espelho.py:426`, `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF adapta a classificação lida da tela, restringindo HH:MM à própria linha e armazenando timestamp completo. (`app/relatorios/pdf_espelho.py:475`, `:477`) | Mesma coleção alimenta resumo e `batida.orfa`. (`app/relatorios/pdf_espelho.py:178`, `:516`, `:568`) |
| `batida.orfa` | PDF atribui atributo transitório, sem `save`. Outro escritor transitório existe no calendário; não encontrei chamada desse serviço pelo PDF. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:622`, `:627`) | Filtro de anomalias, separação/asterisco, alertas da linha e legenda. (`app/relatorios/pdf_espelho.py:675`, `:679`, `:835`, `:844`, `:919`, `:923`, `:983`) |
| `resumo.turnos_abertos` | PDF inicia em zero e conta com guarda; tela conta sem guarda; na competência a autoridade folha pode sobrescrever pelo campo do fechamento. (`app/relatorios/pdf_espelho.py:253`, `:516`, `:517`, `:587`; `app/ponto/services/espelho.py:436`, `:437`; `app/relatorios/cartao_pela_celula.py:112`, `:346`) | Resumo HTML e retorno PDF; datas abertas alimentam `folha_manda`. (`app/templates/ponto/espelho.html:90`, `:92`; `app/relatorios/pdf_espelho.py:603`; `app/relatorios/cartao_pela_celula.py:396`, `:418`) |
| `_abertos_ts`, `datas_turno_aberto`, `dias[].turnos` | PDF guarda entradas contadas; data vem dos cards publicados pela tela, que usa ponte instante→pk→juiz. (`app/relatorios/pdf_espelho.py:518`, `:576`, `:578`; `app/ponto/services/espelho.py:453`, `:461`, `:463`, `:666`, `:777`) | Compositor de abertos e alertas do desenho. (`app/relatorios/pdf_espelho.py:587`, `:597`, `:921`; `app/relatorios/cartao_pela_celula.py:418`) |

**Atenção ao alcance da origem:** `orfas_grade` nem sempre vem de ata: o trecho anterior a `apur_ini` usa builder, assim como o ramo `AtaAgregadaSemMarcos`. O PDF recebe ambos pelo contrato da tela. (`app/ponto/services/espelho.py:493`, `:523`, `:533`, `:536`, `:542`)

**Fecho específico da data:** órfã 10/11 07:02 já não contamina entrada 11/11 07:02 no resumo nem na marca, porque ambos leem timestamps completos recuperados dentro da linha correta. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:178`, `:516`, `:568`)

**Fila herdada comprovável por leitura:** o desenho ainda apaga a data de marcas para filtrar anomalias globalmente. Uma órfã 10/11 07:02 elimina do cabeçalho qualquer anomalia de outro dia contendo “07:02”; ocorrência concreta de produção [nao verificado]. (`app/relatorios/pdf_espelho.py:675`, `:679`; formato das anomalias em `app/ponto/adaptador_motor.py:34`, `:41`)

**Fila semântica:** a classificação original só distingue strings HH:MM; atas antigas podem conter `?`, sem célula retorna vazio, e o adaptador sem marcos também descarta órfãs. Portanto `batida.orfa=False` continua significando ausência de marca disponível, sem provar casamento legítimo. (`app/escala/services/leitor_celula.py:190`, `:390`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:568`)

**Fila herdada de relógio:** `agora` chega à tela, mas não a `folha_manda`; esta chama turno vivo e teto sem repassar o instante. (`app/relatorios/pdf_espelho.py:474`, `:587`; `app/relatorios/cartao_pela_celula.py:398`, `:412`; `app/ponto/turnos.py:2290`, `:2291`; `app/relatorios/cartao_pela_celula.py:40`)

## 12/11 11:56 · sessao d257aac0c7f0 · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` | `responder_pergunta_disputa_core`: instante `now`, texto junto, salva e registra ato (`app/chamados/services/disputa_emissao.py:1462`, `:1467`, `:1468`, `:1508`, `:1511`). `responder_automaticamente`: instante recebido (`:1652`, `:1653`, `:1655`). `carimbar_resposta_pela_trilha`: instante da nota recebido (`:1661`, `:1665`, `:1674`, `:1676`). `carimbar`: completa nulo, apenas com texto, pelo instante do carimbo (`app/chamados/models.py:3125`, `:3131`, `:3133`). `_nascer` conserva `**campos` recebidos (`:3015`, `:3018`, `:3020`). Limpeza: `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`), `ressuscitar_perguntas` (`app/chamados/models.py:1567`). | DateTimeField: instante, não dia (`app/chamados/models.py:2449`). C6 lê projeção temporal, ignora `> agora`, inclui igualdade (`app/chamados/juizes.py:1375`, `:1380`, `:1389`, `:1407`). App usa cru `None` + disputa aberta (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Crédito separa cru nulo/não nulo (`app/chamados/services/raiox.py:135`, `:139`, `:140`). Fio usa `all(p.respondida_em…)` (`app/chamados/services/fio.py:340`). Portanto instante tem tipo comum, mas presença de carimbo recebe significados diferentes de pendência nesses leitores. |
| `validada_em` | `_carimbar_validacao`: `timezone.now`, salva e registra ato (`app/chamados/services/validacao.py:127`, `:134`, `:137`, `:139`). `carimbar(...resposta='validar')`: completa nulo com instante comum se texto (`app/chamados/models.py:3131`, `:3135`, `:3136`, `:3153`). `_nascer` conserva recebido (`:3015`, `:3020`). Limpam: nova resposta (`app/chamados/services/disputa_emissao.py:1471`, `:1480`, `:1508`), devolução (`app/chamados/services/validacao.py:199`, `:202`), reabertura e ressuscitação acima. | DateTimeField (`app/chamados/models.py:2450`). C6 ignora validação posterior a `agora`; presente no instante mata a pergunta antes do marco (`app/chamados/juizes.py:1378`, `:1391`). C19 usa decisão `VALIDACAO` e lâmpada: validada sem efeito pede materializar (`app/chamados/services/tarefa_do_dia.py:141`, `:145`, `:146`); não equivale a morte/encerramento. C19 inclui atos `ato_em <= agora` (`:389`, `:391`) e passivo `<= agora` (`:398`, `:399`). |
| `silenciado_ate`, autor, motivo | Escritores físicos `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`, `:166`, `:171`) e `arquivo.levantar` (`:177`, `:202`, `:210`). Chamadores: arquivar/desarquivar/lote (`app/chamados/views_arquivar.py:37`, `:77`, `:140`); ausência (`app/chamados/services/silencio_ausencia.py:73`, `:87`); isentos (`app/chamados/services/silencio_isentos.py:43`); comandos competência/prescritos/sem dia (`app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `arquivar_prescritos.py:94`; `arquivar_sem_dia_irrecuperavel.py:94`). | Instante de vencimento, ativo exclusivamente `> agora`; igualdade já não silencia (`app/chamados/models.py:508`, `:512`; `app/chamados/services/painel.py:188`, `:190`). Partição usa o mesmo `>` para ARQUIVADO, mas campo preenchido e vencido vira HISTORICO (`app/chamados/services/verbos.py:48`, `:51`), enquanto `qs_na_fila` volta a incluí-lo: classificações diferentes após vencimento. Silêncio não altera `status_local` (`app/chamados/services/arquivo.py:154`). |
| ato `SILENCIO` | `DisputaSupervisao.fechar` nas portas admin/desligamento chama `fechar_por_admin` e registra ato com instante do carimbo (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1926`). | C19 interpreta como decisão que encerra ações (`app/chamados/services/tarefa_do_dia.py:150`, `:151`). É ato de pergunta, distinto do véu temporal do chamado, que não muda estado (`app/chamados/services/arquivo.py:154`; `app/chamados/models.py:3842`, `:3844`). |
| `status_local` do chamado | Juiz motor, transição humana (`app/chamados/models.py:123`, `:124`); escrita persistente `_gravar_estado` (`:201`, `:228`, `:296`). Nascimento `abrir`: motor decide e `get_or_create` (`:934`, `:936`, `:939`, `:972`); `criar`: motor decide e INSERT (`:1061`, `:1065`, `:1096`). | Censo lê status das âncoras (`app/chamados/services/vigia_fio.py:618`) e usa TERMINAIS (`:675`, `:687`). Fila exige COBRANCA (`app/chamados/services/painel.py:189`). Catálogo distingue VIVOS, COBRANCA, ENCERRADOS e TERMINAIS (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`). C6/lastro pode ler ENCERRADOS, não todo TERMINAIS (`app/chamados/models.py:330`, `:332`). Status terminal não prova pergunta morta; app não filtra status do chamado (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Status concreto dentro de TERMINAIS dos 48: [nao verificado]. |
| `via_resolucao` | `_escrever_carimbo` atribui em memória (`app/chamados/models.py:3098`, `:3102`); persistem `carimbar` (`:3153`), identidade (`:3191`), descarimbo (`:3213`), lote (`:3266`, `:3267`). Exceções físicas: `encerrar_por_fato_no_espelho` (`:3276`, `:3277`), `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:125`, `:127`). | C6 lê via associada à materialização; carimbo posterior ao horizonte é retirado, identidade fora do fato permanece (`app/chamados/juizes.py:1376`, `:1382`, `:1384`); vias que vencem resposta (`:1402`) e demais identidades (`:1428`) têm degraus próprios. C19 lê decisões/efeitos/lavra, não resume tudo à via (`app/chamados/services/tarefa_do_dia.py:116`, `:133`, `:141`, `:150`). Via é vocabulário de encerramento (`app/chamados/models.py:2546`); a via isolada não prova qual escritor histórico agiu [nao verificado]. |
| Dia ao qual R/V/via se referem | Instantes acima registram o ato; o dia é outro campo (`app/chamados/models.py:2449`, `:2450`; `app/chamados/juizes.py:334`). | `_dia_de_turno_da_pergunta` usa `data_evento` como dia de turno para fato; conversa grava dia civil e devolve `None` ao juiz de turno (`app/chamados/juizes.py:334`, `:335`, `:338`). C6 janela inclui as duas pontas (`:618`). C19 carrega fatos `conversa=False`, intervalo de dias inclusivo `gte/lte` (`app/chamados/services/tarefa_do_dia.py:373`, `:375`, `:376`). |

| Via raspada | Juiz/escritor atual comprovado |
|---|---|
| `celula_absolveu` | `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula` → `carimbar` (`app/chamados/reconciliador.py:585`, `:596`; `app/chamados/models.py:3140`, `:3153`). |
| `fantasma` | `veredito_orfa` exige luz `is True`, viva e dia encerrado (`app/chamados/reconciliador.py:1083`, `:1085`, `:1087`, `:1089`, `:1091`); consumidor chama `encerrar_fantasma` (`:1148`) → `carimbar` (`app/chamados/models.py:3280`, `:3288`). |
| `fato_ocupado` | Recusa COLISAO, ainda não materializada → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `:1099`, `:1105`). |
| `declaracao` / `ausencia_dia` | Absorção escolhe por `declarou_hora` e carimba (`app/chamados/services/materializacao.py:548`, `:555`, `:567`); materialização principal escolhe via e carimba com validação/lavra (`:1283`, `:1284`). |
| `chamado_encerrado` | `resolver_perguntas_por_chamado_resolvido`: exige C6 morta, escolhe `via_pelo_fato` ou fallback, chama carimbar (`app/chamados/services/disputa_emissao.py:2048`, `:2058`, `:2059`, `:2060`); seleção final da via (`app/chamados/juizes.py:1757`). |
| Autoria histórica individual dessas linhas | As portas atuais acima não provam autor, instante nem caminho que produziu cada registro raspado [nao verificado]. |

## 12/11 11:56 · sessao 6014d0662f17 · - · - · origem=Ronald (tmux)

**Q2 — fecha por inspeção; execução ORM [nao verificado].** O módulo importa diretamente `instantaneo`, `julgar_celula` e `recalcular_fechamento_mes`, chama os três e não instala duplos: `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`, `:34`, `:35`, `:79`, `:83`, `:89`.

- **Regra independente:** cadastro 08–17, intervalo 12–13 e jornada 480 (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`); quatro batidas reais na segunda (`:66`); ausência folga aprovada na terça (`:73`). Os esperados literais são 960 previstos, 2 dias, 480 abonados, 480 realizados (`:107`). A regra produtiva preserva ausência como jornada, inclui o previsto e abona a parte não realizada (`app/ponto/services/dia_pago.py:392`, `:398`, `:400`, `:402`).

- **Integração real:** o recalculador lê `grade_do_fechamento`, chama `por_dia_da_grade` e preenche previstos/abono/realizado (`app/ponto/services/fechamento.py:1014`, `:1047`, `:1048`, `:1062`, `:1064`); o leitor transporta os três metadados da ata (`app/escala/services/leitor_celula.py:436`). O simulador chama novamente o recalculador real e entrega seus objetos aos três consumidores (`app/folha/management/commands/simular_folha.py:129`, `:133`, `:134`, `:152`).

- **Morde, por raciocínio do código:** `return {}` antecipado reprova `assertIn` (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`); perda do transporte na ata reprova presença/tipo/id (`:95`, `:97`, `:98`); recalculador cego a `ausencia_tipo` reprova contraprova que exige 480→0 de abono e diferença 480 no previsto (`:130`, `:139`). Trocar a autoridade de `_retidos` pelo gravado, vazio nesta fixture, deixa `apto_folha=None` e reprova o acesso/asserção de percentagem (`app/folha/management/commands/simular_folha.py:76`, `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`, `:128`). Execução desses mutantes [nao verificado].

- **Limites:** a ata antiga continua sintetizada da nova, removendo metadados e mudando `tipo_dia`; não é reprodução executada do cartório histórico (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:101`). Os asserts posteriores também comparam saídas antes/depois; não são todos valores independentes (`:116`, `:119`, `:123`). `ausencia_label` é cobrado por presença, sem conferir seu conteúdo (`:95`, `:98`).

- **Comentário impreciso:** “MESMO objeto” não é identidade demonstrada: `_lido()` e `_foto()` fazem chamadas independentes, embora a implementação atual do simulador transporte seus próprios objetos do recalculador (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`, `:89`, `:121`; `app/folha/management/commands/simular_folha.py:130`). A execução do ensaio com banco e o seu resultado permanecem [nao verificado].

## 12/11 11:56 · sessao a845178fa0c6 · - · - · origem=Ronald (tmux)

| Tela candidata | View/template e função real da lista | Classe c — 15 | Classe e — 48 |
|---|---|---|---|
| Drawer lateral, aba Mensagens | `app/colaboradores/views.py:915`, chamada `:940`, render `:990`; template `app/templates/colaboradores/partials/painel_colaborador.html:193`; lista real: `perguntas_do_app` → `consulta_do_app` → `projecao_do_app`, `app/chamados/services/pergunta_no_app.py:45`, `:61`, `:78`. | A pergunta respondida fica fora de `pendentes`: exige `respondida_em is None`; vai a `historico`, que esse bloco não renderiza. Outras perguntas do dia podem entrar; col516 tem `app=1` no dado. (`app/chamados/services/pergunta_no_app.py:71`, `:74`; `app/templates/colaboradores/partials/painel_colaborador.html:193`) | **Inclui as perguntas pendentes medidas por `app>0`**: usa a mesma consulta da sonda; não filtra status da âncora ou silêncio. Mostra pergunta, dia e marco. (`app/chamados/services/pergunta_no_app.py:49`, `:58`, `:71`, `:106`; `app/templates/colaboradores/partials/painel_colaborador.html:195`) |
| Modal do fio | `app/chamados/views.py:869`, montagem `:889`, render `:927`; template `app/templates/chamados/partials/modal_fio.html:68`, perguntas `:157`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Chamados entram sem filtro de status/silêncio; pergunta viva entra tanto em disputa aberta quanto fechada. **Ressalva:** enumeração de disputas exige `chamado__isnull=False`, ausente no dado; inclusão individual permanece [nao verificado]. (`app/chamados/services/fio.py:177`, `:184`, `:221`, `:235`) | Mesmo predicado e mesma ressalva de FK. Terminal não exclui cartão/pergunta; não equivale a aparecer na fila principal. (`app/chamados/services/fio.py:177`, `:186`, `:225`, `:240`) |
| Página Fio administrativo | `app/chamados/views.py:659`, montagem `:677`, render `:697`; `app/templates/chamados/fio_colaborador.html:153`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Inclui o chamado terminal pelo filtro apenas de colaborador; template renderiza cartões e followups, sem loop de `perguntas_disputa`. Ver o chamado não comprova ver a pergunta. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) | Mesmo comportamento. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) |
| Central/painel de gestão | `app/chamados/views.py:721`, chamada `:727`, templates `:740`, `:767`; lista real `montar_contexto_painel`, `app/chamados/services/painel.py:198`. | Âncora terminal **excluída do padrão** por `status_local__in=COBRANCA`. Drill explícito pelo status a inclui; `historico` inclui apenas resolvido/fechado, não cancelado/superado. Status concreto de cada âncora: [nao verificado]. (`app/chamados/services/painel.py:302`, `:306`, `:316`; `app/chamados/catalogo/motor.py:29`) | Mesmo predicado. (`app/chamados/services/painel.py:302`, `:306`, `:316`) |
| Pendências de disputa | `app/colaboradores/views.py:2998`, chamada `:3001`, template `:3011`; lista real `coletar`, `app/chamados/relatorios_disputa.py:26`. | **Condicional:** só disputa aberta entra. Dado não fornece `fechada_em`; inclusão dos 15 [nao verificado]. Se aberta, a categoria depende de TODAS as vivas da disputa: qualquer sem resposta → `AGUARDA_COLAB`; caso contrário → `AGUARDA_SUPERVISAO`. (`app/chamados/relatorios_disputa.py:18`, `:21`, `:40`) | **Disputa incluída**, pois pendência do app exige disputa aberta; classificação `AGUARDA_COLAB` pela pergunta sem resposta. A tela mostra pessoa e idade da disputa, não dias/perguntas individuais. (`app/chamados/services/pergunta_no_app.py:71`; `app/chamados/relatorios_disputa.py:21`, `:41`; `app/templates/colaboradores/partials/pendencias_disputa.html:6`) |
| Calendário do perfil | `app/colaboradores/views.py:1938`, chamada `:1949`, template `:1965`; lista real `contexto_calendario`, `app/colaboradores/services/calendario.py:159`; grade `app/templates/colaboradores/partials/_calendario_grade.html:67`. | **Dia incluído na janela escolhida**, sem filtro por classe/status de chamado: enumera datas inclusive. Âncora terminal não vira badge de ticket: tickets vêm de `VIVOS`; exceção agrupada exige pai vivo. Resolver dia ausente conforme dado e gate do template. (`app/colaboradores/services/calendario.py:437`, `:447`, `:603`, `:609`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) | Mesmo predicado: dia visível, pergunta não listada por essa grade. (`app/colaboradores/services/calendario.py:603`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:67`) |
| Cadastro × Realidade | `app/escala/views.py:476`, chamada `:487`, template `:492`; lista real `lista`, `app/escala/services/cadastro_realidade.py:787`; ramo C11 `beco_em_lote`, `:442`. | Inclusão **[nao verificado]**: C11 exige ativo, `veredito='indefinida'`, protesto específico, sem porta, viva sob terminal e `folga_nao_definida`; o recorte não informa todos esses campos. (`app/escala/services/cadastro_realidade.py:477`, `:488`, `:515`, `:518`) | Mesma insuficiência. Sem porta + terminal não bastam para provar presença na lista. (`app/escala/services/cadastro_realidade.py:477`, `:518`) |

## 12/11 11:55 · sessao d4ac8dbd5e32 · - · - · origem=Ronald (tmux)

| Obra | PRONTO literal curto na cópia | Cobertura comprovável e limite |
|---|---|---|
| E3-LEITORES | “fio, tela admin, API/app, badge/PWA, o_que_segura, vigia_fio leem SO tarefa_do_dia; 5 respondedores morrem com certidao” (`app/docs/BACKLOG.md:1873`) | Cobre migração dos leitores de todas as classes; não decide por si a ação do estado d-241. O PRONTO do anexo exige “V curada (ou 0)”, mas o dado não traz classificação V/T/X desses 241. (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:9`; linhas contadas no anexo colado, arquivo físico [nao verificado]) |
| E4B | “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1894`) | O universo executável é só `sem_porta`; os 21 `porta_fila` ficam fora desse comando. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`) |
| E4B, cura e | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | `reabrir_credor_de_pergunta_viva` cobre pergunta sem resposta, viva, com âncora fora de VIVOS; veta limbo/exportadas/recusa da reabertura. Portanto cobre a classe e-48 em escopo, sem prova de cura dos 48; c-15 respondida é excluída dessa cura. (`app/chamados/services/disputa_emissao.py:1805`, `:1821`, `:1827`, `:1835`, `:1839`, `:1848`, `:1851`) |
| E4B, demais atos | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | Consulta C19, registra passivo sem linha, pergunta e materializa conforme ação; `tarefa_nada` produz rótulo, sem ato; `rejulgar` só retorna aviso. Não prova cura de d-241, a-2 ou e-sem-viva-1 pelos agregados. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `:98`, `:101`, `:117`, `:138`, `:142`, `:144`, `:148`) |
| E4-ESCRITORES/E4C/E4D | “16 portas de estado da pergunta encolhem para efeitos + decisoes de E1”; “E4c LEITORES DIVERGE + selo 2 -> E4d parar projecao” (`app/docs/BACKLOG.md:1885`) | Cobre troca de representação/leitores/escritores; o trecho não promete ação humana específica nem transição individual dessas nove classes. (`app/docs/BACKLOG.md:1885`) |
| E1-ENTRADAS | “ampliada ao censo de toda porta do fato com as 3 curas de classe no mesmo commit” (`app/docs/BACKLOG.md:1859`) | Cobre portas do fato em âmbito estrutural; atribuir qualquer desses estados concretos a uma das três curas, sem dados adicionais, permanece [nao verificado]. |
| RAIZ | “elo raiz<-filhos: PerguntaDisputa com escritores fora do agregado, DisputaSupervisao nao reage”; sequência F0–F7. (`app/docs/BACKLOG.md:1904`) | Cobre a reação do agregado e o problema da dona fechada em âmbito estrutural; o PRONTO fornecido não identifica qual cura individual alcança c-15/e-48 ou os seis `cobrar` sem app. (`app/docs/BACKLOG.md:1904`) |
| PORTA-QUE-NAO-AGE | Substituída por MAPA-DO-FIO-MORTO no anexo. (`anexo_backlog.txt:11`, arquivo físico [nao verificado]) | Não usar a antiga promessa de 311→326 como cobertura ativa; o aval MAPA é medida, nenhum código. (`aval_mapa_do_fio_morto_1211.txt:2`, `:4`, arquivo físico [nao verificado]) |

**Pergunta 2: NÃO.** Nos anexos `avais_e3_leitores.txt` e `censo.r` não há decisão específica sobre o que C19 deve mandar — ou que deva mandar nada — quando o dia retém a folha e C19 não envia ação. A fase V/T/X é procedimento para decidir; “conservar o universo” e distinguir nada/desconhecido/sem porta não escolhem a ação desse estado. (`avais_e3_leitores.txt:4`, `:5`, `:10`, `:18`; `censo.r`, seção 3, decisões 4–5; arquivos físicos [nao verificado])

Atenção: a cópia possui relato histórico que discutiu esse desacordo e reclassificou universos anteriores; não equivale a decisão escrita no brief/censo anexados nem classifica os atuais 241. (`app/docs/RELATO.md:363`, `:365`, `:370`)

Diferença essencial para d-2: `censo_alcance` lê vivas por **FK célula**; a sonda lê fatos por **data_evento** e o app possui outro universo. Portanto `d + COLAB_RESPONDE` não autoriza concluir incoerência de C6 nem identificar o escritor concreto. (`app/chamados/services/vigia_fio.py:610`, `:614`, `:640`, `:677`; `sonda_mapa_fio.py`, laço de `ps_dia`/`pend_app`, arquivo físico [nao verificado])

## 12/11 11:49 · sessao 6d7e592f4fec · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

As curas de medição e precedência estão presentes e os seis mutantes declarados caem no log fornecido. Restam divergências com o aval literal, um aviso truncado para nomes maiores e a possibilidade de gastar novamente a cota herdada em `auto`. Ocorrência desses caminhos em produção: `[nao verificado]`. (`bin/disjuntor.py:1024`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:210`; `item2v2_mutantes.log:21`, `item2v2_mutantes.log:76`)

As referências aos anexos contam linhas desde o primeiro conteúdo de cada anexo. Comparação pelo histórico Git do intervalo informado e execução integral local do selo: `[nao verificado]`. O script cria, altera e apaga arquivos; minha avaliação da execução usa o log anexado. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`, `bin/tests/test_disjuntor.sh:1676`)

**Delta dos achados da volta 1:**

| Achado | A volta 2 cura? | O selo morde? |
|---|---|---|
| Aviso da segunda AUTO | **Parcialmente.** `auto_esgotada` governa tanto a mensagem quanto o aviso do disparo. Porém `trilha()` corta o detalhe em 300 caracteres: para `AUDITORIA-DE-TRAVAS-CORTE`, com uma reprovação final, o aviso tem 317 e termina em `so linha`, perdendo `/rearma do Ronald`. (`bin/disjuntor.py:1031`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:1194`, `bin/disjuntor.py:210`) | **Sim, para a retirada do aviso**, com `OBRA-A`; não cobre o truncamento acima. O mutante falha no caso 6, rc=6. (`bin/tests/test_disjuntor.sh:1233`, `bin/tests/test_disjuntor.sh:1239`, `bin/tests/test_disjuntor.sh:1988`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:55`) |
| Testemunha positiva de `lib['auto']` | **Sim.** O caso 7 exige `N >= 1`, igualdade entre trilha e medidor e presença de `auto=N` na apresentação. (`bin/tests/test_disjuntor.sh:1241`, `bin/tests/test_disjuntor.sh:1249`) | **Sim.** Retirar `auto` do medidor produz trilha=3, medidor=0 e rc=7. (`bin/tests/test_disjuntor.sh:1991`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:66`) |
| Contrato FALHA × SOBE | **O contrato fornecido está corrigido:** agora pede SOBE, como o código. Não é uma troca de evento no código, que já usava SOBE na volta 1. (`contrato_item2.txt:4`; `bin/disjuntor.py:1053`; `preverif_item2.r`, resposta 1 — localização exata `[nao verificado]`) | O caso 2 exige exatamente um SOBE; trocar o evento por FALHA violaria esse assert. Não há mutante específico dessa troca entre os seis declarados. (`bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1190`, `bin/tests/test_disjuntor.sh:1986`) |
| Nomes da cota e da medida | **Sim, na representação atual:** `auto_usada` é a lista cumulativa por obra; `lib['auto']` conta eventos na janela do painel. **Há ressalva de compatibilidade:** a guarda ignora um antigo `r['auto']`. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) | Os casos 1/2 verificam `auto_usada` e o 7 verifica a medida positiva. Não há caso nem mutante declarado para preservar cota anteriormente gravada em `auto`. (`bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1986`) |
| Precedência DESENHO/ERRO-BOBO × AUTO | **Sim.** A ordenação estável põe ambas as portas antigas antes de AUTO, preservando sua ordem relativa; a cota só é gasta se nenhuma candidata antiga elegível liberar. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1065`) | **Sim para a composição com ERRO-BOBO:** sem ordenação, gasta AUTO e o caso 8 cai, rc=8. DESENHO participa da mesma regra, mas não tem composição própria no caso 8. (`bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1992`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:77`) |

**1. O diff realiza literalmente o item 2?**

**Não integralmente.** O disparo continua sendo por quantidade de reprovações finais ou voltas Codex procedentes, sem consultar FORMA/ERRO-BOBO/DESENHO; a chamada que dispara retorna PARADA antes de examinar liberações. A classe é examinada na linha posterior `AUTOLIBERA`. Isso implementa o mecanismo de porta separada do contrato, mas não um disparo condicionado literalmente a DESENHO. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:656`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1033`, `bin/disjuntor.py:767`)

A segunda ocorrência também não fica exclusivamente dependente do Ronald: a cota restringe apenas `tipo == 'auto'`; uma nova linha antiga `ERRO-BOBO` libera sem cota nem comprovação de autoria. O próprio guardião descreve essa porta como disponível ao Code. Isso preserva o contrato, mas diverge da leitura literal “a 2a da mesma obra sobe a mim”. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:239`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1050`)

A porta nova exige classe aceita e motivo com letra ou dígito; registra a liberação com tipo `auto` e a linha original, incluindo a classe. Esse trecho atende ao rastro exigido. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`, `bin/disjuntor.py:1062`)

**2. Quem escreve e quem lê?**

| Campo/valor | Escritores | Leitores |
|---|---|---|
| Antigo `est['b3'][obra]['auto']` | Nenhum escritor específico identificado na implementação atual; o JSON genérico preserva chaves existentes. (`bin/disjuntor.py:378`, `bin/disjuntor.py:383`, `bin/disjuntor.py:1057`) | Nenhum leitor semântico atual da cota: a guarda lê exclusivamente `auto_usada`. Os carregadores genéricos leem o objeto inteiro. (`bin/disjuntor.py:371`, `bin/disjuntor.py:405`, `bin/disjuntor.py:1177`) |
| `auto_usada` | `decide_b3` acrescenta o SHA quando AUTO libera; `_com_estado` persiste. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:383`) | `auto_esgotada`, chamado pela recusa, pelo aviso do disparo e por `mensagem_b3`; testes 1, 2 e 8. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1194`; `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1256`) |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado, persistido pela mesma porta. (`bin/disjuntor.py:1052`, `bin/disjuntor.py:1081`) | A própria deduplicação de SOBE. (`bin/disjuntor.py:1051`) |
| `lib['auto']` → `b3_libera['auto']` | `ler()` inicializa e incrementa a partir de eventos LIBERA reconhecidos após o corte temporal. (`bin/disjuntor.py:2021`, `bin/disjuntor.py:2025`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:2093`) | `linha()`; `gerar_estado.py` via `ler/linha`; CLI `--linha`; testes antigo e positivo novo. (`bin/disjuntor.py:2310`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2395`; `bin/gerar_estado.py:280`; `bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1246`) |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` chama `trilha()` somente após persistência bem-sucedida. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1073`) | Leitor específico identificado: `conta()` no selo. O medidor do ESTADO não enumera SOBE; o leitor de censo o descarta pelo reconhecedor. (`bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1181`; `bin/disjuntor.py:2027`, `bin/disjuntor.py:2097`, `bin/disjuntor.py:2140`) |
| SHA consumido | `_sha_b3` calcula; `decide_b3` acrescenta a `consumidas`. `_fotografa` grava os SHA anteriores em `topo_no_disparo`. (`bin/disjuntor.py:681`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:836`, `bin/disjuntor.py:845`) | Guarda contra reutilização/anterioridade; AUTO aceita também o armazena em `auto_usada`, e AUTO recusada em `auto_sobe`. (`bin/disjuntor.py:1046`, `bin/disjuntor.py:1052`, `bin/disjuntor.py:1057`) |
| `B3_AUTO_COTA` | Constante definida como 1. (`bin/disjuntor.py:149`) | Predicado da cota, detalhe de SOBE e instrução da mensagem. (`bin/disjuntor.py:1177`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:1187`) |
| Classe/tipo `auto` | Code escreve a linha conforme contrato; o reconhecedor produz a candidata `('auto', linha)`. A fixture escreve pelo auxiliar `rel()`. (`contrato_item2.txt:2`; `bin/disjuntor.py:768`; `bin/tests/test_disjuntor.sh:1151`) | Reconhecedor valida classe/motivo; decisão aplica precedência/cota; LIBERA leva o tipo ao medidor. (`bin/disjuntor.py:767`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:2048`) |

Os leitores decisórios fora do trecho alterado são:

- **Stop:** `hook_stop_fila1 → julga_b3 → decide_b3`; para cada parada, chama `mensagem_b3`, que agora relê a cota persistida. (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1206`; `bin/disjuntor.py:1102`, `bin/disjuntor.py:1194`)
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`; PARADA recebe `mensagem_b3`. (`bin/disjuntor.py:1826`, `bin/disjuntor.py:1828`, `bin/disjuntor.py:1111`, `bin/disjuntor.py:1102`)
- **Wrapper:** `codex_pergunta.sh → --b3-parada → b3_parada → estado_b3`; retorno não zero recusa a volta. (`bin/codex_pergunta.sh:378`; `bin/disjuntor.py:2407`, `bin/disjuntor.py:1164`)
- **ESTADO:** `gerar_estado.py` recebe a medida por `ler/linha`, incorpora `dj_txt` e grava a página. Não usa a cota como contador do painel. (`bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`; `bin/disjuntor.py:2025`)
- **Carregadores genéricos:** os demais consumidores de `ler_estado` selecionam B1, B4 ou listas históricas; a referência do hook aos arquivos do disjuntor serve para rejeitá-los como prova de espera. (`bin/disjuntor.py:600`, `bin/disjuntor.py:1907`, `bin/disjuntor.py:2238`; `bin/hook_stop_fila1.py:664`)

**3. Segunda AUTO, DESENHO e preservação da cota.**

**Estado atual, segunda AUTO sozinha:** não libera; acrescenta o SHA recusado a `auto_sobe`, anuncia uma vez e não o acrescenta a `consumidas`. Duas chamadas e a preservação após rearme são exercitadas pelo caso 2. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1061`; `bin/tests/test_disjuntor.sh:1176`, `bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:3`)

**Segunda ocorrência pela porta antiga:** com cota gasta, obra disparada e linhas novas posteriores à fotografia:

```text
AUTOLIBERA OBRA-A: classe=FORMA segundo erro
ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
```

A linha antiga libera primeiro. Na volta 2, diferentemente do contraexemplo anterior, essa composição nem chega a produzir SOBE para a AUTO, pois encerra o loop na antiga. Não há autenticação do autor no reconhecedor. (`bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1065`; `bin/tests/test_disjuntor.sh:1218`)

**DESENHO pela porta nova:** `classe=DESENHO` é rejeitada. A validação é do rótulo, portanto `classe=FORMA erro de desenho` pode ser aceita; a natureza real do defeito não é julgada pelo reconhecedor. Classificação correta em produção: `[nao verificado]`. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`; `bin/tests/test_disjuntor.sh:1195`)

**Rearme/reinício com estado atual:** rearme não apaga `auto_usada`; a próxima decisão relê o JSON. Se o arquivo desaparecer, a inicialização parte de `{}`; isso é perda de estado, não um efeito necessário do reinício. (`bin/disjuntor.py:1140`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:370`, `bin/disjuntor.py:372`)

**Contraexemplo de compatibilidade:** obra disparada, demais condições de liberação satisfeitas, estado contendo `auto=['sha-da-primeira']` e sem `auto_usada`; uma AUTO nova libera e passa a criar `auto_usada=[sha-da-segunda]`. Não há conversão da chave antiga na migração existente. O contrato novo explicitamente define ausência de `auto_usada` como zero, portanto esse comportamento obedece ao contrato, mas não preserva “1x por obra” entre versões. Existência desse estado em produção: `[nao verificado]`. (`bin/disjuntor.py:808`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`; `contrato_item2.txt:4`)

**4. As linhas DESENHO/ERRO-BOBO antigas mudaram?**

Seus validadores continuam com as mesmas exigências mostradas no diff, e a ordenação nova preserva sua ordem relativa. A cota AUTO não impede essas linhas de liberar. (`bin/disjuntor.py:750`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`)

Na composição introduzida na volta 1, há mudança corretiva: AUTO antes de ERRO-BOBO deixa de gastar cota e consumir o SHA da AUTO; a antiga vence e recebe o evento LIBERA. Isso é demonstrado pelo caso 8. O mesmo critério de ordenação inclui DESENHO. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`)

**5. O selo morde cada mutante? Há assert vazio?**

**Sim, os seis declarados mordem no log fornecido; o controle sem mutação passa.** (`item2v2_mutantes.log:10`, `item2v2_mutantes.log:11`)

| Mutante | Mordida identificada |
|---|---|
| `auto-libera-a-2a` | Caso 2: segunda libera e cota chega a 2; falhas `[2]`, rc=2. (`bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:14`, `item2v2_mutantes.log:21`) |
| `auto-libera-desenho` | Caso 3: DESENHO reconhecida e liberada; falhas `[3]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`; `item2v2_mutantes.log:26`, `item2v2_mutantes.log:32`) |
| `sem-classe-libera` | Casos 3/4: reconhece DESENHO e libera os quatro inválidos; falhas `[3, 4]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`, `bin/tests/test_disjuntor.sh:1210`; `item2v2_mutantes.log:37`, `item2v2_mutantes.log:43`) |
| `sem-aviso-da-2a` | Caso 6: mensagem e aviso perdem a marca; falhas `[6]`, rc=6. (`bin/tests/test_disjuntor.sh:1239`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:54`) |
| `medidor-sem-auto` | Caso 7: trilha=3, medidor=0; falhas `[7]`, rc=7. (`bin/tests/test_disjuntor.sh:1249`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:65`) |
| `auto-sem-precedencia` | Caso 8: ERRO-BOBO=0, AUTO=1, cota gasta e AUTO posterior recusada; falhas `[8]`, rc=8. (`bin/tests/test_disjuntor.sh:1261`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:76`) |

Não há `all([])` nos controles atuais: `rs` recebe quatro resultados e `ctrl`, três. O positivo do medidor também exige pelo menos um evento. (`bin/tests/test_disjuntor.sh:1205`, `bin/tests/test_disjuntor.sh:1208`, `bin/tests/test_disjuntor.sh:1214`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1249`)

Persistem fragilidades: o caso 5 ignora o retorno de `dispara()` e só exige ausência no mapa de paradas; o caso 8 também ignora o segundo disparo e não exige LIBERA(auto)/cota após a tentativa final. São verificações que podem passar sem provar aquela transição se a preparação falhar; o log fornecido não demonstra esse esvaziamento. (`bin/tests/test_disjuntor.sh:1217`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1257`, `bin/tests/test_disjuntor.sh:1261`)

O harness exige âncora única e inclui controle da cópia sem mutação. Isso sustenta os mutantes declarados, sem cobrir automaticamente truncamento, migração da chave antiga ou composição AUTO+DESENHO. (`bin/tests/test_disjuntor.sh:1670`, `bin/tests/test_disjuntor.sh:1674`, `bin/tests/test_disjuntor.sh:1696`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1986`)

**6. Classes que já reprovaram obra.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | **Não identificado para a cota atual.** A alteração específica de `auto_usada` está em `decide_b3`, persistida por `_com_estado`. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1081`) |
| Relógio fora da borda | **Presente, herdado:** `decide_b3` chama `time.time()` quando `agora` falta; `b3_parada` não fornece esse instante. `trilha()` também lê `datetime.now()`. A nova cota é independente de relógio. (`bin/disjuntor.py:992`, `bin/disjuntor.py:1164`, `bin/disjuntor.py:210`, `bin/disjuntor.py:1177`) |
| Selo sem mordida | **Não para os seis declarados.** Há lacunas concretas de cobertura, descritas acima, e o nome curto do caso 6 deixa passar o truncamento. (`item2v2_mutantes.log:21`, `item2v2_mutantes.log:32`, `item2v2_mutantes.log:43`, `item2v2_mutantes.log:54`, `item2v2_mutantes.log:65`, `item2v2_mutantes.log:76`; `bin/tests/test_disjuntor.sh:1236`; `bin/disjuntor.py:210`) |
| Campo com dois significados | **Curado nos nomes atuais:** `auto_usada` e `lib['auto']` são células distintas para cota e medida. A compatibilidade com o antigo `auto` permanece descoberta. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) |
| Ordem de trava | **Não identifiquei inversão acrescentada.** Decisão, persistência e anúncio permanecem sob a trava do estado; wrapper e triagem adquirem resposta antes de estado. A nova leitura da mensagem não adquire outra trava. (`bin/disjuntor.py:363`, `bin/disjuntor.py:395`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:901`, `bin/disjuntor.py:933`, `bin/disjuntor.py:404`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

“Sobe ao Ronald” tem evidência de evento SOBE e de mensagens locais; entrega efetiva ao Ronald permanece `[nao verificado]`. O ESTADO não apresenta SOBE como evento próprio. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:2027`)

## 12/11 11:48 · sessao 3d6205c84e19 · PORTA-QUE-NAO-AGE · sonda · origem=Code

Os valores abaixo vêm exclusivamente do JSON fornecido. Nas tabelas, `[nao verificado]` marca a ausência de comprovação desses valores por arquivo:linha da cópia; as referências ao código explicam os campos.

1. `medicao_completa = true`; `dias_erro = 0`; `dias_medidos = 15`, de `dias_fixos = 15`. Não há dia com erro, etapa/função de falha ou classe de erro reportados. [nao verificado]

2. Tabela por dia.

`F(ids)` representa o objeto completo: `rota = porta_fila`, `fio = chamado`, `fila = [ids]` e `verbos = {cada id: decidir}`. Esses valores são os reportados no JSON. [nao verificado] O formato da porta está em `app/chamados/juizes.py:1657`; os significados de fila, verbos e fio, em `app/chamados/juizes.py:1621`.

A coluna resolver transcreve `resolver_dia_oferecido_nas_datas`; dono transcreve `dono_fora_do_fio_chamado`. O mapeamento exato desse último campo raspado à função do código é [nao verificado]. `motivos_a_perguntar` devolve os motivos que ainda pedem pergunta (`app/chamados/services/fabrica_da_celula.py:92`).

| col | data | porta_alcancavel_do_dia | resolver | n perguntas | n vivas | motivos_a_perguntar | dono | Evidência |
|---|---|---|---|---:|---:|---|---|---|
| col129 | 2026-10-25 | F(35101) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-01 | F(36592, 37904) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-02 | F(37905) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-08 | F(37925) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-10-30 | F(35974) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-06 | F(37406) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-09 | F(38110) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col427 | 2026-09-21 | F(28969) | False | 0 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-21 | F(33286) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-23 | F(34254) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-25 | F(34850) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-27 | F(35331) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col502 | 2026-10-28 | F(35432) | False | 4 | 0 | vazio `[]` | False | [nao verificado] |
| col899 | 2026-10-24 | F(34431) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col999 | 2026-10-28 | F(35602, 35697) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |

3. Tabela por pergunta.

Respondida/validada indica exclusivamente o preenchimento do respectivo timestamp, campos definidos em `app/chamados/models.py:2449` e `app/chamados/models.py:2450`. `validada_por` está em `app/chamados/models.py:3378`; `via_resolucao`, em `app/chamados/models.py:2562`. Vitalidade é calculada em `app/chamados/juizes.py:1389`; o marco admite `True`, `False` e `None` (`app/chamados/juizes.py:930`).

Datas e horas abaixo são locais, todas de 2026, com offset `-03:00`; os microssegundos foram preservados. [nao verificado] “Quem carimbou” usa somente `auditoria_com_payload_da_via_atual`, conforme solicitado; autoria por outros campos ou decisões permanece [nao verificado]. Os campos de usuário e criação da auditoria estão em `app/core/models.py:295` e `app/core/models.py:307`.

| pk | col / dia | motivo | via_resolucao | Respondida? / local | Validada? / local | validada_por_id | pergunta_viva | fato_do_marco_no_espelho | Quem carimbou: usuario_id / criado_em_local / auditoria pk | Evidência |
|---:|---|---|---|---|---|---:|---|---|---|---|
| 50169 | col129 / 10-25 | saida_sem_entrada | fantasma | sim / 10-27 07:53:49.378656 | sim / 10-28 09:45:05.311132 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50170 | col129 / 10-25 | orfao_14h | fantasma | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 52730 | col129 / 11-01 | saida_sem_entrada | declaracao | sim / 11-02 18:07:34.143947 | sim / 11-09 07:44:34.128888 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53028 | col129 / 11-01 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-02 09:42:03.090529 / 845648 | [nao verificado] |
| 53288 | col129 / 11-02 | saida_sem_entrada | declaracao | sim / 11-03 06:31:00.239353 | sim / 11-09 07:44:49.944781 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53333 | col129 / 11-02 | orfao_14h | declaracao | sim / 11-09 06:41:16.655654 | sim / 11-09 07:44:39.581541 | 657 | False | True | [nao verificado] | [nao verificado] |
| 55524 | col129 / 11-08 | saida_sem_entrada | declaracao | sim / 11-09 06:31:03.566947 | sim / 11-09 07:46:49.602920 | 657 | False | False | [nao verificado] | [nao verificado] |
| 55594 | col129 / 11-08 | orfao_14h | declaracao | sim / 11-09 06:31:38.641578 | sim / 11-09 07:51:24.055621 | 657 | False | True | [nao verificado] | [nao verificado] |
| 52131 | col258 / 10-30 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:21:02.666408 / 927797 | [nao verificado] |
| 54827 | col258 / 11-06 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:16:02.672755 / 927673 | [nao verificado] |
| 56040 | col258 / 11-09 | orfao_14h | ausencia_dia | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 46703 | col451 / 10-21 | saida_sem_entrada | fantasma | sim / 10-24 19:12:21.246122 | sim / 10-26 14:54:46.529352 | 657 | False | False | [nao verificado] | [nao verificado] |
| 46704 | col451 / 10-21 | orfao_14h | declaracao | sim / 10-24 19:12:31.807198 | sim / 10-29 11:33:16.500910 | 657 | False | True | [nao verificado] | [nao verificado] |
| 49422 | col451 / 10-23 | saida_sem_entrada | fantasma | sim / 10-24 19:12:52.450098 | sim / 10-26 14:54:46.995846 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49423 | col451 / 10-23 | orfao_14h | declaracao | sim / 10-24 19:13:02.454158 | sim / 10-29 11:33:43.367442 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50179 | col451 / 10-25 | saida_sem_entrada | fantasma | sim / 10-28 18:56:55.079529 | sim / 10-29 11:33:57.614654 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50180 | col451 / 10-25 | orfao_14h | declaracao | sim / 10-28 18:57:04.067558 | sim / 10-29 11:34:01.448378 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50883 | col451 / 10-27 | saida_sem_entrada | fantasma | sim / 10-28 18:57:23.247879 | sim / 10-29 11:34:13.719129 | 657 | False | False | [nao verificado] | [nao verificado] |
| 51025 | col451 / 10-27 | orfao_14h | declaracao | sim / 10-28 18:57:32.171962 | sim / 10-29 11:34:17.021036 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51042 | col502 / 10-28 | saida_sem_entrada | declaracao | sim / 10-30 14:02:10.543140 | sim / 10-30 15:04:02.578131 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51108 | col502 / 10-28 | intervalo_saida | declaracao | sim / 10-30 13:56:58.855588 | sim / 10-30 15:04:09.520077 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51132 | col502 / 10-28 | intervalo_volta | declaracao | sim / 10-30 13:57:12.532781 | sim / 10-30 15:04:15.720394 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51185 | col502 / 10-28 | orfao_14h | declaracao | sim / 10-30 07:16:04.323504 | sim / 10-30 12:19:16.432883 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49654 | col899 / 10-24 | orfao_14h | fato_ocupado | sim / 11-11 07:40:55.133099 | sim / 11-11 07:41:09.186539 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49655 | col899 / 10-24 | saida_sem_entrada | declaracao | sim / 11-01 12:19:46.972682 | sim / 11-11 07:40:58.843348 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51467 | col999 / 10-28 | orfao_14h | fato_ocupado | sim / 11-12 07:08:34.900551 | sim / 11-12 07:08:40.334664 | 657 | False | False | [nao verificado] | [nao verificado] |

4. Classes com o filtro solicitado: `pergunta_viva = False` e `fato_do_marco_no_espelho ∈ {False, None}`. Nesse recorte, os marcos são todos `False`; não há `None`. [nao verificado]

| Classe / via_resolucao | n perguntas | n dias-colab | Dias | pks | Evidência |
|---|---:|---:|---|---|---|
| fantasma | 5 | 5 | col129: 2026-10-25; col451: 2026-10-21, 2026-10-23, 2026-10-25, 2026-10-27 | 50169, 46703, 49422, 50179, 50883 | [nao verificado] |
| declaracao | 4 | 4 | col129: 2026-11-01, 2026-11-02, 2026-11-08; col502: 2026-10-28 | 52730, 53288, 55524, 51185 | [nao verificado] |
| fato_ocupado | 2 | 2 | col899: 2026-10-24; col999: 2026-10-28 | 49654, 51467 | [nao verificado] |
| Sem pergunta, com motivos_a_perguntar não vazio | 0 | 0 | nenhum | nenhum | [nao verificado] |

Esses agrupamentos registram a via atual e o estado observado. A causa C6 por degrau permanece `[nao verificado]`, conforme o próprio JSON.

5. Para col427 em 2026-09-21, o D4 reporta `status = folga`, `fonte = lavrado`; a janela atual informada é 2026-10-21 a 2026-11-20. [nao verificado]

| Operando da regra anexada | Valor / resultado | Evidência |
|---|---|---|
| `dia.get('pode_ultimo_recurso')` | True | [nao verificado] |
| `dia['data'] < hoje` | `2026-09-21 < 2026-11-12` → True | [nao verificado] |
| `dia.get('status') in STATUS_QUE_PEDEM_DECISAO` | `folga` no conjunto informado → False | [nao verificado] |
| Conjunção / retorno real | `True and True and False` → False | [nao verificado] |

**O operando que torna o resultado falso é a pertinência do status:** `folga` não pertence a `('falta', 'irregular', 'aberto', 'sem_certidao')`. O conjunto está em `app/colaboradores/services/calendario.py:94`; a conjunção, em `app/colaboradores/services/calendario.py:103`.

6. **As classes filtradas do item 4 cobrem 11 dos 15 dias**, com soma `5 + 4 + 2 = 11`, sem sobreposição entre esses grupos. Os quatro dias restantes são: [nao verificado]

| Dia-colab fora das classes filtradas | Por quê | Evidência |
|---|---|---|
| col258 / 2026-10-30 | Única pergunta, pk 52131, tem marco True | [nao verificado] |
| col258 / 2026-11-06 | Única pergunta, pk 54827, tem marco True | [nao verificado] |
| col258 / 2026-11-09 | Única pergunta, pk 56040, tem marco True | [nao verificado] |
| col427 / 2026-09-21 | Não tem pergunta; motivos_a_perguntar também está vazio, portanto não entra na classe própria solicitada | [nao verificado] |

Considerando **todas as seis classes originais do JSON**, inclusive as de marco aceso, a soma é `5 + 4 + 4 + 8 + 1 + 2 = 24` participações, cobrindo **14 dias-colab distintos** por haver sobreposições. Nesse conjunto original, o único dia fora de qualquer classe é **col427 / 2026-09-21**, sem perguntas. [nao verificado]

## 12/11 11:46 · sessao ca639e2b122d · - · - · origem=Ronald (tmux)

Âncoras no código, somente leitura:

| Campo/regra | Arquivo:linha |
|---|---|
| `pergunta_viva` | `app/chamados/juizes.py:862`; cálculo delegado em `app/chamados/juizes.py:1389` |
| `validada_em` preenchida encerra a pergunta nesse cálculo | `app/chamados/juizes.py:1391` |
| `fato_do_marco_no_espelho`: `True`, `False` ou `None`; `None` significa impossibilidade de afirmar | `app/chamados/juizes.py:930` |
| Mapa motivo → marco | `app/chamados/juizes.py:922`: `saida_sem_entrada`→`hi`; `orfao_14h`→`hf`; `intervalo_saida`→`hii`; `intervalo_volta`→`hfi` |
| Leitura das lâmpadas e resultado do marco | `app/chamados/juizes.py:965` |
| `motivos_a_perguntar` | `app/chamados/services/fabrica_da_celula.py:92`; retorno exclui os motivos já representados no domínio do fato em `app/chamados/services/fabrica_da_celula.py:154` |
| `porta_alcancavel_do_dia` | `app/chamados/juizes.py:1612`; retorno `rota`, `fila`, `verbos`, `fio` em `app/chamados/juizes.py:1657` |
| Significado de `fila`, `verbos` e `fio` | `app/chamados/juizes.py:1621` |
| `respondida_em` / `validada_em` | `app/chamados/models.py:2449` / `app/chamados/models.py:2450` |
| `validada_por` | `app/chamados/models.py:3378` |
| `via_resolucao` | `app/chamados/models.py:2562` |
| Auditoria: `usuario`, payload antes/depois, `criado_em` | `app/core/models.py:295`, `app/core/models.py:300`, `app/core/models.py:301`, `app/core/models.py:307` |
| `dono_fora_do_fio` | `app/chamados/services/vigia_fio.py:274`; ramos em `app/chamados/services/vigia_fio.py:306`; retorno sem dono em `app/chamados/services/vigia_fio.py:315` |

O mapeamento exato do campo raspado `dono_fora_do_fio_chamado` à função encontrada está [nao verificado].

## 12/11 11:46 · sessao e902d776f71d · FIO-E0-V4-E3-LEITORES · verificador · origem=Code

BLOQUEIA — a migração está parcial: permanecem respondedores próprios, há divergência entre sombra e vigia, e silêncio vencido continua com dois significados. (`app/chamados/services/raiox.py:139`, `app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/services/verbos.py:51`)

FILA — identidade: HEAD `513ff80771b84636b5b485452a74058b2a507ebb` e comparação Git com `087fe758a`: [nao verificado]; a cópia disponibilizada não contém `.git`. Execução ORM, suite e mortes dos mutantes: [nao verificado]. As reproduções abaixo usaram somente funções extraídas por AST, em memória, com entradas sintéticas. [nao verificado]

- FILA — **item 1, sonda/soma:** existe registro histórico de **232**, reconciliado como 118+114; não é resultado certificado desta volta. Sonda atual e inclusão dos mesmos 232 sítios/dias: [nao verificado]. RED de cura não se aplica à medição. (`app/docs/RELATO.md:249`)

- FILA — **item 2, V/T/X:** há classificações históricas diferentes: V=3/T=228/X=1 e, posteriormente, V=15/T=0/X=217 distribuído em subclasses. Classificação válida para este HEAD: [nao verificado]; não escolher uma delas como resultado atual. Sem RED de cura aplicável. (`app/docs/RELATO.md:235`, `app/docs/RELATO.md:237`, `app/docs/RELATO.md:238`, `app/docs/RELATO.md:239`, `app/docs/RELATO.md:240`, `app/docs/RELATO.md:241`)

- FILA — **item 3, V curada ou zero:** a árvore contém exposição administrativa pela vida C6 e pela decisão E1; esses corpos não são alterados pelo diff fornecido. Cura dos V reais, quantidade residual e RED correspondente nesta volta: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:243`, `app/chamados/services/tarefa_do_dia.py:314`)

- FILA — **item 4, chamadores C17:** **nada de defeito local demonstrado na substituição**: folha, vigia, beco e pauta consultam a porta da tarefa. Há casos que separam C17 de C19; retornar ao C17 contraria as expectativas desses testes. Mutantes executados por leitor: [nao verificado]. (`app/folha/fora_da_folha.py:72`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:170`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:166`)

- BLOQUEIA — **item 5, cinco respondedores:** não morreram: crédito, relatório, cobrança e template continuam respondendo por critérios próprios; o fio ainda consome crédito/desenlace. Contraexemplo: timestamp preenchido com resposta vazia produz “aguarda supervisão”, enquanto C19 exige RESPONDER do colaborador. Certidão e RED/mutante por cada sítio restante: [nao verificado]. (`app/chamados/services/raiox.py:139`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/cobranca.py:229`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/chamados/services/fio.py:405`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **item 6, catraca/paridade:** a catraca verifica somente chamadas C17; não certifica zero respondedores nem zero derivações próprias. Contraexemplo: `creditos_perguntas` permanece decidindo por timestamp sem infringir essa contagem. A injeção sintética testa retorno ao C17, não esses respondedores. Paridade executada: [nao verificado]. (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:145`, `app/chamados/tests/test_e3_pendentes_app.py:150`, `app/chamados/services/raiox.py:139`)

- BLOQUEIA — **item 7, DIFF da sombra:** a projeção do instrumento ficou diferente da produção. Para `verbos={âncora: 'validar'}`, `fio='colab'`, a sombra informa `exige_admin=True`; o vigia informa False. É exatamente a combinação coberta pelo caso de resposta vazia; o teste da sombra conserva o cálculo anterior. Queda real por classe: [nao verificado]. (`app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/tests/test_o261e_f5_alcance.py:177`, `app/chamados/tests/test_e3_diff_sombra.py:143`)

- FILA — **item 8, suite inteira:** resultado desta árvore [nao verificado]; não há certificação de execução no material fornecido. Sem sítio de cura/RED próprio: é etapa de execução. [nao verificado] [nao verificado]

- FILA — **item 9, opus final:** aprovação desta árvore [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado]

- FILA — **item 10, pouso:** pouso deste HEAD [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado] [nao verificado]

- FILA — **item 11, cache por disputa:** **nada no caso aberto multicartão demonstrado pelo RED**: o mapa agora é por disputa e alimenta o cache correspondente. Reverter para perguntas do cartão contraria os asserts de resumo e universo; morte executada [nao verificado]. Limite restante: disputa **fechada** com uma viva e uma validada entrega somente a viva ao explicador, porque as mortas são excluídas antes do mapa; portanto “disputa inteira” não vale literalmente nesse ramo. (`app/chamados/services/fio.py:257`, `app/chamados/services/fio.py:330`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:95`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:105`, `app/chamados/services/fio.py:241`, `app/chamados/services/fio.py:255`)

- FILA — **item 12, aviso sem anuência:** **nada de defeito local demonstrado na cura**: web e API usam `aguarda_o_colaborador`, recortando perguntas sem resposta/texto e consultando C15/C6. Reverter ao timestamp cru contraria o caso morta por célula e o caso viva sem texto. Execução/mutante morto [nao verificado]. (`app/chamados/services/fio.py:163`, `app/chamados/services/fio.py:358`, `app/api/views.py:1175`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:125`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:143`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:156`)

- FILA — **bloqueante #4, relógio:** **nada na cura direta de `alimentar_portas`**: C6 agora recebe `agora`; C19 já o passa em sua carga. Retirar o argumento contraria o RED e a verificação de repasse. `fio_vivo` continua sem parâmetro temporal, mas não é chamado diretamente pelo corpo C19 conferido. Execução/mutante morto [nao verificado]. (`app/chamados/juizes.py:1575`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/tests/test_e3_bloqueantes.py:110`, `app/chamados/tests/test_e3_bloqueantes.py:135`, `app/chamados/juizes.py:1494`)

- FILA — **bloqueante #8, furo sem canal:** **nada de derivação crua restante no trecho curado**: mede a porta publicada pela tarefa. Repor `VIVOS`/timestamp nulo contraria os casos chamado sem ação, âncora silenciada e registrada; existe controle positivo com porta. Execução/mutante morto [nao verificado]. (`app/chamados/services/furo_sem_canal.py:90`, `app/chamados/services/furo_sem_canal.py:100`, `app/chamados/tests/test_e3_bloqueantes.py:148`, `app/chamados/tests/test_e3_bloqueantes.py:155`, `app/chamados/tests/test_e3_bloqueantes.py:164`, `app/chamados/tests/test_e3_bloqueantes.py:171`)

- BLOQUEIA — **bloqueante #9, silêncio:** o filtro comum foi colocado no modelo, não em C19, e a lista real do admin continua divergindo. Exemplo: chamado aberto com `silenciado_ate=agora−1min` volta ao queryset e à lista do colaborador, mas vira `HISTORICO` e desaparece do painel padrão. O RED morde a reversão para `IS NULL`, porém só verifica os dois querysets; não morde essa divergência de apresentação. (`app/chamados/models.py:524`, `app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:324`, `app/chamados/tests/test_e3_b4_silencio_vencido.py:35`)

- BLOQUEIA — **#9, porta aparentemente alcançável:** com pergunta exigível ancorada no chamado vencido da célula, a tarefa pode publicar `rota='porta_fila'`, `fila=()`, `verbos={âncora: 'historico'}`. `portas` conserva o chamado; somente `fila` o exclui. O consumidor recebe uma porta cujo chamado foi ocultado na lista administrativa. (`app/chamados/juizes.py:1533`, `app/chamados/services/tarefa_do_dia.py:548`, `app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:551`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/painel.py:331`)

- BLOQUEIA — **segundo juiz de fechamento, fora do diff e alcançado pelo item 11:** o explicador calcula `pode_fechar` somente como `not fio_vivo`, omitindo a guarda C7 de resposta sem veredito. Exemplo: resposta com texto, sem validação/materialização, célula `ok` decidida pelo admin, dia não trancado: C6 pode dar morta, explicador diz “Tudo resolvido”, mas C7 recusa fechar. (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:140`, `app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`, `app/chamados/models.py:1855`, `app/chamados/models.py:1866`, `app/chamados/models.py:1899`)

- BLOQUEIA — **mutante sem alvo:** `diff_fachada_vazia` procura o antigo corpo de `candidata`; agora existe um alias. O harness lança “Mutante sem alvo” antes de executar teste. Esse RC não comprova mordida do RED. (`app/chamados/tests/e3_mutantes.py:112`, `app/chamados/management/commands/diff_e3_sombra.py:43`, `app/chamados/tests/e3_mutantes.py:162`)

- BLOQUEIA — **mutante temporal equivalente:** `alimentacao_sem_agora` substitui somente a primeira ocorrência de `agora=self.agora)`: atinge C6 em `_ler_vida`, deixando `alimentar_fila` intacta. C6 recupera o mesmo instante de `carga.agora`; portanto o mutante não reverte a alimentação pretendida. (`app/chamados/tests/e3_mutantes.py:101`, `app/chamados/tests/e3_mutantes.py:164`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/juizes.py:883`)

- BLOQUEIA — **leitor esquecido: API/app/badges/PWA:** a consulta ainda manda qualquer pergunta com timestamp para histórico. Exemplo: resposta vazia com timestamp deixa de contar como pendência no app/dashboard, enquanto C19 exige RESPONDER. Os dois kernels consomem essa projeção antiga; a alteração da correção do cartão não migra esses consumidores. (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:91`, `app/chamados/services/tarefa_do_dia.py:137`, `app/api/views.py:315`, `app/api/views_core.py:303`, `app/api/views.py:993`, `app/api/views.py:2533`, `app/core/views.py:252`)

- BLOQUEIA — **leitor esquecido: fio/admin/ficha:** crédito por timestamp alimenta verbo e desenlace; ficha traduz esse verbo em dono. No mesmo caso vazio, oferecem validação/admin enquanto a tarefa exige resposta do colaborador. (`app/chamados/services/raiox.py:139`, `app/chamados/services/verbos.py:34`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/fio.py:421`, `app/chamados/services/desenlace.py:72`, `app/colaboradores/services/ficha.py:159`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **leitor esquecido: cobrança:** `_calados` ainda usa pré-filtro `q_vivas`, timestamp nulo e disputa aberta. Pergunta com resposta vazia e timestamp deixa de ser cobrada por esse leitor, embora C19 ainda exija RESPONDER; pergunta morta por tranca também não é eliminada por uma consulta efetiva ao C6 nesse sítio. (`app/chamados/services/cobranca.py:229`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/juizes.py:1395`)

- BLOQUEIA — **leitor esquecido: calendário:** exclui resposta vazia antes de consultar C6 e deriva `pode_ultimo_recurso` separadamente. Exemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto C19 ainda publica RESPONDER. (`app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738`, `app/chamados/services/tarefa_do_dia.py:137`)

- FILA — **leitor esquecido: copiloto/navbar:** copiloto continua expondo C6 e `pergunta_no_app.do_dia`, sem a tarefa; navbar continua contando chamados da fila. Exemplo: chamado na fila sem pergunta pode contar no navbar, enquanto C19 tem apenas ação do sistema e nenhuma porta humana. A navbar usa unidade diferente de perguntas; não equiparar numericamente os badges. (`app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:108`, `app/core/context_processors.py:185`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`)

- BLOQUEIA — **campo `respondida_em`: significados incompatíveis entre leitores.** É instante de resposta humana, automática ou recuperada da trilha; também pode ser preenchido por `carimbar` e limpo na reabertura. C6 recorta pelo instante; C19 lê a decisão; crédito/app/relatório ainda interpretam presença como resposta suficiente. Contraexemplo comum: timestamp com texto vazio. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/juizes.py:1380`, `app/chamados/services/tarefa_do_dia.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:21`)

- BLOQUEIA — **campo `silenciado_ate`: escritores e fronteira estão identificados, leitores discordam.** `arquivo.silenciar` grava; `arquivo.levantar` limpa. O modelo define silêncio vigente por `ate > agora`: igualdade já venceu. Partição/painel conservam significado histórico permanente para prazo preenchido vencido, produzindo o contraexemplo #9. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:512`, `app/chamados/models.py:524`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:331`)

- FILA — **campo `data_evento`: dois significados discriminados.** Fato usa dia do turno; conversa usa dia civil. Nascimento, costura e rechaveamento passam pela atribuição em `gravar_dia_do_fato`; C19 exclui conversas e filtra datas inclusivamente. Mistura material adicional demonstrada: **nada**; legado vivo com data NULL [nao verificado], portanto a condição de triagem #2 continua sem medida. (`app/chamados/models.py:2796`, `app/chamados/models.py:2808`, `app/chamados/models.py:3029`, `app/chamados/services/fio.py:819`, `app/chamados/services/fio.py:898`, `app/chamados/services/fio.py:1056`, `app/chamados/services/tarefa_do_dia.py:375`)

- FILA — **campo `status_local`: nada de segundo escritor persistente demonstrado nos caminhos conferidos.** Gravação de transição em `_gravar_estado`; nascimento pela abertura. As outras atribuições indicadas simulam/restauram memória. Leitores usam universos distintos: VIVOS para crédito, COBRANCA para fila, fila para portas; chamado vivo não equivale a tarefa exigível. (`app/chamados/models.py:228`, `app/chamados/models.py:1108`, `app/chamados/models.py:356`, `app/chamados/models.py:371`, `app/chamados/services/raiox.py:139`, `app/chamados/services/painel.py:189`, `app/chamados/juizes.py:1533`)

- BLOQUEIA — **valores `pode_fechar` e `porta`:** não são campos persistentes nesses trechos. C7 calcula fechamento; o explicador escreve uma resposta paralela incompleta. C19 escreve rota/fila/verbos/fio em memória; `porta_da_tarefa` somente projeta, e folha/vigia/beco/pauta leem essa saída. Os contraexemplos são a guarda C7 omitida e a porta com fila vazia descritas acima. (`app/chamados/models.py:1866`, `app/chamados/services/explicador.py:163`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/tarefa_do_dia.py:565`, `app/folha/fora_da_folha.py:75`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`)

- FILA — **relógios: não existe uniformidade completa.** Nas bordas, vigia e lavrador do retrato capturam um instante; C19/C6 usam dia civil local e incluem fatos com instante `<= agora`. Porém `censo_alcance(agora=..., hoje=None)` lê o dia da máquina; `chamados_visiveis_colab(agora=...)` também calcula sua janela sem derivá-la desse instante. Exemplo: consultar outro mês com apenas `agora` mistura janela da máquina com silêncio/tarefa do instante recebido. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/veredito_lavrado.py:104`, `app/chamados/services/vigia_fio.py:567`, `app/chamados/services/janela_colab.py:42`, `app/chamados/services/janela_colab.py:58`)

- FILA — **ordem de trava: nada novo demonstrado nos caminhos conferidos.** Silêncio chama a porta de ordem; rechaveamento trava antes da escrita; o núcleo implementa C→D→CH→P. Ausência global de inversão por execução/reflexão [nao verificado]. (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/fio.py:1025`, `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`)

- FILA — **limite escritor/cascata:** permanece a condição de `post_save` que trata qualquer silêncio preenchido como motivo para não reprocessar isento, inclusive vencido. Exemplo: entrada em estado vivo com prazo vencido retorna antes do processamento. É lado escritor a registrar no censo, não prova de cura pelo novo filtro. (`app/chamados/signals.py:285`, `app/chamados/signals.py:299`)

- FILA — **classe A/B:** no diff fornecido, **classe B**: nenhum hunk em static/JS, service worker ou template base. Conferência independente pelo Git [nao verificado].

- FILA — **fora do escopo:** **nada demonstrado** como alteração de produto alheia aos avais; propagação de `agora` alcança os chamadores dos serviços migrados. Portão de arquivos contra o diff separado da E4B c10, vaga/teto e marcos desta volta no RELATO: [nao verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78`, `app/folha/retrato_folha.py:1158`, `app/ponto/management/commands/pautas_discordantes.py:68`)

## 12/11 11:44 · sessao f335ea201ffa · PORTA-QUE-NAO-AGE · censo · origem=Code

perguntas de lei: **8 — D1–D8 abaixo, sem escolher entre as leituras existentes.**

O código permite que uma pergunta deixe de estar viva **antes da conferência do marco cobrado**: por validação, tranca, identidade ou absolvição do dia. A porta `carimbar` consulta o C6 depois de aplicar os carimbos em memória; com `resposta='validar'`, a própria validação recém-aplicada pode determinar a morte (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1428`, `app/chamados/models.py:3131`, `app/chamados/models.py:3140`).

O dado raspado informa os 15 dias, mas não traz os campos das perguntas, autores, instantes, atas ou status D4. Portanto, **a classe de morte de cada um dos 15 e o veto concreto do col427 em 21/09 permanecem [nao verificado]**. Os testes citados abaixo foram lidos; execução e resultado atual: [nao verificado]. [nao verificado]

**Convenções da matriz**

| Sigla | Campos |
|---|---|
| C | `materializada_em`, `materializada_por`, `via_resolucao` (`app/chamados/models.py:3129`). |
| R | `resposta_colab`, `respondida_em` (`app/chamados/services/disputa_emissao.py:1467`). |
| V | `validada_em`, `validada_por` (`app/chamados/services/validacao.py:134`). |
| L | `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/models.py:3150`). |

**1. Escritores encontrados por AST e grep**

A varredura usou em memória o detector AST existente, ampliando os campos pesquisados. O detector acompanha atribuições, `setattr`, construtores, dicionários propagados, `update` e `bulk_update`; declara limitações para `__dict__`, nomes vindos de outro módulo e efeitos de `SET_NULL` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`). Universo informado pela execução: 767 arquivos Python de produção em `app`, sem testes/migrations/assets/logs, zero falhas de parse; artefato persistido dessa execução: [nao verificado].

| Escritor físico | Campos e persistência |
|---|---|
| `_escrever_carimbo` | Atribui C; quem persiste são `carimbar`, `carimbar_identidade` e `descarimbar`, por `save(update_fields=campos)` (`app/chamados/models.py:3098`, `app/chamados/models.py:3153`, `app/chamados/models.py:3191`, `app/chamados/models.py:3213`). |
| `carimbar` | Além de C, completa `respondida_em` e V nulos quando há texto e o parâmetro pede responder/validar; pode persistir L no mesmo `save` (`app/chamados/models.py:3131`, `app/chamados/models.py:3150`). |
| `descarimbar_em_lote` | `PerguntaDisputa.objects…exclude(…FORA_DO_FATO).update(...)`: limpa C e os campos recebidos em `**outros` (`app/chamados/models.py:3253`, `app/chamados/models.py:3265`). |
| `encerrar_por_fato_no_espelho` | Atribui e salva apenas `via_resolucao='fato_materializado'` (`app/chamados/models.py:3276`). |
| `relavra_via.relavrar` | Atribui a nova via, relavra e salva `via_resolucao` + L (`app/chamados/services/relavra_via.py:125`). |
| `_carimbar_validacao` | Atribui V, salva os dois campos e registra `DecisaoPergunta.VALIDACAO` (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`). |
| `devolver_validacao_recusada` | Limpa V, salva e registra `DEVOLUCAO` (`app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:205`). |
| `responder_pergunta_disputa_core` | Atribui R; limpa V anterior; incorpora eventual descarimbo por resposta; salva em transação (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1508`). |
| `responder_automaticamente` | Atribui/salva R e registra `RESPOSTA` (`app/chamados/services/disputa_emissao.py:1652`). |
| `carimbar_resposta_pela_trilha` | Atribui/salva `respondida_em` e registra `RESPOSTA` com o instante recebido (`app/chamados/services/disputa_emissao.py:1674`). |
| `reabrir_para_correcao` | Monta limpeza de R/V; usa `qs.update(**campos)` ou `descarimbar_em_lote`, conforme `desfazer_absorcao` (`app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629`). |
| `ressuscitar_perguntas` | Passa `respondida_em=None`, `validada_em=None`, `validada_por=None` para `descarimbar_em_lote` (`app/chamados/models.py:1567`). |
| Nascimento `_nascer` | Recebe `**campos`, constrói a pergunta e faz `save(force_insert=True)`; assinaturas R/V recebidas são conservadas e viram atos da sequência (`app/chamados/models.py:3015`, `app/chamados/models.py:3018`). |
| Efeito declarativo da FK | `validada_por` admite `SET_NULL` ao apagar o usuário; não é atribuição explícita de um serviço de pergunta (`app/chamados/models.py:3378`). |

As atribuições em `_universo_da_ressuscitacao` e `_desfazer_em_memoria` servem à simulação/restauração em memória, sem persistência própria (`app/chamados/models.py:1507`, `app/chamados/models.py:3160`).

Não apareceu outro `bulk_update` dos quatro campos no universo varrido; garantia fora dos limites declarados do detector: [nao verificado]. `flip_auto` é leitura por `filter(via_resolucao=...)`, não escritor (`app/ponto/services/flip_auto.py:128`).

Os apelidos/reexports relevantes não constituem escritores adicionais:

- `_reabrir`: `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2`: `app/chamados/views.py:1435`.
- Reexports dos corpos de materialização e emissão por `chamados.models`: `app/chamados/models.py:4088`, `app/chamados/models.py:4093`.
- `PerguntaDisputa` como `_PD`, `_PD_ordem` e `_PD_inf`: por exemplo, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/validacao.py:428`, `app/chamados/services/painel.py:260`.
- Juízes importados com apelido no materializador: `declarou_hora as _declarou`, `via_pelo_fato as _via_fato`, `fato_do_marco_no_espelho as _lampada_do_marco` (`app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1270`, `app/chamados/services/materializacao.py:906`).

**2. Matriz das operações que encerram ou carimbam encerramento**

`carimbar` não equivale a “exigiu marco aceso”: aplica C e, opcionalmente, resposta/validação; consulta C6; se viva, restaura e retorna `False`; se morta, salva e retorna `True` (`app/chamados/models.py:3130`, `app/chamados/models.py:3140`, `app/chamados/models.py:3153`). `carimbar_identidade` grava sem consultar C6; o julgamento fica no chamador (`app/chamados/models.py:3169`).

| Via / entrada | Juiz → operação | Persistido → retorno | Prova que exercita |
|---|---|---|---|
| **Validação**, resposta recebida, ainda não validada | Guardas de resposta/implausibilidade → `_carimbar_validacao`; depois materialização (`app/chamados/services/validacao.py:393`, `app/chamados/services/validacao.py:404`, `app/chamados/services/validacao.py:435`). | V + ato `VALIDACAO`; recusa de hora declarada pode desfazer V. Retorna `validada`, `ja_no_espelho` ou `nao_gravou` (`app/chamados/services/validacao.py:461`, `app/chamados/services/validacao.py:499`, `app/chamados/services/validacao.py:515`). | C6 mata validada sem exigir prova do marco: `app/chamados/tests/test_pergunta_viva.py:17`. Fecho sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:74`. |
| **`admin_decidiu`**, dia decidido | `q_vivas` do dia → `fechar_por_admin` → identidade (`app/ponto/services/veredito_celula.py:180`, `app/ponto/services/veredito_celula.py:190`). | C + ato `SILENCIO`; helper retorna booleano (`app/ponto/services/veredito_celula.py:195`, `app/chamados/models.py:3065`). | `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| **Silêncio / desligamento**, disputa com perguntas vivas | `respostas_sem_veredito` barra resposta sem julgamento; portas `admin_encerra/desligamento` percorrem vivas e chamam `fechar_por_admin` (`app/chamados/models.py:1887`, `app/chamados/models.py:1919`). | C nas perguntas; ato `SILENCIO`; fecho/autor/resolução/motivo na disputa. `True`; repetição idêntica, `False` (`app/chamados/models.py:1925`, `app/chamados/models.py:1933`, `app/chamados/models.py:1880`). | `app/chamados/tests/test_e4_expand_orm.py:63`. |
| **Folga aceita → `admin_decidiu`** | `o_dia_ja_e_folga` ou `checar_declaracao` → validação e decisão de folga (`app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:305`). | V + C; retorna `folga_ja_declarada` ou `folga_declarada` (`app/chamados/services/validacao.py:288`, `app/chamados/services/validacao.py:302`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:367`). | Teste específico localizado, corpo não aberto: [nao verificado]. |
| **`motivo_retratado`**, motivo removido | Guarda de motivo/disputa → `remover_motivo` → identidade (`app/chamados/models.py:2034`, `app/chamados/models.py:2062`). | Ato `RETRATACAO` + C, preservando carimbo anterior quando existente; retorna `removido` (`app/chamados/models.py:2057`, `app/chamados/models.py:2068`). | Preserva resposta e tira de viva: `app/chamados/tests/test_e1_entradas.py:142`. |
| **`duplicata`**, linha perdedora do fato | `plano_de_retratacao` → `retratar_duplicata` → identidade (`app/chamados/services/fio.py:1341`, `app/chamados/services/fio.py:1346`). | C, conservando instante/autor anterior; `recusa_motivo` aponta canônica. `True`; já duplicata, `False` (`app/chamados/services/fio.py:1351`, `app/chamados/services/fio.py:1360`). | Apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| **`fato_ocupado`**, colisão de tipo oposto | `traduzir_recusa`; código `COLISAO` e sem materialização → identidade, sem teste local da lâmpada (`app/chamados/services/materializacao.py:1097`). | C + motivo, eventualmente nota no destino; retorno implícito `None` (`app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1113`). | `app/chamados/tests/test_a27_recusa_com_dono.py:47`. |
| **`paridade_recusada`**, vizinha do mesmo tipo | **Exige lâmpada `is True`**; depois identidade (`app/chamados/services/materializacao.py:906`, `app/chamados/services/materializacao.py:930`). | C; retorna `None`. Apagada/indeterminada registra recusa, sem C (`app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:931`). | Três valores da lâmpada: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`, `:183`, `:192`. |
| **`celula_absolveu`**, pergunta absolvida pelo dia | `perguntas_absolvidas_pela_celula` → `carimbar`/C6 (`app/chamados/reconciliador.py:585`, `app/chamados/reconciliador.py:596`). | C; incrementa quantidade e pode fechar disputa pelo C7; retorna quantidade (`app/chamados/reconciliador.py:599`, `app/chamados/reconciliador.py:612`, `app/chamados/reconciliador.py:618`). | Consumidor: `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:147`; teto bloqueia carimbo: `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`. |
| **`sem_turno_aberto`**, motivo admitido, sem texto/materialização | Núcleo sem turno aberto; C6 antes e dentro de `carimbar` (`app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:406`). | C; eventual fecho C7; retorna `True` quando lavrou (`app/chamados/reconciliador.py:412`, `app/chamados/reconciliador.py:415`). | **Pergunta previamente validada, marco apagado, via persistida e disputa fechada:** `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`. |
| **`fantasma`**, órfã atual | `veredito_orfa` exige marco aceso, C6 viva e teto encerrado → consumidor → `encerrar_fantasma`/`carimbar` (`app/chamados/reconciliador.py:1083`, `app/chamados/reconciliador.py:1108`, `app/chamados/models.py:3288`). | C; booleano do helper; consumidor devolve contagens (`app/chamados/models.py:3285`, `app/chamados/reconciliador.py:1113`). | Recusa viva respondida e aceita aprovada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:676`. |
| **`juiz_aprovou`**, pendente após fecho da disputa | Lastro aprova; C6 também precisa aceitar o carimbo (`app/chamados/services/materializacao.py:1433`). | C; devolve lista dos PKs carimbados (`app/chamados/services/materializacao.py:1437`). | Resposta por validar não sai: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| **`declaracao`**, aplicação da resposta | Materializa/reutiliza efeito; para hora/data_hora exige lastro; escolhe via por `_declarou`; consulta C6 com V aplicada (`app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1283`). | C + complementos R/V + L; C6 recusando provoca rollback; unidade marca sucesso (`app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1288`). | Plantio real: `app/chamados/tests/test_fechamento_unificado.py:90`. Absorção também pode dar essa via sem plantio próprio: `app/chamados/tests/test_hx_via_ausencia_dia.py:104`. |
| **`ausencia_dia`**, escape/irmãs absorvidas | `declarou_hora` distingue declaração de escape; `carimbar(...resposta='validar')` consulta C6 (`app/chamados/services/materializacao.py:548`, `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567`). | C, complementos R/V conforme texto; ato `ABSORCAO` se gravou (`app/chamados/services/materializacao.py:569`). Retorno integral do serviço: [nao verificado]. | Irmã muda continua sem resposta e recebe a via: `app/chamados/tests/test_hx_via_ausencia_dia.py:78`; escape: `:90`. |
| **`completude_turno`** | `via_pelo_fato`: TM com saída e motivo fora de intervalo → S128 ou materializador (`app/chamados/juizes.py:1754`, `app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/materializacao.py:1270`). | Pelo S128: C, complementos R/V e L; retorna contagens de perguntas/disputas (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | S128 com/sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:60`, `:74`. |
| **`adesao`** | Agrupado, sem batida apurável tocando o dia civil e pai não LIMBO-FOLGAS → `via_pelo_fato` → carimbo (`app/chamados/juizes.py:1713`, `app/chamados/juizes.py:1733`, `app/chamados/juizes.py:1740`). | Mesmos campos/retorno do S128 acima (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`cadastro_folga`** | Mesmo ramo sem batida; pai `sem_previsao_escala/folga_nao_definida` → carimbo (`app/chamados/juizes.py:1738`). | Mesmos campos/retorno do S128 (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`chamado_encerrado`** | Resto de `via_pelo_fato`; também ramo sem credor, após C6 já dar morta (`app/chamados/juizes.py:1757`, `app/chamados/services/disputa_emissao.py:2048`, `app/chamados/services/disputa_emissao.py:2058`). | C; no ramo sem credor completa apenas resposta, não valida; trilha `fecho_sem_credor`; retorno global em contagens (`app/chamados/services/disputa_emissao.py:2060`, `app/chamados/services/disputa_emissao.py:2063`, `app/chamados/services/disputa_emissao.py:2094`). | Sem texto fecha; com resposta viva recusa: `app/chamados/tests/test_o261_resposta_vazia.py:78`, `:103`. |
| **`fato_materializado`** | Comando exige lâmpada `is True` → `encerrar_por_fato_no_espelho` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:41`). | Só via; `True`, ou `False` se já nessa via/fora do fato (`app/chamados/models.py:3274`). | Teste específico do comando: [nao verificado]. |
| **`competencia_trancada`** | Escritor antigo retirado; hoje C6 lê a tranca. A porta de identidade não aceita novo carimbo dessa via (`app/chamados/models.py:3067`, `app/chamados/models.py:3119`). | Nenhum campo novo; acervo antigo é descarimbado ao reabrir (`app/chamados/models.py:3318`). | Leitura sem escrita/destrancar: `app/chamados/tests/test_c7_red_minimos.py:361`, `:369`. |
| **`legado_sem_lastro` / via vazia** | Vocabulário/acervo lido; escritor atual atribuindo esses valores como encerramento não localizado: [nao verificado]. | `legado_sem_lastro` e `chamado_encerrado` pertencem a `VIAS_SEM_LASTRO`; resposta posterior pode descarimbar (`app/chamados/models.py:3766`, `app/chamados/models.py:3314`). | Prova específica de morte por essa via isolada: [nao verificado]. |
| **Re-lavra de via derivada** | `declarou_hora` ou `via_pelo_fato`; apenas vias anteriores `declaracao/completude_turno` (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:95`, `app/chamados/services/relavra_via.py:118`). | Via + L; retorna `(n_via,n_ver)` (`app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:139`). | Teste funcional específico: [nao verificado]. |

`respondida_em` sozinho **não é uma via de morte**: resposta com texto, sem validação, normalmente mantém a pergunta viva; as exceções anteriores na ordem do C6 continuam prevalecendo (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1402`, `app/chamados/juizes.py:1407`). As portas automática e de recuperação pela trilha gravam resposta; a validação posterior pertence a outro escritor (`app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1661`).

**3. Saídas de viva que acontecem só por leitura**

Estas saídas não precisam de uma nova escrita nos quatro campos pesquisados (`app/chamados/juizes.py:1389`).

| Estado de entrada → juiz | Operação / persistência / retorno | Prova |
|---|---|---|
| V presente → primeiro degrau C6 | Não consulta marco; nenhuma escrita; `False` (`app/chamados/juizes.py:1391`). | `app/chamados/tests/test_pergunta_viva.py:17`. |
| Dia trancado, sem toque humano posterior → `dia_da_pergunta_trancado` | Nenhuma escrita; C6 `False` (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:1395`). | `app/chamados/tests/test_c7_red_minimos.py:361`, `:391`. |
| `duplicata/motivo_retratado/tranca` com materialização | Vence até resposta existente; nenhuma escrita; `False` (`app/chamados/models.py:2525`, `app/chamados/juizes.py:1402`). | Duplicata respondida sai da fila: `app/chamados/tests/test_leitores_ouvem_a_lei.py:28`. |
| Demais identidades com materialização, alcançado o degrau | Nenhuma consulta necessária ao marco; `False` (`app/chamados/juizes.py:1427`). | `app/chamados/tests/test_pergunta_viva.py:31`. |
| Célula não acusadora, sem marcos pendentes | C6 absolve pelo dia; não exige positivamente a luz do marco nesse primeiro termo; `False` (`app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1452`). | `app/chamados/tests/test_pergunta_viva.py:39`. |
| Materializada, via sem testemunha | Lastro aceita identidade, ausência de dia, fora da janela viva, ou ausência de falta; exceção da classificação também termina em `True`; C6 pode devolver `False` (`app/chamados/juizes.py:679`, `app/chamados/juizes.py:691`, `app/chamados/juizes.py:702`, `app/chamados/juizes.py:1473`). | Cobertura exata de todas essas condições: [nao verificado]. |
| `escolha` materializada | Lastro usa apenas `materializada_em`; não exige batida/lâmpada (`app/chamados/juizes.py:706`). | Teste específico desse ramo: [nao verificado]. |
| Hora/data_hora, slot não derivável | Lastro pode aceitar destino encerrado ou retratação com trilha; na janela viva tenta vetar pela falta (`app/chamados/juizes.py:765`, `app/chamados/juizes.py:786`, `app/chamados/juizes.py:797`). | Teste específico de cada fallback: [nao verificado]. |
| Hora/data_hora ou texto, últimos degraus | Lastro aceita batida viva no slot, retratação com trilha ou saída do turno; esses termos não exigem a ata do marco acesa (`app/chamados/juizes.py:752`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:817`). | Batida no slot: `app/chamados/tests/test_fechamento_unificado.py:48`; ocorrência com ata apagada: [nao verificado]. |
| Exceção ao co

[truncado em 20000 de 30259 caracteres]

## 12/11 11:44 · sessao 52942a8dad1b · PORTA-QUE-NAO-AGE · censo · origem=Code

```python
# Execucao e resultados em producao: [nao verificado].
# Molde: anexo sonda_medida3.py fornecido no pedido [nao verificado].
# Universo literal do pedido [nao verificado].
#
# Juizes chamados pelo censo:
# alimentar_portas: app/chamados/services/vigia_fio.py:604.
# porta_alcancavel_do_dia: app/chamados/services/vigia_fio.py:635.
# resolver_dia_oferecido_nas_datas, via _resolver_da_rodada:
# app/chamados/services/vigia_fio.py:265,636.
# dono_fora_do_fio, somente no ramo sem porta e passado:
# app/chamados/services/vigia_fio.py:642,648,654.
#
# Perguntas DO DIA: celula_id, sem filtrar por data_evento.
# app/chamados/services/vigia_fio.py:614.
# A fabrica usa outra chave: colaborador + data_evento + conversa=False.
# app/chamados/services/fabrica_da_celula.py:148.
#
# Classes abaixo agrupam observacoes, sem inventar causa/degrau do C6.
# O juiz devolve booleano: app/chamados/juizes.py:862,1389.

import datetime as dt
import inspect
import io
import json
import logging
import traceback
from collections import defaultdict
from contextlib import contextmanager, redirect_stdout, redirect_stderr

from django.db import connection, transaction


FIXO = {
    129: "2026-10-25 2026-11-01 2026-11-02 2026-11-08",
    258: "2026-10-30 2026-11-06 2026-11-09",
    427: "2026-09-21",
    451: "2026-10-21 2026-10-23 2026-10-25 2026-10-27",
    502: "2026-10-28",
    899: "2026-10-24",
    999: "2026-10-28",
}
DATAS = {
    cid: [dt.date.fromisoformat(s) for s in datas.split()]
    for cid, datas in FIXO.items()
}
PARES = [(cid, data) for cid, datas in DATAS.items() for data in datas]
chamada = {}


def endereco(arquivo, linha):
    caminho = (
        "app/" + arquivo.split("/app/", 1)[1]
        if "/app/" in arquivo
        else "<sonda-ou-biblioteca>"
    )
    return caminho + ":" + str(linha)


def tb_seguro(tb):
    # Nao inclui mensagens, codigo-fonte ou variaveis locais.
    return "\n".join(
        endereco(frame.filename, frame.lineno) + " " + frame.name
        for frame in traceback.extract_tb(tb)
    )


class FalhaInternaRegistrada(RuntimeError):
    pass


class Captura(logging.Handler):
    def __init__(self):
        super().__init__(logging.WARNING)
        self.registros = []

    def emit(self, record):
        self.registros.append(record)


@contextmanager
def somente_capturar_logs(captura):
    # Alteracoes apenas em memoria; mensagens livres nao vao aos handlers.
    loggers = [logging.getLogger()] + [
        obj
        for obj in list(logging.Logger.manager.loggerDict.values())
        if isinstance(obj, logging.Logger)
    ]
    anteriores = [
        (obj, obj.handlers[:], obj.propagate, obj.level, obj.disabled)
        for obj in loggers
    ]
    try:
        for obj in loggers:
            obj.handlers = [captura]
            obj.propagate = False
            obj.setLevel(logging.WARNING)
            obj.disabled = False
        yield
    finally:
        for obj, handlers, propaga, nivel, desativado in anteriores:
            obj.handlers = handlers
            obj.propagate = propaga
            obj.setLevel(nivel)
            obj.disabled = desativado


def chamar(funcao, *args, **kwargs):
    global chamada
    assinatura = inspect.signature(funcao)
    chamada = {
        "funcao": funcao.__module__ + "." + funcao.__name__,
        "assinatura": str(assinatura),
    }
    try:
        assinatura.bind(*args, **kwargs)
    except TypeError as exc:
        # bind informa parametros ausentes/incompativeis sem imprimir objetos.
        chamada["argumento_incompativel_ou_ausente"] = str(exc)
        raise

    antes = len(captura.registros)
    resultado = funcao(*args, **kwargs)
    novos = captura.registros[antes:]
    if novos:
        # Falhas internas registradas tambem invalidam a medicao:
        # app/chamados/juizes.py:1463,1476;
        # app/colaboradores/services/calendario.py:478.
        exc = FalhaInternaRegistrada("funcao registrou WARNING/ERROR")
        exc.registros = novos
        causa = next(
            (
                record.exc_info[1]
                for record in novos
                if record.exc_info and record.exc_info[1]
            ),
            None,
        )
        if causa is not None:
            raise exc from causa
        raise exc

    if connection.needs_rollback:
        raise FalhaInternaRegistrada("transacao marcada para rollback")
    return resultado


def erro_seguro(exc):
    causa = exc.__cause__
    return {
        "erro_classe": type(exc).__name__,
        "erro": (
            "falha de leitura; mensagem livre omitida para preservar a raspagem"
        ),
        "chamada": dict(chamada),
        "tb": tb_seguro(exc.__traceback__),
        "causa_classe": type(causa).__name__ if causa is not None else None,
        "causa_tb": (
            tb_seguro(causa.__traceback__) if causa is not None else None
        ),
        "registros_internos": [
            {
                "nivel": record.levelname,
                "fonte": endereco(record.pathname, record.lineno),
                "erro_classe": (
                    record.exc_info[0].__name__
                    if record.exc_info and record.exc_info[0]
                    else None
                ),
            }
            for record in getattr(exc, "registros", ())
        ],
    }


def local(valor):
    return timezone.localtime(valor).isoformat() if valor is not None else None


def evidencia_autoria(p):
    # Autor do carimbo persistido nao garante autor da via atual:
    # app/chamados/models.py:3183;
    # app/chamados/services/relavra_via.py:123.
    #
    # Projecoes sem descricao, payload, texto de resposta ou ator livre:
    # app/core/models.py:295;
    # app/chamados/models.py:3872.
    logs = LogAuditoria.objects.filter(
        modelo="PerguntaDisputa", objeto_id=p.pk
    )
    todos_pks = list(logs.order_by("pk").values_list("pk", flat=True))
    coincidentes = list(
        logs.filter(valor_depois__via_resolucao=p.via_resolucao)
        .order_by("pk")
        .values("pk", "usuario_id", "criado_em")
    )
    decisoes = list(
        DecisaoPergunta.objects.filter(pergunta_id=p.pk)
        .order_by("pk")
        .values("pk", "ato_por_id", "ato_em")
    )
    return {
        "materializada_por_id": p.materializada_por_id,
        "materializada_em_local": local(p.materializada_em),
        "autoria_da_via_atual_apenas_pelo_campo": "[nao verificado]",
        "auditoria_n": len(todos_pks),
        "auditoria_pks": todos_pks,
        "auditoria_com_payload_da_via_atual_n": len(coincidentes),
        "auditoria_com_payload_da_via_atual": [
            {
                "pk": item["pk"],
                "usuario_id": item["usuario_id"],
                "criado_em_local": local(item["criado_em"]),
            }
            for item in coincidentes
        ],
        "decisoes_n": len(decisoes),
        "decisoes": [
            {
                "pk": item["pk"],
                "ato_por_id": item["ato_por_id"],
                "ato_em_local": local(item["ato_em"]),
            }
            for item in decisoes
        ],
        # Auditoria e best-effort: app/core/observ.py:94,139.
        "completude_historica": "[nao verificado]",
        # DecisaoPergunta nao tem via_resolucao:
        # app/chamados/models.py:3872.
        "vinculo_das_decisoes_com_a_via": "[nao verificado]",
    }


captura = Captura()

# Impede saidas livres dos chamadores; somente o JSON final sera impresso.
with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
    try:
        with transaction.atomic():
            with connection.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute("SELECT current_user, current_schema()")
                if cur.fetchone() != ("leitor", "juliani"):
                    raise RuntimeError("contexto_incorreto")

            with somente_capturar_logs(captura):
                from django.db.models import Q
                from django.utils import timezone

                from chamados.juizes import (
                    alimentar_portas,
                    fato_do_marco_no_espelho,
                    pergunta_viva,
                    porta_alcancavel_do_dia,
                    um_colaborador_por_identidade,
                )
                from chamados.models import DecisaoPergunta, PerguntaDisputa
                from chamados.services.fabrica_da_celula import motivos_a_perguntar
                from chamados.services.vigia_fio import (
                    ListaDoAdmin,
                    dono_fora_do_fio,
                )
                from colaboradores.models import Colaborador
                from colaboradores.services.calendario import (
                    STATUS_QUE_PEDEM_DECISAO,
                    contexto_calendario,
                    resolver_dia_oferecido,
                    resolver_dia_oferecido_nas_datas,
                )
                from core.models import LogAuditoria
                from escala.models import CelulaDia
                from ponto.janelas import janela_atual
                from ponto.status_dia import status_do_dia_lavrado

                agora = timezone.now()
                hoje = timezone.localdate(agora)

                filtro = Q(pk__in=[])
                for cid, datas in DATAS.items():
                    filtro |= Q(colaborador_id=cid, data__in=datas)

                colabs = Colaborador.objects.select_related("empresa").in_bulk(
                    list(DATAS)
                )
                celulas = list(
                    CelulaDia.objects.filter(filtro).select_related("colaborador")
                )
                por_dia = {
                    (cel.colaborador_id, cel.data): cel for cel in celulas
                }

                # Mesma alimentacao compartilhada do censo:
                # app/chamados/services/vigia_fio.py:604.
                alim = chamar(
                    alimentar_portas,
                    [cel.pk for cel in celulas],
                    colabs.values(),
                )
                todas = chamar(
                    um_colaborador_por_identidade,
                    PerguntaDisputa.objects.filter(
                        celula_id__in=[cel.pk for cel in celulas]
                    )
                    .select_related("disputa", "celula")
                    .order_by("pk"),
                    alim["instancias"],
                )
                por_celula = defaultdict(list)
                for p in todas:
                    por_celula[p.celula_id].append(p)

                lista = ListaDoAdmin(hoje)
                resolver_cache = {}
                dias = []

                for cid, data in PARES:
                    chamada = {"etapa": "carregar_dia"}
                    try:
                        # Isola erro SQL do dia; erro nunca vira resultado zero.
                        with transaction.atomic():
                            colab = colabs[cid]
                            cel = por_dia[(cid, data)]
                            cel.colaborador = colab
                            perguntas = por_celula.get(cel.pk, [])

                            porta = chamar(
                                porta_alcancavel_do_dia,
                                cel.pk,
                                alimentacao=alim,
                            )

                            # Plural por colaborador, como a rodada do censo:
                            # app/chamados/services/vigia_fio.py:258.
                            if cid not in resolver_cache:
                                try:
                                    resolver_cache[cid] = chamar(
                                        resolver_dia_oferecido_nas_datas,
                                        colab,
                                        DATAS[cid],
                                    )
                                except Exception as exc:
                                    resolver_cache[cid] = (
                                        exc,
                                        dict(chamada),
                                    )
                                    raise

                            resolucao = resolver_cache[cid]
                            if isinstance(resolucao, tuple):
                                exc, origem = resolucao
                                chamada = dict(origem)
                                raise exc
                            oferecido = resolucao[data]

                            linha = {
                                "col": "col%d" % cid,
                                "data": data.isoformat(),
                                "celula_pk": cel.pk,
                                "chave_perguntas": "celula_id",
                                "porta_alcancavel_do_dia": porta,
                                "resolver_dia_oferecido_nas_datas": oferecido,
                                "vivas_lidas_na_alimentacao_pks": sorted(
                                    p.pk
                                    for p in alim["vivas"].get(cel.pk, ())
                                ),
                                "motivos_a_perguntar": chamar(
                                    motivos_a_perguntar,
                                    cel,
                                    hoje,
                                    agora=agora,
                                ),
                                "perguntas": [],
                            }

                            for p in perguntas:
                                viva = chamar(pergunta_viva, p)
                                marco = chamar(fato_do_marco_no_espelho, p)
                                linha["perguntas"].append({
                                    "pk": p.pk,
                                    "disputa_pk": p.disputa_id,
                                    "celula_pk": p.celula_id,
                                    "data_evento": p.data_evento,
                                    "motivo": p.motivo,
                                    "via_resolucao": p.via_resolucao,
                                    "respondida_em_local": local(p.respondida_em),
                                    "validada_em_local": local(p.validada_em),
                                    "validada_por_id": p.validada_por_id,
                                    "pergunta_viva": viva,
                                    # Preserva True/False/None:
                                    # app/chamados/juizes.py:930.
                                    "fato_do_marco_no_espelho": marco,
                                    "autoria": evidencia_autoria(p),
                                })

                            # Mesma condicao de entrada nesse ramo do censo:
                            # app/chamados/services/vigia_fio.py:642,648,654.
                            chamar_dono = porta is None and data < hoje
                            linha["dono_fora_do_fio_chamado"] = chamar_dono
                            if chamar_dono:
                                linha["dono_fora_do_fio"] = chamar(
                                    dono_fora_do_fio,
                                    colab,
                                    data,
                                    perguntas,
                                    lista,
                                    com_papel=True,
                                    oferecido=oferecido,
                                )

                            if cid == 427 and data == dt.date(2026, 9, 21):
                                d4 = chamar(
                                    status_do_dia_lavrado,
                                    colab,
                                    data,
                                    agora=agora,
                                )
                                ctx = chamar(
                                    contexto_calendario,
                                    colab,
                                    {
                                        "modo": "custom",
                                        "data_ini": data.isoformat(),
                                        "data_fim": data.isoformat(),
                                        "mes": str(data.month),
                                        "ano": str(data.year),
                                    },
                                )
                                dia_tela = next(
                                    item
                                    for item in ctx["dias"]
                                    if item["data"] == data
                                )
                                direto = chamar(
                                    resolver_dia_oferecido, dia_tela, hoje
                                )
                                if direto != oferecido:
                                    raise RuntimeError(
                                        "resolver_plural_diverge_da_tela"
                                    )

                                linha["D4"] = {
                                    "status": d4["status"],
                                    "fonte": d4["fonte"],
                                }
                                # O juiz nao retorna motivo separado. Publica
                                # operandos reais e sua expressao original,
                                # sem implementar outro predicado explicativo:
                                # app/colaboradores/services/calendario.py:97.
                                linha["motivo_do_falso_operandos_do_juiz"] = {
                                    "retorno_real": direto,
                                    "pode_ultimo_recurso": (
                                        dia_tela["pode_ultimo_recurso"]
                                    ),
                                    "data": dia_tela["data"],
                                    "hoje": hoje,
                                    "status_D4_na_tela": dia_tela["status"],
                                    "status_que_pedem_decisao": list(
                                        STATUS_QUE_PEDEM_DECISAO
                                    ),
                                    "janela_atual": chamar(
                                        janela_atual, hoje, colab.empresa
                                    ),
                                    "regra": inspect.getsource(
                                        resolver_dia_oferecido
                                    ),
                                    "fonte": (
                                        "app/colaboradores/services/"
                                        "calendario.py:97"
                                    ),
                                    "causa_exclusiva_sem_exame_dos_operandos": (
                                        "[nao verificado]"
                                    ),
                                }

                            if timezone.localdate() != hoje:
                                raise RuntimeError("relogio_cruzou_data_local")

                            linha["ok"] = True
                            transaction.set_rollback(True)

                        dias.append(linha)

                    except Exception as exc:
                        # Descarta a medicao parcial; publica erro do dia.
                        dias.append({
                            "col": "col%d" % cid,
                            "data": data.isoformat(),
                            "ok": False,
                            **erro_seguro(exc),
                        })

                # Classes observadas de perguntas MORTAS pelo juiz real.
                # Um dia pode ter mais de uma classe; nao somar os dias entre
                # classes como

[truncado em 20000 de 23818 caracteres]

## 12/11 11:41 · sessao e7dc53643837 · - · - · origem=Ronald (tmux)

Censo AST executado em memória: 767 arquivos Python de produção em `app`, excluindo testes, migrations, assets e logs; zero falhas de parse. Reutilizei por extração AST o detector de `app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, com campos ampliados; ele cobre atribuição, `setattr`, dicionários propagados, construtor, `update` e `bulk_update`, mas declara limites para nomes vindos de outro módulo e `__dict__` em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`. Execução de testes e confirmação no banco: [nao verificado].

**Escritores físicos encontrados**

| Campo | Atribuição e persistência |
|---|---|
| `via_resolucao` | `_escrever_carimbo`: `app/chamados/models.py:3102`; persistido por `carimbar` em `:3153`, `carimbar_identidade` em `:3191` e `descarimbar` em `:3213`. |
| `via_resolucao` em lote | `PerguntaDisputa.objects…update` em `app/chamados/models.py:3265`, com via vazia, materialização e autor nulos em `:3267`. |
| `via_resolucao` isolada | `encerrar_por_fato_no_espelho`, atribuição/save em `app/chamados/models.py:3276`; `relavrar`, atribuição/save em `app/chamados/services/relavra_via.py:125`. |
| `respondida_em` | `carimbar`, atribuição em `app/chamados/models.py:3133`, save em `:3153`; resposta humana em `app/chamados/services/disputa_emissao.py:1468`, save em `:1508`; automática em `:1653`, save em `:1655`; recuperação pela trilha em `:1674`, save em `:1676`. |
| `validada_em`/`validada_por` | Simetria de `carimbar` em `app/chamados/models.py:3136`, save em `:3153`; ato de validar em `app/chamados/services/validacao.py:134`, save em `:137`; devolução da validação em `:199`, save em `:202`; resposta nova limpa ambos em `app/chamados/services/disputa_emissao.py:1480`, save em `:1508`. |
| Limpeza dos três campos de resposta/validação | `ressuscitar_perguntas` passa campos explícitos a `descarimbar_em_lote` em `app/chamados/models.py:1567`; `reabrir_para_correcao` monta `campos` em `app/chamados/services/disputa_emissao.py:1593` e chama a mesma porta em `:1630`. |
| Alterações somente em memória | Cópia para consultar o C6 em `app/chamados/models.py:1507`; restauração da tentativa recusada em `:3163`. |

Não apareceu outro `bulk_update` nem outro `QuerySet.update` desses campos no universo acima [nao verificado como garantia além dos limites do detector]. `flip_auto.py:128` é **leitor** por `filter`, não escritor; `competencia_trancada.py:126` determina saída por leitura e `:255` apenas reabre o legado.

**Apelidos e reexports**

- Reabertura: `_reabrir` em `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2` em `app/chamados/views.py:1435`.
- `PerguntaDisputa` como `_PD`: `app/colaboradores/models.py:432`, `app/chamados/veredito_lavrado.py:201`, `app/chamados/views.py:398`, `app/colaboradores/services/calendario.py:469`, `app/ponto/services/cartorio.py:1635`, `app/chamados/services/limbo_folgas.py:270`, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/raiox.py:177` e `app/chamados/services/fio.py:392`, `:768`, `:883`.
- `_PD_ordem`: `app/ponto/services/declarar_dia.py:189`, `app/ponto/services/veredito_celula.py:225`, `app/chamados/services/validacao.py:428`; `_PD_inf`: `app/chamados/services/painel.py:260`.
- Reexports explícitos dos escritores pelo modelo: `app/chamados/models.py:4088` e `:4093`. Importação `*` de `chamados` na produção varrida: não encontrada [nao verificado fora desse universo].

**Matriz dos caminhos principais**

| Entrada → juiz → operação | Persistência → retorno → prova aberta |
|---|---|
| Pergunta sem validação → `carimbar` consulta C6 **depois de aplicar carimbos**, `app/chamados/models.py:3130` e `:3140` | Trio materialização/via; havendo texto, `resposta='validar'` preenche resposta e validação antes do C6, `:3131`; save `:3153`, `True` em `:3158`; viva desfaz e retorna `False`, `:3144`. Testes de recusa/restauração: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:687`, `:713`. |
| Admin decide/silêncio → não consulta C6 na porta de identidade, `app/chamados/models.py:3169` | `fechar_por_admin` escolhe `admin_decidiu`, `:3065`; trio em `:3187`, save/`True` em `:3191`; idempotente `False` em `:3056`. Fecho explícito da disputa grava `DecisaoPergunta.SILENCIO`, `:1920`; prova `app/chamados/tests/test_e1_entradas.py:121`. |
| Dia decidido pelo admin → lista `q_vivas` daquele dia, `app/ponto/services/veredito_celula.py:180` | `fechar_por_admin` em `:190`; linha `SILENCIO` em `:195`. Prova real e idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| Folga aceita → juiz `o_dia_ja_e_folga` ou `checar_declaracao`, `app/chamados/services/validacao.py:280`, `:305` | Validação + `admin_decidiu`, `:288`, `:320`, `:329`; retorna `folga_ja_declarada` em `:302` ou `folga_declarada` em `:367`; recusa posterior reverte transação em `:343`. Provas nomeadas em `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:136` e `:197`; corpo não aberto [nao verificado]. |
| Motivo removido → pertence aos motivos atuais e disputa aberta, `app/chamados/models.py:2034` | Linha `RETRATACAO` em `:2057`, identidade `motivo_retratado` em `:2062`, conserva carimbo anterior; retorna `removido` em `:2068`. Prova aberta preserva resposta e constata morte: `app/chamados/tests/test_e1_entradas.py:142`. |
| Duplicata → `plano_de_retratacao`, `app/chamados/services/fio.py:1341` | `retratar_duplicata` grava `duplicata`, conserva carimbo, anota canônica em `:1360`; `True` em `:1364`, já duplicata `False` em `:1351`. Prova aberta apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| Paridade recusada → exige lâmpada `is True`, `app/chamados/services/materializacao.py:906` | Identidade `paridade_recusada` em `:930`, retorna `None` em `:931`; apagada apenas recusa e retorna `None`, `:919`. Provas abertas dos três valores: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:175`, `:183`, `:192`. |
| Colisão tipo oposto → `traduzir_recusa`, `app/chamados/services/materializacao.py:1097` | Sem materialização e código `COLISAO`, identidade `fato_ocupado` + motivo em `:1105`; sem teste local da lâmpada. Retorno implícito `None`, função termina em `:1113`; prova específica [nao verificado]. |
| Ausência declarada → `declarou_hora as _declarou_hora`, `app/chamados/services/materializacao.py:548`; C6 dentro da porta | Irmãs não materializadas: `declaracao` ou `ausencia_dia`, `:555`; `carimbar(...resposta='validar')`, `:567`; se gravou, linha `ABSORCAO`, `:569`; se recusou, evento e lista de recusadas, `:573`. Prova de recusa aberta apenas por localização: `app/chamados/tests/test_o261e_f2_fila.py:238` [nao verificado corpo]. |
| Materializador → juiz de lastro após plantio, `app/chamados/services/materializacao.py:1240`; via por `_declarou`/`_via_fato`, `:1267` | `carimbar` com simetria da validação e lavra, `:1283`; C6 recusa → `_Reprovada`, rollback, `:1285`. Prova específica de rollback localizada em `app/chamados/tests/test_hx_materializa_depois_julga.py:30` [nao verificado corpo]. |
| Fecho de disputa, pendentes aprovadas → lastro em `app/chamados/services/materializacao.py:1433`, C6 na porta | `juiz_aprovou` em `:1437`, devolve lista em `:1439`; prova aberta barra resposta não validada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| Sem turno aberto → motivo limitado, sem texto/materialização e núcleo sem turno, `app/chamados/reconciliador.py:363`; C6 antes em `:398`, novamente pela porta em `:406` | Trio com `sem_turno_aberto`; eventual fecho da disputa por C7 em `:412`; retorna `True`, `:415`. Prova aberta **com marco apagado e validação prévia**: `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`; construção da lâmpada apagada em `:115`. |
| Célula absolveu → `perguntas_absolvidas_pela_celula`, `app/chamados/reconciliador.py:585`; C6 na porta | `celula_absolveu`, `:596`; contabiliza e eventualmente fecha por C7, `:612`. Prova localizada em `app/chamados/tests/test_selo_celula_absolveu.py:66` [nao verificado corpo]. |
| Fantasma atual → `veredito_orfa == ENCERRA`, `app/chamados/reconciliador.py:1108` | Consumidor chama `encerrar_fantasma`; porta passa pelo C6, `app/chamados/models.py:3288`. Provas abertas: barra viva respondida em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`; aceita turno fechado em `:676`. |
| Fato materializado → comando exige lâmpada `is True`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:41` | Só troca via, `app/chamados/models.py:3276`; retorna `True`, `:3278`; não é via cega nesse chamador. Prova específica [nao verificado]. |
| Re-lavra de carimbo derivado → `declarou_hora`/`via_pelo_fato`, `app/chamados/services/relavra_via.py:95` | Altera via e lavra no mesmo save `:125`; retorna `(n_via,n_ver)`, `:139`; não exige lâmpada localmente. Prova específica [nao verificado]. |

**Divergência comprovada para decisão**

O `carimbar` exige “C6 deu morta”, mas o C6 pode responder sobre a **validação que o próprio carimbar acabou de preencher**, antes de conferir o efeito do marco: `app/chamados/models.py:3135` e `:3140`. A outra leitura é a lâmpada de `CLAUDE.md:169`. O teste `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148` comprova a variante já validada: lâmpada de entrada apagada, via `sem_turno_aberto` persistida e disputa fechada. O teste de paridade explicita que validada sai do fio mesmo sem absolvição por fato, `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:210`. Cabe decisão numerada na consolidação, sem escolher entre essas leituras.

**Cobertura no backlog aberto**

- E4 cobre o conjunto de portas de estado, com E4b backfill, E4c leitores e E4d parada da projeção: `app/docs/BACKLOG.md:1885`.
- Identidade/duplicata tem fato próprio adiado para depois da E4d: `app/docs/BACKLOG.md:1889`; excedentes duplicados estão explicitamente em E1/E4: `:1912`.
- Reação da raiz aos carimbadores e fecho fica em RAIZ F3/F4; F5 se combina com E4d: `app/docs/BACKLOG.md:1904`; os sete carimbadores sem C7 estão nomeados em `:1911`.
- `celula_absolveu` pelo dia errado tem complemento específico **ABSOLVIDA-PELA-FOLGA**, E1-ENTRADAS, com pergunta #51407: `app/docs/BACKLOG.md:1935`. Isso não demonstra a classe dos outros 14 dias [nao verificado].

## 12/11 11:40 · sessao 92e371f1103f · - · - · origem=Ronald (tmux)

BLOQUEIA — `app/chamados/services/explicador.py:137` duplica C7 incompleto: define `pode_fechar = not fio_vivo(ps)`, mas C7 exige também ausência de respostas sem veredito (`app/chamados/models.py:1866`). Contraexemplo: pergunta com texto e `respondida_em`, sem validação/materialização, célula `ok` decidida pelo admin; C6 pode considerá-la morta (`app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`), enquanto a guarda mantém a resposta pendente (`app/chamados/models.py:1855`). Explicador publica “Tudo resolvido” (`app/chamados/services/explicador.py:140`), modal oferece fechar/regularizar (`app/templates/chamados/partials/modal_fio.html:446`), e fechamento recusa (`app/chamados/models.py:1899`).

BLOQUEIA — #9 deixa silêncio vencido com significados diferentes: fila e lista do colaborador excluem somente futuro (`app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`), mas a partição classifica qualquer prazo vencido como `HISTORICO` (`app/chamados/services/verbos.py:48`), e a lista padrão administrativa o exclui (`app/chamados/services/painel.py:324`). Contraexemplo: chamado aberto com silêncio vencido aparece ao colaborador e entra na fila, porém desaparece da lista padrão do admin; a tarefa ainda remove esse chamado de sua `fila` publicada (`app/chamados/services/tarefa_do_dia.py:551`). Os testes novos verificam `qs_na_fila` e `chamados_visiveis_colab`, sem verificar a lista efetiva do painel (`app/chamados/tests/test_e3_b4_silencio_vencido.py:36`).

FILA — `respondida_em`: escritor de resposta humana em `app/chamados/services/disputa_emissao.py:1468`, com projeção e decisão no mesmo ato (`app/chamados/services/disputa_emissao.py:1503`); escritores adicionais de resposta automática e carimbo em `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`; leitores relevantes C6 (`app/chamados/juizes.py:1375`), aviso corrigido (`app/chamados/services/fio.py:174`) e crédito ainda por timestamp (`app/chamados/services/raiox.py:139`). Contraexemplo: resposta com timestamp e texto vazio vira crédito de respondida no painel, mas o aviso continua esperando o colaborador (`app/chamados/services/raiox.py:139`, `app/chamados/services/fio.py:174`).

FILA — `silenciado_ate`: escritores conferidos `arquivo.silenciar` e `arquivo.levantar` (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`); instante futuro estrito, igualdade significa vencido (`app/chamados/models.py:512`, `app/chamados/models.py:524`). Contraexemplo de leitor divergente: prazo vencido permanece histórico na partição (`app/chamados/services/verbos.py:51`).

FILA — `data_evento`: atribuição direta única encontrada em `app/chamados/services/fio.py:819`; nascimento usa essa porta (`app/chamados/models.py:3029`), costura também (`app/chamados/services/fio.py:898`) e rechaveamento também (`app/chamados/services/fio.py:1056`). Campo significa dia do turno para fato (`app/chamados/models.py:2796`) e dia civil para conversa (`app/chamados/models.py:2808`); C19 separa `conversa=False` (`app/chamados/services/tarefa_do_dia.py:375`). Ocorrência material adicional de mistura entre esses significados: nada demonstrado; incidência em produção [nao verificado].

FILA — `status_local`: núcleo persistente em `app/chamados/models.py:228`; nascimento em `app/chamados/models.py:1108`; outras atribuições encontradas simulam e restauram estado em memória (`app/chamados/models.py:356`, `app/chamados/models.py:371`). Segundo escritor direto fora dessas portas: nada encontrado; reflexão e kwargs dinâmicos [nao verificado].

FILA — porta: saída escrita em memória pelo próprio C19 (`app/chamados/services/tarefa_do_dia.py:555`), projetada por `porta_da_tarefa` (`app/chamados/services/tarefa_do_dia.py:565`); não há persistência nesse trecho (`app/chamados/services/tarefa_do_dia.py:540`). Contraexemplo material: silêncio vencido pode manter `rota=porta_fila` enquanto `fila=()` porque `da_celula` é calculado antes da exclusão de `HISTORICO` (`app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:555`).

FILA — ordem de trava: nada novo material encontrado nos caminhos conferidos; núcleo de status trava universo antes de salvar (`app/chamados/models.py:280`), silêncio pede a porta de ordem (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`), rechaveamento trava antes da releitura/escrita (`app/chamados/services/fio.py:1025`). Ordem implementada C→D→CH→P em `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`; cobertura global/transitiva [nao verificado].

## 12/11 11:39 · sessao bf78c34d5db7 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 5, respondedores vivos: `raiox.creditos_perguntas` ainda separa respondidas por timestamp (`app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`); `relatorios_disputa.classificar` decide dono por C6 + timestamp (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`); `_calados` usa `q_vivas` + timestamp nulo (`app/chamados/services/cobranca.py:229`); cartão deriva documento/formulário/espera no template (`app/templates/chamados/partials/_card_atendimento.html:65`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`). Contraexemplo: resposta vazia com timestamp continua tarefa RESPONDER do colaborador (`app/chamados/services/tarefa_do_dia.py:135`), mas relatório diz AGUARDA_SUPERVISAO (`app/chamados/relatorios_disputa.py:23`) e cartão diz “Aguardando a supervisao conferir” (`app/templates/chamados/partials/_card_atendimento.html:101`).

BLOQUEIA — API/badge/PWA não migraram: `consulta_do_app` ainda exige timestamp nulo + disputa aberta para pendente (`app/chamados/services/pergunta_no_app.py:70`); `projecao_do_app` conta essa lista (`app/chamados/services/pergunta_no_app.py:90`). Consumidores: dois kernels (`app/api/views.py:315`, `app/api/views_core.py:303`), API chamados (`app/api/views.py:993`), API perguntas (`app/api/views.py:2533`) e dashboard (`app/core/views.py:252`). Contraexemplo: resposta vazia com timestamp cai no histórico, badge não conta RESPONDER, enquanto tarefa mantém RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); o próprio novo teste identifica essa resposta como ainda devida pelo colaborador (`app/chamados/tests/test_e3_leitores_fio_disputa.py:143`).

BLOQUEIA — tela admin/fio continuam derivando dono por fonte antiga: partição chama `creditos_perguntas` (`app/chamados/services/verbos.py:34`); natureza prioriza resposta creditada como `admin_validar` (`app/chamados/catalogo/natureza.py:48`); fio passa esses créditos ao desenlace (`app/chamados/services/fio.py:405`, `app/chamados/services/fio.py:421`); desenlace imprime “Sua vez: Validar” (`app/chamados/services/desenlace.py:72`). Contraexemplo: resposta vazia timestamp presente produz crédito respondido (`app/chamados/services/raiox.py:139`) e oferece validação onde tarefa exige RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`).

BLOQUEIA — #9 não resolve o consumidor real da fila: silêncio vencido vira HISTORICO na partição (`app/chamados/services/verbos.py:48`), e painel padrão exclui esses ids (`app/chamados/services/painel.py:324`). Ao mesmo tempo, queryset da fila e lista do colaborador devolvem o chamado (`app/chamados/services/painel.py:189`, `app/chamados/services/janela_colab.py:77`). Contraexemplo: chamado aberto com `silenciado_ate=agora−1min` aparece para colaborador e no contador COBRANCA (`app/chamados/services/painel.py:212`), mas não na lista padrão administrativa (`app/chamados/services/painel.py:331`). RED #9 verifica somente os dois querysets (`app/chamados/tests/test_e3_b4_silencio_vencido.py:35`), portanto não morde essa divergência de apresentação.

BLOQUEIA — porta da tarefa pode publicar fila vazia sob silêncio vencido: `alimentar_fila` inclui chamado vencido em `fila_ids` (`app/chamados/juizes.py:1533`); `_portas_da_tarefa` mantém esse inteiro em `portas` (`app/chamados/services/tarefa_do_dia.py:548`), exclui HISTORICO apenas de `fila` (`app/chamados/services/tarefa_do_dia.py:551`) e publica rota porque `portas` permanece preenchido (`app/chamados/services/tarefa_do_dia.py:555`). Contraexemplo: pergunta exigível ancorada no chamado vencido da célula gera `porta_fila`, `fila=()`, enquanto lista admin esconde a âncora (`app/chamados/services/painel.py:331`).

BLOQUEIA — item 6, selo não cobre o universo prometido: catraca exige somente um chamador C17 (`app/chamados/tests/test_e3_pendentes_app.py:145`); seu mutante acrescenta chamada C17 (`app/chamados/tests/test_e3_pendentes_app.py:153`). Contraexemplo: todos os leitores próprios acima permanecem e a catraca não os busca, pois o detector só reconhece o alvo literal resolvido `chamados.juizes.porta_alcancavel_do_dia` (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:89`).

BLOQUEIA — leitor administrativo adicional não migrou: calendário consulta C6 sem relógio recebido (`app/colaboradores/services/calendario.py:476`), exclui texto vazio antes do juiz (`app/colaboradores/services/calendario.py:474`) e calcula `pode_ultimo_recurso` separadamente (`app/colaboradores/services/calendario.py:738`), consumido por `resolver_dia_oferecido` (`app/colaboradores/services/calendario.py:103`). Contraexemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto tarefa publica RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); incidência real [nao verificado].

BLOQUEIA — copiloto permanece com derivação e relógio próprios: `dia_do_colab` publica C6 sem `agora` (`app/ponto/services/dia_do_colab.py:71`) e delega retenção ao antigo `do_dia` (`app/ponto/services/dia_do_colab.py:108`), que lê `timezone.localdate`, VIVOS, universo do app e `retencao` (`app/chamados/services/pergunta_no_app.py:217`, `app/chamados/services/pergunta_no_app.py:223`). Contraexemplo: pergunta viva respondida com texto segue descrita genericamente como esperando “resposta ou decisao” (`app/ponto/services/dia_do_colab.py:148`), sem publicar tarefa/porta única (`app/ponto/services/dia_do_colab.py:73`); contador de leitores fora da tarefa e pergunta golden solicitados não foram encontrados por busca textual, ausência completa [nao verificado].

FILA — badge navbar mantém unidade chamado em fila (`app/core/context_processors.py:185`), diferente do badge de perguntas pendentes (`app/chamados/services/pergunta_no_app.py:91`); contraexemplo: chamado na fila sem pergunta produz navbar 1, enquanto tarefa pode ter apenas ação do sistema e nenhuma porta, caso já explicitado no novo teste (`app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`). Migração desse sítio permanece sem RED específico identificado [nao verificado].

FILA — `_calados` ainda trata pré-filtro como vida exata (`app/chamados/services/cobranca.py:229`), embora o contrato de `q_vivas` declare seleção mais generosa que C6 (`app/chamados/models.py:3349`). Contraexemplo: pergunta sem resposta em dia trancado por leitura pode entrar em cobrança apesar de morta no C6; incidência e execução [nao verificado].

## 12/11 11:39 · sessao eef0afa7e826 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

- **B1 curado:** resumo e marca usam o mesmo conjunto de instantes. RED na c14 (`0 != 1`) e GREEN após a cura. [Evidência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:6).
- **B2:** selo compara PDF e tela, incluindo a marca de 10/06. A fixture original não executou por ausência de Django; seus números permanecem **[nao verificado]**. [Detalhes](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:16).
- **B3:** mantido o duplo porque o recalculador exige leituras ORM. A prova parcial mata três mutantes; `recalculador_vazio` sobrevive, expondo a lacuna integrada. [Justificativa em três linhas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:24).

**37 testes sem banco e ruff passaram; oito mutantes mortos e um sobrevivente.** Comandos e saídas no [relatório](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:34). [nao verificado]

Sem commit, respeitando a proibição inicial. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:36). [nao verificado]

## 12/11 11:39 · sessao be892ef56153 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 7, instrumento diverge do vigia migrado: `diff_e3_sombra.py:54` calcula `exige_admin` pelos verbos; `vigia_fio.py:650` usa `porta['fio'] == 'admin'`. Contraexemplo: pergunta respondida sem texto, verbo `validar`, fio `colab`: sombra informa admin=True, vigia False; o próprio caso está em `test_o261e_f5_alcance.py:177`, com assert False em `:192`. O teste do instrumento conserva a resposta antiga em `test_e3_diff_sombra.py:143`. [nao verificado]

BLOQUEIA — selo/mutante sem alvo: `e3_mutantes.py:112` procura o antigo `return` da candidata, mas o código agora tem alias em `diff_e3_sombra.py:43`; o harness encerra por `AssertionError('Mutante sem alvo')` em `e3_mutantes.py:162`. Contagem literal por AST em memória confirmou 0 alvos; morte do mutante por teste: [nao verificado]. [nao verificado]

BLOQUEIA — mutante de relógio atinge sítio errado e é equivalente: `e3_mutantes.py:101` troca o primeiro `agora=self.agora)` por `agora=None)`; há dois alvos, e `replace(...,1)` em `:164` alcança C6 em `tarefa_do_dia.py:493`, deixando `alimentar_fila` em `:533` intacta. C6 recupera `carga.agora` em `juizes.py:883`; portanto não reverte a cura pretendida.

FILA — item 11 não cobre literalmente a disputa inteira quando fechada: `fio.py:241` pré-filtra suas perguntas com `q_vivas`, `:255` descarta mortas antes de preencher o novo mapa em `:257`. Contraexemplo: disputa fechada com uma pergunta viva e outra validada: explicação recebe só a viva; o RED novo monta disputa aberta em `test_e3_leitores_fio_disputa.py:84`. Incidência de produção: [nao verificado].

#4 — cura da alimentação presente em `juizes.py:1575`; C6 recebe o instante, deriva seu dia local em `:885`, e evita o fallback de `:592`. Tarefa já chama C6 com seu instante em `tarefa_do_dia.py:493` e alimenta fila em `:533`. RED e controle em `test_e3_bloqueantes.py:110`/`:129`; verificação de repasse em `:135`. Reversão de `:1575` deve contrariar assert de `:145` por leitura estática; execução e mutante morto: [nao verificado].

#8 — REDs distinguem chamado sem ação, âncora silenciada e registrada em `test_e3_bloqueantes.py:148`, `:155`, `:164`; controle com porta em `:171` e mock da autoridade em `:184`. Reversão ao predicado cru contraria esses asserts por leitura estática; execução/mutante morto: [nao verificado].

#9 — RED vencido, controles vigente/NULL, instante recebido e fronteira exatamente igual em `test_e3_b4_silencio_vencido.py:41`, `:46`, `:50`, `:54`, `:59`. Reversão a NULL contraria o RED de `:43`; teste não exercita `tarefa_do_dia`, apenas os dois querysets (`:35`). Execução/mutante morto: [nao verificado]. [nao verificado]

Item 4 — REDs de folha, pauta e vigia distinguem o C17 anterior em `test_e3_leitores_c17_chamadores.py:124`, `:135`, `:154`, `:170`; controle de não chamada ao C17 em `:183`. Beco possui caso divergente em `test_f3_beco1_folga_na_lista.py:166`. Execução e mutantes por leitor: [nao verificado].

Item 6 — catraca verifica exclusivamente chamadas a C17 em `test_e3_pendentes_app.py:33`/`:145`; injeção demonstra detectar retorno da folha ao C17 em `:150`. Não certifica 0 derivações próprias nem 0 respondedores vivos; sua pergunta concreta é a contagem C17 (`:147`).

Item 11 — caso multicartão e universo completo aberto em `test_e3_leitores_fio_disputa.py:95`; regressão à lista do primeiro cartão contradiz asserts em `:103`/`:105`. Controle todas mortas em `:108`. Execução/mutante morto: [nao verificado].

Item 12 — caso morta sem resposta em `test_e3_leitores_fio_disputa.py:125`; controles viva sem resposta/com texto/sem texto em `:134`, `:138`, `:143`; API em `:156`/`:164`. Reversão ao timestamp cru contraria `:131` e `:146`. Execução/mutante morto: [nao verificado].

PRONTO 1–3/7–10 — o código documental ainda registra E3-LEITORES “1 de 10” em `RELATO.md:77`/`:95`; não há resultado da obra atual nesses trechos. Sonda 232, V/T/X, cura V, queda por classe, suite final, opus e pouso deste HEAD: [nao verificado]. [nao verificado]

## 12/11 11:39 · sessao 43ad31d715f2 · - · - · origem=Ronald (tmux)

Leitura concluída para `disputa_emissao` e encaixe no BACKLOG; testes abaixo foram **abertos, não executados** `[nao verificado]`. [nao verificado]

**Matriz de `disputa_emissao`**

| Entrada → juiz | Operação e persistência | Retorno e prova |
|---|---|---|
| Pergunta apontando ao chamado resolvido, sem `materializada_em`, disputa aberta (`app/chamados/services/disputa_emissao.py:1906`) → ignora pergunta em voo (`:1960`), exige C6 morta (`:1966`), ignora dia trancado (`:1970`); resposta real sem prova externa pertence ao materializador (`:1979`). | `p.carimbar(via_pelo_fato(...), user, resposta='validar', lavrar_veredito=True)` (`:1986`). Persiste trio `materializada_em/materializada_por/via_resolucao`; com texto, completa `respondida_em`, `validada_em/validada_por` nulos; lavra `veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/models.py:3129`, `:3131`, `:3150`). C6 é consultado **com carimbo e validação aplicados**; viva reverte memória e retorna falso (`:3140`). Não planta batida nesta operação (`app/chamados/services/disputa_emissao.py:1889`). | Conta `n_pq`, tenta fechar pelo C7 (`:1989`, `:2091`); retorna dicionário `perguntas_resolvidas/disputas_fechadas` (`:2094`). Prova: célula acusando impede carimbo (`app/chamados/tests/test_fio_le_o_juiz.py:79`); célula absolvendo permite (`:90`); com texto e carimbos nulos S128 grava resposta/validação (`app/chamados/tests/test_materializar_carimba_respondida.py:66`). |
| Disputa aberta do colaborador sem pai vivo e sem destinos vivos (`app/chamados/services/disputa_emissao.py:2027`, `:2033`, `:2036`); pergunta sem materialização/validação (`:2039`) → ignora em voo (`:2045`), exige C6 morta (`:2048`). | Via pelo fato do pai ou `chamado_encerrado` sem pai (`:2058`); `carimbar(... resposta='responder')` (`:2060`). Trio persistido; `respondida_em` apenas se texto, sem validar (`app/chamados/models.py:3129`, `:3131`). Trilha `fecho_sem_credor` (`app/chamados/services/disputa_emissao.py:2063`). | Fecha disputa pelo C7; o contador `perguntas_resolvidas` **não é incrementado neste segundo ramo**, apenas no primeiro (`:1989`, `:2060`, `:2094`). Prova: sem texto fecha com `chamado_encerrado`, sem resposta/validação (`app/chamados/tests/test_o261_resposta_vazia.py:78`); com texto, C6 viva após carimbo faz porta recusar e preserva estado (`:103`). |
| Chamado com `via_encerramento=competencia_trancada` → guarda própria (`app/chamados/services/disputa_emissao.py:1901`). | Nenhum carimbo; morte por leitura da tranca explicitada (`:1898`, `:1968`). | Retorna ambos contadores zero (`:1902`). Prova específica desse retorno não aberta `[nao verificado]`. |
| Resposta humana válida → valida tipo/opções (`app/chamados/services/disputa_emissao.py:1433`), sem exigência de lâmpada nesse escritor. | `resposta_colab/respondida_em`; limpa validação antiga, chama `reabrir_por_resposta`, salva atomicamente e grava `DecisaoPergunta.RESPOSTA` (`:1467`, `:1480`, `:1485`, `:1508`). Escapes ausência disparam materializador (`:1538`). `FOLGA_CONTESTA/OUTRO_MOTIVO` registram recusa, preservando fila admin (`:1486`). | `(True,None)` (`:1559`), erro `(False,texto)` nas guardas (`:1434`). **Respondida não é morte automaticamente**; prova específica resposta viva será a do agente de leitores. Reenvio igual move carimbo e é pergunta de lei já caracterizada (`app/chamados/tests/test_porta_resposta_pergunta.py:196`). |
| Foto entregue por outro caminho, pergunta sem texto → guarda só de texto (`app/chamados/services/disputa_emissao.py:1647`). | `responder_automaticamente`: texto+`respondida_em` e ato resposta (`:1652`). Caller delega validação ao dono (`app/api/foto_recebida.py:39`, `:48`, `:51`). | Booleano (`app/chamados/services/disputa_emissao.py:1658`). Prova idempotência e preservação voz (`app/chamados/tests/test_porta_chamado_e3.py:43`, `:50`). |
| Meio estado texto sem carimbo → guarda (`app/chamados/services/disputa_emissao.py:1670`). | `carimbar_resposta_pela_trilha`: `respondida_em=instante`, save e ato resposta (`:1674`). | Booleano (`:1682`). Prova instante/trilha e exclusões (`app/chamados/tests/test_curar_perguntas_em_limbo.py:29`, `:44`). |
| Reabertura para correção → queryset do juiz `perguntas_corrigiveis` recebido pelo escritor (`app/chamados/services/disputa_emissao.py:1582`). | Limpa `respondida_em/validada_em/validada_por/resposta_colab` (`:1593`); registra foto antes (`:1615`); `desfazer_absorcao=True` usa `descarimbar_em_lote`, caso contrário `qs.update(**campos)` (`:1629`). | Quantidade reaberta (`:1634`). Não mata; escritor de reversão necessário ao censo (`:1575`). Selo citado mas não aberto: `test_reabertura_fantasma.py` `[nao verificado]`. |

**Escolha de via em S128**

`via_pelo_fato` importa `_apur=batidas_apuraveis`, `_aus_cobre=ausencia_cobre`, `_TM=TurnoMaterializado` (`app/chamados/juizes.py:1730`, `:1745`, `:1749`). Resultado:

- Agrupado, sem batida apurável tocando dia civil: `cadastro_folga` se pai LIMBO-FOLGAS; caso contrário `adesao` (`:1713`, `:1733`, `:1738`).
- Ausência aprovada: `ausencia_dia` (`:1746`).
- TM com saída e motivo fora de intervalo: `completude_turno` (`:1754`).
- Restante: `chamado_encerrado` (`:1757`).

Essas são testemunhas diferentes de “marco específico aceso”; a via usa `data_evento` recebida (`:1705`). O carimbo ainda passa pelo C6 acima (`app/chamados/models.py:3140`).

**Silêncio**

A via vem de `DisputaSupervisao.fechar`, não de `disputa_emissao`: exceções `desligamento/admin_encerra` percorrem vivas e chamam `fechar_por_admin`; gravam `DecisaoPergunta.SILENCIO` e trilha com via `admin_decidiu` (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1931`). `admin_encerra` exige motivo (`:1944`); respostas sem veredito recusam todas as portas (`:1887`). Contrato de morte por identidade deve entrar na matriz do agente de escritores (`:1865`).

**Divergências para DECISÃO, sem escolher**

1. C7/C6 podem declarar morta e fechar **validada sem batida no slot**: teste mudou para esperar fechamento, com explicação literal dessa classe (`app/chamados/tests/test_fechamento_unificado.py:74`, `:84`, `:88`); `pode_fechar` consulta resposta sem veredito e `fio_vivo`, não prova de materialização (`app/chamados/models.py:1849`). Outra leitura é a lâmpada marco a marco do aval e `CLAUDE.md:169`. O teste é evidência de desenho atual, não defeito a corrigir por inferência.
2. `chamado_encerrado` pertence a `VIAS_SEM_LASTRO`, cujo comentário diz materializada mas não absolvida (`app/chamados/models.py:3766`), mas ramo sem credor pode persistir exatamente essa via e fechar disputa quando C6 morta (`app/chamados/tests/test_o261_resposta_vazia.py:78`). Leitor deve distinguir materialização administrativa, lastro e viva.
3. Silêncio encerrado mata por `admin_decidiu`, sem consulta de lâmpada no loop que carimba (`app/chamados/models.py:1919`); a regra literal fornecida só permite morte com marco aceso `[nao verificado]` quanto ao alcance que o aval pretende dar às exceções já nomeadas.
4. S128 granular pode fabricar `respondida_em/validada_em` a partir de texto preexistente, na mesma operação do carimbo (`app/chamados/models.py:3131`); o teste cobra os dois carimbos, sem resposta nova do colaborador (`app/chamados/tests/test_materializar_carimba_respondida.py:66`). Leitura como “resposta humana/validação” versus “normalização do encerramento” precisa ficar explícita.

**Aliases e chamadores relevantes**

Reexport compatível de `disputa_emissao` por `chamados.models` em `app/chamados/models.py:4093`–`:4094`, incluindo responder, reabrir e resolver. Web importa core por models (`app/chamados/views.py:1281`), API idem (`app/api/views.py:2619`); web importa `reabrir_para_correcao as _reabrir2` (`app/chamados/views.py:1435`), models como `_reabrir` (`app/chamados/models.py:2220`). S143 importa resolver por models (`app/chamados/signals.py:113`), assim como beco (`app/chamados/services/becos.py:40`) e reconciliador de fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:41`). Não são escritores novos.

**Encaixe aberto no BACKLOG**

- Entradas/respostas/dia do fato: E1-ENTRADAS (`app/docs/BACKLOG.md:1859`); **ABSOLVIDA-PELA-FOLGA** é explicitamente complemento E1 para `data_evento` errado, com RED 51407 e 51467 (`:1935`).
- Autoridade tarefa/C6/tranca: E2 (`:1872`); leitores fio/tela/API/badge/o_que_segura/vigia: E3 (`:1873`).
- Escritores legados: E4-A01..A23; E4b A24/backfill C3; E4c leitores DIVERGE; E4d parar projeção (`:1885`).
- Exceções pós-E4d: A03 reabrir_admin, A05 assinatura, A06 identidade/duplicata, A17/A18 materialização técnica, A20 conferência (`:1887`–`:1892`). Mapeamento exato de cada função para Axx não verificado; `logs/matriz_e4` não foi aberto `[nao verificado]`.
- Reação raiz/filhos/fechamento e autor do fecho: RAIZ F1 matriz, F3/F4 par e zumbi (`:1904`, `:1905`, `:1911`); F5 junto E4d e F6/F7 apenas nomeados (`:1904`), detalhes `[nao verificado]`.
- Duplicatas: complemento E1/E4 depois E4d (`:1912`).
- Série não autoriza inferir qual escritor matou os 15: o dado raspado não contém campos de cada pergunta; classe causal por indivíduo `[nao verificado]`.

## 12/11 11:38 · sessao e4af61514a81 · - · - · origem=Ronald (tmux)

Leituras verificadas na cópia, sem execução de testes. [nao verificado]

- **C6 exato:** `_pergunta_viva_calc` mata por validação (`juizes.py:1391`), tranca lida (`:1395`), vias que vencem resposta com materialização (`:1402`), identidade com materialização (`:1428`), marco materializado aceso (`:1434`), célula não acusadora sem pendentes (`:1452`) e, na exceção do juiz de lastro, retorna `False` (`:1476`). Caminhos completos: `app/chamados/juizes.py:1389`. Resposta com texto vence até o admin decidir o dia ou `_resposta_concorda_com_a_luz` retornar verdadeiro (`:1405`–`:1419`).
- **SQL é aproximação:** `q_vivas()` declara contrato de continência, nunca igualdade com C6: `app/chamados/models.py:3337`; corpo SQL `:3348`–`:3363`. Propriedade `estado` declara validada/resolvida pelos carimbos antes de consultar C6: `app/chamados/models.py:3042`.

Cadeia das portas:

| Leitor | Fonte efetivamente lida |
|---|---|
| Painel/fila | `qs_na_fila` = `qs_vivos` + status COBRANÇA − silêncio futuro: `app/chamados/services/painel.py:188`. |
| Verbo do painel | `particionar_verbos` chama `creditos_perguntas`: `app/chamados/services/verbos.py:34`; crédito usa `q_vivas` e exclui materializada com `VL_SIM`: `app/chamados/services/raiox.py:74`, `:135`. Com resposta → validar, com pergunta → cobrar, sem ambos → decidir: `app/chamados/catalogo/natureza.py:41`, `:58`. |
| Alimentação C17 | Fila/verbos + perguntas efetivamente vivas por C6: `app/chamados/juizes.py:1566`, `:1571`–`:1575`. |
| C17 `porta_fila` | Chamado da célula na fila basta: `_fila` preenche `_verbos` antes de olhar perguntas, `app/chamados/juizes.py:1647`. |
| C17 `porta_pergunta` | C6 viva + âncora na fila + texto não vazio quando respondida + presente no app: `app/chamados/juizes.py:1649`. |
| `censo_alcance` | Aceita a porta de C17 e dá `continue`, sem conferir botão: `app/chamados/services/vigia_fio.py:635`, `:642`–`:647`. Consulta Resolver dia **somente quando C17 devolveu None**: `:636`. |
| `tarefa_do_dia` | Ações pela última `DecisaoPergunta`, luz vigente/efeito; validação sem efeito → materializar/sistema, silêncio → nenhuma ação: `app/chamados/services/tarefa_do_dia.py:116`, `:141`–`:151`. Só publica portas inteiras já exigidas pelas ações, intersectadas com fila: `:538`–`:554`; fila residual não cria tarefa. |
| Calendário | Usa C6 com apelido `_pv`, apenas perguntas de resposta não vazia, para barrar último recurso: `app/colaboradores/services/calendario.py:468`, `:472`–`:477`. Último recurso exige dia anterior ao início da competência atual: `:738`. Resolver exige último recurso + passado + status D4 em falta/irregular/aberto/sem_certidao: `:94`, `:97`–`:104`. Status vem D4: `:663`. |

**Onde fica decidir sem ação:** natureza transforma ausência de crédito em decidir (`app/chamados/catalogo/natureza.py:54`, `:61`); C17 chama isso porta_fila mesmo sem perguntas (`app/chamados/juizes.py:1647`); censo encerra a classificação nesse ramo (`app/chamados/services/vigia_fio.py:642`). Competência corrente impede último recurso (`app/colaboradores/services/calendario.py:738`); em competência encerrada, D4 fora dos quatro status também impede (`:94`, `:103`). Qual status específico do col427 21/09: **[nao verificado]**, dado raspado não contém.

**O admin vê:**

- Pilula Decidir com contagem: `app/templates/chamados/painel_gestao.html:77`.
- Na lista, ramo decidir mostra apenas o badge, enquanto validar/cobrar têm botões próprios: `app/templates/chamados/partials/_lista_chamados.html:81`–`:86`. A linha abre fio ao clicar: `:42`.
- Calendário mantém badge clicável do chamado: `app/templates/colaboradores/partials/_calendario_grade.html:108`. Resolver e DP do dia só existem no ramo `pode_veredito and resolver_dia_oferecido`: `:125`.
- Não é correto concluir por código que todo modal desses 15 fica sem botão. Em disputas abertas, `fio` carrega todas as perguntas; C6 só filtra as fechadas: `app/chamados/services/fio.py:221`, `:235`–`:239`. `todas_respondidas` usa `respondida_em` cru e vira True quando a lista está vazia: `:339`–`:345`. Com aberta e todas respondidas, modal oferece Regularizar disputa e fechar: `app/templates/chamados/partials/modal_fio.html:325`, `:352`–`:359`; fechada mostra “Disputa fechada”: `:376`. Modal exato dos 15: **[nao verificado]**.

Decisões sugeridas para consolidação, sem escolher:

1. **Porta-presença versus porta-ação:** C17 aceita fila sem pergunta (`juizes.py:1647`); tarefa exige ação antes da porta (`tarefa_do_dia.py:545`). Testes preservam ambas leituras: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138` espera porta_fila/decidir; `app/chamados/tests/test_e3_portas_tarefa.py:32` espera rota None/sistema/emissor para fila sem pergunta.
2. **“Viva” SQL versus C6:** crédito do painel usa SQL (`raiox.py:74`), apesar de contrato SQL≠C6 (`models.py:3337`); C17 usa C6 para existência da porta pela pergunta (`juizes.py:1573`). Pode alterar verbo/ação, sem afirmar ocorrência nos 15 **[nao verificado]**.
3. **Carimbo encerrador versus efeito:** C6 validada morre antes da luz (`juizes.py:1391`); tarefa validada sem efeito exige materializar (`tarefa_do_dia.py:145`). `estado` pode dizer resolvida (`models.py:3042`); modal imprime ✓ Validada (`modal_fio.html:228`).
4. **“Todas respondidas” versus pergunta viva/ação:** fio usa timestamp cru (`fio.py:340`); tarefa separa resposta vazia, silêncio, validação sem efeito (`tarefa_do_dia.py:133`). BACKLOG explicitamente inclui correção do ramo no complemento E3 caso7445/cartão35602/pergunta51407: `app/docs/BACKLOG.md:1873`.
5. **Decisão do dia D4 versus alcance:** calendário só oferece Resolver pelos quatro status (`calendario.py:94`), mas censo aceita decidir sem consultar esse resultado (`vigia_fio.py:642`). Sem status do col427, o veto concreto segue **[nao verificado]**.

Provas abertas, não rodadas:

- C6 validada, identidade, célula absolvida, admin: `app/chamados/tests/test_pergunta_viva.py:17`, `:31`, `:39`, `:53`.
- C6 tranca pendente/respondida: `app/chamados/tests/test_viva_delega_ao_juiz.py:138`, `:143`.
- Fila sem pergunta porta_fila: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`; teste puro equivalente `app/chamados/tests/test_e3_pendentes_app.py:160`.
- Tarefa sem pergunta não publica fila residual: `app/chamados/tests/test_e3_portas_tarefa.py:32`; isento com fila residual: `:57`.
- Resolver por D4, incluindo folga/ok recusados: `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`.
- Competência corrente sem Resolver: mesmo arquivo `:94`.
- Calendário lê C6 exato: `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:51`, `:61`.

Encaixe documental: E2 função `app/docs/BACKLOG.md:1872`; E3 migra fio/tela/admin/API/badge/vigia e inclui caso35602 `:1873`; E4b/E4c/E4d=F5 na ordem `:1761`. Não atribuí cura nova nem inferi estado de produção.

## 12/11 11:38 · sessao 131250998015 · - · - · origem=Ronald (tmux)

- Campos reais da pergunta: `respondida_em` e `validada_em` em `app/chamados/models.py:2449`; `validada_por_id` em `app/chamados/models.py:3378`; `materializada_em` e `materializada_por_id` em `app/chamados/models.py:3394` e `app/chamados/models.py:3398`; `via_resolucao` em `app/chamados/models.py:2562`.
- O carimbo grava autor, instante e via juntos em `app/chamados/models.py:3098`. Porém `carimbar_identidade(manter_carimbo=True)` preserva autor/instante anteriores ao trocar a via (`app/chamados/models.py:3183`), e `relavra_via` troca a via sem trocar aqueles campos (`app/chamados/services/relavra_via.py:123`). Portanto, rotular o campo como “autor do carimbo persistido”; autoria da via atual apenas pelo campo é `[nao verificado]`.
- Auditoria disponível: `LogAuditoria`, com `usuario_id`, `modelo`, `objeto_id`, `valor_antes`, `valor_depois`, `criado_em` (`app/core/models.py:285`). Query que não carrega descrição:

```python
logs = list(
    LogAuditoria.objects.filter(
        modelo="PerguntaDisputa", objeto_id=p.pk
    ).order_by("pk").values("pk", "usuario_id", "criado_em")
)
logs_via = list(
    LogAuditoria.objects.filter(
        modelo="PerguntaDisputa",
        objeto_id=p.pk,
        valor_depois__via_resolucao=p.via_resolucao,
    ).order_by("pk").values("pk", "usuario_id", "criado_em")
)
```

- A correlação por `valor_depois__via_resolucao` corresponde aos escritores de auditoria em `app/chamados/services/relavra_via.py:132` e `app/chamados/models.py:1928`. Apresentar contagem e PKs; se necessário, autoria por `usuario_id` e horário local. Não concluir que o último log correspondente prova autoria histórica completa: a auditoria é `best-effort` (`app/core/observ.py:94`) e pode faltar (`app/core/observ.py:139`).
- Existe também `DecisaoPergunta`, ligada por `pergunta_id`, com `ato`, `ato_em` e `ato_por_id`; não possui coluna `via_resolucao` (`app/chamados/models.py:3872`). Query sem texto:

```python
decisoes = list(
    DecisaoPergunta.objects.filter(pergunta_id=p.pk)
    .order_by("pk")
    .values("pk", "ato_por_id", "ato_em")
)
```

- Nunca projetar `LogAuditoria.descricao` (`app/core/models.py:299`), nem `DecisaoPergunta.ato_texto` ou `ato_motivo` (`app/chamados/models.py:3876`). A lista de decisões evidencia trilha da pergunta; ligação de qualquer decisão específica à via atual é `[nao verificado]`.

## 12/11 11:30 · sessao 1cbff62e7def · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

**1. O diff não realiza literalmente o item 2.** O aval diz “para só em DESENHO”, mas o disparo continua dependendo da quantidade de reprovações, sem consultar sua classe; inclusive a chamada que dispara retorna PARADA antes de examinar liberações. A classe aparece apenas na nova linha `AUTOLIBERA`. Referências: `aval_auditoria_de_travas_corte_1211.txt:4` do anexo; `bin/disjuntor.py:652`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1028`, `bin/disjuntor.py:763`.

A exclusividade da segunda ocorrência também não está garantida: a cota protege somente `tipo == 'auto'`. O Code pode liberar pela antiga `ERRO-BOBO`, sem cota nem prova de autoria Ronald; o próprio teste exige essa saída. Isso corresponde à preservação das portas antigas pelo contrato, mas diverge do aval literal. Referências: `contrato_item2.txt:2` do anexo; `bin/disjuntor.py:757`, `bin/disjuntor.py:1044`, `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`.

Há outra divergência textual: o contrato pede “só 1 FALHA por sha recusado”; implementação e teste usam **SOBE**. Referências: `contrato_item2.txt:4` do anexo; `bin/disjuntor.py:1047`, `bin/tests/test_disjuntor.sh:1177`. As linhas dos anexos foram contadas a partir do primeiro conteúdo de cada anexo.

**2. Escritores e leitores.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `est['b3'][obra]['auto']` | `decide_b3` acrescenta SHA na liberação automática; `_com_estado` persiste o JSON. `bin/disjuntor.py:1051`, `bin/disjuntor.py:379`. | Guarda da cota; testes da primeira liberação e sobrevivência ao rearme. `bin/disjuntor.py:1044`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1180`. |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado. `bin/disjuntor.py:1046`. | Deduplicação do aviso. `bin/disjuntor.py:1045`. |
| `lib['auto']` / `b3_libera['auto']` | `ler()` inicializa e conta eventos `LIBERA (auto)`, na janela de 24 horas. `bin/disjuntor.py:1996`, `bin/disjuntor.py:2000`, `bin/disjuntor.py:2025`. | `linha()` formata; `gerar_estado.py` usa `ler()/linha()`; teste antigo consulta o resultado. `bin/disjuntor.py:2285`, `bin/disjuntor.py:2289`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:430`. |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` publica após persistência, através de `trilha()`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:1064`, `bin/disjuntor.py:1067`, `bin/disjuntor.py:205`. | Leitor específico encontrado: teste `conta()`. O ESTADO não enumera `SOBE`; o leitor de censo também o descarta. `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1177`, `bin/disjuntor.py:2002`, `bin/disjuntor.py:2115`. |
| SHA consumido | `_sha_b3()` normaliza/calcula; `decide_b3` grava em `consumidas`. `bin/disjuntor.py:677`, `bin/disjuntor.py:1039`, `bin/disjuntor.py:1055`. | Guarda de reutilização lê `consumidas` e `topo_no_disparo`. A fotografia produz os SHA anteriores ao disparo. `bin/disjuntor.py:1040`, `bin/disjuntor.py:832`. |
| `B3_AUTO_COTA` | Constante `1`. `bin/disjuntor.py:147`. | Guarda, texto de `SOBE` e mensagem do B3. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1048`, `bin/disjuntor.py:1176`. |

Os consumidores decisórios fora do diff convergem na mesma decisão, podendo provocar a gravação da liberação:

- **Stop:** `hook_stop_fila1.py` chama `julga_b3`, que chama `decide_b3`; recebe paradas e mensagens. `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1205`, `bin/disjuntor.py:1096`.
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`. `bin/disjuntor.py:1801`, `bin/disjuntor.py:1105`, `bin/disjuntor.py:1096`.
- **Wrapper Codex:** `--b3-parada → b3_parada → estado_b3`; recusa retorno diferente de sucesso. `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:2383`, `bin/disjuntor.py:1158`.
- **ESTADO:** recebe a contagem de eventos, incorpora `dj_txt` e grava a página; não lê a cota persistida como medida. `bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`, `bin/disjuntor.py:2025`.

Há ainda leitores genéricos do JSON: `_com_estado` e `ler_estado`; os consumidores deste último selecionam B1, B4 e históricos, sem interpretar `auto`. A referência do hook ao arquivo serve para rejeitá-lo como prova de espera. Referências: `bin/disjuntor.py:367`, `bin/disjuntor.py:401`, `bin/disjuntor.py:596`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:2213`, `bin/hook_stop_fila1.py:664`.

**3. Caminhos e contraexemplos.**

- **Segunda AUTOLIBERA, sozinha:** permanece PARADA, registra `auto_sobe` e não consome seu SHA. A recusa dá `continue` antes da liberação e do consumo. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1055`.
- **Segunda ocorrência liberada:** após gastar a primeira AUTO, disparar novamente e escrever no topo:
  ```
  AUTOLIBERA OBRA-A: classe=ERRO-BOBO segundo erro
  ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
  ```
  A primeira linha gera `SOBE`; a segunda libera na mesma decisão, sem autenticar Ronald. Esse caminho está expressamente exercitado pelo teste. `bin/disjuntor.py:757`, `bin/disjuntor.py:1047`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1056`, `bin/tests/test_disjuntor.sh:1214`.
- **`classe=DESENHO`:** não entra pela porta nova. Porém `classe=FORMA erro de desenho` satisfaz o reconhecedor: há validação do rótulo, não da natureza real do defeito. `bin/disjuntor.py:146`, `bin/disjuntor.py:763`. Classificação correta dos defeitos em produção: `[nao verificado]`.
- **Rearme/reinício normal:** não zeram a cota; rearme preserva `auto`, e cada transição relê o estado persistido. Se o estado estiver ausente, a inicialização usa `{}` e a ausência de `auto` vale zero; isso é perda/troca de estado, não efeito do reinício normal. `bin/disjuntor.py:1134`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:367`, `bin/disjuntor.py:368`, `bin/disjuntor.py:1044`. Ocorrência dessa perda em produção: `[nao verificado]`.

**4. As linhas antigas mudam em alguma composição.** Seus reconhecedores individuais continuam iguais, mas uma AUTO válida anterior ganha a seleção e encerra o loop. Com AUTO seguida de `ERRO-BOBO`, passa a consumir o SHA da AUTO, gastar cota e registrar `LIBERA (auto)`; antes, a candidata seria `ERRO-BOBO`, com outro SHA e outra contagem. Referências: `bin/disjuntor.py:746`, `bin/disjuntor.py:757`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1051`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1059`.

Com cota esgotada, a antiga abaixo continua liberando, conforme o controle `c5`. `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`.

**5. Os três mutantes declarados têm mordida identificável.**

| Mutante | Verificação que o rejeita |
|---|---|
| Libera a segunda | `c2` exige duas recusas, um SOBE e recusa após rearme. `bin/tests/test_disjuntor.sh:1942`, `bin/tests/test_disjuntor.sh:1186`. |
| Aceita DESENHO | `c3` exige PARADA e reconhecimento vazio. `bin/tests/test_disjuntor.sh:1943`, `bin/tests/test_disjuntor.sh:1195`. |
| Aceita sem classe | A alteração também aceita DESENHO, atingindo `c3`; os inválidos atingem `c4`. `bin/tests/test_disjuntor.sh:1944`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`. |

O harness confere âncora única e inclui controle da cópia sem mutação. `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1630`, `bin/tests/test_disjuntor.sh:1652`.

Não há `all([])` nesses controles: `rs` recebe quatro resultados e `ctrl`, três. Entretanto, **a medição positiva fica sem prova**: o teste de `ler()` exige apenas `auto=0`, enquanto `b3_auto` conta a trilha diretamente. Retirar `auto` da regex do medidor mantém esse assert satisfeito. Referências: `bin/tests/test_disjuntor.sh:1201`, `bin/tests/test_disjuntor.sh:1204`, `bin/tests/test_disjuntor.sh:1210`, `bin/tests/test_disjuntor.sh:1215`, `bin/tests/test_disjuntor.sh:431`, `bin/tests/test_disjuntor.sh:1156`, `bin/disjuntor.py:2023`.

O controle da porta antiga também ignora o retorno de `dispara()` e verifica apenas ausência no mapa, sem exigir `LIBERA (erro-bobo)`. É uma fragilidade; A já deveria estar PARADA pelo caso anterior. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`. Execução integral do selo com arquivos e travas reais: `[nao verificado]`.

**6. Classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para `auto`: a atribuição específica está em `decide_b3`, persistida pela porta comum. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1075`. |
| Relógio fora da borda | Há leitura interna herdada: `decide_b3` usa `time.time()` quando falta `agora`, como ocorre na via `b3_parada`; `trilha()` usa `datetime.now()`. A cota nova não depende de relógio. `bin/disjuntor.py:988`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:206`, `bin/disjuntor.py:1044`. |
| Selo sem mordida | Os três declarados mordem; falta testemunha positiva do contador `auto` no ESTADO. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`, `bin/tests/test_disjuntor.sh:431`. |
| Campo com dois significados | Existem dois `auto` distintos: lista cumulativa por obra e contador de eventos em 24 horas. Não são a mesma célula; zero no painel não significa cota disponível. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1996`, `bin/disjuntor.py:2025`. |
| Ordem de trava | Sem inversão acrescentada: decisão e persistência ficam sob `LOCK_EX`, com anúncio ainda sob a trava. Wrapper e triagem adquirem resposta antes de estado. `bin/disjuntor.py:359`, `bin/disjuntor.py:374`, `bin/disjuntor.py:390`, `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:897`, `bin/disjuntor.py:929`. |

“Sobe ao Ronald” produz uma linha de trilha que o ESTADO não trata como evento próprio; entrega ou notificação efetiva ao Ronald permanece `[nao verificado]`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:2002`.

_4219 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
