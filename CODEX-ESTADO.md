# CODEX-ESTADO

> Ultima resposta processada: **24/11/2026 02:40** · publicado em **24/11/2026 02:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 24/11 02:40 · sessao 8353229698a2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

Nova execução [nao verificado]. O executor grava arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/1f80df53084f4f5c8ea6abae96379375674459d4/bin/sonda_leitura.sh:126)

Saída fornecida [nao verificado]

```text
25
sonda_leitura: sonda_divergencia.py [284cfa46e4b59aba] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/wt-janela-turno-020503/app (copia), banco segue sombra como leitor

TOT diverge 25
TOT diverge_com_decisaohe 13
TOT diverge_janela_ligada 25
TOT pares_com_chave 23948
TOT sem_dia_do_juiz 107
rc_sombra=0
```

## 24/11 02:31 · sessao 98532e6761ac · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · verificador · origem=Code

**BLOQUEIA.** A nova âncora corrige a volta noturna antes do `hf`, mas cria um par invertido quando a volta e a saída já ocorreram depois do `hf`. Na geometria do selo B-I28, a janela da cópia devolveu **07:32 → 07:00 e 43 min fora**, para **11 min brutos**, em execução isolada em memória. A catraca do dia do par também falhou em duas verificações. `app/ponto/tests/test_o280_bateria_da_lei.py:5007,5081` · `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:168` · `app/core/tests/test_medida_turno_do_par.py:544,560`

## 1. Escritores, leitores e chamadores

- **OK — Memo e chave.** Há três escritas em `_dia_jornada_do_par`: inicialização vazia a cada `calcular_mes`, gravação no laço principal e gravação no crédito do turno aberto. O único leitor é `_janela_do_dia`. Nos dois caminhos, o motor grava `localizar(entrada)` e repassa a mesma entrada a `calcular_periodo`. As três implementações que chamam `_janela_do_par` e a segunda leitura de `Motor12x36ComEscala` usam a mesma `localizar`, que converte ao fuso configurado e preserva segundos e microssegundos. O clipe vem depois da busca. Não encontrei caminho normal com a entrada ajustada, truncada ou em outro fuso usada como chave. `app/ponto/motor_calculo_v2.py:28,73,1549,2343,2493,2593,2800,2927,3265,3593`
- **BLOQUEIA — A chave pode existir com um dia diferente do juiz.** `_dia_do_turno` transpõe `Turno.data_turno` para um mapa por instante localizado das batidas que o juiz classificou como `E`. `calcular_mes` grava esse mapa em `_dia_turno_juiz_calc`. Para `data_jornada`, o laço usa a data civil local na ausência da chave e, com saída anterior fechada e vão inclusivo de **0 a 6 h**, herda `_prev_dj` mesmo quando há resposta diferente do juiz. A janela lê `data_jornada`. O BO-B e a acusação de `aplicar_tolerancia` leem o mapa do juiz. Os overrides comercial e partido também reagrupam pelo juiz após `super().calcular_mes`. `app/ponto/motor_calculo_v2.py:867,884,1612,2030,2432,2439,2459,2494,2949,2983,3355,3367`
- **OK — Crédito do aberto, subclasses e FT.** `_d_a` vem do `data_jornada` já atribuído ao período aberto. O crédito seleciona turnos cujo `t.data_turno == _d_a` e grava `_d_a` para a entrada materializada, inclusive `_za.timestamp` na borda posicional. Essa seleção prova o dia **do grupo**. A chave individual pode faltar no mapa do juiz, que só indexa `E`. `MotorIntermitente` herda o 12x36, `MotorPlantao` delega ao 12x36 e `MotorTurnoPartidoNoturno` herda o partido. A FT do laço principal grava os dois memos e sai da janela pela guarda de folga. O crédito do aberto grava apenas o memo de dia e parte de `resultado.periodos`, cuja classificação é anterior. `app/ponto/motor_calculo_v2.py:889,1532,2483,2493,2529,2550,2559,2572,2593,3575,3601,3610`
- **OK — Marcos e parâmetros.** `_marcos_do_dia_fn` vem do vínculo ou do template. `_janela_do_dia` usa `hi`/`hf` para construir `m_entrada`/`m_saida` e grava `j['dia']`, `j['marco_entrada']` e `j['marco_saida']`. `marco_na_jornada` recebe `date`, hora cadastrada e referência local, mantém o `tzinfo` da referência e zera segundos e microssegundos do **marco**. Seus chamadores são as construções de entrada e saída do par e do BO-B. As pontas efetivas leem os respectivos `datetime`, a extra declarada, o piso, o teto e o estado autorizado. `hi`/`hf` continuam sendo as horas cruas usadas no relato. O cadastro de empresa é lido uma vez por cálculo, e o motor verifica a vigência pelo dia recebido. `app/escala/models.py:1128` · `app/colaboradores/models.py:54,117` · `app/ponto/janela_he.py:33,46` · `app/ponto/motor_calculo_v2.py:1372,1394,1562,1574,1633,1679,1735,1744,1767,1775,1782,2323,2334`

**Censo dos chamadores diretos na cópia**

| Severidade e método | Chamadores |
|---|---|
| **OK — `calcular_periodo`, código de execução** | Laço e crédito do aberto em `app/ponto/motor_calculo_v2.py:2495,2594`. Delegações de `MotorPlantao` e `Motor12x36ComEscala` em `app/ponto/motor_calculo_v2.py:3576,3588`. O censo não encontrou chamada direta em view, serviço, cron, management command ou script da cópia. |
| **OK — `_janela_do_dia`** | `_entrada_efetiva` e `_saida_efetiva` quando `_j` é omitido, e `_janela_do_par`. `app/ponto/motor_calculo_v2.py:1724,1766,1793` |
| **OK — `_janela_do_par`** | `Motor12x36`, `MotorTurnoPartido` e `MotorComercial`. `app/ponto/motor_calculo_v2.py:2800,2927,3265` |
| **FILA — `_entrada_efetiva` e `_saida_efetiva`** | `_janela_do_par` chama ambas passando `j`. `Motor12x36ComEscala` chama `_entrada_efetiva` de novo, sem `j`, e repete a consulta da janela e os efeitos de relato. Essa releitura já existia no comportamento descrito pelo selo B-O272b. `app/ponto/motor_calculo_v2.py:1793,3593` · `app/ponto/tests/test_o280_bateria_da_lei.py:2571` |
| **OK — `calcular_periodo`, testes diretos existentes** | `app/ponto/tests/test_plantao_he_distribuicao.py:27` · `app/ponto/tests/test_plantao_he_8a_hora.py:38` · `app/ponto/tests/test_noturno_desconta_intervalo.py:195,198` · `app/ponto/tests/test_o271_pontualidade_no_comercial.py:60,175` · `app/ponto/tests/test_antecipada_intervalo_12x36.py:53,59,65,70,79` · `app/ponto/tests/test_l084_vence_o_t8.py:65,173` · `app/ponto/tests/test_a6_12x36_tolerancia.py:33` · `app/ponto/tests/test_12x36_atraso_pela_autoridade.py:56,61,66,70,74` · `app/ponto/tests/test_12x36_com_escala_noturna.py:41,53,66` · `app/ponto/tests/test_motor_escala_marcos_do_dia.py:31` |
| **OK — `calcular_periodo`, teste novo** | Chamada com janela ligada que espera `RuntimeError`. `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206` |

## 2. Semântica do dia em cada leitor

- **BLOQUEIA — L-084 por par × BO-B por dia.** A primeira usa `data_jornada` para buscar os marcos, mede as pontas **cruas do par** e pode dispensar a janela antes de consultar o BO-B. O BO-B usa `_dt_b` do juiz e mede a primeira entrada e a última saída do grupo, ou a borda posicional. Ambos exigem **duas** distâncias absolutas estritamente acima de 180 min. Mesmo quando os dias coincidem, uma volta de pausa pode ficar a mais de 180 min de `hi` pela própria posição dentro da jornada. Quando os dias divergem, o BO-B pode dispensar a janela com base em outro dia. Sua acusação grava `_dt_b`, mas o contador grava `dia` do par. `app/ponto/motor_calculo_v2.py:1562,1589,1612,1633,1641,1650,1980`
- **BLOQUEIA — Relato × espelho × Gestão de HE.** `_acusa_cadastro_x_realidade` recebe `j['dia'] = data_jornada` e grava batida e marco em `HH:MM` e minutos arredondados. `pontas_do_relato` exige igualdade com a data da linha, mesma hora de marco, célula da ponta `missing` e ausência de ponta já marcada no mesmo sentido. O espelho apresenta as batidas no dia de `dia_das_batidas`, que usa o juiz por PK e data civil local para a batida que nenhum turno reclama. Portanto uma cauda herdada pelo motor em **D**, mas exibida em **D+1**, pode ter sua acusação ignorada. O caminho normal da grade também usa **primeira E e última S**, enquanto o motor agora pode recortar a saída de **cada par**. A Gestão recebe as pendências do espelho. `app/ponto/motor_calculo_v2.py:1702,1742,1780,2441` · `app/ponto/janela_he.py:217,300,316,334` · `app/ponto/turnos.py:2907` · `app/ponto/services/espelho.py:464,741,754` · `app/ponto/services/he_pendente_lavrado.py:114,132,148`
- **BLOQUEIA — Extra declarada e vigência.** O cadastro escreve minutos inteiros por ponta no tipo de escala. A geração congela os valores no DNA. O vínculo lê `(colaborador, data)`, responde zero se a célula existe sem declaração e lê o template se a célula falta. O motor consulta `data_jornada` pelo vínculo da fatia. Espelho e calendário consultam a data da linha e escolhem o vínculo desse dia. Concordam quando dia e vínculo coincidem. Na herança divergente, podem ler DNA, extra e vigência diferentes. A vigência inclui o dia inicial, o piso inclui a igualdade e a acusação por teto exige sobra estritamente maior. A janela calcula em minutos fracionários e as telas arredondam as pontas publicadas. `app/escala/services/cadastro_tipo.py:256` · `app/ponto/management/commands/gerar_celulas.py:128` · `app/escala/models.py:1238` · `app/ponto/motor_calculo_v2.py:1356,1382,1679,2329` · `app/ponto/services/espelho.py:736` · `app/colaboradores/services/calendario.py:413` · `app/ponto/janela_he.py:70,123,173,242`
- **BLOQUEIA — `DecisaoHE.data`.** O campo é `DateField`. A porta grava **a data recebida**, sem deduzir dia civil nem dia do turno. O formulário individual manda `s.data` do retrato do espelho. Lote e autorização por período leem o mesmo retrato, e o cron de recusa passa a data do item à porta. Assim, para turno reconhecido pelo juiz a origem usual é o **dia exibido do turno**. A divergência aparece quando ele difere de `data_jornada`, ou quando um chamador envia outra data. O motor carrega `{d.data: d.estado}` e `autorizado` solta ambas as pontas dos pares que consultam aquela chave. Uma decisão exibida em **D+1** pode ficar sem efeito no par herdado em **D**, ou a autorização de **D** pode alcançar um par exibido em **D+1**. O selo novo cria a linha em **D** diretamente no ORM e deixa essa ligação sem exercício. Datas de decisões reais [nao verificado]. `app/ponto/models.py:1300` · `app/ponto/portas/he.py:66,195,206,265,369,595` · `app/templates/ponto/gestao_he.html:259` · `app/ponto/views.py:543,630,684` · `app/ponto/services/gestao_he.py:397` · `app/ponto/management/commands/recusar_ponta_pequena.py:156,170` · `app/ponto/motor_calculo_v2.py:1428,1686,1730,1772` · `app/ponto/tests/test_janela_no_dia_do_turno.py:95`
- **FILA — `_janela_fora_por_l084`.** O motor inicializa a lista e insere `dia` com deduplicação nos dois ramos. O leitor externo efetivo encontrado é o teste que mede seu comprimento. O comando `diff_janela_he` conta `len(resultado.dias_cadastro_x_realidade)`, que também recebe acusações de teto da janela e da pontualidade. Assim, esse comando não mede dias únicos retirados pela L-084. `app/ponto/motor_calculo_v2.py:1596,1640,1702,2025,2336,2760` · `app/ponto/tests/test_o280_bateria_da_lei.py:2819` · `app/ponto/management/commands/diff_janela_he.py:63,70`

## 3. `RuntimeError` e produção

- **OK — Alcance no código atual.** A view e o comando de recálculo entram pelo fechamento. O recálculo por evento, inclusive disparado por um fato de um dia, recalcula a competência e chega a `calcular_mes`. Espelho e simulação de autorização passam pela autoridade do período. PDF e diagnóstico também chamam `calcular_mes`. O cron `vigiar_motor_autoridade` e os comandos `diff_espelho_alimentacao` e `diff_extrato_parcial` fazem o mesmo. A entrega de holerite busca o documento gravado. Não identifiquei caminho dessas rotas até o novo `raise` com a chave ausente. Uso fora da cópia e ocorrência em produção [nao verificado]. `app/ponto/views.py:1280` · `app/ponto/services/fechamento.py:286,754,827,1810` · `app/ponto/management/commands/recalcular_fechamento.py:118` · `app/ponto/services/espelho.py:1200,1215` · `app/ponto/services/autorizacao_he_periodo.py:96` · `app/relatorios/pdf_espelho.py:434` · `app/ponto/services/esmeril_espelho.py:174` · `app/config/crons.py:596` · `app/ponto/management/commands/vigiar_motor_autoridade.py:58` · `app/ponto/management/commands/diff_espelho_alimentacao.py:58` · `app/relatorios/management/commands/diff_extrato_parcial.py:68` · `app/holerite/views.py:1304,1393,1445`
- **FILA — Mudança de contrato do chamador direto.** Com par fechado, chave ausente e empresa identificada com janela ligada, o `raise` acontece **antes** da vigência e da consulta de marcos. Pode levantar mesmo com `janela_he_desde` ausente ou posterior ao par. É uma falha explícita, sem retorno à data civil. Se um chamador novo atingir esse ramo, a exceção propaga onde não houver captura. O recálculo por evento tem captura externa. Entre os testes diretos do censo, só o teste novo injeta o contexto de empresa com janela ligada, e espera a exceção. `app/ponto/motor_calculo_v2.py:1379,1394,1532,1549,1556,2793` · `app/ponto/services/fechamento.py:1807,1818` · `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206`

## 4. Marco de saída e valor do par

- **OK — 15:00–23:50 com saída 00:10.** No diff anterior, `m_saida` usava a data civil **da saída**, logo o marco era 23:50 de **D+1**. Com a âncora em **D**, fica 23:50 de **D**. Com a janela aplicável, piso e extra zero, a mesma regra L-097 passa a recortar 20 min, de 550 para 530 min brutos após a janela. Isso é consequência direta da âncora dentro do aval. `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:166` · `app/ponto/tests/test_janela_no_dia_do_turno.py:177`
- **BLOQUEIA — Par inteiro depois do marco.** No B-I28, a cauda de **07:32–07:43** herda o dia do turno 19–07. O motor conserva a entrada às 07:32 e recorta a saída a 07:00. Para a L-084, só a distância da entrada passa de 180 min. O cálculo carimba a duração invertida, limita trabalhadas a zero e conserva **43 min fora**. Antes, a fixture mantinha o par cru de 11 min. A nova saída viola as duas propriedades do aval. Resultado da execução isolada da janela em memória, com os insumos da fixture. Incidência em produção [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4993,5007,5081,5127` · `app/ponto/motor_calculo_v2.py:1574,1589,2800,2826` · `app/ponto/janela_he.py:168,175` · `app/ponto/tests/test_janela_no_dia_do_turno.py:121`
- **BLOQUEIA — A troca também altera a dispensa pela L-084 sem troca do dia civil da entrada.** Com `data_jornada=D`, escala **19:00–00:10**, par **D 00:00–23:55**, piso e extra zero, a geometria anterior via duas pontas longe e preservava **1.435 min brutos**. Com `hf` em **D+1**, a saída fica a 15 min do marco e a janela corta **1.140 min** na entrada. Restam **295 min brutos**. O caso foi reproduzido no recorte em memória. O pareamento dessa geometria em produção [nao verificado]. `app/ponto/motor_calculo_v2.py:1574,1589,1980` · `app/ponto/janela_he.py:115,125`
- **FILA — FT e 24x48.** A FT marcada no laço sai antes da construção dos marcos. Para plantão com `hi == hf`, a autoridade registra que cruza a meia-noite. Um plantão 07:00 de **D** até 07:00 de **D+1** conserva o `hf`. Para **06:40–07:30 de D**, com `hi == hf == 07:00`, a janela isolada passa de **0 min efetivos e 50 fora** para **30 efetivos e 20 fora**, pois o `hf` muda de **D** para **D+1**. O exemplo mostra a mudança de valor na borda de igualdade. Um caso real de 24x48 com essa geometria [nao verificado]. `app/ponto/motor_calculo_v2.py:1538,1578,2493,3575` · `app/escala/servico_jornada.py:104` · `app/ponto/janela_he.py:125,175`

## 5. Classes de reprovação

- **BLOQUEIA — Derivação paralela presente.** O juiz registrado da atribuição da batida é `turnos_do_colab`. O diff não introduz outro pareador nem outro predicado `cruza?`. Ele eleva a herança e a degradação civil já existentes a fonte do dinheiro da janela, enquanto BO-B e apresentação continuam lendo o dia do juiz. Há também composição paralela do **instante de saída**: `marcos_turno` encadeia todos os marcos, inclusive pausa, enquanto os dois ramos da janela usam apenas `hi/hf` e `+1`. Por exemplo, a sequência `18:00 / 23:00 / 01:00 / 19:00` põe `hf` em **D+1** no encadeador e em **D** na janela. Uso dessa sequência em produção [nao verificado]. `app/docs/JUIZES.md:20,24` · `app/core/juizes.py:215` · `app/escala/servico_jornada.py:69,82,104` · `app/ponto/motor_calculo_v2.py:1574,1612,1633,2439`
- **OK — Segundo escritor persistente, relógio fora da borda e mudança de trava ausentes no diff.** As escritas novas são no memo da instância, antes dos dois cálculos. A porta de HE continua adquirindo a trava e gravando a decisão. O marco usa a referência recebida. A L-084 segue antes do clipe e da leitura do estado autorizado. Há **zero `except` adicionados e zero removidos** no diff fornecido. O `except Exception` da consulta de empresa já existia. A chave ausente ganhou um `raise` e o retorno sem janela para empresa desligada ou indisponível. `app/ponto/motor_calculo_v2.py:1394,1549,1589,1674,2494,2593` · `app/ponto/portas/he.py:158,195,206` · `app/ponto/janela_he.py:46`
- **BLOQUEIA — Campo com dois significados presente.** `dia` no relato de janela e no contador vem do período com herança. `dia` da acusação BO-B e da pontualidade vem do juiz. `DecisaoHE.data` recebe o dia apresentado e o motor a consulta pelo dia herdado. `app/ponto/motor_calculo_v2.py:1641,1655,1686,1703,2030,2444` · `app/ponto/services/he_pendente_lavrado.py:148` · `app/ponto/portas/he.py:66`
- **OK — Os dois mutantes têm asserções capazes de morder.** Restaurar `entrada.date()` perde, por exemplo, o recorte de **2 min** esperado na cauda literal de col113. Suprimir o `+1` do marco **do par** quebra a guarda **18:40 → 07:00**, que espera saída em **D+1**, 720 min trabalhados e 20 fora. A execução em memória confirmou a quebra da geometria da guarda sem `+1`. O mutante que remove **só** o `+1` do BO-B não tem prova dedicada localizada. `app/ponto/tests/test_janela_no_dia_do_turno.py:160,231,310,323` · `app/ponto/motor_calculo_v2.py:1579,1635`
- **BLOQUEIA — Matriz incompleta e pré-requisito de pouso aberto.** As fixtures comercial e vespertina gravam `tipo_base='5x2'`, que a fábrica rejeita antes das asserções. A propriedade não cobre a cauda depois do `hf` do B-I28. O caso col200 cobra apenas a entrada, e o DNA criado na fixture não inclui `extra_declarada`, ainda que o tipo tenha minutos cadastrados. Suite integral verde, sonda dos 57 dias e dos ~16 mil pares e zero violações na frota [nao verificado]. `app/ponto/tests/test_janela_no_dia_do_turno.py:68,73,106,121,243,279,290,321` · `app/ponto/motor_calculo_v2.py:3634,3692` · `app/escala/models.py:1245` · `CLAUDE.md:383` · `CORTES.md:643` (anexo)
- **OK — `.replace(hour=...)` fora do módulo da janela ausente nos sítios vigiados.** O novo `replace` fica em `janela_he.py`. O teste estrutural verifica o módulo, os quatro métodos da janela e `calculador/regras.py`. `app/ponto/janela_he.py:48` · `app/ponto/tests/test_janela_he_regra.py:124,136,147,154`

## 6. Selos existentes e assinatura

- **OK — Assinatura no censo da cópia.** `entrada_efetiva` agora exige o marco como `datetime`. Um consumidor com `hi=` falharia pela assinatura, e um `time` posicional falharia na subtração. O chamador executável encontrado já passa `j['marco_entrada']`. `calculador/regras.py` só importa a função. `test_janela_he_regra.py` usa `calcular_mes` e verifica o import e a AST. O teste direto de `saida_efetiva` já passa `datetime`, e as chamadas com aliases no selo do calculador são texto de fixture AST. `app/ponto/janela_he.py:95,115` · `app/ponto/motor_calculo_v2.py:1734` · `app/ponto/calculador/regras.py:30` · `app/ponto/tests/test_janela_he_regra.py:67,124,136` · `app/ponto/tests/test_janela_he_saida.py:134` · `app/ponto/tests/test_chamador_do_calculador_aplica_a_janela.py:134`
- **BLOQUEIA — Duas falhas confirmadas na catraca.** Executei a versão em memória indicada no próprio teste, com `unittest` e sem Django. `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` falha porque o contador desce **84 → 83**. `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` falha porque a entrada civil da janela saiu e o registro mantém posições antigas, inclusive `2408 → 2444` na herança. `app/core/tests/test_medida_turno_do_par.py:20,299,544,560` · `app/ponto/motor_calculo_v2.py:1549,2444`
- **BLOQUEIA — A bateria O280 fixa comportamentos que a obra muda.** B-P4a exige o par antigo invertido e carrega um `expectedFailure` para os **150 min** da lei. B-I28 exige a cauda crua e **731 min** no dia, além de ter outro `expectedFailure`. São incompatibilidades identificadas por leitura e pela execução isolada da janela. Resultado integral desses testes Django e dos pinos [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4926,4935,4961,4982,5081,5113,5123`

## 7. Limite do aval, atraso e competência exportada

- **BLOQUEIA — O recorte excede a correção pedida no B-I28.** Os corpos de `calcular_per

[truncado em 20000 de 22110 caracteres]

## 24/11 02:29 · sessao 08df8f8609fb · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

# DEPOIS — NOTURNO-A-JANELA-NO-DIA-DO-TURNO

A soma das 18 classes é **402**, igual ao `C1 402` do anexo. Em `iguais_so_fechados`, são **320** casos, dos quais **308** têm `motor_agora=0` e **12** têm `motor_agora>0`. [nao verificado]

As marcações `[nao verificado]` nas tabelas indicam que a fonte é o anexo raspado fornecido, sem verificação independente na sombra. [nao verificado]

## 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | [nao verificado] |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+virada_ou_outro_dia` | 1 | [nao verificado] |
| `iguais_com_aberto` | 62 | [nao verificado] |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | [nao verificado] |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 45 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora>0` | 12 | [nao verificado] |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | [nao verificado] |
| `virada_ou_outro_dia` | 3 | [nao verificado] |
| **Soma = C1** | **402** | [nao verificado] |

O anexo termina em `rc_sombra=0` e não traz traceback para transcrever. [nao verificado]

## 2. Exemplos `fech` e corte da faixa 22h–05h

Cada `pares` mantém a ordem do anexo `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]`. O teste usa a borda da faixa que o par bruto alcançou. Nos 12 exemplos com motor zero, a saída efetiva eliminou o trecho após as 22h. Nos quatro com motor positivo, a entrada efetiva ficou antes das 05h do dia seguinte e o par preservou esse trecho da madrugada. [nao verificado] [nao verificado]

### `iguais_so_fechados|bruto5-30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto<=5|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva 19:00:00 < 22:00. [nao verificado] |
| 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 373 | 2026-10-22 | 48.8 | 0.0 | `[["2026-10-22 16:58:11", "2026-10-22 22:48:50", "2026-10-22 16:58:11", "2026-10-22 22:00:00", 301.8, 48.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-24 | 48.9 | 0.0 | `[["2026-10-24 16:52:22", "2026-10-24 22:48:55", "2026-10-24 16:52:22", "2026-10-24 22:00:00", 307.6, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-26 | 49.6 | 0.0 | `[["2026-10-26 16:56:11", "2026-10-26 22:49:38", "2026-10-26 16:56:11", "2026-10-26 22:00:00", 303.8, 49.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-28 | 48.9 | 0.0 | `[["2026-10-28 16:59:53", "2026-10-28 22:48:56", "2026-10-28 16:59:53", "2026-10-28 22:00:00", 300.1, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora>0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 37 | 2026-10-05 | 119.4 | 119.4 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 03:00:38", "2026-10-06 07:00:00", 239.4, 1.0]]` | Não, preservou 03:00:38–05:00 do dia seguinte. [nao verificado] |
| 37 | 2026-10-09 | 119.3 | 119.3 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 03:00:40", "2026-10-10 07:00:00", 239.3, 1.8]]` | Não, preservou 03:00:40–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-10-30 | 120.0 | 137.1 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 03:00:00", "2026-10-31 07:00:00", 240.0, 2.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-11-01 | 120.0 | 137.1 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 03:00:00", "2026-11-02 07:00:00", 240.0, 1.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |

## 3. `motor_agora>0` com `DiaPago=0`

**Sim, no sentido de divergência de valor frente ao recálculo da cópia.** Nos 12 casos dessa subclasse, a sonda reporta noturno legal positivo no motor atual e zero no `DiaPago` usado pelo C1. [nao verificado]

No código da cópia, a janela busca o dia da jornada atribuído ao par e coloca o marco de saída no dia seguinte quando o turno cruza a meia-noite. O motor calcula `minutos_noturnos_legais` sobre as pontas efetivas, desconta o noturno dos intervalos e aplica o fator de hora noturna, com prorrogação conforme a configuração. [`motor_calculo_v2.py:2494`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2494), [`motor_calculo_v2.py:1549`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549), [`motor_calculo_v2.py:1574`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574), [`motor_calculo_v2.py:2833`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2833), [`motor_calculo_v2.py:983`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:983)

Na lavra `versao='motor'`, o fechamento entrega os períodos e o mapa `dia_da_jornada`. `lavrar` soma `minutos_noturnos_legais / 60` em `horas_noturnas` do dia atribuído e substitui as linhas da competência para aquela versão. Se receber os mesmos períodos positivos do recálculo para o mesmo dia, a lavra desse dia sai positiva. [`fechamento.py:1201`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1201), [`fechamento.py:1217`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1217), [`fechamento.py:1227`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1227), [`dia_pago.py:173`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:359)

**A causa histórica do zero fica em aberto.** Data e versão da lavra original, períodos e insumos que ela recebeu, eventual tentativa de relavra e atribuição dos 12 casos à mudança de ancoragem da janela estão [nao verificado].

## 24/11 02:26 · sessao 7e01b5d91af0 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## (1) UNIVERSO e emp_comp

```text
UNIVERSO pares_fechados 16167 invertidos 4 sha256 848979805ef7be0fd6fce6610e41ce9e53c68dcfe3403c902b916763c17bb11d
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 2
emp_comp 2 11/2026 fechados 6337 invertidos 1
emp_comp 2 12/2026 fechados 275 invertidos 0
emp_comp 3 10/2026 fechados 1478 invertidos 1
emp_comp 3 11/2026 fechados 1408 invertidos 0
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 0
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## (2) Linhas INV

```text
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
```

## (3) Classificação e contas

**Os quatro casos são B.** Em cada linha do anexo, o dia civil da entrada coincide com o dia de jornada da coluna 5, a entrada efetiva é igual à entrada bruta e a saída foi recortada para antes de ambas as batidas. A cópia ancora os marcos no dia da jornada e grava as pontas resultantes do recorte. O limite de saída pode incorporar extra declarada. O marco cadastrado e a extra declarada, separadamente para cada par desta execução, ficam [nao verificado]. [app/ponto/motor_calculo_v2.py:1549](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549) · [app/ponto/motor_calculo_v2.py:1574](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574) · [app/ponto/janela_he.py:168](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/janela_he.py:168) · [app/ponto/motor_calculo_v2.py:1873](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1873)

| INV | Classe | Dia civil da entrada = dia de jornada | Saída efetiva < entrada bruta ≤ saída bruta | Saída efetiva − entrada efetiva |
|---|---|---|---|---|
| 109 | B | `2026-10-05` | `22:00:00 < 22:00:41 < 22:10:41` | `22:00:00 − 22:00:41 = −41 s` |
| 399 | B | `2026-09-30` | `18:30:00 < 18:30:29 = 18:30:29` | `18:30:00 − 18:30:29 = −29 s` |
| 456 | B | `2026-11-14` | `19:00:00 < 19:00:08 = 19:00:08` | `19:00:00 − 19:00:08 = −8 s` |
| 473 | B | `2026-09-21` | `19:00:00 < 19:00:18 < 19:23:54` | `19:00:00 − 19:00:18 = −18 s` |

## 24/11 02:23 · sessao e938a376abf2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

A sonda `DEPOIS` parou na inicialização do contêiner, com `rc_sombra=125`. O trecho raspado termina sem contagens de C1 e sem linhas `fech`. (`DADO RASPADO:3–6`, contando a partir da primeira linha `sonda_leitura`.)

### 1. Contagens e soma C1

| Classe | Noturno bruto | Motor agora | Contagem |
|---|---:|---:|---|
| `iguais_so_fechados` | ≤ 5 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | ≤ 5 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | > 0 | [nao verificado] |
| Demais classes, inclusive seus nomes | | | [nao verificado] |
| **Soma = C1** | | | **[nao verificado]** |

O trecho contém o erro do Docker abaixo. Traceback Python [nao verificado]. (`DADO RASPADO:3–6`)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "/home/ronald/saas-hasner/app/staticfiles" to rootfs at "/app/staticfiles": create mountpoint for /app/staticfiles mount: make mountpoint "/app/staticfiles": mkdirat /var/lib/docker/rootfs/overlayfs/ff272d7d2278fe95edf6641b5e8d4057eb88fbf061c4bd2fb5824188b871dde0/app/staticfiles: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

### 2. Exemplos `fech` e corte da faixa

Nas seis subclasses, os pares a transcrever e o resultado dos testes `efetivo_saida <= 22:00` e `efetivo_entrada >= 05:00` ficam [nao verificado]. O motor registra as pontas efetivas e usa o trecho recortado para calcular o noturno, com desconto das pausas e prorrogação conforme a configuração. [`motor_calculo_v2.py:1867`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867), [`motor_calculo_v2.py:2927`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2927), [`motor_calculo_v2.py:983`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) [nao verificado]

### 3. `motor_agora > 0` com `DiaPago = 0`

Se o positivo vier de um período que a lavratura `versao='motor'` incluiria na mesma linha de colaborador, competência e dia da jornada, o zero gravado diverge do que essa lavratura produziria hoje. `lavrar` soma `minutos_noturnos_legais / 60` dos períodos recebidos no dia atribuído e grava o resultado quando é chamada. [`fechamento.py:1200`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1200), [`fechamento.py:1217`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1217), [`dia_pago.py:173`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

A ocorrência dessa divergência na sombra, a data e a versão da lavra e a hipótese de que o zero veio de uma lavra anterior à mudança do motor ficam [nao verificado].

## 24/11 02:22 · sessao 42fa281c12fe · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma dessas linhas aparece no trecho fornecido. [nao verificado]
2. `INV` — nenhuma linha para transcrever. O trecho mostra um erro de montagem do Docker e termina com `rc_sombra=125`. A quantidade de `INV` fica [nao verificado].
3. Classes A, B e C — sem linhas `INV`, faltam os horários de cada par para fazer as contas e atribuir uma classe. [nao verificado]

## 24/11 02:22 · sessao 9d23ce02ad2a · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

### Transcrição verbatim do anexo

```text
CASO 37 2026-10-05 emp 4 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 517 2026-05-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-04 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-05 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-06 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-04 False gerada None
  CEL 2026-10-05 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-06 False gerada None
  BAT 2026-10-06 02:00:15 S app
  BAT 2026-10-06 03:00:38 E app
  BAT 2026-10-06 07:01:01 S app
CASO 113 2026-10-30 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-29 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-30 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-29 False gerada None
  CEL 2026-10-30 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-31 False gerada None
  BAT 2026-10-31 02:00:00 S disputa_s84_retro
  BAT 2026-10-31 03:00:00 E disputa_s84_retro
  BAT 2026-10-31 07:02:00 S disputa_s84_retro
CASO 113 2026-11-01 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-31 False gerada None
  CEL 2026-11-01 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 False gerada None
  BAT 2026-11-02 02:01:00 S disputa_s84_retro
  BAT 2026-11-02 03:00:00 E disputa_s84_retro
  BAT 2026-11-02 07:01:00 S disputa_s84_retro
CASO 174 2026-09-23 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1616 2026-11-16 None TE 12x36 12x36 18:00:00 06:00:00 01:00:00 02:00:00 720 he_extra 0 0
    marcos_do_dia 2026-09-22 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-23 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-24 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-09-22 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-23 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-24 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-09-22 20:58:21 E app
  BAT 2026-09-23 02:46:44 S app
  BAT 2026-09-23 03:46:54 E app
  BAT 2026-09-23 05:02:44 S app
  BAT 2026-09-24 02:32:11 E app
  BAT 2026-09-24 03:24:34 S app
  BAT 2026-09-24 05:00:58 S app
CASO 200 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1548 2026-11-10 None TE 6x1 turno_partido 15:00:00 23:00:00 18:00:00 19:00:00 420 he_extra 10 10
    marcos_do_dia 2026-11-01 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-11-01 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-11-03 02:00:00 S disputa_s84_retro
  BAT 2026-11-03 02:00:33 E app
  BAT 2026-11-03 03:02:17 S app
  BAT 2026-11-03 06:00:15 S app
  BAT 2026-11-03 22:00:47 E app
  BAT 2026-11-04 02:00:24 S app
  BAT 2026-11-04 03:00:36 E app
  BAT 2026-11-04 06:00:48 S app
CASO 218 2026-10-15 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1262 2026-09-20 None TE 6x1 turno_partido 21:00:00 05:00:00 02:00:00 03:00:00 420 he_extra 0 0
    marcos_do_dia 2026-10-14 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-15 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-16 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-10-14 False gerada None
  CEL 2026-10-15 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-16 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-10-16 02:15:08 S app
  BAT 2026-10-16 03:15:06 E app
  BAT 2026-10-16 05:00:01 S app
  BAT 2026-10-16 20:59:13 E app
  BAT 2026-10-17 02:15:42 S app
  BAT 2026-10-17 03:15:07 E app
  BAT 2026-10-17 05:00:33 S app
CASO 296 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 261 2026-07-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-11-01 False gerada None
  CEL 2026-11-02 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 False gerada None
  BAT 2026-11-03 00:15:19 E app
  BAT 2026-11-03 01:29:03 E app
  BAT 2026-11-03 07:00:41 S app
```

### Turno de D por caso

`hi` e `hf` vêm da célula de D. Quando o turno previsto cruza a meia-noite, `hf` cai em D+1. [app/escala/models.py:1135] [app/ponto/selecao_periodo.py:138] [app/escala/servico_jornada.py:111]

A lista de batidas resulta do pareador da cópia alimentado com os marcos e os tipos `E`/`S` do anexo. O pareamento efetivo na sombra, que também pode usar o papel dado pela ata, fica [nao verificado]. [app/ponto/turnos.py:1192] [app/ponto/turnos.py:1944] [app/ponto/turnos.py:3125]

- **CASO 37 · 2026-10-05** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-06: `02:00:15 S`, `03:00:38 E`, `07:01:01 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-10-30** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-31: `02:00:00 S`, `03:00:00 E`, `07:02:00 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-11-01** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-02: `02:01:00 S`, `03:00:00 E`, `07:01:00 S`. [app/ponto/turnos.py:293]
- **CASO 174 · 2026-09-23** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-09-24: `02:32:11 E`, `03:24:34 S`, `05:00:58 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]
- **CASO 200 · 2026-11-02** — `hi` 22:00 em D, `hf` 06:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `02:00:00 S`, `02:00:33 E`, `03:02:17 S`, `06:00:15 S`. [app/ponto/turnos.py:293] [app/ponto/turnos.py:1371]
- **CASO 218 · 2026-10-15** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-10-16: `02:15:08 S`, `03:15:06 E`, `05:00:01 S`. [app/ponto/turnos.py:293]
- **CASO 296 · 2026-11-02** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `00:15:19 E`, `01:29:03 E`, `07:00:41 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]

## 24/11 02:21 · sessao cebe1e5caa38 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. DEPOIS — contagem e saída da sonda

O Docker falhou ao montar `/app/logs` durante a inicialização do contêiner, antes da chamada ao Python. O trecho fornecido contém o erro abaixo. Traceback Python [nao verificado]. [bin/sonda_leitura.sh:203](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:203) · [bin/sonda_leitura.sh:235](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:235)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/2f6ba2df7015b1f000d88291ba0c235856952da9e2aea595a35b059ad3fbbb1a/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

| Classe | Noturno bruto | Motor agora `=0` | Motor agora `>0` |
|---|---:|---|---|
| `iguais_so_fechados` | ≤ 5 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 30 min | [nao verificado] | [nao verificado] |

Demais classes, total C1 e conferência da soma das classes com C1 [nao verificado].

## 2. Exemplos `fech` e corte da faixa noturna

Para cada uma das seis subclasses, os pares `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]` e o resultado do teste por par ficam [nao verificado]. O teste usa as datas completas da faixa em questão, com `efetivo_saida <= 22:00` do início da faixa ou `efetivo_entrada >= 05:00` do fim dela. O motor carimba as pontas efetivas e calcula a interseção em horário local. [app/ponto/motor_calculo_v2.py:89](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:89) · [app/ponto/motor_calculo_v2.py:1867](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867) [nao verificado]

## 3. `motor_agora > 0` com `DiaPago = 0`

**Se a comparação for para o mesmo colaborador, competência, dia de jornada e `DiaPago` de versão `motor`, com os mesmos períodos elegíveis à lavra, o zero gravado está divergente do que o motor da cópia lavraria hoje.** O motor calcula `minutos_noturnos_legais` pelas pontas efetivas, com desconto de intervalo, prorrogação conforme a configuração e aplicação do fator de hora noturna. A lavra da versão `motor` recebe os períodos e o mapa do dia de jornada, soma os minutos legais divididos por 60 e substitui as linhas daquela versão na competência. [app/ponto/motor_calculo_v2.py:983](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) · [app/ponto/motor_calculo_v2.py:2929](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2929) · [app/ponto/services/fechamento.py:1201](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1201) · [app/ponto/services/fechamento.py:1227](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1227) · [app/ponto/services/dia_pago.py:173](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173) · [app/ponto/services/dia_pago.py:359](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

Nesta execução, a ocorrência dessa divergência, a equivalência dos insumos, a data da lavra e a causa do eventual zero, inclusive se veio de uma lavra anterior à correção, ficam [nao verificado].

## 24/11 02:20 · sessao 481b8b509dc8 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
2. `INV` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
3. Classes A, B e C — sem linhas `INV`, não há horários para calcular nem casos para classificar. [nao verificado]

O trecho fornecido termina com um erro do Docker na montagem de `/app/logs` e `rc_sombra=125`. [nao verificado]

## 24/11 02:17 · sessao fc72fded9863 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

**ANTES — C1 = 402.** As 17 classes somam 402. Entre os 320 casos `iguais_so_fechados`, todos estão em `motor_agora=0`. `anexo-sonda:3–20`

As referências `anexo-sonda:N` numeram a saída fornecida a partir de `sonda_leitura`, incluindo a linha em branco. A correspondência da cópia local com o hash informado e o conteúdo do script `sonda_c1c.py` ficam [nao verificado].

### 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `anexo-sonda:4` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `anexo-sonda:5` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:6` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:7` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `anexo-sonda:8` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `anexo-sonda:9` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:10` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `anexo-sonda:11` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `anexo-sonda:12` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `anexo-sonda:13` |
| `iguais_com_aberto` | 62 | `anexo-sonda:14` |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | `anexo-sonda:15` |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | `anexo-sonda:16` |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 57 | `anexo-sonda:17` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:18` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `anexo-sonda:19` |
| `virada_ou_outro_dia` | 3 | `anexo-sonda:20` |
| **Soma = C1** | **402** | `anexo-sonda:3–20` |

O trecho fornecido termina em `rc_sombra=0`, sem traceback para transcrever. `anexo-sonda:1–125`

### 2. Exemplos `fech` e corte da faixa noturna

A comparação usa a data e a hora dos limites da faixa que o par bruto alcançou. Nos pares da tarde, o limite relevante é 22h do próprio dia. Nos pares da madrugada, 05h é do dia civil das batidas. `anexo-sonda:73–84` · [motor_calculo_v2.py:97](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:97)

| Subclasse de `iguais_so_fechados` | `colab` | `dia` | `bruto` | `leg` | `pares` | A ponta efetiva cortou a faixa? | Fonte |
|---|---:|---|---:|---:|---|---|---|
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:73` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:74` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:75` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:76` |
| `bruto<=5\|motor_agora=0` | 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva = 19:00 < 22:00 | `anexo-sonda:77` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:78` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:79` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:80` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-05 | 119.4 | 0.0 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 19:00:00", "2026-10-06 07:00:00", 0, 960.4]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:81` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-09 | 119.3 | 0.0 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 19:00:00", "2026-10-10 07:00:00", 0, 961.2]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:82` |
| `bruto>30\|motor_agora=0` | 113 | 2026-10-30 | 120.0 | 0.0 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 19:00:00", "2026-10-31 07:00:00", 0, 962.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:83` |
| `bruto>30\|motor_agora=0` | 113 | 2026-11-01 | 120.0 | 0.0 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 19:00:00", "2026-11-02 07:00:00", 0, 961.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:84` |

Nos quatro exemplos `bruto>30`, o `dia` do registro antecede a data civil da entrada bruta. No código, `_janela_do_dia` busca os marcos por `entrada.date()`, e `entrada_efetiva` ancora o marco na data da batida. Essa lógica é compatível com o par efetivo invertido mostrado no anexo. O motor devolve zero quando a entrada é igual ou posterior à saída e só soma interseções de duração positiva. O corte nos demais casos de cada subclasse fica [nao verificado]. `anexo-sonda:81–84` · [motor_calculo_v2.py:1528](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528) · [janela_he.py:20](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:20) · [janela_he.py:94](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94) · [motor_calculo_v2.py:91](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:91) · [motor_calculo_v2.py:112](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:112)

### 3. `motor_agora>0` com `DiaPago=0`

**Para a mesma linha de `DiaPago` e o mesmo conjunto de períodos que entrariam na lavra, sim, no sentido de divergência frente ao cálculo atual.** Na versão `motor`, o fechamento entrega os períodos e o mapa `dia_da_jornada` a `lavrar`. A função usa esse mapa quando há dia para o período, soma `minutos_noturnos_legais / 60` em `horas_noturnas` e grava as linhas da competência e da versão. Se o total comparável agora é positivo, uma lavra com esses períodos atribuiria horas noturnas positivas à linha hoje zerada. [fechamento.py:1201](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1201) · [fechamento.py:1217](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1217) · [dia_pago.py:126](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:126) · [dia_pago.py:173](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:173) · [dia_pago.py:224](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:224) · [dia_pago.py:359](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:359)

O anexo mostra **zero casos `motor_agora>0` entre os 320 `iguais_so_fechados`**. A presença dessa divergência nas outras classes, a data e os insumos da lavra original e o motivo de eventual defasagem ficam [nao verificado]. `anexo-sonda:4–20`

## 24/11 02:15 · sessao d29f00865ac3 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. UNIVERSO e emp_comp

Transcrição literal do dado raspado fornecido. Execução da sonda e hashes [nao verificado] de forma independente.

```text
UNIVERSO pares_fechados 16167 invertidos 25 sha256 1247d31fd41f181b3e4f14c331ac9a98cf5d99d830c1833d6ce403daaa6aff5b
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 6
emp_comp 2 11/2026 fechados 6337 invertidos 10
emp_comp 2 12/2026 fechados 275 invertidos 1
emp_comp 3 10/2026 fechados 1478 invertidos 4
emp_comp 3 11/2026 fechados 1408 invertidos 2
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 2
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## 2. Linhas INV

Transcrição literal do mesmo dado raspado.

```text
INV 1010 2 11/2026 N 2026-11-05 2026-11-05 01:04:44 2026-11-05 05:59:33 2026-11-05 18:00:00 2026-11-05 05:59:33 0.0 1015.3 0.0 0.0 INV
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 113 3 11/2026 N 2026-10-30 2026-10-31 03:00:00 2026-10-31 07:02:00 2026-10-31 19:00:00 2026-10-31 07:00:00 0.0 962.0 0.0 0.0 INV
INV 113 3 11/2026 N 2026-11-01 2026-11-02 03:00:00 2026-11-02 07:01:00 2026-11-02 19:00:00 2026-11-02 07:00:00 0.0 961.0 0.0 0.0 INV
INV 174 2 10/2026 N 2026-09-23 2026-09-24 02:32:11 2026-09-24 05:00:58 2026-09-24 21:00:00 2026-09-24 05:00:00 0.0 1108.8 0.0 0.0 INV
INV 174 2 11/2026 N 2026-10-25 2026-10-25 02:32:15 2026-10-25 04:57:35 2026-10-25 21:00:00 2026-10-25 04:57:35 0.0 1107.7 0.0 0.0 INV
INV 200 2 11/2026 N 2026-11-02 2026-11-03 02:00:33 2026-11-03 06:00:15 2026-11-03 22:00:00 2026-11-03 06:00:00 0.0 1199.7 0.0 0.0 INV
INV 218 2 10/2026 N 2026-10-15 2026-10-16 03:15:06 2026-10-16 05:00:01 2026-10-16 21:00:00 2026-10-16 05:00:00 0.0 1064.9 0.0 0.0 INV
INV 218 2 11/2026 N 2026-11-14 2026-11-15 03:17:11 2026-11-15 05:00:03 2026-11-15 21:00:00 2026-11-15 05:00:00 0.0 1062.9 0.0 0.0 INV
INV 218 2 12/2026 N 2026-11-21 2026-11-22 03:22:03 2026-11-22 05:00:16 2026-11-22 21:00:00 2026-11-22 05:00:00 0.0 1058.2 0.0 0.0 INV
INV 296 2 11/2026 N 2026-11-02 2026-11-03 00:15:19 2026-11-03 07:00:41 2026-11-03 19:00:00 2026-11-03 07:00:00 0.0 1125.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-05 2026-10-06 03:00:38 2026-10-06 07:01:01 2026-10-06 19:00:00 2026-10-06 07:00:00 0.0 960.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-09 2026-10-10 03:00:40 2026-10-10 07:01:50 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 961.2 0.0 0.0 INV
INV 382 2 10/2026 N 2026-09-30 2026-10-01 01:01:26 2026-10-01 07:50:17 2026-10-01 23:50:00 2026-10-01 07:50:00 0.0 1368.9 0.0 0.0 INV
INV 382 2 11/2026 N 2026-11-10 2026-11-11 00:02:41 2026-11-11 07:51:20 2026-11-11 23:50:00 2026-11-11 07:50:00 0.0 1428.7 0.0 0.0 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-09 2026-10-10 02:59:26 2026-10-10 07:03:44 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 964.3 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-11 2026-10-12 03:00:00 2026-10-12 07:05:26 2026-10-12 19:00:00 2026-10-12 07:00:00 0.0 965.4 0.0 0.0 INV
INV 673 2 11/2026 N 2026-10-21 2026-10-22 06:57:21 2026-10-22 07:00:00 2026-10-22 19:00:00 2026-10-22 07:00:00 0.0 722.6 0.0 0.0 INV
INV 841 2 11/2026 N 2026-11-13 2026-11-14 06:53:31 2026-11-14 07:05:54 2026-11-14 19:00:00 2026-11-14 07:00:00 0.0 732.4 0.0 0.0 INV
INV 868 3 10/2026 N 2026-10-10 2026-10-10 02:29:34 2026-10-10 06:58:56 2026-10-10 19:00:00 2026-10-10 06:58:56 0.0 990.4 0.0 0.0 INV
INV 922 2 11/2026 N 2026-10-27 2026-10-29 04:07:39 2026-10-29 07:02:13 2026-10-29 23:00:00 2026-10-29 07:00:00 0.0 1134.6 0.0 0.0 INV
INV 973 2 10/2026 N 2026-10-07 2026-10-08 06:55:11 2026-10-08 07:00:43 2026-10-08 19:00:00 2026-10-08 07:00:00 0.0 725.5 0.0 0.0 INV
```

## 3. Classificação e conta por INV

- **A\* — 18 linhas pelo limite efetivo de entrada.** A data civil da entrada difere do dia da jornada. O motor consulta o marco pela data civil da batida, e o recorte leva a entrada efetiva para depois da saída bruta em todas as 18 linhas. A regra admite `limite efetivo = marco nominal − extra declarada antes`. A igualdade com o marco nominal em cada linha fica **[nao verificado]**. [app/ponto/motor_calculo_v2.py:1528][dia] · [app/ponto/motor_calculo_v2.py:1702][entrada-motor] · [app/ponto/janela_he.py:94][entrada]
- **B — 4 linhas.** Em cada uma, `saída efetiva < entrada bruta ≤ saída bruta`, com a entrada intacta. A saída recortada fica no marco de saída ou depois dele se houver extra declarada. A desigualdade põe o par bruto inteiro depois do marco. [app/ponto/janela_he.py:143][saida] · [app/escala/models.py:83][extra-saida]
- **C — 3 linhas.** A entrada bruta está no próprio dia da jornada. O recorte desloca apenas a entrada, que passa da saída bruta. A saída efetiva permanece igual à saída bruta. [app/ponto/motor_calculo_v2.py:1760][pontas] · [app/ponto/janela_he.py:94][entrada] · [app/ponto/janela_he.py:143][saida]

Cada linha abaixo usa os horários da INV na mesma posição do bloco acima. A última coluna faz `efetivo_saida − efetivo_entrada` em `hh:mm:ss`, antes de limitar a duração trabalhada a zero. [app/ponto/motor_calculo_v2.py:2888][duracao] · [app/ponto/motor_calculo_v2.py:2893][trabalhado]

| INV · colab | Classe | Jornada → dia civil da entrada | Entrada bruta → efetiva | Saída bruta → efetiva | Saída efetiva − entrada efetiva |
|---|---|---|---|---|---:|
| 01 · 1010 | [C][pontas] | 05/11 → 05/11 | 01:04:44 → 18:00:00 | 05:59:33 → 05:59:33 | −12:00:27 |
| 02 · 109 | [B][saida] | 05/10 → 05/10 | 22:00:41 → 22:00:41 | 22:10:41 → 22:00:00 | −00:00:41 |
| 03 · 113 | [A*][entrada] | 30/10 → 31/10 | 03:00:00 → 19:00:00 | 07:02:00 → 07:00:00 | −12:00:00 |
| 04 · 113 | [A*][entrada] | 01/11 → 02/11 | 03:00:00 → 19:00:00 | 07:01:00 → 07:00:00 | −12:00:00 |
| 05 · 174 | [A*][entrada] | 23/09 → 24/09 | 02:32:11 → 21:00:00 | 05:00:58 → 05:00:00 | −16:00:00 |
| 06 · 174 | [C][pontas] | 25/10 → 25/10 | 02:32:15 → 21:00:00 | 04:57:35 → 04:57:35 | −16:02:25 |
| 07 · 200 | [A*][entrada] | 02/11 → 03/11 | 02:00:33 → 22:00:00 | 06:00:15 → 06:00:00 | −16:00:00 |
| 08 · 218 | [A*][entrada] | 15/10 → 16/10 | 03:15:06 → 21:00:00 | 05:00:01 → 05:00:00 | −16:00:00 |
| 09 · 218 | [A*][entrada] | 14/11 → 15/11 | 03:17:11 → 21:00:00 | 05:00:03 → 05:00:00 | −16:00:00 |
| 10 · 218 | [A*][entrada] | 21/11 → 22/11 | 03:22:03 → 21:00:00 | 05:00:16 → 05:00:00 | −16:00:00 |
| 11 · 296 | [A*][entrada] | 02/11 → 03/11 | 00:15:19 → 19:00:00 | 07:00:41 → 07:00:00 | −12:00:00 |
| 12 · 37 | [A*][entrada] | 05/10 → 06/10 | 03:00:38 → 19:00:00 | 07:01:01 → 07:00:00 | −12:00:00 |
| 13 · 37 | [A*][entrada] | 09/10 → 10/10 | 03:00:40 → 19:00:00 | 07:01:50 → 07:00:00 | −12:00:00 |
| 14 · 382 | [A*][entrada] | 30/09 → 01/10 | 01:01:26 → 23:50:00 | 07:50:17 → 07:50:00 | −16:00:00 |
| 15 · 382 | [A*][entrada] | 10/11 → 11/11 | 00:02:41 → 23:50:00 | 07:51:20 → 07:50:00 | −16:00:00 |
| 16 · 399 | [B][saida] | 30/09 → 30/09 | 18:30:29 → 18:30:29 | 18:30:29 → 18:30:00 | −00:00:29 |
| 17 · 456 | [B][saida] | 14/11 → 14/11 | 19:00:08 → 19:00:08 | 19:00:08 → 19:00:00 | −00:00:08 |
| 18 · 473 | [B][saida] | 21/09 → 21/09 | 19:00:18 → 19:00:18 | 19:23:54 → 19:00:00 | −00:00:18 |
| 19 · 515 | [A*][entrada] | 09/10 → 10/10 | 02:59:26 → 19:00:00 | 07:03:44 → 07:00:00 | −12:00:00 |
| 20 · 515 | [A*][entrada] | 11/10 → 12/10 | 03:00:00 → 19:00:00 | 07:05:26 → 07:00:00 | −12:00:00 |
| 21 · 673 | [A*][entrada] | 21/10 → 22/10 | 06:57:21 → 19:00:00 | 07:00:00 → 07:00:00 | −12:00:00 |
| 22 · 841 | [A*][entrada] | 13/11 → 14/11 | 06:53:31 → 19:00:00 | 07:05:54 → 07:00:00 | −12:00:00 |
| 23 · 868 | [C][pontas] | 10/10 → 10/10 | 02:29:34 → 19:00:00 | 06:58:56 → 06:58:56 | −12:01:04 |
| 24 · 922 | [A*][entrada] | 27/10 → 29/10 | 04:07:39 → 23:00:00 | 07:02:13 → 07:00:00 | −16:00:00 |
| 25 · 973 | [A*][entrada] | 07/10 → 08/10 | 06:55:11 → 19:00:00 | 07:00:43 → 07:00:00 | −12:00:00 |

[dia]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528
[entrada-motor]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1702
[entrada]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94
[saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:143
[extra-saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/escala/models.py:83
[pontas]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1760
[duracao]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2888
[trabalhado]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2893

## 24/11 02:12 · sessao e9d3f1c19b9b · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Pré-verificação da parte (1) — bloqueia.** A regra nova pode alcançar uma competência exportada, o mesmo dia pode receber impressões diferentes no cron e no julgamento avulso, e o novo `FURO_PARCIAL` pode reter a folha sem produzir uma pergunta que a cobrança saiba fazer. `app/ponto/services/cartorio.py:333–337,1188–1235,1480–1497,1916–1934,2143–2145`, `app/folha/export.py:1285–1294`

Usei o diff fornecido e o código da cópia, em leitura. Confirmação independente do SHA e do pai, execução do RED e da suíte, volumes de produção e efeito em valores ou rubricas [nao verificado]. [nao verificado]

## Achados

1. **Bloqueia — o horizonte não protege a competência exportada.** `HORIZONTE_SEM_PAR = '2026-11'` habilita toda competência de novembro de 2026 em diante pelo mês do fim da janela da empresa. Uma competência 11, 12 ou posterior com TXT vigente passa pelo mesmo teste. O cartório pode relavrar a célula porque o caminho novo não consulta `competencia_entregue` (K6). A guarda temporária existente seleciona setembro de 2026 e atua no recálculo de `FechamentoMensal`, em outra porta. O selo novo verifica datas anteriores ao piso e declara que a guarda de exportação fica fora do teste. O piso preserva o alcance para frente da **L-124**, mas não cumpre a restrição particular desta obra. A **L-113** altera a **L-092** para recálculos e preserva o TXT anterior. Essa regra geral não substitui o recorte explícito desta parte. `app/ponto/services/cartorio.py:316–337,1223–1235,1953–1977`, `app/ponto/supra_juiz.py:97–105`, `app/folha/export.py:556–589`, `app/ponto/services/fechamento.py:27–42,558–575`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:221–247`, `app/docs/LEIS.md:125,145,155`

2. **Bloqueia — a janela altera o sinal e o pulo do cron.** O cron das 06:27 chama `julgar_colab` com todas as células passadas do colaborador, em ordem, sem `--forcar`. `julgar_celula` passa `[cel]`. A segunda passada do próprio lote pode usar só as células reconciliadas. Cada corpo tira `d0,d1` das pontas da lista. O pareador monta segmentos por vínculo, lê de `ini−2` a `fim+1` e só depois filtra por `data_turno`. O contrato de `dias_encerrados` explicita que o pareamento depende da janela. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59–65`, `app/ponto/services/cartorio.py:1073–1077,1188–1229,2142–2145`, `app/ponto/turnos.py:1834–1847,1867–1871,1963–2002,2133–2139`

   Um contraexemplo por leitura estática é a E às 23:30 de D e a S às 00:30 de D+2, em vínculo diurno 08–14, sem outras batidas nem papel de pausa ou eco. A janela avulsa exclui a S e deixa a E aberta. A ampla pode fechar o par de 25h no envelope de 26h porque as duas pontas estão longe dos respectivos marcos. Depois de expirar a aberta, o avulso pode gravar `sem_par:E` e o cron omitir a parte. A S em D+2 também fica fora das batidas diretamente hasheadas para D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria ou relavrar após o avulso sem fato novo. Isso quebra a coerência e a idempotência exigidas pela **L-099**. Incidência em produção [nao verificado]. `app/ponto/turnos.py:312–340,1377–1422,1430–1454,1491–1502,2005–2035`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/services/cartorio.py:1233–1235,601–603,1435–1440,1479–1497`, `app/docs/LEIS.md:122`

3. **Bloqueia — o sinal e o realizado podem vir de pareações diferentes dentro da mesma lavra.** Encerramento e sinal compartilham uma lista, de `turnos_do_colab(c,d0,d1)`, alimentada com a ata persistida. Já a grade usada na comparação de dia cheio pareia com `ref=d1+1` e com uma cópia das lâmpadas e órfãs **desta montagem**. A grade documenta um caso em que a ata anterior mudou os minutos para as mesmas batidas. É o mesmo algoritmo registrado, chamado com duas alimentações. Nos comandos, a mistura é entre a ata lavrada e o sinal calculado agora. O diff não introduz fórmula de minutos nem outro algoritmo de pareamento, mas introduz essa segunda resposta como insumo da decisão. **L-002 e L-099.** `app/ponto/services/cartorio.py:1208–1231,1252–1259`, `app/ponto/turnos.py:1889–1892,1940–1944`, `app/escala/utils.py:1873–1909,2052–2072`, `app/escala/services/leitor_celula.py:397–424`, `app/ponto/management/commands/supra_juiz.py:120–132`, `app/ponto/management/commands/emitir_furo_retroativo.py:60–66`, `app/docs/LEIS.md:60,122`

4. **Bloqueia — a condição diverge do corte em “marcos acesos” e na destinação da espúria.** O ramo novo faz o seguinte. `app/ponto/supra_juiz.py:253–259,290–314,340–341,493–512`

   | Cláusula | Implementação e divergência |
   |---|---|
   | Dia fechado | Usa o veredito de `dias_encerrados`, que consulta turno aberto vivo e compara o instante com o fim previsto. O cartório zera E/S enquanto esse juiz não fecha o dia, inclusive quando o vigia passa `turno_encerrado=True` para julgar só até o marco vencido. O ramo novo atende ao teto da **L-073**, sujeito à alimentação descrita acima. `app/ponto/turnos.py:2121–2130,2166–2182`, `app/ponto/services/cartorio.py:1223–1231,1560–1564`, `app/docs/LEIS.md:100` |
   | E sem par com marcos acesos | Aceita a E inclusive com realizado zero, como pede o literal. Porém `not _n_missing` só conta dicionários com `missing` verdadeiro. Lista vazia ou coluna `None` passa. A montagem da ata pode lavrar `acesa=False` ou `None` para um marco do DNA nessas condições. Há um segundo predicado para a mesma resposta que a lâmpada já expressa. **L-061 e L-002.** `app/ponto/supra_juiz.py:340–341,504–512`, `app/ponto/services/cartorio.py:685–728`, `app/docs/LEIS.md:60,95` |
   | S órfã com realizado positivo | Usa a S do pareador e exige `real > 0`. A condição de zero `missing` também foi aplicada à S, embora o literal associe “marcos acesos” à E. Os ramos antigos ainda podem acusar quando há `missing`. O novo exige trabalho e previsto não zero, exclui isento, sem âncora, justificado e qualquer cobertura aprovada, inclusive parcial. Se passar com `sem_vinculo`, emite código de cadastro. `app/ponto/supra_juiz.py:253–259,290–304,425–485,504–512` |
   | Dia cheio e espúria | `real < prev` exclui igualdade e excedente do ramo novo, em minutos do dicionário da grade. Os ramos antigos de marco apagado continuam aplicáveis. Para a E residual, o ramo apenas suprime o furo novo. A pergunta de espúria está em `SEM_JUIZ` e o fluxo existente de cluster exige ao menos duas órfãs e uma lâmpada apagada. A E única que sobra em dia cheio não é encaminhada por esse fluxo. O exemplo 726/720 consta como relato e fixture, sem confirmação independente da lavra de col63 [nao verificado]. `app/ponto/supra_juiz.py:257–259,451–461,499–512`, `app/core/juizes.py:1223–1225`, `app/ponto/services/cluster_espurio.py:15–36`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:57–64`, `app/docs/RELATO.md:1` |
   | Só competência não exportada | Falta a consulta do estado de entrega, conforme achado 1. `app/ponto/services/cartorio.py:333–337`, `app/folha/export.py:556–589` |

5. **Bloqueia — `FURO_PARCIAL` adquire dois significados que os leitores tratam como um só.** Antes identificava marco apagado com fato. Agora também identifica um resíduo do pareador quando a grade não tem `missing`. Ambos têm precedência para `veredito='furo'`, no mesmo escritor. A **L-088** liga furo a marco não cumprido e pergunta em aberto. O corte novo manda acusar na lavra, mas os seguintes consumidores continuam operando pelo contrato anterior. `app/ponto/supra_juiz.py:451–485,504–512`, `app/escala/models.py:1622–1642`, `app/ponto/portas/celula.py:1096–1105,1126–1134`, `app/docs/LEIS.md:120`

   - **Chamados e cobrança.** O emissor só abre ou espelha cobrança se achar marco apagado nomeável na ata. A fábrica também precisa de motivo de marco. Com as lâmpadas realmente acesas, o novo protesto pode alimentar o contador de furos cobrados e o reprocessamento, que ainda pode devolver `sem_cobranca_depois`. C6 e o reconciliador passam a ler um veredito acusador nos caminhos de perguntas existentes, sujeitos às suas guardas. `app/ponto/services/cartorio.py:1916–1943,2032–2046`, `app/chamados/services/fabrica_da_celula.py:52–74,119–139`, `app/ponto/services/furos_sem_cobranca.py:35,53–63,82–108,117–131`, `app/chamados/juizes.py:1539–1562`, `app/chamados/reconciliador.py:928–963,979–1004`
   - **Folha.** A retenção lê o código e o veredito gravados. Um dia que passe de `concorde` a `furo`, com todos os marcos acesos, pode virar `espelho_furo` e deixar o colaborador fora do TXT. A exceção da **L-033** exige ao menos uma pausa apagada e o conjunto exato `{'FURO_PARCIAL'}`. Com todas acesas, reter demonstra a mudança de elegibilidade, sem demonstrar infração à cláusula *só pausa*. Prévia e lista de fora da folha também leem o resultado. Valores, rubricas e quantidade de retidos em produção [nao verificado]. `app/folha/export.py:968–973,1148–1155,1173,1273–1294`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/docs/LEIS.md:81`
   - **Status, tela e Pauta DP.** O status lavrado passa de `ok` a `irregular` quando o `furo` vence, depois da guarda temporal. Calendário, relatório, painel e app leem o novo estado. A palavra `Em aberto` do calendário ainda vem da lista de E/S do pareador e pode persistir em dia cheio com status `ok`. O novo furo também pode vencer `REALIZADO_ZERO_COM_TURNO` e tirar o dia da Pauta DP dos `discordantes`, embora não haja marco para cobrar. A igualdade entre palavra e status no caso cheio continua pendente pela **L-061/L-099**. `app/ponto/status_dia.py:65–70,154–162,273–289`, `app/colaboradores/services/calendario.py:595–616,748–763`, `app/ponto/services/dia_decidido.py:346–355,419–430`, `app/relatorios/furos.py:119–127`, `app/core/services/painel_op.py:325–336`, `app/api/views.py:1493–1514`, `app/ponto/supra_juiz.py:316–319,504–512`, `app/ponto/services/pautas_dos_discordantes.py:27–31,39–41,76–81,196–213`, `app/docs/LEIS.md:95,122`

6. **Bloqueia — os selos mordem o ramo simples, mas deixam as fronteiras acima sem prova.** Os casos puros de E e S exigem `FURO_PARCIAL`, o de hash distingue `''`, `E`, `S` e `ES`, e o teste avulso morde o pulo com e sem sinal na mesma janela. A comparação entre `sem_par=''` e o default só compara duas chamadas ao código novo. Os casos integrados positivos verificam o sinal bruto e `out['cods']`, sem afirmar a ata persistida com todos os marcos acesos nem o realizado usado. O caso cheio integrado só verifica ausência do código. Faltam ao selo a paridade cron/avulso, a competência **dentro** do horizonte com exportação vigente e o caminho folha/cobrança. A atualização do censo de linhas mantém a catraca de sítios, sem testar essas propriedades. **L-110 e Lei-Akita 5.** RED efetivamente rodado contra o pai e suíte inteira [nao verificado]. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,155–247,255–263`, `app/core/tests/test_medida_turno_do_par.py:291,297,319`, `app/docs/LEIS.md:136`, `CLAUDE.md:28–29,383–392`

7. **Fila — relógio, ordem de trava, pouso e fallbacks.** O comando `supra_juiz` já omitia `agora`, mas a extração muda o instante da leitura interna para **depois** da carga do pareador. O retroativo injeta o instante da borda. Ambos ainda consultam `localdate()` por linha. O cartório injeta `agora` no encerramento, e o avulso conserva as leituras pré-existentes de `localdate()` e `now()` em sua guarda. É desvio do padrão de borda da **L-128**, distinto de usar data civil como teto do ramo novo. `app/ponto/management/commands/supra_juiz.py:120–124,235–237`, `app/ponto/management/commands/emitir_furo_retroativo.py:47,63–66,113–115`, `app/ponto/turnos.py:2148–2163`, `app/ponto/services/cartorio.py:1216–1221,2119–2145`, `app/docs/LEIS.md:150`, `app/docs/PLANO-PADROES.md:97–103`

   - **Escritor, trava e `except`.** O diff acrescenta leituras e código de saída, preserva as portas `lavrar_veredito` e `carimbar` e não introduz `except` nem aquisição nova de trava. O save e seu evento seguem juntos na porta existente. Há sobreposição com o juiz registrado de **marco faltante**, pela aproximação de “acesos” e pelo código reutilizado. O helper em si agrega a saída do pareador já registrado. `app/ponto/services/cartorio.py:1208–1235,1953–1977`, `app/ponto/portas/celula.py:993–1001,1217–1237`, `app/core/juizes.py:193–207,216–248`, `app/escala/servico_jornada.py:452–489`, `app/ponto/supra_juiz.py:86–105,340–341,504–512`
   - **Fallback e estados comprimidos.** Os kwargs são obrigatórios. O mapa ausente e `(False, False)` colapsam ausência de sinal e exclusão pelo horizonte, e o cartório também usa o par falso para dia em curso. `turnos or ()`, `getattr(t, 'aberto', False)` e o `getattr` da empresa ainda convertem carga ou atributo ausente em vazio/falso ou corte padrão. Os caminhos de produção examinados fornecem a lista e o modelo exige empresa. Ocorrência de entrada malformada [nao verificado]. `app/ponto/supra_juiz.py:97–105,239–244`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/janelas.py:36–51`, `app/colaboradores/models.py:321`
   - **Pré-requisito.** O corte de 14:16 pôs PAR-PELO-TURNO à frente. O aval de 23:55 autorizou construção em raias paralelas e pousos separados. A cópia ainda usa o teto previsto+4h que o V6 manda retirar do corte do par. O commit em cópia não comprova pouso antecipado. Ordem efetiva, reconstrução das projeções pela **L-121** e DIFF/aval de impacto aplicável pela **L-094** [nao verificado]. `app/docs/CORTES.md:629–630`, `app/docs/PROMPTS.md:2852,2863`, `app/ponto/turnos.py:1423–1454`, `app/docs/LEIS.md:112,142`
   - **L-102/L-103.** O ramo lê minutos e acrescenta protesto. A regra de apresentação para dia inteiro sem turno pareado e a escrita explícita de zero continuam nas respectivas fontes. Uma E ou S residual pode coexistir com pares fechados. `app/docs/LEIS.md:128–129`, `app/escala/utils.py:2052–2072`, `app/ponto/services/dia_decidido.py:410–418`, `app/ponto/supra_juiz.py:257–259,504–512`

## Censo de origem, leitores e chamadores

| Valor | Quem produz e quem lê |
|---|---|
| `entrada_sem_par`, `saida_orfa` | `_mk` grava `Turno.aberto` quando há entrada e falta saída. `datas_saida_orfa` lê `Turno` com saída presente e entrada ausente. `sem_par_do_pareador` agrega por `data_turno` em `{date: (bool E, bool S)}` e filtra pelo horizonte. `classificar_dia` recebe os dois kwargs e os lê no ramo novo. O calendário lê os sinais de origem em sua própria carga. `app/ponto/turnos.py:234–245,312–324`, `app/ponto/supra_juiz.py:86–105,239–244,504–512`, `app/colaboradores/services/calendario.py:602–616` |
| `HORIZONTE_SEM_PAR` | Constante de módulo `2026-11`, validada no import. `sem_par_no_horizonte` lê a competência do dia do turno por `janela_atual`, pelo corte da empresa, e o helper consome esse resultado. Os testes usam também `empresa=None`, com corte padrão 21. `app/ponto/services/cartorio.py:316–337`, `app/ponto/janelas.py:24–33,36–51,60–63`, `app/ponto/supra_juiz.py:97–105`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:108–123,236–243` |
| `sem_par:` e `CelulaDia.impressao` | `_sem_par_da_impressao` serializa `''`, `E`, `S` ou `ES`, já com o filtro de encerramento. `impressao_insumos` inclui o texto quando preenchido e devolve SHA-256 em hexadecimal. A porta `carimbar` persiste o digest. Os dois ramos do cartório comparam o digest para pular, e `celula_veredito_velho` o copia para a fotografia de reversão. O ramo sem adesão omite `sem_par` na impressão e só entrega os kwargs ao classificador depois do pulo. O ramo normal pode trocar a impressão até em dia cheio ou coberto, pois calcula o hash antes da decisão. `app/ponto/services/cartorio.py:513–514,598–603,1223–1235,1283–1292,1337–1355,1480–1500`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110` |
| Previsto, realizado, marcos e encerramento | O cartório usa a grade nova. Ela materializa minutos líquidos do dia pelos marcos e pausas, e usa `realizado_do_dia` sobre os turnos da montagem. Os dois comandos usam `grade_da_celula`, que transporta minutos da ata e consulta o juiz de previsto quando falta o campo. A grade também fornece `celulas`, `tipo_dia` e `justificado`, e o cartório escreve a ata pela porta de lavra. O encerramento vem do juiz registrado `dia_encerrado`, pela carga de janela. `app/ponto/services/cartorio.py:1216–1221,1252–1259,777–815,1953–1977`, `app/escala/utils.py:1620–1650,1818–1822,2052–2081,2241–2245`, `app/escala/services/leitor_celula.py:397–440`, `app/core/juizes.py:193–202,243`, `app/ponto/turnos.py:2121–2182` |

**Chamadores de produção.** `classificar_dia` tem quatro sítios diretos. Cartório sem adesão e normal passam o par filtrado pelo encerramento. `supra_juiz` e `emitir_furo_retroativo` passam E/S após o horizonte, junto do encerramento em outro kwarg. Naqueles quatro sítios a chave é dia do turno, os sinais são booleanos e a referência de tempo é o fuso Django ativo, cujo padrão é `America/Sao_Paulo`. A S de madrugada pode pertencer à véspera. Há uma diferença pré-existente no insumo vizinho `batidas_no_dia`, que os dois comandos contam por data civil e o cartório por células/órfãs do dia-turno. No comando `supra_juiz`, o código novo alimenta placar e exemplos, e a retratação lê a lavra. O retroativo só emite mediante `FURO_SEM_COBRANCA`. `impressao_insumos` tem os dois sítios do cartório, com reexportação pelo comando do cron. `app/ponto/services/cartorio.py:1337–1355,1480–1486,1501–1508,1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–255`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–117`, `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/turnos.py:43–48,285–324`, `app/config/settings/base.py:93`

**Chamadores diretos em testes.** As linhas abaixo identificam os sítios. Os testes antigos do classificador injetam `False, False`, inclusive em fixtures com órfãs da grade, que são outro conceito. O teste novo injeta E/S sintéticos e omite os kwargs deliberadamente no teste de `TypeError`. Para a impressão, os testes antigos usam o default e o teste novo exercita os quatro valores. Os proxies e spies transportam os kwargs ou substituem a carga, de modo que os testes sintéticos não comprovam paridade de janela, data ou fuso entre as portas reais. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,255–263`, `app/ponto/tests/test_supra_juiz_orfa_builder.py:15–27`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:145–159,191–212,271,337–382,404–409`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:135–181`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:208–215`

- `classificar_dia` em `app/escala/tests/` — `test_cobertura_parcial_periodo.py:118`, `test_hx_numero_honesto.py:79,88,99,108,124,138,150`, `test_montador_realizado_pela_autoridade.py:173`.
- `classificar_dia` em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:43,102`, `test_bo_inss_nunca_bateu_le_a_precedencia.py:273`, `test_bug_lastro_vai_e_volta.py:26,41`, `test_cobparc8_leitor_transporta_justificado.py:80`, `test_cobparc_marco_no_periodo.py:99,170,281,293,489`, `test_concorde_sem_entrada.py:31`, `test_contagem_unica.py:47,51,61,65,74,85,142`, `test_f1c4_3_supra_indefinido_nao_retrata.py:24`, `test_f1c4_3_supra_pergunta_ao_juiz.py:32`, `test_fnd_pergunta_a_juiza.py:86,104,189`, `test_grade_da_celula_cobertura_parcial.py:45`, `test_hx_cobranca_morta.py:23`, `test_supra_juiz.py:26,35,42,55,62,69,77,94,103,110,130,137,144,159,163,169,184,185,215,222,235,245,251`, `test_supra_juiz_cobranca.py:21,70,96`, `test_supra_juiz_fase.py:22`, `test_supra_juiz_indefinido.py:19`, `test_supra_juiz_inflado.py:23,28,33,38,43`, `test_supra_juiz_orfa.py:28,31,34,37,40,43`, `test_supra_juiz_orfa_builder.py:18,20,27`, `test_supra_juiz_teto.py:15,28`.
- `impressao_insumos` em `app/escala/tests/test_cobertura_parcial_periodo.py:205–209` e em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:257,261–262`, `test_cartorio.py:16–35`, `test_celula_concorde_meio_turno.py:56–57,86–88`, `test_e3_isento_na_impressao.py:49–55,81–100`, `test_o146_extra_decla

[truncado em 20000 de 20290 caracteres]

## 24/11 02:10 · sessao 5a41b3c84321 · - · - · origem=Ronald (tmux)

**Bloqueia.** O mesmo colaborador-dia recebe sinais de pareamento produzidos com janelas distintas. O cron entrega toda a série de células passadas, o avulso entrega `[cel]`, e o segundo passe do cron entrega somente as reconciliadas. O pareador lê a janela com `ini−2` e `fim+1`, segmenta conforme o vínculo e filtra os resultados por `data_turno`. O sinal entra no hash só quando presente. Quando os sinais divergem, o mesmo dia pode pular no cron ou ser relavrado com outra impressão. Casos e volume em produção [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:1073–1077`, `app/ponto/services/cartorio.py:1188–1235`, `app/ponto/services/cartorio.py:1480–1497`, `app/ponto/services/cartorio.py:2142–2145`, `app/ponto/turnos.py:1834–1847`, `app/ponto/turnos.py:1961–2002`.

**Bloqueia.** O sinal lê geometria alimentada por atas gravadas. O realizado do cartório lê geometria alimentada por lâmpadas e órfãs da montagem corrente, em outra janela. Os dois caminhos usam o mesmo pareador, com entradas diferentes. Os comandos combinam o pareamento atual com minutos da última ata lavrada. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1252–1259`, `app/ponto/turnos.py:1889–1892`, `app/ponto/turnos.py:1942–1944`, `app/escala/utils.py:1889–1909`, `app/escala/utils.py:2052–2072`, `app/escala/services/leitor_celula.py:397–424`.

**Bloqueia.** `not _n_missing` aceita ausência de células, posições `None` e dispensas, porque a contagem só considera dicionários com `missing` verdadeiro. O ramo exige essa condição também para S órfã, além de trabalho, previsão positiva na prática, falta de cobertura aprovada, falta de justificativa e âncora. Nas coberturas parciais, o novo ramo fecha a porta que a classificação anterior admite para o resto do dia. `app/ponto/supra_juiz.py:339–347`, `app/ponto/supra_juiz.py:472–485`, `app/ponto/supra_juiz.py:504–512`.

**Censo.** `Turno.aberto` é escrito pelo pareador, a lista de S órfãs lê o `Turno` com entrada ausente e saída presente, e `sem_par_do_pareador` agrega ambos em `date: (bool E, bool S)`. É dia-turno atribuído no fuso local Django, inclusive véspera no noturno, e os minutos são inteiros. As quatro chamadas de produção usam essa semântica, mas o cartório suprime os dois flags antes da chamada se o turno está em curso, enquanto os comandos passam o resultado bruto pós-horizonte com `turno_encerrado` separado. As contagens de `batidas_no_dia` que os acompanham diferem, dia civil nos comandos e dia-turno no cartório. `app/ponto/turnos.py:43–48`, `app/ponto/turnos.py:234–245`, `app/ponto/turnos.py:285–324`, `app/config/settings/base.py:93`, `app/ponto/supra_juiz.py:88–105`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/services/cartorio.py:1349–1355`, `app/ponto/services/cartorio.py:1501–1508`, `app/ponto/services/cartorio.py:1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–239`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–116`.

**Censo.** As duas chamadas de produção de `impressao_insumos` estão no cartório. A normal passa `E/S/ES` ou `''`. A sem adesão usa o default `''` no hash, embora entregue flags ao classificador se passar da guarda, e vem de ausência de primeira batida apurável. O token entra no SHA-256 e só o digest é persistido, por `carimbar`. O comando de snapshot lê o digest. `HORIZONTE_SEM_PAR` é constante validada `2026-11`, com único leitor funcional na comparação da competência pelo fim da janela da empresa. Censo de testes com todas as linhas enviado à coordenação. `app/ponto/services/cartorio.py:316–337`, `app/ponto/services/cartorio.py:513–514`, `app/ponto/services/cartorio.py:598–603`, `app/ponto/services/cartorio.py:1283–1285`, `app/ponto/services/cartorio.py:1337–1355`, `app/ponto/services/cartorio.py:1480–1486`, `app/ponto/services/cartorio.py:1976–1977`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110`, `app/ponto/supra_juiz.py:105`, `app/ponto/janelas.py:24–33`, `app/ponto/janelas.py:60–63`.

## 24/11 02:09 · sessao afd447896a19 · - · - · origem=Ronald (tmux)

1. **Bloqueia — a janela muda o sinal e a decisão de pular.** O cron real roda sem `--forcar` e passa todas as células passadas. O avulso passa `[cel]`. A segunda passada do mesmo ato pode usar só as células reconciliadas. Cada corpo busca os turnos entre as pontas da sua lista, alimentando encerramento e sem-par com essa mesma carga. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:2143`.

   A janela do pareador chega a `fim+1`. Por leitura estática, com vínculo diurno 08–14, uma E em D às 23:30 e uma S em D+2 às 00:30, sem outras batidas nem papel de intervalo ou eco, dão `E` em `[D,D]`. Uma janela ampla do mesmo vínculo inclui a S e admite fechar em 25h pelo envelope de 26h, pois ambas as pontas estão a mais de três horas dos respectivos marcos. Depois da expiração, o avulso pode imprimir `sem_par:E` e o cron imprimir sem esse componente. A S em D+2 também fica de fora das batidas diretamente hasheadas de D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria, ou relavrar depois do avulso sem fato novo. Incidência real [nao verificado]. `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1977`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:2005`, `app/ponto/services/cartorio.py:1439`, `app/ponto/services/cartorio.py:1480`, `app/ponto/services/cartorio.py:1491`, `app/docs/LEIS.md:122` (L-099).

2. **Bloqueia — duas respostas do pareador na mesma lavra.** O sinal/encerramento lê `turnos_do_colab(c,d0,d1)` com a ata persistida. O realizado da grade compara o mesmo dia usando `turnos_do_colab` com janela que estende `ref` a `d1+1` e cópia em memória da ata desta montagem. A própria grade explica por que ler papel velho já produziu minuto diferente para as mesmas batidas. É a mesma autoridade com alimentações distintas. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1253`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1944`, `app/escala/utils.py:1873`, `app/escala/utils.py:1889`, `app/escala/utils.py:1909`, `app/escala/utils.py:2056`.

3. **Bloqueia — o horizonte permite a nova regra em exportada.** O piso `2026-11` compara a competência derivada do corte da empresa. Uma 11, 12 ou posterior com TXT vigente passa pelo piso. O juiz de entrega K6 existe. A guarda temporária da 09 age no recálculo de `FechamentoMensal`/`DiaPago`, sem proteger o cartório. O teste antigo da 09 distingue ata relavrável de FM/TXT intactos. O selo novo declara que não prova a guarda da exportada. L-092 e L-113 coexistem com o recorte particular da obra, e a regra para frente está em L-124. `app/ponto/services/cartorio.py:316`, `app/ponto/services/cartorio.py:333`, `app/ponto/supra_juiz.py:105`, `app/folha/export.py:556`, `app/ponto/services/fechamento.py:27`, `app/ponto/services/fechamento.py:558`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:113`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:230`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:145`, `app/docs/LEIS.md:155`.

4. **Bloqueia — o protesto novo pode parar folha e entrar na fila de cobrança sem marco cobrável.** O emissor parcial precisa de marco apagado nomeado. A fila fora do diff inclui `FURO_PARCIAL` no universo cobravel e força relavra avulsa, que pode devolver `sem_cobranca_depois`. A pauta existente para os dias sem fio exige `discordante`, enquanto a nova lavra classifica `furo`. A folha retém `espelho_furo` sob as guardas usuais, pois a exceção L-033 exige pelo menos uma pausa apagada. Incidência real [nao verificado]. `app/ponto/services/cartorio.py:1916`, `app/ponto/services/cartorio.py:2032`, `app/ponto/services/furos_sem_cobranca.py:35`, `app/ponto/services/furos_sem_cobranca.py:82`, `app/ponto/services/furos_sem_cobranca.py:117`, `app/ponto/services/pautas_dos_discordantes.py:27`, `app/ponto/services/pautas_dos_discordantes.py:40`, `app/escala/models.py:1623`, `app/folha/export.py:1150`, `app/folha/export.py:1293`, `app/docs/LEIS.md:81`.

5. **Fila — relógio e ordem.** O comando `supra_juiz` conserva a leitura implícita de `now()` dentro de `dias_encerrados`, agora após a carga explicitada pelo diff. Retroativo passa `agora` da borda, mas ambos leem `localdate()` por linha. No cartório, o novo ramo segue o encerramento por fato/instante conforme L-073. A fila de 14:16 punha PAR-PELO-TURNO à frente. O aval posterior de 23:55 autoriza construção paralela e pousos separados. O V6 prevê mudar o teto previsto+4h, que ainda rege esta cópia. Ordem efetiva de pouso [nao verificado]. `app/ponto/management/commands/supra_juiz.py:122`, `app/ponto/management/commands/supra_juiz.py:235`, `app/ponto/management/commands/emitir_furo_retroativo.py:47`, `app/ponto/management/commands/emitir_furo_retroativo.py:113`, `app/ponto/turnos.py:2121`, `app/ponto/turnos.py:2148`, `app/docs/LEIS.md:100`, `app/docs/LEIS.md:150`, `app/docs/CORTES.md:630`, `app/docs/PROMPTS.md:2852`, `app/docs/PROMPTS.md:2863`.

## 24/11 02:09 · sessao 53ef3fdd98b9 · - · - · origem=Ronald (tmux)

1. **Bloqueia — retenção sem cobrança correspondente.** O novo `FURO_PARCIAL` passa a representar tanto marco apagado quanto despareamento sem `missing`. Ambos lavram `furo` pela mesma porta. A folha retém o segundo por `espelho_furo`, pois sua exceção exige uma pausa apagada. A emissão direta e a fábrica de perguntas, por sua vez, precisam de marco apagado nomeável. O vigia inclui o novo caso entre os furos cobrados e pode apurá-lo como `sem_cobranca_depois` mesmo após rejulgamento. A mudança afeta a elegibilidade do TXT, prévia e lista de retenção. Impacto em produção [nao verificado]. `app/ponto/supra_juiz.py:451–461,504–512`, `app/escala/models.py:1622–1627`, `app/ponto/services/cartorio.py:1916–1934,1953–1977,2032–2041`, `app/ponto/portas/celula.py:1096–1105,1126–1134`, `app/folha/export.py:968–972,1148–1155,1173,1285–1294`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/chamados/services/fabrica_da_celula.py:52–74`, `app/ponto/services/furos_sem_cobranca.py:35,53–62,82–108,117–131`.

2. **Bloqueia — `missing == 0` não equivale à lâmpada acesa.** A condição aceita coleção ausente/vazia e ignora entradas `None`. A própria montagem da ata pode lavrar `acesa=False` ou `None` se falta coluna para o marco do DNA. Esse caminho também permite que o novo código interaja com a L-033 apesar de a condição aparentar excluir todo marco apagado. Para todas as lâmpadas realmente acesas, retenção é comprovável, mas esse caso não satisfaz a premissa da exceção L-033. A mudança abre incompatibilidade com a L-088, que liga furo a marco não cumprido e pergunta em aberto, embora o corte específico mande a lavra acusar o despareamento. `app/ponto/supra_juiz.py:340–341,504–512`, `app/ponto/services/cartorio.py:685–728`, `app/docs/LEIS.md:81,120`.

3. **Bloqueia — os leitores existentes reagem também sem nova pergunta.** O C6 lê um veredito acusador como vida nos degraus indicados, e c2 admite o código persistido como condição de emissão de uma pergunta elegível já viva, submetida aos demais gates. `REALIZADO_ZERO_COM_TURNO` pode coexistir com o novo furo no próprio fixture E com zero, e `furo` tem precedência sobre `discordante`. Assim, o dia pode sair da Pauta DP dos discordantes e a pauta existente pode ser fechada sem que o emissor de marco tenha aberto uma cobrança. `app/chamados/juizes.py:1539–1562`, `app/chamados/reconciliador.py:928–963,979–1004`, `app/ponto/supra_juiz.py:316–319,504–512`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–36,51–53`, `app/escala/models.py:1622–1627`, `app/ponto/services/pautas_dos_discordantes.py:27–31,39,76–81,196–213`.

4. **Fila — status, calendários e contadores.** Trabalho com `furo` vira `irregular` no D4 após o corte do relógio. Calendário, relatório de furos, painel e app passam a mostrar o novo estado, e o app traduz `irregular` em `alerta`. A palavra lateral do calendário ainda usa turno aberto/S órfã e pode dizer `Em aberto` no dia cheio mesmo se o D4 lavrou `ok`. L-002/L-061/L-099 amparam a divergência residual quando se pede a mesma resposta. Em dia insuficiente, `irregular` e `Em aberto` podem coexistir coerentemente. O C4 do afastado também lê o novo conjunto de protestos. `app/ponto/status_dia.py:30–32,68,79–91,154–161,273–289`, `app/colaboradores/services/calendario.py:595–616,635–640,748–763`, `app/ponto/services/dia_decidido.py:346–355,419–430`, `app/templates/colaboradores/partials/_calendario_grade.html:68,98–104`, `app/relatorios/furos.py:119–127`, `app/core/services/painel_op.py:325–336`, `app/api/views.py:1493–1514`, `app/escala/services/cadastro_realidade.py:591–623`, `app/docs/LEIS.md:60,95,122`.

5. **Bloqueia — ordem e espúria.** O corte vigente coloca PAR-PELO-TURNO antes da A-TELA. A mesma cópia ainda traz teto fixo de 240 minutos em caminhos do pareador, enquanto o corte PAR-PELO-TURNO prevê cadastro por empresa e permite o teto antigo provisoriamente. Pouso anterior ou autorização posterior [nao verificado]. A exceção do cheio somente cala o novo ramo. A pergunta de espúrias consta em `SEM_JUIZ` e o fluxo de cluster exige ao menos duas órfãs e uma lâmpada apagada, portanto não trata pela regra demonstrada a E única do dia cheio com marcos acesos. `app/docs/CORTES.md:629–630`, `app/ponto/turnos.py:602,731,768`, `app/ponto/supra_juiz.py:499–512`, `app/core/juizes.py:1223–1225`, `app/ponto/services/cluster_espurio.py:15–36`.

6. **Fila — alcance das demais leis e dos comandos.** L-039 preservada no status, que distingue `irregular` de falta. L-102/L-103 tratam de dia sem turno/valor pareado e o diff lê os minutos da grade. A prova de reconstrução de projeções do pouso, L-121, e de impacto em dinheiro pela L-094 e 7b [nao verificado]. `emitir_furo_retroativo` segue emitindo apenas em `FURO_SEM_COBRANCA`; `supra_juiz` usa o código novo para placar/exemplos, não no `--alarme` nem para decidir a escrita da retratação, que lê a lavra. `app/docs/LEIS.md:87,112,128–129,142`, `CLAUDE.md:413–429`, `app/ponto/status_dia.py:139–160`, `app/ponto/supra_juiz.py:257–259,504`, `app/ponto/management/commands/emitir_furo_retroativo.py:104–117`, `app/ponto/management/commands/supra_juiz.py:225–249,251–292,309–315`.

## 24/11 01:35 · sessao 50bc8a92b541 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

`sonda/anexos` conta as linhas da saída atual a partir de `sonda_leitura`, incluindo a linha em branco. `sonda_inv.r` conta as linhas do anexo anterior a partir do primeiro título.

**UNIVERSO** `pares_fechados 16316 invertidos 25 colabs 18 min_brutos 4370`. `sonda/anexos:3`

As linhas `COLAPSADO` registram os fechados com pontas efetivas iguais. O bloqueio do dia sem previsto produz essa igualdade. `sonda/anexos:4–6` · `app/ponto/motor_calculo_v2.py:1471–1483`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|:---|:---|---:|---:|---:|---:|:---|
| COLAPSADO | 10/2026 | 57 | 0 | 0 | 0 | `sonda/anexos:4` |
| COLAPSADO | 11/2026 | 77 | 0 | 0 | 0 | `sonda/anexos:5` |
| COLAPSADO | 12/2026 | 15 | 0 | 0 | 0 | `sonda/anexos:6` |
| 1 | 10/2026 | 12 | 0 | 0 | 0 | `sonda/anexos:7` |
| 1 | 11/2026 | 14 | 0 | 0 | 0 | `sonda/anexos:8` |
| 2 | 10/2026 | 6038 | 6 | 6 | 692 | `sonda/anexos:9` |
| 2 | 11/2026 | 6337 | 10 | 10 | 1846 | `sonda/anexos:10` |
| 2 | 12/2026 | 275 | 1 | 1 | 98 | `sonda/anexos:11` |
| 3 | 10/2026 | 1478 | 4 | 3 | 769 | `sonda/anexos:12` |
| 3 | 11/2026 | 1408 | 2 | 1 | 483 | `sonda/anexos:13` |
| 3 | 12/2026 | 61 | 0 | 0 | 0 | `sonda/anexos:14` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:15` |
| 4 | 11/2026 | 258 | 0 | 0 | 0 | `sonda/anexos:16` |
| 4 | 12/2026 | 10 | 0 | 0 | 0 | `sonda/anexos:17` |

A saída fornecida termina em `rc_sombra=0`, sem traceback no trecho. `sonda/anexos:1–30`

## 2. Exemplos e ramo pelas pontas

A classificação indica quais pontas mudaram pela janela de HE. O motor aplica os recortes de entrada e saída separadamente e grava as pontas resultantes. `app/ponto/motor_calculo_v2.py:1691–1712,1731–1763,1834–1842`

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:18` · `sonda_inv.r:27` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:19` · `sonda_inv.r:28` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:20` · `sonda_inv.r:34` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:21` · `sonda_inv.r:36` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:22` · `sonda_inv.r:37` |
| 174 | 10/2026 | `2026-09-24 02:32:11` | `2026-09-24 05:00:58` | `2026-09-24 21:00:00` | `2026-09-24 05:00:00` | 0 | 1108.8 | 0.0 | HE nas duas pontas | `sonda/anexos:23` |
| 174 | 11/2026 | `2026-10-25 02:32:15` | `2026-10-25 04:57:35` | `2026-10-25 21:00:00` | `2026-10-25 04:57:35` | 0 | 1107.7 | 0.0 | HE só na entrada | `sonda/anexos:24` |
| 200 | 11/2026 | `2026-11-03 02:00:33` | `2026-11-03 06:00:15` | `2026-11-03 22:00:00` | `2026-11-03 06:00:00` | 0 | 1199.7 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 218 | 10/2026 | `2026-10-16 03:15:06` | `2026-10-16 05:00:01` | `2026-10-16 21:00:00` | `2026-10-16 05:00:00` | 0 | 1064.9 | 0.0 | HE nas duas pontas | `sonda/anexos:26` |
| 218 | 11/2026 | `2026-11-15 03:17:11` | `2026-11-15 05:00:03` | `2026-11-15 21:00:00` | `2026-11-15 05:00:00` | 0 | 1062.9 | 0.0 | HE nas duas pontas | `sonda/anexos:27` |
| 218 | 12/2026 | `2026-11-22 03:22:03` | `2026-11-22 05:00:16` | `2026-11-22 21:00:00` | `2026-11-22 05:00:00` | 0 | 1058.2 | 0.0 | HE nas duas pontas | `sonda/anexos:28` |
| 296 | 11/2026 | `2026-11-03 00:15:19` | `2026-11-03 07:00:41` | `2026-11-03 19:00:00` | `2026-11-03 07:00:00` | 0 | 1125.4 | 0.0 | HE nas duas pontas | `sonda/anexos:29` |

## 3. Destino dos 4.370 minutos brutos

- **No próprio par invertido, zero em `minutos_trabalhados`.** O motor mede `saída efetiva − entrada efetiva` e grava `max(0, duração)`. `minutos_fora_janela` recebe a soma dos recortes, que pode superar a duração bruta do par. O teto de pontualidade lê esse campo separadamente. `sonda/anexos:3,18–29` · `app/ponto/motor_calculo_v2.py:2759–2787,2886–2895,3224–3258,1170–1196`
- **O crédito do aberto pula candidatos cuja entrada bruta já consta em um fechado de `resultado.periodos`, inclusive se o fechado tem zero trabalhado.** A guarda usa `resultado.periodos`, enquanto os fechados de folga ficam em `resultado.periodos_ft`. Para cada candidato aceito, o motor roda `calcular_periodo` e credita os campos retornados separadamente. Se o candidato também sai invertido, acrescenta zero ao trabalhado e pode acrescentar minutos ao `fora_janela`. `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`
- **Outro par contribui com seu próprio `minutos_trabalhados`.** A soma mensal e a lavratura leem esse campo dos períodos. `DiaPago.minutos_realizados` recebe o mapa da grade por dia. Se algum dos 4.370 minutos brutos aparece sob outro pareamento, no crédito de um candidato com outras pontas ou no realizado da grade, é [nao verificado]. `app/ponto/motor_calculo_v2.py:294–300,2549–2560` · `app/ponto/services/dia_pago.py:173–179,234–267,285–292`

## 24/11 01:31 · sessao bf4a511fb0a8 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

As referências `sonda/anexos:N` numeram o trecho fornecido a partir de `sonda_leitura`, contando a linha em branco.

**UNIVERSO** `pares_fechados 16167 invertidos 174 colabs 117 min_brutos 81266`. `sonda/anexos:3`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|---:|:---|---:|---:|---:|---:|:---|
| 1 | 10/2026 | 12 | 2 | 1 | 968 | `sonda/anexos:4` |
| 1 | 11/2026 | 14 | 4 | 2 | 615 | `sonda/anexos:5` |
| 2 | 10/2026 | 6038 | 54 | 46 | 24762 | `sonda/anexos:6` |
| 2 | 11/2026 | 6337 | 74 | 59 | 34599 | `sonda/anexos:7` |
| 2 | 12/2026 | 275 | 14 | 14 | 7866 | `sonda/anexos:8` |
| 3 | 10/2026 | 1478 | 11 | 10 | 4920 | `sonda/anexos:9` |
| 3 | 11/2026 | 1408 | 9 | 7 | 4567 | `sonda/anexos:10` |
| 3 | 12/2026 | 61 | 1 | 1 | 720 | `sonda/anexos:11` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:12` |
| 4 | 11/2026 | 258 | 2 | 2 | 1022 | `sonda/anexos:13` |
| 4 | 12/2026 | 10 | 1 | 1 | 746 | `sonda/anexos:14` |

As linhas somam **81267 minutos brutos**, um a mais que os **81266** do `UNIVERSO`. A origem da diferença é [nao verificado]. A saída fornecida termina em `rc_sombra=0`. O trecho da sonda não contém traceback. `sonda/anexos:1–27`

## 2. Exemplos e ramo das pontas efetivas

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo pela leitura das pontas | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:15` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:16` |
| 37 | 11/2026 | `2026-11-15 02:00:18` | `2026-11-15 07:01:13` | `2026-11-15 02:00:18` | `2026-11-15 02:00:18` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:17` |
| 50 | 12/2026 | `2026-11-22 07:00:48` | `2026-11-22 19:26:47` | `2026-11-22 07:00:48` | `2026-11-22 07:00:48` | 0.0 | 746.0 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:18` |
| 55 | 11/2026 | `2026-11-11 14:00:21` | `2026-11-11 15:08:32` | `2026-11-11 14:00:21` | `2026-11-11 14:00:21` | 0.0 | 68.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:19` |
| 61 | 11/2026 | `2026-11-08 07:02:51` | `2026-11-08 19:00:01` | `2026-11-08 07:02:51` | `2026-11-08 07:02:51` | 0.0 | 717.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:20` |
| 95 | 10/2026 | `2026-10-10 18:59:36` | `2026-10-11 06:55:15` | `2026-10-10 18:59:36` | `2026-10-10 18:59:36` | 0.0 | 655.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:21` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:22` |
| 110 | 11/2026 | `2026-11-13 01:59:07` | `2026-11-13 07:00:02` | `2026-11-13 01:59:07` | `2026-11-13 01:59:07` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:23` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:24` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 114 | 10/2026 | `2026-10-12 09:58:37` | `2026-10-12 17:10:11` | `2026-10-12 09:58:37` | `2026-10-12 09:58:37` | 0.0 | 431.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:26` |

**HE nas duas pontas, colabs 37 e 113.** O ponto que ancora a entrada às 19h do mesmo dia civil é `_janela_do_dia`, que faz `dia = entrada.date()` e consulta `_md(dia)`. Em seguida, `entrada_efetiva` monta o marco com `marco_no_dia(entrada, hi)`, que troca a hora e preserva a data da batida. O motor calcula `data_jornada` em separado e o usa para a folga e a atribuição do período. O `data_jornada` efetivamente atribuído a cada exemplo é [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1520–1542,1676–1712,2400–2408,2445–2461` · `app/ponto/janela_he.py:20–30,94–103`

**Condição do recorte.** A janela precisa estar ativa e vigente, o par precisa passar pela guarda de folga e pela L-084, e o dia consultado pela janela precisa estar sem decisão `autorizado`. A entrada é recortada quando a antecedência, descontada a extra declarada, supera o piso. Para a saída, a janela de saída precisa estar ativa, com marco final, e o excesso depois dele, descontada a extra declarada, também precisa superar o piso. O marco final nasce na data civil da saída e só avança um dia se o turno cruza meia-noite e o marco ficaria antes da **entrada bruta**. Com marcos de 19h e 07h, essa comparação mantém 07h no dia da batida de saída nos quatro exemplos. Os valores cadastrais de marcos, piso, extra declarada e decisão são [nao verificado]. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1389–1395,1520–1550,1641–1653,1691–1712,1731–1763` · `app/ponto/janela_he.py:41–51,73,94–103,145–153`

**Guarda de 180 min.** A L-084 dispensa a janela quando **as duas** distâncias absolutas superam 180 min. Com marcos 19h e 07h, as quatro entradas brutas estão de 15h59min20s a 16h do marco, e as saídas brutas estão de 1min01s a 2min do marco. Assim, a guarda **do par** deixa a janela passar. Há uma segunda guarda com as pontas externas do dia do juiz. As pontas e os marcos usados nessa segunda guarda são [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1559–1571,1582–1606,1623–1629,1931–1949`

**HE só na saída, colab 109.** A entrada permanece `22:00:41` e `_saida_efetiva` recorta a saída para `22:00:00`, deixando-a 41 segundos antes da entrada. A condição é excesso de saída acima do piso após a extra declarada, com a janela de saída ativa, marco final disponível e sem autorização. O motivo específico pelo qual a entrada ficou intacta é [nao verificado]. `sonda/anexos:22` · `app/ponto/motor_calculo_v2.py:1691–1712,1731–1750` · `app/ponto/janela_he.py:145–153`

**Bloqueio do dia sem previsto, sete exemplos com pontas iguais.** A saída efetiva vira uma cópia da entrada efetiva, inclusive os segundos. Isso identifica o ramo na cópia, pois o recorte de saída da janela devolve um marco de minuto inteiro deslocado por extra declarada em minutos inteiros. `_dia_sem_previsto_fora_da_janela` exige trabalho previamente apurado, folga ou feriado, janela vigente, previsto igual a zero e ausência de autorização. Em folga com colaborador identificado, exige também escala certa. O ramo move o trabalho apurado para `minutos_fora_janela`, zera trabalho e noturno e iguala as pontas. Os valores das guardas por exemplo, inclusive se era folga ou feriado, são [nao verificado]. `sonda/anexos:17–21,23,26,49` · `app/ponto/motor_calculo_v2.py:1355–1362,1440–1483,2457–2461` · `app/ponto/janela_he.py:20–30,145–153`

## 3. Destino dos minutos brutos

- **No próprio par, a contribuição para `minutos_trabalhados` é zero.** Na inversão pela janela, os três corpos de `calcular_periodo` calculam a duração pelas pontas efetivas e gravam `max(0, duracao)`. No bloqueio do dia sem previsto, o motor zera o campo depois de calcular o par. `minutos_fora_janela` recebe as sobras do recorte ou o trabalho apurado antes do bloqueio. Na inversão estrita, as sobras podem somar mais que a duração bruta, como mostram os quatro recortes para `19:00 → 07:00`. A L-093 usa esse campo no teto de pontualidade. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1170–1196,1471–1483,2759–2787,2886–2895,3224–3258`

- **O crédito do aberto não repassa o bruto do fechado.** Ele monta candidatos a partir dos turnos do juiz. Se a entrada bruta já pertence a um fechado em `resultado.periodos`, pula o candidato mesmo quando o fechado tem `trab=0`. `_pagos_a` é formado só por `resultado.periodos`. Para um candidato que passa, o motor chama `calcular_periodo` e soma separadamente o `minutos_trabalhados` e o `minutos_fora_janela` retornados. Se esse recálculo também inverter as pontas, só pode carregar a parcela de fora da janela. A existência de candidato separado ou sobreposto nos casos da sonda é [nao verificado]. `sonda/anexos:15–26,39` · `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`

- **Outro par entra na soma pelo próprio valor apurado.** O total mensal soma `p.minutos_trabalhados`, e a lavra faz a soma por dia com esse mesmo campo. `DiaPago.minutos_realizados` vem por um mapa separado da grade. Os **81266** são o bruto publicado pela sonda. Quanto desse tempo aparece em outro par, no realizado da grade ou na folha lavrada é [nao verificado]. `sonda/anexos:3,15–26` · `app/ponto/motor_calculo_v2.py:294–300` · `app/ponto/services/dia_pago.py:173–179,237–267,285–292`

## 24/11 01:29 · sessao 2f08c8a501dd · E4C-F1-B2b-F0 · verificador · origem=Code

**1. NAO PROCEDE para o contrato inteiro.** As três correções pontuais funcionam, mas o rollback ainda deixa exceções descobertas. `app/chamados/services/fio.py:1508`, `app/chamados/models.py:3463`, `app/chamados/models.py:3509`

- **A — PROCEDE.** A lista deixa `StopIteration` subir da guarda. O gerador do diff a convertia em `RuntimeError`. Com campos já carregados, a ordem segue gravadas → empresa → veto. Num lote com dia diferido, a lista lê o dia da segunda pergunta antes de consultar a empresa da primeira, enquanto o gerador intercalava essas leituras. A caracterização registra contagens, e a comparação direta da ordem e da exceção com `327da9db7` fica [nao verificado]. `app/chamados/services/fio.py:1508`, `app/chamados/services/fio.py:1522`, `app/chamados/services/fio.py:1544`, `app/chamados/juizes.py:339`, `app/chamados/models.py:3826`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:179`
- **2 — PROCEDE para a falha do julgador.** O `except` restaura carimbo, resposta, validação e os três campos da lavra. Em `RETIDA`, a porta restaura os campos da tentativa, mantém a lavra anterior e registra `RECUSA_RETIDA`. Ainda escapam do rollback uma falha em `texto_vazio`, no import de `veredito_lavrado` ou em `foto_da_lavra`, todos depois da mutação. Uma falha injetada na foto deixou o carimbo tentado. O rollback também descarta `_pv_memo` e o reset inicial de `recusa_do_carimbo` não restaura o valor anterior em caso de exceção. `app/chamados/models.py:3422`, `app/chamados/models.py:3435`, `app/chamados/models.py:3463`, `app/chamados/models.py:3482`, `app/chamados/veredito_lavrado.py:279`
- **4c — PROCEDE.** A identidade sobrescreve a recusa com `RECUSA_RETIDA` antes de tocar os campos do carimbo. Com o coletor desligado, o julgador continua retornando `False` após uma leitura do `ContextVar`, sem consulta. Em `carimbar`, a foto da lavra agora vem antes dessa leitura. `app/chamados/models.py:3509`, `app/chamados/models.py:3463`, `app/chamados/services/fio.py:1635`

**2. PROCEDE. Há um leitor indireto após `carimbar_identidade`.**

- **Escritores.** O atributo em memória tem default de classe `None`. `carimbar` o zera e escreve `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` escreve apenas `RECUSA_RETIDA`, nesse ramo. Um teste injeta `RECUSA_C6`. `app/chamados/models.py:3387`, `app/chamados/models.py:3422`, `app/chamados/models.py:3426`, `app/chamados/models.py:3451`, `app/chamados/models.py:3461`, `app/chamados/models.py:3470`, `app/chamados/models.py:3510`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`
- **Leitores no código de produção.** Quatro leituras vêm logo após `carimbar`. A quinta está em `_balde_da_recusa`. No caminho de colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, e esta chama `carimbar_identidade(VIA_FATO_OCUPADO)`. O passivo então lê a mesma instância se a via não mudou e há erro. Se a identidade devolveu `RETIDA`, o leitor cai em `recusa:COLISAO`, pois só trata especialmente `RECUSA_PAR_ATO_COLUNA`. Ocorrência com o coletor ligado em produção [nao verificado]. `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:2093`, `app/chamados/services/materializacao.py:2350`, `app/chamados/services/materializacao.py:2356`, `app/chamados/services/materializacao.py:1792`, `app/chamados/services/materializacao.py:2116`, `app/chamados/services/passivo_validado.py:151`, `app/chamados/services/passivo_validado.py:585`, `app/chamados/services/passivo_validado.py:593`
- **Leitores nos testes.** F9b lê diretamente após a identidade. As demais leituras estão em `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:402`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`, `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78` e `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:328`

**3. PROCEDE.** Os dois testes mordem os defeitos nomeados. O primeiro abre a cerca, injeta `StopIteration` no auxiliar chamado pela guarda e exige essa classe. O segundo injeta a falha do julgador e compara instante, autor e via. Com o julgador na posição antiga, a exceção subiria com o trio tentado ainda na instância. Esse segundo teste cobre o trio. A cobertura de validação e lavra está nos testes anteriores. F9b também falharia sem a nova atribuição, embora parta de `None` em vez de semear uma recusa anterior. Execução da suíte Django [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:221`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:234`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:314`, `app/chamados/models.py:3435`, `app/chamados/models.py:3465`, `app/chamados/tests/test_o261e_f2_fila.py:186`

**4. PROCEDE. E e 4b merecem ressalva na triagem.**

- **E.** O helper `engolir` já existia, mas a captura da falha **na leitura** é nova no ramo `alvo`. No legado, `.first()` pode propagar. No item, a falha de leitura vira rastro e `False` após o savepoint. O censo da cópia aponta zero chamadores diretos com `alvo`. `app/chamados/reconciliador.py:505`, `app/chamados/reconciliador.py:525`, `app/core/observ.py:86`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:638`
- **4b.** A fixture herdada lê o relógio real antes do `freeze_time` dos métodos. `setUpTestData` cria o vínculo, cujo `post_save` chama `invalidar_previsto`, que avalia `timezone.now()`. Efeito nas asserções [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116`, `app/chamados/tests/_vinculo_do_dia.py:25`, `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1607`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:223`
- **3, E′, 4a e F — SEM ACHADO adicional.** `A,B,A` segue o contrato de registrar mudanças. A corrida continua possível entre leitura e escrita e já está anotada para a ativação. O ramo aceita `modelo` genérico, com dois modelos exercitados e exigência de lista fechada [nao verificado]. O ponto cego de `getattr` e `**kwargs` é real, com uso efetivo [nao verificado]. As tags de M1 e CERCA são compartilhadas dentro de cada caso, e o teste exige `AssertionError` com a tag esperada. O resultado de **H** consta do anexo para `4346eddae`. Confirmação independente e resultado no delta [nao verificado]. `app/chamados/reconciliador.py:468`, `app/chamados/reconciliador.py:519`, `app/chamados/reconciliador.py:525`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:521`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:538`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:583`

## 24/11 01:29 · sessao 68876d82a44d · - · - · origem=Ronald (tmux)

**4. PROCEDE em E e 4b, com alcance limitado.**

- **E — PROCEDE.** O ramo de item introduz uma captura na leitura de deduplicação. A leitura sem `alvo` pode propagar a exceção. Com `alvo`, `engolir` captura, registra e devolve `False`. O best-effort de `evento` já cobria a escrita. Autorização literal do corte para essa captura e efeito em produção [nao verificado]. `app/chamados/reconciliador.py:505-532`, `app/core/observ.py:73-105`.
- **4b — PROCEDE.** A fixture herdada cria um vínculo antes do `freeze_time(T0)`. O `post_save` desse vínculo chama `invalidar_previsto`, que avalia `timezone.now()`. Os corpos dos testes novos estão congelados. Dependência das asserções do horário real [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116-128`, `app/chamados/tests/_vinculo_do_dia.py:25-26`, `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:102,217-244`.
- **3 — SEM ACHADO adicional.** `A,B,A` dá três linhas por construção, conforme a regra de gravar as mudanças. A corrida continua possível porque a leitura precede o insert sem trava ou unicidade nessa chave. O censo local mostrou os seis chamadores diretos e nenhum passa `alvo`. Ocorrência concorrente em produção [nao verificado]. `app/chamados/reconciliador.py:466-482,525-531`, `app/core/models.py:285-316`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:620-640`.
- **E′ — SEM ACHADO.** O modelo do item é genérico, e o próprio teste usa `PerguntaDisputa` e `ChamadoColaborador`. Uma lista obrigatória de modelos [nao verificado]. `app/chamados/reconciliador.py:516-531`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473-488`.
- **4a — SEM ACHADO adicional.** Confirmei em memória os dois pontos cegos. `getattr` com nome literal passa despercebido e o desempacotamento de `alvo` aparece como lista vazia de argumentos nomeados. Uso efetivo dessas formas para ativar a fatia [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:570-595,614-640`.
- **F — NAO PROCEDE como vacuidade.** Cada tag admite mais de uma asserção, porém o executor exige `AssertionError` com a tag no caso designado. Ligando o coletor em `M1`, a porta devolve uma recusa falsy e a primeira asserção falha. O mutante `CERCA` remove a guarda e provoca o veto esperado pelo caso. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:55-61,91-93,341-354,410-422,521-545`, `app/chamados/reconciliador.py:408-409`.
- **H — SEM ACHADO.** O anexo informa o resultado para `4346eddae`. A execução histórica e o resultado da suíte no delta ficam [nao verificado].

## 24/11 01:28 · sessao fe9f81ea416b · - · - · origem=Ronald (tmux)

**2. PROCEDE.** `recusa_do_carimbo` é atributo em memória com default de classe. `carimbar` escreve `None`, `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` só escreve `RECUSA_RETIDA` no ramo retido. `app/chamados/models.py:3380`, `:3387`, `:3422`, `:3426`, `:3451`, `:3461`, `:3470`, `:3509–3521`.

- Leituras de produção após `carimbar`: `app/chamados/services/materializacao.py:1105`, `:2093`, `:2350`, `:2356`.
- Leitura indireta de produção após `carimbar_identidade`: `app/chamados/services/passivo_validado.py:151`. O laço entrega a mesma instância e a mesma lista de erros a `_materializar_uma` e `_balde_da_recusa`. Se houve colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, que chama `carimbar_identidade(VIA_FATO_OCUPADO)`. Com `RETIDA` a via fica igual e o erro permanece. `_balde_da_recusa` lê o valor, compara com `RECUSA_PAR_ATO_COLUNA` e devolve a classificação do erro de colisão. Ocorrência com coletor ativo em produção [nao verificado]. `app/chamados/services/passivo_validado.py:147–154`, `:577–593`; `app/chamados/services/materializacao.py:1625–1629`, `:1782–1793`, `:2034–2042`, `:2106–2116`; `app/chamados/models.py:3509–3511`.
- Leitura direta após identidade no teste F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `:328`. Demais leituras de teste: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `:402`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`; `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`; `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`; `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`; `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. A injeção de `RECUSA_C6` em teste fica em `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`.

_8039 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
