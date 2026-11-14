# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 12:08** · publicado em **14/11/2026 12:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 12:08 · sessao e2117872ca12 · - · - · origem=Ronald (tmux)

Casos adicionais confirmados:

| Decisão | Regra escrita / caso e resultado | Teste | [nao verificado]
|---|---|---|
| C18: `None` não equivale a folga | L-022/L-061; `trabalha=None`, ata trabalho ou sem tipo → `dia_sem_trabalho=False`; ata folga → `True`; `trabalha=False` → `True`. `app/docs/LEIS.md:73,95`; matriz `app/chamados/tests/test_f1c4_3_none_cobra.py:1-13` | `app/chamados/tests/test_f1c4_3_none_cobra.py::JuizDiaSemTrabalhoTest::test_RED_none_com_ata_de_trabalho_NAO_e_dia_sem_trabalho:39`; `::test_RED_none_sem_tipo_dia_NAO_e_dia_sem_trabalho:42`; `::test_clausula_2_ata_de_folga_segue_dia_sem_trabalho:45`; `::test_MORDE_trabalha_false_e_dia_sem_trabalho:50` |
| Premissa pela ata | L-061; saída apagada em ata trabalho e `trabalha=None` → premissa não regularizada; ata folga → regularizada. `app/chamados/tests/test_f1c4_3_none_cobra.py:7-12` | `app/chamados/tests/test_f1c4_3_none_cobra.py::LeitoresDoJuizNoneCobraTest::test_RED_premissa_do_furo_em_dia_none_NAO_esta_regularizada:105`; `::test_clausula_2_premissa_em_ata_de_folga_esta_regularizada:111` |
| Parcial cobra resto | Regra literal “abono parcial = cobra o resto”, leitura L-061; ata ausência parcial e `trabalha=True/None` → `False`; ausência integral ou `trabalha=False` → `True`. `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:4-20` | `app/chamados/tests/test_emis_ausencia_parcial_cobra.py::JuizAusenciaParcialTest::test_RED_ausencia_parcial_com_trabalha_true_NAO_e_dia_sem_trabalho:40`; `::test_RED_ausencia_parcial_com_trabalha_none_NAO_e_dia_sem_trabalho:43`; `::test_MORDE_ausencia_de_dia_inteiro_segue_dia_sem_trabalho:46`; `::test_trabalha_false_segue_dia_sem_trabalho:54` |
| Ata transporta parcial ao juiz | Mesma matriz; ata lavrada parcial → cobra; integral → cala. `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:19-20,76-82` | `app/chamados/tests/test_emis_ausencia_parcial_cobra.py::AtaLavraACoberturaParcialTest::test_RED_a_ata_lavrada_e_o_juiz_concordam_com_a_regra:76` |
| Fábrica lê folga/ausência/limbo | L-061 explicitada; limbo com saída apagada → pergunta; ata folga/ausência → nenhuma. `app/chamados/tests/test_f1c4_3_emissores.py:6,20-30` | `app/chamados/tests/test_f1c4_3_emissores.py::EmissoresPerguntamAoJuizTest::test_RED_fabrica_limbo_pergunta:80`; `::test_fabrica_folga_nao_pergunta:83`; `::test_fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta:89` |
| Limbo exportado vai ao DP | L-092 explicitada; limbo + TXT vigente → zero pergunta/uma pauta; sem TXT → pergunta/zero pauta; trabalho `True` + TXT → pergunta/zero pauta. `app/chamados/tests/test_f1c4_3_emissores.py:158-166` | `app/chamados/tests/test_f1c4_3_emissores.py::LimboEmCompetenciaExportadaVaiAPautaTest::test_RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp:199`; `::test_limbo_em_aberta_pergunta_sem_pauta:207`; `::test_MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta:212` |
| Leitor supra lê célula soberana | L-061; `trabalha=False` com ata trabalho → `COBRANCA_EM_DIA_SEM_TRABALHO`; limbo sem tipo não ganha código. `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:5-9` | `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py::SupraPerguntaAoJuizTest::test_RED_celula_folga_com_ata_trabalho_e_dia_sem_trabalho_pelo_juiz:47`; `::test_MORDE_limbo_trabalha_none_sem_tipo_na_ata_nao_retrata:56` |

A referência L-092 em `test_fio_le_o_juiz.py:11-12` versa sobre preservar TXT/gravado. Os testes de leitura admin `:105`, `:115`, `:121`, `:277` e validação `:291` não verificam TXT/gravado; não constituem caso L-092 da decisão de mostrar pergunta no fio. `app/chamados/tests/test_fio_le_o_juiz.py:105-125,277-313`.

## 14/11 12:07 · sessao fc236b942a9d · - · - · origem=Ronald (tmux)

Inventário explorado em modo somente leitura; execução desses caminhos em produção [não verificado].

| Grupo | Decisão executável e entradas | Escritor/caminho |
|---|---|---|
| Fábrica da célula | Não pergunta se `nunca_bateu`, dia sem trabalho, situação diferente de ativo, futuro, veredito fora de `VEREDITOS_ACUSAM` ou parcial destinada à lista. `app/chamados/services/fabrica_da_celula.py:105`, `:112`, `:114`, `:120`, `:123` | `perguntar` chama disputa e gera perguntas em `app/chamados/services/fabrica_da_celula.py:172` e `:174`. |
| Marco exigível | Só motivo do marco faltante contido em `vencidos`; sem conjunto vencido não pergunta. `app/chamados/services/fabrica_da_celula.py:58`, `:69`, `:71` | O vencimento vem de `quando + timedelta(minutes=teto_min) <= agora`. `app/ponto/services/cartorio.py:892` |
| Parcial só lista | `cobertura_parcial` e `sem_periodo`, ou julgamento anterior sem fato novo. `app/ponto/services/cartorio.py:424`, `:426`, `:428` | Lido na fábrica (`app/chamados/services/fabrica_da_celula.py:123`), cartório (`app/ponto/services/cartorio.py:1757`), c2 (`app/chamados/reconciliador.py:883`) e cluster (`app/ponto/services/cluster_espurio.py:168`). |
| Passado sem turno | Sem marco vencido nem pendente retorna `None` no passado. `app/chamados/services/fabrica_da_celula.py:47` | Abre pauta de cadastro e não pergunta. `app/chamados/services/fabrica_da_celula.py:203` |
| Limbo exportado | Motivos presentes, `trabalha is None` e competência entregue. `app/chamados/services/fabrica_da_celula.py:228` | Pauta DP em vez de pergunta. `app/chamados/services/fabrica_da_celula.py:214` |
| Deduplicação da pergunta | Exclui motivos já presentes no domínio `q_dos_fatos`. `app/chamados/services/fabrica_da_celula.py:145`, `:154`, `:157` | Porta `PerguntaDisputa.abrir`, por `app/chamados/services/disputa_emissao.py:444`. |
| Pai da disputa | Disputa aberta agrega; inexistente abre chamado pai. `app/chamados/services/disputa_emissao.py:535`, `:568` | Cria `DisputaSupervisao` em `app/chamados/services/disputa_emissao.py:587`. |
| Pergunta datada | Sem data para pergunta de hora não nasce; outras sem data usam conversa. `app/chamados/services/disputa_emissao.py:425`, `:431` | `abrir_conversa` em `:439`; `abrir` em `:444`. |
| Repasse de órfãos | Retira dias na lista do admin, exige regularizável e ausência de pergunta pendente ou cobrança pela ata. `app/chamados/services/disputa_emissao.py:1047`, `:1086` | Sem disputa aberta cria pai em `app/chamados/services/disputa_emissao.py:1104`. |
| Declaração de hora | Exige módulo furo, hora válida, data e casamento com marco faltante; sem célula recusa. `app/chamados/services/declaracao_texto.py:320`, `:322`, `:332`, `:335`, `:342` | Pai via `disputa_para_declaracao` em `:360`; pergunta ausente nasce em `:376`. |
| Declaração em texto | Followup com hora que casa marco gera proposta; sem proposta não aplica. `app/chamados/services/declaracao_texto.py:611`, `:625`, `:649` | Pai em `:659`; pergunta em `:666`. |
| Cobrança do cartório | Códigos de furo/morta/limbo, não desligado, sem vivo e dentro do piso; não afastado; dia previsto com âncora; fora da lista parcial. `app/ponto/services/cartorio.py:1747`, `:1765`, `:1769`, `:1776`, `:1790` | Emissor canônico em `app/ponto/services/cartorio.py:1791`. |
| Furo parcial | `FURO_PARCIAL`, não desligado, piso, não afastado, marcos faltantes; apply sem vivo e fora da lista. `app/ponto/services/cartorio.py:1813`, `:1818`, `:1820`, `:1828` | Emissor em `app/ponto/services/cartorio.py:1829`. |
| Funil de ausência | Degenerada, suspeita, faixa feriado, posto fechado ou célula vetada impedem cobrança. `app/ponto/management/commands/detectar_ausencias.py:601`, `:605`, `:617`, `:629`, `:632` | `ChamadoColaborador.abrir` em `app/ponto/management/commands/detectar_ausencias.py:694`. |
| Sem célula | Fora da exceção de limbo, abre cadastro sem grade. `app/ponto/management/commands/detectar_ausencias.py:658` | `_chamado_dia_sem_grade` abre em `app/ponto/management/commands/detectar_ausencias.py:511`. |
| Folga indefinida | Dia indefinido/defeito, ciclo flexível, passada corrente, início passado e sem cobertura. `app/ponto/management/commands/detectar_ausencias.py:160`, `:165`, `:175`, `:177`, `:179` | Pai limbo em `:191`; ciclo dependente de âncora abre em `:272`. |
| Silêncio sob limbo | Sete dias sem batida impedem calar; caso contrário pode agrupar. `app/chamados/services/limbo_folgas.py:156`, `:167` | Agrupamento sem push/fio pelo retorno em `app/ponto/management/commands/detectar_ausencias.py:759` e `:763`. |
| Grade | Degenerada/suspeita/célula vetam; encerrado reabre. `app/ponto/management/commands/reconciliar_grade.py:495`, `:499`, `:502`, `:529` | Abre em `app/ponto/management/commands/reconciliar_grade.py:507`. Cron registrado sem `--apply`: `app/config/crons.py:462`. |
| Reemissão c2 | Grupo DIA, ativo, piso se nascer/renascer, isenção, cobertura, protesto, parcial, célula, veto da régua, motor e cerca adicional do intervalo. `app/chamados/reconciliador.py:809`, `:821`, `:841`, `:851`, `:856`, `:870`, `:883`, `:887`, `:903`, `:907`, `:909` | Abre em `app/chamados/reconciliador.py:911`. |
| Adesão | Sem batida, não isento/desligado, sem ausência não-falta; encerrado fora de cascata reabre. `app/chamados/services/adesao.py:65`, `:67`, `:70`, `:90` | Pai em `:75`, reabre em `:93`; batida refuta pai em `:145`–`:151`. |
| Cluster | Dia sem trabalho cala; insumos ausentes desconhecido; ≥2 órfãs, marco apagado e intervalo entre órfãs ≤15 min acusam. `app/ponto/services/cluster_espurio.py:25`, `:29`, `:33`, `:35` | Ativo com vínculo aberto, fora da lista parcial, abre em `:183`, `:185`, `:194`, `:196`. Cron usa `--alarme`: `app/config/crons.py:598`. |
| Par relâmpago | Par completo, sem alvo retratável e fora de `so_retratar` abre chamado. `app/ponto/management/commands/detectar_par_relampago.py:353`, `:362`, `:366` | Abre em `:368`; cron `--apply --retratar`: `app/config/crons.py:605`. |
| Turno atrasado | Não isento, aberto com entrada, sem cobertura; lembrete `pos_20`, sem permite HE e sem alerta anterior. `app/ponto/management/commands/processar_alertas_turno.py:85`, `:88`, `:92`, `:97`, `:135`, `:140`, `:314` | Abre em `:335` antes de push/justificativa. |
| Escada de turno | Nível N5; reusa chamado; silenciado cala; sem existente com ausência cala. `app/ponto/management/commands/processar_alertas_avancados.py:94`, `:100`, `:112`, `:116` | Cria em `:121`. |
| Retroativo turno | Aberto com entrada, idade mínima, vínculo, sem cobertura nem irmão nos dois módulos. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`, `:96`, `:101`, `:105`, `:108`, `:118` | Abre em `:151`; comando sob ordem em `app/config/crons.py:947`. |
| Anomalias cadastrais | Órfão sem regularização, múltiplas escalas, ordem inválida, posto desativado/ausente ou férias. `app/chamados/detector_anomalias.py:33`, `:63`, `:83`, `:85`, `:87`, `:89`, `:101`, `:109`, `:117` | Motivos passam à disputa em `app/api/views.py:764` e `app/api/views_core.py:758`. |
| Desvio da batida | Folga grave; comercial cala; órfã informativa; intervalo cala; desvio >30 grave, >15 leve. `app/chamados/detector_anomalias.py:170`, `:188`, `:216`, `:225`, `:239`, `:245` | Grave gera disputa em `app/api/views.py:702`; leve cria `_Ch9` em `:717`; gêmeo em `app/api/views_core.py:711`. |
| Segurança/aparelho | Selfie ausente, mock verdadeiro, assinatura inválida, liveness falhou/indisponível. `app/api/views.py:449`, `:458`, `:475`, `:482`, `:489` | Disputa em `app/api/views.py:764`. |
| S sem E / E sem S | Ausência de turno para S ou aberto fora de intervalo para E, mais motivo da lei. `app/api/views.py:622`, `:638` | Disputa em `:764`; caminho PWA em `app/ponto/services/triagem_batida.py:655`, `:731`, `:735`. |
| GPS | Sem GPS com posto ou posição fora sem cadastro em revisão; reusa chamado vivo por subcategoria. `app/ponto/services/geofence.py:269`, `:270`, `:308`, `:319`, `:353` | Cria `_ChG` em `app/ponto/services/geofence.py:361`. |
| Posição geográfica | Dentro se distância≤raio; sem accuracy incerto; accuracy excessiva sinal ruim; distância−accuracy>raio fora. `app/ponto/services/geofence.py:115`, `:117`, `:119`, `:121` | Registro de juiz em `app/core/juizes.py:1197`; `tem_gps` em `:1207`. |
| Acesso e cadastro | Afastado/sem posto/sem escala produz `_bloqueio`; sem acesso_bloqueado vivo cria chamado. `app/ponto/services/triagem_batida.py:56`, `:80`, `:92`, `:102` | `_Chamado.criar` em `:107`. Sem posto também cria por dia civil em `app/ponto/utils.py:31`, `:40`. |
| Férias na batida | `em_gozo_hoje` existente; dedup por dia civil. `app/ponto/utils.py:89`, `:94` | Cria em `app/ponto/utils.py:103`; detector chama em `app/chamados/detector_anomalias.py:116`. |
| Horário e anuência | Fora de todas as janelas ±10 min; ciclos flexíveis calam; dedup rolante 14h. `app/ponto/services/triagem_batida.py:406`, `:428`, `:452` | Cria em `:459`. Pedido com motivo sem vivo cria em `:323`, `:330`, `:336`. |
| Ausência do colaborador | Abre aviso persistente pela ausência própria; documento altera título. `app/ponto/services/ausencia.py:650`, `:656`, `:659` | Chamadores em `app/ponto/views.py:2547`, `app/api/views.py:1842`. Decidida fora de `ABERTAS` fecha em `app/ponto/services/ausencia.py:693`, `:695`. |
| Credor morto de pergunta viva | Pergunta sem resposta e viva; credor fora de VIVOS; exportada ou limbo impede; senão pede reabertura. `app/chamados/services/disputa_emissao.py:1821`, `:1827`, `:1835`, `:1839`, `:1851` | `credor.reabrir` em `:1851`. |

Reinterpretações importantes: o detector de desvio remonta o casamento batida×marco com `_match_marcos` e mede novamente o desvio (`app/chamados/detector_anomalias.py:212`, `:228`); a reavaliação geofence recalcula as ocorrências contra o posto atual (`app/ponto/services/geofence.py:422`, `:430`) e retrata por premissa (`:433`, `:441`). O helper `motivo_da_lei` devolve `True` em exceção (`app/escala/servico_jornada.py:566`, `:571`).

Registro confirmado: `_C16` veto da régua (`app/core/juizes.py:292`), `_C18` dia sem trabalho (`:294`), cadastro entregue (`:297`), turno aberto há (`:245`), lembrete (`:248`), geofence/selfie/GPS (`:1197`, `:1201`, `:1207`). Cluster e par relâmpago estão explicitamente em `SEM_JUIZ['batida']` (`app/core/juizes.py:1209`).

Os testes `test_quatro_prazos_*` chamam de regra a própria escada/cron, embora citem L-110 (`app/ponto/tests/test_quatro_prazos_turno_aberto_ha.py:3`, `app/ponto/tests/test_quatro_prazos_lembrete_fim_turno.py:3`); a classe de paridade compara com o head (`app/ponto/tests/test_quatro_prazos_turno_aberto_ha.py:88`, `:96`). Esses trechos não demonstram caso independente contra CLT/CCT/L-NNN substantiva.

## 14/11 12:07 · sessao 57496b21bfa7 · - · - · origem=Ronald (tmux)

Exploração concluída, somente leitura. Consolidação de decisões e caminhos com referências:

| Decisão | Predicado efetivo e dado de entrada | Caminho de escrita/leitor | Registro efetivo |
|---|---|---|---|
| Transição de estado | Matriz humana/sistema; terminal absoluto `cancelado/superado`; `fechado` humano exige `admin_ti`; estado nascente aceita EMITIR/REGISTRAR. `app/chamados/catalogo/motor.py::decidir:125` | Retorna `permitido/novo_estado`; matrizes em `app/chamados/catalogo/motor.py:99` e `:110`. | `_C2`, `app/core/juizes.py:278` |
| Premissa morta | Classe conhecida e fato sustentador coletado falso; classe desconhecida ou fato None preserva. `app/chamados/catalogo/premissa.py::avaliar:123`; `::premissa_morta:149` | Chamado retratado por `app/chamados/reconciliador.py::retratar_premissa_morta_colab:1459` → `_retratar:524`. | `_C3`, `app/core/juizes.py:279` |
| Turno aberto do chamado | Núcleo com entrada e sem saída em dia−1/dia; veto se todos respectivos dias posicionalmente completos; contexto `entrada_timestamp` prevalece, senão mesmo `data_turno`. `app/chamados/reconciliador.py::turnos_abertos_de:97`, `::turno_do_chamado_aberto:140` | Alimenta fato em `_veredito_premissa:287`; sem dia usa busca global `_tem_turno_aberto:174`. | SEM JUIZ para estes corpos; o catálogo consumidor é `_C3`, `app/core/juizes.py:279`. |
| Par denunciado existe | Cada hora de contexto encontra exatamente uma batida no dia; retratada ou dois tipos iguais dissolve; ambiguidade conserva None. `app/chamados/reconciliador.py::_par_integro:197` | `_veredito_premissa:295` → catálogo → `_retratar:524`. | SEM JUIZ para o coletor; `_C3` registra catálogo, `app/core/juizes.py:279`. |
| Âncora/filhos sustentam chamado | Escala ativa com âncora nula; filhos de IDs do contexto com status VIVOS. `app/chamados/reconciliador.py::_ancora_ausente:233`, `::_filhos_vivos:243` | `_veredito_premissa:291` → catálogo → `_retratar:524`. | SEM JUIZ para os corpos; `_C3`, `app/core/juizes.py:279`. |
| Retenção da retratação | Irmãs respondidas sem consumo; ou, no ato de aprovar J1, chamado aponta J2 distinta e pendente. `app/chamados/reconciliador.py::retencao_da_retratacao:447` | `_retratar:519` e `encerrar_com_retencao:493` recusam, gravam trilha por `registrar_retencao:479`. | SEM JUIZ; registro de `respostas_sem_veredito` é outra função, `app/core/juizes.py:298`. |
| Absolvição pela célula | Dia conhecido, célula existe, veredito preenchido fora de VEREDITOS_ACUSAM e nenhum marco pendente. `app/chamados/reconciliador.py::_absolvido_pela_celula:1024` | `retratar_chamados_absolvidos_por_celula:1001` → `_retratar:1005`; consumidor outbox `:1369`. | SEM JUIZ para a composição; registro C6 aponta outra função, `app/core/juizes.py:282`. |
| Órfã encerra pela lâmpada | Sem texto de resposta, data presente, fato do próprio marco True, pergunta_viva True, dia anterior ao local e `dia_encerrado_motivo` None; relógio retorna TETO. `app/chamados/reconciliador.py::veredito_orfa:1083` | `encerrar_orfas_pela_lampada:1136` → `p.encerrar_fantasma:1148`; disputa fecha se sem respostas sem veredito e pode_fechar, `:1164`. | SEM JUIZ para composição; C6/C7 registrados em `app/core/juizes.py:282`. |
| Todos marcos cobrados acesos | Cobrados não vazios, todos tipados, cada lâmpada correspondente existe e está acesa; desconhecido retorna None. `app/ponto/services/lastro.py::marcos_acesos:170` | `fechar_pelo_marco_colab:395` → reconciliar `:401`; cartório `_protesta:1459`; sync `app/ponto/services/entrada_sincronizada.py:30`. | SEM JUIZ; busca do corpo não aparece como autoridade em `app/core/juizes.py` [não verificado: prova negativa integral]. |
| Marcos deixaram de existir | Cobrados tipados, ata tem lâmpadas e nenhum cobrado existe nela. `app/ponto/services/lastro.py::marcos_inexistentes:110` | Cartório seleciona e reconcilia `app/ponto/services/cartorio.py:1551`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Leitor lastro recalcula | Sem lâmpada, reconstrói turnos e procura batida do mesmo tipo a até 90 minutos do marco; classifica fechamento por `reconciliaria()`. `app/ponto/services/lastro.py::julgar:299` | Quadro `com_lastro` versus `retido_pelo_fechamento`, `:316`; evento fecha somente lâmpada True, `:395`; vigia detecta fallback como escape, `:423`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Chamado protesta contra dia | `cel.trabalha is not False and marcos_acesos(x, ata_atual) is not True`. `app/ponto/services/cartorio.py::_julgar_colab_corpo::_protesta:1458` | Alimenta classificar_dia `:1464`, estado `cobrado:1502`; não protestantes reconciliados `:1539`; lote adiado se qualquer um com fio e `adiar_pela_lampada`, `:1534`. | SEM JUIZ; o próprio registro só menciona este leitor em comentário, `app/core/juizes.py:145`. |
| Cobrança indevida do cartório | Códigos incluem COBRANCA_EM_DIA_SEM_TRABALHO ou COBRANCA_SEM_FURO. `app/ponto/services/cartorio.py:1565` | Todos vivos passam ao reconciliar `:1573`. | SEM JUIZ para composição; C18 registra dia_sem_trabalho, `app/core/juizes.py:294`. |
| Fio morto só duplicata | Há perguntas, nenhuma viva, nenhuma resposta com texto, todas via duplicata. `app/ponto/services/cartorio.py:1645` | Reconciliar `:1651`; se C8 preserva atual, tenta outros módulos caláveis sem texto, `:1661`. | SEM JUIZ para composição; C8/C15 são partes, `app/core/juizes.py:284`, `:291`. |
| Ausência pendente/aprovada | Chamado VIVO em universo e dia conhecido; aberta silencia/renova sem sobrepor silêncio vivo alheio; aprovada fecha só se `_premissa_morre`; sem cobertura levanta marcador. `app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia:61` | Arquivo `:87`, reconciliar `:102`, levantar `:73`; evento `app/ponto/signals.py:263`. | `_C11`, `app/core/juizes.py:287` |
| Ausência mata premissa | Justificativa retorna True; demais exigem premissa_morta com fatos falsos e célula.veredito fora de acusadores. `app/chamados/services/silencio_ausencia.py::_premissa_morre:35` | Lida por reconciliar_por_ausencia `:95`. | SEM JUIZ como composição separada; `_C11` registra chamador, `app/core/juizes.py:287`. |
| Ausência decidida encerra solicitação | Ausência existe e status não pertence a ABERTAS. `app/ponto/services/ausencia.py::fechar_chamado_da_ausencia:693` | Reconciliar `:695`; universo em `chamados_da_ausencia_decidida:710`; evento `app/ponto/signals.py:271`. | SEM JUIZ para composição; ABERTAS registrada em `app/core/juizes.py:256`. |
| Justificativa aprovada encerra origem | J aprovada, chamado_origem existente e VIVO. `app/ponto/services/justificativa.py::encerrar_chamado_de_origem:33` | `retratar:46`; invocada na aprovação `:217`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Justificativa encerra bloqueio | Mais recente chamado NAO_FECHADO `entrada_bloqueada_turno_aberto` do colaborador, excluindo origem; sem filtro de data. `app/ponto/services/justificativa.py::_encerrar_bloqueio_de_entrada:76` | `encerrar_com_retencao:87`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Ajuste retroativo encerra cascata | Módulos literais de turno e contexto data_turno igual, ou contexto ausente e criado_em no dia; NAO_FECHADOS. `app/ponto/services/justificativa.py::_cascata_do_turno:55` | Materialização chama encerrar_com_retencao, `::materializar_saida_dp:396`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Feriado/folga encerra | Dia apurado, fatos_do_dia.era_dia_previsto não None e faixa feriado/folga; premissa falta_descoberta falsa aceita catálogo. `app/chamados/services/feriado_reavaliar.py::espurios:31` | `encerrar:48` → reconciliar; evento `reavaliar_dias:83`. | SEM JUIZ para composição; partes `_F2` e `_C3`, `app/core/juizes.py:302`, `:279`. |
| Cluster nasce/morre | Dia sem trabalho dá (); ata sem listas dá None; >=2 órfãs, >=1 lâmpada acesa False e par adjacente <=15min dá assinatura. `app/ponto/services/cluster_espurio.py::_leitura_da_celula:25` | Próprio dia de qualquer idade alimenta catálogo `::_vivos_que_a_premissa_matou:74`; reconciliar exige ativo + escala ativa sem fim `:183`. | SEM JUIZ para assinatura/composição; `_C3`, `app/core/juizes.py:279`. |
| Geofence perde acusação | Posto atual com coordenadas, ocorrências com coordenadas; conta classificar_posicao(distância,accuracy,raio)=='fora'; zero acusações vai ao catálogo. `app/ponto/services/geofence.py::reavaliar_chamados_geofence:423` | `retratar:441`; chamado por evento do posto `app/ponto/signals.py:291`. | SEM JUIZ para composição; posição tem registro [não verificado: linha exata]. |
| Adesão fecha no evento | Existe batida não retratada na janela `contexto.periodo`, se dois extremos; sem eles, qualquer batida. `app/colaboradores/services/pos_vinculo.py::fechar_adesao_com_batida:37` | Reconciliar `:43`; signal `app/ponto/signals.py:233`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Adesão fecha no lote | Qualquer batida não retratada na vida; executar só fechar=True e dry=False. `app/chamados/services/adesao.py::sincronizar_adesao:143` | Reconciliar `:151`; predicado diverge do recorte por período do evento, `app/colaboradores/services/pos_vinculo.py:40`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Adesão agrupa furos | Filhos não terminais, data conhecida deve estar no período, ainda não agrupado sob pai. `app/chamados/services/adesao.py:103` | Contexto agrupado_em `:113`; reconciliar `:116`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Limbo permite agrupamento | Sete dias corridos até data sem batida não retratada impedem calar. `app/chamados/services/limbo_folgas.py::semana_muda:155`, `::pode_calar:167` | `agrupar_furo:276` grava contexto, reata perguntas `:283`, reconcilia `:284`; se recusado desfaz reata e repete gate `:287`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Filho segura limbo | Filho não absolvido; dia existe; algum vínculo cobre data e tem defeito_folga; pergunta de fato do dia com fio vivo. `app/chamados/services/limbo_folgas.py::filhos_no_defeito:195` | Detector só reconcilia pai na passada corrente com previsão calculável, sem defeito atual e sem filhos segurando: `app/ponto/management/commands/detectar_ausencias.py:146`. | SEM JUIZ para composição; vínculo/C6 são partes [não verificado: linha exata do registro de vínculo]. |
| Competência trancada encerra | período_trancado existe; chamado VIVO da competência, módulo não isento, não resgatado após tranca, nenhuma pergunta viva ligada. `app/chamados/services/competencia_trancada.py::medir:95`, `:113`, `:160` | `encerrar:209` → encerrar_por_competencia_trancada; disputa exige pode_fechar `:227`. | SEM JUIZ para composição; C14/C20 são partes, `app/core/juizes.py:290`, `:296`. |
| Reabertura da competência | Chamados via competencia_trancada com dia dentro da janela; perguntas por mesma via/data. `app/chamados/services/competencia_trancada.py::reabrir:276` | `q.reabrir_por_competencia:279`; `ch.reabrir(humano=True):290`. | SEM JUIZ [não verificado: prova negativa integral]. |
| CSF pode encerrar | Nenhuma pergunta sem veredito de disputas do pai nem perguntas que o resolvem, ignorando tranca. `app/ponto/supra_juiz.py::perguntas_que_seguram_csf:573`; `::pode_executar_csf:588` | Command só retrata com lavra `cobranca_indevida`, código conhecido, sem conversa e não órfão de regeneração: `app/ponto/management/commands/supra_juiz.py:249`. | Pergunta literal registrada, `app/core/juizes.py:299`; seleção completa do command SEM JUIZ. |
| Beco fecha disputa | Disputa aberta, nenhuma pergunta sem resposta cujo destino não esteja terminal, pai terminal; então pode_fechar. `app/chamados/services/becos.py::classificar:27` | `fechar_beco:54` resolve perguntas e fecha disputa `:56`; signal pai reconcilia `app/chamados/signals.py:43`. | SEM JUIZ para recorte; `_C7`, `app/core/juizes.py:283`. |
| Disputa fechada fecha pai | Não nasceu agora, fechada_em preenchido, pai existente e fora de ENCERRADOS. `app/chamados/signals.py::disputa_fechada_fecha_chamado_pai:29` | Reconciliar `:43`. | SEM JUIZ para composição; `_C7` é decisão da disputa, `app/core/juizes.py:283`. |
| Desligamento encerra | Disputa sem respostas_sem_veredito fecha com motivo desligamento; todos chamados NAO_FECHADOS passam encerrar. `app/colaboradores/services/desligamento.py:164`, `:182` | Disputa.fechar `:167`; chamado.encerrar `:187`; nota só depois de gate `:188`. | SEM JUIZ para composição; respostas_sem_veredito registrado `app/core/juizes.py:298`. |
| Regularização externa encerra | Apenas erros sem ações retorna antes; senão marca materializada, se chamado vinculado encerra. `app/chamados/services/regularizacao_ext.py:192` | reg.status `:198`; chamado.encerrar `:206`. | SEM JUIZ [não verificado: prova negativa integral]. |
| Admin decide dia fecha fio | Perguntas q_vivas do dia de turno e conversa=False; chamados_vivos_do_dia relidos após fechar perguntas. `app/ponto/services/veredito_celula.py::fechar_o_fio_do_dia:180` | fechar_por_admin `:190`, DecisaoPergunta `:195`, reconciliar chamados `:203`. | SEM JUIZ para composição; C6 é parte, `app/core/juizes.py:282`. |
| Resposta tardia reabre | Chamado já não VIVO, célula acusa e `_renascer_por_premissa_viva()` permite. `app/chamados/services/resposta_tardia.py::receber:35` | `:46`; caso contrário nota tardia e pauta [não verificado: trecho posterior]. | SEM JUIZ para composição; `_C8` registra abrir, `app/core/juizes.py:284`. |

Arquivamento é outro efeito de cobrança, pois o corpo escreve silêncio sem `status_local`: `app/chamados/services/arquivo.py::silenciar:166` e `:171`.

| Decisão de arquivo | Predicado/escrita | Registro |
|---|---|---|
| Arquivo individual | Permissão/área e POST; silêncio até prazo_padrao. `app/chamados/views_arquivar.py::arquivar_chamado:26`, `:37` | SEM JUIZ para composição; `_F8` prazo em `app/core/juizes.py:306`. |
| Prescrição cron | VIVO, criado antes do corte, silenciado_ate nulo, resgatado_em nulo e verbo validar/cobrar/decidir. `app/chamados/management/commands/arquivar_prescritos.py::Command.handle:49` → arquivo `:94` | SEM JUIZ para composição; `_F8`, `app/core/juizes.py:306`. |
| Arquivo em lote humano | Mesmos verbos/idade/silêncio nulo e pode_agir; não exige resgatado_em nulo. `app/chamados/views_arquivar.py::arquivar_lote:125`, `:135` | SEM JUIZ para composição; `_C4`, `app/core/juizes.py:280`. |
| Arquivo competência encerrada | Data do fato ou criado_em conforme tipo; data<=corte e nenhuma disputa viva. `app/chamados/management/commands/arquivar_competencia_encerrada.py::Command.handle:111` | SEM JUIZ para composição. `_competencia:179` recalcula mês e fallback21; silêncio de dez anos `::_aplicar:274`, `:288`. |
| Desarquivo | Via tranca exige reabrir humano; depois levantar com resgatado=True e ressuscitar novamente após trilha. `app/chamados/views_arquivar.py::desarquivar_chamado:64`, `:77`, `:86` | SEM JUIZ para composição. |

Nascimento de pergunta até cadastro:

- `app/chamados/services/fabrica_da_celula.py::motivos_a_perguntar:105` cala nunca_bateu; `:112` cala dia_sem_trabalho/inativo; `:114` futuro; `:120` veredito que não acusa; `:123` parcial_so_lista; `:145` dedup pelo fato. Corpo completo SEM JUIZ; `dia_sem_trabalho` é `_C18`, `app/core/juizes.py:294`.
- `app/chamados/services/fabrica_da_celula.py::vencidos_no_instante:46` chama marco+30min; sem marcos passados retorna None, hoje/futuro vazio. `app/ponto/services/cartorio.py::marcos_com_teto:890` exige escala/tipo e compara quando+30min<=agora. Corpo SEM JUIZ [não verificado: prova negativa integral].
- Passado ativo, sem dispensa e sem turno vai a pauta cadastro e retorna: `app/chamados/services/fabrica_da_celula.py::perguntar_pela_celula:201`.
- Pauta já entregue se cabeça viva ou fechada em data local >=dia: `app/chamados/services/fabrica_da_celula.py::pauta_cadastro_existente:327`. Registro literal `app/core/juizes.py:297`.
- Limbo com motivos em competência entregue vai a pauta, não pergunta: `app/chamados/services/fabrica_da_celula.py::limbo_vai_a_pauta:228`; chamado por `perguntar_pela_celula:214`. Composição SEM JUIZ.
- Parcial sem período ou com período sem fato novo desde julgamento vai só à lista: `app/ponto/services/cartorio.py::parcial_so_lista:424`; fato novo consulta timestamps de batida/ausência/justificativa/DNA ou marco recém vencido apagado: `::houve_fato_novo:380`, `::_marco_venceu_apagado:445`. Corpos SEM JUIZ [não verificado: prova negativa integral].
- Ata sem célula permite questionário se sem escala vigente; com escala impede; exceção de leitura retorna True,None: `app/chamados/services/disputa_emissao.py::_ata_do_dia:894`. SEM JUIZ para composição.
- Entrada batida: núcleo recomputa `app/ponto/signals.py::_nucleo_post_save_batida:35`; cartório julga dias `::_cartorio_post_save_batida:132`; reconciliador roda depois `::_reconciliador_post_save_batida:207`.
- Entrada ausência: cartório `app/ponto/signals.py::_cartorio_post_save_ausencia:144`; consumidor após commit reconcilia e encerra solicitação `::_consumidor_ausencia:263`.
- Entrada justificativa: `app/ponto/signals.py::_cartorio_post_save_justificativa:162`.
- Entrada cadastro escala: núcleo recomputa `app/ponto/signals.py::_nucleo_post_save_escala:73`; pós vínculo julga cada dia preservando vereditos humanos e fecha adesão `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo:62`, `:89`.

Casos de teste promissores para conferência pela regra, sem presumir sua qualificação final: [nao verificado]

- `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py::AFechoNoEventoTest::test_A1_MORDE_marco_aceso_fecha_no_evento_com_a_nota_da_batida:97`, `test_A3_contra_exemplo_marco_APAGADO_fica_vivo:122`, `test_A4_todos_ou_nenhum_dois_marcos_um_apagado_fica_vivo:130`; cabeçalho chama “casos pela regra” em `:8`, mas vínculo textual CLT/CCT/L-NNN ainda [não verificado].
- `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py::CorrigeParaALuzTest::test_1_MORDE_resposta_que_concorda_com_a_luz_fecha_a_cobranca_no_ato:140`; `RecusaTest::test_2_MORDE_recusa_da_resposta_discordante_com_dia_coerente_fecha_no_ato:159`; `AdminDecideTest::test_3_MORDE_admin_decide_a_pergunta_muda_do_dia_que_acusa_outro_marco:177`; vínculo CLT/CCT/L-NNN [não verificado].
- `app/ponto/tests/test_ausencia_aberta_nao_fecha_chamado.py::AusenciaAbertaNaoFechaChamadoTest::test_MORDE_aguardando_decisao_segura_o_chamado:39`, `test_MORDE_aguardando_documento_segura_o_chamado:46`; vínculo CLT/CCT/L-NNN [não verificado].

## 14/11 12:07 · sessao ff6601cccd06 · - · - · origem=Ronald (tmux)

Escritores persistentes atuais de `status_local`: `app/chamados/models.py::ChamadoColaborador.abrir:1018` (`get_or_create` com defaults), `::criar:1142` (`create`) e `::_gravar_estado:228,296` (atribuição + save). `::_vivas_entre:356,371` troca o estado somente em memória e restaura em `finally`. Nenhum outro escritor persistente por nome/alias foi encontrado na busca estática dos arquivos Python operacionais; abrangência dinâmica além disso [não verificado].

Inventário dos **39 pontos de chamada** de nascimento encontrados:

| # | arquivo::função:linha | Predicado de emissão lido |
|---|---|---|
| 1 | `app/colaboradores/views.py::solicitar_beneficio:1981` | POST, colaborador associado, tipo e descrição preenchidos (:1971-1978). |
| 2 | `app/colaboradores/views.py::agenda_enviar_aceite:2367` | Permissão editar_agenda e rascunhos AgendaDia existentes (:2353-2359). |
| 3 | `app/ponto/utils.py::bloquear_se_sem_posto:40` | `posto_id` ausente; não existe mesmo módulo criado hoje (:25-38). |
| 4 | `app/ponto/utils.py::bloquear_se_em_ferias:103` | `em_gozo_hoje(...).first()` existe; não existe mesmo módulo criado hoje (:89-101). |
| 5 | `app/ponto/views.py::bater_ponto:291` | POST tipo E, `tem_bloqueio_turno`, sem regularização pendente; nenhum vivo do módulo (:268-290). |
| 6 | `app/ponto/views.py::ping_geo:3012` | `dentro_geofence is False`, não mock, pelo menos duas linhas já filtradas como fora/não mock (:3002-3009). Não exige que as duas últimas leituras gerais sejam fora: filtra antes de ordenar. |
| 7 | `app/escala/regua_defesa.py::vetar_cobranca_se_degenerada:51` | `regua_degenerada`: template ausente/exceção/marco inicial ou final ausente/iguais (:23-32,45-47). |
| 8 | `app/api/views_core.py::api_bater_ponto:711` | Classificador de batida fora de escala devolve `leve` (:696). |
| 9 | `app/api/views.py::api_bater_ponto:717` | `detectar_batida_fora_escala` devolve `leve` (:701-705). |
| 10 | `app/api/views.py::api_justificativas:1966` | Tipo/data/descrição, data ISO válida, nenhum chamado vivo turno_aberto/batida_ausente do dia (:1944-1965). |
| 11 | `app/api/views.py::api_abrir_chamado:2170` | Assunto e descrição preenchidos; classificação manual se área/categoria e área na TAXONOMIA, senão classificador (:2145-2168). |
| 12 | `app/chamados/reconciliador.py::reemitir_chamado_do_dia:911` | Horizonte, isenção, ausência, protesto cobrante, parcial_so_lista, veto da célula e régua, motor; costura em chamado já vivo dispensa alguns gates (:835-909). |
| 13 | `app/chamados/views.py::abrir_chamado_rapido:973` | Permissão gerir_chamados e mensagem preenchida (:967-972); reutiliza vivo mensagem_dp (:975-976). |
| 14 | `app/colaboradores/services/vinculo.py::_executar_aceite:410` | Chamado apenas quando `v['modo']=='aceite'` (:385-386); encerra antigos escala_aceite vivos antes (:401-404). |
| 15 | `app/colaboradores/services/desligamento.py::executar_desligamento:195` | Executado no desligamento, após tentar encerrar chamados NAO_FECHADOS (:182-190); condições anteriores da função [não verificado]. |
| 16 | `app/ponto/services/justificativa.py::rejeitar_justificativa_dp:291` | Justificativa ainda pendente obtida sob lock; grava rejeição e então cria chamado (:277-287). |
| 17 | `app/ponto/services/cluster_espurio.py::reconciliar_colab:196` | Colaborador ativo, escala ativa sem data_fim; dias com assinatura de cluster que não estão em `na_lista` (:183-195). Usa `p['achados']`, não `p['a_abrir']` (:193), deixando dedup à porta. |
| 18 | `app/ponto/services/geofence.py::verificar_geofence:361` | Alerta sem GPS com posto, ou classe fora sem cadastro em revisão; alerta e msg_push definidos; nenhum vivo da subcategoria (:269-280,292-331,353-361). |
| 19 | `app/ponto/services/ausencia.py::abrir_chamado_ausencia_colab:659` | A função emite diretamente aviso da ausência recebida; identidade por ausência_id (:649-662). Predicado de autoria está no chamador [não verificado]. |
| 20 | `app/ponto/services/triagem_batida.py::triagem_bater_ponto:107` | `_bloqueio` por afastado, sem posto ou nenhuma escala ativa, e nenhum vivo acesso_bloqueado (:55-107). |
| 21 | `app/ponto/services/triagem_batida.py::ramo_solicitar_anuencia:336` | Motivo preenchido e nenhum vivo batida_fora_horario (:323-330). |
| 22 | `app/ponto/services/triagem_batida.py::_bloqueio_horario_clt:459` | Vínculo do dia, exclui ciclos flexíveis, fora de todas as janelas hoje/ontem ±10min; nenhum mesmo módulo criado nas últimas 14h (:402-430,452-457). |
| 23 | `app/ponto/management/commands/detectar_par_relampago.py::Command.handle:368` | Par candidato completo pelo juiz; apply; nenhum alvo retratável; não só_retratar (:352-366). |
| 24 | `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle:121` | N5, nenhum turno_aberto vivo do mesmo turno nem turno_aberto_24h NAO_FECHADO; ausência não cobre (:94-120). |
| 25 | `app/ponto/management/commands/reconciliar_grade.py::Command._emitir_furo:507` | Sem veto por régua degenerada/suspeita e sem veto da célula (:494-504). |
| 26 | `app/ponto/management/commands/detectar_ausencias.py::Command.handle:191` | Ciclo fora dos ciclos com âncora, corrente, data posterior ao início, ausência não cobre e não dry (:165-190); condição externa que entrou nesse ramo [não verificado]. |
| 27 | `app/ponto/management/commands/detectar_ausencias.py::Command.handle:272` | Sem data_ancora_colaborador; data posterior ao início; ausência não cobre; corrente; não dry (:245-267). |
| 28 | `app/ponto/management/commands/detectar_ausencias.py::Command._chamado_dia_sem_grade:511` | Chamado por `_abrir_chamado` quando não limbo e não existe CelulaDia (:660-669). |
| 29 | `app/ponto/management/commands/detectar_ausencias.py::Command._abrir_chamado:694` | Passou vetos anteriores; exige célula fora limbo (:660-669); detalhe completo anterior [não verificado]. |
| 30 | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle:151` | Sem irmão detectado, apply; guardas anteriores completas [não verificado] (:124-145). |
| 31 | `app/ponto/management/commands/processar_alertas_turno.py::Command._abrir_chamado_critico:335` | Chamado pelo marco pos_20 quando nenhum AlertaTurno pos_20 existe (:314-316); nascimento ainda passa pela porta. |
| 32 | `app/chamados/services/regularizacao_ext.py::criar_regularizacao_externa:247` | Criação explícita de regularização e itens, sem condição adicional nessa função (:236-247). |
| 33 | `app/chamados/services/abertura.py::abrir_chamado_local:47` | Abertura explícita; categoria válida de catálogo pode trocar módulo; senão classificador (:28-43). |
| 34 | `app/chamados/services/abertura.py::abrir_chamado_para_colaborador:172` | Tipo abrível; dia exigido presente; nenhum vivo com mesma entrada e mesmo autor (:149-170). |
| 35 | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa:568` | Nenhuma disputa aberta do colaborador; existente agrega em vez de nascer (:531-566). |
| 36 | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1104` | Lista de órfãos regularizáveis elegíveis não vazia; nenhuma disputa aberta (:1086-1103). |
| 37 | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1339` | Categoria ancora_limbo, status não ENCERRADOS, nenhuma pergunta ancora_ausente não materializada do destino, nenhuma disputa aberta (:1316-1338). |
| 38 | `app/chamados/services/adesao.py::sincronizar_adesao:75` | Universo de células nunca_bateu; nenhuma batida não retratada; não isento; não desligado; ausência hoje não cobre exceto falta; não dry (:35-41,59-74). |
| 39 | `app/chamados/management/commands/smoke_chamados.py::Command._bloco_b_transicoes._novo:124` | Instrumento smoke; predicado do chamador [não verificado]. |

Núcleo e portas relevantes:

| Decisão | Código | Registro |
|---|---|---|
| Natureza acionável → EMITIR, auditoria → REGISTRAR | `models.py::_evento_nascimento:1097-1098`; `catalogo/natureza.py::eh_acionavel:38`, módulos :18-23 | `eh_acionavel` não encontrado no registro; motor C2 em `app/core/juizes.py:278`. |
| Estado nascente aberto/registrado | `catalogo/motor.py::decidir:130-135` | C2, `app/core/juizes.py:278`. |
| **Sobrescrever estado decidido pelo motor** | `models.py::abrir:985-987` permite defaults substituírem status; `::criar:1110-1111` só troca status quando solicitado aberto | SEM JUIZ específico; motor registrado não decide esses overrides (`app/core/juizes.py:278`). |
| Dedup existente, renascer encerrado, filho sob pai vivo | `models.py::abrir:953-979` | C8, `app/core/juizes.py:284`; agrupamento composto não encontrado no registro. |
| Célula já absolve nascimento | `models.py::_celula_que_absolve:1159-1162` delega `_absolvido_pela_celula` | Funções não encontradas no registro. |
| Régua veta nascimento/renascimento | `models.py::_veto_da_regua:1183-1192`; `_renascer_por_premissa_viva:1062-1065` | C16 `app/core/juizes.py:292`. |
| Renascimento exige ativo, não cascade, competência não trancada | `models.py::_renascer_por_premissa_viva:1052-1057` | Composição específica não encontrada; competência C14 `app/core/juizes.py:290`. |
| Humano não fecha com justificativa pendente | `models.py::transicionar:136-142` | SEM JUIZ específico. |
| Humano não encerra disputa viva/pergunta viva | `models.py::transicionar:148-171` | C6 `app/core/juizes.py:282`; `disputa_viva_do_chamado` não encontrado no registro. |
| Resolvido exige premissa regularizada; legado sem dia aceita qualquer disputa fechada do colaborador | `models.py::transicionar:181-191` | Coletor e fallback não registrados; premissa_morta C3 `app/core/juizes.py:279` não cobre automaticamente esse coletor. |
| Furo regularizado por justificativa/admin/dia sem trabalho/marco aceso/inexistente/célula/fatos | `models.py::premissa_regularizada:1313-1363` | Coletor SEM JUIZ; componentes C18 `app/core/juizes.py:294`, C3 :279. `marcos_acesos`, `marcos_inexistentes`, `_celula_furada` não encontrados. |
| Encerramento automático exige premissa, conversa e pergunta viva permitirem | `models.py::_decisao_do_fechamento:674-697` | C2 :278; conversa `pode_executar_csf` registrado :299; coletor composto SEM JUIZ. |
| Furo não regularizado pode fechar por agrupamento/dedup sobrevivente; sem dia permite fechar | `models.py::_gate_premissa_do_fechamento:774-808` | Coletor SEM JUIZ; C8 registrado aponta `abrir`, não `cobranca_viva_do_dia` (`core/juizes.py:284`). |
| Pergunta viva terminal reata todas ou recusa | `models.py::_plano_pergunta_viva:381-395`; `_gate_pergunta_viva:417-445` | Composição SEM JUIZ; C6 registrado :282. |
| Guarda de reabertura: veto automático, admin sem fato novo, premissa, agrupado, irmão, motor | `models.py::recusa_da_reabertura:1384-1406` | C16 :292 e C2 :278; `houve_fato_novo`, agrupado e irmão não registrados. |
| Estado mudado terminal carimba, via válida e rebaixa; vivo limpa e restaura urgência | `models.py::_gravar_estado:228-253,296` | C1 conjuntos em `core/juizes.py:277`; ritual não tem registro próprio. |
| Analisar/cancelar/encerrar/encerrar por tranca | `models.py::analisar:850-853`, `::cancelar:829-835`, `::encerrar:1660-1666`, `::encerrar_por_competencia_trancada:1676-1686` | C2 :278; gates de pergunta viva compostos acima. |

Escritores **fora das portas**, em migrações:

- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`: aberto de AUDITORIA → registrado; reverso em :15.
- `app/chamados/migrations/0039_a3_via_backfill.py:24`: cancelado duplicado de módulo DUP_MODS com irmão elegível → superado, via e ponteiro; :25-28 preenche vias; reverso em :33.

Nos itens acima, teste por regra efetivamente lido e identificado como caso+resposta CLT/CCT/L-NNN: [não verificado]. O selo `app/chamados/tests/test_selo_status_so_pelas_portas.py::SeloStatusSoPelasPortasTest::test_nenhuma_escrita_fora_das_portas:340` é estrutural (varredura/allowlist :37-53,340-347), portanto não constitui caso pela regra.

## 14/11 12:07 · sessao 741fa9160409 · - · - · origem=Ronald (tmux)

Consolidação de leitores e escritores fora da porta:

| Leitor/escritor | Efeito observado no código | Referência |
|---|---|---|
| Escritor central do chamado | Atribui `status_local`, carimba `resolvido_em`/`via_encerramento`, rebaixa urgência; estado vivo limpa carimbos e restaura urgência; salva. | `app/chamados/models.py::ChamadoColaborador._gravar_estado:228`, `:233`, `:248`, `:296` |
| Simulação, sem persistência | `_vivas_entre` troca `status_local` em memória para julgar o estado posterior e restaura no `finally`. | `app/chamados/models.py::ChamadoColaborador._vivas_entre:356`, `:371` |
| Nascimento fora de `_gravar_estado` | `abrir` compõe `status_local` em defaults; `criar` recebe `status` e faz `objects.create`. | `app/chamados/models.py::ChamadoColaborador.abrir:985`; `::criar:1117`, `:1142` |
| Writer histórico fora da porta | Migração transforma cancelado em superado quando há exatamente um irmão elegível; reverso transforma superado em cancelado. Aplicação dessa migração na produção: [não verificado]. | `app/chamados/migrations/0039_a3_via_backfill.py::backfill:15`, `:20`, `:23`, `:24`; `::desfazer:33` |
| Writer histórico de nascimento | Migração converte aberto de auditoria em registrado; reverso registrado em aberto. Aplicação em produção: [não verificado]. | `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15` |
| Arquivamento | `silenciar` grava `silenciado_ate`, `silenciado_por`, `silenciado_motivo`, `atualizado_em`; não atribui `status_local`. | `app/chamados/services/arquivo.py::silenciar:166`, `:171` |
| Desarquivamento | `levantar` limpa silêncio; quando solicitado, carimba `resgatado_em`. Via competência trancada, a view primeiro chama `reabrir(humano=True)` e depois ressuscita perguntas novamente após a trilha do resgate. | `app/chamados/services/arquivo.py::levantar:194`, `:202`, `:205`; `app/chamados/views_arquivar.py::desarquivar_chamado:64`, `:70`, `:77`, `:86` |
| Painel admin | Fila é `VIVOS ∩ COBRANCA`, excluindo silêncio futuro. Encerrados saem dessa fila e entram no filtro histórico. Contadores de resolvidos/fechados leem status gravado. | `app/chamados/services/painel.py::qs_vivos:155`; `::qs_na_fila:189`; `::montar_contexto_painel:212`, `:217`, `:218`, `:303`, `:316` |
| Verbo/pílula admin | Reinterpreta perguntas e módulo para calcular verbo; silêncio preenchido vira `arquivado` se futuro ou `historico` se vencido. | `app/chamados/services/verbos.py::particionar_verbos:34`, `:47`, `:48`, `:51` |
| Crédito de pergunta admin | Usa `q_vivas`, exclui materializada com `veredito_lavrado='sim'`, separa por `respondida_em`. Não chama o juiz C6 completo nem relavra stale nesse corpo. | `app/chamados/services/raiox.py::creditos_perguntas:74`, `:135`, `:139`, `:151` |
| Rótulos de tela | Admin recebe verbo ou `Registro`; colaborador recebe mapeamento de espera ou `Encerrado`. | `app/chamados/services/verbos.py::rotulo_de_linha:154`; `::rotulo_para_pessoa:164` |
| Fio admin | Perguntas de disputa fechada entram pelo pré-filtro `q_vivas` e são filtradas novamente por `pergunta_viva`. | `app/chamados/services/fio.py::montar_chamados_fio:216`, `:224`, `:237` |
| Formulário rápido do fio | A condição do template é apenas status diferente de `fechado`, chamado local e permissão; oferece transições para análise/resolvido/fechado. | `app/templates/chamados/fio_colaborador.html:254`, `:255`, `:268` |
| Janela de trabalho colab | Usa `q_vivas` diretamente para incluir destinos fora da janela; exige `silenciado_ate IS NULL`, sem comparar a expiração nesse corpo. | `app/chamados/services/janela_colab.py::chamados_visiveis_colab:61`, `:63`, `:69`, `:73` |
| Perguntas no app | Pré-filtra por `q_vivas`, aplica juiz C6 completo; pendente visível requer `respondida_em IS NULL` e disputa aberta. | `app/chamados/services/pergunta_no_app.py::perguntas_do_app:49`, `:58`; `::consulta_do_app:70`, `:71` |
| Badge do app | `Responder DP` prioriza quantidade de perguntas pendentes; depois vêm chamados VIVOS com nota do DP não lida; senão nenhum badge. | `app/chamados/services/pergunta_no_app.py::projecao_do_app:90`, `:92`, `:98`, `:100`, `:103` |
| Dashboard colab | Mostra quantidade e texto `Responder DP` quando há questionários pendentes. | `app/templates/core/dashboard_colaborador_v2.html:78`, `:79`, `:80` |
| Navbar staff | Primeiro lê `PlacarNavbar.badge_chamados`; fallback recalcula verbos dos vivos; segundo fallback conta fila. Segundo número lê payload ou conta fila. | `app/core/context_processors.py::_badge_staff:84`, `:90`, `:93`; `::_badge_registrados:139`, `:150` |
| Navbar colab | Conta chamados da fila, por colaborador; esse número é diferente da seleção de perguntas pendentes do badge do app. | `app/core/context_processors.py::_badge_colab:185`, `:187`; `app/chamados/services/pergunta_no_app.py::consulta_do_app:71` |
| Estado da pergunta | Primeiro lê carimbos de validação/materialização; depois chama C6; sem carimbo pode retornar pendente/respondida mesmo quando C6 retorna morto. | `app/chamados/models.py::PerguntaDisputa.estado:3097`, `:3100`, `:3104` |
| Predicado SQL da pergunta | Recalcula resposta, vias de identidade/tranca, célula acusadora/absolvente e lâmpada pendente. É mais amplo que C6; property `viva` delega diretamente ao C6. | `app/chamados/models.py::PerguntaDisputa.q_vivas:3403`, `:3404`, `:3408`, `:3411`, `:3415`; `::viva:3431` |
| Cron lavra de perguntas | Seleciona perguntas com destino VIVO, identifica stale e, com `--apply`, chama `lavrar`. | `app/chamados/management/commands/lavrar_perguntas_stale.py::Command.handle:57`, `:60`, `:82`, `:89` |
| Cron reconciliador | Relê premissa, retenção e `reconciliaria` para contar resíduos; não encerra nesse laço. | `app/chamados/management/commands/reconciliar_chamados.py::Command.handle:76`, `:79`, `:85` |
| Cron ausência | `--alarme` só acusa; `--apply` chama serviço que encerra pela mesma porta. | `app/ponto/management/commands/fechar_chamados_ausencia.py::Command.handle:35`, `:44` |
| Vigia de fio | Pré-filtra perguntas de disputa fechada com `q_vivas` e aplica C6 antes de contar. | `app/chamados/services/vigia_fio.py:753`, `:759`, `:760` |
| Signal após encerramento | Status encerrado escrito aciona resolver das perguntas; fechamento da disputa tenta reconciliar o pai; cruza fronteira encerrado/vivo para relavrar destinos. | `app/chamados/signals.py::resolve_perguntas_ao_resolver:97`, `:105`, `:117`; `::disputa_fechada_fecha_chamado_pai:29`, `:34`, `:43`; `app/chamados/models.py::_gravar_estado:273`, `:303`, `:312` |
| Portão da folha | O ramo de retenção lê célula/veredito/protestos e exceção da pausa; não lê `status_local` nesse ramo. Chamado encerrado, por si, não prova liberação da folha. | `app/folha/export.py::_dias_sem_decisao:1266`, `:1273`, `:1286`, `:1293`, `:1295` |
| Diagnóstico fora da folha | Alimenta juiz da porta alcançável, que liga a situação do dia à fila; universo retido vem de `classificar_export` e motivos da célula. | `app/folha/fora_da_folha.py::_alimentar:61`; `::dias_que_seguram:119`, `:137` |

Arquivamentos que precisam entrar na contagem de decisões do efeito:

- Manual individual: POST/permissão/área, prazo padrão, depois `arquivo.silenciar`; `app/chamados/views_arquivar.py::arquivar_chamado:26`, `:28`, `:32`, `:36`, `:37`. SEM JUIZ para a seleção manual; o prazo está registrado em `_F8`, `app/core/juizes.py:306`.
- Lote manual: VIVOS, verbo validar/cobrar/decidir, idade, silêncio nulo, permissão contextual; `app/chamados/views_arquivar.py::arquivar_lote:105`, `:121`, `:125`, `:135`, `:137`, `:140`. SEM JUIZ para o conjunto desse corte; `_C4` registra apenas a permissão, `app/core/juizes.py:280`.
- Prescrição cron: idade `--dias` default 30, verbo acionável, silêncio nulo, `resgatado_em IS NULL`; `app/chamados/management/commands/arquivar_prescritos.py::Command.handle:32`, `:49`, `:53`, `:60`, `:94`. SEM JUIZ para o corte completo.
- Competência anterior: escolhe data do fato ou criação conforme escopo, exige data ≤ opção `corte`, impede disputa com pergunta viva e também impede na exceção; `app/chamados/management/commands/arquivar_competencia_encerrada.py::Command.handle:111`, `:121`, `:123`; `::_disputa_viva:154`, `:158`, `:162`; `::_regra_de_corte:170`; `::_aplicar:274`, `:288`. SEM JUIZ para esse conjunto; `_competencia` é cópia declarada pendente em `app/core/juizes.py:488`.
- Sem dia irrecuperável: módulo escopo DIA, vivo sem silêncio, dia do fato ausente, criação anterior à janela de `janela_atual(hoje,None)`, horizonte cinco anos; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py::Command.handle:52`, `:53`, `:58`, `:62`, `:65`, `:88`, `:94`. SEM JUIZ para o conjunto.
- Isento: `isento_ponto=True`, VIVOS, silêncio nulo; `app/chamados/services/silencio_isentos.py::alvos:24`, `:27`; `::silenciar:39`, `:43`. SEM JUIZ para a decisão completa.
- Ausência: viva e no universo; `ausencia_cobre(... para='cobranca')`; ausência aberta suspende/renova respeitando silêncio alheio; aprovada com premissa morta tenta reconciliar; ausência removida/decidida levanta silêncio; `app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia:61`, `:66`, `:78`, `:81`, `:87`, `:94`, `:102`, `:113`. Registrada `_C11`, `app/core/juizes.py:287`.

A execução efetiva dos caminhos, os totais de produção e a aplicação das migrações não são demonstrados pelos anexos fornecidos: [não verificado].

## 14/11 12:06 · sessao 100432e34dd4 · - · - · origem=Ronald (tmux)

| Decisão | Código e predicado lido | Grava / escritor | Leitor / consequência | Juiz real | Caso substantivo |
|---|---|---|---|---|---|
| Reabrir disputa administrativa | `app/chamados/models.py::DisputaSupervisao.reabrir_admin:2194`: exige `fechada_em != None`, contador `<2`, usuário presente | `:2205-2217`: limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; incrementa `reaberturas_admin`; acrescenta motivo novo se ausente | `app/templates/chamados/partials/modal_fio.html:379-390`: mostra fechamento, quantidade `/2`, limita botão | SEM JUIZ — registro real `app/core/juizes.py:276-299` | SEM CASO; testes de comportamento em `test_disputa_transicoes.py::DisputaReabrirAdminTest:131-177` |
| Autorizar ação administrativa e perguntar novamente | `app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin:245-257`: staff/grupo, motivo em choices, justificativa ≥20, última disputa fechada do chamado | `:259` chama porta; `:263-268` cria conversa de confirmação; `:269-270` reabre chamado encerrado com `humano=True` | Push ao colaborador em `:286-294`; formulário em `modal_fio.html:387-410` | SEM JUIZ para esses predicados | SEM CASO |
| Reabrir disputa pelo sistema | `models.py::DisputaSupervisao.reabrir_sistema:2223`: disputa fechada | `:2229-2236` limpa quatro campos de fechamento e acrescenta ocorrência; não incrementa contador administrativo | `models.py::DisputaSupervisao.aberta:1870-1871`; cron em `app/ponto/management/commands/alertar_disputas_pendentes.py:33-64` passa a cobrar disputa aberta | SEM JUIZ | SEM CASO substantivo localizado |
| Pedido de correção pelo colaborador | `models.py::DisputaSupervisao.reabrir_por_colab:2276-2287`: autor, disputa aberta, usadas `<2`, queryset corrigível não vazio | `:2293-2296` reabre perguntas e incrementa `reaberturas_colab` | `services/fio.py:340-343` e `app/api/views.py:1168-1173` recalculam oferta do botão por respostas presentes e teto; `_card_atendimento.html:194-197` mostra contador | SEM JUIZ para teto/seleção | SEM CASO; `test_porta_disputa_ciclo.py:283-350` é contrato/comportamento |
| Escolher respostas corrigíveis | `app/chamados/juizes.py::perguntas_corrigiveis:1673-1675`: respondida e `pergunta_sem_lastro_ou_viva`; helper `:1496` = sem materialização ou C6 viva | Seleção para `reabrir_para_correcao` | `acoes_disputa.py:96-103`, `views.py:1419-1436`, porta do colaborador `models.py:2284` | SEM JUIZ para seleção; C6 somente para subpredicado `pergunta_viva`, registrado `core/juizes.py:282` | SEM CASO |
| Reabertura da supervisão: corrigir ou recobrar | `app/chamados/views.py::reabrir_disputa_supervisao:1416-1436`: pendentes vivas por C6; se nenhuma corrigível e há pendentes vivas, recobra; se corrigíveis, reabre | `:1425` recobra; `:1436` chama porta | Push `:1447-1451`, mensagem de quantidade devolvida `:1459-1460` | C6 para vitalidade; SEM JUIZ para bifurcação | SEM CASO |
| Variante da ação no fio | `services/acoes_disputa.py::acao_reabrir_disputa:95-103`: conta sem resposta **sem filtrar C6**; se nenhuma corrigível retorna informação; caso contrário reabre | `:103` chama porta | Push `:109-110` | SEM JUIZ para contagem/bifurcação | SEM CASO |
| Limpar projeção da pergunta na reabertura | `services/disputa_emissao.py::reabrir_para_correcao:1593`: zera resposta, resposta temporal e validação; `:1627-1628` só registra histórico se algum estado foi retirado; `:1629` distingue desfazer absorção | `:1630` chama descarimbo em lote ou `:1632` faz `qs.update`; `:1615-1628` registra `DecisaoPergunta`; `:1633` emite eventos | `services/pergunta_no_app.py:70-75` põe viva sem resposta e disputa aberta nas pendentes; `:90-99` produz badge “Responder DP” | SEM JUIZ para protocolo | SEM CASO substantivo localizado |
| Recusar resposta e devolver ao colaborador | `services/validacao.py::recusar_pergunta:588-606`: motivo presente, pergunta existente, não validada, respondida | `:611-621` grava motivo e chama reabertura com ato `recusa` | Push `:638-640`; nova pendência pela consulta do app `pergunta_no_app.py:70-75` | SEM JUIZ; **`validar_pergunta` também não aparece no registro real** `core/juizes.py` | SEM CASO |
| Devolver validação recusada e reabrir continente | `services/validacao.py::devolver_validacao_recusada:211-241`: resposta de hora, não materializada, motivo de recusa presente; `reabrir=True` e disputa fechada relida | `:222-225` limpa validação; `:241` chama `reabrir_sistema` | Retorno ao admin e ocorrência em `:246-250` | SEM JUIZ | SEM CASO |
| Identificar perguntas absorvidas por ausência | `services/materializacao.py::perguntas_absorvidas_por:626-642`: mesmo colaborador/dia, hora, vias de ausência/declaracão; grupo `(disputa, materializada_em)` contém escape compatível com tipo; exclui batida plantada não retratada | Seleciona perguntas devolvidas | `devolver_absorvidas_da_ausencia_rejeitada:671` | SEM JUIZ | SEM CASO qualificado |
| Rejeição de ausência devolve perguntas | `materializacao.py::devolver_absorvidas_da_ausencia_rejeitada:662-673`: ausência rejeitada, nenhuma outra cobertura para cobrança, conjunto absorvido não vazio | `:688-692`: reabre com `desfazer_absorcao=True` e reabre disputas pelo sistema | Consulta do app `pergunta_no_app.py:70-75` | SEM JUIZ para composição; cobertura é autoridade registrada da família ausência | SEM CASO pelo recorte CLT/CCT/L-NNN; `test_o261_e1_ausencia_rejeitada_devolve.py:3-5,88-104` usa literal de aval, sem lei substantiva identificada |
| Elegibilidade para desfazer carimbo | `services/relavra_via.py::q_carimbo_desfazivel:181-184`: materializada e via fora de identidade; opcionalmente disputa aberta | Filtro antes da escrita em `:238` | Vigia usa o mesmo filtro em `services/vigia_fio.py:810-818` | SEM JUIZ para elegibilidade | SEM CASO substantivo localizado |
| Desfazer carimbo que C6 não sustenta | `relavra_via.py::desfazer_carimbo_sem_lastro:238-261`: relê linha elegível, exige C6 viva, descarimba; se disputa fechada reabre sistema | `:246` chama porta; `:261` reabre continente | `vigia_fio.py::carimbos_sem_lastro_por_dono:818-823`; contador `materializada_viva:855-856` | C6 para vitalidade, registro `core/juizes.py:282`; SEM JUIZ para composição/continente | SEM CASO substantivo; `test_o261_julgamento_desfaz_carimbo.py:10-19,92-104` cita L-110, que não fornece regra substantiva |
| Guardar irreversibilidade de identidade | `models.py::PerguntaDisputa.fora_do_fato:3257`; `descarimbar:3262`: recusa via em `VIAS_FORA_DO_FATO`; lote exclui mesmas vias `:3320-3322` | `descarimbar:3264-3268` limpa materialização/autor/via; lote `:3322` faz update | C6 mata vias que vencem resposta em `juizes.py:1412-1413` | SEM JUIZ próprio para guarda; C6 registrado | SEM CASO |
| Resposta nova derruba carimbo sem lastro | `models.py::PerguntaDisputa.reabrir_por_resposta:3369-3371`: materializada e via em `VIAS_SEM_LASTRO`, conjunto definido em `:3824-3825` | Descarimba em memória; `disputa_emissao.py::responder_pergunta_disputa_core:1471-1485` também limpa validação anterior; salva em `:1508` | Fila do admin e C6 leem projeção `juizes.py:1400-1429` | SEM JUIZ para invalidação dos campos | SEM CASO |
| Destrancar pergunta histórica | `models.py::PerguntaDisputa.reabrir_por_competencia:3376-3378`: apenas via `competencia_trancada` | Chama `descarimbar`; `services/competencia_trancada.py::reabrir:276-283` seleciona empresa, via e data na janela e registra ocorrência | C6 lê tranca em `juizes.py:1405-1406`; reabrir janela pode torná-la viva sem gravar carimbo | SEM JUIZ para essa seleção/porta; C20 registra leitura da tranca `core/juizes.py:296` | Há caso de L-051 para leitura/reversibilidade, **não prova específica da porta de carimbo legado**: `test_competencia_trancada.py::CompetenciaTrancadaTest::test_MORDE_pergunta_viva_ganha_a_via_e_a_disputa_fecha:122-138` |
| Reabrir fantasma enterrado | `models.py::PerguntaDisputa.reabrir_fantasma:3352-3354`: via fantasma e carimbo presente | Chama `descarimbar` | Comando abaixo restaura continente e pai | SEM JUIZ para guarda | SEM CASO substantivo específico da guarda |
| Seleção do comando de fantasmas | `management/commands/reabrir_orfas_enterradas.py::Command.handle:67-108`: fantasma materializada, não validada, datada; luz exatamente falsa, C6 viva, janela histórica fixa, competência não exportada; pai não recusa e não ressuscitaria irmãs | `:157` pergunta; `:165` disputa; `:172` pai, pelas portas | Exportadas vão à Pauta DP `:99-103,178` | C6 registrado; SEM JUIZ para seleção composta; usa `marcos_da_competencia()['exportada']` `:95` | **L-092 qualificado para veto de exportada:** `test_reabrir_orfas_enterradas.py::ReabrirOrfasEnterradasTest::test_exportada_nao_reabre_e_vai_para_a_pauta:128-140`; regra/casos escritos `:1-11` |
| Restaurar alcance do credor de pergunta viva | `services/disputa_emissao.py::reabrir_credor_de_pergunta_viva:1821-1848`: pergunta sem resposta e viva, âncora existente fora VIVOS; recusa conjunto com competência entregue/indecidível, texto sem carimbo, ou guarda do credor | `:1851` `credor.reabrir()`; trilha `:1852-1861` | Chamado volta a listar pergunta; acionamento pelo resolver em `:1943-1947` | C6 e C1 nas subperguntas; SEM JUIZ para composição de alcance/limbo | SEM CASO qualificado localizado |

Escritores concretos de reabertura de disputa encontrados: apenas `models.py:2205-2217` e `:2229-2233`; incremento do colaborador em `:2295-2296`. Chamadores de sistema: `models.py:1622`, `services/relavra_via.py:261`, `services/validacao.py:241`, `services/materializacao.py:692`, `management/commands/reabrir_orfas_enterradas.py:165`. Escrita persistente de reabertura fora dessas portas não apareceu nas buscas por atribuições/reset desses campos; ausência absoluta de escritor oculto por SQL dinâmico: [não verificado].

Chamadores da porta de correção encontrados: `models.py:2293`, `services/acoes_disputa.py:103`, `views.py:1436`, `services/validacao.py:620`, `services/materializacao.py:688`, comando histórico `management/commands/curar_perguntas_em_limbo.py:69`. Este último decide por igualdade exata com conjunto fechado medido `:46-58` e `--apply` `:64`, sem juiz registrado ou caso substantivo localizado.

Leitor que recalcula: `models.py::PerguntaDisputa.q_vivas:3404-3418` monta SQL por resposta, identidade, lâmpada pendente e veredito, como pré-filtro amplo; a consulta do app refina com C6 em `pergunta_no_app.py:49-58`. `DisputaSupervisao.pode_fechar:1908-1909` está registrado C7, porém `fechar:1942-1958` refaz a composição resposta sem veredito + perguntas vivas no corpo.

C20 tem caso substantivo L-051 de toque humano: `test_tranca_vivas_08.py:10-14`; `ToqueSoComAssinaturaDoAtoTest::test_RED_a_lavra_do_cartorio_com_usuario_da_request_NAO_e_toque:82`, `::test_contra_exemplo_lavra_via_admin_destranca:95`, `::test_contra_exemplo_registrar_log_com_acoes_destranca:101`.

## 14/11 12:06 · sessao 1dd9602e23ee · - · - · origem=Ronald (tmux)

| decisão | código/predicado lido | gravação / leitor | juiz registrado | caso pela regra |
|---|---|---|---|---|
| Fato novo autoriza desfazer fechamento administrativo | `app/ponto/services/cartorio.py::houve_fato_novo:357`, corpo:380-395: batida em janela `[dia-1,dia+2)` com criação/retratação em `(desde,ate]`; ausência cobrindo dia ou justificativa do dia com criação/atualização/aprovação nesse intervalo; regeneração/edição da célula nesse intervalo; senão marco vencido apagado | Só lê; consumido por `app/chamados/models.py:1391-1394` e `app/chamados/services/vigia_loop.py:25,50` | **SEM JUIZ**: registro `app/core/juizes.py:182-345` não contém essa função | L-110, caso e resposta: `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:7-12`; `::ReabrirPerguntaAPremissaTest::test_MORDE_16360_fechado_pelo_admin_sem_fato_novo_nao_reabre:73`, `::test_o_par_do_16360_com_fato_novo_na_celula_reabre:80` |
| Outro marco vencido apagado é fato novo | `app/ponto/services/cartorio.py::_marco_venceu_apagado:431`, corpo:445-451: diferença entre marcos vencidos até `ate` e até `desde`; ao menos uma lâmpada `acesa is False` com `(tipo,hora)` nessa diferença | Só lê; recalcula vencimento duas vezes em:445-446; relido pelo contador `app/chamados/services/vigia_loop.py:50` | **SEM JUIZ**, registro `app/core/juizes.py:182-345` | L-110: `app/chamados/tests/test_reabertura_por_marco_novo_nao_e_laco.py:8-11`; `::ReaberturaPorMarcoNovoNaoELacoTest::test_MORDE_fecha_pela_E_e_reabre_pela_S_nao_e_laco:71`, mesmo marco:75, S acesa:79 |
| Marco venceu | `app/ponto/services/cartorio.py::marcos_com_teto:882`, corpo:890-893: vínculo/template inexistente→`[]`; `quando + timedelta(minutes=teto_min) <= agora` | Só lê; `marcos_vencidos:865-879`, fábrica `app/chamados/services/fabrica_da_celula.py::vencidos_no_instante:35-49` | **SEM JUIZ** próprio, `app/core/juizes.py:182-345` | Caso de saída 17h vencida 17h30 no teste acima: `app/chamados/tests/test_reabertura_por_marco_novo_nao_e_laco.py:8-11,71-87` |
| Parcial fica só na lista administrativa | `app/ponto/services/cartorio.py::parcial_so_lista:398`, corpo:423-428: sem cobertura parcial→False; `periodo_parcial=="sem_periodo"`→True; demais parciais→julgamento anterior presente e nenhum fato novo | Cala emissão em `app/ponto/services/cartorio.py:1757-1758,1774-1776,1825-1828`; cala pergunta em `app/chamados/services/fabrica_da_celula.py:122-125` | **SEM JUIZ**, registro `app/core/juizes.py:182-345` | `test_cobparc5_parcial_sem_periodo_so_lista.py:6-25` escreve regra e respostas; **não cita CLT/CCT/L-NNN**. Pelo critério estrito solicitado: **SEM CASO**; há casos funcionais `::_SemPeriodo::test_RED_primeiro_julgamento_vai_a_lista_sem_pergunta:157`, rejulgamento:160 |
| Dia sem trabalho cala cobrança | `app/chamados/juizes.py::dia_sem_trabalho:441`, corpo:450-456: parcial de ausência só cala com `trabalha is False`; demais: `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho | Leitores: `_celula_furada:494`, fábrica `app/chamados/services/fabrica_da_celula.py:112,201` | `_C18`, `app/core/juizes.py:294` | L-022/L-061 escritos em `app/chamados/tests/test_f1c4_3_none_cobra.py:1-5`; `::JuizDiaSemTrabalhoTest::test_RED_none_com_ata_de_trabalho_NAO_e_dia_sem_trabalho:39`, `::test_MORDE_trabalha_false_e_dia_sem_trabalho:50` |
| Dia ainda furado/regularizado | `app/chamados/juizes.py::_celula_furada:459`, corpo:476-518: ausência inteira que cobre→False; célula ausente ou ata sem lâmpadas→None; dia sem trabalho consulta véspera; lâmpada apagada→True; órfã só prende se régua confiável | Consumido por `app/chamados/models.py::premissa_regularizada:1299`, chamada:1346; lê ata corrente por `celula_do_dia_agora:403-408` | **SEM JUIZ próprio** para `_celula_furada`; cobertura registrada em `app/core/juizes.py:190-191`, C18:294; premissa_morta C3:279 não registra essa função | L-110 em `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:7-12`; `::ReabrirPerguntaAPremissaTest::test_MORDE_32431_fechado_pelo_sistema_com_o_dia_regularizado_nao_reabre:61`, par apagado:67 |
| Elegibilidade para desfazer materialização | `app/chamados/services/relavra_via.py::q_carimbo_desfazivel:171`, corpo:181-184: materialização presente, via fora `VIAS_DE_IDENTIDADE`, opcional disputa aberta | Relido na query sob trava `relavra_via.py:238`; fábrica anota mesmo Q: `app/chamados/services/fabrica_da_celula.py:138-156`; vigia referido pelo código em `relavra_via.py:174-176` | **SEM JUIZ próprio**, `app/core/juizes.py:182-345` | L-110 com cenário e resposta `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:10-19`; `::JulgamentoDesfazCarimboSemLastroTest::test_MORDE_via_de_identidade_nao_se_desfaz:117`; fechada:139 |
| Materialização sem lastro cai e disputa fechada reabre | `app/chamados/services/relavra_via.py::desfazer_carimbo_sem_lastro:187`, corpo:240-261: candidato existe e `pergunta_viva`→`descarimbar`; disputa fechada→`reabrir_sistema` | `materializada_em/via_resolucao` pela porta `p.descarimbar():246`; disputa pela porta:261; julgamento chama com `reabrir_fechada=False` em `app/chamados/services/fabrica_da_celula.py:209` | C6 para vitalidade, `app/core/juizes.py:282`; composição “descarimbou e disputa fechada→reabrir” **SEM JUIZ** próprio | L-110: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:10-19`; `::JulgamentoDesfazCarimboSemLastroTest::test_MORDE_o_julgamento_desfaz_o_carimbo_da_pergunta_viva:92` |
| Cobrança fica órfã após reescrita | `app/escala/services/regeneracao.py::cobranca_orfa:141`, corpo:148-155: exige marcos faltantes e dia; compara criação com reescrita | `dia_reescrito_apos:108`, corpo:117-127: carimbo mais novo posterior manda; regeneração presente e anterior cala janela histórica; senão janela do log; marcador em `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas:232`, corpo:252-264→porta contexto grava `orfa_regeneracao` | `_C10`, `app/core/juizes.py:286` | Testes `app/escala/tests/test_cobranca_orfa_regeneracao.py` e `test_o10b_orfa_edicao.py` trazem cenários funcionais, mas o último:4-23 usa JUIZES.md/LEI-AKITA sem CLT/CCT/L-NNN: **SEM CASO** pelo critério estrito |
| Reescrever marcos órfãos ou segurar resposta | `app/ponto/management/commands/reconciliar_grade.py::Command._relavrar_orfa:430`, corpo:445-474: sem marcador→nada; mesmas horas→limpa marcador; pergunta respondida e `fio_vivo`→`ORFA_FIO_PRESO`; senão apply reescreve | Porta contexto grava `marcos_faltantes`, `orfa_regeneracao_curada` e retira `orfa_regeneracao`:473-474 | C15 para fio em `app/core/juizes.py:291`; decisão de reescrita/igualdade→**SEM JUIZ** próprio | `app/escala/tests/test_cobranca_orfa_regeneracao.py::...::test_ADVERSARIAL_pergunta_respondida_barra_a_reescrita:332` funcional; **SEM CASO** por falta de CLT/CCT/L-NNN |
| Regeneração recusa vínculo sem vigência declarada | `app/ponto/portas/celula.py::regenerar_celulas_vinculo:112`, corpo:153-162: `not tem_vigencia_declarada(ec)`→0 e motivo | Não altera célula; `barrados` e evento auditoria:156-161 | **SEM JUIZ próprio** para essa recusa, registro `app/core/juizes.py:182-345`; vínculo cobre é outra pergunta | L-099/L-110 com casos: `app/escala/tests/test_e4_regenerar_recusa_sem_vigencia.py:3-12`; `::RegenerarRecusaSemVigenciaTest::test_MORDE_desativado_sem_fim_e_recusado_com_motivo:52`, par com fim:64 |
| Regenerar, editar ou reverter cria dado para fato novo | `app/ponto/portas/celula.py::regenerar_celulas_vinculo:112`, corpo:284-311: pula lavrada; calcula previsto; cria ausente; só reescreve `origem=="gerada"` e diferença trabalha/geradora/DNA. `::editar_dia:572`, corpo:597-647: autor/motivo/booleano, guarda lavrada, vínculo; igual→no-op. `::reverter_regeneracao_dia:779`, corpo:818-837: só gerada com DNA anterior e guarda lavrada | Regeneração normal grava trabalha/geradora/DNA/anterior/carimbo/contador:306-311; edição grava origem/editada_em/por:638-647; reversão carimba regenerada_em:834-837. **Ramo passado fora vigência:272-282 grava trabalha/DNA sem regenerada_em** | K9 guarda lavrada `app/core/juizes.py:343`; regenerar/editar/reverter próprios **SEM JUIZ**, registro:182-345 | Testes de reescrita funcional em `app/escala/tests/test_cobranca_orfa_regeneracao.py:146-175`; **SEM CASO** global para todas essas decisões |
| Mudança de cadastro dispara julgamento e reavaliação | `app/ponto/portas/celula.py::_o_chao_do_dia_mudou:404`, corpo:460-472: recalculo e tocadas; pula futura e veredito humano; julga forçado. Corpo:528-540: qualquer tocada→on_commit rejunção e feriado/folga, mesmo recalculo=False | Chama cartório e depois dinheiro:472,496; leitores evento:534-539 | **SEM JUIZ** para esse portão próprio, registro `app/core/juizes.py:182-345` | `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:173-188` confere que fechado permanece fechado, mas **SEM CASO** no critério CLT/CCT/L-NNN |
| Feriado/folga encerra cobrança após cadastro | `app/chamados/services/feriado_reavaliar.py::espurios:24`, corpo:34-39: recalcula fatos do dia; previsto None→não decide; faixa feriado/folga e premissa morta→encerra | Porta `encerrar:44-56` chama `ch.reconciliar`; evento em `reavaliar_dias:65-85` | F2 `app/core/juizes.py:302`, C3:279; filtro composto próprio **SEM JUIZ** | Teste regeneração feriado `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:84-94`; **SEM CASO** estrito |
| Precedência de férias/afastamento/atestado/feriado/folga/trabalho | `app/ponto/precedencia.py::fatos_do_dia:242-256` delega `::fatos_do_periodo:259`; corpo:273-278 ausências não rejeitadas; :357-368 lê trabalha da célula ou recalcula previsto; :376-397 feriado suprime salvo vínculo que não vira folga e vence menor índice de PRECEDENCIA | Retorno relido pelo emissor `app/ponto/management/commands/detectar_ausencias.py:617` e reavaliador:34 | Registro `app/core/juizes.py:186-187`, F2:302 | Não localizei caso explícito CLT/CCT/L-NNN para **toda** essa precedência: **SEM CASO** [não verificado] |
| Folga de calendário deve pertencer ao vínculo do dia | `app/ponto/precedencia.py::folga_lancada_vale_no_dia:222`, corpo:235-239: None→False; sem calendário→True; calendário→mesmo vínculo do dia | `folga_lancada_do_dia:212-219`; precedência:356; grade lê função conforme `app/ponto/precedencia.py:231-233` | Registro pela autoridade `folga_lancada_do_dia`, `app/core/juizes.py:204` | L-110 e resposta: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:8-11`; `::FolgaDeCalendarioEDoVinculoTest::test_MORDE_folga_de_calendario_de_outro_vinculo_nao_e_folga_do_dia:45`, manual:50 |
| Posto fechado em feriado cala renascimento | `app/colaboradores/services/feriado_do_posto.py::cobra_no_dia:60`, corpo:67-71: posto None ou `opera_em_feriado is not False`→cobra; senão não cobra se feriado municipal/nacional do posto | Emissor chama em `app/ponto/management/commands/detectar_ausencias.py:629-630` antes de `abrir` | **SEM JUIZ próprio**, `app/core/juizes.py:182-345` | `app/colaboradores/tests/test_feriado_do_posto.py::NuloCobraTest::test_MORDE_posto_que_FECHA_nao_cobra_no_feriado:80` é funcional; **SEM CASO** sem CLT/CCT/L-NNN |
| Célula veta cobrança | `app/ponto/marcos.py::celula_veta_cobranca:122`, corpo:155-163: célula ausente→não veta; trabalha False→FOLGA; veredito em `VEREDITOS_QUE_VETAM`→veta | Emissor `app/ponto/management/commands/detectar_ausencias.py:632-634`; é leitura de campo, sem recalcular grade | **SEM JUIZ próprio**, `app/core/juizes.py:182-345` | **SEM CASO** [não verificado] |
| Limbo de folga cala dia, semana muda não cala | `app/chamados/services/limbo_folgas.py::pode_calar:161`, corpo:167-170; `::semana_muda:145`, corpo:155-158: nenhuma batida não retratada nos sete dias inclusivos | Emissor agrupa se pode calar e pai existe em `app/ponto/management/commands/detectar_ausencias.py:749-763`; caso contrário ressalva:764-774 | **SEM JUIZ próprio**, `app/core/juizes.py:182-345` | **SEM CASO** [não verificado] |

Os gatilhos que chegam à célula são save/delete de batida, ausência e save de justificativa: `app/ponto/signals.py:131-165`; recalculam os dias por `::_julgar_dias:85-93`. Batida aciona dia e véspera em `::_dias_batida:108-112`; ausência usa `dias_da_ausencia` em:115-128. Esses gatilhos não escrevem diretamente `status_local`: chegam ao cartório e deste ao emissor/portas, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1791-1793,1828-1831`.

O emissor que efetivamente pede o renascimento é `app/ponto/management/commands/detectar_ausencias.py::Command._abrir_chamado:571`: pede `ChamadoColaborador.abrir` em:694; se permaneceu encerrado devolve veto em:740-741; somente se vivo espelha novos marcos em:744-745. O espelho usa porta contexto em `::_espelhar_marcos:544`, grava:559-560 e acrescenta ocorrência `reaberto_por_marco` em:564-568. O caso L-110 da cobrança E→S é `app/chamados/tests/test_reabrir_pergunta_o_marco.py:12-14`, `::CobrancaDoDiaRenascePeloMarcoNovoTest::test_MORDE_a_cobranca_do_dia_renasce_pelo_marco_novo_e_cobra_a_S:57`.

## 14/11 12:06 · sessao e2117872ca12 · - · - · origem=Ronald (tmux)

C6 é leitor que recalcula a vida da pergunta: `pergunta_viva` chama `_pergunta_viva_calc`, com memo por estado/instante; não lê apenas `materializada_em` (`app/chamados/juizes.py:883-894`). Registro efetivo: `_C6 → chamados/juizes.py::pergunta_viva` (`app/core/juizes.py:282`).

| Decisão | Predicado efetivo / insumo | Fonte | Juiz | Caso pela regra |
|---|---|---|---|---|
| Projeção no instante | Carimbos posteriores a `agora` são ignorados; resposta futura perde texto; via futura é zerada salvo vias fora do fato | `app/chamados/juizes.py::projecao_temporal_da_pergunta:1387` | C6, registro `app/core/juizes.py:282` | SEM CASO qualificado localizado [não verificado] |
| Validação encerra | `validada is not None → False` | `app/chamados/juizes.py::_pergunta_viva_calc:1401` | C6 | SEM CASO qualificado localizado [não verificado] |
| Tranca cala | `dia_da_pergunta_trancado(p)` ou `carga.trancada(p) → False` | `app/chamados/juizes.py::_pergunta_viva_calc:1405` | C6/C20, registro `app/core/juizes.py:296` | L-051, “arquivado reversível por toque” em `app/docs/LEIS.md:92`; `app/chamados/tests/test_tranca_vivas_08.py::ToqueSoComAssinaturaDoAtoTest::test_RED_a_lavra_do_cartorio_com_usuario_da_request_NAO_e_toque:82`, `::test_contra_exemplo_lavra_via_admin_destranca:95`, `::test_contra_exemplo_registrar_log_com_acoes_destranca:101`; casos literais `:10-14`, asserts `:85-86`, `:99`, `:109` |
| Via vence resposta | Via em `VIAS_QUE_VENCEM_A_RESPOSTA` e materialização presente encerra | `app/chamados/juizes.py::_pergunta_viva_calc:1412` | C6 | SEM CASO qualificado localizado [não verificado] |
| Resposta mantém cobrança | Respondida + texto não vazio + ausência de veredito admin no dia: viva salvo concordância com luz | `app/chamados/juizes.py::_pergunta_viva_calc:1415-1429` | C6 | Testes comportamentais concretos em `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py::PerguntaRespondidaEMarcoAcesoTest:56-66`; só referem L-110, metarregra (`app/docs/LEIS.md:136`); SEM CASO substantivo identificado [não verificado] |
| Concordância com luz | Só hora/data_hora; exige marco aceso; duas pausas sem identificação → desconhecido; resposta e luz comparáveis; `classe_por_delta(...) == CLASSE_A` e veredito não acusa | `app/chamados/juizes.py::_resposta_concorda_com_a_luz:1342-1375` | Composição C6; registro C6 `app/core/juizes.py:282`; registro próprio dessa composição não localizado [não verificado] | Mesma ressalva: casos 19:02/19:06/sem luz/furo são literais e assertados (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:7-11,56-66`), porém L-110 não dita a tolerância/resultado (`app/docs/LEIS.md:136`) |
| Identidade encerra | Via em `vias_que_encerram_por_identidade` e materialização presente | `app/chamados/juizes.py::_pergunta_viva_calc:1437-1439` | C6 | SEM CASO qualificado localizado [não verificado] |
| Próprio marco encerra | Materialização presente e `fato_do_marco_no_espelho(p) is True` | `app/chamados/juizes.py::_pergunta_viva_calc:1444-1446` | C6 | SEM CASO qualificado localizado [não verificado] |
| Célula acusa/reabre leitura | `cel.veredito in VEREDITOS_ACUSAM → True` | `app/chamados/juizes.py::_pergunta_viva_calc:1448-1451` | C6 | SEM CASO qualificado localizado [não verificado] |
| Célula absolve até teto | Veredito não acusador encerra se não há marcos pendentes ou próprio marco acendeu | `app/chamados/juizes.py::_pergunta_viva_calc:1461-1463` | C6 | `test_o261_pergunta_viva_teto.py::PerguntaVivaExigeFatoEncerrado::test_RED_veredito_ATE_O_TETO_nao_mata_a_pergunta:52` e `::test_MORDE_com_o_turno_VENCIDO_a_celula_que_nao_acusa_mata_como_sempre:57` têm caso/resultado; norma citada é seção de CLAUDE, não CLT/CCT/L-NNN (`app/chamados/tests/test_o261_pergunta_viva_teto.py:3-14`): SEM CASO pelo critério estrito |
| Reapuração da falta | Dia do turno dentro da janela viva e `classificar_falta(...)[1] is not None → True` | `app/chamados/juizes.py::_pergunta_viva_calc:1464-1472` | C6; classificador registrado em `app/core/juizes.py` [linha não verificada] | SEM CASO qualificado localizado [não verificado] |
| Carimbo e testemunha | Sem materialização → viva; com materialização → inverso de `_pergunta_materializada_de_fato`; exceção → morta | `app/chamados/juizes.py::_pergunta_viva_calc:1476-1489` | C6 | SEM CASO qualificado localizado [não verificado] |

`fato_do_marco_no_espelho` casa motivo com marco DNA e lâmpadas `(hora,tipo)`; para pausas exige todas acesas; lâmpada/marco ausente resulta `None` (`app/chamados/juizes.py:941-971`). `_pergunta_materializada_de_fato` pode reapurar `classificar_falta` (`:679-705`), buscar batidas plantadas não retratadas/slot (`:752-761`), ou aceitar destino encerrado somente se falta não contradiz na janela viva (`:779-797`).

Leitores e efeitos:

| Leitor | O que efetivamente relê |
|---|---|
| Fio admin versus colaborador | Admin inclui perguntas C6 vivas em disputas fechadas; colaborador `so_janela` deixa fechadas fora: `app/chamados/services/fio.py:216-239`. |
| Card acionável | `tem_pergunta_viva = any(p.viva and not raiox.pergunta_sem_acao_possivel(...))`: `app/chamados/services/fio.py:283-285`; vivos vêm primeiro `:418-419`. |
| Tela colaborador | Botão enviar/reenviar por `respondida_em`, resposta anterior visível: `app/templates/chamados/partials/_card_atendimento.html:135-155`; correção/teto 2: `:190-198`. |
| Tela admin | Mostra fechamento/data/contador e deriva teto `reaberturas_admin >= 2`: `app/templates/chamados/partials/modal_fio.html:379-384`; reabrir questionário exibe número de respostas retiradas: `:367-373`. |
| Painel/badges | `qs_na_fila` exige `COBRANCA` e exclui `silenciado_ate > agora`: `app/chamados/services/painel.py:188-190`; abertos/em análise/ativos usam essa fila `:212-222`. |
| Vigia fechada/viva | Pré-filtro SQL + C6 efetivo, conta perguntas e disputas: `app/chamados/services/vigia_fio.py::viva_em_disputa_fechada:753-764`. |
| Vigia loop | Pares consecutivos de encerrado→vivo; pergunta `houve_fato_novo` entre os instantes: `app/chamados/services/vigia_loop.py::trocas_de_estado_em_loop:45-54`. |
| Pré-filtro recalculado SQL | `PerguntaDisputa.q_vivas` deriva resposta, identidade, via, veredito e pendência SQL; não representa todos degraus C6: `app/chamados/models.py:3403-3418`. |
| Portão chamado | Saída de vivo para terminal: pergunta ao plano de vivas; se não reatáveis recusa, se reatáveis reaponta antes de fechar: `app/chamados/models.py::_gate_pergunta_viva:417-445`. |
| Portão disputa | Recusa respostas sem veredito e perguntas vivas no fechamento ordinário: `app/chamados/models.py::DisputaSupervisao.fechar:1942-1959`; juiz registrado C7 `app/core/juizes.py:283`. |
| Portão folha | Retenção usa célula/veredito/protestos/pausa, não `status_local` nem `fechada_em`: `app/folha/export.py::_dias_sem_decisao:1264-1296`; `classificar_export` é K1/K2 registrado `app/core/juizes.py:319-320`. |

Casos das guardas da raiz:

- Ativo/cascade: `test_selo_dedup_premissa_viva.py::test_DESLIGADO_nao_reabre:128` e `::test_CASCADE_nao_reabre:137`, com corte comportamental nas linhas 15-20, mas sem CLT/CCT/L-NNN substantiva: SEM CASO qualificado [não verificado].
- Agrupamento: `test_agrupado_em.py::AgrupadoEmTest::test_pai_vivo_bloqueia_renascer_e_reabrir:32`, `::test_pai_fechado_libera:42`; mesma limitação [não verificado].
- Irmão: `test_selo_i4b_um_chamado_por_turno.py::test_MORDE_reabrir_com_irmao_vivo_NAO_reabre_e_entrega_o_fio:89`, asserts `:99-105`; mesma limitação [não verificado].
- Veto: `test_o261_veto_na_porta.py::PortaVetaNascimento::test_MORDE_renascer_automatico_vetado_reabrir_humano_passa:512`, `::test_MORDE_reabrir_automatico_vetado_deixa_trilha_e_o_dry_concorda:534`; L-111 aparece só na remoção de cópias (`:39`), não determina o veto: SEM CASO qualificado [não verificado].
- Premissa/fato novo: casos entrada/resultado explícitos em `test_reabrir_pergunta_a_premissa.py:7-12`, métodos `ReabrirPerguntaAPremissaTest:61,67,73,80,85`; e marco novo `test_reabrir_pergunta_o_marco.py::CobrancaDoDiaRenascePeloMarcoNovoTest::test_MORDE_a_cobranca_do_dia_renasce_pelo_marco_novo_e_cobra_a_S:57`; citam apenas L-110 como metarregra (`app/docs/LEIS.md:136`), não norma substantiva: SEM CASO sob interpretação estrita [não verificado].
- Órfã exportada: norma substantiva L-092 (`app/docs/LEIS.md:125`) e caso concreto `test_reabrir_orfas_enterradas.py::test_exportada_nao_reabre_e_vai_para_a_pauta:128`, asserts `:139-140`. Decisão código: `app/chamados/management/commands/reabrir_orfas_enterradas.py:93-103`; checa lâmpada apagada + C6 viva `:78-84`, janela histórica `:88-92`, pai/irmãs `:105-112`.

## 14/11 12:06 · sessao 1e766264e852 · - · - · origem=Ronald (tmux)

Casos com norma substantiva e resposta fixa identificados:

| Predicado | Norma escrita | Caso |
|---|---|---|
| Foto do 12x36 determina fase fora da foto | `app/docs/LEIS.md:73` — L-022 | `app/escala/tests/test_o37_foto_parcial.py::O37FotoParcialTest::test_RED_col418_os_dias_fora_da_foto_ALTERNAM:69`; `::test_MORDE_a_foto_em_dia_IMPAR_inverte_a_fase:87` |
| 12x36 sem fase retorna `None` | `app/docs/LEIS.md:73` — L-022 | `app/escala/tests/test_t1_ligado_por_padrao.py::T1LigadoPorPadraoTest::test_12x36_sem_fase_segue_none_e_nao_e_defeito_de_folga:55` |
| Batida retratada não entra na apuração | `app/docs/LEIS.md:76` — L-025 | `app/ponto/tests/test_batidas_apuraveis.py::BatidasApuraveisTest::test_retratada_fora_do_universo:32` |
| Teto temporal compara instante também no noturno de ontem | `app/docs/LEIS.md:100` — L-073 | `app/chamados/tests/test_pergunta_do_dia_em_curso.py::TetoPorInstanteParaToda_a_Data::test_MORDE_noturno_de_ontem_saida_das_06h_nao_vencida_as_0610:291`; `::test_MORDE_noturno_de_ontem_saida_vencida_as_0640:294` |
| Tranca não cria/carimba pergunta nova; leitura muda vitalidade | `app/docs/LEIS.md:92` — L-051 | `app/chamados/tests/test_c7_red_minimos.py::C7_08_CompetenciaTrancadaTest::test_RED_trancada_fecha_pela_porta_com_trilha_e_nao_carimba:329`; `::test_RED_o_C6_le_a_tranca_sem_escrever:361`; `::test_RED_destrancar_devolve_a_pergunta_viva_sozinha:369` |
| Lavra do cartório com usuário humano não equivale a toque do admin | `app/docs/LEIS.md:92`; regra aplicada transcrita em `app/chamados/tests/test_tranca_vivas_08.py:6` | `app/chamados/tests/test_tranca_vivas_08.py::ToqueSoComAssinaturaDoAtoTest::test_RED_a_lavra_do_cartorio_com_usuario_da_request_NAO_e_toque:82`; `::test_contra_exemplo_lavra_via_admin_destranca:95` |
| Ausência declarada deriva minutos da grade | `app/docs/LEIS.md:75` — L-024 | `app/chamados/tests/test_h06_falta_pela_grade.py::FaltaDeclaradaPelaRespostaLeAGrade::test_MORDE_6x1_de_7h_desconta_420:86`; `::test_MORDE_12x36_desconta_o_previsto_da_grade_660:90`; `::test_MORDE_meio_periodo_desconta_240:94` |

Não creditei como prova normativa inequívoca:

- Ausência pendente suspende cobrança: há casos fixos em `app/ponto/tests/test_ausencia_cobre_para.py:35`, mas o fundamento transcrito é aval sem identificação CLT/CCT/L-NNN (`app/ponto/tests/test_ausencia_cobre_para.py:3`); norma substantiva correspondente [não verificado].
- Ausência parcial cobra o resto: há casos fixos e aval em `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:4`, mas L-061 citada ali corresponde à leitura da lâmpada (`app/docs/LEIS.md:95`); norma CLT/CCT/L-NNN para a parcialidade [não verificado].
- Impedir nascimento/pergunta em exportada: testes citam L-092 (`app/chamados/tests/test_f1c4_3_emissores.py:158`; `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:65`), enquanto o texto L-113 responde expressamente que chamado pode nascer nessa competência (`app/docs/LEIS.md:153`). Qual corte vale [não verificado].
- Congelamento da célula: `app/escala/tests/test_celula_dia.py:68` verifica conservação de estado contra L-020 (`app/docs/LEIS.md:71`), porém a instrução exclui testes de “não mudou”; crédito como caso admissível [não verificado].

## 14/11 12:06 · sessao a55411207dc0 · - · - · origem=Ronald (tmux)

Casos que atendem ao critério estrito identificado:

| Decisão coberta | Caso pela regra | Regra e resposta verificáveis |
|---|---|---|
| Dia trancado fecha disputa sem carimbar pergunta nova | `app/chamados/tests/test_c7_red_minimos.py::C7_08_CompetenciaTrancadaTest::test_RED_trancada_fecha_pela_porta_com_trilha_e_nao_carimba:329` | L-051 escrita em `app/docs/LEIS.md:92`; caso em `app/chamados/tests/test_c7_red_minimos.py:319`; resultado explícito em `:343–350`. |
| Tranca mata pergunta por leitura; destranca devolve viva | Mesma classe, `test_RED_o_C6_le_a_tranca_sem_escrever:361` e `test_RED_destrancar_devolve_a_pergunta_viva_sozinha:369` | L-051 e interpretação do caso em `app/chamados/tests/test_c7_red_minimos.py:326`; respostas literais em `:364–375`. |
| Ausência parcial cobra restante; ausência integral/folga cala | `app/chamados/tests/test_emis_ausencia_parcial_cobra.py::JuizAusenciaParcialTest::test_RED_ausencia_parcial_com_trabalha_true_NAO_e_dia_sem_trabalho:40` e demais métodos `:43`, `:46`, `:50`, `:54` | L-061 explicitamente associada à leitura da ata em `:10`; casos e respostas em `:13–18`; asserts concretos em `:40–55`. |
| Emissores leem folga/ausência na ata | `app/chamados/tests/test_f1c4_3_emissores.py::EmissoresPerguntamAoJuizTest::test_fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta:89` | L-061 em `:6`; casos e respostas em `:20–29`. Há também materialização `:109`, cluster `:127`, intervalo `:149`. |
| Supra lê célula soberana para cobrança em dia sem trabalho | `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py::SupraPerguntaAoJuizTest::test_RED_celula_folga_com_ata_trabalho_e_dia_sem_trabalho_pelo_juiz:47` | L-061, caso e resposta em `:5–9`; assert do código de acusação em `:53`. |
| Teto por instante também no turno de ontem | `app/chamados/tests/test_pergunta_do_dia_em_curso.py::TetoPorInstanteParaToda_a_Data::test_MORDE_noturno_de_ontem_saida_das_06h_nao_vencida_as_0610:291` e `test_MORDE_noturno_de_ontem_saida_vencida_as_0640:294` | L-073 em `:213`; expectativa literal: 06:10 não pergunta; 06:40 pergunta `orfao_14h`, em `:291–295`. |
| Justificativa dispensa lâmpada do marco | `app/ponto/tests/test_ausencia_ata_dispensa.py::DispensaPorJustificativa::test_MORDE_dia_justificado_dispensa_o_marco:58` | L-061 e caso em `:50–55`; resposta lâmpada acesa/dispensada/n_missing zero em `:59–63`. Não prova diretamente fechamento da origem. |
| Justificado não vira furo na releitura | `app/ponto/tests/test_cobparc8_leitor_transporta_justificado.py::DiaJustificadoNaoEFuroTest::test_RED_dia_justificado_sem_batida_nao_e_furo_pelo_leitor:87` | L-061 em `:4`; cadastro e resposta em `:13–17`; resultado sem `FURO_*` em `:100`. |
| Comando pontual não reabre pergunta exportada e aponta pauta | `app/chamados/tests/test_reabrir_orfas_enterradas.py::ReabrirOrfasEnterradasTest::test_exportada_nao_reabre_e_vai_para_a_pauta:128` | L-092 e caso em `:1–11`; resultado em `:139–140`. Vigência de corte não decidida. |

**SEM CASO pelo critério estrito:**

- Aprovação de justificativa → encerra origem: `app/ponto/tests/test_justificativa_encerra_origem.py:6–8` usa o próprio coletor como fundamento; LEI-AKITA 13 é protocolo, sem CLT/CCT/L-NNN substantiva.
- Ausência aberta segura / decidida fecha: testes em `app/ponto/tests/test_ausencia_aberta_nao_fecha_chamado.py:39` e `app/ponto/tests/test_chamado_ausencia_fecha.py:45` conferem comportamento, mas fundamentação `:1–6` de ambos não apresenta CLT/CCT/L-NNN substantiva.
- C7 geral — muda viva, resposta sem veredito, vazia, motivo administrativo, desligamento: `app/chamados/tests/test_c7_red_minimos.py:128–317` tem casos por aval C7; só recorte da tranca `:319` apresenta L-051 substantiva.
- O168: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:1–15` fundamenta pelo aval/cascata; sem CLT/CCT/L-NNN substantiva.
- SLA: `app/chamados/tests/test_prazo_vencido_pelo_juiz.py:1–9` declara a soma do juiz; teste de paridade em `:51–62`.
- Reabrir pela premissa/fato novo: `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:7–12` cita L-110, que é protocolo; não apresenta a regra substantiva que decide aqueles resultados.
- Fechar pela premissa: `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:1–12` fundamenta pelos avais e pelo mesmo coletor, sem CLT/CCT/L-NNN substantiva.
- `AprovarExcecaoCLTTest` cita art.468 em `app/chamados/tests/test_views.py:284`, mas não explicita como a regra jurídica responde ao caso; há apenas expectativa de implementação em `:334–340`.

**Registro efetivo:**

- `_C3` registra somente `chamados/catalogo/premissa.py::premissa_morta`, em `app/core/juizes.py:279`; função real `app/chamados/catalogo/premissa.py:144–150`.
- `ChamadoColaborador.premissa_regularizada`, `_gate_premissa_do_fechamento`, `recusa_da_reabertura`, `cartorio.houve_fato_novo`, `lastro.marcos_acesos`, `lastro.marcos_inexistentes`, `cartorio.marcos_vencidos` e `marcos_com_teto`: **SEM JUIZ próprio** no registro `app/core/juizes.py:182–345`.
- `_C11` registra `reconciliar_por_ausencia`, em `app/core/juizes.py:287`; não registra `ponto.services.ausencia.fechar_chamados_ausencia_decidida`.
- `_C18`: `dia_sem_trabalho`, `app/core/juizes.py:294`; `_C19`: `tarefa_do_dia`, `:295`; `_C20`: `dia_da_pergunta_trancado`, `:296`.
- `dia_encerrado`: registrado por pergunta literal na família turno/marcos, `app/core/juizes.py:235`.
- Juízes sem id `_C`: pauta cadastro `app/core/juizes.py:297`, `respostas_sem_veredito` `:298`, `pode_executar_csf` `:299`.

L-110 exige indicar a regra substantiva aplicada ao cadastro/batidas; sua simples citação não certifica o caso: `app/docs/LEIS.md:136`.

## 14/11 12:02 · sessao eceeba4f7e5d · RMDIR-COM-GUARDA · verificador · origem=Code

**[bloqueia] A garantia irrestrita do rc ainda tem um contraexemplo: Bash em modo POSIX, arquivo do rc vazio e sinal tratado numa janela que força repetir um `wait` já concluído. O segundo `wait` pode devolver 127, que vira a saída da suíte.** O script não desliga esse modo; sua ativação no pouso é **[não verificado]**. (`bin/suite.sh:34`, `bin/suite.sh:358`, `bin/suite.sh:360`, `bin/suite.sh:364`)

1. **Caminhos do rc**

   **[fila] O caminho normal preserva o rc:** o subshell captura `_r`, tenta gravá-lo e executa `exit "$_r"`, mesmo se o `echo` falhar. Arquivo legível e não vazio prevalece; vazio ou ilegível usa `_RC_WAIT`. (`bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`)

   | Sequência | Resultado |
   |---|---|
   | Sinal após instalar a trap, antes de lançar o subshell: fica enfileirado; quando houver filho, o polling tenta entregá-lo. | O sinal não determina mais o rc de reserva. (`bin/suite.sh:333`, `bin/suite.sh:341`, `bin/suite.sh:347`, `bin/suite.sh:360`) |
   | Sinal antes ou durante o polling; trava termina com 9; arquivo escrito. | Lê 9 do arquivo. A marca antiga é zerada antes da espera final. (`bin/suite.sh:341`, `bin/suite.sh:358`, `bin/suite.sh:359`) |
   | Mesma sequência, mas escrita impedida. | Usa o rc da espera final; desapareceu a fabricação de 143 pelo nome TERM. (`bin/suite.sh:358`, `bin/suite.sh:360`) |
   | Sinal realmente interrompe o `wait` final. | A trap marca `_NO_WAIT`; o laço chama `wait` novamente. Com arquivo válido, seu conteúdo ainda prevalece. (`bin/suite.sh:333`, `bin/suite.sh:358`, `bin/suite.sh:359`) |
   | **[bloqueia]** Arquivo vazio; primeiro `wait` completo devolve 9; sinal chega antes do teste da marca; Bash POSIX. | Repete uma espera já consumida, obtém 127 e devolve 127. Também há falso positivo se o sinal chegar entre o zeramento e a entrada no `wait`. Ocorrência no pouso **[não verificado]**. (`bin/suite.sh:358`, `bin/suite.sh:360`, `bin/suite.sh:364`) |

   **[fila] Há outra divergência condicional:** arquivo já contém 9, mas um sinal externo mata o próprio subshell entre o `echo` e seu `exit`; a suíte prefere o arquivo, embora o status terminal do subshell seja outro. Esse evento não aparece no dado: **[não verificado]**. (`bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:364`)

2. **Término, 127 e identidade do processo**

   - **[fila] Pode não terminar sob sinais incessantes:** basta que toda tentativa receba uma marca antes do teste. O laço não limita tentativas. Uma sequência finita, por si, não demonstra travamento permanente. (`bin/suite.sh:333`, `bin/suite.sh:334`, `bin/suite.sh:335`, `bin/suite.sh:358`)
   - **[bloqueia] Pode devolver 127 por espera repetida em Bash POSIX**, conforme a sequência acima. Em Bash padrão, recolher o filho pelo mecanismo interno do shell não significa automaticamente esquecer seu status; não há outro `wait "$_PID_RUN"` nesse trecho. Modo/versão de produção **[não verificado]**. (`bin/suite.sh:358`)
   - **[fila] PID reciclado ameaça principalmente o polling anterior:** arquivo vazio mais `kill -0` verdadeiro para o número reaproveitado pode prolongar a espera; com fila pendente, `pkill -P` pode atingir filhos desse novo processo. Arquivo não vazio rompe esse polling. (`bin/suite.sh:344`, `bin/suite.sh:347`)
   - **[fila] Reciclagem numérica não faz `wait` obter status de qualquer processo do sistema:** ele depende da tabela de filhos/status do Bash. Para devolver o rc de outro filho, seria necessária substituição da identidade guardada, por exemplo por um novo filho com o mesmo número; isso não está demonstrado pelo dado: **[não verificado]**. Os pontos envolvidos são o armazenamento do PID e sua reutilização nas consultas. (`bin/suite.sh:342`, `bin/suite.sh:344`, `bin/suite.sh:358`)

3. **Leitores e escritores**

   **[fila] O mapa atual é:** (`bin/suite.sh:329`, `bin/suite.sh:333`, `bin/suite.sh:347`, `bin/suite.sh:358`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`)

   | Campo | Escritores | Leitores |
   |---|---|---|
   | `_NO_WAIT` | Inicialização `:329`; traps `:333–335`; zeramento `:358` | Teste em `:358` | [nao verificado]
   | `_FILA` | Inicialização `:329`; acréscimo nas traps `:333–335`; retirada da cabeça `:347` | Traps `:333–335`; teste e extração `:345–347` | [nao verificado]
   | `_RC_WAIT` | Captura de `$?` em `:358` | Reserva em `:360` |
   | `_RC_RUN` | Leitura do arquivo `:359`; reserva `:360` | Teste `:360`; aviso `:361`; saída `:364` | [nao verificado]

   **[fila] A busca em `bin/` não encontrou `_HOUVE` nem `_ULTIMO`.** A ausência é resultado da inspeção, sem uma linha individual para citar: **[não verificado por referência pontual]**. Os testes ainda leem e reescrevem textualmente âncoras de `_RC_RUN`, `_RC_WAIT` e `_FILA` para produzir mutantes. (`bin/tests/test_rm_com_guarda.sh:264`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:379`, `bin/tests/test_rm_com_guarda.sh:380`)

4. **Mordida do caso novo**

   - **[fila] Por leitura, morde dedc3e324 fora de root:** TERM executa `chmod 000` no arquivo temporário e encerra a trava com 9; o teste exige 9. O fallback antigo mostrado no delta escolheria 143 por TERM. Execução efetiva contra dedc3e324 **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:316`, `bin/tests/test_rm_com_guarda.sh:319`, `bin/tests/test_rm_com_guarda.sh:320`; delta fornecido)
   - **[fila] Não roda sob root:** o cenário inteiro está condicionado a UID diferente de zero; o sucesso geral do caso 6 não explicita essa omissão. (`bin/tests/test_rm_com_guarda.sh:314`, `bin/tests/test_rm_com_guarda.sh:322`, `bin/tests/test_rm_com_guarda.sh:386`)
   - **[fila] Há mutante que pode passar:** substituir o laço final por um único `wait`, mantendo o fallback novo. O cenário pode entregar TERM durante o polling e encontrar o run já terminado na espera final; `sleep 1` não sincroniza essa janela. Execução desse mutante **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:319`, `bin/suite.sh:344`, `bin/suite.sh:358`, `bin/suite.sh:360`)
   - **[fila] A ressalva de root alcança o selo anterior:** o caso 5 também usa `chmod 000`, sem excluir root; se a escrita continuar permitida, o mutante do fallback fixo em 1 não é exercido e pode provocar a própria reprovação do selo. Execução sob root **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:274`)

5. **Classes anteriores no delta**

   - **[fila] Segundo escritor, derivação paralela e ordem de trava:** não identifiquei reintrodução nas alterações fornecidas. Os alvos continuam vindo da porta, e as duas limpezas permanecem no comando executado pela trava. (`bin/suite.sh:112`, `bin/suite.sh:117`, `bin/suite.sh:293`, `bin/suite.sh:296`, `bin/suite.sh:317`, `bin/suite.sh:318`)
   - **[fila] Selo sem mordida:** o cenário novo distingue o fallback antigo fora de root, mas deixa sem cobertura garantida a interrupção da espera final e o cenário sob root. (`bin/tests/test_rm_com_guarda.sh:314`, `bin/tests/test_rm_com_guarda.sh:319`, `bin/suite.sh:358`)
   - **[bloqueia] Campo com significado ampliado:** `_NO_WAIT` registra “chegou sinal desde a marca”, mas seu leitor invalida também um `wait` que terminou corretamente. Essa diferença entre chegada e interrupção produz o contraexemplo POSIX; não é necessário PID reciclado. (`bin/suite.sh:331`, `bin/suite.sh:333`, `bin/suite.sh:358`, `bin/suite.sh:360`)

## 14/11 12:00 · sessao 62343981bf41 · - · - · origem=Ronald (tmux)

- **Fluxo normal:** não há divergência demonstrada. `_run` fornece `_r`; o subshell tenta gravá-lo e sai com o mesmo `_r`, mesmo quando o `echo` falha. Arquivo legível não vazio prevalece; vazio/ilegível aciona `_RC_WAIT`. `bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`.

- **Sinal anterior ao laço ou recebido no polling:** fica em `_FILA`, segue ao filho e não define mais o rc de reserva. `_NO_WAIT` é zerado ao entrar na espera final; portanto um sinal já tratado não transforma rc 9 em 143. `bin/suite.sh:333`, `bin/suite.sh:344`, `bin/suite.sh:347`, `bin/suite.sh:358`.

- **Sinal durante a espera final:** a marca força outra chamada a `wait`; sinal depois do retorno de um `wait` completo, antes do teste da marca, também força repetição. Esse segundo caso é decisivo no Bash POSIX. `bin/suite.sh:333`, `bin/suite.sh:358`.

- **[bloqueia], condicional ao modo POSIX:** conforme a sonda informada pelo pai, nesse modo `wait "$p"` completo seguido de novo `wait "$p"` retorna respectivamente 9 e 127 [não verificado na cópia]. Sequência: arquivo do rc vazio; primeiro `wait` devolve 9; sinal chega antes do teste e marca `_NO_WAIT`; segundo `wait` devolve 127; fallback devolve 127. O script não desliga POSIX. `bin/suite.sh:34`, `bin/suite.sh:358`, `bin/suite.sh:360`. Ativação desse modo em produção: **[não verificado]**.

- **Bash padrão:** experimento local apenas em memória obteve `wait` repetido 9/9 e `wait` interrompido por TERM 143 seguido de espera 9 [não verificado na cópia]. Recolhimento do filho pelo shell, por si, não demonstra perda do status; não existe outro `wait` desse PID neste trecho. `bin/suite.sh:358`.

- **[fila], sinais incessantes:** se cada iteração receber sinal antes de testar a marca, o laço final pode continuar indefinidamente, mesmo com o filho terminado. Uma quantidade finita de sinais permite sair quando houver iteração sem marca, ressalvada a questão POSIX acima. `bin/suite.sh:333`, `bin/suite.sh:358`.

- **[fila], PID reciclado e arquivo vazio:** o polling pode continuar enquanto o número reaproveitado responder a `kill -0`; com fila pendente, pode tentar sinalizar filhos desse outro processo. Arquivo não vazio rompe esse polling. `bin/suite.sh:343`, `bin/suite.sh:344`, `bin/suite.sh:347`. O `wait` não passa a obter status arbitrário de um processo não filho apenas pelo reaproveitamento numérico [não verificado na cópia]. Substituição/perda do cache Bash por outro filho com o mesmo PID exige condições adicionais não demonstradas [não verificado].

- **Mapa:** `_FILA`: inicialização 329; leitura/escrita nas traps 333–335; leitura 345–347; remoção da cabeça 347. `_NO_WAIT`: inicialização 329; escrita nas traps 333–335; zeramento e leitura 358. `_RC_WAIT`: escrita 358, leitura 360. `_RC_RUN`: escrita 359/360, leitura 360/361/364. `bin/suite.sh:329`, `bin/suite.sh:333`, `bin/suite.sh:345`, `bin/suite.sh:347`, `bin/suite.sh:358`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:361`, `bin/suite.sh:364`.

- Busca de `bin/` não encontrou `_HOUVE` nem `_ULTIMO`; os símbolos atuais aparecem nos lugares acima e nos leitores/escritores textuais dos mutantes. `bin/tests/test_rm_com_guarda.sh:264`, `bin/tests/test_rm_com_guarda.sh:265`, `bin/tests/test_rm_com_guarda.sh:272`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:379`, `bin/tests/test_rm_com_guarda.sh:380`.

- Divergência entre arquivo já escrito e status do subshell também seria possível se um agente externo matasse o subshell entre o `echo` e `exit`, ou adulterasse o arquivo; esses eventos não constam dos dados. **[fila] [não verificado]**, pontos envolvidos: `bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:364`.

## 14/11 11:59 · sessao 4539784bb97f · - · - · origem=Ronald (tmux)

- **[fila] O caso novo morde o defeito anterior fora de root:** TERM faz a trava executar `chmod 000 .../tmp.*` e sair 9; a asserção exige 9. O subshell preserva esse 9 apesar da falha do `echo`. No delta fornecido, dedc3e324 escolheria 143 pelo nome TERM quando o arquivo estivesse vazio. Referências: `bin/tests/test_rm_com_guarda.sh:314`, `:316`, `:319`, `:320`; `bin/suite.sh:341`; comportamento da versão anterior deduzido do anexo, execução **[não verificado]**.

- **[fila] O caso novo não roda sob root:** todo o cenário está dentro de `if [ "$(id -u)" != 0 ]`. Não há aviso específico de cenário pulado antes do texto de sucesso do caso 6. Referências: `bin/tests/test_rm_com_guarda.sh:314`, `:322`, `:386`.

- **[fila] Não força sinal durante o `wait` final:** usa somente `sleep 1; kill -TERM`, sem marco que confirme a entrada em `bin/suite.sh:358`. Um mutante que substitua o laço de reespera por um único `wait`, mantendo o fallback novo, pode passar quando TERM for tratado no laço de polling e o run terminar antes da espera final. Referências: `bin/tests/test_rm_com_guarda.sh:318`, `:319`; `bin/suite.sh:344`, `:352`, `:358`, `:360`. Execução do mutante **[não verificado]**.

- **[fila] Não há mutante novo que restaure o fallback por nome do sinal:** o acréscimo é apenas cenário funcional; os mutantes existentes do caso 6 cobrem caminho físico, erro de rmdir, limpeza após sinal, nome repassado e sobrescrita da fila. Referências: `bin/tests/test_rm_com_guarda.sh:312`, `:345`, `:353`, `:362`, `:370`, `:378`.

- **[fila] Ressalva já existente para root no selo inteiro:** o caso 5 tenta impedir escrita com chmod, mas não pula root; seu mutante troca somente o fallback por 1. Se root escrever normalmente, esse fallback não é exercido e o mutante pode passar, provocando `_falha`. Referências: `bin/tests/test_rm_com_guarda.sh:130`, `:231`, `:233`, `:271`, `:273`, `:274`. Execução sob root **[não verificado]**.

- **[fila] Classes do delta:** nenhuma segunda origem de limpeza, derivação paralela de alvos ou mudança na ordem da trava aparece nas alterações fornecidas; o código continua derivando alvos da porta e executando `_run` pela trava. Referências: `bin/suite.sh:293`, `:296`, `:317`, `:318`; alcance exato do histórico além do delta fornecido **[não verificado]**. A limitação concreta do selo é a cobertura omitida sob root e a ausência de sincronização com a espera final, acima.

## 14/11 11:49 · sessao 7df6ebc9fb89 · FRAC5 · sonda · origem=Code

Não reexecutei: não localizei o anexo `sonda.py` na cópia. [nao verificado]

Transcrição da saída fornecida: [nao verificado]

```text
universo_agendado_nao_iniciado 5
recusados_total 0
fracao_menor_que_5_dias (literal dias<5; o juiz do main ainda nao tem a 2a metade) 0 []
ERROS 0
```

Não há linhas `RECUSA`, contagens por recusa ou PKs no trecho fornecido. [nao verificado]

Não há `Traceback` no trecho fornecido; sua ocorrência fora desse trecho é [nao verificado].

## 14/11 11:49 · sessao 87a073847532 · RMDIR-COM-GUARDA · verificador · origem=Code

**[bloqueia] A condição (1), lida como “sempre devolver o rc do run”, não está integralmente cumprida:** sem conteúdo em `_RCF` e havendo sinal registrado, o código ignora `_RC_WAIT` e fabrica o retorno pelo último sinal. A correção específica do rc do `echo` está presente. — [bin/suite.sh:341](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/suite.sh:341), `bin/suite.sh:354`, `bin/suite.sh:358`.

Esta é uma verificação estática. Execução dos REDs, resultados de suite/opus e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Condição (1): correção implementada; garantia absoluta incompleta.** O subshell captura `_r` imediatamente após `_run` e termina com `exit "$_r"`; falha do `echo` deixa de determinar seu status. Sem sinal e sem arquivo legível, o pai usa `_RC_WAIT`. — `bin/suite.sh:341`, `bin/suite.sh:354`, `bin/suite.sh:357`.

   O RED faz o docker falso aplicar `chmod 000` ao arquivo e exige retorno 75; o mutante substitui o fallback por 1. Pelo delta fornecido, o caso falharia no `0fae8f7f8`: falhando a escrita, o subshell antigo terminaria com o status do `echo`. Essa comparação é estrutural; execução contra o hash anterior **[nao verificado]**. — [bin/tests/test_rm_com_guarda.sh:130](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/tests/test_rm_com_guarda.sh:130), `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:235`, `bin/tests/test_rm_com_guarda.sh:273`.

   **[bloqueia] Falta o cruzamento sinal + arquivo indisponível:** se `_RC_WAIT` for 9, `_RCF` vier vazio e `_ULTIMO=TERM`, a suite devolve 143. Os testes atuais exercitam retorno 9 após TERM e arquivo indisponível separadamente; não cobrem essa combinação. — `bin/suite.sh:355`, `bin/suite.sh:358`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:309`.

2. **Condição (2): cumpre para os seis alvos entregues pela porta.** A limpeza percorre os argumentos derivados da montagem; a porta escreve staticfiles, três caches, logs e media. O RED cria os seis diretórios vazios, conta os ausentes e exige exatamente 6. — `bin/suite.sh:60`, `bin/suite.sh:296`, [bin/arvore_do_push.sh:55](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/arvore_do_push.sh:55), `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`, `bin/tests/test_rm_com_guarda.sh:153`, `bin/tests/test_rm_com_guarda.sh:157`.

   **O RED tem mordida específica:** o mutante restaura a lista antiga de três. Pelo delta, no `0fae8f7f8` sobrariam os três caches e a contagem seria 3. Execução histórica **[nao verificado]**. — `bin/tests/test_rm_com_guarda.sh:211`, `bin/tests/test_rm_com_guarda.sh:213`, `bin/tests/test_rm_com_guarda.sh:214`.

   **[fila] “Sempre seis” exige qualificação:** staticfiles só é emitido quando existe na raiz da própria porta; sem esse diretório, ela entrega cinco. A fixture garante staticfiles preenchido, portanto testa efetivamente seis, mas não prova uma saída invariável da porta. — `bin/arvore_do_push.sh:50`, `bin/arvore_do_push.sh:55`, `bin/tests/test_rm_com_guarda.sh:114`, `bin/tests/test_rm_com_guarda.sh:115`.

3. **Condição (3): cumpre no caminho normal de dois `--dir`.** Fora da trava há apenas `--so-confere`, que retorna antes do laço de remoção. `_run` chama a trava, que adquire o flock antes de executar `--na-vez`; ambas as limpezas ficam nesse comando. — `bin/suite.sh:58`, `bin/suite.sh:289`, `bin/suite.sh:317`, `bin/suite.sh:318`, `bin/suite.sh:112`, `bin/suite.sh:117`, [bin/trava_teste.sh:35](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/trava_teste.sh:35), `bin/trava_teste.sh:42`.

   **O RED tem mordida específica:** a trava falsa usa flock; o primeiro docker falso mantém `logs`, o segundo `--dir` é lançado durante essa espera e o teste acusa seu desaparecimento. O mutante recoloca a limpeza fora da trava. Pelo delta, a limpeza prévia do `0fae8f7f8` também removeria esse diretório durante a sobreposição. Execução histórica **[nao verificado]**. — `bin/tests/test_rm_com_guarda.sh:120`, `bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245`, `bin/tests/test_rm_com_guarda.sh:250`, `bin/tests/test_rm_com_guarda.sh:281`.

   **[fila] O RED comprova preservação do diretório com docker falso; bind real permanece [nao verificado].** A sobreposição depende de quatro segundos e não há confirmação de que o segundo processo alcançou o ponto relevante dentro dessa janela. — `bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245`, `bin/tests/test_rm_com_guarda.sh:248`.

**a) Quem escreve e quem lê**

| Objeto | Escritor | Leitor |
|---|---|---|
| Nomes da montagem | `arvore_do_push.sh --montagem`, nos `printf` | A suite recebe `_m` e tokeniza `MONTAGEM`. — `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`, `bin/suite.sh:293`, `bin/suite.sh:295`. |
| `_alvos_da_montagem` | Extrai destinos de `-v` e `--tmpfs`; escreve sua saída | `ALVOS` e `_alvos_da_porta`. — `bin/suite.sh:81`, `bin/suite.sh:88`, `bin/suite.sh:92`, `bin/suite.sh:296`. |
| `_alvos_da_porta` | Chama a porta e reutiliza a mesma extração | Ramo `--limpa-montagens`, por `_al`/`_AL`. — `bin/suite.sh:90`, `bin/suite.sh:98`. |
| `ALVOS` | Inicializado vazio; preenchido pela extração da montagem | `_run` transporta; `--na-vez` divide em `_NV_AL`; pré/pós consomem esse array. — `bin/suite.sh:283`, `bin/suite.sh:296`, `bin/suite.sh:318`, `bin/suite.sh:109`, `bin/suite.sh:112`, `bin/suite.sh:117`. |
| `_RCF` | Pai cria o caminho; subshell escreve o conteúdo | Pai consulta tamanho, lê e remove. — `bin/suite.sh:338`, `bin/suite.sh:341`, `bin/suite.sh:344`, `bin/suite.sh:355`. |
| `_NV_RC` | Uma atribuição imediatamente após `"$@"` | Decide pós-limpeza e fornece o retorno. — `bin/suite.sh:115`, `bin/suite.sh:116`, `bin/suite.sh:120`. |

Não identifiquei segundo escritor **da lista usada pela limpeza normal**: ela recebe argumentos derivados da montagem. Os nomes repetidos no teste constituem o oráculo e o mutante, não outra lista operacional. — `bin/suite.sh:60`, `bin/suite.sh:296`, `bin/tests/test_rm_com_guarda.sh:153`, `bin/tests/test_rm_com_guarda.sh:213`.

**[fila] A exclusividade não é imposta à interface:** uma invocação direta de `--na-vez` fornece sua própria lista, sem consultar a porta. — `bin/suite.sh:108`, `bin/suite.sh:109`.

**b) Entradas de `--na-vez`**

| Entrada | Chega a `rmdir`? |
|---|---|
| Caminho absoluto ou fora de `app/` | Recusado pelo primeiro `case`. — `bin/suite.sh:61`. |
| Barra adicional, como `app/a/b` | Recusado pelo segundo `case`. — `bin/suite.sh:62`. |
| Qualquer `..` | Recusado, inclusive dentro do nome. — `bin/suite.sh:62`. |
| `app/` | Recusado por faltar o primeiro caractere do nome. — `bin/suite.sh:61`. |
| Lista vazia | Nenhum alvo é percorrido; o comando continua. — `bin/suite.sh:109`, `bin/suite.sh:115`. |
| Symlink final | Ignorado por `! -L`. — `bin/suite.sh:67`. |
| Symlink intermediário que saia da cópia | Barrado pela comparação física. — `bin/suite.sh:70`, `bin/suite.sh:71`. |

**[fila] Nome com espaço perde identidade:** `"app/foo bar"` vira `app/foo` e `bar`; se `app/foo` for diretório físico vazio, pode ser removido. `bar` é recusado com rc 1, que não impede o comando. Além disso, `app/[A-Za-z0-9._-]*` restringe apenas o primeiro caractere após a barra; o `*` não repete a classe. — `bin/suite.sh:109`, `bin/suite.sh:61`, `bin/suite.sh:73`, `bin/suite.sh:113`, `bin/suite.sh:115`.

**Rc 2 da limpeza anterior impede, sim, o comando.** Isso cobre cópia inválida e árvore viva; alvo malformado recebe rc 1, não 2. — `bin/suite.sh:53`, `bin/suite.sh:56`, `bin/suite.sh:61`, `bin/suite.sh:112`, `bin/suite.sh:113`.

**c) Outros caminhos de rc**

- **[fila] Trava ocupada:** retorna 75 antes de executar `--na-vez` ou docker. É “não executou”, não rc de um run realizado. — `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`.
- **[fila] Docker ausente:** se alcançar a invocação, o status de comando ausente é 127 e será preservado. Pode haver recusa anterior com 1 em `teste_envfile`, que também pode tentar docker. Disponibilidade real **[nao verificado]**. — `bin/suite.sh:319`, `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/suite.sh:276`, `bin/recursos.sh:122`.
- **[fila] Sinal na cadeia real:** o repasse alcança a trava, filha direta do subshell; ela não implementa repasse explícito aos descendentes. Os REDs usam travas falsas com handlers próprios. Portanto, rc da trava e rc do docker não são demonstrados como equivalentes sob sinal. — `bin/suite.sh:347`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:314`.

**d) Classes de reprovação**

- **Segundo escritor / derivação paralela:** não demonstrados no caminho normal; os dois consumidores reutilizam `_alvos_da_montagem`. A lista livre de `--na-vez` permanece **[fila]**. — `bin/suite.sh:92`, `bin/suite.sh:296`, `bin/suite.sh:109`.
- **[fila] Selo dependente de permissão:** o RED de `_RCF` não confirma que `chmod 000` impediu o acesso nem exclui root. Se a injeção não funcionar, o mutante sobrevive e o próprio selo acusa falta de mordida; não é verde silencioso. — `bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:235`, `bin/tests/test_rm_com_guarda.sh:274`.
- **[fila] Ordem de trava nas portas avulsas:** `--limpa-montagens` limpa diretamente; `--na-vez` também não verifica posse da trava. Isso não invalida a ordem comprovada para dois `--dir`, mas impede afirmar “toda limpeza é travada”. — `bin/suite.sh:94`, `bin/suite.sh:99`, `bin/suite.sh:108`, `bin/suite.sh:112`.
- **[fila] Campo com dois significados:** `ALVOS` representa uma coleção, mas é transportado como texto separado por espaços, permitindo a perda de identidade descrita acima. — `bin/suite.sh:296`, `bin/suite.sh:109`.

A identificação dos **quatro `[fila]` originais**, seu registro em linhas de fila e a conclusão posterior de suite + um opus permanecem **[nao verificado]**. [nao verificado]

## 14/11 11:47 · sessao 57ec1f874399 · - · - · origem=Ronald (tmux)

- `--na-vez` recusa barra adicional e qualquer `..` antes de `rmdir`; `app/` também não passa. Lista vazia produz zero alvos e permite o comando. Referências: `bin/suite.sh:61`, `bin/suite.sh:62`, `bin/suite.sh:109`, `bin/suite.sh:115`.
- [fila] Nome com espaço é ambíguo: `app/foo bar` vira dois tokens; pode remover `app/foo`, rejeitar `bar` com rc 1 e ainda executar o comando. O padrão `app/[A-Za-z0-9._-]*` restringe somente o primeiro caractere do nome; o `*` seguinte admite outros caracteres. Referências: `bin/suite.sh:61`, `bin/suite.sh:73`, `bin/suite.sh:109`, `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:115`.
- Symlink final é ignorado; symlink intermediário que saia da cópia falha na comparação física. Recusa rc 2 da limpeza anterior impede o comando. Referências: `bin/suite.sh:67`, `bin/suite.sh:70`, `bin/suite.sh:71`, `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:115`.
- As duas limpezas do caminho normal `--dir` estão dentro da trava: esta prende fd 9 antes de executar `--na-vez`, que limpa antes/depois do comando. Referências: `bin/trava_teste.sh:35`, `bin/trava_teste.sh:36`, `bin/trava_teste.sh:42`, `bin/suite.sh:317`, `bin/suite.sh:318`, `bin/suite.sh:112`, `bin/suite.sh:117`.
- [fila] Isso não vale para toda entrada pública: `--limpa-montagens` chama a limpeza diretamente; `--na-vez` também não verifica posse da trava. Referências: `bin/suite.sh:94`, `bin/suite.sh:99`, `bin/suite.sh:108`, `bin/suite.sh:112`.
- O caminho normal preserva o rc do comando, inclusive se o `echo` falhar; a mudança relevante é `exit "$_r"` no subshell. Referências: `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/trava_teste.sh:42`, `bin/suite.sh:341`, `bin/suite.sh:354`, `bin/suite.sh:357`.
- [fila] Há exceção explícita a “sempre rc do run”: sem conteúdo em `_RCF` e havendo sinal, o código ignora `_RC_WAIT` e devolve 143/130/129 pelo último sinal, independentemente do rc obtido. Os REDs de sinal e arquivo indisponível são separados. Referências: `bin/suite.sh:355`, `bin/suite.sh:356`, `bin/suite.sh:357`, `bin/suite.sh:358`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:317`.
- rc 75 pode vir da trava sem executar o comando: `bin/trava_teste.sh:35`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`. Docker ausente no ponto da invocação resulta em status de comando ausente, preservado pelo encadeamento; a existência/disponibilidade real do Docker é [nao verificado]. Referências: `bin/suite.sh:319`, `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/suite.sh:341`.
- Sinais são enviados ao filho direto do subshell, a trava; a trava real não implementa repasse explícito aos descendentes. Os REDs verificam travas falsas que recebem TERM/HUP e escolhem seus próprios rc. Referências: `bin/suite.sh:347`, `bin/trava_teste.sh:35`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:314`.
- `rc >= 128` evita a limpeza posterior, mas a chamada seguinte volta a fazer a limpeza anterior; sobrevivência efetiva de container/bind após sinal é [nao verificado]. Referências: `bin/suite.sh:112`, `bin/suite.sh:116`, `bin/suite.sh:117`.

## 14/11 11:46 · sessao 558dff52491e · - · - · origem=Ronald (tmux)

- **Escritor da montagem:** os nomes saem de `bin/arvore_do_push.sh:55` e `bin/arvore_do_push.sh:80-81`; o staticfiles é condicional à existência do diretório da própria porta (`bin/arvore_do_push.sh:50`). Portanto, “sempre seis” exige essa condição; sem ela são cinco (`bin/arvore_do_push.sh:50-56`, `bin/arvore_do_push.sh:80-82`).

- **Leitura/derivação na suite:** `_m` recebe a porta (`bin/suite.sh:293`), `MONTAGEM` tokeniza (`bin/suite.sh:295`), `_alvos_da_montagem` extrai destinos de `-v`/`--tmpfs` (`bin/suite.sh:81-88`) e escreve o valor derivado de `ALVOS` (`bin/suite.sh:296`), inicialmente vazio (`bin/suite.sh:283`). `_run` o transporta (`bin/suite.sh:318`); `--na-vez` o divide em `_NV_AL` e ambas as limpezas o leem (`bin/suite.sh:109-117`). `_alvos_da_porta` chama a mesma extração para a porta avulsa; não há implementação paralela desse cálculo nesses dois caminhos (`bin/suite.sh:90-99`).

- **Segundo escritor:** nenhuma lista de nomes de limpeza está repetida no caminho normal da suite: a função itera os argumentos (`bin/suite.sh:60`), que vêm da montagem (`bin/suite.sh:296`). Os seis nomes explícitos do selo são o oráculo independente e o mutante de três nomes (`bin/tests/test_rm_com_guarda.sh:153-156`, `bin/tests/test_rm_com_guarda.sh:210-217`). Há listas de logs/media/staticfiles fora da porta, por exemplo a sonda de montagem `:ro`, que declara outro escopo (`bin/sonda_leitura.sh:197-210`); chamá-las de segundo escritor da limpeza da suite não se sustenta nessas linhas.

- **[fila] Interface aceita outro produtor:** `--na-vez` aceita diretamente a string de alvos fornecida pelo chamador, sem consultar a porta nem adquirir trava (`bin/suite.sh:108-120`). O chamador normal a põe sob `trava_teste.sh` (`bin/suite.sh:317-318`), mas exclusividade da fonte para todas as invocações diretas é **[nao verificado]**.

- **Arquivo de rc:** o pai cria e guarda o caminho em `_RCF` (`bin/suite.sh:338`); o subshell escreve o conteúdo pelo único `echo` (`bin/suite.sh:341`); o pai lê tamanho no laço (`bin/suite.sh:344`), lê conteúdo com `cat` e remove (`bin/suite.sh:355`). Em seguida escolhe a reserva somente se o conteúdo vier vazio (`bin/suite.sh:356-359`). `_NV_RC` recebe uma vez o status de `"$@"`, serve de guarda da limpeza posterior e é devolvido (`bin/suite.sh:115-120`).

- **[fila] Campo delimitado por espaço:** `ALVOS` é serializado com espaços (`bin/suite.sh:296`) e desserializado com `read -a` (`bin/suite.sh:109`); a própria montagem também é texto tokenizado por espaços (`bin/suite.sh:295`, `bin/suite.sh:92`). Um nome com espaço perde sua identidade nesses passos; isso não constitui saída de diretório, mas impede tratar a lista como uma coleção geral de nomes (`bin/suite.sh:61-70`).

- A existência e o conteúdo das quatro linhas de fila exigidas pelo aval permanecem **[nao verificado]**; não há referência delas no dado fornecido, além da exigência de registrá-las **[nao verificado]**.

## 14/11 11:46 · sessao 3ab108696133 · - · - · origem=Ronald (tmux)

- Condição (1): o subshell agora termina com `_r`, preservando rc do `_run` mesmo se `echo` falhar; sem sinal, o fallback usa `_RC_WAIT` (`bin/suite.sh:341`, `bin/suite.sh:354-358`). O RED injeta `chmod 000` e exige rc 75 (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233-235`); o mutante força fallback 1 (`bin/tests/test_rm_com_guarda.sh:271-275`). No código anterior mostrado pelo delta, falha de escrita deixaria o subshell com rc do `echo`, portanto esse caso falharia com 1 em vez de 75 [nao verificado por execução].

- **[fila] Mordida do RC depende de permissão:** esse caso não confirma que a escrita/leitura de `_RCF` efetivamente falhou e não exclui root (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233-235`). Para outro teste baseado em chmod, o selo explicitamente exclui root (`bin/tests/test_rm_com_guarda.sh:299`, `bin/tests/test_rm_com_guarda.sh:345`). Se chmod não bloquear, o mutante de fallback sobreviverá, e o próprio selo ficará vermelho pela falta de mordida (`bin/tests/test_rm_com_guarda.sh:274`).

- Condição (2): há RED comportamental que cria seis diretórios, conta seis ausentes e exige exatamente 6 (`bin/tests/test_rm_com_guarda.sh:153-157`). O mutante troca a derivação pela lista antiga de três (`bin/tests/test_rm_com_guarda.sh:211-215`). Na versão anterior do delta, os três caches ficariam; a contagem seria 3 [nao verificado por execução].

- Condição (3): o caminho normal chama `trava_teste.sh` e só então `--na-vez`; pre/post estão nesse modo (`bin/suite.sh:317-318`, `bin/suite.sh:112-118`); a trava real adquire flock antes de executar o comando (`bin/trava_teste.sh:35-42`). O RED usa flock real numa trava falsa, segura o primeiro run com `logs` e lança segundo `--dir`, exigindo ausência de `montagem.arrancada` (`bin/tests/test_rm_com_guarda.sh:120`, `bin/tests/test_rm_com_guarda.sh:131-133`, `bin/tests/test_rm_com_guarda.sh:245-250`). O mutante recoloca limpeza fora da trava (`bin/tests/test_rm_com_guarda.sh:279-283`); a limpeza anterior do delta também removeria `logs` durante essa janela [nao verificado por execução].

- **[fila] Concorrência tem janela temporal:** o primeiro run dorme quatro segundos, e o teste não confirma que o segundo chegou à limpeza antes de essa janela acabar (`bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245-250`). O teste detecta o mutante sob sobreposição; a sobreposição numa execução concreta está [nao verificado].

- **[fila] Guarda da viva pode não ser exercida:** o caso e seu mutante dependem da existência da viva e de nenhum dos seis alvos estar vazio; caso contrário imprimem aviso e seguem (`bin/tests/test_rm_com_guarda.sh:170-185`, `bin/tests/test_rm_com_guarda.sh:192-197`). Isso pode deixar o mutante sem mordida; o próprio selo o acusa como falha (`bin/tests/test_rm_com_guarda.sh:196`).

## 14/11 11:42 · sessao 758c610315a2 · JANELA-NAO-SEGURA-POUSO · verificador · origem=Code

1. **[fila] A faixa está correta para todos os dias: 10.080 pares dia/minuto conferidos em memória, sem divergência.** `proibido()` retorna **0 quando barra**; `--janela` retorna **1 quando barra**. ([bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55), [modo horário:75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75))

   | Dia | HHMM | Guarda | `proxima()` |
   |---|---|---|---|
   | Segunda a domingo | `0000`–`0559` | PROIBIDA | hoje às 06:00 |
   | Segunda a domingo | `0600`–`2319` | PERMITIDA | agora |
   | Segunda a domingo | `2320`–`2359` | PROIBIDA | amanhã às 06:00 |

   **[fila] Bordas:** `0000` e `2359` barram; `0600` libera; `2320` barra inclusivamente, embora a mensagem diga “depois das 23:20”. Zeros à esquerda, inclusive `0008`, `0559`, `0600`, `0808` e `0909`, não causaram erro octal: as comparações usam `[ ... -ge/-lt ... ]`. Não encontrei contraexemplo entre horários HHMM válidos. ([bin/janela_auth.sh:58](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:58), [próxima abertura:63](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:63))

2. **[fila] Escritor e leitores da regra:**

   - A decisão horária é escrita em `proibido`; a próxima abertura, em `proxima`. ([bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55), [63](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:63))
   - A guarda lê os alvos declarados em `auth_sitios.txt` e consulta a decisão nos modos horário e deploy. ([bin/janela_auth.sh:84](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:84), [75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75), [107](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:107))
   - O deploy chama a guarda ou a atravessa quando recebe motivo não vazio; o selo chama a guarda diretamente. ([bin/deploy.sh:144](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:144), [bin/tests/test_janela_auth.sh:41](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:41))

   **[fila] Prosa velha ainda afirmativa:** a lista de alvos continua proibindo sexta 18h+/fim de semana; O4 no BACKLOG conserva a mesma formulação. A guarda também repete a frase antiga, embora seu cabeçalho já declare a substituição. ([bin/auth_sitios.txt:3](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/auth_sitios.txt:3), [app/docs/BACKLOG.md:1257](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/BACKLOG.md:1257), [bin/janela_auth.sh:9](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:9))

   **[fila] Há registros históricos de bloqueio por fim de semana**, que não demonstram um segundo cálculo vigente. `trava.sh` calcula outra janela, de dinheiro/backup; não reproduz a velha proibição de auth. Outro recálculo dessa proibição em `bin/` ou `app/config/`: **[nao verificado]**. ([app/docs/RELATO.md:4251](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/RELATO.md:4251), [bin/trava.sh:25](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/trava.sh:25))

   **[bloqueia] Falta registrar o corte e a obra futura.** A busca integral não encontrou `JANELA-NAO-SEGURA-POUSO` em CORTES nem `JANELA-VIRA-SMOKE` no BACKLOG; o cabeçalho promete “Texto literal em CORTES.md”. Isso deixa incompletos os itens 3 e 4 do corte fornecido. ([bin/janela_auth.sh:4](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:4), [app/docs/CORTES.md:1](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/CORTES.md:1), [app/docs/BACKLOG.md:1](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/BACKLOG.md:1))

3. **[fila] O selo morde as duas regressões pedidas.** Reprodução da seção A exclusivamente em memória:

   - Regra atual: **11/11**.
   - Regra velha: **4 falhas** — sexta `1800`, sábado `1000`/`0600`, domingo `2319`.
   - Nunca barrar: **4 falhas** — sábado `2320`, domingo `0559`, quarta `2320`/`0559`.

   Os casos e expectativas estão em [bin/tests/test_janela_auth.sh:26](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:26).

   **[fila] Lacunas:** a seção A só confere prefixo da saída, ignorando status; `proxima()` não é exercitada pelo modo `--janela`. O selo completo não foi executado porque cria, escreve e apaga temporários. ([bin/tests/test_janela_auth.sh:41](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:41), [bin/janela_auth.sh:75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75), [temporários:79](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:79))

4. **[fila] Diretamente, o diff de `deploy.sh` muda só comentário. Indiretamente, o deploy passa a liberar os horários novos porque chama a guarda alterada.** O motivo do corte não é preenchido automaticamente: continua dependendo de quem chama fornecer `SEM_JANELA_AUTH_MOTIVO`. ([bin/deploy.sh:127](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:127), [144](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:144), [bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55))

   **[fila] Cumprimento em produção de reversão pré-anotada, ato único, `/api/me/` 401 e linha no topo do RELATO: [nao verificado].**

## 14/11 11:41 · sessao daef3b0a03f3 · - · - · origem=Ronald (tmux)

- **[fila] Prosa antiga ainda prescritiva:** `bin/auth_sitios.txt:3-4` afirma que sexta 18h+ e fim de semana barram; `app/docs/BACKLOG.md:1257` conserva a mesma regra no item O4. `bin/janela_auth.sh:9` repete a frase antiga, embora o próprio cabeçalho `bin/janela_auth.sh:2-4` a revogue expressamente. O selo contextualiza a citação antiga com a mudança em `bin/tests/test_janela_auth.sh:4-6`; o deploy também a marca como passado em `bin/deploy.sh:128-129`.

- **[bloqueia] Registro exigido pelo corte não encontrado:** buscas literais por `JANELA-NAO-SEGURA-POUSO` e `JANELA-VIRA-SMOKE` em `app/docs` não retornaram ocorrência; logo o código promete “Texto literal em CORTES.md” sem que eu localizasse o registro (`bin/janela_auth.sh:4`). A exigência geral de registrar no mesmo turno está em `app/docs/CORTES.md:5-7`. A página é gerada de `app/docs/CORTES.json` (`app/docs/CORTES.md:3`), pelo escritor `bin/gerar_cortes.py:114-116`. A ausência em quaisquer fontes externas à cópia é **[não verificado]**.

- **[fila] Escritor e leitores:** a decisão horária mora em `bin/janela_auth.sh:55-60`; a próxima abertura em `bin/janela_auth.sh:63-69`. A guarda lê a lista declarada (`bin/auth_sitios.txt:13-22`) em `bin/janela_auth.sh:84-95`. O modo horário consulta a decisão em `bin/janela_auth.sh:75-81`; o modo de deploy em `bin/janela_auth.sh:107-114`. O consumidor operacional em `bin/` chama essa guarda, ou atravessa com motivo não vazio, em `bin/deploy.sh:144-148`. O selo chama a guarda em `bin/tests/test_janela_auth.sh:41`, `:86`, `:97`, `:116`.

- **[fila] Sem segundo cálculo encontrado no escopo:** as buscas em `bin/` e `app/config/` localizaram a comparação horária na guarda (`bin/janela_auth.sh:58-59`, `:66-67`) e a chamada pelo deploy (`bin/deploy.sh:146`). Não há diretório `config/` na raiz desta cópia **[não verificado por arquivo:linha]**.

- **[fila] Referências históricas não devem ser tratadas como regra nova:** a negativa de um deploy de outubro em domingo permanece em `app/docs/RELATO.md:3853`; o relato de fim de semana em `app/docs/RELATO.md:4251`; os estados antigos das obras Q4/Q4b em `app/docs/TICKETS.md:224-225`. São registros de atos anteriores, embora estes últimos conservem linguagem de estado corrente.

- **[fila] Consumidor fora do escopo solicitado:** `fatias_agendadas/o135-teto20/esteira.sh:64` também chama a guarda da cópia com `origin/main`, sem recalcular o horário. Outra prosa antiga fora de `bin/`, `app/docs`, `app/config` está em `app/core/management/commands/replay_credenciais.py:4-5`.

_5130 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
