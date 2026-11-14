# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 10:11** · publicado em **14/11/2026 10:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 10:11 · sessao 40c452a14830 · - · - · origem=Ronald (tmux)

Complemento upstream. Todos os caminhos abaixo são leituras da cópia; campos `_...` nas batidas são marcas transitórias produzidas pelo pareador (`app/ponto/turnos.py:1315-1325,1370,1413,1477`).

| Pergunta / decisão | Predicado lido no código | Endereço |
|---|---|---|
| Universo apurável | `Batida.objects.filter(retratada_em__isnull=True)`; aplica `timestamp>=ini`, `timestamp<=fim`, colaborador quando informados; ordena timestamp | `app/ponto/turnos.py::batidas_apuraveis:2875-2883` |
| Duplicata no motor | Só tipos `E/S`; compara com última limpa; mesmo tipo e gap `<3min` ignora; constante3 em `:70` | `app/ponto/motor_calculo_v2.py::MotorBase.remover_duplicatas:897-911`; chamadas `:2351,882` |
| Eco de flush | Ordena por `criado_em` ou timestamp; somente `E`; ambas criações conhecidas; diferença de chegada ≤120s; diferença de evento entre1800s e `cont_max_s`; atual ao vivo até600s; anterior retida ao menos1800s → `_eco_flush=True` | `app/ponto/turnos.py::_marca_ecos_flush:355-374` |
| Uso do eco | `E` com `_eco_flush` é anexada ao turno existente, sem abrir novo | `app/ponto/turnos.py::parear_turnos:1398-1401` |
| Ordem e modo de intervalo | Ordena batidas por timestamp; sem marco de intervalo chama `_marca_intra_duracao` | `app/ponto/turnos.py::parear_turnos:1217-1221` |
| Marcos DNA ou template | Para o dia local da batida, usa pausa DNA se suas duas pontas existem; senão pausa template; bordas DNA se presentes; senão bordas template | `app/ponto/turnos.py::parear_turnos._ivm_de:1229-1236`, `._bm_de:1237-1244` |
| Modo duração sem bordas | `intra_max_s<=0` desliga; sem bordas, depois de E aberta, padrão S,E,S, gap S→E entre0 e teto da pausa, e E→S seguinte entre0 e envelope contínuo → marca S/E como intra | `app/ponto/turnos.py::_marca_intra_duracao:386,392-419` |
| Modo duração com bordas | Ignora já marcadas; padrão S→E com gap0..`intra_max_s`; S distante >90min das bordas; E distante >90min **ou** S imediatamente posterior dentro de `cont_max_s` comprova continuidade → marca pausa | `app/ponto/turnos.py::_marca_intra_duracao:425-458` |
| Duas saídas sem volta registrada | Para S distante >90min da borda seguida de S, busca S posterior dentro de `cont_max_s` e a≤90min de borda; a primeira vira interna | `app/ponto/turnos.py::_marca_intra_duracao:476-490` |
| Ata informa papel | Chave é timestamp local naive truncado no minuto; aceita somente `E/S/X/Xi`; sem mapa/timestamp → `None` | `app/ponto/turnos.py::_papel_da_ata:896-909` |
| Ata vence papel gravado | `X/Xi` marca intra/ata; `Xi` também declarado; demais usam papel da ata ou primeira letra do tipo gravado | `app/ponto/turnos.py::parear_turnos:1310-1326,1382` |
| Teto da absorção de pausa | Batida ainda não interna, há marcos de intervalo e E atual, duração desde E >`cont_max_s`, distância ao intervalo ≤90min → termina corrente como aberto antes de tratar batida | `app/ponto/turnos.py::parear_turnos:1349-1355` |
| Marco de intervalo mais próximo | Ainda não interna e turno existe; distância intervalo≤90min; borda ausente ou distância intervalo≤distância borda → marca interna | `app/ponto/turnos.py::parear_turnos:1356-1370` |
| Batida interna | Anexa ao corrente; uma S interna grava `_ult_iv_s`; não abre/fecha turno | `app/ponto/turnos.py::parear_turnos:1371-1376` |
| Volta deslocada | E com corrente, S interna anterior, gap `>0` e `<=intra_max_s`, e E longe >90min das bordas ou sem bordas → interna; senão E fecha corrente como aberto e inicia outro | `app/ponto/turnos.py::parear_turnos:1406-1420` |
| Teto do par | Com DNA `hi/hf`, teto é `(hf-hi)%1440 +240min`; sem ambas ou bordas iguais usa envelope26h; ambas distâncias das pontas aos respectivos marcos >180min devolvem envelope26h; duração >teto separa E aberta/S órfã | `app/ponto/turnos.py::_teto_da_jornada:78-82`; `::parear_turnos:1430-1457` |
| Pausa deslocada com marcos | Há intervalo template e duas próximas batidas; S longe>90min das bordas; próximas E,S; S→E entre0 e teto pausa; E longe>90min da borda; E→S entre0 e envelope; S final não absorvível por marco → marca primeira S/próxima E internas | `app/ponto/turnos.py::parear_turnos:1466-1481`; `_absorvivel:1246-1251` |
| Par degenerado | Duração≤`DURACAO_ECO_SEG` recusa fecho, declara par degenerado e produz S órfã mantendo E corrente; constante lida do detector | `app/ponto/turnos.py::_par_degenerado:130-131`; `::parear_turnos:1487-1490` |
| Par normal / órfã | S não recusada fecha corrente; S sem corrente vira órfã; corrente remanescente vira aberto | `app/ponto/turnos.py::parear_turnos:1491-1498` |
| Pós-fecho com próxima S | Só turno com E sem S; procura primeiro turno seguinte; próxima E interrompe busca, salvo candidato posicional; primeira S órfã é candidata; recusa degenerado ou duração>jornada+4h/default`cont_max_s`; funde batidas e remove candidato | `app/ponto/turnos.py::_fechar_aberto_com_saida_seguinte:1061-1102` |
| Candidata posicional entre turnos | Exige mapa; turno candidato sem S e com exatamente1 batida; ata não a chama E/S; dia anterior com `hi/hf`; pausa fechada/carimbada de15min..3h e candidata posterior; se `hi` da candidata existe, distância à instância mais próxima de `hi` deve ser **maior** que distância ao `hf` anterior; empate recusa | `app/ponto/turnos.py::_saida_pela_posicao_entre_turnos:991-1026`; `::_pausa_fechada_carimbada:923-928` |
| Pós-fecho na pausa sem volta | Turno com E sem S/batidas; última batida é S marcada intra e há alguma batida posterior na vida ou recorte; duração não degenerada e≤teto jornada → fecha nessa S | `app/ponto/turnos.py::_fechar_aberto_na_pausa_sem_volta:1156-1188` |
| Dia da véspera | Sem marcos véspera ou sem cruzamento → data local; S órfã pertence à véspera se cabe no envelope `hi_véspera+teto`; com E, hora antes de `hf`, distância modular desde início>90min e saída ausente ou dentro do envelope → véspera; senão dia local | `app/ponto/turnos.py::_data_do_turno:285-309`; `::_cabe_na_jornada_da_vespera:229-231` |
| Véspera elegível no pareador | Não considera véspera se já existe turno dela aberto ou cuja S tem data local da véspera; com mapa exige ambas bordas dela; sem mapa exige duas bordas template | `app/ponto/turnos.py::parear_turnos._vespera:1295-1302` |
| Pausas monetizadas | Primeiro casa pares `_intra_declarado` não coincidentes com bordas; depois pares adjacentes não usados, primeira marcada, ambas fora das bordas, duas marcas da ata **ou** próxima marcada+papel da primeira S **ou** tipos S→E, cronológicos; `intervalos_do_turno` só devolve duração≥15min | `app/ponto/turnos.py::_pares_marcados:811-816,819-885`; `::intervalos_do_turno:542-546` |
| Borda posicional do dia | Exige exatamente1 turno com fim fechado; nenhum par marcado;≥4 batidas distintas; entrada do fechado é primeira batida; última após fim fechado; exatamente1 gap interior15min..3h; minuto final>inicial | `app/ponto/turnos.py::_borda_posicional:673-704` |
| Número pela borda posicional | Trabalhado do dia vira vão primeira→última menos único gap, truncados ao minuto; mínimo0 | `app/ponto/turnos.py::realizado_dos_turnos:725-732`; motor lê a borda em `app/ponto/motor_calculo_v2.py:2527-2538` |
| Cadastro usado para papel da ata | Sem alimentação, `_escalas_da_ata` ordena por colaborador, ativa, início e última linha ganha; escolha é por período, sem filtro de vigência aqui | `app/ponto/turnos.py::_escalas_da_ata:1556-1562`; `::papel_por_minuto_da_ata:1592-1594` |
| Ata admissível para relabelar | `periodos_do_dia` sem erro/desalinhado; alguma luz acesa; cada minuto de luz contém exatamente1 batida apurável; exige entrada/saída e não aberto; ambas pontas distantes>180min do marco recusam | `app/ponto/turnos.py::papel_por_minuto_da_ata:1631-1657,1682-1700` |
| Batidas sobrantes na ata | Só interior entrada→saída e fora de todas luzes; minuto com≠1 batida torna dia ambíguo; próxima de luz até`JANELA_CLUSTER_MIN` vira órfã interna; restante aceito apenas como par de2 com gap≤teto ou perna única até teto de luz de intervalo; demais dias não emitem papel | `app/ponto/turnos.py::papel_por_minuto_da_ata:1728-1767` |
| Ata versus DNA | `periodos_do_dia` monta esperados sobre DNA; forma posicional aceita lâmpadas com nomes `·I\d+` quando não existem esperados; ausentes/tamanhos diferentes recusam; hora de cada lâmpada deve casar HH:MM do esperado | `app/ponto/juiz_batida.py::periodos_do_dia:157-176,190-204,227-235` |
| Dia posicional | Luzes acesas HH:MM legíveis ancoradas na data da célula; se próxima hora anda para trás, soma1dia | `app/ponto/juiz_batida.py::periodos_do_dia:212-224` |
| Bordas pela ata | Usa `hi/hf` ou primeira/última lâmpada posicional; só acesa gera entrada/saída; qualquer ponta não acesa deixa aberto | `app/ponto/juiz_batida.py::periodos_do_dia:256-267` |
| Intervalos pela ata | Par de pausa com ambas acesas/instantes e fim≥início entra na lista;>1 pausa soma minutos truncados; primeiro par aceso usa diferença; par incompleto usa cadastro; sem marco de pausa→0; flag indenizável lida do template | `app/ponto/juiz_batida.py::periodos_do_dia:280-313` |
| Dono do dia | Célula com geradora exige geradora entre candidatos e vigência contendo data, senão`None`; sem geradora filtra candidatos com início≤data, fim ausente/ou≥data e vigência declarada, escolhe máximo `(ativa,data_inicio)` | `app/escala/alimentacao.py::vinculo_do_dia:269-286`; `::vinculo_cobre_a_data:214-216` |
| Segmentação dos turnos | Para cada dia pergunta dono; dias consecutivos de mesmo dono juntam segmento; batidas dentro de segmento±2dias; cada segmento pareia separado e conserva só turnos com `data_turno` dentro do segmento | `app/ponto/turnos.py::_turnos_do_colab_puro:1956-1995` |
| Trabalho do dia cadastrado | Célula existente devolve `cel.trabalha`, inclusive `None`; sem célula usa cálculo | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho:1087-1096` |
| Fase dos ciclos | Âncora do colaborador com ano>1940 e sem foto do mês:12x36 `delta%2==0`;24x48 `delta%3==0` | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho_calculado:1280-1295` |
| Foto de folgas | Dia na foto→False; fora da foto usa ciclo template se sabe; senão referência de folga mais próxima, empate dia anterior, distância ímpar→trabalho, par→folga; sem referência→None | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho_calculado:1296-1364` |
| Ciclo template | Intermitente→False;12x36/24x48→None; semanal sem lista→True/senão weekday na lista;6x1 com folga válida exclui weekdays/senãoTrue;5x2 com folga exclui weekdays/senão weekday<5;personalizado com dict não vazio→chave weekday presente/senãoNone | `app/escala/models.py::TipoEscala.eh_dia_trabalho:820-868` |
| Feriado vira folga | Só muda base exatamenteTrue; exige `not trabalha_em_feriado`, escala presente, ciclo em `CICLOS_FERIADO_VIRA_FOLGA`, feriado alcança município do posto do vínculo | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho_calculado:1369-1371`; `::feriado_vira_folga:1389-1395` |
| Alcance territorial do feriado | Nacional sempre; estadual UF normalizada igual; municipal UF e nome normalizado iguais; restanteFalse; sem município só nacional | `app/core/feriados.py::alcanca:59-67`; `::_lugar:50-54`, `::_norm:35-37`; `::eh_feriado:105` |
| Local cadastrado do feriado | Posto do vínculo→município; datas do período perguntam vínculo por dia e alcance pelo lugar daquele vínculo | `app/core/feriados.py::lugar_do_vinculo:131-132`; `::feriados_do_colab:160-166` |
| Marcos do vínculo | DNA com `marcos` não vazio →hi,hf,hii,hfi congelados; sem isso usa template | `app/escala/models.py::EscalaColaborador.marcos_do_dia:1117-1133` |
| Marcos do template | Override do dia ganha; entrada/saída do override caem no padrão se vazias; modo intervalo livre retira marcos de pausa; sem override usa horários padrão | `app/escala/models.py::TipoEscala.marcos_do_dia:335-345` |
| Pausas declaradas | DNA com marcos: pares completos; célula existente sem marcos→nenhuma; sem célula template; template adiciona2ª pausa somente se primeira existe, sem override e sem modo livre | `app/escala/models.py::EscalaColaborador.pausas_do_dia:1182-1208`; `::TipoEscala.pausas_do_dia:304-308` |
| Extra declarada | Célula existente usa apenas `dna.extra_declarada`; objeto não dict→0/0; ausência da chave→0/0; somente sem célula usa campos vivos do template | `app/escala/models.py::EscalaColaborador.extra_declarada_do_dia:1230-1239`; `::TipoEscala.extra_declarada_do_dia:324` |
| Minutos previstos | Lista de vínculos explicitamente vazia→0; sem vínculo/template→0; trabalho não exatamenteTrue→0; semhi/hf→0; span≤0 soma1440; pausas com duração positiva<span descontam; se lista de pausas vazia, usa primeiro parhii/hfi | `app/escala/utils.py::minutos_previstos_do_dia:2256-2274,2291,2305-2320` |
| Intervalo já descontado do previsto | Mesmas guardas vínculo/trabalho; exigehi,hf,hii,hfi; span≤0 soma1440, intra negativa soma1440; devolve só se`0<intr<span` | `app/escala/utils.py::intervalo_declarado_do_dia:2208-2227` |
| Instantes dos marcos | Lê vínculo quando fornecido, senão template; semhi/hf→[]; encadeia E,S,E,S; cada marco≤anterior avança1dia | `app/escala/servico_jornada.py::marcos_turno:76-94` |
| Matching por papel | Obtém papel do turno ou alternância; L084 nas duas pontas desliga esse modo; estrutura dos marcos deve ser E,(S,E)*,S; entrada no marco de volta somente se ele é mais próximo; saída no último; pares ocupam pausas em ordem com menor distância; soltas só marco mais próximo livre e cronológico | `app/escala/utils.py::_match_marcos:596-614`; `::_alinhar_por_papel:515-571` |
| Cluster no matching por papel | Batida perde marco se seu marco mais próximo já pertence a outra batida com distância menor ou igual | `app/escala/utils.py::_alinhar_por_papel:577-581` |
| Matching sem papel | Candidatos não consumidos dentro de primeiro-marco−tolerância até último+tolerância; DP minimiza distância absoluta em minutos, salto custa1e9; cluster retira ocupação de marco distante se marco mais próximo já tem batida tão próxima ou melhor | `app/escala/utils.py::_match_marcos:625-652`; `::_alinhar:222-239` |

Registro para consolidar sem confundir nome do arquivo com inscrição: `_T1` turno e `_T6` intervalo constam efetivamente de `JUIZES['turno/marcos']` em `app/core/juizes.py:208,214`; `_T4` matching em `:212`, `_T5` marcos em `:213`; trabalho/previsto/feriado em `:184-188`; período pela ata em `JUIZES['batida']` `:1193`; PAR monetário em `:1194`. Os predicados internos adicionais acima não têm cada um inscrição individual identificada [não verificado]. Registro do vínculo e cobertura está na família escala definida após `app/core/juizes.py:1303` [não verificado o endereço exato da inscrição nesta leitura].

Correção sobre caso do degrau: existe teste numérico com regra escrita no plantão — `app/ponto/tests/test_plantao_he_distribuicao.py::TestPlantaoHEDistribuicao::test_plantao_24h_distribui_he50_he100:23`, referência Art59 em `:4-8`, asserts960extra/120HE50/840HE100 em `:28-31` — informado pelo explorador de casos. A bateria declara que não certifica o degrau em `app/ponto/tests/test_o280_bateria_da_lei.py:3710-3713,5994-5996`; esses textos devem aparecer sem escolher entre eles.

## 14/11 10:09 · sessao 7e09ef4f766a · - · - · origem=Ronald (tmux)

| Predicado solicitado | Resultado da busca |
|---|---|
| Classe da saída | `app/ponto/tests/test_janela_he_saida.py::JanelaHeSaidaTest::test_RED_so_a_saida_07h06_conta_ate_o_marco:91`; classe em `:34`. |
| `classificar_export` | **SEM CASO** pelo filtro CLT/CCT/L-NNN localizado. Testes comportamentais em `app/folha/tests/test_classificar_export.py::ClassificarExportTest:34`, `:40`, `:47`, `:56`; a referência de `:48` é somente “LEI31/07”. |
| Carimbo `conferir` | **SEM CASO** pelo filtro CLT/CCT/L-NNN localizado. `app/folha/tests/test_porta_do_export.py:2-13` ancora em O1/corte; casos de ausência/divergência/verde/outra competência/ontem/erro em `::PortaDoExportTest:44`, `:50`, `:508`, `:514`, `:520`, `:528`. |
| Rubrica ativa, empresa sobre global | **SEM CASO** pela regra. `app/folha/tests/test_r1_caracterizacao_rubricas_map.py:1-5` declara caracterização; `::RubricasMapCaracterizacaoTest::test_02_override_vence_global:31` e `::test_04_inativa_cai_fora:45`. |
| Unidade/centésimos do TXT | **SEM CASO** CLT/CCT/L-NNN localizado. `app/folha/tests/test_integracao.py::LinhaDominioTests::test_horas_centesimal_const11:38` espera `1,5→000000150` em `:39-40`, sem regra identificada. |
| Forma CRLF | Há L-113 no cabeçalho, mas teste de forma, sem conferir dinheiro pela regra: `app/folha/tests/test_l113_a0_forma_do_txt.py::FormaDoTxtTest::test_MORDE_duas_listas_dao_dois_conteudos:66`, `::test_MORDE_lista_vazia_da_string_vazia:73`, `::test_MORDE_uma_linha_nao_perde_o_terminador:78`; escopo declarado em `:11-18`. |
| Dedup/colisão Batida | **SEM CASO** CLT/CCT/L-NNN localizado. Testes numéricos sem esse lastro em `app/ponto/tests/test_registrar_batida.py::RegistrarBatidaGuardTest::test_oposto_60s_automatico_recusa:32`, `::test_oposto_60s_humano_cria_e_flaga:45`, `::test_mesmo_tipo_60s_dedup:67`; cabeçalho apenas S137/S148 em `:9-12`. |
| Envelope e exceção de marco próprio | **SEM CASO** CLT/CCT/L-NNN localizado. `app/ponto/tests/test_dedup_envelope_template.py::DedupEnvelopeTemplateTest::test_eco_oposto_120s_recusado_com_template:37`, `::test_marco_proprio_tipo_cria_mesmo_perto_de_oposto:48`; cabeçalho P5/status quo em `:9-12`. |
| Ciclo semanal complementado por foto | **CASO:** `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F_FOTO_folga_avulsa_no_mes_nao_apaga_o_folga_dia_semana:2058`. L-023 em `:2059`; previstos 14/15/17/18, quatro faltas e32h em `:2066-2088`. |
| Paridade 12x36 pela foto | **CASO:** mesmo arquivo/classe, `::test_F_FOTOb_foto_parcial_no_12x36_a_fase_sai_da_foto_nao_da_ancora:2090`. L-023/L-022 em `:2091`; foto14/16→previstos15/17/19, três faltas e33h em `:2106-2134`. |
| Marcos/previsto descontam pausas | **CASO:** `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F7_intervalo_dividido_em_dois:1968`; Art71 em `:1969`, `660−75=585` em `:1971-1982`. O teste específico `app/escala/tests/test_previsto_desconta_as_duas_pausas.py::PrevistoDescontaTodasAsPausasTest::test_MORDE_duas_pausas_descontam_as_DUAS:58` espera550, mas cabeçalho `:2-21` só ordem/R4/O114, sem CLT/CCT/L-NNN. |
| Vigência/troca de vínculo | **CASO:** `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F5_troca_de_vinculo_no_meio_da_competencia:1867`; L-021/L-099 em `:1868`; regra/caso em `:1875-1892`, resposta16h trabalhadas/1h atraso/2h antecipada em `:1944-1952`. |
| Folga calendário versus manual | Caso escrito com L-110, porém resposta categórica sem minutos/dinheiro: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py::GradePorTurnoFolgaDoVinculoTest` (`:98`) herda `::test_MORDE_folga_de_calendario_de_outro_vinculo_e_trabalho_na_grade:70`, `::test_folga_a_mao_e_da_pessoa_e_folga_na_grade:75`, `::test_folga_de_calendario_do_vinculo_do_dia_e_folga_na_grade:80`; regra em `:10-20`, respostas literais em `:72`, `:77`, `:82`. |

## 14/11 10:09 · sessao b6c44cd5961f · - · - · origem=Ronald (tmux)

| Decisão | Caso forte pela regra |
|---|---|
| Chegada antes do marco | `app/ponto/tests/test_janela_he_regra.py::JanelaHeRegraTest::test_RED_06h40_com_marco_07h00_nao_gera_HE:83`; L-097 escrita :2-6; 20 min antecipados → trabalho660/HE0, :86-89 |
| Piso da janela | Mesmo arquivo, `JanelaHeRegraTest::test_MORDE_06h52_esta_dentro_do_piso_e_NADA_muda:91`; Art.58 §1, :92; piso10/antecipação8 → trabalho668, :94 |
| Teto da janela | Mesmo arquivo, `JanelaHeRegraTest::test_RED_05h30_passa_do_teto_e_entra_em_CADASTRO_x_REALIDADE:97`; antecipação90 → trabalho660/HE0 e acusação, :100-103 |
| Vigência da janela | Mesmo arquivo, `JanelaHeRegraTest::test_MORDE_a_vigencia_protege_a_competencia_09:107`; L-092, :108; antes da vigência → trabalho680, :110 |
| Duas pontas fora | `app/ponto/tests/test_janela_he_saida.py::JanelaHeSaidaTest::test_RED_as_duas_pontas_juntas_nao_geram_HE:82`; L-097/bloqueio total, :2-15; 18:51–07:06 contra19–07, pausa1h → trabalho660/HE0, :85-89 |
| Saída depois do marco | Mesmo arquivo, `JanelaHeSaidaTest::test_RED_so_a_saida_07h06_conta_ate_o_marco:91`; trabalho660/HE0, :94-95 |
| Entrada antes, piso zero | Mesmo arquivo, `JanelaHeSaidaTest::test_RED_so_a_entrada_18h51_conta_do_marco:97`; trabalho660/HE0, :100-101 |
| Autorização faz contar | Mesmo arquivo, `DiaAutorizadoContaTest::test_RED_o_dia_AUTORIZADO_passa_a_contar_os_minutos_de_fora:255`; L-097 escrita :199; trabalho740, :258 |
| Recusa mantém padrão | Mesmo arquivo, `DiaAutorizadoContaTest::test_MORDE_o_NAO_tem_o_efeito_do_PADRAO_e_nao_muda_numero:262`; trabalho720, :265 |
| Autorização abrange ambas as pontas | Mesmo arquivo, `DiaAutorizadoContaTest::test_MORDE_a_decisao_e_do_DIA_e_nao_da_PONTA:269`; trabalho735, :274 |
| Decisão de outra data não autoriza | Mesmo arquivo, `DiaAutorizadoContaTest::test_MORDE_a_decisao_de_OUTRO_dia_nao_vale_para_este:277`; trabalho720, :282 |
| Dia sem previsto bloqueia inteiro | `app/ponto/tests/test_he100_dia_sem_previsto.py::DiaSemPrevistoForaDaJanelaTest::test_RED_dia_sem_previsto_nao_paga_sem_decisao:69`; L-097 escrita :4-9; 330 reais → trabalho0/fora330/FT0, :73-75 |
| Dia sem previsto autorizado | Mesmo arquivo, `DiaSemPrevistoForaDaJanelaTest::test_autorizado_paga_inteiro:83`; trabalho330/fora0/FT5,5h, :89-91 |
| Duas pontas longe preservam trabalho | `app/ponto/tests/test_o280_bateria_da_lei.py::V3Familia3Test::test_v3_F9c_a_L084_recusa_o_marco_e_as_horas_CONTAM:4013`; L-084+Art.4 CLT, :4014; trabalho300/atraso0/antecipada0, :4027-4029 |
| Duas pontas longe acusam cadastro | Mesmo arquivo, `V3ReapontarTest::test_i26_B4_as_duas_pontas_longe_viram_CADASTRO_x_REALIDADE:5654`; L-084, :5655; trabalho540/atraso0/antecipada0 e CXR1, :5667-5670 |
| Uma ponta longe cobra atraso | `app/ponto/tests/test_l084_vence_o_t8.py::L084VenceOT8Test::test_RED_a_entrada_a_452_min_do_marco_VOLTA_a_ser_atraso:69`; L-084 escrita :6-17; atraso452, :76 |
| Duas pausas saem do trabalho | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F7_intervalo_dividido_em_dois:1968`; Art.71 §1 e ordem02/10, :1969; trabalho585/pausa75/HE0, :1981-1982 |

Não encontrei caso forte **numérico de folha** para exclusão de batida retratada ou recorte pela data: `[nao verificado]`. Há L-025 escrita e casos comportamentais em `app/ponto/tests/test_o247_retratada_fora_do_pareamento.py:3-6`, :84, :96, :117, mas seus resultados são listas/booleanos/contagem de saídas, :91-94, :114, :133-136.

O teste de par aberto em `app/ponto/tests/test_o280_bateria_da_lei.py::V3Familia1Test::test_v3_turno_aberto_nao_recebe_hora_inventada:3694` afirma trabalho/intra zero, :3699-3700, porém o método cita somente a matriz “absurdo”, :3695; lastro CLT/CCT/L-NNN para esse caso: `[nao verificado]`.

O caso de cauda inteiramente após o marco existe pela L-097 em `app/ponto/tests/test_o280_bateria_da_lei.py::V3FrotaTest::test_v3_B_I28_PINADO_o_minuto_fora_do_marco_nao_gera_HE:5124`, esperado HE50 zero, :5142; está marcado `expectedFailure`, :5123.

## 14/11 10:09 · sessao d40d5604fe39 · - · - · origem=Ronald (tmux)

`remover_duplicatas` está ativo: é chamado para o mapa de turnos e para os períodos calculados; mantém somente E/S e exclui repetição do mesmo tipo com intervalo inferior a três minutos (`app/ponto/motor_calculo_v2.py:882`, `:902`, `:909`, `:2351`, `:2354`).

| Decisão | Predicado efetivo | Referência |
|---|---|---|
| Papel da batida | Papel E/S/X/Xi da ata prevalece sobre tipo gravado; X/Xi vira interna | `app/ponto/turnos.py::parear_turnos:1310`, `:1316`, `:1382` |
| Eco de entrada | E com `_eco_flush=True` não abre turno | `app/ponto/turnos.py::parear_turnos:1398` |
| Identificação do eco | Chegadas ≤120s; eventos entre 1.800s e 14h; atual ao vivo ≤600s; anterior atrasado ≥1.800s | `app/ponto/turnos.py::_marca_ecos_flush:368`, `:370` |
| Pausa por marco | Distância ao intervalo ≤90min e não maior que distância à borda | `app/ponto/turnos.py::parear_turnos:1358`, `:1364` |
| Volta deslocada | E após S interna; `0<gap≤intra_max_s`; distante >90min das bordas | `app/ponto/turnos.py::parear_turnos:1408` |
| Nova entrada | E seguinte deixa turno anterior aberto e inicia outro | `app/ponto/turnos.py::parear_turnos:1417` |
| Teto do fechamento | Jornada declarada +4h; sem bordas diferentes, teto recebido; no fechamento principal, fallback 26h | `app/ponto/turnos.py::_teto_da_jornada:78`, `:82`; `::parear_turnos:1433` |
| Exceção do teto | Ambas as pontas distantes mais que `CADASTRO_NAO_DESCREVE_MIN` devolvem envelope 26h | `app/ponto/turnos.py::parear_turnos:1444` |
| S além do teto | Entrada fica aberta; saída vira órfã | `app/ponto/turnos.py::parear_turnos:1447`, `:1453` |
| Par mínimo | Duração ≤60s não forma turno; S fica órfã | `app/ponto/turnos.py::_par_degenerado:130`; `::parear_turnos:1487` |
| Saída restante | S fecha entrada disponível; sem entrada, vira órfã | `app/ponto/turnos.py::parear_turnos:1491`, `:1495` |
| Dia do turno | Dia local; pode migrar à véspera quando cruza meia-noite, entrada antes de `hf`, distante >90min de `hi`, saída dentro do envelope da véspera | `app/ponto/turnos.py::_data_do_turno:285`, `:291`, `:301`, `:302` |
| Intervalo descontável | Par marcado com duração ≥900s | `app/ponto/turnos.py::intervalos_do_turno:545` |
| S órfã no motor | Não produz período calculável | `app/ponto/adaptador_motor.py::periodos_de_turnos:30`, `:36` |

Os dois caminhos têm alimentação diferente: o mapa do dia usa `turnos_de_batidas`, que marca ecos e não recebe o mapa de papéis; a formação dos períodos recebe papéis da ata e os parâmetros `AUT_*` (`app/ponto/motor_calculo_v2.py:882`, `:503`; `app/ponto/turnos.py:1533`, `:1535`).

## 14/11 10:08 · sessao cb2d6c9fd49a · - · - · origem=Ronald (tmux)

Inventário final complementar, com caminhos relativos à raiz:

| Decisão | Código / predicado efetivo | Gravação e registro | Caso pela regra |
|---|---|---|---|
| Ativar janela HE | `app/ponto/motor_calculo_v2.py::MotorBase._empresa_da_janela_do_dia:1390`: exige empresa, `janela_he_ativa`, `janela_he_desde` preenchida e `dia >= desde`. | Alimenta `_janela_do_dia`; SEM JUIZ específico no registro `app/core/juizes.py:182` e `:1192`. | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O273_recusa_revogada_nao_move_o_dia_e_a_autorizacao_move:2606` liga o cadastro e confere 720 min, HE0, fora40. Não isola borda da vigência; SEM CASO dessa borda [nao verificado]. |
| Excluir janela por folga ou cadastro | `_janela_do_dia:1526` folga memorizada retorna `None`; `:1532-1542` sem função/marco retorna `None`; `:1562` e `:1606` `_cadastro_nao_descreve` para par e para pontas do dia retorna `None`. | A janela deixa E/S intactas. SEM JUIZ da janela. Marcos são `_T5` em `app/core/juizes.py:213`; tolerância é `_T7:215`, cuja pergunta registrada se restringe à pontualidade. | Caso específico da L084 na janela não confirmado [nao verificado]. |
| Autorização do dia | `_carregar_decisoes_he:1416` lê `DecisaoHE.objects.vigentes()`; `_janela_do_dia:1653` testa estado `autorizado`; `_entrada_efetiva:1697` e `_saida_efetiva:1739` retornam timestamps crus se autorizado. | Não escreve DecisaoHE; altera os insumos do período. SEM JUIZ específico no registro. | `BugsTest::test_B_O273_recusa_revogada_nao_move_o_dia_e_a_autorizacao_move:2606`: L097, sem decisão/recusada/revogada=720/HE0/fora40; autorizada=760/HE40/fora0, asserções `:2655`, `:2684`. |
| Recortar entrada | `app/ponto/janela_he.py::entrada_efetiva:94`: `bruto=marco-entrada`; `minutos_fora_da_extra:73` calcula `max(0, bruto-extra)`; `dentro_da_janela:51` aceita `sobra<=piso`; senão retorna marco deslocado e sobra `:102`. | `MotorBase._entrada_efetiva:1702`; SEM JUIZ específico. | `app/ponto/tests/test_o146_extra_declarada_da_escala.py::O146ExtraDeclaradaTest::test_e_extra_antes_60_entrada_no_limite_deslocado_da_60_de_HE:221`; `::test_d_entrada_10_min_antes_SEM_extra_declarada_e_ponta_de_10_como_hoje:212`, regra L097 `:213`. |
| Recortar saída | `MotorBase._saida_efetiva:1734` exige janela, `saida_ativa` e marco; `app/ponto/janela_he.py::saida_efetiva:145-153` usa sobra após extra, tolera `<=piso`, senão retorna marco+extra. | E/S efetivas e `minutos_fora_janela`; SEM JUIZ específico. | `O146ExtraDeclaradaTest::test_a_saida_em_marco_mais_127_com_extra_120_da_120_de_HE_e_ponta_de_7:148`: 120HE/7fora, L097 `:160`; `::test_b_saida_em_marco_mais_60_com_extra_120_da_60_de_HE_e_ponta_zero:185` afirma 60, não 120, L024 `:186`. |
| Dia inteiro sem previsto | `_dia_sem_previsto_fora_da_janela:1448-1469`: exige folga OU feriado, empresa em janela, previsto exatamente0, sem autorização; se folga e colaborador conhecido exige `escala_certa_no_dia`. | `:1472` soma trabalhadas em fora; `:1473-1476` zera trabalhadas, noturnas, extras50/100 e subconjuntos, intra. SEM JUIZ da composição. | SEM CASO confirmado [nao verificado]. |
| Extra declarada por célula | `app/escala/models.py::EscalaColaborador.extra_declarada_do_dia:1234-1239`: célula existente lê `dna.extra_declarada`; chave ausente/malformada=>zeros; somente sem célula lê template. | Alimenta limites da janela; SEM JUIZ específico. | `O146ExtraDeclaradaTest::test_k_celula_que_EXISTE_e_nao_declara_responde_ZERO_nunca_o_template_vivo:312`: L020/L052, 480trabalho/HE0/127fora `:324-328`. |
| Jornada prevista | `app/escala/utils.py::minutos_previstos_do_dia:2256-2274`: zero sem vínculos, vínculo/tipo, dia `is not True`, hi/hf; span<=0 soma1440; `:2305-2319` subtrai pausas válidas `0<intr<span`, inclusive segunda pausa. | Lido por `_previsto_do_dia:1253`, `_jornada_do_dia:1273`. Juiz registrado `app/core/juizes.py:185`. | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F1_previsto_do_mes_e_soma_dos_dias_nao_30_x_jornada:1299`. |
| Intervalo devolvido ao limite | `app/escala/utils.py::intervalo_declarado_do_dia:2208-2227`: mesmas guardas de vínculo/dia; exige quatro marcos; somente primeiro intervalo, válido `0<intr<span`. `MotorBase._limite_com_suprimido:1785-1803` soma `min(suprimido, declarado)`. | Altera limite HE; SEM JUIZ específico do intervalo devolvido. | `app/ponto/tests/test_he_intra_dobrada.py::HeIntraDobradaTest::test_RED_doze_horas_sem_intervalo_NAO_tem_hora_extra:77` e `::test_MORDE_treze_horas_sem_intervalo_TEM_uma_hora_de_extra:88`; regra escrita `:12-20`. |
| HE diária no partido | `MotorTurnoPartido.calcular_mes:2933-2939` zera HE individuais; `:3092` feriado em dobra tudo100; `:3120` semanal pula diária; `:3128-3144` usa previsto ou trabalho, legais limita480 e soma ganho noturno, aplica limite com suprimido; `:3151` excesso<=10 exclui; `:3154-3158` até120 é50, restante100. | `ultimo.minutos_extra_50`; SEM JUIZ do cálculo mensal. Registro `app/core/juizes.py:1194` declara somente `MotorBase.calcular_periodo`. | Régua legais: `app/ponto/tests/test_regua_excedente.py::ReguaExcedenteTest::test_regua_legais_parecer:55` cita CCTcl10, calcula ~111,4min por turno; usa fator importado, sem independência desse fator. Degrau120: SEM CASO pela regra, explicitamente excluído pela bateria `app/ponto/tests/test_o280_bateria_da_lei.py:2203-2207`. |
| HE semanal no partido | `MotorTurnoPartido.calcular_mes:3161-3187`: apenas `semanal_44`; exclui feriados; agrupa seg-dom; excesso=soma−2640; exclui `<=10*dias`; lança inteiro como50 no último período da última jornada. | `ultimo.minutos_extra_50=:3187`; SEM JUIZ específico. | `test_tolerancia_he.py::ToleranciaHeTest::test_semanal_dentro_da_tolerancia_nao_gera_he:49` e `::test_semanal_estourou_o_teto_conta_integral:53` conferem40=>0,60=>60, mas arquivo só cita mandato, não CLT/CCT/L. Pelo critério estrito: SEM CASO confirmado [nao verificado]. |
| HE mensal comercial | `MotorComercial.calcular_mes:3332-3337` zera HE individuais; `:3350` feriado tudo100 e `continue`; `:3387-3407` limite inicial480, marcos do vínculo/template se turno não cruza meia-noite, subtrai intervalo; `:3409` devolve suprimido; `:3414` excesso<=10 exclui; `:3417-3421` degrau120. | `ultimo.minutos_extra_50`; SEM JUIZ específico da função mensal. | Tolerância pela regra na v3: `app/ponto/tests/test_o280_bateria_da_lei.py::V3Familia2Test::test_v3_F9b_a_tolerancia_do_Art58_tambem_e_da_HORA_EXTRA:3829` confere8=>HE0,15=>HE15; é calculador v3, não esse método. Degrau: SEM CASO. |
| Selecionar motor | `get_motor:3640-3692`: sem tipo=>12x36; intermitente por ciclo OU base; base desconhecida levanta; 12x36+hi=>com escala; base partido com hi/hf e geometria contínua troca para comercial ou12x36 conforme cruza meia-noite; senão mapa. | Instância do motor. SEM JUIZ da fábrica; geometria partido e meia-noite registradas `app/core/juizes.py:216-220`. | SEM CASO pela regra confirmado para dispatch inteiro [nao verificado]. |
| Dia de trabalho e ciclo | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho:1094` célula=>trabalha; senão `eh_dia_trabalho_calculado:1289-1295` ancora válida sem foto=>%2/%3; foto `:1309-1364` data presente folga, fora template se sabe, senão paridade da folga mais próxima; `:1369` feriado pode converter True emFalse. | Decide periodos/periodos_ft do motor `:2447`, `:2492-2494`. Juiz `app/core/juizes.py:184`. | `FronteiraTest::test_F_FOTOb_foto_parcial_no_12x36_a_fase_sai_da_foto_nao_da_ancora:2090`; ver corpo antes certificar números [nao verificado]. |
| Universo batidas | `app/ponto/turnos.py::batidas_apuraveis:2875-2883`: retratada NULL, datas inclusivas, colaborador opcional, ordenação timestamp; não exige aprovada. | Input dos turnos. SEM JUIZ registrado por essa função. | SEM CASO pela regra confirmado [nao verificado]. |
| Intervalo julgado | `app/ponto/turnos.py::intervalos_do_turno:542-546`: somente pares marcados de duração>=15min. | `app/ponto/adaptador_motor.py::periodos_de_turnos:44`, depois período do motor. Juiz `_T6`, `app/core/juizes.py:214`. | SEM CASO numérico especificamente do piso confirmado [nao verificado]. |
| Ata substitui geometria | `_periodos_pelo_marco:559-576` exclui partido/sem células; `:632-645` exclui desalinhado, aberto, sem pontas/batidas reais; `:669-675` se apuráveis dentro do período excedem lâmpadas mantém geometria; `:685-721` substitui períodos e pausas pelos da ata com timestamps reais. | `periodos`, `intervalos`; juiz `periodos_do_dia` registrado `app/core/juizes.py:1193`, mas composição das guardas não registrada separadamente. | SEM CASO pela regra de todas essas guardas confirmado [nao verificado]. |

Escritores diretos HE50 do motor: default `app/ponto/motor_calculo_v2.py:222`; zero por `setattr :1473-1476`; `Motor12x36.calcular_periodo:2851,2853`; `MotorTurnoPartido.calcular_mes:2934,3155,3157,3187`; `MotorComercial.calcular_periodo:3290,3292`; `MotorComercial.calcular_mes:3333,3418,3420`. Delegações: 24x48 `:3535`; 12x36ComEscala `:3553`.

Escritores HE50 noturna: default `:224`; `_aplicar_he_noturna:1012-1022` grava apenas se resultado truthy; zeros `:1473-1476`, `:2937`, `:3336`. `he_noturna:430-451` recua pela ponta trabalhada, desconta intervalo e reparte minutos noturnos proporcionalmente entre50/100. Caso Sum264: `app/ponto/tests/test_he_noturna_sumula264.py::TestHeNoturnaSumula264::test_19_00_com_1h_extra_dentro_da_noite:29` e `::test_07_20_extra_fora_da_noite:43`; não conferem rateio proporcional misto. SEM CASO confirmado desse rateio [nao verificado].

A bateria explicitamente recusa usar degrau120 como resposta da regra: `app/ponto/tests/test_o280_bateria_da_lei.py:2203-2207` e `::V3ReapontarTest::test_i26_H2_acima_do_limite_a_hora_segue_devida_sem_teto:5991-6004`.

## 14/11 10:08 · sessao 07d4b231675a · - · - · origem=Ronald (tmux)

Inventário complementar, em ordem do efeito à origem:

| Decisão | Predicado e origem efetivos | Juiz registrado | Caso pela regra |
|---|---|---|---|
| Somar noturno normal e folga | `ResultadoMes.horas_noturnas`: soma `minutos_noturnos_legais` de `periodos + periodos_ft`, divide por 60 (`app/ponto/motor_calculo_v2.py:321`). | SEM JUIZ específico; registro amplo do par em `app/core/juizes.py:1194`. | `app/ponto/tests/test_dia_pago_soma.py::LavraDaFolgaTrabalhadaTest::test_MORDE_no_dia_com_OS_DOIS_as_horas_somam_e_as_rubricas_seguem_a_lei_de_cada_um:260`; regra cl.38-d/Art.73 em `:179`, assert numérico em `:273`. |
| Quantificar janela noturna | Localiza pontas; `inicio >= fim` ou duração `>26h` retorna zero; soma interseções 00–05 e 22–24 (`app/ponto/motor_calculo_v2.py::minutos_noturnos:89`). | SEM JUIZ específico; `app/core/juizes.py:1194` registra o par. | Janela: `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F2_borda_utc_turno_21_03:1330`, esperado 300/342,86 em `:1346`. **Teto 26h: SEM CASO pela regra**; `::AbsurdoTest::test_A7_par_de_26h_exatas_fecha_e_26h01_nao:2278` declara medir comportamento atual em `:2287`. |
| Prorrogar após 05h | `prorrogacao and max(inicio,00h)<05h and fim>05h` estende segmento até `fim` (`app/ponto/motor_calculo_v2.py::minutos_noturnos:110`). | SEM JUIZ específico. | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F4b_a_prorrogacao_pos_05h_e_cadastro_e_nao_default:1420`, esperado 480/548,57 em `:1441`. |
| Descontar pausa noturna | Recorta pausas ao par, une sobreposições; só soma trecho com incremento positivo de `minutos_noturnos`; desconto limitado ao todo (`app/ponto/motor_calculo_v2.py::minutos_noturnos_dos_intervalos:164`, `:191`, `:196`). | SEM JUIZ específico da conta; juiz de intervalo registrado em `app/core/juizes.py:214`. | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O263_intervalo_batido_nao_recebe_adicional_noturno:2689`, esperado 360/411,43 em `:2700`. |
| Aplicar hora de 60 ou 52,5 min | `1.0 if hora_reduzida_afastada else FATOR_HORA_NOTURNA`; multiplica relógio (`app/ponto/motor_calculo_v2.py::MotorBase.fator_hora_noturna:992`, `::calcular_noturnas:986`). | SEM JUIZ específico do fator. | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F4c_a_hora_reduzida_afastada_muda_o_fator_nao_o_relogio:1446`, esperado 360/360 em `:1463`. |
| Escrita normal dos minutos | `_escrever_noturnas` atribui ambos os campos (`app/ponto/motor_calculo_v2.py:1003`). Chamado por 12x36 em `:2792`, partido em `:2896`. | Registro amplo do par: `app/core/juizes.py:1194`; escritor específico não registrado. | Casos F4/F4c acima exercem o resultado (`app/ponto/tests/test_o280_bateria_da_lei.py:1416`, `:1463`). |
| Zerar dia sem previsto não autorizado | Exige folga ou feriado, empresa com janela, previsto zero, ausência de autorização; folga sem escala certa retorna antes; zera minutos noturnos via `setattr` (`app/ponto/motor_calculo_v2.py::MotorBase._dia_sem_previsto_fora_da_janela:1448`, `:1469`, `:1473`). | SEM JUIZ específico desse bloqueio. | SEM CASO confirmado para o número noturno desse bloqueio [não verificado]. |
| Cortar pontas antes da conta | `calcular_periodo` chama `_janela_do_par`, recorta intervalos e então calcula noturno (`app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2759`, `:2764`, `:2792`). | SEM JUIZ específico da janela. | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O272b_com_a_janela_LIGADA_o_recorte_nao_desconta_duas_vezes:2552`; esperado trabalhado 720 e pausa descontada zero em `:2600`; não mede noturno. |
| Dispensar janela para folga/cadastro incompatível | Folga retorna `None`; empresa/marco ausente retorna `None`; duas pontas além do teto do cadastro recusam janela, por par e por dia (`app/ponto/motor_calculo_v2.py::MotorBase._janela_do_dia:1526`, `:1530`, `:1541`, `:1562`, `:1606`). | Folga: pergunta registrada em `app/core/juizes.py:184`; L-084/janela específica: SEM JUIZ. | `app/ponto/tests/test_bob_dia_pago_se_contradiz.py::DiaPagoSeContradizTest::test_caso2_col577_L084_do_dia_sem_corte_e_com_o_par_posicional:157`, esperado 438/zero fora em `:161`; não mede noturno. |
| Honrar autorização | Estado vigente `autorizado` evita clipe nas duas pontas (`app/ponto/motor_calculo_v2.py:1416`, `:1697`, `:1739`). | SEM JUIZ específico. | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O273_recusa_revogada_nao_move_o_dia_e_a_autorizacao_move:2606`; regra explícita 720 sem decisão/760 autorizado em `:2616`. |
| Piso e extra declarada das pontas | Sobra `max(0,minutos-extra)`; sobra `<=piso` preserva batida; senão usa marco deslocado (`app/ponto/janela_he.py::minutos_fora_da_extra:73`, `::entrada_efetiva:99`, `::saida_efetiva:149`). Saída desativada preserva ponta (`app/ponto/motor_calculo_v2.py:1734`). | SEM JUIZ específico. | Caso B-O273 acima cobre entrada sem extra declarada (`app/ponto/tests/test_o280_bateria_da_lei.py:2612`); cobertura numérica de extra declarada/saída noturna [não verificado]. |
| Dia aberto não recebe noturno dos pares fechados | Sem saída, `calcular_periodo` retorna período com zeros; crédito posterior calcula par fechado mas copia somente trabalhadas e fora da janela (`app/ponto/motor_calculo_v2.py:2752`, `:2553`, `:2559`). | SEM JUIZ específico da exclusão noturna. | SEM CASO para noturno parcial; o caso encontrado confere 357 minutos **diurnos** (`app/ponto/tests/test_bob_dia_pago_se_contradiz.py::DiaPagoSeContradizTest::test_caso1_col349_dia_aberto_leva_a_soma_dos_pares_fechados:131`). |
| Dia da jornada e competência | Mapa usa turnos do juiz; entrada sem mapa usa data civil; período até 6h após fim anterior herda dia; exclui jornada fora de `[ini,fim]` (`app/ponto/motor_calculo_v2.py::MotorBase._dia_do_turno:889`, `::calcular_mes:2403`, `:2407`, `:2429`). | `_T1`, registro em `app/core/juizes.py:208`; herança de 6h específica: SEM JUIZ. | Caso do dia de entrada/madrugada: `app/ponto/tests/test_bateria_lavratura.py::TurnoCruzaMeiaNoiteTest::test_MORDE_o_dia_de_entrada_leva_a_saida_da_madrugada_seguinte:534`, esperado 720 em `:541`; herança 6h específica [não verificado]. |
| Ata substitui geometria | Somente resposta alinhada/fechada, com batidas reais nas pontas e sem mais batidas internas que lâmpadas; substitui períodos desse dia e recupera segundos das batidas (`app/ponto/motor_calculo_v2.py::_periodos_pelo_marco:632`, `:641`, `:669`, `:685`, `:688`, `:717`). | `periodos_do_dia` registrado em `app/core/juizes.py:1193`; guardas específicas de substituição: SEM JUIZ. | Caso pela regra específico dessas guardas [não verificado]. |
| Ler entrada/saída/pausas da ata | Exige ata/DNA alinhados; bordas acesas fornecem pontas; pausa batida exige ambas as lâmpadas e volta `>=` ida (`app/ponto/juiz_batida.py::periodos_do_dia:195`, `:230`, `:259`, `:282`). | Registro em `app/core/juizes.py:1193`. | `app/ponto/tests/test_bateria_lavratura.py::TresParesNoMesmoDiaTest::test_MORDE_as_duas_pausas_batidas_saem_do_tempo:964`, esperado 630 em `:967`; testa pausas múltiplas, não diretamente noturno. |
| Piso do intervalo julgado | Só retorna pares marcados com duração `>=PISO_INTERVALO_SEGUNDOS` (`app/ponto/turnos.py::intervalos_do_turno:544`). | `_T6`, registro em `app/core/juizes.py:214`. | Caso numérico desse piso contra regra [não verificado]. |
| Parear tipos e marcos | Papel da ata precede tipo gravado; intervalo absorvido não abre/fecha; E abre e S fecha; teto da jornada e par degenerado podem impedir fecho (`app/ponto/turnos.py::parear_turnos:1310`, `:1371`, `:1382`, `:1402`, `:1421`, `:1447`, `:1487`). | `_T1` aponta wrapper `turnos_do_colab`, em `app/core/juizes.py:208`; subdecisões não têm IDs próprios. | Teto do dia: `app/ponto/tests/test_h_portao_teto_do_dia.py::PortaoDoFechoLeOTetoDoDiaTest::test_col879_par_de_21h27_nao_fecha:32`; aderência desse teste à regra escrita [não verificado]. |
| Excluir duplicata/tipo D | Só E/S; elimina mesmo tipo quando gap `<3min` (`app/ponto/motor_calculo_v2.py::MotorBase.remover_duplicatas:902`, `:909`; constante em `:70`). | SEM JUIZ específico. | SEM CASO pela regra: bateria A3 compara caminho com/sem duplicata, em `app/ponto/tests/test_o280_bateria_da_lei.py:2163`; teste dedicado só espera geometria em `app/ponto/tests/test_motor.py::Test12x36Patologico::test_batida_dentro_gap_duplicata:744`. |
| Leitor v3 recalcula e reescreve | Com `cct is not None`, releitura calcula noturno do par e atribui ambos os minutos; rubrica v3 também calcula `legais/60` (`app/ponto/calculador/periodo.py:748`, `:754`, `:755`; `app/ponto/calculador/regras.py:844`, `:849`). | SEM JUIZ específico do recálculo noturno. | `app/ponto/tests/test_o280_bateria_da_lei.py::V3ReapontarTest::test_i26_H1_a_janela_noturna_do_PAR_e_o_intervalo_gozado:5966`, esperado 420/480 sem pausa e 360/411,43 com pausa em `:5986`. |

A decisão de **26h na janela noturna** permanece independente do teto atual do pareador: `minutos_noturnos` mantém o literal `26*3600` (`app/ponto/motor_calculo_v2.py:94`), enquanto o fecho de turno lê `_teto_da_jornada` e pode voltar a 26h por L-084 (`app/ponto/turnos.py:1433`, `:1444`).

## 14/11 10:08 · sessao 0752a4a800ac · - · - · origem=Ronald (tmux)

Há mais três escritores diretos fora de `app/`, todos lendo valores de snapshot:

| Escritor | Origem e gravação | Condição |
|---|---|---|
| `bin/restore_o263_comp10.py::<módulo>:48` | `VALOR` vem de `CAMPOS` importado em `:21`, filtrado em `:27`; `setattr(f,c,r[c])` em `:49`, `save(update_fields=VALOR)` em `:50`. `DiaPago(**r)` em `:34`, `bulk_create` em `:56`. | `DRY` default `True` em `:23`; escrita no `else` de `:39`. |
| `bin/restore_relavratura_10_2026.py::<módulo>:79` | `CAMPOS` importado em `:29`, `VALOR` em `:34`; `setattr` em `:80`, `save(update_fields=VALOR)` em `:81`. `DiaPago(**r)` em `:61`, `bulk_create` em `:86`. | `DRY` default `True` em `:31`; escrita no `else` de `:68`. |
| `bin/restore_badge_linha_09_10.py::<módulo>:38` | Lista de campos da foto em `:27`; valores por `zip(campos,l['val'])` em `:38`, `DiaPago(**kw)` em `:39-41`; `bulk_create` em `:63`. | `DRY=True` literal em `:23`; escrita no `else` de `:58`. |

Os dois primeiros incluem `horas_folga_trabalhada` pela lista `app/ponto/management/commands/aplicar_09_corte_b.py:70`. O terceiro aceita a lista de campos do snapshot; presença concreta do campo na foto **[não verificado]**. Execução desses scripts e valores restaurados **[não verificado]**.

A afirmação anterior de censo completo apenas em `app/` estava incompleta: esses scripts persistem fora da porta `restaurar_fechamento`, conforme `bin/restore_o263_comp10.py:50`, `bin/restore_relavratura_10_2026.py:81` e `bin/restore_badge_linha_09_10.py:63`.

`bin/sombra_mascara_pessoal.py:44-64` tem escrita genérica, mas a configuração de `FechamentoMensal` limita-se a `observacao` em `bin/sombra_regras_pessoais.py:110`; esse caminho não configura a rubrica examinada.

## 14/11 10:08 · sessao 567935764c13 · - · - · origem=Ronald (tmux)

Levantamento somente por leitura; execução dos casos e fatos de produção: [não verificado].

| decisão | predicado e dado de entrada | arquivo::função:linha | registro efetivo | caso numérico pela regra |
|---|---|---|---|---|
| Universo de batidas | Entra `retratada_em IS NULL`; `aprovada` não é filtro; recorte por timestamp | `app/ponto/turnos.py::turnos_do_colab:1863`; `::batidas_apuraveis:2876` | **SEM JUIZ** nominal para esses filtros; wrapper `turnos_do_colab` registrado em `app/core/juizes.py:208` | **SEM CASO** que tenha sido conferido contra regra escrita para esse filtro [não verificado] |
| Nascimento: deduplicação | Mesmo tipo em ±60 s retorna existente; mesma chave colaborador/timestamp também retorna existente | `app/ponto/registro_batida.py::registrar_batida:106` e `:120` | **SEM JUIZ** nominal em `app/core/juizes.py:182` | `app/ponto/tests/test_registrar_batida.py::test_mesmo_tipo_60s_dedup:67` apenas testa comportamento; **SEM CASO** pela regra [não verificado] |
| Nascimento: competência/colisão | Sem fato de chão consulta `competencia_fechada`; oposta recusa sem autoridade humana, salvo marco próprio em ±5 min. Janela oposta 300 s com escala/marcos, senão 60 s | `app/ponto/registro_batida.py::registrar_batida:130`, `:145`; `::_janela_oposto:29`; `::_casa_marco_proprio:42` | **SEM JUIZ** nominal dessas decisões em `app/core/juizes.py:182` | **SEM CASO** pela regra conferido [não verificado] |
| Vínculo do dia | Geradora nomeada pela célula vale se encontrada e dentro de início/fim; sem geradora usa candidatos cobertos, máximo `(ativa,data_inicio)` | `app/escala/alimentacao.py::vinculo_do_dia:269` | `_E1`, `app/core/juizes.py:1302`, registro `:1310` | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F5_troca_de_vinculo_no_meio_da_competencia:1867`: L-021/L-099, 16 h trabalhadas, atraso 1 h, antecipada 2 h, FT 0 (`:1944`) |
| Cobertura do vínculo | Início existente e ≤data; fim nulo ou ≥data; `ativa or data_fim is not None` | `app/escala/alimentacao.py::vinculo_cobre_a_data:214`; `::tem_vigencia_declarada:196` | `_E2`, `app/core/juizes.py:1303`, registro `:1311` | `app/ponto/tests/test_o280_bateria_da_lei.py::AbsurdoTest::test_A10_batida_fora_da_vigencia_do_vinculo:2340`: L-099, fora da vigência, falta/horas_falta/extra 0 (`:2353`); não cobre todos os ramos |
| Dia de trabalho/fase | Célula existente devolve `trabalha`; sem célula calcula. Âncora válida e sem foto mensal: módulo 2 no 12x36, módulo 3 no 24x48. Foto vence e complementa ciclo conhecido; desconhecido continua pela paridade da foto mais próxima | `app/escala/models.py::EscalaColaborador.eh_dia_trabalho:1094`; `::eh_dia_trabalho_calculado:1289`, `:1309`, `:1364` | chave “o dia e de trabalho para o vinculo?”, `app/core/juizes.py:184` | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F_FOTO_folga_avulsa_no_mes_nao_apaga_o_folga_dia_semana:2058` (L-023, 4 faltas/32 h); `::test_F_FOTOb_foto_parcial_no_12x36_a_fase_sai_da_foto_nao_da_ancora:2090` (L-022/L-023, previstos 15/17/19, 3 faltas/33 h) |
| Folga lançada aplicável | Nula não vale; sem calendário de origem vale; calendário exige `folga.escala_colaborador_id == ec_do_dia.pk` | `app/ponto/precedencia.py::folga_lancada_vale_no_dia:235`; seleção `::folga_lancada_do_dia:216` | chave “este dia tem folga lancada que vale para o colaborador?”, `app/core/juizes.py:204` | Testes de predicado existem em `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `:50`; **SEM CASO numérico de folha** conferido [não verificado] |
| Alcance do feriado | Nacional sempre; estadual exige UF; municipal exige UF e município normalizado. Lugar é município do posto do vínculo de cada dia | `app/core/feriados.py::alcanca:59`; `::feriados_do_colab:161` | chave “o dia e feriado para o colaborador?” → `eh_feriado`, `app/core/juizes.py:188`; casca `app/core/feriados.py:105` | **SEM CASO** numérico conferido para alcance por lugar [não verificado] |
| Feriado transforma previsto em folga | Não transforma se `trabalha_em_feriado`, sem escala ou ciclo fora de `CICLOS_FERIADO_VIRA_FOLGA`; senão pergunta feriado pelo lugar do vínculo | `app/escala/models.py::EscalaColaborador.feriado_vira_folga:1389`; chamador `::eh_dia_trabalho_calculado:1369` | **SEM JUIZ** nominal para `feriado_vira_folga`; registro mais amplo de precedência em `app/core/juizes.py:186` | `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py::FeriadoTrabalhadoSaiComoFeriadoTest::test_feriado_trabalhado_vai_para_HE100_feriado:117`: regra L-086 escrita `:14`, esperado HE100 feriado 4 h/FT 0 `:123`; controle folga comum 4 h `::test_MORDE_domingo_de_folga_segue_folga_trabalhada:158` |
| Precedência e ausência | Ausências excluem apenas rejeitada neste caminho; composição usa faixa da ausência, feriado salvo só pagamento, folga lançada/previsto falso e trabalho previsto verdadeiro; ordena por `PRECEDENCIA` | `app/ponto/precedencia.py::fatos_do_periodo:273`, `:376`, `:379`, `:396`; ordem constante `:46` | chave “qual fato vence o dia...?”, `app/core/juizes.py:186` → `fatos_do_dia`, casca `app/ponto/precedencia.py:242` | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956`: Lei 605/49, justificada com batidas vale 480 min, falta/abono/extra 0 (`:1966`); **não cobre a tabela inteira** |
| Ausência para folha | `para='folha'` exige aprovada; cobrança exclui rejeitada. Cobre data única, intervalo fechado ou aberto somente nos tipos permitidos; desempate `-data,-pk` | `app/ponto/turnos.py::_pelo_efeito:2516`; `::ausencia_cobre:2554` | chaves folha/cobrança `app/core/juizes.py:190`; cobertura do período `:253` | Caso F6 acima cobre justificativa preservando trabalho; **SEM CASO** conferido que prenda numericamente todos os filtros/status/desempate [não verificado] |
| Marcos congelados e alinhamento | Marcos do DNA não vazio vencem template; senão template. `_match_marcos` tenta papel do turno/alternância; abstém com L-084 nas duas pontas; depois raio de atribuição e alinhamento; cluster impede marco distante ocupado por batida com vizinho mais próximo | `app/escala/models.py::EscalaColaborador.marcos_do_dia:1124`; `app/escala/utils.py::_match_marcos:596`, `:609`, `:625`, `:644` | marcos `app/core/juizes.py:213`; atribuição `:212` | `app/ponto/tests/test_bateria_lavratura.py::TurnoCruzaMeiaNoiteTest::test_MORDE_o_dia_de_entrada_leva_a_saida_da_madrugada_seguinte:534` (720 min; regra escrita `:504`); `::test_MORDE_a_folga_do_12x36_nao_recebe_a_madrugada_da_vespera:545` (folga 0 min); específico cluster **SEM CASO** conferido [não verificado] |
| Escala certa: LIMBO/A2/A8 | Recusa vínculo LIMBO ou A2/A8 na competência do dia; A8: ≥3 dias com `trabalha is False`, turno e ≥2 batidas; A2: ≥2 gaps de 1 ou 2 dias dentro de A8. `so_escala=True` retorna antes do motor e chamados | `app/escala/services/escala_certa.py::motivo_escala_errada:31`, `:38`, `:43`; `app/ponto/services/esmeril_espelho.py::_escala_do_colab:71`; `::assinaturas_do_colab:101` | **SEM JUIZ** nominal dessas funções/assinaturas em `app/core/juizes.py:182` | `app/escala/tests/test_escala_certa_deterministica.py::EscalaCertaDeterministica::test_MORDE_pela_porta_do_dinheiro_a_lavra_vazia_nao_paga_100:73`: L-099/regra de exclusão escrita `:1`, FT esperada 0 (`:87`). Cobertura separada do limiar A2/A8 por número de horas: **SEM CASO** conferido [não verificado] |
| Pareamento, intervalo e dia do turno | Ata responde antes do tipo gravado; `Xi/X` internos. Intervalo por proximidade ≤90 min quando não mais perto da borda; modo duração S→E ≤3 h e prova/posição conforme ramo. Data normalmente início; madrugada só vai à véspera com jornada cruzando meia-noite e dentro do envelope | `app/ponto/turnos.py::parear_turnos:1310`, `:1356`, `:1382`; `::_marca_intra_duracao:413`, `:453`; `::_data_do_turno:285` | wrapper data/turno `app/core/juizes.py:208`; intervalo `:214`; auxiliares não têm registro próprio | `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py::PausaQueAAtaDeclaraTest::test_RED_a_pausa_vale_181_min_e_o_liquido_544:679`: regra escrita `:598`, L-085 `:609`, 725−181=544 min (`:687`,`:690`); `app/ponto/tests/test_bateria_lavratura.py::TurnoCruzaMeiaNoiteTest::test_MORDE_a_folga_do_12x36_nao_recebe_a_madrugada_da_vespera:545` |
| Exclusões de geometria: eco/teto/par curto | Eco flush exige chegada ≤120 s, evento entre 1800 s e 14 h, viva até 600 s do relógio e anterior atrasada ≥1800 s. Fecho exige duração ≤jornada dos marcos+4 h, ou envelope sem marcos; L-084 nas duas pontas restaura 26 h; par ≤60 s não forma turno | `app/ponto/turnos.py::_marca_ecos_flush:368`; `::_teto_da_jornada:78`; `::parear_turnos:1430`, `:1444`, `:1447`, `:1487`; `::_par_degenerado:130` | **SEM JUIZ** nominal para eco, teto e piso em `app/core/juizes.py:182`; wrapper registrado `:208` | Eco: `app/ponto/tests/test_eco_flush_mesmo_turno.py::EcoSoNoMesmoTurnoTest::test_col478_E_de_24h_antes_nao_faz_da_viva_um_eco:29` escreve regra L-087/L-110 e 716 min `:4`, mas asserção compara par `:37`, sem conferir 716. Teto: `app/ponto/tests/test_o280_bateria_da_lei.py::AbsurdoTest::test_A6_PINADO_o_par_alem_do_teto_do_dia_NAO_fecha:2220`, regra 960<1020, trabalhado 0 `:2223`, asserção `:2247`, marcado expectedFailure `:2219`. Piso: métodos A13/A14 da mesma classe, linhas `:2394`,`:2417` (conteúdo numérico ainda não conferido [não verificado]) |

Dois cuidados para a consolidação:

- Não conte A3 duplicadas como caso pela regra: a expectativa é copiada de `leitura(r0)`, isto é, paridade entre caminhos (`app/ponto/tests/test_o280_bateria_da_lei.py:2163`).
- O realizado da ata pode usar borda posicional e janela declarada (`app/ponto/turnos.py::realizado_dos_turnos:725`,`:756`), porém o motor não necessariamente consome esse resultado: a bateria declara essa diferença nos métodos `ContratoDaTrocaTest::test_C6_o_motor_NAO_alcanca_a_borda_posicional:3183` e `::test_C6b_dois_leitores_do_MESMO_dia_discordam:3210` de `app/ponto/tests/test_o280_bateria_da_lei.py`. Não transportar automaticamente esses predicados para o número de FT sem conferir o adaptador.

## 14/11 10:08 · sessao 9e1d272d061a · - · - · origem=Ronald (tmux)

Escritores encontrados de `FechamentoMensal.horas_noturnas`:

| Caminho | Evidência |
|---|---|
| Derivação canônica | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:918`: `round(resultado.horas_noturnas, 2)`; persiste por `save()` quando aberto em `:1137-1144`, por `update(horas_noturnas=...)` no outro ramo em `:1160-1163`. |
| Inicialização | Campo decimal, duas casas, default zero: `app/ponto/models.py::FechamentoMensal:413`; `get_or_create` em `app/ponto/services/fechamento.py:898-901`. |
| Restauração genérica | `app/ponto/services/fechamento.py::restaurar_fechamento:1374-1393`: aceita campos reais do modelo, exclui só identidade/competência e executa `update(**_upd)`. |
| Chamador que restaura AN | `app/ponto/management/commands/aplicar_09_corte_b.py::Command.handle:265-268`: restaura os campos completos do snapshot; `horas_noturnas` está em `CAMPOS` em `:68`. A restauração de `:220` toca só banco de horas. |
| Chamador que restaura AN | `app/ponto/management/commands/desvio_o68b.py::Command.handle:195-199`: restaura todos os campos não nulos do snapshot, exceto mes/ano. |
| Escritor direto fora da porta | `bin/restore_o263_comp10.py:48-50`: `setattr` por campo e `f.save(update_fields=VALOR)`; `VALOR` vem de `CAMPOS` em `:21,27`, incluindo AN. Só executa se `DRY` falso (`:37-40`). |
| Escritor direto fora da porta | `bin/restore_relavratura_10_2026.py:79-81`: mesmo escritor genérico; definição `VALOR` em `:29,34`; ramo `DRY` em `:66-69`. |

O comando `aplicar_09_he_intra` **não escreve AN**: `ALVO` exclui o campo (`app/ponto/management/commands/aplicar_09_he_intra.py:39-40`), `mex` filtra por `ALVO` (`:117`) e só esses campos chegam à restauração (`:139-150`). `diff_reclassificar_partido` restaura apenas `saldo_banco_horas` em `app/ponto/management/commands/diff_reclassificar_partido.py:224-226`.

Leitores e reinterpretadores:

| Leitor | O que faz |
|---|---|
| Porta HTTP do TXT | **Deriva online**: `app/folha/views.py::gerar_dominio_txt:208-212` chama recálculo em leitura e classifica objetos derivados; injeta os mesmos objetos em `montar_linhas_export` em `:268`. |
| API da montagem sem injeção | Lê gravado em `app/folha/export.py::montar_linhas_export:717-722`; evento AN lê `fech.horas_noturnas` em `:447`. |
| Cartão em competência | `app/relatorios/cartao_pela_celula.py::totais_da_folha:69-76` verifica existência do gravado, mas obtém números por `fechamento_lido`; este chama `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)` em `app/ponto/services/fechamento.py:1759-1761`. Mapeia `adicional_noturno` para `total_noturnas` em `app/relatorios/cartao_pela_celula.py:90-100`. |
| Espelho/tela | Roda `apurar_por_fatia` em `app/ponto/services/espelho.py:1163-1165`; soma AN dos períodos em `::somar_periodos:82-99`; em competência sobrescreve resumo via `folha_manda` em `:1283-1284`. |
| PDF | Roda apuração por fatias em `app/relatorios/pdf_espelho.py:418-419`; soma períodos em `:501-502`; aplica `folha_manda` em `:628`; imprime badge de AN em `:782`. |
| Sem fechamento, com DiaPago | `app/relatorios/cartao_pela_celula.py::folha_manda:341-346` escolhe lavratura; `_totais_da_lavratura` soma DiaPago em `:142-143` e lê AN em `:176`. |
| Portão cartão × TXT | **Lê gravado**, diferente do leitor online: `app/relatorios/cartao_pela_celula.py::divergencia:250-253`; compara `total_noturnas` em `:265-269`, diferença `>0,02h`. |
| Portão linha × badge | `app/folha/porta_export.py::linha_x_badge:261-275`: só mede badge com fonte `folha`; compara soma de DiaPago com badge. Par AN em `:246`. |
| Tela lista fechamento | Lê objetos gravados em `app/ponto/views.py::fechamento_linhas:1079-1085`; repassa em `app/ponto/services/fechamento_lista.py::montar_resumos:80,107`; mostra AN em `app/templates/ponto/_fech_linha.html:31`. |
| Painel individual | Obtém `fechamento_lido` em `app/ponto/views.py::painel_fechamento:993`; exibe em `app/templates/ponto/partials/painel_fechamento.html:33`. |
| Ranking individual | Deriva por `fechamento_lido` em `app/folha/services/ranking_he.py:206`; publica AN em `:225`. |
| Relatório horas/mês e CSV | Lê gravado em `app/relatorios/views.py:68-73`, CSV `:94`, soma `:108`, template `app/templates/relatorios/horas_mes.html:68`. |

Decisões adicionais do fechamento:

- **Quem entra no universo do cálculo**: `app/ponto/services/fechamento.py::universo_da_competencia:431-444`: inclui não desligados ou desligados com `data_demissao >= início da janela da empresa`; filtra empresa se fornecida. SEM JUIZ nominal para essa função no registro pesquisado; janela é chamada existente.
- **Fatia e atribuição do cadastro**: `::apurar_por_fatia:189-193` consulta `vinculo_do_dia` para geradoras; `:206-207` recorta por competência, início de operação e vigência; `:229-230` só atribui dia sem dono quando exatamente um vínculo o cobre; `:253-261` filtra batidas pelo dia local e pertencimento à fatia; `:286-300` calcula e concatena/soma resultados. SEM JUIZ nominal para `apurar_por_fatia`.
- **Ausência/justificativa**: `::recalcular_fechamento_mes:660-664` inclui justificativas aprovadas pela data do turno; `:683-701` usa cobertura de ausência, ignora parcial paga como abono integral, adiciona ABONA às justificadas e suprime qualquer efeito diferente de DESCONTA. `_montar_motor:731-740` só põe dia previsto se `esc.eh_dia_trabalho` e não suprimido; carrega CCT na **data inicial de cada fatia**.
- **Batida fora de toda fatia**: `::recalcular_fechamento_mes:792-796` seleciona dias com batida sem cobertura de fatia; `:838-843` acumula **só minutos trabalhados**, e `:902-903` os adiciona a trabalhadas; AN continua exclusivamente `resultado.horas_noturnas` em `:918`. SEM JUIZ para essa exclusão de AN.
- **Arredondamento e persistência**: `:918`, `:1128-1163`; SEM JUIZ nominal.
- **Guarda de escrita da competência**: configuração `(9,2026)` em `app/ponto/services/fechamento.py:36-42`; exportadas só são buscadas se escrita ou lavratura em `:539-540`; exclui colaboradores das exportadas em `:560-564`. SEM JUIZ nominal dessa guarda.

Registro real, sem ampliar escopo por docstring:

- `_TT5` é a pergunta de totais da competência (`app/core/juizes.py:719`) e aponta **somente** para `folha/export.py::eventos_do_fechamento` (`:732`).
- `_K1/_K2` apontam para `classificar_export` (`app/core/juizes.py:319-320`).
- Juiz do par não tem constante `_Q`: chave literal `JUIZES['batida']['quantas horas este PAR (turno) vale?']` aponta para `MotorBase.calcular_periodo` (`app/core/juizes.py:1192-1194`).

Caso pela regra das fatias, com número independente:

- `app/ponto/tests/test_regua_por_fatia_na_janela.py::DuasReguasNaJanelaTest::test_a_regra_de_cada_fatia_isolada_e_a_que_a_lei_diz:160`; assert numérico em `:181`.
- `...::test_RED_a_testemunha_paga_a_regua_de_hoje_na_janela_inteira:186`; assert **18,29h** em `:198-201`.
- `...::test_RED_o_cartao_em_pdf_mostra_o_mesmo_numero_da_regra:204`; assert **18,29h** em `:214-216`.
- Regra escrita com Art.73/Súmula60 e Art.59-A e respostas **10,29 + 8,00 = 18,29h** em `:31-42`.
- Para restauração, arredondamento, exclusão de AN dos órfãos e predicados do portão comparador, caso numérico contra regra específica: **SEM CASO localizado**. [não verificado]

## 14/11 10:07 · sessao 40c452a14830 · - · - · origem=Ronald (tmux)

Predicados e escritores encontrados no motor, com endereços da cópia:

| Decisão | Código efetivo | Grava / repercussão | Registro |
|---|---|---|---|
| Escolha da classe de cálculo | `app/ponto/motor_calculo_v2.py::get_motor:3640`: sem escala → `Motor12x36`; intermitente → `MotorIntermitente`; base 12x36 com início → `Motor12x36ComEscala`; base partida com horários e `not _eh_partido` → comercial ou 12x36 conforme cruzamento | Instância que calcula os períodos | SEM JUIZ específico; geometria partida registrada em `app/core/juizes.py:220`, cruzamento em `:216` |
| Feriado simples pelo ciclo | `app/ponto/motor_calculo_v2.py::get_motor:3693`: sem override, `ciclo not in CICLOS_FERIADO_SIMPLES`; conjunto contém somente `12x36` em `:3623` | `feriado_em_dobra` | `_F3`, registro `app/core/juizes.py:303` |
| Ativação da régua CCT | `app/core/regua_cct.py::get_motor_cct:444`: parâmetro global, texto limpo igual a `'1'`; desligada, só regime `clt` recebe prorrogação legal pelo ciclo em `:453-457` | Parâmetros do motor | SEM JUIZ específico localizado no registro `app/core/juizes.py` |
| Aplicação/vigência CCT | `app/core/regua_cct.py::_aplicacao_vigente:254`: aplicação ativa da empresa, praça ou empresa inteira; sindicato com início/fim incluindo competência; praça vence; empate no nível → piso em `:261-275` | Sindicato ou ausência de convenção | SEM JUIZ específico localizado |
| Regime e parâmetros CCT | `app/core/regua_cct.py::regua_para:329`: vínculo do dia resolve praça/ciclo; `clt` → piso declarado; diferente de `cct` → piso; aplicação ausente/empatada → piso; CCT encontrada força `regua_excedente='legais'` em `:354-378` | Dicionário da régua | SEM JUIZ específico localizado; vínculo do dia tem registro próprio [não verificado neste recorte] |
| CCT noturna e dobra | `app/core/regua_cct.py::get_motor_cct:466`: prorrogação sobrescreve argumento; hora reduzida afastada exige flag, ciclo 12x36 e trava desligada `:470-472`; dobra 12x36 exige cláusula em `:477-478` | Flags lidos pelo motor | SEM JUIZ específico localizado; `_F3` cobre ciclo em `app/core/juizes.py:303` |
| Períodos pelo pareador | `app/ponto/motor_calculo_v2.py::turnos_via_autoridade:467`: marcos template + DNA; base sem marcos de intervalo os retira em `:475`; papel da ata alimentado em `:496-506` | Turnos e pausas usados no dinheiro | `turno/marcos`, registro de turno em `app/core/juizes.py:208`, intervalo em `:214` |
| Substituição pela ata | `app/ponto/motor_calculo_v2.py::_periodos_pelo_marco:632`: rejeita ata desalinhada/aberta, sem pontas ou sem correspondência com Batida; rejeita se batidas não retratadas dentro do período excedem luzes acesas `:641,669-675`; substitui períodos do dia em `:685-721` | Períodos e pausas; conserva segundos reais via `:606-609` | `batida` / pergunta dos períodos, registro `app/core/juizes.py:1193`; as guardas de admissibilidade não têm entrada própria localizada |
| Competência e dia | `app/ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2403`: dia do juiz, fallback data civil; intervalo entre entrada e último fim de 0 a 6h herda dia em `:2405-2408`; exclui fora de `data_ini/data_fim` em `:2429-2432` | Inclusão do período | Turno registrado em `app/core/juizes.py:208`; herança de 6h SEM JUIZ específico |
| Folga retira do total normal | `app/ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2447`: vínculo não intermitente pergunta `eh_dia_trabalho(...) is False`; fallback usa folga semanal e datas previstas; período vai a `periodos_ft` ou `periodos` em `:2468-2494` | HE100 mensal soma somente `periodos`, `app/ponto/motor_calculo_v2.py:328-344` | Trabalho do vínculo em `app/core/juizes.py:184`; fallback semanal SEM JUIZ específico |
| Folga semanal prevista | `app/ponto/motor_calculo_v2.py::MotorBase._folga_pelo_dia_previsto:950`: não é folga se weekday não pertence ao conjunto ou se dia foi previsto explicitamente | Classificação FT e dispensa da janela | SEM JUIZ específico |
| Empresa alcançada pela janela HE | `app/ponto/motor_calculo_v2.py::MotorBase._empresa_da_janela_do_dia:1390`: exige empresa, janela ativa, início declarado e dia igual/posterior ao início | Habilita clipagem | SEM JUIZ específico |
| Marco e alcance da janela | `app/ponto/motor_calculo_v2.py::MotorBase._janela_do_dia:1526`: folga não recebe janela; usa data civil da entrada; exige método de marcos e `hi`; resolve saída e cruzamento em `:1546-1550` | Dicionário da janela | Marcos em `app/core/juizes.py:213`; alcance/âncora da janela SEM JUIZ específico |
| Cadastro não descreve | `app/ponto/motor_calculo_v2.py::MotorBase._cadastro_nao_descreve:1946`: ambas distâncias não nulas, ambas absolutas maiores que teto; exige `and` | Retira janela em `:1562` e `:1606`; pontualidade em `:1988` | Pontualidade em `app/core/juizes.py:215`; guarda da janela SEM JUIZ específico |
| Autorização do dia | `app/ponto/motor_calculo_v2.py::MotorBase._carregar_decisoes_he:1416`: lê `DecisaoHE.objects.vigentes()`; clipadores dispensam corte somente quando estado igual a `'autorizado'`, `:1697` e `:1739` | Entrada/saída efetivas | SEM JUIZ específico |
| Extra declarada desloca limite | `app/ponto/motor_calculo_v2.py::MotorBase._extra_declarada:1356`: pergunta função do vínculo/template; converte parcelas em inteiros; exceção → `(0,0)`; injeta em `:1646-1653` | Antes/depois no limite da janela | SEM JUIZ específico |
| Entrada efetiva | `app/ponto/janela_he.py::entrada_efetiva:92`: `sobra=max(0, bruto-extra)`; se sobra ≤ piso conserva; acima retorna marco deslocado; teto só sinaliza, `:99-103` | Duração, noturno e HE calculados sobre entrada efetiva em `app/ponto/motor_calculo_v2.py:2759-2761` | SEM JUIZ específico |
| Saída efetiva | `app/ponto/motor_calculo_v2.py::MotorBase._saida_efetiva:1734`: exige janela, saída ativa e marco; regra pura em `app/ponto/janela_he.py::saida_efetiva:143-153` usa sobra após extra e piso | Duração, noturno e HE sobre saída efetiva | SEM JUIZ específico |
| Dia sem previsto bloqueado | `app/ponto/motor_calculo_v2.py::MotorBase._dia_sem_previsto_fora_da_janela:1448`: folga ou feriado, empresa alcançada, previsto exatamente zero, sem autorização; folga sem escala certa escapa em `:1465-1470`; demais zeram por `setattr` em `:1473-1476` | Zera trabalhado, HE100, feriado e noturno; colapsa segmento em `:1482-1483` | SEM JUIZ específico para o bloqueio |
| Intervalo recortado | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2764`, `MotorTurnoPartido.calcular_periodo:2891`, `MotorComercial.calcular_periodo:3237` | Pausas recortadas à janela efetiva antes das contas | PAR registrado em `app/core/juizes.py:1194` |
| Intervalo e duração 12x36 | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2767`: somente duração >360 desconta intra real; suprimido ≤10 é zerado em `:2776`; trabalhado `max(0,duracao)` em `:2785` | Trabalhado e indenização que altera limite | PAR, `app/core/juizes.py:1194` |
| Intervalo e duração comercial | `app/ponto/motor_calculo_v2.py::MotorComercial.calcular_periodo:3239`: somente duração >240 desconta intra real; depois duração >360 calcula indenização; suprimido ≤10 zera | Trabalhado em `:3256` | PAR, `app/core/juizes.py:1194` |
| Limite com intervalo indenizado | `app/ponto/motor_calculo_v2.py::MotorBase._limite_com_suprimido:1785`: sem suprimido conserva limite; senão soma `min(suprimido, intervalo_declarado)` em `:1803` | Limite abatido do trabalhado para HE | SEM JUIZ específico |
| Previsto e ausência do previsto | `app/ponto/motor_calculo_v2.py::MotorBase._previsto_do_dia:1232`: memo, depois juiz por colaborador; erro/colaborador ausente → `None`; `_jornada_do_dia:1273` usa template somente em `None`; resultado zero faz HE zero em `:2835-2841` | Limite HE | Previsto registrado em `app/core/juizes.py:185`; fallback SEM JUIZ específico |
| Feriado | `app/ponto/motor_calculo_v2.py::MotorBase.eh_feriado:963`: conjunto injetado é soberano; sem injeção usa fallback; `_feriado_dobra_no_dia:934` exige flag e feriado | Toda duração vira HE100 e parcela feriado | Feriado em `app/core/juizes.py:188`; dobra `_F3` em `:303`; borda do feriado `_F4` declarada SEM JUIZ em `:350-352` |
| Tolerância HE | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2845`: `0<extra<=10` zera; TP `calcular_mes:3151` e comercial `calcular_mes:3414` conservam zero até10; acima contam tudo | Excedente | PAR registrado para 12x36 em `app/core/juizes.py:1194`; por DIA SEM JUIZ específico |
| Degrau HE100 | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2847-2854`; comercial por período `:3286-3293`; TP por dia `:3153-3159`; comercial por dia `:3416-3422`: até120→50%; excedente de120→100% | `minutos_extra_100` | PAR para cálculo por par; por DIA SEM JUIZ específico; **SEM CASO pela regra**: a bateria recusa certificar o degrau em `app/ponto/tests/test_o280_bateria_da_lei.py:3710-3713,5994-5996` |
| Régua legais/semanal | `app/ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:3120`: semanal pula diário; legais limita jornada a480 e soma ganho noturno `:3132-3144`; semanal exclui feriado, soma seg–dom, abate2640, exige excesso >10×dias, todo excesso em50% `:3161-3187` | Excedente normal TP | SEM JUIZ específico |
| Janela noturna | `app/ponto/motor_calculo_v2.py::minutos_noturnos:89`: localiza; duração ≤0 ou >26h→0; conta00–05/22–24; prorroga se habilitado, há interseção00–05 e fim>05 em `:110` | Noturno; ganho legal pode alterar HE no TP | SEM JUIZ específico |
| Split HE noturna | `app/ponto/motor_calculo_v2.py::he_noturna:430`: exige saída/extra positiva; recua por trabalho na ponta final, desconta pausas noturnas, rateia HE noturna pela proporção50/100 em `:440-450` | `minutos_extra_100_noturna` via `_aplicar_he_noturna:1020` | SEM JUIZ específico |

Todos os escritores explícitos de `minutos_extra_100` no motor: `app/ponto/motor_calculo_v2.py:2811,2854,2935,3101,3158,3270,3293,3334,3358,3421`; escritor dinâmico zerador em `:1473-1476`. Da parcela feriado: `:2812,2936,3102,3271,3335,3359`, além do mesmo zerador dinâmico. Da parcela noturna: `:1020,2938,3337`, além do zerador dinâmico.

Leitura mensal soma somente `resultado.periodos`, dividindo minutos por60: `app/ponto/motor_calculo_v2.py::ResultadoMes.horas_extra_100:328-329`, `horas_extra_100_feriado:331-332`, `horas_extra_100_noturna:342-344`; folgas ficam separadas em `periodos_ft` por `MotorBase.calcular_mes:2492-2494`.

Casos numéricos já conferidos por leitura:

- Janela entrada: `app/ponto/tests/test_janela_he_regra.py::JanelaHeRegraTest::test_RED_06h40_com_marco_07h00_nao_gera_HE:83` exige660 trabalhados/HE0; `test_MORDE_06h52_esta_dentro_do_piso_e_NADA_muda:91` exige668; vigência em `test_MORDE_a_vigencia_protege_a_competencia_09:107` exige680.
- Autorização: `app/ponto/tests/test_janela_he_saida.py::DiaAutorizadoContaTest::test_RED_o_dia_AUTORIZADO_passa_a_contar_os_minutos_de_fora:255` exige740; `test_MORDE_a_decisao_e_do_DIA_e_nao_da_PONTA:269` exige735.
- Feriado/12x36: `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F4_feriado_em_12x36_noturno_e_hora_simples:1399`, regra aberta e assertivas em `:1404-1418`.
- Prorrogação: mesma classe, `test_F4b_a_prorrogacao_pos_05h_e_cadastro_e_nao_default:1420`, exige480 de relógio/548,57 legais em `:1441`.
- Hora reduzida afastada: mesma classe, `test_F4c_a_hora_reduzida_afastada_muda_o_fator_nao_o_relogio:1446`, exige360/360 em `:1463`.
- Ausência com batidas: mesma classe, `test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956`, exige480 trabalhados e zero faltas/abonos/HE em `:1966`.
- Pausas múltiplas: mesma classe, `test_F7_intervalo_dividido_em_dois:1968`, exige585 trabalhados,75 descontados,zero indenização/extra em `:1981`.

A enumeração completa dos predicados internos de `parear_turnos` e dos escritores de cadastro/batida não foi concluída neste recorte [não verificado].

## 14/11 10:07 · sessao b3d4c0013af5 · - · - · origem=Ronald (tmux)

Escritores encontrados:

| Escritor | Número/origem e predicado lido | Registro/caso |
|---|---|---|
| `app/ponto/services/fechamento.py::recalcular_fechamento_mes:920` | `round(resultado.horas_extra_50, 2)` atribui HE50 mensal em horas. Persiste com `save()` quando aberto (`:1137-1144`); nos outros status usa `update(...horas_extras_50=...)` (`:1160-1165`). Modo leitura retorna objeto/dict sem essa persistência (`:1134-1136`). | Round/escolha da persistência: SEM JUIZ; SEM CASO numérico pela regra localizado. |
| `app/ponto/services/fechamento.py::restaurar_fechamento:1374` | Valida nomes de campos e motivo, exclui identidades (`:1380`), escreve os valores recebidos com `update` (`:1392-1393`). Pode escrever HE50 recebida. | SEM JUIZ; SEM CASO. |
| `app/ponto/services/fechamento.py::aprovar:1335` | Muda status/carimbos e chama `save()` inteiro (`:1340`), regravando também HE50 carregada no objeto. | K1 aponta ao classificador, não a esta persistência (`app/core/juizes.py:319`). SEM CASO numérico. |
| `bin/restore_relavratura_10_2026.py::<módulo>:79` | `setattr(f,c,r[c])` e `f.save(update_fields=VALOR)` (`:79-81`); `VALOR` deriva de `CAMPOS` (`:29-34`), que inclui HE50 (`app/ponto/management/commands/aplicar_09_corte_b.py:68`). Só executa quando `DRY=False` (`:66-69`). | SEM JUIZ; SEM CASO. |
| `bin/restore_o263_comp10.py::<módulo>:48` | Mesma reposição de snapshot por `setattr` e `save(update_fields=VALOR)` (`:48-50`); `VALOR` deriva de CAMPOS (`:21-27`). Guardas alvo/DRY (`:29-40`). | SEM JUIZ; SEM CASO. |

Chamadores que decidem valores repostos pela porta:

- `app/ponto/management/commands/aplicar_09_he_intra.py::Command.handle:105`: calcula diferença entre motor com `_limite_com_suprimido` antigo e atual; seleciona `abs(delta)>0.005` nos campos ALVO (`:115-119`); grava `round(valor_snapshot+delta,2)` (`:139-150`). SEM JUIZ e SEM CASO pela regra localizado.
- `app/ponto/management/commands/aplicar_09_corte_b.py::Command.handle:259`: restaura snapshot inteiro dos colaboradores selecionados fora do escopo (`:265-268`).
- `app/ponto/management/commands/desvio_o68b.py::Command.handle:194`: restaura campos do snapshot excluindo identidades/valores nulos (`:196-199`).
- `app/ponto/management/commands/diff_reclassificar_partido.py:224`: também chama `restaurar_fechamento`; predicado completo [nao verificado].

Decisões anteriores à atribuição mensal:

| Decisão | Código efetivo | Juiz/caso |
|---|---|---|
| Universo do cálculo | `universo_da_competencia:431-444`: inclui quem não está desligado OU desligado cuja demissão alcança início da janela da empresa; desligado sem data não satisfaz comparação. | SEM JUIZ específico. Testes em `test_fechamento_escopo_competencia.py::EscopoCompetenciaFechamentoTest::{test_desligado_na_competencia_recalcula:27,test_desligado_antigo_fica_fora:56}` só verificam existência, sem número de HE: estritamente SEM CASO. | [nao verificado]
| Restrição explícita da chamada | `recalcular_fechamento_mes:556-557`: `colaborador_ids is not None`, inclusive lista vazia. | SEM JUIZ; SEM CASO. |
| Guarda da competência | `recalcular_fechamento_mes:539-540,560-564`: consulta exportadas apenas se houver escrita de FM ou DiaPago; quando competência consta da guarda e há empresas exportadas, exclui colaboradores dessas empresas. Guarda contém `(9,2026)` (`:36-43`). | SEM JUIZ; SEM CASO pela regra localizado. |
| Fatia por vigência | `apurar_por_fatia:206-207`: início `max(ini,esc.data_inicio,DATA_INICIO_OPERACAO)`; fim `min(fim,data_fim ou fim)`. | SEM JUIZ próprio do corte/composição. |
| Dono por célula/vínculo | `apurar_por_fatia:189-193,224-238`: consulta dono da célula; sem dono, só incorpora quando exatamente uma vigência cobre a data; pula fatia inteiramente de outro dono. | Delegados `_E1/_E2`: registro efetivo `app/core/juizes.py:1310-1311`. Clamp e unicidade não têm registro próprio. |
| Batidas da fatia | `apurar_por_fatia:253-261`: dia local dentro de início−1/fim+1; havendo donos, exclui batidas interiores de dias não pertencentes ao vínculo, preservando padding externo. | Dia local delegado a `ponto/turnos.py::data_local` (`:203-204`); filtro/composição SEM JUIZ específico. |
| Composição | `apurar_por_fatia:294-300`: concatena períodos das fatias e soma campos declarados. | SEM JUIZ específico; testes de composição/paridade não contam como caso numérico pela regra. | [nao verificado]
| Justificativa/ausência | `recalcular_fechamento_mes:660-664`: justificativa aprovada dentro da janela alimenta datas justificadas. `:693-701`: parcial paga continua pelas batidas; efeito ABONA justifica; efeito diferente de DESCONTA suprime previsão. | Cobertura/efeito registrados em `app/core/juizes.py:190,252-253`; seleção de Justificativa aprovada e conjunto de integração sem registro próprio. |
| Previsão/CCT por fatia | `recalcular_fechamento_mes::_montar_motor:731-740`: `esc.eh_dia_trabalho(dia)` e dia não suprimido; chama CCT com `fatia_ini`, tipo e conjuntos. | Dia trabalho registrado `app/core/juizes.py:184`; `get_motor_cct`/montagem não aparece como registro em JUIZES. |

Caso pela regra para troca de vigência: `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F5_troca_de_vinculo_no_meio_da_competencia:1867`. Expõe CLT71/58 e L021/L099 (`:1868-1892`), compara 16h trabalho, 1h atraso, 2h antecipada e folga zero (`:1944-1952`). Não confere HE50; não cobre sozinho round, clamp, padding ou composição.

Leitores/recalculadores:

- `fechamento_lido` chama **novamente o cálculo** com `somente_leitura=True,retornar_objeto=True` (`app/ponto/services/fechamento.py:1759-1761`). Tela detalhe usa-o (`app/ponto/views.py:993`), outra tela também (`:1480`); painel GET ainda consulta FM gravado (`:1134-1141`).
- `totais_da_folha` exige FM existente e usa fechamento recalculado (`app/relatorios/cartao_pela_celula.py:71-76`); `_totais_do_fechamento` lê eventos e HE50 (`:90-99`).
- `folha_manda` escolhe fechamento online ou soma DiaPago (`app/relatorios/cartao_pela_celula.py:321-343`). PDF chama-o (`app/relatorios/pdf_espelho.py:628`); espelho também (`app/ponto/services/espelho.py:1284`).
- Linhas de espelho leem DiaPago (`app/ponto/services/espelho.py:805-813`); PDF HE50 usa `extras_50` (`app/relatorios/pdf_espelho.py:844-845`). `rubricas_do_dia` lê HE50 diretamente e calcula minutos extras `(HE50+HE100)*60` (`app/ponto/services/dia_pago.py:914-928`); calendário chama esse leitor (`app/colaboradores/services/calendario.py:656`).
- `autoridade_do_periodo` remonta motor CCT por fatia e injeta previsão/células/ausências (`app/ponto/services/espelho.py:1100-1133`), portanto é leitura que reapura.
- Porta compara soma DiaPago e fechamento (`app/folha/porta_export.py:388`); `conferir` exige métrica de hoje, competência correspondente, medição válida e zero falhas (`:807-821`). HE pendente que trava entra no motivo (`:853-864`).
- Extrato soma DiaPago e arredonda HE50 (`app/relatorios/views.py:529`); ranking usa `fechamento_lido` (`app/folha/services/ranking_he.py:206,226`).

Lavratura relacionada: `app/ponto/services/dia_pago.py::lavrar:180` converte `p.minutos_extra_50/60`; quando recebe rubricas, sobrescreve só campo declarado e fora de `do_motor` (`:312-330`); persiste `DiaPago` (`:345-360`). Além da porta, os restores reconstroem DiaPago por `bulk_create`: `bin/restore_relavratura_10_2026.py:86`, `bin/restore_o263_comp10.py:56`, `bin/restore_badge_linha_09_10.py:63`.

## 14/11 10:07 · sessao 98ae62e9139f · - · - · origem=Ronald (tmux)

| trecho | fonte e predicado lido | efeito |
|---|---|---|
| Escritor mensal | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:923` | `round(resultado.horas_extra_100_feriado, 2)` em horas decimais. |
| Parcela promovida de folga | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:934` | Seleciona `periodos_ft` somente quando `escala_certa_no_dia(...)` responde verdadeiro. |
| Separação feriado/folga | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:947` | Data local da entrada; `escala_vigente`; inclui quando vínculo existe e `feriado_vira_folga(data)` responde verdadeiro. |
| Soma promovida | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:954` | Soma `minutos_trabalhados` desses períodos ÷60; substitui campo por `round(resultado.horas_extra_100_feriado + _h_fer, 2)` em `:955`. |
| Persistência mensal | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1128` | Leitura retorna objeto/dicionário; aberto usa `save()` em `:1144`; demais estados usam `update`, incluindo campo em `:1168`. |
| Restauração | `app/ponto/services/fechamento.py::restaurar_fechamento:1373` | Aceita campos concretos conhecidos, exige motivo; exclui identidade e atualiza campos recebidos em `:1393`; portanto pode escrever esse campo. |
| Aprovação | `app/ponto/services/fechamento.py::aprovar:1335` | Altera carimbos e salva objeto inteiro em `:1340`, reescrevendo valor existente. |
| Modelo | `app/ponto/models.py::FechamentoMensal.save:496` | Impede `save()` de registro já aprovado que continua aprovado; `update()` anterior não passa por esse método. |
| DiaPago motor | `app/ponto/services/dia_pago.py::lavrar:173` | Soma `p.minutos_extra_100_feriado/60` em `:183`; soma minutos trabalhados de `ft_feriado/60` em `:250`. |
| DiaPago calculador | `app/ponto/services/dia_pago.py::lavrar:312` | Quando recebe rubricas, campo pertencente a `CAMPOS_DIA` e ausente em `do_motor` substitui valor por `float(_valor or 0)` em `:329-330`. |
| Persistência DiaPago | `app/ponto/services/dia_pago.py::lavrar:345` | Constrói registros, apaga a versão existente e grava por `bulk_create` em `:359-360`. Chamadas motor `fechamento.py:1201`; oráculo `:1277`, somente competência ≥10/2026 por `:1265`. |
| Outro criador DiaPago | `app/ponto/services/dia_pago.py::retratar_exportada:472` | Cria dia sem linha com rubricas monetárias zero em `:478-479`; ajuste idem `:510-515`; `bulk_create` em `:545`. Linhas existentes atualizam apenas grade/carimbos em `:538-540`. |
| Restauração via comandos | `app/ponto/management/commands/aplicar_09_corte_b.py::Command.handle:266`; `app/ponto/management/commands/desvio_o68b.py::Command.handle:197` | Restauram conjunto de campos fotografado, incluindo campo feriado; passam pela porta `restaurar_fechamento`. `aplicar_09_he_intra.py:39-40` não inclui feriado em `ALVO`; sua presença em `CAMPOS:44` é fotografia. |

Decisões anteriores à soma:

- `app/ponto/services/fechamento.py::apurar_por_fatia:189`: célula com geradora pergunta `vinculo_do_dia`; `:206-207` corta por janela, início de operação e vigência; `:224-238` inclui dia sem dono somente quando um vínculo o cobre e é o vínculo corrente; pula fatia sem dias próprios; `:242-243` pula intervalo invertido.
- `app/ponto/services/fechamento.py::apurar_por_fatia:253`: batidas por data local dentro de pad ±1 dia; `:255-261` remove dias internos pertencentes a outro vínculo; `:286-300` roda motor e concatena resultados.
- `app/ponto/services/fechamento.py::recalcular_fechamento_mes:660`: justificativa somente aprovada, com `data_turno` na janela e não nula; cobertura ausência por `:683`; `:694-695` parcial paga deixa contar batidas; `:698-701` ABONA inclui dia justificado, efeito diferente de DESCONTA suprime previsto.
- `app/ponto/services/fechamento.py::recalcular_fechamento_mes._montar_motor:731`: previsto somente `esc.eh_dia_trabalho(data)` e data fora de `_datas_suprimidas`; `get_motor_cct` usa início da fatia em `:736`.
- `app/escala/services/escala_certa.py::motivo_escala_errada:31`: rejeita vínculo LIMBO; `:38-45` usa competência do dia e rejeita assinaturas A2/A8; `::escala_certa_no_dia:51` aceita somente motivo `None`.
- `app/ponto/services/esmeril_espelho.py::_escala_do_colab:71`: candidatos exigem célula `trabalha is False`, turno no dia e pelo menos duas batidas; A8 exige três dias em `:72`; A2 exige dois pares separados por um ou dois dias em `:74`.
- `app/ponto/services/esmeril_espelho.py::assinaturas_do_colab:95`: lê células na janela e `turnos_do_colab`, conta dias e batidas; `so_escala=True` retorna em `:105`, antes da consulta de chamados.
- `app/escala/models.py::EscalaColaborador.feriado_vira_folga:1390`: rejeita `trabalha_em_feriado` ou escala ausente; `:1392` rejeita ciclo fora de `CICLOS_FERIADO_VIRA_FOLGA`; `:1395` pergunta `eh_feriado(data, lugar_do_vinculo(self))`.

Leitores e reinterpretações:

- `app/ponto/services/fechamento.py::fechamento_lido:1759`: **recalcula**, chamando o escritor com `somente_leitura=True, retornar_objeto=True`.
- `app/relatorios/cartao_pela_celula.py::totais_da_folha:71`: exige registro mensal existente; chama `fechamento_lido` em `:73`.
- `app/relatorios/cartao_pela_celula.py::_totais_do_fechamento:90`: reaplica `eventos_do_fechamento` com ciclo/cláusula; `:97-99` soma evento feriado a `total_extras`; `:95` lê HE100 bruta.
- `app/relatorios/cartao_pela_celula.py::divergencia:250`: lê mensal **gravado**, usa mesmo mapeamento e compara HE50/HE100 em `:265-269`; não compara evento feriado isoladamente.
- `app/ponto/services/espelho.py::espelho_do_colab:1283` e `app/relatorios/pdf_espelho.py::_coletar_dados_espelho:628`: chamam `folha_manda`.
- `app/ponto/services/espelho.py::montar_dias:811`: valores diários por `pago_do_dia` e `rubricas_do_dia`.
- `app/ponto/services/dia_pago.py::pago_do_dia:890`: linha ausente ou sem rubrica monetária devolve `None`; `:897-898` soma trabalhadas, folga e `detalhe.folga_feriado_min/60`.
- `app/ponto/services/dia_pago.py::rubricas_do_dia:914`: lê HE100 total; não soma o subset feriado; minutos extras = `(HE50 + HE100)*60` em `:928`.
- `app/relatorios/pdf_espelho.py::_build_story:857`: presença da coluna depende de rubrica lavrada **ou** campo do motor; valor diário vem de rubricas lavradas.
- `app/folha/porta_export.py::dias_fora_do_dia_pago:78`: recalcula autoridade do período; `:115-116` compara trabalho contra `pago_do_dia`, tolerância 0,02h.
- `app/folha/porta_export.py::medir:451`: compara soma DiaPago contra gravado; tolerância `_TOL_H=0.015` em `:239`; diferenças monetárias de exportada são separadas em `:459-460`.
- `app/folha/porta_export.py::dias_acima_do_limite_sem_decisao:230`: trava somente acima do limite e sem qualquer estado decisório em `:232`; `medir:690-691` só soma se cadastro `he_pendente_trava_export`.
- `app/folha/porta_export.py::conferir:807`: exige snapshot de hoje, competência correta `:815`, valor medido não negativo `:818`, zero falhas `:821`; não recalcula ali.
- `app/relatorios/pdf_espelho.py::gerar_pdf_espelho_bytes._bloqueio_disputa:1085`: recusa PDF se `motor_bloqueado_por_disputa` responder disputa; bloqueio entregue à geração em `:1113`.

Caso pela regra confirmado: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:13-20` escreve L-086 e caso 6x1, não trabalha em feriado, 08–12 → 4h HE100 feriado e 0h folga. `::FeriadoTrabalhadoSaiComoFeriadoTest::test_feriado_trabalhado_vai_para_HE100_feriado:117-125` confere mensal e DiaPago; `::test_MORDE_feriado_trabalhado_conta_UMA_vez:127-139` exige 4h feriado e 0h trabalhadas. Os nomes `escala_certa_no_dia`, `_escala_do_colab` e `feriado_vira_folga` não aparecem no registro de `app/core/juizes.py`; para suas perguntas específicas: SEM JUIZ.

## 14/11 10:07 · sessao 7e09ef4f766a · - · - · origem=Ronald (tmux)

Casos localizados por leitura; resultado de execução [não verificado].

| Decisão | Caso pela regra | Regra e resposta escrita |
|---|---|---|
| Export separa HE100 normal da parcela feriado | `app/folha/tests/test_export_59a.py::Export59ATest::test_01_12x36_suprime_parcela_feriado:19` | CLT 59-A no cabeçalho; `22−20=2` para `hora_extra_100_diurna` (`:1-5`, `:26`). |
| CCT autoriza emissão de feriado no 12x36 | `app/folha/tests/test_export_59a.py::ExportFeriado12x36ClausulaTest::test_MORDE_12x36_com_clausula_emite_a_parcela_feriado:48` | Cláusula de CCT descrita em `:43-46`; feriado 20, HE100 normal 2 (`:50-52`). |
| Tolerância diária HE | `app/ponto/tests/test_a6_12x36_tolerancia.py::A6ToleranciaDoze36Test::test_MORDE_variacao_de_10min_nao_e_hora_extra:37`; `::test_controle_11min_passa_do_teto_e_conta_integral:42` | Art.58 §1/Súmula366 (`:1-4`): 730 trabalhados → HE0; 11 minutos → HE11 (`:39-44`). |
| Entrada antecipada conta do marco | `app/ponto/tests/test_janela_he_regra.py::JanelaHeRegraTest::test_RED_06h40_com_marco_07h00_nao_gera_HE:83` | L-097 (`:2-16`): 660 trabalhados, HE0 (`:86-89`). |
| Piso da entrada | Mesmo arquivo/classe, `::test_MORDE_06h52_esta_dentro_do_piso_e_NADA_muda:91` | L-097/Art58 (`:2-16`): 668 trabalhados (`:94-95`). |
| Teto da entrada | Mesmo arquivo/classe, `::test_RED_05h30_passa_do_teto_e_entra_em_CADASTRO_x_REALIDADE:97` | L-097: 660 trabalhados, HE0 e cadastro acusado (`:100-105`). |
| Vigência da janela | Mesmo arquivo/classe, `::test_MORDE_a_vigencia_protege_a_competencia_09:107` | L-092 (`:108`): antes da vigência, 680 trabalhados (`:110-111`). |
| Empresa desliga janela | Mesmo arquivo/classe, `::test_MORDE_empresa_com_a_janela_DESLIGADA_nao_muda_nada:161` | L-097/cadastro (`:2-16`): 680 trabalhados (`:163-166`). |
| Saída recorta ao marco | `app/ponto/tests/test_janela_he_saida.py::BloqueioTotalTest::test_RED_so_a_saida_07h06_conta_ate_o_marco:91` **nome da classe [não verificado]** | L-097 corrigida (`:2-20`): 660 trabalhados, HE0 (`:94-95`). |
| Autorização remove recorte | `app/ponto/tests/test_janela_he_saida.py::DiaAutorizadoContaTest::test_RED_o_dia_AUTORIZADO_passa_a_contar_os_minutos_de_fora:255` | L-097 (`:199-204`): 740 trabalhados (`:258-260`). |
| Recusa mantém padrão | Mesmo arquivo/classe, `::test_MORDE_o_NAO_tem_o_efeito_do_PADRAO_e_nao_muda_numero:262` | L-097 (`:199-204`): 720 trabalhados (`:265-267`). |
| Autorização vale pelo dia inteiro | Mesmo arquivo/classe, `::test_MORDE_a_decisao_e_do_DIA_e_nao_da_PONTA:269` | Chave diária explicitada (`:270-271`): ambas pontas retornam, 735 trabalhados (`:274-275`). |
| Ausência não apaga trabalho | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956` | Lei605/49 art6 (`:1957-1961`): 480 trabalhados, falta0, abono0, extra0 (`:1966`). |
| Todas as pausas saem do tempo | Mesmo arquivo/classe, `::test_F7_intervalo_dividido_em_dois:1968` | Art71 (`:1969-1975`): 660−75=585; extra0 (`:1981-1982`). |
| Pausa curta no 12x36 | Mesmo arquivo/classe, `::test_F7c_PINADO_o_mesmo_dia_no_12x36_paga_a_pausa_como_trabalho:2016` | Art71 §2 (`:2017-2020`): esperado270; `expectedFailure` em `:2015`; cobrança numérica em `:2038`. |
| 12x36 feriado simples | Mesmo arquivo/classe, `::test_F4_feriado_em_12x36_noturno_e_hora_simples:1399` | Art59-A/CCT (`:1400-1410`): 660 trabalhados, HEfer0, extra0 (`:1416-1418`). |
| Feriado dobra jornada inteira | Mesmo arquivo/classe, `::test_F10_feriado_em_dia_de_trabalho_dobra_a_jornada_INTEIRA:1480` | Súmula146 (`:1481-1489`): HE100480, fer480 (`:1492-1494`). |
| Feriado não acumula excedente novamente | Mesmo arquivo/classe, `::test_F10b_o_dia_em_dobro_nao_volta_ao_calculo_normal_de_HE:1496` | Súmula146 (`:1497-1503`): HE100600, fer600 (`:1506-1508`). |
| Janela noturna/hora reduzida | Mesmo arquivo/classe, `::test_F2_borda_utc_turno_21_03:1330` | Art73 (`:1331-1339`): 300 relógio/342,86 legais (`:1346-1349`). |
| Prorrogação depende CCT/ciclo | Mesmo arquivo/classe, `::test_F4b_a_prorrogacao_pos_05h_e_cadastro_e_nao_default:1420` | Art73§5/Súmula60/Art59-A (`:1421-1432`): 480 relógio/548,57 legais (`:1441-1444`). |
| CCT afasta redução | Mesmo arquivo/classe, `::test_F4c_a_hora_reduzida_afastada_muda_o_fator_nao_o_relogio:1446` | CCT38-d (`:1447-1456`): 360 relógio/360 legais (`:1463-1465`). |
| Noturno exclui pausa | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O263_intervalo_batido_nao_recebe_adicional_noturno:2689` | Art73 (`:2690-2695`): 360 relógio/411,43 legais (`:2700`). |
| Dia sem previsto bloqueia | `app/ponto/tests/test_he100_dia_sem_previsto.py::DiaSemPrevistoForaDaJanelaTest::test_RED_dia_sem_previsto_nao_paga_sem_decisao:69` | L-097 escrita em `:4-9`: 330 minutos → zero pago/330 fora (`:73-75`). |
| Dia sem previsto autorizado paga | Mesmo arquivo/classe, `::test_autorizado_paga_inteiro:83` | L-097 (`:4-9`): 330 trabalhados/5,5FT (`:89-91`). |
| Exceção folga sem escala certa | `app/ponto/tests/test_he100_dia_sem_previsto.py::DiaSemPrevistoEscalaCertaTest::test_MORDE_folga_sem_escala_certa_nao_zera_nem_publica:142`; `::test_MORDE_folga_com_escala_certa_zera_e_publica:151` | L-007 (`:135-137`): 330 trabalhados sem escala certa; zero com escala certa (`:147-159`). |
| Degrau120 no plantão | `app/ponto/tests/test_plantao_he_distribuicao.py::TestPlantaoHEDistribuicao::test_plantao_24h_distribui_he50_he100:23` | Cabeçalho escreve Art59: extra960, HE50120, HE100840 (`:4-8`); asserts `:28-31`. |

Há declaração textual divergente sobre o lastro do degrau120: a bateria O280 evita certificar a divisão e declara ausência de lastro em `app/ponto/tests/test_o280_bateria_da_lei.py:2203-2207` e `:5994-5996`; o teste de plantão acima escreve a divisão sob Art59 em `app/ponto/tests/test_plantao_he_distribuicao.py:7-8`. Não escolhi qual vale.

BO0611 não conta pelo critério solicitado: declara “caracterização medida, não lei” em `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:11-13`; compara dicionário `MEDIDO` e idempotência em `:665-677`.

Caso L-115 disponível para borda posicional: `app/ponto/tests/test_bateria_lavratura.py::SaidaPosicionalDepoisDaPausaTest::test_A_col263_a_ultima_batida_depois_da_pausa_e_a_SAIDA:1438`; regra e tabela numérica em `:1293-1347`; comparação da autoridade e ata contra resposta escrita em `:1411-1433`.

Regra por fatia também tem resposta numérica externa ao código: `app/ponto/tests/test_regua_por_fatia_na_janela.py::DuasReguasNaJanelaTest::test_RED_a_testemunha_paga_a_regua_de_hoje_na_janela_inteira:186` e `::test_RED_o_cartao_em_pdf_mostra_o_mesmo_numero_da_regra:204`; Art73/Art59-A produzem 10,29+8=18,29 horas noturnas em `:22-42`, asserts `:198-201` e `:214-216`.

## 14/11 10:07 · sessao 0752a4a800ac · - · - · origem=Ronald (tmux)

| Decisão / escrita | Evidência |
|---|---|
| Selecionar FT com escala certa pelo dia civil local da entrada | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:933-935`: `_certa(colab.id, timezone.localtime(p.entrada).date(), cache)` |
| Retirar FT que é feriado de vínculo que folga em feriado | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:946-953`: vínculo existente e `feriado_vira_folga(dia)`; remove por identidade |
| Gravar total mensal em horas decimais, arredondado em duas casas | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:965-966`: `round(sum(p.minutos_trabalhados for p in _ft_certa)/60, 2)` |
| Persistência mensal | Aberto: `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1137-1144`; demais status: `:1160-1170`; `app/ponto/services/fechamento.py::aprovar:1340` salva a instância inteira, conservando o número |
| Restaurador também escreve o campo | `app/ponto/services/fechamento.py::restaurar_fechamento:1374-1393` aceita campos válidos arbitrários e executa `update(**_upd)`; comandos que podem entregar FT: `app/ponto/management/commands/aplicar_09_corte_b.py::Command.handle:265-268`, `app/ponto/management/commands/desvio_o68b.py::Command.handle:195-199` |
| Persistência diária | `app/ponto/services/dia_pago.py::lavrar:234-239` soma FT certa/60; `:324-330` permite substituir com resultado do calculador; cria em `:345-347`, persiste `:360`; ajuste nasce zero `:350-358` |
| Outro escritor de zero diário | `app/ponto/services/dia_pago.py::retratar_exportada:472-479` cria dia ausente com campos monetários zero; cria ajuste zero `:510-515`; persiste `:545` |
| Excluir LIMBO | `app/escala/services/escala_certa.py::motivo_escala_errada:31-33`: vínculo do dia com `regua_classe == 'LIMBO'` |
| Excluir A2/A8 da competência do dia | `app/escala/services/escala_certa.py::motivo_escala_errada:38-45`: `janela_atual(dia, empresa)`, `assinaturas_do_colab(..., so_escala=True)`, presença em `('A2','A8')`; bool final `::escala_certa_no_dia:51` |
| Construir candidatos A8 | `app/ponto/services/esmeril_espelho.py::_escala_do_colab:71`: célula `trabalha is False`, existe turno atribuído ao dia e pelo menos duas batidas |
| Limiar A8 | `app/ponto/services/esmeril_espelho.py::_escala_do_colab:72-73`: pelo menos três dias candidatos |
| Limiar A2 | `app/ponto/services/esmeril_espelho.py::_escala_do_colab:74-75`: pelo menos duas diferenças adjacentes de um ou dois dias |
| Origem dos candidatos | `app/ponto/services/esmeril_espelho.py::assinaturas_do_colab:95-105`: células da janela, `turnos_do_colab`, contagem de turnos e batidas por `data_turno` |
| Predicado feriado | `app/escala/models.py::EscalaColaborador.feriado_vira_folga:1389-1395`: recusa se trabalha em feriado ou não existe template; exige ciclo em `CICLOS_FERIADO_VIRA_FOLGA`; consulta `eh_feriado(data,lugar_do_vinculo(self))` |
| Reinterpretação paralela | `app/ponto/calculador/alimentacao.py::rubricas_por_dia:514-524` alimenta `regras.do_dia`; `app/ponto/calculador/regras.py::folga_trabalhada_do_dia:390-399` repete exclusão intermitente, decisão de folga e escala certa; `::do_dia:664-690` repete partição feriado/FT |
| Destino do paralelo | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1265-1296` grava `DiaPago.versao='oraculo'`; `app/ponto/services/dia_pago.py::lavrar:329-330` substitui campos. O mensal continua recebendo sua soma própria em `fechamento.py:965-966` |

As decisões específicas de escala certa, A2/A8, partição mensal FT/feriado e o calculador paralelo estão **SEM JUIZ registrado**: não aparecem nas entradas de `JUIZES` em `app/core/juizes.py:182`, `:1192` ou `:1310`. O registro tem o vínculo do dia (`_E1`, `:1310`), o dia de trabalho (`celula/precedencia`, `:184`), o feriado (`:188`) e o valor do par (`batida`, `:1194`). A borda do feriado está explicitamente em `SEM_JUIZ[_F4]`, `app/core/juizes.py:351`.

Casos numéricos com regra escrita:

- Partição feriado/folga: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py::FeriadoTrabalhadoSaiComoFeriadoTest::test_feriado_trabalhado_vai_para_HE100_feriado:117` verifica **4h feriado / 0 FT**, `:123-125`; `::test_MORDE_domingo_de_folga_segue_folga_trabalhada:158` verifica **0 feriado / 4h FT**, `:164`; regra L-086 escrita em `:13-20`.
- Mesma partição pela folha: `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F11c_promocao_ft_feriado_leva_a_parte_noturna_para_a_folha:1714`; conta da regra `:1722-1727`, esperado FT **0**, `:1748-1755`.
- Paralelo: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py::CalculadorParticaoDoFeriadoTest::test_MORDE_folga_feriado_sai_em_HE100_feriado_e_nao_em_folga:230` verifica **0 FT / 8h feriado**; `::test_folga_comum_segue_folga:235` verifica **8h FT**.
- Previsão vence dia semanal: `app/ponto/tests/test_s5b_regra_pontualidade.py::RegraFolgaTrabalhadaTest::test_MORDE_sem_vinculo_cai_no_dia_da_semana_E_A_CELULA_VENCE:375`, L-086 e respostas **480min / 0**, `:376-382`.
- Escala recorrente excluída: `app/escala/tests/test_escala_certa_deterministica.py::EscalaCertaDeterministica::test_MORDE_pela_porta_do_dinheiro_a_lavra_vazia_nao_paga_100:73` verifica FT **0**, `:87`; referência L-099 `:1-7`. Não confere os limites **três dias**, **duas adjacências** ou o **LIMBO** separadamente contra uma regra numérica: **SEM CASO** para esses limites.
- Transporte diário e exclusão de outras rubricas: `app/ponto/tests/test_dia_pago_soma.py::LavraDaFolgaTrabalhadaTest::test_MORDE_ft_certa_leva_hora_NOTURNO_e_INTRA_e_nao_leva_antecipada_nem_HE:220`, regra escrita `:175-187`, esperado FT **7h**, `:227`, demais parcelas `:231-243`.
- Testes de passagem export/cartão e presença de escritor em `app/ponto/tests/test_classe3_folga_100.py::ExportFolgaTrabalhadaTest:43` são conferência de passagem/implementação; não fornecem caso independente da regra para soma/arredondamento mensal, restauração ou limiares A2/A8. [não verificado]

## 14/11 10:07 · sessao 6b3d645139b7 · - · - · origem=Ronald (tmux)

Casos lidos contra regra escrita; não executei testes. [nao verificado] [nao verificado]

| Predicado coberto | Caso citável | Resposta efetivamente assertada |
|---|---|---|
| Tolerância HE diária e totalidade após ultrapassar limite | `app/ponto/tests/test_o280_bateria_da_lei.py::V3Familia2Test::test_v3_F9b_a_tolerancia_do_Art58_tambem_e_da_HORA_EXTRA:3829` | Art.58 §1/Súmula366; 488 trabalhados contra480 → HE0;495 → HE15 inteiros (`:3842-3849`). |
| Limite HE inclui somente intervalo suprimido descontado do previsto; excesso acima2h segue devido | mesmo arquivo `::V3Familia2Test::test_v3_A6_e_A6b_a_hora_acima_do_limite_legal_segue_DEVIDA:3854` | Art.59 §1/Súmula376II; D12 previsto660, presença1020, pausa0 → limite720 e HE300; comercial480/presença840 → HE300 (`:3857-3875`). |
| Sem previsto não se declara excedente | mesmo arquivo `::V3Familia2Test::test_v3_o_dia_sem_previsto_nao_recebe_excesso_e_o_censo_CONTA:3877` | Caso existe; detalhes numéricos ainda [nao verificado]. |
| Janela HE L097 e exclusão de pausa fora do recorte | mesmo arquivo `::BugsTest::test_B_O272b_com_a_janela_LIGADA_o_recorte_nao_desconta_duas_vezes:2552` | Entrada05h, marcos07–19, pausa06–06:30; janela ligada → trabalhado720, pausa descontada0, intra indenizada60, HE0 (`:2595-2604`). |
| Decisão HE vigente: sem decisão/recusa/revogação versus autorização | mesmo arquivo `::BugsTest::test_B_O273_recusa_revogada_nao_move_o_dia_e_a_autorizacao_move:2606` | L097; entrada06:20 contra07h →720/HE0 nos três primeiros estados; autorização40→760/HE40 (`:2660-2687`). |
| Folga trabalhada excluída de HE comum, mesmo feriado | mesmo arquivo `::FronteiraTest::test_F3_feriado_em_dia_de_folga_e_folga_trabalhada:1386` | Súmula146/corte escrito; folga D12 feriado→FT660, HE0, HEferiado0 (`:1393-1397`). |
| 12x36 feriado simples | mesmo arquivo `::FronteiraTest::test_F4_feriado_em_12x36_noturno_e_hora_simples:1399` | Art.59A/CCT38d; trabalho660→HE0 e HEferiado0; noturno relógio360/legal411,43 (`:1411-1418`). |
| Feriado dobra jornada inteira e substitui excedente HE50 | mesmo arquivo `::FronteiraTest::test_F10_feriado_em_dia_de_trabalho_dobra_a_jornada_INTEIRA:1480`, `::test_F10b_o_dia_em_dobro_nao_volta_ao_calculo_normal_de_HE:1496`; caminho v3 `::V3Familia5Test::test_v3_F10b_a_dobra_SUBSTITUI_a_faixa_do_excedente:4475` | Súmula146;480→HE5040? **Correção: HE50=0, HE100=480** (`:1492-1494`);600→HE50=0, HE100=600 (`:1506-1508`,`:4487-4488`). |
| Dia da jornada decide feriado, não data da cauda | mesmo arquivo `::V3Familia5Test::test_v3_F10e_CURA_a_cauda_que_entra_no_feriado_nao_dobra:4491` | L085; jornada inicia véspera e cauda no feriado→HEferiado0 (`:4504-4512`). |
| Ausência/justificativa não apaga trabalho batido | mesmo arquivo `::FronteiraTest::test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956` | Lei605/49art6; comercial com justificativa e batidas completas→trabalho480, faltas0, abonos0, HE0 (`:1963-1966`). |
| Todas pausas saem do trabalho | mesmo arquivo `::FronteiraTest::test_F7_intervalo_dividido_em_dois:1968` | Art71;660 bruto−75 pausa→585 trabalho e HE0 (`:1977-1982`); ata: `app/ponto/tests/test_bateria_lavratura.py::TresParesNoMesmoDiaTest::test_MORDE_as_duas_pausas_batidas_saem_do_tempo:964`→630, porque720−60−30 (`:967-969`). |
| Janela22–05 e retirada pausa noturna | o280 `::V3ReapontarTest::test_i26_H1_a_janela_noturna_do_PAR_e_o_intervalo_gozado:5966` | Art73§2/Art4;21–07→420 relógio/480 legais sem pausa; pausa01–02→360/411,43 (`:5980-5989`). |
| Prorrogação pós05h pelo cadastro CCT | o280 `::FronteiraTest::test_F4b_a_prorrogacao_pos_05h_e_cadastro_e_nao_default:1420` | Art73§5/Súmula60II;19–07,pausa01–02,flagTrue→480 relógio/548,57 legais (`:1436-1444`). |
| Hora reduzida afastada pelo cadastro | o280 `::FronteiraTest::test_F4c_a_hora_reduzida_afastada_muda_o_fator_nao_o_relogio:1446` | CCT38d;mesmo turno→360 relógio/360 legais (`:1461-1467`). |
| Pausa após05h usa janela prorrogada do turno; HE noturna não inclui pausa | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py::P5ParteNaoPassaDoTodoTest::test_P5_HE50_noturna_pela_regra:68` | CLT73§5/Súmula60II/CCT38d;22–07,pausa05:30–06:30,480trabalho/360previsto→HE50=2h e HE50noturna=2h (`:14-23`,`:68-74`). `::test_P5_noturnas_pela_regra:81`→8h. |
| Isento não emite evento | `app/folha/tests/test_isento_cargo_export.py::IsentoCargoExportTest::test_isento_zero_eventos:58` | Art62;HE50=8,HE100=8,atraso9→eventos=[] (`:59-68`). |
| Isenção de marcação não fabrica minuto realizado | `app/ponto/tests/test_bateria_lavratura.py::IsentoNaoAcusaETampoucoFabricaTest::test_MORDE_o_credito_nao_fabrica_minuto_que_ninguem_bateu:811` | Art62;uma batida08:05,saída creditada17h→realizado0 e previsto540 (`:750-765`,`:815-819`). |
| CCT por empresa/regime/aplicação/vigência/prioridade/empate | `app/core/tests/test_regua_pela_empresa.py::ReguaPelaEmpresaTest::test_RED1_MORDE_o_PAR_mesma_empresa_cct_praca_COM_e_SEM_vinculo_MESMA_regua:119`; `::test_RED2_empresa_em_CLT_vence_a_linha_de_aplicacao_ATIVA:139`; `::test_RED3_colab_SEM_escala_ativa_recebe_a_CCT_da_empresa:153`; `::test_RED4_regime_VAZIO_cai_no_piso_legal_e_o_contador_ACUSA:168`; `::test_RED5_empresa_cct_SEM_linha_de_aplicacao_e_LACUNA_nao_decisao:199`; `::test_RED6_linha_de_PRACA_vence_a_linha_de_EMPRESA_da_mesma_empresa:224`; `::test_RED7_duas_linhas_ATIVAS_do_mesmo_nivel_dao_piso_legal_e_ACUSAM:247` | Regra escrita antes do código e L110 (`:2-15`); helpers assertam CCT38dTrue/prorrogaçãoFalse (`:104-106`); casos de cadastro e saída, não valor HE final. |

Cuidados para consolidar:

- **BO0611 férias/desligado não conta como caso pela regra**: arquivo explicitamente declara “CARACTERIZACAO MEDIDA, nao a lei” (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:11-13`); os casos `_Base::test_C0...:708`, `test_F1...:715`, `test_F2...:725`, `test_D1...:753`, `test_D2...:766`, `test_D3...:773` conferem a tabela `MEDIDO`, cujo significado está declarado em `:54-56`.
- **Split HE50/HE100 em120min tem lacuna expressa**: `o280::V3ReapontarTest::test_i26_H2_acima_do_limite_a_hora_segue_devida_sem_teto:5991` não afirma120/840 como regra; chama o degrau120 de convenção sem lastro escrito (`:5994-5996`), e confere somente limite480/total960/soma960 (`:6000-6004`). Há testes antigos numericamente120/840 com menção CLT: `app/ponto/tests/test_plantao_he_distribuicao.py::TestPlantaoHEDistribuicao::test_plantao_24h_distribui_he50_he100:23` e `test_he50_isolada.py::TestHE50Isolada::test_3h_extra_split_2h_he50_1h_he100:48`; não trate isso como lastro resolvido diante da declaração expressa da bateria. (`app/ponto/tests/test_o280_bateria_da_lei.py:5994-5996`)
- **Paridade não conta**: `test_s5b_split_noturno_tem_um_dono.py:62-64` conserva conta antiga como testemunha; `:305-320` obtém esperado chamando `he_noturna`, portanto compara caminhos do código.
- **TXT fonte versus foto**: `test_txt_le_a_celula_e_o_juiz.py::...::test_RED_o_valor_da_linha_vem_da_CELULA_e_nao_da_FOTO:214` injeta HE2 e exige `000000200` (`:215-217`); isso testa leitura/layout, não decide HE pela CLT/CCT. A classe exata [nao verificado].
- **Portão de paridade**: `test_porta_do_export.py::PortaDoExportTest::test_RED_carimbo_com_divergencia_RECUSA_com_a_LISTA:50` testa recusa/lista conforme texto normativo `:4`; não confere dinheiro contra CLT/CCT/LNNN e deve ficar fora de “caso numérico pela regra”.

Registro real:

- `_K1` e `_K2`→`folha/export.py::classificar_export` (`app/core/juizes.py:319-320`).
- `_F3`→`CICLOS_FERIADO_SIMPLES` (`:303`).
- Tolerância de pontualidade→`MotorBase.aplicar_tolerancia` (`:215`); não confundir com tolerância diária HE.
- `JUIZES['batida']` registra `periodos_do_dia` e `MotorBase.calcular_periodo` (`:1192-1194`).
- `regua_para`, `_aplicacao_vigente`, `get_motor_cct` **não aparecem no registro**; só há `core/regua_cct.py` como pendência feriado (`app/core/juizes.py:486`), portanto não usar o anexo JUIZES.md como prova de juiz CCT. [nao verificado]

## 14/11 10:07 · sessao b6c44cd5961f · - · - · origem=Ronald (tmux)

Registro efetivo em `app/core/juizes.py`:

| Pergunta | Registro |
|---|---|
| Totais da competência, inclusive evento exportado | `_TT5` → `folha/export.py::eventos_do_fechamento`, :732 |
| Entrada no TXT / bloqueio de aprovação | `_K2` / `_K1` → `classificar_export`, :319-320 |
| Dobra ou hora simples no feriado | `_F3` → `CICLOS_FERIADO_SIMPLES`, :303 |
| Supressão do trabalho por feriado | `_F2` → `fatos_do_dia`, :302 |
| Borda do feriado ao cruzar meia-noite | `_F4`, explicitamente `SEM_JUIZ`, :351 |
| Tolerância de entrada/saída | `turno/marcos` → `MotorBase.aplicar_tolerancia`, :215; alias `_T7`, :395 |
| Valor do par | `batida` → `MotorBase.calcular_periodo`, :1194 |
| Períodos e intervalo do dia | `batida` → `periodos_do_dia`, :1193 |
| Dia de trabalho / minutos previstos | `celula/precedencia`, :184-185 |
| Feriado para colaborador | `celula/precedencia` → `eh_feriado`, :188 |
| Cobertura de ausência | `celula/precedencia`, :190-191; `ausencia/ferias`, :253 |
| Efeito da ausência | `ausencia/ferias` → `efeito_vigente`, :252 |
| Vínculo do dia / cobertura da data | `_E1` / `_E2`, :1310-1311 |

A supressão específica do export também está em **PENDENTES** `_F3`, :485; o literal da CCT está em PENDENTES `_F3`, :486. Isso não elimina o registro geral `_TT5`, :732. Não localizei entradas de `JUIZES` para `regua_para`, `get_motor_cct`, `he_noturna`, `dobra_de_feriado_do_dia` ou o calculador: `[nao verificado]` como autoridade específica.

Casos numéricos pela regra confirmados por leitura:

| Decisão | Caso e resposta escrita |
|---|---|
| Export suprime feriado 12x36 | `app/folha/tests/test_export_59a.py::Export59ATest::test_01_12x36_suprime_parcela_feriado:19`; Art.59-A §2 escrito :1; agregado 20 h → evento 0, :21 |
| Export mantém feriado de outra escala | Mesmo arquivo, `Export59ATest::test_02_outras_escalas_emitem_feriado:29`; 5x2, 20 h → 20, :31 |
| Cláusula devolve emissão | Mesmo arquivo, `ExportFeriado12x36ClausulaTest::test_MORDE_12x36_com_clausula_emite_a_parcela_feriado:48`; 20 h → 20, :50; sem cláusula → 0 em `test_sem_clausula_segue_o_piso_legal:54`, :56 |
| Tolerância de HE | `app/ponto/tests/test_o280_bateria_da_lei.py::V3Familia2Test::test_v3_F9b_a_tolerancia_do_Art58_tambem_e_da_HORA_EXTRA:3829`; Art.58 §1 + Súmula366, :3830; 488/480 → HE0, :3844; 495/480 → HE15, :3848 |
| Dobra integral de feriado | Mesmo arquivo, `FronteiraTest::test_F10_feriado_em_dia_de_trabalho_dobra_a_jornada_INTEIRA:1480`; Súmula146; 480 trabalhados → 480 HE100fer, :1492 |
| Dobra substitui excedente | Mesmo arquivo, `FronteiraTest::test_F10b_o_dia_em_dobro_nao_volta_ao_calculo_normal_de_HE:1496`; Súmula146; 600 → 600 HE100fer, HE50 zero, :1506 |
| Feriado em folga do ciclo | Mesmo arquivo, `FronteiraTest::test_F3_feriado_em_dia_de_folga_e_folga_trabalhada:1386`; Súmula146 + corte19/09; FT660 e HEfer0, :1396 |
| 12x36 simples | Mesmo arquivo, `FronteiraTest::test_F4_feriado_em_12x36_noturno_e_hora_simples:1399`; Art.59-A §2; trabalhado660, HEfer0, :1416 |
| Promoção de FT no feriado | Mesmo arquivo, `FronteiraTest::test_F11c_promocao_ft_feriado_leva_a_parte_noturna_para_a_folha:1714`; corte BO-B(3) + Súmula264, :1715; FM/DP HEfer10 h, FT0, noturna1,14 h, HE100not1 h, :1748-1755 |
| Dia da jornada manda no feriado | Mesmo arquivo, `V3Familia5Test::test_v3_F10e_CURA_a_cauda_que_entra_no_feriado_nao_dobra:4491`; L085, :4492; jornada começa na véspera, cauda no feriado → HEfer0, :4509 |
| Ausência não apaga trabalho | Mesmo arquivo, `FronteiraTest::test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956`; Lei605/49 art.6, :1957; trabalho480, faltas0, abonos0, :1966 |
| Janela noturna | Mesmo arquivo, `V3Familia4Test::test_v3_F2_a_janela_do_art73_pelo_juiz_IMPORTADO:4309`; Art.73 §1/§2, :4310; 21–03 → 300 relógio /342,86 legais, :4318 |
| Intervalo sai do noturno | Mesmo arquivo, `V3Familia4Test::test_v3_B_O263_o_intervalo_gozado_sai_do_adicional:4321`; Art.73, :4322; 19–07, pausa01–02 →360/411,43, :4335 |
| Prorrogação cadastrada | Mesmo arquivo, `V3Familia4Test::test_v3_F4b_a_prorrogacao_vem_do_CADASTRO_e_NAO_de_default:4338`; Art.73 §5 + Súmula60II, :4339; piso360/411,43 contra CCT480/548,57, :4352-4355 |
| Afastamento da hora reduzida | Mesmo arquivo, `V3Familia4Test::test_v3_F4c_a_cl38d_muda_o_FATOR_e_nao_o_relogio:4358`; CCT cl.38-d, :4359; relógio360, legais360 ou411,43, :4375-4378 |
| Split noturno do feriado | Mesmo arquivo, `FronteiraTest::test_F11_feriado_no_turno_partido_leva_o_noturno_para_HE100_noturna:1654`; Súmulas146/264 + Art.73§2, :1655; HEfer180/noturna120, :1670-1672. Comercial: `test_F11b_feriado_no_comercial_que_entra_na_janela_noturna:1697`, HEfer840/noturna60, :1708-1710 |
| Duas parcelas noturnas no feriado | Mesmo arquivo, `FronteiraTest::test_F11d_partido_de_dois_pares_todo_noturno_do_feriado_e_HE100_noturna:1769`; Súmulas146/264, :1770; HEfer420, HE100not360, :1779 |
| Especificidade da aplicação CCT | `app/core/tests/test_regua_pela_empresa.py::ReguaPelaEmpresaTest::test_RED6_linha_de_PRACA_vence_a_linha_de_EMPRESA_da_mesma_empresa:224`; cláusula literal :225; praça30% /restante20%, :240-243 |

Ressalvas para a contagem:

- O gêmeo v2 da borda do feriado, `FronteiraTest::test_F10e_PINADO_a_cauda_que_entra_no_feriado_nao_dobra:1565`, tem `@expectedFailure` em :1564; confere zero contra L085 em :1576, mas não é confirmação verde. [nao verificado]
- `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:11-13` declara “caracterização medida, não a lei”; os casos :708-779 consomem `MEDIDO`. Não contam.
- Switch da CCT possui teste booleano em `app/ponto/tests/test_prorrogacao_pos5h_por_cadastro.py::ProrrogacaoPos5hPorCadastroTest::test_o_switch_da_regua_CCT_nao_desliga_a_CLT:142`; não confere número da rubrica.
- `app/ponto/tests/test_feriado_12x36_art59a.py::ExcecaoPorCCTTest::test_MORDE_a_clausula_cadastrada_devolve_a_dobra:230` verifica flag, não número.
- Não confirmei caso numérico da rubrica que atravesse, na mesma execução, `AplicacaoConvencao` + switch e confira horas literais: `[nao verificado]`.

## 14/11 10:07 · sessao d40d5604fe39 · - · - · origem=Ronald (tmux)

Mapa entregue ao agente principal com referências aos predicados, quatro escritores do campo de período, dois do mensal e lavratura do `DiaPago`.

Pontos a preservar na consolidação:

- O predicado efetivo da dobra é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não consulta dia previsto, apesar de comentários em outros trechos afirmarem isso (`app/ponto/motor_calculo_v2.py:934`, `:2807`, `:3088`).
- O calculador grava versão `oraculo` do `DiaPago`; o bloco lido não substitui o valor do `FechamentoMensal` (`app/ponto/services/fechamento.py:923`, `:955`, `:1265`, `:1277`, `:1294`).
- A tolerância diária de HE fica no ramo de excedente normal; o ramo da dobra paga o trabalhado positivo antes dessa tolerância (`app/ponto/motor_calculo_v2.py:2809`, `:2845`, `:3350`, `:3414`).
- A promoção de folga em feriado exige escala certa e `feriado_vira_folga`; acrescenta as horas desse conjunto ao mensal (`app/ponto/services/fechamento.py:934`, `:949`, `:955`).
- `MotorIntermitente` troca o conjunto de feriados; a comparação por identidade de `eh_feriado` permite então o fallback ao calendário (`app/ponto/motor_calculo_v2.py:3581`, `:965`, `:967`).
- `calculador/periodo.py::reler` também escreve o campo, mas chamamento produtivo permanece `[nao verificado]` (`app/ponto/calculador/periodo.py:770`, `:844`).

## 14/11 10:07 · sessao c1dd33744d92 · - · - · origem=Ronald (tmux)

1. **Fonte real do TXT:** `app/folha/views.py::gerar_dominio_txt:207-212` recalcula com `somente_leitura=True, retornar_objeto=True`; `:268` entrega os mesmos objetos a `montar_linhas_export`. Portanto o TXT dessa porta não depende dos números persistidos do fechamento.

2. **Campo e unidade:** `app/folha/export.py::eventos_do_fechamento:420-442` calcula `float(horas_extras_100)-float(horas_extras_100_feriado)` em horas decimais; `::montar_linha_10:39-45` emite dias inteiros se `rub.tipo=='dias'`, senão `round(horas*100)` em nove posições. O cadastro efetivo de produção não foi fornecido: [nao verificado].

3. **Escritor mensal principal:** `app/ponto/services/fechamento.py::recalcular_fechamento_mes:922-923` atribui `round(resultado.horas_extra_100,2)` e `round(resultado.horas_extra_100_feriado,2)`; persiste por `save()` quando aberto (`:1137-1144`), por `update()` nos demais (`:1160-1169`); leitura pura só devolve objeto/dict (`:1128-1136`).

4. **Outro escritor mensal:** `app/ponto/services/fechamento.py::restaurar_fechamento:1373-1393` aceita os campos concretos recebidos, exclui identidade e faz `update(**_upd)`. Chamadores: `app/ponto/management/commands/aplicar_09_he_intra.py::Command.handle:149`; `app/ponto/management/commands/aplicar_09_corte_b.py::Command.handle:220,266`. Campos têm default zero em `app/ponto/models.py:417-418`; `FechamentoMensal.save:496-503` recusa aprovado→aprovado, mas o escritor principal usa `update()` nesse ramo.

5. **Promoção feriado na folga:** `app/ponto/services/fechamento.py::recalcular_fechamento_mes:934-956`: retém `periodos_ft` somente se `escala_certa_no_dia`; depois exige vínculo vigente e `ec.feriado_vira_folga(dia)`; soma as mesmas horas aos dois campos. Essa parcela cancela na subtração de `export.py:421`. `escala_certa_no_dia` e essa promoção não aparecem como autoridade em `JUIZES`; não confundir com a pergunta do vínculo, registrada `_E1/_E2` em `app/core/juizes.py:1310-1311`.

6. **Escritor diário motor:** `app/ponto/services/dia_pago.py::lavrar:173-184` soma minutos de cada período/60 nos dois campos; `:244-250` soma minutos trabalhados/60 aos dois para `ft_feriado`; usa `_dia_de(p,dia_da_jornada)` e descarta se sem dia (`:174-176`, `:245-247`); constrói linhas `:345-347`, substitui universo da versão por delete/bulk_create `:359-360`.

7. **Escritor diário alternativo:** `app/ponto/services/dia_pago.py::lavrar:312-330` exige dono declarado e sobrescreve cada campo de `CAMPOS_DIA` recebido em `rubricas.valores` se não estiver em `do_motor`. `app/ponto/services/fechamento.py:1265-1296` chama esse caminho para `(ano,mes)>=(2026,10)`, `versao='oraculo'`; o motor usa outra chamada, `:1201-1237`, `versao='motor'`.

8. **Decisores alternativos:** `app/ponto/calculador/regras.py::do_dia:684-687` grava ambos como `_certa/60` se `folga_e_feriado and _certa`; `:779-789` grava ambos como dobra/60 se `_dobra>0`; `:799-815` grava HE100 como `max(extra-120,0)/60` se `extra>0` e rubrica não estiver em `_nd`. São reinterpretações paralelas; esse caminho não alimenta diretamente o TXT online citado no item 1.

9. **Segundo escritor diário de zeros:** `app/ponto/services/dia_pago.py::retratar_exportada:471-479` cria dias ausentes da grade com rubricas monetárias zero; `:509-515` cria ajuste com esses zeros; persiste `:545`. Atualização de linhas existentes em `:538-543` não escreve HE100 nem feriado. A guarda desse caminho está em `:451-460`.

10. **Leitura online comum:** `app/ponto/services/fechamento.py::fechamento_lido:1759-1761` chama o próprio recalculador em modo leitura; `app/relatorios/cartao_pela_celula.py::totais_da_folha:69-76` exige existência de fechamento, depois lê online. `::_totais_do_fechamento:90-99` lê eventos e total HE100; `::folha_manda:321-346` escolhe esses totais, depois lavratura se indisponíveis.

11. **Tela/PDF/espelho:** `app/ponto/views.py::painel_fechamento:993` chama `fechamento_lido`; `app/ponto/services/espelho.py::espelho_do_colab:1283-1284` chama `folha_manda`; `app/relatorios/pdf_espelho.py::_coletar_dados_espelho:628` também. Esses leitores provocam novo cálculo pela porta comum; não apenas leem os números gravados.

12. **Outros leitores ainda gravados:** `app/folha/views.py:331,589,858` chama `classificar_export` sem objetos injetados; `app/ponto/services/fechamento_lista.py::montar_resumos:73` faz o mesmo; `app/folha/export.py::classificar_export:783-788` consulta fechamento persistido nesse caso.

13. **Registro do emissor:** `app/core/juizes.py:719,732` registra `_TT5 → folha/export.py::eventos_do_fechamento`; `:319-320` registra `_K1/_K2 → classificar_export`; a conversão centesimal, o tipo da rubrica e a seleção empresa/global não têm registro próprio nessa tabela.

14. **Rubrica e exclusão de linha:** `app/folha/export.py::_rubricas_map:988-990` usa somente rubricas ativas, empresa sobrescreve global; `::montar_linhas_export:716-738` exige classificação `entra`, status em `STATUS_NO_TXT`, valor truthy e rubrica existente. O bloco de faltas (`:740-744`) é irmão da emissão HE100, não fonte do número HE100.

15. **Gates do classificador:** `app/folha/export.py::classificar_export:866-884` exclui rescisão, código ausente e férias com batida; `:902-967` exclui cadastro zero se horas zero, ≥2 batidas apuráveis e nenhum dia com previsto; `:968-979` exclui retenção do espelho e código não homologado. `_K2` está registrado em `app/core/juizes.py:320`; os subpredicados próprios não ganham automaticamente registro por estarem dentro dele.

16. **Retenção por célula:** `app/folha/export.py::motivos_retencao_celula:1205-1213` isenta ponto e desligado; `::_dias_sem_decisao:1239-1296` corta admissão/demissão, ignora dia sem célula e sem vínculo, retém célula ausente passada, veredito acusador salvo protestos todos não cobráveis ou só pausa, e veredito nulo com `trabalha is not None` no passado. `esta_desligado` registrado em `app/core/juizes.py:267`; o laço de retenção não possui entrada própria.

17. **Porta do export:** `app/folha/views.py::gerar_dominio_txt:128-133` recusa retorno não vazio de `porta_export.conferir`; `:163-166` exige fechamento persistido elegível existente; `:270-279` recusa ausência de linhas e erro de layout.

18. **Carimbo e HE pendente:** `app/folha/porta_export.py::conferir:807-821` exige medição de hoje, mesma competência, valor não negativo e zero; `::dias_acima_do_limite_sem_decisao:225-234` exige acima do limite e ausência de qualquer estado decisório; `::medir:687-691` só incorpora essa falha quando `empresa.he_pendente_trava_export`. Nenhuma dessas funções está registrada em `JUIZES` de `app/core/juizes.py`.

19. **Portão do cartão:** `app/relatorios/pdf_espelho.py::gerar_pdf_espelho_bytes:1082-1088` recusa disputa retornada por `motor_bloqueado_por_disputa`; `:1113` entrega esse bloqueio à emissão. `app/ponto/services/disputa.py::motor_bloqueado_por_disputa:29-31` delega a `colaborador_em_disputa`. No fechamento, a guarda real de `save()` só é aprovado→aprovado (`app/ponto/models.py:496-503`), não disputa.

20. **PDF do TXT:** `app/folha/pdf_export.py::traduzir_texto:112-125` lê valor bruto `[24:33]` e soma por código; `::_fmt:36-40` divide por 100 se o cadastro atual disser `horas`, mantém inteiro se `dias`, expõe bruto se desconhecido. Não recalcula HE; reinterpreta unidade pelo cadastro consultado (`:95-103`).

## 14/11 10:07 · sessao 63339a8259ae · RMDIR-COM-GUARDA · verificador · origem=Code

**Fechamento parcial:** a ambiguidade do 127 e o mutante sempre TERM receberam cura; permanecem lacunas em sinais consecutivos, falhas do arquivo de rc e propagação da falha de DADOS (`bin/suite.sh:291`, `bin/suite.sh:295`, `bin/suite.sh:302`, `bin/codex_pergunta.sh:306`). Resultados de execução e ocorrência em produção: **[nao verificado]**.

1. **Procedentes 2–5 da volta 2**

   | Item | Veredito |
   |---|---|
   | **2 — rc 127 ambíguo** | **PROCEDE — fechado nesse ponto:** o subshell escreve o retorno de `_run`; o pai lê o arquivo, sem interpretar 127 como “já recolhido”. O selo exige HUP → 127 (`bin/suite.sh:291`, `bin/suite.sh:302`, `bin/tests/test_rm_com_guarda.sh:236`). |
   | **3 — sinal antes do filho nascer** | **PROCEDE — cura pontual presente:** as traps antecedem o lançamento; sinal registrado fica pendente e `pkill` sem sucesso provoca nova tentativa. O selo não força essa janela; há ainda perda de sinais consecutivos, descrita abaixo (`bin/suite.sh:285`, `bin/suite.sh:291`, `bin/suite.sh:294`, `bin/suite.sh:296`, `bin/tests/test_rm_com_guarda.sh:227`). |
   | **4 — mutante sempre TERM** | **PROCEDE — par que morde presente:** a trava falsa distingue TERM → 9 de HUP → 127, e o mutante substitui o repasse por TERM. Resultado executado: **[nao verificado]** (`bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:236`, `bin/tests/test_rm_com_guarda.sh:269`). |
   | **5 — mktemp DADOS/CAB** | **PROCEDE para impedir uso do caminho após falha; DUVIDA para abortar integralmente o wrapper:** CAB falha no processo principal; DADOS também pode falhar dentro da substituição de comando cujo status externo não é conferido (`bin/codex_pergunta.sh:285`, `bin/codex_pergunta.sh:306`, `bin/codex_pergunta.sh:336`). |

   **PROCEDE — ressalva concreta de DADOS:** `_pergunta_raspada` chama `_nomes` dentro de `$(_pergunta_raspada)`. Seu `exit 3` encerra essa substituição; o `printf` externo pode concluir e marcar `INI=1`. Sem sonda/anexo, o fluxo pode continuar até chamar Codex, caso as etapas seguintes funcionem (`bin/codex_pergunta.sh:296`, `bin/codex_pergunta.sh:306`, `bin/codex_pergunta.sh:321`, `bin/codex_pergunta.sh:422`).

2. **_RCF vazio, origem do rc e fallback**

   **PROCEDE — pode ficar vazio com `_run` terminado normalmente:** falha de redireção ou de `echo` não é tratada; o script não habilita `errexit`. Separadamente, falha de `cat` produz `_RC_RUN` vazio mesmo que o arquivo tenha conteúdo (`bin/suite.sh:34`, `bin/suite.sh:291`, `bin/suite.sh:302`). Ocorrência de falha de E/S: **[nao verificado]**.

   **PROCEDE — o fallback pode mascarar rc real:** o status do `wait` é descartado; conteúdo vazio vira 143/130/129/1 conforme o último sinal registrado. Assim, um retorno real como 0, 75 ou 127 pode ser substituído, e a mensagem “run morreu sem devolver rc” também pode representar falha de escrita/leitura (`bin/suite.sh:301`, `bin/suite.sh:302`, `bin/suite.sh:304`, `bin/suite.sh:305`).

   **PROCEDE — no caminho normal, a origem está correta:** o único escritor visível captura `$?` imediatamente após `_run`; não captura o rc de `sleep`, `pkill` ou `wait`. `_run` chama a trava, que devolve 75 por falta de vez ou o status do comando executado (`bin/suite.sh:273`, `bin/suite.sh:291`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`). Escrita por processo externo: **[nao verificado]**.

3. **Sleep, dois sinais e duração do laço**

   **PROCEDE — há caminho para entregar um sinal recebido durante o sleep:** a trap registra estado e a próxima passagem tenta o repasse. Na semântica usual do Bash, a trap fica adiada até terminar o comando externo; portanto existe atraso pelo sleep, sem precisar esperar o run inteiro (`bin/suite.sh:285`, `bin/suite.sh:294`, `bin/suite.sh:299`). Entrega efetivamente medida: **[nao verificado]**.

   **PROCEDE — dois sinais podem virar uma entrega:** se TERM e HUP forem registrados antes do próximo repasse, sobra apenas `_SINAL=HUP`; um `pkill -HUP` bem-sucedido atualiza `_N_ENTREGUE` para o contador inteiro, quitando ambos (`bin/suite.sh:285`, `bin/suite.sh:287`, `bin/suite.sh:295`).

   **PROCEDE — existe outra janela de perda:** após expandir `pkill "-$_SINAL"`, uma trap pode registrar outro sinal enquanto o comando externo termina; a atribuição seguinte usa o contador atualizado e pode marcar esse segundo sinal como entregue sem repassá-lo. É inferência da sequência de operações; ocorrência: **[nao verificado]** (`bin/suite.sh:295`).

   **DUVIDA — zumbi:** `kill -0` sozinho não distingue processo vivo de zumbi, mas o Bash normalmente recolhe filhos assíncronos durante seu funcionamento. Não há evidência de zumbi permanente nesta execução: **[nao verificado]** (`bin/suite.sh:293`, `bin/suite.sh:299`, `bin/suite.sh:301`).

   **PROCEDE — reciclagem de PID não é protegida:** o laço consulta apenas o número, sem conferir identidade nem usar o rc já escrito como condição de término. Se o PID for reutilizado antes da próxima consulta, pode acompanhar outro processo indefinidamente enquanto ele existir; um repasse pendente também pode atingir filhos desse outro processo (`bin/suite.sh:293`, `bin/suite.sh:295`, `bin/suite.sh:302`). Reciclagem efetiva: **[nao verificado]**.

   **DUVIDA adicional — INT:** o wrapper nasce em background, a trava real não instala handler INT, e o selo exercita TERM/HUP. A herança usual de INT ignorado em comandos assíncronos merece verificação; interrupção efetiva por INT: **[nao verificado]** (`bin/suite.sh:291`, `bin/trava_teste.sh:20`, `bin/trava_teste.sh:42`, `bin/tests/test_rm_com_guarda.sh:224`, `bin/tests/test_rm_com_guarda.sh:233`).

4. **Consumidores de rc 75 e 2**

   **PROCEDE — `volta_intermediaria.sh` depende de 75:** captura o rc da suite, distingue 75 de vermelho e preserva 75 quando os demais verificadores passaram. Rc 2 da suite entra como vermelho e resulta em rc 1 da volta (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:33`, `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`, `bin/volta_intermediaria.sh:44`).

   **PROCEDE — o caminho normal do delta preserva 75:** grava, lê e devolve esse valor; a ressalva é a falha de transporte descrita no achado 2. Há selo específico exigindo `75 sim` (`bin/suite.sh:291`, `bin/suite.sh:302`, `bin/suite.sh:312`, `bin/tests/test_suite_motivo.sh:87`).

   **PROCEDE — régua e pre-push não passam por esse transporte:** ambos chamam a trava diretamente. A régua julga o texto do log; o pre-push distingue 75 e preserva o retorno capturado (`bin/regua.sh:165`, `bin/regua.sh:170`, `bin/regua.sh:178`, `bin/pre-push.sh:142`, `bin/pre-push.sh:149`, `bin/pre-push.sh:157`).

   **DUVIDA — scripts de esteira em `logs/` e hook efetivamente instalado:** conteúdo e dependências de rc 75/2 nesses caminhos são **[nao verificado]**.

5. **O caso 6 morde o delta? Quais mutantes escapam?**

   **PROCEDE — morde TERM → 9, HUP → 127 e veto à limpeza após sinal**, incluindo o novo mutante sempre TERM (`bin/tests/test_rm_com_guarda.sh:228`, `bin/tests/test_rm_com_guarda.sh:229`, `bin/tests/test_rm_com_guarda.sh:236`, `bin/tests/test_rm_com_guarda.sh:269`).

   **PROCEDE — não força sinal anterior ao nascimento, dois sinais ou interrupção do `wait` final:** os sinais são enviados após um segundo; o `wait` atual fica depois do laço. O comentário “depois de um wait interrompido” não corresponde a uma janela deliberadamente construída pelo teste (`bin/tests/test_rm_com_guarda.sh:227`, `bin/tests/test_rm_com_guarda.sh:231`, `bin/tests/test_rm_com_guarda.sh:235`, `bin/suite.sh:301`).

   **DUVIDA — mutante candidato a verde:** trocar o lançamento por `_run &` e recolher `_RC_RUN=$?` imediatamente após o `wait` final elimina o transporte pelo arquivo, preservando os retornos esperados nos cenários atuais. Verde efetivo: **[nao verificado]** (`bin/suite.sh:291`, `bin/suite.sh:301`, `bin/suite.sh:302`, `bin/tests/test_rm_com_guarda.sh:227`, `bin/tests/test_rm_com_guarda.sh:235`).

   **DUVIDA — outro candidato:** inserir `_N_ENTREGUE=$_N_SINAL` logo após atribuir `_PID_RUN` descarta sinais anteriores a esse ponto; o selo não provoca essa janela. Verde efetivo: **[nao verificado]** (`bin/suite.sh:292`, `bin/suite.sh:294`, `bin/tests/test_rm_com_guarda.sh:227`, `bin/tests/test_rm_com_guarda.sh:235`).

   **PROCEDE — retirar todo o `pkill` deveria ser acusado:** os handlers da trava falsa deixariam de produzir os retornos exigidos 9/127. Resultado executado: **[nao verificado]** (`bin/suite.sh:295`, `bin/tests/test_rm_com_guarda.sh:224`, `bin/tests/test_rm_com_guarda.sh:228`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:236`).

6. **Quem escreve e quem lê**

   | Estado | Quem escreve | Quem lê |
   |---|---|---|
   | `_RCF` | Pai atribui o caminho: `bin/suite.sh:290`; subshell escreve o conteúdo: `bin/suite.sh:291` | Subshell usa o caminho: `bin/suite.sh:291`; pai lê e remove: `bin/suite.sh:302` |
   | `_N_SINAL` | Pai inicializa e traps incrementam: `bin/suite.sh:284`, `bin/suite.sh:285`, `bin/suite.sh:286`, `bin/suite.sh:287` | Traps no incremento; laço na comparação e quitação: `bin/suite.sh:285`, `bin/suite.sh:286`, `bin/suite.sh:287`, `bin/suite.sh:294`, `bin/suite.sh:295` |
   | `_SINAL` | Pai inicializa e traps substituem: `bin/suite.sh:284`, `bin/suite.sh:285`, `bin/suite.sh:286`, `bin/suite.sh:287` | Repasse, fallback e veto à limpeza: `bin/suite.sh:295`, `bin/suite.sh:304`, `bin/suite.sh:308` |
   | `_PID_RUN` | Pai recebe `$!`: `bin/suite.sh:292` | Existência, seleção dos filhos e recolhimento: `bin/suite.sh:293`, `bin/suite.sh:295`, `bin/suite.sh:301` |

   **PROCEDE — `_SINAL` tem múltiplos papéis:** último nome recebido, sinal a repassar, escolha do fallback e memória de “houve sinal” para vetar limpeza. Essa memória não prova que o run foi interrompido (`bin/suite.sh:285`, `bin/suite.sh:295`, `bin/suite.sh:304`, `bin/suite.sh:308`).

   **PROCEDE — `_PID_RUN` identifica o subshell escritor; o alvo do repasse é seu filho**, a trava. `_N_SINAL` conta registros das traps, enquanto `_N_ENTREGUE` marca o contador inteiro após um único `pkill`, origem da perda de sequência (`bin/suite.sh:291`, `bin/suite.sh:292`, `bin/suite.sh:285`, `bin/suite.sh:295`).

## 14/11 10:07 · sessao 8bbc76de12b1 · - · - · origem=Ronald (tmux)

Evidências do export e leitores, todas da cópia:

| Decisão/etapa | Corpo lido | Registro / caso |
|---|---|---|
| Fonte efetiva do TXT pela tela | `app/folha/views.py::gerar_dominio_txt:207` chama `recalcular_fechamento_mes(...somente_leitura=True, retornar_objeto=True)`; passa os mesmos objetos ao classificador em `:212` e ao montador em `:268`. Portanto essa porta recalcula antes de emitir; não usa apenas o valor persistido. | A escolha de fonte não aparece como autoridade específica no registro consultado: **SEM JUIZ [não verificado]**. |
| Campo do evento | `app/folha/export.py::eventos_do_fechamento:446`: `float(getattr(fech,'horas_folga_trabalhada',0) or 0)`. | `_TT5`, registro em `app/core/juizes.py:732`; teste `app/ponto/tests/test_classe3_folga_100.py::ExportFolgaTrabalhadaTest::test_MORDE_o_export_emite_a_folga_trabalhada:50` apenas injeta 11,17 e verifica repasse em `:53`; não prova cálculo contra regra. |
| Isenção elimina todos eventos | `app/folha/export.py::eventos_do_fechamento:417`: `if isento_ponto(colaborador): return []`, `:418-419`. | `_TT5` registra a função em `app/core/juizes.py:732`; caso com Art.62 e resposta zero: `app/folha/tests/test_isento_cargo_export.py::IsentoCargoExportTest::test_isento_zero_eventos:58`, asserção `:68`. |
| Universo/status de montagem | `app/folha/export.py::montar_linhas_export:717`: filtra `STATUS_NO_TXT`, literal `('aberto','aprovado')` em `:687`; só percorre colaboradores em `entra_set`, `:726-727`. | `_K2` registra `classificar_export`, não o filtro de `montar_linhas_export`: `app/core/juizes.py:320`; autoridade específica desse filtro **SEM JUIZ [não verificado]**; **SEM CASO [não verificado]**. |
| Zero e rubrica ausente eliminam linha | `app/folha/export.py::montar_linhas_export:733`: `if not valor: continue`; `:735-737`: `if not rub: continue`. | Função não aparece no registro vivo consultado: **SEM JUIZ [não verificado]**; **SEM CASO [não verificado]**. |
| Rubrica e tipo | `app/folha/export.py::_rubricas_map:988-990`: globais ativas, sobrescritas por específicas ativas da empresa. O número 200 não é constante runtime; depende do cadastro. | **SEM JUIZ [não verificado]** para a resolução. Migração altera global 150→200 em `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py::para_200:11`. Teste `app/ponto/tests/test_classe3_folga_100.py::RubricaDaFolgaTrabalhadaTest::test_MORDE_a_rubrica_global_da_folga_trabalhada_e_200:68` verifica migração, não número apurado. Rubrica real de produção: **[não verificado]**. |
| Unidade/forma do valor TXT | `app/folha/export.py::montar_linha_10:39-45`: se tipo `dias`, inteiro arredondado; qualquer outro tipo, `int(round(float(valor)*100))`; campo numérico em nove posições. Portanto, para tipo horas, centésimos de hora decimal, sem multiplicar por 2. | Função não aparece no registro consultado: **SEM JUIZ [não verificado]**; **SEM CASO [não verificado]**. |
| Falta é outro ramo | `app/folha/export.py::montar_linhas_export:740-744` consulta `datas_falta` e rubrica `falta_diurna`; não compõe `folga_trabalhada_diurna`, que já saiu pelo laço `:730-739`. | Os corpos são independentes em `app/folha/export.py:730-744`. |
| Rescisão | `app/folha/export.py::sai_pela_rescisao:60`: demissão não nula e `<= fim_janela`; `classificar_export:866-871` retira colaborador inteiro. | `_K2`, `app/core/juizes.py:320`. Caso contra CLT art.477 escrito em `app/folha/tests/test_rescisao_modulo_proprio.py:1-5`: `::RescisaoModuloProprioTest::test_desligado_na_janela_vira_rescisao:24`; fronteiras `::H12DemissaoNaJanelaTest::test_demissao_no_ultimo_dia_da_janela_segue_rescisao:72` e `::test_MORDE_demissao_depois_da_janela_nao_e_rescisao:75`. Não calcula montante FT. |
| Sem código | `app/folha/export.py::classificar_export:872-875`: código strip vazio → fora. | `_K2`, `app/core/juizes.py:320`; `test_classificar_export.py:40` verifica comportamento, sem regra CLT/CCT/L-NNN escrita: **SEM CASO [não verificado]**. |
| Férias com batida | `app/folha/export.py::dias_ferias_com_batida:491-503`: faixas de férias intersectadas à competência e data local de batida apurável dentro da faixa; `classificar_export:876-884` retira colaborador inteiro. | `_K2` registra classificador em `app/core/juizes.py:320`, helper não registrado especificamente **[não verificado]**. Caso numérico FT contra regra: **SEM CASO [não verificado]**. |
| Cadastro zero | `app/folha/export.py::classificar_export:902-946`: trabalhadas zero, pelo menos duas batidas apuráveis e nenhum dia com minutos previstos; retém em `:959-967`. | `_K2`, `app/core/juizes.py:320`; previsto é juiz registrado em `:185`; caso contra regra: **SEM CASO [não verificado]**. |
| Pendência do espelho | `app/folha/export.py::classificar_export:968-973`; compat `motivos_retencao_espelho:1306-1307` relê janela e células, ignorando números recebidos; `motivos_retencao_celula:1207-1213`. | `_K2`, `app/core/juizes.py:320`; helper não registrado especificamente **[não verificado]**. |
| Desligado não retém célula | `app/folha/export.py::motivos_retencao_celula:1207-1211`: isento ou `esta_desligado` → vazio. | `esta_desligado` registrado em `app/core/juizes.py:267` [confirmar número exato antes de usar]; caso pela lei escrita em `app/folha/tests/test_lei_desligado_nao_trava.py:1-10`: classe **confirmar nome [não verificado]**, `::test_MORDE_desligado_nao_retem_nenhum_dia:67` e controle `::test_MORDE_ativo_e_afastado_seguem_retendo:79`. |
| Furo só de pausa não retém | `app/folha/export.py::nao_retem_pela_pausa:1173`: protestos exatamente `{'FURO_PARCIAL'}` e `_furo_so_da_pausa`; esse helper em `:1151-1155` exige sem desalinhado/aberto/não-sei, alguma lâmpada apagada e todas de pausa. | Helpers não registrados especificamente **[não verificado]**; casos L-033: `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83`; controle `::test_MORDE_entrada_faltando_RETEM:94`. Conferem lista de retenção, não montante FT. |
| Outros predicados da retenção | `app/folha/export.py::_dias_sem_decisao:1239-1297`: recorta admissão/demissão; sem célula e sem vínculo não cobra; passado sem célula/veredito retém; vereditos acusadores retêm salvo protestos não cobraveis ou pausa; dia demissão recebe exceções. | `_K2` é registro do consumidor classificador `app/core/juizes.py:320`; helper não registrado especificamente **[não verificado]**; casos individuais de montante FT: **SEM CASO [não verificado]**. |
| Homologação | `app/folha/export.py::_codigos_nao_homologados:999-1012`: catálogo vazio desliga gate; eventos zero ou sem rubrica ignorados; código zfill fora do catálogo retém pelo classificador `:974-979`. | `_K2`, `app/core/juizes.py:320`; caso numérico contra regra **SEM CASO [não verificado]**. |
| Gate da porta | `app/folha/views.py::gerar_dominio_txt:128-133` chama `porta_export.conferir`; `app/folha/porta_export.py::conferir:807-821` exige snapshot de hoje e mesma competência, valor não nulo/não negativo e zero falhas; positivo recusa em `:865-867`. | Não aparece em `JUIZES`: **SEM JUIZ [não verificado]**; casos de paridade não contam pela exigência da chamada. |

Leitores/reinterpretações:

- `app/relatorios/cartao_pela_celula.py::totais_da_folha:69-76` só verifica existência de fechamento, chama `fechamento_lido` e usa objeto recalculado. `_totais_do_fechamento:97-99` inclui FT em `total_extras`; `:116` expõe `trab_folga`. `folha_manda:321-346` escolhe esses totais; se ausentes, usa `_totais_da_lavratura`, que lê soma DiaPago (`:126-175`).
- `app/ponto/services/espelho.py::espelho_do_colab:1283-1284` e `app/relatorios/pdf_espelho.py::_coletar_dados_espelho:628` chamam `folha_manda`. O espelho também roda motor em `autoridade_do_periodo:1164-1179`; PDF monta/roda motor em `_coletar_dados_espelho:394-434`.
- `app/ponto/services/espelho.py::montar_dias:811-813` mostra `pago_do_dia` e rubricas DiaPago. `app/ponto/services/dia_pago.py::pago_do_dia:890-898` retorna `None` sem linha ou sem nenhuma rubrica monetária, senão soma trabalhadas + FT + detalhe de folga-feriado/60.
- `app/ponto/services/espelho.py::horas_do_dia:133-136`: DiaPago existente manda, inclusive zero; sem linha mostra `periodo.minutos_trabalhados/60`, uma leitura do motor ao vivo. App usa esse caminho em `app/api/views.py::api_espelho_v2:1525-1543`.
- **Comparador do cartão não compara FT:** `app/relatorios/cartao_pela_celula.py::divergencia:265-269` só compara extras50, extras100, noturno, intra e dias_falta; `trab_folga` e `total_extras` ficam fora da lista literal.
- Outro comparador soma trabalhadas+FT nos dois lados: `app/folha/porta_export.py::linha_x_badge:245-276`; tolerância `0.015h` em `:239`.
- PDF de export lê o próprio TXT; `app/folha/pdf_export.py::traduzir_texto:112-125` extrai inteiro bruto e resolve tipo pelo cadastro atual ativo; `_fmt:36-40` divide por 100 se horas. Resolução de tipo própria por código em `:95-101`, podendo reinterpretar unidade conforme cadastro atual. Ele não roda motor nesse corpo.
- `app/folha/views.py::preview_exportacao:330-331` lê classificação gravada; geração usa a leitura recalculada em `:207-212`.
- Não creditar `test_bateria_bo0611_05_ferias_desligado.py`: o próprio arquivo declara “caracterização medida, não a lei” em `:11-13`; testes `:712`, `:722`, `:732`, `:763` conferem tabela medida. [nao verificado]

Nenhum dado fornecido demonstra uma linha real do TXT, rubrica ativa, batidas ou valores de produção: **[não verificado]**.

_5025 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
