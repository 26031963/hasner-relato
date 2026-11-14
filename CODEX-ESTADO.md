# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 09:12** · publicado em **14/11/2026 09:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 09:12 · sessao 24031e8f5efb · - · - · origem=Ronald (tmux)

Juiz 3 — `app/ponto/precedencia.py::fatos_do_dia`

- Código: delega ao período de um dia em `app/ponto/precedencia.py:256`. A implementação exclui ausências rejeitadas, aceita as demais em `app/ponto/precedencia.py:273`, aplica as faixas de tipos em `app/ponto/precedencia.py:52` e ordena pela precedência em `app/ponto/precedencia.py:396`, cuja tabela está em `app/ponto/precedencia.py:46`. Feriado não entra na disputa quando o dia está previsto e `not ec.feriado_vira_folga(data)`, em `app/ponto/precedencia.py:376`. Sem fatos, devolve faixa e tipo nulos em `app/ponto/precedencia.py:391`.
- Texto que responde: ordem completa em `app/docs/JUIZES.md:107`. Em LEIS.md/CORTES.md não achei texto que estabeleça a ordem completa. A L-089 trata do efeito de ausência em folga, em `app/docs/LEIS.md:121`, mas seu universo é abono/desconto, portanto não a equipararia automaticamente à classificação da A2. O corte `app/docs/CORTES.md:360` manda os emissores consultar este juiz, sem estabelecer a ordem.
- Conflito comprovável, coluna 5: **sim**, `app/docs/JUIZES.md:94` escreve a exceção somente como `trabalha_em_feriado`, enquanto `app/ponto/precedencia.py:376` usa `not ec.feriado_vira_folga(data)`. Essa função também retorna falso para ciclos fora de `CICLOS_FERIADO_VIRA_FOLGA`, independentemente da flag, em `app/escala/models.py:1392`. O próprio comentário descreve o caso 12x36 com flag falsa em `app/ponto/precedencia.py:373`.
- Coluna 6: achado **parcial**, `app/escala/utils.py:1210` decide ausência aprovada contra folga por conta própria e escreve `tipo_dia` em `app/escala/utils.py:1214` e `app/escala/utils.py:1220`. A cobertura vem de outro juiz, específico de folha, em `app/escala/utils.py:1188`. Portanto há prioridade própria ausência versus folga, mas o universo não coincide integralmente com a A2. Se a consolidação exigir igualdade completa de universo, registrar “nenhum achado” para cópia integral. Busca: `PRECEDENCIA`, `PRECEDENCIA.index`, `faixa_de`, `tipo_do_dia`, `tipo_dia`, `feriado_so_no_pagamento`; fora das partes citadas, os resultados eram leitura/delegação ou pergunta diferente.
- Coluna 7: possível pergunta documental, caso se aceite comentário de código como candidata escrita: “a exceção depende só da flag (`app/docs/JUIZES.md:94`) ou do predicado compartilhado com o gerador (`app/ponto/precedencia.py:371`)?” Se “duas candidatas escritas” exigir dois textos normativos externos ao código, usar **“-”**: não achei o corte MANHA-0511 nos cinco documentos pedidos.

Juiz 4 — `app/core/feriados.py::eh_feriado`

- Código: `app/core/feriados.py:105` devolve booleano do conjunto formado em `app/core/feriados.py:83`. Nacional vale sempre, estadual exige UF, municipal exige UF e município, desconhecido não vale: `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:67`. Normaliza acentos/caixa/espaços em `app/core/feriados.py:32`; sem município, UF/cidade são nulos em `app/core/feriados.py:50`, portanto só nacional passa.
- Coluna 3: **nenhum texto** em LEIS.md/CORTES.md que estabeleça esse alcance. `app/docs/JUIZES.md:109` somente identifica a autoridade.
- Coluna 5: **nao**. O texto explicativo de nacional/UF/município corresponde ao código, em `app/core/feriados.py:17` e `app/core/feriados.py:60`.
- Coluna 6: **nenhum achado** de cópia do alcance após busca de `Feriado.objects`, `abrangencia`, `eh_feriado`, `feriados_de`, `municipio` em Python de produção, incluindo commands, excluindo testes/migrations. O motor lê conjunto injetado em `app/ponto/motor_calculo_v2.py:963`; seu fallback delega em `app/ponto/motor_calculo_v2.py:41`. `feriados_do_colab` compõe os juízes existentes em `app/core/feriados.py:161` e `app/core/feriados.py:164`.
- Coluna 7: **“-”**.

Juiz 10 — `app/ponto/precedencia.py::folga_lancada_do_dia`

- Coluna 3: corte **GRADE-FOLGA-JUIZ**, `app/docs/CORTES.md:417`: calendário pertence ao vínculo, manual pertence à pessoa e sobrevive à troca. Nenhum L-NNN específico encontrado. O mesmo texto está em `app/docs/JUIZES.md:114`.
- Código: devolve a primeira candidata aceita, ou `None`, em `app/ponto/precedencia.py:216`; carregador ordena por pk em `app/ponto/precedencia.py:207`. O predicado rejeita `None`, aceita manual e aceita calendário somente do vínculo do dia em `app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`.
- Coluna 5: **nao**, corte e código do juiz concordam, `app/docs/CORTES.md:417` e `app/ponto/precedencia.py:237`.
- Coluna 6: **achado inequívoco**, auditor C5 em `app/chamados/management/commands/auditar_invariantes_chamados.py:1536`: inclui FolgaDia quando seu próprio vínculo cobre ou qualquer outro vínculo cobre, em `app/chamados/management/commands/auditar_invariantes_chamados.py:1538`, sem aplicar distinção de origem. Depois acusa a célula com trabalho em `app/chamados/management/commands/auditar_invariantes_chamados.py:1546`. A limitação da catraca que deixa esse caso passar está declarada em `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19`. Busca: `FolgaDia.objects`, `calendario_origem_id`, `folga_lancada_vale_no_dia`, `folga_lancada_do_dia`. A grade já delega em `app/escala/utils.py:1149`; não contá-la como cópia.
- Coluna 7: **“-”**; não achei dois textos normativos concorrentes para essa pergunta.

## 14/11 09:12 · sessao 0b01170cd260 · - · - · origem=Ronald (tmux)

Previsto: há derivação independente no montador da grade. `montar_grade_prevista_periodo_por_turno` soma a duração dos segmentos de marcos E→S em `min_por_data[d]` (app/escala/utils.py:1635) e publica o resultado em `minutos_previstos` (app/escala/utils.py:1809). É evidência mais aderente para a coluna 6 que o fallback do motor. Busca: `min_por_data`, `minutos_previstos`.

Dia de trabalho: `fatos_do_periodo` decide `previsto` lendo diretamente `cel.trabalha` ou chamando `eh_dia_trabalho_calculado` (app/ponto/precedencia.py:357, app/ponto/precedencia.py:366). Há diferença concreta quando a célula existe com `trabalha=None`: o juiz registrado devolve `None` (app/escala/models.py:1094), enquanto esse leitor tenta calcular novamente (app/ponto/precedencia.py:364). Busca: `trabalha`, `previsto`, `eh_dia_trabalho_calculado`.

## 14/11 09:12 · sessao 31d98b4b56d5 · - · - · origem=Ronald (tmux)

ponto/turnos.py::realizado_do_dia ; quantos minutos o dia realizou? (app/core/juizes.py:194) ; L-102: palavra sem número no dia com ata e sem par (app/docs/LEIS.md:128), L-103: escritor do fechamento converte ausência de turno em zero declarado (app/docs/LEIS.md:129), L-115: saída posicional depois da pausa fechada (app/docs/LEIS.md:151), nenhum texto correspondente localizado em CORTES.md por busca de realizado_do_dia, BUG-144 e R1-BALDE-A [nao verificado] ; pede turnos e intervalo do vínculo e delega ao corpo puro (app/ponto/turnos.py:779, app/ponto/turnos.py:788, app/ponto/turnos.py:794, app/ponto/turnos.py:795), sem turno devolve minutos=None (app/ponto/turnos.py:720), saída posicional usa última batida menos pausa (app/ponto/turnos.py:725), demais turnos somam duração até último fechamento menos pausas reconhecidas ou janela fixa declarada, sem desconto de janela livre não batida (app/ponto/turnos.py:735, app/ponto/turnos.py:756, app/ponto/turnos.py:760, app/ponto/turnos.py:766, app/ponto/turnos.py:771) ; nao: None do juiz e zero do escritor são explicitamente distinguidos pela L-103 (app/docs/LEIS.md:129, app/ponto/turnos.py:721) ; achado: cadastro_realidade calcula líquido do dia por saída menos entrada menos intervalo, por conta própria (app/escala/services/cadastro_realidade.py:238, app/escala/services/cadastro_realidade.py:241), drawer soma pares E→S pelo tipo gravado para presença de sete dias (app/colaboradores/services/drawer.py:45, app/colaboradores/services/drawer.py:48, app/colaboradores/services/drawer.py:51), buscas: minutos_realizados, realizado_do_dia, realizado_dos_turnos, total_seconds ; -

ponto/status_dia.py::status_do_dia_lavrado ; qual o status do dia, incluindo ok, irregular, falta, folga e ausência? (app/core/juizes.py:199) ; L-039: falta significa decidida (app/docs/LEIS.md:87), L-061: consumidor lê a lâmpada (app/docs/LEIS.md:95), L-073: teto por instante ou fato encerrado (app/docs/LEIS.md:100), E3-STATUS-JUIZ: status lavrado ou andamento antes do encerramento (app/docs/CORTES.md:415) ; avulso chama o lote de um par (app/ponto/status_dia.py:218), turno aberto ou batida antes do encerramento retorna aberto (app/ponto/status_dia.py:280, app/ponto/status_dia.py:283), dia encerrado com status lavrado lê a coluna (app/ponto/status_dia.py:287), batida sem lavra retorna sem_certidao (app/ponto/status_dia.py:292), sem batida e sem lavra consulta natureza A2 e retorna falta/ausencia, folga, previsto antes do encerramento ou sem_certidao depois, e sem_dados sem fato (app/ponto/status_dia.py:298, app/ponto/status_dia.py:310, app/ponto/status_dia.py:312, app/ponto/status_dia.py:314, app/ponto/status_dia.py:318) ; nao (app/docs/CORTES.md:415, app/ponto/status_dia.py:280, app/ponto/status_dia.py:287) ; nenhum achado, procurados status_dia, status_do_dia, sem_certidao, escala_suspeita, folga_trabalhada e atribuições/returns do vocabulário em app e templates, leitores conferidos delegam ao D4 (app/colaboradores/services/calendario.py:606, app/api/views.py:1483, app/ponto/services/espelho.py:170), previsão do painel é declaradamente outra pergunta (app/core/services/painel_op.py:27) ; -

Contribuição para contagem: 2 juízes lidos (app/core/juizes.py:194, app/core/juizes.py:199), 0 conflitos vigentes identificados nas duas linhas acima, 1 com achado externo (app/escala/services/cadastro_realidade.py:241), 0 perguntas de lei com duas candidatas vigentes identificadas [nao verificado].

DOSSIES.md:142 conserva “com o número da ata rotulado”, mas L-102 registra expressamente sua alteração posterior para “sem número”, portanto é superação documentada e não conflito vigente (app/docs/DOSSIES.md:142, app/docs/LEIS.md:128).

## 14/11 09:11 · sessao 0b01170cd260 · - · - · origem=Ronald (tmux)

Juízes 1, 2 e 7 investigados. Os três têm conflito textual com o código ou entre candidatas escritas, e os três têm leitor/escritor externo localizado. Referências e linhas propostas foram enviadas para consolidação.

O conflito do previsto tem duas candidatas expressamente registradas: descontar a pausa do template, chegando a 480 minutos (app/escala/utils.py:2298), ou respeitar a ausência de pausa declarada, chegando a 540 minutos (app/escala/utils.py:2300). O código vigente mantém o desconto (app/escala/utils.py:2314).

Para trabalho, L-022 exige a foto mais próxima ou resposta indefinida sem foto (app/docs/LEIS.md:73), enquanto o cálculo usa a âncora no mês sem foto (app/escala/models.py:1289).

Para furo, L-087 declara que o tipo gravado não decide (app/docs/LEIS.md:119), mas o juiz decide pela presença e contagem de E/S após o veto posicional (app/escala/servico_jornada.py:459, app/escala/servico_jornada.py:480).

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

_4979 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
