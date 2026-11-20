# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 11:56** · publicado em **20/11/2026 11:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 11:56 · sessao f9a1536028af · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Cópia indicada conferida. A identidade Git de HEAD/main e os logs originais citados nos documentos ficam [nao verificado].

## FAIXA-NO-CADASTRO -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta o aval para executar depois do contrato 20. O campo e a migração a partir do mapa atual já estão descritos. app/docs/BACKLOG.md:2334
Q2: Quando chegar a vez dessa mudança, você autoriza que o administrador cadastre qual ausência prevalece quando duas coincidem? app/docs/BACKLOG.md:2334
Q3 (a): Autorizar na vez registrada. Tela ganha o campo de prioridade. Na folha, a migração reproduz a prioridade atual e alterações posteriores podem mudar qual ausência prevalece. Alcance hoje: nao medido. Construir campo, migração e leitor único. app/docs/BACKLOG.md:2334, app/ponto/precedencia.py:52,381
Q3 (b): Adiar o aval. Tela com o formulário atual e folha pela prioridade vigente. Alcance hoje: nao medido. Nenhuma construção nesta etapa. app/templates/ponto/partials/painel_tipo_ausencia.html:28, app/ponto/precedencia.py:76
Q4: nao respondida quanto à execução. MANHA-1211 item 7 autorizou o registro do desenho. app/docs/BACKLOG.md:2334, app/docs/RELATO.md:2197
Q5: vira complemento de EFEITO-NONE-UM-LEITOR, na posição registrada após o contrato 20 e com aval próprio. app/docs/GRADE-MESTRA.md:1325,1328, app/docs/BACKLOG.md:2334

## DISPUTA-FECHADA-SEM-RESOLUCAO -- classe: resto -- tipo: corte
Q1: "sem pergunta". O registro pedia um corte para garantir que o encerramento deixasse o motivo na trilha. app/docs/BACKLOG.md:1222, app/docs/GRADE-MESTRA.md:1049
Q2: Como o administrador deve saber por que uma disputa foi encerrada quando a explicação está em branco? app/docs/BACKLOG.md:1222
Q3 (a): Usar o motivo que já fica na trilha dos novos encerramentos. Tela atual e efeito direto zero na folha. Alcance hoje: nao medido. Nenhuma nova regra de encerramento. app/chamados/models.py:2059,2095,2106
Q3 (b): Exibir no detalhe dos registros antigos “motivo não registrado” quando faltar a informação. Efeito zero na folha. Alcance hoje: nao medido. Em 30/09, 30 de 4.682 fechadas tinham o texto vazio. Construir o aviso. app/docs/BACKLOG.md:1222, app/chamados/migrations/0049_disputa_motivo_fechamento.py:15
Q4: ja respondida pelo corte C7-DISPUTA-FECHA-POR-UM-JUIZ, com L-007, para a trilha dos novos encerramentos. O código grava o motivo estruturado e exige texto na porta de encerramento manual. app/docs/BACKLOG.md:1618, app/docs/LEIS.md:65, app/chamados/models.py:2095,2106
Q5: pode morrer como obra avulsa de encerramento novo. A migração admite motivo vazio no legado, e o registro original preserva o histórico. app/chamados/migrations/0049_disputa_motivo_fechamento.py:15, app/docs/BACKLOG.md:1222

## VIGENCIA-SOBREPOSTA-COL515 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta decidir como tratar dois vínculos históricos inativos que cobrem os mesmos dias com horários opostos. app/docs/BACKLOG.md:1223
Q2: Quando o histórico de uma pessoa mostra dois horários para os mesmos dias, o administrador deve preservar os dois registros ou corrigir as datas quando houver prova? app/docs/BACKLOG.md:1223
Q3 (a): Preservar as datas e avisar ou recusar se uma reativação criar conflito. Admin vê o motivo ao tentar reativar. Folha sem recálculo por essa opção. Alcance hoje: nao medido. O registro de 30/09 traz 1 pessoa, 2 vínculos e 11 dias. Construir a guarda na reativação. app/docs/BACKLOG.md:1223, app/colaboradores/services/vinculo.py:505,523
Q3 (b): Corrigir o histórico com prova e trilha. Admin vê o período corrigido. Efeito na folha: nao medido. Alcance hoje: nao medido, com o mesmo caso registrado em 30/09. Construir a correção pela porta existente, respeitando a regra da folha já entregue. app/docs/BACKLOG.md:1223,1248, app/docs/LEIS.md:125,155
Q4: nao respondida para o histórico inativo. VINCULO-LINHA-DO-TEMPO trata a linha ativa, que é o universo do censo atual do código. app/docs/BACKLOG.md:1248, app/colaboradores/services/vinculo.py:505,523
Q5: vira complemento de VINCULO-LINHA-DO-TEMPO. O corte sobre o histórico inativo segue pendente. app/docs/GRADE-MESTRA.md:1078,1082, app/docs/BACKLOG.md:1223

## O62 -- classe: espurias/HE -- tipo: corte
Q1: "sem pergunta". Falta corrigir a resposta que diz “escala sem intervalo” quando a duração está cadastrada e faltam os horários da pausa no registro do dia. app/docs/BACKLOG.md:1469, app/ponto/juiz_batida.py:294,304
Q2: Quando a escala prevê uma pausa e o registro do dia não traz os horários dela, como a pausa deve aparecer para o administrador e contar na folha? app/docs/BACKLOG.md:1469, app/docs/LEIS.md:80
Q3 (a): Corrigir primeiro o que se informa. Admin vê a duração prevista e que faltam horários. Efeito imediato zero na apuração. Alcance hoje: nao medido. Na amostra original, 3 de 5 pessoas tinham dias assim, incluindo 17 de 20 dias de col87. Construir a correção da resposta e da exibição. app/docs/BACKLOG.md:1469, app/ponto/juiz_batida.py:304,309
Q3 (b): Corrigir a exibição e aplicar a duração cadastrada onde a apuração ainda a omite. Admin distingue pausa prevista de pausa registrada. A folha desconta a pausa nesses casos. Alcance hoje: nao medido. Construir a correção na fonte e medir por rubrica. app/docs/LEIS.md:80,193, app/docs/RELATO.md:496
Q4: ja respondida pelo corte INTERVALO-CADASTRADO-NAO-VIRA-HE, L-032. A regra escrita manda descontar da jornada a duração cadastrada. app/docs/LEIS.md:80
Q5: segue para execução. A proteção anterior do motor cobriu o caso documentado, e o retorno incorreto ainda existe na cópia. app/docs/RELATO-ARQUIVO.md:6405,6409,6412, app/ponto/juiz_batida.py:304

## O310-RAIOX -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta o corte específico para guardar o histórico, com modelo e migração. A lente está registrada como publicada. app/docs/BACKLOG.md:1727, app/folha/retrato_folha.py:723
Q2: Para comparar a situação de ontem com a de hoje, o administrador precisa só da foto anterior ou também de um histórico por data? app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q3 (a): Guardar a foto anterior. Admin vê o comparativo de entradas e bloqueios por responsável. Efeito zero na folha. Alcance hoje: nao medido. Construir armazenamento da foto anterior, migração e comparação. app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q3 (b): Guardar fotos datadas. Admin vê o mesmo comparativo, com consulta às datas guardadas. Efeito zero na folha. Alcance hoje: nao medido. Construir histórico por data, migração e consulta. app/docs/CORTES.md:357, app/folha/retrato_folha.py:725
Q4: nao respondida quanto ao armazenamento. O256-RAIO-X-DO-RETRATO já manda ler duas fotos gravadas e informar quando falta a anterior. app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q5: vira complemento de O310. A lente foi publicada com diferença de folha zero registrada. Resta o histórico. app/docs/BACKLOG.md:1727

## O34 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta escolher o tratamento dos ajustes que deixam de encaixar quando o código muda. O registro traz três propostas de desenho. app/docs/BACKLOG.md:1436, app/docs/GRADE-MESTRA.md:1238
Q2: Quando uma correção pronta deixa de encaixar porque o sistema mudou, ela deve se refazer sozinha, mostrar o que mudou ou ter prazo para ser refeita ou descartada? app/docs/BACKLOG.md:1436
Q3 (a): Refazer o ajuste automaticamente. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. Construir a reconstrução automática e sua conferência. app/docs/BACKLOG.md:1436
Q3 (b): Parar a aplicação e mostrar ao operador o trecho que mudou. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. Construir o diagnóstico com a comparação. app/docs/BACKLOG.md:1436
Q3 (c): Dar prazo e registrar o ato de refazer ou descartar. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. A passada registrada achou 11 de 14 falhas dessa família e 17 ajustes em espera. Construir prazo e destinos na fila. app/docs/BACKLOG.md:1436
Q4: nao respondida. ZUMBIDO trata a segunda falha pelo mesmo motivo e veta a terceira tentativa. A escolha entre as três propostas segue aberta. app/docs/CORTES.md:307, app/docs/BACKLOG.md:1436
Q5: vira complemento de O31, como já agrupado na Grade. app/docs/GRADE-MESTRA.md:1234,1238

## O37 -- classe: resto -- tipo: !
Q1: "o gravado de competencia exportada e reescrito, ou fica e a diferenca vira Pauta DP?" app/docs/RELATO-ARQUIVO.md:24120
Q2: A folha de agosto deve ser refeita por causa dos dias de plantão registrados errado ou a diferença deve ir ao departamento pessoal? app/docs/RELATO-ARQUIVO.md:24098,24119
Q3 (a): Preservar a folha e encaminhar a diferença ao DP. Admin sem tarefa nova na tela. Alcance hoje: nao medido. A medida registrou 22 dias de 2 pessoas e 161 h de diferença de jornada prevista em agosto. Depois, preparar a linha para o DP. app/docs/RELATO-ARQUIVO.md:24089,24100, app/docs/RELATO.md:1012
Q3 (b): Reescrever pela porta de retificação. Admin veria os dias corrigidos e a folha seria recalculada. Alcance hoje: nao medido, com os mesmos 22 dias na medida histórica. Efeito por rubrica: nao medido. Depois, conferir e comparar os valores antes e depois pela porta existente. app/docs/RELATO-ARQUIVO.md:24091,24119, app/ponto/portas/regen_exportada.py:87
Q4: ja respondida pelo corte COMPLEMENTO-1 RESPOSTAS-R4-E1PORTA-1611 item 3, de 16/11. Passivo exclusivamente anterior a setembro vai à lista do DP. app/docs/RELATO.md:1012, app/docs/BACKLOG.md:1439
Q5: vira complemento de O54, sem espera avulsa pelo Ronald para os casos medidos de agosto. Entrada efetiva na lista do DP [nao verificado]. app/docs/BACKLOG.md:1460, app/docs/RELATO.md:1012

## O296 -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o aval nominal para trocar a identificação pessoal no comentário do arquivo de cálculo protegido. app/docs/BACKLOG.md:1704, CLAUDE.md:142
Q2: Você autoriza retirar a identificação pessoal também do comentário dentro do cálculo da folha? app/docs/BACKLOG.md:1704
Q3 (a): Substituir a identificação somente fora do arquivo protegido. Efeito zero na tela e na folha. Alcance hoje: nao medido. A medição de 16/10 registrou 9 comentários fora. Depois, fazer as substituições de texto. app/docs/BACKLOG.md:1704
Q3 (b): Incluir o comentário do arquivo protegido com aval nominal. Efeito zero na tela e na folha. Alcance hoje: nao medido. A medição registrou 10 comentários ao todo. Depois, fazer as substituições de texto nos sítios conferidos. app/docs/BACKLOG.md:1704, CLAUDE.md:142
Q4: nao respondida. A O295 tratou a publicação, e o registro da O296 mantém o aval específico para o arquivo protegido. app/docs/BACKLOG.md:1703,1704
Q5: segue pelo `!` nominal do décimo comentário. app/docs/BACKLOG.md:1704, app/docs/GRADE-MESTRA.md:410

## REJULGAR-61-COBPARC -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` próprio, com a lista das 55 atas de competências entregues. app/docs/BACKLOG.md:906, app/docs/GRADE-MESTRA.md:657
Q2: Você autoriza revisar os registros antigos de ausências parciais dos meses que já foram entregues? app/docs/BACKLOG.md:906
Q3 (a): Preservar os registros e levar diferenças comprovadas ao DP. Admin vê os registros atuais, e a folha entregue fica preservada. Alcance hoje: nao medido. Em 08/11, eram 61 atas, 55 entregues e 6 trancadas. Depois, preparar a lista e os comparativos. app/docs/BACKLOG.md:906, app/docs/LEIS.md:125,145
Q3 (b): Autorizar revisão nominal. Admin vê os registros revistos com motivo e trilha. Eventual retificação da folha entregue passa pela porta própria. Alcance hoje: nao medido, com 55 atas no lote documentado do `!`. Depois, conferir o lote e reutilizar a porta. app/docs/BACKLOG.md:906, app/ponto/portas/regen_exportada.py:87,111,116
Q4: nao respondida. TRES-DO-TOPO-1109 item 2 decidiu a regra da COBPARC e reservou o `!` próprio do acervo. app/docs/RELATO.md:2920, app/docs/BACKLOG.md:906
Q5: segue. O lote inclui setembro e foi registrado como obra separada da COBPARC. app/docs/BACKLOG.md:906, app/docs/RELATO.md:1012

## RESIDUO-PREMIG-NO-BIND-MOUNT -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para apagar os arquivos antigos da instalação em uso. app/docs/BACKLOG.md:1761, app/docs/GRADE-MESTRA.md:421
Q2: Você autoriza remover os arquivos antigos da instalação ou quer mantê-los onde estão? app/docs/BACKLOG.md:1761
Q3 (a): Manter os arquivos. Efeito zero na tela e na folha. Alcance hoje: nao medido. Em 19/10 havia 64 arquivos, 28 fora das pastas de estáticos, logs e mídia. Depois, usar o censo já corrigido. app/docs/BACKLOG.md:1761, app/docs/RELATO-ARQUIVO.md:43426
Q3 (b): Conferir a lista e remover os arquivos autorizados. Efeito previsto zero na tela e na folha. Alcance hoje: nao medido, com 64 arquivos na medição histórica. Depois, executar a limpeza sobre o inventário conferido. app/docs/BACKLOG.md:1761, app/docs/LEIS.md:67
Q4: nao respondida quanto a apagar. L-009 exige autorização para a remoção. app/docs/LEIS.md:67
Q5: pode morrer como bloqueio do censo. A correção passou a classificar os ignorados e registrou `EXTRA-VIVO=0`. Remoção efetiva [nao verificado]. app/docs/RELATO-ARQUIVO.md:43426

## JANELA-DO-NOTURNO-CRAVADA -- classe: espurias/HE -- tipo: !
Q1: "sem pergunta". Falta o aval para tocar o cálculo protegido e permitir cadastrar os horários noturnos da convenção. app/docs/BACKLOG.md:1769, CLAUDE.md:142
Q2: Você autoriza que o administrador cadastre o horário noturno de cada convenção? app/docs/BACKLOG.md:1769
Q3 (a): Usar a faixa fixa de 22h a 05h. Tela com o cadastro atual e folha pela faixa fixa. Casos com horário diferente hoje: nao medido. Nenhuma construção. app/docs/BACKLOG.md:1769, app/ponto/motor_calculo_v2.py:99,103
Q3 (b): Cadastrar início e fim na tela. A folha pode mudar o adicional e a parte noturna da hora extra onde a convenção usar outra faixa. Alcance hoje: nao medido. Construir os campos, a leitura comum e os testes de cálculo. app/docs/LEIS.md:64, app/ponto/motor_calculo_v2.py:983,1014
Q4: nao respondida quanto ao `!`. L-006 já define o cadastro pela tela para regras que variam. app/docs/LEIS.md:64, CLAUDE.md:142
Q5: segue. O cálculo ainda fixa os horários e a alteração exige aval explícito. app/ponto/motor_calculo_v2.py:101,103, app/docs/BACKLOG.md:1769

## PROTEGE-VAZIA-EM-55-LEIS -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar linhas de leis já escritas. app/docs/BACKLOG.md:1755, app/docs/RELATO-ARQUIVO.md:299
Q2: Você autoriza revisar as regras publicadas e marcar os trechos de código que elas protegem expressamente? app/docs/LEIS.md:43, app/docs/RELATO-ARQUIVO.md:299
Q3 (a): Deixar a coluna como está. Efeito zero na tela e na folha. Alcance na cópia: 55 linhas com sítio na coluna de quem aplica e proteção vazia. Total elegível: nao medido. Nenhuma construção. app/docs/LEIS.md:57, bin/tests/test_lei_protege_sitio.sh:66,77
Q3 (b): Autorizar a revisão linha a linha. Efeito zero na tela e na folha. Alcance na cópia: as mesmas 55 linhas candidatas. Total elegível: nao medido. Depois, preencher as proteções explícitas e conferir a trava em ato próprio. app/docs/LEIS.md:43,57,131, bin/tests/test_lei_protege_sitio.sh:77
Q4: nao respondida quanto ao `!`. LEI-PROTEGE-SITIO já define o critério: preencher quando o texto da lei protege expressamente o sítio. app/docs/LEIS.md:43,48, app/docs/BACKLOG.md:1755
Q5: segue. A L-097 exemplifica proteção explícita com a coluna vazia. app/docs/RELATO-ARQUIVO.md:780, app/docs/LEIS.md:115

RESUMO: 1 sobe ao Ronald como lei, sobre vigências históricas inativas (app/docs/BACKLOG.md:1223). 3 já respondidas, DISPUTA-FECHADA-SEM-RESOLUCAO, O62 e O37 (app/docs/BACKLOG.md:1618, app/docs/LEIS.md:80, app/docs/RELATO.md:1012). 7 podem morrer ou virar complemento (app/docs/GRADE-MESTRA.md:1049,1082,1238,1328, app/docs/BACKLOG.md:1460,1727, app/docs/RELATO-ARQUIVO.md:43426). 0 fecham por evidência de uso pelo corte de 16/11, que tratou de outras obras (app/docs/BACKLOG.md:2367, app/docs/RELATO.md:1037).

## 20/11 11:56 · sessao a4b0bc5e8dec · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Li a cópia fornecida em somente leitura. A confirmação do hash como HEAD de `main` fica [nao verificado], pois a cópia não tem metadados Git.

## O275 -- classe: espurias/HE -- tipo: corte
Q1: “O FLAG NOVO DA O275 PODE SER LIGADO NUM TIPO DE PLANTAO (`12x36`/`24x48`), OU O PLANTAO RESPONDE SO PELA PRECEDENCIA QUE JA EXISTE?” app/docs/RELATO-ARQUIVO.md:746
Q2: O administrador pode tornar um feriado dia de trabalho quando ele cai na folga de um plantão? app/docs/RELATO-ARQUIVO.md:748
Q3 A: Admin pode usar a opção também nos plantões, salvo intermitente. A folha passa a contar o feriado como dia previsto onde a opção estiver ligada. Alcance hoje nao medido. O censo registrou 337 vínculos 12x36 potencialmente expostos. Atualiza e confere a implementação guardada. app/docs/RELATO-ARQUIVO.md:749–753
Q3 B: Admin usa a opção em escalas semanais, 6x1 e 5x2. A folha dos plantões mantém a previsão atual. Alcance hoje nao medido. O censo apontou 1 vínculo no alvo e 4 tipos cadastrados como 12x36 fora do alcance. Acrescenta a restrição por ciclo e ajusta os testes. app/docs/RELATO-ARQUIVO.md:749–753
Q4: nao respondida. SDF-FERIADO-E-DIA autoriza o cadastro. A escolha entre A e B permanece aberta. app/docs/BACKLOG.md:1682 e app/docs/RELATO.md:417
Q5: segue. A publicação espera A ou B, e a O277 depende desse cadastro para tratar a dobra. app/docs/BACKLOG.md:1682 e app/docs/BACKLOG.md:1684

## O10-D-PERMUTA-RECIPROCA -- classe: resto -- tipo: corte
Q1: “(a) DNA carrega marcos do cedente ou (b) d1=d2 fora da porta”. O RELATO também lista tipo e aprovação da carta, falha de gravação, “cede so plantao”, descanso e o ! de escala. O literal desses cortes e do ! fica [nao verificado]. app/docs/RELATO.md:886–887
Q2: Numa troca no mesmo dia, quem assume recebe os horários de quem cede ou o sistema recusa a troca? app/docs/RELATO.md:887
Q3 A: A operação aceita a troca e mostra os horários assumidos ao admin. A folha trata o plantão assumido como dia normal com os adicionais cabíveis. Alcance hoje nao medido. Implementa o transporte dos horários do cedente. app/docs/RELATO.md:887 e app/docs/CORTES.md:286
Q3 B: A operação mostra a recusa ao admin. A folha conserva os plantões originais. Alcance hoje nao medido. Implementa a recusa, condicionada à revisão do corte que já permite a mesma data. app/docs/RELATO.md:887 e app/docs/RELATO.md:2719
Q4: ja respondida pelo corte LEI-O10-D-PERMUTA quanto a permitir a mesma data, reafirmado no preparo de 16/11. O desenho dos horários e os demais itens listados na revisão seguem abertos. app/docs/RELATO.md:2719 e app/docs/RELATO.md:1055 e app/docs/RELATO.md:886–887
Q5: segue. A cópia aprovada ainda aguarda os cortes residuais e o ! de escala. app/docs/RELATO.md:879 e app/docs/RELATO.md:886

## O14 -- classe: resto -- tipo: smoke
Q1: “sem pergunta”. Falta “SMOKE de olho no app de um noturno 12x36 (ex.: col174 ou col788) depois do deploy.” app/docs/PENDENTES_RONALD.json:1587
Q2: No celular do trabalhador noturno, a saída da madrugada e as horas aparecem junto com o restante do plantão? app/docs/PENDENTES_RONALD.json:1587
Q3 A: Se a conferência passar, o admin conserva a tela atual, o colaborador vê o turno no dia correto e a folha conserva os valores. Alcance hoje nao medido. Registra a conferência do aparelho. app/docs/BACKLOG.md:1414 e app/api/views.py:1440–1453 e app/api/views.py:1475–1485
Q3 B: Se falhar, o admin recebe o dia e a divergência vistos no celular. A conferência é uma leitura da folha. Alcance hoje nao medido. A correção depende do erro observado [nao verificado]. app/docs/PENDENTES_RONALD.json:1587 e app/docs/RELATO-ARQUIVO.md:27292–27293
Q4: nao respondida quanto à conferência no aparelho. app/docs/BACKLOG.md:1414
Q5: segue. Os 126 de 368 colaboradores com aviso diferente são da medição do servidor. O fechamento por uso de 16/11 registra outras operações. app/docs/RELATO-ARQUIVO.md:27288–27293 e app/docs/RELATO.md:1037

## O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL -- classe: fio morto -- tipo: corte
Q1: “sem pergunta” de mérito pendente. “esta PERGUNTA ja foi materializada de fato (clique morto)?” é o que a função já responde. A declaração separada dessa autoridade exige a frase de corte. app/docs/BACKLOG.md:1002 e app/docs/LEIS.md:69
Q2: Como o sistema sabe se ainda há algo que o administrador pode fazer naquela pergunta? app/chamados/services/raiox.py:160–194
Q3 A: Registra a regra existente por pergunta. O admin só vê a ação enquanto ela puder produzir efeito. A folha segue consultando as ações disponíveis para o dia. Alcance hoje nao medido. Formaliza o registro com teste do comportamento atual. app/docs/BACKLOG.md:1002 e app/chamados/services/fio.py:310–319 e app/folha/fora_da_folha.py:70–83
Q3 B: O botão do admin passa a ler a mesma tarefa do dia que a folha já usa. Alcance hoje nao medido. Migra o leitor restante e confere que o botão continua disponível quando ainda há ação. app/docs/BACKLOG.md:2112 e app/chamados/services/tarefa_do_dia.py:142–147 e app/chamados/services/fio.py:310–319
Q4: ja respondida pelo corte CLIQUE MORTO de 02/09. O efeito comprovado retira a ação, e o registro sozinho deixa a pergunta disponível. LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN responde por qual caminho se chega ao dia. app/chamados/services/raiox.py:160–194 e app/docs/CORTES.md:457 e app/docs/BACKLOG.md:1002
Q5: vira complemento de FIO-E0-V4-E3-LEITORES. A migração já abrange a tela e a folha, e a tela ainda consulta a regra pelo caminho antigo. app/docs/BACKLOG.md:2112 e app/chamados/services/fio.py:310–319

## O176 -- classe: resto -- tipo: corte
Q1: “o que o pacote declara tocar (`declarados()`) ja esta na arvore?” app/docs/BACKLOG.md:1553
Q2: Para liberar um novo trabalho, basta encontrar o título do anterior ou é preciso conferir que suas mudanças entraram? app/docs/BACKLOG.md:1553
Q3 A: A fila interna continua liberando pelo título, sem efeito direto na tela do admin ou na folha. Alcance hoje nao medido. Em 04/10 houve 0 liberações erradas entre 47 rótulos examinados. Mantém o mecanismo atual. app/docs/BACKLOG.md:1553 e app/core/esteira_vigia.py:269–314
Q3 B: A fila interna só libera após comprovar a entrega, sem efeito direto na tela do admin ou na folha. Alcance hoje nao medido. Constrói o verificador compartilhado de conteúdo e testa dois pacotes com o mesmo título. app/docs/BACKLOG.md:1553 e app/docs/CORTES.md:317
Q4: nao respondida. ESTEIRA-SECA-1-E-2-AGORA exige uma fonte comum. L-011 exige a assinatura da nova autoridade. O critério de conteúdo ainda espera corte. app/docs/CORTES.md:317 e app/docs/LEIS.md:69 e app/docs/BACKLOG.md:1553
Q5: segue. A cópia ainda usa o título para decidir se o pacote já entrou. app/core/esteira_vigia.py:269–314 e app/docs/BACKLOG.md:1553

## O94 -- classe: resto -- tipo: !
Q1: “o gravado de 09 passa a nao pagar essas 283 h (e elas viram cobranca a resolver com o DP/supervisao), ou ficam como estao ate a cobranca ser respondida?” app/docs/PENDENTES_RONALD.json:369
Q2: As horas de setembro dos turnos sem saída devem ser retiradas do cálculo registrado agora ou mantidas até o DP resolver a pendência? app/docs/PENDENTES_RONALD.json:369
Q3 A: Admin vê a cobrança da saída faltante. O cálculo registrado perde as horas indevidas, com eventual retificação pelo DP. Alcance hoje nao medido. A medida inicial foi 14 pessoas e 283,01 h, depois revista para 12 e 219,02 h. Prepara nova comparação e ato nominal com trilha e reversão. app/docs/BACKLOG.md:1495 e app/docs/RELATO-ARQUIVO.md:6233–6234 e app/docs/RELATO.md:5054
Q3 B: Admin e DP recebem os dois valores e tratam a cobrança. A folha emitida conserva o valor enquanto o DP decide. Alcance hoje nao medido. Prepara a pauta com a diferença atualizada, sem executar recálculo por este ato. app/docs/BACKLOG.md:1676 e app/docs/RELATO.md:5054
Q4: nao respondida quanto ao ! nominal. L-113 e RELAVRA-09-MOTOR-DE-HOJE separam o cálculo atual da folha já emitida. O registro posterior autoriza guarda e medição da 09 e deixa a aplicação para autorização própria. app/docs/LEIS.md:155 e app/docs/BACKLOG.md:1676 e app/docs/RELATO.md:5054
Q5: segue como ! de dinheiro. O JSON ainda tem resposta nula e a última medida do alcance deste ato [nao verificado]. app/docs/PENDENTES_RONALD.json:366–373

## HIGIENE-DE-CONTEXTO -- classe: resto -- tipo: !
Q1: “pode QUEBRAR o paragrafo em dois -- regra num, historia no outro -- sem mudar uma palavra”. Era o ! do item 5. app/docs/RELATO-ARQUIVO.md:27476–27490
Q2: Pode separar a regra da história que a explica, preservando todas as palavras da regra? app/docs/RELATO-ARQUIVO.md:27488–27490
Q3 A: Tela do admin e folha ficam iguais. Alcance documental atual nao medido. A leitura histórica marcou 5 trechos misturados em 27 parágrafos. Move a história com ponteiro e confere a regra palavra por palavra. app/docs/RELATO-ARQUIVO.md:27481–27489 e app/docs/BACKLOG.md:1321
Q3 B: Tela do admin e folha ficam iguais. Alcance documental atual nao medido. Na leitura histórica, 0 dos 27 parágrafos continham só história. Mantém os trechos juntos, sem trabalho novo. app/docs/RELATO-ARQUIVO.md:27481–27490
Q4: ja respondida pelo corte DIETA-DO-CLAUDE-MD (O224), que aplica L-109 e preserva a regra palavra por palavra. app/docs/BACKLOG.md:1321 e app/docs/LEIS.md:135
Q5: pode morrer como pendência independente. Os itens 1–4 e 6 foram fechados, e O220/O224 registram a transferência da história e sua conclusão. app/docs/BACKLOG.md:1388 e app/docs/BACKLOG.md:1337 e app/docs/BACKLOG.md:1321

## O148-EDITAR -- classe: resto -- tipo: corte
Q1: “sem pergunta” em forma interrogativa. A proposta literal que espera corte é “calendario com uso edita por versao com vigencia, nunca no lugar”. app/docs/BACKLOG.md:819 e app/docs/CORTES.md:390
Q2: Quando um calendário de folgas já é usado, o administrador salva no mesmo cadastro ou cria uma versão que começa a valer numa data escolhida? app/docs/CORTES.md:390
Q3 A: Admin escolhe a data da nova versão e pode consultar a anterior. A folha passa a usar as novas folgas a partir da vigência. Alcance hoje nao medido. Em 16/11 havia 72 calendários do tipo dias do mês em uso. Constrói versões, vigência, trilha e edição. app/docs/CORTES.md:390 e app/docs/RELATO.md:1058
Q3 B: Admin edita o cadastro existente. A porta atual sincroniza as folgas e células futuras, sem recalcular a folha no clique. Alcance hoje nao medido. Mantém a porta atual. app/escala/folgas.py:571–601 e app/escala/folgas.py:605–640
Q4: nao respondida. O148-EDITAR registra a proposta e pede corte de lei. CALENDARIO-DE-FOLGAS-ARQUIVA-NAO-APAGA decide o arquivamento. app/docs/CORTES.md:390 e app/docs/BACKLOG.md:2751
Q5: vira complemento de O148, como já consta na grade, com o corte de versão pendente. O smoke de 16/11 usou calendário sem vínculo nem folga materializada e saiu sem salvar. app/docs/GRADE-MESTRA.md:1523–1526 e app/docs/RELATO.md:983

## DIA-CONVOCADO-PELO-ADMIN -- classe: resto -- tipo: !
Q1: “sem pergunta” pendente. Falta o aval de construção. O literal do posto já respondido foi “o ato de convocar vence a marca do posto para aquele colab-dia, ou convocacao em posto fechado exige reabrir o posto?” app/docs/BACKLOG.md:2290 e app/docs/RELATO.md:115
Q2: O administrador pode convocar alguém para trabalhar no feriado mesmo quando o posto está marcado como fechado? app/docs/RELATO.md:115
Q3 A: Admin convoca com motivo e recebe aviso de posto fechado. A folha passa a tratar aquele dia como previsto, admite a cobrança de falta e aplica a regra de feriado no pagamento. Alcance hoje nao medido. A leitura registrou 0 dias já editados, 12 postos e 26 vínculos potencialmente expostos. Constrói a tela e adapta os leitores do dia e da cobrança. app/docs/RELATO.md:114–118 e app/docs/BACKLOG.md:2290
Q3 B: Admin teria de reabrir o posto antes de convocar. A folha manteria o dia como feriado sem falta até a convocação válida. Alcance hoje nao medido. Constrói a conferência do posto e a recusa explicada na tela. app/docs/RELATO.md:115 e app/docs/BACKLOG.md:2290
Q4: ja respondida pelo corte FILA-PARALELA-1911-1150 item A, que permite a convocação individual com aviso, e pelo corte RESPOSTA-DESENHO-DIA-CONVOCADO-1911-1145, que a torna dia previsto. app/docs/RELATO.md:115 e app/docs/RELATO.md:118
Q5: vira complemento de O10-TROCA-DE-PLANTAO-ENTRA. O vínculo já foi registrado e a construção aguarda aval. app/docs/RELATO.md:132 e app/docs/BACKLOG.md:2290

## LAUDO-col33-0310 -- classe: contrato 20 -- tipo: corte
Q1: “sem pergunta” pendente. O literal já respondido começa em “(a) rejulgar o resto pela 7b (comp 10 aberta: reversao antes, prova de frota depois), por grupo com DIFF publicado” e oferece também só o grupo de ata movida ou esperar o ciclo natural. app/docs/RELATO.md:4876
Q2: Os registros antigos de outubro devem ser revistos em lote, só onde mudam as horas registradas, ou à medida que cada dia voltar a ser analisado? app/docs/RELATO.md:4876
Q3 A: Admin vê os chamados dos dois grupos atualizados. A folha muda no grupo de horas, e o segundo grupo teve 0 linhas de DiaPago alteradas na comparação. Alcance hoje nao medido. Foram medidos 11 e 7 colaboradores nos dois grupos. Executa em lotes pela porta existente, com comparação e reversão. app/docs/RELATO.md:5071–5077
Q3 B: Admin vê a atualização do primeiro grupo e encaminha os demais aos responsáveis. A folha muda só no primeiro grupo. Alcance hoje nao medido. O primeiro grupo medido tinha 11 colaboradores. Executa um lote restrito. app/docs/RELATO.md:4876 e app/docs/RELATO.md:5071
Q3 C: Admin e folha recebem a atualização quando cada dia for revisto pelo fluxo regular. Alcance hoje nao medido. Dispensa lote específico. app/docs/RELATO.md:4876
Q4: ja respondida pelo corte REJULGAR-COMP10-RESTO-DA-FROTA, opção A, com inclusão posterior de col444 por REJULGAR-COMP10-COL444-E-DERIVA. app/docs/CORTES.md:392 e app/docs/CORTES.md:395
Q5: pode morrer como pendência de corte. Col33 foi resolvido, os grupos foram aplicados e os quatro revertidos foram reaplicados. A divergência de col988 tem registro próprio. O rejulgamento era pré-requisito do trabalho de dia da batida no contrato 20. app/docs/RELATO.md:5263 e app/docs/RELATO.md:5076–5081 e app/docs/RELATO.md:4794 e app/docs/BACKLOG.md:849 e app/docs/BACKLOG.md:1849

## BO-ROTULO-REABRIR-SO-NO-DETALHE -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta aval para construir. SMOKES-1211 item 2 já reservou “Reabrir” ao detalhe completo. app/docs/BACKLOG.md:2337
Q2: Como o administrador deve ver a opção “Reabrir” somente quando o chamado puder ser reaberto? app/docs/BACKLOG.md:2337 e app/docs/LEIS-UI.md:39–49
Q3 A: Admin vê no detalhe local só as ações permitidas ao seu perfil. O detalhe integrado oferece comentário. Sem efeito direto na folha. Casos hoje nao medidos. Constrói o filtro por estado e perfil e retira o seletor integrado sem efeito. app/docs/BACKLOG.md:2337 e app/chamados/views.py:292–310 e app/chamados/catalogo/motor.py:139–156
Q3 B: Admin vê ações permitidas nos dois detalhes quando a mudança puder ser gravada. Sem mudança na regra da folha. Casos hoje nao medidos. Constrói a ligação do detalhe integrado às transições autorizadas e confere as duas telas. app/docs/BACKLOG.md:2219–2220 e app/docs/BACKLOG.md:2337
Q4: ja respondida pelo corte SMOKES-1211 item 2 quanto ao lugar e pela LEIS-UI L4 quanto à exibição da ação. O aval de construção continua pendente. app/docs/BACKLOG.md:2337 e app/docs/LEIS-UI.md:39–49
Q5: segue. O smoke de rótulos de 12/11 cobriu 3 de 5 telas e saiu sem salvar. O fechamento por uso de 16/11 é de REABRIR-NAO-DESTROI. app/docs/RELATO.md:2198 e app/docs/RELATO.md:1037 e app/docs/BACKLOG.md:2337

RESUMO: 3 sobem ao Ronald como lei (O275, O176 e O148-EDITAR), 6 já têm resposta de regra no escopo indicado em Q4, e 5 podem morrer ou ficar como complemento. O148-EDITAR conta também como complemento porque o vínculo com O148 já está escrito e a lei segue pendente. app/docs/RELATO.md:417 e app/docs/BACKLOG.md:1553 e app/docs/GRADE-MESTRA.md:1523–1526; demais fontes em Q4 e Q5 de cada ficha.

## 20/11 11:55 · sessao bbbe83f93203 · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Usei a cópia local e os anexos da mensagem. A confirmação do hash pelo Git ficou [nao verificado]. A entrega está no chat, em modo somente leitura.

```text
## DST-L1 -- classe: espurias/HE -- tipo: corte
Q1: “nasce o juiz `decisao_admin_dia_sem_trabalho`?” app/docs/RELATO.md:542. A revisão da alternativa pela hora extra foi marcada como respondida. Restam o registro da divisão dos casos e a medição de como aparecem as correções de escala e as invalidações de batida. app/docs/RELATO.md:451; app/docs/BACKLOG.md:2827
Q2: Quando alguém bate ponto num dia em que não estava escalado, como o administrador registra o que aconteceu? app/docs/RELATO.md:542
Q3: A) Usar a decisão de hora extra. O admin chega a Autorizar ou Não pelo dia; a decisão dispara recálculo e pode liberar a pendência de exportação. A porta existente atendia 55 de 65 dias com duas ou mais batidas na sombra de 18/11, dentro de 113 dias observados; hoje, nao medido. Depois, ligar as telas à porta e dar destino aos casos que ela não recebe. app/docs/BACKLOG.md:1976; app/docs/RELATO.md:461; app/ponto/portas/he.py:175,190,229; app/folha/porta_export.py:676
Q3: B) Registrar o reconhecimento do trabalho numa ação própria do dia. O admin vê esse reconhecimento separado da autorização de hora extra; o efeito financeiro desse reconhecimento teria de ser definido, com alcance nao medido. Depois, construir o registro e a leitura da decisão por pessoa e dia. app/docs/RELATO.md:506,522,542
Q4: ja respondida pelo corte DST-L1-1800 quanto a mandar o dia à decisão do admin. O desenho pela hora extra e a resposta à revisão constam de DSTL1-PELA-PORTA-DA-HE e RESPOSTAS-1811-1312, com registro e medição ainda na fila. app/docs/CORTES.md:549; app/docs/RELATO.md:451; app/docs/BACKLOG.md:1976,2827
Q5: vira complemento de DIA-SEM-TRABALHO-PORTA. O backlog já registra a ação pela hora extra como parte L1 dessa obra. app/docs/BACKLOG.md:1975,1976

## O-JANELA-HE -- classe: espurias/HE -- tipo: smoke
Q1: “sem pergunta”. Falta o retorno do teste de clique dos 13 passos, com filtros, recusa em lote, autorização com motivo, PDF e atalho na Central. app/docs/BACKLOG.md:1255; app/docs/RELATO-ARQUIVO.md:28550,28554,28561,28573
Q2: Os filtros, as decisões de hora extra, o PDF e o atalho da Central funcionam como esperado quando o administrador os usa? app/docs/RELATO-ARQUIVO.md:28550,28573
Q3: A) Roteiro passa. O admin confirma o que viu na tela; autorizar põe o fechamento na fila de recálculo. Cobertura do roteiro inteiro: nao medido. Depois, registrar o resultado e encerrar o teste de clique. app/docs/RELATO-ARQUIVO.md:28554,28565,28573; app/docs/RELATO.md:1037
Q3: B) Algum passo falha. O admin registra o clique e o que a tela mostrou; efeito da falha na folha e alcance: nao medido. Depois, reproduzir o passo e delimitar a correção. app/docs/BACKLOG.md:1255; app/docs/RELATO-ARQUIVO.md:28550,28573
Q4: nao respondida. L-097 define o que deve acontecer com as horas extras; o corte de uso deixou esta confirmação de tela aberta. app/docs/LEIS.md:115; app/docs/RELATO.md:1037
Q5: segue. PRIORIDADE-ESPELHO-VERDADE-E-TRES-CORTES, item 4 de 16/11, fechou BO-HE-RECUSAR-LOTE-500 com 71 atos e zero erro após a cura; O-JANELA-HE ficou aberta com 72 atos desde 01/10 e dois erros anteriores à cura. app/docs/BACKLOG.md:2367; app/docs/RELATO.md:1037

## FRAC5-MINIMO-5 -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta o novo corte de pouso da correção guardada, com as pendências da revisão. app/docs/BACKLOG.md:788; app/docs/GRADE-MESTRA.md:1785
Q2: O sistema deve impedir que o administrador agende uma parte das férias com menos de cinco dias? app/docs/BACKLOG.md:788
Q3: A) Manter a correção guardada. O admin vê o aviso atual de até três partes, uma com pelo menos 14 dias; a validação e o efeito na folha seguem os caminhos atuais. O corte registrou 5 agendamentos e 0 com parte menor que 5; alcance atual nao medido. Depois, nenhuma construção nessa opção. app/templates/ferias/agendar.html:10; app/ferias/models.py:264; app/docs/CORTES.md:484
Q3: B) Autorizar a correção guardada. O admin recebe recusa ao agendar ou remarcar fora da regra; ao aprovar, pode ver aviso de aprovação sem agendamento. Sem agendamento, a ausência de férias correspondente não é criada; impacto na folha e alcance atual nao medido. Depois, retomar a cópia guardada, tratar os achados da revisão e medir o histórico. app/docs/BACKLOG.md:788; app/docs/RELATO.md:1626; app/ferias/models.py:388; app/ferias/services.py:774,790
Q4: nao respondida para o novo pouso. FRAC5-SO-O-QUE-O-CORTE-MANDOU escolheu a opção (b) anterior, que liberou o texto e mandou guardar a correção. app/docs/CORTES.md:476,484
Q5: segue. O backlog exige novo corte de pouso e registra que falta medir o histórico. app/docs/BACKLOG.md:788

## ESCALA-NAO-DESCREVE C8 -- classe: espurias/HE -- tipo: !
Q1: “sem pergunta” para o aval atual. O fechamento está suspenso à espera do corte sobre a tabela 2d. A pergunta anterior foi “a amostra fica minutos_do_oraculo apesar da concatenacao, vai a outra fonte, ou o detector exclui (contado) o dia com >1 bloco?” app/docs/RELATO.md:48,122; app/docs/BACKLOG.md:2377
Q2: De quais registros devem sair as sugestões de horário que o administrador vê quando as batidas e a escala cadastrada divergem? app/docs/RELATO.md:48,61,122
Q3: A) Liberar a fonte cortada às 13h. O admin vê a sugestão calculada por essa fonte; o cálculo da sugestão não grava folha. A sombra registrou 233 pessoas e 15 dias com mais de um bloco; alcance atual nao medido. Depois, implementar o brief com o limite conhecido e testar os dias partidos. app/docs/RELATO.md:74,110,122; app/ponto/services/esmeril_espelho.py:239
Q3: B) Usar os turnos fechados do sistema como fonte. As sugestões podem mudar; o efeito na folha dependeria de aplicar a escala pela ação da tela. Na sombra, essa fonte coincidiu com a proposta nova em 139 de 233 e com a antiga em 4; alcance atual nao medido. Depois, rever o brief e testar a dependência do cadastro nessa fonte. app/docs/RELATO.md:48,61; app/templates/escala/cadastro_x_realidade.html:32
Q4: ja respondida pelo corte ORDEM-E-RESPOSTAS-1911-1300, item 4, quanto à pergunta anterior. O fechamento com a tabela posterior continua suspenso. app/docs/RELATO.md:48,74,122
Q5: segue. A obra está sem construtor e espera o aval sobre a tabela 2d. app/docs/BACKLOG.md:2377; app/docs/RELATO.md:48

## JUIZ-DE-DONO-DO-DIA -- classe: contrato 20 -- tipo: corte
Q1: “sem pergunta”. Falta autorizar o critério que atribui a causa ao dia de cada pergunta para decidir entre a cobrança ao colaborador e a lista do admin, ligada ao R2 do placar. app/docs/GRADE-MESTRA.md:59; app/docs/CORTES.md:425,426; app/docs/BACKLOG.md:1201; app/core/placar_estrutural.py:135
Q2: Como saber se o problema daquele dia deve continuar com o funcionário, ir ao administrador ou ser corrigido no sistema? app/docs/BACKLOG.md:1201; app/docs/LEIS.md:122
Q3: A) Manter a cobrança atual. O admin não recebe uma nova classificação na lista; a folha segue pelas regras atuais. A medida de 27/10 registrou 787 perguntas em 394 dias de colaboradores; hoje, nao medido. Depois, nenhuma construção nessa opção. app/docs/CORTES.md:426; app/docs/RELATO.md:4840
Q3: B) Classificar pelo cadastro e pelas batidas do próprio dia. O admin recebe na lista os problemas de cadastro ou de batida; a classificação direciona a ação, e o efeito de eventual correção na folha é nao medido. Casos que trocariam de destino: nao medido. Depois, ligar a classificação aos dados do dia e testar os destinos. app/docs/CORTES.md:425; app/docs/LEIS.md:122; app/ponto/management/commands/e6_oraculo.py:53,85
Q4: nao respondida. L-099 define as categorias e FIO-EXPORTADA-X-O44 mantém a cobrança provisória; o critério para esses dias ficou pendente. app/docs/LEIS.md:122; app/docs/CORTES.md:426; app/docs/GRADE-MESTRA.md:59
Q5: segue. A Grade Mestra ainda registra a falta de critério autorizado para o dia. app/docs/GRADE-MESTRA.md:59

## RAIZ-DA-DISPUTA -- classe: fio morto -- tipo: corte
Q1: F1 — “QUEM FECHA o orfao?” e “reabrir a dona, nascer outra ou Pauta DP?” app/docs/RELATO.md:328
Q2: Quem encerra um chamado que ficou sem pergunta ao funcionário, e para onde vai um motivo novo quando a discussão original já foi encerrada? app/docs/RELATO.md:328
Q3: Chamado sem pergunta, A) Encerrar pelo evento que resolve o caso. O admin vê o encerramento com trilha; efeito sobre retenção na folha e casos que podem fechar: nao medido. Depois, ligar o evento ao fechamento. app/docs/RELATO.md:245; app/folha/fora_da_folha.py:17
Q3: Chamado sem pergunta, B) Mandar à fila de quem pode resolver. O admin vê responsável e ação; o encaminhamento não decide a folha. Alcance efetivo nao medido; o recorte de 19/11 achou 84 chamados vivos sem pergunta de destino, entre 5.504. Depois, construir o encaminhamento com responsável. app/docs/RELATO.md:245; app/folha/fora_da_folha.py:46
Q3: Motivo novo, A) Reabrir a discussão original. O admin vê o motivo no histórico reaberto; efeito na folha depende da resolução e é nao medido. Depois, construir a reabertura com trilha. app/docs/RELATO.md:245,328; app/docs/CORTES.md:515
Q3: Motivo novo, B) Abrir outra discussão. O admin vê uma nova pendência; efeito na folha e alcance nao medido. Depois, unificar a emissão e evitar duplicatas. app/docs/RELATO.md:245,328; app/chamados/services/disputa_emissao.py:540,1110
Q3: Motivo novo, C) Encaminhar ao DP. O admin vê a pauta com a origem; o encaminhamento não altera o gravado. Alcance nao medido nas três opções: 24.399 é o estoque de perguntas em discussões fechadas, e o instante de chegada do motivo não é registrado. Depois, construir a pauta com vínculo à origem. app/docs/RELATO.md:245,328; app/docs/LEIS.md:125
Q4: nao respondida para as duas perguntas da F1. RAIZ-F1-UM-ELEITOR-DO-DESTINO resolveu a escolha anterior do chamado de destino; BO-DISPUTA-DESENHO-RAIZ registrou a questão do motivo novo na F1. app/docs/RELATO.md:2453,328; app/docs/CORTES.md:515
Q5: segue. A leitura de 19/11 confirmou as duas perguntas abertas e mediu o estoque, sem identificar quantos motivos chegaram depois do encerramento. app/docs/RELATO.md:245,328

RESUMO: 2 obras sobem ao Ronald como lei, JUIZ-DE-DONO-DO-DIA e RAIZ-DA-DISPUTA F1, esta com duas perguntas (app/docs/GRADE-MESTRA.md:59; app/docs/RELATO.md:328). 2 têm corte para a pergunta de desenho já registrada, DST-L1 e C8 (app/docs/CORTES.md:549; app/docs/RELATO.md:74,122). 1 pode virar complemento, DST-L1 (app/docs/BACKLOG.md:1975,1976).
```

## 20/11 11:54 · sessao 76801b9e1966 · - · - · origem=Ronald (tmux)

## FAIXA-NO-CADASTRO -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta autorizar a execução após o contrato 20. O desenho registrado prevê campo no cadastro e migração a partir do mapa atual. app/docs/BACKLOG.md:2334
Q2: Quando chegar a vez dessa mudança, o administrador poderá cadastrar qual ausência prevalece quando duas regras caem no mesmo dia? app/docs/BACKLOG.md:2334
Q3: A) Autorizar na vez registrada. Tela ganha o campo. Na folha, mudanças posteriores na prioridade podem alterar qual ausência vale. Alcance atual não medido. Construir campo, migração e leitura única. app/docs/BACKLOG.md:2334; app/ponto/precedencia.py:45
Q3: B) Adiar o aval. Tela mantém as escolhas atuais e a folha segue a prioridade vigente. Alcance atual não medido. Nenhuma construção neste item. app/templates/ponto/partials/painel_tipo_ausencia.html:28; app/ponto/precedencia.py:76
Q4: A direção já está registrada pelo MANHA-1211, item 7. O aval para executar segue pendente. app/docs/BACKLOG.md:2334; app/docs/RELATO.md:2197
Q5: vira complemento de EFEITO-NONE-UM-LEITOR, na posição POS-20 e com seu aval próprio. app/docs/GRADE-MESTRA.md:1325; app/docs/GRADE-MESTRA.md:1328; app/docs/BACKLOG.md:2334

## DISPUTA-FECHADA-SEM-RESOLUCAO -- classe: resto -- tipo: corte
Q1: "sem pergunta". O registro pede corte para exigir resolução ao fechar e identifica falta de trilha do motivo. app/docs/BACKLOG.md:1222
Q2: Como o administrador deve saber por que uma disputa foi encerrada quando a explicação está em branco? app/docs/BACKLOG.md:1222
Q3: A) Usar o motivo já gravado nos fechamentos novos. A tela de regularização já pede texto. Folha segue pela mesma apuração. Alcance atual não medido. Nenhuma nova regra de fechamento. app/chamados/models.py:2059; app/chamados/models.py:2095; app/chamados/services/acoes_disputa.py:121
Q3: B) Mostrar no detalhe quais encerramentos antigos ficaram sem motivo registrado. Sem alteração de folha. Aferição de 30/09, 30 de 4.682 com resolução vazia, estoque atual não medido. Construir o aviso a partir do registro existente. app/docs/BACKLOG.md:1222; app/chamados/migrations/0049_disputa_motivo_fechamento.py:11
Q4: ja respondida pelo corte C7-DISPUTA-FECHA-POR-UM-JUIZ quanto à trilha dos fechamentos novos. O campo de texto pode ficar vazio com motivo estruturado gravado. app/docs/BACKLOG.md:1618; app/docs/LEIS.md:65; app/chamados/models.py:2040; app/chamados/models.py:2095
Q5: pode morrer como obra avulsa de fechamento novo. O estoque antigo segue sem aferição atual e a migração preserva o legado em branco. app/chamados/models.py:2095; app/chamados/migrations/0049_disputa_motivo_fechamento.py:11; app/docs/BACKLOG.md:1222

## VIGENCIA-SOBREPOSTA-COL515 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta decidir o tratamento de dois vínculos históricos inativos com horários opostos no mesmo período. app/docs/BACKLOG.md:1223
Q2: O que fazer quando o histórico de uma pessoa mostra dois horários de trabalho diferentes para os mesmos dias? app/docs/BACKLOG.md:1223
Q3: A) Preservar histórico e avisar ou recusar eventual reativação conflitante. Administrador recebe a explicação ao tentar reativar, folha sem recálculo por essa opção. Medida histórica de 1 pessoa, 2 vínculos e 11 dias, frota atual não medida. Construir a guarda pela linha do tempo. app/docs/BACKLOG.md:1223; app/colaboradores/services/vinculo.py:505
Q3: B) Corrigir as datas históricas com evidência e trilha. O admin vê o período corrigido. Efeito na folha não medido. Mesmo alcance histórico. Construir tratamento do passivo com as regras de competência. app/docs/BACKLOG.md:1223; app/docs/BACKLOG.md:1248; app/docs/LEIS.md:155
Q4: nao respondida para o histórico inativo. VINCULO-LINHA-DO-TEMPO já trata a linha ativa e seu censo filtra `ativa=True`. app/docs/BACKLOG.md:1248; app/colaboradores/services/vinculo.py:505; app/colaboradores/services/vinculo.py:523
Q5: vira complemento de VINCULO-LINHA-DO-TEMPO. app/docs/GRADE-MESTRA.md:1078; app/docs/GRADE-MESTRA.md:1082; app/docs/RELATO.md:1191

## O62 -- classe: espurias/HE -- tipo: corte
Q1: "sem pergunta". Falta corrigir a resposta que diz “escala sem intervalo” quando há duração cadastrada e faltam os horários da pausa no registro do dia. app/docs/BACKLOG.md:1469; app/ponto/juiz_batida.py:294
Q2: Quando a escala prevê uma pausa e o dia não tem os horários dela, como a pausa deve aparecer e contar na folha? app/docs/BACKLOG.md:1469; app/docs/LEIS.md:80
Q3: A) Mostrar duração prevista e falta dos horários. Folha segue a apuração atual. Construir correção de resposta e leitores de tela. Amostra antiga de 3 de 5 pessoas, 17 de 20 dias no col87. Frota atual não medida. app/docs/BACKLOG.md:1469; app/ponto/motor_calculo_v2.py:669
Q3: B) Levar também a duração aos leitores de cálculo que ainda a omitem. Tela distingue pausa prevista de pausa registrada. Folha aplica a duração onde a regra ainda falta. Efeito atual não medido. Construir a correção na fonte e medir por rubrica antes do pouso. app/docs/LEIS.md:80; app/docs/LEIS.md:193; app/docs/LEIS.md:142
Q4: ja respondida pelo corte INTERVALO-CADASTRADO-NAO-VIRA-HE, L-032. app/docs/LEIS.md:80
Q5: segue para execução. A proteção no motor já existe e o retorno incorreto ainda está na cópia. app/docs/RELATO-ARQUIVO.md:6403; app/docs/RELATO-ARQUIVO.md:6412; app/ponto/juiz_batida.py:304

## 20/11 11:54 · sessao 73b72598e64b · - · - · origem=Ronald (tmux)

## O176 -- classe: resto -- tipo: corte
Q1: "o que o pacote declara tocar (`declarados()`) ja esta na arvore?" app/docs/BACKLOG.md:1553
Q2: O sistema pode liberar o trabalho pelo título ou precisa conferir se as mudanças daquele pacote já entraram? app/docs/BACKLOG.md:1553
Q3: (a) Usar título. Tela do admin e folha sem efeito direto. Na fila de trabalho, nomes iguais podem liberar um alvo ainda ocupado. Manter o verificador atual. app/core/esteira_vigia.py:264,302-314; bin/fabricante_alvo.py:128-137.
Q3: (b) Conferir o conteúdo entregue. Tela do admin e folha sem efeito direto. A fila só libera com prova de entrega. Construir a comparação compartilhada e teste de dois pacotes com o mesmo título. app/docs/BACKLOG.md:1553; app/docs/CORTES.md:317.
Q3: Alcance hoje nao medido nas duas. Em 04/10, 0 liberações erradas entre 47 rótulos de pacote. app/docs/BACKLOG.md:1553
Q4: nao respondida. ESTEIRA-SECA-1-E-2-AGORA já manda usar uma fonte. L-011 exige o corte para a autoridade nova. app/docs/CORTES.md:317; app/docs/LEIS.md:69.
Q5: segue. A guarda ainda usa o título e a mudança de critério espera corte. app/core/esteira_vigia.py:302-314; app/docs/BACKLOG.md:1553.

## O94 -- classe: resto -- tipo: !
Q1: "o gravado de 09 passa a nao pagar essas 283 h (e elas viram cobranca a resolver com o DP/supervisao), ou ficam como estao ate a cobranca ser respondida?" app/docs/PENDENTES_RONALD.json:369
Q2: As horas de setembro com saída faltando devem sair do fechamento antes que a supervisão esclareça os casos? app/docs/PENDENTES_RONALD.json:369
Q3: (a) Admin recebe cobrança pelas saídas. A retificação retiraria 283,01 h do fechamento dos 14 da medição de 27/09. Preparar nova medição e retificação com trilha, sujeita à guarda da 09 e ao aval. app/docs/BACKLOG.md:1495; app/ponto/services/fechamento.py:19-41.
Q3: (b) Admin/DP confere pago e calculado. Folha emitida preservada durante a apuração. Os mesmos 14 da medição histórica. Preparar a pauta com os dois valores pela porta existente. app/docs/BACKLOG.md:1495; app/pautas/management/commands/pauta_pago_x_devido_exportada.py:71.
Q3: Alcance hoje nao medido. A medida publicada é de horas, pela soma de trabalho e folga trabalhada. app/docs/BACKLOG.md:1495.
Q4: nao respondida quanto ao ! nominal. L-113 altera L-092. O corte BO-B de 23/10 deu ! para a guarda e a medição da 09, com o apply pendente. app/docs/LEIS.md:125,155; app/docs/RELATO.md:5054.
Q5: segue como ! de dinheiro. Código da cura no ar e decisão sobre o gravado pendente. app/docs/BACKLOG.md:1495; app/docs/PENDENTES_RONALD.json:366-373.

## HIGIENE-DE-CONTEXTO -- classe: resto -- tipo: !
Q1: "pode QUEBRAR o paragrafo em dois -- regra num, historia no outro -- sem mudar uma palavra". app/docs/RELATO-ARQUIVO.md:27488-27489
Q2: Podemos separar o texto que manda fazer algo da história que explica por quê, preservando as palavras? app/docs/RELATO-ARQUIVO.md:27481-27490
Q3: (a) Separar e mover a história com referência para consulta. Tela e folha sem efeito. Hoje nao medido. A O224 já fez a passagem e conferiu o texto movido. app/docs/BACKLOG.md:1321; app/docs/LAPIDES.md:23-34.
Q3: (b) Conservar os trechos juntos. Tela e folha sem efeito. Hoje nao medido. Nenhuma construção documental para esses trechos. Em 01/10, 5 trechos de história estavam misturados com regra, em 27 parágrafos com zero inteiramente históricos. app/docs/RELATO-ARQUIVO.md:27476-27490.
Q4: ja respondida pelo corte L-109, aplicado com o aval da O224, DIETA-DO-CLAUDE-MD, para mover narrativa e preservar a regra palavra por palavra. app/docs/LEIS.md:135; app/docs/BACKLOG.md:1321.
Q5: pode morrer como pendência independente. Itens 1–4 e 6 fechados, passagem do item 5 comprovada na O220/O224. A meta posterior de tamanho está em CLAUDE-MD-ENXUTO. app/docs/BACKLOG.md:1388,1337,1321,1081.

## O148-EDITAR -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta cortar a regra proposta neste literal: "calendario com uso edita por versao com vigencia, nunca no lugar". app/docs/BACKLOG.md:819; app/docs/CORTES.md:390
Q2: Quando um calendário de folgas já é usado, o administrador deve salvar na mesma ficha ou criar uma nova versão que comece a valer em uma data escolhida? app/docs/CORTES.md:390
Q3: (a) Admin escolhe a data de início e consulta versões. Na folha, a regra do calendário novo vale dali em diante. Construir versão, vigência, trilha e editor. app/docs/CORTES.md:390.
Q3: (b) Admin continua salvando na ficha existente. As folgas e dias futuros do vínculo podem mudar, e o clique salva com recálculo da folha desligado. Preservar a porta atual. app/escala/views.py:319,354-357; app/escala/folgas.py:571-601,605-640.
Q3: Alcance hoje nao medido nas duas. A leitura de 16/11 encontrou 72 calendários em uso, sem cobrir todos os casos de folga materializada. app/docs/BACKLOG.md:842; app/escala/models.py:1582-1592.
Q4: nao respondida. O148-EDITAR é registro de ato e pede corte para a versão. O corte sobre arquivar responde à exclusão. app/docs/CORTES.md:390; app/docs/LEIS.md:206,317; app/docs/BACKLOG.md:2751.
Q5: vira complemento de O148, como já registrado. O smoke de 16/11 cobriu o editor de três grades e nada gravou. app/docs/GRADE-MESTRA.md:1523-1527; app/docs/BACKLOG.md:819-820,1524.

Nas quatro fichas, 2 sobem como lei, 1 já respondida e 2 podem morrer ou virar complemento. O148-EDITAR conta como lei pendente e como complemento. O94 segue como !. app/docs/BACKLOG.md:819,1321,1495,1553.

## 20/11 11:54 · sessao 921ff40c3d99 · - · - · origem=Ronald (tmux)

## DIA-CONVOCADO-PELO-ADMIN -- classe: resto -- tipo: !
Q1: “sem pergunta” pendente. Falta aval de construção. As duas decisões de desenho constam como respondidas. app/docs/BACKLOG.md:2290, app/docs/RELATO.md:115, app/docs/RELATO.md:118
Q2: Pode liberar a tela para convocar uma pessoa para trabalhar no feriado? app/docs/BACKLOG.md:2290
Q3: (a) Autorizar a tela com motivo e aviso de posto fechado. A convocação torna o dia previsto, permite cobrança de falta e mantém a dobra conforme o ciclo. Última leitura registrou 0 dias editados e 26 vínculos potenciais em 12 postos fechados. Alcance atual não medido. Construir tela e ajustar os leitores listados no desenho. app/docs/RELATO.md:114–118, app/ponto/portas/celula.py:572–591, app/ponto/motor_calculo_v2.py:914–934
Q3: (b) Aguardar autorização. A tela continua sem convocação e a folha continua com o tratamento atual do feriado. Alcance atual não medido. O desenho aguarda aval. app/docs/BACKLOG.md:2290, app/docs/RELATO.md:114–118
Q4: ja respondida pelo corte RESPOSTA-DESENHO-DIA-CONVOCADO-1911-1145 e pelo corte FILA-PARALELA-1911-1150 item A. app/docs/RELATO.md:115, app/docs/RELATO.md:118
Q5: vira complemento de O10-TROCA-DE-PLANTAO-ENTRA, onde já está registrada. app/docs/RELATO.md:132, app/docs/BACKLOG.md:2290

## LAUDO-col33-0310 -- classe: resto -- tipo: corte
Q1: “sem pergunta” pendente. A pergunta histórica começava “(a) rejulgar o resto pela 7b (comp 10 aberta: reversao antes, prova de frota depois), por grupo com DIFF publicado”; o corte escolheu essa opção e os atos estão registrados. app/docs/RELATO.md:4876, app/docs/CORTES.md:392, app/docs/RELATO.md:5076–5077
Q2: Os registros antigos de outubro devem ser atualizados agora em todos os casos, só onde mudam as horas, ou conforme cada dia for revisto? app/docs/RELATO.md:4876
Q3: (a) Atualizar os chamados de ambos os grupos e as horas onde mudarem. A leitura de 27/10 registrou 11 e 7 colaboradores nos grupos, com 0 DiaPago no segundo. Alcance atual não medido. Executar lotes pela rotina existente com comparação e reversão. app/docs/RELATO.md:5069–5077
Q3: (b) Atualizar chamados e folha só no grupo que muda as horas, encaminhando os outros casos aos responsáveis. A leitura de 27/10 registrou 11 colaboradores no grupo. Alcance atual não medido. Executar o lote restrito e fazer a triagem à parte. app/docs/RELATO.md:4876, app/docs/RELATO.md:5071–5072
Q3: (c) Atualizar tela e folha por revisão regular, sem lote. Alcance atual não medido. app/docs/RELATO.md:4876
Q4: ja respondida pelo corte REJULGAR-COMP10-RESTO-DA-FROTA, com o complemento REJULGAR-COMP10-COL444-E-DERIVA. app/docs/CORTES.md:392, app/docs/CORTES.md:395
Q5: pode morrer como pendência de corte. O caso nominal está resolvido, os lotes foram aplicados e a divergência de col988 tem item próprio. app/docs/RELATO.md:5263, app/docs/RELATO.md:5076–5077, app/docs/RELATO.md:4794, app/docs/BACKLOG.md:849

## BO-ROTULO-REABRIR-SO-NO-DETALHE -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta aval de construção. O lugar de “Reabrir” já foi registrado no SMOKES-1211 item 2. app/docs/BACKLOG.md:2337
Q2: Pode corrigir o menu para o admin só ver “Reabrir” quando o chamado puder ser reaberto? app/docs/BACKLOG.md:2337
Q3: (a) Mostrar no detalhe local apenas ações aceitas pelo sistema e manter comentário no detalhe integrado. A correção da oferta não grava folha. Casos atuais não medidos. Construir filtro por estado e perfil e retirar o seletor ignorado. app/chamados/catalogo/motor.py:139–156, app/chamados/views.py:292–310, app/docs/BACKLOG.md:2337
Q3: (b) Mostrar no detalhe integrado ações com gravação comprovada e aplicar o mesmo filtro no local. A regra da folha permanece, efeitos indiretos não medidos. Casos atuais não medidos. Construir a ligação às transições autorizadas com aval próprio. app/docs/BACKLOG.md:2219–2220, app/chamados/services/fecho_runtime.py:146–175
Q4: ja respondida pelo corte SMOKES-1211 item 2 quanto ao lugar, e pela LEIS-UI L4 quanto a oferecer apenas ação executável. app/docs/BACKLOG.md:2337, app/docs/LEIS-UI.md:39–49
Q5: segue pelo aval de construção. O smoke de 12/11 verificou os rótulos da obra anterior, com três telas cobertas e nenhuma gravação. app/docs/BACKLOG.md:2337, app/docs/RELATO.md:2198

## 20/11 11:54 · sessao 9d752332afc2 · - · - · origem=Ronald (tmux)

O310-RAIOX — resto, corte. Sem pergunta literal para a persistência do histórico. O corte O256-RAIO-X-DO-RETRATO já manda comparar dois retratos gravados e informar quando falta o anterior. O aval específico para modelo e migração permanece pendente. A lente publicada não moveu a folha e a obra já é complemento de O310. Opções para o histórico são guardar só a foto anterior ou guardar fotos datadas, ambas sem efeito de folha e com alcance atual não medido. app/docs/BACKLOG.md:1727, app/docs/CORTES.md:357, app/folha/models.py:366, app/folha/retrato_folha.py:723.

O34 — resto, corte de desenho. Sem pergunta literal. A linha traz as três opções para o Ronald: reconstruir o ajuste sozinho, falhar exibindo o que mudou ou dar prazo para recriar ou descartar. Nenhum efeito direto na tela do admin ou na folha. Alcance atual não medido. Na passada registrada foram 11 de 14 falhas, 8 de âncora e 3 de padrão, com 17 fatias em espera. ZUMBIDO já proíbe o terceiro relance pelo mesmo motivo e exige registro da causa, mas não escolhe uma das três opções. Vira complemento de O31, como já consta da Grade. app/docs/BACKLOG.md:1436, app/docs/CORTES.md:307, app/docs/GRADE-MESTRA.md:1234, app/docs/GRADE-MESTRA.md:1238.

O37 — resto, !. Literal: “o gravado de competencia exportada e reescrito, ou fica e a diferenca vira Pauta DP?” O residual histórico medido foi de 22 dias em dois colaboradores, 161 h na competência 08, sem edição humana. As opções históricas eram corrigir o gravado pela porta, com a tela refletindo os dias corrigidos e impacto efetivo por rubrica ainda não medido, ou preservar a folha e destinar o comparativo ao DP. Já respondida pelo corte COMPLEMENTO-1 RESPOSTAS-R4-E1PORTA-1611 item 3, que manda passivos exclusivamente anteriores à 09 à lista do DP. Pode morrer como espera autônoma e já é complemento de O54. Entrada efetiva na lista do DP [nao verificado]. app/docs/RELATO-ARQUIVO.md:24091, app/docs/RELATO-ARQUIVO.md:24120, app/docs/RELATO.md:1012, app/docs/BACKLOG.md:1460, app/docs/GRADE-MESTRA.md:881.

O296 — resto, !. Sem pergunta literal. Falta autorizar a troca no comentário do motor. As opções são trocar os nove comentários fora da zona ou incluir o décimo com autorização nominal. Ambas alteram só comentários, com efeito nulo na tela do admin e na folha. São 10 locais na medição histórica, alcance atual não medido. A resposta da O295 mantém o histórico público e não concede a autorização nominal do motor. Segue pelo !. O comentário público registrado separadamente integra um complemento, com contagem própria. app/docs/BACKLOG.md:1703, app/docs/BACKLOG.md:1704, app/docs/BACKLOG.md:1732, app/docs/PENDENTES_RONALD.json:2671, CLAUDE.md:142.

## 20/11 11:54 · sessao 5466f272498e · - · - · origem=Ronald (tmux)

## REJULGAR-61-COBPARC -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` próprio com a lista das 55 atas de competências entregues. app/docs/BACKLOG.md:906; app/docs/GRADE-MESTRA.md:657
Q2: Você autoriza revisar a lista de registros antigos de faltas parciais de meses já entregues? app/docs/BACKLOG.md:906
Q3 (a): Preservar as atas, levar as diferenças provadas para conferência do DP e preservar a folha entregue. Alcance atual não medido, 55 entregues e 6 trancadas na medida de 08/11. Depois, preparar a lista e a comparação. app/docs/BACKLOG.md:906; app/docs/LEIS.md:125
Q3 (b): Autorizar revisão nominal, com registro do motivo e comparação para o DP. A folha entregue só pode ser retificada pela exceção autorizada. Alcance atual não medido, 55 no lote que espera `!`. Depois, conferir o lote e reutilizar a porta existente. app/docs/BACKLOG.md:906; app/ponto/portas/regen_exportada.py:87,111-117
Q4: nao respondida. TRES-DO-TOPO-1109 item 2 resolve a regra da COBPARC e exige aval separado para o acervo. app/docs/RELATO.md:2920
Q5: segue. O registro separa a revisão histórica da obra da regra atual. app/docs/BACKLOG.md:897,906

## RESIDUO-PREMIG-NO-BIND-MOUNT -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para apagar os arquivos da instalação em uso. app/docs/BACKLOG.md:1761; app/docs/GRADE-MESTRA.md:421
Q2: Você autoriza remover os arquivos antigos da instalação ou quer deixá-los onde estão? app/docs/BACKLOG.md:1761
Q3 (a): Manter os arquivos. Tela e folha iguais, inventário atual não medido, 64 arquivos registrados em 19/10. Depois, usar a classificação já corrigida. app/docs/BACKLOG.md:1761; app/docs/RELATO-ARQUIVO.md:43426
Q3 (b): Conferir a lista e remover os arquivos autorizados. Tela e folha sem mudança prevista, alcance atual não medido, 64 registrados em 19/10. Depois, fazer a limpeza com o inventário conferido. app/docs/BACKLOG.md:1761
Q4: nao respondida. L-009 exige autorização para apagar. app/docs/LEIS.md:67
Q5: pode morrer como bloqueio do censo. Já se distinguem os ignorados e o registro da correção mostra `EXTRA-VIVO=0`. Delete efetivo [nao verificado]. app/docs/RELATO-ARQUIVO.md:43426

## JANELA-DO-NOTURNO-CRAVADA -- classe: espurias/HE -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar o cálculo protegido e permitir cadastrar os horários noturnos da convenção. app/docs/BACKLOG.md:1769; CLAUDE.md:142
Q2: Você autoriza que cada convenção tenha seu próprio horário de trabalho noturno no sistema? app/docs/BACKLOG.md:1769
Q3 (a): Manter 22h–05h. A tela continua sem campos de horário, a apuração conserva essa faixa, casos com outra faixa não medidos. Depois, nenhuma construção. app/docs/BACKLOG.md:1769; app/ponto/motor_calculo_v2.py:101,103
Q3 (b): Cadastrar início e fim pela tela. Adicional e parcela noturna da hora extra podem mudar onde a convenção estabelecer faixa diferente, casos não medidos. Depois, construir campos, leitura comum e testes de cálculo. app/docs/LEIS.md:64; app/ponto/motor_calculo_v2.py:983-987,1014-1022
Q4: nao respondida quanto ao `!`. L-006 já define que uma regra variável nasce no cadastro pela tela. app/docs/LEIS.md:64; CLAUDE.md:142
Q5: segue. O motor ainda fixa os horários e o aval segue exigido. app/ponto/motor_calculo_v2.py:101,103; app/docs/BACKLOG.md:1769

## PROTEGE-VAZIA-EM-55-LEIS -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar as linhas de leis já escritas. app/docs/BACKLOG.md:1755; app/docs/RELATO-ARQUIVO.md:299
Q2: Você autoriza conferir as regras já publicadas e marcar os trechos de código que elas protegem expressamente? app/docs/LEIS.md:43-48; app/docs/RELATO-ARQUIVO.md:299
Q3 (a): Manter a coluna. Tela e folha iguais, 55 candidatas na triagem desta cópia e total elegível não medido. Depois, nenhuma construção. app/docs/LEIS.md:57; bin/tests/test_lei_protege_sitio.sh:49-91
Q3 (b): Autorizar a conferência. Tela e folha iguais, 55 candidatas e total elegível não medido. Depois, preencher só as proteções explícitas e conferir a trava em ato próprio. app/docs/LEIS.md:43-48,57,131; bin/tests/test_lei_protege_sitio.sh:49-91
Q4: nao respondida quanto ao `!`. LEI-PROTEGE-SITIO já resolve o critério de preenchimento. app/docs/RELATO-ARQUIVO.md:37575-37589; app/docs/LEIS.md:43-48
Q5: segue. A L-097 ainda exemplifica proteção explícita com coluna vazia. app/docs/RELATO-ARQUIVO.md:780; app/docs/LEIS.md:115

## 20/11 11:54 · sessao 86af01f9c4a6 · - · - · origem=Ronald (tmux)

## O275 -- classe: espurias/HE -- tipo: corte
Q1: “O FLAG NOVO DA O275 PODE SER LIGADO NUM TIPO DE PLANTAO (`12x36`/`24x48`), OU O PLANTAO RESPONDE SO PELA PRECEDENCIA QUE JA EXISTE?” app/docs/RELATO-ARQUIVO.md:746
Q2: O administrador pode transformar um feriado em dia de trabalho mesmo quando ele cai na folga do plantão? app/docs/RELATO-ARQUIVO.md:748
Q3: A) Opção de cadastro também nos plantões, exceto intermitente. A folha passa a receber o feriado como dia previsto onde a opção se aplicar e a regra da dobra cabe à O277. Alcance atual não medido, censo histórico identificou 337 vínculos 12x36 expostos e 0 tipos ligados. Atualiza e confere a implementação guardada. app/docs/RELATO-ARQUIVO.md:749-753; app/docs/BACKLOG.md:1682-1684; app/docs/RELATO.md:1771
Q3: B) Opção limitada a escalas que seguem dias da semana. Os plantões mantêm o previsto atual e a folha recebe o feriado previsto nas escalas alcançadas. Alcance atual não medido, censo anterior achou 1 vínculo no alvo e 4 tipos cadastrados como 12x36 que exigiriam recadastro. Acrescenta a restrição e ajusta os testes. app/docs/RELATO-ARQUIVO.md:749-753
Q4: não respondida. A precedência atual preserva a folga do plantão, e o alcance da opção nova continua em corte. app/ponto/precedencia.py:13-18; app/docs/RELATO.md:1771
Q5: segue. Implementação guardada e publicação retida pela escolha. app/docs/BACKLOG.md:1682

## O10-D-PERMUTA-RECIPROCA -- classe: contrato 20 -- tipo: corte
Q1: “(a) DNA carrega marcos do cedente ou (b) d1=d2 fora da porta”. Demais literais sobre carta, falha de gravação, “cede so plantao”, descansos e `!` de escala [nao verificado]. app/docs/RELATO.md:886-887; app/core/contratos_estruturais.py:113-125
Q2: Na troca de plantão no mesmo dia, quem assume recebe os horários de quem cede ou a operação recusa a troca? app/docs/RELATO.md:887
Q3: A) Tela permite a troca e mostra os horários cedidos. A folha usa o plantão assumido como dia normal, com adicionais cabíveis pelo contrato da troca. Alcance hoje não medido. Constrói a preservação dos horários do cedente. app/docs/RELATO.md:887; app/docs/CORTES.md:286
Q3: B) Tela recusa esse uso da troca e a folha conserva a previsão anterior. Alcance hoje não medido. Constrói a recusa explícita, se houver novo corte que permita essa restrição. app/docs/RELATO.md:883-887; app/docs/RELATO.md:2719
Q4: ja respondida pelo corte LEI-O10-D-PERMUTA quanto a permitir a mesma data, reafirmado na preparação de 16/11. Não respondida quanto a como levar os horários a quem assume e às demais pendências listadas na revisão. app/docs/RELATO.md:2719; app/docs/RELATO.md:1055; app/docs/CORTES.md:528; app/docs/RELATO.md:886-887
Q5: segue. A cópia aprovada ainda espera os cortes residuais e o `!` de escala. app/docs/RELATO.md:879; app/docs/RELATO.md:886

## O14 -- classe: resto -- tipo: smoke
Q1: “sem pergunta”. Falta “SMOKE de olho no app de um noturno 12x36 (ex.: col174 ou col788) depois do deploy.” app/docs/PENDENTES_RONALD.json:1587; app/docs/BACKLOG.md:1414
Q2: No celular de quem faz plantão à noite, a saída da madrugada aparece no mesmo dia que as outras marcações do plantão? app/docs/PENDENTES_RONALD.json:1587
Q3: A) Conferência passa. Admin já vê o dia pela mesma origem. O app alinha marcações e turno ao dia certo e lê o valor de folha já calculado. Alcance atual não medido. Registra a conferência. app/api/views.py:1440-1453; app/api/views.py:1475-1485; app/docs/RELATO-ARQUIVO.md:27288-27293
Q3: B) Conferência falha. Registra o dia divergente visto no app e confronta com o dia da tela do admin. O teste por si só não altera folha. Alcance atual não medido. A correção depende da falha observada [nao verificado]. app/docs/PENDENTES_RONALD.json:1587; app/docs/RELATO-ARQUIVO.md:27292-27293
Q4: não respondida quanto à conferência no aparelho. app/docs/BACKLOG.md:1414
Q5: segue. O registro da redução de avisos em 126 dos 368 colaboradores é da medição anterior do servidor, e o celular continua pendente. app/docs/RELATO-ARQUIVO.md:27288-27293

## O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL -- classe: fio morto -- tipo: corte
Q1: “esta PERGUNTA ja foi materializada de fato (clique morto)?” é a pergunta que o código já responde. O que ficou pendente foi o corte formal para registrar uma autoridade própria, se essa forma for mantida. app/docs/BACKLOG.md:1002; app/chamados/services/raiox.py:160-194
Q2: Antes de mostrar um botão ao administrador, o sistema consegue saber se a ação já fez efeito? app/chamados/services/fio.py:310-319
Q3: A) Registra o critério por pergunta separadamente. Tela mantém botão apenas quando há ação. Folha segue consultando a rota do dia. Alcance atual não medido. Formaliza o registro e conserva o critério atual. app/docs/BACKLOG.md:1002; app/chamados/services/fio.py:310-319; app/folha/fora_da_folha.py:69-73
Q3: B) Leva o leitor residual da tela à migração da tarefa do dia, que já consulta a prova do efeito. Folha já lê essa tarefa. Alcance atual não medido. Migra o leitor residual com prova de comportamento. app/docs/BACKLOG.md:2112; app/chamados/services/tarefa_do_dia.py:142-147; app/folha/fora_da_folha.py:69-73
Q4: ja respondida pelo corte CLIQUE MORTO de 02/09. O efeito efetivo tira a ação. O registro sem efeito não basta. O corte LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN trata da rota do dia. app/chamados/services/raiox.py:74-95; app/docs/CORTES.md:457; app/docs/BACKLOG.md:1002
Q5: vira complemento de FIO-E0-V4-E3-LEITORES. A tela ainda tem leitor residual, e a regra do efeito já é lida pela tarefa. app/docs/BACKLOG.md:2112; app/templates/chamados/partials/_card_atendimento.html:214; app/chamados/services/tarefa_do_dia.py:142-147

## 20/11 11:31 · sessao 31268814822e · E4C-F5 · verificador · origem=Code

Revisei os dois itens por leitura estática da cópia fornecida. O hash informado, a execução dos controles e os totais de frota ficaram [nao verificado]. O único resultado de execução usado abaixo é o log anexado. `medida4.log:9–16`

## P1 — As premissas conferem?

### Item 1 — A1(c), saída (a)

- **(a) Relógio e trava.** `_materializar_ausencia_de_resposta` cria ou obtém a ausência, lê `agora` uma vez, materializa a lista de perguntas-hora do mesmo dia e calcula `_via`. Tudo isso antecede a trava do colaborador, adquirida dentro do `atomic` de cada irmã. O mesmo `agora` vai para `carimbar` e para a linha `ABSORCAO`. A resposta humana faz o inverso na sua própria transação: trava, relê e só então lê seu relógio. `app/chamados/services/materializacao.py:999,1024–1069` · `app/chamados/services/disputa_emissao.py:1478–1495`
- **(b) Recusa e trilha.** `carimbar` consulta C6 com o carimbo aplicado em memória. Se C6 devolver viva, restaura a instância e retorna `False`. O materializador acrescenta o PK a `recusadas`, chama `evento` com `ACAO_IRMA_VIVA_NA_AUSENCIA` e devolve a lista. Há teste que exige o PK, carimbo vazio e uma linha de auditoria. O chamador do escape descarta o retorno da lista. `app/chamados/models.py:3346–3365` · `app/chamados/services/materializacao.py:1070–1094` · `app/chamados/tests/test_o261e_f2_fila.py:241–258` · `app/chamados/services/disputa_emissao.py:1557–1560`
- **(c) Dado e leitor.** A porta humana grava `PerguntaDisputa.respondida_em` e uma `DecisaoPergunta.RESPOSTA` com o mesmo `ato_em`, texto, autor e `resposta_mantem_validacao=False`. `DecisaoPergunta.leitura` ordena a sequência real e sintética e entrega o instante da resposta vigente. `composta` leva esse instante e o texto da projeção. Já existe o corte estrito de horizonte em `projecao_temporal_da_pergunta`: uma resposta vigente some da vista se seu instante for maior que o horizonte. Esse leitor entrega a distinção temporal sobre a composta, sem uma segunda ordenação. A composta não expõe a origem humana da linha e também pode trazer instante de resposta automática ou sintética. `app/chamados/services/disputa_emissao.py:1493–1508,1526–1531` · `app/chamados/models.py:4284–4289,4306–4317,4333–4362` · `app/chamados/juizes.py:1383–1403`
- **(d) B foi reproduzido.** O escape `ATESTADO` entra pela porta real. O teste interpõe a resposta `19:02` em T2 na chamada da trava e força a releitura antes do `carimbar` real com T1. Com texto e validação vazia, `carimbar` põe T1 na coluna. A leitura insere a validação sintética em T1 e a `RESPOSTA` humana de T2 a desfaz. `ABSORCAO` não repõe a validação lida. O log mostra coluna `12:00:00`, composta `None` e resposta `12:00:01`. É prova do cenário interposto com releitura. A incidência em produção ficou [nao verificado]. `medida4.log:9–16` · `app/chamados/services/disputa_emissao.py:1557–1560` · `app/chamados/models.py:3349–3372,4290–4303,4333–4351` · `app/chamados/services/materializacao.py:1063–1069`
- **(e) Grupo.** `perguntas_absorvidas_por` restringe a hora, colaborador, dia, via e carimbo, acha o grupo `(disputa_id, materializada_em)` por uma **pergunta-hora de escape carimbada** e exclui as que têm batida plantada viva. O consumidor encontrado é a devolução na rejeição da ausência. Deixar de fora uma irmã comum sem carimbo preserva o grupo quando a âncora permanece. Se a recusada for a única âncora de escape, o leitor devolve `[]`, inclusive com outras irmãs carimbadas. O escape do anexo é do tipo `escolha`, fora desse índice de busca. A sonda, portanto, não prova a conservação do grupo. `app/chamados/services/materializacao.py:1033–1036,1123–1139,1166–1169` · `app/ponto/signals.py:254–262` · `test_e4c_f5_item4_medida_relogio_comum.py:29–31`
- **(f) Releitura, sim. Prefetch antigo nesse caminho, não.** As instâncias `q` saem da consulta filtrada antes da trava. `_declarou_hora` lê o texto dessa foto, e o caminho segue direto para `carimbar` depois de travar. Uma leitura nova sob a trava é necessária para obter as colunas atuais. Não há `Prefetch('decisoes')` nessa consulta de irmãs. Fio e tarefa têm prefetch em consultas próprias. A invalidação feita por `DecisaoPergunta.registrar` vale para a instância recebida pelo escritor, não para a outra instância já materializada pela absorção. `app/chamados/services/materializacao.py:1033–1064` · `app/chamados/juizes.py:1785–1803` · `app/chamados/models.py:2841–2848,4333–4339,4376–4387` · `app/chamados/services/fio.py:260–267` · `app/chamados/services/tarefa_do_dia.py:532–538`

### Item 2 — SILÊNCIO

- **(g) Tranca e toque.** A tarefa **já lê** `dias_trancados` em lote e sai cedo com `competencia_trancada`. C6 usa C20 pelo invólucro `dia_da_pergunta_trancado`, com o mesmo corpo em lote. O juiz usa K8/K7 e consulta `LogAuditoria` depois de `Period​oFechado.fechado_em`, por colaborador-dia, exigindo usuário humano. As assinaturas são `cobrar_dia_manual` no colaborador, com `dia=...`, `veredito_celula` na célula, com `via=admin` ou `acoes`, e `desarquivar_chamado` no chamado ligado à célula. O push de **Cobrar resposta** registra uma ocorrência de cobrança, enquanto **Cobrar este dia** grava a assinatura que C20 lê. `app/core/juizes.py:304,342–344` · `app/chamados/juizes.py:979–1009,1067–1098,1242–1332,1412–1415` · `app/chamados/services/tarefa_do_dia.py:227–235,550–555` · `app/chamados/services/cobrar_dia.py:31–45` · `app/chamados/views_cobrar.py:34–57`
- **(h) E3-V10.** Confere para o **fio do colaborador-dia**. O corte manda escolher a primeira ação alcançável na ordem colab > admin e contar explicitamente o dia sem porta. Para vencimento, manda usar C9. O corte tem unidade diferente da instância da pergunta. `app/docs/CORTES.md:490` · `app/core/juizes.py:293` · `app/chamados/services/tarefa_do_dia.py:748–778`
- **(i) Botão e contador.** No modal, **Cobrar resposta** fica no bloco de disputa com permissão contextual, aberta, fora do ramo `ancora_ausente`, quando `todas_respondidas` é falso. O fio calcula esse sinal por `aguarda_o_colaborador`, que reduz as perguntas listadas que esperam texto e estão vivas pelo C6. Na Central, a lista detalhada oferece o mesmo POST na linha com `item.verbo == 'cobrar'` sem `cobrado_hoje`. O POST exige `gerir_chamados` e acesso à área. Hoje o compositor exclui o verbo residual `cobrar` e não inclui essa ação no conjunto administrativo alcançável. Já há rótulo `SEM PORTA` na Folha e contagem por **dia** no vigia. Um contador dessa classe por instância ficou [nao verificado]. `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/chamados/services/fio.py:164–190,388–395` · `app/templates/chamados/partials/_lista_chamados.html:83–84` · `app/chamados/views_cobrar.py:24–43` · `app/chamados/services/tarefa_do_dia.py:751–766` · `app/folha/fora_da_folha.py:56` · `app/chamados/services/vigia_fio.py:582–595,658–662`
- **(j) Obra registrada.** `PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA` consta no backlog como registrada, à espera de corte, com base de 2.333 **colaborador-dias** com pergunta devida vencida por C9 e sem ação administrativa alcançável. Somar perguntas ressuscitadas diretamente a esse número mistura unidades e pode incluir dias fora do recorte C9 ou contar o mesmo dia duas vezes. O cruzamento ficou [nao verificado]. `app/docs/BACKLOG.md:771` · `app/docs/CORTES.md:490` · `respostas_2011_1135.txt:7`
- **(k) Compensadora como último ato.** A fonte existente é a sequência integral já ordenada por `(ato_em, pk)` na carga, com a marca reconhecida por `DecisaoPergunta.es_da_ressuscitacao`. Para a tarefa, vale o último ato real até o `agora` da carga. Hoje `_fatos_das_perguntas` descarta a compensadora **antes** de aplicar esse horizonte. O `decisoes[-1]` de `_pela_decisao` já vê a lista filtrada, e `leitura`/`composta` não devolvem o último ato nem a marca. `app/chamados/models.py:19–25,1713–1719,4225,4246–4250` · `app/chamados/services/tarefa_do_dia.py:158–164,387–403,532–545`

**(l) Teste-alvo e controles.** O teste-alvo existe em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador`. Ele compara a razão e os trios `(acao, de, porta)` antes e depois. Não verifica o botão, o acesso do admin ou o fio publicado. Os sete módulos de controle identificáveis pelo v05 e o golden estão abaixo. “Preservável” é juízo estático sobre a expectativa, execução [nao verificado]. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320–344`

| Arquivo::teste | Expectativa sem alteração | [nao verificado]
|---|---|
| `app/chamados/tests/test_e2_tarefa_do_dia.py::test_ato_duplicado_e_a_mesma_tarefa` e `::test_a_pendencia_historica_e_do_admin_ate_a_decisao` | Preservável. Ambos usam silêncio comum, sem compensadora. `app/chamados/tests/test_e2_tarefa_do_dia.py:277–289,499–508` |
| `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py::test_RED_reabertura_marcada_nao_vira_devolvida_ao_colaborador` e `::test_MORDE_reabertura_comum_segue_devolvida_ao_colaborador` | Preservável. A base é uma resposta, e a reabertura comum continua tendo efeito próprio. `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:54–69,78–95` |
| `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py::test_RED_b_mesmo_ressuscitar_mesma_tarefa_e_mesmo_csf` | Preservável **se K6 for aplicado também à pergunta no marco ordinário**. O teste usa uma exportada com `SILENCIO` real e compara tarefa inteira e CSF. A seleção atual de K6 pode pular os marcos ordinários. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:106–120,138–161` · `app/chamados/services/tarefa_do_dia.py:598–617` |
| `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py::test_RED_silencio_decidido_nao_tem_dono_admin` e `::test_dono_admin_True_colaborador_False` | Preservável. Exercitam o silêncio comum e sua projeção sem ação administrativa. `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:59–65,92–96` |
| `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py::test_sequencias` | Preservável. `rm` é uma resposta que mantém a validação, sem compensadora. O teste conserva o recuo até o silêncio efetivo. `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:26–53` |
| `app/chamados/tests/test_e3_portas_tarefa.py::test_v8_R15_produto_decisoes_residual_A` | Preservável. Q12 fornece só `SILENCIO` e exige que a rota residual siga `porta_fila`. `app/chamados/tests/test_e3_portas_tarefa.py:721–759` |
| `app/chamados/tests/test_e3_portas_v6_sem_banco.py::test_encerrada_externa_tem_porta_do_chamado_e_passivo_preserva_pergunta` | Preservável se `silencio_decidido` continuar na guarda de encerradas. Remover essa razão muda a rota esperada. O mutante `encerrada_rota_pergunta` cobre a guarda. `app/chamados/tests/test_e3_portas_v6_sem_banco.py:98–106` · `app/chamados/tests/e3_portas_v6_mutantes.py:67–70` |
| `app/chamados/tests/f2_ressuscitar_tarefa_main.json`, consumido pelo teste F2 acima | Preservável sob a mesma condição de K6. P2 tem `silencio_decidido` com ações vazias antes e depois. `app/chamados/tests/f2_ressuscitar_tarefa_main.json:39–46,114–121` · `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155–161` |

Os outros dois controles do próprio módulo-alvo também são preserváveis: ressuscitação sem decisão e reabertura comum com o mesmo motivo textual. O código sustenta a possibilidade de mudar só a expectativa-alvo. O item escrito e os anexos ainda não demonstram esse resultado. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:340–362` · `respostas_2011_1135.txt:11`

## P2 — Avais, leis, registro e selos

### Item 1

A saída (a) é compatível com a equivalência do aval das 11h00 no caso medido e com a validação sintética já lida pela sequência. O selo F5 exige a composta nas novas chamadas ao juiz temporal ou ao juiz de resposta, e a catraca nominal de colunas cruas não inclui `_materializar_ausencia_de_resposta`. **`Juízes novos=0` fecha para a comparação temporal da resposta vigente** usando os leitores existentes. O mesmo não está demonstrado para um predicado que exija reconhecer especificamente a **origem humana** do ato. A composta não retorna essa origem, e o registro C21 responde se há instante e texto. Uma regra paralela de sequência ou uma autoridade nova ficaria sob L-002 e L-011, ainda que o teste nominal não a detectasse. `respostas_2011_1100.txt:4` · `app/chamados/models.py:4306–4317,4354–4362` · `app/chamados/juizes.py:1383–1403` · `app/core/juizes.py:305` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:15–25,298–307` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:147–149,188–193` · `app/docs/LEIS.md:60,69` · `CLAUDE.md:314–315`

### Item 2

**`Juízes novos=0` fecha para “trancada sem TXT que o admin não tocou”** pela combinação de C20 e K6/K8, e é compatível com **“ressuscitada em silêncio”** como composição de C19 sobre a sequência ordenada, o horizonte e a marca existente. Para **“cobrar alcançável”**, o registro tem o juiz do dia e o código tem os insumos das telas. Uma resposta compartilhada que cubra a oferta real por chamado, a permissão e a sua atribuição à instância ficou [nao verificado]. A presença de um PK na fila é insuficiente. O selo E3 veda repetir o predicado do app, e o selo da cobrança guarda a rota existente e o ramo do modal. Contar só a partir da tarefa exige a mesma autoridade que a tela usa. `app/core/juizes.py:301–305,342–344` · `app/chamados/services/tarefa_do_dia.py:387–403,734–766` · `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/chamados/views_cobrar.py:24–43` · `app/chamados/tests/test_e3_degrau_dono_e_porta.py:380–385` · `app/chamados/tests/test_selo_painel_oferece_cobrar.py:173–193` · `app/docs/LEIS.md:60,69,149`

O aval de 11h35 conserva a precedência do dia de E3-V10/L-129 e a compensadora da F2. A colisão é de **cobertura de dono**: o aval de 11h00 manda ADMIN em aberta e o corte da ressuscitação exige destino para a pergunta. O item novo contém dois destinos sem um dono nomeado, detalhados em P3. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6–10` · `app/docs/CORTES.md:490,543–545` · `app/docs/LEIS.md:65,149`

## P3 — O literal fecha?

### Item 1

**A parte de dados é alcançável no caso medido** com a trava e a ordem atuais. A resposta humana deixa `validada_em` nulo e grava a linha que desfaz a validação. Sem aplicar o carimbo da absorção nessa irmã, coluna e composta podem ficar nulas, sem ato de reparo e com T1 preservado nas demais absorções. O log prova B no caminho antigo, e o verde desse contrafactual ficou [nao verificado]. O teste anexado ainda exige que `carimbar` seja chamado uma vez e, no fim, só asserta ausência de B. A expectativa atual, sozinha, não prova a recusa, a trilha ou o grupo. `app/chamados/services/disputa_emissao.py:1481–1508,1526–1531` · `app/chamados/services/materializacao.py:1033–1069` · `app/chamados/models.py:4333–4351` · `test_e4c_f5_item4_medida_relogio_comum.py:53–61` · `medida4.log:9–16`

**O conjunto literal não fecha.** Na própria sonda `19:02` concorda com a luz `19:00`. C6 pode dar a pergunta por encerrada pelo fato mesmo sem carimbo, e `PerguntaDisputa.estado` com resposta vigente e sem carimbo devolve `RESPONDIDA`. Assim, **“fora da absorção”** é demonstrável como efeito da recusa, enquanto **“pendente/viva”** como estado não decorre dela. A trilha reaproveitada também é best-effort, fora do `atomic` da irmã, e a descrição atual atribui a recusa a C6. Por fim, deixar a irmã intocada não garante coluna vazia se houver uma validação legítima posterior já gravada sob a mesma ordem de trava. `test_e4c_f5_item4_medida_relogio_comum.py:24–33,42–44` · `app/chamados/juizes.py:1335–1380,1426–1440` · `app/chamados/models.py:3249–3268` · `app/chamados/services/materializacao.py:1060–1076` · `app/core/observ.py:94–105,133–139` · `app/chamados/services/validacao.py:163–174,752`

### Item 2

1. **Trancada sem TXT.** K6 falso e C20 verdadeiro podem coexistir. Pela definição do aval de 11h00, esse caso é “em aberta” e iria a ADMIN. Pela regra de 11h35, a instância fica arquivada, sem pergunta e sem ação no resultado da tarefa. O toque devolve o dia ao julgamento, mas **Cobrar resposta** não é o toque de C20. A saída antecipada também não impede, por si, que a composição publique uma porta residual de outro chamado do dia. Logo, arquivar a instância não dá destino à pergunta, e eventual dono do dia não supre essa lacuna. Quantidade e casos nominais [nao verificado]. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6` · `app/core/juizes.py:342–344` · `app/chamados/services/tarefa_do_dia.py:227–235,678–691,734–767` · `app/chamados/juizes.py:1088–1098` · `app/chamados/views_cobrar.py:44–57`
2. **“Sem porta” por instância.** O contrato geral da tarefa permite `acoes=[]` e fio `nada`. A obrigação específica do corte é que a **pergunta ressuscitada** tenha destino. Uma ressuscitada em aberta, sem botão alcançável, termina no literal de 11h35 com rótulo e contador, sem ADMIN nessa instância e sem DP, reservado ao caso entregue. A precedência colab > admin continua válida no fio do dia, mas não resolve essa falta de dono na pergunta. `app/chamados/services/tarefa_do_dia.py:42–54,766–774` · `app/docs/CORTES.md:490,545` · `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:7–10`

## P4 — Exportada, dinheiro, `folha/`, escala/vínculo e `!`

### Item 1

A decisão de não carimbar a irmã e registrar a recusa fica em Chamados. O caminho real, porém, já cria ou reutiliza uma ausência **antes** da absorção e pode reconciliar o destino depois da recusa. A criação pode acionar o cartório e, se o insumo da Folha mudar, invalidar `FechamentoMensal.derivado_em`. O `UPDATE` da invalidação não filtra por K6. Isso comprova alcance indireto sobre o gravado no caminho existente, não um delta imputável à saída (a). Alteração efetiva em exportada, valores, escala/vínculo ou necessidade de `!` para esse delta ficaram [nao verificado]. Escrita no gravado exportado ou em dado de escala/vínculo tem a cerca expressa no `CLAUDE.md`. `app/chamados/services/materializacao.py:1024–1033,1070–1089` · `app/ponto/signals.py:143–147` · `app/ponto/services/ausencia.py:154–164,195,594–599` · `app/ponto/services/fechamento.py:1648–1656` · `CLAUDE.md:265–268,404–415`

### Item 2

A publicação de tarefa e o destino textual do DP são de leitura no aval. K6 cobre a entrada da exportada, mas a carga atual consulta K6 para um recorte que pode deixar de fora o marco ordinário. A Folha já consome o fio publicado, e o retrato não tem tradução própria para `cobrar`, podendo cair no link legado. Há, portanto, alcance sobre **a leitura** da Folha, que o aval de 11h00 manda registrar. Edição de `folha/`, execução de toque administrativo, escrita em exportada, dinheiro ou dado de escala/vínculo fora do declarado ficaram [nao verificado]. `respostas_2011_1100.txt:5–6` · `respostas_2011_1135.txt:6–10` · `app/docs/CORTES.md:545` · `app/chamados/services/tarefa_do_dia.py:563–578,598–619` · `app/folha/fora_da_folha.py:71–73,275–280` · `app/folha/retrato_folha.py:1015–1021,1044–1048,1066–1071` · `CLAUDE.md:265–268`

**Item 1 — PROCEDE (AVAL-DEVOLVIDO).** A recusa evita o carimbo que produz B na sonda, mas não entrega o estado pendente exigido, e a trilha reaproveitada é best-effort. `medida4.log:16` · `app/chamados/models.py:3259–3268` · `app/core/observ.py:94–105,133–139`

**Item 2 — PROCEDE (AVAL-DEVOLVIDO).** Tranca sem TXT e ausência de botão deixam classes de ressuscitadas em aberta sem o dono exigido pelo corte da pergunta. E3-V10 autoriza contar o **dia** sem porta. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6–8` · `app/docs/CORTES.md:490,545`

## 20/11 11:24 · sessao 0014d54c272b · CODEX-FECHA-A-VOLTA-V1-RASCUNHO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 11:20 · sessao fd2a82382307 · E4C-F5-CONSTROI · construtor · origem=Code

## Achados

1. **`PerguntaDisputa.carimbar` pode produzir B** — coluna preenchida, composta sem validação. Se `quando` antecede uma `RESPOSTA` que desfaz validação, a leitura aplica a validação sintética e depois a apaga. A absorção abre essa janela ao capturar `agora` e carregar as perguntas antes da trava, passando o instante anterior para `carimbar`. O C6 pode aceitar o carimbo pela concordância com o marco. [chamados/models.py:3344](chamados/models.py#L3344) · `chamados/models.py:3355`, `:4339–4348` · `chamados/services/materializacao.py:1033–1036`, `:1060–1064` · `chamados/juizes.py:1428–1440`
2. **`reabrir_para_correcao` pode preservar A preexistente** — composta validada, coluna vazia. Se só existe a `VALIDACAO` física e a projeção já está sem texto, resposta, validação e materialização, o filtro exclui a pergunta da compensação. O lote grava a coluna nula e a validação física segue vigente. [chamados/services/disputa_emissao.py:1636](chamados/services/disputa_emissao.py#L1636) · `chamados/services/disputa_emissao.py:1650–1653` · `chamados/models.py:4342–4348`

Os dois caminhos têm REDs dedicados e ficaram sem cura de produção nesta volta. [chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512](</home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512>) · `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:532`

## Censo

HEAD lido por `git rev-parse HEAD`: `8f84213791cde624afb4c4d1c2d496c730fae0df`. Os caminhos abaixo são relativos a `app/`. A varredura encontrou nove sítios de atribuição ou envio ao lote e cinco `save` associados. `composta` usa a validação de `leitura`, que ordena atos por instante, dá às linhas sintéticas a última posição no empate e aplica os atos que desfazem a validação. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:31`, `:452` · `chamados/models.py:4333–4351`, `:4354–4362`

| Escrita | Função | Efeito | Evidência na mesma função ou transação |
|---|---|---|---|
| `chamados/services/validacao.py:168`, save `:171` | `_carimbar_validacao` | PREENCHE | `registrar` grava `VALIDACAO` com o instante da coluna no mesmo `atomic`. `chamados/services/validacao.py:170–175` |
| `chamados/services/validacao.py:287`, save `:290` | `devolver_validacao_recusada` | LIMPA | A guarda entra se coluna ou composta estiver validada. O `atomic` salva a coluna nula e registra `DEVOLUCAO`, que apaga a validação vigente. Cobre A e B separadamente. `chamados/services/validacao.py:277–296` · `chamados/models.py:4302` |
| `chamados/services/validacao.py:365`, save `:367` | `devolver_paridade_bug_par_103` | LIMPA | Sob a trava, salva a coluna nula e registra `DEVOLUCAO` com a foto anterior no mesmo `atomic`. `chamados/services/validacao.py:347–348`, `:361–374` |
| `chamados/services/disputa_emissao.py:1506`, save `:1526` | `responder_pergunta_disputa_core` | LIMPA | Depois da trava, a `RESPOSTA` recebe o instante da resposta e `resposta_mantem_validacao=False`. O registro e o `save` ficam no mesmo `atomic`. `chamados/services/disputa_emissao.py:1478–1493`, `:1526–1531` · `chamados/models.py:4317` |
| `chamados/services/disputa_emissao.py:1614`, lote `:1653` | `reabrir_para_correcao` | LIMPA | Registra `REABERTURA` ou `RECUSA` antes do lote, no mesmo `atomic`, para as linhas que passam no filtro da foto. A isolado fica de fora conforme o achado 2. `chamados/services/disputa_emissao.py:1624–1626`, `:1636–1653` · `chamados/models.py:4189`, `:4302` |
| `chamados/models.py:1582` | `ChamadoColaborador._universo_da_ressuscitacao` | LIMPA em cópia | A sonda limpa a cópia e troca seu cache pela sequência posterior ao instante simulado da reabertura. O C6 recebe essa cópia. `chamados/models.py:1558`, `:1569`, `:1581–1595` |
| `chamados/models.py:1721` | `ChamadoColaborador.ressuscitar_perguntas` | LIMPA | `registrar` preserva a foto e grava a `REABERTURA` compensadora no universo relido sob trava. `descarimbar_em_lote` encaminha os campos ao `update` no mesmo `atomic`. `chamados/models.py:1646–1651`, `:1669–1674`, `:1702–1722`, `:3508–3511` |
| `chamados/models.py:3355`, save `:3372` | `PerguntaDisputa.carimbar` | PREENCHE | A validação sintética vem da coluna quando falta linha física correspondente. A equivalência fecha quando o carimbo prevalece na sequência. Um ato posterior pode produzir B conforme o achado 1. `chamados/models.py:3354–3377`, `:4290–4294`, `:4333–4348` |
| `chamados/models.py:3384` | `PerguntaDisputa._desfazer_em_memoria` | RESTAURA em memória | Repõe o valor que `carimbar` capturou antes da tentativa. Os caminhos de recusa ou exceção chamam essa restauração. `chamados/models.py:3346–3347`, `:3358–3365`, `:3373–3386` |

## Arquivos e verificação

- [test_e4c_f5_selo_par_ato_coluna.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452) — `SimpleTestCase` com descoberta por AST, resolução de aliases e regras por classe. O censo só alimenta a anti-vacuidade. O selo segue um nível para um auxiliar local que chame `registrar`, confere a foto de `carimbar` usada por `_desfazer_em_memoria` e confere o repasse do lote ao `update`. Os escritores persistentes do censo chamam `registrar` diretamente quando gravam o ato. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:31`, `:260`, `:318`, `:452`, `:499`
- [e4c_f5_voltaE_mutantes.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:6) — três patches de fonte em memória, cada um com `assert fonte.count(antes) == 1`. A checagem estática confirmou um diagnóstico por mutante no teste `test_escritores_fecham_o_par`. `limpa_sem_ato` acusa `ressuscitar_perguntas`; `preenche_e_desfaz` acusa `_carimbar_validacao`; `escritor_novo` acusa `_limpar_por_fora`. `chamados/tests/e4c_f5_voltaE_mutantes.py:6–38` · `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452–462`

## Comandos para o Code

O primeiro comando é o **GREEN esperado da varredura AST na base**. O segundo executa o módulo inteiro, com os dois REDs dos achados esperados no HEAD. Os três últimos isolam os mutantes no teste AST. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452`, `:512`, `:532` · `chamados/tests/e4c_f5_voltaE_mutantes.py:5`

```bash
selo_e=chamados.tests.test_e4c_f5_selo_par_ato_coluna
teste_ast="$selo_e.SeloParAtoColunaTest.test_escritores_fecham_o_par"

env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$selo_e" --settings=config.settings.ci

F5_VOLTA_E_MUTANTE=limpa_sem_ato PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
F5_VOLTA_E_MUTANTE=preenche_e_desfaz PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
F5_VOLTA_E_MUTANTE=escritor_novo PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
```

**[nao verificado]** Execução pelo runner Django, suíte, banco e ocorrência dos dois caminhos em produção. A checagem local executou somente a varredura AST, os patches em memória e trechos isolados do leitor e do filtro de reabertura. `chamados/models.py:4320–4362` · `chamados/services/disputa_emissao.py:1636–1651`

## 20/11 11:17 · sessao e099536c8d72 · CODEX-FECHA-A-VOLTA-V1-RASCUNHO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 10:59 · sessao f9ea3a01eb41 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a volta **E-CONTADOR** na base `48be81433dab4bb4ee9641e2daeedd22acf1cbff`. O contador lê `DecisaoPergunta.composta(p).validada_em` e `p.validada_em` uma vez por pergunta, com `decisoes` carregadas em lotes de 2.000 por padrão. A e B têm esperado `0` e dono `ESTRUTURA`. [vigia_ato_x_coluna.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/vigia_ato_x_coluna.py:8)

O `vigiar_fio_morto` passa o mesmo queryset do contador de texto dentro de `tranca_em_lote()`, imprime `VFM|ATO_X_COLUNA|A=%d B=%d (esperado 0 e 0)|universo=%d` e publica `ato_x_coluna_a` e `ato_x_coluna_b` no `censo` e no `esperado` do carimbo. As duas chaves ficam fora de `ALARMA_SEMPRE` e `ALARMA_EM_0`. O encaixe usa o comando e o cron existentes. `app/chamados/management/commands/vigiar_fio_morto.py:79`, `:104`, `:188`, `:233`, `:274`, `:283`, `app/config/crons.py:1737`.

### Leitura e escritores

O passivo com coluna preenchida e zero decisões cai em **`(True, True)`**. `leitura(p)` acrescenta a validação sintética no instante da coluna e `composta(p)` devolve essa validação. A pergunta soma um ao universo e zero a A e B. `app/chamados/models.py:4290`, `:4333`, `:4345`, `:4360`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:63`.

| Campo ou caminho | Escrita na cópia |
|---|---|
| `PerguntaDisputa.validada_em` na validação e nas devoluções | `_carimbar_validacao` grava a coluna e `VALIDACAO` na mesma transação. `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` limpam a coluna e registram `DEVOLUCAO` na mesma transação. `app/chamados/services/validacao.py:168`, `:287`, `:347`, `:365`. |
| `PerguntaDisputa.validada_em` na resposta, reabertura, recusa e ressuscitação | A resposta humana limpa a coluna e registra `RESPOSTA` com `resposta_mantem_validacao=False` no mesmo `atomic`. Reabertura e recusa limpam pelo lote e registram o ato no mesmo `atomic`. A ressuscitação registra `REABERTURA` e limpa pelo lote no mesmo `atomic`. `app/chamados/services/disputa_emissao.py:1478`, `:1506`, `:1529`, `:1626`, `:1636`, `:1653`, `app/chamados/services/validacao.py:943`, `app/chamados/models.py:1646`, `:1719`, `:3509`. |
| `PerguntaDisputa.validada_em` no carimbo com `resposta='validar'` | `carimbar` persiste a coluna. A validação sintética vem da leitura, conforme o aval das 11:00. A sonda da ressuscitação e `_desfazer_em_memoria` atribuem o campo em memória. `app/chamados/models.py:1582`, `:3354`, `:3372`, `:3384`, `:4290`. |
| `DecisaoPergunta.composta(p).validada_em` | `composta` produz o valor em memória a partir de `leitura`. `DecisaoPergunta.registrar` persiste as linhas de ato que alimentam a leitura. `app/chamados/models.py:4333`, `:4345`, `:4360`, `:4365`. |

### Testes e leitores [nao verificado]

- O [teste novo](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:19) cobre porta real, A, B, passivo, recorte inclusive vazio, carimbo com `--alarme` e custo. O teste do comando captura o payload de `_carimbar` em memória, com A=1 e B=2 para detectar troca de chaves. O teste O261 confere o JSON em disco. `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:47`, `:78`, `:99`, `app/chamados/tests/test_o261_vigia_fio.py:826`.
- Os dois inventários de dublês ganharam a chamada nova. `test_e4a2_c5_memoria` executa o `handle` extraído por AST e `test_fio_mudo_placar` substitui as derivações do vigia. O contrato O261 ganhou as duas chaves nas comparações integrais de `censo` e `esperado`, com os valores anteriores preservados. `app/chamados/tests/test_e4a2_c5_memoria.py:132`, `app/core/tests/test_fio_mudo_placar.py:128`, `app/chamados/tests/test_o261_vigia_fio.py:816`, `:837`, `:851`, `:881`.
- O selo de chamadas F5 varre fontes por chamadas a dois juízes. O contrato do dono de `validada_em` varre escritas. O contrato do relógio do vigia seleciona `censo` e `censo_alcance`. A inclusão dispensa alterações nesses contratos. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126`, `:152`, `app/chamados/tests/test_contract_dono_validada.py:47`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:389`.
- Fora de `app/`, `bin/fio_estado.py` lê o JSON, percorre todas as chaves de `esperado` e publica as divergências. `bin/gerar_estado.py` usa esse leitor. Ambos já aceitam A e B sem edição. O teste de `bin/` verifica o caminho e campos específicos do alcance. Dentro de `app/`, o `pipeline_placar` lê as chaves específicas `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. `bin/fio_estado.py:98`, `:121`, `:191`, `bin/gerar_estado.py:260`, `bin/tests/test_fio_estado.sh:117`, `:203`, `app/core/management/commands/pipeline_placar.py:85`, `:135`.

### Comandos para o Code

**GREEN e vizinhos**, a partir de `app/`. Os casos novos estão em `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:47`.

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4c_f5_contador_ato_x_coluna

PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4c_f5_contador_texto \
  chamados.tests.test_f7_vigia_validada_sem_linha \
  chamados.tests.test_o261_vigia_fio \
  chamados.tests.test_e4a2_c5_memoria \
  core.tests.test_fio_mudo_placar \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_contract_dono_validada
```

**RED por mutante.** A primeira troca faz A sair `0` no caso `(True, False)`. A segunda faz B sair `0` no caso `(False, True)`. Cada comando exige uma âncora de linha única e aplica a troca em memória. A falha esperada é na asserção de contagem. `app/chamados/services/vigia_ato_x_coluna.py:17`, `:19`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:55`, `:59`.

```bash
mutar_contador() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python - "$@" <<'PY'
import sys
from pathlib import Path

import django
from django.conf import settings
from django.test.utils import get_runner

django.setup()
from chamados.services import vigia_ato_x_coluna

antes, depois, teste = sys.argv[1:] [nao verificado]
fonte = Path(vigia_ato_x_coluna.__file__)
texto = fonte.read_text(encoding='utf-8')
linhas = [linha for linha in texto.splitlines(keepends=True) if linha.strip() == antes]
if len(linhas) != 1:
    raise SystemExit(f'Âncora inválida: {antes!r}, ocorrências: {len(linhas)}')
(linha,) = linhas
mutado = texto.replace(linha, linha.replace(antes, depois, 1), 1)
escopo = vars(vigia_ato_x_coluna).copy()
exec(compile(mutado, str(fonte), 'exec'), escopo)
vigia_ato_x_coluna.contar.__code__ = escopo['contar'].__code__
prefixo = 'chamados.tests.test_e4c_f5_contador_ato_x_coluna.ContadorAtoXColunaTest.'
sys.exit(get_runner(settings)(interactive=False).run_tests([prefixo + teste])) [nao verificado]
PY
}

mutar_contador 'if celula == (True, False):' 'if celula == (False, True):' \
  test_RED_a_ato_validado_coluna_vazia

mutar_contador 'elif celula == (False, True):' 'elif celula == (True, False):' \
  test_RED_b_coluna_preenchida_ato_devolvido
```

**Mutante `retry_reativa` do aval**, pelo executor existente. `app/chamados/tests/bug_par_mutantes.py:68`, `:189`, `app/chamados/services/validacao.py:702`.

```bash
PYTHONDONTWRITEBYTECODE=1 python -m chamados.tests.bug_par_mutantes \
  --django --mutante retry_reativa
```

Conferi a sintaxe dos seis arquivos e a unicidade das duas âncoras sem carregar Django. Banco, suíte, execução dos mutantes, custo observado e nova medição de frota **[nao verificado]**. O `A=0`, `B=0` em 51.953 perguntas é o dado do anexo fornecido. A remedição e a certificação ficam com Code e Opus. [nao verificado]

## 20/11 10:56 · sessao e05141ea45e1 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2, F5 SILÊNCIO.** O corte autoriza ADMIN com Cobrar na tarefa do dia, mas o literal ainda deixa em aberto a precedência da tranca e o que fazer quando o botão existente fica inacessível. A ordem de reescrever o golden também alcança um controle de competência entregue. `respostas_2011_1100.txt:5`, `app/chamados/services/tarefa_do_dia.py:227`, `app/templates/chamados/partials/modal_fio.html:354`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:106`

Identidade da cópia com o hash informado [nao verificado].

## P1 — As premissas conferem?

- **(a) Os quatro literais conferem no conteúdo.** Aberta é competência sem TXT vigente pelo K6, inclusive aprovada ou trancada sem TXT e com TXT invalidado sem substituto. A “lista dele” é a tarefa do dia. Cobrar é o botão existente. Entregue pelo K6 vai à lista textual do DP, com “paga fora” adiada até existir o juiz correspondente. A publicação às 10:33 ficou [nao verificado]. `respostas_1711_1410.txt:6`, `respostas_1711_1150.txt:2`, `respostas_1711_1120.txt:2`
- **(b) O encaixe A confere.** É a primeira proposta da seção 7 do v05. Transporta a marca pela carga e publica Cobrar como ação explícita de ADMIN na composição, com âncora alcançável e projeção na Central. `r.txt:60`
- **(c) A origem confere.** `CargaDaTarefa` carrega a sequência de atos ordenada por `ato_em, pk`. `_fatos_das_perguntas` reconhece a compensadora e a descarta antes do corte por `agora`. `_pela_decisao` devolve `silencio_decidido` sem ação. `_portas_da_tarefa` compõe o dono, a rota e a fila do dia. `app/chamados/services/tarefa_do_dia.py:532`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:724`
- **(d) Cobrar já existe.** Central e modal usam o POST `cobrar_chamado`. O gestor autorizado pelo departamento aciona o botão, o colaborador recebe o push e deve a resposta. A Central trata o verbo residual `cobrar` como pendência do colaborador, fora do balão ADMIN. O POST consulta a cobrança anterior por colaborador e dia e registra a ocorrência após o envio bem-sucedido. `app/templates/chamados/partials/_lista_chamados.html:83`, `app/templates/chamados/partials/modal_fio.html:378`, `app/chamados/views_cobrar.py:24`, `app/chamados/views_cobrar.py:34`, `app/chamados/services/verbos.py:20`
- **(e) O juiz existe e a tarefa já o lê.** K6 responde se existe TXT vigente. “Aberta” é o complemento dessa resposta para a competência identificada por K8. `_ler_exportadas` já importa o leitor de `folha.export` e os modelos de leitura de `folha.models`, consulta em lote e chama K6 sem escrita nem alteração em `folha/`. `app/core/juizes.py:185`, `app/core/juizes.py:342`, `app/folha/export.py:556`, `app/folha/export.py:580`, `app/chamados/services/tarefa_do_dia.py:563`
- **(f) Há formatador de texto, mas gerador pronto para (c) ficou [nao verificado].** `partir_em_pautas` recebe linhas prontas. A aba DP lê `Pauta` gravada. O comando mais próximo imprime no `--dry`, seleciona o beco de folga indefinida e usa `escrever` no modo de aplicação. O corte pede PK e `colNNN`, sem escrita. Com a evidência disponível, vale a alternativa do aval de nomear DP e registrar a lista como obra própria. `app/pautas/services.py:103`, `app/pautas/views.py:29`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:92`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:119`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:130`, `app/docs/CORTES.md:545`, `respostas_2011_1100.txt:5`

**(g) O conjunto de oito módulos e um golden que a leitura permite identificar:**

| Arquivo::teste | O que o assert cobre | [nao verificado]
|---|---|
| `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador` | Compara razão e ações da mesma pergunta antes e depois da ressuscitação. É a expectativa diretamente atingida por (b). `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343` |
| `app/chamados/tests/test_e2_tarefa_do_dia.py::test_ato_duplicado_e_a_mesma_tarefa` | Silêncio comum duplicado conserva `silencio_decidido` e `[]`. `app/chamados/tests/test_e2_tarefa_do_dia.py:277` |
| `app/chamados/tests/test_e3_portas_tarefa.py::test_v8_R15_produto_decisoes_residual_A` | Q12 conserva `silencio_decidido` com rota residual `porta_fila`. `app/chamados/tests/test_e3_portas_tarefa.py:721`, `app/chamados/tests/test_e3_portas_tarefa.py:733`, `app/chamados/tests/test_e3_portas_tarefa.py:756` |
| `app/chamados/tests/test_e3_portas_v6_sem_banco.py::test_encerrada_externa_tem_porta_do_chamado_e_passivo_preserva_pergunta` | Silêncio com âncora externa usa `porta_fila`. `app/chamados/tests/test_e3_portas_v6_sem_banco.py:98` |
| `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py::test_sequencias` | Uma resposta que preserva validação deixa o silêncio efetivo no caso da matriz. `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34` |
| `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py::test_RED_silencio_decidido_nao_tem_dono_admin`, `::test_dono_admin_True_colaborador_False` | Silêncio sem compensadora resulta em `aguarda_admin=False`. `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:59`, `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:92` |
| `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py::test_RED_reabertura_marcada_nao_vira_devolvida_ao_colaborador`, `::test_MORDE_reabertura_comum_segue_devolvida_ao_colaborador` | A fixture é respondida. O teste distingue compensadora de reabertura comum. `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:54`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:83` |
| `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py::test_RED_b_mesmo_ressuscitar_mesma_tarefa_e_mesmo_csf` | Com K6 verdadeiro, compara a tarefa inteira e o CSF antes e depois com o golden. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:106`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:151`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:158` |
| `app/chamados/tests/f2_ressuscitar_tarefa_main.json` | Golden de entregue. A pergunta P2 tem `silencio_decidido` e `acoes: []` antes e depois. `app/chamados/tests/f2_ressuscitar_tarefa_main.json:39`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:114` |

**PROCEDE.** O 1150 manda reescrever a expectativa do caso F5 SILÊNCIO. A ordem de reescrever os oito módulos e o golden como se todos afirmassem a mesma expectativa inclui controles de silêncio comum, de pergunta respondida e de competência entregue. `respostas_1711_1150.txt:2`, `respostas_2011_1100.txt:5`, `app/chamados/tests/test_e2_tarefa_do_dia.py:277`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:83`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:109`

## P2 — Colisões e «juízes novos = 0»

- **Zero juízes novos fecha para o recorte definido.** `es_da_ressuscitacao` já identifica a compensadora. C19 já julga o silêncio efetivo e responde quem age e por qual porta. K8 e K6 já respondem a competência e a entrega. O registro carece de um juiz separado com a frase “ressuscitada em silêncio decidido”, mas essa combinação pertence à pergunta já registrada em C19. `app/chamados/models.py:4246`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:182`, `app/core/juizes.py:158`, `app/core/juizes.py:303`, `app/core/juizes.py:342`
- **Os selos delimitam o encaixe.** A catraca F5 cobra leitura pelos instantes da composta nos sítios nominados e nos chamadores dos dois juízes L-130. O selo de juiz único e o de leitor sem derivação própria vedam decidir dono ou competência de novo na tela. O tripwire da marca reserva a escrita à ressuscitação. A bateria da tarefa cobra zero escrita, relógio na borda e zero SQL de Folha ou `FechamentoMensal` no caso ordinário testado. Consultar K6 indiscriminadamente nesse caminho faria esse caso falhar. A leitura seletiva já tem precedente e selo próprio. `app/chamados/tests/test_e4c_f5_selo_catraca.py:298`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:189`, `app/docs/LEIS.md:60`, `app/docs/LEIS.md:151`, `CLAUDE.md:20`, `app/chamados/tests/test_contract_juiz_chamado.py:43`, `app/core/tests/test_medida_leitor_do_dia.py:39`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:66`, `app/chamados/tests/test_e2_tarefa_do_dia.py:518`, `app/chamados/tests/test_e2_tarefa_do_dia.py:879`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1010`, `app/chamados/tests/test_e3_c8b_sem_banco.py:457`
- **As colisões literais são o golden e a porta alcançável.** O 1120 preserva a F2 e autoriza a exceção do silêncio em aberta na F5. Se “nomeia DP” alterar a saída comparada em entregue, o golden também muda. E a regra vigente de dono do dia exige ação alcançável, com precedência do colaborador. Publicar ADMIN incondicionalmente contraria essa regra. `respostas_1711_1120.txt:2`, `respostas_1711_1120.txt:3`, `app/docs/CORTES.md:490`, `app/docs/CORTES.md:543`, `app/docs/CORTES.md:545`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:159`, `app/chamados/services/tarefa_do_dia.py:748`, `app/chamados/services/tarefa_do_dia.py:766`

**PROCEDE.** A autorização para mudar o caso aberto está dada. A reescrita do golden de entregue e ADMIN como dono incondicional do dia excedem esse recorte. `respostas_1711_1150.txt:2`, `app/docs/CORTES.md:490`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:109`

## P3 — O literal fecha na operação?

- **O fato é legível, mas o alcance ainda precisa fechar.** A trilha distingue `REABERTURA` marcada de reabertura comum e permite ordenar por `(ato_em, pk)` até `agora`. A tarefa já distingue silêncio efetivo, ausência de ato e passivo sem linha. O literal deixa em aberto se uma compensadora histórica ainda qualifica depois de ato ordinário posterior. Também abrange trancada sem TXT, enquanto C20 encerra a tarefa cedo se o admin ainda não tocou o dia. Sem data ou empresa, a abertura pelo K6 fica indecidível. `app/chamados/models.py:1713`, `app/chamados/models.py:4224`, `app/chamados/models.py:4246`, `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:227`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/services/tarefa_do_dia.py:567`, `app/chamados/services/vigia_fio.py:789`, `respostas_1711_1410.txt:6`
- **A saída de (b) é na própria tarefa do dia.** Hoje a instância ordinária fica sem ação exigível. O aval atribui Cobrar ao ADMIN nessa tarefa. A `fila` pública contém chamados das portas do dono escolhido para o dia. Se outro marco tiver resposta de colaborador alcançável no app, esse colaborador tem precedência no dia. A Central seleciona chamados por status e verbo próprios e pode manter a âncora fora da lista padrão. `respostas_1711_1150.txt:2`, `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:748`, `app/chamados/services/tarefa_do_dia.py:766`, `app/chamados/services/tarefa_do_dia.py:791`, `app/chamados/services/painel.py:300`
- **Central e modal compartilham o POST, com critérios distintos de exibição.** A Central oferece Cobrar pela partição de verbos. O modal usa a âncora, a disputa aberta, o ramo sem `ancora_ausente` e a espera de resposta lida pelo fio. A ressuscitação reabre a disputa das perguntas afetadas, mas isso por si só deixa sem prova a alcançabilidade do botão para todo caso do aval. `app/chamados/services/verbos.py:30`, `app/templates/chamados/partials/_lista_chamados.html:83`, `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:164`, `app/chamados/services/fio.py:388`, `app/chamados/models.py:1729`, `app/templates/chamados/partials/modal_fio.html:354`
- **Com TXT vigente, o destino do corte é DP.** O ramo ADMIN/Cobrar da F5 fica fora desse recorte. Hoje K6 protege as extensões administrativas da tarefa, enquanto o silêncio do marco ordinário continua sem ação. O envelope público da tarefa carece de destino DP próprio. O aval determina a lista textual sem escrita e prevê registrar sua construção separadamente quando faltar gerador. `respostas_1711_1120.txt:2`, `respostas_2011_1100.txt:5`, `app/docs/CORTES.md:545`, `app/chamados/services/tarefa_do_dia.py:273`, `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:809`

**PROCEDE.** Falta fechar a precedência de C20 para trancada sem TXT e a saída quando a instância se qualifica, mas o botão existente fica inacessível. O literal também precisa distinguir o dono da instância do dono do dia. `app/chamados/services/tarefa_do_dia.py:227`, `app/chamados/services/tarefa_do_dia.py:748`, `app/chamados/services/tarefa_do_dia.py:766`, `app/templates/chamados/partials/modal_fio.html:354`, `app/docs/CORTES.md:490`

## P4 — Exportada, dinheiro, escala/vínculo, `folha/` e `!`

- O escopo declarado lê K6 nas entregues, nomeia DP e condiciona a lista textual ao caminho sem escrita. A tarefa é leitora. O ato de Cobrar já autorizado pelo corte é o push com ocorrência no chamado. Nesse caminho, a única escrita acionada pelo botão é a ocorrência. Alterar o gravado da exportada, dinheiro ou dados de escala/vínculo fica fora do item. `respostas_2011_1100.txt:5`, `app/docs/CORTES.md:545`, `app/chamados/services/tarefa_do_dia.py:830`, `app/chamados/views_cobrar.py:44`, `app/chamados/services/nota.py:83`, `app/docs/LEIS.md:125`, `CLAUDE.md:265`
- O retrato é o consumidor indireto já declarado. A Folha seleciona primeiro os dias que retêm e depois lê a porta da tarefa. Em próxima lavra, o retrato pode refletir dono e ação dos dias já retidos. Seu mapa atual carece de entrada para `cobrar` e pode renderizar o rótulo legado “Validar no fio”. O impacto na frota e a contagem por K6 exigida antes do PRONTO estão [nao verificado]. `respostas_2011_1100.txt:5`, `app/folha/fora_da_folha.py:200`, `app/folha/fora_da_folha.py:215`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1066`, `app/folha/retrato_folha.py:1246`, `app/folha/management/commands/lavrar_retrato_folha.py:72`

**SEGUE.** No recorte declarado, a leitura de K6, a lista sem escrita e o botão existente dispensam `!` adicional. O efeito indireto no retrato já consta do aval. `respostas_2011_1100.txt:5`, `app/docs/CORTES.md:545`, `CLAUDE.md:265`

## 20/11 10:56 · sessao 8297cc8fec11 · FERIADO-POR-POSTO · verificador · origem=Code

Revisão estática dos itens 3 e 4 de `RESPOSTAS-2011-1100`. A cópia foi identificada pelo hash fornecido. Confirmação independente do commit, execução dos REDs, remedição dos 9 e 18 e estado em produção [nao verificado]. `bin/codex_pergunta.sh:304,311-319`, `respostas_2011_1100.txt:9-11`.

## P1. As premissas conferem?

- **(a) Perfil da sombra.** Já existe um marcador explícito, `HASNER_SOMBRA=True`, lido por `_exigir_lavra_de_prod_na_sombra`. A guarda do feriado é nova, mas pode usar esse marcador sem criar outro predicado de perfil. A árvore tem critérios distintos para outros fins. O replay procura `sombra` no módulo ou no nome do banco, o comando de férias compara o nome configurado, e o runner consulta `current_database()`. `BANCO_DE_ENSAIO` também é verdadeiro em CI. `app/config/settings/sombra.py:47-65`, `app/ponto/services/esmeril_espelho.py:291-298`, `app/core/management/commands/replay_credenciais.py:46-65`, `app/ferias/management/commands/listar_ferias_dp.py:46-53`, `bin/sombra.sh:78-80`, `app/config/settings/ci.py:35-41`.
- **(b) Rede e máscara.** A máscara zera os tokens FCM e desativa as assinaturas Web Push, substituindo endpoints e chaves por marcadores. Os settings esvaziam VAPID e GLPI, usam e-mail dummy e apontam a IA para a porta `9`. O runner usa rede Docker, proxy morto para HTTP(S) e exceções para bancos e endereços locais. Isolamento geral de rede e máscara efetivamente aplicada à base da sonda [nao verificado]. `bin/sombra_mascara.py:64-84`, `app/config/settings/sombra.py:16-25,53-60`, `bin/sombra.sh:182-191,266-273`, `CLAUDE.md:206`.
- **(c) Dry-run atual.** `marcar_feriado_do_posto --posto PK --fecha|--opera`, sem `--apply`, imprime `plano_da_marca` e retorna. Sem posto, lista sugestões. Hoje só o `--apply` resolve o autor e chama `aplicar_marca(posto, novo, autor=…, motivo=…, hoje=…)`. Pelo aval, a chamada da prévia passaria pela `previa_da_marca`, usando a mesma porta dentro do bloco revertido. O encaixe ainda precisa responder de onde vem o motivo, pois a CLI seca aceita omiti-lo e a porta exige texto não vazio. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:27-66,77-113`, `app/colaboradores/services/feriado_do_posto.py:268-286`, `feriado_a5_respostas_2011_1035.txt:3,9`, `respostas_2011_1100.txt:9`.
- **(d) Citações.** Conferem. O repasse chama o emissor dentro de `atomic` quando `notificar` está ativo. O emissor chama Web Push e FCM de forma síncrona. `detectar_ausencias` chama `push_supervisao` no ramo não flexível. O signal de escala chama `invalidar_previsto`, que põe `previsto_em` em `NULL` e atualiza `atualizado_em` nas linhas do colaborador cujo `previsto_em` estava preenchido, sem filtro de competência ou exportação. `app/chamados/services/disputa_emissao.py:1041-1066,1303-1305,1809-1821`, `app/ponto/management/commands/detectar_ausencias.py:775-786`, `app/escala/signals.py:137-151`, `app/ponto/services/fechamento.py:1607-1613`.
- **(e) O-OUTBOX.** Existe a linha `O-OUTBOX`, marcada **NO AR**, com a linha antiga fechada. O item 3 manda registrar ali a classe de envio antes do commit. `app/docs/BACKLOG.md:807-808`, `respostas_2011_1100.txt:9`.

**PROCEDE.** “Sem rede” excede o que o runner garante, e o dry-run atual pode chegar sem o motivo exigido pela porta real. `bin/sombra.sh:182-191`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:34-37,105-113`, `app/colaboradores/services/feriado_do_posto.py:284-285`.

## P2. Colide com o aval vigente, leis, cortes ou selos?

- O 1100 altera as provas do item 1 do 1035 e preserva batidas cruas com regra e rótulo, tela na fila 2 depois do comando, ato de produção com `!`, pre-opus e pouso a partir de 21/11 com `sombra_c2b = 0` no dia. Chamar a porta real cabe no escritor único da L-007 e no selo da marca. `respostas_2011_1100.txt:9-11`, `feriado_a5_respostas_2011_1035.txt:3-8`, `app/docs/LEIS.md:65,76,113`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:119-131`.
- **Comando e suíte.** O selo enumera `*/management/commands/*.py`, e o comando já tem casa em `FORA_DE_PIPELINE`. A recusa nova colide com três casos atuais na régua CI que chamam o dry-run e esperam o plano sem simular a sombra. `app/chamados/tests/test_contract_crons.py:211-237`, `app/config/crons.py:1350-1355`, `bin/suite.sh:324-325`, `app/colaboradores/tests/test_feriado_por_posto.py:386-400`, `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:73-99`.
- **Relógio, settings e rollback.** O teste do comando já congela o relógio. A CLI passa `hoje`, mas a porta real alcança leituras internas já existentes no cartório e no receiver de turnos. Há precedentes de leitura de settings na casca dos comandos. Selo geral de settings lidos fora da borda [nao verificado]. `CLAUDE.md:372-375`, `app/core/tests/test_selo_teste_sem_relogio.py:1-12,66-84`, `app/colaboradores/tests/test_feriado_por_posto.py:386-392`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:87,113`, `app/ponto/portas/celula.py:460-475`, `app/ponto/signals.py:53-74`, `app/ferias/management/commands/listar_ferias_dp.py:46-53`. A sonda da função real é compatível com o `atomic` desfeito previsto em `CLAUDE.md`. Um savepoint manual, sozinho, deixa callbacks na fila de `on_commit`, conforme precedente da árvore. `CLAUDE.md:193-206`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:332-345`.

**PROCEDE.** A nova recusa muda três expectativas vigentes da suíte. A proteção de 10/2026 no eventual apply segue sendo uma objeção distinta, em P4. `app/colaboradores/tests/test_feriado_por_posto.py:386-400`, `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:73-99`, `feriado_a5_respostas_2011_1035.txt:5-8`, `app/docs/CORTES.md:529`.

## P3. O que não fecha no literal?

- **RED (a).** A suíte canônica carrega `config.settings.ci`. O SaaS é o perfil padrão do `manage.py`. O mesmo teste pode exercer a guarda real com `HASNER_SOMBRA` ausente e com a flag verdadeira por `override_settings`, como já faz o esmeril. O negativo prova a recusa **fora da sombra** em CI. O perfil SaaS ativo e o `rc` do processo CLI são verificações distintas. Hoje o ramo seco imprime e retorna. Zero chamada à porta e zero tentativa de DML são observações distintas de reler o banco intacto, pois a captura de SQL também vê escritas revertidas. `bin/suite.sh:324-325`, `app/manage.py:7-18`, `app/ponto/tests/test_ensaio_de_dinheiro_exige_lavra_de_prod.py:44-76`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:105-113`, `app/chamados/tests/test_e4_backfill.py:226-240`.
- **Push na sombra mascarada.** `push_colaborador` retorna sem usuário ou sem assinatura ativa. `push_supervisao` consulta destinatários e usa o mesmo emissor Web, que retorna sem assinatura ativa. `push_fcm_colaborador` retorna sem token antes de abrir credencial ou chamar a rede. Com destinos presentes e apenas a rede indisponível, Web Push é síncrono e a chamada não passa timeout explícito. O emissor da disputa captura e loga a exceção, e pode deixar de chegar ao FCM se o Web Push lançar. A supervisão registra a falha em `stderr` e segue. O FCM captura falhas de inicialização ou envio e devolve erro com log. Timeout, entrega ou falha efetiva na sonda [nao verificado]. `bin/sombra_mascara.py:64-84`, `app/chamados/push_utils.py:12-26,38-55,89-118`, `app/chamados/fcm_utils.py:11-17,55-106`, `app/chamados/services/disputa_emissao.py:1799-1821`, `app/ponto/management/commands/detectar_ausencias.py:775-786`.
- **Prova (b).** Além dos modelos nomeados, a retratação aceita grava `FollowupChamado`, o receiver de escala recompõe `TurnoMaterializado` com `DELETE` e `bulk_create`, e o mesmo `UPDATE` de `FechamentoMensal` altera `atualizado_em`. Conforme o ramo, também há escrita em `DisputaSupervisao`, `DecisaoPergunta`, `Justificativa`, `Pauta`, `ContadorPauta` e `auth_user` se faltar o usuário `sistema`. Igualdade de chaves e valores nas tabelas listadas deixa essas escritas fora da prova. `app/colaboradores/services/feriado_do_posto.py:296-308`, `app/chamados/services/nota.py:102-109`, `app/ponto/signals.py:53-74`, `app/ponto/nucleo.py:40-83`, `app/ponto/services/fechamento.py:1607-1613`, `app/colaboradores/signals.py:49-82`, `app/chamados/models.py:2217-2235`, `app/core/utils.py:6-18`, `app/ponto/services/justificativa.py:321-355`, `app/chamados/services/fecho_runtime.py:118-141,156-169`, `app/pautas/services.py:180-188,344-368`.

**PROCEDE.** O RED literal com SaaS ativo e `rc` real não é o teste sob CI. A comparação de banco também deixa de fora pelo menos `FollowupChamado`, `TurnoMaterializado` e `FechamentoMensal.atualizado_em`. `bin/suite.sh:324-325`, `app/manage.py:7-18`, `app/chamados/services/nota.py:102-109`, `app/ponto/nucleo.py:57-66`, `app/ponto/services/fechamento.py:1607-1613`.

## P4. Toca exportada, dinheiro, escala/vínculo em produção ou `!` fora do declarado?

- O 1100 autoriza a prévia na sombra. O 1035 reserva o apply em produção ao Code, posto a posto, após mostrar o dry-run e com `!` do Ronald. O comando atual verifica `--apply` e motivo. A exigência de `!` é operacional. No apply, a porta grava a marca do posto e a chave do vínculo, regenera células e pode agendar recálculo monetário das competências tocadas no `on_commit`, com escrita em `FechamentoMensal` e `DiaPago`. Impacto efetivo em dinheiro [nao verificado]. `respostas_2011_1100.txt:9-11`, `feriado_a5_respostas_2011_1035.txt:5-8`, `CLAUDE.md:265-268`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:105-113`, `app/colaboradores/services/feriado_do_posto.py:288-308`, `app/colaboradores/services/vinculo.py:894-922`, `app/ponto/portas/celula.py:483-496`, `app/ponto/services/fechamento.py:1137-1162,1191-1202,1798-1824`.
- A guarda de TXT vigente ou holerite publicado protege a **regeneração**. Fora desse recorte, o signal invalida fechamentos materializados do colaborador e altera também `atualizado_em`. A retratação direta em **FECHA** seleciona pelo dia do fato e seu signal pode rejulgar a célula. Para corte 21, a virada de 21/11 tira outubro da janela de regeneração. Os outros dois caminhos seguem com seus próprios recortes. O 1035 proíbe mover 10/2026, e `CORTES.md` registra essa competência como paga fora do sistema nas empresas 2, 3 e 4. Ocorrência efetiva nesses postos em produção [nao verificado]. `app/folha/export.py:597-628`, `app/ponto/portas/celula.py:218-221,284-288`, `app/ponto/services/fechamento.py:1607-1613`, `app/colaboradores/services/feriado_do_posto.py:248-264,296-305`, `app/chamados/signals.py:189-218`, `app/ponto/janelas.py:24-51,66-72`, `feriado_a5_respostas_2011_1035.txt:6-7`, `app/docs/CORTES.md:529`, `CLAUDE.md:411-417`.

**PROCEDE quanto ao futuro apply.** Registrar o efeito em `previsto_em` documenta o achado. A proteção de 10/2026 continua sem prova para a invalidação de `FechamentoMensal` e para o rejulgamento disparado pela retratação direta. `respostas_2011_1100.txt:10`, `feriado_a5_respostas_2011_1035.txt:7`, `app/ponto/services/fechamento.py:1607-1613`, `app/chamados/signals.py:189-218`.

## 20/11 10:55 · sessao c82266270857 · - · - · origem=Ronald (tmux)

**P2 — PROCEDE.** A recusa fora da sombra colide com três casos da suíte atual que chamam o dry-run em CI e esperam a saída do plano. O 1035 mantém a suíte verde como condição de pouso. Usar o comando existente respeita o selo B6.1, pois `marcar_feriado_do_posto` já consta em `FORA_DE_PIPELINE` como ato humano. Reusar `aplicar_marca` preserva o escritor único. `feriado_a5_respostas_2011_1035.txt:5-8`, `bin/suite.sh:324-325`, `app/colaboradores/tests/test_feriado_por_posto.py:386-400`, `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:73-99`, `app/chamados/tests/test_contract_crons.py:211-237`, `app/config/crons.py:1350-1355`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:119-131`.

O selo de relógio cobra congelamento, já presente nos dois módulos de teste. A CLI lê `hoje` na borda, mas o plano aciona `qs_na_fila()` sem `agora` e o caminho real também lê data na regeneração e no signal de escala. Selo geral de leitura de settings fora da borda [nao verificado]. O contrato de settings localizado cobre cópia literal de listas nos perfis SaaS e prod. A prévia por `atomic` desfeito encaixa na prática da sonda real. O savepoint manual, por si, deixa problema de callbacks descrito na árvore. `app/core/tests/test_selo_teste_sem_relogio.py:24-44,66-84`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:87-89,112-113`, `app/colaboradores/services/feriado_do_posto.py:248-253`, `app/chamados/services/painel.py:186-190`, `app/ponto/portas/celula.py:460-475`, `app/ponto/signals.py:53-74`, `app/core/tests/test_contract_settings_herdam.py:40-44,64-77`, `CLAUDE.md:193-206`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:333-345`.

**P3 — PROCEDE.** A suíte usa `config.settings.ci`. Ela já testa a mesma flag `HASNER_SOMBRA` com `override_settings` e com a flag ausente, o que prova a bifurcação fora/dentro da sombra. A execução literal com o perfil `config.settings.saas` e o rc da nova guarda estão [nao verificado]. O comando hoje usa retorno normal após imprimir erro. Há precedente de teste de recusa por `SystemExit` e de captura de DML inclusive quando a escrita sofre rollback. A chamada real também exige motivo não vazio, enquanto a CLI e os testes atuais o reservam ao `--apply`. `bin/suite.sh:324-325`, `app/config/settings/ci.py:6-18,35-41`, `app/config/settings/sombra.py:53-65`, `app/ponto/services/esmeril_espelho.py:291-295`, `app/ponto/tests/test_ensaio_de_dinheiro_exige_lavra_de_prod.py:44-75`, `app/manage.py:7-18`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:27-37,63-65,77-92,104-113`, `app/colaboradores/services/feriado_do_posto.py:268-285`, `app/core/tests/test_replay_credenciais.py:86-92`, `app/chamados/tests/test_e4_backfill.py:226-240`.

A flag já existe, mas há critérios concorrentes na árvore. `listar_ferias_dp` usa nome exato do banco e `replay_credenciais` aceita substring no módulo ou no banco. `BANCO_DE_ENSAIO` também vale no CI e teste_motor. A execução de sombra liga à rede Docker e configura proxy HTTP/HTTPS morto com exceções. A máscara desativa e substitui endpoints e zera tokens, levando Web Push e FCM à saída sem destinatário/token. O registro O-OUTBOX existe e está marcado NO AR. `app/config/settings/sombra.py:27-65`, `app/config/settings/ci.py:35-41`, `app/config/settings/teste_motor.py:15-18`, `app/ferias/management/commands/listar_ferias_dp.py:46-53`, `app/core/management/commands/replay_credenciais.py:46-63`, `bin/sombra.sh:182-190`, `bin/sombra_mascara.py:63-84`, `app/chamados/push_utils.py:20-26,89-118`, `app/chamados/fcm_utils.py:101-106`, `app/docs/BACKLOG.md:807-808`.

## 20/11 10:54 · sessao ddf3b608f25e · - · - · origem=Ronald (tmux)

**P3(b) — PROCEDE.** `FollowupChamado` e `TurnoMaterializado` ficam fora das tabelas nomeadas. A retratação aceita grava o follow-up. O signal do vínculo recompõe os turnos com `DELETE` e `bulk_create`. `FechamentoMensal` grava também `atualizado_em` ao limpar `previsto_em`. `app/colaboradores/services/feriado_do_posto.py:296-308`, `app/chamados/services/nota.py:102-109`, `app/ponto/signals.py:53-74`, `app/ponto/nucleo.py:40-83`, `app/ponto/services/fechamento.py:1607-1613`.

- Conforme os ramos, a prova precisa alcançar também `DisputaSupervisao`, `DecisaoPergunta`, `auth_user` quando falta `sistema`, `Justificativa`, `Pauta` e `ContadorPauta`. O receiver de `FolgaDia` exige mudança de vigência, enquanto a propagação salva apenas `trabalha_em_feriado`. `app/colaboradores/signals.py:49-82`, `app/chamados/models.py:2217-2235`, `app/core/utils.py:6-18`, `app/ponto/services/cartorio.py:1038,1585`, `app/ponto/services/justificativa.py:337-355`, `app/chamados/services/fecho_runtime.py:118-141,156-169`, `app/pautas/services.py:180-188,344-368`, `app/escala/signals.py:18-28,185-204`, `app/colaboradores/services/vinculo.py:894-898`.

**P4 — PROCEDE quanto ao alcance de um futuro `--apply`.** A prévia foi limitada à sombra pelo item 3 do 1100. O item 3 do 1035 condiciona o apply ao `!` do Ronald e o item 4 preserva a proibição de mover 10/2026. O comando verifica `--apply` e motivo. O `!` é exigência operacional. `respostas_2011_1100.txt:7-9`, `feriado_a5_respostas_2011_1035.txt:5-7`, `CLAUDE.md:265-268`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:105-113`.

- A guarda pula dias com TXT vigente ou holerite publicado, na regeneração. A invalidação de `FechamentoMensal` filtra apenas pelo colaborador e por `previsto_em` preenchido. A retratação direta em FECHA segue o dia do fato e o signal pode rejulgar e carimbar célula. A cerca S143 atua nas escritas de pergunta que atravessam o ato cercado. Para corte 21, a virada em 21/11 retira outubro da janela de regeneração. Esses outros caminhos seguem seus próprios recortes. `app/ponto/portas/celula.py:218-221,284-288`, `app/folha/export.py:597-628`, `app/ponto/services/fechamento.py:1607-1613`, `app/colaboradores/services/feriado_do_posto.py:248-264,296-308`, `app/chamados/signals.py:189-218`, `app/ponto/services/cartorio.py:1920-1923,2059-2102`, `app/chamados/services/disputa_emissao.py:1911-1924`, `app/ponto/janelas.py:24-51,66-72`.
- No apply, o vínculo é gravado e a regeneração pode agendar recálculo monetário por competência tocada no `on_commit`, que grava `FechamentoMensal` e `DiaPago`. O corte registra 10/2026 das empresas 2, 3 e 4 como paga fora do sistema. O alcance efetivo em produção e os valores monetários [nao verificado]. `app/colaboradores/services/vinculo.py:894-922`, `app/ponto/portas/celula.py:483-496`, `app/ponto/services/fechamento.py:1191-1202,1798-1824`, `app/ponto/services/dia_pago.py:359-360`, `app/docs/CORTES.md:529`.

## 20/11 10:54 · sessao fe6eae466a53 · - · - · origem=Ronald (tmux)

- **P1(a)** Existe uma fonte positiva para o perfil Django, `HASNER_SOMBRA=True`, lida pelo esmeril. A árvore tem guardas distintas, como o replay que procura `sombra` no módulo ou no nome, o comando de férias que testa o nome e a guarda de infraestrutura que consulta `current_database()`. O feriado precisa de uma guarda nova no comando e pode usar o sinal de perfil já existente. `app/config/settings/sombra.py:29,47-65`, `app/ponto/services/esmeril_espelho.py:291-298`, `app/core/management/commands/replay_credenciais.py:46-65`, `app/ferias/management/commands/listar_ferias_dp.py:46-53`, `bin/sombra.sh:67-80`.
- **P1(b)** A máscara apaga tokens e substitui endpoints e chaves por marcadores, com assinaturas inativas. O runner tem rede Docker, proxy morto para HTTP(S) e exceções locais. Os settings neutralizam credenciais e backends específicos. Isolamento geral de rede em execução [nao verificado]. `bin/sombra_mascara.py:64-84`, `bin/sombra.sh:183-191,266-276`, `app/config/settings/sombra.py:16-25,47-60`, `CLAUDE.md:206`.
- **P1(d/e)** Os sites de push são síncronos. A disputa chama Web Push e FCM dentro de `atomic`, e o emissor chama supervisão se não for flexível. O signal invalida `previsto_em` não nulo e carimba `atualizado_em` em todas as competências materializadas do colaborador com previsto, sem filtro de data/TXT. Há `O-OUTBOX` no backlog, já marcado NO AR. `app/chamados/services/disputa_emissao.py:1041-1066,1303-1305,1809-1821`, `app/ponto/management/commands/detectar_ausencias.py:775-787`, `app/escala/signals.py:137-151`, `app/ponto/services/fechamento.py:1607-1613`, `app/docs/BACKLOG.md:807`.
- **P2** O selo da marca varre arquivos de produção, inclusive management, e exige que `aplicar_marca` seja a única chamada ao escritor inferior. O selo de relógio cobra congelamento nos testes que leem hora real. O selo que enumera `config/settings` cobre `BANCO_DE_ENSAIO`, que também é verdadeiro em CI. Selo geral de settings fora da borda ou específico da enumeração de management [nao verificado]. `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:27-33,119-131`, `app/core/tests/test_selo_teste_sem_relogio.py:1-12,66-84`, `app/ponto/tests/test_porta_batida.py:98-119`, `app/config/settings/ci.py:35-41`.
- **P3(a)** CI e o perfil SaaS padrão chegam ao ramo fora da sombra pela ausência da mesma flag. A suite pode exercitar a recusa no perfil CI e emular a sombra via override, como já faz o teste do esmeril. A entrada do perfil SaaS pode ser conferida separadamente, por precedente de importação do módulo pela suite. Execução do RED com SaaS ativo, rc real e zero escrita [nao verificado]. A casca atual imprime aviso e retorna no dry-run. `bin/suite.sh:325`, `app/manage.py:7-18`, `app/ponto/tests/test_ensaio_de_dinheiro_exige_lavra_de_prod.py:44-76`, `app/core/tests/test_contract_settings_herdam.py:79-90,105-110`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:105-113`.
- **P3(a)** Em sombra mascarada, Web Push retorna sem assinatura ativa, FCM retorna sem token antes da credencial ou rede, e supervisão consulta os destinatários e percorre o mesmo emissor Web. Com destinatários ativos e apenas rede indisponível, Web Push faz chamada síncrona sem timeout explícito, o chamador da disputa retém exceções e registra log, o da supervisão registra em stderr e segue, e FCM retém exceções e devolve falha. Duração ou entrega real [nao verificado]. `app/chamados/push_utils.py:12-26,38-55,89-118`, `app/chamados/fcm_utils.py:11-17,55-106`, `app/chamados/services/disputa_emissao.py:1809-1821`, `app/ponto/management/commands/detectar_ausencias.py:775-787`.
- **P3(b)** Se a prova reler apenas `FechamentoMensal.previsto_em`, o segundo campo escrito nesse mesmo `UPDATE`, `atualizado_em`, também escapa da comparação. `app/ponto/services/fechamento.py:1607-1613`.

## 20/11 10:45 · sessao d6f223219c45 · E4C-F5-CONSTROI · construtor · origem=Code

**E4C-F5-CONSTROI · E-ITEM1-DEVOLUCAO**

### Item 1

- `devolver_validacao_recusada` descarta só o prefetch de `decisoes` depois do refresh parcial. A coluna preenchida basta para entrar na guarda. Com a coluna nula, a guarda e a foto inicial compartilham a leitura fresca de `DecisaoPergunta.composta`. No caminho comum, a guarda acrescenta zero `SELECT`s e a foto posterior acrescenta um `SELECT` de decisões. [validacao.py:269](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:269) · [models.py:4329](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4329)
- Em **A**, a devolução grava `DEVOLUCAO` pela porta existente, mantém a coluna nula e, com a foto padrão, emite `validacao_devolvida` com o instante da validação no antes e `None` no depois. Retorna `True`. Em **B**, a coluna preenchida aciona a mesma escrita. Nasce uma `DEVOLUCAO`, como na guarda anterior, e a composta segue sem validação vigente. [validacao.py:283](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:283) · [validacao.py:309](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:309) · [models.py:4342](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4342)
- A catraca aceitou a guarda na execução em memória com **zero entradas novas de allowlist**. Na Volta D, inverti a expectativa de A para `True` e **uma** `DEVOLUCAO`. O caso comum cobre uma devolução, um evento e retorno `None` na repetição. [test_e4c_f5_selo_catraca.py:298](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:298) · [test_e4c_f5_voltaD_portas.py:390](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:390) · [teste novo:137](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:137) [nao verificado]

### Chamadores da foto e de `antes=`

| Chamador | Efeito |
|---|---|
| `devolver_validacao_recusada` | A foto inicial usa `antes=` quando fornecido. A foto padrão passa a ver o ato em A. A foto final relê a composta após gravar `DEVOLUCAO`. [validacao.py:283](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:283) · [validacao.py:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:297) |
| `validar_pergunta` | Tira a foto antes do carimbo e a passa em `antes=`. A guarda anterior já barra A, e a foto preserva o carimbo em B. O resultado da comparação nos estados concordantes permanece igual. A foto pré-carimbo com coluna nula faz um `SELECT` adicional de decisões nesse chamador. [validacao.py:698](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:698) · [validacao.py:732](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:732) · [validacao.py:798](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:798) |
| `test_devolver_le_a_disputa_do_banco_e_reabre` | Passa em `antes=` a foto tirada antes do carimbo e do fechamento em outra instância. A expectativa de reabrir a disputa permanece. [test_pvmuda_carimbo_em_voo.py:140](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_pvmuda_carimbo_em_voo.py:140) |
| `test_A11_devolucao_conserva_resposta_e_2x_um_ato` | O teste instrumentado passou a executar a foto real com uma composta dublada. Passou localmente com as mesmas asserções. [test_e4_expand.py:144](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4_expand.py:144) | [nao verificado]

### REDs e mutantes

| RED | Mutante | Primeira asserção que deve falhar no ORM |
|---|---|---|
| A | `item1_so_coluna` | Espera uma `DEVOLUCAO` e recebe zero. [Linha 83](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:83) |
| A, trilha | `item1_foto_so_coluna` | A linha é gravada, mas o retorno é `None` e o evento não sai. [Linha 86](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:86) |
| B | `item1_so_ato` | A coluna conserva o instante anterior. [Linha 108](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:108) |
| PREFETCH | `item1_prefetch_velho` | Espera uma `DEVOLUCAO` e recebe zero. [Linha 126](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:126) |

Comandos para o Code, usando o seletor em memória do repositório. [cortes_f2_f9_apoio.py:78](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/cortes_f2_f9_apoio.py:78)

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
item1=chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna
caso="$item1.DevolucaoAtoOuColunaTest"

python3 manage.py test "$item1" chamados.tests.test_e4c_f5_selo_catraca \
  chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest.test_devolucao_e_o_escritor_da_coluna

CORTES_F2_MUTANTE=item1_so_coluna python3 manage.py test "$caso.test_RED_A_devolve_ato_e_deixa_trilha"
CORTES_F2_MUTANTE=item1_foto_so_coluna python3 manage.py test "$caso.test_RED_A_devolve_ato_e_deixa_trilha"
CORTES_F2_MUTANTE=item1_so_ato python3 manage.py test "$caso.test_RED_B_limpa_coluna_mesmo_sem_validacao_vigente"
CORTES_F2_MUTANTE=item1_prefetch_velho python3 manage.py test "$caso.test_RED_PREFETCH_enxerga_validacao_posterior_a_carga"

python3 manage.py test chamados.tests.test_e4c_f5_item4_medida_relogio_comum
```

### Item 4 e limites

**Espero vermelho na linha 72.** A medição interpõe a porta humana em T2, relê a irmã na entrada de `carimbar` após a trava existente e mantém T1. Escolhi uma ata `concorde`, com marco aceso às 19:00 e resposta às 19:02. O C6 dá a pergunta por encerrada no ramo de concordância com o marco, e `carimbar` persiste a coluna em T1. A `RESPOSTA(False)` de T2 vence a validação sintética na composta. A simulação em memória reproduziu B. A execução ORM está **[nao verificado]**. [medida:39](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item4_medida_relogio_comum.py:39) · [asserção:72](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item4_medida_relogio_comum.py:72) · [juizes.py:1428](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/juizes.py:1428) · [models.py:3354](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3354) · [models.py:4333](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4333)

A releitura do item 4 ficou só no teste, conforme o escopo de medição. O laço produtivo, a ordem de trava e o relógio comum ficaram intactos. A devolução usa o ato existente, sem ato de reparo. [medida:47](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_item4_medida_relogio_comum.py:47) · [materializacao.py:1033](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/materializacao.py:1033) · [validacao.py:289](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:289) [nao verificado]

Sem banco, conferi sintaxe, catraca e os quatro mutantes em memória. A execução do módulo instrumentado `test_e4_expand` também mostrou dois erros de dublê, A13 e A14, que reproduzi na base e deixei fora da volta. Suite ORM, remedição A/B e efeitos em produção **[nao verificado]**. [test_e4_expand.py:197](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4_expand.py:197) · [test_e4_expand.py:212](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4_expand.py:212) [nao verificado]

_7508 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
