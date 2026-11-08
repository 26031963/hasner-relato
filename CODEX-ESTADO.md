# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 17:58** · publicado em **08/11/2026 18:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 17:58 · sessao 3691d7efd094 · FIO-E0-V4-E1 · verificador · origem=Code

**NÃO PROCEDENTE a conformidade integral da 15f.** A inversão antiga do reuso isolado foi corrigida, mas permanecem composições que violam a ordem global, leitores sem discriminação da conversa e lacunas nas provas. (`app/chamados/models.py:2910`; `app/chamados/services/disputa_emissao.py:993`; `app/chamados/models.py:390`; `app/chamados/tests/test_e1_v9.py:94`.)

Execução das suítes, RED/GREEN históricos, ocorrência dos contraexemplos em produção e confirmação da ancestralidade Git declarada: **[nao verificado]**.

**1. Situação dos itens 3, 4, 6 e 7 da v15e**

| Item | Agora | Fundamentação |
|---|---|---|
| **3 — Matriz** | **Parcial** | Há provas novas dos ramos antes mal citados, inclusive escrita parcial seguida de rollback. A5 percorre o `raise`, porém provoca divergência de instância no mesmo dia; continuam ramos de classificação sem prova na extensão. (`app/chamados/tests/test_e1_v15f.py:284`; `app/chamados/tests/test_e1_v15f.py:250`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`.) |
| **4 — Conversa** | **Curado localmente; parcial globalmente** | A ida exclui conversa e o contador inclui conversa NULL. A reversão continua selecionando por PK sem esse filtro; outros leitores continuam usando o dia civil como dia do fato. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`, `:474`; `app/chamados/services/fio.py:1118`; `app/chamados/models.py:390`.) |
| **6 — Catracas/mutantes** | **Parcial** | O wrapper agora exige atomic; o espião alcança o contrato real; M6 exige motivo específico. Permanecem os escapes descritos no item 4 abaixo. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:551`, `:567`; `app/chamados/tests/test_e1_v15f.py:169`.) |
| **7 — Ordem de trava** | **Parcial** | Corrigidos o reuso isolado com rechave/costura e a aquisição entre candidatos do migrar/reverter. Isso não garante ordem entre todas as chamadas da mesma transação, nem entre PKs adquiridas por chamadas diferentes. (`app/chamados/models.py:2910`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:375`, `:469`; `app/chamados/services/disputa_emissao.py:993`.) |

**2. Ordem de trava — PROCEDENTE a permanência da classe**

O helper ordena **cada conjunto recebido**, mas não mantém um controle da maior classe/PK já adquirida pela transação. Portanto, chamadas individualmente ordenadas podem compor uma sequência global invertida. (`app/chamados/models.py:2625`, `:2639`, `:2648`, `:2661`, `:2665`.)

| Caminho | Resultado da conferência estática |
|---|---|
| **Criação isolada, incluindo conversa** | Adquire Colaborador e insere Pergunta; não adquire explicitamente Disputa/Chamado nesse caminho. Os locks de FK do INSERT ficam fora do rastreador utilizado. (`app/chamados/models.py:2811`, `:2974`; `app/chamados/tests/test_e1_v9.py:51`.) |
| **Reuso sem rechave, mesmo dono, isolado** | Colaborador → Chamado, quando há costura → UPDATE Pergunta. Sem costura ou alteração, não há esse UPDATE. (`app/chamados/models.py:2811`, `:2926`, `:2938`, `:2955`, `:2964`; `app/chamados/services/fio.py:768`.) |
| **Reuso com rechave/costura, mesmo dono, isolado** | O bloco novo antecipa Disputa, destinos e Pergunta. Corrige o P→Chamado anterior. (`app/chamados/models.py:2910`; `app/chamados/services/fio.py:969`, `:768`.) |
| **`rechavear_no_reuso` isolado** | Consulta o veto e chama a porta que adquire C→D→P; o Colaborador consultado posteriormente por `fato_livre_para` já foi adquirido. O DRY executa essa porta e desfaz o ato. (`app/chamados/services/fio.py:1020`, `:1026`, `:1029`, `:969`; `app/chamados/models.py:2740`.) |
| **`_migrar` e `_reverter`** | Antecipam C→D→Chamado→P dos candidatos antes dos laços. Curam a inversão anterior entre grupos/linhas desse conjunto. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:374`, `:375`, `:468`, `:469`.) |
| **Reapontamento e chamadores com atomic aberto** | Permanecem contraexemplos entre classes e entre PKs, abaixo. (`app/chamados/services/fio.py:889`, `:892`; `app/chamados/services/disputa_emissao.py:964`, `:993`.) |

**Contraexemplo estático entre classes, mesmo colaborador:** o agrupador abre atomic, adquire somente C, encontra uma pergunta ligada ao chamado e complementa os marcos da disputa dela. Se o marco irmão tem canônica em outra disputa, `_ja_perguntou` não a encontra porque consulta apenas a disputa atual; `abrir`, porém, pode reusar essa canônica e costurá-la. (`app/chamados/services/disputa_emissao.py:964`, `:968`, `:979`, `:990`, `:1316`; `app/chamados/models.py:2871`, `:2938`, `:2964`.)

Depois desse reuso, `_criar_pergunta_se_couber` devolve `True` e o agrupador salva a Disputa atual, ainda sem pré-trava: **C→Chamado→Pergunta→Disputa**. A chamada seguinte a `ch.analisar()` também pode adquirir disputas abertas depois da Pergunta. (`app/chamados/services/disputa_emissao.py:834`, `:844`, `:991`, `:993`, `:994`; `app/chamados/models.py:806`, `:290`.) Execução e ocorrência em produção: **[nao verificado]**.

**Contraexemplo estático entre colaboradores:** `_nascer` adquire primeiro C da disputa; o bloco novo pode depois adquirir C do destino com PK menor. E4-c aceita dias iguais sem conferir igualdade de donos. (`app/chamados/models.py:2811`, `:2911`; `app/chamados/juizes.py:2027`.) Existência desse arranjo em produção: **[nao verificado]**.

**Contraexemplo estático entre perguntas:** no reapontamento que data uma canônica menor, as perdedoras retratáveis são atualizadas antes dela. Ordenar apenas as perdedoras não impede **P maior→P menor**, porque a canônica está excluída dessa ordenação. (`app/chamados/services/fio.py:889`, `:892`, `:1207`, `:1253`, `:1271`; `app/chamados/models.py:3145`.) Execução com esse acervo: **[nao verificado]**.

**Outro caso de PK:** a declaração antecipa os chamados vivos; uma costura posterior pode adquirir um destino terminal, excluído daquele conjunto, com PK menor. A decisão sobre o terminal acontece depois da aquisição da linha. (`app/chamados/services/declaracao_texto.py:324`, `:332`, `:348`; `app/chamados/services/disputa_emissao.py:530`; `app/chamados/models.py:2656`; `app/chamados/services/fio.py:768`, `:770`.) Execução: **[nao verificado]**.

Sobre o primeiro argumento de `trava_na_ordem`:

- **Lista ordenada:** aceita lista/tupla/set/frozenset; elimina repetidos, ignora `None` e instâncias com PK `None`, ordena os PKs e também ordena a consulta. `None` escalar resulta em conjunto vazio. (`app/chamados/models.py:2625`, `:2631`, `:2636`, `:2639`.)
- **Reentrância:** repete as consultas; para linhas já possuídas, a repetição é reentrante. Não impede aquisição posterior de uma linha nova fora da ordem. (`app/chamados/models.py:2609`, `:2639`, `:2648`, `:2661`, `:2665`.)
- **Modo universo:** `abertas_do_colaborador` admite no máximo um colaborador. (`app/chamados/models.py:2633`.)
- **`select_for_update` fora do helper:** não há chamada acrescentada pelo diff fornecido; nos arquivos produtivos alterados, as chamadas executáveis dessas quatro entidades estão no helper. (`app/chamados/models.py:2639`, `:2648`, `:2661`, `:2665`.) Correspondência histórica integral entre diff e commits: **[nao verificado]**.

**3. Conversa — cura local PROCEDENTE; esmeril global NÃO PROCEDENTE**

A conversa nasce com `localdate`; seu adaptador devolve pedido/gravado. O contador agora a inclui, mas compara o dia gravado consigo mesmo: acusa NULL, não determina independentemente se uma conversa datada conserva o dia civil original. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1507`, `:1508`, `:1125`, `:1130`.)

Permanecem estes consumidores sem discriminação suficiente:

| Consumidor | Tratamento encontrado |
|---|---|
| **Reversão** | Desvia antes do filtro da ida, busca qualquer PK do JSON e passa o dia do arquivo à porta de rechaveamento. Conversa passa pelo dedup e pelo E4-c; a porta pode escrever esse dia e trocar a célula. Com veto favorável e célula compatível, o caminho aceita a alteração. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:199`, `:474`, `:478`, `:485`, `:489`, `:493`; `app/chamados/models.py:2741`; `app/chamados/juizes.py:1997`; `app/chamados/services/fio.py:989`, `:990`.) |
| **Gate de Chamado** | Inclui conversa nas ancoradas, consulta competência pelo campo cru e procura cobrança sobrevivente desse dia. (`app/chamados/models.py:344`, `:364`, `:390`.) |
| **Pauta de competência** | Seleciona perguntas pela janela de `data_evento`, sem excluir conversa. (`app/chamados/services/competencia_trancada.py:106`.) |
| **Lavra** | Calcula expiração com `p.data_evento`; a documentação ainda associa conversa a NULL. (`app/chamados/veredito_lavrado.py:229`, `:107`.) |
| **Comparação/recusa de data** | Compara `data_evento` com o dia do chamado sem testar conversa; a materialização pode registrar `DATA_EM_DISPUTA`. (`app/chamados/services/fio.py:85`, `:88`; `app/chamados/services/materializacao.py:1191`, `:1213`.) |
| **Materialização de hora** | A seleção não exclui conversa. Uma conversa-hora pode alcançar código que usa `data_evento` para marcos, combinação de hora e ajuste overnight, se passar pelas demais guardas. (`app/chamados/services/materializacao.py:1369`, `:1221`, `:344`, `:371`, `:414`, `:421`.) |
| **Folha e tela do dia** | Agrupam/selecionam pelo campo cru, admitindo conversa. (`app/folha/previa.py:65`, `:67`; `app/ponto/services/dia_do_colab.py:65`.) |

Arquivo de reversão contendo conversa, conversa-hora materializada e efeitos desses caminhos em produção: **[nao verificado]**.

Há uma correção necessária à narrativa da triagem: o emissor normal **já barra pergunta de hora sem dia antes de chegar à conversa**. A linha 439, isoladamente, não demonstra emissão produtiva de conversa-hora; a porta aceita esse tipo e o teste novo o constrói diretamente. (`app/chamados/services/disputa_emissao.py:425`, `:430`, `:439`; `app/chamados/models.py:2764`; `app/chamados/tests/test_e1_v15f.py:64`.)

Não identifiquei no delta um novo escritor que escolha dia civil para uma linha de fato. A classificação ainda lê o antigo dia civil da saída para reconhecer o acervo a migrar; essa leitura é explícita, e o valor novo vem do juiz. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:102`, `:158`, `:160`.)

**4. Catracas e mutantes — atingem produção, mas a prova continua parcial**

O arnês extrai a função real, recompila uma substituição e instala o objeto no módulo/classe utilizado pelo caminho produtivo. O novo espião chama o contrato real e observa PK, dia relido e dia esperado. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:331`, `:342`, `:349`, `:356`; `app/chamados/tests/test_e1_v15f.py:169`, `:182`, `:185`.)

As correções específicas estão presentes: atomic exato no wrapper, mutantes de remoção/perda do savepoint e M6 condicionado ao motivo `pergunta_unica_por_episodio`. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:206`, `:551`, `:567`.)

Permanecem mutantes plausíveis:

| Mutante | Lacuna |
|---|---|
| **Inverter PK de Disputa/Chamado/Pergunta no helper** | `violacoes_linhas` verifica rank, mas não compara PKs. A exclusão é expressa; o teste antigo de SQL crescente cobre Colaborador. (`app/chamados/tests/test_e1_v9.py:51`, `:94`, `:106`; `app/chamados/tests/test_e1_v13_ordem.py:59`.) |
| **Migrar com `if ok and ln['p'].chamado_destino_id is None:`** | A catraca AST continua encontrando o contrato. Os espiões novos usam candidatos construídos sem destino, deixando o ramo com destino sem testemunha nesses cenários. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:147`; `app/chamados/tests/test_e1_v15f.py:182`; `app/chamados/tests/test_plantio_noturno_mais1.py:64`.) |
| **Retirar o atomic de `_reverter`** | A prova nova fornece seu próprio atomic ao redor da chamada e percorre sucesso. A catraca inspeciona `_migrar`, não `_reverter`; a base também é `TestCase`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:465`; `app/chamados/tests/test_e1_v15f.py:151`; `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:141`; `app/chamados/tests/test_plantio_noturno_mais1.py:39`.) |

O detector de ordem aceita sequência descendente dentro da classe, e o detector AST do migrar aceita a guarda condicional descrita; isso decorre dos predicados efetivamente implementados. Sobrevivência desses mutantes à **suíte inteira**: **[nao verificado]**. (`app/chamados/tests/test_e1_v9.py:98`; `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:147`.)

**5. Linhas da seção “Volta 15f”**

A conferência abaixo é do vínculo estático teste→ramo; execução dos testes: **[nao verificado]**. [nao verificado]

| Linha | O teste percorre o anunciado? | [nao verificado]
|---|---|
| **A5** | **Parcial:** percorre o `raise` exato, mas ambas as datas são DIA; quem causa a recusa é `instancia_marco='X'`. Não demonstra episódio realmente em outro dia. (`app/chamados/tests/test_e1_v15f.py:286`, `:287`, `:292`; `app/chamados/services/fio.py:1688`; `app/chamados/models.py:2881`.) |
| **A6** | **Sim, nas formas declaradas:** real preserva K; injeção acrescenta retratada à eleição e alcança a recusa da canônica. (`app/chamados/tests/test_e1_v15f.py:307`, `:315`, `:316`; `app/chamados/models.py:2890`.) |
| **A12** | **Sim, com distinção:** o real recusa na entrada; somente a injeção alcança o segundo portão, com espião identificando a canônica. (`app/chamados/tests/test_e1_v15f.py:327`, `:339`, `:343`; `app/chamados/models.py:2930`.) |
| **G6** | **Sim:** injeta exceção na porta, exige erros/PAREI e igualdade das fotografias/trilha; não provoca escrita parcial antes da exceção. (`app/chamados/tests/test_e1_v15f.py:215`, `:217`, `:225`.) |
| **G7** | **Sim:** remove índice, cria gêmea, confirma fato conjunto e verifica migração da primeira/recusa da segunda. (`app/chamados/tests/test_e1_v15f.py:228`, `:236`, `:240`, `:246`.) |
| **G8** | **Sim:** observa primeira escrita real, faz a gêmea mentir sucesso e verifica retorno dos dois dias/trilhas, enquanto outro grupo migra. (`app/chamados/tests/test_e1_v15f.py:250`, `:257`, `:263`, `:266`, `:268`.) |
| **C1** | Absorção: corresponde ao ramo. (`app/chamados/tests/test_e1_v15f.py:361`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`.) |
| **C2** | Materializada sem plantada, sem via de absorção: corresponde. (`app/chamados/tests/test_e1_v15f.py:365`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:72`.) |
| **C3** | Ambiguidade de **plantadas**: corresponde; não cobre ambiguidade de turnos vivos. (`app/chamados/tests/test_e1_v15f.py:371`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:73`.) |
| **C4** | Cobre plantada sem turno encontrado; não cobre plantada associada a dois turnos. (`app/chamados/tests/test_e1_v15f.py:375`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`.) |
| **C5** | Cobre exclusão da materializada antes do julgamento do novo dia. (`app/chamados/tests/test_e1_v15f.py:380`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158`.) |
| **C6** | Juiz None injetado: corresponde. (`app/chamados/tests/test_e1_v15f.py:386`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:161`.) |
| **C7** | Divergência juiz×turno injetada: corresponde. (`app/chamados/tests/test_e1_v15f.py:392`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:165`.) |
| **C8** | Órfã materializada fora do aval: corresponde. (`app/chamados/tests/test_e1_v15f.py:399`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.) |
| **C9** | DRY real: corresponde. “Sem escrita” significa aqui **sem persistência**: a porta roda e o atomic desfaz. (`app/chamados/tests/test_e1_v15f.py:405`; `app/chamados/services/fio.py:1029`, `:1030`, `:1032`.) |
| **Conversa/contadores** | Corresponde à exclusão da ida e à contagem de conversa NULL. O teste do rótulo exige somente substring. (`app/chamados/tests/test_e1_v15f.py:72`, `:78`, `:96`, `:109`.) |
| **Ordem A8/A11 + G4/G3** | **Parcial:** combina rechave+costura; o migrar usa cenário limpo, não G3 vetado. Não prova A11 isolado sem rechave nem ordem crescente de PK. (`app/chamados/tests/test_e1_v15f.py:117`, `:140`, `:147`; `app/chamados/tests/test_e1_v9.py:51`.) |
| **Catraca abrir** | Corresponde à exigência estrutural do atomic e aos três mutantes novos. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:206`.) |
| **Catraca migrar** | Corresponde aos três candidatos do cenário e aos mutantes de remoção/instância velha; não demonstra todos os ramos possíveis. (`app/chamados/tests/test_e1_v15f.py:182`, `:194`, `:199`, `:203`.) |
| **M6** | Agora exige motivo específico; morre pelo índice. Não demonstra tratamento de acervo previamente duplicado. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:551`, `:567`.) |

**Caminhos produtivos sem prova na extensão nova:** plantada em dois turnos; dois turnos só de saída; múltiplos turnos abertos candidatos da órfã; órfã sem marcos. C3/C4 não substituem essas provas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`; `app/chamados/tests/test_e1_v15f.py:371`, `:375`.)

**6. Quem escreve e quem lê os valores envolvidos no delta**

| Campo/valor | Escritores e persistência | Leitores envolvidos |
|---|---|---|
| **`data_evento`** | Atribuidor: `gravar_dia_do_fato`. Persistem nascimento, costura que data e rechaveamento. O valor vem do juiz/adaptador; na reversão vem do arquivo. (`app/chamados/services/fio.py:800`, `:863`, `:892`, `:989`, `:995`; `app/chamados/models.py:2973`, `:2974`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:478`.) | Identidade/dedup, contrato, contadores, classificação, veto/reversão, materialização e lavra; leitores sem discriminador estão discriminados no item 3. (`app/chamados/models.py:2726`; `app/chamados/services/fio.py:1085`, `:1058`, `:1125`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97`, `:391`, `:485`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
| **`celula`** | Nascimento escolhe a do dia do fato; reuso troca a FK; rechaveamento escolhe a do novo dia. (`app/chamados/models.py:2860`, `:2924`, `:2964`, `:2971`; `app/chamados/services/fio.py:986`, `:990`, `:995`.) | Rechaveamento lê célula velha; reversão fotografa/compara a FK; juiz e sinal/evento leem a célula vinculada. (`app/chamados/services/fio.py:972`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:355`, `:493`; `app/chamados/juizes.py:1121`; `app/chamados/signals.py:244`.) |
| **`chamado_destino`** | Nascimento, reuso na porta e `reapontar_resolvedora`. A guarda terminal também altera/restaura temporariamente a instância. (`app/chamados/models.py:2971`, `:2940`, `:2964`; `app/chamados/services/fio.py:890`, `:892`, `:779`, `:783`.) | Pré-travas novas, âncora do fio, E4-c, guarda terminal e materialização. (`app/chamados/models.py:2912`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`, `:471`; `app/chamados/services/fio.py:97`, `:991`, `:768`; `app/chamados/services/materializacao.py:339`.) |
| **`universo`** | Contadores locais, incrementados antes dos descartes e devolvidos no dicionário; não são campo persistido. (`app/chamados/services/fio.py:1054`, `:1056`, `:1066`, `:1121`, `:1123`, `:1133`.) | Backfill imprime ambos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`, `:550`.) |
| **`universo_rotulo`** | Literais produzidos pelos próprios contadores, declarando contagem anterior aos descartes. (`app/chamados/services/fio.py:1067`, `:1134`.) | Backfill imprime literalmente; teste exige `"ANTES dos descartes"`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`, `:550`; `app/chamados/tests/test_e1_v15f.py:109`.) |

A exclusividade literal reivindicada para `chamado_destino` na docstri

[truncado em 20000 de 22136 caracteres]

## 08/11 17:55 · sessao bdaaabc83e2f · - · - · origem=Ronald (tmux)

Correção: a cadeia anterior **declaração → rechave de episódio BATIDA** não se sustenta: os motivos da declaração são MARCO e o derivador de episódio exige BATIDA. (`app/chamados/services/declaracao_texto.py:42`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/catalogo/motivos.py:35`, `app/chamados/catalogo/motivos.py:48`, `app/chamados/catalogo/motivos.py:49`, `app/chamados/models.py:3416`.)

**Contraexemplo real de composição, mesmo dono, sem rechave: C→Ch→P→D.** O agrupador abre atomic e trava somente C; encontra uma pergunta ligada ao chamado, obtém sua disputa e complementa os marcos. (`app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:968`, `app/chamados/services/disputa_emissao.py:979`, `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:990`.)

Premissa estática: um marco irmão está ausente dessa disputa, mas tem canônica em outra disputa do mesmo colaborador, já no dia correto e com destino diferente. `_ja_perguntou` consulta apenas a disputa atual, enquanto `abrir` deduplica entre disputas; portanto o caminho pode costurar e salvar a canônica existente. (`app/chamados/services/disputa_emissao.py:1316`, `app/chamados/services/disputa_emissao.py:829`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/models.py:2871`, `app/chamados/models.py:2926`, `app/chamados/models.py:2938`, `app/chamados/models.py:2964`.)

O helper retorna `True` também depois desse reuso, levando o agrupador a salvar a disputa atual **depois do UPDATE da Pergunta**, embora não a tenha travado antes. A chamada seguinte a `ch.analisar()` também pode adquirir disputas abertas depois da Pergunta. (`app/chamados/services/disputa_emissao.py:844`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/services/disputa_emissao.py:993`, `app/chamados/services/disputa_emissao.py:994`, `app/chamados/models.py:806`, `app/chamados/models.py:290`.)

Execução desse contraexemplo, ocorrência em produção e existência de um chamador produtivo BATIDA com atomic externo que alcance especificamente o novo bloco `2910`: **[nao verificado]**.

## 08/11 17:53 · sessao 2663eef25924 · - · - · origem=Ronald (tmux)

**Item 3: cura substancial, mas não matriz integral. Item 6: parcial.** Os novos testes exercitam os alvos reais e corrigem as falhas específicas anteriores; permanecem caminhos sem cobertura e mutantes plausíveis que os selos novos não detectam. (`app/chamados/tests/test_e1_v15f.py:165`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:342`; lacunas abaixo.)

Suítes Django, banco, RED/GREEN históricos e sobrevivência de uma suíte inteira: **[nao verificado]**. Rodei somente AST/funções detectoras em memória, com `python3 -B`, sem importar Django nem escrever arquivos. **[nao verificado quanto a evidência persistida; resultados observados na ferramenta]**. [nao verificado]

| Linha 15f | Conferência estática do caminho citado |
|---|---|
| A5 | Percorre a guarda e exige tipo exato `FatoEmOutroDia`; o dado tem **mesmo dia**, mas `instancia_marco='X'`, portanto prova a guarda por identidade diferente, não episódio realmente noutro dia. (`test_e1_v15f.py:284`, `:287`, `:292`; guarda `models.py:2873`, `:2881`; recusa da identidade `fio.py:1688`.) |
| A6 | Real elege K; injeção inclui retratada no universo entregue ao juiz canônico e percorre `recusa_batida_retratada`. Declaração da injeção está correta. (`test_e1_v15f.py:296`, `:307`, `:312`; produção `models.py:2885`, `:2890`.) |
| A12 | Primeiro teste recusa na entrada, antes do reuso. Segundo desativa essa guarda e espião exige chamada com canônica, atingindo o gate do reuso. Não prova ocorrência natural do segundo gate como primeira recusa. (`test_e1_v15f.py:327`, `:334`, `:339`, `:343`; produção `models.py:2930`.) | [nao verificado]
| G6 | Injeta erro na porta real utilizada pelo comando, exige erros=3, PAREI, rollback e fotos iguais. Não há escrita parcial antes do erro nessa prova. (`test_e1_v15f.py:215`, `:217`, `:225`; produção `migrar_data_evento_dia_do_turno.py:400`, `:411`.) |
| G7 | Remove índice, cria gêmea do fato, confirma universo conjunto; apply exige primeira migrada e segunda recusada por colisão. (`test_e1_v15f.py:228`, `:236`, `:240`, `:246`.) |
| G8 | Primeiro membro usa porta real e grava; gêmea mente sucesso. Exige observação da escrita parcial, rollback de ambos e migração de outro grupo. Prova efetiva do atomic por grupo. (`test_e1_v15f.py:250`, `:257`, `:263`, `:266`, `:268`; contrato `migrar...py:407`, captura `:408`.) |
| C1 | Absorção efetiva em `_fonte_materializada`. (`test_e1_v15f.py:361`; `migrar...py:70`.) |
| C2 | Materializada sem plantada e sem via de absorção. (`test_e1_v15f.py:365`; `migrar...py:72`.) |
| C3 | Duas **batidas plantadas**, não dois turnos vivos candidatos. (`test_e1_v15f.py:371`; `migrar...py:73`.) |
| C4 | Plantada fora do intervalo usado para carregar turnos, nenhuma associação; prova `not ts`, não o outro ramo `len(ts)>1`. (`test_e1_v15f.py:375`; `migrar...py:77`, `:145`.) |
| C5 | Remove saída e atinge exclusão da materializada antes do juiz do novo dia. (`test_e1_v15f.py:380`; `migrar...py:158`.) |
| C6 | Juiz None injetado, atinge `_Fora sem_fonte`. (`test_e1_v15f.py:386`; `migrar...py:161`.) |
| C7 | Dia divergente injetado, atinge juiz × turno da janela. (`test_e1_v15f.py:392`; `migrar...py:165`.) |
| C8 | Materializa órfã e planta saída, atinge fora do aval. (`test_e1_v15f.py:399`; `migrar...py:168`.) |
| C9 | Chama DRY real com sucesso e colisão. “Sem escrita” deve significar **sem persistência**: a porta escreve e o atomic desfaz. (`test_e1_v15f.py:405`; `fio.py:1029`, `:1030`, `:1032`.) |
| Conversa | Porta real cria conversa datada; DRY/apply verificam exclusão/não migração; contador injeta NULL e compara com conversa datada. Rótulo só exige substring, não prova numeração do universo. (`test_e1_v15f.py:64`, `:72`, `:78`, `:96`, `:106`.) |
| Ordem A8/A11 + G4/G3 | Um cenário combina rechave + costura; migrar usa base limpa, três migrantes, e reversão. **Não exercita G3 vetado**, nem prova A11 sem rechave; não verifica pk crescente. (`test_e1_v15f.py:117`, `:140`, `:147`; rastreador `test_e1_v9.py:51`, `:94`.) |
| Catraca abrir | Agora exige exatamente `transaction.atomic()` como dono do bloco `_nascer` + contrato; mutações if True/savepoint=False/nullcontext são verificadas. (`test_e1_v15e_contrato_de_saida.py:109`, `:113`, `:206`.) |
| Catraca migrar | Espião chama contrato real e verifica três PKs, dia relido e dia esperado; recompila `_migrar` real e muta chamada real. (`test_e1_v15f.py:165`, `:182`, `:185`, `:194`, `:199`, `:203`.) |
| M6 | Deixou de aceitar motivo arbitrário: agora exige substring `pergunta_unica_por_episodio`. Prova morte pelo índice, não tratamento de acervo já duplicado. (`test_e1_v15e_contrato_de_saida.py:471`, `:548`, `:561`, `:567`.) |

**Caminhos reais ainda sem linha/prova na extensão nova:** plantada associada a dois turnos; dois turnos só de saída; múltiplos turnos abertos candidatos da órfã; órfã sem marcos. São ramos distintos dos C3/C4 adicionados. (`migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`; novos C3/C4 `test_e1_v15f.py:371`, `:375`.) A seção fornecida admite alguns, mas omite explicitamente os dois ramos adicionais `:78` e `:120` na lista final. **[nao verificado quanto à matriz não existente como arquivo na cópia]**.

**Mutantes plausíveis ainda descobertos:**

1. **Ordem invertida dentro da classe:** trocar apenas `order_by('pk')` por `order_by('-pk')` na query de Pergunta/Chamado/Disputa do helper. `violacoes_linhas` só acompanha rank, presença de algum Colaborador e PK desconhecida; não compara PKs. Seq descendente foi aceita em execução isolada do detector. (`test_e1_v9.py:94`, `:98`, `:106`, `:109`; exclusão explícita `:51`.) O teste antigo SQL ASC cobre **só Colaborador**, portanto escolher mutante em outra classe. (`test_e1_v13_ordem.py:59`, `:68`, `:71`.) [nao verificado]

2. **Contrato pulado num ramo do migrar:** trocar a guarda por `if ok and ln['p'].chamado_destino_id is None:`. O detector AST continua devolvendo `[]`: ele busca chamada posterior em qualquer descendente do corpo do try, sem testar condição. (`test_e1_v15e_contrato_de_saida.py:143`, `:147`, `:149`; guarda de produção `migrar...py:406`.) Novos espiões usam migrantes da `_Cenario`, produzidos sem destino: `_pergunta` chama abrir sem destino, e gêmea também não passa destino. Assim o ramo com destino fica sem testemunha nesses testes. (`test_plantio_noturno_mais1.py:62`, `:64`; `test_migrar_data_evento_dia_do_turno.py:55`, `:65`, `:68`; `test_e1_v15f.py:233`, `:182`.) Sobrevivência da suíte completa **[nao verificado]**; sobrevivência do detector AST confirmada em memória. [nao verificado]

3. **Atomic retirado da reversão:** substituir o `with transaction.atomic()` da reversão por `if True:`. A nova prova já fornece seu próprio atomic ao redor da chamada e só percorre sucesso; as bases são `django.test.TestCase`. Portanto não distingue transação da porta e transação fornecida pelo teste. (`migrar...py:465`; `test_e1_v15f.py:151`, `:153`; `_Cenario` em `test_migrar...py:48`; base `test_plantio_noturno_mais1.py:39`.) A catraca inspeciona somente `_migrar`, não `_reverter`. (`test_e1_v15e_contrato_de_saida.py:141`.) Suíte completa **[nao verificado]**. [nao verificado]

O arnês não é derivação paralela manual: extrai AST/trecho da função real, recompila com uma substituição e instala objeto no módulo/classe de produção. (`test_e1_v15e_contrato_de_saida.py:331`, `:342`, `:349`, `:356`; `test_e1_v15f.py:185`.)

## 08/11 17:53 · sessao bdaaabc83e2f · - · - · origem=Ronald (tmux)

**Ordem global integral: NÃO PROCEDENTE; cura parcial.** Os casos isolados novos são ordenados, mas o helper não impede que uma chamada posterior acrescente linhas anteriores na ordem da mesma transação. (`app/chamados/models.py:2625`, `app/chamados/models.py:2639`, `app/chamados/models.py:2648`, `app/chamados/models.py:2661`, `app/chamados/models.py:2665`.)

| Caminho | Conferência |
|---|---|
| Nascimento isolado | Toma C e insere P; não toma D/Ch explicitamente. O rastreador não cobre os locks de FK do INSERT. (`app/chamados/models.py:2811`, `app/chamados/models.py:2974`; `app/chamados/tests/test_e1_v9.py:51`.) |
| Reuso sem rechave, mesmo dono, ato isolado | C → Ch da costura → UPDATE P; localmente conforme. (`app/chamados/models.py:2811`, `app/chamados/models.py:2938`, `app/chamados/services/fio.py:768`, `app/chamados/models.py:2964`.) |
| Reuso com rechave, mesmo dono, ato isolado | O novo bloco toma D/Ch/P antes do rechave e da costura; cura a inversão antiga P→Ch. (`app/chamados/models.py:2910`, `app/chamados/services/fio.py:969`, `app/chamados/services/fio.py:768`.) |
| `rechavear_no_reuso` isolado | Veto por leitura, depois porta C→D→P; o acesso C posterior em `fato_livre_para` é reentrante. (`app/chamados/services/fio.py:1020`, `app/chamados/services/fio.py:1026`, `app/chamados/services/fio.py:969`, `app/chamados/models.py:2740`.) |
| `_migrar` / `_reverter` | Os conjuntos completos de linhas candidatas entram C→D→Ch→P antes dos laços; cura a inversão entre grupos/linhas identificada anteriormente. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:374`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:375`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:468`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:469`.) |
| Chamadores com atomic aberto | Permanecem contraexemplos de linhas fora do conjunto inicialmente travado, descritos abaixo. (`app/chamados/services/declaracao_texto.py:324`, `app/chamados/services/disputa_emissao.py:530`, `app/chamados/models.py:2912`.) |

**Contraexemplo normal, mesmo colaborador, D depois de Ch:** `declarar_hora_colab` abre atomic e chama `disputa_para_declaracao`, que trava C, disputas **abertas** e chamados **vivos**. Uma canônica de disputa fechada fica fora desse conjunto; `_candidata_reusavel` devolve `None` quando sua disputa difere, fazendo o chamador passar por `abrir`. Se o episódio do contexto da disputa aberta é o da canônica fechada com data antiga, `_nascer` encontra a linha pelo episódio e o novo bloco adquire D_fechada depois dos Ch_vivos. D_fechada também pode ter pk menor que a D_aberta já adquirida. (`app/chamados/services/declaracao_texto.py:324`, `app/chamados/services/declaracao_texto.py:332`, `app/chamados/services/disputa_emissao.py:530`, `app/chamados/models.py:2646`, `app/chamados/models.py:2656`, `app/chamados/services/declaracao_texto.py:248`, `app/chamados/services/declaracao_texto.py:348`, `app/chamados/models.py:2832`, `app/chamados/models.py:2695`, `app/chamados/models.py:2912`.) A mesma composição existe em `aplicar_declaracao`. (`app/chamados/services/declaracao_texto.py:626`, `app/chamados/services/declaracao_texto.py:629`, `app/chamados/services/declaracao_texto.py:636`.) Execução do contraexemplo e ocorrência em produção: **[nao verificado]**.

**Contraexemplo sem rechave, pk menor de Chamado depois de maior:** na composição acima, destino terminal não integra os Ch_vivos do topo. Reuso de canônica já datada no dia correto pode pedir a costura para esse destino, cuja linha é adquirida antes da decisão de recusar por terminal. Se seu pk for menor que um Ch_vivo já adquirido, a ordem entre pks é invertida. (`app/chamados/models.py:2656`, `app/chamados/models.py:2904`, `app/chamados/models.py:2926`, `app/chamados/models.py:2938`, `app/chamados/services/fio.py:768`, `app/chamados/services/fio.py:770`, `app/chamados/services/fio.py:785`.) Execução: **[nao verificado]**.

**Contraexemplo adicional, colaboradores:** `_nascer` trava primeiro o dono da disputa, depois o novo bloco inclui o dono do destino. E4-c compara dias, sem conferir igualdade de donos; assim o código aceita tentar adquirir C_destino menor depois de C_disputa maior. (`app/chamados/models.py:2811`, `app/chamados/models.py:2911`, `app/chamados/juizes.py:1997`, `app/chamados/juizes.py:2027`, `app/chamados/juizes.py:2043`.) Ocorrência e execução: **[nao verificado]**.

**Reapontamento também não garante ordem de pk entre perguntas:** quando `p` sem dia é a canônica menor pk e há outras vivas retratáveis, o código retrata as perdedoras primeiro e só depois salva `p`. As perdedoras são ordenadas entre si, excluindo a canônica; portanto UPDATE P_maior → UPDATE P_menor continua possível, sem pré-trava coletiva de P nesse método. (`app/chamados/services/fio.py:913`, `app/chamados/services/fio.py:932`, `app/chamados/services/fio.py:889`, `app/chamados/services/fio.py:1207`, `app/chamados/services/fio.py:1253`, `app/chamados/services/fio.py:1271`, `app/chamados/models.py:3145`, `app/chamados/services/fio.py:892`.) Execução com acervo duplicado: **[nao verificado]**.

**Lista / `None` / reentrância:** `_pks` elimina `None`, elimina repetidos e ordena; listas, tuplas, conjuntos e frozensets são aceitos para colaboradores, e a query também usa pk crescente. `None` escalar produz lista vazia. O modo `abertas_do_colaborador` admite no máximo um colaborador. O helper sempre reexecuta os `SELECT FOR UPDATE`; reentrância significa não adquirir novamente linha já possuída, não uma validação da ordem das próximas chamadas. (`app/chamados/models.py:2625`, `app/chamados/models.py:2631`, `app/chamados/models.py:2633`, `app/chamados/models.py:2636`, `app/chamados/models.py:2639`.)

**Prova insuficiente para ordem por pk:** `violacoes_linhas` só compara ranks de tabelas e presença anterior de algum colaborador; seu limite explicitamente exclui ordem por pk. Trocar uma query do helper para `order_by('-pk')` conserva os ranks e não é detectado por esse verificador. Os testes novos usam exatamente esse verificador. (`app/chamados/tests/test_e1_v9.py:51`, `app/chamados/tests/test_e1_v9.py:94`, `app/chamados/tests/test_e1_v9.py:99`, `app/chamados/tests/test_e1_v9.py:106`, `app/chamados/tests/test_e1_v15f.py:131`, `app/chamados/tests/test_e1_v15f.py:145`, `app/chamados/tests/test_e1_v15f.py:155`.) O teste novo de abrir começa sem os locks anteriores dos chamadores e usa uma única disputa e destino. (`app/chamados/tests/test_e1_v15f.py:118`, `app/chamados/tests/test_e1_v15f.py:122`, `app/chamados/tests/test_e1_v15f.py:123`.)

`select_for_update` acrescentado fora de `trava_na_ordem` pelo diff fornecido: **não encontrado no diff fornecido [nao verificado por histórico Git]**; a cópia disponível não é repositório Git. **[nao verificado]**

## 08/11 17:53 · sessao d3b39850338a · - · - · origem=Ronald (tmux)

**Cura local da conversa: PROCEDENTE; cura semântica global: NÃO PROCEDENTE.** A ida do migrar exclui conversa e o contador passa a incluir conversa NULL, mas continuam leitores sem discriminador e a reversão continua podendo regravar conversa. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`; `app/chamados/services/fio.py:1118`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:474`.)

**3 — ESMERIL semântico**

- `abrir_conversa` grava dia civil por `localdate`; `dia_do_turno_da_linha` devolve pedido/gravado no ramo conversa. O contador compara esse valor consigo mesmo: detecta conversa NULL, mas não descobre conversa datada no dia errado. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:1508`; `app/chamados/services/fio.py:1125`; `app/chamados/services/fio.py:1130`.)
- `reuso_sem_rechave` continua excluindo conversa, coerente com nascimento sempre novo. (`app/chamados/services/fio.py:1051`; `app/chamados/models.py:2857`.)
- **Reversão sem proteção:** `handle --reverter` desvia antes do queryset filtrado; `_reverter` lê qualquer PK do JSON, recebe o dia dele e chama a porta de rechaveamento sem testar conversa. A porta aceita a conversa no dedup e no E4-c, escreve o dia fornecido e escolhe célula desse dia. Com competências abertas e célula compatível com o JSON, esse caminho pode confirmar alteração do dia civil da conversa. Trata-se de contraexemplo estático; existência de arquivo legado contendo conversa e execução em produção são `[nao verificado]`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:199`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:468`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:478`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:485`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:489`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:493`; `app/chamados/models.py:2741`; `app/chamados/juizes.py:1997`; `app/chamados/services/fio.py:989`; `app/chamados/services/fio.py:990`.)
- **Materialização ainda lê conversa como turno:** a seleção de perguntas prontas não exclui conversa; o ramo de hora chama o plantio; este lê `p.data_evento`, busca marcos para esse dia e ajusta saída overnight. A porta de conversa aceita `tipo_resposta='hora'` em `**campos`; o teste novo cria precisamente conversa `orfao_14h/hora`. Ocorrência em produção: `[nao verificado]`. (`app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1221`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:371`; `app/chamados/services/materializacao.py:414`; `app/chamados/services/materializacao.py:421`; `app/chamados/models.py:2764`; `app/chamados/tests/test_e1_v15f.py:64`.)
- **Cuidado com a triagem:** o emissor normal já impede pergunta de hora sem dia antes de chegar a `abrir_conversa`; não sustentar que sua linha 439 necessariamente emite conversa-hora no código atual. (`app/chamados/services/disputa_emissao.py:425`; `app/chamados/services/disputa_emissao.py:430`; `app/chamados/services/disputa_emissao.py:439`.)
- **Gate de Chamado:** `_ancoradas` admite conversa; `_vivas_entre` consulta competência com `data_evento` cru; a busca de sobrevivente cobra o dia cru. (`app/chamados/models.py:344`; `app/chamados/models.py:364`; `app/chamados/models.py:390`.)
- **Pauta/tranca:** seleciona perguntas pela janela de `data_evento`, sem conversa; a reabertura também seleciona assim. (`app/chamados/services/competencia_trancada.py:106`; `app/chamados/services/competencia_trancada.py:251`.)
- **Lavra:** lê o campo para calcular janela de vida de turno; o cálculo usa `date.today()`, e a documentação ainda associa conversa a NULL. (`app/chamados/veredito_lavrado.py:229`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:107`.)
- **Tela/recusa:** `data_do_fato_de` compara `data_evento` com dia do chamado sem discriminar conversa, e a materialização pode registrar `DATA_EM_DISPUTA`; folha agrupa resposta pelo dia cru; tela de dia também seleciona pelo dia cru. (`app/chamados/services/fio.py:85`; `app/chamados/services/fio.py:88`; `app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1213`; `app/folha/previa.py:65`; `app/folha/previa.py:67`; `app/ponto/services/dia_do_colab.py:65`.)
- O próprio código registra estes leitores numa fila, mas a conclusão acima vem dos leitores efetivos, não da declaração da fila. (`app/chamados/services/fio.py:1309`; `app/chamados/services/fio.py:1317`.)

**6 — Inventário dos campos nos caminhos alcançados pelo delta**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `data_evento` | Atribuição única em `gravar_dia_do_fato`; nascimento persiste; costura que data persiste; rechaveamento persiste. (`app/chamados/services/fio.py:800`; `app/chamados/models.py:2973`; `app/chamados/models.py:2974`; `app/chamados/services/fio.py:863`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:989`; `app/chamados/services/fio.py:995`.) | Juiz/adaptador da linha, contadores, contrato, classificação/migração/reversão, lavra, materialização e os leitores sem discriminador acima. (`app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:1061`; `app/chamados/services/fio.py:1085`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:392`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:485`.) |
| `celula` | Nascimento escolhe pelo dia do fato e insere; reuso troca FK e salva; rechaveamento escolhe pelo novo dia e salva. (`app/chamados/models.py:2860`; `app/chamados/models.py:2971`; `app/chamados/models.py:2974`; `app/chamados/models.py:2924`; `app/chamados/models.py:2964`; `app/chamados/services/fio.py:986`; `app/chamados/services/fio.py:990`; `app/chamados/services/fio.py:995`.) | Pré-trava do delta recebe perguntas/destinos, não célula; reversão fotografa/compara FK; rechaveamento lê célula velha; juiz lê `p.celula`; post-save/outbox acorda célula da FK ou do par colaborador/dia. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:355`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:493`; `app/chamados/services/fio.py:972`; `app/chamados/juizes.py:1121`; `app/chamados/signals.py:244`; `app/chamados/models.py:3196`; `app/chamados/models.py:3199`.) |
| `chamado_destino` | Nascimento; reuso dentro da porta; costura `reapontar_resolvedora`. A guarda de costura faz alteração temporária em memória e restaura. (`app/chamados/models.py:2971`; `app/chamados/models.py:2940`; `app/chamados/models.py:2964`; `app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:779`; `app/chamados/services/fio.py:783`.) | Delta monta pré-travas com destino pedido/atual; migrar e reversão pré-travam os destinos; âncora do fio, E4-c, guarda terminal, lavra/materialização usam o destino. (`app/chamados/models.py:2912`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:471`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:991`; `app/chamados/services/materializacao.py:339`.) |
| `universo` | Variáveis locais zeradas e incrementadas antes dos descartes, entregues no dicionário. (`app/chamados/services/fio.py:1054`; `app/chamados/services/fio.py:1056`; `app/chamados/services/fio.py:1066`; `app/chamados/services/fio.py:1121`; `app/chamados/services/fio.py:1123`; `app/chamados/services/fio.py:1133`.) | Backfill imprime os dois valores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:550`.) |
| `universo_rotulo` | Literais produzidos pelos dois contadores. (`app/chamados/services/fio.py:1067`; `app/chamados/services/fio.py:1134`.) | Backfill imprime literalmente; teste novo apenas exige substring `"ANTES dos descartes"`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:550`; `app/chamados/tests/test_e1_v15f.py:109`.) |

**7 — Classes**

- **Campo com dois significados permanece**, com leitores que ignoram o discriminador e reversão que ainda aceita conversa; a inclusão no contador não estabelece independentemente qual era o dia civil correto. (`app/chamados/services/fio.py:1508`; `app/chamados/models.py:364`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:489`.)
- **Relógio fora da borda permanece preexistente:** `expira_em` consulta `date.today()`; as alterações de contadores não acrescentam relógio. (`app/chamados/veredito_lavrado.py:112`; `app/chamados/services/fio.py:1051`; `app/chamados/services/fio.py:1118`.)
- **Segundo atribuidor de `data_evento` introduzido pela 15f:** não identificado nos arquivos alterados; os três persistidores continuam recebendo o helper. (`app/chamados/services/fio.py:800`; `app/chamados/models.py:2973`; `app/chamados/services/fio.py:863`; `app/chamados/services/fio.py:989`.)
- **Segundo juiz introduzido pelos contadores:** não identificado: usam `dia_do_turno_da_linha` e o predicado real de saída; o contador de reuso também chama a porta real em DRY. (`app/chamados/services/fio.py:1058`; `app/chamados/services/fio.py:1063`; `app/chamados/services/fio.py:1125`; `app/chamados/services/fio.py:1130`.)
- Regra de autoridade lida: documentação manda conferir autoridade viva no código. (`app/docs/JUIZES.md:10`; `app/docs/JUIZES.md:14`.)

## 08/11 17:00 · sessao a9251fb1a473 · FIO-E0-V4-E1 · verificador · origem=Code

**NÃO PROCEDENTE a conformidade integral da 15e.** A cura do reuso NULL e a passagem das saídas de sucesso pelo contrato estão presentes; permanecem uma mudança indevida de significado possível no migrar, inversão de travas e lacunas nas provas da matriz. (`app/chamados/models.py:2904`, `app/chamados/models.py:2791`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:201`; `app/chamados/services/fio.py:969`, `app/chamados/services/fio.py:768`.)

Execução atual das suítes, resultados GREEN/RED dos anexos, números e ocorrência dos contraexemplos em produção: **[nao verificado]**.

1. **Escrita fora do helper: NÃO PROCEDENTE como existência de outro atribuidor; há persistências fora dele.**

   A única atribuição autônoma encontrada é `linha.data_evento = dia_do_turno`; o helper deliberadamente não salva. (`app/chamados/services/fio.py:800`, `app/chamados/services/fio.py:796`.)

   Incluindo literalmente os `save` solicitados, estes são os pontos que persistem o campo:

   | Caminho | Recebe o valor do helper | Persiste |
   |---|---|---|
   | Nascimento | `app/chamados/models.py:2964` | `app/chamados/models.py:2965` — `force_insert=True` |
   | Costura que data linha | `app/chamados/services/fio.py:863` | `app/chamados/services/fio.py:892` — campo em `campos` |
   | Rechaveamento | `app/chamados/services/fio.py:989` | `app/chamados/services/fio.py:995` — `campo_dia` |

   Portanto, “nenhuma persistência fora do helper” seria falso; “nenhum segundo sítio escolhe e atribui o valor” corresponde ao código encontrado. (`app/chamados/services/fio.py:790`, `app/chamados/models.py:2964`, `app/chamados/services/fio.py:863`, `app/chamados/services/fio.py:989`.)

   Não encontrei atribuição adicional por `update`, `bulk_update`, `setattr`, `F()`, SQL cru ou migração de dados. Conferi também os argumentos dinâmicos: os chamadores atuais de `descarimbar_em_lote` passam resposta/validação, e o lote de lavras usa seus três campos próprios. (`app/chamados/models.py:1555`; `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1531`; `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`.)

2. **Contrato em toda saída de sucesso: PROCEDENTE, com limite semântico.**

   `abrir` e `abrir_conversa` chamam `_sob_contrato`; este executa `_nascer` e depois `contrato_de_saida` dentro do mesmo `atomic`, antes de retornar. Ambos os retornos de `_nascer` entregam `_dia_fato`. (`app/chamados/models.py:2759`, `app/chamados/models.py:2771`, `app/chamados/models.py:2789`, `app/chamados/models.py:2956`, `app/chamados/models.py:2977`.)

   Não encontrei retorno antecipado ou captura que devolva pergunta com sucesso contornando essa chamada. O `except` final de `_nascer` cobre a trilha de emissão, e o retorno continua passando pelo wrapper. (`app/chamados/models.py:2969`, `app/chamados/models.py:2974`, `app/chamados/models.py:2791`.)

   No migrar, somente resultados `ok` são considerados migrados, e cada um passa pelo contrato com uma releitura do banco. Veto, recusa e exceção seguem canais de recusa. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:378`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:389`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:392`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:399`.)

   **Instância versus banco:** o reuso rechaveado relê a pergunta; o nascimento faz `force_insert`; o migrar faz `get(pk)`. Não identifiquei nesses caminhos uma saída bem-sucedida cuja instância tenha dia diferente do persistido. O contrato, porém, só compara os valores recebidos: não consulta o banco nem julga independentemente o dia. (`app/chamados/models.py:2911`, `app/chamados/models.py:2965`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:391`; `app/chamados/services/fio.py:1080`.)

3. **Matriz completa e comprovada: NÃO PROCEDENTE.**

   Há correspondências incorretas entre linha anunciada e teste citado: [nao verificado]

   | Linha | O que a prova efetivamente faz |
   |---|---|
   | A5 | Espera `ReusoSemRechave` por colisão, não demonstra o ramo anunciado de `FatoEmOutroDia`. (`app/chamados/tests/test_e1_v13.py:143`, `app/chamados/tests/test_e1_v13.py:146`.) |
   | A6 | Exclui a retratada do universo e reusa K; não elege uma canônica retratada e depois a recusa. (`app/chamados/tests/test_e1_v15b.py:136`, `app/chamados/tests/test_e1_v15b.py:139`, `app/chamados/tests/test_e1_v15b.py:140`.) |
   | A12 | Exercita `resolvedora_valida` diretamente, sem percorrer a recusa de `abrir`. (`app/chamados/tests/test_e1_v15b.py:153`; ramo: `app/chamados/models.py:2921`.) |
   | G6 | O teste de DRY não injeta exceção no grupo. (`app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:136`.) |
   | G7 | Exercita `_grupos_fora` do backfill, não colisão durante o apply do migrar. (`app/chamados/tests/test_e1_v13d_exportada_fora.py:118`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:124`.) |
   | G8 | S4 chama `_rodar()` em DRY e verifica texto; não prova rollback de apply após escrita parcial de um grupo com vários membros. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:302`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:86`, `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:91`.) |

   Faltam caminhos reais da classificação: absorção, fontes ambíguas, ausência de turno, materializada cuja data não corresponde à saída, divergência entre juiz e turno da janela e `orfao_14h` materializada fora do aval. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:155`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:162`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:165`.)

   Também faltam o DRY real de `rechavear_no_reuso` e a entrada de conversa no migrar descrita abaixo. (`app/chamados/services/fio.py:1028`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:201`.)

4. **Mesmo significado entre porta, juiz e contadores para conversa: NÃO PROCEDENTE.**

   A porta da conversa toma **dia civil** por `localdate`; `dia_do_turno_da_linha` devolve o pedido ou o gravado quando `conversa=True`; o helper escreve esse valor e o contrato compara com ele. Não há julgamento de turno nesse ramo. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1499`, `app/chamados/services/fio.py:1500`, `app/chamados/services/fio.py:800`, `app/chamados/services/fio.py:1080`.)

   Ambos os contadores excluem conversa. Assim, não cobrem todas as linhas submetidas ao contrato das duas portas. (`app/chamados/services/fio.py:1048`, `app/chamados/services/fio.py:1112`.)

   **Contraexemplo estático adicional — conversa migrada de dia civil para dia do turno:**

   - `abrir_conversa` permite `motivo='orfao_14h'`, `tipo_resposta='hora'`, gravando dia civil D; o queryset do migrar seleciona esses campos sem excluir conversa. (`app/chamados/models.py:2764`, `app/chamados/models.py:2771`, `app/chamados/models.py:2962`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:201`.)
   - Com um turno aberto iniciado em D−1 cuja saída prevista seja D, `_fonte_viva` seleciona sua entrada; a classificação obtém D−1 do juiz e o põe em `novo`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:105`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:112`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:123`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:157`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:164`.)
   - Conversa forma universo unitário, passa por `fato_livre_para`, não recebe recusa de episódio e é aceita pelo E4-c. (`app/chamados/services/fio.py:1788`; `app/chamados/models.py:2741`; `app/chamados/services/fio.py:1419`; `app/chamados/juizes.py:1997`.)
   - Satisfeitos os demais requisitos do apply, o rechaveamento grava D−1; o contrato aceita porque compara com o mesmo `ln['novo']`. Os dois contadores continuam excluindo essa linha. (`app/chamados/services/fio.py:989`, `app/chamados/services/fio.py:995`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:391`; `app/chamados/services/fio.py:1048`, `app/chamados/services/fio.py:1112`.)

   A lavra também lê `data_evento` diretamente para expiração, embora sua documentação ainda associe conversa a NULL. O campo com dois significados e leitores pendentes continua declarado no próprio código. (`app/chamados/veredito_lavrado.py:107`, `app/chamados/veredito_lavrado.py:229`; `app/chamados/services/fio.py:1307`.)

5. **Contadores: PROCEDENTE que agora mordem a classe NULL, dentro de universos diferentes.**

   | Contador | Universo e contagem efetiva |
   |---|---|
   | `reuso_sem_rechave` | Fatos com episódio, fora das vias excluídas, incluindo NULL e sem corte de horizonte. Pula exceções de domínio e dias iguais; conta somente quando o rechaveamento real em DRY recusa. (`app/chamados/services/fio.py:1048`, `app/chamados/services/fio.py:1056`, `app/chamados/services/fio.py:1060`.) |
   | `sem_dia_na_saida` | Fatos com ou sem episódio/data, fora das vias excluídas. Episódio sem dia vira `None` e conta; ponte entre dois dias fica fora; usa o predicado real da saída. (`app/chamados/services/fio.py:1112`, `app/chamados/services/fio.py:1120`, `app/chamados/services/fio.py:1122`, `app/chamados/services/fio.py:1124`.) |

   NULL com episódio cujo juiz dá D entra no primeiro contador: `[None,D]` produz competência desconhecida e recusa. Também viola diretamente o segundo. (`app/chamados/services/fio.py:1020`, `app/chamados/services/fio.py:1234`, `app/chamados/services/fio.py:1080`.)

   `universo` é incrementado antes dos descartes; não significa “quantidade efetivamente comparada”. São fotografias do acervo atual, não acumuladores das recusas ocorridas nas portas. (`app/chamados/services/fio.py:1053`, `app/chamados/services/fio.py:1062`, `app/chamados/services/fio.py:1117`, `app/chamados/services/fio.py:1126`.)

   **Custo:** o predicado não consulta; os contadores consultam o juiz por identidade ainda não memorizada, carregam batida e podem percorrer vizinhos. O DRY do reuso pode adquirir travas, salvar e produzir trilha, desfazendo depois; NULL é recusado pelo veto antes dessa execução. (`app/chamados/services/fio.py:1080`, `app/chamados/services/fio.py:1392`, `app/chamados/services/fio.py:1396`, `app/chamados/services/fio.py:1547`, `app/chamados/services/fio.py:1028`, `app/chamados/services/fio.py:1022`.)

   Os leitores de produção localizados são as duas chamadas no DRY do backfill. Contagem e latência de produção: **[nao verificado]**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:540`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`.)

6. **Mutantes sobre produção: PROCEDENTE quanto ao alvo; prova desigual quanto à mordida.**

   O arnês extrai a função do arquivo real, recompila a substituição e a instala na classe/módulo utilizado pela produção. Não é uma implementação paralela escrita à mão. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:319`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:330`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:344`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:349`.)

   | Classe do aval | Mordida implementada |
   |---|---|
   | Remover helper num ramo | M1/M1b alteram `_nascer` e exigem `SemDiaNaSaida`. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:495`.) |
   | Helper não atribui/persistência omitida | M2a remove atribuição; M2b retira o campo do `save`; M2c retira releitura. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:502`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:506`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:510`.) |
   | Juiz devolve data civil | M3 altera `_dia_do_fato`; o caso noturno exige o dia inicial. O contrato sozinho aceita juiz e campo errados juntos. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:468`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:514`; `app/chamados/services/fio.py:1080`.) |
   | NULL com identidade resolvível | M4 restaura a guarda antiga; M4b também desliga o contrato, expondo o retorno NULL. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:519`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:524`.) |
   | Reusar outro episódio | M5 altera a eleição real; o cenário verifica criação, PK, episódio, dia e preservação da anterior. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:447`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:530`.) |
   | Duas canônicas | M6 força `ex=None`, mas aceita qualquer exceção como derrota, sem motivo específico. Pode morrer no índice ao inserir; não demonstra tratamento de acervo previamente duplicado. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:483`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:536`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:553`; `app/chamados/models.py:2965`, `app/chamados/models.py:3434`.) |

   **Catracas insuficientes para a promessa completa:** em mutações somente em memória, o detector de abertura aceitou retirar o `atomic` do wrapper; o de migrar aceitou tornar o contrato inalcançável. O primeiro verifica instruções/ordem, sem exigir o `atomic`; o segundo encontra uma chamada posterior na AST, sem exigir sua execução. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:109`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:116`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:138`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:142`.)

7. **Classes anteriores: há ocorrências presentes.**

   **Ordem de trava — PROCEDENTE:** no reuso que rechaveia, a transação toma Colaborador → Disputa → Pergunta; depois, havendo destino diferente, pode tomar Chamado novo pela costura. É Pergunta antes de Chamado, contrariando a ordem global declarada por transação. (`app/chamados/services/fio.py:969`; `app/chamados/models.py:2917`, `app/chamados/models.py:2929`; `app/chamados/services/fio.py:768`; regra: `app/chamados/models.py:2604`.)

   O migrar também trava C/D/P por membro, podendo adquirir a Disputa seguinte após a Pergunta anterior. Deadlock ocorrido: **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:370`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:373`.)

   **`savepoint=False` — não encontrei a fuga sugerida:** um `IntegrityError` não capturado nos saves de `_nascer` propaga até o `atomic` de `_sob_contrato`, desfazendo o ato. A trilha tem seu próprio savepoint, com captura externa; não sustenta aqui uma conclusão de sucesso com rollback silencioso. (`app/chamados/models.py:2807`, `app/chamados/models.py:2955`, `app/chamados/models.py:2965`, `app/chamados/models.py:2789`; `app/core/observ.py:105`, `app/core/observ.py:133`.)

   **Campo com dois significados e selo incompleto — presentes**, pelos itens 4 e 6. Segundo juiz geométrico ou segundo atribuidor introduzido pelo contrato: não encontrados; ele compara valores, enquanto a geometria continua delegada a T1. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1080`, `app/chamados/services/fio.py:1397`; `app/ponto/turnos.py:2925`.)

   **Relógio:** a conversa lê `localdate` na porta; o contrato não lê relógio. Continua existindo `date.today()` dentro de `expira_em`, chamado pela lavra. Atribuir esse sítio à alteração da 15e: **[nao verificado]**. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1080`; `app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:229`.)

8. **Laudo opus 4 integralmente curado: NÃO PROCEDENTE; os dois bloqueantes NULL estão curados no código.**

   O contraexemplo anterior agora segue: a linha NULL do episódio entra no universo pelo dia do juiz, pode ser eleita, satisfaz `None != D`, chama `rechavear_no_reuso`, recebe veto por competência desconhecida e levanta `ReusoSemRechave` antes da costura. (`app/chamados/services/fio.py:1679`, `app/chamados/services/fio.py:1682`; `app/chamados/models.py:2885`, `app/chamados/models.py:2904`; `app/chamados/services/fio.py:1020`, `app/chamados/services/fio.py:1234`; `app/chamados/models.py:2908`.)

   Se a porta mentir e declarar sucesso sem persistir, a releitura entrega o valor gravado ao contrato. A linha NULL passou a integrar ambos os contadores, e existem casos explícitos de recusa sem escrita e contagem dessa classe. (`app/chamados/models.py:2911`, `app/chamados/models.py:2791`; `app/chamados/services/fio.py:1048`, `app/chamados/services/fio.py:1112`; `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:302`, `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:320`.)

   Dos itens de fila do laudo, continuam: chamadas sem captura local da recusa; rechaveamento sem veto interno; DRY que toma travas; destino `None` ignorado no reuso; regex limitado; e pontos cegos declarados da catraca. (`app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/fio.py:963`, `app/chamados/services/fio.py:1029`; `app/chamados/models.py:2917`; `app/chamados/tests/test_contract_pergunta_fato.py:12`; `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:15`, `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:43`.)

   O item dos dois significados permanece, com o contraexemplo do migrar acima. Cura integral dos cabeçalhos e comprovação histórica dos RED citados: **[nao verificado]**. (`app/chamados/services/fio.py:1307`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:201`.)

## 08/11 16:58 · sessao c9cd28074a03 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Escritor autônomo extra: NÃO encontrado.** A atribuição está em `app/chamados/services/fio.py:800`. Persistem esse valor `app/chamados/models.py:2965`, `app/chamados/services/fio.py:892` e `app/chamados/services/fio.py:995`; todos recebem o campo do helper (`:2964`, `:863`, `:989`). O helper explicitamente não salva (`app/chamados/services/fio.py:796`). Varredura AST em memória incluindo migrações encontrou somente essa atribuição; resultados dinâmicos externos à cópia [nao verificado].

- **Campos dinâmicos conferidos:** `descarimbar_em_lote(**outros)` pode tecnicamente aceitar outros campos (`app/chamados/models.py:3198`, `:3212`), mas seus dois chamadores atuais passam apenas campos de resposta/validação: `app/chamados/models.py:1555` e `app/chamados/services/disputa_emissao.py:1494`, `:1531`. O ramo `qs.update(**campos)` usa o mesmo dict (`:1533`). Lote da lavra usa apenas seus três campos (`app/chamados/veredito_lavrado.py:194`, `:207`). Os saves dinâmicos de carimbo também não incluem `data_evento` (`app/chamados/models.py:3074`, `:3097`, `:3098`, `:3132`, `:3136`, `:3156`, `:3158`). Save geral da própria pergunta identificado apenas na inserção e no override delegado (`app/chamados/models.py:2965`, `:3369`).

- **Semântica conversa: PROCEDENTE a divergência de cobertura.** A porta escreve dia civil (`app/chamados/models.py:2771`), o adaptador retorna pedido/gravado para conversa (`app/chamados/services/fio.py:1500`) e o contrato compara esse valor (`:1080`). Ambos os contadores excluem todas as conversas (`:1048`, `:1112`), portanto não verificam essa porta. O discriminador protege alguns leitores: C6 retorna ausência de turno para conversa (`app/chamados/juizes.py:336`); E4-c aceita conversa sem comparar dia (`:1997`). Permanecem leitores que usam dia civil como dia de competência/turno: `app/chamados/models.py:364`, `:390`; lavra usa `expira_em(p.data_evento)` (`app/chamados/veredito_lavrado.py:229`), embora a documentação ainda associe conversa a NULL (`:107`). A fila está declarada em `app/chamados/services/fio.py:1307`.

- **Contraexemplo concreto adicional: migrar pode alterar dia civil de conversa para dia do turno, passando no contrato.** `abrir_conversa(motivo='orfao_14h', tipo_resposta='hora')` é permitido pela assinatura e construção (`app/chamados/models.py:2764`, `:2771`, `:2962`). O migrar seleciona motivo/tipo/dia, sem excluir conversa (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:201`). Com um turno aberto iniciado D−1 cuja saída prevista é D, `_fonte_viva` identifica a entrada (`:105`–`:123`); `classificar` recebe D−1 do juiz e registra `novo` (`:157`, `:164`). Conversa fica sozinha no universo (`app/chamados/services/fio.py:1788`), e `fato_livre_para` a aceita (`app/chamados/models.py:2741`). A recusa de episódio ignora conversa (`app/chamados/services/fio.py:1419`); E4-c a aceita (`app/chamados/juizes.py:1997`); re-chaveamento escreve D−1 (`app/chamados/services/fio.py:989`, `:995`) e o contrato compara com o mesmo `ln['novo']` (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:391`). Depende das demais condições do apply, como competência aberta; ocorrência em produção [nao verificado].

- **`reuso_sem_rechave`:** universo = fatos com episódio, fora das vias excluídas, incluindo NULL e sem corte de horizonte (`app/chamados/services/fio.py:1048`). `universo` incrementa antes dos skips (`:1053`); juiz indisponível por exceção de domínio fica fora (`:1056`), dias iguais/None ficam fora (`:1058`); conta somente a recusa da porta real em dry (`:1060`). NULL com juiz D conta porque o veto `[None,D]` retorna desconhecida (`:1234`), recusada antes da porta (`:1022`).

- **`sem_dia_na_saida`:** universo = fatos com/sem episódio e com/sem dia, fora das vias excluídas (`app/chamados/services/fio.py:1112`); `universo` incrementa antes dos skips (`:1117`). Episódio sem dia vira `dia=None` e conta (`:1120`); ponte entre dias fica fora (`:1122`); usa o predicado real (`:1124`). São contadores do estado atual, não acumuladores de recusas ocorridas (`:1062`, `:1126`).

- **Custo:** o predicado do contrato tem zero consulta (`app/chamados/services/fio.py:1080`). Os contadores têm queryset base e consulta por identidade inédita `(batida,colaborador)` ao carregar batida e consultar T1 (`:1392`, `:1396`, `:1397`; `app/ponto/turnos.py:2925`). Traduzir None pode consultar dono e universo apurável (`app/chamados/services/fio.py:1429`, `:1434`). Acervo sem episódio percorre vizinhos com cache (`:1511`, `:1547`, `:1563`). O dry do reuso pode executar travas, saves e trilha da porta real, depois desfazer (`:1028`, `:1030`, `:1031`, `:969`, `:995`, `:998`); o caso NULL é recusado antes disso (`:1022`). Quantidade e latência de produção [nao verificado].

- **Leitores de produção dos contadores:** somente o DRY do backfill foi localizado: `app/chamados/management/commands/backfill_decisoes_pergunta.py:540` e `:546`, impressões em `:541` e `:547`. Não há monitoramento automático demonstrado pelo dado fornecido [nao verificado].

- **Juiz novo/derivação paralela introduzidos pelo contrato: NÃO encontrados.** O predicado apenas compara valor e recusa (`app/chamados/services/fio.py:1080`, `:1093`). A geometria continua delegada ao juiz T1 (`:1397`; `app/ponto/turnos.py:2925`). **Campo com dois significados: PRESENTE**, demonstrado pelas portas e pelo caminho de migração acima (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:989`).

## 08/11 16:58 · sessao 51a0c76b074f · - · - · origem=Ronald (tmux)

Item 3 — **NÃO PROCEDENTE como matriz completa e comprovada.**

- **A5:** a prova citada espera `ReusoSemRechave` por colisão; não percorre o `FatoEmOutroDia` descrito na matriz. `app/chamados/tests/test_e1_v13.py:136`, `:143`, `:146`; ramo alegado: `app/chamados/models.py:2873`.
- **A6:** a prova exclui a retratada do universo e retorna K, em vez de eleger retratada e levantar exceção. `app/chamados/tests/test_e1_v15b.py:128`, `:136`, `:139`, `:140`. A exclusão real ocorre em `app/chamados/services/fio.py:1684`.
- **A12:** o teste chama `resolvedora_valida` diretamente; não passa pela porta e seu ramo de recusa. `app/chamados/tests/test_e1_v15b.py:146`, `:153`; ramo real: `app/chamados/models.py:2921`.
- **G6:** `test_MORDE_dry_nao_escreve` verifica DRY sem injetar exceção de grupo. `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:136`.
- **G7:** o teste citado importa `_grupos_fora` do **backfill**, não exercita `migrar` nem duas linhas colidindo durante apply. `app/chamados/tests/test_e1_v13d_exportada_fora.py:115`, `:118`, `:124`.
- **G8:** S4 verifica texto de DRY e três perguntas de colaboradores diferentes; não prova rollback de apply em grupo com múltiplos membros. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:300`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:53`, `:63`, `:66`, `:86`, `:91`. O código efetivo mantém contrato dentro do atomic do grupo. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:365`, `:389`.
- Caminhos de classificação ausentes: absorção/sem fonte/materializada ambígua; saídas coerentes/sem turno da fonte viva; materializada cujo dia gravado não é a saída; juiz divergente da janela; `orfao_14h` materializada fora do aval. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`, `:97`, `:155`, `:162`, `:165`.
- `rechavear_no_reuso` também possui caminhos DRY com execução real e rollback, ausentes da matriz; A8/A9 só resumem seu uso pela abertura. `app/chamados/services/fio.py:1020`, `:1025`, `:1028`.

Item 6 — **PROCEDENTE quanto a mutar o fonte real, com ressalvas de força probatória.**

O arnês lê o arquivo real, extrai a função por AST, recompila com uma substituição e instala no módulo/classe usados pela produção; não mantém uma implementação paralela escrita à mão. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:319`, `:330`, `:344`, `:349`.

| Classe exigida | Mordida implementada |
|---|---|
| Remover helper/porta num ramo | M1/M1b mutam `_nascer`; cenário deve terminar em `SemDiaNaSaida`. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:495` |
| Helper não atribui/persistência omitida | M2a muta helper; M2b retira campo do `update_fields`; M2c retira releitura. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:502` |
| Juiz devolve data civil | M3 muta `_dia_do_fato`; caso noturno exige dia inicial. O contrato não detecta juiz e campo errados juntos; o cenário pela regra detecta. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:468`, `:514` |
| NULL com identidade resolvível | M4 repõe guarda antiga; M4b também desliga contrato para evidenciar retorno NULL. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:519` |
| Reusar outro episódio | M5 muda eleição real; cenário compara criação, PK, episódio, dia e integridade da linha anterior. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:447`, `:530` |
| Duas canônicas | M6 força `ex=None`; qualquer exceção ou resultado não conforme mata o mutante, sem exigir motivo específico. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:459`, `:483`, `:536`, `:552` |

M6 pode morrer pelo índice no `save(force_insert=True)`, sem chegar a produzir duas canônicas; prova rejeição da mutação, não correção de acervo previamente duplicado. `app/chamados/models.py:2965`; `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:536`, `:553`. Execução atual dos testes de banco e resultados GREEN/RED: **[nao verificado]**.

**Catracas com pontos cegos demonstrados somente em memória, sem criar arquivo:** o detector de abrir aceita retirar o `with transaction.atomic()` de `_sob_contrato`; o detector de migrar aceita pôr o contrato dentro de `if False`. Ambos retornaram `[]`, assim como o fonte real. A primeira função só compara instruções irmãs/ordem; a segunda procura chamada posterior em qualquer descendente e o handler. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:109`, `:116`, `:138`, `:142`. A alegação de verificar atomic e incondicionalidade excede o que essas catracas verificam. `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:14`.

## 08/11 16:57 · sessao df4592845df5 · - · - · origem=Ronald (tmux)

2. **PROCEDENTE para saídas com sucesso das duas portas e linhas migradas.** `abrir` e `abrir_conversa` só retornam o resultado de `_sob_contrato`; este chama `_nascer`, executa `contrato_de_saida` dentro do atomic e só depois retorna (`app/chamados/models.py:2759`, `:2771`, `:2789`). Os dois retornos de `_nascer` entregam `_dia_fato` (`app/chamados/models.py:2956`, `:2977`). As recusas levantam exceção e não retornam linha; o `except` final só cobre a trilha de emissão (`app/chamados/models.py:2908`, `:2922`, `:2969`).

A instância de reuso é relida depois de `rechavear_no_reuso`; o nascimento persiste com `save(force_insert=True)`; `save` possui atomic próprio (`app/chamados/models.py:2911`, `:2965`, `:3368`). No apply, cada resultado `ok` passa pelo contrato com `objects.get(pk=...)`; veto, recusa e exceções são contabilizados como recusas, não como migradas (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`, `:389`, `:392`, `:399`). Não encontrei caminho real nessas portas que dê sucesso com instância datada diferentemente do banco; comportamento executado em banco permanece **[nao verificado]**.

7. **Ordem global de trava: NÃO PROCEDENTE.** Há sequência real contrária ao contrato por transação:

- `_nascer` toma Colaborador; no reuso que troca dia, `rechavear_data_evento` toma Colaborador → Disputa → Pergunta (`app/chamados/models.py:2811`; `app/chamados/services/fio.py:1026`, `:969`).
- Depois, se o destino pedido difere, `_nascer` chama `_costura_nao_mata_o_fio`, que toma Chamado novo enquanto a Pergunta segue travada (`app/chamados/models.py:2917`, `:2929`; `app/chamados/services/fio.py:768`).
- A ordem declarada é Colaborador → Disputa → Chamado → Pergunta para a transação inteira (`app/chamados/models.py:2604`, `:2613`). Deadlock ocorrido em produção: **[nao verificado]**.

O laço do migrar também toma C/D/P por linha, podendo tomar a Disputa da segunda linha depois da Pergunta da primeira; a trava do grupo não é adquirida em um único conjunto ordenado (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:370`, `:373`).

`savepoint=False` em `_nascer` não apresenta, por si, uma fuga de IntegrityError capturado: erros dos saves não são capturados no corpo; escapam ao atomic de `_sob_contrato`, que desfaz o ato (`app/chamados/models.py:2807`, `:2955`, `:2965`, `:2789`). A trilha possui savepoint próprio e engole a falha fora dele (`app/core/observ.py:105`, `:133`); o código explica explicitamente o risco de rollback silencioso sem esse savepoint (`app/core/observ.py:124`). Execução dessa falha: **[nao verificado]**.

8. **Bloqueantes centrais do laudo anterior curados no código; itens de fila permanecem.**

Contra-exemplo NULL: a linha entra pelo episódio em `do_fato`, ganha o dia do juiz no universo e pode ser a canônica (`app/chamados/models.py:2695`; `app/chamados/services/fio.py:1679`, `:1682`; `app/chamados/models.py:2885`). Agora `None != _dia_fato` entra no rechaveamento; o veto lê `[None, dia]`, retorna recusa e `_nascer` levanta `ReusoSemRechave` antes da costura (`app/chamados/models.py:2904`; `app/chamados/services/fio.py:1020`; `app/chamados/models.py:2908`, `:2912`). Se a porta mentir e responder sucesso sem escrever, há releitura e contrato de saída (`app/chamados/models.py:2911`, `:2791`). A linha NULL deixou de ser excluída de `reuso_sem_rechave`, e `sem_dia_na_saida` também a examina (`app/chamados/services/fio.py:1048`, `:1112`, `:1124`).

Permanecem:

- Recusa sem captura nos emissores e chamadas API/triagem (`app/chamados/services/disputa_emissao.py:444`, `:834`, `:670`, `:682`; `app/api/views_core.py:794`; `app/ponto/services/triagem_batida.py:734`). HTTP 500 efetivamente observado: **[nao verificado]**.
- `rechavear_data_evento` continua sem veto próprio; o reuso o compõe externamente (`app/chamados/services/fio.py:963`, `:1020`).
- Contador dry continua executando a porta real sob atomic e adquirindo travas (`app/chamados/services/fio.py:1029`, `:969`).
- Destino `None` continua sem reapontamento no reuso (`app/chamados/models.py:2917`).
- Regex limitado continua; lista variável e migrations seguem fora da catraca declarada (`app/chamados/tests/test_contract_pergunta_fato.py:12`; `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:15`, `:43`).

## 08/11 16:46 · sessao 68935ce718a0 · PERGUNTA-SEM-SELO · leitura · origem=Code

As linhas atuais dos 28 sítios coincidem com as do anexo; os endereços estão na primeira coluna. “Nenhum” significa que não encontrei juiz registrado para **a pergunta própria do sítio**, nos catálogos pedidos. (`app/core/juizes.py:170`; `app/core/juizes.py:706`; `app/core/juizes.py:1025`; `app/core/juizes.py:1173`; `app/core/juizes.py:1285`; `app/docs/JUIZES.md:20`.)

**Base B**, usada nos exemplos: fixture hipotética com `col01` ativo, operacional, empresa/posto ativos; vínculo 5x2 vigente desde 01/11/2026, 08:00–17:00 sem pausa; D=06/11/2026; célula D `trabalha=True`, geradora desse vínculo e DNA 08:00–17:00 igual ao template; sem ausência, férias, demissão, substituição ou outras batidas além das declaradas em cada linha. Horários em −03:00. Cadastro proposto, execução e ocorrência em produção: **[não verificado]**. As respostas abaixo são deduzidas do código citado.

Na coluna de caso, **“— sem juiz”** significa que falta uma resposta autoritativa registrada da mesma pergunta para comparar; não entra na contagem de “sem divergência”.

| Sítio nesta árvore — mesma linha no anexo | Pergunta que responde por conta própria | Juiz registrado que responde essa pergunta | CASO QUE MORDE: entrada e resposta de cada um | Onde o RED moraria |
|---|---|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | A batida retroativa está com tipo invertido? (`app/chamados/management/commands/auditar_disputas.py:120`) | `decidir_tipo_estrito`, `app/ponto/turnos.py:2395`; registro: `app/core/juizes.py:198`. | **Não achei divergência.** O sítio chama exatamente o juiz, com os mesmos marcos do vínculo, timestamp local e tipo de cada batida examinada. O juiz é puro; não carrega outro universo de batidas. (`app/chamados/management/commands/auditar_disputas.py:116`; `app/chamados/management/commands/auditar_disputas.py:119`; `app/ponto/turnos.py:2395`) | `app/chamados/tests/test_auditar_disputas_flip_pelo_juiz.py:21` |
| `app/chamados/services/explicador.py:40` | Existe vizinha nos ±20 minutos do slot, e seu tipo explica a falta de prova? (`app/chamados/services/explicador.py:34`; `app/chamados/services/explicador.py:61`) | **nenhum** para esse diagnóstico; os registros de vida da pergunta/fio respondem outras perguntas. (`app/core/juizes.py:266`; `app/core/juizes.py:275`) | — sem juiz. A vizinha crua pode sustentar o diagnóstico “batida existe, mas o juiz ainda não a reconhece”; isso não fornece duas respostas registradas da mesma pergunta. (`app/chamados/services/explicador.py:65`) | `app/chamados/tests/test_explicador_disputa.py:65` |
| `app/chamados/services/explicador.py:53` | Houve marcação na janela estendida que justifique encaminhar o problema para cadastro? (`app/chamados/services/explicador.py:56`; `app/chamados/services/explicador.py:98`) | **nenhum** para causa/dono desse diagnóstico. (`app/core/juizes.py:260`; `app/docs/JUIZES.md:38`) | — sem juiz. A existência crua determina `CADASTRO` e “corrigir a escala”. (`app/chamados/services/explicador.py:58`; `app/chamados/services/explicador.py:100`) | `app/chamados/tests/test_explicador_disputa.py:89` |
| `app/chamados/services/explicador.py:132` | Quais marcas alimentam os diagnósticos em lote? (`app/chamados/services/explicador.py:133`) | **nenhum** para diagnóstico/dono. (`app/core/juizes.py:260`; `app/docs/JUIZES.md:38`) | — sem juiz nessa pergunta. **O fechamento é uma subpergunta distinta, sem divergência encontrada:** `pode = not fio_vivo(ps)` chama o juiz registrado com o mesmo universo `ps`; `_bts` não participa desse booleano. (`app/chamados/services/explicador.py:137`; `app/core/juizes.py:275`) | `app/chamados/tests/test_explicador_disputa.py:65` |
| `app/chamados/services/materializacao.py:94` | A vizinha anterior, em 12 horas, tem o mesmo tipo e barra o plantio? (`app/chamados/services/materializacao.py:99`) | **nenhum** para adjacência/guarda de plantio; o juiz de flip responde outra pergunta. (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`) | — sem juiz. (`app/chamados/services/materializacao.py:85`) | `app/chamados/tests/test_guarda_paridade_materializador.py:59` |
| `app/chamados/services/materializacao.py:100` | A vizinha posterior, em 12 horas, tem o mesmo tipo e barra o plantio? (`app/chamados/services/materializacao.py:105`) | **nenhum**, pela mesma diferença entre guarda de plantio e flip. (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`) | — sem juiz. (`app/chamados/services/materializacao.py:100`) | `app/chamados/tests/test_guarda_paridade_materializador.py:59` |
| `app/core/views.py:360` | O colaborador deve ficar fora dos ausentes de hoje por ter uma entrada nas últimas 24 horas? (`app/core/views.py:388`; `app/core/views.py:399`) | `classificar_falta`, `app/escala/servico_jornada.py:279`. O registro vincula expressamente `ausentes_hoje` à pergunta de julgamento do dia. (`app/core/juizes.py:709`; `app/core/juizes.py:741`) | **B; única E08:00, retratada D08:30; relógio D18:00. Sítio:** retira `col01` dos ausentes, pois a E crua fornece seu ID. **Juiz:** `('E', 'saida_sem_entrada', 'ENTROU')`: terminou o horário previsto e não há marca viva. (`app/core/views.py:360`; `app/core/views.py:399`; `app/escala/servico_jornada.py:377`; `app/ponto/turnos.py:1862`; `app/escala/servico_jornada.py:472`) | `app/core/tests/test_supervisor_turnos_pelo_juiz.py:60` |
| `app/core/views.py:793` | Houve atividade hoje para classificar a cobertura do posto? (`app/core/views.py:865`; `app/core/views.py:866`) | **nenhum** para esse agregado. `turno_aberto_de` responde turno aberto vivo agora. (`app/core/juizes.py:199`) | — sem juiz. `classificar_cobertura` recebe esse insumo, mas não consta como autoridade da pergunta nos registros pedidos. (`app/core/views.py:866`; `app/core/juizes.py:706`; `app/docs/JUIZES.md:20`) | `app/core/tests/test_painel_tv_turno_pelo_juiz.py:67` |
| `app/escala/services/furos_diarios.py:261` | Nunca bateu? Qual a última atividade? Está em `sumiu`? (`app/escala/services/furos_diarios.py:281`; `app/escala/services/furos_diarios.py:283`) | **nenhum**; o próprio registro diz “última batida, nunca bateu… deveria ler nenhum”. (`app/core/juizes.py:767`; `app/core/juizes.py:768`) | — sem juiz. (`app/core/juizes.py:768`) | `app/escala/tests/test_furos_diarios.py:61` |
| `app/escala/services/furos_diarios.py:267` | Houve batida na competência que faça o caso virar `escala_x_real`, em vez de `sumiu`? (`app/escala/services/furos_diarios.py:284`; `app/escala/services/furos_diarios.py:285`) | **nenhum** para essa distinção de atividade/classificação. (`app/core/juizes.py:765`; `app/core/juizes.py:768`; `app/docs/JUIZES.md:91`) | — sem juiz. (`app/escala/services/furos_diarios.py:284`) | `app/escala/tests/test_furos_diarios.py:61` |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | Qual membro do micropar pode ser retratado sem estragar o dia, consideradas as guardas e a simulação? (`app/ponto/management/commands/detectar_par_relampago.py:214`; `app/ponto/management/commands/detectar_par_relampago.py:244`) | **nenhum**; par relâmpago permanece em `SEM_JUIZ`. (`app/core/juizes.py:1187`) | — sem juiz. `par_completo` não decide qual membro retratar. (`app/core/juizes.py:228`; `app/ponto/management/commands/detectar_par_relampago.py:244`) | `app/ponto/tests/test_par_relampago_v3.py:48` |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | Quais marcas adjacentes formam micropar candidato à retirada sem piorar a grade? (`app/ponto/management/commands/detectar_par_relampago.py:298`; `app/ponto/management/commands/detectar_par_relampago.py:309`) | **nenhum** para candidatura/relâmpago. (`app/core/juizes.py:1187`) | — sem juiz na pergunta integral. **Na subpergunta “par completo?”, não achei divergência:** chama `par_completo` com a janela do próprio carregador do juiz. Isso não valida a seleção inicial por `aprovada=True`. (`app/ponto/management/commands/detectar_par_relampago.py:288`; `app/ponto/management/commands/detectar_par_relampago.py:291`; `app/ponto/management/commands/detectar_par_relampago.py:300`; `app/ponto/turnos.py:2200`) | `app/ponto/tests/test_par_relampago_pelo_turno.py:58` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | Quem tem marcas na janela e quantas marcas há em cada data local para alimentar a classificação? (`app/ponto/management/commands/emitir_furo_retroativo.py:55`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`) | **nenhum** para contagem/seleção; `classificar_dia` é consumidor desse insumo. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`; `app/core/juizes.py:170`; `app/docs/JUIZES.md:91`) | — sem juiz. A contagem tem efeito: o portão de furo lê `not batidas_no_dia`. (`app/ponto/supra_juiz.py:313`) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | Qual primeira marca estabelece o piso de operação? (`app/ponto/management/commands/emitir_furo_retroativo.py:75`; `app/ponto/management/commands/emitir_furo_retroativo.py:105`) | **nenhum** para esse piso de adesão. (`app/core/juizes.py:170`; `app/docs/JUIZES.md:20`) | — sem juiz. Esse piso alimenta `pre_adocao`. (`app/ponto/supra_juiz.py:288`) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/reconciliar_grade.py:84` | Quem bateu na janela e entra no conjunto inicial `batted`? (`app/ponto/management/commands/reconciliar_grade.py:86`) | **nenhum** para esse universo de medição. (`app/core/juizes.py:170`; `app/docs/JUIZES.md:20`) | — sem juiz. A seleção final também inclui `com_chamado` e `em_op`; a consulta não decide sozinha toda inclusão. (`app/ponto/management/commands/reconciliar_grade.py:106`) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:88` | Qual primeira marca estabelece o piso abaixo do qual ignorar dias? (`app/ponto/management/commands/reconciliar_grade.py:90`; `app/ponto/management/commands/reconciliar_grade.py:136`) | **nenhum** para esse piso de adesão. (`app/core/juizes.py:170`; `app/docs/JUIZES.md:20`) | — sem juiz. (`app/ponto/management/commands/reconciliar_grade.py:136`) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:118` | Faltou marco ou o dia está limpo a ponto de matar o fato de um chamado? (`app/ponto/management/commands/reconciliar_grade.py:151`; `app/ponto/management/commands/reconciliar_grade.py:266`) | `classificar_falta`, `app/escala/servico_jornada.py:279`; registro: `app/core/juizes.py:180`. | **B; E08:00/S17:00, ambas retratadas D17:30; relógio D+1 12:00; `GRADE_POR_TURNO=True`; `ref=D+1`, `dias=2`; chamado vivo `batida_ausente` apenas de D. Sítio:** as duas marcas ocupam a grade, D entra em `avaliado_limpo` e o chamado recebe plano `RETRACT`. **Juiz:** `('E', 'saida_sem_entrada', 'ENTROU')`. O guarda do juiz no CREATE não protege esse caminho de retratação. (`app/escala/utils.py:2107`; `app/escala/utils.py:1344`; `app/ponto/management/commands/reconciliar_grade.py:155`; `app/ponto/management/commands/reconciliar_grade.py:243`; `app/ponto/management/commands/reconciliar_grade.py:266`; `app/ponto/management/commands/reconciliar_grade.py:303`; `app/escala/servico_jornada.py:472`) | `app/ponto/tests/test_reconciliar_grade_coberto.py:70` |
| `app/ponto/management/commands/supra_juiz.py:113` | Quem tem marcas na janela e quantas existem em cada data local para alimentar a classificação? (`app/ponto/management/commands/supra_juiz.py:117`; `app/ponto/management/commands/supra_juiz.py:222`) | **nenhum** para contagem/seleção. (`app/core/juizes.py:170`; `app/docs/JUIZES.md:20`) | — sem juiz. O resultado chega a `batidas_no_dia` e afeta os portões de furo/furo parcial. (`app/ponto/management/commands/supra_juiz.py:229`; `app/ponto/supra_juiz.py:314`; `app/ponto/supra_juiz.py:437`) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |
| `app/ponto/management/commands/supra_juiz.py:132` | Qual primeira marca estabelece o piso de operação entregue ao classificador? (`app/ponto/management/commands/supra_juiz.py:135`; `app/ponto/management/commands/supra_juiz.py:227`) | **nenhum** para esse piso. (`app/core/juizes.py:170`; `app/docs/JUIZES.md:20`) | — sem juiz. O piso alimenta `pre_adocao`. (`app/ponto/supra_juiz.py:288`) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |
| `app/ponto/propositor.py:346` | O dia está completo o suficiente para vetar cobrança de marco faltante? Esse resultado é usado para calar turno aberto. (`app/ponto/propositor.py:325`; `app/chamados/reconciliador.py:112`) | `classificar_falta`, `app/escala/servico_jornada.py:279`; registro: `app/core/juizes.py:180`. O juiz usa o mesmo veto posicional. (`app/escala/servico_jornada.py:442`) | **B; E08:00/S17:00, ambas retratadas D17:30; relógio D+1 12:00. Sítio:** `True`, porque as marcas cruas completam os slots. **Juiz:** `('E', 'saida_sem_entrada', 'ENTROU')`, porque seu universo fica vazio e não entra no veto posicional. (`app/ponto/propositor.py:346`; `app/ponto/propositor.py:351`; `app/ponto/propositor.py:352`; `app/ponto/turnos.py:1862`; `app/escala/servico_jornada.py:440`; `app/escala/servico_jornada.py:472`) | `app/ponto/tests/test_clone_posicional_nao_cobra.py:61` |
| `app/ponto/services/flip_auto.py:35` | Quais marcas adjacentes têm mesmo tipo e distância inferior a 12 horas? (`app/ponto/services/flip_auto.py:43`) | **nenhum** para essa formação de pares; decidir flip por marcos é outra pergunta. (`app/core/juizes.py:198`) | — sem juiz. A leitura posterior de `batidas_do_dia_turno` fornece a contagem, sem refazer a adjacência inicial. (`app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`) | `app/ponto/tests/test_flip_auto_pergunta_ao_juiz.py:78` |
| `app/ponto/services/flip_auto.py:291` | O dia está perfeito antes/depois dos flips, permitindo decisão automática? (`app/ponto/services/flip_auto.py:303`; `app/ponto/services/flip_auto.py:305`) | **nenhum** registrado para “dia perfeito”. `dia_perfeito` recebe esse título na própria docstring, fora dos registros exigidos. (`app/ponto/services/simular_dia.py:1`; `app/core/juizes.py:195`; `app/docs/JUIZES.md:27`) | — sem juiz registrado dessa pergunta. O sítio chama `decidir_tipo_estrito` para os tipos, mas mantém `reais` inteira no julgamento da grade. (`app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:300`; `app/ponto/services/flip_auto.py:303`) | `app/ponto/tests/test_flip_auto.py:7` |
| `app/ponto/services/geofence.py:51` | Quantas das dez últimas marcas GPS caem no raio e sustentam divergência de vínculo? (`app/ponto/services/geofence.py:56`; `app/ponto/services/geofence.py:67`) | **nenhum** para esse agregado/limiar de vínculo. `classificar_posicao` está registrado para uma posição individual. (`app/core/juizes.py:1178`; `app/ponto/services/geofence.py:95`) | — sem juiz do agregado. O subpredicado individual “dentro” coincide geometricamente: distância ≤ raio efetivo. Isso não fornece autoridade registrada para selecionar as dez marcas ou aplicar 80%. (`app/ponto/services/geofence.py:60`; `app/ponto/services/geofence.py:67`; `app/ponto/services/geofence.py:114`; `app/ponto/services/geofence.py:184`) | `app/ponto/tests/test_reconciliar_vinculo.py:49` |
| `app/ponto/services/veredito_celula.py:215` | O dia já está completo? Qual combinação de flips/plantios o deixa perfeito? (`app/ponto/services/veredito_celula.py:223`; `app/ponto/services/veredito_celula.py:271`) | **nenhum** registrado para essa pergunta composta. `dia_perfeito` existe no módulo de simulação; o registro de casamento de marcos responde outra pergunta. (`app/ponto/services/simular_dia.py:11`; `app/core/juizes.py:200`; `app/docs/JUIZES.md:27`) | — sem juiz registrado dessa pergunta. Já chama `grade_dia`/`dia_perfeito`; compará-lo às mesmas funções com a mesma lista não produziria divergência. (`app/ponto/services/veredito_celula.py:216`; `app/ponto/services/veredito_celula.py:218`; `app/ponto/services/veredito_celula.py:219`) | `app/ponto/tests/test_veredito_celula.py:45` |
| `app/ponto/services/vinculo_na_lista.py:49` | A evidência GPS aponta vínculo divergente, coordenada errada, itinerância ou posto não cadastrado? (`app/ponto/services/vinculo_na_lista.py:86`) | **nenhum** nos registros pedidos. O decisor chamado é `classificar_evidencia`; o juiz registrado de geofence responde uma posição individual. (`app/ponto/services/vinculo_evidencia.py:48`; `app/core/juizes.py:1178`) | — sem juiz registrado dessa pergunta. O sítio já entrega os números coletados a `classificar_evidencia`. (`app/ponto/services/vinculo_na_lista.py:86`) | `app/ponto/tests/test_c2_vinculo_vira_lista.py:63` |
| `app/ponto/utils.py:186` | Há turno aberto que impede oferecer o colaborador para cobertura? (`app/ponto/utils.py:183`; `app/ponto/utils.py:192`) | `turno_aberto_de`, `app/ponto/turnos.py:2281`; registro: `app/core/juizes.py:199`. | **B; única E08:00, retratada D08:30; relógio D09:00; cobertura [D09:00,D17:00). Sítio:** exclui `col01` por suposto turno aberto. **Juiz:** `None`; a única marca é excluída antes do pareamento. (`app/ponto/utils.py:187`; `app/ponto/utils.py:191`; `app/ponto/utils.py:192`; `app/ponto/turnos.py:2219`; `app/ponto/turnos.py:2222`; `app/ponto/turnos.py:2290`) | `app/ponto/tests/test_motor_substituicao.py:208` |
| `app/ponto/utils.py:244` | A próxima batida é entrada ou saída? (`app/ponto/utils.py:240`) | `proximo_tipo_de`, `app/ponto/turnos.py:2448`; registro: `app/core/juizes.py:197`. | **B, substituindo o ciclo por intermitente; única E08:00, retratada D08:30; relógio D09:00. Sítio:** `S`, pela última marca crua. **Juiz:** `E`; no ramo intermitente não encontra marca viva nas últimas 14 horas. (`app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/ponto/turnos.py:2464`; `app/ponto/turnos.py:2467`; `app/ponto/turnos.py:2471`) | `app/ponto/tests/test_proxima_batida.py:23` |
| `app/ponto/views.py:1598` | Qual entrada pertence ao turno D para associar à justificativa? (`app/ponto/views.py:1601`; `app/ponto/views.py:1637`) | `turnos_do_colab`, `app/ponto/turnos.py:1848`, para geometria e pertença ao turno; registro: `app/core/juizes.py:196`; `app/docs/JUIZES.md:20`. | **B; única batida #1 E08:00, retratada D08:30; POST D09:00 com `data_turno=2026-11-06`, sem justificativa anterior. Sítio:** escolhe #1 para `batida_entrada`. **Juiz:** lista de turnos vazia, sem entrada de D; #1 é excluída na consulta. (`app/ponto/views.py:1598`; `app/ponto/views.py:1602`; `app/ponto/views.py:1637`; `app/ponto/turnos.py:1861`; `app/ponto/turnos.py:1862`; `app/ponto/turnos.py:1913`) | `app/ponto/tests/test_vinculo_justificativa_cross_day.py:30` |

Contagem das 28 linhas referenciadas: **6 com caso que morde; 1 sem divergência; 0 sumidos; 21 sem juiz**. As igualdades de subperguntas anotadas em `explicador:132`, `detectar_par_relampago:290` e `geofence:51` não foram contadas novamente. (`app/chamados/services/explicador.py:137`; `app/ponto/management/commands/detectar_par_relampago.py:300`; `app/ponto/services/geofence.py:60`.)

## 08/11 16:45 · sessao 75defa6f7a41 · V3-BATERIA-DE-CASOS-PELA-REGRA · leitura · origem=Code

A bateria existe, mas **não achei prova completa de espelho = fechamento = exportação = DiaPago**. F11c alcança fechamento e DiaPago; o teste da porta do export declara que terminou na função de comparação, sem atravessar o TXT. (`app/ponto/tests/test_o280_bateria_da_lei.py:1714`; `app/folha/tests/test_porta_do_export.py:224`)

Leitura estática, sem executar motor ou testes e sem alterar arquivos. Resultado verde desta cópia, sobrevivência executada dos mutantes e estado de produção: **[não verificado]**. [nao verificado]

**1. INVENTÁRIO**

Nesta cópia, contei **138 IDs em `CASOS`** — 117 classificados como `mes`, oito como `periodo` e 13 como `fora` —, **165 métodos de teste na O280** e **52 na lavratura**. IDs, métodos, propriedades e releituras não são a mesma contagem. (`app/ponto/tests/test_o280_bateria_da_lei.py:278`; `app/ponto/tests/test_o280_bateria_da_lei.py:1067`; `app/ponto/tests/test_o280_bateria_da_lei.py:1073`; `app/ponto/tests/test_bateria_lavratura.py:169`)

O censo abaixo distingue **oráculo positivo explícito na apuração** de **teste da rubrica emitida no TXT**. Não contei gêmeos “MEDIDOS” como resposta pela regra; o próprio arquivo os define como números do motor atual. Zeros e propriedades de conservação também não foram contados como casos positivos. (`app/ponto/tests/test_o280_bateria_da_lei.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:2874`)

| Rubrica efetivamente emitida | Origem da emissão | Casos positivos explícitos relacionados na bateria | Prova até o TXT nos dois arquivos |
|---|---|---:|---:|
| `hora_extra_diurna` — HE50 | `app/folha/export.py:441` | **0**; o positivo direto encontrado é gêmeo medido | **0** |
| `hora_extra_100_diurna` — HE100 normal | `app/folha/export.py:421`; `app/folha/export.py:442` | **0** com faixa positiva decidida pela norma | **0** |
| `hora_extra_feriado_diurna` | `app/folha/export.py:443` | **8 IDs / 10 métodos** | **0** |
| `folga_trabalhada_diurna` | `app/folha/export.py:446` | **5 cenários / 7 métodos**, principalmente minutos de FT do motor | **0** |
| `adicional_noturno` | `app/folha/export.py:447` | **12 IDs / 16 métodos** | **0** |
| `intrajornada_diurna` | `app/folha/export.py:448` | **5 IDs / 5 métodos**; há pino e dimensionamento parcial pendente | **0** |
| `atraso_diurno` — atraso **+ antecipada** | `app/folha/export.py:383`; `app/folha/export.py:451` | **27 IDs / 29 métodos** relacionados às duas parcelas | **0** |
| `falta_diurna` — dias decididos | `app/folha/export.py:732` | **0 da rubrica final**; dois IDs afirmam montantes de falta total | **0** |

A ausência de teste até o TXT decorre do alcance das costuras: motor/releitura na O280 e cartório na lavratura; F11c acrescenta fechamento e DiaPago, sem conferir emissão. (`app/ponto/tests/test_o280_bateria_da_lei.py:128`; `app/ponto/tests/test_o280_bateria_da_lei.py:3549`; `app/ponto/tests/test_o280_bateria_da_lei.py:1745`; `app/ponto/tests/test_bateria_lavratura.py:134`)

Não confundi os eventos possíveis do cadastro com os emitidos: **HE noturna não tem linha própria**, e **DSR não é evento de importação**. Código FOPAG vigente por empresa: **[não verificado]**; é cadastro. (`app/folha/export.py:424`; `app/folha/export.py:453`; `app/folha/models.py:47`)

Os nomes e as normas dos casos positivos são estes. A repetição v2/v3 do mesmo ID não acrescenta outro caso, conforme a própria costura declara. (`app/ponto/tests/test_o280_bateria_da_lei.py:3541`)

| ID / cobertura | Nome do teste | Norma que cita / localização | [nao verificado]
|---|---|---|
| F10 — feriado | `test_F10_feriado_em_dia_de_trabalho_dobra_a_jornada_INTEIRA`; `test_v3_F10_a_dobra_vem_do_juiz_IMPORTADO` | Súmula 146. `app/ponto/tests/test_o280_bateria_da_lei.py:1480`; `app/ponto/tests/test_o280_bateria_da_lei.py:4458` |
| F10b — feriado | `test_F10b_o_dia_em_dobro_nao_volta_ao_calculo_normal_de_HE`; `test_v3_F10b_a_dobra_SUBSTITUI_a_faixa_do_excedente` | Súmula 146; não pagar novamente o excedente. `app/ponto/tests/test_o280_bateria_da_lei.py:1496`; `app/ponto/tests/test_o280_bateria_da_lei.py:4475` |
| F10c — feriado | `test_F10c_feriado_previsto_na_escala_dobra_igual_e_o_ramo_morto_nao_fala` | L-086, cláusula antiga de feriado revogada; Súmula 146. `app/ponto/tests/test_o280_bateria_da_lei.py:1510` |
| F11 — feriado e noturno | `test_F11_feriado_no_turno_partido_leva_o_noturno_para_HE100_noturna` | Súmulas 146/264; CLT art.73 §2. `app/ponto/tests/test_o280_bateria_da_lei.py:1654` |
| F11r — feriado e noturno | `test_F11r_referencia_o_12x36_ja_passa_a_dobra_pelo_split` | Súmulas 146/264; CLT art.73. O ciclo da fixture é 6x1. `app/ponto/tests/test_o280_bateria_da_lei.py:1675` |
| F11b — feriado e noturno | `test_F11b_feriado_no_comercial_que_entra_na_janela_noturna` | Súmulas 146/264; CLT art.73 §2. `app/ponto/tests/test_o280_bateria_da_lei.py:1697` |
| F11c — feriado e noturno | `test_F11c_promocao_ft_feriado_leva_a_parte_noturna_para_a_folha` | Corte BO-B(3), 23/10; Súmula 264. `app/ponto/tests/test_o280_bateria_da_lei.py:1714` |
| F11d — feriado e noturno | `test_F11d_partido_de_dois_pares_todo_noturno_do_feriado_e_HE100_noturna` | Súmulas 146/264/60 II; CLT art.73 §5. `app/ponto/tests/test_o280_bateria_da_lei.py:1769` |
| F3 — FT | `test_F3_feriado_em_dia_de_folga_e_folga_trabalhada` | Súmula 146; cortes 19/09 e 30/09. `app/ponto/tests/test_o280_bateria_da_lei.py:1386` |
| F3b — FT | `test_F3b_feriado_em_folga_no_ramo_que_dobra_deixa_o_campo_sem_leitor`; `test_v3_F3b_CURA_o_periodo_de_FOLGA_nao_guarda_dobra_sem_leitor` | Súmula 146; corte 19/09, FT fora da HE. `app/ponto/tests/test_o280_bateria_da_lei.py:1783`; `app/ponto/tests/test_o280_bateria_da_lei.py:4514` |
| F8 — FT | `test_F8_dobra_real_com_lastro` | Súmula 146; cortes 19/09 e 30/09. `app/ponto/tests/test_o280_bateria_da_lei.py:2041` |
| B-O263FT — FT | `test_B_O263FT_folga_trabalhada_nao_se_recorta`; `test_v3_B_O263FT_na_folga_nao_ha_marco_e_nada_pousa` | O263-FT; L-084. `app/ponto/tests/test_o280_bateria_da_lei.py:2702`; `app/ponto/tests/test_o280_bateria_da_lei.py:4092` |
| Sem ID — FT | `test_v3_a_folga_trabalhada_segue_a_familia_1` | Corte 19/09, FT fora da HE. `app/ponto/tests/test_o280_bateria_da_lei.py:4540` |
| F2 — noturno | `test_F2_borda_utc_turno_21_03`; `test_v3_F2_a_janela_do_art73_pelo_juiz_IMPORTADO` | CLT art.73 §§1/2; Súmula 60; L-085. `app/ponto/tests/test_o280_bateria_da_lei.py:1330`; `app/ponto/tests/test_o280_bateria_da_lei.py:4309` |
| F4 — noturno | `test_F4_feriado_em_12x36_noturno_e_hora_simples` | CLT art.59-A; art.73; parecer registrado de 08/09. `app/ponto/tests/test_o280_bateria_da_lei.py:1399` |
| F4b — noturno | `test_F4b_a_prorrogacao_pos_05h_e_cadastro_e_nao_default`; `test_v3_F4b_a_prorrogacao_vem_do_CADASTRO_e_NAO_de_default` | CLT art.73 §5; Súmula 60 II; art.59-A, parágrafo único. `app/ponto/tests/test_o280_bateria_da_lei.py:1420`; `app/ponto/tests/test_o280_bateria_da_lei.py:4338` |
| F4c — noturno | `test_F4c_a_hora_reduzida_afastada_muda_o_fator_nao_o_relogio`; `test_v3_F4c_a_cl38d_muda_o_FATOR_e_nao_o_relogio` | CCT Vigilantes Londrina, cl.38-d. `app/ponto/tests/test_o280_bateria_da_lei.py:1446`; `app/ponto/tests/test_o280_bateria_da_lei.py:4358` |
| F10i — noturno | `test_F10i_GUARDA_sem_trabalho_em_00_05_a_cauda_nao_prorroga` | Súmula 60 II. `app/ponto/tests/test_o280_bateria_da_lei.py:1628` |
| B-O263 — noturno | `test_B_O263_intervalo_batido_nao_recebe_adicional_noturno`; `test_v3_B_O263_o_intervalo_gozado_sai_do_adicional` | CLT art.73 §2; O263. `app/ponto/tests/test_o280_bateria_da_lei.py:2689`; `app/ponto/tests/test_o280_bateria_da_lei.py:4321` |
| I26-H1 — noturno | `test_i26_H1_a_janela_noturna_do_PAR_e_o_intervalo_gozado` | CLT arts.73 §2 e 4. `app/ponto/tests/test_o280_bateria_da_lei.py:5966` |
| B-O272b — intra | `test_B_O272b_com_a_janela_LIGADA_o_recorte_nao_desconta_duas_vezes` | O272; L-097; CLT art.71. `app/ponto/tests/test_o280_bateria_da_lei.py:2552` |
| B-SOBRA — intra | `test_B_SOBRA_batida_no_raio_da_ponta_nao_vira_marco_de_intervalo` | BUG-SOBRA-TIPO-TROCADO; CLT art.71 §§1/4. `app/ponto/tests/test_o280_bateria_da_lei.py:2760` |
| F7d — intra | `test_v3_F7d_a_pausa_REAL_fora_do_marco_e_intrajornada` | CLT art.71 §§1/2/4; Súmula 437. `app/ponto/tests/test_o280_bateria_da_lei.py:3738` |
| F7e — intra, pinado | `test_v3_F7e_PINADO_o_descanso_que_HOUVE_nao_se_indeniza` | CLT art.71 §§1/2/4; Súmula 437; L-099. O próprio corpo registra pergunta sobre a geometria. `app/ponto/tests/test_o280_bateria_da_lei.py:3803` |
| B-P4b — intra | `test_v3_B_P4b_o_par_REPETIDO_paga_UM_turno_UMA_vez` | CLT art.4; L-110. `app/ponto/tests/test_o280_bateria_da_lei.py:5253` |
| F-FOTO — falta total | `test_F_FOTO_folga_avulsa_no_mes_nao_apaga_o_folga_dia_semana` | L-023. `app/ponto/tests/test_o280_bateria_da_lei.py:2058` |
| F-FOTOb — falta total | `test_F_FOTOb_foto_parcial_no_12x36_a_fase_sai_da_foto_nao_da_ancora` | L-023 e L-022. `app/ponto/tests/test_o280_bateria_da_lei.py:2090` |

Os **27 IDs de pontualidade** estão abaixo. Eles não testam a composição final da linha `atraso_diurno`, que soma as duas parcelas. (`app/folha/export.py:383`)

| ID | Nome do teste | Norma citada / localização | [nao verificado]
|---|---|---|
| F5 | `test_F5_troca_de_vinculo_no_meio_da_competencia` | L-021; L-099; O262. `app/ponto/tests/test_o280_bateria_da_lei.py:1867` |
| A14b | `test_A14b_o_limiar_do_par_relampago_e_quem_zera_o_A14`; `test_v3_A14b_acima_do_limiar_o_TETO_le_o_trabalhado_da_v3` | Limiar do par relâmpago; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:2460`; `app/ponto/tests/test_o280_bateria_da_lei.py:4140` |
| B-O271a | `test_B_O271a_comercial_tem_pontualidade`; `test_v3_B_O271a_o_comercial_passa_pela_MESMA_regua` | O271; CLT art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:2714`; `app/ponto/tests/test_o280_bateria_da_lei.py:4036` |
| F9 | `test_F9_a_tolerancia_tem_dois_numeros_e_o_segundo_e_diario`; `test_v3_F9_a_tolerancia_e_o_teto_pelo_juiz_IMPORTADO` | Art.58 §1; Súmula 366; decisão da casa de dez por marcação. `app/ponto/tests/test_o280_bateria_da_lei.py:3294`; `app/ponto/tests/test_o280_bateria_da_lei.py:3989` |
| B-I27a e B-I27b | `test_v3_B_I27a_e_b_o_marco_e_do_dia_da_JORNADA_nao_do_dia_da_BATIDA` | L-085; art.58 §1; Súmula 366; L-093/L-084. `app/ponto/tests/test_o280_bateria_da_lei.py:4163` |
| B-I27e | `test_v3_B_I27d_e_e_o_turno_partido_que_atravessa_a_meia_noite` | L-085/L-084/L-093; art.58 §1. `app/ponto/tests/test_o280_bateria_da_lei.py:4205` |
| B-I27f | `test_v3_B_I27f_o_plantao_de_24h_MORDE_a_comparacao_que_nao_e_juiz` | O276; regra de bordas coincidentes. `app/ponto/tests/test_o280_bateria_da_lei.py:4230` |
| I26-A1 | `test_i26_A1_cabeca_longe_conta_a_TOTALIDADE` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5551` |
| I26-A4 | `test_i26_A4_cauda_antes_do_marco_conta_a_TOTALIDADE` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5587` |
| I26-B1 | `test_i26_B1_o_RED_do_O271_pelo_teto_da_L093` | L-084/L-093; art.58; Súmula 366. `app/ponto/tests/test_o280_bateria_da_lei.py:5624` |
| I26-B2 | `test_i26_B2_cauda_antes_do_marco_no_comercial` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5632` |
| I26-B3 | `test_i26_B3_a_fronteira_da_tolerancia_nos_DOIS_lados` | Art.58 §1; Súmula 366; tolerância dez da casa. `app/ponto/tests/test_o280_bateria_da_lei.py:5639` |
| I26-B5 | `test_i26_B5_uma_ponta_longe_cobra_mesmo_a_235` | L-084/L-093; art.58; Súmula 366. `app/ponto/tests/test_o280_bateria_da_lei.py:5673` |
| I26-D1 | `test_i26_D1_a_batida_no_hii_e_saida_PARA_o_intervalo` | O271; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5689` |
| I26-D4 | `test_i26_D4_a_fronteira_e_a_CONFORMIDADE_nunca_o_raio` | O271; conformidade dez da casa. `app/ponto/tests/test_o280_bateria_da_lei.py:5710` |
| I26-E1 | `test_i26_E1_o_RED_da_L084_uma_ponta_longe_COBRA` | L-084/L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5733` |
| I26-E2 | `test_i26_E2_dentro_dos_180_a_cura_de_26_09_nao_afrouxa` | L-084. `app/ponto/tests/test_o280_bateria_da_lei.py:5745` |
| I26-E5 | `test_i26_E4_E5_o_gate_do_T8_e_o_seu_PAR_DE_CONTROLE` | L-084/L-093. E5 é a metade positiva. `app/ponto/tests/test_o280_bateria_da_lei.py:5759` |
| I26-F1 | `test_i26_F1_cabeca_longe_no_noturno` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5780` |
| I26-F2 | `test_i26_F2_cauda_antes_do_marco_no_noturno` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5787` |
| I26-F3 | `test_i26_F3_as_DUAS_pontas_no_noturno_e_o_PAR_que_se_afirma` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5794` |
| I26-G1 | `test_i26_G1_a_saida_PARA_o_intervalo_nao_e_fim_de_turno` | O271; art.58 §1; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5857` |
| I26-G2 | `test_i26_G2_a_volta_do_intervalo_no_fragmento_de_depois` | O271; art.58 §1; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5878` |
| I26-G3 | `test_i26_G3_antecipada_REAL_com_o_intervalo_no_meio` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5895` |
| I26-G4 | `test_i26_G4_atraso_REAL_com_o_intervalo_no_meio` | Art.58 §1; Súmula 366; L-093. `app/ponto/tests/test_o280_bateria_da_lei.py:5911` |
| I26-G5 | `test_i26_G5_duracao_sem_janela_e_sem_volta_NAO_compra_o_gate` | L-084; CLT art.71 §1. `app/ponto/tests/test_o280_bateria_da_lei.py:5926` |

Há quatro limites materiais nesse inventário:

- **HE total não prova HE50/HE100 separadamente.** `test_i26_H2_acima_do_limite_a_hora_segue_devida_sem_teto` afirma o excesso e a soma das partes, recusando fixar o degrau sem lastro. (`app/ponto/tests/test_o280_bateria_da_lei.py:5991`)
- **F9 não prova cobrança legal após cinco minutos.** O corpo espera atraso zero às 08:07 e onze minutos às 08:11. A questão do anexo permanece questão de lei. (`app/ponto/tests/test_o280_bateria_da_lei.py:3351`; `app/ponto/tests/test_o280_bateria_da_lei.py:3355`; `app/docs/BACKLOG.md:1629`)
- **Intra parcial escrita não equivale a dimensionamento homologado.** F7d/F7e escrevem o suprimido; o backlog mantém o conflito art.71 pós-2017 × “INTEGRAL” pendente. (`app/ponto/tests/test_o280_bateria_da_lei.py:3760`; `app/ponto/tests/test_o280_bateria_da_lei.py:3806`; `app/docs/BACKLOG.md:1629`)
- **Falta total não prova `falta_diurna`.** F1/A1 constroem esperado pelo próprio previsto; a rubrica exportada exige ausência decidida cujo tipo desconta. (`app/ponto/tests/test_o280_bateria_da_lei.py:1316`; `app/ponto/tests/test_o280_bateria_da_lei.py:2140`; `app/docs/LEIS.md:87`)

**2. CASOS QUE FALTAM**

Os casos E abaixo completam a conferência **até a emissão**, lacuna dos dois arquivos. Os C completam a seleção pelo cadastro real: a fixture atual não declara regime e chama `get_motor` diretamente; flags passadas ao motor não provam Empresa → Aplicação → Sindicato. (`app/ponto/tests/test_o280_bateria_da_lei.py:1113`; `app/ponto/tests/test_o280_bateria_da_lei.py:146`; `app/docs/BACKLOG.md:1530`)

Cada linha descreve uma fixture proposta, não um colaborador de produção. Os resultados são referências de horas/minutos/dias; valor monetário sem salário-base: **[não verificado]**.

| id | cadastro — escala, regime, empresa/aplicação, sindicato, feriado | batidas — horário local | rubrica | valor devido pela REGRA, com aritmética aberta | norma |
|---|---|---|---|---|---|
| **E1** | 5x2, 08–17, pausa12–13; empresa CLT, sem aplicação, sindicato não aplicável; dia útil sem feriado; HE autorizada | E08 / S12 / E13 / S18 | HE50; intra | Trabalhado `240+300=540`; previsto `540−60=480`; excesso `540−480=60min` → **1h de HE50**, no piso legal. Pausa integral gozada → intra **0** | CLT art.59 §1; L-097. `app/docs/LEIS.md:115`; `app/ponto/tests/test_o280_bateria_da_lei.py:461` |
| **E2** | 5x2, 08–17, pausa12–13; CLT, sem aplicação; feriado cadastrado, previsto como trabalho, sem compensação | E08 / S12 / E13 / S17 | HE100 feriado **e ausência de HE100 normal** | `240+240=480min` → **8h de referência em feriado a100%**; parcela normal **0**. Não emitir novamente as mesmas8h como HE100 normal | Súmula146; L-086 vigente; conservação L-110. `app/ponto/tests/test_o280_bateria_da_lei.py:1483`; `app/docs/LEIS.md:118`; `app/docs/LEIS.md:136` |
| **E3** | 5x2, folga declarada no dia, escala certa; CLT, sem aplicação/sindicato aplicável; sem feriado | E08 / S12 / E13 / S17 | FT | `240+240=480min` → **8h de FT a100%**; nenhuma HE de feriado | Cortes19/09 e27/09, CLASSE3-FOLGA-100; L-084. `app/ponto/calculador/regras.py:377`; `app/ponto/calculador/regras.py:381` |
| **E4** | 12x36, plantão19–07, pausa01–02; **CLT declarado**, sem aplicação/sindicato aplicável; sem feriado | E19 / S01(+1) / E02(+1) / S07(+1) | Noturno | Janela estrita: `(22–01)+(02–05)=180+180=360min`; reduzida: `360÷52,5=6,857142h` → **6,86h** na precisão centesimal | CLT arts.73 §§1/2 e59-A, parágrafo único; L-035. `app/docs/LEIS.md:83`; `app/core/regua_cct.py:159` |
| **E5** | 5x2, 08–17, intervalo cadastrado60min, indenizável; CLT, sem aplicação/sindicato aplicável; sem feriado | E08 / S17; nenhuma pausa | Intra | Intervalo devido60, gozado0: `60−0=60min` → **1h de referência indenizada**, com acréscimo legal. Supressão total evita escolher entre os critérios divergentes de supressão parcial | CLT art.71 §4; L-031/L-032. `app/docs/LEIS.md:79`; `app/docs/LEIS.md:80` |
| **E6** | 5x2, 08–17, pausa12–13; CLT, sem aplicação/sindicato aplicável; sem feriado | E08:20 / S12 / E13 / S16:30 | Atraso + antecipada | Trabalhado `220+210=430`; falta temporal `480−430=50`; atraso20 + antecipada30 = **50min**, ou `50÷60=0,833333h`. As duas pontas superam ambas as tolerâncias em disputa | CLT art.58 §1; Súmula366; L-084/L-093; composição da rubrica. `app/docs/LEIS.md:116`; `app/docs/LEIS.md:111`; `app/folha/export.py:383` |
| **E7** | 5x2, 08–17, pausa12–13; CLT, sem aplicação/sindicato aplicável; sem feriado; **ausência aprovada de um dia, tipo descontante,480min** | Nenhuma | Falta | Decisão cobre um dia: **1dia** de falta; dimensão temporal `480÷60=8h`. Conferir também a data no registro11 | L-039; contrato do bloco de falta. `app/docs/LEIS.md:87`; `app/folha/export.py:215` |
| **C1** | 12x36,19–07, pausa01–02; empresa CCT, **aplicação vigente** Vigilantes Londrina PR000251/2026; redução afastada e pós5 desmarcado; sem feriado | E19 / S01(+1) / E02(+1) / S07(+1) | Noturno | `180+180=360min`; hora da cl.38-d tem60min: `360÷60=` **6h**. Adicional segue devido | CCT cl.38-d; L-036. `app/core/regua_cct.py:129`; `app/docs/LEIS.md:84` |
| **C2** | Mesma escala; empresa **CCT sem AplicacaoConvencao vigente**, sem sindicato selecionado; sem feriado | Mesmas de C1 | Noturno + fonte da regra | A lacuna não retira pós5: `(22–07)−60=480min`; `480÷52,5=` **9,142857h**, ou548,57min legais. Fonte deve acusar **sem aplicação** | L-035: exceção12x36 só com CLT declarado; corte registrado sobre lacuna. `app/docs/LEIS.md:83`; `app/core/regua_cct.py:192`; `app/core/regua_cct.py:204`; `app/core/regua_cct.py:371` |
| **C3** | Mesma escala; empresa CCT com **duas aplicações vigentes no mesmo nível**, sindicatos conflitantes; sem feriado | Mesmas de C1 | Noturno + fonte da regra | Não escolher pela inserção. Piso preservado: `480÷52,5=` **9,142857h**. Fonte acusa **empate**; inverter inserção mantém resultado | L-006/L-034; regra registrada de empate. `app/docs/LEIS.md:64`; `app/docs/LEIS.md:82`; `app/core/regua_cct.py:27`; `app/core/regua_cct.py:371` |
| **C4** | 6x1 noturno,19–07, pausa01–02; empresa CCT com aplicação SINEEPRES vigente; sem feriado | Mesmas de C1 | Noturno | **DUVIDA:** qual texto/cláusula vigente do SINEEPRES decide a prorrogação e a redução neste cadastro? Referência à cl.10 existe; texto integral não achei | L-035; referências cadastrais. `app/docs/LEIS.md:83`; `app/core/views_config.py:293`; `app/core/models.py:72` |
| **C5** | 12x36,07–19, pausa12–13; empresa CCT com aplicação e sindicato com `feriado_12x36_em_dobra=True`; feriado previs

[truncado em 20000 de 27006 caracteres]

## 08/11 16:42 · sessao bfe42fe62f74 · - · - · origem=Ronald (tmux)

**Correção da linha `app/ponto/propositor.py:346`: há juiz equivalente e caso que morde.** A pergunta de negócio é “o dia está completo o suficiente para não cobrar marco faltante?”: o propositor declara “dia completo, nada a perguntar”, e `classificar_falta` chama a mesma função posicional para produzir esse veto. (`app/ponto/propositor.py:268`; `app/escala/servico_jornada.py:436`; `app/escala/servico_jornada.py:442`; `app/chamados/reconciliador.py:105`)

| Sítio | Pergunta | Juiz registrado | Caso que morde | RED próximo |
|---|---|---|---|---|
| `app/ponto/propositor.py:346` — mesma linha antiga/atual | Há dia completo suficiente para vetar cobrança de marco faltante? (`app/ponto/propositor.py:325`; `app/chamados/reconciliador.py:112`) | `classificar_falta`, `app/escala/servico_jornada.py:279`; registro `app/core/juizes.py:180` | **Hipótese concreta:** D=06/11/2026, sexta; col01 ativo, vínculo 5x2 vigente desde 01/11, marcos 08–17 sem pausa; célula trabalha=True, DNA 08–17 igual ao template; sem ausência, férias ou outras marcas; E08 e S17, ambas retratadas às 18h; relógio 07/11/2026 12h. **Sítio: `True`**, pois lê ambas as marcas cruas e a atribuição encontra os dois slots. (`app/ponto/propositor.py:346`; `app/ponto/propositor.py:351`; `app/ponto/propositor.py:315`; `app/ponto/propositor.py:320`) **Juiz: `('E', 'saida_sem_entrada', 'ENTROU')`**: o universo de turnos exclui ambas, `do_dia=[]`, o veto posicional não entra e a contagem acusa a entrada ausente. (`app/ponto/turnos.py:1861`; `app/ponto/turnos.py:1862`; `app/escala/servico_jornada.py:392`; `app/escala/servico_jornada.py:403`; `app/escala/servico_jornada.py:440`; `app/escala/servico_jornada.py:469`; `app/escala/servico_jornada.py:471`) Execução/ocorrência em produção: **[não verificado]**. | `app/ponto/tests/test_clone_posicional_nao_cobra.py:61` |

Na fatia de nove sítios, isso aumenta os casos em um e reduz os sem juiz em um; mantém geofence agregado como **nenhum**, pois seu limiar de vínculo é outra pergunta. (`app/ponto/services/geofence.py:64`; `app/ponto/services/geofence.py:67`; `app/core/juizes.py:1178`)

## 08/11 16:41 · sessao e34fb5e18f84 · - · - · origem=Ronald (tmux)

Os 11 sítios continuam nas mesmas linhas do censo; nenhum sumiu. As consultas atuais estão em `app/escala/services/furos_diarios.py:261`, `app/escala/services/furos_diarios.py:267`, `app/ponto/management/commands/detectar_par_relampago.py:205`, `app/ponto/management/commands/detectar_par_relampago.py:290`, `app/ponto/management/commands/emitir_furo_retroativo.py:53`, `app/ponto/management/commands/emitir_furo_retroativo.py:74`, `app/ponto/management/commands/reconciliar_grade.py:84`, `app/ponto/management/commands/reconciliar_grade.py:88`, `app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/supra_juiz.py:113` e `app/ponto/management/commands/supra_juiz.py:132`.

| Sítio atual = antigo | Pergunta própria | Juiz registrado para essa pergunta | Caso comparativo | RED próximo |
|---|---|---|---|---|
| `app/escala/services/furos_diarios.py:261` | Já houve batida na vida e qual foi a última? O resultado define `ultima_batida`, `nunca_bateu` e `sumiu`. (`app/escala/services/furos_diarios.py:281`, `app/escala/services/furos_diarios.py:283`, `app/escala/services/furos_diarios.py:285`.) | **nenhum**; o próprio registro diz “última batida, nunca bateu… deveria ler nenhum”. (`app/core/juizes.py:767`.) | Sem comparação com juiz registrado; não transformar `ids_com_adesao` em juiz registrado por analogia. Ele é chamado pelo `ranking_colabs`, uma pergunta de janela distinta. (`app/escala/services/furos_diarios.py:228`, `app/escala/services/furos_diarios.py:232`.) | `app/escala/tests/test_furos_diarios.py:61` |
| `app/escala/services/furos_diarios.py:267` | Houve batida na competência capaz de fazer 100% furado virar `escala_x_real`, em vez de `sumiu`? (`app/escala/services/furos_diarios.py:264`, `app/escala/services/furos_diarios.py:284`, `app/escala/services/furos_diarios.py:285`.) | **nenhum** para a classificação `escala_x_real`/`sumiu`; o registro das perguntas de tela não declara essa pergunta. (`app/core/juizes.py:694`, `app/core/juizes.py:706`; ausência de entrada específica: [não verificado].) | Inaplicável sem juiz registrado. | `app/escala/tests/test_furos_diarios.py:61` |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | Qual membro do micropar pode ser retratado por simulação, dadas as guardas e o resto do dia? (`app/ponto/management/commands/detectar_par_relampago.py:208`, `app/ponto/management/commands/detectar_par_relampago.py:214`, `app/ponto/management/commands/detectar_par_relampago.py:229`.) | **nenhum**; “o par é relâmpago?” permanece em `SEM_JUIZ`. `par_completo` responde outra pergunta. (`app/core/juizes.py:1187`, `app/core/juizes.py:228`.) | Inaplicável sem juiz registrado. A escolha local exige exatamente uma exclusão que melhore e deixe score perfeito. (`app/ponto/management/commands/detectar_par_relampago.py:244`.) | `app/ponto/tests/test_par_relampago_v3.py:48`; guardas: `app/ponto/tests/test_guardas_par_relampago.py:50` |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | Quais marcas aprovadas são adjacentes, formam micropar e permitem retirar o par sem estragar a grade? (`app/ponto/management/commands/detectar_par_relampago.py:294`, `app/ponto/management/commands/detectar_par_relampago.py:298`, `app/ponto/management/commands/detectar_par_relampago.py:309`.) | **nenhum** para adjacência/relâmpago/retratabilidade. (`app/core/juizes.py:1187`.) A subpergunta “par completo?” possui `par_completo`, registrado em `app/core/juizes.py:228`. | Para a **subpergunta de completude**, **não achei divergência**: o sítio chama o próprio juiz na linha 300, com a janela carregada pelo próprio carregador do juiz nas linhas 288–289; o juiz simplesmente executa essa janela. (`app/ponto/management/commands/detectar_par_relampago.py:300`, `app/ponto/turnos.py:2197`, `app/ponto/turnos.py:2200`.) Isso não prova igualdade da candidatura inteira, pois a lista inicial filtra `aprovada=True`. (`app/ponto/management/commands/detectar_par_relampago.py:291`.) | `app/ponto/tests/test_par_relampago_pelo_turno.py:58`; candidatura geométrica: `app/ponto/tests/test_par_relampago_v3.py:75` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | Há marcas para incluir o colaborador e quantas há em cada data local? (`app/ponto/management/commands/emitir_furo_retroativo.py:55`, `app/ponto/management/commands/emitir_furo_retroativo.py:100`.) | **nenhum** para contagem/seleção; `classificar_dia` é consumidor dessa contagem, não uma autoridade registrada dessa pergunta. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`, `app/core/juizes.py:170`, `app/docs/JUIZES.md:91`; ausência de registro da contagem: [não verificado].) | Inaplicável sem juiz registrado para a pergunta própria. A contagem crua efetivamente altera o portão de furo: exige `not batidas_no_dia`. (`app/ponto/supra_juiz.py:313`.) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | Qual a data da primeira marca, usada como início da operação? (`app/ponto/management/commands/emitir_furo_retroativo.py:75`, `app/ponto/management/commands/emitir_furo_retroativo.py:105`.) | **nenhum** para esse piso de adesão nos registros pedidos. (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:101`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado. O piso altera `pre_adocao` no classificador. (`app/ponto/supra_juiz.py:288`, `app/ponto/supra_juiz.py:334`.) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/reconciliar_grade.py:84` | Quem bateu na janela e entra no universo inicial `batted`? A seleção final une esse conjunto a chamados abertos e colaboradores em operação. (`app/ponto/management/commands/reconciliar_grade.py:95`, `app/ponto/management/commands/reconciliar_grade.py:105`, `app/ponto/management/commands/reconciliar_grade.py:106`.) | **nenhum** para esse universo de medição. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado; não supor que trocar o conjunto sempre exclui colaborador, porque há a união com `com_chamado | em_op`. (`app/ponto/management/commands/reconciliar_grade.py:106`.) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:88` | Qual a primeira marca de cada selecionado, abaixo da qual dias devem ser ignorados? (`app/ponto/management/commands/reconciliar_grade.py:90`, `app/ponto/management/commands/reconciliar_grade.py:116`, `app/ponto/management/commands/reconciliar_grade.py:136`.) | **nenhum** para piso de adesão. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado. `piso_visual` não responde esse piso do reconciliador; é explicitamente piso do espelho. (`app/ponto/tests/test_piso_nao_sobe_por_batida.py:4`.) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:118` | Quais marcos ficaram faltando e o dia está limpo a ponto de matar o fato de um chamado? (`app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:155`, `app/ponto/management/commands/reconciliar_grade.py:266`.) | **`classificar_falta`**, registrado em `app/core/juizes.py:180` e `app/docs/JUIZES.md:91`; implementação `app/escala/servico_jornada.py:279`. | **CASO QUE MORDE**, detalhado abaixo. A guarda do juiz no ramo CREATE não protege o caminho `avaliado_limpo → mortos → RETRACT`. (`app/ponto/management/commands/reconciliar_grade.py:242`, `app/ponto/management/commands/reconciliar_grade.py:266`, `app/ponto/management/commands/reconciliar_grade.py:303`.) | `app/ponto/tests/test_reconciliar_grade_coberto.py:17` ou `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/supra_juiz.py:113` | Há marcas na janela para incluir o colaborador e quantas há em cada data local? (`app/ponto/management/commands/supra_juiz.py:117`, `app/ponto/management/commands/supra_juiz.py:222`.) | **nenhum** para contagem/seleção. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado para a pergunta própria. O resultado cru chega a `batidas_no_dia`; os portões de furo e furo parcial o leem. (`app/ponto/management/commands/supra_juiz.py:229`, `app/ponto/supra_juiz.py:314`, `app/ponto/supra_juiz.py:437`.) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |
| `app/ponto/management/commands/supra_juiz.py:132` | Qual a primeira marca usada como piso de operação? (`app/ponto/management/commands/supra_juiz.py:135`, `app/ponto/management/commands/supra_juiz.py:227`.) | **nenhum** para esse piso nos registros pedidos. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado; o piso alimenta `pre_adocao`. (`app/ponto/supra_juiz.py:288`.) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |

Caso concreto proposto para `reconciliar_grade:118`, deduzido do código e **não executado** [não verificado]:

- Cadastro hipotético: colaborador ativo, não isento, admissão e vínculo 5x2 vigentes desde 01/07/2026; turno 08:00–17:00, sem intervalo; célula de 20/07/2026 `trabalha=True`, DNA com E08:00/S17:00; sem ausência; chamado vivo `batida_ausente` com `data_turno=2026-07-20`. Relógio em 21/07/2026 20:00−03. Batidas únicas em 20/07 E08:00/S17:00, ambas `aprovada=False`, ambas retratadas em 21/07 10:00−03. Fixture proposta [não verificado].
- **Sítio:** recebe as duas marcas pelo manager cru; a grade por turno transforma todas as marcas E/S recebidas em `locs`, sem examinar retratação. Com os marcos ocupados, não há `missing`, o dia entra em `avaliado_limpo`, e o chamado recebe plano `RETRACT`. (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/escala/utils.py:1344`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:155`, `app/ponto/management/commands/reconciliar_grade.py:266`, `app/ponto/management/commands/reconciliar_grade.py:303`.)
- **Juiz:** carrega os turnos apuráveis; sem as duas retratadas, não há marca viva; monta esperado E/S e devolve `('E', 'saida_sem_entrada', 'ENTROU')`. (`app/escala/servico_jornada.py:390`, `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:430`, `app/escala/servico_jornada.py:469`, `app/escala/servico_jornada.py:471`; exclusão de retratadas: `app/ponto/turnos.py:2864`.)
- Para tornar explícita a configuração do caso, usar `GRADE_POR_TURNO=True`: o wrapper encaminha então ao montador examinado. (`app/escala/utils.py:2107`.)

Contagem estrita do lote: **1 caso que morde; 0 sem divergência na pergunta própria integral; 0 sumidos; 10 sem juiz**. Há ainda **1 subpergunta sem divergência**, `par_completo` dentro de `varrer_pares_embutidos`, que não deve ser contada como cura da candidatura integral. (`app/ponto/management/commands/detectar_par_relampago.py:290`, `app/ponto/management/commands/detectar_par_relampago.py:300`, `app/core/juizes.py:1187`.)

## 08/11 16:41 · sessao bfe42fe62f74 · - · - · origem=Ronald (tmux)

Os nove sítios permanecem nas mesmas linhas do censo; nenhum sumiu. As referências abaixo são da árvore atual. Casos são hipóteses deduzidas do código; execução e ocorrência em produção: **[não verificado]**.

| Sítio antigo = atual | Pergunta do sítio | Juiz registrado da mesma pergunta | Caso que morde / limite | RED próximo |
|---|---|---|---|---|
| `app/ponto/propositor.py:346` | O dia está posicionalmente completo, ignorando tipos, pela atribuição global? (`app/ponto/propositor.py:325`, `app/ponto/propositor.py:351`) | **nenhum** para essa pergunta específica. O registro declara casamento de marco por `_match_marcos`, que é outro algoritmo; a função aqui chama `primeiro_furo_posicional` → `atribuir_global`. (`app/core/juizes.py:200`; `app/docs/JUIZES.md:27`; `app/ponto/propositor.py:315`; `app/ponto/propositor.py:351`) | Comparação com juiz registrado: inaplicável. Não elevar `primeiro_furo_posicional` ou `atribuir_global` a juiz registrado. (`app/ponto/propositor.py:351`; `app/core/juizes.py:200`) | `app/ponto/tests/test_furo_posicional.py:6` |
| `app/ponto/services/flip_auto.py:35` | Quais batidas adjacentes têm mesmo tipo e distância inferior a 12h na janela? (`app/ponto/services/flip_auto.py:31`, `app/ponto/services/flip_auto.py:43`) | **nenhum**. `decidir_tipo_estrito` responde se um tipo deve virar, não quais pares adjacentes entram no inventário. (`app/core/juizes.py:198`; `app/ponto/services/flip_auto.py:41`) | Comparação inaplicável. A posterior chamada à porta apenas fornece `n_bat` a `classificar_par`; não refaz a adjacência. (`app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`) | `app/ponto/tests/test_flip_auto_pergunta_ao_juiz.py:78` (exercita fila S-S) |
| `app/ponto/services/flip_auto.py:291` | O dia está perfeito antes/depois dos flips, condicionando auto/humano? (`app/ponto/services/flip_auto.py:303`, `app/ponto/services/flip_auto.py:305`) | **nenhum** para “dia perfeito” no registro exigido. Existe `dia_perfeito`, chamado pelo sítio, mas seu título de “juiz único” está somente na própria docstring. (`app/ponto/services/simular_dia.py:1`; `app/ponto/services/simular_dia.py:11`; registro de marcos: `app/core/juizes.py:200`) | Comparação com juiz registrado dessa pergunta: inaplicável. Não dizer que diverge de `decidir_tipo_estrito`: o sítio chama exatamente esse decisor para os flips; o universo cru só permanece na grade. (`app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:300`; `app/ponto/services/flip_auto.py:303`) | `app/ponto/tests/test_flip_auto.py:7` |
| `app/ponto/services/geofence.py:51` | Quantas das dez últimas marcas com GPS caem no raio do posto e sustentam divergência do vínculo? (`app/ponto/services/geofence.py:37`, `app/ponto/services/geofence.py:56`, `app/ponto/services/geofence.py:67`) | **nenhum** para contagem/evidência do vínculo. `classificar_posicao` está registrado para a posição de **uma batida**, não para esse agregado. (`app/core/juizes.py:1178`; `app/ponto/services/geofence.py:95`) | Comparação inaplicável. O ramo individual “dentro” é geometricamente igual a `classificar_posicao`: ambos usam distância ≤ raio; isso não registra um juiz para a seleção das dez marcas nem para o limiar de 80%. (`app/ponto/services/geofence.py:58`; `app/ponto/services/geofence.py:67`; `app/ponto/services/geofence.py:114`) | `app/ponto/tests/test_reconciliar_vinculo.py:49` |
| `app/ponto/services/veredito_celula.py:215` | O dia já está completo e qual combinação de flips/plantios o deixa perfeito? (`app/ponto/services/veredito_celula.py:220`, `app/ponto/services/veredito_celula.py:223`, `app/ponto/services/veredito_celula.py:271`) | **nenhum** para essa pergunta composta. `dia_perfeito` existe, mas não está no registro exigido; `_match_marcos` só é registrado para casamento de marcos. (`app/ponto/services/simular_dia.py:1`; `app/core/juizes.py:200`; `app/docs/JUIZES.md:27`) | Comparação com juiz registrado da mesma pergunta: inaplicável. O sítio já usa `grade_dia` e `dia_perfeito`; não inventar divergência dele com essas mesmas funções recebendo o mesmo universo. (`app/ponto/services/veredito_celula.py:216`; `app/ponto/services/veredito_celula.py:218`; `app/ponto/services/veredito_celula.py:219`) | `app/ponto/tests/test_veredito_celula.py:45` |
| `app/ponto/services/vinculo_na_lista.py:49` | A evidência GPS aponta vínculo divergente, coordenada errada, itinerância ou posto ausente? (`app/ponto/services/vinculo_na_lista.py:86`) | **nenhum** no registro exigido. O sítio chama `classificar_evidencia`, denominado juiz pelo próprio módulo, mas esse nome não aparece em `core/juizes.py` nem em `docs/JUIZES.md`. A distinção entre agregado e posição individual aparece nas assinaturas. (`app/ponto/services/vinculo_evidencia.py:48`; `app/ponto/services/geofence.py:95`; `app/core/juizes.py:1178`) | Comparação inaplicável. Já delega o veredito a `classificar_evidencia` com os números coletados; não há divergência inventável com a mesma chamada e mesmos números. (`app/ponto/services/vinculo_na_lista.py:86`; `app/ponto/services/vinculo_evidencia.py:48`) | `app/ponto/tests/test_c2_vinculo_vira_lista.py:63` |
| `app/ponto/utils.py:186` | Há turno aberto que impede oferecer o colaborador para cobertura? (`app/ponto/utils.py:183`; `app/ponto/utils.py:192`) | **`turno_aberto_de`**, `app/ponto/turnos.py:2281`; registro `app/core/juizes.py:199`. | **Hipótese:** col01 ativo, perfil colaborador, mesma empresa, sem demissão, férias ou ordem ativa; vínculo/célula 06/11/2026 08–17, sem intervalo; única batida E 08:00, retratada 08:30; consultar cobertura 09–17, relógio 09:00. Sítio: **exclui**, porque a E crua está nas 14h e entra em `com_turno_aberto`. Juiz: **`None`**, pois a única marca é excluída antes do pareamento. (`app/ponto/utils.py:187`; `app/ponto/utils.py:191`; `app/ponto/utils.py:192`; `app/ponto/turnos.py:2217`; `app/ponto/turnos.py:2219`; `app/ponto/turnos.py:2222`; `app/ponto/turnos.py:2276`) | **`app/ponto/tests/test_motor_substituicao.py:208`** — testa diretamente esse helper. |
| `app/ponto/utils.py:244` | A próxima batida é E ou S? (`app/ponto/utils.py:240`) | **`proximo_tipo_de`**, `app/ponto/turnos.py:2448`; registro `app/core/juizes.py:197`. | **Hipótese:** col02 com vínculo ativo de ciclo **intermitente**; única marca E em 06/11/2026 08:00, retratada 08:30; relógio 09:00. Sítio: **S**, pois alterna a última marca gravada sem janela/filtro. Juiz: **E**, pois o ramo intermitente busca só marcas vivas das últimas 14h; encontra nenhuma. (`app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/ponto/turnos.py:2464`; `app/ponto/turnos.py:2467`; `app/ponto/turnos.py:2471`) | **`app/ponto/tests/test_proxima_batida.py:23`** |
| `app/ponto/views.py:1598` | Qual entrada pertence ao dia da justificativa? Seleciona E pelo dia civil e grava a associação. (`app/ponto/views.py:1598`; `app/ponto/views.py:1629`; `app/ponto/views.py:1637`) | **`turnos_do_colab`**, `app/ponto/turnos.py:1848`; registro da geometria/data_turno `app/core/juizes.py:196`, `app/docs/JUIZES.md:20`. **Observação de fronteira:** o registro é da geometria do turno; não há juiz específico para escolher o FK da justificativa. | **Hipótese:** col03, vínculo/célula 06/11/2026 08–17, única marca E 08:00 retratada 08:30; POST em 08/11 com `data_turno=2026-11-06`, sem justificativa prévia. Sítio: grava `j.batida_entrada` apontando a marca retratada. Geometria do juiz para 06/11: não contém essa batida nem entrada, pois a consulta a exclui. (`app/ponto/views.py:1598`; `app/ponto/views.py:1635`; `app/ponto/views.py:1637`; `app/ponto/turnos.py:1861`; `app/ponto/turnos.py:1862`; `app/ponto/turnos.py:1913`) | **`app/ponto/tests/test_vinculo_justificativa_cross_day.py:30`** |

Contagem desta fatia, **se a escolha da entrada for tratada como geometria do turno**: **3 com caso, 0 sem divergência, 0 sumidos, 6 sem juiz**. Se a pergunta exigir literalmente o juiz específico do FK da justificativa, a última linha passa a **nenhum**, resultando em **2 com caso e 7 sem juiz**. A fronteira decorre da pergunta registrada (`app/core/juizes.py:196`) e da gravação efetuada (`app/ponto/views.py:1637`).

## 08/11 16:40 · sessao da69c2b5513e · - · - · origem=Ronald (tmux)

Os oito sítios continuam nas linhas informadas no censo; os trechos atuais mostram as consultas nos mesmos endereços. (`app/chamados/management/commands/auditar_disputas.py:103`; `app/chamados/services/explicador.py:40`; `app/chamados/services/explicador.py:53`; `app/chamados/services/explicador.py:132`; `app/chamados/services/materializacao.py:94`; `app/chamados/services/materializacao.py:100`; `app/core/views.py:360`; `app/core/views.py:793`.)

| Sítio atual | Pergunta própria | Juiz registrado | Caso/comparação | RED próximo |
|---|---|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | Esta batida retroativa tem tipo invertido? (`:119-120`) | `decidir_tipo_estrito`, `app/ponto/turnos.py:2395`; registro `app/core/juizes.py:198`. | **Não achei divergência.** Chama exatamente `decidir_tipo_estrito(mk, localtime(timestamp), tipo)`, com os marcos do vínculo vigente naquele dia (`auditar_disputas.py:111-120`). O juiz é puro e não recebe nem filtra um universo de batidas: recebe esses mesmos três insumos (`app/ponto/turnos.py:2395`). A presença de retratada na consulta não faz o decisor discordar de si próprio. | `app/chamados/tests/test_auditar_disputas_flip_pelo_juiz.py:21`. |
| `app/chamados/services/explicador.py:40` | Existe vizinha ±20 min do slot e qual é seu tipo, para explicar a ausência de prova? (`:34-41`, `:61-65`) | **nenhum** para esse diagnóstico; os registros de pergunta viva e fio vivo são outras perguntas (`app/core/juizes.py:266`; `app/core/juizes.py:275`). | Sem comparação com juiz registrado. O diagnóstico pode dizer “batida existe, mas juiz não reconhece” inclusive quando a vizinha está retratada, pois a consulta não a exclui (`:40`, `:65`); isso não demonstra divergência com um juiz registrado da mesma pergunta. | `app/chamados/tests/test_explicador_disputa.py:65`. |
| `app/chamados/services/explicador.py:53` | Houve marcação na janela estendida do dia que justifique encaminhar o diagnóstico para cadastro? (`:47-59`, `:98-100`) | **nenhum** para causa/dono desse diagnóstico; `pergunta_viva` responde vida da pergunta (`app/core/juizes.py:112`, `:266`), não causa/dono. | Sem comparação com juiz registrado. A existência crua em `:53-55` basta para produzir `CADASTRO|…corrigir a escala` em `:56-59`; não existe autoridade registrada para esse encaminhamento nos registros consultados (`app/core/juizes.py:170`; `app/docs/JUIZES.md:19`). | `app/chamados/tests/test_explicador_disputa.py:89`. |
| `app/chamados/services/explicador.py:132` | Quais marcas alimentam os diagnósticos de todas as perguntas da disputa? (`:125-133`) | **nenhum** para diagnóstico/dono. Para a pergunta distinta “alguma pergunta está viva?”, existe `fio_vivo`, `app/chamados/juizes.py:1233`, registrado em `app/core/juizes.py:275`. | Quanto ao diagnóstico, sem comparação com juiz registrado. **Quanto ao fechamento, não achei divergência:** `pode = not fio_vivo(ps)` usa a mesma função e o mesmo universo `ps`; `_bts` não participa desse booleano (`explicador.py:121-138`). Não transformar a consulta de diagnósticos em um segundo veredito de fechamento. | `app/chamados/tests/test_explicador_disputa.py:65`; igualdade do fechamento também cabe em `:80`. |
| `app/chamados/services/materializacao.py:94` | A vizinha cronológica anterior na janela de 12 h tem o mesmo tipo do plantio e o barra? (`:85-99`) | **nenhum**. `decidir_tipo_estrito` decide flip por marcos, não adjacência/guarda de plantio (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`). | Sem comparação com juiz registrado. Exemplo do comportamento cru: plantio S19:00, única anterior S15:00 retratada → devolve essa S e barra, pois não lê `retratada_em` (`materializacao.py:94-99`). Não há resposta de juiz registrado da mesma pergunta para contrastar. | `app/chamados/tests/test_guarda_paridade_materializador.py:59`. |
| `app/chamados/services/materializacao.py:100` | A vizinha cronológica posterior na janela de 12 h tem o mesmo tipo do plantio e o barra? (`:100-105`) | **nenhum**, pela mesma distinção de pergunta (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`). | Sem comparação com juiz registrado. Exemplo do comportamento cru: plantio S12:00, nenhuma anterior, única posterior S15:00 retratada → devolve essa S e barra (`materializacao.py:100-105`). Não eleger o juiz de flip como juiz dessa guarda. | `app/chamados/tests/test_guarda_paridade_materializador.py:59`. |
| `app/core/views.py:360` | Há entrada nas últimas 24 h que retire o colaborador da lista de ausentes? (`:357-363`, `:388-405`) | Para a pergunta estrita de existência de entrada: **nenhum**. Para o julgamento do dia, o registro marca esse trecho como pendente de `_TT3`, cuja autoridade é `classificar_falta` (`app/core/juizes.py:696`, `:709`, `:741-742`). | **Caso se consolidado como julgamento de ausência/furo:** colab ativo visível; vínculo 6x1 08–17 sem intervalo, vigente; dia útil com `trabalha=True`, sem ausência; relógio 17/09/2026 18:00 local; única E07:00 retratada. Sítio: inclui a E e pula o colab em `:399`, não o lista ausente (`views.py:360`, `:399`). Juiz: após fim previsto (`servico_jornada.py:375-378`), universo vem de turnos sem retratadas (`:386-404`), esperado E/S (`:430-435`), vazio ⇒ `('E', 'saida_sem_entrada', 'ENTROU')` (`:468-472`). É dedução de leitura, sem execução. Se a comparação exigir literalmente “teve entrada nas últimas 24 h”, usar **nenhum**, sem generalizar o juiz de furo. | `app/core/tests/test_supervisor_turnos_pelo_juiz.py:60`; fixture de retratada próxima em `app/core/tests/test_e4_7_painel_entrada_retratada.py:59`. |
| `app/core/views.py:793` | Houve atividade hoje, para classificar cobertura do posto? (`:865-866`) | **nenhum** para essa pergunta. `turno_aberto_de` é autoridade de turno aberto vivo agora, pergunta distinta (`app/core/juizes.py:694`, `:707`). | Sem comparação com juiz registrado. Com posto contendo colab, `previsto=0`, `presente=0` e só batida retratada hoje, o cru produz `teve_atividade=True` (`views.py:793`, `:865`) e `classificar_cobertura` retorna `encerrado` (`app/ponto/selecao_periodo.py:237-243`). Essa função não está registrada como juiz nesses registros; não inventar resposta autoritativa alternativa. | `app/core/tests/test_painel_tv_turno_pelo_juiz.py:67`. |

Contagem deste lote, usando a pergunta estrita do sítio: **0 com caso comparável que morde; 1 sem divergência; 0 sumidos; 7 sem juiz**. Se a consolidação adotar o julgamento `_TT3` explicitamente atribuído ao supervisor no registro, a linha `core/views.py:360` passa a **1 caso que morde**, e os sem juiz caem para **6**. A ressalva decorre da diferença entre “existiu entrada nas últimas 24 h” e “o dia acusa falta/furo”, explicitada acima. (`app/core/views.py:360`; `app/core/views.py:399`; `app/core/juizes.py:696`; `app/core/juizes.py:741-742`.)

## 08/11 16:40 · sessao 80bb75efeb0a · - · - · origem=Ronald (tmux)

Censo estático: `test_o280_bateria_da_lei.py` possui **138 IDs em CASOS** (117 `mes`, 8 `periodo`, 13 `fora`) e **165 métodos `test_`**; `test_bateria_lavratura.py` possui **52 métodos**. Contagem por AST, sem importar a aplicação nem executar testes; fontes: `app/ponto/tests/test_o280_bateria_da_lei.py:278`, `:1073`, `:5991`; `app/ponto/tests/test_bateria_lavratura.py:169`, `:1549`.

Inventário de **oráculos positivos inscritos**, deduplicados por ID, excluindo gêmeos medidos e perguntas abertas:

| Rubrica | Casos positivos | Observação |
|---|---:|---|
| HE50 | **0** | A6 e H2 recusam definir as faixas sem lastro; único positivo `he50` direto é B-I28 medido. `app/ponto/tests/test_o280_bateria_da_lei.py:2206`, `:5117`, `:5994`. |
| HE100 não feriado | **0** | H2 afirma somente total e soma das partes, sem fixar 120/840. `app/ponto/tests/test_o280_bateria_da_lei.py:5991`. |
| HE100 feriado | **8 IDs / 10 métodos** | F10, F10b, F10c, F11, F11r, F11b, F11c, F11d. Normas individualizadas em `app/ponto/tests/test_o280_bateria_da_lei.py:305`, `:309`, `:314`, `:340`, `:346`, `:352`, `:356`, `:362`. |
| FT | **5 cenários / 7 métodos** | F3, F3b, F8, B-O263FT e cenário sem ID `test_v3_a_folga_trabalhada_segue_a_familia_1`; estes afirmam sobretudo minutos do motor, não necessariamente rubrica final da folha. `app/ponto/tests/test_o280_bateria_da_lei.py:1386`, `:1783`, `:2041`, `:2702`, `:4092`, `:4514`, `:4540`. |
| Noturno | **12 IDs / 16 métodos** | F2, F4, F4b, F4c, F10i, F11, F11r, F11b, F11c, F11d, B-O263, I26-H1. `app/ponto/tests/test_o280_bateria_da_lei.py:1330`, `:1399`, `:1420`, `:1446`, `:1628`, `:1654`, `:1675`, `:1697`, `:1714`, `:1769`, `:2689`, `:5966`. |
| Intra | **5 IDs / 5 métodos decididos** | B-O272b, B-SOBRA, F7d, F7e, B-P4b; F2b separado como aberto, com dois pinos. `app/ponto/tests/test_o280_bateria_da_lei.py:2552`, `:2760`, `:3738`, `:3803`, `:5253`, `:1354`, `:3607`. |
| Atraso + antecipada | **27 IDs / 29 métodos** | Relação completa enviada nas mensagens anteriores; inclui I26-D4, cujo esperado varia na tabela. `app/ponto/tests/test_o280_bateria_da_lei.py:976`, `:5710`. |
| Falta diurna exportada | **0 diretamente** | Há 2 IDs com literal positivo de **falta total**, F-FOTO e F-FOTOb; não conferem separação diurna/noturna. F1/F1b e A1 usam a própria autoridade de previsto para construir o esperado. `app/ponto/tests/test_o280_bateria_da_lei.py:1303`, `:1316`, `:2058`, `:2090`, `:2146`. |

O censo acima **não é o censo de todos os zeros**; a extração dos zeros enviada antes identifica chamadas diretas, mas não cobre integralmente dicionários intermediários, tabelas e asserções fora de `confere`. Portanto, não declarar aqueles zeros como inventário exaustivo. [não verificado]

As quatro propriedades:

- **Partes somam:** `test_P4_contador_igual_universo` e `test_v3_P4_contador_igual_universo`; lavratura também possui `test_PROPRIEDADE_as_partes_somam_o_total_em_A/B`. `app/ponto/tests/test_o280_bateria_da_lei.py:2874`, `:5463`; `app/ponto/tests/test_bateria_lavratura.py:1483`, `:1486`.
- **Idempotência:** `test_P2_idempotencia_na_mesma_instancia`, `test_v3_P2_idempotencia_da_releitura`; lavratura `test_PROPRIEDADE_lavrar_duas_vezes_e_uma_vez_em_A/B/F/G`. `app/ponto/tests/test_o280_bateria_da_lei.py:2801`, `:5408`; `app/ponto/tests/test_bateria_lavratura.py:1500`, `:1503`, `:1506`, `:1509`.
- **Previsto independente do realizado:** fora dos dois arquivos, existe `test_idempotente_e_previsto_nao_depende_do_realizado`, que cobra 440 antes e depois da mudança das batidas. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:236`.
- **Todo leitor mesmo número:** cobertura parcial. F11c confere FechamentoMensal e DiaPago; testes por fatia conferem espelho e PDF, em outro cenário. Não achei teste único ligando espelho, fechamento, export e DiaPago com resposta da regra; C7 do próprio núcleo está declarado `fora`. `app/ponto/tests/test_o280_bateria_da_lei.py:610`, `:1749`; `app/ponto/tests/test_regua_por_fatia_na_janela.py:186`, `:204`. O teste do export admite não atravessar `medir`/TXT. `app/folha/tests/test_porta_do_export.py:224`.

Alertas para consolidação: A6 atualmente contém um pino legal aberto/zero e testes antigos afirmando 1020/60/300; estes últimos são medição, segundo o próprio CASOS. A6b é medido/pergunta aberta. F2b, F7g e F10h têm pergunta aberta; C10 guarda comportamento atual. Não tratá-los como resposta legal decidida. `app/ponto/tests/test_o280_bateria_da_lei.py:457`, `:474`, `:1354`, `:426`, `:329`, `:639`.

## 08/11 16:38 · sessao 253d5dbeaf26 · - · - · origem=Ronald (tmux)

Confirmado: **sem aplicação e empate resultam em 548,57 minutos legais**, para 19–07 com pausa01–02. A regra registrada mantém a prorrogação quando CLT não está declarado: `(540−60) × 60/52,5 = 548,571428 min`. `app/docs/LEIS.md:83`; `app/core/regua_cct.py:192`; `app/core/regua_cct.py:204`.

Fontes exatas, para empresa `EMP`:

- `legal (empresa cct, sem aplicacao de convencao: EMP)`
- `legal (empresa cct, empate de convencao: EMP)`

`app/core/regua_cct.py:371`.

**SINEEPRES:** referências à cl.10 e à restrição22–05 existem, mas texto literal integral não achei; cabe **DUVIDA sem número**. `app/core/views_config.py:293`; `app/ponto/motor_calculo_v2.py:745`; `app/core/models.py:72`.

**8792:** usar ausência decidida cujo tipo desconta; zero batidas, sozinho, não estabelece a rubrica. `app/docs/LEIS.md:87`.

## 08/11 16:38 · sessao ae3c2c50aa96 · FIO-E0-V4-E2-FUNCAO · leitura · origem=Code

**1. MATRIZ DE TRANSIÇÕES**

Referências abreviadas:

- `T` = anexo `predesenho_e2_tarefa_do_dia_e93205c6.py`, numerado desde a primeira linha.
- `P` = `app/chamados/tests/test_e2_tarefa_do_dia.py` do diff; a coluna de prova identifica o teste escrito. Execução dos testes: [nao verificado].
- `E1` = arquivo da árvore fornecida; `E2` = arquivo de destino do diff.
- `NS↑` = define `nao_sei=True`; `[]` = nenhuma ação. Nos ramos por instância, outra instância pode já ter definido `nao_sei=True`. (`T:173`, `T:246`, `T:283`)
- `porta=P` = chamado retornado por `chamado_que_lista`, se estiver na fila; caso contrário, `None`. (`T:122`, `T:123`)

As linhas abaixo seguem a precedência do código. Os retornos antecipados do corpo acontecem **depois** da montagem da carga e da obtenção de grade, instâncias, marcos e cobertura. (`T:308`, `T:396`, `T:399`)

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| `agora=None`, argumento da porta (`T:416`) | Nenhum consultado | Levanta exceção | `ValueError` (`T:417`) | `SeloDeterminismoERelogio.test_MORDE_sem_agora_recusa` |
| Sem `carga` (`T:418`) | Resultados carregados para um colaborador e um dia | Abre carga e delega | Resultado de `carga.tarefa` (`T:419`, `T:422`) | `SeloDeterminismoERelogio.test_determinismo_e_lote_igual_a_casca` |
| Carga fornecida, mesmo `agora` (`T:420`) | Usa o instante da carga | Delega | Resultado de `carga.tarefa` (`T:422`) | `SeloDeterminismoERelogio.test_determinismo_e_lote_igual_a_casca` |
| `carga.agora != agora` (`T:420`) | Nenhum novo julgamento | Levanta exceção | `ValueError` (`T:421`) | sem teste | [nao verificado]
| Dia fora de `[carga.ini,carga.fim]` (`T:394`) | Nenhum julgamento desse dia | Levanta exceção | `ValueError` (`T:395`) | `Caso12Verificador.test_a_carga_so_responde_a_sua_janela` |
| Colaborador sem entrada em `_cadastro`, inclusive colaborador não carregado (`T:317`, `T:376`) | Grade não chega a ser consultada | Acesso direto ao mapa | `KeyError`; não produz `nao_sei` (`T:376`) | sem teste | [nao verificado]
| Grade sem `dias`/`isento_ponto`, ou retornando `None` (`T:376`, `T:378`) | Retorno sem o contrato esperado | Indexa o retorno sem proteção | `KeyError`/`TypeError`; não produz `nao_sei` (`T:378`) | sem teste | [nao verificado]
| Dia chega ao juiz sem `insumo_dia_sem_trabalho` ou sem suas chaves | Insumo ausente | Acesso direto às chaves | `KeyError` propagado (`T:199`; `app/ponto/supra_juiz.py:70`, `:71`) | sem teste |
| Autoridade indisponível, importação ou chamada lança exceção | Não há resposta utilizável | Não há `try/except` na porta/carga/corpo | Propaga a exceção (`T:308`, `T:347`, `T:367`, `T:376`, `T:422`) | sem teste | [nao verificado]
| Grade: `tipo_dia='fora_vinculo'` (`T:176`) | Leitor único declarou fora do vínculo | Encerra antes da isenção/tranca | Razão `fora_vinculo`; instâncias vazias; `nao_sei=False` (`T:177`) | `Caso13VinculoEIsencaoPeloLeitorUnico.test_demitido_na_vespera_sem_fato_nao_deve_nada`; `test_admitido_amanha_sem_fato_nao_deve_nada` |
| Grade: isenção verdadeira, sem retorno anterior (`T:180`) | Leitor único declarou isenção | Conserva instâncias, esvazia ações | `isento_ponto`; sem ações (`T:181`) | `Caso13VinculoEIsencaoPeloLeitorUnico.test_isento_de_ponto_nao_deve_marco` |
| Par `(colab,dia)` presente em `_trancado` (`T:367`, `T:187`) | `dias_trancados` verdadeiro | Conserva instâncias, esvazia ações | `competencia_trancada`; sem ações (`T:190`) | `Caso14DiaArquivadoPelaTranca.test_dia_trancado_nao_tem_tarefa` |
| Competência reaberta, ou toque humano posterior reconhecido (`T:367`) | Juiz da tranca falso | Segue o julgamento normal | Não retorna `competencia_trancada` (`T:187`) | `Caso14DiaArquivadoPelaTranca.test_reaberta_nao_tranca`; `test_desarquivado_pelo_O44_e_julgado_como_qualquer_dia`; `test_cobrar_e_resolver_dia_desarquivam_so_aquele_dia` |
| Juiz da tranca não encontra período fechado | Ausência de tranca resulta em falso | Segue o julgamento normal | Demais ramos (`E1 app/chamados/juizes.py:965`; `T:187`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| `gdia=None`, ou grade declara `sem_celula` (`T:196`) | Dia ausente/sem célula no leitor único | Encerra | `sem_celula`; instâncias vazias; `NS↑` (`T:197`) | `Caso2NoturnoSemIrA23.test_a_pergunta_da_saida_e_do_turno_de_21_pelo_dia_do_turno` — parcial: verifica célula e instâncias, não a razão/NS |
| Dia de trabalho sem instâncias; há marcos e nenhum venceu (`T:200`, `T:201`) | Teto: todos pendentes | Encerra | `nao_venceu`; instâncias vazias; `nao_sei=False` (`T:204`) | `CasoClassesDoDiffV6b.test_T_ata_sem_lampadas_antes_de_todo_teto_nada_e_exigivel` |
| Dia de trabalho sem instâncias; algum marco venceu (`T:200`) | Teto já exigível; leitor não fornece lâmpadas | Encerra | `ata_sem_lampadas`; `NS↑` (`T:206`) | `Caso12Verificador.test_dia_de_trabalho_sem_lampadas_e_nao_sei`; teste T acima | [nao verificado]
| Dia de trabalho sem instâncias e sem marcos (`T:200`) | Teto sem resposta: lista vazia | Mesmo ramo residual | `ata_sem_lampadas`; `NS↑` (`T:206`) | sem teste específico | [nao verificado]
| Dia sem trabalho, sem instâncias (`T:199`, `T:232`) | `dia_sem_trabalho=True` | Laço vazio | Instâncias vazias; razão geral permanece `''` (`T:173`, `T:286`) | `Caso5TrocaDePlantao.test_cada_um_deve_o_seu_turno` |
| Instância em dia sem trabalho (`T:238`) | `dia_sem_trabalho=True` | Não consulta a decisão da pergunta | `dia_sem_trabalho`; ações `[]` (`T:239`) | `Caso12Verificador.test_folga_com_lampadas_apagadas_nao_cobra` |
| Lâmpada `True` (`T:241`) | Leitor único: marco aceso | Não consulta decisão/cobertura para cobrar | `aceso`; ações `[]` (`T:242`) | `CasoClassesDoDiffV6b.test_L_lampada_acesa_responde_antes_da_pergunta` |
| Lâmpada `None`, inclusive chave `acesa_lavrada` ausente (`T:84`, `T:244`) | Leitor não sabe se aconteceu | Marca desconhecimento | `lampada_nao_sabe`; ações `[]`; `NS↑` (`T:245`) | sem teste com asserção específica desse retorno | [nao verificado]
| Lâmpada apagada sem marco correspondente por tipo/hora (`T:74`, `T:99`) | Teto ausente: `vencido=None` | Marca desconhecimento | `marco_fora_do_turno`; ações `[]`; `NS↑` (`T:248`) | sem teste | [nao verificado]
| Vínculo do dia ausente ou sem tipo de escala | Corpo do teto devolve `[]` | Sem casamento de teto; aplica os ramos anteriores conforme as lâmpadas | `marco_fora_do_turno` ou `ata_sem_lampadas`, salvo retorno anterior (`E2 app/ponto/services/cartorio.py:855`; `T:99`, `T:206`, `T:248`) | sem teste específico |
| Marco apagado ainda não vencido (`T:252`) | Teto falso | Aguarda o teto | `nao_venceu`; ações `[]` (`T:253`) | `Caso1DoisMarcosNoMesmoDia.test_o_teto_e_por_marco` |
| Agora antes do fim do turno, mas marco anterior vencido | Teto é individual por marco | Julga o marco anterior; mantém o fim pendente | Pode haver tarefa antes do fim do turno (`T:252`, `T:274`) | `Caso1DoisMarcosNoMesmoDia.test_o_teto_e_por_marco` |
| Agora depois do fim, antes de fim + 30 minutos | Teto da saída ainda falso | Aguarda | Saída `nao_venceu` (`E2 app/ponto/services/cartorio.py:859`; `T:252`) | `Caso2NoturnoSemIrA23.test_a_saida_de_22_vence_pelo_instante` |
| Agora alcançou marco + teto | Teto verdadeiro, por `<= agora` | Segue para cobertura/pergunta | Não há encerramento global por “turno terminou” (`E2 app/ponto/services/cartorio.py:859`; `T:255`) | `Caso2NoturnoSemIrA23.test_a_saida_de_22_vence_pelo_instante`; `SeloDeterminismoERelogio.test_relogio_1400_x_1401_sem_escrita` |
| Cobertura parcial lavrada (`T:208`) | `ausencia_parcial_cobra_o_resto=True` | Desconsidera cobertura geral; segue pelas lâmpadas/perguntas | Demais ramos (`T:208`) | sem teste específico | [nao verificado]
| Sem cobertura para o dia (`T:208`) | Cobertura ausente: `None` | Segue para pergunta | Demais ramos (`T:255`, `T:265`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| Cobertura aprovada, marco apagado e vencido (`T:255`) | Ausência `aprovada` | Dispensa ação | `ausencia_aprovada`; ações `[]` (`T:256`) | `Caso4Atestado.test_ausencia_sem_pergunta_pela_cobertura` |
| Cobertura aguardando documento (`T:258`) | Ausência `aguardando_documento` | Atribui ao colaborador | `ausencia_aguarda_documento`; `entregar_documento(colaborador,None)` (`T:259`) | `Caso12Verificador.test_ausencia_aguardando_documento_e_do_colaborador` |
| Outra cobertura não rejeitada (`T:262`) | Ausência ainda a decidir | Atribui ao admin | `ausencia_a_decidir`; `decidir_ausencia(admin,None)` (`T:263`) | `Caso4Atestado.test_ausencia_sem_pergunta_pela_cobertura` |
| Segunda ocorrência da posição, com pergunta na posição (`T:266`) | Identidade não separa a ocorrência | Não escolhe pergunta | `instancia_sem_identidade`; ações `[]`; `NS↑` (`T:268`) | sem teste | [nao verificado]
| Segunda ocorrência, sem pergunta na posição (`T:266`) | Nenhuma pergunta encontrada | Solicita emissão | `sem_pergunta`; `perguntar(sistema,emissor)` (`T:272`) | sem teste | [nao verificado]
| Primeira ocorrência sem pergunta (`T:274`) | Nenhuma pergunta encontrada | Solicita emissão | `sem_pergunta`; `perguntar(sistema,emissor)` (`T:275`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| Há perguntas candidatas (`T:265`) | Não consulta a canônica da E1; toma a última por `pk` | Define `pergunta_id` e consulta decisão | Resultado de `_pela_decisao` (`T:278`, `T:280`) | sem teste com candidatas concorrentes | [nao verificado]

A seleção de perguntas tem estes caminhos próprios; uma pergunta excluída deixa a instância seguir como `sem_pergunta` se nenhuma outra candidata permanecer. (`T:221`, `T:274`)

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| `conversa=True` (`T:322`) | Discriminador da pergunta do fato | Não carrega a conversa | Sem candidata dessa conversa (`T:321`) | `Caso12Verificador.test_a_conversa_nao_e_a_pergunta_do_marco` |
| `data_evento=NULL` ou fora da janela (`T:323`) | **Não consulta** `dia_do_turno_da_linha` | Query exclui a linha | Não há ramo `nao_sei` específico dessa pergunta (`T:321`, `T:326`) | sem teste para NULL | [nao verificado]
| Pergunta gravada no dia civil seguinte, diferente do turno pedido (`T:323`, `T:230`) | **Não relê** o dia pelo juiz do turno | Não associa à véspera | Turno pedido fica sem essa pergunta | `Caso2NoturnoSemIrA23.test_MORDE_data_evento_do_dia_civil_nao_e_relido_como_vespera` |
| Motivo fora do catálogo de marcos (`T:222`) | Catálogo não o identifica como marco | Ignora | Sem candidata desse motivo | sem teste específico | [nao verificado]
| Via fora do fato, inclusive duplicata (`T:224`) | Via atual exclui a identidade | Ignora, salvo exceção temporal da retratação | Sem candidata | sem teste específico para duplicata | [nao verificado]
| Via `motivo_retratado`, sem retratação posterior a `agora` (`T:224`) | Identidade atual excluída | Ignora | Sem candidata | `SeloHorizonteCortaOsFatosDaPergunta.test_no_presente_a_via_atual_decide_como_antes` |
| Via `motivo_retratado`, mas ato de retratação posterior a `agora` (`T:338`) | Ato ainda fora do horizonte | Permite que a pergunta participe | Decisão anterior continua aplicável (`T:224`) | `SeloHorizonteCortaOsFatosDaPergunta.test_retratacao_posterior_a_agora_nao_mata_a_pergunta_em_agora` |
| Última decisão até `agora` é retratação (`T:228`) | Sequência encerra a participação | Ignora | Sem candidata | mesmo teste anterior, leitura após a retratação | [nao verificado]
| Via limpa e resposta posterior à retratação (`T:224`, `T:228`) | Último ato é resposta | Mantém a pergunta | `aguarda_validacao` | `SeloHorizonteCortaOsFatosDaPergunta.test_no_presente_a_via_atual_decide_como_antes` |
| Decisão posterior a `agora` (`T:336`) | Ato não pertence ao horizonte | Não inclui em `_dec` | Julga pela última decisão até `agora`, ou pela ausência dela | `SeloHorizonteCortaOsFatosDaPergunta.test_decisao_posterior_a_agora_nao_existe_em_agora`; `test_ultima_decisao_e_a_ultima_ate_agora` |
| Passivo com `ato_em > agora` (`T:343`) | Autoridade da E1 devolve instante posterior | Não inclui no passivo | Não bloqueia a decisão anterior | `SeloHorizonteCortaOsFatosDaPergunta.test_passivo_posterior_a_agora_nao_existe_em_agora` |
| Batida com `criado_em > agora` (`T:355`) | Efeito ainda fora do horizonte | Não inclui em `_efeitos` | Dentro do SIM, segue sem esse efeito | `SeloHorizonteCortaOsFatosDaPergunta.test_efeito_posterior_a_agora_nao_existe_em_agora` |

Para os próximos ramos, a instância já passou por dia de trabalho, lâmpada apagada, teto vencido, cobertura e seleção de pergunta. (`T:238`, `T:255`, `T:280`) [nao verificado]

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| Pergunta no passivo (`T:124`) | `linhas_da_projecao_sem_linha`: projeção sem ato correspondente | Não inventa ação | `passivo_sem_linha`; ações `[]`; `NS↑` (`T:125`, `T:282`) | `Caso8PassivoSemInventar.test_projecao_sem_linha_e_nao_sei` |
| Nenhuma decisão até `agora` (`T:126`) | Sequência vazia | Cobra resposta | `aguarda_resposta`; `responder(colaborador,P)` (`T:127`) | `Caso1DoisMarcosNoMesmoDia.test_a_resposta_de_uma_nao_fecha_a_outra` |
| Última decisão resposta com texto vazio (`T:129`) | `texto_vazio=True` | Cobra nova resposta | `respondida_sem_texto`; `responder(colaborador,P)` (`T:131`) | sem teste | [nao verificado]
| Última decisão resposta com texto (`T:130`) | `texto_vazio=False` | Encaminha ao admin | `aguarda_validacao`; `validar(admin,P)` (`T:132`) | `Caso1DoisMarcosNoMesmoDia.test_a_resposta_de_uma_nao_fecha_a_outra` |
| Última decisão recusa (`T:133`) | Sequência: `RECUSA` | Devolve ao colaborador | `devolvida_ao_colaborador`; `responder(colaborador,P)` (`T:134`) | sem teste | [nao verificado]
| Última decisão reabertura (`T:133`) | Sequência: `REABERTURA` | Devolve ao colaborador | Mesmo retorno anterior | `CasoClassesDoDiffV6b.test_C_chamado_vivo_e_porta_nao_dono` |
| Última decisão devolução (`T:135`) | Sequência: materialização recusada | Encaminha ao admin | `materializacao_recusada`; `validar(admin,P)` (`T:136`) | `Caso3MaterializacaoRecusada.test_devolucao_volta_ao_admin` |
| Validação; lavra ausente, vazia ou indeterminada (`T:348`, `T:139`) | Sem SIM/NAO vigente | Não chama novamente o juiz caro | `veredito_sem_lavra_vigente`; ações `[]`; `NS↑` (`T:140`) | `Caso11ViaSemTestemunha.test_sem_lavra_vigente_e_nao_sei` |
| Validação; lavra vencida por insumo posterior ou expiração (`T:349`) | `esta_stale=True` | Exclui lavra utilizável | Mesmo retorno anterior | `Caso7EventoSemEntrega.test_o_efeito_e_o_veredito_do_juiz_e_nao_suposto`; `Caso12Verificador.test_lavra_com_relogio_virado_e_nao_sei` |
| Validação; `julgado_em > agora` | `esta_stale=True`: lavra futura | Exclui lavra utilizável | Mesmo retorno anterior (`T:349`, `T:140`; `E2 app/chamados/veredito_lavrado.py:199`) | `Selo0514LavraNoRelogioDaBorda.test_lavra_julgada_depois_de_agora_nao_existe_em_agora` |
| Validação; lavra vigente `nao` (`T:141`) | Juiz do fechamento lavrado: NAO | Solicita materialização | `validada_sem_efeito`; `materializar(sistema,materializacao)` (`T:142`) | `Caso7EventoSemEntrega.test_o_efeito_e_o_veredito_do_juiz_e_nao_suposto` |
| Validação; lavra vigente SIM e Batida apurável carregada (`T:143`) | SIM; efeito presente | Solicita novo julgamento da célula | `efeito_sem_lampada`; `rejulgar(sistema,cartorio)` (`T:144`) | mesmo teste anterior | [nao verificado]
| Validação; lavra vigente SIM, sem Batida apurável carregada (`T:145`) | SIM, inclusive via sem testemunha | Encerra | `encerrada_pelo_juiz`; ações `[]` | `Caso11ViaSemTestemunha.test_o_juiz_do_fechamento_responde_e_nao_a_batida` |
| Última decisão silêncio (`T:146`) | Sequência: `SILENCIO` | Encerra | `silencio_decidido`; ações `[]` (`T:147`) | `Caso6Concorrencia.test_ato_duplicado_e_a_mesma_tarefa` |
| Absorção; ausência aprovada (`T:150`) | Status atual da ausência: aprovada | Encerra | `absorvida_aprovada`; ações `[]` (`T:151`) | sem teste desse ramo: `Caso4` é interceptado pela cobertura | [nao verificado]
| Absorção; ausência rejeitada (`T:152`) | Status atual: rejeitada | Devolve ao colaborador | `absorcao_rejeitada`; `responder(colaborador,P)` (`T:153`) | `Caso4Atestado.test_absorcao_segue_a_decisao_da_ausencia` |
| Absorção; aguardando documento (`T:154`) | Status atual: aguardando documento | Atribui ao colaborador | `absorcao_aguarda_documento`; `entregar_documento(colaborador,None)` (`T:155`) | `Caso12Verificador.test_absorcao_aguardando_documento_e_do_colaborador` |
| Absorção; outro status, inclusive ausência/status ausente (`T:149`) | Status não corresponde aos três anteriores | Atribui ao admin | `absorcao_pendente`; `decidir_ausencia(admin,P)` (`T:156`) | sem teste desse ramo | [nao verificado]
| Ato não reconhecido (`T:157`) | Sequência sem interpretação neste corpo | Marca desconhecimento | `ato_desconhecido:<ato>`; ações `[]`; `NS↑` | sem teste | [nao verificado]
| Âncora ausente ou chamado fora da fila (`T:123`) | `chamado_que_lista=None`, ou fila não o contém | Mantém ação sem porta | Ação de responder/validar/decidir com `porta=None` | `CasoClassesDoDiffV6b.test_U_universo_soberano_sem_chamado_cada_dono_pela_sequencia`; `Caso12Verificador.test_a_porta_e_o_chamado_na_fila` |

Há duas distinções necessárias sobre **juiz devolvendo None/ausente**:

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| Juiz do dia do episódio devolve `None` | A E2 **não consulta esse juiz** ao carregar as perguntas; usa `data_evento` gravado (`T:321`) | Não existe tradução desse None na tarefa | Não há retorno específico; as recusas pertencem às portas da E1 (`app/chamados/services/fio.py:1395`, `:1407`) | sem teste |
| Juiz do fechamento devolve `None` durante **lavrar**, fora da tarefa | O ternário de `lavrar` trata o retorno como falso | Lavra `nao`; se vigente e com validação, tarefa pede materialização | `validada_sem_efeito`, não `nao_sei` (`E2 app/chamados/veredito_lavrado.py:237`; `T:142`) | sem teste para o juiz retornando None |
| Juiz do fechamento lança exceção durante **lavrar** | `lavrar` captura e grava `indeterminado` | Tarefa exclui essa lavra | `veredito_sem_lavra_vigente`; `NS↑` (`E2 app/chamados/veredito_lavrado.py:239`, `:243`; `T:349`, `T:140`) | `Caso11ViaSemTestemunha.test_sem_lavra_vigente_e_nao_sei` verifica o valor indeterminado, não a falha do juiz |

A declaração “0 escritores” tem uma dependência que não permite afirmá-la incondicionalmente: o juiz da tranca chama `get_user_sistema`, cujo corpo usa `get_or_create`. O teste da tranca prepara esse usuário antes da medição; sua existência em produção não aparece no dado fornecido. (`E2 app/chamados/juizes.py:1124`; `app/core/utils.py:18`; `P::Selo14JuizEmLote._universo`; existência em produção: [nao verificado])

---

**2. O QUE O REBASE SOBRE A E1 MUDA**

“Não mudou” abaixo significa conteúdo integral igual ao blob OLD indicado no diff, conferido por hash do arquivo. “Mudou” significa conteúdo atual diferente daquele blob; histórico intermediário de edições/reversões: [nao verificado].

Os caminhos da tabela são relativos a `app/`.

| Arquivo do diff | E1 também mudou? | Encontro e significado — referências dos dois lados |
|---|---|---|
| `chamados/juizes.py` | **Sim** — `86a84167` → `8b80f4d4` | E1 usa `_dia_de_turno_da_pergunta`: conversa devolve `None`, sem competência de fato. O hunk E2 lê `data_evento` cru no lote de um, recolocando o dia civil da conversa na tranca. (`E1:332`, `:336`, `:948`; `E2:933`) |
| `chamados/management/commands/auditar_invariantes_chamados.py` | **Sim** — `e0faea82` → `e18325f6` | No trecho S6, continuam as chamadas antigas sem horizonte/agora. E2 acrescenta uma borda comum para marcos e staleness. Onde mais o arquivo dive

[truncado em 20000 de 28413 caracteres]

## 08/11 16:37 · sessao 1f01c7b4cabf · - · - · origem=Ronald (tmux)

**Lista emitida encontrada: 8 eventos, sujeitos ao cadastro de rubrica por empresa/global.** São sete eventos em horas e um em dias; os códigos FOPAG vigentes em produção não são verificáveis sem o cadastro. `app/folha/export.py:439-452`, `app/folha/export.py:732-736`, `app/folha/export.py:976-983`, `app/folha/models.py:47-54`.

| Evento exportado | Fonte em produção |
|---|---|
| `hora_extra_diurna` — HE50 | `app/folha/export.py:441` |
| `hora_extra_100_diurna` — HE100 normal | `app/folha/export.py:421`, `app/folha/export.py:442` |
| `hora_extra_feriado_diurna` — HE100 feriado | `app/folha/export.py:420-423`, `app/folha/export.py:443` |
| `folga_trabalhada_diurna` | `app/folha/export.py:446` |
| `adicional_noturno` | `app/folha/export.py:447` |
| `intrajornada_diurna` | `app/folha/export.py:448` |
| `atraso_diurno` — atraso **mais** saída antecipada | `app/folha/export.py:383-399`, `app/folha/export.py:451-452` |
| `falta_diurna` — dias decididos | `app/folha/export.py:169-185`, `app/folha/export.py:215-220`, `app/folha/export.py:732-736` |

Não confundir com os 24 itens possíveis de `EVENTO_CHOICES`: o gerador efetivo emite somente os oito acima. `app/folha/models.py:13-38`, `app/folha/export.py:722-736`.

HE noturna e hora reduzida não geram linha independente; o adicional inclui as horas noturnas trabalhadas, inclusive extras. DSR é excluído expressamente. Existem helpers de 8932, mas a montagem efetiva não os chama nem emite esse evento. `app/folha/export.py:424-437`, `app/folha/export.py:453-460`, `app/folha/export.py:193-206`, `app/folha/export.py:722-736`.

Códigos históricos/documentados: HE50 150, HE100/feriado/FT 200, adicional 25, intra 270, atraso+antecipada 8069, falta 8792; o código configurado vigente segue **[não verificado]**. Fontes: `app/folha/export.py:384`, `app/folha/export.py:430-458`, `app/folha/export.py:187-214`; HE100 global 200: `app/folha/migrations/0009_evento_he100_rubrica_200.py:9-10`; FT global passa a 200: `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`. Há demonstração explícita de intra global 243 e override por empresa 270: `app/folha/tests/test_r1_caracterizacao_rubricas_map.py:21-43`.

**Mutantes de exportação que escapam ao escopo das duas baterias.** Os arquivos `test_o280_bateria_da_lei.py` e `test_bateria_lavratura.py` não chamam os montadores de TXT/eventos; a primeira costura chama motor e calculador, a segunda cartório. A sobrevivência abaixo é conclusão de leitura; execução de mutantes: **[não verificado]**. `app/ponto/tests/test_o280_bateria_da_lei.py:128-159`, `app/ponto/tests/test_o280_bateria_da_lei.py:3538-3558`, `app/ponto/tests/test_o280_bateria_da_lei.py:4609-4636`, `app/ponto/tests/test_bateria_lavratura.py:134-145`.

| Evento | Mutação concreta | Caso mínimo que deve comparar também a emissão |
|---|---|---|
| HE50 | Substituir valor de `hora_extra_diurna` por zero. `app/folha/export.py:441` | Jornada CLT comum, uma hora extra autorizada: exige uma hora no evento HE50; art.59 §1 e L-097. `app/docs/LEIS.md:115` |
| HE100 normal | Retirar a subtração `- extra_100_feriado`. `app/folha/export.py:421` | Feriado decidido cuja HE100 toda é feriado: exigir **zero no evento normal** e total no evento feriado. Não exige decidir a faixa >2h. Regra da separação: `app/folha/export.py:420-443`; dúvida da faixa: `app/ponto/tests/test_o280_bateria_da_lei.py:2198-2206`. |
| HE100 feriado | Inverter/negar a condição que preserva dobra cadastrada no 12x36. `app/folha/export.py:422` | Par de fixtures 12x36 com sindicato `feriado_12x36_em_dobra=False/True`, exigindo ausência/presença da parcela. `app/folha/export.py:406-412`; cadastro e leitor: `app/core/models.py:56`, `app/core/regua_cct.py:477`. |
| FT | Zerar `horas_folga_trabalhada`. `app/folha/export.py:446` | Trabalho comprovado em folga com escala certa: horas em FT/100%; cortes 19 e 27/09 descritos em `app/ponto/calculador/regras.py:367-388`; rubrica global: `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`. |
| Noturno | Zerar `horas_noturnas`. `app/folha/export.py:447` | 12x36 com AplicacaoConvencao cl.38-d: hora de 60min mantém adicional, não zero. L-036: `app/docs/LEIS.md:84`. |
| Intra | Zerar `horas_intra_indenizada`. `app/folha/export.py:448` | Supressão **integral** de intervalo devido evita escolher entre integral e suprimido parcial: ambos coincidem neste caso; L-031/L-032: `app/docs/LEIS.md:79-80`. Para supressão parcial, manter DUVIDA conforme conflito declarado. |
| Atraso+antecipada | Suprimir a parcela de saída antecipada da chamada. `app/folha/export.py:451-452` | Dia com ambas as pontas devidas e não nulas; exigir soma no TXT, respeitando teto previsto−realizado. Soma da rubrica: `app/folha/export.py:383-399`; L-093: `app/docs/LEIS.md:111`. |
| Falta | Omitir `_buf.extend(montar_bloco_falta(...))`. `app/folha/export.py:735` | Um dia de falta **decidida/aprovada**, sem batidas, exige um dia e registro 11 com data; dia apenas sem batida não substitui a decisão para esta rubrica. L-039: `app/docs/LEIS.md:87`; emissão: `app/folha/export.py:169-185`, `app/folha/export.py:215-220`. |

**Normas que exigem cuidado na consolidação:**

- L-031 diz só o suprimido, mas sua própria linha declara que a leitura da flag foi adiada: não tratar descrição do motor como decisão nova. `app/docs/LEIS.md:79`.
- L-086 contém texto antigo sobre feriado previsto, explicitamente revogado na própria linha: feriado previsto dobra segundo cadastro/ciclo. `app/docs/LEIS.md:118`.
- A faixa HE100 acima de duas horas não tem lastro normativo achado na bateria; ela própria o declara. `app/ponto/tests/test_o280_bateria_da_lei.py:2198-2206`, `app/ponto/tests/test_o280_bateria_da_lei.py:3705-3712`. Use DUVIDA para um valor positivo dessa faixa.
- L-085: turno pertence ao início. `app/docs/LEIS.md:117`.
- L-035/L-036: prorrogação e redução devem seguir regime/cadastro/CCT, inclusive cl.38-d. `app/docs/LEIS.md:83-84`.
- O obstáculo declarado de construir caso completo que entra no TXT está mesmo na ponta citada pelo backlog: `app/folha/tests/test_porta_do_export.py:216-229`; não é uma prova de fechamento=export=DiaPago, porque o teste declara que passou a chamar a função de comparação isolada.

## 08/11 16:37 · sessao 253d5dbeaf26 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum motor/teste executado e nenhuma escrita realizada. [não verificado] [nao verificado]

**Normas utilizáveis na tabela**

| Regra | Evidência |
|---|---|
| Gabarito = regra aplicada a cadastro e batidas; 4 propriedades fixas | `app/docs/LEIS.md:136` |
| Fonte de previsto = grade; proibido derivador paralelo | `app/docs/LEIS.md:75` |
| Jornada pertence ao dia de início, inclusive feriado | `app/docs/LEIS.md:117` |
| Feriado previsto pode dobrar conforme ciclo/cadastro; cláusula antiga L-086 foi revogada | `app/docs/LEIS.md:118` |
| Teto de pontualidade = previsto − trabalhado; soma atraso + antecipada limitada; rateio proporcional | `app/docs/LEIS.md:111` |
| L-031 True indeniza somente suprimido, False preassinalado; registro também declara leitura da flag adiada | `app/docs/LEIS.md:79` |
| Piso legal tolerância 5 min/marcação e 10/dia | `app/core/regua_cct.py:39` |
| CLT 12x36 não prorroga noturno pós-05h; CLT comum prorroga; CCT depende do sindicato | `app/core/regua_cct.py:153` |
| Cl.38-d Vigilantes Londrina, PR000251/2026: hora 60 minutos no 12x36, adicional permanece | `app/core/regua_cct.py:127` |
| Empresa CCT sem aplicação/empate → piso legal com `fonte`; empate não escolhe pela ordem | `app/core/regua_cct.py:1`, `app/core/regua_cct.py:269`, `app/core/regua_cct.py:366` |

**Lacuna principal do recorte dos cinco casos**

A fixture base cria empresa sem `regime_trabalhista` (`app/ponto/tests/test_o280_bateria_da_lei.py:1113`), e a costura chama `get_motor` diretamente (`app/ponto/tests/test_o280_bateria_da_lei.py:146`, `app/ponto/tests/test_o280_bateria_da_lei.py:156`). Portanto os casos F4b/F4c de flags não provam a escolha pelo cadastro Empresa + AplicacaoConvencao + Sindicato. O F4c troca o fator por argumento direto e compara resultados (`app/ponto/tests/test_o280_bateria_da_lei.py:4358`). Criação de AplicacaoConvencao/Sindicato nesses dois arquivos: não achei. [não verificado]

**Casos mínimos sugeridos para a tabela 2**

| id | cadastro e batidas locais | resposta pela regra | norma |
|---|---|---|---|
| CCT-REAL-38D | 12x36 19–07, pausa 01–02; regime CCT, aplicação vigente Vigilantes, redução afastada e pós5=False, dia sem feriado; E19/S01/E02/S07 | Noturno relógio `(22–01)=180 + (02–05)=180 = 360 min`; legais `360 × 1 = 360 min = 6h`; adicional continua devido, não multiplicar as horas da rubrica por 20% | `app/core/regua_cct.py:127` |
| CCT-SEM-APLICACAO | Mesmas batidas, empresa CCT sem aplicação | Noturno relógio 360; legais `360 × 60/52,5 = 411,428571 min = 6,857142h`; `fonte` informa sem aplicação | `app/core/regua_cct.py:153`, `app/core/regua_cct.py:366` |
| CCT-EMPATE | Mesmas batidas; duas aplicações vigentes no mesmo nível | Mesmo piso: `360 × 60/52,5 = 411,428571 min`; fonte informa empate; repetir invertendo a ordem das aplicações | `app/core/regua_cct.py:269`, `app/core/regua_cct.py:366` |
| CCT-6X1 | 6x1 noturno 19–07 com pausa 01–02; empresa CCT + aplicação SINEEPRES, pós5=False | Com cláusula comprovada restringindo a 22–05, relógio360 e legais411,428571; texto primário da cl.10 não achei nesta busca; vinculação legal específica permanece **DUVIDA** | Restrição configurável descrita em `app/core/models.py:72`; regra cadastro em `app/core/regua_cct.py:166` |
| CCT-FERIADO-12X36 | 12x36 07–19, pausa12–13, empresa CCT com aplicação e sindicato feriado_12x36_em_dobra=True; feriado previsto | Jornada trabalhada `300+360=660 min`; pela regra cadastrada, dobra660 min; cláusula específica que conceda a dobra não achei, então **DUVIDA** para nome/cláusula da CCT | `app/core/models.py:49`, `app/folha/export.py:402` |
| LEI-71 | 5x2 08–17 pausa cadastral12–13, pausa realizada12–12:30; empresa CLT; sem feriado | **DUVIDA**: aplicar só suprimido pós-2017 ou critério “INTEGRAL” declarado no comentário? Não fechar número por iniciativa própria | `app/docs/LEIS.md:79`, `app/ponto/motor_calculo_v2.py:66` |
| LEI-58 | 5x2 08–17 pausa12–13; CLT; E08:06/S12/E13/S17; sem feriado | **DUVIDA**: prevalece literal 5/marcação e 10/dia ou decisão declarada 10/ponta? O caso isola marcação >5 com total diário <10 | `app/core/regua_cct.py:45`, `app/ponto/motor_calculo_v2.py:52`, `app/ponto/tests/test_o280_bateria_da_lei.py:650` |
| LEI-DEGRAU | Dia normal com excesso superior a120 minutos e pares reconhecidos | **DUVIDA**: qual cláusula cria 100% após duas horas? Art.59 só fixa mínimo50%; não inventar repartição | `app/ponto/tests/test_o280_bateria_da_lei.py:2203` |

**Correção indispensável para o inventário**

F9 começa anunciando 5/10 legal, mas os asserts reais esperam atraso **0 em08:07** e **11 em08:11**. Não contar F9 como teste de cobrança literal após cinco minutos. `app/ponto/tests/test_o280_bateria_da_lei.py:3294`, `app/ponto/tests/test_o280_bateria_da_lei.py:3351`, `app/ponto/tests/test_o280_bateria_da_lei.py:3355`.

F1 calcula o esperado pela própria autoridade de previsto, sem número legal independente; não contar como caso com gabarito aberto independente do código. `app/ponto/tests/test_o280_bateria_da_lei.py:1302`, `app/ponto/tests/test_o280_bateria_da_lei.py:1316`.

**Outras lacunas/limites**

Troca de empresa no meio da competência não tem cadastro temporal correspondente; o próprio CASOS F5b declara fora, pois colaborador tem uma empresa e a troca reescreve a janela inteira. `app/ponto/tests/test_o280_bateria_da_lei.py:379`.

A dificuldade da prova completa export/fechamento está documentada: a fixture precisa integração Domínio, fechamento com horas, células julgadas em todos os dias e rubricas homologadas; o teste referido acabou medindo função parcial. `app/folha/tests/test_porta_do_export.py:216`.

Para 8932, o ponto exporta dias aprovados dos tipos que abonam, exceto exclusões do catálogo; férias ficam em módulo próprio. `app/folha/export.py:193`, `app/folha/export.py:207`. Não tratar abonos do motor como prova automática da linha8932: essa rubrica consulta Ausencia diretamente. `app/folha/export.py:201`.

Para mutantes do export, todos os casos “pela porta real” devem ir até o TXT: mesmo F11c mede FechamentoMensal e DiaPago, sem provar emissão do TXT. `app/ponto/tests/test_o280_bateria_da_lei.py:1732`, `app/ponto/tests/test_o280_bateria_da_lei.py:1745`.

_2905 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
