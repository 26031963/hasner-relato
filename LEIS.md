# LEIS.md — INDICE UNICO DAS LEIS VIGENTES

_Criado em 25/09 por ordem do Ronald (LEIS-INDICE). **Nenhuma lei inventada**: cada linha sai de
`CLAUDE.md`, `docs/CORTES.json`/`CORTES.md` ou do `BACKLOG`, e a coluna ORIGEM diz de onde._

**Como usar**: todo prompt e todo commit citam os `L-NNN` que aplicam. A coluna DONO diz o
`arquivo::funcao` que APLICA a lei -- quando ela esta vazia ou diz NENHUM, a lei esta escrita e
**ninguem a le**, que e a informacao mais util deste indice.

**Selo**: `bin/tests/test_leis_indice.sh` -- corte em `docs/CORTES.json` que nao aparece nem como
lei nem na lista de ATOS declarada no fim deste arquivo = VERMELHO.

| ID | a lei em uma frase | origem | dono (aplica) | selo | estado |
|---|---|---|---|---|---|
| **L-001** | ORIGEM, NUNCA BAND-AID: a cura vai no sitio que e a FONTE; "fallback"/"por enquanto"/"acerta o caso comum" sao nomes do band-aid e param a fatia | LEI-AKITA 1 -- corte Ronald 23/09 18:xx | CLAUDE.md secao 0 (conduta, sem dono de codigo) | `bin/tests/test_lei_akita.sh` (cobra a linha de selo de conduta no commit) | vigente |
| **L-002** | TESTEMUNHA LE, NAO RECALCULA: tela, PDF, TXT, badge e contador leem a MESMA autoridade; leitor com funcao propria de derivacao = vermelho | LEI-AKITA 2 -- corte Ronald 23/09 18:xx | `core/juizes.py` (registro por familia) + `core/contratos_estruturais.py` | `core/tests/test_contract_juiz_tela.py` e os `test_contract_juiz_*.py` por familia | vigente |
| **L-003** | ZERO NUMERO SEM MEDICAO NA FONTE: medir chamando a funcao REAL que o sistema usa para julgar, nunca replicando a logica nem lendo campo lateral | LEI-AKITA 8 -- corte Ronald 23/09 18:xx | CLAUDE.md secao 0 + secao 6 ("nao reconstruir em sonda propria") | sem selo automatico -- cobrada na revisao de cada fatia | vigente |
| **L-004** | ESCOPO DO AVAL E LITERAL: aval condicional que nao fecha na condicao = PAROU, com o numero | LEI-AKITA 9 -- corte Ronald 23/09 18:xx | CLAUDE.md secao 0 | sem selo automatico | vigente |
| **L-005** | CONSTRUIR EM COPIA DO HEAD, APLICAR NO COMMIT: nunca editar a arvore viva | LEI-AKITA 10 -- corte Ronald 23/09 18:xx | CLAUDE.md secao 0 + secao 2 (template no bind-mount muda a tela NA HORA) | `.git/hooks/pre-commit` -> tripwire `index_vs_arvore` (recusa commit com index != disco) | vigente |
| **L-006** | TUDO TEM CADASTRO: regra que varia por cliente/praca/empresa nasce como cadastro pela UI, com nome e leitor; chave sem leitor = vermelho | LEI-AKITA 12 -- corte Ronald 23/09 18:xx | transversal; ver L-020 (regime por empresa) e L-031 (intrajornada) como casos | `core/tests/test_regime_por_empresa.py::test_MORDE_a_regra_e_CADASTRO_e_nao_literal` (pela AST) | vigente |
| **L-007** | UM ESCRITOR POR ESTADO, NADA EM BRANCO: campo de estado tem 1 escritor canonico + tripwire; acao critica grava usuario, antes/depois e motivo | LEI-AKITA 7 -- corte Ronald 23/09 18:xx | por familia; ver L-021 (vigencia) e L-030 (flip de batida) | `core/contratos_estruturais.py` contrato 2 + `escala/tests/test_vigencia_um_escritor.py` (pela AST) | vigente |
| **L-008** | AUTORIZACAO PREVIA DA SESSAO: construir, medir, provar na sombra, curar, fabricar fatia, rodar DIFF e cadastrar com trilha = autorizado; a esteira nunca para por aval | corte Ronald 22/09 19:0x (AUTORIZACAO-PREVIA) | CLAUDE.md secao 7b | sem selo automatico | vigente |
| **L-009** | PRE-APROVADO x NUNCA PRE-APROVADO: pre-aprovacao cobre ROTINA, nunca CONTORNO. Nunca pre-aprovado: apply de dinheiro, mudanca de dado de escala/vinculo, apagar ou voltar ao HEAD arquivo que prod usa, regra de negocio fora do pedido, qualquer atalho | regra permanente -- Ronald 25/09 22:1x | CLAUDE.md secao 7b | sem selo automatico (e conduta); as 3 ultimas proibicoes nasceram medidas em 25/09 | vigente |
| **L-010** | PROMPT-NAO-SE-REPETE: todo prompt ganha linha em `app/docs/PROMPTS.md`; prompt que pede obra vira item no BACKLOG no MESMO turno | corte Ronald 22/09 23:3x (PROMPT-NAO-SE-REPETE) | `app/docs/PROMPTS.md` + bloco OBRAS de `app/docs/BACKLOG.md` | `bin/tests/test_prompt_virou_item.sh` | vigente |
| **L-011** | TRAVA JUIZ-NOVO: registrar autoridade nova em `core/juizes.py` exige a frase "corte Ronald: juiz <nome> nasce" no CORTES.md | corte Ronald 24/09 10:xx | `core/juizes.py` | `bin/tests/test_juiz_novo_tem_corte.sh` | vigente |
| **L-012** | CODIGO NO AR SO POR `bin/deploy.sh`: gravar um `.py` nao o poe no ar (max_requests=0 nas 3 cascas); template no bind-mount, ao contrario, muda a tela NA HORA | BUG 128 -- 12/09 | `bin/deploy.sh` | `bin/selo_reciclagem.sh` + a prova de rota do proprio deploy | vigente |
| **L-020** | CELULA SOBERANA: dia + marcos sao DADO com DNA congelado, irretroativo -- com UMA excecao formal: passado ERRADO POR CADASTRO se reescreve pela porta, e a celula DENUNCIA o ato | corte Ronald 16/08 (excecao) + campos de denuncia desde 01/09 | `escala/models.py::CelulaDia` (dado) e `ponto/portas/celula.py::regenerar_celulas_vinculo` (a UNICA porta) | `ponto/tests/test_contract_competencia_lavrada.py` (a guarda da lavra se abre por ATO, com trilha) | vigente |
| **L-021** | UM ESCRITOR DE VIGENCIA: `data_fim` de vinculo nunca e anterior ao `data_inicio`; a guarda mora no FILTRO da consulta, e o que sobra dela e RECUSADO com pk no log | corte Ronald 25/09 (VINCULO-FIM-ANTES-DO-INICIO) | `colaboradores/services/vinculo.py::validar_vigencia` + `fechar_vigencia` (9 portas leem) | `escala/tests/test_vigencia_um_escritor.py` (o contrato 1-ESCRITOR pela AST) | vigente -- `CheckConstraint` ainda BLOQUEADO por 53 registros do passivo |
| **L-022** | FASE DO 12x36: a PARIDADE DA FOTO vence a ancora. Dia fora da foto herda a paridade da folga mais proxima; SEM foto alguma a resposta e `None` ("nao decidido"), nunca trabalho | corte Ronald 25/09 ("a paridade da foto vence; aplica os 52 dias") | `escala/models.py::EscalaColaborador.eh_dia_trabalho_calculado`, ramo da foto do mes | `escala/tests/test_o37_foto_parcial.py` (o PAR: mesma ancora, fotos deslocadas 1 dia -> vereditos OPOSTOS) | vigente |
| **L-023** | A FOTO COMPLEMENTA onde o ciclo sabe responder e SUBSTITUI onde ele nao sabe -- uma folga avulsa no mes NAO apaga o `folga_dia_semana` do template | BO admin 21/09 (mat 1758) | `escala/models.py::eh_dia_trabalho_calculado` | coberto por L-022; o caso 6x1/5x2 tem os 44 vinculos medidos no proprio comentario | vigente |
| **L-024** | A GRADE E FONTE UNICA DE PREVISTO: existindo fonte, e proibido derivar em paralelo | S133 | `escala/utils.py::minutos_previstos_do_dia` | `core/contratos_estruturais.py` familia escala | vigente |
| **L-025** | UNIVERSO DE APURACAO = `ponto/turnos.py::batidas_apuraveis` (exclui `retratada_em`); EXIBICAO de marcacao le o cru, por lei (Portaria 671) | CLAUDE.md secao 4 | `ponto/turnos.py::batidas_apuraveis` | PENDENTE: o tripwire que proibiria `Batida.objects.filter` cru em caminho de apuracao NAO EXISTE (achado B5-F1, 04/09) | vigente -- SEM selo |
| **L-026** | COMPETENCIA CORRE DO DIA 21 AO 20 (`Empresa.dia_inicio_competencia`), nunca mes civil; nenhum juiz usa 21 cravado | CLAUDE.md secao 1 + corte K8-COMPETENCIA-NAO-E-MES-CIVIL (25/09 09:2x) | `ponto/janelas.py::janela_fechamento` / `janela_atual` / `corte_da_empresa` | `ponto/tests/test_contract_competencia_nao_e_mes_civil.py` | vigente |
| **L-030** | A BATIDA DE CHAO NUNCA E BARRADA EM RUNTIME; chokepoint unico de escrita = `ponto/registro_batida.py`; o motor NUNCA fabrica batida | CLAUDE.md secao 4 (zona inviolavel) | `ponto/registro_batida.py` | `ponto/tests/` (contratos de batida) + `tripwire_tipo_batida` (cron 07:20) | vigente |
| **L-031** | INTRAJORNADA: quem decide se o intervalo NAO BATIDO indeniza e o CADASTRO `TipoEscala.intervalo_indenizavel`. `True` = indeniza so o suprimido; `False` = pre-assinalado, sem HE e sem indenizacao, e a batida faltante vira PERGUNTA | decidida 19/08 (Art.71 par.4 / Sumula 437); reafirmada no adendo Ronald 25/09 19:0x | CAMPO existe em `escala/models.py:82`; **`ponto/motor_calculo_v2.py` NAO O LE em nenhuma linha** -- `:758-771` indeniza sempre | SEM SELO -- e a O49 | vigente, SEM DONO QUE A LEIA: 275 templates com `False` contra 63 com `True`, a MAIORIA indenizada contra o proprio cadastro |
| **L-032** | PRE-ASSINALACAO DO INTERVALO (CLT art. 74 par.2): o intervalo CADASTRADO sempre sai da jornada; batida de intervalo fora do horario exato mas dentro do envelope E o intervalo -- nunca espuria, nunca HE | ordem Ronald 25/09 (INTERVALO-CADASTRADO-NAO-VIRA-HE) | NENHUM: `intervalo_duracao_min` nao tem leitor no motor; `motor_calculo_v2.py:355` fixa `intrajornada_minutos = 60` | SEM SELO -- e a O49 | vigente, SEM DONO: RED medido -- [nome] te#187 (13:00-14:30, 90 min) recebeu +1,68 h e +1,63 h de HE em 13 e 18/08 |
| **L-033** | FURO SO DE INTERVALO NAO TRAVA A FOLHA: dia com entrada E saida batidas e so a batida de intervalo faltando e PAGAVEL, e a pergunta ao colab segue aberta sem bloquear | adendo Ronald 25/09 19:2x | a definir (porta de aptidao do fechamento) | SEM SELO -- porta extra da E2: `furo_so_intervalo` que bloqueia apto = 0 | vigente, SEM DONO: 832 celulas furo = 293 so intervalo + 539 entrada/saida; 49 colabs travados SO por intervalo |
| **L-034** | REGIME POR EMPRESA: a EMPRESA vence a PRACA. `Empresa.regime_trabalhista` = `clt` -> piso legal mesmo em praca com CCT vigente; vazio = a praca decide | corte Ronald 25/09 ("JSP = CLT, demais = CCT") | `core/regua_cct.py::regua_para` | `core/tests/test_regime_por_empresa.py` (o PAR: mesma praca, mesma CCT, empresa em CLT x sem regime) | vigente -- aplicado: emp3=`clt`, emp2 e emp4=`cct` |
| **L-035** | PRORROGACAO NOTURNA POS-05h, decidida em UM sitio pelo CADASTRO: CLT + 12x36 = NAO conta (art. 59-A par. unico) -> 22:00-05:00 reduzida = 8h/noite; CLT + jornada comum = conta (art. 73 par.5); CCT = o que a CCT cadastrada diz. A regra do 12x36 entra SO com `clt` DECLARADO | ordem Ronald 25/09 + aval das 18h (ADICIONAL-NOTURNO-12X36) | `core/regua_cct.py::prorrogacao_pos5h_legal`, lida por `regua_para` e `get_motor_cct`; o default do motor virou `None` (`motor_calculo_v2.py:290` e `get_motor:1556`) | `ponto/tests/test_prorrogacao_pos5h_por_cadastro.py` (13 casos; MORDE: sem `clt` declarado o dinheiro NAO se move) | vigente -- aplicado em 09/2026 na emp3: 39 fechamentos, -953,63 h |
| **L-036** | CCT VIGILANTES LONDRINA cl.38-d: no 12x36, ainda que noturno, a hora e NORMAL de 60 min -- afasta a hora reduzida do art.73 par.1, e o adicional de 20% SEGUE DEVIDO (a rubrica 0025 existe) | parecer do advogado 01/09; `HORA_REDUZIDA_12X36_EM_SECO=False` em prod | `core/regua_cct.py` + `Sindicato.hora_reduzida_afastada_12x36` | CLAUDE.md secao 6b guarda o lastro; selo proprio nao ha | vigente |
| **L-037** | CARTAO = ESPELHO, e o PERIODO e o PEDIDO: o cartao PDF desenha o dict `dias` da tela, e as linhas cobrem o periodo pedido pela celula de cada dia, QUALQUER vinculo -- o piso visual deixou de ser um CORTE | corte Ronald 24/09 12:5x (CARTAO-E-ESPELHO) + 25/09 (CARTAO-CORTADO) | `ponto/services/espelho.py` (`vis_ini = min(piso_visual(...), apur_ini)`) | `relatorios/tests/test_cartao_cobre_o_periodo.py` + `api/tests/test_bug139_espelho_app_mesma_fonte.py` (app == admin) | vigente -- alcanca tambem o app dos ~750 |
| **L-038** | PDF SEM REGRA PROPRIA: o PDF nao tem regra de dia; le o MESMO juiz do espelho | corte Ronald 23/09 17:5x (PDF-SEM-REGRA-PROPRIA), revoga o "`_dj` como fallback" de 22/09 | `relatorios/pdf_espelho.py` -> `escala/utils.py::minutos_previstos_do_dia` e o juiz do dia | `core/tests/test_contract_juiz_tela.py::test_MORDE_pendente_curado_sai_da_lista` cobrou a saida do pendente quando a tela foi curada | vigente |
| **L-039** | FALTA TEM UM SIGNIFICADO = decidida (ausencia lancada cujo tipo desconta) | corte Ronald 23/09 09:2x (FALTA-UM-SIGNIFICADO) | `escala/servico_jornada.py::classificar_falta` | `core/contratos_estruturais.py` familia ausencia | vigente (commit `1a4d626f`) |
| **L-040** | LISTA UNICA CADASTRO x REALIDADE: uma so lista de "o cadastro diz uma coisa e o ponto mostra outra", por posto -> vinculo -> assinatura -> acao, com o destino sugerido do propositor; o PDF e a MESMA lista | corte Ronald 18/09 | `escala/services/cadastro_realidade.py::lista` (le `ponto/services/esmeril_espelho.py::ler_lavra`) | `escala/tests/test_cadastro_x_realidade.py` (4 casos, inclui a rota antiga do LIMBO redirecionando) | vigente -- **BUG ABERTO**: nao ve a classe "vinculo com vigencia impossivel" (14 dos 53 ausentes da lavra de 81) |
| **L-041** | O PROPOSITOR DECIDE qual vinculo/escala as batidas confirmam, e a decisao da classe VINCULO-FIM sai dele -- sem criterio novo e sem perguntar de novo | aval Ronald 25/09 (regra permanente da classe) | `escala/services/propositor.py::propostas` (destino sugerido + confianca) | sem selo proprio; o consumo esta em `escala/services/cadastro_realidade.py` | vigente |
| **L-042** | A PROPOSTA NAO SE GUARDA: so a DECISAO mora em `DecisaoProposta`; a proposta e RECALCULADA a cada corrida | PROPOSTA-NO-FIO 12/09 | `escala/services/propositor.py` | declarado no docstring de `propostas()` | vigente |
| **L-050** | ARQUIVO SIMPLES -- EXPORTADO SEM FRONTEIRA: sem aviso nao ha fronteira; `dia_em_competencia_exportada` decide, e nao uma data de corte | aval Ronald 25/09 10:3x (EXPORTADO-SEM-FRONTEIRA) | a definir na fatia (O44 ARQUIVO-SIMPLES v2) | SEM SELO ainda | construindo |
| **L-051** | PASSIVO TRANCADA E HISTORIA: as 1.994 perguntas com `via_resolucao='competencia_trancada'` ficam INTOCADAS como historia, em estado arquivado reversivel por toque; nenhuma pergunta NOVA nasce com essa via | aval Ronald 25/09 10:3x (PASSIVO-TRANCADA-E-HISTORIA) | a definir na fatia (O44) | SEM SELO ainda | construindo |
| **L-052** | DIA QUE VIROU FOLHA NAO SE TOCA, nem para corrigir: competencia com TXT emitido e pulada pela porta de regeneracao | BUG 23 -- 31/08 (HX-REGEN-NAO-TOCA-EXPORTADO) | `ponto/portas/celula.py::regenerar_celulas_vinculo` (a guarda mora na PORTA, nao no chamador) | `ponto/tests/test_contract_competencia_lavrada.py` | vigente -- **BUG ABERTO**: a guarda barra a competencia SEGUINTE a exportada (off-by-one): barrou 21/07 e 14-20/08 da emp4, cuja ultima exportacao termina em 20/07 |
| **L-060** | CRON DE VARREDURA E VIGIA, NUNCA JUIZ: contador "esperado 0", so alarma; o papel e declarado em `config/crons.py` | principio Ronald 17/09 (tabuleiro e lampadas) | `config/crons.py` (papel por entrada) | `juizes_por_varredura` no placar | vigente |
| **L-061** | O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata | principio Ronald 17/09 | `chamados/catalogo/motor.py::decidir` (estado x evento) e os leitores | `core/contratos_estruturais.py` familia chamado | vigente |
| **L-062** | SILENCIAR EXIGE prazo + tripwire + item de fila: desligar emissor "para limpar fila" ja deixou o sistema mudo por 26 dias | CLAUDE.md secao 6 | transversal | sem selo automatico | vigente |
| **L-070** | SELO ANTI-VACUIDADE: todo selo que usa mock ou afirma sobre a SAIDA precisa de um caso que MORDE -- dois valores diferentes que precisam dar status diferentes | CLAUDE.md secao 6 (4 selos vazios em 01/09, um por familia) | transversal | o proprio `test_..._MORDE` de cada selo | vigente |
| **L-071** | TODO SELO DE FRONT RENDERIZA AS DUAS CASCAS: `base.html` (admin) e `base_app.html` (colaborador) sao arvores SEPARADAS | corte Ronald 08/09 | `core/tests/test_selo_modal_nao_nasce_vazio.py::test_MORDE_as_DUAS_cascas` | o proprio selo + o grafo de includes | vigente |
| **L-072** | FRONT SEM SMOKE NAO SOBE: fatia que toca `static/js/`, o service worker ou template BASE nao faz push sem smoke de clique do Ronald nas DUAS cascas | BUG 73 -- 07/09 | `core/tests/test_smoke_chromium.py` (o piso, nao o teto -- nao ve rota, Caddy, CSP nem gesto) | o proprio smoke + `bin/node_check.sh` | vigente |
| **L-073** | TETO TEMPORAL: teto por DATA nao basta (no cross-meia-noite o DIA acaba antes do TURNO); so valem comparar INSTANTE ou exigir FATO ENCERRADO | CLAUDE.md secao 6 -- 08/08, 3 vitimas num dia | emissores e juizes que julgam marco/celula missing | `ponto/tests/test_contract_teto_temporal.py` | vigente |
| **L-074** | UM RUN POR VEZ no `juliani_db_test`, e o veredito de uma suite e o rc do PROCESSO, nunca o do `grep` que formata | corte Ronald 24/09 09:3x + cura 25/09 (PUSH-QUE-NAO-DIZ-POR-QUE) | `bin/trava_teste.sh` (a trava) + `bin/pre-push.sh::_teste_irmao` (o veredito) | `bin/tests/test_prepush_veredito_e_o_rc.sh` (o PAR: forma velha devolve rc do grep e imprime NADA) | vigente |
| **L-075** | FONTE UNICA DAS LABELS: a lista de apps da suite mora em `bin/regua.sh::LABELS` e todo mundo a LE | achado 24/09 (arvore vermelha por 58 min com a suite dizendo OK) | `bin/regua.sh::LABELS` | `bin/tests/test_labels_fonte_unica.sh` (app com teste fora do LABELS = vermelho; lista repetida = vermelho) | vigente |
| **L-076** | SEGREDO FORA DO ARGV: a senha do banco de teste vai por `--env-file`, nunca em `-e` no argv, e a fonte e `logs/.senha_teste` (600) | corte Ronald 25/09 08:4x (SEGREDO-FORA-DO-ARGV) | `bin/recursos.sh::teste_envfile` (escreve `DB_PASSWORD` e `POSTGRES_PASSWORD` do MESMO valor) | `bin/tests/test_segredo_fora_do_argv.sh` (`grep` de senha em argv dentro de `bin/` = 0) | vigente |
| **L-077** | ZONA INVIOLAVEL: script que POSTa em porta HTTP de prod E ESCRITA, nunca "teste"; smoke de porta so contra banco LATERAL restaurado, dentro de `atomic()` com `raise` no fim | INCIDENTE 27/08 (144 perguntas + 25 batidas fabricadas em prod) | CLAUDE.md secao 6 + `bin/sombra.sh` (o banco lateral oficial) | sem selo automatico -- e proibicao | vigente |
| **L-078** | GATE TEMPORAL = CRON/AT + ARQUIVO: espera longa nunca e processo do Code (morre com a sessao); agenda-se com `bin/deploy_agendado.sh` | licao 16/09, corte Ronald | `bin/deploy_agendado.sh` (cron de 1 disparo em /etc/cron.d) | `deploys_agendados` no placar | vigente |
| **L-079** | PAUSA COM DONO: pausa sem quem/por que/condicao de saida nao tem saida -- ela simplesmente dura | corte Ronald 21/09 14:2x | `bin/pausar.sh` | `core/esteira_vigia.py` alarma pausa > 30 min sem dono | vigente |
| **L-080** | CHAVE DE MATCH SEMPRE CPF OU NOME COMPLETO, nunca matricula; timestamps em UTC no banco, apresentados em UTC-3 | CLAUDE.md secao 4 (zona inviolavel) | transversal (importadores, holerite, folha) | sem selo unico -- cobrado por fatia | vigente |
| **L-081** | CURA-MAIS-RESTRITIVA: bug PROVADO com mais de uma cura candidata NAO espera o Ronald -- aplica-se a mais RESTRITIVA (ou as duas, se nao conflitam), o porque vai ao RELATO e segue; so espera `!` se a cura cair na L-009 | corte Ronald 26/09 ~09:4x (nasceu da O57: bug provado, duas curas na mao, e eu registrei "nao decidido") | `CLAUDE.md` secao 7b | transversal -- a esteira inteira | **vigente** |
| **L-082** | AVAL-DE-CRITERIO (**altera a L-009**): apply de dinheiro com `!` de CRITERIO aplica sem nova parada se (a) o DIFF move so os campos-alvo nomeados, (b) todo outro campo de todo colab da ZERO e (c) o total fica na faixa aprovada; qualquer violacao = PAREI com a tabela. Sem criterio escrito, vale a L-009 | corte Ronald 26/09 ~11:4x (nasceu do apply de 09/2026: `!` de +12,29 h, gravado moveu +13,29 h com 10 campos fora do alvo em 2 colabs) | `CLAUDE.md` secao 7b | todo apply de dinheiro | **vigente** |

## Leis escritas que NAO TEM DONO (a lista mais importante daqui)

| lei | o que falta | numero medido |
|---|---|---|
| `L-031` intrajornada por `intervalo_indenizavel` | `motor_calculo_v2.py` nao le o campo; `:758-771` indeniza sempre | **275** templates com `False` contra **63** com `True` |
| `L-032` pre-assinalacao do intervalo | `intervalo_duracao_min` sem leitor; `:355` fixa `intrajornada_minutos = 60` | [nome] te#187: **+1,68 h** (13/08) e **+1,63 h** (18/08) de HE |
| `L-033` furo so de intervalo nao trava folha | porta de aptidao do fechamento nao distingue a classe | **293** celulas furo so de intervalo; **49** colabs travados so por isso |
| `L-025` universo de apuracao | tripwire contra `Batida.objects.filter` cru em caminho de apuracao NAO EXISTE | achado B5-F1, 04/09 |

## Leis VIGENTES com BUG ABERTO no dono

| lei | bug | RED |
|---|---|---|
| `L-040` lista unica | nao ve a classe "vinculo com vigencia impossivel" | **14** dos 53 ausentes da lavra de 81 |
| `L-052` dia que virou folha nao se toca | a guarda barra a competencia SEGUINTE a exportada (off-by-one) | barrou **21/07** e **14-20/08** da emp4, cuja ultima exportacao termina em **20/07** |
| `L-021` um escritor de vigencia | `CheckConstraint` bloqueado pelo passivo | **53** registros com `data_fim < data_inicio` |

## CORTES que sao ATO, nao lei (declarados para o selo nao os cobrar)

- `ACESSO-NUNCA-EM-LOTE`
- `ASSINATURA-EC-P256`
- `AUSENCIAS-DRAWER-E-LOTE`
- `CARTAO-TOTAL-IGUAL-SOMA`
- `CATALOGO-SAIDA-ANTECIPADA-DESCONTA`
- `CERT-VIGIA`
- `CHAMADO-GANHA-CADASTRO`
- `CHAMADO-VARREDURA-NAO-JULGA`
- `COL200-DIA-DO-TURNO`
- `CONTRATO-3-SEM-CONSUMIDOR-SAI`
- `CORTES-REGISTRADOS`
- `CREDENCIAL-POR-ESTADO`
- `E3-CHAMADO-APOS-ARQUIVO-SIMPLES`
- `ESPELHO-SINAIS`
- `ESPELHO-TELA-LE-A-FOLHA`
- `ESTEIRA-RETA-FINAL`
- `ESTEIRA-SECA-1-E-2-AGORA`
- `EXPORT-09-DESCONHECIDO`
- `FABRICANTE-LE-O-BACKLOG`
- `FECHAMENTO-ONLINE`
- `FECHAMENTO-UI-PORTAS`
- `[nome]-IOS`
- `FILA-24-09-16-5X`
- `JANELA-EXATA`
- `JUIZ-BATIDA-NASCE`
- `JUIZ-DE-BATIDA-E-DE-ESCALA`
- `JUIZ-ESCALA-NASCE`
- `K1-DIA-DO-CHAMADO`
- `K8-COMPETENCIA-NAO-E-MES-CIVIL`
- `MARCA-DO-VEREDITO`
- `ME-POSTO-GEO`
- `NOITE-23-09`
- `NON-STOP`
- `PARAMETRO-GANHA-ROTULO`
- `PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO`
- `PERTO-DO-MOTOR-ESPERA-O-EXPORT`
- `PISO-NAO-SOBE-POR-BATIDA`
- `PORTA-RETRATAR-BATIDA`
- `RELATORIO-ATESTADOS-FOTOS`
- `RESUMO-UMA-FONTE`
- `RETRATAR-93752`
- `SUSPENSAO-DESCONTA-JORNADA`
- `TETO-DA-MATRIZ-E-21`
- `TROCA-DE-PLANTAO`
- `W12X36-HPD`
- `ZUMBIDO`

## CORTES que viraram lei

- `AUTORIZACAO-PREVIA` -> **L-008**
- `CARTAO-E-ESPELHO` -> **L-037**
- `EXPORTADO-SEM-FRONTEIRA` -> **L-050**
- `FALTA-UM-SIGNIFICADO` -> **L-039**
- `LEI-AKITA` -> **L-001..L-007**
- `PASSIVO-TRANCADA-E-HISTORIA` -> **L-051**
- `PDF-SEM-REGRA-PROPRIA` -> **L-038**
- `PROMPT-NAO-SE-REPETE` -> **L-010**
