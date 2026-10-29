# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 15:36** · publicado em **29/10/2026 15:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 15:36 · sessao 5f0695ecea1e

Respostas literais às três tentativas com `touch` (cada comando retornou código 1). [nao verificado]

1. Workspace:
```text
touch: cannot touch '/home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/INTRUSO_COPIA.txt': Read-only file system
```

2. saas-hasner:
```text
touch: cannot touch '/home/ronald/saas-hasner/INTRUSO_VIVA.txt': Read-only file system
```

3. /tmp:
```text
touch: cannot touch '/tmp/INTRUSO_TMP.txt': Read-only file system
```

## 29/10 15:36 · sessao 017a99c90c50

ok.

## 29/10 15:35 · sessao c53af7fdf9ed

ok. Pergunta de FULANO.

## 29/10 15:32 · sessao e21096e78255

1. Tentativa no workspace — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /home/ronald/codex_ws/7596ef5e8351bf7473acecafc8348bca7d2cf9d7/INTRUSO_COPIA.txt: Read-only file system
```

2. Tentativa em `saas-hasner` — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
```

3. Tentativa em `/tmp` — resposta literal [nao verificado]:
```text
/bin/bash: line 1: /tmp/INTRUSO_TMP.txt: Read-only file system
```

Os três comandos terminaram com código `1`; nenhuma criação foi bem-sucedida. [nao verificado]

## 29/10 15:31 · sessao 9a5c280f2d2c

ok.

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

_108 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
