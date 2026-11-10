# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 10:11** · publicado em **10/11/2026 10:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 10:11 · sessao 9671e38735dd · - · - · origem=Ronald (tmux)

Bancos 1–4:

- **1 realizado no código e no teste de memória:** cron usa `--alarme`, papel `vigia`, fora da tupla; parser rejeita `--apply`. `app/config/crons.py:1521`, `app/config/crons.py:2032`, `app/config/crons.py:2168`, `app/chamados/tests/test_e4a2_matriz_memoria.py:68`.
- **2 realizado no caminho carimbo→placar:** alvo 1 gera alarme pela função real extraída; teste passou. `app/chamados/tests/test_e4a2_matriz_memoria.py:222`.
- **3 realizado em memória:** fronteira Empresa/pk, retido fora da C5 diverge, carimbo publica contador e esperado, placar compara ambos; testes passaram. `app/chamados/tests/test_e4a2_matriz_memoria.py:183`, `app/chamados/tests/test_e4a2_matriz_memoria.py:188`, `app/chamados/tests/test_e4a2_c5_memoria.py:145`.
- **4 executado verde:** `PYTHONDONTWRITEBYTECODE=1 bash bin/tests/test_papel_prazo_nao_deriva.sh` retornou rc0, 12 crons `prazo`, juízes=0. A exceção real está vazia e casos sintéticos cobram delegação, derivação e isolamento da exceção. `bin/tests/test_papel_prazo_nao_deriva.sh:305`, `bin/tests/test_papel_prazo_nao_deriva.sh:384`, `bin/tests/test_papel_prazo_nao_deriva.sh:410`.
- Bancos 5/6, integração ORM e suíte inteira: **[nao verificado]**. [nao verificado]

Mordida:

- Executei os **22 testes de memória**, verdes, incluindo os **26 mutantes**. Cada mutante exige trecho único, controle verde anterior e `AssertionError` com mensagem pretendida. `app/chamados/tests/test_e4a2_matriz_memoria.py:262`, `app/chamados/tests/test_e4a2_c5_memoria.py:207`.
- Executei separadamente controle do concorde e mutante `contador_volta_a_ata`: controle verde; mutante falhou com **`1 != 0 : concorde rejeitado pelo juiz nao e alvo`**. O teste usa o juiz real extraído por AST e confirma a exclusão de `concorde` na tupla real. `app/chamados/tests/test_e4a2_matriz_memoria.py:124`, `app/chamados/tests/test_e4a2_matriz_memoria.py:143`, `app/chamados/tests/test_e4a2_matriz_memoria.py:164`. Execução histórica sobre base real 1a505ad69: **[nao verificado]**; não há `.git` nesta cópia **[nao verificado]**.
- Também executei **11 testes por AST** de fonte/relógio/relatório/SQL e **10 de censo** por AST, substituindo `SimpleTestCase` por `unittest.TestCase` e somente `settings.BASE_DIR` no censo; verdes. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:72`, `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:442`.

**Achado procedente — vacuidade remanescente:**

- Mínimo13 e teste anti-esvaziamento existem nas duas suítes memória. `app/chamados/tests/test_e4a2_matriz_memoria.py:256`, `app/chamados/tests/test_e4a2_matriz_memoria.py:263`, `app/chamados/tests/test_e4a2_c5_memoria.py:201`, `app/chamados/tests/test_e4a2_c5_memoria.py:208`.
- **Não existem mínimos nos catálogos ORM**: os métodos apenas iteram `MUTANTES`; executei seus corpos reais por AST com `MUTANTES=[]` e ambos passaram vazios. `app/chamados/tests/test_e4a2_nunca_bateu.py:309`, `app/core/tests/test_fio_mudo_placar.py:293`, `app/core/tests/test_fio_mudo_placar.py:308`.
- **Catálogo do relatório também sem mínimo:** com catálogo vazio, só roda controle verde; os mutantes verificam qualquer `AssertionError`, não a mensagem pretendida. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:20`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:220`.
- Os laços ORM preservam controle verde e mensagem pretendida quando há entradas, mas sua execução integrada permanece **[nao verificado]**. `app/chamados/tests/test_e4a2_nunca_bateu.py:315`, `app/chamados/tests/test_e4a2_nunca_bateu.py:322`, `app/core/tests/test_fio_mudo_placar.py:313`, `app/core/tests/test_fio_mudo_placar.py:327`.

As mudanças de fixture `concorde`→`furo` preservam contagem, proibição de escrita e retorno, isolando explicitamente o outro vigia; a proteção do concorde ficou em teste próprio. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:309`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:133`, `app/chamados/tests/test_e4a2_matriz_memoria.py:164`. A retirada das exigências de `[BARRADO]`/`FIO DECIDIDO` reduz o contrato de apresentação antigo; agora o relatório exige sua ausência e os comportamentos permanecem cobertos por juiz real/excluídos C5. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:208`, `app/chamados/tests/test_e4a2_matriz_memoria.py:176`, `app/chamados/tests/test_e4a2_nunca_bateu.py:239`.

## 10/11 10:10 · sessao a99179c7c1bc · - · - · origem=Ronald (tmux)

3. **Fronteira comum existe; ela normaliza iteráveis, sem validar o domínio do PK.** `_pks_de_empresa` devolve `None` ou uma lista de inteiros; ambos contadores a chamam, inclusive `contador_nunca_bateu` quando chamado diretamente. No aninhamento há duas invocações da mesma função, sem duas implementações. (`app/chamados/management/commands/disparar_perguntas_competencia.py:24`, `:44`, `:79`, `:112`)

| Entrada | Resultado da fronteira |
|---|---|
| `[Empresa]`, queryset/iterador de Empresa | lista dos `pk` convertidos por `int` |
| `[2]`, `['2']` | `[2]` |
| `None` | `None`, frota inteira |
| `[]` | `[]`, universo de empresas vazio |
| Empresa escalar ou inteiro escalar | `TypeError`, pois não são iteráveis |
| string escalar `'12'` | `[1, 2]`, pois itera caracteres |
| string escalar `'12,13'` | `ValueError` na vírgula |
| `[pk_de_colaborador]` | aceita o inteiro como PK de empresa; não valida tipo lógico |

Todos esses comportamentos decorrem da expressão única e foram confirmados carregando somente essa função por AST, em memória. (`app/chamados/management/commands/disparar_perguntas_competencia.py:24`)

**Chamadores concretos:** `handle` do disparar omite `empresas`, portanto usa `None`; `vigiar_fio_morto --empresa` transforma os IDs em **lista de objetos Empresa** e a repassa ao contador; esse é o caminho cuja normalização evita repassar objetos à C5. `pipeline_placar` não chama contador/C5, apenas lê o arquivo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:179`; `app/chamados/management/commands/vigiar_fio_morto.py:156`, `:160`, `:181`; `app/core/management/commands/pipeline_placar.py:92`)

**C5 direta exige distinção:** a chamada direta de `contador_nunca_bateu` usa a fronteira; a função crua `colabs_nunca_bateu` **não** a usa e executa `Empresa.objects.filter(pk__in=list(empresas))`. Há chamadas cruas fora da obra em `_nunca_bateu` e `folha.vigia_retido_calado.censo`; a tela passa um conjunto de PKs e o comando `vigiar_retido_calado` passa lista de inteiros/None. Logo, “toda chamada C5 da árvore usa essa fronteira” seria falso; “todas as entradas dos contadores do disparar” é verdadeiro. (`app/chamados/management/commands/disparar_perguntas_competencia.py:44`; `app/escala/services/cadastro_realidade.py:177`, `:207`; `app/folha/vigia_retido_calado.py:43`; `app/escala/views.py:487`; `app/folha/management/commands/vigiar_retido_calado.py:29`)

Testes da fronteira: lista de objetos, lista de strings e iterador de objetos na C5 direta; equivalência iterador de objetos/lista de PKs no contador; `None` também aparece nos casos retidos. Não há caso nesses testes para string escalar ou PK de colaborador. (`app/chamados/tests/test_e4a2_c5_memoria.py:89`; `app/chamados/tests/test_e4a2_matriz_memoria.py:183`, `:188`)

5. **Mapa de escrita/leitura e unidades:**

| Valor | Produtor/escritor | Leitor e significado |
|---|---|---|
| RC do disparar | `--alarme && deixou` → 2; demais ramos normais → 0. (`app/chamados/management/commands/disparar_perguntas_competencia.py:208`) | `cron_run.sh` registra exit cru e aceita 0/2 como conclusão; `crons_quebrados` só acusa fora de 0/2. (`bin/cron_run.sh:11`, `:22`, `:32`; `app/core/services/crons_quebrados.py:16`, `:46`) |
| `VIGIA FIO MUDO`, `ALVOS`, distribuição | contador produz motivos distintos por colab/dia/motivo; relatório imprime total e pessoas distintas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`, `:113`, `:187`, `:191`, `:205`) | Nos consumidores de produção pesquisados, não há parser dessas linhas; canal durável é o carimbo. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `:241`; `app/core/management/commands/pipeline_placar.py:98`) |
| `NUNCA_BATEU_SEM_ADESAO` | pares únicos retidos; esperado = interseção dos pares com C5; dono CADASTRO; totais C5 informativos separados. (`app/chamados/management/commands/disparar_perguntas_competencia.py:47`, `:58`, `:182`) | Vigia imprime e carimba valores e bloco completo; placar compara `censo`/`esperado`, sem derivar universo. (`app/chamados/management/commands/vigiar_fio_morto.py:210`, `:233`, `:241`, `:242`; `app/core/management/commands/pipeline_placar.py:98`, `:105`) |
| `FABRICA-POR-EVENTO` | conta motivos faltantes de pergunta **ou pauta DP**, com total de células distinto. (`app/chamados/services/fabrica_da_celula.py:234`, `:242`; `app/chamados/management/commands/disparar_perguntas_competencia.py:199`, `:202`) | `placar_code.sh` extrai última contagem e rotula só `perguntas_que_o_julgamento_nao_fez`; continua divergência de unidade conhecida. (`bin/placar_code.sh:390`, `:392`) |
| `hoje`/`medido_em` | Vigia lê um instante, calcula data civil local, mede com ambos e carimba. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `:181`, `:229`) | Placar exige `registro.hoje == dia.isoformat()`, inteiros reais e número igual ao esperado; outro dia/ausência/malformação → `NAO_MEDIDO`. (`app/core/management/commands/pipeline_placar.py:94`, `:100`, `:102`, `:105`) |
| ESTADO | Vigia grava arquivo atomicamente por `.novo`/replace, apenas em frota e antes do RC. (`app/chamados/management/commands/vigiar_fio_morto.py:118`, `:227`, `:252`) | `fio_estado` compara genericamente todos os esperados e mostra divergências; carimbo não carrega novos esperados obrigatórios no leitor, portanto schema antigo sem essas chaves não as denuncia por si só. (`bin/fio_estado.py:123`, `:125`, `:191`) |

**Divergências que permanecem concretas:**

- `pipeline_placar.medidos` reconhece RC 2 como alarme, e `placar` pode exibir situação `ALARME`, mas não a inclui na lista que determina `ALARME A5`/RC 2. Assim, RC 2 de cron pode coexistir com `A5 OK`; disparar é encadeado e nem entra no universo `CRONS` de `esperados`. Isso é diferente do novo alarme por carimbo, que entra explicitamente. (`app/core/management/commands/pipeline_placar.py:41`, `:76`, `:124`, `:131`, `:135`, `:158`; `app/config/crons.py:1521`)

- `placar_code.sh` lê a última linha FPE do log e só valida que o **mtime do arquivo** é posterior à meia-noite; não associa a linha a uma data embutida. Atualizar o arquivo hoje pode tornar elegível uma última linha FPE antiga. (`bin/placar_code.sh:390`, `:391`)

- O `hoje` do carimbo é data civil local derivada do instante; datas de `celula.data` são dias do turno filtrados por janela. O contador não redefine `hoje` como dia do turno. Já o próprio comando do pipeline usa `date.today()` e duas leituras `datetime.now()` da borda; equivalência de fuso do processo com Django em produção é [nao verificado]. (`app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/chamados/management/commands/disparar_perguntas_competencia.py:89`; `app/core/management/commands/pipeline_placar.py:148`; `app/config/settings/base.py:93`)

- A tela C5 recebe dias e formata **uma assinatura por pessoa** com quantidade de dias; isso não é o contador de pares retidos. Os campos `c5_pessoas`/`c5_dias` mantêm essa diferença explícita. (`app/escala/services/cadastro_realidade.py:204`, `:208`; `app/chamados/management/commands/disparar_perguntas_competencia.py:58`)

- Não localizei leitor de produção da linha `[BARRADO]`, legenda `FIO DECIDIDO` ou chave específica do resultado `cfm['barrados']`; suas exigências permanecem apenas na fonte-base literal de teste ou foram removidas das asserções. O relatório novo explicitamente testa sua ausência. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:208`, `:599`; consumidores novos em `app/chamados/management/commands/vigiar_fio_morto.py:181`, `:241`)

## 10/11 10:10 · sessao 710fef0c0607 · - · - · origem=Ronald (tmux)

Achados para itens 1, 2 e 7:

- **Não há mais decisão paralela de motivos no contador**: depois de selecionar chamados vivos e excluir os que têm pergunta viva, ele separa `nunca_bateu` e apenas itera os motivos retornados por `motivos_a_perguntar(celula, hoje, agora)`, deduplicando `(colab,dia,motivo)`. `app/chamados/management/commands/disparar_perguntas_competencia.py:90`, `:100`, `:102`, `:106`, `:107`.
- **O juiz é comum; os universos não são iguais**: contador exige chamado vivo dentro da janela; cartório chama a fábrica para toda célula julgada neste ato, sem exigir chamado, e o consumidor de eventos relê células por colaborador/data. `app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/ponto/services/cartorio.py:992`, `:1006`; `app/chamados/services/fabrica_da_celula.py:297`.
- **Diferença importante em `ja_viva`**: contador exclui o chamado inteiro se qualquer pergunta ligada está viva; juiz elimina motivos individualmente pelo domínio do fato. Com pergunta viva A e outro motivo B devido, contador nem consulta o juiz, enquanto caminho da célula pode perguntar B. Isso corresponde ao recorte “chamado sem pergunta viva”; não é o mesmo predicado de dedup por motivo. `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/services/fabrica_da_celula.py:145`, `:154`, `:157`.
- **Desligado/parcial/fato existente**: desligado é recusado no próprio juiz; parcial usa `cartorio.parcial_so_lista`; fato existente usa `q_dos_fatos`, a mesma fonte usada por `linhas_dos_fatos`. `app/chamados/services/fabrica_da_celula.py:112`, `:122`, `:145`; `app/chamados/models.py:2743`, `:2746`.
- **Parcial usa o mesmo predicado com instantâneos próprios**: contador usa `cel.julgada_em`; cartório passa `julgada_antes`, capturado antes do corpo. A distinção é expressamente exigida pela função, não cópia de regra. `app/chamados/services/fabrica_da_celula.py:123`; `app/ponto/services/cartorio.py:954`, `:1008`, `:389`.
- **Retratada**: `VIAS_FORA_DO_FATO=('duplicata','motivo_retratado')`; o domínio exclui ambas, permitindo que o motivo volte. Pergunta validada/materializada dentro do fato continua eliminando o motivo; “materializada” sozinha não equivale a “viva”. `app/chamados/models.py:2393`, `:2754`; `app/chamados/juizes.py:1325`, `:1336`; `app/chamados/services/fabrica_da_celula.py:154`.
- **Motivo do juiz nem sempre vira pergunta no app**: escritor pode desviar limbo em competência entregue para Pauta DP; o contador FIO MUDO usa diretamente os motivos e não desconta a pauta. Já o outro vigia, `motivos_que_o_evento_deixou`, desconta pautas do limbo. Isso é consequência do juiz explicitamente mandado pela errata, mas deve constar da análise de unidade/significado. `app/chamados/services/fabrica_da_celula.py:214`, `:223`, `:242`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`.

Matriz:

- **(a)** Lê a lâmpada `veredito=='nunca_bateu'`, sem reconsultar adesão. Fábrica devolve `[]`; contador encaminha os pares para comparação C5, filtrando janela anterior até ontem e desligados ausentes da C5. Não persiste perguntas. Fonte da lâmpada: cartório calcula adesão pela primeira batida não retratada e lavra `nunca_bateu` quando ausente e sem cobertura integral. `app/chamados/services/fabrica_da_celula.py:87`, `:105`, `:183`; `app/chamados/management/commands/disparar_perguntas_competencia.py:44`, `:51`, `:56`, `:58`, `:102`; `app/ponto/services/cartorio.py:1165`, `:1173`, `:1243`, `:1252`.
  Provas disponíveis: teste real com controle furo, comparação C5 real e ausência estrutural simulada; teste em memória para retido fora da C5. `app/chamados/tests/test_e4a2_nunca_bateu.py:184`, `:218`, `:297`; `app/chamados/tests/test_e4a2_matriz_memoria.py:188`.
- **(b)** Juiz verifica dia sem trabalho, situação, futuro, teto, `VEREDITOS_ACUSAM`, parcial e fatos existentes; contador soma todos os motivos retornados, esperado zero, dono ESTRUTURA. `app/chamados/services/fabrica_da_celula.py:112`, `:114`, `:119`, `:120`, `:123`, `:157`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`, `:113`.
  Provas: alvo sem escrita; alvo um publicado deixa placar vermelho; RED concorde rejeita veredito fora da tupla. `app/chamados/tests/test_e4a2_matriz_memoria.py:164`, `:200`, `:222`.
- **(c)** Pergunta ligada viva elimina ambos os contadores; pergunta do fato já existente também elimina o motivo no juiz. Retratada é exceção correta da errata. Prova real materializa pela porta e verifica vitalidade antes de exigir `(0,0)`; prova em memória tem controle sem pergunta. `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/tests/test_e4a2_nunca_bateu.py:148`; `app/chamados/tests/test_e4a2_matriz_memoria.py:205`.
- **(d)** Garantia do contador é “chamado já encerrado pela tranca está fora de VIVOS”, não consulta direta da tranca. Teste real cria dois cenários, tranca pela porta, verifica status fora de VIVOS e contadores zerados. `app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/chamados/tests/test_e4a2_nunca_bateu.py:163`.
  Há caminho além do caso pequeno do teste: fechamento trata encerramento como best-effort; acima de 100 ações dispara subprocesso sem esperar. Enquanto encerramento está pendente ou falhou, chamado vivo trancado pode continuar selecionado; `motivos_a_perguntar` não consulta a tranca. Isso não reabre o item descartado pela errata, mas impede afirmar que tranca e VIVOS são predicados equivalentes em qualquer instante. `app/ponto/services/fechamento.py:1420`, `:1426`; `app/chamados/services/competencia_trancada.py:45`, `:276`, `:297`; `app/chamados/services/fabrica_da_celula.py:105`.

Retorno:

- `rc=2` somente se `--alarme` e `deixou` não vazio; logo pode coexistir com retido/alvo contado, mas sua origem é o outro vigia. Sem `deixou` retorna normalmente mesmo com FIO MUDO/C5 divergentes. `app/chamados/management/commands/disparar_perguntas_competencia.py:208`.
- Testes que isolam rc dos casos (a)/(b) explicitamente dublam `motivos_que_o_evento_deixou=[]`; teste combinado preserva rc2 da célula. `app/chamados/tests/test_e4a2_nunca_bateu.py:128`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:133`, `:162`.

Classes históricas:

- Nenhum segundo escritor de perguntas permanece no comando examinado; ele deriva listas, imprime e lança `SystemExit`. Escrita permanece em fábrica→portas de criação/geração. `app/chamados/management/commands/disparar_perguntas_competencia.py:106`, `:182`, `:208`; `app/chamados/services/fabrica_da_celula.py:165`, `:172`, `:217`.
- Relógio do comando é lido uma vez na borda e passado tanto à vitalidade quanto ao juiz; o antigo `_ja_perguntou→fio_vivo` sem `hoje` saiu deste caminho. `app/chamados/management/commands/disparar_perguntas_competencia.py:137`, `:100`, `:106`; antigo auxiliar permanece em `app/chamados/services/disputa_emissao.py:1415`.
- Não executei testes nesta exploração; execução e estado de produção [nao verificado]. [nao verificado]

## 10/11 10:06 · sessao 993b5d2f515a · FIO-E0-V4-E4A-2 · construtor · origem=Code

A cura está no diff: FIO MUDO lê `motivos_a_perguntar(celula, hoje, agora)`, conta todos os motivos devolvidos e deduplica por colaborador/dia/motivo. A normalização de empresas é compartilhada pelos dois contadores. ([comando:24](chamados/management/commands/disparar_perguntas_competencia.py:24), [comando:106](chamados/management/commands/disparar_perguntas_competencia.py:106))

**RED concorde:** escrito e executado antes da cura; na base, falhou com `1 != 0` (rc 1). Após a cura, passou com zero alvos. O teste consulta o juiz real por AST e confirma que ele rejeita `concorde`. ([teste:141](chamados/tests/test_e4a2_matriz_memoria.py:141), [teste:164](chamados/tests/test_e4a2_matriz_memoria.py:164))

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4a2_matriz_memoria.MatrizMemoriaTest.test_RED_concorde_sem_pergunta_nao_e_alvo -v
```

Arquivos tocados, além do comando:

- [test_e4a2_matriz_memoria.py:24](chamados/tests/test_e4a2_matriz_memoria.py:24): RED, delegação, relógio e mutantes.
- [test_e4a2_c5_memoria.py:89](chamados/tests/test_e4a2_c5_memoria.py:89): entrada direta normalizada e guarda contra vacuidade.
- [test_e4a2_disparar_e_vigia.py:171](chamados/tests/test_e4a2_disparar_e_vigia.py:171): relatório e fixture acusadora.
- [test_jv4a_disparar_vigia_da_celula.py:133](chamados/tests/test_jv4a_disparar_vigia_da_celula.py:133): fixture `furo`, preservando as asserções de contagem, escrita e retorno.
- [test_e4a2_nunca_bateu.py:230](chamados/tests/test_e4a2_nunca_bateu.py:230): leitura dos excluídos diretamente no resultado C5.
- [test_selo_emissor_fio_mudo.py:87](chamados/tests/test_selo_emissor_fio_mudo.py:87): retirada das exigências dos baldes.
- [test_colab_nao_vaza_fio_mudo.py:38](chamados/tests/test_colab_nao_vaza_fio_mudo.py:38): dublê na nova dependência.
- [test_cobparc5_parcial_sem_periodo_so_lista.py:337](chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:337): selo exige delegação ao juiz.
- [test_fio_mudo_placar.py:265](core/tests/test_fio_mudo_placar.py:265): mantém dedup/janela e verifica os argumentos enviados ao juiz.

Saíram `barrados.desligado`, `ja_viva`, `fato_ja_existe` e `parcial_lista`, toda a linha `[BARRADO]` e a legenda `FIO DECIDIDO`. Seus leitores eram o relatório de `handle` e as asserções ajustadas acima. O rótulo agora declara “motivos devidos pelo juiz da celula”. `barrados_desligado` permanece exclusivamente no resultado C5. ([relatório:179](chamados/management/commands/disparar_perguntas_competencia.py:179), [resultado C5:58](chamados/management/commands/disparar_perguntas_competencia.py:58), [teste do relatório:208](chamados/tests/test_e4a2_disparar_e_vigia.py:208))

| Campo | Quem escreve/produz | Quem lê |
|---|---|---|
| `empresas` normalizadas | [_pks_de_empresa:24](chamados/management/commands/disparar_perguntas_competencia.py:24) | [ORM:95](chamados/management/commands/disparar_perguntas_competencia.py:95), [C5:46](chamados/management/commands/disparar_perguntas_competencia.py:46); C5 espera empresas em [cadastro_realidade:177](escala/services/cadastro_realidade.py:177) |
| Motivos devidos | [juiz:92](chamados/services/fabrica_da_celula.py:92) | [contador:106](chamados/management/commands/disparar_perguntas_competencia.py:106) |
| `alvos`, `numero`, `esperado`, `dono` do FIO MUDO | [contador:113](chamados/management/commands/disparar_perguntas_competencia.py:113) | [relatório:179](chamados/management/commands/disparar_perguntas_competencia.py:179), [vigia:209](chamados/management/commands/vigiar_fio_morto.py:209) |
| Contagem/esperado C5, informativos, universo e diagnósticos | [contador_nunca_bateu:58](chamados/management/commands/disparar_perguntas_competencia.py:58) | [relatório:182](chamados/management/commands/disparar_perguntas_competencia.py:182), [vigia:212](chamados/management/commands/vigiar_fio_morto.py:212), resultado completo no [carimbo:242](chamados/management/commands/vigiar_fio_morto.py:242) |
| `censo`/`esperado` no ESTADO | [vigia:233](chamados/management/commands/vigiar_fio_morto.py:233), [vigia:241](chamados/management/commands/vigiar_fio_morto.py:241) | [pipeline_placar:98](core/management/commands/pipeline_placar.py:98), [fio_estado:125](../bin/fio_estado.py:125) |

Placar e ESTADO comparam número e esperado, sem rederivar alvos. A divergência de significado já conhecida continua fora desta volta: `placar_code.sh` rotula como perguntas o contador FABRICA que também inclui pauta. ([placar:107](core/management/commands/pipeline_placar.py:107), [legenda:202](chamados/management/commands/disparar_perguntas_competencia.py:202), [bin:392](../bin/placar_code.sh:392))

Passaram **22 testes em memória**, com **26 mutantes rejeitados**. Os catálogos exigem mínimo de 13 cada; esvaziá-los retornou rc 1. O mutante que volta à ata também falhou diretamente com `1 != 0`. ([matriz:262](chamados/tests/test_e4a2_matriz_memoria.py:262), [C5:207](chamados/tests/test_e4a2_c5_memoria.py:207), [RED:166](chamados/tests/test_e4a2_matriz_memoria.py:166))

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4a2_matriz_memoria \
  chamados.tests.test_e4a2_c5_memoria -v
```

Este comando imprime e executa o comando individual de **cada mutante**; rc 0 do ensaio significa que capturou a falha esperada. ([matriz:272](chamados/tests/test_e4a2_matriz_memoria.py:272), [C5:217](chamados/tests/test_e4a2_c5_memoria.py:217))

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from importlib import import_module
import subprocess
import sys

for grupo in ('matriz', 'c5'):
    modulo = f'chamados.tests.test_e4a2_{grupo}_memoria'
    m = import_module(modulo)
    assert len(m.MUTANTES) >= m.MINIMO_MUTANTES
    for nome, *_ in m.MUTANTES:
        cmd = [sys.executable, '-m', modulo, nome]
        print('PYTHONDONTWRITEBYTECODE=1 ' + ' '.join(cmd), flush=True)
        subprocess.run(cmd, check=True)
PY
```

Também passaram por AST 11 testes de fonte/relógio/relatório/SQL e 10 do censo de delegação. ([testes:72](chamados/tests/test_e4a2_disparar_e_vigia.py:72), [censo:442](chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:442))

Não rodei integração com banco, os 165 módulos, suite inteira ou medição de produção: **[nao verificado]**. `PYTHONDONTWRITEBYTECODE=1 python3 -m ruff --version` retornou `No module named ruff`; Ruff limpo: **[nao verificado]**. Certificação por Code/suite e opus final: **[nao verificado]**. [nao verificado]

## 10/11 09:47 · sessao b3403e72900e · FIO-E0-V4-E4A-2 · verificador · origem=Code

**Pre-verificação: REPROVADA na aderência literal à MATRIZ.** Há seleção diferente do juiz da fábrica, caminho de classe (b) com rc 2, ausência de guarda da tranca na porta inteira e universo por Empresa, enquanto a matriz pede pks de colaborador. (`disparar_perguntas_competencia.py:72`, `:109`, `:189`, `:238`; `fabrica_da_celula.py:120`.)

1. **MATRIZ (a), (b), (c), (d)**

| Linha | Estado, juiz, operação e persistência | Retorno, prova e caminhos diferentes |
|---|---|---|
| **(a) PARCIAL** | Lê `veredito == 'nunca_bateu'` pelo predicado da fábrica; separa os pares em retidos e compara com C5. Não consulta adesão novamente nem chama a porta escritora. Nada escrito pelo contador. (`fabrica_da_celula.py:87`; `disparar_perguntas_competencia.py:98`, `:134`.) | O retido isolado retorna normalmente; a fábrica também retorna antes de emitir. A prova de divergência `(1,0)` está em `test_e4a2_matriz_memoria.py:130`. **Limites:** só entra chamado vivo; desligado ausente da C5 é retirado do numerador; datas anteriores ao piso também saem. Logo “um retido fora da C5 ⇒ divergência” não vale para todos os retidos. (`disparar_perguntas_competencia.py:42`, `:47`, `:86`; `fabrica_da_celula.py:183`.) |
| **(b) NÃO CONFORME** | O contador chama diretamente `motivos_pela_ata` e aplica barreiras próprias; não passa pela decisão completa de `motivos_a_perguntar`. Conta `fio_mudo_alvos`, esperado 0, dono ESTRUTURA; nada escreve. (`disparar_perguntas_competencia.py:105`, `:109`, `:120`, `:129`, `:136`.) | **Pode retornar 2:** a mesma célula também alimenta `deixou`; com `--alarme`, isso produz `SystemExit(2)`. O teste de limbo exige precisamente FIO MUDO 1, deixou 1, nenhuma pergunta e rc 2. (`disparar_perguntas_competencia.py:199`, `:238`; `test_jv4a_disparar_vigia_da_celula.py:160`.) | [nao verificado]
| **(c) CONFORME PARA PERGUNTA VIVA; MAIS ESTREITO QUE O TEXTO LITERAL** | Pergunta viva ligada ao chamado encerra a seleção antes dos dois contadores; nada escreve. (`disparar_perguntas_competencia.py:96`.) | Prova com controle positivo: `test_e4a2_matriz_memoria.py:147`; integração proposta: `test_e4a2_nunca_bateu.py:148`. Entretanto, uma linha com `materializada_em` e via retratada volta a ser alvo; portanto o código não implementa “qualquer materializada ⇒ nada”. (`test_jv4a_disparar_vigia_da_celula.py:151`, `:155`.) |
| **(d) NÃO PROVADA NA PORTA INTEIRA** | O contador exclui chamados encerrados pelo filtro `VIVOS`; não consulta diretamente a tranca. O primeiro universo continua selecionando células. Nada escreve. (`disparar_perguntas_competencia.py:86`, `:189`.) | O teste em memória apenas troca status para `resolvido`. O teste Django tranca realmente, mas confere somente `_contador`, sem executar o comando inteiro. Como trancar não altera célula/ata, uma célula acusadora sem pergunta do fato continua alcançável pelo ramo `deixou`, permitindo rc 2. (`test_e4a2_matriz_memoria.py:156`; `test_e4a2_nunca_bateu.py:163`; `fechamento.py:1422`; `disparar_perguntas_competencia.py:199`, `:238`.) | [nao verificado]

O contraexemplo mais direto ao **juiz único de (b)** já está nos testes: `concorde` deve produzir FIO MUDO 1, mas a fábrica rejeita veredito preenchido fora de `VEREDITOS_ACUSAM`; `concorde` está fora dessa tupla. Assim, compartilhar auxiliares não tornou a decisão de **DEVIDA** única. (`test_jv4a_disparar_vigia_da_celula.py:132`; `fabrica_da_celula.py:120`; `escala/models.py:1767`.)

A prova chamada “classes a/b rc zero” **silencia explicitamente o outro ramo** com `motivos_que_o_evento_deixou=[]`. Prova o retorno dos contadores isolados, não o contrato da porta inteira. (`test_e4a2_nunca_bateu.py:128`, `:135`.)

2. **CONTRATO DE SAÍDA, bancos 1–4**

- **Banco 1 — realizado no código:** lista de juízes por varredura vazia, papel `vigia`, cron com `--alarme` e parser sem `--apply`. Há prova explícita da rejeição do argumento. (`config/crons.py:2032`, `:2168`, `:1521`; `disparar_perguntas_competencia.py:145`; `test_e4a2_matriz_memoria.py:55`.)
- **Banco 2 — ligação realizada; significado divergente:** `fio_mudo_alvos` é carimbado e comparado pelo placar; alvo 1 deixa o placar vermelho. Porém o universo inclui o `concorde` rejeitado pela fábrica, como mostrado no item 1. (`vigiar_fio_morto.py:233`, `:241`; `pipeline_placar.py:135`; `test_e4a2_matriz_memoria.py:164`; `test_jv4a_disparar_vigia_da_celula.py:133`.)
- **Banco 3 — realizado de forma diferente da matriz literal:** calcula `numero=|pares|` e `esperado=|pares∩C5|`, mas os pares já foram reduzidos por chamado vivo, piso temporal e exclusão de desligados ausentes da C5. Além disso, a entrada normalizada contém pks de **Empresa**. O teste exige deliberadamente `(0,0)` para desligado fora da C5. (`disparar_perguntas_competencia.py:42`, `:47`, `:49`, `:72`, `:86`; `test_e4a2_nunca_bateu.py:230`.) [nao verificado]
- **Banco 4 — lista de exceções vazia e selo com mordida sintética:** não fica dependente de haver juiz real na lista para exercitar o verificador. (`bin/tests/test_papel_prazo_nao_deriva.sh:305`, `:384`, `:391`, `:405`.)

O selo foi executado nesta sessão e terminou verde, anunciando 12 crons de prazo e zero juízes por varredura; não foi criado registro persistido dessa execução: **[nao verificado]**. [nao verificado]

**Bancos 5 e 6:** alvo 165, suíte inteira, C11 efetivamente no ar e medição da rodada de produção permanecem **[nao verificado]**. O material fornecido não apresenta uma medição dessa rodada que certifique ALVOS 0: **[nao verificado]**. [nao verificado]

3. **Universo e tipos recebidos**

**Não: todo contador não recebe lista de pks de colaboradores.** `empresas` filtra `Empresa.pk`; `colab` é escalar separado; `contador_nunca_bateu` recebe pares contendo objetos Colaborador e extrai seus pks internamente. (`disparar_perguntas_competencia.py:41`, `:48`, `:58`, `:72`, `:89`, `:91`; `cadastro_realidade.py:177`.)

| Chamador/entrada | Comportamento |
|---|---|
| `vigiar_fio_morto --empresa` | Ainda fornece `list[Empresa]`; a fronteira do contador converte para inteiros. (`vigiar_fio_morto.py:160`, `:181`; `disparar_perguntas_competencia.py:72`.) |
| Frota e `disparar.handle` | Usam `None`, preservando todas as empresas; não materializam lista de pks. (`vigiar_fio_morto.py:156`; `disparar_perguntas_competencia.py:202`, `:65`.) |
| Testes | Passam `[Empresa]`, `[empresa.pk]` e iterador de objeto com `.pk`; há comparação dos resultados. (`test_e4a2_nunca_bateu.py:118`, `:124`; `test_e4a2_matriz_memoria.py:125`.) | [nao verificado]
| Tela C5 | Fornece conjunto de pks de Empresa. (`escala/views.py:487`.) |
| Outro comando | `vigiar_retido_calado` fornece inteiros ou `None`; seu censo chama C5. (`vigiar_retido_calado.py:29`; `folha/vigia_retido_calado.py:43`.) |
| Queryset/iterável na fronteira | Seus elementos são consumidos e convertidos por `.pk` ou `int`; não há validação de que representam Empresas. (`disparar_perguntas_competencia.py:72`.) |
| String na fronteira | Pela compreensão, `"12"` vira `[1,2]`; `"1,2"` levanta `ValueError`; Empresa escalar não iterável levanta `TypeError`. (`disparar_perguntas_competencia.py:72`.) |
| C5 chamada diretamente | Continua usando `list(empresas)` em `Empresa.pk__in`, sem a normalização da fronteira. A correção não protege chamadas diretas com objetos; execução dessa exceção ORM: **[nao verificado]**. (`cadastro_realidade.py:177`.) |

Chamador concreto que ainda forneça string não foi identificado; ausência universal desse chamador: **[nao verificado]**. `pipeline_placar` lê o JSON e não chama o contador. (`pipeline_placar.py:92`, `:135`.)

4. **Quem escreve, quem lê e significado**

| Campo/saída | Escritor → leitor e unidade |
|---|---|
| **rc de `--alarme`** | `disparar` produz 2 somente por `deixou`; `cron_run` registra e considera 0/2 elo concluído; `crons_quebrados` também aceita ambos. Nenhum desses dois leitores interpreta 2 exclusivamente como FIO MUDO. (`disparar_perguntas_competencia.py:238`; `bin/cron_run.sh:13`, `:19`, `:22`; `crons_quebrados.py:16`, `:46`.) |
| **`VIGIA FIO MUDO`** | Imprime `cfm.numero`; as provas conferem o texto e sua quantidade. (`disparar_perguntas_competencia.py:210`; `test_e4a2_matriz_memoria.py:191`.) |
| **`FABRICA-POR-EVENTO`** | Imprime `len(deixou)`, incluindo pergunta **ou Pauta DP**. `placar_code` raspa esse prefixo, mas apresenta o resultado apenas como “perguntas que o julgamento não fez”. Há diferença de unidade/apresentação. (`disparar_perguntas_competencia.py:228`, `:231`; `bin/placar_code.sh:390`, `:392`.) |
| **Contadores no carimbo** | `disparar` calcula; `vigiar_fio_morto` publica número e esperado; pipeline compara ambos. O vigia não inclui esses novos contadores em seu próprio `gritou`. (`disparar_perguntas_competencia.py:49`, `:136`; `vigiar_fio_morto.py:185`, `:233`, `:241`; `pipeline_placar.py:98`, `:105`.) |
| **ESTADO renderizado** | `fio_estado` compara genericamente todas as chaves de `esperado`; exibe as novas quando divergentes. Não devolve o bloco integral C5 nem mostra explicitamente seus totais informativos. (`bin/fio_estado.py:125`, `:143`, `:191`.) |
| **`c5_dias/c5_pessoas`** | Totais da C5 inteira, impressos nos dois comandos e guardados no bloco integral, fora de `censo/esperado`. (`disparar_perguntas_competencia.py:51`, `:205`; `vigiar_fio_morto.py:210`, `:242`.) |
| **`celula_nunca_bateu`** | Causa atribuída pelo veredito; catálogo dá dono admin; `retencao`, `medir` e `do_dia` a propagam. Não comprova presença na C5. O comando publica causa/quantidade/dono; `dia_do_colab` lê `do_dia`. (`disputa_emissao.py:758`; `pergunta_no_app.py:35`, `:99`, `:167`, `:189`; `chamados_vivos_sem_pergunta_no_app.py:19`; `dia_do_colab.py:108`.) |

Há três problemas adicionais de leitura:

- **Hoje injetado escapa pelo segundo dedup:** contador → `_ja_perguntou` → `fio_vivo` → `pergunta_viva` sem `hoje` → fallback com `date.today()`. Pode mudar o balde de barreira; diferença no total final não foi demonstrada. (`disparar_perguntas_competencia.py:124`; `disputa_emissao.py:1422`; `chamados/juizes.py:1434`, `:1389`, `:577`.)
- **rc 2 pode coexistir com `A5 OK`:** pipeline marca situação `ALARME`, mas não a acrescenta à lista que controla seu próprio alarme e conclusão. (`pipeline_placar.py:124`, `:131`, `:158`, `:163`.)
- **Datas têm bordas diferentes:** seleção usa `celula.data`; o carimbo deriva dia civil local de um instante único; pipeline usa `date.today()` e duas chamadas `datetime.now()`. Falha concreta de fuso em produção: **[nao verificado]**. (`disparar_perguntas_competencia.py:101`; `vigiar_fio_morto.py:166`, `:230`; `pipeline_placar.py:148`.)

5. **Os testes mordem?** [nao verificado]

- **Memória C5/matriz:** os 12 mutantes C5 e 10 da matriz têm controle verde antes de cada mutação, ocorrência única do alvo e captura de `AssertionError` pela mensagem pretendida. (`test_e4a2_c5_memoria.py:30`, `:191`; `test_e4a2_matriz_memoria.py:24`, `:199`.) [nao verificado]
- **Django:** os 11 mutantes de `nunca_bateu` e 12 do placar possuem controles e captura da asserção pretendida no código. Sua execução permanece **[nao verificado]**. (`test_e4a2_nunca_bateu.py:34`, `:311`; `test_fio_mudo_placar.py:30`, `:299`.)
- **Escrita:** existe veto SQL da chamada inteira, incluindo callbacks `on_commit`, e controles de UPDATE que não mudam contagens. Portanto a prova proposta vai além de “quantidade de linhas não mudou”; execução Django **[nao verificado]**. (`test_e4a2_disparar_e_vigia.py:325`, `:338`, `:356`.)
- **Rc na 9b:** o caso exclusivamente FIO MUDO foi invertido para zero; retratada, limbo e demais casos consultados conservaram 2, acrescentando prova da pendência da célula. As fórmulas gerais retiraram somente `_mudos` da condição. (`test_jv4a_disparar_vigia_da_celula.py:139`, `:157`, `:167`; `test_pergunta_do_dia_em_curso.py:194`, `:398`; `test_selo_emissor_fio_mudo.py:290`; `test_e4a2_disparar_e_vigia.py:202`.)
- **Vacuidade:** as listas atuais são não vazias, mas seus laços não exigem quantidade mínima; esvaziar `MUTANTES` faria passar o teste de mutantes isolado. A tranca em memória é apenas status simulado, e a prova de rc injeta `deixou` manualmente. (`test_e4a2_matriz_memoria.py:156`, `:181`, `:199`; `test_e4a2_c5_memoria.py:191`.) [nao verificado]

Nesta sessão passaram os 16 testes de memória, incluindo os 22 mutantes; também passaram 11 testes de fonte/relatório/relógio/SQL extraídos por AST, sem Django. Resultados observados sem registro persistido: **[nao verificado]**. Isso não certifica integração Django, os 165 módulos ou a suíte inteira: **[nao verificado]**. [nao verificado]

6. **Classes que já reprovaram obra**

| Classe | Resultado desta leitura |
|---|---|
| **Segundo juiz/derivação paralela** | **Presente na decisão de DEVIDA:** contador admite `concorde`, fábrica rejeita. (`disparar_perguntas_competencia.py:109`; `fabrica_da_celula.py:120`; `test_jv4a_disparar_vigia_da_celula.py:133`.) |
| **Segundo escritor fora da porta** | Não identificado no corpo de `disparar`; o verificador de fonte veta portas e operações de escrita. Ausência de DML em toda integração: **[nao verificado]**. (`test_e4a2_disparar_e_vigia.py:38`, `:81`, `:325`.) |
| **Relógio fora da borda** | **Presente**, pela cadeia do segundo dedup até `date.today()`. (`disparar_perguntas_competencia.py:124`; `chamados/juizes.py:1434`, `:577`.) |
| **Selo/RED sem mordida** | Os mutantes exercitados mordem, mas não cobrem composição completa de rc e tranca; o teste de rc zero desliga justamente o ramo concorrente. (`test_e4a2_matriz_memoria.py:199`; `test_e4a2_nunca_bateu.py:135`, `:163`.) | [nao verificado]
| **Ordem de trava** | Na porta de pauta consultada, trava antecede seleção e escrita; não identifiquei inversão nesse trecho. (`fabrica_da_celula.py:364`, `:365`, `:367`, `:369`.) |
| **Campo com dois significados** | **Presente na apresentação:** `FABRICA-POR-EVENTO` soma perguntas e pautas, mas o raspador apresenta apenas perguntas; “FIO DECIDIDO” significa existência de linha do fato, sem exigir decisão humana. (`disparar_perguntas_competencia.py:214`, `:231`; `bin/placar_code.sh:392`.) |
| **Pouso antes do pré-requisito** | Pouso, suíte completa e medição de produção não demonstrados: **[nao verificado]**. | [nao verificado]

## 10/11 09:43 · sessao 745e78cb38b7 · - · - · origem=Ronald (tmux)

Achados principais, todos na cópia:

- **(b) não usa o mesmo juiz de DEVIDA.** O contador consulta `motivos_pela_ata` diretamente e aplica barreiras próprias (`app/chamados/management/commands/disparar_perguntas_competencia.py:105`, `:109`, `:120`, `:129`). O juiz da fábrica rejeita `veredito` preenchido fora de `VEREDITOS_ACUSAM` (`app/chamados/services/fabrica_da_celula.py:120`). O teste exige expressamente **concorde → FIO MUDO 1**, enquanto o juiz rejeita concorde (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132`; `app/escala/models.py:1767`). É uma seleção paralela com resultado diferente, não apenas outro nome da mesma decisão.

- **(b) pode produzir rc 2 na porta inteira.** A célula entra também em `deixou` (`app/chamados/management/commands/disparar_perguntas_competencia.py:199`), e `--alarme + deixou` sai 2 (`:238`). O caso limbo comprova FIO MUDO 1, deixou 1, nenhuma pergunta e rc 2 (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:160`). A prova “classes a/b rc zero” elimina esse caminho por mock (`app/chamados/tests/test_e4a2_nunca_bateu.py:135`).

- **(d) só fica fora do contador por status encerrado; a porta inteira não tem guarda da tranca.** O contador filtra `VIVOS` (`app/chamados/management/commands/disparar_perguntas_competencia.py:86`). O primeiro universo seleciona células, incluindo as que continuam acusando depois de encerrado o chamado (`:189`, `:199`). Trancar não toca célula/ata e o encerramento é best effort (`app/ponto/services/fechamento.py:1422`). O teste de tranca só chama `_contador`, nunca o comando inteiro (`app/chamados/tests/test_e4a2_nunca_bateu.py:163`). A prova em memória muda apenas status para resolvido (`app/chamados/tests/test_e4a2_matriz_memoria.py:156`).

- **Há relógio fora da borda pelo segundo dedup.** `contador_fio_mudo` repassa `hoje` à primeira vitalidade (`app/chamados/management/commands/disparar_perguntas_competencia.py:96`), mas `_ja_perguntou` não recebe hoje (`:124`). Este chama `fio_vivo` (`app/chamados/services/disputa_emissao.py:1422`), que chama `pergunta_viva(p)` sem hoje (`app/chamados/juizes.py:1434`). Nos degraus condicionais da vitalidade (`:1389`), `_dentro_da_janela_viva` usa `date.today()` (`:577`). O teste do relógio inspeciona somente `_viva` (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:158`).

- **Universo literal não é lista de pks de colaboradores em todo contador.** `empresas` é normalizado para pks de Empresa (`app/chamados/management/commands/disparar_perguntas_competencia.py:72`) e filtra `empresa_id` (`:91`); C5 consulta `Empresa.pk` (`app/escala/services/cadastro_realidade.py:177`). `colab` é escalar opcional (`disparar_perguntas_competencia.py:58`, `:89`), e `contador_nunca_bateu` recebe pares com objetos Colaborador, convertidos internamente para `(colab.pk,dia)` (`:41`, `:48`).

- **(c) a exclusão é de pergunta viva, não de toda pergunta materializada.** O teste retratada deixa uma linha `materializada_em` e exige novo alvo e rc 2, sem escrever irmã (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:151`, `:155`, `:158`). Logo “materializada → nada” precisa ser lido como “materializada e viva” para coincidir com esse código (`app/chamados/management/commands/disparar_perguntas_competencia.py:96`).

- **(a) cálculo:** nunca_bateu é lido da lâmpada pelo predicado único da fábrica (`app/chamados/services/fabrica_da_celula.py:87`); a fábrica retorna vazio antes de emissão (`:183`); contador separa retidos (`app/chamados/management/commands/disparar_perguntas_competencia.py:98`), compara pares únicos com C5 (`:49`), respeita piso anterior/ontem e barreira RH (`:42`, `:47`). Os testes não provam o desenho literal para células sem chamado vivo: ambos os contadores partem de chamados vivos (`:86`).

Executei apenas provas AST em memória com `python -B`: concorde deu juiz `[]` versus contador 1; handle com chamado encerrado e um motivo fornecido pelo juiz saiu 2. As bordas foram dubladas; isso confirma o fluxo acima, sem certificar integração Django. Execução sem registro em arquivo: **[nao verificado]**. [nao verificado]

## 10/11 09:43 · sessao 56a01243294d · - · - · origem=Ronald (tmux)

Achados principais:

- **Universo literal divergente:** o parâmetro é `empresas`; normaliza **pks de Empresa** e filtra `colaborador__empresa_id__in`. O colaborador é um filtro escalar separado. `contador_nunca_bateu` recebe pares `(objeto Colaborador, dia)`, não lista de pks de colaboradores. `app/chamados/management/commands/disparar_perguntas_competencia.py:41`, `:58`, `:72`, `:89`, `:91`.
- **Ainda há relógio interno alcançável:** contador chama `_ja_perguntou` sem `hoje` → `fio_vivo` → `pergunta_viva` sem `hoje` → fallback `_dentro_da_janela_viva` usa `date.today()`. Isso pode alterar o balde `ja_viva` versus `fato_ja_existe`; não provei diferença no total final, pois a identidade do fato é checada depois. `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `:129`; `app/chamados/services/disputa_emissao.py:1422`; `app/chamados/juizes.py:1434`, `:1389`, `:577`.
- **Placar admite `ALARME` com conclusão `A5 OK`:** rc 2 incrementa `alarmes`, vira situação `ALARME`, mas somente `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA` entram na lista que determina retorno e `A5 OK`. Uma pendência exclusivamente no ramo célula pode ter esse resultado quando os outros alarmes estão limpos. `app/core/management/commands/pipeline_placar.py:76`, `:124`, `:131`, `:158`, `:163`.
- **Placar tem relógios de borda inconsistentes:** usa `date.today()` e duas chamadas `datetime.now()`, enquanto vigia usa um `timezone.now()` e deriva data local dele. Falha real por fuso nesta execução: [nao verificado]. `app/core/management/commands/pipeline_placar.py:148`; `app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/config/settings/base.py:93`.
- **Placar lê duas fotografias potenciais:** as duas chamadas de `alarmes_fio_mudo` reabrem o JSON; `os.replace` garante JSON inteiro por leitura, mas não garante que os dois contadores venham da mesma rodada se houver troca entre chamadas. Ocorrência real: [nao verificado]. `app/core/management/commands/pipeline_placar.py:92`, `:135`; `app/chamados/management/commands/vigiar_fio_morto.py:137`.

Chamadores/tipos:

| Chamador | Entrada e efeito |
|---|---|
| `vigiar_fio_morto --empresa` | Monta `list[Empresa]`; contador normaliza para pks. `app/chamados/management/commands/vigiar_fio_morto.py:156`, `:160`, `:181`; `app/chamados/management/commands/disparar_perguntas_competencia.py:72`. |
| `vigiar_fio_morto` frota | Passa `None`, preservando frota inteira. `app/chamados/management/commands/vigiar_fio_morto.py:156`, `:181`. |
| `disparar.handle` | Omite empresas, portanto `None`; passa `colab` escalar. `app/chamados/management/commands/disparar_perguntas_competencia.py:202`. |
| Testes reais | `[self.emp]` e `[self.emp.pk]`. `app/chamados/tests/test_e4a2_nunca_bateu.py:118`, `:124`. |
| Teste memória | Iterador de objeto com `.pk` e `[2]`. `app/chamados/tests/test_e4a2_matriz_memoria.py:122`. |
| C5 avulsa | Não normaliza objetos: usa `list(empresas)` diretamente em `Empresa.pk__in`. `[Empresa]`/queryset de Empresa não recebem a cura da fronteira do contador; exceção ORM executada: [nao verificado]. `app/escala/services/cadastro_realidade.py:177`. |
| Outra porta | `vigiar_retido_calado` passa lista de pks inteiros ou None, com hoje de borda. `app/folha/management/commands/vigiar_retido_calado.py:29`; `app/folha/vigia_retido_calado.py:43`. |
| Tela | Passa conjunto de pks de Empresa → `lista` → `_nunca_bateu` → C5. `app/escala/views.py:487`; `app/escala/services/cadastro_realidade.py:878`, `:207`. |

Strings não foram encontradas nos chamadores pesquisados [nao verificado como ausência universal]. Pelo trecho da fronteira, `"12"` é iterada e vira `[1,2]`; `"1,2"` dá `ValueError`; objeto Empresa escalar não iterável dá `TypeError`; queryset/lista de instâncias convertem `.pk`; `None` mantém frota. `app/chamados/management/commands/disparar_perguntas_competencia.py:71`.

Escritores e leitores:

- `fio_mudo_alvos`: contador calcula nome/número/esperado/dono; vigia imprime e carimba número/esperado; pipeline compara ambos. `app/chamados/management/commands/disparar_perguntas_competencia.py:136`; `app/chamados/management/commands/vigiar_fio_morto.py:208`, `:233`, `:241`; `app/core/management/commands/pipeline_placar.py:98`, `:105`, `:135`.
- `nunca_bateu_sem_adesao`: calcula pares únicos retidos e interseção C5; vigia carimba ambos e diagnóstico integral; pipeline compara com dono CADASTRO. `app/chamados/management/commands/disparar_perguntas_competencia.py:38`, `:49`; `app/chamados/management/commands/vigiar_fio_morto.py:234`, `:241`, `:242`; `app/core/management/commands/pipeline_placar.py:136`.
- `c5_pessoas/c5_dias`: totais da C5 inteira, não do recorte; impressos nos dois comandos e persistidos apenas no bloco integral `nunca_bateu_sem_adesao`, fora de `censo/esperado`. `app/chamados/management/commands/disparar_perguntas_competencia.py:51`, `:205`; `app/chamados/management/commands/vigiar_fio_morto.py:210`, `:242`.
- `bin/fio_estado.py` compara automaticamente todas as chaves de `esperado`; mostra as novas chaves se divergentes, mas não devolve bloco integral C5 nem mostra nomes/valores novos quando iguais. Portanto “carimbo ESTADO” está entregue, “visualização explícita dos totais C5 no ESTADO” não está implementada neste leitor. `bin/fio_estado.py:125`, `:143`, `:191`.
- `rc --alarme` é negócio genérico para `cron_run`, `crons_quebrados` e pipeline, não interpretação “rc2 = FIO MUDO”. `bin/cron_run.sh:19`, `:22`, `:32`; `app/core/services/crons_quebrados.py:16`, `:46`; `app/core/management/commands/pipeline_placar.py:76`.
- `FABRICA-POR-EVENTO` nasce do tamanho de `deixou`, incluindo pergunta **ou Pauta DP**; `placar_code` raspa só esse prefixo, mas apresenta “perguntas que julgamento não fez”, sem mencionar Pauta. Diferença de apresentação/unidade. `app/chamados/management/commands/disparar_perguntas_competencia.py:228`, `:231`; `bin/placar_code.sh:390`, `:392`.
- `VIGIA FIO MUDO` nasce de `cfm.numero`; não encontrei raspador dessa linha fora de testes [nao verificado como ausência universal]. `app/chamados/management/commands/disparar_perguntas_competencia.py:210`.
- `celula_nunca_bateu` é causa por chamado/veredito, não prova de pertença C5; catálogo diz “conforme universo C5”, dono admin. `motivo_fora_da_fabrica` lê predicado → `retencao` → `medir/do_dia`; comando publica `causa:n:dono` → `placar_code`. `app/chamados/services/disputa_emissao.py:758`; `app/chamados/services/pergunta_no_app.py:35`, `:99`, `:167`, `:189`; `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:19`; `bin/placar_code.sh:319`.
- Tela C5 lê veredito e lavra dos desligados pela função única, formata assinatura e oferece adesão/ausência. Não deriva adesão novamente. `app/escala/services/cadastro_realidade.py:181`, `:191`, `:208`, `:690`; `app/escala/views.py:487`.

Não executei banco, logs, comandos de carimbo ou testes que criam arquivos [nao verificado por arquivo]. [nao verificado]

## 10/11 09:42 · sessao 0d9a248c194b · - · - · origem=Ronald (tmux)

Auditoria de testes, somente leitura: [nao verificado]

- **Executado: 16 testes em memória, verdes; 22 mutantes rejeitados** — 10 da matriz e 12 C5. Cada laço executa o verificador original antes da mutação, exige ocorrência única do trecho e captura `AssertionError` com a mensagem pretendida. Código: `app/chamados/tests/test_e4a2_matriz_memoria.py:24`, `:199`; `app/chamados/tests/test_e4a2_c5_memoria.py:30`, `:191`. Resultado desta execução: **[nao verificado por arquivo]**.
- **Executado: selo prazo verde**, anunciando `12 cron(s)` e `juizes_por_varredura=0`. Resultado desta execução: **[nao verificado por arquivo]**. A lista real de exceções está vazia em `bin/tests/test_papel_prazo_nao_deriva.sh:305`; a mordida sintética continua ativa em `:384` e `:391`, com falha explícita em `:405`. A lista distinta `JUIZES_DO_PRAZO` não pode estar vazia: `:253`.
- **Django: não certificado.** Há 11 mutantes em `app/chamados/tests/test_e4a2_nunca_bateu.py:34`, com controle verde em transação antes de cada mutação e captura da asserção pretendida em `:311`. Os 12 mutantes do placar têm o mesmo mecanismo em `app/core/tests/test_fio_mudo_placar.py:299`. Execução real: **[nao verificado]**.

Limites relevantes:

1. O teste intitulado `classes_a_b_rc_zero` **silencia explicitamente o ramo `motivos_que_o_evento_deixou`**. Portanto prova rc zero do FIO MUDO/retidos isolados, não o retorno da porta completa quando a célula também deixa pendência. `app/chamados/tests/test_e4a2_nunca_bateu.py:128`, `:135`, `:145`.
2. A prova de rc em memória injeta `deixou`, `_mudos` e `cnb` diretamente; não executa a seleção dos dois universos. `app/chamados/tests/test_e4a2_matriz_memoria.py:171`, `:177`, `:181`.
3. A “tranca” em memória significa apenas chamado simulado com `status='resolvido'`. A integração com `trancar_periodo` existe no teste Django, com controle positivo antes da tranca, mas não foi executada nesta auditoria. `app/chamados/tests/test_e4a2_matriz_memoria.py:156`; `app/chamados/tests/test_e4a2_nunca_bateu.py:163`.
4. Nenhuma lista atual de mutantes está vazia; porém seus laços não possuem guarda explícita contra esvaziamento. Esvaziar `MUTANTES` faria passar o teste de mutantes isolado. `app/chamados/tests/test_e4a2_matriz_memoria.py:199`; `app/chamados/tests/test_e4a2_c5_memoria.py:191`.
5. A proteção contra escrita não se limita a contagens: o teste Django captura SQL e executa callbacks `on_commit`; existem controles com UPDATE direto e adiado que mantêm as contagens. Execução: **[nao verificado]**. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:325`, `:338`, `:356`.

Sobre rc na volta 9b:

- O caso exclusivamente FIO MUDO exige `deixou=0`, nenhuma pergunta criada e rc zero. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132`.
- Retratada conserva rc dois e agora exige `deixou=1`; limbo também conserva dois. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:154`, `:160`.
- Os outros casos citados reforçam a origem do dois pela linha `FABRICA-POR-EVENTO`; não afrouxam o valor esperado. `app/chamados/tests/test_selo_emissor_fio_mudo.py:290`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:194`, `:398`, `:403`.
- As fórmulas gerais de rc foram alteradas para `alarme and celulas`; isso remove somente a participação do FIO MUDO, mantendo a exigência de dois quando há pendência da célula. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:202`; `app/chamados/tests/test_e4a2_matriz_memoria.py:195`.

Alvo 165, suíte inteira, integração Django e resultado de produção: **[nao verificado]**. [nao verificado]

## 10/11 09:18 · sessao a5cd4764c1cf · FIO-E0-V4-E4A-2 · construtor · origem=Code

Corrigi a caracterização para **FIO MUDO ⇒ rc 0**, mantendo contagem, impressão e nenhuma pergunta criada. O teste foi renomeado; a retratada agora exige explicitamente `deixou=1` antes de aceitar rc 2. [teste:132](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132), [retratada:157](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:157). [nao verificado]

Arquivos tocados, todos testes: [nao verificado]

- [test_jv4a_disparar_vigia_da_celula.py:132](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132): inversão e motivo da retratada.
- [test_selo_emissor_fio_mudo.py:291](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_selo_emissor_fio_mudo.py:291): rc 2 condicionado à célula deixar 1.
- [test_pergunta_do_dia_em_curso.py:194](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_pergunta_do_dia_em_curso.py:194): leitura explícita de pendência da célula.
- [test_e4a2_matriz_memoria.py:41](/home/ronald/wt-codex-fio-e0-v4-e4a-2-091339/app/chamados/tests/test_e4a2_matriz_memoria.py:41): mutantes adicionais e conferência da impressão.

A tabela usa caminhos relativos a `chamados/tests/`; linhas atuais:

| Asserção | Origem do 2 | Ação |
|---|---|---|
| `test_jv4a_disparar_vigia_da_celula.py:83` | Célula deixou 1 | Mantida |
| Mesmo arquivo`:90` | Limbo deixou 1 | Mantida |
| Mesmo arquivo`:129` | Falta de pauta deixou 1 | Mantida |
| Mesmo arquivo`:139` | FIO MUDO; célula deixou 0 | **Invertida para 0** |
| Mesmo arquivo`:158` — antiga :157 | Retratada; célula `furo` deixou 1 | Mantida; motivo explicitado |
| Mesmo arquivo`:167` | Limbo deixou 1, também FIO MUDO | Mantida |
| `test_pergunta_do_dia_em_curso.py:135` | Célula deixou 2 | Mantida |
| Mesmo arquivo`:163` | Células deixaram 2 ou 4 | Mantida |
| Mesmo arquivo`:195` | Célula deixou 2 | Mantida; motivo explicitado |
| Mesmo arquivo`:400` | Célula deixou quantidade positiva | Mantida; leitura fortalecida |
| Mesmo arquivo`:404` | Célula deixou quantidade positiva | Mantida; motivo explicitado |
| `test_selo_emissor_fio_mudo.py:292` | Célula deixou 1, também FIO MUDO | Mantida; motivo explicitado |
| `test_selo_disparo_nao_cobra_desligado.py:118` | Célula deixou quantidade positiva | Mantida |
| `test_e4a2_disparar_e_vigia.py:202` | Somente `alarme and celulas` | Mantida |
| Mesmo arquivo`:330` | Célula deixou 1; também existe FIO MUDO | Mantida |
| Mesmo arquivo`:117` | Erro de argumentos do parser | Mantida; não é retorno do vigia |
| `test_e4a2_matriz_memoria.py:195` | Somente `alarme and celulas` | Mantida |
| Mesmo arquivo`:64` | Erro de argumentos do parser | Mantida |

A antiga :157 prepara `_celula(True)`, cujo padrão é `veredito='furo'`; portanto, não isola FIO MUDO. Agora o teste exige `deixou=1`, além de FIO MUDO 1 e nenhuma pergunta adicional. A execução ORM dessa explicação permanece **[nao verificado]**. `test_jv4a_disparar_vigia_da_celula.py:38,147,154-158`. [nao verificado]

Os demais arquivos pedidos não exigem inversão: COLAB roda sem `--alarme`; F1 chama outro comando; nunca_bateu já exige retorno normal com e sem alarme. `test_colab_nao_vaza_fio_mudo.py:34,45,53`; `test_selo_f1_cego.py:75`; `test_e4a2_nunca_bateu.py:132-143`.

Quem escreve o rc é `disparar_perguntas_competencia.py:238-242`: único `SystemExit(2)`, condicionado a `alarme and deixou`. Quem lê inclui os testes acima e `pipeline_placar.py:76-80`, que interpreta exit 2 como alarme de negócio. O placar lê FIO MUDO separadamente pelo ESTADO em `pipeline_placar.py:85-107,135`. O contrato informado de `bin/cron_run.sh:11`, aceitando 0 e 2 como elo feito, está **[nao verificado na copia]**.

Execução local: **16 testes em memória passaram**, incluindo rejeição dos mutantes; isso não certifica a integração. Verificadores em `test_e4a2_matriz_memoria.py:171-208` e `test_e4a2_c5_memoria.py`. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4a2_matriz_memoria chamados.tests.test_e4a2_c5_memoria -v

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria fio_mudo_volta_a_rc_dois
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria celula_perde_rc_dois
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria fio_mudo_impresso_como_zero
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria vigia_escreve_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria vigia_escreve_questionario
```

Não rodaram os módulos Django, o alvo 165 nem a suíte inteira: integração **[nao verificado]**. Ruff foi tentado com `PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache --config ruff.toml` nos quatro arquivos, mas o módulo está ausente; Ruff limpo **[nao verificado]**. [nao verificado]

## 10/11 08:51 · sessao 46e2e0dfb3e1 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Cura aplicada: normalização única para pks, rc 0 para FIO MUDO/retidos e exclusão da pergunta viva antes dos dois contadores. O rc 2 do caminho da célula permanece. [disparar:72](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/disparar_perguntas_competencia.py:72), [disparar:95](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/disparar_perguntas_competencia.py:95), [disparar:238](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/disparar_perguntas_competencia.py:238). [nao verificado]

Arquivos tocados:

- [disparar_perguntas_competencia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/disparar_perguntas_competencia.py:24): cura e textos.
- [test_e4a2_matriz_memoria.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:1): novo; matriz e oito mutantes.
- [test_e4a2_nunca_bateu.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_nunca_bateu.py:121): provas ORM e mutante escritor.
- [test_e4a2_disparar_e_vigia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_disparar_e_vigia.py:202): contrato de retorno atualizado.
- [test_fio_mudo_placar.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/core/tests/test_fio_mudo_placar.py:295): filtro por `empresa_id`.

As provas da matriz ficaram assim:

| Linha | Prova |
|---|---|
| (a) Retido; C5 ausente produz `(1,0)`; nenhuma escrita | [teste:126](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:126) | [nao verificado]
| (b) Alvo; nenhuma escrita; placar vermelho | [teste:138](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:138), [teste:160](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:160) | [nao verificado]
| (c) Pergunta viva exclui ambos, inclusive `nunca_bateu` | [teste:143](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:143) | [nao verificado]
| (d) Encerrado fica fora; teste da tranca real entregue | [memória:152](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:152), [ORM:163](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_nunca_bateu.py:163). Execução ORM: [nao verificado] | [nao verificado]

Os testes de fronteira, retorno e precedência da pergunta viva falharam antes das respectivas curas. Após as curas, os **16 testes sem banco passaram**, incluindo os oito mutantes com controle verde e captura da asserção pretendida. [testes:121](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:121), [testes:167](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:167), [mutantes:191](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:191). [nao verificado]

Contrato de saída:

1. Fora de `JUIZES_POR_VARREDURA` e parser rejeitando `--apply`: [teste:51](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:51). [nao verificado]
2. `fio_mudo_alvos` no ESTADO, comparado pelo placar: [teste:160](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:160). [nao verificado]
3. Retidos publicados/comparados e fronteira passando pks: [publicação:138](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_c5_memoria.py:138), [fronteira:121](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:121). [nao verificado]
4. Selo executado: **OK**, com exceções vazias. [lista:305](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/bin/tests/test_papel_prazo_nao_deriva.sh:305), [saída:416](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/bin/tests/test_papel_prazo_nao_deriva.sh:416).
5. Alvo de módulos e suíte inteira: **[nao verificado]**. [nao verificado]
6. Produção: **[nao verificado]**; ALVOS 0 é o dado fornecido no briefing.

Quem escreve e quem lê:

| Campo | Escritor → leitor; significado |
|---|---|
| `empresas` | `vigiar_fio_morto.py:160` fornece objetos; `disparar_perguntas_competencia.py:72` normaliza uma vez → filtro `:91` e C5 em `cadastro_realidade.py:177`. São **pks de Empresa**. |
| `fio_mudo_alvos` | `disparar_perguntas_competencia.py:136` calcula → `vigiar_fio_morto.py:233,241` carimba → `pipeline_placar.py:85,135` compara. Unidade: alvos `(colab,dia,motivo)`. |
| `nunca_bateu_sem_adesao` | `disparar_perguntas_competencia.py:49` calcula pares/esperado C5 → `vigiar_fio_morto.py:234,241` → `pipeline_placar.py:136`. Unidade: pares `(colab,dia)`. |
| Totais C5, diagnóstico e universo | `disparar_perguntas_competencia.py:51` → relatório `:204` e bloco integral em `vigiar_fio_morto.py:242`; totais informativos. |
| Pergunta viva | Escritor existente em `fabrica_da_celula.py:160` → leitura em `disparar_perguntas_competencia.py:96`; encerra a seleção. |

A divergência objeto/pk estava na entrada da C5 e foi corrigida. O dia contado continua sendo `celula.data`; `hoje` do carimbo é civil local. O placar compara a fotografia desse dia, enquanto `bin/fio_estado.py:125` compara os esperados publicados. O raspador de `bin/placar_code.sh:390` continua lendo exclusivamente `FABRICA-POR-EVENTO`. [seleção:86](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/disparar_perguntas_competencia.py:86), [carimbo:230](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/management/commands/vigiar_fio_morto.py:230), [placar:94](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/core/management/commands/pipeline_placar.py:94).

Comandos executáveis das provas e de cada mutante, definidos em [matriz:24](/home/ronald/wt-codex-fio-e0-v4-e4a-2-083750/app/chamados/tests/test_e4a2_matriz_memoria.py:24):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4a2_c5_memoria \
  chamados.tests.test_e4a2_matriz_memoria -v

PYTHONDONTWRITEBYTECODE=1 bash ../bin/tests/test_papel_prazo_nao_deriva.sh

for mutante in \
  fronteira_repassa_empresa contador_nao_compara_c5 \
  vigia_escreve_pergunta vigia_escreve_questionario \
  placar_ignora_fio_mudo fio_mudo_volta_a_rc_dois \
  pergunta_viva_vira_alvo chamado_encerrado_vira_alvo
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m \
    chamados.tests.test_e4a2_matriz_memoria "$mutante" || exit
done
```

Para a sessão, a lista de módulos tocados/afetados é:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_o261_vigia_fio \
  chamados.tests.test_snc2_toque_em_lote \
  chamados.tests.test_e4a2_nunca_bateu \
  chamados.tests.test_e4a2_disparar_e_vigia \
  chamados.tests.test_e4a2_c5_memoria \
  chamados.tests.test_e4a2_matriz_memoria \
  chamados.tests.test_selo_emissor_fio_mudo \
  core.tests.test_fio_mudo_placar \
  --settings=config.settings.ci -v 2
```

Essa execução ORM, a suíte completa e a integração real da tranca permanecem **[nao verificado]**. Ruff também não executou: `No module named ruff`; Django/freezegun estão ausentes neste Python **[nao verificado por arquivo]**. A certificação permanece com a sessão e o opus final. [nao verificado]

## 10/11 08:50 · sessao a8a93123d0a6 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · verificador · origem=Code

**A cura é parcial.** A gramática foi unificada e o mutante de espaço tem mordida; permanecem perda do diagnóstico da recusa, uma origem real omitida pelo censo e incompatibilidade com chaves antigas. Há também um contraexemplo à equivalência geral entre emissão e guarda. (`app/pautas/marcadores.py:31`, `app/pautas/tests/mutar_prefixos_reservados.py:132`, `app/core/esteira_vigia.py:936`, `app/core/esteira_vigia.py:1424`, `app/core/management/commands/alarme_esteira.py:62`)

1. **Achados PROCEDENTES da triagem, um por um**

   **A — duas gramáticas: curado quanto à fonte única.** `CHAVE_ESTEIRA` define a linguagem; `chave_esteira_valida` usa essa expressão; a reserva deriva dela. Comando e leitor usam o mesmo predicado, e a guarda humana consome o padrão derivado. Assim, `a b` e `:a` são recusadas antes de consultar, escrever ou fechar. (`app/pautas/marcadores.py:31`, `app/pautas/marcadores.py:34`, `app/pautas/marcadores.py:40`, `app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:34`, `app/pautas/services.py:47`)

   **A — preservação dos emissores reais: não demonstrada integralmente.** A fila admite diretório com espaço, e emissores concatenam seu basename sem validação; portanto o código pode formar `fatia:a b`, agora recusada. Os basenames do censo são amostras sintéticas sem whitespace. Existência desse caso em produção: **[nao verificado]**. (`app/core/esteira_vigia.py:119`, `app/core/esteira_vigia.py:121`, `app/core/esteira_vigia.py:412`, `app/pautas/tests/test_prefixos_reservados_esteira.py:135`)

   **B — ausência de teste de equivalência/mutante: curado para o caso apontado, incompleto como propriedade geral.** O teste executa o corpo do comando, captura o texto e aplica a guarda real; o mutante libera espaço no emissor. Reexecutado nesta sessão: **11 falhas, zero erros**. Porém a seleção de entradas não cobre todo valor aceito, conforme o contraexemplo de tamanho abaixo. (`app/pautas/tests/test_prefixos_reservados.py:288`, `app/pautas/tests/test_prefixos_reservados.py:297`, `app/pautas/tests/mutar_prefixos_reservados.py:132`, `app/pautas/tests/mutar_prefixos_reservados.py:235`)

2. **ESTEIRA: quem escreve, quem lê e compatibilidade**

   As origens das chaves são:

   | Origem | Chaves e encaminhamento |
   |---|---|
   | `decidir` | Famílias por fatia/basename e alarmes fixos; acrescenta ações e registra a chave na memória. (`app/core/esteira_vigia.py:402`, `app/core/esteira_vigia.py:412`, `app/core/esteira_vigia.py:461`, `app/core/esteira_vigia.py:557`, `app/core/esteira_vigia.py:592`) |
   | Emissores diretos | `relance:…`, `quarentena:zumbido:…`, `quarentena:prazo`, `quarentena:deposito`, `auto_revert`, `quarentena`, `arvore:vermelho`. (`app/core/esteira_vigia.py:992`, `app/core/esteira_vigia.py:1212`, `app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1277`, `app/core/esteira_vigia.py:1307`, `app/core/esteira_vigia.py:1364`, `app/core/esteira_vigia.py:1371`) |
   | `autocura` | `arvore:import` e `arvore:json`, incorporadas às ações de `passada`. (`app/core/esteira_vigia.py:1424`, `app/core/esteira_vigia.py:1427`, `app/core/esteira_vigia.py:1542`) |
   | Transporte e escrita | `passada` envia motivo/resolver; `_pauta` chama `alarme_esteira`; o comando escreve `ESTEIRA <chave>: <texto>` por `services.escrever`. (`app/core/esteira_vigia.py:1603`, `app/core/esteira_vigia.py:1608`, `app/core/esteira_vigia.py:936`, `app/core/management/commands/alarme_esteira.py:60`) |

   **Leitores:** `handle` escolhe motivo/resolver e valida; `pauta_viva` valida e procura a primeira viva por prefixo textual; resolver fecha essa pauta; a guarda humana reconhece a reserva derivada. (`app/core/management/commands/alarme_esteira.py:30`, `app/core/management/commands/alarme_esteira.py:18`, `app/core/management/commands/alarme_esteira.py:41`, `app/pautas/services.py:47`)

   **Toda chave formável passa? Não.** Além do basename com espaço, o próprio teste demonstra que um caminho de teste com espaço é preservado em `sem-commit:<arquivo>` e produz chave recusada. O censo dos arquivos presentes não prova restrição dos futuros insumos externos. (`app/pautas/tests/test_prefixos_reservados_esteira.py:180`, `app/pautas/tests/test_prefixos_reservados_esteira.py:194`, `app/pautas/tests/test_prefixos_reservados_esteira.py:135`)

   **Algo anteriormente aceito passa a ser recusado? Sim, no contrato do comando:** chaves com whitespace ou `:` inicial antes aceitas pelo fluxo fornecido agora encontram a rejeição explícita. Perda efetiva de alarme em produção: **[nao verificado]**. (`app/core/management/commands/alarme_esteira.py:34`; comportamento anterior: `diff_v45_delta.patch` fornecido, **[nao verificado]** na árvore Git)

   **A recusa perde seu diagnóstico no caminho real.** O comando escreve stderr e retorna normalmente; `_pauta` captura stdout/stderr e descarta o resultado. Os relatos posteriores registram a ação presumida, sem confirmar que uma Pauta nasceu. Há rastro da tentativa em alguns caminhos, mas nenhum rastro próprio da rejeição nesse transporte. (`app/core/management/commands/alarme_esteira.py:35`, `app/core/esteira_vigia.py:936`, `app/core/esteira_vigia.py:1215`, `app/core/esteira_vigia.py:1604`)

   Isso pode persistir: `decidir.alarme` registra a chave **antes** do envio; um alarme sem repetição periódica pode permanecer deduplicado mesmo sem Pauta criada. (`app/core/esteira_vigia.py:403`, `app/core/esteira_vigia.py:406`, `app/core/esteira_vigia.py:412`)

   **Contraexemplo adicional:** uma chave de **472 letras** passa na gramática. Com os oito caracteres de `ESTEIRA `, o corte `[:480]` remove o delimitador `:`. O texto resultante passa pela guarda e não casa com o prefixo completo procurado pelo leitor. Reproduzi em memória; o teste explícito de comprimento usa apenas 460 letras. Ocorrência em produção: **[nao verificado]**. (`app/pautas/marcadores.py:31`, `app/core/management/commands/alarme_esteira.py:62`, `app/core/management/commands/alarme_esteira.py:18`, `app/pautas/services.py:47`, `app/pautas/tests/test_prefixos_reservados.py:274`)

3. **Consumidores de `sem-commit:<arquivo>` e chave antiga** [nao verificado]

   O rótulo agrupa os testes por dono e é propagado para `quem`, motivo `dono=…`, chave `quarentena:zumbido:…`, prosa da Pauta, relato e retorno. (`app/core/esteira_vigia.py:1174`, `app/core/esteira_vigia.py:1205`, `app/core/esteira_vigia.py:1211`, `app/core/esteira_vigia.py:1212`, `app/core/esteira_vigia.py:1215`, `app/core/esteira_vigia.py:1219`)

   - **JSON:** `quarentenar` grava `quem` e motivo como valores; leitor desse JSON não foi identificado na cópia. Leitores externos: **[nao verificado]**. (`app/core/esteira_vigia.py:1243`, `app/core/esteira_vigia.py:1245`)
   - **Lista:** o motivo é limitado a 120 caracteres; o parser interpreta ID/prazo e conserva o restante como texto. Runner e integrador consultam IDs, sem casar o formato do dono. (`app/core/esteira_vigia.py:1098`, `app/core/quarentena_lista.py:19`, `app/core/quarentena_lista.py:21`, `app/core/runner_quarentena.py:36`, `app/core/integrador_lote.py:780`)
   - **Relato:** recebe e insere a linha sem interpretar o rótulo. (`app/core/esteira_vigia.py:1215`, `app/core/esteira_vigia.py:895`, `app/core/esteira_vigia.py:903`)

   Não identifiquei consumidor operacional que case `sem commit (`; a forma antiga permanece como mutante. Ausência de consumidores fora da cópia: **[nao verificado]**. (`app/pautas/tests/mutar_prefixos_reservados.py:139`)

   **Se existir uma Pauta antiga, ela fica inacessível por esses dois caminhos:** a chave nova procura outro prefixo; `--resolver` com a chave antiga é recusado pelo espaço antes da consulta. Sua existência em produção é **[nao verificado]**. (`app/core/esteira_vigia.py:1174`, `app/core/esteira_vigia.py:1212`, `app/core/management/commands/alarme_esteira.py:18`, `app/core/management/commands/alarme_esteira.py:34`)

   Não atribuir isso a uma nova quebra de fechamento automático do zumbido: no fluxo examinado, retirar testes da quarentena já não resolve a Pauta correspondente. (`app/core/esteira_vigia.py:1260`, `app/core/esteira_vigia.py:1274`)

4. **O censo AST é fechado? Morde código real?**

   **Para nova chamada direta `_pauta([...])`, sim:** exige assinatura/lista/opção conhecidas e compara função, opção, expressão e multiplicidade por `Counter`. Nova chamada direta, mesmo válida ou duplicada, exige atualização explícita. (`app/pautas/tests/test_prefixos_reservados_esteira.py:61`, `app/pautas/tests/test_prefixos_reservados_esteira.py:95`, `app/pautas/tests/test_prefixos_reservados_esteira.py:128`, `app/pautas/tests/test_prefixos_reservados_esteira.py:231`)

   **Para todas as origens reais de chaves, não.** `autocura` produz alarmes reais, mas as entradas usadas para testar `a[1]` de `passada` vêm somente de `decidir`. (`app/core/esteira_vigia.py:1424`, `app/core/esteira_vigia.py:1427`, `app/core/esteira_vigia.py:1542`, `app/pautas/tests/test_prefixos_reservados_esteira.py:132`, `app/pautas/tests/test_prefixos_reservados_esteira.py:147`)

   Confirmei a fuga em memória: trocar `arvore:import` por **`arvore import`** manteve `rodar_porta` verde: **16 testes, zero falhas e zero erros**. Portanto uma chave inválida pode surgir numa origem real sem o selo acusar. (`app/core/esteira_vigia.py:1424`, `app/pautas/tests/mutar_prefixos_reservados.py:30`, `app/pautas/tests/test_prefixos_reservados_esteira.py:147`)

   **São corpos reais com limites substituídos:** comando/leitor são compilados da AST real; catálogo e guarda são reais. Entretanto imports de topo são removidos, nomes são injetados, e BaseCommand/ORM/persistência são duplos. As expressões de `decidir` são avaliadas com entradas sintéticas, sem executar seus ramos. Isso prova regras e argumentos, não ligação real dos imports, CLI ou banco. (`app/pautas/tests/test_prefixos_reservados.py:28`, `app/pautas/tests/test_prefixos_reservados.py:233`, `app/pautas/tests/test_prefixos_reservados.py:237`, `app/pautas/tests/test_prefixos_reservados.py:259`, `app/pautas/tests/test_prefixos_reservados_esteira.py:135`)

   **O mutante de espaço pedido morde**, mas sua morte não elimina a fuga de `autocura`. (`app/pautas/tests/mutar_prefixos_reservados.py:132`, `app/pautas/tests/test_prefixos_reservados.py:307`, `app/pautas/tests/test_prefixos_reservados_esteira.py:147`)

5. **Mudança de `test_selo_zumbido_quarentena`**

   **Preserva a mordida declarada:** executa `passada(dry=True, agora=T0)` real e exige `curar_zumbido(T0, True)` somente para árvore vermelha. (`app/core/tests/test_selo_zumbido_quarentena.py:135`, `app/core/tests/test_selo_zumbido_quarentena.py:138`, `app/core/esteira_vigia.py:1552`)

   **Reduz a execução da costura do host:** `_ultima_atividade`, leitura de EVENTOS/SLOTS e existência de SLOTS agora são substituídas. A fila já é vazia e a própria cura continua mockada nessa classe; portanto esse teste não certifica tais leituras nem seus efeitos. (`app/core/tests/test_selo_zumbido_quarentena.py:125`, `app/core/tests/test_selo_zumbido_quarentena.py:126`, `app/core/tests/test_selo_zumbido_quarentena.py:134`, `app/core/esteira_vigia.py:704`, `app/core/esteira_vigia.py:1529`, `app/core/esteira_vigia.py:1532`)

   `CuraTest` continua executando separadamente a cura real com bordas substituídas. A troca da classe-base está explícita; equivalência completa das proteções de infraestrutura de `SimpleTestCase` e `unittest.TestCase`: **[nao verificado]**. (`app/core/tests/test_selo_zumbido_quarentena.py:12`, `app/core/tests/test_selo_zumbido_quarentena.py:81`, `app/core/tests/test_selo_zumbido_quarentena.py:94`)

6. **Classes que já reprovaram obra**

   - **Segundo juiz:** não identificado nos caminhos alterados. O juiz de entrega ao DP continua registrado e chamado pelo escritor após a trava; a guarda humana decide outra pergunta. (`app/core/juizes.py:293`, `app/chamados/services/fabrica_da_celula.py:308`, `app/chamados/services/fabrica_da_celula.py:355`, `app/chamados/services/fabrica_da_celula.py:358`, `app/pautas/services.py:40`)
   - **Segundo escritor fora da porta:** não identificado nesses caminhos; os emissores seguem chamando `services.escrever`. (`app/chamados/services/fabrica_da_celula.py:360`, `app/core/management/commands/alarme_esteira.py:60`, `app/pautas/services.py:197`)
   - **Relógio fora da borda:** não introduzido nos predicados novos; a data é lida no comando. Ausência universal na obra: **[nao verificado]**. (`app/pautas/marcadores.py:34`, `app/pautas/services.py:40`, `app/core/management/commands/alarme_esteira.py:56`)
   - **Derivação paralela:** curada para a gramática ESTEIRA nos três consumidores examinados. (`app/pautas/marcadores.py:40`, `app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:34`, `app/pautas/services.py:47`)
   - **Selo sem mordida:** há mordida no defeito de espaço, porém o fechamento alegado é incompleto por omitir `autocura`. (`app/pautas/tests/mutar_prefixos_reservados.py:132`, `app/core/esteira_vigia.py:1424`, `app/pautas/tests/test_prefixos_reservados_esteira.py:147`)
   - **ESMERIL-SEMANTICA: presente e preexistente.** O mesmo `texto` guarda prosa e identidade; o esmeril extrai a ação dele. Em ESTEIRA, isso também permite que a busca por `a` case uma Pauta de `a:b`, podendo suprimir abertura ou fechar outra chave. (`app/pautas/models.py:89`, `app/ponto/services/pautas_do_esmeril.py:50`, `app/ponto/services/pautas_do_esmeril.py:67`, `app/pautas/marcadores.py:31`, `app/core/management/commands/alarme_esteira.py:18`, `app/core/management/commands/alarme_esteira.py:41`, `app/core/management/commands/alarme_esteira.py:44`)

Reexecução local: **41 testes, resultado OK com um skip explícito de integração Django**; essa execução não certifica HTTP/banco, produção ou a árvore futura do merge. (`app/pautas/tests/test_prefixos_reservados.py:358`; produção e merge: **[nao verificado]**)

## 10/11 08:48 · sessao 9893d15b7198 · - · - · origem=Ronald (tmux)

Q4: **fechado para novas chamadas diretas `_pauta([...])`, mas incompleto para todas as origens reais de chaves**. O varredor reconhece `ast.Name('_pauta')`, exige lista/uma opção e compara `Counter` por função, opção, expressão e multiplicidade; nova chamada direta, inclusive duplicada/válida, fica vermelha. Referências: `app/pautas/tests/test_prefixos_reservados_esteira.py:61`, `:95`, `:128`, `:231`.

Existe origem real omitida: `autocura` emite `arvore:import` e `arvore:json`, acrescentadas às ações em `passada`; o censo de `a[1]` só alimenta valores gerados de `decidir`. Referências: `app/core/esteira_vigia.py:1424`, `:1427`, `:1542`; `app/pautas/tests/test_prefixos_reservados_esteira.py:132`, `:147`.

Comprovei em memória, sem criar arquivos, substituindo apenas `'arvore:import'` por `'arvore import'`: `rodar_porta` continuou com **16 testes, zero falhas, zero erros**, e o censo continuou com 2515 valores. A deficiência decorre dos caminhos acima; execução adicional sem artefato persistido: [nao verificado] como evidência arquivada. Esse mutante não consta da lista de mutantes, que cobre alterações em `_pauta`/`decidir`: `app/pautas/tests/mutar_prefixos_reservados.py:146`.

O mutante pedido **morde**: `esteira_emissor_aceita_chave_com_espaco` altera o corpo real do comando e produziu **11 falhas, zero erros**, repetindo o anexo. Referências da construção/aplicação/critério: `app/pautas/tests/mutar_prefixos_reservados.py:132`, `:30`, `:235`; asserções: `app/pautas/tests/test_prefixos_reservados.py:297`, `:307`, `:319`. Execução adicional sem artefato persistido: [nao verificado] como evidência arquivada.

São corpos reais, com limites substituídos: comando/leitor compilados da AST real, catálogo real, `curar_zumbido`/`quarentenar` reais; ORM, subprocessos, persistência e BaseCommand são duplos. Referências: `app/pautas/tests/test_prefixos_reservados.py:233`, `:234`, `:237`, `:244`, `:259`; `app/pautas/tests/test_prefixos_reservados_esteira.py:21`, `:196`, `:223`. As emissões de `decidir` são expressões reais avaliadas com valores sintéticos, sem executar seus ramos: `app/pautas/tests/test_prefixos_reservados_esteira.py:135`. Imports de topo do alarme são removidos e nomes são injetados; sua ligação real não fica certificada: `app/pautas/tests/test_prefixos_reservados.py:234`, `:260`.

Q5: **a mordida declarada da costura permanece**: executa `passada(dry=True, agora=T0)` real e exige `curar_zumbido(T0, True)` somente com árvore vermelha. Referências: `app/core/tests/test_selo_zumbido_quarentena.py:124`, `:138`; condição real: `app/core/esteira_vigia.py:1552`.

Perde execução incidental de `_ultima_atividade` e das leituras reais de EVENTOS/SLOTS/`exists(SLOTS)`, agora substituídas; logo não prova toda a costura do host. Referências: `app/core/tests/test_selo_zumbido_quarentena.py:126`; `app/core/esteira_vigia.py:704`, `:1529`, `:1532`. A fila já é vazia e a cura permanece mockada nesse teste: `app/core/tests/test_selo_zumbido_quarentena.py:125`, `:134`. `CuraTest` continua exercitando separadamente `curar_zumbido` real com limites substituídos: `app/core/tests/test_selo_zumbido_quarentena.py:81`, `:94`.

A mudança para unittest está explícita e mantém as quatro classes coletáveis: `app/core/tests/test_selo_zumbido_quarentena.py:12`, `:25`, `:60`, `:78`, `:121`. A proteção adicional contra acessos acidentais a banco antes fornecida pelo `SimpleTestCase` não é demonstrável pela cópia da dependência: [nao verificado]. Reexecutei os 11 testes core, todos verdes; não arquivei saída por restrição de somente leitura: [nao verificado] como evidência persistida.

## 10/11 08:47 · sessao 439b841d9483 · O169-FECHADA · verificador · origem=Code

1. **A: nenhuma via conhecida excluída continua como “desconhecida”.** O mapa cobre todas as vias de `VIAS_ENCERRAMENTO` diferentes de RECONCILIAR; `''` e `None` têm rótulos próprios. RECONCILIAR pertence ao universo contado. ([motor.py:93](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/catalogo/motor.py:93), [cluster_espurio.py:110](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:110), [cluster_espurio.py:126](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:126))

| Via | Comparação com o escritor real |
|---|---|
| ENCERRAR | O gate documenta desligamento **e ajuste retroativo**, e grava a via correspondente. A omissão apontada foi corrigida. ([models.py:1605](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:1605), [models.py:1619](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:1619)) |
| CANCELAR | O texto corresponde ao descarte de chamado vivo descrito pelo gate. Porém, o backfill também grava essa via apenas por encontrar status cancelado com via vazia; não comprova o motivo histórico. ([models.py:773](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:773), [0039_a3_via_backfill.py:25](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/migrations/0039_a3_via_backfill.py:25)) |
| SUPERAR | Corresponde ao escritor remanescente: duplicata cancelada, irmão único do mesmo colaborador/data, gravação de `superado` e sua FK. O gate atual foi removido. ([0039_a3_via_backfill.py:15](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/migrations/0039_a3_via_backfill.py:15), [models.py:794](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:794)) |
| VIA_LEGADO | Corresponde aos terminais que permaneceram sem via após as derivações do backfill. ([0039_a3_via_backfill.py:28](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/migrations/0039_a3_via_backfill.py:28)) |
| TRANSICAO_HUMANA | `transicionar → _aplicar → _gravar_estado` grava essa via. O backfill também atribui `admin` por respondente não sistêmico; a via não prova passagem histórica por `transicionar`. ([models.py:199](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:199), [models.py:450](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:450), [0039_a3_via_backfill.py:26](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/migrations/0039_a3_via_backfill.py:26)) |
| Competência trancada | O gate específico grava `VIA_COMPETENCIA_TRANCADA` pelo núcleo. ([models.py:1622](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:1622), [models.py:1639](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/models.py:1639)) |

**Ressalva semântica:** ENCERRAR também atende outros usos globais, como substituição de aceite de escala; portanto seu rótulo não esgota todos os chamadores do gate. Isso não demonstra registro de cluster mal rotulado no dado fornecido. ([vinculo.py:384](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/colaboradores/services/vinculo.py:384); ocorrência no cluster: **[nao verificado]**)

**JSON:** `None` e `''` não causam exceção no `json.dumps` atual. Contudo, `None` vira chave textual `"null"`: uma via desconhecida literal `'null'` colide com ela na serialização, inclusive nos rótulos. Reproduzido em memória; ocorrência dessa via e consumidor externo afetado: **[nao verificado]**. ([cluster_espurio.py:142](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:142), [cluster_espurio.py:145](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:145), [detectar_cluster_espurio.py:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:63))

2. **B: os três mutantes morrem pelas asserções pretendidas.** O carregador compila a fonte real; serviço/comando são carregados inteiros, com ORM e ambiente substituídos por dublês. ([test_o169_cluster_encerrado_sem_lastro.py:31](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:31), [test_o169_cluster_encerrado_sem_lastro.py:104](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:104))

| Mutante | Falha observada |
|---|---|
| FK cruzada | `1 != 0` e `0 != 1` na asserção de `numero`, usando assinaturas opostas nas células cruzada/correta. ([teste:299](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:299), [teste:302](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:302)) |
| Consulta por par | `9 != 3` na contagem de consultas; a asserção anterior de `numero` passa. Consultas vazias também entram na contagem. ([teste:83](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:83), [teste:315](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:315), [teste:316](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:316)) |
| Rótulos trocados | Falha na igualdade exata de `excluidos`, mesmo sem ocorrências. ([teste:233](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233), [teste:247](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:247)) |

Na execução local: **61 testes OK, incluindo os 35 mutantes; nenhum listado sobreviveu**. O harness exige alvo único, rejeita erros de infraestrutura e exige falha de asserção. Isso não certifica SQL real nem produção. ([test_o169_cluster_encerrado_mutantes.py:135](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:135), [mutantes:141](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:141), [contrato:7](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:7))

3. **Quem escreve e quem lê:**

| Campo | Escritor em memória | Leitores encontrados |
|---|---|---|
| `excluidos` | Catálogo de motivos, inclusive sem ocorrências. ([cluster:126](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:126)) | JSON integral e contrato de rótulo exato. ([comando:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:63), [teste:247](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:247)) |
| `fora_do_universo` | Quantidade observada por via, dentro do filtro de módulo/status/colaborador. ([cluster:106](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:106)) | JSON e contratos de contagem. ([comando:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:63), [teste:222](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:222)) |
| `numero` | Soma **por chamado**, quando o par tem assinatura viva. ([cluster:125](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:125)) | JSON, decisão/mensagem do alarme e contratos. ([comando:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:63), [comando:70](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:70), [teste:348](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:348)) |
| `esperado` | Constante zero. ([cluster:146](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:146)) | JSON e comparação `numero != esperado`; teste injeta `esperado=1` para provar a comparação. ([comando:70](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:70), [teste:416](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:416)) |

Não identifiquei leitor atribuindo outro significado a esses campos. Atenção: `fora_do_universo` **não é censo de todos os terminais**: `ENCERRADOS` contém somente resolvido/fechado; status cancelado/superado ficam fora também dessa seleção. ([motor.py:30](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/chamados/catalogo/motor.py:30), [cluster:106](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:106))

4. **Regressão numérica no delta: não.** O delta fornecido altera rótulos/importação e testes; preserva seleção, dia, guarda de FK, agrupamento, soma e `esperado=0`. Portanto não muda quem entra em `numero`. Impacto quantitativo em produção: **[nao verificado]**. ([cluster_espurio.py:106](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:106), [cluster_espurio.py:125](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:125), [cluster_espurio.py:146](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:146))

5. **Classes anteriores, no diff da obra:**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não identificado: usa `dia_do_fato` e o predicado existente `_leitura_da_celula`. ([cluster:102](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:102), [cluster:113](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:113), [cluster:125](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:125)) |
| Segundo escritor fora da porta | Não identificado: contador só consulta; APPLY usa `reconciliar_colab`; alarme desabilita APPLY. ([cluster:109](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:109), [comando:39](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:39), [comando:51](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:51)) |
| Relógio fora da borda | Não introduzido: leitura permanece no `handle`; contador não depende de hoje. ([comando:40](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:40), [cluster:88](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:88)) |
| Derivação paralela | Não identificada: assinatura vem da leitura existente da célula, sem remontar grade. ([cluster:15](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:15), [cluster:125](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:125)) |
| Selo sem mordida | Os três apontados agora mordem; nenhum dos 35 listados sobreviveu. RED histórico anterior à cura: **[nao verificado]**. ([mutantes:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:63), [mutantes:141](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:141)) |
| Campo com dois significados | Não identificado nos leitores encontrados; permanece a ressalva de colisão das chaves no JSON descrita em A. ([cluster:146](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/services/cluster_espurio.py:146), [comando:63](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:63), [comando:70](/home/ronald/codex_ws/778aa33deaad9f8b780689d1d3fe5994ae240581/app/ponto/management/commands/detectar_cluster_espurio.py:70)) |

## 10/11 08:47 · sessao 630ba9818bbe · - · - · origem=Ronald (tmux)

- **Triagem A curada nos três pontos:** gramática definida em `app/pautas/marcadores.py:31`, predicado em `:34`, reserva derivada em `:40`; comando e leitor chamam esse predicado em `app/core/management/commands/alarme_esteira.py:34` e `:15`; guarda usa o padrão do catálogo em `app/pautas/services.py:47`.

- **Triagem B ganhou mordida concreta:** teste executa comando e aplica guarda ao texto capturado em `app/pautas/tests/test_prefixos_reservados.py:288` e `:297`; mutante permite espaço no emissor em `app/pautas/tests/mutar_prefixos_reservados.py:133`; morte exige falha sem erro de infraestrutura em `:237`. Execução nesta exploração: [nao verificado].

- **Segundo juiz/escritor não aparece nos caminhos examinados:** pergunta de cadastro permanece registrada em `app/core/juizes.py:293`; juiz continua em `app/chamados/services/fabrica_da_celula.py:308`; escritor segue trava → juiz → `pautas.services.escrever` em `:355`, `:358`, `:360`. Guarda de texto decide outra pergunta em `app/pautas/services.py:40`. Selo antigo foi adaptado para permitir declaração no catálogo e barrar uso operacional fora dos sítios em `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:160` e `:279`.

- **Relógio:** predicados novos não leem relógio, conforme seus corpos em `app/pautas/marcadores.py:34` e `app/pautas/services.py:40`; `timezone.localdate()` está na borda do comando em `app/core/management/commands/alarme_esteira.py:56`, preexistente conforme diff fornecido. Ausência universal de leitura fora da borda: [nao verificado].

- **ESMERIL-SEMANTICA permanece:** `Pauta.texto` é o campo de prosa em `app/pautas/models.py:89`; escritor embute ação em `app/ponto/services/pautas_do_esmeril.py:50`; leitor usa esse mesmo texto como identidade e extrai a ação em `:66` e `:67`.

- **Problema adicional de identidade textual permanece em ESTEIRA:** gramática aceita `a` e `a:b` em `app/pautas/marcadores.py:31`; leitor para `a` busca prefixo `ESTEIRA a:` e também casa `ESTEIRA a:b: …` em `app/core/management/commands/alarme_esteira.py:18`. Portanto pode suprimir abertura em `:44`–`:47` ou fechar pauta de chave mais longa em `:39`–`:41`. O mecanismo é preexistente no leitor dado; ocorrência em produção: [nao verificado].

## 10/11 08:47 · sessao 5ba882585d0a · - · - · origem=Ronald (tmux)

Achados para as questões 2–3:

- **Gramática única está implementada:** `[^\s:]\S*`, sem limite de tamanho; predicado por `re.fullmatch`; reserva derivada da mesma expressão. Comando e leitor chamam o predicado. A guarda humana usa `PADROES_RESERVADOS`. (`app/pautas/marcadores.py:31`, `app/pautas/marcadores.py:34`, `app/pautas/marcadores.py:40`, `app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:34`, `app/pautas/services.py:47`.)

- **Quem escreve:** `decidir` produz `fatia`, `gate`, `limite`, `pronta_fora_da_fila`, `pausa_sem_dono`, `teto_morto`, `trava_a_vazia`, `repete`, `vigia_sem_efeito`; `passada` encaminha motivo/resolver. Outros emissores produzem `relance`, `quarentena:zumbido:<dono>`, `quarentena:prazo`, `quarentena:deposito`, `auto_revert`, `quarentena`, `arvore:vermelho`. (`app/core/esteira_vigia.py:412`, `:426`, `:431`, `:461`, `:483`, `:495`, `:532`, `:557`, `:592`, `:1603`, `:1608`, `:992`, `:1212`, `:1274`, `:1277`, `:1307`, `:1364`, `:1371`.)

- **Quem lê:** memória de alarmes deduplica pela chave; `_pauta` envia argumentos ao comando; `handle` escolhe `resolver`/`motivo`; `pauta_viva` consulta prefixo textual; resolver fecha a primeira viva encontrada; abertura escreve `ESTEIRA <chave>: <texto>`. (`app/core/esteira_vigia.py:402`, `app/core/esteira_vigia.py:936`, `app/core/management/commands/alarme_esteira.py:30`, `:18`, `:41`, `:62`.)

- **Não se pode afirmar que todo valor que um emissor real pode formar passa.** A fila permite diretório com espaços (`split(None, 1)`), mas os emissores concatenam `basename` sem validação. Assim `fatia:a b` é formável pelo código e recusada agora. O censo usa basenames sintéticos sem whitespace, explicitamente. Ocorrência em produção: **[nao verificado]**. (`app/core/esteira_vigia.py:119`, `:121`, `:412`, `:557`, `app/pautas/tests/test_prefixos_reservados_esteira.py:135`, `:153`.)

- **Recusa pode perder o rastro da recusa:** o comando escreve stderr e retorna normalmente, mas `_pauta` captura stdout/stderr e descarta o `CompletedProcess`, sem verificar retorno nem registrar stderr. Os relatos de alarmes/zumbido registram a ação presumida, sem confirmar abertura. (`app/core/management/commands/alarme_esteira.py:34`, `app/core/esteira_vigia.py:936`, `:1215`, `:1604`.) A memória já marcou o alarme antes do envio; um alarme sem `cada_min` pode ficar deduplicado mesmo sem Pauta criada. (`app/core/esteira_vigia.py:403`, `:406`, `:412`.)

- **Borda adicional de identidade textual:** chave de 472 letras passa a gramática; o `[:480]` corta o delimitador final, pois `'ESTEIRA '` mede oito caracteres. A guarda não reserva esse texto sem `:`, e o leitor procura prefixo completo que não foi persistido. O teste aceita somente até 460 letras. Trata-se de falha demonstrável pelo código; ocorrência real **[nao verificado]**. (`app/pautas/marcadores.py:31`, `app/core/management/commands/alarme_esteira.py:18`, `:62`, `app/pautas/services.py:47`, `app/pautas/tests/test_prefixos_reservados.py:279`.)

- **`sem-commit:<arquivo>` é propagado como valor opaco:** dono agrupa testes; vira `quem`, motivo `dono=...`, chave de Pauta, texto de Pauta, relato e retorno. (`app/core/esteira_vigia.py:1174`, `:1205`, `:1211`, `:1212`, `:1215`, `:1219`.) JSON grava `quem`/motivo sem interpretar; lista corta motivo em 120 caracteres. (`app/core/esteira_vigia.py:1245`, `:1098`.) O parser da lista interpreta ID/prazo e deixa o restante como texto; runner e integrador usam IDs. (`app/core/quarentena_lista.py:19`, `:21`, `app/core/runner_quarentena.py:36`, `:77`, `app/core/integrador_lote.py:780`.) Persistência do relato insere a linha sem interpretar dono. (`app/core/esteira_vigia.py:895`, `:903`, `:925`.)

- **Não encontrei leitor de produção que case `sem commit (` nem parser de `quarentena.jsonl`** no censo textual da cópia; a forma antiga aparece somente no mutante. A gravação identificada do JSON está em `app/core/esteira_vigia.py:1243`; mutante em `app/pautas/tests/mutar_prefixos_reservados.py:139`. Leitor externo à cópia **[nao verificado]**.

- **Pauta antiga pode ficar inacessível ao resolver ESTEIRA:** `quarentena:zumbido:sem commit (...)` contém espaços, portanto o resolver atual rejeita antes da consulta; a chave nova usa outro prefixo e não encontra a antiga. (`app/pautas/marcadores.py:31`, `app/core/management/commands/alarme_esteira.py:30`, `:34`, `:18`, `app/core/esteira_vigia.py:1174`, `:1212`.) A existência dessas pautas em produção **[nao verificado]**. Não atribuir uma regressão de “fechamento automático do zumbido”: os resolvers existentes de `decidir` abrangem pausa/teto/trava/vigia, e `conferir_quarentena` remove testes da lista sem resolver Pauta de zumbido. (`app/core/esteira_vigia.py:489`, `:503`, `:529`, `:596`, `:1260`, `:1274`.)

## 10/11 08:46 · sessao 42f15a7e4013 · - · - · origem=Ronald (tmux)

Mapa dos campos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `fora_do_universo` | `app/ponto/services/cluster_espurio.py:109-110`, devolvido em `:148` | Serialização integral em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:222-225`, `:254-260`. |
| `excluidos` | `app/ponto/services/cluster_espurio.py:126-145`, devolvido em `:148` | Serialização em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:229-247`, `:362-366`. |
| `numero` | Soma por chamado em `app/ponto/services/cluster_espurio.py:125`, devolvido em `:146` | JSON em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; decisão do alarme e mensagem em `:70-73`; contratos exemplares em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:302`, `:315`, `:348-349`. |
| `esperado` | Constante zero em `app/ponto/services/cluster_espurio.py:146` | JSON e comparação em `app/ponto/management/commands/detectar_cluster_espurio.py:63`, `:70-73`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:227`, `:356`; fixture sintética de valor 1 em `:416-421`. |

A semântica é coerente nos leitores encontrados: `excluidos` é catálogo de motivos, inclusive sem ocorrências; `fora_do_universo` contém quantidades observadas por via; `numero` conta chamados com assinatura, preservando duplicatas do mesmo par; `esperado` é o alvo do alarme (`app/ponto/services/cluster_espurio.py:109-148`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233-247`, `:342-349`; `app/ponto/management/commands/detectar_cluster_espurio.py:70`). Não encontrei outro consumidor produtivo desse retorno na busca integral da cópia; consumidores externos são **[nao verificado]**.

`None` e `''` **não quebram o `json.dumps` atual**. Executei, em memória, o serviço e o comando reais pelos helpers: `{None: 1, '': 1}` saiu como `{"null": 1, "": 1}`; o rótulo de `None` continua sob a chave textual `"null"` (`app/ponto/services/cluster_espurio.py:141-148`; `app/ponto/management/commands/detectar_cluster_espurio.py:63`; helpers em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:150-166`).

Há uma limitação reproduzida: se existir também a via desconhecida literal `'null'`, ela colide no JSON com a chave Python `None`. Reproduzi fonte `{None: 2, '': 1, 'null': 3}` → JSON com duas propriedades `"null"` → `json.loads` preserva apenas `3`; os rótulos também colidem. O código admite valores desconhecidos e não normaliza as chaves (`app/ponto/services/cluster_espurio.py:110`, `:142-145`; `app/ponto/management/commands/detectar_cluster_espurio.py:63`). Ocorrência de `'null'` e leitor externo afetado: **[nao verificado]**.

O cron configurado chama `--alarme`; o envelope lê o código de saída, sem interpretar esses campos JSON (`app/config/crons.py:598`; `bin/cron_run.sh:11-13`, `:22-25`). Portanto não há quebra demonstrada desse leitor existente; a colisão é uma limitação do formato de saída, condicionada a um valor que o dado fornecido não comprova.

## 10/11 08:45 · sessao db4c347226f8 · - · - · origem=Ronald (tmux)

Conferência dos escritores:

| Via/rótulo | Escritor real e comparação |
|---|---|
| `TRANSICAO_HUMANA` → “Via humana” | `transicionar` valida a transição e chama `_aplicar`; este grava `EVENTO_VIA['TRANSICAO_HUMANA']`. O backfill também atribui `admin` a resolvido/fechado com respondente fora dos usuários de sistema. Rótulo compatível; não prova passagem histórica por `transicionar`. (`app/chamados/models.py:128`, `app/chamados/models.py:199`, `app/chamados/models.py:447`, `app/chamados/migrations/0039_a3_via_backfill.py:26`) |
| `ENCERRAR` → “desligamento ou ajuste retroativo” | O gate documenta ambos e grava `EVENTO_VIA['ENCERRAR']`. Há chamadores concretos para desligamento e ajuste retroativo. (`app/chamados/models.py:1605`, `app/chamados/models.py:1619`, `app/colaboradores/services/desligamento.py:155`, `app/ponto/services/justificativa.py:400`, `app/chamados/reconciliador.py:496`) |
| `CANCELAR` → “Cancelamento de chamado vivo…” | O gate descreve descarte administrativo/emissor aposentado; o motor permite CANCELAR apenas de aberto/em_análise/registrado. O backfill carimba `cancelado` pela condição status cancelado + via vazia, sem provar motivo histórico. (`app/chamados/models.py:773`, `app/chamados/models.py:783`, `app/chamados/models.py:788`, `app/chamados/catalogo/motor.py:116`, `app/chamados/migrations/0039_a3_via_backfill.py:25`) |
| `SUPERAR` → “Duplicata cancelada com irmão único provado no backfill” | A migration seleciona cancelados de módulos de furo, exige irmão único do mesmo colaborador/data e grava `superado` com FK. O gate atual foi removido; o estado permanece para o acervo. Rótulo fiel ao escritor remanescente. (`app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794`) |
| `VIA_LEGADO` → “Terminal sem via derivável no backfill” | Após as derivações anteriores, a migration grava `legado` nos terminais ainda com via vazia. Rótulo fiel. (`app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:28`) |
| Competência trancada | O serviço aplica apenas se medição declarou trancada e chama `encerrar_por_competencia_trancada`; o gate grava a constante própria pelo núcleo. (`app/chamados/services/competencia_trancada.py:167`, `app/chamados/services/competencia_trancada.py:184`, `app/chamados/models.py:1639`) |
| `''` / `None` | São explicitamente rotulados como vazios; ocorrência persistida de `None` não está demonstrada. (`app/ponto/services/cluster_espurio.py:141`, `app/ponto/services/cluster_espurio.py:142`; ocorrência: [nao verificado]) |

Nenhuma via conhecida excluída cai em “desconhecida”: as seis vias diferentes de RECONCILIAR constam nominalmente no dicionário; o fallback usa `setdefault`. RECONCILIAR pertence ao universo contado. (`app/chamados/catalogo/motor.py:88`, `app/chamados/catalogo/motor.py:93`, `app/chamados/catalogo/motor.py:95`, `app/ponto/services/cluster_espurio.py:110`, `app/ponto/services/cluster_espurio.py:111`, `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:144`)

Ressalva de precisão: os textos descrevem classes de via; os escritores não comprovam, individualmente, ADMIN/DP, premissa administrativa ou motivo histórico de cada registro. CANCELAR recebe `motivo` livre; ENCERRAR não recebe motivo; o backfill usa heurísticas de campos. (`app/chamados/models.py:773`, `app/chamados/models.py:789`, `app/chamados/models.py:1605`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:26`)

O rótulo ENCERRAR tampouco esgota todos os usos globais do gate: troca de escala encerra chamados `escala_aceite`, e regularização externa encerra seu chamado. Esses usos não demonstram exclusão incorretamente rotulada no módulo cluster: regularização cria módulo próprio; troca de escala filtra categoria. (`app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/services/regularizacao_ext.py:250`)

## 10/11 08:45 · sessao 2a7073c850aa · - · - · origem=Ronald (tmux)

B: **os três mutantes morrem pelas asserções pretendidas**, executando a fonte real alterada apenas em memória. O carregador lê/compila a fonte; serviço e comando são carregados inteiros; juízes puros vêm da fonte real. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:31`, `:104`, `:108`, `:130`)

| Mutante | Falha observada na reexecução |
|---|---|
| FK cruzada: remove guarda de colaborador | `1 != 0` e `0 != 1`, ambas na asserção de `numero`; a FK cruzada tem assinatura inversa à célula correta. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:63`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:299`, `:302`) |
| Consulta por par | `9 != 3` na contagem de avaliações; a asserção anterior de `numero` passa. Há três colaboradores, três datas por colaborador e consultas vazias também são contadas. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:66`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:83`, `:307`, `:313`, `:315`, `:316`) |
| Rótulos humana/cascata trocados | Falha na igualdade exata de `excluidos`, independentemente de ocorrências. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:74`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233`, `:247`) |

Executei `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro ponto.tests.test_o169_cluster_encerrado_mutantes -v`: **61 testes OK, incluindo os 35 mutantes listados; nenhum desses 35 sobreviveu**. Resultado desta execução [nao verificado por artefato persistido]; definição/coleta dos mutantes em `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`, `:150`.

O harness exige alvo textual único, rejeita erros de infraestrutura e exige falha de asserção; nos três casos acima confirmei também a linha exata que falhou. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:135`, `:141`, `:142`)

Limite: a contagem observa avaliações do dublê, inclusive resultado vazio; SQL real, banco, gates de escrita e produção permanecem [nao verificado]. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:7`, `:81`, `:158`)

## 10/11 03:16 · sessao 3f625a779cd6 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

Corrigido na origem: o ramo sem SHA retorna `sem-commit:<arquivo>` ([esteira_vigia.py:1174](core/esteira_vigia.py:1174)).

O [relatório de entrega](pautas/tests/prefixos_reservados_volta5.r:5) contém o censo de quem escreve/lê cada campo, arquivos tocados, comandos, saídas e comando individual dos mutantes.

Resultados locais registrados:

- RED nas bases `5b6861f6f` e `2c998ff23`, ambos com zero erros ([saída](pautas/tests/prefixos_reservados_volta5.r:67)).
- Prefixos: 30 testes, 1 skip; core/zumbido: 11 testes OK ([saída](pautas/tests/prefixos_reservados_volta5.r:89)).
- **88 mutantes mortos; zero sobreviventes**, incluindo chamada nova a `_pauta` sem cobertura ([saída](pautas/tests/prefixos_reservados_volta5.r:112)).
- 2.205 rótulos reais válidos; caminho com espaço recusado, com stderr registrado ([saída](pautas/tests/prefixos_reservados_volta5.r:353)).

Ruff ausente ([saída](pautas/tests/prefixos_reservados_volta5.r:108)). Integração Django/HTTP/banco e suite completa: **[nao verificado]**. Certificação permanece com Code e opus final.

## 10/11 03:04 · sessao d0f364fe0074 · O169-FECHADA · construtor · origem=Code

Cura entregue em três arquivos:

- [cluster_espurio.py:126](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:126): rótulos por via conhecida, cascata com “desligamento ou ajuste retroativo”, vazias nomeadas e desconhecida reservada às demais vias. `numero`, `esperado` e `universo` preservados.
- [test_o169_cluster_encerrado_sem_lastro.py:233](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233): contratos de rótulo exato, FK cruzada e K consultas para K colaboradores com vários pares.
- [test_o169_cluster_encerrado_mutantes.py:63](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:63): três mutantes exigidos e seis adicionais para os rótulos.

Saída local: **61 testes OK; 35 mutantes executados individualmente, todos mordidos por asserção**. O harness rejeita erros de infraestrutura como prova. ([mutantes:131](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:131)) [nao verificado]

O RED de rótulos retornou `FAILED (failures=1)` antes da cura; depois passou. Comandos dos contratos: ([contrato:233](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233)) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro.ClusterEncerradoSemLastroTest.test_RED_rotulo_fiel_por_via_exata -v

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro ponto.tests.test_o169_cluster_encerrado_mutantes -v
```

Para emitir e executar o comando de **cada mutante**, individualmente, usando a lista entregue em [MUTANTES:13](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess
from ponto.tests.test_o169_cluster_encerrado_mutantes import MUTANTES

prefixo = 'ponto.tests.test_o169_cluster_encerrado_mutantes.MutantesO169Test.test_mutante_'
for nome in MUTANTES:
    comando = ['python3', '-m', 'unittest', prefixo + nome, '-v']
    print('PYTHONDONTWRITEBYTECODE=1 ' + ' '.join(comando), flush=True)
    subprocess.run(comando, check=True)
PY
```

Ruff sobre todo `app/`: `All checks passed!`, com as regras de [ruff.toml:4](/home/ronald/wt-codex-o169-fechada-025847/app/ruff.toml:4). Comando executado:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache .
```

Quem escreve e quem lê os campos usados pelo contador:

| Campos | Escritor | Leitor |
|---|---|---|
| `status_local`, `via_encerramento` | [_gravar_estado:228](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:228), [via:245](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:245); escritores por via citados em [cluster:127](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:127) | [contador:106](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:106) |
| Chamado: `colaborador_id`, `modulo_origem`, `contexto_json`, `celula_id` | [abrir:958](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:958), [persistência:971](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:971) | [contador:106](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:106), [dia_do_fato:98](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/juizes.py:98) |
| Célula: `colaborador_id`, `data`, `trabalha` | [geração:259](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:259), [edição:600](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:600) | [contador:117](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:117), [dia_sem_trabalho:426](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/juizes.py:426) |
| `ata`: órfãs, lâmpadas e tipo/cobertura do dia | [ata_do_dia:704](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cartorio.py:704), [lavra:1042](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:1042) | [_leitura_da_celula:25](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:25) |
| `nome`, `numero`, `esperado`, `dono`, `universo`, `fora_do_universo`, `excluidos` | [contador:146](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:146), em memória | [comando:63](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/management/commands/detectar_cluster_espurio.py:63); alarme lê `numero != esperado` em [comando:70](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/management/commands/detectar_cluster_espurio.py:70) |
| `consultas_celulas`, no teste | [dublê:83](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:83), inclusive consulta vazia | [contrato:316](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:316) | [nao verificado]

Não executados: banco/SQL, produção, concorrência, medição de frota/cron e certificação Code/opus. Esses resultados permanecem **[nao verificado]**.

_3360 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
