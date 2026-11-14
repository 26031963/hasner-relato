# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 03:39** · publicado em **14/11/2026 03:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 03:39 · sessao fa514ab77cc5 · CORTE-DAS-TRAVAS · censo · origem=Code

1. **(a) O Codex não viu o bloqueio com o tema presente.** (`verif_final.q:4` e `verif_final.q:7`)

   **Pergunta — verif_final.q:** “cura do "andavel" do hook Stop”; “Cada peca faz o que o texto do aval diz, nem mais nem menos?” (`verif_final.q:4` e `verif_final.q:7`)

   **Resposta — verif_final.r:** “a cópia contém tetos em segundos por papel e exige ausência de outro item andável para PAREI devolver o turno” (`bin/codex_pergunta.sh:95`; `bin/hook_stop_fila1.py:1151`, referências da resposta).

2. **(b) O Codex viu e a triagem descartou.** (`verif_v2.q:6`)

   **Pergunta — verif_final.q:** “Mais: religar a escrita do logs/AUDITORIA-TRAVAS/triagem.log (parou em 10/11 13:42).” (`verif_final.q:4`)

   **Resposta — verif_final.r:** “O acréscimo no wrapper apenas imprime uma instrução; não chama o escritor de triagem” (`bin/codex_pergunta.sh:469`, referência da resposta).

   **Descarte literal — verif_v2.q:** “#5 destino logs/AUDITORIA-TRAVAS/triagem.log x logs/codex/triagem.log, wrapper so imprime: DESCARTADO (o arquivo de AUDITORIA e copia byte a byte da trilha unica; um 2o escritor violaria um-escritor-por-estado; triar exige juizo de quem leu a resposta) -- vai ao opus como contestavel.” (`verif_v2.q:6`)

3. **(a) O Codex não viu esse bloqueio com o tema presente.** (`verif_final.q:3`; `verif_v2.r:7`)

   **Pergunta — verif_final.q:** “PROIBIDO: afrouxar qualquer trava fora desta lista; usar "so leitor" em obra que toca dinheiro, escala, vinculo ou exportada.” (`verif_final.q:3`)

   **Resposta — verif_v2.r:** “Mesmo assim, esse arquivo continua “so leitor”: o ramo de templates não examina conteúdo.” (`verif_v2.r:7`; `bin/classe_da_obra.py:156`, referência da resposta).

4. **(a) O Codex não viu os casos residuais com o tema presente.** (`verif_v3.q:13`; `verif_v3.r:7`)

   **Pergunta — verif_v3.q:** “O ramo template de classe_da_obra.py: ha mudanca de template/static que move dinheiro, escrita ou universo julgado e ainda sai "so leitor"?” (`verif_v3.q:13`)

   **Resposta — verif_v3.r:** “A regex examina a linha alterada, sem reconhecer tags Django que produzem controles.”; “Portanto, a alteração muda uma entrada de escrita, embora escape da cura.” (`verif_v3.r:7`; `bin/classe_da_obra.py:175`, referência da resposta).

## 14/11 02:27 · sessao 20149fde517f · E3-FORMA-SQL · verificador · origem=Code

1. **[bloqueia] Falta a matriz normativa anunciada para validar integralmente as prioridades.** O anexo disponível se declara histórico da volta 1 e contém status R1–R16, mas não o contrato I1–I8 nem as linhas prescritivas originais. Ele registra R2 como “esperado admin, obtido colab”; essa descrição ficou desatualizada para o caso simultâneo simples, que agora espera e obtém `admin` no teste sem banco. A autorização das demais combinações permanece **[não verificado]**. (`app/docs/e3_v8_matriz.txt:1`; `app/docs/e3_v8_matriz.txt:5`; `app/docs/e3_v8_matriz.txt:9`; `app/chamados/tests/test_e3_portas_tarefa.py:459`)

   As referências abaixo são da cópia indicada. A correspondência criptográfica com o hash informado e a comparação Git com a base são **[não verificado]**.

2. **[fila] A prioridade nova depende da origem do dono; não estabelece “admin sempre vence colab”.** `_portas_da_tarefa` literalmente retorna `None` e modifica `out`; a publicação ocorre depois por `porta_da_tarefa`. O corpo elimina `colab` dos donos das ações quando há ação administrativa alcançável, acrescenta o dono residual e então escolhe `colab` primeiro. (`app/chamados/services/tarefa_do_dia.py:588`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/services/tarefa_do_dia.py:596`; `app/chamados/services/tarefa_do_dia.py:599`; `app/chamados/services/tarefa_do_dia.py:609`)

   Na tabela, **A** são donos humanos das ações exigíveis com porta na fila; **R** são donos correspondentes aos verbos residuais, antes de a escada residual escolher um deles. `C=colab`, `A=admin`. As nove combinações abaixo cobrem a presença dos dois donos nessas fontes; acrescentar `chamado` ou `sistema` não muda a precedência humana. (`app/chamados/services/tarefa_do_dia.py:588`; `app/chamados/services/tarefa_do_dia.py:597`; `app/chamados/juizes.py:1640`)

   | Donos das ações | Donos residuais | Prescrição recuperável da matriz | Docstring antes do diff | Docstring depois / resultado atual |
   |---|---|---|---|---|
   | nenhum | C+A | [não verificado] | colab | colab |
   | C | A | [não verificado] | colab | colab |
   | C | C+A | [não verificado] | colab | colab |
   | A | C | [não verificado] | colab | colab |
   | A | C+A | [não verificado] | colab | colab |
   | C+A | nenhum | **R2: admin** | colab | **admin** |
   | C+A | A | [não verificado] | colab | admin |
   | C+A | C | [não verificado] | colab | **colab** |
   | C+A | C+A | [não verificado] | colab | **colab** |

   Fontes de todas as saídas: ações em `app/chamados/services/tarefa_do_dia.py:588`, descarte em `:591`, residual em `:596`, seleção em `:597`; escada residual em `app/chamados/juizes.py:1640`. A prescrição R2 está registrada em `app/docs/e3_v8_matriz.txt:5`. A coluna anterior deriva do trecho removido no diff fornecido; conferência na base Git **[não verificado]**.

   **Contraexemplo concreto:** `validar@101 + responder@202` publica `admin`; acrescentar `303:cobrar`, residual do mesmo colab-dia, publica `colab`, conservadas as duas ações. Isso corresponde à docstring nova: fonte (1) prioriza admin entre ações; fonte (3) prioriza colab ao combinar residual. Não há divergência demonstrada entre essa docstring e o corpo; falta a prescrição independente da matriz para julgar a combinação. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:45`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/services/tarefa_do_dia.py:596`)

   **Linhas nomeadas:** R2 é o único corpo da matriz que coloca as duas ações humanas juntas: mesma porta `101` ou portas distintas `101/202`, obtendo `admin` nos dois casos. R1 contém apenas `responder`; o verbo `validar` da mesma porta não constitui outra ação administrativa. R3 é administrativo isolado. (`app/chamados/tests/test_e3_portas_tarefa.py:441`; `app/chamados/tests/test_e3_portas_tarefa.py:459`; `app/chamados/tests/test_e3_portas_tarefa.py:463`; `app/chamados/tests/test_e3_portas_tarefa.py:473`)

   R4–R5, R7–R12 e R14 não exercitam duas ações humanas simultâneas; R13 exercita a ação administrativa isolada; R15 varia uma pergunta e preserva residual `decidir`, sem afirmar `fio`; R6 está fora da obra e R16 é execução pendente. Portanto esses corpos não prescrevem as combinações adicionais da tabela. (`app/chamados/tests/test_e3_portas_tarefa.py:479`; `app/chamados/tests/test_e3_portas_tarefa.py:490`; `app/chamados/tests/test_e3_portas_tarefa.py:505`; `app/chamados/tests/test_e3_portas_tarefa.py:615`; `app/chamados/tests/test_e3_portas_tarefa.py:633`; `app/chamados/tests/test_e3_portas_tarefa.py:683`; `app/chamados/tests/test_e3_portas_tarefa.py:686`; `app/docs/e3_v8_matriz.txt:16`; `app/docs/e3_v8_matriz.txt:26`)

   **Outra combinação não nomeada:** `decidir_ausencia@101 + responder@202` também passa a publicar `admin`, porque a condição nova examina apenas `de`, não `acao`. R13 fornece `decidir_ausencia`, mas testa essa ação sozinha. Justificação normativa específica dessa extensão: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:170`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/tests/test_e3_portas_tarefa.py:629`)

3. **[fila] Escrita e leitura dos cinco campos estão concentradas, mas os consumidores não têm todos o mesmo universo.** O mapa público é uma cópia de cinco chaves da tarefa, sem nova decisão ou persistência. (`app/chamados/services/tarefa_do_dia.py:603`; `app/chamados/services/tarefa_do_dia.py:609`)

   | Campo | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | `fio` | ações + residual, seleção final: `app/chamados/services/tarefa_do_dia.py:588`, `:597`; residual: `app/chamados/juizes.py:1645` | Folha: `app/folha/fora_da_folha.py:89`, `:236`; Vigia: `app/chamados/services/vigia_fio.py:658`; diff: `app/chamados/management/commands/diff_e3_sombra.py:143` |
   | `nao_sei` | inicialização e causas da tarefa: `app/chamados/services/tarefa_do_dia.py:193`, `:258`, `:320` | Folha: `app/folha/fora_da_folha.py:245`; Vigia: `app/chamados/services/vigia_fio.py:651`; furo: `app/chamados/services/furo_sem_canal.py:98` |
   | `rota` | `app/chamados/services/tarefa_do_dia.py:599` | Vigia: `app/chamados/services/vigia_fio.py:657`; furo: `app/chamados/services/furo_sem_canal.py:97`; diff: `app/chamados/management/commands/diff_e3_sombra.py:143` |
   | `fila` | associação à célula: `app/chamados/juizes.py:1568`; filtro nomeável: `app/chamados/services/tarefa_do_dia.py:584` | Folha: `app/folha/fora_da_folha.py:241`; diff: `app/chamados/management/commands/diff_e3_sombra.py:50` |
   | `verbos` | partição: `app/chamados/services/verbos.py:47`; transporte: `app/chamados/juizes.py:1565`; projeção: `app/chamados/services/tarefa_do_dia.py:583` | filtro de `fila` e residual: `app/chamados/services/tarefa_do_dia.py:584`, `:596`; diff: `app/chamados/management/commands/diff_e3_sombra.py:144` |

   **Tela:** a Folha lê fios por dia, escolhe o grupo por colaborador e o retrato converte esse grupo no fio gravado sob `dono`; depois faz o agrupamento visual em baldes e escolhe a frase. Isso não reescreve a porta. O novo `nao_sei` não é transportado por esse caminho da tela. (`app/folha/fora_da_folha.py:190`; `app/folha/fora_da_folha.py:195`; `app/folha/retrato_folha.py:364`; `app/folha/retrato_folha.py:1187`; `app/folha/retrato_folha.py:438`; `app/folha/retrato_folha.py:481`; `app/folha/retrato_folha.py:1069`)

   Pauta lê ausência de fio pela Folha; beco testa existência da projeção; backfill preserva as oito colunas antigas com `linha[:8]`. (`app/ponto/services/pautas_dos_discordantes.py:81`; `app/escala/services/cadastro_realidade.py:497`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:253`)

   **Semântica e contraexemplos:**

   - Dia do turno e dia civil permanecem separados: uma saída após meia-noite pode ter `data_civil` seguinte sem mudar `dia_do_turno`. A associação sem célula usa colaborador e dia forte. (`app/chamados/services/tarefa_do_dia.py:113`; `app/chamados/services/tarefa_do_dia.py:228`; `app/chamados/services/tarefa_do_dia.py:577`; `app/chamados/juizes.py:1574`)
   - `None` significa nenhuma porta; `{}` não é saída válida. Substituir a projeção por `{}` provoca chave ausente nos leitores. Uma porta com `fila=()` continua sendo porta. (`app/chamados/services/tarefa_do_dia.py:609`; `app/folha/fora_da_folha.py:89`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/services/furo_sem_canal.py:97`; `app/chamados/tests/test_e3_portas_tarefa.py:483`)
   - Vigia deduplica colab-dia; grupos da Folha contam colaboradores; furo conta células do seu recorte. Logo comparar diretamente esses totais não prova divergência nem igualdade em produção. (`app/chamados/services/vigia_fio.py:593`; `app/folha/fora_da_folha.py:195`; `app/chamados/services/furo_sem_canal.py:79`; `app/chamados/services/furo_sem_canal.py:109`)

4. **[fila] Checklist das classes reprovadoras: há limites remanescentes, sem novo defeito persistente demonstrado nos corpos alterados.**

   | Classe | Resultado e contraprova |
   |---|---|
   | Segundo juiz duplicando registrado | **Ausente nos corpos alterados.** A projeção copia a tarefa; C17 permanece apenas no comparador. A catraca rejeita um leitor adicional. (`app/chamados/services/tarefa_do_dia.py:609`; `app/chamados/management/commands/diff_e3_sombra.py:127`; `app/chamados/tests/test_e3_pendentes_app.py:145`; `:150`) |
   | Segundo escritor fora da porta | **Ausente nos corpos alterados.** A escrita produtiva acrescentada compõe `out` em memória. Persistência zero em integração ORM: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:599`; `app/docs/e3_v8_matriz.txt:6`) |
   | Relógio fora da borda | **Ausente na cadeia E3 com instante injetado; presente no legado sem instante.** `consulta_do_app` ainda omite `agora`; os fallbacks internos continuam existentes. (`app/chamados/juizes.py:1619`; `:1632`; `app/chamados/services/pergunta_no_app.py:61`; `:73`; `app/chamados/juizes.py:592`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`) |
   | Derivação paralela da decisão | **Ausente nos leitores alterados:** leem o mapa; sem porta, leem `nao_sei` da tarefa original. (`app/folha/fora_da_folha.py:235`; `:245`; `app/chamados/services/vigia_fio.py:644`; `:651`; `app/chamados/services/furo_sem_canal.py:98`) |
   | Ordem de trava | **Nenhuma nova ordem de locks SQL identificada nos corpos alterados.** Os contextos usados são de consulta da tranca. Comportamento concorrente ORM: **[não verificado]**. (`app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:100`) |
   | Pouso antes do pré-requisito | Pouso efetivo **[não verificado]**. O próprio anexo deixa alcance ORM e R16 pendentes; não há prova fornecida de cumprimento desses pré-requisitos. (`app/docs/e3_v8_matriz.txt:6`; `:26`) |
   | Campo com dois significados | **Não demonstrado para dia/porta.** Há duas origens para `verbos[pk]=None`: registro sem verbo ou membro ausente da partição. A projeção perde essa proveniência, mas conserva ambos como ausência de verbo observado; nenhum erro adicional foi demonstrado por isso. (`app/chamados/catalogo/natureza.py:63`; `app/chamados/juizes.py:1565`; `:1651`) |

5. **[fila] Os dois selos destacados conservam mordida, com níveis diferentes de prova.**

   **C6:** executei os três mutantes sem banco; retirar C6, retirar `q_vivas` ou retirar o instante produziu, cada um, uma falha de assert e zero erros após controle verde. O executor exige essa distinção. (`app/chamados/tests/e3_v8_volta2_selos.py:11`; `:36`; `:46`; `app/chamados/tests/test_contract_pergunta_viva.py:81`; `:84`)

   **Golden:** por inspeção, a reversão “chamado vivo sem pergunta não move fio” é rejeitada: o teste cria chamado real, exige fio `chamado` e sua frase, mantém o balde e depois verifica a mudança para admin por projeção substituída. Não depende apenas da contagem do balde. Execução ORM dessa mordida: **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:517`; `:522`; `:523`; `:524`; `:535`; `:541`; `:542`)

6. **[fila] Expectativas alteradas: há respaldo referenciado, mas o golden não é justificado especificamente pela linha R7 fornecida.**

   | Alteração | Justificação recuperável |
   |---|---|
   | `colab_ids` nos argumentos do alimentador | R8, ligação sem célula. (`app/chamados/tests/test_e3_portas_tarefa.py:140`; `app/docs/e3_v8_matriz.txt:18`) |
   | F5B: `None/nada` → porta com verbo `None/chamado` | R7 explicitamente registra a inversão. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:291`; `app/docs/e3_v8_matriz.txt:17`) |
   | Texto do relógio no diff | R11 e transmissão atual do instante. (`app/chamados/tests/test_e3_diff_sombra.py:23`; `app/docs/e3_v8_matriz.txt:21`; `app/chamados/services/pergunta_no_app.py:61`) |
   | Assert AST do filtro C6 com `agora` | R11/L-128; preserva C6 e exige instante. (`app/chamados/tests/test_contract_pergunta_viva.py:81`; `app/docs/e3_v8_matriz.txt:21`; `app/docs/LEIS.md:149`) |
   | Mocks acrescentam `nao_sei`; assert de repasse | Contrato aditivo/independência de R5. (`app/chamados/tests/test_e3_bloqueantes.py:188`; `app/chamados/tests/test_o261e_f5_alcance.py:372`; `app/folha/tests/test_haiku_fora_da_folha.py:154`; `app/docs/e3_v8_matriz.txt:201`) |
   | Golden: chamado sem pergunta passa a mover fio | Cita E3-PORTA-SEM-CASO. R7 da tabela trata especificamente membro ausente da partição; I5 não foi fornecido. O respaldo geral aparece no contrato de publicação, mas o aval original está **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:502`; `app/docs/e3_v8_matriz.txt:17`; `app/chamados/services/tarefa_do_dia.py:559`) |
   | Mock golden: `decidir` → `validar`, mantendo fio admin | Ajuste de fixture; a expectativa final de admin continua explícita. (`app/folha/tests/test_haiku_fora_da_folha.py:532`; `:541`) |

   Portanto, não identifiquei inversão sem **referência alegada**; a autorização independente do golden por I5/aval integral e a extensão de R2 para `decidir_ausencia + responder` permanecem **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:502`; `app/chamados/services/tarefa_do_dia.py:170`; `:591`)

7. **[fila] O instrumento de sombra não compara o novo `nao_sei`.** `projecoes` enumera os campos dos consumidores, mas omite esse campo, agora lido por Folha e Vigia. **Contraexemplo:** duas portas iguais em rota/fio/fila/verbos, diferentes apenas em `nao_sei`, geram projeções iguais no comparador. Isso limita a evidência do instrumento; os testes específicos dos leitores cobrem separadamente o repasse. (`app/chamados/management/commands/diff_e3_sombra.py:46`; `:49`; `:53`; `:133`; `app/folha/fora_da_folha.py:245`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/tests/test_e3_portas_tarefa.py:490`)

8. **[bloqueia] Alcance ORM e conclusão dos proibidos têm limites explícitos.** R2/R3 precisam confirmar crédito/partição reais; R8 precisa confirmar nascimento pelo escritor, dia forte e pertencimento à fila; R14 precisa confirmar integração e persistência; R16 precisa da execução da sessão. Os corpos preparados não substituem esses resultados. (`app/chamados/tests/test_e3_portas_tarefa.py:202`; `:220`; `:225`; `:267`; `:301`; `app/docs/e3_v8_matriz.txt:26`)

   Nos corpos produtivos alterados, não identifiquei juiz novo, encerramento de chamado ou supressão nova de chamado vivo: a união conserva portas residuais e apenas restringe a nomeação em `fila` à lista padrão. SC-A/R6 continua declarado fora da obra. (`app/chamados/services/tarefa_do_dia.py:575`; `:584`; `:599`; `app/docs/e3_v8_matriz.txt:16`)

   No diff textual fornecido não aparecem alterações em models/migrations/`core/juizes.py`/motor/crons/bin; confirmação de ausência de mudanças fora desse diff: **[não verificado]**.

   Reexecutei **26 testes sem banco e 7 da catraca C17, todos passando**. Isso não executa os casos ORM: o executor sem banco remove expressamente a classe correspondente. (`app/chamados/tests/e3_portas_regra.py:25`; `:27`; `:77`; `app/chamados/tests/test_e3_pendentes_app.py:118`)

## 14/11 02:24 · sessao 7f6a76a4a4f0 · - · - · origem=Ronald (tmux)

1. **Escritores e leitores do mapa.** Os cinco campos são projeção em memória; `porta_da_tarefa` copia cinco chaves da tarefa e devolve `None` quando não há rota (`app/chamados/services/tarefa_do_dia.py:603-609`).

| Campo | Escritor / fornecedor | Leitores de produção |
|---|---|---|
| `fio` | decisão final em `_portas_da_tarefa`: `app/chamados/services/tarefa_do_dia.py:588-600`; residual consulta `app/chamados/juizes.py:1645-1651` | Folha `app/folha/fora_da_folha.py:86-89,190,236`; Vigia `app/chamados/services/vigia_fio.py:658-659`; diff `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `nao_sei` | inicialização `app/chamados/services/tarefa_do_dia.py:192-193`; causas `:215,224,258-262,284,288,320-323`; copiado na projeção `:609` | Folha `app/folha/fora_da_folha.py:245`; Vigia `app/chamados/services/vigia_fio.py:651-652,635`; furo `app/chamados/services/furo_sem_canal.py:98-100` |
| `rota` | `app/chamados/services/tarefa_do_dia.py:599-600` | projeção `:609`; Vigia `app/chamados/services/vigia_fio.py:657-659`; furo `app/chamados/services/furo_sem_canal.py:97`; diff `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `fila` | associação célula `app/chamados/juizes.py:1567-1568`; filtro nomeável `app/chamados/services/tarefa_do_dia.py:584,600` | Folha `app/folha/fora_da_folha.py:241`; diff `app/chamados/management/commands/diff_e3_sombra.py:50,143` |
| `verbos` | partição `app/chamados/services/verbos.py:30-52`; transporte `app/chamados/juizes.py:1560,1565,1578`; recorte portas `app/chamados/services/tarefa_do_dia.py:583,600` | residual `:596`; nomeação fila `:584`; diff `app/chamados/management/commands/diff_e3_sombra.py:144` |

2. **Tela: leitores indiretos, sem segundo escritor da porta.** `fora_da_folha` lê cada fio, soma dias e escolhe grupo do colaborador (`app/folha/fora_da_folha.py:190-205`; prioridade grupo `:92-103`). `montar_linhas` consulta essa fonte e grava o grupo convertido em fio sob a chave `dono` (`app/folha/retrato_folha.py:1158,1187`; conversão `:364-368`). A tela lê o fio gravado (`:438-441`), converte-o em balde visual (`:192,474-481`) e lê a frase `DESTRAVA` (`:1069`). `nao_sei` não é transportado por esse caminho: a linha montada contém `dono`, tipo e datas, mas não esse campo (`:1187-1194`). Exibição por outros consumidores: **[não verificado]**.

3. **Unidades e `None`.** O dia da tarefa é o dia do turno (`app/chamados/services/tarefa_do_dia.py:192,228-246`); a data civil do marco permanece campo separado (`:112-113`). O vínculo sem célula usa `(colaborador_id,dia_do_turno)` e `dia_do_fato(fortes=True)` (`:577-578`; `app/chamados/juizes.py:1574-1576`). Vigia deduplica `(colab,data)` (`app/chamados/services/vigia_fio.py:591-594`); furo conta PKs de células (`app/chamados/services/furo_sem_canal.py:79-100,109`). Portanto, igualdade universal entre contagens em produção é **[não verificado]**.

   `None` é ausência de porta; `{}` não é saída válida da projeção (`app/chamados/services/tarefa_do_dia.py:604-609`). Contraexemplo: substituir a projeção por `{}` causa acesso a chave inexistente em Folha, Vigia e furo (`app/folha/fora_da_folha.py:89`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/services/furo_sem_canal.py:97`). Uma porta válida com `fila=()` continua porta, por exemplo a âncora externa R4 (`app/chamados/tests/test_e3_portas_tarefa.py:477-478`, conferir numeração atual com agente da matriz).

4. **Classes do checklist, escopo produção alterada.**
   - **Segundo juiz duplicado: ausente nos corpos alterados.** A projeção copia a decisão, e C17 permanece apenas no comparador (`app/chamados/services/tarefa_do_dia.py:606-609`; `app/chamados/management/commands/diff_e3_sombra.py:127`; exceção registrada `app/core/juizes.py:132-138`).
   - **Segundo escritor persistente: ausente nos corpos alterados.** A escrita acrescentada é `out.update`, em memória (`app/chamados/services/tarefa_do_dia.py:599-600`).
   - **Relógio fora da borda: ausente na cadeia E3 injetada; presente no comportamento legado sem instante.** Cadeia atual: `alimentar_portas` guarda instante (`app/chamados/juizes.py:1619`), `_no_app` repassa (`:1632`), app entrega a C6 (`app/chamados/services/pergunta_no_app.py:61`), C6 converte o instante em dia local (`app/chamados/juizes.py:885-887`). Sem instante existem `date.today()` e `timezone.now()` internos (`app/chamados/juizes.py:592`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`); a consulta normal do app continua omitindo `agora` (`app/chamados/services/pergunta_no_app.py:73`).
   - **Derivação paralela nos leitores alterados: ausente.** Todos leem a projeção ou, sem ela, apenas `nao_sei` da tarefa original (`app/folha/fora_da_folha.py:235-245`; `app/chamados/services/vigia_fio.py:643-659`; `app/chamados/services/furo_sem_canal.py:95-100`).
   - **Ordem de trava SQL nova: ausente nos corpos alterados.** O app usa contexto de consulta da tranca, e o diff envolve a consulta em `tranca_em_lote` (`app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:100`). Efeito ORM e ausência global de persistência: **[não verificado]**.
   - **Campo com duas causas: presente, fila.** `None` no verbo pode vir da auditoria/registro (`app/chamados/catalogo/natureza.py:63,68,73`) ou da ausência do membro na partição (`app/chamados/juizes.py:1565`). Ambos são tratados como ausência de verbo observado; o residual conserva `fio=chamado` (`app/chamados/juizes.py:1647-1651`). Não há contraexemplo de erro adicional demonstrado só por essa perda de proveniência.

5. **Outros consumidores.** Pauta identifica ausência de fio pela Folha (`app/ponto/services/pautas_dos_discordantes.py:79-84`); beco testa diretamente `porta_da_tarefa(...) is not None` (`app/escala/services/cadastro_realidade.py:492-498`). Backfill aceita a nona coluna e preserva as oito anteriores por `linha[:8]` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:253`).

6. **Proibidos, limitação da prova.** Nos trechos produtivos do diff fornecido não aparece escrita em models/migrations/core/juizes/motor/crons/bin nem chamada de encerramento; os corpos alterados alimentam e projetam dados (`app/chamados/juizes.py:1549-1579`; `app/chamados/services/tarefa_do_dia.py:553-609`). Alteração fora do diff fornecido e confirmação criptográfica do hash: **[não verificado]**. A obra continua publicando chamados residuais e preservando histórico fora da lista padrão (`app/chamados/services/tarefa_do_dia.py:575-584`); R6 segue fora da obra conforme matriz (`app/docs/e3_v8_matriz.txt:16`).

## 14/11 02:24 · sessao b7b3cb0bd835 · - · - · origem=Ronald (tmux)

1. **[fila] Falta contrato normativo integral para julgar todas as combinações.** O arquivo é explicitamente histórico da volta 1 e contém status, não o contrato I1..I8 anunciado. R2 registra esperado `admin`, mas as demais prescrições de prioridade não estão presentes. Não usar a nova docstring como prova independente de autorização. `app/docs/e3_v8_matriz.txt:1`, `:5`, `:9`, `:30`.

2. **[fila] A nova prioridade é condicional à origem do dono.** Com `C=colab`, `A=admin`, ações alcançáveis e donos dos verbos residuais:

| Donos das ações | Donos residuais | Antes do diff | Hash atual |
|---|---|---|---|
| nenhum | C+A | colab | colab |
| C | A | colab | colab |
| C | C+A | colab | colab |
| A | C | colab | colab |
| A | C+A | colab | colab |
| C+A | nenhum | colab | admin |
| C+A | A | colab | admin |
| C+A | C | colab | colab |
| C+A | C+A | colab | colab |

O código remove `colab` dos donos das ações antes de acrescentar o residual, que pode recolocá-lo. A escada final volta a priorizar `colab`. Confirmei as nove saídas executando somente os corpos AST em memória. `app/chamados/services/tarefa_do_dia.py:588`, `:591`, `:596`, `:597`; `app/chamados/juizes.py:1640`, `:1645`.

Contraexemplo: `validar@101 + responder@202` devolve `admin`; acrescentar residual `303:cobrar` devolve `colab`, preservadas as duas ações. Isto corresponde à docstring nova — fonte (1), admin entre ações; fonte (3), colab ao combinar residual — mas não prova a prescrição integral da matriz. `app/chamados/services/tarefa_do_dia.py:42`, `:45`.

3. **[fila] R2 cobre apenas parte das dimensões possíveis.** O teste novo verifica duas ações humanas na mesma porta `101` e em portas distintas `101/202`, ambas ligadas à célula; em ambos os casos espera e obtém `admin`. `app/chamados/tests/test_e3_portas_tarefa.py:459`, `:463`, `:469`, `:470`.

Com `fila_cel={}` e as duas portas nas ações, o resultado é `porta_pergunta`, `fila=()`, `fio=admin`. Acrescentar residual `cobrar` fora dessas portas muda para `porta_fila`, `fio=colab`. Essas extensões não aparecem no corpo R2; alcance ORM não demonstrado. `app/chamados/services/tarefa_do_dia.py:571`, `:581`, `:599`; `app/docs/e3_v8_matriz.txt:29`.

4. **[fila] A correção identificada como R2 também muda ações administrativas diferentes de validar.** `decidir_ausencia@101 + responder@202`, ambas na fila, passa de `colab` para `admin`, porque o novo `if` olha apenas o dono. A razão R13 produz `decidir_ausencia`, mas seu teste contém apenas essa ação administrativa. Sustentação normativa específica para alterar essa combinação: **[não verificado]**. `app/chamados/services/tarefa_do_dia.py:170`, `:591`; `app/chamados/tests/test_e3_portas_tarefa.py:615`, `:629`, `:630`.

5. **[fila] Relação observável das linhas R com simultaneidade.**

- **R1:** só ação `responder`; `validar` é verbo da mesma porta, não outra ação admin. Prescreve/obtém `colab` no teste. `app/chamados/tests/test_e3_portas_tarefa.py:441`, `:444`, `:447`.
- **R2:** único corpo R contendo duas ações humanas simultâneas; prescreve/obtém `admin`, mesma porta ou portas distintas. `app/chamados/tests/test_e3_portas_tarefa.py:459`.
- **R3:** só ação admin, não prova concorrência humana. `app/chamados/tests/test_e3_portas_tarefa.py:473`.
- **R4:** pergunta encerrada, nenhuma ação humana; residual `decidir`. `app/chamados/tests/test_e3_portas_tarefa.py:479`.
- **R5:** substitui o mapa para testar leitores; não produz simultaneidade. `app/chamados/tests/test_e3_portas_tarefa.py:490`, `:497`.
- **R6:** fora da obra segundo o anexo. `app/docs/e3_v8_matriz.txt:16`.
- **R7:** membro sem verbo, sem ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:505`.
- **R8:** residual sem célula com verbo `decidir`, sem ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:518`.
- **R9:** passivo/âncora `validar`, sem ação colab simultânea. `app/chamados/tests/test_e3_portas_tarefa.py:283`, `:289`, `:537`.
- **R10:** nenhuma ação; conserva âncora. `app/chamados/tests/test_e3_portas_tarefa.py:291`, `:299`.
- **R11:** testa cadeia de relógio até C6, não concorrência de donos. `app/chamados/tests/test_e3_portas_tarefa.py:560`.
- **R12:** motivos de desconhecimento com residual `decidir`, não concorrência humana. `app/chamados/tests/test_e3_portas_tarefa.py:570`, `:586`.
- **R13:** ato desconhecido ou absorção pendente; teste administrativo isolado. `app/chamados/tests/test_e3_portas_tarefa.py:594`, `:615`.
- **R14:** paridade de universo/contagem; sem duas ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:633`, `:640`.
- **R15:** varia uma pergunta Q2..Q16 e conserva residual `101:decidir`; não põe ações colab+admin juntas e nem afirma `fio`. `app/chamados/tests/test_e3_portas_tarefa.py:651`, `:683`, `:686`.
- **R16:** execução de suíte pendente, não um caso de concorrência. `app/docs/e3_v8_matriz.txt:26`.

O teste anterior explícito `validar@101 + residual cobrar@202` permanece esperando `colab`; outro teste conserva essa expectativa com terceiro residual `decidir`. Portanto a cópia mantém contratos diferentes para admin vindo de ação e colab vindo do residual, coerentes com a docstring nova. A aprovação normativa dessa combinação além do material fornecido permanece **[não verificado]**. `app/chamados/tests/test_e3_portas_v6_sem_banco.py:32`; `app/chamados/tests/test_e3_portas_tarefa.py:429`.

## 14/11 02:23 · sessao 78ed26487d90 · - · - · origem=Ronald (tmux)

1. **Selos de C6 mordem a reversão.** Executei sem banco os três mutantes de `app/chamados/tests/e3_v8_volta2_selos.py:11`: `sem_C6`, `sem_q_vivas` e `sem_instante`; cada controle passou e cada mutante produziu uma falha de assert, zero erros. Os asserts são `app/chamados/tests/test_contract_pergunta_viva.py:81` e `:84`; o executor exige controle verde e falha real em `app/chamados/tests/e3_v8_volta2_selos.py:36` e `:46`.

2. **Golden conserva mordida por inspeção; execução ORM [nao verificado].** O chamado real nasce em `app/folha/tests/test_haiku_fora_da_folha.py:517`; as expectativas `{'chamado':1,'admin':1}` e `DESTRAVA['chamado']` em `:522` e `:523` rejeitam a volta do defeito “vivo sem pergunta não move fio”. O balde continua conferido em `:524` e `:525`; a projeção trocada posteriormente deve mover fio/frase para admin em `:535`, `:541` e `:542`. A fonte da tela passa por `montar_linhas` nos helpers `:402`, `:419` e `:432`.

3. **Expectativas alteradas e respaldo verificável:**

   | Alteração | Respaldo |
   |---|---|
   | `test_e3_portas_tarefa.py:140`: `colab_ids` no carregamento | R8, `app/docs/e3_v8_matriz.txt:18`; implementação `app/chamados/services/tarefa_do_dia.py:545` |
   | `test_o261e_f5b_juiz_porta.py:291`: None/nada → porta com verbo None/chamado | R7, `app/docs/e3_v8_matriz.txt:17`; aval (ii) alegado em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:277` |
   | `test_e3_diff_sombra.py:23`: texto do relógio | R11, `app/docs/e3_v8_matriz.txt:21`; cadeia `app/chamados/services/pergunta_no_app.py:61` |
   | `test_contract_pergunta_viva.py:81`: igualdade AST com `agora` | R11, `app/docs/e3_v8_matriz.txt:21`; L-128, `app/docs/LEIS.md:149` |
   | Golden `test_haiku_fora_da_folha.py:522`: chamado vivo sem pergunta muda fio | Aval E3-PORTA-SEM-CASO alegado em `:502`, já citado em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:163` e `:241` |
   | Mocks recebem `nao_sei`, com assert adicional em `test_haiku_fora_da_folha.py:154` | R5, `app/docs/e3_v8_matriz.txt:15`; semântica em `:201` |
   | Golden mock passa de verbo decidir para validar, mantendo fio admin em `test_haiku_fora_da_folha.py:532` | Adequação de fixture; o assert final de fio admin já permanece em `:541` |

4. **Limite documental relevante.** A matriz disponível contém status históricos R1–R16 em `app/docs/e3_v8_matriz.txt:9`, mas não contém o contrato anunciado I1–I8; I1 e I5 aparecem como referências nos comentários `app/folha/tests/test_haiku_fora_da_folha.py:149` e `:502`. O conteúdo literal original desses contratos/avais é **[nao verificado]**. Assim, nenhuma inversão de expectativa está totalmente sem justificativa alegada, mas a autorização integral não pode ser validada só pelo anexo disponível.

## 14/11 02:12 · sessao 359866b81b9d · E3-FORMA-SQL · construtor · origem=Code

R2 foi corrigida na origem. Em R14/R5, os leitores já consumiam o mapa; as falhas vinham das fixtures. [Cura e diagnóstico](docs/e3_v8_volta2.txt:16).

| Ocorrência | Classe | Cura |
|---|---|---|
| 1. KeyError porta_pergunta | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:11) |
| 2. KeyError porta_fila | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:12) |
| 3. KeyError historico | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:13) |
| 4. KeyError auditoria | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:14) |
| 5. R14 ORM | II | [Mesma competência e universo do export](docs/e3_v8_volta2.txt:15) |
| 6. R2 | I | [Admin precede entre ações alcançáveis](docs/e3_v8_volta2.txt:16) |
| 7. Golden | II | [Expectativa corrigida por R7/I5](docs/e3_v8_volta2.txt:17) |
| 8. Endpoint | III | [Selo AST preservando C6 e instante](docs/e3_v8_volta2.txt:18) |
| 9. Ruff | I | [Cinco achados corrigidos na origem](docs/e3_v8_volta2.txt:19) |
| 10. R14 puro | II | [Janela do fornecedor corrigida](docs/e3_v8_volta2.txt:20) |
| 11. R5 puro | II | [Mesmo defeito de janela](docs/e3_v8_volta2.txt:21) |

A entrega inclui [arquivos tocados](docs/e3_v8_volta2.txt:45), [quem mais lê cada função](docs/e3_v8_volta2.txt:66) e [mutantes novos com comandos individuais](docs/e3_v8_volta2.txt:125).

Localmente: [RED com quatro falhas por valor](docs/e3_v8_volta2_red.txt:107), [26 testes GREEN](docs/e3_v8_volta2_green.txt:29), [70 regressões OK](docs/e3_v8_volta2_regressoes.txt:3) e [resultados dos mutantes](docs/e3_v8_volta2.txt:111).

**Incompleto:** sete ocorrências exigem rerodada ORM; Ruff está indisponível. Esses resultados e a certificação seguem **[nao verificado]**. [Pendências e comando do recorte](docs/e3_v8_volta2.txt:143).

## 14/11 01:42 · sessao 7ad1129d1278 · CORTE-DAS-TRAVAS · verificador · origem=Code

**REPROVADO.** A cura melhora os casos apresentados, mas ainda permite escrita classificada como `so leitor` e pendências apagadas como zero. (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:175`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`)

Os contraexemplos abaixo são sintéticos, exercitados em memória sobre o código da cópia; ocorrência em produção `[nao verificado]`. Execução integral dos selos `[nao verificado]`: os scripts criam e removem arquivos, contrariando a restrição desta verificação. (`bin/tests/test_triagem_pendente.sh:12`, `bin/tests/test_gate_pela_classe.sh:27`)

1. **[bloqueia] `bin/classe_da_obra.py:175` — controle de formulário gerado por template continua “só marcação”.**

   A regex examina a linha alterada, sem reconhecer tags Django que produzem controles. Contraexemplo sobre arquivo existente: trocar `{% opcao_mover "fechado" %}` por `{% opcao_mover "resolvido" %}` em `app/templates/chamados/detalhe.html:134` resulta em `so leitor`. A tag gera `<option value=...>`, dentro do campo `novo_status`; o backend lê esse valor e o entrega à transição. Portanto, a alteração muda uma entrada de escrita, embora escape da cura. (`app/chamados/templatetags/chamados_tags.py:77`, `app/templates/chamados/detalhe.html:130`, `app/chamados/services/acoes_chamado.py:296`, `app/chamados/services/acoes_chamado.py:325`, `bin/classe_da_obra.py:215`)

2. **[bloqueia] `bin/classe_da_obra.py:144` — conteúdo do diff confundido com cabeçalho.**

   Contraexemplo: dentro de `<script>`, trocar `--saldo;` por `++saldo;`. As linhas do diff são `---saldo;` e `+++saldo;`; ambas são descartadas como cabeçalho. Nenhuma chega à verificação de faixa de script, e a classe sai `so leitor`. O descarte também deixa de avançar a numeração dessas linhas. (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:149`, `bin/classe_da_obra.py:153`, `bin/classe_da_obra.py:174`, `bin/classe_da_obra.py:181`, `bin/classe_da_obra.py:215`)

3. **[bloqueia] `bin/disjuntor.py:1025` — retrocesso de relógio vira zero silencioso.**

   Contraexemplo: `ini=10:00:00`, seguido no arquivo por `fim=09:59:59`, rc `0`, duração `0`, sem triagem. O `pop` remove a ocorrência, a comparação rejeita o par, e a CLI devolve rc `0`, em vez de `None`/rc `4`. O algoritmo anterior ainda apontaria pendência; aqui há afrouxamento do delta. (`bin/disjuntor.py:1024`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1045`, `bin/disjuntor.py:1052`)

   Duração zero não impede esse histórico de ser produzido: `T0C` é capturado antes do `ini`. Se `T0C=09:59:59`, o `ini` cruza para `10:00:00` e o relógio recua para `09:59:59`, o trap pode escrever fim anterior e duração zero, aceita pelo parser. Produção desse cenário `[nao verificado]`. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:273`, `bin/codex_limite_estado.py:69`)

4. **[bloqueia] `bin/disjuntor.py:1031` — uma triagem pode quitar duas ocorrências.**

   Contraexemplo, mesma obra/hash/pid: ocorrência A em `[10:00,10:05]`, ocorrência B em `[10:05,10:10]`; apenas A está triada, com fim `10:05`. O intervalo inclusivo aceita esse mesmo instante para ambas e devolve zero. A linha não é consumida nem vinculada univocamente à ocorrência. Com relógio recuado e intervalos sobrepostos, o problema também ocorre longe da borda. (`bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1052`)

   Há ainda aceitação de instante inválido: terceira parte `2026-11-12T10:03:XX` satisfaz textualmente um intervalo `10:00..10:05`. `_triagens` valida a estrutura externa, mas não esse instante; o vigia corta e compara a string sem parse. O escritor normal gera formato válido; corrupção real `[nao verificado]`. (`bin/disjuntor.py:974`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`, `bin/codex_pergunta.sh:464`)

O rastreio dos campos responde à pergunta 1:

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| `ini/fim`, obra, papel, hash12, pid, sessão, rc, duração | Wrapper: `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:305` | Parser único: `bin/codex_limite_estado.py:63`; chave sem papel em `bin/codex_limite_estado.py:74`. |
| `eventos` | Lista local preenchida pelo parser: `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:82` | Pareamento por ordem do arquivo: `bin/disjuntor.py:1020`. Não é uma nova trilha persistida. |
| “fim legível” | Predicado local: hora parseável, quebra final, tipo, campos mínimos, rc inteiro e duração com dígitos. `bin/codex_limite_estado.py:68` | Alimenta `ilegiveis`; o vigia transforma lista não vazia em `None`. `bin/codex_limite_estado.py:70`, `bin/disjuntor.py:1017`. |
| Fim do sidecar | Wrapper antes do EXIT/trap: `bin/codex_pergunta.sh:464`, `bin/codex_pergunta.sh:472`, `bin/codex_pergunta.sh:282` | `_le_id` e composição da volta: `bin/disjuntor.py:886`, `bin/disjuntor.py:928`. |
| Terceiro campo da volta, `hash12@pid@fim@sha12` | Composto e gravado por `--triagem`: `bin/disjuntor.py:928`, `bin/disjuntor.py:939` | B3 conserva a volta completa como identidade; vigia interpreta o terceiro componente como instante pertencente a intervalo. `bin/disjuntor.py:976`, `bin/disjuntor.py:985`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`. |
| Hora da própria linha de triagem | `datetime.now()`: `bin/disjuntor.py:939` | `_triagens` não usa esse primeiro campo para casar ocorrências. `bin/disjuntor.py:973`, `bin/disjuntor.py:976`. |
| Classe do template/obra | Decisão por arquivo e máximo pela precedência: `bin/classe_da_obra.py:212`, `bin/classe_da_obra.py:240` | Publicada pela CLI, consultada pelo portão e pelos testes. `bin/classe_da_obra.py:334`, `bin/classe_da_obra.py:277`, `bin/tests/test_gate_pela_classe.sh:70`. |

**Os significados não estão inteiramente alinhados.** As horas escritas são instantes com precisão de segundos, sem offset; o parser produz `datetime` sem fuso, enquanto o vigia compara strings. Uniformidade do fuso entre execuções `[nao verificado]`. O corte `desde` inclui igualdade, e o casamento da triagem inclui ambas as bordas. A identidade completa usada pelo B3 não equivale ao casamento temporal usado pelo vigia, como demonstra o achado 4. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:464`, `bin/codex_limite_estado.py:65`, `bin/disjuntor.py:1029`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:985`)

Quanto aos históricos pedidos na pergunta 3:

| Histórico construído | Resultado do código |
|---|---|
| `ini` sem `fim` | Não conta: pertence ao universo ainda não concluído. `bin/disjuntor.py:990`, `bin/disjuntor.py:1028`. |
| Dois `ini` consecutivos da mesma chave, depois `fim` | O segundo substitui o primeiro e recebe o fim. `bin/disjuntor.py:1022`, `bin/disjuntor.py:1024`. |
| `fim` de outra chave entre `ini` e `fim` | Não consome a primeira chave. `bin/disjuntor.py:1024`. |
| `fim` anterior ao `ini` | Descarta o par e pode retornar zero: achado 3. `bin/disjuntor.py:1025`, `bin/disjuntor.py:1052`. |

**O ramo template não afrouxa em execução normal:** o retorno anterior era leitor; agora é leitor ou escritor, e a agregação escolhe o máximo. Isso não elimina os falsos leitores dos achados 1–2. (`bin/classe_da_obra.py:214`, `bin/classe_da_obra.py:215`, `bin/classe_da_obra.py:240`)

5. **[fila] `bin/classe_da_obra.py:350` — existe afrouxamento excepcional para obra combinada com dinheiro.**

   Contraexemplo com falha Git injetada em memória: a obra contém arquivo `app/folha/...`, já classificado como dinheiro, e template; a nova leitura do diff desse template lança `Erro`. A CLI devolve `escritor`, rc `3`; com o retorno anterior do ramo template, devolve `dinheiro`, rc `0`. É consequência da política existente “erro = escritor”, agora alcançável por uma operação adicional do delta. Falha equivalente em produção `[nao verificado]`. (`bin/classe_da_obra.py:187`, `bin/classe_da_obra.py:137`, `bin/classe_da_obra.py:75`, `bin/classe_da_obra.py:352`, `bin/classe_da_obra.py:353`)

**A exigência numérica não muda os números dos outros leitores, na pergunta 4.** `legivel` somente alimenta `ilegiveis`; os ramos que montam `inis/fins` continuam sem consultar esse predicado. Permanecem iguais `ler_chamadas`, `ultimas_chamadas`, `sessoes_do_code`, `tokens_por_papel` e `_censo_24h`. As derivações de origem e tokens também recebem os mesmos retornos legados. (`bin/codex_limite_estado.py:70`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:79`, `bin/codex_limite_estado.py:91`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:109`, `bin/codex_limite_estado.py:204`, `bin/disjuntor.py:2254`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2156`)

6. **[fila] `bin/codex_limite_estado.py:69` — duração emitida pode ser considerada ilegível.**

   O escritor calcula duração por subtração de relógio civil; um recuo pode produzir duração negativa, rejeitada por `isdigit()`. Nesse caso o vigia devolve `None`, conservadoramente; não é o falso zero do achado 3. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:273`, `bin/disjuntor.py:1017`)

As mordidas dos casos novos, na pergunta 5:

| Caso novo | Mutante da fonte que a asserção elimina |
|---|---|
| Marcação fora do script | Classificar como escritor qualquer template que contenha script, ignorando a posição da linha. `bin/tests/test_gate_pela_classe.sh:139`, `bin/classe_da_obra.py:179`. |
| Divisor dentro do script | Remover as faixas de script; M8 exercita explicitamente esse mutante. `bin/tests/test_gate_pela_classe.sh:140`, `bin/tests/test_gate_pela_classe.sh:143`, `bin/tests/test_gate_pela_classe.sh:144`. |
| Campo `name=` | Desativar `FORM.search`; M9 exercita explicitamente esse mutante. `bin/tests/test_gate_pela_classe.sh:141`, `bin/tests/test_gate_pela_classe.sh:145`, `bin/tests/test_gate_pela_classe.sh:146`. |
| Static `.js` | Desativar o ramo incondicional `.js`. `bin/tests/test_gate_pela_classe.sh:142`, `bin/classe_da_obra.py:171`. |
| PID reutilizado, exatamente uma pendência nova | Restaurar o casamento pelo último `fins[chave]`: produziria duas pendências. `bin/tests/test_triagem_pendente.sh:61`, `bin/codex_limite_estado.py:80`. |
| Inverso: nova triada, velha pendente | Mesmo mutante anterior: produziria zero. `bin/tests/test_triagem_pendente.sh:68`. |
| Sidecar um segundo antes | Trocar pertinência ao intervalo por igualdade dos fins. `bin/tests/test_triagem_pendente.sh:76`, `bin/disjuntor.py:1031`. |
| Rc/duração vazios; rc `abc` | Remover a exigência numérica: deixaria de retornar rc `4` nesses três casos. `bin/tests/test_triagem_pendente.sh:79`, `bin/tests/test_triagem_pendente.sh:83`, `bin/codex_limite_estado.py:69`. |

7. **[fila] `bin/tests/test_gate_pela_classe.sh:139` — há mutantes que passam em falso.**

   Trocar o `yield` do lado removido por `pass` preserva os quatro resultados novos, mas faz a remoção isolada de `<input name="valor">` sair `so leitor`. Os novos fixtures usam substituições, cujo lado adicionado ainda denuncia script/formulário. Tampouco exercitam tags Django ou a colisão `---`/`+++` dos bloqueios acima. (`bin/classe_da_obra.py:148`, `bin/tests/test_gate_pela_classe.sh:61`, `bin/tests/test_gate_pela_classe.sh:62`, `bin/tests/test_gate_pela_classe.sh:63`, `bin/tests/test_gate_pela_classe.sh:64`)

8. **[fila] `bin/tests/test_triagem_pendente.sh:79` — as novas asserções não fecham as classes de falha.**

   Sobrevivem: trocar `p[8].isdigit()` por `bool(p[8])`, aceitando duração `abc`; usar chave global no pareamento, pois os fixtures são seriais; usar `setdefault` para conservar o primeiro de dois `ini`; comparar horas só até minuto. Os casos novos não cobrem duração não numérica preenchida, intercalação de chaves, dois inícios consecutivos, retrocesso ou compartilhamento de borda. (`bin/codex_limite_estado.py:69`, `bin/disjuntor.py:1022`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1031`, `bin/tests/test_triagem_pendente.sh:18`, `bin/tests/test_triagem_pendente.sh:55`, `bin/tests/test_triagem_pendente.sh:79`)

9. **[fila] `bin/pre-push.sh:147` — grep corrigido, sem mordida correspondente nos casos novos.**

   O emissor escreve no stderr; `_teste_irmao` reúne as saídas e agora aceita `.CATRACA-AVISO:`. Reintroduzir a âncora não afetaria os novos casos de classificação/triagem. O filtro continua apenas imprimindo: o retorno é `_rc`. Destino do aviso e truncamento por `tail -12` permanecem na fila anterior. (`app/core/tests/test_medida_turno_do_par.py:467`, `bin/pre-push.sh:145`, `bin/pre-push.sh:147`, `bin/pre-push.sh:157`, `bin/tests/test_gate_pela_classe.sh:139`, `bin/tests/test_triagem_pendente.sh:59`)

No checklist da pergunta 6:

| Classe de reprovação | Resultado neste recorte |
|---|---|
| Segundo juiz duplicado | O delta continua usando o leitor único de chamadas e `_triagens`; a divergência nova é o significado de identidade versus intervalo, bloqueio 4. `bin/disjuntor.py:1003`, `bin/disjuntor.py:1014`, `bin/disjuntor.py:1031`. |
| Segundo escritor fora da porta | `eventos` é local; a escrita persistida continua em `--triagem`, sob a porta existente. `bin/codex_limite_estado.py:78`, `bin/disjuntor.py:938`, `bin/disjuntor.py:944`. |
| Relógio fora da borda | O problema demonstrado é o uso dos instantes persistidos sem tratamento suficiente de recuo/ambiguidade: bloqueios 3–4. `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`. |
| Derivação paralela de fonte única | Não nasce outro parser de chamadas; nasce uma associação temporal incapaz de garantir uma triagem por ocorrência. `bin/disjuntor.py:1014`, `bin/disjuntor.py:1031`. |
| Selo/RED sem mordida suficiente | Lacunas concretas nos achados 7–9. `bin/tests/test_gate_pela_classe.sh:139`, `bin/tests/test_triagem_pendente.sh:79`, `bin/pre-push.sh:147`. |
| Ordem de trava | A porta existente adquire a trava da resposta antes da triagem e usa a trava do estado para ler/acrescentar. `bin/disjuntor.py:912`, `bin/disjuntor.py:915`, `bin/disjuntor.py:944`. |
| Pouso antes do pré-requisito | O portão continua exigindo tree certificada; a insuficiência de `_limpa` permanece fila anterior. `bin/classe_da_obra.py:279`, `bin/classe_da_obra.py:301`, `bin/classe_da_obra.py:340`. |
| Campo com dois significados | Confirmado no terceiro componente da volta: identidade integral no B3, pertinência temporal no vigia. `bin/disjuntor.py:985`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`. |

A fila anterior não foi promovida a bloqueio novo: autoria de `ERRO-BOBO`, exclusões por caminho, ignorados em `_limpa`, mutantes combinados M2/M3/M4, vigia com `{}` e texto de LEIS continuam nos respectivos pontos. (`bin/disjuntor.py:772`, `bin/classe_da_obra.py:186`, `bin/classe_da_obra.py:301`, `bin/tests/test_gate_pela_classe.sh:120`, `bin/disjuntor.py:1003`, `app/docs/LEIS.md:66`)

**Veredito final: REPROVADO.** (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:175`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`)

## 14/11 01:39 · sessao 3bde72701314 · - · - · origem=Ronald (tmux)

Achados no delta:

- **[bloqueia] `bin/disjuntor.py:1024-1026` — fim anterior ao ini desaparece como zero.** `pop` consome a ocorrência; a condição descarta o par e `[]` vira rc 0 em `bin/disjuntor.py:1052`. Contraexemplo sintético confirmado em memória: ini `10:00:00`, fim rc 0/dur 0 `09:59:59`, trilha vazia → `[]`. Esse formato pode sair do escritor: `T0C` é capturado antes do ini (`bin/codex_pergunta.sh:265,305`); T0C `09:59:59`, ini `10:00:00`, relógio recua, fim e leitura de epoch `09:59:59` produzem duração 0 (`bin/codex_pergunta.sh:272-273`). Produção desse cenário: `[nao verificado]`. O algoritmo anterior contaria pendência; o delta afrouxa esse caso.

- **[bloqueia] `bin/disjuntor.py:1031` — uma triagem quita várias ocorrências.** `any` não consome nem vincula univocamente a linha, e os intervalos são inclusivos. Contraexemplo sintético confirmado: mesma chave, ocorrências `10:00..10:05` e `10:05..10:10`; somente primeira triada em `10:05` → `[]`. Com recuo de relógio, primeira `10:00..10:05`, segunda `10:03..10:06`, única triagem `10:04:59` também quita ambas. O escritor conserva identidade completa `hash@pid@fim@sha` (`bin/disjuntor.py:928`); o vigia reduz a chave e aceita pertença temporal (`bin/disjuntor.py:1011,1031`).

- **[bloqueia ou agrupável ao anterior] `bin/disjuntor.py:1011,1031` — instante não é validado antes da comparação.** `_triagens` aceita qualquer identificador não vazio (`bin/disjuntor.py:974-976`). Contraexemplo sintético: terceira parte `2026-11-12T10:03:XX`, ini `10:00`, fim `10:05` → considerada triada pela comparação textual. Antes, a igualdade com o fim normal não casava. O escritor normal produz formato válido (`bin/codex_pergunta.sh:464`); corrupção em produção `[nao verificado]`.

Comportamentos examinados:

| Histórico sintético | Resultado | Referência |
|---|---|---|
| Ini sem fim | `[]`, coerente com universo concluído | `bin/disjuntor.py:990,1019-1033` |
| Dois ini consecutivos da mesma chave, depois fim | Somente o segundo pareia | `bin/disjuntor.py:1022-1026` |
| Fim de outra chave entre ini e fim | Não consome a ocorrência da primeira chave | `bin/disjuntor.py:1024` |
| Fim anterior ao ini | `[]`, problema bloqueante acima | `bin/disjuntor.py:1025` |

Escritores e leitores:

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| Ini/fim, chave, rc, duração | `bin/codex_pergunta.sh:272-273,305-306` | `bin/codex_limite_estado.py:63-82`; eventos por ordem do arquivo, `bin/disjuntor.py:1020-1026` |
| Fim do sidecar | `bin/codex_pergunta.sh:464-466` | `_le_id`, `bin/disjuntor.py:879-886`; compõe volta, `bin/disjuntor.py:928` |
| Terceira parte da volta | `bin/disjuntor.py:928,939-940` | Identidade completa no B3, `bin/disjuntor.py:976,984-985`; intervalo inclusivo no vigia, `bin/disjuntor.py:1011,1031` |
| Hora da própria triagem | `bin/disjuntor.py:939` | `_triagens` não valida nem usa esse primeiro campo temporal, `bin/disjuntor.py:973-976` |

Os escritores usam hora local textual sem offset (`bin/codex_pergunta.sh:272,305,464`; `bin/disjuntor.py:939`); o parser cria `datetime` sem fuso (`bin/codex_limite_estado.py:65`) e o vigia compara strings (`bin/disjuntor.py:1025,1029,1031`). Uniformidade de fuso entre execuções em produção: `[nao verificado]`.

Selos novos:

| Caso | Mutante que a asserção reprova | Mutante relevante que sobrevive |
|---|---|---|
| PID reutilizado, contagem exata | Algoritmo anterior com último `fins[chave]`: produziria duas pendências | Pareamento com chave global fixa: os históricos da suíte são seriais | [nao verificado]
| Inverso, nova triada/velha pendente | Mesmo algoritmo anterior: produziria zero | Mesmo mutante de chave global |
| Sidecar 1 s antes | Igualdade exata com fim do chamadas.log | Comparar instantes somente até minuto; a suíte continua verde | [nao verificado]
| Rc/duração vazios; rc `abc` | Remover as novas condições numéricas | Duração `.isdigit()` → `bool(p[8])`: nenhuma fixture testa duração não vazia e não numérica |

Referências das asserções: `bin/tests/test_triagem_pendente.sh:59-83`; fixtures seriais: `bin/tests/test_triagem_pendente.sh:18-28,55-56,81`. A suíte não executa explicitamente mutantes da triagem (`bin/tests/test_triagem_pendente.sh:59-83`). Outro sobrevivente é trocar a atribuição por `aberto.setdefault(...)`: passa os casos atuais, mas pareia o primeiro de dois ini consecutivos (`bin/disjuntor.py:1022`).

**Veredito deste recorte: REPROVADO**, pelos falsos zeros em `bin/disjuntor.py:1025,1031`.

## 14/11 01:39 · sessao abc6b9967f16 · - · - · origem=Ronald (tmux)

[bloqueia] `bin/classe_da_obra.py:175` e `bin/classe_da_obra.py:181`: detectar tokens na linha alterada não prova “só marcação”. Contraexemplo real, mutação hipotética em `app/templates/chamados/detalhe.html:134`: `{% opcao_mover "fechado" %}` → `{% opcao_mover "resolvido" %}` retorna `so leitor`, reproduzido em memória. A tag escreve `<option value=status_local>` em `app/chamados/templatetags/chamados_tags.py:77`; o campo `novo_status` está em `app/templates/chamados/detalhe.html:130`, é lido em `app/chamados/services/acoes_chamado.py:296` e determina a transição em `app/chamados/services/acoes_chamado.py:325`. A cura não fecha a classe de controles gerados por template.

[bloqueia] `bin/classe_da_obra.py:144`: o leitor confunde conteúdo do diff com cabeçalho. Contraexemplo sintético reproduzido em memória: dentro de `<script>`, substituir `--saldo;` por `++saldo;` produz linhas `---saldo;` e `+++saldo;`; ambas são descartadas. `_linhas_numeradas` entrega `[]`, `_template_escreve` devolve `None` em `bin/classe_da_obra.py:181` e a classe sai `so leitor` em `bin/classe_da_obra.py:215`. O descarte também deixa de incrementar os contadores usados em `bin/classe_da_obra.py:149` e `bin/classe_da_obra.py:153`.

[fila] `bin/tests/test_gate_pela_classe.sh:139`: os quatro casos novos não cobrem remoção nem demonstram leitura dos dois lados. Mutante concreto: trocar `yield '-', nb, linha[1:].strip()` por `pass` em `bin/classe_da_obra.py:148`. Em memória, ele conserva os quatro resultados novos e deixa a remoção isolada de `<input name="valor">` sair `so leitor`; HEAD devolve `escritor`. Os fixtures novos apenas substituem conteúdo em `bin/tests/test_gate_pela_classe.sh:61`.

Mapa das mordidas novas:

| Caso | Mutante que deve reprovar | Evidência |
|---|---|---|
| Marcação fora do script | Rejeitar qualquer arquivo que contenha script | `bin/tests/test_gate_pela_classe.sh:139`; seleção por linha em `bin/classe_da_obra.py:179` |
| Divisor dentro do script | Remover os intervalos de script | Mutante M8 em `bin/tests/test_gate_pela_classe.sh:143` e conferência em `bin/tests/test_gate_pela_classe.sh:144` |
| Campo `name=` | Desativar `FORM.search` | Mutante M9 em `bin/tests/test_gate_pela_classe.sh:145` e conferência em `bin/tests/test_gate_pela_classe.sh:146` |
| Static `.js` | Desativar o ramo `.js` | Caso em `bin/tests/test_gate_pela_classe.sh:142`; ramo em `bin/classe_da_obra.py:171`; mutante incorporado para esse ramo `[nao verificado]` |

O delta do ramo template é monotônico quando termina normalmente: antes retornava leitor; agora retorna leitor ou escritor em `bin/classe_da_obra.py:212`, `bin/classe_da_obra.py:214` e `bin/classe_da_obra.py:215`; a agregação escolhe a maior classe em `bin/classe_da_obra.py:240`. Afrouxamento normal causado por esse delta `[nao verificado]`.

A classe é calculada em `bin/classe_da_obra.py:238`, agregada em `bin/classe_da_obra.py:241` e publicada no stdout em `bin/classe_da_obra.py:334`. O portão consulta a classificação em `bin/classe_da_obra.py:277`; o selo lê o stdout, sem preservar o rc do Python, em `bin/tests/test_gate_pela_classe.sh:70`. O próprio instrumento declara não estar conectado à conduta em `bin/classe_da_obra.py:37`.

**Veredito deste recorte: REPROVADO**, pelos dois escapes do ramo novo em `bin/classe_da_obra.py:144` e `bin/classe_da_obra.py:175`.

## 14/11 01:38 · sessao 8f71c0724121 · - · - · origem=Ronald (tmux)

Q4: **nenhum número dos consumidores antigos muda por causa do novo teste numérico**. `legivel` só acrescenta à lista opcional `ilegiveis`; a montagem de `inis/fins` continua aceitando os mesmos registros que antes (`bin/codex_limite_estado.py:68-82`). Consumidores:

| Consumidor | Leitura | Efeito do delta |
|---|---|---|
| `ler_chamadas` | `bin/codex_limite_estado.py:91` | Nenhum; conta `ini` (`:92-93`). |
| `ultimas_chamadas` | `bin/codex_limite_estado.py:99` | Nenhum; mantém último fim por chave (`:102-103`). |
| `sessoes_do_code` | `bin/codex_limite_estado.py:109` | Nenhum; alimenta origem em `bin/gerar_codex_estado.py:176`. |
| `tokens_por_papel` | `bin/codex_limite_estado.py:204` | Nenhum; alimenta `bin/disjuntor.py:2156`. |
| `_censo_24h` | `bin/disjuntor.py:2254` | Nenhum; usa apenas `ini` (`:2259-2264`). |
| `triagem_pendente` | `bin/disjuntor.py:1014` | Novo consumidor de `eventos`; retorna `None` quando `ilegiveis` não vazia (`:1017-1018`). |

Confirmei em Python `-B`, inteiramente em memória, que os retornos legados permanecem iguais para rc/duração válidos, vazios, rc `abc` e duração `-1`; a distinção nova aparece somente na lista `ilegiveis` (`bin/codex_limite_estado.py:68-82`). Ocorrência desses valores em produção: `[nao verificado]`.

**Escritor versus leitor.** O wrapper escreve rc a partir de `$?`, duração como `date +%s - T0C`, e hora com `date '+%Y-%m-%d %H:%M:%S'` (`bin/codex_pergunta.sh:265-273`); escreve o início no mesmo formato (`:305-306`). O leitor converte a hora para `datetime` sem fuso, conserva a string original e mantém rc/duração como strings (`bin/codex_limite_estado.py:65-80`). O fuso real dos processos é `[nao verificado]`; não há offset nos campos escritos (`bin/codex_pergunta.sh:272`, `:305`).

**[fila] `bin/codex_limite_estado.py:69` — duração produzida pelo escritor pode ser considerada ilegível.** Um retrocesso do relógio pode tornar negativa a subtração feita pelo wrapper (`bin/codex_pergunta.sh:265`, `:273`); `'-1'.isdigit()` rejeita esse fim, levando o vigia a `None` (`bin/disjuntor.py:1017-1018`). É conservador, sem zero silencioso; ocorrência real `[nao verificado]`.

**Selos novos da triagem:**

| Caso novo | Mutante que deve reprovar |
|---|---|
| Somente nova ocorrência pendente, exatamente uma linha (`bin/tests/test_triagem_pendente.sh:59-62`) | Restaurar uso do último `fins[chave]`: atribui o fim novo às duas ocorrências, produzindo duas pendências. O armazenamento do último fim está em `bin/codex_limite_estado.py:80`. |
| Inverso: nova triada, velha pendente (`bin/tests/test_triagem_pendente.sh:64-69`) | Mesmo mutante: a triagem do fim novo cobre as duas entradas, produzindo zero; o código atual pareia ocorrências em `bin/disjuntor.py:1019-1027`. |
| Sidecar um segundo antes do trap (`bin/tests/test_triagem_pendente.sh:72-76`) | Trocar pertinência ao intervalo por igualdade entre os fins, em `bin/disjuntor.py:1032`. |
| rc/duração vazios e rc `abc` (`bin/tests/test_triagem_pendente.sh:79-83`) | Remover a validação numérica de `bin/codex_limite_estado.py:69`; deixa de retornar rc 4. |

**[fila] `bin/tests/test_triagem_pendente.sh:79` — falta duração não numérica preenchida.** Mutante sobrevivente concreto: trocar `p[8].isdigit()` por `bool(p[8])` em `bin/codex_limite_estado.py:69`. Os três casos novos continuam ilegíveis, mas `fim ... sessX 0 abc` passa como legível. Confirmei essa sobrevivência do predicado em memória; o caso ausente é visível na lista de fixtures (`bin/tests/test_triagem_pendente.sh:79`).

**[fila] `bin/pre-push.sh:147` — correção do grep sem selo novo correspondente.** O emissor escreve `CATRACA-AVISO:` no stderr (`app/core/tests/test_medida_turno_do_par.py:465-467`), e o consumidor une as saídas e agora reconhece o aviso com prefixo de progresso (`bin/pre-push.sh:145-147`). Reintroduzir `^CATRACA-AVISO:` não afeta os novos casos dos dois testes do delta, que exercitam classificação e triagem, sem chamar `_teste_irmao` (`bin/tests/test_gate_pela_classe.sh:139-146`; `bin/tests/test_triagem_pendente.sh:59-83`). O limite `tail -12` continua podendo omitir avisos anteriores; já existia no delta fornecido, portanto permanece fila (`bin/pre-push.sh:147`). O retorno continua sendo o rc da execução, sem novo julgamento pelo filtro (`:157`).

## 14/11 01:29 · sessao cec8d1e8f17c · CORTE-DAS-TRAVAS · verificador · origem=Code

**REPROVADO.** As curas deixam contraexemplos em #1/#2, #6 e #9. (`bin/classe_da_obra.py:156`; `bin/disjuntor.py:1021`; `bin/pre-push.sh:147`)

As reproduções abaixo usaram entradas simuladas em memória. Ocorrência desses casos em produção e identidade Git da cópia: **[nao verificado]**.

1. **[bloqueia] #1/#2 — template que altera dinheiro continua “so leitor”.**

   Há um contraexemplo concreto na cópia: o campo monetário chama `mascararValor`, que divide a entrada por `100` e devolve o resultado ao campo enviado. Trocar `/100` por `/10` altera o valor recebido pelo backend e encaminhado à gravação do benefício. Mesmo assim, esse arquivo continua “so leitor”: o ramo de templates não examina conteúdo. (`app/templates/colaboradores/beneficio_form.html:36`; `app/templates/colaboradores/beneficio_form.html:70`; `app/colaboradores/views.py:1777`; `app/colaboradores/views.py:1797`; `app/colaboradores/services/beneficios.py:11`; `bin/classe_da_obra.py:156`)

   A cura fecha os dois exemplos `.py` apresentados: constante sem token e comparador passam a **escritor**. `.sql` e `.json` comuns também caem em escritor; migrations têm ramo próprio. O buraco remanescente é a presunção de leitura por caminho. (`bin/classe_da_obra.py:138`; `bin/classe_da_obra.py:155`; `bin/classe_da_obra.py:158`; `bin/tests/test_gate_pela_classe.sh:130`)

2. **[bloqueia] #6 — PID reutilizado ainda produz zero falso.**

   `fins` conserva somente o último fim por `(obra, hash, pid)`. Depois, o vigia aplica esse último fim a **todos** os inícios com essa chave. Reprodução: duas chamadas às 10h e 15h, somente a das 15h triada → resultado `[]`; desaparece a pendência das 10h. (`bin/codex_limite_estado.py:70`; `bin/codex_limite_estado.py:74`; `bin/disjuntor.py:1021`; `bin/disjuntor.py:1024`)

   O selo novo aceita esse defeito: exige `rc=1` e presença da chamada nova, sem conferir cardinalidade. No cenário do próprio selo, a chamada antiga já triada também aparece pendente. Falta ainda o sentido inverso, antiga pendente/nova triada, que zera indevidamente. (`bin/tests/test_triagem_pendente.sh:28`; `bin/tests/test_triagem_pendente.sh:54`; `bin/tests/test_triagem_pendente.sh:58`; `bin/disjuntor.py:1021`)

3. **[bloqueia] #6 — os dois campos “fim” vêm de consultas distintas ao relógio.**

   O sidecar captura `date` antes do encerramento; o trap captura outro `date` para `chamadas.log`. A triagem usa o primeiro, mas o vigia exige igualdade com o segundo. Reprodução: sidecar `15:04:59`, chamadas `15:05:00` → resposta legitimamente triada permanece pendente. (`bin/codex_pergunta.sh:464`; `bin/codex_pergunta.sh:472`; `bin/codex_pergunta.sh:272`; `bin/disjuntor.py:928`; `bin/disjuntor.py:1024`)

   O formato nominal é compatível: o escritor troca espaço por `T`, e o vigia desfaz essa transformação. O defeito é tratar duas amostras temporais como identidade única. (`bin/disjuntor.py:928`; `bin/disjuntor.py:1009`)

4. **[bloqueia] #6 — linha longa, mas malformada, ainda pode virar zero.**

   Um `fim` com hora válida, nove campos, `rc` vazio ou `zero` e duração vazia recebe `ilegiveis=[]`. O vigia o descarta como `rc != '0'` e devolve zero. A validação cobre quantidade de campos, tipo, hora e quebra final, mas não valida os valores de `rc`/duração. É resíduo do achado anterior, não afrouxamento novo. (`bin/codex_limite_estado.py:64`; `bin/codex_limite_estado.py:74`; `bin/disjuntor.py:1022`)

5. **[bloqueia] #9 — aviso precedido pelos pontos do runner continua sendo comido.**

   `_aviso` não inicia uma linha nova; o grep exige `^CATRACA-AVISO:`. A reprodução com o emissor real e `TextTestRunner(verbosity=1)` produz `.CATRACA-AVISO: ...`, eliminado pelo filtro, enquanto `Ran` e `OK` permanecem. As chamadas do pre-push não especificam verbosity. Ocorrência concreta com os workers de produção: **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:467`; `bin/pre-push.sh:145`; `bin/pre-push.sh:147`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`)

As demais curas ficam assim:

| Achado | Resultado por peça |
|---|---|
| **#3** | Fecha untracked comum com `--untracked-files=all`; caso 4 verifica arquivo solto, rastreado modificado e recusa antecipada do pouso. (`bin/classe_da_obra.py:243`; `bin/tests/test_gate_pela_classe.sh:175`; `bin/tests/test_gate_pela_classe.sh:192`) |
| **#4** | Fecha a descida de A1/B1: descida avisa, subida reprova; os totais preservam teto independente. Os dois testes novos passam e detectam, respectivamente, restaurar igualdade e retirar o teto. (`app/core/tests/test_medida_turno_do_par.py:503`; `app/core/tests/test_medida_turno_do_par.py:514`; `app/core/tests/test_medida_turno_do_par.py:625`; `app/core/tests/test_medida_turno_do_par.py:634`) |
| **#5 [fila/contestável]** | A porta de escrita continua `--triagem`; o wrapper apenas fornece o comando. A equivalência byte a byte da cópia de auditoria é **[nao verificado]**. (`bin/disjuntor.py:938`; `bin/codex_pergunta.sh:469`) |
| **#7 [fila]** | Continua sem autoria verificada: o ramo ERRO-BOBO aceita obra e motivo, sem identidade do autor. (`bin/disjuntor.py:759`; `bin/disjuntor.py:772`) |
| **#8** | R0 fecha a diferença de caixa; os dois exemplos novos acusam R0, e retirar `re.I` faz o caso falhar. O pouso ganhou cobertura funcional; seu mutante novo testa especificamente ignorar untracked. (`bin/tests/test_disjuntor.sh:1336`; `bin/tests/test_disjuntor.sh:1382`; `bin/tests/test_disjuntor.sh:1389`; `bin/tests/test_gate_pela_classe.sh:178`) |
| **#10** | Fecha a afirmação incorreta no comentário do wrapper. A redação residual “pid/arquivo” permanece na documentação gerada, como fila. (`bin/codex_pergunta.sh:51`; `app/docs/LEIS.md:66`) |

**Não identifiquei afrouxamento de dinheiro/escritor para classe inferior no delta:** o retorno alterado sobe de leitor para escritor; `_limpa` amplia a recusa. Em `_criterios`, a flexibilização fica nas contagens abaixo do registro; subida continua vermelha. (`bin/classe_da_obra.py:155`; `bin/classe_da_obra.py:243`; `app/core/tests/test_medida_turno_do_par.py:520`)

**[fila]** A exclusão por caminho ainda antecede dinheiro/JUIZES: `app/docs/`, `/tests/` e basename `test_` recebem leitor. Existe inclusive função sob testes chamada por um command da aplicação; portanto esse caminho não prova ausência de uso operacional. (`bin/classe_da_obra.py:74`; `bin/classe_da_obra.py:79`; `bin/classe_da_obra.py:131`; `bin/classe_da_obra.py:159`; `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`)

**[fila]** “Árvore física = tree” continua mais amplo que a prova: `_limpa` exclui ignorados explicitamente, e a suite monta o diretório físico. Um pacote adicional sob `app/core/tests/media/` seria candidato a escapar do status; execução desse contraexemplo: **[nao verificado]**. (`bin/classe_da_obra.py:241`; `.gitignore:12`; `bin/suite.sh:230`)

Os **seis chamadores diretos** de `_linhas_chamadas` são:

| Chamador | Chamada | Efeito do delta |
|---|---|---|
| `ler_chamadas` | `bin/codex_limite_estado.py:83` | Não passa `ilegiveis`. |
| `ultimas_chamadas` | `bin/codex_limite_estado.py:91` | Não passa `ilegiveis`; lê `rc`. |
| `sessoes_do_code` | `bin/codex_limite_estado.py:101` | Não passa `ilegiveis`; lê sessão. |
| `tokens_por_papel` | `bin/codex_limite_estado.py:196` | Não passa `ilegiveis`; lê sessão. |
| `triagem_pendente` | `bin/disjuntor.py:1012` | Passa `ilegiveis` e consome o novo `fim['hora']`. |
| `_censo_24h` | `bin/disjuntor.py:2247` | Não passa `ilegiveis`; usa os inícios. |

`legivel=False` **não interrompe o processamento**. Assim, início cortado com sete campos e hora válida continua entrando nos retornos, embora seja também marcado ilegível; fim cortado com nove campos também entra. Tipo estranho e fim curto são ignorados. Os chamadores antigos mantêm esse comportamento; o vigia é quem transforma a lista de ilegíveis em `None`. A chave nova `hora` não altera os acessos explícitos antigos. (`bin/codex_limite_estado.py:64`; `bin/codex_limite_estado.py:68`; `bin/codex_limite_estado.py:95`; `bin/codex_limite_estado.py:103`; `bin/codex_limite_estado.py:200`; `bin/disjuntor.py:1015`)

O contrato entre escritores e leitores é:

| Estado/campo | Quem escreve e quem lê | Significado/borda |
|---|---|---|
| `chamadas.log`, hora do fim | Wrapper escreve; `_linhas_chamadas` lê. (`bin/codex_pergunta.sh:272`; `bin/codex_limite_estado.py:74`) | Hora local sem offset explícito, capturada no trap. Fuso efetivo: **[nao verificado]**. |
| `triagem.log`, terceiro campo da volta | Wrapper fornece `fim` no sidecar; `--triagem` monta/grava a volta; `_triagens` lê para B3 e vigia. (`bin/codex_pergunta.sh:464`; `bin/disjuntor.py:928`; `bin/disjuntor.py:939`; `bin/disjuntor.py:949`; `bin/disjuntor.py:1001`) | É o fim do sidecar, não o instante da triagem. O primeiro campo da linha é a hora da própria triagem. (`bin/disjuntor.py:939`) |
| Corte `--desde` | Vigia compara com o **início**, por string. (`bin/disjuntor.py:1019`) | Inclusivo na igualdade; depende do formato esperado. |
| `CRITERIOS_REGISTRADOS` | Constante local; `_criterios` lê; medida vem de `len(items)`. (`app/core/tests/test_medida_turno_do_par.py:108`; `app/core/tests/test_medida_turno_do_par.py:518`; `app/core/medida_turno_do_par.py:441`) | Contagem/teto inclusivo, sem fuso ou relógio. |
| `CATRACA-AVISO` | `_aviso` escreve stderr; pre-push junta stdout/stderr e filtra. (`app/core/tests/test_medida_turno_do_par.py:467`; `bin/pre-push.sh:145`) | O consumidor presume início de linha, que o produtor não garante. (`bin/pre-push.sh:147`) |

No checklist, **relógio fora da borda e campo com dois significados estão presentes** nos dois “fins”; não há identidade temporal compartilhada. O vigia reutiliza os leitores existentes e permanece sem escrita, mas o `{}` retira a garantia de detectar trilha encolhida contra `gravadas`, mantendo garantias diferentes para vigia e B3. (`bin/codex_pergunta.sh:272`; `bin/codex_pergunta.sh:464`; `bin/disjuntor.py:1001`; `bin/disjuntor.py:1012`; `bin/disjuntor.py:958`; `bin/disjuntor.py:977`)

**[fila] Selos:** M2/M3/M4 agora mordem duas alterações combinadas, não a remoção isolada da guarda anunciada no cabeçalho. Os selos novos de critérios e caixa de DESENHO mordem suas regressões; o de PID reutilizado não demonstra o pareamento correto, conforme o bloqueio acima. (`bin/tests/test_gate_pela_classe.sh:10`; `bin/tests/test_gate_pela_classe.sh:111`; `bin/tests/test_gate_pela_classe.sh:122`; `app/core/tests/test_medida_turno_do_par.py:625`; `bin/tests/test_disjuntor.sh:1389`; `bin/tests/test_triagem_pendente.sh:58`)

## 14/11 01:27 · sessao 666d8be00034 · - · - · origem=Ronald (tmux)

- **[bloqueia] #9 fecha só parte do aviso perdido.** `_aviso` escreve `CATRACA-AVISO:` sem quebra inicial; o filtro exige início da linha. Reproduzi em memória usando `_aviso` extraído por AST e `unittest.TextTestRunner(verbosity=1)`: saída `.CATRACA-AVISO: criterio A1 DESCEU…`; o grep exato elimina o aviso e conserva apenas `Ran`/`OK`. As chamadas do pre-push não definem verbosity; a execução concreta com workers em produção é [nao verificado]. Evidências: `app/core/tests/test_medida_turno_do_par.py:467`; `bin/pre-push.sh:145`; `bin/pre-push.sh:147`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`; `app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:72`; `app/core/runner_quarentena.py:93`.

- **#4 fecha a cura declarada, sem afrouxamento adicional identificado.** A mudança troca igualdade por teto nas duas contagens, preserva reprovação de subida e gera aviso na descida; os totais continuam com teto independente. `CRITERIOS_REGISTRADOS` é constante local, escrita na declaração e lida por `_criterios` e pelos dois selos novos; os números medidos vêm de `len(items)`, sem significado temporal. Evidências: `app/core/tests/test_medida_turno_do_par.py:108`; `app/core/tests/test_medida_turno_do_par.py:503`; `app/core/tests/test_medida_turno_do_par.py:514`; `app/core/tests/test_medida_turno_do_par.py:574`; `app/core/medida_turno_do_par.py:441`.

- **Selos novos de #4 mordem.** Executei os dois testes extraídos em memória: atuais verdes; trocar `assertLessEqual` por `assertEqual` provoca dois subtestes vermelhos na descida; retirar efetivamente o teto provoca dois vermelhos na subida. Evidências dos testes e da asserção: `app/core/tests/test_medida_turno_do_par.py:520`; `app/core/tests/test_medida_turno_do_par.py:625`; `app/core/tests/test_medida_turno_do_par.py:634`.

- **#8, R0, fecha case sensitivity declarada e os mutantes novos mordem.** Executei `c9` extraído em memória: universo 4, nenhuma violação real e R0=1 para cada um dos cinco exemplos. Remover `re.I` de `_DESENHO_I` faz `c9` retornar falso. O universo e a lógica de R1/R2 antigos permanecem; o delta só amplia detecção. Evidências: `bin/tests/test_disjuntor.sh:1336`; `bin/tests/test_disjuntor.sh:1339`; `bin/tests/test_disjuntor.sh:1357`; `bin/tests/test_disjuntor.sh:1378`; `bin/tests/test_disjuntor.sh:1389`.

- **[fila] Restrição por parcela já existia:** A1 subir enquanto parcelas nomeadas descem pode manter o total e ainda reprovar `_criterios`; isso não é afrouxamento introduzido pelo delta, porque o `assertEqual` anterior também reprovaria. Evidências: `app/core/medida_turno_do_par.py:426`; `app/core/tests/test_medida_turno_do_par.py:520`; igualdade anterior no anexo `verif_v2.diff` [nao verificado como arquivo físico].

## 14/11 01:27 · sessao c488e1df051e · - · - · origem=Ronald (tmux)

Resultado da peça: **REPROVADO** — #6 não fecha; há dois contraexemplos confirmados em memória e um resíduo de validação. (`bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:74`, `bin/disjuntor.py:1021`)

- **[bloqueia] PID reutilizado ainda permite zero falso.** `fins` guarda só o último fim por `(obra, hash, pid)`; todos os `ini` dessa chave recebem esse fim. Com duas chamadas às 10h e 15h, triagem apenas da chamada das 15h, o vigia devolve `[]`: a pendência das 10h desaparece. (`bin/codex_limite_estado.py:70`, `bin/codex_limite_estado.py:74`, `bin/disjuntor.py:1021`, `bin/disjuntor.py:1024`)
- **[bloqueia] Os dois “fim” não são o mesmo instante.** O sidecar captura `date` antes da gravação e do encerramento; o trap captura outro `date` para `chamadas.log`. Se houver virada de segundo, a triagem legítima nunca casa. Reprodução em memória: sidecar `15:04:59`, chamadas `15:05:00` → volta pendente. O problema é identidade temporal; o formato nominal é compatível. (`bin/codex_pergunta.sh:464`, `bin/codex_pergunta.sh:471`, `bin/codex_pergunta.sh:472`, `bin/codex_pergunta.sh:272`, `bin/disjuntor.py:928`, `bin/disjuntor.py:1009`, `bin/disjuntor.py:1024`)
- **[bloqueia, resíduo #6] Linha estruturalmente longa continua podendo produzir zero falso.** Fim com hora válida, nove campos, `rc` vazio ou `zero` e duração vazia recebe `ilegiveis=[]`; o vigia ignora esse fim como `rc != '0'` e devolve zero. A validação não verifica os valores de `rc` e duração. Este comportamento já existia; a cura continua incompleta. (`bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:74`, `bin/disjuntor.py:1022`)
- **[bloqueia, selo sem mordida suficiente] O caso novo de PID reutilizado não exige a contagem correta.** Ele exige `rc=1` e presença da chamada nova, deixando passar a chamada antiga já triada também aparecer como pendente. Também falta o sentido inverso: antiga pendente/nova triada, que atualmente zera. (`bin/tests/test_triagem_pendente.sh:54`, `bin/tests/test_triagem_pendente.sh:57`, `bin/tests/test_triagem_pendente.sh:58`, `bin/disjuntor.py:1021`)

Os chamadores diretos de `_linhas_chamadas` são `ler_chamadas`, `ultimas_chamadas`, `sessoes_do_code`, `tokens_por_papel`, `triagem_pendente` e `_censo_24h`. Só o vigia fornece `ilegiveis`; os outros mantêm o comportamento anterior: `ini` sem quebra final ainda entra, tipo desconhecido é ignorado, fim curto é ignorado. A chave nova `hora` em `fins` é consumida pelo vigia; os outros acessam os campos antigos explicitamente. (`bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:91`, `bin/codex_limite_estado.py:101`, `bin/codex_limite_estado.py:196`, `bin/disjuntor.py:1012`, `bin/disjuntor.py:2247`, `bin/codex_limite_estado.py:68`, `bin/codex_limite_estado.py:95`, `bin/codex_limite_estado.py:103`, `bin/codex_limite_estado.py:200`)

`chamadas.log` é escrito pelo wrapper; o fim da volta em `triagem.log` vem do sidecar desse wrapper, transformado pelo escritor `--triagem`. Os formatos são locais, sem offset explícito; o fuso efetivo de produção é **[nao verificado]**. O corte do vigia é inclusivo pelo `ini`: `ini == desde` entra. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:464`, `bin/disjuntor.py:928`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1019`)

O `{}` é uma limitação declarada: o vigia não verifica redução da trilha contra o estado persistido e suas `gravadas`; B3 verifica. Isso não é um segundo escritor, mas perguntas com garantias diferentes sobre a mesma fonte. (`bin/disjuntor.py:996`, `bin/disjuntor.py:1001`, `bin/disjuntor.py:955`, `bin/disjuntor.py:958`, `bin/disjuntor.py:969`, `bin/disjuntor.py:977`)

## 14/11 01:27 · sessao 6d35a6323582 · - · - · origem=Ronald (tmux)

Achado principal **[bloqueia] #1/#2 parcialmente curados**: `app/templates/colaboradores/beneficio_form.html:70` transforma dinheiro (`/ 100`), devolvendo ao campo enviado em `:71`; o backend lê esse valor em `app/colaboradores/views.py:1777`, passa-o à porta em `:1797–1800`, que grava em `app/colaboradores/services/beneficios.py:11`. Trocar `/100` por `/10` permanece **“so leitor”**, pois o classificador aceita qualquer template/static elegível sem examinar seu conteúdo (`bin/classe_da_obra.py:156–157`). Reproduzi a classificação em memória, sem escrita.

Outras conclusões:

- **#1/#2 fecham os exemplos .py apresentados**: constante CCT e comparadores sem token passam a escritor pelo fallback (`bin/classe_da_obra.py:146–155`; testes e mutante em `bin/tests/test_gate_pela_classe.sh:130–134`). `.sql` e `.json` comuns também terminam escritor (`bin/classe_da_obra.py:158`); migrations recebem escritor antes (`:138–139`).
- **[fila] caminho não prova teste/doc**: exclusões ocorrem antes de dinheiro/JUIZES (`bin/classe_da_obra.py:74–79,131,159–160`). Contraexemplos sintéticos `app/core/test_calculo.py`, `app/docs/calculo.py` e `app/folha/tests/calculo.py` recebem leitor mesmo com código de cálculo; classificação reproduzida em memória. Há uso real em produção de código sob testes: `app/chamados/management/commands/tblib_ausente_na_imagem.py:17–18` chama função de `app/core/tests/test_contract_tblib_no_paralelo.py:24–30`, cujo arquivo recebe leitor pela mesma regra.
- **Não há enfraquecimento no classificador pelo delta**: o único retorno funcional alterado sobe leitor para escritor; a precedência dos demais ramos permanece anterior ao fallback (`bin/classe_da_obra.py:131–163`; delta fornecido).
- **#3 fecha untracked comum**: `_limpa` usa `--untracked-files=all` (`bin/classe_da_obra.py:243`). Caso4 verifica limpo, untracked, mutante que ignora untracked, rastreado sujo e recusa de pouso antes da suite (`bin/tests/test_gate_pela_classe.sh:175–192`).
- **[fila] “árvore física = tree” continua mais amplo que `_limpa`**: ela exclui ignorados explicitamente (`bin/classe_da_obra.py:241–243`); `.gitignore:12` ignora diretórios `media/` em qualquer nível. Um pacote adicional `app/core/tests/media/{__init__.py,test_extra.py}` ficaria fora do status, dentro da montagem física (`bin/suite.sh:204,230`) e fora do tmpfs específico `/app/media` (`bin/arvore_do_push.sh:81`). O runner usa `DiscoverRunner` e delega a descoberta (`app/core/runner_quarentena.py:72,76`; `app/config/settings/ci.py:40`). Execução desse contraexemplo não realizada: **[nao verificado]**.
- **[fila] mutantes M2/M3/M4 passaram a exigir duas alterações**: além de remover a guarda específica, restauram leitor no fallback (`bin/tests/test_gate_pela_classe.sh:111–128`). Isso está explícito no helper, mas contradiz o cabeçalho que promete uma única linha alterada (`:10–11`). A mordida agora prova a combinação, não a indispensabilidade isolada de JUIZES/universo (`:122–128`).
- **#8 pouso tem nova cobertura funcional e mutante para untracked** (`bin/tests/test_gate_pela_classe.sh:175–192`); não foi acrescentado mutante específico removendo a guarda de entrada de `pouso_inteira` (`:178–192`).

Nenhum script de selo foi executado, pois cria arquivos temporários (`bin/tests/test_gate_pela_classe.sh:25–26,31–46`).

## 14/11 01:24 · sessao 21bb236c1f22 · E3-FORMA-SQL · construtor · origem=Code

Entrega escrita, **ainda pendente de R2**: a hipótese simultânea retorna `colab`; a matriz exige `admin`. ([evidência](/home/ronald/wt-codex-e3-forma-sql-005806/app/e3-v8-r2-pendente.out:1))

Resultados sem banco:

- Base: `Ran 25; FAILED=14; ERROR=0`. ([RED](/home/ronald/wt-codex-e3-forma-sql-005806/app/e3-v8-red.out:208))
- Após alterações: `Ran 25; OK`; regressões: `Ran 70; OK`. ([GREEN](/home/ronald/wt-codex-e3-forma-sql-005806/app/e3-v8-green.out:28), [regressões](/home/ronald/wt-codex-e3-forma-sql-005806/app/e3-v8-regressoes.out:3))
- Mutantes: **42 + 15 mordidas por assert**, com comandos individuais na [tabela](/home/ronald/wt-codex-e3-forma-sql-005806/app/docs/e3_v8_matriz.txt:73).

Entregues a [matriz R1–R16](/home/ronald/wt-codex-e3-forma-sql-005806/app/docs/e3_v8_matriz.txt:8), o [ESMERIL de escritores/leitores](/home/ronald/wt-codex-e3-forma-sql-005806/app/docs/e3_v8_matriz.txt:194) e a [pré-verificação](/home/ronald/wt-codex-e3-forma-sql-005806/app/docs/e3_v8_matriz.txt:235).

Alcance ORM de R2/R3/R8, persistência zero ORM e R16: **[nao verificado]**. Falta a sessão executar:

```bash
bash ~/saas-hasner/bin/suite.sh \ [nao verificado]
  --dir /home/ronald/wt-codex-e3-forma-sql-005806 \
  --only "chamados core"
```

Sem declaração de pouso ou certificação. [nao verificado]

## 14/11 01:20 · sessao df4d76a780ab · - · - · origem=Ronald (tmux)

Os ajustes apontados foram atendidos por leitura: R8 limpa relações antes de `.only`; R4 verifica Q11; R13 cobre os dois caminhos; R14 verifica `limite=0`; R15 coleta a razão efetiva. (`chamados/juizes.py:1572`, `chamados/tests/test_e3_portas_tarefa.py:241`, `:584`, `:589`, `:619`, `:658`)

O alvo administrativo agora é específico de `expor_admin`. (`chamados/tests/e3_v8_mutantes.py:65`)

Persiste **R2**: a hipótese simultânea obtém `colab`, enquanto a matriz exige `admin`; alcance ORM continua pendente. (`e3-v8-r2-pendente.out:1`)

Execução ORM, R16 e certificação final: **[nao verificado]**. Nenhum novo defeito concreto encontrado nesta revisão limitada.

## 14/11 01:17 · sessao df4d76a780ab · - · - · origem=Ronald (tmux)

Os ajustes corrigem os pontos de revisão R8, R4 ORM e a declaração de R11. (`chamados/juizes.py:1572`, `chamados/tests/test_e3_portas_tarefa.py:235`, `chamados/management/commands/diff_e3_sombra.py:107`)

R13 ganhou cobertura do caminho administrativo dispensado. (`chamados/tests/test_e3_portas_tarefa.py:585`)

Persiste uma falha no executor: `absorcao_admin_sem_rotulo` tem alvo repetido e encerrou com prova inválida. É necessário tornar o contexto da substituição exclusivo de `expor_admin`. (`chamados/tests/e3_v8_mutantes.py:62`, `e3-v8-mutantes.out:1464`)

R2 continua pendente de alcance ORM e cura; o teste puro agora identifica corretamente a hipótese administrativa isolada. (`chamados/tests/test_e3_portas_tarefa.py:220`, `:440`)

Execução ORM e certificação: **[nao verificado]**.

## 14/11 01:14 · sessao df4d76a780ab · - · - · origem=Ronald (tmux)

Pré-verificação encontrou:

- **R8 reprovado:** `.only(...)` conserva os `select_related` herdados da fila, incluindo relações omitidas, podendo gerar `FieldError`. Limpar relações antes da seleção ou fornecer `_base` adequado. (`chamados/juizes.py:1571`, `chamados/services/painel.py:98`)
- **R2 ainda não curado:** ações simultâneas `validar/admin` e `responder/colaborador` sobre a mesma porta produzem `fio='colab'`. O teste puro contém somente a ação administrativa; não comprova o caso simultâneo do teste ORM. (`chamados/services/tarefa_do_dia.py:588`, `chamados/tests/test_e3_portas_tarefa.py:212`, `:439`)
- **R4 ORM não representa Q11:** registra `SILENCIO` sem projetar o encerramento da pergunta e espera `cobrar/colab`. O puro cobre validação com lavra; a fixture ORM precisa corresponder. (`chamados/tests/test_e3_portas_tarefa.py:235`, `:452`)
- **R11 deixou declaração obsoleta:** o comando ainda anuncia leitura do relógio da máquina pelo app. (`chamados/management/commands/diff_e3_sombra.py:107`)
- **R13:** falta mutante do segundo caminho alterado, `expor_admin`. (`chamados/services/tarefa_do_dia.py:258`, `chamados/tests/e3_v8_mutantes.py:55`)

A adaptação posicional preserva os oito índices anteriores. (`chamados/services/vigia_fio.py:635`, `chamados/management/commands/backfill_fio_morto_e4b.py:253`)

Comando executado sem banco:

```bash
env -u DJANGO_SETTINGS_MODULE PYTHONDONTWRITEBYTECODE=1 \
python3 -m unittest chamados.tests.test_e3_pendentes_app.CatracaC17Test
```

Resultado: **Ran 7; OK**. A catraca cobra um único chamador permitido de C17. (`chamados/tests/test_e3_pendentes_app.py:146`)

Alcance e execução ORM: **[nao verificado]**. Nenhum arquivo editado por este revisor.

## 14/11 01:08 · sessao bb855a61a1c6 · CORTE-DAS-TRAVAS · verificador · origem=Code

**REPROVADO para pouso.** Há caminhos que classificam dinheiro/universo como “so leitor”, certificação que ignora arquivos executáveis não rastreados e catraca que ainda reprova sem aumento do total (`bin/classe_da_obra.py:151`; `bin/classe_da_obra.py:237`; `app/core/tests/test_medida_turno_do_par.py:563`).

Identidade do commit/base, arquivo original do aval, inteira final e retomada efetiva em produção: **[nao verificado]**. [nao verificado]

1. **[bloqueia] Classificador: dinheiro pode sair “so leitor”.**  
   O reconhecimento de dinheiro depende do caminho; qualquer `.py` restante sem os tokens enumerados recebe `LEITOR` (`bin/classe_da_obra.py:50`; `bin/classe_da_obra.py:145`). Contraprovas em memória: mudar `adicional_noturno_pct: 20.0` para `40.0`, ou a conversão de horas extras de `/ 60` para `/ 30`, devolve **“so leitor”** (`app/core/regua_cct.py:42`; `app/ponto/services/dia_pago.py:180`; `bin/classe_da_obra.py:151`). Isso viola diretamente a proibição de usar essa classe em dinheiro.

2. **[bloqueia] Classificador: mudar universo julgado também pode sair “so leitor”.**  
   O trecho `j[0] <= dia <= j[1]` seleciona os dias da janela de cada empresa. Trocar somente os limites por `<` altera o universo e devolve **“so leitor”**, confirmado em memória (`app/chamados/services/furo_sem_canal.py:75`; `bin/classe_da_obra.py:151`). O classificador examina somente linhas alteradas e procura uma enumeração de tokens, sem reconhecer essa restrição de universo (`bin/classe_da_obra.py:117`; `bin/classe_da_obra.py:55`).

3. **[bloqueia] Inteira final: os arquivos executados podem divergir da tree certificada.**  
   Trecho: `status --porcelain --untracked-files=no`. Arquivos não rastreados ficam fora da limpeza, mas a suite monta o diretório físico `copia/app` (`bin/classe_da_obra.py:237`; `bin/suite.sh:204`; `bin/suite.sh:230`). Portanto, um módulo não rastreado pode participar da execução sem integrar a tree registrada; comparar `HEAD^{tree}` antes/depois não detecta isso (`bin/classe_da_obra.py:247`; `bin/classe_da_obra.py:255`). Ocorrência em produção: **[nao verificado]**.

4. **[bloqueia] Catraca: o corte “reprova só pelo total” não alcançou o teste completo.**   [nao verificado]
   `_catraca()` aceita redução, mas logo depois permanece `assertEqual(CRITERIOS_REGISTRADOS, ...)`, congelando A1/B1 em 6/82 (`app/core/tests/test_medida_turno_do_par.py:508`; `app/core/tests/test_medida_turno_do_par.py:108`; `app/core/tests/test_medida_turno_do_par.py:563`). Esses campos são **contagens**, apesar do nome “critérios” (`app/core/medida_turno_do_par.py:441`).  
   Contraprova em memória: retirar o corpo pareador de `_alinhar_por_papel` produz pareadores **9→8**, derivações **85→85**, A1 **6→5**. `_catraca()` apenas avisa; o teste completo reprova pela igualdade restante (`app/core/tests/test_medida_turno_do_par.py:158`; `app/core/tests/test_medida_turno_do_par.py:563`).

5. **[bloqueia] Triagem: o destino implementado diverge do destino literal pedido.**  
   O aval fornecido pede `logs/AUDITORIA-TRAVAS/triagem.log` **[nao verificado no arquivo original]**. O código escreve e vigia **`logs/codex/triagem.log`** (`bin/disjuntor.py:148`; `bin/disjuntor.py:936`). O acréscimo no wrapper apenas imprime uma instrução; não chama o escritor de triagem (`bin/codex_pergunta.sh:469`). A retomada da escrita no destino pedido permanece **[nao verificado]**.

6. **[bloqueia] Vigia: há falsos “zero pendentes” por integridade e por identidade.**  
   `chamadas.log` malformado tem linhas descartadas silenciosamente; um `fim` cortado também desaparece da leitura. O vigia interpreta a ausência resultante como nenhuma conclusão a triar e retorna rc 0 (`bin/codex_limite_estado.py:57`; `bin/codex_limite_estado.py:66`; `bin/disjuntor.py:1015`; `bin/disjuntor.py:1038`). Ambos foram reproduzidos em memória.  
   Além disso, a triagem identifica a volta por `hash12@pid@fim@sha12`, mas o vigia reduz isso a **obra/hash12/pid**. Uma chamada posterior no mesmo hash, após reutilização do PID, pode herdar a triagem antiga e sumir das pendências (`bin/disjuntor.py:928`; `bin/disjuntor.py:1003`). Essa contraprova também retornou zero em memória. O vigia ainda passa `{}` ao leitor, perdendo a conferência histórica de truncamento usada pelo B3 (`bin/disjuntor.py:997`; `bin/disjuntor.py:955`).

7. **[fila — brecha herdada] B3: “linha do Ronald” é reserva textual, sem autoria conferida.**  
   `ERRO-BOBO` é aceito pelo conteúdo; a cota só se aplica a `tipo == 'auto'` (`bin/disjuntor.py:772`; `bin/disjuntor.py:1115`). Uma linha nova `ERRO-BOBO OBRA-X: texto do Code` liberou, em simulação em memória, uma obra com AUTO já esgotada (`bin/disjuntor.py:1124`). A guarda protege `--rearma-b3`, mas não autentica quem escreveu essa linha no RELATO (`bin/disjuntor.py:249`; `bin/disjuntor.py:264`). **Não é afrouxamento introduzido pelo diff**: o reconhecedor antigo já aparece como contexto no anexo fornecido **[nao verificado na base Git]**.

8. **[fila] Os selos ainda têm mutantes plausíveis que passam.**
   
   - **GATE:** o selo fabrica certificados diretamente no livro e testa o portão, sem executar `pouso_inteira`. Suprimir a condição de recusa do emissor não é exercitado por esse selo (`bin/tests/test_gate_pela_classe.sh:121`; `bin/tests/test_gate_pela_classe.sh:135`; `bin/classe_da_obra.py:256`).
   - **B3/caso 9:** adicionar o literal `B3: o Code libera desenho pela AUTOLIBERA, do Ronald` passou o **caso 9 completo em memória**. `DESENHO` é procurado distinguindo maiúsculas/minúsculas; sujeito/verbo não (`bin/tests/test_disjuntor.sh:1332`; `bin/tests/test_disjuntor.sh:1356`).
   - **Triagem:** o selo corrompe `triagem.log`, mas não cobre `chamadas.log` malformado nem reutilização de PID/hash, os falsos zeros do achado 6 (`bin/tests/test_triagem_pendente.sh:17`; `bin/tests/test_triagem_pendente.sh:50`).

9. **[fila] Catraca: aviso ao RELATO não tem entrega comprovada.**  
   `_aviso()` somente escreve stderr; os novos MORDE conferem listas retornadas, sem cobrar a publicação (`app/core/tests/test_medida_turno_do_par.py:467`; `app/core/tests/test_medida_turno_do_par.py:593`; `app/core/tests/test_medida_turno_do_par.py:602`). O filtro do pre-push descarta `CATRACA-AVISO` da saída apresentada (`bin/pre-push.sh:146`). Escrita desses avisos no RELATO: **[nao verificado]**.

10. **[fila] Leitores humanos externos continuam recebendo o significado antigo de `arquivo=`.**  
    O wrapper ainda diz “jsonl cresce = prova de vida”, e LEIS ainda descreve prova por “pid/arquivo” (`bin/codex_pergunta.sh:51`; `app/docs/LEIS.md:66`). Agora arquivo sozinho falha e, junto do PID, é inerte (`bin/hook_stop_fila1.py:748`).

O mapa de escritores, leitores e significados ficou assim:

| Dado | Quem escreve | Quem lê / significado |
|---|---|---|
| `chamadas.log` | Wrapper: `ini` e `fim` (`bin/codex_pergunta.sh:305`, `:272`). | Leitor comum `_linhas_chamadas`; vigia o reutiliza (`bin/codex_limite_estado.py:49`; `bin/disjuntor.py:1007`). TAB; chave obra/hash12/PID; duração em segundos desde `T0C`, incluindo etapas do wrapper (`bin/codex_pergunta.sh:265`, `:273`). |
| Sidecar `.id` | Wrapper remove o anterior e grava obra/papel/hash/PID/fim/SHA da resposta desta chamada (`bin/codex_pergunta.sh:401`, `:464`). | `_le_id` e `_triagem_sob_trava`, com conferência do SHA (`bin/disjuntor.py:879`, `:926`). Seu `fim` é carimbado antes do `fim` escrito pelo EXIT; não são necessariamente o mesmo instante (`bin/codex_pergunta.sh:464`, `:272`). |
| `triagem.log` | `--triagem`, sob trava da resposta e depois do estado (`bin/disjuntor.py:912`, `:930`, `:938`). | B3/rearme e vigia usam `_triagens`; última linha por volta prevalece (`bin/disjuntor.py:976`, `:1062`, `:1195`, `:997`). Volta completa contém fim/SHA; vigia descarta essas parcelas — achado 6. |
| Estado do disjuntor | `_com_estado`: flock exclusivo, temporário e replace (`bin/disjuntor.py:374`, `:392`). | Decisão, rearme e mensagens (`bin/disjuntor.py:1147`, `:1220`, `:1260`). `auto_usada` conta liberações históricas por obra; `auto_sobe` deduplica anúncios por SHA (`bin/disjuntor.py:1116`, `:1123`, `:1243`). |
| Fotografia B3 | `_fotografa` grava SHA das linhas e instante Unix em segundos (`bin/disjuntor.py:847`, `:856`). | Liberação exige linha nova/não consumida e **mtime ≥ disparado_em**; igualdade é aceita (`bin/disjuntor.py:1111`, `:1113`). |
| `disjuntor.log` | `trilha`; anúncio B3 depois da gravação do estado, ainda sob trava (`bin/disjuntor.py:220`, `:406`, `:1139`). | Painel conta eventos `LIBERA (auto)` desde `agora−86400`, limite inferior inclusivo (`bin/disjuntor.py:2087`, `:2107`, `:2114`). Isso não é a cota histórica `auto_usada` (`bin/disjuntor.py:158`). |
| `REGISTRO` / `LINHAS_REGISTRADAS` | Literais no teste (`app/core/tests/test_medida_turno_do_par.py:158`, `:254`). | `_identidade`: chave `(arquivo::função, sítio ou critério)`; linhas são ocorrências AST (`app/core/tests/test_medida_turno_do_par.py:518`, `:523`). Unidade da medida: função/critério mais parcelas nomeadas; uma função pode contar duas vezes (`app/core/medida_turno_do_par.py:10`). |
| Topo do RELATO | Porta automática `no_topo`; wrapper apenas emite a sugestão ESPERANDO (`app/core/esteira_vigia.py:865`; `bin/codex_pergunta.sh:397`). Autoria material de AUTOLIBERA/DESENHO: **[nao verificado]**. | Stop usa `_topo`; B3 usa o mesmo recorte, com dupla leitura estável (`bin/hook_stop_fila1.py:717`; `bin/disjuntor.py:805`, `:812`). |
| `hook_stop_espera.json` | Rotina de escrita retirada no diff fornecido **[nao verificado na base Git]**. | Restam lápide e teste de ausência; consumidor executável restante não localizado na cópia (`bin/hook_stop_fila1.py:538`; `bin/tests/test_hook_esperando_com_prova.sh:114`). Remoção do arquivo legado no host: **[nao verificado]**. |
| Livro/log da inteira | `pouso_inteira` grava log e seis campos no livro (`bin/classe_da_obra.py:250`, `:258`). | Parser usa último veredito/último `Ran N`; portão usa a **tree**, não o commit (`bin/classe_da_obra.py:224`, `:207`, `:216`). O leitor do livro não valida commit, obra, hora ou conteúdo do log (`bin/classe_da_obra.py:207`). |

Os carimbos de chamadas, triagem, trilha e livro usam hora local sem fuso gravado; igualdade dos fusos dos processos em produção: **[nao verificado]** (`bin/codex_pergunta.sh:272`; `bin/disjuntor.py:939`, `:220`; `bin/classe_da_obra.py:259`).

Por peça, os limites preservados que consegui conferir são:

- **ESPERANDO:** não encontrei afrouxamento além de tornar `arquivo=` inerte. Permanecem PID vivo, identidade do processo, ancestralidade e rejeição de campos repetidos; a prova do RELATO vem antes da liberação (`bin/hook_stop_fila1.py:605`, `:739`, `:1261`, `:1294`). PID vivo com arquivo parado/ausente está explicitamente coberto (`bin/tests/test_hook_esperando_com_prova.sh:117`).
- **AUTOLIBERA:** aceita somente FORMA/ERRO-BOBO, uma vez por obra; rearme preserva a cota. Não aceita `classe=DESENHO` (`bin/disjuntor.py:151`, `:778`, `:1115`, `:1206`).
- **Juízes/ordem/escritor:** classificador consulta a união base/cabeça de JUIZES; B3 mantém decisão/gravação/anúncio na mesma transição; catraca e ESTADO reutilizam o mesmo medidor (`bin/classe_da_obra.py:166`; `bin/disjuntor.py:1147`; `bin/turno_do_par_estado.py:36`). Nenhum segundo juiz de negócio foi identificado nesses caminhos; abrangência fora da cópia: **[nao verificado]**.
- **Consumidores externos:** o selo do censo mantém a tupla de ESPERANDO; ESTADO lê o medidor, sem depender de REGISTRO; novos selos entram pelo glob da régua (`bin/tests/test_censo_antes_da_vaga.sh:329`; `bin/gerar_estado.py:299`; `bin/regua.sh:138`). O classificador declara estar sem ligação à conduta; pouso real protegido por ele: **[nao verificado]** (`bin/classe_da_obra.py:35`).
- **p95/andável:** a cópia contém tetos em segundos por papel e exige ausência de outro item andável para PAREI devolver o turno (`bin/codex_pergunta.sh:95`; `bin/hook_stop_fila1.py:1151`). Medição que sustenta os percentis, execução dos selos completos e inteira final: **[nao verificado]**.

## 14/11 01:03 · sessao 8bdaa99be379 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — a catraca completa ainda reprova com total menor.** `_catraca()` permite queda e retorna aviso (`app/core/tests/test_medida_turno_do_par.py:503`), mas o mesmo teste continua exigindo igualdade de A1/B1 (`app/core/tests/test_medida_turno_do_par.py:563`), congelados em 6/82 (`app/core/tests/test_medida_turno_do_par.py:108`). Esses campos são contagens, não definições dos critérios (`app/core/medida_turno_do_par.py:441`). Contraprova em memória: substituir o corpo de `_alinhar_por_papel` por `return []` retirou o pareador de `app/escala/utils.py:520`: pareadores 9→8, derivações 85→85; `_catraca()` apenas avisou, porém o teste completo falhou por A1 6→5 (`app/core/tests/test_medida_turno_do_par.py:563`).

2. **FILA — aviso prometido ao RELATO termina em stderr; selo não cobra sua publicação.** `_aviso()` só escreve stderr (`app/core/tests/test_medida_turno_do_par.py:465`), chamado pelo teste principal (`app/core/tests/test_medida_turno_do_par.py:560`). Os novos MORDE conferem a lista devolvida por `_identidade()`, sem conferir stderr (`app/core/tests/test_medida_turno_do_par.py:587`, `app/core/tests/test_medida_turno_do_par.py:597`); o caso de redução testa apenas `_catraca()` com um número artificialmente decrementado (`app/core/tests/test_medida_turno_do_par.py:906`). Mutante em memória `_aviso=lambda msg:None` passou os seis casos relevantes. O filtro existente do pre-push elimina `CATRACA-AVISO` (`bin/pre-push.sh:146`). Escrita efetiva desses avisos no RELATO: **[nao verificado]**.

3. **FILA — documentação externa mantém o significado antigo de `arquivo=`.** O wrapper ainda descreve “jsonl cresce = prova de vida” (`bin/codex_pergunta.sh:51`) e LEIS mantém “pid/arquivo” (`app/docs/LEIS.md:66`); agora arquivo sem PID reprova e arquivo junto do PID é inerte (`bin/hook_stop_fila1.py:738`, `bin/hook_stop_fila1.py:748`).

**ESPERANDO:** não identifiquei afrouxamento adicional ao descarte da prova por arquivo: continuam a checagem de identidade/processo e ancestralidade (`bin/hook_stop_fila1.py:605`), rejeição de campos repetidos/ilegíveis (`bin/hook_stop_fila1.py:739`) e prova do RELATO antes da espera (`bin/hook_stop_fila1.py:1261`, `bin/hook_stop_fila1.py:1294`). Os casos PID vivo+arquivo parado/ausente estão explicitamente cobertos (`bin/tests/test_hook_esperando_com_prova.sh:117`).

**Mapa escritores/leitores:**

- **REGISTRO/LINHAS:** literais mantidos no código (`app/core/tests/test_medida_turno_do_par.py:158`, `app/core/tests/test_medida_turno_do_par.py:254`), lidos por `_identidade()` (`app/core/tests/test_medida_turno_do_par.py:518`). Chave: `(arquivo::funcao, sitio|criterio)`; valores de linhas: ocorrências AST (`app/core/tests/test_medida_turno_do_par.py:523`). Unidade: função por critério mais parcelas nomeadas, permitindo multiplicidade da função (`app/core/medida_turno_do_par.py:10`).
- **Saída da medida:** escritor único do resultado é `censo_fontes()` (`app/core/medida_turno_do_par.py:336`); consumidores externos importam o mesmo medidor (`bin/turno_do_par_estado.py:33`, `bin/gerar_estado.py:299`). Não dependem de REGISTRO nem do stderr da catraca (`bin/turno_do_par_estado.py:36`).
- **Topo RELATO:** wrapper emite a sugestão ESPERANDO em stderr (`bin/codex_pergunta.sh:397`); inserção material é manual ou pela porta `no_topo()` (`app/core/esteira_vigia.py:865`), cujo chamador de censo é `disjuntor.py:347`. Leitura central em `_topo()` (`bin/hook_stop_fila1.py:467`); ORÇAMENTO e Stop compartilham a fotografia por memo (`bin/hook_stop_fila1.py:758`, `bin/hook_stop_fila1.py:1052`, `bin/hook_stop_fila1.py:1294`).
- **Memória removida:** `hook_stop_espera.json` aparece apenas como lápide e verificação de ausência no recorte examinado (`bin/hook_stop_fila1.py:538`, `bin/tests/test_hook_esperando_com_prova.sh:114`). Nenhum consumidor executável restante localizado **[nao verificado fora da cópia pesquisada]**.
- **Chamador externo preservado:** o selo do censo continua consumindo a mesma tupla de três posições de `_esperando_com_prova()` (`bin/tests/test_censo_antes_da_vaga.sh:329`). Handoff e alarme usam o reconhecedor de PAREI-censo preservado (`bin/handoff_sessao.sh:129`, `bin/alarme_sessao_ociosa.py:137`).

_4918 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
