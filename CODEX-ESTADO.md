# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 15:28** · publicado em **29/10/2026 15:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 15:28 · sessao 7e4719651533

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 1319444918a3

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 1258252f08e3

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao cdb1b9f8e444

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 126426b53b16

ok
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:28 · sessao 3ccabb649915

ok.
bin/achado_por.py:1, bin/alarme_sem_fatia.sh:1, bin/alarme_sessao_ociosa.py:1

## 29/10 15:25 · sessao 23341f1d3b2f

*Numeração de `anexo_log.txt`: `# ponto.log de hoje...` é a linha 1; linhas vazias contam.*

- **Sim, o POST de atestado do app chegou em 29/10, do user 903**: recebido às **11:49:44,630** (`origem=api_app`); após parse, às **11:49:44,754**, registrou `tipo=atestado arquivo=True`. O acesso confirma `POST /api/ausencias/ → 200` às **11:49:44**. (`anexo_log.txt:2–3,25`)
- **PWA, user 1006**: uploads de atestado às **15:21:15,054** e **15:21:17,447**, `origem=pwa`; os POSTs em `/ponto/ausencias/submeter/` retornaram **302** às **15:21:15** e **15:21:17**. (`anexo_log.txt:4–5,39–40`)
- Na `api_ausencias`, **antes do parse**, `post_recebido` registra origem, user, tamanho declarado, content-type (até 60 caracteres) e teto dos campos; o primeiro acesso a `request.data` vem depois. ([app/api/views.py:1767](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1767), [1772](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1772))
- **Após parse**, `upload_ausencia` registra origem, user, tipo, presença do arquivo, tamanho, content-type e plataforma. ([app/api/views.py:1832](/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/app/api/views.py:1832)) Correspondência dos users com `colNN`: **[não verificado]**.

## 29/10 15:22 · sessao 95904850ee23

stdout [nao verificado]:
```text
rc=1
```

stderr [nao verificado]:
```text
/home/ronald/wt-codexpro-2910-1525/bin/sonda_leitura.sh: line 132: /home/ronald/wt-codexpro-2910-1525/logs/.env_leitor.novo: Read-only file system
```

Código de saída do comando solicitado: `1`. [nao verificado]

## 29/10 15:09 · sessao 1068ea4b2151

CLI_OK

## 29/10 15:05 · sessao 13f1f90d816f

CLI_OK

## 29/10 14:52 · sessao efe6b065e388

O risco de o futuro vencer pressupõe que ele satisfaça os filtros da consulta, especialmente `ativa=True`. A ordenação padrão é `-data_inicio`: `.first()` favorece o futuro; mapas que sobrescrevem por colaborador nessa ordem conservam o mais antigo. (`app/escala/models.py:877`; `app/escala/models.py:1003`)

Contei pontos lógicos de leitura, separando seleção de vínculo de leitura independente de posto. Chamadas ao mesmo helper não foram recontadas. Impacto efetivo nos dados de **bo09**: [não verificado].

**Risco alto — vínculo futuro pode fornecer escala, marco, veto ou alvo de alteração antes da sua vigência.**

| # | Arquivo:linha | Função | O que alimenta |
|---|---|---|---|
| A01 | `app/escala/utils.py:920` | `data_saida_esperada` | Saída esperada da entrada; escolhe ativa, sem fim, mais recente. |
| A02 | `app/colaboradores/services/calendario.py:341` | `contexto_calendario` | Grade mensal; primeiro ativo. A guarda pode ocultar dias sem célula cobertos pelo vínculo anterior. |
| A03 | `app/colaboradores/views.py:268` | `painel_vinculo` | Agenda de 14 dias pelo primeiro ativo. |
| A04 | `app/colaboradores/views.py:2202` | `agenda_modal_view` | Semana navegável pelo primeiro ativo. |
| A05 | `app/colaboradores/views.py:2860` | `disponibilidade` | Retorno de folga; ativos com `setdefault` conservam o futuro. |
| A06 | `app/colaboradores/views_fase.py:45` | `fase_12x36` | Tela de fase, âncora e comparação de calendários pelo ativo mais recente. |
| A07 | `app/colaboradores/services/drawer.py:22` | `contexto_drawer` | Escala escolhida controla também se a grade será montada. |
| A08 | `app/escala/views.py:17` | `minha_escala` | Previsão de 14 dias com um único primeiro ativo. |
| A09 | `app/escala/services/furos_diarios.py:29` | `apurar_colab` | Ativo futuro pode vetar a apuração inteira antes da leitura da grade. |
| A10 | `app/escala/services/furos_diarios.py:291` | `universo_empresa` | Classificação LIMBO/suspeita e previsão da janela pelo ativo mais recente. |
| A11 | `app/escala/services/impacto_troca.py:34` | `impacto_da_troca` | Âncora, furos anteriores e aviso de troca retroativa. |
| A12 | `app/escala/services/escala_auto.py:172` | `dry_da_frota` | Mais recente ativo sem fim alimenta propostas baseadas no histórico. |
| A13 | `app/ponto/services/espelho.py:42` | `piso_visual` | Início futuro eleva o piso do espelho/calendário, limitado a hoje. |
| A14 | `app/ponto/turnos.py:2428` | `proximo_tipo_de` | Tipo futuro decide o ramo intermitente da sugestão E/S. |
| A15 | `app/ponto/turnos.py:2447` | `proximo_tipo_de` | Horários futuros desempatam retorno do intervalo versus saída. |
| A16 | `app/ponto/services/faltas_de_hoje.py:35` | `faltas_de_hoje_sem_cobranca` | Cron/contador: LIMBO, trabalho e marco de hoje. |
| A17 | `app/ponto/services/abriu_nao_bateu.py:194` | `_hora_de_bater_passou` | Cron/contador julga o dia pelo marco do primeiro ativo. |
| A18 | `app/ponto/management/commands/censo_fase_12x36.py:295` | `Command._curar` | Tipo do ativo mais recente entra na cura com virada histórica. |
| A19 | `app/ponto/management/commands/reconciliar_fantasmas.py:172` | `Command.handle` | Diagnóstico de completude de dias antigos pelo tipo escolhido hoje. |
| A20 | `app/api/views.py:312` | `api_me` | Escala de hoje apresentada no app pelo primeiro ativo. |
| A21 | `app/api/views_core.py:301` | `api_me` | Implementação do kernel: mesma escolha para a escala de hoje. |
| A22 | `app/api/views.py:918` | `api_escala_v2` | Previsão de 14 dias com um único primeiro ativo. |
| A23 | `app/chamados/juizes.py:1251` | `_marco_esperado_da_grade` | Defesa/classificação: marco esperado do fato pelo ativo mais recente. |
| A24 | `app/chamados/services/desvio_escala.py:44` | `medir_desvio` | Diagnóstico de batidas de 14/84 dias contra a escala escolhida hoje. |
| A25 | `app/inteligencia/calculadores.py:319` | `_calc_escalas_desvio` | Tipo atual decide exclusão de intermitente e comparação com 30 dias. |
| A26 | `app/ferias/management/commands/alertar_retornos.py:28` | `Command.handle` | Cron/push de hoje e amanhã; `setdefault` conserva o futuro. |
| A27 | `app/folha/export.py:787` | `classificar_export` | Qualquer ativo 12x36 sem âncora pode bloquear aprovação da competência. |
| A28 | `app/core/views.py:401` | `dashboard_supervisor` | Lista de ausentes de hoje pelo primeiro ativo. |
| A29 | `app/chamados/views.py:442` | `detalhe_chamado_local` | Corroborar régua pode atingir o vínculo futuro do template do chamado. |
| A30 | `app/colaboradores/views.py:226` | `vincular_folga` | Primeiro ativo recebe calendário e materialização do mês atual/próximo. |
| A31 | `app/colaboradores/services/vinculo.py:388` | `_executar_aceite` | Ativo mais recente vira `ec_nova` do aceite com `data_inicio` específica. |

A célula existente pode preservar trabalho e marcos do dia, reduzindo parte desses efeitos; isso não corrige automaticamente a seleção nem os vetos anteriores à leitura da célula. (`app/escala/models.py:1066`; `app/escala/models.py:1094`; `app/escala/services/furos_diarios.py:29`)

**Risco alto condicionado à mudança de posto — posto cadastral atual decide fatos de outra data.**

| # | Arquivo:linha | Função | O que alimenta |
|---|---|---|---|
| B01 | `app/core/feriados.py:113` | `lugar_do_colab` | Município atual para feriados usados por espelho, PDF, fechamento e célula. |
| B02 | `app/ponto/models.py:100` | `Batida.posto_para_calculo` | Fallback central: sem `posto_efetivo`, a batida assume o posto cadastral atual. |
| B03 | `app/ponto/management/commands/detectar_ausencias.py:623` | `Command._abrir_chamado` | Posto atual decide veto de feriado para `data_ref`. |
| B04 | `app/ponto/services/geofence.py:48` | `vinculo_contagem` | Últimas batidas GPS comparadas ao posto atual. |
| B05 | `app/ponto/services/vinculo_na_lista.py:65` | `achar` | Posto/colegas atuais julgam amostra histórica de GPS. |
| B06 | `app/colaboradores/services/feriado_do_posto.py:99` | `presenca_em_feriado` | Município atual classifica células dos últimos 120 dias. |
| B07 | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:95` | `Command._marcar` | Posto atual seleciona chamados cuja data do fato será julgada. |
| B08 | `app/colaboradores/services/situacional.py:179` | `montar_situacional` | Tela compara GPS da batida ao posto cadastral atual. |
| B09 | `app/colaboradores/painel_cell.py:40` | `montar_linha_colab` | Atualização da célula compara GPS ao posto cadastral atual. |
| B10 | `app/chamados/services/fila_por_causa.py:164` | `particionar` | Município atual classifica feriados de chamados históricos. |

B01 propaga-se, sem nova escolha datada de posto, para `feriados_do_colab`, fechamento, PDF e `feriado_vira_folga`. (`app/ponto/services/espelho.py:147`; `app/ponto/services/fechamento.py:622`; `app/relatorios/pdf_espelho.py:294`; `app/escala/models.py:1363`)

**Risco médio/alto — conserva um vínculo para toda a janela, percorre ambos ou usa existência de ativo como condição.**

| # | Arquivo:linha | Função | O que alimenta |
|---|---|---|---|
| C01 | `app/escala/views_plano_folgas.py:55` | `_grid_ctx` | Grade mensal cria uma linha por ativo; atual e futuro coexistem. |
| C02 | `app/escala/services/furos_diarios.py:148` | `ranking` | Histórico agregado pelos templates ativos; pode entrar nos dois. |
| C03 | `app/escala/services/fase_12x36.py:106` | `fora_de_fase` | Mapa de ativos conserva o mais antigo para a janela de fase. |
| C04 | `app/escala/services/meio_periodo.py:152` | `observado_meio_periodo` | Mapa aplica um template aos turnos históricos. |
| C05 | `app/escala/services/propositor.py:414` | `propostas` | Todos ativos são comparados ao período; futuro também gera proposta. |
| C06 | `app/escala/services/cadastro_realidade.py:190` | `_jornada_que_mente` | Mapa de tipos ativos alimenta cálculo/diagnóstico das células. |
| C07 | `app/escala/management/commands/celulas_contra_o_template.py:28` | `contra_o_template` | Mapa ativo compara o template conservado ao acervo de células. |
| C08 | `app/escala/services/fase_conflitante.py:71` | `medir` | Diagnóstico de foto/âncora de todos ativos sobre a janela inteira. |
| C09 | `app/escala/management/commands/termometro_regua.py:35` | `Command.handle` | Cron avalia ativos, inclusive futuro, contra histórico de batidas. |
| C10 | `app/ponto/selecao_periodo.py:278` | `turnos_em_turno_stored` | Ordem ascendente com `setdefault` conserva o antigo para presença/tela. |
| C11 | `app/ponto/services/piso_intervalo.py:28` | `pares_abaixo_do_piso` | Todos ativos podem repetir colaborador e aplicar marcos distintos à janela. |
| C12 | `app/ponto/services/bordas_realizado.py:62` | `bordas_do_realizado` | Todos ativos reaplicam regime/intervalo à mesma janela. |
| C13 | `app/ponto/services/triagem_batida.py:92` | `triagem_bater_ponto` | Qualquer ativo, inclusive futuro, satisfaz a condição de possuir escala. |
| C14 | `app/ponto/services/cluster_espurio.py:75` | `reconciliar_colab` | Ativo sem fim decide entrada na reconciliação histórica. |
| C15 | `app/ponto/management/commands/detectar_par_relampago.py:335` | `Command.handle` | Todos ativos sem fim podem repetir processamento de pares. |
| C16 | `app/ponto/management/commands/detectar_cluster_espurio.py:36` | `Command.handle` | Todos ativos sem fim podem repetir reconciliação de clusters. |
| C17 | `app/ponto/management/commands/tripwire_celulas.py:17` | `medir_stale` | Mapa conserva um ativo para comparar DNA de células futuras. |
| C18 | `app/ponto/management/commands/propor_reorganizacao.py:42` | `Command._run` | Qualquer ativo intermitente exclui a pessoa da janela histórica. |
| C19 | `app/ponto/management/commands/fechar_chamados_meio_periodo.py:59` | `Command.handle` | Mapa ativo fornece template a chamados de até 90 dias. |
| C20 | `app/ponto/management/commands/detectar_intervalo_ausente.py:110` | `Command.handle` | Existência de ativo decide reconciliação de chamados antigos. |
| C21 | `app/ponto/management/commands/diff_espelho_alimentacao.py:94` | `Command.handle` | Sonda: ramo A do motor recebe um ativo para toda competência. |
| C22 | `app/ponto/management/commands/detectar_ausencias.py:184` | `Command.handle` | Contagem de LIMBO pelos ativos do template, sem recorte de vigência em `data_ref`. |
| C23 | `app/ponto/management/commands/tripwire_celulas.py:57` | `medir_ausentes` | Parcial: verifica início, mas ignora fim/dono; pode duplicar ausências. |
| C24 | `app/folha/services/ranking_he.py:132` | `ranking` | Mapa de ciclos ativos conserva um ciclo para filtrar a competência. |
| C25 | `app/relatorios/views.py:492` | `extrato_parcial` | Mapa ativo alimenta pareamento/turnos abertos de todo período. |
| C26 | `app/relatorios/pendencias.py:74` | `com_direcao` | Mapa ativo alimenta diagnóstico e recomendação sobre 30 dias. |
| C27 | `app/relatorios/furos.py:86` | `escalas_em_limbo` | Ativos sem fim testados hoje/amanhã; futuro pode ser acusado. |
| C28 | `app/relatorios/management/commands/diff_extrato_parcial.py:107` | `Command.handle` | Sonda: motor antigo recebe mapa de ativos para o período. |
| C29 | `app/chamados/detector_anomalias.py:56` | `detectar_anomalias` | Atual e futuro sem fim podem ser acusados como colisão. |
| C30 | `app/chamados/services/disputa_emissao.py:312` | `gerar_perguntas_disputa` | Dropdown do fato oferece ativos sem fim, inclusive futuro. |
| C31 | `app/chamados/reconciliador.py:232` | `_ancora_ausente` | Futuro sem âncora pode manter a premissa do chamado. |
| C32 | `app/chamados/services/desvio_escala.py:106` | `texto_desvio` | Colegas ativos, inclusive futuros, alimentam recomendação sobre histórico. |
| C33 | `app/core/views.py:789` | `painel_tv` | Mapa ativo conserva um vínculo para fim de turno e cobertura. |
| C34 | `app/core/services/painel_op.py:76` | `montar_painel_operacional` | Mapa ativo conserva um vínculo para previsão, fim e cobertura. |

C23 tem proteção temporal incompleta: compara `data_inicio <= d <= teto`, sem considerar `data_fim` nem resolver o vínculo daquele dia. (`app/ponto/management/commands/tripwire_celulas.py:67`)

**Risco indireto — atribuição, recorte, encaminhamento e apresentação de fatos datados pelo cadastro atual.**

| # | Arquivo:linha | Função | O que alimenta |
|---|---|---|---|
| D01 | `app/escala/services/furos_diarios.py:125` | `ranking` | Posto/praça/supervisor atuais agrupam furos do período. |
| D02 | `app/escala/services/furos_diarios.py:170` | `ranking_colabs` | Posto/praça atuais filtram e rotulam histórico. |
| D03 | `app/escala/services/furos_diarios.py:269` | `universo_empresa` | Posto/praça cadastrais atribuídos à janela consultada. |
| D04 | `app/colaboradores/services/drawer.py:32` | `contexto_drawer` | Posto do vínculo mais recente ou cadastro rotula batidas de sete dias. |
| D05 | `app/ponto/services/espelho.py:212` | `resumo_escala` | Cabeçalho: tipo, horários e posto do ativo mais recente. |
| D06 | `app/ponto/worklist.py:49` | `coletar_gaps_dp` | Posto do primeiro ativo agrupa chamados de dias antigos. |
| D07 | `app/ponto/services/fechamento_lista.py:56` | `montar_resumos` | Mapa ativo conserva rótulo de escala sem considerar competência. |
| D08 | `app/ponto/views.py:2044` | `_escala_rotulo_do_colab` | Drawer da ausência exibe rótulo do primeiro ativo. |
| D09 | `app/ponto/services/triagem_batida.py:332` | `ramo_solicitar_anuencia` | Chamado fora de horário recebe rótulo do primeiro ativo. |
| D10 | `app/ponto/services/geofence.py:201` | `posto_do_colab_agora` | Fallback cadastral na geometria da API/aparelho. |
| D11 | `app/ponto/services/geofence.py:400` | `reavaliar_chamados_geofence` | Reavalia chamados pelo posto atual, explicitamente intencional. |
| D12 | `app/ponto/models.py:265` | `AlertaGeofence.ainda_vale` | Caducidade do alerta pelo posto atual, intencionalmente. |
| D13 | `app/ponto/management/commands/reconciliar_alertas_geofence.py:40` | `Command.handle` | Cron marca alertas caducos pela lotação atual. |
| D14 | `app/ponto/signals.py:304` | `_geofence_post_save_posto._reavaliar` | Evento seleciona chamados pelo posto atual para reavaliação. |
| D15 | `app/ponto/utils.py:25` | `bloquear_se_sem_posto` | Cadastro atual decide bloqueio/chamado da batida. |
| D16 | `app/ponto/services/triagem_batida.py:80` | `triagem_bater_ponto` | Posto cadastral decide sinal “sem posto” e contexto do chamado. |
| D17 | `app/ponto/services/espelho.py:218` | `resumo_escala` | Fallback independente ao posto cadastral no cabeçalho. |
| D18 | `app/ponto/views.py:3068` | `cobertura_disponiveis` | Tela de cobertura atribui `posto_origem` cadastral. |
| D19 | `app/ponto/realtime.py:28` | `publicar_batida` | Canal do evento da batida usa posto cadastral atual. |
| D20 | `app/ponto/services/pautas_do_esmeril.py:31` | `alvo_da_lavra` | Pautas/cron encaminham lavra ao posto atual. |
| D21 | `app/ponto/services/dias_em_aberto.py:126` | `passivo_da_competencia` | Relatório da competência exibe posto atual. |
| D22 | `app/ponto/services/dia_do_colab.py:77` | `dia_do_colab` | Tela/IA de dia específico exibe posto atual. |
| D23 | `app/ponto/services/vigia_entrada.py:136` | `do_dia` | Cron/relatório do dia exibe posto atual. |
| D24 | `app/ponto/services/abriu_nao_bateu.py:143` | `com_nomes` | Lista de mudos exibe posto atual. |
| D25 | `app/ponto/services/vinculo_na_lista.py:105` | `item` | Item do diagnóstico exibe posto atual. |
| D26 | `app/ponto/services/vinculo_na_lista.py:152` | `colabs_em_revisao` | Revisão compara posto atual ao posto da lavra. |
| D27 | `app/ponto/services/diagnostico_escala.py:196` | `diagnostico_escala` | Link do plano do mês julgado recebe posto atual. |
| D28 | `app/ponto/services/gestao_he.py:264` | `enriquecer` | Tela HE usa posto/praça atuais nos filtros e rótulos do período. |
| D29 | `app/ponto/services/deslogados.py:206` | `com_nomes` | Lista/cron de deslogados exibe posto atual. |
| D30 | `app/ponto/management/commands/processar_alertas_turno.py:329` | `Command._abrir_chamado_critico` | Chamado de `data_turno` recebe posto atual. |
| D31 | `app/ponto/management/commands/detectar_vinculo_divergente.py:40` | `Command.handle` | Diagnóstico do cron exibe posto atual. |
| D32 | `app/inteligencia/calculadores.py:73` | `preenchimento_espelho` | Métrica de dias do período agregada por posto atual. |
| D33 | `app/relatorios/dia_da_frota.py:56` | `dia_da_frota` | Vínculo datado; fallback de posto retorna cadastro atual. |
| D34 | `app/folha/views_retrato.py:107` | `_cadastro` | Ativos ascendentes sobrescritos: futuro rotula retrato da competência. |
| D35 | `app/holerite/views.py:282` | `_pessoas_da_celula` | Mesmo algoritmo: futuro rotula grade de entrega da competência. |
| D36 | `app/relatorios/views.py:830` | `filtro_search_ajax` | Pessoas do caderno de espelhos selecionadas pelos vínculos ativos. |
| D37 | `app/relatorios/views.py:856` | `filtro_search_ajax` | Associações/contadores do caderno usam pares ativos, incluindo futuros. |
| D38 | `app/folha/services/ranking_he.py:122` | `ranking` | Posto atual filtra e rotula ranking de HE da competência. |
| D39 | `app/folha/services/ranking_he.py:221` | `apuracao_colab` | Apuração histórica exibe posto atual. |
| D40 | `app/relatorios/views.py:78` | `horas_mes` | Praça/posto atuais recortam e rotulam relatório da competência. |
| D41 | `app/relatorios/views.py:150` | `inconsistencias` | Praça/posto atuais recortam e rotulam fechamentos históricos. |
| D42 | `app/relatorios/views.py:202` | `absenteismo` | Praça/posto atuais recortam e rotulam ausências do período. |
| D43 | `app/relatorios/views.py:288` | `atestados_acumulados` | Praça/posto atuais recortam e rotulam atestados do período. |
| D44 | `app/relatorios/views.py:461` | `extrato_parcial` | Praça/posto atuais recortam e rotulam extrato histórico. |
| D45 | `app/relatorios/views.py:623` | `beneficios_consolidado` | Praça/posto atuais recortam e rotulam benefícios da competência. |
| D46 | `app/relatorios/pdf_espelho.py:724` | `_build_story` | Cabeçalho do espelho PDF exibe posto atual. |
| D47 | `app/relatorios/pdf_chamados.py:63` | `_build_story` | Cabeçalho do histórico forense exibe posto atual. |
| D48 | `app/relatorios/furos_pdf.py:35` | `_card_flow` | Cartão PDF de furos do período exibe posto atual. |
| D49 | `app/templates/relatorios/furos.html:41` | Template de furos | Cartão HTML de furos do período exibe posto atual. |
| D50 | `app/chamados/services/adesao.py:14` | `_texto` | Texto do chamado sobre `ini/fim` recebe posto atual. |
| D51 | `app/chamados/services/painel.py:130` | `montar_qs_filtrado` | Praça do posto atual recorta chamados históricos. |
| D52 | `app/chamados/services/painel.py:264` | `montar_contexto_painel` | Praça atual recorta métricas históricas de validação. |
| D53 | `app/folha/views_retrato.py:115` | `_cadastro` | Fallback independente ao posto cadastral para o retrato. |
| D54 | `app/holerite/views.py:310` | `_pessoas_da_celula` | Fallback independente ao posto cadastral na grade. |
| D55 | `app/core/views.py:352` | `dashboard_supervisor` | Posto cadastral seleciona visibilidade/atribuição dos fatos do painel. |
| D56 | `app/core/views.py:831` | `painel_tv` | Posto cadastral agrupa turnos/cobertura da TV. |
| D57 | `app/core/services/painel_op.py:231` | `montar_painel_operacional` | Posto cadastral agrupa linhas/cobertura operacional. |
| D58 | `app/core/views.py:1012` | `batida_detalhe_json` | Drawer da batida exibe posto cadastral atual. |
| D59 | `app/templates/core/_d2_feed.html:11` | `dashboard` / `dashboard_dados` | Feed de batidas exibe posto cadastral atual. |
| D60 | `app/templates/core/_d2_alertas.html:12` | `dashboard` / `dashboard_dados` | Lista de chamados exibe posto cadastral atual. |
| D61 | `app/colaboradores/views.py:927` | `painel_colaborador` | Primeiro ativo rotula painel com batidas do mês. |
| D62 | `app/colaboradores/services/detalhe.py:73` | `contexto_detalhe` | Primeiro ativo rotula detalhe com histórico/mapa de batidas. |
| D63 | `app/colaboradores/services/troca_pelo_chat.py:

[truncado em 20000 de 20928 caracteres]

## 29/10 14:45 · sessao 14cc3d7a87dc

Auditoria somente leitura do escopo fora `ponto`, `escala`, `colaboradores`, `core`; nenhum arquivo criado/editado/apagado. [não verificado — declaração operacional]

`EscalaColaborador.Meta.ordering = ["-data_inicio"]`: `.first()` e `setdefault` no queryset padrão escolhem o vínculo futuro; compreensão de dict que sobrescreve por colaborador termina no vínculo mais antigo; laço ascendente com sobrescrita escolhe o futuro. `app/escala/models.py:1003`.

**Seletores e leituras com efeito operacional/data julgada:**

| Arquivo:linha | Função | Seleção e alimentação; risco atual + futuro |
|---|---|---|
| `app/api/views.py:312` | `api_me` | `ativa=True.first()`; futuro alimenta `_escala_de_hoje` e tela do app em `:380`. |
| `app/api/views_core.py:301` | `api_me` | Cópia física do kernel; futuro alimenta escala de hoje do app em `:367`. |
| `app/api/views.py:918` | `api_escala_v2` | `ativa=True.first()`; um único vínculo futuro alimenta 14 dias, inclusive hoje (`:931–939`). |
| `app/chamados/juizes.py:1251` | `_marco_esperado_da_grade` | Ativo mais recente sem filtro de `perg.data_evento`; futuro fornece marco esperado à defesa/classificação de resposta histórica (`:1247`, `:1256`). |
| `app/chamados/services/desvio_escala.py:44` | `medir_desvio` | Ativo mais recente; futuro fornece marcos dos últimos 14 dias e referência para comparar 84 dias de batidas (`:48–61`). |
| `app/chamados/services/desvio_escala.py:106` | `texto_desvio` | Todos os colegas com vínculo ativo no template, sem início/fim; futuro entra no diagnóstico “outros usam corretamente/deslocados”, alimentando recomendação de realocação/correção (`:109–124`). |
| `app/chamados/services/fila_por_causa.py:164` | `particionar` | Município de `Colaborador.posto` atual para feriados dos dias históricos dos chamados; alimenta gaveta/causa da fila (`:161–169`). |
| `app/ferias/management/commands/alertar_retornos.py:28` | `Command.handle` | Ativos + `setdefault`; futuro é usado para hoje e amanhã; alimenta cron/push de retorno de folga (`:29–35`, `:45`). |
| `app/folha/export.py:787` | `classificar_export` | Ativo 12x36 sem âncora, sem competência/início/fim; vínculo futuro sem âncora pode bloquear aprovação da competência (`:793`, `:808` e sequência). |
| `app/folha/services/ranking_he.py:132` | `ranking` | Dict de ciclos de ativos sem data da competência; conserva o mais antigo na ordenação padrão e ignora a transição; alimenta filtro de ranking histórico (`:135`). |
| `app/inteligencia/calculadores.py:73` | `preenchimento_espelho` | Agrupa métricas de dias do período por `Colaborador.posto_id` atual, sem posto do dia; alimenta métrica por posto (`:77–85`). |
| `app/inteligencia/calculadores.py:319` | `_calc_escalas_desvio` | Ativo mais recente; futuro decide exclusão de intermitente e rótulo da escala comparado à grade dos últimos 30 dias (`:321–330`). |
| `app/relatorios/views.py:492` | `extrato_parcial` | Dict de ativos sem data; vínculo mais antigo fornece template ao pareador de todo período; alimenta `turnos_abertos`, não rubricas (`:509–514`, `:532`). |
| `app/relatorios/pendencias.py:74` | `com_direcao` | Dict de ativos sem data; vínculo mais antigo alimenta diagnóstico dos últimos 30 dias, recomendação de destino e alerta de mudança de regime (`:80–97`). |
| `app/relatorios/furos.py:86` | `escalas_em_limbo` | Todos ativos sem fim, inclusive futuro; testa hoje/amanhã e pode acusar limbo/cadastro do futuro (`:93`, `:108–124`). |
| `app/relatorios/dia_da_frota.py:56` | `dia_da_frota` | Vínculo do dia é correto em `:42`, mas, sem posto nele, recua a `Colaborador.posto` atual para identificar quem acusa naquele dia (`:57–61`). |
| `app/relatorios/management/commands/diff_extrato_parcial.py:107` | `Command.handle` | Dict de ativos alimenta lado antigo da comparação; motor usa esse vínculo sobre todo período (`_antes`, `:49–71`). É comando diagnóstico, não teste/smoke. |
| `app/chamados/detector_anomalias.py:56` | `detectar_anomalias` | Conta ativos sem fim e sem início; atual + futuro ambos abertos viram `colisao_escala` (`:61–65`). |
| `app/chamados/services/disputa_emissao.py:312` | `gerar_perguntas_disputa` | Opções `escala_dropdown`: ativos sem fim, sem data do fato; oferece vínculo futuro na pergunta (`:313–315`). |
| `app/chamados/reconciliador.py:232` | `_ancora_ausente` | `.exists()` de qualquer ativo sem âncora, sem data; futuro sem âncora pode manter premissa de chamado de âncora (`:226–235`). |
| `app/chamados/views.py:442` | `detalhe_chamado_local` | Ativo mais recente do template do chamado; ação `corroborar_regua` pode atingir vínculo futuro em vez do vínculo do fato (`:438–444`). |

**Seleções atuais de tela/cadastro, separar como risco baixo ou fora do recorte estritamente “julga dia”:**

| Arquivo:linha | Função | Alimentação |
|---|---|---|
| `app/folha/views_retrato.py:107` | `_cadastro` | Ativos em ordem ascendente + sobrescrita: futuro vira escala/posto/praça do retrato de competência, tela e PDF (`:109–122`). Fallback independente a `Colaborador.posto` em `:115`. |
| `app/holerite/views.py:282` | `_pessoas_da_celula` | Mesmo algoritmo: futuro vira escala/posto/praça da grade de entrega de holerite da competência (`:283–284`, `:310–312`). Fallback independente a `Colaborador.posto` em `:310`. |
| `app/api/views_mensageria.py:1212` | `api_mensageria_pendencias_cadastro` | Todos ativos 6x1 sem calendário; futuro entra na lista “cadastros que fabricam cobrança falsa” (`:1213–1221`, `:1247–1250`). |
| `app/chamados/services/limbo_folgas.py:64` | `aplicar_sugestao` | Conta vínculos ativos do template, inclusive futuros, para alcance textual da sugestão (`:69`). Não escolhe vínculo para julgar dia. |
| `app/relatorios/views.py:830` | `filtro_search_ajax` | Todos ativos de escala, sem data; futuro entra na seleção de pessoas para caderno de espelhos (`:831–835`). |
| `app/relatorios/views.py:856` | `filtro_search_ajax` | Pares escala/colaborador ativos sem data; futuro infla associação/contador de opções do caderno (`:858–866`). |
| `app/relatorios/views.py:1319` | `_acc_escala` | Primeiro vínculo ativo da relação, sem data; futuro vira coluna Escala do builder (`:1320–1321`). **Metadado de cadastro**, não cálculo de dia. |

A tabela operacional tem **21 sítios**; a de tela/cadastro tem **7 seletores adicionais**, ou **28 sítios**, antes dos acessos diretos de posto listados abaixo. A contagem inclui o posto em `fila_por_causa:164`, em `preenchimento_espelho:73` e o fallback de `dia_da_frota:56`; não inclui os dois fallbacks de folha/holerite como seletores extras.

**Outros acessos a posto atual que contextualizam/recortam dados de período, sem selecionar vínculo:**

- `app/folha/services/ranking_he.py:122`, `:146`, função `ranking`: filtro e rótulo por posto atual no ranking da competência; `:221`, `apuracao_colab`: rótulo atual junto da apuração histórica.
- `app/relatorios/views.py:78`, `:92`, `horas_mes`: recorte de praça e coluna Posto atuais no relatório da competência; `:150`, `:159`, `inconsistencias`: recorte e Posto atuais sobre fechamentos; `:202`, `:212`, `absenteismo`: recorte e Posto atuais sobre ausências do período; `:288`, `:300`, `atestados_acumulados`: idem para atestados.
- `app/relatorios/views.py:461`, `:560`, `extrato_parcial`: recorte de praça/coluna Posto atuais; `:623`, `:654`, `beneficios_consolidado`: recorte e rótulo atuais de benefícios da competência.
- `app/relatorios/pdf_espelho.py:724`, `_build_story`: cabeçalho de espelho usa `Colaborador.posto` atual, enquanto vínculo do cálculo é datado em `:303` e `:567`.
- `app/relatorios/pdf_chamados.py:63`, `_build_story`: cabeçalho do histórico forense usa posto atual.
- `app/relatorios/furos_pdf.py:35`, `_card_flow`, e `app/templates/relatorios/furos.html:41`: posto atual no cartão de furos do período.
- `app/chamados/services/adesao.py:14`, `_texto`: posto atual no texto de chamado sobre o período `ini/fim` (`:16–18`).
- `app/chamados/services/painel.py:130`, `montar_qs_filtrado`: chamados históricos recortados por praça do posto atual; `:264`, `montar_contexto_painel`: métricas históricas de validação recortadas por essa praça.

**Posto atual puramente cadastral/operacional, sem dia julgado; candidatos para exclusão da contagem principal:**

- `app/api/views.py:191`, `_validar_assinatura_keystore`; `app/api/views_core.py:115`, mesmo nome: comparação posto do aparelho/cadastro, sinal de contexto.
- `app/api/views.py:356`, `api_me`; `app/api/views_core.py:345`, `api_me`: rótulo do posto cadastral no app.
- `app/api/views_mensageria.py:805` e `:831`, `api_mensageria_situacao_ferias`: candidatos/cabeçalho; `:972`, `_onde_trabalha`: rótulo; `:1053`, `api_mensageria_pessoa`: cabeçalho; `:1277`, `_cadastro_incompleto`: lista sem posto.
- `app/chamados/views.py:695`, `fio_colaborador`: posto atual para mapa/cabeçalho e `app/templates/chamados/fio_colaborador.html:107`.
- `app/chamados/detector_anomalias.py:87`, `:99`, `:107`, `detectar_anomalias`: comparação de ordem vigente/posto e cadastro posto desativado/ausente.
- `app/chamados/services/abertura.py:62`, `abrir_chamado_local`; `:187`, `abrir_chamado_para_colaborador`: posto atual no contexto de abertura.
- `app/comunicados/services.py:23`, `para_colaborador`; `:63`, `audiencia_de`: público/gate de comunicado por posto atual.
- `app/ferias/views.py:65`, `:85`, `lista_ferias`: filtros por posto atual; `:597`, `agendar_lote`: filtro cadastral de candidatos.
- `app/relatorios/views.py:356`, `:369`, `ferias_vencendo`; `:409`, `:420`, `afastados`; `app/relatorios/services.py:313`, `gerar_pdf_ferias_gestao_bytes.<locals>.build`: posto atual em lista/relatório cadastral de férias/afastados.
- `app/inteligencia/views.py:87`, `index`: disponíveis sem posto; `:221`, `:227`, `:293`, `api_kpi`: posto atual/cadastro sem posto.
- `app/relatorios/views.py:828`, `:847`, `filtro_search_ajax`: associação/contador por `Colaborador.posto_id` para caderno. **Explicitamente cadastro**: `:1334`, `_COLS_COLAB`; `:1361`, `_grupos_por_posto`; `:1444`, `:1454`, `relatorio_builder`. O contrato aparece em `:1348` e `:1355–1356`.

Templates adicionais que apenas exibem esses metadados e não selecionam vínculo: `app/templates/relatorios/ferias_vencendo.html:45`, `beneficios_consolidado.html:66`, `extrato_parcial.html:69`, `inconsistencias.html:42`, `historico_chamados.html:105`, `afastados.html:39`, `horas_mes.html:66`; `app/templates/ferias/_tabela_ferias.html:26` e `:38`, `saldo.html:8`.

**Exclusões relevantes:**

- `app/chamados/detector_anomalias.py:144`, `detectar_batida_fora_escala`: usa `escala_vigente` na data da batida.
- `app/folha/export.py:348`, `_ciclo_do_colab`: usa vínculo no fim da janela da competência.
- `app/relatorios/pdf_espelho.py:303` e `:567`: vínculo da janela/dia pelas portas corretas.
- `app/relatorios/dia_da_frota.py:42`: seleção de vínculo correta; só o fallback de posto em `:56` é candidato.
- `app/chamados/services/limbo_folgas.py:189`, `filhos_no_defeito`: carrega todos para resolver por dia, sem escolha por estado atual.
- `app/inteligencia/resumo.py:49`, `resumo_colaborador`: histórico completo, não `.first()` ativo.
- `app/chamados/models.py:2336`, `resposta_display`: busca pelo PK explicitamente respondido, não pelo estado atual.
- `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:116`: algoritmo antigo de comparação já filtra a vigência do dia; não atende ao padrão pedido.
- `smoke_chamados.py`: teste/smoke excluído. [nao verificado]

## 29/10 14:45 · sessao ffa1088b5e99

Auditoria estática de `app/ponto`: **52 sítios fortes + 1 limítrofe**, contando a seleção do vínculo e a leitura independente de `Colaborador.posto` separadamente; usos repetidos da mesma fonte na função foram agrupados. A ordenação padrão é `-data_inicio`, portanto `.first()` sem ordem explícita favorece o vínculo futuro ativo; dicionários sobrescritos nessa ordem acabam com o vínculo mais antigo. `app/escala/models.py:1003`.

**Seleções de EscalaColaborador — 25 fortes**

| Arquivo:linha | Função | Alimenta / risco com atual + futuro |
|---|---|---|
| `app/ponto/services/espelho.py:42` | `piso_visual` | Espelho e calendário: maior início ativo vira piso, podendo cortar histórico até hoje (`:44-47`). |
| `app/ponto/services/espelho.py:212` | `resumo_escala` | Cabeçalho espelho: tipo, horário, dias e posto do futuro ativo (`:218-247`). |
| `app/ponto/turnos.py:2428` | `proximo_tipo_de` | Motor de sugestão E/S: ramo intermitente escolhido pelo futuro ativo (`:2430-2437`). |
| `app/ponto/turnos.py:2447` | `proximo_tipo_de` | Motor de sugestão E/S: marcos de intervalo/saída do futuro ativo (`:2449-2462`). |
| `app/ponto/selecao_periodo.py:278` | `turnos_em_turno_stored` | Tela/presença: jornada do vínculo ativo mais antigo, pois `order_by("data_inicio")` + `setdefault`; futuro do período não é resolvido por data (`:279-291`). |
| `app/ponto/worklist.py:49` | `coletar_gaps_dp` | Worklist agrupada por posto: posto do futuro ativo para chamados com dias antigos (`:51-70`). |
| `app/ponto/services/faltas_de_hoje.py:35` | `faltas_de_hoje_sem_cobranca` | Cron/contador: julga limbo, dia de trabalho e marco de hoje pelo futuro ativo (`:39-48`). |
| `app/ponto/services/abriu_nao_bateu.py:194` | `_hora_de_bater_passou` | Cron/contador de mudos: marco de entrada do futuro ativo julga `dia` (`:198-204`). |
| `app/ponto/services/piso_intervalo.py:28` | `pares_abaixo_do_piso` | Contador de período: cada vínculo ativo entra, podendo duplicar colaborador e aplicar marcos de borda atuais/futuros (`:35-59`). |
| `app/ponto/services/bordas_realizado.py:62` | `bordas_do_realizado` | Contadores/cron: cada ativo reaplica `livre` e `intervalo_do_dia(d)` a toda janela; duplicação e vínculo errado (`:67-79`). |
| `app/ponto/services/fechamento_lista.py:56` | `montar_resumos` | Tela fechamento: rótulo ativo sobrescrito por colaborador; pela ordenação padrão termina no mais antigo, independentemente da competência (`:61`, `:116`). |
| `app/ponto/views.py:2044` | `_escala_rotulo_do_colab` | Drawer de ausência: rótulo do futuro ativo (`:2046`). |
| `app/ponto/services/triagem_batida.py:92` | `triagem_bater_ponto` | Tela/batida: existência de qualquer ativo basta para sinal de “sem escala”; futuro satisfaz o gate (`:93-97`). |
| `app/ponto/services/triagem_batida.py:332` | `ramo_solicitar_anuencia` | Chamado/tela: rótulo fora de horário baseado no tipo do futuro ativo (`:335-343`). |
| `app/ponto/services/cluster_espurio.py:75` | `reconciliar_colab` | Evento/cron: gate exige ativo sem fim hoje para reconciliar dias da janela; futuro satisfaz gate (`:77-85`). |
| `app/ponto/management/commands/detectar_par_relampago.py:335` | `Command.handle` | Cron de pares passados: universo de ativos sem fim; dois vínculos repetem processamento do colaborador (`:349-355`). |
| `app/ponto/management/commands/detectar_cluster_espurio.py:36` | `Command.handle` | Cron: mesmo universo ativo sem fim, repetindo reconciliação/diagnóstico por vínculo (`:43-52`). |
| `app/ponto/management/commands/tripwire_celulas.py:17` | `medir_stale` | Cron/tripwire: um vínculo por colaborador em dict, último = mais antigo; compara todas as células futuras com ele, sem vigência (`:19-29`). |
| `app/ponto/management/commands/reconciliar_fantasmas.py:172` | `Command.handle` | **Diagnóstico**, não decisão final: tipo/intervalo do futuro ativo alimentam `_completude` dos dias antigos e texto de detalhe (`:203-208`). |
| `app/ponto/management/commands/propor_reorganizacao.py:42` | `Command._run` | Cron/propositor: exclui colaborador da janela histórica se qualquer ativo for intermitente; futuro intermitente exclui passado (`:46-59`). |
| `app/ponto/management/commands/fechar_chamados_meio_periodo.py:59` | `Command.handle` | Comando de fechamento: dict ativo resolve um template para chamados de até 90 dias; último = mais antigo, sem dia julgado (`:68-80`). |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:110` | `Command.handle` | Cron: existência do futuro ativo decide entrada na reconciliação de chamados antigos; cálculo de furos em si vem da célula (`:113-129`). |
| `app/ponto/management/commands/censo_fase_12x36.py:295` | `Command._curar` | Comando de cura: tipo do futuro ativo entra no payload de novo vínculo na data histórica `x['virada']` (`:311-316`). |
| `app/ponto/management/commands/diff_espelho_alimentacao.py:94` | `Command.handle` | **Sonda/diff**, não produção: ramo A do motor recebe um ativo atual para toda competência (`:103-110`). |
| `app/ponto/management/commands/detectar_ausencias.py:184` | `Command.handle` | Cron/chamado: universo ativo por template conta limbo em `data_ref`; sem escolha por data, dois vínculos podem contar duas vezes (`:183-188`). |

**Colaborador.posto — 27 sítios fortes**

| Arquivo:linha | Função | Alimenta / natureza |
|---|---|---|
| `app/ponto/models.py:100` | `Batida.posto_para_calculo` | **Fonte central**: fallback do posto da batida para cadastro atual; geofence usa em `app/ponto/services/geofence.py:236`, ping usa em `app/ponto/views.py:2926`. |
| `app/ponto/management/commands/detectar_ausencias.py:623` | `Command._abrir_chamado` | **Alto**: posto atual decide veto de feriado do `data_ref`; também descrição do chamado em `:677`, `:683`. |
| `app/ponto/services/geofence.py:48` | `vinculo_contagem` | Diagnóstico de vínculo: últimas batidas GPS contra posto atual (`:51-62`). |
| `app/ponto/services/geofence.py:201` | `posto_do_colab_agora` | API/geometria do aparelho: fallback cadastro; também usa a property central pela entrada em `:200`, consumidor `:219`. |
| `app/ponto/services/geofence.py:400` | `reavaliar_chamados_geofence` | Reconciliação de chamados antigos contra posto **atual por intenção explícita** (`:386-412`). |
| `app/ponto/models.py:265` | `AlertaGeofence.ainda_vale` | Alerta/tela: caducidade contra posto **atual por intenção explícita** (`:258-270`). |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py:40` | `Command.handle` | Cron: carimbo de alerta caduco usa `colaborador__posto_id` atual (`:41-52`). |
| `app/ponto/signals.py:304` | `_geofence_post_save_posto._reavaliar` | Evento: seleciona chamados pelo posto atual antes da reavaliação (`:303-306`). |
| `app/ponto/utils.py:25` | `bloquear_se_sem_posto` | Batida/chamado: cadastro atual decide “sem posto” (`:26-42`). |
| `app/ponto/services/triagem_batida.py:80` | `triagem_bater_ponto` | Tela/batida: sinal “sem posto” e contexto do chamado; repetido em `:120`. |
| `app/ponto/services/espelho.py:218` | `resumo_escala` | Cabeçalho espelho: fallback independente para posto atual quando vínculo escolhido não tem posto (`:246`). |
| `app/ponto/views.py:3068` | `cobertura_disponiveis` | Tela cobertura: `posto_origem` atual. |
| `app/ponto/realtime.py:28` | `publicar_batida` | Evento: canal de notificação é posto atual, não posto do dia da batida (`:38-43`). |
| `app/ponto/services/pautas_do_esmeril.py:31` | `alvo_da_lavra` | Pautas/cron: encaminha assinaturas da lavra para posto atual (`:34-41`, `:61`). |
| `app/ponto/services/dias_em_aberto.py:126` | `passivo_da_competencia` | Tela/relatório de competência: rótulo do posto atual. |
| `app/ponto/services/dia_do_colab.py:77` | `dia_do_colab` | Tela/IA de um dia específico: metadado posto atual. |
| `app/ponto/services/vigia_entrada.py:136` | `do_dia` | Cron/relatório do dia: metadado posto atual. |
| `app/ponto/services/abriu_nao_bateu.py:143` | `com_nomes` | Cron/lista: metadado posto atual. |
| `app/ponto/services/vinculo_na_lista.py:65` | `achar` | Cadastro x realidade: posto/colegas atuais julgam amostra histórica GPS; usos em `:75-87`. |
| `app/ponto/services/vinculo_na_lista.py:105` | `item` | Tela Cadastro x realidade: posto atual no item. |
| `app/ponto/services/vinculo_na_lista.py:152` | `colabs_em_revisao` | Tela/geofence: compara posto atual com posto da lavra; intenção explícita de revisão atual em `:139-144`. |
| `app/ponto/services/diagnostico_escala.py:196` | `diagnostico_escala` | Tela: URL do plano de folgas de `fim.month/year` recebe posto atual. |
| `app/ponto/services/gestao_he.py:264` | `enriquecer` | Tela HE de período: posto e praça atuais alimentam rótulos/filtros (`:265-291`). |
| `app/ponto/services/deslogados.py:206` | `com_nomes` | Cron/lista: metadado posto atual. |
| `app/ponto/management/commands/processar_alertas_turno.py:329` | `Command._abrir_chamado_critico` | Cron/chamado do `data_turno`: metadado posto atual (`:335`). |
| `app/ponto/management/commands/checklist_golive.py:47` | `Command.handle` | Checklist **cadastral atual**: posto nulo/“definir”; repetido nas consultas `:48`, `:59`. |
| `app/ponto/management/commands/detectar_vinculo_divergente.py:40` | `Command.handle` | Saída do cron: nome do posto atual no diagnóstico. |

**Limítrofe — 1**

`app/ponto/management/commands/tripwire_celulas.py:57`, `medir_ausentes`: universo `ativa=True`, mas cada dia recebe `ec.data_inicio <= d <= teto` em `:67`; não pergunta pelo dono do dia nem respeita `data_fim`. Com dois ativos pode duplicar ausências futuras. Há comparação temporal parcial, por isso separei dos 52 fortes. `app/ponto/management/commands/tripwire_celulas.py:57-68`.

**Exclusões verificadas**

- `gerar_celulas_periodo` filtra `ativa=True`, mas julga cada `d` por `vinculo_cobre_a_data(ec,d)`; é equivalente temporal e não entra. `app/ponto/portas/celula.py:568`, `app/ponto/portas/celula.py:588`.
- `medir_pausa_faltando` usa `vinculo_do_dia(cel.colaborador_id,cel.data,...)`; não entra. `app/ponto/management/commands/tripwire_celulas.py:107`.
- A seleção principal do motor/espelho já usa o vínculo do último dia, com alimentação do período; não confundir com os dois sítios atuais remanescentes no começo do arquivo. `app/ponto/services/espelho.py:1069`.
- `gerar_batidas_smoke_motor` é gerador de dados smoke e sua seleção também filtra interseção do período; deixei fora do conjunto forte. `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73-79`.
- `core.feriados.lugar_do_colab`, já coberto pelo root, é outra fonte central de posto atual; não somei seus chamadores em ponto. `app/ponto/services/fechamento.py:622`.

**Contagem de app/ponto: 52 fortes; 53 incluindo o limítrofe.**

## 29/10 14:45 · sessao fb6d6264daf3

Operacionais: **21 seletores de vínculo**. As linhas abaixo identificam a seleção e o consumidor; futuras escalas podem responder por dias anteriores quando não houver célula, e guardas podem vetar todo período mesmo quando o leitor posterior é datado.

| Arquivo:linha | Função | Alimenta/risco |
|---|---|---|
| `app/escala/utils.py:920` | `data_saida_esperada` | Ativa+fim nulo+latest; marcos da entrada em 927 → data de saída esperada. |
| `app/colaboradores/services/calendario.py:341` | `contexto_calendario` | First ativa; calendário mensal usa `ec.eh_dia_trabalho` em 750; guarda em 747 rejeita dia sem célula que outro vínculo cobriria. |
| `app/colaboradores/views.py:268` | `painel_vinculo` | First ativa; agenda de 14 dias usa essa escala em 289/292. |
| `app/colaboradores/views.py:2202` | `agenda_modal_view` | First ativa; semana navegável usa escala em 2223/2233. |
| `app/colaboradores/views.py:2860` | `disponibilidade` | Todos ativos com `setdefault`; escolhe um vínculo por colaborador para procurar retornos de folga. |
| `app/colaboradores/views_fase.py:45` | `fase_12x36` | Latest ativa → template/âncora em 47/49, calendários em 98, comparação de fase em 101. |
| `app/colaboradores/services/drawer.py:22` | `contexto_drawer` | Latest ativa; rótulo e gate de previsão em 98–104 podem ocultar o calendário chamado em 121. |
| `app/escala/views.py:17` | `minha_escala` | First em 25; calendário de 14 dias em 40/43. |
| `app/escala/views_plano_folgas.py:55` | `_grid_ctx` | Todos ativos, uma linha por vínculo no mês; previsão em 76 sem escolher vínculo por dia. |
| `app/escala/services/furos_diarios.py:29` | `apurar_colab` | Latest ativa; guarda 30/31 veta período inteiro antes de `grade_da_celula` em 36; `apurar` grava/apaga os furos em 97–99. |
| `app/escala/services/furos_diarios.py:148` | `ranking` | Eixo template agrega histórico pelo join `escalas__ativa=True`; dois ativos fazem dias entrarem nos dois templates (110/149). |
| `app/escala/services/furos_diarios.py:291` | `universo_empresa` | Latest ativa → classe LIMBO/suspeita em 295–299 e julgamento de todos os dias em 306. |
| `app/escala/services/impacto_troca.py:34` | `impacto_da_troca` | Latest ativa → âncora em 37 e furos anteriores em 39; aviso/veredito de troca retroativa. |
| `app/escala/services/escala_auto.py:172` | `dry_da_frota` | Ativa+fim nulo, latest por colaborador; deduplica em 178 e gera propostas em 182. |
| `app/escala/services/fase_12x36.py:106` | `fora_de_fase` | Ativas viram dict por colaborador em 114; última iterada vence; aplica uma escala à janela passada em 181. |
| `app/escala/services/meio_periodo.py:152` | `observado_meio_periodo` | Ativas viram dict por colaborador; usa template atual para marcos de turnos passados em 174. |
| `app/escala/services/propositor.py:414` | `propostas` | Itera todas ativas; vínculo futuro também é candidato a corrigir padrão da janela anterior; proposta em 486. |
| `app/escala/services/cadastro_realidade.py:190` | `_jornada_que_mente` | Dict template por colaborador em 193; template atual alimenta `periodos_do_dia(cel, te)` em 200 e diagnóstico C2. |
| `app/escala/management/commands/celulas_contra_o_template.py:28` | `contra_o_template` | Dict de ativas por colaborador; compara template selecionado à data de cada célula do acervo em 36. |
| `app/escala/services/fase_conflitante.py:71` | `medir` | Diagnóstico/cron itera todas ativas e compara foto/âncora em toda janela em 79/80, sem limitar à vigência. |
| `app/escala/management/commands/termometro_regua.py:35` | `Command.handle` | Diagnóstico/cron de todas ativas, inclusive futura, via avaliação em 43/47; avisos em 68/75. |

**Oito leituras independentes de posto atual sobre fatos/períodos:**

| Arquivo:linha | Função | Alimenta/risco |
|---|---|---|
| `app/escala/services/furos_diarios.py:108` | `ranking` | Agrega período pelo posto/praça atual; supervisor pelo posto atual em 135. |
| `app/escala/services/furos_diarios.py:170` | `ranking_colabs` | Filtra/rotula histórico com posto/praça atual em 170–173. |
| `app/escala/services/furos_diarios.py:269` | `universo_empresa` | Posto/praça do cadastro substituem atribuição histórica em 269/270 e 315–317. |
| `app/colaboradores/services/feriado_do_posto.py:99` | `presenca_em_feriado` | Posto atual determina município, classificação de feriado e agrupamento das células dos últimos 120 dias; classificação em 110. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:95` | `Command.handle` | Seleciona chamados pelo posto atual e julga a data do fato em 102. |
| `app/colaboradores/services/drawer.py:32` | `contexto_drawer` | Posto do vínculo latest ou `colab.posto`; reaplicado às batidas dos últimos sete dias em 59. |
| `app/colaboradores/services/situacional.py:179` | `montar_situacional` | Compara geolocalização da batida recente ao posto atual; tela situacional. |
| `app/colaboradores/painel_cell.py:40` | `montar_linha_colab` | Mesma comparação geográfica no refresh da célula. |

**Nove seletores adicionais de EC para tela/metadados/edição atuais, risco inferior:**

| Arquivo:linha | Função | Alimenta/risco |
|---|---|---|
| `app/colaboradores/views.py:48` | `vincular_colaboradores` | Prefetch ativas; `[0]` em 71 define escala exibida e pendências cadastrais. |
| `app/colaboradores/views.py:226` | `vincular_folga` | First ativa; escolhe vínculo editado e materializado, chamada de troca em 239. |
| `app/colaboradores/views.py:336` e `:355` | `vincular_posto` | Mesmo seletor first ativa nos ramos erro/sucesso; conta **uma leitura por função**; renderiza linha de vínculo. |
| `app/colaboradores/views.py:394` | `vincular_escala._render_row` | First ativa → linha pós-vinculação. |
| `app/colaboradores/views.py:927` | `painel_colaborador` | First ativa → escala do painel lateral. |
| `app/colaboradores/services/detalhe.py:73` | `contexto_detalhe` | First ativa → escala da ficha de detalhe. |
| `app/colaboradores/services/troca_pelo_chat.py:54` | `checar_troca` | Latest ativa → rótulo “de” da troca em 56/61. |
| `app/colaboradores/services/vinculo.py:180` | `executar_vinculo` | Latest ativa → “antes” na trilha/histórico em 181/182. |
| `app/colaboradores/services/vinculo.py:388` | `_executar_aceite` | Latest ativa → `ec_nova` do contexto do aceite em 404; pode nomear futura diferente da troca anunciada. |

Não incluí consultas por PK explícito, contadores de uso de template, manutenção que percorre vínculos sem eleger vínculo do dia, nem apresentações puramente cadastrais de `Colaborador.posto`. Exemplos: PK em `app/escala/views_plano_folgas.py:109`/`:176`; contagem em `app/escala/views.py:81`; propagação com guarda temporal em `app/escala/services/cadastro_tipo.py:334`/`:337`/`:338`; calendário de folgas consulta carregador temporal em `app/escala/folgas.py:78`. `ficha_do_colab` escolhe vínculo corretamente por data em `app/colaboradores/services/ficha.py:128`; `salario_em` é outra entidade e filtra data em `app/colaboradores/models.py:473`.

Dois vínculos ativos fazem parte da semântica explicitamente documentada no modelo: leitores devem escolher pela data em `app/escala/models.py:1025`–`:1028`.

**Total deste recorte: 38 leituras — 21 seletores operacionais + 8 postos sobre fatos/períodos + 9 seletores de tela/metadados/edição.**

## 29/10 14:44 · sessao 1ee182181f58

HEAD auditado: `8ea0a691d3177b7d69e5f591250d0c4d0c8d1f68`.

O código usa **mais de um critério**: vigência em D, vínculo gerador da célula, vínculo ativo mais recente, vínculo recebido por parâmetro e FK atual de posto. A ordenação padrão de `EscalaColaborador` é `-data_inicio`: portanto, `.filter(ativa=True).first()` escolhe o ativo de início mais recente, mesmo futuro (`app/escala/models.py:1003`).

Censo estático consolidado abaixo. Testes e migrations: não li. Templates: li os campos da porta de vínculo/fase citados ao final; demais templates, não li. Completude de acessos dinâmicos ou indiretos fora dos padrões pesquisados: **[nao verificado]**. Não executei aplicação, testes ou banco. [nao verificado]

**1. Juiz de vínculo, fase e horários**

| Arquivo:linha — função | Critério de escolha; origem de D |
|---|---|
| `app/escala/alimentacao.py:196` — `vinculo_do_dia` | Se a célula nomeia geradora, usa essa FK **somente dentro da vigência**; ausente/fora da vigência retorna `None`. Sem geradora, candidatos cobrindo D, desempate por `(ativa,data_inicio)` (`:237–254`). D é argumento. |
| `app/escala/alimentacao.py:73` — `escalas_do_periodo_por_colab`; `:131` — singular | Carregadores: início≤fim da janela, fim≥início ou nulo, ativo ou encerrado com fim declarado; ordenam por ativa/início decrescentes (`:116–121`). Não escolhem sozinhos o dono de D. |
| `app/escala/alimentacao.py:173` — `vinculo_cobre_a_data`; `:187` — predicado SQL | Início≤D, fim nulo ou ≥D, com vigência declarada; D recebido. |
| `app/escala/alimentacao.py:257` — `vinculo_do_dia_carregado`; `:288` — plural | Carregam vínculos/células e delegam ao juiz; D recebido. |
| `app/escala/servico_jornada.py:15` — `escala_vigente` | Carrega vínculo/célula e delega ao juiz (`:47–58`); D recebido. |
| `app/escala/models.py:1048` — `eh_dia_trabalho` | EC recebido; lê `CelulaDia(colaborador,D).trabalha`, senão cálculo do próprio EC (`:1066–1073`). |
| `app/escala/models.py:1075` — `marcos_do_dia` | EC recebido; lê `dna.marcos` da célula por colaborador+D, **sem conferir geradora**; sem marcos, template do EC (`:1094–1108`). |
| `app/escala/models.py:1110` — `intervalo_do_dia`; `:1141` — `pausas_do_dia` | EC recebido; primeira pausa/pausas do DNA por colaborador+D; só sem célula usa template (`:1138–1139`, `:1157–1181`). |
| `app/escala/models.py:1183` — `extra_declarada_do_dia` | EC recebido; extra do DNA ou zero; só sem célula usa template (`:1200–1210`). |
| `app/escala/models.py:1213` — `defeito_folga_nao_definida` | Tipo do EC recebido, configuração 6x1 e foto de `FolgaDia` no mês de D (`:1217–1221`). |
| `app/escala/models.py:1223` — `eh_dia_trabalho_calculado` | Fase do próprio EC: âncora, módulo 2/3 para 12x36/24x48, salvo foto mensal (`:1242–1267`). Foto pode usar paridade da folga mais próxima; sem foto, ciclo do template (`:1280–1342`). Feriado usa **posto atual do colaborador**, via `lugar_do_colab` (`:1357–1363`). |
| `app/escala/models.py:326` — `TipoEscala.marcos_do_dia`; `:816` — `eh_dia_trabalho` | Template recebido; horários/ciclo conforme D/weekday. Não escolhem vínculo. |
| `app/escala/servico_jornada.py:61` — `marcos_turno`; `:92` — `fim_turno` | EC recebido fornece marcos de D; sem EC, template recebido (`:61–95`). |
| `app/ponto/selecao_periodo.py:43` — `janela_turno_de` | EC recebido: `ec.marcos_do_dia(data_ref)`; senão template (`:138–160`). |
| `app/ponto/selecao_periodo.py:185` — `previsto_agora_por_hora` | Template recebido; weekday e hora recebidos (`:223–234`). |

**2. Grade, casamento, turnos e cálculo**

| Arquivo:linha — função | Critério; origem de D/instante |
|---|---|
| `app/escala/utils.py:953` — grade legacy; `:1221` — grade por turno | Todos os vínculos+células → `vinculo_do_dia` por D=`ref−i`; ref recebido ou hoje (`:964–971`, `:1240–1255`). |
| `app/escala/utils.py:2021` — `montar_grade_prevista_periodo` | Flag escolhe um dos builders anteriores; ambos usam juiz por D (`:2021–2025`). |
| `app/escala/utils.py:634` — `montar_grade_prevista` | EC recebido; trabalho por método do EC; horários do mapa recebido ou template. D=`ref−i` (`:648`, `:725–741`). |
| `app/escala/utils.py:1538` — montagem dos marcos da grade por turno | Vínculo escolhido para D; horários do DNA ou template; pausas do DNA ou template; encadeamento em D (`:1538–1559`). |
| `app/escala/utils.py:551` — `_match_marcos` | **Não escolhe vínculo**: recebe marcos absolutos, batidas e eventualmente turno; usa papel do turno/alternância e alinhamento (`:562–599`). |
| `app/escala/utils.py:353` — `marco_ocupado_pela_batida` | Recebe dia e horários; encadeia e chama matcher (`:393`). Outros chamadores locais: grade recebida (`:741`), probe (`:1590`) e grade principal (`:1641–1643`). |
| `app/escala/utils.py:257` — `_marcos_def_publico` | União de horários/pausas/overrides do template recebido; não resolve vigência (`:257–296`). A grade limita a ECs que venceram algum D (`:1297–1315`). |
| `app/escala/utils.py:27` — `pausas_dna_periodo`; `:84` — `marcos_dna_periodo` | Células por colaborador e janela; devolvem mapa por data, sem escolher EC (`:43–61`, `:93–107`). |
| `app/escala/utils.py:173` — `_eh_abertura` | EC recebido; D civil de `dt_local`; campos crus do template e depois marcos do EC/template (`:174–204`). Chamador: **[nao verificado]**. |
| `app/escala/utils.py:912` — `data_saida_esperada` | **Ativa + fim nulo + maior início**, sem início≤D; D civil local de `ts_entrada` (`:920–927`). |
| `app/escala/servico_jornada.py:199` — outra `data_saida_esperada` | `escala_vigente(D)`; D civil local de `ts_entrada`; calcula fim com **template, sem EC** (`:199–207`). |
| `app/escala/servico_jornada.py:220` — `_turno_noturno_completo`; `:278` — `classificar_falta`; `:503` — `procedencia_fina` | `escala_vigente(data_evento)`; data recebida. Primeiro aceita EC já fornecido (`:233`, `:353`, `:534`). |
| `app/escala/utils.py:2077` — `_esc_vigente_do_dia` | Juiz com vínculos/célula/geradora carregados; D recebido (`:2115–2129`). |
| `app/escala/utils.py:2132` — `intervalo_declarado_do_dia`; `:2165` — `minutos_previstos_do_dia` | Helper anterior, depois métodos do EC no mesmo D (`:2153`, `:2204`). |
| `app/ponto/turnos.py:772` — `realizado_do_dia` | `escala_vigente(data)`; D recebido (`:786`). |
| `app/ponto/turnos.py:1848` — `turnos_do_colab` | Juiz por cada D da janela ini/fim; agrupa dias contíguos com mesmo dono (`:1928–1939`). |
| `app/ponto/turnos.py:2073` — `dia_encerrado_motivo`; `:2108` — `dias_encerrados` | Juiz em D recebido/iterado na janela (`:2080`, `:2124–2131`). |
| `app/ponto/turnos.py:2259` — `turnos_abertos_de` | Juiz para hoje derivado de `agora`; EC fornecido ao cálculo puro (`:2323–2327`). |
| `app/ponto/turnos.py:1536` — `_escalas_da_ata` | **Todos** os EC, ordenados por colaborador/ativa/início; última linha vence. Nenhum D participa (`:1556–1559`). |
| `app/ponto/turnos.py:1563` — `papel_por_minuto_da_ata` | Usa mapa anterior: uma escala para a ata inteira ini/fim (`:1590–1592`). |
| `app/ponto/turnos.py:2414` — `proximo_tipo_de` | Ativa `.first()` → início mais recente; horários crus de fim de intervalo/turno comparados com agora (`:2447–2455`; ordenação em `app/escala/models.py:1003`). |
| `app/ponto/selecao_periodo.py:256` — `turnos_em_turno_stored` | Ativas ordenadas por início **crescente**, `setdefault`: vence a **mais antiga**. D não seleciona EC; duração limita entrada.timestamp+jornada+margem (`:278–290`). |
| `app/ponto/nucleo.py:7` — `escalas_no_periodo` / `te_vigente_em` | Carrega janela, ordena `(ativa,início)` e guarda último que cobre D; não consulta geradora (`:7–27`). |
| `app/ponto/nucleo.py:30` — `dt_fim_previsto_de`; `:39` — `recompute_turnos` | TE anterior, D=`t.data_turno`; janela calculada sem EC/DNA (`:30–57`). |
| `app/ponto/calculador/alimentacao.py:151` — `marcos_por_dia` | Juiz por D de `resultado.dia_da_jornada`; marcos do EC (`:179–189`). |
| `app/ponto/calculador/alimentacao.py:341` — `rubricas_por_dia` | Vínculos da janela+células → juiz por D (`:408–420`, `:479`). |
| `app/ponto/motor_calculo_v2.py:812` — `_turnos_do_juiz` | TE recebida ou `self.escala`; DNA carregado em ini−2/fim+1 (`:812–827`). |
| `app/ponto/motor_calculo_v2.py:2203` — `calcular_mes` | EC/TE recebidos; fase vem de `EC.data_ancora_colaborador`; métodos EC/TE fornecem horários (`:2209`, `:2228–2255`). |
| `app/ponto/motor_calculo_v2.py:1986` — cálculo por período; `:2386`, `:2624`, `:2949` — folga/múltiplos períodos/Zona 5 | EC/TE recebidos; métodos por dia da jornada, com fallback em campos crus (`:1986–2002`, `:2624–2639`, `:2949–2964`). |
| `app/escala/regua_defesa.py:16`, `:118`, `:503` — degeneração/diagnóstico/entradas | TE/EC recebidos; D ou ref recebidos (`:26`, `:139`, `:524`, `:539`). |
| `app/escala/services/previsto_cego.py:78`, `:114` | Vínculos carregados → helper canônico/minutos previstos por D da janela (`:90`, `:162–174`). |

**3. Cartório, chamados, reconciliação e registro**

Na tabela, **juiz(D)** significa `escala_vigente`, `vinculo_do_dia` ou seu carregador; a regra está em `app/escala/alimentacao.py:237–254`.

| Arquivo:linha — função | Critério; origem de D |
|---|---|
| `app/ponto/services/cartorio.py:675` — `marcos_vencidos` | Juiz(D), célula explicitamente alimentada; D recebido (`:679`). |
| `app/ponto/services/cartorio.py:839` — `_cadastro` do julgamento | Todos os EC+células da janela → cadastro de cada `data_` (`:845`). |
| `app/ponto/services/cartorio.py:1347` — `_julgar_colab_corpo` | Juiz para a data em julgamento (`:1349`). |
| `app/ponto/services/cartorio.py:1446` — `_marcos_do_furo_parcial` | Juiz(D), D recebido (`:1450`). |
| `app/ponto/services/cartorio.py:1554` — `julgar_celula` | Juiz no dia da célula; fim do turno usado para hoje (`:1556`). |
| `app/ponto/supra_juiz.py:136` — cadastro carregado; `:147` — `cadastro_do_dia_carregado` | Juiz em D do classificador/argumento. |
| `app/ponto/precedencia.py:259` — `fatos_do_periodo` | ECs da janela+geradoras → juiz por D; calendário de folga do EC escolhido (`:291`, `:297–299`). |
| `app/ponto/precedencia.py:469` — `veto_de_lancamento` | Juiz no D examinado (`:470`). |
| `app/chamados/detector_anomalias.py:137` — `detectar_batida_fora_escala` | Seleciona EC pelo **dia civil da batida** (`:144`); depois deriva dia do turno e conserva o EC já escolhido (`:157–164`). |
| `app/chamados/juizes.py:301` — `_ajustar_saida_overnight` | Juiz em `data_evento` recebido (`:303`). |
| `app/chamados/juizes.py:443` — `_celula_furada` | Juiz no D avaliado. |
| `app/chamados/juizes.py:1247` — `_marco_esperado_da_grade` | **Ativo mais recente**, sem vigência; D=`perg.data_evento`; lê marcos do **template** (`:1251–1256`). |
| `app/chamados/juizes.py:1378` — `folha_julga_pontualidade` | Se não recebeu template, juiz em `_data`; recebido, conserva o parâmetro (`:1384`). |
| `app/chamados/juizes.py:1575` — `_turno_cruza_meia_noite` | Juiz no dia recebido (`:1578`). |
| `app/chamados/services/disputa_emissao.py:752` — `_tem_base`; `:839` — `_ata_do_dia` | Juiz em `data` / `data_evento` da pergunta/ata (`:753`, `:840`). |
| `app/chamados/services/limbo_folgas.py:189` — `filhos_no_defeito` | Qualquer EC cobrindo dia do filho e com defeito de folga; não escolhe geradora (`:199`). |
| `app/chamados/services/limbo_folgas.py:252` — `agrupar_furos_sob_limbo` | Juiz no D do furo. |
| `app/chamados/services/desvio_escala.py:42` — `medir_desvio` | Ativo mais recente; aplica marcos à janela passada e compara batidas de 84 dias (`:54`, `:60–64`). |
| `app/chamados/views.py:440` — `detalhe_chamado_local` | Ativo do tipo gravado no contexto, maior início; não usa dia do chamado (`:442`). |
| `app/chamados/reconciliador.py:232` — `_ancora_ausente` | Existência de qualquer EC ativo sem âncora; sem D (`:235`). |
| `app/chamados/detector_anomalias.py:56` — colisão; `:99` — posto | Mais de um ativo sem fim; posto é FK atual do colaborador (`:64`, `:103`). |
| `app/chamados/services/disputa_emissao.py:312` — `gerar_perguntas_disputa` | Opções: todos os ativos sem fim; sem D (`:314`). |
| `app/chamados/models.py:2336` — `resposta_display` | EC pela PK gravada na resposta, não por vigência (`:2338`). |
| `app/ponto/registro_batida.py:28` — `_janela_oposto` | Juiz no dia local do timestamp (`:30`). |
| `app/ponto/registro_batida.py:40` — `_casa_marco_proprio` | Juiz no dia local e véspera; fornece EC aos marcos (`:47`). |
| `app/ponto/propositor.py:334` — `dia_posicional_completo` | Juiz no dia recebido; slots do template escolhido (`:337`). |
| `app/ponto/services/triagem_batida.py:149` — `triagem_bater_ponto` | Juiz no dia local de agora; EC para marcos (`:159`). |
| `app/ponto/services/triagem_batida.py:402` — `_bloqueio_horario_clt` | Juiz no dia local de `agora_local` (`:409`). |
| `app/ponto/services/triagem_batida.py:332` — `ramo_solicitar_anuencia` | Ativo mais recente para rótulo; instante da tentativa no contexto (`:335`, `:347`). |
| `app/ponto/services/triagem_batida.py:92` | Existência de qualquer EC ativo habilita registro; sem D. |
| `app/ponto/services/veredito_celula.py:40` — `_marcos_do_dia` | Juiz(D); fallback template após célula (`:43`). |
| `app/ponto/services/vigia_de_hora.py:46` — `_ontem_em_janela`; `:70` — `_acesas_de_sobra` | Juiz em `cel.data` / hoje, com célula alimentada (`:47`, `:71`). |
| `app/ponto/services/flip_auto.py:66` — `classificar_pares` | Juiz no dia dos pares (`:72`). |
| `app/ponto/services/furos_vetados.py:16` — `vinculo_do_emissor` | Juiz no D recebido; template julga degeneração (`:28`). |
| `app/ponto/services/declarar_dia.py:58`, `:90`, `:105`, `:176` | `_ec_do_dia`, teste de folga, recusa e escrita: juiz no dia declarado/recebido. |
| `app/ponto/services/cobertura.py:276` — `declarar_cobertura_passada` | Juiz no dia declarado; template histórico do titular (`:290`). |
| `app/ponto/services/faltas_de_hoje.py:28` — `faltas_de_hoje_sem_cobranca` | **Ativo mais recente**, sem vigência; D=hoje. Ciclo/fase pelo EC; horário da célula ou template (`:45`). |
| `app/ponto/services/abriu_nao_bateu.py:194` — `_hora_de_bater_passou` | Ativo mais recente; template aplicado ao dia recebido (`:203`). |
| `app/ponto/services/bordas_realizado.py:62` — `bordas_do_realizado` | Itera ECs ativos, aplica modo de intervalo e pausa desse EC a cada D da grade (`:78`). |
| `app/ponto/services/piso_intervalo.py:28` — `pares_abaixo_do_piso` | Itera ativos com intervalo por duração; horários do template para batidas da janela, sem EC por D (`:43`). |
| `app/ponto/services/diagnostico_escala.py:49` — `_vigente` | Primeiro EC carregado que cobre D; sem geradora (`:55`). Janela padrão hoje−83…hoje (`:128–150`). |
| `app/ponto/services/diagnostico_escala.py:182` — diagnóstico agregado | `vinculos[0]` como atual; referência=fim da janela, sem seleção por esse D (`:189`). |
| `app/ponto/worklist.py:49` — `coletar_gaps_dp` | Posto do ativo mais recente; dia do chamado não participa (`:51`). |
| `app/ponto/services/cluster_espurio.py:75` | Existência de ativo sem fim; sem D. |

**4. Fechamento, espelho, folha e relatórios**

| Arquivo:linha — função | Critério; origem de D |
|---|---|
| `app/ponto/services/fechamento.py:125` — `vinculo_de_referencia` | Percorre fim→início, devolve primeira resposta do juiz. |
| `app/ponto/services/fechamento.py:191` — `apurar_por_fatia` | Juiz para geradoras; fatias limitadas por início/fim de cada EC (`:205–206`). |
| `app/ponto/services/fechamento.py:923` — `recalcular_fechamento_mes` | Juiz em cada dia `_d_f` da janela para feriados. |
| `app/ponto/services/espelho.py:1058` — `autoridade_do_periodo` | Carrega janela+geradoras; juiz no **fim** fornece EC/template de referência (`:1070`); cálculo por fatias (`:1104–1116`). |
| `app/ponto/services/espelho.py:297` — `_realizado_do_dia_tela` | Juiz por D com vínculos/células do período (`:298`). |
| `app/ponto/services/espelho.py:42` — `piso_visual` | Ativo mais recente define piso; início futuro é limitado a hoje (`:47`). |
| `app/ponto/services/espelho.py:212` — `resumo_escala` | Ativo mais recente; posto do EC ou FK atual; horários do template; semana de hoje (`:218–235`). |
| `app/ponto/services/esmeril_espelho.py:93` — `assinaturas_do_colab` | Juiz no **fim da janela**; template recebido pelo motor para a janela (`:154–166`). |
| `app/ponto/services/fechamento_lista.py:52` — `montar_resumos` | Carrega lista completa; mapa de rótulos sobrescrito pelos ativos, sem D (`:56–61`). |
| `app/ponto/services/teto_na_lista.py:68` — `medir` | Juiz no `data_turno` do par (`:76`). |
| `app/folha/export.py:330` — `_ciclo_do_colab` | Juiz no fim da janela da competência (`:346–349`). |
| `app/folha/export.py:746` — `classificar_export` | Todos os EC alimentam minutos previstos por cada D da competência (`:910–916`); guarda de âncora considera **qualquer ativo 12x36 sem âncora**, sem vigência (`:787–793`). |
| `app/folha/export.py:1150` — `motivos_retencao_celula` | Existência de algum EC cobrindo cada D da janela (`:1193–1209`). |
| `app/folha/previa.py:28` — `montar_previa` | Vínculos do período/cobertura; D=`cel.data`, exclui D≥hoje (`:61–82`). |
| `app/core/regua_cct.py:297` — `regua_para` | Juiz em `competencia` recebido; praça do EC escolhido (`:328–330`). |
| `app/folha/views_retrato.py:89` — `_cadastro` | Ativos ordenados início crescente, sobrescrita: mais recente; posto EC ou FK atual; enriquece retrato sem D (`:107–122`). |
| `app/folha/services/ranking_he.py:94` — `ranking` | Mapa de ativos para ciclo da competência, sem vigência (`:130–135`). |
| `app/holerite/views.py:250` — `_pessoas_da_celula` | Ativo mais recente por ordenação/sobrescrita; posto EC ou atual; lista da competência ano/mês (`:280–312`). |
| `app/relatorios/dia_da_frota.py:25` — `dia_da_frota` | Juiz no dia recebido, com célula; posto EC ou FK atual (`:42–56`). |
| `app/relatorios/pdf_espelho.py:187` — `_coletar_dados_espelho` | Juiz no fim da janela para referência (`:303–304`); carrega vínculos+geradoras para cálculo (`:319–360`). |
| `app/relatorios/pdf_espelho.py:383` — `_montar_motor` | EC recebido da fatia; CCT em `fatia_ini` (`:390–391`). |
| `app/relatorios/pdf_espelho.py:561` — `_realizado` | Juiz por D impresso, com células/vínculos carregados (`:566–568`). |
| `app/relatorios/views.py:437` — `extrato_parcial` | Mapa de ativos fornece geometria às batidas da janela; cálculo financeiro separado (`:487–511`). |
| `app/relatorios/views.py:1317` — `_acc_escala` | Primeiro ativo em `c.escalas.all()`, sem D (`:1321`). |
| `app/relatorios/pendencias.py:65` — `com_direcao` | Mapa de ativos; diagnóstico hoje/próximos 30 dias (`:74–80`). |
| `app/relatorios/furos.py:78` — `escalas_em_limbo` | Todos os ativos sem fim, aplicados a hoje/amanhã, sem início≤D (`:85–95`). |
| `app/inteligencia/calculadores.py:295` — `_calc_escalas_desvio` | Ativo mais recente aplicado à grade de 30 dias até ontem (`:312–324`). |

**5. Telas, APIs, painéis e serviços de escala**

| Arquivo:linha — função | Critério; origem de D |
|---|---|
| `app/colaboradores/services/ficha.py:98` — `ficha_do_colab` | Todos os EC → juiz em hoje, **sem células**; hoje recebido ou local (`:117–128`). |
| `app/colaboradores/queries.py:50` — `colaboradores_em_operacao`; `app/colaboradores/views.py:2553` — `painel_situacional` | Existência de vínculo cobrindo hoje (`:62–65`, `:2605–2608`). |
| `app/colaboradores/services/calendario.py:246` — `contexto_calendario` | Ativo mais recente no cabeçalho (`:341`); seleção separada pelo juiz em cada dia da grade para extra (`:428–430`, `:473–475`). |
| `app/colaboradores/views.py:17` — `vincular_colaboradores` | Primeiro ativo do prefetch; cadastro atual, sem D (`:48`, `:71`). |
| `app/colaboradores/views.py:212` — `vincular_folga` | Ativo mais recente para alterar calendário; sem D (`:226–236`). |
| `app/colaboradores/views.py:262` — `painel_vinculo` | Ativo mais recente; aplica EC aos próximos 14 dias (`:268–294`). |
| `app/colaboradores/views.py:311` — `vincular_posto`; `:373` — `vincular_escala` | Ativo mais recente ao renderizar resultado (`:

[truncado em 20000 de 41454 caracteres]

## 29/10 14:42 · sessao 41a4d35fbf65

Complementos às lacunas:

| sítio | critério / D / efeito |
|---|---|
| `app/escala/utils.py:551-631` `_match_marcos` | **Não seleciona vínculo**: recebe marcos absolutos. Papel pelo turno fornecido (`:562`), ou alternância (`:563-564`); alinhamento por papel (`:578-590`), fallback DP no envelope dos marcos (`:591-599`). |
| `app/escala/utils.py:353-394` `marco_ocupado_pela_batida` | Recebe `dia` e tupla de horários; encadeia e chama matcher. Não procura vínculo. |
| `app/escala/utils.py:634,648,725-741` `montar_grade_prevista` | EC recebido; trabalho por `esc.eh_dia_trabalho(d)`; marcos do mapa ou TE do EC; matcher com `derivar=False`. D=`ref-i`. |
| `app/escala/utils.py:1221,1254-1255,1538-1559` grade por turno | EC do juiz para D; horários `dna_mapa[d]` ou `te.marcos_do_dia(d)` (`:1547`); pausas do DNA ou TE (`:1555-1557`); encadeia em D (`:1559`). |
| `app/escala/utils.py:1353-1357,1376` alimentação grade | D percorre `ref-dias .. ref`; carrega DNA e pausas nessa janela; FolgaDia pelos ECs da lista inteira. |
| `app/escala/utils.py:1590,1641-1643` demais callers locais do matcher | Probe de intervalo e chamada principal da grade por turno; principal passa turno do juiz se existe par de intervalo. |
| `app/escala/utils.py:173-204` `_eh_abertura` | EC recebido; D=`dt_local.date()`. Primeiro campos crus TE, depois método EC.marcos_do_dia(D) ou TE; exceção conserva crus. Não encontrei chamada em `escala/` na busca realizada. |
| `app/escala/utils.py:257-296` `_marcos_def_publico` | TE recebido: união estática de horários escalares/pausas e overrides `horarios_por_dia`; não resolve D/vigência. Grade por turno usa apenas ECs que venceram algum D da janela (`:1297-1315`). |
| `app/escala/utils.py:27,43-61` `pausas_dna_periodo`; `:84,93-107` `marcos_dna_periodo` | Célula `(colaborador,D)` da janela, sem selecionar EC; retorno é mapa por data. |
| `app/escala/utils.py:209-214` `calibrar_ancora_se_aplicavel` | No-op, retorna 0; não infere fase de batida. |
| `app/escala/utils.py:2028-2058` `recalibrar_ancora_retroativa` | Não infere fase. Se mudou período, delega invalidação à porta `invalidar_ancoras_incoerentes(colaborador,data_inicio)` (`:2044-2045`); senão consulta existência de qualquer EC ativo com âncora (`:2050-2054`). |
| `app/escala/utils.py:2021-2025` `montar_grade_prevista_periodo` | Flag `GRADE_POR_TURNO` escolhe um dos dois builders; ambos delegam vínculo por D ao mesmo juiz. |

Corpos de EC lidos agora:

- `app/escala/models.py:1138-1139` `intervalo_do_dia` retorna primeira pausa de `pausas_do_dia`.
- `app/escala/models.py:1157-1181` `pausas_do_dia`: célula por colaborador+D; DNA existente com marcos fornece pares; célula existente sem marcos → `()`; **só sem célula** cai no TE.
- `app/escala/models.py:1200-1210` `extra_declarada_do_dia`: célula por colaborador+D; extra do DNA ou zero; só sem célula cai no TE.
- `app/escala/models.py:1217-1221` `defeito_folga_nao_definida`: tipo do próprio EC, configuração 6x1, existência de FolgaDia desse EC no mês de D.
- `app/escala/models.py:1242-1267` fase calculada: âncora declarada no EC; sem foto mensal, 12x36 modulo2 e 24x48 modulo3.
- `app/escala/models.py:1280-1335` foto mensal: D explicitamente na foto → folga; fora da foto usa ciclo do TE se souber, senão paridade da FolgaDia mais próxima (empate data menor), inclusive fora do mês na consulta de vizinhas.
- `app/escala/models.py:1336-1342,1357-1363` sem foto usa ciclo TE; feriado pode converter trabalho em folga. **Lugar é `lugar_do_colab(self.colaborador)`, não EC.posto**.
- `app/escala/models.py:962-967` `recusa_ancora_futura` aceita âncora até `max(hoje,data_inicio)`; confirma suporte explícito ao início futuro no modelo.

Seletores antes apenas parcialmente lidos:

- `app/escala/services/escala_auto.py:172-182` `dry_da_frota`: ativa+fim nulo, ordena colaborador/`-data_inicio`, primeiro por colaborador; portanto **mais recente**, sem início<=hoje. Futuro pode vencer imediatamente.
- `app/escala/services/cadastro_realidade.py:186-200` `_jornada_que_mente`: competência de hoje até hoje; dict de TE por colaborador em todos ECs ativos, último iterado vence, sem vigência. Essa TE alimenta `periodos_do_dia(cel,te)` para todas células da competência.

Limites: buscas excluíram testes/migrations; templates não li. Em `escala/services/leitor_celula.py` fiz busca de definições e referências EC/TE/posto; não encontrei seletor de vínculo nessa busca, corpos completos não li. Métodos de TE puramente textuais/código administrativo não foram todos lidos integralmente. Não afirmar censo sem omissão de acessos indiretos `[nao verificado]`.

## 29/10 14:41 · sessao df5ef1ecfe7e

Censo do escopo fora de `escala/`, `ponto/`, `chamados/`, `api/`. Linhas relativas a `app/` da cópia `8ea0a691d3177b7d69e5f591250d0c4d0c8d1f68`. Busca ampla por `EscalaColaborador`, `escala_vigente`, `vinculo_do_dia`, `escalas_do_periodo`, `.escalas`, `.posto`, `posto_id`, `tipo_escala`, `data_ancora_colaborador`; leitura dos blocos abaixo. Testes, migrations e templates: não li nesta divisão. Totalidade além desses padrões: [não verificado].

**Escolha temporal pela autoridade**

| Arquivo:linha / função | Critério e origem de D |
|---|---|
| `colaboradores/queries.py:50,62-65` `colaboradores_em_operacao` | Gate de existência com `q_vinculo_cobre_a_data(hoje)`; D=`timezone.localdate()`. |
| `colaboradores/views.py:2553,2605-2608` `painel_situacional` | Mesmo gate `q_vinculo_cobre_a_data(_hj)`; D=hoje. |
| `colaboradores/services/ficha.py:98,117-128` `ficha_do_colab` | Carrega todos os vínculos, chama `vinculo_do_dia(colab.pk, hoje, vinculos)`, **sem células** deliberadamente; hoje é argumento ou `localdate`. |
| `colaboradores/services/calendario.py:428-430,473-475` `contexto_calendario` | Carrega período+geradoras, chama `vinculo_do_dia` para extra declarada; D=`_d['data']` da grade do mês. |
| `folha/export.py:330,346-349` `_ciclo_do_colab` | `vinculo_do_dia_carregado(colab, janela_fechamento(mes,ano,empresa)[1])`; D=fim da janela da competência. |
| `folha/export.py:746,910-916` `classificar_export` | Carrega **todos** os EC e passa a `minutos_previstos_do_dia`; D=laço `_i.._f` da competência. |
| `folha/export.py:1150,1193-1209` `motivos_retencao_celula` | Carregador `escalas_do_periodo`; existência por `any(vinculo_cobre_a_data(e,d))`; D=ini..fim. |
| `folha/previa.py:28,61-62,77-82` `montar_previa` | Carrega período por colaborador; filtra cobertura pelo juiz; D=`cel.data`, exclui D>=hoje. |
| `core/regua_cct.py:297,328-330` `regua_para` | `vinculo_do_dia_carregado(colaborador, competencia)` e praça do **EC escolhido**; D=argumento `competencia`. |
| `relatorios/dia_da_frota.py:25,42-43,56` `dia_da_frota` | `escala_vigente(cel.colaborador_id,data,celulas={...})`; posto do EC, fallback FK do colaborador; D=argumento `data`. |
| `relatorios/pdf_espelho.py:187,303-304` `_coletar_dados_espelho` | `vinculo_do_dia_carregado(colaborador.pk,data_fim)`; D=fim da janela impressa. |
| `relatorios/pdf_espelho.py:319-322,350-360` `_coletar_dados_espelho` | **Carregador**, não escolhe: `escalas_do_periodo(data_ini,data_fim_mes)` + ECs nomeados pelas células fora da janela. |
| `relatorios/pdf_espelho.py:383,390-391` `_montar_motor` | Usa `esc_fatia.tipo_escala`, recebido da apuração por fatia; referência CCT=`fatia_ini`. Não escolhe vínculo aqui. |
| `relatorios/pdf_espelho.py:561,566-568` `_realizado` | `vinculo_do_dia(colaborador.pk,d,_escalas_periodo,_cels_esp)`; D=dia impresso. |
| `relatorios/management/commands/vinculo_do_dia_divergentes.py:75,99-116` `handle` | Monitor compara juiz `vinculo_do_dia`113 com seletor antigo intervalo114-116 (`max(ativa,data_inicio)`). D=laço ini..fim108; não é caminho de cálculo produtivo. |

**Escolha por ativa, sem comparar início ao dia consultado**

| Arquivo:linha / função | Critério; D/instante efetivamente consultado |
|---|---|
| `colaboradores/views.py:17,48,71` `vincular_colaboradores` | Prefetch `ativa=True`; primeiro da lista. Tela cadastral atual, sem D explícito. |
| `colaboradores/views.py:212,226-236` `vincular_folga` | `ativa=True.first()`; escolhe EC a alterar calendário. Sem D. |
| `colaboradores/views.py:262,268-272` `painel_vinculo` | `ativa=True.first()` para painel atual. |
| `colaboradores/views.py:311,336,355` `vincular_posto` | `ativa=True.first()` ao renderizar resultado da escrita de posto. |
| `colaboradores/views.py:373,394` `vincular_escala._render_row` | `ativa=True.first()` para linha resultante da escrita. |
| `colaboradores/views.py:915,927` `painel_colaborador` | `ativa=True.first()` para ficha/painel. |
| `colaboradores/views.py:2185,2202-2204,2207-2223,2233` `agenda_modal_view` | Um EC `ativa=True.first()` para **todos os sete D da semana** do GET `semana` ou hoje; chama `eh_dia_trabalho(d)` e `marcos_do_dia(d)`. Agenda específica é outra escolha: `(colab,data=d)`, exclui substituída, mais recente por `criado_em`2228-2230. |
| `colaboradores/views.py:2818,2860-2861,2878-2885` `disponibilidade` | Primeiro ativo por colaborador (`setdefault`); usa para hoje e próximos sete dias. |
| `colaboradores/views_fase.py:36,45-49` `fase_12x36` | Ativo mais recente por `-data_inicio`, sem D; base do preview de fase. D do preview/declaração vem GET56-57,81-93. |
| `colaboradores/services/drawer.py:17,22-32` `contexto_drawer` | Ativo mais recente por `-data_inicio`; escala/horas e posto; fallback `colab.posto`. Referência atual=`agora`19, presença últimos sete dias20. |
| `colaboradores/services/detalhe.py:10,72-75` `contexto_detalhe` | `ativa=True.first()` para detalhe das batidas; não compara com período observado. |
| `colaboradores/services/calendario.py:246,341` `contexto_calendario` | EC ativo `.first()` para contexto superior do calendário; escolha separada da escolha por dia473. |
| `colaboradores/services/troca_pelo_chat.py:50,54-56` `checar_troca` | Ativo mais recente por `-data_inicio` para rótulo “de”; `data_inicio` da troca é argumento e vai ao juiz59. |
| `colaboradores/services/vinculo.py:167,180-182` `executar_vinculo` | Ativo mais recente por `-data_inicio` para texto de histórico **antes da escrita**; não decide D. |
| `core/views.py:335,401-405` `dashboard_supervisor` | Ativo `.first()` por colaborador e depois `eh_dia_trabalho(hoje)`; D=hoje. |
| `core/views.py:443,454-458` `onboarding` | Ativo `.first()` para exigir âncora; sem D. |
| `core/views.py:749,787-789,806,819-825,861` `painel_tv` | Dicionário sobrescrevendo por colaborador, somente `ativa=True`; geometria do turno usa `turno.data_turno`823, cobertura atual usa `ec.tipo_escala`861. Agrupa por `colab.posto_id`831. |
| `core/services/painel_op.py:19,74-79,116-123,162-164` `montar_painel_operacional` | Dicionário de ativos, sem vigência; `_previsto_agora` usa mesmo EC para ontem/hoje116 e marcos120; prazo turno usa `_tm.data_turno`164. Agrupa por FK atual231-232. |
| `ferias/management/commands/alertar_retornos.py:9,15-16,27-35` `handle` | Primeiro EC ativo por colaborador (`setdefault`); compara `eh_dia_trabalho(hoje)` e amanhã. |
| `folha/views_retrato.py:89,107-115,122` `_cadastro` | Itera ativos ordenados `data_inicio` crescente, sobrescreve mapa: **ativo de início mais recente**; posto EC ou FK atual, escala EC. Enriquecimento de linhas do retrato, sem D. |
| `folha/services/ranking_he.py:94,130-135` `ranking` | Filtro de ciclo da competência usa dicionário EC `ativa=True`, sem vigência e sem desempate explícito local. |
| `folha/export.py:746,787-793` `classificar_export` | Exige âncora se **qualquer EC ativo 12x36 sem âncora**, sem conferir competência nem `data_inicio`. |
| `relatorios/pendencias.py:65,74-80` `com_direcao` | Mapa de ativos; diagnóstico do EC para hoje e 30 dias. |
| `relatorios/furos.py:78,85-95` `escalas_em_limbo` | Todos `ativa=True,data_fim IS NULL`; testa hoje e amanhã, sem início<=D. |
| `relatorios/views.py:437,490-492,509-511` `extrato_parcial` | Mapa ativo para geometria `montar_periodos_de` das batidas da janela (`inicio..fim_real`502); cálculo financeiro usa outra fonte487. |
| `relatorios/views.py:807,830-832,856-858` `filtro_search_ajax` | Filtra pessoas/conta uso por EC ativo e tipo; sem D. |
| `relatorios/views.py:1317-1321` `_acc_escala` | Primeiro EC com `.ativa` em `c.escalas.all()`; sem D. |
| `holerite/views.py:250,280-284,310-312` `_pessoas_da_celula` | Ativo mais recente (ordena crescente e sobrescreve); posto EC ou FK atual, escala EC; lista de entrega da competência `ano,mes`285. |
| `inteligencia/calculadores.py:295,312,319-324` `_calc_escalas_desvio` | EC ativo mais recente; julga suspeita sobre grade dos 30 dias até ontem. |
| `colaboradores/services/geofence_raios.py:35,63-64` `raios_fora_do_padrao` | Conta pessoas por `EC.posto=p,ativa=True`; sem D. |
| `colaboradores/management/commands/sincronizar_feriado_admin.py:31,35-42` `handle` | Todos EC ativos da empresa para escolher alteração de `trabalha_em_feriado`; sem D. |
| `relatorios/management/commands/diff_extrato_parcial.py:81,107` `handle` | Comparador com mapa de ativos; `_antes` usa `esc.tipo_escala`40. D=janela comparada. |

**Históricos/carregadores/portas que não escolhem “vigente em D”**

| Arquivo:linha / função | Pergunta/critério |
|---|---|
| `inteligencia/resumo.py:17,49-52` `resumo_colaborador` | Lista todos EC ordenados `-data_inicio` e histórico por `-data`; não elege vigente. |
| `colaboradores/services/ficha.py:78,94,121-122,130-132` `linha_do_vinculo`, `ficha_do_colab` | Linha lê posto de EC recebido; lista todos e trocas da competência pelo `data_inicio`; vigente é autoridade128. |
| `colaboradores/services/vinculo.py:50,99-101` `julgar_vinculo` | EC com igualdade `data_inicio=D` do POST; **idempotência**, não busca vigente. |
| `colaboradores/services/troca_pelo_chat.py:41,47` `precisa_inicio_do_turno` | Existência de EC com início igual ao argumento; não busca vigente. |
| `colaboradores/services/vinculo.py:460,473`, `486,504`, `809,817` | `vigencias_impossiveis`: todos fim não nulo; `vigencias_sobrepostas`: ativos para auditoria; `fechar_vinculos_ativos`: ativos como alvos de escrita. |
| `colaboradores/management/commands/corrigir_escala_retroativa.py:53,64,107-110` `_rodar` | EC por PK explícita do argumento; projeção altera template/âncora; não escolhe vínculo por data. |
| `colaboradores/management/commands/tripwire_vinculo_orfao.py:41,45` `_rodar`; `fechar_vinculo_de_desligado.py:32,34` `_rodar` | Busca auditoria de ativos do colaborador desligado; bloco completo além da linha de query não li. |

**Posto atual aplicado a fatos/dias históricos ou ao instante** [nao verificado]

| Arquivo:linha / função | Critério e referência |
|---|---|
| `core/feriados.py:106,113-114` `lugar_do_colab` | `Colaborador.posto.municipio`, sem D; todos chamadores herdam praça atual. |
| `colaboradores/services/feriado_do_posto.py:74,86-99,102-110` `presenca_em_feriado` | Para células históricas ini..fim (padrão últimos120 dias), posto=`cel.colaborador.posto`; feriado pelo município atual. |
| `colaboradores/services/calendario.py:343-346` `contexto_calendario` | Feriados da janela pelo `lugar_do_colab` atual; escolha de EC por dia473 não modifica essa origem. |
| `relatorios/pdf_espelho.py:293-294` `_coletar_dados_espelho` | Feriados da competência pelo `lugar_do_colab` atual. |
| `colaboradores/services/situacional.py:65,179,201-203` `montar_situacional` | Geofence de última coordenada contra `c.posto` atual. |
| `colaboradores/services/detalhe.py:92-94,133-137` `contexto_detalhe` | Centro/raio de geofence por `colaborador.posto` atual. |
| `colaboradores/painel_cell.py:11,40,56-58` `montar_linha_colab` | Posto/geofence da linha por FK direta atual. |
| `core/views.py:968,1012` `batida_detalhe_json` | Exibe posto atual junto da batida consultada. |
| `inteligencia/calculadores.py:10,73-79` `preenchimento_espelho` | Agrupa resultado histórico por `c.posto_id` atual. |
| `folha/services/ranking_he.py:94,122,146` `ranking`; `167,221` `apuracao_colab` | Filtro/label de posto pela FK atual, embora resultados sejam da competência. |
| `relatorios/views.py:57,92-93` `horas_mes`; `129,159` `inconsistencias`; `180,212` `absenteismo`; `249,300` `atestados_acumulados`; `336,369` `ferias_vencendo`; `388,420` `afastados`; `437,560` `extrato_parcial`; `601,654` `beneficios_consolidado` | Labels de posto pela FK atual. |
| `relatorios/views.py:1352,1355,1361` `_grupos_por_posto` | Agrupa pela lotação atual explicitamente; comentário1348 declara não ser escala do dia. |
| `relatorios/pdf_espelho.py:676,724` `_build_story`; `relatorios/pdf_chamados.py:25,63` `_build_story`; `relatorios/furos_pdf.py:25,35` `_card_flow`; `relatorios/services.py:267,294,313` `gerar_pdf_ferias_gestao_bytes.build` | Labels de posto atual nos documentos. |
| `colaboradores/views.py:2818,2852,2885,2902,2922` `disponibilidade` | Posto atual nos retornos de férias/folga e ausências, cujas datas diferem de hoje. |
| `colaboradores/services/ficha.py:211-218` `ficha_do_colab` | Lotação/UF cadastral por FK atual, separada do EC vigente128. |

**Auxiliares atuais sem pergunta temporal explícita**: `colaboradores/views.py:80,342,361,401,625` (linhas cadastrais/busca); `core/busca.py:85,89,151,154` (rótulos); `colaboradores/services/cadastro.py:74,80` (pendência de cadastro); `colaboradores/mixins.py:200,231-232` (visibilidade/permissão); `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:42,58,78,109` (rótulo/âncora de pauta). Todos leem FK atual; não demonstram escolha temporal.

**Conclusão sobre vínculo futuro**

- Leitores da primeira tabela consultam D pela autoridade/carregador; a gravação antecipada de um novo EC não equivale, nesses sítios, a escolher o mais recente sem limite temporal (`ficha.py:128`, `regua_cct.py:329`, `pdf_espelho.py:303,567`).
- Toda a tabela de ativos ignora `data_inicio<=D` no seletor. Com EC futuro já `ativa=True`, pode ler o futuro **antes** do início; em consultas retrospectivas pode continuar usando o EC novo **depois** do início para D antigo. Isso está demonstrado especialmente em agenda por semana (`views.py:2202-2223`), histórico de geometria (`relatorios/views.py:492,511`), turno vivo de ontem (`painel_op.py:76,116,164`), competência do holerite (`holerite/views.py:282-284,310-312`) e diagnóstico de últimos30dias (`inteligencia/calculadores.py:312,319-324`). Resultado efetivamente errado em dados reais: [não verificado].
- `Colaborador.posto` é outro eixo: leitores citados não têm argumento D para escolher lotação histórica (`core/feriados.py:113-114`, `feriado_do_posto.py:99`). Criar EC futuro sozinho não prova alteração dessa FK; o executor **copia** o posto atual para o EC (`services/vinculo.py:266,280`).

**Portas futuras e escrita**

- Confirmação adicional à investigação do root: `julgar_vinculo` recebe `data_inicio` do POST (`services/vinculo.py:53`), só impõe piso (`70-71`) e submete âncora à guarda (`131-134`). `executar_vinculo` escreve `data_inicio` recebido e `ativa=True` (`278-285`) e trata anteriores desde já (`203-208`); não há agendamento por relógio nesse executor.
- Troca por chat reaproveita a mesma porta: `trocar_escala_porta` recebe `data_inicio`, converte ISO, chama `julgar_vinculo` e `executar_vinculo` (`colaboradores/services/troca_pelo_chat.py:65-78`).
- `fase_12x36` lê `data_declarada` do GET (`views_fase.py:81`) e, no modo declarada, a usa como âncora/início (`89-93`); a tela prepara valor para a porta, não grava nessa função.
- Portas auxiliares também aceitam datas de argumento: `ajustar_vinculo_pelo_sistema` cria EC `data_inicio=inicio,ativa=True` (`services/vinculo.py:925,930-932`); `desfazer_ajuste_do_sistema` cria retorno `data_inicio=volta,ativa=True` (`936,945-947`). Chamadores dessas duas: não li.
- Posto direto: root já confirmou `views.py:311-332,1366-1400` como alterações imediatas, sem data de vigência. Não encontrei nesta divisão outra porta de posto futuro; totalidade de portas fora dos padrões buscados [não verificado].

## 29/10 14:41 · sessao c03300972490

Censo do escopo delegado; caminhos de seleção abaixo. `EC` = EscalaColaborador; `V(D)` = delegação para `escala_vigente`/`vinculo_do_dia`/carregador correspondente. Não executei aplicação, testes ou banco. Escala/alimentacao e implementação desses juízes ficaram com outro investigador; aqui conferi os chamadores. [nao verificado]

**Seletores que não usam vigência de D**

| Arquivo:linha | Função | Critério e origem de D |
|---|---|---|
| `api/views.py:312-314`; `api/views_core.py:301-303` | `api_me` | EC `ativa=True`, `.first()`, sem data; auxiliares `_escala_de_hoje` aplicam EC recebido a hoje em `api/views.py:252-261`, `api/views_core.py:241-250`. |
| `api/views.py:918-939` | `api_escala_v2` | EC ativa `.first()`, uma única EC aplicada aos 14 dias desde hoje (`926,932-939`); posto dela em `967`. |
| `ponto/views.py:2044-2046` | `_escala_rotulo_do_colab` | EC ativa `.first()`, sem D; rótulo exibido no painel de ausência. |
| `chamados/views.py:440-442` | `detalhe_chamado_local` | EC ativa com `tipo_escala_id` do contexto, última `data_inicio`; não usa dia do chamado. |
| `chamados/juizes.py:1247-1256` | `_marco_esperado_da_grade` | D=`perg.data_evento`; EC ativa de maior início, sem comparar D; chama **template** `tipo_escala.marcos_do_dia(d)`. |
| `chamados/services/desvio_escala.py:42-54` | `medir_desvio` | EC ativa de maior início; aplica seus marcos a hoje−1…14 e compara 84 dias de batidas (`60-64`). |
| `ponto/services/faltas_de_hoje.py:28-45` | `faltas_de_hoje_sem_cobranca` | D=hoje local; EC ativa de maior início; ciclo/fase/folga por essa EC, horário preferencial da célula com fallback template. |
| `ponto/services/abriu_nao_bateu.py:194-203` | `_hora_de_bater_passou` | D=`dia` recebido; EC ativa `.first()`, horário de `tipo_escala.marcos_do_dia(dia)`. |
| `ponto/services/triagem_batida.py:332-335` | `ramo_solicitar_anuencia` | EC ativa `.first()` para rótulo de ocorrência fora de horário; instante da tentativa em contexto (`347`). |
| `ponto/services/espelho.py:42-47` | `piso_visual` | EC ativa de maior início define piso; futuro vira hoje pelo clamp `46-47`, podendo esconder passado antes do início. |
| `ponto/services/espelho.py:212-235` | `resumo_escala` | EC ativa de maior início; posto da EC ou FK atual do colab (`218`); horários do template; semana de hoje (`234-235`). |
| `ponto/services/fechamento_lista.py:56-61` | `montar_resumos` | Mapa de rótulo sobrescrito por toda EC ativa, sem critério de D explícito; lista completa separada em `52-54`. |
| `ponto/services/bordas_realizado.py:62-78` | `bordas_do_realizado` | Itera ECs ativas; aplica intervalo livre daquela EC e `ec.intervalo_do_dia(d)` a cada D da grade na janela. |
| `ponto/services/piso_intervalo.py:28-43` | `pares_abaixo_do_piso` | Itera EC ativa com intervalo modo duração; usa hi/hf do template para parear batidas entre `data_ini` e `data_fim`; não escolhe EC por dia da batida. |
| `ponto/worklist.py:49-51` | `coletar_gaps_dp` | Posto da primeira EC ativa; D dos chamados vem depois por `dia_do_fato`, sem afetar seleção. |
| `ponto/management/commands/tripwire_celulas.py:17-29` | `medir_stale` | Mapa sobrescrito por EC ativa; para células de hoje…fim compara `ec.eh_dia_trabalho_calculado(cel.data)`/DNA; sem seleção por vigência. |
| `ponto/management/commands/fechar_chamados_meio_periodo.py:59-75` | `handle` | Mapa sobrescrito por EC ativa; D=`dia_do_fato(ch)`; template dessa EC decide dia curto. |
| `ponto/management/commands/reconciliar_fantasmas.py:172-178` | `handle` | Primeira EC ativa fornece existência de intervalo; dia histórico vem de `dia_do_fato(c)`. |
| `ponto/management/commands/detectar_intervalo_ausente.py:110-115` | `handle` | Primeira EC ativa serve como gate existência/template; os furos depois vêm de `furos_intervalo(colab,hoje,dias)`. |
| `ponto/management/commands/diff_espelho_alimentacao.py:94-110` | `handle` | Primeira EC ativa para lado legado da comparação; lado por período separado `96,105`; D=janela CLI `ini,fim`. |
| `ponto/management/commands/censo_fase_12x36.py:295-312` | `_curar` | Última EC ativa fornece template do POST de cura; virada tem contraprova canônica separada `304-308`. |
| `ponto/management/commands/supra_juiz.py:130-146` | `_sem_fase` dentro de `handle` | Ordena todos por início e guarda último início≤`dref`; **ignora fim/ativa**, depois lê ciclo/âncora. Não antecipa futuro, mas pode usar EC encerrada após seu fim. |
| `ponto/services/diagnostico_escala.py:49-55,150` | `_vigente`, `diagnostico_escala` | Primeiro EC carregado que `vinculo_cobre_a_data(ec,d)`; não consulta geradora da célula para desempatar. Janela default hoje−83…hoje (`128-137`). |
| `ponto/services/diagnostico_escala.py:182-189` | `diagnostico_escala` | Diagnóstico agregado usa `vinculos[0]` como “atual”, ref=fim, inclusive template para pausa; sem selecionar EC em fim. |

**V(D), com data explícita**

| Arquivo:linha | Função | Origem de D / particularidade |
|---|---|---|
| `chamados/detector_anomalias.py:137-164` | `detectar_batida_fora_escala` | EC escolhida pelo **dia civil da batida** (`144`); depois deriva `data_turno_ref` pelo turno (`157-164`) e usa EC já escolhida para aquele dia do turno. Potencial divergência overnight/troca: hipótese de efeito, seleção distinta comprovada. |
| `chamados/juizes.py:301-303` | `_ajustar_saida_overnight` | `data_evento` recebido. |
| `chamados/juizes.py:443` | `_celula_furada` | D=`d` avaliado pela função. |
| `chamados/juizes.py:1378-1384` | `folha_julga_pontualidade` | Quando template não foi recebido, todos os ECs → V(`_data`); caso contrário respeita template recebido. |
| `chamados/juizes.py:1575-1578` | `_turno_cruza_meia_noite` | `dia` recebido. |
| `chamados/services/disputa_emissao.py:752-753` | `_tem_base` | `data` recebido, testa dia de trabalho EC. |
| `chamados/services/disputa_emissao.py:839-840` | `_ata_do_dia` | `data_evento` da pergunta/ata. |
| `chamados/services/limbo_folgas.py:189-199` | `filhos_no_defeito` | Todos ECs; `any(vinculo_cobre_a_data(e,dia) and defeito_folga...)`; D dos filhos. |
| `chamados/services/limbo_folgas.py:252` | `agrupar_furos_sob_limbo` | D=`d` do furo. |
| `api/views.py:619-622`; `api/views_core.py:611-614` | `api_bater_ponto` | D=`_dia_retro` solicitado para registro retroativo; horários lidos do template da EC escolhida. |
| `ponto/registro_batida.py:28-30` | `_janela_oposto` | D=dia local do timestamp da batida. |
| `ponto/registro_batida.py:40-47` | `_casa_marco_proprio` | D=dia local da batida e véspera; marcos recebendo `ec`. |
| `ponto/propositor.py:334-337` | `dia_posicional_completo` | `dia` recebido; template da EC fornece slots. |
| `ponto/precedencia.py:259-291` | `fatos_do_periodo` | ECs da janela incluindo geradoras; V(D) em cada data do período; folga de calendário vinculada à EC `297-299`. |
| `ponto/precedencia.py:469-470` | `veto_de_lancamento` | D=`d` da data examinada. |
| `ponto/supra_juiz.py:136` | fechamento `_f` de `cadastro_do_dia` | V(D) usando todos ECs/células fornecidos; D do classificador. |
| `ponto/supra_juiz.py:147` | `cadastro_do_dia_carregado` | D recebido, carregador canônico. |
| `ponto/services/cartorio.py:675-679` | `marcos_vencidos` | D recebido + célula explicitamente alimentada. |
| `ponto/services/cartorio.py:839-845` | `_cadastro` em julgamento | Todos ECs+células da janela → `cadastro_do_dia(data_)`. |
| `ponto/services/cartorio.py:1347-1349` | `_julgar_colab_corpo` | D=`data` em julgamento; alias canônico `_vinculo_do_dia`. |
| `ponto/services/cartorio.py:1446-1450` | `_marcos_do_furo_parcial` | D recebido. |
| `ponto/services/cartorio.py:1554-1556` | `julgar_celula` | D da célula; usa fim do turno para hoje. |
| `ponto/services/triagem_batida.py:149-159` | `triagem_bater_ponto` | Dia local de `agora`; usa EC para marcos da tentativa. |
| `ponto/services/triagem_batida.py:402-409` | `_bloqueio_horario_clt` | Dia local de `agora_local`. |
| `ponto/services/veredito_celula.py:40-43` | `_marcos_do_dia` | D recebido; fallback template após leitura da célula. |
| `ponto/services/vigia_de_hora.py:46-47` | `_ontem_em_janela` | D=`cel.data`, célula alimentada. |
| `ponto/services/vigia_de_hora.py:70-71` | `_acesas_de_sobra` | Hoje, célula de hoje alimentada. |
| `ponto/services/flip_auto.py:66-72` | `classificar_pares` | D=`dia` dos pares. |
| `ponto/services/furos_vetados.py:16-28` | `vinculo_do_emissor`, consumidor | D recebido; template da EC julga régua degenerada. |
| `ponto/services/declarar_dia.py:58,90,105,176` | `_ec_do_dia`, `o_dia_ja_e_folga`, `_recusa_do_dia`, `_escrever_folga` | D recebido/declarado. |
| `ponto/services/cobertura.py:276-290` | `declarar_cobertura_passada` | D=`dia` declarado; template histórico do titular grava cobertura. |
| `ponto/services/esmeril_espelho.py:93,154-166` | `assinaturas_do_colab` | EC no **fim da janela**; template vai ao motor para janela inteira. |
| `ponto/services/teto_na_lista.py:68-76` | `medir` | D=`p['data_turno']` do par separado pelo teto. |
| `ponto/services/fechamento.py:125` | `vinculo_de_referencia` | Percorre fim→início e devolve primeira resposta V(D). |
| `ponto/services/fechamento.py:191,205-206` | `apurar_por_fatia` | V(D) para geradoras; fatia por `max(ini,inicio)`/`min(fim,data_fim)` de cada EC. |
| `ponto/services/fechamento.py:923` | `recalcular_fechamento_mes` | V(`_d_f`) para feriados dos dias da janela. |
| `ponto/services/espelho.py:297-298` | `_realizado_do_dia_tela` | V(D) com ECs/células do período. |
| `ponto/services/espelho.py:1058-1070` | `autoridade_do_periodo` | ECs da janela + geradoras; V(**fim**) fornece `esc` e template da Autoridade; cálculo usa fatias (`1104-1116`). |
| `api/views.py:1369-1372,1627-1652` | view do espelho/app | Recebe `esc`/template da Autoridade no fim da janela; aplica a dias faltantes `_d` no preenchimento do calendário, inclusive `esc.eh_dia_trabalho(_d)`/template.marcos. Isso é **EC recebido**, não seleção local por D. |

**Comandos — V(D) / cobertura de D**

- `ponto/management/commands/detectar_ausencias.py:136-143`, `handle`: hoje e ontem; V(data_ref).
- `processar_alertas_turno.py:106,134`, `handle`: `tm.data_turno`.
- `processar_alertas_avancados.py:37`, `handle`: ontem.
- `emitir_turno_aberto_retroativo.py:104`, `handle`: `data` iterada.
- `reconciliar_grade.py:156,195`, `handle`: `dt`/`_d_rg` da janela; segundo caminho alimenta todos ECs/células.
- `emitir_furo_retroativo.py:68-71`, `handle`: cadastro_do_dia com todos ECs e células de `ini…fim`.
- `supra_juiz.py:158-161`, `handle`: mesmo carregador para dias do classificador; seletor particular `_sem_fase` registrado acima.
- `ausencias_sem_efeito_na_celula.py:55`, `classificar`: existência de EC cobrindo D via `q_vinculo_cobre_a_data(d)`.
- `curar_residuo_bug115.py:82`, `handle`: dia do chamado, só se célula ausente.
- `vigia_plantio_overnight.py:42`, `batidas_plantadas_no_dia_errado`: `t.data_turno`.
- `vigiar_motor_autoridade.py:51`: hoje.
- `censo_col900.py:108-111`, `handle`: dia explicitado no censo, todos ECs/células.
- `censo_fase_12x36.py:111`, `handle`: cada `_d` da janela.
- `dna_x_batida_real.py:73-77`, `handle`: cada `d`, todos ECs/células.
- `aplicar_col900.py:60-69,108-110`, `handle`: dia da operação; segundo caminho força ausência de célula para escolher cadastro.
- `diff_col900.py:64-66`, `_mutacao`: dia do caso, sem célula.
- `diff_calculador.py:374`: bulk de ECs; detalhes posteriores da delegação não li.
- `tripwire_celulas.py:89-107`, `medir_pausa_faltando`: todos ECs, V(`cel.data`) com célula.
- `tripwire_celulas.py:57-68`, `medir_ausentes`: itera ECs ativas mas só testa `ec.data_inicio <= d <= teto`; **não data_fim**, detector de célula ausente.
- `gerar_celulas.py:277-279`, `_outro_vinculo_cobre`: outro EC explicitamente cobrindo `d` pelo predicado canônico.
- `chamados/management/commands/auditar_disputas.py:112-116`, `handle`: dia local da batida.
- `chamados/management/commands/auditar_invariantes_chamados.py:359-360`, `contraprova_piso`: existência cobrindo `p.data_evento`; `390`: valida geradora contra `c.data`.

**Portas: EC recebido/seleção para geração**

- `ponto/portas/celula.py:568-590`, `gerar_celulas_periodo`: itera ECs ativas, **depois filtra `vinculo_cobre_a_data(ec,d)`**, calcula fase em D; não é leitor active-only.
- `ponto/portas/regen_exportada.py:142-155`: ECs da janela+geradoras; por D escolhe único EC cobrindo data; recusa ambiguidade; sem cobertura usa FK da geradora apenas para limpar órfã.
- `ponto/portas/celula.py:228-264`: `regenerar_celulas_vinculo` recebe EC e verifica/atualiza relação geradora da célula; seleção inicial do EC é responsabilidade do chamador. Corpo integral da porta não li.

**Leituras de cadastro sem “EC do dia” (não omitir no T20)**

- `chamados/detector_anomalias.py:56-64`: colisão = >1 ativa sem fim; `99-103`: posto atual do colaborador ativo/inativo.
- `chamados/reconciliador.py:232-235`, `_ancora_ausente`: qualquer EC ativa sem âncora; sem D.
- `chamados/services/disputa_emissao.py:312-314`, `gerar_perguntas_disputa`: dropdown = todas ativas sem fim; sem D.
- `chamados/models.py:2336-2338`, `resposta_display`: EC por PK gravado na resposta; não escolhe vigência.
- `api/views_mensageria.py:1212-1221`, `api_mensageria_pendencias_cadastro`: todas ECs ativas 6x1 sem calendário, sem D.
- `ponto/services/triagem_batida.py:92`: existência de qualquer EC ativa para habilitar registro.
- `ponto/services/cluster_espurio.py:75`: existência ativa sem fim para reconciliar.
- `ponto/management/commands/detectar_cluster_espurio.py:36`, `detectar_par_relampago.py:335`, `propor_reorganizacao.py:42`: universo por EC ativa/sem fim; não seleção por D.
- `chamados/services/limbo_folgas.py:64`; `desvio_escala.py:106`: contagem/universo de outras ECs ativas do mesmo template.
- `ponto/management/commands/gerar_batidas_smoke_motor.py:73,125-126`: gerador de dados artificial lê ativa; fase usa âncora ou PERIODO_INI. Não executado.

**FK de posto atual, inclusive leitores históricos**

- `ponto/models.py:100`, `Batida.posto_para_calculo`: `posto_efetivo or colaborador.posto`; sem lookup por timestamp.
- `ponto/services/geofence.py:48`, `vinculo_contagem`: posto atual contra últimas batidas GPS; `236`, `verificar_geofence`: propriedade anterior; `400`, `reavaliar_chamados_geofence`: posto atual contra chamado.
- `ponto/services/vinculo_na_lista.py:65-87,105`, `achar`/`item`: coordenadas e raio do posto atual contra conjunto histórico de pontos.
- `api/views.py:356`, `api/views_core.py:345`, `api_me`: posto atual.
- `api/views_mensageria.py:805,831,972,1053`: férias/onde_trabalha/pessoa apresentam posto atual.
- `chamados/services/abertura.py:62,187`: contexto do chamado copia posto atual.
- `chamados/services/adesao.py:14`; `chamados/views.py:695`: texto/fio com posto atual.
- `ponto/services/dias_em_aberto.py:126`, `dia_do_colab.py:77`, `vigia_entrada.py:136`, `abriu_nao_bateu.py:143`: rótulos com posto atual.
- `ponto/views.py:3068`, `cobertura_disponiveis`: posto origem atual do substituto.
- Cobertura tem **outra FK efetiva por ordem**: `api/views.py:594`, `api/views_core.py:586`, `ponto/services/triagem_batida.py:690` recebem `ordem_ativa.posto`; serviço `ponto/services/cobertura.py:74,104` usa template/posto escolhidos explicitamente no payload.

**Consequência de vínculo futuro**

Provado no código: active-only/último início não comparam a validade com D nos sítios da primeira tabela. **Hipótese condicionada à porta manter nova EC ativa e/ou desativar a anterior:** selecionam antecipadamente EC futura, ou deixam de achar EC antiga; `.first()` sem ordering explícito não permite afirmar qual das duas ativas vence sem conferir Meta.ordering. Mesmo depois do início, continuam errando para consultas históricas anteriores à vigência. Seletores V(D) e portas com `vinculo_cobre_a_data` não antecipam vínculo apenas por ele existir. FK `colaborador.posto` muda todas as leituras acima no momento da gravação dessa FK, independentemente do D consultado.

Não li telas/portas de gravação fora do escopo; parent tem investigação própria delas. Não afirmo completude literal app-inteiro a partir deste escopo: busca cobriu referências a EC/aliases, juízes de vínculo, marcos, fase e posto nos diretórios delegados; chamadas apenas indiretas sem esses termos podem faltar.

## 29/10 14:41 · sessao 41a4d35fbf65

Escopo lido por busca estática e trechos: `escala/`, `ponto/turnos.py`, `ponto/motor_calculo_v2.py`, `ponto/calculador/`, `ponto/nucleo.py`, `ponto/selecao_periodo.py`, na cópia indicada. Não executei código da aplicação, testes ou banco. Não posso afirmar exaustividade de acessos indiretos/dinâmicos `[nao verificado]`.

Seletores por dia:

| arquivo:linha / função | critério e origem de D |
|---|---|
| `app/escala/alimentacao.py:196,236-254` `vinculo_do_dia` | Geradora da célula `(colaborador,D)`, mas só dentro de `data_inicio..data_fim`; geradora ausente da lista/fora da vigência → `None`. Sem geradora: `vinculo_cobre_a_data`, desempate `max(ativa,data_inicio)`. |
| `app/escala/alimentacao.py:257` `vinculo_do_dia_carregado`; `:288-301` plural | Carregam célula/escalas e delegam à anterior; D argumento. |
| `app/escala/servico_jornada.py:15,47-58` `escala_vigente` | Carrega célula+D/geradora, delega `vinculo_do_dia`; D argumento. |
| `app/escala/servico_jornada.py:199-207` `data_saida_esperada` | `escala_vigente(D)`; D=`localtime(ts_entrada).date()`. Depois chama `fim_turno(te,d)` **sem EC**, portanto horários do template. |
| `app/escala/servico_jornada.py:220,233` `_turno_noturno_completo`; `:278,353` `classificar_falta`; `:503,534` `procedencia_fina` | `escala_vigente(data_evento)`; `data_evento` argumento; o primeiro aceita EC já carregado. |
| `app/escala/utils.py:953,964-971` grade legacy; `:1221,1240-1255` grade por turno | Todos vínculos + células → `vinculo_do_dia` para cada D=`ref-i`; `ref` argumento ou hoje. |
| `app/escala/utils.py:2077,2115-2129` `_esc_vigente_do_dia` | Todos vínculos + geradora anotada/célula carregada → juiz; D argumento. |
| `app/escala/utils.py:2132,2153` `intervalo_declarado_do_dia`; `:2165,2204` `minutos_previstos_do_dia` | EC selecionado pelo helper acima; marcos do mesmo D argumento. |
| `app/ponto/turnos.py:772,786` `realizado_do_dia` | `escala_vigente(data)`; D argumento. |
| `app/ponto/turnos.py:1848,1928-1939` `turnos_do_colab` | `vinculo_do_dia` por cada D entre ini/fim; agrupa dias contíguos com mesmo dono. |
| `app/ponto/turnos.py:2073-2080` `dia_encerrado_motivo` | `escala_vigente(data)`; D argumento. |
| `app/ponto/turnos.py:2108,2124-2131` `dias_encerrados` | Todos vínculos + células → juiz por D da janela argumento. |
| `app/ponto/turnos.py:2259,2323-2327` `turnos_abertos_de` | Juiz para `hoje` derivado do instante `agora`; fornece EC ao `_turno_aberto_puro`. |
| `app/ponto/calculador/alimentacao.py:151,179-189` `marcos_por_dia` | Vínculos com início<=fim + juiz por D de `resultado.dia_da_jornada.values()`; EC.marcos_do_dia(D). |
| `app/ponto/calculador/alimentacao.py:341,408-420,479` `rubricas_por_dia` / helper | Vínculos início<=fim + célula de cada D → juiz. |
| `app/escala/services/escala_certa.py:19-31` `motivo_escala_errada` | `escala_vigente(colab_id,dia)`; D argumento. |
| `app/escala/services/previsto_cego.py:78,90;114,162-174` | Todos vínculos alimentam `_esc_vigente_do_dia`/`minutos_previstos_do_dia`, D da janela ini..fim. |
| `app/escala/services/cadastro_realidade.py:477-497` `_vinculos` | Cobertura SQL de hoje + `vinculo_do_dia(cid,hoje,ecs)` **sem células**; escolhe fallback de cobertura. |
| `app/escala/services/semana_do_posto.py:59,73-79` `_colabs_do_posto` | EC.posto==posto e cobre algum dos sete dias seg..dom; enumera membros da semana, não resolve sobreposição por célula. |
| `app/escala/folgas.py:59,74-80` `vinculos_com_calendario_no_mes` | Carregador da janela mensal, restringe ativa+calendário; ano/mês argumento. |
| `app/escala/folgas.py:363,372-383` `vinculo_que_cobre` | Cobertura de D; único candidato vence, ou único ativo dentre vários; caso contrário ambíguo. |
| `app/escala/folgas.py:392,407-414` `reparentar_folga` | FK da folga se cobre `fd.data`; senão `vinculo_do_dia_carregado` para fd.data. |
| `app/escala/services/fase_na_tela.py:125,157-165` `inicio_do_vinculo` | Cobertura de `alvo=dia or hoje`, **mais antigo** `data_inicio`; sem cobertura, menor início de toda linha do tempo. Não consulta geradora. |
| `app/ponto/nucleo.py:7-27` `escalas_no_periodo`/`te_vigente_em` | Carregador da janela, ordena `(ativa,data_inicio)` asc, último que cobre D vence; não consulta geradora. |
| `app/ponto/nucleo.py:30-36,39-57` `dt_fim_previsto_de`/`recompute_turnos` | TE anterior para D=`t.data_turno`; `janela_turno_de(te,D)` **sem EC**, sem DNA do EC. |

Seletores que ignoram vigência do dia:

| arquivo:linha / função | critério; origem de D; efeito possível de futuro |
|---|---|
| `app/escala/utils.py:912,920-927` `data_saida_esperada` | Ativa + fim nulo + início mais recente. D civil de ts_entrada. EC futuro pode vencer imediatamente. |
| `app/escala/views.py:11,17-43` `minha_escala` | `.filter(ativa=True).first()` para todo calendário hoje..+13. Não escolhe EC por cada D. |
| `app/escala/services/impacto_troca.py:22,28-39` | Ativa mais recente para dias `max(data_inicio,hoje-JANELA)..ontem`; futuro pode alterar leitura do passado. |
| `app/escala/services/furos_diarios.py:19,29-36` `apurar_colab` | Ativa mais recente para guarda de régua; dias ini..min(fim,ontem) depois vêm da célula. |
| `app/escala/services/furos_diarios.py:237,280-305` `universo_empresa` | Ativa mais recente para classe de régua/sem escala; dias da competência passada. Posto/praca vêm do Colaborador atual em `:269-270,315-317`. |
| `app/escala/services/meio_periodo.py:120,145-155,174,187` `observado_meio_periodo` | Dict por colaborador sobre todas ativas: última iterada vence; sem ordem explícita. Template por D das batidas/janela ini..hoje. |
| `app/escala/services/fase_12x36.py:86,103-115` `fora_de_fase` | Dict por colaborador de ativas ciclo elegível, última iterada; janela hoje-janela. |
| `app/escala/services/fase_conflitante.py:61,67-80` `medir` | Todas ativas, compara foto e âncora para cada D da janela; não restringe D à vigência do EC. Não é escolha única. |
| `app/escala/services/propositor.py:379,410-427` `propostas` | Todas ativas, cada EC julgado contra janela hoje-janela..ontem; não restringe vigência. Posto exibido é `ec.colaborador.posto` em `:499`. |
| `app/escala/services/cadastro_realidade.py:169,190` `_jornada_que_mente` | Universo ativas + colaborador ativo. Miolo completo não li. |
| `app/escala/services/escala_auto.py:163,172,186` `dry_da_frota` | Ativas + fim nulo; censo/propostas sem vigência de D. Miolo completo não li. |
| `app/escala/management/commands/celulas_contra_o_template.py:25-28` | Dict por colaborador de ativas; última iterada vence, sem filtro de vigência. |
| `app/escala/management/commands/termometro_regua.py:22,35` | Todos EC ativos dos colaboradores selecionados; miolo completo não li. |
| `app/ponto/turnos.py:1536,1556-1559;1563,1590-1592` `_escalas_da_ata`/`papel_por_minuto_da_ata` | Todos EC, ordena ativa/data_inicio, última vence por colaborador; uma TE para toda ata ini..fim. Não filtra vigência. |
| `app/ponto/turnos.py:2414,2447-2455` `proximo_tipo_de` | Ativa `.first()` sem ordem explícita, usa horários crus de fim intervalo/fim turno comparados com agora. |
| `app/ponto/selecao_periodo.py:256,278-291` `turnos_em_turno_stored` | Ativas ordenadas início **ascendente**, `setdefault`: **primeira/mais antiga** vence, apesar comentário dizer mais recente. Usa duração dessa TE no teto entrada.timestamp+jornada+margem; data_turno não seleciona EC. |

Consumidores de EC/TE recebido, não seletores autônomos:

- `app/escala/models.py:326` `TipoEscala.marcos_do_dia(D)` lê template por weekday; `:816` trabalho do tipo; `:905` padrão de feriado TE/posto.
- `app/escala/models.py:1048,1066-1073` EC.eh_dia_trabalho: célula `(colaborador,D).trabalha`, senão cálculo do próprio EC.
- `app/escala/models.py:1075,1094-1108` EC.marcos_do_dia: célula `(colaborador,D).dna.marcos` **sem conferir geradora**; senão template do EC. Assim DNA existente pode proteger horários mesmo quando EC escolhido é futuro; fallback/fase/template permanecem expostos.
- `app/escala/models.py:1110,1141,1183` intervalo/pausas/extra por D; corpos completos não li.
- `app/escala/models.py:1223,1242-1275` cálculo de dia: âncora do EC, foto mensal de FolgaDia; foto vence fase; resto do ramo não li.
- `app/escala/servico_jornada.py:61-89,92-95` marcos_turno/fim_turno: EC.marcos_do_dia(D) se recebido; senão template.
- `app/escala/utils.py:634,687,707,733` montar_grade_prevista: template recebido, mapa marcos por D pode antecedê-lo.
- `app/escala/regua_defesa.py:16,26;118,139;503,524,539` degeneração/diagnóstico/entradas fora do início: TE/EC recebido, D argumento/ref.
- `app/ponto/selecao_periodo.py:43,138-160` janela_turno_de: EC.marcos_do_dia(data_ref) se fornecido, senão template. `:185,223-234` previsto_agora_por_hora: template, weekday/hora argumentos.
- `app/ponto/motor_calculo_v2.py:812-827` `_turnos_do_juiz`: TE recebida ou self.escala; marcos_dna_periodo(colab,ini-2,fim+1).
- `app/ponto/motor_calculo_v2.py:2203,2209,2228-2255` calcular_mes: EC/TE recebidos para todo cálculo, fase exclusivamente EC.data_ancora_colaborador; horários/extra por métodos do EC ou TE.
- `app/ponto/motor_calculo_v2.py:1986-2002` calcular_periodo: método do EC/TE por dia, fallback campos crus; `:2386` folga via EC.eh_dia_trabalho(data_jornada); `:2624-2639` marcos por dia de jornada para múltiplos períodos; `:2949-2964` Zona5 idem.
- Chamadores externos que escolhem EC para o motor ficam no escopo dos demais agentes.

Administração/coleta, sem evidência de seleção de dono do dia:

- `app/escala/views.py:74,81;115,119;151,160-166;229,235;251,257;379,395;443,452`: listas por tipo/ativo, exclusão, FK explícita; não ler como escolha por D.
- `app/escala/views_plano_folgas.py:51,55;98,109;144,176`: grid de ECs e escrita por pk ativo.
- `app/escala/regua_defesa.py:246,259-268`: universo de templates em ECs ativos da empresa, não dono do dia.
- `app/escala/services/meio_periodo.py:67,80-85`: frota por template e dia sintético de weekday.
- `app/escala/services/regeneracao.py:36,52`: todos EC do colaborador para janela.
- `app/escala/services/vinculos_inconsistentes.py:16,20`; `vigencia_sem_trilha.py:50,69`: censo de todos EC.
- `app/escala/services/cadastro_tipo.py:327,334`: propagação em ECs ativos do template.
- `app/escala/services/escala_auto_executor.py:115,134-136`: ECs explícitos identificados pelo histórico.
- Comandos `ajustar_escalas_auto.py:94`, `desfazer_corroboracao_regua.py:26`, `lavrar_foto_folgas.py:164`, `corrigir_vinculo_vigencia.py:51,69,128`: pk explícito ou todos EC para correção.
- `app/escala/signals.py:19,25-27;80,107;143,145-168`: instância/estado anterior por pk, não seletor temporal.

Escrita futura no meu escopo: `app/escala/services/fase_na_tela.py:23-31` calcula teto de âncora como `max(hoje,data_inicio)`, portanto admite a âncora correspondente ao início futuro no helper de tela; a porta que efetivamente salva vínculo/fase está fora deste escopo e não li.

## 29/10 14:38 · sessao df5ef1ecfe7e

**Instalado: `codex-cli 0.165.0`.** Conferi `codex --version`, `codex --help`, `codex exec --help`, `codex features list` e `codex login status`. Login atual: **ChatGPT**. Não li código do Hasner.

### 1. Subagentes nativos

**Sim.** A instalação informa `multi_agent stable true`. A documentação descreve agentes paralelos com contextos separados, resultados devolvidos ao principal e consolidação na resposta final. Os filhos herdam sandbox e aprovação do principal. [Documentação de subagentes](https://learn.chatgpt.com/docs/agent-configuration/subagents).

Acionamento por instrução ao agente, por exemplo:

```bash
codex --enable multi_agent \
  --sandbox read-only --ask-for-approval never \
  -C "$HOME/codex_ws" \
  'Divida a investigação em tarefas independentes. Use até três subagentes explorer em paralelo, espere os resultados e consolide.'
```

Para configurar explicitamente o teto:

```bash
codex --enable multi_agent \
  -c agents.max_threads=3 \
  --sandbox read-only --ask-for-approval never \
  -C "$HOME/codex_ws"
```

`agents.max_threads` é documentado como alias de `agents.max_concurrent_threads_per_session`, excluindo o principal. **Padrão efetivo e aplicação desse teto na 0.165.0: [nao verificado]**; não consegui inicializar uma sessão de teste. [Referência de configuração](https://learn.chatgpt.com/docs/config-file/config-reference). [nao verificado]

`codex agents` abre o navegador de sessões; não é um comando shell para criar um filho — confirmado pelo help.

### 2. `exec`: pergunta de arquivo, resposta em arquivo

Sintaxe conferida no help instalado:

```bash
codex --ask-for-approval never exec \
  --sandbox read-only \
  --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  --output-last-message /tmp/codex-resposta.txt \
  - < /tmp/codex-pergunta.txt
```

**A posição importa nesta versão:** `codex exec -a never` foi rejeitado. A opção funciona no parser global, **antes de `exec`**.

Flags confirmadas:

| Flag | Efeito indicado pelo help |
|---|---|
| `-` | Lê o prompt de stdin. |
| `-C` | Define diretório de trabalho. |
| `--skip-git-repo-check` | Permite diretório sem `.git`, como cópia por archive. |
| `-o`, `--output-last-message` | Grava a última mensagem. |
| `--json` | Emite **eventos JSONL em stdout**. |
| `--output-schema` | Define JSON Schema da resposta final. |
| `--ephemeral` | Evita persistir arquivos da sessão. |

Eventos JSONL separados da resposta final:

```bash
codex --ask-for-approval never exec \
  --sandbox read-only --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  --json \
  -o /tmp/codex-final.txt \
  - < /tmp/codex-pergunta.txt \
  > /tmp/codex-eventos.jsonl
```

Para **resposta final JSON**, acrescente:

```bash
--output-schema /tmp/schema.json -o /tmp/codex-final.json
```

`--json` sozinho não transforma a resposta final em JSON estruturado. Origem: help instalado e [documentação não interativa](https://learn.chatgpt.com/docs/non-interactive-mode). Execução completa com arquivos: **[nao verificado]**.

### 3. Vários `codex exec` simultâneos

Exemplo shell, com saídas distintas:

```bash
codex --ask-for-approval never exec \
  -s read-only --skip-git-repo-check -C "$HOME/codex_ws" \
  -o /tmp/resposta-a.txt - < /tmp/pergunta-a.txt &
pid_a=$!

codex --ask-for-approval never exec \
  -s read-only --skip-git-repo-check -C "$HOME/codex_ws" \
  -o /tmp/resposta-b.txt - < /tmp/pergunta-b.txt &
pid_b=$!

wait "$pid_a"; rc_a=$?
wait "$pid_b"; rc_b=$?
printf 'a=%s b=%s\n' "$rc_a" "$rc_b"
```

**Este teste paralelo não foi executado.** [nao verificado]

- Conflito de sessões/cache na 0.165.0: **[nao verificado]**.
- Limite de processos concorrentes: **[nao verificado]**; não aparece nos helps consultados. Não confundir com teto de subagentes.
- Limites de uso/taxa desta conta ChatGPT e comportamento sob concorrência: **[nao verificado]**.
- O help oferece `--no-daemon`, antes de `exec`, para não usar o servidor compartilhado. Isso não comprova isolamento de todos os caches.

### 4. Outro processo pode chamar e esperar?

**Sim, esse é o uso documentado de `codex exec`: scripts/CI, sem TUI.** O comando em primeiro plano só devolve o controle ao terminar. [Origem: documentação oficial](https://learn.chatgpt.com/docs/non-interactive-mode).

```bash
codex --ask-for-approval never exec \
  --sandbox read-only --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  -o /tmp/resposta.txt - < /tmp/pergunta.txt
rc=$?

if [ "$rc" -eq 0 ]; then
  cat /tmp/resposta.txt
else
  printf 'codex falhou: %s\n' "$rc" >&2
fi
```

- **TTY:** não exigido pelo modo documentado.
- **Login:** `codex login status` confirmou autenticação nesta sessão. Disponibilidade para outro usuário/ambiente: **[nao verificado]**.
- **Confirmações de execução:** `never` manda devolver falhas ao agente, sem pedir aprovação — help instalado.
- Outros bloqueios de inicialização/configuração: não estão eliminados por `never`.

**Teste realmente rodado**, sem TTY e sem ler arquivos: [nao verificado]

```bash
codex --ask-for-approval never --no-daemon exec \
  --ephemeral --skip-git-repo-check \
  --sandbox read-only -C /tmp \
  -o /dev/null \
  'Nao use ferramentas, nao leia arquivos. Responda apenas CLI_OK.'
```

Resultado:

```text
Error: failed to initialize in-process app-server client:
Read-only file system (os error 30)
```

Portanto, confirmei sintaxe e tentativa de inicialização; **não confirmei uma resposta completa do modelo**. O sandbox `read-only` do agente não dispensa armazenamento gravável para o runtime do CLI e para o arquivo de saída.

_103 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
