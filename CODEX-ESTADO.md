# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 18:45** · publicado em **06/11/2026 18:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 18:45 · sessao 96f30b8ed283 · - · - · origem=Ronald (tmux)

Censo persistente: **2 sítios físicos** — nascimento em `app/chamados/models.py:2476` e preenchimento de pergunta antiga sem data em `app/chamados/services/fio.py:832-848`. Há **6 chamadas a `PerguntaDisputa.abrir`** e **4 chamadas de reapontamento com data explícita**, detalhadas abaixo.

| Sítio | Motivos | Fonte da data | Cobra saída overnight? | Coerência com `_T1` |
|---|---|---|---|---|
| `models.py::abrir`, `app/chamados/models.py:2476` | Qualquer motivo recebido | Parâmetro; ausente vira `localdate()` em `:2424-2427` | Conforme chamador | Não normaliza pelo turno; depende do chamador (`:2414-2431`) |
| `fio.py::reapontar_resolvedora`, `app/chamados/services/fio.py:832-848` | Mantém motivo existente | Parâmetro, **somente se pergunta não tiver data** (`:832-835`) | Conforme pergunta | Consulta `resolvedora_valida` (`:838`), mas não deriva data pelo `_T1` |
| `gerar_perguntas_disputa`, `app/chamados/services/disputa_emissao.py:389-396` | Motivos do catálogo; inexistente é ignorado (`:294-296`; catálogo `app/chamados/catalogo/perguntas.py:34-139`) | Grade → contexto do motivo → data local de `ultima_batida`; chamado derivado quando aplicável (`:319-396`) | Sim para `orfao_14h` | **Mista**, conforme desdobramento abaixo |
| `_criar_pergunta_se_couber`, `app/chamados/services/disputa_emissao.py:770-777` | `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta`, conforme chamadores (`:809-813`, `:985-993`, `:1038-1048`, `:1071-1080`) | Parâmetro; quatro fontes desdobradas abaixo | Sim | **Mista** |
| `criar_disputa_ancora_de_chamado`, `app/chamados/services/disputa_emissao.py:1195-1202` | `ancora_ausente` | Sem parâmetro: relógio de `abrir` (`models.py:2424-2427`) | Não; pergunta pela primeira entrada (`disputa_emissao.py:1132-1134`) | Não aplicável: instante completo vem da resposta (`:1199`) |
| `declarar_hora_colab`, `app/chamados/services/declaracao_texto.py:325-326` | Quatro motivos do mapa `:42-46` | `dia` do marco faltante escolhido (`:288-300`) | Sim para marco `fim` | Coerente com a célula: `_marcos_faltantes` devolve `data_dia`, inclusive véspera (`:88-123`) |
| `aplicar_declaracao`, `app/chamados/services/declaracao_texto.py:614-615` | Quatro motivos do mapa `:42-46` | `p['dia']`, proposta casada com marco faltante (`:566-586`) | Sim para `fim` | Mesma semântica de célula da anterior |
| `acao_reabrir_disputa_admin`, `app/chamados/services/acoes_disputa.py:259-264` | Motivo escolhido entre `MOTIVOS_CHOICES` (`:245-249`) | Sem parâmetro: relógio de `abrir` (`models.py:2424-2427`) | Não nesta pergunta; `tipo_resposta='escolha'` (`acoes_disputa.py:262`) | Não aplicável |
| Reapontamento c2, `app/chamados/reconciliador.py:937-943` | Grupo dia: quatro motivos (`:786-800`; `models.py:3048-3049`) | `data_ctx`, derivada do argumento `dia` (`reconciliador.py:801`) | Sim | Não normaliza; preserva dia recebido |
| Reapontamento histórico, `app/chamados/reconciliador.py:1620-1626` | Motivo da pergunta (`:1541-1542`) | Data da pergunta; ausente, contexto do motivo (`:1552-1553`) | Conforme pergunta | Mista; depende da fonte anterior |
| Reapontamento derivado, `app/chamados/reconciliador.py:1638` | Motivo da pergunta (`:1541-1542`) | `_derivar_chamado_destino`: data da pergunta/contexto; depois dia do chamado (`disputa_emissao.py:174-179`, `:228-237`) | Conforme pergunta | Mista |
| Reapontamento de declaração, `app/chamados/services/declaracao_texto.py:320` | Quatro motivos do mapa `:42-46` | `dia` do marco escolhido (`:299-300`) | Conforme marco | Dia da célula; mesma fonte do nascimento `:325-326` |

**Desdobramento do gerador genérico (`disputa_emissao.py:389`):**

- **Célula:** `fabrica_da_celula.perguntar` injeta `dia` no contexto (`app/chamados/services/fabrica_da_celula.py:129-141`); no julgamento, recebe `cel.data` (`:174-176`). Os motivos saem dos marcos faltantes da ata (`:47-64`). Pode cobrar saída overnight e usa o dia da célula, coerente com a unidade do turno (`app/chamados/services/declaracao_texto.py:42-46`, `:88-123`).
- **Pendências:** `perguntar_entradas_pendentes` lê cada `entradas_pendentes[*].data`, classifica e injeta essa data (`app/chamados/services/disputa_emissao.py:257-274`). O produtor grava **dia local da entrada**, não dia do relógio (`app/ponto/utils_turno_orfao.py:31-40`).
- **Detector de órfão da API:** `detector_anomalias` grava em `contexto['orfao_14h']['data']` a **data da saída esperada** (`app/chamados/detector_anomalias.py:31-50`). Usa `escala.utils.data_saida_esperada`, que encadeia marcos e pode avançar o dia (`app/escala/utils.py:936-939`, `:950-969`). Portanto pode entregar D+1; diverge da semântica de dia da célula/entrada.
- **API sem data específica:** o contexto carrega `ultima_batida.timestamp=agora.isoformat()` (`app/api/views.py:772-779`; clone `app/api/views_core.py:772-779`), e o gerador usa a data local desse timestamp (`app/chamados/services/disputa_emissao.py:350-364`). É data do relógio da requisição; não há normalização pelo turno.
- **PWA, entrada faltante:** `saida_sem_entrada.data=agora_local.date()` (`app/ponto/services/triagem_batida.py:649-660`), consumida como contexto (`app/chamados/services/disputa_emissao.py:343-347`). Cobra entrada, não saída.
- **Conversa sem data:** termina no relógio `localdate()` do nascimento (`app/chamados/models.py:2424-2427`).
- **Smoke fora da pasta de testes:** injeta data literal `"2026-05-15"` para perguntas com `{data}` e chama o gerador (`app/chamados/management/commands/smoke_chamados.py:90-102`); sem placeholder, cai nas outras fontes/fallback do gerador (`app/chamados/services/disputa_emissao.py:343-396`).
- **Lote de competência:** delega a `fabrica_da_celula.perguntar(colab, dia, [motivo], ...)` (`app/chamados/management/commands/disparar_perguntas_competencia.py:268-280`), logo segue o ramo de dia explícito da célula (`app/chamados/services/fabrica_da_celula.py:129-141`).
- **Retro de selfie:** fornece `batida_em`, mas não `data` nem `ultima_batida` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`); o gerador não lê `batida_em` para datar (`app/chamados/services/disputa_emissao.py:343-364`), portanto pode cair no relógio do nascimento (`app/chamados/models.py:2424-2427`).

**Desdobramento do helper de órfãos (`disputa_emissao.py:770`):**

| Fonte | Evidência | Motivos / semântica |
|---|---|---|
| Complemento de marcos | `app/chamados/services/disputa_emissao.py:806-813` | Quatro motivos via mapa; `dia` da célula. Chamado já perguntado usa `_ddc(contexto)` ou data da pergunta em `:913-920`; ramo por ata usa dia do chamado em `:1006-1014`. |
| Intervalo dedicado | `app/chamados/services/disputa_emissao.py:982-993` | `intervalo_saida`/`intervalo_volta`; data do próprio marco no contexto (`:985-987`). |
| Grupo por batida de origem | `app/chamados/services/disputa_emissao.py:1037-1048` | Classifica dia **da entrada**, mas grava `resolver_data_evento` ou dia da entrada. Para `orfao_14h`, resolver devolve **data da saída esperada** (`app/escala/servico_jornada.py:483-491`), calculada pelo fim do turno (`:200-209`) e encadeamento com +1 (`:61-85`, `:89-93`). Este é o ramo D→D+1 explícito. |
| Sem origem | `app/chamados/services/disputa_emissao.py:1065-1084` | Data de `_ddc(contexto_json)`; classifica esse dia, cria âncora e complementos. Hierarquia: `data_turno`, `data_batida`, `entrada_timestamp`, pendentes BR, `horario_previsto` (`app/chamados/data_evento.py:16-39`). |

O predicado de preenchimento em `reapontar_resolvedora` impede que seus quatro chamadores substituam uma `data_evento` já preenchida (`app/chamados/services/fio.py:832-835`). A atribuição `models.py:2418` pertence a `_Proxy`, sem persistência (`app/chamados/models.py:2414-2419`); `fio.py:843` apenas desfaz a atribuição em memória antes do retorno de recusa (`app/chamados/services/fio.py:841-844`).

Contagem consolidada: **2 escritores físicos persistentes; 6 sítios de nascimento por `abrir`; 4 delegações de reapontamento com data explícita** (`app/chamados/models.py:2476`; `app/chamados/services/fio.py:834-848`; seis e quatro sítios enumerados na tabela). A busca fora de testes/migrations não encontrou outro `create`, `get_or_create`, `defaults`, `bulk_create`, `bulk_update`, `setattr` ou `.update()` que grave `data_evento` [nao verificado por referência estática única]. O P13 lista apenas `models.py` e `services/fio.py`, mas seu detector cobre somente atribuição de atributo e `.update(...)`, portanto sozinho não prova o censo ampliado (`app/core/tests/test_contract_escritores_estado.py:60-62`, `:177-181`).

## 06/11 18:44 · sessao 38685ef3f498 · - · - · origem=Ronald (tmux)

_T1 é a constante `core/juizes.py:368`: `_T1 = 'a que turno (data_turno) pertence a batida?'`. O registro aponta para **`turnos_do_colab`**, em `app/core/juizes.py:195-196`, assinatura **`turnos_do_colab(colab, ini, fim, celulas=None)`**, em `app/ponto/turnos.py:1848`. Ela entrega turnos com `data_turno` na janela e passa ao corpo `_turnos_do_colab_puro`, em `app/ponto/turnos.py:1849-1857,1913-1916`; este passa ao pareador e filtra pelo `data_turno`, em `app/ponto/turnos.py:1990-1993`.

O mapa para apresentação é **`dia_das_batidas(colab, ini, fim, turnos=None)`**, em `app/ponto/turnos.py:2874`. Seu contrato explicitamente remete ao _T1: batida reclamada pertence a `t.data_turno`; não reclamada pertence a `data_local(timestamp)`, em `app/ponto/turnos.py:2877-2881`. O corpo é **`_dia_das_batidas_puro(turnos, batidas_da_janela)`**, em `app/ponto/turnos.py:2908`, com a atribuição exata `out[b.pk] = t.data_turno`, em `app/ponto/turnos.py:2915-2916`, e `setdefault(..., data_local(...))`, em `app/ponto/turnos.py:2917-2918`.

Chamadores diretos de `dia_das_batidas` fora de testes/migrations: espelho do app (`app/api/views.py:1446`), justificativas do app pelo alias `_ddb_j` (`app/api/views.py:2037-2038`) e espelho da tela (`app/ponto/services/espelho.py:450-451`). O lote oferece `CargaDaJanela.dia_das_batidas(self, colab)` e chama o mesmo corpo (`app/ponto/turnos.py:3007-3010`); o painel o lê em `app/core/services/painel_op.py:330`.

Chamadores diretos de **`turnos_do_colab`** encontrados, incluindo aliases, fora de testes/migrations: [nao verificado]

- No próprio módulo: `app/ponto/turnos.py:778,2096,2148,2179,2901,3033`.
- Ponto: `app/ponto/nucleo.py:47`; `app/ponto/turno_leitura.py:89,95`; `app/ponto/views.py:377,1585,3177`; `app/ponto/services/espelho.py:614`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82`.
- Comandos: `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:151`; `app/ponto/management/commands/censo_fase_12x36.py:180`.
- Chamados: `app/chamados/models.py:1273`; `app/chamados/juizes.py:371,1842`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:123,220`; `app/chamados/services/disputa_emissao.py:644`.
- Escala: `app/escala/utils.py:1602,1919`; `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250,392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`.
- Outros: `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213,451`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`; `app/colaboradores/services/drawer.py:45`.

**Vigia:** o corpo vigente difere do cabeçalho antigo que descreve comparação de hora com entrada prevista: `app/ponto/management/commands/vigia_plantio_overnight.py:8-10,19-23`. O predicado executado é:

1. Candidato: colaborador com batida `origem__in=('disputa_s84_retro',)`, `aprovada=True`, `timestamp >= now()-dias`, em `app/ponto/management/commands/vigia_plantio_overnight.py:16,30-32`.
2. Carrega `turnos_do_colab(colab, hoje-dias, hoje)`, seleciona no turno batidas de mesma origem aprovadas e exige `t.data_turno is not None`, em `app/ponto/management/commands/vigia_plantio_overnight.py:26-29,38-41`.
3. Exige vínculo e tipo; calcula `ec.eh_dia_trabalho(t.data_turno)`; **só emite quando `trab is False`**, em `app/ponto/management/commands/vigia_plantio_overnight.py:42-56`.
4. Não consulta `PerguntaDisputa`, `data_evento`, entrada que a saída deveria fechar, distância de 24h nem exige cruzamento de meia-noite: seu corpo completo vai de `app/ponto/management/commands/vigia_plantio_overnight.py:19` a `:57`.

Logo, ele **não detecta necessariamente** o deslocamento “um dia depois”: se a batida errada receber `data_turno=D+2` e esse dia for trabalho no 12x36, `trab is False` falha; o dia pretendido D não é comparado (`app/ponto/management/commands/vigia_plantio_overnight.py:38-56`). Há caminho concreto para `data_turno=D+2`: com mapa de DNA e sem marcos em D+1, `_vespera` retorna `None` (`app/ponto/turnos.py:1297-1299`); `_data_do_turno` sem véspera retorna o dia local da referência (`app/ponto/turnos.py:283-286`). O mapa só inclui DNA com `marcos` (`app/escala/utils.py:103-108`). Isso explica a cegueira estrutural; **o retorno efetivo do pareador no caso de produção continua [nao verificado]**. Não afirmar universalmente que toda S D+2 escapará: sem mapa/na configuração alternativa o pareador pode atribuí-la à véspera (`app/ponto/turnos.py:1297-1300,291-296`).

**Tipo E/S do app:** não há resposta binária “sempre aceita” ou “sempre deriva”. A API recebe e valida `'E'/'S'` (`app/api/views.py:432,438-439`; clone `app/api/views_core.py:419,425-426`).

- Ao vivo, `abs(agora-ts_efetivo) <= 300s`: chama `proximo_tipo_de(colab, ts_efetivo)` e substitui o tipo divergente, em `app/api/views.py:600-608`; clone `app/api/views_core.py:592-600`.
- Fora disso: busca vínculo do dia local, obtém marcos do template no dia, chama `tipo_por_marcos(...)` e pode substituir o cliente, em `app/api/views.py:609-631`; clone `app/api/views_core.py:601-623`.
- `tipo_por_marcos` devolve o único tipo casado em até 30 minutos; sem marcos, sem casamento ou ambiguidade conserva o cliente, em `app/ponto/turnos.py:2422-2445`.
- A view passa o tipo **já julgado** a `registrar_batida`, em `app/api/views.py:683-684`; clone `app/api/views_core.py:680-681`.
- `registrar_batida(colaborador, timestamp, tipo, origem, *, autoridade_humana=False, fato_de_chao=None, **campos)` não deriva E/S: dedup pode devolver existente (`app/ponto/registro_batida.py:94,113-122`), e os dois `create` gravam diretamente `tipo=tipo` (`app/ponto/registro_batida.py:152-158`).
- `proximo_tipo_de` dá E sem turno; S aberto fora de intervalo; em intervalo desempata pela proximidade aos marcos; intermitente tem ramo de alternância, em `app/ponto/turnos.py:2448-2493`.

**PERGUNTA:** “A batida materializada está no dia/instante do turno que a pergunta pretendia completar?” não é a pergunta executada pelo vigia; este pergunta se o **turno resolvido atual** está em dia que o vínculo diz folga (`app/ponto/management/commands/vigia_plantio_overnight.py:19-23,38-56`).

## 06/11 18:44 · sessao c3c6441f8b27 · - · - · origem=Ronald (tmux)

Leitores relevantes e premissas, somente leitura:

| Leitor | O que faz com `data_evento` / premissa |
|---|---|
| `_ts_tipo_do_plantio` | Usa como data-base do turno: busca régua e grade do dia, combina HH:MM nela e chama ajuste overnight; admite resultado no dia seguinte. `app/chamados/services/materializacao.py:279-297`, `:344-347`, `:370-372`, `:413-420`; ajuste em `app/chamados/juizes.py:303-314`. |
| `_slot_esperado` | Para resposta `hora`, delega ao plantio, herdando a premissa acima. Para `data_hora`, a data vem da resposta completa, sem ajuste. `app/chamados/juizes.py:331-347`. |
| `estado_aplicacao_pergunta` | Quando ainda não materializada, deriva exatamente pelo plantio e procura batida/vizinha naquele timestamp. Se já carimbada, pode retornar “aplicada” antes de derivar. `app/chamados/services/materializacao.py:431-452`. |
| `_turno_tem_saida` | Assume **dia do turno/entrada**: pede `turnos_do_colab(colab, dia, dia)` e exige `t.data_turno == p.data_evento`. `app/chamados/juizes.py:362-372`. |
| `via_pelo_fato` | Mistura dois recortes declarados: para adesão, qualquer batida apurável que toque o **dia civil**; para completude, `TurnoMaterializado.data_turno == data_evento`, isto é, **dia do turno**. `app/chamados/juizes.py:1447-1462`, `:1470-1485`. |
| `_pergunta_materializada_de_fato` / `_plantio_na_hora_do_slot` | Deriva slot; para plantadas vinculadas de resposta `hora`, aceita mesma hora em `data_evento -1/0/+1`, com ±20 minutos. Para `data_hora`, exige instante completo. Portanto esse termo deliberadamente não revalida uma data única. `app/chamados/juizes.py:242-268`, `:701-725`. |
| `_pergunta_viva_calc` | Na ausência de prova suficiente, chama `classificar_falta(colab, p.data_evento)`; usa como **dia da célula/jornada**. `app/chamados/juizes.py:1197-1203`; classificação por turno em `app/escala/servico_jornada.py:392-393`. |
| `_celulas_da_pergunta` | Só alimenta a classificação quando `p.celula.data == p.data_evento`, reforçando a identidade **dia da célula**; caso contrário não usa a FK como insumo. `app/chamados/juizes.py:612-629`. |
| `data_do_fato_de` | Devolve o próprio `p.data_evento` e compara por igualdade com dia do chamado. A prosa diz “é nela que a materialização planta a batida”: premissa de **dia do evento faltante**, embora o ajuste permita outro dia. `app/chamados/services/fio.py:43-52`, `:84-88`. |
| Tela do fio / filtros | `data_do_fato` delega à função acima; divergência mostra os dois dias. A tela explicita “Dia da pergunta … (é aqui que a batida nasce)”: premissa de **dia civil do plantio**. `app/chamados/templatetags/chamados_extras.py:15-31`, `:37-58`; `app/templates/chamados/partials/modal_fio.html:161-167`. |
| Gate `DATA_EM_DISPUTA` / conferência | Materialização bloqueia diferença entre `data_evento` e dia do chamado até conferência; a conferência mantém `data_evento`, apenas carimba que ele vale. `app/chamados/services/materializacao.py:1166-1173`; `app/chamados/services/validacao.py:611-627`. |
| `resolvedora_pertence_ao_turno` | Não exige uma identidade única: aceita igualdade; aceita diferença ±1 se o turno do menor dia cruza a meia-noite **ou se isso é indeterminado**. Reconhece explicitamente que perguntas/chamados podem usar entrada ou saída. `app/chamados/juizes.py:1982-1997`. |
| `_dia_do_plantio` / `_dias_do_par` do passivo validado | Premissa explícita **dia da célula/entrada**: saída overnight do dia 20 nasce no dia 21, mas competência é a do dia 20; entrada 19:00 e saída 07:00 têm o **mesmo** `data_evento`. `app/chamados/services/passivo_validado.py:98-113`, `:460-466`. |
| `lavrar_sem_turno_aberto` / `veredito_orfa` | Usam como **dia do turno** para verificar turno aberto e encerramento temporal. `app/chamados/reconciliador.py:369-372`, `:1071-1082`. |
| `acordar_celulas` / semeadura | Preferem FK da célula; sem ela buscam `(colaborador, data_evento)`, assumindo **dia da célula**. `app/chamados/models.py:2671-2684`; `app/chamados/reconciliador.py:1291-1299`. |
| Fábrica da célula / pergunta no app | Identificam perguntas do dia por igualdade `data_evento == cel.data` ou dia da ata; premissa **dia da célula**. `app/chamados/services/fabrica_da_celula.py:118-126`; `app/chamados/services/pergunta_no_app.py:92-110`. |
| Materialização de ausência / aceite de folga | Cria ausência ou declara folga diretamente em `p.data_evento`; trata como **dia da jornada a decidir**, não timestamp da saída. `app/chamados/services/materializacao.py:472-485`, `:516-532`; `app/chamados/services/validacao.py:250-271`. |
| Régua de validação | `_marco_esperado_da_grade` usa como dia dos marcos da escala; fallback de `motivo_implausivel_validacao`. `app/chamados/juizes.py:1517-1543`, `:1700-1709`. |
| `classe_do_lote` | Combina **diretamente** `data_evento + HH:MM` para procurar conflito ±`CONFLITO_LOTE_MIN`; não passa pelo ajuste overnight nesse trecho. Premissa **dia civil da batida**. `app/chamados/services/validacao.py:824-829`. |
| `vigia_carimbo` | Lê o juiz e `_slot_esperado`; herda o mesmo cálculo de data do plantio. `app/chamados/services/vigia_carimbo.py:94-116`. |
| `vigia_fio` | Censo principal agrupa por `p.celula_id`, não por data; medição de dono/exportação usa competência de `p.data_evento`, assumindo **dia financeiro da jornada**. `app/chamados/services/vigia_fio.py:400`, `:466`, `:616`, `:815-817`. |
| `vigia_plantio_overnight` | Não lê `PerguntaDisputa.data_evento` nem `pergunta_origem`: resolve o `data_turno` da própria batida e acusa somente `ec.eh_dia_trabalho(t.data_turno) is False`. `app/ponto/management/commands/vigia_plantio_overnight.py:30-49`. |
| Tranca / exportação / prazo | Usam diretamente como dia da competência: `dia_da_pergunta_trancado`, seleção de perguntas da competência e `expira_em`. Premissa **dia financeiro da jornada**; não recalculam a data pelo timestamp ajustado. `app/chamados/juizes.py:931-945`; `app/chamados/services/competencia_trancada.py:106-108`; `app/chamados/veredito_lavrado.py:95-116`. |
| Recalcular após validar | Passa `perg.data_evento` ao recalculo por evento, como dia do fato financeiro. `app/chamados/services/validacao.py:404-408`. |
| Calendário / prévia da folha | Indexam perguntas pelo dia `data_evento`; premissa **dia da célula apresentada/apurada**. `app/colaboradores/services/calendario.py:472-477`; `app/folha/previa.py:64-67`. |
| API / atendimento / consulta do dia | API serializa a data sem conversão; cards a exibem; `dia_do_colab` lista perguntas por igualdade à data pedida. Não aplicam ajuste overnight. `app/api/views.py:1168-1169`; `app/api/views_mensageria.py:568`; `app/templates/chamados/partials/_card_atendimento.html:87-90`; `app/ponto/services/dia_do_colab.py:64-71`. |
| Dia/competência do próprio chamado | `_dia_da_disputa`, versão em lote e `_dias_das_perguntas_vivas` usam datas das perguntas como fallback/insumo para dia ou competência do chamado. `app/chamados/juizes.py:70-77`, `:203-211`, `:2024-2040`. |
| Auditoria de órfãs | Procura chamado com `contexto_json.data_turno == p.data_evento` **ou** `celula.data == p.data_evento`, assumindo **dia do turno/célula**; cobertura de ausência usa o mesmo dia. `app/chamados/management/commands/auditar_invariantes_chamados.py:63-78`, `:156-163`. |

**Divergência provada:** a fonte `resolver_data_evento(orfao_14h)` entrega **data da saída esperada**, enquanto materializador, completude e passivo usam essa mesma coluna como **data-base da jornada/célula**; telas e conflito do lote a tratam como **dia civil da batida**. `app/escala/servico_jornada.py:483-491`; `app/chamados/services/materializacao.py:414-420`; `app/chamados/juizes.py:371`; `app/chamados/services/passivo_validado.py:463-465`; `app/templates/chamados/partials/modal_fio.html:166`; `app/chamados/services/validacao.py:824-829`.

**PERGUNTA:** o leitor de conflito do lote calcula `data_evento + hora` diretamente, enquanto o slot canônico passa pelo ajuste overnight; os dois leitores podem consultar dias distintos para a mesma resposta. `app/chamados/services/validacao.py:824-829`; `app/chamados/services/materializacao.py:413-420`.

**PERGUNTA:** a prova de materialização por batida vinculada aceita ±1 dia do `data_evento`, portanto não distingue por si só a saída correta D+1 da saída tardia D+2 quando a pergunta está em D+1 e ambas têm a hora declarada. `app/chamados/juizes.py:263-268`, `:718-725`.

## 06/11 18:42 · sessao b78facd1c787 · PAINEL-SITUACIONAL-UM-MONTADOR · censo · origem=Code

A cópia tem **dois montadores com contratos diferentes**: o lote produz 21 chaves; a célula viva produz 18, sem `estado_app`, `em_turno` e `justif_pend`. Só o lote considera o token ao resolver `plataforma`. (`app/colaboradores/services/situacional.py:180–210`; `app/colaboradores/painel_cell.py:41–70`)

Valores atuais, quantidade de colaboradores afetados, comportamento observado em produção e resultado de execução dos testes: **[nao verificado]**. [nao verificado]

1) **Quem monta a linha e os contextos**

Os produtores da linha são `montar_situacional(colabs_qs, corte, agora, f_plataforma='')` e `montar_linha_colab(c, maps=None)`. O primeiro devolve `(linhas, contadores, by_plat)`; o segundo devolve diretamente um dicionário. (`app/colaboradores/services/situacional.py:65,180,230–238`; `app/colaboradores/painel_cell.py:11,41`)

Comparação de **todas as chaves**, agrupando apenas campos com a mesma fonte:

| Campo | `montar_situacional` | `montar_linha_colab` |
|---|---|---|
| `colab` | O objeto `c`. (`app/colaboradores/services/situacional.py:181`) | O objeto `c`. (`app/colaboradores/painel_cell.py:42`) |
| `empresa` | Nome fantasia, senão razão social; `None` sem empresa. (`app/colaboradores/services/situacional.py:182`) | Mesma expressão. (`app/colaboradores/painel_cell.py:43`) |
| `last_login` | `c.usuario.last_login`, senão `None`. (`app/colaboradores/services/situacional.py:178,183`) | Mesma fonte. (`app/colaboradores/painel_cell.py:44`) |
| `l24` | Compara login com **`agora` recebido − 1 dia**. (`app/colaboradores/services/situacional.py:184`) | Compara com **`timezone.now()` interno − 1 dia**. (`app/colaboradores/painel_cell.py:45`) |
| `appver`, `ultimo_ip` | Do device ativo mais recente por `-ultimo_login`; `None` sem device. (`app/colaboradores/services/situacional.py:16–23,185,187`) | Mesma escolha no modo pontual; com `maps`, usa o device fornecido. (`app/colaboradores/painel_cell.py:20–21,28–29,46,48`) |
| `plataforma` | Chama `plataforma_exibicao(dev, principal, tem_batida, token_plat)`. Token: o mais recente por usuário com plataforma não vazia. (`app/colaboradores/services/situacional.py:90–93,186`) | Chama o helper com **três argumentos**, omitindo token. (`app/colaboradores/painel_cell.py:47`) |
| `tem_push` | Existência de assinatura ativa, carregada em conjunto. (`app/colaboradores/services/situacional.py:94,188`) | Existência pontual de assinatura ativa, ou pertencimento ao `push_set` fornecido. (`app/colaboradores/painel_cell.py:22,30,49`) |
| `tem_foto` | `bool(c.foto)`. (`app/colaboradores/services/situacional.py:189`) | Mesma expressão. (`app/colaboradores/painel_cell.py:50`) |
| `ultima_batida` | Última batida crua com `timestamp >= corte`; a consulta não impõe `timestamp <= agora` nem exclui retratadas. (`app/colaboradores/services/situacional.py:137–145,190`) | Última batida crua das **14 horas anteriores ao `now()` interno**, ou timestamp fornecido em `maps`. (`app/colaboradores/painel_cell.py:24,27,33–35,51`) |
| `estado_app` | Sem device → `sem device ativo`; sem adesão viva na vida → `instalou, nunca bateu`; sem adesão viva em `[corte, agora]` → `bateu e parou`; senão `ativo`. (`app/colaboradores/services/situacional.py:115,150,191–194`; `app/ponto/services/adesao.py:48–65`) | **Ausente.** O dicionário completo está em `app/colaboradores/painel_cell.py:41–70`. |
| `geo`, `geo_tipo` | Cor e tipo devolvidos por `_geo_do_pino`, usando coordenadas e accuracy da última batida do corte recebido. (`app/colaboradores/services/situacional.py:137–145,179,195–198`) | Mesmo helper, mas com a batida de 14 h ou coordenadas fornecidas em `maps`. (`app/colaboradores/painel_cell.py:25,27,33–40,52–53`) |
| `b_lat`, `b_lon` | Coordenadas dessa última batida convertidas em string; ausência/valor falso vira `''`. (`app/colaboradores/services/situacional.py:199–200`) | Mesma conversão, com a janela/insumo da célula. (`app/colaboradores/painel_cell.py:39,54–55`) |
| `p_lat`, `p_lon` | Coordenadas do posto em string, ou `''`. (`app/colaboradores/services/situacional.py:201–202`) | Mesmas expressões. (`app/colaboradores/painel_cell.py:56–57`) |
| `p_raio` | `_raio_do_posto(c.posto)`. (`app/colaboradores/services/situacional.py:203`) | Mesmo helper. (`app/colaboradores/painel_cell.py:58`) |
| `em_turno` | Booleano do resultado de `turnos_abertos_de(colabs, agora)`. (`app/colaboradores/services/situacional.py:169–170,208`) | **Ausente.** (`app/colaboradores/painel_cell.py:41–70`) |
| `justif_pend` | Quantidade de justificativas pendentes daquele colaborador, ou zero. (`app/colaboradores/services/situacional.py:124–128,209`) | **Ausente.** (`app/colaboradores/painel_cell.py:41–70`) |
| `led` | Disputa → vermelho; turno pelo carregador plural com `agora` recebido → amarelo; batida do corte → verde; senão cinza. (`app/colaboradores/services/situacional.py:56–62,169–170,210`) | Mesma precedência, mas consulta singular `turno_aberto_de(c, agora=timezone.now())`; verde/cinza depende da batida de 14 h ou de `maps`. (`app/colaboradores/painel_cell.py:68–70`) |

Não há chave exclusiva da célula. **`token_plat` não é chave de saída de nenhum dos dois**: é parâmetro do helper; no lote vem de `token_plat_map`. (`app/colaboradores/services/situacional.py:90–93,180–210`; `app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/views.py:3034`)

O helper resolve plataforma nesta ordem: **token → device → plataforma principal → `desconhecido` se há batida → `None`**. Portanto a divergência não se limita à janela de batidas: a célula também omite a fonte de maior precedência. (`app/colaboradores/views.py:3039–3045`; `app/colaboradores/painel_cell.py:47`)

`maps` exige `devices_map`, `push_set`, `disputa_set`, `batida_recente` e `batida_coord`; não fornece estado do app, token ou instante de referência e não substitui a consulta singular de turno. (`app/colaboradores/painel_cell.py:20–25,45,47,68–70`)

Os demais produtores explícitos de contexto são:

| Função | Contexto produzido |
|---|---|
| `_cell_colab_status` | `{'l': montar_linha_colab(c)}` para `_ps_linha.html`. (`app/colaboradores/painel_cell.py:75–81`) |
| `painel_situacional` | `quadros_barra`, `linhas`, `placar`, `proximo_offset`, `qs_lotes`, `restantes`, `total_linhas`, `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `plat_app`, `plat_apk`, `plat_ios`, `plat_pwa_android`, `plat_pwa_ios`, `plat_nunca`, `op_em_turno`, `op_justif`, `empresas`, `f_periodo`, `f_plataforma`, `f_empresa`, `f_praca`, `empresa_fora_rollout`, `cortados_painel`, `pracas`, `f_q`, `gmaps_key`. (`app/colaboradores/views.py:2696–2727`) |
| `painel_situacional_linhas` | `linhas`, `proximo_offset`, `restantes`, `qs_lotes`, para `_ps_lote.html`. (`app/colaboradores/views.py:2809–2814`) |
| `quadros_barra_situacional` | Quadros com `titulo`/`itens`; itens com `rotulo`, `numero`, `cor`, `attrs` e opcionais `classe`, `negrito_escuro`, `title`; atalhos com `atalho`, `rotulo`, `icone`, e `botao`/`onclick` ou `href`. Alimenta `quadros_barra`. (`app/colaboradores/views.py:2490–2549,2693–2697`) |

O painel e o lote percorrem `linhas`, vinculando cada dicionário a `l` antes de incluir `_ps_linha.html`. `render_cell` apenas encaminha template e contexto para `render_to_string`; não monta outra linha. (`app/templates/colaboradores/partials/painel_situacional.html:300–306`; `app/templates/colaboradores/partials/_ps_lote.html:11`; `app/core/live_cell.py:27–28`)

2) **Chamadores, argumentos e imports**

Chamadas diretas de `montar_linha_colab`:

| Chamador | Argumentos e forma de acesso |
|---|---|
| `_cell_colab_status` | `montar_linha_colab(c)`, com objeto buscado por `pk=ident` e relações empresa/posto/usuário carregadas. (`app/colaboradores/painel_cell.py:75–81`) |
| O200, `_cores` | Import local `from colaboradores import painel_cell`; chama `painel_cell.montar_linha_colab(self.c)`, com `now` interceptado para o instante `agora`. (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:105–115`) |
| O200c, `_pino` | Mesmo import local; passa `Colaborador.objects.get(pk=self.c.pk)`, com relógio interceptado. (`app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:137–148`) |
| LED, `_led(agora)` | Mesmo import local; passa `self.c`, com `painel_cell.timezone.now` interceptado para o argumento `agora`. (`app/colaboradores/tests/test_led_pelo_juiz.py:56–67`) |

Essas quatro chamadas omitem `maps`; nenhuma passa corte, instante ou usuário ao montador. (`app/colaboradores/painel_cell.py:11,81`; `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115`; `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148`; `app/colaboradores/tests/test_led_pelo_juiz.py:67`)

Chamadas diretas de `montar_situacional` em código de produção:

| Chamador | Argumentos |
|---|---|
| `painel_situacional`, com placar | `list(colabs_qs[:30]), corte, agora, f_plataforma`. (`app/colaboradores/views.py:2659,2664–2665`) |
| `painel_situacional`, sem placar | `colabs_qs, corte, agora, f_plataforma`; depois seleciona as primeiras 30 linhas. (`app/colaboradores/views.py:2679–2680`) |
| `painel_situacional_linhas`, com plataforma | Lista dos objetos correspondentes aos IDs do recorte em `[offset:offset+100]`, seguida de `corte, agora, ''`. (`app/colaboradores/views.py:2758,2797–2802`) |
| `painel_situacional_linhas`, sem plataforma | `list(qs[offset:offset+100]), corte, agora, ''`. (`app/colaboradores/views.py:2804–2805`) |
| `lavrar_placar_situacional.Command.handle` | Lista de todo `colaboradores_em_operacao()` com `select_related`; `corte=agora−7 dias`; `agora=timezone.now()`; plataforma omitida. (`app/core/management/commands/lavrar_placar_situacional.py:31,48–56`) |
| Auditor de invariantes, bloco S7 | Lista de todo `colaboradores_em_operacao()` com `select_related`; `_corte7=_agora7−7 dias`; `_agora7=_tz_s7.now()`; plataforma omitida. Só entra na chamada quando há placar global com idade ≤300 s. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1202–1216`) |

Nesses chamadores os imports de `montar_situacional` são **locais e sem apelido**. `_tz_l` e `_tz_s7` são apelidos de `timezone`. (`app/colaboradores/views.py:2645,2749,2752`; `app/core/management/commands/lavrar_placar_situacional.py:44`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1200,1212`)

Chamadas diretas nos testes: [nao verificado]

| Chamador | Argumentos / import |
|---|---|
| `_monta(n)` | `_qs(n)`, `corte=agora−7 dias`, `agora=timezone.now()`; queryset da fixture com `select_related`, ordenado por nome/pk. Import no topo, sem apelido. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:29,83–92`) |
| `_situ` | `Colaborador.objects.filter(pk=self.c.pk)`, `agora−7 dias`, `agora`; import local, sem apelido. (`app/colaboradores/tests/test_situacional_pelo_juiz.py:19–23`) |
| O200, `_cores` | Queryset de `self.c.pk`, com `select_related`; `agora−14h`, `agora`; import local. (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:112–120`) |
| O200c, `_pino` | Queryset de `self.c.pk`, com `select_related`; `agora−14h`, `agora`; import local. (`app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:145–152`) |
| Adesão, `_estados` | Queryset de `retratado` e `vivo`; `corte=agora−14h`, `agora`; import no topo. (`app/colaboradores/tests/test_adesao_pelo_juiz.py:18,48–53`) |
| Estado do app, `_estados` | Queryset de `retratado`, `vivo`, `misto`; `corte=_dias(30)`, `agora=timezone.now()`; import no topo. (`app/colaboradores/tests/test_estado_app_pelo_juiz.py:23,65–69`) |
| `test_placar_global_bate_com_a_soma_das_linhas` | Lista do universo em operação com `select_related`; `corte=agora−PERIODO_PADRAO_DIAS`, `agora`; import local. (`app/core/tests/test_selo_placar_situacional.py:80–91`) |

Há também execução indireta pelo comando de lavra nos testes do placar e do Haiku; ela chega ao mesmo `handle`, que prepara o universo e os tempos. (`app/core/tests/test_selo_placar_situacional.py:81,103,111,121,124,132,187,199,203,223,237,250`; `app/core/tests/test_placar_situacional_um_so_numero.py:91,126`; `app/core/tests/test_haiku_em_turno_agora.py:65,76`; `app/core/management/commands/lavrar_placar_situacional.py:48–56`)

Nenhuma assinatura recebe usuário: o escopo chega pelo queryset/lista preparado pelo chamador. O universo `colaboradores_em_operacao()` exige colaborador ativo, empresa ativa, usuário com nome de 11 dígitos, empresa em rollout e vínculo que cubra hoje. (`app/colaboradores/services/situacional.py:65`; `app/colaboradores/painel_cell.py:11`; `app/colaboradores/queries.py:25–29,50–65`)

3) **Carga, lote, tempos e célula viva**

Na carga, `agora=timezone.now()`; `periodo='hoje'` significa **últimas 24 h**, `'30d'` significa 30 dias e qualquer outro valor resulta em 7 dias. O padrão do GET é `'7d'`. (`app/colaboradores/views.py:2566,2584–2590`)

O lote calcula **outro `agora` a cada requisição**, por `_tz_l.now()`, e reaplica a mesma regra de corte. Ele não recebe o instante da carga inicial. (`app/colaboradores/views.py:2764,2771–2777,2801–2805`)

Os escopos das duas views têm diferenças concretas:

| Aspecto | Carga | Lote |
|---|---|---|
| Permissão | `login_required` e `tem_acao(user, 'ver_painel_situacional')`. (`app/colaboradores/views.py:2552,2562–2564`) | `login_required` e `is_superuser or is_staff`. (`app/colaboradores/views.py:2739,2754–2756`) |
| Praça | GET, persistindo preferência; sem GET, preferência salva. (`app/colaboradores/views.py:2569–2573`) | GET ou preferência salva. (`app/colaboradores/views.py:2766–2767`) |
| Busca | Nome, CPF **ou matrícula**. (`app/colaboradores/views.py:2620–2624`) | Nome ou CPF. (`app/colaboradores/views.py:2784–2786`) |
| Ordem | Nome, pk. (`app/colaboradores/views.py:2658`) | Nome, pk. (`app/colaboradores/views.py:2787`) |

As URLs da carga e do lote são `/colaboradores/painel-situacional/` e `/colaboradores/painel-situacional/linhas/`. (`app/config/urls.py:26`; `app/colaboradores/urls.py:81–83`)

A cadeia da célula viva é:

- O `<tr>` declara `data-cell="colab-status:<pk>"` e `data-cell-every="45"`. (`app/templates/colaboradores/partials/_ps_linha.html:1`)
- O JS chama **`/live-cell/colab-status/<pk>/`**. A rota importa `live_cell` com o apelido `live_cell_view`; o resolvedor procura o tipo registrado e chama `fn(request, ident)`. (`app/static/js/live-cell.js:10,35–38`; `app/config/urls.py:39`; `app/core/urls.py:4,16`; `app/core/live_cell.py:18–22`)
- `_cell_colab_status(request, ident)` é o renderizador registrado para `colab-status`; busca somente por PK, carrega empresa/posto/usuário e chama `montar_linha_colab(c)`. Não encaminha período, filtros, corte, agora ou usuário ao montador. (`app/colaboradores/painel_cell.py:74–81`)
- A resposta contém um `<tr>` completo. O JS cria um `tbody` temporário e faz `replaceChild(novo, el)`: **substitui a linha inteira, incluindo atributos e todas as células**. Se o colaborador não existe, `None` vira HTTP 204 e o JS remove a linha. (`app/templates/colaboradores/partials/_ps_linha.html:1–21`; `app/static/js/live-cell.js:40–51`; `app/colaboradores/painel_cell.py:79–80`; `app/core/live_cell.py:23–25`)

Os 45 s são cadência **base**, com jitter de 0,75–1,25, condicionada à visibilidade e ao teto de seis requisições simultâneas. (`app/static/js/live-cell.js:11,17,58–76`)

O painel inicia `LiveCell.scan()`. Separadamente, a sentinela dos lotes usa HTMX com `outerHTML`, substituindo a própria sentinela pelas próximas linhas e eventual nova sentinela. (`app/templates/colaboradores/partials/painel_situacional.html:306,327`; `app/templates/colaboradores/partials/_ps_lote.html:11–12`)

4) **Usar `montar_situacional` para um colaborador**

**Sim, aceita queryset de um colaborador; não há dependência de agregados do lote para decidir sua linha.** A função materializa a entrada, restringe os mapas aos IDs/usuários recebidos e monta cada linha antes de calcular os contadores. (`app/colaboradores/services/situacional.py:82–94,115–150,175–238`)

Não há paginação, limite ou ordenação interna de colaboradores: a ordem é a da entrada; o único corte adicional de colaboradores é `f_plataforma`, quando preenchido. O carregador plural de turnos também separa insumos e julgamento por colaborador. (`app/colaboradores/services/situacional.py:82,172–176`; `app/ponto/turnos.py:2324–2362`)

O retorno para um colaborador continua sendo:

- Uma lista `linhas`, com a linha de 21 campos se ele estiver na entrada e passar pelo filtro opcional. (`app/colaboradores/services/situacional.py:172–211`)
- Contadores locais: `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `op_em_turno`, `op_justif`. (`app/colaboradores/services/situacional.py:213–237`)
- `by_plat`, inicializado pelo catálogo de plataformas mais `nunca`, contando a plataforma exibida das linhas retornadas. (`app/colaboradores/services/situacional.py:226–238`)

A extração pode seguir esta forma; com queryset unitário existente e sem filtro de plataforma, o teste O200 já usa `linhas[0]`. (`app/colaboradores/services/situacional.py:65,172–181,230`; `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:116–124`)

```python
linhas, _, _ = montar_situacional(qs.filter(pk=ident), corte, agora)
linha = next((l for l in linhas if l['colab'].pk == ident), None)
```

Isso **não significa custo zero**: ainda carrega devices, tokens, push, adesão, disputas, justificativas, batidas e turnos, além de calcular os agregados. As relações empresa/posto/usuário são acessadas na montagem; os chamadores de tela usam `select_related` para elas. (`app/colaboradores/services/situacional.py:88–170,178–238`; `app/colaboradores/views.py:2593,2779`)

O teste de custo exige igualdade do número de queries entre **30 e 100** colaboradores; não estabelece custo absoluto nem mede o caso de um. Os testes seguintes verificam equivalência do conjunto de turnos e do campo `em_turno`. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:94–130`)

5) **Quem lê os campos afetados**

| Campo / leitor | Uso |
|---|---|
| `estado_app` — partial | Seleciona três rótulos explicitamente; o ramo final escreve `sem device ativo`. Como a linha da célula não fornece a chave, esse é o ramo correspondente à ausência dela. (`app/templates/colaboradores/partials/_ps_linha.html:12`; `app/colaboradores/painel_cell.py:41–70`) |
| `estado_app` — testes | Os testes de adesão e estado do app extraem a chave das linhas do lote e distinguem batidas vivas/retratadas e uso dentro/fora da janela. (`app/colaboradores/tests/test_adesao_pelo_juiz.py:48–70`; `app/colaboradores/tests/test_estado_app_pelo_juiz.py:65–88`) |
| `plataforma` — partial e filtro JS | Alimenta `data-plat` e a pílula; o filtro do navegador lê `data-plat`. (`app/templates/colaboradores/partials/_ps_linha.html:1,11`; `app/templates/colaboradores/partials/painel_situacional.html:56`) |
| `plataforma` — agregação e barra | O serviço soma `by_plat` pela plataforma exibida; a view expõe os contadores e prepara a barra. (`app/colaboradores/services/situacional.py:226–229`; `app/colaboradores/views.py:2525–2532,2693–2695,2709–2714`) |
| `token_plat` — helper compartilhado | É lido por `plataforma_exibicao` e tem precedência sobre device/principal. Os testes próprios cobrem essa precedência e os fallbacks. (`app/colaboradores/views.py:3034–3045`; `app/colaboradores/tests/test_pilula_plataforma.py:9–23`; `app/colaboradores/tests/test_plataforma_pwa.py:45–59`) |
| Plataforma/token — ficha | A ficha busca seu próprio device/token e chama `plataforma_exibicao`; não consome o dicionário de nenhum dos dois montadores. (`app/colaboradores/services/ficha.py:140–151`) |

Há uma distinção adicional: **o filtro de plataforma do servidor usa o device ativo, enquanto a plataforma exibida pode vir do token**. Nesse filtro, `nunca` significa ausência de device ativo; ele não testa ausência de plataforma exibida. (`app/colaboradores/services/situacional.py:16–31,172–186`)

O **lavrador do placar não lê `estado_app`, `plataforma` ou `token_plat`**: chama o serviço 

[truncado em 20000 de 23910 caracteres]

## 06/11 18:39 · sessao 9ad1417ffcd9 · - · - · origem=Ronald (tmux)

Auditoria complementar dos itens 1 e 4:

- São **21 chaves no lote e 18 na célula**; exclusivas de `montar_situacional`: `estado_app`, `em_turno`, `justif_pend`; exclusivas de `montar_linha_colab`: nenhuma. `token_plat` **não é chave de saída** em nenhum montador: é argumento de `plataforma_exibicao`, alimentado no lote pelo mapa de tokens e omitido na célula. (`app/colaboradores/services/situacional.py:180–210`; `app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/views.py:3034–3045`)

- As 18 comuns são: `colab`, `empresa`, `last_login`, `l24`, `appver`, `plataforma`, `ultimo_ip`, `tem_push`, `tem_foto`, `ultima_batida`, `geo`, `geo_tipo`, `b_lat`, `b_lon`, `p_lat`, `p_lon`, `p_raio`, `led`. (`app/colaboradores/painel_cell.py:41–70`; `app/colaboradores/services/situacional.py:180–210`)

- Diferenças de derivação entre chaves comuns: `l24` usa `agora` recebido no lote, `timezone.now()` interno na célula; `plataforma` usa token somente no lote; `ultima_batida`, `b_lat`, `b_lon`, `geo`, `geo_tipo` usam a janela definida pelo `corte` recebido no lote e janela interna de 14h na célula sem `maps`; `led` pergunta ao carregador plural com `agora` no lote e ao singular com `now()` na célula, com verde/cinza dependentes das diferentes janelas de última batida. (`app/colaboradores/services/situacional.py:137–150,169–210`; `app/colaboradores/painel_cell.py:20–70`)

- A célula aceita `maps`, mas exige as entradas `devices_map`, `push_set`, `disputa_set`, `batida_recente`, `batida_coord` por indexação direta; isso só substitui esses insumos, não fornece `estado_app` nem token e não substitui a consulta de turno/relógio do LED. (`app/colaboradores/painel_cell.py:20–25,41–70`)

- Outros campos comuns usam as mesmas expressões/fontes por pessoa: `colab`, `empresa`, `last_login`, `appver`, `ultimo_ip`, `tem_push`, `tem_foto`, `p_lat`, `p_lon`, `p_raio`. Device é escolhido por `ativo=True` e `-ultimo_login` em ambos; push por `ativa=True`. (`app/colaboradores/services/situacional.py:16–23,88–94,181–203`; `app/colaboradores/painel_cell.py:28–30,42–58`)

Produtores diretos de contexto:

- `painel_situacional`: `quadros_barra`, `linhas`, `placar`, `proximo_offset`, `qs_lotes`, `restantes`, `total_linhas`, `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `plat_app`, `plat_apk`, `plat_ios`, `plat_pwa_android`, `plat_pwa_ios`, `plat_nunca`, `op_em_turno`, `op_justif`, `empresas`, `f_periodo`, `f_plataforma`, `f_empresa`, `f_praca`, `empresa_fora_rollout`, `cortados_painel`, `pracas`, `f_q`, `gmaps_key`. (`app/colaboradores/views.py:2696–2727`)
- `painel_situacional_linhas`: `linhas`, `proximo_offset`, `restantes`, `qs_lotes`. (`app/colaboradores/views.py:2809–2814`)
- `_cell_colab_status`: `{'l': montar_linha_colab(c)}`. (`app/colaboradores/painel_cell.py:75–81`)
- `quadros_barra_situacional` produz os dicionários aninhados de `quadros_barra`: quadros com `titulo`/`itens`; itens de operação/adoção com `rotulo`, `numero`, `cor`, `attrs`, opcionalmente `classe`, `negrito_escuro`, `title`; atalhos com `atalho`, `rotulo`, `icone` e `botao`/`onclick` ou `href`. O painel os passa ao include da barra. (`app/colaboradores/views.py:2490–2549,2693–2697`; `app/templates/colaboradores/partials/painel_situacional.html:228`)
- Não confundir `render_cell` com um montador adicional: apenas passa o contexto recebido para `render_to_string`. (`app/core/live_cell.py:27–28`)

Item 4, conclusão e limites:

- Um queryset de um colaborador é suportado: a função converte a entrada em lista, recorta os mapas pelos IDs/usuários dessa lista, mantém a ordem de entrada e não impõe paginação ou limite interno; há somente o filtro opcional de plataforma antes da construção das linhas. (`app/colaboradores/services/situacional.py:82–94,115–150,172–176`)
- Os agregados são calculados **depois** das linhas, a partir delas, e não retroalimentam qualquer campo; portanto o total do universo recebido muda, mas não decide a linha individual. (`app/colaboradores/services/situacional.py:175–238`)
- O turno plural recebe somente os colaboradores fornecidos, carrega os insumos pelos IDs e julga cada colaborador separadamente, armazenando a resposta pelo seu ID. (`app/ponto/turnos.py:2324–2362`)
- Retorno: `(linhas, contadores, by_plat)`; `contadores` contém `total_ativos`, `logaram_24h`, `fora_posto`, `com_push`, `dormindo`, `op_em_turno`, `op_justif`; `by_plat` inicializa as plataformas de `DeviceColaborador.PLATAFORMA_CHOICES` e `nunca`, somando a plataforma exibida nas linhas. (`app/colaboradores/services/situacional.py:213–238`)
- Extração segura conceitual: `linhas, _, _ = montar_situacional(qs.filter(pk=pk), corte, agora)`; `linha = next((l for l in linhas if l['colab'].pk == pk), None)`. Sem filtro de plataforma e com colaborador existente na entrada, haverá a única linha; com filtro de plataforma ele pode ser excluído. (`app/colaboradores/services/situacional.py:65,82–85,172–181,230`)
- Não afirmar “custo zero”: mesmo para um colaborador são carregados mapas, adesão, justificativas, batidas e turno, e calculados contadores/histograma; as relações `empresa`, `posto`, `usuario` são acessadas durante a montagem. Os chamadores de produção já usam `select_related` dessas relações. (`app/colaboradores/services/situacional.py:88–170,178–238`; `app/colaboradores/views.py:2593,2779`; `app/colaboradores/painel_cell.py:77`)
- Teste de custo existente compara **30 e 100** e exige diferença zero no número de queries, sem estabelecer teto absoluto nem medição para um colaborador. Os testes seguintes verificam equivalência do conjunto de turno aberto nos dois sentidos e o veredito `em_turno` na linha, não igualdade de todas as chaves entre os dois montadores. (`app/colaboradores/tests/test_situacional_pergunta_em_lote.py:94–130`)
- Ressalva importante: o filtro `f_plataforma` usa **device ativo** (`nunca` = ausência de device ativo), enquanto a plataforma exibida pode vir do token; são cálculos distintos na mesma função. (`app/colaboradores/services/situacional.py:26–31,172–186`)

## 06/11 18:39 · sessao beb420b7acdb · - · - · origem=Ronald (tmux)

Itens 5–6, com referências sempre relativas à raiz:

- `estado_app` é lido pelo template da linha: três valores explícitos (`ativo`, `bateu e parou`, `instalou, nunca bateu`); qualquer outro caso segue para `sem device ativo`. Portanto a ausência dessa chave no montador da célula alcança o ramo final do template (`app/templates/colaboradores/partials/_ps_linha.html:12`; dicionário sem a chave em `app/colaboradores/painel_cell.py:41`).
- `plataforma` alimenta `data-plat` e a pílula da linha; o JS filtra por esse atributo (`app/templates/colaboradores/partials/_ps_linha.html:1`, `app/templates/colaboradores/partials/_ps_linha.html:11`, `app/templates/colaboradores/partials/painel_situacional.html:56`).
- O serviço conta `by_plat` usando a **plataforma exibida** de cada linha; a view transforma isso em contadores e quadros da barra (`app/colaboradores/services/situacional.py:226`, `app/colaboradores/services/situacional.py:229`, `app/colaboradores/views.py:2693`, `app/colaboradores/views.py:2709`, `app/colaboradores/views.py:2525`).
- O filtro servidor de plataforma usa o **device ativo mais recente**, sem consultar token; `nunca` significa ausência desse device. Esse filtro e a plataforma exibida não seguem a mesma derivação (`app/colaboradores/services/situacional.py:16`, `app/colaboradores/services/situacional.py:26`, `app/colaboradores/services/situacional.py:172`, `app/colaboradores/services/situacional.py:186`).
- `token_plat` não é chave do dicionário da linha: é o quarto argumento de `plataforma_exibicao`, preenchido a partir do token mais recente com plataforma não vazia. A precedência do helper é token → device → `plataforma_principal` → `desconhecido` se há batida, senão `None` (`app/colaboradores/services/situacional.py:90`, `app/colaboradores/services/situacional.py:186`, `app/colaboradores/views.py:3034`).
- `montar_linha_colab` chama esse helper com três argumentos, omitindo token; a ficha do colaborador também chama o helper, mas obtém seu próprio token mais recente, independentemente dos dois montadores (`app/colaboradores/painel_cell.py:47`, `app/colaboradores/services/ficha.py:140`, `app/colaboradores/services/ficha.py:146`). A ficha transforma sua plataforma em texto de cabeçalho (`app/colaboradores/services/ficha.py:214`).
- O lavrador chama `montar_situacional` e ignora `_by_plat`; não lê `estado_app`, `plataforma` ou `token_plat`. Para agrupar/contar lê `colab.empresa_id`, `last_login`, `geo_tipo`, `tem_push`, `em_turno`, `justif_pend` e o tamanho da lista; não depende do formato exclusivo de `montar_linha_colab` (`app/core/management/commands/lavrar_placar_situacional.py:56`, `app/core/management/commands/lavrar_placar_situacional.py:59`, `app/core/management/commands/lavrar_placar_situacional.py:63`).
- Os leitores diretos de `montar_linha_colab` dependem das chaves do seu retorno: render da célula com `l`, testes GEO com `geo`, GEO/raio com `geo_tipo` e `geo`, LED com `led` (`app/colaboradores/painel_cell.py:81`, `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115`, `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148`, `app/colaboradores/tests/test_led_pelo_juiz.py:67`).
- Os testes de estado do app leem `estado_app` do serviço de lote: adesão retratada versus viva; uso no período retratado versus vivo e última retratada precedida de viva (`app/colaboradores/tests/test_adesao_pelo_juiz.py:48`, `app/colaboradores/tests/test_adesao_pelo_juiz.py:55`, `app/colaboradores/tests/test_estado_app_pelo_juiz.py:65`, `app/colaboradores/tests/test_estado_app_pelo_juiz.py:71`).
- O helper de plataforma tem testes próprios de precedência token/device/fóssil/desconhecido/None e testes de resolução sem token (`app/colaboradores/tests/test_pilula_plataforma.py:9`, `app/colaboradores/tests/test_plataforma_pwa.py:45`).

As três linhas pedidas são chamadas dentro de helpers, não asserções isoladas:

| Referência | O que a chamada extrai e o que os testes afirmam | [nao verificado]
|---|---|
| `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:115` | `_cores` congela o relógio da célula, extrai `geo`, chama lote para o mesmo colaborador com corte de 14 h, guarda o resumo e retorna as duas cores (`:105`, `:113`, `:119`, `:124`). Asserções: GPS incerto/sinal ruim/accuracy ausente não produzem vermelho; confiantemente fora continua vermelho e contador 1; dentro continua verde; sem coordenada é cinza (`:128`, `:149`, `:162`, `:181`, `:196`, `:215`). |
| `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:148` | `_pino` congela relógio, busca colaborador atualizado, monta célula e lote com corte de 14 h e retorna pares `(geo_tipo, geo)` (`:137`, `:146`, `:151`, `:154`). Asserções relevantes: raio zero usa padrão, não acusa batida a 50 m/accuracy 10; contador `fora_posto == 0`; raio 100 declarado continua acusando 300 m/accuracy 10; pino e escritor concordam nos casos definidos (`:159`, `:175`, `:190`, `:199`, `:214`). |
| `app/colaboradores/tests/test_led_pelo_juiz.py:67` | `_led` congela `painel_cell.timezone.now` e extrai somente `led` (`:56`, `:66`). Afirma que última batida E com turno expirado não pinta amarelo; turno realmente aberto continua amarelo; ausência de batidas produz cinza (`:69`, `:96`, `:114`). Não chama o montador de lote (`:56`). |

Existe selo explícito das duas raízes **dos montadores GEO**: O200 compara célula/lote em cinco combinações `(metros, accuracy)`; O200c compara o par inteiro `(geo_tipo, geo)` em três raios × três combinações de batida, nove cenários. Isso não verifica paridade de todas as chaves nem de `estado_app`/`plataforma` (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:204`, `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:235`).

Há também a regra e o selo genérico das duas **cascas HTML**, diferentes das duas funções: a regra aponta para o teste que verifica se `core/_dialogo.html` é alcançável por includes a partir de `base.html` e `base_app.html`; o teste seguinte conta exatamente um arquivo dono de `id="hx-confirma-fundo"` (`CLAUDE.md:200`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:115`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:121`).

## 06/11 18:38 · sessao 94c497bc14a3 · - · - · origem=Ronald (tmux)

**Chamadores diretos de `montar_linha_colab` (nenhum passa `maps`; assinatura `c, maps=None`):**

| Chamador | Argumentos / contexto | Referência |
|---|---|---|
| `_cell_colab_status(request, ident)` | `c`, obtido por `pk=ident`, com `select_related('empresa','posto','usuario')`; sem corte/agora/usuário explícitos | `app/colaboradores/painel_cell.py:75`, `:77`, `:81` |
| `O200...::_cores` | `self.c`; relógio congelado em `agora=timezone.now()`; lê `['geo']` | `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:105`, `:111`, `:113`, `:115` |
| `O200c...::_pino` | `Colaborador.objects.get(pk=self.c.pk)`; relógio congelado em `agora`; lê `geo_tipo` e `geo` | `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:137`, `:144`, `:146`, `:148`, `:154` |
| `LedPeloJuizTest::_led(agora)` | `self.c`; patch de `painel_cell.timezone.now` com o argumento `agora`; devolve `['led']` | `app/colaboradores/tests/test_led_pelo_juiz.py:56`, `:65`, `:66`, `:67` |

Os três testes importam localmente o módulo `painel_cell` e chamam o atributo, sem apelido; o registro em `ColaboradoresConfig.ready` apenas importa o módulo e registra o renderizador, sem chamar o montador. `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:111`; `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:144`; `app/colaboradores/tests/test_led_pelo_juiz.py:65`; `app/colaboradores/apps.py:9`; `app/colaboradores/painel_cell.py:74`.

**Chamadores diretos de `montar_situacional` — produção:**

| Chamador | Argumentos / universo | Referência |
|---|---|---|
| `painel_situacional`, ramo com placar | `list(colabs_qs[:30]), corte, agora, f_plataforma` | `app/colaboradores/views.py:2659`, `:2664` |
| `painel_situacional`, ramo sem placar | `colabs_qs, corte, agora, f_plataforma`; monta tudo, entrega primeiras 30 | `app/colaboradores/views.py:2679`, `:2680` |
| `painel_situacional_linhas`, com filtro plataforma | lista de objetos dos IDs retornados por `ids_do_recorte(qs,f_plataforma)`, cortados por `offset:offset+100`; `corte, agora, ''` | `app/colaboradores/views.py:2758`, `:2797`, `:2801` |
| `painel_situacional_linhas`, sem filtro plataforma | `list(qs[offset:offset+100]), corte, agora, ''` | `app/colaboradores/views.py:2804` |
| `lavrar_placar_situacional.Command.handle` | lista de todo `colaboradores_em_operacao().select_related('empresa','posto','usuario')`, `corte=agora−7 dias`, `agora=timezone.now()`; plataforma omitida | `app/core/management/commands/lavrar_placar_situacional.py:44`, `:48`, `:49`, `:54`, `:56` |
| `auditar_invariantes_chamados.Command.handle`, S7 | lista de todo `colaboradores_em_operacao().select_related(...)`, `_corte7=_agora7−7 dias`, `_agora7=_tz_s7.now()`; plataforma omitida; só quando existe placar global com idade ≤300s | `app/chamados/management/commands/auditar_invariantes_chamados.py:1202`, `:1204`, `:1210`, `:1212`, `:1213`, `:1214`, `:1216` |

Os imports do montador nas views, no lavrador e no auditor são locais e sem `as`; `_tz_l` e `_tz_s7` são apelidos de `timezone`, não do montador. `app/colaboradores/views.py:2645`, `:2749`, `:2752`; `app/core/management/commands/lavrar_placar_situacional.py:44`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1200`, `:1212`.

**Chamadores diretos — testes:** [nao verificado]

| Chamador | Argumentos | Referência |
|---|---|---|
| `SituacionalPerguntaEmLoteTest::_monta(n)` | `_qs(n)` (primeiros `n` colaboradores da fixture, `select_related`, ordenados nome/pk), `corte=agora−7 dias`, `agora=timezone.now()`; import no topo sem apelido | `app/colaboradores/tests/test_situacional_pergunta_em_lote.py:29`, `:83`, `:87`, `:90` |
| `SituacionalPeloJuizTest::_situ` | `Colaborador.objects.filter(pk=self.c.pk)`, `agora−7 dias`, `agora`; import local sem apelido | `app/colaboradores/tests/test_situacional_pelo_juiz.py:19`, `:20`, `:22` |
| O200 `_cores` | queryset de `self.c.pk` com `select_related`, `agora−14h`, `agora`; mesmo agora usado no patch da célula | `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:112`, `:113`, `:116`, `:119` |
| O200c `_pino` | queryset de `self.c.pk` com `select_related`, `agora−14h`, `agora`; mesmo agora usado no patch da célula | `app/colaboradores/tests/test_o200c_raio_de_um_sitio.py:145`, `:146`, `:149`, `:151` |
| Adesão `_estados` | queryset dos dois colaboradores `retratado/vivo`, `corte=agora−14h`, `agora`; import no topo sem apelido | `app/colaboradores/tests/test_adesao_pelo_juiz.py:18`, `:48`, `:50` |
| Estado app `_estados` | queryset dos três `retratado/vivo/misto`, `corte=_dias(30)`, `agora=timezone.now()`; import no topo sem apelido | `app/colaboradores/tests/test_estado_app_pelo_juiz.py:23`, `:65`, `:66` |
| `test_placar_global_bate_com_a_soma_das_linhas` | lista do universo em operação com `select_related`, `corte=agora−PERIODO_PADRAO_DIAS`, `agora`; import local sem apelido | `app/core/tests/test_selo_placar_situacional.py:80`, `:83`, `:87`, `:89`, `:91` |

Nenhuma dessas chamadas passa usuário ao montador: a assinatura recebe apenas `colabs_qs,corte,agora,f_plataforma=''`; o escopo vem do queryset/lista preparado pelo chamador. `app/colaboradores/services/situacional.py:65`.

**Origem dos tempos, escopo e rotas:**

- Carga: `agora=timezone.now()`; `periodo='hoje'` significa últimas 24h, `'30d'` significa 30 dias e os demais valores dão 7 dias. Acesso por `login_required` e `tem_acao(user,'ver_painel_situacional')`; filtros empresa, praça persistida em `PreferenciaUI`, busca por nome/CPF/matrícula; universo parte de `colaboradores_em_operacao()`, com ordem nome/pk. `app/colaboradores/views.py:2552`, `:2562`, `:2566`, `:2569`, `:2584`, `:2592`, `:2594`, `:2618`, `:2622`, `:2658`.
- Lote recalcula seu próprio `agora=_tz_l.now()` e corte com a mesma regra de período a cada requisição; usa `login_required` e gate `is_superuser or is_staff`; praça vem do GET ou preferência, empresa do GET, busca por nome/CPF — aqui não inclui matrícula. `app/colaboradores/views.py:2739`, `:2754`, `:2764`, `:2771`, `:2779`, `:2784`, `:2786`.
- Carga `/colaboradores/painel-situacional/` e lote `/colaboradores/painel-situacional/linhas/` são montados por prefixo e rotas. `app/config/urls.py:26`; `app/colaboradores/urls.py:81`, `:82`.
- `_cell_colab_status` é registrado em `'colab-status'`, recebe `(request, ident)`; rota universal `/live-cell/<tipo>/<ident>/` resolve o registro e chama `fn(request,ident)`. URL concreta: `/live-cell/colab-status/<pk>/`. O alias de import relevante aqui é `live_cell as live_cell_view`. `app/colaboradores/painel_cell.py:74`, `:75`; `app/core/live_cell.py:18`, `:19`, `:22`; `app/core/urls.py:4`, `:16`; `app/config/urls.py:39`.
- A célula busca o colaborador só pelo PK, com `select_related`; não passa filtros do painel, corte, agora ou usuário ao montador. Se não encontra, retorna `None`, convertido em HTTP 204. `app/colaboradores/painel_cell.py:77`, `:79`, `:81`; `app/core/live_cell.py:23`.
- Cada `<tr>` declara `data-cell="colab-status:<pk>" data-cell-every="45"`; o JS faz fetch nessa URL, cria um `tbody` temporário e substitui o elemento antigo inteiro por `replaceChild(novo,el)`. Portanto substitui a linha inteira, incluindo atributos e todas as células; HTTP 204 remove a linha. `app/templates/colaboradores/partials/_ps_linha.html:1`; `app/static/js/live-cell.js:10`, `:38`, `:40`, `:46`, `:50`.
- Os 45s são cadência base com jitter de 0,75–1,25 e atualização condicionada a visibilidade e teto de seis requisições simultâneas; não é garantia de atualização exata a cada 45s. `app/static/js/live-cell.js:11`, `:17`, `:58`, `:61`, `:69`, `:76`.
- O painel chama `LiveCell.scan()` ao carregar; nos lotes, a sentinela usa `hx-swap="outerHTML"` para substituir a própria sentinela pelo lote de `<tr>` e nova sentinela. `app/templates/colaboradores/partials/painel_situacional.html:306`, `:327`; `app/templates/colaboradores/partials/_ps_lote.html:11`, `:12`.

Não executei views, testes, comando de lavra ou acesso a banco/logs; resultado de execução em produção permanece [nao verificado]. [nao verificado]

## 06/11 18:38 · sessao 35fb1de5f07d · DIA-SEM-TRABALHO-NA-FRENTE · sonda · origem=Code

O corpo de `sonda_dst.py` não aparece no texto recebido nem foi localizado na cópia. Portanto, a conformidade das chamadas e a implementação exata de `por_que`/`pelo_colab` ficam **[não verificado]**.

**1) Literais**

Transcrição da saída fornecida, sem reexecução ou confirmação independente. O marcador **[não verificado]** aplica-se a todo o bloco. Grupos com menos de três exemplos foram preservados como recebidos. **[não verificado]**

```text
U_D_BASE sem_porta classe d n=217 omitidos=0
U_D n=173 (CAUSA-D 11:2x: 165+5=170) fora={'BATIDA': 3, 'CADASTRO': 14, 'ESTRUTURA_mas_dia_de_trabalho': 27}
U_A n=170 (chat 17:4x PROD: 175) | INTERSECAO mesmo colab+dia=0 | so_D=173 so_A=170 | UNIAO=343
LINHAS 343 == UNIAO 343 True
QUADRO (por_que, batidas, porta, status pede decisao?, segura folha?) -- soma=343
GRUPO|n=156|folga_do_calendario ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 156}|em={'D': 156}
   ex col144 2026-09-21 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-23 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=10/2026
GRUPO|n=113|folga_do_calendario ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 15, 'CADASTRO': 71, 'ESTRUTURA': 27}|em={'A': 113}
   ex col60 2026-11-03 clausula=trabalha_False fer=False/True/False aus=[] n_bat=3 orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=3 orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col97 2026-11-02 clausula=trabalha_False fer=False/None/None aus=[] n_bat=2 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=28|feriado_sem_trabalha_em_feriado ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'CADASTRO': 21, 'ESTRUTURA': 7}|em={'A': 28}
   ex col43 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=2 orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col203 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=4 orfas=4 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col207 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=2 orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=13|folga_do_calendario ; bat1 ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 12, 'ESTRUTURA': 1}|em={'A': 13}
   ex col49 2026-11-04 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col69 2026-10-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col300 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=8|feriado_trab_feriado_True_mas_folga ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 8}|em={'D': 8}
   ex col144 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col209 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col268 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=0 orfas=0 porta=None status=folga resolver=False comp=11/2026
GRUPO|n=5|ausencia_lancada ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 5}|em={'D': 5}
   ex col137 2026-11-03 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col137 2026-11-05 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col292 2026-10-22 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(5110, 'atestado', 'por_outro', 'sem_chamado')] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=5|feriado_trab_feriado_True_mas_folga ; bat2+ ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'ESTRUTURA': 1, 'BATIDA': 1, 'CADASTRO': 3}|em={'A': 5}
   ex col366 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=4 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col788 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=8 orfas=8 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col868 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=4 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=2|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat0 ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 2}|em={'A': 2}
   ex col97 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=0 orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col97 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=0 orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=2|folga_do_calendario ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 2}|em={'D': 2}
   ex col348 2026-10-24 clausula=trabalha_False fer=False/True/False aus=[] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col348 2026-10-31 clausula=trabalha_False fer=False/True/False aus=[] n_bat=2 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=2|feriado_sem_trabalha_em_feriado ; bat1 ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 1, 'CADASTRO': 1}|em={'A': 2}
   ex col820 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col900 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=1 orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; sem_porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-01 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat2+ ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=2 orfas=2 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; sem_porta ; pede ; nao_segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-10-21 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat1 ; porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-02 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=1 orfas=1 porta=porta_pergunta status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; bat2+ ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-04 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=3 orfas=2 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ausencia_lancada_pelo_colab ; bat2+ ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col889 2026-10-25 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4670, 'atestado', 'pelo_colab', 'sem_chamado')] n_bat=4 orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=1|feriado_sem_trabalha_em_feriado ; bat0 ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col969 2026-10-12 clausula=trabalha_False fer=True/False/True aus=[] n_bat=0 orfas=None porta=None status=folga resolver=False comp=10/2026
GRUPO|n=1|folga_do_calendario ; bat2+ ; sem_porta ; nao_pede(ok) ; nao_segura|dono={'BATIDA': 1}|em={'A': 1}
   ex col975 2026-11-01 clausula=trabalha_False fer=False/True/False aus=[] n_bat=4 orfas=2 porta=None status=ok resolver=False comp=11/2026
MARGINAL|POR_QUE|{'feriado_sem_trabalha_em_feriado': 31, 'folga_do_calendario': 285, 'ata_tipo_dia_folga_sem_ausencia_cobrindo': 8, 'ausencia_lancada': 5, 'feriado_trab_feriado_True_mas_folga': 13, 'ausencia_lancada_pelo_colab': 1}
MARGINAL|BAT|{'2+': 157, '1': 19, '0': 167}
MARGINAL|PORTA|{None: 340, 'porta_pergunta': 3}
MARGINAL|STATUS|{'folga_trabalhada': 161, 'escala_suspeita': 5, 'irregular': 8, 'folga': 165, 'aberto': 3, 'ok': 1}
MARGINAL|PEDE_DECISAO|{False: 332, True: 11}
MARGINAL|RESOLVER_DIA|{False: 343}
MARGINAL|SEGURA|{False: 163, True: 180}
MARGINAL|DONO|{'CADASTRO': 104, 'BATIDA': 30, 'ESTRUTURA': 209}
MARGINAL|EM|{'A': 170, 'D': 173}
MARGINAL|CLAUSULA|{'trabalha_False': 329, 'ata_tipo_dia=folga': 8, 'ata_tipo_dia=ausencia': 6}
D_FOLGA_TRABALHADA n=161 com_bat=161 sem_porta=161 resolver_dia_True=0 (STATUS_QUE_PEDEM_DECISAO inclui? False)
C444 {'col': 'col444', 'dia': '2026-10-25', 'emp': 2, 'em': 'A', 'clausula': 'trabalha_False', 'tipo_dia_ata': 'folga', 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'aus': [], 'por_que': 'folga_do_calendario', 'n_bat': 4, 'n_orfas': 4, 'porta': None, 'status': 'folga_trabalhada', 'status_pede_decisao': False, 'resolver_dia': False, 'segura': False, 'comp': '11/2026', 'dono': 'ESTRUTURA'}
ERROS {}
CONCLUSAO_LIBERADA
```

**2) Funções, contratos e decisões próprias da sonda**

| Função e localização | Argumentos e contrato do código | A sonda respeita? |
|---|---|---|
| `dia_sem_trabalho(cel)` — `app/chamados/juizes.py:413` | Instância da célula, com `trabalha` e `ata`; retorna booleano. Ausência parcial recebe tratamento específico em `app/chamados/juizes.py:426`. | **[não verificado]** |
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` — `app/chamados/services/vigia_fio.py:542` | Empresas visíveis por padrão; recorte por interseção. `limite` não corta os dias `sem_porta`; linhas têm célula, colaborador, data, competência, rota, causa, detalhe e papel. `app/chamados/services/vigia_fio.py:545`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:551`. | **[não verificado]** |
| `InsumosDoDono.dono(self, data)` — `app/ponto/management/commands/e6_oraculo.py:172` | Data do dia consultado, sobre instância carregada para colaborador/janela. O construtor exige `colaborador, ini, fim, hoje` e `agora` por palavra-chave; demais alimentações são opcionais. `app/ponto/management/commands/e6_oraculo.py:114`, `app/ponto/management/commands/e6_oraculo.py:128`. | **[não verificado]** |
| `dia_das_batidas(colab, ini, fim, turnos=None)` — `app/ponto/turnos.py:2874` | Colaborador e janela apresentada; `turnos` é alimentação opcional. Retorna `{batida.pk: date}`, respeitando o dia do turno e a data local das não reclamadas. `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2895`, `app/ponto/turnos.py:2911`. | **[não verificado]** |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` — `app/ponto/turnos.py:2774` | Cinco argumentos separados; retorna interseção fechada de datas. Sem fim, estende até `fim` somente para `TIPOS_EM_ABERTO`; nos demais tipos, dia único. `app/ponto/turnos.py:2775`, `app/ponto/turnos.py:2779`. | **[não verificado]** |
| `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1341` | Recebe **ID da célula**; alimentação opcional é a de `alimentar_portas(lote)`. Retorna `None` ou dados da porta. `app/chamados/juizes.py:1342`, `app/chamados/juizes.py:1370`. | **[não verificado]** |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` — `app/ponto/status_dia.py:221` | Pares `(colaborador, data)`; retorna mapa por `(colaborador_id, data)`. A carga deve conter datas e colaboradores; `fatos` exige `carga`. `app/ponto/status_dia.py:222`, `app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:259`. | **[não verificado]** |
| `resolver_dia_oferecido_nas_datas(colaborador, datas)` — `app/colaboradores/services/calendario.py:117` | Várias datas do mesmo colaborador; lê o campo do calendário e retorna `{date: bool}`; data não devolvida responde `False`. `app/colaboradores/services/calendario.py:118`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:135`. | **[não verificado]** |
| `o_que_segura(colab, mes, ano)` — `app/folha/fora_da_folha.py:203` | Colaborador e competência; retorna dias retentores, veredito e orientação. As datas da saída são strings. `app/folha/fora_da_folha.py:204`, `app/folha/fora_da_folha.py:227`. | **[não verificado]** |
| `competencia_do_fato(colaborador, quando)` — `app/ponto/services/fechamento.py:1586` | Fato de um dia; retorna `(mes, ano)`. Aceita `date` ou `datetime`; datetime consciente é localizado antes de extrair a data. `app/ponto/services/fechamento.py:1587`, `app/ponto/services/fechamento.py:1542`, `app/ponto/services/fechamento.py:1578`. | **[não verificado]** |

Sobre `por_que`:

- A regra existente para **dia sem trabalho** usa `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`. Na ausência parcial, somente `trabalha is False` mantém essa conclusão. Uma reprodução que omita essa exceção diverge da implementação. `app/chamados/juizes.py:422`, `app/chamados/juizes.py:426`, `app/chamados/juizes.py:428`.
- A causa **feriado que vira folga** já tem juiz: `feriado_vira_folga`. Exige flag de trabalho em feriado falsa, ciclo elegível e feriado no lugar do vínculo. Usar somente “é feriado + flag falsa” seria uma reprodução incompleta. `app/escala/models.py:1373`, `app/escala/models.py:1382`, `app/escala/models.py:1389`.
- `trabalha_em_feriado=True` coexistir com folga não contradiz a regra: a transformação por feriado somente altera `base is True`, preservando a folga que já existia. `app/escala/models.py:1367`.
- Os nomes apresentados podem servir como rótulos de agrupamento; determinar se o script apenas rotula, repete predicados ou altera a classificação exige seu corpo. **[não verificado]**

Sobre `pelo_colab`: já existe identificação de autoria pelo vínculo entre `ausencia.registrado_por.colaborador` e `ausencia.colaborador_id`. Ela integra a guarda de aprovação sem documento para tipos que exigem anexo; não certifica origem via disputa nem decisão administrativa. `app/ponto/services/ausencia.py:627`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:630`. Se o teste da sonda reproduz essa identificação, ou usa outro critério, fica **[não verificado]**.

**3) Guarda do emissor, outros escritores e consumidores do status**

Em `motivos_a_perguntar`, a ordem é:

1. `dia_sem_trabalho(cel)`; somente se falso, pelo curto-circuito do `or`, testa colaborador diferente de `ativo`. Qualquer resultado verdadeiro devolve `[]`. `app/chamados/services/fabrica_da_celula.py:91`.
2. Dia futuro: `cel.data > hoje`. `app/chamados/services/fabrica_da_celula.py:93`.
3. Define `agora`, se necessário, e calcula `vencidos_no_instante`, antes da próxima guarda. `app/chamados/services/fabrica_da_celula.py:95`, `app/chamados/services/fabrica_da_celula.py:98`.
4. Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM`. `app/chamados/services/fabrica_da_celula.py:99`.
5. `parcial_so_lista`, usando ata e julgamento anterior recebido ou da célula. `app/chamados/services/fabrica_da_celula.py:102`.
6. Carrega perguntas do mesmo colaborador/data, excluindo `VIA_DUPLICATA`, e elimina motivos já existentes da saída de `motivos_pela_ata`. `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`.

Dentro de `motivos_pela_ata`, vencidos vazios calam; `_ata_do_dia` que não permite também cala; sem ata e com conjunto de vencidos, cala; sem ata e `vencidos=None`, consulta `classificar_falta`; com ata, seleciona os marcos vencidos. `app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:59`.

Assim, uma célula com `trabalha is False` já é recusada na **primeira guarda**, mesmo havendo batidas; esse predicado não consulta `status='folga_trabalhada'`. `app/chamados/juizes.py:428`, `app/chamados/services/fabrica_da_celula.py:91`. Que as células medidas tinham exatamente o estado declarado na saída permanece **[não verificado]**.

**Sim, existem outros caminhos de nascimento sem essa guarda global:**

- **Chamado da célula:** `detectar_par_relampago.Command.handle` pode chamar `ChamadoColaborador.abrir`, após suas próprias condições, com módulo `par_relampago` e `data_turno`. A porta resolve a célula antes da gravação. `app/ponto/management/commands/detectar_par_relampago.py:352`, `app/ponto/management/commands/detectar_par_relampago.py:368`, `app/ponto/management/commands/detectar_par_relampago.py:383`, `app/chamados/models.py:913`, `app/chamados/models.py:932`.
- As guardas centrais de absolvição e veto alcançam conjuntos específicos de módulos que não incluem `par_relampago`; portanto não constituem veto geral a nascimento em folga. `app/chamados/models.py:1073`, `app/chamados/models.py:1097`, `app/ponto/services/furos_vetados.py:74`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`.
- **Pergunta:** a API pode acrescentar `foto_ausente`, registrar a última batida e chamar `criar_ou_atualizar_disputa`; este chama `gerar_perguntas_disputa`, que pode derivar a data da última batida e chamar `PerguntaDisputa.abrir`. Esse gate resolve a célula e cria a pergunta sem consultar `dia_sem_trabalho`. `app/api/views_core.py:469`, `app/api/views_core.py:474`, `app/api/views_core.py:772`, `app/api/views_core.py:778`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:350`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/models.py:2430`, `app/chamados/models.py:2476`.

**Nascer não basta para ser porta alcançável:** chamado da célula precisa estar na fila; pergunta precisa estar viva, no app, sob âncora na fila e passar pelo teste de resposta não vazia. `app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1380`. A existência efetiva dessas portas nos 161 casos não é demonstrada; a saída fornecida declara todos sem porta. **[não verificado]**

Além de `resolver_dia_oferecido`, o consumidor direto encontrado de `STATUS_QUE_PEDEM_DECISAO` é o teste que importa, confere a tupla e itera seus valores. `app/chamados/tests/test_o261_vigia_fio.py:901`, `app/chamados/tests/test_o261_vigia_fio.py:902`, `app/chamados/tests/test_o261_vigia_fio.py:903`. O uso de produção está em `resolver_dia_oferecido`; a tupla não contém `folga_trabalhada`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:104`.

**4) Ausência do próprio colaborador via disputa**

O leitor é **`decididos_do_periodo`**, que monta `{data: cartao_do_dia}`. Sua consulta não exige autor admin, aprovação ou chamado de origem; expande as datas do lançamento e deixa o mais recente sobrescrever. Portanto, a autoria pelo próprio colaborador não impede que o dia

[truncado em 20000 de 21951 caracteres]

## 06/11 18:35 · sessao c724227316a6 · - · - · origem=Ronald (tmux)

`motivos_a_perguntar` recusa nesta ordem:

1. `dia_sem_trabalho(cel)`; pelo `or` com curto circuito, só depois testa colaborador diferente de `ativo`. Qualquer um devolve `[]`. (`app/chamados/services/fabrica_da_celula.py:89-92`)
2. Dia futuro (`cel.data > hoje`), devolvendo `[]`. (`app/chamados/services/fabrica_da_celula.py:93-94`)
3. Define `agora` quando ausente e calcula `vencidos_no_instante`; esse cálculo antecede a próxima guarda. (`app/chamados/services/fabrica_da_celula.py:95-98`)
4. Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:99-100`)
5. `parcial_so_lista`, usando a ata e o julgamento anterior recebido ou da própria célula. (`app/chamados/services/fabrica_da_celula.py:101-104`)
6. Retira motivos já presentes em perguntas do mesmo colaborador/data, excluindo somente `VIA_DUPLICATA`; retorna motivos de `motivos_pela_ata` ainda ausentes. (`app/chamados/services/fabrica_da_celula.py:118-126`)

Dentro de `motivos_pela_ata`, conjunto vazio de vencidos cala; `_ata_do_dia` que não permite cala; sem ata e com conjunto de vencidos também cala; sem ata e `vencidos=None` consulta `classificar_falta`; com ata, só seleciona marcos vencidos. (`app/chamados/services/fabrica_da_celula.py:45-64`)

`dia_sem_trabalho` aceita ausência parcial como exceção: nela, só `cel.trabalha is False` cala; nos demais casos, cala por `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`. (`app/chamados/juizes.py:422-428`)

Há outros escritores possíveis sem essa guarda global; portanto o silêncio desse emissor não demonstra impossibilidade de outra porta. Um exemplo concreto de chamado da célula é `detectar_par_relampago.Command.handle`: após suas guardas próprias (`par_completo`, dry-run/retratador), chama `ChamadoColaborador.abrir` com módulo `par_relampago` e `contexto_json.data_turno`; não consulta `dia_sem_trabalho` nesse caminho. (`app/ponto/management/commands/detectar_par_relampago.py:327`, `app/ponto/management/commands/detectar_par_relampago.py:352-385`)

O catálogo declara `par_relampago` como fato do dia, chave `data_turno`, dono supervisão; `abrir` resolve a célula correspondente antes de gravar. (`app/chamados/catalogo/modulos.py:113-114`; `app/chamados/models.py:913-932`) As guardas centrais não proíbem todo nascimento em folga: `_celula_que_absolve` só alcança `MODULOS_DE_TURNO`; `_veto_da_regua` só alcança a união `FURO_MODULES + MODULOS_DE_TURNO`, listas que não incluem `par_relampago`. (`app/chamados/models.py:1073-1076`; `app/chamados/models.py:1097-1099`; `app/ponto/services/furos_vetados.py:74-81`; `app/chamados/catalogo/furo_modulos.py:15-16`; `app/chamados/reconciliador.py:336-337`) Assim, há possibilidade estática de um chamado desse módulo nascer para uma folga trabalhada que satisfaça o detector; ocorrência nos 161 dias da amostra: **[não verificado]**.

Também há nascimento de pergunta por outro caminho: a API acrescenta `foto_ausente` conforme `selfie_ausente`, registra `ultima_batida` e chama `criar_ou_atualizar_disputa`; esse helper chama `gerar_perguntas_disputa`, que deriva a data local da última batida e chama `PerguntaDisputa.abrir`. (`app/api/views_core.py:459-475`; `app/api/views_core.py:769-780`; `app/chamados/services/disputa_emissao.py:605-618`; `app/chamados/services/disputa_emissao.py:350-364`; `app/chamados/services/disputa_emissao.py:389-396`) O gate da pergunta resolve a célula pelo dia e efetua o `create`, sem guarda `dia_sem_trabalho`. (`app/chamados/models.py:2398-2477`) `foto_ausente` tem pergunta no catálogo. (`app/chamados/catalogo/perguntas.py:94-97`)

Nascimento de pergunta não garante porta alcançável: ela ainda precisa estar viva pelo C6, no app, sob âncora na fila, sem resposta vazia quando respondida; chamado da célula precisa estar na fila. (`app/chamados/juizes.py:1341-1369`; `app/chamados/juizes.py:1375-1386`) O C6 pode encerrar pela célula, mas resposta textual ainda não validada tem precedência quando não houve decisão explícita do admin. (`app/chamados/juizes.py:1135-1162`; `app/chamados/juizes.py:1180-1196`) Logo, possibilidade de outro escritor: **sim, condicional**; porta efetivamente nascida para algum dos 161 casos: **[não verificado]**.

A busca global do identificador encontrou somente um leitor de produção de `STATUS_QUE_PEDEM_DECISAO`: `resolver_dia_oferecido`, em `app/colaboradores/services/calendario.py:104`; o outro consumidor é o teste, que importa, confere a tupla e itera seus status. (`app/chamados/tests/test_o261_vigia_fio.py:901-903`) A tupla contém `falta`, `irregular`, `aberto`, `sem_certidao`, sem `folga_trabalhada`. (`app/colaboradores/services/calendario.py:94`)

Nenhum `AGENTS.md` foi encontrado nos diretórios ancestrais consultados nem sob `app`; existência fora desses lugares: **[não verificado]**.

## 06/11 18:35 · sessao 03448364f99c · - · - · origem=Ronald (tmux)

Contratos encontrados:

| Função | Contrato documentado |
|---|---|
| `dia_das_batidas(colab, ini, fim, turnos=None)` | `app/ponto/turnos.py:2874`. Recebe colaborador e janela apresentada; `turnos` é alimentação opcional. Retorna `{batida.pk: date}`: batida reclamada pertence ao dia do turno; outra pertence à sua data local. `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2895`, `app/ponto/turnos.py:2911`. |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` | `app/ponto/turnos.py:2774`. Retorna lista de datas na interseção com `[ini,fim]`; ausência sem fim estende até `fim` somente para `TIPOS_EM_ABERTO`, senão ocupa o dia único. `app/ponto/turnos.py:2775`, `app/ponto/turnos.py:2779`. |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` | `app/ponto/status_dia.py:221`. `pares` contém `(colaborador,data)`; retorna `{(colaborador_id,data):dict}`. `carga` deve conter datas e colaboradores; `fatos` só pode ser passado com `carga`. `app/ponto/status_dia.py:222`, `app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:259`. |
| `o_que_segura(colab, mes, ano)` | `app/folha/fora_da_folha.py:203`. Retorna os dias que retêm o colaborador naquela competência, com veredito, motivo, fio, destrava e chamado. As datas retornadas são strings. `app/folha/fora_da_folha.py:204`, `app/folha/fora_da_folha.py:227`. |
| `competencia_do_fato(colaborador, quando)` | `app/ponto/services/fechamento.py:1586`. Retorna `(mes,ano)` do fato de um dia; aceita `date` ou `datetime`, normalizando datetime consciente para a data local. `app/ponto/services/fechamento.py:1587`, `app/ponto/services/fechamento.py:1542`, `app/ponto/services/fechamento.py:1578`. |

A conformidade das chamadas da sonda com esses contratos é **[não verificado]**: o texto fornecido contém a saída, mas não o corpo de `sonda_dst.py`; tampouco localizei esse arquivo na cópia **[não verificado]**.

Para avaliar `por_que`:

- `dia_sem_trabalho` já decide por duas cláusulas: `trabalha is False` ou `ata.tipo_dia` presente e diferente de `trabalho`; ausência parcial conserva cobrança, salvo `trabalha is False`. `app/chamados/juizes.py:413`, `app/chamados/juizes.py:426`.
- Identificar feriado apenas por `trabalha_em_feriado=False` repetiria incompletamente a regra existente: `feriado_vira_folga` exige também ciclo elegível e feriado no lugar do vínculo. `app/escala/models.py:1373`, `app/escala/models.py:1382`, `app/escala/models.py:1389`.
- `feriado_trab_feriado_True_mas_folga` não demonstra contradição: a transformação por feriado só altera `base is True`, preservando folga já existente; a precedência também admite feriado vencer folga. `app/escala/models.py:1367`, `app/ponto/precedencia.py:17`, `app/ponto/precedencia.py:376`.
- Ausência, feriado, folga e trabalho já têm classificação e precedência próprias na produção; o vencedor é obtido ordenando os fatos por `PRECEDENCIA`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`.
- Sem o script, dizer se `por_que` apenas agrupa ou reimplementa/contradiz essa regra permanece **[não verificado]**.

## 06/11 18:34 · sessao e15b5cdc899a · - · - · origem=Ronald (tmux)

O leitor central é **`decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)`**, que declara responder “houve decisão humana neste dia?”, inclusive para rejeitadas; sua consulta filtra colaborador e interseção com a janela, **sem exigir autor admin, aprovação ou chamado de origem**. Portanto, uma ausência registrada pelo próprio colaborador alcança esse leitor se sua data/período intersectar a janela. Ele expande o intervalo e monta `cartao_do_dia`; o lançamento mais recente vence. (`app/ponto/services/dia_decidido.py:529`, `:532`, `:547`, `:550`, `:557`, `:561`.)

O cartão inclui `veredito`, `cor`, `rotulo`, minutos, tipo, status, ID, quem e quando; `quem_quando` prefere quem aprovou/recusou e usa quem registrou como alternativa. Logo, não exclui o próprio colaborador na autoria. (`app/ponto/services/dia_decidido.py:500`, `:506`, `:512`, `:516`.)

A ligação com a tela do admin existe:

- Calendário chama `decididos_do_periodo` e `cobertura_ausencia_periodo(..., para='folha')`; passa ambos para `do_dia` e publica `palavra_dia`, `cor_dia`, `veredito_dia`. O template mostra o rótulo no chip de ausência ou “O que foi decidido neste dia: …”. (`app/colaboradores/services/calendario.py:531`, `:533`, `:534`, `:715`, `:724`; `app/templates/colaboradores/partials/_calendario_grade.html:97`, `:98`.)
- Espelho chama o mesmo leitor para a janela, transporta `_decisao_do_dia` e entrega-o a `do_dia`. (`app/ponto/services/espelho.py:647`, `:649`, `:871`, `:328`, `:340`.)
- Histórico do drawer lê `linha_do_tempo`, que consulta todas as ausências do colaborador, exclui filhas de intervalo e distingue pendentes; o template imprime rótulo, quem e quando. (`app/ponto/views.py:2014`, `:2015`; `app/ponto/services/dia_decidido.py:566`, `:587`, `:593`, `:610`; `app/templates/ponto/partials/painel_ausencia.html:184`, `:188`.)

**Limite:** alcançar `decididos_do_periodo` não demonstra que houve aprovação do admin. Essa função não filtra status, e `cartao_do_dia` calcula o veredito pelo tipo, salvo rejeitada; `do_dia` trata pedido pendente separadamente quando recebe `pendente`. (`app/ponto/services/dia_decidido.py:550`, `:453`, `:461`, `:468`, `:420`.) Para aprovada, a cobertura de folha entra primeiro; pendência deve dizer “aguardando decisão”. (`app/ponto/turnos.py:2514`; `app/ponto/services/dia_decidido.py:363`, `:224`, `:420`.)

No dado fornecido, `col889/2026-10-25` tem ausência `4670`, tipo `atestado`, `pelo_colab`, `sem_chamado`; **status da ausência, datas inicial/final, origem efetiva via disputa, aprovação, rótulo renderizado e presença efetiva na tela não constam**. [não verificado] O código admite alcançar esse dia, porém a sonda não mediu esse leitor. [não verificado]

Sobre `pelo_colab`, já existe uma regra equivalente de identificação de autoria: `getattr(ausencia.registrado_por, 'colaborador', None)` e comparação de seu `pk` com `ausencia.colaborador_id`; ali ela serve especificamente à guarda de aprovação de tipo que exige documento sem anexo. Não prova origem via disputa nem decisão administrativa. (`app/ponto/services/ausencia.py:627`, `:628`, `:630`, `:632`.)

A materialização via disputa usa `registrado_por=user`, `chamado_origem=p.chamado_destino` e descrição da disputa; a documentação distingue falta aprovada e atestado cujo estado inicial é decidido pelo juiz. Assim, ausência nascida via disputa não implica decisão do admin, e um destino vazio pode produzir `chamado_origem=None`. (`app/chamados/services/materializacao.py:495`, `:498`, `:499`, `:520`, `:525`, `:526`, `:527`.)

## 06/11 18:26 · sessao eeb4262ba705 · DIA-SEM-TRABALHO-NA-FRENTE · sonda · origem=Code

**1) Transcrição literal**

Fonte deste bloco: o dado raspado da mensagem, sem arquivo numerado disponível para conferência [nao verificado]. Preservei os exemplos existentes; os grupos menores não trazem três exemplos. Não há linha `CONCLUSAO` no texto recebido: há `INCOMPLETO: 100 erros` [nao verificado].

```text
U_D_BASE sem_porta classe d n=217 omitidos=0
U_D n=173 (CAUSA-D 11:2x: 165+5=170) fora={'BATIDA': 3, 'CADASTRO': 14, 'ESTRUTURA_mas_dia_de_trabalho': 27}
U_A n=170 (chat 17:4x PROD: 175) | INTERSECAO mesmo colab+dia=0 | so_D=173 so_A=170 | UNIAO=343
LINHAS 343 == UNIAO 343 True
QUADRO (por_que, batidas, porta, status pede decisao?, segura folha?) -- soma=343
GRUPO|n=156|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 156}|em={'D': 156}
   ex col144 2026-09-21 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-23 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
GRUPO|n=126|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 27, 'CADASTRO': 71, 'ESTRUTURA': 28}|em={'A': 126}
   ex col49 2026-11-04 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-03 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=30|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'CADASTRO': 22, 'ESTRUTURA': 7, 'BATIDA': 1}|em={'A': 30}
   ex col43 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col203 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=4 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col207 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=8|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 8}|em={'D': 8}
   ex col144 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col209 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col268 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
GRUPO|n=5|ausencia_lancada ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 5}|em={'D': 5}
   ex col137 2026-11-03 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col137 2026-11-05 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col292 2026-10-22 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(5110, 'atestado', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=5|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'ESTRUTURA': 1, 'BATIDA': 1, 'CADASTRO': 3}|em={'A': 5}
   ex col366 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col788 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=8 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col868 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=3|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 3}|em={'A': 3}
   ex col97 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col97 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col741 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=2|folga_do_calendario ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 2}|em={'D': 2}
   ex col348 2026-10-24 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col348 2026-10-31 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=2|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 2}|em={'A': 2}
   ex col741 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
   ex col866 2026-11-04 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-01 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; nao_segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-10-21 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-02 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=aberto resolver=False comp=11/2026
GRUPO|n=1|ausencia_lancada_pelo_colab ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col889 2026-10-25 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4670, 'atestado', 'pelo_colab', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=1|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col969 2026-10-12 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=None porta=None status=folga resolver=False comp=10/2026
GRUPO|n=1|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(ok) ; nao_segura|dono={'BATIDA': 1}|em={'A': 1}
   ex col975 2026-11-01 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=2 porta=None status=ok resolver=False comp=11/2026
MARGINAL|POR_QUE|{'feriado_sem_trabalha_em_feriado': 31, 'folga_do_calendario': 285, 'ata_tipo_dia_folga_sem_ausencia_cobrindo': 8, 'ausencia_lancada': 5, 'feriado_trab_feriado_True_mas_folga': 13, 'ausencia_lancada_pelo_colab': 1}
MARGINAL|BAT|{'ERRO': 343}
MARGINAL|PORTA|{None: 340, 'porta_pergunta': 3}
MARGINAL|STATUS|{'folga_trabalhada': 161, 'escala_suspeita': 5, 'irregular': 8, 'folga': 165, 'aberto': 3, 'ok': 1}
MARGINAL|PEDE_DECISAO|{False: 332, True: 11}
MARGINAL|RESOLVER_DIA|{False: 343}
MARGINAL|SEGURA|{False: 163, True: 180}
MARGINAL|DONO|{'CADASTRO': 104, 'BATIDA': 30, 'ESTRUTURA': 209}
MARGINAL|EM|{'A': 170, 'D': 173}
MARGINAL|CLAUSULA|{'trabalha_False': 329, 'ata_tipo_dia=folga': 8, 'ata_tipo_dia=ausencia': 6}
D_FOLGA_TRABALHADA n=161 com_bat=0 sem_porta=161 resolver_dia_True=0 (STATUS_QUE_PEDEM_DECISAO inclui? False)
C444 {'col': 'col444', 'dia': '2026-10-25', 'emp': 2, 'em': 'A', 'clausula': 'trabalha_False', 'tipo_dia_ata': 'folga', 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'aus': [], 'por_que': 'folga_do_calendario', 'n_bat': 'ERRO', 'n_orfas': 4, 'porta': None, 'status': 'folga_trabalhada', 'status_pede_decisao': False, 'resolver_dia': False, 'segura': False, 'comp': '11/2026', 'dono': 'ESTRUTURA'}
ERRO|dia_das_batidas|col43|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col49|2026-11-04|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col60|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col69|2026-10-25|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col97|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col123|2026-11-05|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col130|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col137|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col144|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col203|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col207|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col209|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col211|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col227|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col242|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col268|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col270|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col278|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col282|2026-11-01|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col292|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col300|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col301|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col312|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col328|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col348|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col354|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col366|2026-10-27|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col369|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col373|2026-10-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col384|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERROS {'dia_das_batidas:TypeError': 100}
INCOMPLETO: 100 erros
rc_sombra=0
```

**2) Funções, contratos e decisões da sonda**

O corpo de `sonda_dst.py` não está disponível no material consultável; sua identificação/hash na saída não permite conferir suas chamadas. Portanto, os argumentos efetivamente passados a **cada uma** das funções abaixo são [nao verificado].

| Função e localização | Contrato documentado/implementado | Sonda cumpre? |
|---|---|---|
| `dia_sem_trabalho(cel)` — `app/chamados/juizes.py:413-428` | Recebe célula; lê `trabalha` e `ata`. Ausência parcial tem tratamento próprio. | [nao verificado] |
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` — `app/chamados/services/vigia_fio.py:542-554` | Censo de dias retidos sem porta; `empresas=None` usa empresas visíveis. `limite` não corta os dias sem porta. | [nao verificado] |
| `InsumosDoDono.dono(self, data)` — `app/ponto/management/commands/e6_oraculo.py:172-183` | Recebe a data; utiliza os insumos da instância. O construtor recebe colaborador, `ini`, `fim`, `hoje` e exige `agora=` — `:113-145`. | [nao verificado] |
| `dia_das_batidas(colab, ini, fim, turnos=None)` — `app/ponto/turnos.py:2874-2897` | Janela apresentada; `turnos=` é alimentação. Retorna **`{batida.pk: date}`**, não contagem. | [nao verificado] |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` — `app/ponto/turnos.py:2774-2790` | Retorna datas no intervalo fechado; ausência sem fim estende até `fim` somente para `TIPOS_EM_ABERTO`. | [nao verificado] |
| `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1341-1370` | Recebe **ID da célula**; alimentação opcional vem de `alimentar_portas(lote)`. Retorna `None` ou dicionário de rota/fila/verbos/fio. | [nao verificado] |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` — `app/ponto/status_dia.py:221-247` | Pares `(colaborador, data)`; carga opcional deve conter pessoas e datas; `fatos` somente com carga. Validações em `:259-267`. | [nao verificado] |
| `resolver_dia_oferecido_nas_datas(colaborador, datas)` — `app/colaboradores/services/calendario.py:117-138` | Datas de um mesmo colaborador; retorna `{date: bool}` lendo o calendário. Dia ausente do calendário responde `False`. | [nao verificado] |
| `o_que_segura(colab, mes, ano)` — `app/folha/fora_da_folha.py:203-231` | Recebe colaborador e competência; retorna dicionário com os dias retidos e seus motivos/fios. | [nao verificado] |
| `competencia_do_fato(colaborador, quando)` — `app/ponto/services/fechamento.py:1586-1588` | Fato de um dia; retorna `(mes, ano)`. Normaliza `date`/`datetime` pelo caminho `:1542-1583`. | [nao verificado] |

O erro impresso comprova apenas a mensagem de comparação entre `date` e `datetime`; sem a chamada e o traceback, atribuí-lo definitivamente a argumentos errados da sonda seria [nao verificado]. O código chama o pareador quando `turnos` não foi alimentado e contém comparação de datas de segmento com `data_turno`. (`app/ponto/turnos.py:2900-2905,1990-1993`)

**`com_bat=0` não comprova “zero batidas”**: a própria saída registra `MARGINAL|BAT|{'ERRO': 343}`; a contagem correta das batidas permanece [nao verificado].

Quanto às decisões próprias:

- **`por_que`:** a implementação e sua ordem de precedência são [nao verificado]. O juiz existente não devolve esses motivos: devolve um booleano por `trabalha is False` ou tipo de ata preenchido diferente de `trabalho`, com exceção para cobertura parcial. Tratar os nomes impressos como causas oficiais extrapolaria esse contrato. (`app/chamados/juizes.py:413-428`)
- **`folga_do_calendario`:** `trabalha=False`, isoladamente, não documenta a causa da folga; o próprio código explica que a célula guarda esse booleano sem guardar o porquê. O rótulo pode servir ao agrupamento, mas sua derivação pela sonda é [nao verificado]. (`app/escala/models.py:1373-1384`)
- **Os dois rótulos de feriado:** `trabalha_em_feriado=True` pode coexistir com folga; essa flag impede a transformação por feriado, mas não transforma uma folga já existente em trabalho. Portanto, o nome `feriado_trab_feriado_True_mas_folga` não demonstra contradição. A regra completa também considera ciclo e lugar do vínculo; um teste apenas da flag seria insuficiente. (`app/escala/models.py:1367-1371,1382-1395`)
- **`ausencia_lancada` e `ata_tipo_dia_folga_sem_ausencia_cobrindo`:** a ata não laboral já participa do juiz; a lista de ausências não é requisito adicional desse predicado. Se a sonda usa essa lista somente para subdividir os resultados, é agrupamento; se redecide o booleano com ela, a equivalência é [nao verificado]. (`app/chamados/juizes.py:422-428`)
- **`pelo_colab`:** já existe teste de identidade entre o colaborador de `registrado_por` e o colaborador da ausência. Se a sonda compara `registrado_por_id` com `colab.usuario_id`, repete essa identidade; a comparação efetivamente escrita é [nao verificado]. Essa identidade não prova origem via disputa nem aprovação. (`app/ponto/services/ausencia.py:627-631`; `app/colaboradores/models.py:351`; `app/chamados/services/materializacao.py:495-528`)

**3) Guarda do emissor, outros escritores e leitores da constante**

`motivos_a_perguntar` executa nesta ordem:

1. Recusa por `dia_sem_trabalho(cel)`; somente se falso, pelo curto-circuito do `or`, verifica colaborador diferente de `ativo`. (`app/chamados/services/fabrica_da_celula.py:89-92`)
2. Recusa data futura. (`app/chamados/services/fabrica_da_celula.py:93-94`)
3. Obtém `agora`, calcula marcos vencidos e recusa veredito preenchido fora de `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:95-100`)
4. Recusa ausência parcial destinada somente à lista administrativa, por `parcial_so_lista`. (`app/chamados/services/fabrica_da_celula.py:101-104`)
5. Carrega perguntas existentes, exclui duplicatas e devolve somente motivos ainda não perguntados. O cálculo dos motivos aplica o conjunto de marcos vencidos e as guardas de ata. (`app/chamados/services/fabrica_da_celula.py:118-126,45-64`)

Para as linhas cuja cláusula impressa é `trabalha_False`, **a primeira guarda basta**, independentemente de o status ser `folga_trabalhada`. (`app/chamados/juizes.py:428`; `app/chamados/services/fabrica_da_celula.py:91-92`; correspondência efetiva das células com a impressão: [nao verificado])

**Sim, existem caminhos fora dessa fábrica que podem criar chamado da célula ou pergunta:**

- **Chamado da célula:** a abertura administrativa aceita tipo catalogado e data do fato; por exemplo, `ausencia_colab` tem escopo DIA e chave `data`. A criação resolve a célula por colaborador/data. As guardas dessa criação não constituem proibição universal de `folga_trabalhada`. (`app/chamados/services/abertura.py:147-153,172-192`; `app/chamados/catalogo/modulos.py:120-122`; `app/chamados/models.py:1026-1039,1073-1076,1097-1106,1193-1201`)
- **Pergunta por batida em folga:** o detector devolve anomalia grave quando a escala diz folga; a API encaminha o motivo à disputa; o helper gera perguntas. A data é extraída do contexto e `PerguntaDisputa.abrir` associa a célula daquele colaborador/data, sem consultar `dia_sem_trabalho` nesse nascimento. (`app/chamados/detector_anomalias.py:164-175`; `app/api/views_core.py:725-730,771-780`; `app/chamados/services/disputa_emissao.py:350-366,389-397,598-618`; `app/chamados/models.py:2428-2430,2476-2477`)

**Nascer não basta para constituir porta alcançável:** o chamado precisa estar na fila; pela pergunta, ela precisa estar viva, no app e sob âncora na fila. Uma pergunta

[truncado em 20000 de 22672 caracteres]

## 06/11 18:25 · sessao 2da3cfdb4a00 · PAUTA-08 · verificador · origem=Code

1. **Fecha o achado da volta 2 sobre congelar o gravado até a gravação da pauta, mas não estabelece essa garantia para toda escrita da reversão.** O comando agora executa o `select_for_update` de `FechamentoMensal`, confere o hash e escreve cabeça, filhas e atualização da reversão dentro do mesmo `atomic` por colaborador. Não identifiquei caminho que persista uma cabeça nova sem essa conferência no mesmo bloco. ([comando:190](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:217))

   **Existe, porém, reversão gravada sem conferência no mesmo `atomic`: o JSON inicial.** Ele recebe o plano e os hashes medidos anteriormente, com `pautas=[]`, e é escrito antes do laço e da transação. O próprio teste de hash divergente abre esse arquivo após a recusa. ([comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:162](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:162), [comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [teste:211](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:211))

   O arquivo também continua fora do rollback do banco: falha no commit posterior à sua atualização pode deixar PKs mortos nele; `_reverter` pula PK inexistente. A comparação final ocorre depois das transações por colaborador. ([comando:214](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:214), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [comando:275](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:275))

2. **O teste novo morde presença e ordem do SQL, não toda a garantia anunciada.** Exige algum `FOR UPDATE` contendo `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`. Não verifica o filtro, linhas bloqueadas, lock anterior ao hash, duração do bloqueio ou concorrência. ([teste:225](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225), [teste:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228), [teste:231](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:231))

   **Mutante que passaria por análise estática:** mover somente o lock da linha 196 para depois do `if/raise` de hash, antes de `_existente`. Continua havendo lock antes do `INSERT`; o teste de hash adulterado continua recusando antes de criar pautas ou acrescentar PKs ao JSON. As demais verificações seriais preservam seus resultados. Execução dos 26 testes com esse mutante: **[nao verificado]**. ([comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:202](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [teste:204](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:217))

   Esse mutante reabre uma janela entre conferência e lock: uma alteração concorrente pode entrar ali, seguida da escrita da cabeça sem reconferência; o hash final só reclamaria depois da saída do `atomic`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:218), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228))

3. **Sim: a frase nova sobre HE autorizada em exportada corresponde ao código, condicionada a haver autorizações efetivas.** `_ato` chama automaticamente `_linha_para_o_dp` quando `exportada` e `autorizados` são verdadeiros; o emissor chama `pautas.services.escrever`. A confirmação envolve `_ato` em transação e recusa divergência do DIFF mostrado; a prévia desfaz sua transação. ([LEIS.md:177](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/docs/LEIS.md:177), [autorizacao_he_periodo.py:299](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:299), **[emissor:244](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:244)**, [prévia:321](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:321), [confirmação:343](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:343))

4. **Escritores e leitores no caminho examinado:**

   | Objeto | Quem escreve/produz | Quem lê |
   |---|---|---|
   | Hash do gravado | `hash_do_gravado` calcula MD5 truncado sobre campos de `FechamentoMensal`; o comando guarda o resultado no plano e no JSON. ([regen_exportada.py:62](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:62), [comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:165](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:165)) | O comando compara por colaborador e no fim. A porta REGEN calcula antes/depois, envia à auditoria e devolve `mudou`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [regen_exportada.py:206](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:206), [regen_exportada.py:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:218)) |
   | `FechamentoMensal` | PAUTA-08 pede somente leitura. No ramo escritor relacionado, `recalcular_fechamento_mes` usa `get_or_create`, `save` ou `update`; REGEN chama esse ramo. ([comando:135](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:889](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:889), [fechamento.py:1128](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1128), [fechamento.py:1144](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1144), [regen_exportada.py:179](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:179)) | `hash_do_gravado`, `valores_do_gravado`, recálculo que semeia objeto destacado e novo SELECT de bloqueio. ([regen_exportada.py:71](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:71), [regen_exportada.py:81](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:81), [fechamento.py:881](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:881), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196)) |
   | Arquivo de reversão | `_escrever` chama `_grava` inicialmente e após acrescentar PKs; `_grava` serializa em temporário e executa `os.replace`. ([comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [comando:216](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216), [comando:302](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:302)) | `_reverter` carrega JSON, valida marca/schema e percorre PKs para cancelar pautas; **não reconfere `hash_gravado` do plano**. ([comando:262](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:262), [comando:266](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:266), [comando:273](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:273), [comando:283](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:283)) |

   Ocorrência desses cenários em produção: **[nao verificado]**.

## 06/11 18:23 · sessao 88a1e5c18480 · - · - · origem=Ronald (tmux)

O teste novo **morde a presença e a ordem textual do SQL**, não a garantia inteira anunciada: procura qualquer `FOR UPDATE` em `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`; não verifica se o lock antecede o hash, quais linhas foram selecionadas ou uma disputa concorrente. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225`, `:228`, `:229`, `:231`)

**Mutante preferível:** mover somente o lock de `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` para depois da comparação/recusa de hash (`:199`–`:201`), antes de `_existente` (`:202`). O SQL continua antes do `INSERT` (`:207`), satisfazendo o novo teste; o teste do hash adulterado continua recusando antes das criações e preservando a lista vazia do JSON. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204`, `:213`, `:214`, `:217`, `:231`)

Esse mutante permite alteração concorrente **entre a conferência e a aquisição do lock**: a pauta pode ser criada sem nova conferência depois da alteração; a comparação final só ocorre após sair do `atomic` daquela pessoa. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190`, `:199`, `:207`, `:218`, `:228`)

Outro mutante explícito: acrescentar `pk__isnull=True` ao filtro do lock, gerando `SELECT … FOR UPDATE` sem travar linha alguma; o teste não inspeciona o filtro nem o resultado. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228`, `:231`)

A previsão de que esses mutantes passam pelos 26 testes decorre da inspeção estática das asserções; execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

## 06/11 18:23 · sessao a75e25bc3777 · - · - · origem=Ronald (tmux)

1. `motivos_a_perguntar` recusa na seguinte ordem:

   - Primeiro `dia_sem_trabalho(cel)`; por curto circuito, só consulta situação do colaborador se esse juiz devolver falso. Colaborador diferente de ativo também recusa. **`app/chamados/services/fabrica_da_celula.py:89-92`**.
   - Data futura. **`app/chamados/services/fabrica_da_celula.py:93-94`**.
   - Calcula `agora` e marcos vencidos; depois recusa veredito preenchido que não esteja em `CelulaDia.VEREDITOS_ACUSAM`. **`app/chamados/services/fabrica_da_celula.py:95-100`**.
   - Recusa parcial destinado só à lista administrativa (`parcial_so_lista`). **`app/chamados/services/fabrica_da_celula.py:101-104`**.
   - Consulta perguntas existentes, excetuando duplicatas, e só devolve motivos da ata ainda não perguntados. Teto dos marcos é aplicado dentro de `motivos_pela_ata`. **`app/chamados/services/fabrica_da_celula.py:118-126`**, **`:45-64`**.

   O juiz considera `trabalha is False` ou tipo de ata preenchido diferente de trabalho; ausência parcial deixa o restante cobrável, salvo `trabalha is False`. **`app/chamados/juizes.py:422-428`**. Logo, para os registros cuja cláusula é `trabalha_False`, a primeira guarda já explica a recusa; não depende do status `folga_trabalhada`. **`app/chamados/services/fabrica_da_celula.py:91`**, **`app/chamados/juizes.py:428`**.

2. **Há outros escritores; ausência de porta na medição não prova impossibilidade de emissão.**

   Caminho específico para batida em folga:

   - `detectar_batida_fora_escala` usa `esc.eh_dia_trabalho(data_turno_ref)` e devolve grave / `batida_em_folga` quando falso. **`app/chamados/detector_anomalias.py:164-175`**.
   - API converte grave em motivo `batida_fora_escala_grave` e chama `criar_ou_atualizar_disputa`, preenchendo também `ultima_batida.timestamp`. **`app/api/views_core.py:725-730`**, **`:771-780`**; mesmo código em `app/api/views.py`.
   - O helper abre/agrega disputa e chama o gerador de perguntas. **`app/chamados/services/disputa_emissao.py:598-618`**.
   - O gerador lê `ultima_batida.timestamp`, converte para data local e passa `data_evento` a `PerguntaDisputa.abrir`. **`app/chamados/services/disputa_emissao.py:350-366`**, **`:389-397`**.
   - A porta consulta a célula por colaborador/data e cria a pergunta com essa célula; não tem guarda `dia_sem_trabalho`. **`app/chamados/models.py:2428-2430`**, **`:2476-2477`**.
   - Nascer pergunta não garante porta alcançável: `porta_alcancavel_do_dia` ainda exige pergunta viva, no app e âncora na fila. **`app/chamados/juizes.py:1378-1381`**. Pergunta respondida com texto pode permanecer viva apesar de a célula não acusar, salvo decisão administrativa; pergunta nova sem resposta pode morrer pelo veredito não acusador. **`app/chamados/juizes.py:1150-1162`**, **`:1181-1196`**.

   Outro caminho concreto de **chamado da célula**: abertura administrativa no fio recebe tipo catalogado e data do fato, cria chamado com a chave de data; `criar` resolve a célula do colaborador/data. Exemplo de tipo permitido: `ausencia_colab`, de escopo DIA, chave `data`. **`app/chamados/services/abertura.py:147-153`**, **`:172-192`**; **`app/chamados/catalogo/modulos.py:120-122`**; **`app/chamados/models.py:1026-1028`**, **`:1193-1201`**. As guardas posteriores são absolvição de módulo de turno e veto da régua; não constituem veto universal a `folga_trabalhada`. **`app/chamados/models.py:1031-1039`**, **`:1073-1076`**, **`:1097-1106`**. O chamado só constitui porta se integrar a fila considerada pelo juiz. **`app/chamados/juizes.py:1375-1386`**.

   Outros emissores de cobrança possuem guardas diferentes: detector canônico chama `celula_veta_cobranca`; c2 também, e essa função veta `trabalha is False` sem chamar `dia_sem_trabalho`. **`app/ponto/management/commands/detectar_ausencias.py:630-633`**, **`app/chamados/reconciliador.py:876-880`**, **`app/ponto/marcos.py:154-163`**. Portanto não se deve dizer que “todo escritor passa por dia_sem_trabalho”. **Mesmas referências.**

3. `STATUS_QUE_PEDEM_DECISAO`: a busca em Python encontrou somente definição e uso produtivo em `resolver_dia_oferecido`; fora dele, leitura direta apenas no teste que importa, compara a tupla e a percorre. **`app/colaboradores/services/calendario.py:94`**, **`:103-104`**; **`app/chamados/tests/test_o261_vigia_fio.py:901-903`**. O vigia lê indiretamente a decisão pela porta plural do calendário. **`app/chamados/services/vigia_fio.py:263-271`**.

4. Não encontrei escritores chamados `extra_sem_aviso` ou `batida_orfa`; `batida_orfa` aparece como mapeamento legado de categoria para `orfao_14h`. **`app/chamados/models.py:3025-3026`**. Não usar os nomes como escritores provados. A ocorrência de `BATIDA_ORFA_FORA_TOLERANCIA` é protesto do suprajuiz. **`app/ponto/supra_juiz.py:518-522`**.

## 06/11 18:22 · sessao 4f61a43c87d5 · - · - · origem=Ronald (tmux)

Leitor existente e alcance:

- `app/ponto/services/dia_decidido.py:529` — `decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` responde `{date: cartao_do_dia}`. A consulta seleciona ausências do colaborador que tocam a janela, sem filtro por autor, status ou chamado (`:550-555`); expande cada ausência entre `data` e `data_fim or data`, recortada pela janela (`:558-562`). Assim alcança também ausência registrada pelo próprio colaborador, desde que o dia esteja nessa janela.
- `app/ponto/services/dia_decidido.py:512-526` — `cartao_do_dia` entrega veredito, cor, rótulo, minutos, tipo, status, ID, quem e quando. `quem_quando` prefere `aprovado_por`, depois `registrado_por`, e usa `aprovada_em or criado_em` (`:500-509`).
- A grade do admin carrega `decididos_do_periodo` em `app/colaboradores/services/calendario.py:531-534`, fornece a decisão a `do_dia` em `:715-720` e publica palavra/cor/veredito em `:724`. O template mostra a palavra e usa o tooltip “O que foi decidido neste dia” em `app/templates/colaboradores/partials/_calendario_grade.html:98`; se há ausência, o chip usa a palavra no título e abre o registro (`:97`).
- O espelho carrega o mesmo leitor em `app/ponto/services/espelho.py:647-649`, transporta `_decisao_do_dia` em `:871` e fornece a `do_dia` em `:340-345`.
- Atenção: alcance não garante que a palavra mostrada será “decidido/aprovado”. `do_dia` considera cobertura, decisão e pedido pendente (`app/ponto/services/dia_decidido.py:399-435`). Pedido pendente no espelho ganha palavra “aguardando decisão” (`:420-425`). Os dados raspados não trazem `Ausencia.status` da ausência 4670; aprovação e palavra efetiva para col889 são `[nao verificado]`.

Autoria:

- O teste de identidade já existe no gate de aprovação: `app/ponto/services/ausencia.py:627-631` verifica se `ausencia.registrado_por.colaborador.pk == ausencia.colaborador_id`. A relação inversa é sustentada por `Colaborador.usuario`, OneToOne de `auth.User`, `related_name="colaborador"` em `app/colaboradores/models.py:351`.
- Se a sonda compara `registrado_por_id == colab.usuario_id`, repete essa identidade existente; a chamada exata da sonda permanece `[nao verificado]`. Identidade do autor, sozinha, não comprova origem via disputa.
- A autodeclaração via disputa está documentada em `app/chamados/services/materializacao.py:495-508`; a criação passa `registrado_por=user` e `chamado_origem=p.chamado_destino` em `:520-528`. O caminho `declara_falta` passa `request_user` e `ch` em `:484-492`.
- Não encontrei `_quem_da_ausencia`; a função de autoria no módulo é `quem_quando`, `app/ponto/services/dia_decidido.py:500`.

Contratos das funções:

- `app/ponto/turnos.py:2774-2776` — `dias_da_ausencia(tipo, data, data_fim, ini, fim)` devolve lista de datas em intervalo fechado. Sem `data_fim`, estende até `fim` somente para `TIPOS_EM_ABERTO`; nos demais tipos usa um dia (`:2779-2789`).
- `app/ponto/turnos.py:2874-2897` — `dia_das_batidas(colab, ini, fim, turnos=None)` devolve **mapa `{batida.pk: date}`**, não lista/número. `ini/fim` são a janela apresentada; `turnos=` é alimentação. A implementação chama `turnos_do_colab` e `janela_com_pad` (`:2900-2905`).
- `turnos_do_colab` documenta intervalo por `data_turno` em `app/ponto/turnos.py:1848-1857`; o corpo compara `seg_ini <= t.data_turno <= seg_fim` em `:1993`. Um `datetime` misturado com `date` poderia falhar, mas sem a sonda/traceback a origem exata do TypeError é `[nao verificado]`.
- A busca por nome não encontrou `sonda_dst.py` na cópia; argumentos efetivamente utilizados pela sonda são `[nao verificado]`.

## 06/11 18:22 · sessao 5a6a06837842 · DIA-SEM-TRABALHO-PORTA · leitura · origem=Code

HASH LIDO = 7c140052a45f90bc52eb5d68726782428fe9307c [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| G01 | Aval literal: “(1) batida em FOLGA DO CALENDARIO -> porta de decisao do admin (folga trabalhada)”. | Colaborador `situacao='ativo'` (app/colaboradores/models.py:323). FolgaDia `escala_colaborador=V`, `data=05/10/2026`, `calendario_origem=F` (app/escala/models.py:1585; app/escala/models.py:1588; app/escala/models.py:1589). CelulaDia `colaborador=C`, `data=05/10`, `trabalha=False` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Batida `colaborador=C`, `timestamp=05/10 12h e 16h`, `tipo=E/S` (app/ponto/models.py:22; app/ponto/models.py:23; app/ponto/models.py:24); apresentação UTC−3 (CLAUDE.md:143). | O teste alcança uma ação do admin para decidir a folga trabalhada desse dia. Botão, URL, entidade criada e retorno obrigatório de uma função específica: **regra nao decide**. “Dia decidido” antes da decisão: **regra nao decide**. A distinção entre porta alcançável e “Resolver dia” está escrita em app/chamados/juizes.py:1344 e app/chamados/juizes.py:1367. | Tratar toda célula `trabalha=False` como motivo suficiente para eliminar também a decisão administrativa (guarda em app/chamados/services/fabrica_da_celula.py:91). | golden |
| G02 | Aval literal: “(2) batida em FERIADO sem trabalha_em_feriado -> porta de decisao”. | Colaborador e Batida como G01. Feriado `data=06/10/2026`, `abrangencia='nacional'` (app/core/models.py:12; app/core/models.py:13). EscalaColaborador `trabalha_em_feriado=False` (app/escala/models.py:896). CelulaDia `colaborador=C`, `data=06/10`, `trabalha=False` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Batidas em 06/10, mesmos campos de G01. | O teste alcança uma ação administrativa para decidir o trabalho nesse feriado. Botão, URL, entidade e retorno obrigatório de função específica: **regra nao decide**. “Dia decidido” antes da decisão: **regra nao decide**. Não se exige `fio='admin'` só pelo nome do ator: a escada de fios distingue validar de outros verbos de porta (app/chamados/juizes.py:1323). | Suprimir a decisão porque o feriado foi materializado como `trabalha=False` (app/chamados/juizes.py:413). | golden |
| G03 | Aval literal: “(3) dia com AUSENCIA LANCADA -> NENHUMA porta, mostrado como ‘dia decidido’”; representação da decisão em app/docs/PROMPTS.md:53. | CelulaDia `colaborador=C`, `data=07/10/2026` (app/escala/models.py:1681; app/escala/models.py:1685). Ausencia `colaborador=C`, `tipo='atestado'`, `data=07/10`, `data_fim=07/10`, `status='aprovada'` (app/ponto/models.py:743; app/ponto/models.py:749; app/ponto/models.py:750; app/ponto/models.py:751; app/ponto/models.py:799). Sem Batida para C nesse dia (app/ponto/models.py:22; app/ponto/models.py:23). | Nenhuma porta para decidir novamente esse dia, pelo aval. Grade, cartão e app mostram a decisão com palavra e efeito do catálogo, verde para abono; nunca só tick (app/docs/PROMPTS.md:53; app/ponto/catalogo/ausencias.py:95). Exigir literalmente o texto “dia decidido”, um status com esse nome ou um retorno específico: **regra nao decide**. | Oferecer nova decisão apesar da ausência aprovada; ou representar a decisão apenas por tick (app/ponto/services/dia_decidido.py:3). | golden |
| B01 | Aval literal: “batida cross-meia-noite que cai no dia de folga pela autoridade de dia ponto/turnos.py::dia_das_batidas”; dia reclamado pelo turno em app/ponto/turnos.py:2877. | Entrada G01, com CelulaDia `data=05/10` (app/escala/models.py:1685). Batida `timestamp=05/10 22h e 06/10 06h`, `tipo=E/S`, `colaborador=C` (app/ponto/models.py:23; app/ponto/models.py:24; app/ponto/models.py:22). Pré-condição: a autoridade atribui ambas a 05/10 (app/ponto/turnos.py:2875). | A ação administrativa exigida por G01 pertence a **05/10**, incluindo a saída de 06/10. O mapa de dia associa as duas identidades de batida a 05/10 (app/ponto/turnos.py:2877; app/ponto/turnos.py:2892). | Agrupar por `timestamp.date()` e perder a saída na avaliação da folga (app/ponto/turnos.py:2883). | borda |
| B02 | Batida reclamada pertence ao dia do turno, não ao dia civil da saída (app/ponto/turnos.py:2877). | CelulaDia `data=06/10`, `trabalha=False` (app/escala/models.py:1685; app/escala/models.py:1686). Batida `timestamp=06/10 06h`, `tipo='S'`, `colaborador=C` (app/ponto/models.py:23; app/ponto/models.py:24; app/ponto/models.py:22). Pré-condição: a autoridade reclama a saída para 05/10 (app/ponto/turnos.py:2877). | O mapa atribui a saída a 05/10. Essa saída não comprova trabalho na folga de 06/10; existência de outra causa de porta em 06/10: **regra nao decide** (app/ponto/turnos.py:2877). | Usar a data civil da saída como evidência de trabalho na folga seguinte (app/ponto/turnos.py:2883). | borda |
| B03 | Aval literal do feriado, combinado com `feriado_vira_folga`: ciclos weekday, flag falsa e praça do vínculo (app/escala/models.py:1373; app/escala/models.py:1382). | Entrada G02. TipoEscala `tipo_ciclo='5x2'` (app/escala/models.py:117; app/escala/models.py:29). EscalaColaborador `tipo_escala=T`, `posto=P`, `trabalha_em_feriado=False` (app/escala/models.py:874; app/escala/models.py:873; app/escala/models.py:896). Feriado cadastrado na praça de P, com `data`, `abrangencia`, `uf`, `municipio` (app/core/models.py:12; app/core/models.py:13; app/core/models.py:14; app/core/models.py:15). | `feriado_vira_folga` responde verdadeiro nas condições escritas; a ação de decisão de G02 continua exigida. A conversão para folga não elimina o fato de ser feriado (app/escala/models.py:1377; app/escala/models.py:1382). | Fazer a conversão para folga apagar a necessidade da decisão do feriado trabalhado (app/escala/models.py:1369). | borda |
| B04 | A conversão exige `trabalha_em_feriado=False` (app/escala/models.py:1382); o aval mínimo também contém essa condição. | Entrada B03, alterando apenas EscalaColaborador `trabalha_em_feriado=True` (app/escala/models.py:896). | `feriado_vira_folga=False`, pela condição escrita (app/escala/models.py:1382). Obrigatoriedade ou proibição de porta pelo trabalho nesse feriado: **regra nao decide** no aval desta obra. | Ignorar a flag e converter qualquer feriado em folga (app/escala/models.py:1390). | borda |
| B05 | Ausência autodeclarada via disputa: falta nasce aprovada, “confissao contra interesse: nada a decidir” (app/chamados/services/materializacao.py:495); aval literal do caso 3. | DisputaSupervisao `colaborador=C` (app/chamados/models.py:1666). PerguntaDisputa `disputa=D`, `celula=CD`, `data_evento=08/10`, `resposta_colab='FALTA_DIA'`, `respondida_em` e `validada_em` preenchidos (app/chamados/models.py:2211; app/chamados/models.py:2209; app/chamados/models.py:2218; app/chamados/models.py:2227; app/chamados/models.py:2228; app/chamados/models.py:2229). Ausencia resultante `tipo='falta'`, `status='aprovada'` (app/ponto/models.py:749; app/ponto/models.py:799). | A ausência nasce aprovada; a autoria da declaração não impede o dia decidido (app/chamados/services/materializacao.py:498). Nenhuma nova porta para decidir o mesmo dia, pelo aval. Exibição com palavra de falta, desconto e horas em vermelho (app/docs/PROMPTS.md:53; app/ponto/catalogo/ausencias.py:88). | Exigir aprovação adicional só porque a falta veio da resposta do próprio colaborador (app/chamados/services/materializacao.py:498). | borda |
| B06 | Atestado autodeclarado sem documento não nasce decidido (app/chamados/services/materializacao.py:499); ausência aberta conserva o chamado vivo e silenciado (app/chamados/services/materializacao.py:556). | Mesma disputa de B05, com PerguntaDisputa `resposta_colab='ATESTADO'` (app/chamados/models.py:2227). Ausencia `tipo='atestado'`, `status='aguardando_documento'` (app/ponto/models.py:749; app/ponto/models.py:799). | O lançamento aparece pendente, amarelo, sem antecipar abono aprovado (app/ponto/services/dia_decidido.py:37). O chamado não é fechado como ausência decidida (app/chamados/services/materializacao.py:560). Aplicação da frase genérica “AUSENCIA LANCADA -> NENHUMA porta” a esse estado aberto: **regra nao decide** no aval específico. | Tratar a mera existência de Ausencia como aprovação e fechar o chamado (app/chamados/services/materializacao.py:556). | borda |
| B07 | “rejeitada nunca cobre dia” (app/docs/PROMPTS.md:50); rejeição deve continuar visível (app/ponto/services/dia_decidido.py:11). | Entrada G03, alterando Ausencia `status='rejeitada'` (app/ponto/models.py:799). | A ausência não cobre folha nem suspende cobrança; a exibição informa o atestado rejeitado, em laranja (app/ponto/services/dia_decidido.py:11; app/docs/PROMPTS.md:53). Obrigatoriedade de uma porta de decisão do **dia** só por essa rejeição: **regra nao decide**. A ação “Reverter rejeicao” é prescrita separadamente (app/docs/PROMPTS.md:50). | Aplicar a ausência rejeitada como cobertura aprovada ou ocultar a rejeição (app/ponto/services/dia_decidido.py:11). | borda |
| B08 | Abono parcial cobra o resto; a lâmpada distingue marco dispensado de marco apagado (app/chamados/juizes.py:403). | CelulaDia `trabalha=True`, `ata` com `tipo_dia='ausencia'`, `cobertura_parcial=True` e marco restante apagado (app/escala/models.py:1686; app/escala/models.py:1778; definição dos carimbos em app/chamados/juizes.py:407). Ausencia `tipo='declaracao'`, `status='aprovada'` (app/ponto/models.py:749; app/ponto/models.py:799). | A parte descoberta continua cobrável; não se dispensa o dia inteiro pelo rótulo de ausência (app/chamados/juizes.py:403). Porta exata e momento de sua oferta: **regra nao decide** nesta cláusula. | Fazer toda `ata.tipo_dia='ausencia'` eliminar também a cobrança dos marcos descobertos (app/chamados/juizes.py:423). | borda |
| B09 | Competência padrão 21→20 (CLAUDE.md:81); saída do cross de fim recebe dia mesmo estando em fim+1 (app/ponto/turnos.py:2895). | Empresa `dia_inicio_competencia=21` (app/colaboradores/models.py:46). Entrada G01 parametrizada com CelulaDia `data=20/10` e `21/10` (app/escala/models.py:1685). No primeiro, Batida `timestamp=20/10 22h e 21/10 06h` (app/ponto/models.py:23), ambas atribuídas a 20/10 pela autoridade (app/ponto/turnos.py:2877). | A decisão do dia 20 pertence à competência 10; a do dia 21, à 11. A saída em 21/10 do turno de 20/10 permanece com o dia 20 (CLAUDE.md:81; app/ponto/turnos.py:2895). A exigência de porta dos casos mínimos não ganha exceção de competência no aval literal. | Recortar por mês civil ou descartar a saída porque seu timestamp ultrapassa o dia 20 (app/ponto/turnos.py:2888). | borda |
| B10 | Folga de calendário pertence ao vínculo e só vale nos dias dele; geradora manda dentro da vigência (app/docs/JUIZES.md:94; app/escala/alimentacao.py:272). | EscalaColaborador V antigo `data_inicio=01/09`, `data_fim=10/10`, `ativa=False`; V novo `data_inicio=11/10`, `ativa=True` (app/escala/models.py:875; app/escala/models.py:876; app/escala/models.py:877). FolgaDia `escala_colaborador=V antigo`, `data=05/10` (app/escala/models.py:1585; app/escala/models.py:1588). CelulaDia `escala_geradora=V antigo`, `data=05/10` (app/escala/models.py:1692; app/escala/models.py:1685), com batidas de G01. | A troca posterior não apaga a folga válida de 05/10 nem a decisão exigida por G01. O vínculo antigo responde por esse dia apesar de atualmente inativo (app/escala/alimentacao.py:272). | Consultar apenas o vínculo atualmente ativo e reclassificar o passado (app/escala/alimentacao.py:272). | borda |
| B11 | Geradora fora da vigência devolve `None`, dono CADASTRO, sem escolher outro vínculo (app/escala/alimentacao.py:272); L-099 (CLAUDE.md:176). | Entrada B10, mas CelulaDia `data=15/10`, `escala_geradora=V antigo` (app/escala/models.py:1685; app/escala/models.py:1692), cuja EscalaColaborador `data_fim=10/10` (app/escala/models.py:876). | A autoridade do vínculo devolve `None`; divergência com dono CADASTRO (app/escala/alimentacao.py:272). Classificação do dia como folga trabalhada e porta específica: **regra nao decide** para esse cadastro inválido. | Substituir silenciosamente a geradora inválida pelo vínculo novo (app/escala/alimentacao.py:277). | borda |
| B12 | O aval exige **batida** nos casos 1 e 2; todo dia não trabalhado leva palavra do veredito (app/ponto/services/dia_decidido.py:186). | Entrada G01 sem nenhuma Batida de C no dia (campos de seleção em app/ponto/models.py:22; app/ponto/models.py:23); sem Ausencia para C/data (app/ponto/models.py:743; app/ponto/models.py:750). | A folga é mostrada com palavra, conforme a regra de representação (app/ponto/services/dia_decidido.py:186). Oferecer ou proibir porta administrativa nessa folga sem batida: **regra nao decide** no aval. Não há ausência decidida fornecida na entrada. | Exibir somente ícone/tick para a folga, sem palavra (app/ponto/services/dia_decidido.py:186). | borda |
| B13 | Sem decisão humana, furo não vira falta decidida; dia previsto encerrado sem batida é irregular (app/ponto/status_dia.py:122); falta tem um significado, decidida (app/docs/LEIS.md:87). | CelulaDia `colaborador=C`, `data=09/10`, `trabalha=True` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Sem Batida para C/data (app/ponto/models.py:22; app/ponto/models.py:23), sem Ausencia para C/data (app/ponto/models.py:743; app/ponto/models.py:750). Dia encerrado como pré-condição da regra (app/ponto/status_dia.py:122). | O status não afirma falta decidida: é irregular nas condições escritas (app/ponto/status_dia.py:122). O furo aparece como dia em aberto, separado de falta decidida (app/docs/CORTES.json:77). | Transformar automaticamente ausência de batida em falta humana já decidida (app/ponto/status_dia.py:122). | borda |
| B14 | “ultimo recurso + passado + status que pede decisao” (app/colaboradores/services/calendario.py:89). | Item do calendário `data=05/11, 06/11 ou 07/11`, `status='irregular'`, `pode_ultimo_recurso=True` (app/colaboradores/services/calendario.py:722; app/colaboradores/services/calendario.py:726; app/colaboradores/services/calendario.py:738). Parâmetro `hoje=06/11` (app/colaboradores/services/calendario.py:97). | `resolver_dia_oferecido` é verdadeiro somente para 05/11; falso para hoje e futuro, pela exigência de passado (app/colaboradores/services/calendario.py:100). Existência de outra porta hoje: **regra nao decide** nessa regra do botão. | Trocar o limite de passado por inclusão de hoje (app/colaboradores/services/calendario.py:103). | borda |
| A01 | “chamar 2x com a mesma entrada = mesmo estado, 1 linha de trilha, 0 chamado/pergunta duplicada” (app/docs/ESTRUTURAL.md:22); aval exige “rodar 2x = mesma porta, 0 duplicata”. | Entrada G01 ou G02, repetida duas vezes, conservando os mesmos Colaborador, CelulaDia, FolgaDia/Feriado e Batida com os campos e definições dessas entradas. | Mesma porta lógica e mesmo estado final; uma linha de trilha do ato idempotente, zero chamados/perguntas duplicados (app/docs/ESTRUTURAL.md:22). Qual entidade materializa a porta: **regra nao decide** no aval desta obra. | Recriar a porta ou sua trilha a cada julgamento repetido (app/docs/ESTRUTURAL.md:22). | adversarial |
| A02 | L-099 exige determinismo e idempotência com o cadastro existente (CLAUDE.md:176); contrato de duplicatas em app/docs/ESTRUTURAL.md:22. | Entrada G01 entregue simultaneamente a duas execuções; identidades e campos dos models permanecem os mesmos de G01. | Ao término das duas execuções, a decisão continua alcançável e há zero chamados/perguntas duplicados (aval do caso 1; app/docs/ESTRUTURAL.md:22). Ordem de conclusão, bloqueio, isolamento e retorno transitório de cada execução: **regra nao decide**. | Permitir que duas execuções criem duplicatas para a mesma entrada (app/docs/ESTRUTURAL.md:22). | adversarial |
| A03 | Universo de apuração exclui retratada (CLAUDE.md:155); exibição conserva a marcação com asterisco (app/ponto/models.py:64). | Entrada G01 com as únicas Batida `retratada_em` preenchidas (app/ponto/models.py:67); `colaborador`, `timestamp`, `tipo` mantidos (app/ponto/models.py:22; app/ponto/models.py:23; app/ponto/models.py:24). | As batidas não entram no universo apurável; continuam na exibição de marcações com asterisco (CLAUDE.md:155; app/ponto/models.py:64). Existência ou destino histórico da porta após retratação: **regra nao decide** no aval. | Contar a batida retratada como trabalho apurável para acionar o caso de folga trabalhada (app/ponto/turnos.py:2847). | adversarial |
| A04 | Status e veredito são escritos no mesmo ato/save (app/docs/ADR/0002-celula-e-o-dia.md:44); evento julga, consumidor lê a lâmpada (CLAUDE.md:168). | CelulaDia `veredito`, `status_dia`, `status_rotulo` inicialmente nulos (app/escala/models.py:1770; app/escala/models.py:1816; app/escala/models.py:1817), com insumos de G01. Observar o estado publicado durante e após uma lavra. | A leitura persistida não apresenta veredito novo com status da lavra anterior: ambos pertencem ao mesmo save (app/docs/ADR/0002-celula-e-o-dia.md:44). Porta antes de existir lavra e ordem de atualização de outros artefatos: **regra nao decide**. | Gravar/publicar o veredito e só depois gravar o status em outro ato (app/docs/ADR/0002-celula-e-o-dia.md:44). | adversarial |
| A05 | Alterar ausência rejulga pelo outbox (app/docs/ADR/0002-celula-e-o-dia.md:16); aval literal do caso 3. | Entrada G03, porém legado com CelulaDia `ata={}`, `status_dia=None`, `status_rotulo=None` (app/escala/models.py:1778; app/escala/models.py:1816; app/escala/models.py:1817). Ausencia conserva `status='aprovada'` e período integral (app/ponto/models.py:799; app/ponto/models.py:750; app/ponto/models.py:751). | Depois de processado o evento de rejulgamento, vale G03: nenhuma porta para decidir novamente e palavra/efeito da ausência (aval do caso 3; app/docs/PROMPTS.md:53). Resposta antes de qualquer evento, migração automática e prazo de convergência: **regra nao decide**. | Fazer o caminho legado ignorar a ausência aprovada após o rejulgamento (app/docs/ADR/0002-celula-e-o-dia.md:16). | adversarial |
| A06 | Regeneração denuncia a alteração por campos próprios (CLAUDE.md:146); mudança de DNA rejulga pelo outbox (app/docs/ADR/0002-celula-e-o-dia.md:16); consumidor lê a lâmpada (CLAUDE.md:170). | Entrada G01 após primeira avaliação. CelulaDia muda `trabalha=False→True`, com `dna`, `regenerada_em`, `regeneracoes`, `dna_anterior` atualizados (app/escala/models.py:1686; app/escala/models.py:1697; app/escala/models.py:1718; app/escala/models.py:1723; app/escala/models.py:1726). | A regeneração fica identificável pelos campos de histórico e provoca novo julgamento dos insumos (CLAUDE.md:146; app/docs/ADR/0002-celula-e-o-dia.md:16). Consumidores leem a nova lâmpada (CLAUDE.md:170). Fechar, substituir ou conservar a porta anterior: **regra nao decide**. | Conservar indefinidamente o julgamento anterior apesar de mudar o DNA da célula (app/docs/ADR/0002-celula-e-o-dia.md:16). | adversarial |
| A07 | E-SEM-VIVA (B): desligado que nunca bateu segura a folha e vai à lista do admin com porta (app/docs/RELATO.md:60); recorte da competência em app/docs/BACKLOG.md:1799. | Colaborador `situacao='desligado'`, `data_demissao=31/10` (app/colaboradores/models.py:323; app/colaboradores/models.py:325). CelulaDia `colaborador=C`, `data=15/10`, `veredito='nunca_bateu'` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1770). Zero Batida de C na vida (app/ponto/models.py:22). Empresa `dia_inicio_competencia=21` (app/colaboradores/models.py:46); competência retida como pré-condição do corte (app/docs/BACKLOG.md:1799). | O desligamento não exclui esse dia anterior à demissão: a folha continua retida e o admin encont

[truncado em 20000 de 23185 caracteres]

## 06/11 18:19 · sessao 6911586370aa · - · - · origem=Ronald (tmux)

- Tabuleiro: “EVENTO carimba e julga na hora”; consumidor “le a lampada, nunca re-julga”. `CLAUDE.md:168-170`.
- Idempotência explícita: “chamar 2x com a mesma entrada = mesmo estado, 1 linha de trilha, 0 chamado/pergunta duplicada”. `app/docs/ESTRUTURAL.md:22-25`.
- L-099 exige coerência, determinismo e idempotência com o cadastro existente; divergência tem dono ESTRUTURA/CADASTRO/BATIDA; os dois últimos vão à lista do admin. `CLAUDE.md:176`; `app/docs/ESTRUTURAL.md:29-39`.
- E-SEM-VIVA (B): desligado que nunca bateu segura a folha e vai à lista do admin com porta; recorte “dias antes da demissão em comp que ele segura”. `app/docs/RELATO.md:60`; `app/docs/PROMPTS.md:1900`; `app/docs/BACKLOG.md:1799`.
- Definição escrita de porta: “o dia em que a tela oferece ‘Resolver dia’”, lida de `resolver_dia_oferecido`; fio vivo pode ser pergunta viva **ou** essa ação. Assim, observar a oferta de “Resolver dia” tem lastro; exigir criação de chamado/pergunta em todo caso não decorre dessa regra. `app/docs/CORTES.md:271`.
- Dia decidido: efeito abona=verde; desconta=vermelho com horas; suprime=cinza; rejeitado=laranja; hover quem/quando; “Nunca tick sem palavra”; vale grade/cartão/app. `app/docs/PROMPTS.md:53`. L-039: falta significa ausência lançada cujo tipo desconta, decidida. `app/docs/LEIS.md:87`.
- Ausência própria via disputa: docstring expressa `falta -> aprovada` (“confissão contra interesse: nada a decidir”); atestado sem documento fica aguardando documento; chamada fecha somente se ausência nasce decidida. `app/chamados/services/materializacao.py:495-508`. Comentário de regras S4 explicita ausência aberta mantém chamado vivo e silenciado; FALTA_DIA aprovada nasce decidida. `app/chamados/services/materializacao.py:556-561`.
- Rejeitada nunca cobre o dia. `app/docs/PROMPTS.md:50`. Perguntas absorvidas voltam a pendente quando a ausência é rejeitada. `app/docs/PROMPTS.md:1377`; `app/docs/RELATO.md:1703`.
- Pendente não representa efeito futuro aprovado; é amarelo e “ninguém decidiu ainda”. `app/ponto/services/dia_decidido.py:37-40`.
- Competência padrão 21→20, configurável por empresa; nunca presumir mês civil ou corte 21 invariável. `CLAUDE.md:81-82`; `app/docs/JUIZES.md:75`.
- Folga de calendário pertence ao vínculo e só vale nos dias dele; folga manual pertence à pessoa e sobrevive à troca. Geradora da célula manda; sem ela, vigência. `app/docs/JUIZES.md:94-103`.
- Retratada sai do universo apurável. `CLAUDE.md:155`.
- Estado intermediário: status e veredito devem ser escritos no mesmo ato/save; DNA, vínculo, batidas, ausência e feriado alterados devem provocar rejulgamento por outbox. `app/docs/ADR/0002-celula-e-o-dia.md:12-18`; `app/docs/ADR/0002-celula-e-o-dia.md:44-49`.
- Concorrência explicitamente escrita para consumidor outbox: idempotente e `FOR UPDATE SKIP LOCKED`. `app/docs/CORTES.md:351`. Isolamento/ordenação específica entre duas ofertas simultâneas da porta: [não verificado].
- Aval do instrumento Specification by Example e formato da tabela: `app/docs/RELATO.md:6736-6737`; `app/docs/BACKLOG.md:1812`.

Lacunas procuradas:

- Aval `DIA-SEM-TRABALHO-PORTA` registrado em arquivo: não encontrado nas buscas por nome exato nos documentos/cópia; usar corte literal fornecido pelo usuário. [não verificado]
- Regra específica para dados legados sem ata/status que determine obrigatoriamente oferta ou recusa da porta: [não verificado].
- Regra específica para ordenação observável durante processamento ainda pendente do outbox: [não verificado].
- Manifesto da cópia contendo hash informado: não encontrado; `.git` ausente e busca pelo hash completo sem resultado. O nome do diretório corresponde ao hash fornecido, mas identificação de commit por conteúdo não foi verificável. [não verificado] [nao verificado]

## 06/11 18:18 · sessao cf2c3d5b1b40 · - · - · origem=Ronald (tmux)

Referências adicionais para consolidar:

- `FolgaDia`: vínculo `app/escala/models.py:1585`, data `:1588`, calendário de origem `:1589`, tipo `:1594`; definição declara “foto materializada” de folga rígida de vínculo/data (`app/escala/models.py:1582`).
- `DisputaSupervisao`: colaborador `app/chamados/models.py:1666`, contexto `:1675`, chamado `:1679`, fechamento `:1687`; campo próprio `status` [não verificado].
- `PerguntaDisputa`: célula `app/chamados/models.py:2209`, disputa `:2211`, motivo `:2216`, data `:2218`, tipo de resposta `:2222`, resposta do colaborador `:2227`, respondida em `:2228`, validada em `:2229`, pendência administrativa `:2317`, motivo da recusa `:2321`, via `:2322`.
- Ausência declarada na própria resposta: regra escrita diferencia falta aprovada por confissão e atestado submetido ao juiz de estado; ausência aberta segura o chamado; operação idempotente (`app/chamados/services/materializacao.py:495`).
- Estados de ausência: aguardando documento, aguardando decisão, aprovada, rejeitada (`app/ponto/models.py:701`).
- Item do calendário: data `app/colaboradores/services/calendario.py:722`, status/ausência `:726`, último recurso `:738`; regra literal exige último recurso + passado + status lavrado que pede decisão (`app/colaboradores/services/calendario.py:89`).
- Tipo de ciclo: `app/escala/models.py:117`; ciclos e conjunto de ciclos em que feriado vira folga `app/escala/models.py:26`.
- Situação e datas do colaborador: `app/colaboradores/models.py:323`, `:324`, `:325`.

Cuidados normativos encontrados:

- Vínculo do dia: carimbo da geradora manda, mas somente dentro da vigência; inatividade atual não invalida dia coberto; fora da vigência é `None` e dono CADASTRO, sem escolher outro vínculo (`app/escala/alimentacao.py:272`). Geradora ausente da carga também não autoriza escolher outro (`app/escala/alimentacao.py:264`).
- Feriado: município/posto do vínculo que responde pelo dia, não posto atual do colaborador (`app/escala/models.py:1382`).
- Batida retratada fica fora da geometria/apuração; continua exibida com asterisco (`app/ponto/models.py:64`).
- Estado intermediário: materializa, depois julga dentro de transação; reprovação desfaz tudo, e carimbo sem lastro não sai (`app/chamados/services/materializacao.py:18`).
- Dia decidido deve mostrar o que foi decidido pelo efeito do catálogo; nunca apenas tick (`app/ponto/services/dia_decidido.py:3`). Rejeitada não cobre folha nem suspende cobrança, mas deve aparecer para colaborador e administrador (`app/ponto/services/dia_decidido.py:11`). Pendente é amarelo, sem antecipar efeito favorável do tipo (`app/ponto/services/dia_decidido.py:37`).

## 06/11 18:15 · sessao 0c5a4b131c42 · E-SEM-VIVA-B · verificador · origem=Code

**Não identifiquei defeito funcional novo no delta.** A fronteira inferior ganhou prova, o selo cobre `**{...}` literal e a docstring passou a distinguir ausência de fechamento novo de fechamento preexistente. Permanecem limitações do selo e dois problemas do contador do vigia. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`, `app/escala/services/cadastro_realidade.py:148`, `app/folha/vigia_retido_calado.py:43`.

Execução da suíte Django, sobrevivência à suíte integral e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **CURADO — o teste novo distingue os mutantes inferiores; os superiores sobrevivem a ele isoladamente.** [nao verificado]

   O filtro exige início da anterior inclusive e ontem inclusive. O fixture novo contém exatamente 20/09 e 21/09, exigindo somente 21/09. `app/escala/services/cadastro_realidade.py:176`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`.

   | Mutante simples | Teste novo isolado | Como provar | [nao verificado]
   |---|---|---|
   | Remover `data__gte` | **Morre:** devolve 20 e 21/09. | Fixture e expectativa em `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |
   | Trocar `gte` por `gt` | **Morre:** devolve vazio. | A única data permitida está exatamente no início inclusivo. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:280`. |
   | Usar início da atual | **Morre:** devolve vazio, pois o início passa a 21/10. | `app/escala/services/cadastro_realidade.py:174`, `app/ponto/janelas.py:27`. |
   | Remover `data__lte` | **Sobrevive:** continua devolvendo 21/09. | O fixture não contém hoje ou futuro. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |
   | Trocar `lte` por `lt` | **Sobrevive:** continua devolvendo 21/09. | O fixture não contém ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |

   **No conjunto dos testes ativos, esses cinco mutantes simples são distinguíveis:** `_ativo` cria ontem, 24/10, e hoje; `ESPERADO` inclui ontem e exclui hoje. Assim, retirar o limite superior inclui hoje; torná-lo estrito perde ontem. Isso foi conferido pelos predicados em memória, sem executar Django. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:261`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:269`.

2. **BAIXA — o selo AST continua com pontos cegos plausíveis.**

   A cura reconhece expansão de dicionário **literal**; não resolve expressões, variáveis nem chamadas transitivas. As sondas abaixo executaram `_veredito` extraído do código, em memória. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:308`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`.

   | Forma | Resultado e causa |
   |---|---|
   | `Q(data_demissao__isnull=False)` | **Detecta:** examina kwargs de qualquer chamada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`. |
   | `Q(**{"data_demissao__isnull": False})` | **Detecta:** cura do delta. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`. |
   | `Q(("data_demissao__isnull", False))` | **Não detecta:** não examina strings dentro da tupla posicional. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:329`. |
   | `kw={"data_demissao__isnull":False}; filter(**kw)` | **Não detecta:** exige `ast.Dict` diretamente na expansão. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`. |
   | `F("data_demissao")` | **Não detecta:** `F` está fora dos nomes examinados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296`. |
   | `values(dem=F("data_demissao"))` | **Não detecta:** o kwarg é `dem`; a referência dentro de `F` escapa. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`. |
   | `values("colaborador__data_demissao")` | **Não detecta:** compara a string posicional inteira, sem separar `__`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`. |
   | `from operator import attrgetter as ag; ag("data_demissao")(c)` | **Não detecta campo:** o detector usa o nome sintático `ag`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328`. |
   | `from folha import export as e; e.classificar_export(...)` | **Escapa:** registra módulo `folha`, e chamadas por atributo não entram em `chamadas`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:313`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322`. |
   | `import folha.export as e` | **Bloqueia pelo módulo.** `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:343`. |
   | `from folha.export import classificar_export as e; e(...)` | **Detecta o apelido.** `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322`. |
   | Helper externo lendo campo ou chamando juiz | **Não acompanha:** analisa somente o corpo selecionado de `colabs_nunca_bateu`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:311`. |

   **Relevância:** kwargs em variável, `F()` e helpers são caminhos plausíveis para introduzir futuramente um predicado paralelo na seleção de desligados. Não demonstram recomposição no corpo atual: ele seleciona desligados por `situacao` e cruza dias com o retrato. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:192`.

   **Como provar:** manter uma chamada descartada a `linhas_do_retrato` e acrescentar qualquer leitura cega acima; o requisito positivo continua satisfeito. O selo não prova fluxo até o retorno. Os testes sem retrato, divergência lavra/vivo e cadastro torto combatem alterações comportamentais específicas, mas não fecham todas essas formas. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:341`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`.

3. **SEM DIVERGÊNCIA DE CALENDÁRIO — lavrador e leitor concordam para o mesmo `hoje` e corte.**

   | Valor | Quem produz/escreve | Quem lê |
   |---|---|---|
   | Corte da empresa | Formulário valida/clampa; porta persiste. `app/colaboradores/views.py:495`, `app/colaboradores/views.py:523`, `app/colaboradores/services/estrutura.py:17`. | `corte_da_empresa`, com default 21 e clamp 2–28. `app/ponto/janelas.py:50`. |
   | Janela atual | `periodo_apuracao_atual` → `periodo_apuracao`. `app/ponto/janelas.py:24`. | `janela_atual`, usada por C5 e lavrador. `app/ponto/janelas.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/folha/management/commands/lavrar_retrato_folha.py:60`. |
   | Janela anterior | `janela_anterior`, perguntando pela janela do dia anterior ao início atual. `app/ponto/janelas.py:71`. | C5 lê `[0]` como limite inferior; C5 e lavrador leem `[1]` como competência. `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`, `app/folha/management/commands/lavrar_retrato_folha.py:61`. |
   | `(empresa, mes, ano)` do retrato | Lavrador usa mês/ano do **fim** de ambas as janelas e persiste. `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 constrói os mesmos pares e consulta a empresa. `app/escala/services/cadastro_realidade.py:184`, `app/escala/services/cadastro_realidade.py:185`. |
   | Janela do conteúdo lavrado | `dias_que_seguram` usa `janela_fechamento`; export delega ao mesmo juiz. `app/folha/fora_da_folha.py:118`, `app/folha/export.py:47`. | Retentor recebe essas bordas; universo do fechamento também usa esse juiz. `app/folha/fora_da_folha.py:137`, `app/ponto/services/fechamento.py:423`. |

   **Como provar:** a sonda das funções reais percorreu todos os dias de 2024–2028 e cortes 2–28: 49.329 combinações, sem divergência de contenção, continuidade ou reconstrução da janela pelo mês/ano do fim. O código trata `dia >= corte` e dezembro→janeiro explicitamente. `app/ponto/janelas.py:15`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:30`, `app/ponto/janelas.py:71`.

   Exemplos, corte 21: em 20/10/2026 selecionam competências 09 e 10; em 21/10, 10 e 11; em 21/12, 12/2026 e 01/2027; em 01/01/2027 permanece esse último par. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:66`.

   **Limite da conclusão:** isso não certifica contemporaneidade da lavra. O command usa data real ou competência explícita; C5 aceita `hoje` arbitrário e lê o retrato atualmente gravado, sem seleção histórica por `lavrado_em`. No corte, antes da nova lavra, uma competência pode ainda não ter retrato; o desligado fica sem esses dias, conforme a política implementada. `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/management/commands/lavrar_retrato_folha.py:55`, `app/folha/management/commands/lavrar_retrato_folha.py:74`, `app/escala/services/cadastro_realidade.py:169`, `app/escala/services/cadastro_realidade.py:185`.

4. **MÉDIA — remanescente: o vigia perde competência e dias ao declarar roteamento.**

   **O que está errado:** transforma `{colaborador: datas}` em conjunto de IDs e aplica esse conjunto a todas as competências selecionadas. Uma C5 positiva somente na anterior pode retirar de `calados` a linha da corrente. A unidade pessoa é declarada, mas o resultado também é apresentado por competência. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:54`.

   **Como provar:** sonda em memória do corpo real de `censo`, com dois retratos, mesmo colaborador e fio `ninguem`, mas C5 contendo somente 05/10: resultado `bloqueados=2`, `calados=0`, `roteados=2`, um roteado em cada competência. Isso comprova o mecanismo do contador, sem executar ORM. `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   O cenário pode ser anterior com `nunca_bateu` e corrente retida por `sem_celula`, sem porta: o retentor produz esse segundo tipo de dia, enquanto C5 lê somente células `nunca_bateu`. Ocorrência em produção: **[nao verificado]**. `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/escala/services/cadastro_realidade.py:175`.

5. **MÉDIA — remanescente: “roteado à lista” pode referir empresa excluída da tela.**

   **O que está errado:** vigia e C5 aceitam retrato/empresa inativa; a tela e as próximas lavras exigem `empresas_visiveis`. Portanto, o contador pode declarar encaminhamento a uma lista que não exibe aquela empresa. `app/folha/vigia_retido_calado.py:33`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/colaboradores/queries.py:85`.

   **Como provar:** lavrar empresa visível com C5 positiva, torná-la inativa e consultar `censo([id])`. O retrato permanece selecionável, mas a empresa sai da tela e do lavrador. A sonda do contador real com retrato de empresa inativa e C5 positiva devolveu `roteados=1`, `calados=0`, `cego=0`; o caminho completo em Django e sua ocorrência em produção são **[nao verificado]**. `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:56`, `app/colaboradores/queries.py:85`.

6. **INFORMATIVO — resumo dos demais escritores/leitores e mudança desde a rodada 2.**

   | Campo/valor consumido ou produzido | Quem escreve | Quem lê |
   |---|---|---|
   | `Colaborador`: ID, empresa, situação | Criação; edição/transferência da empresa; desligamento/reativação da situação. `app/colaboradores/services/colaborador.py:91`, `app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1386`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`. | C5 filtra empresa, identifica desligados e associa IDs. `app/escala/services/cadastro_realidade.py:175`, `app/escala/services/cadastro_realidade.py:179`. |
   | `CelulaDia`: colaborador/data e `veredito='nunca_bateu'` | Gerador cria identidade do dia; cartório decide o veredito pela porta de lavra. `app/ponto/portas/celula.py:633`, `app/ponto/services/cartorio.py:1241`, `app/ponto/portas/celula.py:814`. | C5 lê veredito, empresa, datas e ID. `app/escala/services/cadastro_realidade.py:175`. |
   | Retrato: `empresa`, `mes`, `ano`, `linhas`, `lavrado_em` | `lavrar_retrato_folha --apply`. `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 seleciona chave; vigia seleciona último dia de lavra e relata competências; porta lê JSON. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:50`, `app/folha/retrato_folha.py:110`. |
   | JSON: `colab`, `status`, `motivo` | Export decide status/motivo; montador copia ID e valores. `app/folha/export.py:962`, `app/folha/retrato_folha.py:1178`. | `_balde` interpreta status/motivo; C5 exige `furo_espelho`; vigia lê bloqueados. `app/folha/retrato_folha.py:89`, `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:46`. |
   | JSON: `datas` de furo | Retentor → `dias_que_seguram` → conversão ISO → montador. `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1183`. | C5 une datas e compara com `d.isoformat()`; recorte temporal também as lê. `app/escala/services/cadastro_realidade.py:190`, `app/escala/services/cadastro_realidade.py:192`, `app/folha/retrato_folha.py:1242`. |
   | JSON: `dono`, contendo fio | `_fio_cru`, chamado pelo montador. `app/folha/retrato_folha.py:364`, `app/folha/retrato_folha.py:1186`. | `fio_da_linha`, usado pelo vigia para selecionar `ninguem`. `app/folha/retrato_folha.py:428`, `app/folha/vigia_retido_calado.py:47`. |
   | `{cid: datas}` → assinatura `C5` e URL | C5 monta retorno; formatador monta assinatura; `acao` fornece URL. `app/escala/services/cadastro_realidade.py:194`, `app/escala/services/cadastro_realidade.py:202`, `app/escala/services/cadastro_realidade.py:657`. | `lista` incorpora assinatura; vigia reduz retorno a IDs. `app/escala/services/cadastro_realidade.py:833`, `app/folha/vigia_retido_calado.py:43`. |
   | `hoje`, contadores, `cego` e alarmes | Command fornece data; `censo` calcula resultados em memória. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:56`. | C5 calcula ontem/janelas; formatador e command imprimem/avaliam alarme. `app/escala/services/cadastro_realidade.py:170`, `app/folha/vigia_retido_calado.py:60`, `app/folha/management/commands/vigiar_retido_calado.py:36`. |

   Nos testes, os escritores são os fixtures de empresa/colaborador/célula, `_fechar`, o command real e os retratos artificiais dos negativos; essas gravações pertencem ao código de teste. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:39`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:53`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:66`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:69`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:196`.

   **Desde a rodada 2:** mudaram a prova inferior, o reconhecimento AST do dicionário literal e a explicação documental. A ressalva sobre desligado sem data agora concorda com o caminho do código: ele não ganha fechamento novo pelo universo, mas fechamento antigo continua sendo lido pelo export; C5 segue a lavra. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`, `app/escala/services/cadastro_realidade.py:148`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`.

_2407 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
