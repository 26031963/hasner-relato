# MAPA DE JUIZES — quem responde cada pergunta, por familia

_GERADO de `app/core/juizes.py` (adendo Ronald 25/09, LEIS-INDICE). **Nao editar a mao**: rode o
gerador. Selo: `bin/tests/test_mapa_juizes.sh` -- juiz declarado em `core/juizes.py` sem linha aqui
= VERMELHO._

A lei que este mapa serve e a **L-002** do `docs/LEIS.md` (TESTEMUNHA LE, NAO RECALCULA): cada
pergunta tem UM dono, e os leitores leem dele. As tres colunas sao, por familia:
**PERGUNTA** -> **JUIZ** (o `arquivo::funcao` que responde) · **SEM JUIZ** (pergunta que ninguem
responde ainda, com o motivo declarado) · **PENDENTES** (leitores que respondem por conta propria e
esperam migrar -- a lista so encolhe).


## Familia `batida`

| pergunta | juiz (dono) |
|---|---|
| quantas horas este PAR (turno) vale? | `ponto/motor_calculo_v2.py::MotorBase.calcular_periodo` |
| que periodos e que intervalo teve este dia? | `ponto/juiz_batida.py::periodos_do_dia` |

**SEM JUIZ declarado** (3):

- **a batida e espuria (cluster)?** -- nao migrada: `detectar_cluster_espurio --apply` e o cluster-guard de `escala/utils.py` decidem em sitios diferentes
- **a batida esta dentro do geofence?** -- nao migrada: `reconciliar_geofence` e `acao_recusar_geofence` respondem cada um por si -- candidata do corte de 25/09, sem censo
- **o par e relampago?** -- nao migrada: `detectar_par_relampago` decide e RETRATA; nenhum outro leitor pergunta

## Familia `turno/marcos`

| pergunta | juiz (dono) |
|---|---|
| a entrada atrasou ou a saida adiantou alem da tolerancia? | `ponto/motor_calculo_v2.py::MotorBase.aplicar_tolerancia` |
| a proxima batida e entrada ou saida? | `ponto/turnos.py::proximo_tipo_de` |
| a que turno (data_turno) pertence a batida? | `ponto/turnos.py::turnos_do_colab` |
| este turno tem dois blocos (e partido)? | `escala/servico_jornada.py::eh_turno_partido` |
| ha turno aberto vivo agora? | `ponto/turnos.py::turno_aberto_de` |
| o dia esta encerrado? | `ponto/turnos.py::dia_encerrado` |
| o tipo gravado deve virar (flip)? | `ponto/turnos.py::decidir_tipo_estrito` |
| o turno previsto cruza a meia-noite? | `escala/servico_jornada.py::turno_cruza_meia_noite` |
| o vao entre batidas foi intervalo? | `ponto/turnos.py::intervalos_do_turno` |
| quais sao os marcos previstos do dia? | `escala/models.py::EscalaColaborador.marcos_do_dia` |
| que marco previsto a batida ocupa? | `escala/utils.py::_match_marcos` |
| que status (ok, atraso, adianto, grave) tem a lampada do marco? | `escala/utils.py::status_do_delta` |

## Familia `celula/precedencia`

| pergunta | juiz (dono) |
|---|---|
| o dia e de trabalho para o vinculo? | `escala/models.py::EscalaColaborador.eh_dia_trabalho` |
| o dia e feriado para o colaborador? | `core/feriados.py::eh_feriado` |
| qual fato vence o dia (ferias > afastamento > atestado > feriado > folga > trabalho)? | `ponto/precedencia.py::fatos_do_dia` |
| quantos minutos o dia preve? | `escala/utils.py::minutos_previstos_do_dia` |
| quantos minutos o dia realizou? | `ponto/turnos.py::realizado_do_dia` |
| que marco do dia faltou (furo)? | `escala/servico_jornada.py::classificar_falta` |
| uma ausencia cobre o dia para a folha? | `ponto/turnos.py::ausencia_cobre` |
| uma ausencia suspende a cobranca do dia? | `ponto/turnos.py::ausencia_cobre` |

## Familia `escala`

_nenhum juiz declarado nesta familia._

**SEM JUIZ declarado** (2):

- **o vinculo cobre a data?** -- UMA copia desde 22/10 (aval CONTRATOS RESTANTES (2)): `escala/folgas.py::_cobre`; `gerar_celulas.py::_outro_vinculo_cobre` passou a CHAMA-la (impacto medido na sombra: 0 em 39.463 pares vinculo x dia). Falta so a frase da TRAVA JUIZ-NOVO para declarar aqui
- **qual vinculo vale neste dia?** -- a LEI existe (O69, prompt dele de 27/09 00:xx): a resposta e `CelulaDia.escala_geradora`, lida por `escala/alimentacao.py::vinculo_do_dia`. Falta a FRASE da TRAVA JUIZ-NOVO pelo nome da funcao, e eu nao posso escreve-la: a autonomia de 11/10 exige autoridade unica MEDIDA, e sao 8 arquivos (9 linhas)

## Familia `ausencia/ferias`

| pergunta | juiz (dono) |
|---|---|
| a ausencia esta aberta ou decidida? | `ponto/services/ausencia.py::ABERTAS` |
| a ausencia exige documento, e em que prazo? | `ponto/services/ausencia.py::estado_inicial` |
| a ausencia paga, desconta ou suprime o dia? | `ponto/catalogo/ausencias.py::efeito_vigente` |
| ha 3 ou mais lancamentos medicos na competencia? | `ponto/services/alertas_ausencia.py::resumo_atestados` |
| o aviso de 30 dias foi dado (art.135)? | `ferias/situacao.py::DIAS_AVISO_PREVIO` |
| o colaborador esta afastado hoje? | `ponto/turnos.py::afastado_hoje` |
| o colaborador esta de ferias hoje? | `ferias/services.py::em_gozo_hoje` |
| o fracionamento e valido (art.134)? | `ferias/models.py::AgendamentoFerias.clean` |
| o lancamento colide com outro fato (recusar)? | `ponto/precedencia.py::veto_de_lancamento` |
| o periodo venceu ou esta vencendo (art.137)? | `ferias/models.py::status_de_periodo` |
| qual o saldo do periodo aquisitivo? | `ferias/models.py::PeriodoAquisitivo.dias_saldo` |
| quanto a ausencia dura, e o tipo e valido? | `ponto/catalogo/ausencias.py::fim_de` |
| quantos dias ainda podem ser vendidos (abono, 1/3)? | `ferias/models.py::PeriodoAquisitivo.dias_vendaveis` |
| que dias a ausencia ocupa? | `ponto/turnos.py::cobertura_ausencia_periodo` |

## Familia `chamado`

| pergunta | juiz (dono) |
|---|---|
| a ausencia do dia suspende, fecha ou levanta a cobranca viva? | `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia` |
| a cobranca ficou orfa depois da regeneracao? | `escala/services/regeneracao.py::cobranca_orfa` |
| a disputa pode fechar? | `chamados/models.py::DisputaSupervisao.pode_fechar` |
| a fabrica de perguntas alcanca o chamado (e se nao, por que)? | `chamados/services/disputa_emissao.py::motivo_fora_da_fabrica` |
| a pergunta da disputa esta viva? | `chamados/juizes.py::pergunta_viva` |
| a premissa do chamado morreu (pode fechar)? | `chamados/catalogo/premissa.py::premissa_morta` |
| alguma pergunta desta colecao esta viva? | `chamados/juizes.py::fio_vivo` |
| o chamado e duplicado (mesmo fato)? | `chamados/models.py::ChamadoColaborador.abrir` |
| o chamado esta vivo? | `chamados/catalogo/motor.py::VIVOS` |
| o prazo (SLA) do chamado estourou? | `chamados/catalogo/motor.py::prazo_estourou` |
| pode transitar de um estado a outro? | `chamados/catalogo/motor.py::decidir` |
| por que o chamado vivo de furo nao tem pergunta no app? | `chamados/services/pergunta_no_app.py::retencao` |
| qual a competencia do chamado? | `chamados/juizes.py::competencia_do_chamado` |
| qual o dia do chamado? | `chamados/catalogo/modulos.py::data_do_chamado` |
| quem pode agir no chamado? | `chamados/utils.py::pode_agir_chamado` |

## Familia `fechamento`

| pergunta | juiz (dono) |
|---|---|
| a competencia esta fechada para a celula deste colaborador (TXT vigente ou holerite publicado)? | `folha/export.py::competencia_fechada_para_celula` |
| a competencia ja foi entregue (lavrada)? | `folha/export.py::competencia_entregue` |
| a competencia pode ser aprovada/trancada (o que bloqueia)? | `folha/export.py::classificar_export` |
| o periodo foi reaberto? | `ponto/services/fechamento.py::periodo_trancado` |
| o que "fechado" quer dizer (qual fronteira barra a escrita)? | `ponto/services/fechamento.py::competencia_trancada` |
| qual e a competencia encerrada (a anterior a de hoje)? | `ponto/janelas.py::janela_anterior` |
| quanto da competencia esta cumprido (prontidao)? | `folha/export.py::prontidao` |
| que FechamentoMensal e o da competencia de hoje? | `ponto/janelas.py::janela_atual` |
| quem entra no TXT da folha? | `folha/export.py::classificar_export` |

## Familia `feriado/prazo`

| pergunta | juiz (dono) |
|---|---|
| a competencia do dia esta fechada (pode escrever)? | `ponto/janelas.py::competencia_fechada` |
| a que competencia (corte a corte) pertence o dia? | `ponto/janelas.py::janela_atual` |
| feriado trabalhado paga dobra ou hora simples? | `ponto/motor_calculo_v2.py::CICLOS_FERIADO_SIMPLES` |
| o feriado suprime o trabalho do dia? | `ponto/precedencia.py::fatos_do_dia` |
| qual o prazo de arquivo/silencio do chamado? | `chamados/catalogo/motor.py::PRAZO_ARQUIVO_DIAS` |
| qual o prazo do DP para responder a justificativa? | `ponto/catalogo/justificativas.py::prazo_estourou` |

**SEM JUIZ declarado** (2):

- **o que e dia util?** -- nenhuma: seg-sex sem feriado nem escala (VT, VA, janela comercial do escalonamento)
- **que horas do turno que cruza a meia-noite contam como feriado?** -- nenhuma: tudo pela data local da ENTRADA (turno partido pelo dia da jornada); 191 vinculos ativos cruzam a meia-noite em 14/09 -- PARADO S6-BORDA-DO-FERIADO

**PENDENTES nesta familia**: 13 leitor(es) com regra propria, esperando migrar.

## Familia `tela`

| pergunta | juiz (dono) |
|---|---|
| a ausencia vale no dia (e quanto)? | `ponto/catalogo/ausencias.py::efeito_vigente` |
| o chamado ou a pergunta esta vivo na fila? | `chamados/juizes.py::pergunta_viva` |
| o colaborador entra na folha (e por que nao)? | `folha/export.py::classificar_export` |
| o dia acusa (falta, furo, ok) -- o que a celula julgou? | `escala/servico_jornada.py::classificar_falta` |
| o dia oferece "Resolver dia" (a decisao do dia esta pendente na celula)? | `colaboradores/services/calendario.py::resolver_dia_oferecido` |
| o periodo de ferias esta vencido ou vencendo? | `ferias/models.py::status_de_periodo` |
| quais os totais da competencia (horas, HE, noturno, faltas)? | `folha/export.py::eventos_do_fechamento` |
| quais sao os turnos do colaborador (pares de batida)? | `ponto/turnos.py::turnos_do_colab` |
| qual competencia a tela mostra? | `ponto/janelas.py::janela_atual` |
| quantos minutos o dia realizou? | `ponto/turnos.py::realizado_do_dia` |
| quem esta em turno (turno aberto vivo) agora? | `ponto/turnos.py::turno_aberto_de` |

**SEM JUIZ declarado** (1):

- **numero de tela sem autoridade declarada (score, faixa de horario, atraso de sincronizacao)?** -- score (produtor inteiro por conta propria, pendentes A1/C1), faixa manha/tarde/noite e atraso de sincronizacao -- pergunta de negocio de cada tela, vem na fatia dela

**PENDENTES nesta familia**: 110 leitor(es) com regra propria, esperando migrar.

## Familia `portas`

| pergunta | juiz (dono) |
|---|---|
| a porta de escrita tem uso humano e smoke de clique? | `core/portas_da_tela.py::censo` |

**PENDENTES nesta familia**: 124 leitor(es) com regra propria, esperando migrar.

## Totais

| familia | juizes | sem juiz | pendentes |
|---|---|---|---|
| `batida` | 2 | 3 | 0 |
| `turno/marcos` | 12 | 0 | 0 |
| `celula/precedencia` | 8 | 0 | 0 |
| `escala` | 0 | 2 | 0 |
| `ausencia/ferias` | 14 | 0 | 0 |
| `chamado` | 15 | 0 | 0 |
| `fechamento` | 9 | 0 | 0 |
| `feriado/prazo` | 6 | 2 | 13 |
| `tela` | 11 | 1 | 110 |
| `portas` | 1 | 0 | 124 |
| **total** | **78** | **8** | **247** |
