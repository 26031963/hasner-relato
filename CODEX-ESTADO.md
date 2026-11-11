# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 23:24** · publicado em **10/11/2026 23:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 23:24 · sessao fd6adff78494 · - · - · origem=Ronald (tmux)

K4: `27 != 28` compara **PKs**, não contagens (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:138`). A porta seleciona a primeira disputa aberta (`chamados/services/disputa_emissao.py:1290`), ordenada por `-aberta_em` (`chamados/models.py:1804`). O campo usa `auto_now_add=True`; não há evidência de defeito no patch de relógio (`chamados/models.py:1756`).

A local vazia nasce antes dos chamados (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:130`). Criar chamados aciona o cartório (`chamados/signals.py:188`); um encerramento aciona S143 (`chamados/signals.py:91`), cujo resolver varre todas as disputas abertas e fecha as vazias (`chamados/services/disputa_emissao.py:2130`, `chamados/services/disputa_emissao.py:2178`). **Qual chamado efetivamente provocou esse caminho no traceback: [nao verificado].** Correção defensável da fixture: criar a local vazia por último e verificar, antes da chamada, que continua aberta, sem motivos/perguntas e posterior à dona; preservar todas as asserções finais.

K5: o repasse chama `ch.analisar()` (`chamados/services/disputa_emissao.py:1492`, `chamados/services/disputa_emissao.py:1496`), acordando o cartório; ele ressuscita perguntas (`ponto/services/cartorio.py:1633`), cuja porta reabre a dona (`chamados/models.py:1573`). A reabertura limpa somente `fechada_em`, `fechada_por`, `resolucao` e `motivo_fechamento`, anotando o chamado quando existe (`chamados/models.py:2173`). O teste deve exigir essa transformação exata e preservar os demais campos.

Na segunda passada do replay, a dona já aberta pode ser selecionada, receber o motivo e ser retornada com push; exigir novamente `None` e nenhum push conflita com esse caminho (`chamados/services/disputa_emissao.py:1290`, `chamados/services/disputa_emissao.py:1453`, `chamados/services/disputa_emissao.py:1516`). Separar as expectativas da primeira passagem das da segunda.

O `SystemExit(2)` do auditor é um alarme deliberado quando F1/F2 são positivos; o teste deve capturá-lo, exigir código 2 e continuar verificando I23 (`chamados/management/commands/auditar_invariantes_chamados.py:1942`).

## 10/11 23:13 · sessao cc7466557084 · FIO-E0-V4-E4B · leitura · origem=Code

**Os dois resultados vêm de perguntas diferentes no código:** o command consulta ações exigíveis por instância de marco; o vigia usado pelo command consulta retenção da folha e alcançabilidade de uma porta. O vigia não consulta `tarefa_do_dia`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–76`; `app/chamados/services/tarefa_do_dia.py:235–288`; `app/chamados/services/vigia_fio.py:635–699`)

Os números 253, 116, 64, 1 resolvido e 385 → 384 são as medições fornecidas no enunciado. A distribuição dos 116 e dos 64 entre os ramos abaixo, e os insumos de cada dia, permanecem **[nao verificado]**.

**P1. Quem diz “nada” e quem diz “morto”.**

O caminho da tarefa é `ato_do_dia` → `tarefa_do_dia` → `CargaDaTarefa.tarefa` → `_tarefa_pura`. A função pública exige `agora`; na chamada avulsa do command, carrega exatamente `[dia, dia]`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–76`; `app/chamados/services/tarefa_do_dia.py:396–406`; `app/chamados/services/tarefa_do_dia.py:442–452`)

Há uma distinção relevante: **`tarefa_nada` é um rótulo construído pelo command**, não o nome de um retorno direto do juiz. Ele aparece quando:

- não foram acumulados `registrar_decisao`, `perguntar`, `materializar` ou `rejulgar`;
- não foi encontrada ação com `porta is None`;
- `t['nao_sei']` é falso.

Ações humanas **com porta preenchida** não entram na lista `humanos`; portanto, isoladamente, `tarefa_nada` não prova que todas as listas `acoes` estavam vazias. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`)

| Aspecto | Predicado da tarefa |
|---|---|
| Universo de perguntas | `PerguntaDisputa` pela coluna `colaborador_id`, `conversa=False`, `data_evento` dentro de `[ini,fim]`, ordenadas por colaborador e PK. Não filtra por pergunta viva, disputa aberta, validação ou materialização. (`app/chamados/services/tarefa_do_dia.py:323–329`) |
| Identidade considerada | Somente motivos de marco; exclui vias fora do fato, com a exceção temporal da retratação posterior a `agora`; exclui última decisão de retratação. Agrupa por `data_evento == dia` e posição/instância do marco. (`app/chamados/services/tarefa_do_dia.py:223–234`) |
| Decisão considerada | Sequência de `DecisaoPergunta`, ordenada por `(ato_em,pk)`, considerando somente `ato_em <= agora`; a última linha dessa sequência governa a ação. (`app/chamados/services/tarefa_do_dia.py:337–342`; `app/chamados/services/tarefa_do_dia.py:129–159`) |
| Dispensas do dia | `fora_vinculo`, isenção e competência trancada retornam sem ação. Sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`) |
| Ausência de lâmpadas | Em dia que não é dispensado por trabalho, sem instâncias: havendo marcos e nenhum vencido, retorna `nao_venceu`; caso contrário, `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`) |
| Dispensas por instância | Sem ação quando dia sem trabalho, lâmpada `True`, marco não vencido ou ausência aprovada. Lâmpada `None` ou teto desconhecido produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`) |
| Marco que exige pergunta | Lâmpada apagada, marco vencido, sem cobertura que desvie a ação e sem pergunta da posição: exige `PERGUNTAR`. (`app/chamados/services/tarefa_do_dia.py:258–279`) |
| Encerramentos pela sequência | Validação com lavra vigente `sim` e sem Batida de efeito: `encerrada_pelo_juiz`; silêncio: `silencio_decidido`; absorção em ausência aprovada: `absorvida_aprovada`. Todos retornam ações vazias. (`app/chamados/services/tarefa_do_dia.py:140–154`) |
| Demais ações | Resposta, recusa, reabertura, devolução e absorção não final podem exigir ação humana; validação com lavra `nao` exige materialização; efeito presente com lâmpada ainda apagada exige rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`) |

O `dia_sem_trabalho` compartilhado considera `trabalha is False` **ou** `ata.tipo_dia` preenchido e diferente de `trabalho`. Para ausência parcial que cobra o resto, somente `trabalha is False` dispensa o dia inteiro. (`app/chamados/juizes.py:413–441`; `app/ponto/supra_juiz.py:65–71`)

O vigia usado neste backfill é **`censo_alcance`**, selecionando `alcance_dias` com rota `sem_porta`; não é o `censo` antigo que começa em chamado vivo com célula. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`; `app/chamados/services/vigia_fio.py:493–499`)

| Aspecto | Predicado do vigia usado |
|---|---|
| Empresas | Empresas visíveis; o recorte solicitado é uma interseção. A fonte de visibilidade exige empresa ativa e exclui o prefixo de CNPJ declarado nela. (`app/chamados/services/vigia_fio.py:566–570`; `app/colaboradores/queries.py:78–87`) |
| Competências | A atual entra sempre no enumerador; a anterior entra se não estiver entregue. Não há teste de entrega da **atual** nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`) |
| Universo inicial | `dias_que_seguram`: lê `classificar_export`, seleciona colaboradores cujo motivo é `furo_espelho` e obtém seus dias pela retenção da célula. O censo deduplica `(colaborador,data)`. (`app/folha/fora_da_folha.py:95–99`; `app/folha/fora_da_folha.py:118–137`; `app/chamados/services/vigia_fio.py:581–588`) |
| Modelos/status da folha | `classificar_export` começa em `FechamentoMensal` da empresa/mês/ano, sem filtro de status; `furo_espelho` só é atribuído quando a cadeia chega ao teste da retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`) |
| Retenção do dia | Exclui isento, recorta admissão/demissão e lê `CelulaDia` por `data__range`. Retém acusação, salvo protestos inteiramente não cobráveis ou exceção da pausa; também retém cegueira passada nas condições expressas. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`) |
| Vereditos acusadores | `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`. (`app/escala/models.py:1767–1768`) |
| Perguntas examinadas | Por **`celula_id`**, sem filtro explícito `conversa=False` ou `data_evento`; seleciona as vivas pelo C6 `pergunta_viva`. (`app/chamados/juizes.py:1502–1505`) |
| Porta alcançável, C17 | Chamado da célula na fila **ou** pergunta viva da célula, presente no universo do app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`) |
| Fila/status | `VIVOS = aberto, em_analise, registrado`; a fila exige `COBRANCA = aberto, em_analise` e exclui `silenciado_ate > agora`. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/services/painel.py:188–190`) |
| Exclusões antes de contar morto | Havendo porta, sai; hoje/futuro sai; depois saem Resolver dia oferecido, ausência aberta cobrindo, aceite de folga validado e dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:306–315`) |

O C6 lê projeção/carimbos, tranca, vias, resposta, veredito da célula, fato do marco e materialização. Seu corpo não decide pela última `DecisaoPergunta`, como faz a tarefa. (`app/chamados/juizes.py:1324–1410`; `app/chamados/services/tarefa_do_dia.py:129–159`)

A condição exata da divergência é a conjunção:

- **“Morto”**: dia no universo retido, C17 sem porta, `data < hoje` e nenhum dos quatro baldes de dono.
- **`tarefa_nada`**: redução do command sem ato de sistema/passivo, sem ação com porta nula e `nao_sei=False`.

Assim, retenção agregada e ausência de porta podem coexistir com dispensa ou encerramento das ações por marco. O código desses dois caminhos não exige equivalência entre as respostas. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:180–288`)

Para ser especificamente **classe `d`**, ainda é necessário: célula existente; nenhum `veu` de chamado vivo da célula ou âncora viva fora da fila; nenhuma pergunta C6 viva ligada à célula; dia fora da lista CxR; colaborador presente na lista CxR. Isso não significa “nenhuma pergunta histórica existe para o fato”. (`app/chamados/services/vigia_fio.py:661–678`; `app/chamados/services/fabrica_da_celula.py:145–157`)

Um caminho suficiente demonstrável é: dia que permanece retido e sem porta/dono, mas cujo juiz de tranca responde verdadeiro; a tarefa retorna sem ação por `competencia_trancada`, enquanto a retenção continua lendo veredito/protestos. Ocorrência desse caminho nos 116: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:190–198`; `app/folha/export.py:1233–1256`; `app/chamados/services/vigia_fio.py:635–699`)

**Existe juiz registrado:** `_C19` aponta para `tarefa_do_dia`; o command chama essa autoridade. O vigia consulta C17/C6 e a cadeia de dono, sem chamar C19 no corpo de `censo_alcance`. O registro declara que os cinco respondedores anteriores continuam vivos durante essa etapa. (`app/core/juizes.py:150–155`; `app/core/juizes.py:289–292`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`; `app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:635–656`)

**P2. Ramos da fábrica sem inserção de `PerguntaDisputa`.**

Primeiro, o limite da medição: **`segue_sem_porta` não prova zero perguntas criadas nem zero perguntas vivas depois do ato**. O command descarta o retorno da fábrica e classifica pelo reaparecimento do `celula_id` no recenso `sem_porta`; a classe/detalhe impressos são os guardados no plano anterior. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:195–196`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:240–258`)

O caminho é `perguntar_pela_celula` → `motivos_a_perguntar` → `motivos_pela_ata`; com motivos, segue por `perguntar` → `criar_ou_atualizar_disputa` → `gerar_perguntas_disputa`. Depois de obter a disputa, `perguntar` chama o gerador novamente. (`app/chamados/services/fabrica_da_celula.py:160–175`; `app/chamados/services/fabrica_da_celula.py:201–220`; `app/chamados/services/disputa_emissao.py:669–688`)

**Nenhum retorno normal abaixo é distinguido pelo command:** tanto `[]` quanto uma lista de motivos são descartados. Exceção que escapa recebe tratamento diferente: rollback do dia e `recusa:<tipo>`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

| Ramo | Condição e saída |
|---|---|
| Sem adesão | `veredito == 'nunca_bateu'`: retorno imediato `[]`, inclusive antes da pauta de cadastro. Esse veredito pertence ao conjunto acusador da retenção. (`app/chamados/services/fabrica_da_celula.py:87–89`; `app/chamados/services/fabrica_da_celula.py:183–184`; `app/escala/models.py:1767–1768`) |
| Passado sem turno previsto | Dia passado, colaborador ativo, não dispensado por `dia_sem_trabalho`, e nenhum marco vencido **nem pendente**: `vencidos_no_instante` devolve `None`; tenta pauta de cadastro e retorna `[]`. Vínculo/tipo ausente ou cadeia vazia podem produzir ausência de marcos. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:201–205`; `app/ponto/services/cartorio.py:840–859`) |
| Pauta de cadastro já entregue | Após a trava, a pauta não é repetida se existe cabeça viva do colaborador ou pauta fechada em data civil local `>= dia`. O booleano do escritor é ignorado; com pauta nova ou antiga a fábrica retorna `[]`. (`app/chamados/services/fabrica_da_celula.py:327–335`; `app/chamados/services/fabrica_da_celula.py:364–373`; `app/chamados/services/fabrica_da_celula.py:204–205`) |
| Guardas gerais | `dia_sem_trabalho`, situação atual diferente de `ativo` ou dia futuro: motivos vazios. Veredito preenchido fora de `VEREDITOS_ACUSAM` também devolve vazio; veredito vazio não cai nessa guarda. (`app/chamados/services/fabrica_da_celula.py:105–121`) |
| Parcial somente na lista | `cobertura_parcial` com `periodo_parcial == 'sem_periodo'`; ou parcial com julgamento anterior e sem fato novo desde ele. O command não passa `julgada_antes`, portanto a fábrica usa `cel.julgada_em`. Retorna motivos vazios. (`app/ponto/services/cartorio.py:395–400`; `app/chamados/services/fabrica_da_celula.py:123–125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`) |
| Nenhum marco venceu | `motivos_pela_ata` retorna `[]` para teto vazio ou `None`. Havendo somente marcos pendentes, não abre pauta de falta de turno: existem marcos, apenas nenhum venceu. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:58–61`) |
| Exceção engolida ao ler ata | `_ata_do_dia` captura a exceção e retorna `(True,None)`. Apesar do comentário “falha de leitura não barra”, este chamador barra porque `faltantes is None`. (`app/chamados/services/disputa_emissao.py:894–901`; `app/chamados/services/fabrica_da_celula.py:64–66`) |
| Ata ausente/agregada | Estado `sem_celula` com escala vigente retorna `(False,None)`; sem escala retorna `(True,None)`. Ambos impedem motivos neste caminho. A ausência de pares de marcos no leitor também pode produzir `sem_celula`. (`app/chamados/services/disputa_emissao.py:902–913`; `app/chamados/services/declaracao_texto.py:121–127`; `app/chamados/services/fabrica_da_celula.py:65–66`) |
| Nenhum faltante elegível | O leitor exclui isento/fora do vínculo, dia não trabalho sem parcial cobrando resto, marcos sem E/S ou hora e células sem `missing`. A fábrica exige adicionalmente marco vencido, data igual à célula e posição/tipo mapeados. A contribuição da véspera não passa por `f[0] == dia`. (`app/chamados/services/declaracao_texto.py:101–143`; `app/chamados/services/fabrica_da_celula.py:68–74`) |
| Dedup anterior à emissão | Remove motivos que já têm linha do fato: colaborador, dia, motivo, instância, `conversa=False`, fora `duplicata/motivo_retratado`. **Não exige vitalidade, disputa aberta ou ausência de validação/materialização.** Se todos foram removidos, não chama `perguntar`; ainda pode tentar desfazer carimbos existentes. (`app/chamados/services/fabrica_da_celula.py:145–157`; `app/chamados/models.py:2746–2755`; `app/chamados/models.py:2393`; `app/chamados/services/fabrica_da_celula.py:207–220`) |
| Limbo entregue | `trabalha is None`, motivos presentes e competência entregue: pauta DP e `[]`. Dedup da pauta considera cabeça do dia, DP e marcador, inclusive feita/cancelada. (`app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/fabrica_da_celula.py:257–285`) |

Esses ramos não têm todos a mesma possibilidade de explicar os 64: dia futuro sai do vigia antes de `sem_porta`; entrega detectada pelo backfill sai como `bang:*` antes da fábrica. Em estado estável, essas condições não explicam um `segue_sem_porta` executado. A participação das demais guardas nos 64 é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:648–651`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:194–197`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:227–233`)

Mesmo recebendo motivos, o gerador ainda pode terminar sem inserir:

| Ramo interno | Condição |
|---|---|
| Catálogo/texto/data | Motivo sem pergunta no catálogo; texto datado sem data e sem fallback da grade; pergunta de hora sem dia do turno: `continue`. A leitura de fallback da grade também captura exceção e devolve `None`. (`app/chamados/services/disputa_emissao.py:75–89`; `app/chamados/services/disputa_emissao.py:314–316`; `app/chamados/services/disputa_emissao.py:346–366`; `app/chamados/services/disputa_emissao.py:425–430`) |
| Dedup genérico | Motivo não datado sem episódio já existente; intervalo sem data ou já existente na disputa/dia; conversa repetida: `continue`. Os quatro motivos normais da fábrica são datados, de hora, e recebem contexto ISO do dia; os ramos não datados/conversa não são seu caminho normal. (`app/chamados/services/disputa_emissao.py:26`; `app/chamados/services/disputa_emissao.py:325–329`; `app/chamados/services/disputa_emissao.py:395–401`; `app/chamados/services/disputa_emissao.py:431–438`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/declaracao_texto.py:42–47`; `app/chamados/catalogo/perguntas.py:48–58`; `app/chamados/catalogo/perguntas.py:124–134`) |
| Dedup com destino não derivado | Para motivos que exigem destino, zero ou múltiplos candidatos devolvem `(None,None)`. O dedup seguinte usa **`data_evento=_data_ev`**, antes de escolher o fallback. Assim, uma pergunta legada da mesma disputa/motivo com data nula pode impedir a emissão de um dia cujo fallback é válido. Ocorrência nos 64: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:238–244`; `app/chamados/services/disputa_emissao.py:410–424`) |
| Reuso na porta final | `PerguntaDisputa.abrir` pode encontrar uma canônica do fato, inclusive em outra disputa, e devolver a existente com `criada=False`. O gerador acrescenta essa pergunta à lista `criadas` mesmo sem inserção; a fábrica devolve motivos, não essa lista. (`app/chamados/models.py:2776–2781`; `app/chamados/models.py:2905–2922`; `app/chamados/models.py:3008`; `app/chamados/services/disputa_emissao.py:444–471`; `app/chamados/services/fabrica_da_celula.py:219–220`) |

Não achei filtro geral de período de apuração em `motivos_a_perguntar` ou `gerar_perguntas_disputa`; procurei os corpos completos dessas funções. Os recortes encontrados são dia/teto, parcial, identidade/dedup e encaminhamento específico do limbo entregue; no reuso há também a porta de rechaveamento. (`app/chamados/services/fabrica_da_celula.py:92–157`; `app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/disputa_emissao.py:300–471`; `app/chamados/models.py:2940–2956`)

Recusa de destino cruzado, fato em outro dia, batida retratada, rechaveamento recusado ou contrato de saída **levanta exceção**; quando escapa, produz `recusa:<tipo>`, não `segue_sem_porta`. (`app/chamados/models.py:2818–2822`; `app/chamados/models.py:2875–2890`; `app/chamados/models.py:2909–2926`; `app/chamados/models.py:2952–2956`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

As exceções engolidas de reatar órfã e de registrar nascimento ocorrem **depois** das inserções. A primeira pode deixar a pergunta órfã, mas não é um ramo de ausência de criação. (`app/chamados/services/disputa_emissao.py:459–470`; `app/chamados/services/disputa_emissao.py:587–603`; `app/chamados/models.py:3014–3031`)

**P3. Escritores, leitores e significado dos insumos.**

A tabela identifica as portas escritoras e os leitores pertinentes aos caminhos acima; não atribui a elas a autoria dos valores concretos da medição, que é **[nao verificado]**.

| Campo/família | Quem escreve ou produz | Quem lê / significado |
|---|---|---|
| `CelulaDia.data`, `trabalha`, DNA/geradora | Geração/regeneração e edição pelas portas da célula. (`app/ponto/portas/celula.py:243–272`; `app/ponto/portas/celula.py:534`; `app/ponto/portas/celula.py:603–604`; `app/ponto/portas/celula.py:816–875`) | Retenção lê por data; tarefa e fábrica usam o vínculo/marcos do dia. `trabalha=None` não equivale a folga no juiz de cobrança. (`app/folha/export.py:1206`; `app/ponto/turnos.py:3049–3062`; `app/ponto/services/cartorio.py:840–859`; `app/chamados/juizes.py:426–441`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `lavrar_veredito`; a ata é produzida por `ata_do_dia`. (`app/ponto/portas/celula.py:974–1040`; `app/ponto/services/cartorio.py:696–738`) | Retenção lê acusação/protestos; C6 lê veredito/via; tarefa e fábrica leem a ata por finalidades distintas. (`app/folha/export.py:1233–1256`; `app/chamados/juizes.py:1338–1387`; `app/chamados/services/tarefa_do_dia.py:84–102`; `app/chamados/services/fabrica_da_celula.py:120–139`) |
| `ata.lampadas`: tipo/hora/acesa/luz | `ata_do_dia` chama o produtor das lâmpadas e as inclui na ata. (`app/ponto/services/cartorio.py:701–713`) | O leitor produz `missing` e `acesa_lavrada`; fábrica usa o primeiro, tarefa usa o segundo. `acesa=None` vira `missing=Fals

[truncado em 20000 de 33148 caracteres]

## 10/11 23:10 · sessao f3099e60799a · - · - · origem=Ronald (tmux)

Matriz complementar P3, somente leitura; escritores abaixo são os encontrados nos caminhos examinados, sem afirmar censo exaustivo de todos os chamadores da árvore. [não verificado]

| Campo/função | Quem escreve/produz | Quem lê | Significado e diferenças |
|---|---|---|---|
| `Ausencia.tipo/data/data_fim/status` | Nascimento em `criar_ausencia`: deriva fim e monta `kwargs`, incluindo `status` explícito ou inicial; instancia e salva a ausência (`app/ponto/services/ausencia.py:197`, `:331`, `:341`, `:385`, `:418`). Edição de tipo/range em `editar_ausencia_registro` (`:465`, `:477`, `:498`, `:509`). Transição de status em `mudar_estado` (`:808`, `:824`, `:835`). | Carga da tarefa chama `_pelo_efeito` + `_q_cobertura` e `_cobertura_pura` (`app/ponto/turnos.py:3064`, `:3072`); tarefa examina status (`app/chamados/services/tarefa_do_dia.py:258`). Vigia consulta cobertura na cadeia de dono (`app/chamados/services/vigia_fio.py:274` e bloco correspondente abaixo, já no agente P1). | Para cobrança, exclui apenas `rejeitada`; para folha, exige `aprovada` (`app/ponto/turnos.py:2511`). Cobertura por data é inclusiva; sem fim, estende só tipos abertos; empate vence maior `(data,pk)` pela ordem e sobrescrita (`:2753`, `:2757`, `:2765`, `:2780`). `aguardando_documento` é bola colaborador; `aguardando_decisao` é DP (`app/ponto/services/ausencia.py:779`, `:791`, `:795`); `ABERTAS` contém esses dois, sem `pendente` (`:765`). |
| `PeriodoFechado` / tranca | `trancar_periodo` cria/retranca e limpa reabertura (`app/ponto/services/fechamento.py:1400`, `:1408`, `:1412`). `reabrir_periodo` grava `ativo=False` e `reaberto_em` (`:1432`, `:1437`). | `periodo_trancado` procura `reaberto_em IS NULL` (`:1464`, `:1470`); `dias_trancados` mapeia dia do turno à competência com empresa e chama esse juiz (`app/chamados/juizes.py:966`, `:1024`, `:1028`); tarefa carrega resultado em lote (`app/chamados/services/tarefa_do_dia.py:365`). | `ativo` é documento; a leitura da tranca usa `reaberto_em` (`app/ponto/services/fechamento.py:1464`). Tranca e exportação são perguntas distintas; a própria descrição separa aprovada/exportada/trancada/paga (`:1455`). |
| `ExportacaoDominio` / entregue | `ExportacaoDominio.registrar` cria histórico por geração (`app/folha/models.py:174`, `:182`). `invalidar` carimba invalidada/substituta (`:143`, `:158`, `:162`). | `competencia_entregue` usa vigente por empresa/mês/ano e recusa vigente invalidada (`app/folha/export.py:556`, `:580`). Composição dia→competência chama `janela_atual(dia, empresa)` (`app/chamados/services/vigia_fio.py:822`, `:838`). Backfill lê antes e novamente sob trava (`app/chamados/management/commands/backfill_fio_morto_e4b.py:194`, `:227`); fábrica lê para limbo (`app/chamados/services/fabrica_da_celula.py:223`, `:230`). | Competência é do dia, não mês civil, e usa corte da empresa (`app/chamados/services/vigia_fio.py:838`; `app/ponto/janelas.py:60`). |
| Lista CxR / `dono_do_dia` / presença colab e dia | `cadastro_realidade.lista` monta dicionários de fontes atuais e lavras, unifica donos em memória e retorna `postos` (`app/escala/services/cadastro_realidade.py:787`, `:800`, `:885`, `:886`, `:926`, `:970`). `um_dono_por_dia` reconsulta o juiz quando assinaturas divergem e atribui `a['dono_do_dia']` em memória (`:985`, `:1017`, `:1024`, `:1028`). Não achei persistência da **lista montada** nesses corpos; suas fontes incluem lavras persistidas (`:793`, `:886`). | `dias_na_lista` extrai somente assinaturas com `data`, resolvendo dono (`:1044`, `:1058`). `ListaDoAdmin` lê lista, guarda mapa de dias e conjunto de colaboradores (`app/chamados/services/vigia_fio.py:205`, `:216`, `:218`); `dia_na_lista` e `colab_na_lista` são leituras separadas (`:230`, `:248`). | Colaborador constar na lista não implica que o dia conste: assinatura de vínculo inteiro não nomeia dia (`app/escala/services/cadastro_realidade.py:1044`). Classe `d` usa precisamente ausência do dia mais presença do colab (`app/chamados/services/vigia_fio.py:677`). |
| Cadastro: `situacao`, `data_demissao`, `data_admissao`, `isento_ponto`, empresa | Porta central declara operações e salva: `gravar`, `salvar_inteiro`, `criar_colaborador` (`app/colaboradores/services/colaborador.py:23`, `:55`, `:73`, `:91`). Desligamento/reativação atribuem situação/data_demissão e salvam pela porta (`app/colaboradores/services/desligamento.py:103`, `:210`). Tela atribui admissão e empresa (`app/colaboradores/views.py:1313`); nascimento recebe admissão e ativo (`:1188`, `:1205`); Passe Livre grava flag pela porta (`:176`). | Leitor da célula lê admissão/demissão e flag (`app/escala/services/leitor_celula.py:358`, `:365`); tarefa usa esse leitor (`app/chamados/services/tarefa_do_dia.py:380`); fábrica exige situação atual ativo (`app/chamados/services/fabrica_da_celula.py:112`). | Leitor considera fora antes da admissão ou após demissão; no dia da demissão, depende de fato lavrado (`app/escala/services/leitor_celula.py:385`, `:389`). Fábrica usa status cadastral atual, sem reconstruir situação no dia (`app/chamados/services/fabrica_da_celula.py:112`). Portanto os dois não aplicam o mesmo predicado de elegibilidade (`app/escala/services/leitor_celula.py:389`; `app/chamados/services/fabrica_da_celula.py:112`). |

Pontos temporais que merecem aparecer na consolidação:

- O backfill separa `agora=timezone.localtime()` de `hoje=--hoje` opcional: vigia recebe `hoje`, tarefa recebe `agora`, e fábrica recebe ambos. Portanto `--hoje` não rebobina o relógio da tarefa/teto; se isso foi usado na medição, [não verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169`, `:176`, `:193`, `:117`.)
- Vigia `censo_alcance` chama `alimentar_portas` sem `agora`; alimentação chama C6 sem `hoje`; `ListaDoAdmin` criada dentro desse censo também não recebe `agora`. A tarefa injeta `agora` na fila e corta decisões por ele. (`app/chamados/services/vigia_fio.py:604`, `:623`; `app/chamados/juizes.py:1497`, `:1504`; `app/chamados/services/tarefa_do_dia.py:337`, `:363`.)
- Janela da competência é fechada nos dois extremos: corte do mês anterior até corte−1 do mês nomeado (`app/ponto/janelas.py:10`); tarefa exige `ini <= dia <= fim` (`app/chamados/services/tarefa_do_dia.py:398`); retenção folha usa `data__range` e laço `<= fim` (`app/folha/export.py:1206`, `:1225`). Não achei borda exclusiva nesses três caminhos examinados; teto de marco é inclusivo em `marco + 30min <= agora` (`app/ponto/services/cartorio.py:828`, `:858`).
- C6 fallback `_dentro_da_janela_viva` chama janelas **sem empresa**, portanto usa corte default 21; a expiração da lavra faz o mesmo. Já entregue e tranca usam empresa. Isso é diferença verificável de parâmetros, sem demonstrar diferença efetiva para as empresas da medição. (`app/chamados/juizes.py:578`, `:597`; `app/chamados/veredito_lavrado.py:143`; `app/ponto/janelas.py:36`, `:50`; `app/chamados/services/vigia_fio.py:838`; `app/chamados/juizes.py:1024`.)
- C6 fallback sem `hoje` usa `datetime.date.today()`; bordas e expiração usam data local Django. Settings-base define São Paulo e `USE_TZ=True`; fuso efetivo do processo/prod e coincidência de datas nesta rodada são [não verificado]. (`app/chamados/juizes.py:577`; `app/chamados/veredito_lavrado.py:95`, `:104`; `app/config/settings/base.py:93`.)
- `CargaDaTarefa` declara mistura explícita de horizonte: decisões/lavra/efeito cortados por `agora`, mas identidade/pergunta/ata/cobertura/status/fila/cadastro/vínculo/tranca atuais. Logo não é fotografia histórica integral. (`app/chamados/services/tarefa_do_dia.py:301`.)

## 10/11 23:09 · sessao 60d5ae8dd978 · - · - · origem=Ronald (tmux)

P1 — rastreamento do código, sem inferir a distribuição interna dos 116 dias:

- O command usa **`tarefa_do_dia`**, chamada por `ato_do_dia`; o rótulo `tarefa_nada` é construído pelo próprio command quando não acumulou atos de sistema/passivo, não acumulou ações com `porta is None` e `t['nao_sei']` é falso. Portanto o rótulo, isoladamente, não prova que `acoes == []`: ações humanas **com porta preenchida** também não entram em `humanos`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–98`)
- A tarefa efetivamente julgada é `_tarefa_pura`, chamada por `CargaDaTarefa.tarefa`, com cadastro, grade, instâncias de marco, cobertura, perguntas, decisões, efeitos, fila, passivo, lavra e tranca. (`app/chamados/services/tarefa_do_dia.py:396–406`)
- O universo das perguntas da tarefa é `PerguntaDisputa(colaborador_id__in=ids, conversa=False, data_evento__gte=ini, data_evento__lte=fim)`, ordenado por colaborador e pk; não exige pergunta viva, disputa aberta ou carimbo não validado. (`app/chamados/services/tarefa_do_dia.py:323–329`)
- Dentro desse universo, só considera motivos de marco; exclui `VIAS_FORA_DO_FATO`, salvo retratação posterior ao `agora`, e exclui última decisão de retratação; associa pelo `data_evento == dia` e pela instância/posição do motivo. (`app/chamados/services/tarefa_do_dia.py:223–234`)
- A decisão considerada é a última linha em ordem `(ato_em, pk)` com `ato_em <= agora`; os atos posteriores só alimentam o conjunto de retratações posteriores. (`app/chamados/services/tarefa_do_dia.py:337–342`)

Predicado da tarefa:

- Retorna nada para `fora_vinculo`, isento ou competência trancada; sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`)
- Em dia de trabalho sem instâncias de lâmpada, retorna nada se existem marcos e nenhum venceu; caso contrário retorna `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`)
- Por instância, não exige ação se `dia_sem_trabalho`, lâmpada `True`, marco ainda não vencido ou ausência aprovada; lâmpada `None` ou marco sem teto produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`)
- Para marco apagado, vencido e não coberto, sem pergunta da identidade exige `PERGUNTAR`; havendo pergunta, julga a última decisão. (`app/chamados/services/tarefa_do_dia.py:268–288`)
- Última validação com lavra vigente `sim` e sem efeito de Batida produz `encerrada_pelo_juiz/[]`; silêncio produz `silencio_decidido/[]`; absorção com ausência aprovada produz `absorvida_aprovada/[]`. (`app/chamados/services/tarefa_do_dia.py:140–154`)
- Última resposta, recusa, reabertura, devolução ou absorção não final gera ação humana; validação sem efeito gera materialização, e efeito sem lâmpada gera rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`)
- `dia_sem_trabalho` lê `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho; ausência parcial com resto cobravel só é calada por `trabalha is False`. (`app/chamados/juizes.py:426–441`)

Predicado do vigia usado pelo backfill:

- O command usa **`censo_alcance`**, selecionando `alcance_dias` cuja rota é `sem_porta`; não usa o `censo` legado para definir seu plano. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`)
- O universo são os dias retornados por `dias_que_seguram` para cada empresa visível e cada competência de `competencias_a_lavrar`, deduplicados por `(colab_id, data)`. (`app/chamados/services/vigia_fio.py:581–588`)
- Empresas visíveis são ativas, excluindo CNPJ iniciado por `11222333`. (`app/colaboradores/queries.py:78–87`)
- Competências são a atual e a anterior se esta não estiver entregue; a atual entra sem teste de entregue nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`)
- `dias_que_seguram` parte de `classificar_export`, retém somente colaboradores com motivo `furo_espelho`, depois obtém seus dias pela mesma `motivos_retencao_celula`. (`app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)
- `classificar_export` lê `FechamentoMensal` da empresa/mês/ano sem filtro de status; sua cadeia pode classificar antes por rescisão, cadastro etc., e só atribui `furo_espelho` quando chega ao teste dos motivos de retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`)
- Retenção usa `CelulaDia` por `data__range=(ini,fim)`, recorta admissão/demissão e exclui isento; retém célula acusadora salvo todos os protestos não cobraveis ou exceção da pausa; também retém cegueira passada, sob as condições do código. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`)
- Acusadores são `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`; o conjunto não cobravel contém `FURO_PRE_ADOCAO`. (`app/escala/models.py:1767–1768`; `app/ponto/supra_juiz.py:34–36`)
- O vigia pergunta ao C17 `porta_alcancavel_do_dia`; havendo porta, sai do morto; dia de hoje/futuro também sai; depois saem os quatro baldes de `dono_fora_do_fio`. O restante incrementa `fio_morto_alcance` e recebe `sem_porta`. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:695–699`)
- Os quatro baldes são Resolver dia oferecido; ausência cobrindo com status em `ABERTAS`; aceite de folga validado; dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:297–315`)
- C17 considera porta um chamado da célula na fila ou pergunta viva da célula, no app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`)
- A fila exige status em `COBRANCA` e exclui silêncio no futuro; `COBRANCA=('aberto','em_analise')`, enquanto `VIVOS` também inclui `registrado`. (`app/chamados/services/painel.py:188–190`; `app/chamados/catalogo/motor.py:24–31`)
- As perguntas do vigia são carregadas **por `celula_id`**, sem `conversa=False` nem filtro de `data_evento`; a seleção de vivas usa C6. (`app/chamados/juizes.py:1502–1505`)
- C6 aplica estado da projeção/carimbos, tranca, vias, resposta, veredito da célula, lâmpada do marco, classificador e materialização; ele não usa a sequência de decisões da tarefa para responder. (`app/chamados/juizes.py:1324–1410`)

Condição exata de divergência:

`morto = dia retido ∧ sem C17 ∧ data < hoje ∧ nenhum balde de dono`; simultaneamente `tarefa_nada = sem ato de sistema/passivo ∧ sem ação com porta None ∧ nao_sei=False`. O primeiro testa retenção agregada e alcançabilidade por célula; o segundo testa exigibilidade por instância/decisão e depois passa pela redução do command. Não existe equivalência imposta entre os predicados. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:235–288`)

Para a **classe d**, além do predicado de morto: existe célula; não existe `veu` de chamado vivo da célula ou âncora viva fora da fila; `ps` vivas está vazio; `dia_na_lista == (False,None)`; o colaborador aparece na lista CxR. Isso não identifica a razão da tarefa dos 116 dias; não achei essa distribuição no dado fornecido. (`app/chamados/services/vigia_fio.py:661–678`; [não verificado quanto à distribuição dos 116])

Juízes:

- C19 está registrado para a pergunta das ações exigíveis e aponta a `tarefa_do_dia`; o command o lê. (`app/core/juizes.py:150–155`; `app/core/juizes.py:291`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`)
- O vigia lê C17, C6 e os baldes de dono; não chama C19 em `censo_alcance`. (`app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:604–610`; `app/chamados/services/vigia_fio.py:635–656`)
- O registro declara que os cinco respondedores anteriores seguem vivos na E2; não prova que todos os leitores do HEAD já migraram. (`app/core/juizes.py:153–154`)
- C17, C6, C15 e C18 também estão registrados. (`app/core/juizes.py:278–291`)

Nuances de significado/relógio para P3:

- A tarefa usa dia do turno em `data_evento`; calcula `data_civil` separadamente a partir do instante local do marco. (`app/chamados/services/tarefa_do_dia.py:213–215`; `app/chamados/services/tarefa_do_dia.py:91–102`)
- A tarefa lê perguntas por colaborador/data do turno; o vigia lê perguntas ligadas à célula. Uma pergunta com chave ou FK divergente pode entrar num universo e não no outro; ocorrência nos dados fornecidos é [não verificado]. (`app/chamados/services/tarefa_do_dia.py:324–329`; `app/chamados/juizes.py:1502–1505`)
- O backfill captura `agora=timezone.localtime()` uma vez; `hoje` pode ser sobrescrito independentemente por `--hoje`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169–170`)
- Esse `agora` chega à tarefa e à fábrica, mas `_alcance` recebe apenas `hoje`; `censo_alcance` chama `alimentar_portas` sem `agora`, C6 sem `hoje`, e cria `ListaDoAdmin` sem `agora`. Logo o vigia não está inteiramente congelado no mesmo instante da tarefa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–69`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:193`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/services/vigia_fio.py:623`; `app/chamados/juizes.py:1504`)
- Sem `agora`, a fila usa `timezone.now()`; a retenção chamada pelo vigia também não recebe `hoje`, usando `timezone.localdate()` internamente. Isso permite diferença temporal sem demonstrá-la nos 116/64 casos. (`app/chamados/services/painel.py:188–190`; `app/folha/fora_da_folha.py:95–99`; `app/folha/export.py:1197`)
- Os intervalos de dias da tarefa e da retenção são inclusivos; os marcos podem cair em outro dia civil. (`app/chamados/services/tarefa_do_dia.py:325–326`; `app/chamados/services/tarefa_do_dia.py:398–399`; `app/folha/export.py:1206`; `app/folha/export.py:1225`; `app/chamados/services/tarefa_do_dia.py:102`)
- O leitor da grade preserva a lâmpada triestado `acesa_lavrada`, enquanto seu campo antigo `missing` trata `acesa=None` como acesa; a tarefa escolhe explicitamente o triestado. (`app/escala/services/leitor_celula.py:119–137`; `app/chamados/services/tarefa_do_dia.py:84–88`)
- O leitor pode declarar `fora_vinculo` no dia da demissão sem fato lavrado, enquanto a retenção ainda retém célula acusadora nesse dia; este é um caminho de divergência permitido pelo código, participação nos 116 dias [não verificado]. (`app/escala/services/leitor_celula.py:385–393`; `app/folha/export.py:1229–1254`)

## 10/11 23:08 · sessao c225cf111a84 · - · - · origem=Ronald (tmux)

P2 — A saída `segue_sem_porta` não demonstra ausência de criação: ela só demonstra que o recenso ainda encontrou a célula na rota `sem_porta`; o command não inspeciona o retorno da fábrica nem conta perguntas novas (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116-117`, `:240`, `:246-249`). Qual ramo ocorreu em cada um dos 64 dias: [não verificado]; o dado fornecido não contém os insumos dessas guardas.

O caminho efetivo é `perguntar_pela_celula` → `motivos_a_perguntar` → `motivos_pela_ata`; com motivos, `perguntar` → `criar_ou_atualizar_disputa` → `gerar_perguntas_disputa`, e `perguntar` chama o gerador novamente quando recebe uma disputa (`app/chamados/services/fabrica_da_celula.py:178`, `:207`, `:217-220`, `:160-175`; `app/chamados/services/disputa_emissao.py:669-688`).

Ramos que terminam sem criar `PerguntaDisputa`:

| Condição | Resultado / referências | Command distingue? |
|---|---|---|
| `cel.veredito == 'nunca_bateu'` | Retorna `[]` imediatamente; nem pauta cadastro (`app/chamados/services/fabrica_da_celula.py:87-89`, `:183-184`). `nunca_bateu` consta em `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`). | Não: retorno descartado (`backfill_fio_morto_e4b.py:116-117`). |
| Passado, colaborador ativo, `not dia_sem_trabalho(cel)`, e teto não encontra marcos vencidos **nem pendentes** | `vencidos_no_instante` devolve `None`; escreve/tenta deduplicar pauta cadastro e retorna `[]` (`fabrica_da_celula.py:45-49`, `:201-205`). Marcos vazios podem vir de vínculo/tipo ausente ou `marcos_turno` vazio (`app/ponto/services/cartorio.py:840-845`, `:855-859`). | Não; o booleano da pauta também é ignorado (`fabrica_da_celula.py:204-205`; command `:117`). |
| Pauta cadastro já existe | O escritor consulta **depois da trava**; retorna `False` sem escrever (`fabrica_da_celula.py:364-368`). O juiz aceita cabeça viva por colaborador ou fechada em data **civil local >= dia** (`:327-335`); não deduplica por dia (`:313-314`, `:327-335`). A fábrica retorna `[]` em ambos os casos (`:204-205`). | Não (`backfill...:117`). |
| Dia sem trabalho, colaborador não ativo, dia futuro | `motivos_a_perguntar` retorna `[]` (`fabrica_da_celula.py:110-115`). | Não (`backfill...:117`). Dia futuro é incompatível com `sem_porta` no mesmo `hoje`, pois o alcance o retira antes (`vigia_fio.py:648-651`). |
| Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM` | Retorna `[]`; veredito vazio **não** cai nessa guarda (`fabrica_da_celula.py:120-121`). | Não (`backfill...:117`). |
| Ausência parcial mandada somente à lista | `parcial_so_lista` retorna verdadeiro para `cobertura_parcial` e `periodo_parcial == 'sem_periodo'`, ou parcial com julgamento anterior e sem fato novo desde ele; a fábrica usa `cel.ata`, `cel.julgada_em` por default e `agora` (`app/ponto/services/cartorio.py:395-400`; `fabrica_da_celula.py:123-125`). O command não passa `julgada_antes`, portanto vale esse default (`backfill...:117`; `fabrica...:178`, `:123-124`). | Não (`backfill...:117`). |
| Nenhum marco vencido | `motivos_pela_ata` recebe `None` ou conjunto vazio e retorna `[]`; no passado sem turno o ramo anterior já direcionou a cadastro (`fabrica_da_celula.py:58-61`, `:201-205`). Há turno ainda pendente ⇒ conjunto vencido vazio, sem cadastro (`:46-49`). | Não (`backfill...:117`). |
| Leitura da ata falha ou ata não autoriza | `_ata_do_dia` engole exceção, registra e retorna `(True,None)`; embora o comentário diga “falha de LEITURA nao barra”, este chamador **barra** quando `faltantes is None` (`app/chamados/services/disputa_emissao.py:894-901`; `fabrica_da_celula.py:64-66`). Estado `sem_celula` com escala vigente ⇒ `(False,None)`; sem escala ⇒ `(True,None)`; ambos não geram motivos neste caminho (`disputa_emissao.py:902-913`; `fabrica...:65-66`). | Não: exceção engolida vira `[]` (`backfill...:117`). |
| Ata lida, mas nenhum faltante elegível | `_marcos_faltantes` ignora isento/fora vínculo, dia diferente de trabalho sem parcial cobrando resto, marcos sem tipo E/S ou hora, célula não `missing`; sem pares registra `sem_celula` (`app/chamados/services/declaracao_texto.py:101-143`). A fábrica ainda exige `(tipo,hora)` no teto vencido, `f[0] == cel.data`, mapeamento `(posição,tipo)` conhecido e motivo único (`fabrica_da_celula.py:68-74`). A contribuição da véspera de `_marcos_faltantes` não passa por `f[0] == dia` aqui (`declaracao_texto.py:135-142`; `fabrica...:71`). | Não (`backfill...:117`). |
| Todos os motivos já têm linha do fato | Consulta por `disputa__colaborador=colab`, `data_evento=cel.data`, `conversa=False`, exclui duplicata; anota `q_dos_fatos`; remove todo motivo em `ja` (`fabrica_da_celula.py:145-157`). `q_dos_fatos` exige coluna `colaborador_id`, motivo, dia, instância derivada do motivo, `conversa=False`, via fora de `('duplicata','motivo_retratado')` (`app/chamados/models.py:2746-2755`, `:2393`). **Não exige pergunta viva, disputa aberta ou não validada/materializada** (`models.py:2753-2755`). | Não; retorna `[]`, embora possa desfazer carimbo de candidatas existentes (`fabrica...:208-209`, `:220`; `backfill...:117`). |
| Limbo (`trabalha is None`), motivos e competência entregue | Encaminha pauta DP e retorna `[]` (`fabrica_da_celula.py:214-216`, `:228-231`). Pauta deduplica por cabeça ancorada em colab:dia, DP e prefixo, incluindo feita/cancelada (`:257-265`, `:276-285`). | Não (`backfill...:117`). No backfill, entregue/indeterminada já é barrado duas vezes e vira `bang:*`, portanto este ramo não explica por si um `segue_sem_porta` num estado estável (`backfill...:194-197`, `:227-231`). |

O alcance que produz **classe d** não exige “chamado vivo com célula”: a rota vem dos dias que seguram a folha; `d` requer nenhum `ps` vivo, dia fora da lista, colaborador na lista, depois de excluir porta, hoje/futuro e donos por lei, e depois de `veu` vazio (`app/chamados/services/vigia_fio.py:577-587`, `:635-678`). Portanto guardas da fábrica relativas a falta de chamado não contradizem automaticamente a classe d (`vigia_fio.py:663-678`).

Ramos no gerador:

- Ausência de pergunta no catálogo → `continue` (`app/chamados/services/disputa_emissao.py:314-316`). Não corresponde aos quatro motivos normais desta fábrica: o mapa produz somente entrada/saída/intervalos, todos presentes e de tipo `hora` no catálogo (`app/chamados/services/declaracao_texto.py:42-47`; `app/chamados/catalogo/perguntas.py:48-58`, `:124-134`).
- Dedup não datado sem episódio; texto sem data e fallback de grade sem dia; datado de intervalo sem data ou com linha existente; pergunta de hora sem dia; conversa repetida: todos são `continue` explícitos (`disputa_emissao.py:325-329`, `:346-366`, `:398-401`, `:425-438`). Os ramos de não datado/conversa não são o caminho normal dos quatro motivos da fábrica, que entrega contexto `{motivo: {'data': dia.isoformat()}}` e catálogo de hora (`fabrica_da_celula.py:172`; `disputa_emissao.py:26`; `models.py:3657`; `catalogo/perguntas.py:48-58`, `:124-134`).
- **Dedup relevante que não usa a mesma chave do primeiro filtro:** para `saida_sem_entrada`/`orfao_14h`, `_derivar_chamado_destino` com 0 ou 2+ candidatos devolve `(None,None)`; depois o gerador deduplica por `disputa`, `motivo`, **`data_evento=_data_ev`**, `conversa=False`, excluindo vias fora do fato (`disputa_emissao.py:238-244`, `:410-420`). Assim uma linha legada com `data_evento=None` pode impedir a criação mesmo que `_data_fallback` seja o dia válido da célula; sem essa linha, `_dia_p` usa o fallback (`:417-424`). Ocorrência nos 64: [não verificado].
- Intervalos deduplicam por disputa+motivo+dia fallback; a porta final `abrir` também pode reutilizar uma canônica global em outra disputa/estado em vez de criar; o retorno é `(ex,False,...)`, sem exigir vitalidade ou disputa aberta nesse ponto (`disputa_emissao.py:395-401`; `app/chamados/models.py:2905-2922`, `:3008`). O gerador ainda acrescenta a reutilizada a `criadas` sem testar `_criada`, então sua lista tampouco é contagem estrita de inserções (`disputa_emissao.py:444`, `:459`, `:470-471`). A fábrica devolve `ms`, não a lista do gerador (`fabrica_da_celula.py:219-220`).
- Não achei filtro geral de competência/período de apuração em `motivos_a_perguntar` ou `gerar_perguntas_disputa`; procurei o corpo completo das duas funções. Há somente o encaminhamento específico de limbo entregue na fábrica, as guardas de dia/teto/parcial e as recusas de rechaveamento no reuso (`fabrica_da_celula.py:92-157`, `:214-231`; `disputa_emissao.py:300-471`; `models.py:2940-2956`).
- A recusa de rechavear, fato em outro dia, destino cruzado, batida retratada ou contrato de saída levanta exceção, não retorna silenciosamente; o command faz rollback do dia e escreve `recusa:<tipo>`, portanto não são `segue_sem_porta` quando escapam (`models.py:2875-2890`, `:2909-2926`, `:2952-2956`, `:2818-2822`; `backfill_fio_morto_e4b.py:236-239`).
- Exceção engolida de reatar órfã ocorre **depois** da pergunta criada e a mantém órfã; não é ramo “sem criar”, embora possa deixar a porta inalcançável (`disputa_emissao.py:459-470`). Logs de nascimento de disputa/pergunta também têm `except`, mas após as inserções, portanto não explicam ausência de criação (`disputa_emissao.py:587-603`; `models.py:3014-3031`).

Para P3, os pontos de significado mais úteis são: instância derivada de posição da ata (`declaracao_texto.py:42-57`, `:75-83`); o leitor de ata converte `acesa=None` em `missing=False`, mas preserva `acesa_lavrada=None` para tarefa (`app/escala/services/leitor_celula.py:119-137`); fábrica exige `missing`, tarefa pode ler o triestado. Teto usa instante `quando + 30min <= agora`, com marco ancorado no dia do turno e horário formatado no fuso local (`app/ponto/services/cartorio.py:828`, `:831-859`). Pergunta datada usa dia do turno; conversa grava `localdate()` civil e não participa do fato (`app/chamados/models.py:2776-2803`). A pauta de cadastro fechada usa data civil local inclusiva (`fabrica_da_celula.py:331-335`).

## 10/11 22:53 · sessao 0902fbce9ab3 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Aplicadas as correções nos três testes: imports/variável mortos removidos e capturas amarradas por defaults. Arquivos tocados em [entrega:5](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:5). [nao verificado]

Todos os B023 têm chamada dentro da mesma iteração:

| Fecho | Variáveis |
|---|---|
| 5c:126 | `filtro`, `a` |
| 5f:87 | `filtro`, `a` |
| 5f:147 | `filtro`, `a` |
| 5f:149 | `ps`, `dona`, `outro_dia`, `criada`, `outra` |

Prova por ocorrência e rastreamento em [entrega:11](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:11).

Os 14 `--mutante` completos pararam no controle: 5c com 3 erros; 5f com 1 falha e 3 erros, iguais à base. Cinco mutantes de 5f deram controle verde + RED limpo usando `--item`. Comandos e saídas completos em [entrega:166](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:166) e [entrega:1316](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:1316). [nao verificado]

**Ruff zero e GREEN+RED integral: [não verificado].** Ruff está ausente; os controles encontram um módulo double ausente. Limites registrados em [entrega:33](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:33).

## 10/11 22:42 · sessao 4744e2951238 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura aplicada: contador novo `I23`, corte histórico do `I8` restaurado, chave HOJE atualizada e cinco achados de lint tratados. Os nove arquivos tocados estão na [lista da entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:5). [nao verificado]

RED do ID único observado na base; controle verde e quatro mutantes com uma assertion RED cada. [Resultados e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:29). [nao verificado]

**Ruff zero [nao verificado].** `ruff check --no-cache app/` retornou `ruff: command not found`, rc 127. Django também está ausente; ORM e dois selos [nao verificado]. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:63).

Comandos para a sessão, dentro de `app/`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5h_forma.py --red-base
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5h_forma.py --mutante id_colidido

PYTHONDONTWRITEBYTECODE=1 RUFF_NO_CACHE=1 python3 -B manage.py test core.tests.test_contract_esmeril escala.tests.test_semselo5_ativo_nao_responde_por_dia --settings=config.settings_test --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_bo_disputa_volta5g --settings=config.settings_test --noinput
```

A memória 5g passou nos dez casos; 5b/5c apresentaram os mesmos erros de importação reproduzidos na base. [Detalhes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:51). [nao verificado]

## 10/11 22:34 · sessao e0e7d447ad36 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura implementada nos seis arquivos listados na [entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:5): emissor, serviço de trilha, vigia, testes em memória, testes ORM e registro da entrega. [nao verificado]

A [escrita única via `core.observ.evento`](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/descarte_disputa.py:31) ocorre após commit, com deduplicação por **dona + motivo + pergunta_id**. Os sítios de encaminhamento e a sobrevivência aos rollbacks estão [documentados por caminho](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:13). [nao verificado]

Resultados observados e comandos de cada RED/mutante:

- **10 testes em memória passando; três REDs contra a base; oito mutantes mortos por assertions**, sem aceitar erro de montagem. [Comandos e resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:66). [nao verificado]
- **I8 lê somente a trilha**; o I8 histórico permanece, com descrição distinta. [Vigia](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/management/commands/auditar_invariantes_chamados.py:77).
- **Seis casos ORM entregues, sem execução pelo Codex**. ORM, regressão completa e certificação final: **[nao verificado]**. [Comando para o Code](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:100).

[perguntas de lei: 0](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:109)

## 10/11 22:23 · sessao be870e2c21a5 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura implementada em [disputa_emissao.py:502](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/disputa_emissao.py:502): episódio explícito no replay, motivos locais sem seed estrangeiro, análise condicionada ao replay confirmado, âncora pelo agregador e declaração pública com pergunta obrigatória (`:724`, `:1279`, `:1474`, `:1577`).

Arquivos tocados, além do emissor:

- `chamados/tests/test_bo_disputa_volta5f_memoria.py:80` — REDs e mutantes novos.
- `chamados/tests/test_bo_disputa_volta5_memoria.py:68` — montagem única e execução do alvo.
- `chamados/tests/test_bo_disputa_volta3_memoria.py:234` — repetibilidade, sem alegar comparação histórica.
- `chamados/tests/test_bo_disputa_volta5b_orfaos.py:126` — RED ORM do lote misto.
- `chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:183` — expectativa sem seed indevido.
- `chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:400`
- `chamados/tests/test_bo_disputa_sem_pergunta.py:70`
- `chamados/tests/test_porta_disputa_emissao.py:86`
- `chamados/tests/test_sla_pelo_juiz.py:133`

Os oito testes novos passaram em memória; cada mutante abaixo produziu falha por assertion no alvo. Referências: `chamados/tests/test_bo_disputa_volta5f_memoria.py:80`, `:101`, `:111`, `:135`, `:165`, `:181`, `:187`, `:204`.

Comandos dos REDs com seus mutantes; cada comando exige primeiro controle verde: [nao verificado]

```bash
red5f() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5f_memoria.py "$@"
}

# F1
red5f --item F1 --mutante seed_local
red5f --item F1 --mutante ausencia_admite_motivo
# F2
red5f --item F2 --mutante episodio_da_dona
# F3
red5f --item F3 --mutante analisa_divergente
# F4
red5f --item F4 --mutante ancora_save_proprio
# F5
red5f --item F5 --mutante publica_sem_pergunta
# F6
red5f --item F6
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_insere
# F7
red5f --item F7 --mutante alega_paridade
```

F1 ORM escrito em `chamados/tests/test_bo_disputa_volta5b_orfaos.py:126`. Comando reservado ao Code; execução ORM/banco: **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_bo_disputa_volta5b_orfaos.OrfaosVolta5bTest.test_RED_F1_lote_misto_local_vazia_nao_ganha_seed_estrangeiro
```

Certificação pelo Code e pelo opus final: **[nao verificado]**.

perguntas de lei: 0

## 10/11 22:12 · sessao ef42db15fa9d · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não há base para liberar: permanecem duplicação de motivo no repasse, replay com insumo implícito variável e selos de mutação quebrados.** Referências: [disputa_emissao.py:1275](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/services/disputa_emissao.py:1275), [models.py:2866](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/models.py:2866), [test_bo_disputa_volta5_memoria.py:81](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/tests/test_bo_disputa_volta5_memoria.py:81).

1. **Estado dos achados anteriores.**

   `opus5d.txt`, `preverif3.r`, `matriz2.r` e os três briefs não estão no material textual efetivamente recebido nem foram encontrados na cópia. Não consigo enumerar seus achados e atribuir honestamente “curado/parcial/igual” a cada um. Conteúdo e confronto individual: **[nao verificado]**.

   Quanto aos problemas identificáveis no pedido e nos patches:

   | Problema | Estado atual verificável |
   |---|---|
   | Exceção do gerador deixar nascimento incompleto | **Curado no núcleo:** criação do pai, DS e perguntas ficam sob atomic; a exceção propagada desfaz o conjunto. `app/chamados/services/disputa_emissao.py:623`, `:653`, `:670`, `:674`. |
   | Pergunta estrangeira deixar nova DS sem pergunta | **Parcial:** núcleo e declaração desfazem o ensaio; órfãos também desfazem quando só houve estrangeiras. Persistem os problemas de motivos e replay descritos abaixo. `app/chamados/services/disputa_emissao.py:681`, `:774`, `:1444`. |
   | Dois escritores de agregação | **Parcial:** o helper reúne as escritas alteradas, mas a âncora conserva união e save próprios. `app/chamados/services/disputa_emissao.py:558`, `:1531`, `:1562`. |
   | Luz HX sem leitor | **Curado no instrumento:** I7a conta o defeito estrutural; I7b publica a abstinência por falta de ata como INFO. Medição atual de produção: **[nao verificado]**. `app/chamados/management/commands/auditar_invariantes_chamados.py:57`, `:73`, `:82`. |
   | Dona fechada | **Pergunta de lei aberta no reuso genérico:** o agregador não a salva; a ressuscitação pelo cartório continua podendo reabri-la pela lei existente. `app/chamados/services/disputa_emissao.py:563`; `app/chamados/models.py:1574`. |

2. **Retornos, dona dos motivos e nascimento sem pergunta.**

   Há seis chamadas datadas a `PerguntaDisputa.abrir`. Cinco consomem pergunta e indicador de criação; a primeira tentativa da declaração descarta `criada` em `_`, embora confira a dona da pergunta. Portanto, literalmente, **não são todos os retornos `(p, criada)` integralmente lidos**. `app/chamados/services/disputa_emissao.py:502`, `:710`, `:773`, `:781`, `:990`, `:1453`.

   No núcleo, os motivos admitidos em A vêm das perguntas locais; os estrangeiros são encaminhados a B. Se A já possui pergunta local do mesmo motivo em outro dia, o motivo pode legitimamente aparecer nas duas listas: o próprio teste de ciclo exige esse estado. `app/chamados/services/disputa_emissao.py:598`, `:638`, `:648`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:36`.

   **Existe também duplicação indevida no repasse:** A aberta com `motivos=[]` recebe o seed `orfao_14h`. Num lote com intervalo local e pergunta `orfao_14h` estrangeira em B aberta, o helper retorna `motivos_antes`, que já contém o seed. A conserva `orfao_14h` sem possuir essa pergunta, B recebe o motivo, e a pergunta local de intervalo impede o rollback. Isso independe da pergunta de lei sobre dona fechada. `app/chamados/services/disputa_emissao.py:1274`, `:1275`, `:980`, `:1002`, `:1007`, `:1335`, `:1349`, `:1429`, `:1444`.

   Para dona aberta, os caminhos regulares conferidos encaminham o motivo; não demonstram exclusividade global nem removem cópias preexistentes. O contador também não detecta o caso acima, porque exige DS sem **nenhuma** pergunta. `app/chamados/services/disputa_emissao.py:566`, `:613`; `app/chamados/management/commands/auditar_invariantes_chamados.py:62`.

   Nos dois consumidores produtivos da declaração, os argumentos da pergunta agora entram no helper transacional. A estrangeira desfaz a agregação/nascimento local e repete a porta na dona. `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/services/disputa_emissao.py:763`, `:777`, `:781`.

   Entretanto, a API pública ainda aceita `pergunta=None` e pode gravar DS aberta sem pergunta fora de HX/h2. Não encontrei consumidor produtivo externo dessa forma; ela é usada internamente e expressamente exercitada pelos testes. `app/chamados/services/disputa_emissao.py:747`, `:750`, `:754`, `:764`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:397`.

   O h2 só estrangeiro agora desfaz o nascimento. Com ata completa e nenhuma estrangeira, permanece o nascimento seguido de fechamento vazio; sem ata, permanece aberta e acende a luz. `app/chamados/services/disputa_emissao.py:1444`, `:1474`, `:1477`; `app/chamados/models.py:1903`.

3. **Push: momento, disputa e motivos.** [nao verificado]

   | Caminho | Comportamento atual |
   |---|---|
   | Nascimento pelo wrapper | Envia para a DS resultante, com motivos admitidos; exige que continue aberta. `app/chamados/services/disputa_emissao.py:699`, `:827`, `:838`. |
   | Agregação pelo wrapper | Envia os novos motivos de A; motivo repetido com pergunta de outro dia não notifica. `app/chamados/services/disputa_emissao.py:647`, `:832`, `:838`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:252`. |
   | Encaminhamento para B | **Não transporta os novos motivos de B para o push:** o retorno do agregador é usado apenas para verificar dona aberta. `app/chamados/services/disputa_emissao.py:565`, `:613`, `:817`, `:832`. |
   | Órfãos, principal | Envia `motivos_atualizados`, inclusive na repetição do mesmo ato e no caso HX sem pergunta. `app/chamados/services/disputa_emissao.py:1484`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:154`, `:162`. |
   | Complementos antes do filtro | Agrupa motivos por dona; exclui a principal para evitar repetir sua notificação nesse coletor. `app/chamados/services/disputa_emissao.py:1226`, `:1257`, `:1465`. |
   | Declaração | Não há envio direto nesse helper. `app/chamados/services/disputa_emissao.py:758`. |
   | Âncora | Envia `ancora_ausente` para a local após analisar o chamado. `app/chamados/services/disputa_emissao.py:1575`, `:1577`. |

   Assim, o wrapper mantém nascimento=admitidos e agregação=novos **da local**; órfãos mantém `motivos_atualizados`. A admissão nova na dona estrangeira não recebe tratamento equivalente de notificação. Aprovação específica dessa omissão e comparação histórica executável com `6c0517eb9`: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:613`, `:832`, `:1002`, `:1486`.

   Não encontrei envio direto da principal antes do rollback dos ensaios estrangeiros. O repasse desfeito retorna antes do push principal; os complementos notificados nesse ramo são anteriores ao savepoint interno. `app/chamados/services/disputa_emissao.py:1278`, `:1447`, `:1461`, `:1462`.

   Isso **não prova ausência de push por cascata**: o cartório é síncrono e há emissores indiretos. Cenário completo que comprove push escapando especificamente desse rollback: **[nao verificado]**. `app/chamados/signals.py:188`, `:216`; `app/ponto/management/commands/detectar_ausencias.py:778`, `:802`.

   O envio é imediato, sem `on_commit`. Portanto, uma falha depois da notificação dos complementos pode desfazer o atomic externo após o envio. Ocorrência concreta: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1230`, `:1465`, `:1473`, `:1963`, `:1971`.

4. **Savepoints e replay.**

   Núcleo, órfãos e declaração repetem a porta com os argumentos conservados, trocando a disputa pela dona. As guardas verificam criação ou PK diferente; núcleo e órfãos desfazem o replay divergente, enquanto declaração levanta `ValueError`. `app/chamados/services/disputa_emissao.py:710`, `:711`, `:781`, `:782`, `:1453`, `:1454`.

   **Argumentos iguais não significam todos os insumos iguais:** no replay genérico BATIDA, o coletor não conserva `batida_origem_id`; `_nascer` volta a derivá-la do contexto da disputa recebida. O contexto da dona pode apontar outro episódio e alterar a identidade/dia procurados. Reprodução ORM desse caso: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:500`, `:504`; `app/chamados/models.py:2866`, `:2875`, `:3463`.

   Esse risco de episódio não se aplica da mesma forma aos motivos MARCO usados na declaração/repasse: o derivador retorna `None` para essa família. `app/chamados/models.py:3461`; `app/chamados/catalogo/motivos.py:34`, `:48`.

   **Órfãos continua analisando todos os chamados mesmo se o replay divergir.** A análise não está condicionada ao sucesso do replay correspondente; pode chamar o cartório depois de a costura ter sido desfeita. `app/chamados/services/disputa_emissao.py:1454`, `:1456`, `:1459`; `app/chamados/signals.py:216`.

   Os signals já executaram antes de a porta devolver o resultado: o callback de beco é registrado com `on_commit`, e o outbox síncrono escreve no banco. Esses mecanismos não demonstram, por si, vazamento externo; os testes de memória não certificam o comportamento transacional real desses receivers. `app/chamados/models.py:3006`, `:3017`; `app/chamados/signals.py:238`, `:257`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:50`.

5. **Agregador, segundo predicado e censo AST.**

   `_agregar_motivos_na_dona` é o ponto comum das agregações modificadas, mas **não é o único escritor de motivos/contexto do emissor**. A âncora conserva outro predicado de união, criação própria e atribuição/save direto de motivos. `app/chamados/services/disputa_emissao.py:558`, `:1531`, `:1554`, `:1562`.

   `ATOS_EMISSAO` corresponde ao censo executado pelo gate, mas declara explicitamente duas escritas da âncora. Logo, gate verde não demonstra agregador único. `app/chamados/tests/test_porta_disputa_emissao.py:83`, `:105`, `:118`, `:128`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:283`.

6. **Ordem C → D → Ch → P.**

   No núcleo, as donas são consulta lazy, avaliada depois de C; as travas externas permanecem durante o rollback interno e o replay. `app/chamados/services/disputa_emissao.py:623`, `:625`, `:627`, `:653`, `:701`; `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`.

   No repasse, o fecho é travado antes das escritas, relido sob C e recusado se crescer; ensaio, replay e análise permanecem no atomic externo. `app/chamados/services/disputa_emissao.py:1156`, `:1167`, `:1173`, `:1178`, `:1215`.

   Nos dois consumidores da declaração, o atomic externo e `_trava_do_ato` antecedem o novo savepoint; a pré-trava inclui donas abertas/fechadas, chamados e perguntas do fato. `app/chamados/services/declaracao_texto.py:299`, `:301`, `:348`, `:352`, `:643`, `:645`.

   **A garantia não cobre autonomamente pendentes do wrapper:** ele chama `perguntar_entradas_pendentes` depois de sair do atomic do núcleo. O helper abre P e depois salva DS, sem atomic/trava próprios. Sem transação externa, P pode estar commitada antes da agregação de motivos/contexto; com transação externa, as travas anteriores permanecem. Não é prova de inversão simultânea de locks, mas é uma fronteira sem a atomicidade anunciada. `app/chamados/services/disputa_emissao.py:650`, `:830`, `:310`, `:313`, `:575`; `app/chamados/models.py:2819`.

7. **Testes e mordida.** [nao verificado]

   | Família | O que verifica / o que não prova |
   |---|---|
   | `sem_pergunta` | Rollback, preservação da batida e I7a/I7b. O ORM “depois da pergunta” não afirma que P nasceu antes de levantar; a irmã memória possui essa testemunha positiva. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:58`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:301`. |
   | Volta2 | Consulta depois de C, dona fechada antes de Ch e trava repetida; mede doubles, não concorrência PostgreSQL. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:143`, `:150`. |
   | Volta3/4 | Dias, ciclos e replay conservado. A “paridade BASE versus atual” executa a fonte atual duas vezes: prova repetibilidade, não paridade histórica. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:300`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:20`, `:45`. |
   | Volta5 | Possui cenário adversarial de replay criado/PK diferente no núcleo. Porém `nasce_sem_pergunta` encontra cinco sítios e `replay_insere` encontra dois; ambos exigem um e abortam na montagem. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:66`, `:73`, `:77`, `:81`, `:194`. |
   | 5b | Reuso, critérios de push, HX e h2; a memória simula `analisar` só como mudança de status, sem cartório. `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:120`, `:152`, `:185`, `:258`. |
   | 5c | Exercita ambos os consumidores da declaração e agregação estrangeira. Não cobre candidata/costura real, pois candidata sempre retorna `None`, trava é Mock e a porta é double. O lote misto usa `intervalo_saida`, deixando passar o seed `orfao_14h`. `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:77`, `:89`, `:95`, `:209`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:22`. |
   | 5e | Demonstra que os asserts rejeitam dona fechada e ausência de nota. Chama a reabertura diretamente, com save/refresh simulados, e muta asserts do teste; não prova a integração cartório→ressuscitação. `app/chamados/tests/test_bo_disputa_volta5e_memoria.py:30`, `:49`, `:59`, `:72`, `:84`. |
   | Ajuste E1 | O mutante desliga `_trava_do_repasse` no agrupador real; não certifica todos os novos replays. `app/chamados/tests/test_e1_fila_v2_2_integracao.py:211`. |
   | Reuso 746 | Agora usa motivo com insumo de pergunta disponível; conserva asserts de reuso do pai/nota, mas deixa de exercitar o antigo caso `orfao_14h` sem insumo. `app/chamados/tests/test_3b_reuso_746.py:23`, `:28`, `:31`. |

   Os asserts protegidos estão presentes nos endereços indicados. Permanecerem **historicamente intocados** não pode ser certificado sem a base comparável. `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`. Integridade histórica: **[nao verificado]**.

   Os 683 testes ORM são informação recebida, não certificação desta leitura. Suíte inteira verde: **[nao verificado]**. [nao verificado]

8. **Quem escreve, quem lê e semântica.**

   | Campo | Escritores e leitores confirmados |
   |---|---|
   | `DS.motivos` | Escrevem agregador, nascimentos, âncora, aparo/remoção e reabertura administrativa. Leem admissão, push e resolução de motivos. `app/chamados/services/disputa_emissao.py:558`, `:670`, `:1299`, `:1562`; `app/chamados/models.py:1966`, `:2063`, `:2159`; `app/api/foto_recebida.py:63`. |
   | `DS.contexto` | Escrevem agregador/nascimentos e portas de contexto/aparo/remoção. Leem resolvedor de pergunta, derivador de episódio e auditor. `app/chamados/services/disputa_emissao.py:572`, `:670`; `app/chamados/models.py:1967`, `:2064`, `:2100`, `:2118`, `:3463`; `app/chamados/management/commands/auditar_invariantes_chamados.py:31`. |
   | `DS.fechada_em` | Escrevem fechamento e reaberturas admin/sistema. Leem eleição, agregador, push, receivers e app. `app/chamados/models.py:1932`, `:2149`, `:2173`; `app/chamados/services/disputa_emissao.py:563`, `:630`, `:827`; `app/chamados/signals.py:28`; `app/api/views.py:1059`. |
   | `P.disputa` | Nasce no construtor; reuso retorna a canônica preservando a FK. Leem roteamento do motivo, autorização e fallback de listagem. `app/chamados/models.py:3008`, `:3014`; `app/chamados/services/disputa_emissao.py:606`; `app/chamados/services/fio.py:598`; `app/api/views.py:1063`. |
   | `P.chamado_destino` | Escrevem nascimento, reuso e costura; leem juiz da resolvedora e seleção do card. `app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; `app/api/views.py:1061`. |

   Há tensão semântica verificável: `motivos` é documentado como cenários ativos; o núcleo admite pelo vínculo com pergunta local, enquanto órfãos conserva o seed sem essa pergunta. Além disso, contexto da DS é usado para derivar identidade BATIDA, embora seja atualizado ao longo dos atos. `app/chamados/models.py:1741`, `:3455`; `app/chamados/services/disputa_emissao.py:598`, `:1275`.

   Esse mapa não certifica censo exaustivo de todos os leitores nem ausência de outras premissas divergentes. **ESMERIL-SEMANTICA completo: [nao verificado].**

9. **Classes de reprovação e proibidos.**

   Permanecem segundo escritor/predicado na âncora, insumo de identidade rederivado no replay, selo sem mordida em dois mutantes e fronteira transacional incompleta nos pendentes. `app/chamados/services/disputa_emissao.py:1531`, `:1562`, `:500`, `:830`; `app/chamados/models.py:2866`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`.

   O cartório continua sendo chamado pela análise; a ressuscitação continua usando a lei HX existente. A falta de ata continua impedindo fechamento e mantendo o aviso, agora acompanhado do contador. `app/chamados/signals.py:216`; `app/chamados/models.py:1574`; `app/chamados/services/disputa_emissao.py:1474`; `app/chamados/management/commands/auditar_invariantes_chamados.py:74`.

   Alteração das sete disputas existentes, execução em produção, identidade criptográfica da cópia, aprovação de critérios adicionais pelos briefs ausentes e ausência de qualquer outra lei nova: **[nao verificado]**.

**NAO LIBERA. Achados que permanecem:**

- Motivo `orfao_14h` pode ficar indevidamente nas duas disputas abertas no lote misto. `app/chamados/services/disputa_emissao.py:1275`, `:1007`, `:1429`.
- Replay genérico conserva kwargs, mas rederiva episódio pela dona; órfãos ainda analisa chamados após replay divergente. `app/chamados/services/disputa_emissao.py:500`, `:1459`; `app/chamados/models.py:2866`.
- Dois mutantes abortam por montagem, e a suposta comparação com a base compara a versão atual consigo mesma. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:245`.
- Agregador único não está estabelecido; pendentes podem agregar fora do atomic do núcleo; novas admissões na dona estrangeira não chegam ao coletor de push. `app/chamados/services/disputa_emissao.py:1562`, `:830`, `:613`.
- Não verificados: achados individualizados dos pareceres ausentes, histórico dos protegidos, reprodução ORM dos novos contraexemplos, efeitos externos completos das cascatas e suíte inteira. **[nao verificado]**. [nao verificado]

## 10/11 22:10 · sessao 093c3c32dff7 · - · - · origem=Ronald (tmux)

Travas e efeitos:

- Núcleo genérico prende C antes de avaliar `donas`, depois D e Ch; conserva essas travas fora do savepoint desfeito e durante o replay. A implementação de `trava_na_ordem` adquire C/D/Ch/P por essa sequência. `app/chamados/services/disputa_emissao.py:623`, `:652`, `:701`; `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`.
- Órfãos adquire o fecho antes de escrever; relê sob C e recusa crescimento. O atomic externo engloba ensaio, replay e análise posterior. `app/chamados/services/disputa_emissao.py:1156`, `:1166`, `:1173`, `:1175`, `:1216`, `:1223`, `:1450`.
- Declaração pelos dois callers conserva atomic externo e pré-trava do fato antes do novo savepoint. `_trava_do_ato` inclui donas abertas/fechadas, chamados e perguntas do fato. `app/chamados/services/declaracao_texto.py:288`, `:299`, `:301`, `:348`, `:352`, `:643`, `:645`; `app/chamados/services/disputa_emissao.py:763`.
- **Lacuna em pendentes da varredura:** o wrapper termina o atomic do núcleo antes de chamar `perguntar_entradas_pendentes`; esse helper agora agrega/salva DS depois da porta de P, sem atomic/trava próprios. Sem atomic externo, a pergunta pode estar commitada antes da escrita de motivos/contexto, e a DS é salva sem conservar C/D. Com atomic externo, as travas do núcleo continuam cobrindo DS do colaborador. `app/chamados/services/disputa_emissao.py:623`, `:650`, `:817`, `:830`, `:310`, `:313`, `:575`. API chama diretamente: `app/api/views.py:794`; cron: `app/ponto/management/commands/marcar_foto_ausente_retro.py:94`.

Replay:

- Os kwargs conservam dia/texto/destino/opções, mas **não congelam todo o fato na família BATIDA**: o coletor omite `batida_origem_id`, e `_nascer` deriva novamente o episódio do contexto da disputa recebida. Trocar a local pela dona pode mudar episódio e, pelo juiz, dia/canônica. `app/chamados/services/disputa_emissao.py:500`, `:503`, `:710`; `app/chamados/models.py:2866`, `:2875`, `:3452`, `:3463`.
- `criada=True` ou pk diferente é desfeito no núcleo/órfãos e levanta na declaração. Antes dessa guarda, insert/save e seus signals já executaram. `app/chamados/services/disputa_emissao.py:710`, `:711`, `:1453`, `:1454`, `:781`, `:782`; `app/chamados/models.py:3006`, `:3017`.
- Os motivos usados nos caminhos de declaração/órfãos são MARCO; o derivador de episódio devolve `None` para essa família. O risco de rederivação de episódio acima se aplica ao replay genérico BATIDA. `app/chamados/catalogo/motivos.py:34`, `:35`, `:48`, `:49`; `app/chamados/models.py:3461`.

Rollback e push: [nao verificado]

- Signals de P agendam beco com `on_commit` e escrevem outbox síncrono no banco. Não são, por si, envio externo de push. `app/chamados/signals.py:227`, `:238`, `:250`, `:257`.
- `_enviar_push_disputa_colab` é envio imediato, webpush e FCM; não usa `on_commit`. `app/chamados/services/disputa_emissao.py:1947`, `:1963`, `:1971`; `app/chamados/push_utils.py:40`; `app/chamados/fcm_utils.py:85`.
- Nos blocos explicitamente desfeitos por estrangeira, não encontrei chamada direta de `_enviar_push_disputa_colab` antes de `set_rollback`. Núcleo envia no wrapper; órfãos envia após sair do savepoint; declaração não envia diretamente. `app/chamados/services/disputa_emissao.py:681`, `:838`, `:1444`, `:1461`, `:1465`, `:1486`, `:758`.
- Há cascata síncrona real: `Chamado.abrir` → `get_or_create` → signal do cartório → possível emissor de ausência → push de supervisão e repasse. `app/chamados/models.py:971`; `app/chamados/signals.py:188`, `:214`, `:216`; `app/ponto/services/cartorio.py:1751`, `:1789`; `app/ponto/management/commands/detectar_ausencias.py:778`, `:802`. **Não reproduzi cenário completo admissível que prove push escapando especificamente do rollback por estrangeira [nao verificado].**
- Push de complementos ocorre ainda dentro do atomic externo e antes de `pode_fechar`; falha posterior pode desfazer banco após esse envio imediato. Não há teste/reprodução desse caso nesta auditoria. `app/chamados/services/disputa_emissao.py:1216`, `:1230`, `:1465`, `:1473` [nao verificado quanto à ocorrência].

Escritores/leitores e semântica:

| Campo | Escritores identificados | Leitores relevantes |
|---|---|---|
| DS.motivos | agregador; creates do núcleo/órfãos; **âncora mantém união/save próprios**; portas de remoção/aparo/reabertura admin | admissão/push; foto recebida; fechamento/materialização/listagem | [nao verificado]
| DS.contexto | agregador; creates; portas de anotar/renovar/aparar/remover | resolução dia/episódio, auditor I7a/b, derivação de destino |
| DS.fechada_em | `fechar`, `reabrir_admin`, `reabrir_sistema` | eleição da aberta, guardas agregador/push, três receivers de fechamento | [nao verificado]
| P.disputa | construtor `_nascer`; reuso devolve `ex` preservando FK | dona do agregado; eleição local/estrangeira; fallback de listagem |
| P.chamado_destino | `_nascer` e `fio.reapontar_resolvedora`; há costura em `ChamadoColaborador` | âncora que lista, juiz de resolvedora, vida do fio, S143 |

Referências da tabela: `app/chamados/services/disputa_emissao.py:558`, `:670`, `:1299`, `:1531`, `:1554`, `:1562`; `app/chamados/models.py:1961`, `:2063`, `:2100`, `:2118`, `:2149`, `:2155`, `:2173`, `:1932`, `:2982`, `:3008`, `:3014`, `:367`; `app/chamados/services/fio.py:925`, `:116`; `app/api/foto_recebida.py:63`; `app/chamados/signals.py:28`, `:129`, `:153`.

**A frase “agregador único” não vale literalmente nem dentro do emissor:** a âncora continua com o segundo predicado de união e o segundo save; o gate declara duas escritas dessa função. `app/chamados/services/disputa_emissao.py:1531`, `:1562`; `app/chamados/tests/test_porta_disputa_emissao.py:83`.

DS.motivos continua documentado como “cenários ativos”, enquanto o núcleo passa a admitir motivos somente com pergunta local; órfãos ainda conserva seed `orfao_14h`, e contexto pode guardar informação de motivo encaminhado para outra dona. Há diferença semântica entre essas populações; impacto completo em todos os leitores **[nao verificado]**. `app/chamados/models.py:1741`; `app/chamados/services/disputa_emissao.py:598`, `:638`, `:633`, `:1275`.

## 10/11 22:09 · sessao 5fbc4ce8791f · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Motivo pode terminar nas duas disputas abertas.** No repasse, `motivos_atualizados` recebe `['orfao_14h']` também quando A já existe com `motivos=[]`; esse seed é salvo antes de perguntar. Se a pergunta `orfao_14h` pertence a B, o helper devolve `motivos_antes`, que já contém o seed, e agrega o motivo em B. Um motivo local de intervalo mantém A com pergunta, impedindo desfazer A. Resultado: A tem `intervalo_saida` e `orfao_14h`, B tem `orfao_14h`, mas a pergunta desse motivo continua só em B. Referências: `app/chamados/services/disputa_emissao.py:1274`, `:1307`, `:980`, `:1002`, `:1007`, `:1313`, `:1349`, `:1429`, `:1444`. Confirmei em sonda inline `python3 -B`, usando as funções produtivas extraídas pelo ambiente de memória; persistência ORM desse cenário [nao verificado].

2. **Replay conserva kwargs, mas não todos os insumos implícitos da identidade.** O gerador conserva motivo/texto/opções/destino/dia, omitindo `batida_origem_id`; a porta deriva essa origem do contexto da DS fornecida. No replay, a DS passa a ser a dona, cujo contexto pode conter outro episódio. Referências: `app/chamados/services/disputa_emissao.py:500`, `:504`, `:710`; `app/chamados/models.py:2866`, `:3452`, `:3463`. O resultado pode divergir: as guardas verificam `criada` ou PK diferente e desfazem o banco: emissor `:711`, órfãos `:1454`, declaração `:782`. Reprodução ORM com família BATIDA [nao verificado].

3. **Replay divergente dos órfãos ainda analisa os chamados.** Depois de desfazer o replay divergente, o agrupador executa toda a lista `analisar`, independentemente de qual replay teve sucesso. Referências: `app/chamados/services/disputa_emissao.py:1454`, `:1456`, `:1459`. A declaração levanta `ValueError`; o núcleo registra `replay-divergente`, portanto as três respostas diferem: `:783`, `:715`. Consequências completas do cartório nessa divergência [nao verificado].

4. **O agregador não é o único escritor em todo o módulo.** A âncora continua criando DS com motivos/contexto e salvando diretamente `disputa.motivos`; também ignora o retorno de `abrir_conversa`. Referências: `app/chamados/services/disputa_emissao.py:1554`, `:1562`, `:1567`. São sítios preservados no diff integral fornecido; atribuir essa duplicidade especificamente ao delta v11 seria [nao verificado].

5. **Pendentes do wrapper ficam fora do atomic próprio do núcleo.** O núcleo retorna dentro de seu atomic; o wrapper chama pendentes depois. O helper abre P pelo atomic da porta, depois salva D no agregador sem atomic próprio. Referências: `app/chamados/services/disputa_emissao.py:623`, `:650`, `:699`, `:830`, `:831`, `:310`, `:313`, `:575`; `app/chamados/models.py:2819`. Garantia de trava externa em todos os consumidores [nao verificado].

Mapa de push: [nao verificado]

- Wrapper: nascimento usa admitidos; agregação usa novos de A; verifica DS aberta e, se pendentes chamou o gerador, verifica novamente. Referências: `app/chamados/services/disputa_emissao.py:699`, `:647`, `:817`, `:827`, `:832`, `:835`.
- Admissões em B não produzem push próprio: `agregar_estrangeiras` descarta a lista de novos do agregador; helper de órfãos também não coleta B para notificação. Referências: `app/chamados/services/disputa_emissao.py:565`, `:576`, `:613`, `:1002`. Se isso viola o aval versus apenas expõe um caso não especificado: [nao verificado].
- Órfãos: principal recebe `motivos_atualizados`; complementadas recebem união `_mot`; evita repetir a principal no coletor. Referências: `app/chamados/services/disputa_emissao.py:1257`, `:1258`, `:1228`, `:1465`, `:1484`.
- Ensaio de órfãos desfeito não chega ao push da principal; só notifica complementos anteriores ao savepoint. Referências: `app/chamados/services/disputa_emissao.py:1278`, `:1447`, `:1461`, `:1462`.
- Declaração não chama push nesse helper; âncora chama uma vez. Referências: `app/chamados/services/disputa_emissao.py:758`, `:785`, `:1577`.
- O envio efetivo é síncrono, Web Push e FCM, sem `on_commit` nessa função. Referências: `app/chamados/services/disputa_emissao.py:1963`, `:1971`. Entrega externa ou duplicidade efetiva [nao verificado].

Retornos:

- Chamadas datadas do gerador, replay do núcleo, helper de órfãos e replay dos órfãos consomem P e criada; a primeira tentativa da declaração usa P e descarta criada. Referências: `app/chamados/services/disputa_emissao.py:502`, `:523`, `:710`, `:990`, `:1008`, `:1453`, `:773`.
- `abrir_conversa` do gerador consome ambos; âncora descarta ambos. Referências: `app/chamados/services/disputa_emissao.py:495`, `:523`, `:1567`.
- Os dois consumidores produtivos externos da declaração passam `pergunta`; a forma sem pergunta aparece internamente e em testes. Referências: `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/services/disputa_emissao.py:764`; `app/chamados/tests/test_porta_disputa_emissao.py:175`.
- API `disputa_para_declaracao(pergunta=None)` ainda pode criar DS sem P; isso é exercitado intencionalmente por teste. Referências: `app/chamados/services/disputa_emissao.py:747`, `:754`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:405`.
- H2 sem estrangeira mantém fecho vazio; com estrangeira e sem P local desfaz o nascimento. Referências: `app/chamados/services/disputa_emissao.py:1444`, `:1473`, `:1479`.

Lacunas de testes pertinentes: [nao verificado]

- O lote misto existente usa estrangeira `intervalo_saida`, não `orfao_14h`, portanto não prova remoção do seed. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:22`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:209`.
- O double de identidade compara apenas motivo e dia, sem episódio; não prova a estabilidade implícita do replay BATIDA. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:109`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`.
- Nenhuma alteração de arquivo foi necessária para as sondas inline; efeitos ORM, PostgreSQL, banco/log de produção e suite inteira [nao verificado]. [nao verificado]

## 10/11 22:08 · sessao cabc2df259bf · - · - · origem=Ronald (tmux)

Achados prioritários:

- **Selo quebrado:** `nasce_sem_pergunta` e `replay_insere` varrem o arquivo inteiro, mas exigem exatamente uma alteração. Hoje encontram respectivamente cinco e dois sítios; abortam por montagem antes de demonstrar assertion RED. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:66`, `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`; sítios em `app/chamados/services/disputa_emissao.py:682`, `:776`, `:778`, `:1446`, `:1448` e `:711`, `:1454`.
- **Paridade sobre a própria versão:** o teste anuncia gerador BASE versus atual, mas instancia `AmbienteVolta3()` nas duas repetições; a comparação final demonstra repetibilidade da fonte atual. As assertions de dia esperado continuam úteis. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:246`, `:289`, `:292`, `:300`.
- **5e testa o assert, não a integração:** chama diretamente `reabrir_sistema`, com `save`/`refresh_from_db` simulados; os mutantes removem assertions do teste. Não certifica que `analisar()`/cartório efetivamente reabre a dona. `app/chamados/tests/test_bo_disputa_volta5e_memoria.py:30`, `:46`, `:49`, `:59`, `:70`, `:74`.
- **Declaração sem cobertura do ramo candidata/costura:** `_candidata_reusavel` sempre retorna `None`, trava é `Mock` e `PerguntaDisputa.abrir` é double. A cobertura não prova `disputa_emissao.py:765-771`, ordem real de travas nem efeitos dos receivers. `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:77`, `:89`, `:95`, `:97`.
- **Divergência nova sem selo específico:** os testes 5c demonstram replay bem sucedido e rollback inicial, mas não forçam pergunta criada/PK diferente no replay de declaração ou órfãos; o teste adversarial existente exerce o replay do núcleo. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:194`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:118`, `:192`; portas `app/chamados/services/disputa_emissao.py:781`, `:1453`.
- **Possível vacuidade no ORM “depois da pergunta”:** chama o gerador e levanta sem afirmar que alguma pergunta nasceu antes. A irmã memória contém essa assertion positiva. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:58`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:301`.

Cobertura útil existente:

| Família | O que morde | Limite |
|---|---|---|
| Sem pergunta | Exceção antes/depois da escrita, preservação da batida, separação I7a/I7b e pergunta morta como linha. `test_bo_disputa_sem_pergunta_memoria.py:286`, `:296`, `:329`, `:355`, `:376` | Atomic/ORM/signals simulados. `test_bo_disputa_sem_pergunta_memoria.py:50` |
| Volta2 | Consulta lazy após C, dona fechada antes de Ch e ausência de trava repetida. `test_bo_disputa_volta2_memoria.py:143` | Conta eventos dos doubles; não SQL/concorrência PostgreSQL. `test_bo_disputa_volta2_memoria.py:1` |
| Volta3/4 | Ordem inicial, dias mistos/indeterminados, ciclos sem reentrada e argumentos conservados. `test_bo_disputa_volta3_memoria.py:156`; `test_bo_disputa_volta4_memoria.py:20`, `:45`, `:106` | Porta de pergunta simulada. `test_bo_disputa_volta4_memoria.py:31`, `:63` |
| Volta5 | Motivo somente na dona, rollback por divergência, idempotência, push e AST sem reentrada. `test_bo_disputa_volta5_memoria.py:121`, `:194`, `:270`, `:334` | Dois mutantes quebrados acima; cenário de dona aberta escondida é artificial e explicitamente declarado. `test_bo_disputa_volta5_memoria.py:233` | [nao verificado]
| 5b | Reuso estrangeiro, push por critério base, sem ata, h2 desfeito, criadas reais/pendentes. `test_bo_disputa_volta5b_orfaos_memoria.py:152`, `:185`, `:223`, `:239`, `:258` | Double de `analisar()` só muda status; não executa cartório. `test_bo_disputa_volta5b_orfaos_memoria.py:120` | [nao verificado]
| 5c | Dois callers da declaração, saídas normais/recusa, rollback, pendentes estrangeiros, agregador e critérios opostos de push. `test_bo_disputa_volta5c_memoria.py:118`, `:149`, `:176`, `:237`, `:252`, `:262` | Limites da declaração acima. | [nao verificado]
| Gate | Censo real por função confrontado em igualdade exata com `ATOS_EMISSAO`. `test_porta_disputa_emissao.py:105`, `:118`, `:128`; reutilizado em `test_bo_disputa_volta5c_memoria.py:283` | Conta sítios/escritores; não prova semântica de motivos/contexto. `test_porta_disputa_emissao.py:122` |
| 5e | Assert rejeita dona fechada mesmo com nota antiga e reabertura sem nota. `test_bo_disputa_volta5e_memoria.py:50`, `:84`, `:88` | Não prova integração. |

Os protegidos contêm os asserts solicitados: sem ata continua sem pergunta/aberta; com ata completa fecha; muda não notifica; pergunta antiga aponta ao dia correto; duas passadas conservam uma pergunta. `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`. Permanecerem **historicamente intocados**: [nao verificado], pois não há `.git`/base disponível.

Executei previamente 82 testes de memória em nove módulos, todos verdes, com `PYTHONDONTWRITEBYTECODE=1 python3 -B`; não executei ORM nem suite inteira. Evidência em arquivo/log da execução: [nao verificado]. Após receber a regra ` .claude/agents/verificador.md:7`, limitei os próximos comandos a leitura/AST; não executei os mutantes.

`opus5d.txt`, `preverif3.r`, `matriz2.r` e briefs não foram encontrados na cópia; conteúdo/achados individualizados: [nao verificado].

## 10/11 22:03 · sessao 38fa1f071ca0 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Arquivos tocados, somente testes: [nao verificado]

- [test_bo_disputa_volta5b_orfaos.py:102](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:102): E1/E2 exigem dona reaberta e nota quando houver chamado; preservam costura, push e ausência de nascimento local. [nao verificado]
- [test_bo_disputa_volta5e_memoria.py:1](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5e_memoria.py:1): prova sem banco, RED e mutantes dos novos asserts.

E3: não encontrei outro ORM com essa exigência depois de `analisar`. As expectativas restantes cobrem emissão/replay ([volta5.py:170](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:170), [sem_pergunta.py:83](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta.py:83)).

Execução local: controle **4/4**, dois RED com **rc 1**, dois mutantes mortos por assertion; comandos do [runner:98](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5e_memoria.py:98):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --red sem_reabertura
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --red sem_nota
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --mutante ignora_reabertura
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --mutante ignora_nota
```

**Motivo não chega à dona reaberta: sim, por leitura do código.** O agregador retorna `None` enquanto ela está fechada ([disputa_emissao.py:562](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:562)), antes de `analisar` ([disputa_emissao.py:1458](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1458)); a reabertura não agrega motivos ([models.py:2173](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2173)).

Execução ORM/cartório e certificação pela suite/opus: [nao verificado]. [nao verificado]

perguntas de lei: 0

## 10/11 21:54 · sessao bae3df84aecf · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

D1/D2: cortei a reentrada de `analisar()` no cartório quando há pergunta ligada a dona fechada. [Emissor:1190](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1190)

D3: âncora única, mantendo `assertRaisesRegex`. [Teste:211](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_e1_fila_v2_2_integracao.py:211) [nao verificado]

Resultados:

- Base original: **3 assertions RED**, zero errors. [Registro:56](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_red_base.txt:56)
- Memória: **85 + 35 testes verdes**. [Execuções:3230](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_execucoes.txt:3230), [medida do turno:49](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_turno.txt:49) [nao verificado]
- **35 mutantes** com controle GREEN e assertion RED, zero errors. [Registro:3230](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_execucoes.txt:3230)

Arquivos tocados, limites e comandos individuais estão no [relato da entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/RELATO_BO_DISPUTA_VOLTA5D.txt:1). ORM/mod11 e certificação Code/opus final: [nao verificado].

perguntas de lei: 0

## 10/11 21:40 · sessao f71fee8a366a · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

C1–C6 implementados. Os arquivos tocados, as duas linhas `lei:` de C7 e os limites estão no [relato](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/RELATO_BO_DISPUTA_VOLTA5C.txt:1).

78 testes em memória e 35 da catraca verdes; 31 mutantes com RED por assertion, sem erros. [Comandos e resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5c_execucoes.txt:3). [nao verificado]

ORM, banco, produção e suite completa: [nao verificado]. Certificação reservada ao Code/suite e opus final. [nao verificado]

perguntas de lei: 0

## 10/11 21:33 · sessao 5c7f63d0e5be · FIO-E0-V4-E4B · censo · origem=Code

**Não identifiquei edição compartilhada obrigatória para a E4b; há dependências de leitura cujo comportamento precisa ser reconferido após BO-DISPUTA**, especialmente a fábrica chamando emissor e gerador. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`)

Identidade Git do HEAD/base e atribuição cronológica dos hunks: **[nao verificado]**. O `diff_obra_v10.patch` não consta do texto recebido nem foi localizado na cópia; a interseção exaustiva contra esse patch é **[nao verificado]**. As referências abaixo são da cópia atual; “compatível” significa que o corpo sustenta a descrição fornecida, sem certificar identidade textual com a base **[nao verificado]**.

**1. CENSO do brief e 26 portas**

| Escritor/leitor do brief | Referência atual | Delta relevante |
|---|---|---|
| Comando novo E4b | **[nao verificado]** | Ainda não há implementação conferível do comando solicitado. |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:3943` | **Brief correto**; censo antigo apontava `:3932`. Faz `bulk_create` e acorda células; não contém deduplicação própria. (`app/chamados/models.py:3953`, `app/chamados/models.py:3954`) |
| `desfazer_carimbo_sem_lastro` | `app/chamados/services/relavra_via.py:187` | Endereço continua válido; porta a chamar. |
| `perguntar_pela_celula` | `app/chamados/services/fabrica_da_celula.py:178` | **`:171` não é mais a definição**. Pode retornar por sem adesão, pauta de cadastro ou limbo exportado; chamada não prova pergunta criada. (`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:215`) |
| Materialização, citada sem função | `app/chamados/services/materializacao.py:1158` | Unidade canônica; ausência em `:462`; Batida delegada em `:927` e `:1007`. |
| Reconciliação, citada sem função | `app/chamados/reconciliador.py:761`; `app/chamados/reconciliador.py:1533` | Reemissão continua em `:761`; **reatar órfã passou de `:1529` para `:1533`**. |
| `censo` | `app/chamados/services/vigia_fio.py:318` | Endereço válido. **`limite=0` não significa ilimitado**: listas só recebem enquanto `len < limite`. (`app/chamados/services/vigia_fio.py:454`, `app/chamados/services/vigia_fio.py:475`) |
| `censo_alcance` | `app/chamados/services/vigia_fio.py:542` | Endereço válido. Todo `sem_porta` entra; demais linhas continuam limitadas. Para inventário integral, usar limite suficientemente grande, não zero. (`app/chamados/services/vigia_fio.py:628`) |
| `classificar`, mencionado como parte do vigia | `app/chamados/services/vigia_carimbo.py:65` | É gerador do universo de **perguntas validadas**, importado pelo vigia; não é o classificador a–e. As classes a–e permanecem em `censo_alcance`. (`app/chamados/services/vigia_fio.py:328`, `app/chamados/services/vigia_fio.py:667`) |
| `vigiar_fio_morto.handle` | `app/chamados/management/commands/vigiar_fio_morto.py:152` | **`:151 → :152`**. Corpo agora lê `contador_fio_mudo`, transportando também `nunca_bateu_sem_adesao` e seus esperados. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| `montar_contexto_painel` | `app/chamados/services/painel.py:198` | Endereço válido; contagens continuam vindo de `particionar_verbos` sobre o universo vivo filtrado. (`app/chamados/services/painel.py:230`) |
| `exige_o_admin` | `app/chamados/services/verbos.py:25` | Endereço válido; soma Validar + Decidir. (`app/chamados/services/verbos.py:22`, `app/chamados/services/verbos.py:27`) |
| Leitor do carimbo | `bin/fio_estado.py:98` | Continua lendo JSON e comparando **todos os esperados transportados**; portanto também alcança os novos contadores, sem edição necessária da E4b. (`bin/fio_estado.py:125`, `bin/fio_estado.py:147`) |

As vinte portas de alteração:

| Porta | Referência do censo → atual | Semântica frente à descrição fornecida |
|---|---|---|
| `_gravar_estado` | `app/chamados/models.py:201 → :201` | Compatível: estado canônico do chamado e consequências. |
| `fechar` | `app/chamados/models.py:1852 → :1855` | Compatível: F; exceções registram `SILENCIO`. (`app/chamados/models.py:1924`) |
| `reabrir_admin` | `app/chamados/models.py:2123 → :2126` | Compatível: F, contador e motivos. |
| `reabrir_sistema` | `app/chamados/models.py:2160 → :2163` | Compatível: limpeza de F. |
| `carimbar` | `app/chamados/models.py:3092 → :3103` | Compatível: C, assinaturas R/V condicionais e L opcional. |
| `carimbar_identidade` | `app/chamados/models.py:3157 → :3168` | Compatível: identidade; preservação opcional de instante/autor. |
| `descarimbar` | `app/chamados/models.py:3192 → :3203` | Compatível: limpeza C; memória quando `salvar=False`. |
| `descarimbar_em_lote` | `app/chamados/models.py:3241 → :3252` | Compatível: C e campos de `outros`, com donos travados. |
| `encerrar_por_fato_no_espelho` | `app/chamados/models.py:3257 → :3268` | Compatível: troca de via, sem fabricar efeito. |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:127 → :127` | Compatível: V e DP `VALIDACAO`. |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:163 → :163` | Compatível: limpa V, conserva resposta, DP `DEVOLUCAO`. |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1422 → :1425` | Compatível: R, invalidação V/C, DP `RESPOSTA`. |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1559 → :1562` | Compatível: R/V/C e DP; continua sem escrever F. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1635 → :1638` | Compatível: conserva texto existente; instante recebido. |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1658 → :1661` | Compatível: exige testemunha; instante histórico em R/DP. |
| `relavrar` | `app/chamados/services/relavra_via.py:106 → :106` | Compatível: via derivada e L. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:784 → :784` | Compatível: pendência/recusa; efeitos delegados. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1083 → :1083` | Compatível: recusa técnica; não demonstra DP `RECUSA` humana. |
| `recusar_pergunta` | `app/chamados/services/validacao.py:533 → :533` | Compatível: recusa humana pela porta de correção. |
| `conferir_data_do_fato` | `app/chamados/services/validacao.py:625 → :625` | Compatível: sinalizadores e auditoria dos dois dias; sem DP próprio de conferência. |

Os seis nascimentos:

| Porta | Referência do censo → atual | Delta |
|---|---|---|
| `ChamadoColaborador.abrir` | `app/chamados/models.py:833 → :833` | Descrição compatível. |
| `ChamadoColaborador.criar` | `app/chamados/models.py:1054 → :1054` | Descrição compatível. |
| `_abrir_ou_agregar_disputa` | `app/chamados/services/disputa_emissao.py:474 → :474` | Continua podendo retornar DS antes da geração feita pelo wrapper. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:670`) |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1024 → :1027` | Descrição de nascimento/contexto compatível. |
| `criar_disputa_ancora_de_chamado` | `app/chamados/services/disputa_emissao.py:1299 → :1302` | Descrição de âncora/nascimento compatível. |
| `PerguntaDisputa._nascer` | `app/chamados/models.py:2822 → :2825` | **Censo incompleto:** nascimento com assinaturas R/V também registra os atos correspondentes no mesmo atomic. Não reconstruir esses atos novamente. (`app/chamados/models.py:3018`, `app/chamados/models.py:3019`) |

Distinções semânticas que precisam acompanhar os endereços corrigidos:

| Distinção | Resultado atual e consequência |
|---|---|
| **D3 — “tem resposta”** | Continua diferente entre timestamp (`app/chamados/models.py:3497`), SQL com texto literal não vazio (`app/chamados/models.py:3348`), C6 com `.strip()` (`app/chamados/juizes.py:1341`) e sequência/passivo sem linha (`app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/services/tarefa_do_dia.py:132`). Nenhum desses critérios substitui globalmente os demais. |
| **D4 — validação × efeito** | V ainda mata a vivacidade C6; a tarefa, pela DP `VALIDACAO`, exige lavra vigente e distingue materializar, rejulgar e encerrar. (`app/chamados/juizes.py:1325`, `app/chamados/services/tarefa_do_dia.py:140`) |
| **D5/D6 — dia × instante** | Continua fato=dia do turno e conversa=dia civil; plantio combina hora e cadeia, incluindo tentativa no dia seguinte e envelope inclusivo. (`app/chamados/juizes.py:332`, `app/chamados/juizes.py:324`, `app/chamados/juizes.py:326`) |
| **D9 — relógio** | **Atualizar a formulação absoluta do censo:** C6 já aceita `hoje`, inclui-o no memo e propaga-o; `date.today()` permanece fallback. Nem todo chamador injeta esse dia. (`app/chamados/juizes.py:577`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:852`, `app/chamados/services/vigia_fio.py:813`) |
| **Dia do carimbo** | `hoje` do carimbo é o dia civil local da medição, não o dia do turno de cada pergunta. A dívida de interpretação está registrada. (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:230`, `app/docs/BACKLOG.md:1899`) |
| **Legado e executor** | `linhas_da_projecao_sem_linha` passou de `models.py:3903` para `app/chamados/models.py:3914`; correspondência segue pergunta+ato+instante. `PerguntaDisputa.save` está em `app/chamados/models.py:3413`; imutabilidade DP em `app/chamados/models.py:3903`. (`app/chamados/models.py:3927`, `app/chamados/models.py:3931`, `app/chamados/models.py:3935`) |
| **Exportada × indeterminada** | O juiz de entrega é `competencia_entregue`; a composição por dia está em `competencia_do_dia_entregue`. **Não copiar o balde de medida:** o vigia põe dia indeterminado em “aberta”; isso não autoriza APPLY. (`app/folha/export.py:556`, `app/chamados/services/vigia_fio.py:822`, `app/chamados/services/vigia_fio.py:831`) |

**2. Cruzamentos com BO-DISPUTA**

Aqui, **escrita significa edição do arquivo-fonte**. Chamar uma porta que produz efeitos no banco é dependência de leitura/uso, não edição daquele arquivo. A lista exata do patch permanece **[nao verificado]**; os cruzamentos abaixo seguem os arquivos de cura indicados nas matrizes fornecidas **[nao verificado quanto aos hunks futuros]**.

| Arquivo | E4b precisa fazer | Comportamento pressuposto que a BO pode mudar |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py:178` | **Ler/chamar; nenhuma edição obrigatória identificada.** | Hoje chama wrapper e depois gerador novamente. E4b não pode depender de duas tentativas nem tratar motivos retornados como perguntas efetivamente criadas. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:220`) |
| `app/chamados/services/disputa_emissao.py:474` | **Leitura transitiva obrigatória** pela fábrica. | Retorno de DS não prova pergunta própria: núcleo retorna antes da geração do wrapper. A cura de nascimento/dona precisa preservar os insumos efetivos do fato. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:670`, `app/chamados/services/disputa_emissao.py:682`) |
| `app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1302` | **Leitura/chamada conforme o destino apurado**, sem edição demonstrada. | Não usar “DS retornada” como prova de nascimento local; conferir pergunta, dona e efeito após a operação. O nascimento pode preceder a pergunta. (`app/chamados/services/disputa_emissao.py:1120`, `app/chamados/services/disputa_emissao.py:1358`) |
| `app/chamados/services/disputa_emissao.py:1661` | **Leitura/chamada histórica, se houver testemunha.** | Reconstruir instante testemunhado; não usar resposta nova para inventar história. (`app/chamados/services/disputa_emissao.py:1670`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1677`) |
| `app/chamados/services/declaracao_texto.py:86` | **Leitura transitiva** de marcos/ata; não há necessidade demonstrada de chamar os consumidores humanos. | `_candidata_reusavel` não resolve dona global: pode recusar candidata estrangeira e o consumidor abrir depois da DS. Preservar dia/ata e conferir dona real. (`app/chamados/services/declaracao_texto.py:250`, `app/chamados/services/declaracao_texto.py:254`, `app/chamados/services/declaracao_texto.py:360`, `app/chamados/services/declaracao_texto.py:376`) |
| `app/ponto/management/commands/reconciliar_grade.py:412` | **Nenhuma leitura operacional, chamada ou edição exigida pelo contrato E4b.** | Seu helper passa `motivos=[]`; não serve como substituto da fábrica para criar pergunta faltante. (`app/ponto/management/commands/reconciliar_grade.py:424`, `app/ponto/management/commands/reconciliar_grade.py:425`) |
| `app/chamados/management/commands/smoke_chamados.py:82` | **Nenhuma chamada ou edição exigida.** | O precedente chama núcleo e gerador separadamente; não copiar esse mecanismo para o backfill. (`app/chamados/management/commands/smoke_chamados.py:99`, `app/chamados/management/commands/smoke_chamados.py:102`) |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:14` | **Nenhuma edição exigida; diagnóstico eventual.** | O helper futuro das matrizes não é uma porta atual conferida para E4b **[nao verificado]**. O inventário contratado continua no vigia. (`app/chamados/services/vigia_fio.py:318`, `app/chamados/services/vigia_fio.py:542`) |

Dependências comuns que as matrizes também exigem preservar:

| Arquivo | Cruzamento e consequência |
|---|---|
| `app/chamados/models.py:3943` | **Ler/chamar**, mantendo seleção sob trava; `registrar` não deduplica o lote por pergunta+ato+instante. (`app/chamados/models.py:3953`) |
| `app/chamados/services/fio.py:1784`; `app/chamados/services/fio.py:1225` | **Ler identidade/canônica** quando necessário; eleição considera texto normalizado e PK, não apenas timestamp de resposta. (`app/chamados/services/fio.py:1231`, `app/chamados/services/fio.py:1233`) |
| `app/chamados/reconciliador.py:1533` | **Ler/chamar religação**; alcança derivador do emissor e possível reemissão. (`app/chamados/reconciliador.py:1556`, `app/chamados/reconciliador.py:1621`) |
| `app/chamados/services/materializacao.py:1158` | **Ler/chamar efeito canônico**; Batida continua por `registrar_batida`, com `fato_de_chao=False`. (`app/chamados/services/materializacao.py:927`, `app/chamados/services/materializacao.py:930`) |
| `app/chamados/signals.py:42`; `app/chamados/signals.py:144`; `app/chamados/signals.py:163` | **Ler efeitos transitivos**: pai, regularizações no commit e materialização síncrona. Save de motivos/contexto em DS fechada não é presumidamente inócuo. |

**DRY precisa conservar o silêncio de push:** rollback do banco não desfaz envio imediato. A fábrica já silencia o caminho; o wrapper envia diretamente. Callbacks de commit e efeitos síncronos também precisam ser distinguidos na conferência. (`app/chamados/services/fabrica_da_celula.py:218`, `app/chamados/services/disputa_emissao.py:671`, `app/chamados/services/disputa_emissao.py:687`, `app/chamados/signals.py:144`, `app/chamados/signals.py:163`)

Assim, **nenhuma edição compartilhada é necessária demonstrada pelo escopo do brief**; compatibilidade após a mudança da BO e interseção completa com o patch ausente permanecem **[nao verificado]**.

**3. Linhas do mapa/fila que alcançam o universo ou as portas**

O trecho recebido de `mapa.r` termina truncado durante LOTE-VALIDAR; conteúdo posterior é **[nao verificado]**. Os endereços conferíveis abaixo são do BACKLOG atual.

| Linha/item | Muda o que E4b faz? |
|---|---|
| **BO-0611-07-E4B-COBRE** — `app/docs/BACKLOG.md:1893` | **Sim, na entrega do DRY:** demonstrar se cobre o item 07; se não cobre, registrar residual O261. A linha não demonstra cobertura. |
| **BUG-PAR-FROTA-1110** — `app/docs/BACKLOG.md:1882` | Cruza validação/materialização; **não acrescenta cura conjunta ao backfill**. A aplicação atual ainda é individual. (`app/chamados/services/validacao.py:463`, `app/chamados/services/materializacao.py:1376`) |
| **DISPUTA-ZUMBI-1110** — `app/docs/BACKLOG.md:1886` | Exige distinguir carimbo, efeito e fechamento; não autoriza E4b a implementar fechamento por conta própria. |
| **PERGUNTA-DUPLICATA-1110** — `app/docs/BACKLOG.md:1887` | Pode afetar seleção/contagem; não autoriza acrescentar deduplicação: cura registrada depois da E4d. |
| **BUG-PLANTIO-NOTURNO-MAIS-1** — `app/docs/BACKLOG.md:1891` | Preservar dia do turno/instante; não incorporar migração de dia ao backfill. Autoridades atuais continuam em `app/chamados/juizes.py:332` e `app/chamados/juizes.py:296`. |
| **LOTE-VALIDAR-REGRESSAO / JOB** — `app/docs/BACKLOG.md:1892`; `app/docs/BACKLOG.md:1831` | Cruza validação, mas não muda a decisão nem autoriza criar job nesta obra; regressão ainda pede medida da causa. |
| **BO-0611-05-RESTO-DA-CLASSE** — `app/docs/BACKLOG.md:1889` | Não afrouxar dedup/colisão para “curar” passivo. As guardas continuam na porta. (`app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:132`, `app/ponto/registro_batida.py:150`) |
| **BO-0611-05-TEM-GPS-LEITORES** — `app/docs/BACKLOG.md:1890` | Não inventar coordenada nem presença GPS. **A pergunta antiga do mapa foi respondida:** zero e par parcial significam sem GPS. (`app/docs/CORTES.md:442`, `app/ponto/services/geofence.py:126`) |
| **BO-ROTULO-ENCERRADO** — `app/docs/BACKLOG.md:1884` | Não muda o efeito do backfill; cura expressamente de apresentação. |
| **SONDA3-METODO-CORRECAO** — `app/docs/BACKLOG.md:1885` | Não tratar transição aceita com superusuário como prova de ação do supervisor ou regularização do fato. |
| **BO-DISPUTA / matriz / RAIZ** — `app/docs/BACKLOG.md:1877`; `app/docs/BACKLOG.md:1878`; `app/docs/BACKLOG.md:1883` | Dependência das portas; não incorporar cura do agregado nem mexer nas sete por shell. |
| **CARIMBO-VELHO / CIVIL-X-TURNO** — `app/docs/BACKLOG.md:1898`; `app/docs/BACKLOG.md:1899` | Ausência de campo novo não prova zero; dia da medição não vira dia do turno. |
| **E4A1-C6-X-PASSADO-SEM-TURNO** — `app/docs/BACKLOG.md:1902` | Chamar fábrica não garante atravessar descarimbo: existe retorno antecipado para pauta. (`app/chamados/services/fabrica_da_celula.py:201`, `app/chamados/services/fabrica_da_celula.py:205`) |
| **E4A2-RELOGIO-FIO-VIVO / O169-JUIZES-760** — `app/docs/BACKLOG.md:1903`; `app/docs/BACKLOG.md:1905` | Não presumir relógio integralmente injetado nem destino encerrado como prova suficiente de efeito. |

O mapa **não redefine a–e nem permite classe→dono/cura fixa**: o classificador continua consultando célula, vivacidade, âncora e lista real do admin. (`app/chamados/services/vigia_fio.py:661`, `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:673`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:677`)

**4. NÃO TOCAR**

| Cerca | Delta recomendado para o brief |
|---|---|
| `disparar_perguntas_competencia.py` | **Manter.** Envelheceu apenas a justificativa “obra em curso”: agora é vigia, com `handle :145`; o reparo indicado é semear eventos para consumo posterior. (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`, `app/chamados/management/commands/disparar_perguntas_competencia.py:11`, `app/chamados/management/commands/disparar_perguntas_competencia.py:145`) |
| `config/crons.py` e `vigiar_fio_morto.py` | **Manter.** Cron do disparar já está sem apply; carimbo já transporta os novos contadores. E4b DRY não precisa reeditar nenhum deles. (`app/config/crons.py:1521`, `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| Fábrica, materialização, reconciliador, relavra e modelos | Tornar explícito na lista final o **“só chamar” já existente no CENSO**; especialmente fábrica compartilhada com a cura BO. (`app/chamados/services/fabrica_da

[truncado em 20000 de 21988 caracteres]

## 10/11 21:31 · sessao c9f8f6b73718 · - · - · origem=Ronald (tmux)

Item 3 — linhas do mapa/fila que cruzam E4b:

| Item | Referência atual | Consequência para o backfill |
|---|---|---|
| **BO-0611-07-E4B-COBRE** | `app/docs/BACKLOG.md:1893` | Acrescenta obrigação explícita de **conferir pelo DRY** se item 07 é coberto; se não, residual O261. A linha não prova cobertura nem permite anunciar cura. (`app/docs/BACKLOG.md:1893`) |
| **BUG-PAR-FROTA-1110 / BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS** | `app/docs/BACKLOG.md:1882`; `app/docs/BACKLOG.md:1929` | Cruza as portas de validação/materialização, mas não autoriza E4b a aplicar metades isoladas nem implementar o novo par. A porta atual continua materializando unidade em `validacao.py:463` e lote em `materializacao.py:1376`; a cura conjunta permanece registrada. (`app/chamados/services/validacao.py:463`; `app/chamados/services/materializacao.py:1376`; `app/docs/BACKLOG.md:1929`) |
| **DISPUTA-ZUMBI-1110** | `app/docs/BACKLOG.md:1886` | Afeta interpretação de carimbo e fechamento; E4b precisa distinguir efeito e continente. Não transformar `pode_fechar` em autorização para fechar por conta própria. A linha manda separar premissa por sonda e declara carimbadores sem C7. (`app/docs/BACKLOG.md:1886`) |
| **PERGUNTA-DUPLICATA-1110** | `app/docs/BACKLOG.md:1887` | Duplicatas podem afetar seleção e contagem; não acrescentar deduplicação ao backfill. A cura está registrada **depois da E4d**. (`app/docs/BACKLOG.md:1887`) |
| **BUG-PLANTIO-NOTURNO-MAIS-1** | `app/docs/BACKLOG.md:1891`; ocorrência anterior `app/docs/BACKLOG.md:1817` | Não autoriza migrar dia no E4b. A leitura mantém fato = dia do turno e conversa = dia civil; plantio tenta dia e dia seguinte no envelope inclusivo. (`app/chamados/juizes.py:332`; `app/chamados/juizes.py:324`; `app/chamados/juizes.py:326`) |
| **LOTE-VALIDAR-REGRESSAO-0311 / LOTE-VALIDAR-VIRA-JOB** | `app/docs/BACKLOG.md:1892`; `app/docs/BACKLOG.md:1831` | Cruza porta de validação, mas acrescenta investigação de desempenho/job; não muda a autorização de E4b para criar decisões ou efeitos. A regressão ainda manda medir causa antes. (`app/docs/BACKLOG.md:1892`) |
| **BO-0611-05-RESTO-DA-CLASSE** | `app/docs/BACKLOG.md:1889` | Não permite afrouxar dedup/colisão para forçar o passivo. A porta conserva dedup de mesmo tipo, competência e recusa automática por oposta. (`app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:150`) |
| **BO-0611-05-TEM-GPS-LEITORES** | `app/docs/BACKLOG.md:1890` | E4b não deve inventar presença GPS nem normalizar coordenadas no comando. A pergunta do mapa antigo foi respondida pelo corte vigente: zero/par parcial = sem GPS; isso é leitura, não veto de nascimento. (`app/docs/CORTES.md:442`; `app/ponto/services/geofence.py:126`; `app/ponto/services/geofence.py:136`) |
| **BO-ROTULO-ENCERRADO** | `app/docs/BACKLOG.md:1884` | Não altera o efeito do backfill; é cura de rótulo, expressamente sem mexer em estado. (`app/docs/BACKLOG.md:1884`) |
| **SONDA3-METODO-CORRECAO** | `app/docs/BACKLOG.md:1885` | Não usar “transição tecnicamente aceita com superusuário” como prova de ação do supervisor ou regularização do fato. A própria linha distingue esses resultados. (`app/docs/BACKLOG.md:1885`) |
| **BO-DISPUTA / matriz da porta / RAIZ-DA-DISPUTA** | `app/docs/BACKLOG.md:1877`; `app/docs/BACKLOG.md:1878`; `app/docs/BACKLOG.md:1883` | Dependência de coerência das portas, não autorização para E4b reparar as sete por shell nem incorporar raiz/agregado. O registro proíbe mexer nas sete por shell e preserva HX. (`app/docs/BACKLOG.md:1877`) |

As classes a–e continuam decididas em `censo_alcance`: sem célula=e; véu=a; respondida sem texto=b; respondida com âncora terminal=c; sem vivas, colaborador na lista mas dia fora=d; demais=e. Portanto o mapa não substitui esse classificador por “classe→cura” ou “classe→dono”. (`app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:669`; `app/chamados/services/vigia_fio.py:673`; `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:677`; `app/chamados/services/vigia_fio.py:680`)

Outras linhas atuais que afetam a leitura do universo/portas:

| Linha | Consequência |
|---|---|
| `app/docs/BACKLOG.md:1898` FIO-ESTADO-CARIMBO-VELHO | Carimbo anterior à E4a2 pode não conter campos novos; ausência de chave não prova zero. (`app/docs/BACKLOG.md:1898`) |
| `app/docs/BACKLOG.md:1899` CARIMBO-DIA-CIVIL-X-TURNO | `hoje` do vigia é dia civil local da medição, não dia do turno da pergunta. O código usa `timezone.localdate(agora)` e grava esse valor. (`app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/management/commands/vigiar_fio_morto.py:230`) |
| `app/docs/BACKLOG.md:1902` E4A1-C6-X-PASSADO-SEM-TURNO | Não presumir que chamar a fábrica sempre atravessa descarimbo: há retorno antecipado para passado sem turno previsto. (`app/chamados/services/fabrica_da_celula.py:201`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:205`) |
| `app/docs/BACKLOG.md:1903` E4A2-RELOGIO-FIO-VIVO | Não afirmar controle integral de C6 por relógio do comando; dívida registrada de injeção de relógio. (`app/docs/BACKLOG.md:1903`) |
| `app/docs/BACKLOG.md:1905` O169-JUIZES-760 | Não tomar destino encerrado como efeito materializado sem a conferência da autoridade; alcance produtivo segue dependente de medida. (`app/docs/BACKLOG.md:1905`) |

Item 4 — cerca e delta pós-11h:

| Arquivo/cerca | Resultado |
|---|---|
| `disparar_perguntas_competencia.py` | **Manter NÃO TOCAR**. O motivo “E4a2 em curso” envelheceu: agora é vigia; `handle` fica em `:145`, não `:65`; não aceita `--apply`; ajuda declara leitura; reparo indicado é `consumir_eventos_celula --semear`, cujo consumo fica para rodada seguinte. (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`; `app/chamados/management/commands/disparar_perguntas_competencia.py:11`; `app/chamados/management/commands/disparar_perguntas_competencia.py:128`; `app/chamados/management/commands/disparar_perguntas_competencia.py:132`; `app/chamados/management/commands/disparar_perguntas_competencia.py:145`) |
| `config/crons.py` | Manter cerca: cron de disparar já usa `--comp-corrente --alarme` sem apply, papel vigia registrado; não há razão de E4b reeditar para reproduzir E4a2. (`app/config/crons.py:1521`; `app/config/crons.py:2044`; `app/config/crons.py:2168`) |
| `vigiar_fio_morto.py` | Manter cerca desta volta DRY. `handle` passou do endereço antigo `:151` para `:152`; agrega novos contadores do disparar e seus esperados no carimbo. A composição atual ainda avalia `d` antes de incorporar alcance/materializada, portanto etapa futura do alarme não se resume a acrescentar nomes à tupla. (`app/chamados/management/commands/vigiar_fio_morto.py:152`; `app/chamados/management/commands/vigiar_fio_morto.py:181`; `app/chamados/management/commands/vigiar_fio_morto.py:184`; `app/chamados/management/commands/vigiar_fio_morto.py:185`; `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| `ponto/services/geofence.py` | Acrescentar como dependência protegida explícita da obra já pousada: juiz `tem_gps` em `:126`; geofence chama em `:269`; sem GPS cria alerta com coordenadas nulas. Não editar essa regra no E4b. (`app/ponto/services/geofence.py:126`; `app/ponto/services/geofence.py:269`; `app/ponto/services/geofence.py:271`; `app/ponto/services/geofence.py:275`) |
| `core/templatetags/hasner_filters.py`, `ponto/views.py` e registro já cercado `core/juizes.py` | Complemento de cerca GPS: filtros delegam ao juiz; contador offline também; registro aponta juiz. Não reconstruir presença por nulidade no E4b. (`app/core/templatetags/hasner_filters.py:30`; `app/core/templatetags/hasner_filters.py:34`; `app/core/templatetags/hasner_filters.py:38`; `app/ponto/views.py:485`; `app/core/juizes.py:1201`) |
| `ponto/registro_batida.py` | Acrescentar explícito **só chamar pela materialização, não editar**. Conserva competência por `fato_de_chao=False`, dedup e colisão; o novo comando não pode abrir outro escritor para contorná-los. (`app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:123`; `app/ponto/registro_batida.py:127`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:150`) |
| `fabrica_da_celula.py` | Tratar como porta protegida e dependência de BO-DISPUTA, não copiar lógica: `perguntar_pela_celula` atual em `:178`; pode retornar sem emitir por sem adesão, pauta de cadastro ou limbo exportado. Logo chamada não prova pergunta criada. (`app/chamados/services/fabrica_da_celula.py:178`; `app/chamados/services/fabrica_da_celula.py:183`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:215`) |

A pergunta GPS do mapa.r não deve ser reaberta: o corte atual é explícito, inclusive sobre zeros e par parcial. As duas perguntas sobre desligado/férias existem no backlog, mas pertencem à obra registrada correspondente e não são condição normativa nova para E4b cumprir seu DRY pelas portas existentes. (`app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1881`)

Identidade de commit e atribuição exata de cada hunk a pouso pós-11h: **[nao verificado]**, pois esta cópia não fornece metadados Git verificáveis. [nao verificado]

## 10/11 21:30 · sessao 724cd7299560 · - · - · origem=Ronald (tmux)

Conferência das **26 portas** abaixo. “Compatível” significa que o corpo atual sustenta a descrição do censo; identidade textual com a base e autoria/data dos deltas são **[nao verificado]** sem árvore-base/Git.

| Porta | Censo → HEAD | Semântica conferida |
|---|---|---|
| `ChamadoColaborador._gravar_estado` | `app/chamados/models.py:201` → **:201** | Compatível: estado/via/autor, urgência e consequências nas perguntas. `app/chamados/models.py:228`, `:245`, `:292`, `:309`. |
| `DisputaSupervisao.fechar` | `app/chamados/models.py:1852` → **:1855** | Compatível: fechamento F; exceções carimbam mudas e registram DP `SILENCIO`; zero perguntas usa `vazia`. `app/chamados/models.py:1903`, `:1921`, `:1924`, `:1932`. |
| `reabrir_admin` | `app/chamados/models.py:2123` → **:2126** | Compatível: limpa F/resolução/motivo; conta reabertura e agrega motivo opcional. `app/chamados/models.py:2149`, `:2153`, `:2161`. |
| `reabrir_sistema` | `app/chamados/models.py:2160` → **:2163** | Compatível: limpa F/resolução/motivo, idempotência por aberta; nota do sistema. `app/chamados/models.py:2167`, `:2173`, `:2180`. |
| `PerguntaDisputa.carimbar` | `app/chamados/models.py:3092` → **:3103** | Compatível: C; completa R/V condicionalmente; julga com C6, restaura memória se recusa; L opcional. **Não chama DP no ramo de assinatura**. `app/chamados/models.py:3130`, `:3139`, `:3149`, `:3152`. |
| `carimbar_identidade` | `app/chamados/models.py:3157` → **:3168** | Compatível: C sem C6, conserva instante/autor quando solicitado; recusa no mesmo save. `app/chamados/models.py:3182`, `:3187`, `:3190`. |
| `descarimbar` | `app/chamados/models.py:3192` → **:3203** | Compatível: limpa os três C; `salvar=False` apenas memória; fora do fato recusa. `app/chamados/models.py:3206`, `:3208`, `:3211`. |
| `descarimbar_em_lote` | `app/chamados/models.py:3241` → **:3252** | Compatível: exige donos travados, exclui fora do fato, C + `outros`. `app/chamados/models.py:3258`, `:3264`. |
| `encerrar_por_fato_no_espelho` | `app/chamados/models.py:3257` → **:3268** | Compatível: só troca via, sem fabricar efeito. `app/chamados/models.py:3273`, `:3275`. |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:127` → **:127** | Compatível: V e DP `VALIDACAO` no mesmo instante/atomic; preserva projeção anterior sem linha. `app/chamados/services/validacao.py:133`, `:134`, `:139`. |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:163` → **:163** | Compatível: limpa V, conserva resposta, DP `DEVOLUCAO`; contexto de continente recarregado. `app/chamados/services/validacao.py:188`, `:199`, `:205`, `:214`. |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1422` → **:1425** | Compatível: R, limpa V, delega C, advertência de escape, DP `RESPOSTA`; trava C antes de persistir. `app/chamados/services/disputa_emissao.py:1467`, `:1480`, `:1485`, `:1497`, `:1506`, `:1511`. |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1559` → **:1562** | Compatível: limpa R/V; C opcional; foto em DP `REABERTURA`/`RECUSA`; **não escreve F**. `app/chamados/services/disputa_emissao.py:1593`, `:1615`, `:1629`, `:1632`. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1635` → **:1638** | Compatível: texto normalizado existente é no-op; R e DP no instante recebido. `app/chamados/services/disputa_emissao.py:1647`, `:1652`, `:1656`. |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1658` → **:1661** | Compatível: exige texto sem carimbo; conserva instante testemunhado em R/DP. `app/chamados/services/disputa_emissao.py:1670`, `:1674`, `:1677`. |
| `relavrar` | `app/chamados/services/relavra_via.py:106` → **:106** | Compatível: via derivada e L. `app/chamados/services/relavra_via.py:118`, `:126`. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:784` → **:784** | Compatível: pendência/recusa continuam escrita semântica própria; efeito/carimbo delegados. `app/chamados/services/materializacao.py:850`, `:851`, `:924`, `:927`. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1083` → **:1083** | Compatível: recusa técnica; colisão delega identidade, demais salvam string; nota técnica, sem DP `RECUSA` humana. `app/chamados/services/materializacao.py:1093`, `:1099`, `:1102`, `:1107`. |
| `recusar_pergunta` | `app/chamados/services/validacao.py:533` → **:533** | Compatível: recusa humana exige motivo e R; limpa via reabertura com `ato='recusa'`. `app/chamados/services/validacao.py:561`, `:576`, `:590`, `:593`. |
| `conferir_data_do_fato` | `app/chamados/services/validacao.py:625` → **:625** | Compatível: juiz de divergência, sinalizadores de conferência e auditoria com dois dias/justificativa; não gera DP de conferência. `app/chamados/services/validacao.py:658`, `:664`, `:667`. |
| `ChamadoColaborador.abrir` | `app/chamados/models.py:833` → **:833** | Compatível: nascimento/reuso do chamado pela porta; estado canônico preservado. `app/chamados/models.py:833`. |
| `ChamadoColaborador.criar` | `app/chamados/models.py:1054` → **:1054** | Compatível: nascimento de chamado pela porta. `app/chamados/models.py:1054`. |
| `_abrir_ou_agregar_disputa` | `app/chamados/services/disputa_emissao.py:474` → **:474** | Corpo HEAD é anterior à cura BO anexada: nascimento/agregação; núcleo atual retorna DS antes da geração do wrapper. `app/chamados/services/disputa_emissao.py:603`, `:634`, `:665`. |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1024` → **:1027** | Descrição de nascimento aberto/contexto continua aplicável; cura futura matriz não é HEAD. `app/chamados/services/disputa_emissao.py:1027`. |
| `criar_disputa_ancora_de_chamado` | `app/chamados/services/disputa_emissao.py:1299` → **:1302** | Nascimento/âncora continua nesta porta. `app/chamados/services/disputa_emissao.py:1302`. |
| `PerguntaDisputa._nascer` | `app/chamados/models.py:2822` → **:2825** | **Descrição do censo incompleta:** além de identidade/defaults/reuso/L, nascimento com R/V recebidos conserva assinaturas em DP no mesmo atomic: `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha([p]))`. `app/chamados/models.py:3014`, `:3017`, **:3018–3019**. |

Delta dos executores e juízes que importa ao backfill:

| Item | HEAD / consequência |
|---|---|
| DP canônica | Censo `models.py:3932` → **`app/chamados/models.py:3943`**; **brief já aponta corretamente :3943**. Lote vazio retorna antes do atomic; lote não vazio faz `bulk_create` e acorda células. **Não deduplica por si**: seleção canônica antes continua necessária. `app/chamados/models.py:3947`, `:3951`, `:3953`, `:3954`. |
| Imutabilidade DP | `save` antigo :3892 → **`app/chamados/models.py:3903`**; reescrita recusa; delete também recusa. `app/chamados/models.py:3904`, `:3908`. |
| Correspondência legado | Antiga :3903 → **`app/chamados/models.py:3914`**; correspondência continua `(pergunta, ato, instante)`, inclui dedup no próprio lote, conserva V por autor. `app/chamados/models.py:3927`, `:3931`, `:3935`. |
| Pergunta save | Antiga :3402 → **`app/chamados/models.py:3413`**. |
| D3: “tem resposta” | Distinções permanecem: timestamp em **`app/chamados/models.py:3497`**; SQL timestamp + texto não vazio literal em **:3348**; C6 timestamp + `.strip()` em **`app/chamados/juizes.py:1341`**; tarefa usa DP e passivo sem linha em **`app/chamados/services/tarefa_do_dia.py:127`**, **:132**. |
| D4: validação ≠ efeito | Rótulo por V/M agora **`app/chamados/models.py:3034`**, condições **:3041**; C6 V mata em **`app/chamados/juizes.py:1325`**; DP `VALIDACAO` exige lavra vigente e distingue materializar/rejulgar/encerrar em **`app/chamados/services/tarefa_do_dia.py:140`**. |
| D5/D6 | Dia do turno para fato, civil para conversa continuam explícitos em **`app/chamados/juizes.py:332`**; instante de plantio com cadeia e envelope inclusivo continua em **`app/chamados/juizes.py:296`**, **:324**, **:326**. |
| **D9 precisa atualizar** | Já há injeção `hoje`: **`app/chamados/juizes.py:569`**, **:839**, **:1324**. Memo inclui `hoje` em **:852**; cálculo o propaga em **:856**, **:1389**, **:1406**. `date.today()` fica **fallback**, em **:577**. Portanto “C6 conserva date.today que --agora não controla” precisa qualificar o chamador: sem `hoje`, conserva; com `hoje`, usa o dia fornecido. |
| D9 — bordas preservadas | Janela inclusiva em **`app/chamados/juizes.py:603`**; lavra expira `<=` por dia local e julgamento local anterior à borda em **`app/chamados/veredito_lavrado.py:116`**. O instrumento ainda imprime literalmente o limite do fallback em **`app/chamados/management/commands/diff_e3_sombra.py:105`**. |

Não atribuir automaticamente `EXPAND` do nascimento ou injeção de `hoje` aos pousos posteriores às 11h: essa cronologia é **[nao verificado]**; a linha de DP do próprio brief já coincide com HEAD.

## 10/11 21:30 · sessao 8b2dc0981577 · - · - · origem=Ronald (tmux)

O patch `diff_obra_v10.patch` não está disponível na cópia nem no texto recebido; seus hunks e a lista exata de arquivos tocados permanecem **[nao verificado]**. As matrizes1/2 descrevem uma implementação não presente nesta árvore: aqui o núcleo está em `app/chamados/services/disputa_emissao.py:474`, a geração continua posterior ao núcleo no wrapper em `app/chamados/services/disputa_emissao.py:670`, e não há redirecionamento recursivo nesse núcleo (`app/chamados/services/disputa_emissao.py:521`, `app/chamados/services/disputa_emissao.py:603`).

**Item 2 — cruzamentos de código previstos pelas matrizes**

“Escrita” abaixo significa editar o arquivo-fonte. Chamar uma porta que escreve no banco é uso da porta, sem edição do arquivo que a implementa. O comando E4b ainda não existe nesta cópia **[nao verificado quanto à implementação futura]**.

| Arquivo previsto na cura BO-DISPUTA | Cruzamento necessário da E4b | Comportamento que a E4b precisa conferir/preservar |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py` | **LEITURA + chamada de porta**, sem edição necessária demonstrada: `perguntar_pela_celula` está em `:178`; chama `perguntar` em `:219`; esta chama wrapper e gerador em `:172` e `:174`. | A fábrica usa dia da célula, respeita teto pelo instante, pode abrir pauta sem pergunta e devolve **motivos**, não prova de perguntas criadas (`:201`, `:204`, `:207`, `:215`, `:220`). BO pretende retirar a segunda geração; E4b não pode depender de duas tentativas nem contar `ms` como efeito consumado (`:172`, `:174`, `:220`). |
| `app/chamados/services/disputa_emissao.py` | **LEITURA transitiva obrigatória** pela fábrica: núcleo `:474`, wrapper `:634`, gerador `:300`, pendentes `:257`. Outras portas dependem da classificação: órfãos `:1027`; âncora `:1302`. | Não supor que retornar DS significa pergunta própria criada: o núcleo cria DS em `:587` e retorna em `:603`, enquanto a geração ocorre depois, em `:670` ou `:682`. A geração admite ausência de dia e `continue` (`:425`, `:430`) e pode devolver reuso da porta (`:444`, `:470`). A E4b precisa observar resultado real e dona efetiva, sem implementar por conta própria o planejamento que BO pretende corrigir. |
| `app/chamados/services/disputa_emissao.py` | **LEITURA/chamada condicional de porta histórica**, sem edição necessária: `carimbar_resposta_pela_trilha :1661`; resposta `:1425`; correção `:1562`. | Resposta nova conserva legado antes de sobrescrever (`:1466`) e registra DP com o mesmo instante (`:1462`, `:1511`). A reconstrução da E4b deve continuar distinguindo ato testemunhado de resposta nova; não usar o core de resposta para inventar história (`:1462`, `:1466`, `:1511`). |
| `app/chamados/services/declaracao_texto.py` | **LEITURA transitiva**, sem necessidade demonstrada de chamar `declarar_hora_colab :306` ou `aplicar_declaracao :641` para o backfill. A fábrica lê mapeamento em `fabrica_da_celula.py:62`; o emissor lê marcos/abstenção em `disputa_emissao.py:859`, `:864`, `:895`. | `_marcos_faltantes :86` e a abstenção existente precisam conservar o significado do dia/ata. Não presumir que `_candidata_reusavel :230` resolve dona global: ela retorna `None` quando a canônica pertence a outra DS (`:250`, `:254`). Os consumidores ainda abrem depois do nascimento da DS (`:360`, `:376`, `:659`, `:666`); BO pretende corrigir justamente essa composição. |
| `app/ponto/management/commands/reconciliar_grade.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de precedente **[nao verificado]**. Sítio previsto pela matriz: `_disputa :412`. | O helper chama núcleo com `motivos=[]` (`:424`, `:425`). Não é alternativa para a fábrica nem prova de pergunta faltante criada. |
| `app/chamados/management/commands/smoke_chamados.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de precedente **[nao verificado]**. Sítio previsto: `_bloco_a_motivos :82`. | O smoke chama núcleo em `:99` e gerador novamente em `:102`; seu mecanismo não deve ser copiado pelo backfill. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de diagnóstico **[nao verificado]**. | A cópia começa diretamente pelo piso I6 (`:14`, `:29`); o helper I7 `_dias_da_disputa_sem_pergunta` descrito nas matrizes não existe aqui **[nao verificado quanto ao futuro BO]**. O censo E4b deve usar o vigia contratado, não importar um helper futuro dessa auditoria. |

**Dependências comuns que as matrizes mandam preservar, sem demonstrar edição pela BO**

| Arquivo | E4b lê/chama e implicação |
|---|---|
| `app/chamados/models.py` | DP `registrar :3943` faz `bulk_create :3953`, sem deduplicação nessa operação; correspondência legada está em `:3927`, `:3931`, `:3935`. BO declara manter a porta da pergunta; edição por BO **[nao verificado]**. E4b deve selecionar sob trava e conservar a identidade completa, sem deduplicar só por existência de algum ato. |
| `app/chamados/services/fio.py` | Universo `:1784`, canônica `:1225`, dia da linha `:1608`, recusas `:1572` e `:1591`. A canônica prefere texto normalizado, depois menor PK (`:1231`, `:1232`, `:1233`); isso é distinto de `respondida_em`. Edição por BO **[nao verificado]**. |
| `app/chamados/reconciliador.py` | Religação `reatar_pergunta_orfa :1533` chama o derivador do emissor (`:1556`) e pode reemitir chamado (`:1621`). Usa data da pergunta (`:1557`), não simplesmente o último contexto da DS. Edição por BO **[nao verificado]**. |
| `app/chamados/services/materializacao.py` | Unidade `:1158`, ausência declarada `:462`, batida pela porta em `:927`/`:1007`. A unidade não possui barreira de exportada (`:1166`, `:1167`, `:1215`); a E4b deve fornecer sua própria seleção/revalidação contratada. Edição por BO **[nao verificado]**. |
| `app/chamados/signals.py` | Save de DS fechada pode reconciliar pai (`:28`, `:42`), agendar regularização no commit (`:129`, `:144`) e materializar pendentes sincronamente (`:153`, `:163`). Não supor que save só de motivos/contexto seja sem efeito: essas guardas não inspecionam `update_fields`. Edição por BO **[nao verificado]**. |

**DRY e integração:** rollback de banco não basta para impedir push disparado imediatamente pelo wrapper (`app/chamados/services/disputa_emissao.py:671`, `:687`, `:1767`). O caminho contratado da fábrica já silencia push (`app/chamados/services/fabrica_da_celula.py:218`); a guarda está em `app/chamados/services/disputa_emissao.py:1753`. A E4b deve preservar esse caminho e recontar os efeitos reais, incluindo cascatas síncronas; efeitos dependentes do acervo e execução dos callbacks permanecem **[nao verificado]**.

**Conclusão para consolidação:** não há necessidade demonstrada de a E4b **editar** qualquer dos arquivos previstos pelas matrizes BO; ela pode escrever somente seu comando novo e os dois selos pedidos **[nao verificado quanto à implementação futura e ao patch ausente]**. Há dependência de leitura/chamada da fábrica e do emissor alterados pela BO, portanto a compatibilidade precisa ser reconferida após seu pouso (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`).

**cruzamento de escrita com BO-DISPUTA: 0**, no escopo proposto do brief; confirmação contra `diff_obra_v10.patch`: **[nao verificado]**.

_3522 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
