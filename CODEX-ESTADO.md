# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:35** · publicado em **11/11/2026 15:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 15:35 · sessao 36452142e014 · DATA-EVENTO-CRU · verificador · origem=Code

**Veredito: bloqueantes — há leitores reais fora do censo e ocorrências de escrita/schema registradas como leitores.** (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

1. **O selo MORDE nas formas reconhecidas, mantendo o JSON fixo.** Chave nova, aumento em escopo existente e redução sem baixar a allowlist ficam vermelhos; desaparecimento também exige baixa. Isso vale para Python e templates. Confirmei esses cinco cenários em memória usando a comparação existente. (`app/chamados/tests/test_catraca_data_evento_cru.py:133`)

   O teste denominado `test_MORDE_leitor_novo_no_censo` prova descoberta, mas não executa a comparação que reprova o leitor novo. Não executei esse caso, pois cria e apaga arquivos temporários. (`app/chamados/tests/test_catraca_data_evento_cru.py:178`)

   A catraca compara apenas com o JSON atual: aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior. Substituir uma leitura por outra no mesmo escopo também preserva a contagem, limite já declarado. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`)

2. **Falsos negativos.** O varredor reconhece `getattr` literal, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F(...))`, subscrito literal e `.get('data_evento')`. Um `{% with d=p.data_evento %}` também entra. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`; `app/chamados/tests/test_catraca_data_evento_cru.py:174`)

   Escapam nomes calculados, argumentos expandidos por `*`/`**`, campos declarados em listas/tuplas, `source='data_evento'`, serialização genérica e template `|attr`. A detecção depende de atributo, subscrito literal, argumento posicional literal ou keyword que comece pelo campo. (`app/chamados/tests/test_catraca_data_evento_cru.py:23`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   **Leituras verdadeiras omitidas hoje:**

   - `pergunta_origem__data_evento__isnull=False` lê o campo pela relação, mas não casa com a regex ancorada em `data_evento`. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/tests/test_catraca_data_evento_cru.py:50`)
   - A chave da família marco contém `data_evento`, usado por `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(...))`; essas ocorrências escapam. (`app/chamados/services/fio.py:1373`; `app/chamados/services/fio.py:1948`; `app/chamados/services/fio.py:1949`; `app/chamados/services/fio.py:1950`; `app/chamados/services/fio.py:1970`)
   - O lookup dinâmico de nulidade também escapa. (`app/chamados/services/fio.py:1378`; `app/chamados/services/fio.py:1946`)
   - Fora de `app/`, mensageria lê o valor serializado com `i.get('data_evento')`; essa exclusão está declarada. (`mensageria/nucleo/ferramentas.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:25`)

   Existência atual de serializer ou template `|attr` lendo esse campo: **[nao verificado]**.

3. **Falsos positivos.** Atribuição simples e `create(data_evento=...)` são excluídos; uma leitura no lado direito continua contando. (`app/chamados/tests/test_catraca_data_evento_cru.py:48`; `app/chamados/tests/test_catraca_data_evento_cru.py:82`)

   Contudo, qualquer outra chamada com keyword `data_evento=` conta. Assim, `abrir` e `abrir_conversa` entram apenas por encaminharem o valor ao escritor, sem ler campo de pergunta. (`app/chamados/tests/test_catraca_data_evento_cru.py:96`; `app/chamados/models.py:2789`; `app/chamados/models.py:2801`)

   Os dois `Q` internos às declarações de índices também contam, apesar da exclusão de `UniqueConstraint`: a recursão visita seus filhos. A allowlist registra essas duas ocorrências em `PerguntaDisputa.Meta`. (`app/chamados/tests/test_catraca_data_evento_cru.py:99`; `app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/tests/catraca_data_evento_cru.json:39`)

   Não há resolução de modelo: um atributo homônimo de outro objeto seria contado igualmente. Outro modelo concreto afetado hoje: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:82`)

4. **Juiz excluído pelo nome exato.** A exclusão é `out.pop(JUIZ, 0)`. Renomeá-lo faz surgir chave nova e deixa a contagem do juiz isento em zero, reprovando também a prova de existência; não passa vazio. (`app/chamados/tests/test_catraca_data_evento_cru.py:44`; `app/chamados/tests/test_catraca_data_evento_cru.py:127`; `app/chamados/tests/test_catraca_data_evento_cru.py:146`)

5. **JSON e contratos.** O leitor é `test_so_desce`; a mensagem exige baixa manual. Escritor automatizado/autoria histórica: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:134`; `app/chamados/tests/test_catraca_data_evento_cru.py:143`)

   Não há colisão nos enumeradores examinados: O20 seleciona `test_contract_*.py`; P13 exclui `tests/` e exige `.py`; o selo de relógio seleciona `test*.py`, excluindo o JSON. Ausência universal de colisões na suite: **[nao verificado]**. (`app/core/tests/test_o20_contrato_julga_codigo.py:164`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:150`; `app/core/tests/test_selo_teste_sem_relogio.py:53`)

   O JSON soma **217 ocorrências em 120 chaves: 212 Python + 5 templates**, coincidindo com o censo implementado. Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**. (`app/chamados/tests/catraca_data_evento_cru.json:2`; `app/chamados/tests/catraca_data_evento_cru.json:119`)

6. **Classes anteriores de reprovação.** Há mordida efetiva na comparação, porém cobertura incompleta nas formas acima. Os arquivos novos implementam contagem e comparação, sem introduzir juiz do dia, leitura de relógio ou derivação paralela do dia. Execução da suite Django/pre-push neste hash: **[nao verificado]**. (`app/chamados/tests/test_catraca_data_evento_cru.py:68`; `app/chamados/tests/test_catraca_data_evento_cru.py:112`; `app/chamados/tests/test_catraca_data_evento_cru.py:133`)

**Bloqueantes para aprovação:** incluir os leitores reais omitidos e retirar os repasses de escrita/schema do censo, recalibrando a allowlist. (`app/ponto/management/commands/vigia_plantio_overnight.py:75`; `app/chamados/services/fio.py:1948`; `app/chamados/models.py:2789`; `app/chamados/models.py:3486`)

## 11/11 15:34 · sessao 97538cdfafe8 · - · - · origem=Ronald (tmux)

Inventário estático de `select_for_update`: **23 sites fora de testes/migrations; 18 com alcance por código até Pauta DP**, dos quais **16 têm borda produtiva identificada e 2 pertencem às portas editar/trocar usadas só pelos testes encontrados**. Contagem por AST de `Call(Attribute(..., 'select_for_update'))`; inclui as linhas enumeradas abaixo. A execução do censo não deixou arquivo de saída: **[nao verificado]**. [nao verificado]

| Recurso / função | Sites de aquisição | Ordem e alcance até DP |
|---|---|---|
| Colaborador, Disputa, Chamado, Pergunta — `PerguntaDisputa.trava_na_ordem` | `app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692` | **C → D → Ch → P**, cada conjunto por PK; `_gravar_estado` pede universo antes de salvar, e o save acorda cartório/fábrica. `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`. |
| FechamentoMensal — command pago/devido | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` | Atomic `:189` → C `:193` → FM `:196` → Pauta cabeça `:207` → DP → filhas `:211` → DP. |
| Justificativa — aprovação/rejeição/materialização | `app/ponto/services/justificativa.py:142`, `:277`, `:344`; `app/ponto/views.py:1794` | **C/D/Ch antes da Justificativa**, pelos pedidos `:141`, `:276`, `:343` e view `:1793`; depois save de J → signal cartório → fábrica → DP. `app/ponto/services/justificativa.py:208`; `app/ponto/services/justificativa.py:287`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1020`. |
| AgendamentoFerias e PeriodoAquisitivo — `_travar_agendamento` | `app/ferias/services.py:364`, `:367` | Ag → PA → Ausencia por UPDATE/DELETE/INSERT → cartório ou pauta de férias recusadas → DP. `_trocar_janela`: atomic `:433`, locks `:434`, exclusão `:445`, save `:450`; sync em `app/ferias/models.py:449` e `:451`; pauta direta em `app/ponto/services/ausencia.py:1216` e `:1245`. |
| AgendamentoFerias — alinhamento por data | `app/ferias/services.py:600` | Atomic `:599` → Ag → save de status `:604` → sync Ausencia `app/ferias/models.py:451` → cartório/DP, ou pauta de recusa `app/ponto/services/ausencia.py:1245`. |
| SolicitacaoFerias — view avaliar | `app/ferias/views.py:740` | Atomic `:739` → Solicitação → aprovação `:753` → criação/save Agendamento `app/ferias/services.py:503` → sync Ausencia `app/ferias/models.py:451` → DP; depois UPDATE PA `app/ferias/services.py:506`. |
| DecisaoHE — decisão/período | `app/ponto/portas/he.py:174`, `:555` | Prévia atomic `app/ponto/services/autorizacao_he_periodo.py:321` ou confirmação `:343` → `_ato` → `autorizar_em_lote` `:277` → HE `app/ponto/portas/he.py:555` → decisão HE `:562`/`:174` → pauta DP `app/ponto/services/autorizacao_he_periodo.py:300`/`:244`. |
| Ausencia — rederivação H06 | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138` (chamada continua em `:139`) | Atomic `:137` → Ausencia → editar `:169` → save `app/ponto/services/ausencia.py:509` → signal `app/ponto/signals.py:147` → cartório/fábrica/DP. |
| CelulaDia — editar dia | `app/ponto/portas/celula.py:593` | Atomic `:592` → Celula → órfãs `:626` → `app/ponto/management/commands/gerar_celulas.py:255` → contexto `app/chamados/services/contexto.py:103` → C/Ch `app/chamados/models.py:486`. Julgamento/DP vem após saída desse atomic, em `app/ponto/portas/celula.py:627` e `:434`; fica na mesma transação quando herda o atomic de trocar `:652`. Chamador encontrado: `app/ponto/portas/celula.py:675`; bordas externas encontradas só testes, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. |
| Ausencia — trocar plantão | `app/ponto/portas/celula.py:657` | Atomic `:652` → Ausencias por PK `:657` → células por colaborador/data `:673` → editar `:675` → C/Ch por órfãs → julgamento → DP. Uso identificado só em teste, por exemplo `app/ponto/tests/test_o10b_editar_dia_troca.py:116`; chamador produtivo **[nao verificado]**. |

**Cinco sites excluídos do alcance DP no mesmo atomic lido:**

| Sites | Razão |
|---|---|
| `app/ponto/portas/he.py:318`, `:445` | Recusar ponta/revogar têm os commands próprios como chamadores encontrados; não alcançam a pauta do ato de período. Chamadas: `app/ponto/management/commands/recusar_ponta_pequena.py:170`; `app/ponto/management/commands/lavrar_he_pendente.py:59`. |
| `app/core/services/aplicacao_convencao.py:115` | O atomic cadastra/altera AplicacaoConvencao e grava trilha; retorna em `:124`/`:134`. Não foi encontrado receiver que leve esse model ao cartório; alcance indireto **[nao verificado]**. |
| `app/holerite/management/commands/processar_lote_holerite.py:54`, `:124` | Atomics cuidam de estado do lote; chamada ao motor fica fora deles, em `:89`. Alcance de DP dentro desses dois atomics **[nao verificado]**. |

| Advisory | Aquisição / duração |
|---|---|
| Âncora E4B | `app/chamados/services/fecho_runtime.py:89`: `pg_advisory_xact_lock`, dentro atomic `:119`; todas as âncoras ordenadas em `:120`, antes de escrever `:127`. |
| Vigia de hora | `app/ponto/services/vigia_de_hora.py:99`: `pg_try_advisory_lock`, sessão; falha retorna em `:100`/`:101`; julgamento em `:131`, unlock em `:136`. Não é aquisição bloqueante; não há atomic amplo em `rodar`. |
| Consumidor de eventos | `app/chamados/reconciliador.py:1235`: `pg_try_advisory_lock`, sessão; libera em `:1242`. O consumidor usa atos separados por chamado, sem atomic de lote, conforme implementação/chamadas `:1364`, `:1365`, `:1368`; delimitação descrita em `:1325`/`:1326`. |

**Escritas que também precisam entrar na sequência, embora não sejam `select_for_update`:**

- `Pauta INSERT → trilha → Contador.get_or_create → Contador UPDATE`: `app/pautas/services.py:180`, `:186`, `:188`, `:352`, `:354`. `lavrar` **não tem decorador atomic** em `:344`; chamado avulso em autocommit não conserva DP entre alvos; comportamento efetivo do banco **[nao verificado]**.
- Pauta existente UPDATE → trilha → DP: ler `app/pautas/services.py:215`/`:218`; fazer `:247`/`:251`; fazer pelo sistema `:272`/`:276`; cancelar `:294`/`:297`.
- Cartório escreve Celula antes de chamar a fábrica: `app/ponto/services/cartorio.py:1845` → `app/ponto/portas/celula.py:1072` → atomic/save `:904`/`:905`; depois fábrica `app/ponto/services/cartorio.py:1020`. Na lavra do cartório, o argumento é `save=False` em `app/ponto/services/cartorio.py:1825`, portanto citar somente `celula.py:1040` não prova o UPDATE desse caminho.
- Núcleo de Batida: signal `app/ponto/signals.py:36` → `app/ponto/nucleo.py:57` → DELETE de TurnoMaterializado `:58`/`:81` → bulk INSERT `:59`; o cartório posterior do mesmo save vem em `app/ponto/signals.py:135`.
- Invalidar FM é **síncrono**: `app/ponto/services/fechamento.py:1656`. Exemplo DP pode precedê-lo: aprovação `app/ponto/services/justificativa.py:208` → signal/cartório/fábrica → DP → `invalidar_fechamento` `:215`. O recálculo, separadamente, é `on_commit` em `app/ponto/services/fechamento.py:1824`.
- Passivo: atomic do par `app/chamados/services/passivo_validado.py:371` → plantio `:428` → regeneração exportada `:438` → outer da porta `app/ponto/portas/regen_exportada.py:169` → regeneração `:174` → FM `:179` → DiaPago `:188` → Pauta passivo `app/chamados/services/passivo_validado.py:443`. A regeneração pode acionar cartório/fábrica antes do FM por `app/ponto/portas/celula.py:354`/`:434`.
- Matrícula sem número pede C do maior matriculado em `app/colaboradores/models.py:436`; seu atomic `:429` termina antes do `super().save` `:439`, salvo atomic herdado. Alcance DP naquele atomic isolado **[nao verificado]**.

**Pares e limites:**

| Par | Avaliação estática |
|---|---|
| DP → âncora / âncora → DP | Inversão demonstrada no runtime pelo ato externo: transição antes do agrupador `app/chamados/services/fecho_runtime.py:151`/`:154`; fábrica DP `app/chamados/services/fabrica_da_celula.py:369` ou `:279`; agrupador âncora `app/chamados/services/fecho_runtime.py:121` → DP `:127`/`app/pautas/services.py:354`. Recursos compartilháveis entre colaboradores diferentes pela âncora empresa/competência `app/chamados/services/fecho_runtime.py:116` e contador por departamento `app/pautas/services.py:352`. |
| C → Celula / Celula → C | Oposto efetivo de transição: C `app/chamados/models.py:2666` → cartório `app/chamados/signals.py:217` → Celula `app/ponto/portas/celula.py:905`. Inverso em trocar: outer `app/ponto/portas/celula.py:652` → Celula `:593` → órfãs `:626` → C `app/chamados/models.py:486`. Código permite o par para mesmos C/Celula; chamador de trocar encontrado só testes `app/ponto/tests/test_o10b_editar_dia_troca.py:116`. Runtime concorrente **[nao verificado]**. |
| DP → FM / FM → DP | Há as duas ordens **de classes**: aprovação J `app/ponto/services/justificativa.py:208`/`:215` e pago/devido `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`/`:207`. **Não certificar deadlock desse par:** aprovação já toma C em `app/ponto/services/justificativa.py:141`/`:129`, e pago toma o mesmo C antes do FM em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`. Para o mesmo FM, esse C serializa os dois caminhos; outro ato sem esse serializador **[nao verificado]**. |
| Regeneração Celula → C contra transição C → Celula | Regeneração tem UPDATE Celula `app/ponto/portas/celula.py:276` antes de órfãs/C `:289`; entretanto cartório vem depois do atomic local `:354`. A porta exportada conserva ambos no outer `app/ponto/portas/regen_exportada.py:169`/`:174`, mas seu único chamador produtivo encontrado é passivo `app/chamados/services/passivo_validado.py:438`, cujo plantio já carimba pela porta em `app/chamados/services/materializacao.py:1277` antes da regeneração. Não certificar aquisição nova de C nem ciclo runtime sem resolver essa pré-trava. |
| Justificativa → C / C → Justificativa | Não demonstrado no código lido: as quatro bordas pedem C/D/Ch antes de J (`app/ponto/services/justificativa.py:141`, `:276`, `:343`; `app/ponto/views.py:1793`). |

Correção ao meu informe intermediário: `rejeitar_ausencias_de_ferias` usa UPDATE sem signal (`app/ponto/services/ausencia.py:1281`), portanto o alcance cartório/DP do cancelamento-admin vem do save posterior do Agendamento (`app/ferias/services.py:417` → `app/ferias/models.py:451`), que remove Ausencias e dispara `post_delete` (`app/ponto/services/ausencia.py:1190`; `app/ponto/signals.py:152`).

## 11/11 15:34 · sessao b65421d650d3 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Falso negativo atual, sem reflexão:** `Batida.objects.filter(pergunta_origem__data_evento__isnull=False)` consulta o campo de `PerguntaDisputa`, mas o prefixo relacional impede casar com `^data_evento...$`. O vínculo de `pergunta_origem` é `PerguntaDisputa`. — `app/ponto/management/commands/vigia_plantio_overnight.py:73-75`; `app/ponto/models.py:41`; `app/chamados/tests/test_catraca_data_evento_cru.py:50,97`.
- **Leituras dinâmicas atuais omitidas:** a chave da família MARCO inclui `'data_evento'`; `grupos_de_colisao` a usa em `values(*chave)`, `order_by(*chave)`, `g[k]` e `filter(**dict(zip(chave,ch)))`. Nenhuma dessas ocorrências casa com os predicados de constantes diretas do varredor. O queryset vem de `PerguntaDisputa`. — `app/chamados/services/fio.py:1373,1430-1433,1945,1948-1950,1970`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:64-65,84-98`.
- **Filtros dinâmicos atuais omitidos:** `_NULO_FORA_DO_INDICE['marco']` é `'data_evento__isnull'`; seus usos em `exclude(**{...})` e `filter(**{...})` ficam fora. — `app/chamados/services/fio.py:1378,1946,2000`; varredor `app/chamados/tests/test_catraca_data_evento_cru.py:96-98`.
- `getattr(p,'data_evento')`, `values/values_list/only('data_evento')`, `F/OuterRef('data_evento')`, `filter(data_evento__...)`, `annotate(d=F('data_evento'))`, `d['data_evento']` e até `d.get('data_evento')` **são vistos**: constantes posicionais de quaisquer chamadas não excluídas entram. — `app/chamados/tests/test_catraca_data_evento_cru.py:82-98`.
- `fields=['data_evento']`, `source='data_evento'`, constantes em tuplas/listas passadas por `*`, argumentos por `**`, `getattr(p,nome)` e `|attr` **não são vistos**; presença atual de serializers ou templates `|attr` de `PerguntaDisputa`: **[nao verificado]**. Os limites de reflexão/serialização/`|attr` já estão declarados. — `app/chamados/tests/test_catraca_data_evento_cru.py:23-25,64-65,89-98,105-109`.
- `{% with d=q.data_evento %}` **é visto**, inclusive no teste sintético. — `app/chamados/tests/test_catraca_data_evento_cru.py:174-176`.
- Fora de `app/`, existe consumo do valor serializado: `mensageria/nucleo/ferramentas.py:127` lê `i.get('data_evento')`; a API fornece o campo de `p` em `app/api/views_mensageria.py:568`. Esse serviço está explicitamente fora do censo. — `app/chamados/tests/test_catraca_data_evento_cru.py:25,41,112`; transporte `mensageria/nucleo/core_client.py:42-55`.
- A única declaração de campo modelo encontrada é `PerguntaDisputa.data_evento`; **não há evidência para afirmar segundo modelo existente**. O comentário diz “UM modelo”, sem dizer “outro”. — `app/chamados/models.py:2406,2423`; `app/chamados/tests/test_catraca_data_evento_cru.py:26`. Outro modelo hoje: **[nao verificado]**.
- Os `getattr` dinâmicos de histórico e reversão não demonstram leitores omitidos de `data_evento`: suas listas não incluem o campo. — `app/chamados/services/historico.py:98-105`; `app/chamados/services/passivo_validado.py:64-66,83`.

## 11/11 15:34 · sessao 5bcb028b7dc6 · - · - · origem=Ronald (tmux)

- O JSON é lido apenas por `test_so_desce`, via `open(..., encoding='utf-8')` e `json.load`; não há escritor automatizado demonstrado. A baixa depende da alteração manual exigida pela mensagem do teste. `app/chamados/tests/test_catraca_data_evento_cru.py:42`, `:134`, `:135`, `:143`. Autoria histórica do arquivo: [nao verificado].

- Soma conferida em memória: **217 ocorrências / 120 chaves**, sendo **212 Python + 5 templates**, diferente de “190+4” do contrato fornecido. Fonte: `app/chamados/tests/catraca_data_evento_cru.json:2` até `:121`; os três templates somam 1+3+1 em `:119`, `:120`, `:121`. Não encontrei publicação do contador em BACKLOG/ESTADO: [nao verificado].

- **Sem colisão demonstrada nos enumeradores examinados**: O20 considera somente `test_contract_*.py`, portanto nem o novo `test_catraca_...py` nem o JSON entram nesse censo (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:164`); seu outro varredor exige `.py` (`:88`, `:92`). P13 exclui `tests/` e exige `.py` (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:146`, `:147`, `:150`). O selo de relógio considera somente arquivos `test*.py`, podendo examinar o novo teste, mas não o JSON (`app/core/tests/test_selo_teste_sem_relogio.py:50`, `:53`). O vocabulário inclui testes, mas suas extensões são `.py/.html/.js/.md`, excluindo JSON (`app/pautas/tests/test_contract_vocabulario.py:20`, `:29`, `:32`). Ausência universal de colisões na suite executada: [nao verificado].

- A integração declarada está no código: `chamados` integra `LABELS` (`bin/regua.sh:23`), executadas pela régua (`:165`, `:168`) e pelo arsenal do pre-push, que deriva a mesma lista (`bin/pre-push.sh:184`, `:186`, `:187`). Hook instalado e execução real nesta cópia: [nao verificado].

- **Excluir o teste não fica protegido por esses selos**: labels exige apenas que o app com *algum* teste esteja na lista (`bin/tests/test_labels_fonte_unica.sh:24`, `:30`, `:32`); testes fora do git compara testes presentes na árvore com os rastreados, sem exigir a existência desta catraca específica (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`; `app/core/selos_no_git.py:26`). Excluir só o JSON, mantendo a catraca, causa falha de abertura (`app/chamados/tests/test_catraca_data_evento_cru.py:134`).

- O diff fornecido acrescenta instrumento e censo, sem juiz de domínio ou leitura de relógio; a catraca mede sintaxe (`app/chamados/tests/test_catraca_data_evento_cru.py:67`, `:112`, `:133`). O caso de mordida cria arquivos temporários (`:181`, `:184`, `:186`, `:188`); não o executei por respeito à regra de leitura. Regra paralela do dia introduzida por esses dois arquivos: não demonstrada (`:43`, `:127`, `:149`).

## 11/11 15:33 · sessao 363ba840fe9f · - · - · origem=Ronald (tmux)

- **Morde nas formas reconhecidas:** chaves novas, aumentos e quedas falham separadamente; queda ou desaparecimento exige ajustar JSON (`app/chamados/tests/test_catraca_data_evento_cru.py:137–144`). O censo atual coincide exatamente com JSON: 120 chaves, 217 ocorrências, sendo 212 Python + 5 templates; juiz excluído tem 1 ocorrência (`app/chamados/tests/test_catraca_data_evento_cru.py:112–128`; `app/chamados/tests/catraca_data_evento_cru.json:2–121`).
- **Juiz protegido por nome exato:** exclusão via `out.pop(JUIZ, 0)`; renomear gera leitor novo e faz a prova de existência falhar; não passa vazio (`app/chamados/tests/test_catraca_data_evento_cru.py:44`, `:127`, `:137`, `:146–149`).
- **FP concreto:** qualquer keyword `data_evento=` em chamada não listada como escrita conta, mesmo quando apenas entrega o dia a uma porta escritora (`app/chamados/tests/test_catraca_data_evento_cru.py:48–49`, `:92–98`). Casos atuais: `_sob_contrato` (`app/chamados/models.py:2789`, `:2801`); `reapontar_resolvedora` (`app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/declaracao_texto.py:370`); `abrir` (`app/chamados/services/declaracao_texto.py:377`, `:667`; `app/chamados/services/disputa_emissao.py:451`, `:844`).
- **FP concreto em schema:** embora `UniqueConstraint` seja excluído, a recursão visita os `Q(data_evento__gte=...)` internos, registrando duas ocorrências como leitores na chave `PerguntaDisputa.Meta` (`app/chamados/models.py:3486`, `:3492`; `app/chamados/tests/catraca_data_evento_cru.json:39`; `app/chamados/tests/test_catraca_data_evento_cru.py:99`).
- **Escrita simples não conta:** exige contexto `Load`; `create(data_evento=...)` está na exclusão; leitura real no RHS continua contando (`app/chamados/tests/test_catraca_data_evento_cru.py:48`, `:82–99`). O escritor existente é excluído corretamente (`app/chamados/services/fio.py:800`).
- **FN adicional:** `p.data_evento += 1` também lê o valor anterior, mas AST usa contexto `Store`, e o varredor não o registra (`app/chamados/tests/test_catraca_data_evento_cru.py:82`).
- **Sem resolução de modelo:** atributo/keyword/subscrito são reconhecidos por grafia; campo homônimo de outro objeto contaria igualmente (`app/chamados/tests/test_catraca_data_evento_cru.py:82–98`).
- **Prova chamada MORDE é parcial:** teste temporário prova descoberta de uma chave Python e um template; não chama a comparação que deve reprovar, nem injeta aumento no mesmo escopo (`app/chamados/tests/test_catraca_data_evento_cru.py:178–191`). A comparação efetiva, entretanto, implementa os três vermelhos (`:137–144`).
- **Observação de integridade:** comparação só usa JSON presente; não compara ao JSON de commit anterior. Aumentar JSON junto com código pode passar (`app/chamados/tests/test_catraca_data_evento_cru.py:134–139`).

## 11/11 15:33 · sessao d323d1ac48ed · - · - · origem=Ronald (tmux)

Censo por AST de todos os 3.323 `.py` sob `app/`, sem falha de parse, resolvendo `from pautas.services import escrever as ...`, `from pautas import services as _ps/sv`, import relativo da view e chamadas internas da porta: **33 callsites produtivos `escrever`**, **22 callsites em testes**, **6 callsites produtivos `lavrar`**, **1 em teste**. Resultado da execução em memória, sem arquivo de saída: **[nao verificado]**. A unidade é callsite AST, não função nem frequência runtime. O alias produtivo `_ps` está em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`; o relativo `sv`, em `app/pautas/views.py:12`.

Os 33 sites produtivos de `escrever` se dividem em **18 DP literais + 4 por constante DP + 3 destinatários dinâmicos que podem ser DP + 8 excluídos por destino diferente**. A contagem pode ser reconstruída pela tabela abaixo, onde há uma referência por callsite.

Legenda `P→D`: `INSERT Pauta` → trilha → `ContadorPauta.get_or_create`/`UPDATE`, dentro do atomic da porta (`app/pautas/services.py:152`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:352`, `app/pautas/services.py:354`). O `UPDATE` é a aquisição de linha do contador; o código não usa `select_for_update` nele (`app/pautas/services.py:354`).

| Emissor/site(s) DP | Chamador produtivo / ordem demonstrada |
|---|---|
| `fecho_runtime._escrever_pautas`, 1 literal (`app/chamados/services/fecho_runtime.py:127`) | Wrapper atomic: transição antes do agrupador (`app/chamados/services/fecho_runtime.py:147`, `app/chamados/services/fecho_runtime.py:151`, `app/chamados/services/fecho_runtime.py:154`); agrupador toma todas as advisories ordenadas, depois `P→D` (`app/chamados/services/fecho_runtime.py:119`, `app/chamados/services/fecho_runtime.py:120`, `app/chamados/services/fecho_runtime.py:127`). |
| `pauta_dp_do_limbo_exportado`, 1 literal (`app/chamados/services/fabrica_da_celula.py:279`) | Fábrica a chama no julgamento (`app/chamados/services/fabrica_da_celula.py:215`); própria função lê dedup e chama `P→D`, sem atomic externo próprio (`app/chamados/services/fabrica_da_celula.py:277`, `app/chamados/services/fabrica_da_celula.py:279`). Locks herdados do cartório/transição permanecem no atomic do chamador (`app/chamados/services/fecho_runtime.py:147`). |
| `pauta_cadastro_sem_turno`, 1 literal (`app/chamados/services/fabrica_da_celula.py:369`) | Fábrica chama em `:204`; atomic próprio `:364` → `PerguntaDisputa.trava_na_ordem(colab)` `:365` → dedup `:367` → `P→D` `:369` (`app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:364`, `app/chamados/services/fabrica_da_celula.py:365`, `app/chamados/services/fabrica_da_celula.py:367`, `app/chamados/services/fabrica_da_celula.py:369`). |
| `pre_fechamento._pauta`, 1 literal (`app/folha/management/commands/pre_fechamento.py:110`) | Cron declarado (`app/config/crons.py:406`). Fecha Pautas antigas por portas individuais antes de escrever a nova; não há atomic envolvendo o conjunto nesse corpo (`app/folha/management/commands/pre_fechamento.py:85`, `app/folha/management/commands/pre_fechamento.py:96`, `app/folha/management/commands/pre_fechamento.py:101`, `app/folha/management/commands/pre_fechamento.py:110`). Cada fechamento `UPDATE Pauta→D`; nova `P→D` (`app/pautas/services.py:272`, `app/pautas/services.py:276`, `app/pautas/services.py:180`, `app/pautas/services.py:188`). |
| `pautas_de_ferias._pauta`, 1 literal (`app/ferias/management/commands/pautas_de_ferias.py:167`) | Cron declarado (`app/config/crons.py:711`); leitura dedup → `P→D`, sem outra trava própria no corpo (`app/ferias/management/commands/pautas_de_ferias.py:145`, `app/ferias/management/commands/pautas_de_ferias.py:154`, `app/ferias/management/commands/pautas_de_ferias.py:167`). |
| `_pauta_de_vencido`, 1 literal (`app/ponto/services/ausencia.py:1055`) | Command chama escalonador (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25`, `app/ponto/management/commands/escalonar_documentos_ausencia.py:26`); ele muda Ausência e chama Pauta (`app/ponto/services/ausencia.py:1002`, `app/ponto/services/ausencia.py:1005`); emissor lê dedup e faz `P→D` (`app/ponto/services/ausencia.py:1039`, `app/ponto/services/ausencia.py:1055`). Atomic herdado de execução externa além dessas portas: [nao verificado]. |
| `_pauta_de_ferias_recusada`, 1 literal (`app/ponto/services/ausencia.py:1245`) | `AgendamentoFerias.save` salva agendamento, sincroniza espelho (`app/ferias/models.py:449`, `app/ferias/models.py:451`, `app/ferias/models.py:392`); espelho tenta criar Ausências, coleta recusas e escreve Pauta (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:1216`). Nas portas de alteração: `Agendamento FOR UPDATE→PA FOR UPDATE→Ausências→P→D` (`app/ferias/services.py:364`, `app/ferias/services.py:367`, `app/ferias/services.py:445`, `app/ferias/services.py:450`). No agendamento novo/histórico, `P→D→UPDATE PA` (`app/ferias/services.py:298`, `app/ferias/services.py:302`, `app/ferias/services.py:321`, `app/ferias/services.py:326`). |
| `pautas_dos_discordantes.sincronizar`, 1 constante DP (`app/ponto/services/pautas_dos_discordantes.py:38`, `app/ponto/services/pautas_dos_discordantes.py:177`) | Command importa/chama serviço (`app/ponto/management/commands/pautas_discordantes.py:114`). Abre primeiro, fecha depois, sem atomic envolvendo ambos no serviço (`app/ponto/services/pautas_dos_discordantes.py:173`, `app/ponto/services/pautas_dos_discordantes.py:175`, `app/ponto/services/pautas_dos_discordantes.py:182`). Assim, cada abertura é `P→D`; cada fechamento `UPDATE Pauta→D` (`app/pautas/services.py:180`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/pautas/services.py:276`). |
| HE `_linha_para_o_dp`, 1 literal (`app/ponto/services/autorizacao_he_periodo.py:244`) | Views importam serviço (`app/ponto/views.py:696`, `app/ponto/views.py:734`); prévia/confirmar envolvem atomic (`app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:343`). `_ato` autoriza lote antes da Pauta (`app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:299`): `DecisaoHE FOR UPDATE` em cada dia, ordem de entrada (`app/ponto/portas/he.py:546`, `app/ponto/portas/he.py:548`, `app/ponto/portas/he.py:555`), retrava em `decidir_he` (`app/ponto/portas/he.py:174`), depois `P→D` (`app/ponto/services/autorizacao_he_periodo.py:244`). Recálculo agendado em on_commit, não demonstra FM síncrono depois de DP (`app/ponto/portas/he.py:191`, `app/ponto/portas/he.py:202`). |
| Passivo `_pauta_da_competencia`, 2 constantes DP; `pauta_de_quem_saiu`, 1 constante DP (`app/chamados/services/passivo_validado.py:49`, `app/chamados/services/passivo_validado.py:229`, `app/chamados/services/passivo_validado.py:233`, `app/chamados/services/passivo_validado.py:266`) | Command chama reparar (`app/chamados/management/commands/materializar_passivo_validado.py:68`). Atomic por par `:371` → plantio `:427/:428` → regeneração exportada `:438` → Pauta cabeça/filhas ou sem movimento `:443/:450` (`app/chamados/services/passivo_validado.py:371`, `app/chamados/services/passivo_validado.py:427`, `app/chamados/services/passivo_validado.py:428`, `app/chamados/services/passivo_validado.py:438`, `app/chamados/services/passivo_validado.py:443`, `app/chamados/services/passivo_validado.py:450`). Regeneração chama recálculo FM e retrato DPago antes do retorno (`app/ponto/portas/regen_exportada.py:169`, `app/ponto/portas/regen_exportada.py:174`, `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:188`); sequência integral dos locks internos do plantio/regeneração: [nao verificado] nesta subpesquisa. |
| `resposta_tardia.receber`, 1 literal (`app/chamados/services/resposta_tardia.py:63`) | View importa serviço (`app/chamados/views.py:1050`). Ramo tardio anexa nota, registra log, então `P→D`; não declara atomic próprio (`app/chamados/services/resposta_tardia.py:31`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:55`, `app/chamados/services/resposta_tardia.py:63`). |
| `competencia_trancada._pauta_do_dp`, 1 literal (`app/chamados/services/competencia_trancada.py:231`) | Disparo síncrono chama `encerrar` (`app/chamados/services/competencia_trancada.py:281`, `app/chamados/services/competencia_trancada.py:282`); `encerrar` usa atomics separados por chamado e disputa (`app/chamados/services/competencia_trancada.py:182`, `app/chamados/services/competencia_trancada.py:197`), depois escreve Pauta fora desses blocos (`app/chamados/services/competencia_trancada.py:207`, `app/chamados/services/competencia_trancada.py:208`). Herança de atomic do disparador de tranca: [nao verificado]. |
| `reabrir_orfas_enterradas._pautas`, 1 literal (`app/chamados/management/commands/reabrir_orfas_enterradas.py:208`) | Command de runtime; atomics das reaberturas encerram em cada iteração (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`); chamada `_pautas` depois do laço (`app/chamados/management/commands/reabrir_orfas_enterradas.py:178`): dedup → `P→D` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:201`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`). |
| `pauta_folga_nao_definida_exportada`, 2 literais (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`) | Command envolve cabeça/filhas em atomic (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133`): `P cabeça→D→P filhas→D`, sem advisory E4B nesses sites (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`). |
| `pauta_dia_em_aberto`, 3 literais (`app/pautas/management/commands/pauta_dia_em_aberto.py:127`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:142`) | Command chama a porta por filha/cabeça, sem atomic de conjunto no corpo (`app/pautas/management/commands/pauta_dia_em_aberto.py:76`, `app/pautas/management/commands/pauta_dia_em_aberto.py:126`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:141`): cada chamada `P→D`. |
| `pauta_pago_x_devido_exportada`, 2 literais, alias `_ps` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:157`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) | Atomic (`:189`) → `PerguntaDisputa.trava_na_ordem(colab)` (`:193`) → FM `FOR UPDATE` (`:196`) → `P cabeça→D→P filhas→D` (`:207/:211`) (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`). |
| View genérica, 1 dinâmico (`app/pautas/views.py:76`, `app/pautas/views.py:78`) | Endpoint POST → `P→D` quando POST informa DP; não declara atomic próprio (`app/pautas/views.py:70`, `app/pautas/views.py:72`, `app/pautas/views.py:76`). |
| Responder, 1 dinâmico (`app/pautas/services.py:194`, `app/pautas/services.py:196`) | View chama `responder` (`app/pautas/views.py:112`); DP ocorre se remetente da Pauta original for DP e `autor_user_id` ausente (`app/pautas/services.py:196`, `app/pautas/services.py:197`); `P resposta→D`, referência ao pai (`app/pautas/services.py:200`, `app/pautas/services.py:180`, `app/pautas/services.py:188`). Estado concreto com autoria ausente: [nao verificado]. |
| Semanal, 1 dinâmico (`app/pautas/semanal.py:276`) | Chamador de ausência fornece DP (`app/ponto/services/vigia_ausencia.py:239`); command importa revisão (`app/ponto/management/commands/vigiar_familia_ausencia.py:23`). Sem atomic de conjunto (`app/pautas/semanal.py:202`); ramo zerado fecha Pautas por portas individuais (`app/pautas/semanal.py:226`, `app/pautas/semanal.py:229`), ramo aceso escreve `P→D` (`app/pautas/semanal.py:235`, `app/pautas/semanal.py:276`). |

Os **8 excluídos**:

| Callsite | Destino lido |
|---|---|
| `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107` | `supervisao`, constante `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:24` |
| `app/ponto/services/pautas_dos_deslogados.py:77` | `supervisao`, constante `app/ponto/services/pautas_dos_deslogados.py:27` |
| `app/ponto/services/cobertura.py:333` | `supervisao`, literal `app/ponto/services/cobertura.py:333` |
| `app/ponto/services/afastado_avisa.py:74` | `supervisao`, literal `app/ponto/services/afastado_avisa.py:74` |
| `app/ponto/services/pautas_do_esmeril.py:76` | `supervisao`, literal `app/ponto/services/pautas_do_esmeril.py:76` |
| `app/core/services/crons_quebrados.py:71` | `ti`, literal `app/core/services/crons_quebrados.py:71` |
| `app/core/management/commands/alarme_esteira.py:54` | `ti`, literal `app/core/management/commands/alarme_esteira.py:54` |
| `app/core/management/commands/alarme_arvore.py:50` | `ti`, literal `app/core/management/commands/alarme_arvore.py:50` |

`lavrar` tem **5 sites internos**: após escrever (`app/pautas/services.py:188`), marcar lida (`app/pautas/services.py:218`), marcar feita (`app/pautas/services.py:251`), feita pelo sistema (`app/pautas/services.py:276`), cancelar (`app/pautas/services.py:297`). Nos quatro últimos, a ordem é `UPDATE Pauta→trilha→D` (`app/pautas/services.py:215`, `app/pautas/services.py:247`, `app/pautas/services.py:272`, `app/pautas/services.py:294`). O **6º é o command avulso**, que não envolve todas as lavras num atomic próprio: `app/pautas/management/commands/lavrar_pautas.py:34`, `app/pautas/management/commands/lavrar_pautas.py:36`, `app/pautas/management/commands/lavrar_pautas.py:51`; `lavrar` também não tem decorador atomic (`app/pautas/services.py:344`). Logo não carregar o contador DP de uma iteração até a seguinte sem demonstrar um atomic externo: [nao verificado].

Par adicional estrutural encontrado, além de DP↔âncora E4B:

| Caminho A | Caminho B | Alcance/limite |
|---|---|---|
| **DP→PeriodoAquisitivo**: `registrar_gozo_historico` atomic → `ag.save()` → espelho com recusa → Pauta DP → `alvo.save()` (`app/ferias/services.py:277`, `app/ferias/services.py:298`, `app/ferias/models.py:449`, `app/ferias/models.py:451`, `app/ponto/services/ausencia.py:1216`, `app/ponto/services/ausencia.py:1245`, `app/ferias/services.py:302`). `executar_agendamento` produz a mesma ordem (`app/ferias/services.py:315`, `app/ferias/services.py:321`, `app/ferias/services.py:326`). | **PeriodoAquisitivo→DP**: `_trocar_janela` atomic → trava AG → trava PA → save AG → espelho → Pauta DP (`app/ferias/services.py:433`, `app/ferias/services.py:364`, `app/ferias/services.py:367`, `app/ferias/services.py:450`, `app/ferias/models.py:451`, `app/ponto/services/ausencia.py:1245`). | Chamadores runtime: histórico `app/ferias/views.py:211`; agendar `app/ferias/views.py:299`, `app/ferias/views.py:636`; corrigir datas `app/ferias/views.py:503`. **Inversão no grafo condicional; ciclo concreto [nao verificado]**: o guard anterior do agendamento pode recusar (`app/ferias/models.py:435`), enquanto a Pauta exige recusa posterior de `criar_ausencia` (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`). Não conferi constraints SQL/FKs efetivamente implantadas nem fixture concorrente que atravesse ambos os guards: [nao verificado]. |

Não transformar esse último candidato em “deadlock demonstrado”: só o ramo condicional e as duas ordens estão no código; satisfação conjunta e concorrência real ficam **[nao verificado]**.

## 11/11 15:32 · sessao ed15150ab293 · FABLE-GABARITO · verificador · origem=Code

1. **BLOQUEANTE.** Entrada: alterar `PRAZO` em `app/core/tests/marca_cron15.py:24`, declarando B. Esse valor participa da decisão do cron de produção (`app/core/management/commands/crons.py:92`, `:97`). O selo aceita o diretório `tests` indiscriminadamente (`bin/selo_proporcao.py:66`) e libera B (`bin/selo_proporcao.py:105`, `:111`). Motivo literal: **“arquivo de teste”**, **“qualquer outro .py no diff = classe A”** e **“A fuga entre modulos NAO entra no custo aceito”**; regras registradas em `app/docs/CORTES.md:444`. A fuga concreta alcança produção.

2. **BLOQUEANTE.** Entrada: acrescentar `## FEITO: exemplo` à allowlist `bin/tests/afirma_sem_prova_base.txt`, declarando B. O portão lê esse arquivo e desconsidera afirmações nele presentes (`bin/relato_afirma_com_prova.py:54`, `:77`, `:97`). O selo libera seu caminho pela mesma exceção indiscriminada (`bin/selo_proporcao.py:66`, `:111`). Motivo literal: **“CLASSE B so se TODO arquivo do diff”** pertencer às categorias autorizadas; a categoria é **“arquivo de teste”**, não qualquer conteúdo de `bin/tests/` (`app/docs/CORTES.md:444`). Aqui muda uma permissão efetivamente consumida pelo portão.

3. **FILA.** Entrada: C0 → C1 altera `app/x/views.py`; C1 → C2 altera `app/templates/x/a.html`. Com base C1, o selo recebe somente o template; com C0, recebe também o `.py` e exige A (`bin/selo_proporcao.py:78`, `:89`, `:92`, `:108`). Isso demonstra possibilidade de enganar pelo recorte da entrada. Motivo literal do aval original: **“achado do Codex sobre a FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta”**; regra registrada em `app/docs/CORTES.md:443`. Obrigação específica de usar `merge-base(main, HEAD)` nos avais: **[nao verificado]**.

4. **FILA, nos limites comprovados.** Entrada: BASE e HEAD com listas diferentes; o código lê a BASE (`bin/selo_proporcao.py:47`, `:91`), enquanto um comentário promete HEAD (`bin/tests/test_selo_proporcao.sh:91`). Os testes efetivos também usam BASE (`bin/tests/test_selo_proporcao.sh:44`). Aplica-se o literal **“achado […] sobre a FORMA de testar […] vira linha de fila”**, registrado em `app/docs/CORTES.md:443`. Contrato e promessa específica da MEDIDA na cópia: **[nao verificado]**. Chamador automático de pouso não localizado; execução manual no pouso: **[nao verificado]**. Portanto, não está demonstrado descumprimento do literal **“SELO no pouso”** (`app/docs/CORTES.md:443`).

Reprodução dos rc nesta sessão: **[nao verificado]**; os resultados descritos são compatíveis com os ramos citados do código.

## 11/11 15:32 · sessao b199bd514621 · - · - · origem=Ronald (tmux)

**Matriz humana: 20 arestas distintas permitidas**, sendo 10 pela tabela comum e outras 10 pelo privilégio `admin_ti`; `cancelado` e `superado` não podem ser origem nem destino de transição humana. (`app/chamados/catalogo/motor.py:99`; `app/chamados/catalogo/motor.py:142`; `app/chamados/catalogo/motor.py:145`; `app/chamados/catalogo/motor.py:150`; `app/chamados/catalogo/motor.py:153`)

Notação das sequências:

- **E**: `atomic E4B → C → D abertas → Ch vivos/pais/alvo → UPDATE Ch → receivers → relavra S2 → âncoras ordenadas → INSERT Pauta E4B → contador DP → fim atomic E4B`. Os receivers e a relavra podem acrescentar escritas opcionais descritas abaixo. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:307`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:127`)
- **N**: `atomic _gravar_estado → C → D abertas → Ch vivos/pais/alvo → UPDATE Ch → receivers → eventual relavra S2 → fim atomic _gravar_estado`; este ramo não chama o agrupador E4B. (`app/chamados/services/fecho_runtime.py:144`; `app/chamados/services/fecho_runtime.py:145`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:303`)
- **C/D/Ch/P** são as linhas de Colaborador, Disputa, Chamado e Pergunta; cada conjunto explícito é adquirido em PK crescente. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`)

| De → para | Perfil pela porta | Sequência | DP antes da âncora E4B |
|---|---|---|---|
| aberto → em_analise | Comum (`motor.py:100`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior nesse ramo (`fecho_runtime.py:145`) |
| aberto → resolvido | Comum (`motor.py:100`) | E (`fecho_runtime.py:147`) | Pode, pela fábrica (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| aberto → fechado | Comum (`motor.py:100`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| aberto → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| em_analise → aberto | Comum (`motor.py:101`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| em_analise → resolvido | Comum (`motor.py:101`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| em_analise → fechado | Comum (`motor.py:101`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| em_analise → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → aberto | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → em_analise | Comum (`motor.py:102`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → fechado | Comum (`motor.py:102`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| resolvido → registrado | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → aberto | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → em_analise | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → resolvido | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| fechado → registrado | admin_ti (`motor.py:150`; `:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → aberto | admin_ti (`motor.py:153`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → em_analise | Comum (`motor.py:105`) | N (`fecho_runtime.py:144`; `models.py:280`) | Sem âncora posterior (`fecho_runtime.py:145`) |
| registrado → resolvido | admin_ti (`motor.py:153`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |
| registrado → fechado | Comum (`motor.py:105`) | E (`fecho_runtime.py:147`) | Pode (`fabrica_da_celula.py:204`; `:215`; `:369`) |

Na tabela, `motor.py` significa `app/chamados/catalogo/motor.py`, `models.py` significa `app/chamados/models.py` e os nomes dos serviços pertencem a `app/chamados/services/`. As arestas dependem dos guards da porta: decisão de perfil, justificativa pendente, disputa viva, pergunta viva em competência aberta e premissa regularizada para `resolvido`. (`app/chamados/models.py:132`; `app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`)

As 20 arestas têm chamador runtime pelos dois serviços: detalhe local e resposta rápida no fio/modal; ocorrência concreta em produção **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:719`; `app/chamados/services/fio.py:722`; `app/chamados/views.py:547`; `app/chamados/views.py:674`; `app/chamados/views.py:954`; `app/chamados/urls.py:14`; `app/chamados/urls.py:21`; `app/chamados/urls.py:39`)

| Subcaminho síncrono dos receivers | Ordem e escritas |
|---|---|
| S143, apenas destino encerrado | Retoma C/D/Ch pela porta, tenta devolver alcance antes dos carimbos, grava perguntas mortas, pode reabrir pai e fechar disputas. (`app/chamados/signals.py:96`; `:115`; `app/chamados/services/disputa_emissao.py:1878`; `:1945`; `:1984`; `:2009`; `:2058`; `:2090`) |
| Carimbo de pergunta | `UPDATE Pergunta`, dentro de `PerguntaDisputa.save` e seu atomic; o receiver pode inserir EventoCelula. (`app/chamados/models.py:3152`; `:3422`; `:3423`; `app/chamados/signals.py:258`; `app/ponto/portas/celula.py:896`) |
| Fecho de disputa | Reaquisição C/D/Ch, `UPDATE Disputa`, S88 pode reconciliar o pai; S144 carimba pendentes já aprovadas pelo lastro. (`app/chamados/models.py:1910`; `:1936`; `app/chamados/signals.py:42`; `:164`; `app/chamados/services/materializacao.py:1418`; `:1427`) |
| Cartório do chamado | Roda se o status mudou e não há julgamento reentrante; deriva dia pelo juiz e chama `julgar_celula`. (`app/chamados/signals.py:193`; `:211`; `:215`; `:217`) |
| Lavra da célula | `UPDATE CelulaDia → INSERT EventoCelula`; a lavra do modelo delega à porta. (`app/escala/models.py:1826`; `app/ponto/portas/celula.py:904`; `:905`; `:907`; `:896`) |
| Fábrica após a lavra | Cartório chama fábrica das células julgadas; cadastro sem turno toma C e escreve DP; limbo exportado escreve DP diretamente, herdando as travas do ato. (`app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `:215`; `:279`; `:364`; `:365`; `:369`) |
| Relavra S2 após o save | Pode `UPDATE Pergunta` depois do cartório e, portanto, depois de uma Pauta da fábrica. (`app/chamados/models.py:296`; `:307`; `:312`; `app/chamados/veredito_lavrado.py:275`) |
| Eventos pós-commit | Beco de disputa e silêncio de isento são callbacks `on_commit`; não integram a sequência anterior ao commit do atomic E4B. (`app/chamados/signals.py:239`; `:280`) |

As condições da emissão antecipada são explícitas: célula passada de colaborador ativo, dia não dispensado e teto sem turno para cadastro; para limbo, motivos existentes, `trabalha=None` e competência entregue. Os guards e a célula concreta que satisfazem cada combinação em produção são **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:201`; `:203`; `:204`; `:228`; `:231`)

| Transição indireta | Chamador síncrono alcançável e ordem |
|---|---|
| aberto → resolvido; em_analise → resolvido; registrado → resolvido | `RECONCILIAR` pelo S88, cartório ou cluster; C/D/Ch antes do UPDATE do estado e seus receivers. (`app/chamados/catalogo/motor.py:111`; `app/chamados/signals.py:42`; `app/ponto/services/cartorio.py:986`; `:1511`; `:1524`; `:1545`; `:1623`; `:1638`; `app/ponto/services/cluster_espurio.py:124`; `app/chamados/models.py:650`; `:654`; `:711`) |
| resolvido → em_analise; fechado → em_analise | `REABRIR` no S143 ou no renascimento por emissão; `_gravar_estado` seguido de ressuscitação C/D/Ch/P, UPDATE perguntas e possível reabertura de disputa. (`app/chamados/catalogo/motor.py:113`; `app/chamados/services/disputa_emissao.py:1849`; `:2009`; `app/chamados/models.py:921`; `:1019`; `:1442`; `:1451`; `:1562`; `:1566`; `:1575`) |
| aberto → em_analise; registrado → em_analise | `ANALISAR` pelo repasse do emissor do cartório; C/D/Ch/P do conjunto no topo do repasse, depois `_gravar_estado`. (`app/chamados/catalogo/motor.py:112`; `app/ponto/services/cartorio.py:1763`; `:1801`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/chamados/services/disputa_emissao.py:1059`; `:1085`; `:1159`; `:1178`; `:1223`; `:1252`; `app/chamados/models.py:806`) |
| inexistente → aberto | Emissão de chamado pela fábrica, cluster ou emissor do cartório; C antes do dedup e INSERT. (`app/chamados/catalogo/motor.py:131`; `app/chamados/models.py:902`; `:905`; `:935`; `:971`; `app/chamados/services/disputa_emissao.py:568`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/management/commands/detectar_ausencias.py:693`) |
| inexistente → registrado | Existe no catálogo de nascimento; alcance concreto desse destino pelos emissores deste recorte **[nao verificado]**. (`app/chamados/catalogo/motor.py:133`; `app/chamados/models.py:935`) |
| `ENCERRAR` / `CANCELAR` | Existem no catálogo e no modelo; caminho síncrono saindo desses receivers até essas portas **[nao verificado]**. (`app/chamados/catalogo/motor.py:114`; `:116`; `app/chamados/models.py:773`; `:1612`) |

**Censo dos três nomes:** 17 chamadas AST, contando apelidos de `from ... import ...`, acessos qualificados e excluindo strings de mutantes; execução sem artefato persistido **[nao verificado]**. Os callsites lidos são:

| Função | Produção | Teste | [nao verificado]
|---|---|---|
| `transicionar_com_pauta` — 3 | 2: `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722` | 1: `app/chamados/tests/test_e4b_c6_memoria.py:75` |
| `_escrever_pautas` — 13 | 1: `app/chamados/services/fecho_runtime.py:154` | 12: memória `:128`, `:129`, `:163`, `:168`, `:189`, `:231`; ORM `:138`, `:182`, `:208`, `:209`, `:225`, `:266`, nos arquivos `app/chamados/tests/test_e4b_c6_memoria.py` e `app/chamados/tests/test_e4b_c6_orm.py` |
| `_travar_ancora` — 1 | 1: `app/chamados/services/fecho_runtime.py:121` | 0 encontrados **[nao verificado]** |

Os 12 testes diretos de `_escrever_pautas` não executam transição de Chamado nessa chamada; o teste de memória do adaptador executa a transição simulada para `fechado`. (`app/chamados/tests/test_e4b_c6_memoria.py:75`; `:128`; `:163`; `:231`; `app/chamados/tests/test_e4b_c6_orm.py:138`; `:182`; `:208`; `:225`; `:266`)

## 11/11 15:31 · sessao a139ee4f6b1c · - · - · origem=Ronald (tmux)

(1) **BLOQUEANTE.** A exceção aceita qualquer diretório `tests` ([bin/selo_proporcao.py:66](bin/selo_proporcao.py:66)), mas esse módulo participa do caminho de produção: import em [app/core/management/commands/crons.py:92](app/core/management/commands/crons.py:92), decisão pelo prazo em [app/core/management/commands/crons.py:97](app/core/management/commands/crons.py:97). Entrada concreta: alterar `PRAZO` ([app/core/tests/marca_cron15.py:24](app/core/tests/marca_cron15.py:24)). O caminho recebe B e retorna 0 ([bin/selo_proporcao.py:105](bin/selo_proporcao.py:105), [bin/selo_proporcao.py:111](bin/selo_proporcao.py:111)). Contraria “qualquer outro .py no diff = classe A” e “A fuga entre modulos NAO entra no custo aceito” [não verificado: transcrição fornecida].

(3) **FILA**, pelos elementos fornecidos. A base é argumento explícito ([bin/selo_proporcao.py:78](bin/selo_proporcao.py:78)); exige ancestralidade ([bin/selo_proporcao.py:89](bin/selo_proporcao.py:89)) e calcula exatamente esse intervalo ([bin/selo_proporcao.py:92](bin/selo_proporcao.py:92)). Entrada concreta: C0 → C1 altera `.py` fora da lista → C2 altera template; base=C1 inclui apenas template, classificado B ([bin/selo_proporcao.py:62](bin/selo_proporcao.py:62)). Os avais não prescrevem main/merge-base; enquadramento literal: “FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta” [não verificado: transcrição fornecida]. Vincular a base ao diff completo da obra é pendência; impor especificamente merge-base seria requisito adicional [não verificado].

Os retornos foram inferidos do código; execução dessas entradas nesta sessão [não verificado].

## 11/11 15:31 · sessao acf78761590e · - · - · origem=Ronald (tmux)

**Item 4: FILA.** Motivo literal: “achado [...] sobre a FORMA de testar [...] vira linha de fila, nunca volta” — anexo fornecido [não verificado em arquivo].

- A divergência comprovada está no comentário “revisao head”, em `bin/tests/test_selo_proporcao.sh:91`; a implementação lê BASE em `bin/selo_proporcao.py:47` e `bin/selo_proporcao.py:91`. Os testes efetivos também preparam a lista na BASE, em `bin/tests/test_selo_proporcao.sh:44`.
- Entrada concreta: BASE e HEAD contendo listas distintas; a chamada usa a lista da BASE, conforme `bin/selo_proporcao.py:47`.
- Contrato que promete HEAD e MEDIDA “pousos B com rc 1”: não encontrados na cópia [não verificado]. O fluxo do selo imprime a classificação e retorna, conforme `bin/selo_proporcao.py:99`.
- Chamador automático de pouso não encontrado [não verificado]. Isso não comprova ausência de execução manual no pouso [não verificado]. O literal “SELO no pouso” consta do aval fornecido; a exigência de integração automática não consta dele [não verificado em arquivo]. [nao verificado]

## 11/11 15:25 · sessao 6dc09a5905d6 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** A c8 corrige a intercalação dentro de `_escrever_pautas`, mas **B1 continua BLOQUEANTE no ato completo**: outro escritor pode tomar o contador DP durante a transição, antes das âncoras E4B. Também permanece o **BLOQUEANTE de encerramento pela suíte inteira pendente**. (`app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fabrica_da_celula.py:369`; `app/pautas/services.py:354`; `app/docs/FIO-E0-V4-E4B-c8.txt:97`; `CLAUDE.md:375`)

Quanto ao **item 1**, a classificação dos achados anteriores é:

| Achado da c7 | Situação depois da c8 |
|---|---|
| **B1: âncoras intercaladas com contador DP** | **Curado só no caso do agrupador isolado; não curado pela CLASSE global — BLOQUEANTE.** Os dois laços corrigem aquele trecho, mas a transição anterior pode escrever outra Pauta DP. (`app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fabrica_da_celula.py:369`) |
| Dia civil de conversa virando competência | **Curado pela CLASSE desse leitor.** Usa o juiz de dia do turno, descarta `None` e consulta a janela da empresa. (`app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:112`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/juizes.py:336`) |
| Duplicação concorrente marcador+âncora | **Curado pela CLASSE dos participantes desse agrupador**, quanto ao dedup: advisory antecede `exists` e criação. A porta genérica não impõe essa idempotência; não achei unicidade correspondente no modelo. (`app/chamados/services/fecho_runtime.py:119`; `app/chamados/services/fecho_runtime.py:123`; `app/pautas/services.py:180`; `app/pautas/models.py:123`) |
| Motivo limbo truncado | **Curado pela CLASSE dos formatos produzidos pelo S143.** Reserva os motivos e abrevia listas com IDs inteiros e contagens; ORM compara texto entregue à porta e persistido. (`app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:226`; `app/chamados/tests/test_e4b_c6_orm.py:228`) |
| Coleta sobrevivendo ao rollback S143 | **Curado pela CLASSE das exceções propagadas pelo resolver ou pela saída do savepoint.** O merge acontece depois da saída normal; o token é restaurado no `finally`. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`) |
| Reentrega não acrescenta IDs | **Não curado — FILA.** Pauta viva causa `continue`; a nota individual permanece. (`app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamento futuro fora das duas bordas | **Não curado — FILA de alcance.** `fechar()` usa transição direta; sem contexto, o coletor não entrega. (`app/chamados/models.py:454`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`) |
| Relógio fora da borda | **Não curado — FILA preexistente.** S143 passa `hoje=None`; o juiz pode executar `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Prova concorrente e falha real de escrita da Pauta | **Não curado — FILA de prova.** O novo ensaio observa uma conexão e chama diretamente o agrupador; a falha ORM existente é injetada no resolver. Não achei ensaio de falha real de `escrever` nesses testes. (`app/chamados/tests/test_e4b_c6_orm.py:249`; `app/chamados/tests/test_e4b_c6_orm.py:266`; `app/chamados/tests/test_e4b_c6_orm.py:273`) |
| Leitores crus de `data_evento` no caminho completo | **Não curado — FILA preexistente.** Permanecem na recusa de exportada, em S128/HX e no gate do chamado. (`app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`) |
| Lint vermelho | **Curado no código dos dois apontamentos:** import excedente removido e variável ligada ao parâmetro default. Certificação do lint pelo dado fornecido; execução independente **[nao verificado]**. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_memoria.py:297`) |
| Suíte inteira | **Não curado — BLOQUEANTE de encerramento.** Não achei comprovante integral; o recibo a deixa pendente e a regra exige seu GREEN. (`app/docs/FIO-E0-V4-E4B-c8.txt:87`; `app/docs/FIO-E0-V4-E4B-c8.txt:97`; `CLAUDE.md:375`) |

**O contraexemplo global de B1 está no próprio runtime.** O `atomic` externo executa a transição antes de chamar o agrupador. A gravação do chamado dispara o cartório; este pode chamar a fábrica e escrever Pauta DP de cadastro sem turno ou de limbo exportado, tomando o contador antes das âncoras E4B. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/services/fecho_runtime.py:151`; `app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`; `app/pautas/services.py:354`)

| Passo | T1 — colaborador X | T2 — colaborador Y da mesma empresa |
|---|---|---|
| 1 | Durante a transição, a fábrica escreve Pauta e toma **DP**. (`app/chamados/services/fabrica_da_celula.py:369`; `app/pautas/services.py:188`; `app/pautas/services.py:354`) | |
| 2 | | Toma **âncora A**; tenta escrever e espera **DP**. (`app/chamados/services/fecho_runtime.py:121`; `app/chamados/services/fecho_runtime.py:127`; `app/pautas/services.py:354`) |
| 3 | Retorna da transição, pede **A** e espera T2, mantendo DP no ato externo. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fecho_runtime.py:121`) | Continua esperando DP. (`app/pautas/services.py:354`) |

A trava de colaborador não elimina esse ciclo: X e Y podem ser distintos, enquanto a âncora usa empresa+competência e o contador é único por departamento. (`app/chamados/models.py:2666`; `app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:116`; `app/pautas/models.py:167`)

O estado necessário é permitido estruturalmente: o cartório usa o dia da célula do chamado; `_ancoradas()` não exige que as perguntas tenham esse mesmo dia. O próprio teste ancora perguntas de dois dias no mesmo chamado. Assim, a célula que provoca Pauta de cadastro pode ser diferente da pergunta viva exportada que provoca a entrega E4B. Configuração concreta e reprodução concorrente: **[nao verificado]**. (`app/chamados/juizes.py:98`; `app/chamados/models.py:344`; `app/chamados/tests/test_e4b_c6_orm.py:74`; `app/chamados/tests/test_e4b_c6_orm.py:75`; `app/chamados/services/fabrica_da_celula.py:201`)

As ordens relevantes encontradas são:

| Caminho | Ordem demonstrável |
|---|---|
| Duas bordas humanas | Ambas entram em `transicionar_com_pauta`. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`) |
| Runtime sem Pauta anterior nos receivers | **Colaborador → Disputas → Chamados → eventuais Perguntas → âncoras ordenadas → INSERT Pauta → contador DP.** (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`; `app/chamados/services/fecho_runtime.py:120`; `app/pautas/services.py:180`; `app/pautas/services.py:188`) |
| Runtime com emissão da fábrica | **Colaborador/Disputa/Chamado e demais escritas da transição → Pauta da fábrica → DP → âncoras E4B → Pauta E4B → DP.** Esta é a inversão bloqueante. (`app/chamados/models.py:290`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:369`; `app/chamados/services/fecho_runtime.py:154`) |
| Cadastro sem turno isolado | **Colaborador → Pauta → DP.** (`app/chamados/services/fabrica_da_celula.py:364`; `app/chamados/services/fabrica_da_celula.py:365`; `app/chamados/services/fabrica_da_celula.py:369`) |
| Pauta pago/devido exportada | **Colaborador → FechamentoMensal → Pauta cabeça/filhas → DP.** (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) |
| Passivo validado | **Plantio → regeneração → escrita de FechamentoMensal → Pauta DP.** A ordem integral das travas internas dessas portas fica **[nao verificado]**. (`app/chamados/services/passivo_validado.py:427`; `app/chamados/services/passivo_validado.py:438`; `app/ponto/services/fechamento.py:1160`; `app/chamados/services/passivo_validado.py:443`) |
| Ler, fazer ou cancelar Pauta | **UPDATE Pauta → contador do destinatário.** (`app/pautas/services.py:215`; `app/pautas/services.py:218`; `app/pautas/services.py:247`; `app/pautas/services.py:251`; `app/pautas/services.py:272`; `app/pautas/services.py:276`; `app/pautas/services.py:294`; `app/pautas/services.py:297`) |
| Lavra avulsa | Obtém/cria o contador e o atualiza; não achei aquisição de âncora E4B nesse comando. (`app/pautas/management/commands/lavrar_pautas.py:51`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |

**Não achei ciclo demonstrado com FechamentoMensal.** O caminho pago/devido toma FM antes de DP; o recálculo por evento consultado usa `on_commit`, portanto não demonstra uma aquisição síncrona FM depois de DP nesse ato. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/ponto/services/fechamento.py:1824`)

Nos demais pontos encontrados, a ordem **interna da porta** é INSERT Pauta→trilha→contador; a ordem integral de todas as travas herdadas de cada chamador é **[nao verificado]**. Não achei outra aquisição própria da advisory E4B nesses pontos. (`app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`; `app/chamados/services/fecho_runtime.py:89`)

- Interface, respostas e emissão semanal: `app/pautas/views.py:76`; `app/pautas/services.py:194`; `app/pautas/semanal.py:276`.
- Emissores DP: `app/folha/management/commands/pre_fechamento.py:110`; `app/ferias/management/commands/pautas_de_ferias.py:167`; `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107`; `app/ponto/services/ausencia.py:1055`; `app/ponto/services/ausencia.py:1245`; `app/ponto/services/pautas_dos_discordantes.py:177`; `app/ponto/services/pautas_dos_deslogados.py:77`; `app/ponto/services/afastado_avisa.py:74`; `app/ponto/services/autorizacao_he_periodo.py:244`.
- Chamados e comandos: `app/chamados/services/resposta_tardia.py:63`; `app/chamados/services/competencia_trancada.py:231`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`; `app/pautas/management/commands/pauta_dia_em_aberto.py:127`; `app/pautas/management/commands/pauta_dia_em_aberto.py:139`; `app/pautas/management/commands/pauta_dia_em_aberto.py:142`.
- Outros departamentos: `app/ponto/services/pautas_do_esmeril.py:76`; `app/ponto/services/cobertura.py:333`; `app/core/services/crons_quebrados.py:71`; `app/core/management/commands/alarme_esteira.py:54`; `app/core/management/commands/alarme_arvore.py:50`.

Quanto ao **item 2**, o mapa dos valores produtivos alcançados é:

| Campo/valor | Quem escreve ou produz | Quem lê / significado |
|---|---|---|
| `_RECUSAS`, token e lote | Declaração, instalação, restauração e merge após sucesso. (`app/chamados/services/fecho_runtime.py:13`; `app/chamados/services/fecho_runtime.py:34`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`; `app/chamados/services/fecho_runtime.py:149`) | Coletor compara PK de chamado e indexa por PK de pergunta; agrupador recebe o lote confirmado. (`app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/fecho_runtime.py:20`; `app/chamados/services/fecho_runtime.py:154`) |
| Motivo e IDs do limbo | S143 produz os formatos e preserva nota individual. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1855`) | Gate admite os prefixos; montador separa nomes e IDs. (`app/chamados/services/disputa_emissao.py:1946`; `app/chamados/services/fecho_runtime.py:66`; `app/chamados/services/fecho_runtime.py:69`) |
| `data_evento` e `conversa` | `abrir` fornece turno/`False`; conversa fornece dia civil/`True`; atribuição central pela porta do fio. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/fio.py:801`) | O novo agrupador usa o discriminador pelo juiz; os leitores crus remanescentes não o fazem nesses pontos. (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/models.py:364`) |
| Empresa e corte | Relação cadastral do colaborador e cadastro da empresa. (`app/colaboradores/models.py:321`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Runtime usa a empresa da pergunta; janela consulta o corte efetivo. (`app/chamados/services/fecho_runtime.py:114`; `app/ponto/janelas.py:50`; `app/ponto/janelas.py:63`) |
| Competência, âncora e agrupamento | Ano/mês do **fim da janela por corte**, agrupados por texto da âncora. (`app/chamados/services/fecho_runtime.py:115`; `app/chamados/services/fecho_runtime.py:116`; `app/chamados/services/fecho_runtime.py:117`) | Ordenação, dedup, porta, chip e payload conservam esse identificador. Não achei nesses leitores interpretação como mês civil. (`app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:123`; `app/pautas/services.py:184`; `app/pautas/services.py:336`; `app/pautas/views.py:40`) |
| Marcador e chave advisory | Constante e SHA256 de marcador+tipo+âncora. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:86`) | PostgreSQL recebe a chave; dedup usa o marcador textual. (`app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:124`) |
| Texto e listas | Montador distribui o teto; porta persiste até 500 caracteres. (`app/chamados/services/fecho_runtime.py:76`; `app/chamados/services/fecho_runtime.py:79`; `app/pautas/services.py:185`) | Payload entrega texto; nota S143 guarda motivo integral. (`app/pautas/views.py:40`; `app/chamados/services/disputa_emissao.py:1855`) |
| Destinatário DP, remetente TI e autor | Runtime fornece valores; porta resolve permissão e grava. (`app/chamados/services/fecho_runtime.py:127`; `app/pautas/services.py:49`; `app/pautas/services.py:87`; `app/pautas/services.py:180`) | Listagem usa destinatário; autoria permanece na trilha. (`app/pautas/services.py:308`; `app/pautas/models.py:105`; `app/pautas/views.py:38`) |
| `feito_em`, `cancelado_em`, `lido_em` | Portas centrais de estado da Pauta. (`app/pautas/services.py:215`; `app/pautas/services.py:247`; `app/pautas/services.py:272`; `app/pautas/services.py:294`) | `.vivas()` controla dedup; lavra usa leitura e prazo. (`app/chamados/services/fecho_runtime.py:124`; `app/pautas/models.py:57`; `app/pautas/models.py:64`; `app/pautas/services.py:356`) |
| Contador DP | `lavrar` escreve `nao_lidas`, `atrasadas` e `lavrado_em`. (`app/pautas/services.py:354`) | Service e view leem o lavrado. (`app/pautas/services.py:362`; `app/pautas/views.py:34`) |
| Estado/destino/usuário do ato | Bordas transportam; escritor atribui status e usuário e salva. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:228`; `app/chamados/models.py:231`; `app/chamados/models.py:296`) | Adaptador exige VIVOS→ENCERRADOS; receiver exige save de status encerrado. (`app/chamados/services/fecho_runtime.py:144`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`) |

Nos valores acrescentados à prova, o wrapper produz `(sql, params, in_atomic_block)`; o gate lê chaves, índices, contagens e presença de transação. As constantes de base/caso/mutante selecionam a sobreposição usada pelo roteiro. (`app/chamados/tests/test_e4b_c6_orm.py:249`; `app/chamados/tests/e4b_c6_verificar.py:76`; `app/chamados/tests/e4b_c6_verificar.py:90`; `app/chamados/tests/e4b_c6_verificar.py:134`; `app/chamados/tests/e4b_c6_verificar.py:230`)

Quanto ao **item 3**, **o ORM mede a ordem SQL real da chamada observada**, não apenas contagem: registra a consulta, executa-a, compara as chaves exatas ordenadas e exige a última advisory antes de `exists`, INSERT de Pauta e UPDATE de contador. Travar só a primeira âncora ou inverter as âncoras presentes é detectado. (`app/chamados/tests/test_e4b_c6_orm.py:249`; `app/chamados/tests/test_e4b_c6_orm.py:251`; `app/chamados/tests/e4b_c6_verificar.py:94`; `app/chamados/tests/e4b_c6_verificar.py:99`)

Há, porém, três lacunas — **FILA de prova**, diretamente relacionadas à alegação global:

- **Contador previamente travado passa:** o classificador ignora `SELECT … FOR UPDATE` sobre `pautas_contadorpauta`; só reconhece seu UPDATE. Um mutante que tome DP por SELECT antes das âncoras e mantenha os UPDATEs depois satisfaz o gate. (`app/chamados/tests/e4b_c6_verificar.py:78`; `app/chamados/tests/e4b_c6_verificar.py:84`; `app/chamados/tests/e4b_c6_verificar.py:88`)
- **Outra chave de ordenação pode passar:** memória e ORM usam uma única empresa. Ordenar apenas pela competência coincide com a ordem completa nesses lotes; não distingue uma futura ordem diferente entre empresas. (`app/chamados/tests/test_e4b_c6_memoria.py:227`; `app/chamados/tests/test_e4b_c6_memoria.py:233`; `app/chamados/tests/test_e4b_c6_orm.py:253`; `app/chamados/tests/test_e4b_c6_orm.py:257`)
- **Ordem diferente em outro caminho passa:** o wrapper envolve somente `_escrever_pautas`, excluindo a transição e seus receivers — precisamente o caminho do novo contraexemplo. (`app/chamados/tests/test_e4b_c6_orm.py:265`; `app/chamados/tests/test_e4b_c6_orm.py:266`; `app/chamados/services/fecho_runtime.py:151`)

Além disso, o gate verifica apenas um booleano de `atomic`, sem identificar a transação nem seu tempo de vida. Não achei prova de concorrência ou de manutenção das travas até o commit nesses testes. (`app/chamados/tests/e4b_c6_verificar.py:76`; `app/chamados/tests/e4b_c6_verificar.py:95`; `app/chamados/tests/test_e4b_c6_orm.py:28`)

Quanto ao **item 4**, reaparece **ordem de trava — BLOQUEANTE B1**; permanecem **relógio fora da borda e campo temporal com dois significados — FILA preexistente**; a prova de ordem global tem as lacunas acima. (`app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fabrica_da_celula.py:369`; `app/chamados/juizes.py:577`; `app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/tests/test_e4b_c6_orm.py:266`)

**Não achei segundo juiz, derivação paralela de competência ou escrita produtiva E4B fora da porta no delta.** A composição usa juiz temporal, janela existente e `escrever`. O retorno antecipado continua retirado: após a recusa seguem S128, HX e C7. (`app/chamados/services/fecho_runtime.py:104`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:127`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1953`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2089`)

Nos chamadores externos indicados pelo aval, não achei instalação do coletor E4B: chamam o resolver, e registrar sem contexto não entrega Pauta E4B. Ocorrência e quantidade em produção: **[nao verificado]**. (`app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_f

[truncado em 20000 de 22202 caracteres]

## 11/11 15:23 · sessao 2b3ccc385b07 · - · - · origem=Ronald (tmux)

B1 está curado apenas no trecho `_escrever_pautas`; **a ordem global do ato continua com inversão possível**. O helper agora adquire todas as âncoras antes da porta, mas a transição que o precede pode adquirir o contador DP por outro emissor. (`app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/fecho_runtime.py:127`; `app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fecho_runtime.py:154`)

**Contraexemplo estático — BLOQUEANTE:**

1. T1 inicia fechamento do colaborador X; trava C→D→Ch, grava status e dispara os receivers. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/models.py:290`; `app/chamados/models.py:296`)
2. O receiver chama o cartório; o cartório chama a fábrica, que pode emitir Pauta DP de cadastro sem turno ou limbo exportado, adquirindo o contador DP. (`app/chamados/signals.py:209`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:2012`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:215`; `app/chamados/services/fabrica_da_celula.py:369`; `app/pautas/services.py:188`; `app/pautas/services.py:354`)
3. T2, de outro colaborador Y da mesma empresa, trava a âncora A e espera o contador DP. (`app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:116`; `app/chamados/services/fecho_runtime.py:121`; `app/chamados/services/fecho_runtime.py:127`)
4. T1 retorna da transição, pede A e espera T2; conserva DP na transação externa. A âncora é por empresa/competência e o contador é único por departamento, portanto X e Y podem ter travas C/D/Ch/P disjuntas e compartilhar A+DP. (`app/chamados/services/fecho_runtime.py:147`; `app/chamados/services/fecho_runtime.py:154`; `app/chamados/services/fecho_runtime.py:116`; `app/pautas/models.py:167`)

O estado necessário é estruturalmente permitido: a célula do chamado fornece o dia ao cartório; as perguntas ancoradas podem ter outros dias, sem igualdade exigida pelo filtro. O próprio teste existente ancora perguntas de dois dias no mesmo chamado. Pode-se ter célula do chamado sem turno e pergunta viva exportada em outra célula. Configuração concreta e execução concorrente: **[nao verificado]**. (`app/chamados/juizes.py:98`; `app/chamados/models.py:344`; `app/chamados/tests/test_e4b_c6_orm.py:74`; `app/chamados/tests/test_e4b_c6_orm.py:75`; `app/chamados/services/fabrica_da_celula.py:201`)

**Ordens encontradas:**

| Caminho | Ordem relevante |
|---|---|
| Runtime sem emissão anterior no signal | C→D→Ch→P eventualmente→âncoras ordenadas→INSERT Pauta→trilha→contador DP. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`; `app/chamados/services/fecho_runtime.py:120`; `app/pautas/services.py:180`; `app/pautas/services.py:188`) |
| Runtime com Pauta da fábrica no signal | C→D→Ch→eventuais P/célula→Pauta da fábrica→DP→âncoras E4B→Pauta E4B→DP. (`app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:369`; `app/chamados/services/fecho_runtime.py:154`) |
| `pauta_cadastro_sem_turno` isolado | C→INSERT Pauta→DP; não adquire âncora E4B. (`app/chamados/services/fabrica_da_celula.py:364`; `app/chamados/services/fabrica_da_celula.py:365`; `app/chamados/services/fabrica_da_celula.py:369`) |
| Pauta pago/devido exportada | C→FechamentoMensal→Pauta cabeça/filhas→DP. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`) |
| Passivo validado | Plantio→regeneração→UPDATE FechamentoMensal→Pauta DP; possíveis travas anteriores dependem das portas de plantio/regeneração. (`app/chamados/services/passivo_validado.py:371`; `app/chamados/services/passivo_validado.py:427`; `app/chamados/services/passivo_validado.py:438`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:1160`; `app/chamados/services/passivo_validado.py:443`) |
| Estado de Pauta | UPDATE Pauta→trilha→contador; nenhuma âncora E4B nessa porta. (`app/pautas/services.py:215`; `app/pautas/services.py:218`; `app/pautas/services.py:247`; `app/pautas/services.py:251`; `app/pautas/services.py:272`; `app/pautas/services.py:276`; `app/pautas/services.py:294`; `app/pautas/services.py:297`) |
| Lavra avulsa | Contador somente, sem C/D/Ch/P/FM/advisory nesse comando. (`app/pautas/management/commands/lavrar_pautas.py:42`; `app/pautas/management/commands/lavrar_pautas.py:51`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) |

Não achei aquisição síncrona de FechamentoMensal na cauda de `cartorio.julgar_colab`. O recalculo por evento consultado é agendado em `on_commit`, portanto essa cauda não demonstra DP→FM retidos juntos. (`app/ponto/portas/celula.py:445`; `app/ponto/portas/celula.py:458`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1824`)

O teste c8 observa diretamente `_escrever_pautas`, deixando fora justamente a transição e seus receivers; por isso pode continuar verde diante dessa inversão global. (`app/chamados/tests/test_e4b_c6_orm.py:265`; `app/chamados/services/fecho_runtime.py:151`; `app/chamados/services/fecho_runtime.py:154`)

Outros pontos de chamada de `escrever`, sem aquisição própria da âncora E4B nos trechos consultados, seguem pela ordem interna INSERT Pauta→trilha→contador do destinatário. (`app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`)

- Interface e respostas: `app/pautas/views.py:76`, `app/pautas/services.py:194`, `app/pautas/management/commands/suporte_hasner.py:133`.
- Emissores DP: `app/pautas/semanal.py:276`, `app/folha/management/commands/pre_fechamento.py:110`, `app/ferias/management/commands/pautas_de_ferias.py:167`, `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107`, `app/ponto/services/ausencia.py:1055`, `app/ponto/services/ausencia.py:1245`, `app/ponto/services/pautas_dos_discordantes.py:177`, `app/ponto/services/pautas_dos_deslogados.py:77`, `app/ponto/services/afastado_avisa.py:74`, `app/ponto/services/autorizacao_he_periodo.py:244`.
- Outros departamentos: `app/ponto/services/pautas_do_esmeril.py:76`, `app/ponto/services/cobertura.py:333`, `app/core/services/crons_quebrados.py:71`, `app/core/management/commands/alarme_esteira.py:54`, `app/core/management/commands/alarme_arvore.py:50`.
- Chamados/comandos: `app/chamados/services/resposta_tardia.py:63`, `app/chamados/services/competencia_trancada.py:231`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:127`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`, `app/pautas/management/commands/pauta_dia_em_aberto.py:142`.

No censo produtivo consultado, não achei outro SQL de escrita de `ContadorPauta` além da porta central, nem outra aquisição da advisory E4B. Isso não torna a ordem global segura, porque os emissores anteriores compartilham o contador. (`app/pautas/services.py:352`; `app/pautas/services.py:354`; `app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:121`)

## 11/11 15:21 · sessao c3c87e94559f · - · - · origem=Ronald (tmux)

1. **O gate ORM observa a ordem real de emissão do SQL**, pois `execute_wrapper` registra a consulta e depois chama `execute`; não é somente contagem. Ele compara os parâmetros exatos das advisory locks, na ordem esperada, exige INSERT/UPDATE/exists e põe a última advisory antes dessas operações. **Travar somente a primeira âncora ou inverter as âncoras dos lotes presentes falha.** (`app/chamados/tests/test_e4b_c6_orm.py:249`; `app/chamados/tests/test_e4b_c6_orm.py:265`; `app/chamados/tests/e4b_c6_verificar.py:90`; `app/chamados/tests/e4b_c6_verificar.py:94`; `app/chamados/tests/e4b_c6_verificar.py:99`)

2. **A prova não cobre toda aquisição da trava do contador.** O classificador reconhece `UPDATE pautas_contadorpauta`, mas ignora `SELECT ... FOR UPDATE` nesse contador. Um mutante que pretrave a linha DP por SELECT e depois execute normalmente todas as âncoras passa nesse gate; também não há observação de outros chamadores, porque o teste chama diretamente `_escrever_pautas`. (`app/chamados/tests/e4b_c6_verificar.py:78`; `app/chamados/tests/e4b_c6_verificar.py:84`; `app/chamados/tests/e4b_c6_verificar.py:88`; `app/chamados/tests/test_e4b_c6_orm.py:266`)

3. **Outra chave de ordenação pode sobreviver.** Os lotes de memória usam exclusivamente empresa 2, e os lotes ORM exclusivamente `self.emp`; ordenar só pelo sufixo da competência produz exatamente a mesma sequência nesses casos, embora possa produzir outra ordem com várias empresas. A ordem produtiva atual é pelo texto completo da âncora. (`app/chamados/tests/test_e4b_c6_memoria.py:227`; `app/chamados/tests/test_e4b_c6_memoria.py:233`; `app/chamados/tests/test_e4b_c6_orm.py:253`; `app/chamados/tests/test_e4b_c6_orm.py:257`; `app/chamados/services/fecho_runtime.py:120`)

4. **Confirmações locais, somente em memória:** sobrepus o módulo sem gravar arquivos e executei o teste c8 com dois mutantes adicionais: SELECT FOR UPDATE do contador antes das âncoras; ordenação somente pela competência. Ambos sobreviveram. A simulação isolada do gate também aceitou a trava antecipada do contador. Não há arquivo de saída dessas execuções: **[nao verificado]**. A razão está no classificador e nos insumos descritos acima. (`app/chamados/tests/e4b_c6_verificar.py:78`; `app/chamados/tests/test_e4b_c6_memoria.py:223`)

5. **O fake não certifica banco nem rollback.** Seu `atomic` apenas incrementa/decrementa profundidade; `execute` só guarda registros; o escritor c8 emite SQL sintético e acrescenta à lista. Isso explica por que seu GREEN não elimina inversões envolvendo outras travas reais. O teste ORM executa a porta real, mas usa perguntas `SimpleNamespace`, uma conexão e chamada direta ao agrupador. (`app/chamados/tests/test_e4b_c6_memoria.py:36`; `app/chamados/tests/test_e4b_c6_memoria.py:46`; `app/chamados/tests/test_e4b_c6_memoria.py:207`; `app/chamados/tests/test_e4b_c6_orm.py:253`; `app/chamados/tests/test_e4b_c6_orm.py:266`)

6. **Estar em atomic não prova a identidade nem a duração da mesma transação.** O gate só recebe um booleano; a classe ORM é `TestCase`, e o fake recebe apenas profundidade. Não achei verificação de release/commit entre fases nem ensaio com duas conexões nesses testes. (`app/chamados/tests/e4b_c6_verificar.py:76`; `app/chamados/tests/e4b_c6_verificar.py:95`; `app/chamados/tests/test_e4b_c6_orm.py:28`; `app/chamados/tests/test_e4b_c6_memoria.py:47`)

7. **Não achei fixture invisível determinando o resultado dos REDs funcionais.** Viva/mortas e exportada/aberta são verificadas pelos juízes reais antes do ato; a conclusão ORM confere carimbos, estados, ausência de reabertura e Pauta persistida. A conversa é criada pela porta, mas sua recusa é injetada diretamente no agrupador: a prova é daquele leitor, não do caminho S143 completo. (`app/chamados/tests/test_e4b_c6_orm.py:78`; `app/chamados/tests/test_e4b_c6_orm.py:82`; `app/chamados/tests/test_e4b_c6_orm.py:113`; `app/chamados/tests/test_e4b_c6_orm.py:120`; `app/chamados/tests/test_e4b_c6_orm.py:175`; `app/chamados/tests/test_e4b_c6_orm.py:182`)

8. **Não achei RED contado por fixture quebrada no protocolo.** Ele exige GREEN anterior, rc=1, um failure no caso e mensagem-alvo, e rejeita erro ou precondição. Os testes do protocolo usam resultados fabricados para testar esse parser; não são prova da execução dos mutantes. O `--c8` agrega os ensaios c7 e os dois c8; não agrega os ensaios c6, que o dado fornecido apresenta separadamente. (`app/chamados/tests/e4b_c6_verificar.py:242`; `app/chamados/tests/e4b_c6_verificar.py:255`; `app/chamados/tests/test_e4b_c6_protocolo.py:91`; `app/chamados/tests/e4b_c6_verificar.py:230`)

9. **Execução local:** c8 memória mais protocolo: 9 OK. Ao executar os 24 testes memória/protocolo, houve um único erro por ausência de `.git`, no teste que pede `git show` da base; não é evidência de regressão produtiva nem contradiz o GREEN fornecido de outro ambiente. Saídas sem arquivo: **[nao verificado]**. Dependência está explícita. (`app/chamados/tests/e4b_c6_verificar.py:102`; `app/chamados/tests/test_e4b_c6_memoria.py:281`)

## 11/11 15:20 · sessao ab3c1bbff372 · - · - · origem=Ronald (tmux)

Classificação dos achados c7, excluindo conclusão global de travas delegada ao outro explorer:

| Achado c7 | Situação c8 e evidência |
|---|---|
| Dia civil de conversa virando competência no agrupador | **Curado pela CLASSE desse leitor**: toda pergunta passa pelo juiz, `None` sai, e a competência vem da janela da empresa. (`app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:112`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/juizes.py:336`) |
| Duas Pautas concorrentes marcador+âncora | **Curado pela CLASSE dos participantes que passam por esse agrupador**, condicionado à ordem global de travas: advisory antes de dedup/criação, no atomic. A porta isolada continua criando e o modelo não possui unicidade marcador+âncora. (`app/chamados/services/fecho_runtime.py:119`; `app/chamados/services/fecho_runtime.py:121`; `app/chamados/services/fecho_runtime.py:123`; `app/pautas/services.py:180`; `app/pautas/models.py:123`) |
| Motivo limbo truncado | **Curado pela CLASSE dos formatos produzidos pelo S143**: reserva nomes, abrevia listas inteiras com contagem; teste compara entrada da porta e persistido. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:226`; `app/chamados/tests/test_e4b_c6_orm.py:228`) |
| Memória sobrevivendo a rollback do S143 | **Curado pela CLASSE de exceções propagadas pelo resolver ou saída do savepoint**: só incorpora o lote após os contextos saírem normalmente, restaurando token no `finally`. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`) |
| Reentrega não acrescenta novos IDs | **Não curado — FILA**: Pauta viva leva a `continue`; nota S143 preserva rastro individual. (`app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamentos futuros fora das duas bordas sem entrega | **Não curado — FILA de alcance**: `fechar` chama transição direta; coletor ausente não entrega; só achei instalação nas duas bordas. (`app/chamados/models.py:454`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`) |
| Relógio fora da borda | **Não curado — FILA preexistente**: S143 fornece `None`; juiz pode ler relógio. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Falta de prova concorrente e falha real da escrita | **Não curado — FILA de prova**: c8 observa SQL de uma conexão chamando diretamente agrupador; erro de memória é na transição e erro ORM é no resolver, não em `escrever`. (`app/chamados/tests/test_e4b_c6_orm.py:248`; `app/chamados/tests/test_e4b_c6_orm.py:263`; `app/chamados/tests/test_e4b_c6_memoria.py:113`; `app/chamados/tests/test_e4b_c6_orm.py:273`) |
| Suíte inteira | **Não curado — BLOQUEANTE de encerramento**, pela regra vigente e dado fornecido pelo usuário; execução integral independente **[nao verificado]**. (`CLAUDE.md:375`; `app/docs/FIO-E0-V4-E4B-c8.txt:80`) |
| Lint vermelho | Import excedente removido e variável ligada ao default do contexto; GREEN do anexo fornecido, execução própria **[nao verificado]**. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_memoria.py:297`) |

Mapa de escritores/leitores do percurso produtivo alcançado:

| Campo/valor | Produção/escrita | Leitura/significado |
|---|---|---|
| `_RECUSAS`, token, lote | Declaração, instalação e restauração; lote mesclado após sucesso. (`app/chamados/services/fecho_runtime.py:13`; `app/chamados/services/fecho_runtime.py:34`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`; `app/chamados/services/fecho_runtime.py:148`; `app/chamados/services/fecho_runtime.py:153`) | Coletor compara PK do chamado e indexa por PK da pergunta; transição entrega dicionário ao agrupador. (`app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/fecho_runtime.py:20`; `app/chamados/services/fecho_runtime.py:154`) |
| Motivo recusa / IDs limbo | S143 produz os dois formatos e grava nota do credor. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1844`; `app/chamados/services/disputa_emissao.py:1855`; `app/chamados/services/disputa_emissao.py:1860`) | Coletor recebe apenas prefixos admitidos; montador interpreta nomes, lista limbo e IDs de pergunta. (`app/chamados/services/disputa_emissao.py:1946`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:65`; `app/chamados/services/fecho_runtime.py:69`; `app/chamados/services/fecho_runtime.py:77`) |
| `data_evento` / `conversa` | `abrir` fornece dia do turno e `False`; conversa fornece dia civil e `True`; nascimento recebe discriminador e usa atribuidor único de data. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/models.py:3014`; `app/chamados/models.py:3016`; `app/chamados/services/fio.py:801`) | Juiz retorna `None` para conversa e agrupador o usa. **Leitores crus permanecem** no teste de exportada de S143, via S128/HX e gate do chamado. (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`) |
| Empresa / corte | Relação do colaborador; portas de cadastro do colaborador e empresa; corte cadastrado. (`app/colaboradores/models.py:321`; `app/colaboradores/services/colaborador.py:60`; `app/colaboradores/services/colaborador.py:76`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Agrupador usa empresa da pergunta; `janela_atual` consulta corte e deriva janela por corte, não mês civil. (`app/chamados/services/fecho_runtime.py:114`; `app/ponto/janelas.py:50`; `app/ponto/janelas.py:60`; `app/ponto/janelas.py:24`) |
| Competência / âncora / marcador / chave | Agrupador usa ano/mês do fim da janela; chave SHA256 de marcador+tipo+âncora. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/services/fecho_runtime.py:116`) | Dedup usa par+marcador; porta conserva ID como texto; chip/payload conservam par. Não achei nesses leitores reinterpretação como mês civil. (`app/chamados/services/fecho_runtime.py:123`; `app/pautas/services.py:184`; `app/pautas/services.py:336`; `app/pautas/views.py:40`) |
| Texto | Montador reserva 500 caracteres; porta persiste com teto. (`app/chamados/services/fecho_runtime.py:76`; `app/chamados/services/fecho_runtime.py:79`; `app/pautas/services.py:185`) | Payload DP entrega texto; motivo completo permanece na nota. (`app/pautas/views.py:40`; `app/chamados/services/disputa_emissao.py:1855`) |
| DP/TI/autor | Runtime fornece DP/TI e usa `autor_sistema`; porta resolve remetente, valida e persiste. (`app/chamados/services/fecho_runtime.py:127`; `app/pautas/services.py:87`; `app/pautas/services.py:49`; `app/pautas/services.py:157`; `app/pautas/services.py:180`) | Listagem usa destinatário; autor é trilha e não viaja no payload. (`app/pautas/services.py:308`; `app/pautas/models.py:105`; `app/pautas/views.py:38`) |
| `feito_em` / `cancelado_em` | Portas centrais de estado. (`app/pautas/services.py:247`; `app/pautas/services.py:272`; `app/pautas/services.py:294`) | Dedup usa `.vivas()`; feita/cancelada permite nova entrega. (`app/chamados/services/fecho_runtime.py:124`; `app/pautas/models.py:57`; `app/pautas/models.py:64`) |
| Contador DP | `escrever` chama `lavrar`; esta obtém alvo e atualiza não lidas, atrasadas e instante. (`app/pautas/services.py:188`; `app/pautas/services.py:352`; `app/pautas/services.py:354`) | `nao_lidas` lê lavrado; view o usa. (`app/pautas/services.py:362`; `app/pautas/views.py:34`) |
| `status_local`, destino e usuário | Bordas transportam; `_gravar_estado` atribui e salva. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:228`; `app/chamados/models.py:231`; `app/chamados/models.py:296`) | Adaptador exige VIVOS→ENCERRADOS; receiver exige estado encerrado e save que escreveu status. (`app/chamados/services/fecho_runtime.py:144`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`) |

Das classes históricas, **campo temporal com dois significados e relógio fora da borda permanecem no caminho completo**; c8 não acrescenta juiz de vitalidade, fechamento ou competência e mantém escrita pela porta. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/juizes.py:577`; `app/chamados/services/fecho_runtime.py:104`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:127`)

A retirada do retorno antecipado continua: após registrar recusa, seguem S128, HX e juiz C7. Não achei novo retorno anulando esses passos. (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1953`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2046`; `app/chamados/services/disputa_emissao.py:2089`)

Não achei emissão E4B fora do runtime nos chamadores indicados: eles chamam resolver, enquanto a coleta exige contexto instalado pelas duas bordas. Isso não constitui sonda de produção. (`app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/fecho_runtime.py:149`)

Limitação de apresentação preexistente: UI não oferece rota de competência; conserva âncora e avisa ao clicar. Não é interpretação como mês civil. (`app/pautas/views.py:40`; `app/templates/core/_copiloto.html:564`; `app/templates/core/_copiloto.html:570`)

## 11/11 15:20 · sessao b649f707e734 · PROPORCAO-POR-RISCO · verificador · origem=Code

REPROVADO

1. **Ainda há configuração de portão classificada B.** `app/core/tests/test_contract_esmeril.py:15` fornece `VULTURE_IGNORE_NAMES`; `bin/esmeril_da_copia.sh:22` lê esse valor, `:24` aplica as exceções e `:27` decide se libera. O arquivo continua B por `bin/selo_proporcao.py:74`. A mesma dependência existe em `bin/molde_fatia/rodar.sh:20-22`.

2. **A lista fechada não sobrevive ao seu escritor oficial.** `CORTES.md` se declara gerado (`app/docs/CORTES.md:3`), mas a geração a partir do JSON fornecido não reproduz os marcadores presentes em `app/docs/CORTES.md:444`. O gerador monta as linhas do JSON e sobrescreve o Markdown inteiro (`bin/gerar_cortes.py:106-116`). Incorporar essa regeneração numa base futura faz o selo retornar rc 2 por lista ausente (`bin/selo_proporcao.py:50-52`).

3. **A guarda prova igualdade com o `main` local, não a “obra inteira”.** Um `main` local que já contém C1 permite julgar somente C1..C2, mesmo que C1 pertença à mesma obra ainda não publicada. O selo não consulta `origin/main` nem uma fronteira registrada da obra (`bin/selo_proporcao.py:83-100`).

4. **O harness ainda pode certificar cobertura que não ocorreu.** Três negativos novos continuam verdes sem a guarda pretendida; além disso, falhar o segundo commit de `recorte` pode produzir os dois rc esperados por motivos diferentes (`bin/tests/test_selo_proporcao.sh:64-67`, `:111-114`; `bin/selo_proporcao.py:68-76`, `:92-98`).

A correspondência Git da cópia com o hash informado e os estados reais das branches são [nao verificado]. Não executei o harness completo, que cria arquivos e repositórios (`bin/tests/test_selo_proporcao.sh:11-17`); as contraprovas abaixo foram conferidas por leitura e avaliação somente em memória das funções citadas.

**1. B1..B5**

| Achado | Resultado da v7 | Evidência |
|---|---|---|
| **B1 — `test_*.py` em qualquer pasta** | **Cura os exemplos concretos.** Migration e management recebem A; basename sozinho também deixou de bastar. | `bin/selo_proporcao.py:68-75`. |
| **B2 — produção sob `tests/`** | **Cura em parte.** `marca_cron15.py` agora recebe A, mas produção chamada `test_*.py` sob `tests/` continua B. Há consumidor concreto dessa forma. | `app/core/management/commands/crons.py:92`; `bin/selo_proporcao.py:74`; `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`. |
| **B3 — dados de portão sob `bin/tests/`** | **Cura os caminhos denunciados; cura parcial da categoria.** `bin/tests/*` fica A, mas a lista de exceções do esmeril permanece em arquivo B. | `bin/selo_proporcao.py:66-67`; `app/core/tests/test_contract_esmeril.py:15`; `bin/esmeril_da_copia.sh:22-27`. |
| **B4 — base livre/recorte** | **Cura em parte.** Fecha C1..C2 quando `main` continua na base original; não fecha recorte já absorvido pelo `main` local. | `bin/selo_proporcao.py:96-100`; `bin/tests/test_selo_proporcao.sh:110-114`. |
| **B5 — contrato/comentário e integração inexistente** | **Cura documental parcial.** O comentário passa a dizer BASE. Não nasce chamador de pouso nem contador: as chamadas concretas continuam no harness, e o selo apenas imprime/retorna. A conduta manual prometida é [nao verificado]. | `bin/tests/test_selo_proporcao.sh:101`, `:42`, `:52`; `bin/selo_proporcao.py:110-119`. |

O harness tem chamador de host pelo laço genérico da régua; isso testa repositórios sintéticos, sem substituir uma chamada sobre o intervalo real do pouso (`bin/regua.sh:138-143`; `bin/tests/test_selo_proporcao.sh:13`, `:42`).

**2. Caminhos ainda B que têm consumidores efetivos**

| Caminho | Quem lê/carrega |
|---|---|
| **`app/core/tests/test_contract_esmeril.py`** | Define a lista de exceções em `:15`; consumida pelos portões em `bin/esmeril_da_copia.sh:22-27` e `bin/molde_fatia/rodar.sh:20-22`. B por `bin/selo_proporcao.py:74`. |
| **`app/core/tests/test_contract_tblib_no_paralelo.py`** | Define `tblib_instalado()` em `:24`; importado pelo command em `app/chamados/management/commands/tblib_ausente_na_imagem.py:17-18`. O host executa esse command e publica o contador em `bin/placar_code.sh:463-465`. B por `bin/selo_proporcao.py:74`. |
| **`app/static/pwa.js`** | Carregado pelas duas cascas (`app/templates/base.html:20`; `app/templates/base_app.html:20`). Grava batidas offline e envia POST ao backend (`app/static/pwa.js:112-119`, `:153-157`). B por `bin/selo_proporcao.py:72`. |
| **`app/static/sw.js`** | A rota aponta para a view em `app/core/urls.py:24`; a view lê e serve o JavaScript em `app/core/views.py:570-576`. B por `bin/selo_proporcao.py:72`. |
| **`app/static/manifest.json`, `app/static/icons/icon-192.png`, `app/static/icons/icon-512.png`**, além de `pwa.js` e `sw.js` | Participam do identificador do build: nomes em `app/core/views.py:524-525`, leitura em `:548-552`. A seleção efetiva entre `/app/staticfiles` e `/app/static` no host é [nao verificado]; a escolha está em `:528-530`. B por `bin/selo_proporcao.py:72`. |
| **`app/templates/base.html`, `base_app.html`, `ponto/espelho.html`, `colaboradores/detalhe.html`, `chamados/painel_gestao.html`** | O portão declara essas páginas e as compila (`bin/prova_de_casca.py:21-22`, `:49-53`); também lê os HTML da árvore (`:30-36`). O deploy chama o portão antes de recarregar (`bin/deploy.sh:295-297`). B por `bin/selo_proporcao.py:70`. |

O exemplo antigo do cron continua sendo código de produção importado de `tests/`, mas **já não é B**: `marca_cron15.py` é importado em `app/core/management/commands/crons.py:92` e não satisfaz o nome exigido por `bin/selo_proporcao.py:74`.

**3. Situações concretas de `base_main`**

Os grafos seguintes são contraprovas do algoritmo; sua ocorrência no pouso real é [nao verificado]. A comparação responsável é `bin/selo_proporcao.py:96-98`.

| Situação | Caso e resultado |
|---|---|
| **`main` local atrasado** | Cadeia `O → M → H`; `main=O`, `origin/main=M`, raia nasceu em M e H muda só template. Passar base=M, adequada ao destino remoto, dá **rc 2**, pois o selo calcula O. É falso relativamente ao pouso remoto; segue literalmente o contrato que escolheu `main` local (`bin/selo_proporcao.py:94-98`). |
| **`main` local adiantado** | Cadeia `O → C1 → C2`; `origin/main=O`, `main=C1`, head=C2. C1 da mesma obra altera `views.py`; C2 só template. Base=C1 passa e o diff omite C1: **B/rc 0**, supondo lista válida (`bin/selo_proporcao.py:96-100`, `:113-119`). |
| **Worktree** | Estar numa worktree não causa erro por si: o Git recebe sua raiz via `-C`, e a raiz é validada por `realpath`. Porém, numa cópia com apenas `origin/main`, sem referência `main` resolvível, o merge-base falha e vira **rc 2** (`bin/selo_proporcao.py:43-45`, `:85-87`, `:96`, `:104-106`). |
| **Raia que recebeu merge de `main`** | O bifurca para C1 na raia e M no main; R mescla C1 e M; H acrescenta template. Com main=M, base=M passa e **o saldo de C1 continua no diff**. Passar a base original O dá rc 2, embora seja intervalo mais amplo, não recortado. Receber merge de main, sozinho, não esconde o escritor (`bin/selo_proporcao.py:96-100`). |

Se C1 já entrou no `main` ou seu efeito foi revertido em H, o diff de pontas não mostra esse efeito. O selo examina nomes entre duas árvores, não a sequência completa de alterações da obra (`bin/selo_proporcao.py:100-101`).

**4. Os casos novos mordem?**

“Continua verde” abaixo significa que o harness aceita o resultado mesmo com a proteção pretendida removida; a classificação decorre de `bin/selo_proporcao.py:64-76`.

| Caso no harness | Contraprova | Avaliação |
|---|---|---|
| `tests_ok`, `:61` | Retirar a aceitação de teste muda rc 0 para 1. | **Morde.** | [nao verificado]
| `tests_helper`, `:62` | Aceitar qualquer arquivo em `tests/` muda rc 1 para 0. | **Morde a exigência de nome.** |
| `test_basename`, `:63` | Aceitar basename sem pasta `tests/` muda rc 1 para 0. | **Morde a exigência de pasta.** |
| `migration_test_nome`, `:64` | Sem `carregado_por_pasta`, continua rc 1 porque falta `tests/` no caminho do arquivo. O `mkdir` não altera isso. | **Continua verde; não isola a guarda.** | [nao verificado]
| `migration_tests_dir`, `:65` | Sem a guarda de pasta, vira teste permitido e rc 0. | **Morde.** | [nao verificado]
| `commands_test_nome`, `:66` | Sem a guarda de pasta, continua rc 1 porque falta `tests/`. | **Continua verde; não prova o braço `management`.** | [nao verificado]
| `bin_tests`, `:67` | Sem `fora_app`, `base.txt` continua fora: falta nome permitido. | **Continua verde; não isola a guarda.** | [nao verificado]
| `bin_tests_test_py`, `:68` | Sem `fora_app`, vira teste permitido e rc 0. | **Morde.** | [nao verificado]
| `templates_fora_app`, `:69` | Sem `fora_app`, vira template permitido e rc 0. | **Morde.** |
| `recorte`, `:110-114` | Com C2 preparado corretamente, remover `base_main` troca o primeiro rc 2 por 0. | **Morde nominalmente.** |

As linhas da primeira coluna são de `bin/tests/test_selo_proporcao.sh`; os vereditos rc 1 estão em `bin/selo_proporcao.py:116-118`.

Há três problemas adicionais:

- **Recorte verde por outro motivo:** `:112` não verifica sucesso nem avanço de HEAD. Se C2 não nascer, HEAD=C1; a primeira chamada dá rc 2 por igualdade, e a segunda dá rc 1 por `views.py`. Os resultados esperados aparecem sem exercitar `base_main` (`bin/tests/test_selo_proporcao.sh:111-114`; `bin/selo_proporcao.py:92-93`, `:116-118`).
- **Cobertura antiga mascarada:** retirar `ancestral` ainda deixa `revisoes_trocadas` e `nao_ancestral` com rc 2 pela guarda nova. O caso de igualdade tem conferência adicional da mensagem; aqueles dois não têm (`bin/tests/test_selo_proporcao.sh:128-133`; `bin/selo_proporcao.py:94-98`).
- **Lista e omissão silenciosa:** mover `main` para a base defeituosa evita rejeição antecipada, mas `caso_lista` ainda confere só rc 2. O caso `sem_repo_no_repo_do_script` continua condicionado à existência de histórico, sem aviso de omissão (`bin/tests/test_selo_proporcao.sh:51-53`, `:141-143`).

A mudança de `shell_bin` acrescenta preparo de diretório; não constitui prova independente de uma guarda nova (`bin/tests/test_selo_proporcao.sh:80`; `bin/selo_proporcao.py:66-76`).

**5. Quem escreve e quem lê**

| Valor | Escritor/fornecedor | Leitor |
|---|---|---|
| **Lista persistida** | Autoridade declarada: Ronald (`app/docs/CORTES.md:444`). Escritor mecânico: JSON → `markdown()` → sobrescrita de CORTES (`bin/gerar_cortes.py:23`, `:32`, `:106-116`). Quem acrescentou diretamente a linha atual é [nao verificado]. | `git show base:CORTES`, parser e pertinência (`bin/selo_proporcao.py:50-58`, `:64`). |
| **Lista sintética** | Harness escreve e commita sua lista (`bin/tests/test_selo_proporcao.sh:17`, `:22`, `:26`); entorta bases em `:102-109`. | Mesmo selo, chamado em `:42` e `:52`. |
| **Base, head, classe e repo** | Argumentos externos (`bin/selo_proporcao.py:83`); fornecedor concreto no harness (`bin/tests/test_selo_proporcao.sh:42`). Fornecedor efetivo no pouso é [nao verificado]. | Validação da raiz, resolução das revisões, guardas e veredito (`bin/selo_proporcao.py:85-100`, `:116`). |
| **Referência `main` / merge-base** | No harness, `main` é criado/movido por `git branch` (`bin/tests/test_selo_proporcao.sh:29`, `:51`, `:53`). Escritor real da referência é [nao verificado]. Git calcula o merge-base (`bin/selo_proporcao.py:96`). | Igualdade com base (`bin/selo_proporcao.py:97`). |
| **Nomes do diff** | Git produz nomes com `--no-renames --name-only -z`; Python deduplica e ordena (`bin/selo_proporcao.py:100-101`). | Guarda de vazio e classificador (`:102`, `:108-112`). Não há leitura do conteúdo para decidir classe (`:61-76`). |
| **Resultado/rc** | Python imprime e retorna (`bin/selo_proporcao.py:110-119`), codifica o protocolo em `:129`; wrapper normaliza (`bin/selo_proporcao.sh:8-11`). | Harness confere rc (`bin/tests/test_selo_proporcao.sh:42-43`, `:113-114`); régua recebe o resultado do harness (`bin/regua.sh:140-143`). |
| **Linha de medida no RELATO / contador** | O selo não os escreve: termina em stdout/rc (`bin/selo_proporcao.py:110-119`). Escrita manual efetiva e consumo no pouso são [nao verificado]. | Não há consumidor implementado demonstrado pelo fluxo citado; ocorrência externa [nao verificado]. |

**6. Contrato e implementação**

- **“Qualquer `.md` faz A” é absoluto demais.** Um caminho como `app/templates/nota.md` seria B pela guarda de templates. O caso exercitado cobre apenas `app/docs/NOTA.md` (`bin/selo_proporcao.py:70-71`; `bin/tests/test_selo_proporcao.sh:78`).
- **“Obra inteira” excede a garantia implementada.** O código garante somente base igual ao merge-base do `main` local, seguido de diff de pontas (`bin/selo_proporcao.py:96-101`).
- **A implementação aceita classe A e pode retornar rc 0 mesmo com arquivos FORA.** Isso está no uso do código, enquanto o contrato fornecido diz que obra A não chama (`bin/selo_proporcao.py:4`, `:80`, `:116-119`).
- **Os comentários ainda divergem.** O harness diz `tests/ OU test_*.py`, mas o código exige ambos; CORTES ainda descreve selo em linhas adicionadas/escrita (`bin/tests/test_selo_proporcao.sh:4`; `bin/selo_proporcao.py:74`; `app/docs/CORTES.md:443`).
- **A lista tem precedência sobre as proibições.** O contrato fornecido admite lista OU filtros, o que corresponde ao código. Porém, os comentários “só app” e “management/migrations A sempre” não são absolutos: um item previamente listado retorna antes dessas guardas (`bin/selo_proporcao.py:13-17`, `:64-69`).
- **Os limites conhecidos estão documentados no código, mas permanece outro consumidor relevante:** produção importando teste e PWA aparecem no comentário; a configuração do esmeril consumida por `bin/` permanece B (`bin/selo_proporcao.py:25-26`; `app/core/tests/test_contract_esmeril.py:15`; `bin/esmeril_da_copia.sh:22-27`).

## 11/11 15:18 · sessao ab98b69c3c6e · - · - · origem=Ronald (tmux)

Perguntas 3, 5 e 6 — resultado da exploração, sem gravações.

1. `base_main` amarra ao `main` local, não à base de pouso no remoto nem ao início registrado da obra: `bin/selo_proporcao.py:96-98`. Cenários concretos derivados do algoritmo; ocorrência real nesta cópia [nao verificado], pois não há `.git`.

   - **Falso rc2 com main atrasado:** cadeia `O → M → H`; `main=O`, `origin/main=M`, raia criada de `M`, `H` só altera template. O intervalo correto para pousar sobre o remoto é `M..H`. O selo aceita que `M` seja ancestral (`:94`), mas calcula `merge-base(main,H)=O` e devolve 2 (`:96-98`).
   - **Recorte aceito com main adiantado:** cadeia `O → C1 → C2`; `origin/main=O`, `main=C1`, raia `head=C2`; C1 da mesma obra altera `views.py`, C2 só template. Passar `base=C1` satisfaz a guarda e compara somente C1..C2 (`:100`): B/rc0, omitindo o escritor de C1. Não há leitura de `origin/main` nem identidade da obra em `:83-103`.
   - **Worktree, isoladamente, não causa erro:** a raiz da worktree passa na comparação de `realpath` e o Git é chamado com `-C repo` (`:41-43`, `:85-87`). Uma worktree/clone com apenas `origin/main`, sem uma referência `main` resolvível, falha no `merge-base` por `git_rc`, rc2 (`:44-45`, `:96`, `:104-106`).
   - **Raia que recebeu merge do main não recorta automaticamente:** `O→C1` na raia e `O→M` no main; merge `R=(C1,M)`, seguido de template H, com main=M. A base aceita é M e o diff M..H ainda inclui o saldo de C1 (`:96-100`). Se o mesmo C1 já estiver no main, ou se seu efeito for revertido no head, o diff de pontas não o mostra; o código examina árvores, não todos os commits (`:100-101`). Não chamar isso de perda automática causada por qualquer merge. [nao verificado]

2. **Produtor da lista persistida encontrado e problema de durabilidade:**

   - `app/docs/CORTES.md:3` declara arquivo gerado de `CORTES.json`.
   - O produtor real é `bin/gerar_cortes.py:23-24`, que lê o JSON em `:31-32`, transforma cada entrada em linha em `:106-109` e **sobrescreve integralmente** o Markdown em `:114-116`.
   - A lista hoje está em `app/docs/CORTES.md:444`.
   - O JSON fornecido não contém `PROPORCAO-POR-RISCO` nem os marcadores; termina em `app/docs/CORTES.json:1367`. Verificação somente em memória, chamando `markdown(cortes)` com `python3 -I -B`: Markdown atual tem 1 início/1 fim; Markdown regenerado tem **0 início/0 fim**. Nenhum arquivo foi escrito.
   - Consequência concreta condicional: executar legitimamente o gerador e incorporar o resultado apaga a lista; uma base posterior assim produz rc2 no selo, em `bin/selo_proporcao.py:50-52`. Não afeta retroativamente uma base antiga, porque a leitura é por `git show`.
   - Autoridade humana declarada: módulos novos só por corte do Ronald, `app/docs/CORTES.md:444`; o parser não autentica autoria/corte, apenas formato e presença em base (`bin/selo_proporcao.py:49-58`).

3. **Mapa de escritores/leitores relevante à pergunta 5:**

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | CORTES persistido | `bin/gerar_cortes.py:116`, fonte `:23`, leitura `:32`, frase `:109`; autoridade `app/docs/CORTES.md:444` | `git show base:CORTES` em `bin/selo_proporcao.py:50`; parser `:51-58`; pertinência `:64` |
   | Lista sintética do harness | `bin/tests/test_selo_proporcao.sh:17`, valores `:22`; corrupções `:102-109`; autorização HEAD `:87-88`; disco `:119` | mesmo selo, chamado em `:42`, `:52`, `:120` |
   | `base/head/classe/repo` | argumentos externos `bin/selo_proporcao.py:83`; chamador concreto harness `bin/tests/test_selo_proporcao.sh:42` | revisões resolvidas `bin/selo_proporcao.py:89-91`; base/head `:92-100`; classe `:116`; repo `:85` |
   | `main` sintético | harness `bin/tests/test_selo_proporcao.sh:29`, `:51`, `:53` | `bin/selo_proporcao.py:96` |
   | merge-base | subprocesso Git em `bin/selo_proporcao.py:96` | igualdade com base `:97` |
   | nomes do diff | Git `diff --no-renames --no-ext-diff --name-only -z` em `bin/selo_proporcao.py:100`; ordenação/deduplicação `:101` | vazio `:102`; classificador `:108-112`; impressão `:110`, `:114-115` |
   | rc | `bin/selo_proporcao.py:116-119`, protocolo `:129`, tradução `bin/selo_proporcao.sh:8-11` | harness `bin/tests/test_selo_proporcao.sh:42-43`, `:52`, `:113-114`; pouso humano segundo contrato anexo [nao verificado] |
   | RELATO da medida | nenhum gravador do selo: termina imprimindo e retornando `bin/selo_proporcao.py:110-119` | integração de consumo do resultado no pouso [nao verificado] |

   O harness é executado pelo laço genérico de selos do host em `bin/regua.sh:138-143`; o atalho de régua já verde retorna antes em `:84-94`. Isso testa o instrumento em repositórios sintéticos, não julga o diff real de pouso.

4. **Contrato/comentários versus implementação:**

   - O contrato anexado admite lista **OU** filtros; código corresponde: lista retorna antes das restrições (`bin/selo_proporcao.py:64-69`). Contudo o comentário `:13` diz “só app” e “migrations/management A sempre”, o que não vale para um item previamente listado.
   - A frase absoluta “qualquer .md faz A” do contrato anexado tem exceção pelo algoritmo: hipotético `app/templates/nota.md` seria B (`bin/selo_proporcao.py:70-71`). `app/docs/NOTA.md`, caso efetivamente testado, é A (`bin/tests/test_selo_proporcao.sh:78`).
   - `bin/tests/test_selo_proporcao.sh:4` ainda descreve `tests/ OU test_*.py`; implementação exige ambos (`bin/selo_proporcao.py:74`).
   - `app/docs/CORTES.md:443` ainda descreve selo de linhas adicionadas/escrita; código julga nomes de arquivos (`bin/selo_proporcao.py:100`, `:61-76`); a linha B3 `app/docs/CORTES.md:444` já diz caminho.
   - O contrato agora admite ausência de script de pouso e de contador, corrigindo a promessa documental; não implementa integração/medida. A busca das referências encontra chamadas concretas no harness; a saída do selo permanece stdout/rc (`bin/selo_proporcao.py:110-119`), não escreve RELATO nem contador.

## 11/11 15:17 · sessao 8f4e55fbbe88 · - · - · origem=Ronald (tmux)

Análise somente em memória da função `motivo_b`; não executei o harness que cria arquivos (`bin/tests/test_selo_proporcao.sh:11`, `:13`, `:17`).

1. Casos novos/alterados do harness:

| Caso | Guarda pretendida | Se retirada/afrouxada | Avaliação |
|---|---|---|---|
| `tests_ok` (`:61`) | aceitação `teste` (`selo_proporcao.py:74`) | Retirar aceitação produz rc1 | Mata. | [nao verificado]
| `tests_helper` (`:62`) | exigir nome `test_*.py` (`:74`) | Aceitar qualquer arquivo em `tests/` produz rc0 | Mata o alargamento para pasta apenas. |
| `test_basename` (`:63`) | exigir pasta `tests/` (`:74`) | Aceitar basename apenas produz rc0 | Mata o alargamento para nome apenas. |
| `migration_test_nome` (`:64`) | `carregado_por_pasta` (`:68`) | Sem ela continua rc1, pois o arquivo não fica em `tests/` | Não isola; o `mkdir migrations/tests` não muda o caminho gravado. |
| `migration_tests_dir` (`:65`) | `carregado_por_pasta` (`:68`) | Sem ela produz rc0 por `teste` | Mata. | [nao verificado]
| `commands_test_nome` (`:66`) | `carregado_por_pasta`, braço `management` (`:68`) | Sem ela continua rc1, pois falta `tests/` | Não isola; nenhum caso novo mata a retirada só do braço `management`. |
| `bin_tests` (`:67`) | `fora_app` (`:66`) | Sem ela continua rc1: `base.txt` não atende `teste` | Não isola. | [nao verificado]
| `bin_tests_test_py` (`:68`) | `fora_app` (`:66`) | Sem ela produz rc0 por `teste` | Mata. | [nao verificado]
| `templates_fora_app` (`:69`) | `fora_app` (`:66`) | Sem ela produz rc0 por `templates` | Mata. |
| `recorte` (`:110-115`) | `base_main` (`:96-98`) | Na preparação nominal, retirar guarda dá rc0 no teste que exige2 | Mata nominalmente, mas tem falso positivo de preparo abaixo. | [nao verificado]

Os resultados da tabela seguem diretamente da prioridade das condições em `bin/selo_proporcao.py:64-76`, com rc1 em `:116-118`; confirmei os dois mutantes de função em memória.

2. Falsos positivos e mascaramento:

- **Segundo commit do recorte pode falhar e o caso continuar verde.** `:112` não verifica `echo`, `git add`, `git commit` nem avanço do HEAD. Se `c2` não nascer, HEAD=C1: a primeira chamada retorna2 por `base == head` (`bin/selo_proporcao.py:92-93`), e a segunda retorna1 pelo `views.py` (`bin/tests/test_selo_proporcao.sh:111-114`). Assim o nome “base_recortada” passa por outra guarda.
- **A guarda nova mascara cobertura da ancestralidade.** Desligando `GUARDA:ancestral`, `revisoes_trocadas` e `nao_ancestral` continuam rc2 pela comparação `base_main`, pois a base passada é HEADC enquanto `merge-base(main, head)` é BASE (`bin/tests/test_selo_proporcao.sh:128`, `:131-133`; `bin/selo_proporcao.py:94-98`).
- O caso `base_igual_head` também chegaria a `base_main` se `igual` fosse removida, mas a checagem adicional do texto identifica isso (`bin/tests/test_selo_proporcao.sh:129-130`).
- Os testes de lista continuam verificando somente rc2; mudar `main` para a base defeituosa evita que `base_main` os barre antes da leitura, mas não identifica qual validação da lista produziu o rc (`bin/tests/test_selo_proporcao.sh:45-53`).
- `sem_repo_no_repo_do_script` ainda pode ser silenciosamente omitido se a cópia não tiver histórico Git (`bin/tests/test_selo_proporcao.sh:141-143`).

3. Achados B1–B5, pela evidência que analisei:

- **B1: cura os exemplos concretos.** Migrações e comandos ficam A pela guarda de pasta e, independentemente, nomes `test_*.py` fora de `tests/` deixam de ser aceitos (`bin/selo_proporcao.py:68-75`).
- **B2: cura o helper concreto; cura parcial do problema geral.** `marca_cron15.py` não atende `test_*.py`, mas arquivo de produção chamado `test_*.py` dentro de `tests/` segue B, limite admitido pelo próprio comentário (`bin/selo_proporcao.py:25-26`, `:74-75`).
- **B3: cura os exemplos concretos de `bin/tests/*`**, ausentes da lista fechada atual (`bin/selo_proporcao.py:64-67`; `app/docs/CORTES.md:444`).
- **B4: cura o recorte linear do exemplo apenas relativamente ao `main` local.** Não autentica a referência `main` nem a fronteira histórica da obra (`bin/selo_proporcao.py:96-100`).
- **B5: o delta não integra chamador nem contador.** Os únicos consumidores encontrados na busca por `selo_proporcao` são o wrapper e o harness (`bin/selo_proporcao.sh:7`; `bin/tests/test_selo_proporcao.sh:9`, `:42`). A mudança contratual fornecida corrige a descrição, declarando ambos pendentes; a ocorrência efetiva de conduta manual/RELATO fica [nao verificado].

4. Divergências documentais úteis:

- Cabeçalho do harness ainda diz `tests/ OU test_*.py`, enquanto o código exige ambos (`bin/tests/test_selo_proporcao.sh:4`; `bin/selo_proporcao.py:74`).
- A linha antiga de CORTES ainda descreve selo “nas linhas adicionadas” e “escrita com B declarado = rc1”; o código julga nomes do diff e conteúdo não interfere (`app/docs/CORTES.md:443`; `bin/selo_proporcao.py:100-118`).
- “So caminho sob app pode ser B” e “migrations/management = A sempre” têm exceção técnica pela prioridade da lista: se um caminho desses for incluído na lista da base, retorna B antes das duas guardas (`bin/selo_proporcao.py:13-17`, `:64-69`). A lista atual contém apenas dois caminhos sob `app/chamados/` (`app/docs/CORTES.md:444`).
- O comentário “nenhum .py fora da lista passa” contradiz as aceitações de Python sob templates/static e test_*.py sob tests (`bin/selo_proporcao.py:24`, `:70-75`).

A cópia disponível não contém repositório Git utilizável: `git status`, `git rev-parse HEAD` e `git log` retornaram “not a git repository”; hash e estados reais de branches não foram confirmados [nao verificado].

## 11/11 15:17 · sessao ef11ff740dce · - · - · origem=Ronald (tmux)

Achados concretos ainda classificados B:

| Caminho permitido | Quem carrega/lê e efeito |
|---|---|
| `app/core/tests/test_contract_esmeril.py` | Define `VULTURE_IGNORE_NAMES` em `app/core/tests/test_contract_esmeril.py:15`. `bin/esmeril_da_copia.sh:22` extrai o valor, `:24` passa a `vulture --ignore-names` e `:27` decide LIMPO. `bin/molde_fatia/rodar.sh:20` extrai, `:21` aplica, `:22` barra esmeril sujo. B por `bin/selo_proporcao.py:74`. **Allowlist de portão permanece em arquivo B.** |
| `app/core/tests/test_contract_tblib_no_paralelo.py` | `tblib_instalado()` definido em `app/core/tests/test_contract_tblib_no_paralelo.py:24`; importado pelo command de produção em `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`, chamado em `:18`. `bin/placar_code.sh:463` executa o command; `:465` publica o contador. B por `bin/selo_proporcao.py:74`. |
| `app/static/pwa.js` | Carregado em `app/templates/base.html:20` e `app/templates/base_app.html:20`; registra service worker em `app/static/pwa.js:14`, grava batida offline em `:107`–`:119`, inicia sincronização em `:126`. B por `bin/selo_proporcao.py:72`. |
| `app/static/sw.js` | URL ligada à view em `app/core/urls.py:24`; leitura e resposta JavaScript em `app/core/views.py:570`–`:576`. `_raiz_do_static()` escolhe `/app/staticfiles` se houver SW coletado, senão `/app/static`, em `app/core/views.py:528`–`:530`. B por `bin/selo_proporcao.py:72`; ramo efetivo no host [nao verificado]. |
| `app/templates/base.html`, `base_app.html`, `ponto/espelho.html`, `colaboradores/detalhe.html`, `chamados/painel_gestao.html` | Portão declara as cinco páginas em `bin/prova_de_casca.py:21`, compila em `:49`–`:53`, lê os HTML da árvore de templates em `:30`–`:36`; chamado antes do reload por `bin/deploy.sh:295`–`:297`. B por `bin/selo_proporcao.py:70`. |

O build também lê conteúdo dos cinco recursos `sw.js`, `pwa.js`, `manifest.json`, `icons/icon-192.png`, `icons/icon-512.png`: lista em `app/core/views.py:524`, leitura em `:548`–`:552`, raiz escolhida em `:528`–`:530`; os caminhos sob `app/static/` passam por `bin/selo_proporcao.py:72`.

O antigo exemplo `app/core/tests/marca_cron15.py` continua importado por produção em `app/core/management/commands/crons.py:92` e afeta a aceitação do horário em `:97`, mas **agora recebe A**, pois seu basename não satisfaz `test_*.py` em `bin/selo_proporcao.py:74`.

A leitura de `app/core/tests/testes_sem_relogio.txt` por `bin/placar_code.sh:458` continua, mas o `.txt` recebe A pelo mesmo teste de nome em `bin/selo_proporcao.py:74`.

## 11/11 15:13 · sessao 6f34a2441ef2 · DATA-EVENTO-CRU · leitura · origem=Code

**Veredito: ajustar o contrato antes de considerá-lo fechado.** Os pontos principais são o alcance de `mensageria/`, a identidade dos escopos e a definição de “leitura”; além disso, a cópia contém **cinco** ocorrências diretas nos templates, incompatíveis com o “+4” sem explicação adicional. (`bin/regua.sh:167`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`; `app/templates/chamados/partials/_card_atendimento.html:87`, `:90`, `:164`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/meu_atendimento.html:19`)

1. **`arquivo::funcao -> contagem` é utilizável, mas não é estável contra refatoração.** A casa já possui uma convenção melhor descrita como `arquivo::escopo`: empilha classes e funções aninhadas e usa `<modulo>` quando não há escopo nomeado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:91`, `:93`, `:132`)

   | Edição/caso | Consequência e nome recomendado |
   |---|---|
   | Inserir linhas/comentários | Preserva a chave; a linha serve apenas ao diagnóstico. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`) |
   | Renomear função, classe ou arquivo | Muda a chave: tratar como **reidentificação**, sem registrar como cura. Consequência da composição em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:132`. |
   | Mover função para outro arquivo ou escopo | Também muda a chave: exige correspondência explícita entre origem e destino. Mesma composição em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`. |
   | Código no nível do módulo | `arquivo::<modulo>`. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`) |
   | Método de classe / função aninhada | `arquivo::Classe.metodo` / `arquivo::externa.interna`; usar somente o último nome perderia essa distinção. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:132`) |
   | Lambda | Recomendo contar no escopo lexical que a contém, declarando essa agregação; é o comportamento do percurso existente, que não cria nome para lambda. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:133`) |

   **A contagem mede ocorrências, não identidade dos leitores:** substituir uma ocorrência por outra dentro da mesma chave preservaria o número e escaparia dessa comparação. Limite inferido do formato do anexo; implementação da nova catraca **[não verificado]**.

   Falta definir quais formas entram: há `getattr(p, 'data_evento', None)` com nome literal fora do juiz e extrações explícitas por `values_list(..., 'data_evento')`. Esses casos precisam entrar ou aparecer nominalmente nos limites; não são nomes calculados dinamicamente. (`app/chamados/juizes.py:652`, `:854`; `app/chamados/services/fio.py:1854`, `:2024`)

   A exceção deve ser **somente a função indicada**, preservando os demais leitores de `juizes.py`: existe leitura direta em outra função desse mesmo arquivo. (`app/chamados/juizes.py:250`, `:271`, `:334`)

2. **A label `chamados` permite varrer todo `/app`; não fornece acesso ao serviço irmão `mensageria/`.** O P13 já demonstra um teste dessa label calculando a raiz por `__file__` e percorrendo todo `APP`, inclusive outros apps. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:33`, `:144`)

   - **Quem monta `LABELS`:** a fonte é `bin/regua.sh:23`; o pre-push lê essa declaração em `bin/pre-push.sh:184` e passa as labels ao runner em `:186`.
   - **Cwd configurado:** `/app`, em `app/Dockerfile:8`; os comandos mostrados não fornecem `-w`. (`bin/pre-push.sh:143`; `bin/regua.sh:166`)
   - **Arquivos disponíveis:** régua e pre-push montam apenas `app/` em `/app`; o archive do pre-push também extrai apenas `app`. Assim, `api/` e `templates/` ficam dentro da árvore disponível. (`bin/regua.sh:167`; `bin/pre-push.sh:144`; `bin/arvore_do_push.sh:98`; `app/config/settings/base.py:72`)
   - **Mensageria:** possui montagem própria de `./mensageria` em `/srv`. Para incluí-la, seria necessário ampliar extração, montagem e raiz do censo; trocar a label não resolve. (`docker-compose.mensageria.yml:3`, `:14`; `bin/arvore_do_push.sh:98`)

   Isso descreve a configuração da cópia; mounts e cwd de um container efetivamente executado são **[não verificado]**.

3. **O regex não perde uma leitura direta apenas porque houve `with/as` ou renomeação da variável.** Nos exemplos sintéticos abaixo, a ocorrência continua escrita na atribuição ou no acesso posterior; execução como template Django **[não verificado]**:

   | Forma sintética | Ocorrências de `\.data_evento\b` |
   |---|---:|
   | `{% with d=q.data_evento %}{{ d }}{% endwith %}` | 1 |
   | `{% with q.data_evento as d %}{{ d }}{% endwith %}` | 1 |
   | `{% with q as outra %}{{ outra.data_evento }}{% endwith %}` | 1 |
   | `{{ q\|attr:"data_evento" }}` | 0 |

   `|attr` é, portanto, uma lacuna sintática **se esse filtro estiver disponível**; sua disponibilidade nesta aplicação é **[não verificado]**. Filtros e tags podem esconder o acesso no Python: a casa já possui filtros que recebem a pergunta e delegam a leitura a uma função. (`app/chamados/templatetags/chamados_extras.py:15`, `:30`)

   O contrato deve chamar isso de **censo de acessos diretos no markup**, retirar comentários antes de contar e declarar acessos indiretos como limite. Há helper existente para comentários de bloco/HTML, mas ele não remove `{# ... #}`. (`app/core/censo_fonte.py:125`, `:132`, `:133`)

   O total Python **190** permanece **[não verificado]** sem o detector e o censo correspondentes; o subtotal direto dos templates da cópia é **5**, nos locais citados no veredito.

4. **Existem varredores de `data_evento`, porém os identificados julgam escrita.** O P13 AST procura atribuições e chamadas de escrita e autoriza `fio::gravar_dia_do_fato`; o contrato de estado também registra esse campo e procura escritas. Uma catraca de leitores tem objeto distinto e pode permanecer como instrumento sem recalcular o dia. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`, `:97`, `:103`, `:165`; `app/core/tests/test_contract_escritores_estado.py:64`, `:179`)

   Já existe reutilização do scanner P13 pelo selo E1: não convém copiar esse instrumento para reimplementar escrita. (`app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:33`, `:51`, `:115`)

   Há uma **colisão concreta numa cura futura**: outro selo exige literalmente `data_pergunta=p.data_evento`. Migrar esse leitor ao juiz exigirá atualizar essa expectativa junto. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:129`, `:130`)

   Não identifiquei outro censo geral de leituras; ausência absoluta **[não verificado]**.

5. **Os varredores de diretório relevantes impõem estas condições:**

   | Selo | Efeito sobre os arquivos propostos |
   |---|---|
   | O20 | Enumera `test_contract_*.py`: o nome `test_catraca_…` fica fora. Se renomeado, passa pelo teto de contratos que leem texto cru. (`app/core/tests/test_o20_contrato_julga_codigo.py:164`, `:176`, `:188`, `:204`) |
   | Relógio | Enumera `test*.py`; chamadas de relógio sem congelamento podem reprovar o novo teste. (`app/core/tests/test_selo_teste_sem_relogio.py:42`, `:53`, `:67`) |
   | CRON15 | Parseia todo `.py` diretamente em `*/tests/`; exige sintaxe válida e mantém fechada a lista de marcas. (`app/core/tests/test_marca_cron15.py:29`, `:33`, `:35`, `:50`) |
   | Vocabulário | Inclui testes e seu texto; aceita `.py/.html/.js/.md`, deixando o JSON fora. (`app/pautas/tests/test_contract_vocabulario.py:19`, `:20`, `:29`, `:41`) |
   | Testes fora do Git | Cobra o `.py` versionado; as colheitas mostradas não incluem JSON. (`bin/testes_fora_do_git.sh:46`, `:49`, `:57`) |
   | P13/escritores | Excluem `tests/` e filtram `.py`; as fixtures desse teste e seu JSON ficam fora dessas varreduras. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`, `:150`; `app/core/tests/test_contract_escritores_estado.py:191`, `:194`) |

   Não encontrei uma enumeração geral de `tests/*.json`; garantia de ausência **[não verificado]**. O selo de labels verifica apps com testes, não a permanência desse arquivo específico; o selo de Git tampouco guarda arquivos removidos. Portanto, manter a remoção do guardião como `!` declarado é coerente, mas não prometer proteção automática por esses selos. (`bin/tests/test_labels_fonte_unica.sh:24`, `:30`, `:32`; `app/core/tests/test_selo_testes_no_git.py:42`)

   A promessa de rodar “em segundos”, o caso injetado que fica vermelho e a monotonicidade contra uma edição simultânea que aumente o JSON continuam **[não verificado]** sem implementação e execução do instrumento.

## 11/11 15:12 · sessao 96eb919cc168 · - · - · origem=Ronald (tmux)

Para (5), não há colisão demonstrada apenas por criar os dois nomes propostos; sem implementação, colisões por conteúdo são [não verificado].

- **O20:** enumera somente `*/tests/test_contract_*.py`; `test_catraca_data_evento_cru.py` fica fora desse censo. Se renomeado para `test_contract_*`, as leituras cruas passam pelo teto zero, com tratamento explícito para AST e exceções declaradas. (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:176`, `:178`, `:188`, `:197`, `:204`)
- **Relógio:** o novo `.py` entra porque o selo enumera `test*.py`; chamadas reais de relógio sem congelamento podem provocar vermelho. O JSON fica fora. (`app/core/tests/test_selo_teste_sem_relogio.py:32`, `:42`, `:48`, `:53`, `:67`)
- **Marca CRON15:** enumera todos os `.py` diretamente em `*/tests/`, faz `ast.parse` sem tolerar erro, e só admite os 15 métodos marcados. O JSON é excluído pela extensão. (`app/core/tests/test_marca_cron15.py:25`, `:29`, `:32`, `:35`, `:48`)
- **Raio de um sítio:** inclui testes, pois as exclusões são apenas migrations/node_modules/staticfiles; exige Python compilável e procura `or` com leitura de `raio_metros`. Uma fixture textual `"p.data_evento"` não satisfaz esse critério. JSON fica fora do `rglob('*.py')`. (`app/core/tests/test_selo_raio_de_um_sitio.py:44`, `:45`, `:50`, `:66`, `:73`, `:92`, `:97`)
- **Vocabulário:** varre inclusive testes e prosa do `.py`; palavras proibidas podem colidir. Só aceita extensões `.py/.html/.js/.md`, excluindo JSON. (`app/pautas/tests/test_contract_vocabulario.py:19`, `:20`, `:29`, `:32`, `:35`, `:41`)
- **Testes fora do Git:** cobra o novo `.py` versionado; não inclui o JSON nas duas colheitas mostradas. Esse selo não prova a existência obrigatória do novo teste: o próprio teste de comparação declara que arquivo no Git e ausente da árvore está fora de seu escopo. (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`, `:57`; `app/core/tests/test_selo_testes_no_git.py:42`)
- **P13/escritores:** não acusam fixtures dentro de `chamados/tests/`, pois excluem a pasta; JSON também é excluído pelo filtro `.py`. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:146`, `:147`, `:150`; `app/core/tests/test_contract_escritores_estado.py:190`, `:191`, `:194`)

Não encontrei uma varredura geral de `tests/*.json`; afirmar que nenhuma existe em toda a árvore continua **[não verificado]**.

_3949 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
