# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 00:27** · publicado em **08/11/2026 00:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 00:27 · sessao 27af10e89625 · - · - · origem=Ronald (tmux)

**Item 1(c): mapa de produção**

| Função/campo | Leitores/chamadores |
|---|---|
| `chave_do_dia` | `_vizinhos_do_fato` `app/chamados/services/fio.py:1180`; `_base_da_chave` `:1197`; `_chave_do_passo` `:1203`; pré-carga de cache `:1308`; seleção de sementes `:1312`. |
| `_base_da_chave` | caminhada sem dia `fio.py:1224`; limite do fecho `:1277`, `:1281`; seleção de sementes `:1312`. |
| `_chave_do_passo` | `_vizinhos_em_cache` `fio.py:1208`; pré-carga em `universo_do_fato` `:1307`. |
| `_vizinhos_em_cache` | `_dias_alcancados_sem_dia` `fio.py:1223`; `fecho_do_fato` `:1278`. |
| `_dias_alcancados_sem_dia` | inferência do dia do fecho `fio.py:1273`; veto da ponte sem dia `:1283`; seleção prévia de sementes `:1313`. |
| `fecho_do_fato` | veto `episodios_unidos` de `plano_de_retratacao` `fio.py:1051`; `universo_do_fato` `:1317`; contador `:1344`; `universo_do_grupo` `:1357`; `universo_da_linha` `:1365`. |
| `recusadas` | coletor opcional escrito em `fio.py:1288-1290`; `universo_do_fato` agrega recusas `:1316-1320`; `_nascer` lê recusas para `FatoEmOutroDia` `app/chamados/models.py:2806-2815`; contador lê lista não vazia `fio.py:1343-1346`. |
| `dia` | porta passa o dia pedido `fio.py:1317`; fecho o une aos dias das sementes e exige no máximo um `:1269-1274`. |
| `_viz` | criado/reutilizado `fio.py:1261`, pré-carregado `:1307-1309`, passado à caminhada sem dia `:1313` e ao fecho `:1317`; consulta real de cache ausente `:1208-1211`. |
| `universo_do_fato` | `_nascer` `models.py:2807`; `_candidata_reusavel` `app/chamados/services/declaracao_texto.py:244`. |
| `universo_do_grupo` | partição de grupos `fio.py:1421`; apply backfill lê universo antes das travas de D/P `app/chamados/management/commands/backfill_decisoes_pergunta.py:213`, e relê antes de eleger canônica `:226`. |
| `universo_da_linha` | `_colisao_ao_datar` `fio.py:891`; `_migrar_sob_trava` `:1548`. |
| `grupos_de_colisao` | backfill DRY `backfill_decisoes_pergunta.py:360`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:58`. |
| `fecho_cruza_dias` | **Nenhum chamador de produção encontrado**; os chamadores reais são testes em `app/chamados/tests/test_e1_v13.py:114`, `:116`, `:195`, `:208`, `:216`. A doc promete o RESUMO DRY `fio.py:1329`, mas o resumo real não inclui a função/contador `backfill_decisoes_pergunta.py:414-422`. |
| Migração 0052 | Só adiciona FK nula `app/chamados/migrations/0052_pergunta_batida_origem.py:18-21`; não chama fecho. |
| Migração 0053 | Juízo SQL congelado próprio por família, sem chamar fecho `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36-46`; episódio agrupado **sem dia** `:42-43`; índice idem `:64-66`. |

**Outros leitores por caminho de um passo, sem `fecho_do_fato`:**

- `fato_livre_para` consulta diretamente `do_fato` e exclui a própria PK — deliberadamente preserva a identidade do índice do episódio, inclusive outro dia (`models.py:2721-2725`; teste declara a diferença `test_e1_v13.py:151-158`). [nao verificado]
- `declaracao_texto` tela/projeção consulta `do_fato` em `:446`, `:606`; lote usa `linhas_dos_fatos` em `:559-562`; escolhe canônica via `_segura_da_lista` `:264-269`.
- Gatilho de migração de data detecta colisão por `do_fato` **sem episódio** `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:152-154`, enquanto a porta usa episódio por `fato_livre_para` (`models.py:2725`).
- `disparar_perguntas_competencia` usa `linhas_dos_fatos` `app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `pergunta_no_app` usa o mesmo domínio `app/chamados/services/pergunta_no_app.py:113`; ambos para motivos de marco.

**Item 2: achados**

1. **NÃO PROCEDE a propriedade universal “todo membro vê o mesmo universo”.** Com `A1(alfa,d1)` e `B1(beta,d1)`, sem acervo legado, `universo_do_fato(cid,gps,d1,batida_origem_id=None)` consulta ambas e semeia ambas (`fio.py:1303`, `:1312-1317`); o fecho aceita sementes incondicionalmente (`:1262-1266`). Porém `universo_da_linha(A1)` só consulta episódio alfa + legado sem episódio (`models.py:2675-2679`) e vê `{A1}`; B1 vê `{B1}`. Portanto o universo multissemente `{A1,B1}` não é o componente visto por cada membro. A assertiva verifica apenas dias das sementes, sem identidade completa/conectividade (`fio.py:1269-1270`). Contraexemplo reproduzido extraindo as funções AST, com `do_fato` simulado fiel; execução Django/DB `[nao verificado]`.

2. **PROCEDE parcialmente o limite por chave:** vizinhos são filtrados pela base `(colaborador,motivo,instancia)` e pelo dia escolhido (`fio.py:1277-1285`); conversa é excluída da consulta de vizinhos (`:1177-1178`) e isolada em `universo_da_linha` (`:1363-1365`). Entretanto a identidade BATIDA do índice é `(batida_origem,motivo)`, sem dia (`models.py:2392-2395`; `0053:64-66`), e o novo fecho acrescenta corte por dia expressamente (`fio.py:1188-1189`). Não afirmar que todos os fatos de BATIDA no mesmo dia passam a ter universo único: contraexemplo acima.

3. **NÃO PROCEDE a igualdade documentada com `linhas_dos_fatos` em todo caso:** `fio.py:1251-1253` promete igualdade; uma linha datada e sua irmã sem dia entram juntas no fecho (`test_e1_v13.py:190-194`), mas `linhas_dos_fatos` compara `data_evento` exatamente ao dia pedido (`models.py:2708-2710`), excluindo a irmã nula.

4. **Cache: nenhum erro concreto encontrado no percurso normal com PK escalar.** A chave incorpora PK, chave do dia e episódio (`fio.py:1203`); dia alterado em memória gera outra chave; a pré-carga exige os mesmos parâmetros efetivos do `do_fato` (`:1307-1309` frente `:1180`). Omitir `instancia_marco` da condição de pré-carga não muda a consulta: a instância consultada deriva do motivo em `models.py:2707-2708`. Teste AST de 511 subconjuntos (NULL/d1/d2 × sem episódio/alfa/beta) não encontrou assimetria nos **fechos unitários**; execução DB `[nao verificado]`. [nao verificado]

5. **NÃO PROCEDE compatibilidade com colaborador objeto em `universo_do_fato`.** `do_fato` recebe argumento para filtro de FK (`models.py:2709`), mas `alvo` mantém o objeto cru (`fio.py:1302`), comparado com `chave_do_dia` que contém ID (`:1191-1192`, `:1312`). Resultado: linhas consultadas são recusadas e universo fica vazio. O cache não devolve vizinhos errados: a pré-carga também não casa (`:1308`). Os dois chamadores de produção passam `disputa.colaborador_id`, escalar (`models.py:2807`; `declaracao_texto.py:244`), logo impacto nesses chamadores `[nao verificado]`.

6. **NÃO PROCEDE “recusa declarada com contador” na integração de produção.** A função contador não é chamada pelo DRY (`backfill_decisoes_pergunta.py:414-422`); as outras portas leem fecho descartando `recusadas` (`fio.py:1051`, `:1357`, `:1365`, `:1421`). Só `_nascer` transforma determinada recusa em exceção (`models.py:2809-2815`).

**Pouso/pré-requisito e regressão** [nao verificado]

- **NÃO PROCEDE equivalência DRY/guarda 0053.** Mesmo episódio em dois dias distintos, ambos no horizonte, sem legado: `grupos_de_colisao` detecta duplicação por episódio (`fio.py:1393-1395`), divide pelos fechos (`:1421`) e descarta as duas partes unitárias (`:1427-1428`), devolvendo zero colisão. O guarda 0053 ainda conta um grupo duplicado de episódio sem dia e recusa a migração (`0053:42-46`). Os testes novos usam `D0` abaixo do horizonte (`test_e1_v13.py:24`) e não cobrem este impasse. [nao verificado]
- **NÃO PROCEDE coerência da suite existente:** `test_e1_v10.py:89-91` exige o fecho antigo atravessando DIA/DIA2; `test_e1_v11.py:86-91` exige a ponte pelo outro dia para vetar retratação. Essas expectativas contradizem o novo corte em `fio.py:1281`. Falha efetivamente executada `[nao verificado]`. [nao verificado]
- A seleção de sementes replica a classificação de linha sem dia/ponte antes da função de fecho (`fio.py:1312-1313` versus `:1281-1283`); isto é uma segunda aplicação da regra, embora ambas usem `_dias_alcancados_sem_dia`. Não há segundo algoritmo independente para eleger canônica nos caminhos auditados: todos continuam usando `canonica_do_grupo` (`models.py:2817`; `declaracao_texto.py:247`; `fio.py:893`, `:1552`; backfill `:237`; dedup `:76`).

## 08/11 00:27 · sessao 3770bb7fc195 · - · - · origem=Ronald (tmux)

Item 3 — **NAO PROCEDE como garantia por transação de todas as portas D/CH/P**; os quatro pontos novos têm guarda local, mas caminhos alcançados deixam escritas antes/depois da região protegida. (`app/chamados/services/validacao.py:585`, `app/chamados/services/disputa_emissao.py:1439`, `app/chamados/models.py:2227`)

| Porta nomeada | C antes da escrita local | Limite |
|---|---|---|
| `descarimbar_em_lote` | Exige os donos antes do UPDATE; não adquire trava, depende do chamador. `app/chamados/models.py:3104`, `:3109` | `ressuscitar_perguntas` adquire C no atomic antes de Decisão/Pergunta. `app/chamados/models.py:1547`, `:1552`, `:1553`, `:1555` |
| `reabrir_para_correcao` | Atomic próprio, reúne donos, trava, exige e só então insere Decisão e atualiza Pergunta. `app/chamados/services/disputa_emissao.py:1501`, `:1505`, `:1508`, `:1509`, `:1510`, `:1511`, `:1528` | Caller `recusar_pergunta` escreve P antes de entrar nessa porta; caller `reabrir_por_colab` escreve D depois de sair dela. `app/chamados/services/validacao.py:585`, `:588`; `app/chamados/models.py:2224`, `:2227` |
| `views:1283 → responder_core` | Atomic, trava e exige C antes de `p.save` e Decisão. `app/chamados/views.py:1283`; `app/chamados/services/disputa_emissao.py:1389`, `:1392`, `:1393`, `:1394`, `:1397` | Ramo `FOLGA_CONTESTA`/`OUTRO_MOTIVO` salva P fora desse atomic, sem readquirir C. `app/chamados/services/disputa_emissao.py:1428`, `:1439` |
| Segunda instância da batida | `cartorio` chama reconciliador; cada carimbo entra em atomic, trava e exige C antes de `carimbar`. `app/ponto/services/cartorio.py:958`; `app/chamados/reconciliador.py:591`, `:592`, `:593`, `:594` | Exceção é capturada por pergunta; fechamento D posterior usa `.fechar()`, que abre atomic e adquire C próprio. `app/chamados/reconciliador.py:600`, `:611`; `app/chamados/models.py:1897`, `:1902` |

**Contraexemplos concretos no escopo expandido:**

1. **Escrita antes da trava:** `detalhe_chamado_local → acao_recusar_pergunta → validacao.recusar_pergunta` salva `recusa_motivo` antes de chamar `reabrir_para_correcao`; esse save de Pergunta apenas abre atomic e grava, sem selo nem trava C. (`app/chamados/views.py:359`, `:518`; `app/chamados/services/acoes_disputa.py:80`; `app/chamados/services/validacao.py:533`, `:585`, `:588`; `app/chamados/models.py:3267`, `:3268`)

2. **Escrita em nova transação depois da trava anterior:** resposta `FOLGA_CONTESTA`/`OUTRO_MOTIVO` salva `recusa_motivo` depois que termina o atomic da resposta; a view HTML tem somente `@login_required`. (`app/chamados/services/disputa_emissao.py:1389`, `:1397`, `:1428`, `:1439`; `app/chamados/views.py:1227`, `:1228`, `:1283`; banco configurado sem `ATOMIC_REQUESTS`: `app/config/settings/prod.py:9`, `app/config/settings/saas.py:83`)

3. **Outra escrita posterior:** `reabrir_por_colab` chama helper que termina seu atomic e depois salva `Disputa.reaberturas_colab`; as bordas web/API não abrem atomic. (`app/chamados/models.py:2224`, `:2227`; `app/chamados/services/disputa_emissao.py:1501`, `:1530`; `app/chamados/views.py:1317`, `:1334`; `app/api/views.py:2700`, `:2718`)

4. **Reabertura D fechada depende de atomic externo:** `ressuscitar_perguntas` encerra atomic após descarimbar e depois chama `reabrir_sistema`; esta salva D sem adquirir/exigir C. O caminho fica protegido quando existe atomic externo, mas a função isolada não garante isso. (`app/chamados/models.py:1547`, `:1555`, `:1582`, `:1583`, `:2155`, `:2169`)

5. **Absorção alcançada pela resposta:** depois do atomic da resposta, o ramo ausência chama materializador; cada `q.carimbar` abre atomic próprio sem adquirir/exigir C; `carimbar` salva diretamente e `PerguntaDisputa.save` também não tem o selo. (`app/chamados/services/disputa_emissao.py:1424`, `:1427`; `app/chamados/services/materializacao.py:557`, `:558`; `app/chamados/models.py:2998`, `:3267`, `:3268`)

6. **Porta de conversa escreve sem C:** `_nascer` trava somente `not conversa`; `abrir_conversa` passa `conversa=True` e chega ao create. (`app/chamados/models.py:2752`, `:2766`, `:2767`, `:2864`)

**Dois colaboradores:** o lote recebido por uma chamada de `trava_na_ordem` é ordenado por pk na consulta; `reabrir_para_correcao` usa essa forma. Não existe ali recusa de aquisição incremental invertida na mesma transação, por exemplo `trava_na_ordem(C_maior)` seguida de `trava_na_ordem(C_menor)`: o método ordena somente cada chamada. O teste verifica uma chamada de lote, não essa propriedade global. (`app/chamados/models.py:2614`, `:2615`, `:2622`, `:2623`; `app/chamados/services/disputa_emissao.py:1509`; `app/chamados/tests/test_e1_v13_ordem.py:59`, `:71`, `:73`)

**Selo/mutantes:** as quatro guardas são runtime, por conjunto de ids, não por nome de caller; mutantes exercitam reabrir, responder e reconciliador. O caso descarimbar só chama a porta sem registro — o patch atuou na fixture, não durante a porta — e não verifica estado depois da exceção. Todos os casos usam `TestCase`, portanto não demonstram a separação entre transações das escritas posteriores listadas. (`app/chamados/models.py:2326`, `:2330`, `:2331`, `:2333`; `app/chamados/tests/test_e1_v13_ordem.py:33`, `:38`, `:75`, `:80`, `:86`, `:89`, `:91`, `:96`, `:130`, `:153`, `:156`)

Item 1(e) — **PROCEDE: há escape possível da exceção pelo caminho da batida; perda de batida não demonstrada.** (`app/chamados/models.py:2813`; `app/api/views.py:794`; `app/ponto/registro_batida.py:157`)

| Elemento | Local e efeito |
|---|---|
| Classe | `FatoEmOutroDia` herda `ResolvedoraCruzada`. `app/chamados/models.py:2337` |
| Quem levanta | Único raise, `_nascer`, quando universo pedido está vazio e há recusada com o episódio solicitado. `app/chamados/models.py:2807`, `:2809`, `:2813` |
| Porta | `abrir → _nascer`, sem captura. `app/chamados/models.py:2741` |
| Emissão BATIDA | `gerar_perguntas_disputa → abrir`; o try existente começa só depois, no reatar de pergunta já criada. `app/chamados/services/disputa_emissao.py:444`, `:462`, `:466` |
| Emissor superior | `criar_ou_atualizar_disputa → gerar_perguntas_disputa`, tanto nascimento quanto agregação, sem captura. `app/chamados/services/disputa_emissao.py:670`, `:682` |
| Bordas da batida | Os dois kernels chamam emissor sem try naquele trecho. `app/api/views.py:794`; `app/api/views_core.py:794` |
| Escrita da batida | O kernel já chamou `registrar_batida`; o chokepoint já executou `Batida.objects.create`. `app/api/views.py:684`; `app/api/views_core.py:681`; `app/ponto/registro_batida.py:157` |
| Testemunha | Teste sintético exige `FatoEmOutroDia` diretamente da porta e ausência de criação/reapontamento P. `app/chamados/tests/test_e1_v13.py:137`, `:142`, `:146` |

Não encontrei captura nominal de `FatoEmOutroDia` ou `ResolvedoraCruzada` em produção; os raises de `ResolvedoraCruzada` já existentes também ficam em `_nascer`. (`app/chamados/models.py:2786`, `:2828`; captura global externa: `[nao verificado]`)

Assim, o novo raise pode interromper o retorno de sucesso e os efeitos posteriores da API, embora a batida já tenha sido escrita; ocorrência real, HTTP final e qualquer rollback externo em produção são `[nao verificado]`. (`app/api/views.py:684`, `:794`, `:799`; `app/ponto/registro_batida.py:157`)

Classes de reprova pertinentes: **pouso antes do pré-requisito** em `validacao:585`; **selo parcial** nos quatro locais, sem proteção dos saves alcançados; **ordem por transação incompleta** nas escritas posteriores e na ordenação apenas por chamada. (`app/chamados/services/validacao.py:585`; `app/chamados/models.py:3267`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/models.py:2622`)

## 08/11 00:25 · sessao 140db4b0f5ed · - · - · origem=Ronald (tmux)

Resultado da leitura, sem executar testes nem escrever arquivos. A cópia não contém `.git`; a correspondência do snapshot com o hash informado é `[nao verificado]`. [nao verificado]

**1(a,b) — registro e fila**

| Escritor/leitor | Local | Efeito |
|---|---|---|
| Escritor do sentinela | `app/chamados/models.py:2318` | `_registrar_travados` chama **o método da conexão**, com `_TravaDoColaborador(cid)`; não usa `transaction.on_commit`. |
| Chamador único de `_registrar_travados` | `app/chamados/models.py:2623` | Depois do SELECT de C em `app/chamados/models.py:2622`. |
| Conteúdo do sentinela | `app/chamados/models.py:2290` | Armazena `cid`; executar o callback retorna `None`, sem escrita nem remoção própria (`app/chamados/models.py:2293`). |
| Leitor do registro | `app/chamados/models.py:2305` | Varre `conn.run_on_commit`, seleciona instâncias do sentinela; fora de atomic retorna vazio (`app/chamados/models.py:2303`). |
| Leitor para deduplicar registro | `app/chamados/models.py:2313` | Evita registrar C já presente. |
| Selo | `app/chamados/models.py:2331` | Subtrai os registrados dos C pedidos e levanta `ColaboradorNaoTravado` (`app/chamados/models.py:2333`). |
| Porta lote | `app/chamados/models.py:3104` | Exige donos pela FK da disputa. |
| Porta resposta | `app/chamados/services/disputa_emissao.py:1393` | Exige dono antes de salvar P (`:1394`). |
| Porta correção | `app/chamados/services/disputa_emissao.py:1510` | Exige conjunto de donos. |
| Fachada segunda instância | `app/chamados/reconciliador.py:593` | Exige dono antes de `carimbar` (`:594`). |
| Contador chokepoint | `app/ponto/tests/test_selo_chokepoint_escrita.py:215` | Lê a mesma fila, excluindo sentinelas. |
| Mutante | `app/chamados/tests/test_e1_v13_ordem.py:33` | Troca `_registrar_travados` por no-op; não escreve segundo estado. |

Não encontrei outro escritor de `_TravaDoColaborador`/registro na árvore; a busca negativa, sem uma linha que a represente, é `[nao verificado]`. Há outros escritores da **fila**, listados abaixo; eles registram trabalho, não o sentinela.

| Escritores da fila em produção | Callback |
|---|---|
| `app/chamados/signals.py:144` | `_retomar` |
| `app/chamados/signals.py:225` | `_fechar` |
| `app/chamados/signals.py:266`, `app/chamados/signals.py:293` | `_silenciar` |
| `app/chamados/signals.py:322` | `_reavaliar` |
| `app/chamados/signals.py:356` | `_reconciliar` |
| `app/ponto/signals.py:272` | `_depois` |
| `app/ponto/signals.py:307` | `_reavaliar` |
| `app/ponto/portas/celula.py:502` | `_reavaliar_o_dia` |
| `app/ponto/services/fechamento.py:1817` | `_rodar` |

O teste do contador registra adicionalmente um `lambda: None` e exige incremento de um (`app/ponto/tests/test_selo_chokepoint_escrita.py:287`, `:288`).

**Manipulações de fila/testes** [nao verificado]

- Os patches de `transaction.on_commit` não interceptam `conn.on_commit`: isso decorre da chamada em `app/chamados/models.py:2318`; o novo teste explicita e verifica essa pretensão (`app/chamados/tests/test_e1_v13_ordem.py:124`, `:126`).
- Outros patches “roda agora”: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:185`; `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:84`; `app/ponto/tests/test_o10b_editar_dia_troca.py:115`, `:121`, `:192`, `:367`, `:370`.
- A pretensão de queda no rollback de savepoint e sobrevivência no release está coberta pelo teste de atomic interno (`app/chamados/tests/test_e1_v13_ordem.py:107`, `:113`, `:114`, `:116`).
- Uso de savepoint manual em produção: a trava de C acontece **antes** da criação desse savepoint (`app/chamados/services/limbo_folgas.py:274`, `:277`); o rollback ocorre em `:285`. Portanto esse caso não testa registro criado **dentro** de savepoint manual.
- Outros rollbacks manuais explícitos: `app/chamados/tests/test_e1_v9.py:62`; `app/chamados/tests/test_matriz_guarda_clt.py:100`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:132`, `:294`.
- `run_and_clear_commit_hooks` não aparece na árvore pesquisada `[nao verificado]`. Sua implementação, assim como a implementação de `captureOnCommitCallbacks`, commit e rollback do Django, não está nesta cópia `[nao verificado]`; a versão exigida é Django 5.2.14 (`app/requirements.txt:13`). Não substituir essa lacuna pela docstring que afirma equivalência de vida com a trava (`app/chamados/models.py:2284`).
- Particularmente, **savepoint manual** merece ressalva: o novo teste só usa `atomic()`, não `transaction.savepoint()` (`app/chamados/tests/test_e1_v13_ordem.py:107`). A equivalência geral alegada para ambos fica `[nao verificado]`.

**Inventário de `captureOnCommitCallbacks`** — cada sítio abaixo captura callbacks; quando `execute=True`, solicita executá-los, incluindo o no-op se capturado. A retenção/remoção efetiva da fila pela implementação Django permanece `[nao verificado]`.

| Arquivo | Linhas |
|---|---|
| `app/api/tests/test_atestado_na_resposta.py` | 51 |
| `app/chamados/tests/test_jv1_volta4_posto_do_dia.py` | 46, 119, 135 |
| `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py` | 71, 118, 144 |
| `app/chamados/tests/test_varredura_becos_evento_pergunta.py` | 55, 65, 76, 86, 88 |
| `app/chamados/tests/test_e2_celula_regenerada_reavalia.py` | 70, 146, 202, 204; **execute=False** em 226 |
| `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py` | 33, 39, 66 |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py` | 87, 163 |
| `app/chamados/tests/test_laco_ausencia_aberta_nao_vai_e_volta.py` | 66 |
| `app/chamados/tests/test_s4_pendente_suspende_cobranca.py` | 86 |
| `app/chamados/tests/test_pep_reentrada.py` | 101 |
| `app/chamados/tests/test_c20_juiz3_outbox.py` | 242 |
| `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py` | 57, 68, 110, 134, 176 |
| `app/chamados/tests/test_reg_externa_disputa_beco.py` | 156 |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py` | 87, 131, 182, 188, 242, 260, 265; **execute=False** em 328 |
| `app/chamados/tests/test_varredura_isentos_evento.py` | 38, 52, 65, 71, 82 |
| `app/chamados/tests/test_varredura_feriado_evento.py` | 58, 83 |
| `app/ponto/tests/test_reconciliar_geofence.py` | 102 |
| `app/ponto/tests/test_evento_justificativa_rejulga_o_dia.py` | 31, 42 |
| `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py` | 115, 122, 136, 155, 174 |
| `app/ponto/tests/test_varredura_classe_b_ausencia.py` | 103, 114, 124 |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py` | 274 |

Há contagem **filtrada pelo nome** do callback de E2 (`app/chamados/tests/test_e2_celula_regenerada_reavalia.py:149`, `:228`, `:229`), preservada frente a sentinelas sem esse nome. Já `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:331` conta **todos** os callbacks e chama o primeiro (`:333`); ele não exclui sentinelas. Se uma trava nova ocorrer nesse ato capturado, pode alterar a premissa desse teste `[nao verificado]`.

**Achados úteis para item 3**

1. **NAO PROCEDE a afirmação de selo geral das portas D/CH/P.** As quatro verificações estão nas fachadas acima, não nas portas gerais. Os próprios testes criam Disputa e Pergunta com o mutante ativo, sem recusa (`app/chamados/tests/test_e1_v13_ordem.py:44`, `:47`, `:48`, `:53`). Isso prova que retirar o registro não impede genericamente escrita D/P.
2. **Registro é atestado dos IDs pedidos**, não dos IDs efetivamente retornados pelo banco: descarta o resultado do SELECT e registra `_cids` (`app/chamados/models.py:2622`, `:2623`). Um ID inexistente seria marcado como travado sem linha adquirida; aplicabilidade a dono de FK válido `[nao verificado]`.
3. O mutante da segunda instância verifica retorno zero e ausência do carimbo (`app/chamados/tests/test_e1_v13_ordem.py:156`, `:157`, `:159`); a exceção é engolida pela fachada (`app/chamados/reconciliador.py:600`).
4. O mutante do lote não verifica estado posterior e não mantém `_mutante` no ato; verifica somente falta inicial de registro e exceção (`app/chamados/tests/test_e1_v13_ordem.py:86`, `:88`). Não prova aquisição adequada pelo seu chamador.
5. O teste multicolaborador exige ORDER BY e trava anterior à primeira escrita (`app/chamados/tests/test_e1_v13_ordem.py:68`, `:71`, `:72`, `:73`).

**5 — tetos**

- **PROCEDE quanto à correspondência estrutural:** os dois números novos, 211 e 265, justificam +3 por um carimbo (`app/ponto/tests/test_selo_chokepoint_escrita.py:161`, `:162`). O código acrescenta atomic e SELECT de C antes de carimbar (`app/chamados/reconciliador.py:591`, `:592`, `:594`); o SELECT está em `app/chamados/models.py:2622`.
- **NAO VERIFICADO quanto aos valores medidos e a “um carimbo” nesses cenários:** os logs citados nos comentários não foram fornecidos como resultados observáveis. Os testes exigem que 1 e 3 chamados fechem antes de cobrar seus tetos (`app/ponto/tests/test_selo_chokepoint_escrita.py:389`, `:390`, `:394`, `:395`), mas não contam especificamente quantos carimbos ocorreram.
- **NAO PROCEDE que a filtragem esconda callback funcional atual:** só exclui objetos `_TravaDoColaborador` (`app/ponto/tests/test_selo_chokepoint_escrita.py:215`), cujo `__call__` retorna `None` (`app/chamados/models.py:2293`). Todos os demais callbacks continuam contados; o teste mantém mordida sobre lambda comum (`app/ponto/tests/test_selo_chokepoint_escrita.py:285`, `:287`, `:288`).
- A filtragem deixa de medir alocação, varredura e execução dos próprios sentinelas; esse custo existe no código, embora não seja trabalho de negócio (`app/chamados/models.py:2318`, `:2305`, `:2293`). Não esconderia suas queries porque as queries de aquisição permanecem no CaptureQueriesContext (`app/ponto/tests/test_selo_chokepoint_escrita.py:380`, `:381`).

## 07/11 23:40 · sessao 9e8813eff9c8 · FIO-E0-V4-E1 · censo · origem=Code

Censo estático da raiz `app/`. Uso **CH** = `ChamadoColaborador`, **D** = `DisputaSupervisao` e **P** = `PerguntaDisputa`. (`chamados/models.py:27`; `chamados/models.py:1691`; `chamados/models.py:2297`)

A classificação precisa distinguir caminhos: `trava_a_ordem_para_escrever()` retorna sem travar quando não existe `atomic`; `P.save()`, por sua vez, abre seu próprio `atomic`, sem adquirir C. (`chamados/models.py:484`; `chamados/models.py:486`; `chamados/models.py:3173`)

Execuções, quantidades efetivas de donos e locks observados em produção: **[nao verificado]**. Cascatas implícitas e transações internas do ORM não presentes na cópia: **[nao verificado]**.

**1. Sítios de escrita**

Cada linha corresponde a um sítio; quando há caminhos com classes diferentes, ambos aparecem na mesma célula.

| Arquivo:linha | Função | Entidade / operação | Classificação da transação |
|---|---|---|---|
| `chamados/models.py:296` | `_gravar_estado` | CH / save | **TRAVA-ANTES**: `chamados/models.py:290`, no atomic `chamados/models.py:280`. |
| `chamados/models.py:498` | `rebaixar` | CH / save | **INDETERMINADO**: chama a porta em `chamados/models.py:497`, condicionada ao atomic em `chamados/models.py:484`. Chamador de produção não localizado **[nao verificado]**. |
| `chamados/models.py:553` | `escalar_urgencia` | CH / save | **SEM-ATOMIC** pelo command `ponto/management/commands/processar_alertas_avancados.py:173`; **TRAVA-ANTES**, via `chamados/models.py:552` → `chamados/models.py:486`, quando chamado no atomic de `ponto/management/commands/escalonar_chamados_supervisao.py:131`. |
| `chamados/models.py:571` | `apagar_em_ensaio` | CH / delete | **SEM-TRAVA** no atomic de `ponto/management/commands/zerar_dados_operacionais.py:80`, chamada em `ponto/management/commands/zerar_dados_operacionais.py:84`. Tem guarda de ensaio antes do delete: `chamados/models.py:570` → `ponto/registro_batida.py:326`. |
| `chamados/models.py:591` | `definir_prazo_sla` | CH / save | **TRAVA-ANTES**, via `chamados/models.py:590` → `chamados/models.py:486`, no atomic de `chamados/services/cobranca.py:176`; **SEM-ATOMIC** pela recobrança `chamados/views.py:1425` → `chamados/services/disputa_emissao.py:1604`. |
| `chamados/models.py:610` | `marcar_escalonado_dp` | CH / save | **SEM-ATOMIC** pelo chamador `ponto/management/commands/escalonar_chamados_supervisao.py:103`; a porta de `chamados/models.py:609` retorna pela condição de `chamados/models.py:484`. |
| `chamados/models.py:636` | `_rebaixar_sem_post_save` | CH / update | **INDETERMINADO** como execução isolada: herda o save que disparou o receiver `chamados/signals.py:72`. Pelo `_gravar_estado`, **TRAVA-ANTES** em `chamados/models.py:290`; a identificação do disparo concreto falta **[nao verificado]**. |
| `chamados/models.py:971` | `abrir` | CH / get_or_create | **TRAVA-ANTES**: `chamados/models.py:905`, atomic `chamados/models.py:902`. |
| `chamados/models.py:1095` | `criar` | CH / create | **SEM-TRAVA** pela API `api/views.py:2201`, com atomic próprio em `chamados/models.py:1094`. **TRAVA-ANTES** pela abertura de formulário, que trava em `chamados/services/abertura.py:164` antes da chamada em `chamados/services/abertura.py:172`. |
| `chamados/models.py:1928` | `DisputaSupervisao.fechar` | D / save | **TRAVA-ANTES**: `chamados/models.py:1902`, atomic `chamados/models.py:1897`. |
| `chamados/models.py:1960` | `aparar_motivo_respondido` | D / save | **SEM-ATOMIC**: `api/views.py:887` ou `api/views_core.py:877` → `api/foto_recebida.py:72`; o aparo precede a validação em `api/foto_recebida.py:73`. |
| `chamados/models.py:2057` | `remover_motivo` | D / save | **TRAVA-ANTES**: `chamados/models.py:2018` e `chamados/models.py:2020`, no atomic `chamados/models.py:2017`. |
| `chamados/models.py:2097` | `anotar_contexto` | D / save | **SEM-ATOMIC** pelos chamadores `chamados/views.py:1343` e `ponto/management/commands/escalonar_chamados_supervisao.py:159`. |
| `chamados/models.py:2115` | `renovar_contexto` | D / save | **SEM-ATOMIC** pelo command `ponto/management/commands/alertar_disputas_pendentes.py:48`, no laço de `ponto/management/commands/alertar_disputas_pendentes.py:42`. |
| `chamados/models.py:2153` | `reabrir_admin` | D / save | **SEM-ATOMIC** pelo caminho `chamados/services/fio.py:583` → `chamados/services/acoes_disputa.py:256`. A conversa é aberta depois, em `chamados/services/acoes_disputa.py:260`. |
| `chamados/models.py:2169` | `reabrir_sistema` | D / save | **TRAVA-ANTES** pelos caminhos `chamados/services/relavra_via.py:230` → `chamados/services/relavra_via.py:261`, `chamados/services/materializacao.py:676` → `chamados/services/materializacao.py:683` e `chamados/management/commands/reabrir_orfas_enterradas.py:153` → `chamados/management/commands/reabrir_orfas_enterradas.py:165`. **SEM-ATOMIC** pela ressuscitação avulsa de `chamados/views_arquivar.py:86`: a reabertura em `chamados/models.py:1583` fica depois do atomic de `chamados/models.py:1547`. |
| `chamados/models.py:2227` | `reabrir_por_colab` | D / save | **SEM-ATOMIC** por `chamados/views.py:1334` e `api/views.py:2718`; o atomic da correção já terminou antes do retorno em `chamados/services/disputa_emissao.py:1520`. |
| `chamados/models.py:2253` | `limpar_resolucao_residual` | D / save | **INDETERMINADO**: método sem trava/atomic próprio; chamador de produção não localizado **[nao verificado]**. |
| `chamados/models.py:2769` | `_nascer` | P / save da existente | **TRAVA-ANTES**: `chamados/models.py:2691`, no atomic `chamados/models.py:2689`. |
| `chamados/models.py:2773` | `_nascer` | P / create | Fato: **TRAVA-ANTES** em `chamados/models.py:2691`. Conversa: **SEM-TRAVA**, porque a condição de `chamados/models.py:2690` pula C; entrada em `chamados/services/acoes_disputa.py:260`. |
| `chamados/models.py:2907` | `carimbar` | P / save | **SEM-TRAVA** pelo consumidor `chamados/reconciliador.py:1355` → `chamados/reconciliador.py:586`; o atomic é o de `chamados/models.py:3173`. **TRAVA-ANTES** pelo resolvedor que trava em `chamados/services/disputa_emissao.py:1683` e carimba em `chamados/services/disputa_emissao.py:1749`. |
| `chamados/models.py:2945` | `carimbar_identidade` | P / save | **TRAVA-ANTES** por `Disputa.fechar`, trava `chamados/models.py:1902`, chamada `chamados/models.py:1913`. **SEM-TRAVA** nos caminhos de recusa de materialização sem C ancestral: `chamados/services/materializacao.py:921` e `chamados/services/materializacao.py:1096`; atomic do save em `chamados/models.py:3173`. |
| `chamados/models.py:2967` | `descarimbar` | P / save | **TRAVA-ANTES** por `chamados/services/relavra_via.py:230` → `chamados/services/relavra_via.py:246` e `chamados/management/commands/reabrir_orfas_enterradas.py:153` → `chamados/management/commands/reabrir_orfas_enterradas.py:157`. **SEM-TRAVA** pela reabertura de competência em `chamados/services/competencia_trancada.py:253` → `chamados/services/competencia_trancada.py:254` → `chamados/models.py:3073`. |
| `chamados/models.py:3015` | `descarimbar_em_lote` | P / update | **TRAVA-ANTES** por `chamados/models.py:1552` → `chamados/models.py:1555`. **SEM-TRAVA** pela correção que abre atomic em `chamados/services/disputa_emissao.py:1496` e chama o lote em `chamados/services/disputa_emissao.py:1516`. O `.update` está em `chamados/models.py:3016`. |
| `chamados/models.py:3027` | `encerrar_por_fato_no_espelho` | P / save | **SEM-TRAVA**: atomic por pergunta em `chamados/management/commands/encerrar_perguntas_com_fato.py:67`, chamada em `chamados/management/commands/encerrar_perguntas_com_fato.py:68`. |
| `chamados/models.py:3174` | `PerguntaDisputa.save` | P / super.save | Herda a classificação do chamador: **SEM-TRAVA**, por exemplo, via `chamados/services/disputa_emissao.py:1389`; **TRAVA-ANTES** via `chamados/models.py:2691` → `chamados/models.py:2769`. Sempre tem atomic próprio em `chamados/models.py:3173`. |

| Arquivo:linha | Função | Entidade / operação | Classificação da transação |
|---|---|---|---|
| `chamados/services/vinculos.py:89` | `apontar_justificativa` | CH / save | **TRAVA-ANTES**: `chamados/services/vinculos.py:88`; atomic `chamados/services/vinculos.py:86`. |
| `chamados/services/vinculos.py:101` | `apontar_celula` | CH / save | **TRAVA-ANTES**: `chamados/services/vinculos.py:100`; atomic `chamados/services/vinculos.py:98`. |
| `chamados/services/vinculos.py:117` | `apontar_ticket_glpi` | CH / save | **TRAVA-ANTES**: `chamados/services/vinculos.py:116`; atomic `chamados/services/vinculos.py:114`. |
| `chamados/services/vinculos.py:135` | `assumir_responsavel` | CH / save | **TRAVA-ANTES**: `chamados/services/vinculos.py:134`; atomic `chamados/services/vinculos.py:131`. |
| `chamados/services/vinculos.py:147` | `liberar_responsavel` | CH / save | **TRAVA-ANTES**: `chamados/services/vinculos.py:146`; atomic `chamados/services/vinculos.py:143`. |
| `chamados/services/contexto.py:78` | `anotar_contexto` | CH / save | **TRAVA-ANTES**: `chamados/services/contexto.py:77`; atomic `chamados/services/contexto.py:75`. |
| `chamados/services/contexto.py:104` | `renovar_contexto` | CH / save | **TRAVA-ANTES**: `chamados/services/contexto.py:103`; atomic `chamados/services/contexto.py:101`. |
| `chamados/services/contexto.py:127` | `retirar_contexto` | CH / save | **TRAVA-ANTES**: `chamados/services/contexto.py:126`; atomic `chamados/services/contexto.py:124`. |
| `chamados/services/contexto.py:163` | `fundir_contexto` | CH / save | **TRAVA-ANTES**: `chamados/services/contexto.py:162`; atomic `chamados/services/contexto.py:160`. |
| `chamados/services/texto.py:36` | `reescrever_texto` | CH / save | **TRAVA-ANTES**: `chamados/services/texto.py:35`; atomic `chamados/services/texto.py:31`. |
| `chamados/services/arquivo.py:171` | `silenciar` | CH / save | **SEM-ATOMIC** pela view `chamados/views_arquivar.py:37`. **TRAVA-ANTES** em `chamados/services/arquivo.py:170` → `chamados/models.py:486` no lote sob atomic `chamados/views_arquivar.py:134`. |
| `chamados/services/arquivo.py:210` | `levantar` | CH / save | **SEM-ATOMIC** pela view `chamados/views_arquivar.py:77`. Sob atomic externo, **TRAVA-ANTES** pela porta `chamados/services/arquivo.py:209` → `chamados/models.py:486`. |
| `chamados/services/disputa_emissao.py:296` | `perguntar_entradas_pendentes` | D / save | **TRAVA-ANTES** por `chamados/services/disputa_emissao.py:968` → `chamados/services/disputa_emissao.py:1179`. **SEM-ATOMIC** por `api/views.py:794` → `chamados/services/disputa_emissao.py:685`: o atomic da abertura em `chamados/services/disputa_emissao.py:521` já terminou. |
| `chamados/services/disputa_emissao.py:565` | `_abrir_ou_agregar_disputa` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:530`; atomic `chamados/services/disputa_emissao.py:521`. |
| `chamados/services/disputa_emissao.py:587` | `_abrir_ou_agregar_disputa` | D / create | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:530`; atomic `chamados/services/disputa_emissao.py:521`. |
| `chamados/services/disputa_emissao.py:993` | `criar_disputa_a_partir_de_chamados_orfaos` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:968`; atomic `chamados/services/disputa_emissao.py:964`. |
| `chamados/services/disputa_emissao.py:1032` | `criar_disputa_a_partir_de_chamados_orfaos` | D / create | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:968`. |
| `chamados/services/disputa_emissao.py:1041` | `criar_disputa_a_partir_de_chamados_orfaos` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:968`. |
| `chamados/services/disputa_emissao.py:1070` | `criar_disputa_a_partir_de_chamados_orfaos` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:968`. |
| `chamados/services/disputa_emissao.py:1165` | `criar_disputa_a_partir_de_chamados_orfaos` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:968`. |
| `chamados/services/disputa_emissao.py:1259` | `criar_disputa_ancora_de_chamado` | D / create | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:1232`; atomic `chamados/services/disputa_emissao.py:1223`. |
| `chamados/services/disputa_emissao.py:1268` | `criar_disputa_ancora_de_chamado` | D / save | **TRAVA-ANTES**: `chamados/services/disputa_emissao.py:1232`. |
| `chamados/services/relavra_via.py:127` | `relavrar` | P / save | **SEM-TRAVA**: command abre atomic em `chamados/management/commands/relavrar_via_i6.py:101` e chama em `chamados/management/commands/relavrar_via_i6.py:102`. |
| `chamados/services/validacao.py:137` | `_carimbar_validacao` | P / save | **TRAVA-ANTES** pelos chamadores que travam em `chamados/services/validacao.py:285`, `chamados/services/validacao.py:317` ou `chamados/services/validacao.py:431`. |
| `chamados/services/validacao.py:202` | `devolver_validacao_recusada` | P / save | **TRAVA-ANTES**: `chamados/services/validacao.py:431`, chamada em `chamados/services/validacao.py:464`, dentro do atomic `chamados/services/validacao.py:425`. |
| `chamados/services/validacao.py:585` | `recusar_pergunta` | P / save | **SEM-TRAVA** pelo caminho `chamados/services/acoes_disputa.py:80`; atomic do save em `chamados/models.py:3173`. |
| `chamados/services/validacao.py:661` | `conferir_data_do_fato` | P / save | **SEM-TRAVA** pelo caminho `chamados/views.py:1312`; atomic do save em `chamados/models.py:3173`. |
| `chamados/services/materializacao.py:848` | `_materializar_resposta_hora` | P / save | **TRAVA-ANTES** pela validação em `chamados/services/validacao.py:431` → `chamados/services/validacao.py:463`. **SEM-TRAVA** pelos caminhos `chamados/services/acoes_disputa.py:139`, `chamados/services/fio.py:670` e `chamados/services/validacao.py:469`; atomic local em `chamados/services/materializacao.py:1217`. |
| `chamados/services/materializacao.py:1100` | `_carimbar_recusa` | P / save | **SEM-TRAVA** nos caminhos que chegam ao gate em `chamados/services/materializacao.py:1210` ou ao finally em `chamados/services/materializacao.py:1294`, sem C ancestral; atomic do save em `chamados/models.py:3173`. **TRAVA-ANTES** quando ainda sob `chamados/services/validacao.py:431`. |
| `chamados/services/disputa_emissao.py:1389` | `responder_pergunta_disputa_core` | P / save | **SEM-TRAVA** por `chamados/views.py:1283`, `api/views.py:2659` e `chamados/services/acoes_disputa.py:368`; atomic local `chamados/services/disputa_emissao.py:1388`. **TRAVA-ANTES** pela declaração sob `chamados/services/declaracao_texto.py:322`, que mantém a trava de `chamados/services/disputa_emissao.py:530`. |
| `chamados/services/disputa_emissao.py:1434` | `responder_pergunta_disputa_core` | P / save | Mesmas entradas: **SEM-TRAVA** pelos HTTP acima; **TRAVA-ANTES** pela declaração sob `chamados/services/declaracao_texto.py:322` ou `chamados/services/declaracao_texto.py:624`, com trava em `chamados/services/disputa_emissao.py:530`. Atomic próprio do save em `chamados/models.py:3173`. |
| `chamados/services/disputa_emissao.py:1518` | `reabrir_para_correcao` | P / update | **SEM-TRAVA** por `chamados/services/acoes_disputa.py:103`, `chamados/services/validacao.py:588` e `chamados/management/commands/curar_perguntas_em_limbo.py:69`; atomic `chamados/services/disputa_emissao.py:1496`. **TRAVA-ANTES** na devolução de ausência rejeitada: `chamados/services/materializacao.py:676` → `chamados/services/materializacao.py:679`. |
| `chamados/services/disputa_emissao.py:1541` | `responder_automaticamente` | P / save | **SEM-TRAVA** por `api/foto_recebida.py:48`, antes da validação em `api/foto_recebida.py:51`; atomic local `chamados/services/disputa_emissao.py:1540`. |
| `chamados/services/disputa_emissao.py:1562` | `carimbar_resposta_pela_trilha` | P / save | **SEM-TRAVA**: atomic `chamados/services/disputa_emissao.py:1561`; chamador `chamados/management/commands/curar_perguntas_em_limbo.py:67`. |
| `chamados/services/fio.py:870` | `reapontar_resolvedora` | P / save | **TRAVA-ANTES**: `chamados/services/fio.py:855`; ramo que data passa por `chamados/services/fio.py:849` e trava em `chamados/models.py:2646`. Atomic `chamados/services/fio.py:843`. |
| `chamados/services/fio.py:965` | `rechavear_data_evento` | P / save | **TRAVA-ANTES**: `chamados/services/fio.py:947`; atomic `chamados/services/fio.py:943`. |
| `chamados/veredito_lavrado.py:207` | `gravar_lavras_em_lote` | P / bulk_update | **INDETERMINADO quanto à fronteira transacional do ORM**: porta e command não adquirem C nem abrem atomic; chamadas em `chamados/management/commands/backfill_veredito_lavrado.py:143` e `chamados/management/commands/backfill_veredito_lavrado.py:146`. Atomic interno de `bulk_update`: **[nao verificado]**. |
| `chamados/veredito_lavrado.py:231` | `lavrar` | P / save | **TRAVA-ANTES** por `chamados/models.py:312`, sob trava `chamados/models.py:290`. **SEM-TRAVA** pelo command `chamados/management/commands/lavrar_perguntas_stale.py:87`; atomic do save em `chamados/models.py:3173`. |

Os signals abaixo chegam aos sítios da tabela por delegação; não são uma segunda ocorrência do mesmo `.save`:

| Gatilho | Encadeamento de escrita |
|---|---|
| `chamados/signals.py:19` — disputa fechada | Atomic `chamados/signals.py:41` → `chamados/signals.py:42` → CH; **TRAVA-ANTES** em `chamados/models.py:290`. |
| `chamados/signals.py:54` — urgência ao resolver | `chamados/signals.py:72` → update CH em `chamados/models.py:636`; herda a transação originadora. |
| `chamados/signals.py:91` — resolver perguntas | Atomic `chamados/signals.py:114` → `chamados/signals.py:115`; **TRAVA-ANTES** no resolvedor `chamados/services/disputa_emissao.py:1683`. |
| `chamados/signals.py:121` — retomar regularizações | Callback registrado em `chamados/signals.py:144`, executa a retomada em `chamados/signals.py:137`; a materialização pode criar batida e atingir os receivers de `ponto/signals.py:132` e `ponto/signals.py:207`. |
| `chamados/signals.py:148` — materializar pendentes | Delega em `chamados/signals.py:163`, na transação originadora; carimbos passam por `chamados/models.py:2907`. |
| `chamados/signals.py:188` — cartório do CH | Julga em `chamados/signals.py:203`, na transação originadora; portas alcançadas no corpo em `ponto/services/cartorio.py:1488`, `ponto/services/cartorio.py:1622` e `ponto/services/cartorio.py:1789`. |
| `chamados/signals.py:215` — becos da P | Callback `chamados/signals.py:225` → `chamados/signals.py:224`; trava no serviço em `chamados/services/becos.py:51`. |
| `chamados/signals.py:256` e `chamados/signals.py:270` — isentos | Callbacks em `chamados/signals.py:266` e `chamados/signals.py:293` → silêncio sob atomic `chamados/services/silencio_isentos.py:41` → CH em `chamados/services/arquivo.py:171`. |
| `chamados/signals.py:312` — feriado | Callback `chamados/signals.py:322` → reavaliação em `chamados/signals.py:321`; fechos passam pelo escritor CH `chamados/models.py:296`. |
| `chamados/signals.py:337` — filho morto | Callback `chamados/signals.py:356` → reconciliador em `chamados/signals.py:355`; inclui carimbo P em `chamados/reconciliador.py:586` e retração em `chamados/reconciliador.py:1427`. |
| `ponto/signals.py:132` e `ponto/signals.py:144` — batida/ausência | Julgamento síncrono em `ponto/signals.py:135` ou `ponto/signals.py:147` → `ponto/signals.py:93` → portas do cartório. |
| `ponto/signals.py:207` — reconciliador da batida | `ponto/signals.py:215` → carimbos/reconciliação em `chamados/reconciliador.py:1418`, `chamados/reconciliador.py:1419` e `chamados/reconciliador.py:1427`. |
| `ponto/signals.py:276` — consumidor da ausência | Callback registrado em `ponto/signals.py:272`; devolução/reconciliação em `ponto/signals.py:262`, `ponto/signals.py:269` e `ponto/signals.py:271`. |
| `ponto/signals.py:292` — geofence do posto | Callback `ponto/signals.py:307` → reavaliação em `ponto/signals.py:306`. |

O `pre_save` de CH apenas lê o estado anterior; o outbox de P escreve `EventoCelula`. (`chamados/signals.py:184`; `chamados/signals.py:244`)

**2. Laços cujo universo pode abranger vários colaboradores sob um mesmo atomic da aplicação**

| Atomic externo / l

[truncado em 20000 de 30185 caracteres]

## 07/11 23:35 · sessao 5f4f8b5530e6 · - · - · origem=Ronald (tmux)

Confirmei a cadeia literal **CH antes de D**:

| Passo | Evidência |
|---|---|
| Criação da batida | `app/ponto/registro_batida.py:152` ou `:157` |
| Receiver síncrono do cartório | `app/ponto/signals.py:132` → `:135` → `_julgar_dias`, que chama cartório em `:93` |
| Entrada no julgamento | `app/ponto/services/cartorio.py:1989` → corpo em `:948` |
| Espelhamento do chamado vivo | `app/ponto/services/cartorio.py:1789` ou `:1798` |
| Porta de contexto | `app/ponto/management/commands/detectar_ausencias.py:558` → `app/chamados/services/contexto.py:101` atomic, `:103` trava, `:104` save |
| Aquisição C + CH | `app/chamados/models.py:486`: `trava_na_ordem(self.colaborador_id, chamados=(self.pk,))`; C em `:2547`, CH em `:2567`; essa chamada não pede universo de D |
| Fábrica posterior ao corpo | `app/ponto/services/cartorio.py:997` → `app/chamados/services/fabrica_da_celula.py:186` → `:149` |
| Aquisição posterior de D abertas | `app/chamados/services/disputa_emissao.py:530` pede `abertas_do_colaborador=True`; D em `app/chamados/models.py:2554` |

Sem atomic externo herdado, a transação de `contexto.py:101` termina antes de o fluxo chegar à fábrica em `cartorio.py:997`; com atomic externo, os locks C/CH continuam na transação que depois pede D. A identificação desse percurso com o nome **“2a instância”** é [nao verificado].

Correção do sítio `app/chamados/veredito_lavrado.py:231`: pelo command `lavrar_perguntas_stale.py:87`, é **SEM-TRAVA**, pois o próprio `PerguntaDisputa.save` abre atomic em `app/chamados/models.py:3173`. Pela chamada `app/chamados/models.py:312`, é **TRAVA-ANTES**, com trava em `:290`. Assim, o sítio tem caminhos mistos; não recebe um único rótulo universal sem registrar essa distinção.

## 07/11 23:34 · sessao 5f4f8b5530e6 · - · - · origem=Ronald (tmux)

Área lida: fora de `chamados/models.py`, `chamados/signals.py`, `chamados/services/` e comandos de chamados, o censo por AST aponta dois escritores diretos atribuídos às três entidades; a leitura confirmou ambos em `app/chamados/veredito_lavrado.py:207` e `:231`. A exaustividade além dos sítios atribuídos pelo varredor é [nao verificado].

| Sítio | Função | Entidade/operação | Classificação por caminho lido |
|---|---|---|---|
| `app/chamados/veredito_lavrado.py:207` | `gravar_lavras_em_lote` | P `bulk_update` | **SEM-ATOMIC** no caminho do backfill: chamadas `app/chamados/management/commands/backfill_veredito_lavrado.py:143` e `:146`; a função não abre atomic nem trava (`app/chamados/veredito_lavrado.py:197`). |
| `app/chamados/veredito_lavrado.py:231` | `lavrar` | P `save` | **INDETERMINADO como rótulo único: caminhos mistos.** Pela chamada `app/chamados/models.py:312`, **TRAVA-ANTES** em `app/chamados/models.py:290`, dentro do atomic `:280`. Pela chamada `app/chamados/management/commands/lavrar_perguntas_stale.py:87`, **SEM-ATOMIC** (command lido; não há atomic/lock no caminho). |

As chamadas `models.py:2767`, `models.py:2905`, `services/fio.py:869`, `services/fio.py:964` e `services/relavra_via.py:126` passam `salvar=False`; não executam o save de `veredito_lavrado.py:231` nesses caminhos.

Laços multicolaborador confirmados fora da sua área:

| Atomic externo / laço | Escrita alcançada |
|---|---|
| `app/chamados/views_arquivar.py:134` / `:135` | `arquivo.silenciar` em `:140`, sobre queryset sem restrição obrigatória a um colaborador (`:115`, filtro opcional `:118`). |
| `app/escala/views.py:407` / `:408` (`aplicar_folga_calendario`) | `trocar_calendario_de_folga` em `:409`; relavra em `:416` → `app/escala/folgas.py:272` → regeneração → marcador de CH em `app/ponto/portas/celula.py:289` e julgamento em `:434` (pode alcançar portas D/CH/P). |
| `app/ponto/portas/celula.py:652` / `:673` (`trocar_plantao`) | Dois colaboradores obrigatoriamente distintos (`:662`), ordenados por colaborador/data (`:674`), chama `editar_dia` em `:675` → marcador de CH em `:626`; julgamento indireto pelo `_o_chao_do_dia_mudou` em `:627`, que chama cartório em `:434`. |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:22` / `:43`, interno `:70` | `garantir_ausencia` em `:76` → `app/ponto/services/ausencia.py:417-418` salva Ausência; receiver síncrono `app/ponto/signals.py:147` → `_julgar_dias`/cartório `:93` (portas D/CH/P indiretas). Callback `ponto/signals.py:272` roda depois do commit e não integra essa transação. |
| `app/ponto/management/commands/gerar_celulas.py:309` / `app/ponto/portas/celula.py:837`, interno `:839` | O corpo escreve **CelulaDia**, com `create :849`, saves `:861`, `:871`. Não escreve D/CH/P diretamente nesse corpo (`:811-873`). Produzir D/CH/P por algum gancho não lido é [nao verificado]. |

`app/ponto/portas/regen_exportada.py:171` não é multicolaborador: o plano foi construído para um único `colaborador` em `:140-143`.

`app/escala/services/cadastro_tipo.py:334` é laço de frota, mas **não encontrei atomic envolvendo esse laço no caminho local**: `salvar_tipo` começa em `:140`, chama propagação em `:320`; cada regeneração abre seu atomic próprio em `app/ponto/portas/celula.py:218`. O decorador `cadastro_tipo.py:41` pertence ao helper `_hpd_pares :43`, não a `salvar_tipo`. Chamador externo que imponha atomic à frota é [nao verificado].

Batida, passos literais lidos:

1. `app/ponto/registro_batida.py:152` e `:157`: `Batida.objects.create`.
2. Receiver de cartório: `app/ponto/signals.py:132`, chama `_julgar_dias :135`; este chama `cartorio.julgar_celula :93`.
3. `app/ponto/services/cartorio.py:1989` chama `julgar_colab`; corpo inicial em `:948`.
4. Chamados podem passar por `reconciliar()` em `cartorio.py:1488`, `:1501`, `:1600`, ou pela passada adiada `:963`.
5. `app/chamados/models.py:650-654`: atomic de `reconciliar` → decisão → `_gravar_estado`; `_recarregar_estado_se_mudou` trava em `:711`, `_gravar_estado` trava em `:290`.
6. Esses dois locks pedem **Disputas abertas antes de CH**. A implementação seleciona D abertas em `models.py:2552-2555`, depois CH em `:2567`.
7. A fábrica ocorre depois do corpo em `cartorio.py:997` → `app/chamados/services/fabrica_da_celula.py:186` → `:149` criar/atualizar disputa; porta adquire universo em `app/chamados/services/disputa_emissao.py:530`.
8. Segundo receiver da batida: `app/ponto/signals.py:207-215` → `app/chamados/reconciliador.py:1418`, `:1419`, `:1427`. Retração: `reconciliador.py:522-523` → mesmo `reconciliar` acima.
9. Onde **CH já pode estar seguro antes de adquirir D fechada**: `models.py:1432` `_gravar_estado` → `:1441` `ressuscitar_perguntas` → trava explícita das D selecionadas em `:1552` → `:1582-1583` `reabrir_sistema`. C já foi adquirido em `:290`; a primeira aquisição do CH está em `:2567`, e o primeiro universo exclui D fechadas por `:2552`.
10. Outro lock que pede somente C+CH: `app/chamados/services/fio.py:768` (`_costura_nao_mata_o_fio`); sua chegada pela batida e posterior D na mesma transação não foram fechadas nesta leitura [nao verificado].

Não identifiquei com segurança qual desses percursos corresponde ao rótulo externo **“2a instância da batida”** [nao verificado].

## 07/11 23:34 · sessao 2783e9e9098d · - · - · origem=Ronald (tmux)

Nenhum novo laço multicolaborador escritor de D/CH/P confirmado nos candidatos desta rodada.

| Candidato | Resultado com evidência |
|---|---|
| `aplicar_09_corte_b.py:197–213` | Há atomic externo e loop de empresas. O serviço chamado percorre colaboradores (`fechamento.py:619`) e grava FechamentoMensal (`898,1137,1153`) e DiaPago (`1193`); não confirmei caminho D/CH/P. Demais efeitos transitivos dos cálculos: [nao verificado]. |
| `reconciliar_grade.py:472` | Atomic por um `ch`, para renovar/retirar contexto (`473–474`). O loop de colaboradores está fora (`115`), assim como a chamada (`228`). Não é transação multicolaborador. |
| `curar_residuo_bug115.py:120` | Atomic de `_ambiguo` por um `ch` (`121–127`). Loops externos (`95–106`) não estão dentro de atomic. |
| `gerar_batidas_smoke_motor.py:106–107` | Chama `semear`, que faz Batida.bulk_create e trilha (`registro_batida.py:331–338`), sem signals de save. Guarda de ensaio em `333→326–328`. Não confirmei escrita D/CH/P. |
| `lavrar_placar_situacional.py:101–111` | Loop escreve somente PlacarSituacional (`105–106`) e apaga placares (`110–111`); busca nos arquivos `*signals.py` não encontrou receiver desse modelo. |
| `escala/folgas.py:162,309,354,382,490` | Cada ato recebe um único vínculo/colaborador. O loop de `384` percorre dias do mesmo `ec`; a relavra também recebe esse `ec` (`395–398`). `reparentar_folgas_do_colaborador` filtra um colaborador (`512–515`). |
| `escala/folgas.py:555,585` | Atos escrevem calendário e seus tamanhos (`560–572`, `587–593`); nenhum receiver desses modelos encontrado nos arquivos `*signals.py`. Não confirmei escrita D/CH/P. |

A relavra de folgas pode chegar às portas de chamado, mas mantém um único vínculo no ato: `folgas.py:272–276` chama `regenerar_celulas_vinculo(ec, ...)`. Um chamador externo que envolva vários vínculos em atomic precisa ser contado no próprio chamador; esses chamadores adicionais não foram auditados nesta rodada. [nao verificado]

## 07/11 23:32 · sessao 2e5bb51989dd · - · - · origem=Ronald (tmux)

Censo de `app/chamados/services`: **36 sítios diretos** de escrita nas três entidades, conforme os endereços da tabela. P = `PerguntaDisputa`, D = `DisputaSupervisao`, CH = `ChamadoColaborador`.

| Arquivo:linha | Função | Entidade/operação | Classificação e fundamento |
|---|---|---|---|
| `services/vinculos.py:89` | `apontar_justificativa` | CH/save | TRAVA-ANTES `:88`, atomic `:86` |
| `services/vinculos.py:101` | `apontar_celula` | CH/save | TRAVA-ANTES `:100`, atomic `:98` |
| `services/vinculos.py:117` | `apontar_ticket_glpi` | CH/save | TRAVA-ANTES `:116`, atomic `:114` |
| `services/vinculos.py:135` | `assumir_responsavel` | CH/save | TRAVA-ANTES `:134`, atomic `:131` |
| `services/vinculos.py:147` | `liberar_responsavel` | CH/save | TRAVA-ANTES `:146`, atomic `:143` |
| `services/contexto.py:78` | `anotar_contexto` | CH/save | TRAVA-ANTES `:77`, atomic `:75` |
| `services/contexto.py:104` | `renovar_contexto` | CH/save | TRAVA-ANTES `:103`, atomic `:101` |
| `services/contexto.py:127` | `retirar_contexto` | CH/save | TRAVA-ANTES `:126`, atomic `:124` |
| `services/contexto.py:163` | `fundir_contexto` | CH/save | TRAVA-ANTES `:162`, atomic `:160` |
| `services/texto.py:36` | `reescrever_texto` | CH/save | TRAVA-ANTES `:35`, atomic `:31` |
| `services/arquivo.py:171` | `silenciar` | CH/save | TRAVA-ANTES `:170` **se** atomic externo; helper não trava fora de atomic (`models.py:484`). Nos chamadores sem atomic, SEM-ATOMIC, sujeito à leitura do override CH feita pelo agente models. |
| `services/arquivo.py:210` | `levantar` | CH/save | TRAVA-ANTES `:209` **se** atomic externo; mesma guarda `models.py:484` |
| `services/disputa_emissao.py:296` | `perguntar_entradas_pendentes` | D/save | TRAVA-ANTES `:968` pelo chamador `:1179`; pelo chamador `:685`, atomic de `_abrir_ou_agregar` já terminou (`:521`, retorno `:604`), portanto classificação depende do atomic externo de `criar_ou_atualizar_disputa` |
| `services/disputa_emissao.py:565` | `_abrir_ou_agregar_disputa` | D/save | TRAVA-ANTES `:530`, atomic `:521` |
| `services/disputa_emissao.py:587` | `_abrir_ou_agregar_disputa` | D/create | TRAVA-ANTES `:530`, atomic `:521` |
| `services/disputa_emissao.py:993` | `criar_disputa_a_partir_de_chamados_orfaos` | D/save | TRAVA-ANTES `:968`, atomic `:964` |
| `services/disputa_emissao.py:1032` | mesma | D/create | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1041` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1070` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1165` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1259` | `criar_disputa_ancora_de_chamado` | D/create | TRAVA-ANTES `:1232`, atomic `:1223` |
| `services/disputa_emissao.py:1268` | mesma | D/save | TRAVA-ANTES `:1232` |
| `services/relavra_via.py:127` | `relavrar` | P/save | SEM-TRAVA; command `relavrar_via_i6.py:101-102` abre atomic sem C; fallback de P.save também tem atomic `models.py:3173` |
| `services/validacao.py:137` | `_carimbar_validacao` | P/save | TRAVA-ANTES pelos chamadores `validacao.py:285`, `:317`, `:431`; chamadas `:288`, `:320`, `:435` |
| `services/validacao.py:202` | `devolver_validacao_recusada` | P/save | TRAVA-ANTES `validacao.py:431` pelo chamador único da app `:464`, ainda no atomic `:425` |
| `services/validacao.py:585` | `recusar_pergunta` | P/save | SEM-TRAVA no caminho `acoes_disputa.py:80`; P.save abre atomic `models.py:3173` |
| `services/validacao.py:661` | `conferir_data_do_fato` | P/save | SEM-TRAVA no caminho `views.py:1312`; P.save abre atomic `models.py:3173` |
| `services/materializacao.py:848` | `_materializar_resposta_hora` | P/save | TRAVA-ANTES quando via `validacao.py:431/463`; SEM-TRAVA via `acoes_disputa.py:139`, `fio.py:670`, ou `validacao.py:469` (fora do atomic `:425`). Atomic local `materializacao.py:1217`, chamada `:1219`. |
| `services/materializacao.py:1100` | `_carimbar_recusa` | P/save | TRAVA-ANTES via `validacao.py:431/463`; SEM-TRAVA no gate anterior ao atomic (`materializacao.py:1210`) e no finally depois dele (`:1294`) quando os chamadores HTTP não têm atomic; P.save garante atomic `models.py:3173`. Também alcançado sob atomic do passivo (`passivo_validado.py:371`, `:511`, `:545`) sem trava C prévia garantida. |
| `services/disputa_emissao.py:1389` | `responder_pergunta_disputa_core` | P/save | SEM-TRAVA via `views.py:1283`, `api/views.py:2659`, `acoes_disputa.py:368`; atomic local `:1388`. TRAVA-ANTES via `declaracao_texto.py:322/330/389` ou `:624/627/661`, que mantêm vivo o lock de `_abrir_ou_agregar_disputa:530`. |
| `services/disputa_emissao.py:1434` | mesma | P/save | Mesmos caminhos acima: SEM-TRAVA nos HTTP diretos, TRAVA-ANTES nos de declaração; atomic próprio P.save `models.py:3173`. |
| `services/disputa_emissao.py:1518` | `reabrir_para_correcao` | P/QuerySet.update | SEM-TRAVA via `acoes_disputa.py:103`, `validacao.py:588`, `curar_perguntas_em_limbo.py:69`; atomic próprio `:1496`. TRAVA-ANTES via `materializacao.py:672/676/679` (devolução da ausência rejeitada). |
| `services/disputa_emissao.py:1541` | `responder_automaticamente` | P/save | SEM-TRAVA local; atomic `:1540`; chamada `api/foto_recebida.py:48`, antes de validar (`:51`). Atomic/lock ancestral dessa entrada precisa ser consolidado pelo agente responsável por API. |
| `services/disputa_emissao.py:1562` | `carimbar_resposta_pela_trilha` | P/save | SEM-TRAVA; atomic `:1561`, command chamador `curar_perguntas_em_limbo.py:67` sem lock |
| `services/fio.py:870` | `reapontar_resolvedora` | P/save | TRAVA-ANTES `:855` no ramo sem datar; ramo datando usa `fato_livre_para` em `:849`, que trava C em `models.py:2646`; atomic `fio.py:843` |
| `services/fio.py:965` | `rechavear_data_evento` | P/save | TRAVA-ANTES `:947`, atomic `:943` |

Todos os caminhos P sem trava continuam sendo **SEM-TRAVA**, e não SEM-ATOMIC: o override abre atomic em `app/chamados/models.py:3173` e escreve em `:3174`.

Laços multicolaborador confirmados nesta área:

- `app/chamados/services/silencio_isentos.py:41-43`, atomic envolvendo queryset inteiro; `alvos(None)` pega vários colaboradores (`:24-28`).
- `app/chamados/services/relavra_via.py:117-127`, sob atomic único em `app/chamados/management/commands/relavrar_via_i6.py:101-102`; seleção sem restrição por colaborador em `:55`.

O exemplo `cobranca.py:176-178` é laço de vários pais, com perguntas recortadas pelo colaborador de entrada (`cobranca.py:140`, `:163`; `aviso_pendencia.py:10`). O pai é obtido por `p.disputa.chamado` (`cobranca.py:60-66`); o código não confirma aqui que o dono CH seja o mesmo da disputa. Mais de um dono efetivo nesse laço: [nao verificado].

Outros laços com atomic por unidade, não abrangendo todos os colaboradores: `relavra_via.py:215/219`, `passivo_validado.py:353/371` (por colaborador/competência), `passivo_validado.py:403/404` (por disputa), `limbo_folgas.py:316` → `agrupar_furo` com atomic em `:270`.

## 07/11 23:32 · sessao 2783e9e9098d · - · - · origem=Ronald (tmux)

Inventário completo dos sítios explícitos em `app/chamados/models.py`; não encontrei escrita SQL direta em `app/chamados/signals.py`, mas os receivers delegam aos escritores indicados abaixo (`signals.py:42,72,115,163,203,224,265,292,321,355`).

Abreviações: CH = ChamadoColaborador; D = DisputaSupervisao; P = PerguntaDisputa. Classificações mistas exigem desdobramento por caminho; não inventei uma garantia universal.

| Sítio (`app/chamados/models.py`) | Função | Entidade/operação | Classificação/evidência |
|---|---|---|---|
| 296 | `_gravar_estado` | CH.save | TRAVA-ANTES, `290`, dentro do atomic `280` |
| 498 | `rebaixar` | CH.save | INDETERMINADO: chama porta `497`, que só trava com atomic preexistente (`484–486`); busca por chamadas de produção não encontrou chamador |
| 553 | `escalar_urgencia` | CH.save | Misto: TRAVA-ANTES `552→486` quando atomic externo, como command `escalonar_chamados_supervisao.py:131–138`; SEM-ATOMIC no command `processar_alertas_avancados.py:173` |
| 571 | `apagar_em_ensaio` | CH.delete | Não alcança banco de produção segundo guarda chamada `569–570`; caller `zerar_dados_operacionais.py:80–84`, atomic externo sem trava C. Guarda concreta `registro_batida.py:316` precisa leitura complementar |
| 591 | `definir_prazo_sla` | CH.save | Misto: TRAVA-ANTES `590→486`, cobrança com atomic `cobranca.py:176–178`; `disputa_emissao.py:1604` não abre atomic nessa função, chamadores externos ainda necessários |
| 610 | `marcar_escalonado_dp` | CH.save | SEM-ATOMIC no único caller encontrado, `escalonar_chamados_supervisao.py:103`; porta `609→484` retorna sem travar em autocommit |
| 636 | `_rebaixar_sem_post_save` | CH.QuerySet.update | INDETERMINADO por caminho originador do save; receiver `signals.py:54–72` não abre atomic nem trava; herda a transação do save |
| 971 | `abrir` | CH.get_or_create | TRAVA-ANTES `905`, atomic `902` |
| 1095 | `criar` | CH.create | SEM-TRAVA no caminho API `api/views.py:2201`; atomic próprio `1094`, nenhum C antes do create na função |
| 1928 | `DisputaSupervisao.fechar` | D.save | TRAVA-ANTES `1902`, atomic `1897` |
| 1960 | `aparar_motivo_respondido` | D.save | SEM-ATOMIC no caminho `api/views.py:887→api/foto_recebida.py:72` e gêmeo `api/views_core.py:877`; aparo precede resposta/validação `foto_recebida.py:73` |
| 2057 | `remover_motivo` | D.save | TRAVA-ANTES `2018/2020`, atomic `2017` |
| 2097 | `anotar_contexto` | D.save | SEM-ATOMIC nos callers `chamados/views.py:1343` e `escalonar_chamados_supervisao.py:159` |
| 2115 | `renovar_contexto` | D.save | SEM-ATOMIC no único caller encontrado `alertar_disputas_pendentes.py:48`, loop `42`, handle `30` |
| 2153 | `reabrir_admin` | D.save | SEM-ATOMIC: `acoes_disputa.py:256`, vindo `fio.py:583`; factory conversa `260` e reabertura CH `267` vêm depois |
| 2169 | `reabrir_sistema` | D.save | Misto. TRAVA-ANTES em `relavra_via.py:230→261`, `reabrir_orfas_enterradas.py:153→165`, `materializacao.py:676→683`. Via `models.py:1583` ocorre depois da saída do atomic próprio `1547–1555`; depende de atomic externo do caller |
| 2227 | `reabrir_por_colab` | D.save | SEM-ATOMIC nas views `chamados/views.py:1334` / `api/views.py:2718`: `_reabrir` termina seu atomic antes de voltar (`disputa_emissao.py:1496–1520`) |
| 2253 | `limpar_resolucao_residual` | D.save | INDETERMINADO: método sem atomic/trava, nenhum caller de produção encontrado |
| 2769 | `_nascer` | P.save existente | TRAVA-ANTES `2691`; ramo só existe sob `if not conversa` (`2713`) |
| 2773 | `_nascer` | P.create | Misto: datada TRAVA-ANTES `2691`; conversa SEM-TRAVA no caminho `acoes_disputa.py:260`, atomic da factory `2689` e guarda da trava `2690` |
| 2907 | `carimbar` | P.save | SEM-TRAVA comprovado pelo consumidor: `reconciliador.py:1355→582/586`; não há atomic externo nessa etapa, save abre atomic `3173`. Outro caminho `1365→1136/1137→encerrar_fantasma→carimbar` também sem C |
| 2945 | `carimbar_identidade` | P.save | Misto/depende do caller: coberto por D.fechar `1902→1913→2819`; remover motivo `2018→2053`; restante em fio/materialização precisa consolidação do outro explorer |
| 2967 | `descarimbar` | P.save | Misto: TRAVA-ANTES em `relavra_via.py:230→246`, `reabrir_orfas_enterradas.py:153→157`; SEM-TRAVA em `competencia_trancada.py:253–254→models.py:3073`, salvo trava de outer caller ainda não lido |
| 3015–3017 | `descarimbar_em_lote` | P.QuerySet.update | Misto: TRAVA-ANTES via `models.py:1552→1555`; sem C próprio via `disputa_emissao.py:1496→1516` |
| 3027 | `encerrar_por_fato_no_espelho` | P.save | SEM-TRAVA: único caller encontrado command `encerrar_perguntas_com_fato.py:67–68`, atomic por pergunta, antes de fechamento D em outro atomic `87–88` |
| 3174 | `save` override | P.super.save | INDETERMINADO por caller; sempre abre atomic `3173`, portanto não classificar esse sítio como SEM-ATOMIC |

Precisões para consolidação:

- A porta de campo retorna sem travar em autocommit (`models.py:483–486`). Não se pode transformar todo `trava_a_ordem_para_escrever()` em TRAVA-ANTES sem subir ao caller.
- `ressuscitar_perguntas` trava C em `1552`, atualiza P em `1555`, sai do próprio atomic e depois reabre D em `1582–1583`. Callers encontrados: `models.py:1441`, `views_arquivar.py:86`, `cartorio.py:1622`. O `reabrir()` de CH também não mantém um atomic próprio entre `_gravar_estado` (`1432`) e ressuscitação (`1441`).
- `cobranca.py:176–178` percorre `pais` obtidos pelos FKs `p.disputa.chamado` (`51–67`), partindo de pendências de um colaborador (`163–164`). Dado fornecido não prova se existem pais de dono diferente; multiplicidade de donos nesse laço é [nao verificado].
- O consumidor chama os carimbos de P sem atomic externo: `reconciliador.py:1345–1373`; só a marcação de EventoCelula abre atomic em `1378`. Travas obtidas e já commitadas pela etapa anterior não cobrem esses saves. [nao verificado]

Signals que disparam escrita D/CH/P, ainda que por delegação:

| Receiver | Passo de escrita |
|---|---|
| `signals.py:19` disputa fechada | atomic `41`, CH.reconciliar `42` → CH._gravar_estado e trava `models.py:290` |
| `signals.py:54` urgência ao resolver | chama CH._rebaixar_sem_post_save `72` → update `models.py:636`; sem atomic/trava próprios |
| `signals.py:91` resolver perguntas | atomic `114`, resolver `115` → serviço de disputa |
| `signals.py:121` retomar regularizações | callback `132–144`, chama `retomar_regularizacoes_confirmadas` `137` |
| `signals.py:148` materializar pendentes | serviço `163`, na transação originadora |
| `signals.py:188` cartório CH | `203`, na transação originadora |
| `signals.py:215` becos P | callback `220–225`, serviço `224` |
| `signals.py:256` isento Colaborador | callback `261–266`, silenciar `265` |
| `signals.py:270` isento CH | callback `288–293`, silenciar `292` |
| `signals.py:312` feriado | callback `317–322`, reavaliar `321` |
| `signals.py:337` filho morto | callback `350–356`, reconciliador `355` |

`_cartorio_pre_save_chamado` apenas lê (`signals.py:179–184`); `_outbox_post_save_pergunta` grava EventoCelula, não uma das três entidades (`signals.py:238–244`).

Configurações lidas: `config/settings/saas.py:83–92` e `prod.py:9–18` não ativam ATOMIC_REQUESTS; busca em `app/config` não achou ATOMIC_REQUESTS/AUTOCOMMIT. Perfil/config efetivamente carregado em produção: [nao verificado].

## 07/11 21:33 · sessao 348e377151bc · FIO-E0-V4-E1 · verificador · origem=Code

**1. C8 — PROCEDENTE o +2 por save; NÃO PROCEDE tratá-lo como explicação completa do caminho.** O helper de campo pede C e aquele Chamado; isso executa dois SELECTs, mesmo quando as travas são reentrantes. `renovar_contexto` chama o helper antes do save e pode retornar antes dele por igualdade. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`, `app/chamados/services/contexto.py:103`)

Há outro custo que essa justificativa não discrimina: na emissão, `trava_na_ordem(...abertas_do_colaborador=True)` consulta **C, Disputas abertas e Chamados vivos/pais**; depois ocorre uma leitura simples separada da Disputa. São quatro SELECTs nesse trecho, inclusive quando os conjuntos de Disputas/Chamados estão vazios. A fábrica do cartório alcança esse núcleo. (`app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:531`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:662`)

Portanto, **a busca dos Chamados vivos e a separação entre travar e reler a Disputa ficam fora da decomposição escrita dos tetos**. O comentário histórico descreve apenas +1 de C nesse portão; os comentários novos atribuem +2/+6 às portas de campo. (`app/ponto/tests/test_selo_chokepoint_escrita.py:174`, `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

**Que essas consultas adicionais tenham sido introduzidas exatamente em 12b/12c, ou expliquem determinado delta contra versões anteriores: [nao verificado].** O delta fornecido só muda tetos/comentários; não fornece aquela implementação anterior. Assim, o +2/+6 está explicado localmente, mas a exclusividade desse acréscimo não está provada. **[nao verificado]**

**2. Triagem v14, itens 1 a 7.**

| Item | Veredito da conferência |
|---|---|
| **1 — Ordem global** | **PROCEDENTE o residual.** O comando envolve várias células em um atomic; o cartório percorre células/chamados e ressuscita; cada ressuscitação antecipa apenas suas Disputas, seu Chamado e suas Perguntas. Com duas candidatas distintas, D2/CH2 podem ser aquisições novas após P1. Chamá-lo “pergunta de desenho” não fecha a violação da ordem. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1552`) |
| **2 — Porta fora de transação** | **NÃO PROCEDE como inversão sob autocommit.** A porta de arquivo não abre atomic; o helper retorna fora de atomic. A triagem preserva a ressalva que v14 já fazia. Configuração efetiva de autocommit em produção: **[nao verificado]**. (`app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:171`, `app/chamados/models.py:484`) |
| **3 — Ressuscitação** | **PROCEDENTES a janela e a seleção anterior à trava.** O atomic interno termina após descarimbar, e a reabertura vem depois; o universo é escolhido antes da aquisição de C. Classificar como preexistente não corrige esses fatos, mas também não contradiz a ressalva de v14. Comparação histórica independente: **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1547`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`, `app/chamados/models.py:1582`) |
| **4 — Campos/comportamento** | **NÃO PROCEDE alteração explícita de valores no delta dos tetos. PROCEDENTE a diferença de disparo do S143 na volta inteira:** saves de campo retornam pela guarda. Declarar essa mudança não a torna apenas ordem de travas. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`, `app/chamados/signals.py:98`, `app/chamados/signals.py:104`) |
| **5 — Catraca** | **PROCEDENTE-LIMITE.** Ela procura `.save`, aceita chamada de ordem por posição textual e não verifica receptor, ramo ou fluxo; seu universo é limitado. Conferência somente em memória reproduziu **0 acusações atuais e 16 sem o helper**. Isso confirma mordida local, sem cobrir o lote residual. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:228`, `app/chamados/tests/test_e1_v12.py:234`, `app/chamados/tests/test_e1_v12.py:242`, `app/chamados/tests/test_e1_v12.py:137`) |
| **6 — Custo/selo** | **PROCEDENTE a multiplicação por save.** A justificativa é incompleta como censo do caminho, conforme item 1. Atribuir a diferença entre execuções a “memo/relógio” como causa estabelecida é **[nao verificado]**. (`app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/disputa_emissao.py:530`) |
| **7 — Classes anteriores** | **Ordem permanece PROCEDENTE**, pelo item 1. **Dois significados permanecem PROCEDENTES:** coluna nomeada com `update_fields`, valor diferente no save inteiro; “declarado” reconhece, não elimina a diferença. “Pré-requisito fechado pela sombra no hash final”: **[nao verificado]**, sem prova dessa execução no material. (`app/chamados/models.py:1552`, `app/chamados/signals.py:79`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`) |

As duas afirmações sobre as curas candidatas conferem:

- **“Topo com abertas não alcança a FECHADA” — PROCEDENTE.** A flag filtra `fechada_em__isnull=True`; a ressuscitação procura expressamente Disputas fechadas para reabri-las. A aquisição local da segunda ressuscitação não antecipa D2 antes de P1. (`app/chamados/models.py:2552`, `app/chamados/models.py:1582`, `app/chamados/models.py:1552`)
- **“C primeiro basta exige censo” — PROCEDENTE.** Existem escritores sem aquisição local prévia de C: `descarimbar_em_lote` faz UPDATE direto; `reabrir_para_correcao` o chama ou usa `qs.update`; a resposta HTML chega ao save da Pergunta sem essa aquisição na cadeia examinada. O override de `Pergunta.save` acrescenta atomic, sem adquirir C. (`app/chamados/models.py:3015`, `app/chamados/services/disputa_emissao.py:1496`, `app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1518`, `app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1388`, `app/chamados/models.py:3173`)

**A triagem não refutou os achados que reconheceu como desenho, preexistência ou limite. Se esses rótulos forem usados como baixa, o motivo é insuficiente; especialmente nos itens 1 e 7.** A ordem residual e os dois predicados continuam presentes. Deadlock efetivo e ocorrência das duas candidatas em produção: **[nao verificado]**. (`app/chamados/models.py:1552`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`)

**3. Estado que explica 211 versus 217 — [nao verificado].** Passar sob teto 211 demonstra **≤211**, sem revelar a contagem exata: o selo usa `assertLessEqual`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:240`, `app/ponto/tests/test_selo_chokepoint_escrita.py:241`)

O código oferece fontes de variação, mas não identifica qual ocorreu nas duas rodadas:

- **Relógio:** `setUpTestData` calcula `dia = localdate()-2`; o cartório consulta novamente data/hora; o runner permite congelamento por `CONGELAR_EM`. Valores efetivos nas duas execuções: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:229`, `app/ponto/services/cartorio.py:1965`, `app/ponto/services/cartorio.py:1989`, `app/core/runner_quarentena.py:94`)
- **Memo:** empresa e tranca são memorizadas no request ou na instância do colaborador. Os FICAM não fazem limpeza explícita; a limpeza existente pertence à comparação com rollback em outro teste. Isso não prova aquecimento pelo `setUpTestData` nem transmissão entre métodos. Estado efetivo do memo: **[nao verificado]**. (`app/chamados/juizes.py:957`, `app/chamados/juizes.py:963`, `app/chamados/juizes.py:993`, `app/ponto/tests/test_selo_chokepoint_escrita.py:393`, `app/ponto/tests/test_selo_chokepoint_escrita.py:440`)
- **Percurso:** o espelho pode retornar sem save; o 3-FICAM exige somente **pelo menos um** chamado vivo, não três. Portanto, o passe não certifica três saves de espelho. (`app/ponto/management/commands/detectar_ausencias.py:556`, `app/ponto/management/commands/detectar_ausencias.py:558`, `app/ponto/tests/test_selo_chokepoint_escrita.py:425`)

**Não há no dado fotografia de memo, relógio e consultas das duas execuções que permita atribuir as consultas ausentes a um desses estados. A explicação causal afirmada pela triagem permanece [nao verificado].**

## 07/11 21:31 · sessao 1e8be6e76dc3 · - · - · origem=Ronald (tmux)

Aprofundamento: **não há prova de que o memo foi aquecido por `setUpTestData`**, nem de que o memo passou de um método do selo ao seguinte; a limpeza dentro de uma comparação com rollback não demonstra essa herança (`app/ponto/tests/test_selo_chokepoint_escrita.py:435-447`).

- Colaborador nasce antes de `cls.dia`; seu `save` normaliza campos e gera matrícula, sem chamar o memo do C6 (`app/ponto/tests/test_selo_chokepoint_escrita.py:226-229`; `app/colaboradores/models.py:415-439`).
- Escala nasce depois de `cls.dia`; seu receiver recompõe turnos numa janela calculada pelo relógio, enquanto os receivers de regeneração retornam quando `created=True` (`app/ponto/tests/test_selo_chokepoint_escrita.py:230-233`; `app/ponto/signals.py:53-74`; `app/escala/signals.py:54-55`; `app/escala/signals.py:104-105`). Aquecimento indireto do memo por esse percurso: **[nao verificado]**.
- `get_user_sistema` não possui cache próprio: executa `get_or_create` (`app/core/utils.py:6-18`).
- O runner permite congelamento pelo ambiente `CONGELAR_EM`; a utilização e o instante nas rodadas citadas são **[nao verificado]** (`app/core/runner_quarentena.py:93-100`).

**Conclusão mantida:** a causa das consultas ausentes continua **[nao verificado]**. O dado fornece um passe sob teto 211, não contagem exata nem fotografia de memo, relógio ou consultas (`app/ponto/tests/test_selo_chokepoint_escrita.py:240-246`).

## 07/11 21:31 · sessao 69aba3bfc04d · - · - · origem=Ronald (tmux)

**4 — NÃO PROCEDE alteração explícita dos valores pelo delta fornecido; PROCEDENTE a mudança de comportamento na volta inteira.** A guarda do S143 retorna para save que não nomeia `status_local`; portanto há mudança de disparo, declarada no próprio código. (`app/chamados/signals.py:98`, `app/chamados/signals.py:104`) O delta anexado altera somente tetos/comentários; demais diferenças históricas para o pai são **[nao verificado]**.

**5 — PROCEDENTE-LIMITE.** A catraca procura apenas `ast.Call` cujo método é um `Attribute.save`, aceitando qualquer chamada de ordem em linha anterior, sem conferir receptor, ramo ou fluxo. Alias do método não é visto; alias do objeto continua sendo `.save`. (`app/chamados/tests/test_e1_v12.py:228`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`) O universo é limitado a quatro módulos e à classe `ChamadoColaborador`; helpers externos ficam fora. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:242`) Os testes de mordida existem; execução e contagem efetiva de 16 acusações nesta revisão são **[nao verificado]**. (`app/chamados/tests/test_e1_v12.py:253`, `app/chamados/tests/test_e1_v12.py:259`)

**6 — PROCEDENTE o custo de duas queries por helper executado; NÃO PROCEDE usá-lo como descrição completa do caminho.** O helper retorna fora de atomic ou sem PK e chama `trava_na_ordem` somente com C e CH; isso executa os SELECTs de C e CH. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`) `renovar_contexto` pode retornar antes do helper. (`app/chamados/services/contexto.py:98`)

Além disso, no núcleo da emissão, `trava_na_ordem(...abertas_do_colaborador=True)` executa C, Disputas e Chamados vivos; depois há uma leitura simples separada da Disputa. São **quatro SELECTs** nesse trecho, mesmo se as coleções de D/CH retornarem vazias. (`app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:531`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`) Esse núcleo é alcançado pela fábrica do cartório. (`app/chamados/services/fabrica_da_celula.py:184`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:662`) C8 descreve o +1 histórico de C e o +2/+6 das portas, mas não explica nessa justificativa a busca dos vivos e a separação entre trava e leitura do núcleo. (`app/ponto/tests/test_selo_chokepoint_escrita.py:174`, `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`) Quantificação desse delta contra implementação anterior não disponível: **[nao verificado]**.

**7 — PROCEDENTE a duplicidade de predicados, mesmo declarada.** Com `update_fields`, `_salvou_o_status` pergunta se a coluna foi nomeada; no save inteiro pergunta se o valor difere da leitura anterior. Declarar não transforma os dois critérios num só. (`app/chamados/signals.py:79`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`) “Pouso antes do pré-requisito fecha com sombra no hash final”: **[nao verificado]**; o dado apenas afirma o fechamento, sem prova da sombra executada.

## 07/11 21:30 · sessao a70fe2b95732 · - · - · origem=Ronald (tmux)

- **Triagem 1: PROCEDENTE quanto ao residual; “é desenho” não o fecha.** O comando mantém várias células sob um atomic (`app/ponto/management/commands/celula_veredito_velho.py:133`, `:141`, `:144`); o cartório percorre células e chamados e ressuscita (`app/ponto/services/cartorio.py:1303`, `:1591`, `:1622`). Cada ressuscitação trava apenas suas disputas, seu chamado e seus IDs (`app/chamados/models.py:1541`, `:1552`): a segunda pode adquirir D2/CH2 novos depois de P1; o rastreador rejeita precisamente aquisição nova de rank menor (`app/chamados/tests/test_e1_v9.py:86`, `:106`). Existência das duas candidatas em produção e deadlock efetivo: **[nao verificado]**.

- **Candidata “topo com abertas”: insuficiência PROCEDENTE.** A flag adiciona somente disputas `fechada_em__isnull=True` (`app/chamados/models.py:2552`), enquanto ressuscitação alcança expressamente as fechadas e as reabre (`app/chamados/models.py:1582`, `:1583`). A trava local pode pegá-las antes da própria Pergunta, mas não antecipa a fechada da segunda ressuscitação antes de P1 (`app/chamados/models.py:1552`).

- **Candidata “C primeiro basta”: garantia universal NÃO demonstrada; exigência de censo PROCEDENTE.** Há escritores sem aquisição local de C: `descarimbar_em_lote` faz UPDATE direto (`app/chamados/models.py:3007`, `:3015`); `reabrir_para_correcao` entra em atomic e faz esse UPDATE ou `qs.update`, sem chamar a trava (`app/chamados/services/disputa_emissao.py:1496`, `:1516`, `:1518`). Há caminho produtivo de resposta HTML até `p.save()` sem C nessa cadeia: view seleciona e chama core (`app/chamados/views.py:1228`, `:1239`, `:1283`), core salva dentro de atomic (`app/chamados/services/disputa_emissao.py:1388`, `:1389`), e o override apenas envolve `super().save()` em atomic (`app/chamados/models.py:3173`, `:3174`). A API também chama o core diretamente (`app/api/views.py:2644`, `:2659`). Esses exemplos derrubam a premissa “todos entram por C”; não demonstram, sozinhos, um ciclo de deadlock **[nao verificado]**.

- **Triagem 2: NÃO PROCEDE como inversão sob autocommit; não descartou o que v14 afirmava.** View chama `silenciar` (`app/chamados/views_arquivar.py:37`), porta não abre atomic e salva (`app/chamados/services/arquivo.py:141`, `:170`, `:171`), helper retorna fora de atomic (`app/chamados/models.py:484`). Receiver pode chamar cartório quando status da instância diverge da leitura (`app/chamados/signals.py:184`, `:192`, `:203`). Configuração e estado efetivos de autocommit na execução: **[nao verificado]**.

- **Triagem 3: janela PROCEDENTE; classificá-la como preexistente não a corrige.** O bloco interno termina após descarimbar (`app/chamados/models.py:1547`, `:1555`); reabertura vem depois (`app/chamados/models.py:1559`, `:1582`). Seleção ocorre antes da trava (`app/chamados/models.py:1533`, `:1541`, `:1552`). Comparação histórica independente com pai indisponível nesta cópia sem `.git`: **[nao verificado]**.

- **Triagem 7: “declarado” não elimina dois significados.** O código declara e implementa coluna nomeada quando há `update_fields` e valor divergente no save inteiro (`app/chamados/signals.py:79`, `:85`, `:87`). Ordem permanece o residual do item 1 (`app/chamados/models.py:1552`). “Fecha com prova da sombra no hash final” não pode ser confirmado pelo material fornecido: **[nao verificado]**.

## 07/11 21:30 · sessao 1e8be6e76dc3 · - · - · origem=Ronald (tmux)

**Causa exata da diferença: [nao verificado].** O dado relata que o 3-FICAM passou com teto 211; isso demonstra **≤211**, não medição exata de 211. O selo usa `assertLessEqual`, sem imprimir contagem quando passa (`app/ponto/tests/test_selo_chokepoint_escrita.py:240-246`).

O código comprova estas fontes de variação:

- **Data e relógio:** `setUpTestData` calcula `cls.dia = localdate()-2`, sem data fixa; o cartório consulta novamente `localdate()` e `now()` ao julgar (`app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`).
- **Memo:** o leitor guarda empresa e resultado de `periodo_trancado` por chave de empresa/mês/ano; o dono é o request ou a instância do colaborador (`app/chamados/juizes.py:957-965`; `app/chamados/juizes.py:983-996`). Os testes FICAM não limpam esse memo; a limpeza explícita existente pertence a outro teste, de comparação do gate (`app/ponto/tests/test_selo_chokepoint_escrita.py:393-426`; `app/ponto/tests/test_selo_chokepoint_escrita.py:435-440`).
- **Caminho efetivamente executado:** `_espelhar_marcos` retorna sem save se a lista já coincide; o selo 3-FICAM exige apenas **pelo menos um** dos três chamados vivo (`app/ponto/management/commands/detectar_ausencias.py:555-559`; `app/ponto/tests/test_selo_chokepoint_escrita.py:423-426`).

Portanto, “dependência de ordem por memo/relógio do setUpTestData” é uma hipótese compatível com essas portas, mas a execução anterior que aqueceu o memo, os valores do memo, as datas/horas de ambas as rodadas e as consultas ausentes não constam do dado: **[nao verificado]**. Não atribuir as seis consultas especificamente ao memo, ao relógio ou a três saves sem dump comparativo: **[nao verificado]**.

## 07/11 21:11 · sessao c09619854497 · FIO-E0-V4-E1 · verificador · origem=Code

**PROCEDENTE: ainda existe aquisição de Disputa/Chamado depois de Pergunta do mesmo colaborador, na mesma transação. A cura unitária não fecha a ordem global do lote.** O contraexemplo estático é o cartório ressuscitando perguntas de duas células do mesmo colaborador sob atomic externo. Ocorrência desse estado na frota e deadlock efetivo: **[nao verificado]**. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1552`)

**1. Ordem global — PROCEDENTE o residual; NÃO PROCEDE repetir como intactas as inversões unitárias corrigidas.**

A cadeia residual é:

| Passo | Evidência |
|---|---|
| `celula_veredito_velho`, inclusive com `--aplicar`, envolve a janela em um atomic e entrega várias células do mesmo colaborador ao cartório. | `app/ponto/management/commands/celula_veredito_velho.py:44`, `app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/management/commands/celula_veredito_velho.py:156` |
| `julgar_colab → _julgar_colab_corpo` percorre as células; cada chamado elegível pode ressuscitar suas perguntas. | `app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622` |
| Primeira ressuscitação: adquire `C → D1 → CH1 → P1`. Seu universo contém apenas as disputas das perguntas escolhidas, `self.pk` e os IDs escolhidos. | `app/chamados/models.py:1541`, `app/chamados/models.py:1552` |
| Segunda célula elegível, com D2/CH2 distintos: C é reentrante; **D2 e CH2 são aquisições novas depois de P1**, ainda no atomic externo. | `app/chamados/models.py:1552`, `app/chamados/models.py:2548`, `app/chamados/models.py:2554`, `app/chamados/models.py:2567` |

Esse contraexemplo pressupõe duas ressuscitações elegíveis e nenhuma aquisição anterior cobrindo D2/CH2. Os reconciliadores anteriores são condicionais; não garantem antecipadamente esse universo. Existência concreta das candidatas: **[nao verificado]**. (`app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1501`, `app/ponto/services/cartorio.py:1514`)

**Isso demonstra quebra da ordem exigida, não um deadlock demonstrado:** C já está retido e serializa outra transação que também entre por C. Reentrância de C não torna D2/CH2 reentrantes. O próprio rastreador denuncia a aquisição nova de rank menor depois de Pergunta. (`app/chamados/models.py:2547`, `app/chamados/tests/test_e1_v9.py:86`, `app/chamados/tests/test_e1_v9.py:106`)

As **16 portas** efetivamente chamam o helper antes do save:

| Grupo | Portas e localização da trava |
|---|---|
| Texto — 1 | `reescrever_texto`: `app/chamados/services/texto.py:35` |
| Contexto — 4 | `anotar_contexto`: `app/chamados/services/contexto.py:77`; `renovar_contexto`: `app/chamados/services/contexto.py:103`; `retirar_contexto`: `app/chamados/services/contexto.py:126`; `fundir_contexto`: `app/chamados/services/contexto.py:162` |
| Vínculos — 5 | `apontar_justificativa`: `app/chamados/services/vinculos.py:88`; `apontar_celula`: `app/chamados/services/vinculos.py:100`; `apontar_ticket_glpi`: `app/chamados/services/vinculos.py:116`; `assumir_responsavel`: `app/chamados/services/vinculos.py:134`; `liberar_responsavel`: `app/chamados/services/vinculos.py:146` |
| Arquivo — 2 | `silenciar`: `app/chamados/services/arquivo.py:170`; `levantar`: `app/chamados/services/arquivo.py:209` |
| Modelo — 4 | `rebaixar`: `app/chamados/models.py:497`; `escalar_urgencia`: `app/chamados/models.py:552`; `definir_prazo_sla`: `app/chamados/models.py:590`; `marcar_escalonado_dp`: `app/chamados/models.py:609` |

O helper adquire **Colaborador e aquele Chamado**, sem antecipar o universo de Disputas. Ele corrige a aquisição tardia de C nessas portas, mas não prova sozinho a ordem de toda continuação possível. (`app/chamados/models.py:486`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`)

Em `agrupar_furo`, a cadeia unitária anterior foi corrigida: o topo adquire C, Disputas abertas, pais/vivos e furo/pai antes de contexto, reapontamento e reconciliação. A solicitação posterior dessas mesmas linhas é reentrante. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:282`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/fio.py:855`)

Os receivers relevantes ficam assim:

| Receiver | Cadeia e efeito sobre a ordem |
|---|---|
| Chamado: rebaixamento | Faz UPDATE de urgência no mesmo PK, pelo ritual do modelo. (`app/chamados/signals.py:72`, `app/chamados/models.py:636`) |
| Chamado: S143 | Save de campo retorna pela guarda; save canônico de status já adquiriu o universo antes. Depois, o resolver solicita C → Disputas → Chamados e pode carimbar Perguntas. (`app/chamados/signals.py:104`, `app/chamados/models.py:290`, `app/chamados/services/disputa_emissao.py:1683`, `app/chamados/services/disputa_emissao.py:1749`) |
| Chamado: cartório | Compara banco/instância e pode julgar sincronamente, inclusive com instância desatualizada. A fábrica pode alcançar nova aquisição de universo. (`app/chamados/signals.py:191`, `app/chamados/signals.py:203`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/disputa_emissao.py:530`) |
| Chamado: isentos e filho morto | Registram trabalho em `on_commit`; esse trabalho não permanece dentro da transação anterior. (`app/chamados/signals.py:293`, `app/chamados/signals.py:356`) |
| Disputa: S88 | Ao salvar fechada, pode chamar `pai.reconciliar → _gravar_estado → trava_na_ordem`. O fechamento canônico antecipa o universo; na reabertura, S88 retorna. (`app/chamados/signals.py:28`, `app/chamados/signals.py:42`, `app/chamados/models.py:1902`, `app/chamados/models.py:290`) |
| Disputa: retomada | Trabalho em `on_commit`. (`app/chamados/signals.py:144`) |
| Disputa: S144 | Ao fechar, materializa pendentes pela porta de materialização; na reabertura retorna. (`app/chamados/signals.py:153`, `app/chamados/signals.py:163`, `app/chamados/services/materializacao.py:1428`) |

**2. Porta fora de transação — PROCEDENTE o caminho; NÃO PROCEDE chamá-lo inversão pelo UPDATE já confirmado.**

Existe caminho condicional: `arquivar_chamado → arquivo.silenciar`, sem atomic próprio; o helper retorna. Se o status da instância estiver desatualizado, o post_save do cartório pode prosseguir e alcançar, por exemplo, `ressuscitar_perguntas`, que abre atomic e pede C. S143 não faz isso pelo save de campo, pois `update_fields` não contém status. (`app/chamados/views_arquivar.py:37`, `app/chamados/services/arquivo.py:170`, `app/chamados/models.py:484`, `app/chamados/signals.py:86`, `app/chamados/signals.py:191`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1547`)

**Sob a premissa de autocommit**, a transação do UPDATE já terminou: ela não continua segurando o Chamado enquanto o receiver pede C em outra transação. Portanto, essa sequência temporal não constitui aquela inversão de travas retidas. Configuração efetiva e execução concorrente em produção: **[nao verificado]**. (`app/chamados/models.py:480`, `app/chamados/models.py:484`, `app/chamados/services/arquivo.py:171`)

Texto, contexto e vínculos abrem atomic antes do helper; entrar nessas funções sem transação externa não faz seu save ocorrer em autocommit. As duas portas de arquivo e os quatro métodos de campo do modelo não abrem atomic próprio. (`app/chamados/services/texto.py:31`, `app/chamados/services/contexto.py:101`, `app/chamados/services/vinculos.py:86`, `app/chamados/services/arquivo.py:141`, `app/chamados/models.py:488`, `app/chamados/models.py:573`)

**3. Ressuscitação — PROCEDENTE a janela; NÃO PROCEDE atribuí-la como novidade ao delta.**

Sem atomic externo, o bloco termina depois de descarimbar as Perguntas, antes do log e de `reabrir_sistema`. Pode ficar confirmado o estado intermediário “Pergunta ressuscitada / Disputa fechada”; erro ou concorrência nessa separação continua possível. A trava nova não ampliou essa fronteira transacional. Ocorrência efetiva: **[nao verificado]**. (`app/chamados/models.py:1547`, `app/chamados/models.py:1555`, `app/chamados/models.py:1559`, `app/chamados/models.py:1582`)

Com atomic externo, as travas sobrevivem à saída do atomic interno. Reabrir **as mesmas Disputas** é reentrante; S88/S144 retornam por estarem abertas, e a nota de reabertura cria Followup sem passar contexto novo. Isso não cobre outra ressuscitação do lote, como demonstrado no item 1. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1583`, `app/chamados/models.py:2172`, `app/chamados/signals.py:28`, `app/chamados/signals.py:153`, `app/chamados/services/nota.py:110`)

Também permanece uma seleção anterior à trava: IDs, julgamento e disputas são obtidos antes de adquirir C; a porta não refaz o julgamento depois de eventual espera. Portanto, a adição não prova ausência de seleção envelhecida. Dano concorrente concreto: **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`)

**4. Campos — NÃO PROCEDE mudança explícita de valores no delta 12b/12c; a volta inteira não é somente ordem de travas.**

Os escritores canônicos e consumidores relevantes às cadeias revisadas são:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `status_local` | Transição em `_gravar_estado`; nascimento em `abrir/criar`. (`app/chamados/models.py:228`, `app/chamados/models.py:296`, `app/chamados/models.py:971`, `app/chamados/models.py:1095`) | S143, cartório e C6. (`app/chamados/signals.py:96`, `app/chamados/signals.py:191`, `app/chamados/juizes.py:758`) |
| `contexto_json` do Chamado | As quatro portas de contexto. (`app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:102`, `app/chamados/services/contexto.py:125`, `app/chamados/services/contexto.py:161`) | Cartório lê dia; espelho lê marcos; reconciliador lê filhos. (`app/ponto/services/cartorio.py:126`, `app/ponto/management/commands/detectar_ausencias.py:555`, `app/chamados/reconciliador.py:242`) |
| `contexto` da Disputa | Emissão, remoção de motivo, anotar/renovar. (`app/chamados/services/disputa_emissao.py:544`, `app/chamados/models.py:2057`, `app/chamados/models.py:2097`, `app/chamados/models.py:2115`) | Merge e guardas das próprias portas. (`app/chamados/services/disputa_emissao.py:544`, `app/chamados/models.py:2093`, `app/chamados/models.py:2111`) |
| Justificativa relacionada | `apontar_justificativa`. (`app/chamados/services/vinculos.py:87`) | Reconciliação. (`app/chamados/reconciliador.py:451`, `app/chamados/reconciliador.py:529`) |
| Célula | `apontar_celula`. (`app/chamados/services/vinculos.py:99`) | Juiz do dia. (`app/chamados/juizes.py:100`) |
| Ticket GLPI | `apontar_ticket_glpi`. (`app/chamados/services/vinculos.py:115`) | Encaminhamento da view. (`app/chamados/views.py:220`) |
| Responsável/instante | Assumir/liberar. (`app/chamados/services/vinculos.py:132`, `app/chamados/services/vinculos.py:144`) | Resposta da atribuição e fila. (`app/chamados/views.py:1036`, `app/chamados/services/fila.py:154`) |
| Arquivo/véu | `silenciado_ate/por/motivo`, opcionalmente `resgatado_em`, pelas portas de arquivo. Não existe choice de status “arquivado”. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:28`, `app/chamados/models.py:87`) | Predicado temporal, reconciliação da ausência, isentos e prescrição. (`app/chamados/models.py:511`, `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/signals.py:284`, `app/chamados/services/arquivo.py:182`) |
| Urgência/original | Ritual de rebaixamento, escalar, transição e UPDATE do receiver. (`app/chamados/models.py:468`, `app/chamados/models.py:553`, `app/chamados/models.py:246`, `app/chamados/models.py:636`) | Guarda de escalada pelo juiz de SLA. (`app/chamados/models.py:546`) |
| `prazo_sla` | Porta de SLA e nascimento na emissão. (`app/chamados/models.py:588`, `app/chamados/services/disputa_emissao.py:581`) | Juiz do prazo e cobrança. (`app/chamados/catalogo/motor.py:54`, `app/chamados/catalogo/motor.py:68`, `app/chamados/services/cobranca.py:246`) |
| Escalonado DP/em | `marcar_escalonado_dp`. (`app/chamados/models.py:606`) | Comando lê flag; fila lê instante. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/fila.py:181`) |
| Texto do Chamado | `reescrever_texto`, além do nascimento. (`app/chamados/services/texto.py:18`, `app/chamados/services/texto.py:33`) | A própria porta compara o valor anterior. (`app/chamados/services/texto.py:28`) |
| `atualizado_em` | Listado nos saves das portas. (`app/chamados/services/contexto.py:52`, `app/chamados/services/vinculos.py:65`, `app/chamados/services/texto.py:36`) | Marco de atualização do painel. (`app/chamados/services/painel.py:65`) |
| Materialização/autor/via da Pergunta | Atribuição central e portas de carimbar/descarimbar, inclusive lote. (`app/chamados/models.py:2852`, `app/chamados/models.py:2907`, `app/chamados/models.py:2938`, `app/chamados/models.py:3015`) | Estado, C6, seleção S143; a própria porta preserva/restaura autor. (`app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/disputa_emissao.py:1711`, `app/chamados/models.py:2881`) |
| Resposta/respondida/validação/autor | Resposta, validação/devolução e ressuscitação. (`app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:199`, `app/chamados/models.py:1555`) | Estado/C6, marco do painel e preservação da decisão histórica. (`app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/painel.py:67`, `app/chamados/models.py:3689`) |
| Lavra/julgamento/expiração da Pergunta | `veredito_lavrado.lavrar`. (`app/chamados/veredito_lavrado.py:227`) | `esta_stale`. (`app/chamados/veredito_lavrado.py:179`) |
| `fechada_em` da Disputa | Fechar e reabrir admin/sistema. (`app/chamados/models.py:1924`, `app/chamados/models.py:2141`, `app/chamados/models.py:2165`) | Receivers, ressuscitação e universo de travas. (`app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:1582`, `app/chamados/models.py:2552`) |

No delta fornecido, permanecem as atribuições, os filtros e os `update_fields`; entram aquisições de trava. Isso não garante equivalência entre entrelaçamentos concorrentes nem igualdade de timestamps após espera. Contexto/vínculos continuam decidindo pela instância antes da trava, sem refresh. (`app/chamados/models.py:486`, `app/chamados/models.py:1552`, `app/chamados/services/contexto.py:97`, `app/chamados/services/contexto.py:103`, `app/chamados/services/vinculos.py:84`)

Já a **volta inteira** altera o disparo do S143: saves de campo e saves inteiros sem diferença detectada deixam de tentar o resolver. Logo, “só mudou ordem” é falso para `afb67623..f67b36a6`. (`app/chamados/signals.py:83`, `app/chamados/signals.py:104`)

**5. Catraca — PROCEDENTE o alcance limitado; NÃO localizei outro escritor produtivo por save fora desse conjunto.**

A varredura de escritores da árvore e a inspeção complementar não localizaram `ChamadoColaborador.save()` produtivo adicional fora do modelo e das quatro portas. Escrita dinâmica não demonstrada: **[nao verificado]**. (`app/core/censo_escritas.py:469`, `app/chamados/models.py:296`, `app/chamados/services/texto.py:36`, `app/chamados/services/contexto.py:78`, `app/chamados/services/vinculos.py:89`, `app/chamados/services/arquivo.py:171`)

A catraca não vê `QuerySet.update/bulk_update`, chamada por alias de método (`salvar = ch.save; salvar()`), nem segue helpers externos. Outro módulo fica fora. Alias do **objeto**, como `outro.save()`, continua visível. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`, `app/chamados/tests/test_e1_v12.py:242`)

Ela aceita uma chamada anterior à “ordem” sem conferir receptor, argumentos, ramo executado ou fluxo; `ast.walk` inclui funções internas e só trata `FunctionDef`. Portanto, uma trava em ramo morto ou sobre objeto errado pode liberar o save. (`app/chamados/tests/test_e1_v12.py:229`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:232`)

Na extração executada somente em memória, a árvore atual deu zero acusações; retirar as chamadas ao helper produziu **16 acusações**. Há mordida para a mudança local, mas ela não mede a ordem acumulada entre duas ressuscitações. O RED novo exercita apenas uma ressuscitação seguida de contexto no mesmo Chamado. (`app/chamados/tests/test_e1_v12.py:217`, `app/chamados/tests/test_e1_v12.py:137`, `app/chamados/tests/test_e1_v12.py:140`)

**6. Custo — PROCEDENTE a multiplicação; relevância operacional medida é [nao verificado].**

São duas consultas adicionais por helper executado dentro de atomic: C e Chamado. Reentrância não elimina essas consultas; fora de atomic ou em no-op anterior ao helper, esse acréscimo não ocorre. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`)

| Caminho | Multiplicação verificável |
|---|---|
| Escalonamento a cada cinco minutos | N2 chama escalar e anotar contexto no mesmo atomic: até **+4 por candidato alterado**. N1 chama marcar DP fora de atomic: +0 pelo helper; demais anotações acrescentam +2 quando escrevem. (`app/config/crons.py:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:51`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:138`) |
| Espelho do cartório | +2 por contexto alterado no laço de chamados vivos. (`app/ponto/services/cartorio.py:1787`, `app/ponto/management/commands/detectar_ausencias.py:558`) |
| Rejunção, no caminho da batida/outbox/vigia | +2 por vínculo efetivamente escrito. (`app/chamados/rejuncao.py:73`, `app/chamados/rejuncao.py:107`) |
| Agrupamento de furos | Trava superior de universo, mais duas consultas do helper de contexto, repetidas por furo. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:331`) |
| Cobrança | +2 por pai com SLA alterado no lote. (`app/chamados/services/cobranca.py:176`, `app/chamados/services/cobranca.py:178`) |

Os testes contêm asserts de **7 consultas na escalada** e **6 no vínculo**. Execução desses testes nesta revisão, volume real, contenção e latência: **[nao verificado]**. (`app/chamados/tests/test_porta_chamado_ciclo.py:109`, `app/chamados/tests/test_porta_chamado_ciclo.py:265`, `app/chamados/tests/test_porta_vinculos.py:77`, `app/chamados/tests/test_porta_vinculos.py:339`)

**7. Classes anteriores de reprovação.**

| Classe | Veredito neste recorte |
|---|---|
| Segundo juiz | **NÃO PROCEDE como introdução do delta:** o helper usa a porta existente. (`app/chamados/models.py:486`, `app/chamados/models.py:1552`) |
| Segundo escritor fora da porta | **NÃO PROCEDE como introdução do delta:** permanecem os saves nas portas; o helper só solicita travas. (`app/chamados/services/contexto.py:103`, `app/chamados/services/contexto.py:104`, `app/chamados/models.py:486`) |
| Relógio fora da borda | **NÃO PROCEDE como novidade do delta:** as leituras de relógio dessas portas permanecem anteriores ao helper. (`app/chamados/models.py:551`, `app/chamados/services/texto.py:34`) |
| Derivação paralela | **NÃO PROCEDE como novidade do delta:** a seleção da ressuscitação continua na mesma função. (`app/chamados/models.py:1533`) |
| Selo/RED sem mordida | **PROCEDENTE quanto ao alcance insuficiente; NÃO PROCEDE dizer que é vazio.** A catraca 

[truncado em 20000 de 20886 caracteres]

## 07/11 21:08 · sessao 50a9c5887032 · - · - · origem=Ronald (tmux)

**Q2 — PROCEDENTE o caminho; NÃO PROCEDE chamá-lo inversão de trava retida pelo UPDATE anterior.** `arquivar_chamado → arquivo.silenciar` não abre `atomic`; o helper retorna quando `in_atomic_block=False`. Se a instância traz status velho, o cartório compara status banco/instância, ignora `update_fields` e pode julgar depois do save. Em autocommit, a transação desse UPDATE já acabou; execução concreta e configuração efetiva de produção são **[nao verificado]**. (`app/chamados/views_arquivar.py:37`, `app/chamados/services/arquivo.py:170`, `app/chamados/models.py:484`, `app/chamados/signals.py:184`, `app/chamados/signals.py:191`, `app/chamados/signals.py:203`)

Texto/contexto/vínculos abrem `atomic` antes de chamar o helper; portanto “chamada externa à transação” não significa que seu save ocorra fora de transação. Os quatro métodos de campo do modelo e as duas portas de arquivo não abrem atomic próprio. (`app/chamados/services/texto.py:31`, `app/chamados/services/contexto.py:101`, `app/chamados/services/vinculos.py:86`, `app/chamados/models.py:488`, `app/chamados/models.py:513`, `app/chamados/models.py:573`, `app/chamados/models.py:594`, `app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:177`)

**Q4 — mapa de escritores e leitores concretos:**

| Campo | Escritor | Leitor |
|---|---|---|
| `status_local` | `_gravar_estado` atribui e salva; rebaixa/restaura urgência junto. `app/chamados/models.py:228`, `app/chamados/models.py:246`, `app/chamados/models.py:250`, `app/chamados/models.py:296` | S143, cartório e C6. `app/chamados/signals.py:96`, `app/chamados/signals.py:191`, `app/chamados/juizes.py:758` |
| `contexto_json` do Chamado | Quatro portas de contexto. `app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:102`, `app/chamados/services/contexto.py:125`, `app/chamados/services/contexto.py:161` | Juiz de dia/marcos/agrupamento e reconciliador de filhos. `app/chamados/juizes.py:100`, `app/chamados/juizes.py:534`, `app/chamados/juizes.py:1462`, `app/chamados/reconciliador.py:242` |
| `contexto` da Disputa | Remover motivo, anotar e renovar. `app/chamados/models.py:2032`, `app/chamados/models.py:2057`, `app/chamados/models.py:2097`, `app/chamados/models.py:2115` | Guardas das próprias portas. `app/chamados/models.py:2093`, `app/chamados/models.py:2111` |
| Justificativa, célula, ticket, responsável | Cinco portas de vínculos. `app/chamados/services/vinculos.py:87`, `app/chamados/services/vinculos.py:99`, `app/chamados/services/vinculos.py:115`, `app/chamados/services/vinculos.py:132`, `app/chamados/services/vinculos.py:144` | Reconciliação, juiz do dia, view GLPI e fila. `app/chamados/reconciliador.py:451`, `app/chamados/reconciliador.py:529`, `app/chamados/juizes.py:100`, `app/chamados/views.py:220`, `app/chamados/services/fila.py:154` |
| Arquivo/véu | `silenciado_ate/por/motivo` e `resgatado_em`, pelas portas de arquivo; arquivo não é choice de status. `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:28`, `app/chamados/models.py:87` | Predicado temporal, dono do silêncio, receiver de isentos. `app/chamados/models.py:507`, `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:80`, `app/chamados/signals.py:278` |
| Urgência/original | Ritual de rebaixamento, escalar, transição e UPDATE do receiver. `app/chamados/models.py:468`, `app/chamados/models.py:498`, `app/chamados/models.py:553`, `app/chamados/models.py:246`, `app/chamados/models.py:636` | Guarda por `sla_limite_horas`. `app/chamados/models.py:546` |
| SLA | Porta e nascimento dos chamados de disputa. `app/chamados/models.py:588`, `app/chamados/services/disputa_emissao.py:581`, `app/chamados/services/disputa_emissao.py:1026`, `app/chamados/services/disputa_emissao.py:1253` | Juiz do prazo e cobrança. `app/chamados/catalogo/motor.py:54`, `app/chamados/catalogo/motor.py:68`, `app/chamados/services/cobranca.py:246` |
| Escalonado DP/em | Porta carimba após push. `app/chamados/models.py:606`, `app/chamados/models.py:610` | Comando lê flag; gestão lê instante. `app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/fila.py:181` |
| Materialização/autor/via da Pergunta | Atribuição central; portas carimbar, identidade, descarimbar e lote. `app/chamados/models.py:2852`, `app/chamados/models.py:2907`, `app/chamados/models.py:2937`, `app/chamados/models.py:2963`, `app/chamados/models.py:3015` | Estado, memo C6 e seleção da materialização/S143. `app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/materializacao.py:434`, `app/chamados/services/disputa_emissao.py:1711` |
| Resposta/respondida/validação/autor | Resposta, validação, devolução e ressuscitação. `app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/disputa_emissao.py:1381`, `app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:199`, `app/chamados/models.py:1555` | Estado e memo do C6; ressuscitação registra projeção antes de apagar. `app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/models.py:1553` |
| `fechada_em` da Disputa | Fechar, reabrir admin, reabrir sistema. `app/chamados/models.py:1924`, `app/chamados/models.py:2141`, `app/chamados/models.py:2165` | S88, retomada, S144, ressuscitação e universo de travas. `app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:1582`, `app/chamados/models.py:2552` |

**NÃO PROCEDE alteração explícita dos valores no delta 12b/12c:** as chamadas novas precedem as atribuições persistidas pelos mesmos `update_fields`; helper só adquire travas. Porém a volta inteira inclui mudança de disparo do S143; afirmar “só ordem” para `afb..HEAD` seria falso. (`app/chamados/models.py:475`, `app/chamados/models.py:497`, `app/chamados/models.py:1552`, `app/chamados/services/contexto.py:103`, `app/chamados/signals.py:104`)

**Limite concorrente preservado:** contexto/vínculos continuam lendo a instância e decidindo antes da trava, sem releitura; a adição da trava não demonstra cura de sobrescrita por fotografia velha. (`app/chamados/services/contexto.py:97`, `app/chamados/services/contexto.py:103`, `app/chamados/services/vinculos.py:84`, `app/chamados/services/vinculos.py:88`)

## 07/11 21:07 · sessao 92e78a3613dc · - · - · origem=Ronald (tmux)

**PROCEDENTE: a volta 12c corrige a cadeia unitária, mas não demonstra a ordem global durante todo o atomic externo.** Há contraexemplo estático por duas ressuscitações do mesmo colaborador, sem depender de duplicados no mesmo dia. (`app/chamados/models.py:1552`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`)

Cadeia:

1. `celula_veredito_velho` abre **um atomic envolvendo a janela inteira**; carrega várias células do mesmo colaborador e chama `julgar_colab`. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:139`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/management/commands/celula_veredito_velho.py:144`)
2. `julgar_colab → _julgar_colab_corpo` percorre as células e chama `ressuscitar_perguntas` para o chamado elegível. (`app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622`)
3. Na primeira célula: `C → D1 → CH1 → P1`. O topo da ressuscitação pede somente suas disputas, `self.pk` e seus IDs de perguntas; não pede `abertas_do_colaborador=True`. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`)
4. Na segunda célula elegível do mesmo C, com D2/CH2 distintos: C é reentrante, **D2 e CH2 são aquisições novas depois de P1**, na mesma transação externa. A implementação não amplia o universo sem a flag. (`app/chamados/models.py:1552`, `app/chamados/models.py:2548`, `app/chamados/models.py:2551`, `app/chamados/models.py:2560`)

O caminho pressupõe duas ressuscitações elegíveis e ausência de uma trava anterior que já cobrisse D2/CH2; os reconciliadores anteriores são condicionais, portanto não constituem pré-requisito incondicional de aquisição desse universo. Estado correspondente na frota e deadlock observado: **[nao verificado]**. (`app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1501`, `app/ponto/services/cartorio.py:1514`, `app/ponto/services/cartorio.py:1522`)

Também há continuação potencial pela fábrica: após o corpo/ressuscitações, `julgar_colab` chama a fábrica, que pode chegar à emissão e adquirir o universo aberto do colaborador — linhas que a ressuscitação não necessariamente adquiriu. Elegibilidade concreta dessa continuação: **[nao verificado]**. (`app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:184`, `app/chamados/services/fabrica_da_celula.py:186`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530`)

**Não confundir violação de rank com deadlock demonstrado:** no contraexemplo a trava C já está retida; outra transação que também obedeça C primeiro será serializada. O fato demonstrável é aquisição nova D2/CH2 após P1, contrariando a ordem global expressa e o critério do rastreador; ciclo concorrente efetivo é **[nao verificado]**. (`app/chamados/models.py:2547`, `app/chamados/tests/test_e1_v9.py:94`, `app/chamados/tests/test_e1_v9.py:106`)

**NAO PROCEDE como inversão nova a cadeia unitária corrigida:** uma ressuscitação sob atomic externo retém C, suas DS, seu Chamado e suas Perguntas. Reabrir as mesmas DS depois do atomic interno é reentrante; o SELECT de reabertura usa os mesmos IDs recolhidos antes. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1582`, `app/chamados/models.py:1583`)

Os receivers de fechamento da Disputa retornam quando a instância está aberta; reabrir não aciona reconciliação S88 nem materialização S144. A nota de `reabrir_sistema` cria Followup e não passa contexto novo para salvar o Chamado. (`app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:2169`, `app/chamados/models.py:2172`, `app/chamados/services/nota.py:103`, `app/chamados/services/nota.py:110`)

**PROCEDENTE a janela sem atomic externo; NAO PROCEDE chamá-la novidade do delta:** o bloco que altera Perguntas termina antes do log e da reabertura das Disputas. Assim, pode haver estado intermediário confirmado “Pergunta ressuscitada / Disputa ainda fechada”; a adição da trava não estende a transação até a reabertura. O diff fornecido acrescenta a trava, mantendo essa fronteira. Ocorrência concorrente real é **[nao verificado]**. (`app/chamados/models.py:1547`, `app/chamados/models.py:1555`, `app/chamados/models.py:1559`, `app/chamados/models.py:1582`; delta anexado `codex_e1_v14.diff` **[nao verificado como arquivo local]**)

Os IDs e as disputas também são coletados **antes** de adquirir C e as linhas; a porta não refaz o julgamento do universo depois de esperar pelas travas. Portanto, a nova trava não prova ausência de seleção envelhecida. Entrelaçamento efetivo e alteração incorreta observada são **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`)

**NAO PROCEDE repetir a inversão antiga no `agrupar_furo` unitário:** agora o topo adquire C, abertas, pais/vivos e furo/pai antes de contexto, reapontamento e reconciliação. O reapontamento pede C e o destino antes de salvar P; essas linhas já estão cobertas no caso corrigido. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:282`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/fio.py:855`, `app/chamados/services/fio.py:858`, `app/chamados/services/fio.py:768`, `app/chamados/services/fio.py:870`)

## 07/11 21:07 · sessao 155a5efe7473 · - · - · origem=Ronald (tmux)

**5. PROCEDENTE o alcance limitado; não localizei escritor produtivo adicional de `ChamadoColaborador.save()`.** O censo AST da árvore, executado somente em memória, retorna os cinco saves do modelo e os doze das quatro portas; sua implementação varre código produtivo, inclusive `management/`. A busca adicional pelos saves em módulos que referenciam o modelo não encontrou outro escritor de Chamado. Escrita dinâmica não demonstrada: **[nao verificado]**. (`app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:54`, `app/chamados/models.py:296`, `app/chamados/services/texto.py:36`, `app/chamados/services/contexto.py:78`, `app/chamados/services/vinculos.py:89`, `app/chamados/services/arquivo.py:171`)

A catraca:

- Examina apenas quatro arquivos e `ChamadoColaborador` no modelo; outro módulo fica fora. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:242`)
- Só reconhece `Call` com `Attribute` literal `save`; `QuerySet.update`, `bulk_update`, `salvar = ch.save; salvar()` ficam fora. Alias do objeto, como `outro.save()`, continua visível. (`app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`)
- Não segue helper; se o helper estiver fora dos arquivos/classe examinados, seu save desaparece desta catraca. (`app/chamados/tests/test_e1_v12.py:242`)
- Aceita chamada anterior à ordem independentemente de receptor, argumentos, ramo executado e domínio do fluxo; `ast.walk(fn)` também inclui funções internas. Só trata `FunctionDef`, não `AsyncFunctionDef`. (`app/chamados/tests/test_e1_v12.py:229`, `app/chamados/tests/test_e1_v12.py:231`)
- O `update` conhecido de `_rebaixar_sem_post_save` permanece no próprio modelo, sobre o mesmo PK, chamado pelo receiver. (`app/chamados/models.py:625`, `app/chamados/models.py:636`)

**6. PROCEDENTE a multiplicação linear de consultas; relevância operacional medida na frota é [nao verificado].** O helper acrescenta duas consultas quando realmente chamado dentro de atomic: Colaborador e Chamado; não consulta Disputa sem parâmetros/universo. Fora de atomic retorna; guardas idempotentes das portas podem retornar antes dele. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`)

| Caminho | Multiplicação verificável |
|---|---|
| Cron de escalonamento, a cada cinco minutos | Laço sobre candidatos; N2 faz `escalar_urgencia` e `anotar_contexto` no mesmo atomic: até **+4** por candidato alterado. N1 chama `marcar_escalonado_dp` fora de atomic: **+0** pelo helper. Outros carimbos de contexto: **+2** cada alteração. (`app/config/crons.py:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:51`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:138`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:163`) |
| Cartório por empresa | Laço de colaboradores e, depois, chamados vivos: **+2** por contexto realmente modificado pelo espelho. (`app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1787`, `app/ponto/management/commands/detectar_ausencias.py:558`) |
| Rejunção no caminho da batida/outbox/vigia | **+2** por vínculo de célula modificado; o próprio código identifica a frequência por linha deste laço. (`app/chamados/rejuncao.py:73`, `app/chamados/rejuncao.py:107`) |
| Agrupamento de furos | Laço de furos; trava superior de universo pode fazer três consultas, seguida das duas do helper de contexto. Reentrância não elimina a nova consulta. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:316`, `app/chamados/services/limbo_folgas.py:331`) |
| Cobrança | **+2** por chamado pai com SLA alterado no atomic do lote. (`app/chamados/services/cobranca.py:176`, `app/chamados/services/cobranca.py:178`) |

Os números declarados no diff estão presos por verificações efetivas de contagem: escalada **7**, vínculo **6**. Execução desses testes com banco nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_porta_chamado_ciclo.py:109`, `app/chamados/tests/test_porta_chamado_ciclo.py:265`, `app/chamados/tests/test_porta_vinculos.py:77`, `app/chamados/tests/test_porta_vinculos.py:339`)

## 07/11 21:00 · sessao 72546ee75f51 · FIO-E0-V4-E1 · censo · origem=Code

O censo abaixo distingue caminhos da cópia; ocorrência efetiva desses caminhos em produção é **[nao verificado]**. Os perfis locais não configuram `ATOMIC_REQUESTS`; configuração efetivamente carregada em produção é **[nao verificado]**. — `app/config/settings/saas.py:83`, `app/config/settings/prod.py:9`

Em **(b)**, “sim” significa transação já aberta **antes de entrar na porta**. O atomic interno da porta pode, mesmo com (b)=não, manter a escrita e seus receivers na mesma transação. — `app/chamados/services/contexto.py:100`, `app/chamados/services/texto.py:31`, `app/chamados/services/vinculos.py:86`

A notação **R** significa **(e)=sim, condicionalmente pelo receiver**: o status lido do banco difere do status da instância; as guardas permitem julgamento e a fábrica chega à trava de Colaborador/Disputa. A ocorrência é **[nao verificado]**, mas o caminho existe no código. O bloqueio do S143 para saves de campo não elimina esse outro receiver. — `app/chamados/signals.py:100`, `app/chamados/signals.py:180`, `app/chamados/signals.py:188`, `app/chamados/signals.py:193`, `app/chamados/signals.py:199`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530`

**R anterior** significa que uma chamada/iteração anterior completou esse caminho na mesma transação: (c)=sim pela disputa travada/escrita; (d)=sim somente se o Colaborador travado foi o mesmo. Callbacks `on_commit` ficam fora da transação anterior. — `app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:565`, `app/chamados/services/disputa_emissao.py:587`, `app/chamados/signals.py:221`, `app/chamados/signals.py:289`

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/api/views.py:2033` — `api_justificativas` → `apontar_justificativa` | Não | Não; J salva antes, sem atomic envolvendo ambos | Não | **Sim, R**; J pode desatualizar a instância carregada antes. — `app/api/views.py:1986`, `app/api/views.py:2021`, `app/ponto/signals.py:165` |
| `app/api/views.py:3159` — `api_ausencia_documento` → `renovar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/api/views.py:3160` — `api_ausencia_documento` → `reescrever_texto` | Não | Não; atomic da chamada anterior terminou | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/texto.py:31` |
| `app/ponto/views.py:288` — `bater_ponto` → `renovar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/views.py:1648` — `_abrir_justificativa_fluxo` → `apontar_justificativa` | Não | Não; J salva antes, sem atomic envolvendo ambos | Não | **Sim, R**; CC foi carregado antes do signal de J. — `app/ponto/views.py:1615`, `app/ponto/views.py:1633`, `app/ponto/signals.py:165` |
| `app/chamados/views.py:1025` — `atribuir_chamado` → `liberar_responsavel` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/vinculos.py:139` |
| `app/chamados/views.py:1029` — `atribuir_chamado` → `assumir_responsavel` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/vinculos.py:128` |
| `app/chamados/views_arquivar.py:37` — `arquivar_chamado` → `silenciar` | Não | Não | Não | Não na mesma transação; porta sem atomic interno. — `app/chamados/services/arquivo.py:170` |
| `app/chamados/views_arquivar.py:77` — `desarquivar_chamado` → `levantar` | Não | Não; eventual `reabrir` anterior já retornou | Não na mesma transação | Não na mesma transação; ressuscitação posterior também está fora. — `app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:86`, `app/chamados/services/arquivo.py:208` |
| `app/chamados/views_arquivar.py:140` — `arquivar_lote` → `silenciar` | **Sim**, atomic `app/chamados/views_arquivar.py:134` | Não na primeira passagem; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/views_arquivar.py:135` |
| `app/ponto/services/triagem_batida.py:292` — `ramo_regularizar_saida` → `escalar_urgencia` | Não | Não; `update_or_create` de J já terminou | Não | Não na mesma transação. — `app/ponto/services/triagem_batida.py:258`, `app/chamados/models.py:538` |
| `app/ponto/services/triagem_batida.py:369` — `ramo_solicitar_anuencia` → `escalar_urgencia` | **Sim**, atomic `app/ponto/services/triagem_batida.py:368` | Não; primeiro ato do bloco | Não | **Sim, R** |
| `app/ponto/services/triagem_batida.py:371` — `ramo_solicitar_anuencia` → alias `_renovar_contexto` | **Sim**, atomic `app/ponto/services/triagem_batida.py:368` | Não; **sim se R anterior em `app/ponto/services/triagem_batida.py:369`** | Não; sim nesse R anterior | **Sim, R**. — `app/ponto/services/triagem_batida.py:367` |

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:94` — `Command.handle` → `silenciar` | Não | Não | Não | Não; porta sem atomic interno. — `app/chamados/services/arquivo.py:170` |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:288` — `Command._aplicar` → `silenciar` | **Sim**, atomic `app/chamados/management/commands/arquivar_competencia_encerrada.py:285` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/arquivar_prescritos.py:94` — `Command.handle` → `silenciar` | **Sim**, atomic `app/chamados/management/commands/arquivar_prescritos.py:91` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/management/commands/arquivar_prescritos.py:92` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` — `Command.handle` → `renovar_contexto` | **Sim**, atomic por item `app/chamados/management/commands/nomear_marco_por_lampada.py:107` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98` — `Command.handle` → `renovar_contexto` | **Sim**, atomic `app/chamados/management/commands/carimbar_por_batida_coincidente.py:97` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100` — `Command.handle` → `apontar_celula` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/reconciliar_grade.py:454` — `Command._relavrar_orfa` → `retirar_contexto` | Não; antes do atomic posterior | Não | Não | **Sim, R**, sob atomic interno. — `app/ponto/management/commands/reconciliar_grade.py:472`, `app/chamados/services/contexto.py:122` |
| `app/ponto/management/commands/reconciliar_grade.py:473` — `Command._relavrar_orfa` → `renovar_contexto` | **Sim**, atomic `app/ponto/management/commands/reconciliar_grade.py:472` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/reconciliar_grade.py:474` — `Command._relavrar_orfa` → `retirar_contexto` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/ponto/management/commands/reconciliar_grade.py:473`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/curar_residuo_bug115.py:121` — `Command._ambiguo` → `renovar_contexto` | **Sim**, atomic `app/ponto/management/commands/curar_residuo_bug115.py:120` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/processar_alertas_avancados.py:173` — `Command.handle` → `escalar_urgencia` | Não | Não | Não | Não na mesma transação; método sem atomic interno. — `app/chamados/models.py:538` |
| `app/ponto/management/commands/processar_alertas_turno.py:423` — `Command._abrir_justificativa_automatica` → `apontar_justificativa` | Não | Não; atomic de `get_or_create` já terminou | Não | **Sim, R**, sob atomic interno. — `app/ponto/management/commands/processar_alertas_turno.py:391`, `app/chamados/services/vinculos.py:86` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:64` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` — `Command.handle` → `marcar_escalonado_dp` | Não | Não | Não | Não na mesma transação; método sem atomic interno. — `app/chamados/models.py:593` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:132` — `Command.handle` → `escalar_urgencia` | **Sim**, atomic `app/ponto/management/commands/escalonar_chamados_supervisao.py:131` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:138` — `Command.handle` → `anotar_contexto` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/ponto/management/commands/escalonar_chamados_supervisao.py:132`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:163` — `Command.handle` → `anotar_contexto` | Não | Não na mesma transação; anotação da DS anterior já retornou | Não | **Sim, R**. — `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`, `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:179` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:195` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |

Os três sítios seguintes precisam de variantes: são funções também chamadas por outros módulos. **P anterior** abaixo significa ressuscitação ou reconciliação anterior do cartório: UPDATE de PD, save de DS ou trava de DS pela porta do estado. — `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1533`, `app/chamados/models.py:2994`, `app/chamados/models.py:1561`, `app/chamados/models.py:2147`, `app/ponto/services/cartorio.py:1488`, `app/chamados/models.py:694`

| (a) Chamada — função/caminho | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/ponto/management/commands/gerar_celulas.py:255` — `_marcar_cobrancas_orfas`, regeneração/edição isolada | **Sim**, atomic da regeneração/edição | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim nesse R anterior | **Sim, R**; o julgamento normal posterior fica fora do atomic próprio. — `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:354` |
| `app/ponto/management/commands/gerar_celulas.py:255` — primeiro receiver de âncora durante `executar_vinculo` | **Sim**, atomic externo | Não no caminho sem atos anteriores; sim se R/P anterior | Não nesse caminho inicial | **Sim**: receiver posterior chama `remover_motivo`, que trava C e DS; também pode haver julgamento no rabo. — `app/colaboradores/services/vinculo.py:187`, `app/colaboradores/services/vinculo.py:274`, `app/escala/signals.py:117`, `app/escala/signals.py:161`, `app/chamados/models.py:1996`, `app/chamados/models.py:1998`, `app/ponto/portas/celula.py:434` |
| `app/ponto/management/commands/gerar_celulas.py:255` — foto de folga após receiver de âncora/remover motivo | **Sim**, mesmo atomic externo | **Sim**, quando aquele receiver atuou: PD carimbada e DS salva antes | **Sim**, pelo receiver anterior | **Sim/pode**, pelo julgamento posterior. — `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:163`, `app/escala/folgas.py:272`, `app/chamados/models.py:2031`, `app/chamados/models.py:2035`, `app/chamados/models.py:1996`, `app/ponto/portas/celula.py:434` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — `_espelhar_marcos`, comando sem atomic externo | Não | Não na mesma transação | Não na mesma transação | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — via `_abrir_chamado`, sob atomic externo do cartório | **Sim**, herdado | Não sem P anterior; **sim com P anterior** | **Sim**, `Chamado.abrir` anterior | **Sim/pode**, pelo fio posterior ou fábrica. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:744`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/chamados/services/disputa_emissao.py:968`, `app/ponto/services/cartorio.py:997` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — chamada direta do cartório, sob atomic externo | **Sim**, por exemplo o comando de veredito velho | Não sem P anterior; **sim com P anterior** | Não sem trava anterior; **sim** se houve reconciliação ou wrapper que trava C | **Sim/pode**, pela fábrica posterior. — `app/ponto/services/cartorio.py:1789`, `app/ponto/services/cartorio.py:1798`, `app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/services/veredito_celula.py:225`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/disputa_emissao.py:530` |
| `app/ponto/management/commands/detectar_ausencias.py:765` — `_abrir_chamado`, comando sem atomic externo | Não | Não na mesma transação | Não na mesma transação | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/management/commands/detectar_ausencias.py:765` — `_abrir_chamado`, sob atomic externo | **Sim**, herdado | Não sem P anterior; **sim com P anterior** | **Sim**, `Chamado.abrir` anterior | **Sim/pode**, pelo fio/fábrica posterior. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/chamados/services/disputa_emissao.py:968`, `app/ponto/services/cartorio.py:997` |

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/chamados/services/abertura.py:115` — `abrir_chamado_local` → `apontar_ticket_glpi` | Não no caminho da view | Não | Não | **Sim, R**. — `app/chamados/views.py:197`, `app/chamados/services/vinculos.py:112` |
| `app/chamados/services/abertura.py:116` — `abrir_chamado_local` → `renovar_contexto` | Não no caminho da view | Não; transação anterior terminou | Não | **Sim, R**. — `app/chamados/views.py:197`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/adesao.py:112` — `sincronizar_adesao` → `renovar_contexto` | Não no comando chamador | Não na mesma transação | Não; `abrir/reabrir` anteriores já retornaram | **Sim, R**; `reconciliar` seguinte tem transação própria. — `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:74`, `app/chamados/services/adesao.py:92`, `app/chamados/services/adesao.py:115` |
| `app/chamados/services/celula_carimbada.py:109` — `backfill` → `apontar_celula` | Não no corpo; transação de entrada **[nao verificado]** | Não no corpo; contexto externo **[nao verificado]** | Não no corpo; contexto externo **[nao verificado]** | **Sim, R**, se executado; entrada de produção **[nao verificado]**. — `app/chamados/services/celula_carimbada.py:74`, `app/chamados/services/vinculos.py:97` |
| `app/chamados/services/declaracao_texto.py:202` — `declarar_nao_trabalhou` → `renovar_contexto` | Não no caminho da view | Não | Não | **Sim, R**. — `app/chamados/views.py:1066`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/cobranca.py:178` — `cobrar_colaborador` → `definir_prazo_sla` | **Sim**, atomic `app/chamados/services/cobranca.py:176` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/services/cobranca.py:177` |
| `app/chamados/services/disputa_emissao.py:1604` — `recobrar_questionario` → `definir_prazo_sla` | Não no caminho da view | Não | Não | Não na mesma transação. — `app/chamados/views.py:1425`, `app/chamados/models.py:575` |
| `app/chamados/services/silencio_isentos.py:43` — `silenciar` → `arquivo.silenciar` | **Sim**, atomic `app/chamados/services/silencio_isentos.py:41` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/services/silencio_isentos.py:42` |
| `app/chamados/services/silencio_ausencia.py:86` — `reconciliar_por_ausencia` → `silenciar` | Não no comando/callback de entrada | Não | Não | Não na mesma transação. — `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54`, `app/ponto/signals.py:272`, `app/chamados/services/arquivo.py:170` |
| `app/chamados/services/silencio_ausencia.py:72` — closure `_levantar` → `levantar` | Não no comando/callback de entrada | Não na mesma transação; eventual reconciliação anterior já terminou | Não na mesma transação | Não na mesma transação. — `app/chamados/services/silencio_ausencia.py:101`, `app/chamados/services/silencio_ausencia.py:107`, `app/ponto/signals.py:272`, `app/chamados/services/arquivo.py:208` |
| `app/chamados/services/limbo_folgas.py:67` — `aplicar_sugestao` → `renovar_contexto` | Não no comando chamador | Não | Não | **Sim, R**. — `app/ponto/management/commands/detectar_ausencias.py:236`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/limbo_folgas.py:69` — `aplicar_sugestao` → `reescrever_texto` | Não no comando chamador | Não; atomic anterior terminou | Não | **Sim, R**. — `app/ponto/management/commands/detectar_ausencias.py:236`, `app/chamados/services/texto.py:31` |
| `app/chamados/services/limbo_folgas.py:270` — `agrupar_furo`, sem atomic externo | **Sim**, atomic local `app/chamados/services/limbo_folgas.py:269` | Não; primeiro ato do bloco | Não | **Sim**: reapontamento/reconciliação posteriores. — `app/chamados/services/limbo_folgas.py:277`, `app/chamados/services/fio.py:855`, `app/chamados/services/limbo_folgas.py:278`, `app/chamados/models.py:290` |
| `app/chamados/services/limbo_folgas.py:270` — `agrupar_furo`, herdando atomic por `_abrir_chamado` | **Sim**, herdado e local | Não sem P/R anterior; **sim com P anterior ou R anterior de `_espelhar_marcos`** | **Sim**, `Chamado.abrir` anterior | **Sim**, pelos mesmos atos posteriores. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:744`, `app/ponto/management/commands/detectar_ausencias.py:761`, `app/chamados/services/limbo_folgas.py:277` |
| `app/chamados/rejuncao.py:107` — `rejuntar_celula_dos_vivos` → `apontar_celula` | Não no consumidor; regeneração entra por `on_commit` | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/reconciliador.py:1354`, `app/chamados/reconciliador.py:1378`, `app/ponto/portas/celula.py:498`, `app/ponto/portas/celula.py:502`, `app/chamados/services/vinculos.py:97` |
| `app/chamados/services/materializacao.py:715` — `_ressuscitar_por_resposta`, respostas diretas/correção administrativa | Não; atomic da resposta termina antes | Não na mesma transação | Não na mesma transação | Não na mesma transação. — `app/chamados/services/disputa_emissao.py:1388`, `app/chamados/services/disputa_emissao.py:1418`, `app/chamados/views.py:1283`, `app/api/views.py:2659`, `app/chamados/services/acoes_disputa.py:368` |
| `app/chamados/services/materializacao.py:715` — `_ressuscitar_por_resposta`, declarações com atomic externo | **Sim**, atomic da declaração | **Sim**, save anterior de PD | **Sim**, trava anterior da emissão | **Sim, R**; validação de `aplicar

[truncado em 20000 de 29894 caracteres]

## 07/11 20:56 · sessao 00393fa21d8e · - · - · origem=Ronald (tmux)

Concordo: **(e)=sim condicional** é a leitura estrita de “pode travar”, desde que se explicite a condição e não se afirme ocorrência em produção. O `pre_save` lê o status atual do banco, mas o `post_save` compara esse valor com o status da instância; saves de campo não renovam esse status em memória. (`app/chamados/signals.py:180`, `app/chamados/signals.py:188`; `app/chamados/services/contexto.py:102`)

Se houver divergência e as demais guardas permitirem julgamento/emissão, o receiver chega à fábrica e à trava de Colaborador/Disputa; sob atomic interno ou externo, isso ocorre com a escrita de CC ainda na mesma transação. (`app/chamados/signals.py:199`; `app/ponto/services/cartorio.py:997`; `app/chamados/services/disputa_emissao.py:530`; `app/chamados/services/texto.py:31`; `app/chamados/services/contexto.py:100`; `app/chamados/services/vinculos.py:86`)

Correção do meu recorte:

- **e=sim condicional:** `app/api/views.py:2033`, `:3159`, `:3160`; `app/ponto/views.py:288`, `:1648`; `app/chamados/views.py:1025`, `:1029`; `app/ponto/services/triagem_batida.py:369`, `:371`; `app/chamados/views_arquivar.py:140`. A condição é o caminho do receiver acima; os dois últimos blocos têm atomic externo em `app/ponto/services/triagem_batida.py:368` e `app/chamados/views_arquivar.py:134`.
- **e=não na mesma transação:** `app/chamados/views_arquivar.py:37`, `:77` e `app/ponto/services/triagem_batida.py:292`, porque estes caminhos não têm atomic externo nem atomic interno nessas portas. (`app/chamados/services/arquivo.py:170`, `:208`; `app/chamados/models.py:538`)
- **Propagação anterior:** em `app/ponto/services/triagem_batida.py:371`, se o save de `:369` percorreu esse caminho, (c) e (d) passam a sim; ambos estão sob `:368`. No lote `app/chamados/views_arquivar.py:140`, iteração anterior pode igualmente fornecer (c)=sim, e (d)=sim quando for o mesmo colaborador, sob `:134`.

Há inclusive mecanismo local para instância desatualizada nas duas views de justificativa: CC é carregado antes do save de J, cujo receiver julga o dia antes da porta de vínculo; a instância original não é recarregada nesse intervalo. Isso demonstra possibilidade pelo código, sem demonstrar ocorrência. (`app/api/views.py:1986`, `:2021`, `:2033`; `app/ponto/views.py:1615`, `:1633`, `:1648`; `app/ponto/signals.py:165`)

_2818 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
