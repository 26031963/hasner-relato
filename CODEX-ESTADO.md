# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 17:52** · publicado em **26/11/2026 17:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 17:52 · sessao 7d57a27180df · PAR-PELO-TURNO · leitura · origem=Code

# PAR-PELO-TURNO c1 v2 -- brief do construtor (base de65d64f4)

## CENSO

leitores:

- Teto e dia no juiz. `app/ponto/turnos.py:63::_teto_s_da_jornada` e `app/ponto/turnos.py:1434::parear_turnos` leem o cálculo do teto. `app/ponto/turnos.py:230::_cabe_na_jornada_da_vespera`, `app/ponto/turnos.py:1086::_fechar_aberto_com_saida_seguinte` e `app/ponto/turnos.py:1177::_fechar_aberto_na_pausa_sem_volta` leem a saída numérica. `app/ponto/turnos.py:298::_data_do_turno` e `app/ponto/turnos.py:302::_data_do_turno` leem a guarda da véspera. `app/ponto/turnos.py:318::_mk` lê o dia. `app/ponto/turnos.py:1501::parear_turnos` e `app/ponto/turnos.py:1502::parear_turnos` leem os pós-fechos.
- `parear_turnos`, 13 chamadas diretas de execução. `app/ponto/turnos.py:1535::turnos_de_batidas`, `app/ponto/turnos.py:2005::_turnos_do_colab_puro`, `app/ponto/turnos.py:2315::_turno_aberto_puro`, `app/ponto/motor_calculo_v2.py:530::turnos_via_autoridade`, `app/chamados/services/fila_por_causa.py:197::particionar`, `app/escala/services/meio_periodo.py:168::observado_meio_periodo`, `app/escala/services/semana_do_posto.py:120::_bateram`, `app/ponto/management/commands/reconciliar_fantasmas.py:99::_completude`, `app/ponto/services/diagnostico_escala.py:142::diagnostico_escala`, `app/ponto/services/lastro.py:268::julgar`, `app/ponto/services/piso_intervalo.py:48::pares_abaixo_do_piso`, `app/ponto/services/triagem_batida.py:178::triagem_bater_ponto`, `app/ponto/utils_turno_orfao.py:31::detectar_bloqueio_turno_orfao`.
- `turnos_de_batidas`, quatro chamadas. `app/ponto/motor_calculo_v2.py:909::_turnos_do_juiz`, `app/escala/services/propositor.py:117::_dias_batidos`, `app/escala/services/propositor.py:152::_horario_mediano`, `app/escala/services/propositor.py:522::propostas`.
- `_turnos_do_colab_puro`, três chamadas. `app/ponto/turnos.py:1928::turnos_do_colab`, `app/ponto/turnos.py:3125::CargaDaJanela.turnos`, `app/chamados/services/carga_pergunta_viva.py:182::turnos`.
- `turnos_do_colab`, 51 chamadas nos seis itens a seguir. API e chamados. `app/api/views.py:1447::api_espelho_v2`, `app/chamados/detector_anomalias.py:157::detectar_batida_fora_escala`, `app/chamados/juizes.py:447::_turno_tem_saida`, `app/chamados/juizes.py:2303::_turno_cruza_meia_noite`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145::classificar`, `app/chamados/models.py:1378::premissa_regularizada`, `app/chamados/services/disputa_emissao.py:722::_orfao_resolvido_cross_meianoite`, `app/chamados/services/materializacao.py:399::_achar_batida_origem`, `app/chamados/services/materializacao.py:513::turnos_sem_par`.
- Colaboradores e escala. `app/colaboradores/services/drawer.py:47::contexto_drawer`, `app/colaboradores/services/feriado_do_posto.py:473::_classificar_previa`, `app/escala/detector_proposta.py:65::medir_padrao`, `app/escala/regua_defesa.py:361::turnos_longos_suspeitos`, `app/escala/services/escala_auto.py:87::propostas_do_vinculo`, `app/escala/services/impacto_troca.py:18::_trabalhados`, `app/escala/servico_jornada.py:249::_turno_noturno_completo`, `app/escala/servico_jornada.py:403::classificar_falta`, `app/escala/utils.py:1627::montar_grade_prevista_periodo_por_turno`, `app/escala/utils.py:1952::montar_grade_prevista_periodo_por_turno`.
- Comandos de ponto. `app/ponto/management/commands/censo_fase_12x36.py:180::handle`, `app/ponto/management/commands/detectar_ausencias.py:337::handle`, `app/ponto/management/commands/detectar_par_relampago.py:56::pares_relampago`, `app/ponto/management/commands/dna_x_batida_real.py:69::handle`, `app/ponto/management/commands/e6_oraculo.py:159::InsumosDoDono.turnos`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88::handle`, `app/ponto/management/commands/marcar_abandono_provisorio.py:46::handle`, `app/ponto/management/commands/verificar_nucleo.py:29::handle`, `app/ponto/management/commands/vigia_plantio_overnight.py:38::batidas_plantadas_no_dia_errado`.
- Núcleo e serviços de ponto. `app/ponto/nucleo.py:47::recompute_turnos`, `app/ponto/services/bordas_realizado.py:66::bordas_do_realizado`, `app/ponto/services/esmeril_espelho.py:102::assinaturas_do_colab`, `app/ponto/services/espelho.py:628::montar_dias`, `app/ponto/services/passivo_s84.py:82::classificar`, `app/ponto/services/regularizacao.py:61::regularizar_turno_orfao`, `app/ponto/services/teto_na_lista.py:68::medir`, `app/ponto/services/triagem_batida.py:553::_autoresolve_f2_f4`, `app/ponto/services/vigia_pareador_celula.py:97::censo`.
- Leitura do turno. `app/ponto/turno_leitura.py:89::turno_invariante`, `app/ponto/turno_leitura.py:95::turno_invariante`, `app/ponto/turnos.py:780::realizado_do_dia`, `app/ponto/turnos.py:2112::dia_encerrado_motivo`, `app/ponto/turnos.py:2170::dias_encerrados`, `app/ponto/turnos.py:2201::par_completo_da_janela`, `app/ponto/turnos.py:2998::dia_das_batidas`, `app/ponto/turnos.py:3193::batidas_do_dia_turno`.
- Telas e relatórios. `app/ponto/views.py:396::turnos_abertos`, `app/ponto/views.py:1613::_abrir_justificativa_fluxo`, `app/ponto/views.py:3356::regularizar_retorno`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124::handle`, `app/relatorios/pdf_espelho.py:213::_coletar_dados_espelho`, `app/relatorios/pdf_espelho.py:451::_coletar_dados_espelho`.
- Lote de turnos. `app/colaboradores/services/calendario.py:252::contexto_calendario`, `app/ponto/status_dia.py:273::status_dos_dias_lavrados`, `app/ponto/turnos.py:3135::CargaDaJanela.dias_encerrados`, `app/ponto/turnos.py:3139::CargaDaJanela.dia_das_batidas`. As cargas de turno dos chamados e do juiz do dono também são lidas em `app/chamados/juizes.py:448::_turno_tem_saida`, `app/escala/servico_jornada.py:252::_turno_noturno_completo`, `app/escala/servico_jornada.py:404::classificar_falta` e `app/ponto/management/commands/e6_oraculo.py:185::InsumosDoDono.dono`.
- Dia por PK. `app/ponto/turnos.py:3004::dia_das_batidas` e `app/ponto/turnos.py:3140::CargaDaJanela.dia_das_batidas` leem o mesmo corpo. Chamadas do mapa em `app/api/views.py:1444::api_espelho_v2`, `app/api/views.py:2042::api_justificativas`, `app/core/services/painel_op.py:330::_d4_em_lote`, `app/ferias/management/commands/listar_ferias_com_batida.py:54::handle`, `app/ponto/management/commands/vigia_plantio_overnight.py:87::batidas_fora_do_turno_da_pergunta`, `app/ponto/services/espelho.py:465::montar_dias` e `app/ponto/turnos.py:3024::dia_do_turno_da_batida`.
- Dia de uma batida. `app/api/views.py:750::api_bater_ponto`, `app/api/views_core.py:744::api_bater_ponto`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160::classificar`, `app/chamados/services/disputa_emissao.py:76::_data_furo_da_grade`, `app/chamados/services/disputa_emissao.py:283::perguntar_entradas_pendentes`, `app/chamados/services/disputa_emissao.py:400::gerar_perguntas_disputa`, `app/chamados/services/fio.py:1822::_dia_do_fato`, `app/chamados/services/foto_e4b.py:283::foto_do_colaborador`, `app/escala/servico_jornada.py:509::resolver_data_evento`, `app/ponto/management/commands/lavrar_estoque_par_relampago.py:58::_classificar`, `app/ponto/services/par_relampago_na_lista.py:101::_origem_pelo_vinculo_na_data`, `app/ponto/services/par_relampago_na_lista.py:326::verbos_do_sinal`, `app/ponto/services/triagem_batida.py:729::ramo_registrar_es`.
- Batidas do dia e sobras. `app/chamados/models.py:1377::premissa_regularizada`, `app/chamados/services/disputa_emissao.py:831::_tem_base`, `app/escala/servico_jornada.py:531::procedencia_fina`, `app/ponto/services/flip_auto.py:104::classificar_pares` leem `batidas_do_dia_turno`. `app/colaboradores/services/calendario.py:612::contexto_calendario`, `app/ponto/services/espelho.py:631::montar_dias` e `app/relatorios/pdf_espelho.py:622::_coletar_dados_espelho` leem `datas_saida_orfa`.
- Apuração. `app/ponto/adaptador_motor.py:29::periodos_de_turnos` lê o turno, as pontas e o intervalo julgado. `app/ponto/motor_calculo_v2.py:557::_periodos_via_autoridade` lê a geometria e `app/ponto/motor_calculo_v2.py:558::_periodos_via_autoridade` entrega o trio a `_periodos_pelo_marco`. Os quatro leitores são `app/ponto/motor_calculo_v2.py:2861::montar_periodos`, `app/ponto/motor_calculo_v2.py:2991::montar_periodos`, `app/ponto/motor_calculo_v2.py:3329::montar_periodos` e `app/ponto/motor_calculo_v2.py:3649::montar_periodos`. O mapa por entrada passa por `app/ponto/motor_calculo_v2.py:916::_dia_do_turno` e é lido em `app/ponto/motor_calculo_v2.py:2461::calcular_mes`, `app/ponto/motor_calculo_v2.py:3038::calcular_mes` e `app/ponto/motor_calculo_v2.py:3442::calcular_mes`. A janela de HE já usa o dia do juiz para ancorar os marcos. `app/ponto/motor_calculo_v2.py:1664::_janela_do_dia`.
- C9. `app/ponto/turnos.py:178::_declara_separado_pelo_teto` lê o primeiro carimbo e `app/ponto/turnos.py:198::pares_separados_pelo_teto` e `app/ponto/turnos.py:203::pares_separados_pelo_teto` leem geometria e carimbo. `app/ponto/services/teto_na_lista.py:68::medir` lê os eventos. `app/ponto/management/commands/lavrar_teto_do_dia.py:28::handle` lê a medição e `app/ponto/services/teto_na_lista.py:123::ultimo_lavrado` lê a foto. A projeção passa por `app/escala/services/cadastro_realidade.py:434::_teto_do_dia`, `app/escala/services/cadastro_realidade.py:772::acao`, `app/escala/services/cadastro_realidade.py:1016::lista`, `app/escala/services/cadastro_realidade.py:1086::lista`, `app/escala/services/cadastro_realidade.py:1202::um_dono_por_dia` e `app/escala/services/cadastro_realidade.py:1249::dias_na_lista`. Tela e PDF passam por `app/escala/views.py:525::cadastro_x_realidade` e `app/escala/views.py:527::cadastro_x_realidade`. O HTML lê assinatura e detalhe em `app/templates/escala/cadastro_x_realidade.html:31`.
- HE. `app/ponto/services/espelho.py:734::montar_dias` lê as pontas e o relato do motor e `app/ponto/services/espelho.py:939::montar_dias` e `app/ponto/services/espelho.py:946::montar_dias` leem a projeção. Outros leitores de `he_fora_da_janela` são `app/ponto/services/he_pendente_lavrado.py:68::minutos_fora_do_dia`, `app/ponto/services/he_pendente_lavrado.py:138::apurar`, `app/folha/porta_export.py:190::pendencias_he_da_janela`, `app/ponto/views.py:180::espelho_ponto`, `app/relatorios/pdf_espelho.py:932::_build_story` e `app/templates/ponto/espelho.html:189`. Leem o retrato `app/ponto/views.py:820::gestao_he_pdf`, `app/ponto/views.py:950::gestao_he`, `app/ponto/services/gestao_he.py:428::itens_sem_decisao`, `app/ponto/services/gestao_he.py:493::pendentes_na_central`, `app/ponto/services/he_pendente_lavrado.py:249::fotos_paradas`, `app/ponto/services/he_pendente_lavrado.py:254::fotos_paradas` e `app/ponto/management/commands/recusar_ponta_pequena.py:66::handle`. Leem a decisão vigente `app/ponto/services/gestao_he.py:93::estado_por_dia`, `app/ponto/services/gestao_he.py:123::_decisoes`, `app/ponto/management/commands/lavrar_he_pendente.py:48::handle`, `app/ponto/portas/he.py:148::_vigente` e `app/ponto/motor_calculo_v2.py:1521::_decisoes_he`.
- Censo de execução por chamada direta e leitores das projeções identificadas, incluindo comandos. Referências conferidas na cópia indicada. Identidade Git de HEAD, despacho por reflexão, consumidores externos e execução em produção [nao verificado].

escritores:

- `app/ponto/turnos.py:82::_teto_da_jornada` produz `previsto + 4 h` e a indicação de teto do dia. `app/ponto/turnos.py:1433::parear_turnos` fornece o envelope de 26 h ao fecho principal. `app/ponto/turnos.py:180::_declara_separado_pelo_teto` escreve o carimbo transitório de C9, hoje chamado por `app/ponto/turnos.py:1452::parear_turnos`.
- `app/ponto/turnos.py:285::_data_do_turno` determina o dia e `app/ponto/turnos.py:323::_mk` escreve o `Turno`. `app/ponto/turnos.py:1096::_fechar_aberto_com_saida_seguinte` e `app/ponto/turnos.py:1186::_fechar_aberto_na_pausa_sem_volta` completam a geometria por `_replace` e carregam o `data_turno` recebido. `app/ponto/turnos.py:3042::_dia_das_batidas_puro` publica o mapa por PK e `app/ponto/turnos.py:3044::_dia_das_batidas_puro` preenche o dia das marcas sem turno que as reivindique.
- `app/ponto/turnos.py:210::pares_separados_pelo_teto` produz dia, pontas, marcos, teto e vão da C9. `app/ponto/services/teto_na_lista.py:78::medir` escreve o item com a competência daquele dia e o dono de `app/ponto/management/commands/e6_oraculo.py:182::InsumosDoDono.dono`. `app/ponto/services/teto_na_lista.py:96::medir` produz totais e rótulo. `app/ponto/services/teto_na_lista.py:113::lavrar` escreve `MetricaSnapshot`. `app/escala/services/cadastro_realidade.py:105` declara a descrição C9, `app/escala/services/cadastro_realidade.py:435::_teto_do_dia` produz a assinatura e `app/escala/services/cadastro_realidade.py:1233::um_dono_por_dia` propaga o dono julgado quando as assinaturas divergem.
- Fronteiras de entrada. `app/ponto/registro_batida.py:118::registrar_batida` e `app/ponto/registro_batida.py:255::anotar` escrevem a batida e suas anotações. `app/escala/alimentacao.py:228::vinculo_do_dia` e `app/escala/utils.py:84::marcos_dna_periodo` fornecem vínculo e DNA. `app/ponto/turnos.py:1565::papel_por_minuto_da_ata` produz E/S/X/Xi. `app/ponto/turnos.py:344::_marca_ecos_flush`, `app/ponto/turnos.py:415::_marca_intra_duracao` e `app/ponto/turnos.py:1315::parear_turnos` escrevem carimbos de trabalho em memória. A apuração seleciona batidas vivas em `app/ponto/turnos.py:1868::turnos_do_colab` e `app/ponto/turnos.py:2941::batidas_apuraveis`.
- Fronteiras de saída. `app/ponto/services/espelho.py:790::montar_dias` escreve `he_fora_da_janela`. `app/ponto/services/he_pendente_lavrado.py:138::apurar` monta a pendência e `app/ponto/services/he_pendente_lavrado.py:183::lavrar` grava o retrato. `app/ponto/portas/he.py:44::_gravar` e `app/ponto/portas/he.py:394::_marcar_revogada` escrevem decisão e revogação. `app/ponto/services/dia_pago.py:360::lavrar` e `app/ponto/services/fechamento.py:1168::recalcular_fechamento_mes` são escritores do gravado financeiro.

decisoes:

- **Lei 1:** a órfã de madrugada absorvida pela jornada da véspera herda o **dia da véspera**, inclusive na borda de competência. A jornada pertence ao dia de início, L-085. Fecho e dia saem do **mesmo juiz**. `app/docs/PROMPTS.md:3025`, `app/docs/RELATO.md:170`, `app/docs/LEIS.md:117`.
- **V6, padrão juiz único.** Antecipação e prorrogação da mesma jornada preservam o intervalo efetivo e seguem para a apuração vigente. `previsto + 4 h` vira motivo de C9 quando seria o único corte de um par válido. O limite absoluto de fecho é 26 h. A classificação de pausa continua pela classe, Base 3 h, Partido 0 e Plantão 8 h, com as duas luzes `Xi` da ata fora do teto da pausa inferida. A janela de HE e a Gestão existentes classificam e publicam a pendência. `app/docs/PROMPTS.md:2852`, `app/ponto/motor_calculo_v2.py:755`, `app/ponto/motor_calculo_v2.py:2976`, `app/ponto/motor_calculo_v2.py:3629`, `app/ponto/turnos.py:1572`, `app/docs/LEIS.md:115`.
- **Fora desta obra.** Cadastro de duração da V2, alerta de interjornada, confirmação por ocorrência, porta nova, alterações nas batidas originais ou retratadas, reclassificação geral do dia civil, caso col673, desempate de PK entre dois turnos na troca de vínculo, autorização automática de HE, crédito duplicado e qualquer escrita financeira de competência exportada. A guarda de absorção de pausa continua em seu contrato próprio. `app/docs/PROMPTS.md:2823`, `app/docs/PROMPTS.md:2852`, `app/docs/BACKLOG.md:1864`, `app/docs/BACKLOG.md:2029`, `app/docs/RELATO.md:18`, `app/ponto/turnos.py:1349`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`.

perguntas de lei: 0

## ARQUIVOS

Lista fechada para o construtor. A escolha inclui a descrição pública do comando C9 e autoriza, nos testes legados nomeados, somente a substituição explícita de contratos superados pela V6 ou a preservação do controle com uma fixture que realmente o exerça. `app/docs/PROMPTS.md:2852`, `app/docs/LEIS.md:136`, `app/ponto/management/commands/lavrar_teto_do_dia.py:20`.

- `app/ponto/turnos.py` — juiz do fecho e do dia, guarda de 26 h, carimbo do par válido e leitura de C9. Os três pontos de fecho e a atribuição de véspera entram na prova. `app/ponto/turnos.py:216`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1170`, `app/ponto/turnos.py:1430`.
- `app/ponto/services/teto_na_lista.py` — consumir o evento do juiz e corrigir a descrição da lavra. Preservar chave, dono, horários, vão, teto e competência derivada de `data_turno`. `app/ponto/services/teto_na_lista.py:25`, `app/ponto/services/teto_na_lista.py:68`, `app/ponto/services/teto_na_lista.py:100`.
- `app/escala/services/cadastro_realidade.py` — adequar texto e detalhe C9 para fotos antigas e novas. O texto comum é que o vão real ultrapassou o teto do dia e coube no envelope de 26 h. `app/escala/services/cadastro_realidade.py:105`, `app/escala/services/cadastro_realidade.py:430`.
- `app/ponto/management/commands/lavrar_teto_do_dia.py` — adequar docstring e `help` da C9. A execução segue a medição e a lavra existentes. `app/ponto/management/commands/lavrar_teto_do_dia.py:2`, `app/ponto/management/commands/lavrar_teto_do_dia.py:20`, `app/ponto/management/commands/lavrar_teto_do_dia.py:26`.
- `app/ponto/tests/test_par_pelo_turno.py` — arquivo novo proposto para os REDs, os controles e a integração da matriz. `app/docs/LEIS.md:136`, `app/docs/BACKLOG.md:2027`.
- `app/ponto/tests/test_col129_par_separado_pelo_teto.py` — a asserção de dois turnos de col129 contradiz literalmente a V6. Preservar o gabarito de horário, dia, vão, teto, contagem e identidade por PK. No caso F, uma cópia condicionada a `t.aberto` perde a mordida quando o turno fecha. Preservar a contraprova da absorção do caso G. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:65`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:106`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`.
- `app/ponto/tests/test_h_portao_teto_do_dia.py`, `app/ponto/tests/test_o280_bateria_da_lei.py` e `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py` — revisar somente os contratos em que a recusa dependia do teto diário antigo. Col879 requer prova do papel das batidas da própria fixture. O pino A6 exige zero por `1.020 > 960`. O selo de posição recusa a candidata pela guarda antiga. Conservar as recusas por jornada distinta, falta de prova, pausa em curso, degenerado e envelope absoluto. `app/ponto/tests/test_h_portao_teto_do_dia.py:32`, `app/ponto/tests/test_o280_bateria_da_lei.py:2226`, `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py:177`, `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py:286`.
- `app/ponto/tests/test_col129_teto_vira_lista.py` — preservar contagem, idempotência, minutos e porta pelo dono. O legado que exige `BATIDA` por causa do furo precisa de um furo legítimo para continuar provando essa porta. No par recuperado, a referência é o resultado do juiz do dono. `app/ponto/tests/test_col129_teto_vira_lista.py:72`, `app/ponto/tests/test_col129_teto_vira_lista.py:155`, `app/ponto/tests/test_col129_teto_vira_lista.py:209`, `app/ponto/services/teto_na_lista.py:86`.

Obras e correções do mapa da base.

- **DIA-DA-BATIDA-JUIZ-UNICO F4c/F4d** compartilha `app/ponto/turnos.py` conforme o escopo informado. O ponto de encontro é a alimentação de `turnos_do_colab`, a resposta na casca e em lote e o dia por PK em janelas sobrepostas. A F4b está registrada e a F4c consta aguardando o portão de carga. Diff exato da F4c/F4d e estado da F4d [nao verificado]. Conferir a composição na árvore candidata com o mesmo PK no dia 20 pela casca, pelo lote e pela consulta individual. `app/docs/RELATO.md:18`, `app/docs/RELATO.md:34`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:2998`, `app/ponto/turnos.py:3024`, `app/ponto/turnos.py:3140`.
- **E1-PREVISAO-24H** cruza `app/escala/services/cadastro_realidade.py` no agregador da lista. A cópia E1a acrescenta a assinatura de previsão inválida e disputa o código C13 já

[truncado em 20000 de 38663 caracteres]

## 26/11 17:50 · sessao 227df873e3ce · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3-CONSTRUIR c12.** O RED novo passou na cópia da c11 (`rc 0`) e falhou com o mutante (`rc 1`). A falha ocorreu na asserção que exige que só o arquivo anterior permaneça. O mutante deixou o arquivo novo no storage simulado. `app/ponto/tests/test_atestado_f3_contrato.py:459,485` · `app/ponto/tests/mutantes_atestado_f3.py:421,443`

### Testes [nao verificado]

- `test_RED_reabrir_compensa_apos_sair_do_atomic_local` simula `False → True → False`, provoca um `RuntimeError` após o upload e confere a propagação da mesma exceção, a exclusão do arquivo novo, a preservação do anterior e o registro de `anexo_apagado=True` como único aviso. `app/ponto/tests/test_atestado_f3_contrato.py:459–490`
- `green_c11` passou nos 12 testes do seletor local. A certificação do Code e do Opus final está [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:419–420`

O diff contém só os dois arquivos de teste, com **zero `except` novos**. O mutante lê `in_atomic_block` na entrada do `with` e reutiliza o valor nos dois ramos. A substituição de `ausencia.py` ocorre em memória. `app/ponto/tests/test_atestado_f3_contrato.py:459` · `app/ponto/tests/mutantes_atestado_f3.py:309,378,443`

No censo, `lote_atestados` chama `pdf_bytes` no ramo `pdf`. A rota de PDF aponta para essa view. `app/ponto/views_atestado.py:99,135–137` · `app/relatorios/urls.py:17`

### Comandos do par green e mutante

A partir de `app/`, o primeiro comando deve sair com `0` e o segundo com `1`. `app/ponto/tests/mutantes_atestado_f3.py:421,443,456`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 green_c12
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 c12_reabrir_le_atomic_dentro_do_with
```

### Diff

```diff
diff --git a/app/ponto/tests/mutantes_atestado_f3.py b/app/ponto/tests/mutantes_atestado_f3.py
--- a/app/ponto/tests/mutantes_atestado_f3.py
+++ b/app/ponto/tests/mutantes_atestado_f3.py
@@ -62,0 +63 @@
+SAIDA_ATOMIC_LOCAL = PORTA + 'test_RED_reabrir_compensa_apos_sair_do_atomic_local'
@@ -307,0 +309,14 @@
+def mutar_leitura_do_atomic(fonte):
+    entrada = '        with transaction.atomic(using=banco) if dados_atestado else nullcontext():\n'
+    compensacao = 'if anexo_gravado and not conexao.in_atomic_block:'
+    for antes, depois in (
+        (entrada, entrada + '            em_atomic = conexao.in_atomic_block\n'),
+        (compensacao, compensacao.replace('conexao.in_atomic_block', 'em_atomic')),
+        (CONDICAO_AVISO_ORFAO, CONDICAO_AVISO_ORFAO.replace('conexao.in_atomic_block', 'em_atomic')),
+    ):
+        if fonte.count(antes) != 1:
+            raise SystemExit(f'Seletor deve aparecer uma vez em {AUSENCIA}: {antes.strip()}')
+        fonte = fonte.replace(antes, depois, 1)
+    return fonte
+
+
@@ -389 +404 @@
-             'green_c11',
+             'green_c11', 'green_c12',
@@ -391 +406 @@
-             'c8_try_dentro_do_atomic')
+             'c8_try_dentro_do_atomic', 'c12_reabrir_le_atomic_dentro_do_with')
@@ -405,0 +421,2 @@
+    elif nome == 'green_c12':
+        fontes, testes = {}, (SAIDA_ATOMIC_LOCAL,) [nao verificado]
@@ -425,0 +443,3 @@
+    elif nome == 'c12_reabrir_le_atomic_dentro_do_with':
+        caminho = APP / AUSENCIA
+        fontes, testes = {caminho: mutar_leitura_do_atomic(caminho.read_text(encoding='utf-8'))}, (SAIDA_ATOMIC_LOCAL,) [nao verificado]
diff --git a/app/ponto/tests/test_atestado_f3_contrato.py b/app/ponto/tests/test_atestado_f3_contrato.py
--- a/app/ponto/tests/test_atestado_f3_contrato.py
+++ b/app/ponto/tests/test_atestado_f3_contrato.py
@@ -458,0 +459,33 @@
+    def test_RED_reabrir_compensa_apos_sair_do_atomic_local(self):
+        self._preparar_storage_da_reabertura()
+        falha = RuntimeError('falha após gravar o arquivo no atomic local')
+
+        @contextmanager
+        def atomic(**kwargs):
+            self.conexao.in_atomic_block = True
+            try:
+                yield
+            finally:
+                self.referencias[self.registro.pk] = self.nome_antigo
+                self.conexao.in_atomic_block = False
+
+        def falhar_apos_upload(*args, **kwargs):
+            self.assertTrue(self.conexao.in_atomic_block)
+            self.assertIn(self.nome_novo, self.arquivos)
+            raise falha
+
+        with patch.object(self.transacao, 'atomic', atomic), \
+                patch.object(ausencia, 'mudar_estado', side_effect=falhar_apos_upload), \
+                self.assertLogs(ausencia.logger, level='WARNING') as logs, \
+                self.assertRaises(RuntimeError) as erro:
+            ausencia._reabrir(self.registro, self.documento, dados_atestado=CAMPOS)
+
+        self.assertIs(erro.exception, falha)
+        self.assertFalse(self.conexao.in_atomic_block)
+        self.assertEqual(self.arquivos, {self.nome_antigo})
+        self.documento.storage.delete.assert_called_once_with(name=self.nome_novo)
+        self.assertEqual(logs.output, [
+            f'WARNING:ponto:falha na reabertura: ausencia={self.registro.pk} '
+            f'nome={self.nome_novo} anexo_apagado=True',
+        ])
+
```

## 26/11 17:49 · sessao 6385f2c074c3 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

A c2h ficou na cópia com a bateria de saída, os mutantes em memória e as duas entradas da catraca atualizadas. O GREEN completo ficou bloqueado por dois pontos da base: o cálculo direto do Partido devolve minutos brutos com pausa interna, e a catraca integral ainda tem **13 registros de linha divergentes**, após corrigir os dois pedidos. [Partido](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:2997) · [Catraca](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:567)

| Item | Entrega | RED | GREEN local |
|---|---|---|---|
| 1. Catraca | `[530]` → `[531]` nas duas entradas pedidas. [Pareadores](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:261) · [Derivações](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:351) | O teste integral falhou por asserção na base. | As duas entradas conferem com o AST. A catraca integral ainda acusa 2 registros em `pareadores` e 11 em `derivacoes_de_dia`. | [nao verificado]
| 2. Bateria | Oito casos com valores fixados pela regra para 12x36 e Comercial, mais dois controles do Partido com intervalo entre períodos. [Grade](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:32) · [Controles](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:123) | `deed2fe72` deu 8 falhas por asserção. | 18 subcasos passaram em memória. O diagnóstico da grade com pausa interna no Partido deu 8 falhas. |
| 3. Selos P2/P3 | Docstrings registram os limites de `vars()`, `copy` com `setattr` e apelidos de `max`, e apontam para a bateria. [P2](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2_base_da_he.py:627) · [P3](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_contract_juiz_batida.py:377) | Os mutantes P2/P3 falharam na bateria. | 13 testes dos selos passaram em memória. | [nao verificado]
| 4. Commit | Corpo abaixo. A proibição de `git commit` prevaleceu. | — | SHA de commit novo [nao verificado]. | [nao verificado]

### Comandos e saídas

Medição da linha pedida, a partir de `app/`. Resultado `pareadores [531]` e `derivacoes_de_dia [531]`. [Censo AST](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/medida_turno_do_par.py:448) · [Chamada medida](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:531)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo
medida = censo('.')
for grupo in ('pareadores', 'derivacoes_de_dia'):
    print(grupo, next(i['linhas'] for i in medida[grupo]['itens'] if
                      i['chave'] == 'ponto/motor_calculo_v2.py::turnos_via_autoridade'
                      and i.get('sitio') == 'porta_parear_turnos'))
PY
```

Para cada linha abaixo, execute o comando indicado após `export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.base`. As saídas foram medidas localmente pela função `rodar`, com `unittest` e dublês de infraestrutura em memória porque `django.test` estava indisponível. A suíte Django do Code e o Opus final ficam [nao verificado]. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/ramo2b_c2h_mutantes.py:57) [nao verificado]

| Comando | Saída local |
|---|---|
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes red_deed2fe72` | rc 1, 8 asserções. No cruzamento, o oráculo devolveu `(700, 0, 60, 760)` e a regra esperava `(700, 20, 40, 760)`. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes green` | rc 0, 2 testes, 18 subcasos. | [nao verificado]
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes sem_ponta` | rc 1, 8 asserções. Reproduziu a perda dos 20 min de gozo. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes copy_setattr` | rc 1, 8 asserções. Base **1**, esperada **700** no cruzamento. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes teto_max` | rc 1, 16 asserções. Trabalhado **800**, esperado **760** no cruzamento. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes inclui_cauda` | rc 1, 5 asserções. Base **760**, esperada **680** na pausa anterior ao marco. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes perde_fim_no_marco` | rc 1, 2 asserções. Gozo **0**, esperado **20** na pausa que termina no marco. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes partido_interno` | rc 1, 8 asserções. No cruzamento, o Partido devolveu `(720, 0, 0, 800)` e a regra esperava `(700, 20, 40, 760)`. |

Todas as variantes registraram zero erros de execução. A tupla segue a ordem `(base, gozo, intra, trabalhado)`. A contagem AST deu **0 `ExceptHandler` novos** nos cinco arquivos do delta. [Asserções da bateria](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:94) · [Injeções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/ramo2b_c2h_mutantes.py:23)

```text
$ git -C .. rev-parse HEAD
c193310ebb5ccd111ec99d59af06e144461224ca
$ git -C .. diff --exit-code deed2fe72 -- app/ponto/motor_calculo_v2.py
```

O diff do motor saiu vazio, rc 0. [Porta da tripla](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:1830)

### Corpo preparado para o commit [nao verificado]

```text
RAMO2-B c2h: mede a saida da base da HE e registra a linha da catraca

OBRA: RAMO2-B-EM-TURNO-QUE-CRUZA
BASE: c193310ebb5ccd111ec99d59af06e144461224ca

Regra: RAMO2-B-C2-RESPOSTA-PAUSA-2611 (L-032, L-031, L-097).
A bateria fixa base, gozo, intra e trabalhado pela geometria do turno
e confere a tripla None nos periodos sem cauda.
Catraca de linhas: porta_parear_turnos na linha 531, em dois registros.
Selos AST P2/P3: limites documentados e mutantes medidos na saida.

LEI-AKITA:
origem=ponto/calculador/alimentacao.py:173-178
testemunha=ponto/calculador/nucleo.py:146-153, segmentos e batidas
RED=ponto/tests/test_ramo2b_c2h_saida.py:115, com alimentacao de deed2fe72
quem-mais-le=ponto/calculador/alimentacao.py:106-117,168-173,469-475,539-554
  (rubricas_por_dia -> ponto/calculador/regras.py::do_dia:584-616,767-789);
  ponto/services/fechamento.py:1291-1300;
  ponto/management/commands/diff_calculador.py:414-424;
  ponto/calculador/periodo.py:745-759; folha/porta_export.py:339-358
juizes novos=0
padrao=juiz unico

fora do marco: PAUSA-NA-BORDA-DO-PAR (fatia propria)

Pendencias medidas: 13 registros de linha herdados na catraca integral
e 8 contraexemplos da grade de pausa interna no calcular_periodo do Partido.
```

## 26/11 17:47 · sessao 6845351dc60f · - · - · origem=Ronald (tmux)

Conferi a matriz na cópia e reproduzi em memória os caminhos puros do pareador com as dependências de fronteira simuladas. Encaminhei os gabaritos e as citações ao agente principal.

- A borda sintética 20/10 16:00 → 21/10 05:00, DNA 18:00–00:30, dá 780 min contra 630. Na base há dois turnos, a saída cai dia 21 e existe um C9 no dia 20. Pela Lei 1, o esperado é um par e um C9 no dia 20, ambos os PKs na competência 10 e zero na 11. `app/ponto/turnos.py:1295`, `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:3037`, `app/ponto/janelas.py:24`, `app/docs/PROMPTS.md:3025`.
- As duas rotas de pós-fecho têm fixtures independentes que ficam abertas na base. A de posição leva pausa de 60 min e vão de 780 min contra 732. A de pausa sem volta comprovada leva 795 contra 780, com controle de pausa ainda em curso. `app/ponto/turnos.py:991`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1170`.
- O teto absoluto precisa de RED próprio. A base fecha 27 h com previsão de 23 h 50 e teto calculado de 27 h 50. A V6 exige 0 pares e 0 C9 nesse caso, com 26 h exatas permitidas. `app/ponto/turnos.py:78`, `app/ponto/turnos.py:1433`, `app/ponto/turnos.py:1447`, `app/docs/PROMPTS.md:2852`.
- O teste legado de col129 exige dois turnos onde a V6 exige um. O de col879 também cobra separação por teto e precisa de exame do papel da nova entrada antes de qualquer alteração. A contraprova de absorção tem mecanismo distinto e deve continuar sem C9. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`, `app/ponto/tests/test_h_portao_teto_do_dia.py:32`, `app/ponto/turnos.py:1349`.
- A série HJ-026 de col155 inclui 08/11, dia omitido na matriz do dossiê. A sequência nominal de 08/11, a composição dos oito domingos auditados e os gabaritos de produção por rubrica seguem [nao verificado]. `app/docs/BACKLOG.md:745`, `app/docs/RELATO.md:542`, `app/docs/RELATO.md:544`.
- A Lei 1 resolve o dia quando a absorção é demonstrada. Substituir globalmente a guarda da véspera por 26 h derrubaria os controles da jornada diurna e da órfã sem entrada. A duplicidade de PK entre dois turnos na troca de vínculo segue registrada na obra DIA-DA-BATIDA. `app/ponto/tests/test_o195_dia_do_turno_por_envelope.py:77`, `app/ponto/tests/test_o195_dia_do_turno_por_envelope.py:141`, `app/docs/RELATO.md:18`, `app/docs/RELATO.md:139`.

## 26/11 17:47 · sessao 80756353ca60 · - · - · origem=Ronald (tmux)

- Lei 1 fechada em `app/docs/PROMPTS.md:3025`, `app/docs/RELATO.md:170` e `app/docs/LEIS.md:117`. V6 em `app/docs/PROMPTS.md:2852`, fila em `app/docs/BACKLOG.md:2027` e `app/docs/RELATO.md:141,161`. Interjornada em obra própria em `app/docs/BACKLOG.md:2029`.
- O dossiê trata PAR-RELÂMPAGO c7 como obra em voo. A cópia já traz o pouso `e216b756a` registrado em `app/docs/RELATO.md:111–112`. Preserve reapresentação e identidade do par em `app/ponto/turnos.py:1850,1921–1928,2193–2240,2971,3001–3024` e C13 em `app/escala/services/cadastro_realidade.py:135,406–427,1012–1013`.
- DIA-DA-BATIDA F4b altera a alimentação, a casca e o lote no mesmo `turnos.py`, confirmado na árvore `0d081f585` pelas linhas `1850,2901,2934,2952,2964,3126`. F4c aparece em `app/docs/RELATO.md:18`. Escopo comum F4c/F4d conforme enunciado. Delta exato F4c/F4d [nao verificado]. E1a `8d81e64c5` compartilha o agregador CxR e traz C13 de previsão inválida em `../8d81e64c58a57603b52de46bc6fcc8ab4251c949/app/escala/services/cadastro_realidade.py:135–141,719,980,989–995,1045`. O motor do E1a voltou à base antiga conforme `app/docs/RELATO.md:14,26`. FERIADO compartilha a integração de cálculo e a fila, com seu escopo registrado em `app/docs/BACKLOG.md:2005,2027`.
- Produção recomendada para o brief em `app/ponto/turnos.py:216–324,1084–1102,1170–1189,1430–1502`, `app/ponto/services/teto_na_lista.py:39–106` e `app/escala/services/cadastro_realidade.py:100–107,430–443`. Inclua `app/ponto/management/commands/lavrar_teto_do_dia.py:2,20` estritamente se corrigir a descrição pública do comando. Ela ainda diz “pares separados”. O dono continua pela mesma função em `app/ponto/services/teto_na_lista.py:86` e `app/ponto/management/commands/e6_oraculo.py:98–110,182`.
- Execução em memória da função real com stubs de infraestrutura confirmou dois turnos na borda sintética `20/10 17:30 E → 21/10 06:30 S` com DNA dia 20 `21:30–05:42` e dia 21 `08:00–17:00`. A base dá 1 aberto no dia 20, 1 órfã no dia 21 e C9 `780/732` atribuída ao dia 20. A regra exige 1 fechado, 0 órfãs, PK da saída pertencente ao dia 20 e 1 C9 na competência 10, 0 na 11. `app/ponto/turnos.py:1430–1457,1491–1502`, `app/ponto/janelas.py:50–63`, `app/colaboradores/models.py:46`.
- O fecho principal usa DNA pela data civil da entrada (`app/ponto/turnos.py:1430`), enquanto pós-fechos usam `data_turno` (`:1086,:1177`). A borda anterior pode passar com o juiz de dia intacto. O discriminante adicional usa caso com madrugada que tenha pertencimento comprovado ao turno da véspera e saída posterior a `hi + previsto +4h`. Exemplo em memória: E 21 00:30, S 21 07:10, DNA do dia 20 `23:50–03:00` e do 21 `08:00–17:00`. Base fecha 400 minutos no dia 21. Com pertencimento provado ao 20, resultado exigido no 20. `app/ponto/turnos.py:216–231,285–309`.
- `27 h` com previsto `23h50` reproduz fecho na base. `26 h` é o máximo, com testes na igualdade e em `26 h +1 s`. O 12x36 literal `19–01/02–07` já fecha em memória, então funciona como controle junto de um RED de extensão. `app/ponto/turnos.py:78–82,1447–1457,1491–1502`. A matriz mantém a guarda própria de absorção em `app/ponto/turnos.py:1327–1355`.
- O gabarito antigo contém efeitos incompatíveis com a V6 em `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61–63`, `app/ponto/tests/test_h_portao_teto_do_dia.py:32–36`, além de BATIDA decorrente do furo em `app/ponto/tests/test_col129_teto_vira_lista.py:209–225`. Os novos REDs devem morder antes de qualquer adaptação das caracterizações superadas. Contadores, idempotência, dono pela função real e casos de furo legítimo continuam cobertos.
- `app/docs/RELATO.md:6225–6232` registra comp. 11 em 21/10–20/11, empresas 2/3/4 e 960 colaboradores. PAR antigo tinha 14 colab-dias em 8 colaboradores. O B1 do INTERVALO V2 tinha 760 colab-dias/199 colaboradores e B2 tinha 1584/275. A `mesa/TABELA.md` documentada em `app/docs/RELATO.md:6234–6247` contém quatro chamados/26 opções. Universo e predicado de 142 [nao verificado]. Os oito domingos ZERO de col129/155 são o recorte fornecido pelo anexo e as oito chaves individuais permanecem [nao verificado].
- As linhas de triagem corretas são `app/docs/RELATO.md:542–550` para os grupos do laudo e `:554,:566,:568` para pontas diurnas e números do teto. Para 15/11, cite a matriz raspada para instantes individuais e use `app/docs/RELATO.md:566,:568` para 780/783 e âncoras; pausa completa e rubricas [nao verificado]. Para 11/10 há horas sintéticas em `app/ponto/tests/test_col129_teto_vira_lista.py:52–56`; sequência de produção [nao verificado]. Nenhuma nova pergunta de lei confirmada.

## 26/11 17:47 · sessao c9070bf5d1ec · - · - · origem=Ronald (tmux)

Censo AST enviado com 13 chamadas diretas de `parear_turnos`, 51 de `turnos_do_colab`, quatro de `turnos_de_batidas`, três de `_turnos_do_colab_puro` e as fronteiras de atribuição por PK, C9 e HE. Os pontos de origem permanecem em `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1513`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1934` e `app/ponto/turnos.py:3034`.

A cópia registra 760 dia-colaborador no B1 e 1.584 no B2 de INTERVALO V2, e 14 dia-colaborador em oito colaboradores no recorte PAR da competência 11. O universo do 142 permanece [nao verificado]. `app/docs/RELATO.md:6225–6234`.

A asserção legada de col129 exige o par separado e conflita com o fecho da V6 para a mesma fixture. Preserve o gabarito de C9 e explicite a substituição da obrigação geométrica pela regra aprovada. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:57–71`, `app/docs/PROMPTS.md:2852`.

PAR-RELÂMPAGO consta como pousada. DIA F4c consta no relatório da cópia, e o delta de F4d está [nao verificado]. `app/docs/RELATO.md:18`, `app/docs/RELATO.md:112`.

## 26/11 17:46 · sessao 9da75a87c3a9 · O30-F3-CONSTRUIR · verificador · origem=Code

**O30-F3-CONSTRUIR c11 — bloqueio 2 curado.** O RED de `KeyboardInterrupt` falhou por falta do aviso com o blob c10 reconstruído em memória e passou com a c11. Os três blobs alterados e suas reconstruções c10 conferem com o delta fornecido. A cópia veio sem `.git`. A identidade dos commits e o comportamento em produção ficam [nao verificado]. `app/ponto/services/ausencia.py:1435,1442,1444` · `app/ponto/tests/test_atestado_f3_contrato.py:524,539` · `app/ponto/tests/mutantes_atestado_f3.py:96,408`

### 1. Saídas de `_reabrir`

| Saída | Resultado na c11 |
|---|---|
| Sucesso | **nada**. `concluiu` vira `True` depois da saída normal do `with`. A função marca `_criada=False` e `_reaberta=True` e devolve a instância. `app/ponto/services/ausencia.py:1435,1443,1446` |
| `Exception` ou `BaseException` com upload detectado e atomic externo ainda ativo | Emite um aviso de possível órfão e deixa a exceção subir. A compensação fica fora desse caminho. Os testes de `RuntimeError` e `KeyboardInterrupt` conferem a identidade da exceção, o aviso exato e a ausência de consulta ou exclusão. `app/ponto/services/ausencia.py:1436,1441,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:521,524,538,542` |
| Retorno antecipado | **nada** na árvore. O único `return` fica depois do `finally`. Injetei em memória um `return` logo após o upload e ele emitiu o aviso sob atomic externo, pois ainda pulava `concluiu = True`. `app/ponto/services/ausencia.py:1422,1435,1443,1448` |
| Falha antes do upload, ausência de upload ou documento já `_committed` | **nada**. `anexo_gravado` só recebe valor se o documento passar de pendente a `_committed` durante o `save`. `app/ponto/services/ausencia.py:1408,1412,1416,1421` |
| Falha no `save` do `FileField` | Se a falha vier **depois** da transição para `_committed`, o `finally` interno captura `(storage, nome)`, inclusive para `BaseException`. Se o storage deixar um arquivo e falhar **antes** dessa confirmação, a porta fica sem sinal e o aviso deixa de sair. Reproduzi os dois caminhos em memória. A ocorrência com o storage de produção fica [nao verificado]. `app/ponto/services/ausencia.py:1416,1418,1421,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:551,555` |
| Falha ao sair do `with` | `concluiu` ainda está `False`. Com upload detectado, o estado da conexão **após** a saída do `with` decide o caminho. Simulei falhas na saída com e sem atomic externo. `app/ponto/services/ausencia.py:1411,1435,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:439,444` |

**Sem atomic externo, a compensação continua igual à c10.** Com `Exception` e upload detectado, a porta tenta apagar o nome novo, preserva o nome anterior e qualquer nome ainda referenciado, registra `anexo_apagado` e relança. Uma `BaseException` fora de `Exception` continua subindo sem compensação nem aviso quando a conexão está fora de atomic. `app/ponto/services/ausencia.py:249,256,262,266,1436,1443`

O aviso sinaliza **risco**. Sob atomic externo, ele pode sair mesmo que o arquivo acabe referenciado, pois a porta não consulta referências nesse caminho. Se o chamador fizer rollback **depois** de `_reabrir` devolver, a porta já encerrou sem aviso. A view de INSS tenta compensar esse segundo caso quando recebe a instância e a falha posterior é uma `Exception`. `app/ponto/services/ausencia.py:1437,1443,1448` · `app/ponto/views.py:2569,2572,2588,2590,2596`

### 2. Quem escreve e quem lê

| Valor | Escritor, leitores e significado |
|---|---|
| `concluiu` | `_reabrir` escreve `False` antes do `try` e `True` depois do `with`. Só o `finally` lê. Significa que o fluxo local chegou ao fim do bloco protegido. O estado final da transação do chamador fica fora desse sinal. `app/ponto/services/ausencia.py:1409,1435,1443,1448` |
| `anexo_gravado` | `_reabrir` inicializa com `None` e o `finally` interno registra `(storage, nome)` pela transição de `_committed`. O `except`, o `finally` externo, os dois avisos e o compensador consomem esse mesmo fato. O compensador consulta separadamente se o nome ainda tem referência. `app/ponto/services/ausencia.py:1408,1416,1421,1437,1443,249,262` |
| `conexao.in_atomic_block` | Em produção, a porta obtém a conexão do alias escolhido pelo router e lê o atributo do Django depois do `with`. O escritor interno do atributo no Django fica [nao verificado] nesta cópia. O teste escreve `False` na fixture e `True` nos casos com atomic externo. Os leitores da porta tratam o valor como “a conexão ainda está dentro de um atomic neste ponto”. `app/ponto/services/ausencia.py:1406,1411,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:368,511,529` |
| Aviso | `_reabrir` escreve no logger `ponto`. A configuração SaaS encaminha o `WARNING` ao `pontofile`. O contrato lê o texto exato e o teste de integração procura a mensagem com ID e nome. Ambos esperam um aviso de possível órfão. Consumo operacional em produção [nao verificado]. `app/ponto/services/ausencia.py:49,1444` · `app/config/settings/saas.py:131,160,181` · `app/ponto/tests/test_atestado_f3_contrato.py:546` · `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164,183` |

### 3. Selo do `Image.open`

As cinco trocas falharam em memória. O selo exige um único tratador, do tipo AST `Tuple`, e compara a tupla inteira com `(UnidentifiedImageError, OSError)`. `app/ponto/tests/test_atestado_f3_contrato.py:228,238,241,244,246`

| Troca | Efeito e ponto da queda |
|---|---|
| `except Exception` | Também converte erros como `ValueError` em `None`. Cai em `assertIsInstance`, pois é `ast.Name`. `app/ponto/services/atestados_lote.py:147,151` · `app/ponto/tests/test_atestado_f3_contrato.py:244` |
| `except BaseException` | Também converte `KeyboardInterrupt` e `SystemExit` em `None`. Cai no mesmo `assertIsInstance`. `app/ponto/services/atestados_lote.py:147,151` · `app/ponto/tests/test_atestado_f3_contrato.py:244,525` |
| `except OSError` | Captura `OSError` e suas subclasses. No mock, isso inclui `UnidentifiedImageError`. A hierarquia no Pillow real fica [nao verificado]. Cai em `assertIsInstance`. `app/ponto/tests/test_atestado_f3_contrato.py:244,792` |
| Tupla reordenada | Captura os mesmos tipos. Cai na igualdade AST pela ordem dos elementos. `app/ponto/services/atestados_lote.py:151` · `app/ponto/tests/test_atestado_f3_contrato.py:245,246` |
| Tupla com tipo a mais | Com `ValueError` a mais, também converte esse erro em `None`. Cai na igualdade AST pelo elemento adicional. `app/ponto/services/atestados_lote.py:151` · `app/ponto/tests/test_atestado_f3_contrato.py:245,246` |

### 4. REDs, mutantes e mocks

- **Mordem.** `green_c11` passou nos 12 testes. O RED histórico e `c11_reabrir_sem_finally` caíram por falta de aviso. `c11_reabrir_sem_marca_de_sucesso` caiu por aviso indevido no sucesso. `c11_pdf_handler_amplo` caiu no selo AST. O mutante adaptado `c8_try_dentro_do_atomic` também caiu, pois deixou o arquivo novo após a falha na saída do `with`. `app/ponto/tests/mutantes_atestado_f3.py:96,124,291,404,423` · `app/ponto/tests/test_atestado_f3_contrato.py:244,456,512,539`
- **O mock de transação deixa passar uma regressão concreta.** Mover a leitura de `in_atomic_block` para dentro do `with` e reutilizar o valor nos dois ramos passou nos 12 testes. Com um contexto simulado que muda a flag de `False` para `True` na entrada e volta a `False` ao sair, o mutante emitiu o aviso de atomic externo e deixou o arquivo. A c11 compensou. A fixture padrão e o teste de falha na saída mantêm a flag fixa. `app/ponto/services/ausencia.py:1411,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:368,444,529`
- **O mock de decode ainda encobre a remoção isolada de `original.load()`.** A transposição simulada chama `load()`. Confirmei o teste comportamental verde e o selo AST vermelho para essa mutação. O verde e o mutante com PDF real pararam no `import PIL`, então o bloqueio 1 do laudo continua [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:235,239,774,812,832` · `app/ponto/tests/mutantes_atestado_f3.py:138,406`

### 5. Contagem AST c10 → c11

| Arquivo | `ExceptHandler` | Chamadas a `suppress` |
|---|---:|---:|
| `app/ponto/services/ausencia.py:266,1436,1821` | 13 → 13 | 0 → 0 |
| `app/ponto/services/atestados_lote.py:151,263,271` | 3 → 3 | 0 → 0 |
| `app/ponto/tests/test_atestado_f3_contrato.py:51,62,64,241,527` | 0 → 0 | 0 → 0 |
| `app/ponto/tests/mutantes_atestado_f3.py:124,207,291` | 0 → 0 | 0 → 0 |

O `suppress` do arquivo de mutantes está numa string usada para gerar uma variante. A contagem acima é do AST do arquivo. `app/ponto/tests/mutantes_atestado_f3.py:207,209`

### 6. Classes já reprovadas

- **2º juiz, 2º escritor fora da porta, relógio fora da borda, derivação paralela, ordem de trava e campo com dois significados:** **nada** novo no delta. Os pontos de decisão, gravação, relógio e trava permanecem nos mesmos caminhos. `app/ponto/services/ausencia.py:301,306,317,1403,1412,1418,1427,1432,1435` · `app/ponto/services/atestados_lote.py:172`
- **Selo/RED sem mordida:** **nada** entre os casos novos registrados. A lacuna da flag fixa permanece na cobertura simulada, conforme o contraexemplo do item 4. `app/ponto/tests/mutantes_atestado_f3.py:96,124,404` · `app/ponto/tests/test_atestado_f3_contrato.py:368,444,529`
- **`except` ou fallback novo:** **nada**. O `except Exception` continua no ramo de compensação. O `finally` externo passa a emitir o aviso existente também quando uma `BaseException` escapa. `app/ponto/services/ausencia.py:1436,1441,1442,1444`

## 26/11 17:41 · sessao 2871c389c64e · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

## Veredito — REPROVADO

1. **B1 corrige o ciclo apontado na F4c entre regularização e `apagar`, mas a ordem ainda falha em caminhos alcançáveis.** A regularização pode travar uma Pergunta no `post_save` e depois adquirir a trava de um Chamado no encerramento do lote. A nova trava do Colaborador também fecha um ciclo com o flip automático sobre a mesma Batida. `app/chamados/services/regularizacao_ext.py:122–123,153–157,203–207` · `app/chamados/reconciliador.py:673–694` · `app/chamados/models.py:773–777` · `app/ponto/management/commands/flip_automatico.py:62–68`
2. **A cura de B2 está no código. Falta confirmar o RED executável.** Nesta cópia, tanto o teste da versão atual quanto o mutante pararam no import de `django.db`. O mutante retornou `RC=2`, `FAIL=0`, `ERROR=1`. Sua queda por asserção fica **[nao verificado]**. `app/ponto/registro_batida.py:328–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:225–232`

## Curas e mutantes

| Item | Sítio e teste que morde | Resultado na cópia | [nao verificado]
|---|---|---|
| **B1** | Trava no topo de `app/chamados/services/regularizacao_ext.py:122–124`. O teste espiona a ordem de entrada no `atomic`, trava, leitura de itens e escrita em `app/ponto/tests/test_juiz_unico_eventos.py:409–432`. | Verde na fonte atual. `f4d-b1-sem-trava` cai com **RC 1**, duas falhas por asserção, em `corrigir` e `remover`. `app/ponto/tests/juiz_unico_eventos_mutantes.py:123–125` · `app/ponto/tests/test_juiz_unico_eventos.py:431` |
| **B2** | O juiz está antes do `delete`, fora da proteção e do savepoint interno. O teste exige o mesmo `DatabaseError`, linha e PK preservados, zero trilha, agendamento, savepoint e commit, e um rollback. `app/ponto/registro_batida.py:333–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | `f4d-b2-protecao-e-savepoint` restaura o bloco que engole a exceção exigida pelo teste. A execução parou antes dessa asserção. **[nao verificado]** `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134` · `app/ponto/tests/test_juiz_unico_eventos.py:333,351` |
| **B3** | INFO passou a carregar `processados=%d`. O teste exige `processados=0` e uma só chamada para dois dias da mesma competência. `app/ponto/services/fechamento.py:1841–1845` · `app/ponto/tests/test_juiz_unico_eventos.py:474–483` | Verde na fonte atual. `f4d-b3-info-antigo` cai com **RC 1** na mensagem. O RED de K3, que exige nova tentativa quando uma exceção sobe ao callback, também cai com **RC 1**. `app/ponto/tests/juiz_unico_eventos_mutantes.py:135–141,171–175` · `app/ponto/tests/test_juiz_unico_eventos.py:462–483` |
| **E5** | A promessa do cron saiu do comentário. A configuração da cópia declara que `recalcular_fechamento` fica sem cron. `app/ponto/registro_batida.py:192–194` · `app/config/crons.py:1148–1155` | Verde na fonte atual. `f4d-e5-comentario-antigo` cai com **RC 1** na asserção sobre o comentário. `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` |

## B1 — Ordem dentro do `atomic`

- **Sim, é a primeira aquisição de trava de linha daquele `atomic`.** Com os argumentos usados, `trava_na_ordem` pede só o Colaborador. No par original, regularização e `apagar` agora pedem o mesmo Colaborador antes de escrever ou travar a Batida. A consulta que veta disputa aberta ainda roda antes da trava. Uma abertura concorrente pode passar entre a consulta e a aquisição, e o materializador segue sem reler. `app/chamados/services/regularizacao_ext.py:59–66,122–124` · `app/chamados/models.py:2901–2902,2933–2967` · `app/ponto/registro_batida.py:328–336` · `app/chamados/services/disputa_emissao.py:530–542,601–606`
- **Sequência interna.** `corrigir` faz `UPDATE` direto. `adicionar` delega o `INSERT` a `registrar_batida`. Os dois passam pelo `post_save`, que pode travar Disputas, Chamados e Perguntas via reconciliador. `remover` faz `delete` direto e aciona núcleo e cartório pelo `post_delete`. No fim, `transicionar_com_pauta(..., ato=reg.chamado.encerrar)` tenta travar explicitamente o Chamado do lote. Se o `post_save` já travou uma Pergunta e o Chamado do lote era terminal e estava fora do conjunto travado, a aquisição nova é **Pergunta → Chamado**. O teste de B1 usa escritas simuladas e `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:73–83,133–157,203–207` · `app/ponto/registro_batida.py:178–182` · `app/ponto/signals.py:53–55,148–150,216–225` · `app/chamados/reconciliador.py:673–694` · `app/chamados/services/fecho_runtime.py:157–168` · `app/chamados/models.py:773–777,1785–1793,2952–2966` · `app/ponto/tests/test_juiz_unico_eventos.py:417–432`
- **Contraexemplo com dois atos.** T1, `flip_automatico --apply`, abre o `atomic` e atualiza a Batida B via `flip_tipo` → `anotar`. T2, a regularização, trava o Colaborador C e espera B ao tentar corrigir a mesma linha. T1 entra no reconciliador pelo `post_save` e espera C se houver pergunta elegível. Uma pergunta de disputa já fechada satisfaz o seletor, portanto esse cenário é compatível com a guarda da regularização. O ciclo é **T1 segura B e espera C, T2 segura C e espera B**. Execução em PostgreSQL e ocorrência em produção **[nao verificado]**. `app/ponto/management/commands/flip_automatico.py:62–68` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:281–287` · `app/ponto/signals.py:216–225` · `app/chamados/reconciliador.py:673–679,740–762,1580` · `app/chamados/services/regularizacao_ext.py:59–66,122–123,153–157`
- **Há também um ciclo possível com batida de chão.** A regularização segura C em `FOR UPDATE`. O app pode reservar no `INSERT` a chave única `(C, timestamp)` e esperar o `KEY SHARE` do FK em C. Se a regularização tenta corrigir outra Batida para esse mesmo instante, espera a chave ocupada pelo app. A hora válida do aparelho pode coincidir com a hora corrigida. Execução desse ciclo e efeito em produção **[nao verificado]**. `app/chamados/models.py:2938–2941` · `app/chamados/services/regularizacao_ext.py:122–123,144–157` · `app/api/views_core.py:492–495,643–659` · `app/api/hora_do_aparelho.py:135–146` · `app/ponto/registro_batida.py:178–180` · `app/ponto/models.py:22,102–106` · `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10`

## B2 — Chamadores e exceção

- **Produção.** O único chamador encontrado é o POST administrativo `rejeitar` de `batidas_offline_pendentes`, protegido por `ver_painel_situacional`. O `except Batida.DoesNotExist` cobre a consulta inicial. O erro do juiz sai da view antes da resposta de sucesso. Esse ato é a rejeição administrativa de marca offline, e nenhum chamador de chão foi encontrado para `apagar`. A resposta HTTP observada em produção é **[nao verificado]**. `app/ponto/urls.py:78` · `app/ponto/views.py:452–456,467–482,496–500` · `app/docs/LEIS.md:78`
- **Testes.** O chamador em `test_juiz_unico_eventos.py:352` captura `DatabaseError` com `assertRaises`. Os demais deixam uma exceção inesperada falhar o teste. `app/ponto/tests/test_juiz_unico_eventos.py:273,305,314,351–352,368,401` · `app/ponto/tests/test_porta_batida.py:80–81`
- **Limite da proteção.** Se a leitura viva falha, o erro sai do `atomic` antes do `delete`, da trilha e da limpeza do PK. Depois do `delete`, a união dos dias e o agendamento continuam sob `_proteger_fechamento_por_evento`. O callback roda em `on_commit` e conserva seus `except`. O `delete` e seus receivers ficam fora desse guard. `app/ponto/registro_batida.py:197–204,328–343` · `app/ponto/services/fechamento.py:1830–1856`

## B3 — Retorno e log

Com os dois defaults `False`, **todo retorno normal é `int`**. O contador começa em `0`, é incrementado com `1` e é o único retorno desse modo. Universo vazio, guarda que exclui o colaborador ou exceção por colaborador antes do incremento podem deixar `0`. O dicionário pertence ao modo leitura. O `%d` recebe o contador em argumentos separados do formato, com a competência já marcada. Falhas internas de `DiaPago` ainda podem coexistir com `processados=1`, e o `except` mensal registra PK e nome quando falha no nível do colaborador. `app/ponto/services/fechamento.py:468–471,574–584,636–639,1152–1160,1262–1265,1321–1335,1841–1845`

## AST — `except` e `suppress`

Nenhum novo. Contagem de `ast.ExceptHandler` e chamadas a `suppress`, da base do delta para a cópia. Em B2 saiu um **uso** de `_proteger_fechamento_por_evento`, cujo `except` segue atendendo os outros usos. `app/ponto/registro_batida.py:197–212,333–340` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134`

| Arquivo | `except` | `suppress` |
|---|---:|---:|
| `app/chamados/services/regularizacao_ext.py:104,112,158,191` | 4 → 4 | 0 → 0 |
| `app/ponto/registro_batida.py:18,200` | 2 → 2 | 0 → 0 |
| `app/ponto/services/fechamento.py:594,1105,1111,1262,1321,1327,1685,1846,1854` | 9 → 9 | 0 → 0 |
| `app/ponto/tests/juiz_unico_eventos_mutantes.py:82,189` | 0 → 0 | 0 → 0 |
| `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | 0 → 0 | 1 → 0 |

## Classes e escritores/leitores do delta

**Ordem de trava** é a classe que reprova. O selo de B1 morde a remoção da chamada local, com alcance limitado aos dois ramos simulados. A escrita direta de correção e remoção fora da porta já existia. A correção de tipo e horário está registrada como classe 9. O delta reutiliza o juiz de evento, a competência do fato e o escritor mensal, sem introduzir relógio ou derivação paralela. `processados` tem um significado de contagem, mesmo com várias causas possíveis para zero. `app/ponto/tests/test_juiz_unico_eventos.py:409–432` · `app/chamados/services/regularizacao_ext.py:133–157` · `app/core/portas.py:98–105` · `app/docs/CORTES.md:441` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:637,1326–1335,1830–1845`

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `RegularizacaoExterna.colaborador_id` e sentinela da trava | `criar_regularizacao_externa` grava o FK. Guarda, criação de Batida, trilha e nova trava leem a relação ou seu ID. `trava_na_ordem` registra a aquisição na conexão, lida por `colaboradores_travados` e `estampa_das_travas`. `app/chamados/services/regularizacao_ext.py:59–65,73–82,98,123,238–241` · `app/chamados/models.py:2938–2941,4592–4595` · `app/chamados/services/trava_do_colaborador.py:26–28,37–64,72–79` |
| `Batida.pk`, `colaborador_id`, `timestamp`, `tipo` | O `create` da porta gera a linha e o PK. A correção externa também grava `timestamp` e `tipo`. A view lê a marca para a trilha. `apagar` guarda o PK, relê a linha e entrega a marca viva ao juiz. Após sucesso, limpa o PK da instância recebida. `app/ponto/registro_batida.py:172–182,318–343` · `app/chamados/services/regularizacao_ext.py:151–157` · `app/ponto/views.py:474–489` · `app/ponto/turnos.py:3037–3047` |
| `.dias`, `dias_vivos`, `_dias_exclusao`, `dias_da_jornada` | `julgar_evento_da_batida` produz `.dias`. `apagar` guarda os dias vivos. O núcleo escreve `_dias_exclusao` no `post_delete`, e o cartório lê ou preenche pelo mesmo juiz. A porta une os dias e o callback consome a união, inclusive vazia. `app/ponto/turnos.py:3034–3057` · `app/ponto/signals.py:19–25,53–55,113–122,148–150` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:1830–1841` |
| `motivo`, competência e `processados` | `apagar` fornece `motivo='exclusao'`. O resolvedor fornece mês e ano para a deduplicação, a chamada mensal e o log. O escritor mensal produz a contagem, lida agora pelo INFO e já lida pela tela e pelo comando. `app/ponto/registro_batida.py:340` · `app/ponto/services/fechamento.py:1618–1628,637,1326–1335,1830–1850` · `app/ponto/views.py:1280–1281` · `app/ponto/management/commands/recalcular_fechamento.py:130–134` |
| Texto de E5 | O comentário da porta é lido pelo teste via `Path.read_text`. O mutante injeta a versão antiga nessa leitura. `app/ponto/registro_batida.py:187–196` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` |

## 26/11 17:40 · sessao 85c92057ec44 · O30-F3-CONSTRUIR · construtor · origem=Code

A c11 está aplicada na cópia. `_reabrir` agora avisa sobre possível anexo órfão se a execução falhar após gravar o upload sob `atomic` externo, inclusive por `KeyboardInterrupt`. A marca de sucesso é definida após a saída do `with`, e o teste de sucesso exige silêncio. O selo AST de `_foto_no_pdf` exige um único tratador com `(UnidentifiedImageError, OSError)`. [ausencia.py:1435](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1435) · [test_atestado_f3_contrato.py:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) · [test_atestado_f3_contrato.py:509](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:509) [nao verificado]

Da pasta `app/`, os comandos da tabela usam o executor de mutantes em memória. [mutantes_atestado_f3.py:365](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:365)

```bash
f3() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$@"
}
```

| Item | RED | GREEN | Mutantes |
|---|---|---|---|
| 1. Aviso em `_reabrir` | `f3 red_c10_reabrir_keyboard_interrupt` falhou na base com `assertLogs`, rc 1. [Teste:524](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:524) | `f3 green_c11` passou os 12 testes, incluindo `KeyboardInterrupt`, `RuntimeError` e sucesso sob `atomic` externo, rc 0. [Executor:404](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:404) | `f3 c11_reabrir_sem_marca_de_sucesso` caiu em `assertNoLogs`. `f3 c11_reabrir_sem_finally` caiu em `assertLogs`. Ambos rc 1. [Mutantes:125](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:125) | [nao verificado]
| 2. Tipo do tratador do PDF | `f3 c11_pdf_handler_amplo` passou com o selo c10 antes do reforço, rc 0. [Mutante:131](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:131) | `f3 green_c11` passou com o par de exceções especificado, rc 0. [Teste:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) | `f3 c11_pdf_handler_amplo` caiu na asserção que exige a tupla, rc 1. [Teste:244](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:244) | [nao verificado]

**Contagem AST c10 → c11**

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| [ponto/services/ausencia.py:1436](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1436) | 13 → 13 | 0 → 0 |
| [ponto/services/atestados_lote.py:151](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:151) | 3 → 3 | 0 → 0 |
| [ponto/tests/test_atestado_f3_contrato.py:528](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:528) | 0 → 0 | 0 → 0 |
| [ponto/tests/mutantes_atestado_f3.py:117](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:117) | 0 → 0 | 0 → 0 |

O chamador do PDF de lote é `lote_atestados`, em `ponto/views_atestado.py`. Pillow e PDF reais ficam para a suíte da obra [nao verificado]. [views_atestado.py:136](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:136) · [test_atestado_f3_contrato.py:832](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:832)

**SHA.** O `HEAD` continua em `119aaa8bf8870c7eef8bbe3ecd76f703992b994e`. A entrega ficou no diff da cópia porque as regras vedam `git commit` e a operação escreveria fora de `app/`. SHA de commit c11 [nao verificado]. [mutantes_atestado_f3.py:25](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:25)

## 26/11 17:39 · sessao 65c5ec36379d · - · - · origem=Ronald (tmux)

**B1 corrige o ciclo original entre regularização e `apagar`, mas ainda há inversão de trava.** A nova chamada é a primeira instrução do atomic da materialização e trava apenas o Colaborador. A exclusão do DP também começa pelo Colaborador antes da Batida. O teste passou na cópia e o mutante caiu com RC 1, dois `FAIL` e zero `ERROR`. O teste usa mocks de trava e escrita, cobre corrigir e remover e passa `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:122-124`, `app/chamados/models.py:2901-2902,2938-2967`, `app/ponto/registro_batida.py:328-336`, `app/ponto/tests/test_juiz_unico_eventos.py:409-432`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:123-125`.

**Contraexemplo com dois atos.** O `flip_automatico --apply` entra no atomic e `flip_tipo` chama `anotar`, cujo UPDATE segura a Batida B. A regularização concorrente do mesmo Colaborador C segura C pelo delta e espera B ao corrigir. O `post_save` do flip chama o reconciliador, que tenta C quando há pergunta elegível. Uma pergunta de disputa já fechada satisfaz essa condição sem acionar a guarda de disputa aberta. A agenda da cópia contém esse comando. Ocorrência em produção [nao verificado]. `app/config/crons.py:707`, `app/ponto/management/commands/flip_automatico.py:51-68`, `app/ponto/services/flip_batida.py:21-32`, `app/ponto/registro_batida.py:264-287`, `app/chamados/services/regularizacao_ext.py:59-66,122-123,153-157`, `app/ponto/signals.py:216-225`, `app/chamados/reconciliador.py:651-653,673-694,740-762,1580`.

**Dentro do atomic.** Corrigir e adicionar executam `post_save` e podem travar o universo Disputa/Chamado/Pergunta no reconciliador. Remover executa os `post_delete` do núcleo e do cartório. O `transicionar_com_pauta` executa o `encerrar` do chamado do lote, que pede C, D abertas e o próprio Ch antes de decidir. Se o `post_save` já travou uma P e o chamado do lote é terminal e está fora dos destinos previamente travados, `encerrar` pode adquirir esse Ch pela primeira vez depois de P. A posse de C serializa esse exemplo com atos que também respeitam C primeiro. `app/chamados/services/regularizacao_ext.py:78-83,138,157,207`, `app/ponto/registro_batida.py:172-182`, `app/ponto/signals.py:53-55,141-150,216-225`, `app/chamados/reconciliador.py:686-694`, `app/chamados/services/fecho_runtime.py:157-168`, `app/chamados/models.py:773-777,1785-1793,2943-2966`, `app/chamados/catalogo/motor.py:24-31`, `app/chamados/services/acoes_chamado.py:322-329`.

A guarda de disputa ocorre antes da nova trava. Outro ato pode abrir D sob C entre a leitura da guarda e a aquisição de C pela regularização, que prossegue sem reler. Isso já existia no trecho do anexo. `app/chamados/services/regularizacao_ext.py:59-66,122-124`, `app/chamados/services/disputa_emissao.py:530-542,601-606`.

O `colaborador_id` novo é o FK gravado na criação do lote. A chamada também registra um sentinela de trava na fila da conexão, lido pelas portas que exigem ou identificam a trava corrente. `app/chamados/models.py:4592-4595,2939-2941`, `app/chamados/services/regularizacao_ext.py:238-241`, `app/chamados/services/trava_do_colaborador.py:26-28,37-64,72-80`.

O teste e o mutante B2 pararam no import de `django.db` no ambiente somente leitura. O mutante saiu RC 2 com `FAIL=0 ERROR=1`. Queda por asserção [nao verificado]. `app/ponto/tests/test_juiz_unico_eventos.py:333`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:228-232`.

## 26/11 17:39 · sessao 715c4520f925 · - · - · origem=Ronald (tmux)

**B2 curado no código.** `apagar` trava e relê a batida, chama o juiz antes do `delete` e deixa o `DatabaseError` sair do `atomic`. A falha interrompe o ato antes do delete, agendamento, trilha e limpeza do PK. O agendamento pós-delete mantém `_proteger_fechamento_por_evento`; o recálculo roda no `on_commit` e conserva seus `except`. [app/ponto/registro_batida.py:318–343] [app/ponto/registro_batida.py:197–204] [app/ponto/services/fechamento.py:1830–1856]

- O único caller de produção encontrado é o POST `rejeitar` da tela offline. O `except Batida.DoesNotExist` cobre a consulta inicial. A exceção de `apagar` sai da view e impede a resposta `ok=True`. É um ato administrativo protegido por `ver_painel_situacional`, permissão que também pode alcançar supervisão e TI. Resposta HTTP observada em produção [nao verificado]. [app/ponto/urls.py:78] [app/ponto/views.py:452–456,467–482,496–500] [app/colaboradores/mixins.py:322–331,355–377] [app/docs/LEIS.md:78]
- Callers de teste em `test_porta_batida.py:80,81` deixam exceções subir. Em `test_juiz_unico_eventos.py:273,305,314,368,401` também. O caller de `:352` captura com `assertRaises(DatabaseError)`, afirma identidade, preservação das linhas e PK, zero trilha/savepoint/commit/on_commit e um rollback. O teste e o mutante `f4d-b2-protecao-e-savepoint` pararam no import de `django.db` nesta cópia. O mutante anunciou `FAIL=0 ERROR=1`, RC 2. A mordida em execução permanece [nao verificado]. Por inspeção, o mutante engole a exceção que o `assertRaises` exige. [app/ponto/tests/test_porta_batida.py:77–83] [app/ponto/tests/test_juiz_unico_eventos.py:318–360] [app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134,225–232] [nao verificado]

**Contraexemplo adicional para B1.** T1 regulariza e mantém `FOR UPDATE` no colaborador C. T2 bate pelo app no mesmo instante que T1 vai registrar ou usar como novo horário de uma correção. T2 chega primeiro ao INSERT de Batida e reserva a chave única `(C, timestamp)`, depois pede `FOR KEY SHARE` de C pelo FK. T1 chega ao INSERT ou UPDATE sobre a mesma chave e aguarda T2. O código aceita a mesma chave temporal nas duas portas. O conflito `FOR UPDATE` × `KEY SHARE` está documentado no teste equivalente de Posto/FK. Execução do ciclo específico de Batida e ocorrência em produção [nao verificado]. [app/chamados/services/regularizacao_ext.py:73–83,122–123,144–157] [app/chamados/models.py:2939] [app/api/views_core.py:483–495,643–659] [app/api/hora_do_aparelho.py:135–146] [app/ponto/registro_batida.py:133–148,178–180] [app/ponto/models.py:22,102–108] [app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10] [nao verificado]

## 26/11 17:36 · sessao 2d4c89ae68b2 · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO no pré-verificador c10.**

1. **Falta a prova com Pillow e PDF reais do item 1.** O verde e o mutante real pararam no `import PIL`. A queda do mutante com mock foi confirmada. O resultado com as dependências reais fica [nao verificado]. `test_atestado_f3_contrato.py:821-850` · `mutantes_atestado_f3.py:120-125,387-390` [nao verificado]
2. **A c10 perdeu o aviso em uma falha ocorrida dentro da própria reabertura.** Com upload já gravado, `KeyboardInterrupt` em `mudar_estado` sob `atomic` externo gerou o aviso na c9 reconstruída e saiu em silêncio na c10. Reproduzi em memória. Considerei essa saída excepcional parte de “falha loga”, porque ela ocorre antes do retorno e também pode deixar o anexo órfão. `ausencia.py:1413-1428,1434-1445` · `mutantes_atestado_f3.py:134-136`

Os quatro blobs da cópia conferem com o `delta.patch` fornecido. Os dois arquivos de serviço reconstruídos em memória também conferem com os blobs c9 do patch, e os dois REDs correspondentes falharam nas asserções esperadas. A confirmação dos hashes de **commit** e a ocorrência dos contraexemplos em produção ficam [nao verificado]. `mutantes_atestado_f3.py:92-93,391-395` · `test_atestado_f3_contrato.py:228-240,503-507` [nao verificado]

### 1. Os quatro itens do Opus

| Item | Sítio e teste que morde | Resultado | [nao verificado]
|---|---|---|
| 1. `load()` explícito e JPEG truncado | `atestados_lote.py:147-152` · `test_atestado_f3_contrato.py:801-815,821-850` | Código e teste real presentes. O mutante composto sem `load()` e com transposição fora do `try` caiu no mock com `OSError: truncado`. Retirar **só** o `load()` deixou o teste comportamental verde e derrubou o selo AST. A transposição do mock também chama `load()`. Mutante real [nao verificado]. `mutantes_atestado_f3.py:108-125` · `test_atestado_f3_contrato.py:239-240,763-765` | [nao verificado]
| 2. Corpo exato do `try` | `atestados_lote.py:147-158` · `test_atestado_f3_contrato.py:228-240` | Curado. `thumbnail` dentro do `try` derrubou a igualdade AST. `mutantes_atestado_f3.py:126-130` |
| 3. Uma marca e rodapé igual | `atestados_lote.py:201-214,225-232` · `test_atestado_f3_contrato.py:809-815,844-850` | Curado. A marca duplicada caiu com `2 != 1`. O mutante que zera o incremento também caiu, com rodapé `0` para uma marca. `mutantes_atestado_f3.py:131-133,140-142` |
| 4. Aviso na falha sob `atomic` externo | `ausencia.py:1434-1445` · `test_atestado_f3_contrato.py:503-538` | Curado para `Exception`. O mutante que repõe o `finally` caiu em `assertNoLogs`, e o que remove o aviso caiu em `assertLogs`. A saída por `BaseException` ficou sem aviso, conforme o bloqueio 2. `mutantes_atestado_f3.py:134-139` |

### 2. `_foto_no_pdf` e o rodapé

- O tratador cobre `Image.open`, a entrada e a saída do `with`, `original.load()` e `exif_transpose`, para `UnidentifiedImageError` e `OSError`. `thumbnail`, `save`, a construção da imagem do PDF e a abertura do documento no storage ficam fora. `atestados_lote.py:147-158,168-175`
- **Um anexo inacessível ainda pode derrubar o lote.** Contraexemplo reproduzido com mock: `exists()` devolve `True` e `documento.open('rb')` levanta `FileNotFoundError`, como na janela em que o arquivo some entre as chamadas. A exceção chega à montagem do PDF. Se `exists()` já devolve `False`, sai `Anexo indisponível`, com incremento zero. `atestados_lote.py:168-178,210-216` · `views_atestado.py:134-138`
- **Uma imagem decodificada pode virar `ILEGIVEL`.** No contraexemplo em memória, `load()` e a transposição completam, mas a saída do `with` levanta `OSError`. O resultado foi uma marca, rodapé `1` e nenhuma imagem embutida. Um anexo de tipo desconhecido segue como link, inclusive quando a leitura do cabeçalho falha e devolve `(None, None)`. `atestados_lote.py:147-152,171-178,211-214` · `ausencia.py:796-817`
- **O rodapé conta exatamente as marcas geradas pelo ramo `anexo is None`**, uma por passagem, e repete o total em cada página. Uma busca textual tem outro universo: `atestado_medico='ANEXO ILEGIVEL'` imprime a expressão no texto clínico mesmo com uma foto boa e rodapé `0`. `atestados_lote.py:201-214,225-232` · `forms.py:13-15`

### 3. `_reabrir` e o rollback do chamador

O aviso específico exige uma `Exception` que chegue ao tratador, upload detectado e `conexao.in_atomic_block` ainda verdadeiro. Sem atomic externo, o ramo tenta compensar e emite outro aviso. Em sucesso, `_reabrir` devolve a instância com `_criada=False` e `_reaberta=True`, sem emitir o aviso de órfão. `ausencia.py:1406-1421,1434-1445`

**Contraexemplo reproduzido em memória:** o chamador abre um atomic, recebe o retorno de `_reabrir` e só então levanta `RuntimeError`. O rollback restaura a referência anterior no banco simulado. A instância em memória ainda aponta para o nome novo, e o arquivo novo fica no storage. Sem outra referência ou compensação do chamador, sobra um órfão sem aviso de `_reabrir`. O teste de sucesso c10 para antes desse rollback. `ausencia.py:1413-1421,1434-1445` · `test_atestado_f3_contrato.py:409-431,503-513` [nao verificado]

**INSS tem os dois caminhos.** Se a falha vem depois do retorno, a view lê o nome restaurado e tenta compensar. Se `_reabrir` levanta antes de devolver, a atribuição de `ausencia` nunca completa, e a view pula essa limpeza. Para `Exception` com upload detectado, fica o aviso da porta. `views.py:2563-2579,2588-2601` · `ausencia.py:249-264,1434-1442`

### 4. Contagem AST c9 → c10

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| `atestados_lote.py:151,263,271` | 3 → 3 | 0 → 0 |
| `ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818` | 13 → 13 | 0 → 0 |
| `mutantes_atestado_f3.py:101-139,192-196,350-370` | 0 → 0 | 0 → 0 |
| `test_atestado_f3_contrato.py:51-72,228-240,503-538,801-850` | 0 → 0 | 0 → 0 |

O `finally` externo saiu. O `finally` interno que registra a gravação do upload continua. Em `mutantes_atestado_f3.py`, `suppress` aparece como texto de mutante. `ausencia.py:1416-1421,1434-1445` · `mutantes_atestado_f3.py:192-196`

### 5. Classes já reprovadas

- **Sem ocorrência nova no delta** de segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, ordem de trava ou campo persistido com dois significados. A classificação segue em `tipo_real_documento`, a reabertura segue usando `estado_inicial` e `mudar_estado`, e a trava da gravação dos seis campos segue na mesma função. `atestados_lote.py:147-175` · `ausencia.py:301-324,796-817,1403-1433`
- **Selo/RED:** os mutantes pedidos que puderam rodar morderam. O selo novo tem uma lacuna verificável: trocar o tipo do handler por `except Exception` deixou verdes o selo do corpo, o censo de tratadores e o teste de decode com mock. O selo compara o corpo, e o censo conta tratadores. A c10 efetivamente entregue conserva o par de exceções. `atestados_lote.py:151` · `test_atestado_f3_contrato.py:51-72,222-240,801-815` [nao verificado]

### 6. Quem escreve e quem lê

| Campo ou valor | Quem escreve ou produz | Quem lê ou consome |
|---|---|---|
| `Ausencia.documento`, nome e bytes | Upload entra pela view. `criar_ausencia`, `anexar_documento` e `_reabrir` passam o documento ao `save` do `FileField`. `views.py:2525-2538` · `models.py:777-781` · `ausencia.py:571-572,638-642,1118-1120,1413-1421` | Histórico lê a presença. PDF lê nome, disponibilidade e bytes. ZIP lê o original. A rota de anexo abre o documento da raiz. Reabertura e compensação leem o nome e suas referências. `dia_decidido.py:679-687` · `atestados_lote.py:168-175,249-261` · `views.py:2146-2161` · `ausencia.py:249-264,1405` |
| `linha['ausencia']`, `linha['documento']`, `registro.pk`, `pode_ver_anexo` | O ORM fornece a ausência e o ID. `linha_do_tempo` monta a linha e a flag booleana. A view calcula a permissão. `atestados_lote.py:36-49,58-63` · `dia_decidido.py:679-687` · `views_atestado.py:99-105` | PDF usa o ID no resumo, link e marca, e as duas flags para decidir se lê o anexo. A reabertura usa o mesmo ID no aviso. `atestados_lote.py:176-178,190-216` · `ausencia.py:1437-1441` |
| `arquivo`, `tipo`, `original`, `foto`, retorno `None` | `_anexo_no_pdf` abre o stream. `tipo_real_documento` fornece o tipo pelos bytes. Pillow abre, carrega e transpõe. `_foto_no_pdf` produz a imagem ou `None`. `atestados_lote.py:147-158,168-175` · `ausencia.py:789-817` | `_foto_no_pdf` usa o tipo para escolher JPEG ou PNG. `_anexo_no_pdf` propaga `None`. `pdf_bytes` lê esse valor para gerar a marca e incrementar o contador. `atestados_lote.py:153-158,172-175,210-216` |
| Marca, `anexos_ilegiveis` e bytes do PDF | `pdf_bytes` produz a marca e incrementa no mesmo ramo. O rodapé imprime o total. `atestados_lote.py:201-214,225-234` | ReportLab consome os blocos e o callback. A view envia os bytes. Os testes leem a marca, a imagem e o rodapé. `atestados_lote.py:231-234` · `views_atestado.py:134-150` · `test_atestado_f3_contrato.py:809-815,844-850` | [nao verificado]
| `banco`, `in_atomic_block`, `_committed`, `_nome_antes`, `anexo_gravado` | O router fornece o alias. A conexão acompanha o atomic. O `save` do `FileField` grava o upload, e `_reabrir` captura o nome anterior e a dupla `(storage, nome gravado)`. `ausencia.py:1405-1421` · `views.py:2569-2579` | `_reabrir` lê a dupla e o estado da conexão para escolher compensação ou aviso. A compensação lê o nome anterior e consulta as referências ao nome gravado. `ausencia.py:249-264,1434-1442` |
| Aviso e flags de retorno | `_reabrir` emite o aviso pelo logger `ponto` e marca a instância no caminho de sucesso. `ausencia.py:49,1439-1445` | A configuração SaaS encaminha o logger ao handler. Os testes capturam o aviso e leem `_reaberta`. A API e `garantir_ausencia` leem `_criada`. `saas.py:160-166,181` · `test_atestado_f3_contrato.py:503-538` · `api/views.py:1852-1858` · `ausencia.py:690-691` |
| Seis campos `atestado_*` e `documento_referencia_id` | O formulário fornece os seis valores. A criação grava no INSERT. Edição, anexação e reabertura passam pelo gravador comum. O vínculo é declarado no modelo. `forms.py:13-22` · `views_atestado.py:31-50` · `ausencia.py:301-324,567,640-642,744-746,1114-1115,1409-1411` · `models.py:815-819` | O gravador lê os valores atuais e a referência para recusar a gravação na filha. PDF, CSV e painel leem os seis campos. O vínculo também é lido pela rota que serve o anexo. `ausencia.py:292-324` · `atestados_lote.py:104-108,122-138,205-209` · `painel_ausencia.html:100-131` · `views.py:2146-2149` |

## 26/11 17:35 · sessao 593de0114757 · - · - · origem=Ronald (tmux)

A quarta cura funciona para `Exception` dentro de `_reabrir` quando houve upload e a conexão continua em `atomic`. Os dois mutantes c10 do aviso caíram nos asserts de sucesso e falha, respectivamente. O `green_c10` passou os quatro testes. `app/ponto/services/ausencia.py:1413-1445`, `app/ponto/tests/test_atestado_f3_contrato.py:503-538`, `app/ponto/tests/mutantes_atestado_f3.py:134-139,387-388`.

**Regressão nova.** Injetei `KeyboardInterrupt` em `mudar_estado` após o upload, com rollback externo simulado. O novo arquivo ficou sem referência e sem aviso na c10. O mesmo caso, com o `finally` da c9 restaurado em memória, emitiu o aviso. A diferença vem do alcance de `except Exception`. Alcance em produção [nao verificado]. `app/ponto/services/ausencia.py:1417-1442`, `app/ponto/tests/test_atestado_f3_contrato.py:409-431`, `app/ponto/tests/mutantes_atestado_f3.py:134-136`.

**Rollback posterior.** Também reproduzi em memória o retorno bem-sucedido seguido de exceção no chamador. O rollback restaura a referência antiga. A instância devolvida ainda aponta ao nome novo e tem `_reaberta=True`. Sem compensação do chamador, o novo arquivo fica sem referência e sem aviso. No caminho INSS, o chamador tenta compensar se recebeu a instância antes de falhar. Se `_reabrir` falhou antes do retorno, `ausencia` continua `None` e essa compensação é pulada. `app/ponto/services/ausencia.py:1413-1445`, `app/ponto/views.py:2564-2601`.

**Selo do “só”.** Removi em memória apenas `and conexao.in_atomic_block` do aviso. Os oito testes V1 passaram mesmo com avisos falsos em falhas sem `atomic` externo. Os testes novos mordem o aviso no sucesso externo e sua falta na falha externa. `app/ponto/services/ausencia.py:1435-1441`, `app/ponto/tests/test_atestado_f3_contrato.py:433-451,503-556`, `app/ponto/tests/mutantes_atestado_f3.py:67-74`.

**Leituras e escritas.** O upload chega pela porta de criação ou reversão. `_reabrir` lê o nome antigo e o alias do router, obtém a conexão, grava `documento` e observa a mudança de `_committed` para guardar `(storage, name)`. O `except` lê a tupla, `in_atomic_block` e o `pk`. O auxiliar lê a referência SQL e usa o storage para compensar. O logger `ponto` emite o aviso, e o handler SaaS o direciona a `ponto.log`. `_criada` é lido por `garantir_ausencia` e pela API. `_reaberta` é lido pelos testes. `app/ponto/services/ausencia.py:49,241-264,470-476,511-512,690-691,1384-1445`, `app/ponto/models.py:777-780`, `app/config/settings/saas.py:160-166,181`, `app/api/views.py:1852`, `app/ponto/tests/test_atestado_f3_contrato.py:510,535-537`.

AST c9 → c10 conforme o delta informado e a contagem da cópia, em `ExceptHandler/suppress`. `atestados_lote.py` 3/0 → 3/0. `ausencia.py` 13/0 → 13/0. `mutantes_atestado_f3.py` e `test_atestado_f3_contrato.py` 0/0 → 0/0. O `finally` removido é o externo. `app/ponto/services/atestados_lote.py:151,263,271`, `app/ponto/services/ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818`, `app/ponto/tests/mutantes_atestado_f3.py:99-139,350-419`, `app/ponto/tests/test_atestado_f3_contrato.py:51-72,228-240,503-538,801-850`.

## 26/11 17:35 · sessao 867e29256908 · - · - · origem=Ronald (tmux)

- **Load explícito** em `app/ponto/services/atestados_lote.py:147–152`. O teste sem dependências `app/ponto/tests/test_atestado_f3_contrato.py:801–815` derrubou o mutante composto de `app/ponto/tests/mutantes_atestado_f3.py:108–125` com `OSError: truncado`. Remover só `load()` derrubou o selo estrutural na linha 239, enquanto o teste funcional ficou verde porque o mock de `exif_transpose` também chama `load()` na linha 764. O teste real e seu mutante pararam no import de PIL na linha 822. Comportamento com Pillow real [nao verificado].
- **Escopo do try** confirmado em `app/ponto/services/atestados_lote.py:147–158`. O `with` inclui a entrada e a saída do contexto, além de `Image.open`, `load` e `exif_transpose`. O teste em `app/ponto/tests/test_atestado_f3_contrato.py:228–240` derrubou `c10_pdf_thumbnail_no_try` de `app/ponto/tests/mutantes_atestado_f3.py:126–129`. Trocar o tipo da exceção por `Exception` sobreviveu a esse selo, ao censo de tratadores e ao PDF simulado. O selo confere o corpo e a quantidade de tratadores em `app/ponto/tests/test_atestado_f3_contrato.py:51–72,222–240`.
- **Paridade marcador e rodapé** em `app/ponto/services/atestados_lote.py:201,210–214,225–232`. O teste em `app/ponto/tests/test_atestado_f3_contrato.py:810–815` derrubou a marca duplicada e o incremento zero de `app/ponto/tests/mutantes_atestado_f3.py:131–142`. Cada parágrafo de falha tem um incremento. Um campo `atestado_medico` contendo `ANEXO ILEGIVEL` produz a ocorrência textual com total zero, como demonstrado no PDF simulado. O campo é texto livre em `app/ponto/forms.py:14` e entra em `app/ponto/services/atestados_lote.py:207–214`.
- **Contraexemplos do PDF** executados em memória com o helper de `app/ponto/tests/test_atestado_f3_contrato.py:747–790`. `exists=True`, seguido de `FileNotFoundError` no `documento.open`, aborta o lote em `app/ponto/services/atestados_lote.py:169–175,211`. Uma imagem cujo `load` e cuja transposição terminam com sucesso ainda vira `ILEGIVEL` se a saída do contexto de `Image.open` lançar `OSError` em `app/ponto/services/atestados_lote.py:147–152,174–175,211–214`. `exists=False` imprime `Anexo indisponível`, e tipo desconhecido segue para link em `app/ponto/services/atestados_lote.py:169–178`, com detecção em `app/ponto/services/ausencia.py:789–817`.
- **Aviso da reabertura** em `app/ponto/services/ausencia.py:1434–1445`. Os testes de `app/ponto/tests/test_atestado_f3_contrato.py:503–538` derrubaram os mutantes de aviso no `finally` e de aviso removido definidos em `app/ponto/tests/mutantes_atestado_f3.py:134–138`. Um chamador pode receber o objeto reaberto e depois reverter o banco, deixando o novo arquivo sem referência e sem aviso dessa função. A view INSS tem compensação própria para exceção após o retorno, mas não consegue fazê-la quando `_reabrir` levanta antes da atribuição. `app/ponto/services/ausencia.py:1412–1421,1434–1445`, `app/ponto/views.py:2564–2579,2588–2601`. O teste de integração preexistente cobre o segundo caso em `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164–186`, execução com banco [nao verificado].
- **AST** em c9→c10, pelo delta fornecido e AST da cópia. `app/ponto/services/atestados_lote.py:151,263,271` tem 3→3 handlers. `app/ponto/services/ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818` tem 13→13. `app/ponto/tests/mutantes_atestado_f3.py:119` e `app/ponto/tests/test_atestado_f3_contrato.py:221` têm 0→0. Zero `suppress` nos quatro.
- **Origem e leitura do PDF.** `Ausencia.documento` é `FileField` em `app/ponto/models.py:777`, escrito pela criação em `app/ponto/services/ausencia.py:571–572,608,640–641`, pelo anexo em `app/ponto/services/ausencia.py:1118–1120` e pela reabertura em `app/ponto/services/ausencia.py:1413–1421`. `dia_decidido` produz o booleano da linha em `app/ponto/services/dia_decidido.py:679–687`. O PDF lê o booleano, o `FileField`, seu nome, storage e bytes em `app/ponto/services/atestados_lote.py:168–175,210–214`. O tipo sai de `app/ponto/services/ausencia.py:789–817`, a foto sai de `app/ponto/services/atestados_lote.py:148–158`, e o renderizador consome foto, marca e contador em `app/ponto/services/atestados_lote.py:211–234`. O endpoint consome o PDF em `app/ponto/views_atestado.py:134–148`. A rota do link lê o arquivo em `app/ponto/views.py:2144–2161`.
- Os quatro testes de `green_c10` e os treze de `green_c9` passaram com o executor somente em memória de `app/ponto/tests/mutantes_atestado_f3.py:350–370,385–390,410–419`. O RED histórico autenticado por Git e a execução com Pillow real [nao verificado].

## 26/11 17:34 · sessao 0457b6427136 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

**B2-chão fica como pergunta de lei.** A proteção do julgamento em `registrar` e `anotar` ficou fora da F4d. [registro_batida.py:207](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:207)

O diff está na cópia sobre `57aa0cb1b`. A execução local da suíte teve **25 testes sem falha e 1 erro de importação**. O RED→GREEN de B2 com o `atomic` real do Django e a conexão dublada ficou **[nao verificado]**. [test_juiz_unico_eventos.py:318](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:318)

| Item | Cura | RED → GREEN e mutante |
|---|---|---|
| B1 | `trava_na_ordem` é a primeira instrução do `atomic`, antes de ler os itens e de corrigir ou remover a Batida. [regularizacao_ext.py:122](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/chamados/services/regularizacao_ext.py:122) | Faltou `call.travar_colaborador(1)` nos dois ramos da base. Ambos passaram na cópia. Mutante com RC 1, `FAIL=2 ERROR=0`. [teste:409](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:409) | [nao verificado]
| B2 | A leitura viva roda no `atomic` externo, antes do `delete` e da trilha. A proteção do recálculo fica depois do `delete`. [registro_batida.py:328](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:328) | Teste invertido. Na base, na cópia e no mutante, o teste parou em `ModuleNotFoundError: No module named 'django.db'`. A sonda em memória com `sqlite3.DatabaseError` e `nullcontext`, sem conexão, observou na base `retorno=1, pks=[1], trilha=1` e na cópia `erro=DatabaseError, pks=[1, 2], trilha=0`. [teste:318](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:318) | [nao verificado]
| B3 | O INFO registra a contagem devolvida pelo escritor. A prosa de K3 descreve a exceção que sobe ao callback. [fechamento.py:1817](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1817), [fechamento.py:1841](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1841) | `'processados=0' not found` na base. A cópia registrou `processados=0` em INFO com uma chamada ao escritor para dois dias da mesma competência. Mutante com RC 1, `FAIL=1 ERROR=0`. [teste:474](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:474) | [nao verificado]
| E5 | Saiu do comentário a promessa de recálculo pelo cron. [registro_batida.py:192](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:192), [crons.py:1148](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/config/crons.py:1148) | `'cron da madrugada rega o gravado' unexpectedly found` na base. O teste passou na cópia. Mutante com RC 1, `FAIL=1 ERROR=0`. [teste:485](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:485) | [nao verificado]

Comandos da suíte, do RED contra a base literal e dos mutantes, a partir de `app/`. O mutante histórico `f4b-a-sem-protecao` cobre a reintrodução apenas do savepoint e também parou no import de `django.db`. [juiz_unico_eventos_mutantes.py:121](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/juiz_unico_eventos_mutantes.py:121) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest ponto.tests.test_juiz_unico_eventos
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-base
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b1-sem-trava
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b2-protecao-e-savepoint
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4b-a-sem-protecao
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b3-info-antigo
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-e5-comentario-antigo
```

**Contagem no diff contra a base:** `0` `except` novos e `0` `with suppress(...)` novos. A inversão de B2 removeu o `suppress` preexistente no teste. [test_juiz_unico_eventos.py:348](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:348) [nao verificado]

**Fora da volta:** a fila do opus sobre o escritor mensal, a corrida entre aprovar e rejeitar e a escrita direta de Batida pela regularização. Ocorrência e impacto em produção **[nao verificado]**. [fechamento.py:1326](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1326), [views.py:475](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/views.py:475), [regularizacao_ext.py:137](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/chamados/services/regularizacao_ext.py:137)

**Mensagem sugerida**

```text
OBRA: DIA-DA-BATIDA-JUIZ-UNICO — F4d

LEI-AKITA: padrao=juiz unico, juizes novos=0
```

## 26/11 17:30 · sessao 0d996969111a · O30-F3-CONSTRUIR · construtor · origem=Code

A c10 está na cópia. `_foto_no_pdf` chama `original.load()` dentro do `with Image.open` e do `try` existente. `_reabrir` emite o aviso de possível anexo órfão quando há falha após gravar o anexo e a conexão ainda está em `atomic` externo. [atestados_lote.py:147] [ausencia.py:1434]

### RED → GREEN

Execute em `app/` com `PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 <caso>`. `green_c10` passou os quatro testes focados. O módulo de contrato passou 33 testes locais, excluído o PDF real. `selos` passou 14. A certificação fica com o Code e o opus final. [mutantes_atestado_f3.py:373] [test_atestado_f3_contrato.py:1] [nao verificado]

| Caso | Resultado local |
|---|---|
| `red_c9_pdf_try` | RED na base c9 pela ausência de `original.load()`. [test_atestado_f3_contrato.py:228] |
| `c10_pdf_sem_load_transpose_fora_do_try` | RED com `OSError: truncado` escapando no teste simulado. [mutantes_atestado_f3.py:120] [test_atestado_f3_contrato.py:801] | [nao verificado]
| `c10_pdf_thumbnail_no_try` | RED no selo AST quando `thumbnail` entra no `try`. [mutantes_atestado_f3.py:126] [test_atestado_f3_contrato.py:228] |
| `c10_pdf_marca_duplicada` e `c9_pdf_engole_sem_contar` | RED com duas marcas no primeiro mutante e rodapé `0` para uma marca no segundo. [mutantes_atestado_f3.py:131] [test_atestado_f3_contrato.py:809] |
| `red_c9_reabrir_aviso_sucesso` e `c10_reabrir_aviso_no_finally` | RED no `assertNoLogs` pelo aviso após sucesso. [mutantes_atestado_f3.py:93] [mutantes_atestado_f3.py:134] [test_atestado_f3_contrato.py:503] |
| `c10_reabrir_sem_aviso_na_falha` | RED no `assertLogs` pela ausência do aviso na falha. [mutantes_atestado_f3.py:137] [test_atestado_f3_contrato.py:515] |

**PDF real [nao verificado].** `green_c10_pdf_real` e `c10_pdf_real_sem_load_transpose_fora_do_try` pararam no import de `PIL` nesta cópia. Os dois comandos ficam para o container da suíte. O teste gera um JPEG, corta os bytes pela metade, exige que `Image.open` abra e que `load()` levante `OSError`, e confere uma marca `ANEXO ILEGIVEL #21`, rodapé `1` e a foto boa no PDF. [test_atestado_f3_contrato.py:821] [mutantes_atestado_f3.py:123] [mutantes_atestado_f3.py:389] [nao verificado]

### Diff e censo

`git diff --name-only 1e4723516 -- app/` listou os quatro arquivos abaixo. `git diff --check` passou. A contagem é por AST, de `1e4723516` para c10. [test_atestado_f3_contrato.py:49]

| Arquivo | `ExceptHandler` | `with suppress` |
|---|---:|---:|
| `app/ponto/services/atestados_lote.py` [atestados_lote.py:147] | 3 → 3 | 0 → 0 |
| `app/ponto/services/ausencia.py` [ausencia.py:1434] | 13 → 13 | 0 → 0 |
| `app/ponto/tests/mutantes_atestado_f3.py` [mutantes_atestado_f3.py:119] | 0 → 0 | 0 → 0 |
| `app/ponto/tests/test_atestado_f3_contrato.py` [test_atestado_f3_contrato.py:228] | 0 → 0 | 0 → 0 |

- **Leitores.** A rota do PDF chega a `views_atestado.lote_atestados`, que chama `pdf_bytes`. O lote lê o documento e os seis campos. `views.py` serve o documento da raiz, o painel exibe o anexo e os campos, e `anexar_atestado` coleta os valores do formulário. [relatorios/urls.py:17] [views_atestado.py:136] [atestados_lote.py:161] [atestados_lote.py:205] [views.py:2147] [painel_ausencia.html:71] [painel_ausencia.html:95] [views_atestado.py:29]
- **Escritores.** A catraca identifica `criar_ausencia` e `_gravar_dados_atestado` como os escritores dos seis campos. `_reabrir` grava documento, descrição e prazo e delega a transição de estado a `mudar_estado`. `anexar_documento` grava o documento e chama a porta dos seis campos. `_apagar_anexo_escrito_agora` executa a remoção. [test_atestado_f3_contrato.py:74] [test_atestado_f3_contrato.py:169] [ausencia.py:317] [ausencia.py:567] [ausencia.py:1114] [ausencia.py:1411] [ausencia.py:241]
- **Fora do escopo.** B1b, B2a, B2c, B5 e INSS seguem em fila, assim como o possível órfão no rollback posterior do `atomic` externo, o nome anterior defasado, a corrida entre `.exists()` e `storage.delete`, o erro em `documento.open('rb')` e a cópia via `DadosAtestadoForm.base_fields`. Banco e produção ficam [nao verificado]. [views.py:2569] [ausencia.py:256] [ausencia.py:262] [atestados_lote.py:171] [forms.py:13]

**Mensagem de commit sugerida** [atestados_lote.py:149] [ausencia.py:1434] [nao verificado]

```text
OBRA: O30-F3-CONSTRUIR
LEI-AKITA: padrao=escritor unico

Explicita a decodificação no PDF e limita o aviso de órfão à falha da reabertura.
```

## 26/11 17:26 · sessao 2ad7899ff274 · ABONO-X-SAIDA-ANTECIPADA · verificador · origem=Code

**Veredito da volta c1e — reprova.** O diff do motor respeita os sítios autorizados, mas a parcial e o pareador podem dar dias diferentes ao mesmo intervalo. A lista também pode publicar `0/30` para uma ausência que já abateu 30 minutos na jornada. A ordem de invalidação pode inverter a aquisição das travas de fechamento. `app/ponto/services/credito_parcial.py:100–114` · `app/ponto/turnos.py:257–260,1310–1314` · `app/ponto/motor_calculo_v2.py:1237–1293` · `app/ponto/services/ausencia.py:183–205` · `app/ponto/services/fechamento.py:1672–1680`

Os oito blobs da cópia conferem com os índices finais do diff anexado. A cópia veio sem metadados Git, portanto a identidade do objeto do commit e a sequência dos commits ficam **[nao verificado]**. Os contraexemplos abaixo são de código ou de execução sintética em memória. Incidência em produção **[nao verificado]**. [nao verificado]

### 1. Escopo do `!` — nada

No diff fornecido, o motor muda em dois sítios, `_minutos_da_ponta_cobertos` e o bloco da parcial em `_aplicar_teto_pontualidade`. `_parciais_do_dia` já consulta a porta cujo significado mudou. Seu corpo ficou igual no diff. `app/ponto/motor_calculo_v2.py:453–485,1222–1305,1344–1361`

### 2. Escritores, leitores e unidades — média

| Campo ou retorno | Quem escreve e quem lê |
|---|---|
| Chave de `parciais_com_ponta_por_dia` | `credito_parcial` primeiro reúne ausências pela data civil `a.data`, depois escreve `{dia_da_jornada: [Ausencia]}`. O motor lê `.get(dia)` e memoiza por `(colaborador, dia)`. `rubricas_por_dia` lê `.get(k)` e passa a lista ao contexto do teto. Os testes também leem a chave diretamente. `app/ponto/services/credito_parcial.py:15–32,62–81` · `app/ponto/motor_calculo_v2.py:1344–1361` · `app/ponto/calculador/alimentacao.py:510–539` · `app/ponto/calculador/regras.py:117–127` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:380–392` |
| `dia`, `ini_min`, `fim_min`, `adiante` | O teto agrupa pelo mapa recebido e passa `dia` como a **data da jornada**. As pontas são minutos de relógio desde a meia-noite local desse dia, com fração pelos segundos das batidas. O leitor `_minutos_da_ponta_cobertos` traduz cada minuto para a meia-noite civil de `a.data` e consulta o juiz do período por meio de `batida_dentro_da_parcial`. As chamadas efetivas usam `adiante=0`, por default. A simulação da lista usa `1`, em **dias civis**, traduzido em 1.440 minutos. Os demais chamadores diretos são os testes. `app/ponto/motor_calculo_v2.py:453–485,1132–1138,1233–1277` · `app/ponto/turnos.py:2683–2692,2714–2723` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:438–467` |
| `dia`, `causa` | O teto grava a data da jornada e o texto da causa. Nesse ramo a data também coincide com `a.data` por exigência da guarda. A deduplicação lê `dia`. E6 projeta o campo em conjunto de dias. A lavra grava `data` ISO, recorta a competência e conta `causa`. `janela_he` e o resumo do espelho filtram por causa. O fechamento concatena o item inteiro. Os diffs leem a lista, com o problema de classificação do item 6. `app/ponto/motor_calculo_v2.py:1267–1293` · `app/ponto/management/commands/e6_oraculo.py:162–172` · `app/ponto/services/cxr_na_lista.py:84–127,168–187` · `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:748–755,1295–1297` · `app/ponto/services/fechamento.py:116–117,314–317` |
| `ausencia_id`, `ponta`, `periodo`, os dois `abate_*_min` | O teto grava o PK da ausência, a rubrica da ponta, `HH:MM-HH:MM` do cadastro e dois inteiros em minutos de relógio. A deduplicação lê PK e ponta. A lavra copia os cinco campos, preservando os inteiros, e `_cxr_do_motor` os converte em texto. HTML e PDF leem `detalhe`. O calculador também transporta o item em `Rubricas.detalhe['cadastro_x_realidade']`. `DiaPago.lavrar` consome as rubricas, sem copiar esse detalhe. Os testes leem os campos no resultado e na lavra e verificam o texto da C12. `app/ponto/motor_calculo_v2.py:1285–1293` · `app/ponto/services/cxr_na_lista.py:32–47,87–92` · `app/escala/services/cadastro_realidade.py:455–469,1323–1332` · `app/templates/escala/cadastro_x_realidade.html:31` · `app/ponto/calculador/regras.py:281–285,865–884` · `app/ponto/services/dia_pago.py:322–333` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:307–334` |
| Retorno de `vespera_do_cadastro` | A função extrai `(hi, hf)` do DNA da véspera ou das duas bordas do template, ou devolve `None`. São horários locais de cadastro. Os dois leitores são `_vespera`, que passa a resposta a `_mk`/`_data_do_turno`, e `dia_da_jornada_da_parcial`, que passa a resposta diretamente a `_data_do_turno`. O juiz transforma as bordas em comparações com instantes. `app/ponto/turnos.py:216–231,248–260,300–338,1310–1314,1501–1508` · `app/ponto/services/credito_parcial.py:102–114` |
| Retorno de `dia_da_jornada_da_parcial` | `_data_do_turno` escreve a resposta, uma `date` local igual ao dia civil do início ou à véspera. O mapa de parciais usa essa data como chave. `_acordar_projecao` usa a menor data entre ela e a borda civil para invalidar o intervalo. O teste da véspera lê o retorno diretamente. `app/ponto/turnos.py:300–324` · `app/ponto/services/credito_parcial.py:74–80,100–114` · `app/ponto/services/ausencia.py:192–205` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:380–392` |

**Fuso e significado.** A medição da ponta usa o fuso fixo de `settings.TIME_ZONE`. A construção dos instantes e o juiz usam o fuso corrente do Django. O padrão é `America/Sao_Paulo`. A igualdade depende do fuso ativo ser o mesmo. Os dois nomes de abatimento sugerem efeito monetário, mas os números medem sobreposição bruta, conforme o item 9. `app/ponto/motor_calculo_v2.py:28,73–79,1236–1255,1273–1292` · `app/ponto/services/credito_parcial.py:109–114` · `app/ponto/turnos.py:45–48,314` · `app/config/settings/base.py:93–95`

### 3. Juiz e carga — alta

**A decisão do dia é toda de `_data_do_turno`.** O adaptador monta início e fim e devolve diretamente a resposta do juiz. Ele próprio repete a conversão `HH:MM` e a virada `fim < início`, que já existem no juiz do período. A decisão de dia continua única. A derivação da virada tem duas implementações. `app/ponto/services/credito_parcial.py:108–114` · `app/ponto/turnos.py:300–324,2683–2689` · `app/core/juizes.py:216,266–277`

**A carga pode mudar a resposta.** A parcial carrega o vínculo do dia civil e lê o DNA direto numa janela em torno desse dia. `_turnos_do_juiz` usa o template recebido ou o da instância e o mapa de células da janela ou da fatia. `turnos_de_batidas` ainda copia as batidas e marca ecos. Com template `19:00–07:00`, sem DNA na janela curta e com DNA apenas em outro dia da janela longa, `01:00–03:00` pode cair na véspera pela carga curta e no dia civil pela longa. O mapa longo é não vazio e, sem a chave da véspera, devolve `None`. Uma troca de vínculo na borda também pode mudar o template e a decisão de consultar DNA. `app/ponto/services/credito_parcial.py:100–107` · `app/ponto/motor_calculo_v2.py:902–917` · `app/escala/utils.py:84–108` · `app/ponto/turnos.py:257–260,300–324,1529–1547,1846–1859` · `app/ponto/services/fechamento.py:291–311`

**A diferença de fatos também muda o dia.** O pareador veta a véspera quando já atribuiu a ela um turno encerrado naquele dia. O teste existente fixa `16:00–20:00` em D e `03:00–07:30` em D+1, com template `19:00–10:00`, e mantém o segundo par em D+1. Uma parcial sintética com os horários do segundo par, sob o mesmo template e sem DNA, recebe D pela consulta direta de cadastro. `app/ponto/turnos.py:216–231,314–324,1268–1278,1310–1314` · `app/ponto/services/credito_parcial.py:107–114` · `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:33–36`

### 4. Extração de `_vespera` — nada

Para o pareador, a extração preserva referência pela entrada ou saída, cálculo da véspera local, veto pelo turno já pareado e os mesmos dois ramos de cadastro. `_bm_all` continua chegando com os valores vazios retirados. Mapa presente sem a véspera continua devolvendo `None`. `app/ponto/turnos.py:248–260,1233–1239,1268–1278,1310–1314` · `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:33–45`

### 5. Chave dos leitores e bordas da competência — alta

- **O mapa novo resolve as duas bordas dentro do seu próprio contrato.** Lê datas civis até `fim+1`, depois filtra pela jornada. `ini` atribuída a `ini−1` sai do recorte corrente e pode entrar no anterior. `fim+1` atribuída a `fim` entra no recorte corrente. A parcial entra em uma chave, e o crédito de atraso exclui as parciais com período. Isso evita dupla contagem entre o teto e esse crédito. A aprovação invalida a competência da jornada e a civil, zerando o carimbo existente. `app/ponto/services/credito_parcial.py:46–50,62–81` · `app/ponto/turnos.py:300–324` · `app/ponto/services/ausencia.py:186–205,1133–1134` · `app/ponto/services/fechamento.py:1672–1680`
- **Motor e calculador pedem a mesma espécie de chave, mas o motor pode mudar seu valor entre passadas.** A base pode herdar o dia por um gap de até seis horas. Partido e comercial montam outro mapa a partir do juiz, e o calculador usa o mapa final. Se só o dia mudar, `_pontas_parcial` pode pular o novo julgamento porque guarda apenas atraso e saída. Assim, o abatimento pode ficar da primeira chave enquanto o período passa a outra. `app/ponto/motor_calculo_v2.py:1125–1127,1237–1241,1338–1342,2558–2567,3093–3104,3357,3488–3490,3671` · `app/ponto/calculador/alimentacao.py:239–262,510–539`
- **Grade, espelho e PDF ainda têm leituras civis da parcial com período.** A grade procura a cobertura por `a.data`, então pode classificar batidas e dispensar marcos na linha D+1 enquanto o teto abate a ponta em D. A ata recebe essa classificação. O ícone e a cobertura do espelho ficam na linha civil, e o E6 pula a linha marcada com ausência. No PDF, `efeitos_da_ausencia` soma `a.minutos` na data civil no resumo bruto, que pode ser substituído pelo gravado. O fechamento e o `DiaPago` também recebem o abono da grade, calculado como previsto menos coberto na linha de ausência. São efeitos que podem cair em duas competências. Isso, por si, não prova desconto abatido duas vezes. `app/ponto/turnos.py:2849–2851,2868–2879` · `app/escala/utils.py:1230–1233,1246–1252,1272–1295` · `app/ponto/services/cartorio.py:782–790` · `app/ponto/services/espelho.py:593–602,863–890` · `app/ponto/management/commands/e6_oraculo.py:339–341` · `app/ponto/services/efeito_ausencia.py:46–50` · `app/relatorios/pdf_espelho.py:332–336,479,628` · `app/relatorios/cartao_pela_celula.py:320–346` · `app/ponto/services/fechamento.py:1084–1088` · `app/ponto/services/dia_pago.py:386–401`
- **`DiaPago` pode gravar atraso residual fora da competência da jornada.** O fechamento recebe a pontualidade já abatida, mas `atraso_por_dia` agrupa pelo dia civil da entrada. `DiaPago` usa esse mapa para atraso e o dia da jornada para saída. Uma entrada de madrugada atribuída pelo motor à véspera pode, assim, deixar atraso residual em `fim+1` numa linha gravada para a competência de `fim`. A gravação aceita a chave sem recortar de novo. O oráculo de rubricas usa a jornada e substitui valores por suas próprias chaves. `app/ponto/services/fechamento.py:1018–1026,1224–1241,1293–1312` · `app/ponto/services/credito_parcial.py:117–131` · `app/ponto/services/dia_pago.py:173–195,322–330,340–360`

### 6. Publicação e leitores da lista — alta

- **Pode publicar parcial que já abateu.** `_pontas` nasce no laço de cada par. Reproduzi em memória uma jornada com dois pares e parcial D `06:30–19:30`. Ela eliminou 30 minutos de atraso do primeiro par e gerou `0/30` na saída antecipada do segundo. O filtro vê somente o segundo par quando decide publicar. Também há omissão pelo filtro: se o juiz mandar a parcial datada D para D−1, o laço de D nunca a examina para o relato de D. `app/ponto/motor_calculo_v2.py:1237–1293` · `app/ponto/services/credito_parcial.py:74–80` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:344–351,380–392`
- **A mesma instância deduplica por `(dia, ausência, ponta)`, mantendo o primeiro número.** Uma passada posterior pode reescrever a ponta, como faz a Zona 5, sem atualizar ou retirar a linha publicada na primeira. A composição concatena as listas das fatias. Na lavra, só dicionários inteiramente iguais são deduplicados. Duplicação efetiva entre fatias em produção **[nao verificado]**. O calculador usa seu próprio contexto e devolve o relato em `Rubricas.detalhe`. Já a rota de `Censo.cxr` chama a pontualidade sem `parciais=`. `app/ponto/motor_calculo_v2.py:1282–1293,2442,2877–2878,3235–3242,3357,3671` · `app/ponto/services/fechamento.py:314–317` · `app/ponto/services/cxr_na_lista.py:107–113` · `app/ponto/calculador/regras.py:281–285,865–876` · `app/ponto/calculador/periodo.py:474–477,786–793`
- **Os leitores aceitam a forma nova com efeitos diferentes.** `janela_he` e o resumo de HE do espelho excluem a nova causa corretamente. E6 e `cxr_lavrado` projetam presença por dia. O dono continua sendo decidido por dia, e `BATIDA` pode vencer a presença na lista. A C12 preserva os dois números, mas a legenda afirma que o horário real ficou longe do cadastrado e a ação encaminha para template ou vínculo, embora o item descreva a data da ausência. `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:1295–1297,1330–1331` · `app/ponto/management/commands/e6_oraculo.py:98–110,162–172,182–195` · `app/ponto/services/cxr_na_lista.py:168–187` · `app/escala/services/cadastro_realidade.py:133–134,455–469,783–787,1105–1123`
- **Os dois diffs classificados como L-084 ficam semanticamente errados.** `diff_janela_he` conta qualquer item como fora da janela pela L-084. `diff_reclassificar_partido` conta qualquer item como dia-colab que deixou de gerar desconto pela L-084 e pode imprimir deltas `None` quando a amostra é o item novo. `app/ponto/management/commands/diff_janela_he.py:63–70,131–134` · `app/ponto/management/commands/diff_reclassificar_partido.py:186–190,259–273`

### 7. Classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Nada para o dia.** O adaptador chama `_data_do_turno`. A diferença está na carga. `app/docs/JUIZES.md:10–12,19–25` · `app/core/juizes.py:216` · `app/ponto/services/credito_parcial.py:100–114` |
| Segundo escritor fora da porta | **Nada.** O período segue escrito por `decidir`, a invalidação segue pela porta existente e a C12 segue gravada pela lavra existente. `app/ponto/services/ausencia.py:1061–1064,1089–1091,1111–1114,1133–1134` · `app/ponto/services/fechamento.py:1672–1680` · `app/ponto/services/cxr_na_lista.py:145–152` |
| Relógio fora da borda | **Nada novo.** A leitura de `localdate()` da linha em aberto antecede o diff. O comando continua lendo `agora` na borda. `app/ponto/services/ausencia.py:189–193` · `app/ponto/management/commands/lavrar_cxr_do_motor.py:27–32` |
| Derivação paralela | **Alta.** Há segunda carga de template/DNA e repetição da virada do período. A invalidação também usa um universo mais largo que o teto: basta ter as duas horas. Uma edição pode preservar essas horas ao mudar tipo, minutos ou vigência, e invalidar a véspera para linha que o teto já exclui. `app/ponto/services/credito_parcial.py:29–31,74–80,100–114` · `app/ponto/turnos.py:2683–2689` · `app/ponto/services/ausencia.py:193–205,656–680` |
| Selo/RED sem mordida | **Média.** A catraca morde composição, sítios e linhas, dentro de seus critérios léxicos. Ela não prova igualdade de carga nem a decisão publicada. Os testes de comportamento mordem a forma simples, com as lacunas do item 8. `app/core/tests/test_medida_turno_do_par.py:548–585` · `app/core/medida_turno_do_par.py:32–55` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:299–392` |
| Ordem de trava | **Alta.** Na edição de uma parcial de 21/10, de horário diurno para `01:00–03:00` da jornada de 20/10, o laço invalida primeiro a competência 11 pelo estado anterior, depois a 10 pelo novo intervalo, no mesmo `atomic`. O caminho de revisão de batida pode tomar as mesmas competências na ordem 10 → 11. Há inversão possível se ambos alcançarem as mesmas linhas. Ocorrência de deadlock **[nao verificado]**. `app/ponto/views.py:2835–2847` · `app/ponto/services/ausencia.py:183–205,1089–1091,1133–1134` · `app/ponto/services/fechamento.py:1623,1672–1680,1854–1876` · `app/ponto/services/dia_pago.py:702–719` |
| Pouso antes do pré-requisito | **[nao verificado].** A árvore declara suite inteira antes do opus e condições para reconstrução das projeções. O material fornecido não comprova execução, publicação do impacto ou pouso. `CLAUDE.md:392–402,418–437` · `app/docs/LEIS.md:142` |
| Campo com dois significados | **Alta.** Os campos `abate_*` medem sobreposição bruta por par, mas o texto da C12 os apresenta como abatimento. O atraso residual também conserva uma chave civil ao lado da chave de jornada usada para saída. `app/ponto/motor_calculo_v2.py:1237–1293,1306–1337` · `app/escala/services/cadastro_realidade.py:455–459` · `app/ponto/services/dia_pago.py:173–195` |
| `except`, `suppress` ou fallback | **Nada novo de captura.** A extração mantém a escolha anterior entre DNA e template no pareador. A parcial passa a usar esse ramo de template com a sua própria carga. A diferença observável é a do item 3. `app/ponto/turnos.py:248–260,1310–1314` · `app/ponto/services/credito_parcial.py:100–114` |

### 8. Mordida dos testes — média [nao verificado]

| Teste novo | O que pegaria | [nao verificado]
|---|---|
| D+1 `01:00–03:00` | Perda da parcial por recorte civil, chave errada ou falta da tradução de 1.440 minutos. Exige saída residual 240 nas quatro leituras e lista vazia para a parcial. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:79–112,136–141,299–302` |
| D `01:00–03:00` | Abatimento indevido em D+1, perda ou repetição do item nesse cenário, ou perda de dono, PK e números entre motor, lavra e formatador. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:304–334` |
| D `02:00–01:30` e D `06:30–19:30` | O primeiro pega publicação de parcial que já cobre a saída. O segundo pega uma guarda que olha a saída mas ignora abatimento do atraso **no mesmo par**. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:336–351` |
| Aprovação em 21/10 | Pega a volta da invalidação apenas civil. Verifica os dois carimbos nulos, sem verificar relavratura, rubricas nas duas competências ou concorrência. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:353–373` |
| Controle D `23:00–03:00` e véspera com turno | O primeiro pega quebra da virada e falso C12 no período que já cobre 120 minutos. O segundo pega chave fixa por `a.data` ou abandono da véspera do DNA. O segundo consulta juiz e mapa, sem comparar um pareamento real na mesma carga. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:375–392` |
| Dois testes puros novos | Isolam a tradução de D+1 e distinguem o `0` como datada do `120` com `adiante=1`. Os testes adjacentes verificam chamada e uso da resposta do juiz, bordas e segundos. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:446–501` |

**Contraexemplos faltantes.** Dois pares da mesma jornada, segunda passada que muda a ponta ou o dia, DNA fora da janela curta, troca de vínculo no corte, redatação que muda o dia no juiz, cobertura já dada por outra parcial e teto aritmético que reduz o ganho. Também falta cruzar a borda 20/21 pelas duas apurações e conferir a C12 na lista e na rota. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:299–423` · `app/ponto/motor_calculo_v2.py:1237–1337,3235–3242` · `app/ponto/services/credito_parcial.py:74–114` · `app/escala/views.py:521–537`

Em memória, passaram os **35 testes da catraca** com a adaptação documentada para `unittest`, e os **10 testes puros da ponta** com as funções reais extraídas da cópia. Os testes de integração com banco, a suite inteira e os REDs históricos ficam **[nao verificado]**. `app/core/tests/test_medida_turno_do_par.py:28–41,548–567` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:426–501`

### 9. Significado dos

[truncado em 20000 de 22099 caracteres]

## 26/11 17:25 · sessao 4d122d1eb77b · - · - · origem=Ronald (tmux)

1. **Escopo, nada.** O diff altera `_minutos_da_ponta_cobertos` e o bloco da parcial no teto. `_parciais_do_dia` permanece como leitor. Os oito blobs da cópia conferem com o `index` do diff fornecido. O objeto Git do commit está [nao verificado]. `app/ponto/motor_calculo_v2.py:453,477,1244,1273,1344`.

3. **Alta.** O retorno do dia vem exclusivamente de `_data_do_turno`. A carga difere por vínculo civil, janela pontual de DNA em vez da janela da apuração, consulta ao banco em vez de células alimentadas e ausência do veto de turno já encerrado na véspera. Em prova sem banco com cargadores simulados, parcial de 02/09 03:00–07:30 e template 19:00–10:00 retornou 01/09 com mapa pontual vazio. O par com o mesmo horário retornou 02/09 se o pareador tinha encerrado 01/09 ou possuía mapa maior com DNA somente em 07/09. `app/ponto/services/credito_parcial.py:97–114`, `app/ponto/motor_calculo_v2.py:902–917`, `app/ponto/turnos.py:248–260,300–324,1310–1314,1546–1547`.

4. **Extração, nada.** O pareador mantém a referência local, a filtragem das bordas e o veto. O helper executa os mesmos ramos para DNA não vazio e bordas do template. Os oito testes do BUG-145 passaram em memória com dublês de importação do Django. `app/ponto/turnos.py:248–260,1232–1239,1268–1278,1310–1314`, `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:23–57`.

6. **Alta.** `_b0` consulta só as pontas de um `p`, embora o comentário diga jornada. O teto real, com dois pares sintéticos do mesmo dia, liquidou 30 min de atraso em um par e publicou `0/30` para a mesma parcial na saída do segundo. A Zona 5 pode alimentar pontas separadas. `app/ponto/motor_calculo_v2.py:1237–1244,1273–1293,3185–3209,3235–3242`. A causa de madrugada também foi emitida para `08:30–09:30`, porque as guardas medem data e sobreposição após o deslocamento. `app/ponto/motor_calculo_v2.py:1267–1293`. `InsumosDoDono` reduz a lista a um conjunto de dias, e o juiz ainda pode dar BATIDA antes de consultar o conjunto. `app/ponto/management/commands/e6_oraculo.py:98–110,162–172`.

7. **Alta.** Há derivação paralela da leitura dos limites e da virada do período registrado. A cópia de `hora:minuto` e `fim < início` mora na preparação da parcial. Não há segundo predicado de dia. `app/core/juizes.py:266–277`, `app/ponto/turnos.py:2683–2689,2710–2713`, `app/ponto/services/credito_parcial.py:108–114`. A edição de uma parcial na virada pode adquirir fechamento da competência civil primeiro pelo trecho antigo e da anterior depois pelo novo. A revisão C13 percorre ambas em ordem cronológica dentro do ato. Ocorrência concorrente em produção [nao verificado]. `app/ponto/views.py:2835–2848`, `app/ponto/services/ausencia.py:183–205,1089–1091,1133`, `app/ponto/services/fechamento.py:1168,1664–1680,1854–1876`. A invalidação usa presença dos dois horários como universo, enquanto o dinheiro exige tipo pago e parcial vigente. A edição ordinária pode preservar os horários e alterar a condição de parcial. `app/ponto/services/ausencia.py:193–205,656–680`, `app/ponto/services/credito_parcial.py:29–31,54–59`. Segundo escritor, nova leitura de relógio e novo `except/suppress`, nada no diff. A seleção do template na falta de mapa é preexistente, com alcance diferente na carga nova. `app/ponto/turnos.py:248–260,1310–1314`, `app/ponto/services/ausencia.py:190–205`. Pouso e prerequisitos em produção [nao verificado]. Cinco testes da catraca passaram em memória, incluindo composição, números, linhas e mutante B1. O detector declara limitações lexicais. `app/core/tests/test_medida_turno_do_par.py:28–41,510–571,846–852`, `app/core/medida_turno_do_par.py:29–55`.

9. **Alta.** A guarda real é `_b0 > 0` e depois `round(_b1) <= 0`. O primeiro é a soma da cobertura isolada da parcial nas pontas brutas do par atual e vale sempre zero nos itens publicados. O segundo é a cobertura bruta isolada da ponta após deslocar a data, arredondada. Em prova sintética, uma parcial corretamente datada já abateu os mesmos 120 min anunciados para mover a incorreta. O ganho marginal seria zero. Teto diário e demais parciais ficam fora do contrafactual. `app/ponto/motor_calculo_v2.py:1244–1258,1273–1293,1306–1331`. O teste percorre command, snapshot e `_cxr_do_motor`. O HTML/PDF passam por `lista()`, harmonização de dono e seleção da ação C12. A ação de CADASTRO aponta para o template ou vínculo, apesar de a divergência nova ser da data da ausência. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:318–329`, `app/escala/views.py:512–537`, `app/escala/services/cadastro_realidade.py:455–469,783–787,1040–1058,1105–1108`, `app/templates/escala/cadastro_x_realidade.html:31`.

## 26/11 17:23 · sessao 03ced07866e8 · - · - · origem=Ronald (tmux)

**Alta.** O mapa novo é escrito por jornada, e o recorte civil inclui `fim+1` antes de filtrar `ini…fim`. Seus dois consumidores diretos são o teto do motor e o calculador de rubricas. Contudo, o motor base ainda pode herdar por gap de 6 h, enquanto partido/comercial e calculador usam o mapa final do pareador. Um segundo passe que muda o dia e preserva os valores pula a parcial porque `_pontas_parcial` guarda só atraso e saída. [app/ponto/services/credito_parcial.py:74-80, app/ponto/motor_calculo_v2.py:1233-41,1340-61,2558-67,3093-3104,3357,3488-90,3671, app/ponto/calculador/alimentacao.py:239,259-62,511-39]

**Alta.** A grade e o espelho ainda leem cobertura pela data civil. Uma parcial D+1 pode reduzir a saída da jornada D, enquanto a grade classifica a parcial e dispensa marcos na linha civil D+1, o espelho põe a ausência ali e o E6 exclui essa linha. O abono da grade também segue essa linha no fechamento e no `DiaPago`. São efeitos distintos do abatimento, que pode ocorrer em competência diversa. O PDF soma `a.minutos` da data civil no resumo bruto, substituído pelos totais gravados quando disponíveis. [app/ponto/turnos.py:2849-51,2868-79, app/escala/utils.py:1230-33,1246-52,1272-95, app/ponto/services/espelho.py:593-602,863-68, app/ponto/management/commands/e6_oraculo.py:339-41, app/ponto/services/fechamento.py:1084-88, app/ponto/services/dia_pago.py:285-92,386-401, app/ponto/services/efeito_ausencia.py:46-50, app/relatorios/pdf_espelho.py:479,628, app/relatorios/cartao_pela_celula.py:320-46]

**Alta.** Crédito para desconto exclui as parciais com período. O fechamento lê saída e atraso já abatidos, porém o mapa do atraso residual usa a data civil da entrada. Com entrada na madrugada classificada na véspera, o `DiaPago` pode guardar atraso em D+1 e saída em D, inclusive com data fora da competência gravada. O calculador agrupa por jornada. [app/ponto/services/credito_parcial.py:47-50,117-131, app/ponto/services/fechamento.py:1018-26,1224-41,1293-1318, app/ponto/services/dia_pago.py:173-95,322-30,340-60]

**Alta.** `_b0` soma as pontas do par corrente. Assim uma ausência pode abater 30 min no atraso do primeiro par e ainda publicar `0/30` na saída do segundo. O teste de atraso e saída usa um só par. O cálculo deslocado de `_b1` não verifica se a nova data conservaria a jornada. Exemplo estático com turno D 19–07, saída D+1 01:00 e parcial D 06:00–11:01. O relato mede 60 min, mas a redatação efetiva ultrapassa às 11:01 o envelope da jornada D, que fecha às 11:00. [app/ponto/motor_calculo_v2.py:1237-57,1267-92, app/ponto/tests/test_abono_parcial_abate_a_ponta.py:346-54, app/ponto/turnos.py:80-82,229-31,314-24, app/ponto/services/credito_parcial.py:107-14]

**Média.** A invalidação expande para a jornada e conserva a competência civil. Ela só exige os dois horários, enquanto a folha exige parcial paga. Um registro editado para inteiro/range conserva horários e pode invalidar a competência anterior sem ser lido pelo teto. A porta invalida o carimbo. [app/ponto/services/ausencia.py:186-205,637-44,656-80, app/ponto/services/credito_parcial.py:29-31,74-80, app/ponto/services/fechamento.py:1672-80]

**Média.** A C12 real conserva o detalhe no HTML, mas continua dizendo que o horário real ficou longe e encaminha para corrigir o template/vínculo. O teste executa a lavra e o formatador, sem passar por `lista()`, pela consolidação do dono ou pela view e renderização. [app/escala/services/cadastro_realidade.py:129-34,783-87,1042-58,1105-08,1121-23, app/ponto/tests/test_abono_parcial_abate_a_ponta.py:315-334, app/escala/views.py:521-37, app/templates/escala/cadastro_x_realidade.html:31]

**Baixa.** A medição usa minutos fracionários de relógio em `settings.TIME_ZONE`, a parcial é montada em `HH:MM` no fuso ativo do Django, e os campos da C12 são minutos inteiros. Os fusos coincidem na configuração padrão, com divergência possível se o Django estiver com outro fuso ativo. [app/ponto/motor_calculo_v2.py:28,73-79,475-84,1236-55,1292, app/ponto/services/credito_parcial.py:109-14, app/ponto/turnos.py:45-48,314, app/config/settings/base.py:93-95]

## 26/11 17:23 · sessao eb543fa7c75e · - · - · origem=Ronald (tmux)

**6. Alta — o relato pode acusar uma parcial que já abate no mesmo dia.** `_pontas` é recriada por par, e `_b0` soma só as pontas desse par. Em um exemplo sintético com dois pares, a parcial D 06:30–19:30 eliminou 30 min de atraso do primeiro par e produziu `0/30` na saída antecipada do segundo. O teste correspondente usa um único par. `app/ponto/motor_calculo_v2.py:1237–1292` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:344–351`

**6. Média — a deduplicação conserva o primeiro número.** O motor impede repetição de `(dia, ausência, ponta)` na mesma lista, mas não atualiza a entrada se a Zona 5 mudar o bruto. Reexecutei o corpo do teto em memória com bruto 360 na primeira chamada e 30 na segunda. O desconto passou a 30, e a linha permaneceu `0/120`. O fechamento concatena as listas das fatias. A lavra remove apenas cópias integralmente iguais. Incidência em produção [nao verificado]. `app/ponto/motor_calculo_v2.py:1282–1293,2442,2877–2878,3235–3242,3357,3671` · `app/ponto/services/fechamento.py:116–117,306–317` · `app/ponto/services/cxr_na_lista.py:110–113`

**6. Média — os leitores aceitam o dicionário, mas alguns atribuem a ele outra causa.**

- `InsumosDoDono.cxr()` projeta qualquer item com `dia` para um conjunto de dias. `dono()` envia a presença nesse conjunto ao juiz. Batida fora do turno e contagem ímpar podem dar `BATIDA` antes da regra genérica de presença na lista. `app/ponto/management/commands/e6_oraculo.py:98–110,162–172,182–195`
- `janela_he` exige causa contendo `janela de HE`, e o resumo do espelho exige a causa exata de chegada fora da janela. O resultado bruto continua disponível no espelho. `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:748–755,1295–1297,1330–1331`
- A lavra preserva os novos campos, converte `dia` para `data` ISO e recorta a competência. `cxr_lavrado` usa apenas as datas. A lista e o PDF exibem o detalhe, mas a legenda C12 afirma que o horário real ficou longe do cadastrado, e a ação CADASTRO aponta para corrigir o template ou abrir o vínculo. A ocorrência nova descreve a data da ausência. `app/ponto/services/cxr_na_lista.py:32–34,84–113,168–187` · `app/escala/services/cadastro_realidade.py:133–134,446–469,783–787,1105–1123,1331` · `app/templates/escala/cadastro_x_realidade.html:31`
- `diff_janela_he` conta todos os itens como fora da janela pela L-084. `diff_reclassificar_partido` conta todos como dia-colab L-084 e, se tomar o item novo de exemplo, imprime deltas `None`. `app/ponto/management/commands/diff_janela_he.py:63–70,131–134` · `app/ponto/management/commands/diff_reclassificar_partido.py:186–190,259–273`
- O calculador empresta o teto e pode produzir o item em seu contexto próprio. A rota que alimenta parciais leva esse retorno a `det['cadastro_x_realidade']`. A rota de `Censo.cxr` chama a pontualidade sem parciais. `app/ponto/calculador/alimentacao.py:511–515,537–539` · `app/ponto/calculador/regras.py:114–127,281–285,865–876` · `app/ponto/calculador/periodo.py:474–477,786–793`

**9. Alta — o segundo número é cobertura geométrica, e pode diferir do abatimento final.** O primeiro é a soma das interseções da parcial isolada com as pontas brutas do par. Para entrar na lista, essa soma precisa ser zero. A guarda efetiva pula `_b0 > 0` e `round(_b1) <= 0`. `_b1` é a interseção isolada com uma ponta bruta, deslocando a data em 1.440 min. O código não reatribui o dia hipotético pelo juiz, não mede o ganho além de outra parcial já cobrindo os minutos, nem simula o teto L-093 que vem depois. Em checagens sintéticas, o método publicou `0/120` tanto quando outra parcial já cobria os mesmos 120 min como quando o teto aritmético zerava todo o desconto. `app/ponto/motor_calculo_v2.py:475–485,1245–1258,1267–1293,1306–1337` · `app/ponto/services/credito_parcial.py:74–80,100–114`

**9. Média — o teste para no formatador C12.** Exercita o comando, o espelho consumido pela lavra, o juiz de dono, o snapshot e a formatação dos dois valores. Pula `lista()`, que agrega assinaturas, reconcilia donos, aplica visibilidade, escolhe a ação e passa o conteúdo à view, ao HTML e ao PDF. Também pula a orquestração do cron. Execução em produção [nao verificado]. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:304–334` · `app/ponto/management/commands/lavrar_cxr_do_motor.py:27–40` · `app/ponto/services/cxr_na_lista.py:80–106,145–165` · `app/escala/services/cadastro_realidade.py:1040–1058,1076–1079,1105–1126` · `app/escala/views.py:512–536` · `app/config/crons.py:1808–1810`

_8454 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
