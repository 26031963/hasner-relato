# MAPA -- onde mora cada coisa (GERADO por bin/gerar_diagrama.py; NAO EDITAR A MAO)

Leia em 2 min no inicio da sessao. O desenho e docs/ARQUITETURA.mmd (mesmos blocos, com os numeros). O selo
core/tests/test_selo_diagrama_do_codigo.py regenera os dois e compara.

## (a) FRONTEIRA ponto x chamados
- core/tests/test_contract_direcao_a16.py (INERTES, allowlists)

## (b) PIPELINE DIARIO
- config/crons.py::CRONS (sched, depende=, estagio) -> bin/crons.sh install|check

## (c) PORTAS CANONICAS (escritores unicos)
- ponto/services/cartorio.py::julgar_celula -- celula julgada
- ponto/services/ausencia.py::criar_ausencia -- ausencia
- ponto/registro_batida.py::registrar_batida -- batida
- chamados/services/relavra_via.py::relavrar -- via da pergunta
- chamados/services/relavra_via.py::desfazer_carimbo_sem_lastro -- carimbo desfeito
- chamados/services/materializacao.py::materializar_perguntas_validadas_da_disputa -- materializacao
- chamados/reconciliador.py::retratar -- retratacao S128
- chamados/services/fio.py::reapontar_resolvedora -- reapontar fio

## (d) TABULEIRO (papel de cada cron)
- config/crons.py::papel, PAPEIS = juiz, vigia, alerta, lavra, agenda, gerador, infra, prazo, JUIZES_POR_VARREDURA (divida: esperado 0)

## (e) EVENTO (o sinal acorda o juiz do dia)
- ponto/services/cartorio.py::julgar_celula (juiz UNICO do dia)
- ponto/signals.py::_nucleo_post_save_batida (post_save Batida)
- ponto/signals.py::_realtime_post_save_batida (post_save Batida)
- ponto/signals.py::_nucleo_post_delete_batida (post_delete Batida)
- ponto/signals.py::_nucleo_post_save_escala (post_save EscalaColaborador)
- ponto/signals.py::_cartorio_post_save_batida (post_save Batida) -> julga
- ponto/signals.py::_cartorio_post_delete_batida (post_delete Batida) -> julga
- ponto/signals.py::_cartorio_post_save_ausencia (post_save Ausencia) -> julga
- ponto/signals.py::_cartorio_post_delete_ausencia (post_delete Ausencia) -> julga
- ponto/signals.py::_cartorio_post_save_justificativa (post_save Justificativa) -> julga
- ponto/signals.py::_reconciliador_post_save_batida (post_save Batida)
- ponto/signals.py::_consumidor_post_save_ausencia (post_save Ausencia)
- ponto/signals.py::_consumidor_post_delete_ausencia (post_delete Ausencia)
- ponto/signals.py::_geofence_post_save_posto (post_save Posto)
- chamados/signals.py::disputa_fechada_fecha_chamado_pai (post_save DisputaSupervisao)
- chamados/signals.py::rebaixa_urgencia_ao_resolver (post_save ChamadoColaborador)
- chamados/signals.py::resolve_perguntas_ao_resolver (post_save ChamadoColaborador)
- chamados/signals.py::retoma_regularizacoes_ao_fechar_disputa (post_save DisputaSupervisao)
- chamados/signals.py::materializa_perguntas_ao_fechar_disputa (post_save DisputaSupervisao)
- chamados/signals.py::_cartorio_pre_save_chamado (pre_save ChamadoColaborador)
- chamados/signals.py::_cartorio_post_save_chamado (post_save ChamadoColaborador) -> julga
- chamados/signals.py::_becos_post_save_pergunta (post_save PerguntaDisputa)
- chamados/signals.py::_isentos_post_save_colaborador (post_save Colaborador)
- chamados/signals.py::_isentos_post_save_chamado (post_save ChamadoColaborador)
- chamados/signals.py::_feriado_post_save (post_save Feriado)
- escala/signals.py::_ec_capturar_ancora_anterior (pre_save EscalaColaborador)
- escala/signals.py::_ec_vigencia_regenera_celulas (post_save EscalaColaborador)
- escala/signals.py::_ec_ancora_regenera_celulas (post_save EscalaColaborador)
- escala/signals.py::_ec_invalida_previsto (post_save EscalaColaborador)
- escala/signals.py::_ec_revalidar_disputas_ancora (post_save EscalaColaborador)
- escala/signals.py::_ec_folgas_seguem_a_pessoa (post_save EscalaColaborador)

## (f) JUIZES por familia
- core/juizes.py: JUIZES (pergunta -> autoridade), SEM_JUIZ (motivo), PENDENTES (o registro), fora_de_autoridade(familia) -> contador
- familias: celula/precedencia, turno/marcos, ausencia/ferias, chamado, feriado/prazo, fechamento, tela, portas, batida, escala
- contratos: */tests/test_contract_juiz_*.py (contagem e zonas do registro de cada familia)

## (g) PORTAS DE ESCRITA humanas
- core/portas_da_tela.py::censo (familias: chamados, dia do ponto, ausencia, escala/vinculo, pautas, ferias, folha, cadastro/config) -> manage.py censo_portas

## (h) VIGIAS e contadores
- config/crons.py: papel vigia, AGENDADOS_FORA_DO_CRON, linhas `# contador=... · esperado=... · dono=...`
- host: bin/placar_code.sh (o placar inteiro), bin/vigia_arvore.sh, bin/vigia_esteira.sh -> core/esteira_vigia.py

## (i) COBERTURA por familia (O245-CAMADA-COBERTURA: fitness function com inventario)
- fonte: core/juizes.py (JUIZES, SEM_JUIZ, PENDENTES, PROIBIDOS) + censo por AST dos `.py` de producao (o universo de `chamadores`); gerador: gerar_diagrama.py::cobertura; selo: core/tests/test_selo_diagrama_do_codigo.py::CamadaDeCoberturaTest
- CRITERIOS (o que cada numero CONTA):
  - contador = `fora_de_autoridade(familia)`; sem chamar + chama e deriva + nao sei == contador
  - `sem chamar` = a funcao que contem a impressao do pendente (o decorador conta) nao le o juiz da pergunta; `chama e deriva` = le e ainda deriva; `nao sei` = o AST de python nao responde, com o motivo -- nunca zero
  - ler o juiz = ligar o nome (ou o modulo) dele por import -- `as`, relativo, local de funcao, `__init__` que reexporta -- e usar o nome no escopo; metodo `Classe.metodo`: importar a classe ou o modulo e usar o atributo
  - sitio NOVO fora do registro: o AST nao responde. Familia COM padroes proibidos tem varredor (o selo da familia, */tests/test_contract_juiz_*.py::fora_da_autoridade); familia SEM eles nao tem, e o total e o `nao sei` dela publicam "sitio novo: nao sei (sem varredor)", nunca 0 puro
  - selo = */tests/test_*.py que liga o nome da familia ao registro (argumento de `fora_de_autoridade`, chave de PENDENTES/JUIZES/SEM_JUIZ/PROIBIDOS, `FAMILIA =` no topo, ou o apelido PENDENTES_<X>)
  - quem le cada juiz e definicoes de mesmo nome mudam com qualquer import: saem na saida de `manage.py gerar_diagrama`, nao neste arquivo

| familia | perguntas juiz/sem | fora_de_autoridade | sem chamar | chama e deriva | nao sei | proibidos | selos |
|---|---|---|---|---|---|---|---|
| celula/precedencia | 9/0 | 0 | 0 | 0 | 0 | 3 | 2 |
| turno/marcos | 15/0 | 0 | 0 | 0 | 0 | 3 | 1 |
| ausencia/ferias | 15/0 | 0 | 0 | 0 | 0 | 6 | 4 |
| chamado | 17/0 | 0 | 0 | 0 | 0 | 3 | 11 |
| feriado/prazo | 6/2 | 13 | 10 | 2 | 1 | 4 | 1 |
| fechamento | 9/0 | 0 | 0 | 0 | 0 | 5 | 1 |
| tela | 11/1 | 110 + sitio novo: nao sei (sem varredor) | 68 | 5 | 37 + sitio novo: nao sei (sem varredor) | 0 | 2 |
| portas | 1/0 | 124 + sitio novo: nao sei (sem varredor) | 0 | 0 | 124 + sitio novo: nao sei (sem varredor) | 0 | 25 |
| batida | 4/2 | 0 + sitio novo: nao sei (sem varredor) | 0 | 0 | 0 + sitio novo: nao sei (sem varredor) | 0 | 2 |
| escala | 2/0 | 0 + sitio novo: nao sei (sem varredor) | 0 | 0 | 0 + sitio novo: nao sei (sem varredor) | 0 | 1 |

### celula/precedencia -- fora_de_autoridade('celula/precedencia') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0
- padroes proibidos: 3, varridos pelo selo da familia
- selos: ponto/tests/test_contract_juiz_celula.py, ponto/tests/test_realizado_do_dia_autoridade.py
- perguntas:
  - o dia e de trabalho para o vinculo? -> `escala/models.py::EscalaColaborador.eh_dia_trabalho` · registro 0
  - quantos minutos o dia preve? -> `escala/utils.py::minutos_previstos_do_dia` · registro 0
  - qual fato vence o dia (ferias > afastamento > atestado > feriado > folga > trabalho)? -> `ponto/precedencia.py::fatos_do_dia` · registro 0
  - o dia e feriado para o colaborador? -> `core/feriados.py::eh_feriado` · registro 0
  - uma ausencia cobre o dia para a folha? -> `ponto/turnos.py::ausencia_cobre` · registro 0
  - uma ausencia suspende a cobranca do dia? -> `ponto/turnos.py::ausencia_cobre` · registro 0
  - que marco do dia faltou (furo)? -> `escala/servico_jornada.py::classificar_falta` · registro 0
  - quantos minutos o dia realizou? -> `ponto/turnos.py::realizado_do_dia` · registro 0
  - qual o status do dia (ok, irregular, falta, folga, ausencia...)? -> `ponto/status_dia.py::status_do_dia_lavrado` · registro 0

### turno/marcos -- fora_de_autoridade('turno/marcos') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0
- padroes proibidos: 3, varridos pelo selo da familia
- selos: ponto/tests/test_contract_juiz_turno.py
- perguntas:
  - a que turno (data_turno) pertence a batida? -> `ponto/turnos.py::turnos_do_colab` · registro 0
  - a proxima batida e entrada ou saida? -> `ponto/turnos.py::proximo_tipo_de` · registro 0
  - o tipo gravado deve virar (flip)? -> `ponto/turnos.py::decidir_tipo_estrito` · registro 0
  - ha turno aberto vivo agora? -> `ponto/turnos.py::turno_aberto_de` · registro 0
  - que marco previsto a batida ocupa? -> `escala/utils.py::_match_marcos` · registro 0
  - quais sao os marcos previstos do dia? -> `escala/models.py::EscalaColaborador.marcos_do_dia` · registro 0
  - o vao entre batidas foi intervalo? -> `ponto/turnos.py::intervalos_do_turno` · registro 0
  - a entrada atrasou ou a saida adiantou alem da tolerancia? -> `ponto/motor_calculo_v2.py::MotorBase.aplicar_tolerancia` · registro 0
  - o turno previsto cruza a meia-noite? -> `escala/servico_jornada.py::turno_cruza_meia_noite` · registro 0
  - este turno tem dois blocos (e partido)? -> `escala/servico_jornada.py::eh_turno_partido` · registro 0
  - que status (ok, atraso, adianto, grave) tem a lampada do marco? -> `escala/utils.py::status_do_delta` · registro 0
  - o dia esta encerrado? -> `ponto/turnos.py::dia_encerrado` · registro 0
  - o par esta completo? -> `ponto/turnos.py::par_completo` · registro 0
  - ha quanto tempo o turno esta aberto? -> `ponto/turnos.py::turno_aberto_ha` · registro 0
  - que lembrete de fim de turno esta na hora? -> `ponto/turnos.py::lembrete_fim_turno` · registro 0

### ausencia/ferias -- fora_de_autoridade('ausencia/ferias') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0
- padroes proibidos: 6, varridos pelo selo da familia
- selos: core/tests/test_placar_registro_ausencia.py, ponto/tests/test_afastado_hoje.py, ponto/tests/test_contract_juiz_ausencia.py, ponto/tests/test_ferias_na_janela_do_substituto.py
- perguntas:
  - a ausencia paga, desconta ou suprime o dia? -> `ponto/catalogo/ausencias.py::efeito_vigente` · registro 0
  - que dias a ausencia ocupa? -> `ponto/turnos.py::cobertura_ausencia_periodo` · registro 0
  - o lancamento colide com outro fato (recusar)? -> `ponto/precedencia.py::veto_de_lancamento` · registro 0
  - a ausencia exige documento, e em que prazo? -> `ponto/services/ausencia.py::estado_inicial` · registro 0
  - a ausencia esta aberta ou decidida? -> `ponto/services/ausencia.py::ABERTAS` · registro 0
  - a batida do dia esta dentro do periodo que a ausencia parcial declara? -> `ponto/turnos.py::periodo_da_parcial_no_dia` · registro 0
  - o colaborador esta de ferias hoje? -> `ferias/services.py::em_gozo_hoje` · registro 0
  - o colaborador esta afastado hoje? -> `ponto/turnos.py::afastado_hoje` · registro 0
  - qual o saldo do periodo aquisitivo? -> `ferias/models.py::PeriodoAquisitivo.dias_saldo` · registro 0
  - o periodo venceu ou esta vencendo (art.137)? -> `ferias/models.py::status_de_periodo` · registro 0
  - quantos dias ainda podem ser vendidos (abono, 1/3)? -> `ferias/models.py::PeriodoAquisitivo.dias_vendaveis` · registro 0
  - o fracionamento e valido (art.134)? -> `ferias/models.py::AgendamentoFerias.clean` · registro 0
  - o aviso de 30 dias foi dado (art.135)? -> `ferias/situacao.py::DIAS_AVISO_PREVIO` · registro 0
  - quanto a ausencia dura, e o tipo e valido? -> `ponto/catalogo/ausencias.py::fim_de` · registro 0
  - ha 3 ou mais lancamentos medicos na competencia? -> `ponto/services/alertas_ausencia.py::resumo_atestados` · registro 0

### chamado -- fora_de_autoridade('chamado') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0
- padroes proibidos: 3, varridos pelo selo da familia
- selos: chamados/tests/test_c5_canal.py, chamados/tests/test_c5_fabrica.py, chamados/tests/test_c_miudos_1.py, chamados/tests/test_contract_juiz_chamado.py, chamados/tests/test_disputa_fim.py, chamados/tests/test_disputa_s127.py, chamados/tests/test_disputa_so_de_falta.py, chamados/tests/test_o261e_f5b_juiz_porta.py, chamados/tests/test_pergunta_no_app_causas.py, chamados/tests/test_pergunta_viva_no_guarda.py, chamados/tests/test_sla_pelo_juiz.py
- perguntas:
  - o chamado esta vivo? -> `chamados/catalogo/motor.py::VIVOS` · registro 0
  - pode transitar de um estado a outro? -> `chamados/catalogo/motor.py::decidir` · registro 0
  - a premissa do chamado morreu (pode fechar)? -> `chamados/catalogo/premissa.py::premissa_morta` · registro 0
  - quem pode agir no chamado? -> `chamados/utils.py::pode_agir_chamado` · registro 0
  - qual o dia do chamado? -> `chamados/catalogo/modulos.py::data_do_chamado` · registro 0
  - a pergunta da disputa esta viva? -> `chamados/juizes.py::pergunta_viva` · registro 0
  - a disputa pode fechar? -> `chamados/models.py::DisputaSupervisao.pode_fechar` · registro 0
  - o chamado e duplicado (mesmo fato)? -> `chamados/models.py::ChamadoColaborador.abrir` · registro 0
  - o prazo (SLA) do chamado estourou? -> `chamados/catalogo/motor.py::prazo_estourou` · registro 0
  - a cobranca ficou orfa depois da regeneracao? -> `escala/services/regeneracao.py::cobranca_orfa` · registro 0
  - a ausencia do dia suspende, fecha ou levanta a cobranca viva? -> `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia` · registro 0
  - a fabrica de perguntas alcanca o chamado (e se nao, por que)? -> `chamados/services/disputa_emissao.py::motivo_fora_da_fabrica` · registro 0
  - por que o chamado vivo de furo nao tem pergunta no app? -> `chamados/services/pergunta_no_app.py::retencao` · registro 0
  - qual a competencia do chamado? -> `chamados/juizes.py::competencia_do_chamado` · registro 0
  - alguma pergunta desta colecao esta viva? -> `chamados/juizes.py::fio_vivo` · registro 0
  - este dia veta o nascimento/reabertura automatica de chamado de turno? -> `ponto/services/furos_vetados.py::motivo_do_veto` · registro 0
  - por onde alguem chega a este dia (porta alcancavel, com o verbo do painel)? -> `chamados/juizes.py::porta_alcancavel_do_dia` · registro 0

### feriado/prazo -- fora_de_autoridade('feriado/prazo') = 13
- pendentes: sem chamar 10 · chama e deriva 2 · nao sei 1 · [pergunta sem juiz declarado (nao ha quem chamar): 1]
- padroes proibidos: 4, varridos pelo selo da familia
- selos: ponto/tests/test_contract_juiz_feriado.py
- perguntas:
  - o feriado suprime o trabalho do dia? -> `ponto/precedencia.py::fatos_do_dia` · registro 2 (sem chamar 2, chama e deriva 0, nao sei 0)
  - feriado trabalhado paga dobra ou hora simples? -> `ponto/motor_calculo_v2.py::CICLOS_FERIADO_SIMPLES` · registro 6 (sem chamar 6, chama e deriva 0, nao sei 0)
  - a que competencia (corte a corte) pertence o dia? -> `ponto/janelas.py::janela_atual` · registro 3 (sem chamar 2, chama e deriva 1, nao sei 0)
  - a competencia do dia esta fechada (pode escrever)? -> `ponto/janelas.py::competencia_fechada` · registro 0
  - qual o prazo de arquivo/silencio do chamado? -> `chamados/catalogo/motor.py::PRAZO_ARQUIVO_DIAS` · registro 0
  - qual o prazo do DP para responder a justificativa? -> `ponto/catalogo/justificativas.py::prazo_estourou` · registro 1 (sem chamar 0, chama e deriva 1, nao sei 0)
  - [SEM JUIZ] que horas do turno que cruza a meia-noite contam como feriado? · registro 0
  - [SEM JUIZ] o que e dia util? · registro 1 (sem chamar 0, chama e deriva 0, nao sei 1)

### fechamento -- fora_de_autoridade('fechamento') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0
- padroes proibidos: 5, varridos pelo selo da familia
- selos: ponto/tests/test_contract_juiz_fechamento.py
- perguntas:
  - a competencia pode ser aprovada/trancada (o que bloqueia)? -> `folha/export.py::classificar_export` · registro 0
  - quem entra no TXT da folha? -> `folha/export.py::classificar_export` · registro 0
  - quanto da competencia esta cumprido (prontidao)? -> `folha/export.py::prontidao` · registro 0
  - o que "fechado" quer dizer (qual fronteira barra a escrita)? -> `ponto/services/fechamento.py::competencia_trancada` · registro 0
  - qual e a competencia encerrada (a anterior a de hoje)? -> `ponto/janelas.py::janela_anterior` · registro 0
  - a competencia ja foi entregue (lavrada)? -> `folha/export.py::competencia_entregue` · registro 0
  - o periodo foi reaberto? -> `ponto/services/fechamento.py::periodo_trancado` · registro 0
  - que FechamentoMensal e o da competencia de hoje? -> `ponto/janelas.py::janela_atual` · registro 0
  - a competencia esta fechada para a celula deste colaborador (TXT vigente ou holerite publicado)? -> `folha/export.py::competencia_fechada_para_celula` · registro 0

### tela -- fora_de_autoridade('tela') = 110
- pendentes: sem chamar 68 · chama e deriva 5 · nao sei 37 + sitio novo: nao sei (sem varredor) · [nao e python (template/js): 36] · [pergunta sem juiz declarado (nao ha quem chamar): 1]
- padroes proibidos: 0 -- sem varredor de sitio novo: nao sei
- selos: core/tests/test_badge_navbar_consome_particao.py, core/tests/test_contract_juiz_tela.py
- perguntas:
  - quem esta em turno (turno aberto vivo) agora? -> `ponto/turnos.py::turno_aberto_de` · registro 9 (sem chamar 1, chama e deriva 1, nao sei 7)
  - quais sao os turnos do colaborador (pares de batida)? -> `ponto/turnos.py::turnos_do_colab` · registro 9 (sem chamar 7, chama e deriva 0, nao sei 2)
  - o dia acusa (falta, furo, ok) -- o que a celula julgou? -> `escala/servico_jornada.py::classificar_falta` · registro 32 (sem chamar 23, chama e deriva 0, nao sei 9)
  - quantos minutos o dia realizou? -> `ponto/turnos.py::realizado_do_dia` · registro 6 (sem chamar 5, chama e deriva 0, nao sei 1)
  - quais os totais da competencia (horas, HE, noturno, faltas)? -> `folha/export.py::eventos_do_fechamento` · registro 3 (sem chamar 3, chama e deriva 0, nao sei 0)
  - o colaborador entra na folha (e por que nao)? -> `folha/export.py::classificar_export` · registro 7 (sem chamar 3, chama e deriva 0, nao sei 4)
  - qual competencia a tela mostra? -> `ponto/janelas.py::janela_atual` · registro 17 (sem chamar 14, chama e deriva 3, nao sei 0)
  - o chamado ou a pergunta esta vivo na fila? -> `chamados/juizes.py::pergunta_viva` · registro 11 (sem chamar 5, chama e deriva 0, nao sei 6)
  - a ausencia vale no dia (e quanto)? -> `ponto/catalogo/ausencias.py::efeito_vigente` · registro 7 (sem chamar 5, chama e deriva 0, nao sei 2)
  - o periodo de ferias esta vencido ou vencendo? -> `ferias/models.py::status_de_periodo` · registro 5 (sem chamar 2, chama e deriva 1, nao sei 2)
  - o dia oferece "Resolver dia" (a decisao do dia esta pendente na celula)? -> `colaboradores/services/calendario.py::resolver_dia_oferecido` · registro 0
  - [SEM JUIZ] numero de tela sem autoridade declarada (score, faixa de horario, atraso de sincronizacao)? · registro 4 (sem chamar 0, chama e deriva 0, nao sei 4)

### portas -- fora_de_autoridade('portas') = 124
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 124 + sitio novo: nao sei (sem varredor) · [juiz = censo dos proprios sitios (NAO_SE_CHAMA): 124]
- nao sei, por desenho: o juiz e o CENSO que mede a porta (core/portas_da_tela.py::censo) e a porta nao o chama por desenho; ela sai do registro pelo smoke de clique. A TRILHA vem da metade estatica do censo (logo abaixo); uso 14d e quem pode leem o banco e o log de acesso -- nao sei aqui
- portas de escrita no codigo: 168 · portas sem trilha (sem registrar_log no fonte da view): 147 -- core/portas_da_tela.py::portas_do_codigo (a metade estatica do censo), urlconf config.urls_ci
- padroes proibidos: 0 -- sem varredor de sitio novo: nao sei
- selos: chamados/tests/test_smoke_clique_regularizacao_add_item.py, chamados/tests/test_smoke_clique_regularizacao_criar.py, chamados/tests/test_smoke_clique_regularizacao_del_item.py, core/tests/test_contract_portas.py, core/tests/test_smoke_clique_comunicado_confirmar.py, core/tests/test_smoke_clique_comunicado_criar.py, core/tests/test_smoke_clique_comunicado_encerrar.py, core/tests/test_smoke_clique_console_sininho.py, core/tests/test_smoke_clique_criar_setor_ad.py, core/tests/test_smoke_clique_criar_usuario_ad.py, core/tests/test_smoke_clique_excluir_feriado.py, core/tests/test_smoke_clique_feriado_form.py, core/tests/test_smoke_clique_ficha_setor_acao.py, core/tests/test_smoke_clique_ficha_usuario_acao.py, core/tests/test_smoke_clique_grupo_criar.py, core/tests/test_smoke_clique_grupo_salvar.py, core/tests/test_smoke_clique_ia_perguntar.py, core/tests/test_smoke_clique_quadro_setores_api.py, core/tests/test_smoke_clique_vincular_setor_usuario.py, escala/tests/test_smoke_clique_folga_calendario.py, escala/tests/test_smoke_clique_wizard_preview.py, pautas/tests/test_smoke_clique_pauta_cancelar.py, pautas/tests/test_smoke_clique_pauta_escrever.py, pautas/tests/test_smoke_clique_pauta_feita.py, ponto/tests/test_smoke_clique_tipo_ausencia.py
- perguntas:
  - a porta de escrita tem uso humano e smoke de clique? -> `core/portas_da_tela.py::censo` · registro 124 (sem chamar 0, chama e deriva 0, nao sei 124)

### batida -- fora_de_autoridade('batida') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0 + sitio novo: nao sei (sem varredor)
- padroes proibidos: 0 -- sem varredor de sitio novo: nao sei
- selos: ponto/tests/test_contract_juiz_batida.py, ponto/tests/test_juiz_classificar_posicao_nasce.py
- perguntas:
  - que periodos e que intervalo teve este dia? -> `ponto/juiz_batida.py::periodos_do_dia` · registro 0
  - quantas horas este PAR (turno) vale? -> `ponto/motor_calculo_v2.py::MotorBase.calcular_periodo` · registro 0
  - a batida esta dentro do geofence? -> `ponto/services/geofence.py::classificar_posicao` · registro 0
  - a foto da batida esta ausente? -> `ponto/registro_batida.py::selfie_ausente` · registro 0
  - [SEM JUIZ] a batida e espuria (cluster)? · registro 0
  - [SEM JUIZ] o par e relampago? · registro 0

### escala -- fora_de_autoridade('escala') = 0
- pendentes: sem chamar 0 · chama e deriva 0 · nao sei 0 + sitio novo: nao sei (sem varredor)
- padroes proibidos: 0 -- sem varredor de sitio novo: nao sei
- selos: escala/tests/test_contract_juiz_escala.py
- perguntas:
  - qual vinculo vale neste dia? -> `escala/alimentacao.py::vinculo_do_dia` · registro 0
  - o vinculo cobre a data? -> `escala/alimentacao.py::vinculo_cobre_a_data` · registro 0
