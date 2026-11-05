# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 18:12** · publicado em **05/11/2026 18:12** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 18:12 · sessao 720daba17099 · PC1-PEP · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A CASOS PELA REGRA

O hash identifica a cópia informada; sua correspondência com um objeto Git não pôde ser conferida nesta árvore sem `.git`. [nao verificado]

A escolha está respondida: **candidata 1, dentro de `carimbar`, transação, releitura travada e novo julgamento por C6**, com conferência do memo e medição da ordem de trava antes do verde. A indefinição registrada no `pd_pep.r` foi superada pelo aval posterior. `app/docs/PROMPTS.md:1882`.

Os cadastros abaixo são cenários de ensaio; sua ocorrência em produção não está demonstrada pelo dado fornecido. Os resultados esperados vêm da regra, conforme L-110. `app/docs/LEIS.md:136`. Ocorrência em produção: [nao verificado].

| Caso | Cadastro + estado | Ato | Resultado exigido pela regra |
|---|---|---|---|
| A1 — furo continua | Dia de trabalho, fato encerrado, marco faltante lavrado; sem resposta nem decisão humana. | Carimbar `sem_turno_aberto`, `completude_turno` ou equivalente automático. | **Recusar enquanto a pergunta vive**; carimbo não substitui a resolução do fato. `CLAUDE.md:167`; `CLAUDE.md:170`; `app/docs/PROMPTS.md:1882`. |
| A2 — um marco resolvido | Batida acendeu o marco desta pergunta; outro marco mantém o dia acusador. | Encerrar a pergunta do marco aceso. | Reconhecer o fato deste marco, conservando a cobrança do outro. `CLAUDE.md:167`; `app/chamados/models.py:2257`; `app/docs/CORTES.md:413`. |
| A3 — resposta discordante | Pergunta respondida, sem validação; resposta discorda da batida ou não oferece hora comparável. | Carimbo automático. | Conservar a pergunta para decisão administrativa; não transformar silêncio do sistema em concordância. `app/docs/PROMPTS.md:1442`; `app/chamados/juizes.py:1090`; `app/chamados/juizes.py:1096`. |
| A4 — resposta concordante | Batida acendeu o marco, resposta concorda pela régua existente e o dia não acusa. | Rejulgar e encerrar. | Pode encerrar automaticamente pela regra já autorizada, sem tolerância nova. `app/docs/PROMPTS.md:1442`; `app/chamados/juizes.py:1090`. |
| A5 — decisão humana entre coleta e escrita | Consumidor conserva pergunta antiga; admin decide o mesmo dia e grava `admin_decidiu`. | Consumidor tenta seu carimbo automático antigo. | Reler sob trava e preservar decisão, autor e trilha; a tentativa antiga não pode substituir a via humana. `app/docs/PROMPTS.md:1882`; `app/docs/LEIS.md:65`; `app/chamados/services/relavra_via.py:214`. |
| A6 — resposta nova antes da trava | Consumidor conserva resposta A validada; colaborador grava resposta B e desfaz a validação anterior. | Consumidor tenta carimbar/validar usando a intenção formada sobre A. | A assinatura de A não valida B; conservar resposta nova e sua pendência. `app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:333`; `app/chamados/services/disputa_emissao.py:1287`. |
| A7 — resposta nova depois do carimbo | Escritor da resposta já carregou pergunta sem validação; outro ato valida/carimba antes de ele salvar. | Escritor antigo salva resposta nova. | A validação anterior não pode sobreviver como assinatura da resposta nova; a propriedade vale também nessa ordem de intercalação. `app/docs/CORTES.md:333`; `app/docs/LEIS.md:65`; `app/chamados/services/disputa_emissao.py:1287`. |
| A8 — concorrência e repetição | Duas tentativas sobre a mesma pergunta, sem novo fato entre elas. | Executar ambas; repetir a mesma entrada. | Decisão sobre linha travada; repetição equivalente preserva estado, uma trilha e zero duplicata. `app/docs/PROMPTS.md:1882`; `CLAUDE.md:175`. |
| A9 — competência 21→20 | Empresa cadastrada com corte 21; fatos em 20/10 e 21/10; competência 10 trancada e 11 aberta. | Julgar cada pergunta. | 20/10 pertence à competência 10; 21/10 à 11. Aplicar corte cadastrado e competência do fato. Tranca não fabrica batida ou quitação. `CLAUDE.md:81`; `app/ponto/janelas.py:10`; `app/chamados/juizes.py:1142`. |
| A10 — cross-meia-noite | 12x36 19–07, pausa 23–00; às 00:10 a saída das 07:00 ainda não venceu. | Encerrar porque a data civil mudou; consumir volta às 00:00. | A virada civil não encerra o fato; a volta é D+1 dentro da jornada iniciada em D. `CLAUDE.md:185`; `app/docs/JUIZES.md:24`; `app/docs/LEIS.md:117`. |
| A11 — provisório e chegada da batida | Célula provisória não acusa; marco perguntado continua pendente. Depois chega sua batida. | Tentar encerrar antes e rejulgar depois. | Antes, não absolver pelo provisório; depois, reconhecer a batida na passada seguinte. `CLAUDE.md:185`; `app/docs/CORTES.md:413`. |
| A12 — vínculo trocado | Célula histórica gerada pelo vínculo A; hoje existe B com outros marcos. | Julgar/carimbar o passado. | Respeitar geradora e DNA históricos; troca atual não autoriza reescrita silenciosa. Regeneração formal é outro ato. `app/docs/JUIZES.md:98`; `CLAUDE.md:148`; `app/escala/alimentacao.py:231`. |
| A13 — célula sem ata | `ata={}`, `trabalha=None`, sem outro fato resolutivo. Variante: `trabalha=False`. | Encerrar pela ausência de lâmpadas apagadas. | Com `None`, vazio não prova dispensa: **cobra**. Com `False`, cala pela declaração de não trabalho. `app/docs/CORTES.md:416`. |
| A14 — célula contraditória | `trabalha=True`, `veredito='concorde'`, ata vazia e nenhuma prova do marco. | Carimbar pelo veredito. | A ata vazia não prova resolução. Resultado exato dessa combinação contraditória: **[nao verificado]**; teste atual não pode virar regra. `app/docs/LEIS.md:95`; `app/docs/LEIS.md:136`; `app/docs/CORTES.md:416`. |
| A15 — ausência parcial | Ausência cobre parte do dia; marco perguntado está fora do período coberto. | Absorver/carimbar pela ausência. | Cobrar o resto descoberto; parcial não dispensa o dia inteiro. `app/docs/JUIZES.md:58`; `app/docs/JUIZES.md:60`. |
| A16 — continente fechado | Pergunta respondida; disputa encerrada por desligamento/admin, sem decisão do dia. | Transformar fechamento da disputa em `admin_decidiu`. | Não inventar decisão humana; a resposta continua para Validar. `app/docs/CORTES.md:342`. |
| A17 — erro e restauração | Candidato aplicado em memória; C6, lavra ou salvamento falha. | Abortar o ato. | Não deixar a instância declarando escrita que não persistiu; conservar coerência do estado e trilha. `app/docs/LEIS.md:65`; `CLAUDE.md:175`. |
| A18 — antiguidade | Pergunta antiga, marco ainda devido, sem decisão ou prova resolutiva. | Encerrar somente pela idade. | Idade não apaga a lâmpada nem substitui o fato. `app/docs/CORTES.md:253`; `app/docs/PROMPTS.md:1317`. |

## B SITIOS

**Fonte da alteração:** `PerguntaDisputa.carimbar`. O corpo atual aplica candidato, consulta C6 e salva a instância recebida; restaura carimbo e lavra nas exceções tratadas. A releitura travada deve entrar antes dessas mutações, preservando esse escritor. `app/chamados/models.py:2551`; `app/chamados/models.py:2572`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`; `app/chamados/models.py:2605`.

**Autoridades existentes:** C6 responde vitalidade; C7 fechamento da disputa; C15 vitalidade da coleção; C17 alcance da porta; C18 dia sem trabalho. Competência usa `janela_atual`; vínculo histórico usa `vinculo_do_dia`; meia-noite usa `marcos_turno`. O registro vivo prevalece sobre o lookup anexado. `app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`; `app/core/juizes.py:283`; `app/docs/JUIZES.md:24`; `app/docs/JUIZES.md:98`; `app/docs/JUIZES.md:14`.

**Campos, escritores e leitores centrais:**

| Campo/insumo | Escritores encontrados | Leitores centrais |
|---|---|---|
| Tripla `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, chamado por carimbo comum, identidade, restauração e descarimbo; limpeza persistida por `descarimbar_em_lote`. `app/chamados/models.py:2545`; `app/chamados/models.py:2575`; `app/chamados/models.py:2608`; `app/chamados/models.py:2626`; `app/chamados/models.py:2637`; `app/chamados/models.py:2652`. | C6, lastro, estado, SQL de vivas e procedência. `app/chamados/juizes.py:647`; `app/chamados/juizes.py:1135`; `app/chamados/models.py:2488`; `app/chamados/models.py:2735`; `app/ponto/services/procedencia_batida.py:72`. |
| Via isolada | `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. Portanto, a via isolada ainda tem escritores além da porta da tripla. `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. | Relavra, elegibilidade do descarimbo, cartório e apresentação. `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:179`; `app/ponto/services/cartorio.py:1599`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
| `respondida_em` | Carimbo/restauração; resposta humana; resposta automática; recuperação pela trilha; correção e ressuscitação. `app/chamados/models.py:2578`; `app/chamados/models.py:2609`; `app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1429`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`. | C6, estado, validação, filas e API. `app/chamados/juizes.py:1150`; `app/chamados/models.py:2492`; `app/chamados/services/validacao.py:363`; `app/api/views.py:2636`. |
| `validada_em`, `validada_por` | Carimbo/restauração; validação/devolução; resposta nova; correção; ressuscitação. O lookup chama validação de escritor único, mas o carimbo também atribui esses campos. `app/chamados/models.py:2581`; `app/chamados/models.py:2610`; `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/docs/JUIZES.md:38`. | C6 antes dos demais degraus; estado; seleção do materializador; procedência. `app/chamados/juizes.py:1136`; `app/chamados/models.py:2488`; `app/chamados/services/materializacao.py:1293`; `app/ponto/services/procedencia_batida.py:72`. |
| `resposta_colab` | Resposta humana, automática e limpeza para correção. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1393`. | Porta, concordância com luz, slot/lastro, materializador, API, calendário, ficha e prévia. `app/chamados/models.py:2576`; `app/chamados/juizes.py:1113`; `app/chamados/juizes.py:277`; `app/chamados/services/materializacao.py:1183`; `app/api/views.py:2580`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`. |
| Identidade: disputa, célula, destino, motivo, data e tipo | Nascimento por `abrir`; reaproveitamento muda célula/destino; reapontamento também pode preencher data. `app/chamados/models.py:2387`; `app/chamados/models.py:2425`; `app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:834`; `app/chamados/services/fio.py:845`. | C6, seleção do marco, lastro e validação da resolvedora. `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:1135`; `app/chamados/juizes.py:632`; `app/chamados/services/fio.py:838`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, restauração e persistência em lote; backfill prepara objetos para essa persistência. `app/chamados/veredito_lavrado.py:227`; `app/chamados/veredito_lavrado.py:243`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/management/commands/backfill_veredito_lavrado.py:138`. | `esta_stale`, raio-X e passivo/reversão. `app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:173`; `app/chamados/services/passivo_validado.py:64`. |
| `_pv_memo` | C6 grava; restauração e descarimbo removem. `app/chamados/juizes.py:847`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`. | `pergunta_viva` reutiliza por chave. A chave contém veredito/via da célula, mas omite ata/DNA, lidos na concordância com a luz. A releitura precisa eliminar o memo recebido. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1105`; `app/chamados/juizes.py:1111`. |
| Célula, batidas e tranca consultadas por C6 | Célula tem portas próprias de lavra/regeneração; batida tem escritor canônico próprio. Não pertencem ao escritor do carimbo da pergunta. `app/ponto/portas/celula.py:747`; `app/ponto/portas/celula.py:100`; `CLAUDE.md:141`. | C6 consulta tranca, célula, fato do marco e lastro externo. `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1147`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1214`. |

**Todos os pontos diretos encontrados de chamada do carimbo comum:**

| Chamador | Sítio |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620`. |
| Resolução por chamado, ramo principal e ramo sem credor | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675`. |
| Absorção por ausência | `app/chamados/services/materializacao.py:546`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346`. |

O plantio individual já envolve batida e carimbo em transação e transforma `False` em reprovação para reverter a batida; essa composição deve permanecer. O inverso já relê pergunta/disputa travadas antes de C6. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1201`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:227`.

**Demais consumidores encontrados dos campos, estados e funções afetados:**

| Grupo | Sítios |
|---|---|
| Estado e redução | Propriedades/queryset da pergunta, coleção viva e raio-X. `app/chamados/models.py:2481`; `app/chamados/models.py:2713`; `app/chamados/models.py:2807`; `app/chamados/juizes.py:1225`; `app/chamados/juizes.py:1257`; `app/chamados/services/raiox.py:133`. |
| Reconciliador e seleção de trabalho | `app/chamados/reconciliador.py:366`; `app/chamados/reconciliador.py:583`; `app/chamados/reconciliador.py:1017`; `app/chamados/reconciliador.py:1433`; `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/materializacao.py:604`; `app/chamados/services/materializacao.py:1292`; `app/chamados/services/materializacao.py:1341`; `app/chamados/services/validacao.py:422`. |
| Emissão e fábrica | `app/chamados/services/disputa_emissao.py:695`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/disputa_emissao.py:1152`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:138`. |
| Fio, cobrança e ações | `app/chamados/services/fio.py:336`; `app/chamados/services/fio.py:656`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/acoes_disputa.py:341`; `app/chamados/services/declaracao_texto.py:211`; `app/chamados/services/declaracao_texto.py:366`; `app/chamados/services/declaracao_texto.py:635`. |
| App e integrações | `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/views.py:1257`; `app/chamados/views.py:1280`; `app/chamados/views.py:1416`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176`; `app/api/views.py:1009`; `app/api/views.py:1173`; `app/api/views.py:1187`; `app/api/views.py:2580`; `app/api/views.py:2636`; `app/api/views_mensageria.py:567`; `app/api/foto_recebida.py:48`. |
| Apresentação e relatórios | `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:79`; `app/chamados/services/explicador.py:95`; `app/chamados/canal.py:65`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1599`. |
| Histórico, painel e procedência | `app/chamados/services/historico.py:99`; `app/chamados/services/painel.py:67`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`; `app/chamados/services/verbos.py:311`; `app/ponto/services/procedencia_batida.py:72`. |
| Vigias e passivo | `app/ponto/supra_juiz.py:545`; `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:783`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/furo_sem_canal.py:81`; `app/ponto/services/flip_auto.py:129`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:140`; `app/chamados/services/relavra_via.py:179`. |
| Comandos | `app/ponto/management/commands/reconciliar_grade.py:461`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `app/chamados/management/commands/validar_classe_a.py:80`; `app/chamados/management/commands/auditar_disputas.py:71`; `app/chamados/management/commands/auditar_disputas.py:150`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/chamados/management/commands/dedup_perguntas_fato.py:74`. |
| Auditoria de invariantes | Carimbo sem testemunha, materializada viva, via ausente e validação sem plantio. `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`. |
| Templates | `app/templates/chamados/partials/modal_fio.html:184`; `app/templates/chamados/partials/modal_fio.html:212`; `app/templates/chamados/partials/modal_fio.html:223`; `app/templates/chamados/partials/_card_atendimento.html:94`; `app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_card_atendimento.html:137`; `app/templates/chamados/partials/_card_atendimento.html:155`; `app/templates/chamados/partials/_card_atendimento.html:186`; `app/templates/chamados/partials/_card_atendimento.html:384`; `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:42`. |
| Instrumentos fora de `app` | Censo de folha lê resposta/validação; raspagem reconhece o texto da resposta como dado pessoal. `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85`. |
| Eventos e selos | Fechos de chamado/disputa disparam consumidores; salvamento da pergunta agenda beco em `on_commit`. Selos restringem escritores/chamadores internos e verificam restauração após erro. `app/chamados/signals.py:76`; `app/chamados/signals.py:125`; `app/chamados/signals.py:192`; `app/chamados/signals.py:202`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:374`; `app/chamados/tests/test_o261e_f2_fila.py:179`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`. |

Completude para chamadas dinâmicas, SQL externo e caminhos ausentes da cópia: [nao verificado].

## C DIFF PROPOSTO EM TEXTO

Este é o incremento mínimo proposto na fonte para a candidata autorizada: trava/releitura, descarte do memo e recusa da intenção obsoleta, mantendo o corpo escritor e C6 existentes. **Não basta para declarar toda a concorrência resolvida**, pelos contraexemplos D1–D4. `app/docs/PROMPTS.md:1882`; `app/chamados/models.py:2575`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`.

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/mo

[truncado em 20000 de 27571 caracteres]

## 05/11 18:11 · sessao 18d5620d9ed0 · - · - · origem=Ronald (tmux)

Complemento dos seis campos, por leitura do código; dados/execução em produção **[nao verificado]**.

| Campo de `PerguntaDisputa` | Escritores encontrados |
|---|---|
| `disputa_id` | Nascimento em `PerguntaDisputa.abrir`, que faz `objects.create(disputa=...)`. A identidade existente é devolvida sem trocar sua disputa. `app/chamados/models.py:2465`, `app/chamados/models.py:2464`. |
| `motivo` | Nascimento na mesma porta; participa da busca de identidade `(colaborador, motivo, data_evento)`. `app/chamados/models.py:2465`, `app/chamados/models.py:2381`. |
| `tipo_resposta` | Nascimento pelo `**campos` da mesma porta, fornecido pelos emissores. `app/chamados/models.py:2466`; `app/chamados/services/disputa_emissao.py:393`, `app/chamados/services/disputa_emissao.py:774`, `app/chamados/services/disputa_emissao.py:1199`; `app/chamados/services/acoes_disputa.py:262`; `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:614`. |
| `celula_id` | Nascimento; atualização de identidade existente por `abrir` quando encontra a célula da pessoa/data; exclusão da célula produz `SET_NULL`. `app/chamados/models.py:2466`, `app/chamados/models.py:2419`, `app/chamados/models.py:2425`, `app/chamados/models.py:2463`, `app/chamados/models.py:2198`. |
| `data_evento` | Nascimento; conversa recebe `localdate`; `fio.reapontar_resolvedora` preenche somente quando a pergunta ainda não tem dia, antes do julgamento/lavra. `app/chamados/models.py:2416`, `app/chamados/models.py:2465`; `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:847`. |
| `chamado_destino_id` | Nascimento; reapontamento no dedup de `abrir`; `fio.reapontar_resolvedora`; exclusão do chamado produz `SET_NULL`. `app/chamados/models.py:2466`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`; `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`; `app/chamados/models.py:2772`. |

Não encontrei atualização persistida posterior de `disputa`, `motivo` ou `tipo_resposta` nos fontes de produção pesquisados; completude contra operações dinâmicas externas ou banco **[nao verificado]**. A porta de nascimento encontrada conserva os três quando deduplica e só inclui célula/destino no `update_fields`. `app/chamados/models.py:2422`, `app/chamados/models.py:2425`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`.

A exclusão da disputa remove a pergunta por `CASCADE`, podendo fazer a releitura por PK não encontrar linha; isso precisa ter resultado explícito no diff. `app/chamados/models.py:2200`.

**Nascimento: todos os chamadores encontrados da porta**

- Emissão geral: `app/chamados/services/disputa_emissao.py:389`.
- Pergunta de hora: `app/chamados/services/disputa_emissao.py:770`.
- Âncora sem entrada: `app/chamados/services/disputa_emissao.py:1195`.
- Reabertura administrativa: `app/chamados/services/acoes_disputa.py:259`.
- Declaração pelo texto, dois pontos: `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:614`.

**Transferência e lote**

A migração de resolvedoras leva perguntas entre chamados, pelo mesmo `reapontar_resolvedora`; não troca a FK `disputa`. Dedup compara motivo/data e retrata a excedente. `app/chamados/services/fio.py:937`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`. Os três chamadores de reparo do reconciliador delegam a essa porta, podendo preencher a data quando ausente. `app/chamados/reconciliador.py:922`, `app/chamados/reconciliador.py:1570`, `app/chamados/reconciliador.py:1582`.

O `bulk_update` encontrado de `PerguntaDisputa` escreve os três campos da lavra, não os seis identificadores propostos. `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`.

As atribuições de célula/destino nestes outros pontos são fotografias de julgamento em memória, não escritas dessas FKs no banco: célula do julgamento em cópia; chamado atual na cópia; destino candidato temporário da guarda e restauração. `app/chamados/models.py:1432`, `app/chamados/models.py:1439`, `app/chamados/models.py:350`; `app/chamados/services/fio.py:773`, `app/chamados/services/fio.py:777`.

**Conferência humana da data: campo fora do snapshot**

`conferir_data_do_fato` lê `data_evento` e o dia do chamado, mas grava somente `data_conferida_em`/`data_conferida_por`. O `data_evento` no log é evidência da data aceita, não atribuição ao modelo. `app/chamados/services/validacao.py:611`, `app/chamados/services/validacao.py:617`, `app/chamados/services/validacao.py:619`, `app/chamados/services/validacao.py:624`.

Assim, comparar somente aqueles seis campos não detecta uma conferência humana intercalada; a relevância dessa perda para o ato de carimbar depende do escopo do diff, **[nao verificado]**. A materialização usa essa conferência como liberação da divergência. `app/chamados/services/materializacao.py:1110`, `app/chamados/services/materializacao.py:1135`.

**Leitores/autoridades relevantes dos seis**

| Leitura | Autoridade e sítio |
|---|---|
| Identidade do fato | `PerguntaDisputa.do_fato`, usando colaborador da disputa, motivo e data. `app/chamados/models.py:2370`, `app/chamados/models.py:2381`. |
| C6 e memo | C6 lê célula, data, tipo, destino; a chave não contém `disputa_id` nem `motivo`. `app/chamados/juizes.py:811`, `app/chamados/juizes.py:815`, `app/chamados/juizes.py:817`, `app/chamados/juizes.py:1135`. C6 registrado: `app/core/juizes.py:266`. |
| Prova da célula pertence à pessoa/data | `_celulas_da_pergunta` recusa alimentar célula de outra pessoa/data ou incompleta. `app/chamados/juizes.py:621`, `app/chamados/juizes.py:625`, `app/chamados/juizes.py:627`. |
| Geometria do marco | `motivo` identifica marco; tipo/data/resposta participam do slot. `app/chamados/juizes.py:260`, `app/chamados/juizes.py:288`, `app/chamados/juizes.py:333`, `app/chamados/juizes.py:1177`. |
| Admissibilidade da costura | `resolvedora_valida`, chamada antes do nascimento/reapontamento. `app/chamados/juizes.py:1948`; `app/chamados/models.py:2402`, `app/chamados/models.py:2429`; `app/chamados/services/fio.py:838`. Não achei entrada nominal dela em `core/juizes.py`; registro nominal próprio **[nao verificado]**. |
| Data do fato da pergunta | `fio.data_do_fato_de` devolve data_evento, divergência e dia do chamado; não escolhe silenciosamente. `app/chamados/services/fio.py:40`, `app/chamados/services/fio.py:45`, `app/chamados/services/fio.py:51`, `app/chamados/services/fio.py:87`. Registro nominal próprio em core **[nao verificado]**. |
| Dia do chamado | Registro C5 em `catalogo/modulos.py::data_do_chamado`. `app/core/juizes.py:265`. `data_do_fato_de` consulta `dia_do_fato` para comparação. `app/chamados/services/fio.py:85`. |
| Tipo da resposta e escolha do plantio | Resposta humana valida segundo tipo; materialização escolhe hora/data_hora/escolha. `app/chamados/services/disputa_emissao.py:1257`, `app/chamados/services/disputa_emissao.py:1263`, `app/chamados/services/disputa_emissao.py:1272`; `app/chamados/services/materializacao.py:1143`. |
| Validação humana/implausibilidade | Lê tipo, motivo, destino, data e pessoa da disputa. `app/chamados/juizes.py:1565`, `app/chamados/juizes.py:1573`, `app/chamados/juizes.py:1698`, `app/chamados/juizes.py:1706`; `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:374`. |
| Marco histórico/virada da meia-noite | Cadeia `marcos_turno`; vínculo do dia. `app/chamados/juizes.py:288`, `app/chamados/juizes.py:302`; `app/docs/JUIZES.md:24`; `app/core/juizes.py:1285`; `app/docs/JUIZES.md:98`. |
| Reapontamento e frescor da lavra | Data/destino são insumos sem timestamp próprio; a porta relavra junto. `app/chamados/services/fio.py:798`, `app/chamados/services/fio.py:816`, `app/chamados/services/fio.py:846`. |
| Listagem/crédito | `data_do_fato_de` usa `chamado_que_lista`; vigia e porta alcançável consomem a âncora. `app/chamados/services/fio.py:77`; `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1379`; registro C17 `app/core/juizes.py:277`. |

As portas já são declaradas por dono em `core/portas.py`: nascimento/identidade no modelo; reapontamento em `fio.py`; conferência no dono da validação. Comparar os valores na porta existente não requer criar nova autoridade de vitalidade. `app/core/portas.py:241`, `app/core/portas.py:304`, `app/core/portas.py:362`; C6 já registrado em `app/core/juizes.py:266`.

## 05/11 18:10 · sessao 54bdf96d6ab9 · - · - · origem=Ronald (tmux)

Achados confirmados, sem escrita:

- `.sh` passa apenas por citação de linhas; não há AST nem atribuição de chamador ao símbolo Python. Heredoc com `from m.lib import mudada` produz somente faceta arquivo; `python3 -c 'from m import lib; lib.mudada()'` não produz achado. `bin/quem_chama.py:390`, `bin/quem_chama.py:510`, `bin/quem_chama.py:673`.
- A cura por qualquer componente literal de `modulo_de(rel)` em código shell é conservadora: reconhece import absoluto, `from core import placar_estrutural`, relativo `from . import lib` e módulo simples de `bin/`; nomes coincidentes também podem produzir “não varrido”. O predicado foi exercitado em memória com `_codigo_sh` e `_padrao_nome`. `bin/quem_chama.py:124`, `bin/quem_chama.py:347`, `bin/quem_chama.py:479`.
- Rebaixar `tipo` antes de `_tipo_simbolo` rebaixa arquivo e símbolos; guardar `lib` antes disso preserva achados dos templates. `_faceta` continua como único escritor de contagem/“não varrido” e alimenta o resumo existente. `bin/quem_chama.py:311`, `bin/quem_chama.py:655`, `bin/quem_chama.py:657`, `bin/quem_chama.py:690`, `bin/quem_chama.py:713`.
- Falha adicional confirmada: `{% load m_tags %}` seguido de `{% filter rot %}...{% endfilter %}` encontra a biblioteca, mas perde o uso do filtro porque a regex só aceita `|rot`. Correção mínima: aceitar `(?:\||{%\s*filter\s+)\s*` antes do nome escapado. `bin/quem_chama.py:526`, `bin/quem_chama.py:560`.
- Comentário `{# ... #}` multilinha é removido corretamente pelo motor; a fixture atual cobre comentários de uma linha, deixando a borda multilinha sem RED específico. `bin/quem_chama.py:520`, `bin/tests/test_quem_chama.sh:61`, `bin/tests/test_quem_chama.sh:79`.
- As asserções de símbolo parado prendem apenas o texto de saída, permitindo o mesmo texto na faceta errada; precisam selecionar alvo **e faceta** antes de afirmar presença/ausência. O helper posterior `_sec` também delimita somente o alvo. `bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:210`.
- A fixture já não cria `__init__.py`, mas `lib.py` contém leitura interna de `mudada`, que pode mascarar um teste negativo de zero; o RED shell precisa de alvo isolado sem leitura interna/import local. `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:32`, `bin/tests/test_quem_chama.sh:121`.
- O selo inteiro cria diretório, arquivos e commits temporários; não deve ser executado nesta chamada de leitura estrita. `bin/tests/test_quem_chama.sh:24`, `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:118`.

## 05/11 18:10 · sessao 24d2e776eb62 · - · - · origem=Ronald (tmux)

Censo B e normas, somente por leitura:

- **Contrato do instrumento:** lista determinística de chamadores/leitores a partir do diff; “lista é fato, não juiz”, zero autoridade nova (`bin/quem_chama.py:4-6`). Facetas fora da lista positiva devem declarar “não varrido”, nunca usar ausência de achado como prova de zero (`bin/quem_chama.py:8-15`).
- **Juiz existente:** não encontrei registro específico de quem-chama em `app/core/juizes.py` ou `app/docs/JUIZES.md` [nao verificado quanto à exaustividade]. O registro vivo declara quem decide perguntas do domínio (`app/core/juizes.py:3-8`); JUIZES.md é lookup e remete ao registro vivo (`app/docs/JUIZES.md:7-12`). O próprio contrato exclui juiz novo (`bin/quem_chama.py:6`).
- **Leitor externo direto da fonte:** selo escolhe `QUEM_CHAMA_PY` ou `bin/quem_chama.py` (`bin/tests/test_quem_chama.sh:25`), invoca o programa com range, raiz e saída (`bin/tests/test_quem_chama.sh:148`). O selo é descoberto/executado pelo glob da régua (`bin/regua.sh:138-140`).
- **Sítio fonte de cobertura:** `VARRIDOS` é a declaração positiva (`bin/quem_chama.py:37-55`); `_tipo_simbolo` classifica facetas (`bin/quem_chama.py:311-326`); `quem_chama` grava `info['tipo']` e monta alvo (`bin/quem_chama.py:655-660`); despacho de shell usa somente citações de linhas, sem AST do payload (`bin/quem_chama.py:673-674`).
- **Único escritor de apresentação/contadores:** `_faceta` decide N versus “não varrido”, escreve `varridas`, `zeros` e `nao`, depois imprime achados (`bin/quem_chama.py:690-699`). `render` lê os tipos de arquivo e símbolo e chama essa mesma função (`bin/quem_chama.py:713-715`), produz resumo (`bin/quem_chama.py:717-721`). `main` lê o total de `conta['nao']` no resumo de gravação (`bin/quem_chama.py:747-748`). A cura deve alimentar esse escritor, sem apresentação paralela.
- **Leitores/dados próximos:** `Alvos` indexa biblioteca/modulo e citações (`bin/quem_chama.py:363-382`); `varrer_py` interpreta importações e símbolos (`bin/quem_chama.py:390-439`); `varrer_template` lê registros para filtro/tag (`bin/quem_chama.py:553-565`). Não alterar metadados `membro`, `registro`, `nome` para mascarar cobertura: são insumos dessas varreduras (`bin/quem_chama.py:411-414`, `bin/quem_chama.py:426-438`, `bin/quem_chama.py:555-560`).
- **Fragilidade de faceta já publicada:** asserções do selo buscam linha global e aceitam “importa de m.lib” sem vincular ao bloco da faceta (`bin/tests/test_quem_chama.sh:159-160`); caso relativo repete abordagem (`bin/tests/test_quem_chama.sh:166-168`). A reprovação fornecida exige prender a faceta [nao verificado em arquivo local do anexo].
- **Normas:** regra antes do código/L-110 (`CLAUDE.md:45-51`), zero de produção implica apagar pela L-111 (`CLAUDE.md:188`), censo de escritores antes da cura (`CLAUDE.md:193`), selo sobre saída exige MORDE (`CLAUDE.md:191`), escopo inclui escritores/leitores mesmo fora do diff (`CLAUDE.md:371-373`), instrumento usa `padrao=n/a (instrumento)` (`CLAUDE.md:65-70`).
- **Limites de autorização:** B3 suspende obra após reprovação final e exige pergunta de desenho (`CLAUDE.md:271-272`); contrato de instrumento tem cinco linhas (`CLAUDE.md:386`). Teto específico de duas voltas e proibição de varrer Python em `.sh` vêm do anexo MANHA-0511 fornecido pelo usuário [nao verificado em arquivo local]. Não confundir com teto local de dois **construtores**, outra unidade (`CLAUDE.md:288`).
- **Limites de execução:** programa lê ponta/base por Git (`bin/quem_chama.py:17-19`, `bin/quem_chama.py:619-625`); esta cópia não tem `.git`, conforme erro observado, sem arquivo:linha [nao verificado]. Executar o selo violaria “nenhum arquivo”: cria temporário e remove no trap (`bin/tests/test_quem_chama.sh:24`), cria fixture e Git (`bin/tests/test_quem_chama.sh:28-29`). `main --saida/--obra` também cria diretórios e grava (`bin/quem_chama.py:738-743`).

## 05/11 18:09 · sessao d4feb77f32e6 · - · - · origem=Ronald (tmux)

A cobertura deve usar C6 (`pergunta_viva`) para a morte da pergunta, C15 para o conjunto e C17 para distinguir fio alcançável/ação administrativa; C11 registrado continua sendo ausência. (`app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`)

| Ato | Fonte e risco do diff |
|---|---|
| Reabrir | Depois de `_gravar_estado` e `ressuscitar_perguntas`; esta última pode devolver zero e não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1482`) |
| Retratar duplicata | Evento depois da porta `carimbar_identidade`; não presumir que duplicata respondida morreu, porque C6 avalia resposta antes da identidade. (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`) |
| Validação | Observar o desfecho da materialização, nunca o carimbo intermediário. O `atomic` existe justamente para impedir morte temporária observada por sinais; o laço final atualmente fica fora desse bloco. (`app/chamados/services/validacao.py:387`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:430`) |
| Reapontar | Capturar a âncora anterior e emitir sobre origem e destino; hoje a porta trava e protege apenas o destino. Âncora é `chamado_destino` ou pai da disputa. (`app/chamados/services/fio.py:116`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`) |
| Morte por leitura | C6 já responde, mas morte pode decorrer de relógio sem `save`; sobrepor `save()` não basta. O caso está expressamente documentado no modelo. (`app/chamados/juizes.py:1197`; `app/chamados/juizes.py:1214`; `app/chamados/juizes.py:660`; `app/chamados/models.py:2346`) |

O menor encadeamento defensável reutiliza `emitir_evento_celula`, consumidor existente, portas de reata, `reconciliar()` e fábrica. O INSERT permanece único e deve compartilhar a transação do ato; o consumidor relê o estado atual. (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:672`; `app/chamados/reconciliador.py:1283`; `app/chamados/models.py:607`)

Entretanto, **chamar apenas a fábrica nos seis eventos não é uma cura integral**: `motivos_a_perguntar` exclui identidades históricas sem consultar C6; a pergunta humana decidida não deve ganhar irmã nem ter validação apagada. A saída legítima deve ser comprovada pela porta administrativa C17, depois de tentar a emissão devida — usá-la antes da emissão faria todo chamado novo em fila parecer “já alcançável” e dispensaria a pergunta. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1375`)

A lavra temporal existente cobre parte da morte por relógio, sem juiz novo: `lavrar_perguntas_stale` chama `lavrar`, agendado a cada cinco minutos. Seu universo atual exclui perguntas sem destino explícito, mesmo quando o pai está vivo; ampliar esse leitor pela âncora existente é necessário para declarar cobertura dessa classe. O teto de 500 também impede prometer processamento imediato. (`app/chamados/management/commands/lavrar_perguntas_stale.py:35`; `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/config/crons.py:390`; `app/chamados/services/fio.py:116`)

Sobrepor `save()` dos dois modelos é maior que tocar as fontes e ainda deixa `QuerySet.update` descoberto; ressuscitação também ocorre diretamente pelo cartório. Comparar timestamps de lavra produziria eventos continuamente; comparar atributos fora de `update_fields` notificaria mudanças não persistidas. (`app/chamados/models.py:2652`; `app/chamados/models.py:1451`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/veredito_lavrado.py:231`)

Se o diff tocar `EventoCelula`, os leitores adicionais são os vigias, métricas, semeadura e consumidor: `app/chamados/reconciliador.py:1163`, `:1214`, `:1222`, `:1229`, `:1241`, `:1263`, `:1276`, `:1309`, `:1338`, `:1353`; acionadores em `app/chamados/management/commands/consumir_eventos_celula.py:31` e `app/chamados/management/commands/reconciliar_chamados.py:62`.

A distribuição dessas classes nos 494 e a suficiência de qualquer diff sobre eles permanecem **[nao verificado]**.

## 05/11 18:08 · sessao 43bcd2a988ec · - · - · origem=Ronald (tmux)

Pontos críticos para C/D:

1. **Reler e chamar C6 não basta.** `resposta='validar'` preenche `validada_em` usando o texto que estiver na instância; C6 retorna morto imediatamente pela validação. Se a releitura trouxer uma resposta nova, a porta pode validar uma fala que o ato anterior não julgou. Precisa comparar os insumos da instância recebida com a linha travada antes de aplicar a candidatura. `app/chamados/models.py:2576`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1136`.

2. **Preservar identidade humana.** Aplicar o candidato automático antes de julgar troca `admin_decidiu` por outra via; a célula administrativa continua permitindo C6 morto. A fonte única das vias de identidade já existe; recusar sobreposição não precisa de juiz novo. `app/chamados/models.py:2545`; `app/chamados/models.py:2535`; `app/chamados/juizes.py:1147`; `app/chamados/services/relavra_via.py:161`.

3. **Forma curta da alteração na fonte:** inserir transação dentro de `carimbar`; fotografar campos do ato recebido; reler com `select_for_update(of=('self', 'disputa')).select_related('disputa', 'celula')`, equivalente à superfície de trava do caminho inverso; invalidar memo; recusar comando obsoleto ou sobreposição de identidade; aplicar o corpo canônico e consultar C6. `app/chamados/models.py:2551`; `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`.

Trecho textual de releitura possível, com compatibilidade de execução **[nao verificado]**; o projeto fixa Django 5.2.14. `app/requirements.txt:13`.

```diff
@@
-        quando = quando or _tz.now()
+        campos_do_ato = (
+            'disputa_id', 'celula_id', 'motivo', 'data_evento',
+            'tipo_resposta', 'chamado_destino_id', 'resposta_colab',
+            'respondida_em', 'validada_em', 'validada_por_id',
+            'materializada_em', 'materializada_por_id', 'via_resolucao',
+        )
+        recebida = tuple(getattr(self, campo) for campo in campos_do_ato)
+        # Dentro do atomic que envolve também julgamento e save:
+        self.refresh_from_db(
+            from_queryset=type(self).objects
+                .select_for_update(of=('self', 'disputa'))
+                .select_related('disputa', 'celula'))
+        self.__dict__.pop('_pv_memo', None)
+        if recebida != tuple(getattr(self, campo) for campo in campos_do_ato):
+            return False
+        if (self.materializada_em is not None
+                and self.via_resolucao in self.vias_de_identidade()):
+            return False
+        quando = quando or _tz.now()
```

O trecho acima é **parte** do diff: o `atomic` deve envolver até a escrita, e sua saída deve ficar dentro do `try` que restaura carimbo/lavra. Apenas adicionar `@transaction.atomic` cria um contraexemplo novo: erro no COMMIT acontece fora dos `except` atuais e pode deixar carimbo em memória após rollback. `app/chamados/models.py:2584`; `app/chamados/models.py:2594`; `app/chamados/models.py:2600`.

Não comparar todos os `concrete_fields`: lavra e recusa podem mudar durante o próprio plantio sem mudar a autorização sobre a resposta; a porta deve fotografar os insumos do ato, mantendo a lavra como resultado derivado. Os escritores desses resultados estão em `app/chamados/veredito_lavrado.py:211`; `app/chamados/services/materializacao.py:817`.

4. **Memo:** a chave cobre validação, resposta, via, materialização, data, tipo, célula/veredito e destino/status; omite motivo, ata/DNA/trabalha, tranca e instante. A limpeza no PEP impede reutilizar o resultado anterior nesse ato, mas não corrige globalmente a completude da chave. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:1111`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1194`.

5. **Contraexemplo residual decisivo — ordem inversa.** Mesmo o PEP acima não fecha sozinho todas as intercalações:
   - T1 trava pergunta e prepara validação/carimbo.
   - T2, com instância antiga sem validação/carimbo, prepara resposta nova; seu UPDATE espera T1.
   - T1 grava e libera.
   - T2 salva texto/instante novos, mas não limpa a validação recém gravada: a seleção de campos ocorreu sobre a instância antiga.

Essa sequência é possível pela forma do escritor de resposta; ocorrência em produção **[nao verificado]**. Para cobrir as duas ordens, o escritor canônico da resposta precisa reler sob trava antes de decidir quais campos limpar — continua sendo o mesmo escritor. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1285`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1300`.

Também `validar_pergunta` busca e testa resposta antes do `atomic`, permitindo julgamento sobre fotografia anterior à fala gravada. `app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:363`; `app/chamados/services/validacao.py:395`.

6. **Ordem de trava não demonstrada:** o inverso declara `of=('self', 'disputa')`; isso não comprova por leitura a ordem efetiva de aquisição nem ausência de ciclo com chamadores que já escreveram chamado, disputa ou batida. Há comentário explícito exigindo chamado antes de pergunta, e `_materializar_uma` planta batida antes de entrar em `carimbar`. Medição exigida pelo aval permanece **[nao verificado]**. `app/chamados/services/relavra_via.py:221`; `app/chamados/models.py:263`; `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`.

7. **Rollback externo permanece fora do alcance isolado de `carimbar`:** o chamador pode abrir uma transação externa e revertê-la depois do retorno positivo; a instância permanece em memória. O materializador efetivamente mantém uma transação externa envolvendo plantio e carimbo. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1201`.

## 05/11 18:08 · sessao de53790151bb · FUNIL-28 · censo · origem=Code

Consolidação pelo conteúdo das alegações, sem julgamento de procedência. Nas fusões, competências e coberturas foram reunidas; dinheiro=sim quando alguma origem assim o marcou. Classes originais aparecem entre parênteses. [nao verificado: critérios da consolidação]

Referências de LA permanecem relativas a `app/`, salvo `bin/`; `B:n` significa `app/docs/BACKLOG.md:n`. As referências foram conservadas conforme fornecidas. [nao verificado: convenções dos anexos]

**PARTE 1/3**

| id final | classe | arquivo:linha | afirmacao (uma frase) | dinheiro | competencia | obra/BACKLOG que cobre ou nenhuma | ids de origem | respostas de origem |
|---|---|---|---|---|---|---|---|---|
| F-001 | divergencia-texto-codigo (b) | docs/LEIS.md:81; folha/export.py:1147; folha/export.py:1235 | A coluna antiga descreve retenção universal de furo, enquanto o retentor isenta furo exclusivamente de pausa. | sim | n/a | O236 — B:1164 | LA-01 | lote1.r:01; lote2.r:28; lote6.r:01 |
| F-002 | divergencia-texto-codigo (b) | docs/LEIS.md:93; ponto/portas/celula.py:196; ponto/portas/regen_exportada.py:174 | A proibição absoluta de corrigir exportada diverge da porta de retificação com motivo e abertura da guarda. | sim | n/a | O121 — B:1189 | LA-02 | lote1.r:03; lote4.r:11; lote6.r:04 |
| F-003 | divergencia-texto-codigo (b) | docs/LEIS.md:113; folha/views.py:208; ponto/services/fechamento.py:277 | A geração do TXT alcança o motor por recálculo em leitura, apesar da promessa de ler somente a lavratura. | sim | n/a | O-DIA-PAGO S4 — B:1095 | LA-03 | lote1.r:04 |
| F-004 | divergencia-texto-codigo (b) | docs/LEIS.md:115; colaboradores/models.py:115; folha/porta_export.py:230; folha/porta_export.py:690 | A trava de HE considera apenas dias acima do limite sem decisão e depende da configuração da empresa, contrariando as formulações universais. | sim | n/a | O214, item 3 — B:1134 | LA-04 | lote1.r:05; lote7.r:10 |
| F-005 | divergencia-texto-codigo (b) | docs/LEIS.md:149; ponto/portas/celula.py:198; ponto/portas/celula.py:238 | A regeneração ordinária continua bloqueando competências exportadas sem `apesar_da_lavra`, apesar da revogação da trava. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO — B:1008 | LA-05 | lote1.r:07; lote4.r:12; lote6.r:05 |
| F-006 | lacuna-de-teste-ou-selo (b) | docs/LEIS.md:149; folha/management/commands/regerar_txt_dominio.py:118 | Os artefatos do regerador não demonstram publicação do DIFF antes do ato de frota. | sim | n/a | nenhuma | LA-06 | lote1.r:08 |
| F-007 | divergencia-texto-codigo (d) | folha/models.py:97; folha/models.py:134; folha/models.py:137 | `invalidada_em` nulo não garante vigência, porque ela também depende de ser a geração mais recente. | sim | n/a | nenhuma | LA-07 | lote1.r:09; lote2.r:19 |
| F-008 | divergencia-texto-codigo (a) | folha/views.py:213; folha/views.py:247; folha/views.py:268 | Entrar no TXT não garante aprovação, porque o carimbo exclui itens com determinadas travas. | sim | n/a | FECH-05 — B:2069 | LA-08 | lote1.r:10; lote7.r:03 |
| F-009 | divergencia-texto-codigo (e) | folha/tests/test_export_59a.py:1; folha/export.py:411 | A docstring proíbe universalmente parcela de feriado em 12x36, mas o emissor permite a parcela com cláusula de dobra. | sim | n/a | nenhuma | LA-09 | lote1.r:11 |
| F-010 | divergencia-texto-codigo (a) | ponto/services/fechamento.py:476; ponto/views.py:1248; ponto/services/fechamento.py:552 | As promessas de recalcular qualquer exportada omitem a exclusão específica de setembro de 2026. | sim | 09/2026 | nenhuma | LA-10 | lote1.r:18; lote7.r:02 |
| F-011 | divergencia-texto-codigo (a) | ponto/services/fechamento.py:1441; folha/porta_export.py:417; docs/LEIS.md:125; ponto/services/fechamento.py:1144 | As promessas de congelamento de competência trancada, paga ou exportada divergem da escrita que pode atualizar valores fora da guarda de setembro. | sim | 10/2026; guarda de 09/2026 | QUEM-ESCREVE-NO-GRAVADO-DA-EXPORTADA — B:1091 | LA-11 | lote1.r:19; lote2.r:20; lote5.r:B1 |
| F-012 | divergencia-texto-codigo (a) | ponto/services/fechamento.py:1182; colaboradores/services/calendario.py:290; ponto/services/dia_pago.py:852 | O comentário de que ninguém lê `DiaPago` ficou antigo diante dos leitores existentes. | nao | n/a | O-DIA-PAGO S3 — B:1090 | LA-12 | lote1.r:24 |
| F-013 | risco-idempotencia (LA:a; LB:idempotencia) | holerite/models.py:138; holerite/views.py:1191; holerite/views.py:1193; app/holerite/views.py:1174; app/holerite/views.py:1198 | O upload avulso apaga o holerite anterior sem ciência apesar da promessa de preservar versões, e sua repetição substitui novamente o documento e cria outra PK. | nao | n/a | nenhuma | LA-13; LB-103 | lote1.r:25; B06 |
| F-014 | divergencia-texto-codigo (d) | holerite/models.py:123; holerite/views.py:1202 | O upload avulso grava a empresa atual do colaborador apesar do help definir a empresa impressa no documento. | nao | n/a | nenhuma | LA-14 | lote1.r:26 |
| F-015 | risco-idempotencia (a) | holerite/models.py:143; holerite/motor.py:448; holerite/motor.py:480 | A primeira publicação conjunta deixa `anexos_sha` vazio, permitindo anexar novamente a mesma carta na republicação. | nao | n/a | nenhuma | LA-15 | lote1.r:29 |
| F-016 | documentacao (a) | holerite/motor.py:180; holerite/motor.py:186 | As afirmações históricas de recibos sem CPF e correspondência 29/29 carecem dos documentos e do cadastro usados na comparação. | nao | n/a | nenhuma | LA-16 | lote1.r:32 |
| F-017 | divergencia-texto-codigo (d) | ponto/models.py:757; ponto/models.py:875; ponto/models.py:883 | O help fixa falta inteira em 720 minutos, mas o preenchimento automático soma o previsto diário e preserva minutos positivos informados. | sim | n/a | nenhuma | LA-17 | lote1.r:33; lote2.r:18; lote3.r:15; lote7.r:05 |
| F-018 | divergencia-texto-codigo (a) | ponto/motor_calculo_v2.py:53; ponto/motor_calculo_v2.py:2845; ponto/motor_calculo_v2.py:2851 | A promessa de vinte minutos diários sem HE diverge do ramo que elimina excedentes somente até dez minutos. | sim | n/a | nenhuma | LA-18 | lote2.r:01 |
| F-019 | divergencia-texto-codigo (a) | ponto/motor_calculo_v2.py:2104; ponto/motor_calculo_v2.py:2122; ponto/motor_calculo_v2.py:2148 | O comentário exclui genericamente atraso após três horas, mas o código restringe essa exclusão ao dia sem marcos de intervalo. | sim | n/a | nenhuma | LA-19 | lote2.r:02 |
| F-020 | divergencia-texto-codigo (a) | ponto/motor_calculo_v2.py:2807; ponto/motor_calculo_v2.py:934; docs/LEIS.md:118 | O comentário e a cláusula original excluem dobra em feriado previsto, enquanto o helper decide pela política do ciclo sem exigir dia não previsto. | sim | n/a | nenhuma | LA-20 | lote2.r:03,31 |
| F-021 | defeito (LA:c; LC:lacuna) | ponto/motor_calculo_v2.py:3226; ponto/motor_calculo_v2.py:3232; escala/models.py:19; app/escala/models.py:17; app/ponto/calculador/periodo.py:334; app/ponto/calculador/regras.py:764 | O piso de pausa de quinze minutos entre quatro e seis horas não atravessa os portões de indenização do comercial e do calculador. | sim | n/a | O270 — B:1511; O281 — B:1523, parcial | LA-21; LC-11 | lote2.r:04; C12 |
| F-022 | divergencia-texto-codigo (a) | ponto/calculador/alimentacao.py:333; ponto/calculador/nucleo.py:38; ponto/calculador/nucleo.py:186 | A narrativa atribui deduplicação de dez minutos ao núcleo que registra sua retirada e percorre todas as batidas. | sim | n/a | nenhuma | LA-22 | lote2.r:11 |
| F-023 | divergencia-texto-codigo (e) | ponto/tests/test_calculador_portoes_de_marco.py:8; ponto/calculador/regras.py:251; escala/regua_defesa.py:472 | A docstring descreve perdão da entrada por proximidade da volta, mas o código consulta o marco ocupado e sua conformidade. | sim | n/a | nenhuma | LA-23 | lote2.r:16 |
| F-024 | divergencia-texto-codigo (LA:b; LB:implementacao) | docs/LEIS.md:79; ponto/motor_calculo_v2.py:822; ponto/motor_calculo_v2.py:2784; app/ponto/juiz_batida.py:310; app/ponto/motor_calculo_v2.py:822; app/ponto/motor_calculo_v2.py:2773 | A cláusula original atribui a indenização a `intervalo_indenizavel`, mas o motor mantém a indenização do suprimido sem consultar a flag, pendência declarada no código. | sim | n/a | nenhuma | LA-24; LB-29 | lote2.r:26; B02 |
| F-025 | divergencia-texto-codigo (LA:b; LB:implementacao) | docs/LEIS.md:80; ponto/motor_calculo_v2.py:2773; ponto/motor_calculo_v2.py:2784; app/ponto/juiz_batida.py:294; app/ponto/motor_calculo_v2.py:2774 | A afirmação de que o intervalo cadastrado sempre sai da jornada diverge do desconto somente do intervalo real reconhecido, preservando trabalho sem pausa com indenização separada. | sim | n/a | nenhuma | LA-25; LB-30 | lote2.r:27; B02 |
| F-026 | divergencia-texto-codigo (LA:b; LB:implementacao; LC:divergência) | docs/LEIS.md:83; core/regua_cct.py:453; ponto/motor_calculo_v2.py:758; app/core/regua_cct.py:449; app/ponto/motor_calculo_v2.py:758 | Sem aplicação CCT vigente, o switch e o parâmetro indefinido podem alterar a prorrogação pós-05h da mesma empresa 12x36 não CLT por regra derivada do ciclo, contrariando o “somente” documental. | sim | n/a | nenhuma | LA-26; LB-31; LC-12 | lote2.r:29; B02; C12 |
| F-027 | documentacao (b) | docs/LEIS.md:84; core/regua_cct.py:150 | O valor falso da constante nesta cópia não demonstra sua configuração efetiva na implantação de produção. | sim | n/a | nenhuma | LA-27 | lote2.r:30 |
| F-028 | divergencia-texto-codigo (LA:b; LB:implementacao; LC:lacuna) | docs/LEIS.md:119; ponto/motor_calculo_v2.py:573; ponto/turnos.py:1380; app/ponto/motor_calculo_v2.py:559; app/ponto/motor_calculo_v2.py:573; app/ponto/turnos.py:1380 | A cobertura autoritativa incompleta para turno partido, célula ausente ou ata aberta ou desalinhada permite fallback pelo tipo gravado da batida, contrariando sua exclusão universal. | sim | n/a | nenhuma | LA-28; LB-41; LC-109 | lote2.r:32; B02; C19 |
| F-029 | divergencia-texto-codigo (LA:b; LB:implementacao) | docs/LEIS.md:124; escala/models.py:174; ponto/motor_calculo_v2.py:3635; ponto/motor_calculo_v2.py:3668; app/ponto/motor_calculo_v2.py:3635; app/ponto/motor_calculo_v2.py:3668 | As descrições da seleção do motor divergem do despacho por `tipo_base`, que corrige pelo desenho somente determinados templates rotulados como turno partido. | sim | n/a | O136 — B:1346 | LA-29; LB-44 | lote2.r:33; lote4.r:03,13; B02 |
| F-030 | divergencia-texto-codigo (LA:b; LB:implementacao) | docs/LEIS.md:113; ponto/services/fechamento.py:1211; ponto/services/dia_pago.py:357; app/ponto/services/dia_pago.py:333; app/ponto/services/dia_pago.py:357 | Lavraturas sucessivas pela chave `versao`, inclusive `versao='motor'`, apagam e recriam linhas sem preservar revisões anteriores e DIFF por rubrica, apesar da promessa de preservar o passado em nova versão. | sim | n/a | nenhuma | LA-30; LB-45 | lote2.r:34; B02 |
| F-031 | divergencia-texto-codigo (b) | docs/LEIS.md:170; ponto/services/fechamento.py:1214; ponto/services/dia_pago.py:345 | A nota de que ninguém preenche `versao_motor` diverge do fechamento que fornece o carimbo e do escritor que o utiliza. | nao | n/a | nenhuma | LA-31 | lote2.r:35 |
| F-032 | defeito (b) | docs/LEIS.md:111; ponto/motor_calculo_v2.py:1194; ponto/motor_calculo_v2.py:1196 | O arredondamento separado de atraso e antecipada pode produzir soma ligeiramente superior ao teto aritmético. | sim | n/a | nenhuma | LA-32 | lote2.r:36 |
| F-033 | divergencia-texto-codigo (LA:b; LB:implementacao) | docs/LEIS.md:121; escala/utils.py:1231; ferias/models.py:144; ferias/services.py:246; app/ferias/models.py:144 (movida); app/ferias/models.py:54 | A promessa de ausência sem efeito nem consumo em folga diverge do overlay e da contagem aquisitiva de faltas aprovadas sem filtro de jornada, que pode reduzir o direito de férias. | sim | n/a | nenhuma | LA-33; LB-42 | lote3.r:01; B02 |
| F-034 | apresentacao (LA:b; LB:implementacao) | docs/LEIS.md:87; relatorios/pdf_espelho.py:461; relatorios/cartao_pela_celula.py:322; app/relatorios/pdf_espelho.py:461; app/relatorios/pdf_espelho.py:471; app/relatorios/cartao_pela_celula.py:321 | O PDF pode apresentar como “Faltas” dias sem decisão ao preservar o total do motor quando período livre ou fonte indisponível impede a sobrescrita pelos totais da folha. | sim | n/a | nenhuma | LA-34; LB-32 | lote3.r:02; B02 |
| F-035 | documentacao (a) | ponto/precedencia.py:519; ponto/precedencia.py:520; ponto/precedencia.py:536 | O código atual não comprova o censo histórico de dez faltas aprovadas fora da jornada em setembro e zero em outubro. | sim | 09/2026 e 10/2026 | O198 — B:1409 | LA-35 | lote3.r:06 |
| F-036 | divergencia-texto-codigo (e) | ferias/test_services_leitura.py:2; ferias/services.py:118; ferias/models.py:257 | Serviço e property de vencimento usam status gravado e derivado diferentes, contrariando a concordância “SEMPRE”. | nao | n/a | nenhuma | LA-36 | lote3.r:07 |
| F-037 | divergencia-texto-codigo (d) | ferias/models.py:122; ferias/services.py:473 | O limite anunciado como doze meses após o fim é calculado somando 365 dias. | nao | n/a | nenhuma | LA-37 | lote3.r:08 |
| F-038 | divergencia-texto-codigo (d) | ferias/models.py:307; ferias/services.py:314; ferias/services.py:323 | `aviso_emitido` é calculado pela antecedência do agendamento sem informação de emissão participar da decisão. | nao | n/a | nenhuma | LA-38 | lote3.r:09 |
| F-039 | divergencia-texto-codigo (d) | ponto/models.py:584; ponto/services/ausencia.py:333; ponto/services/ausencia.py:779 | A exigência documental anunciada como recusa permite criar ausência aguardando documento. | sim | n/a | nenhuma | LA-39 | lote3.r:12; lote7.r:06 |
| F-040 | divergencia-texto-codigo (d) | ponto/models.py:586; api/views.py:683; api/views_core.py:680; ponto/services/aviso_da_batida.py:79 | `bloqueia_ponto` anuncia impedimento, mas o fluxo pode registrar a batida e depois avisar sobre a ausência. | sim | n/a | nenhuma | LA-40 | lote3.r:13; lote7.r:07 |
| F-041 | divergencia-texto-codigo (d) | ponto/models.py:588; ponto/catalogo/ausencias.py:358; ponto/turnos.py:2598 | A flag parcial permite lançamento por horas, enquanto a cobertura parcial depende também de um único dia e minutos estritamente entre zero e 480. | sim | n/a | nenhuma | LA-41 | lote3.r:14 |
| F-042 | divergencia-texto-codigo (LA:a; LB:implementacao; LC:risco) | api/views.py:815; chamados/tests/test_invariantes_disputa.py:193; docs/LEIS.md:78; api/views.py:423; api/views.py:466; app/ponto/registro_batida.py:145; app/api/views_core.py:402; app/api/views_core.py:435; app/api/views_core.py:454; app/ponto/registro_batida.py:21; app/ponto/registro_batida.py:138; app/ponto/services/triagem_batida.py:668; app/api/views.py:415; app/api/views.py:449; app/api/views.py:462 | As promessas de batida nunca recusada ou apenas avisada divergem das recusas por férias, situação cadastral, GPS ausente e proximidade temporal, que pode alcançar marcação oposta distinta. | sim | n/a | nenhuma | LA-42; LB-28; LC-38; LC-39 | lote3.r:16; lote5.r:E1; lote7.r:08; B02; C14 |
| F-043 | divergencia-texto-codigo (e) | ponto/tests/test_atestados_lote.py:11; ponto/services/atestados_lote.py:60; ponto/services/atestados_lote.py:77 | A promessa de que documento faltante nunca some por filtro diverge da filtragem anterior por tipo e status. | nao | n/a | nenhuma | LA-43 | lote3.r:19 |
| F-044 | divergencia-texto-codigo (a) | chamados/services/materializacao.py:61; chamados/services/materializacao.py:78; ponto/turnos.py:2555 | O comentário conserva o critério antigo de intervalo aberto por bloqueio de ponto, enquanto a consulta usa o conjunto atual de tipos indeterminados. | sim | n/a | nenhuma | LA-44 | lote3.r:25 |
| F-045 | divergencia-texto-codigo (a) | ponto/precedencia.py:13; ponto/precedencia.py:376; escala/models.py:1392 | A exceção textual de feriado exige `trabalha_em_feriado=True`, mas a implementação também considera o ciclo. | sim | n/a | nenhuma | LA-45 | lote3.r:26 |
| F-046 | divergencia-texto-codigo (a) | chamados/juizes.py:414 (movida); chamados/juizes.py:426 (movida) | A descrição de dia sem trabalho omite a exceção da cobertura parcial que preserva a cobrança do restante. | sim | n/a | nenhuma | LA-46 | lote3.r:27 |
| F-047 | divergencia-texto-codigo (d) | escala/models.py:191; core/regua_cct.py:458; ponto/motor_calculo_v2.py:3671 | O campo de perfil promete regras e padrão, embora o caminho atual resolva parâmetros pela régua sem consultar essa FK. | sim | n/a | nenhuma | LA-47 | lote4.r:04 |
| F-048 | divergencia-texto-codigo (d) | escala/models.py:1699; ponto/management/commands/gerar_celulas.py:16; ponto/management/commands/gerar_celulas.py:96 | O help anuncia DNA versão 1, enquanto o produtor grava versão 2. | nao | n/a | nenhuma | LA-48 | lote4.r:05; lote6.r:07 |
| F-049 | defeito (LA:b; LB:implementacao) | docs/LEIS.md:71; ponto/portas/celula.py:233; ponto/portas/celula.py:259; app/ponto/portas/celula.py:225; app/ponto/portas/celula.py:233 | A regeneração fora da vigência ou além do fim do vínculo grava trabalho e DNA sem atualizar DNA anterior, instante e contador de regeneração. | sim | n/a | nenhuma | LA-49; LB-24 | lote4.r:10; lote6.r:03; B02; B03 |
| F-050 | defeito (a) | ponto/portas/celula.py:349; ponto/portas/celula.py:375; ponto/portas/celula.py:397 | A falha capturada no rejulgamento da ata não impede que o fluxo seguinte recalcule o dinheiro. | sim | n/a | nenhuma | LA-50 | lote4.r:14 |
| F-051 | divergencia-texto-codigo (e) | escala/tests/test_f4_template_sem_fase.py:4; escala/models.py:851; escala/tests/test_f4_template_sem_fase.py:43 | A docstring promete `None` para 6x1 sem folga, mas a implementação e a expectativa atual retornam trabalho. | sim | n/a | nenhuma | LA-51 | lote4.r:19 |
| F-052 | divergencia-texto-codigo (e) | escala/tests/test_furos_diarios.py:62; escala/services/furos_diarios.py:54; ponto/portas/celula.py:872 | A promessa de nunca gerar `FuroDiario` para quem nunca bateu diverge da apuração de marcos faltantes sem exigir histórico de batidas. | sim | n/a | nenhuma | LA-52 | lote4.r:20 |
| F-053 | documentacao (a) | escala/servico_jornada.py:129; escala/servico_jornada.py:131; escala/servico_jornada.py:144 | O helper atual não comprova os totais históricos de 37 templates e 761 dias citados na docstring. | sim | 09/2026 | nenhuma | LA-53 | lote4.r:30 |
| F-054 | divergencia-texto-codigo (a) | chamados/services/validacao.py:720; chamados/services/validacao.py:730; chamados/services/validacao.py:873 | O comentário anuncia quatro classes, mas a fila separa cinco grupos. | nao | n/a | nenhuma | LA-54 | lote5.r:A2 |
| F-055 | defeito (a) | chamados/juizes.py:1151 (movida); chamados/juizes.py:1162 (movida) | A resposta pode deixar de manter a pergunta viva sem decisão administrativa quando concorda com a luz e o dia não acusa. | sim | n/a | PERGUNTA-RESPONDIDA-E-MARCO-ACESO — B:1634 | LA-55 | lote5.r:A3 |
| F-056 | documentacao (a) | chamados/catalogo/motor.py:84; chamados/services/arquivo.py:118 | O prazo de silêncio de trinta dias não comprova a afirmação de que a folha já fechou. | sim | n/a | nenhuma | LA-56 | lote5.r:A8 |
| F-057 | divergencia-texto-codigo (LA:b; LB:documentacao) | docs/LEIS.md:189; chamados/services/fabrica_da_celula.py:126; chamados/services/fabrica_da_celula.py:185; chamados/services/cobrar_dia.py:39; chamados/juizes.py:954 (movida); app/docs/LEIS.md:91; app/docs/LEIS.md:189 | Os diagnósticos documentais de L-050 sobre recusa de emissão em exportada e de cobrança na tranca contradizem a reauditoria e os caminhos atuais, nos quais o desvio para pauta se restringe ao limbo e o toque humano pode retirar o silêncio do dia. | sim | n/a | O44 — B:1286 | LA-57; LA-58; LB-35 | lote5.r:B2; lote5.r:B3; B02 |
| F-058 | divergencia-texto-codigo (b) | docs/LEIS.md:92; chamados/services/competencia_trancada.py:125; chamados/services/competencia_trancada.py:181 | A coluna de implementação ainda atribui a `encerrar` escrita da via nas perguntas, embora o fluxo atual só as conte por leitura. | nao | n/a | O44 — B:1286 | LA-59

[truncado em 20000 de 102566 caracteres]

## 05/11 18:08 · sessao bc6b9b78e33e · - · - · origem=Ronald (tmux)

Abertura e menor desenho viável por evento:

- `abrir` já serializa por colaborador e escreve sob `atomic`; seu evento explícito abrange somente `MODULOS_DE_TURNO`, sem `batida_ausente`. `criar` cria sem esse evento e sem `atomic` envolvendo criação+evento. São duas portas obrigatórias do nascimento. (`app/chamados/models.py:863`, `app/chamados/models.py:932`, `app/chamados/models.py:938`, `app/chamados/models.py:1048`; `app/chamados/reconciliador.py:336`)
- Não chamar fábrica dentro da trava de `abrir`: inverteria `Disputa → Colaborador`, a ordem documentada; a fábrica trava disputa. Publicar evento no mesmo `atomic` e consumir fora dessa transação preserva essa ordem. (`app/chamados/models.py:843`, `app/chamados/models.py:866`; `app/chamados/services/disputa_emissao.py:881`; `app/chamados/reconciliador.py:1290`, `app/chamados/reconciliador.py:1324`)
- Só incluir `batida_ausente` na condição do evento não realiza a obra: o consumidor atual apenas retrata absolvidos e encerra órfãs; não chama a fábrica. Não ampliar globalmente `MODULOS_DE_TURNO`, pois ele governa outros ofícios. (`app/chamados/reconciliador.py:336`, `app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`; `app/chamados/models.py:1062`)
- O sinal existente é insuficiente: engole falha e sai quando cartório já julga; este só chama fábrica nas células julgadas naquela passada. Ata estável pode deixar a abertura sem nova emissão. (`app/chamados/signals.py:172`, `app/chamados/signals.py:174`; `app/ponto/services/cartorio.py:995`; `app/ponto/tests/test_o_outbox_celula.py:506`)
- O escritor existente aproveitável é `perguntar_pela_celula → perguntar → portas de disputa/pergunta`. Já compõe dia sem trabalho, ativo, futuro, teto por instante, veredito, parcial, identidade e pauta do limbo exportado. (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:129`, `app/chamados/services/fabrica_da_celula.py:145`)
- O fechamento deve delegar a `ChamadoColaborador.reconciliar`, cujo juiz já compõe premissa, sobrevivente C8 e guarda de pergunta viva; `reconciliaria` é o leitor do mesmo juiz para o vigia. (`app/chamados/models.py:623`, `app/chamados/models.py:629`, `app/chamados/models.py:648`, `app/chamados/models.py:651`, `app/chamados/models.py:711`)

Fontes adicionais necessárias:

| Fonte | Evento ou aposentadoria exigida |
|---|---|
| `reabrir` | Publicar depois de ressuscitar, pois reabertura pode devolver zero perguntas restauráveis. (`app/chamados/models.py:1382`, `app/chamados/models.py:1391`, `app/chamados/models.py:1408`) |
| Pergunta em reapontamento, validação, duplicata | Publicar para a célula/âncora anterior e a posterior; publicar só a nova não avisa quem perdeu o último fio. A identidade histórica também reaponta dentro de `PerguntaDisputa.abrir`. (`app/chamados/models.py:2420`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`) |
| Morte por leitura | Manter leitor puro; C6 já responde. Célula, tranca, identidade e relógio são insumos da resposta, sem obrigatoriedade de novo carimbo. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1197`) |
| Morte temporal sem escrita de pergunta | Já existe acionador `lavrar_perguntas_stale`, papel `lavra`, a cada cinco minutos; ele delega à fonte `veredito_lavrado.lavrar`. Um evento publicado pela fonte após persistir cobre esse disparo sem juiz novo; a cobertura de todos os casos produtivos permanece [nao verificado]. (`app/config/crons.py:390`, `app/config/crons.py:2199`; `app/chamados/management/commands/lavrar_perguntas_stale.py:50`, `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/veredito_lavrado.py:211`) |
| Lavra em lote | `bulk_update` não dispara `post_save`; precisa publicação na própria fonte se incluída na cobertura. (`app/chamados/veredito_lavrado.py:197`, `app/chamados/veredito_lavrado.py:207`) |
| Detector | Aposentar o segundo ato best effort que fabrica fio depois da abertura. (`app/ponto/management/commands/detectar_ausencias.py:787`, `app/ponto/management/commands/detectar_ausencias.py:800`) |
| Grade | Não acrescentar emissor próprio: já abre/reabre pela porta; evento dessa porta deve bastar. (`app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:529`) |
| C2 | Preservar abertura e reapontamento pela porta, sem nova fabricação local. (`app/chamados/reconciliador.py:879`, `app/chamados/reconciliador.py:922`) |
| Disparador | Retirar `_rodar`/`--apply`/dry que ainda escrevem; manter contador e alarme lendo a mesma autoridade. Remover seu registro em `JUIZES_POR_VARREDURA`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:268`, `app/chamados/management/commands/disparar_perguntas_competencia.py:280`, `app/chamados/management/commands/disparar_perguntas_competencia.py:287`, `app/chamados/management/commands/disparar_perguntas_competencia.py:304`; `app/config/crons.py:2007`) |

Contraexemplos obrigatórios ao diff:

- Trocar dedup histórico por perguntas vivas recria pergunta de fato já validado: a fábrica conserva identidade histórica deliberadamente; outro marco residual deve gerar outro motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2387`; `app/chamados/management/commands/disparar_perguntas_competencia.py:201`)
- Sem ata e com escala, não emitir: `_ata_do_dia` abstém e registra falta de ata. Sem escala, existe ramo legal por batidas; generalizar a abstinência também erra. (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/disputa_emissao.py:839`, `app/chamados/services/disputa_emissao.py:842`)
- Parcial sem período mantém chamado vivo e encaminha à lista administrativa; não ressuscitar cobrança do colaborador para zerar número bruto. (`app/chamados/services/disputa_emissao.py:847`, `app/chamados/services/disputa_emissao.py:856`, `app/chamados/services/disputa_emissao.py:894`)
- Só `post_save(PerguntaDisputa)` não cobre relógio nem `bulk_update`. (`app/chamados/models.py:2350`; `app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:207`)
- Chamar a fábrica indiscriminadamente pelo evento de célula muda todos os julgamentos, não apenas os 494; o consumidor precisa usar o universo aprovado e delegar aos guardas existentes. A composição individual dos 494 permanece [nao verificado]. (`app/ponto/portas/celula.py:664`; `app/chamados/reconciliador.py:1309`; `avais.txt (anexo):1`)

Selos afetados: concorrência do `abrir`, fonte do dedup, fábrica por evento, fio mudo por ciclo, O-OUTBOX e teto de escrita da batida. Não executei testes, pois pedem banco/escritas. (`app/chamados/models.py:828`, `app/chamados/models.py:850`; `app/chamados/tests/test_fabrica_por_evento.py:33`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:232`; `app/ponto/tests/test_o_outbox_celula.py:506`; `app/chamados/services/fabrica_da_celula.py:84`; execução [nao verificado])

## 05/11 18:08 · sessao 18d5620d9ed0 · - · - · origem=Ronald (tmux)

Censo somente leitura; não executei testes nem acessei banco/log. Execução, ocorrência e alcance em produção: **[nao verificado]**. [nao verificado]

**Porta e chamadores**

`carimbar` lê/escreve o candidato na instância recebida, consulta C6 e salva; não contém `atomic`, releitura nem `select_for_update`. Restaura campos e lavra na exceção, preservando o contrato de memória já coberto pelos testes. `app/chamados/models.py:2551`, `app/chamados/models.py:2572`, `app/chamados/models.py:2585`, `app/chamados/models.py:2598`, `app/chamados/models.py:2600`; `app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:190`, `app/chamados/tests/test_o261e_f2_fila.py:200`.

Oito chamadas de produção:

- `PerguntaDisputa.encerrar_fantasma`: `app/chamados/models.py:2674`.
- `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:405`.
- `retratar_perguntas_absolvidas_por_celula`: `app/chamados/reconciliador.py:620`.
- `resolver_perguntas_por_chamado_resolvido`, dois ramos: `app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1675`.
- `_materializar_ausencia_de_resposta`: `app/chamados/services/materializacao.py:546`.
- `_materializar_uma`: `app/chamados/services/materializacao.py:1199`.
- `materializar_pendentes_aprovadas`: `app/chamados/services/materializacao.py:1346`.

`_materializar_uma` já planta/carimba dentro de transação e levanta `_Reprovada` quando C6 recusa; uma releitura travada somente em `carimbar` chega depois do plantio. `app/chamados/services/materializacao.py:1142`, `app/chamados/services/materializacao.py:1197`.

**Escritores dos campos**

| Estado | Escritores e portas |
|---|---|
| Tripla `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição em memória `_escrever_carimbo`; persistência por `carimbar`, `carimbar_identidade`, `descarimbar`; limpeza em lote por `descarimbar_em_lote`. `app/chamados/models.py:2545`, `app/chamados/models.py:2598`, `app/chamados/models.py:2633`, `app/chamados/models.py:2641`, `app/chamados/models.py:2652`. |
| Via isolada | `encerrar_por_fato_no_espelho` e `relavrar`; não confundir unicidade da tripla com unicidade de toda escrita na via. `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. |
| `respondida_em` | Carimbo com opção resposta; resposta humana; automática; recuperação por trilha; limpeza por ressuscitação/correção. `app/chamados/models.py:2578`, `app/chamados/models.py:1493`; `app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1429`, `app/chamados/services/disputa_emissao.py:1445`. |
| `validada_em`, `validada_por` | `carimbar(resposta='validar')`; `_carimbar_validacao`; devolução por recusa; resposta nova; correção; ressuscitação. `app/chamados/models.py:2580`, `app/chamados/models.py:1493`; `app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`. |
| `resposta_colab` | Resposta humana; automática; correção para vazio. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1428`, `app/chamados/services/disputa_emissao.py:1393`. |
| Lavra: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; restauração em memória na mesma autoridade; `gravar_lavras_em_lote` faz `bulk_update`. `app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:197`. |
| `_pv_memo` | `pergunta_viva` escreve; `_desfazer_em_memoria` e `descarimbar` removem. `app/chamados/juizes.py:847`; `app/chamados/models.py:2612`, `app/chamados/models.py:2638`. |

A catraca já declara as portas exatas da tripla e via; alteração criando helper que escreve por fora pode quebrá-la. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`. A catraca de validação nomeia `validacao.py` como dono, mas permite `models.py` e `materializacao.py`; portanto o registro “escritor único de validada_em” não equivale a uma atribuição única literal no código. `app/chamados/tests/test_contract_dono_validada.py:26`, `app/chamados/tests/test_contract_dono_validada.py:30`.

**Leitores diretos e seletores relevantes**

- Estado/C6/SQL: `app/chamados/models.py:2481`, `app/chamados/models.py:2713`; `app/chamados/juizes.py:647`, `app/chamados/juizes.py:781`, `app/chamados/juizes.py:1135`.
- Lastro/slot: `app/chamados/juizes.py:277`, `app/chamados/juizes.py:334`, `app/chamados/juizes.py:658`, `app/chamados/juizes.py:673`.
- Materialização/validação/reconciliação: `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:433`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:1183`, `app/chamados/services/materializacao.py:1294`; `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:363`, `app/chamados/services/validacao.py:422`; `app/chamados/reconciliador.py:364`, `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:583`.
- Lavra e stale: `app/chamados/veredito_lavrado.py:162`, `app/chamados/veredito_lavrado.py:166`, `app/chamados/veredito_lavrado.py:173`; raio-X lê lavra/candidato: `app/chamados/services/raiox.py:31`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:173`.
- Via/reatamento/duplicata/fábrica: `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:223`; `app/chamados/services/fio.py:336`, `app/chamados/services/fio.py:656`, `app/chamados/services/fio.py:882`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/disputa_emissao.py:695`.
- Resposta/ação/cobrança: `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/acoes_disputa.py:341`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/services/declaracao_texto.py:211`, `app/chamados/services/declaracao_texto.py:366`; `app/chamados/services/explicador.py:79`, `app/chamados/services/explicador.py:95`; `app/chamados/canal.py:65`.
- Histórico/painel/medidas: `app/chamados/services/historico.py:99`, `app/chamados/services/historico.py:101`, `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:240`, `app/chamados/services/verbos.py:311`.
- Vigias: `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:786`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/furo_sem_canal.py:81`.
- Passivo/reversão: `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:140`, `app/chamados/services/passivo_validado.py:515`.
- Comandos: `app/chamados/management/commands/auditar_invariantes_chamados.py:538`, `app/chamados/management/commands/auditar_invariantes_chamados.py:905`, `app/chamados/management/commands/auditar_invariantes_chamados.py:955`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/dedup_perguntas_fato.py:71`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`.
- Interfaces: `app/api/views.py:1173`, `app/api/views.py:2580`, `app/api/views.py:2636`; `app/api/views_mensageria.py:567`; `app/chamados/views.py:1257`; `app/chamados/views_cobrar.py:169`, `app/chamados/views_cobrar.py:291`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/chamados/templatetags/chamados_tags.py:128`.
- Outros domínios: `app/folha/previa.py:142`; `app/ponto/services/procedencia_batida.py:71`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/cartorio.py:1590`, `app/ponto/services/cartorio.py:1599`; `app/ponto/supra_juiz.py:545`.
- Templates: `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:186`, `app/templates/chamados/partials/_card_atendimento.html:384`; `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/modal_fio.html:212`; `app/templates/chamados/partials/_lote_preview.html:24`.

**Contraexemplos que um diff somente “atomic + releitura + C6” ainda permite**

1. **Valida a resposta nova com a intenção antiga.** Consumidor coletou A; colaborador grava B e limpa validação; carimbar relê B e aplica `resposta='validar'`; C6 morre imediatamente por `validada_em`. A releitura sozinha não preserva a exigência de validar a resposta julgada pelo humano. `app/chamados/services/disputa_emissao.py:1285`, `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1135`.

2. **Sobrescreve identidade humana apesar da releitura.** A decisão administrativa escreve `admin_decidiu`; automático relê, troca a via e C6 pode continuar morto por célula absolvida/validação. C6 responde vitalidade, não “a via automática pode substituir decisão humana?”. É necessário preservar o ato concorrente ou rejeitar candidato obsoleto. `app/chamados/models.py:2511`, `app/chamados/models.py:2575`; `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1195`.

3. **Memo reaproveitado após alteração apenas na ata.** Chave contém veredito/via da célula, mas não `ata`, `dna`, `motivo` ou `trabalha`; C6 consulta prova do marco/pendência. `refresh_from_db` não é garantia de remover atributos particulares como `_pv_memo` [nao verificado]; remover explicitamente o memo antes do julgamento evita depender disso. `app/chamados/juizes.py:811`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1194`; remoções explícitas existentes: `app/chamados/models.py:2612`, `app/chamados/models.py:2638`.

4. **Releitura perde célula em julgamento.** Há caminho explícito que troca a célula em memória pela célula do julgamento em curso porque a lavra persistida ainda é anterior; reler tudo indiscriminadamente pode trocar a autoridade do evento pela foto antiga. O uso deste caminho pelos oito chamadores requer conferência; não provei ocorrência em `carimbar`. `app/chamados/models.py:1437`, `app/chamados/models.py:1439`.

5. **Trava de pergunta não estabiliza todos os insumos de C6.** O inverso trava pergunta e disputa; sua `select_related('celula')` não inclui célula no `of`. Mudança da ata, batida, vínculo, tranca da competência e destino continua sendo domínio distinto da exclusão mútua daquela linha. `app/chamados/services/relavra_via.py:221`; `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1201`.

6. **Ordem de travas precisa ensaio real.** Já há caminhos disputa→trabalho nas perguntas e chamado→signals que chegam ao carimbo; copiar pergunta+disputa não prova ausência de ciclo. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:1156`; `app/chamados/signals.py:75`, `app/chamados/signals.py:125`; `app/chamados/services/relavra_via.py:221`. Deadlock real/ordem do PostgreSQL: **[nao verificado]**.

7. **Eventos observam o resultado final do atomic.** O save de pergunta agenda beco em `on_commit`; ampliar a transação muda quando o leitor roda. O inverso já exige esse adiamento na validação para não fechar a disputa pelo estado intermediário. `app/chamados/signals.py:197`, `app/chamados/signals.py:202`; `app/chamados/services/validacao.py:387`.

8. **Rollback externo depois de sucesso interno.** `carimbar` pode retornar True e o atomic externo posteriormente desfazer a transação; a restauração atual só cobre exceção dentro do corpo. Teste futuro precisa distinguir falha do save, falha do commit do atomic interno e rollback do chamador. Garantia de restauração da instância após rollback externo: **[nao verificado]**. `app/chamados/models.py:2598`, `app/chamados/models.py:2603`; `app/chamados/services/materializacao.py:1142`, `app/chamados/services/materializacao.py:1201`.

O lastro normativo para rejeitar meia correção, preservar trilha e não criar escritor/juiz paralelo está em `CLAUDE.md:17`, `CLAUDE.md:28`, `CLAUDE.md:32`, `CLAUDE.md:45`, `CLAUDE.md:167`, `CLAUDE.md:175`.

## 05/11 18:07 · sessao 3b9ce3fab227 · - · - · origem=Ronald (tmux)

Censo de produção, separado dos testes; chamadas indiretas por reflexão fora dos selos existentes permanecem [nao verificado]. [nao verificado]

| Grupo | Escritores / portas |
|---|---|
| Tripla `materializada_em/materializada_por/via_resolucao` | Atribuição central `_escrever_carimbo`, usada por `carimbar`, restauração, identidade e descarimbo; limpeza persistida em lote por `descarimbar_em_lote`. `app/chamados/models.py:2545`, `app/chamados/models.py:2575`, `app/chamados/models.py:2608`, `app/chamados/models.py:2626`, `app/chamados/models.py:2637`, `app/chamados/models.py:2652`. |
| `via_resolucao` isolada | `encerrar_por_fato_no_espelho`; `relavra_via.relavrar`. `app/chamados/models.py:2662`, `app/chamados/services/relavra_via.py:123`. |
| `respondida_em` | `carimbar`; restauração; resposta humana; limpeza em correção/ressuscitação; resposta automática; recuperação pela trilha. `app/chamados/models.py:2578`, `app/chamados/models.py:2609`, `app/chamados/models.py:1493`, `app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1429`, `app/chamados/services/disputa_emissao.py:1445`. |
| `validada_em/validada_por` | `carimbar(resposta='validar')`; restauração; validação/devolução; resposta nova; correção; ressuscitação. `app/chamados/models.py:2581`, `app/chamados/models.py:2610`, `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1493`. |
| `resposta_colab` | Resposta humana, resposta automática, limpeza para correção. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1428`, `app/chamados/services/disputa_emissao.py:1393`. |
| Lavra | `lavrar` e restauração atribuem os três campos; backfill também prepara objetos, persistidos pela porta em lote. `app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:243`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`. |
| `_pv_memo` | `pergunta_viva` grava; restauração e descarimbo removem. Chave atual inclui validação, resposta, via, instante, data/tipo, célula/veredito/via e destino/status; não inclui ata nem autores. `app/chamados/juizes.py:811`, `app/chamados/juizes.py:847`, `app/chamados/models.py:2612`, `app/chamados/models.py:2638`. |

Os oito pontos de chamada de `carimbar` são `encerrar_fantasma`, duas funções reconciliadoras, dois ramos da resolução por chamado, absorção de ausência, materialização individual e materialização de aprovadas. `app/chamados/models.py:2674`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:620`, `app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:1199`, `app/chamados/services/materializacao.py:1346`.

Leitores relevantes, agrupados:

- **Decisão/estado:** C6, lastro, estado/queryset/propriedades da pergunta, lavra e raio-X. `app/chamados/juizes.py:277`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1257`, `app/chamados/models.py:2488`, `app/chamados/models.py:2735`, `app/chamados/models.py:2807`, `app/chamados/veredito_lavrado.py:163`, `app/chamados/veredito_lavrado.py:179`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:173`.
- **Execução/filas:** reconciliador, materialização, validação, emissão, fábrica, cobrança, avisos, becos, fio, ações e declaração. `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:583`, `app/chamados/reconciliador.py:1017`, `app/chamados/reconciliador.py:1433`, `app/chamados/services/materializacao.py:433`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:604`, `app/chamados/services/materializacao.py:1292`, `app/chamados/services/materializacao.py:1341`, `app/chamados/services/validacao.py:363`, `app/chamados/services/validacao.py:422`, `app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:909`, `app/chamados/services/disputa_emissao.py:1152`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:138`, `app/chamados/services/cobranca.py:229`, `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/becos.py:30`, `app/chamados/services/fio.py:336`, `app/chamados/services/fio.py:656`, `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/acoes_disputa.py:341`, `app/chamados/services/declaracao_texto.py:211`, `app/chamados/services/declaracao_texto.py:366`, `app/chamados/services/declaracao_texto.py:635`.
- **Apresentação/integrações:** views, API, mensageria, relatórios, explicador, canal, calendário, ficha, prévia, dia do colaborador e cartório. `app/chamados/views.py:1257`, `app/chamados/views.py:1280`, `app/chamados/views.py:1416`, `app/chamados/views_cobrar.py:169`, `app/chamados/views_cobrar.py:176`, `app/api/views.py:1009`, `app/api/views.py:1173`, `app/api/views.py:1187`, `app/api/views.py:2580`, `app/api/views.py:2636`, `app/api/views_mensageria.py:567`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/explicador.py:95`, `app/chamados/canal.py:65`, `app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/ficha.py:171`, `app/folha/previa.py:142`, `app/ponto/services/dia_do_colab.py:69`, `app/ponto/services/cartorio.py:1590`, `app/ponto/services/cartorio.py:1599`.
- **Histórico/procedência/métricas:** histórico, painel, verbos e procedência. `app/chamados/services/historico.py:99`, `app/chamados/services/painel.py:67`, `app/chamados/services/painel.py:260`, `app/chamados/services/verbos.py:240`, `app/chamados/services/verbos.py:311`, `app/ponto/services/procedencia_batida.py:72`.
- **Vigias/passivo:** supra, fio, carimbo, furo sem canal, flip automático, passivo validado e relavra. `app/ponto/supra_juiz.py:545`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:783`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/services/furo_sem_canal.py:81`, `app/ponto/services/flip_auto.py:129`, `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:140`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:179`.
- **Comandos consumidores:** reconciliar grade/fantasmas, curar limbo, encerrar por fato, validar classe A, auditorias, reabrir órfãs, relavrar vias e deduplicar. `app/ponto/management/commands/reconciliar_grade.py:461`, `app/ponto/management/commands/reconciliar_fantasmas.py:212`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `app/chamados/management/commands/validar_classe_a.py:80`, `app/chamados/management/commands/auditar_disputas.py:71`, `app/chamados/management/commands/auditar_disputas.py:150`, `app/chamados/management/commands/auditar_invariantes_chamados.py:538`, `app/chamados/management/commands/auditar_invariantes_chamados.py:905`, `app/chamados/management/commands/auditar_invariantes_chamados.py:955`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `app/chamados/management/commands/relavrar_via_i6.py:58`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`.
- **Templates:** modal, atendimento e cartão de pergunta. `app/templates/chamados/partials/modal_fio.html:184`, `app/templates/chamados/partials/modal_fio.html:212`, `app/templates/chamados/partials/modal_fio.html:223`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:100`, `app/templates/chamados/partials/_card_atendimento.html:137`, `app/templates/chamados/partials/_card_atendimento.html:155`, `app/templates/chamados/partials/_card_atendimento.html:186`, `app/templates/chamados/partials/_card_atendimento.html:384`, `app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:24`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/_pergunta_card.html:42`.

C6 já está registrado, assim como C15/C17/C18; não há necessidade de juiz novo para vitalidade. `app/core/juizes.py:266`, `app/core/juizes.py:275`, `app/core/juizes.py:277`, `app/core/juizes.py:278`.

Contraexemplo importante: reler a linha e depois aplicar cegamente `resposta='validar'` pode validar resposta nova diferente da usada pelo plantio; C6 retorna morta imediatamente ao ver `validada_em`. O plantio acontece antes de `carimbar`; resposta nova limpa validação; a opção de carimbo a recoloca. `app/chamados/services/materializacao.py:1142`, `app/chamados/services/materializacao.py:1199`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/models.py:2580`, `app/chamados/juizes.py:1136`.

Outro contraexemplo: decisão humana já persistida torna C6 morta, mas isso não autoriza substituir a via humana por via automática antiga; o caminho inverso documenta precisamente essa perda e usa transação/releitura travada. `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1171`, `app/chamados/services/relavra_via.py:214`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`.

Os selos existentes restringem autores e chamadores internos por função; extrair o corpo para helper altera essas relações e exige considerar o selo. Os testes existentes de restauração verificam erros de `save`, lavra e memo; execução nesta leitura [nao verificado]. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:374`, `app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:190`, `app/chamados/tests/test_o261e_f2_fila.py:200`.

## 05/11 18:07 · sessao cabee82fdf83 · - · - · origem=Ronald (tmux)

- A lacuna está na origem: `.sh` passa por `varrer_linhas`, que só produz achado da faceta arquivo (`q=None`); nunca reconhece importação/uso do símbolo Python. O chamador real de `datas_cravadas_prod` está no payload de `manage.py shell --command`. (`bin/quem_chama.py:510`, `bin/quem_chama.py:673`, `bin/placar_code.sh:384`)

- Não basta acrescentar a lacuna a `FORA`: `_faceta` continuará contando qualquer tipo presente em `VARRIDOS`, inclusive imprimindo zero. A declaração precisa atingir a cobertura das facetas arquivo e símbolos do alvo afetado, preservando achados existentes como avulsos. (`bin/quem_chama.py:65`, `bin/quem_chama.py:690`, `bin/quem_chama.py:713`)

- **Contraexemplo para uma cura restrita às citações atualmente encontradas:** alvo `app/m/lib.py`, shell `python3 -c 'from m import lib; lib.mudada()'`. As chaves atuais incluem `app/m/lib.py`, `lib.py`, `m.lib`; nenhuma aparece no shell. Logo, usar apenas os achados de `varrer_linhas` para acionar “não varrido” mantém esse falso zero. Sem varrer Python no shell, a declaração conservadora deve alcançar o universo Python cuja cobertura depende desses chamadores; uma alternativa mais seletiva precisa declarar suas próprias lacunas. (`bin/quem_chama.py:369`, `bin/quem_chama.py:378`, `bin/quem_chama.py:633`, `bin/quem_chama.py:642`)

- Há outro falso zero conhecido: `{% load m_tags %}` seguido de `{% filter rot %}a{% endfilter %}`. Verifiquei em memória: retorna somente o `load`, sem uso do símbolo. A regex reconhece `|rot`, mas não o primeiro filtro do bloco. Correção mínima: trocar o prefixo de filtro `r'\|\s*'` por `r'(?:\|\s*|{%\s*filter\s+)'`. Os filtros seguintes do bloco já têm `|`. (`bin/quem_chama.py:526`, `bin/quem_chama.py:560`)

- `{# #}` multilinha já é removido preservando linhas; falta prendê-lo na fixture, que só apresenta comentário de uma linha. Verificação em memória confirmou ausência do filtro dentro do comentário multilinha. (`bin/quem_chama.py:520`, `bin/tests/test_quem_chama.sh:61`)

- As negativas de símbolo parado validam o **texto** `importa de m.lib`, sem verificar a **faceta**. Um mutante que associe esse mesmo texto ao símbolo mudado sobrevive. A asserção deve acompanhar o cabeçalho corrente e exigir que `usa_outra.py` e `rel_outra.py` apareçam exclusivamente sob `arquivo`, nunca sob qualquer `simbolo`. (`bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/quem_chama.py:713`, `bin/quem_chama.py:715`)

- A fixture já nasce sem criar `__init__.py`; entretanto, contém diversos imports Python locais para o mesmo alvo, capazes de mascarar o caso shell. O RED adicional deve isolar alvo cujo único chamador seja o payload shell. (`bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:51`, `bin/tests/test_quem_chama.sh:55`, `bin/tests/test_quem_chama.sh:58`)

- Outros limites que um diff apenas shell não corrige: `.py` ilegível por AST vira lista vazia; `simbolos_mudados` só percorre declarações diretamente no corpo de módulo/classe, deixando declarações sob `if` sem faceta; esses resultados não provam cobertura universal. (`bin/quem_chama.py:186`, `bin/quem_chama.py:243`, `bin/quem_chama.py:257`, `bin/quem_chama.py:390`)

## 05/11 18:07 · sessao e4831555b7ac · - · - · origem=Ronald (tmux)

A candidata precisa de **trava + releitura + recusa da intenção obsoleta + invalidação do memo + C6**, mantendo o corpo escritor existente em `carimbar`; somente reler e reaplicar não protege decisão humana nem resposta nova. `app/chamados/models.py:2551`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1136`.

O menor desenho na fonte é adicionar `@transaction.atomic`, fotografar os campos causais recebidos, fazer `refresh_from_db` por queryset com a mesma declaração de trava de `relavra_via`, limpar `_pv_memo`, devolver `False` se a fotografia diferir e, caso contrário, continuar pelo corpo existente. O registro da trava existente é `of=('self', 'disputa')` com `select_related('disputa', 'celula')`; a ordem efetiva e a API `refresh_from_db(from_queryset=...)` no ambiente não foram executadas **[nao verificado]**. `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`; `app/chamados/models.py:2572`.

A fotografia deve compreender `disputa_id`, `celula_id`, `chamado_destino_id`, `motivo`, `data_evento`, `tipo_resposta`, `resposta_colab`, `respondida_em`, `validada_em`, `validada_por_id`, `materializada_em`, `materializada_por_id`, `via_resolucao`; comparar `recusa_motivo` também é conservador, embora não seja insumo direto nos corpos de C6 e lastro examinados. `app/chamados/juizes.py:632`; `app/chamados/juizes.py:718`; `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:1135`; `app/chamados/models.py:2572`.

Contraexemplos indispensáveis:

- **Resposta nova antes da trava:** o emissor limpa a validação anterior; sem comparação de obsolescência, `resposta='validar'` valida a resposta nova automaticamente e C6 aceita pelo degrau zero. O RED deve afirmar que o ato antigo não valida nem carimba essa resposta. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1136`.
- **Decisão humana antes da trava:** `fechar_por_admin` grava identidade `admin_decidiu`; reaplicar o carimbo automático sobre a linha relida pode trocar sua via, e C6 continuar morto. O RED deve preservar a decisão e seu autor, recusando o ato antigo. `app/chamados/models.py:2511`; `app/chamados/models.py:2628`; `app/chamados/juizes.py:1148`.
- **Memo com ata nova:** a chave contém veredito e via da célula, mas omite `ata` e `dna`, lidos para a concordância com a luz; portanto a releitura precisa eliminar `_pv_memo`, mesmo se os campos da chave parecerem iguais. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:1105`; `app/chamados/juizes.py:1111`.
- **Validação obsoleta antes de alcançar a fonte:** `validar_pergunta` lê sem trava e grava a validação antes de materializar/carimbar; uma resposta nova entre essa leitura e `_carimbar_validacao` pode receber assinatura obsoleta que a fotografia dentro de `carimbar` já encontra persistida. Esse contraexemplo permanece fora da proteção proposta na fonte. `app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:424`.
- **Insumos externos durante o julgamento:** travar pergunta e disputa não trava célula, batidas nem tranca de competência; C6 consulta esses insumos. A proteção completa dessas intercalações permanece **[nao verificado]**. `app/chamados/services/relavra_via.py:221`; `app/chamados/juizes.py:720`; `app/chamados/juizes.py:774`; `app/chamados/juizes.py:1140`.
- **Transação externa:** `_materializar_uma` envolve plantio e carimbo em `atomic` e reverte o plantio se `carimbar` retornar `False`; o atomic interno precisa preservar esse comportamento. Rollback externo depois de retorno positivo ainda pode deixar a instância em memória com valores revertidos no banco **[nao verificado]**. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1201`.

O selo verde de concorrência e a medição da ordem real de travas continuam pendentes; inspeção estática não os prova **[nao verificado]**. [nao verificado]

## 05/11 18:07 · sessao 43092e547a2f · - · - · origem=Ronald (tmux)

Os cinco atos têm fontes distintas; C6 já responde à morte por leitura, e o C11 registrado continua sendo ausência/silêncio. Não há motivo demonstrado para criar juiz novo. (`app/docs/PROMPTS.md:1882`; `app/core/juizes.py:266`; `app/core/juizes.py:271`)

| Ato | Fonte e efeito constatado | Consequência para o diff |
|---|---|---|
| Reabrir | Grava vivo e ressuscita perguntas existentes; o universo exige pergunta materializada sem resposta, e consulta C6 sobre o pós-estado. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`; `app/chamados/models.py:1444`) | A abertura sem pergunta histórica precisa passar pelo emissor existente; ressuscitação sozinha não a entrega. (`app/chamados/models.py:1423`; `app/chamados/services/fabrica_da_celula.py:129`) |
| Retratar duplicata | A porta carimba identidade e mantém trilha; não encerra chamado nesse corpo. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`) | Consultar C6 depois: resposta textual sem decisão administrativa precede a identidade e pode manter a pergunta viva. (`app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`) |
| Validação | `_carimbar_validacao` escreve assinatura; C6 mata já no primeiro degrau. A validação tenta materializar e devolver recusa. (`app/chamados/services/validacao.py:127`; `app/chamados/juizes.py:1136`; `app/chamados/services/validacao.py:422`) | Reconciliação dos destinos já passa por `reconciliar`; não criar outro escritor de estado. (`app/chamados/services/materializacao.py:728`; `app/chamados/services/materializacao.py:738`; `app/chamados/models.py:607`) |
| Morte por leitura | C6 contempla tranca, marco atendido, célula não acusadora e efeitos da janela. (`app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) | Gancho só em `PerguntaDisputa.save` deixa mortes sem evento descobertas; getter C6 deve continuar leitura. (`app/chamados/services/competencia_trancada.py:125`; `app/chamados/services/raiox.py:110`) |
| Reapontar | Fonte única salva destino/data e relavra; trava o destino contra encerramento concorrente, mas não trata o antigo depois da perda. (`app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`; `app/chamados/services/fio.py:849`) | Capturar a âncora anterior antes da troca e verificar ambos os lados; a pergunta pode usar o chamado da disputa quando destino é nulo. (`app/chamados/juizes.py:1382`; `app/chamados/services/fio.py:845`) |

A composição existente é suficiente para orquestrar: C6 decide vida, C15 reduz coleção, C3 decide premissa e C17 decide alcance; a emissão continua nas portas da fábrica. Isso é composição de autoridades, sem precisar inventar uma regra paralela para “deve perguntar”. (`app/core/juizes.py:263`; `app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/chamados/services/fabrica_da_celula.py:129`)

Para morte por leitura causada por tranca, existe fonte própria que mede C6, encerra os chamados e encaminha respostas ao DP sem carimbar a pergunta. (`app/chamados/services/competencia_trancada.py:125`; `app/chamados/services/competencia_trancada.py:178`; `app/chamados/services/competencia_trancada.py:207`; `app/chamados/services/competencia_trancada.py:276`)

Para o relógio, já existe `lavrar_perguntas_stale`, programado a cada cinco minutos e classificado como lavra; ele seleciona stale e chama a fonte única `lavrar`. Seu julgamento é de materialização de fato, portanto um gancho ali ainda precisa consultar C6 para afirmar morte da pergunta. (`app/config/crons.py:390`; `app/config/crons.py:2199`; `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:219`)

Contraexemplos para qualquer diff pequeno demais:

- Mover integralmente o ramo do cron conserva a escolha de só um motivo e a barreira de qualquer identidade histórica; não entrega automaticamente todos os marcos residuais. (`app/chamados/management/commands/disparar_perguntas_competencia.py:158`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`)
- Chamar apenas `perguntar_pela_celula` depois dos atos também mantém o filtro de qualquer motivo já existente. A pergunta decidida pode representar ação administrativa legítima, expressamente nomeada pelo diagnóstico existente. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/pergunta_no_app.py:29`; `app/chamados/services/pergunta_no_app.py:94`)
- Reusar objetos antigos pode conservar resultado C6 anterior: a chave memo não inclui ata nem relógio, embora C6 leia os marcos pendentes e a janela. Releitura fresca é necessária para conferir pós-estado; incidência nos 494 é [não verificado]. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1194`; `app/chamados/juizes.py:1197`)

Leitores adicionais atingidos por mudanças em C6/fio: app, resposta humana, relatórios, pendências, folha, calendário/detalhe do dia, vigia, supra e emissão retroativa. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/services/fio.py:602`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/services/aviso_pendencia.py:13`; `app/folha/previa.py:144`; `app/ponto/services/dia_do_colab.py:70`; `app/chamados/services/vigia_fio.py:166`; `app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98`)

Chamadores das portas de perda/costura: dedup, migração, reconciliador C2 e reatamento, declaração textual, limbo e reversão do reatamento ao limbo. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`; `app/chamados/services/fio.py:947`; `app/chamados/reconciliador.py:922`; `app/chamados/reconciliador.py:1570`; `app/chamados/reconciliador.py:1582`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:243`; `app/chamados/services/limbo_folgas.py:276`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`)

## 05/11 18:07 · sessao c7f4cf6801f7 · - · - · origem=Ronald (tmux)

Censo em leitura, com estes pontos para a consolidação:

- **Contrato da fonte:** cada alvo tem faceta arquivo e facetas dos símbolos mudados; tipo fora de `VARRIDOS` deve sair “nao varrido”, com achados avulsos que não fecham o universo. `bin/quem_chama.py:8-15`.
- **Não resolve só alterar o aviso `FORA`:** a condição real que autoriza contagem continua sendo `tipo in VARRIDOS`; ela produz `zeros` inclusive quando o levantamento está incompleto. `bin/quem_chama.py:65-69`, `bin/quem_chama.py:690-699`.

| Estado/função | Escritor/fonte | Leitores |
|---|---|---|
| `VARRIDOS` | `bin/quem_chama.py:38` | `_faceta`: `bin/quem_chama.py:691`; apresentação do contrato: `bin/quem_chama.py:705` |
| `FORA` | `bin/quem_chama.py:65` | `render`: `bin/quem_chama.py:706` |
| `a['tipo']` | classificação em `bin/quem_chama.py:637`, armazenamento em `bin/quem_chama.py:658` | `render`: `bin/quem_chama.py:713` |
| `info['tipo']` | `_tipo_simbolo`, `bin/quem_chama.py:311`; atribuição em `bin/quem_chama.py:656` | `render`: `bin/quem_chama.py:715` |
| Achados externos | despacho em `bin/quem_chama.py:670`; `varrer_py`: `bin/quem_chama.py:672`; shell: `bin/quem_chama.py:674`; template: `bin/quem_chama.py:676`; config: `bin/quem_chama.py:678` | inserção na faceta em `bin/quem_chama.py:681-683`; retorno ordenado em `bin/quem_chama.py:687` |
| Uso próprio | `varrer_proprio`: `bin/quem_chama.py:442` | `quem_chama`: `bin/quem_chama.py:684-686` |
| Contadores | único lugar `_faceta`, `bin/quem_chama.py:694-698` | resumo `bin/quem_chama.py:717-720`; mensagem final `bin/quem_chama.py:747-748` |
| Saída textual | `render`: `bin/quem_chama.py:702` | `main`: `bin/quem_chama.py:737`; gravação opcional `bin/quem_chama.py:738-743`; stdout `bin/quem_chama.py:750` |

**Integrações externas nominalmente encontradas:** o selo escolhe a ferramenta pelo override `QUEM_CHAMA_PY` ou pelo arquivo padrão, e a executa contra fixture, determinismo, erros de saída e commits históricos. `bin/tests/test_quem_chama.sh:18`, `bin/tests/test_quem_chama.sh:25`, `bin/tests/test_quem_chama.sh:148`, `bin/tests/test_quem_chama.sh:253`, `bin/tests/test_quem_chama.sh:257`, `bin/tests/test_quem_chama.sh:262`, `bin/tests/test_quem_chama.sh:266`. A régua executa todos os `tests/test_*.sh`, incluindo este selo. `bin/regua.sh:138-145`.

**Selo que precisa tocar:** os testes de símbolo parado apenas conferem texto global, sem conferir se o achado ficou sob a faceta arquivo; as duas ocorrências estão em `bin/tests/test_quem_chama.sh:159-160` e `bin/tests/test_quem_chama.sh:167-168`. Os auxiliares existentes `_sec`, `_tem_sec`, `_naotem_sec` delimitam o alvo inteiro, não a faceta do símbolo. `bin/tests/test_quem_chama.sh:210-212`. O invariante lê a própria lista `VARRE` emitida, e por isso não certifica sozinho que ela seja semanticamente completa. `bin/tests/test_quem_chama.sh:239-251`. O fixture atual não cria `__init__.py` nos pacotes. `bin/tests/test_quem_chama.sh:28`.

**Juiz:** a ferramenta declara literalmente “lista e FATO, nao juiz” no contrato e no cabeçalho de saída. `bin/quem_chama.py:4-6`, `bin/quem_chama.py:703`. Não encontrei juiz cadastrado para o censo de chamadores em `app/core/juizes.py` nem em `app/docs/JUIZES.md`; a tabela é lookup e indica o registro vivo como autoridade. `app/docs/JUIZES.md:7-12`. O registro define autoridade por família/pergunta, não por qualquer função de instrumento. `app/core/juizes.py:3-14`.

**Irmãos e contraexemplos reais da cópia:**

- Python em string de `manage.py shell --command` chama `datas_cravadas_prod`. `bin/placar_code.sh:384`.
- Python em `python3 -c` chama `core.mapa.contrato.sem_contrato`, `core.placar_registro.linha` e `core.censo_pergunta2.copias`. `bin/placar_code.sh:245`, `bin/placar_code.sh:429`, `bin/placar_code.sh:465`.
- Procurar somente o módulo pontilhado completo no shell deixa passar `from core import juizes as j` e `from core import esteira_vigia as ev`. `bin/placar_code.sh:484`, `bin/placar_code.sh:508`.
- A mesma forma de import de pacote ocorre em heredocs de fabricante, esteira e gate. `bin/fabricante.sh:43`, `bin/fabricante.sh:80`, `bin/fabricante.sh:112`, `bin/esteira.sh:19`, `bin/gate_cobranca.sh:50-51`.
- O shell hoje é lido como linhas e só produz achados de arquivo, nunca de símbolo: `varrer_linhas` fixa `q=None`. `bin/quem_chama.py:510-512`, `bin/quem_chama.py:673-674`.
- Scripts sem extensão com shebang também entram pela mesma classificação de chamador shell; limitar a correção a caminhos terminados em `.sh` deixa essa classe descoberta. `bin/quem_chama.py:612-613`.

**Regras aplicáveis:** referência pela regra e casos antes do código: `app/docs/LEIS.md:136`; não apagar por zero sem censo válido: `app/docs/LEIS.md:137`; correção na fonte e escritor único: `CLAUDE.md:17-21`, `CLAUDE.md:32-36`; instrumento permite `padrao=n/a (instrumento)`: `CLAUDE.md:65-70`.

## 05/11 18:07 · sessao 3bb1b139c66d · - · - · origem=Ronald (tmux)

Leitura somente documental/código; execução de casos, produção e ordem efetiva de aquisição de travas **[nao verificado]**.

**Aval atual supera a dúvida do `pd_pep.r`:** candidata 1 está explicitamente escolhida: `carimbar` contém `atomic`, relê linha travada e C6 rejulga; medir ordem de trava a partir de `relavra_via.py:221`, conferir `_pv_memo` e escrever RED de decisão humana intercalada com resposta nova. `app/docs/PROMPTS.md:1882`. MANHA-0511 item 7 permanece “O261 item 3 PEP no carimbar”. `app/docs/PROMPTS.md:1853`.

**Casos pela regra para seção A:**

| Cadastro + estado + ato | Resultado exigido e lastro |
|---|---|
| Pergunta sem resposta; consumidor guardou versão anterior; admin decide o mesmo dia antes de `carimbar` adquirir sua trava; consumidor tenta carimbo automático. | Julgar linha atual, preservando decisão humana e trilha; versão coletada não pode sobrescrevê-la. Aval candidata 1 `app/docs/PROMPTS.md:1882`; L-007 `app/docs/LEIS.md:65`; caso de perda precisamente descrito `app/chamados/services/relavra_via.py:214`. |
| Pergunta validada na versão coletada; nova resposta chega antes da trava do carimbo; tentativa antiga de concluir. | Nova resposta não pode herdar decisão da resposta anterior; C6 deve julgar os insumos atuais. Aval RED `app/docs/PROMPTS.md:1882`; invariantes das intercalações responder/validar/recusar/fechar/reabrir/lote `app/docs/CORTES.md:333`. |
| Carimbo adquire trava antes da nova resposta; escritor de resposta já conservava instância velha. | Ciclo precisa preservar resposta e trilha nas duas ordens; `atomic` somente na leitura do carimbo não prova essa propriedade para o escritor concorrente. Invariantes exigidas `app/docs/CORTES.md:333`; prova das intercalações **[nao verificado]**. |
| Mesmo cadastro/insumos; repetir carimbo duas vezes. | Mesmo estado, uma linha de trilha, zero duplicata; contrato de idempotência `CLAUDE.md:175`; nenhum estado muda sem trilha `app/docs/CORTES.md:333`. |
| Pergunta que C6 ainda dá por viva, inclusive sem credor alcançável; ato automático tenta finalizar. | Não carimba e não reabre para fabricar credor; vigia de alcance conta. Aval F2 parte 2(a) `app/docs/PROMPTS.md:1777`; julgamento dentro da porta `app/docs/PROMPTS.md:1882`. |
| Empresa com corte 21; pergunta de 20/10, competência 10 trancada; outra de 21/10, competência 11 aberta; tentar carimbar por “mesmo mês civil”. | 20/10 pertence a 21/09–20/10; 21/10 pertence a 21/10–20/11. Aplicar corte cadastrado e competência da data do fato; tranca silencia por leitura, não fabrica quitação/batida. L-026 `app/docs/LEIS.md:77`; L-090 `app/docs/LEIS.md:123`; regra TRANCA-DE-VERDADE `app/chamados/juizes.py:1138`. |
| Turno 12x36 19–07, intervalo 23–00; volta 00:00; tentativa de concluir porque mudou a data civil. | Jornada continua em D, instante da volta é D+1; meia-noite não encerra fato pendente. L-085 `app/docs/LEIS.md:117`; L-073 `app/docs/LEIS.md:100`; geometria encadeada registrada `app/docs/JUIZES.md:24`. |
| Célula provisória não acusadora; marco ainda pendente e sem batida; tentativa de concluir; depois batida chega. | Antes não absolver por provisório; depois batida absolve na passada seguinte, sem manter cobrança do marco já respondido. Teto `CLAUDE.md:185`; corte posterior `app/docs/CORTES.md:413`; sua cláusula literal `app/docs/PROMPTS.md:1705`. |
| Dia com outro furo; marco desta pergunta já testemunhado; tentar fechar esta pergunta. | Pergunta cobra seu marco, não o outro; ausência do outro marco não autoriza continuar perguntando fato já provado. Fonte única por marco `CLAUDE.md:167`; regra nomeada BUG 117 `app/chamados/juizes.py:1173`. |
| Célula histórica gerada por A, hoje inativo mas vigente na data; B ativo hoje; tentativa de carimbo usando B. | A e DNA da célula continuam autoridade do fato; não reinterpretar com vínculo atual. L-020 `app/docs/LEIS.md:71`; juiz/lei da geradora `app/escala/alimentacao.py:231`; vigência prevalece ao flag ativo `app/docs/PROMPTS.md:1409`. |
| Geradora A fora da vigência na data; B parece alternativa; tentar corrigir silenciosamente durante carimbo. | Juiz devolve `None`, dono CADASTRO; não escolher substituta silenciosamente. Aval `app/docs/PROMPTS.md:1409`; regra comentada `app/escala/alimentacao.py:272`. |
| Célula trabalho=True, ata `{}`, sem resposta validada nem decisão humana; tentar concluir porque não há lâmpada apagada na lista vazia. | Ata nunca lavrada é desconhecimento, não prova de resolução; “em aberto (a decidir)” no dinheiro. L-061 `app/docs/LEIS.md:95`; corte `app/docs/PROMPTS.md:163`; cláusula da ata desconhecida `app/chamados/juizes.py:473`. |
| Célula trabalha=None, sem ata de outro tipo; ato tenta dispensar cobrança como folga. | Indeterminado cobra; somente `trabalha=False` ou ata com tipo preenchido diferente de trabalho dispensa, preservada ausência parcial. Corte `app/docs/CORTES.md:416`; registro vivo `app/core/juizes.py:137`. |
| Ausência parcial; marco fora do período coberto; tentativa de absorção por rótulo ausência. | Cobrar resto; rótulo não dispensa dia inteiro. Regra nomeada `app/chamados/juizes.py:403`; exceção registrada `app/docs/JUIZES.md:58`. |
| Sem alteração resolutiva; trancar depois destrancar competência. | Tranca não vira carimbo definitivo; reabertura devolve vitalidade por leitura. TRANCA-DE-VERDADE `app/chamados/juizes.py:1138`. |
| Resposta do colab, marco aceso, concordância classe A, dia coerente; tentar concluir. Variante resposta discordante ou sem hora comparável. | Concordante pode morrer automaticamente; discordante ou incomparável segue ao admin. Aval que altera regra anterior `app/docs/PROMPTS.md:1442`; régua e 45 sem hora vivas `app/docs/PROMPTS.md:1445`. |
| Disputa encerra por desligamento; pergunta já respondida; tentar carimbo `admin_decidiu` sem decisão na célula. | Segue viva, só Validar; encerrar disputa não é decidir dia e carimbo não pode alegar decisão inexistente. Corte explícito `app/docs/CORTES.md:342`. |

Todos são cadastros de ensaio; correspondência com pessoas/dias do dado produtivo fornecido **[nao verificado]**. A exigência de responder pela regra, não pelo código, está em `CLAUDE.md:45` e `app/docs/LEIS.md:136`.

**Autoridades existentes e regras transversais:**

- C6 `pergunta_viva`: `app/core/juizes.py:266`; C15 coleção `fio_vivo`: `app/core/juizes.py:275`; C17 porta alcançável: `app/core/juizes.py:277`; C18 dia sem trabalho: `app/core/juizes.py:278`.
- Competência: `janela_atual` e `competencia_fechada`, `app/core/juizes.py:283`; cadeia de marcos noturnos: `app/docs/JUIZES.md:24`; vínculo histórico: `app/escala/alimentacao.py:228`.
- JUIZES.md é consulta; divergência se resolve pelo registro vivo: `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`.
- Fonte única, sem fallback e sem derivador paralelo: `CLAUDE.md:17`; consumidor lê lâmpada: `CLAUDE.md:170`; escritor único: `CLAUDE.md:32`; sem juiz novo: corte estrutural `app/docs/CORTES.md:64`.
- Candidato de carimbo deve ser julgado sobre linha travada; requisito explícito, não inferência de “PEP”: `app/docs/PROMPTS.md:1882`.
- Atual inverso trava pergunta e disputa com `of=('self','disputa')` e seleciona célula sem travá-la nessa cláusula: `app/chamados/services/relavra_via.py:221`. Ordem real de aquisição não é determinada por ler a tupla Python; medida antes do verde é exigida `app/docs/PROMPTS.md:1882`, execução **[nao verificado]**.
- Não confundir L-120 com regra temporal: L-120 é PORTÃO-DE-FATURAMENTO `app/docs/LEIS.md:141`; o texto do corte temporal cita esse número, mas a lei própria é L-073 `app/docs/LEIS.md:100`.

**Contraexemplos relevantes para seção D:**

1. Releitura da pergunta sem invalidar relações/memo ainda pode julgar célula antiga, sobretudo decisão humana que escreve `CelulaDia` sem disputar a trava de `PerguntaDisputa`; C6 lê `p.celula` e `veredito_via='admin'`. `app/chamados/juizes.py:1147`. Correção depende do diff concreto; cobertura dessa corrida **[nao verificado]**.
2. C6 atual retorna morte quando veredito não acusa e `marcos_pendentes(cel)` está vazio; isso pode aceitar `ata={}`. Não usar esse comportamento nem teste existente como gabarito do caso de ata nunca lavrada. `app/chamados/juizes.py:1195`; regra de desconhecimento `app/chamados/juizes.py:473`; L-110 `app/docs/LEIS.md:136`.
3. Alargar trava para célula ou trocar ordem sem medir pode criar ciclo com produtores; o aval exige medida e toma como referência o inverso. `app/docs/PROMPTS.md:1882`; `app/chamados/services/relavra_via.py:221`. Deadlock efetivo **[nao verificado]**.
4. C6 aceita validação antes de olhar célula; transplantar validação velha para linha com resposta nova contorna o propósito da releitura. `app/chamados/juizes.py:1135`; invariantes e RED do aval `app/docs/PROMPTS.md:1882`.
5. Um `atomic` na porta não torna segura gravação futura de escritor que leu antes e grava depois; todas as ordens responder/validar/recusar/fechar/reabrir/lote exigem invariantes de determinismo e trilha. `app/docs/CORTES.md:333`.
6. Nenhuma regra CLT específica para a implementação desta trava foi fornecida **[nao verificado]**; o lastro suficiente para a mecânica solicitada é o aval explícito, L-007, L-061 e L-110. `app/docs/PROMPTS.md:1882`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:136`.

## 05/11 18:07 · sessao 5d42d1c1ec5b · - · - · origem=Ronald (tmux)

Referências da regra, lidas na cópia:

- O desenho já está decidido: `carimbar` deve abrir `atomic`, reler a pergunta travada e chamar novamente C6; conferir a chave de `_pv_memo`; medir ordem de trava contra `relavra_via.py:221`; RED de intercalação decisão humana/resposta nova. O `pd_pep.r` anterior está superado nesse ponto. `app/docs/PROMPTS.md:1882`.
- Os casos devem nascer da regra e cadastro; o código atual e seus testes não constituem gabarito. Lacuna de regra precisa aparecer como tal. `app/docs/LEIS.md:136`.
- A lâmpada por marco é a fonte dos consumidores; um juiz por pergunta e um escritor por entidade, com idempotência. `CLAUDE.md:167`; `CLAUDE.md:170`; `CLAUDE.md:175`; `app/docs/LEIS.md:95`.
- Estado crítico conserva usuário, antes/depois e motivo; nenhuma intercalação pode apagar estado sem trilha. `app/docs/LEIS.md:65`; `app/docs/CORTES.md:333`.

Casos sugeridos para A, todos sintéticos; ocorrência em produção [nao verificado]:

| Caso: cadastro + estado + ato | Resultado pela regra |
|---|---|
| Trabalho com marco faltante lavrado, fato encerrado, sem resposta/decisão: carimbo automático por `sem_turno_aberto` ou `completude_turno`. | Recusar se C6 ainda encontra pergunta viva; carimbo não resolve o fato. `app/docs/CORTES.md:253`; `app/docs/PROMPTS.md:1882`. |
| Marco desta pergunta respondido pela batida; outro marco ainda falta: encerrar esta pergunta. | Reconhecer a prova deste marco, mantendo cobrança do outro; consumidores leem ata por marco. `CLAUDE.md:167`; `app/docs/CORTES.md:413`. |
| Pergunta respondida sem decisão humana; resposta e batida discordam, ou não existe hora comparável: carimbo automático. | Permanecer para decisão administrativa; resposta não some. `app/docs/PROMPTS.md:1442`; regra literal de 25/10 também transcrita em `app/chamados/juizes.py:1090`; `app/chamados/juizes.py:1096`. |
| Pergunta respondida; batida acendeu marco; resposta concorda pela régua existente e dia coerente: encerrar. | Pode morrer automaticamente; nenhuma tolerância nova. `app/docs/PROMPTS.md:1442`; `app/chamados/juizes.py:1090`. |
| Consumidor carrega instância; depois admin decide o mesmo dia; consumidor tenta carimbo velho. | Reler a linha sob trava, rejulgar e preservar a decisão humana; resposta permanece histórica com trilha. `app/docs/PROMPTS.md:1882`; `app/docs/ADR/0002-celula-e-o-dia.md:31`; comentário que transcreve HX em `app/chamados/juizes.py:1151`. |
| Consumidor conserva pergunta anteriormente validada; colaborador envia resposta nova antes do carimbo. | Carimbo não pode transportar validação antiga como se validasse a nova resposta; reler e rejulgar C6, conservar voz/trilha. `app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:333`; `app/docs/LEIS.md:65`. A limpeza da validação por resposta nova está documentada no escritor em `app/chamados/services/disputa_emissao.py:1304`. |
| Duas tentativas concorrentes para a mesma pergunta. | Cada decisão usa linha travada; duas execuções equivalentes conservam mesmo estado, uma linha de trilha e zero duplicata. `app/docs/PROMPTS.md:1882`; `CLAUDE.md:175`. |
| Empresa corte 21; fatos 20/10 e 21/10, competência 10 trancada e 11 aberta: ler/carimbar. | 20/10 pertence à 10; 21/10 à 11. Tranca usa competência do fato, não mês civil; não fabrica batida nem absolvição. `app/docs/CORTES.md:216`; `app/docs/CORTES.md:66`; regra da tranca transcrita em `app/chamados/juizes.py:1142`. |
| 12x36 19–07, pausa 23–00; às 00:10 segue pendente a saída 07:00: carimbo por “dia anterior”. | A jornada continua no dia inicial; virada civil não encerra fato. Volta 00:00 é instante D+1 dentro da jornada D. `app/docs/LEIS.md:117`; `app/docs/LEIS.md:100`; `CLAUDE.md:185`. |
| Célula histórica gerada pelo vínculo A, agora encerrado; vínculo B atual com outros marcos: julgar passado. | Usar geradora/DNA da célula; troca atual não reescreve silenciosamente o fato histórico. Regeneração formal é ato separado e denunciado. `app/docs/JUIZES.md:98`; `app/docs/LEIS.md:71`; `app/docs/ADR/0003-celula-tres-naturezas.md:24`. |
| Célula sem ata, `trabalha=None`, sem outro fato resolutivo: tentar absolver por ausência de lâmpadas. | Vazio não prova dia sem trabalho: cobra. A regra explícita diz que `trabalha=None` sem ata de outro tipo cobra. `app/docs/CORTES.md:416`; `app/docs/JUIZES.md:55`. |
| Célula sem ata mas `trabalha=False`: mesmo ato. | Dia sem trabalho cala cobrança pelo cadastro declarado, não por ata vazia. `app/docs/CORTES.md:416`. |
| Célula `veredito='concorde'`, ata `{}`, `trabalha=True`, sem prova de marco: carimbo. | O aval não fornece resultado literal específico para a inconsistência “veredito absolve, ata não prova”; não promover teste atual a lei. L-061 e NONE-COBRA impedem afirmar quitação só pelo vazio; decisão exata [nao verificado]. `app/docs/LEIS.md:95`; `app/docs/CORTES.md:416`; `app/docs/LEIS.md:136`. |
| Ausência parcial, marco perguntado fora do intervalo coberto: carimbo por ausência. | Cobrar resto descoberto; parcial não dispensa dia inteiro. `app/docs/JUIZES.md:58`; `app/docs/JUIZES.md:60`. |
| Pergunta respondida sob disputa encerrada por desligamento/admin_encerra, sem decisão do dia. | Continua viva para Validar; fechar continente não autoriza inventar `admin_decidiu`. `app/docs/CORTES.md:342`. |
| Fecho sem credor e pergunta continua viva após C6: carimbar e reabrir automaticamente. | Não carimbar nem reabrir; vigia existente conta conforme seus universos; passivo vai à F3. `app/docs/PROMPTS.md:1777`; `app/docs/PROMPTS.md:1778`. |
| Instância recebeu candidato; lavra/save falha. | Memória não pode declarar ato que não persistiu; estado/trilha permanecem coerentes. L-007 e idempotência sustentam requisito; assinatura exata de restauração é implementação. `app/docs/LEIS.md:65`; `CLAUDE.md:175`. |
| Pergunta antiga ainda com marco devido: encerrar por idade. | Idade não absolve fato; fechamento por idade foi revogado. `app/docs/PROMPTS.md:1317`; `app/docs/CORTES.md:253`. |

Juízes e referências para B:

- C6 já é `chamados/juizes.py::pergunta_viva`; C7 responde fechamento da disputa; C15 agrega vitalidade da coleção; C17 responde alcance; C18 dia sem trabalho. `app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`.
- Competência: `janela_atual`, registrada na família feriado/prazo; corte varia por empresa. `app/core/juizes.py:283`; `app/docs/JUIZES.md:71`.
- Jornada/marcos cruzando meia-noite: autoridade `escala/servico_jornada.py::marcos_turno`; vínculo histórico `vinculo_do_dia`. `app/docs/JUIZES.md:25`; `app/docs/JUIZES.md:98`.
- JUIZES.md é lookup, registro vivo manda quando divergem. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`.

Contraexemplos relevantes para D:

- Copiar a linha travada inteira de volta para instância chamadora pode apagar campos modificados em memória pelo materializador antes da chamada; precisa levantar contrato de cada chamador. Essa possibilidade depende do código de cada chamador [nao verificado].
- Só travar `PerguntaDisputa` não prova estabilidade de célula, vínculo, resposta ou decisão humana durante C6: fontes externas e caches precisam entrar na revisão. O aval manda medir ordem de trava; não manda pressupor que o `of=('self','disputa')` já resolve todas as intercalações. `app/docs/PROMPTS.md:1882`; `app/chamados/services/relavra_via.py:221`.
- “`atomic` sozinho impede perda de decisão” é falso como prova: o aval exige releitura travada **e** C6 rejulgando. `app/docs/PROMPTS.md:1882`.
- Não transportar `_pv_memo` da instância recebida à recém-carregada; o aval explicitamente exige conferir sua chave. Caches de célula/disputa também não equivalem à linha relida. `app/docs/PROMPTS.md:1882`.
- A ordem humana versus resposta nova exige cenários nas duas direções; “resposta sempre vence” contradiz decisão explícita do admin no mesmo dia, e “decisão sempre vence qualquer resposta futura” precisa preservar o histórico e distinguir decisão nova de decisão antiga. `app/chamados/juizes.py:1151`; `app/docs/CORTES.md:333`; `app/docs/LEIS.md:65`.
- Não usar o teste atual de célula `concorde` com ata vazia para decidir a seção A; isso viola L-110. `app/docs/LEIS.md:136`.

## 05/11 18:07 · sessao 34a06d6fce99 · - · - · origem=Ronald (tmux)

Abertura e emissão, com linhas conferidas:

- `ChamadoColaborador.abrir` resolve célula, aplica vetos e cria dentro de `atomic`, com trava do colaborador tomada antes do dedup; o nascimento emite `EventoCelula` somente para `MODULOS_DE_TURNO`, conjunto que não inclui `batida_ausente`. (`app/chamados/models.py:863`; `app/chamados/models.py:913`; `app/chamados/models.py:932`; `app/chamados/models.py:938`; `app/chamados/reconciliador.py:336`)
- `criar` resolve célula e aplica os mesmos vetos, mas termina após criar/trilhar: não há emissão explícita de evento nem de pergunta. (`app/chamados/models.py:1026`; `app/chamados/models.py:1036`; `app/chamados/models.py:1048`)
- O sinal de chamado criado/estado alterado tenta `julgar_celula`, porém fica dentro de `engolir` e retorna durante julgamento; não constitui cobertura confiável da abertura. (`app/chamados/signals.py:164`; `app/chamados/signals.py:172`; `app/chamados/signals.py:174`)
- O cartório pergunta somente pelas células julgadas nesta passada; ata estável/pulada não ganha esse acionamento. (`app/ponto/services/cartorio.py:981`; `app/ponto/services/cartorio.py:986`; `app/ponto/services/cartorio.py:995`)
- Emissores concretos de `batida_ausente`: detector, grade e C2. Detector tenta construir fio posteriormente, com exceção absorvida; grade retorna sem emissão; C2 reaponta uma pergunta existente. (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`)

Fontes já existentes para realizar a obra:

- Escritor único de evento: `emitir_evento_celula`; grava somente identificadores da célula, colaborador e data. O contrato exige chamá-lo dentro da transação existente. (`app/ponto/portas/celula.py:664`; `app/escala/models.py:1851`)
- Consumidor único existente: `consumir_eventos_celula`; já relê fatos, processa por colaborador, conserva eventos pendentes em falha e admite adiamento por relógio sem consumir tentativa. Hoje ele retrata chamados absolvidos e encerra órfãs; não chama a fábrica para sanar fio mudo. (`app/chamados/reconciliador.py:1283`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`; `app/chamados/reconciliador.py:1337`)
- Escritor canônico de perguntas já existe: `perguntar` usa `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa`; `perguntar_pela_celula` aplica critérios do dia e silencia push. (`app/chamados/services/fabrica_da_celula.py:129`; `app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/fabrica_da_celula.py:174`)
- O ramo FIO MUDO do comando é outro acionador escritor: seleciona vivos com célula na janela, qualquer módulo, e usa `perguntar` em `--apply`. A migração precisa retirar essa escrita; trocar apenas o cron para `--alarme` deixa o comando manual como escritor. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:268`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287`)
- Registro/agenda também precisam acompanhar a migração: `JUIZES_POR_VARREDURA` ainda contém o comando por causa do FIO MUDO; o corte condiciona a saída à medida e pouso do evento. (`app/config/crons.py:2007`; `app/docs/CORTES.md:305`)

Contraexemplos indispensáveis para avaliar o diff:

1. Acrescentar somente `batida_ausente` à condição de evento em `abrir` não emite perguntas: o consumidor atual não chama a fábrica. (`app/chamados/models.py:939`; `app/chamados/reconciliador.py:1324`)
2. Acrescentar somente `perguntar_pela_celula` ao consumidor também não cobre os cinco atos: `motivos_a_perguntar` exclui qualquer motivo com identidade histórica não duplicata, sem consultar sua vitalidade. Validação/com resto e morte por leitura com pergunta histórica podem continuar sem nova ação. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`)
3. Fazer chamada síncrona à fábrica dentro de `abrir` muda a ordem do detector: o agrupamento por defeito de folga só é decidido após a abertura; o caminho agrupado deve permanecer administrativo. A fábrica genérica não consulta `pode_calar` nem pai LIMBO. (`app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:758`; `app/chamados/services/fabrica_da_celula.py:89`)
4. O agrupamento atual tenta terminar o filho; em recusa, pode preservá-lo agrupado/vivo. Não cabe tratar automaticamente todo filho agrupado como exclusão legítima: é preciso verificar a ação e a âncora resultantes. (`app/chamados/services/limbo_folgas.py:258`; `app/chamados/services/limbo_folgas.py:263`; `app/chamados/services/limbo_folgas.py:277`)
5. Concorrência: abrir já toma `Colaborador`; emissão de disputa toma `Disputa → Colaborador`. Emitir pergunta sincronamente sob a trava do nascimento pode inverter a ordem. O consumidor atual explicitamente evita travar essas entidades por conta própria. (`app/chamados/models.py:843`; `app/chamados/models.py:866`; `app/chamados/reconciliador.py:1290`)
6. A lista bruta não define dívida de pergunta: `VIVOS` inclui `registrado`, enquanto a fila exclui registrado e silêncio vigente. A competência histórica dos 494 e quantos pertencem a cada exclusão não foram fornecidos. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/services/painel.py:186`; [não verificado])
7. Sem ata, escala vigente exige abstenção; sem escala vigente há ramo das batidas. O código atual também retorna `True, None` em falha de leitura, o que não deve ser copiado como fallback autorizado pelo aval. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:828`; `app/chamados/services/disputa_emissao.py:839`)
8. C17 já responde alcançabilidade por chamado da célula na fila OU pergunta viva no app sob âncora na fila; sua resposta não implica uma pergunta obrigatória para cada chamado vivo. (`app/chamados/juizes.py:1341`; `app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1375`)

Exclusões pela regra que precisam sair do 494 antes do RED: registro sem ação; silêncio ainda vigente; dia parcial destinado à lista administrativa; dia futuro ou marco ainda não vencido; dia sem trabalho; colaborador não ativo; limbo exportado entregue à Pauta DP. A quantidade real de cada classe permanece não verificada. (`app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:186`; `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/services/fabrica_da_celula.py:93`; `app/chamados/services/fabrica_da_celula.py:98`; `app/chamados/services/fabrica_da_celula.py:101`; `app/chamados/services/fabrica_da_celula.py:171`; [não verificado])

Leitores relevantes além dos sítios iniciais: selo de `abrir` lê sua própria fonte e exige dedup/renascimento no corpo; semeadura/vigias leem eventos pendentes; `motivos_que_o_evento_deixou` lê a mesma seleção da fábrica; painel e C17 leem estados, silêncio, perguntas, âncoras e verbos. (`app/chamados/models.py:828`; `app/chamados/reconciliador.py:1257`; `app/chamados/services/fabrica_da_celula.py:191`; `app/chamados/services/painel.py:158`; `app/chamados/juizes.py:1341`)

## 05/11 18:07 · sessao 200581a434d0 · - · - · origem=Ronald (tmux)

Casos de aceite pela regra, sem resultados executados: a referência é a regra aplicada ao cadastro e às batidas, nunca a equivalência ao caminho antigo. (`app/docs/LEIS.md:136`)

| Caso | Cadastro + estado + ato | Resposta exigida pela regra |
|---|---|---|
| Abertura | Ativo; vínculo do dia válido; célula com ata de marco vencido faltante; nenhuma pergunta; abrir `batida_ausente`. | A pergunta nasce no evento da abertura, lendo a dívida da célula; varredura fica como vigia. (`app/docs/PROMPTS.md:1875`; `CLAUDE.md:168`; `CLAUDE.md:169`) |
| Dedup concorrente | Mesmo cadastro e fato; duas aberturas simultâneas. | Mesmo estado final, uma linha de trilha, nenhuma duplicata. (`CLAUDE.md:175`) |
| Reabrir sem restaurável | Dia ainda devido; chamado terminal; nenhuma pergunta restaurável; reabrir. | Reabrir está entre os atos cobertos; a dívida precisa conservar pergunta viva ou decisão administrativa pendente nomeada. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:253`; `app/chamados/services/vigia_fio.py:283`) |
| Duplicata | A perde última resolvedora; canônica do mesmo fato continua em B; retratar duplicata. | Preservar uma cobrança do fato e sua alcançabilidade, sem emitir irmã artificial para cada chamado duplicado. (`CLAUDE.md:175`; `app/docs/CORTES.md:253`; `app/docs/CORTES.md:404`) |
| Duplicata respondida | Pergunta contém resposta humana discordante ainda sem decisão; retratar duplicata. | A resposta não desaparece pelo carimbo de identidade; conversa permanece para decisão humana. (`app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1161`; `app/chamados/juizes.py:1163`) |
| Validação integral | Único marco devido; resposta validada e fato efetivamente resolvido. | Não fabricar outra pergunta para zerar contador bruto; cobrança sem premissa morre por lastro. (`app/docs/CORTES.md:253`; `app/chamados/juizes.py:1136`; `app/chamados/catalogo/premissa.py:144`) |
| Validação parcial | Saída e intervalo faltantes; validar/materializar só saída. | Não perguntar novamente a saída; intervalo continua exigindo fio ou decisão pendente. Marco resolvido não quita outro marco. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Recusa de materialização | Resposta validada; plantio recusado; dívida continua. | Não apresentar quitação fictícia nem perder o dono da decisão. (`app/docs/CORTES.md:253`; `app/docs/LEIS.md:136`) |
| Reapontar | A perde sua última resolvedora para B. | Preservar fio alcançável do dia; a transferência não exige duplicar o mesmo fato. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| Reapontar × fechar | Costura e encerramento do destino concorrem. | Nunca terminar com pergunta viva inacessível sob âncora terminal; preservar porta ou recusar o ato com motivo. (`app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| Morte por leitura | A última pergunta deixa de estar viva pela regra de C6, sem carimbo novo. | C6 é a autoridade existente sobre vida; C15 reduz a coleção. O construtor precisa nomear o juiz e os eventos dos insumos que causaram a morte; inventar juiz para cobrir o ato implica PAREI. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/docs/PROMPTS.md:1882`) |
| Resposta concordante | Batida acende o marco; resposta concorda pela régua existente; dia não acusa. | Pode cessar a pendência; resposta discordante ou sem comparação permanece para decisão humana. (`app/chamados/juizes.py:1090`; `app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1159`) |
| Vencimento exato | Marco às 18h; margem vigente 30 min; observar 18h29m59s e 18h30. | Antes do instante não cobrar esse marco; na igualdade o marco venceu. Não trocar por comparação apenas de data. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:845`) |
| Meia-noite | Turno 22h–06h ancorado ontem; observar 00h01 e 06h10. | Mudança de data não encerra turno; saída às 06h10 ainda não venceu a margem de 30 min. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`; `app/chamados/services/fabrica_da_celula.py:20`) |
| Janela 21→20 | Empresa corte 21; fatos em 20/10 e 21/10; trancar competência 10. | Competência 10 vai de 21/09 a 20/10; 21/10 pertence à próxima. Não estender a tranca pelo mês civil. Corte vem do cadastro da empresa. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:54`; `app/ponto/janelas.py:75`) |
| Tranca | Dia pertence à competência trancada; última pergunta morre por leitura. | Não reemitir pergunta para contornar tranca; ela tira a pergunta da fila, sem plantar batida nem absolver o fato. (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1142`) |
| Toque humano | Dia trancado; admin executa toque humano posterior previsto na regra. | Aplicar a reversibilidade existente; cron ou resposta do colaborador não substituem toque humano. (`app/docs/CORTES.md:83`; `app/chamados/juizes.py:1073`; `app/chamados/juizes.py:1079`) |
| Vínculo trocado | Célula gerada pelo vínculo antigo dentro da vigência; vínculo novo ativo hoje. | Geradora do dia manda; não substituir pelo atual. Geradora fora da vigência resulta em `None` e dono CADASTRO. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| Célula sem ata | Escala vigente; célula sem ata; tentar cobrar marco. | Não fabricar certeza de marco faltante; esperar ata e denunciar ausência dela. `trabalha=None` sozinho também não significa folga. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:842`; `app/core/juizes.py:139`) |
| Parcial sem período | Ausência parcial sem período declarado; dia ainda pendente. | Lista administrativa, sem pergunta, pai ou push; chamado histórico pode continuar vivo legitimamente. (`app/chamados/services/fabrica_da_celula.py:101`; `app/chamados/services/disputa_emissao.py:850`; `app/chamados/services/disputa_emissao.py:892`) |
| Exportada e histórico | Dia antigo exportado; observar alteração nova e passivo sem fato novo. | Não mudar o gravado exportado; diferença segue Pauta DP. Passado fica foto até fato novo; obra não autoriza backfill geral. (`app/docs/LEIS.md:125`; `app/docs/LEIS.md:145`) |
| Sem pergunta, com dono legítimo | Dia tem decisão pendente, ausência aberta, folga contestada validada ou dono CADASTRO/BATIDA já na lista administrativa. | Sair do universo de defeitos antes do RED, lendo o juiz/balde existente; `em_analise` sozinho não constitui ação nomeada. (`app/docs/PROMPTS.md:1882`; `app/chamados/services/vigia_fio.py:274`; `app/chamados/services/vigia_fio.py:283`; `app/chamados/services/vigia_fio.py:306`) |
| Hoje | Fato de hoje ainda em seu ciclo normal de emissão. | Não antecipar alarme de fio morto; hoje/futuro sai do contador alarmado. Isso não impede cobrar marco que já venceu por instante. (`app/chamados/services/vigia_fio.py:420`; `app/chamados/services/fabrica_da_celula.py:8`) |

Autoridades conferidas:

- C6 `pergunta_viva`; C15 `fio_vivo`; C17 `porta_alcancavel_do_dia`; C18 `dia_sem_trabalho`; C16 `motivo_do_veto`. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`; `app/core/juizes.py:276`)
- O **C11 registrado** responde ausência/silêncio, e não o rótulo operacional “FIO MUDO C11=(i)”. Não substituir ou registrar novo juiz por semelhança de nome. (`app/core/juizes.py:117`; `app/core/juizes.py:271`; `app/docs/PROMPTS.md:1875`)
- C17 exige chamado da célula na fila ou pergunta viva no app com âncora na fila; o botão Resolver dia fica expressamente fora dessa porta e é balde legítimo separado do vigia. (`app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:283`)
- Excluir vivos legítimos não significa estreitar `VIVOS`: `registrado` é vivo e não pede ação; `COBRANCA` é outro conjunto. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`)

A mortalidade por leitura tem dependências que um gancho exclusivo de `save(PerguntaDisputa)` não cobre: tranca, decisão/veredito da célula, marco aceso, e janela temporal consultada pelo C6; o juiz precisa permanecer leitura. A cobertura concreta desses eventos pelo diff do construtor deve ser auditada, sem atribuir escrita à função que serve app, painel e censos. (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1148`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1197`; `CLAUDE.md:168`; `CLAUDE.md:170`)

Os 494 têm aval documental, mas classificação individual, exclusões legítimas e distribuição pelos cinco atos são **[não verificado]**; o dado fornecido não contém os registros. (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`)

## 05/11 18:07 · sessao 2d1e22c04bb5 · - · - · origem=Ronald (tmux)

Regras verificadas para consolidar:

- **C11 do registro vivo é ausência**, não “FIO MUDO”: pergunta “a ausência do dia suspende, fecha ou levanta a cobrança viva?” e aponta a `silencio_ausencia.reconciliar_por_ausencia`. O rótulo operacional do aval não autoriza substituir essa autoridade. (`app/core/juizes.py:117`; `app/core/juizes.py:271`)
- **Morte por leitura já tem juiz: C6 `pergunta_viva`**. C15 `fio_vivo` reduz a coleção; C17 `porta_alcancavel_do_dia` responde alcance. Não precisam nascer juiz novo. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`)
- C6 é leitura com memo por instância, cuja regra mora em `_pergunta_viva_calc`; introduzir escrita na consulta faria o consumidor virar produtor. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:839`; `CLAUDE.md:168`; `CLAUDE.md:170`)
- A regra estrutural exige fio vivo ou ação administrativa nomeada enquanto persistir dívida da lâmpada; a morte não pode ser decidida por carimbo isolado, e o cron é vigia esperado zero. (`app/docs/CORTES.md:253`; `CLAUDE.md:167`; `CLAUDE.md:169`)
- O aceite deve nascer de cadastro + batidas + regra, antes de desenho/código; o caminho velho só mede impacto. (`app/docs/LEIS.md:136`)
- Idempotência exigida: repetir dá mesmo estado, uma linha de trilha, zero duplicata; censo de escritores deve fechar antes de alterar filtro/regra. (`CLAUDE.md:175`; `CLAUDE.md:193`)

Casos para A, com resposta pela regra:

| Cadastro/estado/ato | Resposta exigida |
|---|---|
| Ativo, vínculo válido, marco vencido faltante na ata, chamado aberto por `batida_ausente`. | A emissão nasce no evento da célula/abertura contemplada pelo aval; pergunta é do marco devido, não do simples status vivo. (`CLAUDE.md:168`; `app/docs/CORTES.md:253`; `app/chamados/services/fabrica_da_celula.py:8`) |
| Duas aberturas ou perdas simultâneas do mesmo fato. | Mesma dívida/fio final sem duplicação e com trilha idempotente. (`CLAUDE.md:175`) |
| Reabrir com dívida persistente, sem candidata a ressuscitar. | Estado vivo sozinho não satisfaz a regra: deve haver pergunta viva ou ação administrativa nomeada. (`app/docs/CORTES.md:253`) |
| Retratar duplicata cuja canônica cobre o mesmo fato. | Preservar unidade do fato; não criar irmã só porque um chamado ficou sem pergunta. Resposta discordante pendente continua para decisão humana. (`CLAUDE.md:175`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1163`) |
| Validar uma saída, permanecendo intervalo devido. | Marco quitado não volta a ser cobrado; outro marco continua com fio/ação. Pergunta cobra marco, célula julga dia. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Reapontar a última resolvedora de A para B. | Dívida conserva porta alcançável; não existe obrigação de uma pergunta por chamado duplicado. (`app/docs/CORTES.md:253`; `app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| Morte por leitura com competência trancada. | Pergunta sai da fila, resposta fica na trilha e decisão segue ao DP; isso não planta batida nem absolve o dia. (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1142`) |
| Morte por leitura de marco atendido, ainda havendo outro furo. | Não recriar pergunta do marco atendido; conservar cobertura do resto pela lâmpada. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Resposta discordante ou sem hora comparável, sem decisão explícita do admin. | Continua viva para decisão humana; não transformar discordância em quitação. (`app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1158`) |
| Empresa com corte 21, competência 10. | Janela é **21/09→20/10**, não mês civil; 21/10 pertence à seguinte. Usar corte da empresa, nunca 21 fixo. (`app/docs/LEIS.md:77`; `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:54`) |
| Turno 22h→06h ancorado ontem; observar 00h01 ou 06h10. | Meia-noite não encerra fato; saída ainda não vence antes de 06h30, usando margem vigente 30 min e marcos encadeados. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:834`; `app/ponto/services/cartorio.py:846`; `app/chamados/services/fabrica_da_celula.py:18`) |
| Vínculo antigo gerou célula e cobre a data, embora inativo hoje. | Geradora responde; vínculo ativo atual não substitui. Fora da vigência, resposta é `None` e dono CADASTRO. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| Escala vigente, célula sem ata, tentar perguntar marco. | Esperar ata e acender indicação de falta de ata; não inventar marco/furo. `trabalha=None` não equivale a folga. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:839`; `app/docs/JUIZES.md:56`) |
| Ausência parcial sem período, caso mandado à lista admin. | Sem pergunta, sem pai, sem push; ação fica no cadastro/lista. (`app/chamados/services/fabrica_da_celula.py:77`; `app/chamados/services/fabrica_da_celula.py:101`; `app/chamados/services/disputa_emissao.py:847`) |
| `registrado`, sem ação devida. | Vivo legítimo sem cobrança; excluir do universo de defeitos antes de RED. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:25`) |
| Ausência em decisão ou silêncio manual vigente. | Suspensão legítima; não criar pergunta para zerar bruto. Ausência aprovada só fecha se a premissa morreu. (`app/chamados/services/silencio_ausencia.py:77`; `app/chamados/services/silencio_ausencia.py:80`; `app/chamados/services/silencio_ausencia.py:93`) |
| Dia regularizado sem dívida, chamado ainda vivo. | Cobrança sem premissa morre por lastro, não recebe pergunta artificial. (`app/docs/PROMPTS.md:1519`) |
| Histórico fora do horizonte, sem fato novo. | Passado é foto; esta obra não autoriza backfill recorrente. Exportada conserva gravado e diferença tem porta própria. (`app/docs/LEIS.md:145`; `app/docs/LEIS.md:125`) |

Contraexemplos críticos para D:

- **“C6 morreu ⇒ emitir pergunta” é errado**: validação, tranca, identidade, marco atendido e célula conclusiva regular são mortes legítimas; dívida residual exige outro marco/ação. (`app/chamados/juizes.py:1136`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1171`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1195`)
- **C15 verdadeiro não prova alcance**: C17 exige chamado da célula na fila ou pergunta viva no app com âncora na fila. (`app/chamados/juizes.py:1233`; `app/chamados/juizes.py:1341`; `app/chamados/juizes.py:1378`)
- **“Excluir todo `trabalha=None`” cala LIMBO devido**: só `trabalha=False` ou ata válida de outro tipo cala; parcial tem exceções próprias. (`app/docs/JUIZES.md:55`; `app/docs/JUIZES.md:58`)
- **“Sem ata: fabricar pela classificação” viola autoridade quando há escala**: a exceção de ramo só batidas é para ausência de escala; célula com marcos sem ata espera. (`app/chamados/services/disputa_emissao.py:833`; `app/chamados/services/fabrica_da_celula.py:39`; `app/chamados/services/fabrica_da_celula.py:52`)
- **Evento pós-save genérico não demonstra entrega garantida**: o chamado usa `engolir` e pode sair por julgamento em curso; a cura precisa fonte e cobertura das cinco perdas. (`app/chamados/signals.py:172`; `app/chamados/signals.py:174`)
- **Não retirar cron só por proposta**: baixa de pendente exige frota real não vazia, condição literal medida, contador no mesmo commit e reversão declarada. Sem banco/log, cumprimento dessas condições é [nao verificado]. (`app/docs/LEIS.md:139`)

A individualização dos 494 e quantos são vivos legítimos, cada via e eventual redução do universo são [nao verificado]; o anexo só fornece total declarado, sem registros. (`avais.txt`, anexo:1)

## 05/11 17:57 · sessao aa351fdaafbd · - · - · origem=Ronald (tmux)

Critério: classificar pelo conteúdo alegado, sem validar procedência.

- `divergencia-texto-codigo`: comparação explícita de comentário, help, docstring, cláusula ou promessa com o código.
- `defeito`: comportamento funcional ou ausência funcional alegados, inclusive riscos de segurança e carga sem categoria própria.
- `lacuna-de-teste-ou-selo`: teste, selo, saída ou comprovação operacional insuficientes. [nao verificado]
- `documentacao`: relato histórico ou configuração produtiva sem insumos para comprovação.
- `risco-concorrencia` e `risco-idempotencia`: somente quando o mecanismo correspondente aparece na afirmação.
- `instrumentacao`: limitações de censos, auditores, contadores e alarmes.
- `apresentacao`: informação exposta em tela, PDF, mensagem ou payload.

Exceções LA ao predominante `divergencia-texto-codigo`:

| Classe | IDs |
|---|---|
| defeito | LA-21, LA-32, LA-49, LA-50, LA-55 |
| risco-idempotencia | LA-15 |
| apresentacao | LA-34 |
| documentacao | LA-16, LA-27, LA-35, LA-53, LA-56, LA-60 |
| lacuna-de-teste-ou-selo | LA-06, LA-70, LA-76, LA-83 | [nao verificado]

LC: `lacuna` funcional deve virar `defeito`; exceções são LC-69 → `risco-idempotencia` e LC-116 → `instrumentacao`. Entre `risco`, LC-48 → `risco-idempotencia`; LC-05 e LC-98–102 → `documentacao`; restantes → `defeito`.

Exceções entre LC originalmente `divergência`:

| Classe | IDs |
|---|---|
| divergencia-texto-codigo | LC-35 |
| documentacao | LC-45 |
| apresentacao | LC-02, LC-36, LC-66, LC-71, LC-74, LC-76, LC-79 |
| instrumentacao | LC-93 |
| defeito | LC-06, LC-10, LC-12, LC-13, LC-58, LC-59, LC-70, LC-78, LC-82, LC-91, LC-92, LC-103 |

Contagem anterior às fusões nessa proposta:

| Classe | LA | LC |
|---|---:|---:|
| defeito | 5 | 97 |
| divergencia-texto-codigo | 69 | 1 |
| documentacao | 6 | 7 |
| lacuna-de-teste-ou-selo | 4 | 0 | [nao verificado]
| instrumentacao | 0 | 2 |
| risco-idempotencia | 1 | 2 |
| apresentacao | 1 | 7 |
| Total | 86 | 116 |

Não fundir apenas por compartilharem referência:

- LA-17 e LC-84: help de 720 versus derivação automática, e materializador que fornece 720, respectivamente (`ponto/models.py:875`; `app/chamados/services/materializacao.py:519`).
- LA-01 e LC-107: contradição documental da retenção versus derivação de furo somente de pausa (`docs/LEIS.md:81`; `app/folha/export.py:1124`).
- LA-70 e LC-46: comprovação histórica de alcance versus custo de consultas anteriores ao skip (`ponto/services/cartorio.py:1362`; `app/ponto/services/cartorio.py:1362`).
- LB-25 e LC-92: fallback de paridade sem foto versus escopo de vínculo diferente na leitura de folga (`app/escala/models.py:1280`).

_1919 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
