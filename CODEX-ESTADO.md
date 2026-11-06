# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 22:05** · publicado em **05/11/2026 22:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 22:05 · sessao 096a16709250 · C20-FIO-MUDO · verificador · origem=Code

**Parecer: não certificaria a obra como completa.** A escrita continua pelas portas existentes, mas há eleição paralela da canônica, retratação antes da aceitação da costura e falta de garantia transacional na reabertura direta. (`app/chamados/models.py:484`, `app/chamados/models.py:487`, `app/chamados/models.py:492`, `app/chamados/models.py:1408`, `app/chamados/models.py:1478`)

Execução dos testes, reprodução das corridas e resultado real em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/ato | Escritores | Leitores relevantes |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | `PerguntaDisputa.abrir`, tanto nascimento quanto reapontamento por identidade; `fio.reapontar_resolvedora`. O método novo chama essa porta. (`app/chamados/models.py:2526`, `app/chamados/models.py:2552`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:492`) | Âncora, C6/materialização, plano e reconciliador. (`app/chamados/services/fio.py:117`, `app/chamados/juizes.py:817`, `app/chamados/juizes.py:744`, `app/chamados/models.py:344`, `app/chamados/reconciliador.py:1480`) |
| `via_resolucao` | Núcleo `_escrever_carimbo`; portas de carimbar/descarimbar; também os sítios existentes de relavra e encerramento por fato. O novo caminho passa por `retratar_duplicata → carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6; migrador, por C6; ressuscitação. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/services/fio.py:935`, `app/chamados/models.py:1531`) |
| `validada_em` | Validação/devolução administrativa; simetria em `carimbar`; desfazimento por correção e ressuscitação. Não recebe escritor novo neste diff. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6, estado, seleção de ancoradas e reconciliação. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:2575`, `app/chamados/models.py:327`, `app/chamados/reconciliador.py:620`) |
| `status_local` | `_gravar_estado` e nascimento pelas portas existentes. O helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia, gates, guarda da costura e juiz de materialização. (`app/chamados/services/vigia_fio.py:335`, `app/chamados/models.py:400`, `app/chamados/services/fio.py:764`, `app/chamados/juizes.py:744`) |
| Evento `pergunta_reatada` | Gate anterior e helper novo chamam `core.observ.evento`. Antes, `objeto_id` identifica a âncora que fecha; agora também pode identificar o destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico e auditoria genérica. Consumidor de produção específico por esse nome: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`, `app/core/views.py:705`) |
| Lavra do reapontamento | `veredito_lavrado.lavrar` calcula os três campos; `reapontar_resolvedora` grava destino e lavra no mesmo `save`. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Leitura do veredito e controle de envelhecimento do cache. (`app/chamados/juizes.py:632`, `app/chamados/veredito_lavrado.py:173`) |

**Não há segundo escritor direto novo fora da porta.** A autorização do destino já contempla `models.py` e `fio.py`; o diff acrescenta uma chamada, não uma atribuição paralela. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

Há, porém, resultados diferentes entre leitores:

- `fio_vivo` reduz C6; o vigia delega à mesma redução. Reatar muda a âncora, sem criar outra definição de vivacidade. (`app/chamados/juizes.py:1233`, `app/chamados/services/vigia_fio.py:156`)
- O helper aceita pergunta de disputa fechada e não reabre seu continente. O admin deliberadamente mostra essas vivas, mas `_disputa_do_chamado` exige disputa aberta e o caminho do colaborador também preserva esse recorte. Portanto, “reatada e viva” não prova “questionário novamente acessível ao colaborador”. (`app/chamados/models.py:464`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:209`, `app/api/views.py:1008`)
- A ressuscitação existente reabre a disputa quando efetivamente devolve perguntas; a adoção não executa esse efeito. Isso é uma diferença de cobertura, sem autorização para simplesmente reabrir tudo. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`, `app/chamados/services/fio.py:211`)

**2. Juiz novo ou derivação paralela**

Dia, motivos, competência, vivacidade e âncora são consultados nas funções existentes; não identifiquei cópia nova dessas cinco regras. O registro mantém C6 e a redução do fio nas autoridades já declaradas. (`app/chamados/models.py:454`, `app/chamados/models.py:459`, `app/chamados/models.py:468`, `app/chamados/models.py:475`, `app/core/juizes.py:260`)

A diferença para `_plano_pergunta_viva` é o sentido da busca: o plano parte da âncora e procura uma cobrança sobrevivente, conferida por E4-c; o novo método parte da cobrança e procura perguntas do colaborador/dia/motivo. (`app/chamados/models.py:364`, `app/chamados/models.py:373`, `app/chamados/models.py:464`)

**Existe uma segunda regra de eleição:** `_ja` determina que “a que já está aqui vence”, sem consultar `canonica_do_grupo` sobre o grupo completo. Exemplo: pergunta local sem resposta e órfã respondida; a lei da canônica favorece a respondida, mas o helper ignora a órfã inteira. A migração existente compara origem e destino conjuntamente. (`app/chamados/models.py:477`, `app/chamados/models.py:484`, `app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:937`)

**3. Ordem de trava**

- `abrir()` envolve a seleção e criação em `atomic`, adquirindo primeiro a trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`)
- **`reabrir()` diretamente não faz o mesmo:** consulta estado sem trava, chama `_gravar_estado`, depois ressuscita e adota. O `atomic` interno de `_gravar_estado` não envolve toda essa sequência. Quando chamado por `abrir`, herda a transação externa; fora disso, não há a mesma garantia. (`app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1468`, `app/chamados/models.py:1477`)
- `canonica_do_grupo` somente ordena objetos em memória; competência consulta janela e entrega, sem trava explícita nesses helpers. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`)
- `retratar_duplicata` grava a pergunta pela porta de identidade; `reapontar_resolvedora` abre `atomic`, trava o chamado de destino e depois grava a pergunta canônica. Não trava nem relê a pergunta antes da decisão. (`app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`)

Não identifiquei inversão **direta nova** de Disputa→Colaborador nesses helpers: eles não adquirem `select_for_update` de Disputa. O callback da pergunta agenda becos em `on_commit`. Isso não constitui prova geral contra deadlock; o próprio selo declara que não analisa chamadas entre funções. (`app/chamados/services/fio.py:742`, `app/chamados/services/fio.py:878`, `app/chamados/signals.py:191`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**4. Relógio e orçamento de queries**

O método novo não lê relógio diretamente nem recebe instante. Entretanto, chama C6, que pode chegar ao `date.today()` decisório preexistente de `_dentro_da_janela_viva`. Portanto, não introduz essa leitura, mas amplia seu alcance dentro de `abrir` sem injetar relógio da borda. (`app/chamados/models.py:434`, `app/chamados/models.py:475`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`)

Os `now()` de retratação e lavra são carimbos de escrita preexistentes; devem ser distinguidos daquele relógio decisório. A composição da competência recebe o dia explícito. (`app/chamados/services/fio.py:884`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/services/vigia_fio.py:838`)

**Há custo por candidata:** a busca carrega somente `disputa`, enquanto C6 acessa célula, colaborador, empresa e tranca. O helper não aplica o mapa de identidade usado pela migração; `_st` resolve em lote apenas os estados das âncoras. (`app/chamados/models.py:464`, `app/chamados/models.py:469`, `app/chamados/juizes.py:801`, `app/chamados/juizes.py:932`, `app/chamados/services/fio.py:927`)

Os selos citados não certificam esse orçamento:

- `test_c2_corrida_do_abrir` usa módulo `sem_previsao_escala`, que sai na primeira guarda C20; mede deduplicação concorrente, não queries da adoção. (`app/chamados/tests/test_c2_corrida_do_abrir.py:56`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/models.py:452`)
- O chokepoint mede batida/reconciliação/estado; seu `abrir` de preparação está fora da captura correspondente. Quebra efetiva de teto: **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:110`, `app/ponto/tests/test_selo_chokepoint_escrita.py:312`)

**5. Idempotência**

No sucesso sequencial normal, a segunda adoção encontra a pergunta em `self`, inclui seu motivo em `_ja` e não reaponta nem repete `pergunta_reatada`. (`app/chamados/models.py:477`, `app/chamados/models.py:484`)

Isso **não garante zero duplicata**:

- Se já houver uma viva local, a órfã equivalente permanece intocada. (`app/chamados/models.py:484`)
- Uma duplicata respondida pode continuar viva: C6 examina a resposta antes da via de identidade, e a porta documenta explicitamente essa exceção. A frase nova “duplicata já é morta pelo C6” é falsa como afirmação geral. (`app/chamados/models.py:446`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:2702`)

Dois `abrir` da mesma chave serializam pelo colaborador; o segundo retorna o existente sem nova adoção. Dois `reabrir` diretos não têm essa proteção integral. Ambas as execuções podem selecionar a mesma órfã antes da costura; a porta trava o destino, mas salva a pergunta recebida sem refazer a seleção. Duplicação de evento em corrida é um contraexemplo estático possível; reprodução: **[não verificado]**. (`app/chamados/models.py:940`, `app/chamados/models.py:959`, `app/chamados/models.py:1408`, `app/chamados/models.py:464`, `app/chamados/services/fio.py:845`)

“Uma linha” também depende do sucesso da auditoria: `evento` é best-effort e pode retornar sem gravá-la. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`)

**6. Contraexemplos e fronteiras**

| Caso | Resultado demonstrável |
|---|---|
| Outro dia ou motivo | Fica fora do filtro. A busca também não alcança D±1 que E4-c admite como mesma jornada overnight; conformidade desse alcance com o aval: **[não verificado]**. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1991`) |
| Outro módulo | Outro módulo de **destino novo** sai pela constante; módulo diferente da **âncora antiga terminal** não impede adoção. (`app/chamados/models.py:432`, `app/chamados/models.py:452`, `app/chamados/models.py:473`) |
| Morta por C6 ou validada | Não é adotada. A exceção de duplicata respondida segue a precedência do próprio C6. (`app/chamados/models.py:475`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`) |
| Competência entregue | Sai antes da busca. O renascimento admite competência exportada, portanto pode reabrir sem recuperar a órfã por esta cura. A exclusão copia a cerca existente; sua conciliação com L-113 permanece **[não verificado]**. (`app/chamados/models.py:460`, `app/chamados/models.py:347`, `app/chamados/models.py:1030`, `app/docs/LEIS.md:150`) |
| Agrupado/limbo | As entradas normais protegem agrupado sob pai vivo; o helper isolado não consulta agrupamento. Chamado de outro módulo, como destino de limbo, sai pela guarda inicial. (`app/chamados/models.py:960`, `app/chamados/models.py:1442`, `app/chamados/models.py:452`) |
| Pai de adesão/nunca-bateu | O pai de adesão usa outro módulo e não adota. Entretanto, uma órfã preexistente de um `batida_ausente` nunca-bateu pode passar: `nunca_bateu` acusa no C6. R19d não cobre esse estoque. (`app/chamados/services/adesao.py:74`, `app/escala/models.py:1767`, `app/chamados/juizes.py:1181`, `app/chamados/models.py:464`) |
| Destino terminal ou nenhum | Coberto quando C6 permite. “Sem destino” com pai vivo não é órfã: a âncora efetiva é o pai, preservado pela guarda. (`app/chamados/services/fio.py:117`, `app/chamados/models.py:473`, `app/chamados/models.py:492`) |
| Renascer por premissa | Alcança a adoção se efetivamente passar por `reabrir`; existente já vivo retorna sem reparar órfãs. (`app/chamados/models.py:1055`, `app/chamados/models.py:1478`, `app/chamados/models.py:959`) |

**Pouso antes do pré-requisito:** o método retrata irmãs antes de saber se a canônica será aceita. Exemplo estático: célula em D e contexto em D+2; a seleção usa D pela célula, E4-c compara o contexto e recusa, mas as retratações anteriores permanecem. A abertura continua. Execução desse cenário: **[não verificado]**. (`app/chamados/juizes.py:90`, `app/chamados/juizes.py:1973`, `app/chamados/juizes.py:1998`, `app/chamados/models.py:487`, `app/chamados/models.py:493`)

**7. Os cinco atos de perda**

| Ato | Cobertura |
|---|---|
| **Reabrir** | Novo gancho cobre reabertura efetiva de Chamado, após ressuscitar as próprias. `Disputa.reabrir_admin/sistema/por_colab` não chama diretamente a adoção; a casca administrativa chama `ch.reabrir` somente se encerrado. (`app/chamados/models.py:1477`, `app/chamados/models.py:2116`, `app/chamados/models.py:2153`, `app/chamados/models.py:2222`, `app/chamados/services/acoes_disputa.py:265`) |
| **Retratar duplicata** | A porta carimba a duplicata, mas não reata a canônica. Pode retirar a última viva de um chamado enquanto a canônica permanece num terminal; o diff não acrescenta evento nessa operação. (`app/chamados/services/fio.py:878`, `app/chamados/models.py:1009`, `app/chamados/models.py:1478`) |
| **Validação** | Já possui transação para carimbo/materialização/desfecho e recalcula por evento. A morte por validação é legítima; não exige irmã nova. O diff não acrescenta gatilho C20 aqui. (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:405`, `app/chamados/juizes.py:1136`) |
| **Morte por leitura** | C6 já resolve tranca, marco e célula; E2 carimba absolvidas pelo julgamento existente. Não há nova adoção nessa leitura. Morte legítima não demonstra perda indevida de cobertura. (`app/chamados/juizes.py:1138`, `app/chamados/juizes.py:1177`, `app/chamados/reconciliador.py:651`, `app/ponto/services/cartorio.py:958`) |
| **Reapontar** | A porta existente protege o **destino**, valida e lavra no mesmo save. Não verifica o chamado de origem que perde sua última pergunta. A legitimidade de deixar essa origem sem fio precisa ser demonstrada por caso. (`app/chamados/services/fio.py:784`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`) |

Assim, os dois ganchos novos não demonstram, sozinhos, cobertura dos cinco atos. (`app/chamados/models.py:1009`, `app/chamados/models.py:1478`)

**8. Mordida dos testes e R19d** [nao verificado]

R08/R08b exigem cobrança, destino correto, fio vivo e nenhuma irmã; R08r e R19b incluem guardas explícitas de vivacidade. São as verificações mais fortes do arquivo, mas execução RED e eficácia por mutação permanecem **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:125`, `app/chamados/tests/test_c20_fio_mudo.py:164`, `app/chamados/tests/test_c20_fio_mudo.py:204`)

Há lacunas concretas:

- **R19a pode passar sem nascer cobrança:** não exige cobrança não nula e compara contra `getattr(..., None)`. (`app/chamados/tests/test_c20_fio_mudo.py:174`)
- **R19c não exerce adoção futura:** roda o vigia de hoje e consulta ausência amanhã, sem semear órfã futura nem abrir cobrança futura. (`app/chamados/tests/test_c20_fio_mudo.py:210`)
- O controle de tranca é anunciado, mas não há caso executável correspondente; também faltam segunda adoção após sucesso, trilha única, corrida C20, grupo respondido/duplicado e orçamento de queries. (`app/chamados/tests/test_c20_fio_mudo.py:11`, `app/chamados/tests/test_c20_fio_mudo.py:91`)

**R19d procede para dispensar questionário de quem nunca bateu:** o cartório carimba sem emitir, e adesão declara que a ação pertence ao supervisor/RH. Porém, a mesma lei manda agrupar os furos no pai próprio; ela não comprova legitimidade indefinida de qualquer filho diário vivo. R19d não verifica pai nem agrupamento. (`app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1268`, `app/chamados/services/adesao.py:19`, `app/chamados/services/adesao.py:102`, `app/chamados/tests/test_c20_fio_mudo.py:106`)

Por fim, os números reproduzidos no teste somam **495**; retirados os três nunca-bateu, restam **492**, não o universo literal **494**. Reconciliação dos universos e comprovação individual das exclusões: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `app/chamados/tests/test_c20_fio_mudo.py:13`)

## 05/11 22:03 · sessao 107544328c40 · - · - · origem=Ronald (tmux)

Achados principais:

- **`reabrir()` não tem a mesma transação/trava de `abrir()`.** Lê estado sem `select_for_update`, grava estado em transação interna e só depois ressuscita/adota. A unidade estado+adoção depende de transação externa do chamador. `abrir()` garante `atomic` e trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`, `app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1477`)

- **Corrida/idempotência concorrente não garantida.** O helper coleta perguntas/âncoras antes de costurar; a porta trava o destino, mas não relê nem trava a pergunta antes de decidir. Duas execuções podem selecionar a mesma órfã e ambas salvar/registrar o evento; reprodução concorrente [não verificado]. (`app/chamados/models.py:464`, `app/chamados/models.py:473`, `app/chamados/models.py:496`, `app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`)

- **Continente fechado continua fechado.** O helper admite perguntas de qualquer disputa e não reabre a disputa; C6 pode considerá-las vivas, mas `_disputa_do_chamado` só retorna disputa aberta. Assim, existe contracaso estático de destino vivo com fio vivo, mas lookup administrativo ausente; ocorrência em produção [não verificado]. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`) A ressuscitação existente reabre o continente quando altera perguntas; a adoção não executa isso. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`)

Inventário:

| Campo/ato | Escritores existentes e utilização nova | Leitores relevantes |
|---|---|---|
| `chamado_destino` | `PerguntaDisputa.abrir` cria e reaponta identidade; `fio.reapontar_resolvedora` costura. Helper novo apenas chama a porta. (`models.py:2526`, `models.py:2552`, `services/fio.py:845`, `models.py:492`, todos em `app/chamados/`) | Âncora: `services/fio.py:116`; C6/memo: `juizes.py:817`; materialização: `juizes.py:743`; plano/gate: `models.py:344`; órfãs: `reconciliador.py:1480`. |
| `via_resolucao` | Núcleo `_escrever_carimbo`, portas `carimbar_identidade`/`descarimbar`/lote; exceções existentes `encerrar_por_fato_no_espelho` e `relavrar`. Novo dedup chama `retratar_duplicata` → `carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2722`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6: `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`; migrador: `app/chamados/services/fio.py:935`; ressuscitação: `app/chamados/models.py:1531`. |
| `validada_em` | Validação administrativa e devolução; simetria pela porta `carimbar`; desfazimento por resposta/correção/ressuscitação. Nenhum escritor direto novo. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6: `app/chamados/juizes.py:1136`; estado: `app/chamados/models.py:2575`; ancoradas: `app/chamados/models.py:327`; reconciliação: `app/chamados/reconciliador.py:620`. |
| `status_local` | Núcleo `_gravar_estado`; nascimento pelas portas `abrir`/`criar`. Helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:1131`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia: `app/chamados/services/vigia_fio.py:335`; gate: `app/chamados/models.py:400`; costura: `app/chamados/services/fio.py:764`; C6/materialização: `app/chamados/juizes.py:744`. |
| `pergunta_reatada` | Gate antigo e helper novo chamam `core.observ.evento`; antigo associa à âncora que fecha, novo ao destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`; auditoria genérica: `app/core/views.py:705`. Consumidor de produção específico por nome [não verificado]. |
| Lavra | `veredito_lavrado.lavrar` atribui os três campos; `reapontar_resolvedora` salva destino e lavra juntos. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Cache/staleness: `app/chamados/veredito_lavrado.py:173`; C6 consulta o juiz de materialização: `app/chamados/juizes.py:1214`. |

**Não identifiquei segundo escritor direto introduzido pelo diff:** a lista autorizada do destino já contém `models.py` e `fio.py`, e a nova chamada usa `reapontar_resolvedora`. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

**Travas:** `canonica_do_grupo` é cálculo em memória; competência compõe consultas; `retratar_duplicata` atualiza pergunta pela porta; `reapontar_resolvedora` trava chamado destino antes de atualizar a canônica. Não há nova trava explícita de `DisputaSupervisao` nesses helpers, portanto não há inversão direta nova de Disputa→Colaborador. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`, `app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`) O hook da pergunta agenda becos somente em `on_commit`; o selo da ordem admite não enxergar inversões entre funções. (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**Idempotência sequencial:** sucesso move a pergunta para `self`; segunda chamada marca o motivo em `_ja` e pula escrita/evento. Isso não prova “zero duplicata” global: uma órfã do mesmo motivo é ignorada quando já existe viva em `self`, sem retratá-la. (`app/chamados/models.py:477`, `app/chamados/models.py:484`) Duplicata respondida também pode permanecer viva pela precedência da resposta em C6. (`app/chamados/models.py:2704`, `app/chamados/juizes.py:1150`)

**“Uma linha” não é garantia absoluta:** `evento` é best-effort e pode retornar sem linha após falha; o teste novo não conta `LogAuditoria`. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/tests/test_c20_fio_mudo.py:1`)

## 05/11 22:03 · sessao 7fd15dd1c688 · - · - · origem=Ronald (tmux)

Achados estáticos principais:

- **Pouso antes do pré-requisito:** o método retrata as duplicatas antes de saber se a canônica pode ser costurada; se a porta recusar, mantém as retratações e segue a abertura (`app/chamados/models.py:487-495`; `app/chamados/services/fio.py:837-849`). Contraexemplo possível: célula em D, contexto em D+2; `dia_do_fato` escolhe D pela célula, mas E4-c compara o contexto e recusa D×D+2 (`app/chamados/juizes.py:90-92`, `:1973-1999`). Execução desse exemplo: [não verificado].
- **Dedup incompleto:** havendo viva já ancorada no destino, `_ja` pula todo o grupo órfão; não aplica canônica nem retrata a irmã (`app/chamados/models.py:477-490`). A migração existente compara origem+destino e aplica a canônica ao grupo inteiro (`app/chamados/services/fio.py:937-945`).
- **Nenhuma cópia evidente de juiz:** dia, motivo, competência, viva e âncora são delegados às funções existentes (`app/chamados/models.py:454-475`); registro C1/C5/C6/C8/C14/C15 em `app/core/juizes.py:260-277`. `_plano_pergunta_viva` parte das perguntas ancoradas e procura sobrevivente pelo C8/E4-c; o método novo parte da cobrança e procura perguntas do colab-dia-motivo (`app/chamados/models.py:364-377`, `:464-475`). Igualdade de data limita a busca, enquanto E4-c permite D±1 da mesma jornada overnight (`app/chamados/juizes.py:1957-1959`, `:1991-1997`); conformidade dessa limitação com o aval: [não verificado].

Contraexemplos e controles:

- Outro módulo de **destino novo** é recusado pela constante; outro módulo da **âncora antiga** não é recusado, desde que terminal (`app/chamados/models.py:432`, `:452`, `:473`).
- Outro dia/motivo fica fora do filtro; morta pelo C6 fica fora; competência entregue fica fora (`app/chamados/models.py:460-476`).
- Agrupado sob pai vivo é recusado antes da reabertura, e o `abrir` também deduplica filho agrupado (`app/chamados/models.py:1387-1388`, `:1442-1443`, `:960-967`). O helper isolado não pergunta por agrupamento (`app/chamados/models.py:452-492`).
- `nunca_bateu` pertence aos vereditos que acusam e pode manter uma pergunta existente viva pelo C6 (`app/escala/models.py:1767-1768`; `app/chamados/juizes.py:1181-1184`). Assim, órfã prévia pode ser adotada nesse cenário; o método não tem exclusão própria de adesão (`app/chamados/models.py:464-492`). Isso tensiona “não há o que responder pelo questionário” de `app/chamados/services/adesao.py:19-21`, mas proibir criar pergunta nova não implica automaticamente proibir reatar uma existente: [não verificado].
- Pergunta viva em disputa fechada pode ser adotada porque o filtro não exige disputa aberta (`app/chamados/models.py:464-465`); o app continua filtrando disputas abertas (`app/api/views.py:1008`), enquanto o fio do admin deliberadamente mostra vivas em disputas fechadas (`app/chamados/services/fio.py:209-221`). Portanto “reatada” não prova retorno ao questionário do colaborador; reabrir essa disputa automaticamente contrariaria o precedente documentado em `app/chamados/services/fio.py:211-212`.
- Competência entregue: exclusão herdada de `_vivas_entre`, não novo juiz (`app/chamados/models.py:347`, `:460`). Porém o renascimento declara expressamente permitido em competência exportada e chama `reabrir` (`app/chamados/models.py:1030-1035`, `:1055`): pode renascer sem adotar órfã. A resolução da tensão com L-113/L-092 exige distinguir escopo de cura e lei; conformidade final: [não verificado].

Cinco atos:

1. **Reabrir:** coberto para Chamado pelo novo gancho, após ressuscitar próprias (`app/chamados/models.py:1477-1478`); renascimento passa por ele (`:1055`). Reabrir Disputa diretamente não chama adoção (`:2116-2171`, `:2218-2224`); a casca administrativa chama `ch.reabrir` somente se encerrado (`app/chamados/services/acoes_disputa.py:265-266`).
2. **Retratar duplicata:** porta preserva identidade/canônica, mas não reata a canônica (`app/chamados/services/fio.py:878-895`). Pode matar a última pergunta de um chamado vivo enquanto a canônica continua num terminal; o gancho novo não roda nessa operação (`app/chamados/models.py:1007-1009`, `:1478`). `abrir` sobre existente vivo retorna sem adoção (`:956-959`).
3. **Validação:** já atomiza carimbo, materialização e devolução de recusa e recalcula por evento (`app/chamados/services/validacao.py:395-427`). Validada morre legitimamente pelo C6 (`app/chamados/juizes.py:1136-1137`); essa morte não exige pergunta nova.
4. **Morte por leitura:** C6 já decide tranca, marco e célula (`app/chamados/juizes.py:1138-1146`, `:1177-1196`). E2 carimba absolvidas, consulta C6 e poupa trancadas (`app/chamados/reconciliador.py:582-604`, `:651-658`), acionado pelo cartório (`app/ponto/services/cartorio.py:958`). Morte legítima sai do universo; não demonstra lacuna por si.
5. **Reapontar:** já protege destino pela validação E4-c, trava, lavra e save conjunto (`app/chamados/services/fio.py:837-849`). Não verifica o chamado de origem que perdeu a última pergunta; preservar o fio da origem não é efeito garantido dessa porta (`:784`, `:845-848`). Se a transferência torna a origem legítima sem pergunta, essa exclusão precisa de prova própria: [não verificado].

## 05/11 22:02 · sessao 7de248e93add · - · - · origem=Ronald (tmux)

4. **Custo e relógio**

- O método novo não chama `now()`/`today()` diretamente, mas não recebe nem repassa instante: `app/chamados/models.py:434-502`. A composição de competência usa o dia explícito: `app/chamados/services/vigia_fio.py:838`; `app/ponto/janelas.py:60-63`.
- Há relógio decisório indireto preexistente: `pergunta_viva` pode alcançar `date.today()` através de `_dentro_da_janela_viva`: `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`. A regra de relógio injetado consta em `app/docs/PROMPTS.md:1624`. O diff amplia o alcance desse caminho no `abrir`, sem criar essa leitura: `app/chamados/models.py:475`, `app/chamados/models.py:1009`.
- Há timestamps preexistentes dentro do laço: `retratar_duplicata` usa `timezone.now()` quando não recebe `agora`, e a lavra usa `timezone.now()`: `app/chamados/services/fio.py:884-891`; `app/chamados/veredito_lavrado.py:228`. São carimbos de escrita; não confundir com o relógio decisório anterior: mesmos locais.
- Há N+1 potencial concreto: candidatos carregam apenas `disputa`, enquanto C6 acessa `celula`, `disputa.colaborador`, empresa e tranca: `app/chamados/models.py:464-465`; `app/chamados/juizes.py:801`, `app/chamados/juizes.py:932-947`. Fora de request, o memo pertence à instância do colaborador: `app/chamados/juizes.py:966-979`. A adoção não aplica o mapa de identidade que a migração já aplica: `app/chamados/services/fio.py:927-930`.
- O lote `_st` evita consultas individuais de status das âncoras, mas não elimina as consultas de C6: `app/chamados/models.py:469`, `app/chamados/models.py:475`.
- `test_c2_corrida_do_abrir` não mede queries; seu módulo é `sem_previsao_escala`, portanto sai antes do ramo C20: `app/chamados/tests/test_c2_corrida_do_abrir.py:56`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127-150`; `app/chamados/models.py:452`.
- `test_selo_chokepoint_escrita` mede batida/reconciliação/estado; seu `abrir` de preparação fica fora de `CaptureQueriesContext`: `app/ponto/tests/test_selo_chokepoint_escrita.py:110-119`, `app/ponto/tests/test_selo_chokepoint_escrita.py:312-326`. Não certifica orçamento de adoção na abertura; quebra efetiva de teto: **[não verificado]**.

8. **Mordida e R19d**

- R08/R08b cobram nascimento, destino, fio vivo e nenhuma irmã: `app/chamados/tests/test_c20_fio_mudo.py:125-148`. R08r e R19b possuem guardas explícitas de pergunta viva antes da operação: `app/chamados/tests/test_c20_fio_mudo.py:164`, `app/chamados/tests/test_c20_fio_mudo.py:204`.
- R19a pode passar sem cobrança nova: não exige `_cobranca_do_dia()` não nula e compara contra `getattr(..., None)`: `app/chamados/tests/test_c20_fio_mudo.py:174-185`.
- R19c não testa adoção futura: executa vigia de hoje e apenas consulta ausência amanhã; não semeia pergunta futura nem chama abertura/adoção futura: `app/chamados/tests/test_c20_fio_mudo.py:210-213`.
- O anunciado controle de tranca não existe como teste: aparece na descrição, mas os controles executáveis são validada, outro vivo e amanhã: `app/chamados/tests/test_c20_fio_mudo.py:11`, `app/chamados/tests/test_c20_fio_mudo.py:173-213`.
- Não há teste de trilha única, segunda adoção após sucesso, corrida C20, grupo duplicado/respondido ou queries no arquivo novo: `app/chamados/tests/test_c20_fio_mudo.py:91-213`.
- R19d tem suporte literal na lei operacional: zero batida viva → dívida de adesão, cartório carimba e retorna sem emitir; o texto da adesão declara ausência de questionário: `app/ponto/services/cartorio.py:1154-1162`, `app/ponto/services/cartorio.py:1268`; `app/chamados/services/adesao.py:19-21`.
- Essa leitura é válida para dispensar **questionário** do nunca-bateu; não comprova legitimidade indefinida de todo filho diário vivo: adesão prevê pai próprio e agrupamento/reconciliação dos furos: `app/chamados/services/adesao.py:74-86`, `app/chamados/services/adesao.py:102-118`. O R19d não verifica pai nem agrupamento: `app/chamados/tests/test_c20_fio_mudo.py:106-122`.
- O dado reproduzido no teste soma 495 e, retirando os três nunca-bateu, resulta em 492; não demonstra o universo literal 494: `app/chamados/tests/test_c20_fio_mudo.py:13-15`. Reconciliação dos universos: **[não verificado]**.

**Achado adicional:** a afirmação nova “duplicata já é morta por C6” não vale para toda pergunta respondida: C6 pode retornar viva antes de examinar a via de identidade, e a própria porta documenta essa exceção: `app/chamados/models.py:446`; `app/chamados/juizes.py:1150-1172`; `app/chamados/models.py:2702-2708`. Duas candidatas respondidas podem permanecer vivas após retratar a não canônica; o método reaponta somente a canônica: `app/chamados/models.py:487-492`. Esse caso não tem teste C20: `app/chamados/tests/test_c20_fio_mudo.py:91-213`.

## 05/11 21:20 · sessao d0c9cbba8c22 · H12H06-A · verificador · origem=Code

**O delta corrige o caso do teste, mas a correção de duração continua incompleta:** o template ainda decide primeiro por `dias_corridos`, antes de consultar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`)

O material recebido não traz linhas da sonda identificando registros dessa rodada; apontar qual `colNN`/ID permanece errado em produção: **[não verificado]**.

**1. Quais casos ficam corrigidos ou errados**

As conclusões abaixo são por inspeção, sem execução: **[não verificado: resultado executado]**.

| Registro/condição | Resultado na célula após o delta |
|---|---|
| Tipo aberto, `data_fim=None`, `dias_corridos<=1`, sem parcialidade | Exibe “—”: corrige o caso denunciado. (`app/ponto/views.py:2243`, `app/templates/ponto/ausencias.html:164`) |
| Tipo aberto, `data_fim=None`, `dias_corridos=N>1` | **Continua exibindo “N dias”**, embora a cobertura prossiga até o fim da janela consultada. O primeiro ramo impede alcançar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/ponto/turnos.py:2781`) |
| Range fechado de vários dias, mas `dias_corridos<=1` | **Continua exibindo “1 dia”**: `fim_aberto=False`, e a parcialidade também é falsa quando o fim difere do início. (`app/ponto/views.py:2243`, `app/ponto/turnos.py:2599`, `app/templates/ponto/ausencias.html:165`) |
| Range fechado com `dias_corridos=N>1` divergente das datas | **Continua exibindo N**, sem calcular a extensão inclusiva do range. A cobertura usa `data_fim`. (`app/templates/ponto/ausencias.html:161`, `app/ponto/turnos.py:2779`) |
| Tipo fora do catálogo, sem fim e `dias_corridos>1` | Exibe “N dias”, mas o juiz expande somente o dia inicial. Com `dias_corridos<=1`, “1 dia” coincide com essa expansão. (`app/templates/ponto/ausencias.html:160`, `app/ponto/catalogo/ausencias.py:194`, `app/ponto/turnos.py:2783`) |
| Tipo simultaneamente aberto e lançável por minutos, sem fim, com `0<minutos<480` e `dias_corridos<=1` | Exibe minutos antes de alcançar `fim_aberto`; a abertura continua sem representação na célula. Essa combinação de cadastro é oferecida sem exclusão entre os campos. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:78`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`) |

**Tipo inexistente não recebe permissão de abertura:** `fim_de` devolve `POR_LEI`, portanto `em_aberto_permitido` devolve falso. Ausência no catálogo atual também pode decorrer de inativação, pois a carga considera somente tipos ativos. (`app/ponto/catalogo/ausencias.py:194`, `app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:249`)

A divergência entre datas e `dias_corridos` é normalizada no `save`, mas a lista apenas lê o registro: o delta não normaliza nem salva os valores carregados. A existência desses registros em produção permanece **[não verificado]**. (`app/ponto/models.py:894`, `app/ponto/models.py:900`, `app/ponto/views.py:2238`, `app/ponto/views.py:2244`)

**2. Quem escreve e quem lê**

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `fim_aberto` | Atribuição transitória em cada objeto da lista; não é gravação no banco. (`app/ponto/views.py:2243`) | Ramo da duração no template. (`app/templates/ponto/ausencias.html:164`) |
| `Ausencia.data_fim` | Criação: `derivar_data_fim` → kwargs → `save`. Edição: rederiva, atribui e salva. O modelo também deriva fim quando há dias acima de 1 em tipo não aberto; transição de estado inclui o campo em `update_fields`; `rederivar_fim` reaplica o `save`. (`app/ponto/services/ausencia.py:197`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:499`, `app/ponto/services/ausencia.py:508`, `app/ponto/models.py:907`, `app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:1280`) | No caminho da lista: seleção por vigência, juiz de parcialidade e cálculo de `fim_aberto`. Na cobertura: `dias_da_ausencia` e `cobertura_ausencia_periodo`. Há também consumidores no export, relatórios e drawer. (`app/ponto/turnos.py:3087`, `app/ponto/turnos.py:2599`, `app/ponto/views.py:2243`, `app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2760`, `app/folha/export.py:70`, `app/relatorios/views.py:205`, `app/templates/ponto/partials/painel_ausencia.html:124`) |
| Retorno de `em_aberto_permitido(tipo)` | A função calcula `fim_de(tipo) == INDETERMINADO`; não persiste o booleano. A alimentação vem do catálogo vigente, carregado do cadastro ativo ou do fallback. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:192`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`) | A view nova, a validação de `indeterminado` no formulário e o comando que procura bloqueantes sem fim. (`app/ponto/views.py:2243`, `app/ponto/forms.py:85`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:24`) |

Quem alimenta `TipoAusencia.fim` pela tela é `salvar` do cadastro, passando os campos para `gravar_tipo_ausencia`, que cria ou altera o modelo. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1289`)

**Sim: para o mesmo catálogo vigente, `em_aberto_permitido(tipo)` e a pertinência a `Ausencia.TIPOS_EM_ABERTO` respondem à mesma pergunta com o mesmo conjunto:** tipos cujo `fim` é `indeterminado`. O segundo é alias de um conjunto vivo, não uma lista congelada diferente. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:434`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/models.py:689`)

Isso responde à **permissão do tipo**. A duração do registro ainda depende de existir `data_fim`; `dias_da_ausencia` prioriza esse fim explícito. (`app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2781`)

**3. Mordida do teste** [nao verificado]

**Por inspeção, morde o comportamento anterior descrito no patch:** cria `afastamento_inss` sem fim, consulta a lista, isola a célula e rejeita “1 dia”, que era o fallback anterior. RED efetivamente executado em `58ac8543` e GREEN nesta cópia: **[não verificado]**. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:171`, `app/chamados/tests/test_h06_falta_pela_grade.py:177`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`; comportamento anterior no patch fornecido)

**Passaria com outra regressão:** célula vazia, “0 dias” ou “2 dias” satisfazem a única asserção sobre o conteúdo. O teste exige uma célula, mas não exige “—” nem prova a extensão da cobertura. Também não exercita os casos divergentes da tabela. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:178`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

**4. Outro defeito no delta**

O defeito funcional demonstrável é a **correção incompleta pela ordem dos ramos**, acompanhado da asserção insuficiente do teste; outro defeito introduzido pelo delta: **[não verificado]**. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

A célula aberta também mantém `data-value` baseado em `dias_corridos`, mesmo exibindo “—”; esse atributo já está fora do hunk alterado, portanto não o atribuo como regressão nova deste delta. (`app/templates/ponto/ausencias.html:159`, `app/templates/ponto/ausencias.html:164`; patch fornecido)

## 05/11 21:12 · sessao a965ac098cf9 · H12H06-A · verificador · origem=Code

**Retifico a v1: a contagem do TXT, isoladamente, não demonstra regressão de A.** Para a mesma falta aprovada e as mesmas datas, o 8792 recebe a mesma quantidade com 720 ou zero minutos. A troca altera as horas calculadas, mas não introduziu a contagem por calendário. (`app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`)

**1. Condição para falta aprovada com zero**

No escape da resposta, `FALTA_DIA` solicita `tipo='falta', status='aprovada'`. O materializador omite minutos; a porta fornece zero e `Ausencia.save` substitui esse valor pelo previsto do dia. **Uma falta nova permanece aprovada com zero se o previsto consultado nessa gravação resultar em zero.** (`app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O sítio `declara_falta` não faz a falta nascer aprovada: omite status, a porta aplica `estado_inicial` e a aprovação posterior passa novamente pelo `save`, incluindo a persistência de minutos derivados. (`app/chamados/services/materializacao.py:484`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:818`)

O juiz devolve zero sem vínculo, sem tipo, quando `eh_dia_trabalho(...) is not True` ou quando faltam início/fim. **Com previsto positivo preservado até a gravação, esses sítios não produzem falta nova aprovada com zero.** (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:885`)

Antes disso, os emissores de falta impõem guardas que a v1 deixou de considerar:

- O detector exige vínculo, resultado de dia de trabalho e início previsto; seu funil exige célula, com exceção explícita para limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`. Esse ramo exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e dia encerrado; depois o emissor exige vínculo e `eh_dia_trabalho is True`. (`app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- A porta de nascimento também veta chamados dos módulos abrangidos quando o dia está identificado e falta vínculo ou a régua é degenerada. (`app/chamados/models.py:928`, `app/chamados/models.py:1097`, `app/ponto/services/furos_vetados.py:40`)

Essas condições não constituem uma guarda universal `previsto > 0` para toda pergunta: a fábrica da célula consulta `dia_sem_trabalho`, situação, data, veredito e marcos; a fábrica de órfãos existentes admite explicitamente continuar sem vínculo. **Isso não prova nascimento atual de chamado sem escala através da porta de emissão.** (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/chamados/models.py:1105`)

Há uma possibilidade condicional no código de produção: pergunta já existente continua aceita, mas o previsto na materialização passa a zero. Por exemplo, uma pergunta anteriormente respondida, sem validação, sem tranca e sem decisão administrativa pode continuar viva pelo texto; os materializadores não revalidam previsto positivo. Ocorrência desse cenário no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`, `app/api/views.py:2631`, `app/chamados/services/materializacao.py:516`)

O teste sem escala cria chamado e pergunta diretamente; portanto não demonstra que os emissores atuais produziriam essa situação. (`app/api/tests/test_falta_na_resposta.py:26`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`)

**2. T1–T4 e alterações numéricas**

- **T1:** os leitores citados realmente usam minutos não zero para crédito e para pular a justificação integral. Com os valores novos descritos — falta derivada e atestado zero — não aparece a divergência apontada para atestados antigos de 720. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2598`)
  
  **A ressalva é causal:** A também muda números de **atestados novos desses sítios**, depois de aprovados. Antes entravam com 720; agora entram com zero. Nos leitores, isso troca crédito 720 por zero e permite justificar o dia quando o efeito é ABONA. Não depende de migrar legado. (`app/chamados/services/materializacao.py:522`, `app/ponto/models.py:877`, `app/ponto/services/fechamento.py:669`, `app/ponto/services/fechamento.py:690`)
  
  Exemplo deduzido: dia obrigatório sem trabalho, atestado aprovado ABONA, sem justificativa ou cobertura concorrente. A inclusão nas justificadas remove o dia das faltas injustificadas; se era a única falta injustificada da semana, altera também DSR perdido para DSR OK, contadores persistidos pelo fechamento. **É alteração numérica causada pelo diff; por si só, não demonstra defeito.** (`app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2654`, `app/ponto/services/fechamento.py:1085`)

- **T2:** a expansão usa datas e calendário, sem minutos ou grade; o TXT conta `len(datas)`. A divergência entre horas zero e quantidade de dias não foi criada por A. (`app/ponto/turnos.py:2774`, `app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`)

- **T3:** os sítios citados permanecem fora dos hunks funcionais de A; os dois materializadores alterados criam ausência de um dia, sem solicitar range. Não identifiquei nesses pontos outro caminho numérico introduzido pelo diff. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/folha/export.py:118`, `app/ponto/models.py:902`; hunks fornecidos no anexo)

- **T4:** o teste chama atestado aprovado diretamente; o escape real usa status vazio e deixa `estado_inicial` decidir. Em ambos, atestado permanece com zero porque a derivação exige tipo literal `falta`. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`, `app/chamados/services/materializacao.py:581`, `app/ponto/services/ausencia.py:344`, `app/ponto/models.py:877`)

**3. Achado novo no diff**

**Duração de afastamento aberto passa a ser apresentada como “1 dia”.** Um registro de tipo em `TIPOS_EM_ABERTO`, com `dias_corridos=1`, minutos zero e `data_fim=None`, é permitido pela porta; sua cobertura pode continuar por vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros. Zero retorna falso e o novo `else` imprime “1 dia”; no hunk anterior, esse caso imprimia “—”. O predicado de parcialidade não determina a duração total de um afastamento aberto. **É uma regressão de apresentação causada pelo diff.** Ocorrência em produção e impacto financeiro: **[não verificado]**. (`app/ponto/views.py:2238`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior no hunk fornecido)

## 05/11 21:11 · sessao 0a411a29b6d1 · - · - · origem=Ronald (tmux)

Para falta **nova**, o escape `FALTA_DIA` passa `('falta', 'aprovada')`; ambos os sítios omitem minutos, a porta aplica default zero e `save` substitui esse zero pela soma do previsto. Portanto fica aprovada com zero quando o juiz responde zero na ocasião da gravação. O ramo `declara_falta` nasce `aguardando_decisao`; só deriva quando posteriormente aprovado. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:781`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O previsto pode ser zero por ausência de vínculo/tipo, por `eh_dia_trabalho(...) is not True` ou ausência de início/fim. Esses materializadores não perguntam novamente se existe furo, célula de trabalho ou previsto positivo. Em `retornar_mesmo_tipo`, o veto de precedência nem é executado; uma linha existente também pode voltar sem novo `save`. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/chamados/services/materializacao.py:467`, `app/chamados/services/materializacao.py:516`, `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:377`)

**A guarda dos emissores de falta não pode ser omitida**, como foi na v1:

- O detector exige vínculo, dia com trabalho e início previsto; o funil ainda exige célula, ressalvada a exceção declarada de limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`; esse diagnóstico exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e turno encerrado. Depois, o emissor exige vínculo, `eh_dia_trabalho is True` e início. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`, `app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- Os crons de turno aberto também calam sem vínculo; a porta de nascimento do chamado consulta `motivo_do_veto`, que veta vínculo ausente. (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `app/chamados/models.py:1097`, `app/chamados/models.py:1105`, `app/ponto/services/furos_vetados.py:40`)

**As guardas de perguntas são mais amplas que as do detector de falta total.** A fábrica da célula exige ativo, dia que `dia_sem_trabalho` não dispense, data não futura e veredito vazio ou acusador; não consulta diretamente `minutos_previstos > 0`. Dia passado sem marcos pode seguir por `classificar_falta`, e `perguntar` fornece contexto datado para criar disputa/pergunta, inclusive sem destino derivável. (`app/chamados/services/fabrica_da_celula.py:27`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:389`)

Na fábrica de órfãos existentes, `_tem_base` aceita batidas sem escala e `_ata_do_dia` permite explicitamente continuar sem célula quando não há vínculo. `classificar_falta` também tem ramo binário por E/S e fallback sem marcos. Isso demonstra tratamento de **chamado existente**, não prova que um chamado novo ultrapasse o veto atual de nascimento sem vínculo. (`app/chamados/services/disputa_emissao.py:750`, `app/chamados/services/disputa_emissao.py:753`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/escala/servico_jornada.py:453`, `app/escala/servico_jornada.py:478`, `app/chamados/models.py:1105`)

Assim, o caminho defensável para zero é uma pergunta já disponível cujo previsto seja zero quando respondida/aprovada — por exemplo, alteração posterior de vínculo/grade — porque o materializador não revalida as condições de emissão. Ocorrência desse cenário na produção fornecida: **[não verificado]**. O teste sem escala fabrica diretamente chamado e pergunta; não demonstra sua emissão pelos crons. (`app/api/tests/test_falta_na_resposta.py:27`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`, `app/chamados/services/materializacao.py:516`, `app/ponto/models.py:885`)

Não encontrei emissor atual do motivo `declara_falta` no catálogo de perguntas; o caminho materializador continua presente. A existência/origem de perguntas desse motivo em produção permanece **[não verificado]**. (`app/chamados/catalogo/perguntas.py:34`, `app/chamados/services/materializacao.py:1047`)

## 05/11 21:10 · sessao a8bef660087b · - · - · origem=Ronald (tmux)

Há um achado novo de apresentação: **ausência com término aberto pode aparecer como “1 dia”**. A porta aceita `dias_corridos=1`, `minutos=0` e preserva `data_fim=None` para tipo em `TIPOS_EM_ABERTO`; a cobertura desse registro continua até o limite consultado, podendo abranger vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros; com zero minutos, o predicado retorna falso. O novo `else` do template apresenta então **“1 dia”**, enquanto o hunk anterior fornecido apresentava “—”. O predicado responde parcialidade do dia, sem determinar duração total de um afastamento aberto. (`app/ponto/views.py:2237`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior: `diff_A.patch`, hunk de `app/templates/ponto/ausencias.html:160` fornecido pelo usuário)

O caminho HTTP permite esse registro: dias omitidos viram 1; minutos vazios viram 0; o parâmetro indeterminado chega à porta de criação. Ocorrência em produção e impacto financeiro desse achado: **[não verificado]**. (`app/ponto/views.py:2380`, `app/ponto/services/ausencia.py:95`, `app/ponto/views.py:2405`, `app/ponto/views.py:2412`)

Em H12, os sítios inspecionados usam o mesmo predicado e a janela da empresa: classificação, resumos, contador e marcação da linha. Não identifiquei outro defeito causal nesses hunks por inspeção. (`app/folha/export.py:60`, `app/folha/export.py:807`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`)

## 05/11 21:10 · sessao bbbf9f695f68 · - · - · origem=Ronald (tmux)

T1 está correta sobre a idade dos leitores e a concordância com os valores novos de A: falta deriva minutos, mas não pertence a `TIPOS_PAGOS`; atestado zero não gera crédito e, sendo ABONA, justifica o dia. O juiz de cobertura também considera zero dia inteiro. (`app/ponto/models.py:683`; `app/ponto/catalogo/ausencias.py:95`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/turnos.py:2598`)

**Ressalva de causalidade:** isso não significa ausência de delta financeiro em A. O diff troca **os atestados novos desses sítios de 720 para zero**; portanto muda entradas dos leitores antigos mesmo sem migrar as 18 linhas antigas. Os sítios omitem minutos, a porta usa zero e o modelo só deriva tipo `falta`. (`app/chamados/services/materializacao.py:484`; `app/chamados/services/materializacao.py:520`; `app/ponto/services/ausencia.py:130`; `app/ponto/services/ausencia.py:343`; `app/ponto/models.py:877`)

Exemplo deduzido do código, sem afirmar ocorrência em produção: atestado aprovado com efeito ABONA, dia obrigatório sem trabalho, sem Justificativa e sem cobertura concorrente:

- Com 720: crédito de 720 e exclusão da justificação; com zero: crédito zero e inclusão nas justificadas. (`app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/efeito_ausencia.py:55`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`)
- No motor, esse dia sai de `faltas_injustificadas` e passa a `_abono`; mudam contagem e horas de falta/abono. Se era o único dia injustificado da semana, muda também semana DSR perdido para semana DSR OK. (`app/ponto/motor_calculo_v2.py:2600`; `app/ponto/motor_calculo_v2.py:2605`; `app/ponto/motor_calculo_v2.py:2610`; `app/ponto/motor_calculo_v2.py:2620`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:2654`)
- O fechamento persiste os contadores de DSR do motor; a sua `horas_falta` vem separadamente das faltas declaradas, portanto não se deve confundir a alteração de `resultado.horas_falta` com alteração automática desse campo persistido. (`app/ponto/services/fechamento.py:866`; `app/ponto/services/fechamento.py:987`; `app/ponto/services/fechamento.py:1085`)

Esse delta é coerente com atestado de dia inteiro; **não constitui por si só novo defeito**, mas uma descrição de T1 que atribua toda alteração numérica apenas à migração do legado fica incompleta. (`app/ponto/turnos.py:2583`; `app/ponto/services/fechamento.py:685`; `app/ponto/motor_calculo_v2.py:2600`)

T2: os fatos citados estão certos. `datas_no_periodo` usa `dias_da_ausencia`; a expansão usa calendário e limites do registro/janela, sem minutos e sem grade; TXT usa `len(datas)`. Uma mesma falta aprovada de um dia gera a mesma quantidade TXT com 720 ou zero. (`app/folha/export.py:63`; `app/ponto/turnos.py:2774`; `app/folha/export.py:87`; `app/folha/export.py:218`; `app/folha/export.py:732`)

T4: a fraqueza do teste existe, mas não altera o resultado de minutos. O teste usa atestado aprovado diretamente; a tabela real usa status vazio; a porta aplica `estado_inicial`, documento anexado conduz a `aguardando_decisao`, aprovação passa por `mudar_estado`, e nenhuma dessas etapas deriva minutos de atestado. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`; `app/chamados/services/materializacao.py:581`; `app/ponto/services/ausencia.py:344`; `app/ponto/services/ausencia.py:763`; `app/ponto/services/ausencia.py:857`; `app/ponto/services/ausencia.py:808`; `app/ponto/models.py:877`)

A devolução de registro já existente preserva seu valor, sem `save`, de modo que repetir a resposta não converte atestado antigo 720 em zero. (`app/ponto/services/ausencia.py:294`)

## 05/11 21:09 · sessao 052f591a4700 · H12H06-SEPARA-TRAVA · censo · origem=Code

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 05/11 21:05 · sessao a77a07cefa5d · - · - · origem=Ronald (tmux)

Confirmado: o ramo `adesao is None` apenas lavra/carimba `CelulaDia`, condicionado a `apply_`, e retorna antes das emissões: `app/ponto/services/cartorio.py:1162`, `:1207`, `:1242`, `:1264`, `:1268`. A linha `:1223` é comentário.

Esse ramo não preenche `out['cods']`; o ramo normal preenche em `cartorio.py:1426`. Cluster e fábrica posteriores dependem dessas datas (`:986`, `:990`, `:996`). No signal, `julgar_celula` passa somente `[cel]` (`:1989`); assim, o ramo nunca-bateu puro não alcança Colaborador ou Chamado pelo cartório.

FKs confirmadas:

- `CelulaDia.ausencia → Ausencia`, `SET_NULL`: `app/escala/models.py:1796`, `:1797`.
- `Ausencia.documento_referencia → self`, `SET_NULL`: `app/ponto/models.py:789`, `:790`.
- `Ausencia.chamado_origem → ChamadoColaborador`, `SET_NULL`: `app/ponto/models.py:784`, `:785`.

Essas declarações incluem atualizações referenciais no escopo das exclusões; SQL emitido, ordem do Collector, posição desses updates relativamente ao `post_delete` e primeira aquisição efetiva de travas: **[nao verificado]**.

## 05/11 21:05 · sessao b5677bb09b8e · H12H06-A · verificador · origem=Code

**Parecer: reprovo A como concluído.** H12 está coerente por inspeção. Em H06, falta aprovada com zero minutos ainda pode produzir **um dia de falta no TXT**, embora o fechamento compute zero horas; também sobrevivem leitores financeiros com regra própria de parcialidade. (`app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:665`)

Execução da suíte, RED/GREEN na base e ocorrência/impacto em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

O censo abaixo distingue quem fornece os valores de quem efetivamente os persiste: as portas recebem tipo/minutos/status; `criar_ausencia`, `editar_ausencia_registro`, `mudar_estado` e `Ausencia.save` fazem a gravação. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:792`, `app/ponto/models.py:868`)

| Campo/valor | Escritores | Leitores |
|---|---|---|
| `Ausencia.minutos` | Default zero; criação recebe `minutos`; edição recebe/zera; `save` deriva falta aprovada zero. (`app/ponto/models.py:755`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`) | Rateio da falta, fechamento, efeitos, crédito parcial, cobertura, apresentação/API/CSV/auditoria, discriminados abaixo. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2600`) |
| `Ausencia.status` | Criação usa status fornecido ou `estado_inicial`; transição por `mudar_estado`; rejeição em lote de férias usa `update`. Migration histórica 0057 também escreve status. (`app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:1265`, `app/ponto/migrations/0057_a4_backfill_pendente.py:43`) | Derivação, cobertura, precedência, decisão visual, cobrança, filtros/telas, relatórios e vigias. (`app/ponto/models.py:877`, `app/ponto/turnos.py:2514`, `app/ponto/precedencia.py:273`, `app/ponto/services/dia_decidido.py:481`, `app/chamados/services/silencio_ausencia.py:77`, `app/ponto/services/vigia_ausencia.py:108`) |
| `Ausencia.tipo` | Criação e edição; os dois materializadores fornecem o tipo. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`, `app/chamados/services/materializacao.py:485`, `app/chamados/services/materializacao.py:521`) | Derivação literal de `falta`, catálogo/efeito, cobertura/range, precedência, leitores financeiros, cobrança e apresentação. (`app/ponto/models.py:877`, `app/ponto/models.py:842`, `app/ponto/turnos.py:2532`, `app/ponto/precedencia.py:381`, `app/ponto/services/fechamento.py:685`, `app/chamados/services/materializacao.py:602`) |
| `Colaborador.data_demissao` | Agendamento, desligamento efetivo e reativação, persistidos pela porta de colaborador. (`app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/colaborador.py:32`) | Predicado H12, prontidão, recortes de grade/cobrança/universo, ciclo do desligamento e apresentação, discriminados abaixo. (`app/folha/export.py:60`, `app/folha/export.py:1060`, `app/ponto/services/fechamento.py:423`, `app/escala/services/leitor_celula.py:351`, `app/folha/views.py:658`) |
| Chave `rescisao` | Somente `montar_resumos`. (`app/ponto/services/fechamento_lista.py:109`) | Linha/cor/badge e contador da view; o atributo HTML alimenta o filtro. (`app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:117`) |
| `n_rescisao` | Soma de `r['rescisao']` na view. (`app/ponto/views.py:1215`) | Pílula do fechamento. (`app/templates/ponto/fechamento.html:65`) |
| `_DECLARA_FALTA_MAPA` | Declaração em `chamados/models.py`. (`app/chamados/models.py:3018`) | Materializador da pergunta; o selo inspeciona sua AST. (`app/chamados/services/materializacao.py:468`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:50`) |
| `cobre_parte` | Atributo transitório preenchido na lista pela chamada ao juiz existente. (`app/ponto/views.py:2240`) | Duração apresentada na lista. (`app/templates/ponto/ausencias.html:163`) |
| `sai_pela_rescisao` | Definição única do predicado. (`app/folha/export.py:54`) | `classificar_export` e `montar_resumos`. (`app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`) |
| `janela_competencia` | Wrapper de `janela_fechamento`; corte vem de `Empresa.dia_inicio_competencia`. (`app/folha/export.py:47`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`) | Export, resumos, prontidão, prévia, PDFs, relatórios e comandos, discriminados abaixo. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:104`, `app/folha/export.py:1073`, `app/folha/previa.py:42`, `app/folha/validacao_pdf.py:42`, `app/relatorios/services.py:214`) |

Os fornecedores de `tipo/minutos/status` fora do diff também passam pela porta:

| Origem | Sítio |
|---|---|
| Lançamento administrativo, colaborador e API | `app/ponto/views.py:2412`, `app/ponto/views.py:2533`, `app/api/views.py:1820` |
| Veredito da célula e copiloto de ausência | `app/ponto/services/veredito_celula.py:315`, `app/ponto/services/checar_ausencia.py:46` |
| Regularização e abandono provisório | `app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76` |
| Espelho de férias | `app/ferias/models.py:385`, `app/ponto/services/ausencia.py:1186` |
| Edição de tipo/minutos | `app/ponto/views.py:2791` |
| Decisões individuais/em lote | `app/ponto/views.py:85`, `app/ponto/views.py:2287`, `app/ponto/views.py:2316`, `app/ponto/views.py:2640`, `app/ponto/views.py:2698` |
| Reverter rejeição; rejeitar férias | `app/ponto/views.py:2581`, `app/ferias/services.py:356` |
| Rejulgar status e rederivar fim | `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46` |

Os demais leitores de **minutos**, inclusive fora do diff, são:

- Propriedade `horas`, admin e fotografias de auditoria. (`app/ponto/models.py:865`, `app/ponto/admin.py:44`, `app/ponto/services/ausencia.py:446`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`)
- Palavra/cartão/histórico, payload da API e CSV. (`app/ponto/services/dia_decidido.py:423`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`)
- Juiz de parcialidade, lista e drawer. (`app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`, `app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:149`, `app/templates/ponto/partials/painel_ausencia.html:299`)

Os demais leitores de **tipo/status** concentram-se nestes sítios:

| Domínio | Leitores fora do diff |
|---|---|
| Cobertura, sobreposição e afastamento | `app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2521`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2705`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2793`, `app/ponto/turnos.py:3087` |
| Propriedades e apresentação | `app/ponto/models.py:711`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/services/dia_decidido.py:453`, `app/ponto/services/dia_decidido.py:476`, `app/api/views.py:1873`, `app/ponto/admin.py:44` |
| Precedência, grade, furo e cartório | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:339`, `app/escala/servico_jornada.py:340`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/ponto/services/cartorio.py:355` |
| Chamados e cobrança | `app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/historico.py:129`, `app/escala/services/furos_diarios.py:223` |
| Relatórios, calendário, inteligência e férias | `app/relatorios/views.py:197`, `app/relatorios/views.py:277`, `app/colaboradores/services/calendario.py:674`, `app/colaboradores/services/detalhe.py:104`, `app/ponto/services/alertas_ausencia.py:48`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ferias/models.py:144` |
| Vigias e comandos | `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:104`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:115`, `app/ponto/services/vigia_ausencia.py:120`, `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`, `app/inteligencia/management/commands/calcular_scores.py:12`, `app/inteligencia/management/commands/gerar_alertas.py:55` |

Para **`data_demissao`**, além dos leitores financeiros já citados, há leitores de disponibilidade, cadastro, julgamento, holerite e ciclo do vínculo. (`app/ponto/utils.py:180`, `app/escala/services/cadastro_realidade.py:444`, `app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:177`, `app/ponto/supra_juiz.py:194`, `app/holerite/matriz.py:88`, `app/colaboradores/services/vinculo.py:92`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:41`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:55`)

Os demais consumidores de **`janela_competencia`** incluem ausências, grade, férias, retenção, histórico da folha e serviços/comandos auxiliares. (`app/folha/export.py:166`, `app/folha/export.py:185`, `app/folha/export.py:337`, `app/folha/export.py:486`, `app/folha/export.py:1266`, `app/folha/views.py:529`, `app/folha/views.py:588`, `app/folha/services/vigia_pausa.py:123`, `app/folha/services/sem_furo.py:47`, `app/relatorios/cartao_pela_celula.py:24`, `app/ponto/services/vigia_pareador_celula.py:59`, `app/ponto/management/commands/censo_col900.py:89`, `app/ponto/management/commands/gerar_celulas.py:176`, `app/ponto/management/commands/lavrar_status_competencia.py:40`, `app/ponto/management/commands/mapa_divergencia.py:61`, `app/ponto/management/commands/mapa_divergencia.py:93`)

**Sim, sobraram regras próprias contraditórias:** os leitores financeiros de parcialidade; o TXT que conta falta zero; e apresentação que ainda ensina `>0 = parcial`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`, `app/templates/ponto/lancar_ausencia.html:70`)

**2. Condições exatas de `Ausencia.save` e caminhos de gravação**

A derivação acontece em **qualquer `save`**, sem exigir detectar um flip, quando simultaneamente:

- `status == 'aprovada'`;
- `tipo == 'falta'`;
- `not self.minutos`.

Ela soma o juiz do previsto, inclusivamente, de `data` até `data_fim or data`. Valores não zero são preservados. (`app/ponto/models.py:877`, `app/ponto/models.py:882`, `app/ponto/models.py:885`)

Para uma **falta nova** dos dois materializadores, não encontrei bypass: ambos omitem minutos, a porta aplica zero, instancia `Ausencia` e chama `save()` inteiro. A aprovação posterior inclui `minutos` e demais derivados em `update_fields`. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:818`)

Há duas distinções necessárias:

- **Atestado aprovado zero não deriva**, porque o teste exige tipo literal `falta`. Isso é o comportamento introduzido pelos sítios, não um bypass de criação. (`app/ponto/models.py:877`, `app/chamados/services/materializacao.py:523`)
- **Registro existente pode ser devolvido sem `save`**, inclusive uma falta já aprovada com zero. Portanto repetir a materialização não garante rederivação de legado. (`app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:299`)

O `update` de produção identificado rejeita exclusivamente férias; o admin é somente leitura; os receivers de ausência julgam/reconciliam consumidores. Não encontrei `bulk_create` ou `update` que aprove faltas oriundas desses sítios. Ausência absoluta de SQL dinâmico ou chamadas não atribuíveis: **[não verificado]**. (`app/ponto/services/ausencia.py:1265`, `app/ponto/admin.py:38`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/core/censo_escritas.py:43`)

Para **atestado aprovado com efeito ABONA no catálogo**:

| Leitor | Zero | 720 |
|---|---|---|
| Juiz de cobertura | Dia inteiro | Dia inteiro. (`app/ponto/turnos.py:2598`) |
| Lista | “1 dia” | “1 dia”. (`app/ponto/views.py:2240`, `app/templates/ponto/ausencias.html:163`) |
| Fechamento | Justifica o dia; não soma crédito parcial | Soma 720 como crédito e pula justificação do dia. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`) |
| `efeitos_da_ausencia` | Justifica o dia | Crédito 720; pula justificação. (`app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`) |
| `creditos_por_dia` | Nenhum crédito | Crédito 720 no dia inicial do registro. (`app/ponto/services/credito_parcial.py:24`) |
| Teto de pontualidade | Dia inteiro | Também dia inteiro, pelo juiz — divergindo da regra de justificação acima. (`app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2689`, `app/ponto/services/fechamento.py:683`) |
| Drawer | Oculta detalhe de minutos; não pede período parcial | Mostra 720; também não pede período parcial. (`app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:296`, `app/ponto/views.py:2084`) |
| Palavra do dia | Sem duração numérica | Acrescenta 12h. (`app/ponto/services/dia_decidido.py:230`, `app/ponto/services/dia_decidido.py:488`) |
| API/CSV/admin | Valor cru zero | Valor cru 720. (`app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`, `app/ponto/admin.py:44`) |

O TXT efetivo recebe os eventos do fechamento e as datas de falta; não lê diretamente minutos de atestado nesse laço. Assim, a diferença zero/720 chega a ele pelos resultados anteriores do fechamento. (`app/folha/export.py:722`, `app/folha/export.py:732`)

**3. Folga, sem escala e sem célula**

Sem escala vigente, sem tipo, com `eh_dia_trabalho(...) is not True`, ou sem marcos de início/fim, o juiz devolve zero; a falta aprovada grava esse zero após passar pela derivação. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:887`)

**Sem célula não implica zero:** a autoridade existente pode consultar o cálculo do vínculo/template. O diff reutiliza essa autoridade; não acrescenta fallback próprio. (`app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/utils.py:2290`, `app/ponto/models.py:881`)

O contraexemplo relevante é:

1. Os materializadores pedem `retornar_mesmo_tipo`; o veto de precedência só é executado no modo `veto`. Logo esses sítios não passam por esse veto ao criar a linha. (`app/chamados/services/materializacao.py:490`, `app/chamados/services/materializacao.py:526`, `app/ponto/services/ausencia.py:377`)
2. A falta pode nascer aprovada zero sem escala — situação expressamente aceita pelo teste alterado. (`app/api/tests/test_falta_na_resposta.py:44`, `app/api/tests/test_falta_na_resposta.py:48`)
3. O fechamento soma zero minutos/horas. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:987`)
4. O TXT seleciona o registro por tipo/status/data e conta `len(datas)`: **um dia**, se o colaborador entrar no crivo e existir a rubrica. (`app/folha/export.py:87`, `app/folha/export.py:160`, `app/folha/export.py:218`, `app/folha/export.py:732`)
5. Em folga, a grade mantém explicitamente **folga**, apesar da falta aprovada; o seletor de datas do TXT não consulta essa decisão. (`app/escala/utils.py:1227`, `app/escala/utils.py:1231`, `app/folha/export.py:181`)

Portanto, **sim: zero significa nada nas horas, mas pode significar um dia no TXT**. Essa divergência impede certificar a migração dos leitores de H06. (`app/ponto/services/fechamento.py:866`, `app/folha/export.py:218`)

**4. H12: pergunta e janela**

Os quatro sítios pedidos estão alinhados: predicado único `data_demissao is not None and data_demissao <= fim`, classificador, chave dos resumos, linha e contador. (`app/folha/export.py:60`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`, `app/templates/ponto/_fech_linha.html:2`, `app/ponto/views.py:1215`)

A janela também coincide: classificador usa a empresa recebida; resumos usam `colab.empresa`, com cache por empresa; ambos chegam a `janela_fechamento`. Portanto respeitam cortes distintos, sem fixar 21 nesses leitores novos. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:103`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`)

Os baldes de rescisão de `folha/views`, relatórios e retrato acompanham o classificador. (`app/folha/views.py:589`, `app/folha/views.py:594`, `app/relatorios/views.py:1658`, `app/relatorios/views_pend.py:58`, `app/folha/fora_da_folha.py:119`, `app/folha/retrato_folha.py:1157`)

Persistem duas diferenças, com perguntas distintas:

- `prontidao` chama de `"rescisao"` a cegueira de quem saiu **antes do início**; não usa o predicado H12. Isso não decide entrada no TXT, mas o vocabulário não representa a mesma classificação. (`app/folha/export.py:1060`, `app/folha/export.py:1081`, `app/folha/export.py:1083`)
- O raio-X ainda escreve `†deslig.` pela mera presença da data, inclusive futura; seu balde de rescisão, entretanto, lê o juiz. (`app/folha/views.py:658`, `app/folha/views.py:594`)

O corte de aposentadoria continua pendente na cópia: o universo exclui desligados anteriores à janela, mas o classificador ainda consulta seus fechamentos gravados; não existe `encerrado` nas escolhas e o vigia continua incluindo as projeções nulas. Isso é dívida anterior, não regressão do predicado H12 de A. (`app/docs/CORTES.md:270`, `app/ponto/services/fechamento.py:423`, `app/folha/export.py:777`, `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1680`)

**5. Classes de reprovação**

| Classe | Verificação |
|---|---|
| Segundo juiz | Não identifiquei juiz novo no diff: H12 extrai uma condição compartilhada; H06 chama o previsto e a cobertura existentes. K1/K2 continuam registrados em `classificar_export`. (`app/folha/export.py:54`, `app/folha/export.py:858`, `app/ponto/views.py:2240`, `app/core/juizes.py:298`) |
| Segundo escritor | Os sítios alterados continuam pela porta; aprovação persiste os derivados do modelo. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:818`) |
| Relógio fora da borda | Não aparece leitura nova de relógio nos hunks funcionais: o predicado H12 recebe fim; a derivação usa as datas da ausência. (`app/folha/export.py:54`, `app/ponto/models.py:882`, `app/ponto/services/fechamento_lista.py:104`) |
| Derivação paralela | Os dois literais foram removidos, mas sobreviveram os julgamentos financeiros de parcialidade e o contador de dias independente do valor zero. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`) |


[truncado em 20000 de 24439 caracteres]

## 05/11 21:05 · sessao 2b546d724cfa · - · - · origem=Ronald (tmux)

Consolidação das direções inversas; todas são estáticas, execução/primeira aquisição concreta de linhas **[nao verificado]**.

| Par/direção | Testemunha na mesma transação |
|---|---|
| **Colaborador → Empresa** | `normalizar_identificadores.py:106` atomic → C U `:109` → `colaboradores/services/colaborador.py:88` → E U command `:112` → `colaboradores/services/estrutura.py:27`. |
| **Empresa → Colaborador/Ausencia** | Não encontrei testemunha; zero global **[nao verificado]**. Escritores e exclusões administrativas detalhados abaixo. |
| **Chamado/Colaborador → Ausencia nova** | `regularizacao.py:89` atomic → Justificativa I `:91` ou U `:106` → receiver `ponto/signals.py:162/:165` → cartório K, com possíveis L de Chamado/Colaborador → loop `regularizacao.py:110/:111/:118` → `ausencia.py:461` → I `:418` ou reabertura U via `:291`. |
| **Chamado/Colaborador → Ausencia de dia seguinte** | `marcar_abandono_provisorio.py:22` atomic abrangente → loops pessoas `:43` e dias `:70` → garantir `:76`; K do fato anterior precede Ausencia seguinte. Condicionado ao ramo de K que efetivamente trava e à existência de outra iteração. |
| **Chamado/Colaborador → Ausencia de férias seguinte** | Atomic externo dos serviços de férias → `ausencia.py:1182` loop → criar `:1186`; K do dia anterior precede I/U seguinte. Atomic, override e famílias específicos pertencem ao inventário do agente de escritores. |
| **Chamado/Colaborador → nova Ausencia após D antiga** | `ferias/services.py:375` atomic → apagar `:385` → `ausencia.py:1254` D → K → `ferias/services.py:387` ag.save → `ferias/models.py:437` sync → `ausencia.py:1186` I em data nova. Condicionado ao K e ao dia novo faltante. |
| **Chamado/Colaborador → Ausencia de outra irmã** | `ponto/views.py:2631` atomic → loop `:2632/:2633`; K após primeiro U `ausencia.py:819` pode travar Chamado/Colaborador antes do U da próxima irmã. |
| **FechamentoMensal → Ausencia de outra irmã** | Mesmo atomic/loop `views.py:2631/:2632/:2633`: `ausencia.py:895` → U `:819`, carimbos U `:911`, invalidar `:930` → FM U `fechamento.py:1640` → próxima irmã U `ausencia.py:819`. Requer ≥2 irmãs e FM atingida. |
| **Colaborador → FechamentoMensal** | `justificativa.py:125` atomic → registrar batida `:173` → I `registro_batida.py:152` ou `:157` → onboarding `colaboradores/signals.py:87/:102` → C U `colaboradores/services/colaborador.py:61` → `Colaborador.save`, `models.py:439` → invalidar `justificativa.py:200` → FM U `fechamento.py:1640`. Guards onboarding: batida criada `signals.py:94`, onboarding false `:97`, usuário com last_login `:99`. |
| **FechamentoMensal → Colaborador** | `views.py:1793` atomic → invalidar `:1806` → FM U `fechamento.py:1640` → se saída informada e sem batida gerada `views.py:1809`, registrar `:1816` → I `registro_batida.py:152/:157` → mesmo receiver onboarding e C U acima. O K anterior do `j.save()` em `views.py:1800` pode já ter adquirido C; primeira aquisição C posterior a FM **[nao verificado]**. |
| **Colaborador → CelulaDia** | J1 acima: onboarding C U durante registrar batida `justificativa.py:173`; após retorno, `j.save()` `:193` → `ponto/signals.py:162/:165` → K → lavra `cartorio.py:1822` → `portas/celula.py:682` U. Requer guards onboarding e julgamento que efetivamente lavra. |

**Regravação, separada de primeira aquisição:** no `decidir`, K após U `ausencia.py:819` pode travar C/Chamado antes do segundo U `:911`; a própria Ausencia já estava adquirida. Também `_reabrir` pode fazer saves sucessivos `ausencia.py:1124/:1125/:1132`, cada um com K, mas isso não prova primeira aquisição inversa.

**Empresa — fechamento das entradas encontradas:**

- `atualizar_empresa` efetua `empresa.save()` em `app/colaboradores/services/estrutura.py:20`; normalização efetua queryset update `:27`.
- Formulário chama atualização em `app/colaboradores/views.py:521`, sem atomic próprio abrangente na função `:485`.
- `diff_janela_he` tem atomic `app/ponto/management/commands/diff_janela_he.py:74`, E U `:101/:103`, depois `_tot` `:102/:104` → leitura `autoridade_do_periodo` `:59`. Não demonstra C/A U/L.
- `diff_janela_he_total.py:106` atualiza E pela mutação executada dentro de `app/ponto/services/diff_frota.py:72` atomic, callback `:86`; cálculo posterior recebe `somente_leitura=True` `:87/:88`. Não demonstra C/A U/L.
- `aplicar_janela_he_total.py:119` atualiza E, sem atomic abrangente próprio; delegação de recálculo começa em `:137`. Não uni as transações.
- `EmpresaAdmin` herda `ModelAdmin` padrão em `app/colaboradores/admin.py:7/:8`; implementação SQL/atomic/cascatas do Django **[nao verificado]**. `Colaborador.empresa` usa PROTECT em `app/colaboradores/models.py:321`.
- Admin de Colaborador `app/colaboradores/admin.py:48` e de Ausencia `app/ponto/admin.py:38` usam SoLeitura; delete negado por `app/core/admin.py:65/:66`.

**Caminho excluído da prova de mesma transação:** `responder_pergunta_disputa_core` chama `_ressuscitar_por_resposta` em `app/chamados/services/disputa_emissao.py:1326` → Chamado U `app/chamados/services/arquivo.py:208`, depois materializa Ausencia em `disputa_emissao.py:1330` → `materializacao.py:519`. Não há atomic no core definido em `disputa_emissao.py:1247`; as chamadas web/API `app/chamados/views.py:1283` e `app/api/views.py:2643` não forneceram atomic abrangente. Os wrappers transacionais `declaracao_texto.py:302/:605` aceitam/produzem HH:MM (`:276`, `:368`, `:641`), não os escapes de ausência. Atomic externo efetivo desse caminho **[nao verificado]**.

## 05/11 21:04 · sessao fb67b25b570c · - · - · origem=Ronald (tmux)

Inventário da porta e chamadores, sem expansão do cartório.

**Escritores `Ausencia` no código de aplicação**

| Operação | Local | Atomic comprovado |
|---|---|---|
| I, criação nova | `app/ponto/services/ausencia.py:385`, save em `:418` | Próprio em `:417`; inclui receivers desse save. |
| U, edição | `app/ponto/services/ausencia.py:508` | Não possui atomic próprio; chamador HTTP em `app/ponto/views.py:2784` também não o delimita. Atomic abrangente nesse caminho: [nao verificado]. |
| U, prazo após rejulgamento | `app/ponto/services/ausencia.py:536` | Não possui atomic próprio; chamada de edição em `:513`, command em `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`. Atomic abrangente nesses chamadores: [nao verificado]. |
| U, estado | `app/ponto/services/ausencia.py:819` | Herda aprovação de irmãos `app/ponto/views.py:2631` → `:2633` → helper `:85` → `ausencia.py:895`; também herda wrappers quando `_reabrir` é chamado neles, conforme abaixo. |
| U, documento | `app/ponto/services/ausencia.py:843` | Sem atomic próprio; HTTP em `app/ponto/views.py:2151` e API em `app/api/views.py:1853`, `:3130`. Atomic abrangente: [nao verificado]. |
| U, carimbos/período | `app/ponto/services/ausencia.py:911` | Aprovação de irmãos sob `app/ponto/views.py:2631`/`:2633`; chama fechamento em `ausencia.py:930`. |
| U, reabertura: documento/descrição/estado/prazo | `app/ponto/services/ausencia.py:1114`, `:1124`, `:1125` → `:819`, `:1132` | **O ramo de reabertura sai em `:291`, antes do atomic próprio `:417`.** Só compartilha transação quando existe wrapper externo comprovado. |
| D, férias inativas | `app/ponto/services/ausencia.py:1174` | Herda wrapper do gravador de `AgendamentoFerias`; cadeia `app/ferias/models.py:436` → `:437` → `:385` → `ausencia.py:1172`/`:1174`. |
| D, férias fora de desejadas | `app/ponto/services/ausencia.py:1197` | Ramo escrito, mas execução não demonstrada: `existentes` já está no intervalo atual em `:1168`, e `desejadas` percorre exatamente esse intervalo em `:1178`–`:1181`. |
| D, janela de férias | `app/ponto/services/ausencia.py:1254` | Correção sob `app/ferias/services.py:375` → `:385`. Também override de delete em `app/ferias/models.py:439` → `:441`; atomic abrangente desse override isolado: [nao verificado]. |
| U em massa, rejeitar férias | `app/ponto/services/ausencia.py:1267` | `app/ferias/services.py:352` → `:356`; **não emite save/signal**, explicitado em `ausencia.py:1259`/`:1260`. |
| D, ensaio | `app/ponto/services/ausencia.py:1274` | `app/ponto/management/commands/zerar_dados_operacionais.py:80` → `:90`; guarda de ensaio em `:27`, reforçada por `ausencia.py:1272`/`:1273`. |
| U, rederivar fim | `app/ponto/services/ausencia.py:1280` | Chamador em `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`; não delimita atomic próprio. Atomic abrangente: [nao verificado]. |

`Ausencia.save` termina no `super().save` em `app/ponto/models.py:906`, sem atomic explícito no override iniciado em `:866`. Não atribuir retenção da trava da ausência durante receivers a saves isolados fora dos wrappers comprovados. `app/ponto/models.py:866`, `:906`.

**Todas as famílias de criação com atomic comprovado**

- Toda criação realmente nova entra em `app/ponto/services/ausencia.py:417` → `:418`, seja pela HTTP administrativa `app/ponto/views.py:2405`, HTTP colaborador `:2526`, API `app/api/views.py:1820`, IA `app/core/views_ia.py:985` → `app/ponto/services/checar_ausencia.py:46`, ou pelos chamadores abaixo.
- Regularização: `app/ponto/services/regularizacao.py:89` → justificativa I/U `:91`/`:106` → laço `:111` → `:118` → `app/ponto/services/ausencia.py:461` → criação I `:418` **ou reabertura U `:291`**.
- Abandono: decorator `app/ponto/management/commands/marcar_abandono_provisorio.py:22` → laço de colaboradores `:43` → laço de dias `:70` → `:76` → `ausencia.py:461` → I `:418` ou U `:291`; rollback sem `--apply` em `marcar_abandono_provisorio.py:98`.
- Veredito falta/justificado: `app/ponto/services/veredito_celula.py:204` → ramo `:309` → `:315` → I `ausencia.py:418` ou U `:291`; depois fecha fio em `veredito_celula.py:325` e lavra célula em `:330`.
- **Materialização de escolha `declara_falta`:** `app/chamados/services/materializacao.py:1142` → ramo escolha `:1148` → `:1044`/`:1045` → `:484` → I `ausencia.py:418` ou U `:291`; resposta sem tipo em `materializacao.py:468`/`:469` e ausência de data em `:472`–`:478` não escrevem ausência.
- Materialização de escape da resposta: `app/chamados/services/disputa_emissao.py:1327` → `:1330` → `app/chamados/services/materializacao.py:519` → I `ausencia.py:418` ou U `:291`. **Esse core não tem atomic próprio**, e a reabertura U não fica protegida pelo atomic de criação nova. Wrapper abrangente dos HTTP web/API `app/chamados/views.py:1283`, `app/api/views.py:2643`: [nao verificado]. Os chamadores atômicos de declaração de hora passam uma hora escolhida, em `app/chamados/services/declaracao_texto.py:368`, `:641`; alcance do ramo de escape nesses chamadores: [nao verificado].
- Férias: `app/ferias/services.py:277` → `:298`; `:315` → `:321`; wrapper adicional `:156` → `:157`; remarcar `:399` → `:406`; corrigir `:375` → `:387`; aprovar solicitação `app/ferias/views.py:717` → `:731` → `app/ferias/services.py:447`. Todos seguem `app/ferias/models.py:436` → `:437` → `:385` → `app/ponto/services/ausencia.py:1186` → I `:418` ou U `:291`.

**Repetições que acrescentam direções aos pares**

- Aprovação de irmãos não acaba na primeira ausência: `app/ponto/views.py:2631` mantém o atomic em todo o laço `:2632`; cada `:2633` executa `app/ponto/services/ausencia.py:819`, eventualmente `:911`, e fechamento em `:930` → `app/ponto/services/fechamento.py:1640`. Havendo ao menos dois irmãos gravados e fechamento atingido no primeiro, existe **Fechamento → Ausencia seguinte**, além de Ausencia → Fechamento. Existência dos irmãos/linhas de fechamento em produção: [nao verificado].
- Os laços de regularização `app/ponto/services/regularizacao.py:111` e abandono `app/ponto/management/commands/marcar_abandono_provisorio.py:43`, `:70` também mantêm uma transação ao gravar ausências subsequentes. Se o cartório da primeira ausência adquire Colaborador/Chamado/CelulaDia, essas operações precedem a próxima gravação de Ausencia. O ramo exato do cartório deve ser juntado às cadeias deste inventário; execução de produção: [nao verificado].
- O espelho ativo de férias percorre dias em `app/ponto/services/ausencia.py:1182`, criando faltantes em `:1186`; sob os wrappers de férias acima, o cartório de um dia precede a ausência seguinte.
- Cancelamento administrativo: atomic `app/ferias/services.py:352` → U Ausencia `:356`/`app/ponto/services/ausencia.py:1267` **sem receiver** → `Agendamento.save :362` → sync → D Ausencia `ausencia.py:1174` com receiver.
- Correção de férias: atomic `app/ferias/services.py:375` → D da janela antiga `:385`/`ausencia.py:1254` com receivers → agendamento `:387` → sync de novas ausências `ausencia.py:1186`, ou reabertura `:291`.

**Caminhos de ensaio e migrations**

- Ensaio: `app/ponto/management/commands/zerar_dados_operacionais.py:80` → D Chamado `:84` → D Ausencia `:90` → D Fechamento `:93`. É um caminho separado, condicionado à guarda `:27`; não descartá-lo do universo da cópia nem apresentá-lo como executável em produção sem dado. A FK `Ausencia.chamado_origem` declara `SET_NULL` em `app/ponto/models.py:784`/`:785`; SQL/ordem internos do Collector: [nao verificado].
- Migrations históricas fazem U em massa: `app/ponto/migrations/0057_a4_backfill_pendente.py:43`, `:65`; `0058_a4_relogio_recomeca.py:42`, `:63`; `0059_a4_d0_no_primeiro_dia_do_cron.py:36`. Registram `RunPython` em `0057:76`, `0058:75`; não há save para expandir pelo cartório nesses updates. Atomic efetivo do runner e execução no HEAD de produção: [nao verificado].

**Limites para consolidar**

`AgendamentoFerias.save` roda sync mesmo quando só muda confirmação ou status: `app/ferias/services.py:491`, `:500`, `:504`, `:522` → `app/ferias/models.py:436`/`:437`. Esses serviços não têm atomic próprio; **cada I novo** continua com o atomic `ausencia.py:417`, mas uma transação única envolvendo agendamento e todos os dias nesses chamadores é [nao verificado].

A FK de Colaborador em Ausencia é `PROTECT`, não cascata de exclusão: `app/ponto/models.py:743`/`:744`. A autorreferência `documento_referencia` declara `SET_NULL`: `:789`/`:790`; ausência de save/signal no SQL interno dessa atualização e sua ordem efetiva dependem do Django instalado: [nao verificado].

## 05/11 21:04 · sessao a77a07cefa5d · - · - · origem=Ronald (tmux)

Expansão síncrona comum: `Ausencia.save/delete → app/ponto/signals.py:147/:152 → _julgar_dias :85/:93 → cartorio.julgar_celula app/ponto/services/cartorio.py:1960/:1989 → julgar_colab :948 → _julgar_colab_corpo`.

Guards: save `raw` para em `signals.py:145`; cartório já ativo para em `signals.py:90`; futuro/sem célula para em `cartorio.py:1974/:1977`; hoje com trabalho e turno ainda não encerrado para em `:1982/:1986`. A seleção dos dias usa `signals.py:123/:124`, com filtro por células quando há mais de três dias em `:125/:127/:128`. Impressão igual pode impedir escrita em `cartorio.py:1203` e `:1362`; ausência de dia da grade para em `:1366`.

**Colaborador L — quatro cadeias confirmadas.**

1. Emissão no corpo: `cartorio.py:1740` ou `:1778 → detectar_ausencias.py:693 → chamados/models.py:863/:866`. Primeiro ramo exige `_cobraria_furo` (`cartorio.py:1696`), não afastado (`:1714`), vínculo/dia de trabalho/marco inicial (`:1717/:1718/:1721`), `apply_` e fora de parcial só lista (`:1725`). Segundo exige `FURO_PARCIAL`, não desligado/fora do piso (`:1762`), não afastado (`:1767`), marcos parciais (`:1769`), sem vivos/fora de parcial só lista (`:1777`). Vetos anteriores ao lock: `detectar_ausencias.py:600/:604/:616/:628/:632/:659`. O lock ocorre mesmo quando `abrir` encontra chamado existente e retorna pelo dedup (`models.py:866/:867/:885`).
2. Fio do emissor: `detectar_ausencias.py:800/:801 → disputa_emissao.py:894/:943 → models.py:866`; condicionado a órfãos restantes e inexistência de disputa aberta (`disputa_emissao.py:895/:942`).
3. Pós-lavra, cluster: `cartorio.py:1822 → portas/celula.py:849/:681/:682 → cartorio.py:990/:994 → cluster_espurio.py:130 → models.py:866`. Exige célula julgada (`cartorio.py:990`), colaborador ativo e escala ativa sem fim (`cluster_espurio.py:117/:119`), assinatura da ata (`:33/:35`), fora de parcial só lista (`:128`). O laço abre sobre `achados`, inclusive chamado existente (`:127/:130`), embora o relatório de pendências também calcule `a_abrir` (`:107`).
4. Pós-lavra, fábrica: `cartorio.py:997 → fabrica_da_celula.py:176 → :139 → disputa_emissao.py:598/:504 → models.py:866`, no ramo sem disputa aberta (`disputa_emissao.py:467/:471/:502`). Mesmo com disputa existente, existe C2: fábrica `:139/:141 → disputa_emissao.py:606/:618/:389/:404/:408/:409 → reconciliador.py:1600/:900 → models.py:866`. Condições C2: `reconciliador.py:798/:802/:810/:822/:830/:840/:845/:859/:872/:876/:893/:896/:898`. Os gates da fábrica estão em `fabrica_da_celula.py:91/:93/:99/:102`; limbo entregue desvia para pauta em `:171/:172`.

**Colaborador U — FCM síncrono, adicional ao lock.**

`cartorio.py:1740/:1778 → detectar_ausencias.py:801 → disputa_emissao.py:876` (`notificar=True` por default) `→ :1123/:1124 → :1543 → fcm_utils.py:106/:93/:41 → colaboradores/fcm.py:61 → colaboradores/services/colaborador.py:88`.

Exige órfãos restantes (`detectar_ausencias.py:800`), não autoencerrar antes do envio (`disputa_emissao.py:1116/:1122`), push não silenciado (`:1521/:1524`), token presente (`fcm_utils.py:104`), erro Firebase considerado inválido (`:89/:90/:92`) e colaboradores ainda com token (`colaboradores/fcm.py:55/:58`). O queryset da limpeza usa token, podendo abranger várias pessoas: `fcm_utils.py:41`, `colaboradores/fcm.py:55/:61`. Resposta real do provedor e linhas atingidas: **[nao verificado]**.

A fábrica usa `push_disputa_silenciado` (`fabrica_da_celula.py:175`), portanto sua chamada de envio para em `disputa_emissao.py:1521/:1524`. `push_supervisao` do emissor (`detectar_ausencias.py:778`) chama apenas o caminho webpush (`push_utils.py:110/:111`), cuja limpeza exclui `PushSubscription` (`:51`), sem atualizar Colaborador. [nao verificado]

**Chamado L/U/I — antes e depois da célula.**

- Antes da lavra, reconciliações: `cartorio.py:1488/:1501/:1522/:1600/:1615 → chamados/models.py:616/:617/:638/:674/:675`, com U somente se gates permitirem (`:648/:651/:620 → :274/:279`). Os ramos são não protestam/não adiados (`cartorio.py:1482/:1488`), marcos inexistentes (`:1500/:1501`), códigos cobertura indevida/sem furo (`:1514/:1522`), duplicatas com fio morto (`:1591/:1599/:1600/:1615`).
- Emissão: `cartorio.py:1740/:1778 → detectar_ausencias.py:693 → models.py:866` seguido de nascimento I `:932`, ou renascimento `:883` conforme guards da porta. Espelhamento de marcos atualiza Chamado: `cartorio.py:1789/:1798 → detectar_ausencias.py:558 → contexto.py:100/:102`; só quando lista mudou (`detectar_ausencias.py:556`).
- Pós-lavra, adiados: `cartorio.py:952/:959/:963`; E2 pode fechar Disputa em `reconciliador.py:601/:602 → models.py:1865 → chamados/signals.py:28/:33/:42 → models.py:674/:279`.
- Pós-lavra, cluster abre/fecha Chamado (`cartorio.py:994 → cluster_espurio.py:124/:130`), e fábrica abre pai/C2 pelas cadeias acima.
- Receivers de fechamento de Chamado podem reabrir outro pai (`chamados/signals.py:92 → disputa_emissao.py:1628`) ou fechar Disputas (`:1695/:1696`) que voltam ao receiver de Chamado (`chamados/signals.py:42`). Não há reentrada infinita do cartório: `_tl.ativo=True` em `cartorio.py:926`, guard em `chamados/signals.py:174`, restauração em `cartorio.py:1075`.

**CelulaDia U — cadeia e ramos.**

Ramo sem batida na vida: `cartorio.py:1162/:1207/:1242/:1264 → portas/celula.py:849/:681/:682`; ramo normal: `cartorio.py:1799/:1802/:1822 → mesma porta`. `save=False` na lavra grava apenas em memória até `carimbar` (`cartorio.py:1802/:1822`; `portas/celula.py:816/:829/:849`). Idempotência pode retornar sem save (`portas/celula.py:838/:841`). Após o corpo, pode haver outras passadas (`cartorio.py:973/:1038`) e a precedência ainda pode salvar célula (`:1997 → :1949 → portas/celula.py:858/:863/:864 → :682`).

O save da célula insere somente evento quando há campos que acordam (`portas/celula.py:680/:683/:684 → :673`); não consome o evento ali. `CelulaDia → Colaborador` está demonstrado pelos ramos pós-lavra cluster/fábrica acima; `Colaborador → CelulaDia` está demonstrado pela emissão antes da lavra (`cartorio.py:1740/:1778 → models.py:866`, depois `cartorio.py:1822 → portas/celula.py:682`). Todos compartilham o atomic externo do escritor de Ausencia quando ele existe; a porta da célula tem atomic próprio em `portas/celula.py:681`.

**Empresa e FechamentoMensal.**

Não identifiquei lock/U de Empresa nem U de FechamentoMensal na expansão síncrona do cartório examinada; alcance além dessa expansão: **[nao verificado]**. Leituras da empresa no cartório estão em `cartorio.py:1145/:1676`; consulta de entrega na fábrica está em `fabrica_da_celula.py:187/:188`. Não constituem escrita.

O consumidor de Ausencia é posterior ao commit: `ponto/signals.py:276/:279` e `:283/:284 → :272`; suas chamadas em `:262/:269/:271` não justificam seta dentro do atomic emissor.

Invalidação de Fechamento conhecida é chamada explícita da porta de aprovação, não do receiver/cartório: escritor `ponto/services/fechamento.py:1635/:1640`; seu próprio comentário confirma chamadores dentro do atomic do ato em `:1626`. Deve entrar no inventário dos escritores/chamadores, que ficou com o outro agente.

## 05/11 21:01 · sessao 01ebc10b506c · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Falta aprovada com minutos=0 recebe respostas incompatíveis conforme o leitor.** O rateio devolve zero e o fechamento grava `horas_falta=0`; o TXT seleciona pela data/tipo/status e emite `len(datas)` dias, sem consultar minutos ou grade. Portanto, se o colaborador passar no crivo e houver rubrica, uma falta de um dia com zero minutos pode virar **1 dia de falta no TXT**. A lista também mostra “1 dia”. (`app/folha/export.py:87`, `app/folha/export.py:121`, `app/folha/export.py:160`, `app/folha/export.py:215`, `app/folha/export.py:732`, `app/ponto/services/fechamento.py:864`, `app/ponto/services/fechamento.py:987`, `app/templates/ponto/ausencias.html:163`)

- **Na folga, a grade mantém folga apesar da falta aprovada**, enquanto `datas_falta` continua contando o lançamento. Isso reforça a divergência anterior entre previsto/horas e dias do TXT. (`app/escala/utils.py:1227`, `app/escala/utils.py:1231`, `app/folha/export.py:169`)

- **Leitores financeiros ainda têm regra própria de parcial.** `TIPOS_PAGOS and minutos` classifica atestado de 720 minutos como crédito parcial e impede que justifique o dia; o juiz `cobre_parte_do_dia` considera tanto 0 quanto 720 como dia inteiro. O diff migra a lista, mas deixa esses leitores. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2583`, `app/ponto/turnos.py:2600`)

Comportamento de atestado aprovado:

| Leitor | Minutos=0 | Minutos=720 |
|---|---|---|
| Juiz de cobertura | Inteiro | Inteiro (`app/ponto/turnos.py:2598`) |
| Lista | “1 dia” | “1 dia” (`app/ponto/views.py:2240`, `app/templates/ponto/ausencias.html:163`) |
| Fechamento | Justifica dia; não soma crédito | Soma 720 de crédito; pula justificação (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`) |
| `efeitos_da_ausencia` | Justifica dia | Crédito 720; pula justificação (`app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`) |
| `creditos_por_dia` | Nenhum crédito | Crédito 720 no início do registro (`app/ponto/services/credito_parcial.py:24`) |
| Teto de pontualidade | Dia inteiro | Dia inteiro: ambos entram pelo mesmo juiz, criando resposta diferente da justificação anterior (`app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2689`, `app/ponto/services/fechamento.py:683`) |
| Drawer | Oculta campo de detalhes “Minutos”; não abre edição de período parcial | Mostra 720 em detalhes; não abre edição de período parcial (`app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:296`, `app/ponto/views.py:2084`) |
| Palavra/histórico | “Atestado (abonado)” | “Atestado 12h (abonado)” (`app/ponto/services/dia_decidido.py:230`, `app/ponto/services/dia_decidido.py:488`) |
| API/CSV | Devolve zero cru | Devolve 720 cru (`app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`) |

O TXT efetivo não consulta diretamente minutos de atestado: emite eventos do fechamento e faltas decididas. A função `datas_abonadas` conta dias sem olhar minutos, mas não localizei chamador de produção dela; não afirmar emissão atual da rubrica 8932. (`app/folha/export.py:201`, `app/folha/export.py:722`, `app/folha/export.py:732`; chamador de produção de `datas_abonadas`: [não verificado])

**Censo consolidado de leitores fora do diff:**

- Minutos: propriedade `horas`; admin; rateio de falta; fechamento; efeito da ausência; crédito parcial; palavra/cartão/histórico; payload API; CSV; fotografias de auditoria; drawer. (`app/ponto/models.py:865`, `app/ponto/admin.py:44`, `app/folha/export.py:121`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/dia_decidido.py:423`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`, `app/ponto/services/ausencia.py:446`, `app/templates/ponto/partials/painel_ausencia.html:125`)
- Tipo/status na cobertura: `_pelo_efeito`, cobertura unitária/período, sobreposição, afastamento e vigência. (`app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2521`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2705`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2793`, `app/ponto/turnos.py:3087`)
- Tipo/status no estado apresentado e portas: propriedades `esta_aberta`, `exige_documento`, `pode_aprovar`; decisão visual/histórico; painel/lista; filtros de aprovação/rejeição/cancelamento; API e admin. (`app/ponto/models.py:711`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/services/dia_decidido.py:453`, `app/ponto/services/dia_decidido.py:476`, `app/ponto/services/dia_decidido.py:514`, `app/ponto/services/dia_decidido.py:542`, `app/ponto/services/dia_decidido.py:602`, `app/ponto/views.py:2096`, `app/ponto/views.py:2185`, `app/ponto/views.py:2279`, `app/ponto/views.py:2313`, `app/ponto/views.py:2342`, `app/ponto/views.py:2603`, `app/ponto/views.py:2667`, `app/ponto/views.py:2684`, `app/api/views.py:1873`, `app/ponto/admin.py:44`)
- Precedência/grade/juízo de furo/cartório: catálogo vivo, `fatos_do_periodo`, falta classificada, grade, dispensas e sinais. (`app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:339`, `app/ponto/precedencia.py:408`, `app/escala/servico_jornada.py:340`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/ponto/services/cartorio.py:355`, `app/ponto/signals.py:124`, `app/ponto/signals.py:250`)
- Chamados/cobrança: materialização, devolução de absorvidas, silêncio de ausência, histórico, furos diários. (`app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/silencio_ausencia.py:65`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/historico.py:129`, `app/escala/services/furos_diarios.py:223`)
- Relatórios/calendário/inteligência/férias: relatórios por tipo/status; alertas médicos; calendário; resumo e consultas de inteligência; períodos de férias e detalhe de colaborador. (`app/relatorios/views.py:197`, `app/relatorios/views.py:277`, `app/ponto/services/alertas_ausencia.py:48`, `app/colaboradores/services/calendario.py:674`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ferias/models.py:144`, `app/colaboradores/services/detalhe.py:104`)
- Vigias/comandos: rejeitada/viva, aguardando decisão/documento, aprovada sem documento/decisor, tipo válido, ausência versus veredito, ausência em folga, ausência sem fim e inteligência. (`app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:104`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:115`, `app/ponto/services/vigia_ausencia.py:120`, `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/services/vigia_ausencia_veredito.py:54`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:23`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `app/inteligencia/management/commands/calcular_scores.py:12`, `app/inteligencia/management/commands/gerar_alertas.py:55`)
- Outra regra própria de apresentação: formulário ainda diz “>0 = parcial”, contrariando o juiz para 720; espelho ainda testa o status inexistente `pendente`. (`app/templates/ponto/lancar_ausencia.html:70`, `app/ponto/turnos.py:2600`, `app/templates/ponto/espelho.html:191`, `app/ponto/models.py:700`)

**Achado secundário preexistente de range:** `save` soma previsto dia a dia, mas o rateio financeiro divide igualmente por dias corridos; um range com previstos 420/420/0 vira 280/280/280. O teste novo só cobre três dias iguais e não detecta isso. Os dois sítios de chamados auditados criam dia único. (`app/ponto/models.py:882`, `app/ponto/models.py:885`, `app/folha/export.py:118`, `app/folha/export.py:121`, `app/chamados/tests/test_h06_falta_pela_grade.py:128`, `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:519`)

**Testes:** o teste de range não constitui RED próprio desta troca, pois chama a porta já existente sem passar pelos sítios alterados; os testes dos dois sítios e da lista atingem os comportamentos alterados. O novo caso de atestado chama diretamente o helper com status aprovado, enquanto o fluxo real de escape deixa o estado inicial decidir, normalmente aguardando documento. (`app/chamados/tests/test_h06_falta_pela_grade.py:76`, `app/chamados/tests/test_h06_falta_pela_grade.py:98`, `app/chamados/tests/test_h06_falta_pela_grade.py:110`, `app/chamados/tests/test_h06_falta_pela_grade.py:128`, `app/chamados/tests/test_h06_falta_pela_grade.py:142`, `app/chamados/services/materializacao.py:580`)

Não executei testes Django/banco nem escrevi arquivos; resultado de execução da suíte: [não verificado]. [nao verificado]

## 05/11 21:00 · sessao a3ff12ba523a · - · - · origem=Ronald (tmux)

Censo H06 de escritores concluído por leitura e AST, sem banco e sem gravação de arquivos. A varredura canônica atribuiu 14 escritas de `Ausencia`, todas em `app/ponto/services/ausencia.py`; o próprio censo documenta o limite das chamadas não atribuíveis (`app/core/censo_escritas.py:41`).

- **Derivação exata:** `status == 'aprovada'`, `tipo == 'falta'` e `not self.minutos`. Soma `minutos_previstos_do_dia` inclusivamente entre `data` e `data_fim or data`; não requer detectar mudança de estado e preserva números não zero (`app/ponto/models.py:877`).
- **Dois sítios curados:** omitem minutos e passam pela porta com default zero; a porta instancia e executa `save()` inteiro (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`).
- **Aprovação posterior:** `mudar_estado` inclui explicitamente `minutos`, `data_fim`, `dias_corridos` e `aprovada_em` em `update_fields`; portanto persiste a derivação (`app/ponto/services/ausencia.py:818`).
- **Edição para falta aprovada:** escreve o tipo, zera/recebe minutos e executa `save()` inteiro; deriva quando o valor enviado for zero (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`).
- **Atestado zero aprovado:** não entra na derivação, por teste literal do tipo; esse caminho existe por desenho (`app/ponto/models.py:877`, `app/chamados/services/materializacao.py:523`).
- **Bypass encontrado:** o único `QuerySet.update` atribuível de Ausencia rejeita exclusivamente férias; não cria/aprova falta ou atestado (`app/ponto/services/ausencia.py:1265`). Admin é somente leitura (`app/ponto/admin.py:38`). Os receivers julgam/reconciliam outros estados e não escrevem minutos/status/tipo de Ausencia (`app/ponto/signals.py:143`, `app/ponto/signals.py:250`).

Chamadores que fornecem tipo/minutos/status:

| Sítio | Entrada |
|---|---|
| `app/ponto/views.py:2412` | Administração: tipo e minutos recebidos, aprovada |
| `app/ponto/views.py:2533` | Colaborador: tipo/minutos recebidos, estado inicial pela porta |
| `app/api/views.py:1820` | API: tipo/minutos recebidos, estado inicial pela porta |
| `app/chamados/services/materializacao.py:484` | Mapa de resposta, minutos omitidos, estado inicial |
| `app/chamados/services/materializacao.py:520` | Tipo/status recebidos do fluxo, minutos omitidos |
| `app/ponto/services/veredito_celula.py:315` | Falta ou tipo informado, minutos omitidos, aprovada |
| `app/ponto/services/checar_ausencia.py:46` | Copiloto: tipo informado, minutos omitidos, aprovada |
| `app/ponto/services/regularizacao.py:118` | Falta zero, estado inicial |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Falta zero, estado inicial |
| `app/ferias/models.py:385` → `app/ponto/services/ausencia.py:1186` | Férias zero aprovadas |
| `app/ponto/views.py:2791` | Edição de tipo e minutos pela porta |
| `app/ponto/views.py:85` | Casca comum de decisão; usada em `:2287`, `:2316`, `:2348`, `:2640`, `:2670`, `:2698`, `:2731` |
| `app/ponto/views.py:2581` | Reverter rejeição pela porta |
| `app/ferias/services.py:356` | Rejeição de férias pela porta |
| `app/ponto/management/commands/curar_ausencia_sem_fim.py:46` | `rederivar_fim`: save inteiro (`app/ponto/services/ausencia.py:1280`) |
| `app/ponto/management/commands/rejulgar_status_por_tipo.py:31` | Rejulga status aberto pela porta |

Fora de `app`, há cópia da tabela para sombra, sem decisão de domínio: `bin/sombra_incremental.sh:36`. Nenhum outro escritor por nome/constructor/ORM foi identificado; ausência absoluta de escrita por SQL dinâmico ou objetos não atribuíveis é [não verificado].

**Sem grade:** sem escala vigente, sem tipo, dia com trabalho diferente de `True`, ou sem marcos de início/fim retorna zero (`app/escala/utils.py:2282`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`). **Sem célula não significa necessariamente zero**: há fallback preexistente ao cálculo do vínculo e ao template (`app/escala/models.py:1096`, `app/escala/models.py:1133`). A derivação nova reutiliza essa autoridade, sem introduzir fallback próprio (`app/ponto/models.py:881`).

**Mapa:** único escritor funcional é a declaração em `app/chamados/models.py:3018`; único leitor funcional identificado é `app/chamados/services/materializacao.py:468`. As demais referências são teste/documentação (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:50`).

**Migration:** a única 0077 de `ponto` é a fornecida; depende de 0076 e contém somente `AlterField` de `minutos` com novo help_text (`app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:8`, `:12`). Tipo/default permanecem iguais à criação original do campo (`app/ponto/migrations/0008_ausencia.py:23`). SQL efetivamente produzido pelo Django é [não verificado].

**Outro defeito preexistente:** chamada direta a `Ausencia.save` com `dias_corridos > 1`, `data_fim=None`, falta aprovada e minutos zero deriva primeiro somente `data`, depois constrói `data_fim`; o total fica apenas do primeiro dia (`app/ponto/models.py:882`, `app/ponto/models.py:902`). Os dois sítios curados não fornecem range, e a porta normal calcula `data_fim` antes do save (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:197`).

## 05/11 21:00 · sessao c389e01a9572 · - · - · origem=Ronald (tmux)

H12 — núcleo coerente, com duas divergências antigas de leitura e um corte ainda não realizado na cópia.

- `sai_pela_rescisao` responde exclusivamente `data_demissao != None and data_demissao <= fim_janela`; é extração da porta existente, chamada por `classificar_export` e `montar_resumos`, sem segunda fórmula nesses consumidores. `app/folha/export.py:54`, `app/folha/export.py:60`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`.
- A janela coincide: export calcula por `empresa`; resumos calcula por `colab.empresa`, com cache por `empresa_id`; ambos chamam `janela_competencia`, que delega a `janela_fechamento`, cujo corte vem da empresa e é limitado a `[2,28]`. `app/folha/export.py:47`, `app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:103`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`.
- `_fech_linha.html` usa `r.rescisao` tanto no filtro/cor quanto no badge; `fechamento_mensal` soma a mesma chave; `fechamento.html` mostra o contador e seu filtro. `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:65`.
- `rescisao` só é escrita no dict de `montar_resumos`; seus leitores de produção são os pontos acima; `n_rescisao` só é escrito na view e lido no template acima. `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:65`.

Censo de `Colaborador.data_demissao`:

- Declaração: `app/colaboradores/models.py:325`.
- Escritores de negócio: desligamento agendado pela porta `gravar`; desligamento efetivo e reativação pela porta `salvar_inteiro`. `app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/desligamento.py:214`.
- Escritor genérico: operação `situacao` autoriza o campo; `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote` e construtor `criar_colaborador` persistem objetos/campos recebidos. Não encontrei outro sítio de produção atribuindo explicitamente `data_demissao`. `app/colaboradores/services/colaborador.py:32`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/colaborador.py:65`, `app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:91`.
- O Django admin é somente leitura por herança. `app/colaboradores/admin.py:48`.
- Leitores financeiros: predicado H12 e detalhe do export; `prontidao`; recorte de retenção da célula; raio-X da folha; matriz de holerites. `app/folha/export.py:60`, `app/folha/export.py:862`, `app/folha/export.py:1060`, `app/folha/export.py:1079`, `app/folha/export.py:1200`, `app/folha/export.py:1229`, `app/folha/views.py:612`, `app/folha/views.py:658`, `app/holerite/matriz.py:88`.
- Leitores de universo/cadastro: fechamento, disponibilidade para substituição, grade da célula, alimentadores de cadastro, comandos de diagnóstico/lavra. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:429`, `app/ponto/utils.py:180`, `app/escala/services/leitor_celula.py:351`, `app/escala/services/leitor_celula.py:372`, `app/escala/services/cadastro_realidade.py:444`, `app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:177`, `app/ponto/supra_juiz.py:194`, `app/ponto/management/commands/supra_juiz.py:178`, `app/ponto/management/commands/emitir_furo_retroativo.py:73`.
- Leitores do ciclo do desligamento/vínculo: validação de idempotência, texto do veto de novo vínculo, efetivação agendada, fechamento retroativo de vínculo e tripwire. `app/colaboradores/services/desligamento.py:18`, `app/colaboradores/services/vinculo.py:92`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:41`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:55`.
- Apresentação e entrada: edição de colaborador e badge do fechamento; o catálogo de chamados usa uma chave contextual homônima para revisão de desligamento, não lê diretamente o campo do colaborador. `app/templates/colaboradores/editar.html:274`, `app/templates/colaboradores/editar.html:282`, `app/templates/ponto/_fech_linha.html:4`, `app/chamados/catalogo/modulos.py:141`.

Leitores de rescisão fora do diff:

- Raio-X separa rescisões pelo veredito do juiz; relatórios “quem cobrar” também; pendências de cadastro consultam o mesmo juiz; retrato recebe os itens de `fora_da_folha`, que chama `classificar_export`. Portanto migram automaticamente com H12. `app/folha/views.py:589`, `app/folha/views.py:594`, `app/relatorios/views.py:1658`, `app/relatorios/views.py:1659`, `app/relatorios/views_pend.py:58`, `app/folha/fora_da_folha.py:119`, `app/folha/retrato_folha.py:1157`, `app/folha/retrato_folha.py:1181`.
- `prontidao` ainda dá motivo `"rescisao"` apenas quando a demissão é **anterior ao início** da janela; demissão dentro da janela com denominador zero pode dar `"sem_escala"`, embora H12 dê `rescisao=True`. É regra própria sobrevivente, mas responde ao motivo de cegueira do denominador, não decide entrada no TXT. `app/folha/export.py:1060`, `app/folha/export.py:1081`, `app/folha/export.py:1100`.
- O raio-X ainda acrescenta `†deslig.` só pela presença da data; uma demissão futura fora da janela pode ser anotada como desligamento. Essa anotação não decide o balde de rescisão, que lê o juiz. `app/folha/views.py:658`, `app/folha/views.py:594`.
- Universo da tela exclui desligado cuja demissão antecede o início; o classificador consulta todos os fechamentos gravados da competência e marca esse registro antigo como rescisão. Assim, tela e export podem ter universos diferentes, ainda que o predicado H12 seja único. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/ponto/views.py:1114`, `app/folha/export.py:777`, `app/folha/export.py:858`.

`janela_competencia` — demais leitores de produção:

- Export: ausências por tipo/falta, grade do fechamento, férias com batida, classificação, prontidão e retenção. `app/folha/export.py:166`, `app/folha/export.py:185`, `app/folha/export.py:337`, `app/folha/export.py:486`, `app/folha/export.py:807`, `app/folha/export.py:1058`, `app/folha/export.py:1073`, `app/folha/export.py:1266`.
- Prévia, validação PDF, views da folha e serviços auxiliares. `app/folha/previa.py:42`, `app/folha/validacao_pdf.py:42`, `app/folha/views.py:529`, `app/folha/views.py:588`, `app/folha/services/vigia_pausa.py:123`, `app/folha/services/sem_furo.py:47`.
- Relatórios, cartão e fechamento. `app/relatorios/services.py:214`, `app/relatorios/cartao_pela_celula.py:24`, `app/ponto/services/fechamento_lista.py:104`.
- Vigia/comandos: pareador, censo, geração de células, lavra de status e mapa de divergência. `app/ponto/services/vigia_pareador_celula.py:59`, `app/ponto/management/commands/censo_col900.py:89`, `app/ponto/management/commands/gerar_celulas.py:176`, `app/ponto/management/commands/lavrar_status_competencia.py:40`, `app/ponto/management/commands/mapa_divergencia.py:61`, `app/ponto/management/commands/mapa_divergencia.py:93`.

Classes de reprovação:

- Nenhum juiz adicional de negócio identificado em H12: `sai_pela_rescisao` concentra a condição que `classificar_export` lê; o registro já atribui a este último entrada TXT e travas. `app/folha/export.py:54`, `app/folha/export.py:858`, `app/core/juizes.py:298`, `app/core/juizes.py:299`.
- Travas continuam computadas antes da saída antecipada por rescisão e presentes em todos os itens; H12 altera apenas a condição do primeiro gate, preservando precedência. `app/folha/export.py:822`, `app/folha/export.py:840`, `app/folha/export.py:858`, `app/folha/export.py:864`.
- As três portas continuam lendo `travas_aprovacao` do classificador: aprovação em lote, tranca pelo lote de vazios e aprovação individual. `app/ponto/views.py:1296`, `app/ponto/views.py:1298`, `app/ponto/views.py:1409`, `app/ponto/views.py:1506`, `app/ponto/views.py:1508`.
- RED por inspeção: os casos de demissão posterior falhariam na base mostrada no diff (`is not None`); os três testes da linha também falhariam na base, por chave ausente/badge pela data/contador igual a dois. Execução na base: [não verificado]. `app/folha/tests/test_rescisao_modulo_proprio.py:75`, `app/folha/tests/test_rescisao_modulo_proprio.py:79`, `app/ponto/tests/test_h12_rescisao_na_linha.py:41`, `app/ponto/tests/test_h12_rescisao_na_linha.py:45`, `app/ponto/tests/test_h12_rescisao_na_linha.py:51`.
- Os testes antes/dentro/último dia permaneceriam verdes na base; são regressão, não evidência RED. `app/folha/tests/test_rescisao_modulo_proprio.py:66`, `app/folha/tests/test_rescisao_modulo_proprio.py:69`, `app/folha/tests/test_rescisao_modulo_proprio.py:72`.
- H12 não introduz leitura de relógio nem dependência visível das funções reservadas ao POUSO B: o novo predicado só recebe colaborador e fim, e os leitores fornecem mês/ano explícitos. `app/folha/export.py:54`, `app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:40`, `app/ponto/services/fechamento_lista.py:104`.

Corte antigo não realizado:

- O corte `DESLIGADO-FORA-DA-JANELA-APOSENTA-A-LINHA` manda aposentar a linha anterior à janela, com `status=encerrado`, `derivado_em=NULL`, usando o universo único. A cópia ainda só oferece aberto/aprovado/exportado; o vigia segue contando todos os NULL e sua docstring diz que a aposentadoria espera dono. Isso é dívida preexistente da árvore, não deve ser apresentado como alteração de H12: o escopo fornecido preserva demissões anteriores no predicado `<= fim`. `app/docs/CORTES.md:270`, `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1680`, `app/ponto/services/fechamento.py:1692`, `app/ponto/services/fechamento.py:1700`, `app/folha/export.py:60`.

## 05/11 20:58 · sessao df69f701d606 · H12H06-SEPARA-TRAVA · censo · origem=Code

**Encontrei 24 chamadas executáveis de `select_for_update`. Há ordens inversas no código, inclusive `PeriodoAquisitivo → AgendamentoFerias` e `AgendamentoFerias → PeriodoAquisitivo`.** As referências estão no inventário abaixo e em `app/ferias/services.py:355`, `app/ferias/services.py:362`, `app/ferias/services.py:406`, `app/ferias/services.py:409`.

**Não apresento os itens 2–4 como censo integral fechado:** a exaustividade das expansões transitivas e os totais globais por par permanecem **[nao verificado]**. Distingo abaixo chamadas explícitas, sequências comprovadas e limites, sem transformar ordem textual de operações em primeira aquisição comprovada de todas as linhas.

A correspondência desta cópia ao HEAD/hash informado é **[nao verificado]**: a cópia disponível não contém metadados Git utilizáveis. Execuções, linhas atingidas, esperas e deadlocks de produção são **[nao verificado]**.

Notação: `L` = `select_for_update`; `U` = atualização de existente; `D` = exclusão; `I` = inserção. `I` não será contado automaticamente como trava de linha preexistente de outra entidade.

**1. Inventário de `select_for_update`**

Todas as chamadas abaixo usam `()`, exceto a linha com `of=`. Não encontrei chamadas executáveis adicionais com `nowait`, `skip_locked` ou `no_key` na varredura da cópia; geração dinâmica/dependências externas: **[nao verificado]**.

| Arquivo:linha | Função | Modelo/alvos | Argumentos |
|---|---|---|---|
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100` | `Command._rodar` | `ChamadoColaborador` | `()` |
| `app/chamados/models.py:674` | `ChamadoColaborador._recarregar_estado_se_mudou` | `ChamadoColaborador`, por `type(self)` | `()` |
| `app/chamados/models.py:866` | `ChamadoColaborador.abrir` | `Colaborador` | `()` |
| `app/chamados/services/abertura.py:164` | `abrir_chamado_para_colaborador` | `Colaborador` | `()` |
| `app/chamados/services/disputa_emissao.py:467` | `_abrir_ou_agregar_disputa` | `DisputaSupervisao` | `()` |
| `app/chamados/services/disputa_emissao.py:1156` | `criar_disputa_ancora_de_chamado` | `DisputaSupervisao` | `()` |
| `app/chamados/services/fio.py:762` | `_costura_nao_mata_o_fio` | `type(chamado)`; `ChamadoColaborador` nos caminhos examinados | `()` |
| `app/chamados/services/relavra_via.py:221` | `desfazer_carimbo_sem_lastro` | `PerguntaDisputa` **e** `DisputaSupervisao` | `of=('self', 'disputa')` |
| `app/colaboradores/models.py:431` | `Colaborador.save` | `Colaborador` | `()` |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | `AplicacaoConvencao` | `()` |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | `SolicitacaoFerias` | `()` |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `Command._rodar` | `LoteHolerite`, alias `L` | `()` |
| `app/holerite/management/commands/processar_lote_holerite.py:124` | `Command._rodar` | `LoteHolerite` | `()` |
| `app/ponto/portas/he.py:174` | `decidir_he` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | `DecisaoHE` | `()` |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:70` | `_encerrar_bloqueio_de_entrada` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:114` | `travar_origem_antes` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:127` | `aprovar_justificativa_dp` | `Justificativa` | `()` |
| `app/ponto/services/justificativa.py:262` | `rejeitar_justificativa_dp` | `Justificativa` | `()` |
| `app/ponto/services/justificativa.py:329` | `materializar_saida_dp` | `Justificativa` | `()` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | `Justificativa` | `()` |

Particularidades que alteram a interpretação:

- `_recarregar_estado_se_mudou` e `_costura_nao_mata_o_fio` só aplicam `select_for_update` quando `connection.in_atomic_block` está ativo: `app/chamados/models.py:673`, `app/chamados/services/fio.py:761`.
- `of=('self', 'disputa')` pede os dois alvos **na mesma consulta**. A tupla não comprova `PerguntaDisputa → DisputaSupervisao`; ordem interna **[nao verificado]**. `CelulaDia` aparece no `select_related`, mas está fora do `of`: `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:222`.
- `Colaborador.save` encerra seu atomic interno antes de `super().save`. A trava e a gravação só ficam numa transação abrangente quando existe atomic externo: `app/colaboradores/models.py:429`, `app/colaboradores/models.py:436`, `app/colaboradores/models.py:439`.
- `ChamadoColaborador.criar` não contém a trava de Colaborador de `abrir`: `app/chamados/models.py:1055`, `app/chamados/models.py:1056`, `app/chamados/models.py:866`.
- Consulta de trava que não encontra linha não comprova aquisição sobre entidade existente. Isso importa nos ramos “disputa ausente → criar”: `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:1156`, `app/chamados/services/disputa_emissao.py:1163`.

**2. Locks e escritas implícitas**

| Local | Operação e abrangência |
|---|---|
| `app/ponto/services/vigia_de_hora.py:99` | `pg_try_advisory_lock(7180000 + empresa)`, chave em `:97`; liberação em `:136`. O corpo chama julgamentos em `:131`; não há atomic único abrangendo esse corpo. |
| `app/chamados/reconciliador.py:1224` | `pg_try_advisory_lock(chave)`, chave por schema em `:1210`; liberação em `:1231`. |
| Ausência na varredura | Não encontrei ocorrência executável de `LOCK TABLE`, `pg_advisory_lock` bloqueante ou `pg_advisory_xact_lock`; SQL externo/dinâmico: **[nao verificado]**. |

O inventário confirmado de atualizações/exclusões sob atomic — próprio ou externo identificado — é:

| Entidade | Escrita | Atomic/caminho demonstrado |
|---|---|---|
| `ChamadoColaborador` | `save` em `app/chamados/models.py:279` | `_gravar_estado`, atomic em `:274`; gates abrangentes em `:616`, `:744`, `:1542`, `:1558`. |
| `ChamadoColaborador` | `update` em `app/chamados/models.py:602` | Receiver de resolução chamado pelo save acima: `app/chamados/signals.py:72`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/contexto.py:77`, `:102`, `:124`, `:159` | Atomics em `:75`, `:100`, `:122`, `:157`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/vinculos.py:88`, `:99`, `:114`, `:131`, `:142` | Atomics em `:86`, `:97`, `:112`, `:128`, `:139`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/texto.py:35` | Atomic em `:31`. |
| `ChamadoColaborador` | Arquivamento `save` em `app/chamados/services/arquivo.py:170` | Chamadores atômicos: `app/chamados/views_arquivar.py:134`, `app/chamados/services/silencio_isentos.py:41`. |
| `PerguntaDisputa` | `save` em `app/chamados/models.py:2609`, `:2644`, `:2652`; `update` em `:2663` | Chamadas sob os atomics de fechamento `:1846`, costura `app/chamados/services/fio.py:837`, reabertura `app/chamados/services/disputa_emissao.py:1401`, materialização `app/chamados/services/materializacao.py:1142`. |
| `PerguntaDisputa` | Lavra `save` em `app/chamados/veredito_lavrado.py:231` | Relavra dentro do atomic de estado: `app/chamados/models.py:290`, `app/chamados/models.py:295`. |
| `PerguntaDisputa` | Validação `save` em `app/chamados/services/validacao.py:133`; devolução em `:190` | Atomics em `:262`, `:289`, `:395`. |
| `DisputaSupervisao` | `save` em `app/chamados/models.py:1865` | `fechar`, atomic em `:1846`. |
| `DisputaSupervisao` | `save` em `app/chamados/services/disputa_emissao.py:501`, `:923`, `:971`, `:999`, `:1090`, `:1191` | Atomics em `:466`, `:898`, `:1148`. |
| `RegularizacaoItemBatida` | `save` em `app/chamados/services/regularizacao_ext.py:139`, `:163`, `:188` | Atomic abrangente em `:121`. |
| `RegularizacaoExterna` | `save` em `app/chamados/services/regularizacao_ext.py:200`, `:257` | Atomics em `:121`, `:235`. |
| `Batida` | Correção `save` em `app/chamados/services/regularizacao_ext.py:155`; exclusão em `:136` | Atomic externo em `:121`; savepoint adicional em `:152`. |
| `Batida` | Atualização em `app/ponto/registro_batida.py:254` | Por exemplo, flip sob `app/ponto/services/veredito_celula.py:204`/`:295` ou command `app/ponto/management/commands/flip_automatico.py:65`/`:68`. |
| `CelulaDia` | Atualizações em `app/ponto/portas/celula.py:239`, `:270` | Regeneração, atomic em `:212`. |
| `CelulaDia` | Atualização em `app/ponto/portas/celula.py:578` | Reversão, atomic em `:569`. |
| `CelulaDia` | Atualização em `app/ponto/portas/celula.py:682` | `salvar_e_emitir`, atomic em `:681`. |
| `CelulaDia` | Atualizações em `app/ponto/portas/celula.py:645`, `:655` | Command abrangente `app/ponto/management/commands/gerar_celulas.py:310`, chamada em `:311`. |
| `TurnoMaterializado` | DELETE SQL em `app/ponto/nucleo.py:80`; reposição em `:59` | `recompute_turnos`, atomic em `:57`. |
| `FuroDiario` | `update_or_create` em `app/ponto/portas/celula.py:906`; exclusão em `:914` | Atomic em `:904`. |
| `DecisaoHE` | `save` em `app/ponto/portas/he.py:84`, `:375` | Decoradores em `:137`, `:268`, `:392`; lote em `:548`. |
| `Justificativa` | `save` em `app/ponto/services/justificativa.py:193`, `:272`, `:371` | Atomics em `:125`, `:258`, `:327`. |
| `Justificativa` | `save` em `app/ponto/views.py:1800`, `:1833` | Atomic por item em `:1793`. |
| `Justificativa` | `save` em `app/chamados/reconciliador.py:535` | `_retratar`, atomic em `:522`. |
| `Justificativa` | `save` em `app/ponto/services/regularizacao.py:106` | Atomic em `:89`. |
| `Ausencia` | Atualização de status em `app/ponto/services/ausencia.py:819`; carimbos em `:911` | Aprovação de irmãos sob atomic externo `app/ponto/views.py:2631`, chamada em `:2633`. |
| `Ausencia` | Atualização em `app/ponto/services/ausencia.py:1267`; exclusões em `:1254`, `:1174` | Cancelamento/correção de férias: `app/ferias/services.py:352`, `:375`. |
| `FechamentoMensal` | Atualização em `app/ponto/services/fechamento.py:1640` | Atomic por competência em `:1635`; pode herdar atomic de justificativa/ausência. |
| `FechamentoMensal` | Atualização em `app/ponto/services/fechamento.py:1597` | Receiver síncrono de escala em atomic do emissor: `app/escala/signals.py:133`. |
| `FechamentoMensal` | Gravações em `app/ponto/services/fechamento.py:1128`, `:1144` | Atomic externo, por exemplo `app/ponto/portas/regen_exportada.py:169`, chamada em `:179`. |
| `DiaPago` | Exclusão/reposição em `app/ponto/services/dia_pago.py:357`, `:358` | Sob cálculo com atomic externo, por exemplo o da regeneração exportada acima. |
| `DiaPago` | `bulk_update`/`save` em `app/ponto/services/dia_pago.py:536`, `:540` | Atomic em `:532`. |
| `DiaPago` | Atualizações em `app/ponto/services/dia_pago.py:669`, `:671` | Atomic em `:667`. |
| `FolgaDia` | Exclusão/save em `app/escala/folgas.py:333`, `:342` | Atomics de alteração/lote em `:354`, `:382`. |
| `FolgaDia` | Atualização em `app/escala/folgas.py:493` | Reparentamento, atomic em `:490`. |
| `FolgaDia` | Exclusão/reposição em `app/escala/folgas.py:183`, `:184` | Geração de foto, atomic em `:162`. |
| `FolgaCalendario` | Saves em `app/escala/folgas.py:560`, `:564`, `:569` | Atomic em `:555`; primeiro save pode inserir, conforme `:556`. |
| `FolgaCalendarioTamanho` | Exclusão em `app/escala/folgas.py:565`; `update_or_create` em `:571` | Mesmo atomic em `:555`. |
| `EscalaColaborador` | Saves em `app/colaboradores/services/vinculo.py:274`, `:445`, `:799`; updates em `:615`, `:691`, `:753`, `:991` | Atomics em `:187`, `:443`, `:796`, `:614`, `:690`, `:752`, `:989`. |
| `AgendamentoFerias` | Saves em `app/ferias/services.py:362`, `:387`, `:406`; exclusão em `:483` | Atomics em `:352`, `:375`, `:399`, `:482`. |
| `PeriodoAquisitivo` | Saves em `app/ferias/services.py:302`, `:326`, `:355`, `:389`, `:409`, `:450`, `:475`; exclusão em `:484` | Atomics de férias indicados no catálogo seguinte; avaliação sob `app/ferias/views.py:717`. |
| `SolicitacaoFerias` | Save em `app/ferias/services.py:422` | View abrangente `app/ferias/views.py:717`. |
| `Colaborador` | Escritores em `app/colaboradores/services/colaborador.py:61`, `:69`, `:76`, `:88` | Chamadores atômicos de edição, desligamento, transferência e normalização descritos adiante. |
| `Empresa` | Update em `app/colaboradores/services/estrutura.py:27` | Command `app/colaboradores/management/commands/normalizar_identificadores.py:106`, chamada em `:112`. |
| `User` | Save de acesso em `app/core/credenciais.py:114`; save na edição em `app/colaboradores/views.py:1345` | Desligamento/reativação em `app/colaboradores/services/desligamento.py:103`, `:209`; edição em `app/colaboradores/views.py:1340`. |
| `AgendaDia` | Updates em `app/escala/services/agenda.py:33`, `:45` | Proposta em `app/colaboradores/views.py:2358`; resposta em `app/chamados/services/acoes_chamado.py:102`. |
| `SolicitacaoBeneficio` | Update em `app/colaboradores/services/beneficios.py:32` | View abrangente `app/colaboradores/views.py:2065`, chamada em `:2086`. |
| `LoteHolerite` | Saves em `app/holerite/management/commands/processar_lote_holerite.py:68`, `:146`; motor em `app/holerite/motor.py:409`, `:487` | **Três transações distintas**: command `:53`, motor `:385`, command `:123`. |
| `Holerite` | Save em `app/holerite/motor.py:461`; updates em `app/holerite/servicos.py:113`, `:121`, `:129` | Motor atomic `app/holerite/motor.py:385`; envio atomic `app/holerite/servicos.py:144`. |
| `PushSubscription` | Save em `app/ponto/webpush.py:66` | Pode ocorrer no envio de holerites, pelo fallback `app/holerite/servicos.py:42`, dentro do atomic `:144`. |
| `AplicacaoConvencao` | Updates em `app/core/services/aplicacao_convencao.py:132`, `:155` | Decoradores em `:100`, `:137`. |
| `Pauta` | Updates em `app/pautas/services.py:215`, `:247`, `:272`, `:294` | Decoradores em `:203`, `:232`, `:255`, `:280`. |
| `ContadorPauta` | Update em `app/pautas/services.py:354` | Chamado por esses quatro atos: `:218`, `:251`, `:276`, `:297`. |
| `EventoCelula` | Updates em `app/chamados/reconciliador.py:1374`, `:1380` | Atomic próprio em `:1371`, depois do processamento dos chamados. |
| `PlacarSituacional` | `update_or_create` e exclusão em `app/core/management/commands/lavrar_placar_situacional.py:105`, `:110` | Atomic em `:101`; Empresa é consultada em `:100`. |

Outras entidades **inseridas** nos caminhos examinados: `LogAuditoria` (`app/core/observ.py:134`), `FollowupChamado` (`app/chamados/services/nota.py:103`, `:107`), `HistoricoVinculo` (`app/colaboradores/services/historico_vinculo.py:25`), `OrdemSubstituicao` (`app/ponto/services/cobertura.py:100`, `:289`), `Beneficio` (`app/colaboradores/services/beneficios.py:11`), `LoteEspelho` (`app/relatorios/lote_espelho.py:103`), `RespostaAnterior` (`app/chamados/services/disputa_emissao.py:1403`). Não atribuí a essas inserções uma ordem de travas de FK: **[nao verificado]**.

**Signals síncronos que escrevem outra entidade**

Nas linhas com “cartório”, a expansão está detalhada no começo do item 3.

| Emissor/evento | Receiver arquivo:linha | Outra entidade escrita/travada |
|---|---|---|
| `Setor/post_save` | `app/colaboradores/signals.py:10` | `Group.save` em `:21` ou `get_or_create` em `:25`; permissões M2M via `:30` → `app/colaboradores/mixins.py:397`; atualização de Setor em `signals.py:32`. |
| `Setor/post_delete` | `app/colaboradores/signals.py:36` | Exclusão de `Group` em `:43`; cascatas internas **[nao verificado]**. |
| `Posto/post_save` | `app/colaboradores/signals.py:51` | `DisputaSupervisao.remover_motivo` em `:77`: exclui `PerguntaDisputa`, salva Disputa e pode fechá-la, em `app/chamados/models.py:1976`, `:1979`, `:1986`. |
| `Batida/post_save` | `app/colaboradores/signals.py:87` | Atualiza `Colaborador` por `:102` → `app/colaboradores/services/colaborador.py:61`, sob os guards de `signals.py:94`, `:97`, `:99`. |
| `DeviceColaborador/post_save` | `app/colaboradores/signals.py:106` | Atualiza `Colaborador` por `:119` → `app/colaboradores/services/colaborador.py:61`. |
| `Batida/post_save` | `app/ponto/signals.py:35` | `TurnoMaterializado` D/I, via `:26` → `app/ponto/nucleo.py:57`; `EventoCelula` I condicional via `nucleo.py:66`. |
| `Batida/post_delete` | `app/ponto/signals.py:49` | Mesmo recompute, chamado em `:50`. |
| `EscalaColaborador/post_save` | `app/ponto/signals.py:73` | Mesmo recompute, chamado em `:64`. |
| `Batida/post_save` | `app/ponto/signals.py:132` | Cartório, por `:135` → `:93`. |
| `Batida/post_delete` | `app/ponto/signals.py:139` | Cartório, por `:140`. |
| `Ausencia/post_save` | `app/ponto/signals.py:144` | Cartório, por `:147`. |
| `Ausencia/post_delete` | `app/ponto/signals.py:151` | Cartório, por `:152`. |
| `Justificativa/post_save` | `app/ponto/signals.py:162` | Cartório, por `:165`. |
| `Batida/post_save` | `app/ponto/signals.py:207` | Reconcilia chamados em `:215` e fecha adesão em `:234`; a unidade de retratação pode atualizar Justificativa em `app/chamados/reconciliador.py:535`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:35` | Regenera `CelulaDia` pela chamada `:71`; marca contexto de `ChamadoColaborador` órfão pela cadeia `app/ponto/portas/celula.py:283` → `app/ponto/management/commands/gerar_celulas.py:256`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:80` | Mesma regeneração, chamada em `:117`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:125` | Atualiza `FechamentoMensal`, por `:133` → `app/ponto/services/fechamento.py:1597`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:143` | Remove motivo de Disputa por `:161`; escreve Pergunta/Disputa pelo método citado acima. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:173` | Atualiza `FolgaDia`, por `:191` → `app/escala/folgas.py:493`; segue para relavra em `folgas.py:500`. |
| `DisputaSupervisao/post_save` | `app/chamados/signals.py:19` | Trava/atualiza Chamado-pai por `:42` → `app/chamados/models.py:674`, `:279`; insere nota em `signals.py:45`. |
| `DisputaSupervisao/post_save` | `app/chamados/signals.py:125` | Carimba Perguntas pela chamada `:140` → `app/chamados/services/materializacao.py:1346` → `app/chamados/models.py:2609`. |
| `ChamadoColaborador/post_save` | `app/chamados/signals.py:76` | Escreve Perguntas e pode fechar Disputas pela chamada `:92` → `app/chamados/services/disputa_emissao.py:1603`, `:1689`. |
| `ChamadoColaborador/post_save` | `app/chamados/signals.py:165` | Cartório pela chamada `:180`, salvo guards em `:169`, `:174`. |

Receivers que **agendam** o efeito por `on_commit`, portanto não justificam uma seta de trava na transação emissora:

| Emissor | Receiver | Agendamento |
|---|---|---|
| `DisputaSupervisao` | Retomar regularizações, `app/chamados/signals.py:98` | `app/chamados/signals.py:121` |
| `PerguntaDisputa` | Fechar beco, `app/chamados/signals.py:192` | `app/chamados/signals.py:202` |
| `Colaborador` | Silenciar isento, `app/chamados/signals.py:214` | `app/chamados/signals.py:224` |
| `ChamadoColaborador` | Silenciar isento, `app/chamados/signals.py:228` | `app/chamados/signals.py:251` |
| `Feriado` | Reavaliar dia, `app/chamados/signals.py:270` | `app/chamados/signals.py:280` |
| `ChamadoColaborador` | Acordar pai, `app/chamados/signals.py:295` | `app/chamados/signals.py:314` |
| `Ausencia/save/delete` | Consumidores, `app/ponto/signals.py:276`, `:283` | `app/ponto/signals.py:272` |
| `Posto` | Reavaliar geofence, `app/ponto/signals.py:292` | `app/ponto/signals.py:307` |

Dois `pre_save` examinados apenas leem/capturam estado: `app/escala/signals.py:19` e `app/chamados/signals.py:156`. O receiver de `TipoAusencia`, registrado pelos aliases de save/delete em `app/ponto/models.py:1233`, `:1234`, apenas invalida cache em `:1236`.

Os receivers de login chamam exclusão de `Session` ou alteram a sessão da request: `app/core/signals.py:14`, `:40`, `app/core/view

[truncado em 20000 de 45552 caracteres]

## 05/11 20:51 · sessao 35b27ecbba74 · - · - · origem=Ronald (tmux)

Complemento dos commands solicitados:

| Caminho | Atomic e escritas comprovadas |
|---|---|
| `marcar_abandono_provisorio.handle` | Atomic cobre **o comando inteiro** em `app/ponto/management/commands/marcar_abandono_provisorio.py:22`; User `get_or_create` na linha 30 → loop de colaboradores nas linhas 35/43 → loop de dias na linha 70 → `garantir_ausencia` na linha 76. Signals síncronos de Ausencia entram nessa transação: `app/ponto/signals.py:143`/147. Sem `--apply`, rollback explícito na linha 98; com `--apply`, não há esse rollback. Ordem dos colaboradores não traz `order_by` nesse queryset, linha 35. |
| `escalonar_chamados_supervisao`, N2 | Atomic `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`: `escalar_urgencia` linha 132 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria por `models.py:524`/`app/core/observ.py:134` → `anotar_contexto` na linha 138 → Chamado SAVE `app/chamados/services/contexto.py:77`. N3 escreve contexto de Disputa na linha 159 e de Chamado na linha 163 **fora desse atomic**. |
| `triagem_batida.ramo_solicitar_anuencia` | Atomic `app/ponto/services/triagem_batida.py:368`: escala linha 369 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria via linha 524 → renova contexto na linha 371 → Chamado SAVE `app/chamados/services/contexto.py:102`. Followup é anexado na linha 351, antes desse bloco. |
| `curar_residuo_bug115._ambiguo` | Atomic `app/ponto/management/commands/curar_residuo_bug115.py:120`: renovar contexto linha 121 → Chamado SAVE `app/chamados/services/contexto.py:102` → LogAuditoria via comando linha 124 e `app/core/observ.py:134`. Outros ramos geram/julgam célula nas linhas 135/143, fora desse atomic. |
| `reconciliar_grade._relavrar_orfa` | Atomic `app/ponto/management/commands/reconciliar_grade.py:472`: renovar contexto linha 473 → Chamado SAVE `app/chamados/services/contexto.py:102`; retirar contexto linha 474 → Chamado SAVE `contexto.py:124`. Trilha `evento_kw` na linha 477 ocorre depois do bloco. |
| `lavrar_placar_situacional.handle` | Atomic `app/core/management/commands/lavrar_placar_situacional.py:101`: loop PlacarSituacional `update_or_create` linha 105 → DELETE de PlacarSituacional linha 110. Empresa apenas consultada na linha 100. Não aparece segunda entidade escrita nesse bloco. |

Dois comandos são barrados em produção pelo código:

- `zerar_dados_operacionais`: exige ensaio em `app/ponto/management/commands/zerar_dados_operacionais.py:27`; a guarda recusa se `BANCO_DE_ENSAIO` não estiver ativo **ou** o banco for `saas_hasner`, em `app/ponto/registro_batida.py:326`. No atomic da linha 80, sequência das chamadas explícitas: Followup DELETE linha 83 → Chamado DELETE linha 84 → AlertaGeofence DELETE linha 85 → AlertaTurno DELETE linha 87 → PushSubscription DELETE linha 88 → Ausencia DELETE linha 90 → FechamentoMensal DELETE linha 93 → Justificativa DELETE linha 95 → Batida DELETE linha 96 → ImportacaoAEJ DELETE linha 97 → PingGeo DELETE linha 98. Deleções reais nos helpers: `app/chamados/services/nota.py:141`, `app/chamados/models.py:539`, `app/ponto/services/alerta_turno.py:32`, `app/ponto/services/ausencia.py:1274`, `app/ponto/services/fechamento.py:1752`, `app/ponto/registro_batida.py:345`. Signals de Ausencia/Batida podem intercalar escritas nessas chamadas; ordem SQL interna das cascatas [não verificado].
- `gerar_batidas_smoke_motor`: guarda de ensaio em `app/ponto/management/commands/gerar_batidas_smoke_motor.py:45`, mais exigência de host `juliani_db_test` nas linhas 47/48. Atomic da linha 106 chama `semear` na linha 107 → Batida `bulk_create` em `app/ponto/registro_batida.py:335` → trilha na linha 336. Não é caminho executável no banco `saas_hasner` pela guarda citada.

A referência `app/chamados/models.py:849` afirma, em comentário, ausência de atomic envolvendo laços sobre colaboradores; o comando `marcar_abandono_provisorio` efetivamente tem esse formato nas linhas 22,35,43,76. Para a análise de ordem, usar o fluxo executável do comando.

_2004 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
