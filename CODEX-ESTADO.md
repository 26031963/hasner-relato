# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 11:50** · publicado em **09/11/2026 11:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 11:50 · sessao d03007e09ff6 · RETRATO-POR-EVENTO · censo · origem=Code

O frescor atual pode deixar passar mudanças nas fontes do retrato: ele observa somente o carimbo do fechamento, enquanto o lavrador também lê cadastro, células, vínculos, férias, ausências, rubricas e o fio dos chamados. Porém, “escala e ausência não invalidam” não vale para todos os caminhos desta cópia: existem invalidações indiretas do fechamento. (`app/folha/retrato_folha.py:1298`; `app/escala/signals.py:124`; `app/ponto/services/ausencia.py:941`)

Execução atual dos crons, conteúdo atual da fila e custo atual de produção: **[nao verificado]**.

**1. Fontes do lavrador**

O caminho começa em `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export`; acrescenta `_causas_gravadas` e, para o dono do bloqueio, `alimentar_portas/porta_alcancavel_do_dia`. (`app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1157`; `app/folha/retrato_folha.py:1160`; `app/folha/fora_da_folha.py:119`; `app/folha/fora_da_folha.py:166`; `app/folha/fora_da_folha.py:178`)

O fecho inclui os seguintes caminhos transitivos:

- Retenção → célula → protestos → juiz dos períodos/pausas. (`app/folha/export.py:960`; `app/folha/export.py:1266`; `app/folha/export.py:1253`; `app/folha/export.py:1142`)
- Homologação → eventos do fechamento, ciclo do vínculo, régua CCT e datas de ausência. (`app/folha/export.py:994`; `app/folha/export.py:995`; `app/folha/export.py:996`; `app/folha/export.py:1005`)
- Cadastro zero → previsto → métodos de vínculo/template → folgas e feriados. (`app/folha/export.py:934`; `app/escala/utils.py:2266`; `app/escala/utils.py:2269`; `app/escala/models.py:1286`; `app/escala/models.py:1395`)
- Dono → fila/verbos/pergunta viva → tranca, trilha humana, materialização e batidas. (`app/chamados/juizes.py:1307`; `app/chamados/juizes.py:1310`; `app/chamados/juizes.py:1322`; `app/chamados/juizes.py:1157`; `app/chamados/juizes.py:1088`; `app/chamados/juizes.py:1234`)

A enumeração abaixo distingue **leitura ORM da linha inteira** de consumo individual em Python: consultas sem `only/defer/values` carregam os campos concretos declarados, inclusive campos que a decisão não usa. É por isso que `Colaborador.cpf` aparece no inventário, embora o classificador use nome/código e não consulte o atributo CPF. (`app/folha/export.py:777`; `app/folha/export.py:831`; `app/colaboradores/models.py:319`)

Nas relações, o nome do campo inclui a coluna correspondente `*_id`; `pk` designa a identidade usada nas consultas. As declarações desses campos estão nos respectivos modelos. (`app/colaboradores/models.py:321`; `app/escala/models.py:872`; `app/chamados/models.py:2424`)

Campos carregados por ORM, um por linha:

```text
Empresa.pk -> handle (app/folha/management/commands/lavrar_retrato_folha.py:51)
Empresa.razao_social -> empresas_visiveis (app/colaboradores/queries.py:87)
Empresa.nome_fantasia -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.cnpj -> empresas_visiveis (app/colaboradores/queries.py:86)
Empresa.ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.em_rollout -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.dia_inicio_competencia -> corte_da_empresa (app/ponto/janelas.py:49)
Empresa.janela_he_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_piso_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.limite_decisao_he_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.he_pendente_trava_export -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_desde -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.regime_trabalhista -> regua_para (app/core/regua_cct.py:353)

FechamentoMensal.pk -> classificar_export (app/folha/export.py:777)
FechamentoMensal.colaborador -> classificar_export (app/folha/export.py:779)
FechamentoMensal.mes -> classificar_export (app/folha/export.py:778)
FechamentoMensal.ano -> classificar_export (app/folha/export.py:778)
FechamentoMensal.status -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_trabalhadas -> classificar_export (app/folha/export.py:823)
FechamentoMensal.horas_noturnas -> eventos_do_fechamento (app/folha/export.py:447)
FechamentoMensal.horas_extras -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_50 -> eventos_do_fechamento (app/folha/export.py:441)
FechamentoMensal.horas_extras_50_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_100 -> eventos_do_fechamento (app/folha/export.py:421)
FechamentoMensal.horas_extras_100_feriado -> eventos_do_fechamento (app/folha/export.py:420)
FechamentoMensal.horas_extras_100_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_folga_trabalhada -> eventos_do_fechamento (app/folha/export.py:446)
FechamentoMensal.horas_atraso -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_saida_antecipada -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_falta -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_intra_indenizada -> eventos_do_fechamento (app/folha/export.py:448)
FechamentoMensal.turnos_abertos -> classificar_export (app/folha/export.py:825)
FechamentoMensal.minutos_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_incertos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_abonados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_realizados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.motivos_espelho -> classificar_export (app/folha/export.py:777)
FechamentoMensal.causa_espelho -> _causas_gravadas (app/folha/retrato_folha.py:1115)
FechamentoMensal.previsto_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.derivado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_ok -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_perdido -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_reflexo_dsr -> classificar_export (app/folha/export.py:777)
FechamentoMensal.saldo_banco_horas -> classificar_export (app/folha/export.py:777)
FechamentoMensal.inconsistencias -> classificar_export (app/folha/export.py:960)
FechamentoMensal.aprovado_por -> classificar_export (app/folha/export.py:777)
FechamentoMensal.aprovado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.observacao -> classificar_export (app/folha/export.py:777)
FechamentoMensal.criado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.atualizado_em -> classificar_export (app/folha/export.py:777)

Colaborador.pk -> dias_que_seguram (app/folha/fora_da_folha.py:131)
Colaborador.isento_ponto -> isento_ponto (app/core/constants.py:18)
Colaborador.matricula_hasner -> classificar_export (app/folha/export.py:779)
Colaborador.matricula_nexti -> classificar_export (app/folha/export.py:779)
Colaborador.codigo_dominio -> classificar_export (app/folha/export.py:832)
Colaborador.cod_esocial -> classificar_export (app/folha/export.py:779)
Colaborador.cpf -> classificar_export (app/folha/export.py:779)
Colaborador.nome -> classificar_export (app/folha/export.py:831)
Colaborador.empresa -> classificar_export (app/folha/export.py:778)
Colaborador.posto -> classificar_export (app/folha/export.py:779)
Colaborador.situacao -> classificar_export (app/folha/export.py:802)
Colaborador.data_admissao -> motivos_retencao_celula (app/folha/export.py:1198)
Colaborador.data_demissao -> motivos_retencao_celula (app/folha/export.py:1200)
Colaborador.cargo -> classificar_export (app/folha/export.py:779)
Colaborador.data_nascimento -> classificar_export (app/folha/export.py:779)
Colaborador.genero -> classificar_export (app/folha/export.py:779)
Colaborador.estado_civil -> classificar_export (app/folha/export.py:779)
Colaborador.pis -> classificar_export (app/folha/export.py:779)
Colaborador.rg -> classificar_export (app/folha/export.py:779)
Colaborador.rg_emissor -> classificar_export (app/folha/export.py:779)
Colaborador.telefone -> classificar_export (app/folha/export.py:779)
Colaborador.telefone2 -> classificar_export (app/folha/export.py:779)
Colaborador.email_pessoal -> classificar_export (app/folha/export.py:779)
Colaborador.end_rua -> classificar_export (app/folha/export.py:779)
Colaborador.end_numero -> classificar_export (app/folha/export.py:779)
Colaborador.end_complemento -> classificar_export (app/folha/export.py:779)
Colaborador.end_bairro -> classificar_export (app/folha/export.py:779)
Colaborador.end_cidade -> classificar_export (app/folha/export.py:779)
Colaborador.end_uf -> classificar_export (app/folha/export.py:779)
Colaborador.end_cep -> classificar_export (app/folha/export.py:779)
Colaborador.motivo_desligamento -> classificar_export (app/folha/export.py:779)
Colaborador.usuario -> classificar_export (app/folha/export.py:779)
Colaborador.perfil -> classificar_export (app/folha/export.py:779)
Colaborador.deve_trocar_senha -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_token -> classificar_export (app/folha/export.py:779)
Colaborador.foto -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_em -> classificar_export (app/folha/export.py:779)
Colaborador.notificacao_negada_em -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_motivo -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_completo -> classificar_export (app/folha/export.py:779)
Colaborador.wk_em_operacao -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_pendente -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_em -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_versao -> classificar_export (app/folha/export.py:779)
Colaborador.gps_concedido_em -> classificar_export (app/folha/export.py:779)
Colaborador.camera_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.notif_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.telefone_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.email_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.criado_em -> classificar_export (app/folha/export.py:779)
Colaborador.atualizado_em -> classificar_export (app/folha/export.py:779)
Colaborador.plataforma_principal -> classificar_export (app/folha/export.py:779)

CelulaDia.pk -> motivos_retencao_celula (app/folha/export.py:1221)
CelulaDia.colaborador -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.data -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.trabalha -> motivos_retencao_celula (app/folha/export.py:1255)
CelulaDia.origem -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.escala_geradora -> vinculo_do_dia (app/escala/alimentacao.py:270)
CelulaDia.dna -> periodos_do_dia (app/ponto/juiz_batida.py:158)
CelulaDia.gerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regenerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regeneracoes -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.dna_anterior -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_por -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.julgada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.impressao -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.veredito -> motivos_retencao_celula (app/folha/export.py:1233)
CelulaDia.veredito_via -> _pergunta_viva_calc (app/chamados/juizes.py:1168)
CelulaDia.veredito_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.protestos -> motivos_retencao_celula (app/folha/export.py:1246)
CelulaDia.ata -> periodos_do_dia (app/ponto/juiz_batida.py:157)
CelulaDia.ausencia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_entrada -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_saida -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.insumos_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_dia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_rotulo -> motivos_retencao_celula (app/folha/export.py:1206)

EscalaColaborador.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:155)
EscalaColaborador.colaborador -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.posto -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
EscalaColaborador.tipo_escala -> classificar_export (app/folha/export.py:930)
EscalaColaborador.data_inicio -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.data_fim -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:151)
EscalaColaborador.ativa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:150)
EscalaColaborador.regua_confianca -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_classe -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_avaliada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_dias_baixo -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_por -> classificar_export (app/folha/export.py:930)
EscalaColaborador.folga_calendario -> classificar_export (app/folha/export.py:930)
EscalaColaborador.trabalha_em_feriado -> eh_dia_trabalho_calculado (app/escala/models.py:1390)
EscalaColaborador.data_ancora_colaborador -> classificar_export (app/folha/export.py:801)
EscalaColaborador.origem_ancora -> classificar_export (app/folha/export.py:930)
EscalaColaborador.chamado_ancora -> classificar_export (app/folha/export.py:930)

TipoEscala.pk -> classificar_export (app/folha/export.py:930)
TipoEscala.codigo -> classificar_export (app/folha/export.py:930)
TipoEscala.descricao -> classificar_export (app/folha/export.py:930)
TipoEscala.minutos_jornada -> classificar_export (app/folha/export.py:930)
TipoEscala.hora_inicio -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_fim -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_inicio_intervalo -> marcos_do_dia (app/escala/models.py:344)
TipoEscala.hora_fim_intervalo -> marcos_do_dia (app/escala/models.py:345)
TipoEscala.hora_inicio_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.hora_fim_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.noturno -> classificar_export (app/folha/export.py:930)
TipoEscala.permite_hora_extra -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_antes_min -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_depois_min -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_calendario_padrao -> classificar_export (app/folha/export.py:930)
TipoEscala.intervalo_modo -> _intervalo_livre (app/escala/models.py:250)
TipoEscala.intervalo_indenizavel -> periodos_do_dia (app/ponto/juiz_batida.py:310)
TipoEscala.intervalo_duracao_min -> intervalo_cadastrado (app/ponto/juiz_batida.py:143)
TipoEscala.apelido -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_ciclo -> classificar_export (app/folha/export.py:800)
TipoEscala.ciclo_dias -> classificar_export (app/folha/export.py:930)
TipoEscala.is_template -> classificar_export (app/folha/export.py:930)
TipoEscala.template_nome -> classificar_export (app/folha/export.py:930)
TipoEscala.data_ancora -> classificar_export (app/folha/export.py:930)
TipoEscala.dias_trabalho_semana -> eh_dia_trabalho (app/escala/models.py:831)
TipoEscala.template_pai -> classificar_export (app/folha/export.py:930)
TipoEscala.nivel -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_base -> classificar_export (app/folha/export.py:930)
TipoEscala.editavel -> classificar_export (app/folha/export.py:930)
TipoEscala.perfil_apuracao -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_dia_semana -> eh_dia_trabalho (app/escala/models.py:838)
TipoEscala.horarios_por_dia -> _override_do_dia (app/escala/models.py:227)

Posto.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.nome -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.empresa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.tolerante_offline -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.praca -> regua_para (app/core/regua_cct.py:330)
Posto.municipio -> lugar_do_vinculo (app/core/feriados.py:122)
Posto.endereco -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.numero -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.complemento -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.bairro -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cidade -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cep -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.opera_em_feriado -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_minima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_maxima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.latitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.longitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.raio_metros -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.ativo -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.revisar -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)

Praca.pk -> regua_para (app/core/regua_cct.py:330)
Praca.nome -> regua_para (app/core/regua_cct.py:330)
Praca.cidade -> regua_para (app/core/regua_cct.py:330)
Praca.estado -> regua_para (app/core/regua_cct.py:330)
Praca.sindicato -> regua_para (app/core/regua_cct.py:330)
Praca.obs -> regua_para (app/core/regua_cct.py:330)
Praca.adicional_noturno_percentual -> regua_para (app/core/regua_cct.py:330)
Praca.tolerancia_minutos -> regua_para (app/core/regua_cct.py:330)
Praca.banco_horas_prazo_dias -> regua_para (app/core/regua_cct.py:330)
Praca.ativa -> regua_para (app/core/regua_cct.py:330)

Municipio.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.codigo_ibge -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.nome -> _lugar (app/core/feriados.py:53)
Municipio.uf -> _lugar (app/core/feriados.py:52)

Feriado.pk -> feriados_de (app/core/feriados.py:83)
Feriado.nome -> feriados_de (app/core/feriados.py:83)
Feriado.data -> _qs (app/core/feriados.py:29)
Feriado.abrangencia -> alcanca (app/core/feriados.py:60)
Feriado.uf -> alcanca (app/core/feriados.py:63)
Feriado.municipio -> alcanca (app/core/feriados.py:66)
Feriado.fonte -> feriados_de (app/core/feriados.py:83)

AplicacaoConvencao.pk -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.sindicato -> _aplicacao_vigente (app/core/regua_cct.py:255)
AplicacaoConvencao.empresa -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.praca -> _aplicacao_vigente (app/core/regua_cct.py:256)
AplicacaoConvencao.ativo -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.criado_em -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.criado_por -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.motivo -> _aplicacao_vigente (app/co

[truncado em 20000 de 69455 caracteres]

## 09/11 11:41 · sessao 6d93743673c9 · - · - · origem=Ronald (tmux)

Fecho confirmado por AST em memória e inspeção de métodos:

- `classificar_export → motivos_retencao_espelho → motivos_retencao_celula → nao_retem_pela_pausa → periodos_do_dia`. (`app/folha/export.py:960`, `:1266`, `:1253`, `:1165`, `:1142`)
- `classificar_export → _codigos_nao_homologados → eventos_do_fechamento / _ciclo_do_colab / feriado_12x36_em_dobra / datas_falta → regua_para / alimentacao / TipoAusencia / Ausencia`. (`app/folha/export.py:966`, `:994`, `:995`, `:996`, `:1005`, `:380`, `:359`, `:180`)
- `classificar_export → minutos_previstos_do_dia → EscalaColaborador.eh_dia_trabalho / marcos_do_dia / pausas_do_dia → TipoEscala / FolgaDia / Feriado`. (`app/folha/export.py:934`; `app/escala/utils.py:2266`, `:2269`, `:2291`; `app/escala/models.py:1096`, `:1133`, `:1208`, `:1286`, `:1395`)
- `fora_da_folha → dias_que_seguram → classificar_export`; depois `alimentar_portas → particionar_verbos / qs_na_fila / pergunta_viva`; `porta_alcancavel_do_dia → chamado_que_lista / perguntas_do_app`. (`app/folha/fora_da_folha.py:161`, `:119`, `:166`; `app/chamados/juizes.py:1307`, `:1310`, `:1322`, `:1399`, `:1400`, `:1339`)
- `pergunta_viva → dia_da_pergunta_trancado → periodo_trancado / _toque_humano_no_dia → LogAuditoria / User`; outros ramos chegam a `classificar_falta`, materialização e `turnos_do_colab`, lendo batidas, células, vínculos e templates. (`app/chamados/juizes.py:1157`, `:964`, `:971`, `:1087`, `:1088`, `:1221`, `:1234`, `:384`)

A resolução sintática preliminar alcançou 178 funções; esse número exclui expansão posterior de métodos de instância, reexports e callbacks. Portanto, não equivale a prova de 178 funções no fecho completo. `[nao verificado]`

`prontidao`, `grade_do_fechamento`, `get_motor_cct`, `EmpresaIntegracao`, `ExportacaoDominio` e `Holerite` não pertencem aos caminhos inspecionados dessas raízes; `grade_da_celula` entra por `_tipo_pelo_marco`, separadamente. (`app/folha/export.py:757`, `:986`; `app/chamados/services/materializacao.py:282`)

Foram enviados ao principal os inventários detalhados de campos, o mapa de leituras de linhas completas e os relógios. Há uma escrita potencial dentro desse fecho: `get_user_sistema()` usa `User.objects.get_or_create(username='sistema')`; nenhum código da aplicação foi executado nesta inspeção. (`app/core/utils.py:18`; `app/chamados/juizes.py:1087`)

## 09/11 11:41 · sessao 8a265e5c5b41 · - · - · origem=Ronald (tmux)

O-OUTBOX verificado na cópia:

- Modelo `EventoCelula`: campos `celula_id`, `colaborador_id`, `data`, `criado_em`, `processado_em`, `tentativas`, `erro`; sem payload e sem índice único de pendente. Não contém empresa nem competência gravadas (`app/escala/models.py:1845-1871`).
- Escritor único: `emitir_evento_celula`; faz `EventoCelula.objects.create(celula_id=..., colaborador_id=..., data=...)` (`app/ponto/portas/celula.py:882-891`).
- `salvar_e_emitir` grava célula e evento no mesmo `atomic`; somente mudanças de `veredito` ou `ata` acordam o consumidor (`app/ponto/portas/celula.py:878-902`).
- Consumidor único de produção: comando `consumir_eventos_celula`, com advisory lock por schema; cron a cada minuto (`app/chamados/management/commands/consumir_eventos_celula.py:17-31`; `app/chamados/reconciliador.py:1215-1242`; `app/config/crons.py:393-394`).
- Agrupa eventos por colaborador dentro de cada lote de até 500; não é debounce por empresa×competência. Roda lotes até esvaziar ou ultrapassar 45 s (`app/chamados/reconciliador.py:1209-1211`; `app/chamados/reconciliador.py:1342-1352`; `app/chamados/reconciliador.py:1402-1404`).
- Consumidores funcionais: rejunção, retratação de perguntas absolvidas, retratação de chamados absolvidos, premissa morta, órfãs pela lâmpada e fechamento por lastro (`app/chamados/reconciliador.py:1363-1383`).
- Falha mantém pendente, incrementa tentativas e guarda erro; após cinco tentativas sai do universo vivo; processados são apagados após sete dias (`app/chamados/reconciliador.py:1209-1210`; `app/chamados/reconciliador.py:1246-1255`; `app/chamados/reconciliador.py:1388-1404`).
- Semeadura evita pendente vivo já existente por célula; a emissão ordinária não deduplica (`app/chamados/reconciliador.py:1278-1310`; `app/ponto/portas/celula.py:891`).

Emissores já existentes:

- `CelulaDia.lavrar_veredito`, `carimbar` e `carimbar_precedencia`: emitem condicionalmente pela interseção com veredito/ata; alteração apenas de status não emite (`app/ponto/portas/celula.py:1003`; `app/ponto/portas/celula.py:1013-1020`; `app/ponto/portas/celula.py:1035`; `app/ponto/portas/celula.py:1067`; `app/ponto/portas/celula.py:1082`).
- `ChamadoColaborador.abrir` e `criar`: emitem quando nasce chamado de módulo de turno com célula ligada (`app/chamados/models.py:971-980`; `app/chamados/models.py:1093-1099`).
- `recompute_turnos`: emite para turnos anteriormente abertos que fecharam, se existir célula para o dia (`app/ponto/nucleo.py:57-66`; `app/ponto/nucleo.py:86-93`).
- `PerguntaDisputa.save`: atualização existente emite via receiver quando toca `CAMPOS_QUE_ACORDAM`; nascimento não emite. Save e receiver estão no mesmo `atomic` (`app/chamados/models.py:3220-3222`; `app/chamados/signals.py:237-244`; `app/chamados/models.py:3413-3423`).
- `reabrir_para_correcao`: update em lote emite explicitamente depois da escrita; `descarimbar_em_lote`, isoladamente, não emite (`app/chamados/services/disputa_emissao.py:1602-1630`; `app/chamados/models.py:3252-3266`).
- `PerguntaDisputa.acordar_celulas` deduplica as células apenas dentro da chamada, pela PK; usa FK da pergunta ou busca pelo par colaborador/data (`app/chamados/models.py:3231-3248`).
- Batidas e ausências podem emitir indiretamente: seus saves/deletes chamam cartório, que lavra a célula; somente mudança de ata/veredito gera evento (`app/ponto/signals.py:85-93`; `app/ponto/signals.py:131-152`; `app/ponto/portas/celula.py:878-902`).
- Mudança do status do chamado pode emitir indiretamente pelo rejulgamento da célula; alterações apenas de contexto/texto/urgência não passam nessa guarda (`app/chamados/signals.py:187-203`).
- `DisputaSupervisao.fechar` pode emitir indiretamente ao fechar chamado-pai/materializar perguntas; não possui INSERT próprio no outbox (`app/chamados/signals.py:18-45`; `app/chamados/signals.py:147-163`).

Correções relevantes ao aval:

- Escala não pode ser classificada genericamente como “não invalida”: pós-save de `EscalaColaborador` chama `invalidar_previsto`, que atualiza `FechamentoMensal.atualizado_em` quando `previsto_em` não é nulo (`app/escala/signals.py:124-133`; `app/ponto/services/fechamento.py:1607-1613`).
- `salvar_tipo` também invalida fechamento após regeneração de frota (`app/escala/services/cadastro_tipo.py:351-359`).
- Ausência também não pode ser classificada genericamente como “não invalida”: edição e decisão chamam `invalidar_fechamento`; o update só alcança registros com `derivado_em` não nulo (`app/ponto/services/ausencia.py:518-524`; `app/ponto/services/ausencia.py:941-948`; `app/ponto/services/fechamento.py:1652-1656`).
- `TipoEscalaAdmin` é escritor fora da porta: usa `admin.ModelAdmin` editável; `EscalaColaboradorAdmin` usa `SoLeitura`. O admin não chama o serviço `salvar_tipo` que faz a propagação/invalidação (`app/escala/admin.py:7-13`; `app/escala/services/cadastro_tipo.py:306-359`).
- `CatalogoDominio`: escritor Python não encontrado; escritor de produção `[nao verificado]`. Modelo/leitor existem (`app/folha/models.py:248-258`; `app/folha/export.py:259-260`).

Inventário de escritores enviado nas mensagens anteriores: Empresa/Colaborador/EscalaColaborador; FolgaDia/calendários/agenda; Ausencia/TipoAusencia; AgendamentoFerias/PeriodoAquisitivo; ChamadoColaborador/PerguntaDisputa/DisputaSupervisao; FechamentoMensal/PeriodoFechado/RubricaDominio. O censo AST da cópia foi executado sem escrita; ele próprio declara que saves de parâmetros, forms e modelos dinâmicos podem permanecer não atribuíveis, portanto não prova completude sozinho (`app/core/censo_escritas.py:43-56`).

## 09/11 11:39 · sessao 9d3a5d3f52b3 · - · - · origem=Ronald (tmux)

Resultado das questões 4–6, somente leitura:

**4. Frescor e invalidação**

- O juiz compara `Max(FechamentoMensal.atualizado_em)` por empresa/mês/ano com `RetratoFolha.lavrado_em`; fechamento posterior significa `desatualizado`. O universo consultado contém apenas competências atual e anterior das empresas visíveis (`app/folha/retrato_folha.py:1287-1306`).
- A view lê esse juiz e recorta avisos para a competência e empresas exibidas, sem comparar carimbos por conta própria (`app/folha/views_retrato.py:199-210`).
- Estados do juiz: `nunca lavrado`, `desatualizado`, `atualizado`; suas frases vêm de `_rotulo_admin` (`app/folha/retrato_folha.py:1276-1277`, `app/folha/retrato_folha.py:1323-1329`).
- A barra mostra o aviso de desatualização ou `atualizado HH:MM`; sem retrato, a lista mostra “Sem retrato lavrado”. Nesse fluxo não há estado `atualizando` ou `falhou` (`app/templates/folha/_retrato_barra.html:19-22`, `app/templates/folha/_retrato_lista.html:17-20`).
- `FechamentoMensal.atualizado_em` é `auto_now=True` (`app/ponto/models.py:479`).
- Escritores que movem o carimbo: criação/lavra por `recalcular_fechamento_mes` (`app/ponto/services/fechamento.py:898-900`, `app/ponto/services/fechamento.py:1143-1161`); `aprovar` com save completo (`app/ponto/services/fechamento.py:1335-1340`); `restaurar_fechamento` (`app/ponto/services/fechamento.py:1392-1393`); `reabrir_periodo` (`app/ponto/services/fechamento.py:1449-1452`); `invalidar_previsto` (`app/ponto/services/fechamento.py:1610-1613`); `invalidar_fechamento` (`app/ponto/services/fechamento.py:1652-1656`).
- Escritores que escrevem fechamento **sem mover esse carimbo**: `aprovar_lote` faz update sem `atualizado_em`; `reabrir_lote` também; `reabrir` faz save com `update_fields` que o omite (`app/ponto/services/fechamento.py:1329-1332`, `app/ponto/services/fechamento.py:1344-1354`). Portanto, o comentário “sobe em TODO save” precisa dessa ressalva (`app/folha/retrato_folha.py:1264`).
- A afirmação genérica do aval de que escala/veredito/ausência não invalidam não descreve todos os caminhos desta cópia: save de `EscalaColaborador` chama `invalidar_previsto` por signal (`app/escala/signals.py:124-133`); edição e decisão de ausência chamam `invalidar_fechamento` (`app/ponto/services/ausencia.py:509-524`, `app/ponto/services/ausencia.py:935-948`); regeneração de células pode recalcular por competência (`app/ponto/portas/celula.py:445-458`).
- Essas invalidações são condicionais: `invalidar_previsto` só atualiza linhas com `previsto_em` preenchido; `invalidar_fechamento` só atualiza linhas com `derivado_em` preenchido (`app/ponto/services/fechamento.py:1611-1613`, `app/ponto/services/fechamento.py:1652-1656`).
- `recalcular_por_evento` agenda no `on_commit` a lavra do fechamento do colaborador na competência do fato; falha gera warning, sem propagar exceção (`app/ponto/services/fechamento.py:1798-1827`).
- Chamadores dessa rederivação incluem batida, decisão de HE, folga aceita e validação de disputa (`app/ponto/registro_batida.py:177-178`, `app/ponto/portas/he.py:201-202`, `app/chamados/services/validacao.py:360-361`, `app/chamados/services/validacao.py:445-446`).
- A família declara `ponto/services/fechamento.py` como porta única; o teste de censo AST tem allowlist e pendentes vazios e verifica escritores externos (`app/folha/tests/test_chokepoint_folha_gate.py:6-9`, `app/folha/tests/test_chokepoint_folha_gate.py:28-31`, `app/folha/tests/test_chokepoint_folha_gate.py:58-62`, `app/folha/tests/test_chokepoint_folha_gate.py:72`).

**5. Custo**

- O comando aceita uma empresa e competência explícitas; sem os filtros, percorre todas as empresas visíveis e competências anterior/atual. Portanto, suporta a unidade empresa × competência, mas a execução padrão é frota × duas competências (`app/folha/management/commands/lavrar_retrato_folha.py:38-40`, `app/folha/management/commands/lavrar_retrato_folha.py:48-65`).
- A configuração do cron usa `--apply` sem `--empresa`; o comentário confirma uma corrida após todas as empresas do gatilho (`app/config/crons.py:1606-1609`, `app/config/crons.py:1627-1629`).
- Cada `montar_linhas(empresa, mes, ano)` chama `fora_da_folha` uma vez; esta chama `dias_que_seguram`, que chama `classificar_export` uma vez. Assim, **uma chamada a `classificar_export` por empresa × competência**, oito no exemplo de quatro empresas × duas competências (`app/folha/retrato_folha.py:1156-1160`, `app/folha/fora_da_folha.py:160-162`, `app/folha/fora_da_folha.py:118-119`).
- Produção histórica documentada: **17,8 s para oito retratos**, quatro empresas × duas competências, em 11/10; os subtotais são por empresa com duas competências: emp1 1,31 s; emp2 11,04 s; emp3 3,77 s; emp4 1,65 s. Não constituem medição por empresa × competência (`app/config/crons.py:1591-1594`; repetido na docstring `app/folha/management/commands/lavrar_retrato_folha.py:21-22`).
- Outra execução registrada informa **12 s para oito retratos**, atuais/anterior, às 02:52 (`app/docs/RELATO.md:5193`); outra passagem registra oito retratos em 12 s pela via homologada, carimbo de 21/10 18:49:53 (`app/docs/RELATO.md:5533-5534`).
- Há medição **unitária de sombra**, não produção nem tempo integral do lavrador: `fora_da_folha` real de emp2 × competência 11 passou de 7,3 s para 32,0 s no ensaio F5C de 30/10 (`app/docs/RELATO.md:1132`).
- Há ensaio de sombra que registra **10,8 s para o ATO3 `lavrar_retrato_folha`**, com aviso de ambiente de teste em quatro núcleos; não é medida unitária de produção (`app/docs/RELATO.md:4764`).
- `crons_duracao.json` contém **39 s** para o comando (`app/config/crons_duracao.json:49`). Isso é valor de agendamento calculado por **p95 das corridas dos últimos sete dias × 1,5, piso de 30 s**, não duração medida de uma unidade (`bin/crons_duracao.py:9-13`, `bin/crons_duracao.py:69-76`). O arquivo informa fonte `app/logs/placar.jsonl`, geração em 31/10, janela sete dias (`app/config/crons_duracao.json:106-110`).
- O teste comprova uma linha por empresa/competência e idempotência, não benchmark de produção (`app/folha/tests/test_retrato_folha_contador.py:265-280`).
- Custo atual medido em produção por empresa × competência: **[nao verificado]**. Não dividir os totais de oito retratos e apresentar a média como medição unitária.

**6. Lei escrita**

- O48 está registrada como fechamento = leitura da célula soberana em qualquer período; “recalcular deixa de existir”, somente atos persistem (`app/docs/CORTES.md:174`, `app/docs/PROMPTS.md:1135`).
- A etapa do retrato declara fonte existente, universo por empresa e competência, retrato gravado em lote e proibição de calcular ao abrir a tela; chama isso de complemento da O48, sem lei nova (`app/docs/PROMPTS.md:1156`).
- Rodar fora do horário do cron já é permitido pela regra de cron idempotente que “pode ser adiantado na mão” (`CLAUDE.md:246`); há aval específico para relavrar imediatamente pela porta e conferir com `classificar_export` (`app/docs/PROMPTS.md:1339`).
- O padrão de evento/outbox e projeções já inclui retrato: `PLANO-PADROES` nomeia espelho, DiaPago, fechamento, retrato e pílulas como projeções reconstruíveis, além do outbox fora da pilha da batida (`app/docs/CORTES.md:288`).
- O contrato outbox aprovado prescreve evento na mesma transação do veredito e consumidor idempotente a cada minuto; isso é a autorização escrita do mecanismo existente, não prova de que já consome eventos para relavrar retrato (`app/docs/CORTES.md:351`).
- O aval de 22/10 registra invalidação por evento, depois recolocada após 20/20; é registro de ordem, não proibição permanente de lavra fora do cron (`app/docs/PROMPTS.md:1339`, `app/docs/PROMPTS.md:1341`).
- Substituir o anterior está implementado pelo escritor único com `update_or_create(empresa, mes, ano)` (`app/folha/management/commands/lavrar_retrato_folha.py:74-76`), restrição única no banco (`app/folha/models.py:362-364`) e teste de segunda lavra preservando a mesma PK e uma linha (`app/folha/tests/test_retrato_folha_contador.py:270-280`).
- A docstring explicita “SUBSTITUI, um por empresa+competência” (`app/folha/models.py:314-317`); a lei geral exige um escritor por entidade e idempotência “2x = mesmo estado, 1 linha de trilha, 0 duplicata” (`CLAUDE.md:173-175`).
- A L-121 manda reconstruir projeções pela porta quando pousa cura de juiz, observando competência exportada e DIFF/reversão (`app/docs/LEIS.md:142`).
- Para as duas perguntas delimitadas — execução fora do cron e substituição por empresa/competência — não identifiquei pergunta adicional de lei: as regras acima já cobrem ambas. A exigência de não propor lei nova por iniciativa própria está expressa (`app/docs/LEIS.md:138`, `CLAUDE.md:266-270`).

Sugestão de fechamento do laudo consolidado: `perguntas de lei: 0`, seguido de lista vazia ou “Nenhuma”, com as referências acima.

## 09/11 11:15 · sessao 8db4566726fa · O10-D-PERMUTA-NA-TELA · censo · origem=Code

1) **Há entradas genéricas pelo app/PWA; pedido próprio de permuta, com o par estruturado, não foi confirmado [nao verificado].** `Ausencia` guarda uma pessoa e um período; `ChamadoColaborador` guarda solicitação textual e contexto JSON, sem campos próprios para os dois lados da troca (`app/ponto/models.py:743`; `app/ponto/models.py:786`; `app/chamados/models.py:38`).

| Entrada existente | Endpoint, view e gravação |
|---|---|
| Ausência pelo app | `GET/POST /api/ausencias/` → `api_ausencias`; o POST chama `criar_ausencia`, identificando o colaborador e `registrado_por=request.user` (`app/api/urls.py:82`; `app/api/views.py:1700`; `app/api/views.py:1836`). |
| Ausência pelo PWA | `/ponto/ausencias/submeter/` → `submeter_ausencia`; chama a mesma porta e renderiza `ponto/submeter_ausencia.html` (`app/ponto/urls.py:64`; `app/ponto/views.py:2467`; `app/ponto/views.py:2536`; `app/ponto/views.py:2560`). |
| Solicitação textual pelo app | `POST /api/chamados/abrir/` → `api_abrir_chamado`; grava assunto, descrição, classificação, urgência e `modulo_origem='app_colaborador'` (`app/api/urls.py:87`; `app/api/views.py:2153`; `app/api/views.py:2201`). |
| Solicitação textual pelo PWA | `/chamados/abrir/` → `abrir_chamado`, serviço `abrir_chamado_local`, template `chamados/form_abertura.html` (`app/chamados/urls.py:10`; `app/chamados/views.py:147`; `app/chamados/views.py:196`). |

A taxonomia dos chamados contém **“Escala / Turno” → “Troca de turno”**, permitindo um pedido textual dessa natureza; isso não constitui ligação estruturada entre duas cartas ou efetivação da troca (`app/chamados/classificador.py:38`; `app/api/views.py:2201`).

`Ausencia` contém colaborador, tipo, `data/data_fim`, minutos, dias corridos, horários de cobertura parcial, descrição, documento, prazo, chamado de origem, documento de referência, status, autor e carimbos de aprovação/criação (`app/ponto/models.py:743`; `app/ponto/models.py:768`; `app/ponto/models.py:776`; `app/ponto/models.py:799`). O tipo aceito vem do catálogo vivo; a disponibilidade real de `troca_de_plantao` nesse cadastro é **[nao verificado]**, e esse código não consta das choices estáticas do modelo (`app/ponto/services/ausencia.py:193`; `app/api/views.py:1883`; `app/ponto/models.py:661`).

| Estado de `Ausencia` | Quem escreve no fluxo encontrado |
|---|---|
| `aguardando_documento` | `criar_ausencia` usa `estado_inicial`: tipo exige documento e não há anexo (`app/ponto/services/ausencia.py:333`; `app/ponto/services/ausencia.py:790`). |
| `aguardando_decisao` | Criação com documento ou sem exigência dele; `anexar_documento` também transfere para esse estado; o escalonador faz isso quando vence o prazo documental (`app/ponto/services/ausencia.py:790`; `app/ponto/services/ausencia.py:873`; `app/ponto/services/ausencia.py:999`). |
| `aprovada` / `rejeitada` | `decidir`, ato do DP, chama o escritor de status `mudar_estado` (`app/ponto/services/ausencia.py:808`; `app/ponto/services/ausencia.py:897`). |

App e PWA também abrem o chamado `ausencia_colab`, com `ausencia_id`, data, tipo e presença de documento; ele avisa o DP e leva ao registro, sem decidir a ausência (`app/api/views.py:1871`; `app/ponto/views.py:2546`; `app/ponto/services/ausencia.py:637`; `app/ponto/services/ausencia.py:659`).

Os chamados possuem os estados genéricos `aberto`, `em_analise`, `resolvido`, `fechado`, `cancelado`, `registrado` e `superado`; nascem pela fábrica `criar`, e a escrita posterior de `status_local` converge em `_gravar_estado`, incluindo ações de gestão por `transicionar` (`app/chamados/models.py:28`; `app/chamados/models.py:1054`; `app/chamados/models.py:201`; `app/chamados/services/acoes_chamado.py:313`).

| Quem lê hoje no código | Registros consumidos |
|---|---|
| App | GET das últimas 30 ausências do próprio colaborador, com status e descrição; API de chamados visíveis do colaborador (`app/api/views.py:1876`; `app/api/views.py:1031`). |
| Admin/DP | Lista de ausências, com filtros de tipo/status e ações; painel de chamados (`app/ponto/views.py:2155`; `app/ponto/urls.py:53`; `app/chamados/services/painel.py:96`). |
| PWA “Meu Atendimento” | Solicitações e pendências do colaborador; a abertura de novas solicitações depende de `app_solicitacoes`, cujo valor real é **[nao verificado]** (`app/chamados/views.py:612`; `app/chamados/services/solicitacoes_no_app.py:1`). |
| API mensageria/DP | Ausências, contagens por tipo/status e fila aguardando decisão (`app/api/views_mensageria.py:736`). |
| Automação documental | `escalonar_documentos_ausencia` lê ausências aguardando documento; fechamento de chamados consulta a ausência associada (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25`; `app/ponto/services/ausencia.py:992`; `app/ponto/services/ausencia.py:686`). |
| Automação de chamados | `alertar_chamados_sla` lê a fila de chamados e trata vencimentos (`app/chamados/management/commands/alertar_chamados_sla.py:20`; `app/chamados/management/commands/alertar_chamados_sla.py:44`). |

A API de perguntas permite responder uma `PerguntaDisputa` já existente do colaborador; um caminho que transforme essa resposta em par de permuta é **[nao verificado]** (`app/api/views.py:2616`; `app/api/views.py:2643`). Pedidos efetivamente existentes e execução desses crons em produção são **[nao verificado]**.

2) **A porta efetiva duas edições de dia; não faz uma permutação de quatro células.** Ela põe `(cedente, data da carta cedente)` como folga e `(substituto, data da carta substituto)` como trabalho; não grava automaticamente o movimento inverso nas outras duas combinações pessoa/data (`app/ponto/portas/celula.py:673`).

A assinatura é (`app/ponto/portas/celula.py:632`):

```python
trocar_plantao(
    ausencia_cedente, ausencia_substituto, *,
    autor, motivo, barrados=None, apesar_da_lavra=''
)
```

| Argumento | Significado demonstrado |
|---|---|
| `ausencia_cedente` | Instância persistida de `Ausencia`; sua pessoa/data serão editadas para `trabalha=False` (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`). |
| `ausencia_substituto` | Outra `Ausencia`; sua pessoa/data serão editadas para `trabalha=True` (`app/ponto/portas/celula.py:659`; `app/ponto/portas/celula.py:673`). |
| `autor` | Objeto com `pk`, usado como autor da edição e da trilha (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:686`). |
| `motivo` | Texto obrigatório após `strip`, incluído nas trilhas (`app/ponto/portas/celula.py:648`; `app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:691`). |
| `barrados` | Lista opcional que recebe motivos de recusa, incluindo lavra e falta de vínculo (`app/ponto/portas/celula.py:571`; `app/ponto/portas/celula.py:586`; `app/ponto/portas/celula.py:694`). |
| `apesar_da_lavra` | Valor repassado aos dois lados; quando verdadeiro, pula a guarda de lavra e fica registrado na trilha (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:614`; `app/ponto/portas/celula.py:668`). |

As cartas são relidas do banco com trava, usando seus `pk`; a porta exige `documento` preenchido, mas não verifica conteúdo ou existência física do arquivo (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:664`). Nos chamadores de teste, vêm de `Ausencia.objects.create`, com tipo `declaracao` e status `aguardando_decisao`; a montagem do par por um chamador de produção presente nesta cópia é **[nao verificado]** (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`).

| Condição | Comportamento atual da porta |
|---|---|
| Autor ausente / motivo vazio | Levanta `ValueError`; não retorna `recusada` nesses casos (`app/ponto/portas/celula.py:646`). |
| Um lado ausente, registro apagado, mesma pessoa ou falta de uma carta | Retorna `recusada`, com motivo em `barrados` quando fornecido (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:694`). |
| Interjornada de 11h | **Não há checagem de descanso ou consulta aos turnos vizinhos** no corpo da troca/edição (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:646`). |
| Sobreposição de turnos | **Não há checagem geométrica de sobreposição** antes das edições (`app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:675`). |
| Competência exportada/lavrada | Cada lado passa pela guarda de `editar_dia`; se barrar, o par retorna `recusada` e desfaz o outro lado (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:675`). |
| Alcance da lavra | Inclui TXT vigente **ou holerite publicado do próprio colaborador**; não equivale simplesmente a `FechamentoMensal.status` (`app/ponto/management/commands/gerar_celulas.py:126`; `app/folha/export.py:589`). |
| Permissão | Exige identificação de autor, mas não verifica `is_staff`, perfil, `tem_acao` ou permissão contextual (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`). |
| Vínculo | Recusa se o juiz do vínculo do dia não devolver vínculo; não exige que os dois tenham o mesmo vínculo, tipo de escala ou empresa (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`). |
| Datas iguais/diferentes | Não compara as datas. Há testes escritos esperando sucesso tanto na mesma data quanto em datas/competências diferentes (`app/ponto/portas/celula.py:673`; `app/ponto/tests/test_o10b_editar_dia_troca.py:178`; `app/ponto/tests/test_o10b_editar_dia_troca.py:260`). |
| Tipo/status/período das cartas | Não exige tipo específico ou aprovação; usa somente `Ausencia.data`, sem recusar `data_fim > data` (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:675`; `app/docs/BACKLOG.md:1876`). |

Portanto, dos quatro controles citados no aval, **interjornada, sobreposição e permissão do autor não são recusas implementadas**; há somente identificação do autor. A guarda de competência lavrada existe, com a abertura explícita por `apesar_da_lavra` (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:675`).

O substituto recebe os marcos do **próprio vínculo/template naquela data**, não os horários copiados do cedente; o produtor `montar_dna` congela esses marcos (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:591`; `app/ponto/management/commands/gerar_celulas.py:73`).

A transação principal do par é **um `atomic` externo**, contendo as duas chamadas de `editar_dia`; cada edição possui seu `atomic` aninhado, e cada gravação de auditoria usa outro savepoint. Não são dois commits independentes para os lados (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:592`; `app/core/observ.py:133`). As cartas e células são travadas; as células seguem ordem por `(colaborador_id, data)` (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:593`; `app/ponto/portas/celula.py:673`).

Quando ambos mudam, grava duas edições e suas trilhas `editar_dia`, mais uma trilha `trocar_plantao`; se ambos já estiverem iguais, retorna `igual` sem essas novas gravações. Falha obrigatória de trilha levanta exceção e desfaz o ato (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:611`; `app/ponto/portas/celula.py:619`; `app/ponto/portas/celula.py:679`; `app/ponto/portas/celula.py:686`).

Há efeitos adicionais condicionais: marca de cobrança órfã no mesmo ato; rejulgamento de dias já alcançados pelo relógio, salvo vereditos humanos; agendamento do recálculo financeiro, rejunção e reavaliação de falta após commit. Assim, **o conjunto inteiro de efeitos não cabe numa quantidade fixa de transações**, embora o par editado tenha uma transação principal (`app/ponto/portas/celula.py:625`; `app/ponto/portas/celula.py:422`; `app/ponto/portas/celula.py:445`; `app/ponto/portas/celula.py:490`; `app/ponto/services/fechamento.py:1824`).

Os chamadores concretos encontrados estão no teste O10-B: helper e chamadas diretas para confirmação, incompletude, repetição e par vazio (`app/ponto/tests/test_o10b_editar_dia_troca.py:112`; `app/ponto/tests/test_o10b_editar_dia_troca.py:193`; `app/ponto/tests/test_o10b_editar_dia_troca.py:226`; `app/ponto/tests/test_o10b_editar_dia_troca.py:246`; `app/ponto/tests/test_o10b_editar_dia_troca.py:307`). O relato menciona um lote `trocas_plantao_lote_o10c.py:160`, mas esse arquivo/chamador não foi localizado nesta cópia: **[nao verificado]**; o próprio relato registra ausência de chamador de UI (`app/docs/RELATO.md:426`).

3) **Os leitores principais consultam a célula, mas há leituras diretas dos horários da base.** `EC.eh_dia_trabalho` retorna `cel.trabalha`; `EC.marcos_do_dia` lê `cel.dna.marcos` antes do fallback ao template (`app/escala/models.py:1087`; `app/escala/models.py:1117`).

| Leitor | Caminho comprovado |
|---|---|
| Espelho admin | View → `espelho_do_colab`; previsto da competência por `grade_da_celula` (`app/ponto/views.py:157`; `app/ponto/services/espelho.py:506`). |
| Espelho app | Usa `espelho_do_colab`; há exceção nos horários dos dias sem card, abaixo (`app/api/views.py:1379`; `app/api/views.py:1611`). |
| PDF do espelho | Dias por `espelho_do_colab`; coluna “Previsto” usa template diretamente, abaixo (`app/relatorios/pdf_espelho.py:596`; `app/relatorios/pdf_espelho.py:903`). |
| Calendário admin | Grade por `grade_da_celula`, status por `status_dos_dias_lavrados`, rubricas por `DiaPago` (`app/colaboradores/services/calendario.py:332`; `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:655`). |
| Calendário/escala app | `eh_dia_trabalho` para trabalho/folga; template direto para horários (`app/api/views.py:950`; `app/api/views.py:955`). |
| Escala PWA e agenda admin | Usam `EC.eh_dia_trabalho` e `EC.marcos_do_dia` (`app/escala/views.py:38`; `app/colaboradores/views.py:2223`; `app/colaboradores/views.py:2232`). |
| Folha/motor | Fechamento consulta `eh_dia_trabalho`, fornece células e chama apuração com EC; o motor consulta os métodos do EC (`app/ponto/services/fechamento.py:729`; `app/ponto/services/fechamento.py:750`; `app/ponto/services/fechamento.py:278`; `app/ponto/motor_calculo_v2.py:2290`; `app/ponto/motor_calculo_v2.py:2447`). |
| TXT | Verbas vêm de `FechamentoMensal`; retenção consulta células e a grade delega a `grade_da_celula`, sem recalcular os valores pelo template (`app/folha/export.py:703`; `app/folha/export.py:1206`; `app/folha/export.py:329`). |

Os leitores diretos da base encontrados nesses caminhos são:

| Leitura direta | Efeito sobre o dia trocado |
|---|---|
| `/api/escala/v2/`: `te.marcos_do_dia` | Horários não vêm do DNA; trabalho/folga vem da célula (`app/api/views.py:950`; `app/api/views.py:955`; `app/api/views.py:968`). |
| Espelho app, dias sem card: `tipo_escala.marcos_do_dia` | Mesma separação: célula decide trabalho/folga, template fornece horários previstos (`app/api/views.py:1611`; `app/api/views.py:1613`; `app/api/views.py:1639`). |
| Bloco escala de `/api/me/`, nas duas cascas | Horários vêm do template; não representa uma leitura dos marcos congelados do dia (`app/api/views.py:259`; `app/api/views_core.py:248`). |
| Coluna “Previsto” do PDF | Usa `tipo_escala.hora_inicio/hora_fim` nas linhas com turnos, sem consultar o DNA daquela linha (`app/relatorios/pdf_espelho.py:903`; `app/relatorios/pdf_espelho.py:978`). |
| Resumos/cabeçalhos de escala | Tela, PDF e “minha escala” apresentam horários da base; as linhas da escala PWA continuam usando EC (`app/ponto/services/espelho.py:201`; `app/relatorios/pdf_espelho.py:730`; `app/templates/escala/minha_escala.html:33`; `app/escala/views.py:43`). |
| Caminho antigo `montar_grade_prevista`, sem mapa de DNA | Trabalho passa pelo EC, mas horários podem vir diretamente do template; existe chamador sem mapa (`app/escala/utils.py:760`; `app/escala/utils.py:767`; `app/escala/utils.py:1016`). |

Esses caminhos podem mostrar **horários incompatíveis com a célula** quando template/escala ativa e DNA/geradora divergem; não permitem afirmar que toda troca apareceria errada, pois o trabalho/folga passa pela célula e a própria troca gera o DNA do substituto a partir do template dele. Divergência concreta de um par de produção: **[nao verificado]** (`app/api/views.py:950`; `app/api/views.py:1611`; `app/ponto/portas/celula.py:591`; `app/ponto/management/commands/gerar_celulas.py:81`).

Fora desses consumidores principais, também há leitura direta da base na correção E/S retroativa, no propositor, na defesa de pergunta e no horário da cobrança “abriu e não bateu”; o efeito concreto deles sobre uma permuta é **[nao verificado]** (`app/api/views.py:619`; `app/api/views_core.py:612`; `app/ponto/propositor.py:334`; `app/chamados/juizes.py:1546`; `app/ponto/services/abriu_nao_bateu.py:193`).

4) **Os seis campos explícitos da edição são escritos por `editar_dia`, dentro da porta canônica da célula.** Gerar, regenerar e reverter são outras operações do mesmo módulo, não escritores externos (`app/ponto/portas/celula.py:600`; `app/ponto/portas/celula.py:609`; `app/core/portas.py:107`).

| Campo | Escrita e leitores encontrados |
|---|---|
| `trabalha` | Troca escreve `False/True`; outros atos da porta também o escrevem. Leitores: `EC.eh_dia_trabalho`, grade, cartório e detalhe do dia (`app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:788`; `app/ponto/portas/celula.py:849`; `app/escala/models.py:1096`; `app/escala/services/leitor_celula.py:381`; `app/ponto/services/cartorio.py:1408`; `app/ponto/services/dia_do_colab.py:91`). |
| `dna` | Produzido por `montar_dna`, gravado pela edição; gerar/regenerar/reverter também escrevem. Leitores: marcos, pausas, extra declarada, cartório e juiz de batida (`app/ponto/portas/celula.py:591`; `app/ponto/portas/celula.py:604`; `app/ponto/portas/celula.py:244`; `app/ponto/portas/celula.py:786`; `app/ponto/portas/celula.py:849`; `app/escala/models.py:1124`; `app/escala/models.py:1189`; `app/escala/models.py:1235`; `app/ponto/services/cartorio.py:1348`; `app/ponto/juiz_batida.py:158`). |
| `escala_geradora` | Edição escreve o vínculo escolhido para aquele dia; geração/regeneração/reversão também escrevem. O juiz `vinculo_do_dia` lê a geradora e exige sua vigência (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:605`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:789`; `app/ponto/portas/celula.py:849`; `app/escala/alimentacao.py:269`). |
| `origem` | Edição escreve `'editada'`; geração automática escreve `'gerada'`. Regeneração/reversão restringem-se a gerada; geração pula editada; vigias de template também filtram gerada (`app/ponto/portas/celula.py:606`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:777`; `app/ponto/portas/celula.py:854`; `app/ponto/management/commands/tripwire_celulas.py:21`; `app/escala/management/commands/celulas_contra_o_template.py:29`). |
| `editada_em` | Edição escreve o instante atual. Leitores: carimbos de reescrita/cobrança órfã, fato novo do cartório, invalidação de perguntas, fase 12x36 e detalhe do dia (`app/ponto/portas/celula.py:598`; `app/ponto/portas/celula.py:607`; `app/escala/services/regeneracao.py:72`; `app/ponto/services/cartorio.py:365`; `app/chamados/veredito_lavrado.py:176`; `app/escala/services/fase_12x36.py:188`; `app/ponto/services/dia_do_colab.py:94`). |
| `editada_por` | Edição escreve `autor`; `_estado_da_edicao` lê para a trilha. Outro leitor de produção dessa identidade: **[nao verificado]** (`app/ponto/portas/celula.py:608`; `app/ponto/portas/celula.py:531`). |

Todos esses campos também são lidos para antes/depois e idempotência; se a célula não existir, a edição cria `colaborador/data`, e `gerada_em` nasce automaticamente. As autoridades consultam a célula por essa chave, e os relógios de invalidação leem `gerada_em` (`app/ponto/portas/celula.py:524`; `app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:600`; `app/escala/models.py:1702`; `app/escala/models.py:1092`; `app/chamados/veredito_lavrado.py:178`).

A edição não escreve os campos de regeneração `regenerada_em/regeneracoes/dna_anterior`; tampou

[truncado em 20000 de 25142 caracteres]

## 09/11 11:12 · sessao be52b0625b15 · - · - · origem=Ronald (tmux)

Censo dos campos e leis (somente leitura).

A escrita da troca é indireta: `trocar_plantao` chama `editar_dia` uma vez por lado; o cedente recebe `trabalha=False`, o substituto `True` (`app/ponto/portas/celula.py:673-675`). A porta é o escritor declarado de `CelulaDia` (`app/core/portas.py:107-113`).

| Campo | Quem escreve | Quem lê encontrado |
|---|---|---|
| `CelulaDia.trabalha` | Na troca, `editar_dia`, create/save (`app/ponto/portas/celula.py:600-609`). Outros atos da mesma porta: regenerar (`:243-245`, `:259-276`), reverter (`:788-795`), gerar (`:849-871`). | Autoridade `EC.eh_dia_trabalho` (`app/escala/models.py:1087-1096`); leitor de grade (`app/escala/services/leitor_celula.py:381-411`); cartório (`app/ponto/services/cartorio.py:1408-1414`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.dna` | `editar_dia` constrói com `montar_dna(ec,dia,trabalha)` e grava (`app/ponto/portas/celula.py:591`, `:600-609`). Outros atos: regenerar (`:244-245`, `:256-276`), reverter (`:786-795`), gerar (`:849-871`). | `EC.marcos_do_dia` (`app/escala/models.py:1124-1133`), pausas (`:1189-1200`), extra declarada (`:1235-1238`); mapas de DNA (`app/escala/utils.py:46-53`, `:97`, `:130`); cartório/impressão/ata (`app/ponto/services/cartorio.py:1348`, `:1386`, `:1800`, `:1821`); juiz de batida (`app/ponto/juiz_batida.py:158`); detalhe do dia (`app/ponto/services/dia_do_colab.py:46`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.escala_geradora` | `editar_dia` grava o vínculo devolvido por `vinculo_do_dia_carregado` (`app/ponto/portas/celula.py:584`, `:600-609`). Regenerar (`:259-276`), reverter (`:789-795`), gerar (`:849-871`). | Autoridade do vínculo do dia: geradora tem precedência e deve cobrir a vigência (`app/escala/alimentacao.py:269-286`); trilha/idempotência (`app/ponto/portas/celula.py:529`, `:595`). |
| `CelulaDia.origem` | Troca grava `'editada'` em `editar_dia` (`app/ponto/portas/celula.py:600-609`); criação automática grava `'gerada'` (`:259-260`, `:849-852`). | Regeneração só reescreve gerada (`app/ponto/portas/celula.py:238`, `:263`), reversão só gerada (`:777`), geração pula editada (`:854-855`); tripwire só gerada (`app/ponto/management/commands/tripwire_celulas.py:21-22`); comparação com template só gerada (`app/escala/management/commands/celulas_contra_o_template.py:29`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.editada_em` | `editar_dia` (`app/ponto/portas/celula.py:598-609`). | Carimbos da reescrita/cobrança órfã (`app/escala/services/regeneracao.py:72-105`); detalhe do dia (`app/ponto/services/dia_do_colab.py:94`); fato novo do cartório (`app/ponto/services/cartorio.py:365`); invalidação de veredito de perguntas (`app/chamados/veredito_lavrado.py:176-180`); invalidação do censo de fase (`app/escala/services/fase_12x36.py:188-193`); trilha (`app/ponto/portas/celula.py:530`). |
| `CelulaDia.editada_por` | `editar_dia` (`app/ponto/portas/celula.py:600-609`). | Leitor explícito encontrado: `_estado_da_edicao`, para trilha (`app/ponto/portas/celula.py:531`). Outro leitor de produção da identidade desse campo: [nao verificado]. |
| `CelulaDia.colaborador/data` quando não existe célula | `editar_dia` cria a linha com a chave (`app/ponto/portas/celula.py:600`). | Seleção da própria edição (`:593`); autoridades consultam pela chave (`app/escala/models.py:1092-1093`, `:1122-1123`). |
| `CelulaDia.gerada_em` em criação | Automático do modelo (`app/escala/models.py:1702`), decorrente do create de `editar_dia` (`app/ponto/portas/celula.py:600`). | Invalidação das perguntas (`app/chamados/veredito_lavrado.py:178-179`) e fase (`app/escala/services/fase_12x36.py:190`). |

A edição não grava `regenerada_em/regeneracoes/dna_anterior`; isso é declarado e a lista de `update_fields` confirma (`app/ponto/portas/celula.py:538-539`, `:609`). O DNA do substituto usa o template do próprio vínculo, não o horário copiado do cedente (`app/ponto/portas/celula.py:510-511`, `:584-591`, `:635-637`).

Outros estados/trilha:

- Cada edição produz `LogAuditoria`: usuário, ação `editar_dia`, modelo `CelulaDia`, objeto, descrição, antes/depois dos seis campos, motivo e `apesar_da_lavra` (`app/ponto/portas/celula.py:611-618`). O par produz ação `trocar_plantao`, modelo `Ausencia`, objeto cedente, lados com colaborador/data/ausencia/documento/trabalha/resultado e motivo/override (`:682-691`). O escritor físico é `core.observ.evento`, via `evento_kw` (`app/core/observ.py:94-148`).
- Leitor genérico da trilha: `core.views.log_auditoria`, permissão `ver_saude_sistema`, filtro e leitura dos logs (`app/core/views.py:686-738`). A tela mostra usuário/ação/modelo/objeto/descrição/IP, sem renderizar o JSON antes/depois (`app/templates/core/log_auditoria.html:55-74`). Consumidor específico das ações `editar_dia/trocar_plantao`, fora a própria emissão, não encontrado na busca: [nao verificado].
- Cada lado marca cobranças órfãs (`app/ponto/portas/celula.py:625-626`): escreve `contexto_json.orfa_regeneracao={em,marcos_na_emissao}` pela porta `renovar_contexto` (`app/ponto/management/commands/gerar_celulas.py:241-255`). O julgamento dessa marca/reescrita é `cobranca_orfa`, alimentado por ambos os carimbos (`app/escala/services/regeneracao.py:87-105`, `:141-155`).
- O rabo da edição pode chamar julgamento do dia e recálculo financeiro (`app/ponto/portas/celula.py:627-628`, `:424-458`), além de rejunção e reavaliação de falta após commit (`:490-502`). Não confundir os seis campos explícitos da edição com os campos dos modelos alcançados por esses serviços (`:600-609`, `:424-502`).

Juiz:

- `JUIZES.md` é lookup e diz que registro vivo é `core/juizes.py` (`app/docs/JUIZES.md:8-14`). Lá estão dia de trabalho, minutos, precedência, ausência, status e folga (`app/core/juizes.py:170-193`), e geometria/marcos do turno (`:195-237`). Não encontrei juiz registrado para “este par pode trocar?”; esse predicado/registro é [nao verificado]. A função `trocar_plantao` é operação da porta, não aparece como juiz no registro (`app/ponto/portas/celula.py:81-82`; `app/core/juizes.py:170`).
- A tabela aponta permissão de tela para `tem_acao` e contextual para `pode_agir_chamado` (`app/docs/JUIZES.md:48-49`); a troca só exige `autor.pk` e motivo, sem chamar essas autoridades (`app/ponto/portas/celula.py:646-649`, `:652-691`).

Leis já escritas (não transformar em perguntas novas):

- Par, folga sem desconto, assumido normal com AN/intra sem HE100, duas cartas, confirmação pelo admin, pedido por qualquer lado: `app/docs/CORTES.md:187`.
- Escritor humano na mesma porta, origem editada, trilha, duas chamadas no mesmo atomic e exportada barrada: aval posterior O10-B literal em `app/docs/PROMPTS.md:1901`, também `app/docs/RELATO.md:480`; casos registram o literal em `app/ponto/tests/test_o10b_editar_dia_troca.py:4-9`.
- L-113 genérica revogou guarda exportada como trava (`app/docs/LEIS.md:151`), mas o aval O10-B de 06/11 explicitamente manda exportada barrada (`app/docs/PROMPTS.md:1901`). Convém citar ambos sem inventar nova decisão.
- Quem fornece lado substituto no SOLO: admin, nunca inferido; SOLO aplica zero pares; competência 09 só com `!` após DRY (`app/docs/PROMPTS.md:1907`; `app/docs/RELATO.md:431`).
- Fonte do dia/marcos é célula; template é gerador e leitores usam EC (`CLAUDE.md:145-154`).
- Autor/motivo/trilha em ação crítica e escritor único já estão escritos (`CLAUDE.md:32-34`).

Lacuna de lei expressamente registrada: “troca que cai em folga trabalhada pede decisão do admin?” (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`). Não reduzir o número final só às lacunas novas sugeridas: essa pergunta já consta como aberta.

Para agrupamento das demais perguntas do usuário, as decisões específicas de recusa de par por interjornada/sobreposição, empresas/vínculos diferentes, datas iguais/diferentes e tipo/status/período das cartas não aparecem na porta (`app/ponto/portas/celula.py:632-700`). Os 11h são mencionados como “NÃO valida” em outra função de cobertura (`app/ponto/utils.py:152-155`), o que comprova referência existente, não uma lei de elegibilidade da permuta. “Qual regra deve valer?” nesses pontos é [nao verificado].

## 09/11 11:12 · sessao e6a1ea29d182 · - · - · origem=Ronald (tmux)

Item 3 — censo dos leitores:

| Leitor | Fonte comprovada |
|---|---|
| Espelho admin | A view chama `espelho_do_colab` (`app/ponto/views.py:157-158`); o previsto na competência vem de `grade_da_celula` (`app/ponto/services/espelho.py:506-520`). |
| Espelho app | Chama a mesma autoridade `espelho_do_colab` (`app/api/views.py:1379-1385`), mas os dias sem card buscam os horários diretamente no template (`app/api/views.py:1611-1614`). |
| PDF espelho | Busca os dias em `espelho_do_colab` (`app/relatorios/pdf_espelho.py:596-598`), porém a coluna **Previsto** usa `tipo_escala.hora_inicio/hora_fim`, imprimindo-os em cada linha com turnos (`app/relatorios/pdf_espelho.py:903-909`, `:978`). |
| Calendário admin | Consulta `grade_da_celula` (`app/colaboradores/services/calendario.py:332-334`), lê status por `status_dos_dias_lavrados` (`:602-607`) e dinheiro por `DiaPago` (`:290`, `:655-656`). A view consumidora está em `app/colaboradores/views.py:1945-1946`. |
| Calendário/escala API app | `/api/escala/v2/` registrado em `app/api/urls.py:72`; trabalho/folga vem de `escala.eh_dia_trabalho(data)`, porém horários vêm de `te.marcos_do_dia(data)` (`app/api/views.py:934-955`), publicados em `:968-971`. |
| Escala web/PWA | Usa ambos `ec.eh_dia_trabalho` e `ec.marcos_do_dia` (`app/escala/views.py:38-44`); a agenda semanal administrativa também usa ambos (`app/colaboradores/views.py:2223`, `:2232-2234`). |
| Folha/motor | O fechamento consulta `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:729-732`), alimenta o motor com células (`:750`) e chama `apurar_por_fatia` (`:754-755`); esta passa `escala_colaborador=esc` ao motor (`:278-290`). O motor prefere `EC.marcos_do_dia` (`app/ponto/motor_calculo_v2.py:2290-2291`) e pergunta `EC.eh_dia_trabalho` (`:2447-2448`). |
| TXT | As verbas são lidas de `FechamentoMensal`, sem novo motor (`app/folha/export.py:703-724`); retenção consulta `CelulaDia` (`:1206`, `:1233-1256`); `grade_do_fechamento` delega a `grade_da_celula` (`:329-338`). Datas de falta vêm de ausências cujo efeito vigente desconta, não de template (`:169-185`). |

As autoridades realmente passam pela célula: `EC.eh_dia_trabalho` retorna `cel.trabalha` (`app/escala/models.py:1087-1096`); `EC.marcos_do_dia` lê `cel.dna.marcos` antes do fallback de template (`:1117-1133`). `grade_da_celula` carrega `CelulaDia` e lê sua ata/trabalha (`app/escala/services/leitor_celula.py:345-381`).

**Leitores diretos da base encontrados nos caminhos pedidos:**

- Calendário API app: horários dos dias via template (`app/api/views.py:955`); o trabalho/folga continua passando pela célula (`:950`).
- Espelho app, dias sem card: horários via template (`app/api/views.py:1613`, `:1639-1642`); trabalho/folga passa pela célula (`:1611`).
- Bloco escala de `/api/me/`, nas duas cascas: horários via template (`app/api/views.py:259-274`; `app/api/views_core.py:248-263`).
- Coluna Previsto do PDF: campos crus do template, não DNA (`app/relatorios/pdf_espelho.py:903-909`, `:978`).
- Cabeçalhos da escala: resumo da tela usa horas cruas do template (`app/ponto/services/espelho.py:201-212`, `app/templates/ponto/espelho.html:142`); PDF idem (`app/relatorios/pdf_espelho.py:730-740`); “minha escala” apresenta o padrão no cabeçalho (`app/templates/escala/minha_escala.html:33-34`), mas os horários das linhas vêm do EC (`app/escala/views.py:43-44`, `app/templates/escala/minha_escala.html:78-81`).
- Na degradação antiga, `montar_grade_prevista` consulta trabalha pelo EC (`app/escala/utils.py:760-762`), mas pode consultar horas no template sem mapa de DNA (`:767-768`); há chamador sem mapa em `app/escala/utils.py:1016`. O caminho normal `grade_espelho_janela` usa o builder de período (`:2337`), que consulta trabalha e mapa DNA antes do fallback (`:1605-1607`).

**Mostrariam errado?** Não há prova de divergência concreta do par real: **[nao verificado]**. Os caminhos diretos podem publicar horários diferentes da célula quando o template/escala ativa difere do DNA/geradora do dia; os sítios acima demonstram essa separação. Não é correto afirmar que ignoram toda a permuta: os caminhos do app consultam `trabalha` pela célula (`app/api/views.py:950`, `:1611`, `:260`).

Também não se pode assumir que o substituto receberia as horas do cedente: a porta atual faz o substituto trabalhar com o **próprio template** (`app/ponto/portas/celula.py:635-637`), chama `montar_dna` (`:591`), que congela os marcos de `ec.tipo_escala` naquela data (`app/ponto/management/commands/gerar_celulas.py:73-84`). Portanto, logo após a edição e sem diferença entre template atual e geradora, usar template pode coincidir com o novo DNA; o dano efetivo depende de dados não fornecidos **[nao verificado]**.

Outros leitores diretos encontrados, fora dos cinco consumidores principais: correção E/S retroativa nas duas APIs (`app/api/views.py:619-625`, `app/api/views_core.py:612-617`); `dia_posicional_completo` (`app/ponto/propositor.py:334-343`); defesa de pergunta `_marco_esperado_da_grade` (`app/chamados/juizes.py:1546-1563`); horário da cobrança “abriu e não bateu” (`app/ponto/services/abriu_nao_bateu.py:193-203`). Não confundir com fallbacks que primeiro consultam a célula: `veredito_celula._marcos_do_dia` (`app/ponto/services/veredito_celula.py:29-43`) e `marcos_turno` quando recebe `ec` (`app/escala/servico_jornada.py:68-71`).

## 09/11 11:11 · sessao d51be0d9df52 · - · - · origem=Ronald (tmux)

Item 1 — há canais genéricos para o colaborador pedir; não encontrei modelo/endpoint próprio que registre **o par de permuta** estruturado com dois colaboradores e duas datas. A conclusão negativa deve ficar como “[nao verificado] modelo próprio de pedido de permuta”; o que está comprovado é o modelo genérico `Ausencia`, sem campos de par, substituto ou dia assumido (`app/ponto/models.py:743-814`), e a solicitação textual em `ChamadoColaborador` (`app/chamados/models.py:38-52`).

**1) Ausência enviada pelo app/PWA**

- App: `GET/POST /api/ausencias/` → `api_ausencias`; autenticado e vinculado ao colaborador do usuário (`app/api/urls.py:82`; `app/api/views.py:1700-1706`).
- PWA: `/ponto/ausencias/submeter/` → `submeter_ausencia`, formulário `ponto/submeter_ausencia.html` (`app/ponto/urls.py:64`; `app/ponto/views.py:2466-2468`; `app/ponto/views.py:2560-2564`).
- Entrada: `tipo`, `data_inicio`, `data_fim`, `descricao`, documento e minutos; chama `criar_ausencia` com `registrado_por=request.user` (`app/api/views.py:1736`; `app/api/views.py:1788-1791`; `app/api/views.py:1830-1842`; `app/ponto/views.py:2476-2480`; `app/ponto/views.py:2536-2542`).
- Tipo é validado no catálogo vivo/cadastro `TipoAusencia`; a API também oferece as choices vigentes. Portanto, a disponibilidade de `troca_de_plantao` no app depende do cadastro que esta cópia não demonstra — **[nao verificado] no dado fornecido** (`app/ponto/services/ausencia.py:193-195`; `app/api/views.py:1883`; `app/ponto/catalogo/ausencias.py:17-23`). A lista estática de `Ausencia.TIPO_CHOICES` não contém `troca_de_plantao` (`app/ponto/models.py:661-679`).
- Campos persistidos: colaborador, tipo, data/data_fim, minutos/dias corridos, horários de cobertura parcial, hora_extra_100, descrição, documento, prazo_documento, chamado_origem, documento_referencia, status, registrado_por, aprovado_por/aprovada_em e timestamps (`app/ponto/models.py:743-814`).
- Estados: `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada` (`app/ponto/models.py:701-705`). Na criação, `criar_ausencia` chama `estado_inicial`: com documento ou tipo que não exige documento → aguarda decisão; tipo que exige sem documento → aguarda documento (`app/ponto/services/ausencia.py:331-345`; `app/ponto/services/ausencia.py:790-797`).
- Escrita posterior do status é centralizada em `mudar_estado` (`app/ponto/services/ausencia.py:808-835`). `anexar_documento` muda de aguardando documento para aguardando decisão (`app/ponto/services/ausencia.py:846-875`); `decidir` aceita aprovada/rejeitada, como ato DP (`app/ponto/services/ausencia.py:897-927`); escalonamento de documento vencido também passa a aguardando decisão, sem declarar falta (`app/ponto/services/ausencia.py:992-1006`).
- App e PWA abrem chamado genérico `ausencia_colab` após criar a ausência (`app/api/views.py:1871-1874`; `app/ponto/views.py:2546-2547`). Esse chamado é aviso persistente ao DP, não decide a ausência; guarda `ausencia_id`, data, tipo e presença de documento em contexto JSON (`app/ponto/services/ausencia.py:637-644`; `app/ponto/services/ausencia.py:659-678`).

**Leitores comprovados dessas Ausencias**

- App GET devolve últimas 30 do próprio colaborador, incluindo status/descrição/minutos (`app/api/views.py:1876-1895`).
- Admin `/ponto/ausencias/`: exige `ver_ausencias`, consulta vigentes ou registro específico e filtra tipo/status; botões/endpoints de aprovar/rejeitar/anexar/editar existem (`app/ponto/views.py:2155-2187`; `app/ponto/urls.py:53-76`).
- API mensageria/DP lê `ausencias_do_dp`, conta por tipo/status e lista aguardando decisão (`app/api/views_mensageria.py:736-762`).
- Comando `escalonar_documentos_ausencia` chama o escalonador (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25-26`); o escalonador lê fila aguardando documento e pode escrever aguardando decisão (`app/ponto/services/ausencia.py:992-1006`). **Execução atual desse cron em produção: [nao verificado].**
- Fechamento do aviso: `chamados_da_ausencia_decidida` seleciona `ausencia_colab` vivos e consulta ausência associada; `fechar_chamado_da_ausencia` reconcilia quando ausência sai dos estados abertos (`app/ponto/services/ausencia.py:686-716`). O lookup de crons declara `fechar_chamados_ausencia` a cada 30 minutos, mas execução real não é comprovada pelo código (`app/docs/CRONS.md:25`).
- Signals de Ausencia existem como consumidores em save/delete (`app/ponto/signals.py:144`; `app/ponto/signals.py:151`; `app/ponto/signals.py:250`; `app/ponto/signals.py:276`; `app/ponto/signals.py:283`).

**2) Solicitação textual / chamado**

- Existe taxonomia `Escala / Turno` com subcategorias “Troca de turno”, “Folga”, “Plantao” e palavras substituto/substituir (`app/chamados/classificador.py:38-49`).
- App `POST /api/chamados/abrir/` → `api_abrir_chamado`: aceita assunto, descrição, urgência, área, categoria e subcategoria; grava `ChamadoColaborador` com `modulo_origem='app_colaborador'` e nota da abertura (`app/api/urls.py:87`; `app/api/views.py:2151-2177`; `app/api/views.py:2201-2214`).
- Web/PWA `/chamados/abrir/` → `abrir_chamado`, chama `abrir_chamado_local` e renderiza `chamados/form_abertura.html` (`app/chamados/urls.py:10`; `app/chamados/views.py:147`; `app/chamados/views.py:196-215`).
- Não é modelo novo chamado “Solicitacao”: rótulo “Solicitação” decorre de `modulo_origem in ('chamados','app_colaborador')` (`app/chamados/rotulos.py:4-5`; `app/chamados/rotulos.py:26-38`).
- Campos principais: colaborador, assunto/título/descrição, área/categoria/subcategoria, módulo, contexto JSON, célula opcional, status_local, urgência, respondente/resolução, atribuição e SLA (`app/chamados/models.py:38-85`).
- Estados genéricos: aberto/em_analise/resolvido/fechado/cancelado/registrado/superado (`app/chamados/models.py:28-35`). Nascimento passa por `ChamadoColaborador.criar` e motor; escritor central posterior é `_gravar_estado` (`app/chamados/models.py:1054-1067`; `app/chamados/models.py:1093-1099`; `app/chamados/models.py:201-228`). Gestor muda estado por `transicionar` no serviço de ações (`app/chamados/services/acoes_chamado.py:313-319`).
- Leitores: painel DP consulta `ChamadoColaborador` (`app/chamados/services/painel.py:96-115`); “Meu Atendimento” separa solicitações e pendências e passa ao template (`app/chamados/views.py:612-642`); API de chamados carrega chamados visíveis do colaborador (`app/api/views.py:1031-1039`). A criação envia push para supervisão ou DP conforme área, com link painel (`app/api/views.py:2216-2228`).
- A apresentação da nova solicitação no PWA é controlada por parâmetro empresarial `app_solicitacoes`, desligado por padrão; quem tem solicitação já existente ainda vê aba (`app/chamados/services/solicitacoes_no_app.py:1-20`; `app/chamados/views.py:615-642`). **Valor real desse parâmetro: [nao verificado].**
- Leitor automático genérico de chamados: comando `alertar_chamados_sla` lê fila e registra ocorrência/push de SLA (`app/chamados/management/commands/alertar_chamados_sla.py:20`; `app/chamados/management/commands/alertar_chamados_sla.py:44-55`); execução real **[nao verificado]**.

**3) Pergunta**

Existe resposta a `PerguntaDisputa` pelo app, mas ela depende de pergunta existente da disputa do próprio colaborador; não é endpoint para abrir pedido estruturado de permuta (`app/api/views.py:2616-2619`; `app/api/views.py:2643-2655`). GET de perguntas usa `perguntas_do_app` e estados da pergunta (`app/api/views.py:2565-2568`; `app/api/views.py:2583-2585`). **Uma resposta específica de permuta materializar o par: [nao verificado].**

Não importar as contagens antigas de produção presentes em comentários/RELATO como dado atual: o anexo fornecido não contém registros de pedidos, tipos cadastrados ou flags por empresa — **[nao verificado]**.

## 09/11 10:59 · sessao 6efcaa755ca3 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Implementei P1/P4/P5 e escrevi primeiro os REDs, incluindo P2/P3 reservados à sessão. **RED executado, GREEN, mortes dos mutantes e resultado em produção: [nao verificado].**

**Entrega por hunk**

| Arquivo:linha | Alteração |
|---|---|
| [materializacao.py:109](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/materializacao.py:109) | Espera exige a própria metade validada. |
| `chamados/services/materializacao.py:175` | Vizinhança usa `batidas_apuraveis`, autoridade que exclui retratadas (`ponto/turnos.py:2866`). |
| [acoes_chamado.py:348](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/acoes_chamado.py:348) | Detalhe inclui C6 viva **ou** metade em espera pelo mesmo predicado. |
| [test_bug_par_intervalo.py:164](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_bug_par_intervalo.py:164) | P1: S retratada 01:30; valida S01/E02 e cobra alternância canônica, carimbos e fechamento. |
| `chamados/tests/test_bug_par_intervalo.py:457` | Foto de estado/trilha; P2 cobra zero saves, eventos e agendamentos em duas chamadas (`:478`); P3 chama `fechar` real e exige consulta C7 após trava (`:497`). |
| `chamados/tests/test_bug_par_intervalo.py:528` | P4: duas respondidas sem aceite; explicador, declaração do colaborador e aplicação pelo administrador (`:543`, `:556`). |
| `chamados/tests/test_bug_par_intervalo.py:623` | P5: GET real do detalhe e modal, sem contexto injetado; cobra include e aviso HTML (`:627`, `:631`). |
| `chamados/tests/test_bug_par_intervalo.py:702` | P6: carimbo inicialmente vazio; primeira falha cobra 4 saves, 4 agendamentos e 2 eventos; segunda cobra zero novas escritas e foto idêntica (`:726`, `:743`). |
| [mutantes_bug_par_intervalo.py:131](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/mutantes_bug_par_intervalo.py:131) | M38 restaura a guarda exata da volta 2; acrescentados M39–M44. |
| `chamados/tests/mutantes_bug_par_intervalo.py:205` | Inclui `acoes_chamado.py` no `py_compile`; `--comandos-v4` imprime os comandos individuais com settings CI (`:226`). |

Os novos testes usam a classe `TestCase`, relógio congelado e fixture `col960` existentes (`chamados/tests/test_bug_par_intervalo.py:32`, `:45`).

**Quem escreve e quem lê**

| Campo/função | ESCRЕVE / dono | LÊ, inclusive fora do diff |
|---|---|---|
| Espera do par | Predicado sem escrita: `materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `explicador.py:74`; declaração `declaracao_texto.py:213`, `:382`, `:678`; validação `validacao.py:193`, `:483`; vigia `vigia_carimbo.py:109`; tag `chamados_tags.py:19`. |
| Vizinhança / universo | Consulta sem escrita: `materializacao.py:169`; autoridade `ponto/turnos.py:2846`. | Aplicação individual `materializacao.py:962`; julgamento posterior do par `:1364`; apresentação `:539`; reexport `chamados/models.py:4080`. |
| `perguntas_disputa` | Contexto do detalhe `acoes_chamado.py:349`; contexto independente do modal `fio.py:253`. | View `chamados/views.py:549`; detalhe `templates/chamados/detalhe_local.html:309`; modal `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `validacao.py:134`; devolução `:202`; nova resposta `disputa_emissao.py:1477`; complemento pelo carimbo `chamados/models.py:3138`. | Espera `materializacao.py:109`; C6 `chamados/juizes.py:1153`; C7 `chamados/models.py:1850`. |
| `materializada_em/por`, `via_resolucao` | `_escrever_carimbo`, `chamados/models.py:3100`; descarimbo `:3211`; lote `:3269`; relavra `relavra_via.py:125`. | Lastro `chamados/juizes.py:660`; C6 `:1164`; devolução `validacao.py:188`; lavra persistida `veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração `ponto/registro_batida.py:276`, por `anotar` (`:255`). | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`. |
| Fotos e definições dos testes | Fixtures/helpers `test_bug_par_intervalo.py:81`, `:88`, `:472`, `:767`. | Asserções `:495`, `:526`, `:745`; alvos do runner `mutantes_bug_par_intervalo.py:131`. | [nao verificado]
| `MUTANTES` / fontes | Definições `mutantes_bug_par_intervalo.py:131`. | Leitura/recompilação em memória `:166`, `:179`, `:249`; conferência estática `:217`. |

O save do descarimbo agenda beco e pode emitir evento pelos receivers (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`). Exaustividade transitiva deste mapa: **[nao verificado]**.

**Linhas exatas para a SESSÃO escrever em `chamados/models.py`**

P2: após o retorno de `fora_do_fato`, antes de `_escrever_carimbo` (`chamados/models.py:3209`, `:3211`):

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: antes do `if motivo is None` (`chamados/models.py:1908`):

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem` (`chamados/models.py:1923`):

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

É necessário conservar a modalidade original porque `motivo` recebe `FECHOU_VAZIA` ou `FECHOU_VEREDITO` antes da trava (`chamados/models.py:1914`).

**Mutantes e asserções discriminantes previstas**

| Mutante | Regressão | Asserção que deve matar |
|---|---|---|
| M38 | Guarda exata v2 | Ambas passam por `descarimbar` mesmo inicialmente vazias (`test_bug_par_intervalo.py:724`). |
| M39 | Universo cru inclui retratada | Segunda validação aceita o par (`:176`). |
| M40 | Espera sem validação própria | Predicado falso para metade apenas respondida (`:533`). |
| M41 | Detalhe volta a filtrar só C6 | Metade em espera presente no contexto GET (`:641`). |
| M42 | Aplicação retorna antes de validar | Própria metade recebe validação (`:566`). |
| M43 | Declaração prematuramente em espera | Retorna `ja_declarada`, aguardando supervisão (`:550`). |
| M44 | Tag omite espera no modal | Aviso HTML presente uma vez no GET real (`:642`). |

M38 cobra a passagem pela porta; após P2, comparar somente estado/trilha não distingue a guarda antiga que pula um no-op (`test_bug_par_intervalo.py:724`, `:745`). Mortes executadas: **[nao verificado]**.

Comandos individuais reservados ao Code; equivalentes aos impressos por `--comandos-v4` (`mutantes_bug_par_intervalo.py:226`):

```bash
export DJANGO_SETTINGS_MODULE=config.settings.ci
export PYTHONDONTWRITEBYTECODE=1

teste() { [nao verificado]
  python3 manage.py test "chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.$1" --noinput
}
mutante() {
  python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}

teste test_V4_P1_par_aplica_com_S_retratada_entre_metades [nao verificado]
teste test_V4_P2_descarimbar_vazio_duas_vezes_nao_escreve_nem_agenda [nao verificado]
teste test_V4_P3_fechar_real_consulta_C7_sob_trava_e_retem_espera [nao verificado]
teste test_V4_P4_respondidas_sem_aceite_aguardam_validacao [nao verificado]
teste test_V4_P4_declaracao_colab_sem_aceite_aguarda_supervisao [nao verificado]
teste test_V4_P4_aplicar_declaracao_valida_a_propria_metade [nao verificado]
teste test_V4_P5_GET_detalhe_conserva_metade_em_espera [nao verificado]
teste test_V4_P5_GET_modal_conserva_metade_em_espera [nao verificado]
teste test_V4_P6_falha_com_carimbo_vazio_duas_vezes_mesmo_estado_e_trilha [nao verificado]

mutante M38
mutante M39
mutante M40
mutante M41
mutante M42
mutante M43
mutante M44
```

Executei somente a conferência estática abaixo: **rc 0**, `py_compile` dos quatro Python tocados sem persistência e sintaxe/âncoras M1–M44 conferidas (`mutantes_bug_par_intervalo.py:195`, `:211`, `:217`). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

**DECISAO-ABERTA:** P2/P3 aguardam escrita da sessão; P6 depende de P2. A inserção proposta consulta C7 sob trava, mas as leituras anteriores de respostas/perguntas permanecem antes dela (`chamados/models.py:1897`, `:1906`, `:1921`). Prova concorrente, GREEN e certificação pelo Code/opus: **[nao verificado]**.

## 09/11 10:57 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Considerando os novos comandos com `config.settings.ci`, não encontrei bloqueador concreto dos GETs por inspeção: `ci.py:5` seleciona `config.urls_ci`, `config/urls_ci.py:9` inclui as rotas de chamados e os middlewares de sessão/autenticação/mensagens vêm de `config/settings/base.py:47`, `:50`, `:55`. O administrador da fixture tem `is_staff=True` e `is_superuser=True` (`chamados/tests/test_bug_par_intervalo.py:41`), aceitos pelo gestor e por RBAC (`chamados/views.py:35`; `colaboradores/mixins.py:328`). Execução permanece [nao verificado].

O modal recebe perguntas de disputas abertas sem filtro C6 (`chamados/services/fio.py:221`), associa a disputa em `:258` e o template entra pelo mapa real de permissão (`templates/chamados/partials/modal_fio.html:122`). O detalhe passa pelo contexto real da view (`chamados/views.py:549`) e pelo include em `templates/chamados/detalhe_local.html:310`. Os novos testes não injetam contexto (`chamados/tests/test_bug_par_intervalo.py:633`).

As contagens do P6 correspondem ao caminho inspecionado **depois da cura P2**:

- Duas recusas salvam apenas `recusa_motivo` (`chamados/services/materializacao.py:1195`), cada uma agenda um beco (`chamados/signals.py:225`) e não emite outbox porque esse campo não pertence a `CAMPOS_QUE_ACORDAM` (`chamados/models.py:3224`; `chamados/signals.py:245`).
- Duas devoluções salvam validação (`chamados/services/validacao.py:205`), agendam outros dois becos e emitem os dois eventos (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`).
- A segunda recusa idêntica não salva (`chamados/services/materializacao.py:1183`, `:1192`); validação já vazia não salva (`chamados/services/validacao.py:198`) e foto idêntica não audita (`:222`). As notas também dependem da mudança do motivo (`chamados/services/materializacao.py:1195`).

O registro de trava em `on_commit` não deve inflar a primeira contagem: `_travar_ato_materializacao` está antes da captura (`chamados/tests/test_bug_par_intervalo.py:714`), e `_registrar_travados` não adiciona novamente colaborador já registrado (`chamados/services/trava_do_colaborador.py:42`). Resultado executado: [nao verificado].

Única lacuna adicional do P6: o teste configura a falha fixa, mas não cobra que `_materializar_resposta_hora` foi realmente alcançada; uma recusa anterior estável poderia satisfazer parte das mesmas asserções (`chamados/tests/test_bug_par_intervalo.py:719`). Capturar esse mock e cobrar uma chamada por tentativa reforça a origem pretendida da falha. [nao verificado]

## 09/11 10:53 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Mapa por leitura estática:

| Função/campo | Escritor/dono | Leitores fora do diff |
|---|---|---|
| `aguarda_outra_metade_intervalo` | Predicado sem escrita: `chamados/services/materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `chamados/services/explicador.py:74`; declaração `chamados/services/declaracao_texto.py:213`, consumida em `:382`, `:485`, `:678`; validação/devolução `chamados/services/validacao.py:193`, `:483`; vigia `chamados/services/vigia_carimbo.py:109`; tag `chamados/templatetags/chamados_tags.py:19`; estado de aplicação `chamados/services/materializacao.py:527`. |
| `_vizinha_mesmo_tipo` | Consulta sem escrita: `chamados/services/materializacao.py:169`. | Estado da aplicação `chamados/services/materializacao.py:542`; aplicação individual `:965`; par posterior `:1367`; reexport em `chamados/models.py:4080`. |
| `batidas_apuraveis` | Autoridade sem escrita, filtro de retratação em `ponto/turnos.py:2866`. | Espelho `ponto/services/espelho.py:1022`; turno órfão `ponto/utils_turno_orfao.py:28`; diagnóstico `ponto/services/diagnostico_escala.py:142`; fio `chamados/services/fio.py:1521`, `:1881`; validação `chamados/services/validacao.py:896`; consultas em lote `ponto/turnos.py:2989`, `:3009`. |
| Contexto `perguntas_disputa` do detalhe | `chamados/services/acoes_chamado.py:348`, publicado em `:409`. | View GET `chamados/views.py:549`; loop `templates/chamados/detalhe_local.html:309`; include `:310`. Modal possui escritor próprio em `chamados/services/fio.py:253`, consumido por `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `chamados/services/validacao.py:134`; devolução `:202`; nova resposta `chamados/services/disputa_emissao.py:1477`; reabertura em lote `:1590`; carimbo completando validação `chamados/models.py:3138`; reabertura da disputa `:1565`. | C6 `chamados/juizes.py:1153`; foto da validação `chamados/services/validacao.py:147`; crédito por autor `chamados/services/verbos.py:240`; C7 `chamados/models.py:1850`. |
| `materializada_em/por/via_resolucao` | Porta comum `_escrever_carimbo`, `chamados/models.py:3100`–`:3104`; descarimbo `:3211`; lote `:3269`; via por fato `:3278`; relavra `chamados/services/relavra_via.py:125`. | Lastro `chamados/juizes.py:660`, `:672`, `:687`; C6 `:1164`, `:1190`; devolução `chamados/services/validacao.py:188`; lavra persistida `chamados/veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração pela porta `ponto/registro_batida.py:276`, escrita efetiva por `anotar` em `:255`; desfazer humano por `anotar` em `ponto/services/retratar_humano.py:207`. | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`; vigia de carimbo `chamados/services/vigia_carimbo.py:119`. |

Efeitos do `save` de descarimbo: agendamento pelo receiver `chamados/signals.py:215`–`:225`; emissão por `chamados/signals.py:242`; resolução da célula e chamada da porta em `chamados/models.py:3235`–`:3252`. Ausência de deduplicação nessa emissão consta da porta `ponto/portas/celula.py:891`.

Texto mínimo para a SESSÃO, sem edição realizada:

P2: inserir entre o retorno de `fora_do_fato` e `_escrever_carimbo`, em `chamados/models.py:3211`:

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: guardar a modalidade original antes do `if motivo is None` de `chamados/models.py:1909`:

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem`, em `chamados/models.py:1923`, inserir:

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

A flag precisa conservar a entrada original: antes da trava, o código já converte `motivo=None` em `FECHOU_VAZIA` ou `FECHOU_VEREDITO`; verificar `motivo is None` somente depois disso tornaria a guarda inoperante. (`chamados/models.py:1916`)

Essa inserção consulta C7 sob trava, mas as leituras anteriores de `_pend`, `perguntas` e `vivas` permanecem anteriores à trava; proteção integral dessas leituras exigiria movê-las para dentro do bloco após a trava. (`chamados/models.py:1897`, `:1907`, `:1921`)

Exaustividade transitiva do mapa: [nao verificado]. Nenhum teste, sintaxe, banco, rede ou escrita foi executado por este explorer. [nao verificado] [nao verificado]

## 09/11 10:52 · sessao f14c3a354d98 · SONDA-SEM-GRANT · verificador · origem=Code

**Não fecharia a volta 4:** permanecem caminhos que conservam carimbo verde após alteração da sombra e uma limpeza que pode remover container alheio por colisão de nome. (bin/sombra.sh:214,225,228,430,433; bin/tests/test_sonda_sem_grant_pg.sh:41,50,93,95)

1. **Itens PROCEDENTES da triagem v3**

| Item | Fechamento e mordida |
|---|---|
| **2a — fechado; NÃO PROCEDE reabrir a lacuna original.** | SUPERUSER é aplicado; o selo exige `super>0`, rc 5, recusa nomeada e sonda não executada. O log registra precisamente essas falhas para `super=0`. (bin/tests/test_sonda_sem_grant_pg.sh:183,185,186,187,207; anexo sonda_v4_mut.log:11) |
| **2b — fechado para `$PGU → leitor`; NÃO PROCEDE reabrir esse caso.** | O estrago de schema grantable passa pelas mesmas cobranças; a porta agora revoga esse default. O mutante da medição morre na detecção; retirar o REVOKE morre na recuperação pela porta. (bin/tests/test_sonda_sem_grant_pg.sh:183,189,208; bin/sonda_provisionar.sh:132; anexo sonda_v4_mut.log:16,31) |
| **4 — guarda original fechada; residuais abaixo são PROCEDENTES.** | `carimbar` verifica as operações e `refazer` exige invalidação antes do drop. O log mata ausência da guarda e retorno do `mv` ignorado pela asserção “chamou dropdb”. (bin/sombra.sh:94,95,98,99,101,275,276; bin/tests/test_sonda_sem_grant.sh:408; anexo sonda_v4_mut.log:1,26) |
| **6 — fechado no pedido literal; NÃO PROCEDE reabrir invalidado/ontem.** | Placar retorna `?` nesses casos; invalidação também apaga INCOMPLETA. O log registra a comparação esperada matando a leitura antiga. (bin/placar_code.sh:37; bin/sombra.sh:110; bin/tests/test_sonda_sem_grant.sh:434,436,439; anexo sonda_v4_mut.log:6) |
| **10 — silêncio original corrigido; identidade ainda PROCEDENTE.** | O selo cobra rc preservado, falha nomeada e `rm -f` exato; o log mata a implementação antiga nessas cobranças. Isso não demonstra autoria do container removido. (bin/tests/test_sonda_sem_grant_pg.sh:40,44,48,76,80,82; anexo sonda_v4_mut.log:21) |

O log fornecido registra sete mortes compatíveis com as asserções indicadas; patches exatos dos mutantes, comandos completos, baseline e reexecução independente são **[nao verificado]**. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31)

**PROCEDENTE — ressalva no selo 9c:** no caso `mv`, o STAMP aponta para `sem_dir/stamp`, mas a asserção de integridade compara outro arquivo, `RF/stamp`. Ela não prova preservação do carimbo existente após falha do `mv` final. Cura sugerida: provocar a falha final mantendo STAMP apontado para o carimbo verde efetivamente examinado. A mordida contra alcançar `dropdb` continua válida. (bin/tests/test_sonda_sem_grant.sh:394,396,399,408,411)

2. **T4: outros caminhos**

- **PROCEDENTE — bloco:** começando com hoje/OK, nova execução pode calcular FALHOU, falhar em `carimbar` e terminar com o `echo` bem-sucedido. O OK anterior permanece; `conferir` pode aprová-lo enquanto houver migrations. Cura sugerida: invalidar sob a trava antes de executar o bloco, exigir publicação final e testar erro do bloco combinado com falha de gravação. (bin/sombra.sh:356,407,417,429,430,433,455,459)
- **PROCEDENTE — incremental:** a transação altera a janela antes da prova e das máscaras, sem invalidar o carimbo. Falha posterior conserva o verde anterior; falha em `carimbar` também é engolida pelo `echo`. Se o carimbo anterior era completa/OK, a cobertura ainda aceita “completa”. Cura sugerida: invalidar antes da alteração, guardar explicitamente os insumos necessários e exigir publicação final. (bin/sombra_incremental.sh:61,66,68,81,85; bin/sombra.sh:214,216,218,225,228; app/core/sombra_cobertura.py:39,40,41)
- **NÃO PROCEDE — fim do refazer:** `carimbar` é o último comando da função, portanto seu rc se propaga; se falhar, a invalidação anterior permanece. `--dump-agora` converge para a mesma guarda antes do drop; `conferir` não escreve o carimbo. (bin/sombra.sh:110,259,275,276,328,331,332,436,459)

3. **Predicados: NÃO são iguais**

| Caso | Placar | `conferir`, supondo migrations positivas |
|---|---|---|
| Dia vazio ou ontem | `?` | Recusa |
| Hoje, status vazio, INCOMPLETA=0 | `0` | Recusa |
| Hoje, OK, INCOMPLETA ausente | `?` | Aceita |
| Hoje, REFEITA/DIVERGE/FALHOU, número presente | Mostra número | Recusa |
| Hoje, REFAZENDO | `?` | Recusa |

Essas diferenças decorrem de `STATUS != REFAZENDO` mais número obrigatório no placar, contra `STATUS=OK` mais existência mínima do banco em `conferir`. (bin/placar_code.sh:34,35,36,37; bin/sombra.sh:439,455,457,459)

**PROCEDENTE — derivação própria no novo leitor.** Completude medida e ensaio verde são perguntas distintas; não devem ser anunciadas como predicados iguais. Cura sugerida: uma autoridade com parser comum e predicados explicitamente distintos. A cobertura já possui terceiro julgamento: hoje, OK ou REFEITA, depois tipo/base/janela. Até o parsing diverge: última ocorrência no placar, concatenação em `conferir`, última ocorrência com normalização na cobertura. (CLAUDE.md:20,21,22; bin/placar_code.sh:34,37; bin/sombra.sh:439; app/core/sombra_cobertura.py:18,21,35,37,42,45)

Leitores operacionais encontrados:

- **Diretos:** incremental, bloco e `conferir`; placar; `core/sombra_cobertura.py`. (bin/sombra.sh:210,211,224,356,427,439,443,444; bin/placar_code.sh:34,35,36; app/core/sombra_cobertura.py:35,37,39,42,45)
- **Delegados, sem predicado próprio do stamp:** deploy, indicador verde do placar, esteira em `trava.sh`, simulação de folha e diff de janela; os dois últimos também exigem cobertura. (bin/deploy.sh:162; bin/placar_code.sh:169; bin/trava.sh:52; bin/simular_folha.sh:68,70; bin/diff_janela_he_total.sh:19,22)

4. **Porta/defaults — PROCEDENTE a cobertura desigual**

`REVOKE ... ON SCHEMAS` cobre defaults de `$PGU` para `leitor`. A conferência conta defaults de **qualquer concedente**, destinados ao leitor **ou PUBLIC**, também para tabelas e sequências. A porta não revoga todo esse universo. (bin/sonda_provisionar.sh:125,126,128,129,132; bin/sonda_leitor_comum.sh:119,120,121,122)

**A porta deixa esses privilégios, mas não deixa passar como pronto:** a leitura recusa rc 5; a porta recusa rc 1 antes de promover `.env_leitor`. Há bloqueio sem autocura, sem falso pronto nesse caminho. Quem cura é o operador autorizado, revogando o default no concedente/destinatário/escopo de origem; o próprio diagnóstico orienta REVOKE manual. Cura efetiva em produção: **[nao verificado]**. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,135; bin/sonda_provisionar.sh:148,150,151,171)

5. **`_limpa` — PROCEDENTE colisão; NÃO PROCEDE nome vazio no fluxo atual**

O trap precede a criação. Se `run --name "$PGT"` falhar por colisão, o EXIT executa `rm -f "$PGT"` sobre o container que já ocupava o nome. A guarda só exclui três nomes conhecidos; não prova autoria. Cura sugerida: remover exclusivamente o ID obtido da criação bem-sucedida, sem remover por nome após criação recusada; acrescentar caso de colisão ao selo. (bin/tests/test_sonda_sem_grant_pg.sh:35,36,41,50,93,95)

PGT não fica vazio nessa atribuição: contém prefixo literal e PID; a substituição usada pela fixture ocorre em subshell. (bin/tests/test_sonda_sem_grant_pg.sh:35,72,73,74)

**PROCEDENTE — ausência não comprovada:** o rc de `rm` é ignorado e qualquer falha de `inspect` equivale a “sumiu”. Cura sugerida: distinguir inexistência de falha da consulta e testar esta última. Falha real do Docker: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,85,86,87,88)

6. **Classes adicionais**

- **Segundo escritor operacional do stamp — NÃO PROCEDE no código localizado:** publicação e invalidação passam por `carimbar`; os escritores adicionais dos selos usam fixtures temporárias. Escritor externo/manual: **[nao verificado]**. (bin/sombra.sh:101,110,225,328,430; bin/tests/test_sonda_sem_grant.sh:363,394,432,447)
- **Relógio — FILA para o preexistente; PROCEDENTE no novo leitor independente:** HOJE nasce antes da trava, a base relê a data e a cobertura usa `date.today()`. O placar acrescenta outra captura antes de chamar posteriormente `conferir`. Cura sugerida: referência temporal obtida na borda e passada aos julgamentos. Travessia efetiva da meia-noite: **[nao verificado]**. (anexo triagem_preverif_v3.txt:10; bin/sombra.sh:83,328,464; app/core/sombra_cobertura.py:34; bin/placar_code.sh:39,169)
- **Selo sem mordida — NÃO PROCEDE como acusação geral:** há mortes pertinentes registradas; preservação no caso `mv`, identidade do container e os caminhos incremental/bloco continuam fora dessa prova. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31; bin/tests/test_sonda_sem_grant.sh:396,411; bin/tests/test_sonda_sem_grant_pg.sh:80,93)
- **Campos com dois sentidos — PROCEDENTE na interpretação de prontidão:** STATUS distingue resultado do refazer e resultado do bloco; incremental preserva o status anterior mesmo declarando bloco fora da janela. DIA sozinho não significa pronto, e INCOMPLETA é a medição do refazer. Cura sugerida: preservar essas distinções na autoridade comum, sem equiparar “medição válida” a “ensaio verde”. (bin/sombra.sh:227,319,329,331,432; bin/placar_code.sh:37; app/core/sombra_cobertura.py:37)

7. **Quem escreve / quem lê**

Todos os campos do stamp abaixo são materializados por `carimbar`; ele lê o anterior, produz temporários e promove o resultado. (bin/sombra.sh:94,95,98,99,101)

| Campo de `.sombra_stamp` | Quem escreve | Quem lê |
|---|---|---|
| DIA | Invalidação/completo: sombra.sh:110,328 | sombra.sh:210,356,439; placar_code.sh:34; sombra_cobertura.py:35 |
| STATUS | Invalidação/incremental/completo/bloco: sombra.sh:110,227,331,432 | sombra.sh:224,439; placar_code.sh:35; sombra_cobertura.py:37 |
| INCOMPLETA | Invalidação/completo: sombra.sh:110,329 | placar_code.sh:36 |
| TIPO / BASE_DIA | Incremental/completo: sombra.sh:225,328 | sombra.sh:211,441,443; sombra_cobertura.py:39,45 |
| JANELA_INI/FIM | Incremental/completo: sombra.sh:225,329 | sombra.sh:443; sombra_cobertura.py:42 |
| DIVERGE | Completo: sombra.sh:329 | sombra.sh:427,444 |
| INCREMENTAL_EM / ERROS | Incremental/bloco: sombra.sh:226,431 | sombra.sh:443,444 |
| DUMP, REFEITA, BLOCO, INCREMENTAL_S, BLOCO_NA_JANELA, COMANDOS, ALARMES, FORA_DO_ENSAIO | sombra.sh:226,227,328,330,430,431 | Leitor operacional desses campos: **[nao verificado]** |

| Outros estados/arquivos alcançados pelo delta | Quem escreve / quem lê |
|---|---|
| Defaults de schema; `super` | Porta revoga default e escreve NOSUPERUSER; consulta produz `default_escrita`/`super`; parser, porta e leitura consomem. Fixtures aplicam os estragos. (bin/sonda_provisionar.sh:105,132,148; bin/sonda_leitor_comum.sh:68,119,132,145; bin/sonda_leitura.sh:187; bin/tests/test_sonda_sem_grant_pg.sh:207,208) |
| `.env_leitor`, senha, temporário | Porta lê senha e publica; leitura valida/lê e entrega ao container. (bin/sonda_provisionar.sh:77,168,169,171; bin/sonda_leitura.sh:173,174,178,234) |
| Fontes recortadas pelos selos | Selos leem `sombra.sh` e `placar_code.sh`; escritor dessas fontes não é identificado pelo dado. (bin/tests/test_sonda_sem_grant.sh:358,359,380,422) **[nao verificado]** |
| Fixtures 9/9c/10: stamps, dumps vazios, docker falso, argv, saída e temporários | Selo monta; docker falso registra argv; asserções leem. (bin/tests/test_sonda_sem_grant.sh:363,383,384,385,387,394,403,408,411,413,432,447,450) |
| Fixtures `_limpa`: `lf/docker`, `lf/argv`, `lf/out`, `lf/t`; container PGT | Selo cria entradas; falso grava argv; asserções leem; `_limpa` remove pasta/container e consulta existência. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,46,61,63,71,74,76,80,93) |
| Imagem/recursos do selo PG | Selo lê `recursos.sh` e `db_teste.sh`; autoria desses valores: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:29,31) |
| Trilhas de provisionamento/leitura e saída da porta na sombra | Traps escrevem as trilhas; sombra redireciona e lê a falha da porta. Outros leitores operacionais: **[nao verificado]**. (bin/sonda_provisionar.sh:60,61; bin/sonda_leitura.sh:121,122; bin/sombra.sh:252,253) |

## 09/11 10:51 · sessao f5bd0a6923b7 · - · - · origem=Ronald (tmux)

- **PROCEDENTE — T4 residual em bloco:** partindo de carimbo hoje/OK, uma nova execução pode calcular `FALHOU`, falhar ao gravar e terminar com o `echo` bem-sucedido, preservando o OK anterior. `conferir` aceita esse OK se ainda houver migrations. Cura: invalidar antes da execução e exigir sucesso da publicação; testar carimbo inicialmente OK, erro no bloco e falha de gravação. (`bin/sombra.sh:356,407,417,428,429,430,433,455,459`)

- **PROCEDENTE — incremental conserva cobertura mentirosa:** altera a janela e confirma a transação antes de atualizar o carimbo. Falha da prova ou máscara deixa o carimbo antigo; falha de `carimbar` também é ignorada pelo `echo` seguinte. Um carimbo anteriormente `completa/OK` continua descrevendo cobertura completa. Cura: invalidar antes da alteração, preservar explicitamente os insumos necessários e exigir a publicação final; selo cobrando invalidação e recusa após cada falha. (`bin/sombra.sh:210,214,216,218,224,225,228`; `bin/sombra_incremental.sh:61,66,68,77,81,85`; `app/core/sombra_cobertura.py:39,40,41`)

- **NÃO PROCEDE — fim do refazer como preservação de verde antigo:** invalidação é exigida antes do drop; a publicação final é o último comando da função, portanto seu rc se propaga. Falha mantém dia vazio/REFAZENDO. `--dump-agora` converge para a mesma invalidação antes do drop; `conferir` não grava o carimbo. (`bin/sombra.sh:110,259,264,265,275,276,328,331,332,436,459`)

- **NÃO PROCEDE — predicados iguais:** o placar exige dia hoje, status diferente de REFAZENDO e número presente; conferir exige dia hoje, status exatamente OK e migrations positivas, ignorando o número. Status vazio com número: placar mostra número, conferir recusa. Hoje/OK sem INCOMPLETA: placar mostra `?`, conferir pode aprovar. Dia vazio ou ontem são rejeitados pelos dois; REFEITA/DIVERGE/FALHOU são admitidos como medida pelo placar, mas não como ensaio verde por conferir. São perguntas diferentes; centralizar parser e definições explícitas de validade/prontidão evita apresentar duas regras como idênticas. (`bin/placar_code.sh:34,35,36,37`; `bin/sombra.sh:439,455,456,459`)

- **PROCEDENTE — terceiro predicado e parsing próprio:** cobertura aceita hoje com OK **ou REFEITA**, depois aplica tipo/base/janela, sem consultar existência do banco. Seu parser usa última ocorrência e remove espaços/aspas; placar usa última ocorrência literal, enquanto conferir concatena todas as ocorrências. Cura: parser comum e predicados nomeados para perguntas distintas. (`app/core/sombra_cobertura.py:18,19,20,21,34,35,37,39,42,45,47`; `bin/placar_code.sh:34,35,36`; `bin/sombra.sh:439`)

- **Leitores diretos operacionais:** incremental lê DIA/BASE/TIPO/STATUS; bloco lê DIA/DIVERGE; conferir lê DIA/STATUS/TIPO/JANELAS/BASE/INCREMENTAL_EM/DIVERGE/ERROS; placar lê DIA/STATUS/INCOMPLETA; cobertura lê DIA/STATUS/TIPO/JANELAS/BASE. (`bin/sombra.sh:210,211,224,356,427,439,441,443,444`; `bin/placar_code.sh:34,35,36`; `app/core/sombra_cobertura.py:35,37,39,42,45`)

- **Leitores indiretos, sem regra própria do carimbo:** deploy; verde do placar; portão da esteira em `trava.sh`; simulação de folha e diff de janela. Estes dois últimos também exigem cobertura. (`bin/deploy.sh:162`; `bin/placar_code.sh:169`; `bin/trava.sh:40,43,52`; `bin/simular_folha.sh:68,70`; `bin/diff_janela_he_total.sh:19,22`)

- **FILA — relógio:** HOJE é capturado antes da trava; incremental e publicação completa consultam novamente a data; cobertura calcula hoje por conta própria. O placar adiciona captura anterior à chamada posterior de conferir. Cura: data de referência única na borda adequada, passada aos leitores; travessia real da meia-noite **[nao verificado]**. (`bin/sombra.sh:83,211,328,464,466,468,470,477`; `app/core/sombra_cobertura.py:34`; `bin/placar_code.sh:39,169`)

- **NÃO PROCEDE — segundo escritor operacional localizado:** os quatro caminhos escrevem por `carimbar`; invalidação escreve DIA/STATUS/INCOMPLETA, completo escreve os três, incremental/bloco escrevem STATUS. Escritor externo/manual **[nao verificado]**. (`bin/sombra.sh:92,101,110,225,227,328,329,331,430,432`)

## 09/11 10:51 · sessao 86ce7ad8eff9 · - · - · origem=Ronald (tmux)

- **2a/2b: fechados no código e selos mordem a causa indicada.** Estragos `SUPERUSER` e default de schema grantable entram no mesmo ciclo: chave positiva, rc 5, recusa nomeada, ausência de execução e cura pela porta. O log fornecido registra falhas correspondentes; alterações exatas dos mutantes e execução independente **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:181`, `:183`, `:185`, `:186`, `:187`, `:189`, `:207`, `:208`)

- **4: guarda fechada; mordida pertinente.** `carimbar` propaga falhas e `refazer` aborta antes do drop; 9c cobra ausência de `dropdb`, rc, mensagem e temporários. **Ressalva do selo:** caso `mv` aponta `STAMP` para caminho ausente, mas compara o outro arquivo `RF/stamp`; essa asserção não prova preservação de um carimbo existente após falha final de `mv`. (`bin/sombra.sh:94`, `:98`, `:99`, `:101`, `:275`; `bin/tests/test_sonda_sem_grant.sh:396`, `:399`, `:408`, `:409`, `:410`, `:411`, `:413`)

- **6: fechado no pedido literal; mordida pertinente.** Os casos dia vazio, `REFAZENDO`, ontem e número vazio cobram `?`; o mutante registrado cai nessa comparação. Status vazio não foi incluído no selo. (`bin/placar_code.sh:37`; `bin/tests/test_sonda_sem_grant.sh:434`, `:436`, `:437`, `:438`, `:439`, `:440`, `:441`, `:442`)

- **10: falha silenciosa original fechada, identidade ainda PROCEDENTE.** Cleanup preserva rc e acusa container ainda encontrado; o selo mata o comportamento antigo por rc e argv. Porém o trap antecede a criação: se `run --name "$PGT"` falhar por colisão, `EXIT` chama `rm -f "$PGT"` sobre o container alheio. Cura: registrar o ID da criação bem-sucedida, remover somente esse ID e não remover após criação recusada. O selo não cobre colisão. (`bin/tests/test_sonda_sem_grant_pg.sh:39`, `:41`, `:42`, `:44`, `:48`, `:50`, `:76`, `:80`, `:82`, `:93`, `:95`)

- **Nome vazio: NÃO PROCEDE no fluxo atual.** A atribuição sempre contém prefixo literal e PID; a alteração de `PGT` no teste fica no subshell. **“Nome exato criado pelo selo”: não provado**, pois exatidão do nome não prova autoria. (`bin/tests/test_sonda_sem_grant_pg.sh:35`, `:72`, `:73`, `:74`)

- **Limite da verificação de limpeza:** `rm` falha sem exame direto e qualquer erro de `inspect` equivale a “ausente”; o fake não distingue ausência de falha da consulta. Falha efetiva do Docker **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:41`, `:42`, `:85`, `:86`, `:87`, `:88`)

## 09/11 10:50 · sessao 72a0c7209703 · - · - · origem=Ronald (tmux)

- **PROCEDENTE quanto à diferença de cobertura; cura automática ampliada pode ir à FILA.** `REVOKE ... ON SCHEMAS` cura o caso `$PGU → leitor`, mas a conferência conta defaults de **qualquer** concedente e também de `PUBLIC`; a porta só revoga defaults `FOR ROLE $PGU ... FROM $ROLE`. Isso vale para schemas, tabelas e sequências. (bin/sonda_provisionar.sh:125-132; bin/sonda_leitor_comum.sh:119-122)
- **Não há falso pronto nesse caso:** `default_escrita != 0` reprova o parser; leitura sai rc 5 e porta sai rc 1 antes de promover `.env_leitor`. Há bloqueio persistente sem autocura pela porta. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,129-135; bin/sonda_provisionar.sh:147-151,171)
- **Quem cura:** o código manda `REASSIGN/REVOKE a mao`; para defaults, a correção sugerida é um operador autorizado revogar no concedente/objeto/escopo original, para `leitor` ou `PUBLIC`. A realização disso em produção é **[nao verificado]**. (bin/sonda_provisionar.sh:150; bin/sonda_leitor_comum.sh:119-122)
- **Item 2b fechado no domínio testado:** novo estrago é `saas_user → leitor`, `USAGE ON SCHEMAS WITH GRANT OPTION`; a estrutura exige chave positiva, rc 5, chave na recusa, sonda não executada e recuperação pela porta. Outros concedentes e defaults para `PUBLIC` não têm casos nesse conjunto. (bin/tests/test_sonda_sem_grant_pg.sh:179-191,192-209)
- **Item 2a fechado estruturalmente:** estrago SUPERUSER chega à consulta real e às mesmas quatro cobranças; a porta escreve NOSUPERUSER. (bin/tests/test_sonda_sem_grant_pg.sh:156-159,182-189,207; bin/sonda_leitor_comum.sh:68; bin/sonda_provisionar.sh:105)

Mapa dos insumos SQL alterados:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| Default schema de `$PGU` para `leitor` | Porta: REVOKE. (bin/sonda_provisionar.sh:132,144) | Conferência `defaclobjtype='n'`, ACL/grantee/privilégio/grantable. (bin/sonda_leitor_comum.sh:119-122) |
| `rolsuper` | Porta NOSUPERUSER; fixture SUPERUSER. (bin/sonda_provisionar.sh:105; bin/tests/test_sonda_sem_grant_pg.sh:207) | Consulta `super`; parser e consumidores. (bin/sonda_leitor_comum.sh:68,132,145; bin/sonda_leitura.sh:187; bin/sonda_provisionar.sh:148) |
| `default_escrita` | Derivado da consulta, sem escritor materializado de catálogo com esse nome. (bin/sonda_leitor_comum.sh:119-122) | Parser; porta antes/depois; leitura; selo PG. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_provisionar.sh:72,147-148; bin/sonda_leitura.sh:184-187; bin/tests/test_sonda_sem_grant_pg.sh:182-186) |
| `PGU/ROLE/SCHEMAS/PG/BANCO` | Constantes e `_destino`. (bin/sonda_leitor_comum.sh:8-15,22-25) | Geração SQL e execução psql da porta/conferência. (bin/sonda_provisionar.sh:51,125-132; bin/sonda_leitor_comum.sh:64,96,120) |
| `.env_leitor` / senha | Porta, temporário e promoção. (bin/sonda_provisionar.sh:168-171) | Porta lê senha; leitura valida/lê e passa ao container. (bin/sonda_provisionar.sh:77; bin/sonda_leitura.sh:173-178,234) |

## 09/11 10:39 · sessao c1c80cb6a02e · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**Há achados PROCEDENTES; GREEN, mortes M1–M38 e resultado em produção permanecem [nao verificado].**

1. **ATO ÚNICO — NAO PROCEDE para par identificado; garantia delimitada.** Validação, varredura e O330 passam pelo envelope comum. As duas aplicações, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. Sem par identificado, continua possível aplicar uma pergunta individualmente, inclusive com irmã já aplicada. `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1431`.

   **PROCEDENTE — paridade sobre universo errado.** O julgamento posterior usa vizinhas sem excluir batidas retratadas. Uma S retratada às 01:30 pode reprovar S01/E02, embora o conjunto vivo alterne. **Cura:** usar o universo canônico de apuração e testar esse contracaso. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

   O censo AST com apelidos encontrou os plantadores de Pergunta nos ramos hora/data-hora. Regularização externa e veredito do dia têm unidades próprias e permitem resultados parciais: **FILA**, auditoria separada. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/regularizacao_ext.py:166`; `app/ponto/services/veredito_celula.py:320`.

2. **ROLLBACK — limpeza funciona, mas descarimbo vazio tem efeitos.** Após restaurar o banco, o código decide pelo lastro, descarimba quando reprovado, grava recusa nas duas e chama a devolução. `app/chamados/services/materializacao.py:1386`.

   | Campo | Resultado após o tratamento da falha |
   |---|---|
   | `materializada_em/por`, `via_resolucao` | Lastro reprovado: `None/None/''`; lastro aprovado: conserva valores restaurados. `app/chamados/services/materializacao.py:1391`; `app/chamados/models.py:3211`. |
   | `validada_em/por` | Limpos quando é hora declarada, sem materialização, com recusa; não há limpeza incondicional para qualquer tipo. `app/chamados/services/validacao.py:190`; `app/chamados/services/validacao.py:198`. |
   | `recusa_motivo` | Recebe a recusa conjunta, truncada em 240 caracteres. `app/chamados/services/materializacao.py:1393`; `app/chamados/services/materializacao.py:1181`. |

   **PROCEDENTE — `save` sem mudança não é inócuo.** Mesmo com os três campos vazios, `descarimbar` salva com `update_fields=['via_resolucao','materializada_em','materializada_por']`; isso agenda o beco e insere EventoCelula quando há célula, sem deduplicação nessa emissão. **Cura:** tornar o descarimbo vazio um no-op na porta dona, comparando os três campos. `app/chamados/models.py:3211`; `app/chamados/signals.py:225`; `app/chamados/signals.py:242`; `app/chamados/models.py:3248`; `app/ponto/portas/celula.py:891`.

   Esse `save` não chama diretamente LogAuditoria nem re-lavra o veredito da pergunta. A devolução acrescenta E1 quando remove validação e audita quando sua foto muda; a nota de recusa depende de mudança do motivo. O censo estático conta sítios, não quantas vezes esse `save` executa. `app/chamados/models.py:3206`; `app/chamados/services/validacao.py:208`; `app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:1195`; `app/core/censo_escritas.py:451`.

   **“2x = mesmo estado e mesma trilha” não está demonstrado.** Repetir diretamente o descarimbo pode acrescentar eventos; repetir o envelope depois da devolução encontra ambas sem validação e não reaplica o par. `app/chamados/models.py:3215`; `app/ponto/portas/celula.py:891`; `app/chamados/services/materializacao.py:1415`; `app/chamados/services/materializacao.py:1433`.

   **NAO PROCEDE quanto a perda comprovada de carimbo legítimo fora do fato:** essas linhas são excluídas da identificação e a própria porta recusa descarimbá-las; vias de identidade carimbadas são absolvidas pelo lastro. Outro caso legítimo perdido nesta população: **[nao verificado]**. `app/chamados/services/materializacao.py:72`; `app/chamados/models.py:3209`; `app/chamados/juizes.py:670`.

3. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores.** Ambos começam pelo mesmo Colaborador, depois Disputas, Chamados e Perguntas, ordenados por PK; a pergunta é relida após travar. O330 antecipa o universo da competência. Prova concorrente com duas conexões: **FILA**, não executada. `app/chamados/services/materializacao.py:125`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

4. **ESTADO/FECHAMENTO — dois achados PROCEDENTES.**

   - **C7 tem um segundo julgamento dentro de `fechar`.** `pode_fechar` retém espera, mas `fechar` verifica respostas/C6 sem consultá-lo. Com metade aceita e irmã retratada, a porta pode fechar apesar de C7 negar. **Cura:** consultar C7 sob trava; testar a chamada real a `fechar`. P2 só testa `pode_fechar`. `app/chamados/models.py:1863`; `app/chamados/models.py:1897`; `app/chamados/models.py:1907`; `app/chamados/models.py:1943`; `app/chamados/tests/test_bug_par_intervalo.py:427`.
   - **Espera confunde resposta com aceite.** Com ambas respondidas e nenhuma validada, o predicado já retorna espera. O explicador pula “aguarda validação”; “Aplicar declaração” retorna antes de validar a própria pergunta. **Cura:** exigir validação da própria metade para esse estado. `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:117`; `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:678`; `app/chamados/services/declaracao_texto.py:709`.

   | Leitor | O que efetivamente lê/diz |
   |---|---|
   | C6, painel e badge | Validação mata a pergunta; o crédito continua por perguntas vivas/verbos. `app/chamados/juizes.py:1153`; `app/chamados/services/raiox.py:72`; `app/chamados/services/verbos.py:34`. |
   | Fio | Aberta carrega perguntas; fechada só admite C6 vivas, ocultando a espera se o fechamento escapar. `app/chamados/services/fio.py:221`; `app/chamados/services/fio.py:238`. |
   | Explicador, declaração, API | Leem/publicam o predicado de espera. `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:484`; `app/api/views.py:1126`. |
   | Vigia de carimbo/O330 | Separam espera da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`; `app/chamados/services/passivo_validado.py:324`. |
   | `vigia_fio`, `o_que_segura` | Continuam por C6 e autoridades de alcance/dono; não tratam espera como fio vivo. `app/chamados/services/vigia_fio.py:164`; `app/folha/fora_da_folha.py:221`; `app/chamados/juizes.py:1322`. |
   | Crons | Beco consulta C7; alerta diário continua avisando disputas abertas. Não há reabertura automática por espera nesses caminhos. `app/chamados/services/becos.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |

5. **IDENTIDADE — NAO PROCEDE a derivação paralela na tag.** O identificador consulta pausas efetivas/DNA e se abstém por multiplicidade; a tag chama o mesmo predicado. `par_ambiguo` preserva o despacho avulso, não constitui prova de equivalência integral de estado/trilha com HEAD. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:88`; `app/escala/models.py:1189`; `app/chamados/templatetags/chamados_tags.py:18`; `app/chamados/services/materializacao.py:1431`.

6. **TEXTO/TELA — catálogo e proximidade corrigidos; percurso incompleto.** “Colada” exige testemunha viva nos ±60s inclusivos compartilhados com a porta; espera tem entrada própria. **NAO PROCEDE** a duplicação desses predicados. `app/chamados/services/materializacao.py:1357`; `app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:115`; `app/chamados/catalogo/recusa.py:25`.

   **PROCEDENTE — aviso some do detalhe após recarga.** Os dois includes existem, mas o contexto do detalhe exclui a metade validada por C6. P5 injeta a pergunta diretamente. **Cura:** incluir espera no filtro de apresentação e testar GET real. `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/modal_fio.html:160`; `app/chamados/services/acoes_chamado.py:348`; `app/chamados/tests/test_bug_par_intervalo.py:486`.

7. **SELOS/SEMÂNTICA.** M7 e M35 agora têm asserções discriminantes estáticas; mortes executadas continuam **[nao verificado]**. **PROCEDENTE:** P6/M38 não provam a alteração específica v3: começam com carimbo preenchido, portanto passariam com a guarda anterior que só descarimbava nesse caso. **Cura:** mutante restaurando exatamente aquela guarda, cenário já vazio e contagem de saves/eventos/trilha em duas chamadas. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

   **FILA — relógio e dia civil/turno.** Há `now()` por metade no carimbo/aprovação; passar um instante do ato eliminaria essa dispersão. As derivações continuam pelos helpers existentes; contraprova de fuso ou competência neste dado: **[nao verificado]**. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`; `app/chamados/services/materializacao.py:1351`.

8. **ESCRITORES/LEITORES adicionais dos campos centrais**, além dos consumidores acima:

   | Campo | Escritores / leitores fora do diff |
   |---|---|
   | Resposta e `respondida_em` | Resposta humana/automática/trilha; carimbo preenche ausente. Leitura pela derivação do slot. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134`; `app/chamados/juizes.py:339`. |
   | `validada_em/por` | Validação, devolução, nova resposta, preenchimento pelo carimbo e reabertura. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137`; `app/chamados/models.py:1565`. |
   | Materialização/via | Portas de carimbo/descarimbo, lote, encerramento por fato e relavra; leitores C6/lastro/lavra persistida. `app/chamados/models.py:3100`; `app/chamados/models.py:3267`; `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125`; `app/chamados/juizes.py:645`; `app/chamados/veredito_lavrado.py:187`. |
   | Recusa | Identidade, pendência, recusa canônica/humana e escape; leitores catálogo e devolução. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/catalogo/recusa.py:92`; `app/chamados/services/validacao.py:195`. |
   | `contexto_json` | Portas anotar/renovar/retirar/fundir; marco faltante lê seu conteúdo. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161`; `app/chamados/juizes.py:508`. |
   | Batida: instante/tipo/origem/vínculo | `registrar_batida` e `anotar`; leitores tolerância, vizinhança e lastro. `app/ponto/registro_batida.py:104`; `app/chamados/services/materializacao.py:1038`; `app/ponto/registro_batida.py:94`; `app/chamados/services/materializacao.py:169`; `app/chamados/juizes.py:732`. |

   Exaustividade de **todos** os leitores transitivos de cada campo/função na árvore: **[nao verificado]**.

## 09/11 10:37 · sessao 54312d58dc6d · - · - · origem=Ronald (tmux)

- **PROCEDENTE — teste da alteração específica v3 insuficiente.** P6 começa com `materializada_em` preenchido; restaurar a guarda anterior `materializada_em is not None and not lastro` continuaria satisfazendo esse teste. M38 inverte o lastro, sem testar o descarimbo já vazio. Acrescentar cenário vazio, captura dos efeitos de `save` e repetição 2x com contagem de trilha. `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/test_bug_par_intervalo.py:669`; `app/chamados/tests/test_bug_par_intervalo.py:529`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

- **PROCEDENTE — universo da paridade inclui retratadas.** O novo ato consulta `_vizinha_mesmo_tipo`, que busca `Batida` sem excluir retratadas, e recusa incondicionalmente a testemunha encontrada. Contraexemplo estático: sequência viva E18:32/S01:00/E02:00/S06:30 mais S retratada01:30 faz a saída encontrar S01:30 e recusar o conjunto. Cura no helper, usando `batidas_apuraveis`, com RED desse cenário. Ocorrência em produção [nao verificado]. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

- **NAO PROCEDE — M7/M35 continuam sem discriminação estática.** M7 agora encontra DNA com uma pausa e quatro perguntas, exigindo `par_ambiguo`; M35 precisa produzir o aviso HTML exato uma vez no modal, e o runner troca também o filtro registrado. Mortes efetivamente executadas [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:288`; `app/chamados/tests/test_bug_par_intervalo.py:493`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/mutantes_bug_par_intervalo.py:230`.

- **NAO PROCEDE — nova cópia literal da proximidade/identidade.** Porta e ato leem `batida_mesmo_tipo_na_tolerancia`; identidade consulta `escala_vigente → vinculo_do_dia`, `pausas_do_dia` e mapa canônico existente. A guarda pós-par reutiliza o helper antigo; isso não elimina o defeito de universo acima. `app/ponto/registro_batida.py:115`; `app/chamados/services/materializacao.py:1357`; `app/chamados/services/materializacao.py:87`; `app/escala/servico_jornada.py:57`; `app/escala/models.py:1197`; `app/chamados/juizes.py:882`.

- **FILA — relógio por metade.** `_carimbar_aplicacao` lê `now()` internamente e é chamado para cada metade; aprovação também lê por escrita. A expressão foi extraída do escritor anterior segundo o diff fornecido; contraprova temporal neste dado [nao verificado]. Passar um instante do ato seria a cura estrutural. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`.

Inventário de escritores fora e dentro do diff:

| Campo | Escritores |
|---|---|
| `materializada_em/por` | `_escrever_carimbo`, chamado pelas portas; descarimbo em lote. `app/chamados/models.py:3100`; `app/chamados/models.py:3267` |
| `via_resolucao` | Mesmas portas, mais encerramento por fato e relavra. `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125` |
| `validada_em/por` | Validar, devolver, responder novamente, preencher ausente no carimbo. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137` |
| `resposta_colab/respondida_em` | Resposta humana, automática, recuperação pela trilha; carimbo preenche instante ausente. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134` |
| `recusa_motivo` | Identidade, pendência do plantio, recusa canônica, limpeza do par, recusa humana, escape da resposta. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/materializacao.py:1378`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494` |
| `contexto_json` | Portas anotar/renovar/retirar/fundir. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161` |

A existência das asserções não certifica GREEN, RED anterior ou morte dos mutantes: [nao verificado].

## 09/11 10:37 · sessao 30e4a759213b · - · - · origem=Ronald (tmux)

**PROCEDENTE — C7 duplicado na porta de fechamento.** `pode_fechar()` agora retém espera, mas `fechar()` continua julgando apenas respostas sem veredito e perguntas vivas; não consulta C7. Com primeira metade validada e irmã retratada, C7 recusa e `fechar(user)` pode gravar fechamento. Cura: porta consultar C7 sob trava; RED chamar `fechar`, além de `pode_fechar`. `app/chamados/models.py:1863`, `app/chamados/models.py:1897`, `app/chamados/models.py:1907`, `app/chamados/models.py:1943`. O novo P2 só verifica `pode_fechar`. `app/chamados/tests/test_bug_par_intervalo.py:427`.

**PROCEDENTE — espera confunde resposta com aceite.** O predicado não exige validação da própria pergunta quando identifica par. Ambas respondidas, nenhuma validada: ambas viram `aguardando_par`, e o explicador deixa de oferecer validar a própria resposta. Cura: exigir `p.validada_em` para espera de metade aceita. `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:117`, `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:85`.

Isso também bloqueia portas: declaração do colaborador retorna antes de permitir alterar resposta; “Aplicar declaração” retorna antes de validar a própria pergunta já respondida. `app/chamados/services/declaracao_texto.py:382`, `app/chamados/services/declaracao_texto.py:424`, `app/chamados/services/declaracao_texto.py:678`, `app/chamados/services/declaracao_texto.py:709`.

**PROCEDENTE — aviso desaparece do detalhe após recarga.** O include existe, mas o contexto do detalhe seleciona apenas C6 vivas; a metade aceita é C6 morta. O retorno HTMX mostra aviso porque renderiza a pergunta diretamente, enquanto recarregar exclui aquela pergunta. Cura: filtro de apresentação incluir o predicado de espera. `app/chamados/services/acoes_chamado.py:348`, `app/chamados/juizes.py:1153`, `app/templates/chamados/detalhe_local.html:309`, `app/chamados/views.py:399`, `app/chamados/views.py:404`. P5 injeta pergunta diretamente e não testa essa montagem. `app/chamados/tests/test_bug_par_intervalo.py:487`.

**NÃO PROCEDE — templatetag derivando identidade paralela.** A tag chama o mesmo predicado; cartão e modal incluem o mesmo fragmento. `app/chamados/templatetags/chamados_tags.py:18`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:160`. A identidade lê pausas efetivas da escala; essa autoridade lê DNA da célula antes do template. `app/chamados/services/materializacao.py:86`, `app/escala/models.py:1189`, `app/escala/models.py:1197`, `app/escala/models.py:1207`.

Inventário compacto dos demais leitores:

| Leitor | Efeito |
|---|---|
| C6 / consulta viva | Aceite mata pergunta; SQL também a exclui. `app/chamados/juizes.py:1153`, `app/chamados/models.py:3362`. |
| Painel/badge | Crédito usa consulta viva; irmã não validada determina Cobrar/Validar. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`. |
| Fio | Disputa aberta carrega perguntas; fechada só admite C6 vivas, ocultando aceite pendente se fechamento escapar. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:238`. |
| Explicador | Espera antes de estado; fechamento por C7. `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:144`. |
| Projeção/API | Publicam espera e retiram aceitação. `app/chamados/services/declaracao_texto.py:484`, `app/api/views.py:1126`. |
| Vigia de carimbo/O330 | Espera sai da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`, `app/chamados/services/vigia_carimbo.py:133`, `app/chamados/services/passivo_validado.py:324`. |
| Vigia do fio | Continua julgando fio por C6; só importa espera na abertura de classes da porta. `app/chamados/services/vigia_fio.py:164`, `app/chamados/services/vigia_fio.py:370`, `app/chamados/services/vigia_fio.py:462`. |
| `o_que_segura` | Lê `porta_alcancavel_do_dia`, alimentada por C6 e verbos do painel. `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:221`, `app/chamados/juizes.py:1322`, `app/chamados/juizes.py:1396`. |
| Alerta diário | Toda disputa aberta segue recebendo aviso; não distingue espera. `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |
| Auditor I18 | Só acusa recusa vazia; espera normal possui código e não entra. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/services/materializacao.py:1436`. |

Se irmã sair do fato, o vigia poderá classificar o dia como sem fio, passando pelos outros donos; alarme efetivo depende da célula e desses donos **[nao verificado]**. `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:430`.

## 09/11 10:36 · sessao 463d4de49e19 · - · - · origem=Ronald (tmux)

**PROCEDENTE — paridade inclui batidas retratadas.** O pós-par consulta `_vizinha_mesmo_tipo` após ambas as escritas, mas esse helper lê `Batida.objects` sem excluir `retratada_em`; uma S retratada às 01:30 reprova S01/E02 mesmo quando o conjunto vivo alterna. Cura: consumir `batidas_apuraveis`, acrescentando RED com retratada entre metades. `app/chamados/services/materializacao.py:1366`; `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; autoridade efetiva: `app/ponto/turnos.py:2866`. Ocorrência em produção: [nao verificado].

**NAO PROCEDE — divisão transacional para par identificado.** Validação direta e varredura chegam a `_materializar_uma`; O330 também, com inclusão explícita dos carimbos sem lastro. O envelope identifica o par, espera ambas respondidas/validadas e chama `_materializar_par`; as duas escritas, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. `app/chamados/services/validacao.py:469`; `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1414`; `app/chamados/services/materializacao.py:1433`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1380`.

A garantia tem universo delimitado: quando identidade retorna `None`, aplica-se uma pergunta avulsa. Portanto, o código admite plantio individual para intervalo sem par elegível; isso inclui irmã já materializada e casos ambíguos, preservando o caminho avulso. `app/chamados/services/materializacao.py:73`; `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1431`; teste explícito da irmã aplicada: `app/chamados/tests/test_bug_par_intervalo.py:725`.

**NAO PROCEDE — inversão entre dois validadores dessas metades.** Ambos adquirem primeiro a mesma linha do Colaborador, depois disputas, chamados e perguntas; os grupos são ordenados por PK. A validação relê a pergunta após travar. O330 trava o conjunto da competência antes dos dias. `app/chamados/services/materializacao.py:125`; `app/chamados/services/materializacao.py:134`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

**FILA — prova concorrente.** O teste confere argumentos das travas, sem duas conexões concorrentes; ausência operacional de deadlock é [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:605`.

**Censo de plantadores:** os únicos consumidores de `registrar_batida` cujo plantio recebe `PerguntaDisputa` são os ramos hora e data/hora; ambos chegam pelo envelope comum. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/materializacao.py:1477`.

Demais consumidores encontrados por AST, considerando nomes importados/apelidos e chamadas por atributo: APIs de chão (`app/api/views.py:684`, `app/api/views_core.py:681`), exceção (`app/chamados/services/acoes_chamado.py:44`), regularização externa (`app/chamados/services/regularizacao_ext.py:77`), justificativas (`app/ponto/services/justificativa.py:188`, `app/ponto/services/justificativa.py:361`, `app/ponto/views.py:1815`), triagem (`app/ponto/services/triagem_batida.py:701`) e veredito do dia (`app/ponto/services/veredito_celula.py:322`). Instrumento PWA: `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`.

**FILA — unidades externas parcialmente aplicáveis.** Regularização externa continua após falha de metade e pode marcar lote materializado; veredito do dia acumula erro por marco e só reverte se nenhuma ação ocorreu. Não são o aceite das duas PerguntaDisputa. `app/chamados/services/regularizacao_ext.py:166`; `app/chamados/services/regularizacao_ext.py:191`; `app/chamados/services/regularizacao_ext.py:197`; `app/ponto/services/veredito_celula.py:320`; `app/ponto/services/veredito_celula.py:342`.

## 09/11 10:29 · sessao cbbbf78661b6 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

_3118 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
