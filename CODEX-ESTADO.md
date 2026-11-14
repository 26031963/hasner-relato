# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 09:08** · publicado em **14/11/2026 09:08** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 09:08 · sessao 4cb2a70f5db3 · COERENCIA-DAS-LEIS · censo · origem=Code

app/ponto/turnos.py::turnos_do_colab ; a que turno (data_turno) pertence a batida? (app/core/juizes.py:208) ; L-085, jornada pertence ao dia de início (app/docs/LEIS.md:117), L-087, papel pela ata (app/docs/LEIS.md:119), autoridade reafirmada em app/docs/CORTES.md:63 ; carrega batidas não retratadas, determina vínculo por dia e pareia por segmento (app/ponto/turnos.py:1863, app/ponto/turnos.py:1960, app/ponto/turnos.py:1992), atribui data local da entrada ou saída órfã, podendo transportar à véspera pelo envelope noturno (app/ponto/turnos.py:285, app/ponto/turnos.py:297, app/ponto/turnos.py:301) ; sim: pareador nunca decide papel pelo tipo gravado (app/docs/LEIS.md:119) x pareador chamado pelo juiz aceita tipo gravado quando falta papel da ata (app/ponto/turnos.py:1382, app/ponto/turnos.py:1992) ; regularização deriva data_turno da data civil da entrada e grava Justificativa com ela, apesar de já possuir o turno do juiz (app/ponto/services/regularizacao.py:62, app/ponto/services/regularizacao.py:73, app/ponto/services/regularizacao.py:94) ; -

app/ponto/turnos.py::proximo_tipo_de ; a próxima batida é entrada ou saída? (app/core/juizes.py:209) ; nenhum texto ; intermitente alterna pela última batida apurável em 14 horas (app/ponto/turnos.py:2466, app/ponto/turnos.py:2473), demais retornam E sem turno vivo e S fora do intervalo (app/ponto/turnos.py:2474, app/ponto/turnos.py:2477), no intervalo comparam proximidade dos marcos de volta e saída, empate favorece E (app/ponto/turnos.py:2488, app/ponto/turnos.py:2493) ; nao ; proxima_batida_de alterna pela última batida gravada, sem janela ou exclusão de retratadas (app/ponto/utils.py:241, app/ponto/utils.py:244, app/ponto/utils.py:246) ; -

app/ponto/turnos.py::decidir_tipo_estrito ; o tipo gravado deve virar (flip)? (app/core/juizes.py:210) ; nenhum texto ; mantém o tipo sem marcos ou fora de E/S, troca somente quando o marco oposto está a até 10 minutos e o próprio está ausente ou a mais de 30 minutos, usando marcos na mesma data do instante (app/ponto/turnos.py:2393, app/ponto/turnos.py:2403, app/ponto/turnos.py:2410, app/ponto/turnos.py:2416, app/ponto/turnos.py:2419) ; nao ; etiquetas_pela_celula decide flips por atribuição global a slots e teto de distância próprio, podendo substituir o resultado anterior e convertê-lo em automático (app/ponto/services/flip_auto.py:233, app/ponto/services/flip_auto.py:238, app/ponto/services/flip_auto.py:242, app/ponto/services/flip_auto.py:308, app/ponto/services/flip_auto.py:313) ; -

app/ponto/turnos.py::turno_aberto_de ; há turno aberto vivo agora? (app/core/juizes.py:211) ; L-087 rege o pareamento usado pelo juiz (app/docs/LEIS.md:119), nenhum texto específico para a margem de expiração ; carrega batidas apuráveis de três dias e vínculo vigente, pareia com ata e marcos (app/ponto/turnos.py:2218, app/ponto/turnos.py:2219, app/ponto/turnos.py:2278), escolhe a última aberta e a descarta após fim previsto mais quatro horas, sem escala ou fim não expira por esse critério (app/ponto/turnos.py:2006, app/ponto/turnos.py:2011, app/ponto/turnos.py:2024, app/ponto/turnos.py:2030) ; sim: proibição de parear pelo tipo gravado (app/docs/LEIS.md:119) x fallback ao tipo gravado no pareador chamado pelo juiz (app/ponto/turnos.py:1382, app/ponto/turnos.py:2278) ; disponibilidade para cobertura decide turno aberto pela última batida crua E numa janela de 14 horas (app/ponto/utils.py:183, app/ponto/utils.py:191) ; -

app/escala/utils.py::_match_marcos ; que marco previsto a batida ocupa? (app/core/juizes.py:212) ; L-084 e L-087 (app/docs/LEIS.md:116, app/docs/LEIS.md:119), PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO (app/docs/CORTES.md:215), inclusão do alinhamento na obra do juiz do par (app/docs/CORTES.md:318) ; usa alinhamento por papel quando disponível e compatível com L-084, caso contrário usa candidatos no envelope, alinhamento por programação dinâmica e cluster-guard (app/escala/utils.py:596, app/escala/utils.py:609, app/escala/utils.py:613, app/escala/utils.py:629, app/escala/utils.py:633, app/escala/utils.py:650) ; sim: texto diz que a batida só ocupa o marco mais próximo (app/docs/JUIZES.md:28) x código rejeita outro marco somente quando o mais próximo já tem dono ainda mais próximo (app/escala/utils.py:646, app/escala/utils.py:650) ; montador da grade reassocia órfãs aos marcos das pontas por regras próprias de proximidade e ordem (app/escala/utils.py:2025, app/escala/utils.py:2032, app/escala/utils.py:2040) ; -

app/escala/models.py::EscalaColaborador.marcos_do_dia ; quais são os marcos previstos do dia? (app/core/juizes.py:213) ; L-020, célula com DNA congelado (app/docs/LEIS.md:71), L-024, fonte única de previsto (app/docs/LEIS.md:75) ; lê dna.marcos e converte hi, hf, hii e hfi para horários, com marcos vazios ou ausentes consulta o template (app/escala/models.py:1124, app/escala/models.py:1131, app/escala/models.py:1133) ; nao ; veredito_celula._marcos_do_dia replica a seleção DNA/template, acrescentando a exigência de hi preenchido para aceitar o DNA (app/ponto/services/veredito_celula.py:31, app/ponto/services/veredito_celula.py:32, app/ponto/services/veredito_celula.py:38, app/ponto/services/veredito_celula.py:43) ; -

app/ponto/turnos.py::intervalos_do_turno ; o vão entre batidas foi intervalo? (app/core/juizes.py:214) ; L-032, intervalo dentro do envelope (app/docs/LEIS.md:80), L-087, papel pela ata (app/docs/LEIS.md:119), autoridade também nomeada no dossiê (app/docs/DOSSIES.md:73) ; devolve lista vazia para turno ausente e aceita somente pares marcados com duração mínima de 900 segundos (app/ponto/turnos.py:499, app/ponto/turnos.py:542, app/ponto/turnos.py:545) ; sim: proibição de decidir papel pelo tipo gravado (app/docs/LEIS.md:119) x seleção dos pares aceita o ramo S seguido de E pelo tipo gravado quando a ata se abstém (app/ponto/turnos.py:845, app/ponto/turnos.py:881) ; vigiar_motor_autoridade agrupa marcas de intervalo duas a duas e desconta qualquer duração positiva, sem o piso de quinze minutos (app/ponto/management/commands/vigiar_motor_autoridade.py:83, app/ponto/management/commands/vigiar_motor_autoridade.py:85, app/ponto/management/commands/vigiar_motor_autoridade.py:87) ; -

app/ponto/motor_calculo_v2.py::MotorBase.aplicar_tolerancia ; a entrada atrasou ou a saída adiantou além da tolerância? (app/core/juizes.py:215) ; L-084, recusa pontualidade quando ambas as pontas estão a mais de três horas dos marcos (app/docs/LEIS.md:116) ; calcula os desvios, devolve zero para ambas as pontas incompatíveis com o cadastro e, nos demais casos, cobra o desvio integral positivo que ultrapasse 10 minutos por marcação (app/ponto/motor_calculo_v2.py:1973, app/ponto/motor_calculo_v2.py:1988, app/ponto/motor_calculo_v2.py:2005, app/ponto/motor_calculo_v2.py:2006, app/ponto/motor_calculo_v2.py:60) ; sim: descrição promete tolerância de 5 minutos (app/ponto/motor_calculo_v2.py:1952) x corpo compara com constante de 10 minutos (app/ponto/motor_calculo_v2.py:2006, app/ponto/motor_calculo_v2.py:60) ; nenhum achado para esse julgamento, procurados aplicar_tolerancia, TOLERANCIA_CONFORMIDADE_MIN, minutos_atraso e minutos_saida_antecipada, o calculador localizado reutiliza o próprio método (app/ponto/calculador/regras.py:114) ; qual tolerância textual rege a marcação, 5 minutos descritos no método (app/ponto/motor_calculo_v2.py:1952) ou 10 minutos declarados como decisão F0 (app/ponto/motor_calculo_v2.py:53, app/ponto/motor_calculo_v2.py:60)?

app/escala/servico_jornada.py::turno_cruza_meia_noite ; o turno previsto cruza a meia-noite? (app/core/juizes.py:216) ; nenhum texto que fixe a comparação ou a borda de igualdade ; retorna verdadeiro quando ambos os horários existem e fim é menor ou igual ao início, tratando igualdade como 24 horas, horário ausente retorna falso (app/escala/servico_jornada.py:108, app/escala/servico_jornada.py:110, app/escala/servico_jornada.py:111) ; nao ; TipoEscala.clean decide cruzamento por comparação estrita hora_inicio maior que hora_fim e emite a mensagem de cruzamento (app/escala/models.py:359, app/escala/models.py:361) ; -

app/escala/servico_jornada.py::eh_turno_partido ; este turno tem dois blocos, é partido? (app/core/juizes.py:220) ; L-091, desenho da escala decide o regime (app/docs/LEIS.md:124), nascimento assinado em app/docs/CORTES.md:235 ; calcula o vão circular em minutos e exige valor maior que teto_min, padrão 120 minutos, sem uma das pontas retorna falso (app/escala/servico_jornada.py:114, app/escala/servico_jornada.py:130, app/escala/servico_jornada.py:152, app/escala/servico_jornada.py:154, app/escala/servico_jornada.py:156) ; nao ; diff_reclassificar_partido calcula o vão circular e compara com vao_teto por conta própria para declarar PARTIDO DE VERDADE, com padrão 120 minutos (app/ponto/management/commands/diff_reclassificar_partido.py:53, app/ponto/management/commands/diff_reclassificar_partido.py:80, app/ponto/management/commands/diff_reclassificar_partido.py:81, app/ponto/management/commands/diff_reclassificar_partido.py:82) ; -

app/escala/utils.py::status_do_delta ; que status tem a lâmpada do marco? (app/core/juizes.py:221) ; nenhum texto ; módulo do desvio até 10 minutos resulta em ok, positivo até 90 em atraso e acima em atraso_grave, negativo até 90 em adianto e além em adianto_grave (app/escala/utils.py:306, app/escala/utils.py:307, app/escala/utils.py:310, app/escala/utils.py:311, app/ponto/motor_calculo_v2.py:60, app/ponto/motor_calculo_v2.py:65) ; nao ; nenhum achado, procurados status_do_delta, atraso_grave, adianto_grave, TOLERANCIA_GRADACAO_GRAVE_MIN e comparações de delta com 10 e 90, o leitor da célula delega ao juiz (app/escala/services/leitor_celula.py:59, app/escala/services/leitor_celula.py:60) ; -

app/ponto/turnos.py::dia_encerrado ; o dia está encerrado? (app/core/juizes.py:235) ; L-073, teto por instante ou fato encerrado (app/docs/LEIS.md:100), nascimento e correção do predicado (app/docs/CORTES.md:55, app/docs/CORTES.md:62) ; exige ausência de turno aberto vivo e fim previsto já alcançado, sem previsto não afirma encerramento, aberta expirada deixa de impedir a resposta (app/ponto/turnos.py:2077, app/ponto/turnos.py:2116, app/ponto/turnos.py:2121, app/ponto/turnos.py:2124, app/ponto/turnos.py:2011) ; nao ; supra_juiz deriva encerrado_pela_data da comparação entre data e hoje e usa a resposta no ramo sem_vinculo_trab (app/ponto/supra_juiz.py:292, app/ponto/supra_juiz.py:467) ; -

app/ponto/turnos.py::par_completo ; o par está completo? (app/core/juizes.py:240) ; L-073 (app/docs/LEIS.md:100), nascimento do juiz e correção que restabelece sua autoridade própria (app/docs/CORTES.md:56, app/docs/CORTES.md:62) ; carrega turnos de dias encerrados, associa cada batida aos respectivos conjuntos de data_turno e responde verdadeiro quando os conjuntos se intersectam (app/ponto/turnos.py:2179, app/ponto/turnos.py:2182, app/ponto/turnos.py:2186, app/ponto/turnos.py:2187, app/ponto/turnos.py:2202) ; sim: descrição exige batidas no MESMO turno (app/ponto/turnos.py:2194) x predicado exige somente data_turno em comum (app/ponto/turnos.py:2186, app/ponto/turnos.py:2187), embora o núcleo contemple dois turnos fechados no mesmo dia (app/ponto/turnos.py:647) ; nenhum achado de decisão autônoma completa, procurados par_completo, par completo, completude_turno, entrada/saida e t.aberto, a pré-seleção encontrada é seguida da consulta ao juiz (app/ponto/management/commands/detectar_par_relampago.py:123, app/ponto/management/commands/detectar_par_relampago.py:353) ; -

app/ponto/turnos.py::turno_aberto_ha ; há quanto tempo o turno está aberto? (app/core/juizes.py:245) ; corte determina TurnoMaterializado como fonte e substitui expressamente a indicação anterior de turnos_do_colab (app/docs/CORTES.md:95, app/docs/CORTES.md:285) ; consulta a linha materializada por colaborador e data_turno, exige aberta e entrada, retorna horas desde a entrada, demais casos retornam None (app/ponto/turnos.py:3192, app/ponto/turnos.py:3193, app/ponto/turnos.py:3194, app/ponto/turnos.py:3196) ; nao ; painel seleciona entrada de turno aberto obtido de turnos_do_colab e calcula as horas por conta própria (app/ponto/views.py:377, app/ponto/views.py:381, app/ponto/views.py:385) ; -

app/ponto/turnos.py::lembrete_fim_turno ; que lembrete de fim de turno está na hora? (app/core/juizes.py:248) ; corte fixa lembretes em −15, −5, +5, +10 e +20 minutos do fim previsto (app/docs/CORTES.md:286) ; até o fim considera lembretes PRE, depois considera POS, retorna todos cuja distância do instante-alvo seja até 240 segundos inclusive (app/ponto/turnos.py:3224, app/ponto/turnos.py:3225, app/ponto/turnos.py:3226, app/ponto/turnos.py:3232, app/ponto/turnos.py:3235) ; nao ; nenhum achado, procurados lembrete_fim_turno, DELTAS_PRE, DELTAS_POS, pre_15, pre_5, pos_5, pos_10, pos_20 e comparações com fim previsto, o comando localizado chama o juiz (app/ponto/management/commands/processar_alertas_turno.py:118) ; -

Contagem das linhas acima: juízes lidos = 15, igual ao universo AST de 15 entradas (app/core/juizes.py:207, app/core/juizes.py:248). Coluna 5 com sim = 6, juízes 1, 4, 5, 7, 8 e 13, pelas contradições citadas nessas linhas. Coluna 6 com achado = 11, juízes 1–7, 9, 10, 12 e 14, pelos sítios citados nessas linhas. Coluna 7 preenchida = 1, juiz 8 (app/ponto/motor_calculo_v2.py:1952, app/ponto/motor_calculo_v2.py:53). Ocorrência e impacto desses achados em produção: [nao verificado].

## 14/11 09:06 · sessao d5cb62e6044d · - · - · origem=Ronald (tmux)

Conflito do juiz 13 confirmado: a docstring exige as duas batidas no MESMO turno (`app/ponto/turnos.py:2194`), mas o predicado só exige interseção de `data_turno` (`app/ponto/turnos.py:2186`, `app/ponto/turnos.py:2187`).

Dois turnos fechados no mesmo dia são possibilidade expressa do núcleo (`app/ponto/turnos.py:647`) e têm caso em teste existente (`app/ponto/tests/test_vigia_pareador_celula.py:108`, `app/ponto/tests/test_vigia_pareador_celula.py:112`, `app/ponto/tests/test_vigia_pareador_celula.py:116`).

Executei em memória o corpo real extraído por AST: com T1 contendo pks 1/2 e T2 contendo pks 3/4, ambos no mesmo dia encerrado, a consulta entre pk1 e pk4 retornou `True` (`app/ponto/turnos.py:2173`, `app/ponto/turnos.py:2187`). Ocorrência em produção: [nao verificado].

Para a linha 13: coluna 5 = `sim: MESMO turno (app/ponto/turnos.py:2194) versus mesmo dia de turno (app/ponto/turnos.py:2186, app/ponto/turnos.py:2187)`; coluna 7 = `-`.

## 14/11 09:05 · sessao 6686a05b86c0 · - · - · origem=Ronald (tmux)

5) app/escala/utils.py::_match_marcos ; que marco previsto a batida ocupa? ; L-087 app/docs/LEIS.md:119, L-084 app/docs/LEIS.md:116, PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO app/docs/CORTES.md:215, JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES app/docs/CORTES.md:318 ; com turno/papéis usa _alinhar_por_papel, salvo cadastro incompatível, senão candidatos no envelope e DP com cluster-guard, gera células e órfãs e marca consumo (app/escala/utils.py:596, app/escala/utils.py:609, app/escala/utils.py:613, app/escala/utils.py:629, app/escala/utils.py:633, app/escala/utils.py:650, app/escala/utils.py:663) ; sim: “batida só ocupa o marco MAIS PROXIMO” (app/docs/JUIZES.md:28) versus rejeição condicionada a o marco mais próximo ter dono ainda mais próximo (app/escala/utils.py:650), permitindo atribuição a outro marco quando essa condição não ocorre ; montar_grade_prevista_periodo_por_turno reconcilia órfãs nos slots das pontas por regras próprias de proximidade e ordem (app/escala/utils.py:2025, app/escala/utils.py:2032, app/escala/utils.py:2040) ; -

6) app/escala/models.py::EscalaColaborador.marcos_do_dia ; quais são os marcos previstos do dia? ; L-020 app/docs/LEIS.md:71, L-024 app/docs/LEIS.md:75, nenhum corte específico adicional achado [não verificado] ; lê dna.marcos da célula e devolve hi,hf,hii,hfi convertidos para time, se marcos vazio/ausente consulta TipoEscala.marcos_do_dia (app/escala/models.py:1124, app/escala/models.py:1131, app/escala/models.py:1133) ; nao ; ponto/services/veredito_celula.py::_marcos_do_dia replica seleção DNA/template e exige adicionalmente mk.get('hi') (app/ponto/services/veredito_celula.py:31, app/ponto/services/veredito_celula.py:32, app/ponto/services/veredito_celula.py:38, app/ponto/services/veredito_celula.py:43), marcos_dna_periodo também interpreta o DNA em outra implementação (app/escala/utils.py:104, app/escala/utils.py:106) ; -

9) app/escala/servico_jornada.py::turno_cruza_meia_noite ; o turno previsto cruza a meia-noite? ; L-091 enquadra geometria pelo desenho (app/docs/LEIS.md:124), nenhum texto de LEIS/CORTES encontrado fixando a borda de igualdade [não verificado] ; retorna bool(hi and hf) and hf<=hi, igualdade significa turno24h e cruza, horário ausente retorna False (app/escala/servico_jornada.py:108, app/escala/servico_jornada.py:110, app/escala/servico_jornada.py:111) ; nao ; TipoEscala.clean deriva cruzamento com hora_inicio>hora_fim, comparação estrita própria, e emite “Horario cruza meia-noite” (app/escala/models.py:359, app/escala/models.py:361) ; -

10) app/escala/servico_jornada.py::eh_turno_partido ; este turno tem dois blocos, é partido? ; L-091 app/docs/LEIS.md:124, JUIZ-EH-TURNO-PARTIDO-ASSINADO app/docs/CORTES.md:235 ; calcula intervalo circular em minutos e retorna vao>teto_min, default120, sem uma das pontas retorna False (app/escala/servico_jornada.py:114, app/escala/servico_jornada.py:130, app/escala/servico_jornada.py:152, app/escala/servico_jornada.py:154, app/escala/servico_jornada.py:156) ; nao ; diff_reclassificar_partido calcula circularmente o vão e compara com vao_teto por conta própria para declarar “PARTIDO DE VERDADE”, default120 (app/ponto/management/commands/diff_reclassificar_partido.py:53, app/ponto/management/commands/diff_reclassificar_partido.py:77, app/ponto/management/commands/diff_reclassificar_partido.py:80, app/ponto/management/commands/diff_reclassificar_partido.py:81, app/ponto/management/commands/diff_reclassificar_partido.py:82) ; -

11) app/escala/utils.py::status_do_delta ; que status tem a lâmpada do marco? ; nenhum texto específico encontrado em LEIS/CORTES [não verificado] ; abs(delta)<=10 dá ok, delta positivo até90 dá atraso e acima atraso_grave, negativo até90 dá adianto e além adianto_grave (app/escala/utils.py:306, app/escala/utils.py:307, app/escala/utils.py:310, app/escala/utils.py:311, app/ponto/motor_calculo_v2.py:60, app/ponto/motor_calculo_v2.py:65) ; nao ; nenhum achado, procurados atraso_grave, adianto_grave, status_do_delta, TOLERANCIA_GRADACAO_GRAVE_MIN e comparações delta com10/90, o leitor localizado delega ao juiz (app/escala/services/leitor_celula.py:59, app/escala/services/leitor_celula.py:60) ; -

Contagem deste recorte: 5 juízes lidos (app/core/juizes.py:212, app/core/juizes.py:213, app/core/juizes.py:216, app/core/juizes.py:220, app/core/juizes.py:221), 1 conflito, 4 com derivador externo, 0 perguntas de lei pelas duas diretivas normativas exigidas.

Cuidado na consolidação: a colisão de cortes descrita em app/escala/utils.py:2297 a app/escala/utils.py:2304 é sobre desconto da pausa no número de minutos previstos, não prova duas candidatas para a pergunta específica “quais marcos previstos do dia?”.

## 14/11 09:05 · sessao d5cb62e6044d · - · - · origem=Ronald (tmux)

7 — `app/ponto/turnos.py::intervalos_do_turno`

- Pergunta registrada: “o vao entre batidas foi intervalo?” (`app/core/juizes.py:214`).
- Textos: L-032 declara intervalo a batida dentro do envelope (`app/docs/LEIS.md:80`); L-087 manda papel pela ata e proíbe decidir pelo tipo gravado (`app/docs/LEIS.md:119`). Dossiê nomeia `intervalos_do_turno` como autoridade (`app/docs/DOSSIES.md:73`). Nenhum corte específico do piso de quinze minutos encontrado; buscados `INTERVALO`, `PISO`, `BUG.?100`, `15 min`, `pausa` em CORTES.md.
- Código: `None` resulta em lista vazia; pares vêm de `_pares_marcados`, filtrados por duração mínima de 900 segundos (`app/ponto/turnos.py:499`, `app/ponto/turnos.py:542`, `app/ponto/turnos.py:545`).
- Conflito demonstrável: L-087 proíbe papel pelo tipo gravado (`app/docs/LEIS.md:119`), mas `_pares_marcados`, chamado pelo juiz em `app/ponto/turnos.py:545`, aceita o ramo `_tb == 'S' and _tnx == 'E'` (`app/ponto/turnos.py:881`). O comentário explica expressamente que ata muda devolve ao teste do tipo gravado (`app/ponto/turnos.py:845`).
- Fora do juiz: `vigiar_motor_autoridade` seleciona flags, agrupa duas a duas e aceita qualquer gap positivo, sem piso de quinze minutos (`app/ponto/management/commands/vigiar_motor_autoridade.py:83`, `app/ponto/management/commands/vigiar_motor_autoridade.py:87`). Outro respondedor usa duas lâmpadas acesas e aceita pausa sem piso (`app/ponto/juiz_batida.py:280`, `app/ponto/juiz_batida.py:284`).
- Coluna 7: se comentários do código contam como candidata escrita, cabe “Quando a ata se abstém, o tipo gravado pode decidir o intervalo?”: proibição em `app/docs/LEIS.md:119` versus permissão escrita em `app/ponto/turnos.py:845`. Duas normas documentais vigentes para essa alternativa: [nao verificado].

12 — `app/ponto/turnos.py::dia_encerrado`

- Pergunta registrada (`app/core/juizes.py:235`).
- Textos: L-073 admite comparação de instante ou fato encerrado (`app/docs/LEIS.md:100`); cortes do nascimento e sua correção mandam fato encerrado, nunca `data < hoje` (`app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`). As revogações antigas estão explicitamente revogadas, portanto não as trataria como duas candidatas simultâneas (`app/docs/CORTES.md:62`).
- Código: delega a `dia_encerrado_motivo(...) is None` (`app/ponto/turnos.py:2077`); sem turno aberto vivo e com fim previsto alcançado retorna encerrado, sem previsto retorna falso (`app/ponto/turnos.py:2116`, `app/ponto/turnos.py:2121`, `app/ponto/turnos.py:2124`). Aberta expira após fim previsto mais margem (`app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2030`).
- Conflito inequívoco vigente: não afirmado. Existe ponto literal para revisão: “turno fechado” no corte (`app/docs/CORTES.md:62`) contra possibilidade de encerrar após expiração de turno ainda `aberto` (`app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2116`, `app/ponto/turnos.py:2124`). A lei geral permite defesa por instante (`app/docs/LEIS.md:100`).
- Fora do juiz: `supra_juiz` calcula autonomamente `encerrado_pela_data = not (hoje and data and data >= hoje)` (`app/ponto/supra_juiz.py:292`) e usa no ramo sem vínculo (`app/ponto/supra_juiz.py:467`). Também `classificar_falta` compara relógio ao fim previsto por conta própria (`app/escala/servico_jornada.py:386`, `app/escala/servico_jornada.py:388`).
- Coluna 7 sugerida: `-`, pois as alternativas documentais antigas são expressamente corrigidas (`app/docs/CORTES.md:62`).

13 — `app/ponto/turnos.py::par_completo`

- Pergunta registrada (`app/core/juizes.py:240`).
- Textos: L-073 (`app/docs/LEIS.md:100`); nascimento do par e correção que restabelece juiz próprio (`app/docs/CORTES.md:244`, `app/docs/CORTES.md:62`).
- Código: as batidas precisam pertencer a um mesmo dia de turno cujo dia esteja encerrado; o carregador constrói conjuntos de dias por batida e exige interseção (`app/ponto/turnos.py:2179`, `app/ponto/turnos.py:2182`, `app/ponto/turnos.py:2186`, `app/ponto/turnos.py:2187`); casca calcula janela ontem–dia da última batida e chama esse predicado (`app/ponto/turnos.py:2199`, `app/ponto/turnos.py:2202`).
- Conflito: nenhum inequívoco encontrado; corte vigente exige juiz próprio e o emissor chama esse juiz (`app/docs/CORTES.md:244`, `app/ponto/management/commands/detectar_par_relampago.py:353`).
- Fora do juiz: há pré-seleção por `t.aberto` e presença de entrada/saída (`app/ponto/management/commands/detectar_par_relampago.py:123`), mas o `handle` depois pergunta `par_completo` (`app/ponto/management/commands/detectar_par_relampago.py:353`). Não recomendo contar essa pré-seleção como respondedor autônomo da pergunta completa. Buscados `par_completo`, `par completo`, `completude_turno`, combinações `entrada/saida`, `t.aberto` fora de testes/migrations.
- Coluna 7: `-`; mudanças anteriores são expressamente anuladas (`app/docs/CORTES.md:244`).

14 — `app/ponto/turnos.py::turno_aberto_ha`

- Pergunta registrada (`app/core/juizes.py:245`).
- Texto específico: corte decide `TurnoMaterializado` como fonte e retira frase antiga `turnos_do_colab` (`app/docs/CORTES.md:95`, `app/docs/CORTES.md:285`). Nenhum L-NNN encontrado que determine a fórmula das horas.
- Código: consulta linha por colaborador e `data_turno`, exige aberta e entrada, retorna horas desde timestamp da entrada; demais casos retornam `None` (`app/ponto/turnos.py:3193`, `app/ponto/turnos.py:3194`, `app/ponto/turnos.py:3196`).
- Conflito vigente: não; fonte corrigida e código concordam (`app/docs/CORTES.md:95`, `app/ponto/turnos.py:3192`). O literal antigo ainda aparece no fim de CORTES.md:285, porém o mesmo registro diz expressamente que essa frase saiu.
- Fora do juiz: painel calcula horas desde entrada selecionada de `turnos_do_colab` (`app/ponto/views.py:377`, `app/ponto/views.py:381`, `app/ponto/views.py:385`); regularização repete (`app/ponto/views.py:3181`); helper da API repete (`app/ponto/utils_turno_orfao.py:35`).
- Coluna 7: `-`; fonte antiga foi explicitamente substituída (`app/docs/CORTES.md:95`).

15 — `app/ponto/turnos.py::lembrete_fim_turno`

- Pergunta registrada (`app/core/juizes.py:248`).
- Texto específico: corte determina lembretes −15/−5/+5/+10/+20 minutos, números iguais para todos (`app/docs/CORTES.md:286`). Nenhum L-NNN encontrado que determine essas janelas.
- Código: antes ou exatamente no fim usa PRE; depois usa POS; devolve todos os tipos com distância absoluta até 240 segundos, inclusive (`app/ponto/turnos.py:3224`, `app/ponto/turnos.py:3225`, `app/ponto/turnos.py:3226`, `app/ponto/turnos.py:3232`, `app/ponto/turnos.py:3235`).
- Conflito: nenhum encontrado entre corte e código (`app/docs/CORTES.md:286`, `app/ponto/turnos.py:3235`).
- Fora do juiz: nenhum achado de decisão autônoma; buscados `lembrete_fim_turno`, `DELTAS_PRE`, `DELTAS_POS`, `pre_15`, `pre_5`, `pos_5`, `pos_10`, `pos_20`, expressões de tolerância/fim previsto. Comando chama juiz (`app/ponto/management/commands/processar_alertas_turno.py:118`); textos e TTL por tipo são configuração de envio, sem decidir vencimento (`app/ponto/management/commands/processar_alertas_turno.py:237`, `app/ponto/management/commands/processar_alertas_turno.py:264`).
- Coluna 7: `-`; segunda candidata escrita não encontrada.

## 14/11 09:04 · sessao 7519216fd783 · - · - · origem=Ronald (tmux)

1. app/ponto/turnos.py::turnos_do_colab ; a que turno (data_turno) pertence a batida? (app/core/juizes.py:208) ; L-085, jornada pertence ao dia de início (app/docs/LEIS.md:117), L-087, papel vem da ata e nunca do tipo gravado (app/docs/LEIS.md:119), CORTES reafirma autoridade do dia em turnos_do_colab (app/docs/CORTES.md:63) ; carrega batidas não retratadas com pad, escolhe vínculo por dia, segmenta e retorna turnos pareados pelo data_turno (app/ponto/turnos.py:1863, app/ponto/turnos.py:1960, app/ponto/turnos.py:1992), data normalmente local da entrada, com transferência à véspera noturna pelo envelope, inclusive saída órfã (app/ponto/turnos.py:285, app/ponto/turnos.py:297, app/ponto/turnos.py:301) ; sim: nunca usar tipo para parear (app/docs/LEIS.md:119) x fallback executável ao tipo quando ata cala (app/ponto/turnos.py:1382), corpo chamado pelo juiz em app/ponto/turnos.py:1992 ; escritor próprio: regularizar_turno_orfao deriva data_turno da data civil da entrada (app/ponto/services/regularizacao.py:73) e grava Justificativa com essa data (app/ponto/services/regularizacao.py:94), embora já tenha recebido o turno canônico em app/ponto/services/regularizacao.py:62, leitor próprio equivalente em app/ponto/views.py:3232 ; -

2. app/ponto/turnos.py::proximo_tipo_de ; a próxima batida é entrada ou saída? (app/core/juizes.py:209) ; nenhum texto normativo encontrado fixando seu predicado, listado como autoridade em app/docs/JUIZES.md:21, CORTES.md:359 apenas inclui sugestão de próximo tipo no escopo de outra obra (app/docs/CORTES.md:359) ; intermitente alterna última batida apurável nas últimas 14h (app/ponto/turnos.py:2466, app/ponto/turnos.py:2473), demais sem turno vivo retornam E, vivo fora de intervalo S (app/ponto/turnos.py:2474), no intervalo compara proximidade de fim de pausa e fim de turno, empate E (app/ponto/turnos.py:2488, app/ponto/turnos.py:2493) ; nao ; respondedor próprio proxima_batida_de alterna última batida gravada sem janela, jornada ou exclusão de retratadas (app/ponto/utils.py:240, app/ponto/utils.py:244, app/ponto/utils.py:246), grep por proxima_batida_de encontrou definição sem chamador, existência continua relevante pela L-111 (app/docs/LEIS.md:137) ; -

3. app/ponto/turnos.py::decidir_tipo_estrito ; o tipo gravado deve virar (flip)? (app/core/juizes.py:210) ; nenhum texto normativo encontrado fixando 10 e 30 minutos, listado como autoridade em app/docs/JUIZES.md:21, CORTES.md:359 inclui o juiz no escopo sem responder seu predicado (app/docs/CORTES.md:359) ; mantém gravado sem marcos ou tipo E/S, troca apenas com marco oposto a até 10min e próprio a mais de 30min ou ausente, calcula distância no mesmo dia de ts_local (app/ponto/turnos.py:2393, app/ponto/turnos.py:2403, app/ponto/turnos.py:2410, app/ponto/turnos.py:2416, app/ponto/turnos.py:2419) ; nao ; etiquetas_pela_celula decide flips por atribuição global a slots e teto circular de 90min (app/ponto/services/flip_auto.py:151, app/ponto/services/flip_auto.py:233, app/ponto/services/flip_auto.py:238, app/ponto/services/flip_auto.py:242), avaliar_fila pode substituir decisão humana restante por esses flips e classificar auto (app/ponto/services/flip_auto.py:308, app/ponto/services/flip_auto.py:313), portanto é decisão alternativa final, além da chamada ao juiz em app/ponto/services/flip_auto.py:300 ; -

4. app/ponto/turnos.py::turno_aberto_de ; há turno aberto vivo agora? (app/core/juizes.py:211) ; nenhum texto encontrado fixando diretamente sua expiração, L-087 rege o pareamento usado por seu corpo (app/docs/LEIS.md:119), lookup aponta a autoridade (app/docs/JUIZES.md:20) ; carrega batidas apuráveis dos três dias e vínculo vigente (app/ponto/turnos.py:2218, app/ponto/turnos.py:2219), pareia com papel da ata e marcos (app/ponto/turnos.py:2272, app/ponto/turnos.py:2278), escolhe última aberta e descarta depois de fim previsto mais quatro horas, sem escala ou fim não expira (app/ponto/turnos.py:2006, app/ponto/turnos.py:2011, app/ponto/turnos.py:2024, app/ponto/turnos.py:2030) ; sim se contabilizadas regras do corpo delegado: nunca tipo gravado no pareador (app/docs/LEIS.md:119) x fallback ao tipo gravado (app/ponto/turnos.py:1382), alcançado por app/ponto/turnos.py:2278 ; colabs_disponiveis_para_cobertura decide turno aberto por última batida crua E numa janela de 14h, sem perguntar marcos ou expiração (app/ponto/utils.py:183, app/ponto/utils.py:191) ; -

Os quatro têm achado na coluna 6. Conflito certo no juiz 1, e o mesmo conflito no corpo delegado do juiz 4. Nenhuma dupla normativa vigente identificada para coluna 7. Não tratei documentação histórica revogada, lookup de endereço ou descrição de dívida como uma segunda candidata normativa.

## 14/11 08:48 · sessao ad35cd2f0f59 · COERENCIA-DAS-LEIS · censo · origem=Code

app/ponto/catalogo/ausencias.py::efeito_vigente ; a ausência paga, desconta ou suprime o dia? (app/core/juizes.py:252) ; L-006, origem cadastral (app/docs/LEIS.md:64), CATALOGO-SAIDA-ANTECIPADA-DESCONTA (app/docs/CORTES.md:202) ; devolve o efeito do catálogo vigente ou None, usando cadastro ativo e, na ausência dele, CATALOGO (app/ponto/catalogo/ausencias.py:249, app/ponto/catalogo/ausencias.py:278, app/ponto/catalogo/ausencias.py:295), valor cadastrado em produção [nao verificado] ; sim, saída antecipada deve DESCONTAR em minutos (app/docs/CORTES.md:202), mas a semente usada pelo fallback declara SUPRIME (app/ponto/catalogo/ausencias.py:91) ; nenhum achado autônomo inequívoco, procurados efeito_vigente, TIPOS_ABONAM, TIPOS_DESCONTAM, TIPOS_PAGOS e campo efeito, os conjuntos encontrados derivam do catálogo (app/ponto/catalogo/ausencias.py:308) ; -

app/ponto/turnos.py::cobertura_ausencia_periodo ; que dias a ausência ocupa? (app/core/juizes.py:253) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; intervalo inclusivo recortado à consulta, sem data_fim estende até o fim consultado somente para TIPOS_EM_ABERTO, demais ocupam a data inicial, vence maior data/pk, folha aceita aprovadas e cobrança exclui rejeitadas (app/ponto/turnos.py:2772, app/ponto/turnos.py:2778, app/ponto/turnos.py:2781, app/ponto/turnos.py:2516) ; nao (app/ponto/turnos.py:2737, app/ponto/turnos.py:2772) ; achado, houve_fato_novo decide autonomamente se a ausência alcança o dia, tratando data_fim nulo como dia único, buscados data_fim, data__lte e cobertura_ausencia_periodo (app/ponto/services/cartorio.py:383, app/ponto/services/cartorio.py:384) ; -

app/ponto/precedencia.py::veto_de_lancamento ; o lançamento colide com outro fato, devendo ser recusado? (app/core/juizes.py:254) ; L-089 para falta sem jornada (app/docs/LEIS.md:121), RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER para apresentação da sobreposição (app/docs/CORTES.md:338) ; recusa sobreposição reprovada por linha_que_veta, folga lançada salvo travessia de férias, e falta em dia explicitamente sem trabalho, caso contrário retorna None (app/ponto/precedencia.py:461, app/ponto/precedencia.py:496, app/ponto/precedencia.py:514, app/ponto/precedencia.py:538, app/ponto/precedencia.py:545) ; nao (app/docs/LEIS.md:121, app/ponto/precedencia.py:538) ; achado, a prévia do chat decide colisão compondo ausencias_sobrepostas e linha_que_veta, sem chamar veto_de_lancamento nem avaliar folga lançada, seu recorte exclui férias, falta e INSS, buscados veto_de_lancamento, linha_que_veta e ausencias_sobrepostas (app/ponto/services/checar_ausencia.py:10, app/ponto/services/checar_ausencia.py:35, app/ponto/services/checar_ausencia.py:38) ; -

app/ponto/services/ausencia.py::estado_inicial ; a ausência exige documento, e em que prazo? (app/core/juizes.py:255) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; com anexo ou tipo férias retorna aguardando_decisao sem prazo, tipo exigente sem anexo retorna aguardando_documento com prazo, demais aguardando_decisao, a criação estabelece sete dias desde a data local (app/ponto/services/ausencia.py:792, app/ponto/services/ausencia.py:794, app/ponto/services/ausencia.py:796, app/ponto/services/ausencia.py:745, app/ponto/services/ausencia.py:339) ; sim, o texto do campo promete recusar lançamento sem comprovante (app/ponto/models.py:584), enquanto o juiz determina estado de espera e declara que nunca recusa por essa exigência (app/ponto/services/ausencia.py:786, app/ponto/services/ausencia.py:797) ; nenhum achado para a mesma exigência ao colaborador e seu prazo, procurados exige_documento, TIPOS_EXIGEM_DOC e PRAZO_DOCUMENTO_DIAS, a propriedade delega ao juiz (app/ponto/models.py:729), o relatório lê a flag cadastral e declara essa pergunta distinta (app/ponto/services/atestados_lote.py:26, app/ponto/services/atestados_lote.py:29) ; sem comprovante, recusar o lançamento (app/ponto/models.py:584) ou recebê-lo em aguardando_documento (app/ponto/services/ausencia.py:786, app/ponto/services/ausencia.py:797)?

app/ponto/services/ausencia.py::ABERTAS ; a ausência está aberta ou decidida? (app/core/juizes.py:256) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; abertas são aguardando_documento e aguardando_decisao (app/ponto/services/ausencia.py:766), os demais estados declarados são aprovada e rejeitada (app/ponto/services/ausencia.py:768) ; nao, o vocabulário coincide com o lookup (app/docs/JUIZES.md:57, app/ponto/services/ausencia.py:766) ; achado, Ausencia.save classifica aprovada/rejeitada como decisórias por tupla própria para carimbar aprovada_em, buscados ABERTAS e combinações literais dos estados (app/ponto/models.py:869, app/ponto/models.py:872) ; -

app/ponto/turnos.py::periodo_da_parcial_no_dia ; a batida do dia está dentro do período declarado pela ausência parcial? (app/core/juizes.py:261) ; L-006 (app/docs/LEIS.md:64), L-088 (app/docs/LEIS.md:120), COBERTURA-PARCIAL-PERIODO (app/docs/CORTES.md:319), JUIZ-PERIODO-DA-PARCIAL (app/docs/CORTES.md:323), parcial noturna (app/docs/CORTES.md:330), aceite de nao_sei (app/docs/CORTES.md:334) ; falta de qualquer ponta retorna sem_periodo, fim anterior ao início recebe mais 1440 minutos, alguma batida conhecida no intervalo inclusivo retorna dentro, nenhuma dentro com hora ilegível retorna nao_sei, restantes fora (app/ponto/turnos.py:2634, app/ponto/turnos.py:2638, app/ponto/turnos.py:2641, app/ponto/turnos.py:2643) ; nao, os cortes posteriores autorizam a virada e a quarta resposta implementadas (app/docs/CORTES.md:330, app/ponto/turnos.py:2638, app/docs/CORTES.md:334, app/ponto/turnos.py:2643) ; nenhum achado autônomo, procurados hora_ini, hora_fim e comparações das pontas, as ocorrências examinadas são o juiz e a porta de edição (app/ponto/turnos.py:2634, app/ponto/services/ausencia.py:894) ; -

app/ferias/services.py::em_gozo_hoje ; o colaborador está de férias hoje? (app/core/juizes.py:262) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; seleciona agendamentos cujas datas incluem hoje, excluindo cancelados, permite recortar colaborador/empresa e ordena por data_fim (app/ferias/services.py:53, app/ferias/services.py:64, app/ferias/services.py:65, app/ferias/services.py:67, app/ferias/services.py:69, app/ferias/services.py:70) ; nao, o contrato pelas datas corresponde à seleção (app/ferias/services.py:60, app/ferias/services.py:53) ; nenhum achado autônomo para essa pergunta, procurados em_gozo, AgendamentoFerias, situação férias e contenção de hoje pelas datas, bloqueador e aviso delegam ao juiz (app/ponto/utils.py:89, app/ponto/services/aviso_da_batida.py:74) ; -

app/ponto/turnos.py::afastado_hoje ; o colaborador está afastado hoje? (app/core/juizes.py:263) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado], o dossiê identifica a autoridade (app/docs/DOSSIES.md:75) ; retorna a ausência afastamento_inss que cobre o dia, incluindo intervalo aberto autorizado, pelo efeito folha/cobrança, com desempate por maior data/pk (app/ponto/turnos.py:2704, app/ponto/turnos.py:2715, app/ponto/turnos.py:2719, app/ponto/turnos.py:2721, app/ponto/turnos.py:2731) ; nao, o registro documenta a exceção posterior que mantém o campo sob vigia de divergência (app/core/juizes.py:451, app/core/juizes.py:461), cumprimento atual dessa condição [nao verificado] ; achado, lançamento de afastamento_inss grava situacao afastado sem consultar o juiz (app/ponto/views.py:2433, app/ponto/views.py:2435), afastado_na_batida também retorna True pelo campo quando nenhuma ausência cobre o dia, sinalizando divergência (app/ponto/services/afastado_avisa.py:39, app/ponto/services/afastado_avisa.py:41), buscados situacao afastado, TIPOS_AFASTADO e afastado_hoje ; -

app/colaboradores/services/desligamento.py::esta_desligado ; o colaborador está desligado? (app/core/juizes.py:267) ; JUIZ-ESTA-DESLIGADO-NASCE, estado situacao desligado, agendamento futuro e afastamento não equivalem a desligamento (app/docs/CORTES.md:450) ; compara getattr(colaborador, situacao, None) com desligado (app/colaboradores/services/desligamento.py:32) ; nao (app/docs/CORTES.md:450, app/colaboradores/services/desligamento.py:32) ; achado, julgar_vinculo responde pelo campo cru para recusar novo vínculo, buscados esta_desligado e comparações de situacao com desligado (app/colaboradores/services/vinculo.py:91, app/colaboradores/services/vinculo.py:92) ; -

app/ferias/models.py::PeriodoAquisitivo.dias_saldo ; qual o saldo do período aquisitivo? (app/core/juizes.py:268) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; dias_direito menos dias_concedidos menos dias_vendidos, sem piso zero (app/ferias/models.py:220) ; nao, a fórmula coincide com o contrato escrito (app/ferias/models.py:22, app/ferias/models.py:220) ; nenhum achado de fórmula autônoma, procurados dias_direito, dias_concedidos, dias_vendidos e _saldo, os wrappers delegam e status_de_periodo chama a propriedade (app/ferias/situacao.py:69, app/ferias/management/commands/pautas_de_ferias.py:97, app/ferias/models.py:98) ; -

app/ferias/models.py::status_de_periodo ; o período venceu ou está vencendo, art.137? (app/core/juizes.py:269) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; saldo não positivo retorna concedido, data_fim ausente ou ainda não passada retorna em_curso, limite de concessão passado retorna vencido, restante adquirido, vencendo_em_breve usa helper que exige adquirido e limite dentro da janela (app/ferias/models.py:99, app/ferias/models.py:101, app/ferias/models.py:103, app/ferias/models.py:105, app/ferias/models.py:230, app/ferias/models.py:260) ; sim, periodos_vencendo promete a mesma regra de vencendo_em_breve e afirma que vencidos entram (app/ferias/services.py:222), mas o helper dessa propriedade exige adquirido, excluindo o vencido retornado pelo juiz (app/ferias/models.py:260, app/ferias/models.py:103) ; achado, situacao_de_ferias determina vencido por limite passado e saldo positivo e vencendo por janela própria, periodos_vencendo filtra status gravado e limite, buscados status_de_periodo, vencido, vencendo e data_limite_concessao (app/ferias/situacao.py:193, app/ferias/situacao.py:207, app/ferias/services.py:226) ; vencendo inclui períodos vencidos (app/ferias/services.py:222) ou exige estado adquirido, excluindo vencidos (app/ferias/models.py:260, app/ferias/models.py:103)?

app/ferias/models.py::PeriodoAquisitivo.dias_vendaveis ; quantos dias ainda podem ser vendidos, abono de um terço? (app/core/juizes.py:270) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; máximo entre zero e dias_direito dividido por três com divisão inteira, menos dias_vendidos (app/ferias/models.py:238) ; nao, coincide com o teto descrito no módulo (app/ferias/models.py:23, app/ferias/models.py:238) ; nenhum achado de fórmula autônoma, procurados dias_vendaveis, dias_vendidos, dias_direito, divisão por três e um terço, os consumidores examinados leem a propriedade (app/ferias/services.py:312, app/ferias/services.py:501, app/core/views_ia.py:533) ; -

app/ferias/models.py::AgendamentoFerias.clean ; o fracionamento é válido, art.134? (app/core/juizes.py:271) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; considera agendado/em_gozo/concluido, exclui o próprio registro e recusa mais de três frações ou nenhuma fração de pelo menos 14 dias, também verifica datas e duração, o módulo declara não defender o mínimo de cinco dias das demais frações (app/ferias/models.py:362, app/ferias/models.py:366, app/ferias/models.py:374, app/ferias/models.py:379, app/ferias/models.py:383, app/ferias/models.py:18) ; sim, a tela exige mínimo de 14 dias na menor fração (app/templates/ferias/agendar.html:10), o juiz exige pelo menos uma fração de 14 dias (app/ferias/models.py:376) ; achado, leitor informativo publica regra própria para a menor fração, buscados fracionamento, frações, FRACAO_MINIMA e literal 14 (app/templates/ferias/agendar.html:10), o aviso que chama violacoes_de_fracionamento compartilha a implementação (app/ferias/situacao.py:227) ; a menor fração deve ter 14 dias (app/templates/ferias/agendar.html:10) ou basta pelo menos uma com 14 dias (app/ferias/models.py:376)?

app/ferias/situacao.py::DIAS_AVISO_PREVIO ; o aviso de 30 dias foi dado, art.135? (app/core/juizes.py:272) ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; a autoridade registrada é somente a constante 30, sem predicado que confira entrega do aviso (app/ferias/situacao.py:56) ; nao, o texto da tela também informa antecedência mínima de 30 dias (app/templates/ferias/agendar.html:11, app/ferias/situacao.py:56) ; achado, escritores calculam aviso_emitido pela diferença entre início e hoje, repetindo a comparação com a constante, o painel usa 30 literal para aviso pendente, buscados DIAS_AVISO_PREVIO, aviso_emitido e comparações com 30 (app/ferias/services.py:529, app/ferias/services.py:547, app/ferias/services.py:681, app/ferias/services.py:745, app/ferias/views.py:35) ; -

app/ponto/catalogo/ausencias.py::fim_de ; quanto a ausência dura, e o tipo é válido? (app/core/juizes.py:273) ; L-006 determina origem cadastral, sem estipular duração por tipo (app/docs/LEIS.md:64), nenhum corte específico localizado [nao verificado] ; devolve a gramática do fim no catálogo vigente, desconhecido ou informação ausente devolve por_lei, não calcula duração nem rejeita código desconhecido, codigos_validos é outra função (app/ponto/catalogo/ausencias.py:192, app/ponto/catalogo/ausencias.py:194, app/ponto/catalogo/ausencias.py:195, app/ponto/catalogo/ausencias.py:284) ; nao, o fallback para desconhecido está explicitamente descrito (app/ponto/catalogo/ausencias.py:188, app/ponto/catalogo/ausencias.py:194) ; achado, formulário mantém catálogo próprio de mínimos, máximos e dias fixos, com maternidade 120, paternidade cinco, luto dois e fallback máximo 30, buscados fim_de, dias_fixos, regras_do_tipo e CATALOGO (app/ponto/forms.py:13, app/ponto/forms.py:18, app/ponto/forms.py:38, app/ponto/forms.py:91, app/ponto/forms.py:113) ; -

app/ponto/services/alertas_ausencia.py::resumo_atestados ; há três ou mais lançamentos médicos na competência? (app/core/juizes.py:274) ; L-026 e L-090 para competência da empresa (app/docs/LEIS.md:77, app/docs/LEIS.md:123), L-006 para origem cadastral (app/docs/LEIS.md:64), PARAMETRO-GANHA-ROTULO registra consumo de TipoAusencia.medico (app/docs/CORTES.md:221), limiar três nesses textos [nao verificado] ; consulta cobertura para folha na competência da empresa, filtra TIPOS_MEDICOS e retorna registros distintos sobreviventes da cobertura e quantidade de dias, sem aplicar limiar três (app/ponto/services/alertas_ausencia.py:45, app/ponto/services/alertas_ausencia.py:48, app/ponto/services/alertas_ausencia.py:50, app/ponto/services/alertas_ausencia.py:51, app/ponto/services/alertas_ausencia.py:52) ; sim quanto ao contrato literal, o texto afirma contar dias e não registros (app/ponto/services/alertas_ausencia.py:17), mas o juiz também conta e publica registros (app/ponto/services/alertas_ausencia.py:51, app/ponto/services/alertas_ausencia.py:56) ; achado, leitores aplicam registros >= 3 fora do juiz, usando a contagem oficial, buscados resumo_atestados, registros e limiar três (app/ponto/views.py:2224, app/ponto/views.py:2447) ; -

Contagem das linhas acima: juízes lidos == 16, sim, correspondendo ao registro (app/core/juizes.py:251). Coluna (5) com sim: 5. Coluna (6) com achado: 10. Coluna (7) preenchida: 3. Contagens derivadas das ocorrências e fontes citadas nas respectivas linhas.

## 14/11 08:48 · sessao cdef69ceea2b · JUSTIFICATIVA-PAGA · leitura · origem=Code

Convenção: nas referências aos anexos colados, a primeira linha `F` ou `N` é a linha 1, sem contar cabeçalhos.

**1. Contagem de N por mês, motivo e dia_final**

Fontes integrais: mês 9 — `N_depois.txt:1–357`; mês 10 — `N_depois.txt:358–669`; mês 11 — `N_depois.txt:670–746`.

| motivo | dia_final | Mês 9 | Mês 10 | Mês 11 |
|---|---|---:|---:|---:|
| coberto | hf | 150 | 122 | 25 |
| turno_fechado_na_ponta | nada | 155 | 141 | 44 |
| ancora_papel_errado | nada | 23 | 16 | 3 |
| ancora_em_turno_de_outro_dia | nada | 22 | 20 | 2 |
| sem_vinculo_ou_marcos | nada | 4 | 2 | 0 |
| cruza_trabalhado | nada | 1 | 8 | 1 |
| ancora_fora_de_turno | nada | 2 | 3 | 2 |
| **Total** | | **357** | **312** | **77** |

**Total dos três meses: 746 N.** (`N_depois.txt:1–746`)

**2. Diferenças de datas por colaborador e competência**

Com corte default 21, as janelas inclusivas são: **9 = 2026-08-21…2026-09-20; 10 = 2026-09-21…2026-10-20; 11 = 2026-10-21…2026-11-20**. (`app/ponto/janelas.py:15–21`, `app/ponto/janelas.py:50–57`)

Em **todos os 38 pares colab-comp fornecidos**, `antes − depois = ∅`. A tabela apresenta `depois − antes`; cada coluna mensal representa o respectivo colab-comp. (`F_diferentes.txt:1–76`)

Nas células, **fora/dentro** indica a posição das datas listadas em relação à janela daquele mês. Quando há duas datas, a indicação vale para ambas. “Sem N” significa ausência de N no mesmo mês/colab/data, conferida nos blocos completos do mês: `N_depois.txt:1–357`, `N_depois.txt:358–669`, `N_depois.txt:670–746`.

| Colab | Datas somente no depois | Mês 9: motivo N; janela | Mês 10: motivo N; janela | Mês 11: motivo N; janela | Referências F: antes/depois por mês |
|---|---|---|---|---|---|
| 1004 | 2026-10-27 | sem N; **fora** | sem N; **fora** | ancora_papel_errado; **dentro** (`N_depois.txt:746`) | 9: `F_diferentes.txt:26/64`; 10: `F_diferentes.txt:1/39`; 11: `F_diferentes.txt:14/52` |
| 134 | 2026-06-22, 2026-06-24 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:27/65`; 10: `F_diferentes.txt:2/40`; 11: `F_diferentes.txt:15/53` |
| 29 | 2026-06-17 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:28/66`; 10: `F_diferentes.txt:3/41`; 11: `F_diferentes.txt:16/54` |
| 371 | 2026-08-08 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:29/67`; 10: `F_diferentes.txt:4/42`; 11: `F_diferentes.txt:17/55` |
| 44 | 2026-05-21 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:30/68`; 10: `F_diferentes.txt:5/43`; 11: `F_diferentes.txt:18/56` |
| 46 | 2026-05-23, 2026-05-26 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:31/69`; 10: `F_diferentes.txt:6/44`; 11: `F_diferentes.txt:19/57` |
| 707 | 2026-10-26 | sem N; **fora** | sem N; **fora** | ancora_fora_de_turno; **dentro** (`N_depois.txt:711`) | 9: `F_diferentes.txt:32/70`; 10: `F_diferentes.txt:7/45`; 11: `F_diferentes.txt:20/58` |
| 709 | 2026-08-25 | ancora_papel_errado; **dentro** (`N_depois.txt:210`) | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:33/71`; 10: `F_diferentes.txt:8/46`; 11: `F_diferentes.txt:21/59` |
| 712 | 2026-09-07 | ancora_fora_de_turno; **dentro** (`N_depois.txt:213`) | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:34/72`; 10: `F_diferentes.txt:9/47`; 11: `F_diferentes.txt:22/60` |
| 788 | ∅ | ∅ | ∅ | ∅ | 9: `F_diferentes.txt:35/73`; 10: `F_diferentes.txt:10/48`; 11: `F_diferentes.txt:23/61` |
| 81 | 2026-06-28 | sem N; **fora** | sem N; **fora** | sem N; **fora** | 9: `F_diferentes.txt:36/74`; 10: `F_diferentes.txt:11/49`; 11: `F_diferentes.txt:24/62` |
| 922 | ∅ | ∅ | ∅ | ∅ | 9: `F_diferentes.txt:37/75`; 10: `F_diferentes.txt:12/50`; 11: `F_diferentes.txt:25/63` |
| 993 | 2026-10-20 | sem N; **fora** | ancora_papel_errado; **dentro** (`N_depois.txt:669`) | Não consta colab-comp em F_diferentes | 9: `F_diferentes.txt:38/76`; 10: `F_diferentes.txt:13/51`; ausência no bloco 11: `F_diferentes.txt:14–25`, `F_diferentes.txt:52–63` |

**3. Somas mensais de depois − antes**

| Mês | Σ Δ DSR | Σ Δ dias_falta | Fonte |
|---|---:|---:|---|
| 9 | **+11** | **+13** | `F_diferentes.txt:26–38`, `F_diferentes.txt:64–76` |
| 10 | **+11** | **+13** | `F_diferentes.txt:1–13`, `F_diferentes.txt:39–51` |
| 11 | **+10** | **+12** | `F_diferentes.txt:14–25`, `F_diferentes.txt:52–63` |

**4. Caminho que inclui datas fora da janela pedida**

Na cópia, o caminho demonstrável é:

1. `espelho_do_colab` recebe `apur_ini/apur_fim`, mas calcula `vis_ini = min(piso_visual(colaborador, hoje), apur_ini)` e chama `autoridade_do_periodo(colaborador, vis_ini, hoje)`. Portanto, a janela entregue à autoridade pode começar antes de `apur_ini` e terminar depois de `apur_fim`. (`app/ponto/services/espelho.py:1226–1230`)

2. A autoridade monta os efeitos de ausência e os dias previstos nessa janela ampliada; injeta `justificadas` e `datas_previstas_trabalho` no motor. (`app/ponto/services/espelho.py:1068–1070`, `app/ponto/services/espelho.py:1111–1115`)

3. Na fonte das justificativas, somente cobertura inteira (`None`) ou cobertura não vazia põe a data em `justificadas`. Cobertura vazia não a põe nesse conjunto, ressalvado eventual abono por ausência no processamento seguinte. (`app/ponto/services/efeito_ausencia.py:45–46`, `app/ponto/services/efeito_ausencia.py:55–62`)

4. O motor recorta os dias ao **período que ele recebeu**; escolhe os previstos sem trabalho e sem justificativa. **O sítio que acrescenta as datas é `resultado.datas_falta.extend(sorted(faltas_injustificadas))`.** (`app/ponto/motor_calculo_v2.py:2594`, `app/ponto/motor_calculo_v2.py:2601–2615`)

5. Havendo fatias, a composição concatena `datas_falta` dos resultados, sem novo recorte pela competência pedida ao espelho. (`app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:286–297`)

6. O espelho filtra **a lista derivada do resumo**, `datas_furo_apurado`, por `apur_ini/apur_fim`; devolve também o objeto `resultado`, sem aplicar esse filtro à sua `datas_falta`. (`app/ponto/services/espelho.py:1250–1252`, `app/ponto/services/espelho.py:1295–1296`)

Esse caminho explica como o campo bruto pode conter datas fora da competência sem ultrapassar o período recebido pelo próprio motor. A atribuição individual das datas antigas a determinada justificativa/marco não está demonstrada pelos N fornecidos: **[nao verificado]**. Qual trecho desse caminho difere do main `3bbe58e86`: **[nao verificado]**.

Os demais leitores identificados na cópia são:

| Leitor do campo bruto | Filtra pela janela? | Referência |
|---|---|---|
| Resumo do espelho | **Sim**, `apur_ini <= d <= apur_fim` | `app/ponto/services/espelho.py:1250–1252` |
| Calendário | **Sim**, `data_ini <= d <= data_fim` | `app/colaboradores/services/calendario.py:572–574` |
| PDF: `resumo['datas_falta']` e `resumo['datas_furo_apurado']` | **Não**, copia e ordena | `app/relatorios/pdf_espelho.py:463–464` |
| Fechamento → `_furos_encerrados` | **Não recorta a competência**; exclui isentos e exige `dia_encerrado` | `app/ponto/services/fechamento.py:1120–1122`, `app/ponto/services/fechamento.py:366–378` |
| Composição das fatias | **Não**, concatena | `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:294–297` |
| Diagnóstico `desvio_o68b` | **Não**, imprime a lista inteira | `app/ponto/management/commands/desvio_o68b.py:135` |
| Diagnóstico `diff_espelho_alimentacao` | **Não**, compara comprimentos da lista | `app/ponto/management/commands/diff_espelho_alimentacao.py:28`, `app/ponto/management/commands/diff_espelho_alimentacao.py:129–143` |

Depois do filtro `_furos_encerrados`, a lista alimenta o coletor e as duas chamadas de lavratura; a lavratura cria linha para cada data recebida, sem recorte nesse ponto. (`app/ponto/services/fechamento.py:1132`, `app/ponto/services/fechamento.py:1231`, `app/ponto/services/fechamento.py:1298`, `app/ponto/services/dia_pago.py:281–283`)

O consumidor indireto `datas_em_aberto_de` lê `datas_furo_apurado` com **teto `d <= ate`, sem piso de janela**. (`app/ponto/services/dia_decidido.py:316`)

Já `folha.export.datas_falta` consulta as ausências que descontam; essa função não lê o campo `ResultadoMes.datas_falta`. (`app/folha/export.py:179–185`)

## 14/11 08:46 · sessao 048c3e3e5343 · - · - · origem=Ronald (tmux)

Numeração lógica: primeira linha `N` = linha 1 do anexo.

| Motivo | dia_final | Mês 10 | Mês 11 |
|---|---|---:|---:|
| coberto | hf | 122 | 25 |
| turno_fechado_na_ponta | nada | 141 | 44 |
| ancora_papel_errado | nada | 16 | 3 |
| ancora_em_turno_de_outro_dia | nada | 20 | 2 |
| ancora_fora_de_turno | nada | 3 | 2 |
| cruza_trabalhado | nada | 8 | 1 |
| sem_vinculo_ou_marcos | nada | 2 | 0 |
| **Total** | | **312** | **77** |

Fonte integral das contagens: `N_depois.txt:358–669` (mês 10), `N_depois.txt:670–746` (mês 11). Somados aos 357 do mês 9 confirmados pelo outro explorer, fecham 746 linhas.

Referências exatas solicitadas:

- Mês 10, col993, `2026-10-20`: `ancora_papel_errado`, `nada` — `N_depois.txt:669`.
- Mês 11, col707, `2026-10-26`: `ancora_fora_de_turno`, `nada` — `N_depois.txt:711`.
- Mês 11, col1004, `2026-10-27`: `ancora_papel_errado`, `nada` — `N_depois.txt:746`.

Revisão concluída: transcrição por colaborador/motivo e soma em JavaScript, sem criar arquivos.

## 14/11 08:46 · sessao fb9c0423e967 · - · - · origem=Ronald (tmux)

Leitores do campo cru na cópia:

| Consumidor | Referência | Filtra pela janela? |
|---|---|---|
| Resumo do espelho | `app/ponto/services/espelho.py:1250`–`:1252` | Sim: `apur_ini <= d <= apur_fim`. |
| Calendário | `app/colaboradores/services/calendario.py:572`–`:574` | Sim: `data_ini <= d <= data_fim`. |
| PDF, `resumo['datas_falta']` | `app/relatorios/pdf_espelho.py:463` | Não: copia e ordena toda a lista. |
| PDF, `resumo['datas_furo_apurado']` | `app/relatorios/pdf_espelho.py:464` | Não: copia e ordena toda a lista. |
| Fechamento | `app/ponto/services/fechamento.py:1120`–`:1122` | Não recorta `[ini,fim]`: passa a `_furos_encerrados`. Esse filtro exclui isentos e pergunta `dia_encerrado`, sem limites de competência (`:366`–`:378`). |
| Composição das fatias | `app/ponto/services/fechamento.py:96`, `:294`–`:297` | Não: concatena os valores de `datas_falta` das fatias, sem recorte nem deduplicação. |
| Diagnóstico `desvio_o68b` | `app/ponto/management/commands/desvio_o68b.py:135` | Não: imprime toda a lista. |
| Diagnóstico `diff_espelho_alimentacao` | `app/ponto/management/commands/diff_espelho_alimentacao.py:28`, `:129`–`:143` | Não: compara comprimentos do campo cru. |

O **objeto `resultado` devolvido por `espelho_do_colab` permanece cru**, mesmo havendo recorte em `resumo['datas_furo_apurado']`: o retorno devolve diretamente o mesmo objeto em `app/ponto/services/espelho.py:1295`–`:1296`.

Os consumidores indiretos do fechamento recebem a lista após `_furos_encerrados`: coletor em `app/ponto/services/fechamento.py:1132`; lavratura `motor` em `:1231`; lavratura `oraculo` em `:1298`. A lavratura converte todas as datas recebidas em conjunto e cria linha para cada uma, sem recorte (`app/ponto/services/dia_pago.py:281`–`:283`), marcando `furo_apurado` pela pertinência ao conjunto (`:345`–`:346`).

O derivador indireto de `datas_em_aberto` lê `datas_furo_apurado`, mas **não aplica piso de janela**: aceita `d <= ate` em `app/ponto/services/dia_decidido.py:316`. O teto vem de `ate_ontem`, que devolve `min(data_fim, hoje - 1 dia)` (`app/relatorios/cartao_pela_celula.py:37`–`:41`), passado ao derivador em `:412`–`:419`. O PDF chama esse fluxo em `app/relatorios/pdf_espelho.py:629`; espelho em `app/ponto/services/espelho.py:1285`; calendário em `app/colaboradores/services/calendario.py:583`.

Não confundir esse campo com `folha.export.datas_falta`: a função da folha consulta ausências do catálogo (`app/folha/export.py:179`–`:185`) e sua expansão recorta datas por período (`:160`–`:162`); ela não lê `ResultadoMes.datas_falta`.

## 14/11 08:46 · sessao 4e9d037a80d2 · - · - · origem=Ronald (tmux)

Mês 9: **357 N**, linhas lógicas **1–357** do anexo, numerando a primeira N como linha 1. Fonte: `N_depois.txt:1`.

| Motivo | dia_final | Quantidade | Primeira ocorrência |
|---|---|---:|---|
| coberto | hf | 150 | `N_depois.txt:1` |
| turno_fechado_na_ponta | nada | 155 | `N_depois.txt:3` |
| ancora_papel_errado | nada | 23 | `N_depois.txt:6` |
| ancora_em_turno_de_outro_dia | nada | 22 | `N_depois.txt:29` |
| sem_vinculo_ou_marcos | nada | 4 | `N_depois.txt:24` |
| cruza_trabalhado | nada | 1 | `N_depois.txt:41` |
| ancora_fora_de_turno | nada | 2 | `N_depois.txt:213` |

Joins solicitados:

- col709, 2026-08-25: `ancora_papel_errado`, `dia_final=nada`, j1710 — `N_depois.txt:210`.
- col712, 2026-09-07: `ancora_fora_de_turno`, `dia_final=nada`, j2023 — `N_depois.txt:213`.

Contagem feita com 155 sequências por colaborador codificadas em memória e somadas com JS; conferência independente das classes raras: 23 + 22 + 4 + 1 + 2. Soma com os outros meses: [nao verificado].

## 14/11 08:43 · sessao 1f15ef3d0e49 · JUSTIFICATIVA-PAGA · sonda · origem=Code

O dado termina em `F|depois|10|893` incompleto e não contém `F|depois|11`. As comparações usam somente pares F completos: mês 9 completo no trecho; mês 10 até col892. Totais integrais dos meses 10 e 11: [nao verificado].

**1. Contagem das linhas N recebidas**

| motivo | dia_final | mês 9 | mês 10 | referência |
|---|---|---:|---:|---|
| coberto | hf | 150 | 92 | [nao verificado] |
| turno_fechado_na_ponta | nada | 155 | 104 | [nao verificado] |
| ancora_papel_errado | nada | 23 | 12 | [nao verificado] |
| ancora_em_turno_de_outro_dia | nada | 22 | 14 | [nao verificado] |
| ancora_fora_de_turno | nada | 2 | 3 | [nao verificado] |
| cruza_trabalhado | nada | 1 | 5 | [nao verificado] |
| sem_vinculo_ou_marcos | nada | 4 | 1 | [nao verificado] |
| **Total** | | **357** | **231** | [nao verificado] |

Linhas N com `dia_final=hi` ou inteiro: **0**. Linhas N do mês 11 recebidas: **0**. Total N recebido: **588**. [nao verificado]

**2. Todas as datas acrescentadas nos pares completos recebidos**

Janelas usadas: mês 9 = `2026-08-21..2026-09-20`; mês 10 = `2026-09-21..2026-10-20`, pelo cálculo em [app/ponto/janelas.py:10](/home/ronald/codex_ws/eb584c2b0deeae76616b346f07f9356510730aa7/app/ponto/janelas.py:10).

| mês | colab | data | motivo N do mesmo mês/data | fora da janela | referência |
|---:|---:|---|---|---|---|
| 9 | 29 | 2026-06-17 | sem N | sim | [nao verificado] |
| 9 | 44 | 2026-05-21 | sem N | sim | [nao verificado] |
| 9 | 46 | 2026-05-23 | sem N | sim | [nao verificado] |
| 9 | 46 | 2026-05-26 | sem N | sim | [nao verificado] |
| 9 | 81 | 2026-06-28 | sem N | sim | [nao verificado] |
| 9 | 134 | 2026-06-22 | sem N | sim | [nao verificado] |
| 9 | 134 | 2026-06-24 | sem N | sim | [nao verificado] |
| 9 | 371 | 2026-08-08 | sem N | sim | [nao verificado] |
| 9 | 707 | 2026-10-26 | sem N | sim | [nao verificado] |
| 9 | 709 | 2026-08-25 | ancora_papel_errado | não | [nao verificado] |
| 9 | 712 | 2026-09-07 | ancora_fora_de_turno | não | [nao verificado] |
| 9 | 993 | 2026-10-20 | sem N | sim | [nao verificado] |
| 9 | 1004 | 2026-10-27 | sem N | sim | [nao verificado] |
| 10 | 29 | 2026-06-17 | sem N | sim | [nao verificado] |
| 10 | 44 | 2026-05-21 | sem N | sim | [nao verificado] |
| 10 | 46 | 2026-05-23 | sem N | sim | [nao verificado] |
| 10 | 46 | 2026-05-26 | sem N | sim | [nao verificado] |
| 10 | 81 | 2026-06-28 | sem N | sim | [nao verificado] |
| 10 | 134 | 2026-06-22 | sem N | sim | [nao verificado] |
| 10 | 134 | 2026-06-24 | sem N | sim | [nao verificado] |
| 10 | 371 | 2026-08-08 | sem N | sim | [nao verificado] |
| 10 | 707 | 2026-10-26 | sem N | sim | [nao verificado] |
| 10 | 709 | 2026-08-25 | sem N | sim | [nao verificado] |
| 10 | 712 | 2026-09-07 | sem N | sim | [nao verificado] |

**3. Contagens do item 2 e somas das diferenças**

| motivo | mês 9 | mês 10, trecho completo | total observado | referência |
|---|---:|---:|---:|---|
| sem N | 11 | 11 | 22 | [nao verificado] |
| ancora_papel_errado | 1 | 0 | 1 | [nao verificado] |
| ancora_fora_de_turno | 1 | 0 | 1 | [nao verificado] |
| demais motivos | 0 | 0 | 0 | [nao verificado] |
| **Total** | **13** | **11** | **24** | [nao verificado] |

| mês / cobertura | Σ(dsr depois − antes) | Σ(dias_falta depois − antes) | referência |
|---|---:|---:|---|
| 9, pares completos recebidos | +11 | +13 | [nao verificado] |
| 10, pares completos até col892 | +9 | +11 | [nao verificado] |
| 11 | indeterminado | indeterminado | [nao verificado] |

**4. Datas retiradas das faltas**

Nos pares completos recebidos: mês 9 = **0**; mês 10 = **0**. Lista vazia, sem motivos a associar. Mês 11: indeterminado. [nao verificado]

**5. Linhas ERRO**

**0** linhas `ERRO` no trecho recebido. [nao verificado]

## 14/11 08:43 · sessao 67cb87235023 · - · - · origem=Ronald (tmux)

ferias/services.py::em_gozo_hoje (app/core/juizes.py:262) ; o colaborador esta de ferias hoje? (app/core/juizes.py:262) ; nenhum texto específico encontrado em LEIS.md/CORTES.md para este predicado, pesquisados em_gozo_hoje, ferias, férias, S4-FERIAS-HOJE [nao verificado quanto a formulações sem esses termos] ; datas do agendamento incluem hoje, exclui somente cancelado, aceita filtros colaborador/empresa, ordena por data_fim (app/ferias/services.py:53, app/ferias/services.py:64, app/ferias/services.py:65, app/ferias/services.py:67, app/ferias/services.py:69, app/ferias/services.py:70) ; nao (a descrição pelas datas corresponde ao código: app/ferias/services.py:60, app/ferias/services.py:53) ; nenhum achado de decisor autônomo para hoje, procurados AgendamentoFerias, em_gozo, situacao ferias, data_inicio__lte=hoje, data_fim__gte=hoje; bloqueador e aviso delegam (app/ponto/utils.py:89, app/ponto/services/aviso_da_batida.py:74); status_por_data responde estado do agendamento, pergunta diferente (app/ferias/models.py:277) ; -

ponto/turnos.py::afastado_hoje (app/core/juizes.py:263) ; o colaborador esta afastado hoje? (app/core/juizes.py:263) ; nenhum texto específico encontrado em LEIS.md/CORTES.md para o predicado, pesquisados afastado, afastamento, afastado_hoje [nao verificado quanto a formulações sem esses termos]; DOSSIES identifica a autoridade e o escritor paralelo (app/docs/DOSSIES.md:75) ; retorna a Ausencia afastamento_inss que cobre o dia: data única, intervalo fechado ou aberto de tipo autorizado, filtra efeito folha=aprovada/cobranca=não rejeitada, desempata maior data e pk (app/ponto/turnos.py:2704, app/ponto/turnos.py:2715, app/ponto/turnos.py:2719, app/ponto/turnos.py:2721, app/ponto/turnos.py:2731, app/ponto/turnos.py:2516, app/ponto/turnos.py:2518) ; nao, o registro explicita a exceção posterior que mantém o campo enquanto divergência zero (app/core/juizes.py:451, app/core/juizes.py:461) ; achado: lançar afastamento_inss grava situacao afastado sem perguntar ao juiz (app/ponto/views.py:2433, app/ponto/views.py:2435); afastado_na_batida retorna True pelo campo mesmo sem ausência (app/ponto/services/afastado_avisa.py:41, app/ponto/services/afastado_avisa.py:42); buscados situacao afastado, afastado_hoje, TIPOS_AFASTADO ; -

colaboradores/services/desligamento.py::esta_desligado (app/core/juizes.py:267) ; o colaborador esta desligado? (app/core/juizes.py:267) ; L-NNN específico nenhum texto encontrado; corte JUIZ-ESTA-DESLIGADO-NASCE define estado gravado situacao desligado, não data futura nem afastado (app/docs/CORTES.md:450) ; getattr(colaborador, situacao, None) == desligado (app/colaboradores/services/desligamento.py:32) ; nao (app/docs/CORTES.md:450, app/colaboradores/services/desligamento.py:32) ; achado: julgar_vinculo decide pelo mesmo campo cru e recusa vínculo (app/colaboradores/services/vinculo.py:91, app/colaboradores/services/vinculo.py:92); buscados situacao desligado, esta_desligado ; -

ferias/models.py::PeriodoAquisitivo.dias_saldo (app/core/juizes.py:268) ; qual o saldo do periodo aquisitivo? (app/core/juizes.py:268) ; nenhum texto específico encontrado em LEIS.md/CORTES.md, pesquisados dias_saldo, saldo, aquisitivo [nao verificado quanto a formulações sem esses termos]; JUIZES somente aponta o endereço (app/docs/JUIZES.md:57, app/docs/JUIZES.md:58) ; dias_direito menos dias_concedidos menos dias_vendidos, sem piso zero (app/ferias/models.py:220) ; nao (a regra escrita corresponde à fórmula: app/ferias/models.py:22, app/ferias/models.py:220) ; nenhum achado, procuradas expressões dias_direito menos dias_concedidos, dias_concedidos e dias_vendidos, _saldo e templates; wrappers delegam (app/ferias/situacao.py:69, app/ferias/management/commands/pautas_de_ferias.py:97), status_de_periodo chama fget (app/ferias/models.py:98) ; -

ferias/models.py::status_de_periodo (app/core/juizes.py:269) ; o periodo venceu ou esta vencendo (art.137)? (app/core/juizes.py:269) ; nenhum texto específico encontrado em LEIS.md/CORTES.md, pesquisados status_de_periodo, vencimento, aquisitivo, concessao, art137 [nao verificado quanto a formulações sem esses termos]; JUIZES aponta status_de_periodo como autoridade de vencimento (app/docs/JUIZES.md:57, app/docs/JUIZES.md:58) ; saldo <=0 retorna concedido; data_fim ausente ou >=hoje retorna em_curso; limite_concessao <hoje retorna vencido; restante adquirido; não retorna vencendo (app/ferias/models.py:98, app/ferias/models.py:99, app/ferias/models.py:101, app/ferias/models.py:103, app/ferias/models.py:105) ; sim: texto promete vencido ou vencendo saindo deste juiz (app/ferias/models.py:20), mas o retorno residual é adquirido sem qualquer teste de janela para vencendo (app/ferias/models.py:105; ramos completos app/ferias/models.py:99, app/ferias/models.py:101, app/ferias/models.py:103) ; achado: situacao_de_ferias decide vencido somente por limite vencido e saldo positivo, ignorando a precedência de data_fim do juiz (app/ferias/situacao.py:193, app/ferias/situacao.py:194), e vencendo por janela própria (app/ferias/situacao.py:207, app/ferias/situacao.py:209); periodos_vencendo também filtra status gravado com janela própria (app/ferias/services.py:226, app/ferias/services.py:227); buscados data_limite_concessao, vencido, vencendo, status_de_periodo ; vencendo inclui períodos vencidos? candidata escrita inclui: “Vencidos entram” (app/ferias/services.py:222); candidata do helper do juiz exige adquirido (app/ferias/models.py:260), enquanto limite vencido resulta vencido (app/ferias/models.py:103, app/ferias/models.py:104)

Observação para consolidação: a coluna 7 da última linha é defensável apenas se “candidatas escritas” abranger descrição de função versus predicado de código. Se exigir duas candidatas normativas em LEIS/CORTES, use “-”: não encontrei esse par normativo nesses arquivos. A inconsistência literal “vencendo” prometido mas ausente é clara, porém o juiz também pode ser entendido como só o primeiro estágio de composição com vencendo, que o chama (app/ferias/models.py:255, app/ferias/models.py:260).

Contagem deste recorte: 5 juízes lidos (app/core/juizes.py:262, app/core/juizes.py:263, app/core/juizes.py:267, app/core/juizes.py:268, app/core/juizes.py:269); 1 conflito literal; 3 com achado externo; 1 pergunta de lei sob a interpretação ampla acima, zero sob interpretação normativa estrita.

## 14/11 08:43 · sessao e130c04c3e51 · - · - · origem=Ronald (tmux)

app/ponto/catalogo/ausencias.py::efeito_vigente ; a ausencia paga, desconta ou suprime o dia? (app/core/juizes.py:252) ; L-006 (app/docs/LEIS.md:64), CATALOGO-SAIDA-ANTECIPADA-DESCONTA (app/docs/CORTES.md:202) ; devolve linha[0] do catalogo vigente, ou None (app/ponto/catalogo/ausencias.py:293), cadastro ativo com fallback para CATALOGO (app/ponto/catalogo/ausencias.py:249, app/ponto/catalogo/ausencias.py:278), cadastro de producao [nao verificado] ; sim: corte determina saida_antecipada=DESCONTA (app/docs/CORTES.md:202), fallback escrito define SUPRIME (app/ponto/catalogo/ausencias.py:91) ; nenhum achado autonomo inequivoco no grep por efeito_vigente, TIPOS_PAGOS, TIPOS_ABONAM, TIPOS_DESCONTAM, .efeito e comparacoes literais de tipo, consumidores encontrados delegam ao catalogo (app/ponto/models.py:841, app/ponto/catalogo/ausencias.py:455), exaustividade [nao verificado] ; -

app/ponto/turnos.py::cobertura_ausencia_periodo ; que dias a ausencia ocupa? (app/core/juizes.py:253) ; nenhum texto especifico de L-NNN/CORTES encontrado para a gramatica completa de range [nao verificado], autoridade de cobertura declarada em app/docs/JUIZES.md:55 ; data..data_fim inclusive, sem fim estende ate fim consultado somente em TIPOS_EM_ABERTO, demais ficam em data, recorta janela e vence maior data/pk (app/ponto/turnos.py:2753, app/ponto/turnos.py:2772, app/ponto/turnos.py:2781), folha somente aprovada e cobranca nao rejeitada (app/ponto/turnos.py:2516) ; nao (app/ponto/turnos.py:2737, app/ponto/turnos.py:2772) ; achado: houve_fato_novo rederiva se a ausencia alcanca o dia, tratando data_fim nulo somente como data=data, sem excecao de range aberto (app/ponto/services/cartorio.py:383), outro expansor proprio usa fim=data_fim or data (app/ponto/services/dia_decidido.py:558), este ultimo declara pergunta de topo diferente, incluindo rejeitadas (app/ponto/services/dia_decidido.py:532) ; -

app/ponto/precedencia.py::veto_de_lancamento ; o lancamento colide com outro fato (recusar)? (app/core/juizes.py:254) ; L-089 para falta em dia sem jornada (app/docs/LEIS.md:121), RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER para comunicar recusas e fatos da sobreposicao (app/docs/CORTES.md:338) ; recusa sobreposicao que linha_que_veta reprova (app/ponto/precedencia.py:461), recusa folga lancada exceto travessia de ferias (app/ponto/precedencia.py:496, app/ponto/precedencia.py:514), falta recusa quando vinculo responde explicitamente dia sem trabalho (app/ponto/precedencia.py:536), senao None (app/ponto/precedencia.py:545) ; nao (app/docs/LEIS.md:121, app/ponto/precedencia.py:538) ; achado com ressalva: previa do chat responde erro de colisao compondo linha_que_veta e ausencias_sobrepostas, sem chamar veto_de_lancamento nem avaliar folga lancada (app/ponto/services/checar_ausencia.py:35), recorte exclui ferias, falta e INSS (app/ponto/services/checar_ausencia.py:10) ; -

app/ponto/services/ausencia.py::estado_inicial ; a ausencia exige documento, e em que prazo? (app/core/juizes.py:255) ; nenhum texto especifico de L-NNN/CORTES encontrado para estados e sete dias [nao verificado], exclusao de ferias atribuida a corte no proprio codigo (app/ponto/services/ausencia.py:748) ; anexo ou ferias resulta aguardando_decisao sem prazo, tipo exigente sem anexo resulta aguardando_documento com prazo, demais aguardando_decisao (app/ponto/services/ausencia.py:792), prazo sete dias a partir da data local de criacao (app/ponto/services/ausencia.py:745, app/ponto/services/ausencia.py:339) ; nao (app/ponto/services/ausencia.py:748, app/ponto/services/ausencia.py:794) ; achado com ressalva: relatorio responde exigencia lendo diretamente linha[2] do catalogo, sem estado_inicial nem exclusao de ferias (app/ponto/services/atestados_lote.py:25), declara pergunta mais estreita sobre flag cadastral (app/ponto/services/atestados_lote.py:26), enquanto exigencia ao colaborador delega ao juiz (app/ponto/models.py:724, app/ponto/models.py:728) ; -

app/ponto/services/ausencia.py::ABERTAS ; a ausencia esta aberta ou decidida? (app/core/juizes.py:256) ; nenhum texto especifico de L-NNN/CORTES encontrado [nao verificado], ABERTAS e ausencia de status pendente declarados em app/docs/JUIZES.md:57 ; abertas sao aguardando_documento e aguardando_decisao (app/ponto/services/ausencia.py:766), aprovadas e rejeitadas sao os outros estados declarados (app/ponto/models.py:701) ; nao (app/docs/JUIZES.md:57, app/ponto/services/ausencia.py:766) ; achado: Ausencia.save classifica aprovada/rejeitada como decisorias por tupla propria para carimbar aprovada_em (app/ponto/models.py:869, app/ponto/models.py:872), esta_aberta efetivamente delega a ABERTAS (app/ponto/models.py:719) ; -

app/ponto/turnos.py::periodo_da_parcial_no_dia ; a batida do dia esta dentro do periodo que a ausencia parcial declara? (app/core/juizes.py:261) ; L-006 (app/docs/LEIS.md:64), L-088 (app/docs/LEIS.md:120), COBERTURA-PARCIAL-PERIODO (app/docs/CORTES.md:319), JUIZ-PERIODO-DA-PARCIAL (app/docs/CORTES.md:323), parcial noturna (app/docs/CORTES.md:330), aceite de nao_sei (app/docs/CORTES.md:334) ; falta qualquer ponta resulta sem_periodo, fim menor que inicio recebe 1440 minutos, alguma batida conhecida no intervalo inclusivo resulta dentro, nenhuma conhecida dentro com hora ilegivel resulta nao_sei, restantes fora (app/ponto/turnos.py:2634) ; nao (app/docs/CORTES.md:330, app/ponto/turnos.py:2638, app/docs/CORTES.md:334, app/ponto/turnos.py:2643) ; nenhum achado autonomo no grep por a.hora_ini, ausencia.hora_ini, hora_ini/hora_fim e comparacoes de pontas, ocorrencias encontradas no juiz e na porta de edicao (app/ponto/turnos.py:2634, app/ponto/services/ausencia.py:894), exaustividade [nao verificado] ; -

Contagem deste recorte: seis juizes lidos (app/core/juizes.py:252, app/core/juizes.py:261), um conflito demonstrado (app/docs/CORTES.md:202, app/ponto/catalogo/ausencias.py:91), quatro linhas com candidatos externos, sendo duas exigindo avaliacao de identidade da pergunta e uma expansao com pergunta de topo diferente (app/ponto/services/cartorio.py:383, app/ponto/services/checar_ausencia.py:35, app/ponto/services/atestados_lote.py:25, app/ponto/models.py:872), nenhuma pergunta de lei sustentada por duas candidatas normativas neste recorte [nao verificado].

## 14/11 08:43 · sessao b7b78710a686 · - · - · origem=Ronald (tmux)

12) ferias/models.py::PeriodoAquisitivo.dias_vendaveis ; quantos dias ainda podem ser vendidos (abono, 1/3)? (app/core/juizes.py:270) ; nenhum texto específico encontrado em LEIS.md/CORTES.md para o teto do abono [nao verificado] ; max(0, dias_direito // 3 - dias_vendidos) (app/ferias/models.py:238) ; nao ; nenhum achado de fórmula autônoma, procurados dias_vendaveis, dias_vendidos, dias_direito, //3 e 1/3, os consumidores encontrados perguntam à propriedade (app/ferias/services.py:312, app/ferias/services.py:501, app/ferias/services.py:890, app/core/views_ia.py:533) ; -

13) ferias/models.py::AgendamentoFerias.clean ; o fracionamento e valido (art.134)? (app/core/juizes.py:271) ; nenhum texto específico encontrado em LEIS.md/CORTES.md para as frações [nao verificado] ; considera agendados/em_gozo/concluídos, exclui o próprio, recusa mais de três frações e ausência de alguma fração com pelo menos 14 dias, delegando a violacoes_de_fracionamento, também recusa datas invertidas e dias divergentes do intervalo (app/ferias/models.py:362, app/ferias/models.py:366, app/ferias/models.py:374, app/ferias/models.py:379, app/ferias/models.py:383) ; sim, texto exige que a MENOR fração tenha pelo menos 14 dias (app/templates/ferias/agendar.html:10), juiz exige que PELO MENOS UMA tenha 14 dias (app/ferias/models.py:370, app/ferias/models.py:374) ; leitor informativo publica regra própria da menor fração em app/templates/ferias/agendar.html:10, a implementação compartilhada violacoes_de_fracionamento é chamada pelo juiz e pelo aviso, sem cópia da fórmula (app/ferias/models.py:264, app/ferias/situacao.py:227) ; a menor fração precisa ter 14 dias (app/templates/ferias/agendar.html:10) ou basta ao menos uma com 14 dias (app/ferias/models.py:374)?

14) ferias/situacao.py::DIAS_AVISO_PREVIO ; o aviso de 30 dias foi dado (art.135)? (app/core/juizes.py:272) ; nenhum texto específico encontrado em LEIS.md/CORTES.md para o aviso [nao verificado] ; o juiz registrado é somente a constante 30, não um predicado de aviso efetivamente dado (app/ferias/situacao.py:56) ; nao ; escritores calculam aviso_emitido pela antecedência até o início, lendo a constante mas realizando a comparação fora dela (app/ferias/services.py:529, app/ferias/services.py:547, app/ferias/services.py:681, app/ferias/services.py:745), view repete a comparação (app/ferias/views.py:309), painel usa 30 literal para aviso pendente (app/ferias/views.py:35, app/ferias/views.py:86) ; -

15) ponto/catalogo/ausencias.py::fim_de ; quanto a ausencia dura, e o tipo e valido? (app/core/juizes.py:273) ; L-006 determina cadastro com leitor, mas não estipula a duração de cada tipo (app/docs/LEIS.md:64), nenhum corte específico encontrado [nao verificado] ; devolve a gramática do fim do catálogo vigente, desconhecido ou coluna ausente/vazia devolve por_lei, não valida o código nem calcula dias, codigos_validos é outra função (app/ponto/catalogo/ausencias.py:192, app/ponto/catalogo/ausencias.py:194, app/ponto/catalogo/ausencias.py:195, app/ponto/catalogo/ausencias.py:284) ; nao ; formulário tem catálogo próprio de mínimos/máximos/dias fixos e fallback máximo 30, fixa maternidade em 120, paternidade em 5 e luto em 2 e aplica esses limites na duração (app/ponto/forms.py:13, app/ponto/forms.py:18, app/ponto/forms.py:19, app/ponto/forms.py:20, app/ponto/forms.py:38, app/ponto/forms.py:91, app/ponto/forms.py:113), modelo também deriva dias_corridos do intervalo (app/ponto/models.py:899, app/ponto/models.py:901) ; -

16) ponto/services/alertas_ausencia.py::resumo_atestados ; ha 3 ou mais lancamentos medicos na competencia? (app/core/juizes.py:274) ; L-006 rege a origem cadastral do conjunto médico (app/docs/LEIS.md:64), corte PARAMETRO-GANHA-ROTULO registra TipoAusencia.medico consumido (app/docs/CORTES.md:221), nenhum desses textos estabelece a unidade do limiar três ; retorna competência da empresa, cobertura para folha filtrada por TIPOS_MEDICOS, número de registros distintos e número de dias, não aplica limiar três (app/ponto/services/alertas_ausencia.py:45, app/ponto/services/alertas_ausencia.py:48, app/ponto/services/alertas_ausencia.py:50, app/ponto/services/alertas_ausencia.py:51, app/ponto/services/alertas_ausencia.py:52) ; sim quanto ao contrato literal, texto diz conta DIAS e não REGISTROS (app/ponto/services/alertas_ausencia.py:17), código conta e publica registros também (app/ponto/services/alertas_ausencia.py:51, app/ponto/services/alertas_ausencia.py:56) ; leitores decidem o limiar registros >= 3 fora do juiz, usando a contagem oficial (app/ponto/views.py:2224, app/ponto/views.py:2447) ; -

Observações para a consolidação: o conflito 13 é inequívoco e possui duas candidatas expressas. No 16, a divergência literal da docstring com a contagem de registros não permite afirmar que a lei mandaria alertar com três DIAS, porque esse limiar por dias não está escrito nos trechos encontrados. As regras do formulário em 15 são achado autônomo de duração, mas não contradizem diretamente o retorno de fim_de, que responde apenas a gramática do fim.

## 14/11 08:43 · sessao 020eaa2b6e05 · - · - · origem=Ronald (tmux)

Todos os números abaixo derivam apenas do dado no chat, sem arquivo:linha: **[nao verificado]**.

Pares F completos comparados: mês 9 = **384**; mês 10 = **335**, até col892 inclusive; mês 11 = **0**. F depois10 col893 está truncado e foi excluído das somas. **[nao verificado]**

Lista exaustiva das novas faltas nos pares completos: **[nao verificado]**

| mês | colab | data | motivo N do mesmo mês/data | fora 21..20 |
|---:|---:|---|---|---|
| 9 | 29 | 2026-06-17 | sem N | sim |
| 9 | 44 | 2026-05-21 | sem N | sim |
| 9 | 46 | 2026-05-23 | sem N | sim |
| 9 | 46 | 2026-05-26 | sem N | sim |
| 9 | 81 | 2026-06-28 | sem N | sim |
| 9 | 134 | 2026-06-22 | sem N | sim |
| 9 | 134 | 2026-06-24 | sem N | sim |
| 9 | 371 | 2026-08-08 | sem N | sim |
| 9 | 707 | 2026-10-26 | sem N | sim |
| 9 | 709 | 2026-08-25 | ancora_papel_errado | não |
| 9 | 712 | 2026-09-07 | ancora_fora_de_turno | não |
| 9 | 993 | 2026-10-20 | sem N | sim |
| 9 | 1004 | 2026-10-27 | sem N | sim |
| 10 | 29 | 2026-06-17 | sem N | sim |
| 10 | 44 | 2026-05-21 | sem N | sim |
| 10 | 46 | 2026-05-23 | sem N | sim |
| 10 | 46 | 2026-05-26 | sem N | sim |
| 10 | 81 | 2026-06-28 | sem N | sim |
| 10 | 134 | 2026-06-22 | sem N | sim |
| 10 | 134 | 2026-06-24 | sem N | sim |
| 10 | 371 | 2026-08-08 | sem N | sim |
| 10 | 707 | 2026-10-26 | sem N | sim |
| 10 | 709 | 2026-08-25 | sem N | sim |
| 10 | 712 | 2026-09-07 | sem N | sim |

Contagens e somas somente dos pares completos: **[nao verificado]**

| mês | sem N | ancora_papel_errado | ancora_fora_de_turno | total acrescido | fora | Δdsr | Δdias_falta |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 9 | 11 | 1 | 1 | 13 | 11 | +11 | +13 |
| 10 | 11 | 0 | 0 | 11 | 11 | +9 | +11 |
| total observável | 22 | 1 | 1 | 24 | 22 | +20 | +24 |

Datas removidas dos pares completos: **0**, em ambos os meses observáveis; lista vazia e nenhum motivo a associar. **[nao verificado]**

Conferência dos deltas por colaborador alterado: mês9 col29 (+1 dia,+1 dsr), 44 (+1,+1), 46 (+2,+2), 81 (+1,+1), 134 (+2,+1), 371 (+1,0), 707 (+1,+1), 709 (+1,+1), 712 (+1,+1), 993 (+1,+1), 1004 (+1,+1). Mês10 repete as alterações até col712; col993 e1004 não têm depois fornecido. **[nao verificado]**

## 14/11 08:42 · sessao 2ef07e4ad96f · - · - · origem=Ronald (tmux)

Contagem de todas as linhas `N|9` do trecho fornecido: **357**, em **155 colaboradores com N**. [nao verificado]

| mês | motivo | dia_final | linhas N |
|---:|---|---|---:|
| 9 | coberto | hf | 150 |
| 9 | turno_fechado_na_ponta | nada | 155 |
| 9 | ancora_papel_errado | nada | 23 |
| 9 | ancora_em_turno_de_outro_dia | nada | 22 |
| 9 | sem_vinculo_ou_marcos | nada | 4 |
| 9 | ancora_fora_de_turno | nada | 2 |
| 9 | cruza_trabalhado | nada | 1 |

Fonte de todas as células: dado do chat; sem arquivo:linha disponível. [nao verificado]

Conferência por faixas de colaboradores, na ordem `coberto / turno_fechado_na_ponta / ancora_papel_errado / ancora_em_turno_de_outro_dia / sem_vinculo_ou_marcos / ancora_fora_de_turno / cruza_trabalhado`: [nao verificado]

```text
28–199:   8 / 19 / 6 / 5 / 1 / 0 / 0 = 39
200–399: 36 / 26 / 3 / 8 / 0 / 0 / 1 = 74
400–599: 18 / 39 / 2 / 2 / 2 / 0 / 0 = 63
600–799: 37 / 17 / 3 / 3 / 1 / 1 / 0 = 62
800–1004:51 / 54 / 9 / 4 / 0 / 1 / 0 = 119
TOTAL: 150 /155 /23 /22 / 4 / 2 / 1 = 357
```

Não há `dia_final=hi` nem `dia_final=<inteiro>` nas linhas `N|9` fornecidas: **0** de cada. [nao verificado]

## 14/11 08:42 · sessao d988e758e168 · - · - · origem=Ronald (tmux)

| mês | motivo | dia_final | linhas N |
|---:|---|---|---:|
| 10 | coberto | hf | 92 |
| 10 | turno_fechado_na_ponta | nada | 104 |
| 10 | ancora_papel_errado | nada | 12 |
| 10 | ancora_em_turno_de_outro_dia | nada | 14 |
| 10 | ancora_fora_de_turno | nada | 3 |
| 10 | cruza_trabalhado | nada | 5 |
| 10 | sem_vinculo_ou_marcos | nada | 1 |
| **10** | **Total** | | **231** |

Toda a tabela deriva exclusivamente do dado colado no chat. [nao verificado]

Conferência por intervalos de colaborador: até 299 = 47; 300–499 = 53; 500–699 = 46; 700–893 = 85; soma = **231**, distribuídas por **117 colaboradores**. [nao verificado]

As 12 ocorrências de `ancora_papel_errado` são: col152 (1), col278 (1), col360 (1), col384 (1), col518 (1), col583 (1), col616 (3), col788 (1), col820 (2). [nao verificado]

As 14 ocorrências de `ancora_em_turno_de_outro_dia` são: col255 (1), col297 (3), col414 (1), col478 (1), col616 (2), col747 (1), col868 (5). [nao verificado]

As três ocorrências de `ancora_fora_de_turno` são col334, col358 e col841; as cinco de `cruza_trabalhado` são col451, col478, col655, col841 e col849; `sem_vinculo_ou_marcos` ocorre em col890. [nao verificado]

A contagem cobre somente o trecho recebido, encerrado na linha incompleta `F|depois|10|893|...2026-08-22`; não há linhas N do mês 11 no trecho recebido. [nao verificado]

## 14/11 08:37 · sessao 70c7a6690e01 · COERENCIA-DAS-LEIS · censo · origem=Code

app/chamados/catalogo/motor.py::VIVOS ; o chamado esta vivo? (app/core/juizes.py:107) ; corte REGISTRADO-NAO-E-VIVO-DESVIO mantém registrado em VIVOS e distingue vida de pendência (app/docs/CORTES.md:328) ; vivo significa aberto, em_analise ou registrado (app/chamados/catalogo/motor.py:24) ; nao ; nenhum achado de definição independente completa, procurados VIVOS, status_local, conjuntos aberto/em_analise/registrado e exclusões de terminais, exaustividade semântica [nao verificado] ; -

app/chamados/catalogo/motor.py::decidir ; pode transitar de um estado a outro? (app/core/juizes.py:108) ; nenhum texto especificando a matriz integral [nao verificado] ; nascimento aceita EMITIR para aberto e REGISTRAR para registrado, transição humana consulta a matriz com exceção admin_ti, cancelado e superado permanecem terminais absolutos, sistema consulta seu mapa e recusa evento ou origem sem destino (app/chamados/catalogo/motor.py:130, app/chamados/catalogo/motor.py:139, app/chamados/catalogo/motor.py:158) ; nao ; nenhum achado de matriz independente, procurados TRANSICOES_PERMITIDAS, TRANSICAO_HUMANA, decidir e atribuições de status, a matriz do modelo deriva da canônica e pode_transicionar delega ao juiz (app/chamados/models.py:117, app/chamados/models.py:123), exaustividade [nao verificado] ; -

app/chamados/catalogo/premissa.py::premissa_morta ; a premissa do chamado morreu, pode fechar? (app/core/juizes.py:109) ; L-125 determina lastro do próprio dia para cluster, sem morte por idade (app/docs/LEIS.md:146), corte O169-DECIDIDO-PELA-LEI reafirma isso (app/docs/CORTES.md:430), corte RESPOSTAS-2910-MANHA nomeia a célula como juiz da premissa do chamado de turno (app/docs/CORTES.md:357) ; retorna verdadeiro somente para classe catalogada cujo fato sustentador foi coletado e é falso, classe desconhecida ou fato não coletado conservam a premissa viva (app/chamados/catalogo/premissa.py:124, app/chamados/catalogo/premissa.py:128, app/chamados/catalogo/premissa.py:134, app/chamados/catalogo/premissa.py:149) ; nao ; _absolvido_pela_celula decide a absolvição do chamado de turno diretamente pelo veredito da célula e ausência de marcos pendentes, sem premissa_morta (app/chamados/reconciliador.py:1019, app/chamados/reconciliador.py:1028, app/chamados/reconciliador.py:1030), autoridade desse subuniverso expressamente nomeada pelo corte (app/docs/CORTES.md:357) ; -

app/chamados/utils.py::pode_agir_chamado ; quem pode agir no chamado? (app/core/juizes.py:110) ; nenhum texto substantivo em LEIS.md ou CORTES.md [nao verificado], JUIZES.md nomeia a autoridade contextual (app/docs/JUIZES.md:49) ; exige usuário autenticado e decide pelo departamento dono, permite gestor geral, departamento desconhecido ou usuário sem competências declaradas, nos demais casos exige competência ou perfil correspondente (app/chamados/utils.py:32, app/chamados/utils.py:55, app/chamados/utils.py:57, app/chamados/utils.py:60, app/chamados/utils.py:65, app/chamados/utils.py:72) ; nao ; nenhum achado independente, procurados pode_agir_chamado, departamento_do_chamado, pode_agir_departamento, DEPARTAMENTO_DO_DONO e competencias_do_user, recusa_de_area reutiliza os mesmos auxiliares (app/chamados/utils.py:78), exaustividade [nao verificado] ; -

app/chamados/catalogo/modulos.py::data_do_chamado ; qual o dia do chamado? (app/core/juizes.py:111) ; corte K1-DIA-DO-CHAMADO manda declarar chave_data=data_turno no catálogo (app/docs/CORTES.md:177) ; consulta MOTIVOS ou MOTIVOS_APOSENTADOS e devolve o valor da chave_data declarada, módulo desconhecido ou sem chave devolve None (app/chamados/catalogo/modulos.py:222, app/chamados/catalogo/modulos.py:225) ; nao ; dias_do_fato recompõe o dia por célula e data_turno cru, sem consultar a chave catalogada nesse degrau (app/chamados/juizes.py:184, app/chamados/juizes.py:187), a extensão avulsa dia_do_fato consulta o catálogo antes do fallback (app/chamados/juizes.py:113, app/chamados/juizes.py:118) ; -

app/chamados/juizes.py::pergunta_viva ; a pergunta da disputa esta viva? (app/core/juizes.py:112) ; L-051 preserva o passivo da tranca como história reversível (app/docs/LEIS.md:92), C7 determina leitura da tranca com exceção de toque humano posterior (app/docs/CORTES.md:272), LEI-TRANCA-E2-JUIZ exige a mesma forma de lote (app/docs/CORTES.md:427), corte posterior manda absolver quando a batida responde ao marco (app/docs/CORTES.md:413) ; validada ou trancada morre, vias que vencem a resposta precedem seu julgamento, resposta com texto sem decisão do admin continua viva salvo concordância com a luz, depois consulta identidade, fato do marco, célula, falta e testemunha da materialização (app/chamados/juizes.py:1401, app/chamados/juizes.py:1405, app/chamados/juizes.py:1412, app/chamados/juizes.py:1417, app/chamados/juizes.py:1437, app/chamados/juizes.py:1444, app/chamados/juizes.py:1462, app/chamados/juizes.py:1464, app/chamados/juizes.py:1476) ; nao ; nenhum achado de decisão individual exata independente, procurados pergunta_viva, q_vivas, validada_em, materializada_em e respondida_em, q_vivas declara ser pré-filtro e exigir o juiz para a resposta exata (app/chamados/models.py:3392), exaustividade [nao verificado] ; -

app/chamados/models.py::DisputaSupervisao.pode_fechar ; a disputa pode fechar? (app/core/juizes.py:113) ; C7 exige nenhuma pergunta viva e nenhuma resposta sem veredito (app/docs/CORTES.md:272), corte posterior determina encerramento integral no ato do desligamento (app/docs/CORTES.md:451) ; no fechamento SEM porta de exceção, exige respostas_sem_veredito vazia e fio_vivo falso (app/chamados/models.py:1905, app/chamados/models.py:1909) ; nao ; DisputaSupervisao.fechar recompõe a decisão normal sem chamar pode_fechar (app/chamados/models.py:1942, app/chamados/models.py:1951), essa porta também recusa resposta antes da exceção desligamento e o executor pula a disputa respondida (app/chamados/models.py:1942, app/colaboradores/services/desligamento.py:165), comportamento externo à função registrada contrário ao ato exigido pelo corte posterior (app/docs/CORTES.md:451) ; -

app/chamados/models.py::ChamadoColaborador.abrir ; o chamado e duplicado, mesmo fato? (app/core/juizes.py:114) ; corte LEI-DO-MIOLO-CEDE-A-LAMPADA exige dedup pela chave do fato no intervalo, sem definir chave integral para todos os chamados (app/docs/CORTES.md:273), autoridade indicada em JUIZES.md (app/docs/JUIZES.md:62) ; sob trava do colaborador busca pelo lookup recebido, devolve existente e tenta renascer se encerrado, filho agrupado sob pai vivo também satisfaz dedup, nascimento restante usa get_or_create com o mesmo lookup e pode ser vetado pela célula ou régua (app/chamados/models.py:949, app/chamados/models.py:953, app/chamados/models.py:968, app/chamados/models.py:974, app/chamados/models.py:1009, app/chamados/models.py:1014, app/chamados/models.py:1018) ; nao ; abertura deduplica por colaborador, módulo, estado vivo, assunto, descrição, prazo e autor antes de criar (app/chamados/services/abertura.py:165, app/chamados/services/abertura.py:172), processar_alertas_avancados consulta o mesmo fato de turno por conta própria antes de criar (app/ponto/management/commands/processar_alertas_avancados.py:100, app/ponto/management/commands/processar_alertas_avancados.py:121) ; -

app/chamados/catalogo/motor.py::prazo_estourou ; o prazo, SLA, do chamado estourou? (app/core/juizes.py:115) ; nenhum texto específico para a comparação de vencimento [nao verificado] ; verdadeiro quando prazo_sla existe e é menor ou igual a agora, sem prazo retorna falso (app/chamados/catalogo/motor.py:56) ; nao ; sla.vencido decide independentemente, exige status em COBRANCA e compara estritamente vence_em menor que agora (app/chamados/services/sla.py:37, app/chamados/services/sla.py:39), deriva vencimento da última resposta do DP ou nascimento mais SLA da urgência (app/chamados/services/sla.py:20, app/chamados/services/sla.py:23, app/chamados/services/sla.py:30) ; -

app/escala/services/regeneracao.py::cobranca_orfa ; a cobranca ficou orfa depois da regeneracao? (app/core/juizes.py:116) ; nenhum texto específico em LEIS.md ou CORTES.md [nao verificado], regra detalhada de consulta em JUIZES.md (app/docs/JUIZES.md:41) ; exige marcos_faltantes e dia do fato, retorna instante de reescrita posterior à criação ou None, usa o maior carimbo entre regeneração e edição, somente a presença de regenerada_em dispensa a janela histórica (app/escala/services/regeneracao.py:84, app/escala/services/regeneracao.py:120, app/escala/services/regeneracao.py:122, app/escala/services/regeneracao.py:125, app/escala/services/regeneracao.py:149, app/escala/services/regeneracao.py:154) ; nao ; nenhum achado independente, procurados cobranca_orfa, dia_reescrito_apos e comparações de regenerada_em ou editada_em com criado_em, consumidores conferidos delegam (app/ponto/management/commands/gerar_celulas.py:257, app/ponto/management/commands/supra_juiz.py:275), exaustividade [nao verificado] ; -

app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia ; a ausencia do dia suspende, fecha ou levanta a cobranca viva? (app/core/juizes.py:117) ; L-061 exige leitura da lâmpada (app/docs/LEIS.md:95), nenhum corte específico adicional localizado [nao verificado] ; atua somente no universo vivo com dia identificado, ausência aberta silencia ou renova sem sobrescrever silêncio vigente alheio, aprovada fecha se a premissa morreu e a célula não acusa, ressalvada justificativa, caso contrário levanta seu marcador, ausência ausente também levanta seu marcador (app/chamados/services/silencio_ausencia.py:35, app/chamados/services/silencio_ausencia.py:42, app/chamados/services/silencio_ausencia.py:61, app/chamados/services/silencio_ausencia.py:78, app/chamados/services/silencio_ausencia.py:81, app/chamados/services/silencio_ausencia.py:94, app/chamados/services/silencio_ausencia.py:113) ; nao ; materializacao decide resolver o destino quando a ausência não pertence a ABERTAS sem consultar este juiz (app/chamados/services/materializacao.py:587), reconciliar_grade inclui dias cobertos no conjunto de fatos mortos e decide cobertura pela ausência não rejeitada (app/ponto/management/commands/reconciliar_grade.py:266, app/ponto/management/commands/reconciliar_grade.py:400) ; -

app/chamados/services/disputa_emissao.py::motivo_fora_da_fabrica ; a fabrica de perguntas alcanca o chamado, e se nao por que? (app/core/juizes.py:119) ; L-061 determina leitura da lâmpada (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL manda consultar a célula antes de excluir por pergunta encerrada (app/docs/CORTES.md:253) ; exclui chamado de disputa, encerrado e célula nunca_bateu_sem_adesao, aceita cobrança passada com marcos nomeados, pergunta materializada exclui somente com célula ausente ou sem veredito acusador, depois exclui turno fechado na virada e categoria fora dos mapas, demais retornam None (app/chamados/services/disputa_emissao.py:753, app/chamados/services/disputa_emissao.py:755, app/chamados/services/disputa_emissao.py:758, app/chamados/services/disputa_emissao.py:760, app/chamados/services/disputa_emissao.py:772, app/chamados/services/disputa_emissao.py:783, app/chamados/services/disputa_emissao.py:785) ; nao ; nenhum achado independente, procurados motivo_fora_da_fabrica, chamado_orfao_e_regularizavel e nomes das causas, consumidores conferidos delegam (app/chamados/services/disputa_emissao.py:796, app/chamados/services/pergunta_no_app.py:133, app/chamados/services/acoes_disputa.py:219), exaustividade [nao verificado] ; -

app/chamados/services/pergunta_no_app.py::retencao ; por que o chamado vivo de furo nao tem pergunta no app? (app/core/juizes.py:120) ; L-061 exige leitura da lâmpada (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL exige fio ou ação administrativa nomeada para a pendência (app/docs/CORTES.md:253) ; sem dia retorna sem_dia, hoje ou futuro e chamado já no app retornam None, depois prioriza causa da fábrica, ata ausente, parcial na lista do admin, cobertura de ausência, pergunta viva do mesmo dia em outro chamado e marco já perguntado, restante retorna fabrica_nao_rodou (app/chamados/services/pergunta_no_app.py:127, app/chamados/services/pergunta_no_app.py:129, app/chamados/services/pergunta_no_app.py:131, app/chamados/services/pergunta_no_app.py:133, app/chamados/services/pergunta_no_app.py:137, app/chamados/services/pergunta_no_app.py:144, app/chamados/services/pergunta_no_app.py:146, app/chamados/services/pergunta_no_app.py:149, app/chamados/services/pergunta_no_app.py:151, app/chamados/services/pergunta_no_app.py:153) ; nao ; nenhum achado independente, procurados retencao e nomes das causas, leitores conferidos usam o resultado do juiz ou seu censo (app/chamados/services/pergunta_no_app.py:202, app/chamados/services/pergunta_no_app.py:223, app/chamados/services/cobrar_dia.py:46), exaustividade [nao verificado] ; -

app/chamados/juizes.py::competencia_do_chamado ; qual a competencia do chamado? (app/core/juizes.py:122) ; L-026 e L-090 determinam janela pelo corte da empresa (app/docs/LEIS.md:77, app/docs/LEIS.md:123), K8-COMPETENCIA-NAO-E-MES-CIVIL exige competência da data do fato (app/docs/CORTES.md:216), texto específico para escolher o fato mais recente [nao verificado] ; reúne dia_do_fato, dia da ausência e dias das perguntas vivas, sem fatos retorna None, escolhe o fato mais recente e retorna dia, mês e ano do fim da janela da empresa e fonte (app/chamados/juizes.py:2360, app/chamados/juizes.py:2363, app/chamados/juizes.py:2366, app/chamados/juizes.py:2368, app/chamados/juizes.py:2370, app/chamados/juizes.py:2373) ; nao ; cobranca_folha.muda_a_folha calcula a competência independentemente usando apenas dia_do_chamado e janela_atual, sem agregar os demais fatos do juiz (app/chamados/services/cobranca_folha.py:54, app/chamados/services/cobranca_folha.py:63), seu dia vem da chave catalogada (app/chamados/services/cobranca_folha.py:32) ; -

app/chamados/juizes.py::fio_vivo ; alguma pergunta desta colecao esta viva? (app/core/juizes.py:125) ; FIO-MORTO-E-ESTRUTURAL declara fio_vivo como autoridade da redução da coleção e preserva pergunta_viva como juiz individual (app/docs/CORTES.md:253), L-002 exige leitura da mesma autoridade (app/docs/LEIS.md:60) ; retorna verdadeiro se alguma pergunta da coleção satisfaz pergunta_viva, coleção vazia retorna falso (app/chamados/juizes.py:1513) ; nao ; relatorios_disputa.classificar reduz a coleção por conta própria, monta a lista de perguntas vivas e testa seu esvaziamento para declarar PRONTA_FECHAR (app/chamados/relatorios_disputa.py:18, app/chamados/relatorios_disputa.py:19, app/chamados/relatorios_disputa.py:20) ; -

app/ponto/services/furos_vetados.py::motivo_do_veto ; este dia veta o nascimento ou reabertura automatica de chamado de turno? (app/core/juizes.py:131) ; corte VETO-EC-NONE e JUIZ-MOTIVO-DO-VETO determina autoridade única e veto com ec=None, fundamentado em L-093 (app/docs/CORTES.md:403, app/docs/LEIS.md:111) ; retorna VETO_DEGENERADA se o template do vínculo, ou None sem vínculo, for degenerado, depois VETO_REGUA se a régua for suspeita, demais retornam None (app/ponto/services/furos_vetados.py:40, app/ponto/services/furos_vetados.py:42, app/ponto/services/furos_vetados.py:44) ; nao ; detectar_ausencias decide os dois vetos diretamente pelas funções componentes (app/ponto/management/commands/detectar_ausencias.py:601, app/ponto/management/commands/detectar_ausencias.py:605), reconciliar_grade recompõe a mesma decisão (app/ponto/management/commands/reconciliar_grade.py:495, app/ponto/management/commands/reconciliar_grade.py:499) ; -

app/chamados/juizes.py::porta_alcancavel_do_dia ; por onde alguem chega a este dia, porta alcancavel com o verbo do painel? (app/core/juizes.py:293) ; corte LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN define chamado da célula na fila ou pergunta viva no app com âncora na fila (app/docs/CORTES.md:404), corte O261-E-F5-DESENHO determina saída do fio pelo juiz (app/docs/CORTES.md:409) ; reúne portas da célula na fila e âncoras de perguntas vivas no app, exclui respondida sem texto, devolve rota, fila visível, verbos e fio, sem porta retorna None (app/chamados/juizes.py:1654, app/chamados/juizes.py:1657, app/chamados/juizes.py:1661, app/chamados/juizes.py:1663, app/chamados/juizes.py:1667) ; nao ; nenhum achado completo independente, procurados porta_alcancavel_do_dia, alimentar_portas, porta_fila, porta_pergunta, qs_na_fila e FIO_DO_VERBO, consumidores conferidos delegam (app/folha/fora_da_folha.py:67, app/chamados/services/vigia_fio.py:640), exaustividade [nao verificado] ; -

app/chamados/juizes.py::dia_sem_trabalho ; a cobranca do dia cala porque a celula lavrada diz que o dia nao e de trabalho? (app/core/juizes.py:294) ; L-061 (app/docs/LEIS.md:95), corte JUIZ-DIA-SEM-TRABALHO (app/docs/CORTES.md:416), exceção posterior da ausência parcial documentada em JUIZES.md (app/docs/JUIZES.md:67) ; ausência parcial cobra o restante e só trabalha=False cala, demais calam com trabalha=False ou ata.tipo_dia preenchido e diferente de trabalho, trabalha=None sozinho não cala (app/chamados/juizes.py:446, app/chamados/juizes.py:454, app/chamados/juizes.py:456) ; nao ; declaracao_texto._marcos_faltantes dispensa por tipo_dia diferente de trabalho salvo parcial sem consultar dia_sem_trabalho (app/chamados/services/declaracao_texto.py:119), cartorio._protesta deriva a primeira cláusula com marcos apagados (app/ponto/services/cartorio.py:1459), este último expressamente declarado pelo corte (app/docs/CORTES.md:416) ; -

app/chamados/services/tarefa_do_dia.py::tarefa_do_dia ; quais acoes ainda sao exigiveis para cada instancia de marco do dia, de quem e por quais portas? (app/core/juizes.py:295) ; corte FIO-E0-V4 define o juiz por instância, decisão, efeito, dono e porta (app/docs/CORTES.md:426), LEI-TRANCA-E2-JUIZ determina leitura da tranca pelo C20 (app/docs/CORTES.md:427) ; exige agora e carga do mesmo instante, dispensa fora do vínculo, isento e trancado, cruza lâmpada e teto por instância com decisão e efeito, retorna ações de perguntar, responder, validar, materializar, rejulgar, decidir ausência ou entregar documento, explicita nao_sei e publica portas das ações exigíveis (app/chamados/services/tarefa_do_dia.py:566, app/chamados/services/tarefa_do_dia.py:185, app/chamados/services/tarefa_do_dia.py:195, app/chamados/services/tarefa_do_dia.py:254, app/chamados/services/tarefa_do_dia.py:281, app/chamados/services/tarefa_do_dia.py:300, app/chamados/services/tarefa_do_dia.py:116, app/chamados/services/tarefa_do_dia.py:538) ; nao ; nenhum achado completo da mesma pergunta por instância, decisão e efeito, procurados tarefa_do_dia, carga_da_tarefa, cobranca_viva_do_dia, motivos_a_perguntar e dono_fora_do_fio, persistem respostas parciais sobre emissão e dono do dia (app/chamados/services/fabrica_da_celula.py:112, app/chamados/services/vigia_fio.py:306), continuidade de C17 declarada no módulo (app/chamados/services/tarefa_do_dia.py:28), exaustividade [nao verificado] ; -

app/chamados/juizes.py::dia_da_pergunta_trancado ; o dia da pergunta esta arquivado pela tranca da competencia, trancada e nao tocada pelo admin depois? (app/core/juizes.py:296) ; L-051 e L-090 (app/docs/LEIS.md:92, app/docs/LEIS.md:123), C7 determina exceção do toque humano posterior (app/docs/CORTES.md:272), LEI-TRANCA-E2-JUIZ declara autoridade e forma de lote (app/docs/CORTES.md:427) ; consulta dias_trancados com o dia do turno, identifica competência pela janela e periodo_trancado, retorna verdadeiro quando existe tranca sem toque humano posterior, conversa não tem dia de fato a trancar (app/chamados/juizes.py:989, app/chamados/juizes.py:991, app/chamados/juizes.py:1062, app/chamados/juizes.py:1066, app/chamados/juizes.py:1079), toque inclui cobrar, resolver com assinatura do ato e desarquivar (app/chamados/juizes.py:1245) ; nao ; nenhum achado independente, procurados dias_trancados, dia_da_pergunta_trancado, _admin_tocou, fechado_em e veredito_celula, competencia_trancada.me

[truncado em 20000 de 23727 caracteres]

## 14/11 08:35 · sessao 5639edb566f4 · CORTE-DAS-TRAVAS · verificador · origem=Code

**[bloqueia] O “andável” do Stop continua incluindo trabalho estacionado.** `_itens_andaveis` percorre todo o bloco e consulta `_item_parado`, cujo filtro não reconhece `ESTACIONADA` e tampouco lê a indicação POS-20 na descrição (`bin/hook_stop_fila1.py:95`, `bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:244`). Contra-exemplos da própria cópia, confirmados em memória: entram `CONTENCAO-QUE-MORDE-V2`, estacionada, e `TRIAGEM-FILA-OPUS-V6`, POS-20 (`app/docs/BACKLOG.md:889`, `app/docs/BACKLOG.md:1842`). Essa lista impede o PAREI (`bin/hook_stop_fila1.py:1153`). O selo só contrapõe item livre a “espera o !”; não cobre esses casos (`bin/tests/test_hook_parei_com_andavel.sh:40`, `bin/tests/test_hook_parei_com_andavel.sh:55`). Declaração INCOMPLETO nos contratos não fornecidos: **[nao verificado]**.

**[bloqueia] O novo portão pode exigir uma triagem impossível.** O vigia cobra toda ocorrência de verificador com `fim rc=0`, mas o wrapper só produz sidecar quando também existe resposta nova (`bin/disjuntor.py:1039`, `bin/codex_pergunta.sh:463`). Contra-exemplo: Codex retorna zero sem produzir `$NOVA`; o wrapper conserva zero, registra `fim`, não produz sidecar e a triagem recusa a volta (`bin/codex_pergunta.sh:425`, `bin/codex_pergunta.sh:472`, `bin/codex_pergunta.sh:272`, `bin/disjuntor.py:923`). Falhar ao gravar o sidecar também só imprime aviso, conservando zero (`bin/codex_pergunta.sh:466`). `_nega_b3` cobra o histórico inteiro; rearme só arquiva voltas já triadas, sem dispensar essa pendência (`bin/disjuntor.py:1935`, `bin/disjuntor.py:1240`). Portanto, pode negar indefinidamente pela porta normal; ocorrência disso em produção: **[nao verificado]**.

**[bloqueia] Há duas decisões diferentes para admitir a próxima volta.** O PreToolUse consulta B3 **e** pendências; a porta própria do wrapper, `--b3-parada`, consulta somente B3 (`bin/disjuntor.py:1989`, `bin/disjuntor.py:1935`, `bin/codex_pergunta.sh:390`, `bin/disjuntor.py:1265`). Contra-exemplo: B3 livre e uma volta sem triagem; pelo hook nega, executando o wrapper fora dele passa nessa verificação (`bin/disjuntor.py:1940`, `bin/disjuntor.py:1272`). É uma derivação paralela da permissão, embora ambas reutilizem o juiz B3 (`bin/disjuntor.py:1927`, `bin/disjuntor.py:1265`).

**[bloqueia] Fonte inválida pode parecer histórico vazio.** Um arquivo regular cuja abertura falha produz “não sei”; porém `chamadas.log` sendo diretório passa pelo retorno vazio de `not os.path.isfile` (`bin/disjuntor.py:1021`, `bin/codex_limite_estado.py:60`). Contra-exemplo: triagem vazia, B3 livre e um diretório chamado `logs/codex/chamadas.log`: nenhuma ocorrência chega ao vigia, que libera (`bin/codex_limite_estado.py:61`, `bin/disjuntor.py:1048`, `bin/disjuntor.py:1943`). Para `triagem.log`, falha de leitura, truncamento e encolhimento são tratados restritivamente (`bin/disjuntor.py:960`, `bin/disjuntor.py:969`).

**[fila] A catraca ainda reprova além do total.** Total 9→9 passa em `_catraca`, mas parcela A1 6→7 ou mudança de composição reprova (`app/core/tests/test_medida_turno_do_par.py:512`, `app/core/tests/test_medida_turno_do_par.py:524`, `app/core/tests/test_medida_turno_do_par.py:546`). Os próprios selos exigem essa rigidez; não demonstram o corte “só pelo total” (`app/core/tests/test_medida_turno_do_par.py:638`, `app/core/tests/test_medida_turno_do_par.py:655`).

**[fila] `--desde` continua permitindo zero calado.** A validação aceita campos sem zero à esquerda, mas o filtro conserva comparação textual (`bin/disjuntor.py:1061`, `bin/disjuntor.py:1046`). Contra-exemplo reproduzido em memória com as funções reais e leitores simulados: chamada de `2026-11-12 10:00:00`; corte `2026-11-02 00:00:00` dá uma pendência, enquanto `2026-11-2 00:00:00` dá zero, rc 0. O selo novo cobre somente `zzz`, que agora recebe rc 2 (`bin/tests/test_triagem_pendente.sh:150`).

O fechamento de cada item do opus fica assim:

| Item | Situação na cópia e prova |
|---|---|
| **1 — andável** | **Não curado**, pelos contra-exemplos acima; selo incompleto (`bin/hook_stop_fila1.py:244`, `bin/tests/test_hook_parei_com_andavel.sh:40`). |
| **2 — religar triagem** | **Chamador presente**, para Agent final e Bash Codex verificador, mas com os bloqueios novos descritos acima (`bin/disjuntor.py:1935`, `bin/disjuntor.py:1989`, `bin/disjuntor.py:2007`). Os casos exercitam ambas as entradas do hook (`bin/tests/test_triagem_pendente.sh:160`, `bin/tests/test_triagem_pendente.sh:164`). Escrita efetivamente retomada em produção: **[nao verificado]**. |
| **3 e 4 — classificador/templates** | **Retirados do escopo pelo aval fornecido**, não curados por melhoria do classificador (`libera_b3_corte_das_travas.txt:1`, anexo). Os dois arquivos estão ausentes da cópia; ausência sem linha positiva: **[nao verificado]**. |
| **5 — portão de pouso da classe** | **Retirado junto com o item 2**, sem obrigação de ressuscitar aquele portão (`libera_b3_corte_das_travas.txt:1`, anexo). A inteira continua documentada para pre-opus e pouso (`bin/suite.sh:23`). |
| **6 — catraca** | **Não curado**; parcelas e composição continuam reprovando, inclusive por exigência dos selos (`app/core/tests/test_medida_turno_do_par.py:524`, `app/core/tests/test_medida_turno_do_par.py:546`, `app/core/tests/test_medida_turno_do_par.py:643`, `app/core/tests/test_medida_turno_do_par.py:661`). |
| **7 — PRE-VOO mesclado** | Execução na árvore indicada: **[nao verificado]**. A régua executa selos de host antes da suíte, mas `suite.sh` chega diretamente ao executor Django, sem esse preâmbulo (`bin/regua.sh:138`, `bin/suite.sh:194`, `bin/suite.sh:226`). |
| **8 — `--desde`** | **Parcialmente curado**: `zzz` é recusado; data não canônica ainda esvazia o resultado (`bin/disjuntor.py:1061`, `bin/disjuntor.py:1046`, `bin/tests/test_triagem_pendente.sh:150`). |
| **9 — liberação B3 por autoria/conduta** | **Permanece**: DESENHO e ERRO-BOBO são reconhecidos pelo conteúdo; a cota incide somente sobre AUTOLIBERA (`bin/disjuntor.py:761`, `bin/disjuntor.py:772`, `bin/disjuntor.py:1150`). Contra-exemplo: linha ERRO-BOBO elegível libera sem consumir cota auto (`bin/disjuntor.py:1157`, `bin/disjuntor.py:1161`). |

Das curas v3 citadas no opus, as duas do classificador saíram com o instrumento; as duas do vigia continuam presentes: relógio recuado retorna “não sei”, e uma triagem quita somente uma ocorrência (`bin/disjuntor.py:1034`, `bin/disjuntor.py:1045`). Seus casos continuam cobrando esses comportamentos (`bin/tests/test_triagem_pendente.sh:114`, `bin/tests/test_triagem_pendente.sh:117`).

ESPERANDO continua exigindo PID, com casos para arquivo andando sem PID, PID vivo e PID morto (`bin/hook_stop_fila1.py:748`, `bin/tests/test_hook_esperando_com_prova.sh:106`, `bin/tests/test_hook_esperando_com_prova.sh:115`). O timeout continua usando os tetos derivados da medida p95, permitindo apenas rebaixamento; o selo verifica teto e processo efetivamente envolvido (`bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:98`, `bin/tests/test_codex_pergunta.sh:461`, `bin/tests/test_codex_pergunta.sh:468`).

**[fila] Restaram referências documentais ao gate.** Há menções em `app/docs/BACKLOG.md:1038`, `app/docs/BACKLOG.md:2474`, `app/docs/PROMPTS.md:2243`, `app/docs/PROMPTS.md:2252`, `app/docs/RELATO.md:4`, `app/docs/RELATO.md:55` e `app/docs/RELATO.md:135`. O RELATO ainda conserva a aplicação condicionada ao selo e o escopo antigo dos itens 2+5 (`app/docs/RELATO.md:5`, `app/docs/RELATO.md:29`). A busca nos diretórios pedidos não encontrou chamador ou referência literal aos arquivos removidos, `--portao-pouso`, `--pouso-inteira` ou `INTEIRA-FINAL`; por ser resultado negativo sem linha positiva: **[nao verificado]**.

Os escritores e leitores encontrados são:

| Fonte | Escritor | Leitor físico e consumidores |
|---|---|---|
| `triagem.log` | `_triagem_sob_trava`, sob `_com_estado` (`bin/disjuntor.py:938`, `bin/disjuntor.py:944`). | `_triagens` abre a fonte; consomem escritor, vigia, decisão B3 e rearme (`bin/disjuntor.py:960`, `bin/disjuntor.py:931`, `bin/disjuntor.py:1006`, `bin/disjuntor.py:1097`, `bin/disjuntor.py:1230`). |
| `chamadas.log` | Wrapper: `ini` e `fim` pelo trap (`bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:272`). | `_linhas_chamadas`; consomem contagem, últimas chamadas, procedência, tokens, vigia e censo (`bin/codex_limite_estado.py:62`, `bin/codex_limite_estado.py:91`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:109`, `bin/codex_limite_estado.py:204`, `bin/disjuntor.py:1022`, `bin/disjuntor.py:2286`). |

Não apareceu segundo escritor produtivo dessas fontes no código examinado **[nao verificado]**. Os novos casos escrevem fixtures próprias, inclusive uma triagem diretamente fabricada; portanto não comprovam que uma pendência real possui resposta e sidecar triáveis (`bin/tests/test_triagem_pendente.sh:155`, `bin/tests/test_triagem_pendente.sh:167`).

A ordem das negativas está diferenciada: primeiro PARADA, depois NAO SEI do juiz; somente B3 LIVRE chega ao vigia (`bin/disjuntor.py:1927`). Triagem ilegível produz primeiro **“trilha de triagem Codex ilegivel…”**, agora exigido pelo selo do disjuntor; chamadas malformadas com juiz livre produzem **“vigia da triagem nao leu…”**, exigido pelo outro selo (`bin/disjuntor.py:1099`, `bin/tests/test_disjuntor.sh:1675`, `bin/disjuntor.py:1937`, `bin/tests/test_triagem_pendente.sh:173`).

| Mutação | Mordida dos casos novos |
|---|---|
| Retirar o bloco do vigia de `_nega_b3` | Perde a negativa por pendência exigida (`bin/tests/test_triagem_pendente.sh:161`). |
| Liberar quando `pend is None` | Perde `deny` e o motivo do vigia (`bin/tests/test_triagem_pendente.sh:173`). |
| Filtrar exclusivamente outra obra, ou inverter `== obra` | Perde a negativa da própria OBRA-G (`bin/tests/test_triagem_pendente.sh:156`, `bin/tests/test_triagem_pendente.sh:161`). |
| Aceitar `--desde zzz` | Perde rc 2 (`bin/tests/test_triagem_pendente.sh:150`). |
| **Retirar o filtro: `da_obra = pend`** | **Sobrevive aos casos novos**: só existe G, depois nenhuma pendência, depois fonte ruim. Falta H pendente enquanto G deve passar (`bin/disjuntor.py:1938`, `bin/tests/test_triagem_pendente.sh:155`). |

**[fila] O controle após triar também é fraco:** exige apenas desaparecer “sem triagem”; uma negativa diferente passa como sucesso (`bin/tests/test_triagem_pendente.sh:168`). Sobrevivência dos mutantes na suíte completa: **[nao verificado]**.

**[fila] Relógio e fuso ainda precisam de distinção explícita.** `ler(raiz, agora)` recebe o instante da borda, mas chama `CL.ler_chamadas(raiz)` sem repassá-lo; esse leitor consulta outro `datetime.now()` (`bin/disjuntor.py:2131`, `bin/disjuntor.py:2196`, `bin/codex_limite_estado.py:88`). Contra-exemplo: uma leitura histórica usa o dia de `agora` para tokens e o relógio atual para chamadas (`bin/disjuntor.py:2183`, `bin/disjuntor.py:2188`). Wrapper e `--desde` usam hora sem offset; CODEX-ESTADO converte explicitamente para UTC−3 (`bin/codex_pergunta.sh:272`, `bin/disjuntor.py:1061`, `bin/gerar_codex_estado.py:46`, `bin/gerar_codex_estado.py:72`). Uniformidade de TZ em produção e fuso das horas do dado raspado: **[nao verificado]**.

A triagem toma a trava da resposta antes de entrar na transação do estado; o wrapper também trava a resposta antes de consultar B3 (`bin/disjuntor.py:905`, `bin/disjuntor.py:944`, `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:390`). Ciclo novo de travas ou novo juiz de negócio duplicando o registro: **[nao verificado]**.

Identidade Git da cópia, ancestralidade, execução dos selos na árvore mesclada e pouso após todos os pré-requisitos: **[nao verificado]**. Os selos completos não foram executados nesta leitura: criam e removem arquivos (`bin/tests/test_triagem_pendente.sh:155`, `bin/tests/test_triagem_pendente.sh:174`).

**REPROVA** — pelos bloqueios demonstrados no Stop e na admissão da triagem (`bin/hook_stop_fila1.py:244`, `bin/disjuntor.py:1935`, `bin/codex_limite_estado.py:60`).

## 14/11 08:34 · sessao 6fc4ac798458 · - · - · origem=Ronald (tmux)

Correção estrita do C7:

app/chamados/models.py::DisputaSupervisao.pode_fechar ; a disputa pode fechar? (app/core/juizes.py:113) ; corte C7 exige fio_vivo vazio e nenhuma resposta sem veredito (app/docs/CORTES.md:272), corte posterior estabelece excecao para desligamento (app/docs/CORTES.md:451) ; no fechamento SEM porta de excecao exige nenhuma resposta sem veredito e nenhuma pergunta viva (app/chamados/models.py:1905, app/chamados/models.py:1909) ; nao ; DisputaSupervisao.fechar recompõe o predicado normal por conta propria (app/chamados/models.py:1942, app/chamados/models.py:1951), nessa porta externa ao juiz a recusa de resposta sem veredito tambem precede a excecao desligamento (app/chamados/models.py:1942), executor pula disputa respondida (app/colaboradores/services/desligamento.py:165), contrariando o ato exigido pelo corte posterior (app/docs/CORTES.md:451) ; -

Contagem conservadora deste recorte: 8 juizes lidos, 0 conflitos na coluna 5, 4 achados na coluna 6 — C3, C5, C7 e C8 —, 0 perguntas na coluna 7. Referencias: app/core/juizes.py:277 e as oito linhas entregues anteriormente [nao verificado: consolidacao das outras 15 linhas].

_4975 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
