# GRADE-MESTRA -- mapa de todas as obras abertas

> Aval Ronald GRADE-MESTRA (13/11). Documento so; nenhuma lei nova; substitui o MAPA-20-DERIVADO (ref a82f7f536).
> Fonte: Codex leitura sobre o HEAD 00a370839 (BACKLOG bloco OBRAS, 4 faixas), conferida pelo Code em amostra de 10 linhas.
> Manutencao (L-106): marco que muda o estado de uma obra atualiza a linha dela no MESMO commit de docs; obra nova entra no registro.
> Grade (9 casas, nesta ordem): cen-lei-con-R/G-mut-ver-opus-pou-ar. `+` feito, `~` parcial, `.` falta.

## Contagem por balde

| balde | obras | faltam (estado != NO AR hoje) |
|---|---|---|
| ESPINHA-E | 327 | 327 |
| 20/20 | 25 | 22 |
| LATERAL-ESTRUTURA | 352 | 351 |
| INSTRUMENTO | 318 | 317 |
| UI | 86 | 86 |
| CEREJA | 24 | 24 |
| ? | 11 | 11 |
| **total** | **1143** | **1138** |

Estados: REGISTRADA 976; PARADA 137; INTERROMPIDA 16; EM VOO 7; NO AR hoje 5; BRIEF PRONTO 2

Fora da grade: obras com estado terminal anterior a 13/11 sem reabertura (excluidas na leitura; lista no rodape).

## ESPINHA-E (327)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| PERGUNTA-SEM-SELO | ESPINHA-E | Faz leitores estruturais consultarem a autoridade correta. | - | FECHAR2; autorização sobre exportada | +++...... | REGISTRADA (construção histórica) | BACKLOG:751 |
| O-PROJECOES | ESPINHA-E | Faz telas e documentos lerem resultados reconstruíveis da célula. | - | REJULGAR-COMP10-RESTO | .+....... | REGISTRADA | BACKLOG:756 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | ESPINHA-E | Faz motor e ata formarem os mesmos turnos. | - | MOTOR-PASSA-O-TURNO-A-VOLTA; grupo 1 | ++.....+~ | REGISTRADA (cura no ar; resíduos pendentes) | BACKLOG:776 |
| PENDENCIA-SEM-JUIZ-3 | ESPINHA-E | Faz três contadores de pendências usarem a mesma fila. | - | SILENCIO-VENCIDO-VOLTA | ++.....+~ | REGISTRADA (dois leitores no ar; falta um) | BACKLOG:782 |
| COBERTURA-PARCIAL-UM-JUIZ | ESPINHA-E | Faz ausências parciais cobrirem apenas o trecho devido. | - | COBPARC-DIFF-ISOLADO; autorização sobre exportada | .+++.+~+~ | PARADA (reaberta; PAREI de impacto) | BACKLOG:786 |
| GRADE-POS-POUSO | ESPINHA-E | Confere o efeito da nova grade após o deploy. | - | cartório pós-deploy; rebase COBPARC | .+....... | REGISTRADA | BACKLOG:789 |
| COBPARC-AUSENCIA-CALA-DOIS-JUIZES | ESPINHA-E | Faz dois caminhos concordarem sobre cobrar ausência parcial. | - | COBPARC | .+....... | REGISTRADA | BACKLOG:792 |
| COBPARC-FUROS-LEITOR-PARCIAL | ESPINHA-E | Faz leitores reconhecerem os furos restantes da ausência parcial. | - | COBPARC | .+....... | REGISTRADA | BACKLOG:793 |
| FIO-LE-O-JUIZ | ESPINHA-E | Mostra perguntas vivas mesmo em disputas encerradas. | - | clique de validação; push | .++....+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:794 |
| CXR-TELA-LE-O-MOTOR | ESPINHA-E | Mostra ao admin os dias divergentes apontados pelo motor. | - | ATA-X-TURNO-PAUSA; CXR-PRONTO5-DEFINICAO; lavra | .+++..++~ | REGISTRADA (no ar; conclusão parcial) | BACKLOG:798 |
| CXR-PRONTO1-COL841 | ESPINHA-E | Faz sete dias pendentes chegarem à lista do admin. | - | ATA-X-TURNO-PAUSA | .+....... | REGISTRADA | BACKLOG:799 |
| E1-ACERVO-5-GRUPOS-09 | ESPINHA-E | Trata cinco grupos antigos preservando a competência exportada. | - | REGEN-EM-EXPORTADA; DIFF; autorização | .+....... | PARADA (retida por aval) | BACKLOG:800 |
| RETRATADA-CONVERSA-E-REABRIR | ESPINHA-E | Impede conversas sobre batidas já retratadas. | - | pouso E1 | .+....... | REGISTRADA | BACKLOG:802 |
| E1-A2-VAZIO-MEDIR | ESPINHA-E | Identifica perguntas cuja origem não permite determinar o dia. | - | pouso E1; medida na sombra | .+....... | REGISTRADA | BACKLOG:803 |
| E1-DUPLICATA-SOBRE-RETRATADA | ESPINHA-E | Trata perguntas duplicadas sobre episódio retratado. | - | pouso E1; decisão junto da E3 | ......... | REGISTRADA | BACKLOG:805 |
| CXR-PRONTO5-DEFINICAO | ESPINHA-E | Define e mede quando um dia ganhou dono na lista. | - | definição do resultado | .+....... | REGISTRADA | BACKLOG:806 |
| JUIZ-DO-PAR-GRUPO-1 | ESPINHA-E | Faz leitores de batida e marco consultarem a mesma autoridade. | - | detector; LASTRO-DEGRAU-2 | ++.....+~ | REGISTRADA (primeiro pouso no ar; grupo incompleto) | BACKLOG:812 |
| MOTOR-PASSA-O-TURNO-A-VOLTA | ESPINHA-E | Faz o cálculo reconhecer corretamente a volta do intervalo. | - | grupo 1; DIFF; reversão | ++....... | REGISTRADA | BACKLOG:813 |
| O261-COMPLEMENTO-[nome] | ESPINHA-E | Faz perguntas vivas chegarem ao colaborador por chamado vivo. | - | O261; nascimento; contador de alcance | .++...... | REGISTRADA (parte construída) | BACKLOG:816 |
| C6-TETO-MARCO-PENDENTE | ESPINHA-E | Evita cobrar um marco antes de terminar seu prazo. | - | cura no juiz C6 | .+....... | REGISTRADA | BACKLOG:818 |
| PASSIVO-VALIDADO-K6 | ESPINHA-E | Usa a autoridade correta para reconhecer competência entregue. | - | juiz K6 | .+....... | REGISTRADA | BACKLOG:819 |
| PROGRAMA-CELULA-E-O-DIA | ESPINHA-E | Faz todas as telas lerem o mesmo julgamento do dia. | - | E1; E2; E3; E4 | .++...... | REGISTRADA (programa parcial) | BACKLOG:821 |
| PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS | ESPINHA-E | Organiza a unificação do status e a certificação do cálculo. | - | E2; E3-STATUS; E4; casos E1 | .++...... | REGISTRADA (construção histórica) | BACKLOG:822 |
| E4-CAL-RETIDO-FOLHA-NO-LEITOR | ESPINHA-E | Faz o calendário ler o status pronto da folha. | - | E4-CALENDARIO-L1; D4 | .+....... | REGISTRADA (complemento) | BACKLOG:823 |
| E4-CAL-FALLBACK-STATUS-ROTULO | ESPINHA-E | Faz o calendário usar um único rótulo do dia. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | BACKLOG:824 |
| E4-CAL-DOIS-CONTRATOS-DE-CELULAS | ESPINHA-E | Unifica o formato das células recebidas pelo calendário. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | BACKLOG:825 |
| E4-INCLUI-CONTRATO-20 | ESPINHA-E | Faz telas e rotinas lerem o status lavrado na célula. | - | E3; lista dos leitores | .+....... | REGISTRADA | BACKLOG:826 |
| GRADE-FOLGA-DO-VINCULO | ESPINHA-E | Impede folgas de vínculos encerrados de reaparecerem na grade. | - | E4-1 | .++...... | REGISTRADA (construção histórica) | BACKLOG:827 |
| GRADE-V5-SEM-PORTAO | ESPINHA-E | Corrige folga de vínculo encerrado preservando a folha entregue. | - | ? (não indicada) | .++...... | REGISTRADA (construída; curso atual não comprovado) | BACKLOG:844 |
| GRADE-FOLGA-JUIZ | ESPINHA-E | Faz os leitores usarem a mesma decisão sobre folga lançada. | - | decisão GRADE-TETO | .++...~.. | REGISTRADA (construção antiga; opus reprovado) | BACKLOG:845 |
| E4-7-D4-ABERTO-SEM-TURNO | ESPINHA-E | Investiga dias mostrados abertos sem existir turno aberto. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:848 |
| E4-8-SAIDA-I | ESPINHA-E | Mostra a ausência de cobrança registrada pela célula. | - | manter ou reverter; E4-D5-PROD | .+++++++~ | PARADA (parte no ar; decisão pendente) | BACKLOG:852 |
| E4-CAL-DIA-COM-FIM-LE-A-FOLHA | ESPINHA-E | Faz calendário e folha mostrarem a mesma pendência. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento da L1) | BACKLOG:855 |
| E4-CALENDARIO-L1 | ESPINHA-E | Faz o calendário ler o estado oficial sem recalculá-lo. | casos 01–10 | ISENTO-H10; complementos L1 | .+++~.... | PARADA (interrupção antiga; complemento novo registrado) | BACKLOG:856; app/docs/RELATO.md:37 |
| COBPARC-PORTAO-PARCIAL-JUIZ-UNICO | ESPINHA-E | Unifica a regra dos leitores para cobrar ausências parciais. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:860 |
| COBPARC-FILA-MATERIALIZACAO | ESPINHA-E | Faz a cobertura da ausência ser lida da decisão da célula. | - | COBERTURA-PARCIAL-UM-JUIZ | ......... | REGISTRADA | BACKLOG:862 |
| E4-CALENDARIO-QUEM-PUBLICA-EH-TRABALHO | ESPINHA-E | Faz o calendário perguntar à decisão oficial se há trabalho no dia. | - | E4-CALENDARIO-L1, passo 3 | .++...... | REGISTRADA (construção histórica) | BACKLOG:869 |
| D5-VIA-NULA | ESPINHA-E | Impede completar o estado por uma via sem identificação. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:870 |
| E4CAL-P1-ROTULO-SEM-CERTIDAO | ESPINHA-E | Investiga células com estado preenchido e rótulo antigo. | - | medir dono L-099 | ......... | REGISTRADA | BACKLOG:874 |
| E4CAL-P3-ULTIMO-RECURSO-ENGOLE | ESPINHA-E | Impede o calendário de esconder falhas do julgamento do dia. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:875 |
| E4CAL-P3-CENSO-HOJE-PORTA-CURTA | ESPINHA-E | Faz a porta curta receber o mesmo relógio do censo. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:876 |
| E4CAL-P3-LEITURA-ESCREVE | ESPINHA-E | Evita que consultar o calendário tente materializar perguntas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:877 |
| E4CAL-L1-POUSO-0411 | ESPINHA-E | Publica o calendário com os resultados previstos para a L1. | - | conferência do vigia na retomada | .++....+~ | REGISTRADA (pouso antigo; conferência posterior indicada) | BACKLOG:878 |
| PAREADOR-JANELA-ORFA | ESPINHA-E | Faz consulta mensal e isolada darem a mesma decisão para o dia. | - | ? (não indicada) | ...~..... | PARADA (RED antigo estacionado) | BACKLOG:880 |
| E4CAL-CARD-LE-NUCLEO | ESPINHA-E | Faz cartões do calendário usarem os rótulos oficiais. | - | retomada L1 | ......... | REGISTRADA | BACKLOG:881 |
| STATUS-ESCALA-SUSPEITA-VOCAB | ESPINHA-E | Corrige rótulo de ausência de batida quando existe batida. | - | aval para mudar a lavra | ......... | PARADA (aguarda aval) | BACKLOG:882 |
| A2-RAISE-NO-FUROS | ESPINHA-E | Evita erro de tela quando A2 recusa o estado do dia. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:883 |
| E4-APP-MAPA | ESPINHA-E | Faz o aplicativo mostrar o estado oficial por um único adaptador. | - | leitores 1–3 da E4 | .+++++.++ | REGISTRADA (pouso antigo relatado; fontes divergem) | BACKLOG:885; app/docs/RELATO.md:1720 |
| O261-E-CELULA-20-UM-DONO | ESPINHA-E | Garante caminho de ação para perguntas vivas. | - | decisões F3; etapas F4–F5 | ++++++++~ | PARADA (F1–F2 no ar; F3 aguarda desenho) | BACKLOG:893 |
| O261-E-PAI-TERMINAL-RESIDUAL | ESPINHA-E | Completa a ligação entre pergunta e chamado vivo por dia. | - | corte da tabela de triagem | +........ | PARADA (aguarda corte da tabela) | BACKLOG:894 |
| O261-E-F5B-JUIZ-PORTA-ALCANCAVEL | ESPINHA-E | Faz os leitores concordarem sobre o caminho de ação do dia. | - | F5; precedência contraditória | .+....... | REGISTRADA | BACKLOG:895 |
| O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL | ESPINHA-E | Identifica perguntas cujo clique já não produz efeito. | - | registro do juiz com corte | .+....... | REGISTRADA | BACKLOG:896 |
| O261-E-F5C-FILA-DO-OPUS | ESPINHA-E | Corrige resíduos entre fila, auditoria, responsáveis e caminhos de ação. | - | cortes F5C-1 e F5C-2 | ......... | REGISTRADA | BACKLOG:897 |
| E1-PERGUNTA-AO-DP-CONTADOR | ESPINHA-E | Obtém respostas pendentes sobre horas extras, intervalo e convenção. | - | DP/contador | .+....... | PARADA (três respostas pendentes) | BACKLOG:898 |
| E1-STATUS-VERMELHOS | ESPINHA-E | Corrige leitores reprovados na certificação do estado. | - | E3/E4; um leitor por pouso | .+....... | REGISTRADA | BACKLOG:899 |
| V-FASE-AS-OF-CADASTRO-SEM-LINHA | ESPINHA-E | Leva ao admin problemas de cadastro de dias passados. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:903 |
| O261-E-F1C4-LEN-VIVOS | ESPINHA-E | Faz o suprajuiz ler a cobrança decidida na célula. | - | LENVIVOS-A6-ALARME | .+++++++~ | REGISTRADA (item c pousado; A6 pendente) | BACKLOG:906 |
| LENVIVOS-A6-ALARME | ESPINHA-E | Faz o alarme ler a cobrança lavrada sem recalculá-la. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:907 |
| O261-E-F1C4-MEMO-C6 | ESPINHA-E | Evita reutilizar resposta antiga após mudar a célula no ato. | - | próxima obra no C6 | +........ | REGISTRADA (caminho real não demonstrado) | BACKLOG:912 |
| MEMO-C6-BATIDAS-PREFETCH | ESPINHA-E | Confere se mudar batidas deixa a decisão em memória desatualizada. | - | medir transação externa | ......... | REGISTRADA (hipótese não verificada) | BACKLOG:913 |
| F1C4-3-NONE-COBRA | ESPINHA-E | Faz o dia indefinido continuar cobrando pela regra central. | - | tratamento dos 12 reabríveis listados | .++....+~ | REGISTRADA (cura antiga no ar; passivo listado) | BACKLOG:922 |
| F1C4-3-NONE-LEITORES | ESPINHA-E | Faz leitores restantes tratarem trabalho indefinido pela mesma regra. | - | censo; um leitor por pouso | ......... | REGISTRADA | BACKLOG:923 |
| CARTORIO-PROTESTA-1A-CLAUSULA | ESPINHA-E | Define o alcance da regra de dia sem trabalho no cartório. | - | corte do predicado | ......... | PARADA (pergunta antes da migração) | BACKLOG:925 |
| E3-GRADE-SUBTIPO-FOLGA | ESPINHA-E | Preserva o subtipo de folga necessário ao estado oficial. | - | cura da grade fora da E3 | ......... | REGISTRADA | BACKLOG:927 |
| E3-HASH-A2 | ESPINHA-E | Faz mudanças de feriado e folga invalidarem decisões antigas. | - | E2 | ......... | REGISTRADA | BACKLOG:929 |
| EMIS-LEITORES-RESTANTES | ESPINHA-E | Migra leitores restantes para a regra única de dia sem trabalho. | - | COBERTURA-PARCIAL-UM-JUIZ | .+.+..... | PARADA (aguarda precedência) | BACKLOG:930 |
| BUG-ENC-CELULAS-FORMA | ESPINHA-E | Corrige o formato das células entregues ao julgamento de turnos. | - | F2; E4-CARGA-PARIDADE | ++....... | INTERROMPIDA | BACKLOG:933; anexo:interrompidas.txt:9 |
| E4-NAO-LAVRADO-PASSADO | ESPINHA-E | Corrige a apresentação de dias passados sem decisão lavrada. | - | leitores E4 | ......... | REGISTRADA | BACKLOG:939 |
| E3-STATUS-EXPORTADA | ESPINHA-E | Completa o estado antigo sem modificar valores ou arquivos entregues. | - | pouso E3 | .+....... | REGISTRADA | BACKLOG:942 |
| CODS-DO-ATO | ESPINHA-E | Faz cada julgamento reagir apenas aos dados do próprio ato. | - | conferências posteriores registradas | .+++..++. | REGISTRADA (pouso antigo; deploy não comprovado na célula) | BACKLOG:943 |
| MODO-COMUM-OUT-DO-LOTE | ESPINHA-E | Impede dados do lote de decidirem o resultado de um ato individual. | - | prova do cartório; resíduos indicados | +++~..n+~ | REGISTRADA (código no ar; portão posterior indicado) | BACKLOG:946 |
| BACKFILL-UNICO | ESPINHA-E | Centraliza correções antigas com responsável e destino explícitos. | - | antes do último horizonte; corte de exportada | .+....... | REGISTRADA | BACKLOG:947 |
| E3-ISENTO-INSUMOS | ESPINHA-E | Faz a decisão do dia considerar a isenção de ponto. | - | ISENTO-H10 substituta | .+++..~.. | PARADA (SUPERADA por ISENTO-H10) | BACKLOG:954 |
| ISENTO-PONTO-JUIZ-REGISTRADO | ESPINHA-E | Declara a autoridade da isenção e elimina consultas paralelas. | - | corte; comparação de predicado e universo | ......... | REGISTRADA | BACKLOG:955 |
| E3-L02-PENDENTE | ESPINHA-E | Mostra o status usando a lista de problemas já calculada. | - | v3; saída lavrada | ++....... | REGISTRADA | BACKLOG:958 |
| F1C4-3-EMISSORES-FILA | ESPINHA-E | Faz emissores concordarem sobre dias sem cobrança. | - | censo dos juízes | ......... | REGISTRADA | BACKLOG:975 |
| E3-OPUS-FILA | ESPINHA-E | Completa garantias de status, trilha e atualização do dia. | - | E4 para os leitores indicados | ......~.. | REGISTRADA | BACKLOG:977 |
| F1C4-FILA-OPUS | ESPINHA-E | Corrige resíduos de travas, vínculos e reabertura de perguntas. | - | ? (não indicada) | ......~.. | REGISTRADA | BACKLOG:983 |
| E6-POR-DONO | ESPINHA-E | Separa resultados do oráculo pelo responsável pela divergência. | - | pouso F1C4 | .+....... | REGISTRADA | BACKLOG:984 |
| RESPONDIDA-SEM-TEXTO-DUAS-COPIAS | ESPINHA-E | Unifica a identificação de resposta sem texto. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:988 |
| E2-D3-TRABALHA-FORA-DA-IMPRESSAO | ESPINHA-E | Rejulga o dia quando muda sua condição de trabalho. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:990 |
| E2-D3-VINCULO-NA-IMPRESSAO | ESPINHA-E | Rejulga o dia quando vínculo ou escala mudam. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:991 |
| E2-D3-FERIADO-NA-IMPRESSAO | ESPINHA-E | Atualiza o julgamento quando muda um feriado aplicável. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:992 |
| E2-D3-CCT-NA-IMPRESSAO | ESPINHA-E | Atualiza o julgamento quando muda a convenção aplicável. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:993 |
| E2-D3-ISENCAO-NA-IMPRESSAO | ESPINHA-E | Atualiza o julgamento quando muda a isenção de ponto. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:994 |
| E2-D3-JUSTIFICATIVA-APROVADA | ESPINHA-E | Rejulga o dia após aprovar uma justificativa. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:995 |
| E2-D3-BATIDA-DA-VESPERA | ESPINHA-E | Atualiza o dia afetado por batida da véspera. | - | medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:996 |
| E2-D3-EDICAO-HUMANA-SEM-PORTA | ESPINHA-E | Registra a edição humana por uma porta identificável. | - | porta de edição; medir rejulgamentos | .+.+..... | REGISTRADA | BACKLOG:997 |
| O261-FIO-MORTO-PELO-RETRATO | ESPINHA-E | Encaminha dias que seguram a folha sem atendimento acessível. | - | O261-E-CELULA-20-UM-DONO | .++....~~ | REGISTRADA (curas no ar; residual absorvido) | BACKLOG:998 |
| DONO-NAO-VE-FASE-DESLOCADA | ESPINHA-E | Atribui ao cadastro os dias com fase de escala errada. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1001 |
| VETO-IMPRESSAO-NUNCA-BATEU | ESPINHA-E | Rejulga quem nunca bateu após mudar o veto aplicável. | - | VETO-A | ......... | REGISTRADA | BACKLOG:1008 |
| VETO-TRILHA-CICLO | ESPINHA-E | Registra novamente o veto que voltou depois de corrigido. | - | VETO-A | ......... | REGISTRADA | BACKLOG:1009 |
| VETO-ATO-HUMANO-MOSTRA-E-TRILHA | ESPINHA-E | Mostra e registra o veto que o admin decidiu ultrapassar. | - | pouso B do O261-VETO-NA-PORTA | .+....... | REGISTRADA | BACKLOG:1016 |
| DIA-DA-PERGUNTA-E-O-TURNO | ESPINHA-E | Liga cada pergunta ao dia correto do turno. | - | pouso A do VETO | .+....~.. | REGISTRADA (cópia anterior reprovada) | BACKLOG:1025 |
| O261-VETO-NA-PORTA | ESPINHA-E | Impede cobrança automática em dia vetado e preserva a decisão humana. | - | comparação posterior; lotes B/C | .++....~~ | REGISTRADA (A no ar; B/C pendentes) | BACKLOG:1026 |
| CALENDARIO-HOJE-NAO-E-FALTA | ESPINHA-E | Evita mostrar falta antes do encerramento do turno de hoje. | - | filas do opus | .++~..++~ | REGISTRADA (no ar antigo; fila do opus indicada) | BACKLOG:1029 |
| O121-A1-VINCULO-CORRIGE-O-PASSADO | ESPINHA-E | Permite corrigir vínculo passado pela porta autorizada. | - | marco em curso; VETO A | .++...... | REGISTRADA | BACKLOG:1043 |
| RELOGIO-INJETADO | ESPINHA-E | Faz os juízes receberem o instante do julgamento. | - | SELO-DIA-PELO-SIGNIFICADO; censo | .+....... | REGISTRADA | BACKLOG:1057 |
| GRUPO-1-DATA-SAIDA-ESPERADA | ESPINHA-E | Calcula a saída esperada com o vínculo válido naquele dia. | - | grupo 1, sítio 1 | .+....... | REGISTRADA | BACKLOG:1062 |
| FIO-MORTO-SEM-REPARADOR | ESPINHA-E | Mede dias cobrados cuja pergunta morreu sem reparador. | - | medição por caminho | .+....... | REGISTRADA | BACKLOG:1063 |
| E2-EMISSORES-FATOS-DO-DIA | ESPINHA-E | Faz o emissor considerar todos os fatos do dia. | - | contrato 20 | .+....... | REGISTRADA | BACKLOG:1065 |
| CHAVE-DE-ENTRADA-INCOMPLETA | ESPINHA-E | Inclui no julgamento entradas que possam envelhecer a célula. | - | caso medido de célula velha | .+....... | REGISTRADA | BACKLOG:1066 |
| GRUPO-1-LISTA-CODEX-T11 | ESPINHA-E | Migra quatro leitores restantes para as autoridades declaradas. | - | grupo 1; autorização dos helpers do motor | .+....... | REGISTRADA | BACKLOG:1072 |
| CONTRATO-20-E2-QUAL-ATO | ESPINHA-E | Evita cobrar ausência quando feriado ou folga já explica o dia. | - | O-OUTBOX | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1073 |
| EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | ESPINHA-E | Faz os turnos antigos usarem a mesma regra de expiração dos demais. | - | ORFAS | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1076 |
| JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | ESPINHA-E | Impede que a mesma pergunta mude de estado sem um fato registrado. | - | ? (precedência da retomada não escrita) | + + + + . + . . + | REGISTRADA (cura no ar; passivo e trilha pendentes) | app/docs/BACKLOG.md:1077 |
| JUIZ-DE-DONO-DO-DIA | ESPINHA-E | Separa problemas do sistema de problemas de cadastro ou batida. | - | corte para predicado de dono | + . . . . . . . . | PARADA (sem predicado autorizado) | app/docs/BACKLOG.md:1080 |
| GATE10-ESTRUTURA-EM-4-RUBRICAS-DA-10 | ESPINHA-E | Corrige diferenças entre quatro verbas calculadas e a regra aplicável. | - | corte de predicado; autorização se alcançar motor v2 | + + . . . . . . . | PARADA (predicado depende de corte) | app/docs/BACKLOG.md:1082 |
| ESMERIL familia (2) batida | ESPINHA-E | Detecta batidas escritas fora da porta ou lidas sem respeitar retratações. | - | família (1); suíte; merge | + + + + + . . . . | REGISTRADA (fechada na raia; suíte e merge pendentes) | app/docs/BACKLOG.md:1104 |
| ABONO-NO-AR-NAO-FECHOU | ESPINHA-E | Faz os valores diários reproduzirem corretamente o fechamento gravado. | - | autorização; tratamento da competência 10 | + + + . . . . . . | PARADA (09 fechada; pendência truncada de autorização) | app/docs/BACKLOG.md:1126 |
| VINCULO-LINHA-DO-TEMPO | ESPINHA-E | Mantém uma sequência de vínculos válidos sem sobreposição. | - | ESMERIL-2; censo de leitores; autorização para col899 | + + + + . . . . . | PARADA (porta pronta; tripwire e constraint pendentes) | app/docs/BACKLOG.md:1127 |
| O-DIA-PAGO | ESPINHA-E | Faz todas as telas lerem um único valor diário de pagamento. | - | fatias S3, S4, S5 e S5b | + + + . . . . . . | REGISTRADA (fechada por fatias; S5b citada em voo antigo) | app/docs/BACKLOG.md:1131 |
| E4-CALENDARIO | ESPINHA-E | Faz o calendário usar o mesmo pagamento diário dos demais leitores. | - | próximo deploy; O-DIA-PAGO S3b | . + + + . . . . . | REGISTRADA (fechada; deploy ainda citado como próximo) | app/docs/BACKLOG.md:1159 |
| E6-14 | ESPINHA-E | Identifica a causa de cada colaborador ainda fora da certificação. | - | troca da S3; descongelamento | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1162 |
| PLACAR-ESTRUTURAL | ESPINHA-E | Mostra se cálculo, telas e folha permanecem coerentes com o cadastro. | - | autorização dos casos da 09 exportada; lote do R6 | + + + . . . . . . | PARADA (resultados parciais; espera autorização) | app/docs/BACKLOG.md:1173 |
| L111-ORFAOS-DO-MOTOR | ESPINHA-E | Remove duas respostas antigas de cálculo que já não têm leitores. | - | conferência da O275/O277 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1181 |
| O219 | ESPINHA-E | Rejulga dias quando muda a regra, respeitando seu alcance e período. | - | TXT novo da 09; Pauta DP | + + + . . . . + + | REGISTRADA (regra no ar; TXT e pauta pendentes) | app/docs/BACKLOG.md:1197 |
| O228 | ESPINHA-E | Remove código obsoleto de célula e turno com prova de desuso. | - | - | + + + + . . . + + | REGISTRADA (inteira no ar; data do fecho incerta) | app/docs/BACKLOG.md:1201 |
| O222 | ESPINHA-E | Faz o veredito concordar com a cobrança criada no mesmo ato. | - | deploy do ensaio | + + + + . . . . . | REGISTRADA (curada; deploy pendente) | app/docs/BACKLOG.md:1213 |
| O218 | ESPINHA-E | Certifica que a lavratura produz ata correta e estável em diferentes cenários. | - | cura O217; cenários; relavra de col146 | + + . . . . . . . | REGISTRADA (instrumento no ar; bateria incompleta na fonte) | app/docs/BACKLOG.md:1214 |
| O221 | ESPINHA-E | Integra as curas de escrita única e julgamento pelas autoridades. | - | marco O218; suíte da junção | + + + . . . . + + | REGISTRADA (fonte diz no ar e também nada juntado) | app/docs/BACKLOG.md:1215 |
| O129 | ESPINHA-E | Declara o efeito dos 28 campos editáveis do assistente de escalas. | - | O127 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1220 |
| O140 | ESPINHA-E | Grava o furo do dia para as telas lerem a mesma resposta. | - | campo e porta furo_apurado; backfill autorizado | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1224 |
| BUG-8792 | ESPINHA-E | Faz a folha ler as faltas do dia julgado pela célula. | - | O140 no mesmo marco | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1225 |
| O125 | ESPINHA-E | Dá destino a problemas de cadastro ou batida ausentes da lista administrativa. | - | fonte Cadastro x Realidade; emissores existentes | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1232 |
| O117 | ESPINHA-E | Resolve a divergência sobre o previsto de células sem marcos. | - | corte se ainda desejado | + + . . . . . . . | PARADA (zero casos na frota; corte pendente) | app/docs/BACKLOG.md:1234 |
| O108 | ESPINHA-E | Faz o espelho mostrar furos e abonos com a alimentação completa do cálculo. | - | autorização para aplicar | + + + . . . . . . | PARADA (DIFF completo; espera autorização) | app/docs/BACKLOG.md:1243 |
| O105 | ESPINHA-E | Faz o arquivo da folha refletir de forma estável o valor gravado. | - | decidir substituição ou autópsia de datas_falta | + + . . . . . . . | PARADA (PAREI por divergência no TXT) | app/docs/BACKLOG.md:1246 |
| O9 | ESPINHA-E | Faz PDF, tela e aplicativo mostrarem os mesmos dados do espelho. | - | pista para DIFF de frota; passos restantes | + + + . . . . . . | REGISTRADA (passo 1 fechado; restantes pendentes) | app/docs/BACKLOG.md:1262 |
| PREVISTO-EM-DUAS-[nome] | ESPINHA-E | Faz o previsto exibido concordar com a autoridade do previsto. | - | censo dos escritores de minutos_previstos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1276 |
| O14 | ESPINHA-E | Faz o aplicativo atribuir batidas ao mesmo dia de turno do espelho. | - | smoke no celular | + + + . . . . + + | REGISTRADA (no ar desde 01/10; smoke pendente) | app/docs/BACKLOG.md:1293 |
| O27 | ESPINHA-E | Faz o cartão usar a janela correta e totalizar suas linhas. | - | decisão sobre total; O156 | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1308 |
| O33 | ESPINHA-E | Faz o total do cartão somar os minutos exibidos nas linhas. | - | O27; O156 | + + . . . . . . . | PARADA (espera ! da O27) | app/docs/BACKLOG.md:1314 |
| O35 | ESPINHA-E | Mostra e cobra o que falta para fechar os contratos estruturais. | - | O134; famílias pendentes | + + ~ + . . . . . | REGISTRADA (partes construídas; contratos incompletos) | app/docs/BACKLOG.md:1316 |
| O37 | ESPINHA-E | Preserva a alternância do plantão quando existe folga avulsa. | - | ! do passivo exportado | + + + . . . . . + | PARADA (cura no ar; passivo espera !) | app/docs/BACKLOG.md:1318 |
| O40 | ESPINHA-E | Usa a competência correta para decidir se um dia está trancado. | - | fronteira FM/PeriodoFechado; O44 | + + ~ + . . . . + | REGISTRADA (tripwire no ar; fronteira pendente) | app/docs/BACKLOG.md:1321 |
| O44 | ESPINHA-E | Permite retomar um dia antigo sem destrancar outros dias. | - | smoke do caso 1; critérios do item | + + + + . . . + . | REGISTRADA (pousada; marco sem data explícita) | app/docs/BACKLOG.md:1325 |
| O45 | ESPINHA-E | Faz tela e cartão mostrarem os mesmos dias em aberto. | - | cura na fonte identificada | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1326 |
| O48 | ESPINHA-E | Faz o fechamento ler o dia atual em vez de cálculo envelhecido. | - | status encerrado; migration; ! do passo 5 | + + ~ . . . . + + | REGISTRADA (passos no ar; status e exportação pendentes) | app/docs/BACKLOG.md:1329 |
| O49 | ESPINHA-E | Impede pagar o intervalo cadastrado como hora extra indevida. | - | E2; DIFF; ! | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1330 |
| O50 | ESPINHA-E | Impede vínculos com fim anterior ao início e encaminha o passivo. | - | saneamento dos 62; !; constraint | + + + . . . . . . | PARADA (passivo espera !) | app/docs/BACKLOG.md:1331 |
| O50-DESLIGAR-ENCERRA-TUDO | ESPINHA-E | Faz o desligamento encerrar vínculos, previsões futuras e pendências pela porta. | - | LEI-DESLIGADO; vaga serial; ! retroativo | + + . . . . . . . | REGISTRADA (censo concluído; construção na próxima vaga) | app/docs/BACKLOG.md:1332 |
| O50-CELULA-APOS-DESLIGAMENTO | ESPINHA-E | Impede gerar trabalho previsto após o desligamento. | - | O50-DESLIGAR-ENCERRA-TUDO; ! retroativo | + + . . . . . . . | REGISTRADA (absorvida pela variante da O50) | app/docs/BACKLOG.md:1333; app/docs/BACKLOG.md:1332 |
| O50-COMPLEMENTO-LAUDO-1311 | ESPINHA-E | Preserva trabalho anterior e elimina previsões futuras indevidas ao desligar. | - | O50-DESLIGAR-ENCERRA-TUDO; ! retroativo | + + . . . . . . . | REGISTRADA (absorvida pela variante da O50) | app/docs/BACKLOG.md:1334; app/docs/BACKLOG.md:1332 |
| O51 | ESPINHA-E | Mostra a saída antecipada em coluna própria no cartão. | - | E4 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1335; app/docs/BACKLOG.md:1338 |
| O52 | ESPINHA-E | Impede que o cartão rotule folga como dia abonado por treino. | - | E4; leitura da célula | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1336; app/docs/BACKLOG.md:1338 |
| O53 | ESPINHA-E | Faz cálculo e telas usarem uma única interpretação das marcações. | - | quatro itens da FILA; O37 antes da fase 2 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1337 |
| O54 | ESPINHA-E | Faz previsão, cálculo, telas e fechamento concordarem sobre cada dia. | - | portas E0→E6 provadas em sequência | + + ~ . . . . . . | REGISTRADA (programa parcial; curso atual deste id não demonstrado) | app/docs/BACKLOG.md:1338 |
| O60 | ESPINHA-E | Faz o motor ler períodos e jornada declarados pelas autoridades. | - | DIFF; ! da competência 09 | + + . . . . . . . | PARADA (espera ! de critério) | app/docs/BACKLOG.md:1345 |
| O61 | ESPINHA-E | Mostra a diferença entre o TXT entregue e o motor curado da 09. | - | O60; identificação do TXT correto | + + . . . . . . . | REGISTRADA (07/08 excluídas do acerto por corte) | app/docs/BACKLOG.md:1346 |
| O62 | ESPINHA-E | Faz o juiz reconhecer intervalo cadastrado sem depender de marcos congelados. | - | corte de alcance do juiz | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1347 |
| O63 | ESPINHA-E | Evita pagar folga diferentemente por faltar arquivo na sombra. | - | corte sobre a fonte da decisão financeira | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1348 |
| O65 | ESPINHA-E | Faz a geometria ler o papel atribuído às batidas pela ata. | - | O134 item 1; O142 para classe C | + + + . . . . . . | REGISTRADA (BACKLOG aberto; placar diz aplicada) | app/docs/BACKLOG.md:1350; app/core/espelho_verdade.py:77 |
| O66 | ESPINHA-E | Garante caminho de resolução para todo dia em furo ou cobrança. | - | O65; ! do passivo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1351 |
| O67 | ESPINHA-E | Corrige dias com tipo errado, julgamento prematuro ou abono em folga. | - | O66; medições por classe | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1352 |
| O67b | ESPINHA-E | Faz totais, datas e rótulos do cartão corresponderem ao trabalho apurado. | - | O66; O67; medição de frota | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1353 |
| O68 | ESPINHA-E | Impede pagamento silencioso de dias que a ata não explica. | - | O71; O65; ! da parte b | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1354 |
| O51b | ESPINHA-E | Mostra atraso e saída antecipada com os valores da folha. | - | O68; antes do envio de 09 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1355 |
| O70 | ESPINHA-E | Faz cada célula informar qual vínculo gerou a previsão do dia. | - | O68; DIFF; ! se mudar número | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1357 |
| O71 | ESPINHA-E | Atualiza vereditos antigos antes de decidir quem entra na folha. | - | DIFF; ! se alguém sair do TXT | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1358 |
| O72 | ESPINHA-E | Impede que saída após meia-noite vire desconto de um dia inteiro. | - | antes do apply da O68; censo de frota | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1359 |
| O73 | ESPINHA-E | Impede atraso na volta da pausa e certifica resíduos do intervalo. | - | bateria; baldes restantes; L-115 | + + ~ . . . . + . | REGISTRADA (núcleo FEITO no placar; complemento pendente) | app/docs/BACKLOG.md:1360; app/core/espelho_verdade.py:117 |
| O76 | ESPINHA-E | Atribui o plantão ao dia de início mesmo após meia-noite. | - | censo de frota | + + + + . . . . . | REGISTRADA (BACKLOG aberto; placar diz aplicada em 27/09) | app/docs/BACKLOG.md:1363; app/core/espelho_verdade.py:80 |
| O88 | ESPINHA-E | Evita que cadastro deslocado impeça parear um dia trabalhado. | - | O86; L-084 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1368 |
| O87 | ESPINHA-E | Recusa cadastro com intervalo fora do turno e declara o passivo. | - | push E1; ! para os sete templates | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1369 |
| O86 | ESPINHA-E | Separa saída realmente ausente de falha no pareamento do turno. | - | O85; O84; causas O87/O88 | + + ~ + . . . . . | REGISTRADA (guarda com DIFF zero; investigação residual) | app/docs/BACKLOG.md:1370 |
| O95 | ESPINHA-E | Impede absorver saída real como pausa sem prova de retorno. | - | censo; DIFF de frota | + . . . . . . . . | PARADA (cura candidata revertida; espera censo) | app/docs/BACKLOG.md:1371 |
| O94 | ESPINHA-E | Submete à decisão o pagamento antigo de turnos sem saída. | - | ! sobre o gravado | + + + . . . . . + | PARADA (código no ar; gravado espera !) | app/docs/BACKLOG.md:1373 |
| O93 | ESPINHA-E | Impede herdar um dia anterior sem turno real ao pagar folga trabalhada. | - | decisão sobre o dinheiro de 09 | + + . . . . . . . | PARADA (apply parado) | app/docs/BACKLOG.md:1374 |
| O92 | ESPINHA-E | Alimenta a previsão em lote para evitar consultas repetidas por dia. | - | quatro leitores; DIFF da cópia inline | + + ~ . . . . . . | REGISTRADA (chamador da tela fechado; demais pendentes) | app/docs/BACKLOG.md:1375 |
| O89 | ESPINHA-E | Mantém o resultado igual ao recalcular com os mesmos objetos. | - | ! de desenho segundo o BACKLOG | + + + + . . . . + | REGISTRADA (BACKLOG espera !; placar diz no ar em 27/09) | app/docs/BACKLOG.md:1378; app/core/espelho_verdade.py:109 |
| O85 | ESPINHA-E | Faz horas sem vínculo cobrindo o dia entrarem na apuração. | - | dado de vínculo pelo propositor; L-009 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1379 |
| O83 | ESPINHA-E | Separa por causa as horas que a folha deixa de reconhecer. | - | obras das classes A/B/C/D | + + . . . . . . . | REGISTRADA (curso atual deste id não demonstrado) | app/docs/BACKLOG.md:1381 |
| O136 | ESPINHA-E | Faz a escolha do motor reconhecer turno partido nos dois sentidos. | - | O134 item 5; DIFF; ! do motor | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1389 |
| O139 | ESPINHA-E | Distingue crons de prazo daqueles que julgam por conta própria. | - | selo de host no pouso 2 | + + ~ . . . . + + | REGISTRADA (produto no ar; selo pendente) | app/docs/BACKLOG.md:1393 |
| CHAMADO-EM-RAIA | ESPINHA-E | Concentra as mudanças de estado dos chamados numa porta. | - | O123; régua da trilha principal | . + . . . . . . . | REGISTRADA (voo só registrado em 03/10) | app/docs/BACKLOG.md:1394 |
| O141 | ESPINHA-E | Faz aprovação e entrega de fechamento usarem as autoridades declaradas. | - | baixa justificada de K4; K6 | + + ~ . . . . . . | REGISTRADA (K1 curado; K4/K6 pendentes de registro) | app/docs/BACKLOG.md:1395 |
| DOSSIES-DAS-CELULAS | ESPINHA-E | Organiza a cura dos contratos e leitores das famílias estruturais. | - | ordem O134; obras dos seis itens | + + ~ . . . . . + | REGISTRADA (parte no ar; restante sem curso atual comprovado) | app/docs/BACKLOG.md:1396 |
| SITUACAO-E-LAMPADA | ESPINHA-E | Faz os leitores interpretarem afastamento pela autoridade correta. | - | censo dos dez filtros; migração dos leitores | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1397 |
| O142 | ESPINHA-E | Faz o mesmo dia produzir o mesmo turno em janelas diferentes. | - | vínculo do dia; provas de invariância | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1398 |
| O156 | ESPINHA-E | Faz o cartão totalizar os valores impressos nas linhas. | - | item em curso; O27/O33 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1409 |
| O164 | ESPINHA-E | Centraliza alterações de chamados em portas rastreáveis. | - | C2 → C3 → C4 → C1 | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1418 |
| O166 | ESPINHA-E | Faz tarefas de prazo consultarem a autoridade responsável. | - | O139; juiz do prazo | .+~...... | REGISTRADA (construção antiga) | BACKLOG:1420 |
| O170 | ESPINHA-E | Faz cobranças acompanharem os acontecimentos sem esperar varreduras. | - | O167; unicidade; ordem dos eventos; O168; O169 | ++....... | REGISTRADA | BACKLOG:1425 |
| O171 | ESPINHA-E | Faz a folha acompanhar o julgamento atualizado do dia. | - | Medir primeiro evento posterior ao deploy | ++....... | REGISTRADA | BACKLOG:1426 |
| O177 | ESPINHA-E | Faz o cálculo considerar todas as pausas cadastradas. | - | DIFF de frota; reversão; exportadas intactas | ++....... | REGISTRADA | BACKLOG:1432 |
| O178 | ESPINHA-E | Dá responsável e prazo às pendências do administrador. | - | Prazo, dono e contador de leitura da pauta | ++....... | REGISTRADA | BACKLOG:1433 |
| O179 | ESPINHA-E | Investiga dias com batidas que chegam zerados à folha. | - | Medir horas e decidir o dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1434 |
| O180 | ESPINHA-E | Entrega ao administrador dias gerados além do vínculo. | - | Lista dos colaboradores; ! para regenerar passado | ++....... | PARADA (ato de passado espera !) | BACKLOG:1435 |
| O183 | ESPINHA-E | Impede chamados com urgências desconhecidas pelo sistema. | - | Merge; DIFF; tratamento do estoque | ++++..... | REGISTRADA (cura na raia; estoque pendente) | BACKLOG:1438 |
| O189 | ESPINHA-E | Publica o efeito de distinguir ausência de informação de zero. | - | Decisão da L-102 | ++....... | REGISTRADA (medida publicada; nada aplicado) | BACKLOG:1444 |
| O194 | ESPINHA-E | Faz a lavra da folga esperar o encerramento real do turno. | - | Fatia própria da lavra | ++....... | REGISTRADA | BACKLOG:1449 |
| O210 | ESPINHA-E | Atualiza fechamentos que ficaram atrás do cálculo atual. | - | Foto nova; DIFF; reversão; prova posterior | ++....... | REGISTRADA | BACKLOG:1464 |
| O229 | ESPINHA-E | Corrige diferenças entre marcos e leitores de horas. | - | Censo e RED por classe; O197 no item 9 | ++....... | REGISTRADA | BACKLOG:1466 |
| O215 | ESPINHA-E | Compara o arquivo pago com o cálculo atual da folha. | - | TXT na sombra; decisão sobre os dois valores | .+....... | REGISTRADA | BACKLOG:1467 |
| O216 | ESPINHA-E | Refaz a medição após a correção do juiz dos turnos. | - | O195; medição pela função real | ++....... | REGISTRADA | BACKLOG:1468 |
| O217 | ESPINHA-E | Verifica se repetir o julgamento estabiliza a ata. | - | Cura e bateria da O218 | ++....... | REGISTRADA (medição feita; cura na O218) | BACKLOG:1469 |
| O248 | ESPINHA-E | Centraliza a resposta sobre o encerramento real do dia. | - | O258; item 3 da fila | ++....... | REGISTRADA (complemento da O258) | BACKLOG:1480 |
| O249 | ESPINHA-E | Faz a detecção de pares consultar uma única autoridade. | - | O258; juiz antes da mudança do emissor | ++....... | REGISTRADA (complemento da O258) | BACKLOG:1481 |
| O252 | ESPINHA-E | Faz a aprovação seguir o mesmo critério da exportação. | - | Contrato batida × juiz; O48 passo 5; DIFF | ++....... | REGISTRADA | BACKLOG:1484 |
| O254 | ESPINHA-E | Dá testes ao contrato de batidas e coerência à retenção. | - | Corte do primeiro pendente; O48 passo 5; pendente 2 | +++....+. | REGISTRADA (marco B fechado; pendente permanece) | BACKLOG:1486 |
| O257 | ESPINHA-E | Centraliza a confirmação de entrega de uma competência. | - | O252; O48 passo 5; censo dos leitores | ++....... | REGISTRADA | BACKLOG:1487 |
| O258 | ESPINHA-E | Substitui decisões espalhadas por consultas aos juízes oficiais. | - | Item 2; três predicados novos; duas listas vazias | ++~....~+ | REGISTRADA (item 1 publicado; restante pendente) | BACKLOG:1488 |
| SOBRA-NA-BORDA-O-QUE-FICOU | ESPINHA-E | Reúne problemas restantes após mudanças em batidas antigas. | - | Obra própria do item a; censos por classe | ++....... | REGISTRADA | BACKLOG:1493 |
| C7-DISPUTA-FECHA-POR-UM-JUIZ | ESPINHA-E | Faz disputas encerrarem por uma única decisão verificável. | - | C6; escritor único de chamado; lotes após L0 | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1496 |
| BUG-DIA-SEM-SAIDA | ESPINHA-E | Oferece ação aos dias retidos sem pergunta viva. | - | Medições residuais; casos do DP | +++....++ | REGISTRADA (partes publicadas; resíduo pendente) | BACKLOG:1497 |
| BO-B-DIAPAGO-SE-CONTRADIZ | ESPINHA-E | Impede campos incompatíveis de horas no mesmo dia. | - | BO-A; ! do motor; impacto por rubrica | ++....... | REGISTRADA | BACKLOG:1501 |
| VARREDURA-VIRA-EVENTO-24-CRONS | ESPINHA-E | Faz tarefas reagirem aos eventos e deixa varreduras vigiarem. | - | O168; O170 C1; C7; medição zero por classe | ++~....~+ | REGISTRADA (migração parcial) | BACKLOG:1505 |
| CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | ESPINHA-E | Alinha escala e folha às mesmas autoridades. | - | Exportação da 10 para E1; corte de folha | ++~....~+ | PARADA (restantes esperam exportação e lei) | BACKLOG:1506 |
| E6-NONE-E-CADASTRO | ESPINHA-E | Encaminha ao cadastro o dia sem vínculo definido. | - | ? (não indicada) | +++...... | REGISTRADA (marcada FEITA, sem encerramento literal) | BACKLOG:1507 |
| GRADE-ESC-DO-DIA-E1 | ESPINHA-E | Faz a previsão consultar o vínculo oficial do dia. | - | Suíte; pouso; smoke | +++...... | REGISTRADA (cura na cópia; publicação pendente) | BACKLOG:1508 |
| CARIMBO-SEM-VIGENCIA | ESPINHA-E | Faz mudanças de vigência provocarem novo julgamento do dia. | - | Medir vínculos editados após o carimbo | +........ | REGISTRADA | BACKLOG:1509 |
| CONTADOR-PAREADOR-X-CELULA | ESPINHA-E | Detecta turno fechado cuja célula ainda acusa borda órfã. | - | Cura D2; comboio com TURNO-ABERTO item 3 | ++~...... | REGISTRADA (fontes divergem sobre publicação) | BACKLOG:1510; BACKLOG:1506 |
| CONTADOR-AUSENCIA-X-VEREDITO | ESPINHA-E | Detecta ausência aprovada contradita pelo julgamento do dia. | - | ! para quatro casos exportados | +++...... | PARADA (casos exportados esperam !) | BACKLOG:1511 |
| TURNO-ABERTO-UM-EMISSOR | ESPINHA-E | Elimina decisões duplicadas sobre turnos e suas perguntas. | - | Lei do item 4; diagnósticos suspensos; corte de foto | ++~....~+ | PARADA (item 4 em lei; demais suspensos) | BACKLOG:1513 |
| ATA-NAO-DISPENSA-JUSTIFICADA | ESPINHA-E | Faz a ata reconhecer o período abonado pela justificativa. | - | Acesso para aplicar o acervo da 10 | ++++...++ | PARADA (cura publicada; aplicação sem permissão) | BACKLOG:1515 |
| CONCORDE-PREVISTO-ZERO | ESPINHA-E | Investiga dias corretos com batidas e previsão zerada. | - | Medir a causa da previsão zero | ++....... | REGISTRADA | BACKLOG:1517 |
| VIVA-DELEGA-AO-JUIZ | ESPINHA-E | Faz a indicação de pergunta viva consultar o juiz comum. | - | Pouso; censo dos leitores | +++...... | REGISTRADA (construída; pouso pendente) | BACKLOG:1518 |
| Q-VIVAS-LEITORES-SEM-JUIZ | ESPINHA-E | Distingue perguntas realmente vivas das candidatas do filtro. | - | Medir o escritor de fechamento primeiro | +........ | REGISTRADA | BACKLOG:1521 |
| ESPELHO-VERDADE-REMEDIDO | ESPINHA-E | Atualiza o placar com testes e medições comprovados. | - | K6/E1; termômetro R1; antes da triagem | ++....... | REGISTRADA | BACKLOG:1522 |
| VARREDURA-TRIAGEM | ESPINHA-E | Classifica tarefas entre vigia, prazo e evento. | - | Segunda medição; medir vínculo e termômetro | ++~....~+ | REGISTRADA (triagem e lote parciais) | BACKLOG:1523 |
| BUG-CELULA-SEM-VEREDITO | ESPINHA-E | Separa células reais sem julgamento dos dados de teste. | - | SEM-CELULA-NA-JANELA-INTEIRA | ++....... | REGISTRADA (cancelada; resíduo encaminhado) | BACKLOG:1524 |
| BUG-FOLGA-COM-MARCOS | ESPINHA-E | Verifica folgas geradas com marcações indevidas. | - | Casos de dono BATIDA/CADASTRO | ++....... | REGISTRADA (premissa refutada) | BACKLOG:1525 |
| C5-LISTA-DE-FROTA | ESPINHA-E | Corrige faltas descontadas acima da jornada prevista. | - | Medir os 27 casos; invariante falta ≤ previsto | ++....... | REGISTRADA | BACKLOG:1529 |
| FERIADO-DIA-CHAVE | ESPINHA-E | Faz cálculo e fechamento reconhecerem o mesmo dia de feriado. | - | Dia-chave único; caso atravessando meia-noite | ++....... | REGISTRADA | BACKLOG:1532 |
| BUG-117-COMPLEMENTO | ESPINHA-E | Impede cobrar novamente uma resposta já aplicada. | - | Fila anterior | ++....... | REGISTRADA | BACKLOG:1533 |
| LISTA-POR-COMPETENCIA | ESPINHA-E | Separa problemas cadastrais por competência da folha. | - | Apply da competência 10 | .+....... | REGISTRADA | BACKLOG:1534 |
| O137-ESTA-EXPORTADA-K6 | ESPINHA-E | Faz a regularização consultar a autoridade de entrega da folha. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1536 |
| GEOFENCE-PERGUNTA-AO-JUIZ | ESPINHA-E | Faz alertas e localização usarem a mesma avaliação oficial. | - | DRY; declaração do juiz | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1541 |
| AUTORIDADE-DE-FOTO-AUSENTE | ESPINHA-E | Centraliza a decisão sobre ausência de foto. | - | Autoridade de foto antes de retirar o julgador | .+....... | REGISTRADA | BACKLOG:1542 |
| O262 | ESPINHA-E | Declara as autoridades das perguntas sobre escala. | - | Exportação da 10; decisão do nome | ++++...++ | PARADA (declaração publicada; migração pausada) | BACKLOG:1546 |
| O265 | ESPINHA-E | Atualiza o retrato da folha quando o dia termina. | - | Exportação da 10; BUG-LINHA-X-BADGE | .+....... | REGISTRADA | BACKLOG:1550 |
| O266 | ESPINHA-E | Faz a geração consultar as mesmas autoridades de vínculo. | - | DIFF das duas cópias | ++....... | REGISTRADA | BACKLOG:1551 |
| O268 | ESPINHA-E | Centraliza decisões de prazo e encerramento de quatro rotinas. | - | Exportação da competência 10 | .+....... | REGISTRADA | BACKLOG:1553 |
| O276 | ESPINHA-E | Impede fechar um turno acima do teto da jornada. | - | Destino do teto; medição 24x48 | +++...... | REGISTRADA (destravada; pouso sem prova) | BACKLOG:1561 |
| O140-FURO-COMPOSTO | ESPINHA-E | Preserva os dias de falta no fechamento composto. | - | Liberação de fechamento.py; reapuração; ! da 09 | ++....... | PARADA | BACKLOG:1603 |
| TELA-MENOR-QUE-O-GRAVADO | ESPINHA-E | Faz tela e escritor apresentarem as mesmas horas. | - | Corte da adição de 27/09 | ++....... | REGISTRADA | BACKLOG:1616 |
| FURO-APURADO-FALSO-EM-20378-LINHAS | ESPINHA-E | Faz o gravado identificar corretamente os dias com falta. | - | Recarimbo; DIFF; reversão; ! da exportada | ++~...... | REGISTRADA | BACKLOG:1625 |
| BO-CARTAO-ESTRELA-X-REALIZADO | ESPINHA-E | Investiga batida marcada como órfã que também soma horas. | - | Quatro itens restantes; condições O317 | ++....... | REGISTRADA | BACKLOG:1629 |
| REGUA-DO-PERIODO-COMPLEMENTO-19-10 | ESPINHA-E | Confere a régua por fatia e encaminha divergências. | - | Itens 1 e 2; pergunta 18; item 5 após 2 | ++....... | REGISTRADA | BACKLOG:1634 |
| BO-CARTAO-ESTRELA-COMPLEMENTO-19-10 | ESPINHA-E | Mede diferenças de adicional noturno e corrige a estrela. | - | Medição do item 3; RED do item 4 | ++~...... | REGISTRADA | BACKLOG:1635 |
| SUPRA-JUIZ-AINDA-DERIVA-POR-DATA | ESPINHA-E | Faz a revisão consultar o juiz do encerramento do turno. | - | O258 | ++....... | REGISTRADA | BACKLOG:1655 |
| O325 | ESPINHA-E | Detecta fechamentos vencidos e identifica quem os atualiza. | - | ! RECALC-09 para a exportada | ++....... | REGISTRADA | BACKLOG:1660 |
| O327 | ESPINHA-E | Mostra quem o cálculo independente conseguiu avaliar. | - | Lei do ancoramento do trecho extra | +........ | PARADA (espera lei) | BACKLOG:1662 |
| RETIFICACAO-DA-10-8069-PRIMEIRO | ESPINHA-E | Corrige diferenças de pagamento começando pela rubrica 8069. | - | Corte da 243; HE publicada; decisão do admin | ++....... | REGISTRADA (243 parada por corte) | BACKLOG:1666 |
| ITEM-4-ORFA-PELAS-LAMPADAS | ESPINHA-E | Encerra pergunta órfã cujo marco já foi atendido. | - | TURNO-ABERTO-UM-EMISSOR item 4 | .+....... | REGISTRADA | BACKLOG:1672 |
| CASA-CHAMADO-C2 | ESPINHA-E | Transfere avisos à lista do admin antes de encerrá-los. | - | Prova nominal dos 78 na lista | .+....... | REGISTRADA | BACKLOG:1680 |
| CASA-CHAMADO-C3 | ESPINHA-E | Faz sete verificações consultarem as autoridades comuns. | - | SETE-CRONS; ITEM-4-ORFA; censo do escore | ++~....++ | REGISTRADA (quatro publicados; três pendentes) | BACKLOG:1682 |
| CASA-ESCALA-E4 | ESPINHA-E | Faz leitores concordarem sobre o vínculo que cobre a data. | - | E3/C3; ! E4-REGENERAR-VINCULO-SEM-VIGENCIA | ++....... | REGISTRADA (escritores esperam !) | BACKLOG:1683 |
| CASA-ESCALA-E5 | ESPINHA-E | Declara uma autoridade única para o vínculo do dia. | - | TOMADORES vazio; reparentar_folga | .+....... | REGISTRADA | BACKLOG:1684 |
| PAR-RELAMPAGO-PELO-TURNO | ESPINHA-E | Faz a detecção de pares rápidos consultar o turno oficial. | - | Dry e impacto antes | ++....... | REGISTRADA | BACKLOG:1685 |
| REABRIR-PERGUNTA-A-PREMISSA | ESPINHA-E | Evita reabrir cobrança encerrada sem fato novo. | - | Contador; conferência de sete dias sem ciclo | +++....++ | REGISTRADA (medição do portão pendente) | BACKLOG:1686 |
| TERMOMETRO-VIRA-LISTA | ESPINHA-E | Mostra suspeitas de escala na lista sem gerar ciclos de chamados. | - | REABRIR-PERGUNTA-A-PREMISSA; prova nominal | ++....... | REGISTRADA | BACKLOG:1687 |
| REABRIR-PREMISSA-DO-MARCO | ESPINHA-E | Verifica se a reabertura deve considerar dia ou marco. | - | E5; pergunta registrada | ++....... | PARADA (medida sem defeito; proposta descartada) | BACKLOG:1689 |
| ADMIN-RESOLVE-FLAP | ESPINHA-E | Mantém o chamado coerente com a última ação registrada. | - | Reprodução da validação com perguntas irmãs | +.~...... | REGISTRADA (outra linha informa cura) | BACKLOG:1694; BACKLOG:1697 |
| RAIA-DO-CONTRATO-20-NOITE | ESPINHA-E | Migra três rotinas para seus contratos oficiais. | - | CASA-CHAMADO-C2; JUIZ-PAR-COMPLETO; canal da batida | .+....... | REGISTRADA (agente citado apenas em 27/10) | BACKLOG:1707 |
| ORACULO-RECUSA-LINHA-SO-FOLGA | ESPINHA-E | Evita deixar colaboradores sem avaliação silenciosamente. | - | Medir os 79 sem dias de calculador | +.~....++ | REGISTRADA (parte publicada; 79 restantes) | BACKLOG:1708 |
| LOTE-VAZA-ENTRE-COLABS | ESPINHA-E | Faz o cálculo em lote igualar o cálculo individual. | - | Medir a causa | +........ | REGISTRADA | BACKLOG:1709 |
| CONTRATO-20-DOIS-EVENTOS | ESPINHA-E | Atualiza perguntas após batida ou regeneração mudar o dia. | - | Medida de 24 h; condição 2.1 | ++~....++ | REGISTRADA (E2 publicada; medição pendente) | BACKLOG:1714 |
| EMISSOR-COBRA-MARCO-DO-TURNO-VIZINHO | ESPINHA-E | Evita cobrar marcos pertencentes ao turno anterior. | - | EMISSOR-NAO-EMITIU | +........ | REGISTRADA | BACKLOG:1716 |
| PASSADO-SEM-MARCOS-SEM-TETO | ESPINHA-E | Verifica perguntas que nascem antes do fim real do trabalho. | - | Medir encerramento do fato | +........ | REGISTRADA | BACKLOG:1721 |
| LEGADO-51-ACESAS | ESPINHA-E | Encerra perguntas antigas cujo marco já está atendido. | - | Medição pelo juiz real | ++....... | REGISTRADA | BACKLOG:1724 |
| C5-FALTA-MAIOR-QUE-PREVISTO | ESPINHA-E | Investiga descontos de falta maiores que a jornada prevista. | - | Nova medição após pousos de 26–27/10 | ++....... | REGISTRADA | BACKLOG:1725 |
| DIA-DA-BATIDA-JUIZ-UNICO | ESPINHA-E | Faz decisões usarem o dia definido pelo juiz do turno. | - | Rejulgamento de col33; antes de fechar contrato 20 | .+....... | REGISTRADA | BACKLOG:1727 |
| PERGUNTA-DO-DIA-EM-CURSO | ESPINHA-E | Abre cobrança do marco vencido pelo evento e instante corretos. | - | Medição das 48 perguntas futuras | ++....... | REGISTRADA | BACKLOG:1729 |
| VINCULO-DE-HOJE-FORA-DO-CENSO | ESPINHA-E | Localiza leitores que escolhem vínculo sem consultar o juiz. | - | Censo dos sítios | ......... | REGISTRADA | BACKLOG:1739 |
| BUG-LASTRO-VAI-E-VOLTA | ESPINHA-E | Evita fechar e reabrir cobrança sem fato novo. | - | REABRIR-PERGUNTA-A-PREMISSA; LASTRO-DEGRAU-2 | ++....... | REGISTRADA (absorvida pela REABRIR) | BACKLOG:1756; BACKLOG:1686; BACKLOG:812 |
| BUG-LASTRO-CLASSE-2 | ESPINHA-E | Evita ciclos de fechamento e reabertura por outras rotinas. | - | BUG-LASTRO; REABRIR-PERGUNTA-A-PREMISSA | +........ | REGISTRADA (absorvida pela REABRIR) | BACKLOG:1757; BACKLOG:1686 |
| SETE-CRONS-SEM-LEI-ESCRITA | ESPINHA-E | Define o papel de sete rotinas automáticas. | - | Fatias E/C; BUG-LASTRO; LASTRO-DEGRAU-2 | ++....... | REGISTRADA | BACKLOG:1758 |
| LASTRO-DEGRAU-2 | ESPINHA-E | Faz o casamento de batida e marco consultar o juiz do turno. | - | CENSO-DE-LEITORES; pouso de JUIZ-DO-PAR | ++....... | INTERROMPIDA | BACKLOG:1759; BACKLOG:812; anexo:interrompidas.txt:23 |
| R1-ESTRUTURA-91 | ESPINHA-E | Corrige famílias de divergência entre cálculo e regra. | - | E4; E5; motor; classes cegas | .+......+ | REGISTRADA (restante pendente) | BACKLOG:1765 |
| V-FASE-FILA-DONO-DEPENDE-DA-JANELA | ESPINHA-E | Impede mudar o responsável pelo erro passado conforme a janela. | - | decisão de lei | ......... | PARADA (aguarda lei) | BACKLOG:1771 |
| V-FASE-DONO-AS-OF | ESPINHA-E | Classifica o dia pelo diagnóstico que efetivamente o cobriu. | - | último diagnóstico; gatilho de mudança | .+~...... | REGISTRADA (construtor antigo) | BACKLOG:1774 |
| F3-RECORTE-E-CAUDA | ESPINHA-E | Mede perguntas sob chamados encerrados e migra o leitor restante. | - | MODO-COMUM; item 5 | +........ | REGISTRADA (A publicada; B pendente) | BACKLOG:1779 |
| BATERIA-QUEIXAS-ADMIN | ESPINHA-E | Testa quatro queixas nas escalas canônicas pela regra. | - | casos; validação do DP; FIO MUDO | .+....... | REGISTRADA | BACKLOG:1809 |
| O169-ATA-VAZIA-FOLGA | ESPINHA-E | Prova o fechamento da cobrança em folga com ata vazia. | - | caso com mutante | ......... | REGISTRADA | BACKLOG:1822 |
| O169-CONTADOR-NAO-MEDIDO | ESPINHA-E | Expõe chamados vivos sem medição do fato. | - | contador; vigia; caso | ......... | REGISTRADA | BACKLOG:1823 |
| O169-LIMIAR-15-DUPLO | ESPINHA-E | Centraliza o limite usado para classificar agrupamentos. | - | juiz único da assinatura | ......... | REGISTRADA | BACKLOG:1824 |
| TIPO-NA-FONTE | ESPINHA-E | Entrega a ata aos leitores por acesso de significado explícito. | - | corte; E3; DESLIGADO; FERIADO; O168; HJ-090 | ++++.+... | PARADA | BACKLOG:1836; RELATO:112; RELATO:149 |
| C20-FIO-MUDO | ESPINHA-E | Gera eventos quando a cobrança nasce ou perde fundamento. | - | cinco atos de perda | .+~...... | REGISTRADA (construtor antigo) | BACKLOG:1844 |
| C20-FIO-MUDO-FILA-CODEX | ESPINHA-E | Centraliza o reapontamento e torna a reabertura atômica. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1845 |
| C20-FIO-MUDO-FILA-OPUS | ESPINHA-E | Corrige concorrência, ordem, travas e relógio do fio. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1846 |
| MOTOR-LE-O-TURNO | ESPINHA-E | Faz cálculo e pagamento usarem o mesmo turno oficial. | - | E2; E3; DIFF; autorização das exportadas | .+....... | REGISTRADA | BACKLOG:1879 |
| FIO-E0-V4-E1-ENTRADAS | ESPINHA-E | Faz as perguntas nascerem ligadas ao fato correto. | - | desenho; suite; sombra; opus | ++.+.+... | PARADA (último estado escrito) | BACKLOG:1888 |
| FIO-E0-V4-E1-FILA | ESPINHA-E | Corrige travas e leitores restantes do nascimento das perguntas. | - | achados f1–f4; ordem das travas | ..++..+++ | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:1889 |
| E1-FILA-V2-3-A-6 | ESPINHA-E | Corrige leituras, relógios e travas restantes das perguntas. | - | fila da E1 | ......... | REGISTRADA | BACKLOG:1894 |
| E1-FILA-OPUS6-FILA | ESPINHA-E | Apura riscos e custos restantes da revisão da E1. | - | medir acervo e custo | ......... | REGISTRADA | BACKLOG:1895 |
| E1-FILA-V2-2-TRAVA | ESPINHA-E | Trava os registros envolvidos antes de transferir perguntas. | - | resíduos A3/A4/B3/B4 | ..++..+++ | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:1896 |
| E1-FILA-V2-2-OUTRAS-PORTAS | ESPINHA-E | Alinha as outras entradas à mesma ordem de travas. | - | censo das portas | +........ | REGISTRADA | BACKLOG:1897 |
| E1-FILA-V2-2-RESIDUOS | ESPINHA-E | Esclarece documentos e mede o custo da transferência. | - | medição SQL | ......... | REGISTRADA | BACKLOG:1898 |
| RAIZ-DA-DISPUTA-F0F1 | ESPINHA-E | Define origem e destino únicos para as disputas. | - | E3; E4B c4; F2 | ++...+... | REGISTRADA | BACKLOG:1912 |
| FIO-E0-V4-E4-ESCRITORES | ESPINHA-E | Substitui estados paralelos por atos registrados da pergunta. | - | E4B; E4C; E4D | ++.+.++++ | REGISTRADA (E4a-1 no ar; restante pendente) | BACKLOG:1915 |
| FIO-E0-V4-E4-RETIRADA-FISICA | ESPINHA-E | Retira campos antigos depois de zerar seus leitores. | - | série E fechada; zero leitores | ++....... | REGISTRADA | BACKLOG:1916 |
| FIO-E0-V4-FATO-A03 | ESPINHA-E | Registra a reabertura administrativa como fato próprio. | - | E4D | .+....... | REGISTRADA | BACKLOG:1917 |
| FIO-E0-V4-FATO-A05 | ESPINHA-E | Registra a assinatura como fato próprio da pergunta. | - | E4D | .+....... | REGISTRADA | BACKLOG:1918 |
| FIO-E0-V4-FATO-A06 | ESPINHA-E | Registra identidade e duplicação como fatos próprios. | - | E4D | .+....... | REGISTRADA | BACKLOG:1919 |
| FIO-E0-V4-FATO-A17 | ESPINHA-E | Registra a pendência técnica sem depender do estado antigo. | - | E4D | .+....... | REGISTRADA | BACKLOG:1920 |
| FIO-E0-V4-FATO-A18 | ESPINHA-E | Registra a recusa técnica sem depender do estado antigo. | - | E4D | .+....... | REGISTRADA | BACKLOG:1921 |
| FIO-E0-V4-FATO-A20 | ESPINHA-E | Registra a conferência de data com significado explícito. | - | E4D; decisão sobre mudança dos dias | .+....... | REGISTRADA | BACKLOG:1922 |
| FIO-E0-V4-E4A-2 | ESPINHA-E | Faz a fábrica agir por eventos e expõe dias sem perguntas. | - | O168/O169/LASTRO; fila de revisão | .++..++++ | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:1923 |
| BO-DISPUTA-SEM-PERGUNTA | ESPINHA-E | Impede nascer disputa sem pergunta para responder. | - | RAIZ F1/F2; BO-0611-05 | ++....... | REGISTRADA (estacionada) | BACKLOG:1928; BACKLOG:1912 |
| BO-DISPUTA-MATRIZ-ANTES-DO-OPUS | ESPINHA-E | Confere os caminhos de nascimento antes da revisão final. | - | matriz; BO-DISPUTA-SEM-PERGUNTA | ~+....... | REGISTRADA | BACKLOG:1929 |
| BUG-PAR-FROTA-1110 | ESPINHA-E | Trata pausas com uma metade validada e outra pendente. | - | BUG-PAR; RAIZ F3/F4 | +........ | REGISTRADA (absorvida) | BACKLOG:1933 |
| RAIZ-DA-DISPUTA | ESPINHA-E | Faz perguntas e disputas reagirem juntas aos mesmos atos. | - | F0–F7; decisões F1; E4D em F5 | ++....... | REGISTRADA | BACKLOG:1934 |
| F4-PAR-PELA-CELULA | ESPINHA-E | Usa os marcos da célula para identificar o par da pausa. | - | RAIZ F3/F4; decisão D8 | ++....... | REGISTRADA | BACKLOG:1946 |
| DISPUTA-ZUMBI-1110 | ESPINHA-E | Faz a disputa encerrar pelo mesmo juiz em todos os caminhos. | - | C7; RAIZ F3/F4 | ++....... | REGISTRADA | BACKLOG:1952 |
| PERGUNTA-DUPLICATA-1110 | ESPINHA-E | Apura perguntas repetidas e seus efeitos nos contadores. | - | E4D; medir idade e efeito | +........ | REGISTRADA | BACKLOG:1953 |
| DATA-EVENTO-CRU | ESPINHA-E | Identifica leituras de data fora do juiz do turno. | - | construir catraca com o censo | ++...+... | REGISTRADA | BACKLOG:1962 |
| LAVRA-PREMATURA-CONCORDE | ESPINHA-E | Define quando o dia pode ser confirmado antes do turno terminar. | - | decisão de desenho | ...+.+... | PARADA (construção vedada) | BACKLOG:1981 |
| FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE | ESPINHA-E | Apura perguntas contadas pelo vigia sem emissão. | - | conferir cobertura C20 | +........ | REGISTRADA | BACKLOG:1986 |
| E4A2-RELOGIO-FIO-VIVO | ESPINHA-E | Faz leitores de pergunta viva receberem o mesmo relógio. | - | injeção do relógio nos leitores C6 | ......... | REGISTRADA | BACKLOG:1989 |
| O169-JUIZES-760 | ESPINHA-E | Evita aceitar encerramento sem prova de aplicação. | - | medir frota | ......... | REGISTRADA | BACKLOG:1991 |
| O169-NAO-MEDIDO-ESCAPA | ESPINHA-E | Mostra encerramentos que o contador não conseguiu avaliar. | - | ? (precedência não indicada) | .+....... | REGISTRADA | BACKLOG:1997 |
| O169-V5-GRAVAR-ESTADO-SEM-TRILHA | ESPINHA-E | Deixa histórico de cada mudança de estado do chamado. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2002 |
| E4A2-JANELA-POR-EMPRESA | ESPINHA-E | Considera o fechamento próprio de cada empresa. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2007 |
| E4A2-LIMBO-EXPORTADO-COMO-ALVO | ESPINHA-E | Evita cobrar como falha o que foi encaminhado ao DP. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2008 |
| O169-FECHADA | ESPINHA-E | Impede encerrar por idade e trata o legado. | - | E4a-2; corte de desenho | ......... | PARADA (B3 desenho) | BACKLOG:2010 |
| PASSADO-SEM-TURNO-SEM-PAUTA-NAO-CONTADO | ESPINHA-E | Conta dias passados sem pergunta nem pauta. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2014 |
| PLANTIO-DIA-CIVIL-VIZINHO | ESPINHA-E | Usa o dia do turno no lugar da data civil. | - | desenho da obra mãe | ......... | REGISTRADA | BACKLOG:2032 |
| RETRATO-POR-EVENTO | ESPINHA-E | Atualiza o retrato quando um ato altera a apuração. | - | E2; E3 | +........ | REGISTRADA | BACKLOG:2039 |
| F3-BECO-1-VOLTA-3 | ESPINHA-E | Rejulga o mês corrigido e reata perguntas ao pai na fila. | - | fila do opus itens 1–8 | .+++.+++. | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:2050 |
| F3-BECO-2-COBRANCA-MORTA | ESPINHA-E | Trata dias sem caminho por cobrança morta. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2051 |
| JV1-MERGE-FILA | ESPINHA-E | Trata lacunas de janela e alimentação dos feriados. | - | medir dinheiro antes | ......... | REGISTRADA | BACKLOG:2061 |
| E4-8-FORA-VINCULO | ESPINHA-E | Identifica dias cobrados fora da vigência do vínculo. | - | medir frota | .+....... | REGISTRADA | BACKLOG:2066 |
| E4-8-COMP-ANTIGA | ESPINHA-E | Mede certificação de competências antigas antes do uso. | - | medir competências 06–09 | ......... | REGISTRADA | BACKLOG:2067 |
| O261-E-F3-DORMENTE | ESPINHA-E | Dá caminho a chamados dormentes sem encerramento indevido. | - | matriz de becos; classificação de dono | ......... | REGISTRADA | BACKLOG:2074 |
| ACESA-ORFA-PROXIMIDADE | ESPINHA-E | Trata a órfã usada também para acender lâmpada. | - | TIPO item 3 | .+....... | REGISTRADA | BACKLOG:2094 |

## 20/20 (25)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| DISPARAR-PERGUNTAS-NO-CONTRATO-20 | 20/20 | Faz o disparo de perguntas seguir a célula e o caminho devido. | - | E3-A; PAR v5; fio mudo; C11 | ++....... | REGISTRADA (censo fechado; obra aberta) | BACKLOG:814; BACKLOG:2076 |
| O168 | 20/20 | Preserva a cobrança enquanto uma resposta aguarda julgamento. | - | Guarda no evento antes de retirar o executor do supra | +++..++++ | NO AR hoje | BACKLOG:1422; BACKLOG:2076; app/docs/RELATO.md:5; app/docs/RELATO.md:7; app/docs/RELATO.md:31 |
| O168-FILA-OPUS | 20/20 | Corrige oito problemas restantes na proteção das conversas. | - | Itens 3 e 4 antes do executor; item 5 no marco seguinte | ++....... | REGISTRADA | BACKLOG:1423; BACKLOG:2076 |
| O169 | 20/20 | Impede encerrar cobranças apenas porque envelheceram. | - | ? (célula antiga conserva pergunta já superada) | +........ | REGISTRADA (pousada em outra fonte) | BACKLOG:1424; BACKLOG:1821; BACKLOG:2076 |
| LASTRO-FOLGA-FORA-DO-CATALOGO | 20/20 | Evita tratar tipo inválido de ausência como atestado. | - | medir escritor e frota | ......... | REGISTRADA | BACKLOG:1835; BACKLOG:1840 |
| FIO-DUP | 20/20 | Corrige os caminhos de duplicação do fio no cartório. | - | pouso O168 | .+....... | REGISTRADA | BACKLOG:1839 |
| LASTRO-FAIXA-SEM-DEFAULT | 20/20 | Recusa tipos sem faixa definida em vez de escolher silenciosamente. | - | TIPO-NA-FONTE; pouso antes da relavra | .++...... | REGISTRADA | BACKLOG:1840 |
| LEI-FIO-MUDO-B | 20/20 | Preserva o caminho da cobrança sem reabrir continentes automaticamente. | - | fechamento pela E3 do FIO-E0-V4 | ++++.++.. | PARADA (aval manda não pousar) | BACKLOG:1847 |
| FIO-E0-V4-E3-LEITORES | 20/20 | Faz telas e contadores mostrarem a mesma tarefa diária. | - | cura; preverif; suite; sombra; opus | ++++.~... | EM VOO | BACKLOG:1902; RELATO:6; RELATO:33 |
| E3-MATRIZ-DA-RESPOSTA | 20/20 | Documenta e protege as entradas que gravam respostas. | - | portas Q1/Q2; E3 | ++....... | REGISTRADA | BACKLOG:1903 |
| E3-X1-FOLGA-NAO-E-SEM-COBERTURA | 20/20 | Distingue folga de falta de cobertura no julgamento. | - | c8b | ++....... | REGISTRADA | BACKLOG:1906 |
| E3-V15-A-S | 20/20 | Encaminha fatos sem emissor ou responsável ao administrador. | - | c8b; preservar C6 | ++....... | REGISTRADA | BACKLOG:1907 |
| E3-CADASTRO-ADMIN | 20/20 | Entrega cadastros que precisam de decisão humana ao administrador. | - | correção administrativa do cadastro | ++....... | REGISTRADA | BACKLOG:1908 |
| E3-X2-NUNCA-JULGADAS | 20/20 | Identifica células que ainda não receberam julgamento. | - | possível comboio com X1 | +........ | REGISTRADA | BACKLOG:1909 |
| E3-COL962-C6-X-LAMPADA | 20/20 | Apura discordância entre pergunta viva e marcação do dia. | - | medir divergência | ......... | REGISTRADA | BACKLOG:1910 |
| FIO-E0-V4-E4B | 20/20 | Repara perguntas antigas pela entrada normal das correções. | - | itens 18–19; autorização das exportadas | ++++.+ ب++ | REGISTRADA (pouso antigo; partes finais pendentes) | BACKLOG:1924 |
| E4B-C12-DIFF-DE-PAUTA | 20/20 | Confere se a correção cria somente pautas permitidas. | - | comparação c12 com base | .+....... | REGISTRADA | BACKLOG:1925 |
| E3-10-CURA-NO-ESCRITOR | 20/20 | Corrige na origem respostas carimbadas sem texto. | - | censo do escritor; prova no aplicativo | ~+....... | REGISTRADA | BACKLOG:1941 |
| O168-GUARDA-NO-GATE-DO-PAI | 20/20 | Segura o chamado enquanto houver resposta sem julgamento. | - | - | +++++++++ | NO AR hoje | BACKLOG:1942; RELATO:5; RELATO:7; RELATO:31; RELATO:173 |
| E3-FORMA-SQL-DO-JUIZ | 20/20 | Faz consultas ao banco usarem a regra única de resposta. | - | cura da porta; suite; DIFF; opus | ++++.~... | EM VOO | BACKLOG:1943; RELATO:6; RELATO:14; RELATO:38 |
| E3-IMPACTO-E-CONTRATOS-DE-ENTRADA | 20/20 | Confere mudanças de ação e memória antes de publicar E3. | - | A1–A3 antes do opus | ~.~..~... | EM VOO | BACKLOG:1944; RELATO:38 |
| BO-0611-07-E4B-COBRE | 20/20 | Confere se o reparo antigo cobre o problema relatado. | caso 07 | DRY da E4B | ......... | REGISTRADA | BACKLOG:1979 |
| E4A1-C6-X-PASSADO-SEM-TURNO | 20/20 | Corrige o descarimbo em dias passados sem turno. | - | CONTRACT E4B/E4C | +........ | REGISTRADA | BACKLOG:1988 |
| E3-ENTRADA-ORFA-SEM-TURNO | 20/20 | Define o tratamento da entrada real sem turno previsto. | - | lei para dia sem turno com batida | ......... | PARADA (aguarda lei) | BACKLOG:2016 |
| C20-O168 | 20/20 | Segura o chamado pai enquanto houver resposta sem julgamento. | - | - | +++++++++ | NO AR hoje | BACKLOG:2049; RELATO:5; RELATO:7; RELATO:31; RELATO:173 |

## LATERAL-ESTRUTURA (352)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| FERIADO-ESCALA | LATERAL-ESTRUTURA | Corrige o trabalho em feriados conforme a regra vigente. | HJ-074 | FERIADO-USA-A-LEI-DE-1609 | +++~.~... | EM VOO (escopo anterior superado) | BACKLOG:728; app/docs/RELATO.md:134 |
| FERIADO-1609-FILA | LATERAL-ESTRUTURA | Corrige falhas restantes do tratamento de feriados. | HJ-074 | pouso FERIADO | .+....... | REGISTRADA | BACKLOG:729 |
| LEI-DESLIGADO-NAO-TRAVA | LATERAL-ESTRUTURA | Impede que pendências de desligados bloqueiem a folha. | - | ? (não indicada) | ++++++~++ | NO AR hoje | BACKLOG:730; app/docs/RELATO.md:54 |
| DESLIGAR-LIQUIDA-TUDO | LATERAL-ESTRUTURA | Encerra cobranças do desligado e encaminha respostas ao DP. | - | LEI-DESLIGADO-NAO-TRAVA; O50 | ++....... | REGISTRADA (censo entregue) | BACKLOG:731; app/docs/RELATO.md:125 |
| PARIDADE-ABSOLVIDAS-168 | LATERAL-ESTRUTURA | Devolve ao admin respostas de intervalo encerradas sem julgamento. | - | BUG-PAR-DESCONGELA; condições do apply | ++~+.~... | EM VOO (103 incorporadas ao BUG-PAR) | BACKLOG:733; app/docs/RELATO.md:95; app/docs/RELATO.md:35 |
| RAIOX-DIAS-DERIVA | LATERAL-ESTRUTURA | Faz o Raio-X mostrar os dias apontados pela autoridade central. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:734 |
| SITUACAO-LEITOR-CRU | LATERAL-ESTRUTURA | Unifica como o sistema reconhece um desligamento. | - | O50 | .+....... | REGISTRADA | BACKLOG:735 |
| RETRATO-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para montar o retrato da folha. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:737 |
| HJ-092 | LATERAL-ESTRUTURA | Encaminha ao admin as batidas que exigem decisão humana. | HJ-092 | decisão do admin | ++....... | REGISTRADA (fechada como BATIDA; deploy não comprovado) | BACKLOG:741 |
| HJ-093 | LATERAL-ESTRUTURA | Corrige horas zeradas apesar de justificativa aprovada. | HJ-093 | JUSTIFICATIVA-PAGA-O-PERIODO; HJ-090; admin | ++....... | REGISTRADA (classe confirmada) | BACKLOG:743 |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | LATERAL-ESTRUTURA | Impede que lançar ausência transforme um desligado em afastado. | - | corte para ampliar o escopo | .+....... | REGISTRADA | BACKLOG:745 |
| FERIADO-RUBRICA-DO-FLAG-CORRENTE | LATERAL-ESTRUTURA | Evita que a escolha atual de feriado altere a rubrica histórica. | HJ-074 | impacto_g; decisão de desenho | ......... | REGISTRADA (desenho candidato) | BACKLOG:746 |
| PILHA-DA-BATIDA-RECALCULO-INLINE | LATERAL-ESTRUTURA | Reduz o cálculo feito durante o registro da batida. | - | medir latência; O-OUTBOX | +........ | REGISTRADA | BACKLOG:753 |
| O-BITEMPORAL-2 | LATERAL-ESTRUTURA | Preserva quando vínculos e regras valeram e foram registrados. | - | contratos_estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:758 |
| IA-DOMINIO-POR-FERRAMENTA | LATERAL-ESTRUTURA | Limita respostas da mensageria aos dados permitidos ao usuário. | - | 20/20 | .+....... | REGISTRADA | BACKLOG:759 |
| REJULGAR-COMP10-4-REVERTIDOS | LATERAL-ESTRUTURA | Reaplica correções revertidas com prova do que muda. | - | medida atual; SOMBRA-X-PROD-COL988-124639 | .+.....+. | REGISTRADA (aplicação feita; divergência restante) | BACKLOG:766 |
| TERMOMETRO-GRAVA-SEM-TRILHA | LATERAL-ESTRUTURA | Registra quem alterou o escore do vínculo e o motivo. | - | push junto do FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:768 |
| TROCA-DE-POSTO-SEM-VINCULO | LATERAL-ESTRUTURA | Mantém coerentes o posto do cadastro e o vínculo vigente. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:771 |
| LAVRAR-HE-PENDENTE-ESCALA | LATERAL-ESTRUTURA | Acelera a apuração de horas extras reutilizando dados carregados. | - | resíduos de leitores; decisão sobre segunda apuração | ..++.+.+~ | REGISTRADA (núcleo no ar; resíduos pendentes) | BACKLOG:773 |
| N1-ALIMENTACAO-UNICA | LATERAL-ESTRUTURA | Evita carregar repetidamente os mesmos dados das portas. | - | N1; interface E3 se ampliada | +........ | REGISTRADA (não aplicável ao corte atual da E3) | BACKLOG:774; app/docs/RELATO.md:71 |
| BO-ADMIN-2710 | LATERAL-ESTRUTURA | Classifica problemas do admin e os encaminha às obras responsáveis. | - | medidas; dono e destino por item | .+....... | REGISTRADA (medição histórica) | BACKLOG:777 |
| TRILHA-DO-VINCULO-MENTE | LATERAL-ESTRUTURA | Faz o histórico do vínculo registrar corretamente as mudanças. | - | residual RT-03; push FIO | ++...+.+~ | REGISTRADA (núcleo no ar; RT-03 pendente) | BACKLOG:778 |
| REGISTRADO-NAO-E-VIVO | LATERAL-ESTRUTURA | Evita mostrar auditoria como pendência de ação. | - | instrumento de fiscalização | .+.....+~ | REGISTRADA (produto no ar; instrumento pendente) | BACKLOG:780 |
| SILENCIO-VENCIDO-VOLTA | LATERAL-ESTRUTURA | Faz cobranças reaparecerem quando termina seu silêncio. | - | QUARENTENA-DO-COLAB; E3 | .++...... | INTERROMPIDA | BACKLOG:783; anexo:interrompidas.txt:1 |
| QUARENTENA-DO-COLAB | LATERAL-ESTRUTURA | Isola cobranças por período e permite seu retorno. | - | reaplicar SPEC; DIFF; E3 | .+++..~.. | INTERROMPIDA | BACKLOG:784; anexo:interrompidas.txt:3 |
| ATA-DEFASADA-COMP09 | LATERAL-ESTRUTURA | Distingue atas antigas de mudanças causadas pela correção. | - | COBPARC-DIFF-ISOLADO | ++....~.. | PARADA (itens devolvidos ao PAREI) | BACKLOG:787 |
| COBPARC-ATA-DEFASADA-09 | LATERAL-ESTRUTURA | Trata divergências estruturais restantes da competência exportada. | - | nova medida; Pauta DP | ++....... | REGISTRADA | BACKLOG:791 |
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | LATERAL-ESTRUTURA | Preserva o chamado correto na conciliação de duplicados. | - | medir ocorrência | .+....... | REGISTRADA | BACKLOG:797 |
| REJULGAR-61-COBPARC | LATERAL-ESTRUTURA | Reavalia atas antigas de ausência parcial pela regra atual. | - | autorização das 55 atas exportadas | .+....... | PARADA (aguarda autorização) | BACKLOG:801 |
| CXR-MEDIDO-EM-AUTO-NOW | LATERAL-ESTRUTURA | Distingue a hora da medição da hora de gravação. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:807 |
| CXR-BECO-SEM-BATIDA-FORA-DA-LAVRA | LATERAL-ESTRUTURA | Inclui colaboradores sem batidas na medição dos dias sem caminho. | - | medir universo faltante | ......... | REGISTRADA | BACKLOG:808 |
| TETO-NA-LISTA-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para medir e publicar o teto. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:809 |
| TRAVA-COLAB-X-DISPUTA | LATERAL-ESTRUTURA | Investiga possível conflito entre travas de colaborador e disputa. | - | reproduzir com transação externa | ......... | REGISTRADA (hipótese não reproduzida) | BACKLOG:811 |
| JUSTIFICATIVA-ADMIN-FORA-DA-PORTA | LATERAL-ESTRUTURA | Impede aprovações sem efeitos e histórico completos. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:815 |
| REATAR-ESCREVE-EM-DIA-TRANCADO | LATERAL-ESTRUTURA | Evita alterar perguntas de dias protegidos pela tranca. | - | medir na sombra | .+....... | REGISTRADA | BACKLOG:817 |
| PARCIAL-NA-PORTA | LATERAL-ESTRUTURA | Impede cobrança indevida de ausência parcial no nascimento da pergunta. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA | BACKLOG:857 |
| COBPARC-ROTULO-FILA-V2 | LATERAL-ESTRUTURA | Corrige rótulos e contagens de marcos dispensados. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:859 |
| COBPARC-FILA-RELOGIO | LATERAL-ESTRUTURA | Faz cada ato usar um único instante para reconhecer fato novo. | - | resposta do padrão; mutantes; suíte | .++...... | INTERROMPIDA | BACKLOG:861; anexo:interrompidas.txt:7 |
| COBPARC-FILA-CENSO-V7 | LATERAL-ESTRUTURA | Corrige leitores restantes de ausências parciais. | - | COBERTURA-PARCIAL-UM-JUIZ; autorização do dinheiro | ......... | REGISTRADA | BACKLOG:863 |
| COBPARC-FILA-V8-PARCIAL-DEMISSAO | LATERAL-ESTRUTURA | Confere ausência parcial no dia da demissão sem batida. | - | cartório após COBPARC | ......... | REGISTRADA | BACKLOG:864 |
| COBPARC-FILA-V8-VOCABULARIO | LATERAL-ESTRUTURA | Evita confundir tipo de ausência com justificativa aprovada. | - | COBERTURA-PARCIAL-UM-JUIZ | ......... | REGISTRADA | BACKLOG:865 |
| COBPARC-FILA-V8-PARCIAL-JUSTIF | LATERAL-ESTRUTURA | Corrige a cobrança restante quando parcial e justificativa coincidem. | - | COBPARC-FILA-RELOGIO; vaga | ......... | PARADA (construtor estacionado; cópia vazia) | BACKLOG:866 |
| COBPARC-FILA-H10-PARCIAIS-09 | LATERAL-ESTRUTURA | Trata parciais antigas sem alterar silenciosamente competência entregue. | - | fato novo ou REGEN | .+....... | REGISTRADA | BACKLOG:867 |
| COBPARC-FILA-H10-COL926 | LATERAL-ESTRUTURA | Encaminha ao DP o caso separado do pouso da ausência parcial. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:868 |
| V-FASE-AS-OF-TRANSFERIDO | LATERAL-ESTRUTURA | Usa a empresa correta do dia ao conferir colaborador transferido. | - | medir transferidos | ......... | REGISTRADA | BACKLOG:900 |
| V-FASE-AS-OF-PARIDADE-UNIVERSO | LATERAL-ESTRUTURA | Faz a comparação usar o mesmo conjunto de dias e folgas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:901 |
| V-FASE-AS-OF-CUSTO | LATERAL-ESTRUTURA | Mede o custo de buscar decisões anteriores quando falta cobertura. | - | medição na sombra | ......... | REGISTRADA | BACKLOG:902 |
| DEGRAU-HE-100-CADASTRO | LATERAL-ESTRUTURA | Torna configurável pela convenção o limite da hora extra integral. | - | cadastro; leitor na UI | .+....... | REGISTRADA | BACKLOG:904 |
| V-FASE-AS-OF-SEM-CELULA | LATERAL-ESTRUTURA | Reabre a conferência após mudar dia passado sem célula. | - | lei SEM-CELULA | .+....... | REGISTRADA | BACKLOG:905 |
| LENVIVOS-SOBRAS-ESMERIL | LATERAL-ESTRUTURA | Trata resíduos de teste, alarme e cobertura da cobrança. | - | ESMERIL-V2 | .+....... | REGISTRADA | BACKLOG:908 |
| LENVIVOS-EMITIR-INERTE | LATERAL-ESTRUTURA | Remove ou corrige parâmetro sem efeito na emissão. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:909 |
| LENVIVOS-CHAVE-DO-DIA | LATERAL-ESTRUTURA | Faz emissor e cartório associarem a cobrança ao mesmo dia. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:910 |
| LENVIVOS-COBRANCAS-VIVAS | LATERAL-ESTRUTURA | Confere o filtro aplicado às cobranças vivas. | - | conferir antes da cura | +........ | REGISTRADA (candidata em conferência) | BACKLOG:911 |
| V-FASE-AS-OF-ATRASO | LATERAL-ESTRUTURA | Reduz atraso entre a mudança do dia e a conferência da fase. | - | próxima lavra; dependências do cron | ......... | REGISTRADA | BACKLOG:916 |
| V-FASE-AS-OF-UNIVERSO | LATERAL-ESTRUTURA | Reconcilia decisões antigas ao sair da escala 12x36. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:917 |
| V-FASE-AS-OF-SOBREPOSTA | LATERAL-ESTRUTURA | Trata janelas sobrepostas após mudar o início da competência. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:918 |
| V-FASE-AS-OF-INTRADIA | LATERAL-ESTRUTURA | Preserva versões anteriores de decisões do mesmo dia. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:919 |
| PROPOSITOR-JANELA-LONGA | LATERAL-ESTRUTURA | Mantém propostas coerentes em consultas com mais de duas janelas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:920 |
| FILA-POR-CAUSA-AS-OF | LATERAL-ESTRUTURA | Faz a fila passada ler a decisão daquela data. | - | pouso F1/F5 | ......... | REGISTRADA | BACKLOG:921 |
| EMIS-PAUTA-DEDUP | LATERAL-ESTRUTURA | Evita duplicar a pauta do DP em julgamentos concorrentes. | - | nova medição na sombra | ......... | REGISTRADA | BACKLOG:931 |
| GRADE-09-PAUTA | LATERAL-ESTRUTURA | Entrega ao DP a diferença entre pago e devido sem regravar a folha. | - | GRADE-v5; ensaio; diferença medida | ......... | REGISTRADA | BACKLOG:940 |
| ISENTO-H10-MENORES | LATERAL-ESTRUTURA | Trata leitores, testes e decisões restantes da regra de isenção. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:945 |
| NOREVERSE-UI-NO-CORE | LATERAL-ESTRUTURA | Impede links de tela de derrubarem o processamento do núcleo. | - | resultado 6 após a corrente | ++++++++~ | REGISTRADA (cura no ar; resultado posterior pendente) | BACKLOG:953; app/docs/RELATO.md:1810 |
| ISENTO-FLAG-ATEMPORAL | LATERAL-ESTRUTURA | Define o efeito da isenção atual sobre decisões antigas. | - | E3-ISENTO-INSUMOS | ......... | REGISTRADA (desenho futuro) | BACKLOG:956 |
| V3-P5-PRORROGACAO-POR-INTERVALO | LATERAL-ESTRUTURA | Aplica ao intervalo a mesma prorrogação noturna do turno. | - | cópia do main; certificação; impacto | .++...~.. | REGISTRADA (corpo aprovado; certificação pendente) | BACKLOG:967 |
| V3-HE-NOTURNA-POR-PECA | LATERAL-ESTRUTURA | Conta a prorrogação noturna também na hora extra. | - | V3-P5 | ++....... | REGISTRADA (impacto medido zero) | BACKLOG:968 |
| EMIS-FALSE-ATA-TRABALHO | LATERAL-ESTRUTURA | Elimina a contradição entre folga da célula e trabalho da ata. | - | censo próprio | ......... | REGISTRADA | BACKLOG:969 |
| GRADE-FOLGA-C5 | LATERAL-ESTRUTURA | Confere folgas de calendário dentro do vínculo válido. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:970 |
| GRADE-FOLGA-GERADOR | LATERAL-ESTRUTURA | Faz a nova escala respeitar a folga pessoal lançada. | - | juiz da folga; ensaio; autorização do passado | .+....... | REGISTRADA | BACKLOG:971 |
| V3-MOTOR-FERIADO-HE100-NOTURNA | LATERAL-ESTRUTURA | Reconhece o adicional noturno das horas extras de feriado. | - | autorização do motor | ......... | REGISTRADA | BACKLOG:972 |
| COBPARC-B-CAUSA-NO-MOTOR | LATERAL-ESTRUTURA | Leva ao admin ausências parciais sem período definido. | - | autorização do motor | .+....... | PARADA (aguarda autorização) | BACKLOG:973 |
| V3-NOTURNO-RECORTE-LONGO | LATERAL-ESTRUTURA | Evita erros e duplicação de horas em jornadas noturnas longas. | - | autorização do motor | ......... | PARADA (aguarda autorização) | BACKLOG:974 |
| REVERTER-REGEN-TRIESTADO | LATERAL-ESTRUTURA | Preserva o dia indefinido ao desfazer a geração da escala. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:976 |
| P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO | LATERAL-ESTRUTURA | Exige motivo escrito para confirmar resposta improvável. | - | fatia em curso; exame das permissões | .+....... | REGISTRADA | BACKLOG:985 |
| F5-RELOGIO-DO-EXPORT | LATERAL-ESTRUTURA | Usa o mesmo instante em toda a conferência da folha. | - | alteração no juiz da exportação | ......... | REGISTRADA | BACKLOG:987 |
| PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA | LATERAL-ESTRUTURA | Evita alarmes de erro para divergências já previstas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:989 |
| O261-V-FASE-NA-LISTA | LATERAL-ESTRUTURA | Mostra ao admin dias de escala com fase deslocada. | - | VETO A; nova construção | ++....~.. | REGISTRADA (cópia reprovada) | BACKLOG:999 |
| JUIZ-UNICO-DA-ANCORA | LATERAL-ESTRUTURA | Usa referência única para a fase da escala 12x36. | - | V-FASE | .+....... | REGISTRADA | BACKLOG:1000 |
| FOLGA-TRABALHADA-RESOLVE | LATERAL-ESTRUTURA | Permite ao admin resolver uma folga trabalhada em um clique. | - | juiz do botão; batidas em quantidade par | .+....... | REGISTRADA | BACKLOG:1002 |
| BO-[nome]-DESTRAVA | LATERAL-ESTRUTURA | Indica a correção de cadastro que libera o dia na folha. | - | lavrador do retrato; dono do dia | .+....... | REGISTRADA | BACKLOG:1003 |
| FIO-DO-ADMIN-LAVRA-VELHA | LATERAL-ESTRUTURA | Alerta quando a lista do admin usa medição antiga. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1007 |
| AVISO-ATIVO-SEM-VINCULO-NO-DIA | LATERAL-ESTRUTURA | Avisa quando falta vínculo para dia de colaborador ativo. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1011 |
| ANALISAR-RELE-O-BANCO | LATERAL-ESTRUTURA | Impede análise de reativar chamado encerrado por outro ato. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1014 |
| REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO | LATERAL-ESTRUTURA | Evita anunciar reabertura quando o chamado continuou encerrado. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1015 |
| DIAS-ENCERRADOS-CHAVE-DO-CARTORIO | LATERAL-ESTRUTURA | Entrega células com a chave esperada pelo juiz de turnos. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1018 |
| VETO-NA-PORTA-POR-ELIMINACAO | LATERAL-ESTRUTURA | Distingue veto de absolvição ao explicar chamado não criado. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1020 |
| CXR-FALHA-CALADA | LATERAL-ESTRUTURA | Mostra falhas de leitura em vez de lista vazia. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1022 |
| FASE-12X36-SEM-PAPEL-DA-ATA | LATERAL-ESTRUTURA | Confere a fase usando turnos reconhecidos pela ata. | - | juiz dos turnos | ......... | REGISTRADA | BACKLOG:1023 |
| PLANTIO-ENVELOPE-PELO-GERADOR | LATERAL-ESTRUTURA | Usa horários congelados do dia para orientar a batida gerada. | - | fonte da célula | .+....... | REGISTRADA | BACKLOG:1024 |
| COL309-SETEMBRO | LATERAL-ESTRUTURA | Corrige células antigas usando o vínculo autorizado. | - | COL309-09-RECALCULO-DEPOIS-DO-ADMIN | .+.....+. | REGISTRADA (células aplicadas; recálculo separado) | BACKLOG:1027 |
| COL309-09-RECALCULO-DEPOIS-DO-ADMIN | LATERAL-ESTRUTURA | Recalcula setembro após o admin decidir quatro dias pendentes. | - | quatro decisões lavradas do admin | .+....... | PARADA (aguarda admin) | BACKLOG:1028 |
| CAL-PREDICADO-D-MENOR-HOJE-2-COPIAS | LATERAL-ESTRUTURA | Unifica o critério de datas do calendário e da folha. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1031 |
| CAL-CUSTO-NO-MAPA-DE-FUROS | LATERAL-ESTRUTURA | Mede e reduz consultas repetidas no mapa de furos. | - | medição do custo | ......... | REGISTRADA | BACKLOG:1032 |
| CAL-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única leitura do relógio no calendário. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1033 |
| ENTRADA-BLOQUEADA-SEM-DATA-TURNO | LATERAL-ESTRUTURA | Registra o dia do turno no chamado de entrada bloqueada. | - | JUSTIFICATIVA-ENCERRA-ORIGEM | ......... | REGISTRADA | BACKLOG:1048 |
| POSICAO-TOTAL-ATE-A-DATA | LATERAL-ESTRUTURA | Soma o espelho até a data escolhida por uma única fonte. | - | fonte do total até a data | .+....... | REGISTRADA | BACKLOG:1049 |
| POSICAO-FALTAS-E-AUSENCIAS | LATERAL-ESTRUTURA | Reúne faltas e ausências com seus tipos. | - | fonte unificada | .+....... | REGISTRADA | BACKLOG:1050 |
| POSICAO-FERIAS-ATE-A-DATA | LATERAL-ESTRUTURA | Apura férias proporcionais até a data escolhida. | - | regra de proporcionais e avos | .+....... | REGISTRADA | BACKLOG:1051 |
| POSICAO-PORTA-POR-MOTIVO | LATERAL-ESTRUTURA | Oferece o acesso correto para resolver cada impedimento. | - | fonte de URL e pergunta | .+....... | REGISTRADA | BACKLOG:1052 |
| VIGENCIA-FUTURA | LATERAL-ESTRUTURA | Agenda mudanças de posto ou escala para uma data futura. | caso 09 | vínculo do dia do grupo 1 | .+....... | REGISTRADA | BACKLOG:1055 |
| BO2910-ESTRUTURA | LATERAL-ESTRUTURA | Corrige classes de cadastro, ausência e julgamento da triagem. | casos 01, 02, 05, 06, 08 | obras por classe | .+....... | REGISTRADA | BACKLOG:1059 |
| PORTAO-DE-FATURAMENTO | LATERAL-ESTRUTURA | Dá ao admin uma porta para autorizar turno excepcional. | caso 07 | PORTA-CELULA-EDITADA; JORNADA-EXCEPCIONAL-POR-DATA | .+....... | REGISTRADA | BACKLOG:1061 |
| [nome]-NOTURNO-DIA-CIVIL | LATERAL-ESTRUTURA | Identifica perguntas noturnas ligadas ao dia errado. | - | medição da frota; admin para retratação | .+....... | REGISTRADA | BACKLOG:1064 |
| GRADE-DEPENDE-DA-JANELA | LATERAL-ESTRUTURA | Faz o mesmo dia produzir a mesma grade em qualquer janela. | - | LASTRO; grupo 1, sítio 1 | .+....... | REGISTRADA | BACKLOG:1068 |
| DETECTOR-SO-A-1A-PAUSA | LATERAL-ESTRUTURA | Faz o detector considerar todas as pausas do dia. | - | grupo 1, sítio 1 | ...+..... | REGISTRADA | BACKLOG:1069 |
| PAUTAS-PREFILTRO-REPETIDO | LATERAL-ESTRUTURA | Retira a repetição do filtro que decide chamar o motor. | - | pouso COL129 | ......... | REGISTRADA | BACKLOG:1070 |
| OUTBOX-EVENTO-DUPLO | LATERAL-ESTRUTURA | Mede e evita emissão repetida do mesmo evento pelo chamador. | - | pesquisa e medição | .+....... | REGISTRADA | BACKLOG:1071 |
| JUSTIFICATIVA-ENCERRA-ORIGEM | LATERAL-ESTRUTURA | Encerra a cobrança de origem quando a justificativa é aprovada. | - | grupo (1), sítio 1; irmã para cinco casos | + + + . . . . . + | REGISTRADA (parte no ar; cinco casos retidos) | app/docs/BACKLOG.md:1074 |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | LATERAL-ESTRUTURA | Devolve ao fluxo correto perguntas vivas dentro de disputas encerradas. | - | DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA; acervo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1078 |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | LATERAL-ESTRUTURA | Corrige o tratamento das respostas quando ocorre desligamento. | - | DESLIGAR-LIQUIDA-TUDO | + + . . . . . . . | REGISTRADA (superada em 13/11; sucessora definida) | app/docs/BACKLOG.md:1079 |
| O210-COL417-COL931 | LATERAL-ESTRUTURA | Atualiza a apuração de dois colaboradores cujo fechamento ficou antigo. | - | O210 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1081 |
| LOTE-PLANTA-MARCO-AVULSO-E-ABRE-TURNO | LATERAL-ESTRUTURA | Evita que um reparo em lote abra turnos e retire horas trabalhadas. | - | ? (precedência do residual não escrita) | + + + + . . . . . | REGISTRADA (fechada na origem; quatro casos residuais) | app/docs/BACKLOG.md:1084 |
| LOTE-AGRUPA-POR-DIA-E-RASGA-O-PAR-DA-MEIA-NOITE | LATERAL-ESTRUTURA | Mantém juntas as pontas de um turno que atravessa a meia-noite. | - | remedição dos 51 casos em 32 dias | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1085 |
| BO-INSS-LEITORES-QUE-NAO-MIGRARAM | LATERAL-ESTRUTURA | Faz as cobranças concordarem sobre ausências que cobrem parte do dia. | - | ? (seis resíduos sem causa medida) | + + + . . . . . + | REGISTRADA (cura no ar; seis resíduos sem causa) | app/docs/BACKLOG.md:1087 |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | LATERAL-ESTRUTURA | Permite enviar atestado e explica corretamente os erros de reenvio. | - | smoke de reenvio | + + + + . . . . + | REGISTRADA (parte no ar; falta smoke) | app/docs/BACKLOG.md:1091 |
| REABRIR-NAO-DESTROI | LATERAL-ESTRUTURA | Preserva respostas anteriores quando uma pergunta é reaberta. | - | smoke de clique | + + + . . . . . + | REGISTRADA (no ar desde 25/10; falta smoke) | app/docs/BACKLOG.md:1094 |
| UNIVERSO-DE-PERGUNTA-DO-DIA-SAO-DOIS | LATERAL-ESTRUTURA | Faz cartório e vigia considerarem as mesmas perguntas de cada dia. | - | remedição; correção da FK no escritor | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1098 |
| DISPUTA-FECHADA-SEM-RESOLUCAO | LATERAL-ESTRUTURA | Exige que o encerramento da disputa registre o motivo. | - | corte sobre a porta de encerramento | + . . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1101 |
| VIGENCIA-SOBREPOSTA-COL515 | LATERAL-ESTRUTURA | Trata vínculos antigos que cobrem simultaneamente os mesmos dias. | - | corte sobre cadastro | + + . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1102 |
| HE-DE-14H-NUM-DIA (col945 12/09) | LATERAL-ESTRUTURA | Apura por que um dia recebeu quase quinze horas extras. | - | corte; medição da causa | + . . . . . . . . | PARADA (aguarda corte para entrar na fila) | app/docs/BACKLOG.md:1103 |
| BUG-ANCORA-NAO-REGENERA | LATERAL-ESTRUTURA | Faz a correção da fase de trabalho chegar aos dias afetados. | - | decisão sobre col824 e col899 | + + + + . . . . . | PARADA (cura fechada; decisões de cadastro pendentes) | app/docs/BACKLOG.md:1118 |
| FAMILIA-FASE-12x36 | LATERAL-ESTRUTURA | Encontra fases incompatíveis com o trabalho realizado e prepara sua correção. | - | autorização para propostas | + + + . . . . . . | PARADA (itens feitos; propostas aguardam autorização) | app/docs/BACKLOG.md:1128 |
| O-HAIKU-INVESTIGA | LATERAL-ESTRUTURA | Dá ao assistente consultas para investigar escalas e preparar propostas. | - | O-DIA-PAGO S4; VINCULO-LINHA-DO-TEMPO | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1130 |
| O-JANELA-HE | LATERAL-ESTRUTURA | Conta hora extra fora da jornada conforme a regra ou autorização. | - | smoke dos treze passos | + + + . . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1134 |
| CHAMADO-NASCE-EM-EXPORTADA | LATERAL-ESTRUTURA | Investiga cobranças novas abertas para competências já pagas. | - | censo dos emissores; decisão de desenho | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1137 |
| DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO | LATERAL-ESTRUTURA | Trata disputas abertas que já não têm pergunta viva. | - | censo de disputas abertas sem pergunta viva | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1138 |
| COL900-VINCULO-X-CELULA | LATERAL-ESTRUTURA | Mostra a diferença entre a escala corrigida e os dias antigos pagos. | - | autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (aguarda autorização) | app/docs/BACKLOG.md:1140 |
| COL882-O-PROXIMO-COL515 | LATERAL-ESTRUTURA | Prepara a correção de dias trabalhados sem vínculo que os cubra. | - | autorização de vínculo | + + . . . . . . . | PARADA (aguarda autorização de cadastro) | app/docs/BACKLOG.md:1142 |
| DISPUTA-S84-RETRO-PASSIVO | LATERAL-ESTRUTURA | Separa batidas retroativas entre retratação e registro visível do trabalho. | - | cura da origem; autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (fonte conserva espera de autorização) | app/docs/BACKLOG.md:1144 |
| COL369-TRES-RESTANTES | LATERAL-ESTRUTURA | Apura três diferenças de intervalo, pagamento e vínculo do colaborador. | - | smoke da Gestão de HE; autorização para vínculo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1156 |
| O-JANELA-HE parte B2 (AUTORIZAR) | LATERAL-ESTRUTURA | Permite autorizar hora extra e atualizar o pagamento pela porta correta. | - | B1; S4; smoke | + + + + . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1158 |
| BO-354 | LATERAL-ESTRUTURA | Evita perguntas de saída erradas quando as batidas têm tipos invertidos. | - | descongelamento pela L-096 | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1161 |
| O-FOLGA-DIA-ERRADO | LATERAL-ESTRUTURA | Sugere corrigir a escala quando o trabalho recorrente aponta folga cadastrada errada. | - | BO-354; corte de lei; autorização de vínculo | . . . . . . . . . | PARADA (congelada; espera corte) | app/docs/BACKLOG.md:1163 |
| O-GEO-DECISAO | LATERAL-ESTRUTURA | Registra decisões de localização e avisa quando um turno fica sem sinal. | - | corte das etapas 1-2; O198 antes do complemento | . + . . . . . . . | PARADA (etapas congeladas; complemento registrado) | app/docs/BACKLOG.md:1165 |
| O73b | LATERAL-ESTRUTURA | Corrige a cobrança de atraso na volta da pausa do turno partido. | - | descongelamento; autorização sobre a 09 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1166 |
| CORTE-B-30 | LATERAL-ESTRUTURA | Explica individualmente os casos separados do lote de correção. | - | descongelamento; corte do lote B | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1167 |
| O145 | LATERAL-ESTRUTURA | Faz a Gestão de HE enxergar trabalho que não casou com o marco. | - | O229 para parte residual | + + + . . . . . + | REGISTRADA (cura no ar; 33 resíduos) | app/docs/BACKLOG.md:1170 |
| O146 | LATERAL-ESTRUTURA | Paga a extra declarada na escala sem aumentar a jornada devida. | - | linha do copiloto | + + + + . . . . + | REGISTRADA (no ar; falta linha do copiloto) | app/docs/BACKLOG.md:1171 |
| O214 | LATERAL-ESTRUTURA | Permite decidir horas extras por colaborador e período antes da folha. | - | autorização do apply do item 1; smoke do item 2 | + + + . . . . . + | PARADA (itens no ar; apply e smoke pendentes) | app/docs/BACKLOG.md:1172 |
| R1 | LATERAL-ESTRUTURA | Apura o caso em que o papel da batida altera um registro coerente. | - | autorização sobre o caso restante | + + . . . . . . . | PARADA (censo dissolveu a premissa geral; resta um caso) | app/docs/BACKLOG.md:1178 |
| PAR-NAO-ADJACENTE | LATERAL-ESTRUTURA | Preserva o par correto quando há uma batida solta entre suas pontas. | - | LEI-PAR-ANINHADO; volta 5 | . + + + . . . . . | PARADA (versão verde contradiz a lei) | app/docs/BACKLOG.md:1192 |
| O261-E-F2-FILA | LATERAL-ESTRUTURA | Corrige recusas caladas, relógios e identidades no tratamento das perguntas. | - | pouso O261-E-F2 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1193 |
| O261-E-F2-FILA-V2 | LATERAL-ESTRUTURA | Completa testes e corrige informações inconsistentes das perguntas. | - | pouso O261-E-F2 | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1194 |
| REABERTURA-RECUSADA-DEDUP | LATERAL-ESTRUTURA | Evita repetir o registro da mesma reabertura recusada. | - | vez na fila | + . . . . . . . . | PARADA (construtor cancelado sem commit) | app/docs/BACKLOG.md:1195 |
| O232 | LATERAL-ESTRUTURA | Faz a ata reconhecer a saída tardia do mesmo turno. | - | complemento de 25/10; impacto de frota | + + + . . . . + + | REGISTRADA (A/B no ar; complemento da ata pendente) | app/docs/BACKLOG.md:1196 |
| O236 | LATERAL-ESTRUTURA | Identifica por que colaboradores ficam fora do arquivo da folha. | - | medição por motivo; autorização se tocar exportada | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1203 |
| O238 | LATERAL-ESTRUTURA | Faz a trilha apontar o registro certo e distinguir escrita de mudança. | - | O219 A0-b | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1205 |
| O127 | LATERAL-ESTRUTURA | Faz a classificação das batidas considerar todas as pausas cadastradas. | - | O124; DIFF de frota | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1222 |
| O121 | LATERAL-ESTRUTURA | Permite corrigir vínculo passado pela tela com regeneração e trilha. | - | remedição; O65; seis testes antigos | + + + + . . . . . | REGISTRADA (A1 construído; suíte com seis falhas) | app/docs/BACKLOG.md:1228 |
| O123 | LATERAL-ESTRUTURA | Carrega pendências de disputas sem multiplicar consultas por pergunta. | - | pista de testes após fila 1 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1230 |
| O118 | LATERAL-ESTRUTURA | Faz a grade casar as batidas de todas as pausas do dia. | - | print do calendário | + + + . . . . + + | REGISTRADA (código no ar; print pendente) | app/docs/BACKLOG.md:1233 |
| O103 | LATERAL-ESTRUTURA | Permite regenerar somente o dia autorizado, preservando os demais. | - | O105 para o TXT residual | + + + . . . . + . | REGISTRADA (parte fechada; TXT residual pendente) | app/docs/BACKLOG.md:1248 |
| O101 | LATERAL-ESTRUTURA | Exige motivo e registra antes e depois ao mudar a isenção de ponto. | - | pista para executar o selo | + + + . . . . + + | REGISTRADA (código no ar; selo espera pista) | app/docs/BACKLOG.md:1250 |
| O99 | LATERAL-ESTRUTURA | Aplica correções provadas preservando a história do arquivo da folha. | - | decisão sobre 61 dias; TXT | + + + . . . . + + | PARADA (cura 2 no ar; cura 1 e TXT pendentes) | app/docs/BACKLOG.md:1252 |
| O10 | LATERAL-ESTRUTURA | Registra troca de plantão com dias, documentos e confirmação administrativa. | - | credencial; confirmação com dois anexos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1263 |
| HE-OCORRENCIA-COMO-LINHA | LATERAL-ESTRUTURA | Guarda cada ocorrência de hora extra e sua história numa linha consultável. | - | autorização da troca S5b | . + . . . . . . . | REGISTRADA (desenho publicado; nenhuma migration) | app/docs/BACKLOG.md:1271 |
| LOTE-1-PENDENTES | LATERAL-ESTRUTURA | Entrega censos, diferenças e pautas dos cinco grupos de pendências. | - | autorização da troca; decisões dos itens 2 e 5; cadastro do item 3 | + + . . . . . . . | PARADA (item 1 feito; restante espera autorização) | app/docs/BACKLOG.md:1279 |
| LOTE-ATRASO-ADMITIDO | LATERAL-ESTRUTURA | Permite validar em lote os atrasos admitidos pelo colaborador. | - | S5b; UI-RESPOSTA-DIZ-O-QUE-E com smoke; autorização | . . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1284 |
| FECHAMENTO-ZERO-COM-BATIDA | LATERAL-ESTRUTURA | Encaminha cadastros que deixam a folha zerada apesar das batidas. | - | correção autorizada de vínculo | + + . . . . . . . | PARADA (cadastro de vínculo espera autorização) | app/docs/BACKLOG.md:1285 |
| O15 | LATERAL-ESTRUTURA | Entrega ao DP os dias em aberto e as ações necessárias por classe. | - | - | + . + + . . . . + | REGISTRADA (fechada; data do fecho incerta) | app/docs/BACKLOG.md:1294 |
| O19 | LATERAL-ESTRUTURA | Explica falhas e permite enviar documento de ausência pelo aplicativo. | - | autorização e retomada do APP | + . + . . . . . + | PARADA (servidor no ar; APP espera autorização) | app/docs/BACKLOG.md:1298 |
| O28 | LATERAL-ESTRUTURA | Desconta a saída antecipada conforme o catálogo e explica o desconto. | - | DIFF de dinheiro; ! | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1309 |
| O38 | LATERAL-ESTRUTURA | Explica por que uma resposta validada não virou marcação de ponto. | - | O39 para item a; smoke | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1319 |
| O43 | LATERAL-ESTRUTURA | Registra atos e perdas de sessão do administrador para auditoria. | - | O47; esteira seca | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1324 |
| O47 | LATERAL-ESTRUTURA | Faz os atos administrativos guardarem usuário e endereço de origem. | - | O43 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1328 |
| O58 | LATERAL-ESTRUTURA | Registra a emissão do espelho e permite auditoria por período livre. | - | parecer jurídico para hash; Card 4 | + + . . . . . . . | PARADA (hash espera parecer; registro pode avançar) | app/docs/BACKLOG.md:1342 |
| O96 | LATERAL-ESTRUTURA | Permite cadastrar o limite de pausa fixado no código. | - | exportação de 09 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1372 |
| O143 | LATERAL-ESTRUTURA | Encaminha dias trabalhados sem vínculo ao administrador. | - | censo de frota; nome da palavra; pauta com prazo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1399 |
| O144 | LATERAL-ESTRUTURA | Faz duas telas contarem os mesmos dias para o limite de atestados. | - | prova do leitor absenteísmo; censo de derivações | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1400 |
| O147 | LATERAL-ESTRUTURA | Mede o efeito financeiro dos atrasos lançados na competência 09. | - | !; REGEN-EM-EXPORTADA | . + . . . . . . . | PARADA (competência exportada) | app/docs/BACKLOG.md:1401 |
| O149 | LATERAL-ESTRUTURA | Mostra os vínculos cujo fim vem antes do início. | - | lista administrativa; ! para mudar dado | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1403 |
| O151 | LATERAL-ESTRUTURA | Julga chamados do rejulgamento forçado e retrata os sem lastro. | - | porta de retratação; trilha | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1405 |
| O157 | LATERAL-ESTRUTURA | Conta os tipos corretos de ausência ao projetar férias. | - | DIFF por colaborador; catálogo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1410 |
| O159 | LATERAL-ESTRUTURA | Faz ausência integral usar o previsto do dia em vez de 720 minutos. | - | DIFF; reversão; prova posterior | + + . . . . . . . | REGISTRADA (! concedido; construção não iniciada) | app/docs/BACKLOG.md:1412 |
| O172 | LATERAL-ESTRUTURA | Inclui a data na identificação dos insumos do julgamento. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1427 |
| O175 | LATERAL-ESTRUTURA | Evita reconstruir turnos em competências já exportadas. | - | Teto de competência aberta ou corte sobre o cache | ++....... | PARADA (espera corte) | BACKLOG:1430 |
| O181 | LATERAL-ESTRUTURA | Preserva a explicação das horas incluídas na folha. | - | Destino persistente para a trilha | ++....... | REGISTRADA | BACKLOG:1436 |
| O184 | LATERAL-ESTRUTURA | Remove ou corrige uma função que ensina uma medição errada. | - | Decidir remoção ou correção da documentação | ++....... | REGISTRADA | BACKLOG:1439 |
| O198 | LATERAL-ESTRUTURA | Publica faltas aprovadas em dias sem jornada. | - | Corte posterior ao censo | +........ | PARADA (censo publicado; ordem de parar) | BACKLOG:1453 |
| O199 | LATERAL-ESTRUTURA | Faz o aceite da folga decidir o dia e encerrar a cobrança. | - | ! para estoque antigo; O246 | +++....++ | REGISTRADA (ato publicado; estoque pendente) | BACKLOG:1454 |
| O202 | LATERAL-ESTRUTURA | Faz pings consultarem a mesma avaliação oficial de localização. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1457 |
| O204 | LATERAL-ESTRUTURA | Preserva a hora enviada pelo aparelho e registra descartes. | - | Linha do placar em pouso próprio | ++++...++ | REGISTRADA (produto publicado; placar pendente) | BACKLOG:1459 |
| O213 | LATERAL-ESTRUTURA | Conta perguntas cujo tipo de marcação discorda da ata. | - | Publicação do censo antes da cura | ++....... | REGISTRADA (censo feito; nada alterado) | BACKLOG:1465 |
| O243 | LATERAL-ESTRUTURA | Revisa setores críticos e a utilidade dos respectivos testes. | - | Contratos estruturais 20/20; censo antes da poda | .+....... | REGISTRADA | BACKLOG:1474 |
| O246 | LATERAL-ESTRUTURA | Registra aceites antigos de folga que não decidiram o dia. | - | O48 passo 2; RECALC-09; O219; DIFF e reversão | ++....... | REGISTRADA | BACKLOG:1478 |
| O260 | LATERAL-ESTRUTURA | Faz a consulta de retenções devolver o formato prometido. | - | Fatia própria; preservar retorno sem dias | ++~~..... | REGISTRADA (outra linha informa pouso) | BACKLOG:1490; BACKLOG:1494 |
| BO-FERIADO-VINCULO-ANTIGO | LATERAL-ESTRUTURA | Alinha vínculos antigos à decisão sobre trabalho em feriados. | - | Casos mistos e postos encaminhados ao DP | +++...... | REGISTRADA (aplicado; casos do DP pendentes) | BACKLOG:1495 |
| BO-ATESTADO-409-MUDO | LATERAL-ESTRUTURA | Confirma reenvio de atestado e explica recusas reais. | - | Recontagem 48 h; reenvio real; app; linha HAIKU | .++....++ | REGISTRADA (API publicada; verificações pendentes) | BACKLOG:1500 |
| PORTA-CELULA-EDITADA | LATERAL-ESTRUTURA | Permite registrar pela tela o turno autorizado do dia. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1502 |
| INTERVALO-DECLARADO | LATERAL-ESTRUTURA | Pergunta sobre intervalo incompleto e registra a resposta aprovada. | - | BOs em curso; sinal da célula; validação | ++....... | REGISTRADA | BACKLOG:1503 |
| REGEN-EXPORTADA-SEM-EFEITO | LATERAL-ESTRUTURA | Permite corrigir células exportadas sem outros efeitos medidos. | - | Seis condições; reversão; janela de autorização | ++....... | REGISTRADA (validade atual da janela ?) | BACKLOG:1512 |
| INTERMITENTE-BATIDA-SOLTA | LATERAL-ESTRUTURA | Investiga batida isolada de intermitente mostrada como dia correto. | - | Censo de frota; juiz de turno aberto | ++....... | REGISTRADA | BACKLOG:1526 |
| INTERMITENTE-K-FORA-DO-DNA | LATERAL-ESTRUTURA | Preserva o desenho histórico do intervalo do intermitente. | - | Medição e decisão | +........ | REGISTRADA | BACKLOG:1527 |
| INTERMITENTE-CONVOCACAO-CADASTRO | LATERAL-ESTRUTURA | Registra a convocação que define o trabalho do intermitente. | - | Cadastro; leitor na porta da célula | .+....... | REGISTRADA | BACKLOG:1528 |
| O137-8932-ENTERRAR | LATERAL-ESTRUTURA | Remove uma cadeia antiga de abonos sem uso. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1535 |
| O264 | LATERAL-ESTRUTURA | Encaminha a correção do feriado intermitente ao corte posterior. | - | O277 | ++....... | REGISTRADA (cura cancelada; resíduo migrado) | BACKLOG:1549 |
| O269 | LATERAL-ESTRUTURA | Atualiza setembro preservando o arquivo pago e nomeando diferenças. | - | Prova do retrato; DIFF; reversão; Pauta DP | .+....... | REGISTRADA | BACKLOG:1554 |
| O270 | LATERAL-ESTRUTURA | Calcula indenização de intervalo nas jornadas de quatro a seis horas. | - | O263-FT; conferência de CCT; DIFF | ++....... | REGISTRADA | BACKLOG:1555 |
| O271 | LATERAL-ESTRUTURA | Aplica pontualidade ao trabalho comercial pela regra comum. | - | DIFF da 10; condições do apply | ++....... | REGISTRADA | BACKLOG:1556 |
| O273 | LATERAL-ESTRUTURA | Devolve ao admin recusas automáticas que perderam sua justificativa. | - | Elo do cron para 17 revogações | ++++...++ | REGISTRADA (cura publicada; revogações pendentes) | BACKLOG:1558 |
| O275 | LATERAL-ESTRUTURA | Permite cadastrar feriado como dia previsto de trabalho. | - | Decisão FORMA A × FORMA B | ++++..... | INTERROMPIDA | BACKLOG:1560; anexo:interrompidas.txt:19 |
| O277 | LATERAL-ESTRUTURA | Ajusta a dobra de feriado de SDF e intermitente. | - | Pouso da O275 | ++....... | REGISTRADA | BACKLOG:1562 |
| O281 | LATERAL-ESTRUTURA | Mede quatro suspeitas antes de decidir as correções. | - | Medição do bloco A | .+....... | REGISTRADA (leitura antiga) | BACKLOG:1567 |
| O282 | LATERAL-ESTRUTURA | Atualiza estados que ficaram antigos após mudanças nos fatos. | - | O281, bloco A item 3 | .+....... | REGISTRADA | BACKLOG:1568 |
| O306 | LATERAL-ESTRUTURA | Faz respostas de horário persistirem e informa recusas honestas. | - | O317; repetição do smoke | +++....++ | REGISTRADA (cura publicada; passivo e smoke pendentes) | BACKLOG:1592 |
| O314 | LATERAL-ESTRUTURA | Verifica se a falta do mesmo dia é descontada duas vezes. | - | Medir 28 linhas | .+....... | REGISTRADA | BACKLOG:1600 |
| CARIMBO-POR-ORIGEM | LATERAL-ESTRUTURA | Mantém corretos os horários de atualização das gravações. | - | Autorização de desenho | ++....... | PARADA | BACKLOG:1604 |
| EMPATES-SEM-VIGENCIA | LATERAL-ESTRUTURA | Evita anunciar empate com convenção fora da vigência. | - | Decisão da competência de referência | ++....... | PARADA | BACKLOG:1607 |
| EXPIRA-EM-ANDA-COM-HOJE | LATERAL-ESTRUTURA | Guarda a expiração real sem avançá-la diariamente. | - | Censo dos leitores | ++....... | REGISTRADA | BACKLOG:1609 |
| FIO-MORTO-NASCE-NO-DIA-21 | LATERAL-ESTRUTURA | Mede a perda de caminhos de resolução no início da competência. | - | Censo por data da competência | ++....... | REGISTRADA (não construir) | BACKLOG:1611 |
| O317 | LATERAL-ESTRUTURA | Regulariza horários validados e encaminha recusas ao DP. | - | O306; ! e DIFF por competência; O269 para 09 | ++....... | REGISTRADA | BACKLOG:1612 |
| A2-SO-CHEGA-PELA-UNIDADE | LATERAL-ESTRUTURA | Define quem registra mudanças de precedência no julgamento em lote. | - | Decisão entre lote e unidade | ++....... | PARADA | BACKLOG:1619 |
| API-CHAMADOS-V2-SERVE-PERGUNTA-SEM-A-LEI | LATERAL-ESTRUTURA | Faz a API entregar perguntas reconhecidas como vivas. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1621 |
| O320 | LATERAL-ESTRUTURA | Encaminha dias discordantes ao DP com dono e veredito. | - | ? (incompleto não detalhado) | ++++...+. | REGISTRADA (pousada; incompleto declarado) | BACKLOG:1623 |
| GRAVADO-NAO-CARIMBA-A-REGUA | LATERAL-ESTRUTURA | Registra qual régua calculou cada competência. | - | Decisão do local e obrigação do carimbo | ++....... | PARADA | BACKLOG:1630 |
| CADASTRO-DE-CCT-SEM-CARTORIO | LATERAL-ESTRUTURA | Dá trilha às alterações de convenção e régua. | - | Exportação; corte do item c; instrumento do item b | ++~...... | PARADA (itens construídos sem pouso) | BACKLOG:1631 |
| DECIMO-LEITOR-A-PRACA-SAI-DA-ESCALA-DE-HOJE | LATERAL-ESTRUTURA | Investiga a praça atual alterando a régua de período passado. | - | Pouso próprio do vigia | ++....... | REGISTRADA | BACKLOG:1632 |
| ESCREVER-CORTA-PAUTA-EM-SILENCIO | LATERAL-ESTRUTURA | Impede perder orientação ao gravar uma pauta longa. | - | Censo dos 22 chamadores | ++....... | REGISTRADA | BACKLOG:1636 |
| PAUTA-DIA-EM-ABERTO-USA-O-PACKER | LATERAL-ESTRUTURA | Faz pautas longas usarem o empacotador oficial. | - | partir_em_pautas publicado | ++....... | REGISTRADA | BACKLOG:1637 |
| APLICACAO-CONVENCAO-SEM-VIGENCIA | LATERAL-ESTRUTURA | Cadastra desde quando cada empresa segue uma convenção. | - | REGUA-DO-PERIODO; PDFs; resposta do DP | ++....... | REGISTRADA | BACKLOG:1642 |
| COL30-FORA-DOS-TRES-CONJUNTOS-DA-FOLHA | LATERAL-ESTRUTURA | Investiga colaborador ausente das categorias da prévia da folha. | - | Localizar a causa antes da relavratura | ++....... | REGISTRADA | BACKLOG:1644 |
| ABRANGENCIA-DOS-QUATRO-INSTRUMENTOS-SEM-PDF | LATERAL-ESTRUTURA | Confere o enquadramento territorial pelos documentos das convenções. | - | Chegada dos PDFs | ++....... | PARADA | BACKLOG:1645 |
| PERCENTUAL-DO-NOTURNO-SEM-LEITOR | LATERAL-ESTRUTURA | Faz o cálculo consumir o percentual noturno cadastrado. | - | Cadastro e leitor do instrumento | ++....... | REGISTRADA | BACKLOG:1646 |
| JANELA-DO-NOTURNO-CRAVADA | LATERAL-ESTRUTURA | Permite cadastrar a janela noturna da convenção. | - | ! para zona inviolável | ++....... | PARADA | BACKLOG:1647 |
| EXCEDENTE-IGNORA-O-SINDICATO | LATERAL-ESTRUTURA | Faz o cálculo obedecer à regra de excedentes cadastrada. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1648 |
| TOLERANCIA-SEM-LEITOR | LATERAL-ESTRUTURA | Faz tolerâncias cadastradas chegarem ao cálculo. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1649 |
| INTERVALO-MINIMO-SEM-LEITOR | LATERAL-ESTRUTURA | Aplica o intervalo mínimo da convenção cadastrada. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1650 |
| TETO-SEMANAL-12X36-INEXISTENTE | LATERAL-ESTRUTURA | Permite cadastrar o limite semanal da escala 12x36. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1651 |
| RUBRICA-DE-PRONTIDAO-INEXISTENTE | LATERAL-ESTRUTURA | Permite cadastrar o pagamento de prontidão no intervalo. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1652 |
| FURO-APURADO-FALSE-EM-VEZ-DE-NONE | LATERAL-ESTRUTURA | Distingue dia não apurado de dia apurado sem falta. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1653 |
| TRES-GRAFIAS-DE-HOJE-MENOS-UM | LATERAL-ESTRUTURA | Faz três leitores esperarem o encerramento real do turno. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1654 |
| BO-HE-RECUSAR-LOTE-500 | LATERAL-ESTRUTURA | Permite decidir horas extras sem erro de transação. | - | Smoke de tela | +++....++ | REGISTRADA (publicada; smoke pendente) | BACKLOG:1663 |
| SEM-CELULA-11-DEVE-TER-CELULA | LATERAL-ESTRUTURA | Gera previsão também para afastados com vínculo válido. | - | ! para col305 | +++....++ | PARADA (parte publicada; col305 espera !) | BACKLOG:1664 |
| ORIGEM-DISPUTA-S84-RETRO | LATERAL-ESTRUTURA | Identifica origens de batida fora do catálogo permitido. | - | Censo do escritor; ! para reescrever batidas | ++....... | REGISTRADA | BACKLOG:1670 |
| REJULGAR-4-EXPORTADAS | LATERAL-ESTRUTURA | Revê dias exportados com ausência aprovada preservando a exportação. | - | FALTA-EM-FOLGA-VIRA-NUNCA-BATEU | +++...... | REGISTRADA (três resolvidos; um resíduo encaminhado) | BACKLOG:1673 |
| FALTA-EM-FOLGA-VIRA-NUNCA-BATEU | LATERAL-ESTRUTURA | Evita acusar ausência de batida em folga com falta aprovada. | - | Medir a classe; localizar o ramo | ......... | REGISTRADA | BACKLOG:1675 |
| O249-RETRATADOR-NO-DIA-IMPERFEITO | LATERAL-ESTRUTURA | Permite retirar pares acidentais em dias com outras batidas. | - | Pouso do comboio | ++....... | REGISTRADA | BACKLOG:1692 |
| SILENCIO-SEM-PRAZO | LATERAL-ESTRUTURA | Impede cobranças ocultas por silêncios excessivamente longos. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1704 |
| HE100-SEM-PREVISTO-SOZINHA | LATERAL-ESTRUTURA | Evita horas extras de 100% sem a decisão exigida. | - | ? (resíduo sem destino explícito) | +++....++ | REGISTRADA (publicada; resíduo de 75 dias) | BACKLOG:1705 |
| PERIODO-RELER-SEM-CHAMADOR | LATERAL-ESTRUTURA | Remove cálculo paralelo de pontualidade sem uso. | - | Refazer o censo no ato | ++....... | REGISTRADA | BACKLOG:1710 |
| ESMERIL-EXIGE-MEMOS-NO-TRY | LATERAL-ESTRUTURA | Torna visível a falta de dados exigidos pela conferência. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1713 |
| C2-REVISAO-SO-PELO-POSTO | LATERAL-ESTRUTURA | Retira da revisão quem corrigiu as coordenadas. | - | Medir a classe | +........ | REGISTRADA | BACKLOG:1715 |
| PAR-COMPLETO-ROTA-A | LATERAL-ESTRUTURA | Confere o turno antes de retirar um par rápido. | - | Medir a classe | +........ | REGISTRADA | BACKLOG:1717 |
| C2-OS-19-SEM-PRAZO | LATERAL-ESTRUTURA | Dá contador e responsável aos avisos fora da revisão. | - | Marco seguinte ao hotfix C2 | ++....... | REGISTRADA | BACKLOG:1720 |
| LAUDO-col33-0310 | LATERAL-ESTRUTURA | Rejulga dias antigos que conservam o veredito anterior. | - | Corte sobre o restante da frota | +++.....+ | PARADA (universo A publicado; restante espera corte) | BACKLOG:1722 (id anonimizado) |
| SELFIE-RETRO-SEM-ULTIMA-BATIDA | LATERAL-ESTRUTURA | Faz foto atrasada encerrar a disputa criada pelo retroativo. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1723 |
| ATA-X-TURNO-PAUSA | LATERAL-ESTRUTURA | Faz a ata reconhecer a pausa real preservando o dinheiro. | - | FILA-CODEX; conferência 10/10; dinheiro igual | ++....... | INTERROMPIDA | BACKLOG:1728; anexo:interrompidas.txt:21 |
| EMPRESA-ID-ISDIGIT | LATERAL-ESTRUTURA | Evita erro com identificador de empresa numericamente inválido. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1749 |
| PRACA-TOLERANCIA-SEM-LEITOR | LATERAL-ESTRUTURA | Dá destino ao parâmetro de tolerância sem leitor. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1750 |
| FICHA-LE-O-CARIMBO | LATERAL-ESTRUTURA | Faz a ficha ler o vínculo carimbado sem exceder o custo. | - | Alimentar célula carregada ou justificar teto C8 | ++....... | REGISTRADA | BACKLOG:1752 |
| REGUA-VOLTA-VIGILANTES | LATERAL-ESTRUTURA | Reaplica a convenção indicada com motivo e histórico. | - | cura da porta; deploy | .+~...... | REGISTRADA | BACKLOG:1767 |
| RBAC-ESCRITA-POR-PAPEL | LATERAL-ESTRUTURA | Restringe alterações ao papel autorizado e registra o operador. | - | ESMERIL-V2; UIs | .+....... | REGISTRADA | BACKLOG:1769 |
| V-FASE-FILA-FIO-ADMIN-SEM-PORTA | LATERAL-ESTRUTURA | Evita contar uma ação administrativa que não pode ser executada. | - | F5; juiz da porta | ......... | REGISTRADA | BACKLOG:1772 |
| V-FASE-FILA-MIUDOS | LATERAL-ESTRUTURA | Ajusta propostas, atualização e provas do diagnóstico de fase. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1773 |
| TOAST-CORRIDA-2O-POST | LATERAL-ESTRUTURA | Evita erro quando dois envios simultâneos repetem o nome. | - | ? (precedência não indicada) | .+....... | REGISTRADA | BACKLOG:1780 |
| F3-PORTA-E-CRON-PRAZO | LATERAL-ESTRUTURA | Devolve perguntas ao destino correto e separa tarefas de prazo. | - | F3-BECO-1; vaga; autorização do motor | .+....... | REGISTRADA | BACKLOG:1787 |
| LASTRO-PARTE-2 | LATERAL-ESTRUTURA | Fecha cobranças pelo marco correto e transforma o fechador em vigia. | - | JUIZ-1; CRONS-INSTALL-SOB-MARCA | .+....... | REGISTRADA | BACKLOG:1788 |
| LASTRO-PARTE-2-FILA-OPUS | LATERAL-ESTRUTURA | Corrige diferenças de isenção, sinais e eventos. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1789 |
| LASTRO-PARTE-3 | LATERAL-ESTRUTURA | Publica evento quando a resposta permite fechar o chamado. | - | ? (precedência não indicada) | ..+..+++. | REGISTRADA (POUSADO e ABERTA na linha) | BACKLOG:1790 |
| PEP-REENTRADA-FILA | LATERAL-ESTRUTURA | Completa proteções contra reentrada e consultas desnecessárias. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1792 |
| C6-RELOGIO-BORDA | LATERAL-ESTRUTURA | Faz a decisão de pergunta viva receber o relógio externo. | - | censo de chamadores; RED | ......... | REGISTRADA | BACKLOG:1805 |
| H12-DEMISSAO-NA-JANELA | LATERAL-ESTRUTURA | Considera a demissão na competência correta da folha. | - | vaga de negócio; antes da exportação 11 | .+....... | REGISTRADA | BACKLOG:1806 |
| H06-FALTA-PELA-GRADE | LATERAL-ESTRUTURA | Calcula a falta pela jornada prevista, evitando duração fixa. | - | H12; antes da exportação 11 | ......... | REGISTRADA | BACKLOG:1807 |
| MODO-COMUM-PASSE-LIVRE-PELO-DIA | LATERAL-ESTRUTURA | Reconhece a isenção que valia no dia consultado. | - | MODO-COMUM; censo do piloto | .+....... | REGISTRADA | BACKLOG:1813 |
| EFEITO-NONE-UM-LEITOR | LATERAL-ESTRUTURA | Dá resposta única para ausência com tipo fora do catálogo. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1815 |
| SNC2-FILA | LATERAL-ESTRUTURA | Completa testes e trata resíduos de relógio e criação. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1816 |
| F-346 | LATERAL-ESTRUTURA | Faz a remarcação de férias limpar ausências pelo mesmo caminho. | - | fila de concorrência, trilha, veto e selos | .++..++++ | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:1826 |
| H12H06-SEPARA-E-CRONTAB | LATERAL-ESTRUTURA | Separa a correção da falta da atualização do passivo. | - | censo de trava; desenho da parte B | ++...++++ | PARADA (parte B aguarda desenho) | BACKLOG:1827 |
| H12H06-B-SIMPLES | LATERAL-ESTRUTURA | Atualiza trinta linhas com reversão e repetição segura. | - | vaga; complemento B | ++....... | REGISTRADA | BACKLOG:1828 |
| H12H06-B-COMPLEMENTO | LATERAL-ESTRUTURA | Alinha leitores de pagamento parcial ao critério do juiz. | - | H12H06-B-SIMPLES | .+....... | REGISTRADA | BACKLOG:1829 |
| LEI-H12H06-B-A | LATERAL-ESTRUTURA | Aplica a correção e encaminha diferenças da exportada ao DP. | - | cópia de reversão; apply; frota; pauta 08 | .++....+. | REGISTRADA (aplicação e pauta pendentes) | BACKLOG:1830 |
| H12H06-B-PAUTA-08 | LATERAL-ESTRUTURA | Mostra ao DP o pago e o devido na exportada. | - | comando; dry; reversão | .+....... | REGISTRADA | BACKLOG:1831 |
| PAUTA-08-FILA-OPUS | LATERAL-ESTRUTURA | Corrige reversão, transação e documentação das pautas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1832 |
| H12H06-A-FILA | LATERAL-ESTRUTURA | Corrige o rótulo de desligamento e o teste da falta. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1834 |
| TIPO-ITEM3-NAOBLOQ-V5C | LATERAL-ESTRUTURA | Trata resíduos de escrita, leitura e prova da ata tipada. | - | desenho da TIPO-NA-FONTE | ......... | PARADA (aguarda corte) | BACKLOG:1837 |
| DIA-SEM-TRABALHO-PORTA | LATERAL-ESTRUTURA | Oferece decisão administrativa para batida em folga ou feriado. | - | LEI-FIO-MUDO-B | .+.+..... | REGISTRADA | BACKLOG:1848 |
| AUSENCIA-UMA-VIGENTE | LATERAL-ESTRUTURA | Evita descontar dias apresentados como abonados. | - | lei da família; autorização das exportadas | .+....... | REGISTRADA | BACKLOG:1849 |
| PAINEL-SITUACIONAL-UM-MONTADOR | LATERAL-ESTRUTURA | Usa o mesmo montador na atualização individual e na lista. | - | censo; vaga de negócio | .+....... | REGISTRADA | BACKLOG:1851 |
| BUG-PLANTIO-NOTURNO-MAIS-1 | LATERAL-ESTRUTURA | Evita plantar a saída noturna um dia depois do correto. | caso 13–14 | E1-ENTRADAS; BO-0611-05; migração autorizada | .++...... | REGISTRADA (absorvida na E1; complemento pendente) | BACKLOG:1852; BACKLOG:1975 |
| PUSH-DISPUTA-SEM-PERGUNTA | LATERAL-ESTRUTURA | Evita convidar a responder pergunta que não foi criada. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1853 |
| ESMERIL-SEM-INSTANTE | LATERAL-ESTRUTURA | Unifica o significado dos instantes e das datas locais. | - | RED; vaga; rebase; opus; pouso | ..~...... | REGISTRADA (suite antiga não certifica) | BACKLOG:1857 |
| ESMERIL-SEM-DIA-DO-TURNO | LATERAL-ESTRUTURA | Faz leitores perguntarem ao juiz qual é o dia do turno. | - | BUG-PLANTIO; autorização do motor | ......... | REGISTRADA | BACKLOG:1858 |
| ESMERIL-SEM-BORDA | LATERAL-ESTRUTURA | Unifica datas finais e limites das janelas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1859 |
| ESMERIL-SEM-DIAS-PREVISTOS | LATERAL-ESTRUTURA | Conta dias previstos somente quando a célula exige trabalho. | - | cura; selo O155; vaga | ++....... | REGISTRADA | BACKLOG:1860 |
| V2-BUGS-DO-MOTOR | LATERAL-ESTRUTURA | Corrige prorrogação, feriado, passagem de turno e dias abertos. | - | itens 1,3,4,5; apply 11; DIFF/pauta 10 | .++...+++ | REGISTRADA (item 2 no ar; restante pendente) | BACKLOG:1862 |
| ESMERIL-SEM-POLITICA (SILENCIADO-ATE-E-PRAZO) | LATERAL-ESTRUTURA | Alinha o silêncio temporário e remove campos sem consumo. | - | censo; vaga | .+....... | REGISTRADA | BACKLOG:1865 |
| LOTE-VALIDAR-VIRA-JOB | LATERAL-ESTRUTURA | Executa a validação em segundo plano com resultado na tela. | caso 03 | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1866 |
| FERIAS-SALDO-SEM-TRAVA | LATERAL-ESTRUTURA | Protege o saldo de férias contra alterações simultâneas. | HJ-090 | HJ-090 | .+....... | REGISTRADA (absorvida na HJ-090) | BACKLOG:1873 |
| HJ-090 | LATERAL-ESTRUTURA | Faz alterações de férias respeitarem o mesmo saldo e trava. | HJ-090; HJ-091 | datas do DP; aplicação após pouso | +++++~... | EM VOO | BACKLOG:1874; RELATO:10; RELATO:17; RELATO:19; RELATO:53 |
| JUSTIFICATIVA-PAGA-O-PERIODO | LATERAL-ESTRUTURA | Paga somente o período do turno coberto pela justificativa. | HJ-093 | HJ-090; DIFF; motor e folha | ++....... | INTERROMPIDA (brief pronto; ordem viva divergente) | BACKLOG:1875; RELATO:11; RELATO:23; anexo:interrompidas.txt:25 |
| FERIAS-2X-FILA | LATERAL-ESTRUTURA | Completa provas de concorrência, trilha e cancelamento de férias. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1876 |
| V2-I2-FILA | LATERAL-ESTRUTURA | Corrige divergências entre cálculo, espelho e exportação. | - | corte sobre pontas do intervalo | ......... | REGISTRADA | BACKLOG:1877 |
| V2-I2-COL270-DONO | LATERAL-ESTRUTURA | Identifica a origem da mudança do previsto antes do apply. | - | investigação do previsto | ......... | REGISTRADA | BACKLOG:1878 |
| HOLERITE-ZIP-MATRICULA | LATERAL-ESTRUTURA | Retira o envio antigo de holerites sem uso pela tela. | - | E1; E2 | ++....... | REGISTRADA | BACKLOG:1883 |
| HOLERITE-CAMPO-MATCH-LIVRE | LATERAL-ESTRUTURA | Impede enviar holerites com identificação escolhida livremente. | - | HOLERITE-ZIP-MATRICULA; E1; E2 | ++....... | REGISTRADA | BACKLOG:1884 |
| E4B-D2-ANCORA-RAIOX | LATERAL-ESTRUTURA | Faz o diagnóstico apontar o chamado usado pelo fio. | - | correção da âncora | ......... | REGISTRADA | BACKLOG:1904 |
| E4B-D3-ARQUIVAR-PRESCRITOS | LATERAL-ESTRUTURA | Mede pendências sem responsável com o arquivamento desligado. | - | medição do comando desativado | ......... | REGISTRADA | BACKLOG:1905 |
| INTERMITENTE-SEM-CONVOCACAO | LATERAL-ESTRUTURA | Define como representar o dia convocado na escala intermitente. | - | decisão sobre convocação | .+....... | REGISTRADA | BACKLOG:1914 |
| E4B-FILA-OPUS-C14 | LATERAL-ESTRUTURA | Corrige vínculos de pautas e provas restantes do reparo. | - | fila após o 20 | ......... | REGISTRADA | BACKLOG:1926 |
| BO-0611-05 | LATERAL-ESTRUTURA | Permite registrar ponto sem coordenadas de GPS. | - | liberação B3; revisão; smoke | ++.+.+... | PARADA (último estado da linha) | BACKLOG:1927 |
| BO-0611-05-COMPLEMENTO-POUSO | LATERAL-ESTRUTURA | Confere férias, desligamento e testes antes da publicação. | - | BO-0611-05; smoke | ++.+.+... | REGISTRADA | BACKLOG:1931 |
| BO-0611-05-DESLIGADO-FERIAS | LATERAL-ESTRUTURA | Define o efeito financeiro do ponto em férias ou desligamento. | - | duas decisões de lei do DP | +..+..... | REGISTRADA | BACKLOG:1932 |
| REGULARIZAR-TRAVA-PELO-DIA | LATERAL-ESTRUTURA | Evita uma disputa de outro dia bloquear a regularização. | - | RAIZ F1; UI-REGULARIZAR item 4 | .+....... | REGISTRADA | BACKLOG:1935 |
| SAIDA-NOTURNA-NO-DIA-CERTO | LATERAL-ESTRUTURA | Evita rejeitar saída noturna por usar o dia errado. | - | E3-LEITORES; matriz D1–D3 | ~+....... | REGISTRADA | BACKLOG:1937 |
| DECLARADO-X-GRAVADO | LATERAL-ESTRUTURA | Faz a gravação respeitar o horário declarado na resposta. | - | censo; construtor; passivo separado | ++....... | REGISTRADA | BACKLOG:1939 |
| DECLARADO-X-GRAVADO-PASSIVO-11 | LATERAL-ESTRUTURA | Corrige o passivo aberto após conferir seu impacto. | - | DIFF antes do apply; exportadas em pauta | .+....... | REGISTRADA | BACKLOG:1940 |
| BO-0611-05-RESTO-DA-CLASSE | LATERAL-ESTRUTURA | Apura outras barreiras ao registro do ponto. | - | MOTOR-LE-O-TURNO; decisões de lei | ++....... | REGISTRADA | BACKLOG:1955 |
| APP-MENSAGENS-MSG-0 | LATERAL-ESTRUTURA | Liga a justificativa à cobrança correta pela mesma função. | - | TRIAGEM-OFFLINE-SEM-CAMERA; arquivos livres | ++....... | REGISTRADA | BACKLOG:1957 |
| BATIDA-ID-DO-CLIQUE | LATERAL-ESTRUTURA | Identifica cada clique para evitar batidas duplicadas. | - | migration; PWA; build nativo posterior | ......... | REGISTRADA | BACKLOG:1963 |
| BO-0611-05-TEM-GPS-LEITORES | LATERAL-ESTRUTURA | Faz leitores concordarem sobre a existência de GPS. | - | juiz tem_gps da volta 7 | ++....... | REGISTRADA | BACKLOG:1971 |
| ADMIN-FECHA-SEM-MOTIVO | LATERAL-ESTRUTURA | Exige motivo e histórico ao encerrar o chamado. | - | correção da porta administrativa | ++....... | REGISTRADA | BACKLOG:1973 |
| ABSOLVIDA-PELA-FOLGA | LATERAL-ESTRUTURA | Evita absolver cobrança noturna pela folga do dia errado. | - | autorização das exportadas; construção | ++....... | REGISTRADA | BACKLOG:1976 |
| LOTE-VALIDAR-REGRESSAO-0311 | LATERAL-ESTRUTURA | Apura a regressão da validação em lote. | - | fila BO anterior; medir causa | ......... | REGISTRADA | BACKLOG:1977 |
| PREVIEW-REOFERECE-RECUSADA | LATERAL-ESTRUTURA | Evita reoferecer respostas já recusadas no lote. | - | LOTE-VALIDAR-VIRA-JOB | +........ | REGISTRADA | BACKLOG:1978 |
| REVERTER-LEITOR-JANELAS | LATERAL-ESTRUTURA | Faz a leitura reconhecer regenerações desfeitas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1992 |
| REVERTER-SEM-SELECT-FOR-UPDATE | LATERAL-ESTRUTURA | Evita alterações simultâneas durante a reversão. | - | medir ordem das travas | ......... | REGISTRADA | BACKLOG:1993 |
| REVERTER-VOLTA-TRILHA-NA-PORTA | LATERAL-ESTRUTURA | Registra a autorização usada na reversão do dia. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1994 |
| REVERTER-IDA-TRILHA-FRACA | LATERAL-ESTRUTURA | Guarda prova completa da aplicação e evita repetição. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1995 |
| REVERTER-ANTES-LOG-ANTES-DO-JSON | LATERAL-ESTRUTURA | Evita registrar uma cópia de reversão não salva. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1996 |
| PERGUNTA-VIVA-COM-CELULA-ABSOLVEU | LATERAL-ESTRUTURA | Trata pergunta viva após a absolvição do dia. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2004 |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | LATERAL-ESTRUTURA | Permite julgar saída e retorno do intervalo juntos. | - | condições do passivo exportado | +++++~~.. | EM VOO | BACKLOG:2018; RELATO:18; RELATO:36; RELATO:42 |
| BUG-PAR-ESTADO-EXPLICITO | LATERAL-ESTRUTURA | Explicita a espera pela outra metade do intervalo. | - | BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | ......... | REGISTRADA (superada e absorvida) | BACKLOG:2019 |
| PLANTIO-SILENCIO-SEM-DIA | LATERAL-ESTRUTURA | Dá destino visível ao plantio descartado sem dia do turno. | - | desenho da obra mãe | ......... | REGISTRADA | BACKLOG:2031 |
| O10-TROCA-DE-PLANTAO-ENTRA | LATERAL-ESTRUTURA | Permite corrigir trocas históricas pela porta existente. | - | O10-C; lista do admin | .+++..++. | REGISTRADA (B no ar; C pendente) | BACKLOG:2036 |
| O10-B-FILA | LATERAL-ESTRUTURA | Trata concorrência, autorização e histórico das trocas. | - | ? (precedência não indicada) | .+....... | REGISTRADA | BACKLOG:2037 |
| O10-D-PERMUTA-RECIPROCA | LATERAL-ESTRUTURA | Troca plantões de dois colaboradores numa operação segura. | - | fila 1 | ++....... | REGISTRADA | BACKLOG:2038 |
| E-SEM-VIVA-B | LATERAL-ESTRUTURA | Mostra dias retentores anteriores ao desligamento ao administrador. | - | LEI-DESLIGADO-NAO-TRAVA | .++...++. | REGISTRADA (fechada; parte superada hoje; B3 conflitante) | BACKLOG:2045 |
| H12H06-B-FILA-OPUS | LATERAL-ESTRUTURA | Expõe lacunas restantes de ausência, cálculo e cobertura. | - | medir alcance | ......... | REGISTRADA | BACKLOG:2047 |
| G3G4-ESTRUTURA-RESIDUO | LATERAL-ESTRUTURA | Dá obra aos resíduos estruturais sem cobertura. | - | nomear padrão | .+....... | REGISTRADA | BACKLOG:2048 |
| C6-IDENTITY-MAP | LATERAL-ESTRUTURA | Reduz consultas repetidas ao montar a lista. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2052 |
| F-346-FILA-OPUS | LATERAL-ESTRUTURA | Trata concorrência, histórico e erros na remarcação de férias. | - | nomear padrão por item | ......... | REGISTRADA | BACKLOG:2059 |
| E4-8-ROTULO-OK | LATERAL-ESTRUTURA | Explica corretamente a ausência de cobrança no dia. | - | medir células; aval das exportadas | ......... | REGISTRADA | BACKLOG:2069 |
| FUROS-EMPRESA-TIMEOUT | LATERAL-ESTRUTURA | Evita o relatório da empresa ultrapassar o tempo da tela. | - | medir produção; execução em job | ......... | REGISTRADA | BACKLOG:2070 |
| CICLO-C-CEL-TEMPLATE | LATERAL-ESTRUTURA | Trata possível espera circular entre cadastro e célula. | - | reproduzir e medir | ......... | REGISTRADA | BACKLOG:2077 |
| FAIXA-NO-CADASTRO | LATERAL-ESTRUTURA | Permite cadastrar a faixa de precedência da ausência. | - | corte para construir | ......... | REGISTRADA | BACKLOG:2078 |
| EFEITO-SEM-FALLBACK-CATALOGO | LATERAL-ESTRUTURA | Evita voltar silenciosamente ao efeito antigo quando a leitura falha. | - | medir alcance | ......... | REGISTRADA | BACKLOG:2079 |
| LASTRO-FILA-PREVERIF-C11 | LATERAL-ESTRUTURA | Expõe recusas e falhas descartadas pelos leitores e montadores. | - | EFEITO-SEM-FALLBACK-CATALOGO | ......... | REGISTRADA | BACKLOG:2080 |
| O261-PAI-TERMINAL | LATERAL-ESTRUTURA | Encaminha o resíduo sem retomar o desenho vedado. | - | O261-E-PAI-TERMINAL-RESIDUAL | ++....... | REGISTRADA (DEMOLIR hoje; raia como referência) | BACKLOG:2083 |
| O10-C | LATERAL-ESTRUTURA | Entrega lista histórica e ação alcançável para a troca. | - | lista do admin; porta de UI | ......... | INTERROMPIDA | BACKLOG:2085; anexo:interrompidas.txt:29 |
| TIPO-NA-FONTE-LAMPADA-INSTANTE | LATERAL-ESTRUTURA | Guarda com fuso o instante da lâmpada usado pelo plantio. | - | TIPO-DNA-9-CHAVES | ......... | REGISTRADA (absorvida) | BACKLOG:2087 |
| RELOGIO-NA-BORDA | LATERAL-ESTRUTURA | Faz o cadastro usar um único instante recebido na entrada. | - | catraca da c5e | ......... | REGISTRADA | BACKLOG:2088 |
| RELOGIO-NA-BORDA-OPUS-C5E | LATERAL-ESTRUTURA | Trata releituras de relógio e falhas transacionais restantes. | - | RELOGIO-NA-BORDA | ......... | REGISTRADA | BACKLOG:2089 |
| VIGIA-PAREADOR-DIA-CIVIL | LATERAL-ESTRUTURA | Faz a vigilância comparar pelo dia do turno. | - | fora do pouso TIPO item 3 | .+....... | REGISTRADA | BACKLOG:2090 |
| TIPO-ATA-12-CHAVES-A | LATERAL-ESTRUTURA | Faz todas as leituras da ata passarem pelo acesso tipado. | - | item 3 no ar; c5e; censo por chave | ......... | REGISTRADA | BACKLOG:2091 |
| TIPO-ATA-12-CHAVES-B | LATERAL-ESTRUTURA | Converte horas de fato da ata em instantes com fuso. | - | TIPO-ATA-12-CHAVES-A | .+....... | REGISTRADA | BACKLOG:2092 |
| TIPO-DNA-9-CHAVES | LATERAL-ESTRUTURA | Aplica acesso tipado e instantes com fuso ao DNA. | - | programa das 12 chaves da ata | ......... | REGISTRADA | BACKLOG:2093 |
| TRANCA-VIVAS-08-FILA | LATERAL-ESTRUTURA | Alinha o significado do resgate e mede seus efeitos. | - | medir universo e impacto | ......... | REGISTRADA | BACKLOG:2095 |
| E3-PENDENTE-NAO-BOOL | LATERAL-ESTRUTURA | Evita interpretar valores indevidos como lâmpada pendente. | - | pouso A das 12 chaves | ......... | REGISTRADA | BACKLOG:2097 |
| SETNULL-ORFA-DNA | LATERAL-ESTRUTURA | Trata célula futura com vínculo apagado ainda no DNA. | - | corte de desenho | +........ | PARADA (pergunta de desenho) | BACKLOG:2099 |
| TIPO-ITEM3-CONTRACT | LATERAL-ESTRUTURA | Recusa atas fora do formato depois de zerar o passivo. | - | expand no ar; contador zero | .+....... | REGISTRADA | BACKLOG:2100 |
| TIPO-V5-NAO-BLOQUEANTES | LATERAL-ESTRUTURA | Trata relógios internos e comparação civil nos vigias. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2102 |
| CONTRATO-DE-ENTRADA-DO-FIO | LATERAL-ESTRUTURA | Faz o emissor entregar a batida correta ao nascimento da pergunta. | HJ-095 | E3; DESLIGADO/FERIADO | ++....... | BRIEF PRONTO | BACKLOG:2103; RELATO:26 |

## INSTRUMENTO (318)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| DESLIGADOS-RETIDOS-CONTADOR | INSTRUMENTO | Mostra quantos desligados ainda ficam retidos na folha. | - | LEI-DESLIGADO-NAO-TRAVA | .+++.+.++ | NO AR hoje (complemento no mesmo pouso) | BACKLOG:732; app/docs/RELATO.md:103; app/docs/RELATO.md:54 |
| DESLIGADOS-RETIDOS-UNIVERSO | INSTRUMENTO | Faz a contagem de desligados retidos corresponder ao seu rótulo. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:736 |
| OTIMIZA-1-COMPLEMENTO-CARGA | INSTRUMENTO | Reduz a memória retida e mede o pico do vigia. | - | pouso E3; contrato do instrumento | ++....... | BRIEF PRONTO (complemento coberto pelo brief OTIMIZA-1) | BACKLOG:738; app/docs/RELATO.md:25 |
| RAIA-PREPARACAO | INSTRUMENTO | Prepara levantamentos e instruções para as próximas obras. | HJ-091 | marco LEI-DESLIGADO; sequência 1→4 | ++....... | REGISTRADA (levantamentos entregues) | BACKLOG:739; app/docs/RELATO.md:12; app/docs/RELATO.md:28 |
| BO-[nome]-1311-ROTEIRO | INSTRUMENTO | Classifica os casos relatados e identifica o que falta resolver. | casos 01–10 | declaração do colaborador; projeção do retentor | ++....... | REGISTRADA (roteiro parcial) | BACKLOG:740; app/docs/RELATO.md:37 |
| SELOS-CANON-I1 | INSTRUMENTO | Exige levantamento aprovado antes de abrir uma construção. | - | SELOS-CANON-I1-FILA | .+++..++~ | REGISTRADA (núcleo no ar; fila restante) | BACKLOG:747 |
| SELOS-CANON-I1-FILA | INSTRUMENTO | Fecha lacunas na fiscalização do levantamento obrigatório. | - | SELOS-CANON-I1 | .+....... | REGISTRADA | BACKLOG:748 |
| SELOS-CANON-I3 | INSTRUMENTO | Exige etapas e resultados observáveis nas instruções da obra. | - | matriz; vaga | ++....... | REGISTRADA (brief antigo fora dos anexos admitidos) | BACKLOG:749 |
| SELOS-CANON-I2 | INSTRUMENTO | Recusa autorizações de dados sem universo e exclusões definidos. | - | SELOS-CANON-I3 | .+....... | PARADA | BACKLOG:750 |
| ADR-NAS-LEIS | INSTRUMENTO | Registra contexto e consequências de cada lei nova. | - | ? (não indicada) | .+....... | REGISTRADA (conduta vigente) | BACKLOG:757 |
| LAUDO-[nome]-COL94-1610 | INSTRUMENTO | Mede divergências de batida e perguntas invisíveis ao admin. | - | medidas e resposta por item | .+....... | REGISTRADA (medição histórica) | BACKLOG:765 |
| SOMBRA-X-PROD-COL988-124639 | INSTRUMENTO | Identifica por que ensaio e produção julgam diferente. | - | medir o insumo divergente | +........ | REGISTRADA | BACKLOG:767 |
| SOMBRA-NAO-CABE-ANTES-DO-VIZINHO | INSTRUMENTO | Evita que o ensaio ultrapasse a janela dos comandos seguintes. | - | SOMBRA-PORTAO-POR-COMANDO | ++.....+. | REGISTRADA (instrumento parcial pousado) | BACKLOG:769 |
| CRON-NAO-CABE-SOMBRA-87-MIN | INSTRUMENTO | Faz o ensaio caber no horário disponível. | - | N1-PERGUNTAS-DO-APP; LAVRAR-HE-PENDENTE-ESCALA | ++....... | REGISTRADA (cura não comprovada) | BACKLOG:770 |
| SOMBRA-PORTAO-POR-COMANDO | INSTRUMENTO | Exige ensaio atual dos comandos atingidos pela mudança. | - | medir comandos; SOMBRA-INCREMENTAL | .+....... | REGISTRADA | BACKLOG:775 |
| COBPARC-CONTROLE | INSTRUMENTO | Compara a correção com um ensaio sem a correção. | - | explicar chamados adicionais | ++....... | PARADA (COBPARC-DET-NAO-EXPLICA) | BACKLOG:790 |
| O245-CAMADA-COBERTURA | INSTRUMENTO | Mostra autoridades e leitores ainda sem cobertura. | - | push FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:795 |
| E1-RECUSAS-SEM-CONTADOR | INSTRUMENTO | Conta recusas de perguntas com origem inválida ou sem dia. | - | pouso E1 | ......... | REGISTRADA | BACKLOG:804 |
| ODOMETRO-DE-REGRAS | INSTRUMENTO | Detecta regras duplicadas ou excessivas antes da publicação. | - | CLAUDE-MD-RACIONAL; contrato | .+....... | REGISTRADA | BACKLOG:829 |
| REPROVA-E-DESENHO | INSTRUMENTO | Para obra reprovada até registrar sua decisão de desenho. | - | linha DESENHO ou ERRO-BOBO | .++...... | REGISTRADA (construção histórica) | BACKLOG:830 |
| REPROVA-E-DESENHO-CODEX | INSTRUMENTO | Exige decisão quando revisões acumulam achados procedentes. | - | REPROVA-E-DESENHO; suíte v4 | .+....... | REGISTRADA | BACKLOG:831 |
| TRAVA-MENOS-FILA | INSTRUMENTO | Reduz testes repetidos e mostra a espera na fila. | - | contrato do instrumento | .+....... | REGISTRADA (conduta parcial vigente) | BACKLOG:832 |
| TRAVA-COBERTURA | INSTRUMENTO | Vincula testes à obra e remove esperas superadas. | - | vaga; sete curas; COPIA-DO-HEAD | .++..~... | REGISTRADA (volta 4 aguardando vaga) | BACKLOG:834 |
| ADR-0003-CELULA-TRES-NATUREZAS | INSTRUMENTO | Documenta os fatos e as referências vivas da célula. | - | CORRECAO-ADR-0003; censo | ~+....... | REGISTRADA (desenho corrigido) | BACKLOG:835; BACKLOG:836 |
| CORRECAO-ADR-0003 | INSTRUMENTO | Distingue a célula viva da fotografia do fechamento. | - | ADR-0003-CELULA-TRES-NATUREZAS | .+....... | REGISTRADA | BACKLOG:836 |
| PADRAO-DE-CIMA | INSTRUMENTO | Exige rever desenhos que se repetem sem resolver o problema. | - | decisão PDC-DESENHO | .++..~~.. | INTERROMPIDA | BACKLOG:837; anexo:interrompidas.txt:5 |
| DISJUNTOR-B1-ESPERA-OCIOSA | INSTRUMENTO | Detecta agentes apenas esperando testes ou sem produzir. | - | PADRAO-DE-CIMA; contrato | .+....... | REGISTRADA | BACKLOG:838 |
| LEI-AKITA-RESIDUO | INSTRUMENTO | Fecha lacunas na verificação dos compromissos da mudança. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:839 |
| B3-CODEX-VOLTA-NUNCA-TRIADA | INSTRUMENTO | Conta revisões sem triagem para que não desapareçam da fiscalização. | - | REPROVA-E-DESENHO-CODEX | .+....... | REGISTRADA | BACKLOG:840 |
| FASE-CONFLITANTE-VINCULO-DO-DIA | INSTRUMENTO | Faz o vigia conferir o vínculo correto de cada dia. | - | GRADE-FOLGA-JUIZ | ++....... | REGISTRADA | BACKLOG:846 |
| B3-RAIZ-DO-WRAPPER | INSTRUMENTO | Impede uma cópia de consultar a contenção no lugar errado. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:849 |
| B3-CHAVE-DA-OBRA-NOS-DOIS-CONTADORES | INSTRUMENTO | Faz dois contadores reconhecerem a mesma identificação de obra. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:850 |
| B3-NAO-SEI-SEM-SAIDA | INSTRUMENTO | Trata explicitamente a contenção que não pôde ser lida. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:851 |
| B3-CODEX-PREEXISTENTES | INSTRUMENTO | Corrige brechas dos controles e a publicação de zero sem leitura. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:853 |
| CODEX-CUSTO-POR-OBRA | INSTRUMENTO | Publica custo e resultado das chamadas por obra. | - | CLAUDE-MD-ENXUTO; vaga | .++...... | REGISTRADA (contrato escrito) | BACKLOG:854 |
| E4CAL-P1-REVERTER-AMPLO | INSTRUMENTO | Permite desfazer todos os campos alterados pela porta. | - | D5-VIA-NULA | ......... | REGISTRADA | BACKLOG:871 |
| E4CAL-P1-PAREI-FM-CORRENTE | INSTRUMENTO | Evita falso alarme ao comparar a folha corrente ainda mutável. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:872 |
| E4CAL-P1-SELO-CAMPOS-ACORDAM | INSTRUMENTO | Faz o teste detectar eventos e auditoria fora do esperado. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:873 |
| E4CAL-P3-DOCS-VELHAS | INSTRUMENTO | Atualiza registros que ainda descrevem o calendário antigo. | - | pouso L1 | ......... | REGISTRADA (complemento pendente) | BACKLOG:879; app/docs/RELATO.md:1775 |
| TT6-PENDENTE-VELHO | INSTRUMENTO | Reconcilia a pendência TT6 com a mudança do calendário. | - | conferir L-118 após L1 | ......... | REGISTRADA | BACKLOG:884 |
| CONTENCAO-QUE-MORDE-V2 | INSTRUMENTO | Aplica controles de voltas, agentes, revisão e certificação. | - | contrato 20; contrato novo | .+++..~.. | PARADA (estacionada por aval) | BACKLOG:889 |
| CONTENCAO-V2-HORA-DE-LINHA | INSTRUMENTO | Impede publicar horários futuros também em linhas de lista. | - | CONTENCAO-QUE-MORDE-V2 | .+....... | REGISTRADA | BACKLOG:890 |
| F1C4-3-RG-SEM-CELULA-CASO | INSTRUMENTO | Acrescenta teste de dia sem âncora e sem célula. | - | ESMERIL-V2 | ......... | REGISTRADA (volta histórica) | BACKLOG:926 |
| E3-GRADE-POR-TURNO-MEDIR | INSTRUMENTO | Mede o uso da grade alternativa antes de removê-la. | - | medição em produção | ......... | REGISTRADA | BACKLOG:928 |
| EMIS-CATRACA-FLUXO | INSTRUMENTO | Faz o selo conferir o percurso do dado de dia sem trabalho. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:932 |
| E4-CARGA-PARIDADE | INSTRUMENTO | Prova que consultas em lote e isoladas carregam os mesmos dados. | - | F2; antes de BUG-ENC-CELULAS-FORMA | +........ | REGISTRADA | BACKLOG:934 |
| RELATO-ESCRITOR-FORA-DO-FLOCK | INSTRUMENTO | Faz os escritores do relato respeitarem a trava central. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:935 |
| PDC-RELOGIO-NA-CLI | INSTRUMENTO | Faz a linha de comando receber o relógio na entrada. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:936 |
| PDC-PAINEL-SEM-DUVIDA | INSTRUMENTO | Mostra quando não foi possível determinar os disparos. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:937 |
| E4-S4-ENCERRADOS | INSTRUMENTO | Mede o limite de consultas do painel com turnos encerrados. | - | fixture com encerrado lavrado | ......... | REGISTRADA | BACKLOG:938 |
| CODS-ORFAO14H-SEM-GATILHO | INSTRUMENTO | Confere perguntas atrasadas e corrige um teste de controle. | - | renomear teste residual | +........ | REGISTRADA (emissão conferida; renomeação pendente) | BACKLOG:948 |
| ENCADEADO-0111-SEM-CARIMBO | INSTRUMENTO | Identifica por que etapas matinais ficaram sem prova de execução. | - | NOREVERSE-UI-NO-CORE; outros carimbos | +........ | REGISTRADA (causa parcial identificada) | BACKLOG:949 |
| DEPLOY-RELOAD-RESIDUOS | INSTRUMENTO | Corrige resíduos na conferência e no resultado do deploy. | - | contrato; pouso próprio | ......... | REGISTRADA | BACKLOG:951 |
| TELA-PREFIXO-SEM-GUARDA | INSTRUMENTO | Detecta mudanças de endereço que quebrariam links das telas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:952 |
| REVERSAO-PELA-PORTA | INSTRUMENTO | Permite desfazer alterações pela porta que registra sua história. | - | contrato da reversão | .+....... | REGISTRADA | BACKLOG:957 |
| B3-TOPO-FILA | INSTRUMENTO | Identifica corretamente qual obra está bloqueada. | - | DISJUNTOR-B3-TOPO | ......~.. | REGISTRADA | BACKLOG:959 |
| CLAUDE-MD-ENXUTO | INSTRUMENTO | Reduz o documento de instruções preservando suas regras. | - | corte para a meta de tamanho | .++++..+~ | PARADA (parte no ar; meta 8k pendente) | BACKLOG:960 |
| CLAUDE-MD-PISTA-NO-ANEXO | INSTRUMENTO | Entrega também as instruções transferidas para outro documento. | - | CLAUDE-MD-ENXUTO; contrato | ......... | REGISTRADA | BACKLOG:961 |
| CLAUDE-MD-RACIONAL | INSTRUMENTO | Funde instruções repetidas sem alterar as regras. | - | produto; B3 | .+....... | REGISTRADA | BACKLOG:962 |
| CODEX-CONTEXTO-SEM-CONDUTA | INSTRUMENTO | Fornece contexto de fatos e leis ao leitor automático. | - | CLAUDE-MD-ENXUTO; corte do teto | .+....... | PARADA (fontes acima do teto) | BACKLOG:963 |
| CODEX-LICOES | INSTRUMENTO | Reutiliza lições comprovadas nas próximas leituras. | - | CODEX-CONTEXTO-SEM-CONDUTA | ++....... | REGISTRADA | BACKLOG:964 |
| LEI-AKITA-UNIVERSO-DO-PUSH | INSTRUMENTO | Confere padrões nos commits ainda não publicados. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:979 |
| LEI-AKITA-NO-PRE-PUSH | INSTRUMENTO | Impede publicar commits que escaparam da conferência de padrões. | - | LEI-AKITA-UNIVERSO-DO-PUSH | ......... | REGISTRADA | BACKLOG:980 |
| SELO-SETUP-COPIADO-DE-FREEZE | INSTRUMENTO | Detecta relógio congelado que vaza para outros testes. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:981 |
| HX-AUTOR-MUDO-REGEX | INSTRUMENTO | Detecta autor vazio mesmo quando escrito de outra forma. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:982 |
| FIO-ESTADO-LE-O-ALCANCE | INSTRUMENTO | Mostra no painel o alcance real dos dias sem atendimento. | - | F5; cinco itens de fila | .......++ | REGISTRADA (núcleo fechado; fila restante) | BACKLOG:986 |
| ALERTA-DESCARTES-DO-CODEX | INSTRUMENTO | Mostra achados descartados e cobra a justificativa da classificação. | - | contrato do instrumento | .+....... | REGISTRADA | BACKLOG:1004 |
| CODEX-ESCREVE-RED-WRAPPER | INSTRUMENTO | Permite escrever testes de falha em cópia isolada. | - | contrato; verificação do instrumento | .+....... | REGISTRADA | BACKLOG:1005 |
| FIO-DO-ADMIN-NO-ESTADO | INSTRUMENTO | Mostra no painel os dias que dependem do admin. | - | FIO-MORTO-DONO-NA-LISTA | .+....... | REGISTRADA | BACKLOG:1006 |
| VETO-VIGIA-NO-PLACAR | INSTRUMENTO | Mostra no placar chamados vivos em dias vetados. | - | VETO-A | ......... | REGISTRADA | BACKLOG:1010 |
| TESTES-QUE-NAO-PROVAM-[nome] | INSTRUMENTO | Faz testes comprovarem o nascimento do chamado prometido. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1012 |
| CLEAR-SEM-MARCO | INSTRUMENTO | Registra a limpeza de contexto para evitar cobranças antigas. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1013 |
| TESTE-DESLIGADO-C2-VACUO | INSTRUMENTO | Faz o teste detectar a retirada da proteção do desligado. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1017 |
| SELO-CARIMBO-VIRADA | INSTRUMENTO | Testa a virada da meia-noite no carimbo do vigia. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1021 |
| CAL-PALAVRA-BANDAID | INSTRUMENTO | Corrige documentação que descreve solução fora das regras. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1030 |
| CAL-TESTE-HOJE-SEM-PREVISTO | INSTRUMENTO | Cobre em teste o dia de hoje sem previsão de trabalho. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1034 |
| DISJUNTOR-E-CLEAR-RAPIDO | INSTRUMENTO | Interrompe ciclos improdutivos e prepara a troca de contexto. | - | PRONTO 1–6 | .++...... | REGISTRADA (construção antiga) | BACKLOG:1036 |
| DISJUNTOR-E-CLEAR-RAPIDO-COMPLEMENTO | INSTRUMENTO | Acrescenta limites de orçamento e medidas ao disjuntor. | - | DISJUNTOR-E-CLEAR-RAPIDO | .++...... | REGISTRADA (construção antiga) | BACKLOG:1037 |
| SUITE-POR-IMPACTO | INSTRUMENTO | Confere os testes e dias afetados quando a classe permitir. | - | PRONTO-5; rebase; E3; corte do gate | .++...... | INTERROMPIDA | BACKLOG:1038; anexo:interrompidas.txt:15 |
| SELOS-DE-HOST-VERMELHOS-NO-MAIN | INSTRUMENTO | Impede verificações de host vermelhas de escaparem da publicação. | - | SUITE-POR-IMPACTO | ......... | REGISTRADA | BACKLOG:1039 |
| CODEX-CONTEXTO-DA-CASA | INSTRUMENTO | Gera contexto verificável e perguntas neutras para leitura automática. | - | briefs dos construtores em voo | .++...... | INTERROMPIDA | BACKLOG:1040; anexo:interrompidas.txt:17 |
| CODEX-ASTRA-NA-VOLTA-FINAL | INSTRUMENTO | Seleciona explicitamente o modelo da verificação final. | - | decisão após comparativo | .+....... | REGISTRADA (cancelada após empate) | BACKLOG:1041 |
| CODEX-SONDA-LEITURA | INSTRUMENTO | Consulta produção por um envelope de leitura e raspagem. | - | envelope; selo | .++...... | REGISTRADA (construção antiga) | BACKLOG:1044 |
| CODEX-PADRAO-PRO | INSTRUMENTO | Padroniza a leitura automática e mostra seu uso no painel. | - | medição de capacidade | .++...... | REGISTRADA (construção antiga) | BACKLOG:1045 |
| MAIS-CODEX-PLACAR | INSTRUMENTO | Mostra o uso de leitores automáticos e os motivos dos desvios. | - | duas curas de produto no ar | .+....... | PARADA (moratória registrada) | BACKLOG:1046 |
| ESPERA-SEM-TURNO | INSTRUMENTO | Aceita espera somente com trabalho vivo comprovado. | - | contrato da linha do ESTADO | .+.....~~ | REGISTRADA (parte no ar; item do ESTADO pendente) | BACKLOG:1047 |
| CODEX-CONSTRUTOR-PILOTO | INSTRUMENTO | Habilita piloto de construção em cópia isolada. | - | verificadores; pouso do wrapper | .++...... | REGISTRADA (wrapper em cópia; piloto ausente) | BACKLOG:1053 |
| CODEX-RETORNO-0511 | INSTRUMENTO | Mede resultados e custo do uso semanal da leitura automática. | - | medição prevista | .+....... | REGISTRADA | BACKLOG:1054 |
| SELO-DIA-PELO-SIGNIFICADO | INSTRUMENTO | Detecta leitores que inventam o dia fora da autoridade. | - | grupo 1, item 4; antes da LASTRO | .+....... | REGISTRADA | BACKLOG:1056 |
| CODE-CHAMA-CODEX | INSTRUMENTO | Envia leituras a uma cópia identificada do código. | - | ? (não indicada) | .++...... | REGISTRADA (fontes divergem sobre o pouso) | BACKLOG:1058; BACKLOG:1056 |
| PUBLICA-NO-EVENTO | INSTRUMENTO | Publica a resposta final quando ela muda. | - | RELATO-ETAPAS-DO-CUSTO para parte 2 | .++...... | REGISTRADA (construção antiga da parte 1) | BACKLOG:1060 |
| RELATO-ETAPAS-DO-CUSTO | INSTRUMENTO | Mede o custo de cada etapa da publicação. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1067 |
| GATE-EXPORT-10 | INSTRUMENTO | Libera o arquivo da folha depois das conferências e decisões exigidas. | - | decisão do DP sobre HE; autorização | + + + . . . . . . | PARADA (exportação barrada pela porta) | app/docs/BACKLOG.md:1095 |
| FILA-FORA-DO-BLOCO-QUE-O-HOOK-LE | INSTRUMENTO | Impede que obras desapareçam ao gerar novamente a fila. | - | ? (precedência não escrita) | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1096 |
| PARADO-CITADO-E-PROSA-QUE-EXPLICA | INSTRUMENTO | Evita alarmes falsos quando um registro apenas explica uma obra parada. | - | próxima leitura de tickets_placar | + + + + . . . . + | REGISTRADA (instrumento no ar; contador a remedir) | app/docs/BACKLOG.md:1097 |
| ~~PROVA-DOMINIO~~ | INSTRUMENTO | Compararia o cálculo atual com recibos antigos pagos fora do sistema. | - | ? (execução cortada) | . + . . . . . . . | PARADA (cortada; estado fora da lista de exclusão) | app/docs/BACKLOG.md:1117 |
| ESMERIL-MECANICO-POR-TRECHO | INSTRUMENTO | Confere mecanicamente os leitores e escritores por trecho de código. | - | ? (espera truncada de autorização) | + + + . . . . . . | PARADA (fechada na origem; espera truncada preservada) | app/docs/BACKLOG.md:1146 |
| O98 | INSTRUMENTO | Explica os termos da casa e aponta onde existem no código. | - | vaga da raia | + + . . . . . . . | REGISTRADA (arquivo parcial existente) | app/docs/BACKLOG.md:1177 |
| SUITE-DO-NUCLEO-ENTRA-NA-REGUA | INSTRUMENTO | Inclui os testes do núcleo de mensageria nas verificações obrigatórias. | - | O135; pouso | + + + + . . . . . | REGISTRADA (fechada na raia; pouso condicionado) | app/docs/BACKLOG.md:1180 |
| SOMBRA_BLOCO_NA_JANELA-SEM-LEITOR | INSTRUMENTO | Impede certificar uma sombra cujo bloco não cobriu a janela utilizada. | - | pouso próprio de instrumento | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1182 |
| O234 | INSTRUMENTO | Impede publicar contagens de cenários e testes já vencidas. | - | vez de instrumento | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1202 |
| O237 | INSTRUMENTO | Faz os scripts usarem uma única porta de medição na sombra. | - | ? (migração dos scripts é trabalho da própria obra) | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1204 |
| O240 | INSTRUMENTO | Evita aprovar suíte interrompida confundindo mensagem de log com resultado. | - | ? (pré-requisito externo não escrito) | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1207 |
| O241 | INSTRUMENTO | Identifica alterações de duas raias ainda ausentes do código principal. | - | leitura dos 7+7 commits | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1208 |
| O242 | INSTRUMENTO | Faz o teste da corrente usar a própria cópia e preservar carimbos reais. | - | pouso próprio após produto | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1209 |
| O227 | INSTRUMENTO | Permite consultar competência exportada sem pedir autorização de escrita. | - | instrumento após O211 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1210 |
| O226 | INSTRUMENTO | Unifica a resposta sobre quais parâmetros afetam o cálculo. | - | censo dos leitores; decidir autoridade | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1211 |
| O212 | INSTRUMENTO | Mantém na sombra os vínculos necessários para medir os mesmos dias. | - | pouso próprio de instrumento | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1218 |
| HIGIENE-DE-CONTEXTO | INSTRUMENTO | Preserva resumo verificável da sessão e reduz a carga de documentação. | - | autorização do item 5 | + + + . . . . + . | PARADA (itens 1–4 e 6 fechados; item 5 pendente) | app/docs/BACKLOG.md:1267 |
| O16 | INSTRUMENTO | Dá causa e destino aos pacotes que terminavam sem explicação. | - | - | + . + . . . . . . | REGISTRADA (fechada; data do fecho incerta) | app/docs/BACKLOG.md:1295 |
| O17 | INSTRUMENTO | Mostra a idade e a causa da trava de fabricação vazia. | - | - | + . + . . . . + . | REGISTRADA (fechada; data do fecho incerta) | app/docs/BACKLOG.md:1296 |
| O18 | INSTRUMENTO | Separa pacotes parados por causa e encaminha os resíduos. | - | quatro resíduos presos por arquivo de root | + . + . . . . . . | REGISTRADA (fechada sem data; quatro resíduos permanecem) | app/docs/BACKLOG.md:1297 |
| O31 | INSTRUMENTO | Mantém a fabricação de correções andando com limites e alarmes visíveis. | - | fabricante; capacidade de produção | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1312 |
| O32 | INSTRUMENTO | Detecta esteira parada e impede relançamentos repetidos sem causa. | - | O31; fabricante | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1313 |
| O34 | INSTRUMENTO | Dá destino às correções que envelhecem aguardando aplicação. | - | corte de desenho | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1315 |
| O36 | INSTRUMENTO | Impede tratar pacotes caídos como prontos ou publicados. | - | fabricante seco em obra própria | + + + + . . . . + | REGISTRADA (no ar; marco sem data explícita) | app/docs/BACKLOG.md:1317 |
| O42 | INSTRUMENTO | Corrige a seleção e os sinais de funcionamento da fábrica. | - | fabricante; itens 1 e 2 | + + ~ + . . . . . | REGISTRADA (fonte diverge sobre cura dos itens 1/2) | app/docs/BACKLOG.md:1323 |
| O46 | INSTRUMENTO | Encontra sinais de sucesso que não comprovam o fato anunciado. | - | esteira; bugs 2–5 | . + ~ . . . . . . | REGISTRADA (dois casos de abertura curados) | app/docs/BACKLOG.md:1327 |
| O55 | INSTRUMENTO | Evita bloquear o push por uma idade gravada antes da meia-noite. | - | renderizador do placar | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1339 |
| O57 | INSTRUMENTO | Impede que um commit leve alterações preparadas por outra sessão. | - | decisão da guarda de commit | + . ~ . . . . . . | REGISTRADA (selos restaurados; desenho pendente) | app/docs/BACKLOG.md:1341 |
| O64 | INSTRUMENTO | Explica a divergência de inconsistências entre produção e sombra. | - | investigação de ambiente; ! | + . . . . . . . . | PARADA (apply espera explicação) | app/docs/BACKLOG.md:1349 |
| O75 | INSTRUMENTO | Faz o DIFF revelar todos os campos financeiros alterados. | - | medidores de fechamento | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1362 |
| O77 | INSTRUMENTO | Cobra a regra de pontualidade com dois casos opostos. | - | L-084 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1364 |
| O79 | INSTRUMENTO | Impede confundir falta descontável com pergunta de marco faltante. | - | censo AST; L-088 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1366 |
| O91 | INSTRUMENTO | Confere e concilia os vigias declarados com os instalados no host. | - | ! de instalação segundo a célula de estado | + + ~ . . . . . . | PARADA (cabeçalho fechado; estado ainda espera instalação) | app/docs/BACKLOG.md:1376 |
| O90 | INSTRUMENTO | Evita que o hash do último push bloqueie sempre o seguinte. | - | renderizador; régua de tickets | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1377 |
| O82 | INSTRUMENTO | Permite preparar sombra válida entre meia-noite e o backup. | - | deploy das sete curas | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1382 |
| O132 | INSTRUMENTO | Impede que medições ruins definam o horário dos crons. | - | decisão sobre amostras; filtro obrigatório | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1385 |
| O133 | INSTRUMENTO | Limpa o contexto ao fechar marco e retoma pelo handoff. | - | janela sem tarefa viva; ciclo completo provado | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1386 |
| O137 | INSTRUMENTO | Abre revisão de cada família quando seus contratos fecham. | - | família verde; trava de testes | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1390 |
| O137-ESCALA | INSTRUMENTO | Revê código, leitores e documentação da família escala. | - | fim da fila de pouso | + + . . . . . . . | REGISTRADA (agente só consta em registro antigo) | app/docs/BACKLOG.md:1391 |
| O138 | INSTRUMENTO | Lê o hash do git para evitar rodapé sempre vencido. | - | primeira janela sem fila 1 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1392 |
| O150 | INSTRUMENTO | Fecha o furo que permite encerrar com fila estrutural pendente. | - | reprodução do furo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1404 |
| O155 | INSTRUMENTO | Confere dias previstos no fechamento contra dias de trabalho da célula. | - | item em curso; medição antes de cura | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1408 |
| O153 | INSTRUMENTO | Confere se um colaborador consegue acessar dados de outro. | - | cadastro de rotas públicas; selo de autorização | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1413 |
| O160 | INSTRUMENTO | Impede publicar código com conflitos ou importações quebradas. | - | próximo deploy | + + + + . . . . . | REGISTRADA (fechada; publicação ainda indicada como próxima) | app/docs/BACKLOG.md:1414 |
| O163 | INSTRUMENTO | Revê código, leitores e documentação de ausência e férias. | - | O137; família contratualmente fechada | + + . . . . . . . | REGISTRADA (agente só comprovado em registro de 03/10) | app/docs/BACKLOG.md:1417 |
| O165 | INSTRUMENTO | Identifica alterações que podem ser publicadas sem esperar autenticação. | - | Censo das dependências da raia | ~+....... | REGISTRADA (leitura antiga) | BACKLOG:1419 |
| O173 | INSTRUMENTO | Detecta imports incompatíveis entre disco e código servido. | - | Deploy do O142 | ++....... | REGISTRADA | BACKLOG:1428 |
| O174 | INSTRUMENTO | Explica quando tarefas e servidores leem versões diferentes. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1429 |
| O176 | INSTRUMENTO | Impede liberar uma fatia por coincidência de título. | - | Corte para conferir o conteúdo do pacote | ++....... | PARADA (espera corte) | BACKLOG:1431 |
| O185 | INSTRUMENTO | Compara a autenticação com a versão realmente servida. | - | Fatia própria posterior | ++....... | REGISTRADA | BACKLOG:1440 |
| O187 | INSTRUMENTO | Mostra quem consulta cada campo importante do sistema. | - | Fechamento da matriz estrutural | ++....... | REGISTRADA | BACKLOG:1442 |
| O188 | INSTRUMENTO | Impede que renomear código pareça resolver uma pendência. | - | Fechamento da matriz estrutural | .+....... | REGISTRADA | BACKLOG:1443 |
| O190 | INSTRUMENTO | Inclui mensagens na verificação periódica da árvore. | - | Pouso do cert-ast | +........ | REGISTRADA | BACKLOG:1445 |
| O192 | INSTRUMENTO | Evita arquivos de outro usuário nas cópias de teste. | - | Pouso do cert-ast | ++....... | REGISTRADA | BACKLOG:1447 |
| O193 | INSTRUMENTO | Faz o vigia respeitar pausas declaradas com responsável. | - | Pouso próprio junto do cert-ast | ++....... | REGISTRADA | BACKLOG:1448 |
| O196 | INSTRUMENTO | Mostra todas as tabelas alteradas pela relavratura da folha. | - | CELULA-TURNO-FECHA; O145; fatia de dinheiro | ++....... | REGISTRADA | BACKLOG:1451 |
| O201 | INSTRUMENTO | Evita confundir comentários com regras ainda executadas. | - | Decisão sobre a regra ou as lápides | ++....... | REGISTRADA | BACKLOG:1456 |
| O203 | INSTRUMENTO | Impede apagar código e proteção no mesmo commit silenciosamente. | - | Selo consultar a proteção da base | ++....... | REGISTRADA | BACKLOG:1458 |
| O205 | INSTRUMENTO | Confere a árvore deixada por uma reversão automática. | - | Fila de instrumento posterior ao produto | ++....... | REGISTRADA | BACKLOG:1460 |
| O230 | INSTRUMENTO | Permite sondas com acesso restrito de leitura e trilha. | - | Pouso 2; REDs c/d/f; selo na régua | ++++...~. | REGISTRADA (pouso 1 fechado; segundo pendente) | BACKLOG:1470 |
| O233 | INSTRUMENTO | Mostra raias abertas há mais de seis horas sem commit. | - | Pouso 2 da O230 | .+....... | REGISTRADA | BACKLOG:1472 |
| RELATO-CURTO-SELO | INSTRUMENTO | Impede novos relatos acima do limite de linhas. | - | O219; pouso de instrumento da O228 | ++....... | REGISTRADA | BACKLOG:1475 |
| O244 | INSTRUMENTO | Explica em documentos curtos quem manda em cada família. | - | Contratos estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:1476 |
| O245 | INSTRUMENTO | Gera um diagrama de regras, responsáveis e pendências. | - | Contratos estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:1477 |
| O250 | INSTRUMENTO | Mostra por raia a obra, o agente e o que falta pousar. | - | Dois pousos de produto; simular_folha --lido | +++...... | REGISTRADA (construída; pouso pendente) | BACKLOG:1482 |
| HAIKU-CELULA-E-FOLGA | INSTRUMENTO | Expõe contadores de células sem julgamento e folgas com marcos. | - | BUG-CELULA-SEM-VEREDITO; BUG-FOLGA-COM-MARCOS | .+....... | REGISTRADA | BACKLOG:1530 |
| PORTAO-FATURAMENTO-A1 | INSTRUMENTO | Aponta dias com problema sem caminho de resolução. | - | Apply da competência 10 | .+....... | REGISTRADA | BACKLOG:1537 |
| PORTAO-FATURAMENTO-RESTO | INSTRUMENTO | Confere condições de coerência antes do fechamento. | - | Lote EVENTO | .+....... | REGISTRADA | BACKLOG:1538 |
| SOMBRA-SH-PROSA-DA-LAVRA | INSTRUMENTO | Corrige a explicação do ensaio sobre a lavratura. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1539 |
| CRON-SOMBRA-NAO-CABE | INSTRUMENTO | Investiga por que o ensaio excede o horário reservado. | - | Medir tendência da duração | +........ | REGISTRADA | BACKLOG:1544 |
| VIGIA-FIO-BEST-EFFORT-TIPO-MARCO | INSTRUMENTO | Identifica por que uma fiscalização tenta escrever dados. | - | Censo dos chamadores | +........ | REGISTRADA | BACKLOG:1545 |
| O279 | INSTRUMENTO | Retoma a sessão após queda de conexão respeitando paradas legítimas. | - | Liberação ou cura da regra operacional | ++....... | PARADA | BACKLOG:1564 |
| O283 | INSTRUMENTO | Confere os destinos dos botões nas duas interfaces. | - | O274; três hosts corrigidos | .+....... | REGISTRADA | BACKLOG:1569 |
| O285 | INSTRUMENTO | Compara folhas preservando o gravado usado como referência. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1571 |
| O290 | INSTRUMENTO | Mostra por que uma comparação não foi medida. | - | ? (não indicada) | ++++..... | REGISTRADA (feito; pouso sem prova) | BACKLOG:1576 |
| O293 | INSTRUMENTO | Separa falhas da empresa e da competência no registro. | - | Pouso da raia O280 | ++....... | REGISTRADA | BACKLOG:1579 |
| O294 | INSTRUMENTO | Investiga duas mudanças no arquivo exportado de setembro. | - | Leitura da trilha; decisão sobre Pauta DP | ++....... | REGISTRADA | BACKLOG:1580 |
| O296 | INSTRUMENTO | Retira identificações pessoais dos comentários de código. | - | ! para o sítio na zona inviolável | ++....... | PARADA | BACKLOG:1582 |
| O297 | INSTRUMENTO | Alerta quando o relato público deixa de atualizar. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1583 |
| O298 | INSTRUMENTO | Confere se o instrumento executa a medição prometida. | - | ? (não indicada) | ++~...... | REGISTRADA (correções parciais) | BACKLOG:1584 |
| O299 | INSTRUMENTO | Faz o topo do backlog ler a coluna de estado. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1585 |
| O301 | INSTRUMENTO | Detecta ids repetidos que apagam casos de teste. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1587 |
| O302 | INSTRUMENTO | Dá identidade e responsável a containers de teste órfãos. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1588 |
| O303 | INSTRUMENTO | Protege a condição de segurança dos períodos sem carimbo. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1589 |
| O305 | INSTRUMENTO | Corrige referências que passaram a apontar para outra função. | - | Próximo toque em periodo.py | ++....... | REGISTRADA | BACKLOG:1591 |
| O312 | INSTRUMENTO | Prepara um caso de hora extra autorizável no smoke. | - | Caso sintético autorizável | .+....... | REGISTRADA | BACKLOG:1598 |
| CITACAO-POR-LINHA-DERIVA | INSTRUMENTO | Confere referências pelo conteúdo da função citada. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1608 |
| NOME-EM-COMENTARIO-DE-CODIGO | INSTRUMENTO | Remove identificação de colaborador de comentários publicados. | - | Censo de comentários e docstrings | ++....... | REGISTRADA | BACKLOG:1610 |
| O319 | INSTRUMENTO | Confere gravação, persistência e chegada da resposta ao supervisor. | - | O317; O318; ação humana restante | ++....... | REGISTRADA | BACKLOG:1618 |
| TRAVA-FILA1-STRING-E-TRUTHY | INSTRUMENTO | Impede interpretar texto como autorização booleana de fila. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1626 |
| CELULA-DE-ESTADO-TEM-FORMA | INSTRUMENTO | Detecta linhas cuja forma impede ler o estado. | - | Pouso próprio do selo | ++~...... | REGISTRADA | BACKLOG:1627 |
| CONTROLE-SEM-PLANTIO-SEPARA-AS-DUAS-METADES | INSTRUMENTO | Separa o efeito do horário registrado e da relavratura. | - | O317-APPLY-MOVE-MAIS-QUE-A-HORA | ++....... | REGISTRADA | BACKLOG:1628 |
| PROTEGE-VAZIA-EM-55-LEIS | INSTRUMENTO | Detecta leis que nomeiam sítio sem protegê-lo. | - | Autorização para alterar LEIS.md | ++....... | PARADA | BACKLOG:1633 |
| EXCECAO-DE-IMPORT-TARDIO-VIRA-INSTRUMENTO | INSTRUMENTO | Confere exceções de importação entre commit e deploy. | - | ? (não indicada) | +++...... | REGISTRADA (dispositivo fora de bin/) | BACKLOG:1638 |
| RESIDUO-PREMIG-NO-BIND-MOUNT | INSTRUMENTO | Dá destino aos arquivos antigos que confundem o censo. | - | Autorização para apagar | ++....... | PARADA | BACKLOG:1639 |
| REVERSAO-DA-10-NAO-COBRE-O-CARIMBO-DO-ESPELHO | INSTRUMENTO | Faz a reversão cobrir todos os campos alterados. | - | Antes do apply; REGUA10-DUAS-CCT-NA-MESMA-COMPETENCIA | ++....... | REGISTRADA | BACKLOG:1641 |
| APLICACAO-ATIVA-EM-EMPRESA-SEM-CCT | INSTRUMENTO | Conta aplicações ativas em empresas que não usam CCT. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1643 |
| LAPIDE-DO-O197-ESPERA-CORTE-QUE-JA-VEIO | INSTRUMENTO | Corrige a documentação que anuncia decisão concedida como pendente. | - | Marco que tocar O197 | ++....... | REGISTRADA | BACKLOG:1656 |
| CENSO-PRATICA-DECLARADA-X-LEI-E-CCT | INSTRUMENTO | Mostra ao DP diferenças entre horas apuradas, pagas e devidas. | - | PDFs das convenções; sombra | .+....... | REGISTRADA | BACKLOG:1658 |
| O324 | INSTRUMENTO | Separa falha do avaliador de erro da resposta. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1659 |
| O326 | INSTRUMENTO | Registra o nascimento das perguntas para medir seu volume. | - | Migration | ++....... | REGISTRADA | BACKLOG:1661 |
| BREAKER-COSMETICO-DISPARA-SEM-CONTADOR | INSTRUMENTO | Conta limitações de carga aplicadas aos recursos visuais. | - | Censo antes de alterar o limitador | ++....... | REGISTRADA | BACKLOG:1667 |
| CONTADOR-DE-ESCRITA-POR-CRON | INSTRUMENTO | Mostra quais rotinas escrevem e regravam objetos. | - | Placar; verificador | .+++..... | REGISTRADA (construção antiga) | BACKLOG:1690 |
| RELAMPAGO-DURACAO-REMEDIR | INSTRUMENTO | Atualiza a duração declarada da rotina de pares rápidos. | - | Pouso da rotina | ......... | REGISTRADA | BACKLOG:1711 |
| PAPEL-PRAZO-CABECALHO | INSTRUMENTO | Corrige a descrição do papel da rotina no selo. | - | Pouso da raia | .+....... | REGISTRADA | BACKLOG:1712 |
| ESMERIL-7-ACHADOS | INSTRUMENTO | Remove funções sem uso e atualiza o inventário. | - | Marco de cada família | ++....... | REGISTRADA | BACKLOG:1719 |
| ESMERIL-V2 | INSTRUMENTO | Amplia testes automáticos de regras e ciclos de chamados. | - | Contratos em 20/20 | .+....... | REGISTRADA (não construir agora) | BACKLOG:1730 |
| SELO-FASE-12X36-MEIA-NOITE | INSTRUMENTO | Evita falha de teste causada pela passagem da meia-noite. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1733 |
| DEPLOY-SEM-SOMBRA-SO-NO-1O-ARG | INSTRUMENTO | Aceita ou recusa claramente a ordem das opções do deploy. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1734 |
| DESCARTADAS-SEM-LEITOR | INSTRUMENTO | Dá visibilidade às horas de aparelho descartadas. | - | Linha no placar ou remoção pela L-111 | ++....... | REGISTRADA | BACKLOG:1737 |
| SELO-IMPORT-TARDIO-INSTAVEL | INSTRUMENTO | Torna confiável a conferência de imports contra o publicado. | - | Medir interferência entre selos | +........ | REGISTRADA | BACKLOG:1743 |
| FOTOS-SO-A-DE-HOJE-SEM-LEITOR | INSTRUMENTO | Confere alcance dos retratos e elimina competência fixa. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1744 |
| PASTA-DE-SELOS-NO-PUSH | INSTRUMENTO | Impede publicar com selo de host vermelho. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1751 |
| CATRACA-DA-PERGUNTA-2 | INSTRUMENTO | Impede novas cópias da regra de cobertura do vínculo. | - | Próxima parte da E4; três leitores de dinheiro | +++....+. | REGISTRADA (catraca pousada; leitores pendentes) | BACKLOG:1754 |
| GERAR-DIAGRAMA-ESCREVE-NA-ARVORE-VIVA | INSTRUMENTO | Gera o diagrama na cópia de trabalho indicada. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1760 |
| DEPLOY-AGENDADO-LISTAR-OCTAL | INSTRUMENTO | Mostra corretamente os minutos do deploy agendado. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1762 |
| CRONS-DURACAO-CRESCERAM | INSTRUMENTO | Evita sobreposição das tarefas agendadas. | - | investigar duração do auditor | ......... | REGISTRADA | BACKLOG:1763 |
| AUDITOR-ESPELHO | INSTRUMENTO | Confere as contas dos espelhos e separa anomalias. | - | CARIMBO-DA-CONVENCAO; cartão do DP | .+....... | REGISTRADA | BACKLOG:1768 |
| V-FASE-FILA-PENDENTE-912 | INSTRUMENTO | Retira uma referência antiga de autoridade. | - | O141; L-118 | ......... | REGISTRADA | BACKLOG:1770 |
| V-FASE-FILA-LAVRA-TODAS-AS-COMPS | INSTRUMENTO | Mede o custo de atualizar todas as competências. | - | medição; eventual aval | ......... | REGISTRADA | BACKLOG:1775 |
| AUDIT-[nome]-0311 | INSTRUMENTO | Organiza dez problemas relatados e suas lacunas de conferência. | - | tradução do item 7; clique do item 10 | ......... | REGISTRADA | BACKLOG:1777 |
| VERIFICADOR-EM-FABLE | INSTRUMENTO | Usa o verificador indicado e mede seus resultados. | - | virada do reset; medição de três dias | .++~+.... | REGISTRADA | BACKLOG:1785; BACKLOG:1911 |
| R1-E6PLACAR-2O-NUMERO | INSTRUMENTO | Retira um segundo número antigo da mesma pergunta. | - | pouso próprio | ......... | REGISTRADA | BACKLOG:1797 |
| R1-ROTULOS-FILA | INSTRUMENTO | Alinha rótulos, datas e selos às fontes do placar. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1798 |
| CODEX-CONSTRUTOR-NEGOCIO | INSTRUMENTO | Permite construção isolada com regras e acompanhamento do piloto. | - | próximo piloto MOTOR-LE-O-TURNO-INSTRUMENTO | .++..+++. | REGISTRADA (POUSADO e ABERTA na linha) | BACKLOG:1803 |
| CODEX-BANCO-DIRETO | INSTRUMENTO | Oferece leitura isolada do banco com credencial protegida e raspagem. | - | executor dedicado; vaga de instrumento | ++....... | REGISTRADA | BACKLOG:1804 |
| SOMBRA-NAO-CABE-2 | INSTRUMENTO | Reduz verificações para caberem na rotina diária. | - | alarmes; previsto em lote; sete sombras | .+.....++ | REGISTRADA (restante pendente) | BACKLOG:1808 |
| CONFIG-RAIZ-INERTE-EM-APP | INSTRUMENTO | Explica por que a configuração não alcança a sessão do aplicativo. | - | reinício na raiz | +........ | REGISTRADA (leitura feita; reinício pendente) | BACKLOG:1811 |
| CODEX-PRE-DESENHO-PILOTO | INSTRUMENTO | Examina o desenho antes da construção de oito obras. | - | conclusão das oito obras | ++....... | REGISTRADA | BACKLOG:1812 |
| BO-[nome]-0511-CENSO | INSTRUMENTO | Agrupa problemas relatados e lista destinos por colaborador. | - | censo e sonda | ......... | REGISTRADA | BACKLOG:1814 |
| MEDE-CLEAR-X-COMPACT | INSTRUMENTO | Mede consumo e efeitos de limpar ou compactar contexto. | - | medição | ......... | REGISTRADA | BACKLOG:1819 |
| CODEX-QUEIMA-B | INSTRUMENTO | Confere divergências registradas de consumo e diagnóstico. | - | sondas | ......... | REGISTRADA | BACKLOG:1820 |
| HJ-INGESTAO-0511 | INSTRUMENTO | Organiza relatos por colaborador e dia com estado conferido. | - | sonda das funções reais | .+....... | REGISTRADA | BACKLOG:1825 |
| ESPURIA-E-O-QUE-E | INSTRUMENTO | Classifica dias com batidas órfãs e propõe tratamento. | - | medição; desenho; proposta de tela | .+....... | REGISTRADA | BACKLOG:1833 |
| TIPO-ITEM1-DIMENSAO | INSTRUMENTO | Mede acessos à ata e declara limites da ferramenta. | - | conciliar registro do pouso | ..+....+. | REGISTRADA (pouso antigo; linha divergente) | BACKLOG:1838; BACKLOG:1836 |
| VIGIA-PLANTIO-CRIADO-EM-NAO-E-REAPLICACAO | INSTRUMENTO | Faz a vigilância enxergar reaplicações de batidas antigas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1854 |
| SELO-SEM-RELOGIO-JULGA-ARQUIVO | INSTRUMENTO | Evita um teste ocultar relógios soltos no arquivo. | - | ESMERIL-V2 | ......... | REGISTRADA | BACKLOG:1855 |
| ESMERIL-SEMANTICA | INSTRUMENTO | Inventaria significados de datas e horários para orientar correções. | - | aprovação de tela; obras das classes | +........ | REGISTRADA | BACKLOG:1856 |
| PUSH-PRESO-14 | INSTRUMENTO | Destrava a publicação dos commits pela régua vigente. | - | próximo marco fechado; apply | ......... | REGISTRADA | BACKLOG:1861 |
| L-123-MEDIDA-EM-PY | INSTRUMENTO | Distingue dados medidos de código na publicação dos placares. | - | volta 5; vaga; push | .++.+.+.. | REGISTRADA (retomada autorizada) | BACKLOG:1863 |
| ESMERIL-V2-PLACAR-DADO-GERADO | INSTRUMENTO | Faz o placar ler medições de arquivo gerado. | - | ESMERIL-V2 | .+....... | REGISTRADA | BACKLOG:1864 |
| E-SEM-VIVA-B-VIGIA-POR-DIA | INSTRUMENTO | Mede a retenção de desligados por dia. | - | reavaliar após LEI-DESLIGADO-NAO-TRAVA | ......... | REGISTRADA | BACKLOG:1867 |
| BO-[nome]-0611 | INSTRUMENTO | Classifica catorze itens relatados e indica seus destinos. | caso 01–14 | triagem | .+....... | REGISTRADA | BACKLOG:1868 |
| BO-[nome]-0611-V2 | INSTRUMENTO | Confere problemas pelas ações de tela, espelho e pagamento. | caso 02,03,05,07,08,09,10,11,12 | O261; O10-C; L-030; LOTE-VALIDAR | ++....... | REGISTRADA | BACKLOG:1869 |
| MOTOR-LE-O-TURNO-INSTRUMENTO | INSTRUMENTO | Mede cálculos que usam par ou dia fora da autoridade. | - | MOTOR-TURNO-INSTR-FILA; FATIA 1 | +++..++++ | REGISTRADA (pousado; residual pendente) | BACKLOG:1880 |
| MOTOR-TURNO-INSTR-FILA | INSTRUMENTO | Completa provas e limites do medidor de turnos. | - | FATIA 1 de MOTOR-LE-O-TURNO | ......... | REGISTRADA | BACKLOG:1881 |
| DISJUNTOR-B3-TARDIO | INSTRUMENTO | Mostra quando o alarme chega depois da publicação. | - | medir latência da varredura | ......... | REGISTRADA | BACKLOG:1882 |
| PLACAR-LIVENESS | INSTRUMENTO | Mostra cinco sinais de pendências reais antes do faturamento. | - | contrato; primeira medida | ......... | REGISTRADA | BACKLOG:1885 |
| FIO-COM-ASSENTO-E0-V3 | INSTRUMENTO | Registra a avaliação do desenho anterior das obrigações. | - | sucessor v4 | +....+... | REGISTRADA (desenho rejeitado) | BACKLOG:1886 |
| BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE | INSTRUMENTO | Confere a publicação antes de alterar o banco. | - | DEPLOY-PORTAO-RESTOS-V6 | ..++..+++ | REGISTRADA (pouso antigo; residual pendente) | BACKLOG:1890 |
| DEPLOY-RESPAWN-SEM-PORTAO | INSTRUMENTO | Mede reinícios com código ainda sem conferência. | - | medir reinícios por dia | ......... | REGISTRADA | BACKLOG:1891 |
| ATOMICIDADE-DA-PORTA-DIVIDA | INSTRUMENTO | Expõe operações que escapam da transação. | - | cura das violações medidas | +........ | REGISTRADA | BACKLOG:1892 |
| SIGPIPE-PRINTF-CAT-GREP-Q | INSTRUMENTO | Evita falhas indevidas nas verificações por comandos encadeados. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1893 |
| DEPLOY-PORTAO-RESTOS-V6 | INSTRUMENTO | Corrige mensagens e testes restantes da publicação. | - | BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE | ......... | REGISTRADA | BACKLOG:1899 |
| DEPLOY-EXCLUSAO-ENVELOPE | INSTRUMENTO | Evita concorrência entre publicação e tarefas agendadas. | - | E2; contrato | ......... | REGISTRADA | BACKLOG:1900 |
| ORDEM-ATE-O-ZERO | INSTRUMENTO | Ordena as obras até zerar dias sem resolução. | - | E3; E4B; E4C; E4D; O169/O168/LASTRO | ~........ | REGISTRADA | BACKLOG:1913 |
| MAPA-DA-FILA-BO | INSTRUMENTO | Liga cada problema à sua correção estrutural. | - | lote restante de achados | +....+... | REGISTRADA | BACKLOG:1930 |
| PORTA-QUE-NAO-AGE | INSTRUMENTO | Mede dias que oferecem ação sem caminho executável. | - | MAPA-DO-FIO-MORTO | ~........ | REGISTRADA (substituída; despacho aberto na linha) | BACKLOG:1936 |
| RESPOSTAS-1211-1230 | INSTRUMENTO | Reúne os encaminhamentos de cadastro, fila, limites e lastro. | - | obras dos cinco encaminhamentos | ++....~.. | REGISTRADA | BACKLOG:1938 |
| HOOK-B3-PAREI-LEGITIMO | INSTRUMENTO | Reconhece esperas cuja liberação depende do administrador. | - | contrato | .+....... | REGISTRADA | BACKLOG:1945 |
| RAIA-2-BO-VISIVEL | INSTRUMENTO | Ordena a correção dos problemas visíveis da segunda fila. | - | O169; rótulo; triagem; regressão do lote | ~........ | REGISTRADA | BACKLOG:1947 |
| MAPA-ATE-O-20 | INSTRUMENTO | Publica um mapa de obras e dias sem resolução. | - | censo; sonda; selo do publicador | ~........ | REGISTRADA | BACKLOG:1948 |
| GABARITO-FABLE-VERIFICADOR | INSTRUMENTO | Compara revisores nos mesmos casos com regra prévia. | - | seleção de casos; regra de corte | ~........ | REGISTRADA | BACKLOG:1949 |
| SONDA3-METODO-CORRECAO | INSTRUMENTO | Mede correções com a permissão real do executor. | - | nova sonda com perfil real | ......... | REGISTRADA | BACKLOG:1951 |
| AUDITORIA-TRAVAS-POS-RESET-PADRAO-NA | INSTRUMENTO | Avalia bloqueios de publicações que mudam somente comentários. | - | AUDITORIA-DE-TRAVAS | .+....... | REGISTRADA | BACKLOG:1954 |
| GABARITO-FABLE-VIRA-SOMBRA | INSTRUMENTO | Compara cinco revisões independentes sem travar publicação. | - | próximas cinco finais; UI-PAINEL primeiro | ......... | REGISTRADA | BACKLOG:1959 |
| GABARITO-FABLE-SOMBRA-AJUSTE | INSTRUMENTO | Separa falhas perdidas de diferenças de gravidade. | - | GABARITO-FABLE-VIRA-SOMBRA | .....~... | REGISTRADA | BACKLOG:1960 |
| MAPA-20-DERIVADO | INSTRUMENTO | Calcula avanço das obras por provas referenciadas. | - | GRADE-MESTRA sucessora | ..+..+... | REGISTRADA (substituída hoje) | BACKLOG:1964; anexo:aval.txt:1 |
| BO-ROTULO-FILA-DA-FORMA | INSTRUMENTO | Completa a conferência das mudanças de estado pela tela. | - | fila de achados; PROPORCAO-POR-RISCO | ......... | REGISTRADA | BACKLOG:1968 |
| PRONTO-AUSENTE-RASCUNHO | INSTRUMENTO | Lista o que falta em cada obra para conferência. | - | conferência humana | +....+... | REGISTRADA | BACKLOG:1970 |
| BRIEF-COM-PROCEDENCIA | INSTRUMENTO | Exige origem verificável para as decisões do brief. | - | AUDITORIA-DE-TRAVAS; replay | +........ | REGISTRADA | BACKLOG:1974 |
| HOOK-ESPERANDO-PID-MORTO | INSTRUMENTO | Avisa quando o processo aguardado termina sem liberar a fila. | - | contrato; construção futura | +........ | REGISTRADA | BACKLOG:1982 |
| PLACAR-FABRICA-MTIME | INSTRUMENTO | Corrige a comparação de datas no placar da fábrica. | - | pouso próprio | ......... | REGISTRADA | BACKLOG:1983 |
| FIO-ESTADO-CARIMBO-VELHO | INSTRUMENTO | Evita carimbo antigo ocultar campos obrigatórios novos. | - | pouso próprio | ......... | REGISTRADA | BACKLOG:1984 |
| CARIMBO-DIA-CIVIL-X-TURNO | INSTRUMENTO | Distingue dia civil e dia do turno no placar. | - | medir antes | ......... | REGISTRADA | BACKLOG:1985 |
| PLACAR-PNA-ESPERADO-POR-DONO | INSTRUMENTO | Separa pendências administrativas das falhas esperadas em zero. | - | classificação por dono | ......... | REGISTRADA | BACKLOG:1990 |
| O169-RC2-ENGOLIDO | INSTRUMENTO | Faz o alerta de falha chegar ao painel. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1998 |
| O169-TESTE-ORM-RC2 | INSTRUMENTO | Prova o alerta usando banco de teste real. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1999 |
| O169-DONO-VOCABULARIO | INSTRUMENTO | Usa os responsáveis oficiais do placar no diagnóstico. | - | ? (precedência não indicada) | .+....... | REGISTRADA | BACKLOG:2000 |
| O169-CRONS-MD-ALARME | INSTRUMENTO | Alinha a documentação ao alerta instalado. | - | marco do pouso | ......... | REGISTRADA | BACKLOG:2001 |
| O169-V5-SEM-TESTE-CONCORRENTE | INSTRUMENTO | Prova o comportamento com dois aplicadores simultâneos. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2003 |
| PLACAR-A5-RC2-SEM-ALARME | INSTRUMENTO | Evita apresentar falha de comando como painel normal. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2005 |
| PLACAR-FABRICA-UNIDADE | INSTRUMENTO | Distingue perguntas e pautas na contagem publicada. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2006 |
| E4A2-SELO-6G-SUBSTRING | INSTRUMENTO | Faz o teste conferir a contagem exata do alerta. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2009 |
| SELO-MENSAGEM-NO-COMMIT-MSG | INSTRUMENTO | Confere a mensagem antes de gravar o commit. | - | série E | ......... | REGISTRADA | BACKLOG:2011 |
| PAUTA-SEM-TURNO-PREFIXO-RESERVADO | INSTRUMENTO | Evita texto livre se passar por pauta automática. | - | corte de desenho | ..++..... | PARADA (congelada) | BACKLOG:2012 |
| PAUTA-PREFIXO-IDENTIDADE-NO-TEXTO | INSTRUMENTO | Evita identificar pautas por trechos ambíguos de texto. | - | corte de desenho | ......... | PARADA (congelada) | BACKLOG:2013 |
| ACHADO-POR-CASA-TOKEN-EM-PROSA | INSTRUMENTO | Evita citar um bug bloquear indevidamente o envio. | - | série E; contrato | ......... | REGISTRADA | BACKLOG:2017 |
| SONDA-SEM-GRANT | INSTRUMENTO | Faz a leitura consultar sem alterar permissões. | - | próxima vaga | ......... | REGISTRADA | BACKLOG:2020 |
| SONDA-MATRIZ-DE-TRANSICOES | INSTRUMENTO | Expõe transições e permissões antes de retomar a sonda. | - | CODEX-2-RAIAS | +........ | REGISTRADA | BACKLOG:2021 |
| AUDITORIA-0911 | INSTRUMENTO | Separa causas dos resíduos e identifica seus responsáveis. | - | SONDA-SEM-GRANT | ......... | REGISTRADA | BACKLOG:2022 |
| SEGREDO-ARGV-RESIDUO | INSTRUMENTO | Retira senhas dos argumentos visíveis dos processos. | - | SONDA | .+....... | REGISTRADA | BACKLOG:2023 |
| SOMBRA-CARIMBO-MENTE | INSTRUMENTO | Evita certificar sombra alterada sem invalidar a prova. | - | SONDA | ......... | REGISTRADA | BACKLOG:2024 |
| SOMBRA-HOJE-NA-BORDA | INSTRUMENTO | Usa o dia correto após esperas que cruzam meia-noite. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2025 |
| PLACAR-RETIDO-DO-LASTRO | INSTRUMENTO | Representa corretamente a retenção pelo lastro. | - | medir pela porta | ......... | REGISTRADA | BACKLOG:2026 |
| CODEX-2-RAIAS-PORTAO-CPU | INSTRUMENTO | Abre segunda construção somente quando há capacidade. | - | matriz; suite; verificações | ..+...... | REGISTRADA | BACKLOG:2027 |
| CODEX-ALIVIA-1-WRAPPER-BIN | INSTRUMENTO | Permite construir em bin preservando arquivos protegidos. | - | CODEX-2-RAIAS | ......... | REGISTRADA | BACKLOG:2028 |
| CODEX-ALIVIA-3-ESMERIL | INSTRUMENTO | Entrega problemas do código agrupados por família. | - | ALIVIA-1; contrato; capacidade | ......... | REGISTRADA | BACKLOG:2029 |
| PECAS-APOSENTAVEIS-NO-ESTADO | INSTRUMENTO | Mostra peças sem uso e limites da medição. | - | CODEX-2-RAIAS; fila estrutural | ......... | REGISTRADA | BACKLOG:2030 |
| CXR-NA-FILA | INSTRUMENTO | Mostra quais obras cobrem classes abertas do comparativo. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2034 |
| AKITA-MERGE-SEM-LINHA | INSTRUMENTO | Confere merges sem declaração da linha de padrão. | - | contrato | ......... | REGISTRADA | BACKLOG:2041 |
| BO-0511-G2-TRIAGEM | INSTRUMENTO | Classifica os casos do BO e indica seus destinos. | - | ? (precedência não indicada) | +........ | REGISTRADA (medida pronta) | BACKLOG:2042 |
| E-SEM-VIVA-EMPRESA-1 | INSTRUMENTO | Verifica a inclusão da empresa de teste na medição. | - | confirmar operação da empresa | ......... | REGISTRADA | BACKLOG:2043 |
| E-SEM-VIVA-CACA | INSTRUMENTO | Procura causas e destinos de dias sem pergunta viva. | - | lei; E-SEM-VIVA-EMPRESA-1 | +........ | REGISTRADA (medida pronta) | BACKLOG:2044 |
| G1-PORTA | INSTRUMENTO | Identifica dias retentores e a existência de ação disponível. | - | ? (precedência não indicada) | +........ | REGISTRADA (medida pronta) | BACKLOG:2046 |
| CRONS-MARCA-RELOGIO | INSTRUMENTO | Faz a marca temporária usar o relógio da execução. | - | término da marca em 20/11 | ......... | REGISTRADA | BACKLOG:2053 |
| CRIVO-AINDA-VALE | INSTRUMENTO | Identifica promessas antigas diferentes do comportamento do código. | - | LASTRO; MODO-COMUM; BATERIA; contrato | ......... | REGISTRADA | BACKLOG:2054 |
| CODEX-FUNIL-28 | INSTRUMENTO | Destaca erros de valor sem obra correspondente. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2055 |
| G3G4-DONO-MEDIDO | INSTRUMENTO | Classifica casos por responsável e encaminha resíduos estruturais. | - | G3G4-ESTRUTURA-RESIDUO | +........ | REGISTRADA (medida pronta) | BACKLOG:2057 |
| CODEX-RED-POR-EXEMPLO | INSTRUMENTO | Entrega exemplos da regra para orientar testes. | - | abertura das obras | ......... | REGISTRADA | BACKLOG:2058 |
| CODEX-PRE-CODIGO-DUPLO | INSTRUMENTO | Compara duas leituras antes da construção. | - | construtor ainda não aberto | ......... | REGISTRADA | BACKLOG:2060 |
| MARCO-DE-CLEAR-NAO-GRAVOU | INSTRUMENTO | Faz o contador reconhecer a limpeza de contexto realizada. | - | vaga de instrumento; contrato | +........ | REGISTRADA | BACKLOG:2062 |
| TOAST-B-SELO-FINO | INSTRUMENTO | Prova limites e códigos de erro dos avisos. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2063 |
| SELOS-HOST-VERMELHOS-NO-MAIN | INSTRUMENTO | Mede e trata selos de host ainda vermelhos. | - | medição pela régua | ......... | REGISTRADA | BACKLOG:2065 |
| E4-8-FORMA | INSTRUMENTO | Alinha nomes e referências dos testes e leitores. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2068 |
| TOAST-SELO-APELIDO | INSTRUMENTO | Prova a detecção de escritas feitas por apelidos. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:2071 |
| OTIMIZA-DUPLA | INSTRUMENTO | Melhora testes, wrapper e verificações por conjunto contratado. | - | DEPLOY-ELO; cota Codex | ..~...... | REGISTRADA (item 0 feito; 1–5 pendentes) | BACKLOG:2072 |
| OTIMIZA-2-WRAPPER-CODEX | INSTRUMENTO | Melhora a preparação e o retorno das chamadas. | - | cota Codex | ......... | PARADA | BACKLOG:2073 |
| CORTES-JSON-DEFASADO | INSTRUMENTO | Evita regenerar o documento apagando decisões presentes só no texto. | - | preencher JSON; selo | ......... | REGISTRADA | BACKLOG:2075 |
| CONGELA-O-20 | INSTRUMENTO | Mantém o mapa limitado às obras autorizadas. | - | destino documental da GRADE-MESTRA | .+....... | REGISTRADA (destino alterado pelo aval) | BACKLOG:2076; anexo:aval.txt:1 |
| PROPORCAO-FILA-OPUS-V11 | INSTRUMENTO | Trata lacunas de alcance e consumo do selo. | - | AUDITORIA-DE-TRAVAS | ......... | REGISTRADA | BACKLOG:2082 |
| OTIMIZA-1 | INSTRUMENTO | Mostra suítes inteiras e pousos efetivamente concluídos. | - | SUITE-POR-IMPACTO; replay; verificações; pouso | +........ | INTERROMPIDA (brief pronto) | BACKLOG:2086; RELATO:25; anexo:interrompidas.txt:31 |
| NAO-LAVRADA-PASSADO | INSTRUMENTO | Mostra dias passados que ainda não receberam ata. | - | pouso TIPO item 3; medir dono | .+....... | REGISTRADA | BACKLOG:2096 |
| SOMBRA-COM-TETO | INSTRUMENTO | Limita a memória usada pelas sondas na sombra. | - | contrato; pouso próprio | ......... | REGISTRADA | BACKLOG:2098 |
| SOMBRA-LEITURA-V3 | INSTRUMENTO | Permite leituras concorrentes com isolamento e prova corretos. | - | corrigir falhas do instrumento | ..+..+... | REGISTRADA (v1/v2 reprovadas) | BACKLOG:2101 |
| CODEX-CENSO-CONTRATOS-DE-ENTRADA | INSTRUMENTO | Lista desencontros entre produtores e leitores dos dados. | - | corte para abrir obras | +........ | REGISTRADA (censo feito; universo incompleto) | BACKLOG:2104; RELATO:12 |

## UI (86)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO | UI | Avisa quando uma pergunta altera o tipo de batida. | HJ-092 | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:742 |
| API-RETIDOS-ROTULO | UI | Distingue quem está retido de quem está fora da folha. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:744 |
| RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER | UI | Explica a recusa de ausência e as ações possíveis. | - | BOs anteriores | .+....... | REGISTRADA | BACKLOG:760 |
| TELA-6X1-NAO-MOSTRA-HORARIOS-POR-DIA | UI | Mostra os horários por dia na edição da escala 6x1. | - | fila 2 | .+....... | REGISTRADA | BACKLOG:761 |
| LISTA-DE-QUARENTENA | UI | Mostra quem está em quarentena e o caminho para levantá-la. | - | QUARENTENA-DO-COLAB | .+....... | REGISTRADA (incorporada à QUARENTENA) | BACKLOG:763 |
| O148-EDITAR | UI | Permite editar ou arquivar calendários de folgas com histórico. | - | corte sobre edição por versão | ......... | PARADA (lei nova pendente) | BACKLOG:764 |
| TENTATIVA-DO-ADMIN-VIRA-COBRANCA | UI | Mostra o efeito da edição antes de gerar novas cobranças. | - | medir regenerações; proposta de tela; corte | ......... | REGISTRADA (parte exige lei) | BACKLOG:779 |
| CAL-RODAPE-E-PDF-SEM-CERTIDAO | UI | Mostra e conta corretamente dias sem informação certificada. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1035 |
| O256 | UI | Reúne filtros, responsáveis e explicações do retrato da folha. | - | porta de PDF; O261 para complemento de dono; smoke final | + + + . . . . . + | PARADA (PDF sem porta; outras partes no ar) | app/docs/BACKLOG.md:1100 |
| WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA | UI | Permite corrigir a fase desde o vínculo que cobre o dia errado. | - | VINCULO-LINHA-DO-TEMPO | + + + . . . . . . | PARADA (congelada; absorvida por outra obra) | app/docs/BACKLOG.md:1119 |
| AVISO-ENTRADA-ANTECIPADA | UI | Avisa quanto falta para começar a jornada quando o colaborador bate antes. | - | descongelamento pela L-096 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1122 |
| AVISO-ESPURIA-JANELA (desenho) | UI | Define o aviso do aplicativo para batidas fora da janela esperada. | - | confirmação do admin da folha e do dono | . . . . . . . . . | PARADA (desenho aguardando confirmação) | app/docs/BACKLOG.md:1123 |
| BUG-APP-SESSAO-401 | UI | Avisa quando a perda de sessão impede registrar o ponto. | - | autorização; smoke nas duas cascas | + + + . . . . . + | PARADA (admin no ar; app aguarda smoke) | app/docs/BACKLOG.md:1148 |
| UI-ANEXO-AUSENCIA | UI | Mostra anexos pelo conteúdo e prepara a conversão de fotos HEIC. | - | catálogo; O30 F3; rebuild com decodificador HEIC | + + . . . . . . . | PARADA (conversão bloqueada pelo rebuild) | app/docs/BACKLOG.md:1149 |
| UI-PAINEL-MOSTRA-O-APP | UI | Faz o painel mostrar o mesmo rótulo e contagem do aplicativo. | - | lote UI-ANEXO-AUSENCIA | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1150 |
| DASHBOARD-V2-ROTULO-A-MAO | UI | Faz o painel alternativo ler o rótulo comum dos chamados. | - | pouso de UI-PAINEL-MOSTRA-O-APP | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1151 |
| ABSOLVIDA-TEXTO-PLANTAO | UI | Explica as datas do plantão e da saída após meia-noite. | - | UI-PAINEL-MOSTRA-O-APP; POS-20 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1152 |
| UI-REGULARIZAR-FALA-A-LINGUA-DO-ADMIN | UI | Troca termos técnicos por ações claras para corrigir batidas. | - | proposta de tela; POS-20 | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1153 |
| O-JANELA-HE Gestao de HE (LISTAGEM) | UI | Lista horas extras pendentes com os mesmos dados do bloqueio da folha. | - | smoke de clique; merge | + + + + + . . . . | PARADA (pronta na raia; falta smoke para merge) | app/docs/BACKLOG.md:1155 |
| GESTAO-HE-FORMA | UI | Organiza a decisão de horas extras por colaborador e dias expansíveis. | - | desenho aprovado; corte sobre autorização de uma ponta | + + + . . . . . + | PARADA (expandido devolvido à bancada; pergunta aberta) | app/docs/BACKLOG.md:1157 |
| AVISO-DO-DONO | UI | Avisa o responsável quando uma obra fecha ou precisa de autorização. | - | portão da 09; descongelamento; smoke no celular | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1164 |
| O223 | UI | Permite cadastrar pela tela a convenção e o regime da empresa. | - | smoke do pouso A para push | + + + . . . . + + | REGISTRADA (pousos no ar; smoke para push pendente) | app/docs/BACKLOG.md:1198 |
| O225 | UI | Faz os avisos de parâmetro sem efeito acompanharem a declaração oficial. | - | vez de UI | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1212 |
| O128 | UI | Mostra na lista de escalas os quadros já preparados pela página. | - | smoke O122 ou retorno do template | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1219 |
| O122 | UI | Melhora a navegação do plano de escalas. | - | nova entrega e smoke de UI | . + + . . . . . . | REGISTRADA (etapa 1 desfeita pelo gate) | app/docs/BACKLOG.md:1229 |
| O2 | UI | Recusa no cartão data final posterior a ontem com mensagem clara. | - | ? (PAREI preservado em histórico truncado) | . . + . . . . . . | PARADA (fechada com PAREI preservado; motivo incompleto) | app/docs/BACKLOG.md:1255 |
| O8 | UI | Mostra no PDF a identificação das ausências parciais e não decididas. | - | tratamento da ausência rejeitada | + + + . . . . . . | REGISTRADA (parcial e pendente fechados; rejeitada registrada) | app/docs/BACKLOG.md:1261 |
| GESTAO-HE-CALENDARIO-CONTROLE | UI | Permite selecionar no calendário os dias de hora extra para decisão. | - | fatia 2 após troca S5b | + + + . . . . + + | REGISTRADA (fatia 1 no ar; fatia 2 pendente) | app/docs/BACKLOG.md:1268 |
| GESTAO-HE-FATIA-2-LOTE-E-LIMITE | UI | Permite decidir horas extras de vários dias com limite na tela. | - | troca S5b; conciliar limite entregue pela O214 | + + + . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1269 |
| F1 | UI | Reúne atestados e imagens dos anexos num PDF por período. | - | O30 F3 | . + . . . . . . . | REGISTRADA (fechada por absorção; fecho sem data) | app/docs/BACKLOG.md:1303 |
| F2 | UI | Mostra as faltas que reduzem as férias no período aquisitivo. | - | contratos 22/22; O157 | + + . . . . . . . | PARADA (portão contratual) | app/docs/BACKLOG.md:1304 |
| O24 | UI | Permite filtrar, reabrir e recalcular fechamentos pela tela. | - | exportação de 09; CARTAO=ESPELHO; O39 | . + . . . . . . . | REGISTRADA (superada pela O39; fecho sem data) | app/docs/BACKLOG.md:1305; app/docs/BACKLOG.md:1320 |
| O29 | UI | Entrega documentos de atestados em relatório por colaborador ou empresa. | - | O30 F3 | . + . . . . . . . | REGISTRADA (fechada por absorção; fecho sem data) | app/docs/BACKLOG.md:1310 |
| O30 | UI | Organiza o histórico de ausências e baixa documentos em lote. | - | vale da F1; pillow_heif; rebuild | . + ~ . . . . + + | PARADA (F1 no ar; F2/F3 aguardam vale na fonte) | app/docs/BACKLOG.md:1311 |
| O39 | UI | Dá ao DP uma porta visível para reabrir o fechamento. | - | O44; smoke nas duas cascas | . + . . . . . . . | REGISTRADA (suspensa pela O44) | app/docs/BACKLOG.md:1320; app/docs/BACKLOG.md:1325 |
| O59 | UI | Monta o dossiê de rescisão com documentos já emitidos. | - | O58; UI-RESCISAO | . + . . . . . . . | REGISTRADA (absorvida em 13/11 pela UI-RESCISAO) | app/docs/BACKLOG.md:1343 |
| UI-RESCISAO | UI | Reúne liquidação e documentos do desligamento numa tela. | - | pós-20; O58; três perguntas de lei | + + . . . . . . . | REGISTRADA (censo pronto; brief não demonstrado) | app/docs/BACKLOG.md:1344 |
| O74 | UI | Mostra o tipo de turno conforme sua definição real. | - | smoke | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1361 |
| O148 | UI | Permite administrar calendários de folgas sem duplicidade e com busca. | - | O152; medição; foto contínua | + + ~ . . . . + + | REGISTRADA (item 1 no ar; restante não começado) | app/docs/BACKLOG.md:1402 |
| O152 | UI | Oferece busca comum que procura o texto mostrado nas listas. | - | O148; depois dos BOs | + + . . . . . . . | REGISTRADA (parte da obra conjunta com O148) | app/docs/BACKLOG.md:1406 |
| O161 | UI | Permite editar e copiar modelos pela linha do modelo pai. | - | smoke nas cascas que usam o partial | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1415 |
| O162 | UI | Mostra rótulos claros para escala e duração da ausência. | - | O30 F2 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1416 |
| O167 | UI | Impede correções sem trilha pelo painel administrativo. | - | Censo de quem corrige dados pelo painel | ++....... | REGISTRADA | BACKLOG:1421 |
| O206 | UI | Oferece cobrança com prévia fiel e preservação das respostas. | - | Correção da prévia; smoke das duas cascas | +++....++ | PARADA (smoke segurado) | BACKLOG:1461 |
| O235 | UI | Explica o efeito de intervalo vazio ou zero no formulário. | - | O207 | ++....... | REGISTRADA | BACKLOG:1473 |
| O251 | UI | Alinha a prévia da cobrança e expõe uma configuração de cálculo. | - | O207; raia própria; DIFF e reversão | ++....... | REGISTRADA | BACKLOG:1483 |
| BUG-RETRATO-VELHO-NA-TELA | UI | Avisa quando o retrato da folha está desatualizado. | - | O48 passo 6 após 20/20; medição residual | ++~...... | REGISTRADA | BACKLOG:1498 |
| C7-TELA-ADMIN-ENCERRA | UI | Permite encerrar uma disputa com motivo obrigatório. | - | Porta admin_encerra; smoke das duas cascas | ++....... | REGISTRADA | BACKLOG:1499 |
| col155-HE-NAO-POR-MINUTO | UI | Limita a recusa de horas extras aos minutos decididos. | - | Caso pela regra; prova de dinheiro zero | ++....... | REGISTRADA | BACKLOG:1504 (id anonimizado) |
| FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO | UI | Explica que a folga contestada exige correção cadastral. | - | Raia UI; contador do caso col666 | ++....... | REGISTRADA | BACKLOG:1520 |
| O267 | UI | Mostra quantos dias ainda seguram a folha. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1552 |
| O274 | UI | Faz os botões do fio funcionarem no painel lateral. | - | Smoke de clique | ++....... | REGISTRADA | BACKLOG:1559 |
| O284 | UI | Gera espelhos em segundo plano com progresso e download. | - | Smoke; parte B na O280 | +++....++ | REGISTRADA (parte A publicada; smoke pendente) | BACKLOG:1570 |
| O308 | UI | Reabre o assistente com a data gravada no vínculo. | - | Cura do contexto; smoke | ++....... | REGISTRADA | BACKLOG:1594 |
| O310 | UI | Mantém a orientação do retrato visível em todas as abas. | - | O310-RAIOX | +++....++ | REGISTRADA (parte publicada; Raio-X em linha própria) | BACKLOG:1596 |
| O311 | UI | Completa push nativo e cobrança das perguntas de disputa. | - | Cruzamento com O306 | ++~...... | REGISTRADA | BACKLOG:1597 |
| O310-RAIOX | UI | Mostra uma lente baseada no retrato oficial da folha. | - | Corte para história, modelo e migration | +.+....++ | PARADA (lente publicada; história incompleta) | BACKLOG:1605 |
| L7-AJUDA-DA-REGUA-CCT | UI | Faz a ajuda acompanhar a autoridade da convenção. | - | Alteração em core/regua_cct.py | ++....... | REGISTRADA | BACKLOG:1606 |
| O318 | UI | Retira formulário de horários validados que aguardam regularização. | - | O317; O319; smoke | ++++...++ | REGISTRADA (card publicado; smoke pendente) | BACKLOG:1613 |
| O318-DIA-MISTO | UI | Oferece formulário apenas para marcos aceitos pela porta. | - | Item anterior em curso | ++....... | REGISTRADA | BACKLOG:1614 |
| CARD-SEM-DIA-OFERECE-FORMULARIO | UI | Evita oferecer formulário quando a porta não determina o dia. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1620 |
| O323 | UI | Permite editar a prorrogação noturna com histórico. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1657 |
| BUILDER-POR-POSTO | UI | Filtra relatórios por posto com totais e coluna no CSV. | - | ? (não indicada) | .+~...... | REGISTRADA (construção antiga) | BACKLOG:1691 |
| O122-ETAPA-2 | UI | Filtra tipos de escala por praça, posto, busca e uso. | - | Pouso do FECHAR | .+....... | REGISTRADA | BACKLOG:1718 |
| UI-MARQUE-AS-CAIXAS-E-PDF-POSTO | UI | Corrige instruções sem caixas e possível repetição no PDF. | - | Conferir identificadores no PDF | ......... | REGISTRADA | BACKLOG:1726 |
| DOIS-DESTINOS-NA-LINHA | UI | Faz a mesma linha mostrar uma única sugestão de destino. | - | Corte para construir | ++....... | REGISTRADA (não curar agora) | BACKLOG:1755 |
| CARIMBO-DA-CONVENCAO | UI | Mostra qual convenção rege cada trecho calculado. | - | regra usada em cada fatia | .+....... | REGISTRADA | BACKLOG:1766 |
| HOLERITE-SHARE-WEBVIEW | UI | Permite compartilhar o holerite pelo servidor no aplicativo. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1776 |
| PWA-OFFLINE-APP-SHELL | UI | Mantém navegação e fila de batidas sem rede. | - | identidade na fila; smoke | .+....... | REGISTRADA | BACKLOG:1810 |
| PDF-UNIAO-HHMM-ENTRE-DATAS | UI | Evita esconder turno aberto por horários iguais em datas diferentes. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1841 |
| TRIAGEM-FILA-OPUS-V6 | UI | Completa proteções e provas de câmera, selfie e armazenamento. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1842 |
| ESPURIA-TELA (+ESPELHO-DECIDIU-ICONE) | UI | Diferencia folga, feriado, ausência e batida sem decisão. | - | censo fora da L-061; smoke | .+....... | REGISTRADA | BACKLOG:1850 |
| PWA-FILA-OFFLINE | UI | Confere se batidas sem rede ficam guardadas até o envio. | - | leitura do envio e armazenamento | .+....... | REGISTRADA | BACKLOG:1870 |
| BO-ROTULO-ENCERRADO | UI | Distingue os nomes de chamado resolvido e fechado. | - | selo da saída renderizada | .+...~... | REGISTRADA | BACKLOG:1950 |
| TRIAGEM-OFFLINE-SEM-CAMERA | UI | Permite guardar ponto offline quando a câmera falha. | - | classe nomeada; RED; smoke | ++...+... | PARADA (revertida; retomada condicionada) | BACKLOG:1956 |
| APP-MENSAGENS (MSG-1..4) | UI | Reúne acesso a mensagens e envio de mídia no aplicativo. | - | E4C D05; RAIZ F1 | ......... | REGISTRADA | BACKLOG:1958 |
| BO-ROTULO-GLPI-SELETOR-MORTO | UI | Faz a opção de estado executar a mudança prometida. | - | aval próprio | .+....... | REGISTRADA | BACKLOG:1965 |
| BO-ROTULO-REABRIR-RECUSADO | UI | Evita oferecer reabertura proibida ao perfil. | - | correção da oferta de ação | .+....... | REGISTRADA | BACKLOG:1966 |
| BO-ROTULO-TERCEIRO-VOCABULARIO | UI | Usa os mesmos nomes de estado nas telas. | - | unificação dos rótulos | ......... | REGISTRADA | BACKLOG:1967 |
| TRIAGEM-OFFLINE-FILA | UI | Trata pendências de foto, localização e textos da triagem. | - | fila da triagem offline | ......... | REGISTRADA | BACKLOG:1969 |
| GPS-ESTADO-NA-TELA | UI | Mostra se a localização está atual, indisponível ou desatualizada. | - | BO-DISPUTA; E4B; smoke | ++....... | REGISTRADA | BACKLOG:1972 |
| BO-0611-10-TELA-DA-TROCA | UI | Acrescenta a tela da troca relatada. | caso 10 | fim da série E | ......... | REGISTRADA | BACKLOG:1980 |
| PWA-FILA-OFFLINE-400 | UI | Evita perder silenciosamente batidas recusadas ou não enviadas. | - | sonda da quantidade | ......... | REGISTRADA | BACKLOG:2033 |
| G6-PAGINA | UI | Mantém a página da lista após ação administrativa. | - | vaga; smoke nas duas cascas | ......... | REGISTRADA | BACKLOG:2035 |
| BUG-SW-RESPOND-UNDEFINED | UI | Devolve resposta válida se a rede falha sem cópia local. | - | smoke real de batida pela PWA | ..+....++ | REGISTRADA (no ar antigo; homologação pendente) | BACKLOG:2064 |
| BO-ROTULO-REABRIR-SO-NO-DETALHE | UI | Oferece somente mudanças de estado realmente executáveis. | - | corte para construir | +........ | REGISTRADA | BACKLOG:2081 |

## CEREJA (24)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| JORNADA-EXCEPCIONAL-POR-DATA | CEREJA | Permite jornada excepcional para uma data específica. | - | 20/20; TROCA-DE-PLANTAO | .+....... | REGISTRADA | BACKLOG:762 |
| V3-DOBRA-SEM-NOTURNA | CEREJA | Corrige a dobra do calculador preservando motor e folha exportada. | - | localizar residual LMT | ..++..++~ | REGISTRADA (parte no ar; residual pendente) | BACKLOG:891 |
| V3-LAVRAR-ATRASO-CREDITO | CEREJA | Preserva o desconto do crédito ao gravar o atraso. | - | medir frota | ......... | REGISTRADA | BACKLOG:892 |
| V3-RETOMA | CEREJA | Retoma o novo cálculo de adicionais e folgas com impacto identificado. | - | certificação; autorização da troca | .+....... | REGISTRADA | BACKLOG:965 |
| V3-RETOMA-PERIODOS-DA-CELULA | CEREJA | Calcula períodos e pausas com a mesma fonte do espelho. | - | P4; autorização do motor | .++....~. | INTERROMPIDA | BACKLOG:966; anexo:interrompidas.txt:11 |
| V3-RETOMA-FILA | CEREJA | Completa a comparação e as fontes do novo cálculo. | - | pouso V3 | .++...... | INTERROMPIDA | BACKLOG:978; anexo:interrompidas.txt:13 |
| VETO-TRILHA-FRASE-DO-REABRIR | CEREJA | Descreve corretamente uma reabertura recusada no histórico. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:1019 |
| O-DIA-PAGO S5 | CEREJA | Constrói o calculador independente que poderá substituir o cálculo antigo. | - | S1-S2; S5a; S3; autorização da troca | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1132 |
| S5b-CALCULADOR-ESCREVE | CEREJA | Grava os resultados do calculador novo preservando a folha exportada. | - | autorização da troca | + + + . . . . . . | REGISTRADA (escrita exercida; troca aguarda autorização) | app/docs/BACKLOG.md:1141 |
| S5b-TROCA-NAO-CRIA-DESCONTO | CEREJA | Troca o calculador sem criar descontos ausentes no motor atual. | - | explicar pareamento e dupla contagem; novo DIFF; autorização | + + + . . . . . . | PARADA (PAREI no critério por colaborador) | app/docs/BACKLOG.md:1287 |
| S5b-4-REGRAS | CEREJA | Completa quatro regras do calculador usando funções existentes do motor. | - | autorização da troca; item 5 | + + + + . . . . . | PARADA (cinco de seis observáveis feitos; espera autorização) | app/docs/BACKLOG.md:1288 |
| O280 | CEREJA | Reúne as regras em um único cálculo de período. | - | Bateria; esmeril; exportação da 10; ! da troca | +++....~. | PARADA | BACKLOG:1565 |
| V3-BATERIA-DE-CASOS-PELA-REGRA | CEREJA | Confere o novo motor por casos derivados da regra escrita. | - | Perguntas de lei de intervalo e tolerância | ++++..... | REGISTRADA (recorte pronto; sem commit) | BACKLOG:1566 |
| O287 | CEREJA | Audita o motor novo antes da troca. | - | Esmeril sem abertos; ! da troca | +++...... | REGISTRADA | BACKLOG:1573 |
| O300 | CEREJA | Evita consultas repetidas alimentando o cálculo com dados carregados. | - | Troca O280; ! para v2 | ++....... | REGISTRADA | BACKLOG:1586 |
| O304 | CEREJA | Evita contar duas vezes a pontualidade de par duplicado. | - | ! da reâncora | ++....... | REGISTRADA | BACKLOG:1590 |
| O315 | CEREJA | Restringe a mensagem de par quebrado à classe correta. | - | O280 | ++....... | REGISTRADA | BACKLOG:1601 |
| O316 | CEREJA | Fecha cinco pendências antes da troca do motor. | - | ! B13/B14; troca para I17 | ++~....~~ | PARADA (entrega parcial) | BACKLOG:1602 |
| I28-NO-V2 | CEREJA | Mantém a cura revogada vinculada à troca do motor. | - | Troca da v3 | .+....... | PARADA (revogada; não construir) | BACKLOG:1622 |
| V3-BATERIA-O-RESTO-DOS-38 | CEREJA | Completa os cenários pendentes da bateria do motor v3. | - | O261; corte do degrau acima de duas horas | ++~...... | PARADA (corte pendente) | BACKLOG:1665 |
| V3-IMPACTO-COM-DONO | CEREJA | Mostra o impacto da troca de motor por responsável. | - | cobertura integral; autorização das exportadas; O10-B | +........ | REGISTRADA (medida incompleta) | BACKLOG:1800 |
| V3-AUTORIDADE2-DIVERGE | CEREJA | Explica por que duas leituras do motor dão horas diferentes. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1801 |
| V3-ALIMENTACAO-VAZIA | CEREJA | Declara dias sem pares para evitar pagamento sem lavratura. | - | autoridade dos dias | ......... | REGISTRADA | BACKLOG:1802 |
| V3-E1 | CEREJA | Certifica o motor v3 com bateria de cenários. | - | esclarecer motivo; revalidar bateria | ......... | INTERROMPIDA | BACKLOG:2084; anexo:interrompidas.txt:27 |

## ? (11)

| id | balde | entrega | resolve | depende de | grade | estado | fonte |
|---|---|---|---|---|---|---|---|
| ADMIN-ESCREVE-EMPRESA-FORA-DA-PORTA | ? (classificação não decidida) | Faz o administrador alterar a empresa pelo caminho auditável da produção. | - | decidir forma; contratos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1199 |
| FOLGA-TRABALHADA-TRES-NUMEROS | ? (classificação não decidida) | Explica a diferença entre três totais de folga trabalhada. | - | causa da diferença; autorização financeira | + . + . . . . . . | PARADA (medida; espera autorização) | app/docs/BACKLOG.md:1282 |
| FALTA-ACIMA-DO-PREVISTO-[nome removido] | ? (dono não medido) | Investiga falta descontada acima da jornada prevista. | - | Medir dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1540 (id anonimizado) |
| CARIMBO-QUE-A-CELULA-DESMENTE | ? (dono não medido) | Investiga respostas retiradas da fila com problema ainda no dia. | - | Medir dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1617 |
| LAVRA-REGUA-VAZIA-NO-GRAVADO-DA-09 | ? (dono não medido) | Investiga linhas pagas sem identificação da régua. | - | Medir dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1640 |
| NOITE-0411 | ? (ordem com vários baldes) | Ordena entregas de juízes, leitores, otimização e motor. | - | sequência dos itens 1–9 | .+....... | REGISTRADA (ordem substituída) | BACKLOG:1781; BACKLOG:1782 |
| MANHA-0511 | ? (ordem com vários baldes) | Ordena as entregas de fila e infraestrutura. | - | sequência dos itens 1–8 | .+.....++ | REGISTRADA (ordem substituída) | BACKLOG:1782; BACKLOG:1786 |
| ORDEM-POS-E4A2 | ? (ordem com vários baldes) | Define a sequência de correções após E4A2. | - | BOs; E4B; E4C; E4D | .+....... | REGISTRADA (sequência alterada depois) | BACKLOG:1783; BACKLOG:1784 |
| PLANO-JANELA-POS-RESET | ? (ordem com vários baldes) | Define a fila e os limites após o reset. | - | auditoria; sequência registrada | .+....... | REGISTRADA (auditoria reposicionada pelo aval atual) | BACKLOG:1784; anexo:aval.txt:1 |
| ORDEM-VIVA-0811 | ? (ordem com vários baldes) | Prioriza fio, leitores e cálculo na sequência definida. | - | E1→E2→E3; ATA-X-TURNO-PAUSA | .+.....+. | REGISTRADA | BACKLOG:1786 |
| FUNIL-28-PROCEDENTES | ? (áreas não discriminadas) | Guarda achados procedentes para abertura de obras. | - | nomear padrão por obra | ......... | REGISTRADA | BACKLOG:2056 |

## Rodape da leitura (por faixa)

### faixa p1

excluidas: 27 (encerramento anterior a 13/11 sem reabertura ou pendência posterior identificada na mesma célula: BACKLOG:752, BACKLOG:754, BACKLOG:755, BACKLOG:772, BACKLOG:781, BACKLOG:785, BACKLOG:788, BACKLOG:796, BACKLOG:810, BACKLOG:820, BACKLOG:828, BACKLOG:833, BACKLOG:841, BACKLOG:842, BACKLOG:843, BACKLOG:847, BACKLOG:858, BACKLOG:886, BACKLOG:887, BACKLOG:888, BACKLOG:914, BACKLOG:915, BACKLOG:924, BACKLOG:941, BACKLOG:944, BACKLOG:950, BACKLOG:1042) ; duplicatas: nenhuma por id literal nesta faixa do bloco OBRAS (app/docs/BACKLOG.md:728–2105) ; divergencias entre fontes: FERIADO-ESCALA -- BACKLOG:728 conserva a proposta superada, app/docs/RELATO.md:135 mantém o juiz de 16/09 intocado; DESLIGADOS-RETIDOS-CONTADOR -- BACKLOG:732 diz suíte na fila, app/docs/RELATO.md:103 registra cura na cópia e app/docs/RELATO.md:54 registra seu pouso/deploy; RAIA-PREPARACAO -- BACKLOG:739 diz REGISTRADA, app/docs/RELATO.md:12 já entrega o item 4; BO-[nome]-1311-ROTEIRO -- BACKLOG:740 registra lançamento, app/docs/RELATO.md:37 entrega roteiro parcial; LEI-DESLIGADO-NAO-TRAVA -- opus citado com 18 movimentos, medição posterior com 20, causa [nao verificado] (app/docs/RELATO.md:55); ADR-0003-CELULA-TRES-NATUREZAS -- BACKLOG:835 inclui fotografia na célula, BACKLOG:836 a situa no gravado; E4-CALENDARIO-L1 -- BACKLOG:856 diz INTERROMPIDA antiga, BACKLOG:878 registra NO AR em 03/11 e app/docs/RELATO.md:37 acrescenta complemento em 13/11; E4-APP-MAPA -- BACKLOG:885 mantém registro aguardando leitores, app/docs/RELATO.md:1720 registra NO AR antigo; O261-E-F5B-JUIZ-PORTA-ALCANCAVEL -- descrição exige antes da F5, estado diz depois da F5 (BACKLOG:895); NOREVERSE-UI-NO-CORE -- BACKLOG:953 mantém construção futura, app/docs/RELATO.md:1810 registra pouso/deploy com resultado posterior pendente; CODE-CHAMA-CODEX -- BACKLOG:1058 diz em construção, BACKLOG:1056 o cita no ar. Expansão das abreviações da grade usada na consolidação: censo–lei–construção–R/G–mutantes–verificador–opus–pouso–ar [nao verificado: o aval não expande as abreviações]. Identidade do HEAD da cópia [nao verificado: cópia sem metadados Git].

### faixa p2

excluidas: 105 obras únicas (106 ocorrências, contando as duas de O81): terminal anterior a 13/11, sem reabertura ou parte pendente posterior identificada; datas incertas e pendências conservadas foram incluídas. Universo: 336 ocorrências de obras, descontados cabeçalho e separador; 335 ids únicos; 230 linhas incluídas. Fontes: app/docs/BACKLOG.md:1073–1417; cabeçalho e separador em app/docs/BACKLOG.md:1190–1191. Datas cruzadas: SEM-CELULA-NA-JANELA-INTEIRA fechada pelo mesmo hash publicado em 25/10 (app/docs/BACKLOG.md:1089; app/docs/BACKLOG.md:1664); E5-FINAL absorvida pela S4 fechada em 30/09 (app/docs/BACKLOG.md:1168; app/docs/BACKLOG.md:1129).

duplicatas: O81 — app/docs/BACKLOG.md:1179 e app/docs/BACKLOG.md:1383; consolidada e excluída uma única vez como obra.

divergencias entre fontes:
- O81 -- app/docs/BACKLOG.md:1179 diz FECHADA em 28/09; app/docs/BACKLOG.md:1383 conserva planejamento anterior e remete ao estado da primeira ocorrência.
- PLACAR-ESTRUTURAL -- app/docs/BACKLOG.md:1173 diz 3 fechados e 3 parciais de 6; o placar acrescenta BATERIA e SOMA ao principal e separa R1 como termômetro, correspondendo ao 3 fechados e 4 parciais de 7 do anexo ESTADO (app/core/placar_estrutural.py:74; app/core/placar_estrutural.py:118; app/core/placar_estrutural.py:123).
- O145 -- a mesma linha diz NO AR/CURADA e também “não começada” (app/docs/BACKLOG.md:1170).
- O214 -- a mesma linha registra etapas no ar e conserva “nada construído” em outra célula (app/docs/BACKLOG.md:1172).
- DISPUTA-S84-RETRO-PASSIVO -- uma célula espera autorização; outra registra autorização dada (app/docs/BACKLOG.md:1144).
- WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA -- estado CONGELADA; outra célula informa absorção e ausência de item próprio (app/docs/BACKLOG.md:1119).
- E6-14/O73b/CORTE-B-30 -- estados CONGELADOS nas respectivas linhas; PLACAR-E registra E6-14=19, O73b curado e CORTE-B-30=16 (app/docs/BACKLOG.md:1162; app/docs/BACKLOG.md:1166; app/docs/BACKLOG.md:1167; app/docs/BACKLOG.md:1176).
- O221 -- uma célula diz três pousos no ar; outra diz “EM FILA, nada juntado” (app/docs/BACKLOG.md:1215).
- O223 -- uma célula registra pousos no ar; outra conserva “nada construído” (app/docs/BACKLOG.md:1198).
- O218 -- BACKLOG conserva bateria incompleta; placar declara bateria turno/marcos com 16 cenários e 64 testes (app/docs/BACKLOG.md:1214; app/core/placar_estrutural.py:74). Equivalência integral dos escopos: [nao verificado].
- GESTAO-HE-FATIA-2-LOTE-E-LIMITE -- descrição pede default 10 e efeito de tela; localização registra entrega pela O214 com default 15 e efeito real (app/docs/BACKLOG.md:1269).
- O35 -- BACKLOG conserva 12/22; placar declara 19/20 (app/docs/BACKLOG.md:1316; app/core/placar_estrutural.py:440).
- O42 -- descrição diz itens 1/2 curados; estado diz “1 e 2 em cura” (app/docs/BACKLOG.md:1323).
- O65 -- BACKLOG conserva obra livre; placar declara FEITO/aplicada (app/docs/BACKLOG.md:1350; app/core/espelho_verdade.py:77).
- O73 -- BACKLOG conserva complemento dos resíduos; placar declara FEITO o núcleo da volta do intervalo (app/docs/BACKLOG.md:1360; app/core/espelho_verdade.py:117).
- O76 -- BACKLOG conserva medição por fazer; placar declara aplicada em 27/09 (app/docs/BACKLOG.md:1363; app/core/espelho_verdade.py:80).
- O89 -- BACKLOG espera corte de desenho; placar declara no ar em 27/09 (app/docs/BACKLOG.md:1378; app/core/espelho_verdade.py:109).
- O91 -- cabeçalho diz FECHADA pelo corte “não instala”; estado ainda espera autorização para instalar (app/docs/BACKLOG.md:1376).
- O139 -- uma célula registra produto no ar e selo no pouso 2; outra diz “REGISTRADO, nada construído” (app/docs/BACKLOG.md:1393).
- O137 -- uma célula diz nenhuma família em zero; outra registra a primeira família fechada (app/docs/BACKLOG.md:1390).
- O30 -- linha própria aguarda vale da F1; O162 afirma que o vale abriu a F2 (app/docs/BACKLOG.md:1311; app/docs/BACKLOG.md:1416).

Grade: “cen” interpretado como censo e “con” como construção; expansão literal dessas abreviações: [nao verificado]. Em obras parciais, “+” em pouso/ar refere-se à parte explicitamente entregue, conforme a nota de estado e a fonte da linha.

### faixa p3

excluidas: 70 (estado terminal anterior a 13/11, sem reabertura/PAREI/parte pendente posterior na mesma célula; pendência apenas em outra coluna não preserva a obra). Fontes BACKLOG:1437,1441,1446,1450,1452,1455,1462,1463,1471,1479,1485,1489,1491,1492,1494,1514,1516,1519,1531,1543,1547,1548,1557,1563,1572,1574,1575,1577,1578,1581,1593,1595,1599,1615,1624,1668,1669,1671,1674,1676,1677,1678,1679,1681,1688,1693,1695,1696,1697,1698,1699,1700,1701,1702,1703,1706,1731,1732,1735,1736,1738,1740,1741,1742,1745,1746,1747,1748,1753,1761 ; duplicatas: nenhuma de id literal nas linhas BACKLOG:1418 a BACKLOG:1762 ; divergencias entre fontes: O169 -- BACKLOG:1424 mantém pergunta de lei, BACKLOG:1821 registra pouso em 05/11 e BACKLOG:2076 diz pousada; O248 -- BACKLOG:1480 diz nada construído, BACKLOG:1488 registra nascimento do juiz e item 1 no ar; O253 -- BACKLOG:1485 conserva ABERTO e POUSADA sem reabertura posterior; O260 -- BACKLOG:1490 conserva EM CURSO, BACKLOG:1494 informa pouso c2ab8cf5; CONTADOR-PAREADOR-X-CELULA -- BACKLOG:1510 conserva suíte em curso, BACKLOG:1506 informa par 1 no ar; CONTADOR-AUSENCIA-X-VEREDITO -- BACKLOG:1511 conserva casos exportados esperando autorização, BACKLOG:1506 informa par 2 no ar; O264 -- BACKLOG:1549 conserva corte inicial favorável à dobra e decisão posterior contrária, encaminhada à O277 em BACKLOG:1562; O280 -- BACKLOG:1565 conserva liberação para trocar antes do export e corte posterior que mantém a obra parada; SUPRA-JUIZ-AINDA-DERIVA-POR-DATA -- BACKLOG:1655 diz nada feito, BACKLOG:1677 registra migração dos ramos com turno; ADMIN-RESOLVE-FLAP -- BACKLOG:1694 conserva causa aberta, BACKLOG:1697 registra causa curada e trilha_x_banco zerada; ATA-X-TURNO -- BACKLOG:1695 diz duas metades publicadas e conserva entrada como pergunta de lei em outra coluna; FOTO-X-VIVO -- BACKLOG:1742 registra zero fotos paradas e conserva pendência de uma empresa em outra coluna; BUG-LASTRO-VAI-E-VOLTA e BUG-LASTRO-CLASSE-2 -- BACKLOG:1756 e BACKLOG:1757 conservam andamento e dependência antigos, BACKLOG:1686 registra absorção pela REABRIR-PERGUNTA-A-PREMISSA.

### faixa p4

excluidas: 20 (terminais anteriores a 13/11, sem reabertura ou parte pendente posterior própria: BACKLOG:1764,1778,1791,1793,1794,1795,1796,1799,1817,1818,1821,1843,1871,1872,1887,1901,1961,1987,2015,2040) ; duplicatas: VERIFICADOR-EM-FABLE — BACKLOG:1785 e BACKLOG:1911; BUG-PLANTIO-NOTURNO-MAIS-1 — BACKLOG:1852 e BACKLOG:1975 ; divergencias entre fontes: JUSTIFICATIVA-PAGA-O-PERIODO -- BACKLOG:1875 diz retorno à ordem viva, anexo:interrompidas.txt:25 mantém INTERROMPIDA; OTIMIZA-1 -- BACKLOG:2086 e anexo:interrompidas.txt:31 dizem INTERROMPIDA, RELATO:25 registra brief pronto sem retomada comprovada; O168-GUARDA-NO-GATE-DO-PAI e C20-O168 -- BACKLOG:1942 e BACKLOG:2049 dizem construção, RELATO:5 e RELATO:7 registram pouso e NO AR em 13/11; mantidos separados por IDs distintos; TIPO-ITEM1-DIMENSAO -- BACKLOG:1838 prevê pouso, BACKLOG:1836 registra item 1 pousado em 12/11; LASTRO-PARTE-3 -- BACKLOG:1790 contém POUSADO e ABERTA; CODEX-CONSTRUTOR-NEGOCIO -- BACKLOG:1803 contém POUSADO e ABERTA; E-SEM-VIVA-B -- BACKLOG:2045 contém fechamento antigo, B3 antigo e superação parcial em 13/11; MAPA-20-DERIVADO -- BACKLOG:1964 prevê retorno com E3 em suite, anexo:aval.txt:1 determina substituição pela GRADE-MESTRA; PLANO-JANELA-POS-RESET -- BACKLOG:1784 coloca auditoria após reset, anexo:aval.txt:1 a reposiciona após o pouso da E3; HOLERITE-ZIP-MATRICULA -- BACKLOG:1883 contém uso medido e uso em produção não medido; tentativa anterior -- anexo:tentativa1.r troca as âncoras dos dois IDs O168: na cópia O168-GUARDA está em BACKLOG:1942 e C20-O168 em BACKLOG:2049.

