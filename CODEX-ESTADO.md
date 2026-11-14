# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 10:40** · publicado em **14/11/2026 10:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 10:40 · sessao 85ace46cd031 · - · - · origem=Ronald (tmux)

7. `escala/servico_jornada.py::classificar_falta`
- Pergunta: que marco do dia faltou, conforme registro `app/core/juizes.py:192`.
- Textos: L-020, DNA congelado, `app/docs/LEIS.md:71`; L-073, teto por instante, `app/docs/LEIS.md:100`; L-088, furo como marco não cumprido e pergunta aberta, `app/docs/LEIS.md:120`.
- Código: cala para célula de folga, ausência inteira que suspende cobrança, noturno completo ou turno cujo fim não chegou (`app/escala/servico_jornada.py:310`, `app/escala/servico_jornada.py:352`, `app/escala/servico_jornada.py:364`, `app/escala/servico_jornada.py:388`). Lê batidas dos turnos e marcos congelados (`app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:439`). Só S pede entrada; só E ou turno aberto pede saída; depois compara contagens E/S previstas e realizadas (`app/escala/servico_jornada.py:465`, `app/escala/servico_jornada.py:478`, `app/escala/servico_jornada.py:481`). Sem marcos, termina pedindo saída salvo presença exclusiva de S (`app/escala/servico_jornada.py:490`).
- Conflito: nenhum par contraditório inequívoco demonstrado entre os textos aplicáveis acima e essas guardas; a abrangência de L-061 sobre este próprio juiz fica [nao verificado].
- Coluna 6: achado `app/ponto/propositor.py::primeiro_furo_posicional`, que declara segunda opinião e devolve o tipo do primeiro marco em furo por atribuição posicional (`app/ponto/propositor.py:261`, `app/ponto/propositor.py:268`, `app/ponto/propositor.py:315`, `app/ponto/propositor.py:322`). O juiz registrado consome essa opinião apenas como veto de cobrança, em `app/escala/servico_jornada.py:454` e `app/escala/servico_jornada.py:457`; não é mera delegação do propositor ao juiz. Procurados `classificar_falta`, `saida_sem_entrada`, `orfao_14h`, `furo_tipo`, `missing`.
- Coluna 7: `-`.

8. `ponto/turnos.py::realizado_do_dia`
- Pergunta: quantos minutos o dia realizou, `app/core/juizes.py:194`.
- Textos: L-102, dia sem turno pareado leva palavra sem número, `app/docs/LEIS.md:128`; L-103, fechamento escreve zero declarado quando realizado não possui número, `app/docs/LEIS.md:129`; L-115, saída posicional depois da pausa fechada, `app/docs/LEIS.md:151`. Há também o texto anterior da palavra com número da ata em `app/docs/DOSSIES.md:142`.
- Código: busca turnos e vínculo/intervalo do dia e delega ao corpo puro (`app/ponto/turnos.py:779`, `app/ponto/turnos.py:788`, `app/ponto/turnos.py:794`, `app/ponto/turnos.py:795`). Sem turno retorna minutos `None` e `sem_turno=True` (`app/ponto/turnos.py:719`, `app/ponto/turnos.py:721`); ramo posicional desconta pausa batida (`app/ponto/turnos.py:725`, `app/ponto/turnos.py:732`); demais turnos somam entrada até última borda fechada menos pausas batidas, ou janela declarada no modo fixo (`app/ponto/turnos.py:735`, `app/ponto/turnos.py:740`, `app/ponto/turnos.py:756`, `app/ponto/turnos.py:760`, `app/ponto/turnos.py:770`). O fechamento aplica expressamente zero aos sem turno em `app/escala/utils.py:2058`.
- Conflito: **sim, texto histórico superado**: número da ata rotulado em `app/docs/DOSSIES.md:142` versus palavra sem número em `app/docs/LEIS.md:128`. A própria L-102 declara que altera o rodapé original; portanto isso não sustenta uma pergunta de lei vigente (`app/docs/LEIS.md:128`).
- Coluna 6: achado `app/ponto/calculador/nucleo.py::minutos_do_oraculo`, derivação independente de minutos por dia, com corte próprio e soma dos pares (`app/ponto/calculador/nucleo.py:42`, `app/ponto/calculador/nucleo.py:53`, `app/ponto/calculador/nucleo.py:54`, `app/ponto/calculador/nucleo.py:147`, `app/ponto/calculador/nucleo.py:247`). O certificador usa essa função sem perguntar ao realizado registrado (`app/ponto/management/commands/e6_oraculo.py:41`, `app/ponto/management/commands/e6_oraculo.py:153`); sua independência é declarada em `app/config/crons.py:1055`. Procurados `realizado_do_dia`, `realizado_dos_turnos`, `minutos_realizados`, `total_seconds`.
- Coluna 7: `-`, alteração expressamente resolvida em `app/docs/LEIS.md:128`.

9. `ponto/status_dia.py::status_do_dia_lavrado`
- Pergunta: qual o status do dia, `app/core/juizes.py:199`.
- Textos: L-039, falta decidida, `app/docs/LEIS.md:87`; L-061, consumidor lê a lâmpada, `app/docs/LEIS.md:95`; corte de nascimento E3-STATUS-JUIZ, lavrado ou andamento antes do encerramento, `app/docs/CORTES.md:415`. Registro descritivo em `app/docs/JUIZES.md:112`.
- Código: avulso é lote de um, `app/ponto/status_dia.py:218`. Turno vivo ou batida antes de encerrar dá `aberto` (`app/ponto/status_dia.py:280`, `app/ponto/status_dia.py:283`); encerrado com status lavrado devolve a coluna (`app/ponto/status_dia.py:287`); encerrado com batida sem lavra dá `sem_certidao` (`app/ponto/status_dia.py:292`). Sem batida lê precedência: ausência descontável dá `falta`, demais ausências `ausencia`, feriado/folga dá `folga`, trabalho ainda não encerrado dá `previsto` e encerrado sem lavra dá `sem_certidao`, demais casos `sem_dados` (`app/ponto/status_dia.py:308`, `app/ponto/status_dia.py:311`, `app/ponto/status_dia.py:313`, `app/ponto/status_dia.py:316`, `app/ponto/status_dia.py:318`).
- Conflito: não demonstrado entre esse corpo e os textos citados; não use docstrings históricos dos leitores como se fossem regras atuais (`app/colaboradores/services/calendario.py:79`, `app/ponto/services/espelho.py:145`).
- Coluna 6: **nenhum achado** nas buscas `status_dia`, `_status_dia`, `status_do_dia`, atribuições de `status` e retornos com `irregular`, `folga_trabalhada`, `sem_certidao`. Leitores principais efetivamente delegam: calendário em `app/colaboradores/services/calendario.py:606`; app em `app/api/views.py:1480`; espelho em `app/ponto/services/espelho.py:167` e `app/ponto/services/espelho.py:174`. Exaustividade além dessas buscas [nao verificado].
- Coluna 7: `-`.

10. `ponto/precedencia.py::folga_lancada_do_dia`
- Pergunta: este dia tem folga lançada válida para o colaborador, `app/core/juizes.py:204`.
- Textos: corte GRADE-FOLGA-JUIZ, calendário pertence ao vínculo e avulsa pertence à pessoa, `app/docs/CORTES.md:417`; mesma regra em `app/docs/JUIZES.md:114` e `app/docs/JUIZES.md:115`. Nenhuma L-NNN específica encontrada nessas buscas [nao verificado].
- Código: carrega todas as folgas da pessoa por data e pk (`app/ponto/precedencia.py:206`); retorna primeira candidata aceita (`app/ponto/precedencia.py:216`); `None` não vale, folga sem calendário vale sempre, folga de calendário exige vínculo igual ao do dia (`app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`).
- Conflito: não entre corte e código do juiz, ambos afirmam essa mesma distinção (`app/docs/CORTES.md:417`, `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`).
- Coluna 6: achado `app/chamados/management/commands/auditar_invariantes_chamados.py:1536`: C5 trata qualquer FolgaDia como válida se vínculo original cobre a data **ou outro vínculo cobre**, sem testar origem de calendário (`app/chamados/management/commands/auditar_invariantes_chamados.py:1538`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1539`); acusa célula de trabalho a partir desse universo (`app/chamados/management/commands/auditar_invariantes_chamados.py:1546`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1552`). A própria catraca documenta que esse C5 conta calendário de vínculo velho como contradição (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:20`). Procurados `FolgaDia`, `folgas_dia`, `calendario_origem`, `folga_lancada_vale_no_dia`.
- Coluna 7: `-`.

## 14/11 10:40 · sessao 120a536611ff · - · - · origem=Ronald (tmux)

Juiz 9: manteria conflito “nao” e coluna 7 “-”. O corte exige recusar cadastro com início igual ao fim (app/docs/CORTES.md:281); o juiz responde se a geometria cruza a meia-noite, incluindo igualdade (app/escala/servico_jornada.py:111). São perguntas distintas.

Juiz 8: reviso minha classificação anterior e incluiria coluna 7. Pela redação literal do pedido, existem duas candidatas escritas: 5 minutos no docstring (app/ponto/motor_calculo_v2.py:1952) e 10 minutos deliberados na decisão registrada (app/ponto/motor_calculo_v2.py:53). A resposta vigente continua sendo 10 minutos (app/ponto/motor_calculo_v2.py:60, app/ponto/motor_calculo_v2.py:2006). A lápide reconhece expressamente que o docstring está desatualizado (app/ponto/calculador/periodo.py:153).

## 14/11 10:39 · sessao 48f3c99bbc05 · - · - · origem=Ronald (tmux)

app/ferias/services.py::em_gozo_hoje ; o colaborador esta de ferias hoje? (app/core/juizes.py:262) ; nenhum texto específico em LEIS.md/CORTES.md encontrado para DATAS versus STATUS, busca em ambos por ferias, em_gozo, S4-FERIAS-HOJE [nao verificado quanto a ausência absoluta] ; datas inclusivas cobrindo hoje, excluindo cancelado, inclusive concluido com datas vivas, filtros opcionais colaborador/empresa (app/ferias/services.py:53, app/ferias/services.py:65, app/ferias/services.py:66) ; sim, divergencia documental: API declara em_gozo pelo STATUS (app/api/views_mensageria.py:858), juiz consulta DATAS e exclui apenas cancelado (app/ferias/services.py:53, app/ferias/services.py:65) ; nenhum achado de leitor independente para HOJE, buscas em app por em_gozo, status, data_inicio__lte, data_fim__gte, data_inicio <= hoje, em_gozo_hoje, excluídos testes/migrations, status_por_data responde estado do agendamento e não foi contabilizado (app/ferias/models.py:276) ; pela DATA ou pelo STATUS? candidatas escritas app/ferias/services.py:17 e app/api/views_mensageria.py:858, vigência da segunda como lei [nao verificado]

app/ponto/turnos.py::afastado_hoje ; o colaborador esta afastado hoje? (app/core/juizes.py:263) ; nenhum texto específico em LEIS.md/CORTES.md encontrado, DOSSIES aponta a autoridade e a escrita do campo (app/docs/DOSSIES.md:75) ; retorna Ausencia afastamento_inss cobrindo dia por data única, intervalo fechado ou aberto permitido, folha somente aprovada e cobrança não rejeitada, desempate maior data e pk (app/ponto/turnos.py:2704, app/ponto/turnos.py:2715, app/ponto/turnos.py:2718, app/ponto/turnos.py:2731, app/ponto/turnos.py:2516) ; nao ; escritor externo marca situacao afastado só pelo tipo lançado, sem consultar a cobertura do dia (app/ponto/views.py:2436, app/ponto/views.py:2438), leitura do campo tem conformidade condicionada ao contador conforme registro (app/core/juizes.py:461) ; -

app/colaboradores/services/desligamento.py::esta_desligado ; o colaborador esta desligado? (app/core/juizes.py:267) ; JUIZ-ESTA-DESLIGADO-NASCE, unidade situacao desligado (app/docs/CORTES.md:450), sem L-NNN específica encontrada ; verdadeiro somente se situacao gravada igual desligado, agendado ainda não grava e responde falso (app/colaboradores/services/desligamento.py:29, app/colaboradores/services/desligamento.py:32), execução agendada retorna antes da escrita desligado (app/colaboradores/services/desligamento.py:107, app/colaboradores/services/desligamento.py:126, app/colaboradores/services/desligamento.py:129) ; sim, corte diz agendado no aviso previo igual já desligado (app/docs/CORTES.md:450), docstring do juiz diz agendado não grava e responde False (app/colaboradores/services/desligamento.py:29) ; julgar_vinculo testa situacao desligado diretamente e recusa novo vínculo (app/colaboradores/services/vinculo.py:91, app/colaboradores/services/vinculo.py:96), também cartório cala emissão por situacao cru (app/ponto/services/cartorio.py:1748) ; agendado no aviso prévio já está desligado, como app/docs/CORTES.md:450, ou segue ativo até a data, como app/colaboradores/services/desligamento.py:29?

app/ponto/catalogo/ausencias.py::fim_de ; quanto a ausencia dura, e o tipo e valido? (app/core/juizes.py:273) ; nenhum texto específico em LEIS.md/CORTES.md encontrado para gramática do fim, corte 12/09 descrito no próprio catálogo (app/ponto/catalogo/ausencias.py:60) ; lê gramática do cadastro vigente e retorna por_lei, indeterminado ou dia_unico, ausente/desconhecido vira por_lei, não valida tipo nesta função (app/ponto/catalogo/ausencias.py:192, app/ponto/catalogo/ausencias.py:194, app/ponto/catalogo/ausencias.py:195), validade tem helper codigos_validos (app/ponto/catalogo/ausencias.py:284), cadastro real [nao verificado] ; nao ; AusenciaForm usa CATALOGO próprio com limites/dias fixos por tipo, incluindo maternidade 120 dias e feriado_folga até 30 (app/ponto/forms.py:13, app/ponto/forms.py:18, app/ponto/forms.py:28), força duração fixa e impõe mínimo/máximo (app/ponto/forms.py:91, app/ponto/forms.py:113), sem consultar fim_de para esses limites ; -

app/ponto/services/alertas_ausencia.py::resumo_atestados ; ha 3 ou mais lancamentos medicos na competencia? (app/core/juizes.py:274) ; L-090 janela pela competência da empresa (app/docs/LEIS.md:123), PARAMETRO-GANHA-ROTULO registra consumo TipoAusencia.medico (app/docs/CORTES.md:221), nenhum texto específico para limiar três encontrado ; devolve competência e totais, cobertura para folha, tipos médicos do catálogo, registros distintos por pk que venceram cobertura e dias cobertos, não devolve booleano nem aplica limiar três (app/ponto/services/alertas_ausencia.py:45, app/ponto/services/alertas_ausencia.py:48, app/ponto/services/alertas_ausencia.py:50, app/ponto/services/alertas_ausencia.py:51, app/ponto/services/alertas_ausencia.py:52), valores reais do cadastro [nao verificado] ; nao ; nenhum achado de contagem/universo paralelo para três lançamentos, busca em app por resumo_atestados, TIPOS_MEDICOS, atestado/medico com count/Count/3/>=, excluídos testes/migrations, views aplica >=3 aos registros do próprio juiz (app/ponto/views.py:2227, app/ponto/views.py:2450) ; -

## 14/11 10:39 · sessao f6c292934bd4 · - · - · origem=Ronald (tmux)

app/chamados/catalogo/motor.py::VIVOS ; o chamado está vivo? app/core/juizes.py:107 ; REGISTRADO-NAO-E-VIVO-DESVIO mantém registrado em VIVOS, app/docs/CORTES.md:328 ; vivo significa status aberto, em_analise ou registrado, app/chamados/catalogo/motor.py:24 ; nao ; nenhum achado de definição independente, procurados status_local, status_local__in e sequência aberto/em_analise/registrado, leitores encontrados usam VIVOS, app/ponto/services/cartorio.py:1384 e app/chamados/services/historico.py:180 ; -

app/chamados/catalogo/motor.py::decidir ; pode transitar de um estado a outro? app/core/juizes.py:108 ; nenhum texto específico da matriz, endereço documentado em app/docs/JUIZES.md:37 ; inexistente aceita EMITIR ou REGISTRAR, estados desconhecidos recusam, transição humana usa matriz e perfil admin_ti com cancelado/superado terminais absolutos, eventos do sistema usam mapa, app/chamados/catalogo/motor.py:130 e app/chamados/catalogo/motor.py:142 e app/chamados/catalogo/motor.py:153 e app/chamados/catalogo/motor.py:158 ; nao ; nenhum achado de matriz independente, procurados TRANSICOES_PERMITIDAS, TRANSICAO_HUMANA, pode_transicionar e decidir, modelo deriva a tabela e delega decisão, app/chamados/models.py:117 e app/chamados/models.py:124 ; -

app/chamados/catalogo/premissa.py::premissa_morta ; a premissa do chamado morreu, podendo fechar? app/core/juizes.py:109 ; L-125 para cluster, app/docs/LEIS.md:146, O169-DECIDIDO-PELA-LEI exige lastro em vez de idade, app/docs/CORTES.md:430 ; verdadeira somente com classe reconhecida e fato sustentador falso, classe desconhecida ou fato não coletado mantém viva, app/chamados/catalogo/premissa.py:124 e app/chamados/catalogo/premissa.py:128 e app/chamados/catalogo/premissa.py:134 e app/chamados/catalogo/premissa.py:150 ; nao ; nenhum achado de decisão independente, procurados premissa_morta, _premissa_viva, _veredito_premissa e a_fechar, reconciliador consulta avaliar e cluster consulta premissa_morta, app/chamados/reconciliador.py:302 e app/ponto/services/cluster_espurio.py:76 ; -

app/chamados/utils.py::pode_agir_chamado ; quem pode agir no chamado? app/core/juizes.py:110 ; nenhum texto específico em LEIS/CORTES, endereço documentado em app/docs/JUIZES.md:49 ; departamento deriva do dono do módulo ou da área da solicitação, anônimo recusa, gestor geral aceita, departamento desconhecido ou usuário sem competências aceita, demais exigem competência ou perfil correspondente, app/chamados/utils.py:39 e app/chamados/utils.py:42 e app/chamados/utils.py:55 e app/chamados/utils.py:57 e app/chamados/utils.py:60 e app/chamados/utils.py:65 e app/chamados/utils.py:72 ; nao ; nenhum achado de decisão contextual independente, procurados pode_agir_chamado, departamento_do_chamado, tem_perfil e departamento, view e ações consultam juiz, app/chamados/views.py:1015 e app/chamados/services/acoes_disputa.py:187 ; -

app/chamados/catalogo/modulos.py::data_do_chamado ; qual o dia do chamado? app/core/juizes.py:111 ; K1-DIA-DO-CHAMADO declara chave_data=data_turno, app/docs/CORTES.md:177 ; devolve contexto na chave_data declarada no catálogo atual ou aposentado, sem módulo ou chave devolve None, app/chamados/catalogo/modulos.py:222 e app/chamados/catalogo/modulos.py:225 ; nao ; app/chamados/juizes.py::dias_do_fato responde por célula e depois data_turno cru sem consultar catálogo, app/chamados/juizes.py:184 e app/chamados/juizes.py:187, o singular também acrescenta fallback cru e outros degraus, app/chamados/juizes.py:118 e app/chamados/juizes.py:129 ; -

app/chamados/juizes.py::pergunta_viva ; a pergunta da disputa está viva? app/core/juizes.py:112 ; L-051 arquivamento reversível pela tranca, app/docs/LEIS.md:92, L-061 consumidor lê lâmpada, app/docs/LEIS.md:95, FIO-MORTO-E-ESTRUTURAL, app/docs/CORTES.md:253, JUIZ-PODE-FECHAR-NASCE, app/docs/CORTES.md:272, exceção de marco já respondido à regra de teto, app/docs/CORTES.md:413 ; delega ao cálculo com memo por instância, validada ou trancada morre, vias prioritárias materializadas morrem, resposta não vazia vive salvo decisão humana ou concordância resposta/luz, identidade materializada morre, marco materializado aceso morre, célula acusadora vive, célula absolvente definitiva ou marco aceso morre, falta atual vive, sem materialização vive, restante depende da testemunha de materialização com exceção final devolvendo falso, app/chamados/juizes.py:889 e app/chamados/juizes.py:1401 e app/chamados/juizes.py:1405 e app/chamados/juizes.py:1412 e app/chamados/juizes.py:1417 e app/chamados/juizes.py:1429 e app/chamados/juizes.py:1438 e app/chamados/juizes.py:1444 e app/chamados/juizes.py:1450 e app/chamados/juizes.py:1462 e app/chamados/juizes.py:1471 e app/chamados/juizes.py:1476 e app/chamados/juizes.py:1483 e app/chamados/juizes.py:1489 ; nao ; nenhum achado de resposta exata independente, procurados pergunta_viva, viva, q_vivas, validada_em, materializada_em e estado, property delega e SQL declara somente pré-filtro por contenção, app/chamados/models.py:3392 e app/chamados/models.py:3398 e app/chamados/models.py:3424 ; -

app/chamados/models.py::DisputaSupervisao.pode_fechar ; a disputa pode fechar? app/core/juizes.py:113 ; JUIZ-PODE-FECHAR-NASCE exige nenhuma pergunta viva e nenhuma resposta sem veredito, app/docs/CORTES.md:272 ; exige simultaneamente respostas_sem_veredito vazia e fio_vivo falso, app/chamados/models.py:1909 ; nao ; app/chamados/relatorios_disputa.py::classificar declara PRONTA_FECHAR apenas pela inexistência de perguntas vivas sem consultar respostas_sem_veredito, app/chamados/relatorios_disputa.py:18 e app/chamados/relatorios_disputa.py:20 ; -

app/chamados/models.py::ChamadoColaborador.abrir ; o chamado é duplicado, do mesmo fato? app/core/juizes.py:114 ; L-007 um escritor e trilha, app/docs/LEIS.md:65, regra de consulta aponta o dedup a abrir, app/docs/JUIZES.md:62, nenhum corte específico da chave encontrado [nao verificado] ; sob trava do colaborador busca colaborador mais lookup, reutiliza existente e renasce encerrado, aceita filho agrupado sob pai vivo, senão decide nascimento pelo motor, app/chamados/models.py:949 e app/chamados/models.py:953 e app/chamados/models.py:968 e app/chamados/models.py:976 e app/chamados/models.py:982 ; nao ; app/chamados/services/abertura.py faz dedup próprio por colaborador, módulo, VIVOS, assunto, descrição, prazo e autor e retorna igual ou chama criar, app/chamados/services/abertura.py:165 e app/chamados/services/abertura.py:170 e app/chamados/services/abertura.py:172 ; -

Contagem parcial deste lote: 8 juízes lidos, conflitos 0, coluna 6 com achado 3, coluna 7 preenchida 0, conforme as oito linhas acima. Leitura integral dos cinco documentos não concluída neste lote [nao verificado].

## 14/11 10:39 · sessao f500e35b8763 · - · - · origem=Ronald (tmux)

app/escala/models.py::EscalaColaborador.eh_dia_trabalho ; o dia e de trabalho para o vinculo? (app/core/juizes.py:184) ; L-020 (app/docs/LEIS.md:71), L-022 (app/docs/LEIS.md:73), L-023 (app/docs/LEIS.md:74), celula com fallback declarado em CLAUDE.md:155 ; devolve cel.trabalha quando ha celula, senao chama eh_dia_trabalho_calculado (app/escala/models.py:1094-1096), cujo ramo sem foto do mes e com ancora usa paridade 12x36 (app/escala/models.py:1289-1292), foto parcial usa paridade da folga proxima (app/escala/models.py:1364) ; sim: sem foto alguma deve responder None (app/docs/LEIS.md:73) x sem foto do mes com ancora responde paridade, inclusive True (app/escala/models.py:1289-1292) ; app/ponto/management/commands/gerar_batidas_smoke_motor.py::_trabalha_no_dia responde por paridade da ancora, dias da semana e fallback True (app/ponto/management/commands/gerar_batidas_smoke_motor.py:138-146), chamado para gerar batidas em app/ponto/management/commands/gerar_batidas_smoke_motor.py:130, execucao em prod [nao verificado] ; sem celula nem foto alguma, responde None (app/docs/LEIS.md:73) ou usa ancora (app/escala/models.py:1289-1292)?

app/escala/utils.py::minutos_previstos_do_dia ; quantos minutos o dia preve? (app/core/juizes.py:185) ; L-024 (app/docs/LEIS.md:75), fonte unica da grade em CLAUDE.md:148, corte JUIZ-ESCALA-NASCE confirma essa fonte (app/docs/CORTES.md:224) ; zero sem vinculo, tipo ou dia de trabalho True (app/escala/utils.py:2256-2267), span entre hi e hf com virada (app/escala/utils.py:2270-2274), desconta pausas declaradas ou, se vazias, hii/hfi do template (app/escala/utils.py:2305-2320) ; sim: F1 fixa 480 descontando pausa do template em celula existente com DNA.marcos nulo (app/escala/utils.py:2298-2299) x R4 exige nao descontar a pausa que aquele dia nao tem, resultando 540 (app/escala/utils.py:2300-2302), caso F1 escrito tambem em app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:35 e :43 ; app/escala/utils.py::montar_grade_prevista_periodo_por_turno calcula por conta propria soma de segmentos E para S (app/escala/utils.py:1635-1639), publicada como minutos_previstos em app/escala/utils.py:1809, materializacao da grade declarada em app/docs/JUIZES.md:110 ; celula existente com DNA.marcos nulo desconta pausa do template, 480 (app/escala/utils.py:2298-2299), ou nao desconta, 540 (app/escala/utils.py:2300-2302)?

app/ponto/precedencia.py::fatos_do_dia ; qual fato vence o dia? (app/core/juizes.py:186-187) ; L-089 delimita efeito de ausencia ao dia previsto (app/docs/LEIS.md:121), CORTES manda emissores perguntarem fatos_do_dia (app/docs/CORTES.md:360), ordem completa escrita em app/docs/JUIZES.md:107 ; delega ao periodo de um dia (app/ponto/precedencia.py:256), inclui ausencias nao rejeitadas (app/ponto/precedencia.py:273-278), monta presentes (app/ponto/precedencia.py:379-387) e vence pela ordem ferias, afastamento, atestado, feriado, folga, trabalho (app/ponto/precedencia.py:46 e :396-397), feriado do plantao fica so no pagamento se ec.feriado_vira_folga for falso (app/ponto/precedencia.py:376-377) ; sim: folga antes de feriado (app/docs/JUIZES.md:107) x feriado antes de folga (app/ponto/precedencia.py:46) ; app/escala/utils.py::_marcar_ausencias escolhe ausencia contra folga por predicado proprio (app/escala/utils.py:1210), sobrescreve tipo_dia como falta ou ausencia (app/escala/utils.py:1214 e :1220), cobertura delegada a outro juiz em app/escala/utils.py:1188 ; quando feriado e folga coexistem, vence folga (app/docs/JUIZES.md:107) ou feriado (app/ponto/precedencia.py:46)?

Observacao para consolidacao: a divergencia da ordem tambem existe entre dois textos, app/docs/JUIZES.md:107 e a tabela literal em app/ponto/precedencia.py:11. O codigo executavel confirma esta ultima em app/ponto/precedencia.py:46 e :396. A propria docstring conserva outra divergencia: excecao do feriado descrita apenas pela flag em app/ponto/precedencia.py:14, enquanto o corpo usa o predicado por ciclo em app/ponto/precedencia.py:376-377 e app/escala/models.py:1390-1395.

## 14/11 10:39 · sessao 1399bbae5e4b · - · - · origem=Ronald (tmux)

4. app/core/feriados.py::eh_feriado ; o dia e feriado para o colaborador? (app/core/juizes.py:188) ; nenhum texto especifico de alcance geografico encontrado em LEIS.md/CORTES.md ; devolve bool de feriados_de (app/core/feriados.py:105), cujo conjunto usa alcanca (app/core/feriados.py:83): nacional sempre, estadual pela UF, municipal pela UF e nome normalizado, sem municipio somente nacional (app/core/feriados.py:60-66, app/core/feriados.py:102-104) ; nao ; app/colaboradores/services/feriado_do_posto.py:59 decide sozinho nacional OU nome de cidade, sem conferir UF e sem delegar a alcanca, apesar da declaracao de delegacao em app/colaboradores/services/feriado_do_posto.py:50-53 ; -

5. app/ponto/turnos.py::ausencia_cobre ; uma ausencia cobre o dia para a folha? (app/core/juizes.py:190) ; nenhum texto especifico da cobertura geometrica encontrado em LEIS.md/CORTES.md, L-089 rege o efeito somente em dia previsto (app/docs/LEIS.md:121) ; exige aprovada (app/ponto/turnos.py:2516-2517), cobre data inicial, intervalo inclusivo ou intervalo aberto apenas TIPOS_EM_ABERTO, vence maior data e maior pk (app/ponto/turnos.py:2554-2559) ; nao ; app/folha/export.py::ausencias_na_janela filtra por conta propria status aprovada e intersecao inclusiva/aberta (app/folha/export.py:87-91), usado para calcular minutos de falta (app/folha/export.py:140-142), a expansao posterior delega dias_da_ausencia (app/folha/export.py:69-70) ; -

6. app/ponto/turnos.py::ausencia_cobre ; uma ausencia suspende a cobranca do dia? (app/core/juizes.py:191) ; nenhum texto especifico da cobertura por estado encontrado em LEIS.md/CORTES.md ; exclui rejeitada (app/ponto/turnos.py:2518-2519), cobre data inicial, intervalo inclusivo ou intervalo aberto apenas TIPOS_EM_ABERTO, vence maior data e maior pk (app/ponto/turnos.py:2554-2559) ; nao ; app/ponto/precedencia.py::fatos_do_periodo carrega nao-rejeitadas e intersecao por filtros proprios (app/ponto/precedencia.py:273-279), decide quais cobrem cada dia por predicado proprio (app/ponto/precedencia.py:336-339) e inclui suas faixas no julgamento (app/ponto/precedencia.py:380-381) ; -

Observacao de consolidacao: os achados externos das linhas 5 e 6 duplicam o recorte/status da cobertura, sem necessariamente reproduzir a selecao singular inteira do juiz. A linha 5 nao prova violacao da L-089: cobertura geometrica nao demonstra pagamento ou consumo de saldo. A linha 4 tem contradicao entre o comentario do leitor e o corpo do proprio leitor, mas isso nao e texto contra codigo DO JUIZ, portanto nao cabe inflar (5).

## 14/11 10:39 · sessao 8b0e8f288b9c · - · - · origem=Ronald (tmux)

Cinco perguntas examinadas, sem achado confirmado de derivação independente externa da mesma pergunta completa. As consultas externas a ED tratam de versão vigente, histórico ou comparação, e os consumidores de K1/K2/K9 chamam os juízes. Referências específicas abaixo.

1. `folha/export.py::classificar_export`, aprovação/tranca:

- Código: três travas, `conta_vazia` para horas iguais a zero, `turno_aberto` para turnos acima de zero e `sem_ancora_12x36` para vínculo ativo desse ciclo, sem âncora e colaborador ativo. `app/folha/export.py:806`, `app/folha/export.py:831`, `app/folha/export.py:848`.
- Textos: DOSSIES diz crivo igual a `entra` nas três portas. `app/docs/DOSSIES.md:120`. O corte posterior mantém turno aberto barrando o carimbo, dispensando apenas conta vazia. `app/docs/CORTES.md:250`.
- Conflito documental utilizável: `app/docs/DOSSIES.md:120` contra `app/docs/CORTES.md:250`. Também texto contra separação expressa no juiz em `app/folha/export.py:796`.
- Externos verificados: botão lê tupla em `app/ponto/views.py:1298`, aprovação individual em `app/ponto/views.py:1508`, TXT em `app/folha/views.py:244`. Nenhum recalcula os três campos. Os modos dispensam `conta_vazia` explicitamente em `app/ponto/views.py:33` e `app/folha/export.py:762`.
- Coluna 6: “nenhum achado — procurados travas_aprovacao, conta_vazia, sem_ancora_12x36, horas_trabalhadas==0, turnos_abertos>0, horas_trabalhadas=0, turnos_abertos__gt e data_ancora_colaborador__isnull”. A leitura de turno aberto no contador `app/ponto/views.py:1211` lista essa ocorrência, sem julgar aprovação.

2. `folha/export.py::classificar_export`, entrada no TXT:

- Código: nasce `entra`, depois exclui por rescisão até o fim da janela, sem código, férias com batida, cadastro sem previsto com horas zero e pelo menos duas batidas, furo do espelho e código não homologado. `app/folha/export.py:841`, `app/folha/export.py:866`, `app/folha/export.py:872`, `app/folha/export.py:876`, `app/folha/export.py:902`, `app/folha/export.py:924`, `app/folha/export.py:946`, `app/folha/export.py:968`, `app/folha/export.py:974`.
- Status aprovado não é pré-condição. `app/folha/export.py:851`.
- Lei aplicável à retenção por pausa: L-033. `app/docs/LEIS.md:81`. Corte E1 confirma furo pós-encerramento retendo e ausência/isento não retendo. `app/docs/CORTES.md:412`.
- Não achei conflito específico da cadeia de entrada. A discrepância crivo/`entra` é própria de K1. `app/docs/DOSSIES.md:120`, `app/folha/export.py:796`.
- Consumidores: tradução do veredito em `app/ponto/services/fechamento_lista.py:32`, TXT chama o juiz em `app/folha/views.py:212` e passa o mesmo veredito ao montador em `app/folha/views.py:268`.
- Coluna 6: “nenhum achado — procurados classificar_export, status==entra, rescisao_modulo_proprio, cadastro_zero, furo_espelho, nao_homologado, sem_codigo_dominio, ferias_com_batida”.
- Coluna 7: `-`, salvo repetir a divergência K1, o que não recomendo.

3. `folha/export.py::prontidao`:

- Código: isento resulta cego; snapshot confiável usa realizado em minutos, com fallback em horas trabalhadas, mais abonos, dividido pelo previsto e limitado a 100%. `app/folha/export.py:1037`, `app/folha/export.py:1045`, `app/folha/export.py:1076`.
- Sem snapshot, grade de trabalho e ausência; previsto é soma prevista, ausência abona seu previsto e trabalho contribui com mínimo de realizado/previsto por dia. Sem denominador resulta cego. `app/folha/export.py:1095`, `app/folha/export.py:1098`, `app/folha/export.py:1100`, `app/folha/export.py:1104`, `app/folha/export.py:1108`, `app/folha/export.py:1111`.
- Não achei corte ou L-NNN que formule a porcentagem. L-024 só rege a fonte do previsto. `app/docs/LEIS.md:75`. A regra de percentual aparece na docstring e comentários do próprio código. `app/folha/export.py:1026`, `app/folha/export.py:1072`.
- Nenhum conflito identificado.
- Externo chama prontidão em `app/ponto/services/fechamento_lista.py:85`. Escritor materializa componentes, sem calcular percentual, em `app/ponto/services/fechamento.py:1048`, `app/ponto/services/fechamento.py:1061`, `app/ponto/services/fechamento.py:1083`.
- Coluna 6: “nenhum achado — procurados prontidao, pct/percentual/cumprimento, horas_trabalhadas dividido, minutos_realizados dividido por minutos_previstos e min(realizado,previsto)”. Outras porcentagens de furo, como `app/escala/services/furos_diarios.py:154`, respondem outra pergunta.
- Coluna 7: `-`.

4. `folha/export.py::competencia_entregue`:

- Código: ED vigente da tripla empresa/mês/ano, rejeitada quando invalidada, bool ou registro conforme argumento. `app/folha/export.py:588`, `app/folha/export.py:591`, `app/folha/export.py:594`.
- Corte anterior dizia “entregue/lavrada” igual TXT ou holerite. **Linha corrigida: `app/docs/CORTES.md:246`**.
- Corte posterior define ED vigente e declara expressamente substituir a metade holerite. `app/docs/CORTES.md:275`.
- Para coluna 5, há divergência textual entre `app/docs/CORTES.md:246` e `app/docs/CORTES.md:275`, com substituição explícita pela segunda; não apresentar como lei vigente sem solução.
- Coluna 7, leitura literal do pedido: “Entregue inclui TXT ou holerite (`app/docs/CORTES.md:246`) ou só ED vigente (`app/docs/CORTES.md:275`)? A segunda candidata substitui expressamente a primeira.”
- Coluna 6: “nenhum achado — procurados competencia_entregue, ExportacaoDominio.objects, exists, anotar_vigencia, so_vigentes, invalidada_em e holerite publicado”. [nao verificado]
- Não contar `app/folha/haiku_export.py:67`/`:70`, que lista versão vigente inclusive revogada; nem `app/folha/management/commands/regerar_txt_dominio.py:69`, que escolhe anterior a preservar; nem `app/folha/services/vigia_txt.py:215`, que classifica revogada como INCOMPARAVEL em `app/folha/services/vigia_txt.py:220`. Não respondem se a competência foi entregue.

5. `folha/export.py::competencia_fechada_para_celula`:

- Código: chama entregue, soma holerite do próprio colaborador com lote publicado e devolve bool ou fontes. Sem colaborador, só TXT conta. `app/folha/export.py:618`, `app/folha/export.py:620`, `app/folha/export.py:622`, `app/folha/export.py:628`.
- Corte define explicitamente essa composição. `app/docs/CORTES.md:276`. L-052 registra TXT ou holerite como fonte da lavra. `app/docs/LEIS.md:93`.
- Conflito textual: DOSSIES diz entregue para célula igual TXT exportado, contra corte que preserva holerite para célula. `app/docs/DOSSIES.md:122`, `app/docs/CORTES.md:276`.
- Coluna 7: “Para célula basta TXT exportado (`app/docs/DOSSIES.md:122`) ou também holerite publicado do colaborador (`app/docs/CORTES.md:276`)? O segundo corte distingue a pergunta da célula.”
- Coluna 6: “nenhum achado — procurados competencia_fechada_para_celula, _competencias_exportadas, fonte_da_lavra, competencia_mes/ano e lote__publicado_em”.
- `app/ponto/management/commands/gerar_celulas.py:170` e `:178` enumeram candidatas, mas obrigatoriamente perguntam ao juiz em `app/ponto/management/commands/gerar_celulas.py:182`; fontes chamam o juiz em `app/ponto/management/commands/gerar_celulas.py:212`.
- Holerite por empresa em `app/ponto/services/fechamento.py:1539` responde “paga”, outra pergunta, sem julgar célula individual.

## 14/11 10:39 · sessao 120a536611ff · - · - · origem=Ronald (tmux)

escala/models.py::EscalaColaborador.marcos_do_dia ; quais sao os marcos previstos do dia? (app/core/juizes.py:213) ; L-020 celula soberana (app/docs/LEIS.md:71), W12X36-HPD (app/docs/CORTES.md:201) ; consulta celula alimentada ou ORM, usa dna.marcos quando verdadeiro e converte hi/hf/hii/hfi, senao consulta TipoEscala.marcos_do_dia (app/escala/models.py:1117, app/escala/models.py:1124, app/escala/models.py:1131, app/escala/models.py:1133) ; nao ; app/ponto/services/veredito_celula.py::_marcos_do_dia busca e interpreta DNA por conta propria, exige mk.hi e recorre ao template (app/ponto/services/veredito_celula.py:30, app/ponto/services/veredito_celula.py:32, app/ponto/services/veredito_celula.py:38, app/ponto/services/veredito_celula.py:43) ; -

ponto/turnos.py::intervalos_do_turno ; o vao entre batidas foi intervalo? (app/core/juizes.py:214) ; L-032 intervalo cadastrado e envelope (app/docs/LEIS.md:80), ESPINHA-ANTES-DA-UI exige autoridade unica desta pergunta (app/docs/CORTES.md:233), SITIO-SEM-CHAMADOR-AINDA-RESPONDE removeu concorrente antigo (app/docs/CORTES.md:241) ; None devolve lista vazia, senao retorna pares marcados cuja duracao alcanca piso de 15 minutos (app/ponto/turnos.py:499, app/ponto/turnos.py:542, app/ponto/turnos.py:545) ; nao ; app/ponto/juiz_batida.py::periodos_do_dia declara pausa batida pelos dois marcos acesos e fim maior ou igual ao inicio, sem consultar intervalos_do_turno nem exigir seu piso (app/ponto/juiz_batida.py:280, app/ponto/juiz_batida.py:282, app/ponto/juiz_batida.py:285, app/ponto/juiz_batida.py:292) ; -

ponto/motor_calculo_v2.py::MotorBase.aplicar_tolerancia ; a entrada atrasou ou a saida adiantou alem da tolerancia? (app/core/juizes.py:215) ; L-084 duas pontas distantes mais de 3 horas afastam julgamento (app/docs/LEIS.md:116), L-093 limita soma diaria da pontualidade em outra etapa (app/docs/LEIS.md:111), nenhum corte especifico de 5 contra 10 encontrado na busca tolerancia/F0/conformidade [nao verificado quanto a corte fora dos termos pesquisados] ; calcula deltas, devolve zero nas duas pontas se ambas distam mais de 180 minutos, senao cobra o delta integral positivo acima de 10 minutos (app/ponto/motor_calculo_v2.py:1946, app/ponto/motor_calculo_v2.py:1973, app/ponto/motor_calculo_v2.py:1988, app/ponto/motor_calculo_v2.py:2005, app/ponto/motor_calculo_v2.py:2006, app/ponto/motor_calculo_v2.py:60) ; sim: docstring diz tolerancia de 5 minutos (app/ponto/motor_calculo_v2.py:1952), corpo compara com constante de 10 minutos (app/ponto/motor_calculo_v2.py:2006, app/ponto/motor_calculo_v2.py:60) ; app/chamados/rotulos.py::frase_da_resposta importa apenas constante e deriva desconta por comparacao propria de delta, distinguindo atraso e saida antecipada (app/chamados/rotulos.py:134, app/chamados/rotulos.py:154, app/chamados/rotulos.py:156, app/chamados/rotulos.py:163) ; -

escala/servico_jornada.py::turno_cruza_meia_noite ; o turno previsto cruza a meia-noite? (app/core/juizes.py:216) ; L-091 geometria vem do desenho inclusive cruzamento da meia-noite (app/docs/LEIS.md:124), nenhum texto de corte especifico achado buscando turno_cruza_meia_noite e meia-noite [nao verificado quanto a outros termos] ; somente com hi e hf presentes, hf menor ou igual a hi cruza, incluindo igualdade como 24 horas (app/escala/servico_jornada.py:107, app/escala/servico_jornada.py:111) ; nao ; TipoEscala.clean decide cruzamento pela comparacao propria hora_inicio maior que hora_fim, que exclui igualdade (app/escala/models.py:358, app/escala/models.py:359, app/escala/models.py:361) ; -

escala/servico_jornada.py::eh_turno_partido ; este turno tem dois blocos (e partido)? (app/core/juizes.py:220) ; L-091 desenho decide regime e rotulo nao manda no calculo (app/docs/LEIS.md:124), JUIZ-EH-TURNO-PARTIDO-ASSINADO (app/docs/CORTES.md:235) ; sem alguma ponta do intervalo devolve False, senao partido significa vao circular em minutos estritamente maior que teto padrao 120 (app/escala/servico_jornada.py:114, app/escala/servico_jornada.py:152, app/escala/servico_jornada.py:154, app/escala/servico_jornada.py:156) ; nao ; get_motor ainda classifica por tipo_base em MOTOR_POR_BASE fora do ramo especifico de rotulos turno_partido consultando juiz, incluindo comercial por rotulo mesmo com vao superior a 120 minutos (app/ponto/motor_calculo_v2.py:3659, app/ponto/motor_calculo_v2.py:3662, app/ponto/motor_calculo_v2.py:3692, app/docs/LEIS.md:179) ; -

Neste recorte: 5 juizes lidos, 1 conflito, 5 achados externos, 0 perguntas de lei. Ressalva para consolidacao: os cortes app/docs/CORTES.md:294 e app/docs/CORTES.md:299 tratam TOL_MORFOLOGIA, e o segundo retira expressamente a alteracao do primeiro; nao sao conflito vigente de tolerancia de pontualidade.

## 14/11 10:39 · sessao 87c007b4338c · - · - · origem=Ronald (tmux)

app/escala/utils.py::status_do_delta ; que status (ok, atraso, adianto, grave) tem a lampada do marco? (app/core/juizes.py:221) ; nenhum texto específico para os limiares encontrado em LEIS.md/CORTES.md, buscando status_do_delta, conformidade e gradacao ; abs(delta) <= 10 dá ok, positivo até 90 dá atraso e acima atraso_grave, negativo até 90 dá adianto e acima adianto_grave (app/escala/utils.py:306, app/escala/utils.py:307, app/escala/utils.py:310, app/escala/utils.py:311, app/ponto/motor_calculo_v2.py:60, app/ponto/motor_calculo_v2.py:65) ; nao ; app/escala/services/leitor_celula.py::_status_da_luz calcula por conta própria ok até 10 e grave acima de 60 (app/escala/services/leitor_celula.py:58, app/escala/services/leitor_celula.py:61, app/escala/services/leitor_celula.py:62) ; -

app/ponto/turnos.py::dia_encerrado ; o dia esta encerrado? (app/core/juizes.py:235) ; L-073 permite instante ou fato encerrado (app/docs/LEIS.md:100), JUIZ-DIA-ENCERRADO-NASCE corrigido para fato encerrado (app/docs/CORTES.md:55), CHAMADO-X-JUIZ-CORRIGIDO proíbe data < hoje (app/docs/CORTES.md:62), RELOGIO-INJETADO exige instante recebido do chamador (app/docs/CORTES.md:370) ; retorna True quando não há turno aberto vivo, existe fim previsto e agora >= fim, senão False (app/ponto/turnos.py:2077, app/ponto/turnos.py:2116, app/ponto/turnos.py:2117, app/ponto/turnos.py:2121, app/ponto/turnos.py:2124) ; sim, RELOGIO-INJETADO proíbe relógio interno (app/docs/CORTES.md:370), mas dia_encerrado_motivo chamado pelo juiz lê timezone.now() quando agora é None (app/ponto/turnos.py:2077, app/ponto/turnos.py:2097) ; chamados/juizes.py::_turno_tem_saida pergunta se o turno do dia fechou e decide bool(_turnos) and all(not t.aberto for t in _turnos), sem perguntar a dia_encerrado (app/chamados/juizes.py:379, app/chamados/juizes.py:398, app/chamados/juizes.py:400) ; -

app/ponto/turnos.py::par_completo ; o par esta completo? (app/core/juizes.py:240) ; L-073 exige instante ou fato encerrado (app/docs/LEIS.md:100), JUIZ-PAR-COMPLETO-NASCE exige autoridade lida pelo relâmpago (app/docs/CORTES.md:56), CHAMADO-X-JUIZ-CORRIGIDO mantém seu nascimento (app/docs/CORTES.md:62), RELOGIO-INJETADO exige instante do chamador (app/docs/CORTES.md:370) ; as duas batidas precisam compartilhar pelo menos um data_turno cujo dia esteja encerrado, via mapa de donos e dias_encerrados (app/ponto/turnos.py:2179, app/ponto/turnos.py:2182, app/ponto/turnos.py:2186, app/ponto/turnos.py:2187), sem janela fornecida carrega véspera e dia local da última batida (app/ponto/turnos.py:2199, app/ponto/turnos.py:2200, app/ponto/turnos.py:2201) ; sim, RELOGIO-INJETADO proíbe relógio interno (app/docs/CORTES.md:370), mas o caminho padrão par_completo -> par_completo_da_janela -> dias_encerrados lê timezone.now() quando agora é None (app/ponto/turnos.py:2201, app/ponto/turnos.py:2179, app/ponto/turnos.py:2143) ; nenhum achado de duplicador exato, buscando par_completo, par completo, def .*completo e predicados sobre turnos fechados nos arquivos Python fora de testes, as duas portas do relâmpago delegam ao juiz (app/ponto/management/commands/detectar_par_relampago.py:300, app/ponto/management/commands/detectar_par_relampago.py:353) ; -

app/ponto/turnos.py::turno_aberto_ha ; ha quanto tempo o turno esta aberto? (app/core/juizes.py:245) ; JUIZ-TURNO-ABERTO-HA-NASCE fixa fonte TurnoMaterializado e escada 8/24/72 h, com frase anterior turnos_do_colab expressamente retirada (app/docs/CORTES.md:285), RELOGIO-INJETADO exige instante do chamador (app/docs/CORTES.md:370) ; consulta TurnoMaterializado se não recebeu turno, retorna None se ausente, fechado ou sem entrada, caso contrário horas entre timezone.now() e entrada, ignorando agora recebido (app/ponto/turnos.py:3191, app/ponto/turnos.py:3193, app/ponto/turnos.py:3194, app/ponto/turnos.py:3196) ; sim, RELOGIO-INJETADO exige instante do chamador (app/docs/CORTES.md:370), mas o juiz usa timezone.now() (app/ponto/turnos.py:3196) ; core/services/painel_op.py calcula agora menos entrada do turno materializado e divide em horas/minutos (app/core/services/painel_op.py:175, app/core/services/painel_op.py:176), emitir_turno_aberto_retroativo também calcula horas da aberta por conta própria (app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92, app/ponto/management/commands/emitir_turno_aberto_retroativo.py:100) ; -

app/ponto/turnos.py::lembrete_fim_turno ; que lembrete de fim de turno esta na hora? (app/core/juizes.py:248) ; JUIZ-LEMBRETE-FIM-TURNO-NASCE fixa -15/-5/+5/+10/+20 min (app/docs/CORTES.md:286), RELOGIO-INJETADO exige instante do chamador (app/docs/CORTES.md:370) ; até o fim inclusive considera pre_15/pre_5, depois considera pos_5/pos_10/pos_20, retorna todos cuja distância ao instante esperado seja <= 240 segundos (app/ponto/turnos.py:3224, app/ponto/turnos.py:3225, app/ponto/turnos.py:3226, app/ponto/turnos.py:3232, app/ponto/turnos.py:3235) ; nao ; nenhum achado de cálculo independente, buscando lembrete_fim_turno, DELTAS_PRE, DELTAS_POS, pre_15, pre_5, pos_5, pos_10, pos_20 e 240 nos arquivos Python fora de testes, o emissor delega ao juiz (app/ponto/management/commands/processar_alertas_turno.py:117, app/ponto/management/commands/processar_alertas_turno.py:118) ; -

Contagem deste subconjunto: 5 juízes lidos (app/core/juizes.py:221, app/core/juizes.py:235, app/core/juizes.py:240, app/core/juizes.py:245, app/core/juizes.py:248), 3 conflitos, 3 achados externos se incluído _turno_tem_saida como fechamento do dia, 0 perguntas de lei. A inclusão de _turno_tem_saida exige observar que seu texto pergunta pelo turno do dia, e seu predicado dispensa o fim previsto exigido por dia_encerrado (app/chamados/juizes.py:379, app/chamados/juizes.py:400, app/ponto/turnos.py:2121, app/ponto/turnos.py:2124).

## 14/11 10:39 · sessao e38ef4f1362e · - · - · origem=Ronald (tmux)

app/ponto/catalogo/ausencias.py::efeito_vigente ; a ausencia paga, desconta ou suprime o dia? (app/ponto/catalogo/ausencias.py:26) ; L-006 cadastro (app/docs/LEIS.md:64), L-039 falta decidida cujo tipo desconta (app/docs/LEIS.md:87), CATALOGO-SAIDA-ANTECIPADA-DESCONTA (app/docs/CORTES.md:202) ; devolve o efeito do catalogo vigente ou None para tipo ausente (app/ponto/catalogo/ausencias.py:294-295), vigente le banco ativo com fallback na semente (app/ponto/catalogo/ausencias.py:249-251,278), efeito efetivo de producao [nao verificado] ; sim: saida_antecipada DESCONTA no corte (app/docs/CORTES.md:202) versus SUPRIME na semente que o juiz pode ler (app/ponto/catalogo/ausencias.py:91,278) ; achado: a grade identifica desconto/falta pelo codigo literal _slug == 'falta' (app/escala/utils.py:1213-1215), faltas_periodo seleciona somente tipo='falta' (app/ferias/models.py:148) ; saida_antecipada desconta minutos (app/docs/CORTES.md:202) ou suprime o dia (app/ponto/catalogo/ausencias.py:91)?

app/ponto/turnos.py::cobertura_ausencia_periodo ; que dias a ausencia ocupa? (app/core/juizes.py:253) ; nenhum texto especifico de intervalo nas leis/cortes localizados [nao verificado], autoridades declaradas em app/docs/JUIZES.md:55-56 ; expande data..data_fim inclusivo, sem fim estende ate fim da janela somente TIPOS_EM_ABERTO, senao dia unico, recorta na janela e vence maior data e pk (app/ponto/turnos.py:2753-2754,2771-2782), folha aceita aprovada e cobranca exclui rejeitada (app/ponto/turnos.py:2516-2519) ; nao ; achado: fatos_do_periodo monta e testa por conta propria o mesmo predicado de cobertura por dia (app/ponto/precedencia.py:273-279,336-339) ; -

app/ponto/precedencia.py::veto_de_lancamento ; o lancamento colide com outro fato e deve ser recusado? (app/core/juizes.py:254) ; L-089 falta somente em dia previsto (app/docs/LEIS.md:121), GRADE-FOLGA-JUIZ define qual folga lancada vale (app/docs/CORTES.md:417) ; recusa ausencia sobreposta nao absolvida (app/ponto/precedencia.py:461-469), abono sobre falta e absolvido (app/ponto/precedencia.py:127-131,145-147), recusa folga lancada salvo travessia de ferias (app/ponto/precedencia.py:496-518), recusa falta em dia explicitamente sem trabalho (app/ponto/precedencia.py:536-543) ; nao ; achado: submeter_ausencia recusa qualquer ausencia_sobrepoe antes da porta sem aplicar absolvicao de abono sobre falta (app/ponto/views.py:2527-2531) ; -

app/ponto/services/ausencia.py::estado_inicial ; a ausencia exige documento e em que prazo? (app/ponto/services/ausencia.py:22) ; L-006 cadastro (app/docs/LEIS.md:64), nenhum corte especifico de sete dias localizado [nao verificado] ; com documento ou ferias devolve aguardando_decisao e sem prazo, sem documento em TIPOS_EXIGEM_DOC devolve aguardando_documento e precisa_prazo=True, demais aguardando_decisao (app/ponto/services/ausencia.py:791-798), prazo definido em sete dias (app/ponto/services/ausencia.py:745), criacao grava hoje local mais sete dias (app/ponto/services/ausencia.py:339) ; nao ; achado: escapes_disponiveis deriva exige_doc diretamente da posicao 2 do cadastro sem consultar estado_inicial (app/chamados/services/declaracao_texto.py:178-185) ; -

app/ponto/services/ausencia.py::ABERTAS ; a ausencia esta aberta ou decidida? (app/ponto/services/ausencia.py:23) ; nenhum texto especifico de regra nas leis/cortes localizado [nao verificado], JUIZES declara ABERTAS e rejeita vocabulario pendente (app/docs/JUIZES.md:56-57) ; abertas sao aguardando_documento e aguardando_decisao (app/ponto/services/ausencia.py:766), aprovada e rejeitada estao fora desse conjunto (app/ponto/services/ausencia.py:768-776) ; nao ; achado: API decide ofertar anexo usando _a.status == 'pendente', estado que ABERTAS nao contempla (app/api/views.py:1056-1061) ; -

app/ponto/turnos.py::periodo_da_parcial_no_dia ; a batida do dia esta dentro do periodo que a ausencia parcial declara? (app/core/juizes.py:261) ; L-006 periodo como cadastro (app/docs/LEIS.md:64, app/docs/CORTES.md:319), nascimento do juiz (app/docs/CORTES.md:323), periodo noturno pertence ao inicio do turno (app/docs/CORTES.md:330), quarta resposta nao_sei (app/docs/CORTES.md:334), L-085 jornada pertence ao dia de inicio (app/docs/LEIS.md:117) ; falta de qualquer hora devolve sem_periodo, fim menor que inicio soma 1440, alguma batida no intervalo inclusivo devolve dentro, senao hora ilegivel devolve nao_sei e demais fora (app/ponto/turnos.py:2634-2643) ; nao ; nenhum achado de comparador independente apos grep por periodo_da_parcial_no_dia, hora_ini e hora_fim fora de testes e migrations, leitores encontrados chamam o juiz em app/escala/utils.py:1177 e app/chamados/services/materializacao.py:73-83, ausencia total [nao verificado] ; -

Contagem deste recorte: 6 juizes lidos, 1 conflito sim, 5 com achado externo, 1 com duas candidatas escritas. A coluna 7 do primeiro admite semente de codigo como candidata escrita, devendo ser omitida se o criterio exigir duas normas textuais em documentos.

## 14/11 10:39 · sessao 6becb597bcab · - · - · origem=Ronald (tmux)

Linhas consolidadas dos juízes 1–5, para integração. Q3 sem duplicador confirmado da decisão final de flip; `tipo_por_marcos` trata o tipo recebido antes da gravação (`app/api/views.py:579`, `app/api/views.py:601`).

ponto/turnos.py::turnos_do_colab ; a que turno (data_turno) pertence a batida? (app/core/juizes.py:208) ; L-085 (app/docs/LEIS.md:117), COL200-DIA-DO-TURNO: dia do marco de início previsto mais próximo da entrada (app/docs/CORTES.md:186) ; carrega batidas não retratadas e células, delega ao corpo compartilhado (app/ponto/turnos.py:1863, app/ponto/turnos.py:1915), pareia e filtra pela data_turno (app/ponto/turnos.py:1992), definida pelo dia local com atribuição à véspera condicionada ao turno noturno e ao envelope da saída (app/ponto/turnos.py:285, app/ponto/turnos.py:301, app/ponto/turnos.py:308) ; sim: dia do marco inicial previsto mais próximo (app/docs/CORTES.md:186) versus recusa da véspera quando a saída não cabe no envelope (app/ponto/turnos.py:302, app/ponto/turnos.py:307) ; regularizacao deriva data_turno pela data civil local da entrada e a grava na Justificativa, apesar de já ter encontrado o turno do juiz (app/ponto/services/regularizacao.py:61, app/ponto/services/regularizacao.py:73, app/ponto/services/regularizacao.py:94) ; dia do marco inicial previsto mais próximo (app/docs/CORTES.md:186) ou dia local da entrada quando a saída não cabe no envelope da véspera (app/ponto/turnos.py:302, app/ponto/turnos.py:307)?

ponto/turnos.py::proximo_tipo_de ; a proxima batida e entrada ou saida? (app/core/juizes.py:209) ; nenhum texto que fixe o predicado da sugestão [nao verificado]; RELOGIO-INJETADO determina instante recebido do chamador (app/docs/CORTES.md:370) ; intermitente alterna pela última batida apurável nas 14 h anteriores (app/ponto/turnos.py:2466, app/ponto/turnos.py:2473); demais: sem turno vivo E, fora do intervalo S, em intervalo compara distância ao fim de intervalo e ao fim do turno, lendo template da escala ativa (app/ponto/turnos.py:2474, app/ponto/turnos.py:2483, app/ponto/turnos.py:2488, app/ponto/turnos.py:2493) ; sim: juiz nunca lê relógio (app/docs/CORTES.md:370) versus timezone.now interno (app/ponto/turnos.py:2459) ; proxima_batida_de responde pela oposição ao último tipo gravado, sem janela nem geometria (app/ponto/utils.py:240, app/ponto/utils.py:246); existência sem chamador já registrada no dossiê (app/docs/DOSSIES.md:83) ; -

ponto/turnos.py::decidir_tipo_estrito ; o tipo gravado deve virar (flip)? (app/core/juizes.py:210) ; nenhum texto que fixe os limites do flip [nao verificado]; CORRECAO-2910-0800 inclui o sítio no grupo (1), sem definir limites (app/docs/CORTES.md:359) ; mantém sem marcos ou tipo inválido; vira se distância ao melhor marco oposto <=10 min e nenhum marco próprio está a <=30 min; os limites são parâmetros (app/ponto/turnos.py:2393, app/ponto/turnos.py:2397, app/ponto/turnos.py:2403, app/ponto/turnos.py:2416, app/ponto/turnos.py:2419, app/ponto/turnos.py:2421) ; nao ; nenhum achado de outro decisor final de flip após buscas por decidir_tipo_estrito, flip, FLIP_JANELA_MIN, FLIP_PROPRIO_MIN, casa_marco_oposto e tipo_por_marcos; os escritores examinados delegam ao juiz (app/ponto/services/flip_auto.py:300, app/ponto/services/veredito_celula.py:265); tipo_por_marcos determina tipo recebido retroativo antes da gravação, pergunta distinta (app/api/views.py:579, app/api/views.py:601) ; -

ponto/turnos.py::turno_aberto_de ; ha turno aberto vivo agora? (app/core/juizes.py:211) ; nenhum texto com o predicado completo de presença [nao verificado]; RELOGIO-INJETADO exige instante do chamador (app/docs/CORTES.md:370) ; carrega batidas apuráveis de três dias e escala vigente (app/ponto/turnos.py:2218, app/ponto/turnos.py:2219), pareia com papel da ata e marcos (app/ponto/turnos.py:2272, app/ponto/turnos.py:2278), escolhe último aberto e o recusa somente após fim previsto +4 h; sem escala rígida não expira por esse critério (app/ponto/turnos.py:2006, app/ponto/turnos.py:2011, app/ponto/turnos.py:2024, app/ponto/turnos.py:2030) ; sim: juiz nunca lê relógio (app/docs/CORTES.md:370) versus timezone.now interno (app/ponto/turnos.py:2291) ; seleção de substitutos decide turno aberto pela última batida nas 14 h anteriores ser E, sem perguntar ao juiz (app/ponto/utils.py:183, app/ponto/utils.py:191) ; -

escala/utils.py::_match_marcos ; que marco previsto a batida ocupa? (app/core/juizes.py:212) ; L-087: papel contra DNA, tipo gravado não decide (app/docs/LEIS.md:119); JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES inclui _match_marcos sem tipo na obra (app/docs/CORTES.md:318); PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO reconhece essa pergunta e autoridade (app/docs/CORTES.md:215) ; quando dispõe de papéis e cadastro descreve o turno, alinha pelo papel e consome suas batidas (app/escala/utils.py:596, app/escala/utils.py:609, app/escala/utils.py:613, app/escala/utils.py:623); senão usa candidatos no envelope tol_min, DP e cluster guard (app/escala/utils.py:625, app/escala/utils.py:633, app/escala/utils.py:650) ; sim: lookup diz que batida só ocupa marco mais próximo (app/docs/JUIZES.md:28) versus ramo por papel sem esse guard (app/escala/utils.py:613, app/escala/utils.py:624) ; desvio_escala casa por posição em dia completo e pelo marco mais próximo no incompleto, por conta própria (app/chamados/services/desvio_escala.py:74, app/chamados/services/desvio_escala.py:83); materializacao também deriva marco mais próximo circular com tolerância própria (app/chamados/services/materializacao.py:296, app/chamados/services/materializacao.py:299) ; só marco mais próximo (app/docs/JUIZES.md:28) ou marco do papel atribuído ao turno (app/escala/utils.py:592, app/escala/utils.py:613)?

Contagem parcial: 5 lidos (app/core/juizes.py:208, app/core/juizes.py:212); conflitos 4; achados externos 4; perguntas com duas candidatas escritas 2, ambas incluindo código como candidata. Se coluna (7) exigir duas candidatas exclusivamente normativas, não contar automaticamente essas duas: a distinção depende da interpretação do pedido [nao verificado].

## 14/11 10:39 · sessao 737ec969f133 · - · - · origem=Ronald (tmux)

app/ferias/models.py::PeriodoAquisitivo.dias_saldo ; qual o saldo do periodo aquisitivo? (app/core/juizes.py:268) ; nenhum texto especifico em LEIS.md/CORTES.md encontrado por saldo, dias_saldo e periodo aquisitivo [nao verificado] ; dias_direito menos dias_concedidos menos dias_vendidos, inclusive saldo negativo (app/ferias/models.py:220) ; nao (app/ferias/models.py:22, app/ferias/models.py:220) ; nenhum achado na busca em app por dias_saldo, dias_direito, dias_concedidos, dias_vendidos e formula de subtracao, excluidos testes, migrations, docs e static [nao verificado] ; -

app/ferias/models.py::status_de_periodo ; o periodo venceu ou esta vencendo (art.137)? (app/core/juizes.py:269) ; nenhum texto especifico em LEIS.md/CORTES.md encontrado por art.137, status_de_periodo, vencido e vencendo [nao verificado] ; saldo menor ou igual a zero retorna concedido, data_fim ausente ou maior ou igual a hoje retorna em_curso, limite de concessao anterior a hoje retorna vencido, restante adquirido (app/ferias/models.py:98, app/ferias/models.py:101, app/ferias/models.py:103, app/ferias/models.py:105), vencendo usa adquirido e limite ate hoje mais 60 dias (app/ferias/models.py:249, app/ferias/models.py:260) ; nao (app/ferias/models.py:20, app/ferias/models.py:98, app/ferias/models.py:103) ; achado: situacao_de_ferias deriva vencido por limite anterior a hoje e saldo positivo (app/ferias/situacao.py:193) e vencendo pela janela propria de 60 dias e saldo positivo (app/ferias/situacao.py:206), periodos_vencendo filtra status gravado e data sem consultar status_de_periodo (app/ferias/services.py:225), lista_ferias e PDF repetem esse filtro (app/ferias/views.py:64, app/relatorios/services.py:288) ; -

app/ferias/models.py::PeriodoAquisitivo.dias_vendaveis ; quantos dias ainda podem ser vendidos (abono, 1/3)? (app/core/juizes.py:270) ; nenhum texto especifico em LEIS.md/CORTES.md encontrado por art.143, abono e dias_vendaveis [nao verificado] ; maximo entre zero e divisao inteira de dias_direito por tres menos dias_vendidos (app/ferias/models.py:238) ; nao (app/ferias/models.py:23, app/ferias/models.py:238) ; nenhum achado na busca em app por dias_vendaveis, dias_direito, dias_vendidos e divisao por tres, excluidos testes, migrations, docs e static [nao verificado] ; -

app/ferias/models.py::AgendamentoFerias.clean ; o fracionamento e valido (art.134)? (app/core/juizes.py:271) ; nenhum texto especifico em LEIS.md/CORTES.md encontrado por art.134 e fracionamento [nao verificado] ; com periodo e datas preenchidos, recusa mais de tres fracoes vivas e conjunto sem nenhuma fracao de pelo menos 14 dias, usando violacoes_de_fracionamento (app/ferias/models.py:359, app/ferias/models.py:366, app/ferias/models.py:374), tambem recusa fim anterior ao inicio e dias divergentes do intervalo (app/ferias/models.py:379, app/ferias/models.py:383) ; nao (app/ferias/models.py:15, app/ferias/models.py:366, app/ferias/models.py:374) ; nenhum achado independente na busca por fracionamento, FRACAO_MINIMA, FRACOES_MAXIMAS e violacoes_de_fracionamento em app, excluidos testes e migrations [nao verificado], situacao_de_ferias compartilha o helper (app/ferias/situacao.py:227), avisos_de_agendamento apenas alerta fracao curta e manda verificar as outras (app/ferias/situacao.py:341, app/ferias/situacao.py:344) ; -

app/ferias/situacao.py::DIAS_AVISO_PREVIO ; o aviso de 30 dias foi dado (art.135)? (app/core/juizes.py:272) ; nenhum texto especifico em LEIS.md/CORTES.md encontrado por art.135, aviso previo e DIAS_AVISO_PREVIO [nao verificado] ; constante vigente vale 21 dias (app/ferias/situacao.py:56), aviso pendente surge sem aviso_emitido e com antecedencia menor que essa constante (app/ferias/situacao.py:258), escritor considera aviso_ok com antecedencia maior ou igual a essa constante (app/ferias/services.py:529) ; sim: texto declara aviso de 30 dias (app/ferias/situacao.py:46), codigo fixa 21 (app/ferias/situacao.py:56), campo tambem declara 30 ou mais (app/ferias/models.py:317) ; achado: lista_ferias calcula aviso_pend com prazo literal proprio de 30 dias no contador e no filtro (app/ferias/views.py:35, app/ferias/views.py:86) ; qual antecedencia vale: 30 dias escritos no campo (app/ferias/models.py:317) ou 21 dias escritos na constante (app/ferias/situacao.py:56)?

Neste lote: 5 juizes, 1 conflito, 2 com achado independente, 1 pergunta de lei, conforme as cinco linhas acima.

## 14/11 10:39 · sessao 265cd0800f27 · - · - · origem=Ronald (tmux)

K5 — app/ponto/janelas.py::janela_anterior

- Resposta vigente: pega o início da janela que contém a data e consulta a janela do dia imediatamente anterior, retornando o par início/fim; empresa opcional (app/ponto/janelas.py:66, app/ponto/janelas.py:71, app/ponto/janelas.py:72).
- Textos pertinentes: L-026 declara janela por Empresa.dia_inicio_competencia e inclui expressamente janela_anterior (app/docs/LEIS.md:77); JUIZES nomeia a competência encerrada como janela_anterior (app/docs/JUIZES.md:100). O corte K8 reafirma calendário de competência do fato, em vez de mês civil (app/docs/CORTES.md:216).
- Conflito sim: default 21 escrito em CLAUDE.md:82 e na docstring app/ponto/janelas.py:37 contradiz default 20 executado em app/ponto/janelas.py:50. O juiz depende desse caminho em app/ponto/janelas.py:71. Há chamadores sem empresa, por exemplo app/holerite/matriz.py:41 e app/chamados/juizes.py:612.
- Fora do juiz: nenhum achado de derivação independente atual, nas buscas por janela_anterior, janela_atual com timedelta, competencia_mais_recente, ordenação descendente de ano/mês e subtração de mês. Os antigos candidatos hoje consultam o juiz: app/inteligencia/calculadores.py:206, app/folha/services/ranking_he.py:73 e app/holerite/matriz.py:41. A nota que ainda os declara pendentes em app/core/juizes.py:817 não descreve o código atual de app/inteligencia/calculadores.py:206.
- Pergunta de lei: se exigir duas candidatas normativas textuais, “-”: os textos normativos encontrados concordam no default 21, enquanto 20 aparece somente no código (CLAUDE.md:82, app/ponto/janelas.py:37, app/ponto/janelas.py:50). Se código contar como candidata escrita, a pergunta é “sem empresa, corte 21 ou 20?”, com essas mesmas referências.
- Não contar _limites do holerite como derivação de K5: recebe ano/mês e determina universo civil (app/holerite/matriz.py:51, app/docs/CORTES.md:237, app/core/juizes.py:611).

K8 — app/ponto/janelas.py::janela_atual

- Resposta vigente: período que contém a data, usando corte_da_empresa; quando dia >= corte, a competência é o mês seguinte, incluindo virada de ano; antes do corte, mês da própria data (app/ponto/janelas.py:63, app/ponto/janelas.py:26, app/ponto/janelas.py:27, app/ponto/janelas.py:30, app/ponto/janelas.py:33).
- Textos pertinentes: L-026 e L-090 determinam janela por cadastro da empresa, nunca mês civil (app/docs/LEIS.md:77, app/docs/LEIS.md:123); K8-COMPETENCIA-NAO-E-MES-CIVIL explicita a competência da data do fato (app/docs/CORTES.md:216). JUIZES nomeia janela_atual (app/docs/JUIZES.md:99).
- Conflito sim: mesmo fallback 21 textual versus 20 executado (CLAUDE.md:82, app/ponto/janelas.py:50), dependência direta em app/ponto/janelas.py:63.
- Fora do juiz, achado: tela_exportar_dominio escolhe mês/ano civil de timezone.now() por conta própria (app/folha/views.py:90, app/folha/views.py:91), selecionado como “Mês competência” (app/templates/folha/exportar.html:35, app/templates/folha/exportar.html:38), enviado ao preview (app/templates/folha/exportar.html:36), que o passa a classificar_export (app/folha/views.py:320, app/folha/views.py:331). Outro sítio equivalente em index (app/folha/views.py:448, app/folha/views.py:449).
- Pergunta de lei: mesma ressalva de K5 sobre o significado de duas candidatas escritas; não encontrei texto normativo candidato a default 20 (CLAUDE.md:82, app/ponto/janelas.py:37, app/ponto/janelas.py:50).
- L-104 e HOLERITE-MES-CIVIL não são alternativas para o mesmo K8: tratam universo do holerite e vigência de ausência (app/docs/LEIS.md:130, app/docs/CORTES.md:237).

## 14/11 10:38 · sessao 1655b8a96199 · - · - · origem=Ronald (tmux)

K4 — app/ponto/services/fechamento.py::competencia_trancada

- Resposta do código: sem empresa, False; com empresa, True se existir PeriodoFechado para empresa, mês e ano com reaberto_em nulo. Status aprovado, exportado e holerite não participam do predicado. app/ponto/services/fechamento.py:1477, app/ponto/services/fechamento.py:1480.
- Textos pertinentes: CORTES K8 distingue tranca da empresa em PeriodoFechado e fechamento individual em FechamentoMensal. app/docs/CORTES.md:216. L-030 proíbe barrar batida de chão. app/docs/LEIS.md:78. L-113 revoga recusas de recálculo e regeneração em exportada como trava. app/docs/LEIS.md:153. O corte de folha determina que recálculo e TXT não consultem tranca. app/docs/DOSSIES.md:122.
- Conflito substantivo escrito: “trancada (nada mais muda)” no módulo do juiz versus “Nenhuma trava ... exportada/trancada/paga: correcao provada entra em qualquer competencia”. app/ponto/services/fechamento.py:1457 versus CLAUDE.md:422. Outra dupla: modelo diz que existência do registro torna batidas read-only, versus proibição de trava por trancada. app/ponto/models.py:913 versus CLAUDE.md:422.
- Fora do juiz: dia_em_competencia_trancada consulta diretamente PeriodoFechado com reaberto_em nulo e verifica pertencimento do dia à janela; responde à existência da tranca sem chamar K4/K7. app/chamados/juizes.py:2054, app/chamados/juizes.py:2057. medir_frota também seleciona diretamente as trancas vivas, antes de medir cada competência. app/chamados/services/competencia_trancada.py:346.
- Para coluna 7, duas candidatas escritas sobre a fronteira: imutabilidade por existência de PeriodoFechado versus nenhuma trava por trancada. app/ponto/models.py:913, CLAUDE.md:422. Não há duas candidatas de critério para o booleano K4 nos textos legais consultados [nao verificado].

K7 — app/ponto/services/fechamento.py::periodo_trancado

- Resposta do código: sem empresa, None; caso contrário, primeira linha PeriodoFechado da empresa, mês e ano com reaberto_em nulo. A função retorna a tranca viva, não um booleano que distinga “nunca trancado” de “reaberto”. app/ponto/services/fechamento.py:1467, app/ponto/services/fechamento.py:1470.
- Nenhuma definição do critério de reabertura encontrada em LEIS/CORTES/DOSSIES/CLAUDE; CORTES manda o C6 ler periodo_trancado e descontar o dia tocado pelo admin, sem definir reaberto_em/ativo. app/docs/CORTES.md:272.
- Conflito documental de campo: modelo declara “ativo ... False = reaberto”, enquanto o juiz declara “reaberto_em e a verdade; ativo e so documento” e só filtra o timestamp. app/ponto/models.py:933 versus app/ponto/services/fechamento.py:1466, app/ponto/services/fechamento.py:1470. A tabela lookup também afirma “reabertura: SEM juiz”, embora o registro declare K7. app/docs/JUIZES.md:101 versus app/core/juizes.py:334.
- Fora do juiz: trancar_periodo decide por conta própria se houve reabertura mediante periodo.reaberto_em is not None. app/ponto/services/fechamento.py:1412. Também há seleção direta de tranca viva em dia_em_competencia_trancada e medir_frota. app/chamados/juizes.py:2054, app/chamados/services/competencia_trancada.py:346.
- Escritores confirmados: serviço reabrir_periodo grava ativo=False e timestamp; view reabrir_periodo chama K7, depois grava timestamp sem modificar ativo. app/ponto/services/fechamento.py:1437, app/ponto/services/fechamento.py:1439, app/ponto/views.py:1540, app/ponto/views.py:1544, app/ponto/views.py:1546. O retrancamento testa timestamp, grava ativo=True e limpa timestamp. app/ponto/services/fechamento.py:1412, app/ponto/services/fechamento.py:1413, app/ponto/services/fechamento.py:1417.
- Coluna 7 recomendada “-”: não encontrei duas candidatas de lei; a divergência ativo/timestamp está em documentação do modelo e do juiz [nao verificado].

## 14/11 10:36 · sessao 73189c7f2451 · [nome]-ANDAM-JUNTO · sonda · origem=Code

A regra aplicada é a do mapa: **D** = dia segura a folha; **S** = não segura e lavrado não é `irregular`; **I** = sem col/dia, futuro ou `irregular` sem segurar. Em D, **C/E/B** indicam CADASTRO/ESTRUTURA/BATIDA. Fonte: `app/docs/RELATO.md:8252`.

As comparações abaixo derivam dos dois anexos fornecidos; cada resultado está marcado **[nao verificado]** por não ter arquivo:linha local da remedição.

| Unidade | col | Dia | 05/11 → 14/11 | Evidência |
|---|---|---|---|---|
| HJ-001-U1 | col836 | 13/10 | D-C → D-C | [nao verificado] |
| HJ-001-U2 | col836 | 14/10 | D-C → D-C | [nao verificado] |
| HJ-002-U1 | col909 | 16/10 | **D-B → S** | [nao verificado] |
| HJ-003-U1 | col407 | 21/10 | D-B → D-B | [nao verificado] |
| HJ-004-U1 | col789 | 11/10 | S → S | [nao verificado] |
| HJ-004-U2 | col789 | n/a | I → I | [nao verificado] |
| HJ-005-U1 | col42 | 01/10 | **D-E → I** | [nao verificado] |
| HJ-005-U2 | col42 | 08/10 | **D-E → S** | [nao verificado] |
| HJ-005-U3 | col42 | 09/10 | S → S | [nao verificado] |
| HJ-005-U4 | col42 | 15/10 | **D-E → S** | [nao verificado] |
| HJ-005-U5 | col42 | 22/10 | **D-B → S** | [nao verificado] |
| HJ-006-U1 | col226 | 21/10 | S → S | [nao verificado] |
| HJ-007-U1 | col253 | n/a | I → I | [nao verificado] |
| HJ-008-U1 | col283 | n/a | I → I | [nao verificado] |
| HJ-009-U1 | col504 | 21/10 | S → S | [nao verificado] |
| HJ-010-U1 | col255 | 03/10 | S → S | [nao verificado] |
| HJ-011-U1 | col709 | 22/10 | S → S | [nao verificado] |
| HJ-012-U1 | col239 | 07/10 | I → I | [nao verificado] |
| HJ-012-U2 | col239 | 12/10 | D-E → D-E | [nao verificado] |
| HJ-013-U1 | col43 | 10/10 | S → S | [nao verificado] |
| HJ-013-U2 | col43 | 12/10 | S → S | [nao verificado] |
| HJ-013-U3 | col43 | 26/10 | S → S | [nao verificado] |
| HJ-014-U1 | col956 | 16/10 | S → S | [nao verificado] |
| HJ-014-U2 | col956 | 22/10 | S → S | [nao verificado] |
| HJ-015-U1 | col39 | 15/10 | D-B → D-B | [nao verificado] |
| HJ-016-U1 | col297 | 21/09 | D-C → D-C | [nao verificado] |
| HJ-016-U2 | col297 | 06/10 | S → S | [nao verificado] |
| HJ-016-U3 | col297 | 13/10 | S → S | [nao verificado] |
| HJ-016-U4 | col297 | 20/10 | S → S | [nao verificado] |
| HJ-016-U5 | col297 | 27/10 | S → S | [nao verificado] |
| HJ-017-U1 | col923 | 27/09 | D-E → D-E | [nao verificado] |
| HJ-017-U2 | col923 | n/a | I → I | [nao verificado] |
| HJ-018-U1 | col130 | 05/10 | S → S | [nao verificado] |
| HJ-019-U1 | col29 | 15/10 | S → S | [nao verificado] |
| HJ-020-U1 | col189 | 21/09 | S → S | [nao verificado] |
| HJ-020-U2 | col189 | 19/10 | S → S | [nao verificado] |
| HJ-021-U1 | col204 | 21/09 | S → S | [nao verificado] |
| HJ-022-U1 | col502 | 09/10 | S → S | [nao verificado] |
| HJ-023-U1 | col222 | 12/10 | S → S | [nao verificado] |
| HJ-023-U2 | col256 | 12/10 | S → S | [nao verificado] |
| HJ-024-U1 | col55 | 03/10 | S → S | [nao verificado] |
| HJ-025-U1 | col270 | n/a | I → I | [nao verificado] |
| HJ-026-U1 | col129 | n/a | I → I | [nao verificado] |
| HJ-027-U1 | col292 | 21/09 | D-B → D-B | [nao verificado] |
| HJ-027-U2 | col292 | 09/10 | S → S | [nao verificado] |
| HJ-028-U1 | col174 | 25/10 | S → S | [nao verificado] |
| HJ-028-U2 | col174 | 26/10 | S → S | [nao verificado] |
| HJ-029-U1 | col33 | 03/10 | S → S | [nao verificado] |
| HJ-029-U2 | col33 | 04/10 | S → S | [nao verificado] |
| HJ-030-U1 | col94 | 16/10 | D-E → D-E | [nao verificado] |
| HJ-031-U1 | col247 | 24/10 | I → I | [nao verificado] |
| HJ-032-U1 | col163 | n/a | I → I | [nao verificado] |
| HJ-033-U1 | col960 | n/a | I → I | [nao verificado] |
| HJ-034-U1 | col873 | 21/09 | S → S | [nao verificado] |
| HJ-034-U2 | col873 | 26/09 | S → S | [nao verificado] |
| HJ-035-U1 | col949 | n/a | I → I | [nao verificado] |
| HJ-036-U1 | col207 | n/a | I → I | [nao verificado] |
| HJ-037-U1 | col332 | n/a | I → I | [nao verificado] |
| HJ-038-U1 | col446 | n/a | I → I | [nao verificado] |
| HJ-039-U1 | col89 | n/a | I → I | [nao verificado] |
| HJ-040-U1 | col92 | n/a | I → I | [nao verificado] |
| HJ-041-U1 | col231 | n/a | I → I | [nao verificado] |
| HJ-042-U1 | col214 | n/a | I → I | [nao verificado] |
| HJ-043-U1 | col235 | n/a | I → I | [nao verificado] |
| HJ-044-U1 | col246 | n/a | I → I | [nao verificado] |
| HJ-045-U1 | col250 | n/a | I → I | [nao verificado] |
| HJ-046-U1 | col673 | n/a | I → I | [nao verificado] |
| HJ-047-U1 | col194 | n/a | I → I | [nao verificado] |
| HJ-048-U1 | col219 | n/a | I → I | [nao verificado] |
| HJ-049-U1 | col288 | 04/10 | S → S | [nao verificado] |
| HJ-050-U1 | col523 | 25/10 | S → S | [nao verificado] |
| HJ-050-U2 | col523 | 07/11 | **I → S** | [nao verificado] |
| HJ-051-U1 | col123 | n/a | I → I | [nao verificado] |
| HJ-052-U1 | col1023 | n/a | I → I | [nao verificado] |
| HJ-052-U2 | col78 | n/a | I → I | [nao verificado] |
| HJ-053-U1 | n/a | n/a | I → I | [nao verificado] |
| HJ-054-U1 | n/a | n/a | I → I | [nao verificado] |
| HJ-055-U1 | n/a | n/a | I → I | [nao verificado] |
| HJ-056-U1 | col270 | n/a | I → I | [nao verificado] |
| HJ-056-U2 | n/a | n/a | I → I | [nao verificado] |

**Seis unidades mudaram de classe; outras três mudaram campos comparáveis.** As alterações são estas [nao verificado]:

| Unidade | Campos que mudaram: 05/11 → 14/11 | Evidência |
|---|---|---|
| HJ-002-U1 | estado D→S; dono publicado BATIDA→null; motivo “segura: espelho_furo”→“lavrado ok; não segura”; status irregular→ok; export fora→entra; chamado 31891 em_analise→resolvido. Dono bruto atual `b.dono=ESTRUTURA`. | [nao verificado] | [nao verificado]
| HJ-005-U1 | estado D→I; dono publicado ESTRUTURA→null; motivo “segura”→“irregular sem segurar”; export fora→entra; chamados 27706 e 27822 resolvido→fechado. Status continua irregular. | [nao verificado] | [nao verificado]
| HJ-005-U2 | estado D→S; dono publicado ESTRUTURA→null; motivo “segura”→“lavrado escala_suspeita; não segura”; export fora→entra; chamados 29676 e 29809 resolvido→fechado. | [nao verificado] | [nao verificado]
| HJ-005-U3 | export fora→entra; chamados 29918, 29919 e 30279 resolvido→fechado. | [nao verificado] |
| HJ-005-U4 | estado D→S; dono publicado ESTRUTURA→null; motivo “segura”→“lavrado escala_suspeita; não segura”; export fora→entra; chamados 31549 e 31699 resolvido→fechado. | [nao verificado] | [nao verificado]
| HJ-005-U5 | estado D→S; dono publicado BATIDA→null; motivo “segura”→“lavrado escala_suspeita; não segura”; chamado 33487 resolvido→fechado. Export continua fora, hoje com motivo `rescisao_modulo_proprio`; o motivo antigo de export não foi fornecido. | [nao verificado] | [nao verificado]
| HJ-013-U3 | export fora→entra. | [nao verificado] |
| HJ-014-U2 | export entra→fora. | [nao verificado] |
| HJ-050-U2 | estado I→S; motivo “dia futuro”→“lavrado ok; não segura”; status previsto→ok; data deixou de ser futura na leitura. | [nao verificado] |

O `dono=null` de S/I é o campo normalizado do mapa; não significa ausência de valor em `b.dono`. `grupo` e `obra` não foram remedidos pela saída a–f; permanecem **n/a (medida)** nesta comparação [nao verificado].

Os totais de hoje são [nao verificado]:

| Classe | Total | Evidência |
|---|---:|---|
| Divergente com dono CADASTRO | 3 | [nao verificado] |
| Divergente com dono ESTRUTURA | 3 | [nao verificado] |
| Divergente com dono BATIDA | 3 | [nao verificado] |
| Sem divergência hoje pela função | 38 | [nao verificado] |
| Indeterminado | 33 | [nao verificado] |
| **Soma / total de unidades do mapa remedido** | **80** | [nao verificado] |

**3 + 3 + 3 + 38 + 33 = 80.** Os 33 indeterminados são 30 sem col/dia suficiente, mais HJ-005-U1, HJ-012-U1 e HJ-031-U1, todos `irregular` sem segurar [nao verificado].

Para HJ-057+, estas são as linhas do BACKLOG que contêm os IDs ou referências a eles; os conteúdos estão abreviados, preservando os IDs:

| Linha | ID da linha / referência HJ | Conteúdo |
|---|---|---|
| `app/docs/BACKLOG.md:728` | FERIADO-ESCALA / HJ-074 | Escala e trabalho em feriado; escopo anterior superado pelo aval FERIADO-USA-A-LEI-DE-1609. |
| `app/docs/BACKLOG.md:739` | RAIA-PREPARACAO / HJ-091 | Censo antes da vaga, junto às outras obras nomeadas. |
| `app/docs/BACKLOG.md:741` | HJ-092 | Divergência de tipo de marco; fechada como BATIDA; candidatas ao admin. |
| `app/docs/BACKLOG.md:742` | TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO / HJ-092 | Avisar troca de tipo de batida nascida de pergunta. |
| `app/docs/BACKLOG.md:743` | HJ-093 | col1004, 27/10; classe JUSTIFICATIVA-PAGA-O-PERIODO e decisão humana O168. |
| `app/docs/BACKLOG.md:1836` | TIPO-NA-FONTE / HJ-090 | Item 3 parado pelo corte; sequência de dependências inclui HJ-090. |
| `app/docs/BACKLOG.md:1873` | FERIAS-SALDO-SEM-TRAVA / HJ-090 | Absorvida em HJ-090. |
| `app/docs/BACKLOG.md:1874` | HJ-090 / HJ-091 | Travas, saldo e portas de férias; complemento HJ-091 condicionado ao censo de arquivos. |
| `app/docs/BACKLOG.md:1875` | JUSTIFICATIVA-PAGA-O-PERIODO / HJ-093, HJ-090 | Pagamento do período; DIFF antes; retorno à ordem após HJ-090. |
| `app/docs/BACKLOG.md:2109` | CONTRATO-DE-ENTRADA-DO-FIO / HJ-095 | Emissor deve entregar o episódio; dependência E3 e DESLIGADO/FERIADO. |
| `app/docs/BACKLOG.md:2492` | UI-ESCALA-REQUADRO / HJ-069 | Requadro por escala; extensão posterior ao plano e histórico funcional. |
| `app/docs/BACKLOG.md:2500` | RAIZ-DA-DISPUTA-F0-CASO-7279 / HJ-067, HJ-073 | F0 antes de F1 e desses HJ; disputa 7279 como caso-prova. |
| `app/docs/BACKLOG.md:2503` | HJ-066 | Retirar cards de férias e benefícios das duas dashboards. |
| `app/docs/BACKLOG.md:2504` | HJ-068 | Fio do admin acionável; encerrados em histórico; contador correspondente. |
| `app/docs/BACKLOG.md:2505` | HJ-071 | Mostrar intervalo declarado no espelho/PDF; primeiro medir se está gravado. |
| `app/docs/BACKLOG.md:2506` | HJ-073 | Censo de “Decidir” por causa/permissão; cura após censo e F0. |
| `app/docs/BACKLOG.md:2507` | HJ-074 | col148, 02/11; vínculo com FERIADO-ESCALA. |
| `app/docs/BACKLOG.md:2508` | HJ-075 | col1032; escala no app; aguarda imagem para confirmar causa. |
| `app/docs/BACKLOG.md:2509` | HJ-075-EC-FIRST | Escolha de EC ativa por `.first()` sem vigência. |
| `app/docs/BACKLOG.md:2510` | HJ-058 | Anexo HEIC; ocorrência 06/11; cura com O30 F3 e rebuild. |
| `app/docs/BACKLOG.md:2511` | HJ-058-MEDIA-AUSENCIAS | Arquivos de mídia; censo da origem antes de cura. |

Abaixo, o inventário mantém os campos do mapa. **Para todas essas entradas: estado/motivo = “nao medida pela sonda”; dono, grupo, status, export e chamados = n/a (medida)** [nao verificado]. Os anexos não fornecem seus sufixos Ux; atribuí-los como existentes seria [nao verificado].

| ID documental — Ux n/a | col | Dia do caso | Obra da GRADE-MESTRA | Portão |
|---|---|---|---|---|
| HJ-058 | n/a | 06/11 (`app/docs/BACKLOG.md:2510`) | UI-ANEXO-AUSENCIA (`app/docs/GRADE-MESTRA.md:1517`), com O30 (`app/docs/GRADE-MESTRA.md:480`) | **lei**: vale F1 para F2/F3 (`app/docs/BACKLOG.md:1311`); rebuild também pendente (`app/docs/GRADE-MESTRA.md:1517`). |
| HJ-058-MEDIA-AUSENCIAS | n/a | n/a (`app/docs/BACKLOG.md:2511`) | nenhuma [nao verificado] | **censo** da origem antes da cura (`app/docs/BACKLOG.md:2511`). |
| HJ-066 | n/a | n/a (`app/docs/BACKLOG.md:2503`) | nenhuma [nao verificado] | [nao verificado] |
| HJ-067 | n/a | n/a; referência à F0 (`app/docs/BACKLOG.md:2500`) | nenhuma vinculação que prove fechamento [nao verificado] | F0 precede o item (`app/docs/BACKLOG.md:2500`); portão atual [nao verificado]. |
| HJ-068 | n/a | n/a (`app/docs/BACKLOG.md:2504`) | nenhuma [nao verificado] | [nao verificado] |
| HJ-069 | n/a | n/a (`app/docs/BACKLOG.md:2492`) | nenhuma [nao verificado] | **censo** do drawer; exige ausência de arquivo comum (`app/docs/BACKLOG.md:2492`). |
| HJ-071 | n/a | n/a (`app/docs/BACKLOG.md:2505`) | nenhuma [nao verificado] | **censo**: medir gravação do intervalo primeiro (`app/docs/BACKLOG.md:2505`). |
| HJ-073 | n/a | n/a (`app/docs/BACKLOG.md:2506`) | nenhuma [nao verificado] | Censo/F0 exigidos (`app/docs/BACKLOG.md:2506`), com resultados registrados em `app/docs/RELATO.md:532`; portão atual [nao verificado]. |
| HJ-074 | col148 | 02/11 (`app/docs/BACKLOG.md:2507`) | FERIADO-ESCALA (`app/docs/GRADE-MESTRA.md:122`) | Lei nomeada FERIADO-USA-A-LEI-DE-1609 (`app/docs/GRADE-MESTRA.md:122`); bloqueio atual ou **livre** [nao verificado]. |
| HJ-075 | col1032 | n/a (`app/docs/BACKLOG.md:2508`) | nenhuma [nao verificado] | **censo**: aguarda imagem para confirmar causa (`app/docs/BACKLOG.md:2508`). |
| HJ-075-EC-FIRST | n/a | n/a (`app/docs/BACKLOG.md:2509`) | nenhuma [nao verificado] | [nao verificado] |
| HJ-090 | n/a | n/a na linha própria (`app/docs/BACKLOG.md:1874`) | HJ-090 (`app/docs/GRADE-MESTRA.md:263`) | Pouso/NO AR registrado (`app/docs/RELATO.md:8`); portão de trabalho residual [nao verificado]. |
| HJ-091 | n/a | n/a (`app/docs/BACKLOG.md:1874`) | HJ-090, cuja coluna resolve inclui HJ-091 (`app/docs/GRADE-MESTRA.md:263`) | Colisão histórica com HJ-090 (`app/docs/RELATO.md:98`); após seu pouso (`app/docs/RELATO.md:8`), portão atual [nao verificado]. |
| HJ-092 | n/a | n/a; linha dá célula, não col/dia (`app/docs/BACKLOG.md:741`) | HJ-092 (`app/docs/GRADE-MESTRA.md:123`); complemento TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO (`app/docs/GRADE-MESTRA.md:464`) | **lei/decisão humana**: candidatas ao admin (`app/docs/BACKLOG.md:741`; `app/docs/GRADE-MESTRA.md:123`). |
| HJ-093 | col1004 | 27/10 (`app/docs/BACKLOG.md:743`) | JUSTIFICATIVA-PAGA-O-PERIODO (`app/docs/GRADE-MESTRA.md:264`); HJ-093 (`app/docs/GRADE-MESTRA.md:1371`) | **lei O168** para a pergunta; período financeiro depende de DIFF/motor/folha (`app/docs/BACKLOG.md:743`; `app/docs/GRADE-MESTRA.md:264`). |
| HJ-095 | n/a | n/a (`app/docs/BACKLOG.md:2109`) | CONTRATO-DE-ENTRADA-DO-FIO (`app/docs/GRADE-MESTRA.md:682`) | Espera E3 e DESLIGADO/FERIADO (`app/docs/BACKLOG.md:2109`); classificação como arquivo congelado ou colisão atual [nao verificado]. |

HJ-057 não teve linha localizada na cópia [nao verificado]. Em HJ-075, **13/11 08:52 é o instante da conferência do servidor**, não o dia da ocorrência (`app/docs/BACKLOG.md:2508`). O total **80** refere-se ao mapa remedido; a quantidade adicional de unidades pessoa/dia HJ-057+ permanece [nao verificado].

Não há prova para declarar portões restantes **livres**, nem para nomear arquivo congelado ou colisão atual: a regra exige arquivo em comum (`CLAUDE.md:309`), e o diff das raias vivas não foi fornecido [nao verificado].

Quanto a erros, o anexo informa **processadas=50, com_erro=0, puladas=30, soma=80**: nenhuma unidade com erro foi demonstrada [nao verificado]. A comparação das assinaturas desde 05/11 permanece [nao verificado]; a cópia mostra, por exemplo, `agora` obrigatório em `InsumosDoDono.__init__` (`app/ponto/management/commands/e6_oraculo.py:128`), mas isso sozinho não prova quebra da sonda. As 30 puladas continuam indeterminadas; não foram convertidas em zero nem em ausência de divergência [nao verificado].

## 14/11 10:36 · sessao 118a027447d0 · COERENCIA-PELO-EFEITO · censo · origem=Code

`falta_diurna` recebe **`len(datas_falta(...))`**, montado separadamente dos eventos de horas; não recebe `FechamentoMensal.horas_falta` (`app/folha/export.py:218`, `app/folha/export.py:740`). Com rubrica do tipo `dias`, o TXT contém a quantidade inteira; nos demais tipos, contém quantidade × 100 (`app/folha/export.py:39`). Valor exportado, código e tipo efetivamente cadastrados em produção: **[nao verificado]**.

No censo abaixo, **SEM JUIZ** significa ausência de registro para o corpo que executa aquela decisão; não atribuí a um auxiliar o registro de outra função. O registro distingue, por exemplo, `efeito_vigente`, `cobertura_ausencia_periodo`, `estado_inicial` e `veto_de_lancamento` (`app/core/juizes.py:252`). **SEM CASO** significa que não localizei teste numérico contra CLT/CCT/L-NNN para aquela decisão; inexistência absoluta de outro teste: **[nao verificado]**. Testes de recusa, estado, paridade ou caracterização são identificados, mas não entram como prova numérica.

| passo | decisao | arquivo::funcao:linha | grava em | relido por | juiz | caso pela regra |
|---|---|---|---|---|---|---|
| 1 | `tipo == 'dias'`: arredonda quantidade; caso contrário, multiplica por 100 e arredonda. | `app/folha/export.py::montar_linha_10:39` | Campo valor do registro 10 | `partes_da_linha`, `app/folha/export.py:251` | SEM JUIZ | SEM CASO |
| 2 | O número é `len(datas)`, com um registro 11 por elemento. | `app/folha/export.py::montar_bloco_falta:218` | Registros 10/11 | Cartão conta a mesma lista, `app/relatorios/cartao_pela_celula.py:104` | SEM JUIZ | **L-039:** `app/relatorios/tests/test_falta_um_significado.py::FaltaUmSignificado::test_MORDE_o_furo_sem_decisao_nao_vira_falta_e_nao_some:121`: sem decisão → 0; uma falta aprovada → 1. Associação à lei: `app/docs/LEIS.md:87`. Cobre o leitor, não a serialização do TXT. |
| 3 | Concatena expansões e ordena; não transforma a lista em conjunto. Datas repetidas permanecem contáveis. | `app/folha/export.py::_datas_ausencia_tipo_janela:159` | Lista transitória | `montar_bloco_falta`, `app/folha/export.py:218` | SEM JUIZ | SEM CASO |
| 4 | Só emite o bloco quando `_datas and _rub_f`. | `app/folha/export.py::montar_linhas_export:740` | Buffer do TXT | Validador, `app/folha/views.py:275` | SEM JUIZ | SEM CASO |
| 5 | Rubricas ativas da empresa sobrescrevem as globais pelo evento. | `app/folha/export.py::_rubricas_map:988` | Mapa transitório; cadastro escrito por `app/folha/rubricas.py:27` | Montagem `app/folha/export.py:713`; homologação `app/folha/export.py:1014` | SEM JUIZ | SEM CASO |
| 6 | Só associa catálogo quando o código possui exatamente uma descrição distinta. | `app/folha/rubricas.py::_catalogo_inequivoco:12` | `RubricaDominio.catalogo`, `app/folha/rubricas.py:30` | Definição da unidade, `app/folha/rubricas.py:25` | SEM JUIZ | SEM CASO |
| 7 | Catálogo inequívoco com unidade `D` → `dias`; demais → `horas`. | `app/folha/rubricas.py::vincular_evento:24` | `RubricaDominio.tipo`, `app/folha/rubricas.py:29` | Codificação, `app/folha/export.py:39` | SEM JUIZ | SEM CASO |
| 8 | Montagem admite somente fechamentos em `('aberto', 'aprovado')`, inclusive na lista injetada. | `app/folha/export.py::montar_linhas_export:717` | Seleção transitória | Laço de emissão, `app/folha/export.py:725` | SEM JUIZ | SEM CASO |
| 9 | A porta HTTP exige existir fechamento gravado aberto/aprovado antes do cálculo online. | `app/folha/views.py::gerar_dominio_txt:148` | Não grava; pode interromper resposta | Cálculo online, `app/folha/views.py:207` | SEM JUIZ | SEM CASO |
| 10 | Sem integração Domínio ativa, levanta erro antes da montagem. | `app/folha/export.py::codigo_no_sistema_dominio:26` | Cadastro por `app/folha/services/integracao.py:17` e `:24` | Guarda HTTP, `app/folha/views.py:138` | SEM JUIZ | SEM CASO |
| 11 | Demissão existente e `data_demissao <= fim_janela` → fora por rescisão. | `app/folha/export.py::sai_pela_rescisao:60`; `::classificar_export:866` | Item transitório `status='fora'` | `entra_set`, `app/folha/export.py:716` | **K2**, registro `app/core/juizes.py:320` | **SEM CASO numérico.** Há CLT477 e teste de classificação: `app/folha/tests/test_rescisao_modulo_proprio.py::RescisaoModuloProprioTest::test_desligado_na_janela_vira_rescisao:24`. |
| 12 | Código Domínio vazio após `strip()` → fora. | `app/folha/export.py::classificar_export:840`; `:872` | Item transitório | `entra_set`, `app/folha/export.py:716` | **K2**, `app/core/juizes.py:320` | SEM CASO |
| 13 | Férias intersectam a janela por datas; status cancelado é excluído. | `app/ferias/services.py::ferias_na_janela:53` | Queryset | Cruzamento com batidas, `app/folha/export.py:491` | SEM JUIZ; o registro nomeia `em_gozo_hoje`, `app/core/juizes.py:262` | SEM CASO |
| 14 | Uma batida apurável cuja data local pertence a qualquer faixa de férias inclui o dia no cruzamento. | `app/folha/export.py::dias_ferias_com_batida:499` | Mapa de dias | Gate de férias, `app/folha/export.py:876` | SEM JUIZ | SEM CASO |
| 15 | Cruzamento de férias/batida não vazio → fora. | `app/folha/export.py::classificar_export:877` | Item transitório | `entra_set`, `app/folha/export.py:716` | **K2**, `app/core/juizes.py:320` | SEM CASO |
| 16 | Horas trabalhadas zero, pelo menos duas batidas apuráveis e nenhum dia com previsto não zero → `cadastro_zero`. | `app/folha/export.py::classificar_export:902`; `:924`; `:942`; `:946` | Item transitório | `entra_set`, `app/folha/export.py:716` | **K2**, `app/core/juizes.py:320` | SEM CASO |
| 17 | Universo de batidas exige `retratada_em IS NULL`; aplica limites de instante e colaborador. | `app/ponto/turnos.py::batidas_apuraveis:2876` | Queryset | Férias `app/folha/export.py:499`; cadastro zero `app/folha/export.py:914` | SEM JUIZ | SEM CASO |
| 18 | Isenção de ponto libera da retenção por célula. | `app/folha/export.py::motivos_retencao_celula:1207` | Lista vazia de motivos | `classificar_export`, `app/folha/export.py:968` | SEM JUIZ | SEM CASO |
| 19 | `esta_desligado(colab)` libera da retenção por célula. | `app/folha/export.py::motivos_retencao_celula:1207` | Lista vazia de motivos | `classificar_export`, `app/folha/export.py:968` | **ausencia/ferias — desligado**, `app/core/juizes.py:267` | SEM CASO numérico |
| 20 | Retenção recorta início pela admissão e fim pela demissão; janela colapsada retorna vazia. | `app/folha/export.py::_dias_sem_decisao:1239` | Janela transitória | Laço diário, `app/folha/export.py:1265` | SEM JUIZ | SEM CASO |
| 21 | Sem célula e sem vínculo cobrindo o dia → ignora o dia. | `app/folha/export.py::_dias_sem_decisao:1267` | Motivos de retenção | Gate, `app/folha/export.py:968` | SEM JUIZ para esse corte | **SEM CASO numérico.** Caso de motivos vazios sob L-110: `app/folha/tests/test_e4_parte5_dinheiro.py::RetencaoTest::test_MORDE_desativado_sem_fim_nao_retem_dia_sem_celula:69`. |
| 22 | Sem célula, dia passado e diferente do dia de demissão → retém. | `app/folha/export.py::_dias_sem_decisao:1270` | Motivo `sem_celula` | Gate, `app/folha/export.py:968` | SEM JUIZ | **SEM CASO numérico.** Caso de retenção: `app/folha/tests/test_e4_parte5_dinheiro.py::RetencaoTest::test_ativo_aberto_segue_retendo_dia_sem_celula:74`. |
| 23 | Veredito acusador retém, salvo protestos não vazios inteiramente não cobráveis ou exceção da pausa. | `app/folha/export.py::_dias_sem_decisao:1273`; `:1293` | Motivo `espelho_<veredito>` | Gate, `app/folha/export.py:968` | SEM JUIZ | SEM CASO numérico |
| 24 | Exceção da pausa exige protestos exatamente `{'FURO_PARCIAL'}`. | `app/folha/export.py::nao_retem_pela_pausa:1173` | Booleano | Retenção, `app/folha/export.py:1293` | SEM JUIZ | **SEM CASO numérico.** L-033, resposta sem retenção: `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83`. |
| 25 | Ata desalinhada, em aberto ou desconhecida não isenta; exige alguma lâmpada apagada e todas de pausa. | `app/folha/export.py::_furo_so_da_pausa:1150` | Booleano | Exceção, `app/folha/export.py:1173` | SEM JUIZ para esse predicado; `periodos_do_dia` está registrado em `app/core/juizes.py:1193` | **SEM CASO numérico.** Limites comportamentais da L-033: `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_MORDE_entrada_faltando_RETEM:94`. |
| 26 | Veredito nulo, `trabalha is not None`, dia passado e fora da demissão → retém. | `app/folha/export.py::_dias_sem_decisao:1295` | Motivo `sem_celula` | Gate, `app/folha/export.py:968` | SEM JUIZ | SEM CASO |
| 27 | Qualquer motivo retornado pelo retentor → fora por `furo_espelho`. | `app/folha/export.py::classificar_export:968` | Item transitório | `entra_set`, `app/folha/export.py:716` | **K2**, `app/core/juizes.py:320` | SEM CASO numérico |
| 28 | Catálogo de homologação vazio desliga o gate. | `app/folha/export.py::_codigos_nao_homologados:999` | Lista vazia | Classificação, `app/folha/export.py:974` | SEM JUIZ | SEM CASO |
| 29 | Havendo datas e rubrica, código da falta fora de `homo` entra na lista de não homologados. | `app/folha/export.py::_codigos_nao_homologados:1013` | Lista de códigos | Exclusão, `app/folha/export.py:975` | SEM JUIZ | SEM CASO |
| 30 | Validador confere quantidade de registros 11 contra valor apenas para código literal `8792`; erro impede entrega HTTP. | `app/folha/export.py::validar_layout_dominio:280`; `app/folha/views.py::gerar_dominio_txt:275` | Lista de erros | Resposta HTTP, `app/folha/views.py:279` | SEM JUIZ | SEM CASO |
| 31 | Carimbo precisa existir para a empresa no dia local atual. | `app/folha/porta_export.py::conferir:807`; `:811` | Não grava | Porta HTTP, `app/folha/views.py:129` | SEM JUIZ | SEM CASO |
| 32 | Mês/ano do detalhe do carimbo devem coincidir com o pedido. | `app/folha/porta_export.py::conferir:815` | Não grava | Porta HTTP, `app/folha/views.py:130` | SEM JUIZ | SEM CASO |
| 33 | Valor nulo ou negativo do carimbo recusa por medição indisponível. | `app/folha/porta_export.py::conferir:818` | Não grava | Porta HTTP, `app/folha/views.py:130` | SEM JUIZ | SEM CASO |
| 34 | Valor positivo recusa; zero libera. | `app/folha/porta_export.py::conferir:821`; `:867` | Não grava | Porta HTTP, `app/folha/views.py:130` | SEM JUIZ | SEM CASO |
| 35 | Falhas somam divergências PDF/topo/builder/calendário/rubrica e, quando habilitado, dias de HE; resultado incorpora cartão × TXT. | `app/folha/porta_export.py::medir:687`; `:764` | `MetricaSnapshot.valor`, por `::lavrar:772` | `conferir`, `app/folha/porta_export.py:818` | SEM JUIZ | SEM CASO; essa composição é conferência entre caminhos |
| 36 | Cartão × TXT acusa diferença de `dias_falta` somente quando módulo da diferença ultrapassa `0.5`. | `app/relatorios/cartao_pela_celula.py::divergencia:265` | Dicionário de divergências | Medição do portão, `app/folha/porta_export.py:667` | SEM JUIZ | SEM CASO |
| 37 | Corte vem da empresa, vazio usa 21, limitado a `[2,28]`; janela vai do corte anterior ao corte−1 atual. | `app/ponto/janelas.py::corte_da_empresa:50`; `::periodo_apuracao:15`; `::janela_fechamento:57` | Cadastro por `app/colaboradores/views.py:499`, `:523`, `:538` | `datas_falta`, `app/folha/export.py:185` | SEM JUIZ para essas funções; F5 registra `janela_atual`, `app/core/juizes.py:304` | SEM CASO |
| 38 | Seleciona ausência do colaborador, tipo solicitado, **aprovada**, iniciada até o fim e com interseção pelo fim/range aberto. | `app/folha/export.py::ausencias_na_janela:87` | Queryset | Expansão, `app/folha/export.py:160` | SEM JUIZ | SEM CASO que distinga numericamente os estados excluídos |
| 39 | Tipos de falta são os cujo efeito vigente é `DESCONTA`; não há filtro literal exclusivo `tipo='falta'`. | `app/ponto/catalogo/ausencias.py::descontam_vigentes:331`; `app/folha/export.py::datas_falta_janela:180` | Conjunto transitório | Seleção, `app/folha/export.py:181` | SEM JUIZ para o seletor; A1 registra `efeito_vigente`, `app/core/juizes.py:252` | SEM CASO |
| 40 | Catálogo do banco inclui somente `TipoAusencia.ativo=True`. | `app/ponto/catalogo/ausencias.py::_do_banco:249` | Dicionário transitório; cadastro por `app/ponto/services/ausencia.py:1306` | `vigente`, `app/ponto/catalogo/ausencias.py:278` | SEM JUIZ | SEM CASO |
| 41 | Banco vazio ou exceção → semente `CATALOGO`. Na semente, falta desconta e férias abonam. | `app/ponto/catalogo/ausencias.py::_do_banco:252`; `::vigente:278`; semente `:88`, `:101` | Cache do processo | Seletores de tipos, `app/ponto/catalogo/ausencias.py:331` | SEM JUIZ | SEM CASO |
| 42 | Cache preenchido e idade menor que 60 segundos → reutiliza catálogo anterior. | `app/ponto/catalogo/ausencias.py::vigente:275` | `_CACHE`, `app/ponto/catalogo/ausencias.py:279` | Seletores de tipos, `app/ponto/catalogo/ausencias.py:331` | SEM JUIZ | SEM CASO |
| 43 | `data_fim` explícita determina o fim da expansão. | `app/ponto/turnos.py::dias_da_ausencia:2791` | Lista de datas | `datas_no_periodo`, `app/folha/export.py:70` | SEM JUIZ; A2 registra outra função, `app/core/juizes.py:253` | SEM CASO |
| 44 | Sem fim: tipo aberto expande até fim da janela; demais ficam no dia inicial. | `app/ponto/turnos.py::dias_da_ausencia:2793` | Lista de datas | `datas_falta`, `app/folha/export.py:185` | SEM JUIZ | SEM CASO |
| 45 | Expansão usa `max(data, ini)`, `min(fim, fim_janela)` e inclui cada dia até o limite. | `app/ponto/turnos.py::dias_da_ausencia:2797` | Lista de datas | Contagem do bloco, `app/folha/export.py:218` | SEM JUIZ | SEM CASO |
| 46 | Criação recusa tipo ausente de `codigos_validos()`. | `app/ponto/services/ausencia.py::criar_ausencia:194` | Pode impedir `Ausencia` | Construção, `app/ponto/services/ausencia.py:341` | SEM JUIZ para esse corte | SEM CASO |
| 47 | `indeterminado` ou tipo em `TIPOS_EM_ABERTO` → fim nulo. | `app/ponto/services/ausencia.py::derivar_data_fim:70` | `Ausencia.data_fim`, criação `:343`, edição `:500` | Expansão, `app/ponto/turnos.py:2791` | SEM JUIZ; A12 registra `fim_de`, `app/core/juizes.py:273` | SEM CASO |
| 48 | Não aberto com `dias_corridos > 1` → início + dias−1; demais → fim nulo. | `app/ponto/services/ausencia.py::derivar_data_fim:76` | `Ausencia.data_fim` | Expansão, `app/ponto/turnos.py:2791` | SEM JUIZ | SEM CASO |
| 49 | Na criação, minutos positivos e tipo não parcial recusam quando `checar_parcial=True`. | `app/ponto/services/ausencia.py::criar_ausencia:206` | Pode impedir registro | Gravação, `app/ponto/services/ausencia.py:418` | SEM JUIZ | SEM CASO |
| 50 | Modo `devolver_mesmo_fato` devolve registro não rejeitado de mesmo colaborador/tipo/data. | `app/ponto/services/ausencia.py::criar_ausencia:246` | Evita nova linha | API, `app/api/views.py:1805` | SEM JUIZ | SEM CASO |
| 51 | Existindo rejeitada de mesmo fato, verifica conflito e reabre; conflito retorna ou recusa conforme modo. | `app/ponto/services/ausencia.py::criar_ausencia:254`; `:270`; `:277`; `:291` | Estado da linha existente | `_reabrir`, `app/ponto/services/ausencia.py:1120` | SEM JUIZ para a política de reabertura | SEM CASO |
| 52 | `retornar_mesmo_tipo` devolve mesmo tipo/data; inexistindo, permite prosseguir sem conflito de outro tipo. | `app/ponto/services/ausencia.py::criar_ausencia:294` | Evita linha ou permite nova | Materialização, `app/chamados/services/materializacao.py:527` | SEM JUIZ | SEM CASO |
| 53 | Modo `retornar` com sobreposição devolve a existente, sem criar. | `app/ponto/services/ausencia.py::criar_ausencia:311` | Evita nova linha | `garantir_ausencia`, `app/ponto/services/ausencia.py:461` | SEM JUIZ | SEM CASO |
| 54 | Sobreposição é absolvida somente se existente é `falta` e tipo novo abona; qualquer outra linha veta. | `app/ponto/precedencia.py::abona_sobre_falta:127`; `::linha_que_veta:145` | Booleano/linha de veto | Criação `app/ponto/services/ausencia.py:323`; veto `app/ponto/precedencia.py:461` | SEM JUIZ para esses corpos; A3 nomeia `veto_de_lancamento`, `app/core/juizes.py:254` | SEM CASO numérico |
| 55 | Veto final de precedência só é consultado quando `se_existir == 'veto'`. | `app/ponto/services/ausencia.py::criar_ausencia:377` | Pode impedir registro | `veto_de_lancamento`, `app/ponto/services/ausencia.py:380` | SEM JUIZ para a escolha do modo | SEM CASO |
| 56 | Folga lançada válida recusa o lançamento quando sua faixa não atravessa a folga; só férias atravessam. | `app/ponto/precedencia.py::veto_de_lancamento:496`; `:514` | Pode impedir ausência | Criação, `app/ponto/services/ausencia.py:380` | **A3**, `app/core/juizes.py:254` | SEM CASO numérico |
| 57 | Folga lançada existente e novo tipo literal `folga` também recusam. | `app/ponto/precedencia.py::veto_de_lancamento:511` | Pode impedir registro | Criação, `app/ponto/services/ausencia.py:380` | **A3**, `app/core/juizes.py:254` | SEM CASO |
| 58 | Para tipo literal `falta`, vínculo existente com `eh_dia_trabalho(d) is False` recusa; `None` não satisfaz a recusa. | `app/ponto/precedencia.py::veto_de_lancamento:536` | Pode impedir falta | Criação, `app/ponto/services/ausencia.py:380` | **A3**, `app/core/juizes.py:254` | **SEM CASO numérico.** L-089 com recusa/aceitação: `app/ponto/tests/test_o198_falta_em_dia_sem_jornada.py::FaltaEmDiaSemJornadaTest::test_falta_em_dia_sem_jornada_pela_CELULA_e_recusada:100`. |
| 59 | Constraint impede mesmo colaborador/tipo/data entre linhas cujo status não seja rejeitado. | `app/ponto/models.py::Ausencia.Meta:827` | Restrição sobre `Ausencia` | Tradução da colisão, `app/ponto/services/ausencia.py:421` | SEM JUIZ | SEM CASO |
| 60 | Fim anterior ao início levanta validação no `save`. | `app/ponto/models.py::Ausencia.save:894` | Pode impedir gravação | Criação/edição/transição, `app/ponto/services/ausencia.py:418`, `:509`, `:836` | SEM JUIZ | SEM CASO |
| 61 | Sem fim, dias > 1 e tipo não aberto → modelo deriva fim; fim existente deriva `dias_corridos`. | `app/ponto/models.py::Ausencia.save:899`; `:902` | `data_fim`, `dias_corridos` | Expansão, `app/ponto/turnos.py:2791` | SEM JUIZ para o modelo | SEM CASO pelo número de datas |
| 62 | Documento presente ou férias → aguarda decisão; tipo que exige documento sem anexo → aguarda documento; demais → aguarda decisão. | `app/ponto/services/ausencia.py::estado_inicial:792` | Estado inicial, `app/ponto/services/ausencia.py:344` | Seleção de aprovadas, `app/folha/export.py:88` | **A4**, `app/core/juizes.py:255` | SEM CASO numérico |
| 63 | Argumento `status` não vazio substitui o estado calculado; `aprovada` recebe carimbos. | `app/ponto/services/ausencia.py::criar_ausencia:344`; `:347` | `Ausencia.status`, carimbos | Seleção, `app/folha/export.py:88` | SEM JUIZ | SEM CASO |
| 64 | Mesmo estado retorna; destino fora de `_TRANSICOES[atual]` recusa; permitido grava estado. | `app/ponto/services/ausencia.py::mudar_estado:819`; tabela `:768` | `status`, `data_fim`, `minutos`, `dias_corridos`, por `:836` | Seleção, `app/folha/export.py:88` | SEM JUIZ; A5 registra somente `ABERTAS`, `app/core/juizes.py:256` | SEM CASO numérico |
| 65 | Porta DP aceita apenas vereditos `aprovada` ou `rejeitada` e delega a transição. | `app/ponto/services/ausencia.py::decidir:905` | Via `mudar_estado:912`; demais campos em `:928` | Export, `app/folha/export.py:88` | SEM JUIZ | SEM CASO numérico |
| 66 | Edição recusa qualquer outra ausência sobreposta, excluindo apenas a própria PK. | `app/ponto/services/ausencia.py::editar_ausencia_registro:487` | Pode impedir edição | Gravação, `app/ponto/services/ausencia.py:498` | SEM JUIZ para esse corte da edição | SEM CASO |
| 67 | Edição recusa minutos positivos se tipo não estiver no conjunto parcial. | `app/ponto/services/ausencia.py::editar_ausencia_registro:480` | Pode impedir edição | Gravação, `app/ponto/services/ausencia.py:509` | SEM JUIZ | SEM CASO |
| 68 | Edição também recusa tipo ausente do catálogo válido. | `app/ponto/services/ausencia.py::editar_ausencia_registro:474` | Pode impedir mudança de tipo | `Ausencia.tipo`, `app/ponto/services/ausencia.py:498` | SEM JUIZ | SEM CASO |
| 69 | Reabertura recalcula destino pelo tipo e existência de documento; não recebe automaticamente

[truncado em 20000 de 36529 caracteres]

## 14/11 10:34 · sessao 8f443ecca58e · HJ-091 · sonda · origem=Code

Transcrição do dado raspado; os números abaixo não foram confirmados em produção. [nao verificado]

Universo: **3001 ausências de férias, 116 agendamentos, 2943 dias vivos**. [nao verificado]

1. **`ferias_ausencia_sem_agendamento` — aprovadas sem agendamento vivo cobrindo: 58; colaboradores: 53.** PKs literais: `4635, 4839, 4165, 4613, 4630, 4634, 4633, 4631, 4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618, 4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651, 4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662, 4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666, 4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645, 4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639, 4640, 4632`. [nao verificado]

2. **`ferias_dia_sem_ausencia` — dias de agendamento vivo sem ausência aprovada: 4; colaboradores: 2.** A saída fornece tuplas `(colaborador_id, data, [agendamento_id])`, sem PK de ausência: `(127, '2026-10-02', [114]), (344, '2026-11-05', [103]), (344, '2026-11-06', [103]), (344, '2026-11-07', [103])`. [nao verificado]

3. **`rejeitada_ou_cancelada_dentro_de_agendamento_vivo`: 3; colaboradores: 1.** PKs literais: `3977, 3995, 3985`. Todas constam como `rejeitada`, colaborador `344`, cobertura pelo agendamento `103`. [nao verificado]

4. **Backfill — sem marca `#N` na descrição: 58.** PKs literais: `4635, 4612, 4839, 4613, 4630, 4634, 4633, 4631, 4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618, 4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651, 4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662, 4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666, 4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645, 4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639, 4640, 4632`. A PK `4612` está rejeitada; as demais, aprovadas. [nao verificado]

5. **Backfill — `#N` não é agendamento vivo cobrindo o dia: 2.** PKs literais: `4165, 2634`, ambas aprovadas; marcas de agendamento `109` e `77`, respectivamente. [nao verificado]

**#3977 aparece na classe 3**, rejeitada, com marca `#103` e cobertura `[103]`. **#2634 aparece na classe 5**, aprovada, com marca `#77` e cobertura `[114]`. [nao verificado]

As classes específicas **“rejeitadas sem origem dentro de agendamento vivo”** e **“aprovadas sem origem dentro de agendamento vivo”** não têm contagens nem PKs apresentados como tais no recorte; seus resultados são **[nao verificado]**, não zero. [nao verificado]

No trecho fornecido: **mensagens ERRO: 0; Traceback: 0**. O encerramento literal é **`ERROS 0`**. [nao verificado]

## 14/11 10:34 · sessao 5409579ac854 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| sonda_hash | `f8fc49270edcade2` [nao verificado] |
| cpuset | `4-7` [nao verificado] |
| modo | LEITURA [nao verificado] |
| universo_admin_encerrado_colab_com_ferias | 845 [nao verificado] |
| com_dia_do_fato | 419 [nao verificado] |
| dia_coberto_por_ferias | 27 [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | 16 [nao verificado] |
| erros | 0 [nao verificado] |
| pks_veto | `[3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801]` [nao verificado] |
| Ocorrências literais de `ERRO` no dado anexo | 0 [nao verificado] |
| Ocorrências literais de `Traceback` no dado anexo | 0 [nao verificado] |

## 14/11 10:33 · sessao 3a83fe721204 · [nome]-ANDAM-JUNTO · verificador · origem=Code

O aval foi confrontado com a transcrição fornecida; leitura do arquivo literal em `logs/AVAIS-1313/ramos_andam_junto.txt`: **[nao verificado]**.

1. **PROCEDE: aceita tabelas de 8 e 10 colunas sem deslocamento.** O cabeçalho determina o esquema; uma linha de oito células sob cabeçalho de dez vai para “nao lidas”. Cabeçalhos de obras fora do esquema também provocam rejeição das linhas. (`bin/placar_grade.py:70`, `bin/placar_grade.py:75`, `bin/placar_grade.py:81`, `bin/placar_grade.py:84`)

   **PROCEDE: existem células semanticamente inválidas que entram na contagem.** `REGISTRADAxyz` passa porque o estado usa `startswith`; `lei`, `arquivo congelado` e `colisao com` passam sem identificar qual lei, arquivo ou obra. Número sem fonte também passa, embora o aval peça fonte. Já `fecha BO=abc` e `portao=talvez` são rejeitados. (`bin/placar_grade.py:86`, `bin/placar_grade.py:49`, `bin/placar_grade.py:54`, `bin/placar_grade.py:95`, `bin/placar_grade.py:98`)

2. **PROCEDE: a soma implementa a adição dos valores informados por estado.** O resultado publicado é EM VOO **2**, BRIEF PRONTO **1**, REGISTRADA **9**, demais estados **0**; **6 de 297** linhas medidas. Isso soma os números registrados; correspondência com BO distintos e ausência de sobreposição: **[nao verificado]**. (`bin/placar_grade.py:109`, `bin/placar_grade.py:113`, `app/docs/GRADE-MESTRA.md:26`)

   **PROCEDE: complemento não aumenta a contagem de obras.** Ele fica separado, participa da soma e aparece no ranking identificado como complemento. Totais, estados e baldes de obras usam somente `lidas`. (`bin/placar_grade.py:103`, `bin/placar_grade.py:133`, `bin/placar_grade.py:139`, `bin/placar_grade.py:223`, `bin/placar_grade.py:225`)

   **PROCEDE: `?` entra em N, mas não em k.** Isso vale para obras e complementos válidos de dez colunas; tabelas de oito colunas e linhas rejeitadas ficam fora desse denominador. (`bin/placar_grade.py:48`, `bin/placar_grade.py:109`, `bin/placar_grade.py:114`, `bin/placar_grade.py:116`)

3. **PROCEDE: há mordida nos asserts novos abaixo.** Cada mutante indicado altera o `.py` e derruba a respectiva verificação. (`bin/tests/test_placar_grade.sh:106`–`121`)

| Assert em `bin/tests/test_placar_grade.sh` | Mutante do `.py` que o derruba |
|---|---|
| `:106`, `--placar` termina com sucesso | Provocar exceção na seção nova: `bin/placar_grade.py:222`. |
| `:107`, quatro obras | Encaminhar complementos para `lidas`: `bin/placar_grade.py:103`. |
| `:108`, uma não lida | Desligar rejeição de portão: `bin/placar_grade.py:98`. |
| `:109`, inválida listada com motivo | Remover listagem das rejeitadas: `bin/placar_grade.py:230`. |
| `:111`, soma e denominador exatos | Excluir complementos da soma, ou converter `?` em zero: `bin/placar_grade.py:109`, `bin/placar_grade.py:48`. |
| `:112`, linha exata de FB-QUATRO | Imprimir apenas o número, perdendo a fonte: `bin/placar_grade.py:225`. |
| `:113`, ordem e exclusão das não medidas | Ordenar crescente: `bin/placar_grade.py:224`. |
| `:117`, conferência verde | Retornar erro no caminho válido: `bin/placar_grade.py:189`. |
| `:118`, soma no cabeçalho | Remover inclusão da soma: `bin/placar_grade.py:143`. |
| `:120`, fixture realmente mudou | **Não verifica o `.py`**; protege a transformação feita pelo `sed` em `:119`. |
| `:121`, alteração deixa cabeçalho vermelho | Desligar comparação do cabeçalho: `bin/placar_grade.py:183`. |

   **NAO PROCEDE: cobertura completa das rejeições.** Faltam casos com número inválido, zero, largura incompatível e estado inválido de complemento. Remover a rejeição de número inválido ou a conferência de largura passa pelas fixtures existentes; trocar `fb is False` por teste de falsidade também deixa o erro de rejeitar zero escapar. (`bin/tests/test_placar_grade.sh:83`–`104`; `bin/placar_grade.py:81`, `bin/placar_grade.py:95`)

   **NAO PROCEDE: FB-COMPL ausente deixando o assert vazio.** A linha existe; o `sed` a altera e o `cmp` acusa transformação sem efeito. Porém, retirar o sufixo `(compl.)` escapa: o `awk` verifica somente o identificador em `$2`. (`bin/tests/test_placar_grade.sh:98`, `bin/tests/test_placar_grade.sh:113`, `bin/tests/test_placar_grade.sh:119`, `bin/tests/test_placar_grade.sh:120`; `bin/placar_grade.py:225`)

4. **NAO PROCEDE quebra de formato nos consumidores identificados.** O conferidor usa marcadores, o selo espera a soma nova e a publicação transporta os documentos inteiros. (`bin/placar_grade.py:147`, `bin/placar_grade.py:183`, `bin/tests/test_placar_grade.sh:111`, `bin/tests/test_placar_grade.sh:118`)

| Consumidor/caminho | Compatibilidade |
|---|---|
| `relato.sh` | Gera pelo leitor atualizado, raspa e inclui GRADE/PLACAR na publicação: `bin/relato.sh:101`, `bin/relato.sh:119`, `bin/relato.sh:120`, `bin/relato.sh:137`. |
| Raspador | Lê texto inteiro e aplica máscara, sem interpretar colunas: `bin/raspar_publico.py:28`, `bin/raspar_publico.py:31`. |
| Régua/selos | A régua executa o selo por `test_*.sh`; ele confere também a GRADE viva: `bin/regua.sh:138`, `bin/tests/test_placar_grade.sh:124`. |
| `pos_push` | Opera o derivado de TICKETS: `bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:29`. |
| Hook Stop | Usa BACKLOG e RELATO: `bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:34`. |

   Outros leitores externos ou seleção automática da vaga pela tabela: **[nao verificado]**. O código examinado escreve o ranking; essa escrita não comprova um consumidor automático. (`bin/placar_grade.py:222`–`226`)

5. **PROCEDE o desvio de portão em DECLARADO-X-GRAVADO; os demais preenchimentos têm as ressalvas abaixo.** (`app/docs/GRADE-MESTRA.md:270`; `app/docs/RELATO.md:7`)

| Linha preenchida | Veredito |
|---|---|
| DECLARADO-X-GRAVADO | **PROCEDE erro:** registra colisão, omitindo o impedimento de lei pedido no aval. O RELATO confirma retirada da ordem até corte e cinco questões abertas. `app/docs/GRADE-MESTRA.md:270`; `app/docs/RELATO.md:7`, `app/docs/RELATO.md:24`. |
| BO-ADMIN-2710 | **NAO PROCEDE divergência:** `censo` corresponde à pendência de medidas, dono e destino. `app/docs/GRADE-MESTRA.md:135`. |
| RAIZ-DA-DISPUTA | **NAO PROCEDE categoria inválida:** identifica E3 e `chamados/models.py`. Interseção atual dos diffs: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:106`. |
| CONTRATO-DE-ENTRADA-DO-FIO | **NAO PROCEDE categoria inválida:** identifica E3 e `chamados/juizes.py`. Interseção atual dos diffs: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:685`. |
| FIO-E0-V4-E3-LEITORES | **PROCEDE inconsistência:** registra colisão consigo mesma; isso não demonstra arquivo comum com outra raia, critério de RAIAS. Portão substituto: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:117`; `CLAUDE.md:309`. |
| O232 | **PROCEDE falta de sustentação do “em voo”:** aponta JUSTIFICATIVA, que a própria GRADE registra INTERROMPIDA. Colisão atual com agente vivo: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:851`, `app/docs/GRADE-MESTRA.md:267`. |

Validação do leitor e mutantes feita em memória. Execução integral do selo shell: **[nao verificado]**; ele cria e remove arquivos temporários, contrariando a restrição desta auditoria. (`bin/tests/test_placar_grade.sh:17`, `bin/tests/test_placar_grade.sh:71`)

## 14/11 10:33 · sessao 9bd2d678f7ef · - · - · origem=Ronald (tmux)

| ID | col/dia na linha do BACKLOG | Obra na GRADE-MESTRA | Portão e prova |
|---|---|---|---|
| HJ-057 | nenhuma linha encontrada [nao verificado] | nenhuma [nao verificado] | censo [nao verificado] |
| HJ-058 | col=n/a; ocorrência 06/11, universo desde 30/09 — app/docs/BACKLOG.md:2510 | UI-ANEXO-AUSENCIA — app/docs/GRADE-MESTRA.md:1517; O30 — app/docs/GRADE-MESTRA.md:480 | lei: vale da F1 antes de F2/F3 — app/docs/BACKLOG.md:1311; rebuild com pillow_heif também pendente — app/docs/GRADE-MESTRA.md:1517 |
| HJ-058-MEDIA-AUSENCIAS | col/dia=n/a — app/docs/BACKLOG.md:2511 | nenhuma [nao verificado] | censo: origem antes de cura — app/docs/BACKLOG.md:2511 |
| HJ-066 | col/dia=n/a — app/docs/BACKLOG.md:2503 | nenhuma [nao verificado] | censo de arquivos/diff atual [nao verificado] |
| HJ-067 | apenas referência, sem col/dia — app/docs/BACKLOG.md:2500 | nenhuma vinculação inequívoca [nao verificado]; RAIZ-DA-DISPUTA existe em app/docs/GRADE-MESTRA.md:103 | censo: F0 antes de HJ-067 — app/docs/BACKLOG.md:2500; colisão atual [nao verificado] |
| HJ-068 | col/dia=n/a — app/docs/BACKLOG.md:2504 | nenhuma [nao verificado] | censo de arquivos/diff atual [nao verificado] |
| HJ-069 | apenas referência, sem col/dia — app/docs/BACKLOG.md:2492 | nenhuma [nao verificado]; UI-ESCALA-REQUADRO não aparece na GRADE [nao verificado] | censo: drawer antes; vaga exige ausência de arquivo comum — app/docs/BACKLOG.md:2492 |
| HJ-071 | col/dia=n/a — app/docs/BACKLOG.md:2505 | nenhuma [nao verificado] | censo: medir primeiro se hora_ini/hora_fim estão gravadas — app/docs/BACKLOG.md:2505 |
| HJ-073 | col/dia=n/a — app/docs/BACKLOG.md:2506 | nenhuma [nao verificado] | censo + F0 antes da cura — app/docs/BACKLOG.md:2506. O RELATO registra resultado do censo, sem prova de cura: app/docs/RELATO.md:532 |
| HJ-074 | col148 / 02/11 — app/docs/BACKLOG.md:2507 | FERIADO-ESCALA — app/docs/GRADE-MESTRA.md:122 | lei: FERIADO-USA-A-LEI-DE-1609 é dependência nomeada — app/docs/GRADE-MESTRA.md:122; já havia construção na linha app/docs/BACKLOG.md:728. Bloqueio atual ou “livre” [nao verificado] |
| HJ-075 | col1032 / dia=n/a; 13/11 08:52 é conferência do servidor — app/docs/BACKLOG.md:2508 | nenhuma [nao verificado] | censo: aguarda print para confirmar causa — app/docs/BACKLOG.md:2508 |
| HJ-075-EC-FIRST | col/dia=n/a — app/docs/BACKLOG.md:2509 | nenhuma [nao verificado] | censo de alcance/arquivos atual [nao verificado]; linha registra juiz duplicado de vínculo — app/docs/BACKLOG.md:2509 |
| HJ-090 | col/dia=n/a na linha própria — app/docs/BACKLOG.md:1874 | HJ-090 — app/docs/GRADE-MESTRA.md:263 | “livre” como bloqueio atual [nao verificado]; a cópia registra pouso/NO AR em app/docs/RELATO.md:8. Não classificar como ainda colidindo apenas pela GRADE antiga |
| HJ-091 | referência, col/dia=n/a — app/docs/BACKLOG.md:1874 | HJ-090, resolve também HJ-091 — app/docs/GRADE-MESTRA.md:263 | colisão com HJ-090 em ferias/models.py e ferias/services.py registrada em app/docs/RELATO.md:98; esse bloqueio é histórico, pois HJ-090 consta pousada em app/docs/RELATO.md:8. Portão atual: censo [nao verificado] |
| HJ-092 | col/dia=n/a; há cel128589 — app/docs/BACKLOG.md:741 | HJ-092 — app/docs/GRADE-MESTRA.md:123; TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO — app/docs/GRADE-MESTRA.md:464 | lei/decisão humana: classe BATIDA, candidatas ao admin — app/docs/BACKLOG.md:741; dependência decisão do admin — app/docs/GRADE-MESTRA.md:123 |
| HJ-093 | col1004 / 27/10 — app/docs/BACKLOG.md:743 | JUSTIFICATIVA-PAGA-O-PERIODO — app/docs/GRADE-MESTRA.md:264; HJ-093 — app/docs/GRADE-MESTRA.md:1371 | lei O168 para pergunta sem veredito — app/docs/BACKLOG.md:743; cobertura financeira exige DIFF/motor/folha — app/docs/GRADE-MESTRA.md:264. Colisão atual com E3 [nao verificado] |
| HJ-095 | referência, col/dia=n/a — app/docs/BACKLOG.md:2109 | CONTRATO-DE-ENTRADA-DO-FIO — app/docs/GRADE-MESTRA.md:682 | censo/portão de arquivo atual [nao verificado]; dependência documentada E3 e DESLIGADO/FERIADO — app/docs/BACKLOG.md:2109. Não converter dependência em colisão sem diff |

Todos os IDs HJ-057+ acima: **“nao medida pela sonda”**; cobertura deriva exclusivamente do anexo, portanto [nao verificado] no padrão arquivo:linha exigido.

Não encontrei na cópia a lista dos seis arquivos congelados da E3; qualquer portão “arquivo congelado” específico seria [nao verificado]. A regra disponível exige arquivo comum, não família/app: CLAUDE.md:309.

_5073 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
