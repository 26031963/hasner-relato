# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 14:38** · publicado em **15/11/2026 14:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 14:38 · sessao b0efe9af1207 · - · - · origem=Ronald (tmux)

Achados somente leitura; nenhum arquivo editado.

- **B — três selos de escrita:** `chamados/juizes.py:346` atribui `resposta_colab` no adaptador. Guarde `_resposta` e exponha-a em `__getattr__`, mantendo o adaptador estritamente de leitura; testes permanecem intocados (`test_contract_par_resposta.py:90`, `test_contract_texto_com_carimbo.py:166`, `test_e3_porta_A_escritor_do_par.py:229`).
- **B — contador 24→21:** comparação com `151b0666a`, usando fontes integralmente em memória, encontrou três extras: `_q_resposta_conservada`, `reabrir_credor_de_pergunta_viva`, `Command._relavrar_orfa`. Os dois últimos têm código idêntico ao main; perderam classificação PAR porque a origem agora usa `getattr`, invisível ao detector de texto (`test_e3_catraca_respondedores.py:134`). Leia `p.resposta_colab` diretamente na adaptação permitida `_leitura_conservada`, com guarda de existência; isso restaura o fechamento transitivo dos predicados.
- **B — extra SQL:** incorpore a expressão `legado_sem_instante` no próprio OR de `candidatas`, sem alterar sua lógica; a catraca então reconhece texto e instante no mesmo statement (`juizes.py:1668–1672`, detector `test_e3_catraca_respondedores.py:386`).
- **A — equivalência SQL antiga:** a lambda exige carimbo para reconhecer texto (`test_e3_forma_sql_do_juiz.py:96`), contradizendo literalmente P5/P10 da matriz fornecida. Troque somente essa asserção para o texto da leitura canônica; o mutante da face reutiliza essa mesma asserção (`:143`).
- **Incompatibilidade de selos:** a catraca exige uma leitura nominal `.respondida_em` no slot e PAR em `respondida_sem_texto` (`test_e3_catraca_respondedores.py:91`, `:507`), enquanto F1 proíbe esses atributos nesses sítios independentemente de serem aliases de atos (`test_e4c_f1_juizes_le_o_ato.py:134`, `:595`, `:599`). Não há catálogo produtivo consumido por esses testes; os registros moram nos próprios testes (`:22` e catraca `:74`). Propriedades em `LeituraDosAtos` não resolvem a contradição sintática. [nao verificado]

Experimento sem escrita: adaptador recebendo `LeituraDosAtos`, leitura direta na origem e OR SQL incorporado produziram **21 respondedores, nenhuma declaração morta e nenhuma divergência declarada**. Não executei Django/banco. A solução exige registrar nominalmente os aliases canônicos nas asserções F1 ou atualizar o reconhecimento da fonte ato na catraca; esconder atributos por reflexão seria maquiagem. [nao verificado]

## 15/11 14:37 · sessao 405c5a69e6cc · - · - · origem=Ronald (tmux)

Classificação por leitura da cópia, sem edição:

| Ocorrência | Classe | Fundamento |
|---|---|---|
| `e4b_c11_orm::test_RED_A6_beneficio_rejeitado_limbo` | A | Herda cenário texto sem instante, que P5 manda enxergar em D1: `test_e4b_c6_orm.py:77,81`; `test_e4b_c11_orm.py:122`. |
| `e4b_c11_orm::test_RED_limbo_misto...` | A | Mesmo cenário herdado: `test_e4b_c6_orm.py:154`. |
| `e4b_c6_orm::test_RED_limbo_misto...` | A | Mesma causa P5: `test_e4b_c6_orm.py:77,81`. |
| `e4b_c6_memoria::test_RED_limbo...` | A | Texto sem instante permanece vivo; `materializada_em` deve ficar `None`: `test_e4b_c6_memoria.py:110-112`; matriz §4 P5. |
| `e4b_classe_e_memoria::test_P3_P4_evento_recusado...` | A | `q.carimbar` só deve ser chamado no caso texto vazio; texto `'08:00'` permanece vivo: `test_e4b_classe_e_memoria.py:1015-1024`; matriz §4 P5. |
| `e4b_classe_e_memoria::test_mutantes_mordem...` | A | Erro secundário à asserção anterior: `test_e4b_classe_e_memoria.py:1051-1054`. O mutante ainda morde no primeiro caso vazio. |
| `e4b_c12_orm::test_RED_desligamento` | A | P5 torna o limbo da competência aberta vivo (`test_e4b_c12_orm.py:60`); gate recusa fechamento (`models.py:422`). Resultado esperado: chamado aberto, zero pauta e sem carimbos, inclusive após repetição. |
| `fechar_disputa_materializada::test_pergunta_materializada...` | A | Texto sem instante entra em D1 antes do carimbo: `test_fechar_disputa_materializada.py:41-43,60`; matriz §4 P5. `fechada_em` deve continuar `None`. |
| `materializar_carimba_respondida::test_carimba...` | A | Mesmo P5: `test_materializar_carimba_respondida.py:62,75-79`; os três carimbos devem continuar nulos. |
| `reconciliar_pergunta_orfa::test_06...` | A | Mesmo P5: `test_reconciliar_pergunta_orfa.py:104,111-114`; esperado `n=0`, FK e carimbos nulos. |
| `o261e_f2_ramo_e_i20::test_caso2...` | A | P5 conserva texto; motivo `batida_ausente` não tem marco próprio, portanto concordância não mata: `test_o261e_f2_ramo_e_i20.py:260-261`; `juizes.py:1046-1048`. Premissa deve ser `assertTrue`. |
| `e4_passivo::test_classe_c...` | B | Código converte falta de porta em resultado errado: `backfill_fio_morto_e4b.py:264-265`; teste exige `segue_sem_porta`: `test_e4_passivo.py:70`. Cura: nessa atribuição, ação humana sem porta recebe `SEGUE`, mantendo `ato`. | [nao verificado]

Reproduzi sem banco os dois testes de memória: `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4b_c6_memoria.C6MemoriaTest.test_RED_limbo_tem_motivo_proprio_e_limpeza_continua chamados.tests.test_e4b_classe_e_memoria.AlcanceTest.test_P3_P4_evento_recusado_segue_pelos_juizes`: `Ran 2`, `FAILED (failures=2)` [saída do comando; não persistida]. [nao verificado]

Achado adicional B: `test_fio_le_o_juiz.py:208-215` conserva validação de 16/10 com resposta de 18/10 (`:171-173`). A adaptação fabrica atos sintéticos em ordem temporal; a resposta posterior apaga a validação (`atos_pergunta.py:46-48`). A conservação R/V deve respeitar a projeção legada, sem essa reabertura artificial; teste intocado. [nao verificado]

## 15/11 14:37 · sessao d3c03c670f69 · - · - · origem=Ronald (tmux)

Todos os seis grupos de custo são B; nenhuma mudança literal da matriz autoriza subir contagem/teto (`chamados/tests/test_e3_v4_orm.py:91`, `test_selo_memo_pergunta_viva.py:178`, `escala/tests/test_f3_beco1_folga_na_lista.py:398`).

- **Memo**: `_chave_memo_pv` consulta atos e batidas, duas vezes por primeira chamada e novamente em repetições (`chamados/juizes.py:956`, `:960`, `:998`, `:1010`). Cura: chave estritamente em memória; snapshots de decisões/plantios entram quando já carregados. Incluir os campos escalares anteriores e dados de célula/âncora preserva mutações sem `save` (`test_selo_memo_pergunta_viva.py:117`).
- **Tranca/9 queries/SNC2**: projeção consulta atos antes da guarda de tranca (`chamados/juizes.py:1507`, `:1513`). Antecipar a mesma guarda dentro do juiz evita leitura inútil para perguntas mortas. `select_related('ato_por')` nas leituras extras explica consultas a `auth_user` contadas pelo SNC2 (`:338`; `test_snc2_toque_em_lote.py:105`).
- **Modal**: fio já oferece decisões com autores por prefetch (`chamados/services/fio.py:257`). `_leitura_da_pergunta` chama `.select_related()` e perde esse prefetch (`chamados/juizes.py:338`). Consumir cache carregado diretamente; conservar projeções em lote.
- **F5/C11**: `alimentar_portas` julga individualmente (`chamados/juizes.py:1780`), `_no_app` consulta outro lote avulso (`:1801`; `pergunta_no_app.py:59`) e C11 reprojeta por dia (`escala/services/cadastro_realidade.py:502`). Alimentar esses mesmos objetos com snapshot comum; leitores continuam perguntando ao juiz.
- **Carga**: tarefa já lê decisões e correspondências conservadas (`tarefa_do_dia.py:419`, `:428`); nova carga repete ambas (`carga_pergunta_viva.py:80`, `:82`). Preservar resultados integrais e reutilizá-los em `LeiturasDaVida`, mantendo construção direta independente com três queries (`test_e4c_f1v6_orm.py:71`).
- **Relavra ≤8**: mesmas consultas duplicadas da chave atingem pergunta validada que deveria morrer na primeira projeção (`relavra_via.py:242`; `juizes.py:1509`).

**Limite real do desenho:** novos atos via `registrar` usam `bulk_create`, sem `post_save` (`models.py:4096`). O evento existente alcança somente perguntas com célula (`models.py:3317`, `:3328`; `ponto/portas/celula.py:954`). Portanto receiver de `EventoCelula` seria incompleto. Detectar alteração persistida sem atualizar a instância, sem SQL e sem aviso do escritor exige novo mecanismo [não verificado]. Não recomendo declarar invalidabilidade universal apenas com cache em memória. Os testes locais P7 alteram relações em memória e podem ser preservados integralmente (`test_e4c_f1_juizes_le_o_ato.py:277`; `test_e4c_f1v6_matriz.py:171`).

## 15/11 14:33 · sessao 9455ebe3d1c5 · E3-RESTO-R3 · construtor · origem=Code

Cura na cópia; **GREEN integral e ruff zero [nao verificado]**. [Entrega — 36 linhas](chamados/tests/e3_r3_c12_entrega.txt:1).

| Classe | Blocos |
|---|---:|
| A | 40 |
| B | 25 |
| C | 9 |

[Classificação individual](chamados/tests/e3_r3_c12_classificacao.txt:2): seis A também tinham regressões corrigidas no código. Os testes B permanecem intactos. [Auditoria](chamados/tests/e3_r3_c12_auditoria.txt:15).

Evidência local: **314 testes OK**, quatro REDs contra c11 e **49/49 mutantes mortos por asserção**. Comandos e `arquivo::função` constam na [entrega](chamados/tests/e3_r3_c12_entrega.txt:21).

Os 40 módulos foram tentados: 16 GREEN; 24 pararam por dependências ausentes. [Resultados por módulo](chamados/tests/e3_r3_c12_modulos.txt:3).

**PERGUNTA:** qual token usar sem colisão? Mantido `#0f766e`, conforme solicitado. [Registro](folha/retrato_folha.py:303).

LEI-AKITA: `padrao=Single Source of Truth / Aggregate Root; Clock Injection; CQRS com projecoes reconstruiveis`. [Referências](chamados/tests/e3_r3_c12_entrega.txt:36).

## 15/11 14:11 · sessao e8e3cd768849 · CORTES-1358-B · sonda · origem=Code

O invólucro executa a sonda antes de anexar o resultado raspado (`bin/codex_pergunta.sh:327-335`). Não realizei nova execução; não tenho o `sonda.py` anexado disponível nesta cópia. [nao verificado]

Não devolvo uma nova saída JSON como se tivesse sido executada. A tabela abaixo consolida exclusivamente o JSON fornecido; a transcrição integral das listas de IDs não está incluída. [nao verificado]

| Grupo | Chave | n | Referência |
|---|---|---:|---|
| TOTAL | — | 8709 | Dado fornecido [nao verificado] |
| TIPO_RESPOSTA | data_hora | 24 | Dado fornecido [nao verificado] |
| TIPO_RESPOSTA | escolha | 5 | Dado fornecido [nao verificado] |
| TIPO_RESPOSTA | hora | 8680 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-05 · aberta | 3 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-06 · exportada | 50 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-07 · aberta | 116 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-07 · exportada | 691 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-08 · aberta | 1110 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-08 · exportada | 6268 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-09 · exportada | 412 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-10 · aberta | 52 | Dado fornecido [nao verificado] |
| COMPETENCIA | 2026-11 · aberta | 7 | Dado fornecido [nao verificado] |
| EVIDENCIA_FOTO | SEM_EPISODIO | 8709 | Dado fornecido [nao verificado] |

Não há grupo `INDETERMINADA` informado. As competências listadas somam 8709, igual ao TOTAL; o resíduo aritmético é 0. [nao verificado]

| Situação da competência | n | n REGISTRO_FOTO_CRIADO_APOS_V | IDs de pergunta após V | Referência |
|---|---:|---:|---|---|
| exportada | 7421 | 0 | `[]` | Soma dos grupos fornecidos [nao verificado] |
| aberta | 1288 | 0 | `[]` | Soma dos grupos fornecidos [nao verificado] |

## 15/11 14:09 · sessao 85e7e5cdf17b · E4D-PRE-DESENHO · leitura · origem=Code

O ponto de concentração confere: `carimbar` aplica o carimbo, consulta o C6 e salva; **hoje não registra DP nem abre um atomic envolvendo todo esse ato**. O atomic do `save` e o de `registrar` são blocos separados. (`app/chamados/models.py:3186`, `:3222`, `:3235`, `:3532`, `:4095`)

O hash `151b0666a` é o informado no pedido; sua identidade Git nesta cópia está **[nao verificado]**. Os testes abaixo foram inspecionados; resultado de execução **[nao verificado]**.

Nas referências seguintes, caminhos `models.py`, `reconciliador.py`, `juizes.py`, `services/...` e `tests/...` têm base em `app/chamados/`; outros caminhos aparecem completos.

**1. Matriz e censo de chamadas**

Deslocamento das referências do aval:

| Referência fornecida do origin | Local conferido na cópia |
|---|---|
| Porta `models.py:3186` | Continua `models.py:3186`. |
| `reconciliador.py:406`, `:596` | Continuam `reconciliador.py:406`, `:596`. |
| `disputa_emissao.py:1993` | Está em `services/disputa_emissao.py:2007`: **+14 linhas**. |
| `disputa_emissao.py:2067` | Está em `services/disputa_emissao.py:2081`: **+14 linhas**. |
| `materializacao.py:567`, `:1283`, `:1437` | Continuam `services/materializacao.py:567`, `:1283`, `:1437`. |
| `models.py:3386` | Continua `models.py:3386`. |

A comparação acima usa as posições do origin fornecidas no pedido; o conteúdo daquele commit não foi comparado por Git **[nao verificado]**. [nao verificado]

Convenções da matriz:

- **C** significa o ato de conclusão solicitado pelo aval; seu valor literal ainda não existe em `ATOS`, portanto nome/implementação **[nao verificado]**. (`models.py:4002`)
- **V** expande para cinco resultados de `via_pelo_fato`: `cadastro_folga`, `adesao`, `ausencia_dia`, `completude_turno`, `chamado_encerrado`. (`juizes.py:1793`, `:1794`, `:1802`, `:1810`, `:1811`)
- Usar **`ato_motivo=via`** é uma codificação possível para cumprir “via dentro do ato”; essa convenção ainda não está implementada **[nao verificado]**. DP não tem campo próprio de via e `ato_motivo` é texto livre. (`models.py:4014`, `:4019`, `:4026`)
- “Sem filtro de tipo” não significa só os cinco choices atuais: existe fixture de tipo legado `texto`, e a porta não valida `tipo_resposta`. (`catalogo/respostas.py:20`; `tests/test_o261e_f2_porta_carimbo.py:636`; `models.py:3201`)

| Chamada | Via × tipo; argumentos relevantes | Ato que deveria nascer | `ato_motivo` | Atomic atual | Retorno e tratamento | Prova existente |
|---|---|---|---|---|---|---|
| **W1 — `reconciliador.py:406`** | `sem_turno_aberto` × sem filtro de tipo; exige motivo C1 e texto vazio; `resposta=None`. (`reconciliador.py:333`, `:363`, `:365`, `:406`) | C somente se aceita; conclusão ainda ausente. (`models.py:3226`, `:3240`, `:4002`) | Candidato: `sem_turno_aberto`; convenção **[nao verificado]**. (`reconciliador.py:406`) | Função não abre atomic próprio; `save` abre seu bloco interno. Transação herdada depende da entrada. (`reconciliador.py:341`, `:415`; `models.py:3532`) | `False` → retorna `False`; sucesso → pode fechar disputa e retorna `True`. (`reconciliador.py:406`, `:412`, `:415`) | Positivo e caso que acusa: `tests/test_o261_lavra_le_a_lampada.py:148`, `:129`. **Sem teste de C**; catálogo atual em `models.py:4002`. |
| **W2 — `reconciliador.py:596`** | `celula_absolveu` × sem filtro de tipo no laço; `resposta=None`. (`reconciliador.py:585`, `:596`) | C se aceita. (`models.py:3226`, `:3240`, `:4002`) | Candidato: `celula_absolveu`; convenção **[nao verificado]**. (`reconciliador.py:596`) | Atomic por pergunta, com trava do colaborador. (`reconciliador.py:593`, `:594`) | `False` → continua; sucesso → incrementa; exceção → registra erro e segue. (`reconciliador.py:597`, `:599`, `:603`) | Via pelo orquestrador e chamada direta: `tests/test_o261_lavra_le_a_lampada.py:172`; `tests/test_selo_celula_absolveu.py:70`. **Sem teste de C**. (`models.py:4002`) |
| **W3 — `services/disputa_emissao.py:2007`** | V × sem filtro de tipo; `resposta='validar'`, `lavrar_veredito=True`. (`services/disputa_emissao.py:1927`, `:2007`, `:2008`; `juizes.py:1793`) | C se aceita, independentemente de ter criado batida nesta função. (`services/disputa_emissao.py:1910`, `:2007`; `models.py:4002`) | Candidato: via efetivamente recebida; convenção **[nao verificado]**. (`services/disputa_emissao.py:2007`) | Wrapper reutiliza transação corrente ou abre atomic. (`services/disputa_emissao.py:1900`) | `False` → continua; sucesso → incrementa `n_pq`. (`services/disputa_emissao.py:2009`, `:2010`) | Controle positivo com batida real: `tests/test_pep_reentrada.py:229`. **Sem teste de C**. (`models.py:4002`) |
| **W4 — `services/disputa_emissao.py:2081`** | V pelo pai, ou `chamado_encerrado` sem pai × sem filtro de tipo; `resposta='responder'`. O comentário cita escolha, mas o filtro não restringe a escolha. (`services/disputa_emissao.py:2060`, `:2079`, `:2081`) | C se aceita; o evento de auditoria atual não é DP. (`services/disputa_emissao.py:2082`, `:2084`; `models.py:4002`) | Candidato: via efetivamente recebida; convenção **[nao verificado]**. (`services/disputa_emissao.py:2079`) | Mesmo wrapper de W3. (`services/disputa_emissao.py:1900`) | `False` → continua sem trilha de sucesso; sucesso → `fecho_sem_credor`. (`services/disputa_emissao.py:2082`, `:2084`) | Guarda de voo com/sem destino; **C6 mockado**, limite declarado. (`tests/test_pep_reentrada.py:270`, `:301`, `:304`) **Sem teste de C**. (`models.py:4002`) |
| **W5 — `services/materializacao.py:567`** | `declaracao` ou `ausencia_dia` × **hora**; `resposta='validar'`. (`services/materializacao.py:539`, `:555`, `:567`) | **ABSORCAO existente, sem acrescentar C para o mesmo fechamento**; hoje gravada depois pelo chamador. (`services/materializacao.py:568`, `:569`) | Hoje omitido → `''`; candidato futuro: a via, preservando as duas FKs. Convenção **[nao verificado]**. (`services/materializacao.py:569`; `models.py:4019`, `:4035`) | Atomic por irmã; trava do colaborador antes da porta. (`services/materializacao.py:563`, `:566`) | Sucesso → DP ABSORCAO; `False` → lista recusadas e emite trilha da recusa. (`services/materializacao.py:569`, `:574`, `:575`) | Exige uma ABSORCAO por irmã após duas rodadas, FKs e mesmo instante. (`tests/test_e1_entradas.py:239`, `:246`, `:248`, `:250`) |
| **W6 — `services/materializacao.py:1283`** | `declaracao` se `declarou_hora`; senão V com destino ou `chamado_encerrado` × **hora/data_hora/escolha**; `resposta='validar'`, lavra ligada. (`services/materializacao.py:1227`, `:1268`, `:1271`, `:1274`, `:1283`) | C se aceita; deve acompanhar o rollback do plantio. (`services/materializacao.py:1226`, `:1285`; `models.py:4002`) | Candidato: via escolhida nessa chamada; convenção **[nao verificado]**. (`services/materializacao.py:1283`) | Atomic de `_materializar_uma`, com contexto de materialização em voo. (`services/materializacao.py:1226`) | `False` → `_Reprovada` dentro do atomic; exceções viram erro no relato. (`services/materializacao.py:1285`, `:1289`, `:1292`) | Um escritor na reentrada e preservação da via do materializador. (`tests/test_pep_reentrada.py:109`, `:134`) **Sem teste de C**. (`models.py:4002`) |
| **W7 — `services/materializacao.py:1437`** | `juiz_aprovou` × tipos admitidos pelo lastro; **escolha bloqueada hoje**, conforme explicação abaixo; `resposta=None`. (`services/materializacao.py:1424`, `:1433`, `:1437`; `juizes.py:709`) | C quando a chamada efetivamente ocorre e é aceita. (`services/materializacao.py:1437`; `models.py:4002`) | Candidato: `juiz_aprovou`; convenção **[nao verificado]**. (`services/materializacao.py:1437`) | Sem atomic próprio na função. Pela rota do signal de fechamento, herda o atomic de `DisputaSupervisao.fechar`. (`services/materializacao.py:1396`, `:1439`; `signals.py:165`; `models.py:1969`) | Sucesso → inclui PK; `False` → omite. (`services/materializacao.py:1437`, `:1438`) | Positivo, repetição e recusa: `tests/test_porta_desfecho_pergunta.py:222`, `:234`, `:256`. **Sem teste de C**. (`models.py:4002`) |
| **W8 — `models.py:3386`** | `fantasma` × sem filtro de tipo; não altera resposta. (`models.py:3378`, `:3386`) | C se aceita; nenhuma batida é materializada por essa função. (`models.py:3382`, `:3386`, `:4002`) | Candidato: `fantasma`; convenção **[nao verificado]**. (`models.py:3386`) | Sem atomic próprio no método; consumidor de produção envolve a chamada em atomic por pergunta. (`models.py:3378`; `reconciliador.py:1147`, `:1148`) | Propaga bool; já fantasma e carimbada → `False` antes da porta. (`models.py:3383`, `:3386`) | Viva recusada/morta aceita: `tests/test_o261e_f2_porta_carimbo.py:665`, `:676`. **Sem teste de C**. (`models.py:4002`) |

**W7 exige separar legado de fluxo atual:** o queryset exige `materializada_em IS NULL`, mas o gate de escolha exige `materializada_em IS NOT NULL`; portanto escolha não chega normalmente a `:1437`. Alteração concorrente entre leitura e gate **[nao verificado]**. (`services/materializacao.py:1424`, `:1433`; `juizes.py:682`, `:709`)

As 37 escolhas históricas `juiz_aprovou` e os demais quantitativos do brief são somente o dado raspado fornecido; distribuição por execução/chamada e recontagem **[nao verificado]**.

Além das oito chamadas produtivas, há estas chamadas reais nos testes: [nao verificado]

| Chamada adicional | Via × tipo | Ato / motivo esperado | Atomic | Retorno / prova |
|---|---|---|---|---|
| `tests/test_o261e_f2_fila.py:194` | `juiz_aprovou` × hora; validar. (`:175`, `:194`) | Nenhum: save falha; motivo não deve persistir. (`:192`, `:194`) | Sem bloco lexical próprio; classe `TestCase`; atomic do runner **[nao verificado]**. (`:151`, `:191`) | Exceção e memória restaurada. (`:193`, `:195`) |
| `tests/test_o261e_f2_fila.py:205` | `juiz_aprovou` × hora; lavrar. (`:175`, `:205`) | Nenhum: lavra falha. (`:203`) | Mesma base `TestCase`; atomic do runner **[nao verificado]**. (`:151`, `:202`) | Exceção e restauração. (`:204`, `:206`) |
| `tests/test_o261e_f2_fila.py:221` | `juiz_aprovou` × hora; lavrar. (`:175`, `:221`) | Nenhum: save falha após lavra. (`:218`, `:219`) | Mesma base `TestCase`; atomic do runner **[nao verificado]**. (`:151`, `:217`) | Exceção e lavra restaurada. (`:220`, `:222`) |
| `tests/test_o261e_f2_fila.py:228` | `juiz_aprovou` × hora; sem resposta. (`:175`, `:228`) | C no desenho futuro; motivo candidato `juiz_aprovou` **[nao verificado]**. (`models.py:4002`) | Mesmo limite de `TestCase`; save interno. (`tests/test_o261e_f2_fila.py:151`; `models.py:3532`) | `True`, C6 mockado como morto; verifica carimbo/autor/via. (`tests/test_o261e_f2_fila.py:227`, `:230`) |
| `tests/test_o261e_f2_porta_carimbo.py:725` | `juiz_aprovou` × texto legado; responder. (`:636`, `:725`) | Nenhum: C6 mantém viva. (`:725`, `:731`) | Classe `TestCase`; atomic do runner **[nao verificado]**. (`:611`) | `False`, banco e memória sem carimbo novo. (`:725`, `:730`) |
| `tests/test_o261e_f2_porta_carimbo.py:738` | Identidades e tranca × texto legado. (`:636`, `:736`) | Nenhum: via proibida nesta porta. (`models.py:3201`) | Classe `TestCase`; atomic do runner **[nao verificado]**. (`tests/test_o261e_f2_porta_carimbo.py:611`) | `ValueError`. (`:737`, `:738`) |
| **Apelido `orig(...)` — `tests/test_pep_reentrada.py:96`** | Encaminha argumentos reais do materializador. (`:92`, `:96`, `:102`) | Mesma semântica da chamada interceptada; não é outro escritor. (`:96`, `:99`) | Herda o caminho materializador. (`:102`; `services/materializacao.py:1226`) | Propaga bool e fotografa sucessos. (`tests/test_pep_reentrada.py:97`, `:99`) |
| **Apelido dinâmico `orig(...)` — `tests/test_pep_reentrada.py:81`** | Tupla inclui `carimbar`; obtém método por `getattr`. (`:67`, `:85`) | Mesma semântica da porta interceptada. (`:81`, `:87`) | Herda a chamada observada; wrapper não abre atomic. (`:70`, `:83`) | Propaga resultado real. (`:81`) |

Mocks e strings de código não constituem entradas adicionais na porta real: há substituições por Mock, asserts sobre esses mocks e exemplos sintéticos para o analisador AST. (`tests/test_e4b_c6_memoria.py:27`; `tests/test_e4b_classe_e_memoria.py:287`, `:399`, `:1001`; `tests/test_o261e_f2_fila.py:63`, `:66`)

O censo estático encontrou essas oito chamadas produtivas e nenhum apelido produtivo adicional; resolução dinâmica não literal além do código identificado **[nao verificado]**. O utilitário existente de referências considera atributos, nomes soltos e strings para `getattr`/`methodcaller`. (`tests/test_o261e_f2_porta_carimbo.py:450`)

**2. Atomic comum e censo dos escritores de DP**

A ordem atual da porta é: aplicar carimbo e eventuais R/V em memória → C6 → fotografia/lavra opcional → `save` → `True`. Nenhuma chamada a `registrar` está nessa sequência. (`models.py:3212`, `:3213`, `:3222`, `:3230`, `:3233`, `:3235`, `:3240`)

O encaixe técnico que preserva essa leitura atual é colocar **`save` e `registrar` sob um atomic comum dentro de `carimbar`**, com a captura da exceção fora desse bloco e `True` somente depois dos dois. Os atomics internos existentes passam a ficar aninhados nesse bloco comum; falha no DP ou no seu outbox precisa alcançar também o save da pergunta. Isso é inferência de desenho a partir das duas portas. (`models.py:3231`, `:3235`, `:3532`, `:4095`, `:4096`, `:4097`)

A DP usaria o mesmo `quando` já escolhido para o carimbo, o mesmo autor e a via recebida; não outro `now()`. O instante e o autor já estão disponíveis na porta. (`models.py:3207`, `:3208`, `:3212`)

Há dois efeitos associados: o `post_save` emite outbox síncrono e agenda fechamento de becos em `on_commit`; `registrar` também acorda células. O atomic comum faz o callback enxergar a DP ao sair da transação, e mantém pergunta/DP/outbox na mesma unidade de rollback. (`signals.py:240`, `:259`; `models.py:4097`)

`True` continuará significando sucesso **na transação corrente**: um atomic externo ainda pode ser revertido depois do retorno da porta. (`models.py:3240`; `reconciliador.py:593`; `services/materializacao.py:1226`)

Censo das **14 chamadas produtivas** a `DecisaoPergunta.registrar`:

| Escritor lógico | O que registra |
|---|---|
| Ressuscitação | RESPOSTA/VALIDACAO projetadas ainda sem linha, antes de limpar campos. (`models.py:1610`, `:1613`) |
| Fechamento da disputa | SILENCIO após `fechar_por_admin`. (`models.py:1985`, `:1988`) |
| Remoção de motivo | RETRATACAO antes de carimbar identidade. (`models.py:2120`, `:2125`) |
| Nascimento da pergunta | RESPOSTA/VALIDACAO recebidas no nascimento. (`models.py:3100`, `:3102`) |
| Resposta do colaborador | RESPOSTA e conservação do legado. (`services/disputa_emissao.py:1525`) |
| Reabertura/recusa | REABERTURA ou RECUSA, com fotografia. (`services/disputa_emissao.py:1619`, `:1631`) |
| Resposta automática | RESPOSTA e conservação do legado. (`services/disputa_emissao.py:1677`) |
| Resposta pela trilha | RESPOSTA. (`services/disputa_emissao.py:1698`) |
| Validação | VALIDACAO e conservação do legado. (`services/validacao.py:164`) |
| Devolução | DEVOLUCAO e conservação do legado. (`services/validacao.py:230`) |
| Absorção pela ausência | ABSORCAO após `carimbar=True`. (`services/materializacao.py:567`, `:569`) |
| Decisão administrativa do dia | SILENCIO após `fechar_por_admin`. (`app/ponto/services/veredito_celula.py:190`, `:195`) |
| Backfill de decisões | SILENCIO com instante do carimbo. (`management/commands/backfill_decisoes_pergunta.py:324`, `:325`) |
| Backfill de fio morto | RESPOSTA/VALIDACAO conservadas pela função comum. (`management/commands/backfill_fio_morto_e4b.py:141`) |

O escritor físico é `bulk_create` dentro de `registrar`; `linhas_da_projecao_sem_linha` apenas produz dicionários. (`models.py:4096`, `:4076`, `:4080`)

As linhas `:307` e `:319` citadas no brief para o backfill são montagem de relatório/DIFF nesta cópia; a chamada efetiva é `:324`. (`management/commands/backfill_decisoes_pergunta.py:307`, `:319`, `:324`)

Fora do nascimento de atos em produção, existe escritor operacional genérico da **máscara da sombra**: resolve o modelo dinamicamente e usa `_base_manager.bulk_update` sobre `ato_texto`/`ato_motivo`. (`bin/sombra_regras_pessoais.py:142`; `bin/sombra_mascara_pessoal.py:46`, `:51`, `:62`, `:64`)

**3. Alcance de `_desfazer_em_memoria`**

| Cobertura | Situação conferida |
|---|---|
| Carimbo e assinaturas | Restaura `materializada_em`, `materializada_por_id`, `via_resolucao`, `respondida_em`, `validada_em`, `validada_por_id`. (`models.py:3209`, `:3244`, `:3245`, `:3246`) |
| Memo de vitalidade | Remove `_pv_memo`. (`models.py:3249`) |
| Lavra | Restaurada por outro helper: `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`models.py:3237`; `veredito_lavrado.py:284`) |
| **`_par_carregado`** | **Ficaria fora:** save bem-sucedido atualiza a fotografia; falha posterior de DP e restauração dos campos não restaurariam essa fotografia. (`models.py:3534`, `:3244`) |
| DP e EventoCelula | Não são desfeitos pelo helper de memória; dependem do rollback SQL abrangendo save e registrar. (`models.py:3244`, `:4096`, `:4097`; `signals.py:259`) |
| Cache de decisões | Não invalida `self.decisoes` prefetched; o leitor de histórico usa esse cache, enquanto o helper só remove `_pv_memo`. (`models.py:3648`, `:3651`, `:3249`) |
| Rollback externo posterior | Depois de `True`, o método já saiu; reversão posterior do chamador não passa pelo seu `except`. (`models.py:3236`, `:3240`; `services/materializacao.py:1226`) |

Há testes de falha de save, falha de lavra e falha de save depois da lavra; **não cobrem falha de `registrar` após save bem-sucedido**. (`tests/test_o261e_f2_fila.py:187`, `:198`, `:208`)

O teste de DP+evento existente verifica o atomic de `registrar` com infraestrutura simulada; não prova o futuro conjunto pergunta+DP dentro de `carimbar`. (`tests/test_e4_expand.py:353`, `:355`, `:375`, `:378`)

**4. Ato duplo: absorção**

A única das oito chamadas que já grava DP diretamente para aquele carimbo é **W5: `materializacao.py:567` → ABSORCAO em `:569`**. Os eventos de W4 e W8 são trilhas de auditoria, não chamadas a DP. (`services/materializacao.py:567`, `:569`; `services/disputa_emissao.py:2084`; `reconciliador.py:1150`)

O encaixe do pré-desenho é transportar à porta o contexto de absorção — pergunta escape e ausência — e fazer a própria porta registrar **ABSORCAO em lugar da conclusão genérica**, retirando no futuro o `registrar` do chamador. A forma da assinatura ainda é **[nao verificado]**; os dados necessários e a exigência das duas referências já existem. (`services/materializacao.py:571`, `:572`; `models.py:4035`)

**Via × tipo não basta para distinguir absorção:** `declaracao × hora` e `ausencia_dia × hora` também podem chegar por outros caminhos. Portanto a porta precisa desse contexto explícito; deduzir ABSORCAO apenas pela via perderia a distinção. (`services/materializacao.py:555`, `:567`, `:1283`; `services/disputa_emissao.py:2007`; `juizes.py:1802`)

`registrar` não faz deduplicação: executa `bulk_create`. A proteção atual da absorção repetida é o filtro de perguntas ainda sem carimbo; o teste existente exige uma linha após duas materializações. (`models.py:4096`; `services/materializacao.py:539`; `tests/test_e1_entradas.py:242`, `:246`)

**5. `carimbar_identidade`**

Ela grava via, instante e autor; opcionalmente grava `recusa_motivo` no mesmo save. `manter_carimbo=True` conserva instante/autor anteriores e troca a via. (`models.py:3265`, `:3268`, `:3269`, `:3271`, `:3273`)

As vias aceitas são `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado`. `competencia_trancada` permanece legível como identidade, mas não é aceita por esse escritor. (`models.py:2588`, `:3178`, `:3261`)

Ela fica fora do novo ato de conclusão de `carimbar` porque a identidade é julgada pelo chamador e encerra pela própria via; consultar C6 nessa porta seria circular. O código mantém explicitamente essa separação entre as duas portas. (`models.py:3160`, `:3162`, `:3252`)

Isso não significa ausência de DP nas oper

[truncado em 20000 de 27129 caracteres]

## 15/11 14:07 · sessao 68dfeaf9bb7c · REVISAO-CORTES-1358 · verificador · origem=Code

AVAL-DEVOLVIDO: CORTES-1358 A | perguntas 1

Uso **PROCEDE** para apontar motivo de devolução, conforme a regra da revisão. Nas referências aos anexos, a numeração começa na primeira linha de cada bloco fornecido. (`avais_todos.txt/revisao_do_aval.txt:2`)

1. **PROCEDE — as referências não conferem integralmente nesta cópia.**

   - `_leitura_conservada` e `_q_validada_pelo_ato` não estão implementadas nesta árvore. Nas posições citadas encontram-se `_retratada_com_trilha` e `save`; portanto, **não é simples deslocamento**. A entrega atribui aquelas funções à raia F2 `8c6577245`, documentada separadamente. (`app/chamados/juizes.py:285`; `app/chamados/models.py:3522`; `entrega.md:7`; `entrega.md:10`; `app/docs/RELATO.md:37`)
   - Documentalmente, são apresentadas como faces Python/SQL da mesma pergunta; a equivalência das implementações da F2 neste snapshot fica **[nao verificado]**. O código disponível lê a projeção em Python e usa `validada_em IS NULL` no SQL. (`app/docs/RELATO.md:14`; `app/chamados/juizes.py:1388`; `app/chamados/juizes.py:1404`; `app/chamados/models.py:3468`)
   - **`disputa_emissao.py:1480–1481` não zera V.** Essas linhas conservam decisões anteriores e atribuem a resposta. O zeramento está em **1494–1495**, deslocamento de **+14 linhas** em relação ao par citado no aval. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1494`)
   - A referência da entrega a `models.py:4057`, conservação da projeção sem linha, permanece correta; as referências da F2 a `atos_pergunta.py:19–42` não podem ser relocadas, pois esse módulo não está disponível nesta cópia: **[nao verificado]**. (`app/chamados/models.py:4057`; `entrega.md:38`)
   - **187/186 têm fonte no dado fornecido:** 187 perguntas, 186 respostas do colaborador e uma automática. O RELATO também registra esses números. Isso comprova a origem documental, sem nova aferição de produção. (`entrega.md:14`; `entrega.md:19`; `entrega.md:22`; `app/docs/RELATO.md:13`)

2. **NAO PROCEDE — não há colisão demonstrada com a proibição de construtor novo. F2 em voo: sim.** Há registro de F2 construída, com c1/c2/c3 e bloqueio herdado da F1. O COMPLEMENTO-2 admite continuidade da espinha já em voo, na mesma cópia; não libera job novo na pista. A continuidade física dessa raia nesta sessão permanece **[nao verificado]**. (`app/docs/RELATO.md:37`; `avais_todos.txt/complemento2_queima.txt:2`; `avais_todos.txt/queima_ate_a_redefinicao.txt:10`)

3. **NAO PROCEDE quanto a contradição literal demonstrada.** O dado não permite garantir “0 das 187”: a natureza real ou projetada da resposta automática de pg32660 está expressamente **[nao verificado]**. Isso impede certificar o selo, mas não prova que ele seja impossível. Há um “senão” geral vigente: condição que não fecha implica PAROU. (`entrega.md`, seção final `[nao verificado]`; `avais_todos.txt/cortes_1358.txt:5`; `CLAUDE.md:37`)

4. **NAO PROCEDE quanto a alcance além do declarado.** A exige sombra, preservação de estado e zero FM/DiaPago tocados. O universo contém 169 exportadas e cinco perguntas com DiaPago; impacto efetivamente zero permanece **[nao verificado]**, não pode ser deduzido apenas da intenção do corte. (`avais_todos.txt/cortes_1358.txt:5`; `entrega.md:14`; `entrega.md:16`)


AVAL-DEVOLVIDO: CORTES-1358 C2 | perguntas 1, 3

1. **PROCEDE — parte das premissas só tem registro documental, e faltam fontes dos números.**

   - O portão DECLARADO-X-GRAVADO e o universo 163 estão registrados; sua implementação não está demonstrada nesta cópia. (`app/docs/BACKLOG.md:1995`; `app/docs/RELATO.md:21`; implementação: **[nao verificado]**)
   - **8 × 79 tem fonte citável no RELATO**, que o qualifica literalmente como diferença não verificada. Os logs primários dessa reconciliação permanecem **[nao verificado]**. (`app/docs/RELATO.md:21`; `app/docs/RELATO.md:66`)
   - **15 do laço e 29 metades em exportada:** constam do próprio aval, mas não foi fornecido resultado de sonda que os sustente; sua medição fica **[nao verificado]**. (`avais_todos.txt/cortes_1358.txt:7`)
   - Existe I18, porém ele conta validação humana, ausência de **carimbo** e recusa vazia, excluindo usuários de sistema. Isso não equivale ao contador solicitado, “validada sem **batida** e sem recusa”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1076`)

2. **NAO PROCEDE no literal completo.** C2(2) pede instrumento novo, não mera leitura; entretanto, C2 mantém a volta de código aguardando a redefinição e exige CONTRATO-DE-INSTRUMENTO antes. Isso respeita o COMPLEMENTO-3. O COMPLEMENTO-2 não fornece uma dispensa geral para instrumentos. (`avais_todos.txt/cortes_1358.txt:7`; `avais_todos.txt/complemento3_queima.txt:5`; `avais_todos.txt/complemento2_queima.txt:3`; `CLAUDE.md:394`)

3. **PROCEDE — a medida exigida não tem produtor comprovado.** “NOVA = 0 por EXECUÇÃO” não vem acompanhada de função, identificação das execuções ou referência anterior que separe nova ocorrência de legado; esses elementos ficam **[nao verificado]**. I18 não supre a ausência porque mede outro predicado. Não falta o “senão” geral; falta demonstrar a medida pela função real exigida. (`avais_todos.txt/cortes_1358.txt:7`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`; `CLAUDE.md:35`; `CLAUDE.md:37`)

4. **NAO PROCEDE quanto a extrapolação declarada.** C2 proíbe reabrir/retratar fechadas e tocar as 29 metades exportadas; a reconciliação em produção é somente leitura. A identidade das 29 e o cumprimento dessas restrições em execução permanecem **[nao verificado]**. C2 não contém referência numérica de código cujo deslocamento possa ser conferido. (`avais_todos.txt/cortes_1358.txt:7`)


AVAL-DEVOLVIDO: CORTES-1358 E | perguntas 1, 3

1. **PROCEDE parcialmente — estrutura e ordem conferem; o universo não está integralmente comprovado.**

   - Existem `DecisaoPergunta`, `RETRATACAO`, `ato_motivo` e `ato_em`; `materializada_em` existe na pergunta. O schema comporta o ato e o motivo `'obsoleta'`, mas isso não demonstra um escritor L2 já implementado. (`app/chamados/models.py:3982`; `app/chamados/models.py:4001`; `app/chamados/models.py:4016`; `app/chamados/models.py:4019`; `app/chamados/models.py:3502`)
   - **Marcação sem campo novo é estruturalmente possível** pela trilha existente, com ação, objeto e dados antes/depois. Não há campo dedicado `backfill` na DP; a convenção concreta para L2 permanece **[nao verificado]**. `ato_texto` possui significado de texto da resposta, portanto sua existência não comprova um marcador disponível para qualquer uso. (`app/core/models.py:296`; `app/core/models.py:300`; `app/core/observ.py:133`; `app/chamados/models.py:4014`; `app/chamados/models.py:3996`)
   - **52 e 39 exportadas têm registro documental** no RELATO. **13 abertas** consta do aval e fecha a subtração, mas sua classificação como abertas e a medição primária ficam **[nao verificado]**. (`app/docs/RELATO.md:4`; `avais_todos.txt/cortes_1358.txt:9`)
   - **O literal E4-EXPAND-CONTRACT DIZ a ordem:** “E4c MIGRAR LEITORES […] so depois da E4b medida”. Portanto, não cabe responder “não diz”. A referência `LEIS.md:148` permanece na mesma linha e contém apenas vocabulário, conforme corrigido pelo complemento. (`avais_todos.txt/E4C/aval_expand_contract.txt:1`; `app/docs/LEIS.md:148`; `avais_todos.txt/complemento_cortes_1358.txt:3`)

2. **NAO PROCEDE quanto a colisão demonstrada.** E autoriza expressamente **ensaio de escrita na sombra**; não ordena job novo na pista nem abertura de construtor. A ordem de backfill antes da migração encontra respaldo no aval anterior. O apply de produção continua condicionado a PAREI `!`. (`avais_todos.txt/cortes_1358.txt:9`; `avais_todos.txt/E4C/aval_expand_contract.txt:1`)

3. **PROCEDE — o ensaio L2 e sua idempotência não têm execução comprovada pelo código disponível.** O backfill existente descreve conservação de resposta/validação e outros atos, não esse backfill L2. `registrar` faz inserção por `bulk_create`, sem deduplicação nessa porta; chamá-la novamente com as mesmas linhas não produz, por si, “0 linha nova”. A rotina que seleciona o passivo e garante a segunda rodada fica **[nao verificado]**. Isso não prova impossibilidade de implementação, mas impede considerar a condição executável já demonstrada. O “senão” geral existe em `CLAUDE.md:37`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:9`; `app/chamados/models.py:4095`; `app/chamados/models.py:4096`)

4. **NAO PROCEDE quanto a toque fora do declarado.** O ensaio escreve DP e realmente acorda células, emitindo `EventoCelula`; portanto, não é leitura pura. O alcance declarado já inclui efeitos sobre FM e rubricas das exportadas **na sombra**, enquanto o apply em produção é PAREI `!`. (`app/chamados/models.py:4096`; `app/chamados/models.py:4097`; `app/chamados/models.py:3331`; `app/ponto/portas/celula.py:954`; `avais_todos.txt/cortes_1358.txt:9`)

   Há porta de recalculo que pode escrever fechamento inclusive em exportada; entretanto, movimento efetivo de FM/TXT neste ensaio e a cadeia completa desde cada DP permanecem **[nao verificado]**. Não se pode afirmar nem movimento certo, nem impacto zero. (`app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:1810`)

## 15/11 14:07 · sessao aa265cd5f3ea · RESPOSTA-AUTOMATICA-SOBRE-VALIDADA · censo · origem=Code

**RESPOSTA-AUTOMATICA-SOBRE-VALIDADA: registro textual do censo, sem cura.** Registro externo da obra: [nao verificado].

**Quantidade na frota: [nao verificado].** O dado fornecido contém os avais, mas não contém resultado dessa contagem. Não usei números históricos dos documentos da cópia como medição de produção. [nao verificado]

A função está agora em **1654–1679**; a faixa **1640–1666** citada pelo aval deslocou-se. A chamada da foto continua na linha **48**. A identidade Git do snapshot e o diff exato contra `origin 4b75cba` são [nao verificado], pois a cópia não contém metadados Git. [disputa_emissao.py:1654](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1654), [foto_recebida.py:48](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:48)

**1. Porta e chamadores**

`responder_automaticamente` tem estas recusas:

- **Texto anterior não vazio após `strip()`**: retorna `False`, antes de consultar o banco ou examinar o texto novo. [disputa_emissao.py:1663](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1663)
- **Texto novo vazio com instante não nulo**: levanta `RespondidaSemTexto` antes de alterar a instância. Portanto, “só recusa quando já há texto” está incompleto nesta cópia. A guarda recusa o par carimbo preenchido/texto vazio; não é uma validação incondicional do texto isoladamente. [disputa_emissao.py:1671](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1671), [fio.py:1595](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/fio.py:1595), [juizes.py:1528](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/juizes.py:1528)

**Não usa `validada_em` como impedimento.** O auxiliar chamado antes da alteração lê esse campo para conservar uma validação antiga sem ato correspondente. [disputa_emissao.py:1672](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1672), [models.py:4078](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:4078)

Quando aceita, grava diretamente `resposta_colab=texto` e `respondida_em=instante`. No mesmo `atomic`, registra uma `DecisaoPergunta` com `ato='resposta'`, `ato_em=instante`, `ato_texto=texto` e motivo constante; não informa `ato_por`. Também registra as projeções antigas sem linha que o auxiliar encontrou. Retorna `True`. [disputa_emissao.py:1672](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1672), [models.py:4000](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:4000), [models.py:4017](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:4017)

O censo encontrou **um sítio produtivo direto**, oito sítios diretos em testes e dois sítios dinâmicos em teste. Os imports encontrados são locais, sem apelido; as duas chamadas dinâmicas usam o nome da função como string. Chamadas externas à cópia: [nao verificado]. [foto_recebida.py:39](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:39), referências dos testes abaixo.

| Chamador — todos os sítios encontrados | O que faz depois |
|---|---|
| Produção: `api/foto_recebida.py:48`, import local em `:39` | Ignora o booleano; chama sempre `validar_pergunta`; incrementa `n` somente quando `r.get('ok')`; retorna `n`. [foto_recebida.py:48](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:48) |
| `test_porta_chamado_e3.py:46,48,53` | Em `:46`, exige `True` e repete; em `:48`, exige `False` com zero consultas; em `:53`, exige `False` e relê o texto original para conferir preservação. [test_porta_chamado_e3.py:43](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/test_porta_chamado_e3.py:43) |
| `test_e3_10_cura_no_escritor.py:62,75,85` | Em `:62`, espera exceção e confere banco/memória sem alteração; em `:75`, exige `True`, relê o par e verifica ausência do estado sem texto; em `:85`, exige `False` e zero consultas. [test_e3_10_cura_no_escritor.py:55](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/test_e3_10_cura_no_escritor.py:55) |
| `test_e4_expand_orm.py:176,177` | Exige `True`, depois `False`; confere projeção preservada nos demais campos e ato de resposta com o instante passado. [test_e4_expand_orm.py:172](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/test_e4_expand_orm.py:172) |
| Dinâmicos: `test_e4_expand.py:207,208` | O executor extrai a função pelo nome; exige `True`, depois `False`, confere projeção, ato e uma gravação. [test_e4_expand.py:204](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/test_e4_expand.py:204), [test_e4_expand.py:119](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/test_e4_expand.py:119), [e4_apoio.py:8](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/tests/e4_apoio.py:8) |

Na cadeia produtiva, `resolver_disputas_de_foto` seleciona disputas abertas com `foto_ausente` e perguntas desse motivo vinculadas à `batida_origem`, **sem filtro de validação**. Ignora também o retorno de `_validar_pelo_dono`, relê a disputa e incrementa `fechadas` antes de verificar se ela fechou. Se ainda aberta, pode aparar o motivo, fechar a disputa e anexar ocorrência. Assim, esse contador mede disputas **tratadas**, embora a API o exponha como `disputas_fechadas`. [foto_recebida.py:65](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:65), [foto_recebida.py:74](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:74)

As duas entradas de API primeiro anexam a foto, depois chamam esse kernel e devolvem o contador: `views.py:851,861,863` e `views_core.py:835,845,847`. [views.py:851](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/views.py:851), [views_core.py:835](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/views_core.py:835)

**2. Pergunta já validada e sem texto**

Para texto novo não vazio e instante preenchido, a escrita direta bem-sucedida é esta; os demais efeitos dependem das cascatas descritas adiante. [disputa_emissao.py:1671](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1671)

| Campo/estado | Antes → depois direto |
|---|---|
| `resposta_colab` | Vazio ou só brancos → texto recebido; no upload, `'[auto] Foto recebida em upload separado.'`. [disputa_emissao.py:1673](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1673), [foto_recebida.py:25](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:25) |
| `respondida_em` | Nulo **ou data anterior** → `instante`; não preserva o carimbo anterior. [disputa_emissao.py:1674](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1674) |
| `validada_em`, `validada_por` | Permanecem como estavam. [disputa_emissao.py:1676](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1676) |
| `materializada_em`, `materializada_por`, `via_resolucao`, `tipo_resposta`, demais campos da pergunta | Não são escritos diretamente pela função. [disputa_emissao.py:1676](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1676) |
| `DecisaoPergunta` | Acrescenta a resposta nova; pode conservar também resposta antiga e validação antiga sem correspondência por ato/instante, inclusive com texto anterior vazio. [models.py:4070](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:4070), [disputa_emissao.py:1677](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/disputa_emissao.py:1677) |
| `.respondida` | `False → True` se o carimbo era nulo; caso contrário, continua `True`. [models.py:3640](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:3640) |
| `.estado` | Continua `VALIDADA` se não materializada; continua `RESOLVIDA` se materializada. [models.py:3124](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:3124) |

Os leitores relevantes produzem estas consequências:

- **Validador chamado logo depois:** relê o banco, encontra resposta preenchida e devolve `{'ok': False, 'estado': 'ja_validada', 'msg': 'Pergunta ja validada.', 'erros': []}`. Esse caminho retorna antes de nova validação/materialização. [validacao.py:414](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/validacao.py:414)
- **C6 e face SQL:** validação preenchida continua matando a pergunta; uma resposta posterior não desassina nesta cópia. [juizes.py:1403](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/juizes.py:1403), [models.py:3468](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:3468)
- **App:** a consulta primeiro usa `q_vivas`; portanto essa pergunta continua fora do universo retornado. Não cabe afirmar que passou de pendente para histórico do app. [pergunta_no_app.py:52](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/pergunta_no_app.py:52)
- **`respostas_sem_veredito`/`pode_fechar`:** a pergunta continua excluída da lista por ter validação; ela própria não impede o fechamento por esses juízes. Outras perguntas podem impedir. [models.py:1905](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:1905), [models.py:1915](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:1915)
- **Materializador geral, se chamado posteriormente por outra via:** seleciona resposta preenchida + validação preenchida + materialização nula. Se `respondida_em` era nulo, a chamada automática pode colocar a pergunta nesse universo. Isso não prova que será materializada. [materializacao.py:1375](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/materializacao.py:1375)
- **Apresentação e histórico:** `resposta_display` passa a ler o texto novo; `respostas_anteriores` lê a sequência de decisões para identificar respostas superadas. [models.py:3662](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:3662), [models.py:3643](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:3643)

**Há efeitos indiretos:** o `save` acorda células pelo outbox; o registro das decisões também as acorda. Outro receiver agenda, no commit, a verificação de beco, que pode resolver perguntas e fechar a disputa. Depois, o chamador da foto ainda executa sua própria sequência de aparar/fechar. Portanto, “globalmente só dois campos mudam” seria incorreto; quais cascatas ocorreram em produção é [nao verificado]. [signals.py:229](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/signals.py:229), [signals.py:252](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/signals.py:252), [models.py:4095](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:4095), [becos.py:25](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/becos.py:25), [foto_recebida.py:74](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/api/foto_recebida.py:74)

**3. Sonda proposta, somente como texto**

A classificação usa **`competencia_do_dia_entregue`**, composição real de `janela_atual` — respeitando o corte da empresa — com `competencia_entregue`, que exige exportação vigente não invalidada. Sem data, a função devolve `None`; a sonda separa esse universo como indeterminado. [vigia_fio.py:843](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/services/vigia_fio.py:843), [janelas.py:60](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/ponto/janelas.py:60), [export.py:588](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/folha/export.py:588)

A medida temporal disponível é **registro de foto criado após a validação**, pelo episódio `batida_origem`. Não equivale a todos os uploads posteriores: atualizar uma `FotoBatida` existente não renova `criado_em`. Data do último recebimento: [nao verificado]. [models.py:2518](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/chamados/models.py:2518), [ponto/models.py:1036](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/ponto/models.py:1036), [registro_batida.py:313](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/app/ponto/registro_batida.py:313)

O envelope já executa o shell no tenant `juliani`. Execução desta sonda e resultados: [nao verificado]. [sonda_leitura.sh:239](/home/ronald/codex_ws/151b0666a04a8cabd726ac637c217730408872a8/bin/sonda_leitura.sh:239)

```python
# Execucao e resultados em prod: [nao verificado].
# Envelope: bin/sonda_leitura.sh:239-240.
import json
from collections import defaultdict

from django.db import connection, transaction
from chamados.models import PerguntaDisputa
from chamados.juizes import texto_vazio
from chamados.services.vigia_fio import competencia_do_dia_entregue
from ponto.janelas import janela_atual

assert connection.schema_name == "juliani", "tenant inesperado"

grupos = defaultdict(list)


def incluir(grupo, chave, pk, foto_estado):
    grupos[(grupo, tuple(chave))].append((pk, foto_estado))


with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
        )

    # Universo inteiro, incluindo disputas fechadas e respondida_em nulo.
    # chamados/models.py:2525-2527; juizes.py:1523-1525.
    qs = (
        PerguntaDisputa.objects.filter(validada_em__isnull=False)
        .select_related(
            "disputa__colaborador__empresa",
            "batida_origem__foto_batida",
        )
        .order_by("pk")
    )
    memo = {}
    grupos[("TOTAL", ())] = []

    for p in qs.iterator(chunk_size=500):
        if not texto_vazio(p.resposta_colab):
            continue

        empresa = p.disputa.colaborador.empresa

        # Funcao real: chamados/services/vigia_fio.py:843-865.
        entregue = competencia_do_dia_entregue(
            p.data_evento, empresa, memo=memo
        )
        if entregue is None:
            competencia, situacao = "SEM_DATA", "INDETERMINADA"
        else:
            fim = janela_atual(p.data_evento, empresa)[1]
            competencia = "%04d-%02d" % (fim.year, fim.month)
            situacao = "exportada" if entregue else "aberta"

        # Mesmo episodio: chamados/models.py:2518-2519.
        # Criacao do registro, nao ultimo upload:
        # ponto/models.py:1036-1041; registro_batida.py:298-313.
        b = p.batida_origem
        fb = getattr(b, "foto_batida", None) if b is not None else None
        if b is None:
            foto_estado = "SEM_EPISODIO"
        elif fb is None:
            foto_estado = "SEM_REGISTRO_FOTO"
        elif not fb.foto.name:
            foto_estado = "REGISTRO_SEM_ARQUIVO"
        elif fb.criado_em > p.validada_em:
            foto_estado = "REGISTRO_FOTO_CRIADO_APOS_V"
        else:
            foto_estado = "REGISTRO_FOTO_CRIADO_ATE_V"

        incluir("TOTAL", (), p.pk, foto_estado)
        incluir("TIPO_RESPOSTA", (p.tipo_resposta,), p.pk, foto_estado)
        incluir(
            "COMPETENCIA", (competencia, situacao), p.pk, foto_estado
        )
        incluir(
            "TIPO_COMPETENCIA",
            (p.tipo_resposta, competencia, situacao), p.pk, foto_estado
        )
        incluir("EVIDENCIA_FOTO", (foto_estado,), p.pk, foto_estado)

# Saida: rotulos, contagens e apenas PKs de PerguntaDisputa.
for (grupo, chave), linhas in sorted(grupos.items()):
    ids = [pk for pk, _ in linhas]
    posteriores = [
        pk for pk, estado in linhas
        if estado == "REGISTRO_FOTO_CRIADO_APOS_V"
    ]
    print(json.dumps({
        "grupo": grupo,
        "chave": chave,
        "n": len(ids),
        "pergunta_ids": ids,
        "n_registro_foto_criado_apos_v": len(posteriores),
        "pergunta_ids_registro_foto_criado_apos_v": posteriores,
    }, ensure_ascii=True))

print("FIM")
```

## 15/11 14:06 · sessao 18c2f107bdb2 · - · - · origem=Ronald (tmux)

E — evidências para consolidação:

- **Q1:** schema confere: `RETRATACAO` em `app/chamados/models.py:4001`, `ato_motivo` em `:4019`, `materializada_em` em `:3502`. O escritor existente em `:2121-2124` registra remoção de motivo, sem usar o literal `'obsoleta'`. `RELATO.md:4` declara 52 L2/39 exportadas; as 13 abertas resultam da subtração e do aval. A medição primária não está disponível: **[nao verificado]**.
- **A ordem existe:** o anexo `avais_todos.txt`, bloco `E4C/aval_expand_contract.txt:1`, diz “E4c MIGRAR LEITORES [...] só depois da E4b medida”. `app/docs/LEIS.md:148` é somente vocabulário, conforme o complemento.
- **Marcação sem campo novo:** há infraestrutura de trilha com ação livre, modelo, identificador, descrição e JSON em `app/core/models.py:295-306`, escrita por `app/core/observ.py:133-137`; convenção L2 implementada **[nao verificado]**. Não usar `ato_texto` como marcador: sua semântica é texto da resposta (`app/chamados/models.py:3996-3997`).
- **Q2 — NAO PROCEDE:** o aval autoriza expressamente ensaio de escrita na sombra; seu literal não manda abrir construtor nem enfileirar job na pista (anexo `cortes_1358.txt`, item E; `complemento_cortes_1358.txt`, item 1).
- **Q3 — PROCEDE:** não está definido o “senão” para segunda rodada com linhas novas ou diferenças nos resultados publicados (anexo `cortes_1358.txt`, item E). A porta `registrar` faz `bulk_create` sem deduplicação (`app/chamados/models.py:4095-4096`); o backfill existente descreve R/V e outros itens, sem L2 obsoleta (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3-12`, `:324-327`). A idempotência pedida ainda não tem implementação/prova disponível **[nao verificado]**.
- **Q4 — NAO PROCEDE quanto a extrapolação:** escrita DP, eventos, FM/TXT das exportadas na sombra e `PAREI !` no apply estão declarados em E. Criar DP realmente acorda células (`app/chamados/models.py:4096-4098`), que emitem `EventoCelula` (`:3314-3331`; `app/ponto/portas/celula.py:953-954`). O consumidor alcança o colaborador e outras operações (`app/chamados/reconciliador.py:1365-1388`). Movimento real de FM/TXT e alcance numérico: **[nao verificado]**. A porta de recalculo por evento pode escrever FM (`app/ponto/services/fechamento.py:1809-1810`), mas não comprovei cadeia automática direta de um INSERT DP até ela **[nao verificado]**.

## 15/11 14:06 · sessao 022385a85885 · E4C-F1V3 · construtor · origem=Code

Leis pendentes: L1 e L2 mantidas por conservação, sem backfill; frota [não verificado]. [Relato:1](chamados/tests/e4c_f1v6_entrega.txt:1)

[Entrega P1–P12, em 23 linhas](chamados/tests/e4c_f1v6_entrega.txt): RED→GREEN por propriedade, arquivo::função, comandos, escritores/leitores e pendências.

- RED antes da cura: **11 falhas por asserção**. [Evidência](chamados/tests/e4c_f1v6_red.txt:114)
- GREEN local: **112 testes, 2 skips**. [Resultado](chamados/tests/e4c_f1v6_green.txt:4)
- Mutantes: **19 c6 + 22 de tipos SQL**, mortos por asserção. [C6](chamados/tests/e4c_f1v6_mutantes.txt:7), [SQL](chamados/tests/e4c_f1v6_tipos_mutantes.txt:10)

Não fechou: execução ORM/SQL, frota P6, vigia e suite paralela reais. P5 foi tratada na adaptação de leitura; a cura canônica em models/dado permanece pendente pelas proibições desta volta. [Limites](chamados/tests/e4c_f1v6_entrega.txt:21)

LEI-AKITA padrão=**Parallel Change / Expand-Contract**. Certificação reservada ao Code/suite e opus final. [Relato:23](chamados/tests/e4c_f1v6_entrega.txt:23)

## 15/11 14:05 · sessao 55c4017566ce · - · - · origem=Ronald (tmux)

Complemento do censo:

| Classe | Locais efetivos |
|---|---|
| Produção, chamadas diretas | `app/chamados/reconciliador.py:406`, `:596`; `app/chamados/services/disputa_emissao.py:2007`, `:2081`; `app/chamados/services/materializacao.py:567`, `:1283`, `:1437`; `app/chamados/models.py:3386` |
| Testes, chamadas diretas reais | `app/chamados/tests/test_o261e_f2_fila.py:194`, `:205`, `:221`, `:228`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:725`, `:738` |
| Apelido efetivo em teste | `orig = PerguntaDisputa.carimbar`, seguido de `orig(self_p, via, ...)`: `app/chamados/tests/test_pep_reentrada.py:92`, `:96`; wrapper instalado em `:100`, exercitado em `:102` |
| Atribuições de Mock | `app/chamados/tests/test_e4b_c6_memoria.py:27`; `app/chamados/tests/test_e4b_classe_e_memoria.py:287`, `:1001` |
| Asserts sobre Mock | `app/chamados/tests/test_e4b_classe_e_memoria.py:399`, `:462`, `:490`, `:575`, `:1006`; `app/chamados/tests/test_e4b_c4v14_memoria.py:114` |
| Código sintético em strings, excluído das chamadas | `app/chamados/tests/test_o261e_f2_fila.py:63`, `:66`, `:67` |

Não encontrei alias de produção nem `getattr`/`methodcaller` efetivos para essa porta na varredura AST; ausência de construções dinâmicas não literais permanece [não verificado].

**Correção importante de W7:** escolha não chega hoje à chamada `materializar_pendentes_aprovadas → carimbar`: o queryset exige `materializada_em IS NULL` (`app/chamados/services/materializacao.py:1424`), o gate é `_pergunta_materializada_de_fato` (`:1433`), e o ramo escolha desse gate exige `materializada_em IS NOT NULL` (`app/chamados/juizes.py:709`). O ramo anterior de vias também exige materialização não nula (`app/chamados/juizes.py:682`). Isso descreve o fluxo sem mutação concorrente entre carga e gate; concorrência desse cenário: [não verificado].

As cinco choices de tipo não fecham o universo legado: teste existente cria `tipo_resposta='texto'` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:635`), e `carimbar` não testa `tipo_resposta` (`app/chamados/models.py:3186`). Assim, nas linhas sem filtro de tipo, escrever “sem restrição de tipo no chamador”, não “somente os cinco tipos das choices”.

## 15/11 14:05 · sessao b51f314182fb · - · - · origem=Ronald (tmux)

C2 — conclusão: **perguntas 1 e 3 PROCEDEM; 2 e 4 NÃO PROCEDEM no alcance literal**.

1. **PROCEDE — premissas incompletas.** O universo 163 e o portão DECLARADO-X-GRAVADO têm registro documental em `app/docs/BACKLOG.md:1995` e `app/docs/RELATO.md:21`. A reconciliação **8 na sombra × 79 em prod** está registrada com a ressalva literal **“[diferenca nao verificada]”** em `app/docs/RELATO.md:21`; a contagem 8 reaparece em `app/docs/RELATO.md:66`. Os respectivos logs primários não foram fornecidos; comprovação deles [nao verificado]. **15 do laço** e **29 metades em exportada** aparecem como números afirmados pelo aval, sem resultado de sonda citável fornecido [nao verificado].

   Existe um contador próximo, **I18**, mas ele conta `validada_em != NULL`, `materializada_em == NULL`, `recusa_motivo == ''` e exclui os usuários `sistema`/`sistema_bot`; portanto não demonstra a pergunta “validada sem **batida** e sem recusa” pela função real (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`). O juiz de materialização real distingue testemunha/batida por tipo de resposta (`app/chamados/juizes.py:665`, `:711`).

2. **NÃO PROCEDE — colisão não demonstrada no literal completo.** C2 pede instrumento novo, mas sua última frase preserva a espera da volta de código até a redefinição; isso é compatível com COMPLEMENTO-3, item g. COMPLEMENTO-2 libera continuidade de espinha em voo, não todo instrumento; nenhuma revogação geral decorre dele. Referências: anexo `avais_todos.txt`, blocos `cortes_1358.txt`, item C2; `complemento3_queima.txt`, item g; `complemento2_queima.txt`, itens 1–2. Numeração física dos anexos [nao verificado]. Construção imediata seria interpretação adicional, não ordem expressa no C2 [nao verificado].

3. **PROCEDE — a medida exigida ainda não tem produtor comprovado.** “NOVA = 0 por EXECUÇÃO” não apresenta baseline, identificação das execuções nem função produtora fornecida; “esperado = legado medido (15)” depende de medição não fornecida [nao verificado]. I18 não fecha essa lacuna porque mede carimbo, universo humano e recusa, não ausência de batida pelo juiz real (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`). A exigência de medida pela função real está em `CLAUDE.md:35`; contrato de instrumento anterior ao uso, em `CLAUDE.md:394`. O “senão” geral existe: condição que não fecha = PAROU (`CLAUDE.md:37`); não devolver apenas pela ausência da palavra “senão”.

4. **NÃO PROCEDE — extrapolação não demonstrada pelo texto.** O C2 proíbe reabrir/retratar fechadas e tocar as 29 metades em exportada, e manda reconciliar produção só por leitura: anexo `avais_todos.txt`, bloco `cortes_1358.txt`, item C2. A identidade das 29 e o efeito real de execução permanecem [nao verificado]. Não converter ausência de prova em afirmação de que houve escrita/dinheiro tocado [nao verificado].

Deslocamentos próprios de C2: não há referência numérica a linha de código no seu texto; a fonte citada é `logs/BUG-PAR-POUSA/aval.txt`, item 2, entregue apenas no anexo. O registro local de C7 permanece em `app/core/juizes.py:302`; `pode_fechar` está em `app/chamados/models.py:1911`, e a exclusão de validadas de `sem_veredito_entre` está em `:1905`.

## 15/11 14:05 · sessao 76d3ec15c22a · - · - · origem=Ronald (tmux)

A — sugestão: `AVAL-DEVOLVIDO: CORTES-1358 A | perguntas 1, 3`, ressalvada a avaliação do pai sobre “sem senão”. A ausência das duas funções nesta árvore é verificável pela busca somente leitura; as linhas citadas hoje contêm outros métodos. (`app/chamados/juizes.py:285`; `app/chamados/models.py:3522`).

1. **PROCEDE: referências e premissas não conferem integralmente neste snapshot.**

   - `_leitura_conservada` e `_q_validada_pelo_ato` não existem nos arquivos Python desta cópia. `juizes.py:285` define `_retratada_com_trilha`; `models.py:3522` define `save`. Não se trata de simples deslocamento dos símbolos: a entrega fornecida atribui essas funções à raia F2 `8c6577245`, que o RELATO identifica como outra cópia. (`app/chamados/juizes.py:285`; `app/chamados/models.py:3522`; `app/docs/RELATO.md:37`).
   - A pergunta “são duas faces da mesma pergunta?” tem fonte documental: o RELATO chama `_q_validada_pelo_ato` de face SQL do degrau 0 do C6. A equivalência da implementação da F2 não pode ser conferida nesta árvore. (`app/docs/RELATO.md:14`; implementação F2: [nao verificado]).
   - O HEAD lê `validada_em` diretamente e considera a pergunta não viva se houver validação; a face SQL atual exige `validada_em IS NULL` para trazer pergunta viva. Não contém aqui o desempate `V >= R` citado da F2. (`app/chamados/juizes.py:1388`; `app/chamados/juizes.py:1402`; `app/chamados/juizes.py:1404`; `app/chamados/models.py:3468`).
   - **Deslocamento da porta:** `disputa_emissao.py:1480-1481` não zera V: conserva projeção sem linha e atribui resposta. O zeramento está em **1494-1495**; portanto +14 linhas para o par alegado no aval. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1495`).
   - As referências de `entrega.md` à porta `1485-1496`, ao automático `1654-1680` e à projeção sem linha `models.py:4057` conferem no HEAD, sem deslocamento. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1654`; `app/chamados/services/disputa_emissao.py:1679`; `app/chamados/models.py:4057`).
   - **187/186 têm fonte:** o RELATO registra 187 perguntas, 169 exportadas, 186 respostas reais e uma automática, com remissão à entrega LU2-13 e às sondas fornecidas. É prova documental herdada, sem nova aferição. (`app/docs/RELATO.md:13`; `app/docs/RELATO.md:48`; `app/docs/RELATO.md:56`).
   - A entrega fornecida deixa expressamente sem conferir se a resposta automática de pg32660 tem linha `RESPOSTA` real. Portanto “as 187 têm R e V ambos apenas projetados” e a possibilidade de conciliar o RED real com o selo universal 0/187 permanecem [nao verificado].

2. **NAO PROCEDE quanto à colisão construtor novo/COMPLEMENTO-2.**

   **F2 em voo: sim.** Há F2 construída em cópia própria, com c1/c2/c3, entrega e bloqueio herdado da F1; o registro posterior ainda manda ataque à F2. Isso é continuidade de obra existente, enquadrada no COMPLEMENTO-2 fornecido, não abertura de obra nova. (`app/docs/RELATO.md:37`; `app/docs/RELATO.md:95`; anexo `complemento2_queima.txt`, item 1).
   
   Não há autorização em A para enfileirar job novo na pista durante a interdição; a execução concreta da suíte permanece [nao verificado]. O código da F2 e sua identidade física “mesma cópia” não estão acessíveis nesta árvore; a existência documental da raia está em `app/docs/RELATO.md:37`.

3. **PROCEDE quanto à condição ainda não fechada/documentada; evitar declarar contradição já demonstrada.**

   O critério “0 das 187 mudam” exige classificar a pergunta automática, que o dado fornecido deixa sem verificar. Se tiver linha RESPOSTA real posterior a V, o próprio RED manda desassinar; se tiver apenas projeção, recebe a exceção. Qual dos ramos ocorre é [nao verificado]. Não há prova de que os 187 satisfaçam simultaneamente a preservação de estado e o caso que morde; não afirmar que o conflito já aconteceu. (Dados: anexo `entrega.md`, seção `[nao verificado]`; universo e risco de lavra: `app/docs/RELATO.md:48`).
   
   O literal de A dá critérios PRONTO e proibições, mas não traz um ramo explícito para falha do selo 0/187, da lista 186 ou de 0 FM/DiaPago; a frase geral “PAREI só em lei ou !” não nomeia essa falha. (Anexo `cortes_1358.txt`, item A).

4. **NAO PROCEDE quanto a alcance adicional comprovado; impacto zero ainda [nao verificado].**

   A preserva o estado em relação ao main, proíbe gravado e exige sombra/0 FM/0 DiaPago. O universo inclui 169 exportadas e leitores capazes de relavrar, portanto o zero depende do ensaio, que não foi fornecido. Não afirmar que A já tocou ou que comprovadamente deixará de tocar dinheiro. (`app/docs/RELATO.md:48`; anexo `cortes_1358.txt`, item A; ensaio: [nao verificado]).

**Referências importantes para consolidação:** `app/docs/RELATO.md:88` registra CLASSE-D entre pouso F1 e F2; `:40` explicita F1 → CLASSE-D → F2 → E4d. Isso não prova colisão com A (“depois da c6” não diz “imediatamente”), mas impede tratar a vaga da F2 como livre/agendada só pela c6 sem examinar a fila. (`app/docs/RELATO.md:40`; `app/docs/RELATO.md:88`).

## 15/11 14:04 · sessao 55c4017566ce · - · - · origem=Ronald (tmux)

Censo de produção: 8 referências diretas a `PerguntaDisputa.carimbar`; não apareceu alias de atributo, nome solto, `getattr` ou `methodcaller` nessa porta na varredura AST. Referências conferidas: `app/chamados/reconciliador.py:406`, `:596`; `app/chamados/services/disputa_emissao.py:2007`, `:2081`; `app/chamados/services/materializacao.py:567`, `:1283`, `:1437`; `app/chamados/models.py:3386`. O utilitário existente de busca por referência inclui atributo, nome solto e string exata: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:450`.

As duas chamadas de `disputa_emissao.py` deslocaram +14 linhas: 1993→2007 e 2067→2081. As demais posições citadas continuam iguais, inclusive a porta em `app/chamados/models.py:3186`.

| Chamada | Via × tipo possível pelo código | Atomic no chamador | Retorno tratado | Teste existente | [nao verificado]
|---|---|---|---|---|
| `reconciliador.py:406` | `sem_turno_aberto`; não filtra tipo, exige motivos C1 e texto vazio (`reconciliador.py:333`, `:363`, `:365`) | A função não abre atomic (`reconciliador.py:341`, `:415`); somente atomic do `save` (`models.py:3532`) | False→False; True segue fechamento e retorna True (`reconciliador.py:406`, `:412`, `:415`) | Positivo e mordida do carimbo atual: `test_o261_lavra_le_a_lampada.py:148`, `:129`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `reconciliador.py:596` | `celula_absolveu`; sem filtro de tipo no laço (`reconciliador.py:585`, `:596`) | Atomic por pergunta, com trava do colaborador (`reconciliador.py:593`, `:594`) | False→continue; True incrementa; erro é registrado (`reconciliador.py:597`, `:599`, `:603`) | Via pelo orquestrador: `test_o261_lavra_le_a_lampada.py:172`; chamada direta: `test_selo_celula_absolveu.py:70`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `disputa_emissao.py:2007` | `via_pelo_fato`; qualquer tipo permitido pelo filtro; resposta=`validar`, lavrar=True (`disputa_emissao.py:1927`, `:2007`) | Wrapper usa atomic atual ou abre um (`disputa_emissao.py:1900`) | False→continue; True incrementa resolvidas (`disputa_emissao.py:2009`, `:2010`) | Controle positivo real: `test_pep_reentrada.py:229`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `disputa_emissao.py:2081` | `via_pelo_fato(pai)` ou `chamado_encerrado`; não filtra tipo apesar da prosa “escolha”; resposta=`responder` (`disputa_emissao.py:2060`, `:2079`, `:2081`) | Mesmo wrapper (`disputa_emissao.py:1900`) | False→continue; True registra evento de auditoria (`disputa_emissao.py:2082`, `:2084`) | Guarda de voo com C6 mockado, limite declarado: `test_pep_reentrada.py:267`, `:301`, `:304`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `materializacao.py:567` | Somente `hora`; `declaracao` ou `ausencia_dia`; resposta=`validar` (`materializacao.py:539`, `:555`, `:567`) | Atomic por irmã (`materializacao.py:563`) | True→registra ABSORCAO; False→lista recusadas e evento (`materializacao.py:569`, `:574`, `:575`) | Uma ABSORCAO por irmã, mesmas datas, repetição sem duplicata: `test_e1_entradas.py:239`; recusa com trilha: `test_o261e_f2_fila.py:238` |
| `materializacao.py:1283` | `hora`/`data_hora`/`escolha`; `declaracao` se `declarou_hora`, senão `via_pelo_fato` ou `chamado_encerrado`; resposta=`validar`, lavrar=True (`materializacao.py:1227`, `:1268`, `:1271`, `:1274`, `:1283`) | Atomic de `_materializar_uma` e contexto de voo (`materializacao.py:1226`) | False levanta `_Reprovada`, desfaz transação; erro acumulado (`materializacao.py:1285`, `:1289`, `:1292`) | Um escritor durante reentrada e via do materializador: `test_pep_reentrada.py:109`, `:134`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `materializacao.py:1437` | `juiz_aprovou`; qualquer tipo aprovado pelo lastro; resposta=None (`materializacao.py:1424`, `:1433`, `:1437`) | Sem atomic próprio na função (`materializacao.py:1396`, `:1439`); via signal165 durante `Disputa.fechar`, há atomic externo (`signals.py:165`, `models.py:1969`) | True→inclui pk; False→omite (`materializacao.py:1437`, `:1438`) | Positivo, repetição e lastro recusado: `test_porta_desfecho_pergunta.py:222`, `:234`, `:256`; resposta por validar: `test_o261e_f2_porta_carimbo.py:687`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `models.py:3386` | `fantasma`; não filtra tipo nem altera resposta (`models.py:3378`, `:3386`) | Sem atomic próprio; consumidor de produção abre um por pergunta (`reconciliador.py:1147`, `:1148`) | Propaga bool; já fantasma carimbada→False (`models.py:3383`, `:3386`) | Viva recusada e morta aceita: `test_o261e_f2_porta_carimbo.py:665`, `:676`; conclusão: sem teste (`models.py:4000`) | [nao verificado]

`via_pelo_fato` pode devolver **cinco vias**, não somente as duas mostradas na frota de escolhas: `cadastro_folga`, `adesao`, `ausencia_dia`, `completude_turno`, `chamado_encerrado` (`app/chamados/juizes.py:1793`, `:1794`, `:1802`, `:1810`, `:1811`). Os cinco tipos atuais são escolha, hora, posto_dropdown, escala_dropdown e data_hora (`app/chamados/catalogo/respostas.py:20`). Presença de cada combinação na produção: [não verificado].

A única chamada das oito que já registra DP no próprio chamador é a absorção: `app/chamados/services/materializacao.py:567`→`:569`. Os eventos dos outros chamadores são auditoria, não DP, por exemplo `disputa_emissao.py:2084` e `reconciliador.py:1150`.

A porta atual não contém atomic abrangendo seu corpo; chama `save` e retorna True (`app/chamados/models.py:3235`, `:3240`). O `save` abre atomic apenas para `super().save`, terminando antes de devolver à porta (`app/chamados/models.py:3532`, `:3534`). Portanto esse atomic não envolve um futuro `registrar` posterior dentro de `carimbar` (`app/chamados/models.py:3235`).

A nomenclatura do ato futuro e o significado de seu `ato_motivo` não estão implementados; o catálogo atual não possui conclusão e `ato_motivo` é texto livre sem choices (`app/chamados/models.py:4000`, `:4019`). A matriz prospectiva pode registrar “conclusão, nome pendente”, conservando a via dentro do ato conforme o aval fornecido; qualquer decisão adicional sobre codificar a via em `ato_motivo` é [não verificado].

## 15/11 14:04 · sessao 5aae7f45bbfd · - · - · origem=Ronald (tmux)

Censo feito por busca textual + AST em memória; nenhuma escrita nem teste executado. O snapshot não contém `atos_pergunta.py`, `_leitura_conservada` ou `instante_materializacao`; o leitor F1 descrito no anexo permanece **[nao verificado] nesta cópia**. [nao verificado]

**Escritores de `DecisaoPergunta`**

Existem 14 chamadas produtivas à porta `registrar`; nenhuma delas usa apelido de import. O escritor físico é `bulk_create`, sob atomic, seguido do evento da célula (`app/chamados/models.py:4095`, `:4096`, `:4097`).

| Função | Chamada / ato |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Preserva RESPOSTA/VALIDACAO sem linha — `app/chamados/models.py:1610` |
| `DisputaSupervisao.fechar` | SILENCIO após `fechar_por_admin` — `app/chamados/models.py:1985`, `:1988` |
| `DisputaSupervisao.remover_motivo` | RETRATACAO, motivo textual “motivo … removido”, depois identidade — `app/chamados/models.py:2120`, `:2123`, `:2125` |
| `PerguntaDisputa._nascer` | Preserva RESPOSTA/VALIDACAO recebidas no nascimento — `app/chamados/models.py:3100`, `:3102` |
| `responder_pergunta_disputa_core` | RESPOSTA + legado — `app/chamados/services/disputa_emissao.py:1522`, `:1525` |
| `reabrir_para_correcao` | REABERTURA ou RECUSA, com foto — `app/chamados/services/disputa_emissao.py:1619`, `:1631`, `:1632` |
| `responder_automaticamente` | RESPOSTA + legado — `app/chamados/services/disputa_emissao.py:1676`, `:1677` |
| `carimbar_resposta_pela_trilha` | RESPOSTA — `app/chamados/services/disputa_emissao.py:1697`, `:1698` |
| `_carimbar_validacao` | VALIDACAO + legado — `app/chamados/services/validacao.py:162`, `:164` |
| `devolver_validacao_recusada` | DEVOLUCAO + legado — `app/chamados/services/validacao.py:227`, `:230` |
| `_materializar_ausencia_de_resposta` | ABSORCAO se `carimbar` retornou verdadeiro — `app/chamados/services/materializacao.py:567`, `:568`, `:569` |
| `fechar_o_fio_do_dia` | SILENCIO após `fechar_por_admin` — `app/ponto/services/veredito_celula.py:190`, `:195` |
| backfill `aplicar` | SILENCIO, relógio `materializada_em` — `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `:325` |
| backfill `_executar_dia` | RESPOSTA/VALIDACAO conservadas — `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` |

Os dicionários de RESPOSTA/VALIDACAO conservadas nascem em `app/chamados/models.py:4076` e `:4080`; não são escrita física. A consulta de correspondência considera exclusivamente esses dois atos (`app/chamados/models.py:4070`).

Fora do fluxo de produção há uma exceção operacional explícita: a máscara da sombra resolve modelos dinamicamente e usa `_base_manager.bulk_update` (`bin/sombra_mascara_pessoal.py:46`, `:51`, `:62`, `:64`), incluindo os campos DP `ato_texto`/`ato_motivo` (`bin/sombra_regras_pessoais.py:142`). Não é chamada a `registrar` nem nascimento de ato.

**Choices**

`ATOS`: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao`, `absorcao` (`app/chamados/models.py:4000`, `:4001`, `:4002`).

Não há `ATO_MOTIVO` com choices: `ato_motivo` é `CharField(max_length=240, blank=True, default='')` (`app/chamados/models.py:4019`; também migration histórica `app/chamados/migrations/0051_decisao_pergunta.py:47`). Existe apenas a constante textual `MOTIVO_PROJECAO_SEM_LINHA` (`app/chamados/models.py:4054`).

A conclusão nova falta em `ATOS` (`app/chamados/models.py:4002`). A via ainda não tem campo próprio em DP: campos atuais em `app/chamados/models.py:4014` a `:4026`. Nome do novo ato e convenção para codificar a via em `ato_motivo`: **[nao verificado]**, decisão de desenho ainda não presente no código. ABSORCAO exige ambas as referências, e nenhum outro ato pode recebê-las (`app/chamados/models.py:4035`).

**Leitores e impacto**

| Leitor | Comportamento atual / impacto |
|---|---|
| `tarefa_do_dia._pela_decisao` | **Risco concreto:** pega a última linha em `:147`; despacha os oito atos conhecidos em `:148`–`:175`; ato novo cairá em `ato_desconhecido:<valor>, None` em `:176`. Referências: `app/chamados/services/tarefa_do_dia.py:147`, `:176`. |
| Carga de `tarefa_do_dia` | Carrega todos os atos ordenados por pergunta/instante/PK; guarda os anteriores a `agora`; RETRATACAO posterior tem tratamento específico. Conclusão entrará na sequência normalmente e poderá tornar-se a última. `app/chamados/services/tarefa_do_dia.py:410`, `:412`, `:414`. |
| `PerguntaDisputa.respostas_anteriores` | Considera só RESPOSTA e `ATOS_COM_FOTO`; conclusão será ignorada, preservando histórico existente. `app/chamados/models.py:3651`, `:3652`, `:3656`. |
| `disputa_emissao.respostas_anteriores` | Encaminha ao método do modelo; mesmo impacto. `app/chamados/services/disputa_emissao.py:2122`. |
| `fio.montar_chamados_fio` | Prefetch de todas as decisões, com autores; passa a carregar também a nova linha, consumida pelo leitor anterior. `app/chamados/services/fio.py:261`. |
| `modal_fio` | Usa `perg.respostas_anteriores`; não passa a exibir conclusão diretamente. `app/templates/chamados/partials/modal_fio.html:205`, `:209`, `:212`. |
| `linhas_da_projecao_sem_linha` | Só busca RESPOSTA/VALIDACAO com correspondência de instante; conclusão não elimina esse passivo. `app/chamados/models.py:4070`, `:4074`, `:4078`. |
| `backfill_decisoes_pergunta.admin_decidiu_sem_ato` | Só SILENCIO/VALIDACAO fazem a pergunta sair do conjunto sem ato. Conclusão não será reconhecida por esse filtro. `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:105`. |
| Rechecagem do apply desse backfill | Repete filtro SILENCIO/VALIDACAO; mesmo limite. `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`. |
| `foto_e4b` | Fotografa todas as DP do colaborador e todos os campos concretos: a nova conclusão aparecerá no diff como linha criada. `app/chamados/services/foto_e4b.py:250`, `:251`, `:280`. |
| `atos_pergunta.leitura` | Ausente da cópia; contrato/impacto da árvore F1 fornecido pelo anexo **[nao verificado]**. |
| Máscara da sombra | Lê e mascara `ato_texto`/`ato_motivo` de todas as linhas, inclusive novas; atenção se via for armazenada em motivo livre. `bin/sombra_regras_pessoais.py:142`, `bin/sombra_mascara_pessoal.py:51`, `:55`. |

Não achei outro leitor direto de DP, alias de DP, relação `decisoes` da pergunta ou SQL direto sobre sua tabela no código produtivo pesquisado; garantia além da análise estática: **[nao verificado]**.

**Provas existentes úteis**

- Escritor único e proibição de atualização/remoção: AST em `app/chamados/tests/test_e1_entradas.py:185`, teste em `:217`; tem mordida de update/delete em `:224`. Não segue aliases genéricos: comparação textual do nome em `:196`, `:201`.
- Imutabilidade funcional: `app/chamados/tests/test_e1_entradas.py:78`.
- ABSORCAO exatamente uma linha após duas materializações, referências e relógio iguais ao carimbo: `app/chamados/tests/test_e1_entradas.py:239`, `:242`, `:246`, `:248`, `:250`.
- DP + EventoCelula no mesmo atomic, inclusive falha do outbox: `app/chamados/tests/test_e4_expand.py:353`, `:375`, `:378`.
- Teste do ato novo de conclusão: **sem teste**; valor ainda ausente das choices (`app/chamados/models.py:4002`).

## 15/11 14:04 · sessao 6e36adaf9c8d · - · - · origem=Ronald (tmux)

Censo de chamadores conferido por busca textual e AST em memória, sem executar Django/banco nem escrever arquivos:

| Chamador | Depois do retorno |
|---|---|
| **Produção:** import local em `app/api/foto_recebida.py:39`, chamada `:48` | Ignora o `bool`; chama **sempre** `validar_pergunta(p.pk, user, disputa=...)` em `:51`; incrementa `n` somente se `r.get('ok')`, `:52-54`. |
| `app/chamados/tests/test_porta_chamado_e3.py:46` | Exige `True`; chama novamente em `:48`, exige `False` dentro de `assertNumQueries(0)`, `:47-48`. |
| Mesmo arquivo `:48` | Depois da asserção termina o teste, `:48-49`. | [nao verificado]
| Mesmo arquivo `:53` | Exige `False`, relê banco e exige texto original preservado, `:54`. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:62` | Espera exceção; confere motivo `respondida_sem_texto`, ausência de carimbo/texto/DP e memória intacta, `:63-69`. |
| Mesmo arquivo `:75` | Exige `True`; relê pergunta, confere par e predicado `respondida_sem_texto=False`, `:76-78`. |
| Mesmo arquivo `:85` | Exige `False` e zero queries, `:84-85`; termina teste. | [nao verificado]
| `app/chamados/tests/test_e4_expand_orm.py:176` | Exige `True`; chama outra vez em `:177`, exige `False`; compara foto e DP, `:178-179`. |
| Mesmo arquivo `:177` | Compara foto preservando demais campos e ato resposta com instante anterior, `:178-179`. |
| **Teste dinâmico:** `app/chamados/tests/test_e4_expand.py:207` | `self.executar(EMISSAO, 'responder_automaticamente', ...)`, exige `True`; repete em `:208`, exige `False`; compara foto, ato e uma gravação, `:209-211`. |
| Mesmo arquivo `:208` | Compara foto, ato e uma gravação, `:209-211`. O executor extrai/compila função pelo nome, `:119-121`, `app/chamados/tests/e4_apoio.py:8-16`. |

Não achei alias da função: todos os imports dela encontrados são locais sem `as`, nos arquivos acima; `models.py` **não** a reexporta na lista de compatibilidade (`app/chamados/models.py:4235-4237`). A busca de `.py` achou 9 chamadas sintáticas diretas e as duas chamadas dinâmicas de teste acima; qualquer chamada gerada externamente à cópia: `[nao verificado]`.

A base arquivada contém **definição**, não outro chamador: `app/chamados/tests/e4b_c6_bases/0a65a5d64ae282a4ec55e468baf227b93609dcba/chamados/services/disputa_emissao.py.txt:1638-1658`. Menções em catracas e comentários não são chamadas (`app/chamados/tests/test_porta_resposta_pergunta.py:72`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:45`, `app/chamados/tests/test_porta_chamado_e3.py:102`).

Cascata produtiva:

- `resolver_disputas_de_foto` seleciona disputas **abertas** com motivo foto, `app/api/foto_recebida.py:65-68`; perguntas pelo episódio `motivo='foto_ausente', batida_origem=batida`, sem filtro de validação, `:71`.
- Ignora também o total de `_validar_pelo_dono`, `:74`; relê disputa, `:76`; incrementa `fechadas` em `:77` **antes** de verificar `d.fechada_em`, `:78-79`. Esse retorno conta disputas tratadas, inclusive as que permanecem abertas (`:62`, `:77-88`).
- Se ainda aberta, consulta `fio_vivo`, pode aparar motivo, `:81-82`; consulta `pode_fechar`, pode fechar, `:83-84`; se houver chamado, anexa ocorrência, `:85-87`; retorna contador, `:88`.
- APIs: anexam foto antes do kernel (`app/api/views.py:850-851`, `app/api/views_core.py:834-835`); chamam kernel (`views.py:860-862`, `views_core.py:844-846`); retornam o contador como `disputas_fechadas` (`views.py:863`, `views_core.py:847`).
- Para pergunta já validada, `validar_pergunta` relê banco, `app/chamados/services/validacao.py:410-417`; como a automática acabou de preencher `respondida_em`, passa a guarda `:418-420`; encontra `validada_em` e retorna exatamente `{'ok': False, 'estado': 'ja_validada', 'msg': 'Pergunta ja validada.', 'erros': []}`, `:421-423`. A materialização dessa função fica após esse retorno, `:450-477`.

Cuidado com “efeito exato”: o save direto em `app/chamados/services/disputa_emissao.py:1676` dispara receivers:

- `_becos_post_save_pergunta` agenda `fechar_beco_da_disputa` em `on_commit`, `app/chamados/signals.py:229-240`; o leitor classifica pelo `respondida_em`/estado dos chamados e `pode_fechar`, `app/chamados/services/becos.py:25-33`; pode resolver perguntas e fechar disputa, `:36-59`; evento reconsulta após commit, `:66-80`.
- `_outbox_post_save_pergunta` atua sincronamente quando `update_fields` intersecta `CAMPOS_QUE_ACORDAM`, `app/chamados/signals.py:252-259`. Portanto distinguir “dois campos escritos diretamente” de eventuais efeitos condicionais da rede. Sem caso concreto, quais cascatas realmente ocorreram: `[nao verificado]`.

Recusa de texto novo é mais precisa que “recusa sempre vazio”: `recusa_respondida_sem_texto` chama o predicado com o par solicitado, `app/chamados/services/fio.py:1595-1605`; só lança se o par for `respondida_sem_texto`. O teste cobre instante preenchido e `('', '   ', None)`, `app/chamados/tests/test_e3_10_cura_no_escritor.py:58-69`.

## 15/11 14:04 · sessao 1698f1bfad8f · - · - · origem=Ronald (tmux)

A composição já usada pela casa é `competencia_do_dia_entregue(p.data_evento, p.disputa.colaborador.empresa, memo=...)`, que usa `janela_atual` e `competencia_entregue`; não usa mês civil nem status de FechamentoMensal (`app/chamados/services/vigia_fio.py:843-865`). `janela_atual` respeita o corte da empresa (`app/ponto/janelas.py:36-63`). Exportada significa TXT vigente não invalidado (`app/folha/export.py:580-594`).

O modelo tem `FotoBatida.criado_em`, relacionado pelo episódio `p.batida_origem`, mas não há carimbo específico do último recebimento da foto (`app/chamados/models.py:2518-2519`; `app/ponto/models.py:1036-1041`). O upload posterior usa `update_or_create`, preservando `criado_em` quando já existe registro: portanto só é possível contar **registro de foto criado após V**, não todos os recebimentos posteriores (`app/ponto/registro_batida.py:298-313`). `Batida.sincronizado_em` pertence à batida, não à foto (`app/ponto/models.py:72-79`).

Texto vazio inclui whitespace pelo juiz real `texto_vazio` (`app/chamados/juizes.py:1523-1525`). O envelope já chama shell no tenant `juliani` (`bin/sonda_leitura.sh:239-240`).

Sugestão de script, somente SELECT; os únicos identificadores emitidos são PKs de perguntas:

```python
# Bases: chamados/juizes.py:1523-1525;
# chamados/services/vigia_fio.py:843-865;
# ponto/janelas.py:60-63; ponto/models.py:1036-1041.
import json
from collections import defaultdict
from django.db import connection
from chamados.models import PerguntaDisputa
from chamados.juizes import texto_vazio
from chamados.services.vigia_fio import competencia_do_dia_entregue
from ponto.janelas import janela_atual

assert connection.schema_name == "juliani", "tenant inesperado"

por_tipo = defaultdict(list)
por_competencia = defaultdict(list)
por_tipo_competencia = defaultdict(list)
foto_por_grupo = defaultdict(list)
todos = []
memo = {}

qs = (
    PerguntaDisputa.objects.filter(validada_em__isnull=False)
    .select_related(
        "disputa__colaborador__empresa",
        "batida_origem__foto_batida",
    )
    .order_by("pk")
)

for p in qs.iterator(chunk_size=500):
    if not texto_vazio(p.resposta_colab):
        continue
    todos.append(p.pk)
    empresa = p.disputa.colaborador.empresa
    if p.data_evento is None:
        competencia = "SEM_DATA"
        estado = "INDETERMINADA"
    else:
        fim = janela_atual(p.data_evento, empresa)[1]
        competencia = "%04d-%02d" % (fim.year, fim.month)
        estado = (
            "exportada"
            if competencia_do_dia_entregue(
                p.data_evento, empresa, memo=memo
            )
            else "aberta"
        )

    tipo = p.tipo_resposta
    chave = (tipo, competencia, estado)
    por_tipo[tipo].append(p.pk)
    por_competencia[(competencia, estado)].append(p.pk)
    por_tipo_competencia[chave].append(p.pk)

    b = p.batida_origem
    fb = getattr(b, "foto_batida", None) if b is not None else None
    if b is None:
        foto_estado = "SEM_EPISODIO"
    elif fb is None:
        foto_estado = "SEM_REGISTRO_FOTOBATIDA"
    elif not fb.foto.name:
        foto_estado = "REGISTRO_SEM_ARQUIVO"
    elif fb.criado_em > p.validada_em:
        foto_estado = "REGISTRO_FOTO_CRIADO_APOS_VALIDACAO"
    else:
        foto_estado = "REGISTRO_FOTO_CRIADO_ATE_VALIDACAO"
    foto_por_grupo[chave + (foto_estado,)].append(p.pk)

def emitir(rotulo, chave, ids):
    print(json.dumps(
        {"grupo": rotulo, "chave": chave, "n": len(ids),
         "pergunta_ids": ids},
        ensure_ascii=True,
    ))

emitir("total", [], todos)
for chave, ids in sorted(por_tipo.items()):
    emitir("tipo_resposta", [chave], ids)
for chave, ids in sorted(por_competencia.items()):
    emitir("competencia", chave, ids)
for chave, ids in sorted(por_tipo_competencia.items()):
    emitir("tipo_competencia", chave, ids)
for chave, ids in sorted(foto_por_grupo.items()):
    emitir("foto_registro", chave, ids)
print("FIM")
```

`SEM_DATA/INDETERMINADA` evita inventar competência; o vigia da casa opta por aberta para a sua própria medida, explicitamente, mas a composição devolve `None` sem dia (`app/chamados/services/vigia_fio.py:823`, `:852-856`). Se desejar seguir literalmente aquele rótulo de medida, pode usar `SEM_DATA/aberta` e explicar a convenção.

## 15/11 14:04 · sessao fef56d00aea9 · - · - · origem=Ronald (tmux)

Porta/transação/rollback, conferidos:

- Linhas do aval não se deslocaram nesta área: `app/chamados/models.py:3186` (`carimbar`); auxiliares em `:3180`, `:3242`, identidade em `:3251`; `save` em `:3522`; `DecisaoPergunta.registrar` em `:4086`.
- Hoje `carimbar` não abre `atomic`; aplica C/autor/via em memória (`models.py:3212`), aplica R/V só se solicitado e houver texto (`:3213`), consulta C6 (`:3222`), fotografa lavra (`:3230`), eventualmente lavra (`:3233`), salva (`:3235`) e retorna `True` (`:3240`). C6 vivo restaura e retorna `False` (`:3226`); erro restaura e sobe (`:3223`, `:3236`).
- `save` abre seu próprio `atomic` em `models.py:3532`; DP abre outro em `:4095`, escreve via `bulk_create` (`:4096`) e acorda células (`:4097`). Para ser o mesmo atomic, o bloco comum precisa estar dentro da porta, abrangendo `self.save(...)` e `DecisaoPergunta.registrar(...)`, com o `except` atual fora desse bloco, e `True` só depois do sucesso dos dois — inferência de desenho ancorada nesses pontos.
- Ordem concreta preferível para preservar a leitura atual: C6 em memória → lavra opcional → `save` → `registrar` → retorno. O C6 e a lavra hoje antecedem `save` (`models.py:3222`, `:3233`, `:3235`); o ato novo não pode nascer no caminho `False` (`:3226`). Contrato do futuro leitor F1 dentro do próprio C6 não verificável nesta cópia; cuidado para não criar dependência circular no ato que ainda vai nascer [não verificado].
- Atomic comum também mantém callbacks depois do DP: o `post_save` da pergunta agenda fecho de becos em `on_commit` (`app/chamados/signals.py:240`) e emite EventoCelula síncrono (`:259`). Só `save.atomic` permite que o callback rode antes de um `registrar` seguinte em autocommit (`models.py:3532`, `signals.py:240`) — inferência do fluxo.
- Tanto `save` como `registrar` acordam células (`signals.py:259`, `models.py:4097`). `acordar_celulas` deduplica apenas dentro da própria chamada pelo conjunto local `vistos` (`models.py:3321`, `:3330`); deduplicação global do evento não foi inspecionada [não verificado].

O que a restauração cobre:

- `_desfazer_em_memoria` restaura exatamente `materializada_em`, `materializada_por_id`, `via_resolucao`, `respondida_em`, `validada_em`, `validada_por_id`, e apaga `_pv_memo` (`models.py:3244`).
- A lavra é restaurada separadamente por `_vl.desfazer_lavra_em_memoria` (`models.py:3237`), que restaura `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:284`).
- Se o DP falhar **depois de um save bem sucedido**, surge lacuna adicional: `save` já atualizou `_par_carregado` (`models.py:3534`); `_desfazer_em_memoria` não o restaura (`:3244`). A instância pode ficar com campos antigos e fotografia do par nova — inferência direta.
- Não restaura linhas DP nem EventoCelula: isso depende do rollback SQL comum; DP é criado por `bulk_create` e a porta não recebe/atualiza a própria instância da pergunta (`models.py:4094`).
- Também não invalida prefetch de decisões: `respostas_anteriores` explicitamente usa `self.decisoes.all()` e o cache existente (`models.py:3648`); o helper só apaga `_pv_memo` (`:3249`). Se o novo desenho reutilizar uma instância com decisões prefetched, isso exige consideração.
- Rollback posterior do atomic do chamador depois de `carimbar=True` também não chama esse helper: a porta já retornou (`models.py:3240`); preservação de memória nesse cenário não é coberta pelo contrato atual [não verificado quanto a testes]. [nao verificado]

Identidade:

- Aceita só `VIAS_DE_IDENTIDADE` (`models.py:3261`), fonte com `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado` (`:2588`). `competencia_trancada` continua legível, mas não é aceita pelo escritor (`:3178`, `:3261`).
- Grava via + instante + autor; `manter_carimbo=True` preserva instante/autor anteriores se já houver carimbo; pode gravar `recusa_motivo` no mesmo save (`models.py:3265`). Retorna `True` após save (`:3273`), recusa retorno ao fato por identidade inadequada com `False` (`:3263`).
- Não pergunta ao C6 porque a decisão de identidade é do chamador, e C6 conhece essas vias pela própria identidade (`models.py:3252`). Portanto não é conclusão de materialização da porta `carimbar`; misturar os atos eliminaria a distinção que o código mantém (`:3160`).
- Chamadores de identidade reais observados: admin (`models.py:3147`), retratação de motivo (`:2125`), paridade (`materializacao.py:930`), fato ocupado (`:1105`), duplicata (`fio.py:1398`). `remover_motivo` já registra RETRATACAO antes da identidade (`models.py:2120`), com motivo descritivo que não é literal `obsoleta` (`:2123`).

Testes existentes, apenas lidos: [nao verificado]

| Cobertura | Prova |
|---|---|
| C6 recusa, memória e banco sem carimbo | `app/chamados/tests/test_o261e_f2_porta_carimbo.py:713` |
| identidade/tranca rejeitadas por `carimbar` | `test_o261e_f2_porta_carimbo.py:733` |
| via de efeito rejeitada por identidade | `test_o261e_f2_porta_carimbo.py:740` |
| falha de save restaura memória | `app/chamados/tests/test_o261e_f2_fila.py:187` |
| falha de lavra restaura memória | `test_o261e_f2_fila.py:198` |
| falha de save depois da lavra restaura lavra | `test_o261e_f2_fila.py:208` |
| sucesso grava carimbo/autor/via | `test_o261e_f2_fila.py:224` |
| admin identidade 2× não move carimbo | `app/chamados/tests/test_porta_pergunta_identidade.py:194` |
| admin grava autor/via | `test_porta_pergunta_identidade.py:209` |
| falha do futuro `registrar` após save, rollback conjunto DP/pergunta/outbox, restauração de `_par_carregado` | sem teste encontrado [não verificado] | [nao verificado]

Interseções verificáveis nesta cópia:

- `app/chamados/juizes.py:285` ainda lê trilha obsoleta via log; `app/chamados/services/carga_pergunta_viva.py:110` também carrega `LogAuditoria`. F1 `atos_pergunta.leitura` e diff real da outra worktree não estão verificados [não verificado].
- `tarefa_do_dia` escolhe a última decisão (`app/chamados/services/tarefa_do_dia.py:147`) e atualmente cai em `ato_desconhecido:<ato>` para conclusão nova (`:176`); portanto recebe impacto funcional direto do DP novo, ainda que E4d não edite o arquivo.
- `tarefa_do_dia` carrega C6 com `carga_da_vida` e consulta o juiz (`tarefa_do_dia.py:518`, `:521`), logo a migração do leitor precisa conferir também essa carga.
- `vigia_fio` consome a porta da tarefa (`app/chamados/services/vigia_fio.py:561`, `:649`) e ainda consulta C6 em outros censos (`:776`, `:834`); recebe impacto indireto do novo ato/leitor.
- Arquivos declarados pelo usuário para obras em voo coincidem com esses pontos; hashes/diffs reais das worktrees E4c F1 e E3-RESTO-R3 não conferidos [não verificado].

## 15/11 14:03 · sessao 05c6694e5160 · - · - · origem=Ronald (tmux)

Leitores e efeitos confirmados no snapshot, todos os caminhos abaixo relativos à raiz:

- `responder_automaticamente` grava diretamente somente `resposta_colab=texto` e `respondida_em=instante`, incluindo conservação das projeções legadas sem linha e registro da nova decisão RESPOSTA no mesmo `atomic`; não modifica diretamente `validada_em`, `validada_por`, `materializada_em`, `materializada_por`, `via_resolucao`, `tipo_resposta` ou disputa. `app/chamados/services/disputa_emissao.py:1671-1679`.
- A conservação consulta correspondência `(pergunta_id, ato, ato_em)` para RESPOSTA e VALIDACAO; pode adicionar a resposta antiga, mesmo vazia, se havia `respondida_em`, e a validação antiga com `validada_por_id` e texto anterior vazio, se faltavam linhas correspondentes. `app/chamados/models.py:4067-4083`.
- O registro da nova resposta não fornece `ato_por`; campo nullable. `app/chamados/services/disputa_emissao.py:1677-1678`; `app/chamados/models.py:4017`.
- `estado`: antes e depois continua VALIDADA se `materializada_em is None`; continua RESOLVIDA se há materialização, pois validação vence antes da leitura da resposta. `app/chamados/models.py:3124-3126`.
- `.respondida`: muda de False para True se o carimbo anterior era nulo; se já havia carimbo, permanece True, mas troca a data. `app/chamados/models.py:3640-3641`; `app/chamados/services/disputa_emissao.py:1674`.
- C6 atual lê os campos de projeção: `projecao_temporal_da_pergunta` retorna os campos atuais, com recorte por `agora`; `_pergunta_viva_calc` retorna False imediatamente quando `validada` não é nula, antes de considerar resposta. Assim a nova resposta posterior à validação não desassina nem reabre a pergunta neste snapshot. `app/chamados/juizes.py:1388-1405`.
- A chave do memo lê `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, etc.; a troca do par invalida naturalmente o memo por mudança da chave. `app/chamados/juizes.py:854-862,893-905`.
- Face SQL `q_vivas` exige `validada_em__isnull=True`, portanto a pergunta validada continua excluída. `app/chamados/models.py:3468-3471`.
- App: `perguntas_do_app` primeiro filtra por `q_vivas`, depois C6; a pergunta validada fica fora do universo, tanto antes quanto depois. A partição pendente/histórico só opera nas vivas e lê `respondida_em is None` + disputa aberta. `app/chamados/services/pergunta_no_app.py:52-61,76-85`. Portanto não dizer que passa para “histórico do app” neste caso.
- `respondida_sem_texto`: antes pode ser True se havia carimbo antigo; depois será False para texto automático não vazio. `app/chamados/juizes.py:1523-1531`.
- `respostas_sem_veredito`: exige validação nula e materialização nula, além de carimbo OU texto; esta pergunta validada não entra antes nem depois. `app/chamados/models.py:1905-1909`.
- `pode_fechar` exige lista sem veredito vazia E ausência de `fio_vivo`; esta pergunta não bloqueia nenhum desses dois juízos por continuar validada. Outras perguntas podem bloquear, dados não fornecidos. `app/chamados/models.py:1915-1917`; `app/chamados/juizes.py:1404-1405,1507-1516`.
- Fila a validar usa `q_vivas` e `q_respondida_com_texto`; permanece fora pela validação. `app/chamados/juizes.py:1723-1727`; `app/chamados/models.py:3468`.
- Validador relê do banco e, com novo `respondida_em` preenchido, retorna `{'ok': False, 'estado': 'ja_validada', ...}` antes de carimbar/materializar. `app/chamados/services/validacao.py:410-423`.
- Se o materializador geral da disputa for chamado por outra via, seu filtro pega perguntas com `respondida_em` não nulo + `validada_em` não nulo + `materializada_em` nulo. A troca pode inserir a pergunta nesse universo se antes `respondida_em` era nulo; a própria chamada automática seguida da validação recusada não executa esse materializador pelo validador. `app/chamados/services/materializacao.py:1375-1385`; `app/chamados/services/validacao.py:421-423`.
- `.resposta_display` passa a ler o texto novo, com tradução de dropdown/opções dependendo do tipo; fallback é a resposta literal. Histórico de respostas lê decisões ordenadas e considera anterior a resposta superada por outra. `app/chamados/models.py:3643-3659,3662-3686`.

Cascatas que impedem afirmar “globalmente só dois campos mudam”:

- `PerguntaDisputa.save` executa `super().save()` em atomic, portanto receptores participam do save. `app/chamados/models.py:3522-3534`.
- Save do par acorda outbox sincronamente: os campos estão em `CAMPOS_QUE_ACORDAM`; receptor chama `acordar_celulas`. `app/chamados/models.py:3303-3305`; `app/chamados/signals.py:252-259`.
- `DecisaoPergunta.registrar` grava decisões e chama `acordar_celulas` novamente. A função emite um evento por célula encontrada no lote, podendo usar FK ou par colaborador/data; sem célula não emite. `app/chamados/models.py:4094-4099,3314-3332`.
- Outro receptor agenda `fechar_beco_da_disputa` em `on_commit`. `app/chamados/signals.py:229-240`.
- Beco lê `respondida_em` para decidir se ainda há pergunta visível; exige chamado pai terminal e `pode_fechar`. Pode passar a ser beco após a gravação caso o carimbo anterior fosse nulo e os demais requisitos fechem. `app/chamados/services/becos.py:25-33`.
- `fechar_beco_da_disputa` relê do banco e, se FECHÁVEL, chama `fechar_beco`, que resolve perguntas associadas a chamados terminais e fecha disputa quando permitido. Campos adicionais efetivamente alterados dependem do restante do estado, não fornecido. `app/chamados/services/becos.py:66-80,41-59`.
- O chamador explícito da foto ainda relê disputa e, se aberta, lê `fio_vivo`, apara motivo e fecha quando `pode_fechar`; pode anexar ocorrência no chamado. `app/api/foto_recebida.py:74-87`.

Não há `app/chamados/services/atos_pergunta.py`, `_leitura_conservada` ou `_q_validada_pelo_ato` no código desta cópia (pesquisa por nome e arquivos); só menções documentais em `app/docs/RELATO.md:14,50,90,111,127`. Não usar como implementação vigente.

## 15/11 14:02 · sessao 30b1ff93b5e0 · - · - · origem=Ronald (tmux)

B — evidências no snapshot:

- A função está em `app/chamados/services/disputa_emissao.py:1654-1679`; a faixa citada `1640-1666` começa ainda em `reabrir_para_correcao` e não cobre a escrita automática (`app/chamados/services/disputa_emissao.py:1640-1649`, `:1673-1679`). `app/api/foto_recebida.py:48` permanece correto.
- A premissa operacional confere: havendo texto prévio retorna `False`; caso contrário grava texto e resposta, sem guarda nem limpeza de `validada_em` (`app/chamados/services/disputa_emissao.py:1663-1679`).
- O literal universal “só recusa quando já há texto” não confere integralmente: também chama a recusa de resposta sem texto (`app/chamados/services/disputa_emissao.py:1667-1671`), que pode levantar `RespondidaSemTexto` (`app/chamados/services/fio.py:1595-1605`). No chamador citado, o texto constante não vazio elimina esse contracaso (`app/api/foto_recebida.py:25`, `:48`).
- O chamador ignora o booleano da resposta automática e chama o validador incondicionalmente (`app/api/foto_recebida.py:48-53`). O validador relê o banco e devolve `ok=False, estado='ja_validada'` para validação anterior; portanto não desfaz a resposta já gravada (`app/chamados/services/validacao.py:414-423`; `app/chamados/services/disputa_emissao.py:1675-1678`).
- O fluxo externo continua: atualiza a disputa, consulta `fio_vivo`, pode aparar motivo e fechar (`app/api/foto_recebida.py:74-87`).
- Os campos permitem validação com texto vazio: texto tem default vazio e validação é nullable (`app/chamados/models.py:2525-2527`). O total de validadas sem texto na frota é `[nao verificado]`: o anexo fornecido relata um caso, não esse censo `[nao verificado]`.
- B autoriza registro e censo, não construção: não identifico colisão com QUEIMA/COMPLEMENTO-2 no alcance literal fornecido; referência disponível apenas no anexo virtual `avais_todos.txt`, bloco `cortes_1358.txt`, item B `[nao verificado]`.
- O registro/censo de B não autoriza executar o escritor automático nem o fechamento descrito acima; efeitos desses caminhos não são efeitos necessários do trabalho B (`app/chamados/services/disputa_emissao.py:1675-1678`; `app/api/foto_recebida.py:74-87`; alcance do anexo `[nao verificado]`).
- Não há repositório Git acessível nesta cópia; o delta histórico exato contra `4b75cba` é `[nao verificado]`. O deslocamento/inadequação da faixa citada no arquivo atual é verificável como indicado acima (`app/chamados/services/disputa_emissao.py:1640-1679`).

Sugestão ao consolidador: Q1 deve registrar a ressalva literal e o deslocamento; não converter ausência de censo atual em “zero”, nem confundir a guarda extra com correção do risco sobre validação (`app/chamados/services/disputa_emissao.py:1663-1679`; `app/api/foto_recebida.py:25`, `:48`).

_5662 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
