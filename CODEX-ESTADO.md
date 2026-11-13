# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 15:46** · publicado em **13/11/2026 15:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 15:46 · sessao 3a3451fa21d5 · - · - · origem=Ronald (tmux)

Consolidado por **estrutura × módulo emissor × módulo consumidor**, incluindo auxiliares que efetivamente leem o mesmo dict e excluindo caches que apenas transportam:

- `celulas_do_periodo`: **15 módulos consumidores identificados**; referências enviadas na mensagem anterior. Escritor `escala/alimentacao.py:73-78`. Ausência de consumidores indiretos adicionais `[nao verificado]`.
- `folgas_do_periodo`: **1 módulo consumidor efetivo**, `escala/models.py:1281`, chave dinâmica `ec.pk`, default `frozenset()`. Escritor `escala/alimentacao.py:97-102`. Selos de cadeia `escala/tests/test_selo_alimentacao_dias.py:59-73,96-108`.
- `escalas_do_periodo_por_colab`: **6 módulos consumidores**: `escala/folgas.py:79`; `escala/services/cadastro_realidade.py:511,528`; `escala/services/propositor.py:442,446`; `ponto/turnos.py:2361,3038,3058`; `ponto/portas/regen_exportada.py:147`; `folha/previa.py:81`. Escritor `escala/alimentacao.py:145-160`. Defaults `[]` ou `()` para chave cid ausente. Selo explícito de cadeia para turnos: `escala/tests/test_o131_um_carregador_de_candidatos.py:192-224`; demais selos específicos `[nao verificado]`.
- `vinculos_do_dia_carregado`: **3 módulos consumidores**, `escala/proposta_pdf.py:36`; `chamados/services/validacao.py:902-903`; `ponto/management/commands/gerar_celulas.py:321`. Escritor `escala/alimentacao.py:329-331`, chave cid, valor EC ou None. Selos específicos `[nao verificado]`.
- `vinculos_dos_dias`: **4 módulos consumidores**, `colaboradores/management/commands/marcar_feriado_do_posto.py:110`; `chamados/services/fila_por_causa.py:176`; `ponto/services/cadastro_futuro.py:40-41`; `ponto/services/vigia_cadastro_futuro.py:56`. Escritor `escala/alimentacao.py:350-358`, chave `(cid,data)`; primeiro consumidor usa `.get` default None, demais `[]`. Selos específicos `[nao verificado]`.
- kwargs de alimentação utils → models: **1 costura**, emissores `escala/utils.py:2215,2218,2265,2269,2291`; chaves opcionais `celulas,folgas`, somente valores não None; consumidores `escala/models.py:1069,1098,1166`, defaults None. Selo da cadeia específica `[nao verificado]`.

São **30 costuras adicionais identificadas** nesse bloco. Nenhum desencontro de chave demonstrado nesse bloco; exaustividade dos aliases indiretos `[nao verificado]`.

Para `grade_da_celula`/`remapear_na_regua`, considerei o mesmo contrato de grade produzido pelo módulo `escala/services/leitor_celula.py`, agrupando as duas funções na costura de espelho. O inventário entregue tem **23 módulos consumidores efetivos**:

| Módulo consumidor | Leitura | Selo da cadeia ou seletor |
|---|---|---|
| `relatorios/furos.py` | `:132-139,194-206` | sim: `escala/tests/test_f1c_contrato.py:61-62` |
| `escala/services/furos_diarios.py` | `:18-22,45-54` | sim: `escala/tests/test_cobparc_rotulo_furos_diarios.py:58-67` |
| `inteligencia/calculadores.py` | `:48-64,147-161,333` | sim: `inteligencia/tests/test_t42_calculadores_celula.py:25-30` |
| `ponto/worklist.py` | `:18-21` | sim: `ponto/tests/test_worklist_ata.py:44-68` |
| `colaboradores/services/calendario.py` | `:353-385` | sim: `colaboradores/tests/test_relogio_riscado_calendario.py:86-106` |
| `ponto/services/bordas_realizado.py` | `:49-51,71-87` | cadeia específica `[nao verificado]`; testes do predicado `ponto/tests/test_bordas_realizado.py:119-152` |
| `chamados/services/materializacao.py` | `:234-243,284-300` | sim: `chamados/tests/test_p71_marco_sem_hora.py:31-46` |
| `chamados/services/declaracao_texto.py` | `:104-133` | sim: `chamados/tests/test_hx_ata_nao_sabe.py:57-99` |
| `chamados/services/disputa_emissao.py` | `:84-85` | `[nao verificado]` |
| `chamados/services/tarefa_do_dia.py` | `:88-89,461-467,499-501` | sim: `chamados/tests/test_e2_tarefa_do_dia.py:603,1380-1391` |
| `ponto/management/commands/propor_reorganizacao.py` | `:66,91-112` | `[nao verificado]` |
| `ponto/management/commands/emitir_furo_retroativo.py` | `:90-102` | sim: `ponto/tests/test_t42_furo_retroativo_celula.py:20-25` |
| `ponto/management/commands/supra_juiz.py` | `:205-224` | sim: `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:164,215-230` |
| `ponto/supra_juiz.py` | `:70-71,222-530` | sim: `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:41-70` |
| `folha/export.py` | `:1095-1106`, wrapper `:338` | sim: `folha/tests/test_borda_celula.py:62-68` |
| `folha/causas.py` | `:35-43,58,84-100` | cadeia específica `[nao verificado]`; teste de contrato sintético `folha/tests/test_causas_sem_previsao.py:24-67` |
| `ponto/services/fechamento.py` | `:1020,1043-1050` | sim: `ponto/tests/test_o48_sem_celula_nao_e_zero.py:96-185` |
| `ponto/services/dia_pago.py` | `:387-400` | cadeia específica `[nao verificado]`; leis do consumidor `ponto/tests/test_retrato_exportada.py:46-65` |
| `ponto/management/commands/mapa_divergencia.py` | `:99-155,174-194` | `[nao verificado]` |
| `ponto/propositor.py` | `:209-213` | `[nao verificado]` |
| `chamados/juizes.py` | `:438` | cadeia específica `[nao verificado]` |
| `escala/utils.py` | `:845-849,957-965,1086-1110` | sim para isenção: `escala/tests/test_leitor_isento.py:64-90`; demais trechos `[nao verificado]` |
| `ponto/services/espelho.py` | `:519-524,677-685,856,892-893` | sim: `escala/tests/test_fronteira_remap.py:64-91` |

Nesse bloco de grade: **23 costuras identificadas; 14 com selo de cadeia/seletor localizado e 9 sem selo específico localizado** — “sem selo” como ausência de localização nesta leitura, não prova de inexistência `[nao verificado]`.

Nenhum desencontro novo foi fechado aqui. As variantes sem célula omitem campos específicos como `celula_id/n_ok/n_casadas`, enquanto os consumidores localizados usam guardas ou `.get`; produtores e variantes estão em `escala/services/leitor_celula.py:390-445`. `futuro/fora_janela` lidos no espelho são acrescentados pelo mutador `escala/utils.py:951-965`, chamado em `ponto/services/espelho.py:521`; não são falta demonstrada no contrato.

**Total dos dois blocos adicionais: 53 costuras identificadas**, além das 13 do levantamento anterior; unicidade com as costuras já enumeradas pelo root exige consolidação `[nao verificado]`.

## 13/11 15:45 · sessao e4cbbc1b1738 · - · - · origem=Ronald (tmux)

Inventário entregue nas quatro mensagens, com emissores, chaves, consumidores, defaults e selos localizados. Correções posteriores prevalecem: `cadastro_realidade.lista` possui também leitores em `vigia_fio`, `fechar_chamados_vinculo_na_lista` e `fechar_avisos_regua_na_lista`; o diagnóstico `n_dias` é consumido em `ponto/services/diagnostico_escala.py:240`.

Limite principal: a busca estática não prova resolução completa de aliases, chamadas dinâmicas nem leitores JavaScript de APIs; completude global `[nao verificado]`. Nenhum arquivo foi alterado e nenhum banco, log ou teste foi executado. [nao verificado]

## 13/11 15:45 · sessao 2c14b553a604 · - · - · origem=Ronald (tmux)

Censo local entregue nas mensagens anteriores: 28 pares nominais de `CelulaDia.ata`, 20 de DNA, 7 de JSON do fechamento e 17 linhas de métricas antes da deduplicação com o censo principal; completude global e total final [não verificado].

Ajustes essenciais para consolidar:

- Incluir `desfazer_julgamento_da_futura` como escritor da variante `ata=None`. `app/ponto/portas/celula.py:1165`.
- Agrupar os leitores de `chamados/juizes.py` numa única costura por estrutura; há leituras em `:450`, `:965` e `:1354`.
- Se repasses indiretos contam, acrescentar ATA/DNA → `ponto/turnos.py`, que carrega células e delega a `periodos_do_dia`. `app/ponto/turnos.py:1582`, `app/ponto/turnos.py:1586`.
- `DiaPago.detalhe`: escritores confirmados em `app/ponto/services/dia_pago.py:346`, `:358`, `:474`, `:512`; leitor nominal confirmado somente no mesmo módulo, em `:897`. Leitor externo da estrutura [não verificado].
- `Advertencia.detalhe_json`: emissor encontrado fora de ponto, em `app/chamados/services/acoes_chamado.py:203`; não entra como estrutura escrita por ponto.
- `ata.precedencia`: escritor em `app/ponto/services/cartorio.py:2000` e `app/ponto/portas/celula.py:1213`; leitor de negócio em outro módulo [não verificado].
- `diff_calculador.py:546` lê `Rubricas.detalhe`, não `DiaPago.detalhe`: tipo e atribuição em `app/ponto/management/commands/diff_calculador.py:427`, `:502`.
- Não tratar chaves diagnósticas como absolutamente órfãs quando existe leitura opaca do blob; hash do fechamento inclui ambos os JSON em `app/ponto/portas/regen_exportada.py:56`, `app/ponto/management/commands/lavrar_dias_pagos.py:70`.

Selos conferidos incluem transporte de isenção, cobertura parcial e métricas C8/C12; vários outros testes só cobrem fixtures ou leitores isolados, portanto não demonstram a costura completa. `app/ponto/tests/test_carimbo_isencao_lampada.py:144`; `app/escala/tests/test_cobparc7_leitor_transporta_a_lampada.py:29`; `app/ponto/tests/test_c2_vinculo_vira_lista.py:73`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:84`; `app/folha/tests/test_retrato_folha_quadros.py:1464`.

## 13/11 15:45 · sessao 73e2ed51da70 · - · - · origem=Ronald (tmux)

Complemento dos retornos, com correções para a consolidação:

| Estrutura | Emissor/chaves | Consumidor/chaves | Desencontro | Selo |
|---|---|---|---|---|
| `com_nomes` | `ponto/services/abriu_nao_bateu.py:117`: inicia `mudos,recusados`; `:147-149` acrescenta `total,abriram,janela_horas`; retorna em `:150` | `ponto/management/commands/lavrar_abriu_nao_bateu.py:35,36,40,49,51`: `total,mudos,recusados` | **Não** quanto a `total`: o coletor de dict literal perderia a mutação incremental. | Selo desta costura **[não verificado]**. |
| diagnóstico | `ponto/services/diagnostico_escala.py:221-247`: `colaborador,periodo,dias,so_os_dias_que_discordam,paridade_real,horario_mediano,dias_fora_do_horario,troca_de_ritmo,termometro,destino_sugerido,template,pausa_nao_cadastrada,acoes,fonte` | `api/views_mensageria.py:2008-2010`: recebe inteiro, acrescenta `ano_assumido`, serializa | Não há leitura nominal incompatível nesse consumidor. | **Sim**, `api/tests/test_diagnostico_escala.py:122-128` verifica endpoint e campos do diagnóstico. |
| turno órfão | `ponto/utils_turno_orfao.py:37-42`: itens `pk,data,hora,horas`; envelope `tem_bloqueio,tem_regularizacao_pendente,entradas_pendentes,qtd_pendentes` em `:60-65` | `chamados/detector_anomalias.py:33-43`: flags default `None`; `entradas_pendentes=[]`; itens `horas=0,hora=None,pk=None` | Não quanto aos nomes lidos. `data` do item é civil (`utils_turno_orfao.py:39`); o consumidor deriva o dia do turno pelo `pk`, em `detector_anomalias.py:43-48`. | Selo entre estes dois módulos **[não verificado]**. Testes do produtor: `ponto/tests/test_bloqueio_orfao_nucleo.py:33-36`, `test_o247_retratada_fora_do_pareamento.py:86-114`. |
| turno órfão | mesmo produtor | `api/views.py:327,353` | `_bloq['entradas_pendentes']` aparece sintaticamente, mas está no ramo falso do ternário: `regularizacao_obrigatoria=False` em `:330`. Leitura efetiva dessa chave nesse caminho **não acontece pelo corpo mostrado**. | `api/tests/test_api_me_nao_bloqueia.py:55-66` verifica órfão existente e resposta `entradas_pendentes=[]`. |
| turno órfão | mesmo produtor | `api/views_core.py:315,341` | Mesma leitura nominal inativa: flag fixada em `False` em `:318`. | Selo específico deste kernel **[não verificado]**. |
| resumo do espelho | `ponto/services/espelho.py:568`: `grade_pelo_builder=True` somente no fallback | `folha/porta_export.py:514`: `r.get('grade_pelo_builder')`, default `None` | Presença condicional; nenhum nome divergente. Se a régua contar ausência opcional como desencontro, registrar como **opcionalidade**, não como incidente. | **Sim**, `ponto/tests/test_o81_celula_da_competencia.py:59-72` prende os nomes dos dois lados. |
| resumo do espelho | `ponto/services/espelho.py:910-912`: acumula `total_relogio` somente havendo dia dentro da janela; não inicializado em `:1230-1232` | `folha/porta_export.py:511`: `r.get('total_relogio') or 0` | Presença condicional, default explícito no leitor. Efeito problemático **[não verificado]**. | Selo exato produtor–consumidor **[não verificado]**; teste do campo no produtor em `ponto/tests/test_e5_topo_soma_das_linhas.py:30`. |
| mesmo resumo | mesmo escritor `:910-912` | `templates/ponto/espelho.html:47-49`: testa e imprime `resumo_mes.total_relogio` | Presença condicional, condição explícita de exibição. | Selo exato desta costura **[não verificado]**. |
| resumo do espelho | `relatorios/cartao_pela_celula.py:321-345`: acrescenta `fonte_dos_totais` só quando `_comp` existe; recebe o dict de ponto em `ponto/services/espelho.py:1283-1284` | `api/views.py:1660`: `.get('fonte_dos_totais')`, default `None` | Presença condicional; nenhuma incompatibilidade de nome. | **Sim**, `api/tests/test_bug139_espelho_app_mesma_fonte.py:109-118` sela chaves do payload; `api/tests/test_espelho_app_fonte_e_geometria.py:85-105` sela valores alternativos. |
| mesmo resumo | mesmos escritores | `colaboradores/services/ficha.py:200-209`: `total_trabalhadas,total_extras,turnos,turnos_abertos`, todos com `or 0`; `fonte_dos_totais` default `None` | Presença condicional da fonte; nenhuma chave incompatível encontrada nesse corpo. | Selo específico da ficha **[não verificado]**. |

Para o dicionário do espelho não limitar o inventário ao literal do `return`: o envelope está em `ponto/services/espelho.py:1294-1296`; `resumo` recebe mutações em `:1249-1270`, por `montar_dias` em `:1262-1266`, e por `folha_manda` em `:1283-1284`. `montar_dias` ainda grava `datas_turno_aberto` (`:471`), `datas_saida_orfa` (`:617`), somas pelas chaves de `somar_periodos` (`:441`, esquema `:77-79`) e os campos condicionais acima.

Nenhum desencontro novo inequívoco confirmado nesse complemento; preservo D1–D7 do relatório anterior. Ausência global de outros desencontros **[não verificado]**.

## 13/11 15:44 · sessao 3432109c0e42 · - · - · origem=Ronald (tmux)

Censo do recorte enviado em mensagens com todos os sítios e chaves. Nenhum arquivo criado, editado ou apagado; nenhum teste executado. [nao verificado]

Desencontros confirmados:

| Estrutura | Emissor | Consumidor | Desencontro | Selo | Dono / classe |
|---|---|---|---|---|---|
| WebPush | `app/chamados/push_utils.py:28` | `app/static/sw.js:89` | Emite `requireInteraction`; lê `urgente \|\| false`. Emite `url`; clique abre `/` sem lê-la (`app/static/sw.js:102`). | Não localizado [nao verificado]; `app/chamados/tests/test_push_fcm.py:106` verifica somente envio chamado. | ESTRUTURA provável; sim. |
| Ticket GLPI | `app/chamados/glpi_client.py:138` | `app/templates/chamados/partials/tabela_chamados.html:19` | Template lê `urgencia`, ausente no formatador; termina em “Normal”. `assunto` e `colaborador_nome` também ausentes, com alternativas explícitas (`:21`, `:24`, `:30`). | Não localizado [nao verificado]. | ESTRUTURA provável; sim. |
| Followup GLPI | `app/chamados/glpi_client.py:209` | `app/templates/chamados/detalhe.html:80` | Grava `id,data,conteudo`; template lê `autor_tipo,autor_user,criado_em,conteudo`. Transporte confirmado em `app/chamados/views.py:246` e `:262`. | Não localizado [nao verificado]. | ESTRUTURA provável; sim. |

Portas auditadas sem incompatibilidade demonstrada: entradas `defaults/**lookup/**campos` dos chamados (`app/chamados/models.py:834`, `:1055`); entradas das perguntas (`:2777`, `:2795`); decisões (`:3944`); descarimbo em lote (`:3253`); texto (`app/chamados/services/texto.py:21`); leitura de notas (`app/chamados/services/nota.py:126`); anotação de batidas (`app/ponto/registro_batida.py:235`).

Carimbo do fio possui dois módulos leitores comprovados: `bin/fio_estado.py:121` e `app/core/management/commands/pipeline_placar.py:92`, alimentados por `app/chamados/management/commands/vigiar_fio_morto.py:228`. Selos encontrados: `app/chamados/tests/test_o261_vigia_fio.py:787`, `app/core/tests/test_fio_mudo_placar.py:147`, `:197` e `:227`.

Logs: inventário enviado de 47 sítios iniciais mais seis sítios descobertos por resolução de aliases, com consumidores integrais `core.observ.evento` (`app/core/observ.py:94`), `evento_kw` (`:142`) e `registrar_log` (`app/core/auditoria.py:8`). Normalizar pela unidade final solicitada pelo coordenador: estrutura × módulo emissor × módulo consumidor; não somar sítios como costuras.

A persistência do outbox fica fora de `chamados`: `app/ponto/portas/celula.py:954`. As origens em chamados e o payload intermediário foram enumerados; o primeiro grava `{celula_id,colaborador_id,data}`, enquanto `app/chamados/models.py:3229` produz as tuplas intermediárias. Considerar essa fronteira explicitamente na consolidação.

Completude global, execução dos selos e efeitos em produção: [nao verificado].

## 13/11 15:44 · sessao 219ef396477d · - · - · origem=Ronald (tmux)

Recorte concluído por busca estática de chamadas, retornos dict, `render`, chaves `.get`/subscritas e testes. Nenhum arquivo alterado. Testes não executados. A busca não demonstra exaustividade de chamadas dinâmicas, consumidores externos ao repositório ou dados de produção `[nao verificado]`. [nao verificado]

Nos quadros abaixo, **“sem selo” significa teste produtor+consumidor não localizado `[nao verificado]`**, e não prova de inexistência. **Classe** está indicada como “sim” apenas para desencontro de chave demonstrado; diferenças opcionais aparecem nomeadas sem afirmar defeito operacional. [nao verificado]

### Schemas compartilhados

**W — payload wizard.** Emissor `app/escala/views_wizard.py:20-59`: `template_nome,apelido,hora_inicio,hora_fim,hora_inicio_intervalo,hora_fim_intervalo,hora_inicio_intervalo2,hora_fim_intervalo2,intervalo_duracao_min,noturno,permite_hora_extra,he_extra_antes_min,he_extra_depois_min,folga_calendario_padrao_id,folgas,dias_trabalho_semana`; apenas criação acrescenta `tipo_ciclo,tipo_base,tipo_base_ref`; apenas POST com `horarios_por_dia` preenchido acrescenta essa chave (`:49-59`). Outro emissor da mesma porta, fora do recorte, grava apenas `template_nome,folgas`: `app/chamados/services/limbo_folgas.py:92`.

**P — envelope e proposta viva.** Emissor `app/escala/services/propositor.py:465-467,585-589`: envelope `fonte,n,propostas,universo,janela_dias,ritmo_discordante,ritmo_discordante_vinculos,pausa_nao_cadastrada,pausa_nao_cadastrada_vinculos`. Escritores internos das propostas: `:184-195,299-305,319-323,339-342,363-375`, chaves comuns `tipo,template_id,template,ancora,casa_hoje,casaria,previsto_novo`; ramos aplicáveis acrescentam `sobra`; alguns ramos acrescentam `motivo`. Enriquecimento `:539-561`: `colaborador_id,colaborador,posto,vinculo_id,template_atual,ancora_atual,razao_fora_dentro,dias_discordantes,entrou_por,dias_batidos,dias_previstos,aplicar_desde`; `motivo` pode ser reescrito em `:568-570`; `chamados_que_morreriam` nasce em `:572,575-576`, contendo `{chamado_id,dia}` escrito em `:634-635`. `previsto_novo` é removido em `:577`, salvo ramo `so_existencia=True`, que retorna antes da remoção (`:571-574`).

**D — detector v0.** Emissor `app/escala/detector_proposta.py:123-132,153-156`: `padrao,proposta,confianca,porque`; proposta preenchida contém `tipo_escala_id,codigo,descricao,ciclo,horario`. `padrao` vem de `:63-65,82-92`: `dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,dows,padrao_dias`; ramo com dados também grava `marcos_freq`.

### Portas e propostas: 17 pares identificados

| ID / estrutura | Emissor / gravadas | Consumidor / lidas | DESENCONTRO | Selo que prende | Dono provável / classe |
|---|---|---|---|---|---|
| P01 W | `views_wizard.py:20-59`, W | `services/cadastro_tipo.py:148-267`: W por `.get`; `template_nome→''`, ciclo/base→`12x36`, campos texto→`''`, bool via `bool`; duração só altera se dígitos; HPD inválido→`{}`; lê presença de `descricao` em `:185` | `descricao` nunca emitida pelo wizard; HPD pode faltar e é lido por `.get` em `:217`; demais omissões condicionais têm guardas. Trata-se de protocolo opcional, sem dano demonstrado | **sim**, `tests/test_wizard_views.py:28-49,51-67` percorre POST→payload→service→modelo; ponte duração em `tests/test_wizard_intervalo.py:76-89` | ESTRUTURA provável; classe: sim, ausência opcional pelo critério literal |
| P02 ajustes `**aj` | `services/escala_auto_executor.py:65-84`: `_delta_blocos`, seis horários no A7; pausa/duração no A4; `folga_dia_semana` no FOLGA | `services/cadastro_tipo.py:443-445`: `pop('_delta_blocos',0)` e todos demais itens via `setattr` | A4/FOLGA não emitem `_delta_blocos`; default explícito 0. Sem chave de ajuste descartada | **sim**, `tests/test_escala_auto_executor.py:66-84` percorre executor→derivação e confirma pausa do novo template | ESTRUTURA provável; classe: sim, opcional |
| P03 P | `services/propositor.py:465-589`, P | `management/commands/propostas_de_escala.py:39-98`: `propostas,colaborador_id,vinculo_id→None,universo,n,colaborador,tipo,template_atual,ancora_atual,casa_hoje,dias_batidos,motivo` no ramo grade; `template,ancora,casaria,sobra` no outro; `aplicar_desde,chamados_que_morreriam[].chamado_id` | Guardas dos ramos respeitam a ausência de `sobra` na grade. Chaves órfãs globais abaixo | Sem selo real localizado; `tests/test_o261_v_fase_na_lista.py:527,539` usa mocks do produtor | ESTRUTURA provável; classe não demonstrada |
| P04 P | mesmos escritores P | `management/commands/ritmo_discordante.py:20-22`: `ritmo_discordante_vinculos→[]`, itens `colaborador_id,ciclo,ritmo,proposta`; `ritmo_discordante→0` | Chaves emitidas nos dois retornos do envelope | Sem selo localizado | ESTRUTURA provável; classe não demonstrada |
| P05 P | mesmos escritores P | `app/chamados/services/painel.py:522-524`: `propostas[].colaborador_id`; produtor chamado com `so_existencia=True` | Esse ramo preserva `previsto_novo` no dict (`propositor.py:571-577`), consumidor não lê; nenhum dano demonstrado | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada |
| P06 P | mesmos escritores P | `app/colaboradores/services/ficha.py:176-184`: `propostas,tipo`; `.get(template_atual,template,ancora,casa_hoje,casaria,dias_batidos,aplicar_desde)→None`; `chamados_que_morreriam→[]` | Chaves lidas são emitidas; consumidor substitui janela por literal `28` (`:181`) embora envelope tenha `janela_dias` | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada: nome/significado de janela não provado divergente |
| P07 P | mesmos escritores P | `app/chamados/services/proposta_no_fio.py:41-42,68-92,107-114,141-146`: `n→None,propostas`; `dias_batidos,casa_hoje,casaria,aplicar_desde`; `chamados_que_morreriam→[]`, `chamado_id`; `.get(vinculo_id,tipo,ancora,ancora_atual,template,template_atual,motivo,entrou_por)→None`; assinatura lê `tipo,template_id,ancora,aplicar_desde` em `propositor.py:663-665` | `motivo` falta em ramos `:339-342,363-367`; consumidor aceita None | Sem selo produtor real localizado; testes fio substituem propostas, `app/chamados/tests/test_proposta_no_fio.py:131-146` | ESTRUTURA provável; classe sim, opcional |
| P08 P | mesmos escritores P | `app/core/views_ia.py:575-582,598-636,664`: `propostas→[]`, `colaborador_id→None`; `.get(tipo,entrou_por,dias_discordantes,vinculo_id,ancora,ancora_atual,template_atual,template,aplicar_desde,casa_hoje,casaria)`, `dias_discordantes→0`, `chamados_que_morreriam→[]`; assinatura por autoridade `:578` | Chaves lidas emitidas; ramos respeitados | Sem selo produtor real localizado; `app/core/tests/test_ia_escala_com_sim.py:59,104-145` fabrica envelope | ESTRUTURA provável; classe não demonstrada |
| P09 P | mesmos escritores P | `app/api/views_mensageria.py:2034-2049`: `propostas→[]`, `.get(colaborador,posto,ancora,template_atual,template,casa_hoje,casaria,aplicar_desde,tipo)`, `chamados_que_morreriam→[]`, `n→0` | Chaves lidas emitidas | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada |
| P10 linhas da pauta — transporte | `services/propositor.py:644-650`: `chave,n,frase,url` | `services/fase_12x36.py:267-280`: concatena lista e passa `linhas` a `escrever_semanal`; não lê chaves internas | Nenhum desencontro demonstrado; é transporte, excluir da contagem se “leitor” exigir acesso a chave | Sem selo específico localizado | ESTRUTURA provável; classe não |
| P11 linhas da pauta | mesmo emissor `propositor.py:644-650` | `app/pautas/semanal.py:146,163-168`: `alavanca→n→0`, `frase→''`, `chave,n` no fallback, `url` obrigatório | Produtor não grava `alavanca`; usa-se `n`, apesar de frase mencionar chamadas que morreriam (`propositor.py:643-648`) | Sem selo do par localizado | ESTRUTURA provável; classe sim, opcional |
| P12 opções da porta retroativa | `services/propositor.py:765-771`: `colab,vinculo,template,desde,ate,ancora,novo_trecho,regenerar_desde,apesar_da_lavra,apply,schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:63-66,81-86,109-110,158,172,192-194,225,233`: todos salvo `schema`; opcionais `.get` sem default ou `apesar_da_lavra→''` | `schema` gravado mas `_rodar` não lê; só `handle` lê em `:48-51`, deliberadamente evitado pelo emissor (`propositor.py:760-764`) | **sim parcial**, `tests/test_porta_da_proposta.py:148-166` chama aplicar real e confirma trilha/decisão; não prende especificamente `schema` | ESTRUTURA provável; classe sim, chave transportada sem leitura nessa entrada |
| P13 resultado aplicar | `services/propositor.py:788`: `decisao_id,saida` | `management/commands/propostas_de_escala.py:68-72`: `saida,decisao_id` | Sem desencontro de chave | Sem selo real CLI→produtor localizado | ESTRUTURA provável; classe não |
| **P14 resultado aplicar** | **`services/propositor.py:788`: apenas `decisao_id,saida`** | **`app/chamados/services/proposta_no_fio.py:151-153`: `r.get('ok',True)` e `(r or {}).get('msg')`** | **`ok` e `msg` nunca emitidos por aplicar. `ok` ausente vira sucesso True.** Mapeamento não adapta resultado: `app/core/intake.py:49-52,481-487` resolve diretamente `escala.services.propositor.aplicar` | **Sem selo dessas chaves localizado**; mocks de fio em `app/chamados/tests/test_proposta_no_fio.py:131-146` não atravessam produtor real | **ESTRUTURA provável; classe sim** |
| P15 D | `detector_proposta.py:63-92,123-156`, D | `proposta_pdf.py:75-80,121-138`: `proposta,confianca,padrao.dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,padrao_dias,dows`; `proposta.codigo,horario,ciclo`; `porque` | Todas chaves lidas são emitidas em seus ramos; `marcos_freq` não é lido pelo PDF, mas é lido pelo próprio detector `:133-149` | **sim**, `tests/test_detector_proposta.py:141-165,168-181` monta resultados pelo produtor real e gera PDF | ESTRUTURA provável; classe não |
| P16 D | mesmos escritores D | `management/commands/propor_escala_limbo.py:37-57`: `confianca,proposta,padrao` com as mesmas oito chaves de padrão do PDF; proposta `codigo,horario,ciclo`; `porque` | Todas chaves lidas emitidas nos ramos respectivos | Sem selo real CLI localizado | ESTRUTURA provável; classe não |
| P17 D | mesmos escritores D | `services/escala_auto.py:120-125`: `proposta→None,confianca→None,padrao.dias,proposta.tipo_escala_id,codigo,descricao,horario,porque` | Chaves emitidas; ramo `proposta` guarda acesso à proposta e ao mínimo de dados | Sem selo do par real localizado | ESTRUTURA provável; classe não |

### Render e resposta JSON: 13 pares identificados

| ID / estrutura | Emissor / gravadas | Consumidor / lidas | DESENCONTRO | Selo que prende | Dono provável / classe |
|---|---|---|---|---|---|
| P18 contexto minha escala | `views.py:15,23` grava só `sem_vinculo`; ramo normal `:55-70` grava `escala,dias,hoje,ancora_ausente,titulo_pagina`; dias `{data,trabalho,hora_inicio,hora_fim,status_rotulo}` | `app/templates/escala/minha_escala.html:10,24-49,62-84`: `sem_vinculo,escala,ancora_ausente,dias,hoje` e todas chaves dos dias. `base_tpl|default:"base.html"` em `:1` pode vir de contexto externo | Ramo normal não grava `sem_vinculo`, consumido como condição falsa. `titulo_pagina` sem leitura direta localizada. Bases/includes globais não fechados `[nao verificado]` | **sim**, `tests/test_e4_6_minha_escala_e_previsao.py:50-73` atravessa view/template e confirma rótulo HTML | ESTRUTURA provável; classe sim, ausência opcional |
| P19 contexto tipos | `views.py:114-115`: `tipos,uso,quadros_barra`; uso mapa `{tipo_id:n}` em `:85-90` | `app/templates/escala/tipos_lista.html:21,42,89-91`: as três chaves; `uso|get_item` por pk | Sem desencontro nominal demonstrado | **sim parcial**, `tests/test_o122_etapa1_barra_em_tipos.py:56-82` atravessa rota/render e confirma quadro | ESTRUTURA provável; classe não |
| P20 quadros barra | `views.py:105-112`: quadros `{titulo,itens}`; item `{atalho,rotulo,icone,href,onclick,attrs,title}` | `app/templates/core/_barra_gestao.html:65,67` via `tipos_lista.html:21`: lê `titulo,title,itens`; item `atalho,botao,attrs,onclick,ativo,title,icone,rotulo,href|default:'javascript:void(0)'`; ramo não atalho também `classe,cor,negrito_escuro,numero` | `q.title,it.botao,it.ativo` faltam neste emissor; opcionais. Ramo não atalho é inalcançável para o item emitido | **sim**, `tests/test_o122_etapa1_barra_em_tipos.py:56-82` prende título, ícone, href e attrs htmx | ESTRUTURA provável; classe sim, opcionais |
| P21 contexto opções busca | `views.py:229`: `escalas` | `app/templates/colaboradores/partials/_opcoes_escala.html:1-7`: `escalas` | Sem desencontro de chave | Teste específico real busca→partial não lido; `[nao verificado]` | ESTRUTURA provável; classe não |
| P22 contexto lista calendários | `views.py:294-310`: `itens[].{cal,tamanhos,semana_dia_rotulo,colabs,n_colabs}`, `tamanhos[].{tamanho,dias,canonico}` | `app/templates/escala/folga_calendarios_lista.html:12-38`: todos campos item; tamanhos lê só `canonico` (`:21-22`) | `tamanhos.tamanho,dias` gravados sem leitura neste consumidor; sem outro leitor desse dict localizado `[nao verificado]` | `tests/test_editor_folga_modo.py:66-71` verifica contexto, não HTML; **sem selo das chaves órfãs** | ESTRUTURA provável; classe sim, órfãs no par |
| P23 contexto editor calendário | `views.py:363-371`: `cal,tamanhos,tipos,modo,celulas`; tamanho `{tamanho,rotulo,dias_range,marcados}` | `app/templates/escala/folga_calendario_editar.html:6-21,67-68`: `cal,modo,celulas,tamanhos.{tamanho,rotulo,marcados}` | `tipos` e `tamanhos.dias_range` sem leitura no template; sem outro leitor desse dict localizado `[nao verificado]` | `tests/test_editor_folga_modo.py:58-64` verifica contexto, não uso HTML; **sem selo das órfãs** | ESTRUTURA provável; classe sim, órfãs |
| P24 wrapper cadastro realidade | `views.py:492`: `{dados}`; conteúdo vem de `services/cadastro_realidade.lista`, coberto pelo agente contextos | `app/templates/escala/cadastro_x_realidade.html:8-29`: `dados` e chaves internas conforme serviço | Wrapper em si confere; não duplicar contrato interno de `lista` | Selo não conferido aqui `[nao verificado]` | ESTRUTURA provável; classe não demonstrada |
| P25 contexto plano | `views_plano_folgas.py:38-47`: `empresas,mes,ano,meses,anos,empresa_id,posto_id`, condicional `postos,linhas,cab`; `:72-93` linhas `{ec,cels,sem_previsao,escala_nome}`, cels `{dia,tipo,proj}`, cab `{dia,dow,domingo}` | `app/templates/escala/plano_folgas.html:14-90,173,192-193`: todas chaves descritas; `cab|length|default:0` em `:193` | `postos,linhas,cab` omitidos sem empresa; consumidor consulta dentro de guardas/iterações vazias/default | Testes locais de `_grid_ctx` em `tests/test_plano_folgas_filtro.py:53-64`; **selo HTML dessas chaves não localizado** | ESTRUTURA provável; classe sim, omissões opcionais |
| P26 contexto wizard | `views_wizard.py:75-91`: `tipo,criar,ciclos,bases,hpd_json,folgas_atuais,pai,folga_calendarios,perso_valor,drawer,base_tpl` | `app/templates/escala/wizard_tipo_escala.html:1-37,146,225-234`: todos salvo `folga_calendarios`, lido em include P27 | Sem desencontro no conjunto template+include | **sim parcial**, `tests/test_wizard_drawer.py:25-34` atravessa `drawer/base_tpl` e verifica HTML; calendários em `tests/test_wizard_folga_padrao.py:83-89` verifica contexto | ESTRUTURA provável; classe não |
| P27 contexto wizard herdado pelo select | mesmo emissor P26; `selecionado=tipo.folga_calendario_padrao_id` acrescentado pelo include em `wizard_tipo_escala.html:146` | `app/templates/escala/_wizard_cal_select.html:4-5`: `folga_calendarios,selecionado` | `selecionado` nasce no template intermediário, não na view; cadeia completa confere | Sem selo que prenda seleção real localizado | ESTRUTURA provável; classe não |
| P28 contexto select calendário criado | `views_wizard.py:175-178,196-199`: `folga_calendarios,selecionado` | `app/templates/escala/_wizard_cal_select.html:4-5`: ambas | Confere | **sim parcial**, `tests/test_wizard_folga_padrao.py:41-54` cria pela rota e confirma nome do calendário no fragmento; seleção por pk não assertada | ESTRUTURA provável; classe não |
| P29 contexto preview | `views_wizard.py:263-265`: `tipo,folgas_rotulos,eh_perso,perso_dias` | `app/templates/escala/_wizard_preview.html:4-12`: as quatro | Confere | **sim**, `tests/test_wizard_views.py:140-152` atravessa POST→preview→HTML e confirma apelido/HE | ESTRUTURA provável; classe não |
| P30 JSON aplicação lote plano | `views_plano_folgas.py:158,171-190`: erro inicial `{erro}` com 400; sucesso `{ok,gravadas,erros,celulas_relavradas,barrados}`; erros itens `{ec,dia,erro}` | JS `app/templates/escala/plano_folgas.html:174-185`: lê `ok`, `erro||'erro'` se falsy; `erros||[]`, erro item `ec,dia,erro` | Resposta de erro inicial não tem `ok`, JS usa ausência como false. `gravadas,celulas_relavradas,barrados` não são lidas por esse JS; leitura por outros consumidores não localizada `[nao verificado]` | Teste `tests/test_bug136_plano_em_lote_relavra.py:75-81` verifica JSON de erro na API, sem executar o JS; **selo da costura com JS não localizado** | ESTRUTURA provável; classe sim, omissão opcional e campos sem leitor no par |

### Pontos a incorporar na consolidação

- **30 linhas identificadas**, das quais **P10 é mero transporte de lista**; se o critério exigir acesso a chave interna, são **29 costuras** deste recorte. P24 é wrapper e deve ser distinguido da costura de conteúdo `cadastro_realidade.lista→template` do outro agente.
- **Desencontro mais forte: P14**, comprovado com produtor, consumidor e roteamento dinâmico, sem hipótese sobre banco.
- Proposta tem possíveis chaves órfãs `razao_fora_dentro` (`propositor.py:547`), `dias_previstos` (`:559`) e `chamados_que_morreriam[].dia` (`:634-635`). Nenhum leitor nominal do objeto proposta foi localizado; orfandade global permanece `[nao verificado]`.
- **Assimetria de schema em P:** `previsto_novo` é retirado no retorno normal (`propositor.py:577`), mas fica no retorno de existência (`:571-574`). O consumidor atual do ramo de existência lê apenas `colaborador_id` (`app/chamados/services/painel.py:522-524`); dano não demonstrado.
- Há respostas JSON de `views.py:127-133,245-250,266-279,422` e `views_plano_folgas.py:138-139` cujo consumidor não foi localizado neste recorte `[nao verificado]`. Não inventei costura sem leitor.
- `services/agenda.py:11-45` recebe parâmetros escalares/modelos/lista de IDs e grava campos ORM; não identifiquei dict customizado emitido por ela e lido por outro módulo. Não contei kwargs internos de `objects.create/update` como contrato customizado.
- Todos os donos acima são **prováveis pelo protocolo de código**, não aplicação do juiz L-099 por dia de produção `[nao verificado]`.

## 13/11 15:43 · sessao f90fd80293fd · - · - · origem=Ronald (tmux)

Inventário estático de `ChamadoColaborador.contexto_json`, por leitura de referências nominais, AST em memória e seguimento dos aliases e portas. Campo persistido: `app/chamados/models.py:47`. As portas aceitam chaves arbitrárias; portanto reflexão, chamadas dinâmicas, dados históricos e execução real permanecem [nao verificado] (`app/chamados/services/contexto.py:62`, `:82`, `:131`). Não executei testes nem escrevi arquivos.

**Escritores internos encontrados, com shapes**

`app/chamados/services/contexto.py:76`, `:102`, `:125`, `:161` são os saves efetivos de anotar/renovar/retirar/fundir; os emissores semânticos encontrados são:

| Emissor | Chaves gravadas |
|---|---|
| `services/abertura.py:58` | `sistema,nome,empresa,posto,pendente_glpi=True` |
| `services/abertura.py:116` | `pendente_glpi=False` |
| `services/abertura.py:183` | `sistema,nome,empresa,posto,aberto_por_user_id,aberto_por,pendente_glpi=False`, mais chave dinâmica de data em `:191`; resolve por `catalogo/modulos.py:315` |
| `services/adesao.py:79` | `adesao=True,data_inicio,periodo=[ini,fim]` |
| `services/adesao.py:113` | `agrupado_em` |
| `reconciliador.py:691`, `:708`, `:709`, persistido em `:921` | `data_turno,origem='reconciliador_c2'`, condicionalmente `marcos_faltantes,_marco_por='lampada_da_celula'`; marcos têm `{hora,tipo,motivo:'lampada_apagada'}` por `app/ponto/services/lastro.py:134` |
| `services/disputa_emissao.py:580`, reuso em `:586` | `contexto_nascimento` arbitrário; wrapper `:630` passa `{origem}`, wrapper `:666` repassa `contexto or {}` |
| `services/disputa_emissao.py:1116`, reuso `:1122` | `chamados_orfaos_ids` |
| `services/disputa_emissao.py:1351` | `origem='ancora_limbo',chamado_limbo_id` |
| `services/disputa_emissao.py:1357` | `chamados_limbo_ids`, em reuso |
| `services/limbo_folgas.py:68` | `sugestao={classe,dias,ndias,semanas}` |
| `services/limbo_folgas.py:276` | `agrupado_em,defeito_cadastro='folga_nao_definida'`; guarda só por `agrupado_em` |
| `services/declaracao_texto.py:208` | `escape_colab,escape_colab_em`; guarda só por `escape_colab` |
| `management/commands/nomear_marco_por_lampada.py:109` | `marcos_faltantes,_marco_por='reconstruido_pela_lampada'` |
| `management/commands/carimbar_por_batida_coincidente.py:98` | `batida_id,data_turno,_dia_por` |
| `management/commands/smoke_chamados.py:97`, via porta em `:102` | `{motivo:{data}}` quando pergunta do catálogo pede data; caso contrário `{}`; também nasce no contexto do chamado pela porta |

A ponte adicional de fusão está em `services/nota.py:110` → `:123` → `services/contexto.py:131`: `contexto_novo` é arbitrário, listas unem, escalares existentes ficam (`services/contexto.py:146`).

**Leitores nominais e shapes/defaults**

Caminhos abaixo têm prefixo `app/chamados/`, salvo os explicitamente iniciados por `app/`.

| Leitor | Chaves lidas |
|---|---|
| `catalogo/modulos.py:225` | `ctx.get(m['chave_data'])`, default `None`; chave vem de `MOTIVOS`, `:105-198` |
| `juizes.py:114`, `:118`, `:129-130` | chave catalogada; fallback `data_turno=None`; `batida_id=None`, seguido de acesso direto |
| `juizes.py:159-162` | `ultima_batida=None → {}`, `ultima_batida.timestamp=None`; fallback `entrada_timestamp=None` |
| `juizes.py:187`, `:197`, `:226` | lote: `data_turno=None`, `batida_id=None`; depois contexto fraco anterior; **não consulta chave catalogada** |
| `juizes.py:549-551`, `:576`, `:579` | `marcos_faltantes=None → []`; por marco `motivo=None,hora=None,tipo=None`; motivo nomeado impede fallback por tipo |
| `juizes.py:1723`, `:1747` | `agrupado_em=None` |
| `juizes.py:2163` | `entrada_timestamp=None` |
| `juizes.py:2331` | `ausencia_id=None` |
| `models.py:816` | `agrupado_em=None` |
| `models.py:924` | `defaults.contexto_json=None → {}`; `marcos_faltantes=None` |
| `models.py:931` | ORM `contexto_json__has_key='agrupado_em'` |
| `models.py:955-958` | `defaults.contexto_json`; chaves dinâmicas extraídas de `contexto_json__*` |
| `models.py:1068`, `:1235-1236` | contexto inteiro para `_celula_de`; `_modulo_origem=None → ''`; delega dia ao juiz |
| `models.py:1435` | `entrada_timestamp=None` |
| `reconciliador.py:146` | `entrada_timestamp=None` |
| `reconciliador.py:198` | `par=None`, espera lista/tupla de horários |
| `reconciliador.py:243` | `chamados_orfaos_ids=None`; ausência retorna `None`, não `False` |
| `reconciliador.py:836`, `:1589` | filtro ORM `data_turno` |
| `data_evento.py:27-39` | ordem `data_turno=None,data_batida=None,entrada_timestamp=None,pendentes=None,horario_previsto=None`; datas ISO ou texto BR para pendentes |
| `services/disputa_emissao.py:57-59` | todas as chaves por `.items()` e `setdefault`; usa `entrada_sem_saida=None → {}` |
| `services/disputa_emissao.py:68-73` | `entrada_sem_saida.entrada_id=None`, `.entrada_aberta_em=None`, `ultima_batida=None → {}`, `.id=None` |
| `services/disputa_emissao.py:145` | `entrada_timestamp=None` |
| `services/disputa_emissao.py:733`, `:739` | delega marcos a `_marcos_cobrados`; data catalogada |
| `services/disputa_emissao.py:930` | data catalogada |
| `services/disputa_emissao.py:1074`, `:1232` | delega contexto a `data_evento_do_contexto` |
| `services/disputa_emissao.py:1143` | `marcos_faltantes=None → []`; subchaves dos marcos em `:1146-1151` |
| `services/disputa_emissao.py:1170` | data catalogada |
| `services/acoes_chamado.py:30-32` | `tipo_batida='E',hora_tentativa='',motivo_anuencia=''` |
| `services/acoes_chamado.py:57`, `:61` | `foto_b64=None,foto_nome='selfie.jpg'` |
| `services/acoes_chamado.py:96`, `:403` | `agenda_dia_ids=[]` |
| `services/acoes_chamado.py:144-146` | presença de `snapshot_ecs`; `.get(None) → []` |
| `services/acoes_chamado.py:185`, `:191`, `:195-210` | `distancia_metros,alerta_geofence_id,tipo_batida,posto_nome,raio_metros,posto_lat,posto_lon,batida_lat,batida_lon`, todos `.get` sem default; texto usa `or` para vazios |
| `services/acoes_chamado.py:247` | `batida_id=None,ocorrencias=None → []`; em ocorrência `batida_id=None` |
| `services/acoes_chamado.py:267`, `:275-276` | `batida_id=None`, `distancia_metros=None → '?'`, `posto_nome=None → '-'`, `raio_metros=None → '-'` |
| `services/acoes_chamado.py:371` | `hora_tentativa=None → None` |
| `services/limbo_folgas.py:64`, `:84`, `:101` | `tipo_escala_id=None`, também filtro ORM |
| `services/limbo_folgas.py:82-85` | `sugestao=None → {}`, `.dias=None → []`, `.classe=None` |
| `services/limbo_folgas.py:193` | filtro ORM `agrupado_em` |
| `services/limbo_folgas.py:253` | `agrupado_em=None`, guarda |
| `services/adesao.py:111` | `agrupado_em=None`, guarda |
| `services/declaracao_texto.py:205` | `escape_colab=None`, guarda |
| `services/abertura.py:168` | filtro ORM `aberto_por_user_id` |
| `services/contexto.py:70-71`, `:97-100`, `:118-123`, `:144-156` | todas as chaves dinâmicas do ato: `.get(k)=None`, pertencimento e listas; ler e escrever no mesmo módulo |
| `services/feriado_reavaliar.py:78` | filtro ORM `data_turno__in` |
| `services/pergunta_no_app.py:114` | data catalogada |
| `services/cobranca_folha.py:32` | data catalogada |
| `services/materializacao.py:135` | contexto delegado a `data_evento_do_contexto` |
| `services/celula_carimbada.py:45`, `:149` | carrega campo completo e delega dia ao juiz |
| `rejuncao.py:83` | data catalogada |
| `views.py:441` | `tipo_escala_id=None` |
| `views.py:1512` | `ausencia_id=None` |
| `signals.py:361` | ORM `chamados_orfaos_ids__contains` |
| `management/commands/nomear_marco_por_lampada.py:50`, `:65`, `:70` | `marcos_faltantes=None → []`, `horario_previsto=None`, `origem=None` |
| `management/commands/auditar_invariantes_chamados.py:73`, `:93`, `:104`, `:617`, `:686`, `:1016` | ORM data_turno; `entrada_timestamp=None`; chave catalogada por `MOTIVOS[m]['chave_data']` |
| `app/templates/chamados/partials/painel_chamado_turno.html:35-36` | **todas as chaves e valores por `.items`** |
| `app/templates/chamados/partials/modal_fio.html:481-488` | coordenadas batida/posto, distância, raio; raio default `200`, distância `0`, posto_nome/endereço `''` |
| `app/templates/chamados/partials/modal_fio.html:529` | `batida_id`, default `''` |
| `app/templates/chamados/partials/modal_fio.html:544` | `sugestao.classe` |
| `app/templates/chamados/partials/modal_fio.html:561-578` | `classe,evidencia,destino.codigo,destino.n_colabs` |
| `app/templates/chamados/partials/modal_fio.html:588-594` | `motivo_anuencia,tipo_batida,hora_tentativa` |
| `app/templates/chamados/partials/_relatorio_geo.html:18-33` | tipo_batida default `'-'`; tipo_alerta; distância; raio default `'-'`; posto_nome default `'-'`; endereço e coordenadas |
| `app/templates/chamados/detalhe_local.html:227`, `:245-253` | `tem_documento,motivo_anuencia,tipo_batida,hora_tentativa`, últimos dois default `'—'` |
| `app/templates/chamados/partials/_card_atendimento.html:65-70`, `:232-235` | `tem_documento,ausencia_id,data_turno` |

**Leitores externos efetivamente encontrados**

Estes são consumidores do mesmo JSON persistido e entram conforme a fronteira do usuário:

- `app/escala/utils.py:1062`, `:1066-1070`: `agrupado_em=None`; `marcos_faltantes=None → []`; marcos `tipo=None,hora=None`.
- `app/colaboradores/services/calendario.py:438`, `:448-451`: ORM `data_turno` não nulo, presença de `agrupado_em`; `.get('agrupado_em')=None`.
- `app/colaboradores/services/pos_vinculo.py:38-41`: `periodo=None → []`, só restringe datas se tamanho 2.
- `app/escala/services/regeneracao.py:149`, `:196`: `marcos_faltantes=None`; por marco `hora=None`.
- `app/ponto/services/lastro.py:75-78`: `marcos_faltantes=None → []`; `hora=None`, `tipo=None → None`. Entradas pelos consumidores de `:110`, `:170`, `:273`, `:346`.
- `app/ponto/services/entrada_sincronizada.py:51`, `:55`: ORM `chamados_orfaos_ids__contains`; `.get(None)` comparado com lista unitária.
- `app/ponto/services/cartorio.py:136`, `app/ponto/services/cobertura.py:230`, `app/ponto/management/commands/supra_juiz.py:200`, `app/ponto/management/commands/mapa_divergencia.py:293`: data catalogada.
- `app/ponto/worklist.py:54-55`: `pendentes=None`.
- `app/ponto/management/commands/reconciliar_grade.py:376-383`: `orfao_14h=None`; nested `data=None,entradas_pendentes=None → []`, entradas `data=None`; `pendentes=None`.
- `app/ponto/management/commands/reconciliar_grade.py:445-449`, `:465-467`: `orfa_regeneracao=None`, `marcos_faltantes=None → []`, marcos `hora=None`.
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:63`, `:112`, `:147`, `:172`, `:188`: `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h`, todos `.get(None)`.
- `app/ponto/management/commands/reconciliar_fantasmas.py:183-184`: `pendentes=None`.
- `app/ponto/management/commands/curar_residuo_bug115.py:76`, `:83`, `:86`, `:121`: `marcos_faltantes=None → []`, `horario_previsto=None`, `origem=None`, `marcos_candidatos=None`.
- `app/ponto/management/commands/gerar_celulas.py:259-260`: `marcos_faltantes=None → []`; marcos `hora=None`.
- `app/ponto/management/commands/detectar_ausencias.py:557`: `marcos_faltantes=None`.
- `app/ponto/services/faltas_de_hoje.py:64`, `:85`; `app/ponto/services/furos_sem_cobranca.py:69`, `:130`; `app/ponto/services/vigia_de_hora.py:112-113`; `app/ponto/services/veredito_celula.py:150`; `app/ponto/management/commands/flip_automatico.py:76`; `app/ponto/management/commands/processar_alertas_turno.py:166`, `:181`, `:338`: filtros/projeções ORM por `data_turno`.
- `app/api/views.py:1045`, `:3114`: `ausencia_id=None`, filtro ORM pelo mesmo ID; `:1956-1957` filtro data_turno.
- `app/ponto/services/ausencia.py:662`, `:712`, `:715`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:32`: `ausencia_id`, `.get(None)`.
- `app/ponto/services/justificativa.py:57-58`: filtro data_turno.
- `app/escala/regua_defesa.py:54`, `:328`, `:334`: filtro tipo_escala_id; `destino=None → {}`, nested `codigo=None`; `tipo_escala_codigo=None`.
- `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`: `classe=None`.
- `app/ponto/services/geofence.py:403-406`, chamado em `:425`: `ocorrencias=None`, ocorrências `batida_id=None,batida_lat=None,batida_lon=None`; fallback das três chaves no topo.

Leitores da **resposta derivada** de `dias_do_fato`, além do próprio JSON: `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`, `services/juizes_do_dia.py:94`, `services/fila_por_causa.py:162`, `:319`, `:367`, e `management/commands/nomear_marco_por_lampada.py:51`; os consumidores externos do juiz são chamados em `app/colaboradores/management/commands/marcar_feriado_do_posto.py:106`, `corrigir_escala_retroativa.py:142`, `app/ponto/management/commands/curar_residuo_bug115.py:77`, `app/escala/services/propositor.py:633`.

**Escritores externos da mesma estrutura**

Para completar TODOS os emissores do JSON, estes também foram encontrados; avaliar na consolidação se unidade trata nascimento delegado à porta interna como emissão aqui:

- `app/colaboradores/views.py:2376`: `agenda_dia_ids`.
- `app/colaboradores/services/vinculo.py:419`: `snapshot_ecs,ec_nova,template_id,template_nome,data_inicio`.
- `app/colaboradores/services/desligamento.py:182`: `data_demissao,motivo_informado,executado_por_username,executado_por_id,executado_em`.
- `app/ponto/utils.py:120`: `agendamento_pk,data_inicio,data_fim`.
- `app/ponto/views.py:280`, persistência `:288` ou `:301`: `pendentes,total`.
- `app/escala/regua_defesa.py:72`: `tipo_escala_id,tipo_escala_codigo,causa,data_exemplo`.
- `app/api/views.py:714-726`, gêmeo `app/api/views_core.py:708-720`: dict do detector mais `batida_id,data_turno`.
- `app/api/views.py:1978`: `data_turno`; `:3120`: `tem_documento=True`.
- `app/ponto/services/justificativa.py:301`: `data_turno,tipo,motivo_rejeicao,rejeitado_por`.
- `app/ponto/services/cluster_espurio.py:211`: `data_turno,orfas,n_missing`.
- `app/ponto/services/geofence.py:375`: `ocorrencias,alerta_geofence_id,batida_id,tipo_alerta,distancia_metros,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon,batida_lat,batida_lon,tipo_batida`; agregação de ocorrências via nota em `:359`.
- `app/ponto/services/ausencia.py:672`: `ausencia_id,data,tipo,tem_documento`.
- `app/ponto/services/triagem_batida.py:117`: `motivo,situacao,posto`; `:347`: `tipo_batida,hora_tentativa`; `:353-371` renovação `motivo_anuencia,tipo_batida,hora_tentativa`, opcional `foto_b64,foto_nome`; `:470`: `tipo_batida,hora_tentativa,hora_prevista,diferenca_minutos,escala`.
- `app/ponto/management/commands/detectar_par_relampago.py:383`: `data_turno,par,duracao_min`.
- `app/ponto/management/commands/processar_alertas_avancados.py:126`: `data_turno,entrada_timestamp`.
- `app/ponto/management/commands/reconciliar_grade.py:516`: `data_turno,origem,marcos_faltantes[{tipo,hora,motivo}]`; `:473`: `marcos_faltantes,orfa_regeneracao_curada`; retira `orfa_regeneracao` em `:454`, `:474`.
- `app/ponto/management/commands/detectar_ausencias.py:224`: `escala_id,tipo_escala_id,tipo_escala_codigo,tipo_ciclo,colabs_mesmo_template,causa,data_inicio`; `:294`: `escala_id,tipo_ciclo,data_inicio`; `:532`: `data_inicio,causa,escala_id`; `:538`: soma `dias_sem_grade`; `:559`: `marcos_faltantes,_marco_por`; `:706`: `data_turno,horario_previsto,atraso_min`, mais `marcos_faltantes` pelo argumento ou helper; `:766`: `folga_nao_declarada` e companheiras a conferir [nao verificado].
- `app/ponto/management/commands/processar_alertas_turno.py:348`: `data_turno,entrada_timestamp,posto`.
- `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:156`: `_ctx`, shape não concluído [nao verificado].
- `app/ponto/management/commands/gerar_celulas.py:264`: `orfa_regeneracao={em,marcos_na_emissao}`.
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:64`, `:138`, `:163`, `:179`, `:195`: carimbos de escalada listados acima.
- `app/ponto/management/commands/curar_residuo_bug115.py:124`: `marcos_candidatos,_marco_por`, demais companheiras [nao verificado].

**Desencontros literais comprovados e selos**

1. `abertura.py:183-191` → `juizes.py:187-197`: emissor grava chave catalogada (`data` para ausência em `catalogo/modulos.py:120`; `data_inicio` aparece também em `adesao.py:86`), mas o leitor em lote só lê `data_turno` antes de procurar batida/disputa/carimbo. O singular lê catálogo em `juizes.py:114`, e `_dia_do_contexto_fraco` só lê timestamps em `:159-162`. A falta das outras âncoras pode produzir singular com data e lote sem data pela leitura estática. ESTRUTURA provável; classe sim. Selo específico da paridade de **chaves** não localizado [nao verificado]. `tests/test_e1_v9.py:357-368` cobre conversa versus fato, não essas chaves.

2. `abertura.py:183-191` → `views.py:1512`: abertura humana de `ausencia_colab` grava `data`, mas não `ausencia_id`; leitor pede `ausencia_id=None`. ESTRUTURA provável; classe sim; impacto de produção [nao verificado]. `tests/test_form_catalogo.py:76-83` sela módulo e data, sem selar ID.

3. Mesmo emissor → `juizes.py:2331`: pede `ausencia_id` ausente nessa forma humana. ESTRUTURA provável; classe sim; selo específico não localizado [nao verificado].

4. Mesmo emissor → `app/templates/chamados/partials/_card_atendimento.html:65-70`: pede `tem_documento` e usa `ausencia_id` no endpoint; ambas ausentes na forma humana. ESTRUTURA provável; classe sim; selo específico não localizado [nao verificado]. O emissor externo especializado **grava ambas** em `app/ponto/services/ausencia.py:672`: distinguir formas, sem assumir prejuízo observado.

5. `reconciliador.py:708` → `juizes.py:551`, `:576-577`: `marcos_faltantes[].motivo` vale `'lampada_apagada'` no emissor (`app/ponto/services/lastro.py:134`), mas o leitor compara com o motivo da Pergunta e recusa fallback se houver qualquer motivo nomeado. Mesmo nome para etiqueta da lâmpada e identidade da pergunta. ESTRUTURA provável; classe sim. Existem selos separados do escritor e leitor, sem selo encontrado do par com esse shape: escritor `tests/test_emissor_nomeia_o_marco.py:54-74`; leitor `tests/test_selo_juizes_orfaos.py:84-123`, `tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:33`. Cobertura específica da costura [nao verificado].

6. `nomear_marco_por_lampada.py:109` → `juizes.py:551` repete o shape com `'lampada_apagada'`, confirmado por `tests/test_emissor_nomeia_o_marco.py:159-162`. Mesma classe anterior; selo do par [nao verificado].

**Selos positivos utilizáveis**

- `services/limbo_folgas.py:68` → template `modal_fio.html:544`: `tests/test_limbo_folgas.py:97` renderiza o botão, com integração de sugestão/template em `:47-64`; conferir assertions completas para marcar par exato.
- `services/adesao.py:113` → `models.py:816`: `tests/test_agrupado_em.py:32`, `:42` prende bloqueio por pai vivo e liberação após fechamento.
- `services/limbo_folgas.py:276` → reata/fecho/agrupamento: `tests/test_t34_furo_sob_limbo.py:37`.
- `services/disputa_emissao.py:1116/:1122` → fusão: `tests/test_3b_reuso_899.py:31`; consumidor semântico `_filhos_vivos` não necessariamente selado por esse teste [nao verificado].
- `services/disputa_emissao.py:1351/:1357` → fusão: `tests/test_3b_reuso_1061.py:17`; não demonstra leitor semântico dos IDs.
- `services/contexto.py:62/:82/:131` regras de escrita: `tests/test_porta_contexto.py:92-116`, `:132-156`, `:220-257`, `:273-279`.
- `retirar_contexto` e renovação de órfãs: `tests/test_porta_chamado_e2.py:39-69`.
- fusão por nota: `tests/test_anexar_ocorrencia.py:22-48`.
- semântica de `entrada_timestamp`: `tests/test_cq_c8.py:14`.
- nascimento/junção pelo contexto: `tests/test_f1b_celula.py:17-48`, `tests/test_emissor_carimba_celula.py:68-106`.

**Atenção para consolidar contagem**

`app/templates/chamados/partials/painel_chamado_turno.html:35-36` é leitor válido de **todo** o JSON. Portanto, `escape_colab_em`, `defeito_cadastro`, `chamados_limbo_ids`, metadados e quaisquer outras chaves não podem ser declaradas “gravadas que ninguém lê” globalmente: o template as lê. Pode-se registrar ausência de leitor semântico específico como [nao verificad

[truncado em 20000 de 20392 caracteres]

## 13/11 15:43 · sessao 73e2ed51da70 · - · - · origem=Ronald (tmux)

Catálogo local de contextos: cobertura completa de escritores/leitores indiretos **[não verificado]**. A busca foi textual por `contexto_json`, `.contexto`, `contexto=`, `renovar_contexto`, `anotar_contexto`, `contexto_novo`, `criar_ou_atualizar_disputa`, seguida de leitura dos corpos; ela não demonstra fluxo dinâmico completo nem incidentes em produção **[não verificado]**.

Todos os caminhos abaixo são relativos a `app/`. `E` significa dono provável ESTRUTURA, inferido da divergência entre formatos; confirmação L-099 do caso concreto **[não verificado]**. “Sem selo” abaixo significa **selo desta costura não verificado**, nunca prova global de inexistência de teste **[não verificado]**. [nao verificado]

**Desencontros confirmados por comparação do código**

| # | Estrutura | Emissor/chaves gravadas | Consumidor/chaves lidas | Desencontro | Selo/classe |
|---|---|---|---|---|---|
| D1 | disputa de selfie | `ponto/management/commands/marcar_foto_ausente_retro.py:98`: `foto_ausente.{tipo_batida,batida_id,batida_em,_por}` | `chamados/services/disputa_emissao.py:372`: `[motivo].data`; `:387`: `ultima_batida.id` | Nenhuma das duas referências esperadas é gravada pelo emissor; `batida_id` fica em outra posição. | Teste `ponto/tests/test_bug126_selfie_invisivel.py:65` sela existência da disputa e `foto_ausente.batida_id/_por` (`:69-72`), não leitura pelo consumidor. Selo da costura **[não verificado]**; E; classe sim. |
| D2 | mesma | mesmo emissor/chaves D1 | `chamados/models.py:3464`: `ultima_batida.id`, default implícito `None` | O nascimento da identidade do episódio não consulta `foto_ausente.batida_id`. | Mesmo limite de teste D1; E; classe sim. |
| D3 | mesma | mesmo emissor/chaves D1 | `api/foto_recebida.py:67`: `ultima_batida`, default `{}`; `:68`: `id`, default `None` | O fechamento pelo upload separado exige referência em outra posição. | Selo da costura **[não verificado]**; E; classe sim. |
| D4 | contexto `turno_aberto` | `ponto/management/commands/processar_alertas_turno.py:348`: `data_turno,entrada_timestamp,posto` | `chamados/services/disputa_emissao.py:54`: funde contexto disputa/chamado; `:59,68-74`: `entrada_sem_saida.entrada_id/entrada_aberta_em`, `ultima_batida.id` | `_data_por_grade` não consulta `entrada_timestamp`; as referências que consulta não são gravadas pelo emissor. | Selo **[não verificado]**; E; classe sim. |
| D5 | contexto `turno_aberto_24h` | `ponto/management/commands/processar_alertas_avancados.py:126`: `data_turno,entrada_timestamp` | mesmo consumidor/chaves D4 | Mesmo desencontro D4. | Selo **[não verificado]**; E; classe sim. |
| D6 | contexto `turno_aberto_24h` retroativo | `ponto/management/commands/emitir_turno_aberto_retroativo.py:128`, persistido `:156`: `data_turno,entrada_timestamp` | mesmo consumidor/chaves D4 | Mesmo desencontro D4. | Selo **[não verificado]**; E; classe sim. |
| D7 | `sem_previsao_escala.data_inicio` | `ponto/management/commands/detectar_ausencias.py:231`: `ec.data_inicio`; mesmo módulo `:532`: `data_ref` | `chamados/catalogo/modulos.py:126`: declara `data_inicio` do período; `:225`: lê chave declarada | Mesmo nome representa início do vínculo no primeiro emissor e dia sem grade no segundo (`:533`, causa `dia_sem_grade`). Consequência funcional **[não verificado]**. | Selo **[não verificado]**; E; classe sim quanto à dupla semântica. |

D7 contém **dois pares gravador–leitor**, portanto os sete registros acima representam **oito costuras comparadas**, com desencontro estático; não equivalem ao total do módulo **[não verificado]**.

**Outros escritores encontrados e chaves**

| ID | Estrutura/emissor | Chaves |
|---|---|---|
| W1 | entrada bloqueada, `ponto/views.py:280`, renovação `:288`, nascimento `:301` | `pendentes` = resumo textual BR; `total` |
| W2 | acesso bloqueado, `ponto/services/triagem_batida.py:117` | `motivo,situacao,posto` |
| W3 | fora horário, nascimento `ponto/services/triagem_batida.py:347` | `tipo_batida,hora_tentativa` |
| W4 | fora horário, renovação `ponto/services/triagem_batida.py:353`, gravação `:371` | `motivo_anuencia,tipo_batida,hora_tentativa`; condicionais `foto_b64,foto_nome` (`:359-362`) |
| W5 | outro nascimento fora horário, `ponto/services/triagem_batida.py:470` | `tipo_batida,hora_tentativa,hora_prevista,diferenca_minutos,escala` |
| W6 | disputa saída sem entrada, `ponto/services/triagem_batida.py:738` | `saida_sem_entrada.{data,hora_tentativa}` |
| W7 | furo detector, `ponto/management/commands/detectar_ausencias.py:706` | `data_turno,horario_previsto,atraso_min,marcos_faltantes`; forma `{tipo,hora,motivo}` descrita e construída pelo helper a partir de `:476` |
| W8 | atualização do furo, `ponto/management/commands/detectar_ausencias.py:559` | `marcos_faltantes,_marco_por` |
| W9 | furo grade, `ponto/management/commands/reconciliar_grade.py:516` | `data_turno,origem,marcos_faltantes[{tipo,hora,motivo}]` |
| W10 | órfã curada, `ponto/management/commands/reconciliar_grade.py:465`, gravação `:473` | `marcos_faltantes,orfa_regeneracao_curada`; esta copia a marca anterior e acrescenta `relavrada_em,marcos_antes` |
| W11 | marca de órfã, `ponto/management/commands/gerar_celulas.py:264` | `orfa_regeneracao.{em,marcos_na_emissao}` |
| W12 | candidatos, `ponto/management/commands/curar_residuo_bug115.py:124` | `marcos_candidatos,_marco_por` |
| W13 | ausência, `ponto/services/ausencia.py:672` | `ausencia_id,data,tipo,tem_documento` |
| W14 | rejeição justificativa, `ponto/services/justificativa.py:301` | `data_turno,tipo,motivo_rejeicao,rejeitado_por` |
| W15 | cluster, `ponto/services/cluster_espurio.py:211` | `data_turno,orfas,n_missing` |
| W16 | par relâmpago, `ponto/management/commands/detectar_par_relampago.py:383` | `data_turno,par,duracao_min` |
| W17 | férias, `ponto/utils.py:120` | `agendamento_pk,data_inicio,data_fim` |
| W18 | limbo folgas, `ponto/management/commands/detectar_ausencias.py:224` | `escala_id,tipo_escala_id,tipo_escala_codigo,tipo_ciclo,colabs_mesmo_template,causa,data_inicio` |
| W19 | âncora limbo, `ponto/management/commands/detectar_ausencias.py:294` | `escala_id,tipo_ciclo,data_inicio` |
| W20 | sem grade, `ponto/management/commands/detectar_ausencias.py:532` | `data_inicio,causa,escala_id`; ocorrência agregada `dias_sem_grade` em `:538` |
| W21 | ressalva folga, `ponto/management/commands/detectar_ausencias.py:766` | `folga_nao_declarada,nao_calado_porque` |
| W22 | geofence, `ponto/services/geofence.py:345`, nascimento `:375` | `ocorrencias[{batida_id,alerta_geofence_id,distancia_metros,batida_lat,batida_lon,tipo_batida}]`; topo `alerta_geofence_id,batida_id,tipo_alerta,distancia_metros,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon,batida_lat,batida_lon,tipo_batida` |
| W23 | nova ocorrência geofence, `ponto/services/geofence.py:359` | agrega `ocorrencias` com a mesma forma W22; preservação/fusão pela porta `chamados/services/nota.py:110` |
| W24 | escalonamento, `ponto/management/commands/escalonar_chamados_supervisao.py:64,138,163,179,195` | respectivamente `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h` |
| W25 | incidente da disputa, `ponto/management/commands/escalonar_chamados_supervisao.py:159` | `incidente_operacional.{marcado_em,idade_min_no_marco}` |
| W26 | alerta da disputa, `ponto/management/commands/alertar_disputas_pendentes.py:48` | `ultimo_alerta_diario` |

**Pares funcionais adicionais verificáveis — uma linha por par com emissores expandidos abaixo**

Nas linhas com `W7/W8/W9/W10`, cada ID é um emissor distinto e deve virar uma linha na consolidação; mesma regra para W3/W4/W5. Essa é uma matriz de expansão, não uma alegação de que a unidade seja a estrutura **[não verificado]**.

| Emissor | Consumidor | Chaves lidas/defaults |
|---|---|---|
| W1 | `ponto/worklist.py:54` | `pendentes`, default `None` |
| W1 | `ponto/management/commands/reconciliar_fantasmas.py:183` | `pendentes`, default `None` |
| W1 | `ponto/management/commands/reconciliar_grade.py:381` | `pendentes`, default `None`; reconhece formato BR |
| W1 | `chamados/data_evento.py:27` | tenta `data_turno,data_batida,entrada_timestamp`; depois `pendentes` (`:31`), depois `horario_previsto` (`:39`); todos default `None` |
| W3 | `chamados/services/acoes_chamado.py:30` | `tipo_batida='E',hora_tentativa='',motivo_anuencia=''`; `foto_b64=None` (`:57`), `foto_nome='selfie.jpg'` (`:61`) |
| W4 | mesmo leitor W3 | mesmas chaves/defaults |
| W5 | mesmo leitor W3 | mesmas chaves/defaults |
| W3 | `templates/chamados/partials/modal_fio.html:588` | `motivo_anuencia,tipo_batida,hora_tentativa` (`:592-594`) |
| W4 | mesmo template | mesmas chaves |
| W5 | mesmo template | mesmas chaves |
| W3 | `templates/chamados/detalhe_local.html:245` | `motivo_anuencia,tipo_batida` (`:249`, default “—”), `hora_tentativa` (`:251`, default “—”) |
| W4 | mesmo template | mesmas chaves/defaults |
| W5 | mesmo template | mesmas chaves/defaults |
| W6 | `chamados/catalogo/query.py:58` | `[motivo].data`, default `None` |
| W6 | `chamados/services/disputa_emissao.py:372` | `[motivo].data`, default `None` |
| W6 | `chamados/juizes.py:391` | `[p.motivo].data`, default `None` |
| W6 | `chamados/services/materializacao.py:346` | `[p.motivo].data`, default `None`; usado se `p.data_evento` ausente (`:347-350`) |
| W6 | `chamados/services/materializacao.py:478` | mesma chave/default; usado se `p.data_evento` ausente (`:479-482`) |
| W7/W8/W9/W10 | `ponto/services/lastro.py:75` | `marcos_faltantes` default `[]`; item `hora=None,tipo=None` (`:76-78`) |
| W7/W8/W9/W10 | `chamados/juizes.py:549` | `marcos_faltantes` default `[]`; item `motivo=None,hora=None` (`:551`) |
| W7/W8/W9/W10 | `escala/utils.py:1066` | `marcos_faltantes` default `[]`; item `tipo=None,hora=None` (`:1070`) |
| W7/W8/W9/W10 | `escala/services/regeneracao.py:148` | `marcos_faltantes=None`; ausência retorna `None` (`:149-150`) |
| W7/W8/W9/W10 | `chamados/services/disputa_emissao.py:1143` | `marcos_faltantes` default `[]`; itens usados para complementar perguntas |
| W7 | `ponto/services/vigia_de_hora.py:112` | lookup/`values_list` de `contexto_json__data_turno` |
| W9 | mesmo consumidor W7 | mesma chave |
| W11 | `ponto/management/commands/reconciliar_grade.py:445` | `orfa_regeneracao=None`; copia dict inteiro (`:465`) |
| W13 | `chamados/views.py:1512` | `ausencia_id=None` |
| W13 | `api/views.py:1045` | `ausencia_id=None` |
| W13 | `templates/chamados/partials/_card_atendimento.html:65` | `tem_documento`; usa `ausencia_id` na rota (`:70`) |
| W13 | `templates/chamados/detalhe_local.html:227` | `tem_documento` |
| W13 | `ponto/management/commands/reabrir_chamados_ausencia_aberta.py:32` | `ausencia_id=None` |
| W13 | `chamados/juizes.py:2331` | `ausencia_id=None`; consulta data da Ausencia (`:2335`) |
| W13 | `chamados/catalogo/modulos.py:120`, leitor `:225` | `data`, default `None` |
| W14 | `chamados/catalogo/modulos.py:117`, leitor `:225` | `data_turno`, default `None` |
| W15 | `chamados/catalogo/modulos.py:115`, leitor `:225` | `data_turno`, default `None` |
| W16 | `chamados/reconciliador.py:198` | `par=None`; usa cada hora para localizar batidas (`:212-215`) |
| W16 | `chamados/catalogo/modulos.py:113`, leitor `:225` | `data_turno`, default `None` |
| W17 | `chamados/catalogo/modulos.py:123`, leitor `:225` | `data_inicio`, default `None` |
| W18 | `chamados/services/limbo_folgas.py:64` | `tipo_escala_id=None` |
| W18 | `chamados/views.py:441` | `tipo_escala_id=None` |
| W18 | `chamados/catalogo/modulos.py:126`, leitor `:225` | `data_inicio`, default `None` |
| W20 | mesmo consumidor W18 | `data_inicio`, default `None`; dupla semântica D7 |
| W22 | `ponto/services/geofence.py:403` | `ocorrencias=None`; item `batida_id,batida_lat,batida_lon=None`; fallback topo mesmas três (`:406`) |
| W23 | mesmo consumidor W22 | mesmas chaves/defaults |
| W22 | `chamados/services/acoes_chamado.py:184` | `distancia_metros,alerta_geofence_id,tipo_batida,posto_nome,raio_metros,posto_lat,posto_lon,batida_lat,batida_lon` (`:185-210`), defaults `None` ou apresentação `—` |
| W22 | `chamados/services/acoes_chamado.py:246` | `batida_id=None`, `ocorrencias=[]`, item `batida_id=None` |
| W23 | mesmo consumidor `:246` | mesmas chaves |
| W22 | `templates/chamados/partials/_relatorio_geo.html:18` | `tipo_batida` default `-`; `tipo_alerta` (`:19`); `distancia_metros,raio_metros` (`:22`); `posto_nome,posto_endereco,posto_lat,posto_lon,batida_lat,batida_lon` (`:26-33`) |
| W22 | `templates/chamados/partials/modal_fio.html:481` | `batida_lat,batida_lon,distancia_metros,raio_metros,posto_lat,posto_lon,posto_nome,posto_endereco` (`:483-488`); `raio_metros` default `200`, distância `0`, nomes/endereço `''`; `batida_id` default `''` (`:529`) |
| W25 | `chamados/models.py:2101` | porta `anotar_contexto`: lê chave dinâmica `incidente_operacional` e preserva o primeiro valor (`:2105-2106`) |
| W26 | `chamados/models.py:2119` | porta `renovar_contexto`: lê chave dinâmica `ultimo_alerta_diario`, compara valor e grava (`:2123-2124`) |

A matriz acima contém **65 pares**, contando as quatro linhas W7/W8/W9/W10 como quatro pares cada; conferência aritmética manual **[não verificado]**. Não certifico desencontro/classe/selo de todas essas linhas sem revisão individual **[não verificado]**. Nos pares W3 e W5 → `acoes_chamado`, `motivo_anuencia/foto_b64/foto_nome` não são gravadas nesses nascimentos, mas têm defaults explícitos; classificação como contrato quebrado em vez de opcionalidade **[não verificado]**, pelas leituras de `chamados/services/acoes_chamado.py:30-61` e escritores W3/W5.

**Três escritores de turno para quatro leitores adicionais**

Cada uma das seguintes linhas gera três pares, usando os emissores D4/D5/D6:

| Consumidor | Chaves |
|---|---|
| `chamados/juizes.py:2163` | `entrada_timestamp=None`, normaliza `T`/espaço (`:2164`) |
| `chamados/reconciliador.py:146` | `entrada_timestamp=None`, procura entrada exata normalizada (`:148-152`) |
| `chamados/services/disputa_emissao.py:145` | `entrada_timestamp=None`, identifica chamado do turno |
| `chamados/catalogo/modulos.py:107`/`:109`, leitor `:225` | `data_turno=None` |

Esses são **12 pares adicionais** por expansão dos três emissores citados; completude de leitores **[não verificado]**.

**Leitores genéricos e limites para “ninguém lê”**

- `templates/chamados/partials/painel_chamado_turno.html:35` percorre `chamado.contexto_json.items`: portanto qualquer chave de W1–W5 e W7–W24 pode ter consumo genérico de apresentação; alcance efetivo desse template por cada chamado **[não verificado]**.
- `chamados/services/contexto.py:70,97,118,144` copia/lê o contexto inteiro nas portas, inclusive chaves que não têm leitor funcional nomeado.
- `chamados/services/disputa_emissao.py:54-58` copia contexto da disputa e itens do chamado inteiro. Isso impede declarar “ninguém lê `foto_ausente.batida_id`” no sentido de transporte; o que está confirmado é que os três consumidores D1–D3 não consultam essa chave como referência da batida.
- `marcos_candidatos` tem auto leitura no próprio emissor em `ponto/management/commands/curar_residuo_bug115.py:121`; leitor funcional em outro módulo **[não verificado]**. O genérico de contexto acima existe.
- `incidente_operacional` tem leitura de idempotência pela porta `chamados/models.py:2101`; leitor funcional de `marcado_em/idade_min_no_marco` **[não verificado]**. O teste `chamados/tests/test_porta_anotacao_contexto.py:201-212` caracteriza preservação do primeiro marco, não consumo operacional dessas subchaves.
- `ultimo_alerta_diario` é lido pela porta `chamados/models.py:2119`; teste genérico de renovação está em `chamados/tests/test_porta_anotacao_contexto.py:214-230`, mas selo com esse nome de chave concreto **[não verificado]**.
- Escritor externo complementar da estrutura W13: `api/views.py:3120` renova `tem_documento=True`; os leitores de `tem_documento` não dependem apenas do nascimento em ponto. Outros escritores externos de todas as estruturas **[não verificado]**.

Não fechar total do módulo com esta entrega: ela identifica escritores, leitores e expansões rastreados, mas não demonstra o produto completo gravador–leitor, os transportes genéricos alcançáveis, nem todos os escritores de módulos externos das mesmas estruturas **[não verificado]**.

## 13/11 15:43 · sessao 823646fecf8a · - · - · origem=Ronald (tmux)

Método: busca textual por `**campos`, `**kwargs`, `payload`, `EventoCelula`, `valor_antes/depois` e nomes das portas, seguida de leitura dos emissores, consumidores e testes. Os resultados abaixo são estáticos; execução, população de produção e completude global permanecem [não verificado]. [nao verificado]

`ponto/` abaixo significa `app/ponto/`; `tests/` significa `app/ponto/tests/`. A unidade destas linhas é função emissora × módulo consumidor × estrutura, preservando emissores distintos no mesmo arquivo. [não verificado]

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | Desencontro | Selo | Dono provável / classe |
|---|---|---|---|---|---|---|---|
| WebPush central | `ponto/webpush.py:44` | `title, body, url` | `app/static/sw.js:88` | `title || 'Hasner Ponto'`, `body || 'Nova notificação'`, `urgente || false`; clique `:102-107` | `urgente` não é gravada; `url` é transportada como `data` mas o clique abre `/` | Não identificado [não verificado] | ESTRUTURA / sim, inferência pelas chaves |
| WebPush N3 | `ponto/management/commands/processar_alertas_avancados.py:207` | `title, body, url, requireInteraction=True` | `app/static/sw.js:88` | mesmas leituras acima | Emissor grava `requireInteraction`, leitor usa `urgente`; clique não usa `url` | Não identificado [não verificado] | ESTRUTURA / sim, inferência pelas chaves |
| WebPush DP | `ponto/management/commands/processar_alertas_avancados.py:240` | `title, body, url, requireInteraction=True` | `app/static/sw.js:88` | mesmas leituras acima | Igual ao N3 | Não identificado [não verificado] | ESTRUTURA / sim, inferência pelas chaves |
| Resultado WebPush | `ponto/webpush.py:39`, `:43`, `:73-76` | `ok, motivo` em todos os retornos | `app/holerite/servicos.py:42-48` | `.get('ok')`; retorna o dict inteiro em sucesso ou fallback | `motivo` é repassado; não há ausência de chave constatada | `app/holerite/tests/test_selo_sem_notificacao.py:375` testa fallback com `ok,motivo`; cobertura integral [não verificado] | ESTRUTURA / não |
| NOTIFY batida | `ponto/realtime.py:32-43` | `tipo='batida', colab_id, batida_id, ts` | `app/sse_server.py:73-93` | `n.payload` inteiro, sem ler chaves | Nenhuma chave é interpretada pelo relay; consumidor final dessas chaves não localizado [não verificado] | Não identificado [não verificado] | ESTRUTURA / [não verificado] |
| Outbox EventoCelula | `ponto/portas/celula.py:945-954` | `celula_id, colaborador_id, data`; defaults `criado_em`, `processado_em=None`, `tentativas=0`, `erro=''` em `app/escala/models.py:1857-1863` | `app/chamados/reconciliador.py:1247-1263`, `:1344-1409` | `processado_em, tentativas, criado_em, pk, colaborador_id`; marca `erro/tentativas/processado_em` | Não há ausência constatada; o módulo também usa `celula_id` na semeadura/vigia, portanto não é chave globalmente órfã (`:1280` e chamadas relacionadas) | Sim: `tests/test_o_outbox_celula.py:56-61`, `:142-148`, `:163-180` | ESTRUTURA / não |
| Outbox EventoCelula | `ponto/portas/celula.py:954` | `celula_id, colaborador_id, data` | `app/chamados/services/fabrica_da_celula.py:288-301` | `e.data`; colaborador vem do argumento; relê células atuais | `celula_id` deliberadamente não é autoridade neste leitor, conforme `:291-292`; não classificar isoladamente como defeito | Selo específico desse consumo não localizado [não verificado] | ESTRUTURA / não |
| Resultado decidir HE | `ponto/portas/he.py:178-179`, `:203` | `estado,minutos,criada,trocada,no_op` | `ponto/views.py:529-541` | `no_op,estado,trocada,minutos` | `criada` não é lida nessa view; ausência global de leitores [não verificado] | Sim parcial: `tests/test_atomicidade_porta_he.py:50`, `:90-91`; `tests/test_tela_gestao_he.py:254-274` | ESTRUTURA / não |
| Resultado recusar lote | `ponto/portas/he.py:234-246` | `recusados,no_op,erros` | `ponto/views.py:614-625` | as três chaves, indexadas | Sem desencontro constatado | Sim: `tests/test_tela_gestao_he.py:375-390`, `:444-455` | ESTRUTURA / não |
| Resultado autorizar lote | `ponto/portas/he.py:545-571` | `dias,autorizados,no_op,erros` | `ponto/services/autorizacao_he_periodo.py:277-289` | `erros`; preserva dict, sobrescreve `dias`, acrescenta `antes,depois,diff,exportada,periodo` | Sem ausência constatada; preserva o contrato com extensão explícita | Selo do resultado completo não localizado [não verificado] | ESTRUTURA / não |
| Resultado ponta pequena | `ponto/portas/he.py:330`, `:335`, `:344` | `recusada,ja_decidido,minutos` | `ponto/management/commands/recusar_ponta_pequena.py:170-183` | `recusada,minutos` | `ja_decidido` não é lida pelo comando; ausência global [não verificado] | Sim parcial: `tests/test_o273_recusa_do_sistema_envelhece.py:238` | ESTRUTURA / não |
| Resultado revogar recusa | `ponto/portas/he.py:448-477` | `revogada,por_que_nao,minutos_na_decisao,minutos` em todos os ramos | `ponto/management/commands/lavrar_he_pendente.py:56-60` | `['revogada']` | Demais chaves não são lidas nesse comando; são exercitadas nos testes, não bastando para afirmar leitor operacional | Sim: `tests/test_o273_recusa_do_sistema_envelhece.py:130`, `:163`, `:208`, `:273-278` | ESTRUTURA / não |
| Resultado regen exportada | `ponto/portas/regen_exportada.py:221-225` | `celulas,vinculos,sem_cobertura,ambiguos,hash_antes,hash_depois,antes,depois,mudou,campos_que_mudaram,regua_antes,regua_depois` | `app/chamados/services/passivo_validado.py:178-208`, `:444-449` | `.get('antes')`, `.get('depois') or {}`, `.get('campos_que_mudaram') or []`, `regua_antes,regua_depois,hash_antes,hash_depois,celulas,mudou` | `vinculos,sem_cobertura,ambiguos` não são lidas neste consumidor; ausência global [não verificado] | `tests/test_regen_em_exportada.py:91-111` prende os dois hashes na trilha; retorno completo [não verificado] | ESTRUTURA / não |
| `fatos_do_status` | `ponto/status_dia.py:189-192`; chamadas reais `ponto/services/cartorio.py:935`, `:1296`, `:1857` | `tipo_dia,previsto_min,tem_batida,isento,cobertura_parcial,folga_humana` | `ponto/portas/celula.py:1009-1021`, expandindo para `ponto/status_dia.py:113-114` | todos; os dois últimos têm defaults `False` na função final | Sem desencontro constatado | AST exige passagem do argumento: `tests/test_selo_veredito_so_pela_porta.py:158-167`, `:268-287`; esquema completo não fica provado pelo AST | ESTRUTURA / não |

As portas de batida recebem mapas abertos ou mapas de anotação com validação distinta: `registrar_batida(**campos)` sanitiza somente `latitude,longitude,gps_accuracy`, depois entrega todas as chaves ao ORM (`ponto/registro_batida.py:3-18`, `:94-105`, `:152-158`); `anotar(campos)` restringe ao conjunto `tipo,aprovada,aprovada_por,aprovada_em,origem,retratada_em,foto,justificativa,pergunta_origem` (`:203-205`, `:241-254`). Não existe esquema próprio completo para `**campos` nessa porta; validação efetiva adicional do ORM não foi executada. [não verificado]

| Estrutura de entrada | Escritor em ponto | Chaves escritas | Leitor | Desencontro / selo |
|---|---|---|---|---|
| kwargs de nascimento web | `ponto/services/triagem_batida.py:702-713`; submapa `:680-685` | `canal,sequencia,latitude,longitude,gps_accuracy,justificativa,aprovada`; opcionais `ordem_substituicao,posto_efetivo` | `ponto/registro_batida.py:94-105`, `:152-158` | Nenhuma chave exigida pela sanitização precisa estar presente; sem desencontro constatado. Selo completo de esquema [não verificado] |
| kwargs de materialização humana | `ponto/services/veredito_celula.py:323-324` | `aprovada_por,is_intervalo` | mesma porta | Sem desencontro constatado; selo completo [não verificado] |
| kwargs de saída por justificativa | `ponto/services/justificativa.py:188-194`, `:361-368` | primeiro: `justificativa,aprovada,aprovada_por`; retro acrescenta `aprovada_em` | mesma porta | Chave adicional tem natureza opcional; sem desencontro constatado. Selo completo [não verificado] |
| kwargs de aprovação em lote | `ponto/views.py:1815-1821` | `justificativa,aprovada,aprovada_por` | mesma porta | Sem desencontro constatado; selo completo [não verificado] |
| mapa de anotação de aprovação offline | `ponto/views.py:472-475` | `aprovada,aprovada_por,aprovada_em,origem`; substitui a trilha automática por `_snap` | `ponto/registro_batida.py:235-263` | Campos aceitos pelo conjunto fechado; trilha sob outro esquema é explícita, mas todos os leitores dela [não verificado] |
| mapa de flip | `ponto/services/flip_batida.py:30-33` | `campos.tipo`; trilha antes `tipo`, depois `tipo,causa` | `ponto/registro_batida.py:235-263` | As chaves são aceitas; preserva os mapas fornecidos |
| mapa de retratação humana | `ponto/services/retratar_humano.py:179-182` → `ponto/registro_batida.py:274-276` | `aprovada=False,retratada_em`; extra `motivo_retratacao.{id,nome},observacao` | `ponto/registro_batida.py:235-263` | `extra` mescla e pode sobrescrever chaves de `depois` (`:262`); colisão efetiva neste emissor não ocorre |
| mapa de desfazer retratação | `ponto/services/retratar_humano.py:207-209` | `aprovada=True,retratada_em=None` | mesma porta | Campos aceitos; selo da janela e efeito em `tests/test_porta_retratar_batida.py:132-169` |

Todas as oito linhas anteriores têm dono provável ESTRUTURA e classe “não”, por comparação dos campos com `ponto/registro_batida.py:203-205`, `:241-263`; cobertura de outros emissores fora dos pontos citados [não verificado].

Auditoria semanticamente consumida:

| Estrutura | Emissores / chaves | Consumidor / chaves | Desencontro / selo / classe |
|---|---|---|---|
| Trilha de retratação humana | `ponto/services/retratar_humano.py:179-182` + `ponto/registro_batida.py:251-263`: `aprovada,retratada_em,motivo_retratacao.{id,nome},observacao` | `ponto/services/retratar_humano.py:95-100`: `.get('motivo_retratacao') or {}` → `.get('nome') or ''`, `.get('observacao') or ''`; `:243-244` agrupa `nome` com default `(sem motivo registrado)` | `motivo_retratacao.id` não é usado por esses dois leitores; leitor global ausente [não verificado]. Selo positivo do esquema e agrupamento `tests/test_porta_retratar_batida.py:108-110`, `:190-199`. Dono ESTRUTURA, classe não |
| Trilha de flip | `ponto/services/flip_batida.py:33` + `ponto/registro_batida.py:263`: antes `tipo`, depois `tipo,causa` | `ponto/services/flip_batida.py:73-80`: `tipo` default `?`, `causa` default `humano`; normaliza string para `{tipo:...}` em `:46-53` | Sem desencontro entre escritor atual e leitor; compatibilidade de formato legado explicitamente implementada. Teste específico do esquema não localizado [não verificado]. Dono ESTRUTURA, classe não |
| Trilha regenerar vínculo | `ponto/portas/celula.py:372-387`: `desde,ate,alteradas,barrados_dias,barrados_lavra,apesar_da_lavra,cobrancas_orfas_marcadas` | `app/escala/services/regeneracao.py:58-67`: `['desde'],['ate']`, captura `KeyError/TypeError/ValueError` | Outras chaves não são lidas nesse juiz; não afirmar ausência global. Dono ESTRUTURA, classe não; selo específico do esquema [não verificado] |
| Trilha resolver dia, variante lavra | `ponto/portas/celula.py:1085-1088`, `:1100-1104`: `veredito,via,protestos,status_dia` | `app/chamados/juizes.py:1295-1299`: `.get('via') == VIA_HUMANA or 'acoes' in dict` | Leitor admite variantes; não requer `acoes` neste emissor. Selo sim `app/chamados/tests/test_tranca_vivas_08.py:95-99`. ESTRUTURA / não |
| Trilha resolver dia, variante despachante | `ponto/services/veredito_celula.py:356-358`: `veredito,acoes,erros` | mesmo juiz `app/chamados/juizes.py:1298` | Não grava `via`; a alternativa `'acoes' in dict` está explicitamente prevista. Selo sim `app/chamados/tests/test_tranca_vivas_08.py:101-107`. ESTRUTURA / não |

Transporte da auditoria não deve ser confundido com leitura semântica: `core/observ.py:142-148` lê kwargs `acao,modelo='',objeto_id,descricao='',valor_antes,valor_depois,usuario,ip` e `:134-137` grava os blobs inteiros; `core/auditoria.py:22-24` repassa os blobs. O exportador de fotos lê todos os campos concretos em `app/chamados/services/foto_e4b.py:247-252` e inclui auditoria de Batida/Célula/Chamado/Ausência em `:310-323`; portanto “ninguém lê a chave” exige distinguir exportação integral de interpretação da chave. `app/templates/core/log_auditoria.html:55-74` não lê `valor_antes/depois`.

Ficaram sem fechamento exaustivo os consumidores semânticos de todos os demais payloads de auditoria, todos os aliases dinâmicos, todos os kwargs indiretos e o consumo final das chaves do SSE. Contagem total de `app/ponto/`, incluindo todos esses caminhos, [não verificado]. As três costuras WebPush com desencontro estão confirmadas pelo código citado; inexistência de selo específico para elas [não verificado].

## 13/11 15:42 · sessao bcbd0a8413ab · - · - · origem=Ronald (tmux)

Recorte concluído por busca nominal e leitura dos corpos, sem executar testes. A contagem depende de consolidar “emissor” como função ou módulo; abaixo deixo os sítios separados para o root expandir pares sem confundir gravação física com construção do payload. Completude para aliases/reflexão/chaves calculadas: [nao verificado]. [nao verificado]

**1. DisputaSupervisao.contexto — JSONField definido em `app/chamados/models.py:1746`.**

Escritores físicos e construtores encontrados:

| Escritor | Chaves/efeito |
|---|---|
| `app/chamados/services/disputa_emissao.py:587` | Cria contexto com `contexto_nascimento or {}` em `:590`; payload aberto, sem schema enumerado. |
| `app/chamados/services/disputa_emissao.py:548` | Merge de todas as chaves recebidas; para motivo-dict mantém anteriores, aplica novas e grava `qtd`, `ultima_em` em `:557-558`. `qtd` anterior usa `.get('qtd',1)` em `:552`. |
| `app/chamados/services/disputa_emissao.py:630` | Declaração fornece somente `{'origem':origem}`; não faz merge em disputa existente. |
| `app/chamados/services/disputa_emissao.py:1126` | `origem='manual_orfaos'`, `chamados_orfaos_ids=[pk...]`. |
| `app/chamados/services/disputa_emissao.py:1361` | `origem='ancora_limbo'`, `chamado_limbo_id`. |
| `app/chamados/services/disputa_emissao.py:292` | Escreve/atualiza `[motivo]['data']`; persistência em `:296` somente se criou perguntas. |
| `app/chamados/services/fabrica_da_celula.py:172` | Fornece `{motivo:{'data':dia.isoformat()}}` à porta persistente. |
| `app/chamados/management/commands/smoke_chamados.py:97` | Fornece `{motivo:{data}}` se template contém `{data}`, senão `{}`; chama porta em `:100-102`. |
| `app/chamados/models.py:1966` | Remove chave `[motivo]` por `pop(motivo,None)`, grava em `:1968-1969`. |
| `app/chamados/models.py:2042` | Acrescenta `revalidacoes[]` com `evento`, `em`, `motivo_removido`, mais `contexto_extra or {}`; grava em `:2065-2066`. |
| `app/chamados/models.py:2104` | `anotar_contexto`: chave dinâmica, valor aberto, somente quando `.get(chave)` é falso; grava `:2105-2106`. |
| `app/chamados/models.py:2122` | `renovar_contexto`: chave dinâmica, valor aberto, somente quando `.get(chave)!=valor`; grava `:2123-2124`. |
| `app/chamados/views.py:1343` | Chama anotação de `supervisao_omissa_escalado=timestamp ISO`. |

O detector constrói payload também persistido pela porta, via API: `app/api/views.py:433-434`, `:764-765`; gêmeo `app/api/views_core.py:419-420`, `:758-759`.

| Construtor | Chaves |
|---|---|
| `app/chamados/detector_anomalias.py:49` | `orfao_14h.{entradas_pendentes,data,hora_abertura}`. `data` pode ser `None`, inicializado em `:37`. |
| `app/chamados/detector_anomalias.py:55` | `_erro_orfao`. |
| `app/chamados/detector_anomalias.py:65` | `colisao_escala.{escalas_ids,count}`. |
| `app/chamados/detector_anomalias.py:92` | `ordem_substituicao_invalida.ordens[]` com `{ordem_id,problemas}`; atribuição ao contexto em `:98`. |
| `app/chamados/detector_anomalias.py:103` | `posto_desativado.{posto_id,posto_nome}`. |
| `app/chamados/detector_anomalias.py:111` | `sem_posto={}`. |
| `app/chamados/detector_anomalias.py:119` | `em_ferias=info` se dict, senão `{info:str(info)}`. |
| `app/chamados/detector_anomalias.py:121` | `_erro_ferias`. |

Entradas externas da porta, necessárias para conferir o exemplo:

- `app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`: `foto_ausente.{tipo_batida,batida_id,batida_em,_por}`; nenhuma `ultima_batida`.
- `app/api/views.py:758-762` e `app/api/views_core.py:752-756`: `ultima_batida.{id,tipo,timestamp}`.
- `app/ponto/services/triagem_batida.py:735-744`: `saida_sem_entrada.{data,hora_tentativa}`.
- `app/ponto/management/commands/alertar_disputas_pendentes.py:48`: `ultimo_alerta_diario`.
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`: `incidente_operacional`.
- `app/colaboradores/signals.py:81-82`: extras de revalidação `posto_id,posto_nome`.
- `app/escala/signals.py:178-182`: extras de revalidação `escala_id,ancora,origem_ancora`.

Leitores nominais de produção encontrados:

| Consumidor | Chaves lidas/defaults |
|---|---|
| `app/chamados/catalogo/query.py:58-59` | `(contexto_disputa or {}).get(motivo) or {}`; `.get('data')`, default `None`; ausência retorna `None` em `:60-61`. |
| `app/chamados/services/disputa_emissao.py:54-73` | Cópia integral; `entrada_sem_saida` default `{}`; `.get('entrada_id')`; `.get('entrada_aberta_em')`; `ultima_batida` default `{}`, `.get('id')`. |
| `app/chamados/services/disputa_emissao.py:191-196` | `[motivo]` default `{}`; `.get('data')` como fallback da data da pergunta. |
| `app/chamados/services/disputa_emissao.py:273-280` | `[motivo].entradas_pendentes` default `[]`; cada item default `{}`; `.get('pk')`. O `data` civil do item é explicitamente ignorado pelo corpo. |
| `app/chamados/services/disputa_emissao.py:345` | Passa estrutura ao interpolador de texto. |
| `app/chamados/services/disputa_emissao.py:372` | `[motivo].data`, defaults `{}` e `None`. |
| `app/chamados/services/disputa_emissao.py:387` | `ultima_batida.id`, defaults `{}` e `None`. |
| `app/chamados/services/disputa_emissao.py:551-552` | `[motivo]` anterior; `qtd`, default `1`. É autorreleitura dentro do módulo escritor. |
| `app/chamados/models.py:2102` | `.get(chave)` da porta anotadora, default `None`. |
| `app/chamados/models.py:2120` | `.get(chave)` da porta renovadora, default `None`. |
| `app/chamados/models.py:3464` | `ultima_batida.id`, defaults `{}` e `None`. |
| `app/chamados/juizes.py:391` | `[p.motivo].data`, defaults `{}` e `None`; fallback quando pergunta não traz dia. |
| `app/chamados/services/materializacao.py:346-350` | `[p.motivo].data`, defaults `{}` e `None`; só usa quando `p.data_evento` ausente. |
| `app/chamados/services/materializacao.py:478-482` | Mesmo fallback para declaração de ausência. |
| `app/chamados/reconciliador.py:1561-1569` | `[p.motivo].data`, defaults `{}` e `None`; prioriza `p.data_evento`. |
| `app/chamados/management/commands/auditar_disputas.py:75-82` | `[p.motivo].data`, defaults `{}` e `None`; produz diagnóstico. |
| `app/chamados/management/commands/auditar_disputas.py:88-91` | Para cada motivo, `.get(m)` e `.get('data')`; defaults `None`. |
| `app/chamados/management/commands/auditar_disputas.py:191` | Passa contexto ao interpolador. |
| `app/api/foto_recebida.py:67-68` | `ultima_batida` default `{}`, `.get('id')`, default `None`. |
| `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:52-55` | `device_comprometido` default `{}`; `.get('motivo')`, `.get('device_id_prefix')`, defaults `None`. |

Costuras com desencontro comprovável:

| Estrutura | Emissor | Gravadas | Consumidor | Lidas | Desencontro | Selo | Dono/classe |
|---|---|---|---|---|---|---|---|
| `Disputa.contexto` | Porta `disputa_emissao.py:666-667`, payload retro em `ponto/management/commands/marcar_foto_ausente_retro.py:98-103` | `foto_ausente.batida_id`, `batida_em`, `tipo_batida`, `_por` | `chamados/services/disputa_emissao.py:372-392` | `[motivo].data`, `ultima_batida.id` | Origem da batida existe sob chave que consumidor não procura. | Específico do retro: não localizado [nao verificado]. | ESTRUTURA provável; classe sim. |
| mesma | mesma | mesmas | `chamados/models.py:3464` | `ultima_batida.id` | Mesmo desencontro de identidade. | Específico do retro: não localizado [nao verificado]. | ESTRUTURA provável; classe sim. |
| mesma | mesma | mesmas | `api/foto_recebida.py:67-68` | `ultima_batida.id` | Upload não encontra a referência retro por `foto_ausente.batida_id`. Efeito em produção: [nao verificado]. | Específico do retro: não localizado [nao verificado]. | ESTRUTURA provável; classe sim. |

A trilha `revalidacoes` e os campos `ultima_em`, `origem`, `chamados_orfaos_ids`, `chamado_limbo_id` **dentro de Disputa.contexto** não tiveram leitor nominal de produção localizado. Não confundir com as mesmas chaves em `Chamado.contexto_json`, que têm leitores (`app/chamados/reconciliador.py:243`, `app/chamados/signals.py:361`). Ausência global de leitores dinâmicos: [nao verificado].

Selos existentes, sem execução:

- `app/chamados/tests/test_a2_data_evento_batida.py:22-44` prende contexto **canônico** com `ultima_batida.id` à data e identidade da pergunta; `:53-56` prende contexto sem essa chave ao nascimento como conversa. Não testa o emissor retro.
- `app/chamados/tests/test_hx_entradas_pendentes.py:33-41` prende os dias pendentes à emissão; `:64-71` prende ausência da Batida referenciada a nenhuma pergunta.
- `app/chamados/tests/test_porta_disputa_emissao.py:199-208` prende declaração à agregação de motivo sem alteração do contexto existente.
- `app/chamados/tests/test_porta_anotacao_contexto.py:180-230` prende primeiro valor, preservação das outras chaves, idempotência e persistência das portas de anotação/renovação.
- `app/chamados/tests/test_porta_remocao_motivo.py:268-271` prende `revalidacoes[].motivo_removido/evento`; `:344-356` prende extras e `em`. São testes do escritor, não leitor de produção da trilha.
- `app/chamados/tests/test_plantio_noturno_mais1.py:90-96` prende `detector.contexto['orfao_14h']['data']` ao dia do turno.
- `app/chamados/tests/test_smoke_ciclo_disputa.py:57-64` prende materialização de saída/entrada/âncora ao tipo da Batida; fallback exato do contexto por cada origem: [nao verificado].

**2. DisputaSupervisao.motivos — JSONField definido em `app/chamados/models.py:1742`.**

Escritores:

- `app/chamados/models.py:1967`: remove motivo respondido.
- `app/chamados/models.py:2064`: substitui pelos restantes na remoção.
- `app/chamados/models.py:2157-2161`: reabertura administrativa acrescenta motivo.
- `app/chamados/services/disputa_emissao.py:541`: união ordenada dos motivos.
- `app/chamados/services/disputa_emissao.py:589`: lista de motivos no nascimento.
- `app/chamados/services/disputa_emissao.py:1083-1084`: complementação da disputa existente.
- `app/chamados/services/disputa_emissao.py:1125`: nascimento do agrupador.
- `app/chamados/services/disputa_emissao.py:1131-1132`, `:1160-1161`, `:1255-1256`: atualizações do agrupador.
- `app/chamados/services/disputa_emissao.py:1360`, `:1366-1367`: nascimento/atualização da disputa de âncora.

Leitores:

- `app/chamados/models.py:1962`, `:2033`, `:2157`: leitura de lista com `or []`.
- `app/chamados/services/disputa_emissao.py:536-540`: lista com `or []`, set/ordenação.
- `app/chamados/services/disputa_emissao.py:1081`, `:1100`, `:1131`, `:1160`, `:1254`, `:1335`: agregação/comparação do rol; `:1100` e `:1335` pressupõem lista quando disputa existe.
- `app/chamados/relatorios_disputa.py:52`: lista com `or []`, aplica `labels.get(m,m)`.
- `app/chamados/management/commands/auditar_disputas.py:88`: lista com `or []`; `:142`: valor inteiro; `:169-191`: set com `or []`, compara motivos com catálogo e perguntas.
- `app/chamados/services/fio.py:680`: lista com `or []`, transforma em labels.
- `app/chamados/services/acoes_disputa.py:149`: idem.
- `app/api/foto_recebida.py:63`: ORM contém `foto_ausente`; `:70`: iteração sem default.
- `app/colaboradores/signals.py:74`: ORM contém `posto_desativado`.
- `app/escala/signals.py:171`: ORM contém `ancora_ausente`.
- `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49-50`: `or []`; aceita dict convertendo em lista de chaves.
- `app/ponto/management/commands/alertar_disputas_pendentes.py:82`: interpola valor inteiro.
- `app/templates/chamados/detalhe_local.html:301`: join dos itens.
- `app/templates/chamados/partials/modal_fio.html:327`: contém `ancora_ausente`.

Desencontro de shape entre escritores nominais de produção e esses leitores: não localizado; comprovação sobre dados persistidos: [nao verificado]. O agrupador registra motivo antes de saber se pergunta nasce, explicitamente em `app/chamados/services/disputa_emissao.py:828-833`; por isso “motivo sem pergunta” pode existir por guarda e não prova defeito por si.

Selos do rol: `app/chamados/tests/test_porta_disputa_emissao.py:185-208`, `:211-221`; `app/chamados/tests/test_porta_remocao_motivo.py:251-285`; o auditor de dessincronia tem corpo em `app/chamados/management/commands/auditar_disputas.py:181-195`. Cobertura de cada consumidor de apresentação: [nao verificado].

**3. PerguntaDisputa.opcoes — JSONField em `app/chamados/models.py:2444`, default `list`.**

Todos os construtores nominais de produção em chamados encontrados:

| Emissor | Shape gravado |
|---|---|
| `app/chamados/services/disputa_emissao.py:330`, `:441`, `:449` | Lista copiada do catálogo: strings para escolha; dict `{valor,rotulo}` para hora; `[]` quando sem opção. Catálogo em `app/chamados/catalogo/perguntas.py:34-138`. |
| `app/chamados/services/disputa_emissao.py:333-337` | Posto: `{id,nome}`. |
| `app/chamados/services/disputa_emissao.py:339-343` | Escala: `{id,tipo_escala__template_nome}`. |
| `app/chamados/services/disputa_emissao.py:842` | `_criar_pergunta_se_couber` repassa `opcoes`; chamadores `:882`, `:1155`, `:1215`, `:1247` usam escapes `{valor,rotulo}` ou lista vazia. |
| `app/chamados/services/disputa_emissao.py:1376` | `[]`, pergunta de âncora/conversa. |
| `app/chamados/services/declaracao_texto.py:376`, `:666` | `[]`, tipo hora. |
| `app/chamados/services/acoes_disputa.py:267` | Strings `['Sim, confirmo','Nao, foi erro']`, tipo escolha. |

Persistência passa `**campos` por `app/chamados/models.py:2790-2791`, `:2802-2803`, `:2826`; o campo também pode nascer sem `opcoes`, usando default `list` (`models.py:2444`).

Todos os leitores nominais encontrados:

| Consumidor | Shape/chaves lidas |
|---|---|
| `app/chamados/services/disputa_emissao.py:1436-1439` | Escolha: se primeiro item é dict, `.get('valor')` default `None`; senão item inteiro. |
| `app/chamados/services/disputa_emissao.py:1447` | Hora: dicts, `.get('valor')` default `None`. |
| `app/chamados/services/disputa_emissao.py:1458` | Dropdown: dicts, `.get('id')` default `None`. |
| `app/chamados/models.py:3540-3543` | Dict: `.get('valor')` default `None`; `.get('rotulo',self.resposta_colab)`. |
| `app/api/views.py:1157` | Serializa campo integral. |
| `app/api/views.py:2538-2540` | `or []`; hora remove dict cujo `.get('valor')=='OUTRO_MOTIVO'`. |
| `app/api/views_mensageria.py:566` | Serializa com `or []`. |
| `app/templates/chamados/partials/_card_atendimento.html:105-108` | Escolha imprime item inteiro como value e label. |
| `app/templates/chamados/partials/_card_atendimento.html:147-149` | Dropdown lê `id`; label em sequência `nome`, `label`, `descricao`, `tipo_escala__template_nome`, finalmente `id`. |
| `app/templates/chamados/partials/_card_atendimento.html:162-167` | Hora lê `valor`, `rotulo`. |

Desencontros nominais da definição literal “chave lida que algum emissor não grava”: `label`/`descricao` não são gravados pelos emissores dropdown localizados, e um emissor escreve `nome` enquanto outro escreve `tipo_escala__template_nome`; o template explicita alternativas em `app/templates/chamados/partials/_card_atendimento.html:149`. Assim, diferença de chaves confirmada, defeito de costura não demonstrado.

Candidato separado: o responder aceita escolha-dict `{valor,rotulo}` (`disputa_emissao.py:1436-1438`), mas template escolha imprime dict inteiro (`_card_atendimento.html:107-108`). **Nenhum emissor nominal atual de escolha-dict foi localizado**; incidência e classe: [nao verificado].

Selos:

- `app/chamados/tests/test_escape_outro_motivo.py:9-25`: recusa `OUTRO_MOTIVO` e o exclui dos catálogos.
- `app/chamados/tests/test_responder_web_falta.py:31-42`: opções de escape na web levam à ausência materializada.
- `app/chamados/tests/test_e3_app_orm.py:86-100`: endpoint remove `OUTRO_MOTIVO`, preserva outro `valor`.
- Cobertura cruzada completa de todas as variantes de dropdown, serializers e template: [nao verificado].

**4. PerguntaDisputa.chamados_relacionados_ids — JSONField em `app/chamados/models.py:3408-3411`, default `list`.**

Único emissor nominal explícito localizado: `app/chamados/services/disputa_emissao.py:1198` constrói os IDs dos chamados do grupo depois do canônico; passa em `:1216`; helper adiciona ao kwargs em `:835-836` e entrega à porta em `:837-845`. Demais nascimentos omitem a chave e usam default `[]` (`app/chamados/models.py:3408-3409`).

| Estrutura | Emissor | Gravadas | Consumidor | Lidas | Desencontro | Selo | Dono/classe |
|---|---|---|---|---|---|---|---|
| relacionados | `disputa_emissao.py:1198`, `:1216`, `:836` | Lista de PKs dos clones | `materializacao.py:781-787` | `list(p.chamados_relacionados_ids or [])`; fecha clones | Não localizado | Sim, `app/chamados/tests/test_regressao_etapa7.py:483-497` | ESTRUTURA provável; classe não |
| relacionados | mesmas | mesma | `validacao.py:431-434` | Lista com `or []`, acrescenta aos chamados travados | Não localizado | Específico do travamento dos relacionados: [nao verificado] | ESTRUTURA provável; classe não |
| relacionados | mesmas | mesma | `materializacao.py:975` | Lista com `or []`, copia ao LogAuditoria.valor_depois | Não localizado | Específico da cópia na trilha: [nao verificado] | ESTRUTURA provável; classe não |

São **3 sítios de consumo**, em **2 módulos consumidores** (`materializacao.py:781`, `:975`; `validacao.py:433`); contar duas ou três costuras conforme a identidade de consumidor usada no consolidado. Escritor e dois módulos leitores efetivamente localizados; valor dos dados reais: [nao verificado].

**5. Outra costura de dict encontrada seguindo motivos**

`app/chamados/relatorios_disputa.py:47-53` grava `buckets[classificar(d)][]` com `{disputa_id,colaborador_id,nome,dias,motivos}`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:39` lê `it['motivos']`. Falta conferir demais chaves do corpo desse comando no consolidado.

## 13/11 15:41 · sessao 3a3451fa21d5 · - · - · origem=Ronald (tmux)

Método: busquei JSONFields e atribuições explícitas; depois referências de atributos, acessos `.get`/`[]`, serializações e testes. Não executei testes nem consultei produção. Execução dos testes e completude diante de acessos dinâmicos: `[nao verificado]`. [nao verificado]

Critério aplicado: emissor que monta a estrutura em escala conta; model de escala cuja estrutura nasce exclusivamente em ponto fica na fronteira. [nao verificado]

**Inventário para consolidação — referências abaixo relativas a `app/`.**

| Estrutura | Emissor e chaves | Consumidores e chaves/defaults | Desencontro | Selo |
|---|---|---|---|---|
| `TipoEscala.horarios_por_dia` | `escala/services/cadastro_tipo.py:265-267`: copia todo dict recebido; objeto inválido vira `{}` | `escala/models.py:224-231`: weekday string ou inteiro; default `None`; `:233-239`: `hii/hfi` default `''`; `:326-342`: `hi/hf` default escalares, `hii/hfi` default `None`; `:462-471`: `hi/hf` default `''`; `:667-695`: `hi/hf/hii/hfi` default `None`; `:862-866`: presença da chave weekday string | Nenhum desencontro demonstrado para chaves canônicas; validação exige hi/hf e par hii/hfi (`models.py:692-695`). Chaves adicionais arbitrárias preservadas pelo escritor, sem inventário finito `[nao verificado]` | Leitores: `escala/tests/test_horarios_por_dia.py:37-61,75-93`; ponte escritor: `test_cadastro_tipo_service.py:47-56` só exige `hi` persistido |
| mesmo HPD | mesmo escritor | `escala/utils.py:274-283`: `hi/hf/hii/hfi` default `None`; `:735-741`: presença de HPD e delegação `marcos_do_dia` | Não demonstrado | `test_marcos_def_hpd.py:27-45`; `test_horarios_por_dia.py:107` inicia classe da grade |
| mesmo HPD | mesmo escritor | `escala/views_wizard.py:80`: serialização integral, default `{}` | Não demonstrado | Específico deste retorno `[nao verificado]` |
| mesmo HPD | mesmo escritor | `colaboradores/services/drawer.py:99-100`: apenas verdade/falsidade do JSON | Não demonstrado | `[nao verificado]` |
| mesmo HPD | mesmo escritor | `ponto/selecao_periodo.py:223-227`: weekday string, default `None`; `hi/hf` obrigatórios; erro vira `False` (`:228-229`) | Leitor aceita somente weekday string, enquanto `_override_do_dia` também aceita inteiro (`models.py:231`); em JSON persistido transformação das chaves `[nao verificado]`, portanto não classifiquei como desencontro fechado | `[nao verificado]` |
| `FolgaCalendario.celulas` | **dois escritores:** `escala/folgas.py:584-591`: lista de pares deduplicados/ordenados, ou `[]`; `:654-656`: `celulas or []` | **outro módulo:** `escala/views.py:371`: lista integral, default `[]` | Não demonstrado | `test_editor_folga_modo.py:27-46` cobra escrita; `:58-64` cobra leitura GET. Não é teste único da ida e volta |
| mesmo `celulas` | mesmos escritores | **mesmo módulo, não contar como costura externa:** `escala/folgas.py:55`, `:18-39`: pares `[semana,weekday]`, iteração default `[]`, índices `[0]/[1]` convertidos para inteiro (`:29`) | Não demonstrado | `test_folga_semana_dia.py:49-59` materializa foto a partir dos pares |
| `FolgaCalendarioTamanho.dias` | **dois escritores:** `escala/folgas.py:593-595`: dias ordenados/deduplicados por tamanho; `:658-661`: lista filtrada `d <= tamanho` | `escala/models.py:1560`: lista integral; `:1579`: lista integral para `__str__`; `escala/views.py:362`: lista integral transformada em set | Não demonstrado | GET/ida e volta deste campo `[nao verificado]`; escrita `test_editor_folga_modo.py:38-49`; model/getter `test_folga_models.py:13-23` |
| dict de `dias_por_tamanho()` | `escala/models.py:1558-1560`: `{tamanho_mes:list(dias)}` | `escala/folgas.py:56` transporta para `folgas_do_mes`; `:12-15` lê chave `28/30/31`, 29 usa 28, default `[]` | Não demonstrado | **sim**, `test_folga_models.py:19-23` cruza produtor com resolvedor |
| `DecisaoProposta.resumo` | **dois escritores:** `escala/services/propositor.py:717-727` e `:773-776`; chaves `tipo,template,template_id,ancora,ancora_atual,template_atual,casa_hoje,casaria,sobra,aplicar_desde,dias_batidos`, todos via `p.get(k)` | Nenhum leitor de `.resumo` localizado em produção ou template `[nao verificado]`; finalidade forense declarada em `escala/models.py:1927-1930` | Todas as chaves sem leitor localizado; **sem par, não contar costura** | `[nao verificado]` |
| dict `nome_canonico_partes()` | `escala/models.py:486-487`: `tipo,apelido,dias,horario,ciclico`; alterações de dias/horário `:488-522` | `core/templatetags/hasner_filters.py:72-123`: tipo/apelido/dias/horario por `[]`; `:207-217`: mesmas quatro, `.get` + default `''`; `colaboradores/services/ficha.py:82-90`: dias/horario com default `''`, e `codigo_da_escala(:70-71)` lê tipo/apelido com default `''` | **`ciclico` escrito, nenhum consumidor de produção localizado** `[nao verificado]`. Os demais leitores localizados só usam quatro chaves (referências anteriores). Classe **sim**, dono provável **ESTRUTURA** `[nao verificado]` | `test_nome_canonico.py:77-95` cobra inclusive ciclico no produtor; **não prende uso no consumidor**. `colaboradores/tests/test_ficha_cabecalho_compacto.py:28-40` cobra linha da ficha, mas não ciclico |
| contador `vinculos_5x2_6x1_trabalha_feriado()` | `escala/models.py:933`: `total,declarados,nao_declarados` | `escala/management/commands/vinculos_5x2_6x1_trabalha_feriado.py:15-17`: três chaves por `[]` | Não demonstrado | **sim**, `test_feriado_padrao_por_escala.py:100-120`: chama command e confere contagem |
| **ata efêmera**, cópia de CelulaDia | `escala/utils.py:1886-1898`: somente `lampadas,orfas`; omissão de minutos é explícita `:1888-1891`; passa à autoridade de turnos | `ponto/turnos.py:1631-1633` passa celula a `periodos_do_dia`; leitor efetivo `ponto/juiz_batida.py:157-161`: `lampadas/orfas` com default `[]`; lâmpadas leem `hora,tipo,acesa,luz,tipo_real` em `:190-194,215-217,230-246` | Não demonstrado: minuto ausente não é lido nesse consumidor (`juiz_batida.py:157-260`). Campos de lâmpadas vêm do construtor chamado, `ponto/services/cartorio.py:599-612,672-678` | **sim**, convergência de montagem → ata → turno em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:176-205`; atribuição de ata no teste `:197-198` |

**Fronteiras sem emissor de estrutura em escala, não adicionar à contagem deste recorte:**

- DNA e dna_anterior: atribuições reais em `ponto/portas/celula.py:261-307,629-642,827-828,917-926`; montar DNA é `ponto/management/commands/gerar_celulas.py:104`.
- Ata/protestos persistidos: `escala/models.py:1819-1828` apenas delega à porta; atribuições de ata na porta `ponto/portas/celula.py:1003,1064,1091`.
- Outbox `EventoCelula`: escritor único externo declarado em `escala/models.py:1851`, consumidor externo declarado em `:1848`; estrutura escalar descrita em `:1857-1863`.

**Orientação de contagem:** acima, HPD tem **11 funções leitoras**; `nome_canonico_partes` tem **4 funções externas leitoras** se incluir `codigo_da_escala` além de `linha_do_vinculo`; calendário tem multiplicação pelos dois escritores, e os leitores dentro do próprio `folgas.py` não satisfazem “outro módulo”. Os números dependem de consolidar funções ou módulos como identidade do par `[nao verificado]`. Não declarei total global porque os outros agentes cobrem demais estruturas.

## 13/11 15:35 · sessao b9a9a64c7207 · HJ-090 · construtor · origem=Code

Implementação entregue, com REDs e 17 mutantes em memória. Oito fontes e os 17 ASTs compilaram, sem gravar bytecode. Suíte, RED executado na base e morte dos mutantes: **[nao verificado]**. ([registro estático](ferias/HJ090_VOLTA3_ENTREGA.txt:158))

- **Achado 1:** `realinhar_concedidos_ferias.py::Command.handle` mantém o atomic por período até completar `write/flush/fsync`; `_ler_registro` tolera cauda sem terminador. O desenho está no docstring. Os testes preparam falhas de disco, falha no segundo período, cauda truncada e divergência entre snapshot e linha travada. ([implementação](ferias/management/commands/realinhar_concedidos_ferias.py:8), [testes](ferias/test_hj090_volta3.py:98))
- **Achado 2:** `services.py::editar_periodo_aquisitivo` relê pela trava comum, exige direito ≥ concedidos + vendidos e recalcula status; `views.py::editar_periodo` relata `RecusaNaPorta`. Os REDs cobrem consumo 20+10 com pedido de direito 20 e foto velha, comparando o registro inteiro antes/depois. ([porta](ferias/services.py:725), [view](ferias/views.py:844), [REDs](ferias/test_hj090_volta3.py:166))
- **Achado 3:** `vigiar_fio_morto.py::Command.handle` publica `ferias_soma_maior` e `ferias_concedidos_maior`, ambos com esperado zero, lendo a mesma função do tripwire. O teste usa contagens distintas, 1 e 2; o teste vizinho de chaves completas foi ajustado. ([produtor](chamados/management/commands/vigiar_fio_morto.py:229), [RED](ferias/test_hj090_volta3.py:206), [vizinho](chamados/tests/test_o261_vigia_fio.py:789))
- **Achado 5:** `RedPorValor::_recusou` aceita somente `RecusaNaPorta` com a mensagem esperada. Os testes exigem propagação de `TypeError` e rejeitam recusa por outro motivo. ([helper](ferias/test_hj090_red_por_valor.py:45), [testes](ferias/test_hj090_volta3.py:234))

O **censo completo de escritores e leitores**, campo por campo, está na [entrega](ferias/HJ090_VOLTA3_ENTREGA.txt:47). Inclui:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| Direito, datas, status e timestamp | [porta de edição](ferias/services.py:736) | [juiz de status](ferias/models.py:98), [saldo](ferias/models.py:220), telas |
| Registro `periodo/antes/depois/vivos` | [porta produz](ferias/services.py:155); [comando persiste](ferias/management/commands/realinhar_concedidos_ferias.py:126) | [leitor do arquivo](ferias/management/commands/realinhar_concedidos_ferias.py:39), [porta de reversão](ferias/services.py:174) |
| Contadores e esperados de férias | [vigia](chamados/management/commands/vigiar_fio_morto.py:99), [censo](chamados/management/commands/vigiar_fio_morto.py:245) | [comparação genérica do ESTADO](../bin/fio_estado.py:125), [exibição de `fora_do_esperado`](../bin/fio_estado.py:191) |
| Recusa e mensagem | [porta](ferias/services.py:734) | [view](ferias/views.py:848), [helper RED](ferias/test_hj090_red_por_valor.py:49) |

Os **labels e comandos individuais dos REDs** estão [aqui](ferias/HJ090_VOLTA3_ENTREGA.txt:101); os **17 comandos individuais dos mutantes**, [aqui](ferias/HJ090_VOLTA3_ENTREGA.txt:116). O executor considera morte somente por assertion, sem ERROR/SKIP. ([executor](ferias/hj090_volta3_mutantes.py:135))

Não curei a ferramenta fora de `app/`, excluída expressamente desta volta, nem os resíduos de prefill, exclusão, relógios, tooltip e HJ-091 remetidos à fila. As pendências estão [registradas](ferias/HJ090_VOLTA3_ENTREGA.txt:161). O saldo continua sem corte no [juiz existente](ferias/models.py:220), com leitura direta na [tela](templates/ferias/_secao_colab.html:50).

## 13/11 15:31 · sessao b1ab3636f9b5 · - · - · origem=Ronald (tmux)

**Complemento: 19 pares novos**, separados dos 93 anteriores. Nenhuma quebra de chave obrigatória foi confirmada neste complemento; há formatos alternativos e omissões condicionais atendidas por defaults ou discriminadores. Selos: **9 localizados; 10 não localizados**, inexistência desses dez **[não verificado]**. Atribuição por estrutura: **17 escala; 2 ponto** — os dois últimos são as trilhas `veredito_celula`. Dono provável de eventual incompatibilidade estrutural: ESTRUTURA **[não verificado]**.

As linhas pendentes dos gravadores anteriores ficam verificadas:

- M02: `app/escala/management/commands/lavrar_previsto_cego.py:37`, `detalhe=d`.
- M03: `app/escala/management/commands/lavrar_vigencia_impossivel.py:39`, `detalhe=d`.
- M04: `app/escala/management/commands/lavrar_vigencia_sem_trilha.py:33`, `detalhe=d`.

**Schemas compartilhados**

`F12`: o command produz o dicionário em `app/ponto/management/commands/censo_fase_12x36.py:290`; a porta persiste integralmente em `app/escala/services/fase_12x36.py:91`. Chaves: `linhas_de_desvio,linhas_medidas,colabs_de_desvio,automaticos,propostas,linhas,itens,janelas,agora,itens_reabertos,criterio,esperado`. `linhas` contém `colab,empresa,competencia,ec,template,plantoes,casados,paridade,furos,folga_trabalhada` em `:295`; os itens recebem `data,ec_do_dia,template_do_dia,tipo,aplicavel` em `:235` e `colab,empresa,competencia,dono_do_dia` em `:299`; `janelas={empresa,ini,fim,reaberta}` em `:306`. O leitor `ultimo_lavrado` copia integralmente e acrescenta `data_ref,medido_em` em `fase_12x36.py:211`; sem formato atual, retorna apenas `colabs_de_desvio,linhas,itens,sem_medida` em `:210`.

`C7`: `app/escala/services/regua_na_lista.py:48` persiste `total,colabs,universo,esperado,por_colab`. Cada item é produzido em `:31` com `ec,colab,origem,classe,tipo_escala_id,tipo_escala_codigo,declarado,real,evidencia,data_exemplo,destino`. `ultimo_lavrado` acrescenta `medido_em` em `:65`; fallback com `total=None,por_colab={},esperado=0,sem_medida=True` em `:63`.

| ID | Estrutura / emissor | Consumidor / chaves lidas | Desencontro / selo |
|---|---|---|---|
| X01 | F12 — `censo_fase_12x36.py:290` | `app/escala/services/fase_12x36.py:93`, `:106`, `:110`, `:144`, `:179`, `:207`: `colabs_de_desvio`; `janelas,agora`; janela `empresa,ini,fim`; `itens,itens_reabertos`, item `data,colab,empresa`; cópia integral | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:94`; leitura de cobertura/as-of também em `app/escala/tests/test_vfase_dono_as_of.py:83` |
| X02 | F12 via `fase_12x36.py:211` | `app/escala/services/cadastro_realidade.py:421`: `itens` default `()`, item `colab,data,ec_do_dia,template_do_dia,dono_do_dia,aplicavel,competencia,tipo` | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:180` |
| X03 | mesmo | `app/api/views_mensageria.py:1685`: `colabs_de_desvio,linhas_medidas,criterio,data_ref,sem_medida,linhas`, todos `.get` | Variantes normal/sem medida: `sem_medida` só no fallback; demais chaves ausentes nele usam defaults. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:263` |
| X04 | mesmo | `app/chamados/services/fila_por_causa.py:184`: `colabs_de_desvio` default `()` | Nenhuma quebra confirmada. Selo não localizado **[não verificado]** |
| X05 | mesmo | `app/escala/services/propositor.py:493`: `itens` default `()`, item `colab,ec_do_dia` via `.get` | Nenhuma quebra confirmada. Selo específico desta dependência não localizado **[não verificado]**; `test_o261_v_fase_na_lista.py:280` testa o universo do propositor, mas não demonstrei que morde retirar a leitura da lavra |
| X06 | mesmo | `app/ponto/management/commands/vigiar_familia_escala.py:24`: `data_ref,linhas_medidas,colabs_de_desvio,criterio,linhas`; linha `colab,competencia,ec,paridade,plantoes,furos,folga_trabalhada` | Variantes sem medida atendidas por `.get`. Selo não localizado **[não verificado]** |
| X07 | C7 — `app/escala/services/regua_na_lista.py:48` | `app/escala/services/cadastro_realidade.py:347`: `por_colab` default `{}`; item `destino,ec,tipo_escala_codigo,classe,declarado,real`; destino `codigo` | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_termometro_vira_lista.py:51`, `:61` |
| X08 | mesmo | `app/escala/views.py:469`: `por_colab` default `{}`, índice `str(colaborador_id)` default `[]`; item `ec,destino`; destino repassado à porta | Nenhuma quebra confirmada. Selo da realocação por esse payload não localizado **[não verificado]** |
| X09 | retorno C7 — `app/escala/services/regua_na_lista.py:54` | `app/escala/management/commands/termometro_regua.py:94`: `total,colabs,universo` | Nenhuma quebra confirmada. **Sim**, fluxo real executado: `app/escala/tests/test_termometro_vira_lista.py:136` |
| X10 | sugestão — `app/escala/services/fase_12x36.py:50`, `:61`, `:64`: `ancora,motivo,batidas`; `casa_hoje` em duas variantes, `casaria` só na sugestão | `app/escala/services/propositor.py:333`: `ancora` via `.get`, depois indexação sob a guarda | Consumidor lê a única chave presente em todas as variantes. Selo do produtor existe em `app/escala/tests/test_ancora_sugerida.py:43`; selo que prende o par inteiro não localizado **[não verificado]** |
| X11 | impacto — `app/escala/services/impacto_troca.py:47`: `de,ate,furos_antes,furos_depois,dias_furo_antes,dias_furo_depois,frase`, ou `None` em `:32` | `app/colaboradores/services/vinculo.py:142`, transporta integralmente em `impacto` no retorno em `:147` | Nenhuma quebra confirmada. **Sim**: `app/colaboradores/tests/test_troca_escala_porta.py:50`, `:58` |
| X12 | HPD JS — `app/templates/escala/wizard_tipo_escala.html:454`: weekday → `hi,hf`; `hii,hfi` condicionais em `:455`, serialização em `:459`, parâmetro em `:481` | `app/escala/views_wizard.py:53`: lê `horarios_por_dia`, `json.loads`, aceita mapa inteiro em `:57`, inválido → `{}` em `:59` | Omissão de pausa prevista. `app/escala/tests/test_wizard_intervalo.py:84` injeta JSON manual: **não prende emissão do JS**. Selo completo não localizado **[não verificado]** |
| X13 | trilha da lavra — `app/ponto/portas/celula.py:1087`, `:1102`: `veredito,via,protestos,status_dia` | `app/chamados/juizes.py:1295`: `valor_depois`; `.get('via') == VIA_HUMANA` ou presença de `acoes` em `:1298`; `VIA_HUMANA='admin'` em `app/ponto/services/veredito_celula.py:76` | Variante da assinatura atendida pelo OR. **Sim**, escritor real→leitor: `app/chamados/tests/test_tranca_vivas_08.py:56`, `:82`, `:95`; mutante em `:300` |
| X14 | trilha do despachante — `app/ponto/services/veredito_celula.py:356`: `veredito,acoes,erros` | mesmo consumidor `app/chamados/juizes.py:1298`: lê `via` com default `None`, ou presença de `acoes` | `via` ausente é variante deliberada: `acoes` discrimina. O ramo tem teste em `app/chamados/tests/test_tranca_vivas_08.py:101`, porém o teste grava payload manual por `registrar_log`, sem chamar o despachante inteiro. Selo completo escritor→leitor **[não verificado]** |
| X15 | envelope EventoCelula — `app/ponto/portas/celula.py:954`: `celula_id,colaborador_id,data`; defaults do modelo em `app/escala/models.py:1860`: `criado_em,processado_em=None,tentativas=0,erro=''` | `app/chamados/reconciliador.py:1250`, `:1262`, `:1344`, `:1351`, `:1394`: `processado_em,tentativas,criado_em,pk,colaborador_id`; `celula_id` também em `:1274`, `:1296`, `:1309`; lote repassado em `:1388` | Envelope sem blob é declarado em `app/escala/models.py:1848`. Nenhuma quebra confirmada. **Sim**: `app/ponto/tests/test_o_outbox_celula.py:56`, `:163` |
| X16 | mesmo envelope | `app/chamados/services/fabrica_da_celula.py:297`: `data`; relê célula atual por `(colab.pk,data)` em `:299` | `celula_id` antigo não decide a célula atual, explicitado em `:291`. Teste do leitor: `app/chamados/tests/test_e4_c20.py:42`; usa eventos fake. Selo completo produtor→leitor **[não verificado]** |
| X17 | mesmo envelope | `app/ponto/services/lastro.py:422`: `colaborador_id` dos eventos pendentes; filtro de estado definido em `reconciliador.py:1250` | Nenhuma quebra confirmada. Selo específico não localizado **[não verificado]** |
| X18 | mesmo envelope | `app/chamados/management/commands/reconciliar_chamados.py:62`: `colaborador_id` dos pendentes | Nenhuma quebra confirmada. **Sim**, evento real e mutação do estado: `app/chamados/tests/test_c20_juiz3_outbox.py:349` |
| X19 | mesmo envelope | `app/chamados/services/foto_e4b.py:270`: filtro `colaborador_id`, `data` para alcance e registro inteiro pelo `guardar` | Nenhuma quebra confirmada. Selo específico não localizado **[não verificado]** |

O envelope também tem **escritor de estado**: `app/chamados/reconciliador.py:1396` grava `tentativas,erro`; `:1402` grava `processado_em`; `:1409` aplica retenção. Esses campos participam dos filtros dos leitores X15/X17/X18; portanto não são chaves lidas sem escritor. Os demais gatilhos chamam o escritor único `emitir_evento_celula`, cujos chamadores estão declarados em `app/ponto/portas/celula.py:946` e `app/escala/models.py:1851`; não multipliquei a costura por cada gatilho que apenas chama essa porta.

**Correção de classificação para o subtotal anterior:** O01 é desencontro formal com default explícito de sucesso, não quebra obrigatória demonstrada; R01/R02 são excesso sem leitor localizado, cuja ausência global continua **[não verificado]**. Assim, para os **93 anteriores + estes 19 = 112 pares localizados**, não certifique “três quebras”: são **zero quebras obrigatórias confirmadas por mim**, um casamento de schemas com defaults e dois excessos sem consumo localizado. A soma permanece um recorte, sem certidão de completude.

## 13/11 15:27 · sessao b1ab3636f9b5 · - · - · origem=Ronald (tmux)

Consolidei **92 pares por arquivo emissor–arquivo leitor**, agrupando operações do mesmo arquivo sobre a mesma estrutura. É **subtotal verificado, não censo total**: ainda faltam fechar retornos auxiliares, aliases, templates e chamadas dinâmicas. As contagens de desencontro e de inexistência de selo para o universo inteiro permanecem **[não verificado]**.

**Convenções para consolidar**

- `D`: schema produzido em `app/ponto/management/commands/gerar_celulas.py:83` e `:104`; persistido pelas operações de `app/ponto/portas/celula.py:261`, `:278`, `:294`, `:306`, `:639`, `:642`, `:827`, `:917`, `:925`. Chaves: `dna_versao,vinculo_id,tipo_escala_id,tipo_ciclo,marcos,minutos_jornada,intervalo_livre,lampadas`; `marcos={hi,hf,hii,hfi,[hii2,hfi2]}`; `lampadas=[{chave,tipo,hora,instante}]`; `extra_declarada={antes,depois}` nasce condicionalmente em `gerar_celulas.py:130`.
- `A`: ATA produzida em `app/ponto/services/cartorio.py:738`, com `tipo_dia,lampadas,minutos_previstos,minutos_realizados,sem_ancora,n_orfas,orfas,n_missing,n_celulas`; condicionais `cobertura_parcial,periodo_parcial` em `:766`; lâmpada `{tipo,hora,acesa,luz,tipo_real}` em `:711`; `isencao` pelo `_carimbar_isencao` chamado em `:714`; `dispensada` em `:810`; `pendente` em `:915`. Persistência em `app/ponto/portas/celula.py:1064` e `:1091`; `precedencia` em `:1213`, preservada pelas lavras em `:1053`.
- **Omissão condicional prevista** precisa ser distinguida de incompatibilidade: `extra_declarada`, segunda pausa, `isencao`, `dispensada` e `pendente` não existem em todas as variantes, mas têm leitores `.get`; isso satisfaz a definição literal de ausência de chave, sem provar falha.
- Nas linhas abaixo, selo não indicado significa **não localizado nesta inspeção; inexistência [não verificado]**. Dono provável de incompatibilidade de forma: **ESTRUTURA [não verificado]**; não atribuí dado real a CADASTRO/BATIDA sem evidência.

### Persistidos: DNA — 21 pares

| ID | Estrutura/emissor | Consumidor | Chaves lidas |
|---|---|---|---|
| D01 | D | `app/escala/models.py:1124`, `:1189`, `:1235` | `marcos.hi,hf,hii,hfi`, todos os pares de pausa; `extra_declarada.antes,depois` com zero |
| D02 | D | `app/escala/utils.py:53`, `:104`, `:137`, `:153` | `marcos` e pares; `lampadas` default `[]`, `hora,instante` |
| D03 | D | `app/chamados/juizes.py:948`, `:1348` | `marcos`, chave do motivo, pares de pausa |
| D04 | D | `app/chamados/services/adesao.py:36` | `marcos`, repassado a `CelulaDia.marcos_presentes` |
| D05 | D | `app/ponto/juiz_batida.py:159` | `marcos.hi,hf` e todos os pares de pausa |
| D06 | D | `app/ponto/services/cartorio.py:484`, `:615` | `dna_versao,marcos`, chaves de turno |
| D07 | D | `app/ponto/services/dia_do_colab.py:46` | `marcos`, repassado a `marcos_presentes` |
| D08 | D | `app/ponto/services/diagnostico_escala.py:153` | `marcos`, default `{}` |
| D09 | D | `app/ponto/services/esmeril_espelho.py:116` | `marcos`, repassado a `marcos_presentes` |
| D10 | D | `app/ponto/services/faltas_de_hoje.py:43` | `marcos.hi`, fallback de template em `:44` |
| D11 | D | `app/ponto/services/veredito_celula.py:31` | `marcos.hi,hf,hii,hfi` e todos os pares |
| D12 | D | `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79` | `vinculo_id`; comparação do DNA inteiro |
| D13 | D | `app/ponto/services/vigia_entrada.py:126` | `marcos.hi,hf`, `tipo_ciclo` |
| D14 | D | `app/ponto/management/commands/tripwire_celulas.py:30`, `:99`, `:259` | inteiro em comparação; `marcos` |
| D15 | D | `app/ponto/management/commands/aplicar_col900.py:76`, `:174` | inteiro em snapshot; `marcos.hi` |
| D16 | D | `app/ponto/management/commands/censo_col900.py:119` | `marcos`, comparação integral do subdict; `hi` |
| D17 | D | `app/ponto/management/commands/censo_fase_12x36.py:392` | inteiro em snapshot |
| D18 | D | `app/ponto/management/commands/detectar_intervalo_ausente.py:53` | `marcos.hi,hf` e todos os pares |
| D19 | D | `app/ponto/management/commands/backfill_b5_insumos.py:73` | inteiro para `insumos_da_ata` |
| D20 | D | `app/ponto/management/commands/vigia_b5_ausencia.py:79` | inteiro para `insumos_da_ata` |
| D21 | D | `app/pautas/services.py:424` | `marcos` |

O produto tem teste de chaves em `app/escala/tests/test_t2_lampadas.py:28` e de seis marcos em `:34`; isso **não demonstra selo de todos esses leitores**. A comparação do blob inteiro em D12/D14 impede afirmar que suas chaves são globalmente “ninguém lê” apenas por ausência de `.get` nominal.

### Persistidos: ATA — 30 pares

| ID | Estrutura/emissor | Consumidor | Chaves lidas |
|---|---|---|---|
| A01 | A | `app/escala/services/leitor_celula.py:68`, `:91`, `:115`, `:187`, `:379` | schema base; lâmpadas `acesa` default `luz is not None`, `luz,hora,tipo,tipo_real,isencao,dispensada`; `cobertura_parcial,periodo_parcial` |
| A02 | A | `app/chamados/juizes.py:450`, `:493`, `:966`, `:1354` | `tipo_dia,cobertura_parcial,lampadas,orfas,n_orfas`; lâmpadas `tipo,hora,acesa,luz` |
| A03 | A | `app/chamados/reconciliador.py:701`, `:879` | presença de `lampadas`; inteiro para `parcial_so_lista` |
| A04 | A | `app/chamados/services/disputa_emissao.py:940` | inteiro para `parcial_so_lista` |
| A05 | A | `app/chamados/services/fabrica_da_celula.py:123` | inteiro para `parcial_so_lista` |
| A06 | A | `app/chamados/services/materializacao.py:172` | `lampadas,orfas`; lâmpadas `tipo,hora,luz,acesa` |
| A07 | A | `app/ponto/juiz_batida.py:157` | `lampadas,orfas`; uso das lâmpadas começa em `:190` |
| A08 | A | `app/ponto/services/cartorio.py:451`, `:805`, `:834`, `:913` | `lampadas`, `pendente`; dispensa, insumos e teto |
| A09 | A | `app/ponto/services/cluster_espurio.py:27` | `orfas,lampadas`; `acesa` |
| A10 | A | `app/ponto/services/dia_do_colab.py:30` | `lampadas`; `hora,tipo,acesa,luz` |
| A11 | A | `app/ponto/services/diagnostico_escala.py:154` | `lampadas`; `hora,tipo,acesa,luz` |
| A12 | A | `app/ponto/services/esmeril_espelho.py:125` | `orfas,lampadas`; `luz,hora,acesa` |
| A13 | A | `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:148` | `lampadas`; `hora,tipo,acesa,pendente` |
| A14 | A | `app/ponto/services/vigia_entrada.py:80`, `:123` | `lampadas`; `tipo,acesa` |
| A15 | A | `app/ponto/services/vigia_pareador_celula.py:73` | `orfas,n_orfas` default zero, `lampadas` |
| A16 | A | `app/ponto/worklist.py:22` | presença de `lampadas` |
| A17 | A | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61` | `lampadas.acesa` |
| A18 | A | `app/ponto/management/commands/backfill_b5_insumos.py:73` | inteiro para `insumos_da_ata` |
| A19 | A | `app/ponto/management/commands/celula_veredito_velho.py:108` | inteiro em snapshot |
| A20 | A | `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:148` | presença de `lampadas` |
| A21 | A | `app/ponto/management/commands/detectar_intervalo_ausente.py:64` | `lampadas.tipo,hora,acesa,luz`; `acesa` default `luz is not None` em `:68` |
| A22 | A | `app/ponto/management/commands/tripwire_celulas.py:264`, `:342` | `lampadas,orfas,tipo_dia` |
| A23 | A | `app/ponto/management/commands/vigia_b5_ausencia.py:79` | inteiro para `insumos_da_ata` |
| A24 | A | `app/pautas/services.py:432` | `lampadas.hora,acesa` |
| A25 | A | `app/relatorios/dia_da_frota.py:21`, `:61` | `lampadas.tipo,hora,acesa` |
| A26 | A | `app/relatorios/cartao_pela_celula.py:287` | `orfas,lampadas.hora,isencao` |
| A27 | A | `app/folha/previa.py:83` | `lampadas.acesa,hora,tipo` |
| A28 | A | `app/folha/services/sem_furo.py:22` | `lampadas.acesa` |
| A29 | A | `app/folha/validacao_pdf.py:47` | `minutos_realizados`, default zero |
| A30 | A | `app/colaboradores/services/feriado_do_posto.py:97` | `lampadas`, processamento posterior no mesmo corpo |

**Outro emissor, outro alcance:** `app/escala/utils.py:1887` fabrica **ATA em memória** apenas com `lampadas,orfas`, passando-a em `celulas=` a `turnos_do_colab` em `:1898`. Seu leitor efetivo é `app/ponto/juiz_batida.py:157`, que lê essas duas chaves. Esse é **E01**, um par adicional; não deve ser cruzado artificialmente com todos os leitores da ATA persistida.

### Outros JSONFields — 16 pares

| ID | Estrutura / emissor e chaves | Consumidor e leitura |
|---|---|---|
| P01 | `protestos`: `app/ponto/portas/celula.py:1069`, `:1092`, lista de códigos | `app/escala/services/cadastro_realidade.py:478`, `:612`, membership de códigos |
| P02 | mesmo | `app/chamados/reconciliador.py:869`, lista |
| P03 | mesmo | `app/folha/export.py:1173`, `:1254`, conjunto de códigos |
| P04 | mesmo | `app/ponto/services/furos_sem_cobranca.py:57`, membership |
| P05 | mesmo | `app/ponto/services/cartorio.py:934`, `:1084`, lista e presença |
| P06 | mesmo | `app/ponto/management/commands/celula_veredito_velho.py:108`, inteiro |
| P07 | mesmo | `app/ponto/management/commands/supra_juiz.py:166`, inteiro |
| H01 | `horarios_por_dia`: `app/escala/services/cadastro_tipo.py:267`, mapa weekday → dict externo | `app/escala/models.py:231`, `:239`, `:338`, `:686`, `:864`: weekday string/int; `hi,hf,hii,hfi` |
| H02 | mesmo | `app/escala/utils.py:274`: `hi,hf,hii,hfi`; **selo sim**, `app/escala/tests/test_marcos_def_hpd.py:35` |
| H03 | mesmo | `app/escala/views_wizard.py:80`, serializa inteiro |
| H04 | mesmo | `app/ponto/selecao_periodo.py:223`: weekday string, `hi,hf` |
| H05 | mesmo | `app/colaboradores/services/drawer.py:99`, presença do mapa |
| F01 | `FolgaCalendario.celulas`: `app/escala/folgas.py:586`, `:591`, `:656`, lista `[semana,weekday]` | `app/escala/views.py:304`, `:371`, lista e pares |
| F02 | `FolgaCalendarioTamanho.dias`: `app/escala/folgas.py:595`, `:661`, lista de dias | `app/escala/views.py:293`, `:362`, dias |
| F03 | mesmo | `app/escala/models.py:1560`, dias → mapa tamanho do mês |
| S01 | `dna_anterior`: `app/ponto/portas/celula.py:262`, `:307`, cópia do DNA anterior; limpa em `:828` | `app/ponto/management/commands/aplicar_col900.py:175`, `marcos.hi` |

O `horarios_por_dia` tem emissor de conteúdo no JS: `app/templates/escala/wizard_tipo_escala.html:454` grava `hi,hf`, acrescenta `hii,hfi` condicionalmente em `:455`, e serializa em `:459`; a fronteira Python o decodifica em `app/escala/views_wizard.py:53`. Não contei essa costura JS→Python no subtotal de 92.

`DecisaoProposta.resumo` é escrito em `app/escala/services/propositor.py:720` e `:776`; `_resumo` grava `tipo,template,template_id,ancora,ancora_atual,template_atual,casa_hoje,casaria,sobra,aplicar_desde,dias_batidos` em `:724`. A documentação do próprio campo diz “foto … para a forense” em `app/escala/models.py:1927`. **Leitor de produção não localizado; inexistência global [não verificado]**. Como não há par localizado, não entrou nas 92 costuras.

### Portas e retornos — 25 pares

| ID | Estrutura / emissor | Consumidor / chaves |
|---|---|---|
| W01 | payload wizard, `app/escala/views_wizard.py:20`: `template_nome,apelido,hora_inicio,hora_fim,hora_inicio_intervalo,hora_fim_intervalo,hora_inicio_intervalo2,hora_fim_intervalo2,intervalo_duracao_min,noturno,permite_hora_extra,he_extra_antes_min,he_extra_depois_min,folga_calendario_padrao_id,folgas,dias_trabalho_semana`; criação acrescenta `tipo_ciclo,tipo_base,tipo_base_ref`; `horarios_por_dia` condicional em `:57` | `app/escala/services/cadastro_tipo.py:148`: mesmas chaves até `:267`, inclusive defaults; `descricao` aparece apenas em teste de presença em `:185` |
| W02 | patch `app/chamados/services/limbo_folgas.py:92`: `template_nome,folgas` | mesma porta `cadastro_tipo.py:148`; `hora_inicio,hora_fim` lidos com `.get` em `:205`; demais alterações condicionadas à presença |
| C01 | carimbo `app/escala/services/regeneracao.py:104`: `{data:{regenerada_em,editada_em}}` | `app/ponto/management/commands/gerar_celulas.py:257`: `**reescrita[dia]` para `cobranca_orfa`; assinatura receptora em `regeneracao.py:141` |
| C02 | mesmo | `app/ponto/management/commands/supra_juiz.py:170`, uso em chamada de `cobranca_orfa` no laço iniciado em `:260` |
| T01 | detector `app/escala/detector_proposta.py:123`, `:128`, `:153`: `padrao,proposta,confianca,porque`; proposta `{tipo_escala_id,codigo,descricao,ciclo,horario}`; padrão em `:63` e `:82` | `app/escala/proposta_pdf.py:75`, `:121`: outer completo, padrão `dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,padrao_dias,dows`; proposta `codigo,horario,ciclo`; **selo sim**, `app/escala/tests/test_detector_proposta.py:141` |
| T02 | mesmo | `app/escala/management/commands/propor_escala_limbo.py:38`: outer completo, padrão completo exceto `marcos_freq`, proposta `codigo,horario,ciclo` |
| T03 | mesmo | `app/escala/services/escala_auto.py:121`: `proposta,confianca,padrao.dias,porque`; proposta `tipo_escala_id,codigo,descricao,horario` |
| T04 | mesmo | `app/ponto/services/esmeril_espelho.py:234`: `proposta,porque,confianca`; proposta `codigo,descricao,horario` |
| Q01 | propostas `app/escala/services/propositor.py:465`, `:539`, `:585` | `app/chamados/services/proposta_no_fio.py:67`: `n,propostas`; proposta `chamados_que_morreriam,vinculo_id,tipo,ancora,ancora_atual,template,template_atual,motivo` |
| Q02 | mesmo | `app/chamados/services/painel.py:522`: `propostas[].colaborador_id` |
| Q03 | mesmo | `app/colaboradores/services/ficha.py:176`: `propostas`; `tipo,template_atual,template,ancora,casa_hoje,casaria,dias_batidos,aplicar_desde,chamados_que_morreriam` |
| Q04 | mesmo | `app/escala/management/commands/propostas_de_escala.py:39`, `:74`: `propostas,universo,n`; por proposta `colaborador_id,vinculo_id,colaborador,tipo,template_atual,ancora_atual,casa_hoje,dias_batidos,motivo,template,ancora,casaria,sobra,aplicar_desde,chamados_que_morreriam` |
| Q05 | mesmo | `app/escala/management/commands/ritmo_discordante.py:20`: `ritmo_discordante_vinculos` default `[]`, itens `colaborador_id,ciclo,ritmo,proposta`; `ritmo_discordante` default zero |
| Q06 | mesmo | `app/api/views_mensageria.py:2034`: `propostas` default `[]`; `colaborador,posto,ancora,template_atual,template,casa_hoje,casaria,chamados_que_morreriam,aplicar_desde,tipo` |
| Q07 | mesmo | `app/core/views_ia.py:575`: `propostas` default `[]`, `colaborador_id`; assinatura e seleção delegadas |
| O01 | resultado aplicar `app/escala/services/propositor.py:788`: `decisao_id,saida` | `app/chamados/services/proposta_no_fio.py:152`: **lê `ok` com default `True`, `msg` com default `None`; emissor não grava** |
| O02 | mesmo | `app/escala/management/commands/propostas_de_escala.py:69`: `saida,decisao_id`, coincide |
| K01 | kwargs de correção `app/escala/services/propositor.py:765`: `colab,vinculo,template,desde,ate,ancora,novo_trecho,regenerar_desde,apesar_da_lavra,apply,schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:63`: `colab,vinculo,template,desde,apesar_da_lavra,ancora,apply,ate,novo_trecho,regenerar_desde`; `schema` só pertence ao wrapper `handle` em `:50` |
| M01 | snapshot fase conflitante: schema `app/escala/services/fase_conflitante.py:92`; persistência `app/escala/management/commands/lavrar_fase_conflitante.py:35` | `app/escala/services/fase_conflitante.py:112`: cópia integral de `detalhe`, acrescenta `medido_em`; fallback `{total:None,esperado:0,sem_medida:True}` em `:111` |
| M02 | snapshot previsto cego: schema `app/escala/services/previsto_cego.py:193`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/previsto_cego.py:224`: integral + `medido_em`; fallback em `:223` |
| M03 | snapshot vigência impossível: schema `app/escala/services/vigencia_impossivel.py:48`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/vigencia_impossivel.py:72`: integral + `medido_em`; fallback em `:71`; **selo sim**, `app/escala/tests/test_contador_vigencia_impossivel.py:108` |
| M04 | snapshot vigência sem trilha: schema `app/escala/services/vigencia_sem_trilha.py:79`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/vigencia_sem_trilha.py:98`: integral + `medido_em`; fallback em `:97` |
| R01 | rótulo régua `app/escala/regua_defesa.py:632`: `classe,faixa,escore,texto` | `app/chamados/juizes.py:516`: só testa resultado `is None` |
| R02 | mesmo | `app/escala/services/furos_diarios.py:36`, `:305`: presença e `classe`, com default `''` em `:306`; não lê `faixa,escore,texto` |

Os campos de cada proposta Q vêm dos emissores internos de `app/escala/services/propositor.py:184`, `:191`, `:299`, `:303`, `:319`, `:339`, `:363`, `:371`, acrescidos pelo `p.update` em `:539` e `chamados_que_morreriam` em `:572`/`:575`. `motivo` e `sobra` têm variantes; o CLI só lê `sobra` no ramo que não é `grade_por_dia`, em `app/escala/management/commands/propostas_de_escala.py:87`.

**Três pares com desencontro de forma confirmado, além das omissões condicionais:**

1. **O01**: emissor retorna `decisao_id,saida`; consumidor consulta `ok,msg`. O default de sucesso está explícito; falha produtiva **[não verificado]**. O teste troca o retorno real por mock `{'ok': True}` em `app/chamados/tests/test_proposta_no_fio.py:112`, portanto não prende esse casamento de schemas. Dono provável ESTRUTURA; classe sim **[não verificado]**.
2. **R01 e R02**: o produtor entrega `faixa,escore,texto`, mas os únicos chamadores de produção localizados só usam presença/`classe`, conforme as linhas acima. Ausência de outros leitores por alias **[não verificado]**. O teste `app/escala/tests/test_termometro_regua.py:177` testa o produto diretamente, não obriga consumidores a lerem essas chaves. Dono provável ESTRUTURA; classe sim **[não verificado]**.

**Contagem do meu subconjunto:** 21 DNA + 30 ATA + 16 outros JSONFields + 25 portas/retornos + E01 = **93**, não 92. A diferença veio da inclusão explícita da ATA transitória E01 na consolidação. **93 costuras localizadas; 3 desencontros de forma confirmados; número adicional por omissões condicionais [não verificado]; 3 com selo de costura localizado, 90 sem selo localizado, inexistência dos 90 [não verificado].** Atribuição destas estruturas a `escala` segue o modelo proprietário, inclusive quando o escritor fica em `ponto`; se o relatório atribuir pelo arquivo emissor, precisa redistribuir.

**Residual já localizado, ainda fora dessas 93:** lavra de fase em `app/escala/services/fase_12x36.py:85`, produzida pelo command `app/ponto/management/commands/censo_fase_12x36.py:279`; consumidores `app/escala/services/cadastro_realidade.py:421`, `app/api/views_mensageria.py:1685`, além dos leitores internos da lavra em `fase_12x36.py:97`, `:117`, `:197`. Lavra C7 em `app/escala/services/regua_na_lista.py:48`, consumidores `app/escala/services/cadastro_realidade.py:347` e `app/escala/views.py:469`. Há ainda `ancora_sugerida → propositor` em `app/escala/services/fase_12x36.py:50`/`:64` e `app/escala/services/propositor.py:333`, e `impacto_da_troca → julgar_vinculo` em `app/escala/services/impacto_troca.py:47` e `app/colaboradores/services/vinculo.py:142`. Essas pendências impedem chamar o subtotal de censo total.

## 13/11 15:27 · sessao e6c5df440ac0 · - · - · origem=Ronald (tmux)

Ampliação dos dicionários transitórios: **13 pares adicionais enumerados, sem desencontro de chave obrigatório demonstrado neste recorte**. O produtor de `alimentar_portas` e o produtor da tarefa são estruturas diferentes; não há chamada de `tarefa_do_dia` no corpo atual de `censo_alcance`. (`app/chamados/juizes.py:1585`; `app/chamados/services/tarefa_do_dia.py:182`; `app/chamados/services/vigia_fio.py:604`, `:635`)

Todos os donos abaixo são **ESTRUTURA prováveis**, porque a unidade observada é o esquema entre funções/módulos, não a correção do cadastro ou da batida; classe **sim**, como contrato reutilizado. Atribuição provável, não resultado do oráculo L-099. (`CLAUDE.md:179`)

Os testes citados foram lidos, não executados; estado RED/GREEN nesta cópia [não verificado]. [nao verificado]

| # | Estrutura | Emissor; chaves escritas | Consumidor; chaves lidas/defaults | Desencontro | Selo que prende |
|---|---|---|---|---|---|
| T01 | partição dos verbos | `app/chamados/services/verbos.py:62`: `mapa`, `contagens`; mapa contém `chamado_id→verbo/None`; contagens contém `verbo→{n,atrasados,urgentes}` (`:37-39`) | `app/chamados/juizes.py:1540`: `['mapa']`, sem default | Não demonstrado; `contagens` não é consumida por este leitor, mas há outros leitores no próprio serviço (`verbos.py:201`) e fora deste recorte [não verificado] | Caminho real exercitado por `app/chamados/tests/test_e3_portas_tarefa.py:49-55` e `:113-127`; não é uma catraca de todo consumidor futuro |
| T02 | alimentação de fila | `app/chamados/juizes.py:1549`: `verbo`, `fila_ids`, `fila_cel` | `app/chamados/services/tarefa_do_dia.py:529-532`, `:546-549`: `fila_ids`, `verbo[pk]`, `fila_cel.get(celula_id,())`; nenhum default de chave superior | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_portas_tarefa.py:113-127`, `test_RED_lote_le_painel_uma_vez_sem_rejulgar_C6`, usa `wraps=alimentar_fila`, cobra uma alimentação, relógio, zero query na releitura e verbo da tarefa; casos de forma em `:32-82` |
| T03 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: anteriores + `vivas`, `colab`, `no_app={}`, `instancias`; `_no_app` acrescenta `no_app[colab_id]`/instância em `:1593-1601` | `app/chamados/services/vigia_fio.py:605`, `:610`, `:615`: `fila_ids`, `vivas`, `instancias`; passa a estrutura ao `_no_app` em `:621` e ao C17 em `:635` | Não demonstrado. Esse leitor usa os **insumos brutos**, além da resposta da porta | Integração do alcance lê produtor real em `app/chamados/tests/test_o261e_f5_alcance.py:310-315` e `:385-400`; selo independente de todas as chaves brutas [não verificado] |
| T04 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: `verbo,fila_ids,fila_cel,vivas,colab,no_app,instancias` | `app/escala/services/cadastro_realidade.py:484-488`: `vivas.get(pk) or ()`; restante encaminhado como `alimentacao` ao C17 | Não demonstrado | **Sim, integração de comportamento:** `app/escala/tests/test_f3_beco1_folga_na_lista.py:163-175`, `:177-196`, dois contraexemplos com C17 e lista reais; não checa lista textual de todas as chaves |
| T05 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: mesmas chaves | `app/chamados/management/commands/diff_e3_sombra.py:114`, `:62-64`: `fila_cel.get(cel_id,())`, `vivas.get(cel_id,())`, `fila_ids`; encaminha ao `_porta` em `:124` | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:150-163`, `test_AB_categorizado_e_diff_por_consumidor`, executa o command real e cobra casos A/B/diffs; `:165-174` cobre fila residual/isento |
| T06 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`: `rota,fila,verbos,fio`, ou `None` em `:1655`, `:1664` | `app/chamados/services/vigia_fio.py:635`, `:642-646`: se não None, `verbos.values()` e `rota`; sem defaults | Não demonstrado. `fila`/`fio` não são lidos por este consumidor | **Sim:** `app/chamados/tests/test_o261e_f5_alcance.py:317-324`, `test_MORDE_o_papel_SEGUE_o_juiz_e_nao_uma_tabela_do_vigia`, troca resposta do juiz e exige mudança no papel; produtor C17 tem bateria `app/chamados/tests/test_e3_pendentes_app.py:160-192` |
| T07 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`, ou None | `app/escala/services/cadastro_realidade.py:488`: só a distinção `is not None`, sem leitura de chave | Não demonstrado | **Sim:** dois contraexemplos reais `app/escala/tests/test_f3_beco1_folga_na_lista.py:163-196` |
| T08 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`, ou None | `app/folha/fora_da_folha.py:67`, `:78`: `fio`, None→`FIO_SEM_PORTA`; `:225`: `.get('fila') or ()`, depois `list` | Não demonstrado | **Sim:** `app/folha/tests/test_haiku_fora_da_folha.py:93-112`, juiz falso diz `xyz`/`abc` e a folha deve conservar; `:114-148` garante que número do chamado sai de `fila`, não das âncoras em `verbos` |
| T09 | resposta de porta C17 via casca `_porta` | `app/chamados/juizes.py:1667-1668`, casca devolve intacta em `app/folha/fora_da_folha.py:67` | `app/chamados/management/commands/diff_e3_sombra.py:124`, `:51-57`, `:139-141`: `fio,fila,rota,verbos`; None→`nada,(),None,[],False`, conforme projeção | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:138-144` fixa projeções; integração real em `:150-163` |
| T10 | tarefa do dia/CargaDaTarefa | `app/chamados/services/tarefa_do_dia.py:182-183`: `colaborador_id,dia_do_turno,agora,celula_id,nao_sei,razao,instancias`; itens em `:256-258`, ações em `:113`; `:553-554` acrescenta sempre `rota,fila,verbos,fio` antes de `CargaDaTarefa.tarefa` retornar `:533` | `app/chamados/management/commands/diff_e3_sombra.py:125`, `:44`: `rota,fila,verbos,fio`, só projeta se `rota` truthy; `:68`: `razao`; sem defaults de chaves | Não demonstrado. Demais dados da tarefa não são necessários à projeção desse leitor | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:131-136`, conserva exatamente quatro campos e None sem rota; integração `:150-174`; produtor real de tarefas selado por `app/chamados/tests/test_e3_portas_tarefa.py:32-82` |
| T11 | tarefa do dia para backfill | Mesmo produtor de T10; itens têm `instancia,tipo,hora,data_civil,lampada,vencido,motivo,pergunta_id,razao,acoes`; ação tem `acao,de,porta` (`app/chamados/services/tarefa_do_dia.py:113`, `:256-258`, `:316-318`) | `app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `:97-117`: `instancias`, `instancias[].{razao,pergunta_id,acoes}`, `acoes[].{de,acao,porta}`, `razao`, `nao_sei`; todos por acesso obrigatório, sem `.get` | Não demonstrado | Selo real produtor→backfill completo [não verificado]. `app/chamados/tests/test_e4b_classe_e_memoria.py:592-595` **substitui** tarefa por dict sintético e usa corpo real do leitor em `:638`; isso cobre leitor, não compatibilidade com produtor real |
| T12 | relatório censo_alcance | `app/chamados/services/vigia_fio.py:571-575`: `alcance_universo,alcance_competencias,alcance_porta_fila,alcance_porta_pergunta,alcance_de_hoje_pra_frente,fio_morto_alcance,alcance_porta_exige_o_admin,alcance_e_detalhe,alcance_por_rota_papel,alcance_dias,alcance_dias_omitidos`, `alcance_<causas/baldes>`; `:589` acrescenta `alcance_cego`; retorno `:700` | `app/chamados/management/commands/vigiar_fio_morto.py:176`, `:189`: encaminha inteiro a `linhas_alcance`; `:240`: `fio_morto_alcance`; `:243`: persiste inteiro dentro do carimbo `alcance` | Não demonstrado; leitura nominal e cópia integral | Selo específico desse par [não verificado]. Integridade dos campos/listas do produtor coberta por `app/chamados/tests/test_o261e_f5_alcance.py:385-400` |
| T13 | relatório/lista censo_alcance | Mesmo produtor de T12. Linha sempre `[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]` em `app/chamados/services/vigia_fio.py:629` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`: `alcance_dias`, posição 4=`sem_porta`; `:239`: `alcance_universo,fio_morto_alcance`; `:242-254`: posições 3/1 e desempacota as oito colunas, parse ISO de `data` | Não demonstrado | Selo produtor→backfill real completo [não verificado]. Teste de backfill usa linha sintética de oito colunas em `app/chamados/tests/test_e4b_classe_e_memoria.py:620-626`; integridade do produtor isolado em `app/chamados/tests/test_o261e_f5_alcance.py:385-400` |

A estrutura de `alimentar_portas` não pode ser tomada como “mesma resposta” da tarefa apenas por ambos terem informação sobre porta. A primeira entrega mapas brutos `fila_ids,vivas,instancias` usados no vigia (`app/chamados/juizes.py:1585-1587`; `app/chamados/services/vigia_fio.py:605-615`); a segunda entrega ações por instância e resume `fio` pelos donos das ações (`app/chamados/services/tarefa_do_dia.py:545-554`). O DIFF extrai da tarefa somente `rota,fila,verbos,fio` (`app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`) e mede diferenças conhecidas por consumidor (`app/chamados/tests/test_e3_diff_sombra.py:161-163`). Isso é diferença declarada, não desencontro acidental comprovado.

A memória e os números A1/A2/A3 da execução real não são inferíveis da leitura acima. [não verificado]

Adicional fora dessa contagem de 13:

| Estrutura | Emissor | Escritas | Consumidor | Leituras | Desencontro | Selo; dono; classe |
|---|---|---|---|---|---|---|
| push web de chamados | `app/chamados/push_utils.py:28-33` | `title,body,url,requireInteraction=True` | `app/static/sw.js:89-99`, `:102-107` | `title || 'Hasner Ponto'`, `body || 'Nova notificação'`, `urgente || false`; copia payload em notification.data | **Sim:** emissor escreve `requireInteraction`; consumidor procura `urgente`. `url` é transportada dentro de `data` (`:96`) mas o clique não a lê: foca primeira janela ou abre `'/'` (`:105-107`) | Selo específico [não verificado]; ESTRUTURA provável; classe sim |

Correção à minha contagem anterior: os dois pares `data_turno` das APIs leves devem ser classificados como **semânticas distintas observadas**, não automaticamente defeito, porque o catálogo distingue os módulos e a consequência alcançável não foi verificada. Fontes da diversidade continuam `app/api/views.py:714-716`, `app/api/views_core.py:708-710`, `app/ponto/management/commands/processar_alertas_turno.py:349`. Assim, dos 57 pares anteriores há **4 incompatibilidades de chave demonstradas** (retro→gerador, retro→models, retro→foto_recebida, bloqueio→catálogo), mais duas diferenças semânticas cuja classificação como defeito é [não verificado].

## 13/11 15:26 · sessao e5aa1e28ad97 · - · - · origem=Ronald (tmux)

Ampliação: **7 pares adicionais diretos/fronteiriços de retornos de portas**, mais **2 pares do retorno enriquecido de autorização por período**. Sem chave consumida ausente demonstrada; há campos retornados sem leitor de produção localizado. O inventário negativo desses campos continua limitado à busca estática. [não verificado]

| ID / estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | DESENCONTRO | Selo que prende | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| P41 retorno `decidir_he` | `app/ponto/portas/he.py:178`, `:203` | Ambos `{estado,minutos,criada,trocada,no_op}`; ramo idempotente `criada=False,trocada=False,no_op=True`, ramo gravado `no_op=False` | `app/ponto/views.py:529`, `:536` | Indexação de `no_op,estado`; no ramo alterado, `trocada,minutos`, `:540` | `criada` sem leitor de produção localizado; os consumidores internos de `he.py:242` e `:567` leem somente `no_op`. [não verificado] | **Sim para execução do par**, chamadas reais da view: `app/ponto/tests/test_tela_gestao_he.py:254`, `:264`; não prende a sobra `criada` [não verificado] | ESTRUTURA | sim para campo sem leitor [não verificado] |
| P42 retorno `recusar_em_lote` | `app/ponto/portas/he.py:234`, `:240`, `:243`, `:245`, retorno `:246` | `{recusados,no_op,erros}`; erros são tuplas `(colab_id,data,msg)` | `app/ponto/views.py:614` | `recusados,no_op,erros` por indexação; erros inteiros até cinco em `:622` | Nenhuma chave usada ausente; nenhum campo excedente localizado | **Sim**: caminho real tela→porta→view, `app/ponto/tests/test_tela_gestao_he_filtros.py:223`; idempotência do retorno da porta, `app/ponto/tests/test_tela_gestao_he.py:382` | ESTRUTURA | não |
| P43 retorno `autorizar_em_lote` | `app/ponto/portas/he.py:545`, `:565`, `:568`, `:570`, retorno `:571` | `{dias,autorizados,no_op,erros}` | `app/ponto/services/autorizacao_he_periodo.py:277` | `erros` por indexação `:282`; `autorizados` em `:299`; demais preservados para a saída; **reescreve `dias` como total dos itens recebidos**, incluindo os de fora da competência em `:281` | `dias` passa de quantidade dos itens entregues à porta (`he.py:545`) para quantidade de todos os itens do ato (`autorizacao_he_periodo.py:281`): mesmo nome com universos diferentes, transformação explícita | **Sim**: caso cinco dias e retorno, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:177`; borda fora da competência, `:291` | ESTRUTURA | sim, duas definições explícitas |
| P44 retorno `recusar_ponta_pequena` | `app/ponto/portas/he.py:330`, `:335`, `:344` | Todos `{recusada,ja_decidido,minutos}`; ramo já decidido devolve estado em `ja_decidido`; demais `None` | `app/ponto/management/commands/recusar_ponta_pequena.py:170` | `recusada` em `:179`; `minutos` em `:181` | `ja_decidido` sem leitor de produção localizado [não verificado] | **Sim para campos usados**, comando real: `app/ponto/tests/test_o214_ponta_pequena.py:411`; não prende sobra `ja_decidido` [não verificado] | ESTRUTURA | sim para campo sem leitor [não verificado] |
| P45 retorno `revogar_recusa_do_sistema` | `app/ponto/portas/he.py:448`, `:451`, `:458`, `:464`, `:467`, `:471`, `:477` | Todos `{revogada,por_que_nao,minutos_na_decisao,minutos}`; sem decisão `minutos_na_decisao=None`; motivo textual nos cinco ramos falsos; motivo `None` nos dois verdadeiros | `app/ponto/management/commands/lavrar_he_pendente.py:59` | Somente `['revogada']` em `:60`; devolve bool ao callback de `apurar` | `por_que_nao,minutos_na_decisao,minutos` sem leitor do **retorno** em produção localizado; os campos homônimos do modelo têm outro fluxo de leitura. [não verificado] | **Sim para `revogada` e fiação**, comando fim a fim: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:338`; não prende sobras [não verificado] | ESTRUTURA | sim para campos sem leitor [não verificado] |
| P46 retorno `regenerar_em_exportada` | `app/ponto/portas/regen_exportada.py:221` | `{celulas,vinculos,sem_cobertura,ambiguos,hash_antes,hash_depois,antes,depois,mudou,campos_que_mudaram,regua_antes,regua_depois}` | `app/chamados/services/passivo_validado.py:444` | `.get('mudou')` em `:448`; pauta lê `.get('antes')`, `depois or {}` em `:178`; `campos_que_mudaram or []` em `:180`; `regua_antes,regua_depois` em `:189`; `hash_antes,hash_depois,celulas` em `:207`; preserva inteiro no retorno em `:400` | `vinculos,sem_cobertura,ambiguos` sem interpretação pelos consumidores localizados; dict inteiro segue no relatório, portanto “ninguém lê” absoluto **não demonstrado** | **Sim para hash/pauta**, porta real: `app/chamados/tests/test_o317_passivo_pela_porta.py:223`; números pago/devido `:631` e `:650`; régua antiga `:695` | ESTRUTURA | não demonstrada |
| P47 mesmo retorno de regeneração, aninhado no relatório | `app/ponto/portas/regen_exportada.py:221`, repasse `app/chamados/services/passivo_validado.py:400` | Mesmo esquema de P46 | `app/chamados/management/commands/materializar_passivo_validado.py:267` | `(c['regen'] or {}).get('hash_depois','-')` em `:271`; `(c['regen'] or {}).get('mudou')` em `:287` | Nenhuma chave consumida ausente | **Não localizado**; os testes do serviço em `test_o317_passivo_pela_porta.py:223` não executam esse comando [não verificado] | ESTRUTURA | não |

A mudança explícita de significado de `dias` em P43 não demonstra resultado errado: o serviço inclui os itens recusados por estarem fora da competência no total do ato e os nomeia em `erros`. É o caso literal de “mesmo nome com dois significados”, documentado pela atribuição e pelo teste de borda. `app/ponto/services/autorizacao_he_periodo.py:268`, `:281`, `:282`; `app/ponto/tests/test_o214_item2_autorizar_periodo.py:291`.

| ID / estrutura adicional | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | Desencontro | Selo | Dono | Classe |
|---|---|---|---|---|---|---|---|---|
| P48 retorno enriquecido de `previa/confirmar` | `app/ponto/services/autorizacao_he_periodo.py:278`, `:281`, `:286`, `:300`, `:326`, `:356` | Base `{dias,autorizados,no_op,erros}`; acrescenta `{antes,depois,diff,exportada,periodo,previa}`; **`pauta_dp` só existe quando exportada e houve autorização**, `:299` | `app/ponto/views.py:709`, `:751` | Prévia lê `antes,depois,diff` em `:717`, `:720`; confirmação lê `autorizados,no_op,erros,exportada` em `:755`; repassa dict inteiro como `r` em `:714` | `pauta_dp` e `previa` sem leitura terminal de produção localizada [não verificado]; sua presença condicional não quebra os consumidores encontrados | **Sim para saída usada**, prévia HTML e confirmação com o formulário real: `app/ponto/tests/test_o214_item2_autorizar_periodo.py:579`, `:602`; não prende sobras [não verificado] | ESTRUTURA | sim para campos sem leitor [não verificado] |
| P49 mesmo retorno enriquecido | `app/ponto/services/autorizacao_he_periodo.py:286`, `:326`, via `app/ponto/views.py:714` | Esquema P48 | `app/templates/ponto/gestao_he_periodo_previa.html:23` | `periodo[0:2]`; `dias` em `:27`, `:94`, `:107`; `exportada` em `:31`; `erros`, tamanho e tuplas `[1],[2]` em `:65`, `:70` | Nenhuma chave consumida ausente; campos de P48 não usados aqui | **Sim para integração da prévia**, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:579` | ESTRUTURA | não |

**Offline encerrado como estrutura sem leitor, não uma costura adicional:** os dois emissores estão em `app/ponto/management/commands/testar_pwa.py:240` e `:260`, com `{tipo,timestamp_dispositivo,timestamp_real,timestamp_gap_ms}`, primeiro também `latitude,longitude`. A URL é ligada diretamente à função aposentada por `app/ponto/urls.py:13`. O destino `app/ponto/views.py:419` não parseia o corpo nem lê chave alguma: devolve sempre **410** em `:426`. O instrumento espera **200/201** no primeiro cenário, `testar_pwa.py:254`, e **400** no segundo, `:271`. Não há par gravador→leitor do dict; há emissão para uma porta aposentada. `app/ponto/views.py:419`.

**Contagem separada desta ampliação:** 9 pares PONTO; **1 desencontro de significado comprovado** (P43), **4 pares com campos sem leitor terminal localizado** (P41/P44/P45/P48), **1 par sem selo de execução localizado** (P47). Nos quatro pares com sobras, nenhum selo localizado prende especificamente a ausência de leitor dessas sobras. Os números de ausência são relativos à inspeção realizada, não prova universal. [não verificado]

## 13/11 15:25 · sessao 6af76433e3e9 · - · - · origem=Ronald (tmux)

Mudanças necessárias, por leitura da cópia:

- `vigiar_fio_morto.py::ESPERADO_DO_CARIMBO`: incluir contador de férias com esperado `0` (`chamados/management/commands/vigiar_fio_morto.py:98`).
- `vigiar_fio_morto.py::Command.handle`: chamar `ferias_concedidos_x_soma()` e levar seu resultado ao `censo`, pelo caminho já usado pelos demais contadores (`chamados/management/commands/vigiar_fio_morto.py:175`, `chamados/management/commands/vigiar_fio_morto.py:239`).
- A função compartilhada retorna listas distintas `soma_maior` e `concedidos_maior`; o tripwire conta ambas por `len`, sem outra soma (`ferias/services.py:116`, `ferias/management/commands/tripwire_status_ferias.py:38`).
- O item 3 pede explicitamente `concedidos_maior`; o anexo enfatiza `soma_maior`. Sugiro publicar ambos, com esperado `0`, preservando os sentidos já existentes (`ferias/services.py:117`).
- Ajustar o teste que compara os dicionários completos: `VigiaDoFioMortoTest.test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu`; incluir novos valores no `censo` e no `esperado` (`chamados/tests/test_o261_vigia_fio.py:815`, `chamados/tests/test_o261_vigia_fio.py:825`).
- Esse teste executa o comando, que lê o relógio; adicionar `freeze_time` com instante literal ao alterá-lo (`chamados/tests/test_o261_vigia_fio.py:806`, `chamados/management/commands/vigiar_fio_morto.py:166`).

Nenhuma escrita ou execução de teste realizada. Resultados de execução: [nao verificado]. [nao verificado]

## 13/11 15:24 · sessao e6c5df440ac0 · - · - · origem=Ronald (tmux)

Censo parcial de `chamados`: leitura de AST, `rg`, chamadas e corpos dos consumidores, sem importar Django nem executar testes. As declarações persistidas estão em `app/chamados/models.py:47` (`contexto_json`), `:1742` (`motivos`), `:1746` (`contexto`), `:2444` (`opcoes`) e `:3408` (`chamados_relacionados_ids`). A enumeração abaixo não comprova exaustividade de aliases dinâmicos, ORM indireto, templates/JavaScript e todos os pares possíveis. [não verificado]

**Achados de estrutura confirmados**

1. O exemplo fornecido confere: `app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103` escreve `foto_ausente.{tipo_batida,batida_id,batida_em,_por}`. `app/chamados/services/disputa_emissao.py:372` busca `motivo.data` e `:387` busca `ultima_batida.id`; `app/chamados/models.py:3464` busca apenas `ultima_batida.id`. São dois pares gravador–leitor, ambos com desencontro de episódio. A presença das chaves no contexto fundido em produção é [não verificado].

2. Há um terceiro consumidor da mesma forma: `app/api/foto_recebida.py:67-69` só trata a disputa cuja `ultima_batida.id` coincide com a batida recebida. A escrita retroativa acima não entrega essa chave. Dono provável ESTRUTURA e classe sim: são formas de contrato incompatíveis, independentemente de CPF/colaborador. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/api/foto_recebida.py:67`)

3. `app/ponto/views.py:280` escreve `contexto_json={'pendentes': resumo,'total':len(...)}`, renovando em `:288` ou persistindo no nascimento em `:301`, para `entrada_bloqueada_turno_aberto` (`:283`, `:299`). O catálogo declara que esse módulo tem `chave_data='data_turno'` em `app/chamados/catalogo/modulos.py:182`, e seu leitor consulta a chave declarada em `:225`. O emissor não entrega `data_turno`. Dono provável ESTRUTURA; classe sim. Efeito/quantidade em produção: [não verificado].

4. Mesmo nome com dois significados: `contexto_json.data_turno` das APIs leves recebe `localdate(batida.timestamp)`, data civil (`app/api/views.py:714-716`; `app/api/views_core.py:708-710`), enquanto emissores de turno escrevem o dia de turno/grade (`app/ponto/management/commands/processar_alertas_turno.py:349`; `app/ponto/management/commands/reconciliar_grade.py:517`). O catálogo e o leitor consomem ambos pela mesma chave (`app/chamados/catalogo/modulos.py:168`, `:225`). Incidência noturna em produção: [não verificado]. Dono provável ESTRUTURA; classe sim.

5. `horario_previsto` possui duas formas: hora `HH:MM` no detector (`app/chamados/detector_anomalias.py:234`), levada ao `contexto_json` das APIs em `app/api/views.py:726` / `app/api/views_core.py:720`; datetime ISO no detector de ausência (`app/ponto/management/commands/detectar_ausencias.py:708`). `app/chamados/data_evento.py:39` só interpreta a forma que contém data, enquanto `app/chamados/management/commands/nomear_marco_por_lampada.py:65-68` fatia `[11:16]`. A segunda rotina é selecionada por suas consultas; não conferi integralmente se alcança módulos leves. Portanto diversidade de forma comprovada, impacto/desencontro alcançável [não verificado].

**Selos**

- `app/chamados/tests/test_e1_v6_horizonte.py:252-276`, `EmissorBatidaPorEpisodioTest.test_RED_segunda_batida_na_disputa_aberta_ganha_pergunta`, monta manualmente a forma `ultima_batida.{id,tipo,timestamp}` em `:260`, chama a porta em `:268-269` e cobra os episódios em `:272-276`. Prende o caminho feliz da porta; **não prende todos os emissores reais**, portanto não sela as três costuras do retro.
- `app/chamados/tests/test_porta_contexto.py:199-232` prende escrita de campos/idempotência; **não prende o vocabulário entregue pelos emissores**.
- Selo específico de chaves para cada par enumerado abaixo não localizado na leitura realizada; ausência absoluta de teste [não verificado]. Não executar testes: regra de só leitura e ausência de banco autorizado. [não verificado] [nao verificado]

**Catálogo de escritores**

Porta material de atualização de `contexto_json`: `app/chamados/services/contexto.py:62-78` (`anotar`, todas as kwargs; grava as sem valor), `:82-104` (`renovar`, todas as kwargs; `decide_por` controla guarda), `:108-127` (`retirar`, remove chaves nomeadas), `:131-163` (`fundir`, lista por união, escalar ausente). Os módulos abaixo são os emissores lógicos; a porta não conhece um esquema fixo de chaves. (`app/chamados/services/contexto.py:71`, `:98`, `:146`)

| Estrutura/subtipo | Escritores e chaves |
|---|---|
| Disputa, batida API | `app/api/views.py:758-765`, `app/api/views_core.py:752-759`: `ultima_batida.{id,tipo,timestamp}`, além de `contexto_anomalia` produzido pelo detector. |
| Disputa, retro foto | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103`: `foto_ausente.{tipo_batida,batida_id,batida_em,_por}`. |
| Disputa, falta pela célula | `app/chamados/services/fabrica_da_celula.py:172`: `motivo.data`. |
| Disputa, saída sem entrada PWA | `app/ponto/services/triagem_batida.py:738-743`: `saida_sem_entrada.{data,hora_tentativa}`. |
| Disputa, detector | `app/chamados/detector_anomalias.py:49-53`: `orfao_14h.{entradas_pendentes,data,hora_abertura}`; `:55` `_erro_orfao`; `:65-68` `colisao_escala.{escalas_ids,count}`; `:92-98` `ordem_substituicao_invalida.ordens[].{ordem_id,problemas}`; `:103-106` `posto_desativado.{posto_id,posto_nome}`; `:111` `sem_posto={}`; `:119` `em_ferias` com dict delegado ou `info`; `:121` `_erro_ferias`. |
| Disputa, agregação | `app/chamados/services/disputa_emissao.py:548-562`: passa todas as chaves de `merge_contexto`, acrescenta `motivo.{qtd,ultima_em}`, preserva/funde demais chaves do motivo. Nascimento em `:590`. |
| Disputa, origens especiais | `app/chamados/services/disputa_emissao.py:630`: `origem`; `:1126`: `origem='manual_orfaos'`, `chamados_orfaos_ids`; `:1361`: `origem='ancora_limbo'`, `chamado_limbo_id`; `app/ponto/management/commands/reconciliar_grade.py:427`: `origem='grade'`; `app/chamados/management/commands/smoke_chamados.py:97-102`: motivo.data quando pergunta contém `{data}`. |
| Disputa, anotações | `app/chamados/models.py:2042-2046`: `revalidacoes[].{evento,em,motivo_removido,**contexto_extra}`; `:2104` / `:2122`: chave dinâmica recebida das portas; emissores concretos `app/chamados/views.py:1343` (`supervisao_omissa_escalado`), `app/ponto/management/commands/escalonar_chamados_supervisao.py:159` (`incidente_operacional`), `app/ponto/management/commands/alertar_disputas_pendentes.py:48` (`ultimo_alerta_diario`). |
| Disputa, entrada pendente | `app/chamados/services/disputa_emissao.py:292-296`: `motivo.data` por dia derivado dos `entradas_pendentes[].pk`. |
| Chamado, turno aberto | `app/ponto/management/commands/processar_alertas_turno.py:348-352`: `data_turno,entrada_timestamp,posto`; `app/ponto/management/commands/processar_alertas_avancados.py:126-129`: `data_turno,entrada_timestamp`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:135-180`: contexto variável `_ctx`, conteúdo integral não conferido. |
| Chamado, batida ausente | `app/ponto/management/commands/detectar_ausencias.py:706-712`: `data_turno,horario_previsto,atraso_min,marcos_faltantes`; `:559`: renova `marcos_faltantes,_marco_por`; `:766`: `folga_nao_declarada,nao_calado_porque`. |
| Chamado, reemissão c2 | `app/chamados/reconciliador.py:691`: `data_turno,origem`; condicionalmente `:708-709`: `marcos_faltantes,_marco_por`; a estrutura vai à porta em `:921`. |
| Chamado, grade | `app/ponto/management/commands/reconciliar_grade.py:516-518`: `data_turno,origem,marcos_faltantes[].{tipo,hora,motivo}`; `:473`: renova `marcos_faltantes,orfa_regeneracao_curada`. |
| Chamado, regeneração | `app/ponto/management/commands/gerar_celulas.py:264`: `orfa_regeneracao.{em,marcos_na_emissao}`. |
| Chamado, reconstrução de marco | `app/chamados/management/commands/nomear_marco_por_lampada.py:109`: `marcos_faltantes,_marco_por`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`: `batida_id,data_turno,_dia_por`; `app/ponto/management/commands/curar_residuo_bug115.py:124`: `marcos_candidatos,_marco_por`. |
| Chamado, entrada bloqueada | `app/ponto/views.py:280-301`: `pendentes,total`. |
| Chamado, anuência | `app/ponto/services/triagem_batida.py:347`: `tipo_batida,hora_tentativa`; `:353-362`: `motivo_anuencia,tipo_batida,hora_tentativa` e condicionais `foto_b64,foto_nome`; `:470`: `tipo_batida,hora_tentativa,hora_prevista,diferenca_minutos,escala`. |
| Chamado, acesso bloqueado | `app/ponto/services/triagem_batida.py:117-121`: `motivo,situacao,posto`. |
| Chamado, leve | `app/chamados/detector_anomalias.py:231-237`: `tipo,tipo_batida,horario_previsto,horario_real,desvio_minutos,escala`; APIs acrescentam `batida_id,data_turno` (`app/api/views.py:714-716`; `app/api/views_core.py:708-710`) e persistem em `:726` / `:720`. |
| Chamado, geofence | `app/ponto/services/geofence.py:345-352`: ocorrência com `batida_id,alerta_geofence_id,distancia_metros,batida_lat,batida_lon,tipo_batida`; `:359`: agrega `ocorrencias`; `:375-394`: `ocorrencias`, campos da primeira ocorrência e `tipo_alerta,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon`. |
| Chamado, ausência | `app/ponto/services/ausencia.py:672-676`: `ausencia_id,data,tipo,tem_documento`; `app/api/views.py:3120`: renova `tem_documento`. |
| Chamado, agenda | `app/colaboradores/views.py:2376`: `agenda_dia_ids`. |
| Chamado, aceite escala | `app/colaboradores/services/vinculo.py:419-424`: `snapshot_ecs,ec_nova,template_id,template_nome,data_inicio`. |
| Chamado, desligamento | `app/colaboradores/services/desligamento.py:182`: `data_demissao,motivo_informado,executado_por_username,executado_por_id,executado_em`. |
| Chamado, férias | `app/ponto/utils.py:120`: `agendamento_pk,data_inicio,data_fim`. |
| Chamado, justificativa rejeitada | `app/ponto/services/justificativa.py:301`: `data_turno,tipo,motivo_rejeicao,rejeitado_por`; `app/api/views.py:1978`: `data_turno`. |
| Chamado, par relâmpago | `app/ponto/management/commands/detectar_par_relampago.py:383`: `data_turno,par,duracao_min`. |
| Chamado, cluster | `app/ponto/services/cluster_espurio.py:211`: `data_turno,orfas,n_missing`. |
| Chamado, cadastro | `app/ponto/management/commands/detectar_ausencias.py:224`: `escala_id,tipo_escala_id,tipo_escala_codigo,tipo_ciclo,colabs_mesmo_template,causa,data_inicio`; `:294`: `escala_id,tipo_ciclo,data_inicio`; `:532`: `data_inicio,causa,escala_id`; `:538`: `dias_sem_grade`. |
| Chamado, régua | `app/escala/regua_defesa.py:72-76`: `tipo_escala_id,tipo_escala_codigo,causa,data_exemplo`. |
| Chamado, adesão | `app/chamados/services/adesao.py:79-87`: `adesao,data_inicio,periodo`; `:113`: `agrupado_em`. |
| Chamado, limbo | `app/chamados/services/limbo_folgas.py:68-69`: `sugestao.{classe,dias,ndias,semanas}`; `:276-277`: `agrupado_em,defeito_cadastro`. |
| Chamado, escape | `app/chamados/services/declaracao_texto.py:208`: `escape_colab,escape_colab_em`. |
| Chamado, abertura humana | `app/chamados/services/abertura.py:58-64`: `sistema,nome,empresa,posto,pendente_glpi`; `:183-192`: acrescenta `aberto_por_user_id,aberto_por` e chave de dia calculada quando recebe `data_fato`; `:116`: `pendente_glpi=False`. |
| Chamado, agrupamento disputa | `app/chamados/services/disputa_emissao.py:1116`, `:1122`: `chamados_orfaos_ids`; `:1351`: `origem,chamado_limbo_id`; `:1357`: `chamados_limbo_ids`; `:580`, `:586`: cópia/fusão do contexto de nascimento da disputa. |
| Chamado, escalonamento | `app/ponto/management/commands/escalonar_chamados_supervisao.py:64`, `:138`, `:163`, `:179`, `:195`: `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h`. |
| Pergunta, opções | `app/chamados/services/disputa_emissao.py:330-342` e `:449`: lista do catálogo (`valor,rotulo` quando dict), posto `id,nome`, escala `id,tipo_escala__template_nome`; `:842`, `:882`, `:1155`, `:1215`, `:1247`, `:1376`: listas delegadas/escapes/vazias; `app/chamados/services/acoes_disputa.py:267`: lista textual; `app/chamados/services/declaracao_texto.py:376`, `:666`: vazia. |
| Pergunta, clones | `app/chamados/services/disputa_emissao.py:834-845`: `chamados_relacionados_ids` quando `relacionados_ids is not None`. |

**Catálogo dos leitores de chaves**

| Estrutura | Leitores verificados |
|---|---|
| Disputa.contexto | `app/chamados/services/disputa_emissao.py:59-73`: `entrada_sem_saida.{entrada_id,entrada_aberta_em}`, `ultima_batida.id`; `:191-196`, `:372`: `motivo.data`; `:278-280`: `motivo.entradas_pendentes[].pk`; `:387`: `ultima_batida.id`; `:551-552`: motivo.qtd com default 1; `app/chamados/models.py:3464`: `ultima_batida.id`; `app/chamados/catalogo/query.py:58-66`: `motivo.data`, sem ela retorna None quando texto exige data; `app/chamados/juizes.py:391`: `motivo.data`; `app/chamados/services/materializacao.py:346-352`, `:478-484`: `motivo.data` só como fallback a `p.data_evento`; `app/chamados/reconciliador.py:1561-1569`: `motivo.data`, fallback a `p.data_evento`; `app/chamados/management/commands/auditar_disputas.py:75-91`: `motivo`, `data`; `app/api/foto_recebida.py:67-69`: `ultima_batida.id`; `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:52-55`: `device_comprometido.{motivo,device_id_prefix}`. |
| Chamado.contexto_json, data | `app/chamados/catalogo/modulos.py:219-225`: chave dinâmica `MOTIVOS[modulo]['chave_data']`; `app/chamados/juizes.py:118`, `:129`, `:159-166`: `data_turno,batida_id,ultima_batida.timestamp,entrada_timestamp`; lote em `:187`, `:197`; `app/chamados/data_evento.py:27-39`: `data_turno,data_batida,entrada_timestamp,pendentes,horario_previsto`; todas por `.get` sem default explícito. |
| Chamado.contexto_json, turno | `app/chamados/reconciliador.py:146`: `entrada_timestamp`; `:198`: `par`; `:243`: `chamados_orfaos_ids`; `app/chamados/services/disputa_emissao.py:145`: `entrada_timestamp`; `:1143`: `marcos_faltantes`; `app/ponto/services/lastro.py:75-78`: `marcos_faltantes[].{hora,tipo}`; `app/escala/services/regeneracao.py:148-150`, `:196`: `marcos_faltantes`, itens.hora; `app/escala/utils.py:1062-1066`: `agrupado_em,marcos_faltantes`; `app/ponto/management/commands/reconciliar_grade.py:376-381`: `orfao_14h.{data,entradas_pendentes[].data}`, `pendentes`; `:445-465`: `orfa_regeneracao,marcos_faltantes`; `app/ponto/worklist.py:54-55` e `app/ponto/management/commands/reconciliar_fantasmas.py:183-184`: `pendentes`. |
| Chamado.contexto_json, agrupamento | `app/chamados/models.py:816`, `app/chamados/juizes.py:1723`, `:1747`, `app/chamados/services/adesao.py:111`, `app/chamados/services/limbo_folgas.py:253`: `agrupado_em`; `app/chamados/reconciliador.py:243`, `app/ponto/services/entrada_sincronizada.py:51-55`, `app/chamados/signals.py:361`: `chamados_orfaos_ids` inclusive consulta JSON ORM. |
| Chamado.contexto_json, ações | `app/chamados/services/acoes_chamado.py:30-61`: `tipo_batida` default `'E'`, `hora_tentativa` default `''`, `motivo_anuencia` default `''`, `foto_b64`, `foto_nome` default `'selfie.jpg'`; `:96`, `:403`: `agenda_dia_ids` default `[]`; `:144-146`: presença/valor de `snapshot_ecs`; `:185-210`: `distancia_metros,alerta_geofence_id,tipo_batida,posto_nome,raio_metros,posto_lat,posto_lon,batida_lat,batida_lon`; `:247`: `batida_id,ocorrencias[].batida_id`; `:267`, `:275-276`: `batida_id,distancia_metros,posto_nome,raio_metros`; `:371`: `hora_tentativa`. |
| Chamado.contexto_json, ausência | `app/chamados/views.py:1512`, `app/api/views.py:1045`, `app/ponto/services/ausencia.py:715`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:32`: `ausencia_id`; `app/api/views.py:3114`: consulta `contexto_json__ausencia_id`. |
| Chamado.contexto_json, cadastro | `app/chamados/views.py:441`, `app/chamados/services/limbo_folgas.py:64`, `:84`: `tipo_escala_id`; `:82-85`: `sugestao.{dias,classe}`; `app/escala/regua_defesa.py:328-334`: `destino.{codigo}`, `tipo_escala_codigo`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`: `classe`; `app/colaboradores/services/pos_vinculo.py:38`: `periodo`. |
| Chamado.contexto_json, reparadores/auditor | `app/chamados/management/commands/nomear_marco_por_lampada.py:50`, `:65`, `:70`: `marcos_faltantes,horario_previsto,origem`; `app/ponto/management/commands/curar_residuo_bug115.py:76-86`, `:121`: `marcos_faltantes,horario_previsto,origem,marcos_candidatos`; `app/ponto/management/commands/gerar_celulas.py:259`: `marcos_faltantes`; `app/chamados/management/commands/auditar_invariantes_chamados.py:93`, `:104`, `:617`, `:686`: `entrada_timestamp`; `:1016`: chave dinâmica do catálogo. |
| Opcoes | `app/chamados/models.py:3540-3543`: `valor`, `rotulo` com default `resposta_colab`; `app/chamados/services/disputa_emissao.py:1435-1458`: opções textuais, dict.valor e dict.id conforme tipo; `app/api/views.py:2538-2540`: lista inteira, filtra `valor='OUTRO_MOTIVO'`; `:1157`: transmite inteiro; `app/api/views_mensageria.py:566`: transmite inteiro ou `[]`; `app/templates/chamados/partials/_card_atendimento.html:105-108`: texto; `:148-149`: `id,nome,label,descricao,tipo_escala__template_nome`; `:166-167`: `valor,rotulo`. |
| Chamados_relacionados_ids | `app/chamados/services/materializacao.py:781-786`: lista ou `[]`, consulta clones; `:975`: reemite lista; `app/chamados/services/validacao.py:431-433`: lista ou `[]` na ordem de travas. |
| Leitura genérica | `app/templates/chamados/partials/painel_chamado_turno.html:33-35` percorre `contexto_json.items`; portanto não afirmar que campos de contexto_json são globalmente “ninguém lê” apenas porque não há `.get` nominal. |

**Pares prontos para a consolidação**

Para não inflar “sem selo”, todos abaixo têm **selo específico do par [não verificado]**; os dois selos parciais descritos acima não cobrem o vocabulário de todos os emissores. “Não demonstrado” significa que não encontrei incompatibilidade, não uma prova universal. [não verificado]

| # | Estrutura | Emissor → consumidor | Escritas → leituras | Desencontro | Dono provável; classe |
|---|---|---|---|---|---|
| 1 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/chamados/services/disputa_emissao.py:387` | foto_ausente.batida_id → ultima_batida.id | Sim | ESTRUTURA; sim |
| 2 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/chamados/models.py:3464` | idem | Sim | ESTRUTURA; sim |
| 3 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/api/foto_recebida.py:67` | idem | Sim | ESTRUTURA; sim |
| 4 | disputa.contexto | `app/api/views.py:758` → `app/chamados/models.py:3464` | ultima_batida.id → mesma | Não demonstrado | ESTRUTURA; sim |
| 5 | disputa.contexto | `app/api/views_core.py:752` → `app/chamados/models.py:3464` | idem | Não demonstrado | ESTRUTURA; sim |
| 6 | disputa.contexto | `app/api/views.py:758` → `app/api/foto_recebida.py:67` | idem | Não demonstrado | ESTRUTURA; sim |
| 7 | disputa.contexto | `app/api/views_core.py:752` → `app/api/foto_recebida.py:67` | idem | Não demonstrado | ESTRUTURA; sim |
| 8 | disputa.contexto | `app/chamados/services/fabrica_da_celula.py:172` → `app/chamados/services/disputa_emissao.py:372` | motivo.data → mesma | Não demonstrado | ESTRUTURA; sim |
| 9 | disputa.contexto | `app/ponto/services/triagem_batida.py:738` → `app/chamados/services/disputa_emissao.py:372` | saida_sem_entrada.data → mesma | Não demonstrado | ESTRUTURA; sim |
| 10 | disputa.contexto | `app/chamados/services/disputa_emissao.py:590` → `app/chamados/catalogo/query.py:58` | contexto_nascimento → motivo.data | Condicional; origem-only não grava data; texto pode dispensá-la `query.py:56` | ESTRUTURA; sim |
| 11 | disputa.contexto | `app/chamados/services/disputa_emissao.py:562` → `app/chamados/juizes.py:391` | motivo.data, demais → motivo.data | Não demonstrado | ESTRUTURA; sim |
| 12 | disputa.contexto | `app/chamados/services/disputa_emissao.py:562` → `app/chamados/services/materializacao.py:346` | motivo.data → mesma se data_evento ause

[truncado em 20000 de 28724 caracteres]

## 13/11 15:24 · sessao e5aa1e28ad97 · - · - · origem=Ronald (tmux)

Recorte PONTO: **40 pares identificados, 4 com desencontro confirmado, 35 sem selo do par localizado**. Contagem por `(estrutura, módulo gravador, módulo leitor)`; vários pontos no mesmo módulo aparecem juntos. Isso é um **mínimo enumerado**, não fecho de “TODO dict”. [não verificado]

Só li fontes. Usei `rg`, inspeção numerada e AST em memória para localizar chamadas de `ponto.registro_batida`, inclusive aliases importados. Não importei a aplicação, não rodei testes e não consultei banco ou logs. O método não fecha aliases dinâmicos, reflexão, consumidores externos, esquemas guardados apenas no banco nem todas as estruturas transitórias. [não verificado] [nao verificado]

Nos quadros, **E** = ESTRUTURA provável para eventual quebra do contrato; **—** = desencontro não demonstrado, não certificação de compatibilidade; **N** = selo do par não localizado [não verificado]. “Classe sim” aparece somente nos desencontros confirmados. A classificação estrutural segue `CLAUDE.md:179`.

**Catálogo dos esquemas, para evitar repetir chaves:**

- **RB**: porta `registrar_batida`: parâmetros `colaborador,timestamp,tipo,origem`, `autoridade_humana=False`, `fato_de_chao=None`; `**campos` passa inteiro ao modelo; sanitização só examina `latitude,longitude,gps_accuracy` presentes. A omissão de `fato_de_chao` deriva `not autoridade_humana`; a consulta de competência ocorre quando resulta falso. `app/ponto/registro_batida.py:5`, `:94`, `:105`, `:130`, `:152`, `:157`.
- **AN**: porta `anotar`: lê todas as chaves de `campos`, valida contra `CAMPOS_ANOTAVEIS`, compara com atributos atuais, grava e monta trilha; `extra` funde no `depois`. Permitidas: `tipo,aprovada,aprovada_por,aprovada_em,origem,retratada_em,foto,justificativa,pergunta_origem`. `app/ponto/registro_batida.py:203`, `:235`, `:241`, `:244`, `:261`.
- **CA**: `causa_espelho`: `{causa,dias_furo,dias_previstos,evidencia}`, ou `None` se classificação falhar. Produção do dict: `app/folha/causas.py:33`, `:36`, `:45`; persistência: `app/ponto/services/fechamento.py:1080`.
- **VI**: snapshot vínculo: `{total,achados,esperado,por_colab}`, item `{colab,veredito,posto_id,posto,motivo,dist_atual_m,sugerido}`. Leitura acrescenta `medido_em,data_ref`; sem snapshot retorna `total=None,por_colab={},esperado=0,sem_medida=True`. `app/ponto/services/vinculo_na_lista.py:105`, `:118`, `:132`, `:133`.
- **TE**: snapshot teto: `{total,colabs,universo,por_competencia,por_dono,janelas,competencias,erros,erros_amostra,itens,rotulo,onde_olha}`; item `{colab,empresa,data,competencia,entrada,saida,hi,hf,vao_min,teto_min,ec,template_id,dono}`. `app/ponto/services/teto_na_lista.py:78`, `:96`. Leitura acrescenta `medido_em,data_ref`; sem medida `{total:None,itens:[],sem_medida:True}`. `:125`.
- **CX**: snapshot CXR: campos de TE, mais `sem_dia,repetidos_do_motor,fora_da_competencia,por_causa,cobertos`; item-base `{colab,empresa,competencia,data,ec,template_id,dono}`, campos opcionais `causa,real,cadastrado,minutos_fora,entrada_real,entrada_cadastrada,saida_real,saida_cadastrada,delta_entrada_min,delta_saida_min`. `app/ponto/services/cxr_na_lista.py:30`, `:84`, `:125`.
- **HE**: retrato `{competencia,data_ini,data_ref,colabs,dias,pendentes,erros,recusas_revogadas}`; colaborador `{colab,nome,dias_trabalhados,dias,minutos,sem_decisao}`; dia `{data,estado,minutos,pontas}`; ponta `{hora,marco,minutos,sentido}`. `app/ponto/services/he_pendente_lavrado.py:148`, `:160`, `:169`.
- **ES**: esmeril `{lavrado_em,universo,colabs_com_anomalia_recorrente,por_assinatura,por_colab,destino}`; assinatura `{o_que_se_repete,dono,colabs,ocorrencias,top5}`. `app/ponto/services/esmeril_espelho.py:222`, `:227`, `:229`.
- **VF**: quatro snapshots do vigia futuro, cada um com a medida inteira `{contadores,selos,universo,sem_vinculo_do_dia,na_lista_do_admin_c6,em_dia,universo_julgada,julgada_via_admin,ini,fim,esperado,medido_antes_do_gerador}`, mais `total,rotulo`; leitor acrescenta `medido_em`; ausência devolve `total=None,esperado=0,sem_medida=True,rotulo`. `app/ponto/services/vigia_cadastro_futuro.py:83`, `:103`, `:119`, `:121`.
- **AB**: snapshot `{mudos,recusados,total,abriram,janela_horas}`; item `{colaborador_id,colaborador,posto}`, recusados incorporam dados de `erros`. `app/ponto/services/abriu_nao_bateu.py:117`, `:142`, `:145`, `:147`. Leitor acrescenta `medido_em`; ausência `{total:None,sem_medida:True}`. `:159`.

| ID / estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas | Desencontro | Selo do par | Dono | Classe |
|---|---|---|---|---|---|---|---|---|
| P01 RB | `app/ponto/views.py:1815` | RB + `autoridade_humana=True,justificativa,aprovada,aprovada_por` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P02 RB | `app/api/views.py:654`; mapa `:562` | RB + `autoridade_humana=False,canal,sequencia,latitude,longitude,gps_accuracy,timestamp_dispositivo`; condicionais `ordem_substituicao,posto_efetivo` | `app/ponto/registro_batida.py:94` | RB; geo presentes; encaminha demais | — | N | E | não |
| P03 RB | `app/api/views_core.py:645`; mapa `:548` | Mesmas chaves de P02 | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P04 RB | `app/ponto/services/justificativa.py:188`, `:361` | RB + `autoridade_humana=True,justificativa,aprovada,aprovada_por`; só `:361` passa `aprovada_em` | `app/ponto/registro_batida.py:94` | RB, encaminhamento integral | Presença desigual de `aprovada_em`; consumidor não a exige explicitamente | N | E | não |
| P05 RB | `app/ponto/services/veredito_celula.py:323` | RB + `autoridade_humana=True,aprovada_por,is_intervalo` | `app/ponto/registro_batida.py:94` | RB, encaminhamento integral | — | N | E | não |
| P06 RB | `app/ponto/services/triagem_batida.py:702`; mapa `:682` | RB + `autoridade_humana=False,canal,sequencia,latitude,longitude,gps_accuracy,justificativa,aprovada`; condicionais `ordem_substituicao,posto_efetivo` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P07 RB | `app/chamados/services/regularizacao_ext.py:78` | RB + `autoridade_humana=False,aprovada,aprovada_por,aprovada_em,justificativa`; omite `fato_de_chao` | `app/ponto/registro_batida.py:124` | `autoridade_humana`; `fato_de_chao=None` deriva verdadeiro, não consulta competência | Mesmo default significa fato de chão para regularização institucional; a própria porta declara que regularização deve passar falso em `:126` | **Não**; teste adjacente chama a porta diretamente, sem esse emissor: `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:56` | E | **sim** |
| P08 RB | `app/chamados/services/materializacao.py:933`, `:1013` | Ambos `autoridade_humana=False,fato_de_chao=False,aprovada,aprovada_por,aprovada_em`; `:1013` também `sequencia,justificativa` | `app/ponto/registro_batida.py:94` | RB | — | N; teste `test_ajuste_retroativo_folha_fechada.py:56` sela porta direta, não todos os emissores | E | não |
| P09 RB | `app/chamados/services/acoes_chamado.py:44` | RB + `autoridade_humana=True,sequencia,justificativa,aprovada,aprovada_por` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P10 RB | `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229` | RB + `sequencia`; origem teste | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P11 AN | `app/ponto/views.py:472`, `:1835` | `aprovada,aprovada_por,aprovada_em,origem`; outro ramo `justificativa`; `:472` fornece `depois=_snap` | `app/ponto/registro_batida.py:235` | AN; trilha explícita quando fornecida | — | N | E | não |
| P12 AN | `app/ponto/services/justificativa.py:202`, `:377` | `justificativa` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P13 AN | `app/ponto/services/flip_batida.py:30` | `campos.tipo`; trilha `antes.tipo`; `depois.tipo,causa` | `app/ponto/registro_batida.py:235` | AN; trilha explícita encaminhada | — | N | E | não |
| P14 AN | `app/ponto/services/retratar_humano.py:207` | `aprovada,retratada_em` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P15 AN | `app/chamados/services/materializacao.py:954` | `pergunta_origem` | `app/ponto/registro_batida.py:235` | AN | — | N; `app/chamados/tests/test_selo_batida_com_origem.py:143` verifica campo permitido, não todos os contratos AN | E | não |
| P16 AN | `app/chamados/services/acoes_chamado.py:54` | `justificativa` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P17 semeadura `list[dict]` | `app/ponto/management/commands/gerar_batidas_smoke_motor.py:182`; chamada `:107` | `colaborador,timestamp,tipo,sequencia,terminal,tipo_registro,escala,origem,aprovada` | `app/ponto/registro_batida.py:335` | Todos via `Batida(**d)` | — | N; gate de localização da escrita não sela o conteúdo: `app/ponto/tests/test_chokepoint_batida_gate.py:42` | E | não |
| P18 CA | `app/ponto/services/fechamento.py:1080` | CA | `app/ponto/services/fechamento_lista.py:122` | Dict inteiro repassado como `causa` | `dias_furo,dias_previstos` sem leitura terminal localizada [não verificado] | N | E | não |
| P19 CA | `app/ponto/services/fechamento.py:1080` | CA | `app/folha/retrato_folha.py:1113` | `causa` por `.get`, ou `None` se não dict | Igual P18 [não verificado] | N; `app/folha/tests/test_retrato_folha_quadros.py:1459` prende não reclassificar, não esquema integral | E | não |
| P20 CA | `app/ponto/services/fechamento.py:1080` | CA, passando pela linha de `fechamento_lista.py:122` | `app/templates/ponto/_fech_linha.html:16` | `causa` com default `zzz`; `evidencia` em `:18` | Igual P18 [não verificado] | N | E | não |
| P21 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/escala/services/cadastro_realidade.py:363` | `data_ref or '?'`; itens `motivo or veredito,posto or '-',sugerido`; filtro usa `por_colab` e `posto_id` em `vinculo_na_lista.py:147` | — | **Sim**, cadeia lavra→lista: `app/ponto/tests/test_c2_vinculo_vira_lista.py:63` | E | não |
| P22 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/ponto/services/geofence.py:77` | Existência da chave colaborador após `colabs_em_revisao`; filtro `posto_id` em `vinculo_na_lista.py:154` | — | **Sim**, mesma lavra e mudança de posto: `app/ponto/tests/test_c2_vinculo_vira_lista.py:73` | E | não |
| P23 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:33` | `sem_medida`; restante chega pela lista C8 | — | **Sim**, recusa sem lavra e fechamento só dos listados: `app/ponto/tests/test_c2_vinculo_vira_lista.py:87` | E | não |
| P24 TE | `app/ponto/services/teto_na_lista.py:113` | TE | `app/escala/services/cadastro_realidade.py:376` | `itens or ()`; item `colab,data,ec,template_id,dono,competencia,entrada,saida,vao_min,hi or '-',hf or '-',teto_min` | — | **Sim**, lavra→lista e horários: `app/ponto/tests/test_col129_teto_vira_lista.py:71` | E | não |
| P25 CX | `app/ponto/services/cxr_na_lista.py:145` | CX | `app/escala/services/cadastro_realidade.py:393`, `:522`, `:1020` | `itens`; campos opcionais com `.get`; `colab,data,ec,template_id,dono,competencia`; dono também usa `cobertos` e datas via `cxr_lavrado`, `app/ponto/services/cxr_na_lista.py:176` | —; há duas formas opcionais declaradas em `cxr_na_lista.py:29` | **Sim**, motor→lavra→tela: `app/ponto/tests/test_cxr_tela_le_o_motor.py:84` | E | não |
| P26 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/views.py:801`, `:830`, `:906` | Dict repassado a enriquecer; PDF lê `competencia,data_ini,data_ref,colabs or []` | — | N; renderer/ausência têm testes, sem fecho de todas as chaves do produtor: `app/ponto/tests/test_tela_gestao_he.py:82`, `:99` | E | não |
| P27 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/services/gestao_he.py:236`, `:246`, `:278`, `:470` | `colabs or []`, `data_ini,data_ref`; colaborador `colab,dias or [],dias_trabalhados`; dia `data,minutos or 0,pontas or []`; ponta `sentido or 'antes'` | —; `estado` da foto é substituído pelo estado vivo em `:278`, uso diferente explicitado em `:271` | N; igualdade contador/tela tem selo parcial em `app/chamados/tests/test_atalho_he_na_central.py:134` | E | não |
| P28 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/management/commands/recusar_ponta_pequena.py:98`, `:113`, `:128` | `colabs or []`, colaborador `colab,dias`; dia `data,minutos`; idade vem de `calculado_em` em `:105` | — | N | E | não |
| P29 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/chamados/services/validacao.py:897` | `por_colab or {}`, chaves dos códigos por colaborador | — | N | E | não |
| P30 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/escala/services/escala_auto.py:169` | `por_colab or {}`, `.get(str(colaborador_id))` em `:181` | — | N | E | não |
| P31 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/escala/services/cadastro_realidade.py:886` | `por_colab or {}`, `destino or {}` em `:892`; códigos por colaborador | — | N; produtor e leitor têm testes separados, leitor recebe fixture: `app/escala/tests/test_cadastro_x_realidade.py:47`, `:53` | E | não |
| P32 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/api/views_mensageria.py:1793` | `lavrado_em,universo,colabs_com_anomalia_recorrente,por_assinatura` por indexação em `:1796` | — | N | E | não |
| P33 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/ponto/services/pautas_do_esmeril.py:30`, `:33`, `:79` | `por_colab` default `{}`, códigos; `lavrado_em` default `?` | — | N; testes usam dict fabricado, não lavra real: `app/ponto/tests/test_pautas_do_esmeril.py:22`, `:29` | E | não |
| P34 VF | `app/ponto/services/vigia_cadastro_futuro.py:104` | VF | `app/api/views_mensageria.py:648` | Repassa todas as chaves dos quatro contadores | — | N; writer/read tests separados e snapshot simulado: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:821`, `:842` | E | não |
| P35 AB | `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:47` | AB | `app/ponto/services/abriu_nao_bateu.py:160` | Dict inteiro; acrescenta `medido_em` | — | N | E | não |
| P36 AB | `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:47` | AB | `app/api/views_mensageria.py:139` | Dict inteiro via `ultimo_lavrado()`; exceção `{total:None,indisponivel:True}` em `:142` | — | N | E | não |
| P37 push | `app/ponto/webpush.py:44` | `title,body,url` | `app/static/sw.js:89` | `title || 'Hasner Ponto'`, `body || 'Nova notificação'`, `urgente || false`; guarda dict inteiro em notification.data | `urgente` ausente; `url` não é usado no click, que abre `/` em `sw.js:107` | N | E | **sim** |
| P38 push | `app/ponto/management/commands/processar_alertas_turno.py:279` | `title,body,url,requireInteraction` | `app/static/sw.js:89` | Igual P37 | `requireInteraction` escrito, leitor consulta `urgente`; `url` sem uso no click | N | E | **sim** |
| P39 realtime | `app/ponto/realtime.py:32` | `tipo='batida',colab_id,batida_id,ts` | `app/sse_server.py:92` | `n.payload` inteiro, retransmitido como SSE em `:93`; não interpreta chaves | —; consumidor browser não localizado [não verificado] | N | E | não |
| P40 push | `app/ponto/management/commands/processar_alertas_avancados.py:207`, `:240` | Ambos `title,body,url,requireInteraction` | `app/static/sw.js:89` | Igual P37 | Igual P38 | N | E | **sim** |

**JSON persistidos sem par externo de leitura demonstrado — não entram nas 40 costuras:**

| Estrutura | Todos os escritores localizados | Chaves | Leitura / selo |
|---|---|---|---|
| `DiaPago.detalhe` (`app/ponto/models.py:378`) | `app/ponto/services/dia_pago.py:195`, `:257`, `:266`, `:272`, `:277`, `:333`, `:353`, `:356`, `:474`, `:512` | `credito_parcial_min,folga_feriado_min,folga_sem_escala_min,o85_dia_sem_vinculo_min,falta_min,calculador_nao_decide,o_que,dias_sem_regua_no_mapa`; os dois retratos finais escrevem `o_que` | Leitor literal de `folga_feriado_min` está no **mesmo módulo**, `app/ponto/services/dia_pago.py:897`. Leitor externo das chaves não localizado [não verificado]. |
| `FechamentoMensal.motivos_espelho` (`app/ponto/models.py:451`) | `app/ponto/services/fechamento.py:1067`; clone com o mesmo valor em `:1186` | Lista retornada por `folha.export.motivos_retencao_espelho` | Leitor externo do **campo persistido** não localizado [não verificado]. `folha.export.py:972` escreve outro dict `item['motivos_espelho']`, não prova leitura desse JSONField. |
| `Advertencia.detalhe_json` (`app/ponto/models.py:1173`) | `app/chamados/services/acoes_chamado.py:203` | `distancia_metros,raio_metros,posto_nome,posto_lat,posto_lon,batida_lat,batida_lon` | Leitor de produção não localizado [não verificado]. O teste `app/ponto/tests/test_s127_advertencia.py:25` fabrica `distancia,raio,posto` e só verifica roundtrip, não o esquema do emissor. |

**Ampliação identificada, ainda fora da contagem acima:**

- `testar_pwa.py:240` e `:260` são payloads **de batida offline**, não push: `{tipo,timestamp_dispositivo,timestamp_real,timestamp_gap_ms}`; primeiro também `latitude,longitude`; endpoint `/ponto/batida-offline/` em `:249`. Consumidor despachado por `app/ponto/views.py:419`; rastreamento completo ao leitor efetivo pendente [não verificado].
- `app/ponto/portas/he.py:178`, `:203` emite `{estado,minutos,criada,trocada,no_op}`; `:330`, `:335`, `:344` emite `{recusada,ja_decidido,minutos}`; comando lê `recusada` em `app/ponto/management/commands/recusar_ponta_pequena.py:179`. `revogar_recusa_do_sistema` emite formas em `he.py:448`, `:451`, `:458`, `:464`, `:467`, `:471`, `:477`; `app/ponto/management/commands/lavrar_he_pendente.py:60` lê `revogada`. Leitores restantes/selos não fechados [não verificado].
- `app/ponto/portas/regen_exportada.py:221` emite `{celulas,vinculos,sem_cobertura,ambiguos,hash_antes,hash_depois,antes,depois,mudou,campos_que_mudaram,regua_antes,regua_depois}`; `app/chamados/services/passivo_validado.py:444` recebe em `local['regen']`. Leituras posteriores/selos não fechados [não verificado].
- Emissor push adicional de CHAMADOS: `app/chamados/push_utils.py:28` escreve `{title,body,url,requireInteraction:True}` para o mesmo consumidor `app/static/sw.js:89`, com o mesmo desencontro de P38. Deve entrar na contagem de CHAMADOS, não PONTO.
- `colabs_medidos` é lido com fallback por `app/ponto/services/esmeril_espelho.py:329`, enquanto o único produtor localizado desse dict grava `universo`, não `colabs_medidos`, em `:229`. É desencontro interno ao mesmo módulo; não é par entre módulos desta contagem. Não inferir efeito em produção. `app/ponto/services/esmeril_espelho.py:229`, `:329`.

## 13/11 15:21 · sessao 8d66b0d6f2f9 · HJ-090 · verificador · origem=Code

**Persistem quatro bloqueantes de código**, detalhados abaixo. Execução do RED na base, suíte e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **BLOQUEANTE — triagem 5 meio-curada: commit ainda precede o registro efetivo de reversão.** O arquivo `.aplicado.jsonl` nasce antes do lote, mas cada entrada só é escrita após `realinhar_concedidos()` retornar; o `atomic` da porta já terminou nesse momento. ([command:89](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/management/commands/realinhar_concedidos_ferias.py:89); `app/ferias/management/commands/realinhar_concedidos_ferias.py:91–95`; `app/ferias/services.py:150–164`)

   **Contra-exemplo:** a porta grava concedidos `30→33`; `reg.write()`, `flush()` ou `fsync()` falha. O período fica realinhado, com trilha no banco, mas sem entrada íntegra no arquivo aceito por `--reverter`. O snapshot inicial não resolve: contém foto anterior à trava e o próprio comando recusa usá-lo para reversão. Uma última linha JSON truncada também impede carregar todas as entradas anteriores, pois o arquivo inteiro é decodificado antes de começar a reversão. (`app/ferias/management/commands/realinhar_concedidos_ferias.py:54–60`, `:80–95`; `app/ferias/services.py:155–162`)

   **Falha da trilha no meio do lote:** se PA1 terminou e PA2 recebeu `evento() == None`, PA1 permanece aplicado; PA2 desfaz sua alteração; os seguintes não são executados. A atomicidade está por período, não pelo lote. Isso distingue a cura efetiva da triagem 6 da janela ainda aberta na triagem 5. (`app/ferias/services.py:150–164`, `:404–411`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:90–96`)

2. **BLOQUEANTE — escritor de insumo do saldo ainda valida antes da trava.** A edição do direito compara o pedido com `dias_concedidos` da foto da request. A porta abre `atomic`, mas não relê o período sob trava nem recalcula seu status. ([view:844](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/views.py:844); `app/ferias/services.py:725–733`)

   **Contra-exemplo:** edição lê concedidos `0` e aceita direito `20`; outra request agenda `30` pela porta travada; a edição grava depois, deixando saldo `−10`. Mesmo sem concorrência, direito `30`, concedidos `20`, vendidos `10` aceita direito `20`, porque a validação ignora vendidos. É escritor de um insumo que o juiz lê, embora não escreva diretamente `dias_concedidos` ou `dias_vendidos`. (`app/ferias/views.py:834–849`; `app/ferias/services.py:493–511`, `:728–733`; `app/ferias/models.py:219–220`)

3. **BLOQUEANTE — triagem 7 meio-curada: o contador não chega ao ESTADO pelo caminho existente.** O tripwire agora retira `OK` nos dois sentidos e alarma `soma_maior`. Entretanto, só imprime o resultado; o produtor do carimbo consumido pelo ESTADO não inclui os contadores de férias em `esperado`/`censo`. ([tripwire:38](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/management/commands/tripwire_status_ferias.py:38); `app/chamados/management/commands/vigiar_fio_morto.py:98–99`, `:228–241`)

   **Contra-exemplo:** PA com concedidos `30`, soma `33`: o tripwire retorna alarme, mas a execução do produtor do carimbo não entrega esse contador ao ESTADO. O leitor genérico existe e mostraria chaves divergentes, desde que fossem produzidas. (`app/ferias/management/commands/tripwire_status_ferias.py:48–57`; `bin/fio_estado.py:125–126`, `:191–194`; `bin/gerar_estado.py:260–261`)

4. **BLOQUEANTE — triagem 12 meio-curada no caminho até o Haiku.** A API envia `saldo_negativo_dias`; a ferramenta reconstrói o resultado sem repassar esse campo. ([ferramenta:2109](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/mensageria/nucleo/ferramentas.py:2109); `app/api/views_mensageria.py:835–842`)

   **Contra-exemplo:** oito períodos antigos com saldo positivo e o nono com saldo `−3`. A situação ordena por início, a API limita os períodos a oito e a ferramenta descarta o agregado negativo. O Haiku recebe saldo positivo e nenhum valor explícito de saldo negativo. O aviso de consumo acima do direito ainda chega; portanto, não afirmo ocultação completa do problema. (`app/ferias/situacao.py:163–164`, `:179–190`, `:300–302`; `app/api/views_mensageria.py:41`, `:842`; `mensageria/nucleo/ferramentas.py:2113–2119`)

   Na tela, o número negativo individual permanece visível, sem corte. (`app/templates/ferias/saldo.html:41`; `app/templates/ferias/_secao_colab.html:50`)

5. **NAO-BLOQ — RED aceita erro de programação como recusa válida.** `_recusou()` captura qualquer `Exception`. **Contra-exemplo:** trocar o corpo da porta por um `TypeError` antes de escrever deixa verdes os dois testes diretos de recusa por saldo: houve exceção e o banco permaneceu intacto. ([RED:45](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/test_hj090_red_por_valor.py:45); `app/ferias/test_hj090_red_por_valor.py:77–79`, `:91–93`)

   Os testes históricos também não conferem a resposta nem o motivo da recusa: uma negativa antecipada de permissão satisfaria suas asserções de ausência de escrita. Isso enfraquece o selo; não prova que ele deixe de reprovar a implementação anterior por valor. A mordida **executada na base** é **[nao verificado]**. (`app/ferias/test_hj090_red_por_valor.py:40–42`, `:67–71`, `:82–87`, `:96–98`)

6. **NAO-BLOQ — POST convencional preserva o prefill, mas pode escolher um período incapaz de receber o pedido.** Escolhe o primeiro `p.pode_agendar`, cujo critério exige apenas adquirido e saldo positivo. ([view:221](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/views.py:221); `app/ferias/models.py:233–234`)

   **Contra-exemplo:** primeiro período adquirido com saldo `10`, segundo com saldo `30`, histórico futuro de `30` dias. Abre o primeiro formulário com `value=30`, `max=10`, apesar de existir outro período suficiente. Presença desse arranjo nos 32 casos fornecidos: **[nao verificado]**. (`app/ferias/views.py:221–228`; `app/templates/ferias/agendar.html:22`)

7. **NAO-BLOQ — exclusão introduz ordem inversa à documentada.** A ordem declarada é agendamento → período → ausência; a exclusão trava período, apaga ausências e depois apaga agendamento. ([services:744](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/services.py:744); `app/ferias/services.py:557–563`, `:748–749`; `app/ferias/models.py:456–460`; `app/ponto/services/ausencia.py:1267–1271`)

   **Contra-exemplo estrutural:** excluir um período só com agendamento cancelado percorre período → ausência → agendamento. Deadlock concreto entre portas oficiais: **[nao verificado]**, pois a exclusão recusa vivos e `_travar_agendamento` recusa cancelados antes de pedir a trava do período. (`app/ferias/services.py:561–563`, `:745–749`)

   O abono integrado pede a mesma trava do período três vezes, mas mantendo a ordem: entrada da porta, execução do agendamento e releitura para vender. Não há inversão nesses três pedidos. (`app/ferias/services.py:245`, `:493`, `:258`)

8. **NAO-BLOQ — permanecem relógios internos e comando sem `--apply`.** O histórico agora repassa `hoje`, mas o agendamento calcula aviso com outro relógio e a troca de janela também lê a data internamente. **Contra-exemplo:** atravessar a meia-noite entre o cálculo de aviso da view e o da porta pode produzir feedback de 30 dias e gravação de 29 dias de antecedência. (`app/ferias/services.py:451`, `:476`, `:491`, `:637`; `app/ferias/views.py:307–320`)

   Executar apenas `alertar_ferias_vencendo` pode modificar status: chama `sincronizar_status()` incondicionalmente. **Contra-exemplo:** período adquirido cujo limite venceu passa a vencido durante o comando sem flag de aplicação. (`app/ferias/management/commands/alertar_ferias_vencendo.py:14–32`; `app/ferias/models.py:152–158`; `app/ferias/services.py:327–344`)

A conferência dos itens PROCEDENTES da triagem fica assim; “fechou” abaixo significa fechamento pelo código, sem certificação de execução **[nao verificado]**.

| Item | Resultado da cura |
|---|---|
| **1 — NAO-BLOQ** | Os julgamentos específicos de agendamento, abono integrado e venda isolada passaram para depois da trava. Subsiste o escritor de direito do achado 2. (`app/ferias/services.py:245–246`, `:493–505`, `:795–805`) |
| **2 — NAO-BLOQ** | Sincronização e garantia de período existente agora julgam a linha relida sob trava. (`app/ferias/services.py:327–344`, `:374–378`) |
| **4 — NAO-BLOQ** | A contagem de agendamentos vivos passou para dentro da porta, sob trava; ressalva de ordem no achado 7. (`app/ferias/services.py:743–749`) |
| **5 — BLOQUEANTE** | Antes/depois vêm da linha travada; identidade inclui PK e dias dos vivos; arquivos usam abertura exclusiva. Continua a janela commit → registro, achado 1. (`app/ferias/services.py:137`, `:155–176`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:83–95`) |
| **6 — NAO-BLOQ** | Para realinhar e reverter, retorno `None` da trilha levanta dentro do atomic e desfaz aquela unidade. (`app/ferias/services.py:161–162`, `:182–183`, `:404–411`) |
| **7 — BLOQUEANTE** | Alarme corrigido; entrega ao ESTADO ausente, achado 3. (`app/ferias/management/commands/tripwire_status_ferias.py:38–57`; `app/chamados/management/commands/vigiar_fio_morto.py:233–241`) |
| **8 — NAO-BLOQ** | Universo agora é Ausencia férias × batidas apuráveis, sem filtro de status; status e véspera são impressos. Reprodução dos 40 dias: **[nao verificado]**. (`app/ferias/management/commands/listar_ferias_com_batida.py:40–69`) |
| **9 — NAO-BLOQ** | Prefill convencional implementado no redirect, GET e template; ressalva da escolha do período no achado 6. (`app/ferias/views.py:213–228`, `:331–339`; `app/templates/ferias/agendar.html:18–22`) |
| **10 — NAO-BLOQ** | A conta paralela foi removida: `status_de_periodo` chama o `fget` do juiz registrado, inclusive para objeto com os mesmos campos. (`app/ferias/models.py:95–98`, `:219–220`; `app/core/juizes.py:264`) |
| **11 — NAO-BLOQ** | Histórico repassa `hoje` aos dois status; resíduos descritos no achado 8. (`app/ferias/services.py:451`, `:476`) |
| **12 — BLOQUEANTE** | Situação inclui períodos negativos e agregado separado; caminho até Haiku permanece parcial, achado 4. (`app/ferias/situacao.py:300–302`; `mensageria/nucleo/ferramentas.py:2109–2119`) |

O censo dos campos encontra os seguintes escritores e leitores:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **PA.dias_concedidos** | Realinhar, reverter, histórico, agendar, cancelar DP/admin, trocar janela e aprovar solicitação. (`app/ferias/services.py:156`, `:179`, `:474`, `:509`, `:577`, `:604`, `:635`, `:706`) | Juiz do saldo; contador; reversão; edição do direito; telas. (`app/ferias/models.py:220`; `app/ferias/services.py:125–126`, `:176`; `app/ferias/views.py:844`; `app/templates/ferias/saldo.html:37`) |
| **PA.dias_vendidos** | Abono integrado, histórico e venda isolada. (`app/ferias/services.py:259`, `:475`, `:806`) | Saldo, teto vendável e telas. (`app/ferias/models.py:220`, `:238`; `app/templates/ferias/_secao_colab.html:51`) |
| **PA.dias_saldo** | Não é coluna: deriva de direito − concedidos − vendidos; o escritor adicional de direito está no achado 2. (`app/ferias/models.py:218–220`; `app/ferias/services.py:732–733`) | Status, validação normal, abono, histórico, remarcação, aprovação, venda, situação e API. (`app/ferias/models.py:98`; `app/ferias/views.py:250`; `app/ferias/services.py:294–302`, `:470`, `:631`, `:690`, `:802`; `app/ferias/situacao.py:69`; `app/api/views_mensageria.py:841`) |
| **PA.status** | Portas de saldo por `aplicar_status`; sincronização; garantia/criação por defaults. (`app/ferias/models.py:215`; `app/ferias/services.py:344`, `:369–378`) | Permissões de agendar/vender, seleção na aprovação, tripwire, listas e telas. (`app/ferias/models.py:234`, `:242`; `app/ferias/services.py:92`, `:684–686`; `app/ferias/management/commands/listar_ferias_dp.py:67–75`; `app/templates/ferias/_secao_colab.html:42–45`) |
| **AG.dias/datas** | Histórico, agendamento normal e solicitação criam; `_trocar_janela` altera existentes. (`app/ferias/services.py:447–473`, `:506–508`, `:695–705`, `:636–649`) | Soma/identidade, fracionamento, cancelamento, remarcação, materialização, folha e API. (`app/ferias/services.py:105–137`, `:577`, `:604`, `:635`; `app/ferias/models.py:363–386`; `app/ponto/services/ausencia.py:1185–1207`; `app/folha/export.py:491–493`; `app/api/views.py:2244–2247`) |
| **AG.status** | Histórico fornece status; criações normais usam default; cancelamentos, troca de janela e cron atualizam. (`app/ferias/services.py:451`, `:506–508`, `:580–581`, `:612–613`, `:641–649`, `:695–705`, `:825–829`; `app/ferias/models.py:319`) | Portas de escrita, universo da soma, juiz de férias na janela e espelho Ausencia. (`app/ferias/services.py:53`, `:106`, `:137`, `:561`; `app/ponto/services/ausencia.py:1181`) |
| **Ausencia férias** | `AG.save` chama espelhamento; cria dias, remove cancelados, apaga janela e rejeita no cancelamento admin. Portas genéricas também editam e mudam estado. (`app/ferias/models.py:449–454`; `app/ponto/services/ausencia.py:1203–1208`, `:1189–1193`, `:1263–1284`, `:498–509`, `:825–836`) | Cobertura de ausência, materializador e lista com batidas. (`app/ponto/turnos.py:2554`, `:2753`, `:3158`; `app/ponto/services/ausencia.py:1182–1186`; `app/ferias/management/commands/listar_ferias_com_batida.py:40–63`) |

As portas genéricas de Ausencia continuam podendo modificar férias sem as travas de PA/AG; **contra-exemplo:** editar a data da ausência espelhada modifica a ausência sem mudar a janela do agendamento. Esse é o resíduo remetido a HJ-091 na triagem fornecida; implementação dessa obra: **[nao verificado]**. (`app/ponto/services/ausencia.py:465–509`)

Há também consumidores de saldo em relatórios e no intake IA, além dos leitores acima. (`app/relatorios/services.py:290`, `:316`; `app/relatorios/views.py:370`; `app/core/views_ia.py:261–269`, `:532–538`, `:684–688`)

Os contra-exemplos restantes pedidos têm estes resultados pelo código:

- **NAO-BLOQ — reversão consecutiva 2×:** para antes `30`, depois `33`, a primeira volta a `30`; a segunda é pulada porque `30 != 33`. Trocar o conjunto por outros agendamentos de mesma soma também é recusado pela identidade PK/dias. (`app/ferias/services.py:137`, `:174–185`)
- **NAO-BLOQ — cancelar 33 depois de realinhar:** concedidos passa de `30` para `33`; cancelar devolve `33`, deixando concedidos `0`. O corte antigo não permanece nesse caminho. (`app/ferias/services.py:152–158`, `:604–606`)
- **NAO-BLOQ — rótulo contraditório permanece:** saldo `−3` recebe status `concedido`, cujo tooltip diz “Nada pendente”. O número negativo aparece, mas o texto continua inadequado. (`app/ferias/models.py:98–100`; `app/templates/ferias/_secao_colab.html:44`, `:50`)
- **NAO-BLOQ — data cravada removida da lista com batidas:** `--desde` agora é obrigatório; erros do dono são impressos e contados. (`app/ferias/management/commands/listar_ferias_com_batida.py:25`, `:55–66`)
- A lista DP imprime os dois números e origem/status do período usado; a origem é inferida de `obs`. A simulação pergunta ao validador normal e força rollback, mas não executa a porta normal completa. Aceitação dos mesmos **32**, separação medida dos **52** e reprodução dos **40/quatro prioritários**: **[nao verificado]**. (`app/ferias/management/commands/listar_ferias_dp.py:26–32`, `:55–84`, `:98–111`)

_4654 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
