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
- config/crons.py::papel, PAPEIS = juiz, vigia, alerta, lavra, agenda, gerador, infra, JUIZES_POR_VARREDURA (divida: esperado 0)

## (e) EVENTO (o sinal acorda o juiz do dia)
- ponto/services/cartorio.py::julgar_celula (juiz UNICO do dia)
- ponto/signals.py::_nucleo_post_save_batida (post_save Batida)
- ponto/signals.py::_realtime_post_save_batida (post_save Batida)
- ponto/signals.py::_nucleo_post_delete_batida (post_delete Batida)
- ponto/signals.py::_reconciliador_post_save_batida (post_save Batida)
- ponto/signals.py::_nucleo_post_save_escala (post_save EscalaColaborador)
- ponto/signals.py::_cartorio_post_save_batida (post_save Batida) -> julga
- ponto/signals.py::_cartorio_post_delete_batida (post_delete Batida) -> julga
- ponto/signals.py::_cartorio_post_save_ausencia (post_save Ausencia) -> julga
- ponto/signals.py::_cartorio_post_delete_ausencia (post_delete Ausencia) -> julga
- chamados/signals.py::disputa_fechada_fecha_chamado_pai (post_save DisputaSupervisao)
- chamados/signals.py::rebaixa_urgencia_ao_resolver (post_save ChamadoColaborador)
- chamados/signals.py::resolve_perguntas_ao_resolver (post_save ChamadoColaborador)
- chamados/signals.py::retoma_regularizacoes_ao_fechar_disputa (post_save DisputaSupervisao)
- chamados/signals.py::materializa_perguntas_ao_fechar_disputa (post_save DisputaSupervisao)
- chamados/signals.py::_cartorio_pre_save_chamado (pre_save ChamadoColaborador)
- chamados/signals.py::_cartorio_post_save_chamado (post_save ChamadoColaborador) -> julga
- escala/signals.py::_ec_capturar_ancora_anterior (pre_save EscalaColaborador)
- escala/signals.py::_ec_vigencia_regenera_celulas (post_save EscalaColaborador)
- escala/signals.py::_ec_invalida_previsto (post_save EscalaColaborador)
- escala/signals.py::_ec_revalidar_disputas_ancora (post_save EscalaColaborador)
- escala/signals.py::_ec_folgas_seguem_a_pessoa (post_save EscalaColaborador)

## (f) JUIZES por familia
- core/juizes.py: JUIZES (pergunta -> autoridade), SEM_JUIZ (motivo), PENDENTES (o registro), fora_de_autoridade(familia) -> contador
- familias: celula/precedencia, turno/marcos, ausencia/ferias, chamado, feriado/prazo, fechamento, tela, portas
- contratos: */tests/test_contract_juiz_*.py (contagem e zonas do registro de cada familia)

## (g) PORTAS DE ESCRITA humanas
- core/portas_da_tela.py::censo (familias: chamados, dia do ponto, ausencia, escala/vinculo, pautas, ferias, folha, cadastro/config) -> manage.py censo_portas

## (h) VIGIAS e contadores
- config/crons.py: papel vigia, AGENDADOS_FORA_DO_CRON, linhas `# contador=... · esperado=... · dono=...`
- host: bin/placar_code.sh (o placar inteiro), bin/vigia_arvore.sh, bin/vigia_esteira.sh -> core/esteira_vigia.py
