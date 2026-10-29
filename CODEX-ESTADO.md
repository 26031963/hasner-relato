# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 14:42** · publicado em **29/10/2026 14:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 29/10 14:38 · sessao df5ef1ecfe7e

HEAD lido: `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, por `git show`. Sem escrita, testes ou banco. Os dados dos IDs são os fornecidos no laudo; seus registros e trilhas: **nao li**.

## T16 — Data da pergunta × data da célula

### (a) Onde nasce `data_evento`

| Caminho | Origem da data |
|---|---|
| Fábrica por célula | Passa **`cel.data`** e coloca esse dia no contexto do motivo (`app/chamados/services/fabrica_da_celula.py:123`, `:108`). |
| Gerador de perguntas | Usa a data do contexto e entrega ao gate como `data_evento` (`app/chamados/services/disputa_emissao.py:343`, `:347`, `:396`). |
| Contexto sem data | `_data_furo_da_grade` toma `entrada_sem_saida`/`ultima_batida`, converte para **dia civil local** e verifica a célula desse dia; não pergunta `Turno.data_turno` (`app/chamados/services/disputa_emissao.py:57`, `:61`, `:66`, `:68`). |
| Último fallback de anomalia | Usa o dia civil local de `ultima_batida` (`app/chamados/services/disputa_emissao.py:350`, `:364`). |
| Marcos nomeados do chamado | Usa o dia declarado no contexto e consulta a ata desse dia (`app/chamados/services/disputa_emissao.py:973`, `:978`). |
| Agrupamento por batida-origem | Começa pelo dia civil da entrada; depois resolve a data do evento pelo motivo (`app/chamados/services/disputa_emissao.py:1002`, `:1003`, `:1006`). |
| Motivo `orfao_14h` | Usa a **data da saída esperada**, que pode ser D+1 num noturno (`app/escala/servico_jornada.py:482`, `:487`, `:490`). |
| Sem batida-origem | Usa a data do contexto do chamado (`app/chamados/services/disputa_emissao.py:1031`). |
| Conversa sem data | O gate atribui **hoje** (`app/chamados/models.py:2054`, `:2057`). |
| Associação da pergunta à célula | Busca a célula pela **própria `data_evento`**, não copia a célula do chamado (`app/chamados/models.py:2060`). |

A costura só preenche data quando ela está vazia; não corrige data já existente (`app/chamados/services/fio.py:788`, `:790`). O juiz admite D/D+1 quando reconhece turno noturno **ou quando não consegue determinar se cruza a meia-noite** (`app/chamados/juizes.py:1712`, `:1721`, `:1730`, `:1732`).

**Limite para o caso:** com entrada de28/10 e DNA realmente diurno06–18, `resolver_data_evento` não produz29/10 sozinho: o fim encadeado permanece em28/10 (`app/escala/utils.py:925`, `:927`, `:930`). Para explicar a pergunta de29/10 é necessário outro contexto/ramo ou outro DNA lido naquele ato. Isso é **hipótese**; não está demonstrado apenas pelo casamento de18:01 com S18.

### (b) Qual vínculo/DNA casa18:01

| Etapa | Regra encontrada |
|---|---|
| Escolha do vínculo | A grade carrega vínculos/células e chama `vinculo_do_dia` (`app/escala/utils.py:1240`, `:1252`, `:1254`). |
| Geradora declarada | Vale apenas dentro da vigência. Se estiver fora, retorna `None`, sem trocar silenciosamente por outro vínculo (`app/escala/alimentacao.py:237`, `:247`, `:250`). Sem geradora, escolhe vínculo que cobre o dia (`:253`, `:254`). |
| Marcos usados | Primeiro DNA da célula do dia; na ausência, template (`app/escala/utils.py:104`, `:1547`). |
| Matcher | Recebe marcos prontos; não escolhe vínculo nem data (`app/escala/utils.py:551`). |
| Uso do papel do turno | Exige exatamente um turno fechado; neste chamador, também intervalo declarado (`app/escala/utils.py:1533`, `:1534`, `:1641`, `:1642`). |
| Caminho restante | DP por distância, sem exigir que tipo gravado coincida com tipo do marco (`app/escala/utils.py:595`, `:599`); conserva `tipo_real` separadamente (`:627`). |

Assim, **uma entrada isolada18:01 pode ocupar S18** diante de marcos06/18: o turno aberto não fornece papel, e a DP escolhe o marco próximo (`app/escala/utils.py:1534`, `:595`, `:599`). Isso demonstra atribuição posicional, não demonstra erro de data civil.

Pelas vigências informadas, #1482 encerrado em13/10 não cobre28/10; se a célula ainda o nomeia como geradora, o juiz retorna `None` (`app/escala/alimentacao.py:250`). Geradora e DNA efetivos da célula28/10: **nao li**.

Há uma alimentação diferente no datador da saída: ele busca vínculo **ativo e sem fim**, em vez da composição histórica do builder, e depois lê os marcos do dia (`app/escala/utils.py:920`, `:922`, `:927`). Que essa diferença tenha causado p#51310: **hipótese**.

### (c) Resolução sem resposta humana

| Via / escritor | Testemunha consultada |
|---|---|
| `sem_turno_aberto` (`app/chamados/reconciliador.py:402`, `:406`) | Exige ausência de resposta, ausência de turno aberto e pergunta já não viva (`:365`, `:372`, `:398`). Não basta faltar turno aberto. |
| `celula_absolveu` (`app/chamados/reconciliador.py:548`, `:550`) | Célula não acusadora, sem pendências de teto, pergunta não viva (`:525`, `:529`, `:538`, `:544`). |
| `fantasma` (`app/chamados/models.py:2170`, `:2171`) | Sem resposta, lâmpada do próprio marco acesa e dia encerrado; consumidor grava pela porta (`app/chamados/reconciliador.py:878`, `:880`, `:884`, `:886`, `:940`). |
| `juiz_aprovou` (`app/chamados/services/materializacao.py:1337`, `:1341`) | Usa `_pergunta_materializada_de_fato`; corpo integral desse juiz neste HEAD: **nao li**. |
| `completude_turno` | `via_pelo_fato` consulta `TurnoMaterializado.saida` pela `data_evento` (`app/chamados/juizes.py:1203`, `:1207`, `:1209`). |
| `ausencia_dia` | O mesmo juiz usa ausência aprovada no dia da pergunta (`app/chamados/juizes.py:1199`, `:1201`). |

Quando consulta a lâmpada, o juiz lê **`p.celula`**, seu DNA e ATA — não necessariamente a célula do chamado (`app/chamados/juizes.py:842`, `:845`, `:849`, `:866`). Via concreta de p#51310: **nao li**.

**Não são uma única classe comprovada de “dia pelo dia civil”.** Há mistura de semânticas de data em(a), escolha de vínculo/DNA e atribuição posicional em(b), e resolução por testemunha em(c). A cadeia causal específica entre as três continua **hipótese**, pelas diferenças demonstradas acima.

## T17 — Chamado em cobrança sem pergunta viva

**`COBRANCA` depende somente de `status_local` ser `aberto` ou `em_analise`; não exige fio vivo** (`app/chamados/catalogo/motor.py:29`). Fio morto não provoca transição automática: alguém precisa chamar uma porta aceita pelo motor e pela premissa (`app/chamados/models.py:440`, `:467`, `:470`).

| Caminho | O que pode impedir fechamento/reparo |
|---|---|
| `reconciliar()` | Para furo, exige premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:495`, `:498`). **Hipótese:** célula ainda furada, sem essas exceções, conserva cobrança mesmo sem fio (`:522`, `:525`). |
| Reconciliador geral | Só retrata quando encontra premissa conhecida e morta (`app/chamados/reconciliador.py:1203`, `:1208`, `:1209`). `_retratar` ainda pode recusar por irmã respondida sem consumo ou pelo gate final (`:435`, `:456`). |
| `consumir_eventos_celula` | Executa retratação de absolvidos e encerramento de órfãs pela lâmpada (`app/chamados/reconciliador.py:1145`, `:1148`). Sem falha/adiamento, marca processado mesmo sem alteração (`:1165`). **Processado não significa fio reparado.** |
| Retratar absolvidos | Exige célula com veredito **não acusador** e sem pendência de teto (`app/chamados/reconciliador.py:819`, `:823`, `:825`). Célula acusadora fica fora. |
| Encerrar órfãs pela lâmpada | Seleciona perguntas não materializadas/não validadas, em disputa aberta (`app/chamados/reconciliador.py:836`, `:844`); exige pergunta **viva**, sem texto e marco aceso (`:878`, `:880`, `:882`). Não ressuscita pergunta morta. |
| `ressuscitar_perguntas` | Só alcança resolvedoras associadas, materializadas, sem texto, cujo marco não esteja comprovadamente aceso (`app/chamados/models.py:1112`, `:1119`). Pergunta ausente, validada sem materialização, com texto ou ligada a outro chamado fica fora. Limpeza em `:1162`. |
| Pedido de ressuscitação pelo cartório | Pula chamado sem perguntas, com fio vivo ou com qualquer resposta textual (`app/ponto/services/cartorio.py:1261`, `:1263`). Duplicatas tentam fechamento; demais seguem para ressuscitação (`:1265`, `:1271`). Impressão igual impede executar esse corpo (`:1092`). |
| `reabrir()` | Só há transição de resolvido/fechado; não é reparo para chamado já aberto (`app/chamados/catalogo/motor.py:113`). Quando consegue reabrir, pede ressuscitação (`app/chamados/models.py:1098`). |
| Fábrica | Exige célula elegível e motivo vencido (`app/chamados/services/fabrica_da_celula.py:73`, `:80`, `:81`). Dedup considera pergunta morta não duplicata como existente (`:93`, `:95`); o gate reutiliza o fato sem limpar carimbos (`app/chamados/models.py:2061`, `:2084`). |
| Gatilho da fábrica | Só alcança células efetivamente julgadas, não a mera ausência de fio (`app/ponto/services/cartorio.py:769`, `:770`). |
| Evento de beco | Exige pai já resolvido; pai aberto não entra (`app/chamados/services/becos.py:31`). |

Nascimento e mudança de veredito/ata geram eventos, mas somente eventos pendentes alimentam o consumidor (`app/chamados/models.py:736`, `:739`; `app/ponto/portas/celula.py:623`, `:642`; `app/chamados/reconciliador.py:1128`). Isso não comprova reparo do estoque anterior nem superação das guardas acima.

**O261 conta #22914? “Aberto sem pergunta viva” não basta:**

- Precisa módulo de furo e célula associada (`app/chamados/services/vigia_fio.py:184`, `:199`).
- A célula precisa ser acusadora (`:227`).
- O vigia reúne perguntas da **mesma célula**, não só resolvedoras daquele chamado. Outra pergunta viva da célula pode excluí-lo (`:242`, `:244`, `:258`).
- Exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`:261`, `:266`, `:273`, `:279`).
- Passando tudo, soma uma célula em `fio_morto` e, com cobrança, em `fio_morto_cobrando` (`:286`, `:288`).

**Por que nenhum caminho alcançou especificamente #22914: não provado.** Módulo, célula, data, veredito, carimbos das perguntas e estado dos eventos desse ID: **nao li**. Escolher uma das condições acima como causa seria **hipótese**.

## T18 — Retrato × `o_que_segura`

| Fonte | O que efetivamente lê |
|---|---|
| Ficha/drawer | Busca `RetratoFolha` da empresa/competência e localiza a linha do colaborador (`app/colaboradores/views.py:980`, `:982`). O localizador lê **`RetratoFolha.linhas`** (`app/folha/retrato_folha.py:110`, `:935`). |
| Pendências exibidas | Template mostra `retrato_fazer` e **`retrato_linha.datas`**, já gravadas (`app/templates/folha/_retrato_o_que_fazer.html:33`, `:44`, `:46`). |
| Produção da foto | `montar_linhas` chama `fora_da_folha(..., com_itens=True)` (`app/folha/retrato_folha.py:1157`), que usa `classificar_export` e `motivos_retencao_celula` (`app/folha/fora_da_folha.py:120`, `:93`, `:139`). Grava status, motivo, dias e datas (`app/folha/retrato_folha.py:1180`, `:1183`). |
| `o_que_segura` | Consulta **agora** `classificar_export` e a mesma autoridade de dias retidos; devolve o status em `entra` (`app/folha/fora_da_folha.py:191`, `:193`, `:212`). |

**Não encontrei duas regras de retenção nesse caminho.** As duas leituras descendem das mesmas autoridades; a ficha lê a resposta gravada e `o_que_segura` pede resposta atual (`app/folha/retrato_folha.py:1157`; `app/folha/fora_da_folha.py:191`, `:193`).

Há duas distinções relevantes:

1. **“Folga” pode ser rótulo de causa**, não classificação atual de cada data. `tipo='folga'` pode vir de causas como `escala_sem_folga_definida` ou `dias_trocados`, lidas de `FechamentoMensal.causa_espelho` (`app/folha/retrato_folha.py:233`, `:238`, `:374`, `:1115`).
2. O juiz atual de retenção usa veredito/protestos da célula; não possui nesse ramo veto independente por `trabalha=False`. Portanto uma folga ainda acusadora pode reter; uma célula absolvida deixa de reter por esse critério (`app/folha/export.py:1188`, `:1215`, `:1227`, `:1235`).

**O frescor pode não perceber mudança da célula:** compara somente `Max(FechamentoMensal.atualizado_em)` com `RetratoFolha.lavrado_em` (`app/folha/retrato_folha.py:1298`, `:1306`). Veredito/ata são gravados pela porta de célula separadamente (`app/ponto/portas/celula.py:691`, `:697`).

**Hipótese de defasagem:** célula deixa de reter depois da lavra, retrato não é relavrado e nenhum fechamento recebe timestamp posterior. A ficha conserva `linhas.datas`; `o_que_segura` responde `entra`; o indicador ainda pode dizer atualizado (`app/folha/retrato_folha.py:1183`, `:1303`; `app/folha/fora_da_folha.py:212`).

**Conclusão:** o código sustenta **projeção velha como hipótese**, não uma derivação paralela de folga na ficha. Para confirmar o ocorrido, a linha gravada, `lavrado_em`, timestamps dos fechamentos e histórico das células: **nao li**.

## 29/10 14:38 · sessao df5ef1ecfe7e

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 14:38 · sessao df5ef1ecfe7e

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 14:38 · sessao df5ef1ecfe7e

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 14:38 · sessao df5ef1ecfe7e

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 14:38 · sessao df5ef1ecfe7e

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 14:38 · sessao c03300972490

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

## 29/10 14:38 · sessao c03300972490

HEAD lido: `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, por `git show`. Sem escrita, testes ou banco. Os dados dos IDs são os fornecidos no laudo; seus registros e trilhas: **nao li**.

## T16 — Data da pergunta × data da célula

### (a) Onde nasce `data_evento`

| Caminho | Origem da data |
|---|---|
| Fábrica por célula | Passa **`cel.data`** e coloca esse dia no contexto do motivo (`app/chamados/services/fabrica_da_celula.py:123`, `:108`). |
| Gerador de perguntas | Usa a data do contexto e entrega ao gate como `data_evento` (`app/chamados/services/disputa_emissao.py:343`, `:347`, `:396`). |
| Contexto sem data | `_data_furo_da_grade` toma `entrada_sem_saida`/`ultima_batida`, converte para **dia civil local** e verifica a célula desse dia; não pergunta `Turno.data_turno` (`app/chamados/services/disputa_emissao.py:57`, `:61`, `:66`, `:68`). |
| Último fallback de anomalia | Usa o dia civil local de `ultima_batida` (`app/chamados/services/disputa_emissao.py:350`, `:364`). |
| Marcos nomeados do chamado | Usa o dia declarado no contexto e consulta a ata desse dia (`app/chamados/services/disputa_emissao.py:973`, `:978`). |
| Agrupamento por batida-origem | Começa pelo dia civil da entrada; depois resolve a data do evento pelo motivo (`app/chamados/services/disputa_emissao.py:1002`, `:1003`, `:1006`). |
| Motivo `orfao_14h` | Usa a **data da saída esperada**, que pode ser D+1 num noturno (`app/escala/servico_jornada.py:482`, `:487`, `:490`). |
| Sem batida-origem | Usa a data do contexto do chamado (`app/chamados/services/disputa_emissao.py:1031`). |
| Conversa sem data | O gate atribui **hoje** (`app/chamados/models.py:2054`, `:2057`). |
| Associação da pergunta à célula | Busca a célula pela **própria `data_evento`**, não copia a célula do chamado (`app/chamados/models.py:2060`). |

A costura só preenche data quando ela está vazia; não corrige data já existente (`app/chamados/services/fio.py:788`, `:790`). O juiz admite D/D+1 quando reconhece turno noturno **ou quando não consegue determinar se cruza a meia-noite** (`app/chamados/juizes.py:1712`, `:1721`, `:1730`, `:1732`).

**Limite para o caso:** com entrada de28/10 e DNA realmente diurno06–18, `resolver_data_evento` não produz29/10 sozinho: o fim encadeado permanece em28/10 (`app/escala/utils.py:925`, `:927`, `:930`). Para explicar a pergunta de29/10 é necessário outro contexto/ramo ou outro DNA lido naquele ato. Isso é **hipótese**; não está demonstrado apenas pelo casamento de18:01 com S18.

### (b) Qual vínculo/DNA casa18:01

| Etapa | Regra encontrada |
|---|---|
| Escolha do vínculo | A grade carrega vínculos/células e chama `vinculo_do_dia` (`app/escala/utils.py:1240`, `:1252`, `:1254`). |
| Geradora declarada | Vale apenas dentro da vigência. Se estiver fora, retorna `None`, sem trocar silenciosamente por outro vínculo (`app/escala/alimentacao.py:237`, `:247`, `:250`). Sem geradora, escolhe vínculo que cobre o dia (`:253`, `:254`). |
| Marcos usados | Primeiro DNA da célula do dia; na ausência, template (`app/escala/utils.py:104`, `:1547`). |
| Matcher | Recebe marcos prontos; não escolhe vínculo nem data (`app/escala/utils.py:551`). |
| Uso do papel do turno | Exige exatamente um turno fechado; neste chamador, também intervalo declarado (`app/escala/utils.py:1533`, `:1534`, `:1641`, `:1642`). |
| Caminho restante | DP por distância, sem exigir que tipo gravado coincida com tipo do marco (`app/escala/utils.py:595`, `:599`); conserva `tipo_real` separadamente (`:627`). |

Assim, **uma entrada isolada18:01 pode ocupar S18** diante de marcos06/18: o turno aberto não fornece papel, e a DP escolhe o marco próximo (`app/escala/utils.py:1534`, `:595`, `:599`). Isso demonstra atribuição posicional, não demonstra erro de data civil.

Pelas vigências informadas, #1482 encerrado em13/10 não cobre28/10; se a célula ainda o nomeia como geradora, o juiz retorna `None` (`app/escala/alimentacao.py:250`). Geradora e DNA efetivos da célula28/10: **nao li**.

Há uma alimentação diferente no datador da saída: ele busca vínculo **ativo e sem fim**, em vez da composição histórica do builder, e depois lê os marcos do dia (`app/escala/utils.py:920`, `:922`, `:927`). Que essa diferença tenha causado p#51310: **hipótese**.

### (c) Resolução sem resposta humana

| Via / escritor | Testemunha consultada |
|---|---|
| `sem_turno_aberto` (`app/chamados/reconciliador.py:402`, `:406`) | Exige ausência de resposta, ausência de turno aberto e pergunta já não viva (`:365`, `:372`, `:398`). Não basta faltar turno aberto. |
| `celula_absolveu` (`app/chamados/reconciliador.py:548`, `:550`) | Célula não acusadora, sem pendências de teto, pergunta não viva (`:525`, `:529`, `:538`, `:544`). |
| `fantasma` (`app/chamados/models.py:2170`, `:2171`) | Sem resposta, lâmpada do próprio marco acesa e dia encerrado; consumidor grava pela porta (`app/chamados/reconciliador.py:878`, `:880`, `:884`, `:886`, `:940`). |
| `juiz_aprovou` (`app/chamados/services/materializacao.py:1337`, `:1341`) | Usa `_pergunta_materializada_de_fato`; corpo integral desse juiz neste HEAD: **nao li**. |
| `completude_turno` | `via_pelo_fato` consulta `TurnoMaterializado.saida` pela `data_evento` (`app/chamados/juizes.py:1203`, `:1207`, `:1209`). |
| `ausencia_dia` | O mesmo juiz usa ausência aprovada no dia da pergunta (`app/chamados/juizes.py:1199`, `:1201`). |

Quando consulta a lâmpada, o juiz lê **`p.celula`**, seu DNA e ATA — não necessariamente a célula do chamado (`app/chamados/juizes.py:842`, `:845`, `:849`, `:866`). Via concreta de p#51310: **nao li**.

**Não são uma única classe comprovada de “dia pelo dia civil”.** Há mistura de semânticas de data em(a), escolha de vínculo/DNA e atribuição posicional em(b), e resolução por testemunha em(c). A cadeia causal específica entre as três continua **hipótese**, pelas diferenças demonstradas acima.

## T17 — Chamado em cobrança sem pergunta viva

**`COBRANCA` depende somente de `status_local` ser `aberto` ou `em_analise`; não exige fio vivo** (`app/chamados/catalogo/motor.py:29`). Fio morto não provoca transição automática: alguém precisa chamar uma porta aceita pelo motor e pela premissa (`app/chamados/models.py:440`, `:467`, `:470`).

| Caminho | O que pode impedir fechamento/reparo |
|---|---|
| `reconciliar()` | Para furo, exige premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:495`, `:498`). **Hipótese:** célula ainda furada, sem essas exceções, conserva cobrança mesmo sem fio (`:522`, `:525`). |
| Reconciliador geral | Só retrata quando encontra premissa conhecida e morta (`app/chamados/reconciliador.py:1203`, `:1208`, `:1209`). `_retratar` ainda pode recusar por irmã respondida sem consumo ou pelo gate final (`:435`, `:456`). |
| `consumir_eventos_celula` | Executa retratação de absolvidos e encerramento de órfãs pela lâmpada (`app/chamados/reconciliador.py:1145`, `:1148`). Sem falha/adiamento, marca processado mesmo sem alteração (`:1165`). **Processado não significa fio reparado.** |
| Retratar absolvidos | Exige célula com veredito **não acusador** e sem pendência de teto (`app/chamados/reconciliador.py:819`, `:823`, `:825`). Célula acusadora fica fora. |
| Encerrar órfãs pela lâmpada | Seleciona perguntas não materializadas/não validadas, em disputa aberta (`app/chamados/reconciliador.py:836`, `:844`); exige pergunta **viva**, sem texto e marco aceso (`:878`, `:880`, `:882`). Não ressuscita pergunta morta. |
| `ressuscitar_perguntas` | Só alcança resolvedoras associadas, materializadas, sem texto, cujo marco não esteja comprovadamente aceso (`app/chamados/models.py:1112`, `:1119`). Pergunta ausente, validada sem materialização, com texto ou ligada a outro chamado fica fora. Limpeza em `:1162`. |
| Pedido de ressuscitação pelo cartório | Pula chamado sem perguntas, com fio vivo ou com qualquer resposta textual (`app/ponto/services/cartorio.py:1261`, `:1263`). Duplicatas tentam fechamento; demais seguem para ressuscitação (`:1265`, `:1271`). Impressão igual impede executar esse corpo (`:1092`). |
| `reabrir()` | Só há transição de resolvido/fechado; não é reparo para chamado já aberto (`app/chamados/catalogo/motor.py:113`). Quando consegue reabrir, pede ressuscitação (`app/chamados/models.py:1098`). |
| Fábrica | Exige célula elegível e motivo vencido (`app/chamados/services/fabrica_da_celula.py:73`, `:80`, `:81`). Dedup considera pergunta morta não duplicata como existente (`:93`, `:95`); o gate reutiliza o fato sem limpar carimbos (`app/chamados/models.py:2061`, `:2084`). |
| Gatilho da fábrica | Só alcança células efetivamente julgadas, não a mera ausência de fio (`app/ponto/services/cartorio.py:769`, `:770`). |
| Evento de beco | Exige pai já resolvido; pai aberto não entra (`app/chamados/services/becos.py:31`). |

Nascimento e mudança de veredito/ata geram eventos, mas somente eventos pendentes alimentam o consumidor (`app/chamados/models.py:736`, `:739`; `app/ponto/portas/celula.py:623`, `:642`; `app/chamados/reconciliador.py:1128`). Isso não comprova reparo do estoque anterior nem superação das guardas acima.

**O261 conta #22914? “Aberto sem pergunta viva” não basta:**

- Precisa módulo de furo e célula associada (`app/chamados/services/vigia_fio.py:184`, `:199`).
- A célula precisa ser acusadora (`:227`).
- O vigia reúne perguntas da **mesma célula**, não só resolvedoras daquele chamado. Outra pergunta viva da célula pode excluí-lo (`:242`, `:244`, `:258`).
- Exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`:261`, `:266`, `:273`, `:279`).
- Passando tudo, soma uma célula em `fio_morto` e, com cobrança, em `fio_morto_cobrando` (`:286`, `:288`).

**Por que nenhum caminho alcançou especificamente #22914: não provado.** Módulo, célula, data, veredito, carimbos das perguntas e estado dos eventos desse ID: **nao li**. Escolher uma das condições acima como causa seria **hipótese**.

## T18 — Retrato × `o_que_segura`

| Fonte | O que efetivamente lê |
|---|---|
| Ficha/drawer | Busca `RetratoFolha` da empresa/competência e localiza a linha do colaborador (`app/colaboradores/views.py:980`, `:982`). O localizador lê **`RetratoFolha.linhas`** (`app/folha/retrato_folha.py:110`, `:935`). |
| Pendências exibidas | Template mostra `retrato_fazer` e **`retrato_linha.datas`**, já gravadas (`app/templates/folha/_retrato_o_que_fazer.html:33`, `:44`, `:46`). |
| Produção da foto | `montar_linhas` chama `fora_da_folha(..., com_itens=True)` (`app/folha/retrato_folha.py:1157`), que usa `classificar_export` e `motivos_retencao_celula` (`app/folha/fora_da_folha.py:120`, `:93`, `:139`). Grava status, motivo, dias e datas (`app/folha/retrato_folha.py:1180`, `:1183`). |
| `o_que_segura` | Consulta **agora** `classificar_export` e a mesma autoridade de dias retidos; devolve o status em `entra` (`app/folha/fora_da_folha.py:191`, `:193`, `:212`). |

**Não encontrei duas regras de retenção nesse caminho.** As duas leituras descendem das mesmas autoridades; a ficha lê a resposta gravada e `o_que_segura` pede resposta atual (`app/folha/retrato_folha.py:1157`; `app/folha/fora_da_folha.py:191`, `:193`).

Há duas distinções relevantes:

1. **“Folga” pode ser rótulo de causa**, não classificação atual de cada data. `tipo='folga'` pode vir de causas como `escala_sem_folga_definida` ou `dias_trocados`, lidas de `FechamentoMensal.causa_espelho` (`app/folha/retrato_folha.py:233`, `:238`, `:374`, `:1115`).
2. O juiz atual de retenção usa veredito/protestos da célula; não possui nesse ramo veto independente por `trabalha=False`. Portanto uma folga ainda acusadora pode reter; uma célula absolvida deixa de reter por esse critério (`app/folha/export.py:1188`, `:1215`, `:1227`, `:1235`).

**O frescor pode não perceber mudança da célula:** compara somente `Max(FechamentoMensal.atualizado_em)` com `RetratoFolha.lavrado_em` (`app/folha/retrato_folha.py:1298`, `:1306`). Veredito/ata são gravados pela porta de célula separadamente (`app/ponto/portas/celula.py:691`, `:697`).

**Hipótese de defasagem:** célula deixa de reter depois da lavra, retrato não é relavrado e nenhum fechamento recebe timestamp posterior. A ficha conserva `linhas.datas`; `o_que_segura` responde `entra`; o indicador ainda pode dizer atualizado (`app/folha/retrato_folha.py:1183`, `:1303`; `app/folha/fora_da_folha.py:212`).

**Conclusão:** o código sustenta **projeção velha como hipótese**, não uma derivação paralela de folga na ficha. Para confirmar o ocorrido, a linha gravada, `lavrado_em`, timestamps dos fechamentos e histórico das células: **nao li**.

## 29/10 14:38 · sessao c03300972490

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 14:38 · sessao c03300972490

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 14:38 · sessao c03300972490

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 14:38 · sessao c03300972490

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 14:38 · sessao c03300972490

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 14:38 · sessao 41a4d35fbf65

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

## 29/10 14:38 · sessao 41a4d35fbf65

HEAD lido: `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, por `git show`. Sem escrita, testes ou banco. Os dados dos IDs são os fornecidos no laudo; seus registros e trilhas: **nao li**.

## T16 — Data da pergunta × data da célula

### (a) Onde nasce `data_evento`

| Caminho | Origem da data |
|---|---|
| Fábrica por célula | Passa **`cel.data`** e coloca esse dia no contexto do motivo (`app/chamados/services/fabrica_da_celula.py:123`, `:108`). |
| Gerador de perguntas | Usa a data do contexto e entrega ao gate como `data_evento` (`app/chamados/services/disputa_emissao.py:343`, `:347`, `:396`). |
| Contexto sem data | `_data_furo_da_grade` toma `entrada_sem_saida`/`ultima_batida`, converte para **dia civil local** e verifica a célula desse dia; não pergunta `Turno.data_turno` (`app/chamados/services/disputa_emissao.py:57`, `:61`, `:66`, `:68`). |
| Último fallback de anomalia | Usa o dia civil local de `ultima_batida` (`app/chamados/services/disputa_emissao.py:350`, `:364`). |
| Marcos nomeados do chamado | Usa o dia declarado no contexto e consulta a ata desse dia (`app/chamados/services/disputa_emissao.py:973`, `:978`). |
| Agrupamento por batida-origem | Começa pelo dia civil da entrada; depois resolve a data do evento pelo motivo (`app/chamados/services/disputa_emissao.py:1002`, `:1003`, `:1006`). |
| Motivo `orfao_14h` | Usa a **data da saída esperada**, que pode ser D+1 num noturno (`app/escala/servico_jornada.py:482`, `:487`, `:490`). |
| Sem batida-origem | Usa a data do contexto do chamado (`app/chamados/services/disputa_emissao.py:1031`). |
| Conversa sem data | O gate atribui **hoje** (`app/chamados/models.py:2054`, `:2057`). |
| Associação da pergunta à célula | Busca a célula pela **própria `data_evento`**, não copia a célula do chamado (`app/chamados/models.py:2060`). |

A costura só preenche data quando ela está vazia; não corrige data já existente (`app/chamados/services/fio.py:788`, `:790`). O juiz admite D/D+1 quando reconhece turno noturno **ou quando não consegue determinar se cruza a meia-noite** (`app/chamados/juizes.py:1712`, `:1721`, `:1730`, `:1732`).

**Limite para o caso:** com entrada de28/10 e DNA realmente diurno06–18, `resolver_data_evento` não produz29/10 sozinho: o fim encadeado permanece em28/10 (`app/escala/utils.py:925`, `:927`, `:930`). Para explicar a pergunta de29/10 é necessário outro contexto/ramo ou outro DNA lido naquele ato. Isso é **hipótese**; não está demonstrado apenas pelo casamento de18:01 com S18.

### (b) Qual vínculo/DNA casa18:01

| Etapa | Regra encontrada |
|---|---|
| Escolha do vínculo | A grade carrega vínculos/células e chama `vinculo_do_dia` (`app/escala/utils.py:1240`, `:1252`, `:1254`). |
| Geradora declarada | Vale apenas dentro da vigência. Se estiver fora, retorna `None`, sem trocar silenciosamente por outro vínculo (`app/escala/alimentacao.py:237`, `:247`, `:250`). Sem geradora, escolhe vínculo que cobre o dia (`:253`, `:254`). |
| Marcos usados | Primeiro DNA da célula do dia; na ausência, template (`app/escala/utils.py:104`, `:1547`). |
| Matcher | Recebe marcos prontos; não escolhe vínculo nem data (`app/escala/utils.py:551`). |
| Uso do papel do turno | Exige exatamente um turno fechado; neste chamador, também intervalo declarado (`app/escala/utils.py:1533`, `:1534`, `:1641`, `:1642`). |
| Caminho restante | DP por distância, sem exigir que tipo gravado coincida com tipo do marco (`app/escala/utils.py:595`, `:599`); conserva `tipo_real` separadamente (`:627`). |

Assim, **uma entrada isolada18:01 pode ocupar S18** diante de marcos06/18: o turno aberto não fornece papel, e a DP escolhe o marco próximo (`app/escala/utils.py:1534`, `:595`, `:599`). Isso demonstra atribuição posicional, não demonstra erro de data civil.

Pelas vigências informadas, #1482 encerrado em13/10 não cobre28/10; se a célula ainda o nomeia como geradora, o juiz retorna `None` (`app/escala/alimentacao.py:250`). Geradora e DNA efetivos da célula28/10: **nao li**.

Há uma alimentação diferente no datador da saída: ele busca vínculo **ativo e sem fim**, em vez da composição histórica do builder, e depois lê os marcos do dia (`app/escala/utils.py:920`, `:922`, `:927`). Que essa diferença tenha causado p#51310: **hipótese**.

### (c) Resolução sem resposta humana

| Via / escritor | Testemunha consultada |
|---|---|
| `sem_turno_aberto` (`app/chamados/reconciliador.py:402`, `:406`) | Exige ausência de resposta, ausência de turno aberto e pergunta já não viva (`:365`, `:372`, `:398`). Não basta faltar turno aberto. |
| `celula_absolveu` (`app/chamados/reconciliador.py:548`, `:550`) | Célula não acusadora, sem pendências de teto, pergunta não viva (`:525`, `:529`, `:538`, `:544`). |
| `fantasma` (`app/chamados/models.py:2170`, `:2171`) | Sem resposta, lâmpada do próprio marco acesa e dia encerrado; consumidor grava pela porta (`app/chamados/reconciliador.py:878`, `:880`, `:884`, `:886`, `:940`). |
| `juiz_aprovou` (`app/chamados/services/materializacao.py:1337`, `:1341`) | Usa `_pergunta_materializada_de_fato`; corpo integral desse juiz neste HEAD: **nao li**. |
| `completude_turno` | `via_pelo_fato` consulta `TurnoMaterializado.saida` pela `data_evento` (`app/chamados/juizes.py:1203`, `:1207`, `:1209`). |
| `ausencia_dia` | O mesmo juiz usa ausência aprovada no dia da pergunta (`app/chamados/juizes.py:1199`, `:1201`). |

Quando consulta a lâmpada, o juiz lê **`p.celula`**, seu DNA e ATA — não necessariamente a célula do chamado (`app/chamados/juizes.py:842`, `:845`, `:849`, `:866`). Via concreta de p#51310: **nao li**.

**Não são uma única classe comprovada de “dia pelo dia civil”.** Há mistura de semânticas de data em(a), escolha de vínculo/DNA e atribuição posicional em(b), e resolução por testemunha em(c). A cadeia causal específica entre as três continua **hipótese**, pelas diferenças demonstradas acima.

## T17 — Chamado em cobrança sem pergunta viva

**`COBRANCA` depende somente de `status_local` ser `aberto` ou `em_analise`; não exige fio vivo** (`app/chamados/catalogo/motor.py:29`). Fio morto não provoca transição automática: alguém precisa chamar uma porta aceita pelo motor e pela premissa (`app/chamados/models.py:440`, `:467`, `:470`).

| Caminho | O que pode impedir fechamento/reparo |
|---|---|
| `reconciliar()` | Para furo, exige premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:495`, `:498`). **Hipótese:** célula ainda furada, sem essas exceções, conserva cobrança mesmo sem fio (`:522`, `:525`). |
| Reconciliador geral | Só retrata quando encontra premissa conhecida e morta (`app/chamados/reconciliador.py:1203`, `:1208`, `:1209`). `_retratar` ainda pode recusar por irmã respondida sem consumo ou pelo gate final (`:435`, `:456`). |
| `consumir_eventos_celula` | Executa retratação de absolvidos e encerramento de órfãs pela lâmpada (`app/chamados/reconciliador.py:1145`, `:1148`). Sem falha/adiamento, marca processado mesmo sem alteração (`:1165`). **Processado não significa fio reparado.** |
| Retratar absolvidos | Exige célula com veredito **não acusador** e sem pendência de teto (`app/chamados/reconciliador.py:819`, `:823`, `:825`). Célula acusadora fica fora. |
| Encerrar órfãs pela lâmpada | Seleciona perguntas não materializadas/não validadas, em disputa aberta (`app/chamados/reconciliador.py:836`, `:844`); exige pergunta **viva**, sem texto e marco aceso (`:878`, `:880`, `:882`). Não ressuscita pergunta morta. |
| `ressuscitar_perguntas` | Só alcança resolvedoras associadas, materializadas, sem texto, cujo marco não esteja comprovadamente aceso (`app/chamados/models.py:1112`, `:1119`). Pergunta ausente, validada sem materialização, com texto ou ligada a outro chamado fica fora. Limpeza em `:1162`. |
| Pedido de ressuscitação pelo cartório | Pula chamado sem perguntas, com fio vivo ou com qualquer resposta textual (`app/ponto/services/cartorio.py:1261`, `:1263`). Duplicatas tentam fechamento; demais seguem para ressuscitação (`:1265`, `:1271`). Impressão igual impede executar esse corpo (`:1092`). |
| `reabrir()` | Só há transição de resolvido/fechado; não é reparo para chamado já aberto (`app/chamados/catalogo/motor.py:113`). Quando consegue reabrir, pede ressuscitação (`app/chamados/models.py:1098`). |
| Fábrica | Exige célula elegível e motivo vencido (`app/chamados/services/fabrica_da_celula.py:73`, `:80`, `:81`). Dedup considera pergunta morta não duplicata como existente (`:93`, `:95`); o gate reutiliza o fato sem limpar carimbos (`app/chamados/models.py:2061`, `:2084`). |
| Gatilho da fábrica | Só alcança células efetivamente julgadas, não a mera ausência de fio (`app/ponto/services/cartorio.py:769`, `:770`). |
| Evento de beco | Exige pai já resolvido; pai aberto não entra (`app/chamados/services/becos.py:31`). |

Nascimento e mudança de veredito/ata geram eventos, mas somente eventos pendentes alimentam o consumidor (`app/chamados/models.py:736`, `:739`; `app/ponto/portas/celula.py:623`, `:642`; `app/chamados/reconciliador.py:1128`). Isso não comprova reparo do estoque anterior nem superação das guardas acima.

**O261 conta #22914? “Aberto sem pergunta viva” não basta:**

- Precisa módulo de furo e célula associada (`app/chamados/services/vigia_fio.py:184`, `:199`).
- A célula precisa ser acusadora (`:227`).
- O vigia reúne perguntas da **mesma célula**, não só resolvedoras daquele chamado. Outra pergunta viva da célula pode excluí-lo (`:242`, `:244`, `:258`).
- Exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`:261`, `:266`, `:273`, `:279`).
- Passando tudo, soma uma célula em `fio_morto` e, com cobrança, em `fio_morto_cobrando` (`:286`, `:288`).

**Por que nenhum caminho alcançou especificamente #22914: não provado.** Módulo, célula, data, veredito, carimbos das perguntas e estado dos eventos desse ID: **nao li**. Escolher uma das condições acima como causa seria **hipótese**.

## T18 — Retrato × `o_que_segura`

| Fonte | O que efetivamente lê |
|---|---|
| Ficha/drawer | Busca `RetratoFolha` da empresa/competência e localiza a linha do colaborador (`app/colaboradores/views.py:980`, `:982`). O localizador lê **`RetratoFolha.linhas`** (`app/folha/retrato_folha.py:110`, `:935`). |
| Pendências exibidas | Template mostra `retrato_fazer` e **`retrato_linha.datas`**, já gravadas (`app/templates/folha/_retrato_o_que_fazer.html:33`, `:44`, `:46`). |
| Produção da foto | `montar_linhas` chama `fora_da_folha(..., com_itens=True)` (`app/folha/retrato_folha.py:1157`), que usa `classificar_export` e `motivos_retencao_celula` (`app/folha/fora_da_folha.py:120`, `:93`, `:139`). Grava status, motivo, dias e datas (`app/folha/retrato_folha.py:1180`, `:1183`). |
| `o_que_segura` | Consulta **agora** `classificar_export` e a mesma autoridade de dias retidos; devolve o status em `entra` (`app/folha/fora_da_folha.py:191`, `:193`, `:212`). |

**Não encontrei duas regras de retenção nesse caminho.** As duas leituras descendem das mesmas autoridades; a ficha lê a resposta gravada e `o_que_segura` pede resposta atual (`app/folha/retrato_folha.py:1157`; `app/folha/fora_da_folha.py:191`, `:193`).

Há duas distinções relevantes:

1. **“Folga” pode ser rótulo de causa**, não classificação atual de cada data. `tipo='folga'` pode vir de causas como `escala_sem_folga_definida` ou `dias_trocados`, lidas de `FechamentoMensal.causa_espelho` (`app/folha/retrato_folha.py:233`, `:238`, `:374`, `:1115`).
2. O juiz atual de retenção usa veredito/protestos da célula; não possui nesse ramo veto independente por `trabalha=False`. Portanto uma folga ainda acusadora pode reter; uma célula absolvida deixa de reter por esse critério (`app/folha/export.py:1188`, `:1215`, `:1227`, `:1235`).

**O frescor pode não perceber mudança da célula:** compara somente `Max(FechamentoMensal.atualizado_em)` com `RetratoFolha.lavrado_em` (`app/folha/retrato_folha.py:1298`, `:1306`). Veredito/ata são gravados pela porta de célula separadamente (`app/ponto/portas/celula.py:691`, `:697`).

**Hipótese de defasagem:** célula deixa de reter depois da lavra, retrato não é relavrado e nenhum fechamento recebe timestamp posterior. A ficha conserva `linhas.datas`; `o_que_segura` responde `entra`; o indicador ainda pode dizer atualizado (`app/folha/retrato_folha.py:1183`, `:1303`; `app/folha/fora_da_folha.py:212`).

**Conclusão:** o código sustenta **projeção velha como hipótese**, não uma derivação paralela de folga na ficha. Para confirmar o ocorrido, a linha gravada, `lavrado_em`, timestamps dos fechamentos e histórico das células: **nao li**.

_88 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
