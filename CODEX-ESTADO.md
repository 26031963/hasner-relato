# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 08:40** · publicado em **18/11/2026 08:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 08:40 · sessao 80aca513f560 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · verificador · origem=Code

**Resultado: ainda há achados para corrigir ou aceitar expressamente.** A mudança de alcance cura (a), (b) e a coincidência de mês em (c); a troca continua sem teste de integração pela borda real. Há também risco de deadlock por inversão de travas e uma ambiguidade de significado em snapshots históricos. [app/escala/folgas.py:617; app/ponto/services/vigia_cadastro_futuro.py:30; app/ponto/tests/test_residuo_alem_da_competencia.py:50]

| Pergunta | Resultado | Motivo |
|---|---|---|
| 1(a) Foto selecionada por editada | **CONFERE a cura** | A seleção parte de células futuras `origem='gerada'`; editada sozinha não seleciona mês. Há caracterização dentro e fora da janela. [app/ponto/services/cadastro_futuro.py:8; app/escala/folgas.py:620; app/ponto/tests/test_residuo_alem_da_competencia.py:98] |
| 1(b) Dois intervalos no vigia | **CONFERE a cura do recorte** | Geradas e demais origens usam `data >= ini` e `data > hoje`, sem teto; `fim` devolvido é a maior data encontrada, ou `None`. A ressalva de semântica histórica está na pergunta 4. [app/ponto/services/vigia_cadastro_futuro.py:30] |
| 1(c1) Coincidência de mês | **CONFERE a cura do teste** | O teste usa mês posterior e o dublê devolve o dia 15 do mês solicitado; a asserção exige exatamente a data distante. [app/ponto/tests/test_cadastro_futuro_sem_banco.py:645] | [nao verificado]
| 1(c2) Borda real | **PROCEDE parcialmente** | Feriado e encerramento agora atravessam as bordas indicadas; troca ainda chama o serviço diretamente. O teste sem banco exercita `executar_vinculo`, mas com dublês e dia próximo. [app/ponto/tests/test_residuo_alem_da_competencia.py:50; app/ponto/tests/test_residuo_alem_da_competencia.py:79; app/ponto/tests/test_residuo_alem_da_competencia.py:122; app/ponto/tests/test_cadastro_futuro_sem_banco.py:432] |
| Motivo para descartar teste de troca pela borda | **NÃO CONFERE como cura** | A borda realmente exige `request` e `v`; ela passa `desde=data_inicio` e `substitui_dono=True`. Isso explica o custo do teste, mas argumentos equivalentes numa chamada direta não verificam a construção, o acionamento e a transação da borda real. [app/colaboradores/services/vinculo.py:170; app/colaboradores/services/vinculo.py:333; app/ponto/tests/test_residuo_alem_da_competencia.py:50] |
| 2. Censo e teto | **CONFERE condicionalmente** | Os 13 caminhos agrupados abaixo chegam ao alcance sem teto de competência, sujeitos às guardas de data, vínculo e origem. A identidade exata da lista original dos “13 eventos” é **[não verificado]**. [app/ponto/services/cadastro_futuro.py:8; app/ponto/services/cadastro_futuro.py:35] |
| 3. Travas | **PROCEDE risco** | Há ordem inversa entre sincronização de calendário e lavra de foto; a trava da célula também não trava as linhas de foto nem células ainda inexistentes. Ocorrência em produção **[não verificado]**. [app/escala/folgas.py:617; app/escala/folgas.py:180; app/escala/folgas.py:212] |
| 4. Significados | **NÃO CONFERE integralmente** | A nova medida e a lavra são coerentes quanto ao piso pedido, mas `fim` histórico pode ter o significado anterior, sem versão que o distinga no leitor; consumidores externos **[não verificado]**. [app/ponto/services/vigia_cadastro_futuro.py:24; app/ponto/services/vigia_cadastro_futuro.py:106; app/ponto/services/vigia_cadastro_futuro.py:118] |
| 5. Classes de reprovação | **PROCEDEM achados** | Há relógio relido dentro da cadeia e campo persistido com dois significados conforme a época da lavra. Um segundo juiz ou derivação paralela nova nesta alteração **[não verificado]**. [app/escala/folgas.py:278; app/escala/folgas.py:401; app/ponto/services/vigia_cadastro_futuro.py:118] |

**Censo dos caminhos.** Em todas as linhas, “sim” significa passagem pelo alcance comum por intermédio de `regenerar_futuras_do_cadastro`, que seleciona geradas existentes após hoje, sem teto de competência; isso não garante escrita quando as demais guardas recusam o dia. [app/ponto/services/cadastro_futuro.py:8; app/ponto/services/cadastro_futuro.py:35]

| Evento ou grupo de eventos | Escritor / encaminhamento | Alcance; teto |
|---|---|---|
| 1. Troca de vínculo | [vinculo.py:335](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:335) | Sim; piso `data_inicio`, sem teto. |
| 2. Encerramento por alteração de vigência | [vinculo.py:842](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:842), [signals.py:54](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/signals.py:54) | Sim; piso `_desde`, sem teto futuro. |
| 3. Fechamento de vigência em lote | [vinculo.py:652](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:652) | Sim; sem teto. |
| 4. Reabertura de vigência | [vinculo.py:744](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:744) | Sim; sem teto. |
| 5. Mudança de âncora | [signals.py:111](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/signals.py:111) | Sim; há piso derivado da competência aberta e início do vínculo, mas não teto futuro. |
| 6. Trabalha em feriado | [vinculo.py:890](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:890) | Sim; sem teto. |
| 7. Município do posto | [vinculo.py:953](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:953) | Sim; sem teto. |
| 8. Alteração do tipo/template | [cadastro_tipo.py:365](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/services/cadastro_tipo.py:365) | Sim; sem teto. |
| 9. Lançar, alterar ou aplicar folga manual em lote | [folgas.py:332](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:332), [folgas.py:369](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:369), [folgas.py:405](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:405) | Sim, por [folgas.py:297](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:297); lote recusa datas anteriores à competência corrente, sem teto futuro. [app/escala/folgas.py:401] |
| 10. Reparentar folga | [folgas.py:512](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:512) | Sim; sem teto futuro. |
| 11. Trocar calendário do vínculo | [vinculo.py:450](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/colaboradores/services/vinculo.py:450), [folgas.py:620](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:620) | Sim; seleciona dias cobertos pelo vínculo, sem teto. |
| 12. Salvar definição do calendário | [folgas.py:571](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:571), [folgas.py:597](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:597) | Sim; mesma seleção da linha anterior. |
| 13. Salvar, mover ou excluir feriado | [core/signals.py:28](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/core/signals.py:28), [cadastro_futuro.py:56](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/ponto/services/cadastro_futuro.py:56) | Sim; restrito às datas do evento e à resposta da juíza, sem teto de competência. |

Há ainda chamadas explícitas na correção retroativa e na reversão de snapshot; não há evidência aqui de que a enumeração original as exclua ou conte separadamente. [app/colaboradores/management/commands/corrigir_escala_retroativa.py:242; app/colaboradores/services/vinculo.py:1035; **não verificado**]

**Achados**

1. **PROCEDE — inversão de travas.** A sincronização primeiro faz `select_for_update` nas `CelulaDia` e depois apaga/recria `FolgaDia`; `lavrar_foto_folgas --apply` chama o caminho que escreve a foto e depois pede trava nas células futuras. Com uma folga existente e uma célula comum, transações concorrentes podem esperar uma pela outra. A ocorrência e o comportamento do banco de produção são **[não verificado]**. [app/escala/folgas.py:617; app/escala/folgas.py:180; app/escala/folgas.py:212; app/escala/folgas.py:297; app/escala/management/commands/lavrar_foto_folgas.py:163]

2. **PROCEDE — foto não fica integralmente protegida.** O `select_for_update` seleciona células existentes; ele não seleciona para trava as `FolgaDia` nem células ainda ausentes. O gerador consulta células sem essa trava e pode criar ausentes; o resultado de uma intercalação concreta é **[não verificado]**. Entre sincronização e regeneração cadastral, ambas começam por células; a segunda ordena por data e a primeira herda a ordenação do modelo para o colaborador. [app/escala/folgas.py:620; app/ponto/services/cadastro_futuro.py:35; app/escala/models.py:1830; app/ponto/portas/celula.py:894; app/ponto/portas/celula.py:911]

3. **PROCEDE — campo `fim` com dois significados no tempo.** A lavra atual guarda o detalhe integral; `ultimo_lavrado` devolve o detalhe do snapshot mais recente sem versão semântica. Assim, enquanto um snapshot anterior for o último, sua `fim` pode representar o limite antigo, embora uma lavra nova use a última data medida. A existência desses snapshots em produção é **[não verificado]**. [app/ponto/services/vigia_cadastro_futuro.py:90; app/ponto/services/vigia_cadastro_futuro.py:106; app/ponto/services/vigia_cadastro_futuro.py:118; anexo `v2_delta.patch`, trecho de `medir`]

4. **PROCEDE — relógio relido na cadeia de folgas.** O lote aceita e resolve `hoje`, mas a função de relavra chamada depois lê `timezone.localdate()` novamente. A mesma separação aparece no reparentamento; uma virada de dia entre leituras pode mudar o corte. Ocorrência real **[não verificado]**. [app/escala/folgas.py:278; app/escala/folgas.py:385; app/escala/folgas.py:401; app/escala/folgas.py:418; app/escala/folgas.py:512]

5. **PROCEDE — cobertura de teste incompleta para troca distante.** A chamada direta demonstra o serviço com célula distante, mas não sela a borda real para esse cenário; o teste da borda sem banco usa dublês. [app/ponto/tests/test_residuo_alem_da_competencia.py:50; app/ponto/tests/test_cadastro_futuro_sem_banco.py:432]

Quanto aos leitores: `medir` recebe um `fim` sem efeito e devolve a última data existente medida; `ini` e `data_ini` são o **piso solicitado**, não necessariamente o primeiro dia com célula. O comando usa o mesmo argumento `fim` como teto efetivo do gerador, enquanto o vigia o ignora; `ultimo_lavrado` propaga o detalhe e a API o inclui na resposta. Não foi encontrado leitor interno específico que interprete o novo `fim`; leitores externos e dados efetivamente servidos em produção **[não verificado]**. [app/ponto/services/vigia_cadastro_futuro.py:24; app/ponto/services/vigia_cadastro_futuro.py:35; app/ponto/services/vigia_cadastro_futuro.py:106; app/ponto/management/commands/gerar_celulas.py:329; app/api/views_mensageria.py:647]

A cópia não contém metadados Git para confirmar o hash por `git`; execução de testes e dados de produção **[não verificado]**. Nenhum arquivo foi criado, editado ou apagado. [nao verificado]

## 18/11 08:39 · sessao 027483e83a49 · - · - · origem=Ronald (tmux)

Há risco de deadlock por inversão de ordem: a sincronização trava primeiro `CelulaDia` e depois altera `FolgaDia` ([folgas.py:617](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:617), [folgas.py:637](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:637)); a lavra contínua altera a foto e depois trava as células futuras ([lavrar_foto_folgas.py:163](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/management/commands/lavrar_foto_folgas.py:163), [folgas.py:180](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:180), [folgas.py:297](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:297), [cadastro_futuro.py:35](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/ponto/services/cadastro_futuro.py:35)). Ocorrência em produção: [não verificado].

A trava da sincronização cobre células existentes, não protege diretamente as linhas da foto nem uma célula ainda inexistente ([folgas.py:620](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:620), [folgas.py:180](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/escala/folgas.py:180)); o gerador consulta células sem `select_for_update` e cria ausentes ([celula.py:894](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/ponto/portas/celula.py:894), [celula.py:911](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/ponto/portas/celula.py:911)). O resultado concreto de uma corrida depende do isolamento e do entrelaçamento das transações [não verificado].

## 18/11 08:39 · sessao 5d476a20e11b · [nome]-NAO-DESFAZ-O-PAPEL · verificador · origem=Code

| Pergunta | Parecer | Motivo |
|---|---|---|
| 1. Literal do aval | **CONFERE no predicado; cobertura integral não demonstrada.** | A guarda antiga continua presente e a exceção é chamada nos dois ramos: batida a **até 90 minutos inclusive** do marco em que foi sentada e a **mais de 15 minutos** da dona do marco mais próximo. “Próprio marco”, no código, é o marco atribuído à batida; “vizinho” é o marco **mais próximo da batida**, não necessariamente o adjacente na régua. Isso corresponde à leitura operacional do texto fornecido, mas o texto não define expressamente essas duas expressões. [Aval 19:50, item 1: linha não verificada]; [utils.py](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:502), [utils.py:586](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:586), [utils.py:651](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:651). |
| 2. Escritores e leitores | **CONFERE o censo dos chamadores diretos, com uma correção de contagem.** | São **seis chamadas diretas em produção**, incluindo as três em `utils.py` e a chamada pelo transcritor; `regua_defesa.py:492` é um chamador **indireto**, não uma sétima chamada direta. O efeito em números de produção é **[não verificado]**. [utils.py:427](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:427), [utils.py:787](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:787), [utils.py:1661](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:1661), [utils.py:1712](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:1712), [motor_calculo_v2.py:3062](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/motor_calculo_v2.py:3062), [detector_anomalias.py:212](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/chamados/detector_anomalias.py:212). |
| 3. Classes de reprovação e RED | **CONFERE parcialmente; prova insuficiente para liberar.** | Há uma função nova compartilhada, dentro do caminho do juiz registrado, sem leitura de relógio no predicado. Ela repete a aritmética de distância absoluta que a guarda já usa; não demonstra uma nova resposta concorrente ao juiz registrado. Os cinco testes contêm os dois exemplos, um controle e dois casos destinados aos mutantes, mas execução, mutação e três selos exigidos estão **[não verificados]**. [core/juizes.py:221](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/core/juizes.py:221), [utils.py:507](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:507), [test_cirineu_nao_desfaz_o_papel.py:28](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/tests/test_cirineu_nao_desfaz_o_papel.py:28). |
| 4. Contraexemplo | **PROCEDE como risco a testar; infração à lei não demonstrada.** | Uma rajada encadeada pode ter batidas consecutivas separadas por até 15 minutos, mas a primeira e a dona do marco mais próximo separadas por mais de 15. O classificador já chama de rajada a sequência por distâncias **consecutivas**; a exceção compara apenas a batida com a dona. Se a guarda antiga e o raio também forem satisfeitos, a exceção preserva a batida mesmo nesse agrupamento. O aval fornecido, porém, fala literalmente da distância à **dona**, portanto isso não prova que a preservação viole o corte. Uma fixture completa que chegue a essa geometria e seu resultado são **[não verificados]**. [orfa_classe.py:40](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/orfa_classe.py:40), [utils.py:507](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:507); aval 19:50, item 1 [linha não verificada]. |
| 5. Exportação, dinheiro e `!` | **PROCEDE: toca dinheiro indiretamente; efeito e condição de pouso não medidos.** | A Zona 5 usa as células para escolher P1/P2 e escrever minutos de atraso e saída antecipada; logo o motor pode produzir outros valores mesmo sem edição em seu arquivo. O comando de cartório seleciona células passadas sem piso de competência e passa `forcar` à lavra, portanto uma relavra exige delimitação operacional. Alteração efetiva de exportada, rubricas e `!` em produção: **[não verificado]**. [motor_calculo_v2.py:3062](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/motor_calculo_v2.py:3062), [motor_calculo_v2.py:3073](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/motor_calculo_v2.py:3073), [processar_cartorio.py:45](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/management/commands/processar_cartorio.py:45). |

**Achados**

1. **O retorno alterado tem mais de um destino.** `_alinhar_por_papel` escreve o mapa marco → índice; `_match_marcos` transforma o mapa em células, órfãs e marcas de consumo. No ramo DP ele também escreve `par_marco`, `orfa` e `consumed`. A mudança pode trocar uma célula `missing` por uma célula com hora, delta, status e tipo real, e retirar a hora da lista de órfãs; o valor exato depende da geometria. [utils.py:586](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:586), [utils.py:622](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:622), [utils.py:660](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:660), [utils.py:314](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:314).

2. **Efeito por leitor direto:** o transcritor pode devolver outro rótulo/delta, afetando o booleano de volta do intervalo; a grade simples recebe outras células e órfãs; o dry-run da grade por turno pode alterar quais sobras são consideradas para intervalo; a chamada definitiva alimenta a grade e passa ainda por remapeamento e reconciliação; o detector pode trocar a classificação de órfã informativa por `ok` para marco interno ou por desvio de borda. A Zona 5 pode alterar atraso e antecipação, não necessariamente na mesma direção em todos os casos. Nenhum número de frota foi estabelecido. [regua_defesa.py:489](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/regua_defesa.py:489), [utils.py:787](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:787), [utils.py:1661](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:1661), [utils.py:1712](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:1712), [detector_anomalias.py:212](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/chamados/detector_anomalias.py:212), [motor_calculo_v2.py:3062](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/motor_calculo_v2.py:3062).

3. **A propagação pela ata não é uma mudança automática do dado já gravado.** Após lavra, leitores podem receber outra lista ou contagem: por exemplo, o leitor da célula reconstrói órfãs, o propositor usa órfãs e furos, e o vigia conta células com órfã. O censo indireto completo e a variação de cada consumidor informado no brief não foram verificados integralmente nesta leitura: **[não verificado]**. [leitor_celula.py:180](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/services/leitor_celula.py:180), [propositor.py:38](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/propositor.py:38), [vigia_pareador_celula.py:74](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/ponto/services/vigia_pareador_celula.py:74).

4. **Bordas e significados precisam de prova.** A exceção usa `total_seconds()` e converte as constantes de minutos para segundos: exatamente 90 minutos entra; exatamente 15 não entra. O teste novo não fixa essas duas igualdades nem seus segundos vizinhos. O casamento produz horas em `HH:MM`, e consumidores também identificam por minuto, embora a decisão use segundos; duas batidas no mesmo minuto podem ficar indistinguíveis na leitura. Os marcos são encadeados com datas para meia-noite, enquanto `distancia_do_marco` responde outra pergunta, circular pela hora do dia. Testes de mudança de fuso, bordas e colisão por minuto: **[não verificado]**. [utils.py:507](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:507), [utils.py:408](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:408), [utils.py:330](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:330), [regua_defesa.py:395](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/regua_defesa.py:395).

5. **Os controles não estão demonstrados verdes.** O controle existente da rajada aceita até duas células preenchidas e pelo menos duas órfãs; sozinho não prova “uma senta, três órfãs”. O aval corrigido pede conservar o comportamento de hoje, não essa expectativa antiga. Os selos `EntradaComOutraOrfaNoTurno`, O195 col174 e O316 col347 existem, mas seu resultado com o diff é **[não verificado]**. [test_contract_cluster_marco.py:64](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/tests/test_contract_cluster_marco.py:64), [test_ata_x_turno_entrada.py:127](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/tests/test_ata_x_turno_entrada.py:127), [test_montador_realizado_pela_autoridade.py:244](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/tests/test_montador_realizado_pela_autoridade.py:244), [test_montador_realizado_pela_autoridade.py:297](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/tests/test_montador_realizado_pela_autoridade.py:297); aval 19:50, item 1 [linha não verificada].

6. **Antes do pouso:** medir na sombra o diff da competência 11 por pessoa/dia e por rubrica, com células, órfãs, detector, chamados e efeitos indiretos; contar quantos dos dias da autópsia atribuídos à guarda são cobertos, separando os produtores de órfãs; executar os controles e os dois mutantes. A extensão do impacto, o hash da exportada e a decisão de pouso são **[não verificados]**. A exigência de diff e a medição pedida constam do dado fornecido no aval 19:50, item 1 [linha não verificada]; os produtores adicionais aparecem em [utils.py:801](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:801) e [utils.py:1824](/home/ronald/codex_ws/5e7169a9e44316f7a484a72cc512257a538ace5b/app/escala/utils.py:1824).

A cópia disponível não tem metadados Git acessíveis no diretório examinado; identidade do `HEAD`, diff efetivo contra a base e resultado de testes: **[não verificado]**. Foram lidos os arquivos e o diff fornecido, sem escrita. [nao verificado]

## 18/11 08:39 · sessao b6dd6ff7e160 · - · - · origem=Ronald (tmux)

A análise de `fim`, `data_ini` e seus leitores foi enviada ao agente principal, incluindo a ressalva sobre snapshots históricos com o significado anterior de `fim` ([vigia_cadastro_futuro.py:123](/home/ronald/codex_ws/44a37274c7e8420d2894656b929b6991aa092068/app/ponto/services/vigia_cadastro_futuro.py:123)).

## 18/11 08:39 · sessao 75fe743b83c3 · - · - · origem=Ronald (tmux)

Censo e achados enviados ao agente principal, com referências de arquivo e linha.

## 18/11 08:35 · sessao f19eeaada1ea · E4C-F2-P2 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** O diff proposto altera um desfecho de A8 sob recusa: a nova seleção sob trava pode retirar do lote uma pergunta que o HEAD ainda submeteria à guarda. Além disso, o diff precisa ser adaptado à P1 já presente, inclusive para preservar o backfill em uma migração própria. [app/chamados/models.py:1613; app/chamados/models.py:3398; p2.diff, bloco de `ressuscitar_perguntas` e migração 0055: linhas não fornecidas]

| Pergunta | Resultado | Motivo |
|---|---|---|
| 1. Premissas | **CONFERE em parte** | A cópia contém a guarda B2a: fora de uma cerca aberta ela retorna sem julgar; A8 chama `descarimbar_em_lote`, que a consulta. A afirmação de que A8 “grava sem julgamento hoje” vale **fora da cerca**, não para toda chamada. Os três leitores citados existem no HEAD, e o anexo MATRIZ inclui A8. [app/chamados/services/fio.py:1389; app/chamados/models.py:1637; app/chamados/models.py:3398; app/chamados/models.py:1918; app/chamados/models.py:3152; app/chamados/models.py:3674; MATRIZ.md, tabela “Sítios (ii)”: linha não fornecida] |
| 1. Identidade e escopo | **NÃO CONFIRMADO integralmente** | O dado fornecido afirma HEAD `dd489c627` e B2a `4eb10f99f` no ar; esta cópia não disponibiliza metadados Git para confirmar a ancestralidade ou o estado em produção. Dentro de A8, o diff acrescenta seleção sob trava e REABERTURA compensadora, com leitura da sequência e emissão de eventos pela porta de registro. [aval.txt:1; retoma_08_04.txt:2; app/chamados/models.py:4152; p2.diff, bloco de A8: linhas não fornecidas] [não verificado: ancestralidade e produção] |
| 2. Colisão com aval vigente | **PROCEDE** | O complemento de 08:33 resolve nominalmente a condição “pouso da B2” pela B2a e retira a condição de espera pelo sítio classe (ii), mas proíbe mudar o comportamento de A8 sob recusa. A mudança de seleção descrita na pergunta 3 colide com essa proibição literal. [aval.txt:2; aval.txt:3; aval.txt:5] | [nao verificado]
| 3. Condições literais | **PROCEDE** | O diff antigo adiciona 0054 e 0055 como novas; ambas já existem na cópia. A 0055 existente só cria o campo e a restrição, enquanto a 0055 do diff inclui o backfill. Reutilizar simplesmente a 0055 existente não executaria esse backfill; o próprio HEAD prevê migração própria no P2. [app/chamados/migrations/0054_decisao_da_ressuscitacao.py:14; app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:1; p2.diff, migração 0055: linhas não fornecidas] |
| 4. Exportada, dinheiro ou `!` | **NÃO CONFIRMADO integralmente** | A8 pode alcançar pergunta de competência entregue fora de cerca, pois ali a guarda não julga. A REABERTURA muda a sequência e os leitores propostos; isso é declarado no complemento, mas não prova por si só alteração de folha exportada ou de dinheiro. O diff fornecido não demonstra um apply de passivo; efeito completo em exportada, dinheiro e demais operações sujeitas a `!`: **[não verificado]**. [app/chamados/services/fio.py:1397; aval.txt:2; p2.diff, blocos de A8 e leitores: linhas não fornecidas] |

**Recusa em A8, antes e depois**

- **HEAD, com a mesma pergunta ainda no lote:** fora de cerca, a guarda não julga e o lote prossegue. Dentro de cerca, se o veto recusa, a guarda anota a empresa e levanta `EscritaEmCompetenciaEntregue`; a exceção sai do `atomic` de A8, desfazendo as escritas daquela unidade, inclusive fotos de decisão já registradas. O abridor da cerca desfaz seu savepoint e devolve `(None, vetadas)`. [app/chamados/services/fio.py:1402; app/chamados/services/fio.py:1424; app/chamados/models.py:1613; app/chamados/services/fio.py:1431]

- **HEAD + diff, se a pergunta continua selecionada e a mesma guarda recusa:** a REABERTURA e os eventos gravados antes da guarda também estão no `atomic` de A8 e são desfeitos; a exceção segue ao abridor. Nesse caso, o desfecho transacional de recusa permanece igual. [app/chamados/models.py:1613; app/chamados/models.py:4152; app/ponto/portas/celula.py:945; app/chamados/services/fio.py:1424; p2.diff, bloco de A8: linhas não fornecidas]

- **Diferença concreta de seleção:** se uma pergunta escolhida antes da trava deixa de estar materializada antes de a trava ser obtida, o HEAD ainda a encaminha à guarda do lote; o diff a exclui por `materializada_em__isnull=False`. Se ela seria vetada, o HEAD recusa e desfaz a unidade, enquanto o diff não consulta a guarda para ela e pode retornar `0` se nenhuma outra restar. Portanto, não cabe afirmar que o comportamento sob recusa permanece idêntico em todos os casos. [app/chamados/models.py:1599; app/chamados/models.py:1618; app/chamados/models.py:3398; p2.diff, bloco de seleção sob trava: linhas não fornecidas]

Os **docs do ERRO-BOBO** e o **dreno de crons no ato** estão escritos como condições: o aval de 17:58 especifica documentação, drenagem antes de migrate e reload e o tratamento das medidas após reload. O complemento de 18:08 define explicitamente o **denominador da corrida de `respondida_em`**, relativo às respostas humanas desde o reload; ele não define, no texto fornecido, denominadores específicos de N1 e N2. A frase “N1/N2 com denominador pelo COMPLEMENTO-1 18:08” exige essa distinção para fechar literalmente. [libera_b3_f2p2_1758.txt:2; libera_b3_f2p2_1758.txt:4; complemento1_libera_b3_1808.txt:1; aval.txt:4]

**Quem escreve e quem lê os campos da REABERTURA acrescentada em A8**

| Campos gravados | Escritor e leitores identificados |
|---|---|
| `pergunta_id`, `ato`, `ato_em` | A8 monta a linha e `DecisaoPergunta.registrar` a insere. A sequência é ordenada por `ato_em, pk`; `respostas_anteriores` lê o ato e a carga da tarefa usa ato e instante. A leitura proposta pela P2 também percorre esses campos. [app/chamados/models.py:4081; app/chamados/models.py:4152; app/chamados/models.py:3680; app/chamados/services/tarefa_do_dia.py:421; p2.diff, blocos de A8 e `leitura`: linhas não fornecidas] |
| `ato_texto`, `respondida_original_em`, `validada_original_por_id`, `ato_por`, `ato_motivo` | A8 monta a foto e `registrar` escreve. `respostas_anteriores` entrega a REABERTURA à tela; o modal lê esses campos. A tarefa lê `ato_texto` em linhas de RESPOSTA, mas ignora a REABERTURA marcada antes de escolher o último ato. [app/chamados/models.py:4152; app/chamados/models.py:3680; app/templates/chamados/partials/modal_fio.html:210; app/chamados/services/tarefa_do_dia.py:159; app/chamados/services/tarefa_do_dia.py:421; p2.diff, bloco de A8: linhas não fornecidas] |
| `validada_original_em` | A8 monta a foto e `registrar` escreve; a restrição do modelo também referencia o campo. Leitor funcional específico dessa foto: **[não verificado]**. [app/chamados/models.py:4058; app/chamados/models.py:4089; app/chamados/models.py:4152; p2.diff, bloco de A8: linhas não fornecidas] |
| `da_ressuscitacao` | A8 propõe gravar `True` e `registrar` insere; `es_da_ressuscitacao` lê a marca e a carga da tarefa exclui a linha marcada. [app/chamados/models.py:4103; app/chamados/models.py:4152; app/chamados/services/tarefa_do_dia.py:421; p2.diff, bloco de A8: linhas não fornecidas] |

As fotos RESPOSTA/VALIDAÇÃO já são registradas em A8 no HEAD; a P1 já declara `resposta_mantem_validacao=True` na foto RESPOSTA. A P2 propõe passar a ler esse campo para decidir o efeito da resposta sobre a validação. As listas acima identificam leitores encontrados, **sem alegação de censo exaustivo**. [app/chamados/models.py:1619; app/chamados/models.py:4139; p2.diff, `_resposta_desfaz_validacao`: linhas não fornecidas] [não verificado: exaustividade]

O **item 2 do aval de 08:33** é lembrete em raia própria, com prioridade da espinha na trava; o texto fornecido não mostra colisão com o item 1. Colisão real de arquivos ou execução concorrente: **[não verificado]**. [aval.txt:6; retoma_08_04.txt:6]

## 18/11 08:29 · sessao e54c3fff74ff · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · verificador · origem=Code

**Resultado: PROCEDENTE em pontos que impedem aprovar o diff como cumprimento integral do aval.** Os principais são o limite literal dos meses da foto, o significado misto dos intervalos do vigia e a falta de prova dos 13 eventos. Também há custo por vínculo e por célula; seu impacto real está **[não verificado]**. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619), [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35).

A cópia disponível não contém metadados Git nem o arquivo do censo indicado; a identidade do HEAD e a comparação com o pai dependem, portanto, do hash e do patch fornecidos **[não verificado]**. Não executei testes, mutantes nem medições: seus resultados e logs estão **[não verificado]**. [nao verificado]

## 1. Diff versus avais e proibições

O alcance comum seleciona células `gerada` com `data > hoje`, sem teto, e a relavra usa a porta com `estender_horizonte=False`. Isso implementa a parte central do pedido para os chamadores que efetivamente chegam ao serviço. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35).

| Proibição ou limite | Veredito sobre o patch fornecido | Evidência ou contraexemplo |
|---|---|---|
| Tocar o gerador | **NÃO PROCEDENTE** | O patch não altera o corpo do gerador; o vigia continua chamando `montar_dna` e o comando continua chamando `medir` e `lavrar`. [app/ponto/services/vigia_cadastro_futuro.py:27](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:27), [app/ponto/management/commands/gerar_celulas.py:337](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/management/commands/gerar_celulas.py:337). |
| Tocar corpo ou janela da foto | **NÃO PROCEDENTE**, quanto ao corpo da função proibida | A mudança está no seletor que entrega meses; `_gerar_foto_folgas_no_ato` continua recebendo `datas_foto` e gravando a foto mensal. O alcance entregue a ela, porém, tem a ressalva da última linha desta tabela. [app/escala/folgas.py:166](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:166), [app/escala/folgas.py:623](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:623). |
| Tocar o cron `lavrar_foto_folgas` | **NÃO PROCEDENTE** | O patch fornecido não altera o cron, cujo seletor próprio permanece no código. [app/escala/management/commands/lavrar_foto_folgas.py:75](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/management/commands/lavrar_foto_folgas.py:75). |
| Tocar motor | **NÃO PROCEDENTE** | Não há arquivo de motor no patch fornecido; a relavra chama a porta existente. [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50). |
| Mudar leitor | **NÃO PROCEDENTE** quanto a editar o código de leitor; **PROCEDENTE** quanto à mudança de sentido recebido | `ultimo_lavrado` continua lendo `detalhe`, mas esse detalhe passa a conter contagens sobre outro universo. [app/ponto/services/vigia_cadastro_futuro.py:41](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:41), [app/ponto/services/vigia_cadastro_futuro.py:117](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:117). |
| Juiz novo | **NÃO PROCEDENTE** no patch | O novo helper seleciona linhas; a escolha do vínculo continua delegada a `vinculo_do_dia` ou `vinculos_dos_dias`. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:44](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:44), [app/ponto/services/cadastro_futuro.py:73](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:73). |
| Apply em lote do passivo | **NÃO PROCEDENTE** no patch; execução externa **[não verificado]** | O serviço relavra no curso dos eventos e não apresenta um chamador de lote de passivo no patch fornecido. [app/ponto/services/cadastro_futuro.py:20](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:20). |
| Criar célula | **NÃO PROCEDENTE** no caminho novo | O seletor parte de células existentes e chama a porta para cada dia selecionado com `estender_horizonte=False`. [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:43), [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50). |
| Tocar os dois testes da torneira | **NÃO PROCEDENTE** | Eles não constam dos arquivos do patch fornecido. Resultado deles após o diff **[não verificado]**. | [nao verificado]
| Mudar escopo dos meses da foto | **PROCEDENTE no literal do limite do aval 08:04** | A primeira consulta inclui **qualquer origem** dentro da janela, e a segunda acrescenta geradas sem teto. Contraexemplo: só existe uma célula futura *editada* em novembro, dentro da janela: novembro ainda recebe foto sem célula gerada futura naquele mês. O teste novo declara esse comportamento. [app/escala/folgas.py:623](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:623), [app/ponto/tests/test_residuo_alem_da_competencia.py:99](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_residuo_alem_da_competencia.py:99). É preservação do comportamento anterior dentro da janela, mas o aval fornecido diz “só mês que TEM célula gerada futura”. |

Medição prévia do vigia, dry publicado, mutante com log, suite e demais condições de pouso: **[não verificado]**. [nao verificado]

## 2. Escritores, leitores e significado

- **`CelulaDia.origem`, `data`, `escala_geradora` e `dna`.** O modelo define a data da célula, origem gerada/editada, geradora como proveniência e DNA como snapshot; a porta automática escreve os campos e a porta humana pode marcar origem editada. O alcance lê origem e data; o vigia lê também geradora e DNA para comparar com o vínculo do dia e o cadastro. [app/escala/models.py:1681](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1681), [app/ponto/portas/celula.py:251](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:251), [app/ponto/portas/celula.py:632](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:632), [app/ponto/services/vigia_cadastro_futuro.py:54](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:54).

- **`FolgaDia`.** É foto por vínculo e data. O escritor da foto apaga e recria registros de calendário preservando os manuais; há também lançamento manual. O cálculo do vínculo e a precedência leem folgas. O sincronizador escolhe meses a partir de dias selecionados e reconcilia os dias futuros de cada mês, não apenas os dias que têm célula. [app/escala/models.py:1582](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1582), [app/escala/folgas.py:180](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:180), [app/escala/folgas.py:329](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:329), [app/escala/models.py:1249](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1249), [app/ponto/precedencia.py:206](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/precedencia.py:206), [app/escala/folgas.py:632](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:632).

- **Dict de `medir`. PROCEDENTE: os campos não descrevem um único intervalo.** `ini` e `fim` são o intervalo pedido; para não geradas, o filtro usa esse intervalo inclusivo e `data > hoje`; para geradas, ignora *ambos* os limites e usa todas as datas `> hoje`. `fim_do_alcance` é o máximo das geradas, ou `None`. Contraexemplo: com hoje em 18/11 e intervalo pedido 1–20/12, uma gerada de 25/11 entra, mas uma editada de 25/11 não entra. [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/services/vigia_cadastro_futuro.py:88](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:88).

- **Contadores e selos.** `universo`, baldes e selo de DNA contam geradas; `universo_julgada` e selo de julgada contam a união mista descrita acima. Assim, a descrição “TODA futura” do denominador de julgada não corresponde a todas as editadas futuras fora do intervalo pedido. Os demais campos particionam ou qualificam os casos conforme os ramos do vigia. [app/ponto/services/vigia_cadastro_futuro.py:37](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:37), [app/ponto/services/vigia_cadastro_futuro.py:48](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:48), [app/ponto/services/vigia_cadastro_futuro.py:96](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:96).

- **Snapshot e leitores. PROCEDENTE: número e sentido podem mudar.** `lavrar` grava `data_ini=medida['ini']`, numerador, denominador e o dict completo em `detalhe`; uma gerada anterior a `data_ini`, como no contraexemplo, pode compor o número. `ultimo_lavrado` devolve o detalhe e a API do copiloto o expõe. Uma divergência em gerada fora da janela aumenta o número em relação ao universo antigo; o valor de produção **[não verificado]**. Leitura específica por haiku, placar e outros selos **[não verificado]**. [app/ponto/services/vigia_cadastro_futuro.py:105](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:105), [app/ponto/services/vigia_cadastro_futuro.py:117](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:117), [app/api/views_mensageria.py:643](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/api/views_mensageria.py:643).

O serviço novo exclui hoje estritamente; a porta geral e o cron também têm caminhos para o próprio dia de hoje. Isso é diferença declarada de alcance, não prova isolada de erro. [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/management/commands/gerar_celulas.py:303](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/management/commands/gerar_celulas.py:303).

## 3. Propriedade dos 13 eventos

**Cumprimento para os 13: [não verificado].** O item `(e)` do censo não está disponível nesta cópia, portanto não é possível identificar os 13 com segurança nem declarar uma lista exaustiva de falhas.

Nos caminhos identificáveis, folga lançada, vigência, âncora, vínculo, template e feriado chegam ao serviço sem teto, sujeitos às guardas de cada caminho. [app/escala/folgas.py:290](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:290), [app/escala/signals.py:80](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/signals.py:80), [app/escala/signals.py:131](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/signals.py:131), [app/colaboradores/services/vinculo.py:335](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:335), [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/services/cadastro_tipo.py:370), [app/core/signals.py:28](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/core/signals.py:28).

O seletor de foto ainda lê `_janela_default`, mas acrescenta à união as geradas sem teto; essa leitura remanescente **não demonstra, por si, um evento gerado distante perdido**. Não identifiquei outro seletor produtivo `_janela_default` que prove uma falha entre os eventos; a exaustividade permanece **[não verificado]**. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619).

## 4. Classes de reprovação

- **Segundo juiz ou segundo escritor fora da porta — NÃO PROCEDENTE no diff examinado.** O helper seleciona e delega; a escrita de célula passa por `regenerar_celulas_vinculo`. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50).

- **Relógio fora da borda — NÃO PROCEDENTE no código novo examinado.** Os serviços exigem `hoje`; por exemplo, o salvamento de calendário lê uma data e a passa aos vínculos. Outros caminhos não auditados integralmente **[não verificado]**. [app/ponto/services/cadastro_futuro.py:12](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:12), [app/escala/folgas.py:597](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:597).

- **Derivação paralela — PROCEDENTE quanto ao conjunto de meses.** A foto mantém o seletor próprio de todas as origens dentro da janela e o une ao alcance novo. Contraexemplo: a editada sem gerada no mês ainda determina foto, como no item 1. Não é um segundo juiz de vínculo. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619).

- **Selo/RED sem mordida — PROCEDENTE parcialmente.** A caracterização de foto passa por coincidência de mês; os testes novos mais fortes não provam todos os disparos reais. Detalhes no item 6. [app/ponto/tests/test_cadastro_futuro_sem_banco.py:645](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:645), [app/ponto/tests/test_residuo_alem_da_competencia.py:51](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_residuo_alem_da_competencia.py:51).

- **Travas — deadlock ou trava a menos: [não verificado].** As duas consultas da foto usam `select_for_update`; a segunda pode reencontrar linhas já travadas, o que não prova falta de trava. O modelo declara ordenação por colaborador e data, e a consulta da relavra ordena por data; não há aqui demonstração suficiente de inversão efetiva de ordem ou deadlock. Os dublês sem banco não exercitam locks. [app/escala/folgas.py:620](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:620), [app/escala/models.py:1830](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1830), [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:43), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:170](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:170).

- **Campo com dois significados — PROCEDENTE.** `ini`/`fim` e `data_ini` continuam rotulando o intervalo pedido, enquanto os números de geradas podem incluir datas fora dele; o denominador de julgada mistura dois recortes. Contraexemplo no item 2. [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/vigia_cadastro_futuro.py:110](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:110).

## 5. Custo e teto

**PROCEDENTE: há trabalho por vínculo e por célula.** Template percorre os vínculos do tipo; cada um consulta e trava suas células e chama a porta uma vez por dia. Cada chamada à porta consulta exportações e células. Assim, um vínculo com muitas células futuras multiplica chamadas e consultas. [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/services/cadastro_tipo.py:370), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35), [app/ponto/services/cadastro_futuro.py:49](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:49), [app/ponto/portas/celula.py:218](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:218).

**Feriado: NÃO PROCEDENTE a premissa de que necessariamente percorre oito meses por vínculo.** Ele filtra pelas datas de feriado alteradas, agrupa as células por geradora e chama o mesmo serviço com `datas=dias`; ainda assim, paga seleção por vínculo e porta por célula selecionada. [app/ponto/services/cadastro_futuro.py:64](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:64).

Os caminhos podem rodar sincronamente em request: o signal de feriado no save e a propagação de template no fluxo de salvamento. A configuração da UI é `gthread`, `timeout=30`; isso não demonstra prazo máximo de 30 segundos por request. Tempo e número de consultas antes/depois **[não verificado]**. [app/core/signals.py:28](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/core/signals.py:28), [app/escala/views_wizard.py:95](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/views_wizard.py:95), [app/gunicorn.conf.py:17](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/gunicorn.conf.py:17), [app/holerite/views.py:595](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/holerite/views.py:595).

## 6. Testes [nao verificado]

- **PROCEDENTE: coincidência de data na caracterização da foto.** `FIM + 1` é 21/11, mas o dublê do calendário devolve 13/11; ambos são novembro. O assert de mês e o de existência podem passar sem provar folga no dia distante nem foto de mês posterior à competência. [app/ponto/tests/test_cadastro_futuro_sem_banco.py:20](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:20), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:585](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:585), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:645](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:645).

- **NÃO PROCEDENTE: essa lacuna deixa a foto distante inteiramente sem teste.** O teste com banco usa dezembro e verifica `FolgaDia` no dia distante e `cel.trabalha=False`. Sua execução **[não 

[truncado em 20000 de 21867 caracteres]

## 18/11 08:27 · sessao 8d91d47ae5dc · - · - · origem=Ronald (tmux)

Enviados ao agente principal os achados sobre custo e o contraexemplo de troca de vínculo com calendário padrão: a foto é gerada apenas para o mês civil atual e o seguinte, enquanto a relavra alcança células posteriores ([vinculo.py:151](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:151), [vinculo.py:335](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:335)).

O censo indicado não foi encontrado na cópia; a verificação exaustiva dos 13 eventos permanece [não verificado].

## 18/11 08:27 · sessao c4ef979324bd · - · - · origem=Ronald (tmux)

Os achados sobre campos, foto, vigia e leitores do snapshot foram enviados ao agente principal, com referências ao HEAD e contraexemplo para o intervalo misto.

## 18/11 08:27 · sessao 0fae26c8a5a4 · - · - · origem=Ronald (tmux)

Achados sobre testes e travas enviados ao agente principal com referências de arquivo e linha. [nao verificado]

## 18/11 08:15 · sessao 176d33e7aeeb · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · construtor · origem=Code

A implementação e os testes estão escritos na cópia. **A cura ainda não está certificada:** não executei RED, GREEN, mutantes ou suíte, conforme a instrução; a sintaxe dos quatro arquivos passou por análise com `ast.parse`. O HEAD da cópia permanece `dd489c627bb0a8d5f8782945c7e240e6621c98ed`; não criei commit, conforme a proibição. [cadastro_futuro.py:8](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:8), [test_residuo_alem_da_competencia.py:20](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/tests/test_residuo_alem_da_competencia.py:20)

Alterações:

- [cadastro_futuro.py::alcance_celulas_geradas](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:8), `regenerar_futuras_do_cadastro` e `regenerar_futuras_por_feriado`: alcance comum de células geradas existentes, posteriores a hoje, sem teto de competência. A relavra continua com `estender_horizonte=False`.
- [folgas.py::sincronizar_foto_futura_do_calendario](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:605): mantém qualquer origem dentro da janela e seleciona geradas além dela; os meses continuam saindo dos dias selecionados.
- [vigia_cadastro_futuro.py::medir](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/vigia_cadastro_futuro.py:24): mede todo o alcance gerado e preserva as não geradas no intervalo anteriormente solicitado. Os limites retornados são ampliados para abranger as células medidas.
- [test_residuo_alem_da_competencia.py::ResiduoAlemDaCompetenciaTest](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/tests/test_residuo_alem_da_competencia.py:20): casos para troca e encerramento de vínculo, folga manual, calendário, controle de dono, editadas, vigia e feriado. A execução e a capacidade de morder dos casos estão **[não verificado]**.

**ESMERIL-SEMÂNTICA.** `hoje` é dia civil exclusivo (`data > hoje`); `desde`, quando fornecido, é inclusivo. A janela da foto usa os dois extremos inclusivos e a vigência do vínculo também inclui início e fim. [cadastro_futuro.py:14](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:14), [cadastro_futuro.py:38](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:38), [folgas.py:620](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:620), [alimentacao.py:214](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/alimentacao.py:214)

O seletor lê `colaborador_id`, `data` e `origem` da célula; a porta existente cria células e escreve `trabalha`, `escala_geradora`, `dna` e a trilha de regeneração, enquanto a juíza recebe o carimbo para decidir o vínculo. A foto lê a definição do calendário e escreve ou remove `FolgaDia` por vínculo, data e origem de calendário; a folga manual tem porta própria. Os escritores completos de cadastro desses campos são **[não verificado]**. [cadastro_futuro.py:14](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:14), [celula.py:289](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/portas/celula.py:289), [alimentacao.py:228](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/alimentacao.py:228), [folgas.py:43](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:43), [folgas.py:180](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:180), [folgas.py:335](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:335)

O custo cresce com as células futuras adicionais: feriado consulta as células nas datas recebidas e chama a relavra por vínculo encontrado; template percorre os vínculos do tipo e a relavra percorre os dias selecionados. Quantidades de consultas e tempos antes/depois estão **[não verificado]**. [cadastro_futuro.py:68](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:68), [cadastro_tipo.py:370](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/services/cadastro_tipo.py:370)

O mutante principal, **“teto de volta”**, é este patch, para aplicação temporária pela sessão de testes; não foi aplicado aqui: [nao verificado]

```diff
--- a/app/ponto/services/cadastro_futuro.py
+++ b/app/ponto/services/cadastro_futuro.py
@@
     if hoje is None:
         raise ValueError('alcance_celulas_geradas: hoje e obrigatorio')
-    qs = CelulaDia.objects.filter(origem='gerada', data__gt=hoje)
+    from ponto.management.commands.gerar_celulas import _janela_default
+    ini, fim = _janela_default(hoje)
+    qs = CelulaDia.objects.filter(
+        origem='gerada', data__gt=hoje, data__range=(ini, fim))
```

Comandos propostos para a sessão, no ambiente de testes configurado; resultados e configuração estão **[não verificado]**: [nao verificado]

```bash
# Novos casos: repetir na base para RED, na cura para GREEN e com cada mutante.
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_residuo_alem_da_competencia

# Torneira, com a data que coloca hoje+3 além do fim informado.
PYTHONDONTWRITEBYTECODE=1 python3 -c "from freezegun import freeze_time; import manage; import sys; sys.argv=['manage.py','test','escala.tests.test_torneira_folgas']; f=freeze_time('2026-11-18 15:00:00'); f.start(); manage.main()"
```

Para os demais mutantes, aplicar **um de cada vez** e executar o primeiro comando acima:

1. Em `sincronizar_foto_futura_do_calendario`, retirar `| Q(pk__in=geradas.values('pk'))`: deve atingir o teste de foto distante. [nao verificado]
2. Em `regenerar_futuras_por_feriado`, voltar a limitar `datas` ao `fim` de `_janela_default`: deve atingir o teste de feriado distante. [nao verificado]
3. Em `medir`, substituir a seleção das geradas pelo filtro `data__range=(ini, fim)`: deve atingir o teste do vigia. [nao verificado]

A queda esperada desses mutantes, inclusive a dos dois testes da torneira pelo teto, permanece **[não verificado]**. [nao verificado]

O dado raspado informa, antes da cura, **112 divergências de cadastro além do fim**, em 3.865 células geradas, e 112 no alcance novo total; não houve nova medição aqui **[não verificado na fonte]**. Permanecem incompletos: logs RED/GREEN/mutantes e suíte, medições de custo, dry do passivo, medição posterior e revisão final. O censo e as sondas pedidos não estavam disponíveis nos caminhos da cópia consultados; seu conteúdo além dos anexos fornecidos é **[não verificado]**. [nao verificado]

Mensagem proposta para o commit futuro, sem executá-lo: [nao verificado]

```text
Alcança células futuras existentes nos eventos de cadastro

LEI-AKITA: origem=ponto/services/cadastro_futuro.py (alcance do seletor) e escala/folgas.py (meses da foto pelo mesmo alcance), testemunha=vinculo_do_dia e a porta da celula, RED=test_torneira_folgas (2) + troca de vinculo, folga e foto alem do fim, quem-mais-le=censo residuo_alem_da_competencia.r item (e), juizes novos=0, padrao=projecao reconstruivel

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
```

## 18/11 08:07 · sessao a4ee9aacdb54 · RETOMA-POS-POUSOS-1811 · verificador · origem=Code

| Item | Pergunta | Resultado e motivo |
|---|---|---|
| **1 — P2** | **1. Premissas** | **PROCEDE — há dependência de sítio classe (ii).** Os leitores que prendiam a P2 estão nesta cópia com o comportamento B2: `sem_veredito_entre` chama o juiz; `estado` lê `respondida`; e a property delega ao juiz. O universo da ressuscitação usa `q_nao_respondida` e preserva a exclusão de texto sem instante e sem validação. ([models.py:1530](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1530), [models.py:1541](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1541), [models.py:1931](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1931), [models.py:3162](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3162), [models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674)). Entretanto, a escrita da ressuscitação chama `descarimbar_em_lote` em **A8**, sítio que a matriz fornecida lista como classe (ii); o censo fornecido diz que a escrita compensadora P2 opera nesse universo. A aplicação e o efeito da P2 rebaseada estão **[não verificado]**. ([models.py:1599](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1599), [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637); anexos fornecidos `censo.r` e `MATRIZ.md`, arquivo:linha **[não verificado]**). |
| **1 — P2** | **2. Colisão** | **PROCEDE — a retomada precisa ser devolvida no ramo escrito pelo próprio aval.** A exceção para antecipar a P2 dependia de censo independente; o censo fornecido concluiu dependência. O aval atual manda devolver se houver dependência de sítio classe (ii)/B2b, condição encontrada em A8. A continuação sem resolver essa condição não está autorizada pelo literal. (Anexos fornecidos `libera_b3_f2p2_1758.txt`, item 2, `aval.txt`, item 1, e `MATRIZ.md`, A8; arquivo:linha **[não verificado]**; [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637)). |
| **1 — P2** | **3. Literal** | **PROCEDE — “pouso da B2” não fecha automaticamente com B2a.** O corte anterior condicionava o pouso da B2 a três resultados; o aval posterior dividiu a obra, dispensou os 39 sítios para **B2a** e proibiu declarar a B2 fechada. O relato de B2a no ar e os resultados efetivos das condições de pouso são **[não verificado]**. A contagem da corrida de `respondida_em`, seu universo desde o reload e denominador foram definidos no complemento; N1/N2 e seus valores de produção são **[não verificado]**. (Anexos fornecidos `corte_b2_cerca_no_escritor_1711.txt`, item 6, `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 1, e `complemento1_libera_b3_1808.txt`; arquivo:linha **[não verificado]**). | [nao verificado]
| **1 — P2** | **4. Exportada, dinheiro ou `!`** | **CONFERE quanto ao escopo declarado; efeito efetivo [não verificado].** O aval manda preservar condições anteriores de pouso, inclusive crons drenados e parada se as medidas indicadas forem maiores que zero; não apresenta autorização nova para alterar exportada ou dinheiro. Como a P2 não está aplicada nesta cópia e não há dados de execução, seus efeitos efetivos são **[não verificado]**. (Anexos fornecidos `aval.txt`, item 1, e `libera_b3_f2p2_1758.txt`, item 3; arquivo:linha **[não verificado]**). | [nao verificado]
| **2 — resíduo** | **1. Premissas** | **PROCEDE — o seletor atual tem um detalhe que o novo limite precisa corrigir.** Os dois serviços existem; `cadastro_futuro` seleciona futuras **geradas** existentes, enquanto o seletor da foto seleciona células futuras sem filtrar `origem='gerada'`. Portanto, simplesmente ampliar o teto atual da foto não garante o limite novo de meses com célula gerada. Os meses realmente saem dos dias selecionados; a foto alcança do amanhã ao fim de cada mês selecionado. ([cadastro_futuro.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cadastro_futuro.py:26), [folgas.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:619), [folgas.py:627](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:627)). `_gerar_foto_folgas_no_ato` é chamado diretamente por `gerar_foto_folgas` e pelo sincronizador; o comando `lavrar_foto_folgas` chama `gerar_foto_folgas`, e está configurado como cron às 05:47. Outros chamadores do comando são **[não verificado]**. ([folgas.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:150), [folgas.py:639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:639), [lavrar_foto_folgas.py:170](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/lavrar_foto_folgas.py:170), [crons.py:425](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:425)). |
| **2 — resíduo** | **2. Colisão** | **CONFERE — o texto novo responde expressamente às duas devoluções.** Ele autoriza ampliar os meses entregues à foto, conservando corpo e janela do escritor e do cron; aceita também o dreno por evento, inclusive passivo alcançado. A relavra em lote do restante continua condicionada a dry publicado e `!`. O restante das condições de 01:48 continua vigente. (Anexos fornecidos `aval.txt`, item 2, `devolvido_0150.txt` e `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 3; arquivo:linha **[não verificado]**). | [nao verificado]
| **2 — resíduo** | **3. Literal** | **PROCEDE — “a mesma medida 24 h depois” não está reproduzível com o dado entregue.** O texto dá três números de referência com unidades e universos diferentes: 389 células/12 colaboradores, 26 dias de folga/16 colaboradores e 287 células/8 desligados. Não foram fornecidos os scripts ou saídas das três sondas, suas consultas, denominadores completos, regras de deduplicação ou o instante de cada medida; a repetição exata e a comparabilidade dos universos após 24 horas ficam **[não verificado]**. O `!` permanece para o **lote do que sobrar**, não para os eventos aceitos; o que efetivamente sobrará é **[não verificado]**. A janela atual é a união das competências correntes das empresas ativas, portanto seu fim não é necessariamente o da empresa do colaborador. (Anexos fornecidos `aval.txt`, item 2, e `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 3; arquivo:linha **[não verificado]**; [gerar_celulas.py:290](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/gerar_celulas.py:290)). |
| **2 — resíduo** | **4. Exportada, dinheiro ou `!`** | **CONFERE quanto ao `!` declarado; efeitos reais [não verificado].** O dreno por evento sem `!`, inclusive sobre passivo alcançado, foi explicitamente aceito. A porta atual barra células em competência exportada e pode alterar `trabalha`, geradora e DNA das geradas alcançadas; a mudança proposta ainda não existe nesta cópia. A foto ampliada também grava ou apaga `FolgaDia` nas datas futuras dos meses selecionados, e seu efeito financeiro concreto é **[não verificado]**. (Anexo fornecido `aval.txt`, item 2(B), arquivo:linha **[não verificado]**; [celula.py:284](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:284), [celula.py:297](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:297), [folgas.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:180)). |

**Quem escreve e quem lê além da competência**, no alcance identificado pelo código e pelo censo fornecido:

| Dado | Escritores | Leitores |
|---|---|---|
| **FolgaDia** | A foto de calendário reconcilia e grava registros; há também lançamento manual, lote e reparentamento. A relação completa de escritores é **[não verificado]**. ([folgas.py:166](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:166), [folgas.py:329](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:329), [folgas.py:405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:405), [folgas.py:513](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:513)). | `folgas_lancadas_do_periodo` alimenta a precedência; o cálculo da escala também consulta a foto do vínculo, e o Plano de Folgas lê a folga gravada. ([precedencia.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/precedencia.py:195), [models.py:1249](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1249), [views_plano_folgas.py:60](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/views_plano_folgas.py:60)). |
| **CelulaDia** | A porta `regenerar_celulas_vinculo` cria ou reescreve geradas; `editar_dia` cria célula humana; `gerar_celulas_periodo` cria ausentes. A seleção atual de eventos de cadastro limita a relavra futura à janela corrente. ([celula.py:289](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:289), [celula.py:631](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:631), [celula.py:883](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:883), [cadastro_futuro.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cadastro_futuro.py:26)). | `eh_dia_trabalho` lê o booleano da célula, usado pela Minha Escala; calendário/status pode chegar à folga pela precedência. O auditor C5 cruza FolgaDia e célula sem teto futuro indicado no censo. ([models.py:1092](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1092), [views.py:38](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/views.py:38), [status_dia.py:296](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/status_dia.py:296), [auditar_invariantes_chamados.py:1536](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1536)). |

A identificação desta cópia como o hash informado e o estado de produção B2a/S6 estão **[não verificado]**: a cópia consultada não dispõe de metadados Git, e não houve acesso a banco ou logs.

## 18/11 07:46 · sessao 756f54b454dc · E4C-F1-B2b · leitura · origem=Code

| Resultado da leitura | Situação |
|---|---|
| Censo exaustivo de escritas sem guarda | **[não verificado]**. Localizei os caminhos abaixo, inclusive uma migração que a leitura anterior não havia identificado. Falta resolver todos os aliases e chamadas indiretas no universo pesquisado; portanto, não afirmo zero para os demais caminhos. |
| Matriz dos 39 sítios | **[não verificado]**. O anexo disponibilizado é identificado como volta 8 e contém 55 linhas, inclusive linhas sobrepostas e linhas com partição própria. Não consegui estabelecer com segurança a correspondência individual com os 39 da v7 nem concluir todas as cadeias e custos. A matriz abaixo contém somente os sítios cuja evidência nesta árvore consegui conferir. A referência da própria árvore aos 39 está em [fio.py:1397](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1397). |
| Escritores e leitores completos dos cinco campos | **[não verificado]**. A matriz de campos traz achados confirmados, mas não é uma lista completa; faltou rastrear todos os usos ORM, relações, aliases e consumidores. |
| Identificação do sítio para contar | A guarda atual recebe somente `perguntas`; o estado da cerca contém `vetadas`, `memo` e `empresa`, sem identificador do chamador. Assim, hoje não há dado geral na assinatura ou nesse estado para nomear o sítio sem inspecionar a pilha. [fio.py:1389](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1389), [fio.py:1445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1445). |
| Identidade da árvore | **[não verificado]**: o diretório disponibilizado não contém metadados Git para confirmar independentemente o hash informado. |

| Busca declarada para o censo | Resultado e limite |
|---|---|
| Em `app/`, buscas textuais por `PerguntaDisputa`, `perguntadisputa`, relações como `perguntas_que_resolvem`, `.update(`, `bulk_update`, `bulk_create`, `.create(`, `.raw(`, `cursor`, SQL `UPDATE`/`INSERT`, construtores e `_state.adding`; inspeção manual dos resultados relevantes. | A busca direta encontrou o lote de lavras, o lote guardado e a migração por modelo histórico abaixo. **Não prova exaustividade**: atribuição por alias, SQL montado dinamicamente, managers relacionados e objetos passados entre funções requerem rastreamento adicional. O próprio código documenta essa limitação para `qs.update()`: [models.py:3401](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3401). |

| Escrita localizada | Campos e chamador | Passa pela guarda? |
|---|---|---|
| [veredito_lavrado.py:250](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:250), `bulk_update` | `veredito_lavrado`, `julgado_em`, `janela_expira_em`; comando [backfill_veredito_lavrado.py:143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_veredito_lavrado.py:143), inclusive sobra em linha 146. Campos: [veredito_lavrado.py:237](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:237). | **Não**: o corpo do lote chama `bulk_update` diretamente, sem a guarda. |
| [0051_decisao_pergunta.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:25), `PD.objects.update` | `colaborador_id`, derivado da disputa; chamada pela operação de migração [0051_decisao_pergunta.py:101](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:101). | **Não**: `PD` é modelo histórico obtido por `apps.get_model`, e a operação usa `update`. Se essa migração já rodou no ambiente de produção: **[não verificado]**. [0051_decisao_pergunta.py:22](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:22). |
| [0051_decisao_pergunta.py:27](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:27), `PD.objects.filter(...).update` | `instancia_marco`; mesma operação de migração. [0051_decisao_pergunta.py:22](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:22), [0051_decisao_pergunta.py:101](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:101). | **Não**, pelo mesmo caminho de modelo histórico e `update`. |
| [models.py:3135](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3135), `save(force_insert=True)` | Inserção dos campos da instância, incluindo disputa, motivo, destino, célula, conversa, dia e `**campos`; há entradas de emissão e declaração humana em [disputa_emissao.py:453](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:453) e [declaracao_texto.py:377](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:377). Outros chamadores: **[não verificado]**. | **Não**: `save` chama a guarda somente se `not self._state.adding`. [models.py:3562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3562). |
| [models.py:3405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3405), `update` de descarimbo | Zera `materializada_em`, `materializada_por`, `via_resolucao` e grava `**outros`; exemplos de chamadores em [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637) e [disputa_emissao.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1649). | **Sim**, explicitamente antes do `update`; fora da cerca a chamada retorna sem julgar. [models.py:3398](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3398), [fio.py:1402](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1402). |
| Demais `update`, `bulk_update`, `bulk_create`, SQL cru, inserções por outros managers e aliases | **[não verificado]**: não foi demonstrado zero de chamadores nem completado o rastreamento necessário para listar todos. | **[não verificado]**. |

| Regra comum para ler a matriz de sítios | Evidência |
|---|---|
| **Porta** significa chegada à guarda, não julgamento atual: fora da cerca ela retorna antes de consultar. Dentro, compara dia em memória e dia gravado para perguntas que não são conversa; `None` no conjunto é indecidível e a guarda trata o resultado como veto. [fio.py:1402](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1402), [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409), [fio.py:1358](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1358). |
| **Hipótese de recusa** exige uma implementação que crie estado válido fora da cerca. Retirar somente o `return` não basta: o restante acessa `estado['empresa']`, `estado['memo']` e `estado['vetadas']`. A guarda lança antes do `atomic` interno do `save`. [fio.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1420), [models.py:3562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3562). |
| **Custo**: passando a julgar, a forma atual consulta as linhas gravadas e pode consultar empresa e competência; o custo exato por caminho e quais tetos falhariam são **[não verificado]**, pois dependem da implementação e de medição. Há teto com folga zero para `reconciliar (gate da premissa, FECHA com dia e celula)`, e o selo compara consultas medidas ao teto. [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409), [test_selo_chokepoint_escrita.py:134](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_selo_chokepoint_escrita.py:287). |

| Sítio conferido nesta árvore | Porta e sinal acima dela | Se julgar sempre e recusar |
|---|---|---|
| **A5** relavra de destino: [models.py:307](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:307) | Passa pelo `save` da lavra; acima há `self.pk` do chamado e o laço das perguntas de destino. [models.py:309](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:309), [veredito_lavrado.py:275](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:275). | O savepoint da relavra é desfeito e `engolir` trata a falha; a transição anterior está no atomic externo. Efeitos adicionais de receivers e teto atingido: **[não verificado]**. [models.py:280](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:280), [models.py:307](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:307). |
| **A8** ressuscitar: [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637) | Passa pela guarda de lote; o chamado e o conjunto `alvo` estão acima dela. [models.py:1606](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1606), [models.py:3398](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3398). | Se a exceção propagar, o atomic de [models.py:1613](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1613) desfaz as decisões anteriores da unidade e impede descarimbo, despertar e reabertura subsequentes. Capturas externas e custo: **[não verificado]**. |
| **B1a** absolvida por célula: [reconciliador.py:639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:639) | Passa por `carimbar` e `save`; o laço usa usuário de sistema. [reconciliador.py:628](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:628), [models.py:3222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3222). | O atomic por pergunta é desfeito; o `except` registra e segue o laço. Outros efeitos e custo: **[não verificado]**. [reconciliador.py:636](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:636), [reconciliador.py:645](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:645). |
| **B3a** órfã pela lâmpada: [reconciliador.py:1192](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1192) | Passa por `encerrar_fantasma` e `carimbar`; há usuário e contexto do reconciliador acima. [models.py:3419](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3419), [reconciliador.py:1186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1186). | A exceção no carimbo desfaz o atomic da pergunta e impede o evento seguinte; propagação até a entrada e custo: **[não verificado]**. [reconciliador.py:1191](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1191). |
| **B4** reaponte: [fio.py:974](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:974) | O `save` passa; a função recebe `p`, `chamado` e `data_evento`, sem origem ou usuário. Também é chamada por declaração humana, portanto sua presença isolada não identifica sistema. [fio.py:853](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:853), [declaracao_texto.py:371](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:371). | A recusa impede o `save`; efeitos e atomic de cada chamador, inclusive perdedoras e eventual data atribuída em memória: **[não verificado]**. |
| **B6** lote de lavras: [veredito_lavrado.py:250](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:250) | **Não passa** pela guarda atual. O comando chamador é sinal de execução de sistema. [backfill_veredito_lavrado.py:143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_veredito_lavrado.py:143). | Uma mudança apenas na guarda de `save` não o alcança; custo e recusa para um eventual julgamento do lote: **[não verificado]**. |
| **B7** relavra de via: [relavra_via.py:127](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/relavra_via.py:127) | Passa; `ator` e `origem` existem acima, mas não são parâmetros da guarda. [relavra_via.py:106](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/relavra_via.py:106). | A exceção interrompe o laço se não for capturada acima. Atomic externo, reversão dos itens anteriores e custo em cada entrada: **[não verificado]**. |
| **P1** reuso: [models.py:3124](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3124) | Passa no `save` de instância existente; há entradas tanto de emissão quanto de declaração humana. [disputa_emissao.py:453](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:453), [declaracao_texto.py:377](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:377). | O savepoint do contrato de abertura é relevante, mas a propagação e o efeito integral por entrada são **[não verificado]**. [models.py:2919](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2919). |
| **P11** pendência: [materializacao.py:1353](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1353) | Passa; `_materializar_uma` recebe disputa, pergunta e usuário, e o laço de materialização tem atomic por pergunta. [materializacao.py:1833](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1833), [materializacao.py:2098](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2098). | A exceção pode entrar no tratamento da materialização e provocar nova tentativa de gravar recusa no `finally`; o resultado completo dessa segunda recusa e o custo são **[não verificado]**. [materializacao.py:1978](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1978), [materializacao.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1990). |
| **P15** carimbo do plantio: [materializacao.py:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1968) | Passa pelo `save` de `carimbar`; o contexto inclui `request_user` e o atomic do plantio. [materializacao.py:1911](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1911). | A exceção desfaz o atomic do plantio; o tratamento acrescenta erro e o `finally` tenta carimbar a recusa. O efeito da segunda exceção e o custo: **[não verificado]**. [materializacao.py:1978](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1978), [materializacao.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1990). |
| **X1 / A9c** S144: [materializacao.py:2167](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2167) | Passa; o signal é disparado pelo fechamento de disputa e recebe `fechada_por`, que pode ter origem humana ou de sistema. [signals.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:150), [signals.py:165](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:165). | O signal captura a exceção e interrompe sua materialização; **sob cerca**, a anotação faz o abridor desfazer o ato inteiro. Fora dela, com uma hipotética guarda independente, o efeito completo da transação e dos carimbos anteriores é **[não verificado]**. [signals.py:166](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:166), [fio.py:1449](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1449). |

| Campo | Escritores e leitores confirmados; lacuna |
|---|---|
| `data_evento` | `gravar_dia_do_fato` atribui o dia em memória; nascimento usa essa função antes do insert; a guarda lê instância e linha gravada. [fio.py:849](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:849), [models.py:3132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3132), [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409). Para fato, o juiz o define como **dia do turno**; para conversa, o mesmo campo guarda **dia civil** e o juiz retorna `None`. [juizes.py:338](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:338). Lista completa e divergências adicionais: **[não verificado]**. |
| `conversa` | `abrir` passa `False` e `abrir_conversa` passa `True`; esta usa `localdate()`. A guarda discrimina tanto a instância quanto o valor gravado. [models.py:2902](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2902), [models.py:2914](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2914), [fio.py:1414](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1414). Lista completa: **[não verificado]**. |
| `chamado_destino` | Reaponte e reuso atribuem o campo; a relavra filtra perguntas por destino. [fio.py:974](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:974), [models.py:3100](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3100), [models.py:309](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:309). Lista completa: **[não verificado]**. |
| `materializada_em` | O carimbo atribui instante; descarimbo em lote o zera; S144 seleciona as perguntas em que está nulo. [models.py:3216](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3216), [models.py:3405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3405), [materializacao.py:2154](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2154). Lista completa: **[não verificado]**. |
| `disputa.colaborador` | A guarda lê `q.disputa.colaborador_id` para buscar a empresa; a migração histórica copia esse identificador para a coluna desnormalizada da pergunta. [fio.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1420), [0051_decisao_pergunta.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:25). Todos os escritores/leitores da FK da disputa e possíveis divergências: **[não verificado]**. |
| Fuso | A configuração declara `America/Sao_Paulo` e `USE_TZ=True`; o dia da conversa nasce de `localdate()`. Conversões específicas de todos os leitores de instante e eventuais diferenças de fuso: **[não verificado]**. [base.py

[truncado em 20000 de 21334 caracteres]

## 18/11 07:05 · sessao b3c6012a4dc8 · S6-INTERMITENTE-FERIADO-CURA-A · sonda · origem=Code

[Não verificado] Saída da sonda conforme o dado fornecido:

```text
sonda_leitura: script.py [a2fb8b54fbba6929] no cpuset 4-7, lendo sombra como leitor (LEITURA)

MOVEU emp 2 cod 1401 pk 365 [('intermitente', 'intermitente', '2026-07-21', 'None')]
MOVEU emp 2 cod 1995 pk 830 [('intermitente', 'intermitente', '2026-07-21', 'None')]
MOVEU emp 3 cod 197 pk 146 [('intermitente', 'intermitente', '2026-06-24', 'None')]
MOVEU emp 3 cod 450 pk 85 [('intermitente', 'intermitente', '2026-07-22', 'None')]
FIM
rc_sombra=0
```

## 18/11 07:01 · sessao ea69b279ba00 · R1-REMEDE-1811 · censo · origem=Code

**O código não mostra uma diferença de universo, filtro ou unidade de contagem que, por si só, explique 26 × 33.** Se ambas as execuções percorreram os mesmos colaboradores e produziram os mesmos julgamentos, a soma das seis partes e a contagem das linhas `ESTRUTURA` do CSV devem coincidir. A causa concreta dos sete dias de diferença é **[não verificado]**. [app/ponto/management/commands/e6_oraculo.py:314](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:314), [app/ponto/management/commands/e6_oraculo.py:378](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:378), [app/ponto/management/commands/e6_oraculo.py:401](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:401)

- **Universo.** O comando seleciona empresas ativas e colaboradores com `FechamentoMensal` da competência; só aplica o filtro de entrada no TXT quando recebe `so_txt`. Os dois scripts fornecidos chamam sem esse filtro. A sonda intercepta a seleção e a divide por `pk % 6`; se as seis partes cobrem os resíduos e terminam, essa divisão cobre o mesmo universo de colaboradores. [app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:303); anexo da sonda, `filtrar_colaboradores` e chamada de `e6_oraculo` **[linha não verificada]**; anexo da lista, chamada de `e6_oraculo` **[linha não verificada]**.
- **Filtro de dias.** O mesmo laço percorre a janela da empresa e exclui, entre outros, dias sem linha de espelho, isentos ou com ausência, sem minutos disponíveis, sem trabalho em ambos e diferenças explicadas pela janela declarada. Diferenças de até 10 minutos também não entram como divergência. A opção CSV não altera esses filtros. [app/ponto/management/commands/e6_oraculo.py:331](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:331), [app/ponto/management/commands/e6_oraculo.py:342](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:342), [app/ponto/management/commands/e6_oraculo.py:365](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:365), [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:442).
- **Contagem.** Para cada divergência julgada, o comando incrementa uma vez `T['dono_…']`, inclui o colaborador no conjunto do dono e acrescenta uma linha a `div` com esse mesmo dono. O CSV grava `div`; portanto, nesse caminho, uma linha `ESTRUTURA` corresponde a um incremento `ESTRUTURA`. Colaboradores são contados distintamente por conjunto, não por linha. [app/ponto/management/commands/e6_oraculo.py:378](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:378), [app/ponto/management/commands/e6_oraculo.py:401](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:401), [app/ponto/management/commands/e6_oraculo.py:433](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:433), [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:442).
- **Caminhos de execução.** A sonda copia os acumuladores do próprio `handle`, mas também instala interceptações, limpa cache e envolve chamadas do espelho em transações com rollback; a lista chama o comando inteiro com CSV. Além disso, cada chamada do comando captura seu próprio instante e deriva dele `HOJE`; o mesmo dump, por si só, não comprova instantes ou resultados intermediários iguais. Se alguma dessas diferenças afetou esta medição, **[não verificado]**. Anexo da sonda, `copiar_acumuladores`, `limpar` e `observar_espelho` **[linha não verificada]**; [app/ponto/management/commands/e6_oraculo.py:284](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:284), [app/ponto/management/commands/e6_oraculo.py:316](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:316).

**Autoridade:** a tabela e o CSV são duas saídas do mesmo julgamento do `e6_oraculo`; o código não estabelece que uma delas prevaleça sobre a outra quando vêm de execuções diferentes. O R1 em `placar_estrutural.py` declara como fonte medida arquivos `logs/r1remede/r1_dono_*.txt` e identifica a tabela do comando como fonte; o registro exibido no arquivo descreve uma medição anterior em seis partes, não uma leitura direta do CSV desta lista. [app/core/placar_estrutural.py:123](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:123), [app/core/placar_estrutural.py:130](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:130), [app/core/placar_estrutural.py:133](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:133).

A saída `AUTORIDADE` da execução da lista e os resultados individuais das seis partes não foram fornecidos; assim, não é possível conferir o contador da tabela contra o CSV na própria execução, nem reconciliar os colaboradores e dias entre execuções. **[não verificado]** Os scripts constam no texto fornecido, mas seus arquivos não estão nesta cópia; por isso não pude verificar seus números de linha. **[não verificado]**

## 18/11 05:55 · sessao b3b854808243 · R1-REMEDE-1811 · sonda · origem=Code

Transcrição do trecho `CAB` até `FIM_LISTA`, conforme o dado fornecido. Arquivo e linhas de origem: [não verificado].

```text
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 3|110|2026-11-12|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|112|2026-10-21|ESTRUTURA|546|838|838|292|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|112|2026-10-22|ESTRUTURA|541|835|835|294|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|113|2026-10-30|ESTRUTURA|242|0|0|-242|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|113|2026-11-01|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|114|2026-11-08|ESTRUTURA|439|429|499|-10|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|114|2026-11-15|ESTRUTURA|499|440|499|-59|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|130|2026-10-29|ESTRUTURA|335|672|730|337|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|200|2026-10-29|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|200|2026-11-02|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|218|2026-11-14|ESTRUTURA|103|0|0|-103|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|220|2026-11-10|ESTRUTURA|337|430|486|93|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-09|ESTRUTURA|551|597|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-10|ESTRUTURA|551|598|611|47|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-11|ESTRUTURA|550|598|611|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-13|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-17|ESTRUTURA|553|598|612|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|296|2026-11-02|ESTRUTURA|345|0|0|-345|e6_zero_esp_trabalho|par|0|trabalhou |n_batidas=0
ESTRUTURA 2|334|2026-11-16|ESTRUTURA|209|591|683|382|diverge_acima_60|par|3|folga |n_batidas=4
ESTRUTURA 4|37|2026-11-14|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|382|2026-11-10|ESTRUTURA|418|0|0|-418|e6_zero_esp_trabalho|par|3|trabalhou |n_batidas=0
ESTRUTURA 2|516|2026-10-23|ESTRUTURA|361|0|0|-361|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|727|2026-11-10|ESTRUTURA|307|0|0|-307|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|841|2026-11-16|ESTRUTURA|178|0|0|-178|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|874|2026-10-22|ESTRUTURA|667|383|383|-284|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|884|2026-11-08|ESTRUTURA|679|699|739|20|diverge_10_60|par|4|rejeitado |n_batidas=4
ESTRUTURA 3|909|2026-11-11|ESTRUTURA|831|245|374|-586|diverge_acima_60/FORA|IMPAR|CORTE_do_oraculo|em_aberto |n_batidas=4
ESTRUTURA 2|922|2026-11-01|ESTRUTURA|1382|68|68|-1314|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|922|2026-11-04|ESTRUTURA|676|458|458|-218|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 1|950|2026-11-11|ESTRUTURA|480|240|240|-240|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|989|2026-10-23|ESTRUTURA|721|567|567|-154|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|996|2026-10-28|ESTRUTURA|0|580|580|580|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 3|996|2026-11-10|ESTRUTURA|443|1026|1116|583|diverge_acima_60|par|5|em_aberto |n_batidas=6
FIM_LISTA
```

## 18/11 05:50 · sessao d3575bb42232 · R1-REMEDE-1811 · sonda · origem=Code

As seis linhas `SOMA` terminam em `True`. Não há `INCOMPLETO` na saída fornecida. A verificação em arquivo é [não verificado].

```text
sonda_leitura: script.py [455c4e7660bc3a87] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 6 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 100
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 199 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 199 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1298, 'bate_ate_10min': 1258, 'esp_zero_e6_trabalho': 5, 'e6_zero_esp_trabalho': 2, 'diverge_10_60': 1, 'diverge_acima_60': 32, 'dia_batida_impar': 55, 'impar_que_BATE': 27, 'impar_que_DIVERGE': 28, 'impar_dentro_da_faixa': 25, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 28, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1396, 'dia_isento_ou_ausencia': 277, 'dia_sem_trabalho_ambos': 11, 'esp_desconta_janela_declarada': 18, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.9% de 1298 dias comparados
[3] ESTRUTURA     2 dia-colab       8.2 h    1 colabs
[3] CADASTRO      8 dia-colab      40.5 h    2 colabs
[3] BATIDA       30 dia-colab     127.8 h   17 colabs
[4] TOTAL divergentes 40
[5] colabs com divergencia: 20
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=2|horas=8.15|colabs=1
PARCIAL|10/2026|CADASTRO|dias=8|horas=40.4628|colabs=2
PARCIAL|10/2026|BATIDA|dias=30|horas=127.75368121722221|colabs=17
PARCIAL_TOTAL|10/2026|dias=40|horas=176.36648121722221|colabs=20
ESTIMATIVA processados 100 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1206, 'bate_ate_10min': 1163, 'esp_zero_e6_trabalho': 3, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 2, 'diverge_acima_60': 34, 'dia_batida_impar': 64, 'impar_que_BATE': 33, 'impar_que_DIVERGE': 31, 'impar_dentro_da_faixa': 26, 'impar_FORA_da_faixa': 5, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 198, 'dia_sem_lavratura_no_espelho': 1343, 'dia_isento_ou_ausencia': 288, 'dia_sem_trabalho_ambos': 23, 'esp_desconta_janela_declarada': 11, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.4% de 1206 dias comparados
[3] ESTRUTURA     3 dia-colab      10.6 h    2 colabs
[3] CADASTRO      7 dia-colab      65.8 h    5 colabs
[3] BATIDA       33 dia-colab     166.7 h   22 colabs
[4] TOTAL divergentes 43
[5] colabs com divergencia: 26
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=3|horas=10.610499166666667|colabs=2
PARCIAL|11/2026|CADASTRO|dias=7|horas=65.7971625|colabs=5
PARCIAL|11/2026|BATIDA|dias=33|horas=166.71064350722222|colabs=22
PARCIAL_TOTAL|11/2026|dias=43|horas=243.118305173888887|colabs=26
TEMPO_SEGUNDOS 162.1
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 241905
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 80
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1406
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 4830
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 58
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 10
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6629
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3848
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3848
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 626
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 180
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 455936
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 544
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3097
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 183
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13056
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 93
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4326
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 189
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 21916
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4418
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 39962
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 4
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 17580
SOMA 10/2026 40   <- tem de ser igual a soma das classes (40) True
SOMA 10/2026 HORAS 176.36648121722221 True
SOMA 10/2026 COLABS_UNIAO 20 True
SOMA 11/2026 43   <- tem de ser igual a soma das classes (43) True
SOMA 11/2026 HORAS 243.118305173888887 True
SOMA 11/2026 COLABS_UNIAO 26 True
rc_sombra=0
```

## 18/11 05:46 · sessao 6801b2f64f9b · R1-REMEDE-1811 · sonda · origem=Code

Transcrição da saída fornecida — origem e conteúdo independentes não verificados:

```text
sonda_leitura: script.py [972aacf9a2766647] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 5 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 107
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 103
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 210 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 210 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1285, 'bate_ate_10min': 1215, 'esp_zero_e6_trabalho': 9, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 3, 'diverge_acima_60': 55, 'dia_batida_impar': 78, 'impar_que_BATE': 29, 'impar_que_DIVERGE': 49, 'impar_dentro_da_faixa': 42, 'impar_FORA_da_faixa': 7, 'impar_no_conjunto_do_dia': 49, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1533, 'dia_isento_ou_ausencia': 275, 'dia_sem_trabalho_ambos': 45, 'esp_desconta_janela_declarada': 72, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1285 dias comparados
[3] ESTRUTURA     3 dia-colab      29.2 h    3 colabs
[3] CADASTRO     19 dia-colab      52.3 h    8 colabs
[3] BATIDA       48 dia-colab     197.3 h   26 colabs
[4] TOTAL divergentes 70
[5] colabs com divergencia: 29
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=29.23270916666667|colabs=3
PARCIAL|10/2026|CADASTRO|dias=19|horas=52.325450786111105|colabs=8
PARCIAL|10/2026|BATIDA|dias=48|horas=197.29829774694443|colabs=26
PARCIAL_TOTAL|10/2026|dias=70|horas=278.856457699722205|colabs=29
ESTIMATIVA processados 125 universo 210 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1236, 'bate_ate_10min': 1146, 'esp_zero_e6_trabalho': 5, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 6, 'diverge_acima_60': 75, 'dia_batida_impar': 98, 'impar_que_BATE': 37, 'impar_que_DIVERGE': 61, 'impar_dentro_da_faixa': 52, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 61, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 206, 'dia_sem_lavratura_no_espelho': 1402, 'dia_isento_ou_ausencia': 227, 'dia_sem_trabalho_ambos': 38, 'esp_desconta_janela_declarada': 84, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 92.7% de 1236 dias comparados
[3] ESTRUTURA     9 dia-colab      60.5 h    7 colabs
[3] CADASTRO     31 dia-colab     104.1 h   11 colabs
[3] BATIDA       50 dia-colab     255.3 h   25 colabs
[4] TOTAL divergentes 90
[5] colabs com divergencia: 35
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=9|horas=60.54144059916667|colabs=7
PARCIAL|11/2026|CADASTRO|dias=31|horas=104.14722180555555|colabs=11
PARCIAL|11/2026|BATIDA|dias=50|horas=255.3167716316666|colabs=25
PARCIAL_TOTAL|11/2026|dias=90|horas=420.00543403638882|colabs=35
TEMPO_SEGUNDOS 184.8
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 278619
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 34
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1955
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 8224
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 80
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 14
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 8043
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 5767
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 5767
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 350
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 193
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 505992
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 861
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3482
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 218
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 14496
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 171
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4379
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 199
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 28919
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 3793
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 45361
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 4
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 21691
SOMA 10/2026 70   <- tem de ser igual a soma das classes (70) True
SOMA 10/2026 HORAS 278.856457699722205 True
SOMA 10/2026 COLABS_UNIAO 29 True
SOMA 11/2026 90   <- tem de ser igual a soma das classes (90) True
SOMA 11/2026 HORAS 420.00543403638882 True
SOMA 11/2026 COLABS_UNIAO 35 True
rc_sombra=0
```

As seis linhas **SOMA** estão marcadas como `True` no dado fornecido. Não aparece `INCOMPLETO` na saída fornecida. A execução e sua origem não foram verificadas independentemente. [não verificado]

## 18/11 05:42 · sessao b42c501a0c3b · R1-REMEDE-1811 · sonda · origem=Code

Saída da sonda, conforme o dado fornecido [nao verificado]:

```text
sonda_leitura: script.py [22b7b39d0d46bdd1] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 4 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 96
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 94
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 190 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 190 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1235, 'bate_ate_10min': 1188, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 1, 'diverge_10_60': 5, 'diverge_acima_60': 35, 'dia_batida_impar': 54, 'impar_que_BATE': 21, 'impar_que_DIVERGE': 33, 'impar_dentro_da_faixa': 29, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 32, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1275, 'dia_isento_ou_ausencia': 315, 'dia_sem_trabalho_ambos': 18, 'esp_desconta_janela_declarada': 37, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.2% de 1235 dias comparados
[3] ESTRUTURA     3 dia-colab      15.6 h    3 colabs
[3] CADASTRO     23 dia-colab     107.6 h    8 colabs
[3] BATIDA       21 dia-colab      97.0 h   16 colabs
[4] TOTAL divergentes 47
[5] colabs com divergencia: 25
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.566319135833336|colabs=3
PARCIAL|10/2026|CADASTRO|dias=23|horas=107.60888681222221|colabs=8
PARCIAL|10/2026|BATIDA|dias=21|horas=97.03832052916665|colabs=16
PARCIAL_TOTAL|10/2026|dias=47|horas=220.213526477222196|colabs=25
ESTIMATIVA processados 100 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 190 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1069, 'bate_ate_10min': 1036, 'esp_zero_e6_trabalho': 8, 'e6_zero_esp_trabalho': 0, 'diverge_10_60': 0, 'diverge_acima_60': 25, 'dia_batida_impar': 54, 'impar_que_BATE': 29, 'impar_que_DIVERGE': 25, 'impar_dentro_da_faixa': 21, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 24, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 188, 'dia_sem_lavratura_no_espelho': 1319, 'dia_isento_ou_ausencia': 280, 'dia_sem_trabalho_ambos': 27, 'esp_desconta_janela_declarada': 31, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.9% de 1069 dias comparados
[3] ESTRUTURA     1 dia-colab       9.8 h    1 colabs
[3] CADASTRO      4 dia-colab      16.4 h    2 colabs
[3] BATIDA       28 dia-colab     170.3 h   13 colabs
[4] TOTAL divergentes 33
[5] colabs com divergencia: 15
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=1|horas=9.770812222222222|colabs=1
PARCIAL|11/2026|CADASTRO|dias=4|horas=16.358279375833334|colabs=2
PARCIAL|11/2026|BATIDA|dias=28|horas=170.27799270138888|colabs=13
PARCIAL_TOTAL|11/2026|dias=33|horas=196.407084299444436|colabs=15
TEMPO_SEGUNDOS 162.6
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 244200
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 605
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1215
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6259
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 50
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 1
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 36
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 7089
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3868
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3868
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 344
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 171
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 1246
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 453258
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 653
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3025
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13120
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 109
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4097
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 180
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 25134
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 3863
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 40470
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 19410
SOMA 10/2026 47   <- tem de ser igual a soma das classes (47) True
SOMA 10/2026 HORAS 220.213526477222196 True
SOMA 10/2026 COLABS_UNIAO 25 True
SOMA 11/2026 33   <- tem de ser igual a soma das classes (33) True
SOMA 11/2026 HORAS 196.407084299444436 True
SOMA 11/2026 COLABS_UNIAO 15 True
rc_sombra=0
```

As seis linhas **SOMA** estão marcadas como `True`. Não há ocorrência de `INCOMPLETO` na saída fornecida. [nao verificado]

## 18/11 05:39 · sessao 9e4f22a34972 · R1-REMEDE-1811 · sonda · origem=Code

Todas as seis linhas `SOMA` terminam em `True`. Não há `INCOMPLETO` no trecho fornecido. [nao verificado]

Abaixo está a saída fornecida para a parte 3 de 6, sem resumo; ela não tem arquivo e linha identificados no material recebido. [nao verificado]

```text
sonda_leitura: script.py [c3a2fcb8e125469d] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 3 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 102
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 100
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 202 minutos_total 3.3 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 202 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 202 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1236, 'bate_ate_10min': 1169, 'esp_zero_e6_trabalho': 12, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 2, 'diverge_acima_60': 47, 'dia_batida_impar': 65, 'impar_que_BATE': 23, 'impar_que_DIVERGE': 42, 'impar_dentro_da_faixa': 33, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 42, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1421, 'dia_isento_ou_ausencia': 342, 'dia_sem_trabalho_ambos': 22, 'esp_desconta_janela_declarada': 39, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1236 dias comparados
[3] ESTRUTURA     3 dia-colab      15.2 h    3 colabs
[3] CADASTRO      7 dia-colab      35.9 h    4 colabs
[3] BATIDA       57 dia-colab     297.7 h   24 colabs
[4] TOTAL divergentes 67
[5] colabs com divergencia: 29
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.209666666666665|colabs=3
PARCIAL|10/2026|CADASTRO|dias=7|horas=35.88209174|colabs=4
PARCIAL|10/2026|BATIDA|dias=57|horas=297.66026132194446|colabs=24
PARCIAL_TOTAL|10/2026|dias=67|horas=348.752019728611125|colabs=29
ESTIMATIVA processados 125 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1158, 'bate_ate_10min': 1099, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 7, 'diverge_10_60': 3, 'diverge_acima_60': 43, 'dia_batida_impar': 58, 'impar_que_BATE': 20, 'impar_que_DIVERGE': 38, 'impar_dentro_da_faixa': 30, 'impar_FORA_da_faixa': 8, 'impar_no_conjunto_do_dia': 37, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 200, 'dia_sem_lavratura_no_espelho': 1448, 'dia_isento_ou_ausencia': 217, 'dia_sem_trabalho_ambos': 24, 'esp_desconta_janela_declarada': 53, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.9% de 1158 dias comparados
[3] ESTRUTURA     7 dia-colab      21.8 h    6 colabs
[3] CADASTRO     10 dia-colab      35.9 h    6 colabs
[3] BATIDA       42 dia-colab     209.6 h   22 colabs
[4] TOTAL divergentes 59
[5] colabs com divergencia: 28
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=7|horas=21.8227|colabs=6
PARCIAL|11/2026|CADASTRO|dias=10|horas=35.92035111111112|colabs=6
PARCIAL|11/2026|BATIDA|dias=42|horas=209.5975820575|colabs=22
PARCIAL_TOTAL|11/2026|dias=59|horas=267.34063316861112|colabs=28
TEMPO_SEGUNDOS 172.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 261678
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 278
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1738
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6017
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 41
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6974
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4738
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4738
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 620
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 183
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 17
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 488349
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 550
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3269
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 176
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13326
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 111
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4282
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 193
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 24759
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4929
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 41466
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 18839
SOMA 10/2026 67   <- tem de ser igual a soma das classes (67) True
SOMA 10/2026 HORAS 348.752019728611125 True
SOMA 10/2026 COLABS_UNIAO 29 True
SOMA 11/2026 59   <- tem de ser igual a soma das classes (59) True
SOMA 11/2026 HORAS 267.34063316861112 True
SOMA 11/2026 COLABS_UNIAO 28 True
rc_sombra=0
```

_6790 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
