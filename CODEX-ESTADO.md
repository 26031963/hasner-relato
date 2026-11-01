# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 16:49** · publicado em **01/11/2026 16:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 16:49 · sessao 4b97a069739e · O261-E-F2 · verificador · origem=Code

1. **QUEM CHAMA**

As quatro chamadas encontradas fora de `tests/` e `migrations/` coincidem com `VIA_DO_CHAMADOR`, inclusive a via atribuída a cada função. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:398`)

| Local | Chamador | Primeiro argumento literal |
|---|---|---|
| `app/chamados/models.py:2498` | `PerguntaDisputa.fechar_por_admin` | `self.VIA_ADMIN_DECIDIU` |
| `app/chamados/services/fio.py:891` | `retratar_duplicata` | `PerguntaDisputa.VIA_DUPLICATA` |
| `app/chamados/services/materializacao.py:858` | `_materializar_resposta_hora` | `PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| `app/chamados/services/materializacao.py:1033` | `_carimbar_recusa` | `PerguntaDisputa.VIA_FATO_OCUPADO` |

2. **MORDE: existe mutante que passa nos dois testes.** [nao verificado]

Substitua a chamada de `app/chamados/services/materializacao.py:858` pelas três linhas abaixo, mantendo o retorno seguinte:

```python
p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
gravar = p.carimbar_identidade
gravar(via=PerguntaDisputa.VIA_ADMIN_DECIDIU, autor=request_user)
```

A primeira chamada preserva a via esperada pelo teste. A segunda chamada usa `ast.Name`, enquanto o varredor de vias exige `ast.Attribute`; portanto, a gravação de `admin_decidiu` fica invisível para ele. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`)

O varredor de referências reconhece a atribuição do alias, mas ela permanece no mesmo chamador autorizado. Como o resultado vira conjunto de `(arquivo, qualname)`, essa referência adicional não altera a comparação. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`)

Em execução, ambas as vias são aceitas e a última chamada sobrescreve a primeira, persistindo `admin_decidiu`. (`app/chamados/models.py:2597`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Reexecutei os dois métodos de teste em memória, interceptando somente a leitura da fonte mutante: original e mutante passaram nas duas verificações. Usei `unittest.TestCase` no lugar de `SimpleTestCase`; execução integral pelo Django: **[não verificado]**. Métodos exercitados: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`.

Os casos pedidos ficam assim:

| Forma | Resultado |
|---|---|
| Chamada direta com via em variável local | Vira `<dinamica>`; reprova o teste de vias. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`) |
| Chamada direta com `via=outra_variavel` | Também vira `<dinamica>`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`) |
| Keyword diferente de `via` | Sem argumento posicional, vira `<dinamica>`; a assinatura também não aceita esse outro nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`, `app/chamados/models.py:2588`) |
| Duas chamadas diretas com vias distintas | Ambas entram no conjunto; reprova. Duas com a mesma via não são contadas separadamente. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:416`) |
| Chamada direta em função aninhada nomeada | Ganha outro `qualname`; reprova os mapas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:411`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:430`) |

3. **QUEM ESCREVE E QUEM LÊ**

A cadeia de escrita é `carimbar_identidade` → `_escrever_carimbo` → atribuição de `via_resolucao`; quem persiste é `carimbar_identidade`, no mesmo `save` dos campos de materialização. A guarda aceita qualquer uma das quatro identidades, sem vincular a via ao chamador. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Estes leitores **distinguem as quatro vias**:

| Leitor | Decisão que pode mudar |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta hora, com resposta e materializada: paridade → `recusada_paridade`; outras identidades → `aplicada`. É efeito direto do mutante acima. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona paridade; trocar para admin retira a pergunta dessa fonte de candidatos, satisfeitas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`) |
| Cartório | O ramo que tenta reconciliar exige que todas as resolvedoras sejam duplicatas, após as guardas anteriores. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`) |
| Fábrica da célula | Duplicata é excluída do conjunto “já perguntado”; as outras identidades permanecem. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui duplicata do conjunto “já perguntado”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`) |
| `fechar_por_admin` | A guarda idempotente só reconhece `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` e comando de deduplicação | Pulam a pergunta já marcada como duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`) |
| Apresentação e classificação | Mudam a frase do cartão, o motivo do explicador e o balde `via:<valor>`. (`app/templates/chamados/partials/_card_atendimento.html:186`, `app/chamados/services/explicador.py:95`, `app/chamados/services/passivo_validado.py:140`) |

O C6, `_pergunta_materializada_de_fato`, `q_vivas` e `q_carimbo_desfazivel` agrupam as quatro identidades; trocar somente entre elas não muda esses predicados. A guarda da resposta do colaborador precede o encerramento por identidade no C6. (`app/chamados/juizes.py:632`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1066`, `app/chamados/models.py:2708`, `app/chamados/services/relavra_via.py:179`)

Há também leituras para memo, restauração e trilha, sem decisão específica entre as quatro: `app/chamados/juizes.py:790`, `app/chamados/models.py:2552`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:515`.

A re-lavra restringe-se às vias derivadas; o comando de encerramento por fato restringe-se a `SEM_HISTORIA`; os leitores de fantasma/sem lastro/tranca guardam por outras vias. Nenhum desses filtros distingue as quatro identidades. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`)

4. **TRIAGEM V4**

Não encontrei descarte demonstravelmente improcedente **no alcance específico declarado**. Os MORDE realmente usam fontes sintéticas, e o teste da árvore realmente compara os chamadores vivos; isso não garante cobertura de toda forma de chamada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:509`)

O mutante encontrado é alias local de método, não importação renomeada. Assim, aquele descarte não o resolve: a referência fica autorizada pelo conjunto de chamadores, enquanto a chamada indireta escapa ao mapa de vias. A correção v5 barra a troca direta citada na triagem, mas deixa essa lacuna. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:479`)

## 01/11 16:48 · sessao 8e6feb3eb2a7 · - · - · origem=Ronald (tmux)

Mutante de duas linhas, inserido após `app/chamados/services/materializacao.py:858` e antes do retorno em `:859`:

```python
carimbar = p.carimbar_identidade
carimbar(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

A chamada original de paridade permanece. O varredor de vias exige `ast.Call.func` do tipo `ast.Attribute`, portanto não vê a segunda chamada por `ast.Name`; a referência ao método continua na mesma função já declarada e o outro teste reduz as referências a um conjunto de `(arquivo, qualname)`. Assim, ambos passam, enquanto a segunda gravação substitui a via por `admin_decidiu`. Fontes: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:416`, `:433`, `:457`, `:472`; `app/chamados/models.py:2597`, `:2602`, `:2607`.

Executei em memória as duas funções de teste reais, mapas e varredores extraídos por AST: original e mutante deram dois testes, zero falhas, zero erros. Usei `unittest.TestCase` no lugar de `SimpleTestCase`, interceptando somente a leitura da fonte mutada; execução integral pelo Django permanece [não verificado]. [nao verificado]

Quanto aos descartes da triagem, não identifiquei um factualmente improcedente **dentro do alcance específico declarado**:

- Os seis MORDE realmente verificam fontes sintéticas; o teste da árvore realmente compara chamadores reais. A detecção de M1 não implica que detecte o mutante acima. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `:491`, `:509`, `:514`, `:533`.
- A limitação para nome montado decorre de comparar atributo/nome/string exatos. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`.
- A exclusão de `carimbar` do selo de chamadas internas é coerente com consultar o C6; as voltas escrevem dentro das portas autorizadas. `app/chamados/models.py:2565`, `:2609`, `:2619`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `:51`.

É improcedente apenas extrapolar esses descartes para uma garantia geral de que qualquer chamada com via indevida seria barrada: o mutante acima refuta essa garantia. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:457`.

## 01/11 16:47 · sessao b972a23c050a · - · - · origem=Ronald (tmux)

A escrita é `PerguntaDisputa.carimbar_identidade` → `_escrever_carimbo` → `self.via_resolucao = via`; a porta persiste os três campos em `save(update_fields=campos)`. A lista aceita as quatro vias de identidade. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2603`, `app/chamados/models.py:2607`, `app/chamados/services/relavra_via.py:166`)

Leitores que **mudam comportamento entre as quatro vias**:

| Leitor | Diferença |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta tipo hora com resposta e materializada, `paridade_recusada` retorna `recusada_paridade`; as demais retornam `aplicada`. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona `paridade_recusada`, com validação recente; extrai a batida vizinha do motivo e a inclui na fila de flip, incorporada por `fila_flip`. Trocar por `admin_decidiu` remove essa fonte de candidatura, preservadas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`, `app/ponto/services/flip_auto.py:148`) |
| Cartório | Quando todas as resolvedoras são `duplicata`, entra no ramo que tenta reconciliar o chamado. Outra via não entra nesse ramo. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`, `app/ponto/services/cartorio.py:1430`) |
| Fábrica da célula | Exclui `duplicata` do universo que alimenta `ja`; outra identidade permanece e pode impedir nova pergunta daquele motivo. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:104`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui `duplicata` do conjunto já perguntado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`, `app/chamados/management/commands/disparar_perguntas_competencia.py:129`) |
| `fechar_por_admin` | A guarda idempotente só retorna `False` para `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2497`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` | Só retorna imediatamente para `duplicata`; outra identidade é substituída pela via de duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/services/fio.py:883`, `app/chamados/services/fio.py:891`) |
| Comando de deduplicação | Remove as já `duplicata` da lista de excedentes a processar. (`app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/dedup_perguntas_fato.py:76`) |

Leitores que alteram **classificação/apresentação**: `_balde_da_recusa` devolve o valor concreto em `via:<valor>`; o explicador mostra o display da via; o cartão distingue admin, duplicata e as duas recusas. (`app/chamados/services/passivo_validado.py:140`, `app/chamados/services/passivo_validado.py:142`, `app/chamados/services/explicador.py:95`, `app/templates/chamados/partials/_card_atendimento.html:186`)

O C6 **não distingue as quatro** no degrau por identidade; `_pergunta_materializada_de_fato` também as absolve igualmente, `q_vivas` as agrupa e `q_carimbo_desfazivel` exclui todas. A resposta do colaborador é julgada antes do degrau de identidade; logo, não afirmar “qualquer identidade sempre mata a pergunta”. (`app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1055`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:632`, `app/chamados/juizes.py:635`, `app/chamados/models.py:2708`, `app/chamados/models.py:2713`, `app/chamados/services/relavra_via.py:166`, `app/chamados/services/relavra_via.py:179`)

A re-lavra só aceita `declaracao`/`completude_turno`, portanto não troca nenhuma das quatro identidades; `encerrar_perguntas_com_fato` também não as inclui em `SEM_HISTORIA`. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:123`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:42`)

Outros usos são memória/restauração, memo, trilha ou agrupamento por via: `app/chamados/models.py:2552`, `app/chamados/models.py:2613`, `app/chamados/models.py:2627`, `app/chamados/juizes.py:790`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/reconciliador.py:1100`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:515`, `app/chamados/management/commands/auditar_invariantes_chamados.py:194`, `app/chamados/management/commands/auditar_invariantes_chamados.py:205`, `app/chamados/management/commands/auditar_invariantes_chamados.py:432`, `app/chamados/management/commands/auditar_invariantes_chamados.py:541`.

Há ainda escritores/leitores de outras vias: `encerrar_por_fato_no_espelho` escreve `fato_materializado`; as portas de órfã/fantasma/sem lastro/tranca guardam por essas vias, e seus filtros não diferenciam as quatro identidades. (`app/chamados/models.py:2634`, `app/chamados/models.py:2636`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`, `app/chamados/services/materializacao.py:572`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`)

## 01/11 16:44 · sessao 2b54b3c2d6c8 · O261-E-F2 · verificador · origem=Code

O patch de reabertura **não entrega o resultado prometido pelo fixture**: a primeira guarda devolve `veto_do_dia`, por ausência de vínculo de escala. Além disso, os selos novos aceitam mutantes concretos que burlam a intenção da porta. (`app/chamados/models.py:1285`; `app/ponto/services/furos_vetados.py:40`; `app/chamados/tests/test_o261_resposta_vazia.py:36`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`)

As referências abaixo são relativas à raiz da cópia. Para os dois anexos sem arquivo na árvore, numerei o conteúdo fornecido desde a primeira linha: `diff --git` no patch e `# O261-E-F2` na sonda. Execução Django, resultados da sonda e estado de produção: **[nao verificado]**.

1. Quem chama as seis portas.

Estes são os sítios executáveis encontrados para os métodos de `PerguntaDisputa`; chamadas repetidas dentro da mesma função aparecem separadamente pelas linhas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:394`)

| Método | Chamador e sítio |
|---|---|
| `_escrever_carimbo` | `PerguntaDisputa.carimbar` — `app/chamados/models.py:2555` |
| | `PerguntaDisputa._desfazer_em_memoria` — `app/chamados/models.py:2582` |
| | `PerguntaDisputa.carimbar_identidade` — `app/chamados/models.py:2600`, `app/chamados/models.py:2602` |
| | `PerguntaDisputa.descarimbar` — `app/chamados/models.py:2611` |
| `_desfazer_em_memoria` | `PerguntaDisputa.carimbar`, tanto na exceção quanto na recusa — `app/chamados/models.py:2567`, `app/chamados/models.py:2570` |
| `carimbar_identidade` | `PerguntaDisputa.fechar_por_admin` — `app/chamados/models.py:2498` |
| | `retratar_duplicata` — `app/chamados/services/fio.py:891` |
| | `_materializar_resposta_hora` — `app/chamados/services/materializacao.py:858` |
| | `_carimbar_recusa` — `app/chamados/services/materializacao.py:1033` |
| `carimbar` | `PerguntaDisputa.encerrar_fantasma` — `app/chamados/models.py:2648` |
| | `lavrar_sem_turno_aberto` — `app/chamados/reconciliador.py:405` |
| | `retratar_perguntas_absolvidas_por_celula` — `app/chamados/reconciliador.py:618` |
| | `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:523` |
| | `_materializar_uma` — `app/chamados/services/materializacao.py:1166` |
| | `materializar_pendentes_aprovadas` — `app/chamados/services/materializacao.py:1313` |
| | `resolver_perguntas_por_chamado_resolvido`, dois ramos — `app/chamados/services/disputa_emissao.py:1566`, `app/chamados/services/disputa_emissao.py:1638` |
| `descarimbar` | `ChamadoColaborador._universo_da_ressuscitacao`, somente na cópia em memória — `app/chamados/models.py:1439` |
| | `PerguntaDisputa.reabrir_fantasma` — `app/chamados/models.py:2659` |
| | `PerguntaDisputa.reabrir_por_resposta` — `app/chamados/models.py:2676` |
| | `PerguntaDisputa.reabrir_por_competencia` — `app/chamados/models.py:2683` |
| | `desfazer_carimbo_sem_lastro` — `app/chamados/services/relavra_via.py:231` |
| `descarimbar_em_lote` | `ChamadoColaborador.ressuscitar_perguntas` — `app/chamados/models.py:1488` |
| | `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1377` |

As referências descritivas às portas, sem chamada executável, estão nos comentários de declaração do modelo, na descrição do uso nos serviços, no registro de portas e no dossiê. Essas referências não acrescentam chamadores ao conjunto AST porque são comentários ou strings maiores que o nome procurado. (`app/chamados/models.py:2506`; `app/chamados/models.py:2512`; `app/chamados/models.py:2514`; `app/chamados/models.py:2516`; `app/chamados/models.py:2580`; `app/chamados/models.py:2620`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/materializacao.py:857`; `app/core/portas.py:246`; `app/docs/DOSSIES.md:46`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`)

Há homônimos que não são essas portas: `api.credencial.carimbar`, a porta de célula importada pelo cartório e a variável local `carimbar` de competência trancada. (`app/api/views.py:23`; `app/api/views_core.py:23`; `app/ponto/services/cartorio.py:30`; `app/chamados/services/competencia_trancada.py:131`)

**Comparação com `CHAMADORES`:** os quatro chamadores de `_escrever_carimbo`, o único de `_desfazer_em_memoria` e os quatro de `carimbar_identidade` coincidem com os declarados. O dict **não fiscaliza** quem chama `carimbar`, `descarimbar` ou `descarimbar_em_lote`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:380`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:385`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:434`)

| Forma | O selo pega ou perde? |
|---|---|
| `p.metodo(...)`, `PerguntaDisputa.metodo(p,...)`, alias obtido por `g=p.metodo` | **Pega** o atributo; não precisa resolver seu receptor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import local do modelo com alias, seguido de `PD.metodo(...)` | **Pega** pelo atributo; o import local não cria ponto cego nessa forma. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import do próprio nome, renomeado: `from x import _escrever_carimbo as g; g(...)` | **Perde o import como referência**: `ast.alias` não é uma das três formas examinadas. A existência de um export desse método na cópia não foi demonstrada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; **[nao verificado]**) |
| Função aninhada | **Pega**, incluindo a função interna no qualname; portanto cria outra chave na whitelist. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:407`) |
| Classe aninhada | **Pega**, incluindo classe e método internos no qualname. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`) |
| Lambda com referência explícita | **Pega**, mas não acrescenta um componente “lambda” ao qualname. Uma chamada extra dentro de função permitida pode conservar a mesma chave. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`) |
| `super().metodo`, `type(self).metodo`, `self.__class__.metodo` | **Pega**, pois todos apresentam `ast.Attribute.attr` igual ao nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| `getattr(p,'_escrever_carimbo')`, `methodcaller('_escrever_carimbo',...)` | **Pega** a constante exata. Também pega `nome='_escrever_carimbo'; getattr(p,nome)`, no literal. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`) |
| `getattr(p,'_escrever_'+'carimbo')`, `join`, formatação ou nome vindo de fora | **Perde quando não existe atributo, nome ou constante exata**: não avalia a expressão nem acompanha seu valor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |

O selo também descarta linhas e multiplicidade: fiscaliza um **conjunto de `(arquivo, qualname)`**. Não distingue uma chamada legítima de outra chamada indevida na mesma função, nem examina seus argumentos. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`)

Seu universo é menor que “toda árvore fora tests/migrations”: exclui outros diretórios, todos os arquivos não Python, `test_*.py` e fontes que falhem no parse/decodificação. Ausência universal de chamadores dinâmicos fora desse alcance: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:417`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:420`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:425`)

2. Cada selo morde?

Para testar **cada um dos sete testes novos**, apliquei virtualmente o mesmo mutante de produção M1, de três linhas, acrescentado a `app/chamados/reconciliador.py`; as verificações foram executadas apenas sobre AST e fontes em memória. O mutante usa a escrita interna e persiste sem consultar C6. (`app/chamados/models.py:2527`; `app/chamados/models.py:2565`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`)

```diff
+def carimbar_por_fora(p, u, via, agora):
+    p._escrever_carimbo(agora, u.pk, via)
+    p.save(update_fields=['materializada_em', 'materializada_por', 'via_resolucao'])
```

| Teste de `SeloQuemChamaAPorta` | Com M1 na produção | [nao verificado]
|---|---|
| `test_os_chamadores_de_cada_porta_interna_sao_exatamente_os_declarados` | **Vermelho**: encontra a nova chave fora de `CHAMADORES`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`) |
| `test_MORDE_chamada_por_atributo_por_classe_por_alias_getattr_e_methodcaller` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:452`) |
| `test_o_def_e_a_prosa_nao_sao_referencia` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:455`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:460`) |
| `test_MORDE_service_que_escreve_o_carimbo_sem_o_c6` | **Verde**: demonstra que a amostra seria classificada como proibida; não lê o service modificado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`) |
| `test_MORDE_metodo_novo_no_modelo_tambem_fica_fora` | **Verde**: examina a classe escrita na amostra. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:477`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:481`) |
| `test_MORDE_identidade_chamada_de_modulo_que_nao_julga_identidade` | **Verde**: examina a amostra `fechar_em_massa`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:488`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`) |
| `test_MORDE_o_chamador_declarado_em_outro_arquivo_nao_vale` | **Verde**: examina o homônimo artificial. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:496`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:497`) |

Esses seis testes são úteis contra regressões do **varredor**; quem fiscaliza uma mutação efetiva da produção é o primeiro teste da tabela. Não são seis provas independentes de rejeição do código mutado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:462`)

Para `test_MORDE_a_arvore_real_ainda_atribui_o_update_pelo_gerente_reverso`, o mutante concreto em `app/chamados/models.py:1488` é:

```diff
-        n = PerguntaDisputa.descarimbar_em_lote(alvo, validada_em=None, validada_por=None, respondida_em=None)
+        n = alvo.update(materializada_em=None, validada_em=None, validada_por=None, respondida_em=None)
```

**Fica vermelho**, primeiro porque desaparece a âncora literal exigida pelo teste. A segunda metade do teste, na fonte original, também demonstra que o varredor atribui o `alvo.update()` artificial a `PerguntaDisputa` pelo mapa real de relações. Portanto há mordida, mas a rejeição dessa alteração concreta ocorre antes, pela âncora. (`app/core/tests/test_censo_escritas.py:234`; `app/core/tests/test_censo_escritas.py:235`; `app/core/tests/test_censo_escritas.py:240`; `app/core/tests/test_censo_escritas.py:244`)

**Sim, há mutante plausível que passa nos oito testes pedidos, sem montagem de nome.** Em `app/chamados/services/materializacao.py:858`:

```diff
-            p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
+            p.carimbar_identidade(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

Os oito permaneceram verdes no ensaio em memória: arquivo, função e referência continuam iguais. O juiz de paridade passa a registrar uma decisão de admin; `carimbar_identidade` aceita essa via porque ela pertence à lista de identidade. O selo não verifica essa correspondência entre chamador e via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:388`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/models.py:2597`; `app/chamados/juizes.py:1066`)

Há ainda o bypass de M1 trocando sua segunda linha por:

```diff
+    getattr(p, '_escrever_' + 'carimbo')(agora, u.pk, via)
```

Com essa forma, os sete testes de chamadores permanecem verdes; o teste novo do censo também permanece verde porque sua fonte-alvo não mudou. O varredor de escritores não reconhece o `save(update_fields=...)` como atribuição dos valores do carimbo. Resultado da suíte Django completa para esses mutantes: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:183`; `app/core/tests/test_censo_escritas.py:233`)

3. Escritores e leitores dos campos.

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhuma atribuição explícita adicional encontrada para `PerguntaDisputa`: atribuição em `_escrever_carimbo` e limpeza na porta de lote. (`app/chamados/models.py:2529`; `app/chamados/models.py:2626`) |
| `materializada_por` | Mesma concentração, usando `_id` na atribuição e FK na limpeza. Há também o efeito de exclusão de usuário, abaixo. (`app/chamados/models.py:2530`; `app/chamados/models.py:2627`) |
| `via_resolucao` | `encerrar_por_fato_no_espelho`, atribuição e save; `relavra_via.relavrar`, nova via e relavra no mesmo save. (`app/chamados/models.py:2636`; `app/chamados/models.py:2637`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/relavra_via.py:125`) |
| `respondida_em` | Resposta do colaborador; limpeza para correção; resposta automática; recuperação pela trilha. (`app/chamados/services/disputa_emissao.py:1248`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1394`; `app/chamados/services/disputa_emissao.py:1395`; `app/chamados/services/disputa_emissao.py:1410`; `app/chamados/services/disputa_emissao.py:1411`) |
| `validada_em` | `_carimbar_validacao`; `devolver_validacao_recusada`; nova resposta que desfaz validação; limpeza para correção. (`app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:133`; `app/chamados/services/validacao.py:188`; `app/chamados/services/validacao.py:190`; `app/chamados/services/disputa_emissao.py:1260`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`) |
| `validada_por` | Os mesmos quatro atos: atribuição do autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`; `app/chamados/services/validacao.py:189`; `app/chamados/services/disputa_emissao.py:1261`; `app/chamados/services/disputa_emissao.py:1358`) |
| `resposta_colab` | Resposta do colaborador; limpeza para `''` na correção; resposta automática. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1395`) |

`ressuscitar_perguntas` determina a limpeza de `respondida_em`, `validada_em` e `validada_por`, mas entrega a persistência à porta de lote. As atribuições desses instantes em `_universo_da_ressuscitacao` atingem somente uma cópia em memória, sem save. (`app/chamados/models.py:1488`; `app/chamados/models.py:1427`; `app/chamados/models.py:1439`; `app/chamados/models.py:1440`; `app/chamados/models.py:1441`)

Há **dois escritores adicionais do terceiro campo do carimbo**, `via_resolucao`. Ambos são exceções explícitas de `PORTA_VIA`; portanto sua existência não viola a whitelist atual, embora impeça dizer literalmente que somente as seis portas escrevem a tupla inteira. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`)

`materializada_por` e `validada_por` também podem ser zerados pelo mecanismo `on_delete=SET_NULL`. Esse efeito não passa pelas seis portas, e o selo declara expressamente que não o alcança. (`app/chamados/models.py:2742`; `app/chamados/models.py:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

Existe ainda o canal genérico de nascimento `PerguntaDisputa.abrir(**campos)`, que repassa os argumentos a `objects.create`. Não foi localizado chamador atual fornecendo por ele esses campos; impossibilidade de fornecimento dinâmico: **[nao verificado]**. (`app/chamados/models.py:2374`; `app/chamados/models.py:2452`)

O materializador de regularização escreve campos homônimos de **`RegularizacaoExterna`**, não de `PerguntaDisputa`; essa distinção está registrada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`)

As leituras que determinam o comportamento discutido são:

| Campo | Quem lê e para quê |
|---|---|
| `materializada_em` | C6: tranca/identidade, marco e lastro; `.estado`: materialização; materializador: seleção das ainda não materializadas; raiox: consumo; procedência: instante do ato. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1077`; `app/chamados/juizes.py:1107`; `app/chamados/models.py:2477`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Porta: fotografia e restauração; materializador: fotografia anterior; procedência: autoria. (`app/chamados/models.py:2552`; `app/chamados/models.py:2582`; `app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`) |
| `via_resolucao` | C6: tranca/identidade; relavra: elegibilidade e mudança; materializador: paridade; apresentação: explicação ao colaborador. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6: resposta exige data **e** texto; `.estado` e `.respondida`: rótulo; validação: pré-condição; raiox: partição; HX: respostas aguardando consumo; template: apresentação. (`app/chamados/juizes.py:1047`; `app/chamados/models.py:2479`; `app/chamados/models.py:2783`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`; `app/chamados/juizes.py:1634`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6; `.estado`; validação idempotente; seleção do materializador; seleção do fecho sem credor; resposta sem veredito do C7. (`app/chamados/juizes.py:1033`; `app/chamados/models.py:2475`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`; `app/chamados/models.py:1768`) |
| `validada_por` | Fotografias de validação/reversão, atribuição em painel/verbos e procedência. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6: resposta e concordância com a luz; C7: resposta sem veredito mesmo sem timestamp; materializador: hora e guarda de texto; validação; apresentação; auditoria de meio-estado. (`app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1010`; `app/chamados/models.py:1770`; `app/chamados/services/materializacao.py:264`; `app/chamados/services/materializacao.py:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`) |

O histórico também lê os instantes dinamicamente por `getattr`, e a fotografia de passivo lê os campos para reversão; essas leituras não são escritores adicionais. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`)

4. Patch não commitado. [nao verificado]

**(a) O retorno

[truncado em 20000 de 32629 caracteres]

## 01/11 16:40 · sessao 30d159e9fd65 · - · - · origem=Ronald (tmux)

Auditoria concluída e enviada com todas as referências executáveis das seis portas e limites do AST. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:373]

Verificação em memória, sem Django ou alterações de arquivos:

- Mutante com chamada direta externa: teste da árvore vermelho; seis testes de fontes sintéticas verdes. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:433] [nao verificado]
- Mutante usando `getattr(p, '_escrever_' + 'carimbo')`: sete testes novos verdes; scanner de escritores também não o reconhece. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:403] [nao verificado]
- Mutante sem ofuscação, trocando `VIA_PARIDADE_RECUSADA` por `VIA_ADMIN_DECIDIU`: **oito testes pedidos verdes**; a whitelist valida função e arquivo, sem validar a via usada. [app/chamados/services/materializacao.py:858; app/chamados/tests/test_o261e_f2_porta_carimbo.py:388]
- Mutante restaurando `alvo.update(...)` no originário: teste novo do censo vermelho pela âncora desaparecida. [app/chamados/models.py:1488; app/core/tests/test_censo_escritas.py:235]

Resultado da suíte Django completa: [nao verificado]. [nao verificado]

## 01/11 16:39 · sessao 7d4e42e59c6f · - · - · origem=Ronald (tmux)

Q3/Q4b — achados consolidados, somente leitura.

Os dois escritores adicionais de `via_resolucao` são deliberadamente permitidos pelo selo separado `PORTA_VIA`; portanto existem fora das seis portas perguntadas, mas não constituem fuga da allowlist atual. O contrato de exclusividade dos campos `materializada_em`/`materializada_por` é distinto do contrato da via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`, `:44`, `:51`, `:238`)

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhum escritor explícito de `PerguntaDisputa` encontrado nas formas varridas; a atribuição está em `_escrever_carimbo`, e o lote limpa pelo gerente do modelo. (`app/chamados/models.py:2529`, `:2626`) |
| `materializada_por` | Idem; atribuição de FK por `_id` em `_escrever_carimbo`, limpeza no lote. (`app/chamados/models.py:2530`, `:2627`) |
| `via_resolucao` | `PerguntaDisputa.encerrar_por_fato_no_espelho`: atribui e salva somente a via; `relavra_via.relavrar`: atribui a nova via, relavra veredito e salva. (`app/chamados/models.py:2636`, `:2637`; `app/chamados/services/relavra_via.py:123`, `:124`, `:125`) |
| `respondida_em` | `responder_pergunta_disputa_core` atribui `now()` e salva; `reabrir_para_correcao` limpa no `update`; `responder_automaticamente` atribui o instante recebido e salva; `carimbar_resposta_pela_trilha` atribui o instante testemunhado e salva. (`app/chamados/services/disputa_emissao.py:1248`, `:1266`, `:1358`, `:1378`, `:1394`, `:1395`, `:1410`, `:1411`) |
| `validada_em` | `_carimbar_validacao` atribui `now()` e salva; `devolver_validacao_recusada` limpa; resposta nova limpa validação anterior; reabertura para correção limpa em lote. (`app/chamados/services/validacao.py:131`, `:133`, `:188`, `:190`; `app/chamados/services/disputa_emissao.py:1260`, `:1266`, `:1358`, `:1378`) |
| `validada_por` | Os mesmos quatro atos de `validada_em`, com autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`, `:133`, `:189`, `:190`; `app/chamados/services/disputa_emissao.py:1261`, `:1266`, `:1358`, `:1378`) |
| `resposta_colab` | `responder_pergunta_disputa_core` escreve a resposta; `reabrir_para_correcao` escreve `''`; `responder_automaticamente` escreve o texto recebido. (`app/chamados/services/disputa_emissao.py:1247`, `:1266`, `:1358`, `:1378`, `:1393`, `:1395`) |

`ChamadoColaborador.ressuscitar_perguntas` fornece `validada_em=None`, `validada_por=None`, `respondida_em=None` à porta de lote; não executa um segundo `update` próprio desses campos. (`app/chamados/models.py:1488`)

As atribuições `_q.validada_em=None` e `_q.respondida_em=None` são simulação em cópia de memória: o código copia a pergunta, copia `_state` e o cache das relações, aplica `descarimbar(salvar=False)` e consulta o C6. Não persiste essa `_q`. (`app/chamados/models.py:1427`, `:1430`, `:1431`, `:1439`, `:1440`, `:1441`, `:1442`)

Há também comportamento de exclusão de usuário: ambas as FKs de autoria usam `on_delete=SET_NULL`, o que pode zerar autoria fora dessas portas. O próprio varredor declara que não alcança esse mecanismo. (`app/chamados/models.py:2740`, `:2742`, `:2760`, `:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

`regularizacao_ext.materializar_regularizacao_externa` escreve campos homônimos em `RegularizacaoExterna`, não em `PerguntaDisputa`; a exceção está explicitada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`, `:199`, `:200`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `:49`)

`PerguntaDisputa.abrir` recebe `**campos` e os repassa a `objects.create`, portanto é um canal genérico de criação capaz de receber esses campos; nenhum caller atual fornecendo os campos examinados apareceu na varredura explícita. A impossibilidade de fazê-lo dinamicamente não está provada. (`app/chamados/models.py:2374`, `:2452`, `:2453`; [nao verificado] para ausência universal de argumentos dinâmicos)

Leitores que explicam o efeito dos campos:

| Campo | Leitores e consequência |
|---|---|
| `materializada_em` | C6 lê identidade/tranca, fato do marco e lastro; `.estado` distingue validada/materializada; materializador seleciona apenas nulo; raiox exclui materialização com veredito `VL_SIM`; procedência usa a data como alternativa à validação. (`app/chamados/juizes.py:1042`, `:1071`, `:1077`, `:1107`; `app/chamados/models.py:2475`, `:2480`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Foto de reversão do ato, autoria da procedência e rollback em memória da porta. (`app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`, `:72`; `app/chamados/models.py:2552`, `:2581`, `:2582`) |
| `via_resolucao` | C6 julga identidade/tranca; relavra escolhe somente vias derivadas; materializador reconhece recusa de paridade; template escolhe explicação ao colaborador. (`app/chamados/juizes.py:1042`, `:1071`; `app/chamados/services/relavra_via.py:116`, `:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6 só entra no degrau da resposta se data e texto existem; `.estado` distingue pendente/respondida; validação recusa sem data; fila global exige data; raiox separa respondidas/não respondidas pela coluna; template condiciona apresentação. (`app/chamados/juizes.py:1047`, `:1284`; `app/chamados/models.py:2479`, `:2482`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`, `:137`, `:138`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6 mata a pergunta; validação evita repetição; materializador exige não nulo; fecho sem credor seleciona só nulo. (`app/chamados/juizes.py:1033`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`) |
| `validada_por` | Foto da validação e reversão, métricas de autoria e procedência da batida. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`, `:244`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6 exige texto com data e compara com a luz; materializador interpreta a hora e recusa texto vazio; validação classifica a resposta; telas exibem texto; auditoria detecta meio-estado. (`app/chamados/juizes.py:1047`, `:1010`; `app/chamados/services/materializacao.py:264`, `:393`, `:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`, `:1102`, `:1103`) |

O histórico lê dinamicamente os três instantes por `getattr(p, campo)` e usa o texto da resposta para compor os eventos; a foto de reversão de passivo também lê dinamicamente os campos, sem escrevê-los. (`app/chamados/services/historico.py:99`, `:101`, `:103`, `:105`, `:109`; `app/chamados/services/passivo_validado.py:64`, `:66`, `:83`)

Para Q4b, não encontrei escritor atual que grave `resposta_colab` **preenchida** deixando de atribuir também `respondida_em`: o core atribui o par e salva junto; automático também atribui e salva o par. A limpeza de correção deixa `resposta_colab=''` com data nula, que é outro estado. (`app/chamados/services/disputa_emissao.py:1247`, `:1248`, `:1249`, `:1266`, `:1393`, `:1394`, `:1395`, `:1358`, `:1378`)

A porta automática não valida `instante is not None`, mas o único caller de produção localizado passa `agora=timezone.now()`. Assim, a possibilidade da API interna produzir meio-estado existe se usada com `None`; sua ocorrência nos dados fornecidos e a origem histórica dos registros continuam [nao verificado]. (`app/chamados/services/disputa_emissao.py:1382`, `:1391`, `:1393`, `:1394`; `app/api/foto_recebida.py:42`, `:48`)

A frase da sonda “só dado legado” não é uma conclusão provada pelo anexo: ele não traz resultados, datas de escrita ou trilha dos registros. [nao verificado]

## 01/11 16:39 · sessao 8051cb744c18 · - · - · origem=Ronald (tmux)

4e — A sonda usa funções reais, mas não prova uma execução completa do patch: `carimbar` consulta C6 e salva somente quando morta (`app/chamados/models.py:2565`, `app/chamados/models.py:2576`); `reabrir` usa `recusa_da_reabertura` e executa efeitos reais (`app/chamados/models.py:1338`, `app/chamados/models.py:1380`). A execução/saída `SONDA_OK` não consta do dado apresentado. [nao verificado]

O `atomic` externo com exceção desfaz as escritas da conexão participante; isso é ensaio com rollback, não conexão configurada para somente leitura. A configuração sombra declara apenas `default`, herdado de saas (`app/config/settings/sombra.py:27`, `app/config/settings/saas.py:83`); trilhas usam ORM sem outra conexão (`app/core/observ.py:133`). Ausência absoluta de escritas por qualquer conexão ou dependência transitiva: [nao verificado].

Há escapes concretos do procedimento documentado:

- O comando indicado chama `lavra_de_prod` antes da sonda (`bin/sombra.sh:489`). Essa função cria diretório e arquivos (`bin/sombra.sh:137`, `bin/sombra.sh:144`, `bin/sombra.sh:151`) e lança outro processo que grava `MetricaSnapshot` por dois `update_or_create`, fora do `atomic` da sonda (`bin/sombra.sh:157`, `bin/sombra.sh:160`, `bin/sombra.sh:166`).
- O wrapper grava a saída no host usando `tee` (`bin/sombra.sh:492`). O app está montado somente para leitura, mas `/sombra` é gravável e logs/media são tmpfs (`bin/sombra.sh:189`).
- Logging não participa do rollback: exceções do cartório chegam ao logger `ponto` (`app/ponto/services/cartorio.py:1763`), configurado com arquivo (`app/config/settings/saas.py:162`, `app/config/settings/saas.py:181`). `registrar_engolido` também modifica contador em memória e emite logging (`app/core/observ.py:75`, `app/core/observ.py:79`).
- Até C6 pode engolir exceção e continuar, portanto “qualquer exceção sobe” não descreve todas as funções chamadas (`app/chamados/juizes.py:1104`, `app/chamados/juizes.py:1115`); o reconciliador também captura falhas por chamado (`app/chamados/reconciliador.py:1369`).

A medição de laço cobre `reconciliar_chamados_colab`, que chama os reconciliadores e percorre chamados vivos (`app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1356`). Não cobre a rodada do consumidor do outbox (`app/chamados/reconciliador.py:1255`) nem os efeitos pendentes em `on_commit` (`app/chamados/signals.py:121`, `app/chamados/signals.py:202`, `app/chamados/signals.py:251`). O outbox encontrado é tabela ORM, não fila externa (`app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:631`). Ausência de laço após commit e consumidores: [nao verificado].

Item 5 — O registro vivo aponta C6 para `chamados.juizes.pergunta_viva` (`app/core/juizes.py:266`), e a porta efetivamente o consulta (`app/chamados/models.py:2565`): nenhum segundo juiz é necessário para o patch proposto. A dependência problemática é a promessa de reabrir sem demonstrar que as guardas aceitam o fixture: a porta recusa antes de mudar estado (`app/chamados/models.py:1285`, `app/chamados/models.py:1338`).

Os novos testes de formas são testes sobre strings artificiais (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`); a fiscalização da produção é o teste que lê a árvore (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`). O censo novo lê fonte real, mas exige uma âncora textual específica e sua substituição (`app/core/tests/test_censo_escritas.py:233`, `app/core/tests/test_censo_escritas.py:235`, `app/core/tests/test_censo_escritas.py:244`). Não equivale a prova universal contra mutações de escrita.

## 01/11 16:18 · sessao 4f8555cb3c3e · O261-E-F2 · censo · origem=Code

Nos anexos transcritos, conto linhas desde a primeira linha de cada arquivo, incluindo linhas em branco e excluindo o marcador `== anexo ==`.

1. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)” (`codex_v1.q:25`); reiterada para o diff total (`codex_v3.q:14`).  
   **Resposta/triagem sobre o bypass por chamadas permitidas:** **nenhum**. As respostas apontaram escapes nas formas de atribuição, outro defeito do selo (`codex_v1.r:150`; `codex_v1.r:152`). O selo permite `_escrever_carimbo`, cuja implementação escreve sem consultar C6; a consulta está em `carimbar` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`).

2. **(b) — Codex viu; a triagem reduziu a nota.**  
   **Pergunta:** comportamento observável de cada chamador “quando a porta RECUSA”, incluindo o que devolve ou emite (`codex_v1.q:15`; `codex_v1.q:16`; `codex_v1.q:17`).  
   **Resposta:** para `_materializar_ausencia_de_resposta`, “Ignora o booleano. A ausência já criada/reaberta continua” (`codex_v1.r:93`).  
   **Triagem/motivo:** “Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado” (`triagem_v3.md:9`). O núcleo — recusa ignorada e redução do conjunto absorvido — foi tratado; a formulação específica “sem trilha/log” não aparece nesse tratamento (`codex_v1.r:93`; `triagem_v3.md:9`).

3. **(b) — Os dois componentes foram vistos e encaminhados à fila.**  
   **Pergunta:** possibilidade de gravar e reler VIVA; divergência entre memória e banco; ordem de trava (`codex_v1.q:18`; `codex_v1.q:20`; `codex_v1.q:26`).  
   **Resposta:** “sem reler ou travar a pergunta” e “exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão” (`codex_v1.r:109`; `codex_v1.r:126`).  
   **Triagem/motivos:** concorrência anterior ao diff e alegada mudança da ordem de trava de sete chamadores (`triagem_v1.md:9`; `triagem_v2.md:10`). Para memória, a triagem corrigiu “o atomic cobre”, reconheceu que a memória permanece carimbada e encaminhou: “Não é regressão; vai a fila” (`triagem_v3.md:11`).

4. **(b) — Relógio visto e remetido a complemento.**  
   **Pergunta:** “relogio lido fora da borda” (`codex_v1.q:24`), repetida na volta final (`codex_v3.q:13`).  
   **Resposta:** “Os novos métodos têm `now()` de fallback”; a injeção integral permanece incompleta (`codex_v1.r:144`).  
   **Triagem/motivo:** “a leitura do relogio MUDOU DE SITIO, nao nasceu”, encaminhando ao complemento RELOGIO-INJETADO (`triagem_v2.md:9`; confirmado em `triagem_v3.md:12`). O detalhe de `encerrar_fantasma` chamar sem `quando` está no código, mas não foi explicitamente nomeado na resposta citada (`app/chamados/models.py:2648`; `codex_v1.r:144`).

5. **(b) — Visto; descarte agregado, sem motivo individual registrado.**  
   **Pergunta:** “derivacao paralela de fonte unica” (`codex_v1.q:24`; `codex_v1.q:25`).  
   **Resposta:** “Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra”, citando os dois sítios do opus (`codex_v1.r:145`).  
   **Triagem:** encerra agregadamente “2o juiz, derivacao paralela e 2o escritor explicito: nenhum” (`triagem_v2.md:12`). **Classifico em (b) por esse encerramento agregado; motivo específico da triagem para manter as listas 5×4: [nao verificado].** A preexistência foi registrada pela resposta, não como justificativa individual da triagem (`codex_v1.r:145`; `triagem_v2.md:12`).

6. **(b) — Os três restos foram vistos e deixados para outro pouso.**   [nao verificado]
   **Pergunta:** “Algum chamador de `fechar_por_competencia_trancada` [...] sobrou?”, além do mandato “L-111 apaga” (`codex_v1.q:27`; `codex_v1.q:7`).  
   **Resposta:** “Restam referências instrumentais/documentais obsoletas dizendo que a porta existe”, citando exatamente os três arquivos do opus (`codex_v1.r:160`).  
   **Triagem/motivo:** “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5)” (`triagem_v1.md:15`). [nao verificado]

7. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida”, reiterada para o “diff total” (`codex_v1.q:25`; `codex_v3.q:14`).  
   **Resposta/triagem sobre a perda da testemunha da classe (f): nenhum.** O teste passa a exigir uma chamada AST a `descarimbar_em_lote` e um `update()` censado diretamente, sem `[relacionado]` (`app/core/tests/test_censo_escritas.py:210`; `app/core/tests/test_censo_escritas.py:214`). É uma lacuna estrutural inspecionável no teste; classificá-la não depende de recuperar uma execução histórica de RED (`app/core/tests/test_censo_escritas.py:202`).

8. **(e) — Não perguntado quanto ao custo da dupla execução.**  
   **Pergunta mais próxima:** menciona “memo `_pv_memo`”, mas pergunta se a porta grava e a releitura resulta VIVA; não pede avaliação de custo ou quantidade de consultas ao C6 (`codex_v1.q:18`; `codex_v1.q:19`).  
   **Resposta relacionada:** descreve que `lavrar_sem_turno_aberto` “antes salvava depois do pré-C6”, sem apontar o custo da segunda avaliação (`codex_v1.r:88`).  
   **Resposta/triagem do achado de custo: nenhum.** Os dois pontos de chamada e a alteração dos campos da chave estão no código (`app/chamados/reconciliador.py:398`; `app/chamados/reconciliador.py:405`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:790`; `app/chamados/juizes.py:791`).

9. **(e) — Não perguntado quanto ao vocabulário do commit/comentário.**   [nao verificado]
   **Trechos delimitadores:** as perguntas pedem escritores, leitores e classes de defeitos; nenhuma pede auditoria lexical do commit ou da palavra “fallback” em comentários (`codex_v1.q:11`; `codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`).   [nao verificado]
   **Resposta/triagem deste achado: nenhum.** O “fallback” mencionado na resposta refere-se ao relógio, outro tema (`codex_v1.r:144`). A ocorrência no comentário está confirmada (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:404`); a ocorrência no corpo original do commit, além do relato do opus, é **[nao verificado]**.

| achado | classe | volta | trecho |
|---|---|---|---|
| 1 — chamadores sem selo | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 2 — recusa silenciosa na ausência | **(b)** | v1 → triagem v3 | “Ignora o booleano”; triagem: conjunto restrito ao efetivamente carimbado (`codex_v1.r:93`; `triagem_v3.md:9`). |
| 3 — trava e memória após erro | **(b)** | v1–v3 | Janela anterior; memória: “Não é regressão; vai a fila” (`triagem_v1.md:9`; `triagem_v3.md:11`). |
| 4 — relógio nas portas | **(b)** | v1–v3 | “MUDOU DE SITIO, nao nasceu”; complemento RELOGIO-INJETADO (`triagem_v2.md:9`). |
| 5 — duas listas de identidade | **(b)** | v1 → triagem v2 | Separação registrada; encerramento agregado “nenhum”; motivo individual ausente (`codex_v1.r:145`; `triagem_v2.md:12`). |
| 6 — restos de L-111 | **(b)** | v1 | “INCOMPLETO declarado”; instrumento/doc com pouso próprio (`triagem_v1.md:15`). | [nao verificado]
| 7 — testemunha da classe (f) | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 8 — custo de dois C6 | **(e)** | v1, pergunta mais próxima | Memo perguntado quanto à vivacidade, sem pergunta de custo (`codex_v1.q:18`; `codex_v1.q:19`). |
| 9 — palavra “fallback” | **(e)** | v1–v3 | Perguntas de defeitos técnicos, sem auditoria lexical (`codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`). |

## 01/11 16:17 · sessao a802cb69c575 · E4-CARGA · verificador · origem=Code

A mudança corrige um repasse entre contratos incompatíveis: `dias_encerrados` recebe `{(colaborador_id, data): célula}`, enquanto `turnos_do_colab` espera `{data: célula}` com pad. O encerramento continua usando o mesmo julgamento puro. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:2153`)

**Não considero demonstrada a conclusão operacional da obra:** existe exigência explícita de “RED + impacto na sombra”; execução desses testes, impacto e sequência efetiva de pouso estão **[não verificado]**. (`app/docs/BACKLOG.md:861`)

**1. Quem produz e quem consome os valores**

| Valor | Quem produz | Quem lê |
|---|---|---|
| `dias_encerrados(..., celulas=...)` | Chamador; na ausência, `celulas_do_periodo([pk], ini, fim)`, com chave composta. (`app/ponto/turnos.py:2145`, `app/escala/alimentacao.py:41`) | `_dias_encerrados_puro`: alimenta `vinculo_do_dia` e o julgamento do fim previsto/DNA. (`app/ponto/turnos.py:2170`, `app/ponto/turnos.py:2119`, `app/escala/alimentacao.py:237`) |
| `turnos_do_colab(..., celulas=...)` | Chamador; na ausência, carrega `[ini−2, fim+1]` e converte para chave `data`. (`app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1882`) | Candidatura das geradoras (`mais`), papel da ata e escolha do vínculo de cada dia. A segmentação recompõe a chave `(pk,data)`. (`app/ponto/turnos.py:1885`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1954`) |
| Retorno sem motivos | `_dias_encerrados_puro` escreve `{data: (_m is None)}`. (`app/ponto/turnos.py:2172`) | Consumidores booleanos listados abaixo. |
| Retorno com motivos | O mesmo corpo escreve `{data: motivo ou None}`; `None` significa encerrado. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2172`) | Calendário; no método do lote, leitor de status. (`app/colaboradores/services/calendario.py:701`, `app/ponto/status_dia.py:196`) |

Todos os sítios de produção encontrados que passam `celulas` explicitamente às duas funções, incluindo aliases e argumento posicional:

| Chamada | Forma e janela da alimentação | Muda com o diff? |
|---|---|---|
| Cartório → `dias_encerrados` | `{(c.pk,x.data): x}` da lista `cels`; limites `d0=cels[0].data`, `d1=cels[-1].data`. A lista pode ter lacunas. (`app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942`) | **Sim**, quando a chamada ocorre: não fornece `turnos`; agora o pareador carrega suas células com pad. (`app/ponto/turnos.py:2154`) |
| Calendário → `_dias_encerrados` | `_cels_mes`, composta, carregada em `[data_ini,data_fim]`; fornece também os turnos dessa janela. (`app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699`) | **Não**: `turnos` fornecido evita o ramo alterado. (`app/ponto/turnos.py:2154`) |
| `dia_encerrado_motivo` → `turnos_do_colab` | Repasse posicional do próprio parâmetro; deveria receber `{data}` cobrindo `[data−2,data+1]`. (`app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845`) | **Não**: esse repasse permanece. (`app/ponto/turnos.py:2096`) |
| Grade, antes do alinhamento → `_tdc_pre` | Converte `_cels_gr` para `{data}`; carga de `janela_com_pad(_ini_al,ref)`. (`app/escala/utils.py:1258`, `app/escala/utils.py:1260`, `app/escala/utils.py:1532`) | **Não**: chamada direta preservada. (`app/escala/utils.py:1532`) |
| Grade, depois dos overlays → `turnos_do_colab` | `{data}` do mesmo pad; dentro da montagem, cópias com ata fresca não salva; fora dela, células persistidas. (`app/escala/utils.py:1830`, `app/escala/utils.py:1833`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`) | **Não**: essa alimentação continua chegando diretamente ao pareador. (`app/escala/utils.py:1847`) |
| Emissor de turno aberto retroativo → `turnos_do_colab` | `{data}`, carregado em `[ini−2,fim+2]`, portanto um dia além do pad necessário. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/turnos.py:1845`) | **Não**: chamada direta preservada. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`) |

O repasse singular merece uma ressalva: o fechamento fornece `{data}` recortado pelo pad, mas o vigia fornece `{(pk,data)}` de uma única célula, que atravessa `dia_encerrado` e ainda chega incompatível ao pareador. É uma ocorrência **preexistente, não curada por este diff**. (`app/ponto/services/fechamento.py:354`, `app/ponto/services/fechamento.py:361`, `app/ponto/services/vigia_de_hora.py:57`, `app/ponto/turnos.py:2075`, `app/ponto/turnos.py:2096`)

Os leitores de produção do retorno da **função de módulo** são:

| Leitor | Uso |
|---|---|
| Cartório | Bool com default `False`; decide `_em_curso` e alimenta o classificador. (`app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`) |
| Calendário | Motivos; distingue dia em curso e `sem_previsto`. (`app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:701`, `app/colaboradores/services/calendario.py:709`) |
| Comando `supra_juiz` | Bool passado ao classificador. (`app/ponto/management/commands/supra_juiz.py:121`, `app/ponto/management/commands/supra_juiz.py:232`) |
| Emissor de furo retroativo | Bool passado ao classificador. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`) |
| `par_completo_da_janela` | Bool para excluir turnos de dias não encerrados. (`app/ponto/turnos.py:2183`, `app/ponto/turnos.py:2186`) |

O **método homônimo do lote** tem leitores no status, com motivos, e no painel, com bool. Ele chama diretamente `_dias_encerrados_puro`, sem atravessar a linha alterada. (`app/ponto/status_dia.py:196`, `app/core/services/painel_op.py:334`, `app/ponto/turnos.py:3018`)

**2. Contraexemplos, custo e memória**

**Resultado antes correto que passe a errado: [não verificado].** Os novos casos fixam correções nos dois sentidos: X deve encerrar às 08h; Y deve continuar aberto nesse instante. Isso demonstra a expectativa da fixture, não a ausência universal de regressões. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_bug_enc_celulas_forma.py:50`)

**O custo muda no cartório:** passa a existir uma consulta adicional de células por chamada efetiva de encerramento, pois o pareador agora recebe `None` e executa `celulas_do_periodo`. `_enc` memoiza o resultado por execução de `_julgar_colab_corpo`, evitando uma carga por dia. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:1882`, `app/escala/alimentacao.py:41`, `app/ponto/services/cartorio.py:939`)

As células adicionais do pad também podem acrescentar geradoras e disparar a consulta condicional de `mais`; alterações adicionais nos caminhos preguiçosos e relevância em tempo de produção estão **[não verificado]**. O lote conserva suas cargas e seu acesso direto ao corpo puro. (`app/ponto/turnos.py:1885`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:3018`)

A ordem do ramo normal do cartório é:

1. Monta a grade; o pareamento final dessa montagem recebe cópias com **ata fresca em memória**. (`app/ponto/services/cartorio.py:963`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`)
2. Consulta encerramento sob demanda, por exemplo em `_em_curso`; agora esse pareador recarrega células persistidas. (`app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1884`)
3. Depois, no apply, lavra a célula atual com `save=False` e salva por `carimbar`. (`app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`, `app/ponto/portas/celula.py:807`)

Portanto, **há diferença de estado entre a ata fresca usada pela grade e a persistida usada pelo encerramento**. Entretanto, o cartório passa ao encerramento a lista original `cels`, não as cópias frescas da grade; esse diff não remove a alimentação direta dessas cópias. Divergência efetiva dry-run/apply causada pela mudança: **[não verificado]**. (`app/ponto/services/cartorio.py:942`, `app/escala/utils.py:1846`, `app/escala/utils.py:1847`)

Como `_enc` é preguiçoso, a primeira carga pode ocorrer numa iteração posterior: a ordem demonstrada é “antes da lavra **da célula atual**”, não necessariamente antes de todas as lavras da passagem. (`app/ponto/services/cartorio.py:939`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1596`)

No ramo `nunca_bateu`, a pergunta ocorre **somente dentro de `apply_`**, antes da lavra e do carimbo; o dry-run desse ramo não faz essa pergunta. Essa assimetria já está no chamador. (`app/ponto/services/cartorio.py:1040`, `app/ponto/services/cartorio.py:1063`, `app/ponto/services/cartorio.py:1075`, `app/ponto/services/cartorio.py:1097`)

**3. Os testes mordem?** [nao verificado]

A tabela descreve o que as asserções detectariam; execução dos mutantes e resultados efetivos: **[não verificado]**.

| Teste | Mutante plausível que pega | O que deixa passar | [nao verificado]
|---|---|---|
| `test_regra` | Ignorar geradora/intervalo e fechar Y; manter X aberto; retirar `mais` somente do lote. Exige estados e motivos literais. (`app/ponto/tests/test_e4_carga_paridade.py:129`, `app/ponto/tests/test_e4_carga_paridade.py:139`) | Inverter somente o retorno sem motivos; usa `motivos=True`. (`app/ponto/tests/test_e4_carga_paridade.py:107`, `app/ponto/tests/test_e4_carga_paridade.py:115`) |
| `test_paridade_carga_x_casca_em_fixture_mista` | Divergência exclusiva de um carregador, inclusive perder `mais`, batida ou identidade da saída; compara seis campos, motivos e atribuição das batidas. (`app/ponto/tests/test_e4_carga_paridade.py:55`, `app/ponto/tests/test_e4_carga_paridade.py:122`, `app/ponto/tests/test_e4_carga_paridade.py:148`) | Erro compartilhado pelos corpos puros, mantendo os dois lados iguais. (`app/ponto/turnos.py:1913`, `app/ponto/turnos.py:3009`, `app/ponto/turnos.py:3018`) |
| `test_MORDE_carga_sem_mais` | Como controle de sensibilidade, exige que a retirada simulada de `mais` alcance a carga e produza divergência em Y. (`app/ponto/tests/test_e4_carga_paridade.py:159`, `app/ponto/tests/test_e4_carga_paridade.py:162`, `app/ponto/tests/test_e4_carga_paridade.py:165`) | **Sozinho, deixa passar produção já sem `mais`**, porque continua exigindo divergência. Quem reprova essa retirada permanente são `test_regra` e `test_paridade`. (`app/ponto/tests/test_e4_carga_paridade.py:143`, `app/ponto/tests/test_e4_carga_paridade.py:148`, `app/ponto/tests/test_e4_carga_paridade.py:165`) |
| `test_RED_alimentado_igual_a_juiza_e_ao_lote` | Restaurar o repasse antigo da chave composta: constrói essa alimentação e compara com lote e singular. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/tests/test_bug_enc_celulas_forma.py:43`, `app/ponto/tests/test_bug_enc_celulas_forma.py:44`) | Erro compartilhado do julgamento; conversão bool errada; erro exclusivo do ramo com `turnos=` fornecido. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/turnos.py:2154`, `app/ponto/turnos.py:2172`) |
| `test_RED_regra_do_veredito_do_cartorio` | Repasse antigo ou perda do intervalo de Y; fixa X encerrado e Y aberto às 08h. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_bug_enc_celulas_forma.py:50`) | Erros exclusivos de outros instantes, Z ou conversão bool: só consulta X/Y às 08h, com motivos. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/tests/test_bug_enc_celulas_forma.py:49`) |

**O patch alcança o lote:** o construtor faz import local de `escalas_do_periodo_por_colab`; `_lote` é executado dentro do patch, antes dessa resolução e chamada. (`app/ponto/turnos.py:2951`, `app/ponto/turnos.py:2963`, `app/ponto/tests/test_e4_carga_paridade.py:159`)

**A casca fica intacta:** é medida antes do patch e retorna assinaturas e mapas já materializados. Medida dentro dele, também seria mutada, pois a casca singular resolve a função plural pelo global do módulo. (`app/ponto/tests/test_e4_carga_paridade.py:55`, `app/ponto/tests/test_e4_carga_paridade.py:106`, `app/ponto/tests/test_e4_carga_paridade.py:158`, `app/escala/alimentacao.py:154`)

Há lacunas relevantes: não existe célula da fixture fora de D, logo a necessidade de **células no pad** não é testada independentemente; também não há célula fresca não salva, fluxo real do cartório, orçamento de consultas ou assert do modo booleano. (`app/ponto/tests/test_e4_carga_paridade.py:92`, `app/ponto/tests/test_e4_carga_paridade.py:107`, `app/ponto/tests/test_bug_enc_celulas_forma.py:32`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`)

**Não há dependência direta do relógio real nos testes novos:** datas/instantes são constantes, ambas as classes congelam o relógio e os julgamentos recebem `agora`. (`app/ponto/tests/test_e4_carga_paridade.py:39`, `app/ponto/tests/test_e4_carga_paridade.py:47`, `app/ponto/tests/test_e4_carga_paridade.py:100`, `app/ponto/tests/test_bug_enc_celulas_forma.py:24`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`)

**4. Classes de reprovação**

| Classe | SIM/NÃO e prova |
|---|---|
| **(a) Segundo juiz** | **NÃO introduzido.** Os registrados continuam sendo `turnos_do_colab` e `dia_encerrado`; o carregador delega ao mesmo julgamento puro. (`app/core/juizes.py:196`, `app/core/juizes.py:223`, `app/ponto/turnos.py:2153`, `app/ponto/turnos.py:2170`) |
| **(b) Segundo escritor fora da porta** | **NÃO introduzido.** A alteração troca alimentação de leitura; lavra e persistência seguem pelas portas existentes. (`app/ponto/turnos.py:2153`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`) |
| **(c) Relógio fora da borda** | **NÃO introduzido; SIM preexistente no sítio.** Os defaults internos de `timezone.now()` permanecem, apesar do corte de injeção. O cartório fornece `agora`. (`app/ponto/turnos.py:2140`, `app/ponto/turnos.py:2094`, `app/docs/CORTES.md:369`, `app/ponto/services/cartorio.py:942`) |
| **(d) Dois carregadores que podem divergir** | **SIM, preexistentes e mantidos.** Casca e lote carregam separadamente, mas usam os mesmos corpos. O selo novo verifica paridade na fixture; não elimina a duplicidade de composição das cargas. (`app/ponto/turnos.py:1919`, `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:3009`, `app/docs/BACKLOG.md:862`) |
| **(e) Selo/RED sem mordida** | **NÃO inteiramente vazio:** há gabaritos literais e controle de sensibilidade. **SIM, cobertura incompleta:** bool, pad com células e estado não salvo escapam conforme os testes acima. RED executado: **[não verificado]**. (`app/ponto/tests/test_e4_carga_paridade.py:143`, `app/ponto/tests/test_e4_carga_paridade.py:165`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`) |
| **(f) Ordem de trava** | **NÃO alterada pela linha do diff.** Ela apenas remove o argumento de uma leitura. Existe hipótese preexistente de composição Colaborador→Disputa, registrada como não reproduzida; regressão de trava nesta obra: **[não verificado]**. (`app/ponto/turnos.py:2154`, `app/docs/BACKLOG.md:767`) |
| **(g) Pouso antes do pré-requisito** | **NÃO comprovado; cumprimento [não verificado].** Exige paridade antes da cura, cura depois do pouso E4-7, RED e impacto. A documentação declara E4-7 no ar, mas não comprova aqui a ancestralidade/execução efetiva. (`app/docs/BACKLOG.md:862`, `app/docs/BACKLOG.md:861`, `app/docs/BACKLOG.md:789`) |

**5. Leis dos sítios**

O grep encontra proteção explícita para consumo da célula/ata, teto temporal e jornada pelo dia de início; L-085 nomeia `turnos_do_colab`. O registro de juízes também reserva geometria ao pareador e marcos do dia à autoridade da célula. (`app/docs/LEIS.md:95`, `app/docs/LEIS.md:100`, `app/docs/LEIS.md:117`, `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:22`)

**Não identifiquei violação nova dessas regras pela linha alterada:** ela mantém a pergunta ao pareador e a conjunção turno vivo/fim previsto, sem criar julgamento por data. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2122`)

As ressalvas são a leitura de relógio preexistente diante de `RELOGIO-INJETADO`, a cronologia exigida pela L-110 e os pré-requisitos específicos da obra. O fonte contém casos pela regra; não comprova que foram escritos/executados antes da cura nem fornece o impacto exigido. (`app/docs/CORTES.md:369`, `app/docs/LEIS.md:136`, `app/docs/BACKLOG.md:861`, `app/docs/BACKLOG.md:862`; execução e cronologia **[não verificado]**)

## 01/11 16:16 · sessao 0144ff325e9e · - · - · origem=Ronald (tmux)

- **Ordem no cartório:** `_turno_encerrado` monta e guarda o mapa uma vez, sob demanda, em `app/ponto/services/cartorio.py:939-943`. No ramo normal ele pode ser chamado em `:1180`, antes da lavra da célula atual em `:1596` (`save=False`) e do `carimbar` em `:1616`. `carimbar` chama a gravação em `app/ponto/portas/celula.py:807`; esta faz `save` e evento no mesmo `atomic`, em `:639-642`. A ressalva é o caráter preguiçoso: não afirmar que o mapa necessariamente foi calculado antes de **todas** as lavras da passagem. (`app/ponto/services/cartorio.py:937-943`, `:1136`, `:1180`)

- **Nunca bateu / dry-run:** a pergunta de encerramento em `app/ponto/services/cartorio.py:1063` está dentro do `if apply_` de `:1040`; vem antes da lavra em `:1075` e do `carimbar` em `:1097-1099`. Nesse ramo o dry-run não executa essa pergunta; é assimetria preexistente, não criada pela linha alterada. (`app/ponto/services/cartorio.py:1040`, `:1063`, `:1075`, `:1097`)

- **Fornecedores de `cels`:** todos os fornecedores externos encontrados carregam do banco: cron em `app/ponto/management/commands/processar_cartorio.py:61-65`; sombra do veredito velho dentro de `atomic` em `app/ponto/management/commands/celula_veredito_velho.py:133-145`; preenchimento de status em `app/ponto/management/commands/lavrar_status_competencia.py:48-56`; régua, restrita às células `veredito='furo'`, em `app/escala/regua_defesa.py:685-693`; unidade em `app/ponto/services/cartorio.py:1739-1754`. Internamente há segunda passada com lista ordenada em `:833` e releitura da lista original em `:898`. Nenhum desses trechos modifica célula em memória antes de passá-la ao julgamento. (mesmas referências)

- **Célula fresca ainda sem save:** há esse contrato na **grade**, que copia células, substitui sua ata em memória e alimenta diretamente `turnos_do_colab`, em `app/escala/utils.py:1829-1847`. Esse caminho não passa por `dias_encerrados`; portanto a alteração não descarta sua alimentação. No cartório, a montagem da grade ocorre em `app/ponto/services/cartorio.py:963`, mas a alimentação de encerramento continua sendo a lista original `cels` em `:942`, não `_cels_juiz` da grade. Uma divergência real dry-run/apply decorrente da nova leitura está [não verificado].

- **(a) segundo juiz: NÃO introduzido.** Os registrados são `turnos_do_colab` em `app/core/juizes.py:196` e `dia_encerrado` em `:223`; o carregador mantém `_dias_encerrados_puro`, que pergunta à mesma função pura por dia. (`app/ponto/turnos.py:2153-2155`, `:2169-2172`)

- **(b) segundo escritor: NÃO introduzido.** A alteração termina nas mesmas funções de leitura e retorna um mapa; a lavra segue pela porta existente. (`app/ponto/turnos.py:2145-2155`, `:2167-2174`; `app/ponto/services/cartorio.py:1596`, `:1616`; `app/ponto/portas/celula.py:709-713`)

- **(c) relógio fora da borda: NÃO introduzido, mas existe no sítio.** `dias_encerrados` já usa `timezone.now()` quando `agora=None`, em `app/ponto/turnos.py:2140-2141`; `dia_encerrado_motivo` também, em `:2094-2095`. O corte exige injeção de instante em juiz/grade/cartório. (`app/docs/CORTES.md:369`) O cartório injeta `agora` na chamada alterada. (`app/ponto/services/cartorio.py:942`)

- **(d) dois carregadores: SIM, preexistentes; regra paralela nova: NÃO.** Casca e lote são declarados como fornecedores do mesmo corpo em `app/ponto/turnos.py:1919-1924` e `:2159-2161`; a alteração passa a deixar o pareador carregar sua alimentação própria em `:2154`. A diferença de janela/estado entre a alimentação externa de encerramento e a carga do pareador permanece possível por desenho; ocorrência material em produção [não verificado].

- **(f) ordem de trava: NÃO alterada pelo diff.** A linha alterada apenas remove um argumento da chamada de leitura. (`app/ponto/turnos.py:2153-2155`) Há uma hipótese preexistente de composição Colaborador → Disputa via cartório, registrada expressamente como não reproduzida; não confundir isso com prova de regressão deste diff. (`app/docs/BACKLOG.md:767`)

- **(g) pouso antes do pré-requisito:** o pré-requisito específico existe: a ficha exige “RED + impacto na sombra” e determina tocar `turnos.py` somente depois do pouso E4-7. (`app/docs/BACKLOG.md:861`) A ficha do E4-7 declara pouso em 31/10, `a62862d4`, suíte 13124 e aprovação. (`app/docs/BACKLOG.md:789`) A execução efetiva dessa sequência e do impacto da cura está [não verificado].

- **Leis:** o grep em `LEIS.md` encontra L-061 (`:95`), L-073 (`:100`), L-085, que nomeia/protege `turnos_do_colab` (`:117`), L-115 (`:146`) e L-113 (`:148`), além das auditorias `:166/:190`. A mudança preserva a pergunta por turno/instante e o dia de início, em vez de acrescentar predicado por data. (`app/ponto/turnos.py:2114-2122`, `:2153-2155`) L-002 proíbe derivação própria (`app/docs/LEIS.md:60`); o corpo segue único (`app/ponto/turnos.py:2161`, `:2170`). L-110 exige casos pela regra antes do código (`app/docs/LEIS.md:136`); L-114 distingue certificado, impacto e termômetro (`:147`). O fonte dos testes declara essa intenção, mas a cronologia real e os resultados executados são [não verificado].

## 01/11 16:15 · sessao 545be1432c25 · - · - · origem=Ronald (tmux)

Censo de produção com `celulas` explícito, incluindo aliases e argumento posicional:

| Chamada | Escritor/forma/janela | Efeito do diff |
|---|---|---|
| `dias_encerrados` no cartório | `{(c.pk, x.data): x}` da lista `cels`; janela entre primeira e última célula, podendo haver lacunas. `app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942` | Muda: sem `turnos`, agora o pareador recarrega células com pad. `app/ponto/turnos.py:2153` |
| `dias_encerrados` no calendário | `_cels_mes = celulas_do_periodo([pk], data_ini, data_fim)`, chave composta; fornece também os turnos da mesma janela. `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699` | Não muda: `turnos is not None` evita chamada ao pareador nos dois códigos. `app/ponto/turnos.py:2154` |
| `turnos_do_colab` em `dia_encerrado_motivo` | Repasse posicional do parâmetro `celulas`; formato e alcance dependem do chamador, cujo contrato é `{data: CelulaDia}` com pad da janela `[data,data]`. `app/ponto/turnos.py:2090`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845` | Não muda: esta chamada não foi alterada. `app/ponto/turnos.py:2096` |
| `_tdc_pre` na montagem da grade | Converte `_cels_gr` composta para `{data: célula}`; `_cels_gr` vem de `janela_com_pad(_ini_al, ref)`. `app/escala/utils.py:1258`, `app/escala/utils.py:1260`, `app/escala/utils.py:1532` | Não muda: chamada direta ao pareador permanece. `app/escala/utils.py:1532` |
| `turnos_do_colab` após overlays da grade | `{data: célula}`; mesmo pad; fora da montagem usa célula do banco, dentro usa cópia com ata recém-montada, não salva. `app/escala/utils.py:1830`, `app/escala/utils.py:1833`, `app/escala/utils.py:1835`, `app/escala/utils.py:1846`, `app/escala/utils.py:1847` | Não muda: alimentação direta continua preservada. `app/escala/utils.py:1847` |
| Emissor de turno aberto retroativo | `{x.data: x}`, banco cobrindo `[ini−2, fim+2]`, portanto superset do pad `[ini−2, fim+1]`. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/turnos.py:1845` | Não muda: chamada direta permanece. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` |

Os seis são todos os sítios explícitos encontrados no censo AST da cópia, incluindo aliases de imports e passagem posicional; referências dinâmicas externas à cópia são `[não verificado]`.

`dias_encerrados` escreve seu resultado exclusivamente no corpo compartilhado: percorre dias, pergunta `_dia_encerrado_motivo_puro`, produz motivo quando `motivos=True`, senão `_m is None`. A alimentação composta é lida por `vinculo_do_dia` e pelo julgamento/fim previsto. `app/ponto/turnos.py:2169`, `app/ponto/turnos.py:2170`, `app/ponto/turnos.py:2172`; consulta composta em `app/escala/alimentacao.py:237`; DNA composto em `app/escala/models.py:1095`.

Leitores de produção do retorno da função de módulo:

- Cartório lê bool com default `False`; passa ao classificador e calcula `_em_curso`. `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`.
- Calendário lê motivos para `_em_curso` e trata `sem_previsto`. `app/colaboradores/services/calendario.py:701`, `app/colaboradores/services/calendario.py:709`.
- `supra_juiz` recebe bool e passa ao classificador. `app/ponto/management/commands/supra_juiz.py:121`, `app/ponto/management/commands/supra_juiz.py:232`.
- `emitir_furo_retroativo` recebe bool e passa ao classificador. `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`.
- `par_completo_da_janela` recebe bool e exclui turnos de dias não encerrados. `app/ponto/turnos.py:2183`, `app/ponto/turnos.py:2186`.

Leitores do método homônimo de lote: status usa motivos; painel usa bool. Esse método chama diretamente o corpo compartilhado, portanto não é afetado pela alteração da casca. `app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:197`, `app/core/services/painel_op.py:334`, `app/ponto/turnos.py:3018`.

Custo: o cartório passa a pagar uma consulta adicional de células por chamada efetiva de `dias_encerrados`, porque agora o pareador recebe `None` e executa `celulas_do_periodo`; essa função é um único SELECT. O cache `_enc` limita a uma carga por execução do corpo, não uma por dia. `app/ponto/turnos.py:2154`, `app/ponto/turnos.py:1882`, `app/escala/alimentacao.py:41`, `app/ponto/services/cartorio.py:939`. Células do pad podem ainda introduzir geradoras adicionais e disparar o SELECT condicional de `mais`; custo efetivo em produção `[não verificado]`. `app/ponto/turnos.py:1885`, `app/escala/alimentacao.py:123`. O lote mantém cargas próprias e corpo compartilhado, sem passar pela casca alterada. `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:2963`, `app/ponto/turnos.py:3018`.

Ordem e memória no cartório:

1. Monta a grade, cujo pareamento final usa cópias com ata fresca em memória. `app/ponto/services/cartorio.py:963`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`.
2. Consulta encerramento lazy, normalmente em `_em_curso`; este novo pareamento lê células/atas do banco, anteriores à lavra corrente. `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1937`.
3. Calcula/classifica; somente depois, no apply, lavra com `save=False` e salva via `carimbar`. `app/ponto/services/cartorio.py:1243`, `app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`, `app/ponto/portas/celula.py:807`.

Portanto existe diferença estrutural entre a ata fresca usada pela grade e a ata persistida usada por esse pareador; resultado incorreto adicional no dado fornecido `[não verificado]`. A célula corrente ainda não foi lavrada quando encerramento é consultado, e a ata fresca da grade tampouco era repassada pelo cartório antes do diff: sua alimentação era a lista original `cels`. `app/ponto/services/cartorio.py:942`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:1596`.

No ramo `nunca_bateu`, a consulta de encerramento ocorre somente dentro de `apply_`, antes da lavra; dry-run desse ramo não passa por essa consulta. `app/ponto/services/cartorio.py:1040`, `app/ponto/services/cartorio.py:1063`, `app/ponto/services/cartorio.py:1075`.

## 01/11 16:15 · sessao b7e648826626 · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem mutantes: execução e resultado efetivo [não verificado]. [nao verificado]

| Teste novo | Mutante que detecta pela estrutura/asserts | Mutante que deixa passar | [nao verificado]
|---|---|---|
| `test_regra` | Remover `mais=self._geradoras` só do lote; ignorar geradora e usar vínculo ativo; erro compartilhado que fecha Y na saída de intervalo ou mantém X aberto, pois exige aberto X=False/Y=True e motivos literais nos três instantes. `app/ponto/tests/test_e4_carga_paridade.py:129-143`; `app/ponto/turnos.py:2963-2964`. | Retornar bool invertido quando `motivos=False`: só usa `motivos=True`. Também erro só em dia diferente de D ou ramo sem previsto. `app/ponto/tests/test_e4_carga_paridade.py:107`, `:115`, `:143`. |
| `test_paridade_carga_x_casca_em_fixture_mista` | Divergência de carregamento só do lote, como retirar `mais`, mudar a identidade da saída ou perder uma batida: compara todos os seis campos do turno, mapas de motivos e dia por pk nas duas janelas. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:106-124`, `:145-148`. | Erro comum aos corpos compartilhados que mantém ambos iguais. Casca e lote chamam `_turnos_do_colab_puro` e `_dias_encerrados_puro`. `app/ponto/turnos.py:1913`, `:2153-2155`, `:3009-3019`. |
| `test_MORDE_carga_sem_mais` | É controle positivo: injeta retirada de `mais`, exige que o patch tenha sido chamado e que Y difira em turnos. `app/ponto/tests/test_e4_carga_paridade.py:155-165`. | **Sozinho, não mata uma retirada permanente de `mais` no código:** com lote já defeituoso e casca correta, a divergência exigida continua existindo. Quem mata essa retirada permanente são `test_regra` e `test_paridade`. `app/ponto/tests/test_e4_carga_paridade.py:139-148`, `:158-165`. |
| `test_RED_alimentado_igual_a_juiza_e_ao_lote` | Restaurar a linha antiga que repassa o dict composto ao pareador: constrói exatamente esse dict, compara todos os dias contra lote e D contra singular nos três instantes e duas janelas. `app/ponto/tests/test_bug_enc_celulas_forma.py:32-44`; a composição adicional da chave ocorre em `app/ponto/turnos.py:1954-1958`, enquanto o juiz busca `(colaborador_id,data)` em `app/escala/alimentacao.py:237`. | Erro compartilhado no julgamento puro, igualdade preservada; bool errado no modo sem motivos; erro apenas no ramo de `turnos=` fornecido. Todas as chamadas do teste usam `motivos=True` e nenhuma fornece `turnos`. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:43-44`; `app/ponto/turnos.py:2154`, `:2170-2172`. |
| `test_RED_regra_do_veredito_do_cartorio` | Restaurar repasse antigo; usar vínculo ativo no pareador; ignorar a pausa de Y: fixa X encerrado e Y `'turno_aberto_relogio'` às 08h. `app/ponto/tests/test_bug_enc_celulas_forma.py:46-50`. | Erro só às 03h/12h, só em Z, nos outros dias ou na conversão bool. O teste consulta apenas X/Y, D e `INSTANTES[1]`, através da helper com motivos. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:49-50`. |

O patch **alcança a carga do lote**. `_lote()` instancia `CargaDaJanela` dentro do `with`; `CargaDaJanela.__init__` faz import local de `escalas_do_periodo_por_colab` e chama a referência importada com `mais=self._geradoras`. Portanto busca o atributo já patchado de `escala.alimentacao`. `app/ponto/tests/test_e4_carga_paridade.py:111-113`, `:159-161`; `app/ponto/turnos.py:2950-2951`, `:2963-2964`.

A casca medida antes do patch fica materializada e intacta: assinatura contém datas, pks, tuplas e bools; motivos são calculados imediatamente; mapa de dias contém pk→date. Não há queryset pendente nesses resultados. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:103-109`, `:158`; `app/ponto/turnos.py:2172-2174`, `:2925-2933`. Medir a casca *dentro* do patch também a mutaria, pois `escalas_do_periodo` resolve a função plural pelo global do módulo. `app/escala/alimentacao.py:153-154`.

Os testes novos não cobrem célula fresca ainda não salva, execução real de `cartorio`, orçamento de consultas ou ramo sem motivos: a fixture salva células, atualiza a geradora por queryset e a helper relê do banco; nenhum dos asserts mede consultas. `app/ponto/tests/test_e4_carga_paridade.py:92-96`, `:127-165`; `app/ponto/tests/test_bug_enc_celulas_forma.py:32-50`. Também não há célula no pad fora de D: todas as geradoras/células da fixture são criadas para D; assim remover só alimentação das células do pad pode passar nesta fixture, embora as batidas de D+1 existam. `app/ponto/tests/test_e4_carga_paridade.py:77-79`, `:92-94`.

Não há leitura direta do relógio real nos testes novos: D e INSTANTES são construídos de constantes; ambas as classes são congeladas; chamadas de encerramento recebem `agora` explícito. `app/ponto/tests/test_e4_carga_paridade.py:39-48`, `:100`, `:107`, `:115`; `app/ponto/tests/test_bug_enc_celulas_forma.py:24`, `:33`, `:44`. Os defaults `timezone.now()` dessas portas só rodam se `agora is None`, condição não usada nesses testes. `app/ponto/turnos.py:2094-2095`, `:2140-2141`.

## 01/11 16:14 · sessao 79df0d6c3572 · - · - · origem=Ronald (tmux)

| Achado | Classe proposta | Pergunta e resposta/triagem | Evidência da cópia |
|---|---|---|---|
| 6 | **(b)** | `codex_v1.q`, item 7: “Algum chamador [...] (codigo, template, command, string em getattr) sobrou?”; `codex_v1.r`, §7 identifica exatamente as três referências. `triagem_v1.md`, item 7: “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5).” | `bin/esmeril_chamado_censo.py:135`; `bin/esmeril_chamado_censo_de_hoje.txt:38`; `app/docs/DOSSIES.md:46`. |
| 7 | **(a)** | `codex_v1.q`, item 6: “selo/RED sem mordida”; `codex_v3.q`, item 4 repete para o “diff total”. Resposta ou triagem sobre a perda da testemunha viva da classe (f): **nenhum**. É inspeção estrutural de teste, acessível na cópia, não depende de execução histórica de RED. | `app/core/tests/test_censo_escritas.py:202` explica a troca; `:210`–`:213` exige chamada AST; `:214`–`:217` procura genericamente `update()` censado sem `[relacionado]`. |
| 8 | **(e)**, pela leitura estrita da pergunta | Nenhuma pergunta pede custo ou repetição do C6. A mais próxima, `codex_v1.q`, item 3, menciona “memo `_pv_memo`”, mas pergunta se a releitura resulta **VIVA**, não se há duas avaliações. `codex_v1.r`, §2 descreve “antes salvava depois do pré-C6”, e §8 diz “Já exigia C6 morto antes”; não aponta o custo nem há triagem desse custo. | Primeira consulta: `app/chamados/reconciliador.py:398`; chamada da porta: `:405`; segunda consulta: `app/chamados/models.py:2565`; instante/via mudam a chave: `app/chamados/juizes.py:790`–`:791`. |
| 9 | **(e)** | Nenhuma `.q` pede auditoria de vocabulário do commit ou palavra proibida em comentário. Menções a `fallback` em `codex_v1.r`, §6, e `codex_v2.r`, §5, tratam o **relógio**, não esta ocorrência lexical. Resposta/triagem sobre este achado: **nenhum**. | Comentário: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:404` e `app/chamados/tests/test_porta_desfecho_pergunta.py:204`. Corpo do commit não disponível nesta cópia sem `.git`: **[nao verificado]**, além do relato fornecido pelo opus. |

As referências aos anexos acima usam seções/itens porque eles só foram entregues inline; o agente principal precisa aplicar sua convenção de numeração às linhas dos anexos. **[nao verificado]**

## 01/11 16:14 · sessao b71e20460e38 · - · - · origem=Ronald (tmux)

Proposta para 1–5: **a, b, b, b, b**; o #5 exige ressalva sobre ausência de motivo específico da triagem. (`CLAUDE.md:565`.)

1. **(a), tema presente, bypass não apontado.**  
   Pergunta: `codex_v1.q`, nº 6: “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)”; nº 2 também pede “Para cada chamador de `carimbar` / `carimbar_identidade`...”. Respostas: **nenhum** trecho aponta o bypass mediante chamada de `_escrever_carimbo` ou a ausência de restrição dos chamadores de identidade; os achados sobre o selo tratam formas de escrita que escapam. O selo permite `_escrever_carimbo` e verifica atribuições; o método escreve sem C6, enquanto o C6 está em `carimbar`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`.)  
   Atenção: a cura literal proposta pelo opus restringindo `_escrever_carimbo` apenas a dois chamadores excluiria também as chamadas legítimas atuais de reversão e descarimbo. Não é necessário julgar a cura para classificar o achado. (`app/chamados/models.py:2582`; `app/chamados/models.py:2611`.)

2. **(b), visto e reduzido a nota.**  
   Pergunta: `codex_v1.q`, nº 2: comportamento observável “quando a porta RECUSA”; nº 8 inclui “ausencia/declaracao”. Resposta: `codex_v1.r`, §2, linha da tabela `_materializar_ausencia_de_resposta`: **“Ignora o booleano. A ausência já criada/reaberta continua”**; `codex_v3.r`, §2: **“recusas individuais podem reduzir o conjunto carimbado”**. Triagem: `triagem_v3.md`, item 2c: **“Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado.”** Esse é o motivo dado para não promover o tema a correção. O retorno é ignorado e a consulta das absorvidas exige carimbo não nulo. (`app/chamados/services/materializacao.py:523`; `app/chamados/services/materializacao.py:571`.) A expressão específica “sem trilha/log” não é formulada nas respostas fornecidas: **[nao verificado]**.

3. **(b), os dois componentes foram vistos.**  
   Pergunta: `codex_v1.q`, nº 3: gravação seguida de releitura VIVA; nº 4: “instancia em memoria diferente do banco”; nº 6: ordem de trava. Resposta: `codex_v1.r`, §3: **“sem reler ou travar a pergunta”**; §4: **“exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão.”** Triagem: `triagem_v1.md`, item 3a, e `triagem_v2.md`, 5b: janela anterior, travar mudaria ordem de sete chamadores; `triagem_v3.md`, 3b: **“Nao e regressao; vai a fila”**, reconhecendo que o banco volta e a memória não. A guarda encerra antes de lavra/save. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2576`.)

4. **(b), relógio visto e remetido a complemento.**  
   Pergunta: `codex_v1.q`, nº 6: **“relogio lido fora da borda”**; repetida nas voltas seguintes. Resposta: `codex_v1.r`, §6: **“Os novos métodos têm `now()` de fallback”**; `codex_v3.r`, §4: **“Presente, embora encaminhado”**. Triagem: `triagem_v2.md`, 5a: **“a leitura do relogio MUDOU DE SITIO, nao nasceu”**, remetendo ao complemento RELOGIO-INJETADO; `triagem_v3.md`, 3c, confirma o encaminhamento. Os fallbacks existem; `encerrar_fantasma` chama a porta sem `quando`. (`app/chamados/models.py:2550`; `app/chamados/models.py:2602`; `app/chamados/models.py:2648`.) O subdetalhe “ignora o agora do chamador” não foi explicitamente nomeado nas respostas: **[nao verificado]**.

5. **(b), com ressalva: descarte agregado, motivo específico ausente.**  
   Pergunta: `codex_v1.q`, nº 6: **“derivacao paralela de fonte unica”**. Resposta: `codex_v1.r`, §6, derivação paralela: **“Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra.”** O próprio trecho cita os dois sítios exatos do opus. Triagem: `triagem_v2.md`, item 6: **“2o juiz, derivacao paralela e 2o escritor explicito: nenhum.”** Isso registra o encerramento agregado do tema, mas **motivo específico da triagem para manter as listas 5×4: [nao verificado]**. Não atribuir à triagem o argumento de preexistência que aparece na resposta Codex. A divergência está confirmada: o C6 inclui `VIA_COMPETENCIA_TRANCADA`; a lista da relavra tem quatro vias. (`app/chamados/juizes.py:1066`; `app/chamados/services/relavra_via.py:166`.)

## 01/11 15:38 · sessao 8e933ad4f1a9 · O261-E-F2 · verificador · origem=Code

**Não aprovo o selo como prova de escritor único:** a árvore passa, mas formas válidas de escrita ainda escapam, e o resolvedor produz `RecursionError` em programas finitos. A falha está na resolução de chaves e no reconhecimento das chamadas. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:96, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:142)

1. **Detector executado em memória**

Executei os 12 testes da classe estrutural: zero falhas e zero erros. Na varredura real, obtive seis ocorrências para o carimbo e quatro para a via, com `fora=[]` e nenhuma porta declarada ausente; duas das seis ocorrências do carimbo pertencem à regularização externa. Isso confirma a passagem dos testes atuais, mas os contraexemplos abaixo invalidam sua suficiência. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:44, app/chamados/services/regularizacao_ext.py:198)

Nos mutantes, `F` abaixo representa tanto `materializada_em` quanto `via_resolucao`; ambos produziram os mesmos resultados. As escritas por chave local e `dict` posicional também foram executadas contra um receptor em memória, confirmando que entregam o campo ao método enquanto o detector retorna `[]`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149)

| Forma que escapou | Resultado | Causa no detector |
|---|---|---|
| `campo='F'; qs.update(**{campo: None})` | `[]` | A chave do literal precisa ser constante; `_resolve` não é aplicado à chave. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:114) |
| `qs.update(**dict([('F', None)]))` | `[]` | A análise de `dict` só examina argumentos nomeados, ignorando o argumento posicional. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:116) |
| `PD=PerguntaDisputa; p=PD(F=None); p.save()` | `[]` | O nome do construtor não é resolvido por alias. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:144) |
| `for campo in ['F']: setattr(p, campo, None)` | `[]` | `_locais` não coleta os valores atribuídos pelo `for`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| `p.__setattr__('F', None); p.save()` | `[]` | Só a chamada denominada `setattr` recebe esse tratamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:145) |
| `d={}; d \|= {'F': None}; qs.update(**d)` | `[]` | `_locais` não incorpora a união por `AugAssign`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| Dicionário definido na função externa e usado por uma função interna | `[]` | A entrada na função interna substitui o ambiente local, perdendo a variável capturada. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:127) |

Essas lacunas excedem as exclusões declaradas de `obj.__dict__` e `SET_NULL`; não encontrei com isso um escritor clandestino **existente** na produção da cópia. A existência dessas formas no acervo de produção é [não verificado]. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:85, app/chamados/tests/test_o261e_f2_porta_carimbo.py:169)

Este programa executa normalmente, mas o detector levantou `RecursionError`, tanto com `{**d}` quanto com `dict(**d)` na terceira linha. `_resolve` protege ciclos entre nomes; `_chaves_de` reinicia essa proteção ao seguir o espalhamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:99, app/chamados/tests/test_o261e_f2_porta_carimbo.py:113, app/chamados/tests/test_o261e_f2_porta_carimbo.py:120)

```python
def mutante(qs):
    d = {'materializada_em': None}
    d = {**d}
    qs.update(**d)
```

Também medi expansão exponencial: cadeias com duas atribuições iguais por nível produziram 256, 4.096 e 65.536 achados para 8, 12 e 16 níveis. O resolvedor percorre todas as definições, sem deduplicação ou memorização; portanto pode consumir tempo e memória desproporcionais mesmo sem ciclo. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:73, app/chamados/tests/test_o261e_f2_porta_carimbo.py:101)

2. **Escritores e leitores dos sete campos**

Na tabela, “escritor” inclui atribuição em memória e persistência delegada; os chamadores que escolhem a operação continuam nos serviços, mas a atribuição do carimbo foi centralizada no modelo. (app/chamados/models.py:2527, app/chamados/models.py:2576, app/chamados/models.py:2626)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `materializada_em` | `_escrever_carimbo`; limpeza em lote. (app/chamados/models.py:2529, app/chamados/models.py:2627) | C6/lastro; estado, fechamento e ressuscitação; materialização, emissão, reconciliação e validação; mesa, cache e procedência. (app/chamados/juizes.py:624, app/chamados/juizes.py:1077, app/chamados/models.py:1421, app/chamados/models.py:1768, app/chamados/models.py:2477, app/chamados/services/materializacao.py:510, app/chamados/services/disputa_emissao.py:1539, app/chamados/reconciliador.py:583, app/chamados/services/validacao.py:182, app/chamados/services/raiox.py:133, app/chamados/veredito_lavrado.py:163, app/ponto/services/procedencia_batida.py:74) |
| `materializada_por` | `_escrever_carimbo`, pelo `_id`; limpeza em lote; exclusão do usuário por `SET_NULL`. (app/chamados/models.py:2530, app/chamados/models.py:2627, app/chamados/models.py:2762) | Preservação/reversão da porta; fotos de auditoria; passivo; procedência. (app/chamados/models.py:2552, app/chamados/models.py:2600, app/chamados/services/materializacao.py:618, app/chamados/reconciliador.py:1091, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `via_resolucao` | `_escrever_carimbo`; limpeza em lote; `encerrar_por_fato_no_espelho`; `relavrar`. (app/chamados/models.py:2531, app/chamados/models.py:2627, app/chamados/models.py:2636, app/chamados/services/relavra_via.py:123) | C6/lastro, reaberturas e idempotência; emissão, absorção, deduplicação e relavra; explicação, tranca, fábrica, cartório, flip e passivo. (app/chamados/juizes.py:635, app/chamados/juizes.py:1071, app/chamados/models.py:2674, app/chamados/models.py:2681, app/chamados/services/disputa_emissao.py:1628, app/chamados/services/materializacao.py:572, app/chamados/services/fio.py:882, app/chamados/services/relavra_via.py:116, app/chamados/services/explicador.py:95, app/chamados/services/competencia_trancada.py:244, app/chamados/services/fabrica_da_celula.py:104, app/ponto/services/cartorio.py:1429, app/ponto/services/flip_auto.py:129, app/chamados/services/passivo_validado.py:141) |
| `respondida_em` | Resposta normal, automática e recuperação pela trilha; reabertura; porta do carimbo; limpeza em lote. Há também restauração e cópia em memória. (app/chamados/services/disputa_emissao.py:1248, app/chamados/services/disputa_emissao.py:1394, app/chamados/services/disputa_emissao.py:1410, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2558, app/chamados/models.py:1488, app/chamados/models.py:2583, app/chamados/models.py:1441) | C6, fila e corrigíveis; estado e bloqueio de fechamento; validação, materialização, emissão, fio e cache. Também API, cobrança, vigias, telas e relatórios. (app/chamados/juizes.py:1047, app/chamados/juizes.py:1277, app/chamados/juizes.py:1284, app/chamados/models.py:1769, app/chamados/models.py:2479, app/chamados/services/validacao.py:363, app/chamados/services/materializacao.py:1259, app/chamados/services/disputa_emissao.py:1559, app/chamados/services/fio.py:336, app/chamados/veredito_lavrado.py:167, app/api/views.py:1009, app/chamados/services/cobranca.py:229, app/chamados/services/vigia_fio.py:630, app/chamados/views.py:1280, app/chamados/relatorios_disputa.py:20) |
| `validada_em` | Validação/devolução; invalidação por resposta nova; reabertura; autovalidação da porta; limpeza em lote; cópia/restauração em memória. (app/chamados/services/validacao.py:131, app/chamados/services/validacao.py:188, app/chamados/services/disputa_emissao.py:1260, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2561, app/chamados/models.py:1488, app/chamados/models.py:1440, app/chamados/models.py:2584) | Primeiro degrau do C6; estado e fechamento; validação, materialização, emissão e reconciliação; declaração, verbos, painel, vigia, flip e procedência. (app/chamados/juizes.py:1033, app/chamados/models.py:2475, app/chamados/models.py:1768, app/chamados/services/validacao.py:366, app/chamados/services/materializacao.py:1260, app/chamados/services/disputa_emissao.py:1620, app/chamados/reconciliador.py:584, app/chamados/services/declaracao_texto.py:208, app/chamados/services/verbos.py:246, app/chamados/services/painel.py:268, app/chamados/services/vigia_carimbo.py:98, app/ponto/services/flip_auto.py:130, app/ponto/services/procedencia_batida.py:74) |
| `validada_por` | Mesmos atos de validação, invalidação, reabertura e autovalidação; restauração em memória; `SET_NULL`. (app/chamados/services/validacao.py:132, app/chamados/services/validacao.py:189, app/chamados/services/disputa_emissao.py:1261, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2562, app/chamados/models.py:1488, app/chamados/models.py:2585, app/chamados/models.py:2742) | Fotos de validação, resposta e reabertura; porta; auditoria da absorção; verbos, painel, passivo e procedência. (app/chamados/services/validacao.py:139, app/chamados/services/disputa_emissao.py:1250, app/chamados/services/disputa_emissao.py:1373, app/chamados/models.py:2553, app/chamados/services/materializacao.py:616, app/chamados/services/verbos.py:240, app/chamados/services/painel.py:260, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `veredito_lavrado` | `lavrar`, persistência individual/lote e atribuição do backfill. A nova porta solicita a lavra e inclui o campo no mesmo `save`. (app/chamados/veredito_lavrado.py:227, app/chamados/veredito_lavrado.py:231, app/chamados/veredito_lavrado.py:207, app/chamados/management/commands/backfill_veredito_lavrado.py:138, app/chamados/models.py:2572) | Validade do cache, mesa, comparação da relavra, passivo, backfill e auditoria. (app/chamados/veredito_lavrado.py:179, app/chamados/services/raiox.py:134, app/chamados/services/raiox.py:182, app/chamados/services/relavra_via.py:122, app/chamados/services/passivo_validado.py:83, app/chamados/management/commands/backfill_veredito_lavrado.py:120, app/chamados/management/commands/auditar_invariantes_chamados.py:1169) |

Outros consumidores diretos de `respondida_em` incluem declaração textual, becos, furo sem canal, aviso, verbos, ações, cobrança manual, mensageria, suprajuiz e templates. (app/chamados/services/declaracao_texto.py:363, app/chamados/services/becos.py:30, app/chamados/services/furo_sem_canal.py:81, app/chamados/services/aviso_pendencia.py:13, app/chamados/services/verbos.py:311, app/chamados/acoes_disputa.py:95, app/chamados/views_cobrar.py:169, app/api/views_mensageria.py:569, app/ponto/supra_juiz.py:515, app/templates/chamados/partials/_pergunta_card.html:24)

**Sim, leitores intactos passam a receber valores diferentes:**

- **Fecho sem credor:** a recusa preserva texto sem `respondida_em`; a fila exclui a pergunta, a validação devolve `sem_resposta`, `.estado` permanece pendente e o texto bloqueia fechar a disputa. O teste foi invertido para aceitar essa preservação; os leitores não foram migrados. Incidência em produção: [não verificado]. (app/chamados/services/disputa_emissao.py:1638, app/chamados/juizes.py:1284, app/chamados/services/validacao.py:363, app/chamados/models.py:2479, app/chamados/models.py:1768, app/chamados/tests/test_o261_resposta_vazia.py:107)
- **Autor após ressuscitação:** agora `materializada_por` também fica nulo. `procedencia` passa a devolver outro `quem` quando dependia desse autor; não há migração desse leitor. A frase já podia indicar ausência de validação porque a data já era apagada. Impacto visível adicional em produção: [não verificado]. (app/chamados/models.py:1488, app/ponto/services/procedencia_batida.py:72, app/ponto/services/procedencia_batida.py:90)
- **Absorção por ausência:** recusas individuais podem reduzir o conjunto carimbado; `perguntas_absorvidas_por` continua selecionando pelo grupo de instante e via. Isso altera o conjunto devolvido, mas não demonstra erro: ele continua restrito às perguntas efetivamente carimbadas. (app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:569, app/chamados/services/materializacao.py:573)

A lavra granular passou para depois da autovalidação, mas não demonstrei diferença de `veredito_lavrado` causada por essa ordem: o juiz da lavra não consulta `respondida_em`, `validada_em` ou `validada_por`. (app/chamados/models.py:2556, app/chamados/models.py:2574, app/chamados/veredito_lavrado.py:219, app/chamados/juizes.py:610)

3. **Justificativas das triagens**

Há duas justificativas que precisam de correção e uma conclusão de cura que os mutantes contradizem:

- **V2, item 3 — “vias derivadas só existem depois do carimbo”: não é garantia da porta.** `relavrar` aceita a via sem verificar `materializada_em`; o modelo admite data nula e não declara restrição conjunta. O padrão dos escritores atuais não prova a premissa para qualquer entrada/acervo. Acervo contrário em produção: [não verificado]. (app/chamados/services/relavra_via.py:116, app/chamados/services/relavra_via.py:123, app/chamados/models.py:2298, app/chamados/models.py:2756, app/chamados/models.py:2776)
- **V1, nota 4 — “o atomic do chamador cobre”: excessivo para reversão em memória.** A restauração só cobre exceção/recusa do C6; lavra e `save` ficam depois dessa guarda. O rollback não executa `_desfazer_em_memoria`, e o refresh posterior da recusa só recarrega três campos. Não demonstrei uma regressão nova desse problema. (app/chamados/models.py:2564, app/chamados/models.py:2572, app/chamados/models.py:2579, app/chamados/services/materializacao.py:1024)
- **V2, itens 1/6 — selo “curado”: conclusão insuficiente.** As quatro formas acrescentadas estão cobertas, mas permanecem os escapes e a recursão demonstrados no item 1 desta resposta. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:246, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107)

Os demais encaminhamentos principais têm apoio, com estes limites:

- **Limbo:** “não inventa hora” está explícito; completar o instante exige testemunha da trilha. Isso sustenta encaminhar a decisão, mas não elimina a mudança funcional dos leitores descrita acima. (app/chamados/services/disputa_emissao.py:1400, app/chamados/services/disputa_emissao.py:1403, app/chamados/services/validacao.py:363)
- **Relógio para complemento:** o corte autoriza complemento da obra dona; as leituras internas continuam presentes. Encaminhado não significa resolvido. (app/docs/CORTES.md:369, app/docs/CORTES.md:380, app/chamados/models.py:2550, app/chamados/models.py:2602)
- **Concorrência para fila:** a porta continua julgando e salvando a instância recebida, sem trava própria. Existem sete funções chamadoras da porta não identidade; afirmar que qualquer solução necessariamente mudaria a ordem de trava de todas elas é [não verificado]. (app/chamados/models.py:2552, app/chamados/models.py:2576, app/chamados/models.py:2648, app/chamados/reconciliador.py:405, app/chamados/reconciliador.py:618, app/chamados/services/disputa_emissao.py:1566, app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1313)
- **Referências obsoletas:** o instrumento ainda declara a função removida; portanto o item permanece incompleto, como reconhecido na triagem. (bin/esmeril_chamado_censo.py:135, app/chamados/models.py:2501)

4. **Classes de reprovação no diff total**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não identificado nos caminhos alterados: a porta chama C6 e a lavra chama o juiz de lastro existente. (app/chamados/models.py:2565, app/chamados/veredito_lavrado.py:219) |
| Segundo escritor fora da porta | Nenhum escritor explícito encontrado pela varredura atual; a exclusividade não está provada porque os mutantes escapam. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:169, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149) |
| Relógio fora da borda | **Presente**, embora encaminhado: fallback nas duas portas e relógio próprio da lavra. (app/chamados/models.py:2550, app/chamados/models.py:2602, app/chamados/veredito_lavrado.py:228) |
| Derivação paralela nova | Não identificada: a lista de identidade vem da autoridade existente, e o julgamento continua delegado. (app/chamados/models.py:2524, app/chamados/models.py:2565) |
| Selo/RED sem mordida suficiente | **Presente:** os testes passam apesar dos escapes e do resolvedor vulnerável à recursão. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107) |
| Ordem de trava | A porta não acrescenta trava; inversão nova ou deadlock concreto: [não verificado]. (app/chamados/models.py:2533) |
| Pouso antes do pré-requisito | Não demonstrado no plantio granular: a recusa levanta `_Reprovada` dentro do `atomic`. Pouso operacional: [não verificado]. (app/chamados/services/materializacao.py:1109, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1172) |

Há ainda uma limitação importante da aprovação: com texto e `resposta='validar'`, a porta preenche `validada_em` **antes** do C6; seu primeiro degrau então devolve morta. Nesse caminho, a nova consulta não demonstra lastro independente nem pode reprovar pela lei do dia. Isso segue a aplicação do estado completo anunciada pela porta, portanto não o classifico isoladamente como segundo juiz ou pouso indevido. (app/chamados/models.py:2541, app/chamados/models.py:2560, app/chamados/models.py:2565, app/chamados/juizes.py:1033)

## 01/11 15:36 · sessao 6f098fe6ed12 · - · - · origem=Ronald (tmux)

Achado principal: há leitor não migrado com consequência funcional. No fecho sem credor, a recusa mantém texto e `respondida_em=None` (`app/chamados/services/disputa_emissao.py:1638`); a fila exige instante (`app/chamados/juizes.py:1284`), a validação retorna `sem_resposta` (`app/chamados/services/validacao.py:363`) e o fecho da disputa continua bloqueado pelo texto (`app/chamados/models.py:1768`). Incidência em produção: [não verificado].

A porta autoescreve `validada_em` antes de consultar C6 (`app/chamados/models.py:2561`), cujo primeiro ramo devolve morta quando esse campo existe (`app/chamados/juizes.py:1033`). Portanto `resposta='validar'` impede a nova guarda de reprovar por vitalidade, apesar da alegação do comentário de `app/chamados/services/materializacao.py:520`.

Procedência tem consumidores reais no calendário e espelho (`app/colaboradores/services/calendario.py:725`, `app/ponto/services/espelho.py:767`). Zerar `materializada_por` na ressuscitação muda o `quem` lido (`app/ponto/services/procedencia_batida.py:72`); porém `quando` já era nulo antes, e a frase já caía em “sem carimbo de validação” (`app/ponto/services/procedencia_batida.py:90`). Impacto visível adicional: [não verificado].

Mover a lavra para depois de completar resposta/validação não altera diretamente seu julgamento: `lavrar` chama o juiz de lastro (`app/chamados/veredito_lavrado.py:219`), que não consulta esses três campos (`app/chamados/juizes.py:610`). O C6 acrescentado pode carregar caches relacionais antes da lavra (`app/chamados/juizes.py:812`); não encontrei alteração adicional demonstrável do resultado da lavra por essa ordem. [não verificado]

## 01/11 15:36 · sessao bf24d52218f0 · - · - · origem=Ronald (tmux)

Triagens:

- **V2, item 3: motivo não comprovado.** `relavrar` só exige via em `VIAS_DERIVADAS`, sem exigir carimbo (`app/chamados/services/relavra_via.py:116`), e grava nova via em `:123`. O modelo permite via preenchida (`app/chamados/models.py:2298`) e `materializada_em=NULL` (`:2756`); sua `Meta` não declara restrição entre eles (`:2776`). Portanto, “essas vias só existem depois do carimbo” é premissa dos escritores atuais, não garantia da porta. Existência de acervo contrário: **[não verificado]**.

- **Limbo: encaminhamento tem apoio.** A recuperação exige instante testemunhado pela trilha, explicitamente “não inventa hora” (`app/chamados/services/disputa_emissao.py:1400`). A validação recusa ausência de instante (`app/chamados/services/validacao.py:363`), enquanto texto cru bloqueia o fechamento (`app/chamados/models.py:1767`).

- **Relógio: encaminhamento tem apoio, mas a classe continua presente.** O corte define injeção e borda (`app/docs/CORTES.md:369`) e permite complemento da obra dona (`:380`). As portas leem relógio internamente (`app/chamados/models.py:2550`, `:2602`); C6 e lavra também (`app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:228`). Selo executável específico RELOGIO-INJETADO: **[não verificado]**.

- **Concorrência: sete funções chamadoras confirmadas**, com oito sítios (`app/chamados/models.py:2648`; `app/chamados/reconciliador.py:405`, `:618`; `app/chamados/services/disputa_emissao.py:1566`, `:1638`; `app/chamados/services/materializacao.py:523`, `:1166`, `:1313`). A porta não trava nem relê (`app/chamados/models.py:2552`). O diff fornecido substitui escritas das mesmas instâncias; ocorrência de corrida/deadlock: **[não verificado]**.

- **V1, nota 4: justificativa excessiva.** A restauração cobre exceção/recusa do C6 (`app/chamados/models.py:2564`); lavra e `save` ficam depois da guarda (`:2572`). Transação não restaura memória Python. Entretanto, o item foi tratado como nota, e novidade concreta dessa falha no diff permanece **[não verificado]**.

Classes:

- **Relógio fora da borda:** presente nos pontos acima (`app/chamados/models.py:2550`, `:2602`).
- **Gate C6 sem discriminação nas autovalidações:** `resposta='validar'` preenche `validada_em` antes do julgamento (`app/chamados/models.py:2560`); C6 retorna morta imediatamente por esse campo (`app/chamados/juizes.py:1033`). É limitação concreta da aprovação; classificá-la como violação do mandato exige considerar que o mandato também autoriza aplicar a validação antes do C6 (`app/chamados/models.py:2541`).
- **Rollback do plantio antes da recusa:** sustentado; a nova `_Reprovada` ocorre dentro do `atomic` (`app/chamados/services/materializacao.py:1109`, `:1166`), com tratamento fora (`:1172`) e recusa posterior (`:1186`).
- **Segundo juiz ou derivação paralela novos:** não identifiquei nesses caminhos; a porta chama C6 (`app/chamados/models.py:2565`), e a lista de identidade é importada da autoridade existente (`:2524`).
- **Pouso antes de pré-requisito:** ocorrência operacional **[não verificado]**. [nao verificado]

## 01/11 15:35 · sessao fcc8e849bb7b · - · - · origem=Ronald (tmux)

- Não encontrei segundo juiz introduzido: a porta chama `pergunta_viva`, cuja lei permanece em `_pergunta_viva_calc`; a lavra continua chamando `_pergunta_materializada_de_fato`. (`app/chamados/models.py:2565`; `app/chamados/juizes.py:1032`; `app/chamados/veredito_lavrado.py:219`.)

- Não encontrei pouso da batida antes da aprovação no ramo alterado: `_materializar_uma` executa o plantio e `carimbar` dentro de `atomic`; recusa levanta `_Reprovada` dentro desse bloco e só é capturada depois. (`app/chamados/services/materializacao.py:1109`; `app/chamados/services/materializacao.py:1166`; `app/chamados/services/materializacao.py:1172`.)

- A checagem do C6 fica automaticamente satisfeita quando `resposta='validar'` encontra texto e validação nula: a porta preenche `validada_em` antes de chamar o juiz, e o primeiro degrau retorna morta por esse campo. Isso limita o significado da nova checagem nesses chamadores; o fluxo corresponde à aplicação do estado completo descrita na porta. (`app/chamados/models.py:2556`; `app/chamados/models.py:2561`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:1033`.)

- O ramo granular mudou a ordem da lavra em relação à autovalidação: agora responde/valida antes de lavrar. Não identifiquei diferença de `veredito_lavrado` causada por essa ordem: o juiz da lavra lê carimbo, via, resposta e testemunhas, mas não `respondida_em`/`validada_em`. (`app/chamados/models.py:2556`; `app/chamados/models.py:2572`; `app/chamados/juizes.py:609`; `app/chamados/juizes.py:755`.) A ordem antiga está somente no anexo do usuário. [não verificado: arquivo:linha do anexo]

- Um leitor indireto sensível é `perguntas_absorvidas_por`: identifica a absorção pelo grupo de carimbo que contém o escape. A nova recusa individual pode deixar irmãs mudas sem carimbo enquanto aceita a pergunta com texto autovalidada; o leitor passa a devolver somente o subconjunto carimbado. Não demonstrou inconsistência: a devolução continua restrita às perguntas efetivamente absorvidas. (`app/chamados/services/materializacao.py:509`; `app/chamados/services/materializacao.py:523`; `app/chamados/services/materializacao.py:569`; `app/chamados/services/materializacao.py:573`; `app/chamados/services/materializacao.py:579`.)

- A porta não acrescenta `select_for_update`; a janela entre julgamento e `save` permanece. Não sustento, apenas pelo código, a quantificação “travar muda a ordem de sete chamadores”. (`app/chamados/models.py:2552`; `app/chamados/models.py:2565`; `app/chamados/models.py:2576`; quantificação: [não verificado].)

- Há relógio interno nas duas portas novas, e a lavra também lê seu próprio relógio. A ausência de injeção integral é demonstrável; a execução em produção ou impacto temporal específico não foi verificado. (`app/chamados/models.py:2550`; `app/chamados/models.py:2602`; `app/chamados/veredito_lavrado.py:228`; produção: [não verificado].)

- A restauração só envolve exceção do C6 ou recusa: falha posterior de lavra/`save` deixa a instância alterada em memória; não cabe afirmar que **todo** chamador tem transação que resolve isso, e rollback de banco tampouco restaura a instância Python. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2576`; `app/chamados/models.py:2579`.)

## 01/11 15:32 · sessao 2cc3769c9f76 · O261-E-F2 · verificador · origem=Code

**Os selos passam na árvore atual, mas ainda não provam escritor único:** há escritas plausíveis que escapam e leituras que são contadas como escrita. A resolução é superficial e não distingue o objeto receptor das chamadas (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:123`).

1. **Escritas que escapam**

   Extraí o detector por AST e executei em memória, sem importar Django. As 11 provas da classe de selo passaram; a árvore retornou seis ocorrências para o carimbo e quatro para a via, com `fora=[]` e nenhuma porta faltante. Resultados efêmeros, sem arquivo de evidência: [nao verificado]. [nao verificado]

   Acrescentei virtualmente uma função escritora a `chamados/reconciliador.py`, sem alterar arquivo. Estas cinco formas escaparam **para cada um dos quatro campos**: `materializada_em`, `materializada_por`, `materializada_por_id` e `via_resolucao`; os dois selos continuaram passando nos 20 mutantes [nao verificado].

   | Forma do mutante — `CAMPO` representa cada campo | Por que escapa |
   |---|---|
   | `d = {'CAMPO': None}; alias = d; qs.update(**alias)` | `_resolve` não resolve nomes recursivamente: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`. |
   | `d: dict = {'CAMPO': None}; qs.update(**d)` | `_locais` só coleta `Assign`, não `AnnAssign`: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:70`. |
   | `d = {'CAMPO': None}; qs.update(**{**d})` | `_chaves_de` ignora expansões internas do dicionário: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:96`. |
   | `campo = 'CAMPO'; setattr(p, campo, None); p.save()` | `setattr` exige nome literal diretamente no argumento: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:124`. |
   | `p.__dict__.update({'CAMPO': None}); p.save()` | A chamada é examinada por keywords; o dicionário posicional não entra: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:126`. |

   Há também um mecanismo **declarado na produção** fora desse censo: `materializada_por` usa `on_delete=SET_NULL`, permitindo zerar o autor pela exclusão do usuário. A declaração está em `app/chamados/models.py:2759`; ocorrência ou caminho efetivamente exercido de exclusão de usuário: [nao verificado].

2. **Leituras contadas como escrita**

   Sim, em formas plausíveis. Por exemplo:

   ```python
   trilha = {'materializada_em': p.materializada_em}
   evento(antes=dict(**trilha))
   ```

   O detector acusa `dict(**trilha)` porque `dict` integra `METODOS_DE_ESCRITA`, independentemente do destino do resultado (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:40`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:130`).

   Também acusa construir `dict(materializada_em=p.materializada_em)`, preencher `trilha.update(materializada_em=...)` ou atribuir `trilha['materializada_em']=...`; nesses exemplos há somente leitura do modelo e montagem de trilha (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:119`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:128`). Os mutantes confirmaram essas acusações também para autor e via [nao verificado].

   **Não identifiquei falso positivo nas ocorrências retornadas da árvore atual:** elas correspondem às escritas listadas abaixo (`app/chamados/models.py:2529`, `app/chamados/models.py:2625`, `app/chamados/models.py:2635`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/regularizacao_ext.py:198`).

3. **Exatidão das listas; escritores e leitores da via**

   As listas correspondem aos **sítios explícitos encontrados nesta cópia**. A afirmação universal “nenhum outro escreve” não fica demonstrada pelo selo, pelos escapes da pergunta 1 (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:124`).

   | Lista | Escrita efetiva |
   |---|---|
   | `PORTA` | `_escrever_carimbo`: instante e autor, `app/chamados/models.py:2529`; `descarimbar_em_lote`: update, `app/chamados/models.py:2625`; `materializar_regularizacao_externa`: homônimo de **RegularizacaoExterna**, `app/chamados/services/regularizacao_ext.py:198`. |
   | `PORTA_VIA` | `_escrever_carimbo`, `app/chamados/models.py:2531`; `descarimbar_em_lote`, `app/chamados/models.py:2626`; `encerrar_por_fato_no_espelho`, `app/chamados/models.py:2635`; `relavrar`, `app/chamados/services/relavra_via.py:123`. |

   Nenhum escritor **explícito** adicional da via foi identificado [nao verificado]. Os leitores encontrados fora dos quatro sítios de `PORTA_VIA` são:

   | Grupo | Arquivo:linhas |
   |---|---|
   | Juízo e memo | `app/chamados/juizes.py:624`, `:635`, `:790`, `:1042`, `:1071`. |
   | Modelo | `app/chamados/models.py:1838`, `:2489`, `:2552`, `:2644`, `:2656`, `:2673`, `:2680`, `:2712`, `:2714`. |
   | Serviços de chamados | `app/chamados/services/fio.py:882`; `competencia_trancada.py:244`; `explicador.py:95`; `disputa_emissao.py:1628`; `materializacao.py:423`, `:572`, `:614`, `:1023`; `passivo_validado.py:64`, `:83`, `:140`, `:515`; `fabrica_da_celula.py:104`; `relavra_via.py:153`, `:179`, `:230`; `app/chamados/reconciliador.py:1089`. |
   | Ponto e tela | `app/ponto/services/cartorio.py:1429`; `app/ponto/services/flip_auto.py:129`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
   | Commands de chamados | `app/chamados/management/commands/relavrar_via_i6.py:58`; `reabrir_orfas_enterradas.py:67`; `dedup_perguntas_fato.py:74`; `disparar_perguntas_competencia.py:127`; `encerrar_perguntas_com_fato.py:33`; `auditar_invariantes_chamados.py:201`, `:432`, `:541`, `:954`, `:1706`. |

   A descrição “só trocam a via de pergunta já carimbada” depende de premissas dos chamadores: `encerrar_por_fato_no_espelho` não verifica o instante; seu command seleciona materializadas. `relavrar` verifica a via derivada, sem exigir `materializada_em` (`app/chamados/models.py:2633`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `app/chamados/services/relavra_via.py:116`).

4. **Docstring de `carimbar_identidade`**

   **Parcialmente correta.** Acertou que resposta por validar pode vencer a via de identidade e que `retratar_duplicata` aceita esse estado (`app/chamados/juizes.py:1047`, `app/chamados/services/fio.py:882`, `app/chamados/services/fio.py:891`).

   Continua incorreta como regra absoluta em “segue viva […] até o admin decidir”: antes da identidade, o C6 devolve **morta** quando a resposta concorda com a luz e o dia não acusa, mesmo sem decisão do admin. Validação e tranca também encerram antes (`app/chamados/models.py:2593`, `app/chamados/juizes.py:1033`, `app/chamados/juizes.py:1037`, `app/chamados/juizes.py:1055`).

   Portanto, a formulação correta seria **“pode continuar viva”**, condicionada aos degraus anteriores; a fila global ainda aplica `pergunta_viva` (`app/chamados/juizes.py:1284`, `app/chamados/juizes.py:1288`).

5. **Justificativas da triagem**

   Os números abaixo identificam itens do anexo fornecido; `triagem_v1.md` não foi localizado na cópia [nao verificado].

   - **#6, relógio — descarte insuficiente.** “Os escritores antigos já faziam” não torna fora do escopo os fallbacks das portas novas. O escopo inclui sítios criados/tocados e leitores/escritores envolvidos (`app/chamados/models.py:2550`, `app/chamados/models.py:2601`, `app/docs/CORTES.md:385`, `CLAUDE.md:559`).
   - **#3a, concorrência — descarte insuficiente pelo mesmo critério.** Ser anterior explica a origem, mas não exclui automaticamente a porta tocada, que julga e salva a instância recebida sem adquirir trava (`app/chamados/models.py:2552`, `app/chamados/models.py:2576`, `CLAUDE.md:559`). Uma corrida nova introduzida nesta volta: [nao verificado].
   - **#6, detector “curado” — cura parcial.** Os exemplos específicos foram cobertos, mas aliases, anotações e composição de dicionários continuam fora da resolução (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:70`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:96`).
   - **#3b, prosa — cura parcial**, pela exceção de concordância descrita na pergunta 4 (`app/chamados/models.py:2593`, `app/chamados/juizes.py:1056`).
   - **#1/8/9, PERGUNTA — o motivo “não inventa hora” tem apoio.** Existe recuperação com instante testemunhado pela trilha, não com `agora` arbitrário; existência dessa trilha nos casos de produção fornecidos: [nao verificado] (`app/chamados/services/disputa_emissao.py:1399`, `app/chamados/services/disputa_emissao.py:1403`).

6. **Classes que reaparecem**

   | Classe | Resultado |
   |---|---|
   | Selo sem mordida | **Sim, cobertura incompleta no delta v2**, apesar das provas positivas adicionadas: resolução superficial e ausência de análise do receptor (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `:123`). |
   | Relógio fora da borda | **Presente no diff total**, nos fallbacks novos; C6 e lavra também conservam relógios internos. O corte exige injeção e borda nominal (`app/chamados/models.py:2550`, `:2601`; `app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:228`; `app/docs/CORTES.md:369`). |
   | Segundo juiz / derivação paralela | Não identificados no caminho novo: a porta chama C6 e reutiliza a lista de identidade (`app/chamados/models.py:2524`, `:2565`). |
   | Segundo escritor explícito atual fora da porta | Não identificado; os mutantes mostram que o selo não garante sua ausência (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:126`). |
   | Ordem de trava | A porta nova não adquire trava; mudança de ordem não demonstrada (`app/chamados/models.py:2552`, `:2576`). |
   | Pouso antes do pré-requisito / RED histórico / suíte inteira | [nao verificado]. | [nao verificado]

   A recusa granular mantém `_Reprovada` dentro do `atomic`, preservando a estrutura de rollback da batida (`app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1166`, `app/chamados/services/materializacao.py:1172`).

## 01/11 15:30 · sessao ed37d9048a68 · - · - · origem=Ronald (tmux)

As listas correspondem aos sítios de atribuição explícita encontrados, com uma ressalva de escopo: `PORTA` inclui o homônimo `RegularizacaoExterna`, não só `PerguntaDisputa`. Os campos desse outro modelo estão em `app/chamados/models.py:3112`, `:3134`, `:3135`; sua escrita ocorre em `app/chamados/services/regularizacao_ext.py:198`, `:199`, `:200`.

| Lista | Sítio que efetivamente escreve |
|---|---|
| PORTA | `_escrever_carimbo`: atribui instante e autor em `app/chamados/models.py:2529`, `:2530`; `descarimbar_em_lote`: update em `:2625`, `:2626`; homônimo acima. |
| PORTA_VIA | `_escrever_carimbo`, `app/chamados/models.py:2531`; `descarimbar_em_lote`, `:2626`; `encerrar_por_fato_no_espelho`, `:2635`, persistência em `:2636`; `relavrar`, `app/chamados/services/relavra_via.py:123`, persistência em `:125`. |

Não identifiquei atribuição explícita adicional pelos nomes dos campos ou pelos setters dinâmicos examinados; ausência universal de escrita indireta permanece [nao verificado]. `passivo_validado.CAMPOS_REVERSAO` é leitura: lista os campos em `app/chamados/services/passivo_validado.py:64`, e `_reversao_de` usa `getattr` em `:83`, armazenando o snapshot em `:367`.

Há uma escrita indireta condicional que merece separar do censo de atribuições: `materializada_por` usa `on_delete=models.SET_NULL` em `app/chamados/models.py:2759`, `:2761`. Uma exclusão de `User` pode, portanto, zerar o autor pelo coletor do ORM; ocorrência e caminho executável de exclusão neste ambiente são [nao verificado]. O detector não examina essa política: só reconhece alvos de atribuição, `setattr` literal e chamadas selecionadas em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`, `:124`, `:126`.

Leitores de `via_resolucao` fora dos quatro sítios de `PORTA_VIA`:

- Juízo e memo: `app/chamados/juizes.py:624`, `:635`, `:790`, `:1042`, `:1071`.
- Modelo: `app/chamados/models.py:1838`, `:2489`, `:2552`, `:2644`, `:2656`, `:2673`, `:2680`, `:2712`, `:2714`.
- Serviços: `app/chamados/services/fio.py:882`; `competencia_trancada.py:244`; `explicador.py:95`; `disputa_emissao.py:1628`, `:1643`, `:1646`; `materializacao.py:423`, `:572`, `:614`, `:1023`; `passivo_validado.py:83`, `:140`, `:141`, `:142`, `:515`; `fabrica_da_celula.py:104`; `relavra_via.py:153`, `:179`, `:230`; `app/chamados/reconciliador.py:1089`, `:1100`.
- Ponto: `app/ponto/services/cartorio.py:1429`; `app/ponto/services/flip_auto.py:129`.
- Tela: `app/templates/chamados/partials/_card_atendimento.html:186`.
- Commands, todos sob `app/chamados/management/commands/`: `relavrar_via_i6.py:58`, `:59`, `:62`, `:66`, `:68`, `:88`; `reabrir_orfas_enterradas.py:67`, `:128`, `:141`, `:149`; `dedup_perguntas_fato.py:74`; `disparar_perguntas_competencia.py:127`; `encerrar_perguntas_com_fato.py:33`, `:42`, `:49`, `:66`, `:76`; `auditar_invariantes_chamados.py:201`, `:205`, `:432`, `:541`, `:954`, `:1706`.

A descrição “só trocam a via de pergunta já carimbada” depende dos chamadores: `encerrar_por_fato_no_espelho` não verifica `materializada_em` antes de gravar (`app/chamados/models.py:2633`, `:2635`), mas seu command seleciona materializadas (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`). `relavrar` verifica a via derivada, sem verificar instante (`app/chamados/services/relavra_via.py:116`); `relavrar_dia` também filtra pela via, usando a premissa documentada de que ela implica materialização (`:147`, `:153`).

_1434 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
