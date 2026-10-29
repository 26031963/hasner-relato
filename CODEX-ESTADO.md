# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 00:15** · publicado em **29/10/2026 02:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 00:15 · sessao 1ee182181f58

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 00:13 · sessao 31880e2cc879

T8 — achado estático principal, sem executar testes: [nao verificado]

| Caso/entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa depois do fim**. Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | Pré-juiz vê turno `08→17` + `S18` órfã; dois turnos desativam o papel (`_juizpar/app/escala/utils.py:1535`). DP escolhe `S18` para `hf18`, custo zero, e deixa `S17` órfã (`:230`, `:596`). | `turnos.py:1291` absorve 12/13; `:1417` fecha em17; `:1422` cria órfã18. Mesmo após reler a ata nova, `S17` está a60min de18 (>cluster15min: `escala/orfa_classe.py:28`) e a4h da luz de intervalo13 (>teto3h: `turnos.py:317`, `:1689`); o papel da ata abstém-se em `:1693`. Juiz continua `08→17` + órfã18. | **Divergência estável provada por leitura:** a mesma `S18` é saída do dia na ata e órfã na geometria. Reconciliação final não corrige: só mexe em ponta `missing`; `hf` já contém18 (`utils.py:1937`). |
| **Dois turnos no dia**. Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos desativam papel (`utils.py:1535`). DP inicial casa quatro; cluster elimina16 de `hfi13`, pois marco mais próximo é `hf18`, já ocupado pela18 (`:611`). | S10 fica a120min do intervalo12, fora da absorção90 (`turnos.py:1293`); juiz fecha `08→10` e `16→18` (`:1354`, `:1417`). | Ata chama16 de órfã; juiz chama16 de entrada do segundo turno. Não é regressão do patch provada: ramo conserva DP. Reconciliação trata só bordas do dia, não `hfi` (`utils.py:1913`). |
| **24x48, pontas08/08**. `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento move `hf08` paraD+1 (`utils.py:346`), saída−30min. | Zona5 `_prev` move só hora **menor** que `hi`; `hf==hi` ficaD08 (`motor_calculo_v2.py:2981`), referência1440min diferente. | Divergência de instante **condicional à Zona5**. Plantão normal não a executa: `MotorPlantao(MotorBase)` em`:3425`, mapa`:3511`, fábrica`:3606`. Cadastro24x48 que alcance Zona5: **nao provado**. Não reportar como bug produtivo24x48 provado. |
| **Noturno regular**. Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança marcos (`utils.py:346`); único turno fechado entra em papel (`:1535`, `:1642`); bordas e par sentam nos quatro marcos (`:525`). | Zona5 avança01/02/07 porque horas<19 (`motor_calculo_v2.py:2981`). | Sem divergência identificada nesse caso. O teste correspondente foi lido, não rodado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:344`, `:360`). |
| **Tipo gravado trocado**. Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Caso de convergência declarado no teste (`test_juiz_do_par_ata_casa_pelo_turno.py:316`). `_papeis_do_turno` usa pares do juiz, não botões, em `utils.py:421`. | `turnos.py:1317` usa papel da ata quando existe; par da ata admite tipos trocados (`:815`). | Não achei contraexemplo novo provado. Teste compara segunda e terceira lavra, não primeira e segunda (`test…py:338`). | [nao verificado]
| **Intervalo livre**. Marcos só08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, o builder não passa turno para `_match_marcos` (`utils.py:1643`); mantém dry DP e inserção de pausas livres (`:1581`, `:1586`). | Zona5 exige os quatro marcos e pula esse dia (`motor_calculo_v2.py:2972`). | Sem divergência com Zona5: ela não participa. Caminho completo de inferência `_intern/_fdt`: **nao li integralmente**. |

O primeiro caso é o contraexemplo mais forte: inclui o refazimento final dos turnos com ata em memória (`utils.py:1824`, `:1836`) e demonstra por que a reconciliação não o elimina.

## 29/10 00:13 · sessao 38fc51a73cb6

T6 — censo estático de caminhos lidos; sem execução, teste ou banco. [nao verificado]

| Arquivo:linha | Gatilho / quem chama | Premissa antes do nascimento | Consulta veredito/ata antes? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`config/crons.py:338`), `_abrir_chamado_critico` chamado em `:144` | `TurnoMaterializado.aberto` (`:88`), entrada (`:92`), sem ausência (`:97`), lembrete `pos_20` (`:143`), sem HE permitida (`:135`) | **Não**. Lê turno e fim previsto (`:100`, `:118`); nasce `turno_aberto` em `:320`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`config/crons.py:339`) | Ontem é trabalho (`:49`), recomputa turno (`:57`), `turno_aberto_ha` (`:63`), nível N5 (`:94`), sem chamado existente e sem ausência (`:114`) | **Não** ao veredito/ata. `ec.marcos_do_dia` em `:46` é geometria; não absolvição. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto (`:92`), idade mínima (`:101`), vínculo (`:104`), sem ausência (`:108`), sem irmão (`:118`) | **Indireta, ata pelo juiz de turno**: `turnos_do_colab(...celulas=cels)` em `:88`; não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`config/crons.py:340`), `handle:439`; cartório (`cartorio.py:1351`, `:1387`); retroativo (`emitir_furo_retroativo.py:142`) | Sem entrada (`detectar_ausencias.py:395`, `:423`), sem chamado (`:424`); funil com vetos cadastro/feriado (`:600`, `:604`, `:616`, `:623`), célula (`:626`), existência (`:655`) | **Sim, mas insuficiente para “concorde”**: `celula_veta_cobranca` em `:626`; lê `trabalha/veredito` em `ponto/marcos.py:155`. A lista `marcos.py:119` não contém `concorde`. |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço por signal (`chamados/signals.py:174`, `ponto/signals.py:93`), cron (`config/crons.py:583`) | Códigos `FURO_SEM_COBRANCA`/morta/limbo (`:1332`), parcial (`:1374`), horizonte (`:1312`), sem vivos (`:1320`, `:1386`) | **Sim**: ata deste julgamento em `:1141`; parcial consulta `_marcos_do_furo_parcial` em `:1380`. Delega nascimento ao funil acima. |
| `app/ponto/management/commands/reconciliar_grade.py:498` e reabertura `:519` | Comando; cron **sem `--apply`** (`config/crons.py:436`); chamada em `reconciliar_grade.py:253` | Grade com `marks`; veto régua (`:490`) e célula (`:493`) | **Sim, veto de veredito**, com a mesma lacuna `concorde` de `marcos.py:119`. Reabertura passa também pela guarda canônica. |
| `app/chamados/reconciliador.py:710` | Serviço `reemitir_chamado_do_dia`, chamado pelo reconciliador em `:984` | Motivo C1 (`:643`), ativo (`:654`), horizonte (`:674`), isenção (`:684`), ausência (`:686`), protestos cobradores (`:699`/`:701`), veto célula (`:703`) | **Sim**: protestos da célula e veto de veredito; não é só teste de ausência de batida. |
| `app/ponto/views.py:290` | View `bater_ponto` | POST E + bloqueio turno + sem regularização (`:274`), sem chamado vivo (`:280`) | **Não**: o contexto vem de `parear_turnos` cru (`triagem_batida.py:178`), idade (`:181`) e `tem_bloqueio_turno` (`:206`). |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`cartorio.py:760`) e comando detector | Ativo (`:73`), escala ativa sem fim (`:75`), assinatura cluster | **Sim, ata**: `dias_com_cluster` lê célula em `:22`, órfãs (`:26`), apagadas (`:27`), >=2 órfãs e >=1 apagada (`:28`), rajada <=15min (`:30`). |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`config/crons.py:572`) | Pares relâmpago (`:351`), juiz `par_completo` (`:352`), não retratado (`:361`), não `so_retratar` (`:365`) | **Não como gate de veredito/ata**. Premissa é par completo/relâmpago, não célula acusadora. |
| `app/ponto/services/triagem_batida.py:336` | View `ponto/views.py:341`, solicitação de anuência | Motivo textual (`:323`), nenhum chamado vivo (`:325`) | **Não**. |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida (`:663`) | Vínculo do dia (`:402`), janela (`:421`), fora da janela (`:428`/`:433`), dedup14h (`:452`) | **Não** ao veredito/ata; marcos previstos não são absolvição. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | `detectar_batida_fora_escala` (`:730`), resultado leve (`:734`) | **Não como veto de absolvição**. Nascimento de auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API justificativas | Sem chamado de turno vivo (`:2022`/`:2032`), dados de justificativa válidos (`:2011`) | **Não**; wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | View rejeição DP (`ponto/views.py:1911`) | Justificativa pendente encontrada (`:160`), rejeitada (`:166`) | **Não**; chamado interno de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View regularização externa (`colaboradores/views.py:2983`), serviço em `regularizacao_ext.py:318` | Pedido com itens cria regularização (`:236`/`:240`) | **Não**; chamado de confirmação `regularizacao_externa` (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`, wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, retorno do reuso em `:502`) | **Não no nascimento do pai**; a premissa específica é recebida do chamador. |
| `app/chamados/services/disputa_emissao.py:908` | `criar_disputa_a_partir_de_chamados_orfaos`, chamadas no cron de alertas `processar_alertas_turno.py:367` | Lista de chamados órfãos regularizáveis (`:891`), sem disputa (`:907`) | **Sim no filtro de marcos**: `_ata_do_dia` em `:880`; não é um gate de veredito do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora; `acoes_disputa.py:189` | Chamado `ancora_limbo` (`:1109`), não encerrado (`:1111`), sem pergunta (`:1114`), sem disputa (`:1127`) | **Não no nascimento do pai**. |

Reaberturas lidas:

- `app/chamados/models.py:685` → `_renascer_por_premissa_viva:728` → `reabrir:756`: dedup de `abrir` reencontra encerrado; ativo (`:750`), não cascade (`:752`), sem tranca (`:754`); **consulta ata/premissa pela guarda de `reabrir`**.
- `app/chamados/models.py:962` → `premissa_regularizada:875`: reabertura de furo consulta célula (`:889`), decisão admin (`:891`), marcos acesos (`:897`), inexistentes (`:906`) e `_celula_furada` (`:908`). Este lê lâmpadas (`chamados/juizes.py:433`, `:436`) e órfãs (`:438`); ata completa sem furo retorna `False` em `:446`, bloqueando renascimento.
- `app/chamados/views_arquivar.py:68`: view desarquivar, via competência trancada (`:64`); delega à mesma guarda.
- `app/chamados/services/competencia_trancada.py:258`: reabertura de competência, escopo por empresa/via (`:252`) e janela (`:255`); mesma guarda.
- `app/chamados/services/acoes_disputa.py:266`: admin reabre disputa (`:256`), cria pergunta (`:259`), pai encerrado (`:265`); mesma guarda.
- `app/chamados/services/disputa_emissao.py:1606`: pai encerrado com irmãs respondidas sem consumo (`:1604`); mesma guarda.
- `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`: só `ausencia_colab` (`:29`), ausência ainda aberta (`:36`); não é cobrança de marco.
- `app/ponto/management/commands/detectar_ausencias.py:301`: âncora limbo, não cobrança de turno; delega guarda.
- `app/chamados/services/adesao.py:92`: pai de adesão `sem_previsao_escala`, não cobrança de marco.

Portas genéricas: `abrir` novo (`models.py:723`) e `criar` (`:813`) **vinculam** célula, mas não validam sua absolvição. Em `criar`, a junção ocorre em `:804`; não é teste de veredito. Formulários: `chamados/services/abertura.py:47`, chamado por `chamados/views.py:197`; `abertura.py:172`, chamado por `views.py:136`; API genérica `api/views.py:2237`. Permissão efetiva de módulos de turno pelos catálogos/formulários: **não li**.

Conclusão focal: nascimento pode passar sobre `concorde` tanto pelos emissores sem consulta quanto pelo veto parcial (`ponto/marcos.py:119`). Renascimento de furo com ata lavrada completa é bloqueado pelo coletor acima; não aplicar a conclusão de nascimento automaticamente a `reabrir`.

## 29/10 00:12 · sessao 01306c8543e8

T7 — leitura somente, sem execução. Abreviações: CMD=`app/chamados/management/commands/disparar_perguntas_competencia.py`; FAB=`app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Autoridade já existente / diferença |
|---|---|---|
| CMD:155 | Quais chamados estão vivos na janela? | `VIVOS` é autoridade registrada em `app/core/juizes.py:227`; o SQL restringe pela **data da célula**, sem filtro de módulo, `trabalha` ou veredito. O evento FAB:73 exige `trabalha is True`. |
| CMD:156–159 | O chamado pertence ao `--colab`? | Recorte de operador, não juiz de estado; o caminho da célula aplica o mesmo recorte em CMD:77. |
| CMD:163 | O chamado tem alguma resolvedora viva? | Copia literalmente a redução de `fio_vivo`, `app/chamados/juizes.py:1111`, registrado em `app/core/juizes.py:241`; chama corretamente o juiz individual `pergunta_viva` (`app/chamados/juizes.py:778`, registro `core/juizes.py:232`). |
| CMD:165 | Qual é o dia do fato? | Usa `ch.celula.data`; a autoridade geral é `dia_do_fato` (`app/chamados/juizes.py:60`), que prioriza célula. Não é recálculo de competência. |
| CMD:167–170 | Há motivo vencido da ata ainda não selecionado? | Já chama FAB:`motivos_pela_ata` (:34) + `vencidos_no_instante` (:15); FAB:`motivos_a_perguntar` (:68) compõe os mesmos, mas acrescenta trabalho, ativo, futuro, veredito e dedup de perguntas. CMD fio mudo **não testa veredito** nem `trabalha`. |
| CMD:168,172 | Esse fato já entrou nesta rodada? | `_vistos` é dedup em memória, inicialmente fatos faltantes do caminho da célula (CMD:154), não estado persistido. O :172 é redundante depois do filtro em :168. |
| CMD:178 | Pode perguntar ao colaborador? | Testa `situacao == 'ativo'`, também testado pela FAB:73 e por `_ja_perguntou`, `app/chamados/services/disputa_emissao.py:1206`; juiz específico registrado para esse predicado: não localizado. |
| CMD:181–184 | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta somente as perguntas da **primeira disputa** obtida, `disputa_emissao.py:1202`, e delega a `fio_vivo` em :1209. Não consulta todas as disputas do colaborador. |
| CMD:220–224 | Já existe pergunta do mesmo fato? | Mesma identidade de `PerguntaDisputa.do_fato`, `app/chamados/models.py:1927`; o gate `abrir` a usa em :1966. Esse `exists()` **não prova “decidido”**: também barra pergunta pendente, morta ou duplicata; a saída a chama FIO DECIDIDO em CMD:232. |
| CMD:225–226 | O alvo novo entra no lote? | Adiciona uma tupla por chamado; só o **primeiro** motivo ainda não visto é selecionado em CMD:167. FAB:95 devolve todos os motivos ainda não perguntados para aquela célula. |

Universos temporais:

- **Hoje não é excluído por `data >= hoje` neste HEAD.** Ambos passam por `vencidos_no_instante`: marcos vencem por instante (FAB:28; `app/ponto/services/cartorio.py:673`), e sem marcos só dia estritamente passado produz `None`, FAB:30. Hoje/futuro sem marcos produz conjunto vazio, barrado em FAB:45.
- O evento tem veto explícito `cel.data > hoje`, FAB:75; o comando não o escreve diretamente, mas usa os mesmos vencidos (CMD:111,167). Não confundir com `dia_encerrado`, registrado em `app/core/juizes.py:189`: esse exige previsão e fim encerrado (`app/ponto/turnos.py:1978`), enquanto a pergunta pode nascer marco a marco antes do fim e no passado sem marcos (FAB:15–30).
- A competência do comando é **janela de seleção**, CMD:61,74–76,155. O evento FAB:68 não restringe competência; a guarda de competência exportada foi retirada de ambos (CMD:115; FAB:83). O encadeamento agendado ainda passa `--comp-corrente --apply`, `app/config/crons.py:1435`.

O que impede substituir o emissor agendado pelo evento, item por item:

1. **Evento só atende célula efetivamente julgada nesta passada**: `app/ponto/services/cartorio.py:752,762`; impressão igual pula o julgamento em :1085. CMD:155 varre chamados mudos mesmo sem novo julgamento.
2. **Predicados diferentes**: FAB:73,81 veta célula sem trabalho/veredito absolvente; fio mudo CMD:155,167 não aplica esses vetos. Trocar preservando exatamente o universo do cron não está provado.
3. **Dedup não idêntico**: evento FAB:93 exclui duplicatas e considera as demais identidades existentes; fio mudo mistura pergunta viva da primeira disputa (CMD:181) e existência global incluindo duplicatas (CMD:220). Existe leitura canônica do fato em `models.py:1916`, mas esse ramo não a usa.
4. **Mesmo escritor já existe**: CMD:288 e FAB:123 chegam a FAB:`perguntar` (:98), que faz desfazimento de carimbo e emissão. Escritor diferente não é impedimento; seleção/gatilho são.
5. **Cron continua configurado como emissor**, `app/config/crons.py:1435`; alarme existe e não escreve, CMD:261–268, mas não é o argumento configurado.
6. **Equivalência de frota/eventos ausentes: não provado**. Não li logs nem banco; a leitura demonstra diferenças de fluxo e universo, não suas contagens reais.

## 29/10 00:11 · sessao 01306c8543e8

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 00:11 · sessao 01306c8543e8

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 00:11 · sessao 38fc51a73cb6

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 00:11 · sessao 38fc51a73cb6

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 00:11 · sessao 31880e2cc879

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 00:11 · sessao 31880e2cc879

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 28/10 23:11 · sessao 1ee182181f58

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 28/10 22:23 · sessao 1ee182181f58

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 28/10 22:05 · sessao e6c6cd74b726

**Hash de `~/codex_ws`: NÃO PROVADO.** O hash/arquivo de identificação não foi fornecido; a cópia não contém `.git` na raiz. Não consultei `~/saas-hasner`.

Leitura realizada em `/home/ronald/codex_ws`, incluindo `CLAUDE.md`, `app/docs/GLOSSARIO.md` e `app/docs/JUIZES.md`. Referências abaixo são relativas a essa cópia.

**T1 — cinco respostas**

1. `app/ponto/registro_batida.py:94` — **Quem registra uma batida?** `registrar_batida()`; o corpo concentra deduplicação, colisões e criação.
2. `app/ponto/turnos.py:2705` — **Quais batidas entram na apuração?** `batidas_apuraveis()`; filtra `retratada_em IS NULL`, janela opcional e colaborador.
3. `app/ponto/turnos.py:2041` — **O par está completo?** `par_completo()`; compõe pertencimento ao mesmo turno e dia encerrado, com carregador em `:2024`.
4. `app/ponto/portas/celula.py:554` — **Quem gera CelulaDia automaticamente?** `gerar_celulas_periodo()`; cria em `:592` e atualiza em `:614`. Regeneração formal separada: `regenerar_celulas_vinculo`, `:99`.
5. `app/config/crons.py:1924` — **Quais crons ainda julgam por varredura?** A tupla `JUIZES_POR_VARREDURA`; contém os cinco nomes solicitados.

**T2 — casamento fora de `_match_marcos`**

Autoridade declarada: `app/core/juizes.py:166`. “Turno: sim” abaixo indica contexto explícito de turno/envelope, mesmo quando construído localmente; não significa chamada a `turnos_do_colab`.

Sítios de decisão encontrados no código de aplicação e commands:

- `app/ponto/services/lastro.py:274` — **Qual batida satisfaz o marco cobrado quando falta lâmpada?** Filtra tipo e distância ao instante previsto e escolhe a mais próxima em `:287`. **Turno: sim**, formado localmente por `parear_turnos`, `:245`.
- `app/ponto/services/cartorio.py:470` — **Qual hora real acende um marco do DNA ausente da régua?** Procura hora não utilizada em raio de 30 minutos e grava `luz`. **Turno: não nesse ramo**; compara minutos de relógio.
- `app/escala/utils.py:173` — **Esta entrada é abertura ou volta do intervalo?** `_eh_abertura()` compara distância circular a `hi` e `hfi`, em `:204`. **Turno: não**; recebe uma batida e horários.
- `app/ponto/registro_batida.py:35` — **A marca coincide com marco do próprio tipo?** `_casa_marco_proprio()` compara instante/tipo aos marcos de hoje e da véspera, `:47`. **Turno: sim**, pela cadeia prevista.
- `app/ponto/turnos.py:2248` — **O tipo deve ser invertido pelo marco próximo?** `decidir_tipo_estrito()` calcula a melhor distância de cada tipo, `:2258`. **Turno: não**; ancora os horários na data do timestamp.
- `app/ponto/turnos.py:2275` — **Qual tipo de marco corresponde à marca retroativa?** `tipo_por_marcos()` reúne tipos dentro da tolerância, `:2289`. **Turno: não**; usa horário na mesma data.
- `app/ponto/turnos.py:320` — **Estas marcas internas são intervalo ou borda?** `_marca_intra_duracao()` combina sequência E/S, duração e distância às bordas, `:367`. **Turno: sim**, por estado e envelope locais.
- `app/ponto/turnos.py:1181` e `:1291` — **A batida deve ser absorvida como intervalo?** O pareador compara proximidade a marcos de intervalo e de borda. **Turno: sim**, no turno em construção.
- `app/ponto/turnos.py:878` — **A candidata é saída do turno anterior ou entrada do seguinte?** `_saida_pela_posicao_entre_turnos()` compara instantes de `hf` e `hi`, `:943`. **Turno: sim**, recebe os dois turnos.
- `app/ponto/motor_calculo_v2.py:2060` — **A entrada sem intervalo declarado pode ser tratada como cabeça da jornada?** O ramo usa distância ao início para retirar o previsto de entrada. **Turno: não nesse ramo**; recebe período, sem consultar a geometria do turno.
- `app/ponto/motor_calculo_v2.py:2113` — **A saída corresponde ao intervalo ou ao fim da jornada?** Decide por proximidade a `hii` e distância de `hf`. **Turno: não nesse casamento**; a data da saída prevista é tratada posteriormente.
- `app/ponto/calculador/regras.py:259` — **A saída do par é saída para intervalo?** Repete a comparação de proximidade e remove o marco final usado. **Turno: sim**, recebe dia da jornada e pares, mas decide localmente.
- `app/escala/regua_defesa.py:118` — **Quais marcas representam entrada, saída e ida do intervalo no diagnóstico?** Assume primeira, última e segunda marcas, `:167`, `:173`, `:223`. **Turno: sim**, por agrupamento heurístico próprio em `:154`.
- `app/escala/regua_defesa.py:503` — **A primeira marca é entrada inicial deslocada ou volta do intervalo?** Escolhe a primeira marca do dia civil e compara com `hi/hfi`, `:527`. **Turno: não**; agrupamento efetivo por data local em `:521`.
- `app/chamados/services/desvio_escala.py:74` — **Qual marco corresponde a cada marca observada?** Usa posição em dia completo, extremidades em intervalo livre e marco mais próximo nos demais, `:83`. **Turno: sim**, com deslocamento próprio da madrugada em `:66`.
- `app/chamados/services/declaracao_texto.py:131` — **Qual marco faltante corresponde à hora declarada?** `_casar_hora()` ordena distâncias circulares e recusa proximidade ambígua. **Turno: não no casamento**; recebe candidatos já identificados por dia/posição.
- `app/chamados/services/materializacao.py:274` — **Qual tipo de marco corresponde à hora respondida?** `_tipo_pelo_marco()` escolhe o marco mais próximo por distância circular, comparando-o ao marco cobrado. **Turno: não nesse ramo**; consulta a régua da `data_evento`.
- `app/ponto/propositor.py:112` — **Qual slot recebe cada marca?** `atribuir_global()` calcula atribuição monotônica própria, com custos e penalidades, `:133`. **Turno: não no núcleo**; `atribuir_global_turno()`, `:87`, fornece tratamento de virada.
- `app/ponto/propositor.py:261` — **Qual marco permanece faltando depois da atribuição posicional?** `primeiro_furo_posicional()` consome possível pausa e chama a atribuição própria, `:315`. **Turno: não para noturnos**; sequência não crescente é recusada.
- `app/ponto/services/flip_auto.py:165` — **Qual etiqueta E/S cada marca deve receber?** `etiquetas_pela_celula()` lê o slot da atribuição global, `:233`. **Turno: sim**, com janela e véspera próprias.
- `app/chamados/services/materializacao.py:128` — **O plantio ocuparia um marco faltante sem deslocar outras marcas?** Delega a `plantio_preenche_furo`, cujo casamento próprio está em `app/ponto/propositor.py:249`. **Turno: não**, recorte pelo dia civil do plantio.
- `app/ponto/propositor.py:325` — **O dia está completo pela atribuição posicional?** `dia_posicional_completo()` monta slots e marcas e consulta `primeiro_furo_posicional`, `:351`. **Turno: não**, restringe marcas à data local.
- `app/ponto/management/commands/propor_reorganizacao.py:98` — **Qual reorganização marca→slot é proposta?** Reconstrói slots/marcas da ata e chama `atribuir_global`, `:107`. **Turno: não nesse trecho**, processa a linha do dia.

Chamadores dos casamentos acima, sem algoritmo novo no ponto da chamada:

- `app/api/views_core.py:617` e `app/api/views.py:625` — **Qual tipo gravar na marca retroativa?** Delegam a `tipo_por_marcos`. **Turno: não**, selecionam marcos pela data local.
- `app/chamados/management/commands/auditar_disputas.py:119` — **A batida retroativa tem tipo invertido?** Delega a `decidir_tipo_estrito`. **Turno: não**, seleciona marcos pela data local.

Sítios examinados que respondem outra pergunta ou apenas transcrevem resultado:

- `app/ponto/services/lastro.py:82` — **Qual lâmpada já lavrada corresponde ao marco cobrado?** Busca na ata; não casa batida crua.
- `app/ponto/juiz_batida.py:227` — **Qual papel a ata já atribuiu?** Confere correspondência ata/DNA e transcreve luz.
- `app/ponto/motor_calculo_v2.py:2995` — **Qual período ocupa P1/P2?** Chama `_match_marcos`; não atende ao critério de exclusão solicitado.
- `app/chamados/detector_anomalias.py:206` — **Qual marco a marca ocupa para medir desvio?** Chama `_match_marcos`.
- `app/ponto/services/veredito_celula.py:263` — **Qual vizinha fornece candidato de preenchimento?** Busca por proximidade, mas o casamento final é submetido à grade em `:281`.
- `app/ponto/services/triagem_batida.py:434` — **Qual borda exibir como horário previsto fora da janela?** Escolhe borda para informação; não afirma marco cumprido.
- `app/ponto/management/commands/tripwire_celulas.py:183` — **Existe órfã no raio da ponta por instante?** Produz condição de conferência, sem atribuir definitivamente a marca ao marco.

**T3 — os cinco crons**

1. **`disparar_perguntas_competencia`**
   - **Command:** `app/chamados/management/commands/disparar_perguntas_competencia.py:33`; seleção em `:104` e segundo universo em `:155`.
   - **Pergunta:** “Que motivo de furo ainda precisa de pergunta nesta competência?”
   - **Juiz registrado:** nenhum único para toda a seleção. Usa componentes registrados: `classificar_falta` (`app/core/juizes.py:156`), `pergunta_viva` (`:232`) e elegibilidade da fábrica (`:238`). O corpo compartilhado lê ata em `app/chamados/services/fabrica_da_celula.py:34`.
   - **Evento:** sim, para o caminho da célula: `app/ponto/signals.py:132` → julgamento → `app/ponto/services/cartorio.py:763` → `perguntar_pela_celula`. O command mantém segundo universo de chamado com fio mudo, `:131`.
   - **Por quê:** há duas seleções explícitas no command; a chamada reativa demonstrada é por célula julgada.

2. **`fechar_cobranca_com_lastro`**
   - **Command:** `app/ponto/management/commands/fechar_cobranca_com_lastro.py:39`; delegação ao serviço em `:52`.
   - **Pergunta:** “Todos os marcos cobrados por este chamado já possuem lastro?”
   - **Juiz registrado:** **nenhum com esse predicado exato** em `core/juizes.py`; `premissa_morta` está registrado em `:229`. A decisão específica existe em `app/ponto/services/lastro.py:153` e no recuo de `:274`.
   - **Evento:** sim para marcos lavrados: signals de batida/ausência/justificativa em `app/ponto/signals.py:132`, `:144`, `:162` chegam ao cartório, que consulta `marcos_acesos` e reconcilia em `app/ponto/services/cartorio.py:1155`.
   - **Por quê:** o evento usa a ata do julgamento; o serviço do cron também possui casamento de recuo sem lâmpada.

3. **`reavaliar_ausencias_feriado`**
   - **Command:** `app/ponto/management/commands/reavaliar_ausencias_feriado.py:4`; julgamento delegado em `:23`.
   - **Pergunta:** “A cobrança de falta perdeu a premissa porque o dia é feriado ou folga?”
   - **Juízes registrados:** `fatos_do_dia`, `app/core/juizes.py:150` e `:244`; `premissa_morta`, `:229`. Composição efetiva em `app/chamados/services/feriado_reavaliar.py:33`.
   - **Evento:** sim na porta: `app/ponto/portas/celula.py:420` agenda `reavaliar_dias` em `:430`, após commit em `:432`, condicionado a `recalculo` e dias tocados.
   - **Por quê:** cron e evento chamam o mesmo serviço; seus universos são distintos.

4. **`reconciliar_chamados`**
   - **Command:** `app/chamados/management/commands/reconciliar_chamados.py:8`; ações em `:50`, `:54`, `:59`.
   - **Pergunta:** “Quais chamados perderam a premissa, quais perguntas órfãs podem ser reatadas e quais vínculos chamado→célula precisam ser preenchidos?”
   - **Juízes registrados:** `premissa_morta`, `app/core/juizes.py:229`; vitalidade de pergunta e fecho de disputa, `:232–233`. Não há juiz único registrado para a composição inteira nem para re-junção.
   - **Evento:** sim, parcialmente: `app/ponto/signals.py:207` chama `reconciliar_chamados_colab`; `app/ponto/portas/celula.py:425` agenda re-junção após regeneração. O command chama adicionalmente `retratar_chamados_absolvidos_por_celula`, `:59`.
   - **Por quê:** o command contém três ações distintas; o receiver de batida chama somente a primeira entrada de serviço.

5. **`reconciliar_perguntas_orfas`**
   - **Command:** `app/ponto/management/commands/reconciliar_perguntas_orfas.py:96`; decisão de fantasma em `:145`.
   - **Pergunta:** “A pergunta sem resposta sobre entrada/saída já é satisfeita por turno completo e bem-formado?”
   - **Juiz registrado:** **nenhum para o predicado completo**. Geometria usa `turnos_do_colab`, registrado em `app/core/juizes.py:162`; morfologia é local em `_bem_formado`, command `:59`. Fecho final consulta `DisputaSupervisao.pode_fechar`, registrado em `:233`.
   - **Evento:** não encontrei nos dois arquivos solicitados chamada ao command, `_eh_fantasma`, `_bem_formado` ou `encerrar_fantasma`. O receiver `app/ponto/signals.py:207` chama outra rotina: `reconciliar_perguntas_orfas_colab`, por `app/chamados/reconciliador.py:836`.
   - **Por quê:** a rotina reativa trata reatamento/ausência de turno; o command S142 acrescenta contagem de saídas e proximidade de até 180 minutos ao fim previsto, `:66–75`.

## 28/10 21:08 · sessao e6c6cd74b726

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 28/10 21:05 · sessao e6c6cd74b726

**Viabilidade**
- Sim, o recorte pode ser especificado e testado contra `0baf575f6656bf25af30e8b2970e627a74346af0`, usando C6 e fixtures sintéticas em banco descartável.
- Evidências nessa base: o fio restringe perguntas às disputas abertas (`chamados/services/fio.py:209–215`); o encerramento possui decisão própria (`chamados/services/disputa_emissao.py:1501–1545`); C6 já existe (`chamados/juizes.py:778`).
- Nesta etapa consultei somente essa revisão. A reprodução não será cega: soluções posteriores já foram parcialmente conhecidas na preparação anterior.

**Arquivos previstos**
- `chamados/services/fio.py`: seleção administrativa pela autoridade C6.
- `chamados/services/disputa_emissao.py`: guarda dos encerramentos pelo mesmo juiz.
- `chamados/services/vigia_fio.py` e `chamados/management/commands/vigiar_fio_morto.py`: contagem separada e política de alarme.
- Teste novo do recorte; regressões em `chamados/tests/test_fio_disputa_resolvido.py`; template administrativo somente se necessário para exposição.

**Testes previstos** [nao verificado]
- **RED:** viva em disputa fechada aparece ao administrador; chamada de encerramento não enterra pergunta viva; vigia conta o caso. Demonstrar falhas na base.
- Controles: pergunta morta não entra como pendência; disputa aberta mantém comportamento; múltiplas perguntas contam na unidade declarada.
- **GREEN:** implementar as mudanças mínimas e repetir testes específicos e regressões; conferir ausência de reabertura, push e alterações financeiras, além de idempotência. [nao verificado]
- Testar alarmes com universo não vazio: pendência administrativa contada não dispara alarme estrutural; violações estruturais existentes continuam alarmando.

**Impedimentos**
- Execução segura ainda depende de conferir settings, banco descartável, sinais e isolamento das saídas; nada disso foi executado.
- O comando do vigia escreve carimbo em disco (`vigiar_fio_morto.py:91–101`): testes deverão redirecioná-lo para diretório temporário. [nao verificado]
- A unidade proposta é **pergunta viva em disputa fechada**, separada de “dia com fio morto”; essa distinção deve constar das asserções.

## 28/10 21:00 · sessao e6c6cd74b726

1. **Candidato localizado; entrega distribuída em 10 commits**, não em um único patch. [nao verificado]
2. Primeiro: `5b8655f8085b8c5c798399d9a617029d7f463df9`; pai/base anterior: `0baf575f6656bf25af30e8b2970e627a74346af0`.
3. Último da tarefa: `5d86c33b22314ade517ae00cbf6de04d2ed24aab`; pai: `1fcc61e0b548004d5cabcb3756692aad2802822b`.
4. Merge de entrega: `7c9e4c27412376d63a5ece4d668da63902b1ab22`; primeiro pai: `05fb3365b24c1ba44076bc61dc8dc2f7c84bc7aa`.
5. **Autoria:** merge registra `Co-Authored-By: Claude Opus 5.5`; autoria exclusiva pelo Claude: **NÃO PROVADA**.
6. **Problema:** documentação registra 284 perguntas vivas em 129 disputas fechadas, ocultadas ou encerradas por estado do chamado: [RELATO.md:154](/home/ronald/saas-hasner/app/docs/RELATO.md:154).
7. **Contrato:** vitalidade decidida pelo juiz existente; fio administrativo acessível sem reabertura em lote nem push: [CORTES.md:315](/home/ronald/saas-hasner/app/docs/CORTES.md:315). [nao verificado]
8. **Aceitação final:** exportada ou não, viva em disputa fechada oferece Validar, sem reabrir/push; regra anterior de somente leitura foi retirada: [CORTES.md:326](/home/ronald/saas-hasner/app/docs/CORTES.md:326). [nao verificado]
9. **Arquivos:** 22 caminhos; modelos, serviços `fio`, `disputa_emissao`, `validacao`, `acoes_disputa`, vigia/comando e `modal_fio.html`, além dos testes. [nao verificado]
10. **Testes:** principal `chamados/tests/test_fio_le_o_juiz.py`; regressões em chamados e ponto, incluindo fio, reconciliação e chokepoint (inventário Git).
11. **Dependências verificadas no teste histórico:** Django `TestCase`, ORM, `freezegun` e mocks de push; exige banco descartável de testes. [nao verificado]
12. **Evidência histórica:** mensagem do merge declara 12.412 testes, resultado OK e rc 0; não reexecutei nem conferi os logs. [nao verificado]
13. **Reprodução isolada:** plausível a partir da base anterior, com fixtures sintéticas; independência completa do motor financeiro e configuração segura do banco: **NÃO PROVADAS**.
14. **Impedimentos:** critérios mudaram durante a tarefa; smoke posterior foi parcial; defeito de determinismo foi separado: [CORTES.md:325](/home/ronald/saas-hasner/app/docs/CORTES.md:325), [linha 331](/home/ronald/saas-hasner/app/docs/CORTES.md:331). Benchmark integral ainda requer recorte fixo.
15. Nenhuma implementação ou diff apresentado; nenhum worktree criado ou arquivo alterado. Parei.

## 28/10 19:40 · sessao e6c6cd74b726

**Codex → GPT-6 | Handoff técnico 01**

Escopo: leitura estática exclusivamente dos cinco arquivos solicitados. Nenhum teste, código Python, Docker, banco ou Git executado. **Verificado** abaixo significa demonstrado no fonte, sem comprovação em runtime. [nao verificado]

**Fluxo reconstruído**

```text
Batidas não retratadas + vínculo/DNA + ata existente
    → pareamento de turnos
    → grade: marcos, órfãs, previsto e realizado
    → classificação do dia
    → persistência de ata + veredito + impressão
    → emissão/reconciliação de cobrança e perguntas
    → releitura condicional do julgamento
```

A apuração de tempo participa da construção da ata; não ocorre exclusivamente depois da disputa.

| Etapa | Funções, contratos e autoridades verificadas |
|---|---|
| **Batida → universo apurável** | `batidas_apuraveis()` exclui `retratada_em != NULL`, aceita limites opcionais inclusivos e ordena por timestamp. O cartório chama essa função. [ponto/turnos.py:2705](/home/ronald/saas-hasner/app/ponto/turnos.py:2705), [ponto/services/cartorio.py:862](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:862). A gravação inicial da batida está fora do escopo. |
| **Batida → turno** | `turnos_do_colab()` carrega batidas não retratadas com janela ampliada, consulta `vinculo_do_dia`, agrupa dias consecutivos por vínculo e chama `parear_turnos()`. Segmentos recebem cópias das instâncias para isolar marcações transitórias. [ponto/turnos.py:1776](/home/ronald/saas-hasner/app/ponto/turnos.py:1776), [linha 1856](/home/ronald/saas-hasner/app/ponto/turnos.py:1856), [linha 1888](/home/ronald/saas-hasner/app/ponto/turnos.py:1888). |
| **Papel da batida** | `papel_por_minuto_da_ata()` depende de `ponto.juiz_batida.periodos_do_dia`; fornece papéis `E/S/X/Xi`. Quando informado, o papel da ata prevalece sobre o tipo gravado. Sem resposta da ata, existe retorno ao tipo. [ponto/turnos.py:1491](/home/ronald/saas-hasner/app/ponto/turnos.py:1491), [linha 1127](/home/ronald/saas-hasner/app/ponto/turnos.py:1127). |
| **Turno → realizado** | `realizado_do_dia()` delega a `realizado_dos_turnos()`: minutos por `data_turno`, tratamento de pausas e marcas de turno aberto/longo. Sem turno retorna `minutos=None, sem_turno=True`. É tempo realizado; indenização de intervalo pertence ao motor externo. [ponto/turnos.py:642](/home/ronald/saas-hasner/app/ponto/turnos.py:642), [linha 709](/home/ronald/saas-hasner/app/ponto/turnos.py:709). |
| **Grade → ata** | O montador por turno consulta `vinculo_do_dia`, monta os marcos e lê os turnos/realizado. Converte `sem_turno` em zero **com indicador explícito**. `ata_do_dia()` serializa lâmpadas, minutos, órfãs e contagens. [escala/utils.py:1037](/home/ronald/saas-hasner/app/escala/utils.py:1037), [linha 1630](/home/ronald/saas-hasner/app/escala/utils.py:1630), [linha 1773](/home/ronald/saas-hasner/app/escala/utils.py:1773), [ponto/services/cartorio.py:542](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:542). |
| **Julgamento → veredito** | O cartório fornece grade, cobertura, cadastro, encerramento e cobranças a `ponto.supra_juiz.classificar_dia()`. Os códigos alimentam a derivação do veredito; há tratamento explícito de `cobrado` quando existe fio vivo. [ponto/services/cartorio.py:1031](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1031), [linha 1101](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1101), [linha 1114](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1114). |
| **Persistência** | `lavrar_veredito()` escreve ata/veredito/protestos; decisão administrativa impede sobrescrita do veredito pelo cartório, mas permite atualizar a ata. `carimbar()` persiste impressão e insumos junto dos campos lavrados. Repetição idêntica possui guardas de ausência de escrita. [ponto/portas/celula.py:621](/home/ronald/saas-hasner/app/ponto/portas/celula.py:621), [linha 682](/home/ronald/saas-hasner/app/ponto/portas/celula.py:682). |
| **Veredito → cobrança/disputa** | O cartório chama o emissor para furos e depois `perguntar_pela_celula()`. Quando houve emissão, pode repetir o corpo do julgamento para incorporar o fio recém-criado. Criação/materialização efetiva de disputas depende de módulos externos não examinados. [ponto/services/cartorio.py:1347](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1347), [linha 1371](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1371), [linha 747](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:747). |
| **Vitalidade da disputa** | `pergunta_viva()` memoiza por estado da instância; `_pergunta_viva_calc()` considera validação, tranca, resposta, decisão administrativa, marco e veredito. `fio_vivo()` é `any(pergunta_viva(...))`. Disputa aberta sem perguntas também pode bloquear encerramento do chamado. [chamados/juizes.py:778](/home/ronald/saas-hasner/app/chamados/juizes.py:778), [linha 1005](/home/ronald/saas-hasner/app/chamados/juizes.py:1005), [linha 1102](/home/ronald/saas-hasner/app/chamados/juizes.py:1102), [linha 1809](/home/ronald/saas-hasner/app/chamados/juizes.py:1809). |

**Contratos importantes para a próxima IA**

- **Chamado vivo ≠ cobrança com fio vivo.** O julgamento recebe ambos separadamente; o fio depende das perguntas. [ponto/services/cartorio.py:1062](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1062).
- **Idempotência depende dos insumos.** A impressão inclui batidas, estado dos chamados, DNA e componentes adicionais; impressão igual pode pular julgamento, salvo condições explícitas ou `forcar`. [ponto/services/cartorio.py:307](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:307), [linha 1076](/home/ronald/saas-hasner/app/ponto/services/cartorio.py:1076).
- **Ausência de conhecimento tem representação própria.** Ata sem `lampadas` pode produzir `None`; lâmpada `acesa=None` não equivale a marco faltante. [chamados/juizes.py:429](/home/ronald/saas-hasner/app/chamados/juizes.py:429).
- **Regeneração difere de relavratura.** A regeneração pode alterar DNA e, com recálculo habilitado, chamar julgamento dos dias tocados antes da etapa financeira. Falhas desse julgamento são registradas e capturadas. [ponto/portas/celula.py:350](/home/ronald/saas-hasner/app/ponto/portas/celula.py:350).

**Ressalvas verificadas no código**

1. O dispatcher da grade mantém caminho legado. Se `GRADE_POR_TURNO` estiver ausente, o `getattr(..., False)` seleciona esse caminho; configuração efetiva: **NÃO PROVADA**. [escala/utils.py:1816](/home/ronald/saas-hasner/app/escala/utils.py:1816).
2. O previsto do intermitente convocado pode ser substituído pelo realizado: exceção explícita à independência entre previsto e realizado. [escala/utils.py:1794](/home/ronald/saas-hasner/app/escala/utils.py:1794).
3. O cálculo do previsto conserva fallback de pausa e documenta colisão entre decisões anteriores. A divergência normativa está relatada no comentário; sua resolução externa é **NÃO PROVADA**. [escala/utils.py:2021](/home/ronald/saas-hasner/app/escala/utils.py:2021).
4. O ciclo ata → papel → turno → grade → nova ata é demonstrado pelas chamadas. **Hipótese a verificar:** convergência universal desse ciclo; a leitura estática não a comprova.

**Limites do handoff**

O **Handoff 02 não foi fornecido**; divergências específicas contra ele são **NÃO PROVADAS**. Também permanecem fora da prova: gatilhos reais de entrada, implementação dos juízes importados, materialização das respostas e apuração financeira/fechamento. Não foram feitas inferências sobre colaboradores.

Reconhecimento concluído. Aguardando instruções.

## 28/10 19:13 · sessao e6c6cd74b726

Os quatro arquivos existem e foram confrontados por leitura estática. Não executei Python, testes, Docker, banco de dados ou operações Git. [nao verificado]

**1. Hierarquia das autoridades**

A hierarquia demonstrada é funcional; não há uma ordem absoluta entre todos os documentos:

| Camada | Autoridade e fonte |
|---|---|
| Regra normativa | CLT, CCT e decisões `L-NNN` definem a resposta esperada; o código existente não é gabarito. [LEIS.md:136](/home/ronald/saas-hasner/app/docs/LEIS.md:136). |
| Registro das decisões | `LEIS.md` se declara índice, com origem em `CLAUDE.md`, `CORTES.json`/`CORTES.md` ou BACKLOG. Distingue quem aplica a lei, código protegido e evidência. [LEIS.md:3](/home/ronald/saas-hasner/app/docs/LEIS.md:3), [linha 27](/home/ronald/saas-hasner/app/docs/LEIS.md:27), [linha 43](/home/ronald/saas-hasner/app/docs/LEIS.md:43). |
| Autoridade por pergunta | `core/juizes.py` é apontado como registro de perguntas, autoridades, perguntas sem juiz e pendências. Seu conteúdo não foi auditado nesta etapa. [MAPA.md:58](/home/ronald/saas-hasner/app/docs/MAPA.md:58). |
| Contratos estruturais | `MATRIZ` declara conformidade por família, teste, exceções e idempotência; os selos devem conferir essas declarações. [contratos_estruturais.py:21](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:21), [linha 62](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:62). | [nao verificado]
| Mapas derivados | `MAPA-JUIZES.md` declara geração a partir de `core/juizes.py`; `MAPA.md`, por `bin/gerar_diagrama.py`. Ambos proíbem edição manual. [MAPA-JUIZES.md:3](/home/ronald/saas-hasner/app/docs/MAPA-JUIZES.md:3), [MAPA.md:1](/home/ronald/saas-hasner/app/docs/MAPA.md:1). |

A existência e eficácia dos geradores e selos citados permanecem **NÃO PROVADAS**.

**2. Diferença entre os conceitos**

- **Lei de negócio:** determina o comportamento esperado do domínio. Exemplo: regras da janela de horas extras, cadastro e decisão humana. [LEIS.md:115](/home/ronald/saas-hasner/app/docs/LEIS.md:115).
- **Juiz único:** função ou atributo canônico que responde uma pergunta específica. Pode ser compartilhado entre perguntas/famílias; “único” não significa uma função para todo o sistema. [MAPA-JUIZES.md:7](/home/ronald/saas-hasner/app/docs/MAPA-JUIZES.md:7).
- **Contrato estrutural:** restrição sobre a organização do código: um juiz por pergunta, um escritor por entidade e parâmetro consumido ou declarado sem efeito. Idempotência integra o contrato de escrita. [contratos_estruturais.py:33](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:33), [linha 55](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:55).
- **Mapa gerado:** representação derivada das autoridades e relações declaradas. Seus números têm limites explícitos de cobertura; ausência de varredor deve aparecer como “não sei”. [MAPA.md:70](/home/ronald/saas-hasner/app/docs/MAPA.md:70).

Uma lei escrita, uma célula declarada verde e conformidade efetivamente comprovada são evidências distintas. [nao verificado]

**3. Função `total()`**

**CONFIRMADA**, sem execução, em [contratos_estruturais.py:1037](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:1037).

Ela calcula o **teto de contratos exigíveis**:

```text
quantidade de famílias × quantidade de contratos
+ contrato global
− famílias sem cadastro
```

`familias_sem_cadastro()` consulta `core.configuracao_efeito.familias_com_parametro()` e considera apenas as famílias da matriz. [Linha 1021](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:1021).

`total()` não mede contratos cumpridos, não executa testes e não conta verdes. Essa contagem pertence a `verdes()`; o placar combina as duas funções. [Linha 1052](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:1052). [nao verificado]

O retorno atual **20** permanece **NÃO PROVADO**: está documentado, mas depende de uma fonte externa não lida nesta etapa.

**4. Contradições concretas e diferenças explicáveis**

| Achado | Classificação |
|---|---|
| `MAPA.md` apresenta **15** perguntas com juiz em ausência/férias, incluindo `periodo_da_parcial_no_dia`; `MAPA-JUIZES.md` apresenta **14**, omitindo essa pergunta. [MAPA.md:128](/home/ronald/saas-hasner/app/docs/MAPA.md:128), [MAPA-JUIZES.md:68](/home/ronald/saas-hasner/app/docs/MAPA-JUIZES.md:68), [total:177](/home/ronald/saas-hasner/app/docs/MAPA-JUIZES.md:177). | **Divergência documental concreta**, na mesma família. Qual mapa corresponde ao registro atual e quando ocorreu a divergência: **NÃO PROVADO**. |
| `MAPA-JUIZES.md` chama os 124 pendentes de portas de leitores com regra própria; `MAPA.md` explica que esses 124 são “não sei” por desenho: o juiz é um censo que as portas não devem chamar. [MAPA-JUIZES.md:167](/home/ronald/saas-hasner/app/docs/MAPA-JUIZES.md:167), [MAPA.md:217](/home/ronald/saas-hasner/app/docs/MAPA.md:217). | **Contradição de significado**, com a mesma contagem. A descrição genérica do primeiro mapa não representa a explicação específica do segundo. |
| L-121, datada de 27/10, exige “DIFF antes” para reconstrução de projeções não exportadas; `CLAUDE.md` registra retirada dessa exigência em 21/10 e diz que ela sobrevive somente para exportada e troca de motor. [LEIS.md:142](/home/ronald/saas-hasner/app/docs/LEIS.md:142), [CLAUDE.md:597](/home/ronald/saas-hasner/CLAUDE.md:597). | **Tensão normativa concreta.** Pode existir exceção posterior específica, mas a reconciliação exige o corte/ADR original: **NÃO PROVADO**. |

Diferenças que não devem ser tratadas automaticamente como contradições:

- **22 versus 20 contratos:** o próprio módulo preserva 22 como critério histórico e explica a alteração pela L-100. [contratos_estruturais.py:1](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:1).
- **Dez famílias nos mapas versus sete na matriz:** inventário de autoridades e matriz estrutural têm universos distintos. [MAPA.md:60](/home/ronald/saas-hasner/app/docs/MAPA.md:60), [contratos_estruturais.py:45](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:45).
- **Competência 21–20 versus holerite por mês civil:** exceção de domínio explicitamente documentada na L-104. [LEIS.md:130](/home/ronald/saas-hasner/app/docs/LEIS.md:130).
- **Números antigos versus células posteriormente verdes:** há estados auditados em 05/10 e notas posteriores datadas de 24–26/10. Isso demonstra defasagem entre registros, sem provar regressão. [LEIS.md:181](/home/ronald/saas-hasner/app/docs/LEIS.md:181), [contratos_estruturais.py:352](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:352), [linha 495](/home/ronald/saas-hasner/app/core/contratos_estruturais.py:495).

**5. Confronto com o `CLAUDE.md`**

| Afirmação | Resultado |
|---|---|
| Matriz declarada em `core/contratos_estruturais.py`, com três contratos e idempotência no segundo — linhas 279–304. | **CONFIRMADA por leitura do código.** |
| `total()` deriva o teto da fonte de famílias com parâmetros — linhas 289–292. | **CONFIRMADA por leitura do código.** Valor atual: **NÃO PROVADO**. |
| “Hoje 15/20” — linhas 685–686. | **Contrariado como retrato da declaração atual:** há 19 células explicitamente verdes, incluindo a global. A evolução é datada nas notas; conformidade real dessas 19: **NÃO PROVADA**. |
| Todo consumidor deve ler a autoridade — linhas 20–22 e 276. | **CONFIRMADA como norma**, mas os mapas registram pendências e lacunas. Cumprimento universal: **NÃO PROVADO**. |
| Competência exportada permanece intacta pela L-092 — linhas 602–615. | **Qualificada/contrariada como proibição absoluta** pela L-113, que declara revogar a recusa de recálculo e registra sua remoção. Isso não prova autorização irrestrita ao agente. [LEIS.md:148](/home/ronald/saas-hasner/app/docs/LEIS.md:148). |
| TICKETS e HAIKU leem a matriz — linhas 293–295. | Há referências documentais concordantes, mas os consumidores não foram lidos: **NÃO PROVADO no código**. |
| Selos efetivamente impedem violações; produção corresponde ao desenho descrito. | **NÃO PROVADO**, sem leitura dos selos, execução ou inspeção de produção. |

Etapa concluída. Aguardo novas instruções.

## 28/10 19:10 · sessao e6c6cd74b726

Li integralmente [CLAUDE.md](/home/ronald/saas-hasner/CLAUDE.md). Executei apenas leituras desse arquivo. As regras de execução da fila nele descritas não foram acionadas, conforme sua instrução de auditoria SOMENTE LEITURA. [nao verificado]

**1. Regras obrigatórias para agentes de IA**

- Ler o documento inteiro antes de ações de código, infraestrutura, banco ou Git; conferir divergências com o estado vivo (linhas 10–12).
- Seguir a **LEI-AKITA**: corrigir na origem, usar autoridade única, medir riscos e números na fonte, respeitar literalmente avais, construir em cópia do HEAD e validar pela regra, sem usar o código antigo como oráculo (seção 0, linhas 14–60).
- Toda proposta de correção deve declarar origem, autoridade, teste RED, demais leitores, juízes e padrão arquitetural. Padrão novo depende de corte de Ronald (linhas 62–70). [nao verificado]
- Ler antes de afirmar, citar arquivo/linha e consultar `LEIS.md`, `DOSSIES.md` e `CORTES.md` antes de patch ou prompt (seção 6, linhas 335–340).
- Código de negócio exige RED → correção → GREEN → suíte; medições devem chamar a função real e alterações exigem censo de escritores (linhas 345–359). [nao verificado]

**2. Proibições de alteração, Git, testes, deploy e produção** [nao verificado]

| Área | Restrições demonstradas |
|---|---|
| Alterações | Nunca editar a árvore viva durante a construção (39–40, 163–167). `ponto/motor_calculo_v2.py` exige aval explícito (242). Proibido corrigir por código divergências de cadastro ou batida (311–321). |
| Git | `add` por caminho, nunca `-A`; não commitar sem “ok”; `pull/reset/checkout/restore` somente com tar e propósito; nunca dois pushes simultâneos (361–367). Proibido editar `.py` entre certificação da suíte e pouso (542–545). | [nao verificado]
| Testes | Não executar suíte no `saas_core`; usar isolamento de recursos e um run por vez (99–119, 194–239). Proibidos atalhos como pular teste ou `SKIP_TESTS` sem `!` de Ronald (500–502). | [nao verificado]
| Deploy | Publicação exclusivamente por `bin/deploy.sh`; restart simples não serve como deploy (137–141, 177–182). Merge e deploy devem ocorrer no mesmo ato (168–176). Migration pendente segura o deploy (628–633). |
| Produção | POST de teste em produção é escrita. Smoke de porta deve usar banco lateral, transação com exceção final e prova de rollback; nunca `saas_hasner` (402–410). Apagar ou restaurar arquivo usado por produção exige intervenção humana (500–516). | [nao verificado]

O arquivo **não estabelece proibição geral** de alterações, Git, testes ou deploy: contém autorizações operacionais condicionadas na seção 7b. A proibição absoluta nesta auditoria vem da sua instrução. Uma proibição geral equivalente no documento é **NÃO PROVADO**. [nao verificado]

**3. Documentos que definem leis e decisões arquiteturais**

- **`app/docs/LEIS.md`, `DOSSIES.md` e `CORTES.md`**: consulta obrigatória antes de alterações, com referência `L-NNN` (337–340).
- **`app/docs/GLOSSARIO.md` e `PLANO-PADROES.md`**: norte do desenho e padrões obrigatórios para correções (65–70).
- **`app/docs/LEIS-UI.md`**: leis de interface (9).
- **`app/docs/JUIZES.md`**: tabela das autoridades por domínio (325–327).
- **`app/docs/LAPIDES.md`**: histórico das regras; regras vigentes permanecem no `CLAUDE.md` (656–660).
- **`app/docs/PRIMER.md` e `ARQUITETURA.mmd`**: visão geral e diagrama gerado do código (1–8).

O documento também declara a matriz arquitetural em `app/core/contratos_estruturais.py` e a autoridade do teto em `core/contratos_estruturais.py::total()` (279–304).

**Conteúdo, consistência e vigência efetiva desses arquivos externos: NÃO PROVADO**, pois não foram lidos nesta etapa.

**4. Condições que exigem intervenção humana**

- Alterar o motor v2: aval explícito de Ronald (242).
- Regra de negócio nova, predicado novo de juiz, dinheiro de competência exportada, dados de escala/vínculo e apagar ou restaurar arquivo usado por produção: reservados a Ronald (505–521).
- Classe nova de bug que muda o desenho do sistema: decisão conceitual humana (486–487).
- Aval condicional descumprido: parar com os números/tabela (37–38, 617–623).
- Alterações de frontend abrangidas pela regra exigem smoke de clique de Ronald nas duas interfaces antes do push (376–385). [nao verificado]
- Condição de saída de pendência ambígua, universo vazio ou resultado fora da condição: encaminhar pergunta (689–698).

Há qualificações posteriores que impedem aplicar isoladamente as proibições antigas:

- Dinheiro em competência **não exportada** é declarado pré-aprovado com reversão anterior, integridade da exportada por hash e prova de frota posterior (597–615).
- Pergunta de lei deve ser encaminhada, mas não interrompe outras tarefas independentes segundo a regra das linhas 72–84.
- Registrar autoridade já existente pode ser delegado ao agente sob condições medidas (510–521); há delegação específica adicional para juízes de prazo (699–704).

**Existência de avais concretos e cumprimento dessas condições no ambiente atual: NÃO PROVADO.**

Etapa concluída. Aguardo novas instruções.

## 28/10 19:08 · sessao e6c6cd74b726

Saída integral:

```text
## main...origin/main [ahead 6]
 M config/crons_duracao.json
 M docs/PROMPTS.md
 M docs/RELATO.md
?? ../esteira.pausada
?? ../fatias_agendadas/abono-no-ar/esteira.out
?? ../fatias_agendadas/abono-no-ar/esteira_agendada.out
?? ../fatias_agendadas/abono-no-ar/fatia.done
?? ../fatias_agendadas/abono-no-ar/msg_commit.txt
?? ../fatias_agendadas/b2-no-ar/
?? ../fatias_agendadas/fechador-no-ar/
?? ../fatias_agendadas/o135-teto20/esteira.out
?? ../fatias_agendadas/o135-teto20/msg_commit.txt
?? ../fatias_agendadas/o221-pouso3/
?? ../fatias_agendadas/o221-pouso4/
?? ../fatias_agendadas/o221-pouso5/
?? ../fatias_agendadas/o223-pousob_msg.txt
?? ../fatias_agendadas/o44-deploy/ [nao verificado]
?? ../scratchpad/
```

Código de retorno: `0`.

_3 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
