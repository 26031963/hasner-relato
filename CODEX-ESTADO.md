# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 23:32** · publicado em **06/11/2026 23:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 23:32 · sessao b9ba20b70125 · - · - · origem=Ronald (tmux)

Achados e mapa de leitores, somente pela cópia:

1. **P3 está curado no sítio:** filtro de escala certa e promoção usam `_dia_de(p, resultado.dia_da_jornada)`, antes de separar FT certa/feriado/sem escala. A mesma partição segue para `lavrar`. (`app/ponto/services/fechamento.py:941`, `app/ponto/services/fechamento.py:954`, `app/ponto/services/fechamento.py:988`, `app/ponto/services/fechamento.py:1211`)

2. **A testemunha da exportação continua comparando dias diferentes.** `dias_fora_do_dia_pago` soma períodos pela data civil local da entrada; DiaPago foi agrupado por jornada. No F11h, períodos de 3h em d e 4h em d+1 produzem espelho civil 3h/4h versus DiaPago 7h/0h, acusando divergência apesar da atribuição correta ao turno. Esse contador continua fora de `falhas`. (`app/folha/porta_export.py:104`, `app/ponto/services/dia_pago.py:245`, `app/ponto/tests/test_o280_bateria_da_lei.py:1889`, `app/folha/porta_export.py:696`)

3. **Outro invariante do export usa data civil crua:** `_p.entrada.date()` agrupa HE/intra/trabalhado, podendo inclusive usar dia UTC; previsto e intervalo declarado são consultados nessa data. A redistribuição da dobra de um portador para vários períodos pode mudar essa acusação. Ele integra `falhas`, portanto pode afetar a liberação do TXT; ocorrência na frota é [não verificado]. (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:548`, `app/folha/porta_export.py:560`, `app/folha/porta_export.py:689`)

4. **Alerta ainda mistura dia julgado e dia civil:** a escolha “folga semanal” consulta `data_jornada`, mas imprime o nome do dia pelo `weekday()` da entrada do par. Cauda de segunda pertencente à jornada de domingo pode dizer “folga semanal (Segunda)”. Isso muda texto, sem escrita direta de rubrica nesse ramo. (`app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`)

5. **Janela continua civil antes da dobra:** `_janela_do_dia` consulta empresa e marcos por `entrada.date()`. A dobra agora usar o mapa não corrige esse recorte anterior do par efetivo. Trata-se da mesma inconsistência temporal, embora a pergunta ali seja janela/marcos, não identificação de feriado; pode alterar duração e HE. O próprio calculador documenta pares invertidos por essa ancoragem, mas os números comentados não foram reproduzidos nesta análise: [não verificado]. (`app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1570`, `app/ponto/calculador/periodo.py:660`)

**Escritores e leitores dos campos**

| Campo | Escrita e leitura |
|---|---|
| `minutos_extra_100_feriado` | Dobras por período 12x36/comercial; FT partido; dobras diárias repartidas. ResultadoMes soma regulares; fechamento acrescenta FT promovida. DiaPago lê regulares e, para FT feriado, promove trabalhado. (`app/ponto/motor_calculo_v2.py:2852`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3155`, `app/ponto/motor_calculo_v2.py:3327`, `app/ponto/motor_calculo_v2.py:3414`, `app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:250`) |
| `minutos_extra_100_noturna` | Casca `_aplicar_he_noturna` escreve retorno positivo de `he_noturna`; resets e bloqueio zeram. ResultadoMes soma regulares; fechamento e DiaPago acrescentam campo dos FT promovidos. (`app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:1053`, `app/ponto/motor_calculo_v2.py:1509`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/fechamento.py:970`, `app/ponto/services/dia_pago.py:252`) |
| Noturno relógio/legal | `_escrever_noturnas` lê `calcular_noturnas`; legais = relógio × fator. ResultadoMes soma regulares e FT; FM recebe o total; DiaPago soma os campos nos respectivos dias. Não há um campo genérico `minutos_noturnos` no dataclass. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:1016`, `app/ponto/motor_calculo_v2.py:1033`, `app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`) |
| FM / DiaPago persistidos | FM recebe totais e persiste por `save`/`update`; `lavrar` apaga/recria a versão do DiaPago. Falha posterior da lavratura é capturada após persistência do FM. Restauração atualiza somente FM. (`app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1246`, `app/ponto/services/fechamento.py:1400`) |

**Os leitores não compartilham integralmente o mesmo contrato de dia.**

- Calendário lê dinheiro do DiaPago por data; suas batidas/cards vêm de `turnos_do_colab` pela `CargaDaJanela`, agrupados por `t.data_turno`, não por data civil. Assim, quando a base sobrescreve o dia do juiz pela herança de seis horas, dinheiro e geometria podem ter origens diferentes. (`app/colaboradores/services/calendario.py:221`, `app/colaboradores/services/calendario.py:224`, `app/colaboradores/services/calendario.py:610`, `app/colaboradores/services/calendario.py:655`, `app/ponto/motor_calculo_v2.py:2443`)
- Espelho também agrupa cards pelo juiz `dia_das_batidas`, mas lê rubricas do DiaPago. Seu resumo provisório ainda recorta períodos pela data civil local; o PDF chama a mesma soma sem esse recorte. (`app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:813`, `app/ponto/services/espelho.py:84`, `app/ponto/services/espelho.py:439`, `app/relatorios/pdf_espelho.py:502`)
- Tela diária e PDF imprimem rubricas lavradas; não remontam o split. O PDF ainda consulta os períodos para decidir presença de coluna. (`app/templates/ponto/espelho.html:268`, `app/templates/ponto/espelho.html:283`, `app/relatorios/pdf_espelho.py:837`, `app/relatorios/pdf_espelho.py:854`)
- TXT lê FM: HE100 normal/feriado e adicional noturno total. Não emite evento separado para HE100 noturna; portanto mudanças apenas no subconjunto não alteram essas linhas do TXT. Mudança de `horas_noturnas` altera a entrada do adicional. (`app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:443`, `app/folha/export.py:447`)

**V3 conserva campo com dois significados, já F1 da fila.** A alimentação lê dia da jornada e pares portadores de HE; sem mapa declara contador e ignora o período. `reler`, porém, zera pais HE em todos os pares, preserva splits e recoloca a dobra inteira no último. No NPART corrigido, o primeiro pode terminar HE100=0 e HE100 noturna=120. É subconjunto maior que pai por período. Chamadas encontradas de `reler` na cópia são testes; efeito monetário produtivo decorrente é [não verificado]. (`app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/periodo.py:611`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`, `app/ponto/tests/test_o280_bateria_da_lei.py:4678`)

Banco e DSR ainda agrupam extras por data civil; DSR subtrai HE100 feriado, conservando zero para a dobra repartida. A6 ainda conta ocorrências sem deduplicar dia. Esses restos já constam da fila fornecida; impacto concreto na frota é [não verificado]. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3543`, `app/ponto/motor_calculo_v2.py:3545`, `app/ponto/services/esmeril_espelho.py:177`, `app/ponto/services/esmeril_espelho.py:179`)

## 06/11 23:31 · sessao 9f3eff5d28e6 · - · - · origem=Ronald (tmux)

Os seis REDs cobram valores explícitos; nenhum aceita qualquer resultado ou exceção: `confere` compara o subconjunto esperado por igualdade, e `cobra_a_lei` faz a mesma comparação diretamente. (`app/ponto/tests/test_o280_bateria_da_lei.py:1306`, `app/ponto/tests/test_o280_bateria_da_lei.py:1321`)

| RED | Valor exigido e mordida |
|---|---|
| F10d | Dois pares, 660 trabalhados, HE100/feriado zero; troca a caracterização indevida de 90 pela regra. (`app/ponto/tests/test_o280_bateria_da_lei.py:1593`) |
| F10e | HE100 feriado zero, sem `expectedFailure`; é praticamente a mesma fronteira de F10d, com uma única rubrica. (`app/ponto/tests/test_o280_bateria_da_lei.py:1596`, `app/ponto/tests/test_o280_bateria_da_lei.py:1608`) |
| F10f | Dois pares, 660 trabalhados e 660 de HE100/feriado. **Não cobra noturno da cauda**; portanto pode passar mesmo que a prorrogação herdada continue perdida. (`app/ponto/tests/test_o280_bateria_da_lei.py:1624`) |
| F10g | Exige exatamente uma cauda em FT e cobra 120 trabalhados/feriado/HE100 noturna nela. A fixture exercita efetivamente um par distinto após meia-noite. (`app/ponto/tests/test_o280_bateria_da_lei.py:1654`, `app/ponto/tests/test_o280_bateria_da_lei.py:1657`) |
| F11i | A autoridade simulada responde `dia == fer`; exige FM feriado 7h, folga zero e DiaPago feriado 7h. Exercita a data passada ao filtro, mas **não cobra noturno**, nem valida os critérios reais internos de `escala_certa_no_dia`. (`app/ponto/tests/test_o280_bateria_da_lei.py:1819`, `app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |
| N-PRORROG | Cobra `(60,60,0)` para prorrogação ligada sem pausa, ligada com pausa e desligada. Cobre contexto interno do mesmo par 22–07; não cobre prorrogação herdada de outro par. (`app/ponto/tests/test_o280_bateria_da_lei.py:2001`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`) |

Reproduzi em memória as funções puras, sem importar Django nem gravar arquivo. O corpo atual deu **`(60,60,0)`**; recolocando somente o trecho anterior de `he_noturna`, reconstruído pelo diff fornecido, deu **`(0,0,0)`**. Isso demonstra mordida numérica de N-PRORROG no corpo anterior, **não execução do pai inteiro**. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`; execução do pai inteiro: **[não verificado]**)

Para F10d/e/f/g e F11i, os pontos alterados correspondem à condição exercitada pelos REDs; execução integrada no pai e na cópia atual: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`)

**Censo 10 → 11:** reproduzi por AST **11 privados, 35 públicos e 19 portas**. O incremento é aritmeticamente legítimo para o contrato efetivo de ausência de nova **porta pública do DIA**; `_dia_da_jornada_do_par` é privado e recebe o par. O teste conta nomes que contêm `dia`/`jornada`, não examina a semântica de suas decisões. (`app/ponto/tests/test_contract_juiz_batida.py:132`, `app/ponto/tests/test_contract_juiz_batida.py:313`, `app/ponto/tests/test_contract_juiz_batida.py:330`)

**A justificativa do incremento é falsa em seu absoluto:** “não decide dia nenhum” só vale quando a chave existe. A expressão `.get(_e, _e.date())` deriva o dia civil quando falta a chave, enquanto a pertinência da batida já tem autoridade registrada em `turnos_do_colab`. O censo não distingue leitor de mapa de derivador paralelo; passa com ambos. (`app/ponto/tests/test_contract_juiz_batida.py:329`, `app/ponto/motor_calculo_v2.py:964`, `app/core/juizes.py:196`)

O laço mensal grava a chave antes da chamada normal, mas existe outra chamada produtiva no crédito de aberto sem gravação correspondente nesse ponto. O novo helper pode decidir por fallback nesse caminho; o período auxiliar depois empresta somente trabalhado/fora da janela, portanto isso isoladamente não prova dinheiro novo incorreto. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2590`, `app/ponto/motor_calculo_v2.py:2596`)

A base também conserva o segundo julgamento preexistente: recebe o dia do juiz e pode substituí-lo pela herança de intervalo até seis horas. O mapa novo passa adiante essa resposta composta, não necessariamente apenas o veredito do juiz registrado. (`app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2494`)

As adições do diff usam instantes recebidos e datas derivadas deles; não introduzem leitura de relógio nas funções alteradas. Isso não certifica concorrência nem ordem de travas da execução completa. (`app/ponto/motor_calculo_v2.py:452`, `app/ponto/motor_calculo_v2.py:963`, `app/ponto/services/fechamento.py:942`; execução concorrente: **[não verificado]**)

## 06/11 22:48 · sessao 7c8459a8b853 · FIO-E0-V4-E1 · censo · origem=Code

O `remover_motivo` citado é **`DisputaSupervisao.remover_motivo`, em `app/chamados/models.py:1900`**. Ele apaga perguntas em `app/chamados/models.py:1976`; a exclusão também alcança `RespostaAnterior` por `CASCADE`, definido em `app/chamados/models.py:3179`.

O relatório abaixo é um censo estático da cópia. Esquema instalado, execução das portas, existência das trilhas e passivo de produção: **[nao verificado]**.

**1. Decisões sobrescritas ou apagadas**

“Há trilha” abaixo significa que o código tenta produzi-la; não garante sua presença em produção. O escritor de eventos admite falha sem derrubar o ato, em `app/core/observ.py:94` e `app/core/observ.py:105`.

| Escritor | Campos gravados na mesma pergunta | Anterior preservado? |
|---|---|---|
| `responder_pergunta_disputa_core` | Substitui `resposta_colab` e `respondida_em`; limpa `validada_em/por`; pode retirar materialização sem lastro. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1300` | **Não cria RA.** Guarda a validação desfeita em evento, mas não fotografa a resposta substituída. O log contém a resposta nova truncada; a nota contém a nova resposta/display e é best-effort. `app/chamados/services/disputa_emissao.py:1309`, `app/chamados/services/disputa_emissao.py:1318`, `app/chamados/services/disputa_emissao.py:1355` |
| Escape no mesmo core | Substitui `recusa_motivo` por “Colaborador respondeu…”. `app/chamados/services/disputa_emissao.py:1340` | Sem fotografia do motivo anterior e sem RA nesse bloco. `app/chamados/services/disputa_emissao.py:1331`, `app/chamados/services/disputa_emissao.py:1342` |
| `responder_automaticamente` | Escreve texto e instante se o texto estava vazio; pode substituir um instante existente nesse meio-estado. `app/chamados/services/disputa_emissao.py:1431` | Sem RA ou evento próprio na função. `app/chamados/services/disputa_emissao.py:1422` |
| `carimbar_resposta_pela_trilha` | Preenche `respondida_em` somente quando nulo, usando o instante fornecido. `app/chamados/services/disputa_emissao.py:1448` | Não perde carimbo preenchido; registra instante/motivo em evento. `app/chamados/services/disputa_emissao.py:1452` |
| `_carimbar_validacao` | Atribui `validada_em=now()` e `validada_por=autor`. `app/chamados/services/validacao.py:131` | O fluxo normal recusa pergunta já validada e registra log, mas não cria RA por aceite. `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:440` |
| `devolver_validacao_recusada` | Limpa `validada_em/por`, conservando a resposta. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:188` | Evento guarda antes/depois da assinatura, motivo e fechamento da disputa; sem RA. `app/chamados/services/validacao.py:205` |
| `recusar_pergunta` | Substitui `recusa_motivo`; delega a limpeza da resposta/validação à reabertura. `app/chamados/services/validacao.py:541`, `app/chamados/services/validacao.py:547` | Tem fotografia da resposta, instante e recusa anteriores; nota e evento. A reabertura cria RA, mas a chamada **não passa autor/motivo** para ela. `app/chamados/services/validacao.py:551`, `app/chamados/services/validacao.py:555`, `app/chamados/services/validacao.py:547` |
| `reabrir_para_correcao` | Limpa texto, resposta e validação; com `desfazer_absorcao=True`, limpa também materialização, autor e via. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413` | **Cria RA antes do update, no mesmo atomic**, somente se havia texto ou carimbo de resposta. A fotografia não inclui materialização, via, pendência ou recusa. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406` |
| `_materializar_resposta_hora` | Substitui `pendencia_admin` e `recusa_motivo`. `app/chamados/services/materializacao.py:820`, `app/chamados/services/materializacao.py:825` | Sem RA ou fotografia anterior nesse bloco. `app/chamados/services/materializacao.py:822` |
| `_carimbar_recusa` | Troca `recusa_motivo` quando diferente; colisão também carimba `fato_ocupado`. `app/chamados/services/materializacao.py:1064`, `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1078` | Nota contém a recusa atual traduzida, não a anterior; depende de haver chamado-destino. Sem RA. `app/chamados/services/materializacao.py:1079` |
| `carimbar` | Substitui `materializada_em/por/via_resolucao`; pode preencher resposta/validação nulas. `app/chamados/models.py:2583`, `app/chamados/models.py:2587` | A fotografia serve para rollback **em memória**, não para histórico persistente. Sem RA. `app/chamados/models.py:2595`, `app/chamados/models.py:2609`, `app/chamados/models.py:2616` |
| `carimbar_identidade` | Troca carimbo/via e, opcionalmente, recusa. `manter_carimbo=True` conserva instante/autor, mas troca a via. `app/chamados/models.py:2636`, `app/chamados/models.py:2641` | Sem histórico próprio; preservação depende do chamador. `app/chamados/models.py:2625`, `app/chamados/models.py:2644` |
| `descarimbar` / `descarimbar_em_lote` | Limpam materialização, autor e via; o lote aceita campos adicionais. `app/chamados/models.py:2646`, `app/chamados/models.py:2691` | Sem RA ou trilha própria nessas primitivas. `app/chamados/models.py:2652`, `app/chamados/models.py:2699` |
| `encerrar_por_fato_no_espelho` | Substitui apenas a via por `fato_materializado`. `app/chamados/models.py:2707` | Sem fotografia anterior ou RA na função. `app/chamados/models.py:2702` |
| `relavrar` | Substitui via derivada e cache de veredito/relógio. `app/chamados/services/relavra_via.py:121` | Evento conserva via/veredito anteriores e novos; não conserva todos os carimbos nem cria RA. `app/chamados/services/relavra_via.py:130` |
| `conferir_data_do_fato` | Preenche `data_conferida_em/por`; não altera `data_evento`. `app/chamados/services/validacao.py:615` | Guarda justificativa e os dias comparados no log; carimbo já preenchido é recusado. Sem RA. `app/chamados/services/validacao.py:620` |
| `lavrar` / `gravar_lavras_em_lote` | Substituem `veredito_lavrado`, `julgado_em`, `janela_expira_em`. `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:227` | São cache regravável, sem sequência anterior por pergunta nessas funções. `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:211` |

Os atos abaixo passam pelas primitivas anteriores; continuam relevantes porque escolhem **qual decisão sobrescrever ou retirar**:

| Ato delegado | Efeito e preservação |
|---|---|
| `fechar_por_admin` | Troca outra via por `admin_decidiu`; a guarda protege somente repetição do próprio carimbo. Sem RA. `app/chamados/models.py:2513`, `app/chamados/models.py:2522` |
| `retratar_duplicata` | Troca via/recusa para apontar a canônica; pode conservar instante/autor antigos. Não fotografa a via/recusa substituídas. `app/chamados/services/fio.py:891` |
| `ressuscitar_perguntas` | Limpa materialização, validação e resposta temporal do universo selecionado sem texto. Log conserva IDs/rótulos, não fotografia integral das assinaturas apagadas. `app/chamados/models.py:1434`, `app/chamados/models.py:1504`, `app/chamados/models.py:1518` |
| `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta`, `reabrir_por_competencia` | Carimbam ou retiram carimbos; não criam RA por esses atos. `app/chamados/models.py:2721`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/models.py:2756` |
| `desfazer_carimbo_sem_lastro` | Relê sob lock e descarimba; evento guarda instante/via anteriores, mas não o autor anterior da materialização. `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:229` |

Na **disputa**, os escritores adicionais de decisão/remoção são:

| Escritor | Perda e trilha |
|---|---|
| `remover_motivo` | Apaga perguntas de todo aquele motivo, inclusive respondidas; apaga RA por cascata. `contexto.revalidacoes` guarda evento/instante/motivo removido, não as perguntas apagadas. `app/chamados/models.py:1955`, `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |
| `aparar_motivo_respondido` | Retira motivo e sua chave do contexto, conservando perguntas; sem fotografia própria do contexto anterior. `app/chamados/models.py:1893` |
| `fechar` | Grava `fechada_em/por`, `resolucao`, `motivo_fechamento`. Em desligamento/admin_encerra também encerra perguntas vivas por `admin_decidiu`; eventos não fotografam integralmente seus carimbos anteriores. `app/chamados/models.py:1852`, `app/chamados/models.py:1856`, `app/chamados/models.py:1861` |
| `reabrir_admin` / `reabrir_sistema` | Limpam fechamento, autor, resolução e motivo; admin incrementa contador. Nota anterior de resolução depende do receiver do fechamento chegar a copiá-la. `app/chamados/models.py:2063`, `app/chamados/models.py:2087`, `app/chamados/signals.py:42` |
| `limpar_resolucao_residual` | Apaga resolução de disputa aberta; tenta copiar o texto anterior para nota quando há chamado. `app/chamados/models.py:2170` |

O rol `motivos/contexto` também é regravado na emissão/agregação, sem versionamento integral nesses blocos: helper comum, agrupador de órfãos e âncora. Isso é distinto de registrar uma decisão sobre a resposta. `app/chamados/services/disputa_emissao.py:477`, `app/chamados/services/disputa_emissao.py:492`, `app/chamados/services/disputa_emissao.py:922`, `app/chamados/services/disputa_emissao.py:970`, `app/chamados/services/disputa_emissao.py:998`, `app/chamados/services/disputa_emissao.py:1089`, `app/chamados/services/disputa_emissao.py:1190`.

**Silêncio:** existe encerramento humano com motivo; ele é tratado como decisão sobre perguntas mudas e passa por `fechar`. A antiga expiração automática por conversa parada foi removida, segundo a lápide do comando. `app/chamados/models.py:1872`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`. Expiração da janela do juiz continua existindo por leitura do relógio, sem nascer uma decisão imutável equivalente. `app/chamados/juizes.py:1197`, `app/chamados/veredito_lavrado.py:95`.

**2. RespostaAnterior**

O modelo guarda:

| Campo | Papel |
|---|---|
| `pergunta` | FK com `CASCADE`. `app/chamados/models.py:3179` |
| `resposta_anterior` | Texto retirado da pergunta. `app/chamados/models.py:3180` |
| `respondida_original_em` | Instante da resposta retirada. `app/chamados/models.py:3181` |
| `validada_original_em/por` | Assinatura copiada da pergunta. `app/chamados/models.py:3182` |
| `versao_reaberta_em/por` | Instante e autor da reabertura. `app/chamados/models.py:3185` |
| `motivo_reabertura` | Texto até 200 caracteres. `app/chamados/models.py:3188` |

O único escritor encontrado é `reabrir_para_correcao`: `bulk_create` antes da limpeza, no mesmo `atomic`; só cria versão quando havia texto ou carimbo de resposta. Ordenação é por reabertura e PK descendentes. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/models.py:3190`.

Os seis chamadores encontrados **omitem `por` e `motivo`**, deixando os defaults vazios: colaborador, view de reabertura, ação do fio, recusa, devolução da absorção e command de limbo. `app/chamados/models.py:2146`, `app/chamados/views.py:1436`, `app/chamados/services/acoes_disputa.py:103`, `app/chamados/services/validacao.py:547`, `app/chamados/services/materializacao.py:658`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`, `app/chamados/services/disputa_emissao.py:1362`.

Leitores encontrados: helper de consulta, prefetch do fio e modal. O modal lê texto, instante original, autores, instante e motivo da reabertura; `validada_original_em` é armazenado, mas não aparece nessa apresentação. `app/chamados/services/disputa_emissao.py:1723`, `app/chamados/services/fio.py:227`, `app/templates/chamados/partials/modal_fio.html:208`.

Para virar a sequência única pedida, faltam, por comparação com os escritores:

- Registro em **cada** resposta, aceite, recusa, reabertura, retratação e decisão de silêncio; hoje RA nasce somente na reabertura. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:545`, `app/chamados/models.py:1852`.
- Tipo do fato, decisão/resposta à qual ele se aplica, identidade da instância do marco e relógio próprio de cada decisão; esses campos não constam do esquema atual de RA. `app/chamados/models.py:3179`.
- Preservação de recusa, via e absorção; a fotografia atual copia somente texto, resposta e validação. `app/chamados/services/disputa_emissao.py:1407`.
- Proteção contra exclusão em cascata e contra alteração posterior; a classe/migration não estabelecem proteção append-only. `app/chamados/models.py:3179`, `app/chamados/models.py:3190`, `app/chamados/migrations/0050_resposta_anterior.py:16`.

O histórico atual também não resolve essa lacuna: reconstrói eventos pelos **carimbos e texto atuais** da pergunta, sem consultar RA. `app/chamados/services/historico.py:96`.

**3. Identidade e acha-ou-cria**

Há **um nascimento central efetivo**, `cls.objects.create`, precedido de `.last()` por `(colaborador via disputa, motivo, data_evento)`. Célula e destino não integram essa chave; uma pergunta reaproveitada conserva sua disputa original. `app/chamados/models.py:2392`, `app/chamados/models.py:2431`, `app/chamados/models.py:2435`, `app/chamados/models.py:2450`, `app/chamados/models.py:2476`.

| Sítio de produção | Busca/chave local antes de `abrir` |
|---|---|
| `gerar_perguntas_disputa` | Sem data: disputa+motivo; datadas: disputa+motivo+dia. Depois aplica o dedup global do modelo. `app/chamados/services/disputa_emissao.py:299`, `app/chamados/services/disputa_emissao.py:370`, `app/chamados/services/disputa_emissao.py:386`, `app/chamados/services/disputa_emissao.py:389` |
| `_criar_pergunta_se_couber` | `_ja_perguntou` usa disputa+motivo+dia e vitalidade; depois `abrir`. `app/chamados/services/disputa_emissao.py:765`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/services/disputa_emissao.py:1237` |
| Emissão de âncora | Procura destino+`ancora_ausente` não materializada; chama `abrir` sem data. `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1195` |
| Reabertura administrativa | Escolhe última disputa fechada do chamado; cria confirmação sem data, sem dedup próprio de pergunta. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |
| Declaração do colaborador | Primeira reutilizável por disputa+motivo+dia; se nenhuma, `abrir`. `app/chamados/services/declaracao_texto.py:226`, `app/chamados/services/declaracao_texto.py:313`, `app/chamados/services/declaracao_texto.py:325` |
| Aplicação de declaração pelo admin | Mesma candidata reutilizável; fallback para `abrir`. `app/chamados/services/declaracao_texto.py:611`, `app/chamados/services/declaracao_texto.py:614` |

São **seis chamadores diretos do nascimento central**, nos endereços acima. Não foi encontrada chamada concreta de `get_or_create/update_or_create` para essa entidade; completude além da busca estática: **[nao verificado]**.

A conversa é uma exceção relevante: `data_evento=None` na entrada desliga o dedup e vira `timezone.localdate()` antes de gravar. Assim, consultas posteriores não distinguem por esse campo uma conversa de uma pergunta datada do mesmo dia. `app/chamados/models.py:2424`, `app/chamados/models.py:2476`.

**Unicidade:** não há constraint composta da pergunta declarada no `Meta`; a migration criadora também não a estabelece. Há PK/FKs e índices simples do cache, que não protegem a identidade solicitada. Índices/constraints realmente instalados em produção: **[nao verificado]**. `app/chamados/models.py:2857`, `app/chamados/migrations/0014_perguntadisputa.py:19`, `app/chamados/migrations/0014_perguntadisputa.py:30`, `app/chamados/models.py:2346`.

**Locks:** não existe trava da identidade dentro de `abrir`; seu `.last()→create` permanece separado. O `atomic` de `save` protege cada gravação, não torna a busca anterior exclusiva. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`, `app/chamados/models.py:2853`.

| Caminho | Proteção encontrada |
|---|---|
| Helper de disputa | Trava disputa **existente** por colaborador+aberta; a geração normal acontece depois do retorno do helper. `app/chamados/services/disputa_emissao.py:466`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:605` |
| Declarações | Têm atomic externo e usam o helper de disputa; isso não substitui unicidade da pergunta nem cobre a ausência de uma disputa existente. `app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:467` |
| Âncora | Atomic e lock de disputa existente; teste de pergunta ocorre antes desse lock. `app/chamados/services/disputa_emissao.py:1148`, `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1156` |
| Agrupador de órfãos | Atomic, mas seleção da disputa sem `select_for_update`. `app/chamados/services/disputa_emissao.py:898`, `app/chamados/services/disputa_emissao.py:935` |
| Reabertura admin | Sem lock próprio nessa sequência de seleção/reabertura/nascimento. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |

A janela estática da corrida está comprovada; sua reprodução concorrente em banco: **[nao verificado]**. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`.

**Instância de marco candidata:** o dado já oferece as chaves `hi/hii/hfi/hii2/hfi2/hf`, qualificadas pela célula, com ordem canônica e exposição `(tipo, chave, hora)`. A pergunta ainda não persiste essa chave. Usar célula+chave é uma inferência de encaixe; estabilidade através de regeneração do DNA: **[nao verificado]**. `app/escala/models.py:1652`, `app/escala/models.py:1659`, `app/escala/models.py:1672`, `app/chamados/models.py:2209`.

Hoje há perda concreta dessa distinção: declaração reduz marcos internos a ida/volta pelo tipo, e o juiz admite ambiguidade com duas pausas. Cartório e pauta já consomem o vocabulário de marcos da célula. `app/chamados/services/declaracao_texto.py:118`, `app/chamados/services/declaracao_texto.py:300`, `app/chamados/juizes.py:1106`, `app/ponto/services/cartorio.py:586`, `app/pautas/services.py:417`.

**4. Absorção**

A ligação atual é **reconstruída**, não uma FK/fato explícito do ato: o materializador obtém a ausência, captura um `agora` e tenta carimbar as perguntas-hora da mesma disputa/dia com esse instante. `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:549`.

O leitor:

1. Restringe colaborador, dia, tipo de pergunta e vias de absorção. `app/chamados/services/materializacao.py:605`.
2. Procura uma resposta de escape compatível com o tipo da ausência e extrai `(disputa_id, materializada_em)`. `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:609`.
3. Recupera as irmãs daquele grupo e exclui perguntas com batida-origem viva. `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:616`.

| Campo | Outros escritores |
|---|---|
| `disputa` | Nascimento em `abrir`; não foi encontrada transferência posterior. Reapontamento muda destino, não disputa. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:845` |
| `data_evento` | Nascimento; reapontamento preenche somente quando estava vazia. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:847` |
| `materializada_em` | Atribuído pelo helper de carimbo; persistido por `carimbar`, `carimbar_ide

[truncado em 20000 de 36686 caracteres]

## 06/11 22:46 · sessao ee0df63a3e82 · FIO-E0-V4-E2 · censo · origem=Code

**O censo mostra cinco respostas de escopos diferentes:** existência de chamado, vitalidade de pergunta, necessidade de emitir pergunta, alcance pela fila e atribuição fora do fio. Nenhuma delas, isoladamente, responde por todas as ações exigíveis de cada instância de marco. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1135`, `app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/juizes.py:1341`, `app/chamados/services/vigia_fio.py:274`)

**A bateria C-E não fecha nesta cópia.** As referências do aval a `BACKLOG:1797/1813/1821` apontam aqui para outras obras; localização e descrição individual dos dez casos FIO-E0: **[nao verificado]**. (`app/docs/BACKLOG.md:1797`, `app/docs/BACKLOG.md:1813`, `app/docs/BACKLOG.md:1821`)

**1. Os cinco respondedores**

**`cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`**

- **Responde:** `ChamadoColaborador | None`; escolhe o chamado mais novo dos módulos pedidos, em `VIVOS`, cujo dia forte coincide com `dia`. Não confere marco faltante, efeitos, ausência ou silêncio. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1894`)
- **Lê:** chamado `colaborador`, `modulo_origem`, `status_local`, `contexto_json.data_turno`, `celula.data`, `criado_em`, `pk`; o desempate é `-criado_em,-pk`. O dia forte prioriza a célula e depois as informações declaradas do chamado. (`app/chamados/juizes.py:1897`, `app/chamados/juizes.py:80`)
- **Relógio:** recebe o dia; esse percurso não consulta o instante atual. `criado_em` ordena candidatos. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1899`)
- **Todos os chamadores diretos de produção encontrados**, incluindo alias:

| Chamador | Local da chamada |
|---|---|
| `ChamadoColaborador._plano_pergunta_viva` | `app/chamados/models.py:373` |
| `ChamadoColaborador._gate_premissa_do_fechamento` | `app/chamados/models.py:711` |
| `_julgar_colab_corpo`, via `_cobr_c8` | `app/ponto/services/cartorio.py:1604` |
| `resolver_cobranca_da_entrada` | `app/ponto/services/entrada_sincronizada.py:28` |
| `_autoresolve_f2_f4` | `app/ponto/services/triagem_batida.py:589` |

**`pergunta_viva(p)`**

- **Responde:** booleano, delegando a `_pergunta_viva_calc`, com memo `_pv_memo` escrito na instância. Portanto, não é pura pelos argumentos explícitos: consulta outras fontes e relógios internos. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:835`, `app/chamados/juizes.py:1197`)
- **Ordem decisiva:** validação mata; tranca aplicável mata; resposta com texto permanece viva salvo decisão administrativa/concordância com a luz; identidade materializada encerra; materialização com marco aceso encerra; célula acusadora mantém viva; depois entram teto, classificação da falta e conferência de materialização de fato. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1194`)
- **Lê diretamente:** pergunta `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `motivo`, `celula`, `disputa.colaborador`, `chamado_destino`; célula `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas` — `hora`, `tipo`, `acesa`, `luz`, pendência temporal. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:1099`)
- **Lê transitivamente:** empresa/corte da competência, tranca e toque humano posterior; batidas ligadas à pergunta, retratação, slot esperado, batida no slot, turnos, ausências, vínculo do dia e estado do destino em um ramo legado. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:1065`, `app/chamados/juizes.py:718`, `app/chamados/juizes.py:743`, `app/escala/servico_jornada.py:340`)
- **Relógios ocultos:** `_dentro_da_janela_viva` usa `date.today()`; `classificar_falta` usa `_tz.now()` contra o fim do turno. A assinatura não recebe `agora`. (`app/chamados/juizes.py:556`, `app/chamados/juizes.py:562`, `app/escala/servico_jornada.py:375`)

**Todos os chamadores diretos de produção encontrados**, com aliases resolvidos:

| Arquivo | Chamadores e linhas das chamadas |
|---|---|
| `app/chamados/juizes.py` | `pergunta_sem_lastro_ou_viva:1225`; `perguntas_vivas_de:1230`; `fio_vivo:1242`; `alimentar_portas:1302`; `perguntas_respondidas_a_validar:1403`; `_dias_das_perguntas_vivas:2031` |
| `app/chamados/models.py` | `_vivas_entre:351`; `_universo_da_ressuscitacao:1458`; `pendentes_do_colaborador:1739`; `DisputaSupervisao.fechar:1837`; `PerguntaDisputa.estado:2502`; `carimbar:2596`; `viva:2809` |
| `app/chamados/views.py` | `responder_pergunta_disputa:1242,1257`; `reabrir_disputa_supervisao:1417` |
| `app/api/views.py` | `api_disputa_responder:2631` |
| `app/chamados/relatorios_disputa.py` | `classificar:18` |
| `app/chamados/reconciliador.py` | `lavrar_sem_turno_aberto:398`; `perguntas_absolvidas_pela_celula:651`; `_viva_em_competencia_aberta:748`; `veredito_orfa:1075`; `reatar_pergunta_orfa:1579` |
| `app/colaboradores/services/calendario.py` | `contexto_calendario:476` |
| `app/ponto/services/dia_do_colab.py` | `dia_do_colab:71` |
| `app/chamados/services/fio.py` | `montar_chamados_fio:234`; `processar_post_fio:602`; `_costura_nao_mata_o_fio:775`; `migrar_resolvedoras_vivas:932,935` |
| `app/chamados/services/pergunta_no_app.py` | `perguntas_do_app:53`; `_marco_ja_perguntado:111` |
| `app/chamados/services/acoes_chamado.py` | `montar_contexto_detalhe:348` |
| `app/chamados/services/disputa_emissao.py` | `resolver_perguntas_por_chamado_resolvido:1597,1679` |
| `app/chamados/services/competencia_trancada.py` | `medir:122,132,142` |
| `app/chamados/services/relavra_via.py` | `desfazer_carimbo_sem_lastro:227` |
| `app/chamados/services/aviso_pendencia.py` | `perguntas_pendentes_do_colab:13` |
| `app/chamados/services/vigia_fio.py` | `viva_em_disputa_fechada:755`; `carimbos_sem_lastro_por_dono:813` |
| `app/chamados/management/commands/disparar_perguntas_competencia.py` | `handle:148` |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `handle:83` |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | `_contraprova_f1t:202`; `_i6_pares:540`; `handle:858,894,1650,1707,1766,1779` |

A tabela identifica chamadas da função. Há também entrada indireta por `.viva`, que delega integralmente ao juiz, e por `.estado`; eliminar apenas os imports explícitos deixaria essas entradas ativas. (`app/chamados/models.py:2492`, `app/chamados/models.py:2798`)

**`motivos_a_perguntar(cel, hoje, agora=None, *, carimbadas=None, julgada_antes=_JULGADA_DA_CELULA)`**

- **Responde:** lista de motivos que ainda precisam ser emitidos; opcionalmente acrescenta IDs à lista `carimbadas`. Não devolve todas as ações pendentes de perguntas existentes. (`app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/services/fabrica_da_celula.py:123`)
- **Lê:** célula/colaborador — `data`, `trabalha`, `ata`, `veredito`, `julgada_em`, `situacao`; dia sem trabalho; marcos vencidos; parcial destinada à lista administrativa; perguntas anteriores do colab-dia. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:118`)
- **Deduplicação atual:** qualquer pergunta anterior do mesmo motivo bloqueia emissão, exceto `VIA_DUPLICATA`. Não consulta vitalidade nem discrimina a instância do marco. Duas pausas podem ser comprimidas no mesmo motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:59`)
- **Entradas transitivas:** `_ata_do_dia`/`_marcos_faltantes`; vínculo e marcos encadeados; `parcial_so_lista`; julgamento anterior e fatos novos de batida, ausência, justificativa, edição/regeneração da célula ou vencimento de marco. (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/declaracao_texto.py:88`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:370`)
- **Relógio:** `hoje` e `agora` por parâmetro; sem `agora`, usa `timezone.localtime()`. O ramo sem ata/marcos pode chamar `classificar_falta`, que ainda lê seu próprio `now()`. (`app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:55`, `app/escala/servico_jornada.py:377`)
- **Únicos chamadores diretos encontrados:** `perguntar_pela_celula` e `motivos_que_o_evento_deixou`. (`app/chamados/services/fabrica_da_celula.py:164`, `app/chamados/services/fabrica_da_celula.py:199`)

**`porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`**

- **Responde:** `None` ou `{rota, fila, verbos, fio}`. Soma chamado da célula na fila com pergunta viva no app, sob âncora na fila e sem resposta carimbada vazia. O fio prioriza `cobrar→colab`, depois `validar→admin`, senão `chamado`. (`app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1375`)
- **Lê:** partição global dos chamados vivos, pertença à fila global, perguntas da célula julgadas pelo C6, célula→colaborador e perguntas do app por colaborador. Chamados fornecem `id`, `modulo_origem`, `silenciado_ate`, `criado_em`, `urgencia`; perguntas fornecem resposta, destino e chamado da disputa, além das entradas transitivas do C6. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1309`, `app/chamados/services/verbos.py:44`, `app/chamados/services/fio.py:116`)
- **Relógio:** não recebe instante; `particionar_verbos` e `qs_na_fila` fazem leituras distintas de `timezone.now()`. (`app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:188`)
- **Todos os chamadores diretos encontrados:** `censo_alcance`, `cadastro_realidade.beco_em_lote` e `fora_da_folha._porta`. Esta última atende `_fio_do_dia` e a lista de chamados de `o_que_segura`. (`app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:460`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:225`)

**`dono_fora_do_fio(colab, data, perguntas, lista, com_papel=False, *, oferecido)`**

- **Responde, nesta ordem:** `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `fio_do_admin` ou `None`; com `com_papel=True`, devolve também o papel. (`app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:304`)
- **Lê:** `oferecido`; ausência cobrindo a data para cobrança e status em `ABERTAS`; perguntas com `estado==VALIDADA` e resposta `FOLGA_CONTESTA`; presença e dono do dia na lista administrativa. (`app/chamados/services/vigia_fio.py:306`, `app/chamados/services/validacao.py:216`)
- **Relógio:** não lê diretamente. O calendário que produz `oferecido` lê seu instante; a lista recebe `hoje/agora` e os repassa ao carregamento. (`app/colaboradores/services/calendario.py:159`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`)
- **Todos os chamadores diretos encontrados:** `censo` e `censo_alcance`. (`app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:654`)

**2. União das entradas**

| Família | Fatos e insumos necessários evidenciados pelo código |
|---|---|
| **Pessoa e contexto** | Identidade, situação, empresa e corte da competência; vínculo do dia, inclusive geradora da célula e tipo da escala. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/juizes.py:931`, `app/ponto/services/cartorio.py:840`) |
| **Célula e instâncias de marco** | Data do turno, `trabalha`, DNA e geradora, sequência dos marcos com tipo/hora e encadeamento noturno, edição/regeneração. Hoje a pergunta mapeia motivo→chave e, nas pausas múltiplas, agrupa várias luzes; isso não fornece identidade individual suficiente. (`app/chamados/juizes.py:867`, `app/chamados/juizes.py:893`, `app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:845`) |
| **Batidas e efeitos** | Universo apurável, timestamp, tipo, associação ao turno/marco, pergunta de origem, criação e retratação; conferência do slot e do efeito real. A porta apurável exclui `retratada_em` preenchida. (`app/ponto/turnos.py:2844`, `app/chamados/juizes.py:718`, `app/ponto/services/cartorio.py:352`) |
| **Ausências e justificativas** | Tipo, status, faixa de datas, período/minutos da parcial e decisões/carimbos; distinguir cobertura do dia inteiro e do resto exigível. (`app/ponto/turnos.py:2536`, `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:370`) |
| **Decisões da pergunta/dia** | Resposta, validação, recusa/reabertura/retração, via, instante, alvo e decisão administrativa; conferir os efeitos ligados à decisão. Hoje são lidos campos correntes e `RespostaAnterior`, não está demonstrada uma sequência única imutável de decisões E1. (`app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:216`, `app/chamados/models.py:2513`; sequência E1 completa: **[nao verificado]**) |
| **Chamados e portas** | Módulo, célula/dia, estado, silêncio, âncora/destino e chamado da disputa; universo global relevante ao crédito; verbos, presença no app, “Resolver dia” e lista administrativa por dia/dono. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1378`, `app/chamados/services/vigia_fio.py:258`, `app/chamados/services/vigia_fio.py:313`) |
| **Competência e atos aplicáveis** | Tranca, instante da tranca e toque humano posterior sobre o colab-dia. Hoje esse toque depende de `LogAuditoria`; não foi consultado dado de log nesta análise. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1065`) |
| **Tempo** | Um `agora` para vencimento dos marcos, silêncio e janela; `dia_do_turno` deve conservar o eixo da jornada, inclusive meia-noite. (`app/ponto/services/cartorio.py:831`, `app/chamados/services/verbos.py:48`, `app/ponto/turnos.py:3013`) |

**Dependências atuais de estado/projeção, distintas dos fatos de origem:**

- **`veredito_lavrado`: dependência direta da porta, pelo crédito dos verbos.** O crédito exclui pergunta materializada cujo veredito gravado é `VL_SIM`; o código admite leitura desse campo desatualizado até o cron. (`app/chamados/services/raiox.py:110`, `app/chamados/services/raiox.py:133`)
- **`CelulaDia.veredito`, `veredito_via`, ata e julgamento anterior:** influenciam vitalidade, emissão e encaminhamento da parcial. São resultados/carimbos já gravados usados na decisão atual. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`)
- **`materializada_em` e `via_resolucao`:** têm função de encerramento, inclusive por identidade; validação encerra o C6 antes da conferência do efeito. Não se pode tratar todo `materializada_em` como prova de batida. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1207`)
- **`.estado` não é coluna persistida:** é propriedade derivada de validação, materialização, resposta e C6. Mesmo assim, `dono_fora_do_fio` depende do rótulo exato `VALIDADA`. (`app/chamados/models.py:2492`, `app/chamados/services/vigia_fio.py:311`)
- **Estado dos chamados:** o C8 usa `status_local`; a porta usa esse estado mais silêncio e crédito. Um ramo da prova de materialização ainda consulta destino encerrado. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:743`)

**3. `CargaDaJanela` e carregamento por janela**

**Existe:** `CargaDaJanela(colaboradores, ini, fim)`, em `app/ponto/turnos.py:2922`. Carrega células com pad, geradoras, candidatos/vínculos, tipos e atas; carrega batidas apuráveis e última batida sob demanda; memoriza turnos por colaborador. Oferece `turnos`, `dias_encerrados` e `dia_das_batidas`. (`app/ponto/turnos.py:2936`, `app/ponto/turnos.py:2959`, `app/ponto/turnos.py:2974`, `app/ponto/turnos.py:3001`)

**Os nomes literais `precedencia_cobertura_teto` e `portas_com_universo_global` não foram localizados como funções nesta cópia: [nao verificado].** As composições existentes são:

| Composição atual | Granularidade e consultas |
|---|---|
| Precedência | `fatos_do_dia` delega ao período `[dia,dia]`; `fatos_do_periodo` já atende uma janela por colaborador, mas consulta ausências e folgas desse colaborador. (`app/ponto/precedencia.py:242`, `app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:273`) |
| Cobertura | `ausencia_cobre` faz consulta pontual por colab-dia; `cobertura_ausencia_periodo` já carrega a janela por colaborador com o mesmo desempate. (`app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2733`) |
| Teto | `vencidos_no_instante` chama `marcos_vencidos` por célula/dia. Há tratamento próprio para passado sem marcos; não equivale a simplesmente testar “dia encerrado”. (`app/chamados/services/fabrica_da_celula.py:15`, `app/ponto/services/cartorio.py:831`) |
| Portas globais | `alimentar_portas` particiona `qs_vivos()` global e lê `qs_na_fila()` global, depois agrupa perguntas/células pedidas. Sem alimentação, a porta abre esse carregamento para uma célula. (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`, `app/chamados/juizes.py:1375`) |

**Hoje os leitores principais das portas já reutilizam lote:** vigia, Cadastro×Realidade e folha passam alimentação; não há justificativa estática para chamar novamente a partição global a cada colab-dia. Permanecem consultas internas do C6 e carregamento do app por colaborador. (`app/chamados/services/vigia_fio.py:604`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:456`, `app/folha/fora_da_folha.py:218`, `app/chamados/juizes.py:1260`, `app/chamados/juizes.py:1309`)

**Para carregar por janela — inferência de implementação:** ampliar a alimentação com precedência/cobertura/teto, perguntas e decisões por instância, efeitos e portas; realizar uma partição global compartilhada; fornecer o mesmo `agora`; impedir que o corpo puro volte ao ORM, ao relógio ou ao memo de derivação antigo. Os pontos atuais a remover do corpo estão evidenciados nas consultas da precedência, cobertura, C6 e portas. (`app/ponto/precedencia.py:273`, `app/ponto/turnos.py:2552`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:1282`)

**Custo medido nesta cópia, 2,95 queries/colab-dia, meta ≤1 e complexidade efetiva da frota: [nao verificado].** O anexo os fornece como referência/meta; o censo estático não prova esses resultados.

**4. Divergências entre os cinco**

São contraexemplos permitidos pelos predicados; ocorrência e quantidade em produção: **[nao verificado]**.

| Mesmo cenário de colab-dia | Respostas diferentes pelo código |
|---|---|
| Único chamado `registrado` ou com silêncio vigente; nenhuma outra porta | C8 pode devolver o chamado porque usa `VIVOS`; porta pode devolver `None`, pois a fila exige `COBRANCA` e exclui silêncio futuro. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:1382`) |
| Pergunta respondida com texto, sem validação/tranca/decisão que a encerre, sob âncora terminal | C6 pode dizer viva; porta rejeita a âncora fora da fila. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1378`) |
| Pergunta com `respondida_em`, texto vazio e célula acusadora, sem outra porta | C6 pode dizer viva; porta rejeita `respondida_sem_texto`. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1254`, `app/chamados/juizes.py:1380`) |
| Marco continua faltante, pergunta já validada sem plantio | C6 diz morta imediatamente; fábrica não emite porque o motivo já existe; a falta pode permanecer na ata. (`app/chamados/juizes.py:1136`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/declaracao_texto.py:109`) |
| Próprio marco aceso, outro marco do dia faltante, pergunta sem materialização | C6 pode continuar viva pelo veredito acusador do dia: nesse ramo, a luz do próprio marco só encerra com `materializada_em`. A fábrica não reemite o motivo existente. (`app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:123`) |
| Competência trancada, sem

[truncado em 20000 de 27062 caracteres]

## 06/11 22:44 · sessao 7735e49f02c8 · - · - · origem=Ronald (tmux)

Censo de portas fechado por regra explícita: contar **funções que executam a persistência da pergunta** (`save`, `update`, `create`, `delete` ou `bulk_update`), sem contar novamente o helper que só atribui em memória, o chamador que apenas delega, nem ramos/arestas como portas diferentes. O código separa atribuição em memória e persistência em `app/chamados/models.py:2556-2560`, `app/chamados/models.py:2585-2609`, `app/chamados/veredito_lavrado.py:197-207`.

**Resultado: 15 funções para ciclo/estado derivado; 17 incluindo cache; 21 incluindo estado operacional de recusa, pendência e conferência.** São os sítios enumerados abaixo; **“16 portas” do aval não é o total desse recorte vivo**. A lista P13 atual enumera campos/arquivos, não 16 funções, e seu próprio comentário histórico distingue contagem por arquivo de contagem por mão. `app/core/tests/test_contract_escritores_estado.py:61-82`, `app/core/tests/test_contract_escritores_estado.py:83-103`, `app/core/tests/test_contract_escritores_estado.py:133-173`, `app/chamados/services/validacao.py:127-133`.

Não existe `status` persistido em `PerguntaDisputa`: `estado` deriva quatro rótulos de `validada_em`, `materializada_em`, `respondida_em` e `pergunta_viva`. Adotei P=pendente, A=respondida, V=validada, R=resolvida. A fórmula exata é: com validação, V/R conforme exista materialização; sem validação, se viva, P/A conforme exista resposta; se morta e materializada, R; caso contrário P/A. `app/chamados/models.py:2377-2378`, `app/chamados/models.py:2491-2506`.

| # | Persistência de ciclo/estado | Origem → destino permitido pelo código |
|---|---|---|
| 1 | `PerguntaDisputa.abrir` | inexistente → normalmente P; reaproveitada → estado recalculado pela nova célula/destino; `**campos` pode alterar os valores iniciais. `app/chamados/models.py:2398-2405`, `app/chamados/models.py:2430-2477` |
| 2 | `PerguntaDisputa.carimbar` | carimba materialização/via, podendo preencher resposta e validação; só persiste se o C6 disser morta depois da aplicação, portanto destino R. Recusa desfaz em memória e não persiste. `app/chamados/models.py:2562-2614` |
| 3 | `PerguntaDisputa.carimbar_identidade` | estado anterior → estado resultante da via de identidade; não garante R: resposta não validada ainda pode manter A. Mantém instante/autor antigos quando `manter_carimbo=True`. `app/chamados/models.py:2625-2644` |
| 4 | `PerguntaDisputa.descarimbar` | remove materialização/autor/via → V se validação permanecer; caso contrário P/A segundo `respondida_em`. `app/chamados/models.py:2646-2653`, `app/chamados/models.py:2499-2506` |
| 5 | `PerguntaDisputa.descarimbar_em_lote` | mesma retirada dos três campos, mais `outros` definidos pelo chamador; destino depende desses campos adicionais. `app/chamados/models.py:2691-2700` |
| 6 | `PerguntaDisputa.encerrar_por_fato_no_espelho` | troca apenas a via por `fato_materializado`; normalmente pergunta já materializada, podendo mudar vitalidade/rotulagem sem mudar instante. `app/chamados/models.py:2702-2711` |
| 7 | `_carimbar_validacao` | estado sem validação → V se sem materialização, R se materializada. A guardas de resposta/validação anterior estão em `validar_pergunta`, não nesse helper. `app/chamados/services/validacao.py:127-133`, `app/chamados/services/validacao.py:363-368`, `app/chamados/models.py:2499-2501` |
| 8 | `devolver_validacao_recusada` | V → normalmente A; só desfaz validação de resposta de hora não materializada com motivo de recusa. Também pode reabrir a disputa. `app/chamados/services/validacao.py:155-190`, `app/chamados/services/validacao.py:195-212` |
| 9 | `responder_pergunta_disputa_core` | P/A/V/R → resposta nova, instante novo, validação retirada; remove materialização somente das vias sem lastro. Destino normalmente A, podendo permanecer R se materialização sobreviver e C6 disser morta. `app/chamados/services/disputa_emissao.py:1282-1301`, `app/chamados/models.py:2735-2749`, `app/chamados/models.py:2499-2506` |
| 10 | `reabrir_para_correcao` | limpa texto/resposta/validação; com `desfazer_absorcao=True` limpa também materialização/via/autor. Nesse ramo → P. No padrão, materialização pode sobreviver: o destino P prometido pelo comentário não é incondicional pela fórmula de `estado`. `app/chamados/services/disputa_emissao.py:1386-1418`, `app/chamados/models.py:2499-2506` |
| 11 | `responder_automaticamente` | pergunta sem texto → texto+instante; normalmente P→A. Não limpa validação/materialização anteriores; não atua quando já há texto. `app/chamados/services/disputa_emissao.py:1422-1436` |
| 12 | `carimbar_resposta_pela_trilha` | texto existente sem instante → instante da nota; normalmente P→A. Já carimbada ou sem texto é no-op. `app/chamados/services/disputa_emissao.py:1439-1455` |
| 13 | `reapontar_resolvedora` | destino antigo→novo; data nula→data fornecida; relavra cache no mesmo `save`. Não altera resposta/validação/materialização, mas muda insumos da vitalidade. `app/chamados/services/fio.py:829-849` |
| 14 | `relavrar` | via derivada antiga→via atual; relavra cache junto. Com materialização e sem validação, pode alterar R↔P/A pela vitalidade; carimbos de resposta/validação/materialização permanecem. `app/chamados/services/relavra_via.py:104-126`, `app/chamados/models.py:2499-2506` |
| 15 | `DisputaSupervisao.remover_motivo` | pergunta existente→apagada; inclui suas `RespostaAnterior` por cascade. `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |

As duas portas adicionais de **cache** são `lavrar`, que grava `veredito_lavrado`, `julgado_em`, `janela_expira_em`, e `gravar_lavras_em_lote`, que executa o `bulk_update`; o command `backfill_veredito_lavrado` apenas prepara os valores e delega a gravação. Elas não mudam diretamente a property `estado`, mas mudam o estado materializado usado pelos leitores SQL. `app/chamados/veredito_lavrado.py:194-231`, `app/chamados/management/commands/backfill_veredito_lavrado.py:134-140`.

As quatro portas adicionais de **estado operacional** são:

| # | Porta | Origem → destino |
|---|---|---|
| 18 | `_materializar_resposta_hora` | pendência/motivo anteriores → pendência `tipo_nao_conferido` e motivo produzidos pelo cálculo. A mesma função também usa a porta de identidade para paridade. `app/chamados/services/materializacao.py:820-826`, `app/chamados/services/materializacao.py:899` |
| 19 | `_carimbar_recusa` | motivo anterior → motivo atual; na colisão usa também `carimbar_identidade(VIA_FATO_OCUPADO)`. `app/chamados/services/materializacao.py:1058-1077` |
| 20 | `recusar_pergunta` | motivo anterior→recusa humana, depois delega a retirada da resposta/validação para `reabrir_para_correcao`. `app/chamados/services/validacao.py:537-558` |
| 21 | `conferir_data_do_fato` | divergente não conferida→conferida; grava instante/autor, liberando a materialização. Não muda `data_evento`. `app/chamados/services/validacao.py:611-628` |

**Wrappers semânticos separados da contagem acima:** há 16 funções que chamam diretamente as primitivas `carimbar`, `carimbar_identidade`, `descarimbar` ou `descarimbar_em_lote`, excluídas as próprias primitivas e `reabrir_para_correcao`, que já persiste diretamente no ramo padrão. Esse 16 é uma contagem de **chamadores imediatos**, não 16 escritores independentes. Dois deles, `_materializar_resposta_hora` e `_carimbar_recusa`, já aparecem no recorte operacional por outras escritas próprias. `app/chamados/models.py:1504`, `app/chamados/models.py:2522`, `app/chamados/models.py:2721-2756`, `app/chamados/services/materializacao.py:825-826`, `app/chamados/services/materializacao.py:1074-1078`.

| Wrapper | Ato escolhido e referência |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa carimbo, validação e resposta temporal das perguntas sem texto selecionadas pelo C6; →P. `app/chamados/models.py:1434-1458`, `app/chamados/models.py:1492-1504` |
| `PerguntaDisputa.fechar_por_admin` | Via `admin_decidiu`; idem mesma via+carimbo é no-op. `app/chamados/models.py:2508-2523` |
| `PerguntaDisputa.encerrar_fantasma` | Via `fantasma`, sem tocar texto; C6 pode recusar. `app/chamados/models.py:2713-2721` |
| `PerguntaDisputa.reabrir_fantasma` | Retira exclusivamente carimbo de fantasma. `app/chamados/models.py:2723-2733` |
| `PerguntaDisputa.reabrir_por_resposta` | Retira somente carimbo das vias sem lastro, sem salvar; save pertence ao escritor da resposta. `app/chamados/models.py:2735-2749`, `app/chamados/services/disputa_emissao.py:1300-1301` |
| `PerguntaDisputa.reabrir_por_competencia` | Retira exclusivamente via histórica `competencia_trancada`. `app/chamados/models.py:2751-2757` |
| `retratar_duplicata` | Via `duplicata`, conserva instante/autor se já carimbada e escreve a canônica no motivo. `app/chamados/services/fio.py:881-895` |
| `resolver_perguntas_por_chamado_resolvido` | Dois sítios na mesma função: carimbo pela via do fato com auto validação; ramo sem credor carimba com resposta apenas quando há texto. `app/chamados/services/disputa_emissao.py:1617-1618`, `app/chamados/services/disputa_emissao.py:1684-1692` |
| `_materializar_ausencia_de_resposta` | Absorção do dia, vias declaração/ausência, `resposta='validar'`. `app/chamados/services/materializacao.py:548-549` |
| `_materializar_resposta_hora` | Via de identidade `paridade_recusada`. `app/chamados/services/materializacao.py:899` |
| `_carimbar_recusa` | Via de identidade `fato_ocupado` quando aplicável. `app/chamados/services/materializacao.py:1074-1075` |
| `_materializar_uma` | Via declaração ou via sem declaração depois de produzir/conferir efeito. `app/chamados/services/materializacao.py:1243-1254` |
| `materializar_pendentes_aprovadas` | Via `juiz_aprovou`. `app/chamados/services/materializacao.py:1393-1406` |
| `desfazer_carimbo_sem_lastro` | Retira carimbo de pergunta que o C6 já considera viva, sob lock. `app/chamados/services/relavra_via.py:221-231` |
| `lavrar_sem_turno_aberto` | Via `sem_turno_aberto`, sujeita ao C6 com carimbo aplicado. `app/chamados/reconciliador.py:405` |
| `retratar_perguntas_absolvidas_por_celula` | Via `celula_absolveu`, sujeita ao C6. `app/chamados/reconciliador.py:586` |

Os commands não viram escritores extras ao chamar essas portas: encerrar por fato chama `encerrar_por_fato_no_espelho`; reabrir órfãs chama `reabrir_fantasma`; curar limbo chama `carimbar_resposta_pela_trilha`/`reabrir_para_correcao`; relavrar I6 chama `relavrar`. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:147`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-69`, `app/chamados/management/commands/relavrar_via_i6.py:100-102`.

**Estado pode mudar sem escrita na pergunta:** tranca da competência, decisão administrativa/veredito da célula, lâmpada do marco, batida concordante com resposta, classificação da falta, janela temporal e lastro externo são lidos pelo C6. Portanto um censo de `save` na pergunta não fecha todas as mudanças da vitalidade, e relógio/expiração não equivalem a uma decisão append-only já existente. `app/chamados/juizes.py:1135-1218`, `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`.

**Leitores/semântica relevantes ao item 6:**

- `estado` lê os três carimbos e o C6; `.viva` delega diretamente ao C6. `q_vivas` é deliberadamente um prefiltro generoso e não uma implementação equivalente. Logo “estado P/A”, “viva” e “casou q_vivas” não são sinônimos. `app/chamados/models.py:2491-2506`, `app/chamados/models.py:2760-2796`, `app/chamados/models.py:2798-2809`.
- `respondida_em` sozinho decide `.respondida`, enquanto o C6 exige também texto não vazio para o ramo da resposta viva; `responder_automaticamente` testa apenas texto para decidir no-op. Há significados diferentes de “tem resposta”. `app/chamados/models.py:2862-2864`, `app/chamados/juizes.py:1150-1162`, `app/chamados/services/disputa_emissao.py:1431-1435`.
- O materializador seleciona pela nulidade de `materializada_em`; já o C6 pode declarar viva uma pergunta carimbada e morta uma sem carimbo. O help text “batida criada + chamado fechado” não cobre vias fantasma, duplicata, admin e ausência. `app/chamados/services/materializacao.py:1344`, `app/chamados/juizes.py:1135-1218`, `app/chamados/models.py:2827-2829`, `app/chamados/models.py:2625-2632`, `app/chamados/models.py:2713-2721`.
- `validada_em` é instante da decisão no serviço; a porta genérica também pode preenchê-lo automaticamente com o instante do carimbo. Portanto sua existência não prova, sozinha, validação humana. O painel exclui o usuário sistema ao contar validação humana. `app/chamados/services/validacao.py:127-133`, `app/chamados/models.py:2587-2594`, `app/chamados/services/painel.py:255-258`.
- O cache interpreta `julgado_em` como instante da avaliação e `janela_expira_em` como **data** de vencimento. `expira_em` parte de `datetime.date.today()`, enquanto `esta_stale` usa `timezone.now()` e `.date()`. Isso prova referências temporais diferentes no código, não prova divergência concreta de produção. `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`, `app/chamados/veredito_lavrado.py:227-229`.
- `data_evento` é declarado como snapshot do evento, usado como dia do fato para recalcular e como chave de célula; conversa sem data recebe `timezone.localdate()` de nascimento. Assim o mesmo campo já representa dia do fato/turno e dia civil da conversa. `app/chamados/models.py:2218-2220`, `app/chamados/services/validacao.py:404-408`, `app/chamados/models.py:2422-2430`.
- `recusa_motivo` é lido como causa para desfazer validação, mas também guarda referência de duplicata e pendência de tipo; não é sequência histórica nem um único gênero de decisão. `app/chamados/services/validacao.py:180-185`, `app/chamados/services/fio.py:891-895`, `app/chamados/services/materializacao.py:820-826`.
- A conferência da data lê a divergência entre dia do fato e dia apresentado pelo chamado e registra justificativa; não converte `data_evento` nem unifica os relógios. `app/chamados/services/validacao.py:611-625`.

O banco instalado, a execução efetiva dessas portas em produção, o passivo e qualquer número citado em comentários como medição histórica permanecem **[nao verificado]**.

## 06/11 22:44 · sessao 720860fb5d12 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são relativos à raiz; nenhuma escrita executada. O esquema efetivamente instalado em produção permanece [nao verificado].

**(3) Identidade: um `create` central, seis chamadas de produção.**

| Sítio | Busca/chave anterior à criação | Destino |
|---|---|---|
| `app/chamados/models.py:2398` `abrir` | `do_fato`: colaborador via disputa + motivo + `data_evento`, ordenado por PK (`:2392`); pega **última** (`:2431`). Célula é consultada por colaborador+data (`:2430`), mas não integra a chave. | `cls.objects.create` em `:2476`; reaproveitamento muda célula/destino, sem transferir a disputa (`:2435`, `:2450`, `:2474`). |
| `app/chamados/services/disputa_emissao.py:389` `gerar_perguntas_disputa` | Sem data: disputa+motivo (`:299`); motivos datados: disputa+motivo+data (`:370`, `:386`); depois passa pelo dedup global de `abrir`. | Data derivada do evento ou fallback (`:396`). |
| `app/chamados/services/disputa_emissao.py:770` `_criar_pergunta_se_couber` | `_ja_perguntou` (`:765`) usa disputa+motivo+data (`:1237`) e `fio_vivo`, não simples existência (`:1243`). | `abrir` com data/destino (`:771`, `:776`). |
| `app/chamados/services/disputa_emissao.py:1195` âncora | Existe âncora não materializada por chamado_destino+motivo (`:1149`); disputa aberta por colaborador (`:1156`). | `abrir` **sem data**, motivo `ancora_ausente` (`:1197`). |
| `app/chamados/services/acoes_disputa.py:259` reabertura admin | Última disputa fechada do chamado (`:252`), sem dedup de pergunta próprio. | `abrir` sem data (`:260`): confirmação de conversa. |
| `app/chamados/services/declaracao_texto.py:325` declaração colab | `_candidata_reusavel` (`:313`) pega **primeira reutilizável** de disputa+motivo+dia (`:226`), depois fallback a `abrir`. | Motivo reduzido de posição/tipo (`:299`, `:300`), dia informado (`:326`). |
| `app/chamados/services/declaracao_texto.py:614` aplicar declaração admin | Mesma candidata (`:611`), depois `abrir`. | Motivo e dia da proposta (`:614`). |

Na contagem: **1 algoritmo central de acha/cria, 6 chamadores**; nenhuma chamada concreta a `PerguntaDisputa.objects.get_or_create/update_or_create` encontrada [nao verificado como garantia além da busca estática]; central efetivo é `app/chamados/models.py:2431` / `:2476`.

A conversa nasce sempre: o parâmetro original `data_evento=None` desliga o dedup e é convertido ao **dia civil local da emissão**, `timezone.localdate()` (`app/chamados/models.py:2424`). Logo o mesmo campo guarda dia de fato/turno e dia civil da conversa; não há flag persistida distinguindo isso (`:2427`, `:2476`).

**Constraint e locks.** `PerguntaDisputa.Meta` só declara nomes e ordenação (`app/chamados/models.py:2857`); a migration criadora também só esses options (`app/chamados/migrations/0014_perguntadisputa.py:31`). Nenhuma unicidade composta da pergunta está declarada ali; presença real de índice/constraint em PROD [nao verificado]. Há PK e FKs (`app/chamados/models.py:2209`, `:2211`), e índices simples de cache (`:2348`, `:2352`, `:2358`), não a identidade pedida.

`abrir` não trava a busca `last` nem envolve busca+create em atomic; `save` apenas envolve cada gravação individual em atomic (`app/chamados/models.py:2431`, `:2476`, `:2853`). Há locks **externos e parciais**:

- `_abrir_ou_agregar_disputa`: atomic + `select_for_update` na disputa **existente**, por colaborador+aberta (`app/chamados/services/disputa_emissao.py:466`). `criar_ou_atualizar_disputa` só gera as perguntas **depois** do retorno desse helper (`:598`, `:605`). Portanto o helper não estabelece por si só lock cobrindo `last→create` da pergunta.
- Declarações colab/admin mantêm atomic externo (`app/chamados/services/declaracao_texto.py:302`, `:605`), e usam `disputa_para_declaracao` (`:310`, `:608`), que chama o helper travado (`app/chamados/services/disputa_emissao.py:563`).
- Âncora: atomic + lock na disputa existente (`app/chamados/services/disputa_emissao.py:1148`, `:1156`), mas teste de existência da pergunta ocorre antes do lock (`:1149`).
- Agrupador de órfãos tem atomic (`app/chamados/services/disputa_emissao.py:898`) mas a seleção da disputa atual é **sem** `select_for_update` (`:935`), apesar da docstring prometer lock (`:881`).
- Reabertura admin não declara lock próprio (`app/chamados/services/acoes_disputa.py:252`, `:259`).
- Command `disparar_perguntas_competencia`: dedup de existência colab+motivo+dia (`:211`), depois chama fábrica (`:280`), com atomic em apply (`:288`); a fábrica chama `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa` (`app/chamados/services/fabrica_da_celula.py:139`), sem lock próprio da identidade da pergunta.
- Command `smoke_chamados` também chama helper e depois gerador (`app/chamados/management/commands/smoke_chamados.py:99`, `:102`).

A corrida não foi executada/provada em banco [nao verificado]; a janela estática `last→create` é visível em `app/chamados/models.py:2431` / `:2476`.

**Instância de marco candidata.** A pergunta só tem FK célula, motivo e data, sem campo de marco/ordem (`app/chamados/models.py:2209`, `:2216`, `:2218`). O vocabulário existente apropriado é a chave do DNA da célula, **`hi/hii/hfi/hii2/hfi2/hf`**, na ordem canônica declarada por `CelulaDia.chaves_do_turno` (`app/escala/models.py:1652`, `:1659`), exposta por `marcos_presentes` como `(tipo,chave,hora)` (`:1672`). Candidato de desenho, não campo já existente: `instancia_marco = chave` qualificada pela célula; a decisão sobre identidade através de regeneração do DNA [nao verificado].

Há prova concreta de perda da instância: `_MOTIVO_POR_MARCO` só distingue início/fim/ida/volta (`app/chamados/services/declaracao_texto.py:42`); marcos internos são todos reduzidos a `int_ida` ou `int_volta` pelo tipo (`:118`). O juiz admite que duas pausas deixam “duas luzes para uma resposta” e retorna indeterminado (`app/chamados/juizes.py:1106`). Leitores do vocabulário de marco: cartório lê `chaves_do_turno` (`app/ponto/services/cartorio.py:586`); pauta lê `marcos_presentes` (`app/pautas/services.py:417`).

**(4) Absorção: reconstrução por coincidência, sem fato/FK do ato.**

`_materializar_ausencia_de_resposta` cria/retorna ausência por colab+data+tipo (`app/chamados/services/materializacao.py:520`), captura **um** `agora` (`:529`), varre perguntas-hora não materializadas da mesma disputa/data (`:531`) e tenta carimbar cada uma com esse instante (`:549`). O escape e irmãs mudas usam via `ausencia_dia`; quem declarou hora usa `declaracao` (`:548`). O carimbo pode ser recusado pelo C6, e a irmã então fica fora do grupo, com evento `irma_viva_na_ausencia` (`:550`, `:554`).

O leitor `perguntas_absorvidas_por` limita colab/dia/tipo de pergunta/via (`app/chamados/services/materializacao.py:605`), encontra escapes compatíveis com o tipo da ausência e extrai pares `(disputa_id,materializada_em)` (`:602`, `:609`), recupera o grupo (`:614`) e exclui perguntas com batida-origem viva (`:616`). Portanto a ligação é hoje **inferida** por `(disputa,data_evento,materializada_em)` + tipo/dia da ausência, não registrada como ato explícito (`:588`).

Esmeril adicional: a docstring chama `(colab,tipo,data)` de “chave única da ausência” (`app/chamados/services/materializacao.py:595`), mas o modelo atual declara unicidade **parcial somente não rejeitada** (`app/ponto/models.py:827`). Ao recriar/reabrir, o serviço escolhe a última rejeitada do tipo/dia (`app/ponto/services/ausencia.py:254`). Assim o trio não identifica inequivocamente uma ausência histórica rejeitada; a ligação histórica por FK permanece ausente (`app/chamados/services/materializacao.py:588`).

O ato que precisaria carimbar ligação explícita é a própria absorção bem sucedida: ausência efetiva devolvida por `_criar_ausencia` + pergunta escape + irmãs aceitas pela porta + instante comum (`app/chamados/services/materializacao.py:520`, `:529`, `:549`). Hoje só a irmã **recusada** recebe evento com IDs explícitos de escape/ausência (`:554`).

**Quem mais escreve os três campos:**

- `disputa`: atribuição no nascimento (`app/chamados/models.py:2476`); não encontrei transferência posterior da FK da pergunta. Reapontar altera `chamado_destino`, não disputa (`app/chamados/services/fio.py:845`).
- `data_evento`: nascimento (`app/chamados/models.py:2476`); `reapontar_resolvedora` preenche apenas se vazia (`app/chamados/services/fio.py:832`, `:834`, `:847`). `None` no ramo de recusa é só desfazer em memória (`:843`).
- `materializada_em`: atribuição única em `_escrever_carimbo` (`app/chamados/models.py:2558`), persistida por `carimbar` (`:2609`), `carimbar_identidade` (`:2644`), `descarimbar` (`:2652`) e limpeza em lote (`:2699`). Chamadores com outros atos compartilham o campo:
  - fechar admin (`app/chamados/models.py:2522`);
  - fantasma/reabrir fantasma/por resposta/por competência (`:2721`, `:2732`, `:2749`, `:2756`);
  - descarimbo na ressurreição (`:1504`);
  - duplicata (`app/chamados/services/fio.py:891`);
  - reabertura correção (`app/chamados/services/disputa_emissao.py:1414`);
  - chamado resolvido (`:1617`) e resolução sem resposta (`:1691`);
  - paridade/colisão/materialização/juiz aprovou (`app/chamados/services/materializacao.py:899`, `:1074`, `:1252`, `:1406`);
  - sem turno/célula absolveu (`app/chamados/reconciliador.py:405`, `:586`);
  - desfazer carimbo sem lastro (`app/chamados/services/relavra_via.py:231`).
  - revalidação externa monta `descarimbar(salvar=False)` em memória (`app/chamados/models.py:1455`) e restaura estado para consultar juiz; checar contexto antes de contar como gravação.
  
O help_text diz “batida criada + chamado fechado” (`app/chamados/models.py:2829`), enquanto o campo também representa ausência, fantasma, duplicata e recusa (`app/chamados/services/materializacao.py:549`, `app/chamados/models.py:2721`, `app/chamados/services/fio.py:891`, `app/chamados/services/materializacao.py:899`). O significado não é uniforme; usar esse instante como identidade permanente de absorção depende de um campo reutilizado e apagável (`app/chamados/models.py:2699`).

**Leitores para mapa do item (6):**

| Campo | Leitores comprovados |
|---|---|
| colaborador via disputa | identidade `app/chamados/models.py:2392`; app `app/chamados/services/pergunta_no_app.py:46`; folha agrupa `app/folha/previa.py:67`; absorção `app/chamados/services/materializacao.py:606`. |
| disputa / disputa_id | candidata por disputa `app/chamados/services/declaracao_texto.py:227`; absorção agrupa `app/chamados/services/materializacao.py:609`; explicador recorta disputa `app/chamados/services/explicador.py:122`; juiz usa colaborador da disputa `app/chamados/juizes.py:1200`. |
| motivo | identidade `app/chamados/models.py:2392`; candidata `app/chamados/services/declaracao_texto.py:227`; fábrica agrupa/dedup por motivo `app/chamados/services/fabrica_da_celula.py:122`; declaração reduz marco a motivo `app/chamados/services/declaracao_texto.py:300`. |
| data_evento | identidade `app/chamados/models.py:2393`; materializador escolhe dia da batida `app/chamados/services/materializacao.py:344`; absorção `:606`; folha agrupa dia `app/folha/previa.py:65`; expiração `app/chamados/veredito_lavrado.py:112`; juiz janela `app/chamados/juizes.py:1197`; fábrica dia da célula `app/chamados/services/fabrica_da_celula.py:118`; badge/modelo `app/chamados/models.py:373`; app dia `app/chamados/services/pergunta_no_app.py:92`. |
| celula | gate reaponta `app/chamados/models.py:2435`; juiz usa veredito admin e acusação `app/chamados/juizes.py:1147`, `:1181`; outbox escolhe célula ou fallback colab+dia `app/chamados/models.py:2681`. |
| materializada_em | `estado` (`app/chamados/models.py:2501`); C6 (`app/chamados/juizes.py:1171`, `:1177`, `:1207`); SQL vivas (`app/chamados/models.py:2784`); absorção (`app/chamados/services/materializacao.py:609`); frescor da lavra via `Max` (`app/chamados/veredito_lavrado.py:162`); fila materialização (`app/chamados/services/materializacao.py:1347`). |
| chamado_destino | guarda de costura (`app/chamados/services/fio.py:838`); reuso declaração (`app/chamados/services/declaracao_texto.py:314`); absorção decide reconciliar (`app/chamados/services/materializacao.py:563`); juiz/estado usa FK por outras funções [nao verificado para lista completa]. |
| marco/chave/ordem | `CelulaDia.marcos_presentes` (`app/escala/models.py:1679`), cartório (`app/ponto/services/cartorio.py:586`), pauta (`app/pautas/services.py:417`); declaração perde chave ao reduzir posição+tipo (`app/chamados/services/declaracao_texto.py:118`). |

Mistura temporal adicional: o decodificador de contexto prioriza `data_turno`, mas também aceita `data_batida`, data textual de timestamp de entrada e horário previsto (`app/chamados/data_evento.py:27`); `date.fromisoformat(v[:10])` extrai a data escrita sem conversão de timezone (`:11`). O relógio de expiração usa `datetime.date.today()` (`app/chamados/veredito_lavrado.py:112`), conversa usa `timezone.localdate()` (`app/chamados/models.py:2427`), e batida materializada combina `data_evento` com HH:MM e depois ajusta overnight (`app/chamados/services/materializacao.py:414`, `:420`). Equivalência universal de dia civil/dia de turno não está comprovada e o código mostra domínios distintos nesses pontos.

## 06/11 22:43 · sessao ae89694c2775 · - · - · origem=Ronald (tmux)

Censo somente estático; os pontos abaixo descrevem esta cópia, sem afirmar ocorrência em produção. Fontes nas respectivas linhas.

**1. `cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`**

Responde `ChamadoColaborador | None`: chamado mais novo do colaborador e módulos pedidos, com `status_local ∈ VIVOS`, dia forte igual ao solicitado, excluindo opcionalmente o chamado indicado. Ordena por `-criado_em,-pk`; não confere exigibilidade de marco, batida, ausência, decisão nem silêncio/arquivo. `app/chamados/juizes.py:1886-1902`.

Lê `ChamadoColaborador.colaborador`, `modulo_origem`, `status_local`, `contexto_json.data_turno`, `celula.data`, `criado_em`, `pk`; no `dia_do_fato(..., fortes=True)`, a célula vence, depois a chave de data declarada pelo catálogo e `data_turno`. `app/chamados/juizes.py:1897-1902`, `app/chamados/juizes.py:80-121`.

Relógio: recebe `dia`; não lê o instante atual nesse percurso. O `criado_em` serve à ordenação. `app/chamados/juizes.py:1886-1902`.

Todos os chamadores diretos encontrados no código de produção, incluindo alias:

- `ChamadoColaborador._plano_pergunta_viva`: `app/chamados/models.py:373`.
- `ChamadoColaborador._gate_premissa_do_fechamento`: `app/chamados/models.py:711`.
- `_julgar_colab_corpo` via `_cobr_c8`: `app/ponto/services/cartorio.py:1604`.
- `resolver_cobranca_da_entrada`: `app/ponto/services/entrada_sincronizada.py:28`.
- `_autoresolve_f2_f4`: `app/ponto/services/triagem_batida.py:589`.

**2. `pergunta_viva(p)`**

Responde bool, com memo na instância (`_pv_memo`); delega ao cálculo, não é função pura de argumentos explícitos: lê ORM, relógio interno, contexto/memos e escreve o memo no objeto. `app/chamados/juizes.py:822-848`, `app/chamados/juizes.py:1197-1218`.

Precedência efetiva:

1. `validada_em` preenchida → morta. `app/chamados/juizes.py:1136`.
2. Competência trancada, salvo toque humano posterior, ou via legada `competencia_trancada` materializada → morta. `app/chamados/juizes.py:1140-1146`, `app/chamados/juizes.py:919-954`.
3. Resposta com texto e carimbo → viva, exceto admin decidiu o dia ou resposta concorda com a luz e o dia não acusa. `app/chamados/juizes.py:1148-1162`, `app/chamados/juizes.py:1087-1132`.
4. Via de identidade com materialização → morta. `app/chamados/juizes.py:1170-1172`.
5. Materializada com luz do marco acesa → morta. `app/chamados/juizes.py:1177-1179`.
6. Célula acusadora → viva; célula não acusadora absolve quando não há marcos pendentes ou a luz da pergunta acendeu. `app/chamados/juizes.py:1181-1196`.
7. Na janela viva, `classificar_falta` acusando → viva. `app/chamados/juizes.py:1197-1203`.
8. Sem materialização → viva; com materialização, exige testemunha externa, ou morre se esse último juiz falhar. `app/chamados/juizes.py:1207-1218`.

Entradas diretas: `PerguntaDisputa.validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `motivo`, `celula`, `disputa.colaborador`, `chamado_destino`; célula: `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas` (`hora,tipo,acesa,luz`), marcos pendentes. `app/chamados/juizes.py:1135-1218`, `app/chamados/juizes.py:886-916`, `app/chamados/juizes.py:1099-1132`.

Entradas transitivas relevantes:

- Empresa/corte da janela, `PeriodoFechamento.fechado_em`, toque humano posterior em `LogAuditoria`; atos `cobrar_dia_manual`, `veredito_celula`, `desarquivar_chamado`, com usuário diferente do sistema. `app/chamados/juizes.py:935-954`, `app/chamados/juizes.py:961-963`, `app/chamados/juizes.py:1065-1084`.
- `Batida` ligada à pergunta e não retratada, batida no slot (`timestamp,tipo,retratada_em`), slot derivado pelo materializador, completude via `turnos_do_colab`, trilha `retratar_pergunta_obsoleta`; fallback lê `chamado_destino.status_local ∈ ENCERRADOS`. `app/chamados/juizes.py:326-347`, `app/chamados/juizes.py:350-372`, `app/chamados/juizes.py:271-282`, `app/chamados/juizes.py:718-778`.
- `classificar_falta` lê célula (`trabalha,dna`), ausências de cobrança (`tipo,minutos,data,data_fim`), vínculo/tipo do dia, marcos congelados e turnos com batidas apuráveis. `app/escala/servico_jornada.py:302-304`, `app/escala/servico_jornada.py:340-355`, `app/escala/servico_jornada.py:391-435`.

Relógios ocultos: `_dentro_da_janela_viva` lê `date.today()` e monta janela anterior+corrente sem empresa explícita; `classificar_falta` lê `_tz.now()` contra o fim previsto. Não recebe `agora`. `app/chamados/juizes.py:556-588`, `app/escala/servico_jornada.py:375-378`.

Dependência de estado gravado: `validada_em` mata antes de conferir efeito; `via_resolucao`+`materializada_em` encerram por identidade; `celula.veredito/veredito_via/ata` e `destino.status_local` também entram como projeções/estado. Não lê um campo persistido `estado` da pergunta: a property `estado` é derivada dela. `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1170-1196`, `app/chamados/juizes.py:743-759`, `app/chamados/models.py:2492-2506`.

Todos os chamadores diretos de produção identificados por AST, com aliases resolvidos:

- Juízes auxiliares: `pergunta_sem_lastro_ou_viva:1225`, `perguntas_vivas_de:1230`, `fio_vivo:1242`, `alimentar_portas:1302`, `perguntas_respondidas_a_validar:1403`, `_dias_das_perguntas_vivas:2031`, todos em `app/chamados/juizes.py`.
- Modelos: `_vivas_entre:351`, `_universo_da_ressuscitacao:1458`, `pendentes_do_colaborador:1739`, `DisputaSupervisao.fechar:1837`, `PerguntaDisputa.estado:2502`, `carimbar:2596`, `viva:2809`, todos em `app/chamados/models.py`.
- Views: `responder_pergunta_disputa:1242,1257`, `reabrir_disputa_supervisao:1417`, em `app/chamados/views.py`; `api_disputa_responder` em `app/api/views.py:2631`.
- Relatório `classificar`: `app/chamados/relatorios_disputa.py:18`.
- Reconciliador: `lavrar_sem_turno_aberto:398`, `perguntas_absolvidas_pela_celula:651`, `_viva_em_competencia_aberta:748`, `veredito_orfa:1075`, `reatar_pergunta_orfa:1579`, em `app/chamados/reconciliador.py`.
- Calendário `contexto_calendario`: `app/colaboradores/services/calendario.py:476`; `dia_do_colab`: `app/ponto/services/dia_do_colab.py:71`.
- Fio: `montar_chamados_fio:234`, `processar_post_fio:602`, `_costura_nao_mata_o_fio:775`, `migrar_resolvedoras_vivas:932,935`, em `app/chamados/services/fio.py`.
- App: `perguntas_do_app:53`, `_marco_ja_perguntado:111`, em `app/chamados/services/pergunta_no_app.py`.
- `montar_contexto_detalhe`: `app/chamados/services/acoes_chamado.py:348`.
- `resolver_perguntas_por_chamado_resolvido`: `app/chamados/services/disputa_emissao.py:1597` e `:1679`.
- `medir`: `app/chamados/services/competencia_trancada.py:122`, `:132`, `:142`.
- `desfazer_carimbo_sem_lastro`: `app/chamados/services/relavra_via.py:227`.
- `perguntas_pendentes_do_colab`: `app/chamados/services/aviso_pendencia.py:13`.
- Vigia: `viva_em_disputa_fechada:755`, `carimbos_sem_lastro_por_dono:813`, em `app/chamados/services/vigia_fio.py`.
- Comandos operacionais: `disparar_perguntas_competencia.handle`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `reabrir_orfas_enterradas.handle`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:83`; auditor `_contraprova_f1t:202`, `_i6_pares:540`, `handle:858,894,1650,1707,1766,1779`, em `app/chamados/management/commands/auditar_invariantes_chamados.py`.

A property `.viva` é outra entrada transitiva que delega integralmente ao juiz; o inventário acima é de chamadas diretas da função, não de todos os acessos às properties. `app/chamados/models.py:2800-2809`.

**3. `motivos_a_perguntar(cel, hoje, agora=None, *, carimbadas=None, julgada_antes=_JULGADA_DA_CELULA)`**

Responde lista de motivos novos da célula; opcionalmente acrescenta IDs a `carimbadas`. Isso significa “o que ainda falta emitir”, não “todas as ações atualmente exigíveis”. `app/chamados/services/fabrica_da_celula.py:74-85`, `:118-126`.

Lê célula/colaborador (`data,trabalha,ata,veredito,julgada_em,colaborador.situacao`), `dia_sem_trabalho`, teto por instante e marcos do vínculo; corta futuro, inativo, dia sem trabalho, veredito não acusador e `parcial_so_lista`. `app/chamados/services/fabrica_da_celula.py:88-104`, `app/ponto/services/cartorio.py:831-848`.

Dedup: lê **qualquer** `PerguntaDisputa` do colab-dia, exclui apenas `VIA_DUPLICATA`, junta por `motivo`; não aplica `pergunta_viva` ao conjunto `ja` e não discrimina instância do marco. `app/chamados/services/fabrica_da_celula.py:118-126`.

`carimbadas`: usa `materializada_em`, `via_resolucao` fora das identidades e `disputa.fechada_em is None`; são elegibilidade de desfazer, não vitalidade. `app/chamados/services/relavra_via.py:169-182`.

Faltantes saem de `_ata_do_dia`→`_marcos_faltantes`→`grade_da_celula`, olhando dia e véspera e lampadas apagadas; sem escala e sem ata, pode recorrer a `classificar_falta`. Os motivos são comprimidos, sem duplicar motivo para duas pausas. `app/chamados/services/fabrica_da_celula.py:34-65`, `app/chamados/services/disputa_emissao.py:818-844`, `app/chamados/services/declaracao_texto.py:65-127`.

`parcial_so_lista` depende de `cobertura_parcial`, `periodo_parcial`, julgamento anterior e fato novo; este lê Batida criado/retratada, Ausencia criado/atualizado/aprovada, Justificativa criado/atualizado/aprovada, célula regenerada/editada e marco que venceu. `app/ponto/services/cartorio.py:329-400`.

Relógio: `hoje` e `agora` vêm por parâmetro; se `agora=None`, lê `timezone.localtime()`. O ramo fallback `classificar_falta` continua lendo `_tz.now()` mesmo com `agora` passado. `app/chamados/services/fabrica_da_celula.py:93-98`, `:55-57`, `app/escala/servico_jornada.py:375-378`.

Únicos chamadores diretos em produção: `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:164`) e `motivos_que_o_evento_deixou` (`:199`).

**Contradições/casos estáticos úteis à consolidação**

- `registrado` é `VIVOS`, portanto `cobranca_viva_do_dia` o devolve; `registrado` não é `COBRANCA` e não pede ação. Diferença de universo já no nome “cobrança viva”. `app/chamados/juizes.py:1894-1902`, `app/chamados/catalogo/motor.py:24-29`.
- Chamado `VIVOS` de dia já coberto/folga/marco aceso continua retornável pelo C8; fábrica pode dar `[]` por dia sem trabalho ou veredito não acusador, pergunta pode estar morta. É construção permitida pelos predicados, ocorrência no dado raspado [nao verificado]. `app/chamados/juizes.py:1897-1902`, `app/chamados/services/fabrica_da_celula.py:91-100`, `app/chamados/juizes.py:1177-1196`.
- Dia acusa, pergunta validada sem plantio: `pergunta_viva=False` imediatamente; fábrica não emite porque já tem o motivo; falta continua na ata. `app/chamados/juizes.py:1136-1137`, `app/chamados/services/fabrica_da_celula.py:118-126`, `app/chamados/services/declaracao_texto.py:109-123`.
- Pergunta sem carimbo de materialização, próprio marco aceso e outro marco faltante: C6 pode retornar viva por `cel.veredito ∈ VEREDITOS_ACUSAM`; o teste da luz exige `materializada_em` naquele ramo. Fábrica não reemite o motivo já existente. `app/chamados/juizes.py:1177-1184`, `app/chamados/services/fabrica_da_celula.py:118-126`.
- Competência trancada sem toque: C6 dá morta; fábrica não consulta tranca na seleção e pode listar um motivo novo. `app/chamados/juizes.py:1140-1141`, `app/chamados/services/fabrica_da_celula.py:88-126`.
- Duas pausas: C6 da pergunta de intervalo exige luz de **todas** as pausas; fábrica converte várias instâncias faltantes no mesmo motivo e colapsa em uma saída. Nenhum dos dois entrega a identidade de cada instância exigida pelo v4. `app/chamados/juizes.py:894-916`, `app/chamados/services/fabrica_da_celula.py:59-65`, `:123-126`.

## 06/11 22:43 · sessao f2d92a16f8d6 · - · - · origem=Ronald (tmux)

Somente leitura; censo estático da cópia.

**`porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`**

- Retorna `None` ou `{rota, fila, verbos, fio}`; soma chamados da célula na fila e perguntas vivas no app, com âncora na fila e resposta não vazia quando respondidas. O fio prioriza `cobrar→colab`, depois `validar→admin`, senão `chamado`. `app/chamados/juizes.py:1327-1338,1341-1387`.
- Lê, via `alimentar_portas`, a partição **global** dos chamados vivos, pertença à fila global, perguntas das células julgadas pelo C6 e célula→colaborador; o app é carregado sob demanda, uma vez por colaborador no mapa do lote. `app/chamados/juizes.py:1282-1320`.
- Campos explícitos: chamado `id/modulo_origem/silenciado_ate/criado_em/urgencia`, perguntas `respondida_em/resposta_colab/chamado_destino_id/disputa.chamado_id`, além das entradas transitivas de C6 e do app. `app/chamados/services/verbos.py:44-57`; `app/chamados/juizes.py:1254-1257`; `app/chamados/services/fio.py:116-117`.
- Não recebe relógio: a partição lê `timezone.now()` e `qs_na_fila` lê outro `timezone.now()`. Portanto uma alimentação pode cruzar fronteira de silêncio entre as duas leituras. `app/chamados/services/verbos.py:35,48-51`; `app/chamados/services/painel.py:186-188`.
- Chamadores diretos de produção: `vigia_fio.censo_alcance`, `cadastro_realidade.beco_em_lote`, `fora_da_folha._porta`; esta última alimenta `_fio_do_dia`, usado por `fora_da_folha` e `o_que_segura`, e é chamada novamente para listar os chamados de `o_que_segura`. `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:460`; `app/folha/fora_da_folha.py:65-78,178,221-225`.

**`dono_fora_do_fio(colab, data, perguntas, lista, com_papel=False, *, oferecido)`**

- Retorna, nesta ordem: `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `fio_do_admin`, ou `None`; com `com_papel=True`, retorna `(balde,papel)`. `app/chamados/services/vigia_fio.py:274-315`.
- Lê `oferecido` já calculado; ausência cobrindo a data para cobrança e seu status em `ABERTAS`; todas as perguntas da célula, buscando `estado==VALIDADA` e código `FOLGA_CONTESTA`; presença e dono lavrado na lista do admin. `app/chamados/services/vigia_fio.py:306-314`; `app/chamados/services/validacao.py:216-218`.
- Ausência usa `colaborador/data/data_fim/tipo/status`, selecionando maior `(data,pk)`; cobrança aceita toda não rejeitada. `app/ponto/turnos.py:2509-2517,2536-2557`.
- `p.estado` **é property**, não coluna de estado: validação sem materialização→`VALIDADA`; validação com materialização→`RESOLVIDA`; os demais ramos consultam C6 e carimbos. `app/chamados/models.py:2492-2506`.
- Não recebe nem lê relógio diretamente. O `oferecido` vem do calendário, que lê seu próprio `timezone.now()` e deriva `hoje`; a lista recebe `hoje/agora` no construtor e passa ambos a `cadastro_realidade.lista`. `app/chamados/services/vigia_fio.py:199-216,258-271`; `app/colaboradores/services/calendario.py:117-138,159-164`.
- Todos os chamadores de produção encontrados: `censo` e `censo_alcance`. `app/chamados/services/vigia_fio.py:431,654-656`.

**Carga e custo estático**

- Existe `CargaDaJanela(colaboradores, ini, fim)`, carregador dos mesmos corpos de `turnos_do_colab`, `dias_encerrados` e `dia_das_batidas`. Usa janela com pad, células/geradoras, candidatos a vínculo, todos os vínculos com tipo, atas; batidas apuráveis e última batida são carregadas sob demanda em lote, turnos memorizados por colaborador. `app/ponto/turnos.py:2922-2964,2974-3010`.
- Não carrega atualmente perguntas/decisões, chamados/partição global, ausências/cobertura ou precedência: o construtor e os métodos existentes terminam nas entradas e respostas acima. `app/ponto/turnos.py:2936-3010`.
- Os nomes literais `precedencia_cobertura_teto` e `portas_com_universo_global` não foram encontrados por busca em todo o repositório: existência dessas funções nesta cópia **[nao verificado]**.
- Composição existente para portas: `alimentar_portas` chama `particionar_verbos(qs_vivos())` **global**, depois `qs_na_fila()` **global**, filtra perguntas pelas células pedidas e reutiliza o lote. Chamá-la por célula repetiria o universo global; os leitores existentes passam alimentação em lote. `app/chamados/juizes.py:1285-1306,1375`; `app/chamados/services/vigia_fio.py:604,635`; `app/escala/services/cadastro_realidade.py:456,460`; `app/folha/fora_da_folha.py:166,218`.
- Ainda há consultas por pergunta/colaborador no C6 e app; o próprio carregador declara essa limitação. `_no_app` memoiza por colaborador, não por dia. `app/chamados/juizes.py:1260-1262,1309-1320`.
- Precedência de um dia delega a `fatos_do_periodo(colab,data,data)`; período aceita feriados/vínculos/foto/células previamente carregados, mas ainda consulta ausências e folgas por colaborador. `app/ponto/precedencia.py:242-282`.
- Cobertura pontual faz `.first()` por colab-dia; `cobertura_ausencia_periodo` já fornece uma consulta por colaborador para toda a janela com o mesmo desempate. `app/ponto/turnos.py:2536-2557,2733-2770`.
- O teto da pergunta passa por `vencidos_no_instante→cartorio.marcos_vencidos(colab,cel,cel.data,agora)`; não equivale simplesmente a “dia encerrado”, pois mantém ramo de passado sem marcos. `app/chamados/services/fabrica_da_celula.py:15-31`.
- Para janela: ampliar a alimentação com resultados de precedência/cobertura/teto, decisões e perguntas por instância, e partição global única; eliminar consultas internas dos corpos puros e passar o mesmo `agora` às leituras temporais. Isto é necessidade inferida dos pontos de consulta atuais, não implementação existente. `app/ponto/turnos.py:2936-3010`; `app/ponto/precedencia.py:273-282`; `app/chamados/juizes.py:1282-1320`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:188`.

**Divergências concretas pelo código**

1. Um único chamado `registrado` ou silenciado pode ser retornado pelo C8 porque ele filtra `VIVOS`, mas dar porta `None`, pois a fila exige `COBRANCA` e silêncio vencido, supondo nenhuma outra porta. `app/chamados/juizes.py:1894-1902`; `app/chamados/services/painel.py:186-188`; `app/chamados/juizes.py:1382-1383`.
2. Pergunta respondida com texto, não validada/trancada, sem decisão admin nem concordância com a luz, pode viver mesmo sob âncora terminal; a porta exige âncora na fila e pode retornar `None`. `app/chamados/juizes.py:1135-1162,1378-1383`.
3. Pergunta com `respondida_em` e texto vazio pode viver se a célula acusa; a porta rejeita explicitamente `respondida_sem_texto`. `app/chamados/juizes.py:1150,1181-1184,1254-1257,1380`.
4. Porta exclui explicitamente “Resolver dia”; `dono_fora_do_fio` responde decisão pendente quando `oferecido=True`, mesmo sem porta. Também pode responder ausência aberta. São respostas de escopos distintos. `app/chamados/juizes.py:1367-1369`; `app/chamados/services/vigia_fio.py:306-310`.
5. Existência da porta lê C6, mas verbo lê crédito SQL com `q_vivas` e exclui materialização cujo `veredito_lavrado=sim`; o campo pode ficar velho até o cron. Assim pergunta viva e verbo atribuído à âncora podem divergir. `app/chamados/juizes.py:1361-1366`; `app/chamados/services/raiox.py:72,110-118,133-149`.
6. Folga aceita com `validada_em` **e** `materializada_em` vira `RESOLVIDA`; o ramo de dono exige literalmente `VALIDADA`, logo não reconhece essa linha como `escala_nao_descreve_o_dia` — dependendo dos demais ramos, pode cair na lista ou em `None`. `app/chamados/models.py:2499-2501`; `app/chamados/services/vigia_fio.py:311-315`.
7. O que a fábrica considera “ainda perguntar” exclui todo motivo já presente, salvo duplicatas, independentemente de sua vitalidade; uma pergunta existente viva pode significar C6=True e `motivos_a_perguntar=[]`. `app/chamados/services/fabrica_da_celula.py:118-126`.

Ocorrência dessas classes em produção, quantidade de consultas e custo de frota nesta execução: **[nao verificado]**.

## 06/11 22:43 · sessao e59c7c967e20 · - · - · origem=Ronald (tmux)

Censo dos itens 1–2, somente código; existência/conteúdo das trilhas em produção [nao verificado].

**1. Escritores de decisões na mesma linha**

| Ato/escritor | Campos e perda do anterior | Preservação encontrada |
|---|---|---|
| `responder_pergunta_disputa_core` | Substitui `resposta_colab` e `respondida_em`; se já validada, zera `validada_em/por`; pode descarimbar absorção sem lastro. `app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1300`. | Não cria `RespostaAnterior`. Evento conserva a validação anterior, mas não a resposta anterior (`:1309`); log da nova resposta é truncado em 24 caracteres (`:1318`–`:1321`); nota conserva a nova resposta/display, com deduplicação e execução best-effort (`:1346`–`:1358`). Não garante sequência completa de correções. |
| Escape `FOLGA_CONTESTA/OUTRO_MOTIVO`, no mesmo core | Substitui `recusa_motivo` por texto “Colaborador respondeu…”, truncado a 240. `app/chamados/services/disputa_emissao.py:1331`, `:1340`. | Sem snapshot do motivo anterior e sem RA nesse ato; nota da resposta vem depois (`:1343`–`:1358`). |
| `reabrir_para_correcao` | Zera `respondida_em`, `validada_em/por`, `resposta_colab`; com `desfazer_absorcao=True`, também `materializada_em/por` e `via_resolucao`. `app/chamados/services/disputa_emissao.py:1393`, `:1413`–`:1416`; `app/chamados/models.py:2699`. | Cria RA antes do update, no mesmo atomic, apenas se havia texto ou `respondida_em`; copia resposta e validação, mas não recusa/materialização/via (`disputa_emissao.py:1401`–`:1412`). |
| `responder_automaticamente` | Escreve texto e `respondida_em` quando texto vazio; pode substituir carimbo existente com texto vazio. `app/chamados/services/disputa_emissao.py:1431`–`:1435`. | Não cria RA, nem evento próprio nessa função (`:1422`–`:1436`). |
| `carimbar_resposta_pela_trilha` | Preenche somente `respondida_em` nulo, com instante passado; não substitui carimbo preenchido. `app/chamados/services/disputa_emissao.py:1448`–`:1451`. | Evento com novo instante/motivo, sem estado anterior necessário porque guarda exige nulo (`:1452`–`:1454`). |
| `_carimbar_validacao`, chamado pelo validar e pelo aceite de folga | Atribui `validada_em=now`, `validada_por=autor`. `app/chamados/services/validacao.py:127`–`:133`, `:263`, `:290`, `:396`. | `validar_pergunta` recusa carimbo já preenchido (`:366`–`:368`); registra log de validação (`:440`–`:443`). Não cria linha RA a cada aceite. |
| `devolver_validacao_recusada` | Zera `validada_em/por` quando materialização de hora foi recusada e há motivo; resposta fica. `app/chamados/services/validacao.py:180`–`:190`. | Evento `validacao_devolvida` contém antes/depois de assinatura, motivo e fechamento da disputa (`:205`–`:212`); sem RA. A foto anterior fornecida pelo fluxo de validar é anterior ao carimbo recém-tentado (`:384`–`:396`, `:425`). |
| `recusar_pergunta` | Substitui `recusa_motivo`; chama reabertura que limpa resposta/carimbos. `app/chamados/services/validacao.py:541`–`:547`. | RA pela reabertura; nota guarda resposta anterior e motivo; evento guarda resposta, horário e recusa anteriores (`:551`–`:560`). **Não passa `por/motivo` à RA** (`:547`). |
| `_materializar_batida_retroativa` — pendência de tipo | Substitui `pendencia_admin/recusa_motivo` pelo primeiro resultado de `_ts_tipo_do_plantio`. `app/chamados/services/materializacao.py:820`–`:826`. | Sem RA e sem snapshot anterior nesse bloco (`:820`–`:828`). |
| `_carimbar_recusa` | Substitui `recusa_motivo` somente se diferente; colisão também carimba identidade `fato_ocupado`. `app/chamados/services/materializacao.py:1064`–`:1078`. | Nota no chamado-destino quando muda; contém recusa traduzida atual, não motivo anterior (`:1079`–`:1082`). Sem destino, não escreve essa nota (`:1080`). |
| `carimbar` | Substitui `materializada_em/por/via`; pode preencher resposta/validação nulas. `app/chamados/models.py:2583`–`:2594`. | Foto anterior serve para desfazer **em memória** em caso de recusa/exceção; não é fato persistido de histórico (`:2595`–`:2613`). RA não é chamada. |
| `carimbar_identidade` | Substitui carimbo/via e, quando fornecida, recusa; `manter_carimbo=True` preserva instante/autor anterior, mas troca via. `app/chamados/models.py:2636`–`:2644`. | Não contém histórico próprio; trilha depende de quem chama (`:2625`–`:2644`). |
| `fechar_por_admin` | Encaminha para identidade `admin_decidiu`; carimbo de outra via pode ser sobrescrito. `app/chamados/models.py:2513`–`:2522`. | A guarda só impede repetir o próprio `admin_decidiu` já materializado (`:2513`–`:2521`). Sem RA. |
| `retratar_duplicata` | Via `duplicata`; substitui recusa pelo apontamento da canônica; preserva carimbo anterior quando existe. `app/chamados/services/fio.py:882`–`:894`. | Conserva ambas perguntas; não conserva antiga via/recusa nesse ato (`:878`–`:895`). |
| `descarimbar` / `descarimbar_em_lote` | Zera `materializada_em/por/via`; lote aceita outros campos para zerar junto. `app/chamados/models.py:2646`–`:2652`, `:2691`–`:2700`. | Sem trilha própria. A preservação depende dos chamadores. |
| `ressuscitar_perguntas` | Descarimba e zera `validada_em/por/respondida_em`; universo sem resposta textual. `app/chamados/models.py:1434`–`:1438`, `:1504`. | Log agrega IDs e rótulos de estado, não horários/assinaturas anteriores por pergunta; best-effort (`:1517`–`:1530`). |
| `encerrar_por_fato_no_espelho` | Substitui só `via_resolucao` por `fato_materializado`. `app/chamados/models.py:2707`–`:2710`. | Sem RA ou snapshot anterior na função (`:2702`–`:2711`). |
| `encerrar_fantasma` / `reabrir_fantasma` / `reabrir_por_resposta` / `reabrir_por_competencia` | Respectivamente carimba ou desfaz materialização/via. `app/chamados/models.py:2718`–`:2721`, `:2730`–`:2732`, `:2747`–`:2749`, `:2754`–`:2756`. | Funções delegam a `carimbar/descarimbar`; nenhuma cria RA. |
| `relavrar` | Substitui via derivada e lavra cache de veredito/relógio. `app/chamados/services/relavra_via.py:121`–`:126`. | Evento conserva antes/depois da via e veredito, sem RA (`:130`–`:136`). |
| `desfazer_carimbo_sem_lastro` | Descarimba sem tocar resposta/validação; relê pergunta/disputa com `select_for_update`. `app/chamados/services/relavra_via.py:217`–`:231`. | Evento conserva materialização/via anteriores, mas não autor anterior do carimbo (`:229`–`:236`). |
| `DisputaSupervisao.remover_motivo` | **É `app/chamados/models.py:1976`**. Apaga todas perguntas daquele motivo, respondidas inclusive; substitui `motivos/contexto`. | `contexto.revalidacoes` guarda evento, instante e motivo removido, não conteúdo das perguntas (`:1955`–`:1959`). **RA também cai em cascata**, pois FK usa CASCADE (`:3179`). Chamadores: posto reativado `app/colaboradores/signals.py:77`; âncora calibrada `app/escala/signals.py:161`. |
| `Disputa.fechar` — silêncio decidido | Fecha disputa e grava `resolucao/motivo_fechamento`; portas desligamento/admin_encerra carimbam perguntas vivas como admin_decidiu. `app/chamados/models.py:1852`–`:1865`. | Eventos por pergunta guardam via anterior e motivo; evento da disputa guarda motivo/resolução e IDs vivos (`:1856`–`:1869`). Sem RA por decisão de silêncio. |
| `Disputa.reabrir_admin/reabrir_sistema` | Zera `fechada_em/por`, `resolucao`, `motivo_fechamento`; admin incrementa contador. `app/chamados/models.py:2063`–`:2075`, `:2087`–`:2091`. | Sistema anexa nota da reabertura (`:2092`–`:2094`). Resolução anterior pode ter sido copiada no fechamento, mas receiver só faz a nota se o chamado-pai reconciliar (`app/chamados/signals.py:42`–`:45`); preservação integral não garantida. |
| `limpar_resolucao_residual` | Limpa resolução de disputa aberta. `app/chamados/models.py:2170`–`:2174`. | Copia texto anterior para nota, se há chamado (`:2175`–`:2181`). |
| `aparar_motivo_respondido` | Remove motivo/contexto quando resolvido por outro caminho, mantendo perguntas. `app/chamados/models.py:1890`–`:1897`. | Sem snapshot próprio do contexto/motivo anteriores; é distinto de `remover_motivo` (`:1881`–`:1888`). |

**Expiração automática por silêncio não é porta ativa encontrada:** o comando declara removidos `SILENCIO_DO_COLAB_DIAS`, `_disputas_expiradas`, `_encerrar_expiradas`, `_parada_desde`; conversa parada vai ao admin. `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`–`:64`.

**Trilha não equivale a garantia de preservação:** `core.observ.evento` grava LogAuditoria em best-effort, falha não derruba o ato. `app/core/observ.py:94`–`:105`; os campos incluem `valor_antes`, `valor_depois`, ator, usuário e relógio `criado_em`. `app/core/models.py:295`–`:307`.

**2. RespostaAnterior**

- Modelo em `app/chamados/models.py:3165`; migração de criação `app/chamados/migrations/0050_resposta_anterior.py:16`.
- Campos: `pergunta` FK CASCADE; `resposta_anterior`; `respondida_original_em`; `validada_original_em/por`; `versao_reaberta_em/por`; `motivo_reabertura` até 200. Ordena por reabertura descendente e PK descendente. `app/chamados/models.py:3179`–`:3191`.
- Único escritor de produção encontrado: `reabrir_para_correcao`, com `bulk_create`, antes do update e no mesmo atomic; só gera versão se há texto ou carimbo de resposta. `app/chamados/services/disputa_emissao.py:1401`–`:1416`.
- Leitores diretos encontrados: helper `respostas_anteriores` em `app/chamados/services/disputa_emissao.py:1723`–`:1727`; prefetch do fio em `app/chamados/services/fio.py:216`–`:228`; apresentação em `app/templates/chamados/partials/modal_fio.html:204`–`:208`.
- **Os seis chamadores não passam autor/motivo da reabertura**, embora a função aceite esses parâmetros: `app/chamados/models.py:2146`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/services/validacao.py:547`; `app/chamados/services/materializacao.py:658`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. A versão recebe, portanto, defaults `por=None`, `motivo=''`. `app/chamados/services/disputa_emissao.py:1362`, `:1409`.
- Não há proteção de imutabilidade declarada no modelo/migração; a classe define campos e ordenação, sem `save/delete` próprios ou constraint de append-only. `app/chamados/models.py:3165`–`:3191`; `app/chamados/migrations/0050_resposta_anterior.py:15`–`:32`.
- Para representar a sequência pedida, faltam tipos de decisão/fato e gravação a cada resposta, validação, recusa, silêncio/reabertura/retratação; autor e relógio de cada decisão; vínculo com resposta/decisão aplicável e instância do marco; absorção e seus carimbos; preservação contra cascata. A lacuna é deduzida comparando o esquema restrito de RA (`models.py:3179`–`:3191`) com os escritores acima (`disputa_emissao.py:1282`, `validacao.py:131`, `:545`, `models.py:1852`, `:1976`).
- O “histórico” atual do colaborador reconstitui eventos pelos **carimbos atuais** da pergunta e pelo texto atual truncado, sem ler RA: `app/chamados/services/historico.py:96`–`:114`. Assim, não representa sozinho uma sequência imutável de decisões.

## 06/11 22:42 · sessao d5c086f59f77 · V2-BUGS-I2 · verificador · origem=Code

**Não aprovaria o encerramento da volta 2.** Os reparos principais existem, mas a mesma classe de erro sobrevive na dobra por data civil do corpo 12x36 e no FT comercial; há também leitor que conserva o contrato “toda dobra no último período”. (`app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3290`, `app/ponto/calculador/periodo.py:841`)

**1. Destino dos achados (a)–(g)**

| Achado | Verificação da volta 2 |
|---|---|
| **(a) F11d protegia 240 incorretos** | **Curado nesse teste:** agora exige 360 e HE50 noturna zero. F11e deixou de ser necessário nesse caso. Sobrevive outro selo protegendo erro da mesma classe: F10d exige 90 minutos pela data civil; F10e cobra zero sob `expectedFailure`. (`app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1578`) |
| **(b) Só o último par levava o noturno** | **Curado na escrita da dobra dos períodos regulares**, no partido e comercial: cada período recebe seu trabalhado e seu split. O pressuposto antigo permanece na releitura do calculador. (`app/ponto/motor_calculo_v2.py:3122`, `app/ponto/motor_calculo_v2.py:3378`, `app/ponto/calculador/periodo.py:758`) |
| **(c) FT partido não tinha dobra/split** | **Curado na origem desse caminho:** o mensal escreve nos próprios FT, pelo mapa da jornada, antes de o fechamento ler. Isso não cobre o FT comercial ainda decidido pela data civil. (`app/ponto/motor_calculo_v2.py:2970`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:3290`) |
| **(d) Split usava pontas cruas** | **Curado na casca:** usa o par efetivo. Os três construtores fechados carimbam esse par; não identifiquei caminho fechado normal que passe a zero por ausência dele. (`app/ponto/motor_calculo_v2.py:1021`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2901`, `app/ponto/motor_calculo_v2.py:3260`) |
| **(e) Prorrogação contava minutos duas vezes** | **Curada a sobreposição na função de origem.** A medição de frota exigida antes dessa alteração não está demonstrada pelo material fornecido: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:115`; exigência de medir antes em `app/docs/PROMPTS.md:1922`) |
| **(f) Promoção usava dia civil** | **Curado o sítio da promoção**, que agora consulta `_dia_de` com o mapa. **Parcial na classe:** o filtro anterior de escala certa e as dobras por período continuam civis. (`app/ponto/services/fechamento.py:953`, `app/ponto/services/fechamento.py:935`, `app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3290`) |
| **(g) Divergência fora de `falhas`** | **Produto permanece igual:** a reconciliação continua publicada fora de `falhas`. Inclusão efetiva na sonda s1: **[não verificado]**. (`app/folha/porta_export.py:687`, `app/folha/porta_export.py:696`) |

**2. Escritores, leitores e efeito da redistribuição**

Não existe nesse resultado um único campo `minutos_noturnos`: existem relógio e legais, com unidades distintas. A redução multiplica relógio pelo fator; o split HE noturna permanece em minutos de relógio. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:991`, `app/ponto/motor_calculo_v2.py:446`)

| Campo por período | Quem escreve | Quem lê |
|---|---|---|
| `minutos_extra` | Dobras por período; FT partido; dobras diárias repartidas; excedentes ainda no último; resets e bloqueio. (`app/ponto/motor_calculo_v2.py:2819`, `app/ponto/motor_calculo_v2.py:2974`, `app/ponto/motor_calculo_v2.py:3122`, `app/ponto/motor_calculo_v2.py:3175`, `app/ponto/motor_calculo_v2.py:3378`, `app/ponto/motor_calculo_v2.py:1482`) | Resumo do espelho, assinatura A6 e invariante da exportação. (`app/ponto/services/espelho.py:112`, `app/ponto/services/esmeril_espelho.py:176`, `app/folha/porta_export.py:542`) |
| `minutos_extra_100` | Dobras, faixa acima de 120 minutos, resets/bloqueio; releitura também escreve. (`app/ponto/motor_calculo_v2.py:2820`, `app/ponto/motor_calculo_v2.py:2863`, `app/ponto/motor_calculo_v2.py:3356`, `app/ponto/calculador/periodo.py:843`) | ResultadoMes, DiaPago, alimentação do calculador, banco e DSR. (`app/ponto/motor_calculo_v2.py:334`, `app/ponto/services/dia_pago.py:182`, `app/ponto/calculador/alimentacao.py:103`, `app/ponto/motor_calculo_v2.py:3466`, `app/ponto/motor_calculo_v2.py:3509`) |
| `minutos_extra_100_feriado` | Ramos da dobra, resets/bloqueio e releitura. (`app/ponto/motor_calculo_v2.py:2821`, `app/ponto/motor_calculo_v2.py:2976`, `app/ponto/motor_calculo_v2.py:3124`, `app/ponto/motor_calculo_v2.py:3380`, `app/ponto/calculador/periodo.py:844`) | ResultadoMes, DiaPago e exclusão da dobra na base do DSR. (`app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/dia_pago.py:183`, `app/ponto/motor_calculo_v2.py:3509`) |
| `minutos_extra_100_noturna` | `_aplicar_he_noturna`, resets e bloqueio. A casca só atribui resultado positivo. (`app/ponto/motor_calculo_v2.py:1028`, `app/ponto/motor_calculo_v2.py:2947`, `app/ponto/motor_calculo_v2.py:3359`, `app/ponto/motor_calculo_v2.py:1484`) | ResultadoMes, DiaPago normal/promovido e promoção mensal. (`app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:252`, `app/ponto/services/fechamento.py:971`) |
| Noturnos relógio/legais | `_escrever_noturnas`; bloqueio pode zerar; releitura também atribui os dois. (`app/ponto/motor_calculo_v2.py:1009`, `app/ponto/motor_calculo_v2.py:1482`, `app/ponto/calculador/periodo.py:755`) | ResultadoMes inclui regulares e FT; DiaPago e resumo leem os campos correspondentes. (`app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/espelho.py:99`) |

O FM recebe os totais regulares e depois acrescenta FT promovida. O DiaPago recebe as mesmas partições, soma por jornada e lê o subconjunto noturno do FT. A persistência continua no serviço de fechamento e em `lavrar`; a restauração pode modificar somente FM. (`app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:960`, `app/ponto/services/fechamento.py:1210`, `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1399`)

**Leitores cujo comportamento muda:**

- **Calculador/oráculo:** antes recebia somente o par portador da HE; agora recebe todos os pares com HE positiva. No NPART, passa de uma ponta com 420 para duas com 180/240, permitindo somar 120+240 noturnos. (`app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:835`)
- **A6 do esmeril:** transforma uma ocorrência de 420 em ocorrências de 180 e 240, agrupadas por quantidade e data civil. **Há erro próprio:** exige três elementos da lista, sem deduplicar dias; três pares iguais podem representar “três dias”. A6 não decide escala certa, que consulta somente A2/A8. (`app/ponto/services/esmeril_espelho.py:175`, `app/ponto/services/esmeril_espelho.py:179`, `app/escala/services/escala_certa.py:16`)
- **Invariante da `porta_export`:** agrupa pela data crua de entrada. No exemplo de dois pares atravessando meia-noite, a distribuição civil muda de 0/420 para 180/240; pode mudar a acusação quando houver intra positiva. **Esse leitor continua usando outro dia.** (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:542`, `app/folha/porta_export.py:546`, `app/folha/porta_export.py:561`)
- **Banco comercial:** extras continuam agrupadas pela data civil da semana; a semana portadora pode mudar. Efeito monetário concreto na frota: **[não verificado]**. O DSR subtrai HE100 feriado por período, conservando zero para a dobra repartida. (`app/ponto/motor_calculo_v2.py:3465`, `app/ponto/motor_calculo_v2.py:3468`, `app/ponto/motor_calculo_v2.py:3509`)
- **Tela/espelho/PDF:** o resumo soma períodos; a tela diária e os valores do PDF leem rubricas. O PDF também consulta períodos para decidir presença de coluna. A redistribuição isolada conserva os totais de HE; as correções do noturno alteram suas rubricas. (`app/ponto/services/espelho.py:112`, `app/templates/ponto/espelho.html:268`, `app/relatorios/pdf_espelho.py:837`, `app/relatorios/pdf_espelho.py:844`)
- **TXT:** não emite evento próprio para o split HE100 noturna; lê HE100 normal/feriado e adicional noturno total. Portanto o reparo global de `minutos_noturnos` pode atingir o adicional exportado, mesmo fora de feriado. Igualdade de arquivos gerados: **[não verificado]**. (`app/folha/export.py:420`, `app/folha/export.py:439`, `app/folha/export.py:447`)

**Sim, existe pressuposto explícito “só o último”:** `calculador/periodo.py::reler` zera HE nos pares e recoloca toda dobra no último, mas preserva os splits noturnos. Relendo o NPART corrigido, o primeiro período pode ficar com **HE100=0 e HE100 noturna=120**. É subconjunto maior que seu pai por período. Chamador produtivo dessa releitura e dinheiro atual decorrente: **[não verificado]**. (`app/ponto/calculador/periodo.py:758`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`)

**3. Recorte longo: impacto fora de feriado e valor ainda errado**

A alteração não consulta feriado: alcança qualquer chamada com prorrogação habilitada cuja extensão do 00–05 ultrapasse 22h. Com prorrogação desabilitada, essa linha não atua. A exceção de hora reduzida controla o fator, separadamente da prorrogação. (`app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:995`)

Na extração em memória das funções, reconstruindo pontualmente a linha anterior pelo diff, obtive estes valores; isso não constitui execução do pai inteiro. (`app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:115`)

| Período, prorrogação ligada | Antes → agora, minutos de relógio |
|---|---:|
| 04–23 | 1.200 → 1.140 |
| 04–01 seguinte | 1.440 → 1.260 |
| 00–00 seguinte, 24h | 1.560 → 1.440 |
| 04–04 seguinte, 24h | 1.800 → 1.440 |
| 08–08 seguinte, 24h | 600 → 600 |
| 22–08 seguinte | 600 → 600 |

Essas diferenças seguem da sobreposição removida; **cruzar meia-noite, sozinho, não implica mudança**, nem um período habitual de 12h exige alcançar o trecho corrigido. Quantos períodos reais fora de feriado mudam: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:98`, `app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:120`)

Não encontrei contraexemplo ao teto `min(fim, s2_ini)` em si nos recortes examinados. **Existe valor novo ainda errado no split:** jornada contínua 22–07, prorrogação ligada, HE100 final de 60 minutos, das 06–07. O noturno da jornada é 540, mas `he_noturna` devolve zero: recalcula a janela começando às 06h e perde o fato de que esse trecho prorroga a noite anterior. Pela regra de prorrogação declarada, esses 60 deveriam ser HE100 noturna. É defeito remanescente, não regressão criada pelo teto. (`app/ponto/motor_calculo_v2.py:85`, `app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:446`)

**4. Par efetivo ausente**

Nos três construtores fechados, inclusive quando o período depois é classificado FT, o carimbo precede o split. Não encontrei comercial, partido ou 12x36 fechado normal perdendo HE noturna pela guarda nova. (`app/ponto/motor_calculo_v2.py:2467`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2901`, `app/ponto/motor_calculo_v2.py:3260`)

O par falta nos retornos de aberto. Há aberto creditado que copia somente trabalhado/fora da janela de um período auxiliar, descartando efetivos e noturnas; uma dobra posterior pode carregar HE100 com split zero. A guarda anterior também retornava sem saída, portanto isso não demonstra regressão desta troca. A quantidade noturna devida nesse aberto não está demonstrada: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2761`, `app/ponto/motor_calculo_v2.py:2888`, `app/ponto/motor_calculo_v2.py:3239`, `app/ponto/motor_calculo_v2.py:2562`, `app/ponto/motor_calculo_v2.py:3126`)

**5. Datas civis restantes: dinheiro e escopo**

- **`_ft_certa → escala_certa`:** continua passando data civil antes da promoção corrigida. Pode mudar dinheiro se vínculo/LIMBO ou competência/assinaturas diferirem entre cabeça e cauda: o excluído vai para trabalhadas sem 100% e nem chega à promoção. É outra pergunta — confiabilidade da escala — com a mesma inconsistência temporal. Ocorrência real: **[não verificado]**. (`app/ponto/services/fechamento.py:935`, `app/escala/services/escala_certa.py:31`, `app/escala/services/escala_certa.py:38`, `app/ponto/services/fechamento.py:952`, `app/ponto/services/fechamento.py:991`)
- **Comercial por período:** usa `e.date()`. Regulares são corrigidos depois; **FT fica fora desse reset**. Condicional concreto: FT 03–07 de d+1, pertencente à jornada feriada d, prorrogação ligada e escala certa. O corpo civil não escreve HE para suas quatro horas; o fechamento agora promove quatro horas pela jornada e lê split zero. Pela regra, deveria ler 240 minutos noturnos. Execução integrada desse fixture no pareador comercial: **[não verificado]**. **É a mesma pergunta do item 2: dobra e seu subconjunto.** (`app/ponto/motor_calculo_v2.py:3290`, `app/ponto/motor_calculo_v2.py:3308`, `app/ponto/motor_calculo_v2.py:3351`, `app/ponto/services/fechamento.py:953`, `app/ponto/services/fechamento.py:969`)
- **12x36/plantão:** conserva dobra por `e.date()` sem correção mensal equivalente. F10d já codifica a cauda de 90 minutos dobrada indevidamente; F10e declara que deveria ser zero. **Também é a mesma classe da dobra pelo dia errado.** (`app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3555`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1582`)

**6. Mordida dos testes** [nao verificado]

| Testes | O que efetivamente protegem | [nao verificado]
|---|---|
| F11d/f/g/h | Valores explícitos da regra, incluindo FM e DiaPago; `confere` usa igualdade. Não aceitam qualquer resultado ou exceção. Execução no pai inteiro: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1286`, `app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1780`, `app/ponto/tests/test_o280_bateria_da_lei.py:1808`, `app/ponto/tests/test_o280_bateria_da_lei.py:1825`) |
| B-SPLIT-EFETIVO e s5b | Cobram zero pelo par efetivo; s5b também exige resultado positivo com o cru. Na reprodução em memória, a casca atual passou e a leitura crua reconstruída falhou por asserção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1860`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:191`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:196`) |
| N-LONGO-a/b | Cobram 1.140/1.260. A linha anterior reconstruída produz 1.200/1.440; mordem numericamente a sobreposição. (`app/ponto/tests/test_o280_bateria_da_lei.py:1882`, `app/ponto/tests/test_o280_bateria_da_lei.py:1902`, `app/ponto/motor_calculo_v2.py:115`) |
| P5 VOLTA | Compara contra regra independente minuto a minuto e propriedades. Os três testes passam com o teto; reconstruindo a extensão antiga, falham por asserções. **Protegem a regra local, não apenas o código novo.** Essa regra declara expressamente o gatilho 00–05 e o teto de 26h como escolhas da casa. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:192`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:372`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:381`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:390`) |

**Limite importante:** F11f/g/h forçam `escala_certa_no_dia=True`; assim não exercitam o erro civil do filtro anterior. Os testes novos são estritos, mas não certificam essa fronteira. (`app/ponto/tests/test_o280_bateria_da_lei.py:1749`)

**7. Classes estruturais que permanecem**

- **Segundo juiz de pertença:** a base lê o mapa da autoridade e depois pode sobrescrevê-lo por sua própria regra de intervalo de até seis horas. O partido volta a publicar o mapa direto do juiz; classificação e mapa final podem, portanto, ter procedências diferentes. Caso real divergente: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2412`, `app/ponto/motor_calculo_v2.py:2417`, `app/ponto/motor_calculo_v2.py:2951`, `app/ponto/motor_calculo_v2.py:2981`)
- **Campo com dois significados:** HE como quantidade própria do período no v2 corrigido; HE como total diário depositado no último na releitura. A preservação dos splits torna a inconsistência numérica, não apenas terminológica. (`app/ponto/motor_calculo_v2.py:3123`, `app/ponto/calculador/periodo.py:758`, `app/ponto/calculador/periodo.py:843`)
- **Selo protegendo defeito:** F11d foi corrigido; F10d/F10e conservam o padrão errado/esperadamente falho. (`app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1578`)
- **Portas e ordem:** as adições permanecem nas portas existentes. Continua possível persistir FM e capturar falha posterior na lavratura do DiaPago, deixando divergência; ocorrência real: **[não verificado]**. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1151`, `app/ponto/services/fechamento.py:1206`, `app/ponto/services/fechamento.py:1245`)
- **Relógio:** as linhas alteradas usam instantes recebidos, sem nova leitura de “agora”. Não há evidência no diff de nova inversão de trava; execução concorrente: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:1024`, `app/ponto/services/fechamento.py:953`)

A validação executada ficou em leitura e reproduções em memória das funções/testes puros citados. Suíte Django, pai completo, impacto de frota, banco, logs e TXT gerado: **[não verificado]**. [nao verificado]

## 06/11 22:42 · sessao 5499de46e925 · - · - · origem=Ronald (tmux)

A bateria FIO-E0 C-E não fecha nesta cópia: buscas por `FIO-E0`, `C-A`…`C-E` e `V4-1`…`V4-5` nos arquivos `.md/.txt/.py` fora de logs não localizaram os quadros. Localização e conteúdo desses documentos: [nao verificado].

O topo do RELATO contém fatos até aproximadamente 20:47; os avais colados pelo usuário são posteriores, aproximadamente 22:1x/22:4x. Portanto, a ausência pode refletir a defasagem da cópia, sem prova de inexistência na árvore posterior (`app/docs/RELATO.md:3-10`; horário dos avais: [nao verificado] em arquivo, fornecido no chat).

As referências do anexo não correspondem ao BACKLOG desta cópia:

- `app/docs/BACKLOG.md:1797`: BUG-PLANTIO-NOTURNO-MAIS-1.
- `app/docs/BACKLOG.md:1813`: BO-[nome]-0611.
- `app/docs/BACKLOG.md:1821`: O10-TROCA-DE-PLANTAO-ENTRA.

Para o item (5), o máximo sustentado é:

| Casos | O que está provado |
|---|---|
| 1, 2, 6, 7, 10 | Faltam segundo o aval colado; descrição individual [nao verificado]. |
| 3, 4, 5, 8, 9 | Existência na C-E segundo o aval colado; localização, conteúdo e implementação executável [nao verificado]. |

Há outra auditoria Fernando de dez itens, mas não há ponte que permita identificá-la com a bateria FIO-E0: o BACKLOG diz “registrado, nada construído”, item 7 sem tabela de tradução e item 10 sem último clique (`app/docs/BACKLOG.md:1733`). Não usar essa linha para preencher os casos FIO-E0.

Testes executáveis relacionados encontrados, sem correspondência numérica C-E comprovada: [nao verificado]

- Marco vencido hoje, ainda não vencido, dois instantes com resultados diferentes, dia futuro e competência: `app/chamados/tests/test_pergunta_do_dia_em_curso.py:70-105`, `:125-135`.
- Dia misto: quatro marcos faltantes; entrada 08:00 validada sem registro, demais 12:00/13:00/17:00 aceitos; testa projeção, fio, API e supervisor: `app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:4-22`, `:132`, `:184`, `:214`, `:235`, `:250`.
- Veredito provisório até o teto não mata pergunta; fato encerrado sem acusação mata; equivalência `.viva` e consulta: `app/chamados/tests/test_o261_pergunta_viva_teto.py:52-80`.
- Resposta concordante com batida/luz até cinco minutos e dia coerente morre; seis minutos, dia acusando ou luz ausente permanecem vivos: `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:7-11`, `:56-66`.
- Cobrança diária renasce por novo marco faltante, sem criar outro chamado: `app/chamados/tests/test_reabrir_pergunta_o_marco.py:12-14`, `:57-73`.
- Precedência de validada, resposta, decisão admin, identidade, absolvição/acusação da célula e repetibilidade: `app/chamados/tests/test_pergunta_viva.py:17-77`.

Esses testes foram lidos; resultado de execução [nao verificado]. [nao verificado]

## 06/11 22:40 · sessao 439c496efa85 · - · - · origem=Ronald (tmux)

- **(a) curado:** F11d exige `he100_not=360`, `not_rel=360`, HE100=420 e HE50 noturna=0; F11e com `expectedFailure` deixou de existir nesse trecho. A comparação é `assertEqual` dos campos esperados, não captura de exceção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1716`, `:1726`, `:1286`)

- **F11f/g/h cobram valores, sem aceitar qualquer resultado:** F11f exige FM/DP noturna=2h; F11g exige folga=7h e feriado=0; F11h exige FM/DP feriado=7h e noturna=6h. O helper usa `getattr(dp,...,0)`, mas DP ausente ainda reprova porque cada caso exige ao menos um valor positivo do DP. (`app/ponto/tests/test_o280_bateria_da_lei.py:1757`, `:1780`, `:1808`, `:1825`)

- **Limite desses testes:** todos os três forçam `escala_certa_no_dia=True`; portanto não provam o caminho real de decisão da escala certa, inclusive seu uso da data civil. (`app/ponto/tests/test_o280_bateria_da_lei.py:1748`)

- **B-SPLIT-EFETIVO** exige trabalhado=780, extra=300, fora=60, noturno=0 e ambos splits=0. A casca s5b exige `(0,0)` para par efetivo e positivo para o cru; não é teste vazio. Contudo o falso fornece automaticamente o par efetivo, então não cobre sua ausência em períodos reais. (`app/ponto/tests/test_o280_bateria_da_lei.py:1856`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:48`, `:191`, `:193`, `:196`)

- **N-LONGO-a/b mordem a soma duplicada:** resultados exigidos 1.140 e 1.260; também verificam o split HE50. O caso b não verifica explicitamente `_n100`, mas a exige HE100=0 na chamada. (`app/ponto/tests/test_o280_bateria_da_lei.py:1871`, `:1873`, `:1888`, `:1890`)

- **P5 VOLTA protege a regra local:** `_regra` conta união de minutos sem chamar a função de cálculo; `_celula` compara desconto à regra e às propriedades parte≤todo/peça≤duração. Os VOLTA continuam exigindo lista de falhas vazia; não aceitam resultado arbitrário. Seu oráculo incorpora duas convenções declaradas da casa: qualquer toque em 00–05 aciona prorrogação e duração acima de 26h resulta zero. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`, `:155`, `:168`, `:177`, `:192`, `:205`, `:372`, `:381`, `:390`)

- **Prova executada somente em memória:** funções extraídas por AST, sem Django/DB; com o código atual, os três VOLTA passaram. Repondo somente `s1_fim_efetivo=fim`, conforme o diff fornecido, surgiram 1/2/1 falhas, todas de asserção; N-LONGO voltou a 1.200/1.440. A casca s5b passou atual e falhou ao restaurar a leitura crua. Isso é mutação pontual reconstruída, não execução do commit pai completo nem da suíte Django. (`app/ponto/motor_calculo_v2.py:115`, `:1023`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:372`, `:381`, `:390`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:185`) Execução integral no pai: **[não verificado]**.

- **(g) não demonstrado pelo material:** a divergência FM×DP continua publicada fora de `falhas`; os arquivos da sonda citada na triagem não foram fornecidos na cópia localizada. Logo, inclusão/correção na sonda e resultado da frota: **[não verificado]**. (`app/folha/porta_export.py:687`, `:696`, `:697`)

- **Fragilidade preexistente, fora dos novos VOLTA:** `_sob_mutante` aceita qualquer `AssertionError` da chamada, sem conferir mensagem ou valor causador; os novos VOLTA não usam esse mecanismo. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:345`, `:348`, `:373`, `:387`, `:393`)

## 06/11 22:40 · sessao f70c1404c640 · - · - · origem=Ronald (tmux)

Checklist 2 — mapa e efeitos verificados por leitura.

| Campo por período | Escritores no v2 | Leitores relevantes |
|---|---|---|
| `minutos_extra` | Dobra por período 12x36/comercial; FT partido; dobra por dia agora repartida; excedente diário continua no último; resets e bloqueio. (`app/ponto/motor_calculo_v2.py:2819`, `:2974`, `:3122`, `:3175`, `:3208`, `:3291`, `:3354`, `:3378`, `:3437`, `:1482`) | `horas_extra`; resumo espelho; A6 do esmeril; invariante da porta_export. (`app/ponto/motor_calculo_v2.py:270`; `app/ponto/services/espelho.py:112`; `app/ponto/services/esmeril_espelho.py:176`; `app/folha/porta_export.py:542`) |
| `minutos_extra_100` | Dobras e faixa acima de 120; FT partido; repartição por período; resets/bloqueio. (`app/ponto/motor_calculo_v2.py:2820`, `:2863`, `:2944`, `:2975`, `:3123`, `:3292`, `:3315`, `:3356`, `:3379`, `:3442`, `:1483`) | ResultadoMes; DiaPago; resumo; calculador alimenta split; banco/DSR. (`app/ponto/motor_calculo_v2.py:334`, `:3466`, `:3509`; `app/ponto/services/dia_pago.py:182`; `app/ponto/services/espelho.py:114`; `app/ponto/calculador/alimentacao.py:103`) |
| `minutos_extra_100_feriado` | Dobras por período, FT partido e por dia; resets/bloqueio. (`app/ponto/motor_calculo_v2.py:2821`, `:2945`, `:2976`, `:3124`, `:3293`, `:3357`, `:3380`, `:1483`) | ResultadoMes/DiaPago; exclusão do feriado na base semanal do DSR. (`app/ponto/motor_calculo_v2.py:337`, `:2646`, `:3509`; `app/ponto/services/dia_pago.py:183`) |
| `minutos_extra_100_noturna` | `_aplicar_he_noturna`; resets/bloqueio. A casca só atribui quando o resultado é positivo. (`app/ponto/motor_calculo_v2.py:1028`, `:2947`, `:3359`, `:1484`) | ResultadoMes, DiaPago normal e FT feriada, promoção FM. (`app/ponto/motor_calculo_v2.py:349`; `app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:971`) |
| Noturnos | Campos efetivos são `minutos_noturnos_relogio` e `_legais`, escritos por `_escrever_noturnas`; a redução multiplica o relógio pelo fator. (`app/ponto/motor_calculo_v2.py:219`, `:220`, `:991`, `:1009`) | ResultadoMes soma legais de regulares + FT; DiaPago normal/FT; resumo soma legais e relógio separadamente. (`app/ponto/motor_calculo_v2.py:326`; `app/ponto/services/dia_pago.py:179`, `:224`; `app/ponto/services/espelho.py:99`, `:103`) |

FM recebe os totais de ResultadoMes em `fechamento.py:918-924`; FT feriada acrescenta trabalhado às HE100 e feriado, e lê o subconjunto noturno do período em `:960-971`. DiaPago normal soma cada período pelo mapa da jornada; FT promovida usa trabalhado para HE100/feriado e lê o subconjunto. (`app/ponto/services/dia_pago.py:173`, `:182`, `:244`, `:249`, `:252`)

A porta persistente do DiaPago continua `lavrar`, com `bulk_create` após apagar a versão; a versão oráculo pode substituir os valores dentro dessa mesma porta. A restauração de FM aceita os campos pedidos e pode alterar somente FM. (`app/ponto/services/dia_pago.py:329`, `:345`, `:359`; `app/ponto/services/fechamento.py:1381`, `:1399`)

**Efeitos concretos da repartição**

- **Calculador produtivo/oráculo acompanha a nova repartição:** só inclui em `he_pontas` períodos com HE positiva e depois chama `he_noturna` para cada par. NPART passa de uma ponta com 420 para duas com 180/240, permitindo o split 120+240=360. A porta do fechamento alimenta esse caminho e grava versão `oraculo`. (`app/ponto/calculador/alimentacao.py:102`, `:106`; `app/ponto/calculador/regras.py:827`, `:832`, `:835`; `app/ponto/services/fechamento.py:1276`, `:1278`, `:1301`)
- **A6 muda sua contagem:** agrupa `minutos_extra` por período e guarda a data civil, sem deduplicar datas; usa `len(lista)>=3`. NPART muda uma ocorrência de 420 para ocorrências de 180 e 240, podendo acender/apagar a assinatura; três pares iguais no mesmo dia também podem satisfazer “3+ dias”. Essa assinatura não rege dinheiro: escala certa só pergunta A2/A8 e retorna antes de A6. (`app/ponto/services/esmeril_espelho.py:20`, `:175`, `:177`, `:179`, `:101`; `app/escala/services/escala_certa.py:16`, `:42`)
- **A testemunha da porta_export muda a distribuição entre dias:** agrupa pela data civil/UTC de `entrada`, sem mapa da jornada. NPART move HE de 0/420 para 180/240 entre as datas civis; o invariante pode passar a acusar ou silenciar outra linha quando há intra positiva. (`app/folha/porta_export.py:535`, `:538`, `:542`, `:545`, `:561`)
- **Resumo de espelho soma todos os períodos:** preserva total de HE100 com a repartição; FT é excluída da HE desse resumo. Tela diária lê `dia.rubricas`; PDF lê rubricas para valores e consulta períodos somente para decidir presença de coluna. (`app/ponto/services/espelho.py:82`, `:105`, `:112`, `:114`; `app/templates/ponto/espelho.html:268`, `:279`; `app/relatorios/pdf_espelho.py:837`, `:844`, `:857`)
- **TXT não lê o split noturno para emitir evento próprio:** lê HE100 normal/feriado e noturno total; redistribuir HE por período mantendo totais não altera esses eventos diretamente. Igualdade de TXT produzido: [não verificado]. (`app/folha/export.py:420`, `:424`, `:439`, `:447`)
- **DSR exclui a dobra por período:** como HE100 e feriado recebem o mesmo valor em cada período, sua diferença permanece zero. Banco comercial agrupa extras por data civil da semana, podendo mudar a semana portadora; efeito monetário concreto em dados reais: [não verificado]. (`app/ponto/motor_calculo_v2.py:3465`, `:3468`, `:3474`, `:3509`)

**Pressuposto “só o último” ainda existe e conflita com a nova escrita**

`calculador/periodo.py::reler` declara e implementa que HE é rubrica do dia: zera HE em todos e recoloca toda dobra somente no último. Porém não zera nem recalcula `minutos_extra_100_noturna`. Assim, ao reler NPART já repartido pelo v2, o primeiro par pode ficar com **HE100=0 e HE100 noturna=120**; o segundo fica com HE100=420/noturna=240. O total mascara o subconjunto maior que seu pai no primeiro período. (`app/ponto/calculador/periodo.py:758`, `:761`, `:770`, `:835`, `:841`, `:844`)

A lista usada para zerar rubricas de uma cópia duplicada também omite ambos os splits noturnos. Portanto a cópia pode preservar esses subconjuntos depois de zerar HE50/HE100. (`app/ponto/calculador/periodo.py:182`, `:185`, `:739`, `:740`)

Chamador produtivo de `reler` e efeito financeiro atual desses dois defeitos: **[não verificado]**. O caminho produtivo/oráculo observado usa `insumos_do_calculador`/`rubricas_por_dia`, conforme descrito acima. (`app/ponto/services/fechamento.py:1274`, `:1276`, `:1278`)

## 06/11 22:40 · sessao 1ae8f22baf0e · - · - · origem=Ronald (tmux)

**(4) Par efetivo: não encontrei regressão para período fechado; há lacuna preexistente em aberto creditado.**

- 12x36 carimba antes do split (`motor_calculo_v2.py:2774`, `:2864`); partido carimba antes do processamento mensal, inclusive FT (`:2901`, `:2978`, `:3126`); comercial carimba antes do split (`:3260`, `:3316`). A classificação FT ocorre depois de chamar `calcular_periodo`, sem desviar desses carimbos (`:2467`, `:2477`, `:2501`).
- O par falta nos retornos de período aberto dos três construtores (`:2761`, `:2888`, `:3239`). Existe aberto com minutos trabalhados positivos: o crédito copia de `_pc` apenas trabalhado e fora da janela, descartando o par efetivo e noturnas (`:2562`, `:2568`). Eventual dobra mensal desse aberto pode carregar HE100 com HE100 noturna zero (`:3115`, `:3126`; `:3375`, `:3382`). O problema não nasceu da guarda nova: a guarda anterior também retornava sem `saida` — diff_v2 fornecido; execução comparativa [não verificado].

**(5) A cura temporal está parcial.**

1. **`_ft_certa` continua civil e pode alterar dinheiro.** O filtro anterior à promoção passa `timezone.localtime(p.entrada).date()` (`app/ponto/services/fechamento.py:934`). O juiz consulta vínculo/LIMBO e competência/assinaturas pelo dia recebido (`app/escala/services/escala_certa.py:31`, `:38`, `:42`). Se a cauda cruza vínculo ou competência com resposta diferente, perde ou ganha adicional: os excluídos nem chegam ao novo juízo feriado (`fechamento.py:952`) e entram em trabalhadas sem 100% (`:987`, `:991`). É pergunta distinta — “escala certa?” — com a mesma inconsistência temporal. Ocorrência na frota [não verificado].

2. **Comercial por período continua civil; para regulares há posterior correção, para FT não.** O corpo decide por `e.date()` (`motor_calculo_v2.py:3286`, `:3290`); o mensal zera/recalcula somente `resultado.periodos` (`:3348`, `:3351`, `:3354`). Portanto a classificação inicial dos FT sobrevive. Contraexemplo lógico: FT fechado **03–07 de d+1**, pertencendo à jornada feriada d, com prorrogação habilitada e escala certa. O corpo civil não feriado não gera HE para 240 minutos (`:3304`, `:3308`, `:3316`); a promoção pela jornada gera quatro horas HE100 feriado, mas lê HE100 noturna zero (`fechamento.py:953`, `:960`, `:969`), enquanto a regra exige 240 minutos noturnos. Execução desse fixture pelo pareador comercial [não verificado]. É a mesma classe do item 2: dobra e seu subconjunto noturno.

3. **12x36/plantão também conserva decisão civil com efeito monetário.** A dobra por período usa `e.date()` (`motor_calculo_v2.py:2805`, `:2818`); plantão reutiliza esse corpo (`:3555`). O próprio F10d exige os **90 minutos errados** da cauda civil feriada (`app/ponto/tests/test_o280_bateria_da_lei.py:1574`), enquanto F10e exige a regra zero sob `expectedFailure`, incluindo montagem e execução (`:1578`, `:1589`). Esse é selo preexistente que protege defeito, remanescente da mesma classe.

**(7) Autoridade paralela preexistente permanece.**

O registro entrega a pertença da batida ao juiz `turnos_do_colab` (`app/core/juizes.py:196`), mas a base primeiro lê seu mapa e depois sobrepõe `data_jornada = _prev_dj` por distância própria de até seis horas (`motor_calculo_v2.py:2412`, `:2415`, `:2417`). Não há guarda exigindo que o juiz tenha omitido a batida. O partido refaz seu mapa diretamente do juiz (`:2951`, `:2964`) e o sobrepõe no resultado (`:2980`), podendo classificação FT e mapa final terem fontes efetivas diferentes. Contradição concreta no dado de produção [não verificado].

**Portas e ordem de persistência.**

O diff mantém FM no serviço registrado (`app/core/portas.py:500`; `fechamento.py:969`) e DiaPago em `lavrar` (`app/ponto/services/dia_pago.py:244`, `:252`). Permanece a ordem FM persistido → tentativa de DiaPago → exceção capturada (`fechamento.py:1151`, `:1206`, `:1245`); logo falha nessa segunda etapa pode deixar FM e DP divergentes, sem que essa volta cure a atomicidade. Ocorrência real [não verificado].

## 06/11 22:31 · sessao f35164bac815 · FERIAS-CANCELAR-2X · verificador · origem=Code

**Não fecharia a pré-verificação como “classe totalmente curada”.** O cron e as portas de confirmação corrigem os dois caminhos apontados na v2, mas permanecem portas genéricas capazes de gravar ausência de férias, um retorno rápido que não garante o estado atual do banco e uma exigência de RED que não é atendida por cada teste contra o pai. (`app/ferias/services.py:544`, `app/ferias/services.py:560`, `app/ferias/services.py:594`, `app/ponto/services/ausencia.py:193`, `app/ferias/services.py:587`, `app/ferias/test_cancelar_2x.py:83`)

Execução Django contra `f9d6f3e2`, suite verde, identidade Git da cópia, ocorrência e impacto em produção: **[nao verificado]**.

1. **Quem escreve e quem lê o agendamento**

O censo dos escritores de produção converge para `ferias/services.py`; o admin de férias registra somente `Afastamento`. (`app/core/portas.py:489`, `app/ferias/admin.py:4`)

| Campo | Escritores persistentes |
|---|---|
| `status` | Histórico calcula pelo juiz: `app/ferias/services.py:282`; criação comum e aprovação usam o default `agendado`: `app/ferias/services.py:321`, `app/ferias/services.py:489`, `app/ferias/models.py:309`; cancelamento DP: `app/ferias/services.py:382`; cancelamento admin: `app/ferias/services.py:412`; remarcação/correção: `app/ferias/services.py:438`; cron: `app/ferias/services.py:596`. |
| `data_inicio` / `data_fim` | Histórico: `app/ferias/services.py:280`; criação comum: `app/ferias/services.py:322`; aprovação: `app/ferias/services.py:491`; alteração de existente por `_trocar_janela`: `app/ferias/services.py:433`. |
| `confirmacao` | Default: `app/ferias/models.py:314`; envio: `app/ferias/services.py:545`; recusa/confirmação: `app/ferias/services.py:562`, `app/ferias/services.py:565`. |
| `confirmado_em` | Campo anulável: `app/ferias/models.py:318`; envio limpa: `app/ferias/services.py:546`; confirmação carimba: `app/ferias/services.py:566`; recusa preserva o carimbo anterior: `app/ferias/services.py:563`. |

Os leitores encontrados, incluindo fora do diff, são:

| Leitura | Sítios |
|---|---|
| Datas, status e consultas canônicas | `ferias_na_janela`, `em_gozo_hoje`, divergência e próximos agendamentos: `app/ferias/services.py:51`, `app/ferias/services.py:63`, `app/ferias/services.py:77`, `app/ferias/services.py:101`. |
| Permissões, frações, sobreposição e identificação | `app/ferias/models.py:243`, `app/ferias/models.py:324`, `app/ferias/models.py:328`, `app/ferias/models.py:332`, `app/ferias/models.py:339`, `app/ferias/models.py:346`, `app/ferias/models.py:353`, `app/ferias/models.py:394`, `app/ferias/models.py:420`, `app/ferias/models.py:458`. |
| Filtros, KPIs, validações, auditoria e mensagens das views | `app/ferias/views.py:26`, `app/ferias/views.py:72`, `app/ferias/views.py:235`, `app/ferias/views.py:366`, `app/ferias/views.py:414`, `app/ferias/views.py:465`, `app/ferias/views.py:483`, `app/ferias/views.py:544`, `app/ferias/views.py:668`, `app/ferias/views.py:836`, `app/ferias/views.py:878`, `app/ferias/views.py:885`, `app/ferias/views.py:925`. |
| Situação, fracionamento, avisos e colisões | `app/ferias/situacao.py:117`, `app/ferias/situacao.py:164`, `app/ferias/situacao.py:246`, `app/ferias/situacao.py:270`, `app/ferias/situacao.py:343`. |
| Telas | `app/templates/ferias/saldo.html:59`, `app/templates/ferias/_tabela_ferias.html:39`, `app/templates/ferias/_secao_colab.html:117`, `app/templates/ferias/cancelar_admin_massa.html:42`, `app/templates/ferias/_meu_atendimento_ferias.html:6`. |
| API e mensageria | `app/api/views.py:2252`, `app/api/views_mensageria.py:840`, `app/api/views_mensageria.py:900`, `app/api/views_mensageria.py:939`, `app/api/views_mensageria.py:1045`, `app/api/views_mensageria.py:1093`. |
| Disponibilidade, substituições e retorno | `app/colaboradores/views.py:2843`, `app/colaboradores/views.py:2895`, `app/ponto/utils.py:197`, `app/ferias/management/commands/alertar_retornos.py:20`. |
| Gate, aviso da batida e folha | `app/ponto/utils.py:89`, `app/ponto/services/aviso_da_batida.py:74`, `app/folha/export.py:491`. |
| Sincronização e vigia | `app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/tripwire_status_ferias.py:29`. |
| Confirmação | Elegibilidade e resposta: `app/ferias/views.py:860`, `app/ferias/views.py:909`; pendências: `app/chamados/views.py:627`; badges/botão: `app/templates/ferias/_secao_colab.html:122`. |

A inteligência ainda procura `status='aprovado'`, que não pertence às choices do agendamento; esse problema preexistente continua presente. (`app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`, `app/ferias/models.py:293`)

Leitor de produção específico de `AgendamentoFerias.confirmado_em`, além de suas próprias escritas: **[nao verificado]**.

2. **Os três procedentes da v2**

| Procedente | Resultado da leitura |
|---|---|
| Cron gravar destino calculado de datas antigas | **Fechado nesse escritor:** recebe `hoje`, trava o agendamento e calcula `alvo` das datas de `vivo`; não grava mais o `novo` da foto. (`app/ferias/management/commands/sincronizar_status_ferias.py:49`, `app/ferias/services.py:593`, `app/ferias/services.py:594`) |
| Confirmação recriar espelho de cancelado | **Fechado nas duas portas nomeadas:** releem sob trava, recusam cancelado e os saves de confirmação deixam de disparar espelho. (`app/ferias/services.py:544`, `app/ferias/services.py:560`, `app/ferias/models.py:437`, `app/ferias/models.py:443`) |
| RED morrer antes do valor por `permitir_concluido` ausente | O TypeError desprotegido foi retirado: há chamada HTTP e, no teste direto, captura antes da asserção de estado. Isso **não transforma esses testes em RED contra v2**, cuja assinatura já existia no diff fornecido; execução contra o pai **[nao verificado]**. (`app/ferias/test_cancelar_2x.py:149`, `app/ferias/test_cancelar_2x.py:156`, `app/ferias/test_cancelar_2x.py:157`) |

Nas portas atuais de alteração do **agendamento existente**, a troca de datas usa a linha travada e o cron recalcula nela; não encontrei outro escritor de produção gravando um destino calculado da foto antiga. (`app/ferias/services.py:430`, `app/ferias/services.py:433`, `app/ferias/services.py:438`, `app/ferias/services.py:594`)

**A classe global “cancelado não volta a ter ausência de férias” permanece aberta.** Há cinco entradas genéricas que aceitam o tipo sem passar pelo agendamento:

- Lançamento admin, inclusive aprovado: `app/ponto/views.py:2378`, `app/ponto/views.py:2415`, `app/ponto/views.py:2420`.
- Submissão PWA: `app/ponto/views.py:2476`, `app/ponto/views.py:2509`, `app/ponto/views.py:2536`.
- API app: `app/api/views.py:1720`, `app/api/views.py:1771`, `app/api/views.py:1820`.
- Resolver dia, inclusive aprovado: `app/ponto/views.py:3341`, `app/ponto/services/veredito_celula.py:311`, `app/ponto/services/veredito_celula.py:315`.
- Edição de ausência existente, inclusive troca de outro tipo para férias: `app/ponto/views.py:2761`, `app/ponto/views.py:2796`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`.

A vedação existe no `AusenciaForm`, mas essas entradas chamam diretamente os serviços; `criar_ausencia` valida catálogo/colisão e grava sem exigir agendamento ativo. Assim, com tipo válido e janela livre, podem recolocar férias após o cancelamento. Isso não altera o saldo nem ressuscita o status do agendamento; cria divergência entre agendamento e ausência. (`app/ponto/forms.py:32`, `app/ponto/forms.py:65`, `app/ponto/services/ausencia.py:193`, `app/ponto/services/ausencia.py:377`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`)

Existe ainda um contraexemplo da **mesma foto velha**: edição carrega a ausência; cancelamento a apaga; edição executa `save()` completo na foto apagada. A ausência de releitura/trava é visível; a reinserção pelo fallback UPDATE→INSERT do Django é **inferência, execução e fonte ORM na cópia [nao verificado]**. (`app/ponto/views.py:2761`, `app/ponto/services/ausencia.py:1188`, `app/ponto/services/ausencia.py:509`, `app/ponto/models.py:908`)

3. **Mudança de `AgendamentoFerias.save` e `update_fields`**

Não encontrei dependência de produção do espelho com `dias`, `obs`, `aviso_emitido` ou confirmação isolados. Os saves existentes são status; status+obs; datas+dias+aviso+status; ou confirmação. Os três primeiros continuam espelhando, e somente confirmação passa a não espelhar. (`app/ferias/services.py:383`, `app/ferias/services.py:413`, `app/ferias/services.py:446`, `app/ferias/services.py:547`, `app/ferias/services.py:563`, `app/ferias/services.py:567`, `app/ferias/services.py:597`, `app/ferias/models.py:443`)

O comentário “depende SÓ” é incompleto: a criação do espelho também lê `agendado_por` para autoria. Não apareceu alteração isolada desse campo no código de produção aberto; ele é atribuído nas criações, que fazem save completo. (`app/ponto/services/ausencia.py:1205`, `app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:495`)

A nova seleção por `update_fields` fecha o caso de confirmação, mas não torna `save()` uma proteção universal contra foto velha: save completo e save que inclua status/datas continuam espelhando o objeto recebido. (`app/ferias/models.py:439`, `app/ferias/models.py:443`, `app/ponto/services/ausencia.py:1180`)

4. **`alinhar_status_por_data`, retorno e placar**

`novo` agora significa **destino pedido pela foto**; `alvo` significa **destino calculado das datas travadas**. É `alvo` que governa a escrita. (`app/ferias/services.py:581`, `app/ferias/services.py:594`, `app/ferias/services.py:596`)

O caminho rápido quebra a promessa documental de devolver o status do banco: se a foto diz `agendado`, o banco já está cancelado e o pedido é `agendado`, retorna a própria foto sem consultar. Não ressuscita a linha, mas devolve status velho. O command não sofre desse ramo, pois já pula `novo == ag.status` antes de chamar o serviço. (`app/ferias/services.py:585`, `app/ferias/services.py:587`, `app/ferias/management/commands/sincronizar_status_ferias.py:39`)

Após a trava, somente `ag.status` é atualizado; as datas do objeto retornado permanecem as da foto. Portanto, numa remarcação concorrente, o retorno pode conter status atual com datas antigas. (`app/ferias/services.py:594`, `app/ferias/services.py:598`)

O placar conta pedidos da foto cujo status final coincide com `novo`; **não conta necessariamente escritas realizadas**. Uma remarcação pode gerar `recusado` porque o status correto passou a ser outro. Além disso, o serviço pode escrever `alvo != novo`, e o command contabilizar apenas `recusado`, sem contar esse destino escrito. A classificação é coerente como “pedido antigo não confirmado”, mas inadequada como contagem de alterações efetivamente aplicadas. (`app/ferias/services.py:595`, `app/ferias/services.py:597`, `app/ferias/management/commands/sincronizar_status_ferias.py:50`, `app/ferias/management/commands/sincronizar_status_ferias.py:54`)

Os testes do cron descartam stdout; não verificam essa semântica do placar. (`app/ferias/test_cancelar_2x.py:185`)

5. **Views, confirmação e campos com significados divergentes**

Quando o escritor levanta `AgendamentoNaoConfirmavel`, **não há sucesso nem push posterior**: envio retorna antes de PWA/FCM; resposta retorna antes do push DP e dos retornos positivos. (`app/ferias/views.py:869`, `app/ferias/views.py:875`, `app/ferias/views.py:883`, `app/ferias/views.py:918`, `app/ferias/views.py:924`, `app/ferias/views.py:929`)

A guarda da confirmação continua parcial: a elegibilidade completa está na foto da view; sob trava, o escritor recusa somente cancelado. Duas respostas carregadas como `aguardando` podem confirmar e depois recusar, deixando `confirmacao='recusado'` com `confirmado_em` preenchido. Um envio carregado antes também pode redefinir confirmação concorrente para `aguardando`. Aqui haverá sucesso/push porque o escritor **aceitou**, não porque a view ignorou uma recusa. (`app/ferias/views.py:860`, `app/ferias/views.py:909`, `app/ferias/services.py:544`, `app/ferias/services.py:546`, `app/ferias/services.py:560`, `app/ferias/services.py:563`, `app/ferias/services.py:566`)

Cancelamento preserva a confirmação, enquanto a query de pendências não exclui cancelados. Logo, férias canceladas ainda podem aparecer no cartão “Férias para confirmar”; o POST subsequente será recusado. (`app/ferias/services.py:383`, `app/ferias/services.py:413`, `app/chamados/views.py:627`, `app/templates/ferias/_meu_atendimento_ferias.html:3`, `app/ferias/views.py:918`)

6. **RED, checklist e quem mais sofre o efeito**

Pela comparação estática com o diff do pai fornecido, há **cinco testes com mordida por valor na v3**, não 23: [nao verificado]

| Distinguem v3 do pai pelo valor | Asserção |
|---|---|
| Cron remarcado entre foto e escrita | Espera `agendado`: `app/ferias/test_cancelar_2x.py:194`. |
| Confirmar, recusar e enviar cancelado | Conferem ausência/status/confirmação antes de acessar a nova classe: `app/ferias/test_cancelar_2x.py:281`, `app/ferias/test_cancelar_2x.py:284`, `app/ferias/test_cancelar_2x.py:287`, `app/ferias/test_cancelar_2x.py:290`. |
| Save parcial da foto velha | Confere ausência zero/status cancelado: `app/ferias/test_cancelar_2x.py:299`. |

Os outros 18 são regressões ou controles que já encontram a proteção na v2: cinco DP; cinco admin; dois cron; três troca de janela; um HTTP de cancelamento; dois controles do espelho/confirmação. Não constituem RED contra esse pai. (`app/ferias/test_cancelar_2x.py:83`, `app/ferias/test_cancelar_2x.py:120`, `app/ferias/test_cancelar_2x.py:201`, `app/ferias/test_cancelar_2x.py:227`, `app/ferias/test_cancelar_2x.py:247`, `app/ferias/test_cancelar_2x.py:272`, `app/ferias/test_cancelar_2x.py:301`; execução contra o pai **[nao verificado]**)

Outros limites dos selos:

- “Sem escrita” verifica apenas saldo/status/obs, não ausência de UPDATE; as corridas são injeções sequenciais, não concorrência real de transações. (`app/ferias/test_cancelar_2x.py:59`, `app/ferias/test_cancelar_2x.py:101`, `app/ferias/test_cancelar_2x.py:178`)
- O censo AST executado classificou os saves dos dois canceladores como `entidade=None`; o gate ignora esse balde. Esses pontos não estão protegidos pelo selo contra deslocamento futuro para fora da porta. (`app/ferias/services.py:383`, `app/ferias/services.py:413`, `app/core/censo_escritas.py:489`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:33`)
- Não há novo juiz de status: o escritor reutiliza `status_por_data`; a pergunta “está de férias hoje?” continua registrada em `em_gozo_hoje`. (`app/ferias/services.py:594`, `app/ferias/models.py:273`, `app/core/juizes.py:250`)
- O relógio do cron foi passado pela borda, mas o módulo ainda lê relógio dentro de criação/remarcação e no juiz com argumento omitido; não cabe marcar a classe inteira como eliminada. (`app/ferias/management/commands/sincronizar_status_ferias.py:27`, `app/ferias/services.py:314`, `app/ferias/services.py:434`, `app/ferias/services.py:438`, `app/ferias/models.py:280`)
- Cancelamento, remarcação e confirmação seguem agendamento→período→ausência; cron segue agendamento→ausência. Ciclo global de deadlock e comportamento real das travas: **[nao verificado]**. (`app/ferias/services.py:364`, `app/ferias/services.py:367`, `app/ferias/services.py:407`, `app/ferias/services.py:441`, `app/ferias/services.py:593`)

**O gate atual e o cruzamento da folha não dependem de `status='em_gozo'`.** Ambos consultam datas e excluem cancelado. Já badges, botões, próximas férias, concluídas, disponibilidade e avisos de retorno ainda leem o status persistido. (`app/ponto/utils.py:89`, `app/folha/export.py:491`, `app/ferias/services.py:51`, `app/templates/ferias/_secao_colab.html:120`, `app/ferias/views.py:26`, `app/ferias/views.py:32`, `app/colaboradores/views.py:2843`, `app/ferias/management/commands/alertar_retornos.py:20`)

O censo adicional de `Ausencia(tipo='ferias')`, incluindo entradas genéricas, é:

| Escrita/leitura | Sítios abertos |
|---|---|
| Produção e remoção do espelho | `app/ferias/models.py:385`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1188`, `app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1266`, `app/ponto/services/ausencia.py:1281`. |
| Escritores genéricos, decisões, anexos e reabertura | `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:509`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:846`, `app/ponto/services/ausencia.py:927`, `app/ponto/services/ausencia.py:1071`, `app/ponto/services/ausencia.py:1130`, `app/ponto/services/ausencia.py:1141`, `app/ponto/services/ausencia.py:1296`. |
| Autoridades de cobertura e precedência | `app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2733`, `app/ponto/precedencia.py:273`. |
| Apuração, motor, calculador e documentos | `app/ponto/services/fechamento.py:639`, `app/ponto/services/fechamento.py:683`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/credito_parcial.py:26`, `app/ponto/motor_calculo_v2.py:1092`, `app/ponto/calculador/alimentacao.py:532`, `app/ponto/calculador/regras.py:195`, `app/relatorios/pdf_espelho.py:333`, `app/ponto/services/esmeril_espelho.py:164`, `app/ponto/management/commands/diff_calculador.py:412`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`. |
| Cartório, estado do dia, espelho e calendário | `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:1179`, `app/ponto/services/cartorio.py:1306`, `app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:588`, `app/ponto/status_dia.py:181`, `app/ponto/services/dia_do_colab.py:15`, `app/ponto/services/dia_decidido.py:550`, `app/colaboradores/services/calendario.py:485`, `app/colaboradores/services/calendario.py:533`. |
| Telas, API, relatórios e inteligência | `app/ponto/views.py:1002`, `app/ponto/views.py:1680`, `app/ponto/views.py:2076`, `app/ponto/views.py:2179`, `app/api/views.py:1066`, `app/api/views.py:1575`, `app/api/views.py:1861`, `app/api/views.py:3092`, `app/colaboradores/services/detalhe.py:104`, `app/ponto/views_tipos_ausencia.py:24`, `app/relatorios/views.py:197`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:84`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:538`. |
| Cobrança, grade e disponibilidade | `app/ponto/services/faltas_de_hoje.py:59`, `app/ponto/services/deslogados.py:163`, `app/ponto/services/abriu_nao_bateu.py:123`, `app/escala/utils.py:1209`, `app/escala/servico_jornada.py:342`, `app/escala/services/furos_diarios.py:222`, `app/escala/services/cadastro_realidade.py:589`, `app/escala/services/semana_do_posto.py:177`, `app/api/views_mensageria.py:1243`. |
| Chamados, disputas e aliases | `app/chamados/juizes.py:448`, `app/chamados/juizes.py:1475`, `app/chamados/juizes.py:2021`, `app/chamados/services/historico.py:129`, `app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:645`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/pergunta_no_app.py:90`, `app/chamados/reconciliador.py:784`, `app/chamados/services/adesao.py:68`, `app/chamados/services/silencio_ausencia.py:65`, `app/chamados/services/vigia_fio.py:308`, `app/chamados/services/feriado_reavaliar.py:33`. |
| Comandos de cobertura/reconciliação | `app/ponto/management/commands/reconciliar_grade.py:401`, `app/ponto/management/commands/processar_alertas_turno.py:97`, `app/ponto/management/commands/processar_alertas_avancados.py:116`, `app/ponto/management/commands/supra_juiz.py:216`, `app/ponto/management/commands/detectar_ausencias.py:815`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`, `app/ponto/management/commands/emitir_furo_retroativo.py:99`, `app/ponto/management/commands/backfill_b5_insumos.py:72`, `app/ponto/management/commands/vigia_b5_ausencia.py:68`, `app/ponto/management/commands

[truncado em 20000 de 21947 caracteres]

## 06/11 22:29 · sessao cb8a8a969eb0 · - · - · origem=Ronald (tmux)

Complemento do censo, com os sítios genéricos abertos. A leitura desses caminhos admite `Ausencia.tipo='ferias'` porque as autoridades gerais não a excluem (`app/ponto/turnos.py:2529`, `:2552`, `:2753`, `:2815`, `:3087`).

- **Apuração, grade e documentos:** `ausencias_do_periodo`/cobertura em fechamento (`app/ponto/services/fechamento.py:639`, `:683`, `:687`), efeitos de ausência (`app/ponto/services/efeito_ausencia.py:47`, `:53`), crédito parcial (`app/ponto/services/credito_parcial.py:26`), grade (`app/escala/utils.py:1209`), PDF (`app/relatorios/pdf_espelho.py:333`, `:406`), esmeril (`app/ponto/services/esmeril_espelho.py:164`), diff do calculador (`app/ponto/management/commands/diff_calculador.py:412`) e diff do extrato (`app/relatorios/management/commands/diff_extrato_parcial.py:67`).
- **Motor/calculador recebem a ausência por alimentação:** motor lê `dias_com_ausencia_inteira` e zera pontualidade (`app/ponto/motor_calculo_v2.py:1085`, `:1092`, `:1095`); calculador passa a ausência do dia (`app/ponto/calculador/alimentacao.py:532`) ao contexto que empresta o teto do motor (`app/ponto/calculador/regras.py:120`, `:195`). O tipo férias abona no catálogo (`app/ponto/catalogo/ausencias.py:101`).
- **Cartório, estado do dia, espelho e calendário:** cartório detecta fato novo por qualquer ausência (`app/ponto/services/cartorio.py:355`), consulta cobertura (`:1179`, `:1306`) e precedência (`:1948`); espelho lê cobrança e folha (`app/ponto/services/espelho.py:582`, `:588`); calendário idem (`app/colaboradores/services/calendario.py:485`, `:533`) e precedência (`:507`); estado do dia recebe fatos (`app/ponto/status_dia.py:181`, `:298`, `:304`); serviço do dia (`app/ponto/services/dia_do_colab.py:15`); cartões e história (`app/ponto/services/dia_decidido.py:332`, `:550`, `:587`).
- **Telas/API/documentos genéricos:** painel de fechamento (`app/ponto/views.py:1002`), painel de justificativa (`:1680`), painel da ausência/irmãs (`:2076`), lista com vigência ou PK direto (`:2179`, `:2181`), API espelho (`app/api/views.py:1575`), API de ausências (`:1861`), API de documento (`:3092`), detalhe 360 (`app/colaboradores/services/detalhe.py:104`), histórico de chamados do colaborador (`app/chamados/services/historico.py:129`), usos do catálogo (`app/ponto/views_tipos_ausencia.py:24`), absenteísmo/CSV (`app/relatorios/views.py:197`, `:205`), resumo de inteligência (`app/inteligencia/resumo.py:39`, `:44`, `:45`), painel de inteligência (`app/inteligencia/views.py:71`, `:84`), detalhe “top faltosos” (`:239`, `:242`), PDF de inteligência (`:516`, `:538`).
- **Cobrança e disponibilidade:** faltas de hoje (`app/ponto/services/faltas_de_hoje.py:59`, `:61`), deslogados (`app/ponto/services/deslogados.py:163`, `:166`), janela de abriu/não bateu via fatos (`app/ponto/services/abriu_nao_bateu.py:123`), jornada (`app/escala/servico_jornada.py:342`), ranking (`app/escala/services/furos_diarios.py:222`), cadastro versus realidade (`app/escala/services/cadastro_realidade.py:589`), semana do posto (`app/escala/services/semana_do_posto.py:177`), pendências cadastrais da API mensageria (`app/api/views_mensageria.py:1243`), adesão (`app/chamados/services/adesao.py:68`).
- **Chamados/disputas:** célula furada e via da resposta (`app/chamados/juizes.py:448`, `:1475`), data da ausência (`:2021`), cobertura para materialização (`app/chamados/services/materializacao.py:78`), devolução das perguntas absorvidas (`:645`), suspensão por ausência (`app/chamados/services/silencio_ausencia.py:65`), dono do fio (`app/chamados/services/vigia_fio.py:308`), reavaliação de feriado via fatos (`app/chamados/services/feriado_reavaliar.py:33`).
- **Comandos que consultam cobertura geral:** reconciliar grade (`app/ponto/management/commands/reconciliar_grade.py:401`), alertas de turno (`app/ponto/management/commands/processar_alertas_turno.py:97`), alertas avançados (`app/ponto/management/commands/processar_alertas_avancados.py:116`), suprajuiz (`app/ponto/management/commands/supra_juiz.py:216`), detectar ausências (`app/ponto/management/commands/detectar_ausencias.py:815`), emitir turno aberto retroativo (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`), emitir furo retroativo (`app/ponto/management/commands/emitir_furo_retroativo.py:99`), backfill de insumos (`app/ponto/management/commands/backfill_b5_insumos.py:72`), vigia B5 (`app/ponto/management/commands/vigia_b5_ausencia.py:68`), ausência sem efeito (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `:44`), rejulgar ausência em folga (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:33`, `:56`), auditar invariantes de chamados (`app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `:642`, `:659`, `:1466`).
- **Vigias/fila de decisão e eventos:** vigia de ausência/veredito (`app/ponto/services/vigia_ausencia_veredito.py:46`, `:53`), vigia da família (`app/ponto/services/vigia_ausencia.py:59`, `:108`), contadores DP (`app/ponto/services/ausencia.py:963`), escalada por documento (`:992`), universo de chamados de ausência decidida (`:715`), comando correspondente (`app/ponto/management/commands/fechar_chamados_ausencia.py:25`), reabrir chamados (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:33`), rejulgar status pelo tipo (`app/ponto/management/commands/rejulgar_status_por_tipo.py:23`, `:32`), duração incoerente (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`), bloqueante sem fim conforme catálogo (`app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:24`, `:27`), contagem/limpeza de ensaio (`app/ponto/management/commands/zerar_dados_operacionais.py:49`, `:90`), sinais de inclusão/exclusão (`app/ponto/signals.py:124`, `:143`, `:150`).
- **Leitura por referência recebida em outra operação:** troca de plantão relê e trava as duas ausências por PK (`app/ponto/portas/celula.py:657`); nenhuma restrição por tipo nessa carga (`:658`).

Exclusões conferidas:

- Mensageria de ausências DP **exclui férias** na autoridade (`app/ponto/turnos.py:3067`, `:3068`), usada nas duas leituras da mensageria (`app/api/views_mensageria.py:701`, `:1000`).
- Rubrica 8932 **exclui férias**, embora cobertura e motor continuem recebendo o espelho (`app/folha/export.py:197`, `:198`, `:205`; `app/ponto/catalogo/ausencias.py:290`).
- Score de assiduidade/geração de alertas **excluem tipos que abonam**, portanto férias conforme catálogo (`app/inteligencia/management/commands/calcular_scores.py:12`; `app/inteligencia/management/commands/gerar_alertas.py:55`; `app/ponto/catalogo/ausencias.py:101`).
- Ausências médicas históricas/alertas filtram tipos médicos, portanto não são leitores de férias (`app/ponto/views.py:2837`; `app/relatorios/views.py:277`; `app/ponto/services/alertas_ausencia.py:49`).
- O ramo genérico do aviso de batida restringe a tipos com frase cadastrada; hoje suas frases são maternidade/INSS, e férias é tratada antes por `em_gozo_hoje` (`app/ponto/services/aviso_da_batida.py:43`, `:73`, `:79`, `:88`).
- Os contadores de falta de documento/aprovador excluem férias por autoridades/declaração, mas mantêm números específicos de férias no payload (`app/ponto/services/vigia_ausencia.py:43`, `:113`, `:120`, `:215`).

Não afirmar que uma ausência reaparecida afeta apenas a exibição: cobertura aprovada de férias pode abonar o dia e entrar no teto de pontualidade, enquanto `em_gozo_hoje` exclui o agendamento cancelado (`app/ponto/services/efeito_ausencia.py:53`, `:59`, `:65`; `app/ponto/motor_calculo_v2.py:1092`; `app/ferias/services.py:51`). Impacto concreto em produção **[não verificado]**.

## 06/11 22:29 · sessao be4f014968c2 · - · - · origem=Ronald (tmux)

Inventário complementar aberto, incluindo aliases; as consultas genéricas abaixo não excluem `ferias` por tipo.

| App | Função e ponto de leitura |
|---|---|
| chamados | `_celula_furada`: cobertura e explicação integral, `app/chamados/juizes.py:448`; `via_pelo_fato`: cobertura aprovada, `:1475`; `_dia_da_ausencia`: lookup por PK sem filtro de tipo, `:2021`. |
| chamados | `historico_do_colab`: qualquer ausência criada na janela, `app/chamados/services/historico.py:129`. |
| chamados | `_coberto_por_ausencia`: cobertura genérica, `app/chamados/services/materializacao.py:78`. Consumidores abertos: `_criar_pergunta_se_couber`, `app/chamados/services/disputa_emissao.py:763`; `retencao`, `app/chamados/services/pergunta_no_app.py:90`; `reemitir_chamado_do_dia`, alias `_aus_c2`, `app/chamados/reconciliador.py:784`, `:845`. |
| chamados | `sincronizar_adesao`: exclui somente falta da suspensão, `app/chamados/services/adesao.py:68`. |
| chamados | `reconciliar_por_ausencia`: cobertura, status aberto/aprovado, `app/chamados/services/silencio_ausencia.py:65`, `:77`, `:93`; chamado por `reconciliar_colab`, `:127`. |
| chamados | `dono_fora_do_fio`: cobertura e status aberto, `app/chamados/services/vigia_fio.py:308`. |
| chamados | Auditor `_cobre_ausencia_i21`, `app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `Command.handle`, `:642`, `:659`, `:1466`; `contraprova_piso` lê somente o ID da ausência lavrada na célula, `:430`. |
| chamados | `devolver_absorvidas_da_ausencia_rejeitada` verifica outra cobertura genérica, `app/chamados/services/materializacao.py:645`; a seleção de perguntas absorvidas restringe a tipos de escape, `:602-604`. |
| escala | `_marcar_ausencias`: cobertura para folha, campos tipo/label/PK/parcial, `app/escala/utils.py:1209`; aplica férias como ausência, `:1231`, `:1241-1244`. |
| escala | `classificar_falta`: todas as ausências da janela por alias `_aus_periodo`, `app/escala/servico_jornada.py:342`. |
| escala | `ranking_colabs`: cobertura genérica e rótulo, exclui apenas falta, `app/escala/services/furos_diarios.py:222-224`. |
| escala | `_afastado_sem_registro`: universo inicial de afastados, mas cobertura de qualquer tipo nos dias da janela, `app/escala/services/cadastro_realidade.py:573`, `:589`. |
| colaboradores | `contexto_detalhe`: dez ausências recentes, sem filtro de tipo/status, `app/colaboradores/services/detalhe.py:104-107`. |
| colaboradores | `contexto_calendario`: aliases `_cap`/`_cap_folha`, decisões do período e consumo pelo rótulo do dia, `app/colaboradores/services/calendario.py:485`, `:533`, `:534`, `:715-717`. |
| core | `_aplicar_ausencia` lê o retorno da criação para auditoria e resposta, `app/core/views_ia.py:985-994`; não é consulta genérica do acervo. Outras consultas diretas de registros `Ausencia` em core: [não verificado]. |

Exclusões confirmadas:

- `afastados_hoje` e `palavra`, em `app/colaboradores/services/situacao_exibida.py:46`, `:63`, e a disponibilidade em `app/colaboradores/views.py:2911` chamam autoridades restritas a `afastamento_inss`; portanto essas consultas não leem férias (`app/ponto/turnos.py:2702`, `:2713`).
- `core/admin.py` é infraestrutura que enumera os campos do modelo para readonly, não consulta de registros (`app/core/admin.py:46-50`, `:68-75`).
- `_d4_em_lote` carrega `CargaDaJanela` para turnos e D4, sem consumir explicitamente `Ausencia` no corpo aberto (`app/core/services/painel_op.py:327-338`).

A cobertura genérica admite férias; suas restrições por efeito são status aprovado para folha e exclusão de rejeitada para cobrança (`app/ponto/turnos.py:2514-2517`, `:2529-2533`).

## 06/11 22:28 · sessao 1509e73a8ffd · - · - · origem=Ronald (tmux)

Complemento dos leitores genéricos aberto no código:

| Sítio/função | Leitura e tratamento de férias |
|---|---|
| `api.views.api_ausencias` GET | Lista as últimas 30 Ausencias do colaborador **sem excluir tipo/status**; serializa tipo, data, status, descrição, minutos e criação. Inclui `ferias`. `app/api/views.py:1861`, `:1870`, `:1873`, `:1875`, `:1877`, `:1878` |
| `api.views.api_chamados_v2` | Busca Ausencia pelo ID do contexto e colaborador, sem filtro de tipo; lê documento/status para oferecer anexo. O ramo exige status literal `pendente`. `app/api/views.py:1059`, `:1066`, `:1071` |
| `api.views.api_espelho_v2` | Carrega cobertura `para='cobranca'`, sem exclusão local de férias; lê o status para acrescentar `ausencia_pendente`. `app/api/views.py:1574`, `:1575`, `:1584`, `:1586` |
| `api.views.api_ausencia_documento` | Busca qualquer tipo pelo ID e colaborador; só aceita estados de `ABERTAS`. Assim a Ausencia de férias aprovada pode ser lida, mas o anexo é recusado pela condição de estado. `app/api/views.py:3092`, `:3101`, `:3102`, `:3103` |
| `api_mensageria_ausencias` | Agrega tipo/status por `ausencias_do_dp`; **exclui férias** por delegação. `app/api/views_mensageria.py:736`, `:741`, `:742`, `:743`; exclusão efetiva em `app/ponto/turnos.py:3067`, `:3068` |
| `api_mensageria_pessoa` | Lista ausências pela mesma porta que exclui férias; lê datas/tipo/status. O bloco de férias usa os serviços de Agendamento separadamente. `app/api/views_mensageria.py:1030`, `:1031`, `:1032`, `:1044`, `:1045` |
| `folha.export.ausencias_na_janela` | Leitor genérico de Ausencias aprovadas que intersectam a janela; aceita qualquer tipo recebido, inclusive férias. `app/folha/export.py:71`, `:87`, `:88`, `:90` |
| `folha.export.datas_no_periodo`, `_datas_ausencia_tipo_janela`, `_datas_ausencia_tipo` | Leem tipo/data/data_fim e expandem pela autoridade `dias_da_ausencia`; não excluem férias localmente. `app/folha/export.py:63`, `:69`, `:70`, `:146`, `:160`, `:161`, `:165` |
| `folha.export.minutos_de_falta_por_dia`, `datas_falta_janela` | Filtram pelos tipos de `descontam_vigentes`, não por férias literal. `app/folha/export.py:138`, `:140`, `:179`, `:181` |
| `folha.export.tipos_abonados_8932`, `datas_abonadas` | **Excluem férias expressamente** de 8932, independentemente do efeito cadastrado. `app/folha/export.py:193`, `:198`, `:201`, `:204`, `:205`; `app/ponto/catalogo/ausencias.py:290` |
| `relatorios.views.absenteismo` | Inclui qualquer tipo aprovado, portanto férias; lê tipo/datas, agrupa ocorrências e dias pelo juiz. Entrega tela e CSV. `app/relatorios/views.py:197`, `:204`, `:205`, `:209`, `:214`, `:215`, `:218`, `:234` |
| `relatorios.views.atestados_acumulados` | Filtra pelos tipos do catálogo vivo `TIPOS_MEDICOS`; não há exclusão literal de férias. Na semente, férias têm `medico=False`; classificação de produção **[não verificado]**. `app/relatorios/views.py:277`, `:282`, `:283`; `app/ponto/catalogo/ausencias.py:101`, `:385`, `:387` |
| `relatorios.pdf_espelho._coletar_dados_espelho` | Lê efeitos da ausência por delegação para obter justificadas, suprimidas, minutos parciais e dias inteiros, abrangendo férias conforme a cobertura. `app/relatorios/pdf_espelho.py:332`, `:333`, `:334`, `:335`, `:336`, `:406` |
| `inteligencia.views.index` | Conta ausências por tipo sem excluir férias nem status; “sem ausência” também consulta todas as Ausencias. `app/inteligencia/views.py:71`, `:84`, `:127` |
| `inteligencia.views.api_kpi` | “Sem ausência” considera qualquer tipo; “Top faltosos” conta qualquer ausência no período, incluindo férias. `app/inteligencia/views.py:231`, `:232`, `:238`, `:239`, `:242`, `:243`, `:244` |
| `inteligencia.views.exportar_pdf` | O ranking “Top Faltosos” também conta qualquer Ausencia do período, sem excluir férias/status. `app/inteligencia/views.py:516`, `:538`, `:539`, `:611`, `:613`, `:614` |
| `inteligencia.resumo.resumo_colaborador` | Lista todas as ausências recentes; totais de faltas/justificadas usam as aprovadas e o efeito vivo do catálogo. `app/inteligencia/resumo.py:39`, `:42`, `:43`, `:44`, `:45` |
| `_score_assiduidade` e `gerar_alertas.Command.handle` | Excluem rejeitadas e tipos de `abonam_vigentes`; férias ficam fora **se** continuarem abonadas no catálogo vivo. A semente diz ABONA, mas cadastro de produção **[não verificado]**. `app/inteligencia/management/commands/calcular_scores.py:12`, `:14`; `app/inteligencia/management/commands/gerar_alertas.py:55`, `:58`; `app/ponto/catalogo/ausencias.py:101`, `:249`, `:250`, `:278` |

Templates abertos que apresentam essas leituras:

- Absenteísmo apresenta tipo, ocorrências e dias, incluindo o grupo férias recebido da view. `app/templates/relatorios/absenteismo.html:36`, `:41`, `:42`, `:43`.
- Inteligência apresenta a pizza de tipos; “Top faltosos” e “Sem ausência” abrem os endpoints acima. `app/templates/inteligencia/index.html:91`, `:99`, `:141`, `:381`, `:382`.
- Fio do colaborador apresenta justificadas e as cinco Ausencias recentes, sem excluir férias. `app/templates/chamados/fio_colaborador.html:59`, `:60`, `:62`.
- Perfil do colaborador apresenta `ausencias_360` com data/tipo; a origem desse contexto não foi aberta nesta complementação **[não verificado]**. `app/templates/colaboradores/detalhe.html:63`, `:72`, `:74`, `:75`.
- Espelho apresenta o tipo e status da ausência recebida no dia, sem exclusão de férias no template. `app/templates/ponto/espelho.html:191`.
- Lista de Ausencias percorre os registros recebidos e apresenta seu tipo. `app/templates/ponto/ausencias.html:142`, `:157`.

`pautas.semanal.escrever_semanal` recebe contadores prontos em `linhas`, sem consultar Ausencia nesse caminho; a decisão sobre quais contadores estão acesos pertence ao chamador. `app/pautas/semanal.py:202`, `:206`, `:207`, `:214`. Os rótulos de problemas de ausência são apresentados nesse módulo, mas não constituem leitura da tabela Ausencia. `app/pautas/semanal.py:56`, `:58`, `:62`. Outros caminhos indiretos de pautas: **[não verificado]**.

## 06/11 22:25 · sessao cb8a8a969eb0 · - · - · origem=Ronald (tmux)

**Achado de classe: a proteção nova não fecha todos os escritores de `Ausencia(tipo='ferias')`.** As portas genéricas ainda podem escrever esse tipo sem consultar o agendamento: lançamento admin (`app/ponto/views.py:2378`, `:2415`), PWA (`app/ponto/views.py:2476`, `:2509`, `:2536`), API app (`app/api/views.py:1720`, `:1771`, `:1820`), Resolver dia (`app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:311`, `:315`) e edição (`app/ponto/views.py:2761`, `:2794`, `:2796`; `app/ponto/services/ausencia.py:474`, `:498`, `:509`).

O `AusenciaForm` contém a vedação de férias (`app/ponto/forms.py:31`, `:65`), mas esses cinco caminhos não o aplicam: recebem o tipo diretamente e chamam o serviço nas linhas acima. A porta `criar_ausencia` valida catálogo e colisão, constrói a linha e salva; não exige agendamento ativo (`app/ponto/services/ausencia.py:193`, `:377`, `:385`, `:418`). Logo, após cancelar, um POST genérico pode recolocar ausência de férias na mesma janela livre, inclusive aprovada via admin/Resolver dia (`app/ponto/views.py:2420`; `app/ponto/services/veredito_celula.py:316`). Isso não ressuscita `AgendamentoFerias.status` nem devolve saldo novamente; é uma divergência entre decisão e espelho (`app/ferias/services.py:379`, `:382`; `app/ponto/services/ausencia.py:341`).

**Contraexemplo de foto velha:** edição carrega uma Ausência de férias (`app/ponto/views.py:2761`); cancelamento remove essa linha (`app/ponto/services/ausencia.py:1188`); edição passa a foto para `editar_ausencia_registro`, que faz `ausencia.save()` completo (`app/ponto/views.py:2794`; `app/ponto/services/ausencia.py:509`). O modelo não verifica o agendamento nem usa `force_update` (`app/ponto/models.py:868`, `:908`). Reinserção pelo fallback UPDATE→INSERT do Django: **inferência, execução e fonte ORM na cópia [não verificado]**.

Censo dos escritores da ausência de férias:

- Espelho: `AgendamentoFerias.save` → `_sync_ausencia` → `espelhar_ferias`; cria pela porta ou apaga quando cancelado (`app/ferias/models.py:439`, `:443`, `:384`; `app/ponto/services/ausencia.py:1180`, `:1188`, `:1202`).
- Cancelamento admin rejeita antes de o espelho apagar (`app/ferias/services.py:407`, `:413`; `app/ponto/services/ausencia.py:1281`).
- Troca de janela apaga o trecho antigo antes do novo espelho (`app/ferias/services.py:441`, `:446`).
- Exclusão do agendamento apaga a janela (`app/ferias/models.py:446`).
- Escritores genéricos: criação e edição já enumeradas; decisões de estado/carimbos (`app/ponto/services/ausencia.py:808`, `:824`, `:835`, `:897`, `:927`), anexação/reabertura (`:846`, `:1071`, `:1106`, `:1130`, `:1141`), rederivação de fim (`:1293`, `:1296`).
- Endpoints genéricos de decisão aceitam qualquer tipo: aprovar/rejeitar/cancelar em lote (`app/ponto/views.py:2282`, `:2290`, `:2316`, `:2319`, `:2345`, `:2351`); reverter rejeição (`:2583`), aprovar individual (`:2606`), editar período aprovado (`:2670`), rejeitar (`:2687`), cancelar (`:2728`).
- Comandos: `curar_ausencia_sem_fim` seleciona qualquer tipo não aberto com duração incoerente e chama rederivação (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `:46`); `rederivar_minutos_h06_comp11` chama edição, mas relê e trava a ausência antes (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`, `:169`).
- Django admin de Ausência é somente leitura (`app/ponto/admin.py:37`).

Leitores agrupados:

- Autoridades de vigência/cobertura, incluindo férias: `app/ponto/turnos.py:2536`, `:2733`, `:3071`; para folha só aprovada, para cobrança qualquer não rejeitada (`:2509`).
- Precedência lê todas as ausências não rejeitadas (`app/ponto/precedencia.py:273`); fechamento usa cobertura de folha (`app/ponto/services/fechamento.py:680`, `:683`); grade usa a mesma autoridade (`app/escala/utils.py:1195`).
- Cartões/histórico: `app/ponto/services/dia_decidido.py:529`, `:550`, `:566`, `:587`; API lista ausências de todos os tipos (`app/api/views.py:1861`); vigia conta férias explicitamente (`app/ponto/services/vigia_ausencia.py:215`).
- Eventos de inclusão/exclusão da ausência julgam os dias (`app/ponto/signals.py:143`, `:150`).

**Quem lê status alinhado:** badges e botões (`app/templates/ferias/_tabela_ferias.html:42`; `app/templates/ferias/_secao_colab.html:120`, `:124`, `:140`, `:175`; `app/templates/ferias/saldo.html:63`, `:70`), KPI/lista de próximas, aviso pendente e concluídas (`app/ferias/views.py:26`, `:32`, `:75`, `:82`), API férias (`app/api/views.py:2262`), disponibilidade “voltando” (`app/colaboradores/views.py:2843`), aviso de retorno (`app/ferias/management/commands/alertar_retornos.py:20`), fracionamento/situação (`app/ferias/situacao.py:117`, `:164`), permissões do objeto (`app/ferias/models.py:324`, `:328`, `:332`, `:339`, `:346`).

**Gate atual e folha não precisam de `status='em_gozo'`:** gate usa `em_gozo_hoje` (`app/ponto/utils.py:85`, `:89`), aviso idem (`app/ponto/services/aviso_da_batida.py:73`), chip/lente idem (`app/ferias/views.py:25`, `:74`), folha usa `ferias_na_janela` (`app/folha/export.py:484`, `:491`). A autoridade usa datas e exclui apenas cancelado (`app/ferias/services.py:51`, `:63`); está registrada em `app/core/juizes.py:250`.

As portas de cancelamento, remarcação e confirmação adquirem agendamento→período (`app/ferias/services.py:364`, `:367`), e só depois escrevem ausência (`:407`, `:441`, `:446`). Cron trava apenas agendamento (`:594`) antes do espelho; não observei inversão demonstrada entre esses caminhos. Ciclo global de deadlock **[não verificado]**.

## 06/11 22:25 · sessao be4f014968c2 · - · - · origem=Ronald (tmux)

Censo de produção, somente leitura:

| Campo | Escritores persistentes |
|---|---|
| `status` | Histórico: `app/ferias/services.py:282`, save `:298`; criação comum e aprovação usam default `agendado`: `:321`, `:489`, `:499`, `app/ferias/models.py:309`; canceladores: `services.py:382`, `:412`; remarcação/correção: `:438`, `:446`; cron: `:594`, `:597`. |
| Datas | Criação histórica `services.py:280`; comum `:322`; aprovação `:491`; alteração existente exclusivamente `_trocar_janela`, `:433`, `:446`. |
| `confirmacao` | Defaults `models.py:314`; envio `services.py:545`, `:547`; resposta `:562`, `:565`, `:567`. |
| `confirmado_em` | Default nulo `models.py:318`; envio limpa `services.py:546`; confirmação carimba `:566`; recusa não limpa `:562-563`. |

Os saves parciais encontrados são: `['status']`, `['status','obs']`, datas/dias/aviso/status e campos de confirmação; somente estes últimos deixam de espelhar, coerentemente com a mudança (`app/ferias/services.py:383`, `:413`, `:446`, `:547`, `:563`, `:567`, `:597`; `app/ferias/models.py:437-444`). Não apareceu dependência de espelhamento com `dias`, `obs`, `aviso_emitido` ou `agendado_por` isolados no censo de produção; completude contra reflexão/código externo: [não verificado]. O comentário “depende SO” omite `agendado_por`, lido para autoria das ausências novas (`app/ponto/services/ausencia.py:1205`), mas os escritores abertos só o atribuem na criação (`app/ferias/services.py:283`, `:323`, `:495`).

Leitores abertos:

- Autoridades: `ferias_na_janela` lê datas e exclui cancelado, `em_gozo_hoje` delega; `divergencia_status` e `comecam_entre` leem status/datas (`app/ferias/services.py:51`, `:63`, `:77-82`, `:101-108`).
- Modelo: permissões/properties de edição/cancelamento `app/ferias/models.py:324-346`; frações `:353-355`; sobreposição `:394-428`; período excluível `:243`.
- Views: filtros/KPIs `app/ferias/views.py:26-32`, `:72-90`; guards e auditorias `:366-382`, `:414-426`, `:465`, `:483-517`, `:544`, `:668`, `:836`; confirmação `:860`, `:908-909`; notificações `:878`, `:885`, `:925`.
- Situação: frações, vivos, avisos, batidas e colisões (`app/ferias/situacao.py:117-122`, `:164-168`, `:246-262`, `:270-280`, `:343-352`).
- Templates: `app/templates/ferias/saldo.html:59-70`; `_tabela_ferias.html:39-42`; `_secao_colab.html:117-176`; `cancelar_admin_massa.html:42-45`; `_meu_atendimento_ferias.html:6`.
- API: `app/api/views.py:2252-2268`; mensageria `app/api/views_mensageria.py:840-843`, `:900-901`, `:939-944`, `:1045-1046`, `:1093-1095`.
- Disponibilidade/retornos: `app/colaboradores/views.py:2843-2853`, `:2895-2896`; `app/ferias/management/commands/alertar_retornos.py:20-24`.
- Gate e aviso de batida consultam a autoridade pelas datas, sem depender de `status='em_gozo'` (`app/ponto/utils.py:85-89`; `app/ponto/services/aviso_da_batida.py:73-74`). Substituições leem a janela (`app/ponto/utils.py:197-200`); folha também (`app/folha/export.py:491-493`).
- Inteligência continua procurando `status='aprovado'`, ausente nas choices atuais (`app/inteligencia/views.py:94`, `:278`, `:286`; `app/ferias/models.py:293-309`).
- Pendências de confirmação: `app/chamados/views.py:627-629`; badges e botão `app/templates/ferias/_secao_colab.html:122-124`. Leitor de produção de `confirmado_em` além de sua escrita: [não verificado].

Duas ressalvas adicionais:

1. Cancelamento preserva `confirmacao='aguardando'`, e a query das pendências não exclui cancelados; o cartão pode continuar aparecendo, embora a resposta seja recusada pelo escritor (`app/ferias/services.py:382-383`, `:412-413`; `app/chamados/views.py:627-629`; `app/ferias/views.py:918-920`).
2. Duas respostas com fotos `aguardando`: confirmar e depois recusar ainda são aceitas; a segunda sobrescreve confirmação sem limpar o carimbo, porque a guarda travada só recusa cancelado (`app/ferias/services.py:560-567`). Isso não recria o espelho, porque os saves não tocam seus campos (`app/ferias/models.py:437-444`).

Ressalva do selo estrutural: o censo AST executado com `python3 -B` classificou os saves dos canceladores como `entidade=None`; o gate ignora essa classe antes de verificar a porta. Portanto ele não protege esses dois pontos contra movimento futuro para fora da porta (`app/ferias/services.py:383`, `:413`; `app/core/censo_escritas.py:489-491`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:33-35`).

_2632 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
