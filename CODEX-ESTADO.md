# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 01:25** · publicado em **11/11/2026 01:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 01:25 · sessao 985f431d56cb · - · - · origem=Ronald (tmux)

P6 melhora, mas deixa lacunas:

- Executei o comando em memória dos três módulos: **51 testes OK**, com os **31 mutantes mortos por `AssertionError`**. O harness exige original verde, alvo único e mensagem esperada. `app/chamados/tests/test_e4b_classe_e_memoria.py:708`.
- Os dois sobreviventes antigos agora morrem: C7 tem rodada com `trancado=False`; pai adicional é proibido por estado e chamada. `app/chamados/tests/test_e4b_classe_e_memoria.py:319`, `app/chamados/tests/test_e4b_classe_e_memoria.py:243`.
- **Mutante adicional sobrevive:** substituir `objeto_ref` por somente `str(p.pk)` manteve os 36 testes verdes, inclusive o catálogo. Isso perderia notas de novas recusas/novos fechamentos. Os testes reiniciam a fixture para cada motivo e não verificam sua mudança preservando o histórico. `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.
- **Outro sobrevivente:** inverter `sorted(carga)` para ordem decrescente manteve os 36 testes verdes. Não há caso com múltiplas PKs de limbo exigindo formato e ordem estáveis. `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.
- O `--volta1` falhou antes dos testes porque não há repositório Git acessível; a mordida contra o objeto exato `345f15a87` permanece **[nao verificado]**. O runner depende diretamente de `git show`. `app/chamados/tests/test_e4b_classe_e_memoria.py:736`.
- Relógio dos testes novos é fixo; B2d tocado está congelado. `app/chamados/tests/test_e4b_classe_e_memoria.py:22`, `app/chamados/tests/test_e4b_classe_e_memoria.py:396`, `app/chamados/tests/test_b2d_resposta_visivel.py:79`.
- P1 não torna a cascata inteira reproduzível: enumeração chama o C6 sem `hoje`; lavra lê `timezone.now()` existente. `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/models.py:1509`, `app/chamados/veredito_lavrado.py:271`.
- Os defaults vinculados ao laço preservam os asserts apresentados; a autenticação das nove alterações individuais de Ruff exige a fonte anterior separada **[nao verificado]**. `app/chamados/tests/test_e4b_classe_e_memoria.py:556`, `app/chamados/tests/test_e4b_classe_e_memoria.py:638`, `app/chamados/tests/test_e4b_classe_e_memoria.py:644`.
- **Ramo concreto da recusa nova:** `_executar_dia` registra recusa e continua os demais atos. Reproduzi em memória `recusa:credor_carrega_limbo:31` seguida de uma chamada à fábrica. Atingibilidade concorrente em ORM: **[nao verificado]**. `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`.

ORM, suíte inteira, banco/log, efeitos reais e concorrência: **[nao verificado]**. [nao verificado]

## 11/11 01:25 · sessao a79ec8ad0557 · - · - · origem=Ronald (tmux)

P2/P4 e semântica, somente leitura:

- **P2 cura a recusa já conhecida:** `motivo is None and credor.reabrir()` evita entrar nos efeitos de `reabrir`; a recusa recebe nota deduplicada. A consulta é declarada e implementada como leitura, inclusive veto e irmão. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/models.py:1318`, `app/chamados/models.py:1338`, `app/chamados/models.py:1354`.
- **Efeitos evitados:** veto escreve auditoria com deduplicação própria; admin/premissa escrevem `reabertura_recusada` a cada entrada; irmão pode migrar perguntas, anotar e auditar. Referências: `app/chamados/models.py:1155`, `app/chamados/models.py:1390`, `app/chamados/models.py:1394`, `app/chamados/models.py:1400`, `app/chamados/models.py:1411`, `app/chamados/models.py:1427`.
- **Recusa nova entre as duas consultas permanece distinta:** se a consulta externa passa e `reabrir()` recusa na consulta interna, os efeitos internos já podem acontecer; E4B registra `porta_nao_reabriu`, perdendo o motivo específico. Os testes substituem `reabrir` por mock e não cobrem essa mudança de resultado. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1845`, `app/chamados/models.py:1390`, `app/chamados/tests/test_e4b_classe_e_memoria.py:214`. Ocorrência concorrente concreta: [nao verificado].

**Formato e consumidores:**

- Escritor único nominal dos motivos: `credor_carrega_limbo:pk1,pk2[:credor_toca_exportada]` ou `credor_toca_exportada`; o retorno acrescenta `recusa:`. Os PKs saem únicos/crescentes porque a carga passa por dicionário e `sorted`. Referências: `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/services/disputa_emissao.py:1838`.
- O único parser numérico de produção encontrado é o backfill: `res.split(':')[2].split(',')`, seguido de `int`. Ordem arbitrária é aceita e a impressão usa `sorted(set(...))`; vírgula separa IDs, e elemento vazio gera `ValueError`, inclusive no planejamento fora do `try` do ato. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:282`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:309`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:358`.
- S143 apenas testa prefixos; o backfill detecta exportada por substring, reconhecendo também o sufixo combinado. Referências: `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`. Consumidores externos/reflexão: [nao verificado].

**Nota e deduplicação:**

- `tipo` e `objeto_ref` são argumentos usados no hash, não colunas persistidas; conteúdo e `evento_chave` ficam no Followup. A chave combina chamado, tipo, referência e autor; a referência E4B combina pergunta, motivo completo e `resolvido_em`. Unique parcial e tratamento de colisão implementam uma nota por chave. Referências: `app/chamados/services/nota.py:62`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:109`, `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:1698`.
- Conjunto limbo, motivo ou encerramento diferentes geram outra chave. A garantia é por pergunta + motivo + encerramento, não uma nota global por credor. `resolvido_em` é instante, `DateTimeField`; não há conversão nova para data. Referências: `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:60`.
- Conhecida a recusa, a cura produz uma nota Followup e **zero nova linha LogAuditoria**, porque não chama `reabrir`; o selo verifica essa nota com implementação real da porta, mas unique/atomic são doubles. Referências: `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/tests/test_e4b_classe_e_memoria.py:516`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`. PostgreSQL real: [nao verificado].
- Leitores genéricos: histórico lê conteúdo/criação/autoria; SLA usa somente resposta do DP, portanto a nota de sistema não reinicia prazo. Referências: `app/chamados/services/historico.py:80`, `app/chamados/services/historico.py:84`, `app/chamados/services/sla.py:17`.

**Limbo e autoridade:**

- E4B e `carimbar_resposta_pela_trilha` coincidem: timestamp nulo e texto com `.strip()` não vazio. `texto_vazio` é a autoridade existente para vazio textual. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1670`, `app/chamados/juizes.py:1441`.
- O comando histórico diverge: exclui apenas `resposta_colab=''`. Assim, `'  '` entra no conjunto medido do comando, mas não na cerca E4B nem no carimbo. O comando também exige os PKs fixos `MEDIDAS`; não é uma porta geral de cura de qualquer limbo novo. Referências: `app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`.
- Não identifiquei novo juiz nessa composição inline: ela reutiliza `texto_vazio`, exatamente como o brief autorizou; a divergência é do seletor legado. A própria porta de carimbo usa o instante testemunhado pela nota, preservando o significado de instante. Referências: `app/chamados/juizes.py:1441`, `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.

**Lacuna adversarial comprovada:**

Mutação apenas em memória de `app/chamados/services/disputa_emissao.py:1831`, trocando `for pk in limbo` por `for pk in limbo[:1]`, **sobreviveu aos 36 testes do módulo, incluindo o harness dos 31 mutantes**: rc=0, `Ran 36 tests`, `OK`. Ela omite os demais PKs de limbo da nota e da lista do backfill; os testes P4 cobrem um único PK limbo por cenário. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:610`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`, `app/chamados/tests/test_e4b_classe_e_memoria.py:662`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`. Execução realizada com `PYTHONDONTWRITEBYTECODE=1`, `FONTES_MUTADAS` e `unittest`, sem escrita de arquivos; log persistido: [nao verificado].

## 11/11 01:24 · sessao 39471fc9c9ea · FIO-E0-V4-E3 · leitura · origem=Code

**A divisão sustentada é: 3 casos (a), 2 casos (b) com limite de verificação da sequência E1, 8 casos (c) — ADMIN/DP pela ausência — e 2 casos com “sem regra” específica de encaminhamento.** As regras são GRUPO-D, HX-ADMIN/PERGUNTA-RESPONDIDA-E-MARCO-ACESO e COBERTURA-PARCIAL-PERIODO, respectivamente. (`app/docs/CORTES.md:263`, `app/chamados/juizes.py:1341`, `app/docs/CORTES.md:319`)

Nas referências abaixo, `casos_v15.out:N` significa a enésima linha `DIA` do **anexo recebido**. O JSON contém instâncias; o que está vazio são suas `acoes`. Portanto, “sem instâncias” não descreve essas saídas. (`casos_v15.out:1`, `casos_v15.out:12`)

As regras usadas na tabela são:

- **A — GRUPO-D:** “dia acusando sem pergunta e sem chamado”; responsabilidade expressa: “cura pelo emissor que le a lampada”, contando “celula acusa + 0 pergunta + 0 chamado”. A regra geral diz: “Emissao, pergunta e chamado nascem do sinal da celula”. Isso decide **o responsável pela existência do fio**, sem especificar qual pergunta resolve `FATO_EM_AUSENCIA`. (`app/docs/CORTES.md:263`, `CLAUDE.md:168`)
- **B — resposta viva:** HX-ADMIN preserva a resposta sem decisão explícita do ADMIN na célula; PERGUNTA-RESPONDIDA-E-MARCO-ACESO diz que, discordando ou sem como conferir, “segue viva para o admin decidir”. O corte SMOKE-FIO exige apresentar `Validar` quando segue viva e respondida. (`app/chamados/juizes.py:1338`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1350`, `app/docs/CORTES.md:337`)
- **P — parcial sem período:** “hora_ini/hora_fim [...] preenchidos pelo ADMIN na aprovacao”; sem hora, “o dia vai a lista do admin como CADASTRO”. Depois de aprovada, a edição usa a mesma porta `ausencia.py::decidir`. O cartório explicita: “parcial sem periodo vai a lista do admin, nunca chamado”. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:323`, `app/ponto/services/cartorio.py:379`)
- **S — sem regra:** as regras encontradas de `FATO_EM_AUSENCIA` definem quando acusar, mas não escolhem ator/ato para resolver o conflito residual com perguntas mortas. **Sem regra específica localizada para esse encaminhamento.** (`app/docs/CORTES.md:315`, `app/docs/CORTES.md:334`; abrangência da ausência de regra: `[nao verificado]`)

| Colab | Dia | a/b/c — quem devia agir | Regra | Arquivo:linha — dado e ramo |
|---|---|---|---|---|
| col177 | 2026-11-08 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:1`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |
| col252 | 2026-09-29 | **b\* — ADMIN**, resposta viva p38162 | B — resposta viva | `casos_v15.out:2`; `app/chamados/juizes.py:1350`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col258 | 2026-09-28 | **c — ADMIN/DP**, declarar período da ausência | P — parcial sem período | `casos_v15.out:3`; `app/docs/CORTES.md:319`; descarte: `app/chamados/services/tarefa_do_dia.py:244` |
| col258 | 2026-10-19 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:4`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col259 | 2026-10-14 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:5`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col259 | 2026-10-15 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:6`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col292 | 2026-10-22 | **c — sem regra** específica que escolha ator/ato residual | S — perguntas C6 mortas | `casos_v15.out:7`; `app/docs/CORTES.md:315`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col292 | 2026-10-28 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:8`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |
| col354 | 2026-10-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:9`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col354 | 2026-10-30 | **c — sem regra** específica que escolha ator/ato residual | S — pergunta C6 morta | `casos_v15.out:10`; `app/docs/CORTES.md:315`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col368 | 2026-09-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:11`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col655 | 2026-09-30 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:12`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col655 | 2026-10-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:13`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col841 | 2026-10-10 | **b\* — ADMIN**, resposta viva p42367 | B — resposta viva | `casos_v15.out:14`; `app/chamados/juizes.py:1350`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col889 | 2026-10-25 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:15`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |

**Por classe:**

**A — três dias sem pergunta nem chamado.** A obrigação genérica do emissor está escrita. A função de emissão é `perguntar_pela_celula`, que consulta `motivos_a_perguntar`; esta retorna `[]` quando `dia_sem_trabalho(cel)` responde verdadeiro. O cartório possui gatilhos de chamados para `FURO_*`/limbo, mas esses ramos não recebem o protesto isolado `FATO_EM_AUSENCIA`. (`app/chamados/services/fabrica_da_celula.py:178`, `app/chamados/services/fabrica_da_celula.py:207`, `app/chamados/services/fabrica_da_celula.py:112`, `app/ponto/services/cartorio.py:1707`, `app/ponto/services/cartorio.py:1773`)

O anexo identifica `dia_sem_trabalho` como descarte **da tarefa**, nos três dias. Não traz a ata completa nem a execução histórica da fábrica: afirmar que aquele mesmo guarda efetivamente impediu cada emissão histórica é **[nao verificado]**. `trabalha=True` sozinho não afasta o guarda, pois o juiz também lê `ata.tipo_dia` e `cobertura_parcial`. (`casos_v15.out:1`, `casos_v15.out:8`, `casos_v15.out:15`, `app/chamados/juizes.py:435`)

**B — dois dias com resposta viva.** Há dever escrito de decisão pelo ADMIN; a tarefa interrompe a instância em `dia_sem_trabalho` **antes** de alcançar a pergunta e `_pela_decisao`. (`casos_v15.out:2`, `casos_v15.out:14`, `app/chamados/juizes.py:1350`, `app/chamados/services/tarefa_do_dia.py:241`, `app/chamados/services/tarefa_do_dia.py:281`)

O asterisco preserva um limite: o anexo não informa a última `DecisaoPergunta`, o passivo da projeção, a identidade completa nem a FK `PerguntaDisputa.celula`. Assim, o dever humano tem regra, mas **não está provado qual saída E1 exata essas duas perguntas produziriam**: passivo sem linha devolve indeterminação; última resposta textual devolve `validar/admin`. (`app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/services/tarefa_do_dia.py:132`, `app/chamados/services/tarefa_do_dia.py:337`; valores desses campos nos dois casos: `[nao verificado]`)

**P — oito parciais.** A ação pertence ao ADMIN/DP pela ausência, e a ausência de pergunta é compatível com a regra específica. `parcial_so_lista` retorna verdadeiro para `sem_periodo`; a fábrica então retorna `[]`, “sem pergunta, sem pai, sem push”. A tarefa descarta as instâncias em `lampada=True`, razão `aceso`. (`app/docs/CORTES.md:319`, `app/ponto/services/cartorio.py:398`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/tarefa_do_dia.py:244`)

O corte não manda projetar essa edição de cadastro como tarefa de marco. O contrato de `tarefa_do_dia` é por **instância de marco**, enquanto a porta expressamente escolhida para preencher o período é `ausencia.py::decidir`. A presença concreta dos oito dias na lista, e a ausência específica que cada um abriria, são **[nao verificado]**. (`app/docs/CORTES.md:426`, `app/docs/CORTES.md:323`)

**S — dois dias com perguntas mortas.** O anexo não mostra a razão completa da morte C6; não permite concluir que se deve reabrir, ressuscitar ou emitir outra pergunta. Os degraus do C6 incluem validação, tranca, identidade e materialização antes de considerar o veredito acusador. Ator/ato residual: **sem regra específica localizada**. (`casos_v15.out:7`, `casos_v15.out:10`, `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1336`, `app/chamados/juizes.py:1362`, `app/chamados/juizes.py:1368`; regra residual: `[nao verificado]`)

`dono=ESTRUTURA` é o dono medido da divergência, não uma atribuição automática de tarefa ao ADMIN. A ação de preencher período vem do corte específico; isso não reclassifica o resultado do oráculo informado no anexo. (`app/docs/LEIS.md:122`, `app/docs/CORTES.md:319`, `casos_v15.out:3`)

**Quem escreve e quem lê os campos envolvidos nesta decisão:**

| Campo | Escrita | Leitura relevante |
|---|---|---|
| `CelulaDia.veredito` | `classificar_dia` produz os códigos; o mapa deriva `indefinida`/`fato_em_ausencia`; o cartório entrega à porta canônica `lavrar_veredito`, que atribui o campo. (`app/ponto/supra_juiz.py:388`, `app/ponto/supra_juiz.py:404`, `app/escala/models.py:1615`, `app/escala/models.py:1619`, `app/ponto/services/cartorio.py:1813`, `app/ponto/portas/celula.py:1035`) | Retenção da folha, guarda da fábrica e C6. A grade transporta o veredito; a tarefa não possui um ramo direto para esses dois vereditos. (`app/folha/export.py:1233`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/juizes.py:1372`, `app/escala/services/leitor_celula.py:424`, `app/chamados/services/tarefa_do_dia.py:202`) |
| `CelulaDia.trabalha` | Porta de geração: criação/atualização futura; regeneração; edição humana; reversão; tratamento de fora de vigência. (`app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:873`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:243`) | `dia_sem_trabalho` lê junto com a ata. O leitor da célula transporta esse insumo; a tarefa pergunta ao juiz por esse transporte. (`app/chamados/juizes.py:435`, `app/escala/services/leitor_celula.py:428`, `app/ponto/supra_juiz.py:65`, `app/chamados/services/tarefa_do_dia.py:202`) |
| `CelulaDia.protestos` | Códigos de `classificar_dia`; persistência canônica em `lavrar_veredito`. (`app/ponto/supra_juiz.py:388`, `app/ponto/supra_juiz.py:404`, `app/ponto/portas/celula.py:1011`, `app/ponto/portas/celula.py:1034`) | A folha cruza protestos com os códigos não cobráveis. A tarefa trabalha com lâmpadas/perguntas/decisões, sem consultar diretamente `protestos`. (`app/folha/export.py:1245`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:224`) |
| `PerguntaDisputa.celula` | `abrir` busca a célula do dia do fato, atribui no reuso e na inserção; o rechaveamento também altera a FK. (`app/chamados/models.py:2893`, `app/chamados/models.py:2965`, `app/chamados/models.py:3014`, `app/chamados/services/fio.py:1034`) | C6 lê célula/veredito/via; C17 seleciona e agrupa pela FK. **A tarefa carrega perguntas por colaborador, `data_evento` e `conversa=False`, sem filtro pela FK.** (`app/chamados/juizes.py:1338`, `app/chamados/juizes.py:1502`, `app/chamados/services/tarefa_do_dia.py:324`) |
| `instancias` da tarefa | Projeção em memória: `instancias_do_turno` combina posição, lâmpada e teto; não é estado persistido. (`app/chamados/services/tarefa_do_dia.py:7`, `app/chamados/services/tarefa_do_dia.py:91`, `app/chamados/services/tarefa_do_dia.py:390`) | `_tarefa_pura` percorre as instâncias; `_portas_da_tarefa` reúne suas ações para produzir `fio`, `fila`, `verbos` e `rota`. (`app/chamados/services/tarefa_do_dia.py:235`, `app/chamados/services/tarefa_do_dia.py:425`) |
| Lâmpada e identidade de instância | Cartório produz as lâmpadas e lavra a ata; o leitor transporta `acesa` como `acesa_lavrada`; a tarefa transforma em `lampada`. `instancia_marco` da pergunta é derivada na inserção. (`app/ponto/services/cartorio.py:565`, `app/ponto/services/cartorio.py:704`, `app/escala/services/leitor_celula.py:137`, `app/chamados/services/tarefa_do_dia.py:84`, `app/chamados/models.py:3440`) | A tarefa usa `lampada` nos descartes e `instancia_marco` para associar perguntas. Lâmpada acesa pode ser dispensa por ausência, sem batida realizada. (`app/chamados/services/tarefa_do_dia.py:244`, `app/chamados/services/tarefa_do_dia.py:106`, `app/ponto/services/cartorio.py:558`) |

Para a classe P, os insumos adicionais têm caminho explícito: ADMIN escreve `hora_ini/hora_fim` por `decidir → _aplicar_periodo`; a grade escreve `cobertura_parcial`/`periodo_parcial`; o cartório transporta para a ata; `parcial_so_lista` lê esses carimbos. (`app/ponto/services/ausencia.py:891`, `app/ponto/services/ausencia.py:924`, `app/escala/utils.py:1230`, `app/ponto/services/cartorio.py:732`, `app/ponto/services/cartorio.py:395`)

**Compatibilidade dos dois casos (b) com o dict existente:** sim, a ação `validar`, o ator `admin` e as chaves `{acao, de, porta}` já existem. `_pela_decisao` já produz essa combinação para uma última resposta textual. Não exige chave nova. (`app/chamados/services/tarefa_do_dia.py:49`, `app/chamados/services/tarefa_do_dia.py:111`, `app/chamados/services/tarefa_do_dia.py:132`)

A representação cabe mesmo com `porta=None`: isso acontece quando a âncora está fora da fila, e o resumo ainda pode ser `fio='admin'`. **Representar a obrigação não demonstra uma porta alcançável**; nos dois registros, a âncora está resolvida e `porta=null`. (`app/chamados/services/tarefa_do_dia.py:125`, `app/chamados/services/tarefa_do_dia.py:425`, `casos_v15.out:2`, `casos_v15.out:14`)

Os três arquivos citados **ainda não consomem esse dict nesta cópia**; são fontes da tarefa ou mantêm respostas próprias. O módulo declara os respondedores anteriores vivos até a E3. (`app/chamados/services/tarefa_do_dia.py:27`)

| Arquivo | O que lê atualmente |
|---|---|
| `painel.py` | Estado do chamado (`VIVOS`/`COBRANCA`), `silenciado_ate` e relógio; o marcador lê máximos de criação/atualização/resolução dos chamados e resposta/materialização/validação das perguntas. Fornece a fila à tarefa. (`app/chamados/services/painel.py:63`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:188`, `app/chamados/services/tarefa_do_dia.py:363`) |
| `declaracao_texto.py` | Grade: `isento_ponto`, `dias`, `data`, `tipo_dia`, cobertura parcial, `celulas`, `marcos`, `missing`, tipo e hora. Compartilha a função de posição do marco com a tarefa. (`app/chamados/services/declaracao_texto.py:75`, `app/chamados/services/declaracao_texto.py:100`, `app/chamados/services/declaracao_texto.py:118`) |
| `chamados/juizes.py` | `alimentar_fila` lê `particionar_verbos(...)[mapa]`, PK/FK dos chamados na fila e fornece `verbo`, `fila_ids`, `fila_cel`. C17 lê perguntas por FK, C6, âncora e presença no app; produz seu próprio `{rota, fila, verbos, fio}`. Seu campo `instancias` é um mapa de colaboradores, diferente das instâncias de marco. (`app/chamados/juizes.py:1452`, `app/chamados/juizes.py:1499`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`) |

## 11/11 01:21 · sessao bdbe00fb78b9 · - · - · origem=Ronald (tmux)

Leitura somente; nenhum acesso a banco/log. As observações de produção abaixo vêm do anexo fornecido, sem arquivo local numerado. [não verificado]

**Ramos e regras**

- Nos sete `fato_em_ausencia`, `razao='dia_sem_trabalho'` identifica o descarte em `_tarefa_pura`: calcula `sem_trabalho` em `app/chamados/services/tarefa_do_dia.py:202` e interrompe cada instância em `:241-243`, antes de ler a pergunta ou decisão em `:268-283`.
- Nos oito `PARCIAL_SEM_PERIODO`, `razao='aceso'` identifica o descarte em `app/chamados/services/tarefa_do_dia.py:244-246`, também anterior à cobertura e às decisões (`:258-283`).
- `fio='nada'` resulta de nenhuma ação e `nao_sei=False`; não resulta diretamente da inexistência de pergunta: `app/chamados/services/tarefa_do_dia.py:425-434`.
- O corte específico das parciais decide **(c), ADMIN pela lista de cadastro**: literal “hora_ini/hora_fim no lancamento, preenchidos pelo ADMIN na aprovacao”; sem período, “o dia vai a lista do admin como CADASTRO ('parcial sem periodo'), nunca FATO_EM_AUSENCIA”. `app/docs/CORTES.md:319`. A edição posterior à aprovação é explicitamente pela mesma porta `ausencia.py::decidir`, com trilha. `app/docs/CORTES.md:323`.
- Para os dois dias com resposta viva (`col252/29-09`, p38162; `col841/10-10`, p42367), existe fundamento para **(b), ADMIN validar a resposta já existente**: HX-ADMIN faz resposta perder apenas para decisão explícita do admin na célula; cartório não equivale a admin (`app/chamados/juizes.py:1338-1353`). A pergunta respondida só morre por concordância com a luz se o dia não acusa (`:1279-1286`, `:1321`). O corte reforça literalmente “segue VIVA, so Validar” e “a resposta do colab so perde para decisao do admin NA CELULA”, embora no contexto de desligamento (`app/docs/CORTES.md:342`).
- **Limite:** o anexo não mostra `DecisaoPergunta`, texto da resposta, passivo da projeção nem identidade `via_resolucao`. Portanto não prova que o ramo `_pela_decisao` executaria `VALIDAR`: projeção sem linha retorna `nao_sei` em `app/chamados/services/tarefa_do_dia.py:127-135`; a carga usa a sequência e o passivo em `:337-347`. [não verificado: esses campos nos dois dias]
- Para os três `fato_em_ausencia` sem pergunta nem chamado (`col177`, `col292/28-10`, `col889`), o corte **GRUPO-D-E-A-OUTRA-METADE** nomeia expressamente o **emissor**: “dia acusando sem pergunta e sem chamado”, “cura pelo emissor que le a lampada”. `app/docs/CORTES.md:263`. A obrigação geral nasce do sinal da célula: `CLAUDE.md:167-170`.
- `trabalha=True` não implica `dia_sem_trabalho=False`: o juiz também lê `ata.tipo_dia`, exceto a ausência com `cobertura_parcial`; `app/chamados/juizes.py:426-441`; corte literal `app/docs/CORTES.md:416`. O anexo não traz o `tipo_dia` bruto. [não verificado]

**Dict existente / compatibilidade para (b)**

`validar` + `admin` já existem (`app/chamados/services/tarefa_do_dia.py:49-54`), usando a forma `{'acao','de','porta'}` (`:111-112`). `_pela_decisao` já emite essa ação para resposta textual em `:132-135`. A porta pode ser `None` se a âncora não pertence à fila (`:125-126`); mesmo sem porta inteira, a ação produz `fio='admin'` (`:425-434`). Portanto atribuir validação por instância cabe nas chaves e atores existentes; existência de uma porta alcançável para os dois dias não foi comprovada. [não verificado: porta utilizável]

O vocabulário completo de ações é `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`; atores `sistema`, `colaborador`, `admin`; portas de sistema `emissor`, `cartorio`, `materializacao`. `app/chamados/services/tarefa_do_dia.py:49-54`. Não existe ação nomeada “editar período” nem “decidir conflito fato/ausência” nesse vocabulário. `:49-54`.

**Censo dos três arquivos solicitados**

Nenhum dos três é leitor do dict de `tarefa_do_dia` nesta cópia; o próprio módulo declara que os leitores ainda não migraram (`app/chamados/services/tarefa_do_dia.py:27-35`).

| Arquivo | Leitura atual relevante |
|---|---|
| `painel.py` | `ChamadoColaborador.status_local ∈ VIVOS` (`app/chamados/services/painel.py:135-155`); fila = `status_local ∈ COBRANCA`, excluindo silêncio futuro (`:188-190`); marca de perguntas lê máximos de `respondida_em`, `materializada_em`, `validada_em` (`:63-68`). É fornecedor da fila da tarefa, não consumidor do dict. |
| `declaracao_texto.py` | `grade_da_celula`, `isento_ponto`, dias por `data`, `tipo_dia`, `cobertura_parcial`, `celulas`, `marcos`, `missing`, tipo/hora; devolve faltantes (`app/chamados/services/declaracao_texto.py:100-139`). Compartilha `posicao_do_marco` com tarefa (`:75-83`) e mapa motivo/posição (`:42-57`). |
| `juizes.py` | `alimentar_fila` lê `particionar_verbos(qs_vivos())['mapa']` e `qs_na_fila().values_list('pk','celula_id')`, devolve `verbo`, `fila_ids`, `fila_cel` (`app/chamados/juizes.py:1452-1470`), fornecedor da tarefa. C17 carrega perguntas por `celula_id`, aplica C6, consulta presença no app (`:1502-1522`) e devolve suas próprias `rota`, `fila`, `verbos`, `fio` (`:1575-1589`). |

Atenção: `alimentar_portas()['instancias']` é um mapa de objetos `Colaborador`, não as instâncias de marco da tarefa. `app/chamados/juizes.py:1499-1508`.

**PerguntaDisputa.celula e instâncias**

| Campo | Escritor | Leitor |
|---|---|---|
| `PerguntaDisputa.celula` | Nascimento/reuso pela porta `PerguntaDisputa.abrir` (`app/chamados/models.py:2776-2781`); busca célula do colaborador e dia do fato em `:2893-2896`, atualiza FK do reuso em `:2965-2967`, insere em `:3014-3017`. Rechaveamento explícito também troca FK em `app/chamados/services/fio.py:1034-1043`. | C6 acessa `p.celula`, veredito e via (`app/chamados/juizes.py:1338-1340`, `:1371-1375`); C17 agrupa por FK (`:1502-1505`). A tarefa carrega perguntas por `(colaborador_id,data_evento,conversa=False)`, não por FK (`app/chamados/services/tarefa_do_dia.py:324-329`). |
| Instâncias da tarefa | Derivação pura `instancias_do_turno`: posições das lâmpadas da grade, tipo/hora e casamento com teto (`app/chamados/services/tarefa_do_dia.py:57-102`); carga em `:390-394`; não são estado persistido (`:7-9`). | `_tarefa_pura` percorre instâncias em `:235-240`; `_portas_da_tarefa` reúne suas ações em `:425`. |
| Lâmpada usada nas instâncias | A tarefa transporta `acesa_lavrada` da grade em `app/chamados/services/tarefa_do_dia.py:84-88`; a grade lê `ata.lampadas[].acesa` em `app/escala/services/leitor_celula.py:114-137`. | Triestado decide `aceso`/`lampada_nao_sabe` em `app/chamados/services/tarefa_do_dia.py:244-250`. |
| Identidade de instância da pergunta | Mapa motivo→posição em `app/chamados/services/declaracao_texto.py:42-57`; inserção deriva identidade no `save`, conforme `app/chamados/models.py:3009-3010`. | `_pos_da_pergunta` usa `p.instancia_marco` ou mapa do motivo (`app/chamados/services/tarefa_do_dia.py:106-108`), agrupa por posição em `:233-234`. |

A tarefa não lê `protestos` nem `cel.veredito` diretamente; lê a grade e as lâmpadas, o juiz de dia sem trabalho, cobertura, perguntas, decisões e lavra de materialização. `app/chamados/services/tarefa_do_dia.py:163-176`, `:202-211`, `:224-288`, `:348-352`.

## 11/11 01:21 · sessao 1a948378d99c · - · - · origem=Ronald (tmux)

Parecer de leitura:

- **Três FATO sem pergunta e sem chamado**: a responsabilidade **genérica do emissor** tem regra literal em `app/docs/CORTES.md:263`: “cura pelo emissor que le a lampada”, incluindo o contador “celula acusa + 0 pergunta + 0 chamado”. O próprio censo dessa linha inclui `fato_em_ausencia`. Essa cláusula não condiciona sua aplicação ao `lampada=False` das instâncias. **Qual pergunta/chamado e qual ação humana deve nascer para o conflito FATO_EM_AUSENCIA: sem regra específica encontrada** em LEIS/CORTES. O histórico efetivo da não emissão nesses três dias permanece [não verificado].

- **Dois FATO com resposta C6 viva**: há regra para leitura da pergunta viva e apresentação ao ADMIN: `app/docs/CORTES.md:316`, FIO-LE-O-JUIZ-6984, manda carregar “pela pergunta_viva, nao pelo estado da disputa”, sem reabrir; `app/docs/CORTES.md:337`, item 3, explicita “respondida, so falta Validar”; `app/docs/CORTES.md:342` reafirma “segue VIVA, so Validar” e que resposta só perde para decisão ADMIN na célula. A tarefa corta antes de examinar a pergunta em `app/chamados/services/tarefa_do_dia.py:241`–243. **A última DecisaoPergunta, pertencimento real pela FK e passivo da projeção não estão no anexo** [não verificado]; portanto, não identificar o ramo `_pela_decisao` como provado para essas perguntas.

- **Oito PARCIAL_SEM_PERIODO**: **c = ADMIN/DP, preencher período pela lista e porta da ausência**, com regra específica. `app/docs/CORTES.md:319`, COBERTURA-PARCIAL-PERIODO: “hora_ini/hora_fim [...] preenchidos pelo ADMIN na aprovacao”; sem hora, “o dia vai a lista do admin como CADASTRO ('parcial sem periodo')”. `app/docs/CORTES.md:323`, JUIZ-PERIODO-DA-PARCIAL(ii), exige edição posterior da aprovada pela mesma `ausencia.py::decidir`. O emissor deve calar: `app/ponto/services/cartorio.py:379`–382, “NUNCA chamado”, sem condição de fato novo; implementação retorna `True` em `:398`–399, lida pela fábrica em `app/chamados/services/fabrica_da_celula.py:123`–125. A tarefa descarta a instância acesa em `app/chamados/services/tarefa_do_dia.py:244`–246. A regra específica não manda tarefa fabricar pergunta de marco para essa classe.

- **Dois FATO com perguntas C6 mortas**: **sem regra específica de ator/ação que resolva o conflito residual** encontrada. Não inferir reabertura nem novo pedido de marco. `app/docs/CORTES.md:364`, CODEX-T16-T17-REGISTRO, manda medir “celula acusando e pergunta morta por identidade sem reparador”; não decide o ator. Motivo concreto da morte C6 nesses dois dias [não verificado]: faltam vias, materializada_em, respostas, decisões, `PerguntaDisputa.celula` e ata completa.

Distinções necessárias:

- `dono=ESTRUTURA` é dono da divergência; a lei específica pode atribuir o ato de cadastro ao ADMIN. `app/docs/LEIS.md:122` distingue donos e diz que CADASTRO/BATIDA são lista do ADMIN; não converte automaticamente ESTRUTURA em tarefa humana.
- `lampada=True` da instância não prova batida realizada: a ausência pode dispensar e acender “sem luz”, `app/ponto/services/cartorio.py:558`–562.
- Vigia causa **d** não significa dono CADASTRO: é “colab na lista, dia fora”, sem pergunta viva, `app/chamados/services/vigia_fio.py:523`–526 e `:677`–678. Causa **c** é pergunta viva respondida com âncora terminal, `:675`–676.
- `sem_porta` é resultado de alcançabilidade, não julgamento de quem devia emitir: corte literal em `app/chamados/juizes.py:1546`–1551; acesso manual ao chamado fechado e “Resolver dia” ficam fora dessa porta em `:1569`–1571.
- C6 mata validada em `app/chamados/juizes.py:1325`–1326; preserva resposta textual sem decisão ADMIN da célula em `:1341`–1353; acusações da célula dão viva em `:1372`–1375, depois dos degraus anteriores.

Porta existente para os oito parciais: escritor de hora é `_aplicar_periodo`, `app/ponto/services/ausencia.py:891`–894; `decidir` recebe os horários em `:897`–898, permite editar aprovada em `:922`–925 e grava trilha em `:929`–934.

## 11/11 01:21 · sessao 32dd6af07060 · - · - · origem=Ronald (tmux)

Leitura concluída; referências centrais:

| Classe | Regra literal e alcance | Emissão atual |
|---|---|---|
| 8 `PARCIAL_SEM_PERIODO` | “hora_ini/hora_fim [...] preenchidos pelo ADMIN”; sem hora, “lista do admin como CADASTRO”. A edição após aprovação usa `ausencia.py::decidir`. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:323`) | `parcial_so_lista` retorna `True` para `sem_periodo`; fábrica retorna `[]`, expressamente “sem pergunta, sem pai, sem push”. (`app/ponto/services/cartorio.py:370`, `app/ponto/services/cartorio.py:398`, `app/chamados/services/fabrica_da_celula.py:123`) |
| 7 `FATO_EM_AUSENCIA` | Regras encontradas determinam quando acusar: parcial só acusa com fato dentro do período; resposta `nao_sei` conserva acusação anterior. Não encontrei regra específica escolhendo ator/ato para resolver esse conflito. (`app/docs/CORTES.md:315`, `app/docs/CORTES.md:334`; escolha do ato: **sem regra** no material examinado) | Fábrica descarta `dia_sem_trabalho`; chamado automático só possui gatilhos `FURO_*`/limbo, sem gatilho exclusivo para `FATO_EM_AUSENCIA`. (`app/chamados/services/fabrica_da_celula.py:112`, `app/ponto/services/cartorio.py:1707`, `app/ponto/services/cartorio.py:1773`) |

Para as parciais, **(c), ADMIN/DP pela lista de cadastro**, tem fundamento literal; a obrigação específica de a `tarefa_do_dia` projetar esse ato não aparece nesse corte. O corte do juiz fala em ações por **instância de marco**. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:426`)

A regra geral manda emissão derivada da célula; O261 explicita fio obrigatório e responsabilidade do emissor para seu grupo D. Isso não escolhe sozinho o ato específico de resolver `FATO_EM_AUSENCIA`. (`CLAUDE.md:168`, `app/docs/CORTES.md:263`)

Escritores e transporte:

- `veredito`/`protestos`: códigos nascem em `classificar_dia` — parcial em `app/ponto/supra_juiz.py:388` ou `:441`; fato em ausência em `:404` ou `:444`. Mapa para veredito em `app/escala/models.py:1615` e `:1619`; cartório chama lavra em `app/ponto/services/cartorio.py:1813`; escritor canônico grava em `app/ponto/portas/celula.py:1034`.
- `trabalha`: criação em `app/ponto/portas/celula.py:854`, atualização futura em `:873`, regeneração em `:272`, edição humana em `:603`, reversão em `:788`.
- Período parcial: `ausencia.py::decidir` chama escritor único `_aplicar_periodo` em `app/ponto/services/ausencia.py:925`; atribuição em `:893`.
- Grade escreve `tipo_dia`, `cobertura_parcial`, `periodo_parcial` em `app/escala/utils.py:1220`, `:1230`, `:1234`; cartório transporta para ata em `app/ponto/services/cartorio.py:704` e `:732`; fábrica lê em `app/chamados/services/fabrica_da_celula.py:123`.
- `dia_sem_trabalho` lê `trabalha`, `ata.tipo_dia`, `ata.cobertura_parcial`; parcial só cala se `trabalha=False`. (`app/chamados/juizes.py:435`)

Limites: o anexo não fornece `ata.tipo_dia`, `ata.cobertura_parcial`, `ata.periodo_parcial`, registros de ausência ou execução histórica da fábrica. Atribuir o guarda efetivamente executado na emissão histórica é **[não verificado]**. O dado mostra instâncias com `lampada=True` e ações vazias; isso não demonstra marco apagado. **[não verificado: anexo fornecido sem linhas]**

## 11/11 01:19 · sessao 26d759465242 · FIO-E0-V4-E4B · construtor · origem=Code

Volta 3 escrita. [Entrega com P1–P6, censo e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:10).

- **51 testes OK; 31 mutantes mortos; quatro REDs falham na volta 1.** [Resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:126). [nao verificado]
- C20 falhou por Django ausente. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:141).
- Permanece a pendência herdada da fotografia de reversão da cascata. [Detalhes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:384).

Backfill real não executado; ORM, suite inteira e certificação: **[nao verificado]**. [nao verificado]

## 11/11 01:07 · sessao 37bca8857cf8 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**A c20 cura a passagem comum do agrupador, mas a cura na origem continua parcial:** declaração e pendentes alcançam os escritores de destino sem consultar o índice; o replay ainda rederiva episódio quando o valor conservado é `None`. (`app/chamados/services/disputa_emissao.py:1001`, `:300`, `:505`, `:791`, `:799`; `app/chamados/models.py:2866`.)

Nas referências abreviadas, `disputa_emissao.py`, `fio.py`, `descarte_disputa.py` e `declaracao_texto.py` ficam em `app/chamados/services/`; `models.py`, em `app/chamados/`; testes, em `app/chamados/tests/`; `auditar_invariantes_chamados.py`, em `app/chamados/management/commands/`. [nao verificado]

1. **Estado dos achados da preverif4.r.**

   | Achado anterior | Estado nesta cópia |
   |---|---|
   | Exceção do gerador deixar nascimento incompleto | **Curado no núcleo:** pai, DS e perguntas participam do atomic; exceção propagada desfaz o conjunto. `disputa_emissao.py:638`, `:668`, `:685`, `:689`. |
   | Geração vazia confirmar nascimento, inclusive pela chamada vazia da grade | **Curado no núcleo:** sem pergunta local admitida, desfaz o nascimento. `disputa_emissao.py:690`, `:696`, `:698`. |
   | Canônica estrangeira deixar nova DS sem pergunta | **Curado para esse mecanismo:** núcleo, declaração e agrupador desfazem o nascimento local; permanecem as ressalvas de replay e destino abaixo. `disputa_emissao.py:696`, `:725`, `:792`, `:795`, `:1487`. |
   | API pública de declaração aceitar ausência de pergunta | **Curado:** `pergunta` é obrigatório; o ensaio sem pergunta ficou privado. `disputa_emissao.py:738`, `:762`, `:766`. |
   | Seed `orfao_14h` indevidamente conservado no lote com seed estrangeiro | **Curado para motivo novo:** começa sem seed e só o introduz se o próprio seed não foi estrangeiro. Não remove cópias preexistentes. `disputa_emissao.py:1312`, `:1469`, `:1472`. |
   | Segundo agregador na âncora | **Curado:** âncora entrega `['ancora_ausente']` ao agregador; união/save ficaram nele. `disputa_emissao.py:575`, `:581`, `:1582`, `:1613`. |
   | Luz HX sem leitor | **Curado no instrumento:** I7a/I7b usam zero linhas de pergunta e distinguem ausência de ata de silêncio estrutural. Medida atual: **[nao verificado]**. `auditar_invariantes_chamados.py:62`, `:69`, `:73`. |
   | Dona fechada como pergunta de lei aberta | **Não identifico nova pergunta de lei; implementação parcial da trilha/alarme**, conforme item 2. O agregador não salva a fechada; reabertura do sistema continua em porta própria. `disputa_emissao.py:567`; `models.py:2163`. |
   | Replay rederivar episódio pelo contexto da dona | **Parcial:** episódio positivo é conservado; `None` ainda permite rederivação. `disputa_emissao.py:504`; `models.py:2866`. |
   | Agrupador analisar chamado após replay divergente | **Curado nesse ramo:** exige destino confirmado e não recusado. `disputa_emissao.py:1498`, `:1508`, `:1510`. |
   | Dois mutantes abortarem na montagem | **Curado:** montagem fora do runner; alvos precisam falhar por assertion, sem errors. `test_bo_disputa_volta5_memoria.py:389`, `:396`, `:415`. |
   | “Paridade histórica” comparar a fonte atual consigo mesma | **Curada a alegação:** agora se chama repetibilidade e declara o limite. Comparação histórica continua **[nao verificado]**. `test_bo_disputa_volta3_memoria.py:252`, `:254`. |
   | Pendentes fora do atomic do núcleo | **Igual:** wrapper chama depois do retorno; helper abre P e depois agrega DS, sem atomic próprio. `disputa_emissao.py:836`, `:850`, `:300`, `:314`. |
   | Novos motivos admitidos em dona estrangeira não chegarem ao push correspondente | **Igual no núcleo misto:** agregação estrangeira não coleta seus `novos`; wrapper usa os `novos` da local. `disputa_emissao.py:623`, `:662`, `:665`, `:851`. | [nao verificado]
   | Push imediato poder anteceder confirmação final | **Igual no agrupador:** complementos e principal notificam dentro do atomic externo, sem `on_commit`. `disputa_emissao.py:1238`, `:1268`, `:1516`, `:1537`. | [nao verificado]
   | Nem todos os retornos `(p, criada)` serem integralmente consumidos | **Igual literalmente:** primeira tentativa da declaração conserva P e descarta o booleano em `_`; confere a dona. `disputa_emissao.py:791`, `:792`. |
   | Doubles não certificarem cartório/signals/concorrência | **Igual como limite de prova.** O teste c20 declara essas fronteiras simuladas. `test_bo_disputa_volta5l_memoria.py:6`. | [nao verificado]
   | Protegidos historicamente intocados, efeitos externos, produção e suíte inteira | **[nao verificado]**. | [nao verificado]

   A ressalva do replay é concreta: o emissor conserva `batida_origem_id=None`, mas `_nascer` executa `valor or derivador(dona)`. Se a dona aponta uma batida 222, o replay ganha esse episódio; se ela pertence a outro turno, também pode mudar o dia efetivo. A guarda pode desfazer o replay divergente, mas isso não reproduz o fato original. (`disputa_emissao.py:504`, `:725`, `:726`; `models.py:2866`, `:2875`; `fio.py:1633`.)

   Reproduzi em memória a sequência `(pedido=None, efetivo=None)` → `(pedido=None, efetivo=222)`, terminando em `replay-divergente`, sem costura confirmada. O teste F2 existente cobre somente episódio positivo 111; ORM desse contraexemplo: **[nao verificado]**. (`test_bo_disputa_volta5f_memoria.py:115`, `:122`, `:130`; `disputa_emissao.py:729`.) [nao verificado]

2. **c15/c16 — trilha, contador e saídas sem trilha.**

   **Quem escreve:** o agregador encaminha a fechada ao registrador; este fotografa motivos/fechamento, elimina motivos já presentes e agenda a gravação depois do commit. A escrita persistente passa por `core.observ.evento`. (`disputa_emissao.py:567`; `descarte_disputa.py:9`, `:10`, `:31`, `:37`; `app/core/observ.py:140`.)

   A identidade deduplicada é **dona + motivo + pergunta**, inclusive pergunta `None`; origem é procedência. O callback trava C→D em transação própria. (`descarte_disputa.py:20`, `:23`, `:25`, `:27`.)

   **Quem lê:** I23 lê todas as linhas históricas daquela ação/modelo; o comando publica o resultado e está declarado no cron. Conta **linhas de descarte**, não donas distintas, motivos atualmente pendentes ou janela temporal. (`auditar_invariantes_chamados.py:81`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   Portanto, **contador e trilha têm o mesmo universo das linhas efetivamente gravadas**. Não cobrem necessariamente todos os descartes tentados ou confirmados no negócio. (`descarte_disputa.py:31`; `auditar_invariantes_chamados.py:81`.)

   Há estas saídas sem nova linha:

   - **Deliberadas:** motivo já presente; identidade já registrada; savepoint desfeito. (`descarte_disputa.py:10`, `:27`, `:35`.)
   - **Falha da auditoria:** callback ocorre depois do commit; `engolir` captura falhas, e `evento` também pode retornar `None`, ignorado pelo registrador. O descarte pode ficar confirmado sem trilha; I23 fica cego. Ocorrência em produção: **[nao verificado]**. (`descarte_disputa.py:19`, `:31`, `:37`; `app/core/observ.py:92`, `:111`, `:139`, `:145`.)
   - **Dedup antes do agregador:** suponha dona fechada com P local viva cujo motivo falta em `DS.motivos`, num chamado que não segue o caminho por ata. O helper soma o motivo em memória, mas retorna `False` pelo dedup; o complemento termina sem criação, o agrupador não chama o agregador/trilha e pode excluir o chamado e retornar. Reprodução ORM: **[nao verificado]**. (`disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1300`, `:1304`, `:1661`.)

   **Alarme parcial:** I23 positivo aparece como `VIOL`, mas **sozinho não provoca o `SystemExit(2)`**: essa decisão consulta somente F1/F2. O teste 5g exige o alarme F1/F2, além da linha I23; não prova alarme independente de I23. Consumidor externo específico: **[nao verificado]**. (`auditar_invariantes_chamados.py:95`, `:1942`; `test_bo_disputa_volta5g.py:101`, `:104`, `:107`.) [nao verificado]

3. **c19 — seed e caminhos sem pergunta.**

   Nova ou aberta vazia começa sem motivos; recebe `orfao_14h` ao final, salvo reuso estrangeiro **do próprio seed**. Estrangeira de outro motivo não impede o seed; motivos preexistentes são preservados. (`disputa_emissao.py:1312`, `:1313`, `:1469`; `test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

   | Caminho | Resultado |
   |---|---|
   | **HX-ATA-NAO-SABE**, abstenção por `sem_celula` | Marca abstenção; com possibilidade de fechamento, conserva aberta e acende luz. `disputa_emissao.py:1088`, `:1094`, `:1383`, `:1525`. |
   | **h2**, ata disponível e nada a perguntar | Nasce e fecha vazia; zero P permite o fechamento. `disputa_emissao.py:1524`, `:1530`; `models.py:1853`, `:1903`. |
   | Nova, somente perguntas estrangeiras | Desfaz nascimento local, repete na dona e retorna `None`. `disputa_emissao.py:1487`, `:1494`, `:1513`. |
   | Aberta preexistente sem P | Não recebe esse rollback, que exige `nova`; segue ao fechamento/HX. `disputa_emissao.py:1487`, `:1524`. |
   | Exceção propagada durante o nascimento | Desfaz o atomic; não confirma nascimento vazio. `disputa_emissao.py:1238`, `:1316`. |
   | **Recusa técnica sem testemunho de ata** | Falta de data/base, marcos inválidos ou falha de leitura pode terminar zero P e fechar. Não demonstra as premissas estritas de HX ou h2. `disputa_emissao.py:1054`, `:1083`, `:1362`, `:1418`, `:1445`, `:1448`, `:1524`. |

   **Existe caminho fora das duas premissas.** Contraexemplo: chamado `batida_ausente`, aberto, `contexto_json={}`, sem origem/célula/pergunta anterior. Regularizabilidade aceita pelo mapa; não segue ata; o laço sem origem abandona-o por falta de data. A DS recebe o seed e fecha vazia **sem consultar ata nem acender luz**. (`models.py:3667`; `disputa_emissao.py:925`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`.)

   Esse caminho foi reproduzido com funções produtivas via AST e infraestrutura em memória: regularizável, zero P, fechada, zero chamadas à ata/luz. Isso não certifica os efeitos ORM/signals: **[nao verificado]**. A infraestrutura de fechamento é explicitamente double. (`test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

4. **c20 — origem e equivalência de `(motivo, dia)`.**

   | Parte | Índice | Caminho B |
   |---|---|---|
   | Fonte | `ch.contexto_json['marcos_faltantes']`. `disputa_emissao.py:1252`. | A mesma lista do chamado sobrevivente. `disputa_emissao.py:1356`. |
   | Motivo | `tipo == 'S'` → saída; qualquer outro → volta. `:1259`. | Mesma expressão. `:1359`. |
   | Dia | `date.fromisoformat(marco.get('data'))`. `:1256`. | Mesma expressão, depois envolvida por `dia_do_turno_da_celula`. `:1361`, `:1367`. |
   | Exceções | `TypeError`, `ValueError`. `:1257`. | `Exception`. `:1362`. |
   | Universo | Regularizáveis antes dos complementos. `:1250`. | Sobreviventes do filtro posterior aos complementos. `:1291`, `:1297`, `:1351`. |

   **Para JSON ordinário, mesmo marco e estado estável, motivo/dia concordam.** `DiaDoTurno` é `NewType`; o construtor usado por B não converte a data em execução. Tipo ausente/desconhecido vira “volta” em ambos. (`disputa_emissao.py:1259`, `:1359`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2939`, `:2943`.)

   **Não há equivalência universal dos dois procedimentos:** diferem no tratamento de exceções e, principalmente, no universo e momento da leitura. Divergência de exceção com dado JSON persistido ordinário: **[nao verificado]**. (`disputa_emissao.py:1257`, `:1291`, `:1297`, `:1362`.)

   **Índice com chave e B sem execução correspondente:** intervalo regularizável já possui P não materializada e não segue `cobra_marcos_da_ata`. Entra no índice, pode ser complementado e depois é excluído pelo filtro; B não o percorre. O índice continua disponível e pode direcionar o complemento do chamado genérico para esse intervalo. (`disputa_emissao.py:1250`, `:1280`, `:1291`, `:1300`, `:1351`, `:1001`.)

   **B com chave ausente no índice**, mantendo estado e contexto estáveis entre as leituras: **[nao verificado]**. A implementação não fixa o resultado do predicado entre as passagens. (`disputa_emissao.py:1250`, `:1299`.)

   Havendo chave, a passagem comum substitui o destino antes de abrir e conserva-o no replay estrangeiro; sem chave, mantém o destino recebido, sem recusa adicional. Se dois intervalos oferecem a mesma chave, `setdefault` escolhe o primeiro do lote, inclusive para pedidos posteriores do segundo. (`disputa_emissao.py:1001`, `:1018`, `:1029`, `:1260`, `:1497`.)

5. **Censo de escritores de `PerguntaDisputa.chamado_destino`.**

   | Escritor persistente | Local e alcance |
   |---|---|
   | Nascimento | Construtor/inserção em `models.py:3014`, `:3017`. |
   | Reuso da canônica | Julga e atribui destino, conservando a dona da P. `models.py:2972`, `:2980`, `:2982`, `:3006`, `:3008`. |
   | `reapontar_resolvedora` | Julga, atribui e salva. `fio.py:907`, `:909`, `:925`, `:927`. |
   | Deleção da FK | Apagar o chamado produz `NULL`; não escolhe outro chamado. `models.py:3385`, `:3387`. |

   As atribuições de `fio.py:779`/`:783` são temporárias e restauradas; `models.py:366`/`:367` reassocia instância do mesmo PK. O próprio contrato registra os dois escritores produtivos e a FK. (`fio.py:807`, `:814`.)

   **Pedidos pela passagem comum:** complemento anterior ao filtro, B, ata, grupo por origem e caminho sem origem passam pelo override c20. (`disputa_emissao.py:1291`, `:1366`, `:1387`, `:1425`, `:1431`, `:1457`, `:1462`, `:1001`.)

   **Pedidos fora dela:**

   - Pendentes chamam o gerador diretamente; ele deriva destino, abre P e pode chamar a reata. O índice não chega a esse gerador. (`disputa_emissao.py:1486`, `:300`, `:412`, `:503`, `:505`, `:519`; `app/chamados/reconciliador.py:1642`, `:1655`.)
   - Declaração chama `abrir` e, condicionalmente, `reapontar_resolvedora`, sem consultar o índice. (`disputa_emissao.py:784`, `:786`, `:791`, `:799`.)

   **Sim, permanece pedido capaz de costurar no genérico uma pergunta do intervalo explícito:** P de `intervalo_saida` em B, já destinada ao chamado `intervalo`; declaração pelo chamado genérico `ch`, do mesmo colaborador/dia, casa o marco de intervalo da ata e passa `ch` como destino. A candidata estrangeira é recusada pelo helper local; `abrir` reutiliza a canônica e pode trocar seu destino para `ch`, inclusive no replay confirmado. (`declaracao_texto.py:250`, `:254`, `:341`, `:346`, `:365`; `disputa_emissao.py:791`, `:799`; `models.py:2982`, `:3006`.)

   O administrador faz o mesmo pedido. Os juízes conferem colaborador, turno e estado do fio; não elegem preferência pelo chamado com marco explícito de intervalo. (`declaracao_texto.py:650`, `:653`; `app/chamados/juizes.py:2185`, `:2208`, `:2217`; `fio.py:771`.)

   Reproduzido em memória: mesma P/dona, destino `intervalo` → destino `ch`. A porta de reuso é double; execução ORM: **[nao verificado]**. (`test_bo_disputa_volta5c_memoria.py:101`, `:104`; `disputa_emissao.py:799`.)

   Os contraexemplos pedidos **da passagem comum** passaram em memória: só `ch`, só intervalo, ambas as ordens, dona aberta/fechada e replay 2x; também outro dia e intervalo-volta. Isso não certifica o protegido ORM. (`test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:112`, `:119`, `:126`, `:136`; `test_bo_disputa_volta5b_orfaos.py:157`, `:159`.)

6. **Quem escreve, quem lê e significado dos campos.**

   | Campo | Escritores alcançáveis | Leitores que dão significado ao delta |
   |---|---|---|
   | `DS.motivos` | Agregador/nascimentos: `disputa_emissao.py:575`, `:685`, `:1337`, `:1605`; aparo/remoção/reabertura: `models.py:1966`, `:2063`, `:2159`. | União/admissão/seed/push: `disputa_emissao.py:571`, `:606`, `:1312`, `:1469`, `:1535`; auditor: `auditar_invariantes_chamados.py:50`; foto: `app/api/foto_recebida.py:61`. |
   | `DS.fechada_em` | Fechar/reabrir admin/sistema: `models.py:1932`, `:2149`, `:2173`. | Recusa/eleição/push: `disputa_emissao.py:567`, `:621`, `:645`, `:846`, `:1267`, `:1536`; receivers: `app/chamados/signals.py:28`, `:129`, `:153`; validação: `app/chamados/services/validacao.py:214`. |
   | `DS.contexto` | Agregador/nascimentos: `disputa_emissao.py:578`, `:685`, `:1337`, `:1605`; aparo/remoção: `models.py:1967`, `:2064`. | Resolução e episódio: `disputa_emissao.py:346`, `:373`; `models.py:3463`; auditor: `auditar_invariantes_chamados.py:31`. |
   | `P.disputa` | Inserção: `models.py:3014`, `:3017`; reuso preserva-a: `:3008`; deleção da DS usa CASCADE: `:2416`. | Encaminhamento: `disputa_emissao.py:613`, `:1022`; candidata: `declaracao_texto.py:254`; autorização/listagem: `fio.py:116`, `:598`. |
   | `P.chamado_destino` | Os quatro casos do item 5. `models.py:2982`, `:3014`, `:3387`; `fio.py:925`. | Complemento/filtro: `disputa_emissao.py:1280`, `:1301`; juiz: `app/chamados/juizes.py:2150`; autorização: `fio.py:598`; materialização: `app/chamados/services/materializacao.py:1076`. |
   | `P.motivo` | Inserção: `models.py:3014`, `:3017`. | Encaminhamento: `disputa_emissao.py:614`; identidade: `fio.py:1793`; candidata: `declaracao_texto.py:250`; materialização: `app/chamados/services/materializacao.py:1077`. |
   | `P.batida_origem_id` | Argumento conservado: `disputa_emissao.py:504`; nascimento/derivação: `models.py:2866`, `:2868`, `:3449`. | Identidade e dia: `models.py:2907`; `fio.py:1633`. |
   | `P.data_evento` | Porta de atribuição: `fio.py:790`, `:800`; nascimento passa por ela: `models.py:3016`. | Dedup/chave: `disputa_emissao.py:402`, `:1001`, `:1661`; juiz: `fio.py:1608`. |
   | Trilha nova | Registrador → `evento`: `descarte_disputa.py:31`; inserção: `app/core/observ.py:140`. | Dedup: `descarte_disputa.py:27`; I23: `auditar_invariantes_chamados.py:81`; apresentação: `:1925`. |

   **Semântica:** `DS.motivos` é lista de cenários; pode conter seed sem P daquele motivo. `P.motivo` participa da identidade de um fato datado/instância. Não são uma relação um a um nem uma chave global equivalente. (`models.py:1743`, `:2427`, `:3483`; `disputa_emissao.py:1469`.)

   **Dia civil versus turno:** conversa tem discriminador explícito e dia civil; pergunta datada usa o dia do fato/turno. Pendentes ignoram a data civil contextual e derivam pela batida. O problema residual é a origem rederivável no replay, não uma conversão feita por `DiaDoTurno`. (`models.py:2801`, `:2811`, `:2875`; `app/chamados/juizes.py:2188`; `disputa_emissao.py:275`, `:284`; `app/ponto/turnos.py:2943`.)

   O índice novo compara igualdade; não introduz intervalo inclusivo/exclusivo. Nos atos legados, o grupo por origem ainda classifica pela data civil antes de resolver o dia do turno. Equivalência dessas decisões para todo turno noturno: **[nao verificado]**. (`disputa_emissao.py:1001`, `:1411`, `:1412`, `:1417`.)

   Os receivers de fechamento leem o valor de `fechada_em`, sem conferir transição ou `update_fields`; salvar uma fechada não é automaticamente um no-op. O novo agregador evita esse save. (`app/chamados/signals.py:28`, `:129`, `:153`; `disputa_emissao.py:567`.)

7. **Classes de reprovação e mordida dos selos.**

   | Classe | Estado no delta/atos |
   |---|---|
   | Segundo juiz duplicando autoridade registrada | **Nenhum novo identificado no delta** para fechamento, regularizabilidade ou costura: continuam delegados. `disputa_emissao.py:966`, `:1524`; `models.py:2972`; `fio.py:907`. |
   | Segundo escritor fora da porta | **Agregação da âncora curada; nenhum novo escritor persistente de destino encontrado fora dos dois registrados/FK.** Isso não cura os pedidos divergentes que chegam a eles. `disputa_emissao.py:1613`; `fio.py:807`, `:814`; `models.py:2982`. |
   | Relógio fora da borda | **Nenhuma leitura nova identificada em c15–c20; presente nos atos legados:** contexto, seleção de passado e SLA. `disputa_emissao.py:554`, `:914`, `:679`, `:1331`. |
   | Derivação paralela de fonte única | **Presente na c20:** índice e B repetem motivo/dia, com filtros e capturas diferentes. `disputa_emissao.py:1250`, `:1256`, `:1259`, `:1359`, `:1361`. |
   | Selo/RED sem mordida | **Curado nos dois mutantes antigos e ausente nos quatro c20 exe

[truncado em 20000 de 22665 caracteres]

## 11/11 01:06 · sessao bd452f9f99a5 · FIO-E0-V4-E3 · leitura · origem=Code

**V=3, T=229, X=0; soma=232.** As causas se distribuem em **228 d, 2 c e 2 e**. (`sonda_232.out:5–236`; classificação fundamentada abaixo.)

- **V:** existe ação exigível pela lei — decisão do admin ou exposição de pergunta viva — que a tarefa omite. (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`)
- **T:** nenhum marco ou decisão residual demonstrado exige ação; o vigia chama de morto um dia sem tarefa exigível. (`app/docs/LEIS.md:95`; `app/chamados/juizes.py:426–441`; `app/docs/CORTES.md:406`)
- **X:** caso que não cabe nesses critérios; **nenhum neste recorte**. (`sonda_232.out:5–236`, aplicando as regras acima.)

As referências `sonda_232.out:N` numeram **o anexo colado**, contando `99 objects...` como linha 1, a linha vazia como 2 e o primeiro `DIA` como 5. Não representam leitura de um arquivo local.

A tabela principal é esta; **todas as datas são de 2026**. (`sonda_232.out:5–236`)

| Grupo | Classe / quantidade | Colaborador/dia | Fundamentação |
|---|---:|---|---|
| Respondida viva, sem validação, âncora terminal | **V / 2** | col252/09-29; col841/10-10 | Há decisão residual do admin. A causa **c** vem depois de excluir resposta vazia; a tarefa para em `dia_sem_trabalho` antes de examinar a pergunta. (`sonda_232.out:154,224`; `app/chamados/services/vigia_fio.py:673–676`; `app/chamados/services/tarefa_do_dia.py:241–243`; `app/docs/RELATO.md:2336`) |
| Pergunta grave viva em disputa fechada | **V / 1** | col237/11-07 | A lei exige apresentação ao admin pela vida da pergunta, independentemente do fechamento da disputa. O motivo grave fica fora do mapa de marcos da tarefa. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/docs/RELATO.md:1736`) |
| Sem instâncias e sem pergunta viva | **T / 201** | Lista completa na tabela seguinte | Pelo fluxo executado: dia sem trabalho, nenhuma instância elegível, nenhuma ação residual demonstrada. (`sonda_232.out:5–236`, dias discriminados abaixo; `app/chamados/services/tarefa_do_dia.py:199–210,235–243`) |
| Instâncias presentes, todas encerradas por `dia_sem_trabalho` | **T / 16** | col97/11-09; col177/11-08; col292/10-22,10-28; col348/10-24,10-31,11-07,11-08; col354/10-30; col741/11-01,11-05,11-07,11-09; col866/11-06,11-08; col889/10-25 | Não há pergunta viva nesses dias; as instâncias não geram cobrança. (`sonda_232.out:9,20,42–43,54–57,59,86–92`; `app/chamados/juizes.py:426–441`) |
| Instâncias presentes, todas `aceso`, sem pergunta viva | **T / 11** | col354/10-21; col655/09-30,10-21; col922/11-01; col996/10-26; col205/09-21; col258/09-28,10-19; col259/10-14,10-15; col368/09-21 | Os marcos estão cumpridos; não há resposta viva residual demonstrada. (`sonda_232.out:58,85,93,113,138,155–158,189,223`; `app/docs/LEIS.md:95`; `app/docs/CORTES.md:406`) |
| Pergunta não respondida viva, mas todos os marcos acesos | **T / 1** | col962/10-25 | A pergunta `intervalo_saida` não cria falta de um marco já cumprido. Aplica-se a regra específica dos marcos acesos; não existe aqui resposta pendente da exceção acima. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375`) |

Para expandir os **201 T sem instâncias**, use os conjuntos abaixo; cada colaborador indicado corresponde a **todas** as datas da respectiva célula. (`sonda_232.out`, referências por linha na tabela.)

- **A:** 10-21, 10-23, 10-25, 10-27, 10-29, 10-31, 11-02, 11-04, 11-06, 11-08. (`sonda_232.out:10–19`)
- **B:** 09-21, 09-23, 09-25, 09-27, 09-29, 10-01, 10-03, 10-05, 10-07, 10-09, 10-11, 10-13, 10-15, 10-17, 10-19. (`sonda_232.out:123–137`)

| Colaborador(es) | Datas | Quantidade | Fonte |
|---|---|---:|---|
| col144, col209, col268, col312, col427, col969 | A | 60 | `sonda_232.out:10–19,21–30,32–41,44–53,60–69,103–112` |
| col144, col209, col268, col312 | B | 60 | `sonda_232.out:123–137,139–153,159–188` |
| col427 | B, exceto 09-21 | 14 | `sonda_232.out:190–203` |
| col650 | 10-22,10-24,10-26,10-28,10-30,11-01,11-03,11-05,11-07,11-09 | 10 | `sonda_232.out:75–84` |
| col650 | 09-22,09-24,09-26,09-28,09-30,10-02,10-04,10-06,10-08,10-10,10-12,10-14,10-16,10-18,10-20 | 15 | `sonda_232.out:208–222` |
| col969 | 09-26,09-27,10-03,10-04,10-10,10-11,10-12,10-17,10-19 | 9 | `sonda_232.out:228–236` |
| col97 | 11-01,11-03,11-05,11-07 | 4 | `sonda_232.out:5–8` |
| col574 | 10-24,10-31,11-07 | 3 | `sonda_232.out:70–72` |
| col574 | 09-26,10-03,10-10,10-17 | 4 | `sonda_232.out:204–207` |
| col639 | 11-06,11-08 | 2 | `sonda_232.out:73–74` |
| col950 | 10-24,10-28,10-29,10-31,11-01,11-02,11-07,11-08 | 8 | `sonda_232.out:94–101` |
| col950 | 10-12,10-17,10-18 | 3 | `sonda_232.out:225–227` |
| col1005 | 11-02 | 1 | `sonda_232.out:114` |
| col1009, col1018 | 11-08 | 2 | `sonda_232.out:115–116` |
| col1019 | 11-04,11-06,11-08 | 3 | `sonda_232.out:117–119` |
| col1027 | 11-05,11-07,11-09 | 3 | `sonda_232.out:120–122` |
| **Total** | | **201** | Soma das linhas acima |

**1. Entrada no universo e causas do vigia.** Todos os 232 percorrem o mesmo caminho: empresas visíveis → competências de `competencias_a_lavrar` → `dias_que_seguram` → inserção de `(colaborador, data)` em `dias`. `dias_que_seguram` seleciona colaboradores classificados fora por `furo_espelho` e consulta os dias retidos por `motivos_retencao_celula`. O fato exigido para entrar é **retenção da folha**, não pergunta viva ou marco faltante. (`app/chamados/services/vigia_fio.py:581–587`; `app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)

Como todos têm célula, há dois ramos possíveis de retenção: **veredito acusador**, sem passar pelas dispensas de retenção; ou **veredito nulo**, `trabalha` preenchido e dia passado fora da borda de demissão. Qual desses dois introduziu **cada dia** é **[nao verificado]**: a sonda não imprime veredito, protestos nem `trabalha`. (`sonda_232.out:5–236`; `app/folha/export.py:1233–1256`)

Depois, o vigia consulta a porta; separa hoje/futuro; consulta os quatro baldes de responsabilidade já atribuída; somente então incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635–660,695–699`)

| Causa | Condição, na ordem efetiva | Neste recorte |
|---|---|---:|
| **a** | Chamado vivo da célula ou âncora viva fora da fila. (`app/chamados/services/vigia_fio.py:663–670`) | 0 |
| **b** | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:673–674`) | 0 |
| **c** | Pergunta viva respondida, âncora terminal; resposta vazia já foi excluída. (`app/chamados/services/vigia_fio.py:673–676`) | 2 |
| **d** | Sem pergunta viva; dia fora da lista CxR; colaborador presente nela. Célula existente e ausência de chamado vivo decorrem dos testes anteriores. (`app/chamados/services/vigia_fio.py:667–678`) | 228 |
| **e** | Restante, com detalhe nomeado. Aqui: pergunta viva sob chamado terminal. (`app/chamados/services/vigia_fio.py:679–694`) | 2 |

As quantidades dessa tabela vêm das linhas do anexo. (`sonda_232.out:5–236`)

**A causa d não afirma que o dia exige ação, nem atribui dono CADASTRO.** Afirma presença do **colaborador** na CxR e ausência daquele **dia** na lista. A lista vem de `ListaDoAdmin → cadastro_realidade.lista → dias_na_lista`; os colaboradores são extraídos separadamente das linhas por posto. Somente assinaturas que nomeiam uma `data` põem o dia na lista. (`app/chamados/services/vigia_fio.py:205–218,230–251,677–678`; `app/escala/services/cadastro_realidade.py:1044–1064`)

**2. Caminho da tarefa.** A premissa `instancias=[]` vale para **202**, não para todos os 232: são os 201 T da tabela mais col237; os outros 30 têm instâncias com ações vazias. Todos têm `fio='nada'`, fila vazia, `nao_sei=False` e razão geral vazia. (`sonda_232.out:5–236`)

Nos 202 vazios, o fluxo permite inferir `sem_trabalho=True`: caso contrário, ausência de instâncias produziria `nao_venceu` ou `ata_sem_lampadas`. As instâncias são extraídas das células da grade, exigindo tipo E/S e hora; ausência de pergunta não é o critério que decide sua existência. (`app/chamados/services/tarefa_do_dia.py:62–67,84–103,199–210,390–403`)

Nos outros 30, a tarefa encerra cada instância em `dia_sem_trabalho` (**18**) ou `aceso` (**12**), antes de examinar pergunta e decisão. Sem ações, `_portas_da_tarefa` produz `fio='nada'`, fila e verbos vazios; fila residual não cria ação. (`sonda_232.out:9,20,42–43,54–59,85–93,102,113,138,154–158,189,223–224`; `app/chamados/services/tarefa_do_dia.py:241–246,268–283,418–434`)

O fato consultado para dispensar cobrança é `dia_sem_trabalho`, alimentado pela célula/ata; para cumprir marco é a lâmpada lavrada. `marcos_pendentes=[]` apenas informa ausência de marca `pendente` na ata: sozinho não demonstra ausência de obrigação. (`app/ponto/supra_juiz.py:65–71`; `app/chamados/juizes.py:426–441`; `app/ponto/services/lastro.py:138–150`)

**3. Por que os três V e o T de col962 diferem.** Em col252 e col841, a pergunta respondida continua viva. A regra preserva a decisão do admin quando a resposta não foi absolvida pela concordância com a luz e pelo dia coerente; `dia_sem_trabalho` não extingue automaticamente essa decisão residual. A tarefa a ignora porque termina antes da pergunta. (`sonda_232.out:154,224`; `app/docs/RELATO.md:2336`; `app/chamados/juizes.py:1341–1353`; `app/chamados/services/tarefa_do_dia.py:241–243`)

Em col237, disputa fechada e âncora resolvida não substituem resolução da pergunta viva. A lei manda expô-la ao admin; a tarefa restringe-se aos motivos de marco e não alcança `batida_fora_escala_grave`. O conteúdo da decisão que o admin tomará é **[nao verificado]**. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/chamados/services/declaracao_texto.py:42–47`)

Em col962, a pergunta não foi respondida e os quatro marcos estão acesos. A regra específica dispensa cobrança com todos os marcos cumpridos. O C6 pode manter vida pelo julgamento geral da célula, pois seu encerramento direto pelo marco aceso exige também `materializada_em`; isso não transforma o marco cumprido em falta. O ramo individual que manteve esse C6 vivo é **[nao verificado]**. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375,1388–1400`)

**4. As quatro hipóteses anteriores não explicam este recorte.** Não há rota só-fila, porta A/B ou verbos A/B observados: todas as portas são nulas e todos os chamados/âncoras impressos estão terminais. Nenhum retorno passou pelo ramo de isenção. Portanto, nenhuma das quatro hipóteses se aplica como causa demonstrada destes 232 resultados. (`sonda_232.out:5–236`; `app/chamados/juizes.py:1578–1585`; `app/chamados/services/tarefa_do_dia.py:183–189`)

A LEI-C17-UM-JUIZ escolhe a tarefa nos quatro casos anteriormente medidos; ela não declara corretas todas as saídas atuais da tarefa. O aval desta fase exige justamente decidir as divergências pela regra. (`app/docs/RELATO.md:244`; `aval_e3_troca_pelo_diff_1111.txt:4–5`)

**5. Contraexemplo por ISENÇÃO: não aparece nestes 232.** Nenhum retorno tem razão geral `isento_ponto`, obrigatória naquele ramo. Os dois casos em que `dia_sem_trabalho` esconde decisão residual são col252 e col841, classificados V; **dia sem trabalho não equivale a isenção**. Existência desse contraexemplo fora do recorte: **[nao verificado]**. (`sonda_232.out:5–236`; `app/chamados/services/tarefa_do_dia.py:183–189,202,241–243`)

**6. Não falta regra escrita para estas classificações.** Há regra para a lâmpada, para resposta residual e para pergunta viva em disputa fechada. Falta **dado**, para todos os 232, para individualizar o ramo de retenção da folha; isso não é pergunta de lei. (`app/docs/LEIS.md:95`; `app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316,406`; `app/folha/export.py:1233–1256`; `sonda_232.out:5–236`)

Como **V não é vazio**, a condição “classe só T” não se verifica: pela ordem do aval, a cura na tarefa precede a troca dos leitores. (`aval_e3_troca_pelo_diff_1111.txt:5`)

## 11/11 01:04 · sessao 6585a794233b · - · - · origem=Ronald (tmux)

**Itens 2 e 3 — conclusões para consolidação.**

1. **Trilha e contador têm o mesmo universo somente depois de a gravação da auditoria ter sido bem-sucedida.** O agregador encaminha dona fechada para `registrar_descarte_em_dona_fechada`; o registrador elimina motivos já presentes, agenda `on_commit`, trava C→D e deduplica por `(dona,motivo,pergunta_id)` antes de chamar o escritor único `core.observ.evento`. Origem não participa da identidade. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:10`, `:23`, `:27`, `:31`, `:37`; `app/core/observ.py:100`, `:140`.)

   I23 lê **todas as linhas históricas** com ação/modelo correspondentes, sem filtro de estado atual, dia ou janela. Conta linhas, não donas distintas: duas perguntas/motivos da mesma dona podem produzir dois elementos iguais na amostra. O formatador publica `VIOL` se positivo, conserva a descrição integral e integra o comando executado pelo cron. (`app/chamados/management/commands/auditar_invariantes_chamados.py:77`, `:81`, `:89`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   I23 positivo, isoladamente, **não provoca o `SystemExit(2)` deste comando**, que verifica somente F1/F2. Isso prova a diferença entre sinal visual e código de saída; alarme externo que leia a linha I23: **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1942`.)

2. **Há saídas sem uma nova linha de trilha.**

   - Motivo já presente na dona: saída deliberada antes do callback; repetição da mesma identidade: dedup deliberado. Não são novos descartes registrados. (`app/chamados/services/descarte_disputa.py:10`, `:27`.)
   - Replay divergente: savepoint desfeito e nenhum novo encaminhamento confirmado; seus callbacks são cancelados. (`app/chamados/services/disputa_emissao.py:1496`, `:1501`; `app/chamados/services/descarte_disputa.py:35`.)
   - **Falha da própria trilha:** depois de o negócio confirmar, o callback pode falhar na trava/consulta; `engolir` captura. `evento` também captura sua falha de inserção e retorna `None`; o registrador ignora o retorno. Nesse caso, o descarte confirmado fica sem linha, e I23 não consegue contá-lo. O restante observável é ERROR e contador de engolidos do processo. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/descarte_disputa.py:19`, `:23`, `:31`, `:37`; `app/core/observ.py:79`, `:92`, `:111`, `:139`, `:145`.)
   - **Silêncio por dedup anterior ao agregador:** se o complemento anterior ao filtro encontra dona fechada com pergunta local viva cujo motivo falta em `DS.motivos`, o helper acrescenta o motivo em memória, mas retorna `False` pelo dedup. O complemento termina `criou=False`; o agrupador só agrega/registra quando `_cc=True`, depois exclui o chamado já coberto e pode retornar `None`. Não há incorporação nem trilha desse motivo faltante. Pressupõe esse acervo inconsistente e chamado que não segue `cobra_marcos_da_ata`; exemplo possível: categoria `intervalo_ausente`, subcategoria mapeada `batida_orfa`, módulo diferente de `batida_ausente`. Reprodução ORM: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1297`, `:1304`, `:1661`; `app/chamados/models.py:3678`; `app/chamados/juizes.py:1324`, `:1399`.)

3. **A pergunta de lei “dona FECHADA” está encerrada pelo aval fornecido; a implementação preserva a decisão e instrumenta o descarte.** O agregador não salva/reabre a dona fechada. O registro usa snapshot do fechamento encontrado e continua histórico mesmo que outra cascata reabra a dona depois. Persistência dessas cascatas nesta árvore: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:9`, `:22`; aval DUAS-LEIS fornecido, sem arquivo local: **[nao verificado]**.)

4. **Seed c19:** começa dos motivos existentes; somente nova/aberta vazia recebe `orfao_14h` ao final, salvo se o próprio motivo aparece em alguma estrangeira do ato. Estrangeira de outro motivo não impede o seed. O seed também pode permanecer como motivo sem pergunta específica quando há outra pergunta local. Isso é o comportamento explicitamente cobrado pelos selos K3, não exclusividade “um motivo = uma pergunta”. (`app/chamados/services/disputa_emissao.py:1312`, `:1467`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

5. **Sem pergunta no agrupador:**

   | Caminho | Resultado sustentado |
   |---|---|
   | HX: `_ata_do_dia` recusa por `sem_celula` com escala vigente | `_absteve_por_ata=True`; conserva aberta e acende luz. (`app/chamados/services/disputa_emissao.py:1094`, `:1383`, `:1525`.) |
   | h2: ata disponível e nenhum fato a perguntar | Zero perguntas permite `pode_fechar`; fecha como `vazia`. (`app/chamados/services/disputa_emissao.py:1524`; `app/chamados/models.py:1848`, `:1903`.) |
   | Só estrangeiras, local nova sem P | Desfaz nascimento local e repete perguntas na dona; retorna `None`. (`app/chamados/services/disputa_emissao.py:1487`, `:1494`, `:1513`.) |
   | Local preexistente sem P | Não recebe a mesma guarda de rollback, que exige `nova`; passa ao auto-fecho/HX. (`app/chamados/services/disputa_emissao.py:1487`, `:1524`.) |
   | **Sem data/base, marcos inválidos ou erro de leitura** | Existem recusas técnicas sem testemunho de ata; podem terminar zero P e fechar, portanto não demonstram as premissas estritas HX ou h2. (`app/chamados/services/disputa_emissao.py:1054`, `:1083`, `:1356`, `:1362`, `:1413`, `:1418`, `:1445`, `:1448`, `:1451`, `:1524`.) |

6. **Contraexemplo concreto fora das premissas das duas leis:** chamado categoria `batida_ausente`, status `aberto`, `contexto_json={}`, sem célula, sem pergunta materializada nem disputa anterior. O juiz real de regularizabilidade permite-o pelo mapa; `cobra_marcos_da_ata=False`; `dia_do_fato=None`; o laço sem origem abandona-o por falta de data. A DS nasce, recebe seed e fecha vazia **sem consultar ata e sem luz**. Isso não prova “COM ATA e nada a perguntar”, exigido no aval h2. (`app/chamados/services/disputa_emissao.py:923`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`; `app/chamados/models.py:3667`; `app/chamados/juizes.py:87`, `:133`, `:138`.)

   Executei o agrupador e os juízes indicados via AST em memória, com mapas reais e infraestrutura double: `regularizavel=True`, `cobra_ata=False`, `dia=None`, DS com `['orfao_14h']`, zero P, fechada, zero consultas de ata/luz. ORM/signals/banco: **[nao verificado]**. A infraestrutura usada explicita doubles de trava, origem e fechamento. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

7. **Verificação executada:** 5g memória passou 10 testes e K3 passou 2 testes; resultados não certificam Django/PostgreSQL. A trilha double substitui `engolir` por `yield` e `evento` por append infalível, portanto esses verdes não cobrem a falha de auditoria descrita acima. (`app/chamados/tests/test_bo_disputa_volta5g_memoria.py:122`, `:240`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:98`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:297`, `:302`.)

## 11/11 01:03 · sessao edfffd5339dd · - · - · origem=Ronald (tmux)

Minha leitura favorece **V nos dias col252/2026-09-29, col841/2026-10-10 e col237/2026-11-07; T para col962/2026-10-25**. Os demais dias sem pergunta viva, sem ações e sem marco pendente favorecem T; não medi a contagem total do anexo. [nao verificado]

- **col252 e col841: V.** A regra escrita preserva a resposta divergente ou sem hora comparável para decisão do admin; marco aceso sozinho não extingue essa pendência. Isso está explícito em `app/docs/RELATO.md:2336`, implementado em `app/chamados/juizes.py:1341`. A tarefa pula toda análise da pergunta quando `sem_trabalho`, em `app/chamados/services/tarefa_do_dia.py:241`. A sonda demonstra exatamente perguntas respondidas, não validadas, C6 vivas, embora a tarefa diga `dia_sem_trabalho`. [nao verificado — dado do prompt]

- **col237: V.** Pergunta viva em disputa fechada deve aparecer ao admin, sem reabrir e sem push; o contador só deve cair por resolução do admin (`app/docs/CORTES.md:316`; reiterado em `app/docs/RELATO.md:2223`). `batida_fora_escala_grave` é motivo fora do chamado de marco do dia, com porta própria (`app/docs/RELATO.md:1736`). A tarefa limita perguntas aos motivos de marco (`app/chamados/services/tarefa_do_dia.py:225`), explicando a cegueira. A pergunta viva fechada e o retorno vazio constam da sonda. [nao verificado — dado do prompt]

- **col962: T.** A sonda mostra quatro lâmpadas `True`, pergunta `intervalo_saida` não respondida e nenhum marco pendente. [nao verificado — dado do prompt] A regra escrita manda deixar de contar cobrança viva quando todos os marcos estão acesos neste julgamento (`app/docs/CORTES.md:406`). O C6 ainda pode devolver verdadeiro pelo veredito acusador geral (`app/chamados/juizes.py:1372`), pois a precedência do marco preenchido exige `materializada_em` (`app/chamados/juizes.py:1368`). Isso explica o conflito de juízes sem criar falta de marco já cumprido.

**Cuidado com uma falsa equivalência no teste:** `app/chamados/tests/test_e2_tarefa_do_dia.py:814` e `:868` dizem que lâmpada acesa responde antes da pergunta, inclusive respondida. Esse comentário genérico não revoga a regra explícita das respostas divergentes/sem comparação (`app/docs/RELATO.md:2336`). Logo não usaria o teste para classificar col252/841 como T.

**C17 não decide antecipadamente este recorte inteiro:** a decisão documentada abrange os quatro mecanismos nomeados e manda tarefa responder rota/fila/verbos; não diz que toda divergência futura é T (`app/docs/RELATO.md:244`). A própria tarefa declara C6 fora de seus insumos (`app/chamados/services/tarefa_do_dia.py:29`).

**Isenção:** o ramo de isento deixa razão global `isento_ponto` e retorna antes de julgar marcos (`app/chamados/services/tarefa_do_dia.py:183`). As linhas fornecidas têm razão global vazia; portanto não sustentam erro pelo ramo de isenção. [nao verificado — dado do prompt] `dia_sem_trabalho` não significa isenção: é outro predicado e outro ramo (`app/chamados/services/tarefa_do_dia.py:202`; `:241`).

Não encontrei necessidade de nova lei para esses quatro dias: há regras escritas para resposta residual, disputa fechada e marco cumprido (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`; `:406`). A projeção exata dessas regras sobre os demais dias depende da consolidação do anexo. [nao verificado]

## 11/11 01:03 · sessao 27bfee3f46b7 · - · - · origem=Ronald (tmux)

- `instancias=[]`, `nao_sei=False`, `razao=''` não vêm da isenção: esse ramo sempre grava `isento_ponto`; tampouco vêm de tranca, fora de vínculo, célula ausente ou ata de trabalho sem lâmpadas, que deixam razões próprias. Pelo fluxo, correspondem a `sem_trabalho=True` e nenhuma instância extraída da grade. A grade vem do leitor único, e instâncias exigem lâmpadas com tipo E/S e hora. (`app/chamados/services/tarefa_do_dia.py:62`, `:84`, `:94`, `:180`, `:183`, `:190`, `:199`, `:202`, `:235`, `:375`.)

- Com instâncias presentes, `dia_sem_trabalho` encerra cada linha antes da pergunta; `aceso` encerra antes de examinar decisões. Por isso há linhas existentes com ações vazias, além dos dias com lista de instâncias vazia. (`app/chamados/services/tarefa_do_dia.py:235`, `:241`, `:244`.)

- Sem ações, a etapa final produz `fio='nada'`, nenhuma porta, fila vazia e verbos vazios. Fila residual não cria ação. (`app/chamados/services/tarefa_do_dia.py:418`, `:425`, `:426`, `:429`, `:430`, `:431`, `:433`.)

- A tarefa carrega apenas perguntas `conversa=False`; depois aceita somente os quatro motivos do mapa de marcos. `batida_fora_escala_grave` está fora desse mapa. Decisões só são consultadas depois das guardas de dia sem trabalho, lâmpada, vencimento e cobertura. (`app/chamados/services/tarefa_do_dia.py:225`, `:241`, `:244`, `:258`, `:281`, `:323`; `app/chamados/services/declaracao_texto.py:42`.)

- Para `col252/2026-09-29` e `col841/2026-10-10`, a causa `c` comprova pergunta C6 viva, respondida e sob âncora terminal; resposta vazia teria sido causa `b` antes. A regra escrita preserva a decisão do admin quando resposta e batida discordam ou o dia não está coerente. São candidatos sólidos a **V**: a tarefa termina em `dia_sem_trabalho`, sem chegar à resposta pendente de decisão. (`app/chamados/services/vigia_fio.py:673`, `:675`; `app/chamados/juizes.py:1341`, `:1349`, `:1350`; `app/docs/PROMPTS.md:1442`.)

- Para `col962/2026-10-25`, pergunta viva sem resposta e marco aceso não demonstram falta daquele marco: a regra BUG117 diz “fato vence pergunta”. O C6 só aplica esse encerramento direto quando há `materializada_em`, enquanto a tarefa consulta a lâmpada antes. A classificação **T** por marco aceso tem suporte; eventual exigência diferente do marco precisa ser demonstrada pelo dado. (`app/chamados/juizes.py:868`, `:880`, `:1364`, `:1368`; `app/chamados/services/tarefa_do_dia.py:244`.)

- Para `col237/2026-11-07`, a tarefa omite o motivo grave por desenho do seu mapa. Há regra escrita exigindo porta para perguntas que continuem vivas pelo C6, incluindo “todo motivo do dia sem porta”; disputa fechada não resolve por si a pergunta. A natureza exata da ação exigível desse motivo não aparece no recorte: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:225`; `app/chamados/services/declaracao_texto.py:42`; `app/docs/PROMPTS.md:1652`; `app/docs/CORTES.md:316`.)

- Nenhum resultado fornecido tem `razao='isento_ponto'`; portanto nenhum desses resultados percorreu a guarda de isenção. Não há contraexemplo comprovado de erro **por isenção** neste recorte. (`app/chamados/services/tarefa_do_dia.py:183`, `:184`, `:188`, `:189`.)

- LEI-C17-UM-JUIZ decide a favor da tarefa nos quatro campos divergentes anteriores e exige DIFF antes da troca; ela não declara que toda saída atual da tarefa já satisfaz todas as regras. O contrato do juiz é responder ações ainda exigíveis após decisões e efeitos. (`app/docs/RELATO.md:244`; `app/docs/PROMPTS.md:2007`; `app/docs/CORTES.md:426`.)

## 11/11 01:02 · sessao d87f9eac0e8c · - · - · origem=Ronald (tmux)

O caminho do vigia é único para todos os dias: `censo_alcance → dias_que_seguram → classificar_export + motivos_retencao_celula(com_dias=True) → dias.setdefault((colab, data), competência)`. O vigia exige retenção da folha, e não existência de pergunta ou chamado vivo. (`app/chamados/services/vigia_fio.py:581-587`; `app/folha/fora_da_folha.py:118-137`; `app/folha/fora_da_folha.py:95-99`)

Com célula existente, há **dois ramos possíveis de entrada**: veredito acusador que não passa pelas isenções de retenção; ou célula sem veredito, `trabalha is not None`, passada e fora do dia de demissão. A sonda não expõe veredito nem `trabalha`, portanto o ramo individual entre esses dois permanece [nao verificado]. (`app/folha/export.py:1233-1256`)

Antes de contar morto, o vigia procura porta; separa hoje/futuro; e procura decisão pendente, ausência aberta, folga contestada validada ou dia na mesa do admin. Só depois escolhe causa e incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635-660`; `app/chamados/services/vigia_fio.py:304-315`; `app/chamados/services/vigia_fio.py:695-699`)

| Causa | Condição efetiva e referência |
|---|---|
| a | Chamado vivo da célula ou âncora viva fora da fila; como a porta já falhou, é vivo sem porta alcançável. (`app/chamados/services/vigia_fio.py:663-670`) |
| b | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:671-674`) |
| c | Pergunta viva respondida, com âncora terminal; vem depois de excluir resposta vazia. (`app/chamados/services/vigia_fio.py:673-676`) |
| d | Nenhuma pergunta viva, dia ausente da lista CxR, colaborador presente nela; existência de célula e ausência de chamado vivo decorrem dos ramos anteriores. (`app/chamados/services/vigia_fio.py:667-678`) |
| e | Restante, discriminado por detalhe: sem célula, dia na lista com outro dono, sem pergunta viva, pergunta sob terminal, sem âncora, fora do app ou outra. (`app/chamados/services/vigia_fio.py:667-694`) |

A causa **d não atribui dono CADASTRO nem prova que nada é exigível**: seu rótulo efetivo é `colab_na_lista_cxr_dia_fora`, e o teste só combina presença do colaborador com ausência do dia. (`app/chamados/services/vigia_fio.py:538-539`; `app/chamados/services/vigia_fio.py:677-678`)

A “lista” vem de `ListaDoAdmin`, que chama `escala.services.cadastro_realidade.lista`, deriva os dias com `dias_na_lista` e separadamente extrai colaboradores das linhas por posto. Só assinaturas com `data` representam dias; assinatura do vínculo não prova presença daquele dia. (`app/chamados/services/vigia_fio.py:205-218`; `app/chamados/services/vigia_fio.py:230-251`; `app/escala/services/cadastro_realidade.py:1044-1064`)

Para a classificação, os dois dias de causa **c** merecem exame próprio: a lei C6 mantém resposta com texto viva para decisão do admin, salvo decisão explícita dele sobre o dia ou concordância com a luz nos termos do juiz; esse ramo precede a absolvição pelo veredito. Portanto `dia_sem_trabalho` sozinho não permite classificá-los T. (`app/chamados/juizes.py:1341-1353`; `app/chamados/juizes.py:1372-1387`)

A tarefa, entretanto, pula cada instância por `dia_sem_trabalho` ou `lampada is True` **antes** de procurar a pergunta e sua decisão. Isso explica como pode devolver nada apesar de C6 viva, sem demonstrar por si só qual autoridade está correta naquele caso. (`app/chamados/services/tarefa_do_dia.py:241-246`; `app/chamados/services/tarefa_do_dia.py:268-283`)

`marcos_pendentes=[]` informa apenas ausência de lâmpada marcada `pendente` na ata; não informa ausência de acusação, ausência de trabalho ou ausência de obrigação. (`app/ponto/services/lastro.py:138-150`)

## 11/11 01:02 · sessao 2dc0b65e065e · - · - · origem=Ronald (tmux)

1. **Há derivação paralela da mesma chave.** Índice: motivo por `tipo == 'S'` versus demais, dia por `date.fromisoformat(marco.get('data'))`; B repete essas duas expressões. Referências: `app/chamados/services/disputa_emissao.py:1256`, `:1259`, `:1359`, `:1361`.

   Para JSON ordinário e mesmo estado do chamado, as expressões produzem o mesmo motivo/dia. O construtor usado por B não altera o valor nem o tipo em execução: `DiaDoTurno` é `NewType` e `dia_do_turno_da_celula` apenas o retorna. Referências: `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2939`, `:2943`.

   **Não há equivalência literal para todo input/universo:** índice captura somente `TypeError/ValueError`, B captura `Exception`; índice lê os regularizáveis antes dos complementos, enquanto B recebe os sobreviventes do filtro posterior. Referências: `app/chamados/services/disputa_emissao.py:1250`, `:1257`, `:1291`, `:1294`, `:1297`, `:1300`, `:1351`, `:1362`.

   Caso concreto de índice com chave sem execução correspondente de B: intervalo regularizável já possui pergunta não materializada e `cobra_marcos_da_ata(intervalo)==False`. Ele entra no índice, pode ser complementado, e é retirado do lote por `:1300`; B não o percorre. O índice continua disponível ao complemento do genérico e redireciona seu pedido ao intervalo. Referências: `app/chamados/services/disputa_emissao.py:1250`, `:1280`, `:1291`, `:1300`, `:1351`, `:1001`.

   Caso B com chave ausente do índice sem mudanças de estado entre essas leituras: **[nao verificado]**. Mudanças produzidas por complementos/signals ou predicado temporal podem mudar a elegibilidade; reprodução ORM: **[nao verificado]**.

2. **Censo persistente de destino confirmado.**

   - Nascimento: `app/chamados/models.py:3014`, `:3017`.
   - Reuso da canônica: `app/chamados/models.py:2968`, `:2972`, `:2982`, `:3006`.
   - Reaponte: `app/chamados/services/fio.py:907`, `:909`, `:925`, `:927`.
   - Exclusão do chamado: FK `SET_NULL`, `app/chamados/models.py:3385`, `:3387`.
   - Atribuições temporárias/restauradas: `app/chamados/services/fio.py:779`, `:783`; reassociação de mesmo PK: `app/chamados/models.py:366`, `:367`.

   O próprio contrato documenta os dois escritores produtivos e a deleção de FK: `app/chamados/services/fio.py:804`, `:807`, `:814`.

3. **A c20 protege a passagem comum, não todos os pedidos que chegam aos escritores.** O override acontece em `_criar_pergunta_se_couber`; o replay conserva esse destino. Referências: `app/chamados/services/disputa_emissao.py:1001`, `:1018`, `:1029`, `:1497`.

   Pendentes chamam o gerador diretamente, sem lhe passar o índice; o gerador deriva outro destino e chama `abrir`/eventual reata. Referências: `app/chamados/services/disputa_emissao.py:1486`, `:300`, `:412`, `:503`, `:505`, `:514`, `:519`. A reata usa a outra porta escritora. Referências: `app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/fio.py:925`.

4. **Contraexemplo sobrevivente na declaração, alcançável pelos atos tocados.** A declaração do chamado genérico pode casar o marco de intervalo da ata, e passa sempre `ch` como destino. Se a canônica desse intervalo está em outra DS, `_candidata_reusavel` devolve `None`; `abrir` reaproveita essa pergunta e costura em `ch`, inclusive no replay confirmado. Referências: `app/chamados/services/declaracao_texto.py:341`, `:346`, `:362`, `:365`, `:250`, `:254`; `app/chamados/services/disputa_emissao.py:791`, `:799`; `app/chamados/models.py:2982`, `:3006`.

   Admin possui o mesmo pedido de destino. Referências: `app/chamados/services/declaracao_texto.py:618`, `:650`, `:653`.

   Reprodução executada somente em memória com `Ambiente5l`: P1 em dona B2, destino intervalo2 → declaração pelo ch1 → mesma P1/B2 com destino ch1. O ambiente usa a porta double que explicitamente grava a costura; portanto prova o pedido errado conservado pelo emissor, sem certificar ORM/signals. Referências do mecanismo executado: `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`, `:104`; `app/chamados/services/disputa_emissao.py:791`, `:799`. Reprodução ORM: **[nao verificado]**.

   Os juízes dos escritores conferem colaborador/dia/estado do fio, sem preferir o chamado com marco explícito de intervalo. Assim, chamado genérico vivo do mesmo dia satisfaz essas guardas. Referências: `app/chamados/juizes.py:2185`, `:2208`, `:2217`; `app/chamados/services/fio.py:771`; `app/chamados/models.py:2972`, `:2980`.

5. **Contraexemplos solicitados da passagem comum estão cobertos e passaram em memória.** Ambas as ordens, dona aberta/fechada, replay 2x, só genérico, só intervalo, outro dia e intervalo-volta: `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:112`, `:119`, `:126`, `:136`. ORM e suíte completa: **[nao verificado]**.

6. **Executei os quatro mutantes c20, sem arquivos nem banco:** todos os controles passaram 5/5; `destino_generico` e `sem_indice` morreram com cinco failures, `indice_sem_dia` e `troca_marco` com um failure, sem errors. As assertions mordem os destinos em `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:91`, `:132`, `:142`; montagem fora do runner em `:157`, `:159` e rejeição de errors em `:162`.

## 11/11 01:02 · sessao 3d3d25efdc7a · - · - · origem=Ronald (tmux)

Censo dos campos e classes, com endereços da cópia:

| Campo | Quem escreve | Quem lê nos caminhos relevantes |
|---|---|---|
| `DS.motivos` | Agregador `app/chamados/services/disputa_emissao.py:575`, save `:581`; nascimentos `:685`, `:1337`, `:1605`; aparo `app/chamados/models.py:1966`; remoção `:2063`; reabertura admin `:2159`. | União/admissão `app/chamados/services/disputa_emissao.py:571`, `:606`; seed `:1312`, `:1469`; push `:1535`; foto `app/api/foto_recebida.py:61`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:50`. |
| `DS.fechada_em` | Fechamento `app/chamados/models.py:1932`, save `:1936`; reabertura admin `:2149`, save `:2161`; sistema `:2173`, save `:2177`. | Eleição/recusa/push `app/chamados/services/disputa_emissao.py:567`, `:621`, `:645`, `:846`, `:1267`, `:1536`; receivers `app/chamados/signals.py:28`, `:129`, `:153`; validação `app/chamados/services/validacao.py:214`; autorização `app/chamados/services/fio.py:600`. |
| `P.disputa` | Construção e inserção `app/chamados/models.py:3014`, `:3017`; reuso devolve a linha preservando a dona `:3008`. Exclusão da DS apaga perguntas por CASCADE `:2416`. | Encaminhamento `app/chamados/services/disputa_emissao.py:613`; candidata `app/chamados/services/declaracao_texto.py:254`; listagem/autorização `app/chamados/services/fio.py:116`, `:598`; identidade `app/chamados/models.py:3430`. |
| `P.chamado_destino` | Nascimento `app/chamados/models.py:3014`; reuso `:2982`, save `:3006`; reaponte `app/chamados/services/fio.py:925`, save `:927`; exclusão de chamado produz NULL pela FK `app/chamados/models.py:3385`. | Índice c20 `app/chamados/services/disputa_emissao.py:1001`; complemento/filtro `:1280`, `:1301`; juiz `app/chamados/juizes.py:2150`; listagem/autorização `app/chamados/services/fio.py:117`, `:598`; materialização `app/chamados/services/materializacao.py:1076`. |
| `P.motivo` | Construção/inserção `app/chamados/models.py:3014`, `:3017`; a derivação de instância lê o motivo `:3437`. | Encaminhamento `app/chamados/services/disputa_emissao.py:614`; universo do fato `app/chamados/services/fio.py:1793`; motivo para marco `app/chamados/services/declaracao_texto.py:250`; materialização `app/chamados/services/materializacao.py:1077`. |
| Trilha nova | Porta `app/chamados/services/descarte_disputa.py:4`, gravação por `evento` `:31`, agendada `:37`. | Dedup da própria porta `app/chamados/services/descarte_disputa.py:27`; contador I23 `app/chamados/management/commands/auditar_invariantes_chamados.py:81`, ligado no comando `:893`. |

O mapa acima cobre os escritores identificados e leitores relevantes aos atos auditados; censo exaustivo de todos os leitores indiretos de toda a aplicação: **[nao verificado]**.

O significado de `DS.motivos` é “cenários ativos”, enquanto `P.motivo` participa da identidade do fato juntamente com dia e instância. O seed pode aparecer sem pergunta daquele motivo; logo, as listas não têm equivalência um a um. `app/chamados/models.py:1743`, `:2427`, `:3483`; `app/chamados/services/disputa_emissao.py:1469`.

O dia civil e o dia do turno estão distinguidos pelo campo `conversa`: conversa nasce com `localdate`, e o juiz não a compara ao turno do chamado; datadas passam por `dia_do_turno_da_linha`. Pendentes derivam o dia pela batida e ignoram a data civil contextual. `app/chamados/models.py:2801`, `:2811`, `:2875`; `app/chamados/juizes.py:2188`; `app/chamados/services/disputa_emissao.py:275`. Nova divergência inclusivo/exclusivo nesses campos: **[nao verificado]**; a seleção c20 usa igualdade de chave em `app/chamados/services/disputa_emissao.py:1001`.

Classes:

- **Segundo escritor da agregação na âncora: curado.** A âncora passa `['ancora_ausente']` ao agregador; a união e o save ficam no helper. `app/chamados/services/disputa_emissao.py:1582`, `:1613`, `:575`.
- **Derivação paralela: presente na c20.** Índice e caminho B derivam separadamente motivo/dia de `marcos_faltantes`; a análise de equivalência cabe ao explorador c20. `app/chamados/services/disputa_emissao.py:1252`, `:1256`, `:1259`, `:1356`, `:1359`, `:1361`.
- **Relógio novo no delta c15–c20: não identificado.** Leituras internas legadas continuam nos atos: contexto, seleção de passado e SLA. `app/chamados/services/disputa_emissao.py:554`, `:914`, `:679`, `:1331`, `:1599`. Ausência universal de relógio fora da borda: **[nao verificado]**.
- **Trava da trilha: C→D em transação própria, depois do commit.** `app/chamados/services/descarte_disputa.py:20`, `:23`, `:37`.
- **Trava do repasse: universo reunido e revalidado antes das escritas.** `app/chamados/services/disputa_emissao.py:1188`, `:1196`, `:1198`.
- **Pendentes ainda fora da fronteira do núcleo no wrapper.** O núcleo retorna antes da chamada; o helper abre perguntas e depois agrega DS, sem atomic próprio. `app/chamados/services/disputa_emissao.py:836`, `:850`, `:300`, `:314`.
- **Replay ainda parcial para episódio ausente.** O dict fixa `batida_origem_id`, mas `_nascer` usa `valor or derivador`; `None` permite derivar outro episódio da dona. O teste F2 cobre episódio positivo, não ausência. `app/chamados/services/disputa_emissao.py:504`; `app/chamados/models.py:2866`, `:3463`; `app/chamados/tests/test_bo_disputa_volta5f_memoria.py:110`.

## 11/11 01:00 · sessao 96abcaf59a57 · FIO-E0-V4-E4B · construtor · origem=Code

**PAREI em P4, conforme sua instrução.** Não localizei autoridade única para “tem resposta” incluindo texto sem timestamp: `respondida` lê só o carimbo; a recuperação pela trilha reconhece esse meio-estado como resposta existente ([models.py:3497](chamados/models.py:3497), [disputa_emissao.py:1661](chamados/services/disputa_emissao.py:1661)).

Registrei evidências, pendências P1–P6 e comandos em [E4B_ALCANCE_ENTREGA.txt:1](E4B_ALCANCE_ENTREGA.txt:1). O RED P4 falhou por assertion; os 21 testes preexistentes passaram. A cura permanece pendente, sem certificação. [nao verificado]

## 11/11 00:59 · sessao 9151e4f862dc · BRIEF-COM-PROCEDENCIA · censo · origem=Code

As citações abaixo vêm dos anexos transcritos no pedido. A numeração física de linhas desses anexos é **[nao verificado]**; indico arquivo, seção e trecho literal. Presença de citação e autorização da decisão são contagens distintas.

| Par | Erro | ORIGINAL | REPLAY | Fonte ou omissão? |
|---|---|---|---|---|
| 5a | E1 | **Contém:** “so sai se NESTE ato nasceu (`criada`) pelo menos uma pergunta em DS ABERTA” — `orig_5a.md`, D5. | **Repete a classe “critério próprio sem aval”**, embora mude o filtro: “minha decisão de desenho é” e “Não anunciar [...] sem pergunta respondível” — `replay_5a.r`, §6. | Cita código como motivação; a procedência confirma “condicionado a pergunta respondível: **decisões minhas**”. Aval desse critério: **[nao verificado]**. |
| 5a | E2 | **Contém:** “No REPLAY nenhuma pergunta nasce (so reuso), entao a cadeia e inalcancavel la” — TRIAGEM. | **Evitou:** “O gerador não escreve somente perguntas” e “Planejamento não pode chamar esse gerador” — replay, §2. | **Por fonte de código**, com cadeia e efeitos explicitados; não omite o tema. Referências citadas no replay: `disputa_emissao.py:472–477`, `reconciliador.py:405–412`; correspondência histórica: **[nao verificado]**. |
| 5a | E3 | **Fora deste brief:** o episódio indicado é de 21:2x. Há uma formulação anterior: “DECIDIDO PELA LEI [...] + cura mais restritiva; contestavel” — decisões. | Não impõe imutabilidade genérica: “Preserve as portas [...] e prove as cascatas do save, inclusive callbacks” — contrato, canônica fechada. | Evita a proibição genérica e cita código para examinar efeitos. A atribuição específica à corte das 21:2x: **[nao verificado]**. |
| 5a | E4 | **Fora deste brief:** órfãos estão “FORA DESTA VOLTA”. | **O tema específico do seed não aparece.** | Não conto ausência do tema como correção fundamentada — `orig_5a.md`, exclusões; `replay_5a.r`; linhas **[nao verificado]**. |
| 5a | E5 | **Fora deste brief;** ver registro de 5h abaixo. | Não atribui identificador novo ao contador. | `replay_5a.r`; conferência dos IDs existentes **[nao verificado]**. |
| 5f | E1 | **Fora do episódio das 19:27.** Contém outra decisão: “notificar de novo seria push duplicado do mesmo fato” — decisões. | **Evitou o filtro original:** “Não substituir essa política por ‘perguntas criadas nesta chamada’” — D6. | **Por fonte citada:** `opus5d.txt`, item 3, e endereços do código. Conteúdo integral do Opus e aval das decisões técnicas adicionais: **[nao verificado]**. | [nao verificado]
| 5f | E2 | **Fora deste brief.** | A conclusão específica “inalcançável” não reaparece. | Tema específico ausente — `orig_5f.md`; `replay_5f.r`; linhas **[nao verificado]**. |
| 5f | E3 | **Fora do episódio das 21:2x.** O texto posterior diz: “a dona FECHADA segue pergunta de lei aberta: nao mexer” — decisões. | **Evitou decidir:** “É permitido agregar o motivo mantendo-a fechada, ou qual saída deve valer?” — pergunta de lei. | **Por suspensão expressa da decisão**, acompanhada de referências; não apresenta uma solução como autorizada. Linhas dos anexos **[nao verificado]**. |
| 5f | E4 | **Contém a formulação ampla:** “Inclui o seed 'orfao_14h': a local so recebe motivo que tem pergunta LOCAL neste ato ou que ja tinha antes” — F1. | **Restringe a formulação:** “Eliminar o seed indevido [...] **no repasse com ata**”; D5 preserva as saídas sem ata e com ata completa. | **Por delimitação e fontes de código/testes**, não por omissão. O aval apenas chama o erro de “seed [...] largo”; o efeito concreto condenado não está detalhado: **[nao verificado]**. | [nao verificado]
| 5f | E5 | **Fora deste brief;** ver 5h abaixo. | Não atribui identificador novo ao contador. | `replay_5f.r`; conferência dos IDs existentes **[nao verificado]**. |
| 5g | E1 | **Fora do episódio das 19:27.** | Rejeita o filtro original: “Não troque tudo por perguntas criadas/localmente retornadas”. Porém acrescenta: “Minha decisão é aplicar o critério de agregação do wrapper à dona resultante” — D6. | Cita fonte para preservar a política; o acréscimo é declarado próprio. Aval desse acréscimo: **[nao verificado]**. |
| 5g | E2 | **Fora deste brief.** | O tema específico da triagem “inalcançável” não aparece. | `orig_5g.md`; `replay_5g.r`; linhas **[nao verificado]**. |
| 5g | E3 | **Fora do episódio das 21:2x.** Agora diz “dona fechada continua sem save”, com cabeçalho remetendo ao aval das duas leis. | Mantém o comportamento e identifica “Fonte decisória: `aval_duas_leis_1110.txt`, item 2” — D3. | **Evita a atribuição à corte anterior por citar outro aval.** O conteúdo integral e o alcance desse aval posterior: **[nao verificado]**. |
| 5g | E4 | **Fora do episódio das 22:1x;** não trata do seed. | O tema aparece: “nenhum seed indevido”, mas também “Cobrir também ausência/recusa sem admissão de motivo” — F1; D3 preserva HX/h2. | Há fontes e exceções explícitas; afirmar que isso eliminou exatamente o excesso chamado “largo” é **[nao verificado]**. |
| 5g | E5 | **Contém a ordem de alocação:** “G2. [...] **I8** 'motivo em dona FECHADA descartado'”. | Pede contador chamado `motivo_em_dona_fechada`, sem escolher novo identificador numérico. | Não cria o novo `I8`; execução de uma conferência de IDs: **[nao verificado]**. |

Sobre **E5**, o `orig_5h.md` **registra a repetição e manda corrigi-la**: “registra o contador novo como `I8`, mas `I8` ja e o fio solto” e “Renomear o NOVO para `I23`” — item 2b, linha **[nao verificado]**. A cópia confirma um `I8` de fio solto em [auditar_invariantes_chamados.py:860](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/management/commands/auditar_invariantes_chamados.py:860). **Nenhum dos três replays atribui um novo ID numérico sem conferência demonstrada**; a própria conferência não está demonstrada nos replays **[nao verificado]**.

Para a contagem, usei **unidade decisória**, pois contar quebras gráficas daria denominadores diferentes: cada parágrafo normativo, item normativo ou linha normativa de tabela conta uma unidade; continuações ficam juntas. Incluí ordens no CENSO, itens-pai com exigência própria e subitens; excluí títulos, metadados, descrições factuais, comandos shell e os resumos/tabelas finais de procedência dos replays. A numeração física dessas unidades nos anexos é **[nao verificado]**.

“Com fonte” significa citação de aval/lei identificada ou arquivo e linha **na unidade**, incluindo abreviações `:N` resolvidas pelo CENSO. Não significa que a fonte autoriza a decisão — caso do push no replay 5a, explicitamente declarado próprio (`replay_5a.r`, §6; linha **[nao verificado]**). [nao verificado]

| Par | Lado | Unidades decisórias | Com citação | Sem citação |
|---|---|---:|---:|---:|
| 5a | Original | 34 | 14 | 20 |
| 5a | Replay | 54 | 54 | 0 |
| 5f | Original | 12 | 6 | 6 |
| 5f | Replay | 46 | 45 | 1 |
| 5g | Original | 7 | 1 | 6 |
| 5g | Replay | 50 | 46 | 4 |
| **Total** | **Originais** | **53** | **21** | **32** |
| **Total** | **Replays** | **150** | **145** | **5** |

Contagens derivadas dos seis anexos transcritos, conforme o critério acima; localização física arquivo:linha **[nao verificado]**.

As cinco unidades dos replays sem fonte direta são: “Mutantes obrigatórios, aplicados à implementação produtiva” no 5f; e, no 5g, “Registre na RAIZ”, “Confira os oito casos”, “Execute os controles” e “O alvo ORM F1 [...] deve ser executado”. Elas remetem a relatos, referências internas ou aval não identificado naquela unidade (`replay_5f.r`, D7; `replay_5g.r`, D3/D7; linhas **[nao verificado]**). [nao verificado]

## 11/11 00:57 · sessao ec1fa2c5c443 · - · - · origem=Ronald (tmux)

Pelo critério integral combinado: **orig_5f = 13 unidades, 6 com fonte / 7 sem; replay_5f = 46 unidades, 45 com / 1 sem**. Unidades são parágrafos, itens e linhas de tabela normativos; não são linhas físicas. Fontes abreviadas com `:n` resolvidas pelo CENSO contam. Resumos e procedência final do replay ficam fora para evitar duplicação.

Inventário de **orig_5f**, com S/N para fonte direta:

| Nº | Âncora literal | Fonte |
|---:|---|:---:|
| 1 | “escopo = avais anexos” | N |
| 2 | “descartado (vai ao opus): push para novos motivos na dona estrangeira (:613)” | S | [nao verificado]
| 3 | “descartado (vai ao opus): perguntar_entradas_pendentes fora do atomic do nucleo (:830)” | S |
| 4 | “a dona FECHADA segue pergunta de lei aberta: nao mexer” | N |
| 5 | F1: “o motivo cuja pergunta foi REUSADA de outra disputa NAO entra na local” | S, `:1270-1460` contextual |
| 6 | F2: “o coletor de reuso conserva o EPISODIO” | N |
| 7 | F3: “com replay divergente, o chamado cuja costura foi desfeita NAO e analisado” | N |
| 8 | F4: “Ancora (:1564): os motivos da DS existente passam pelo _agregar_motivos_na_dona” | S |
| 9 | F5: “a forma publica exige pergunta” | S, `:764` contextual |
| 10 | F6: “nasce_sem_pergunta e replay_insere ancoram em sitio UNICO” | N |
| 11 | F7: “apague a afirmacao falsa (nome e asserts)” | N |
| 12 | F8: “PROIBIDO” | S, referências dos protegidos |
| 13 | “Feche com: arquivos tocados, por item o RED” | N |

A primeira unidade é limítrofe: contém o contrato “escopo = avais anexos”. Se a tratar como fato introdutório, excluir resulta em **12 = 6 S / 6 N**.

Inventário de **replay_5f**:

| Nº | Seção e âncora literal | Fonte |
|---:|---|:---:|
| 1 | D1: “NÃO LIBERA push nem pouso desta obra” | S | [nao verificado]
| 2 | D1: “Preserve as exceções expressamente decididas para HX” | S |
| 3 | D1: “Não tocar nas sete disputas existentes” | S |
| 4 | D2: “A próxima volta precisa eliminar as divergências” | S |
| 5 | D2: “Exigir um contrato comum de nascimento” | S |
| 6 | D2: “Fechar a API pública de declaração” | S |
| 7 | D3: “Adotar a direção pedida no aval da matriz” | S |
| 8 | D3: “começar o encaixe no serviço” | S |
| 9 | D3: “Conservar a identidade completa do fato” | S |
| 10 | D3: “Preservar a ordem global C → D → Ch → P” | S |
| 11 | D4: “Eliminar o seed indevido de `orfao_14h`” | S |
| 12 | D4: “Não converter isso em exclusividade global” | S |
| 13 | D4: “deixar o cartório ressuscitar e reabrir a dona” | S |
| 14 | D4: “Preservar os asserts atuais de reabertura e nota” | S |
| 15 | D5, estrangeiras: “A nova local não nasce” | S |
| 16 | D5, sem ata: “Continua nascendo aberta” | S |
| 17 | D5, ata completa: “preservar nascimento seguido de fechamento vazio” | S |
| 18 | D5: “Manter I7a como DS aberta sem nenhuma linha de pergunta” | S |
| 19 | D6: “Não substituir essa política” | S |
| 20 | D6: “O resultado da agregação precisa conservar, por dona” | S |
| 21 | D6: “Preservar declaração sem push” | S | [nao verificado]
| 22 | D6: “retirar o efeito externo de push de dentro da transação” | S | [nao verificado]
| 23 | D7: “Entregar a matriz completa” | S |
| 24 | D7, a: “Pai/DS/P persistem juntos” | S |
| 25 | D7, b: “Sem nascimento local vazio” | S |
| 26 | D7, c: “Separar o caso com reabertura pelo cartório” | S |
| 27 | D7, d: “Resolver cada fato e dona sem reentrada” | S |
| 28 | D7, e: “Batida anterior preservada” | S |
| 29 | D7, f: “protegida acusa I7a=0 e I7b=1” | S |
| 30 | D7, g: “Não restaurar o helper vazio removido” | S |
| 31 | D7, h: “Cobrir as três saídas de D5” | S |
| 32 | D7, i: “Distinguir fatos locais legítimos” | S |
| 33 | D7, j: “Destinatária, motivos, `nova`, repetição” | S |
| 34 | D7: “Acrescentar provas dos dois consumidores de declaração” | S |
| 35 | D7: “Mutantes obrigatórios, aplicados à implementação produtiva” | **N** | [nao verificado]
| 36 | D7, mutante: “Desligar a proteção transacional” | S |
| 37 | D7, mutante: “Trocar o episódio BATIDA pelo contexto da dona” | S |
| 38 | D7, mutante: “Desligar a trava real do repasse” | S |
| 39 | D7: “Controle verde seguido de falha comportamental por assertion é obrigatório” | S | [nao verificado]
| 40 | D7: “Não chamar de paridade histórica” | S |
| 41 | D8: “Não afrouxar S127” | S |
| 42 | D8: “Preservar a fixture já corrigida” | S |
| 43 | D8: “Pronto exige: matriz completa” | S |
| 44 | D8: “Executar as famílias memória pertinentes” | S |
| 45 | CENSO: “Não restaurá-lo para satisfazer uma matriz histórica” | S |
| 46 | Após pergunta de lei: “Não inventar reabertura”; “Registrar a linha”; “manter a liberação bloqueada” | S |

A unidade 35 inclui a condição normativa de aplicar mutantes à implementação produtiva, separada dos três filhos. Se for excluída como simples título, resulta em **45 = 45 S / 0 N**.

A pergunta residual em si — “É permitido agregar o motivo mantendo-a fechada, ou qual saída deve valer?” — não fixa uma decisão e não entra; seu parágrafo seguinte entra como unidade 46. Não produzi numeração física dos anexos: **[nao verificado]**.

## 11/11 00:57 · sessao e8000ca1facf · - · - · origem=Ronald (tmux)

Censo integral solicitado:

| Brief | Unidades decisórias | Com citação | Sem citação |
|---|---:|---:|---:|
| `orig_5a.md` | 34 | 14 | 20 |
| `replay_5a.r` | 54 | 54 | 0 |

Unidade: parágrafo normativo, item de lista ou linha de tabela normativa; continuação gráfica permanece na unidade. Em D3 contei abertura, (i) e (ii); em D7, abertura e (a)–(h). Incluí ordens explícitas dentro do CENSO. Excluí títulos, descrições puramente factuais, comandos shell e resumos/tabela final de procedência duplicados. Os anexos são transcrições do pedido, sem correspondência local de linhas conferida [nao verificado].

`orig_5a.md`, unidades com citação: lei inicial; justificativa da volta 5; D2; D3(ii); D6; abertura D7; TRIAGEM; quatro escritores do CENSO que ordenam SAI/NAO tocar/so LER; leitor da canônica “so chamar”; primeiras duas decisões finais.

`orig_5a.md`, unidades sem citação: D1; abertura D3; D3(i); D4; D5; testes (a)–(h); FORA DESTA VOLTA; PARE E RELATE; terceira decisão final; NAO tocar final; Rodar; PERGUNTA; ENTREGA. [nao verificado]

Todas as 54 unidades decisórias de `replay_5a.r` possuem referência direta de código ou aval nomeado; enumeração completa enviada na mensagem anterior. Isso mede **presença de citação**, não autorização: o critério de push é expressamente “minha decisão de desenho”, embora cite código (`replay_5a.r`, §6; anexo transcrito). Assim, S54/N0 não sustenta que o replay evitou E1. [nao verificado]

## 11/11 00:57 · sessao 313a7f40ade9 · - · - · origem=Ronald (tmux)

Pelo critério solicitado, **orig_5g = 7 unidades decisórias: 1 com fonte, 6 sem; replay_5g = 50: 46 com fonte, 4 sem**. Referências abaixo são às seções dos anexos transcritos; numeração física de arquivo:linha desses anexos [nao verificado].

Fonte significa citação na própria unidade de aval/lei nomeado ou `arquivo:linha`, inclusive endereço abreviado resolvível. Citar não significa validar a fonte externa [nao verificado].

| # | Original 5g — âncora literal | Fonte |
|---:|---|:---:|
| 1 | “FollowupChamado nao tem campo tipo [...] a trilha e LogAuditoria” | N |
| 2 | “comportamento NAO muda: dona fechada continua sem save” | N |
| 3 | G1 “grave UMA linha por core.observ.evento” | N |
| 4 | G2 “Contador no vigia [...] I8” | N |
| 5 | G3 “Casos que mordem” | N |
| 6 | G4 “PROIBIDO” — inclui endereços dos testes protegidos | S | [nao verificado]
| 7 | “Feche com: arquivos tocados” | N |

Inventário integral do replay:

| # | Replay 5g — âncora/seção | Fonte |
|---:|---|:---:|
| 1 | D0 “Primeiro confira o patch da 14” | S |
| 2 | D1 “Não volte à fase de ‘somente censo’” | S |
| 3 | “Não altere as sete disputas existentes” | S |
| 4 | D2.1 “pai, disputa e pergunta pertencem ao mesmo ato” | S |
| 5 | D2.2 “a FK da pergunta continua apontando” | S |
| 6 | D2.3 “congele episódio, dia do turno” | S |
| 7 | D2.4 “retorno com criada=True [...] não autoriza análise” | S |
| 8 | D2.5 “Agregador único de admissão” | S |
| 9 | D2.6 “exija pergunta no contrato público” | S |
| 10 | D3 “Sem ata [...] Com ata [...] h2” | S |
| 11 | “Registre na RAIZ-DA-DISPUTA F1 a alternativa” | **N** |
| 12 | “Dona fechada: mantenha o comportamento” | S |
| 13 | “Preserve a reabertura que já ocorre” | S |
| 14 | D4 “Preserve I7a [...] I7b” | S |
| 15 | “declare unidade de contagem, escritor da trilha” | S |
| 16 | D5 “Inclua pendentes na unidade transacional” | S |
| 17 | “Complete a avaliação exigida de resolver antes” | S |
| 18 | “A matriz deve concluir essa avaliação” | S |
| 19 | D6 “Mantenha os critérios históricos” | S |
| 20 | “O ato deve conservar os deltas por dona” | S |
| 21 | “Garanta que efeitos de um ato desfeito não notifiquem” | S |
| 22 | D7 “Confira os oito casos novos relatados pela 14” | **N** |
| 23 | Tabela F1 “nenhum seed indevido” | S |
| 24 | F2 “conservar identidade e dia” | S |
| 25 | F3 “nenhuma análise decorrente do fato recusado” | S |
| 26 | F4 “Mutante restaurando união/save próprios” | S |
| 27 | F5 “Chamada pública sem pergunta falha” | S |
| 28 | F6 “Mutação montada uma vez” | S |
| 29 | F7 “Paridade histórica exige fonte histórica distinta” | S |
| 30 | “Execute os controles e os mutantes relatados abaixo” | **N** |
| 31 | “Acrescente provas que removam a trilha” | S |
| 32 | “Exija ORM para o lote misto F1” | S |
| 33 | “O alvo ORM F1 [...] deve ser executado” | **N** |
| 34 | “Preserve os protegidos HX” | S |
| 35 | D8 “Entregue patch revisável da 14” | S |
| 36 | “Não libera pouso nem push agora” | S | [nao verificado]
| 37 | “O ! de PUSH-E4A2 [...] não é autorização” | S | [nao verificado]
| 38 | Após CENSO: “o comentário antigo [...] não deve ser tratado como estado atual” | S |
| 39 | “A matriz final deve preencher as seis colunas” | S |
| 40 | Matriz a “Nascer atomicamente se houver pergunta” | S |
| 41 | Matriz b “Agregar na dona” | S |
| 42 | Matriz c “Preservar comportamento; trilhar descarte” | S |
| 43 | Matriz d “Roteamento sem reentrada” | S |
| 44 | Matriz e “Desfazer emissão” | S |
| 45 | Matriz f “Abster-se e acender luz” | S |
| 46 | Matriz g “Não ressuscitar helper removido” | S |
| 47 | Matriz h “Lote misto e repasse” | S |
| 48 | Matriz h2 “Nascer e fechar vazia” | S |
| 49 | Matriz i “Distinguir duplicação de dias legítimos” | S |
| 50 | Matriz j “Declarar dona, motivos e momento” | S |

As quatro unidades N citam apenas relato/anexo sem endereço, referência interna ou nenhum fundamento direto. A unidade 11 fala em “relato do aval”, sem identificar qual aval dentro daquele item; a unidade 22 remete aos endereços de D0, indiretamente. [nao verificado]

Incluí as onze linhas da matriz final porque impõem operação/persistido exigido. Excluí fatos descritivos do CENSO, comandos shell, títulos, o resumo “Decisões do censo” e a tabela final de procedência. Se a matriz normativa também for tratada como repetição e excluída, o replay fica **39 = 35 com / 4 sem**. [nao verificado]

_3581 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
