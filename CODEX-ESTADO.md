# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 09:59** · publicado em **16/11/2026 10:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 09:59 · sessao 02a95393c9a8 · CORTES-F2-F9-1611 · censo · origem=Code

**U9b é inalcançável no fluxo normal desta cópia:** a porta anterior exige `pergunta_viva(p) == True`, e nada entre as duas chamadas altera a instância ou invalida seu memo. (`app/chamados/views.py:1238–1257`; `app/chamados/juizes.py:893–905`)

Correspondência da cópia com o hash informado `ab616d1c7dbb1bfda30e6accb10caeb6331071db`: [não verificado].

1. **LEITORES**

   - Na view, a primeira porta importa `chamados.models.pergunta_viva` como `_viva_porta`, consulta a pergunta e descarta aquela que não está viva; o retorno anterior usa “Pergunta nao encontrada ou disputa ja regularizada.” (`app/chamados/views.py:1238–1246`)
   - U9b importa a mesma função como `_viva` e lê `p.materializada_em`; pelo curto-circuito de `and`, só chama `_viva(p)` quando há carimbo. Sua mensagem está nas duas linhas seguintes indicadas. (`app/chamados/views.py:1256–1262`)
   - `chamados.models.pergunta_viva` é reexportação de `chamados.juizes.pergunta_viva`; o juiz também lê `materializada_em`, tanto na chave do memo quanto na projeção usada pelo cálculo. (`app/chamados/models.py:4224–4226`; `app/chamados/juizes.py:857,1389,1403`)
   - Há **citação explícita do ramo** na allowlist: `materializada AND not pergunta_viva`. Portanto, “ninguém cita o ramo” não corresponde à cópia. (`app/chamados/tests/test_contract_leitores_vitalidade.py:225`)
   - A função irmã `pergunta_sem_lastro_ou_viva` menciona a guarda da porta e lê os mesmos insumos pela expressão inversa: `materializada_em is None or pergunta_viva(p)`. (`app/chamados/juizes.py:1495–1499`)
   - Os dois formulários do template apontam para a view e ficam sob `p.viva`; essa propriedade delega ao juiz. (`app/templates/chamados/partials/_card_atendimento.html:77–78,169`; `app/chamados/models.py:3474–3484`)
   - A rota e os inventários citam a **view**, sem fixar U9b ou sua mensagem. (`app/chamados/urls.py:17`; `app/core/juizes.py:1065`; `app/docs/CENSO_PORTAS.md:23,180`; `app/docs/BACKLOG.md:2387`)
   - Não localizei outra ocorrência literal da mensagem fora da própria view. [não verificado]

2. **ESCRITORES — portas de `PerguntaDisputa.materializada_em`**

   | Porta | Escrita e persistência |
   |---|---|
   | `carimbar` | Aplica o carimbo, consulta o juiz, desfaz se viva e salva se aceito. `app/chamados/models.py:3212,3222–3228,3235` |
   | `carimbar_identidade` | Preserva ou atribui o instante e salva. `app/chamados/models.py:3265–3273` |
   | `descarimbar` | Zera instante, autor e via; salva por padrão, admitindo `salvar=False`. `app/chamados/models.py:3286–3295` |
   | `descarimbar_em_lote` | Zera os três campos por `update`. `app/chamados/models.py:3363–3365` |

   As portas de instância atribuem pelo helper `_escrever_carimbo`; `_desfazer_em_memoria` também o usa para restaurar valores, sem persistir nessa função. (`app/chamados/models.py:3180–3184,3242–3249`)

   O selo AST declara os dois sítios diretos da pergunta — helper e limpeza em lote — e verifica escritores fora dessa lista. A atribuição em `regularizacao_ext.py` pertence ao modelo homônimo de regularização externa. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:43–50,226–236`; `app/chamados/services/regularizacao_ext.py:199–201`)

3. **ALCANÇABILIDADE e mudança entre chamadas**

   - Se a primeira chamada retorna `False`, a view faz `p = None` e retorna antes de U9b. Logo, chegar a U9b implica que a primeira chamada retornou `True`. (`app/chamados/views.py:1242–1246`)
   - Entre essa porta e U9b há comentários e outro import; não há escrita, recarga da pergunta ou chamada a outra porta. As duas avaliações recebem o mesmo `p`, sem argumentos temporais ou carga. (`app/chamados/views.py:1242–1257`)
   - O juiz guarda **a chave posterior ao cálculo** e reutiliza o resultado quando ela coincide. A segunda chamada, nesse trecho, retorna o `True` memoizado; assim, `not _viva(p)` é falso. Uma alteração concorrente no banco não provoca nova consulta do cálculo nesse caminho. (`app/chamados/juizes.py:893–905`)
   - **Em geral, o resultado pode mudar:** mutações nos campos da chave invalidam o memo. Há testes de mudança por `validada_em`, carimbo/via e mutação sem `save`; essas mutações não aparecem entre as portas da view. (`app/chamados/juizes.py:854–861`; `app/chamados/tests/test_selo_memo_pergunta_viva.py:123–126,152–167`; `app/chamados/views.py:1242–1257`)

4. **TESTES EXISTENTES** [nao verificado]

   - A allowlist **cita**, mas não fixa a existência do ramo: a asserção só exige ausência de ofensores fora da lista. Remover U9b não viola essa exigência. (`app/chamados/tests/test_contract_leitores_vitalidade.py:225,319–322`)
   - `test_z1_pergunta_materializada_resposta_bloqueada` verifica preservação de texto/carimbo, mas envia para `/disputa/pergunta/<pk>/responder/`; a rota cadastrada é `/disputa/responder/<pk>/`. O caso não afirma status nem mensagem, portanto não comprova U9b. (`app/chamados/tests/test_chamados_views_s109.py:86–112`; `app/chamados/urls.py:17`)
   - O teste da função irmã fixa o predicado para materializada viva e morta, sem chamar a view. O contrato do template fixa o gate `p.viva`, também sem exigir U9b. (`app/chamados/tests/test_selo_juizes_orfaos.py:62–81`; `app/chamados/tests/test_contract_pergunta_viva.py:88–104`)
   - Não identifiquei teste existente cuja asserção fixe a mensagem ou exija o próprio ramo U9b. [não verificado] [nao verificado]

perguntas de lei: 0

## 16/11 09:58 · sessao 2e110cfbe865 · - · - · origem=Ronald (tmux)

Escrita de `PerguntaDisputa.materializada_em` concentrada em dois sítios: atribuição de instância em `app/chamados/models.py:3182` (`_escrever_carimbo`) e limpeza via `update` em `app/chamados/models.py:3363`/`:3365` (`descarimbar_em_lote`). O selo AST declara exatamente esses sítios para a pergunta e verifica escrita fora da lista em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43` e `:226`.

Portas que persistem o campo:

- `carimbar`: aplica carimbo, consulta `pergunta_viva`, desfaz se viva e salva quando morta (`app/chamados/models.py:3212`, `:3222`, `:3226`, `:3235`).
- `carimbar_identidade`: preserva ou atribui instante e salva (`app/chamados/models.py:3265`, `:3268`, `:3273`).
- `descarimbar`: limpa os três campos e, por padrão, salva; admite `salvar=False` (`app/chamados/models.py:3286`, `:3291`, `:3294`).
- `descarimbar_em_lote`: limpa instante, autor e via conjuntamente (`app/chamados/models.py:3335`, `:3363`).

Chamadores diretos de produção encontrados:

| Porta | Chamadores |
|---|---|
| `carimbar` | `lavrar_sem_turno_aberto` (`app/chamados/reconciliador.py:406`); `retratar_perguntas_absolvidas_por_celula` (`app/chamados/reconciliador.py:596`); `encerrar_fantasma` (`app/chamados/models.py:3386`); `_materializar_ausencia_de_resposta` (`app/chamados/services/materializacao.py:567`); `_materializar_uma` (`app/chamados/services/materializacao.py:1283`); `materializar_pendentes_aprovadas` (`app/chamados/services/materializacao.py:1437`); `_resolver_perguntas_por_chamado_resolvido` (`app/chamados/services/disputa_emissao.py:2007`, `:2081`). |
| `carimbar_identidade` | `remover_motivo` (`app/chamados/models.py:2125`); `fechar_por_admin` (`app/chamados/models.py:3147`); `_materializar_resposta_hora` (`app/chamados/services/materializacao.py:930`); `_carimbar_recusa` (`app/chamados/services/materializacao.py:1105`); `retratar_duplicata` (`app/chamados/services/fio.py:1398`). |
| `descarimbar` | `_universo_da_ressuscitacao`, em memória (`app/chamados/models.py:1553`); `reabrir_fantasma` (`app/chamados/models.py:3397`); `reabrir_por_resposta`, sem salvar (`app/chamados/models.py:3414`); `reabrir_por_competencia` (`app/chamados/models.py:3421`); `desfazer_carimbo_sem_lastro` (`app/chamados/services/relavra_via.py:246`). |
| `descarimbar_em_lote` | `ressuscitar_perguntas` (`app/chamados/models.py:1613`); `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1648`). |

O campo homônimo escrito em `app/chamados/services/regularizacao_ext.py:199` pertence à regularização externa, excluída explicitamente do censo da pergunta pelo selo (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`). A migração cria o campo anulável, sem carga de dados (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21`).

Executei apenas em memória o scanner AST existente; retornou os dois sítios acima para a pergunta. Seu alcance exclui `__dict__` e nomes trazidos de outro módulo/atributo, conforme declaração explícita (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`). Nenhuma execução Django/banco foi feita [não verificado].

## 16/11 09:58 · sessao b46d93639d17 · - · - · origem=Ronald (tmux)

- A mensagem inteira está em `app/chamados/views.py:1261` e `:1262`; não encontrei outra ocorrência literal nas fontes `.py/.html/.md` pesquisadas. [não verificado]
- Há **citação explícita do ramo** na allowlist de `app/chamados/tests/test_contract_leitores_vitalidade.py:225`. Esse teste detecta comparações cruas de carimbo (`:307-314`) e só reprova ofensores **fora** da allowlist (`:319-322`); portanto não exige a existência do ramo, nem sua execução, nem a mensagem (`:225`, `:319-322`).
- Há regra irmã em `app/chamados/juizes.py:1495-1499`: `pergunta_sem_lastro_ou_viva(p)` devolve `materializada_em is None or pergunta_viva(p)`, com docstring remetendo à porta. O teste `app/chamados/tests/test_selo_juizes_orfaos.py:62-81` fixa essa função irmã para materializada viva e materializada morta; não chama a view (`:68-81`).
- `app/chamados/tests/test_chamados_views_s109.py:86-112` declara testar bloqueio da resposta materializada e verifica preservação do texto/carimbo (`:107-112`), mas o POST usa `/chamados/disputa/pergunta/<pk>/responder/` (`:102`), enquanto a rota real é `/chamados/disputa/responder/<pk>/` (`app/chamados/urls.py:17`). Não há asserção de status ou mensagem no caso (`test_chamados_views_s109.py:99-112`); logo ele não comprova U9b. O próprio `test_hx_simetria_toque.py:58-63` registra a correção desse problema de URL manual e impede 404 no seu helper.
- Outros testes da view: `test_hx_simetria_toque.py:66-92` fixa toque/reenvio/escape; `test_responder_web_falta.py:33-42` fixa materialização de falta pelo caminho web; `test_smoke.py:170`, `:181-185` fixa somente resolução de URL. Nenhum desses fixa a mensagem U9b nas asserções citadas. [nao verificado]
- Templates: formulários da rota em `app/templates/chamados/partials/_card_atendimento.html:78` e `:169`, dentro do gate `p.viva` aberto em `:77`; o contrato de template verifica esse gate e sua ordem em `app/chamados/tests/test_contract_pergunta_viva.py:88-104`.
- Outros registros da **view**, sem fixar U9b: inventário em `app/core/juizes.py:1065`; documentação em `app/docs/BACKLOG.md:2387`, `app/docs/CENSO_PORTAS.md:23` e `:180`.

## 16/11 09:37 · sessao d8971b8baaa8 · CORTES-F2-F9-1611 · verificador · origem=Code

Referências **F2**, **F4** e **F6** abaixo designam os respectivos anexos `matriz.md`, com linhas contadas desde a primeira linha de cada conteúdo.

1) **NAO PROCEDE.** “Integrais” e “parciais” já classificam a prova existente; não dependem do resultado dos testes novos. “Ramo sem mordida” corresponde às lacunas dos quatro parciais; U9b é um degrau identificado. (`F2:100–101`; `F6:44`) [nao verificado]

A seleção anterior ao primeiro teste é executável pela regra **mais restritiva**: F2 tem marca no contrato coletivo do núcleo; F4 tem marca no próprio título do contrato de entrada. Aplicar (b) literalmente faz esses candidatos esperarem. O complemento não manda liberar um degrau mediante uma cláusula mais estreita quando o contrato coletivo está marcado. (`complemento1_cortes_f2_f9_1611.txt:2`; `F2:21–29`; `F4:20`) [nao verificado]

2) **NAO PROCEDE.** Marca somente na linha, somente no contrato ou nos dois lugares produz o mesmo encaminhamento: espera F1. Sem mordida no mutante novo, registra “NAO MORDE”, mutante tentado e fila da fatia. Se morder, segue para revisão antes do brief. Nos parciais, os ramos já cobertos ficam fora da tentativa; os ramos sem prova seguem esses mesmos dois encaminhamentos. (`complemento1_cortes_f2_f9_1611.txt:2`; `cortes_f2_f9_1611.txt:10`; `F2:100–101`)

3) **NAO PROCEDE — nenhuma colisão demonstrada.** Registrar uma tentativa sem mordida não a credita como prova nem a admite no brief. A autorização continua limitada a testes próprios e revisados, como permite a regra escrita. (`complemento1_cortes_f2_f9_1611.txt:2`; `cortes_f2_f9_1611.txt:10`; `CLAUDE.md:349–350`; `app/docs/PISTA.md:143–145`)

A ordem “dez integrais antes dos quatro parciais” continua aplicável **depois da espera determinada por (b)**; ela não exige executar dez antes da F1. A exclusão de S1/S2 coincide com a retirada desses degraus do escopo F6. (`complemento1_cortes_f2_f9_1611.txt:2`; `F2:101`; `F6:48–49`)

4) **NAO PROCEDE.** A contagem literal, considerando também as marcas dos contratos coletivos, fica:

| Fatia | Entram na seleção antes da F1 | Esperam F1 | Fora do escopo | Fundamento |
|---|---:|---:|---:|---|
| F2 | **0** | **14** | 0 | `F2:21–29`, `:100–101`; complemento:2 |
| F4 | **0** | **21** | 0 | `F4:20`, `:126–127`; complemento:2 |
| F6 | **1** | **8** | **2** | `F6:33–49`, `:94`; complemento:2 |

**F2 — todos esperam**, preservando a ordem posterior:

- Dez integrais: **M1b** (`F2:37`), **M4** (`F2:40`), **M7** (`F2:43`), **M11** (`F2:47`), **M13** (`F2:49`), **M14** (`F2:50`), **M15** (`F2:51`), **M17** (`F2:53`), **M18** (`F2:54`), **M19** (`F2:55`).
- Depois, quatro parciais, somente nos ramos sem prova: **M3** (`F2:39`), **M9** (`F2:45`), **M10** (`F2:46`), **M20** (`F2:56`). A classificação dez/quatro está expressa em `F2:101`.

**F4 — todos os 21 esperam**, pela marca do contrato de entrada, inclusive os que não repetem a marca na linha. (`F4:20`; `complemento1_cortes_f2_f9_1611.txt:2`)

- **M1** (`F4:34`), **M2** (`F4:35`), **M9b** (`F4:43`), **M13** (`F4:47`), **M16** (`F4:50`), **M17** (`F4:51`), **M19** (`F4:53`), **M23** (`F4:57`), **M24** (`F4:58`).
- **E4** (`F4:67`), **E5** (`F4:68`), **E7** (`F4:70`), **E9** (`F4:72`), **E17** (`F4:80`), **E18** (`F4:81`), **E19** (`F4:82`), **E20** (`F4:83`).
- **X1** (`F4:89`), **X2** (`F4:90`), **J1** (`F4:91`), **J2** (`F4:92`).

**F6:**

- Entra agora: **U9b** (`F6:44`).
- Esperam: **U1b** (`F6:33`), **U1c** (`F6:34`), **U2** (`F6:35`), **U3** (`F6:36`), **U6** (`F6:39`), **U8** (`F6:42`), **U9a** (`F6:43`), **U12** (`F6:47`).
- Fora, sem contar como espera: **S1** (`F6:48`) e **S2** (`F6:49`; `complemento1_cortes_f2_f9_1611.txt:2`).

A elegibilidade de U9b pela marca não comprova ausência de cruzamento com arquivos das obras em voo: **[nao verificado]**. As esperas por cruzamento e pela vaga continuam expressas no item 9. (`cortes_f2_f9_1611.txt:10`)

VEREDITO: todos NAO PROCEDE

## 16/11 09:36 · sessao e0de1a397847 · - · - · origem=Ronald (tmux)

A leitura **F2=0, F4=0, F6=1 agora** é defensável como aplicação literal mais restritiva, sem inventar regra nova. Minha objeção anterior deu peso excessivo à seleção específica por cláusula. (`complemento1_cortes_f2_f9_1611.txt:2`)

**A favor da leitura global:** F2 nomeia expressamente um “Contrato lido pelos degraus (núcleo)” e inclui nele uma marca `[DEPENDE-F1]`; F4 marca o próprio título do “CONTRATO DE ENTRADA”. Como (b) determina esperar quando marcado em qualquer dos dois lugares, não exige provar independência efetiva nem procurar uma cláusula mais estreita para liberar testes. (`anexo F2/matriz.md:21`, `:27–29`; `anexo F4/matriz.md:20`; `complemento1_cortes_f2_f9_1611.txt:2`)

**Contra a leitura global:** F2 separa BASE, c3 e F1 dentro do núcleo, e F4 também enumera dependências por cláusula e por ID; isso permite uma leitura operacional mais específica. Porém, essa leitura liberaria degraus apesar da marca existente no contrato coletivo, contrariando a opção explicitamente mais restritiva. A existência de uma leitura menos restritiva não basta para tornar inexequível a regra que manda escolher a outra. (`anexo F2/matriz.md:22–29`, `:69–77`; `anexo F4/matriz.md:105–110`; `complemento1_cortes_f2_f9_1611.txt:2`)

**Conclusão independente:** eu não sustentaria PROCEDE apenas nessa ambiguidade. Declararia no cabeçalho a aplicação global dos contratos marcados: **F2 espera os 14 candidatos; F4 espera os 21; F6 entra somente U9b, espera oito e exclui S1/S2**. Depois da F1, F2 mantém dez integrais antes dos quatro parciais, estes somente nos ramos sem prova; isso não conflita com a espera anterior. (`anexo F2/matriz.md:100–101`; `anexo F4/matriz.md:126–127`; `anexo F6/matriz.md:33–49`, `:94`; `complemento1_cortes_f2_f9_1611.txt:2`)

## 16/11 09:34 · sessao e0de1a397847 · - · - · origem=Ronald (tmux)

F2, por vínculos expressos dos IDs às marcas:

- Agora: **7 integrais** — M11:47, M13:49, M14:50, M15:51, M17:53, M18:54, M19:55 — depois **M20 parcial:56**, só fora de `estado`. (`anexo F2/matriz.md:101`)
- Esperam: **3 integrais** — M1b:37, M4:40, M7:43 — e **3 parciais** — M3:39, M9:45, M10:46. Os contratos vinculam M1–M4/M8–M10 à dependência e mencionam M7 expressamente. (`anexo F2/matriz.md:74`, `:76`)

F4, por cláusulas específicas e aplicação restritiva da cláusula 3:

- Agora: **11** — M19:53, M24:58, E4:67, E5:68, E7:70, E17:80, E18:81, E19:82, E20:83, X1:89, X2:90. (`anexo F4/matriz.md`, linhas individuais)
- Esperam: **10** — M1:34, M2:35, M9b:43, M13:47, M16:50, M17:51, M23:57, E9:72, J1:91, J2:92. M1/M2/M9b/M13/M16/M17 estão no contrato marcado; M23 também; E9/J1 têm marca na linha; J2 aparece na cláusula marcada que o manda entrar junto ou virar fila. (`anexo F4/matriz.md:105–110`, `:72`, `:91`)
- Se a marca da cláusula 3 for limitada ao trecho anterior ao ponto e vírgula, J2 fica sem dependência: **12 agora/9 esperam**. (`anexo F4/matriz.md:109–110`)

F6:

- Agora: **U9b:44**, total **1**.
- Esperam: **8** — U1b:33, U1c:34, U2:35, U3:36, U6:39, U8:42, U9a:43, U12:47.
- Fora: **S1:48, S2:49**. (`anexo F6/matriz.md`, linhas individuais; `complemento1_cortes_f2_f9_1611.txt:2`)

A ambiguidade concreta restante é o alcance de **CONTRATO**: F4 tem o próprio título do contrato de entrada marcado, antes das cláusulas específicas; F2 tem núcleo contratual marcado sem uma lista fechada de IDs. Aplicar esses contratos globalmente faria **todos os respectivos candidatos esperarem**, produzindo seleção diferente. A operação “linha OU contrato” está definida; a associação de cada contrato aos degraus não está inteiramente fechada. (`anexo F4/matriz.md:20`, `:105–110`; `anexo F2/matriz.md:21–29`; `complemento1_cortes_f2_f9_1611.txt:2`)

Não há motivo para reprovar “integrais”, “parciais” ou “ramo sem mordida”: a matriz já define essas classes antes dos novos testes; o resultado futuro do RED não é necessário para classificá-las. A resposta (a) encaminha qualquer tentativa nova que não mate o mutante, incluindo tentativa num parcial. (`anexo F2/matriz.md:100–101`; `complemento1_cortes_f2_f9_1611.txt:2`)

## 16/11 09:33 · sessao 009e5360b3eb · COMPLEMENTO-1-CORTES-F2-F9-1611 · sonda · origem=Code

(1) **Total de linhas: 53. Total_erros: 0. Col237 está entre as linhas: sim.** [nao verificado]

(2) Empresa corresponde a `empresa_id`. “Sem historico” indica `troca_no_historico_para_o_cadastro: null`, sem afirmar ausência de outros registros históricos. [nao verificado]

| Col | Empresa (ID) | Posto do cadastro (ID — nome) | Posto do vínculo (ID — nome) | Início do vínculo | Troca no histórico para o cadastro | Referência |
|---|---:|---|---|---|---|---|
| col50 | 4 | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 459 — SHOPPING BOULEVARD - PORTARIA — J.A Juliani Eireli | 2026-08-01 | 2026-10-06 | [nao verificado] |
| col51 | 4 | 529 — PALHANO BUSINESS - TORRE 2 - ASG — R. A. de [nome] [nome] - G3 Force | 455 — PALHANO BUSINESS - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-09-20 | 2026-07-10 | [nao verificado] |
| col55 | 2 | 516 — APOENA - VIGIA — R. A. de [nome] [nome] - G3 Force | 2 — APOENA - VIGIA — J.A Juliani Eireli | 2026-07-21 | 2026-07-09 | [nao verificado] |
| col69 | 3 | 644 — NORTE SHOPPING VGT — Juliani Segurança Patrimonial | 11 — CONDOMINIO GOLDEN PARK II — Juliani Segurança Patrimonial | 2026-10-29 | 2026-10-29 | [nao verificado] |
| col112 | 3 | 8 — ARTESANO - VIGILANTE — Juliani Segurança Patrimonial | 474 — FURGÕES IBIPORÃ - CONTROLADOR DE ACESSO — J.A Juliani Eireli | 2026-11-09 | 2026-11-10 | [nao verificado] |
| col128 | 3 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 448 — J.A BASE - SUPERVISOR DE VIGILANTE — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col142 | 4 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 22 — JULIANI SEGURANCA PATRIMONIAL LTDA — Juliani Segurança Patrimonial | 2026-06-01 | 2026-10-22 | [nao verificado] |
| col154 | 3 | 527 — BASE OPERACIONAL - J.A — J.A Juliani Eireli | 3 — CASA VISCARDI S/A COM E IMP (MTZ) — Juliani Segurança Patrimonial | 2026-06-24 | 2026-07-10 | [nao verificado] |
| col174 | 2 | null — null | 612 — ARCOS DOURADO LAT — J.A Juliani Eireli | 2026-08-26 | sem historico | [nao verificado] |
| col191 | 2 | 582 — ARCOS DOURADOS JSP — J.A Juliani Eireli | 590 — ARCOS DOURADOS JSO — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col216 | 2 | 536 — BASE OPERACIONAL — Juliani Segurança Patrimonial | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-07-21 | 2026-11-09 | [nao verificado] |
| col220 | 2 | 602 — ARCOS DOURADOS MOV — J.A Juliani Eireli | 599 — ARCOS DOURADOS DCA — J.A Juliani Eireli | 2026-11-10 | 2026-11-15 | [nao verificado] |
| col225 | 2 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 448 — J.A BASE - SUPERVISOR DE VIGILANTE — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col236 | 2 | 642 — FUNDACAO COPEL MARINGA — J.A Juliani Eireli | 1 — A definir — Confiance Force Ltda | 2026-07-21 | 2026-07-31 | [nao verificado] |
| col237 | 2 | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col263 | 2 | 593 — ARCOS DOURADOS FZD — J.A Juliani Eireli | 586 — ARCOS DOURADO RXV — J.A Juliani Eireli | 2026-11-04 | 2026-11-05 | [nao verificado] |
| col303 | 2 | 414 — PALHANO BUSINESS - TORRE 1 — J.A Juliani Eireli | 412 — LONDRINA NORTE SHOPPING - PORTEIRO — R. A. de [nome] [nome] - G3 Force | 2026-07-21 | 2026-08-28 | [nao verificado] |
| col306 | 2 | 414 — PALHANO BUSINESS - TORRE 1 — J.A Juliani Eireli | 24 — BASE ADMINISTRATIVA — J.A Juliani Eireli | 2026-07-21 | 2026-09-11 | [nao verificado] |
| col341 | 2 | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 457 — SOLAR ALTA VISTA - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col358 | 2 | 642 — FUNDACAO COPEL MARINGA — J.A Juliani Eireli | 446 — FUNDAÇÃO COPEL - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-07-21 | 2026-07-31 | [nao verificado] |
| col389 | 2 | 415 — PALHANO BUSINESS - TORRE 2 - VIGIA — R. A. de [nome] [nome] - G3 Force | 457 — SOLAR ALTA VISTA - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-11-10 | [nao verificado] |
| col398 | 2 | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | 2026-07-13 | [nao verificado] |
| col442 | 2 | 469 — LEON - MRV — J.A Juliani Eireli | 466 — LE MONDE - MRV — J.A Juliani Eireli | 2026-07-21 | 2026-09-07 | [nao verificado] |
| col464 | 2 | 589 — LA ROCHE - MRV — J.A Juliani Eireli | 467 — LANCELOT - MRV — J.A Juliani Eireli | 2026-07-21 | 2026-08-05 | [nao verificado] |
| col468 | 2 | 622 — CONCENTRIX RENDICAO ALMOÇO — J.A Juliani Eireli | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col511 | 2 | null — null | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-08-05 | sem historico | [nao verificado] |
| col556 | 2 | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col584 | 2 | 626 — ARCOS DOURADOS ANG — J.A Juliani Eireli | 590 — ARCOS DOURADOS JSO — J.A Juliani Eireli | 2026-08-28 | 2026-11-10 | [nao verificado] |
| col607 | 2 | 429 — VISCARDI DUQUE DE CAXIAS- SERVIÇOS GERAIS — J.A Juliani Eireli | 430 — VISCARDI GUARANIS — J.A Juliani Eireli | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col608 | 2 | 413 — NORTE SHOPPING J.A - BOMBEIROS — J.A Juliani Eireli | 6 — LONDRINA NORTE ADMINISTRADORA DE SHOPPING - JSP — Juliani Segurança Patrimonial | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col626 | 2 | 595 — SOLAR DI MILANO  PORTARIA — J.A Juliani Eireli | 509 — ASSOCIAÇÃO ARTESANO LONDRINA - PORTARIA — J.A Juliani Eireli | 2026-07-21 | 2026-08-17 | [nao verificado] |
| col627 | 2 | 11 — CONDOMINIO GOLDEN PARK II — Juliani Segurança Patrimonial | 439 — GOLDEN PARK 2 - VIGIA — J.A Juliani Eireli | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col634 | 2 | 591 — ARCOS DOURADOS DPI — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-07-21 | 2026-07-16 | [nao verificado] |
| col642 | 3 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 24 — BASE ADMINISTRATIVA — J.A Juliani Eireli | 2026-10-05 | 2026-10-22 | [nao verificado] |
| col647 | 2 | null — null | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col673 | 2 | 615 — CONCENTRIX M2 — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col717 | 2 | 583 — ARCOS DOURADOS DVH — J.A Juliani Eireli | 582 — ARCOS DOURADOS JSP — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col719 | 2 | 614 — CONCENTRIX M3 — J.A Juliani Eireli | 617 — CONCENTRIX JL2 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col787 | 2 | 555 — CONDOMINIO VILLE CAMPESTRE - MULTI FUNCAO — J.A Juliani Eireli | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-07-02 | [nao verificado] |
| col849 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 549 — ARCOS DOURADOS  MC . CBV — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col859 | 2 | 659 — SALGADOMAC — J.A Juliani Eireli | 415 — PALHANO BUSINESS - TORRE 2 - VIGIA — R. A. de [nome] [nome] - G3 Force | 2026-10-07 | 2026-11-13 | [nao verificado] |
| col861 | 2 | 650 — TUO - EMPREENDIMENTOS — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col865 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-10-01 | 2026-10-16 | [nao verificado] |
| col917 | 2 | 421 — ARCOS DOURADOS - ARC — J.A Juliani Eireli | 609 — ARCOS DOURADO MC — J.A Juliani Eireli | 2026-08-18 | 2026-10-02 | [nao verificado] |
| col920 | 2 | 585 — ARCOS DOURADOS CWB — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-09-01 | 2026-11-06 | [nao verificado] |
| col932 | 2 | 656 — Sunset Dream Residence — J.A Juliani Eireli | 459 — SHOPPING BOULEVARD - PORTARIA — J.A Juliani Eireli | 2026-09-13 | 2026-11-09 | [nao verificado] |
| col933 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 421 — ARCOS DOURADOS - ARC — J.A Juliani Eireli | 2026-09-04 | 2026-11-13 | [nao verificado] |
| col961 | 2 | 433 — VISCARDI ALPHAVILLE — J.A Juliani Eireli | 456 — SOLAR ALTA VISTA - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-10-29 | 2026-11-04 | [nao verificado] |
| col962 | 2 | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 652 — CASA VISCARDI CD — J.A Juliani Eireli | 2026-09-25 | 2026-11-10 | [nao verificado] |
| col988 | 2 | 586 — ARCOS DOURADO RXV — J.A Juliani Eireli | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 2026-11-04 | 2026-11-10 | [nao verificado] |
| col990 | 2 | 430 — VISCARDI GUARANIS — J.A Juliani Eireli | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-10-14 | 2026-11-09 | [nao verificado] |
| col999 | 2 | 659 — SALGADOMAC — J.A Juliani Eireli | 656 — Sunset Dream Residence — J.A Juliani Eireli | 2026-11-09 | 2026-11-12 | [nao verificado] |
| col1021 | 3 | 16 — PACAEMBU CAMPAGNOLE — Juliani Segurança Patrimonial | 531 — PACAEMBU - HAROLDO — Juliani Segurança Patrimonial | 2026-11-03 | 2026-11-12 | [nao verificado] |

(3) **42 têm data de troca para o posto do cadastro; 11 não têm essa data.** [nao verificado]

(4) A saída não responde:

- Qual posto deveria prevalecer nem a causa ou o responsável pela divergência. [nao verificado]
- A data efetiva da alteração do cadastro ou do vínculo; a data apresentada é a informada pelo campo de histórico. [nao verificado]
- O histórico completo ou a inexistência de registros além dos exibidos. [nao verificado]
- O nome da empresa correspondente a cada `empresa_id`. [nao verificado]
- A situação fora do recorte informado ou após o dump de 2026-11-16. [nao verificado]

## 16/11 09:28 · sessao 152e2fd156c8 · E4C-F1-NOVA-POUSO-B1 · verificador · origem=Code

**A comparação c2 × main `ab616d1c7`, as 283 linhas da sonda, `total_erros=0` e a execução da suíte estão [nao verificado].** O código elimina a produção de `REABERTURA` pela ressuscitação, mas os testes não certificam todos os estados nem toda a saída publicada da tarefa (`app/chamados/models.py:1629`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`).

Referências abaixo relativas a `app/`.

**1. PROCEDE — os leitores não atribuem todos o mesmo significado aos campos.**

A ressuscitação entrega os valores novos à porta; quem efetivamente grava é `descarimbar_em_lote`, enquanto as atribuições em `_universo_da_ressuscitacao` são simulação em memória (`chamados/models.py:1553`; `chamados/models.py:1629`; `chamados/models.py:3391`).

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `respondida_em` | Resposta normal/automática e recuperação pela trilha; `carimbar` pode completar o instante; ressuscitar o limpa pela porta (`chamados/services/disputa_emissao.py:1482`, `chamados/services/disputa_emissao.py:1675`, `chamados/services/disputa_emissao.py:1696`, `chamados/models.py:3242`, `chamados/models.py:1629`). | **PROCEDE:** C6 exige instante **e texto**; `estado`, propriedade `respondida`, partição do app e seleção de corrigíveis ainda usam o instante isoladamente (`chamados/juizes.py:1423`; `chamados/models.py:3155`; `chamados/models.py:3664`; `chamados/services/pergunta_no_app.py:81`; `chamados/juizes.py:1739`). |
| `resposta_colab` | Resposta normal/automática; reabertura e ressuscitação zeram pelo lote (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1674`; `chamados/services/disputa_emissao.py:1609`; `chamados/models.py:1630`). | **NAO HA** conflito no significado de texto atual para C6, materializador e apresentação; entretanto, a sequência pode continuar contendo a resposta que a projeção apagou (`chamados/juizes.py:1423`; `chamados/services/materializacao.py:835`; `chamados/models.py:3688`; `chamados/models.py:4101`). |
| `validada_em` | Validação carimba, devolução/nova resposta desfazem, `carimbar` pode completar e ressuscitar limpa (`chamados/services/validacao.py:159`; `chamados/services/validacao.py:224`; `chamados/services/disputa_emissao.py:1494`; `chamados/models.py:3245`; `chamados/models.py:1629`). | **PROCEDE:** C6/estado/materialização leem validação atual; painel e placar contam o mesmo campo apagável como atividade humana histórica (`chamados/juizes.py:1405`; `chamados/models.py:3151`; `chamados/services/materializacao.py:1377`; `chamados/services/painel.py:256`; `chamados/services/verbos.py:263`). |
| `validada_por` | Acompanha os escritores da validação e sua limpeza (`chamados/services/validacao.py:160`; `chamados/services/validacao.py:225`; `chamados/services/disputa_emissao.py:1495`; `chamados/models.py:3246`; `chamados/models.py:1629`). | **PROCEDE:** o auxiliar preserva o autor como assinatura histórica somente havendo `validada_em`; painel/placar e procedência da batida consultam a assinatura atual, que ressuscitar apaga (`chamados/models.py:4102`; `chamados/services/painel.py:261`; `chamados/services/verbos.py:263`; `ponto/services/procedencia_batida.py:72`). |
| `materializada_em` | Atribuição central em `_escrever_carimbo`, usada pelas portas; volta em lote em `descarimbar_em_lote` (`chamados/models.py:3209`; `chamados/models.py:3239`; `chamados/models.py:3318`; `chamados/models.py:3393`). | **PROCEDE:** C6, estado e materializador leem carimbo atual; procedência e marcos da lavra também usam esse instante como informação histórica/de mudança, que pode desaparecer (`chamados/juizes.py:1450`; `chamados/models.py:3153`; `chamados/services/materializacao.py:1378`; `ponto/services/procedencia_batida.py:74`; `chamados/veredito_lavrado.py:202`). |
| `materializada_por` | Mesmo escritor central e porta do lote (`chamados/models.py:3210`; `chamados/models.py:3393`). | **PROCEDE:** limpar o autor é coerente com retirar o carimbo atual, mas a procedência da batida procura nesse campo a autoria histórica; C6 não o inclui entre seus insumos mutáveis (`ponto/services/procedencia_batida.py:72`; `chamados/juizes.py:855`). |
| `via_resolucao` | Escritor central, lote, encerramento por fato e relavra (`chamados/models.py:3211`; `chamados/models.py:3393`; `chamados/models.py:3402`; `chamados/services/relavra_via.py:125`). | **NAO HA** significado novo introduzido pela c2: universo/C6/SQL leem via de encerramento ou identidade, e descarimbar retira a via junto do carimbo (`chamados/models.py:1535`; `chamados/juizes.py:1416`; `chamados/juizes.py:1444`; `chamados/models.py:3483`). |
| Linhas de `DecisaoPergunta` | Ressuscitar chama o auxiliar e entrega suas linhas ao escritor `registrar`, que insere por `bulk_create` (`chamados/models.py:1613`; `chamados/models.py:4100`; `chamados/models.py:4104`; `chamados/models.py:4120`). | **PROCEDE:** a tarefa lê as linhas como decisões que determinam a próxima ação; o histórico considera uma RESPOSTA vigente até outra resposta ou REABERTURA/RECUSA, mesmo depois de a ressuscitação apagar sua projeção (`chamados/services/tarefa_do_dia.py:148`; `chamados/models.py:3674`). |

A abertura também aceita projeção inicial por `**campos` e registra suas linhas de resposta/validação; o `save` verifica a integridade do par (`chamados/models.py:3124`; `chamados/models.py:3129`; `chamados/models.py:3558`).

**2. PROCEDE — o universo alcançável é maior que as duas classes novas dos testes.** [nao verificado]

O filtro exige vínculo ao chamado, materialização presente, texto exatamente vazio — ou SQL `NULL` — e via fora de `VIAS_FORA_DO_FATO`; depois exige marco não provadamente aceso e C6 vivo na cópia limpa (`chamados/models.py:1530`; `chamados/models.py:1548`; `chamados/models.py:1550`; `chamados/models.py:1557`).

| `respondida_em` anterior | `validada_em` anterior | Entrada admitida, satisfeitos os demais guardas |
|---|---|---|
| Nulo | Nulo | Sim: carimbo sem resposta/validação (`chamados/models.py:1530`). |
| Preenchido | Nulo | Sim: legado instante sem texto (`chamados/models.py:1533`; `chamados/models.py:1555`). |
| Nulo | Preenchido | Sim: validação sem instante de resposta (`chamados/models.py:1530`; `chamados/models.py:1554`). |
| Preenchido | Preenchido | Sim: ambos são retirados da cópia antes do C6 (`chamados/models.py:1554`). |

Autores, sequência anterior, disputa aberta/fechada e `conversa` não recebem filtro próprio nesse universo; texto apenas com espaços não entra, e `NULL` está contemplado no filtro embora o campo atual não tenha `null=True` (`chamados/models.py:1530`; `chamados/models.py:2552`).

**NAO HA — produção de novo ato de devolução pela ressuscitação c2:** ela preserva projeções sem linha e chama diretamente a porta do lote (`chamados/models.py:1613`; `chamados/models.py:1629`).

Depois da operação:

| Leitor | Resultado |
|---|---|
| `tarefa_do_dia` | Sem decisão: `aguarda_resposta`; RESPOSTA vazia: `respondida_sem_texto`; RESPOSTA com texto: `aguarda_validacao`; RECUSA/REABERTURA anterior: `devolvida_ao_colaborador`; SILENCIO: nenhuma ação (`chamados/services/tarefa_do_dia.py:145`). |
| `tarefa_do_dia`, demais decisões | VALIDACAO depende da lavra/efeito; ABSORCAO depende do estado da ausência; a carga só considera decisões até `agora` (`chamados/services/tarefa_do_dia.py:156`; `chamados/services/tarefa_do_dia.py:167`; `chamados/services/tarefa_do_dia.py:412`). |
| `pergunta_viva` | Sob os mesmos insumos usados na seleção, devolve vivo; isso não é garantia incondicional após concorrência ou quando a célula projetada do julgamento difere da persistida (`chamados/models.py:1548`; `chamados/models.py:1557`; `chamados/juizes.py:415`; `chamados/juizes.py:1403`). |
| `perguntas_do_app` | Inclui a pergunta se passar pelo SQL e C6, independentemente de SILENCIO na sequência; com resposta nula e disputa aberta, a partição a chama de pendente (`chamados/services/pergunta_no_app.py:52`; `chamados/services/pergunta_no_app.py:61`; `chamados/services/pergunta_no_app.py:81`). |
| `medir` | Usa os destinos de todas as vivas como `no_app`, retirando sua causa de retenção; não mede a ausência de ação determinada por SILENCIO na tarefa (`chamados/services/pergunta_no_app.py:208`; `chamados/services/pergunta_no_app.py:141`; `chamados/services/tarefa_do_dia.py:165`). |

**PROCEDE — antes/depois não é invariância geral:** uma projeção legada sem linha pode sair de `passivo_sem_linha` para uma ação derivada da RESPOSTA/VALIDACAO que a operação acabou de registrar (`chamados/services/tarefa_do_dia.py:143`; `chamados/services/tarefa_do_dia.py:419`; `chamados/models.py:1613`).

**Comparação com main:** na entrada estável admitida pelo esquema atual, limpar `resposta_colab` regrava texto já vazio; igualdade das demais operações e inexistência de qualquer divergência com `ab616d1c7` permanecem **[nao verificado]** (`chamados/models.py:1533`; `chamados/models.py:1630`).

**3. PROCEDE — há preservação condicional da resposta, mas não uma foto completa de tudo que é retirado.**

- **NAO HA perda demonstrada do anterior quando existe instante sem linha correspondente:** RESPOSTA conserva instante/texto, e VALIDACAO conserva instante/texto/autor, antes da limpeza e no mesmo `atomic` (`chamados/models.py:1607`; `chamados/models.py:1613`; `chamados/models.py:4098`).
- **PROCEDE — entrada sem ambos os instantes não produz decisão:** o auxiliar retorna vazio, mas o lote apaga materialização, autor e via; a auditoria posterior guarda IDs e rótulos de estado, sem os valores anteriores desses carimbos (`chamados/models.py:4091`; `chamados/models.py:3393`; `chamados/models.py:1653`).
- **PROCEDE — autor órfão de validação também pode sumir sem foto:** o universo não o recusa, o auxiliar só o registra quando existe `validada_em`, e o lote o limpa (`chamados/models.py:1530`; `chamados/models.py:4102`; `chamados/models.py:1629`).
- **PROCEDE — a garantia depende da coerência anterior da sequência:** correspondência significa apenas pergunta+tipo+instante, sem conferir texto ou autor; uma linha correspondente com conteúdo divergente impede nova preservação (`chamados/models.py:4094`; `chamados/models.py:4098`; `chamados/models.py:4102`).
- **PROCEDE — na corrida com resposta chegando antes da trava, a trilha bruta pode conservar o texto sem apresentá-lo como anterior:** a c2 limpa a projeção, mas o leitor da tela só encerra a resposta vigente diante de outra RESPOSTA ou ato com foto (`chamados/models.py:1613`; `chamados/models.py:1629`; `chamados/models.py:3674`).

A ocorrência desses estados em produção está **[nao verificado]**; no estado sem resposta/validação, o texto anterior já é vazio, portanto a falta de foto dos carimbos não demonstra, por si, perda de texto exigido pela L-130 (`chamados/models.py:1533`; `docs/LEIS.md:151`).

**4. NAO HA — a porta do lote não adquire as travas; ela exige a trava do colaborador.**

- Recusa kwargs que tentem controlar os próprios campos do carimbo e recusa par **futuro** ilegal; os `outros` desta chamada, `(None, '')`, formam par legal mesmo sobre legado ilegal (`chamados/models.py:3365`; `chamados/models.py:3370`; `chamados/services/fio.py:1605`; `chamados/models.py:1629`).
- Exige colaboradores das disputas registrados como travados, exclui vias fora do fato e executa o `update`; não repete C6 nem o guarda do marco (`chamados/models.py:3385`; `chamados/models.py:3391`).
- A ordem é universo/disputas antes do `atomic`, depois trava global, preservação/`registrar`, captura do evento, limpeza, evento e reabertura das disputas fechadas (`chamados/models.py:1593`; `chamados/models.py:1607`; `chamados/models.py:1612`; `chamados/models.py:1613`; `chamados/models.py:1628`; `chamados/models.py:1638`).
- A trava global segue Colaborador → Disputa → Chamado → Pergunta, por PK; `registrar` insere e emite evento sem adquirir essa ordem por conta própria (`chamados/models.py:2771`; `chamados/models.py:2780`; `chamados/models.py:2793`; `chamados/models.py:2797`; `chamados/models.py:4119`).

**5. PROCEDE — os testes mordem a volta de REABERTURA, com limites claros.** [nao verificado]

Abaixo, “pega” é avaliação estática das asserções; execução efetiva dos mutantes está **[nao verificado]**.

| Teste novo/alterado | Mutante que pega | Mutante que não pega | [nao verificado]
|---|---|---|
| `test_RED_ressuscitar_zera_o_par_com_o_anterior_na_trilha_sem_REABERTURA` | Reintroduzir REABERTURA ou retirar o registro do legado contraria as asserções de atos/trilha (`chamados/tests/test_f1nova_b1_juiz_respondida.py:281`). | Fazer o auxiliar sempre gravar `ato_texto=''`, perdendo texto chegado na corrida: a fixture e a expectativa já são vazias (`chamados/tests/test_f1nova_b1_juiz_respondida.py:264`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:285`). |
| `test_RED_ressuscitar_sem_decisao_nao_muda_a_tarefa_do_dia` | Recolocar `reabrir_para_correcao` muda a razão para devolução (`chamados/tests/test_f1nova_b1_juiz_respondida.py:331`; `chamados/services/tarefa_do_dia.py:152`). | Deixar de limpar `respondida_em` de legado: nesta fixture ele já é nulo (`chamados/tests/test_f1nova_b1_juiz_respondida.py:304`). |
| `test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador` | Reintroduzir REABERTURA substitui a leitura de SILENCIO pela devolução (`chamados/tests/test_f1nova_b1_juiz_respondida.py:334`; `chamados/services/tarefa_do_dia.py:152`; `chamados/services/tarefa_do_dia.py:165`). | Omitir o evento posterior à limpeza: as asserções comparam estado e leitura direta, não entrega do evento (`chamados/tests/test_f1nova_b1_juiz_respondida.py:325`). |
| `test_MORDE_2a_chamada_nao_dobra_o_ato` | Remover o filtro de materialização permite nova ressuscitação na segunda chamada (`chamados/models.py:1531`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:342`). | Criar REABERTURA somente na primeira chamada: a quantidade inicial é capturada livremente em `n1` (`chamados/tests/test_f1nova_b1_juiz_respondida.py:341`). |
| `test_a_arvore_real_tem_sitio_relacionado_e_o_reu_entre_eles` | Trocar a porta por `alvo.update(...)` perde a chamada exigida e reintroduz escrita relacionada (`core/tests/test_censo_escritas.py:206`; `core/tests/test_censo_escritas.py:217`). | Deixar chamada à porta em ramo morto e escrever diretamente por `PerguntaDisputa.objects...update`: o teste verifica presença sintática, sem fluxo de execução (`core/tests/test_censo_escritas.py:216`; `core/tests/test_censo_escritas.py:220`). |
| `test_MORDE_a_arvore_real_ainda_atribui_o_update_pelo_gerente_reverso` | Retirar `resposta_colab=''` da chamada faz desaparecer a âncora literal (`core/tests/test_censo_escritas.py:241`). | Inserir REABERTURA mantendo a chamada literal: suas verificações continuam limitadas à âncora e à inferência do varredor (`core/tests/test_censo_escritas.py:243`; `core/tests/test_censo_escritas.py:251`). |

**PROCEDE — a comparação chamada “fila” é parcial:** ela retorna somente razão/ações de uma instância, enquanto `fio`, `rota`, `fila` e `verbos` são calculados separadamente e dependem também das pendentes do app (`chamados/tests/test_f1nova_b1_juiz_respondida.py:311`; `chamados/services/tarefa_do_dia.py:610`; `chamados/services/tarefa_do_dia.py:635`).

**6. PROCEDE — existem chamadores e leitores externos ao diff afetados pelo protocolo de escrita.**

- Reabrir chamado, desarquivar e cartório chamam diretamente a porta; recebem a ressuscitação sem o ato REABERTURA da B1 anterior (`chamados/models.py:1498`; `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`; `chamados/models.py:1623`).
- O cartório usa o retorno para contar ressuscitações e pedir rejulgamento; a c2 conserva esse protocolo de retorno (`ponto/services/cartorio.py:1692`; `chamados/models.py:1667`).
- O histórico da tela depende dos atos com foto e muda quando eles deixam de existir; esse efeito é particularmente relevante para resposta chegada na corrida (`chamados/models.py:3680`; `chamados/services/disputa_emissao.py:2119`).
- Materialização, validações históricas, procedência e marcos da lavra dependem dos campos apagados; atribuir essas consequências exclusivamente à c2, frente ao main, está **[nao verificado]** (`chamados/services/materializacao.py:1375`; `chamados/services/painel.py:261`; `ponto/services/procedencia_batida.py:72`; `chamados/veredito_lavrado.py:202`).

**7. Checklist das classes.**

| Classe | Achado |
|---|---|
| Segundo juiz duplicando registrado | **NAO HA no trecho c2:** seleção delega ao C6 e integridade delega aos predicados únicos (`chamados/models.py:1557`; `chamados/models.py:3372`; `chamados/services/fio.py:1603`). |
| Segundo escritor fora da porta | **NAO HA no trecho c2:** ressuscitar entrega a limpeza à porta do lote (`chamados/models.py:1629`; `chamados/models.py:3391`). |
| Relógio fora da borda | **PROCEDE, preexistente no caminho:** o universo chama C6 sem instante, e o fallback da janela lê `date.today()` internamente (`chamados/models.py:1557`; `chamados/juizes.py:1471`; `chamados/juizes.py:596`). |
| Derivação paralela de fonte única | **NAO HA nova no trecho c2:** universo, preservação e integridade reutilizam as respectivas autoridades (`chamados/models.py:1593`; `chamados/models.py:1613`; `chamados/models.py:3372`). |
| Selo/RED sem mordida | **NAO HA quanto à volta de REABERTURA:** há proibição direta e comparação com SILENCIO; cobertura integral de fila/trilha permanece insuficiente (`chamados/tests/test_f1nova_b1_juiz_respondida.py:281`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:334`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:311`). |
| Ordem de trava | **NAO HA inversão nova demonstrada:** a ordem global antecede preservação e limpeza (`chamados/models.py:1612`; `chamados/models.py:1613`; `chamados/models.py:1629`). |
| Pouso antes do pré-requisito | **[nao verificado]:** os anexos estabelecem os pré-requisitos, mas não fornecem sua execução nem prova do pouso. | [nao verificado]
| Campo com dois significados | **PROCEDE, preexistente:** instante isolado ainda significa “respondida” em alguns leitores, enquanto C6 exige instante+texto; adicionalmente, a sequência pode tratar como vigente a resposta já retirada da projeção (`chamados/models.py:3664`; `chamados/juizes.py:1423`; `chamados/models.py:3674`; `chamados/models.py:1629`). |

## 16/11 09:25 · sessao cf0625b825b4 · - · - · origem=Ronald (tmux)

Auditoria estática concluída e enviada com referências.

- A porta valida o par futuro, exige colaborador travado e escreve sem adquirir novas travas; o chamador trava antes de registrar e zerar. `app/chamados/models.py:1612`, `app/chamados/models.py:3370`, `app/chamados/models.py:3385`.
- Os testes detectam a volta de REABERTURA, mas não provam preservação de texto na corrida; todas as fixtures alteradas têm texto vazio. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:264`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:304`.
- O teste do censo exige a chamada literal; inserir REABERTURA mantendo essa chamada escapa dele individualmente. `app/core/tests/test_censo_escritas.py:241`, `app/core/tests/test_censo_escritas.py:252`.
- Execução dos testes, sonda comparativa e pré-requisitos do pouso: [nao verificado]. [nao verificado]

## 16/11 09:25 · sessao cd853badbde5 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **PROCEDE — histórico da tela fica sem ato de retirada.** `respostas_anteriores()` considera uma RESPOSTA vigente até outra RESPOSTA ou ato REABERTURA/RECUSA; VALIDACAO não a encerra. Portanto, numa corrida com resposta chegando antes da trava e sendo apagada pela ressuscitação, a linha RESPOSTA permanece na trilha bruta, mas não aparece como resposta anterior na tela. `app/chamados/models.py:3674`, `app/chamados/models.py:3680`; delegação em `app/chamados/services/disputa_emissao.py:2119` e carregamento em `app/chamados/services/fio.py:258`.

- **PROCEDE — há limpeza sem registro dos valores anteriores por pergunta.** Entrada com `materializada_em != None`, texto vazio e ambos os instantes de resposta/validação nulos passa pelo filtro inicial; o helper devolve `[]`, e a porta apaga instante, autor e via da materialização. A auditoria final registra somente IDs e rótulos de estado. `app/chamados/models.py:1530`, `app/chamados/models.py:4091`, `app/chamados/models.py:3391`, `app/chamados/models.py:1653`. Isso comprova perda da foto do carimbo; nessa entrada o texto anterior já é vazio. `app/chamados/models.py:1533`.

- **PROCEDE — leitor histórico de atividade usa projeção apagável.** Painel chama `validada_em/validada_por` de atividade humana e conta pela projeção; `verbos` também conta validações e respostas pelos campos atuais, e procedência da batida usa esses autores/instantes. Ressuscitar pode apagar esses fatos dos respectivos resultados apesar de conservar linhas R/V. `app/chamados/services/painel.py:256`, `app/chamados/services/verbos.py:263`, `app/chamados/services/verbos.py:333`, `app/ponto/services/procedencia_batida.py:70`; limpeza em `app/chamados/models.py:1629` e `app/chamados/models.py:3393`. Exclusividade dessa consequência em relação ao main: [nao verificado].

- **PROCEDE — significados de “respondida” ainda coexistem nos leitores.** C6 exige instante **e** texto; `estado`, propriedade `respondida` e partição do app usam apenas instante. O helper histórico também produz RESPOSTA com texto vazio quando há instante, e tarefa distingue isso como `respondida_sem_texto`. `app/chamados/juizes.py:1421`, `app/chamados/models.py:3154`, `app/chamados/models.py:3664`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/models.py:4098`, `app/chamados/services/tarefa_do_dia.py:148`.

Mapa dos escritores efetivos:

| Campo | Escritores na cópia |
|---|---|
| `respondida_em` | resposta normal, automática e recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1482`, `:1675`, `:1696`; carimbo/restauro: `app/chamados/models.py:3242`, `:3273`; limpeza pelo lote: `app/chamados/models.py:1629`, `:3393`. |
| `resposta_colab` | resposta normal/automática: `app/chamados/services/disputa_emissao.py:1481`, `:1674`; reabertura/lote: `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1630`, `:3393`. |
| `validada_em`, `validada_por` | validação/devolução: `app/chamados/services/validacao.py:159`, `:224`; nova resposta desfaz: `app/chamados/services/disputa_emissao.py:1494`; carimbo/restauro: `app/chamados/models.py:3245`, `:3274`; limpeza do lote: `app/chamados/models.py:1629`, `:3393`. |
| `materializada_em`, `materializada_por` | atribuição central: `app/chamados/models.py:3209`; portas que a chamam: `:3239`, `:3293`, `:3318`; lote: `:3393`. |
| `via_resolucao` | trio central e lote acima; alteração isolada por fato no espelho: `app/chamados/models.py:3402`; relavra: `app/chamados/services/relavra_via.py:125`. |
| `DecisaoPergunta` | escritor `registrar`, inserção em `app/chamados/models.py:4120`; ressuscitar entrega as linhas em `:1613`; helper preenche ato/instante/texto/autor em `:4100` e `:4104`. |

Inserção por `PerguntaDisputa.abrir(**campos)` também recebe projeção inicial e registra R/V; o `save` aplica a recusa do par. `app/chamados/models.py:3124`, `app/chamados/models.py:3129`, `app/chamados/models.py:3558`.

Outros leitores relevantes:

- C6 lê validação, par e carimbo/via como estado atual: `app/chamados/juizes.py:1389`, `:1405`, `:1416`, `:1423`, `:1444`, `:1450`.
- App seleciona por C6 e depois separa pendentes pelo instante/disputa aberta: `app/chamados/services/pergunta_no_app.py:52`, `:61`, `:81`; `medir` usa os destinos das vivas: `:208`.
- Tarefa lê sequência por `ato_em,pk`, passivo pelo helper e efeitos pela lavra: `app/chamados/services/tarefa_do_dia.py:410`, `:419`, `:424`; RESPOSTA pede validação, REABERTURA/RECUSA devolvem ao colaborador, SILENCIO encerra: `:148`, `:152`, `:165`.
- Materializador seleciona instantes de resposta/validação com materialização nula: `app/chamados/services/materializacao.py:1376`; usa texto como anuência: `:835`.
- Cache da lavra usa máximos dos carimbos da pergunta como marcos de mudança: `app/chamados/veredito_lavrado.py:202`.
- Respostas corrigíveis selecionam pelo instante de resposta: `app/chamados/juizes.py:1739`; fila global de validar usa par com texto e C6: `:1746`.

Chamadores fora do diff: reabertura do chamado em `app/chamados/models.py:1498`, desarquivamento em `app/chamados/views_arquivar.py:86` e cartório em `app/ponto/services/cartorio.py:1673`.

## 16/11 09:24 · sessao d3593eac2ade · - · - · origem=Ronald (tmux)

2. **PROCEDE — universo mais amplo que as duas fixtures novas.** A seleção exige pergunta que resolve o chamado, `materializada_em != None`, texto SQL `''` ou `NULL`, via diferente de `duplicata`/`motivo_retratado`; depois pula marco provadamente aceso e exige C6 vivo na cópia descarimbada. Não restringe `respondida_em`, `validada_em`, autores, última decisão, `conversa` nem disputa fechada. Portanto os eixos de entrada são resposta com/sem instante × validação com/sem instante × qualquer sequência/último ato × disputa aberta/fechada, desde que passem os dois guardas. `resposta_colab` não admite NULL no esquema corrente, embora o filtro o contemple. (`app/chamados/models.py:1530`, `:1548`, `:1550`, `:1553`, `:1557`, `:2498`, `:2552`.)

2. **NAO HA — ato novo de devolução no caminho c2.** Ressuscitar registra apenas projeções ainda sem linha e chama diretamente a porta que zera o par, validação e carimbo; não chama `reabrir_para_correcao`. (`app/chamados/models.py:1612`, `:1613`, `:1628`, `:1629`.)

2. **PROCEDE — resultado da tarefa depende da sequência anterior.** Sem decisão: `aguarda_resposta`/responder-colaborador; RESPOSTA vazia: `respondida_sem_texto`/responder-colaborador; RESPOSTA com texto: `aguarda_validacao`/validar-admin; RECUSA/REABERTURA prévia: `devolvida_ao_colaborador`; SILENCIO: `silencio_decidido`/nenhuma ação; VALIDACAO: resultado depende da lavra/efeito; ABSORCAO: depende do status da ausência. A sequência é cortada por `ato_em <= agora`. (`app/chamados/services/tarefa_do_dia.py:143`, `:145`, `:148`, `:152`, `:156`, `:165`, `:167`, `:410`.)

2. **PROCEDE — antes/depois não é invariância geral desta porta.** Projeção legada com instante e sem linha é `passivo_sem_linha` antes; a porta cria RESPOSTA/VALIDACAO, apaga a projeção e pode passar a outra razão depois. A mudança pode ocorrer mesmo sem REABERTURA. (`app/chamados/services/tarefa_do_dia.py:143`, `:419`; `app/chamados/models.py:4091`, `:4098`, `:4102`, `:1613`, `:1629`.) Isto é comparação antes/depois na c2; igualdade/diferença factual com `ab616d1c7` permanece **[nao verificado]**, porque a árvore não contém histórico Git nem cópia identificada desse main.

2. **PROCEDE — app e medidor ignoram a decisão de SILENCIO.** `perguntas_do_app` usa `q_vivas` seguido de C6; `particao_do_app` chama de pendente a viva com `respondida_em=None` e disputa aberta; `medir` transforma todas as vivas em destinos `no_app`, e esses destinos não recebem causa de retenção. Portanto SILENCIO pode continuar sem ação na tarefa e a pergunta constar viva/pendente no app. (`app/chamados/services/pergunta_no_app.py:52`, `:61`, `:81`, `:141`, `:208`, `:209`; `app/chamados/services/tarefa_do_dia.py:165`.)

2. **PROCEDE — C6 pós não é garantido sem congelar os insumos externos.** A seleção pergunta ao C6 sobre a célula projetada do julgamento; a leitura posterior ordinária pode usar a célula persistida anterior à lavra. O cartório explicitamente pede novo julgamento depois da ressuscitação. Sob os mesmos insumos da cópia, par/carimbo/validação zerados levam ao resultado vivo aprovado pela própria seleção. (`app/chamados/models.py:1548`, `:1553`, `:1557`; `app/chamados/juizes.py:415`, `:428`, `:1405`, `:1454`, `:1482`; `app/ponto/services/cartorio.py:1693`.)

5. **PROCEDE — comparação dos testes novos não cobre o dono publicado inteiro.** `_portas_da_tarefa` só dá porta de colaborador quando há RESPONDER e a pergunta está no conjunto de pendentes do app; calcula depois `fio`/`rota`. Uma fixture legada com instante pode conservar a razão/ação mas alterar essa disponibilidade depois que o instante é zerado. Os testes novos verificam apenas a razão e ações de uma instância, omitindo `fio`, `rota`, `fila`, `verbos`. (`app/chamados/services/tarefa_do_dia.py:610`, `:616`, `:622`, `:635`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320`.)

6. **PROCEDE — três chamadas diretas de produção fora do trecho alterado.** `ChamadoColaborador.reabrir` ressuscita depois de gravar o estado do chamado; desarquivar pela tranca chama novamente depois de escrever o toque humano; o cartório ressuscita e conta/pede rejulgamento quando houve mudança. Todos passam a receber o protocolo c2 sem REABERTURA, em lugar do protocolo B1 anterior mostrado no anexo. (`app/chamados/models.py:1489`, `:1498`; `app/chamados/views_arquivar.py:77`, `:86`; `app/ponto/services/cartorio.py:1673`, `:1692`.)

6. **NAO HA — novos filtros paralelos nos consumidores enumerados.** A carga de reabrir usa `perguntas_a_ressuscitar`; o comando de órfãs também usa essa porta para recusar ressuscitação de irmãs. O universo efetivo continua centralizado em `_universo_da_ressuscitacao`. (`app/chamados/services/disputa_emissao.py:1820`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:107`; `app/chamados/models.py:1507`.)

## 16/11 09:00 · sessao 8b0372b78864 · CORTES-F2-F9-1611 · verificador · origem=Code

**AVAL-DEVOLVIDO: CORTES-F2-F9-1611 | somente item 9 | perguntas 2 e 4.** (`cortes_f2_f9_1611.txt:11`; `revisao_do_aval.txt:6`)

Referências aos anexos numeradas desde a primeira linha de cada conteúdo; F2/F4/F6 distinguem os três `matriz.md`.

1) **NAO PROCEDE.** O wrapper **não oferece modo/flag para escrever RED fora do papel construtor**: o cabeçalho exclui essa via e o parser oferece apenas `--sonda`, `--anexo` e `--construtor`. A permissão normativa existe, mas não está implementada como modo separado nesse wrapper. (`CLAUDE.md:349–350`; `bin/codex_pergunta.sh:30–33`, `:58–66`)

A via disponível é `CODEX_PAPEL=construtor` com `--construtor <wt>`; exige `logs/construtor.vaga`, por `flock -n`, e recusa vaga ocupada. Os demais papéis executam em `read-only`. **Isso cabe no “senão” já escrito:** o item 9 diz “fora da vaga”, mas permite entrar atrás da espinha quando o wrapper a exigir. Portanto, essa limitação não torna o item inexequível. (`bin/codex_pergunta.sh:197–200`, `:248–253`, `:418–426`; `cortes_f2_f9_1611.txt:10`)

O contrato deve anteceder o primeiro uso; a existência do contrato específico desta obra está **[nao verificado]**. (`app/docs/PISTA.md:143–145`, `:170–175`)

2) **PROCEDE.** O item exige mordida, mas não determina o encaminhamento quando o teste não derruba o mutante; também não determina o encaminhamento de dependência cuja marca seja ambígua ou de teste inviável sem alterar fixture fora de `tests/`. A necessidade concreta dessa última alteração está **[nao verificado]**. Os “senões” expressos cobrem cruzamento de arquivos e exigência da vaga. (`cortes_f2_f9_1611.txt:10`) [nao verificado]

A ambiguidade de dependência é concreta: F4 M23 não tem marca na linha, mas seu contrato declara `[DEPENDE-F1]`; F2 também concentra dependências em seção separada. Falta dizer como classificar esses degraus antes da F1. (`anexo F4/matriz.md:57`, `:107`; `anexo F2/matriz.md:69–77`)

3) **NAO PROCEDE — nenhuma colisão demonstrada.** A regra permite RED próprio sob contrato e revisão; o item preserva a espera quando cruzar arquivos, inclusive testes. A autorização posterior, específica para **só testes**, não constitui automaticamente colisão com o veto anterior à construção das fatias. (`CLAUDE.md:309–310`, `:349–350`; `anexo F4/matriz.md:4–5`; `cortes_f2_f9_1611.txt:10`)

O complemento BUG-PAR mantém sua construção aguardando F1 e autoriza somente ensaio de leitura; o item 9 não libera essa construção. (`complemento2_prepara_volta_bug_par.txt:1–2`; `cortes_f2_f9_1611.txt:10`)

Ausência de cruzamento com as árvores atuais F1 B1 c2, R3, BUG-PAR e LOTE7/LOTE3 c7: **[nao verificado]**. O registro disponível dos lotes descreve c6. (`app/docs/RELATO.md:7`, `:12–14`)

As matrizes históricas também precisam respeitar a L-130 vigente; isso não demonstra colisão de um teste ainda não escrito, pois entrada ilegal pode testar recusa. (`app/docs/LEIS.md:151`)

4) **PROCEDE.** As matrizes identificam degraus sem prova, mas a seleção literal não é inequívoca: F2 distingue integrais, parciais e coluna BASE; F4 declara dependências também no contrato; F6 inclui S1/S2 na lista sem prova apesar de retirá-los do escopo. (`anexo F2/matriz.md:100–102`; `anexo F4/matriz.md:105–110`; `anexo F6/matriz.md:48–49`, `:94`)

Contando **degraus**, pela marca explícita **na própria linha**:

| Universo | PROVA=NENHUM | Com marca | Sem marca | Fonte |
|---|---:|---:|---:|---|
| F2, integrais da coluna c3 | 10 | 0 | **10** | `anexo F2/matriz.md:35–56`, `:101` |
| F2, incluindo quatro parciais | 14 | 0 | **14** | `anexo F2/matriz.md:100–101` |
| F4 | 21 | 3 | **18** | `anexo F4/matriz.md:43`, `:72`, `:91`, `:127` |
| F6 | 11 | 8 | **3** | `anexo F6/matriz.md:32–49`, `:94` |

Na F4, os três marcados sem prova são **M9b, E9 e J1**. Na F6, os três sem marca são **U9b, S1 e S2**; excluir S1/S2 deixaria **um**, mas o item 9 não explicita essa exclusão. Ausência da marca na linha não comprova independência da F1. (`anexo F4/matriz.md:43`, `:72`, `:91`, `:107`; `anexo F6/matriz.md:44`, `:48–49`; `cortes_f2_f9_1611.txt:10`)

Existência e conteúdo das árvores históricas e dos caminhos externos de logs, além do material anexado: **[nao verificado]**.

VEREDITO: PROCEDE em 2 e 4.

## 16/11 09:00 · sessao 1cdcad368529 · LOTE7-C7 · verificador · origem=Code

**Por leitura, o teste novo cobre a falha intermediária que faltava; não identifiquei defeito novo no delta.** Ele observa duas tentativas de auditoria, zero rejeições remanescentes e contagem de trilhas restaurada após a exceção. Execuções, mutantes, logs, produção e identidade histórica da árvore: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:277`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:281`)

1. **Ordem das chamadas e contagem de auditorias**

   A sequência atual é: abrir `atomic`, carregar as ausências por **PK**, salvar a primeira como rejeitada, executar sua auditoria real, salvar a segunda como rejeitada e tentar sua auditoria. O mock devolve `None` nessa segunda tentativa; o serviço levanta dentro do `atomic`, desfazendo as duas rejeições e a primeira trilha. “Primeira” significa primeira PK selecionada; o teste não depende de identificar determinada data. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1490`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`)

   O alias `_ev_rej` é importado dentro da função, portanto recebe o mock ativo. `chamadas` aumenta somente para o par `('rejeitar', 'Ausencia')`; outros eventos passam à função real sem aumentar esse contador. (`app/ponto/services/ausencia.py:1482`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`)

   **Não encontrei outro produtor desse par no caminho conferido.** O signal chama o cartório; pela construção desta fixture, não há células, e o cartório retorna antes do julgamento. O consumidor fica em `on_commit`. O acordador final não é alcançado, pois está depois do laço interrompido. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:64`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:194`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:2029`; `app/ponto/signals.py:272`; `app/ponto/services/ausencia.py:1508`)

   **Não identifiquei trilha legítima que sobreviva a esse rollback e faça a contagem global falhar indevidamente.** Havendo células, o cartório pode emitir `('veredito_celula', 'CelulaDia')`, mas essa escrita síncrona também fica dentro da transação da rejeição; `evento` usa um savepoint interno, não uma confirmação independente. O consumidor agendado nesse ato não executa após seu rollback. (`app/ponto/portas/celula.py:1100`; `app/core/observ.py:133`; `app/core/observ.py:134`; `app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1506`; `app/ponto/signals.py:272`)

2. **Comentários sobre o teto**

   **Sim, os dois agora descrevem o comportamento implementado.** `_teto_da_linha_aberta` considera competências com `derivado_em` não nulo e devolve o maior entre hoje e o fim da última competência carimbada. `_acordar_projecao` fornece esse teto ao juiz dos dias; edição e decisão chamam esse mesmo acordador. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:192`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:1123`)

   Não encontrei comentário contrário nos trechos pesquisados da obra. As duas ocorrências potencialmente ambíguas estão qualificadas: “competência futura não tem projeção” aparece para dizer que isso **não é garantido**; “fim vazio = HOJE” descreve a fórmula que **era** usada. Censo universal de toda documentação: `[nao verificado]`. (`app/ponto/services/ausencia.py:170`; `app/ponto/services/ausencia.py:668`)

3. **DESCARTADOS da v2**

   | Motivo | Verificação |
   |---|---|
   | Auditoria antes do `save`, mantendo `atomic`, é equivalente | **Sustenta-se para o contrato persistido “nenhuma rejeição sem trilha”.** Ambas as escritas continuam no mesmo bloco, e `None` provoca rollback. Não implica equivalência de toda ordem observável dos signals. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1506`) |
   | `julgada_em = gerada_em` está fora do escopo | **Aceitável como delimitação, não como eliminação da lacuna.** O relógio congelado ainda permite igualdade entre geração e `instante`; o espião demonstra julgamento real por dia, mas não distingue esses timestamps. A implementação atual grava `agora`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:481`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:490`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:497`; `app/ponto/portas/celula.py:1194`) |
   | `dias_corridos/hora_ini/hora_fim` podem ficar fora do retrato | **Sustenta-se para o escritor atual:** a rejeição persiste somente `status/atualizado_em`, e o override repassa esse `update_fields`. Continua sem provar preservação dos campos omitidos contra mudanças futuras. (`app/ponto/services/ausencia.py:1498`; `app/ponto/models.py:918`; `app/ferias/services.py:683`) |
   | Adesão, veredito e ata ficam fora | São limites de cobertura coerentes: a fixture percorre `nunca_bateu`; os asserts conferem atividade, status, carimbo e FK, sem comparar ata/veredito. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:487`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:493`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`; `app/ponto/services/cartorio.py:1318`) |
   | Relógio, D e invalidação sem retentativa são dívida anterior | Os mecanismos continuam presentes; anterioridade e fila efetiva são `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`) |
   | `obs` como metadado é dívida anterior | A interpretação por substrings existe. Anterioridade e encaminhamento à fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:30`) |

   Os demais limites herdados também permanecem: `_nada_mudou` admite qualquer trilha nova, sem validar sua legitimidade; o selo exige presença de chamada viva, enquanto o positivo impede um acordador globalmente inoperante. Execuções relatadas e igualdade com a base: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:545`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:574`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)

4. **Escritores, leitores e significado**

   | Dado | Quem escreve | Quem lê / significado |
   |---|---|---|
   | `LogAuditoria` anterior | A criação das ausências emite `criar`; `evento` insere as linhas. (`app/ponto/services/ausencia.py:585`; `app/core/observ.py:134`) | O teste fotografa a **contagem total**, depois de `_agendar`, e compara após a exceção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
   | Trilha da rejeição | `_ev_rej` fornece antes/depois; a primeira chamada é real, a segunda retorna `None`. (`app/ponto/services/ausencia.py:1502`; `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:268`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`) | O serviço verifica **sucesso da gravação** pelo retorno; o teste verifica **cardinalidade final**. São medidas diferentes, compatíveis com o rollback esperado. (`app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
   | `Ausencia.status` | O espelho cria `aprovada`; a rejeição atribui e salva `rejeitada`. (`app/ponto/services/ausencia.py:1391`; `app/ponto/services/ausencia.py:1496`; `app/ponto/services/ausencia.py:1498`) | A seleção exclui rejeitadas, a trilha registra esse mesmo estado e o teste exige zero rejeitadas no banco. O significado concorda. (`app/ponto/services/ausencia.py:1489`; `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`) |
   | Status como insumo | `_insumo_da_folha` consulta o estado persistido. (`app/ponto/services/ausencia.py:138`) | Folha inclui somente aprovadas; cobrança exclui rejeitadas. São efeitos distintos do mesmo estado, explicitamente separados pelo predicado. (`app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2531`) |

5. **Defeito novo no delta**

   **Nenhum identificado por leitura.** O limite restante do teste novo é que zero rejeitadas e contagem igual de trilhas não demonstram, isoladamente, identidade ou restauração integral dos registros. O snapshot mais forte continua no negativo da **primeira** falha; isso delimita a prova do teste intermediário, sem evidenciar regressão no serviço atual. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/services/ausencia.py:1484`)

## 16/11 09:00 · sessao 7519f15333fc · - · - · origem=Ronald (tmux)

Revalidação por script em memória:

- **F2:** seção `[DEPENDE-F1]`:69; lista14:100; contagem10 integrais +4 parciais:101; coluna BASE:102.
- **F6:** U9b:44; S1:48; S2:49; lista11:94.
- **F4:** usar sua numeração corrigida; minha contagem anterior omitiu a linha de `fabrica_da_celula.py`.

## 16/11 08:59 · sessao 53037c2ed30b · - · - · origem=Ronald (tmux)

1. A ordem observada é `atomic` → carregar/travar ausências por PK → capturar insumo → para cada linha, salvar `status='rejeitada'` → chamar `evento('rejeitar','Ausencia')`; na segunda resposta `None`, a exceção sai do `atomic`, antes de alcançar `_acordar_projecao`. O teste depende de a primeira chamada filtrada voltar da função real e a segunda ser interceptada, não de qual data representa cada PK. (`app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1488`, `app/ponto/services/ausencia.py:1494`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1499`, `app/ponto/services/ausencia.py:1506`, `app/ponto/services/ausencia.py:1508`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`)

2. O alias de `evento` é importado dentro de `rejeitar_ausencias_de_ferias`, portanto já recebe o mock ativo. O contador filtra estritamente `(acao,modelo)==('rejeitar','Ausencia')`; outros eventos seguem pela função real sem incrementar `chamadas`. (`app/ponto/services/ausencia.py:1482`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`)

3. No caminho lido, não há segundo produtor desse par: o `post_save` de Ausência chama `_julgar_dias`/`julgar_celula`; o cartório devolve `None` sem célula, enquanto o consumidor da ausência agenda trabalho em `on_commit`. O acordador final sequer é alcançado pela falha na segunda auditoria. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:2027`, `app/ponto/services/cartorio.py:2029`; `app/ponto/signals.py:250`, `app/ponto/signals.py:272`; `app/ponto/services/ausencia.py:1506`, `app/ponto/services/ausencia.py:1508`)

4. A fixture comum e `_agendar` não criam células ou batidas; criam colaborador e agendamento, que espelha cinco Ausências aprovadas e suas trilhas de criação antes da fotografia `trilhas_antes`. A afirmação de ausência de células é inferência estática desses construtores; estado efetivamente executado `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:64`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:194`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:202`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:205`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`; `app/ferias/models.py:444`, `app/ferias/models.py:449`; `app/ponto/services/ausencia.py:1389`, `app/ponto/services/ausencia.py:1391`, `app/ponto/services/ausencia.py:585`)

5. Mesmo havendo células, o cartório pode escrever auditoria legítima com outro par, `('veredito_celula','CelulaDia')`; ela não aumenta `chamadas`. Esses escritores síncronos ficam dentro do `atomic` externo da rejeição e também desfazem com ele; o `on_commit` do consumidor é registrado nesse mesmo ato e não roda após seu rollback. Não identifiquei escritor legítimo que sobreviva ao rollback e cause falso negativo na contagem global deste teste. (`app/ponto/portas/celula.py:1085`, `app/ponto/portas/celula.py:1100`; `app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1506`; `app/ponto/signals.py:272`)

Mapa dos campos no caminho:

| Dado | Escritor | Leitor/significado |
|---|---|---|
| `LogAuditoria` prévio | A criação de cada Ausência chama `evento('criar',...)`; `evento` insere o registro em savepoint. (`app/ponto/services/ausencia.py:585`; `app/core/observ.py:133`, `app/core/observ.py:134`) | O teste fotografa a contagem **total**, após `_agendar`, e exige a mesma contagem após a exceção. Não conta somente rejeições. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
| Auditoria da rejeição | `_ev_rej` fornece status antes/depois; `evento` grava via `LogAuditoria.objects.create`. A primeira chamada é real, a segunda devolve `None` sem gravar. (`app/ponto/services/ausencia.py:1499`, `app/ponto/services/ausencia.py:1502`, `app/ponto/services/ausencia.py:1503`; `app/core/observ.py:134`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:268`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`) | O serviço testa **sucesso de gravação**, pelo retorno `is None`; o teste mede **cardinalidade final**, conceito diferente mas compatível com rollback esperado. (`app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
| `Ausencia.status` inicial | Espelho chama `criar_ausencia(...status='aprovada')`, e esta usa esse status nos campos da criação. (`app/ponto/services/ausencia.py:1391`, `app/ponto/services/ausencia.py:486`) | `_insumo_da_folha` lê do banco pelo predicado `_pelo_efeito('folha')`, que inclui somente aprovadas. (`app/ponto/services/ausencia.py:138`; `app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2529`) |
| `Ausencia.status` rejeitado | Atribuição e `save(update_fields=['status','atualizado_em'])`. (`app/ponto/services/ausencia.py:1496`, `app/ponto/services/ausencia.py:1498`) | A seleção exclui já rejeitadas; a auditoria registra o mesmo literal; o teste conta esse literal no banco. O modelo também o declara como “Rejeitada”. (`app/ponto/services/ausencia.py:1489`, `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/models.py:704`) |
| Status para cobrança/cartório | Não há outra escrita de `Ausencia.status` nos receivers lidos; eles julgam ou agendam consumidores. (`app/ponto/signals.py:143`, `app/ponto/signals.py:275`) | Para cobrança, o predicado exclui rejeitadas, coerente com o mesmo estado de negócio; nesta fixture o cartório retorna antes desse processamento por não encontrar célula. (`app/ponto/turnos.py:2530`, `app/ponto/turnos.py:2531`; `app/ponto/services/cartorio.py:2027`, `app/ponto/services/cartorio.py:2029`) |

Limite do teste novo: zero rejeitadas e contagem igual de auditorias não provam, isoladamente, identidade ou restauração integral dos registros; ele não repete o snapshot do teste anterior. Isso não constitui defeito novo observado no serviço. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`)

## 16/11 08:59 · sessao 2244d8af6ed9 · - · - · origem=Ronald (tmux)

Os comentários agora concordam com o código: o teto é `max(hoje, fim da última competência com derivado_em não nulo)`; `_acordar_projecao` usa esse teto para consultar `dias_da_ausencia` e invalida os trechos resultantes. Edição e decisão chamam o mesmo acordador. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:195`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1123`)

Não encontrei comentário contrário nos arquivos Python pesquisados de `app/ponto`. As ocorrências que poderiam confundir estão qualificadas: “competência futura não tem projeção” aparece para dizer que isso **não é garantido**; “fim vazio = HOJE” é identificado como fórmula **anterior**. Censo universal de qualquer documento da obra: `[nao verificado]`. (`app/ponto/services/ausencia.py:170`; `app/ponto/services/ausencia.py:668`)

Os descartes da v2 sustentam-se com limites:

- **Auditoria antes do save, mantendo atomic:** equivalência aceitável para o contrato persistido “sem trilha não fica rejeição”. A trilha e o save ficam no mesmo bloco, e o `None` provoca saída por exceção; isso não demonstra equivalência de toda ordem observável dos sinais. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`)
- **`julgada_em = gerada_em`:** descartar como escopo é decisão justificável, mas “mutante implausível” não elimina a lacuna temporal. O relógio é congelado, a célula é criada antes de `instante` e ambos podem representar o mesmo instante. O espião prova retorno do juiz real com contador positivo por dia; não prova a correção temporal isoladamente. A implementação real escreve `agora`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:481`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:485`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:490`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:497`; `app/ponto/portas/celula.py:1194`; `app/escala/models.py:1702`)
- **Campos omitidos:** a rejeição persiste explicitamente somente `status` e `atualizado_em`; o cancelamento chama essa porta. O override de `save` pode ajustar atributos em memória, mas recebe aquele `update_fields` e o repassa ao `super().save`. Portanto, o motivo de escopo se sustenta para o escritor atual; não constitui teste de preservação dos campos omitidos. (`app/ponto/services/ausencia.py:1498`; `app/ferias/services.py:683`; `app/ponto/models.py:911`; `app/ponto/models.py:917`; `app/ponto/models.py:918`)
- **Trilha nova permitida:** contrato e implementação concordam, mas aceitam qualquer inserção nova, sem verificar se é recusa legítima. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:545`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:574`)
- **Adesão/veredito/ata:** continuam limites reais: a fixture assume o ramo `nunca_bateu`, que retorna antes do restante; os asserts verificam contador, status, timestamp e ausência, sem comparar ata/veredito. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:487`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:493`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`; `app/ponto/services/cartorio.py:1318`)
- **Relógio/D/invalidação:** mecanismos continuam presentes: leitura interna de relógio, wrapper `engolir` sem atomic próprio, falha de invalidação devolvendo zero e insumos iguais pulando nova invalidação. Historicidade e fila efetiva: `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`)
- **Selo/maps vazios:** o selo exige chamada viva; o positivo exige que o fechamento perca o carimbo, portanto impede acordador globalmente inoperante. Execuções relatadas: `[nao verificado]`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)
- **`obs` como metadado:** a interpretação por substrings está presente; anterioridade histórica e gestão em fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:30`)
- **Histórico/base:** trecho atual é compatível com remoção de J1, pois passa do save da Justificativa para 5b. Igualdade byte a byte com base e logs executados: `[nao verificado]`. (`app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:108`)

Não identifiquei defeito novo no delta lido: o novo negativo observa duas tentativas de trilha, nenhuma rejeição persistida, contagem de trilhas restaurada e exceção; os demais trechos mudam documentação/mensagem para o que os asserts efetivamente verificam. Isso é conclusão estática; execução `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:277`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:281`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:250`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:498`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:542`)

## 16/11 08:56 · sessao 452fd522f259 · - · - · origem=Ronald (tmux)

Revisão: **NAO PROCEDE quanto ao wrapper/fallback.** O item 9 diz “fora da vaga”, e prevê explicitamente entrar atrás da espinha caso o wrapper exija essa vaga (`cortes_f2_f9_1611.txt:10`).

Não há flag para escrita de RED fora do papel construtor: o parser só oferece `--sonda`, `--anexo` e `--construtor`, e o cabeçalho exclui o RED excepcional desse wrapper (`bin/codex_pergunta.sh:30-33`; `bin/codex_pergunta.sh:58-66`). A via existente é `CODEX_PAPEL=construtor` com `--construtor <wt>`, obrigatoriamente com `logs/construtor.vaga`; isso se encaixa no fallback escrito (`bin/codex_pergunta.sh:197-200`; `bin/codex_pergunta.sh:248-253`; `bin/codex_pergunta.sh:418-426`).

Também não demonstraria impossibilidade só porque a entrega fica em `logs/`: o wrapper admite o arquivo de resposta sob `logs/`, e o Code pode registrar a entrega; o item não atribui explicitamente essa gravação final ao sandbox do Codex (`bin/codex_pergunta.sh:205-210`; `bin/codex_pergunta.sh:422`; `bin/codex_pergunta.sh:430`; `cortes_f2_f9_1611.txt:10`). A escrita direta de `logs/.../reds.md` pelo Codex construtor seria proibida por estar fora de `app/` (`bin/codex_pergunta.sh:134`; `bin/codex_pergunta.sh:345-346`).

O contrato específico de RED desta obra permanece **[nao verificado]**; a exigência geral existe, mas sua ausência no dado não prova inexistência nem exige criar um novo modo (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`).

## 16/11 08:55 · sessao 30eec1e06ad0 · LOTE3-C7 · verificador · origem=Code

**O delta fecha o mutante da volta 2. Não identifiquei sobrevivente plausível na família pedida para a falha injetada; a implementação atual mantém porta e trilha no mesmo `atomic`, com o `except` fora.** (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`; `app/colaboradores/views.py:333-343`). Execução dos mutantes: **[nao verificado]**.

1. **Poder de distinção do teste** [nao verificado]

| Mutante | Por que o teste o rejeita, por análise estática | [nao verificado]
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta ou só na trilha | A porta deixa o posto novo gravado; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, inclusive o m3 com atomic somente na trilha | A leitura durante a trilha encontra o posto antigo; a nova asserção exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-253`). |
| Atomic envolvendo o `try/except`, capturando a falha internamente sem provocar rollback | O `RuntimeError` ocorre depois do INSERT real; capturá-lo sem desfazer as escritas deixa FK/contagem incompatíveis com as verificações finais. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-259`). |
| Atomic somente no tratamento do erro | Não desfaz as escritas anteriores; falham FK ou contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

Isso não constitui prova universal da estrutura: o snapshot exige o **efeito da gravação**, sem verificar diretamente a chamada de `salvar_inteiro`. Substituí-la por `colaborador.save()` dentro do atomic comum preservaria os efeitos deste teste, removendo a guarda; a recusa é exercitada pelo teste separado. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`).

2. **Leitura dentro do substituto**

No caminho mostrado, ela acrescenta uma consulta e guarda um inteiro numa lista; não altera o objeto usado pela view nem executa gravação ou comando de rollback. A escrita real e o `RuntimeError` continuam depois dessa consulta. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-249`).

**Não há verde por uma asserção engolida:** as asserções ficam depois do POST. Se o `get` falhar, a chamada real não chega ao `append`, e `len(escritas) == 1` falha; se a trilha retornar `None`, outra asserção falha; o toast também exige exatamente a exceção planejada. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-264`).

A leitura observa a gravação visível nessa conexão, sem provar commit. O `TestCase` envolvente também não oculta escritas próprias que permaneceram após o POST, pois FK e contagem são consultadas antes do término do teste. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:253-259`). Outra conexão, concorrência e efeitos de timing: **[nao verificado]**.

3. **Triagem e pouso** [nao verificado]

A correção sobre a releitura procede: o gancho altera somente situação/data/motivo, e o delta observa o posto durante a trilha. Esses testes continuam compatíveis com restaurar `_posto_antes` em memória no tratamento do erro; não distinguem essa restauração da releitura atual. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:247-253`; `app/colaboradores/views.py:343`).

Os demais itens têm fundamento no código:

| Item mantido em fila | Evidência |
|---|---|
| Homônimos podem trocar FK sem trilha | Comparação textual e representação por nome/empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`). |
| Anterior potencialmente obsoleto | Capturado antes da trava; a guarda relê situação. (`app/colaboradores/views.py:326-334`; `app/colaboradores/services/colaborador.py:65-66`, `:89-91`). |
| `sem_posto` repetido | Dois ramos, painel e pendência registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`). |
| Datas com sentidos distintos | Dia atual, início do vínculo e demissão agendada. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:113-116`). |
| Trava adicional por matrícula ausente | Ramo condicional do `save`. (`app/colaboradores/models.py:427-438`). |

**Para o contrato “falha excepcional da trilha desfaz posto e trilha”, nenhum desses itens demonstra bloqueio funcional do pouso atual.** (`app/colaboradores/views.py:333-343`). O aval integral está apenas referenciado no código; conformidade com exigências além do escopo descrito no anexo, preexistência dos passivos e resultados alegados dos logs: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`).

## 16/11 08:55 · sessao 7519f15333fc · - · - · origem=Ronald (tmux)

Contando somente a marca `[DEPENDE-F1]` explícita na linha de cada degrau:

| Matriz | PROVA=NENHUM | Com marca | Sem marca |
|---|---:|---:|---:|
| F2, integrais na coluna c3 | 10 | 0 | **10** |
| F2, incluindo parciais | 14 | 0 | **14** |
| F4 | 21 | 3 | **18** |
| F6 | 11 | 8 | **3** |

Fontes: anexo F2/matriz.md:100-102; F4/matriz.md:126 e :130; F6/matriz.md:94 e respectivas linhas dos degraus abaixo. Numeração começa no primeiro `#` de cada anexo e inclui linhas vazias.

- **F2 integrais:** M1b (:37), M4 (:40), M7 (:43), M11 (:47), M13 (:49), M14 (:50), M15 (:51), M17 (:53), M18 (:54), M19 (:55). Parciais adicionais: M3 (:39), M9 (:45), M10 (:46), M20 (:56). A seção final distingue explicitamente “nenhuma mordida” de “algum ramo sem mordida” (:101); a coluna BASE é outro universo (:102).
- **F4 sem marca:** M1, M2, M13, M16, M17, M19, M23, M24; E4, E5, E7, E17, E18, E19, E20; X1, X2; J2. Os três NENHUM marcados são M9b (:42), E9 (:71) e J1 (:90). A lista completa dos21 aparece em :126.
- **F6 sem marca:** U9b (:44), S1 (:48), S2 (:49). S1/S2 trazem “SOME do escopo F6”; excluí-los daria **1**, mas o item9 não determina essa exclusão. A lista declarada dos11 inclui ambos (:94).

Ausência de marca na linha **não demonstra independência**: F2 tem dependências numa seção separada (:69-77), abrangendo M1-M4, M8-M10 e N1/M7; F4 marca dependência no contrato (:104-109), inclusive para M1/M2/M13/M16/M17 e M23, que não receberam a marca na própria linha.

Portanto, a seleção literal do item9 precisa distinguir coluna BASE/c3, integrais/parciais, dependência da linha/dependência do contrato e os degraus F6 excluídos do escopo. As matrizes fornecem as categorias, mas não uma seleção única inequívoca para “antes do pouso da F1”. (Anexo F2/matriz.md:69, :99-102; F4/matriz.md:104-109; F6/matriz.md:48-49, :94.)

## 16/11 08:54 · sessao 4d65f7eb3223 · - · - · origem=Ronald (tmux)

Não encontrei sobrevivente funcional plausível na família restrita solicitada, por análise estática: o teste exige posto novo durante a trilha, escrita real e, após a exceção, posto antigo, contagem original, HTML antigo e toast exato. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`).

| Mutante | Por que falha |
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta; atomic só na trilha após porta | A porta encerra seu bloco próprio e deixa o posto novo; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, com ou sem atomic somente na trilha | A leitura durante a trilha encontra o antigo; o teste exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247`, `:253`). |
| Capturar o `RuntimeError` dentro do único atomic sem marcar rollback | As duas escritas permanecem; as consultas finais exigem desfazimento de ambas. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-249`, `:256-259`). |
| Atomic envolvendo somente o tratamento do erro | As escritas anteriores ficam preservadas e violam FK/contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

A implementação atual confirma atomic comum, porta antes da trilha e `except` fora do bloco. (`app/colaboradores/views.py:333-343`). Execução desses mutantes: **[nao verificado]**.

Limite: este teste sozinho não comprova passagem por `salvar_inteiro`; não há spy nessa função. Substituí-la por `colaborador.save()` dentro do mesmo atomic preservaria os efeitos examinados, mas removeria a guarda. A guarda é exercitada pelo teste separado de desligamento durante a carga. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`). Esse bypass é um mutante fora da família estrita perguntada. (`app/colaboradores/views.py:334`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:250-264`).

_5986 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
