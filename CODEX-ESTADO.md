# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 16:41** · publicado em **08/11/2026 16:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 16:41 · sessao e34fb5e18f84 · - · - · origem=Ronald (tmux)

Os 11 sítios continuam nas mesmas linhas do censo; nenhum sumiu. As consultas atuais estão em `app/escala/services/furos_diarios.py:261`, `app/escala/services/furos_diarios.py:267`, `app/ponto/management/commands/detectar_par_relampago.py:205`, `app/ponto/management/commands/detectar_par_relampago.py:290`, `app/ponto/management/commands/emitir_furo_retroativo.py:53`, `app/ponto/management/commands/emitir_furo_retroativo.py:74`, `app/ponto/management/commands/reconciliar_grade.py:84`, `app/ponto/management/commands/reconciliar_grade.py:88`, `app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/supra_juiz.py:113` e `app/ponto/management/commands/supra_juiz.py:132`.

| Sítio atual = antigo | Pergunta própria | Juiz registrado para essa pergunta | Caso comparativo | RED próximo |
|---|---|---|---|---|
| `app/escala/services/furos_diarios.py:261` | Já houve batida na vida e qual foi a última? O resultado define `ultima_batida`, `nunca_bateu` e `sumiu`. (`app/escala/services/furos_diarios.py:281`, `app/escala/services/furos_diarios.py:283`, `app/escala/services/furos_diarios.py:285`.) | **nenhum**; o próprio registro diz “última batida, nunca bateu… deveria ler nenhum”. (`app/core/juizes.py:767`.) | Sem comparação com juiz registrado; não transformar `ids_com_adesao` em juiz registrado por analogia. Ele é chamado pelo `ranking_colabs`, uma pergunta de janela distinta. (`app/escala/services/furos_diarios.py:228`, `app/escala/services/furos_diarios.py:232`.) | `app/escala/tests/test_furos_diarios.py:61` |
| `app/escala/services/furos_diarios.py:267` | Houve batida na competência capaz de fazer 100% furado virar `escala_x_real`, em vez de `sumiu`? (`app/escala/services/furos_diarios.py:264`, `app/escala/services/furos_diarios.py:284`, `app/escala/services/furos_diarios.py:285`.) | **nenhum** para a classificação `escala_x_real`/`sumiu`; o registro das perguntas de tela não declara essa pergunta. (`app/core/juizes.py:694`, `app/core/juizes.py:706`; ausência de entrada específica: [não verificado].) | Inaplicável sem juiz registrado. | `app/escala/tests/test_furos_diarios.py:61` |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | Qual membro do micropar pode ser retratado por simulação, dadas as guardas e o resto do dia? (`app/ponto/management/commands/detectar_par_relampago.py:208`, `app/ponto/management/commands/detectar_par_relampago.py:214`, `app/ponto/management/commands/detectar_par_relampago.py:229`.) | **nenhum**; “o par é relâmpago?” permanece em `SEM_JUIZ`. `par_completo` responde outra pergunta. (`app/core/juizes.py:1187`, `app/core/juizes.py:228`.) | Inaplicável sem juiz registrado. A escolha local exige exatamente uma exclusão que melhore e deixe score perfeito. (`app/ponto/management/commands/detectar_par_relampago.py:244`.) | `app/ponto/tests/test_par_relampago_v3.py:48`; guardas: `app/ponto/tests/test_guardas_par_relampago.py:50` |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | Quais marcas aprovadas são adjacentes, formam micropar e permitem retirar o par sem estragar a grade? (`app/ponto/management/commands/detectar_par_relampago.py:294`, `app/ponto/management/commands/detectar_par_relampago.py:298`, `app/ponto/management/commands/detectar_par_relampago.py:309`.) | **nenhum** para adjacência/relâmpago/retratabilidade. (`app/core/juizes.py:1187`.) A subpergunta “par completo?” possui `par_completo`, registrado em `app/core/juizes.py:228`. | Para a **subpergunta de completude**, **não achei divergência**: o sítio chama o próprio juiz na linha 300, com a janela carregada pelo próprio carregador do juiz nas linhas 288–289; o juiz simplesmente executa essa janela. (`app/ponto/management/commands/detectar_par_relampago.py:300`, `app/ponto/turnos.py:2197`, `app/ponto/turnos.py:2200`.) Isso não prova igualdade da candidatura inteira, pois a lista inicial filtra `aprovada=True`. (`app/ponto/management/commands/detectar_par_relampago.py:291`.) | `app/ponto/tests/test_par_relampago_pelo_turno.py:58`; candidatura geométrica: `app/ponto/tests/test_par_relampago_v3.py:75` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | Há marcas para incluir o colaborador e quantas há em cada data local? (`app/ponto/management/commands/emitir_furo_retroativo.py:55`, `app/ponto/management/commands/emitir_furo_retroativo.py:100`.) | **nenhum** para contagem/seleção; `classificar_dia` é consumidor dessa contagem, não uma autoridade registrada dessa pergunta. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`, `app/core/juizes.py:170`, `app/docs/JUIZES.md:91`; ausência de registro da contagem: [não verificado].) | Inaplicável sem juiz registrado para a pergunta própria. A contagem crua efetivamente altera o portão de furo: exige `not batidas_no_dia`. (`app/ponto/supra_juiz.py:313`.) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | Qual a data da primeira marca, usada como início da operação? (`app/ponto/management/commands/emitir_furo_retroativo.py:75`, `app/ponto/management/commands/emitir_furo_retroativo.py:105`.) | **nenhum** para esse piso de adesão nos registros pedidos. (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:101`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado. O piso altera `pre_adocao` no classificador. (`app/ponto/supra_juiz.py:288`, `app/ponto/supra_juiz.py:334`.) | `app/ponto/tests/test_t42_furo_retroativo_celula.py:18` |
| `app/ponto/management/commands/reconciliar_grade.py:84` | Quem bateu na janela e entra no universo inicial `batted`? A seleção final une esse conjunto a chamados abertos e colaboradores em operação. (`app/ponto/management/commands/reconciliar_grade.py:95`, `app/ponto/management/commands/reconciliar_grade.py:105`, `app/ponto/management/commands/reconciliar_grade.py:106`.) | **nenhum** para esse universo de medição. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado; não supor que trocar o conjunto sempre exclui colaborador, porque há a união com `com_chamado | em_op`. (`app/ponto/management/commands/reconciliar_grade.py:106`.) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:88` | Qual a primeira marca de cada selecionado, abaixo da qual dias devem ser ignorados? (`app/ponto/management/commands/reconciliar_grade.py:90`, `app/ponto/management/commands/reconciliar_grade.py:116`, `app/ponto/management/commands/reconciliar_grade.py:136`.) | **nenhum** para piso de adesão. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado. `piso_visual` não responde esse piso do reconciliador; é explicitamente piso do espelho. (`app/ponto/tests/test_piso_nao_sobe_por_batida.py:4`.) | `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/reconciliar_grade.py:118` | Quais marcos ficaram faltando e o dia está limpo a ponto de matar o fato de um chamado? (`app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:155`, `app/ponto/management/commands/reconciliar_grade.py:266`.) | **`classificar_falta`**, registrado em `app/core/juizes.py:180` e `app/docs/JUIZES.md:91`; implementação `app/escala/servico_jornada.py:279`. | **CASO QUE MORDE**, detalhado abaixo. A guarda do juiz no ramo CREATE não protege o caminho `avaliado_limpo → mortos → RETRACT`. (`app/ponto/management/commands/reconciliar_grade.py:242`, `app/ponto/management/commands/reconciliar_grade.py:266`, `app/ponto/management/commands/reconciliar_grade.py:303`.) | `app/ponto/tests/test_reconciliar_grade_coberto.py:17` ou `app/ponto/tests/test_reconciliar_grade_sombra.py:28` |
| `app/ponto/management/commands/supra_juiz.py:113` | Há marcas na janela para incluir o colaborador e quantas há em cada data local? (`app/ponto/management/commands/supra_juiz.py:117`, `app/ponto/management/commands/supra_juiz.py:222`.) | **nenhum** para contagem/seleção. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado para a pergunta própria. O resultado cru chega a `batidas_no_dia`; os portões de furo e furo parcial o leem. (`app/ponto/management/commands/supra_juiz.py:229`, `app/ponto/supra_juiz.py:314`, `app/ponto/supra_juiz.py:437`.) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |
| `app/ponto/management/commands/supra_juiz.py:132` | Qual a primeira marca usada como piso de operação? (`app/ponto/management/commands/supra_juiz.py:135`, `app/ponto/management/commands/supra_juiz.py:227`.) | **nenhum** para esse piso nos registros pedidos. (`app/docs/JUIZES.md:20`; ausência específica: [não verificado].) | Inaplicável sem juiz registrado; o piso alimenta `pre_adocao`. (`app/ponto/supra_juiz.py:288`.) | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:97` |

Caso concreto proposto para `reconciliar_grade:118`, deduzido do código e **não executado** [não verificado]:

- Cadastro hipotético: colaborador ativo, não isento, admissão e vínculo 5x2 vigentes desde 01/07/2026; turno 08:00–17:00, sem intervalo; célula de 20/07/2026 `trabalha=True`, DNA com E08:00/S17:00; sem ausência; chamado vivo `batida_ausente` com `data_turno=2026-07-20`. Relógio em 21/07/2026 20:00−03. Batidas únicas em 20/07 E08:00/S17:00, ambas `aprovada=False`, ambas retratadas em 21/07 10:00−03. Fixture proposta [não verificado].
- **Sítio:** recebe as duas marcas pelo manager cru; a grade por turno transforma todas as marcas E/S recebidas em `locs`, sem examinar retratação. Com os marcos ocupados, não há `missing`, o dia entra em `avaliado_limpo`, e o chamado recebe plano `RETRACT`. (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/escala/utils.py:1344`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:155`, `app/ponto/management/commands/reconciliar_grade.py:266`, `app/ponto/management/commands/reconciliar_grade.py:303`.)
- **Juiz:** carrega os turnos apuráveis; sem as duas retratadas, não há marca viva; monta esperado E/S e devolve `('E', 'saida_sem_entrada', 'ENTROU')`. (`app/escala/servico_jornada.py:390`, `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:430`, `app/escala/servico_jornada.py:469`, `app/escala/servico_jornada.py:471`; exclusão de retratadas: `app/ponto/turnos.py:2864`.)
- Para tornar explícita a configuração do caso, usar `GRADE_POR_TURNO=True`: o wrapper encaminha então ao montador examinado. (`app/escala/utils.py:2107`.)

Contagem estrita do lote: **1 caso que morde; 0 sem divergência na pergunta própria integral; 0 sumidos; 10 sem juiz**. Há ainda **1 subpergunta sem divergência**, `par_completo` dentro de `varrer_pares_embutidos`, que não deve ser contada como cura da candidatura integral. (`app/ponto/management/commands/detectar_par_relampago.py:290`, `app/ponto/management/commands/detectar_par_relampago.py:300`, `app/core/juizes.py:1187`.)

## 08/11 16:41 · sessao bfe42fe62f74 · - · - · origem=Ronald (tmux)

Os nove sítios permanecem nas mesmas linhas do censo; nenhum sumiu. As referências abaixo são da árvore atual. Casos são hipóteses deduzidas do código; execução e ocorrência em produção: **[não verificado]**.

| Sítio antigo = atual | Pergunta do sítio | Juiz registrado da mesma pergunta | Caso que morde / limite | RED próximo |
|---|---|---|---|---|
| `app/ponto/propositor.py:346` | O dia está posicionalmente completo, ignorando tipos, pela atribuição global? (`app/ponto/propositor.py:325`, `app/ponto/propositor.py:351`) | **nenhum** para essa pergunta específica. O registro declara casamento de marco por `_match_marcos`, que é outro algoritmo; a função aqui chama `primeiro_furo_posicional` → `atribuir_global`. (`app/core/juizes.py:200`; `app/docs/JUIZES.md:27`; `app/ponto/propositor.py:315`; `app/ponto/propositor.py:351`) | Comparação com juiz registrado: inaplicável. Não elevar `primeiro_furo_posicional` ou `atribuir_global` a juiz registrado. (`app/ponto/propositor.py:351`; `app/core/juizes.py:200`) | `app/ponto/tests/test_furo_posicional.py:6` |
| `app/ponto/services/flip_auto.py:35` | Quais batidas adjacentes têm mesmo tipo e distância inferior a 12h na janela? (`app/ponto/services/flip_auto.py:31`, `app/ponto/services/flip_auto.py:43`) | **nenhum**. `decidir_tipo_estrito` responde se um tipo deve virar, não quais pares adjacentes entram no inventário. (`app/core/juizes.py:198`; `app/ponto/services/flip_auto.py:41`) | Comparação inaplicável. A posterior chamada à porta apenas fornece `n_bat` a `classificar_par`; não refaz a adjacência. (`app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`) | `app/ponto/tests/test_flip_auto_pergunta_ao_juiz.py:78` (exercita fila S-S) |
| `app/ponto/services/flip_auto.py:291` | O dia está perfeito antes/depois dos flips, condicionando auto/humano? (`app/ponto/services/flip_auto.py:303`, `app/ponto/services/flip_auto.py:305`) | **nenhum** para “dia perfeito” no registro exigido. Existe `dia_perfeito`, chamado pelo sítio, mas seu título de “juiz único” está somente na própria docstring. (`app/ponto/services/simular_dia.py:1`; `app/ponto/services/simular_dia.py:11`; registro de marcos: `app/core/juizes.py:200`) | Comparação com juiz registrado dessa pergunta: inaplicável. Não dizer que diverge de `decidir_tipo_estrito`: o sítio chama exatamente esse decisor para os flips; o universo cru só permanece na grade. (`app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:300`; `app/ponto/services/flip_auto.py:303`) | `app/ponto/tests/test_flip_auto.py:7` |
| `app/ponto/services/geofence.py:51` | Quantas das dez últimas marcas com GPS caem no raio do posto e sustentam divergência do vínculo? (`app/ponto/services/geofence.py:37`, `app/ponto/services/geofence.py:56`, `app/ponto/services/geofence.py:67`) | **nenhum** para contagem/evidência do vínculo. `classificar_posicao` está registrado para a posição de **uma batida**, não para esse agregado. (`app/core/juizes.py:1178`; `app/ponto/services/geofence.py:95`) | Comparação inaplicável. O ramo individual “dentro” é geometricamente igual a `classificar_posicao`: ambos usam distância ≤ raio; isso não registra um juiz para a seleção das dez marcas nem para o limiar de 80%. (`app/ponto/services/geofence.py:58`; `app/ponto/services/geofence.py:67`; `app/ponto/services/geofence.py:114`) | `app/ponto/tests/test_reconciliar_vinculo.py:49` |
| `app/ponto/services/veredito_celula.py:215` | O dia já está completo e qual combinação de flips/plantios o deixa perfeito? (`app/ponto/services/veredito_celula.py:220`, `app/ponto/services/veredito_celula.py:223`, `app/ponto/services/veredito_celula.py:271`) | **nenhum** para essa pergunta composta. `dia_perfeito` existe, mas não está no registro exigido; `_match_marcos` só é registrado para casamento de marcos. (`app/ponto/services/simular_dia.py:1`; `app/core/juizes.py:200`; `app/docs/JUIZES.md:27`) | Comparação com juiz registrado da mesma pergunta: inaplicável. O sítio já usa `grade_dia` e `dia_perfeito`; não inventar divergência dele com essas mesmas funções recebendo o mesmo universo. (`app/ponto/services/veredito_celula.py:216`; `app/ponto/services/veredito_celula.py:218`; `app/ponto/services/veredito_celula.py:219`) | `app/ponto/tests/test_veredito_celula.py:45` |
| `app/ponto/services/vinculo_na_lista.py:49` | A evidência GPS aponta vínculo divergente, coordenada errada, itinerância ou posto ausente? (`app/ponto/services/vinculo_na_lista.py:86`) | **nenhum** no registro exigido. O sítio chama `classificar_evidencia`, denominado juiz pelo próprio módulo, mas esse nome não aparece em `core/juizes.py` nem em `docs/JUIZES.md`. A distinção entre agregado e posição individual aparece nas assinaturas. (`app/ponto/services/vinculo_evidencia.py:48`; `app/ponto/services/geofence.py:95`; `app/core/juizes.py:1178`) | Comparação inaplicável. Já delega o veredito a `classificar_evidencia` com os números coletados; não há divergência inventável com a mesma chamada e mesmos números. (`app/ponto/services/vinculo_na_lista.py:86`; `app/ponto/services/vinculo_evidencia.py:48`) | `app/ponto/tests/test_c2_vinculo_vira_lista.py:63` |
| `app/ponto/utils.py:186` | Há turno aberto que impede oferecer o colaborador para cobertura? (`app/ponto/utils.py:183`; `app/ponto/utils.py:192`) | **`turno_aberto_de`**, `app/ponto/turnos.py:2281`; registro `app/core/juizes.py:199`. | **Hipótese:** col01 ativo, perfil colaborador, mesma empresa, sem demissão, férias ou ordem ativa; vínculo/célula 06/11/2026 08–17, sem intervalo; única batida E 08:00, retratada 08:30; consultar cobertura 09–17, relógio 09:00. Sítio: **exclui**, porque a E crua está nas 14h e entra em `com_turno_aberto`. Juiz: **`None`**, pois a única marca é excluída antes do pareamento. (`app/ponto/utils.py:187`; `app/ponto/utils.py:191`; `app/ponto/utils.py:192`; `app/ponto/turnos.py:2217`; `app/ponto/turnos.py:2219`; `app/ponto/turnos.py:2222`; `app/ponto/turnos.py:2276`) | **`app/ponto/tests/test_motor_substituicao.py:208`** — testa diretamente esse helper. |
| `app/ponto/utils.py:244` | A próxima batida é E ou S? (`app/ponto/utils.py:240`) | **`proximo_tipo_de`**, `app/ponto/turnos.py:2448`; registro `app/core/juizes.py:197`. | **Hipótese:** col02 com vínculo ativo de ciclo **intermitente**; única marca E em 06/11/2026 08:00, retratada 08:30; relógio 09:00. Sítio: **S**, pois alterna a última marca gravada sem janela/filtro. Juiz: **E**, pois o ramo intermitente busca só marcas vivas das últimas 14h; encontra nenhuma. (`app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/ponto/turnos.py:2464`; `app/ponto/turnos.py:2467`; `app/ponto/turnos.py:2471`) | **`app/ponto/tests/test_proxima_batida.py:23`** |
| `app/ponto/views.py:1598` | Qual entrada pertence ao dia da justificativa? Seleciona E pelo dia civil e grava a associação. (`app/ponto/views.py:1598`; `app/ponto/views.py:1629`; `app/ponto/views.py:1637`) | **`turnos_do_colab`**, `app/ponto/turnos.py:1848`; registro da geometria/data_turno `app/core/juizes.py:196`, `app/docs/JUIZES.md:20`. **Observação de fronteira:** o registro é da geometria do turno; não há juiz específico para escolher o FK da justificativa. | **Hipótese:** col03, vínculo/célula 06/11/2026 08–17, única marca E 08:00 retratada 08:30; POST em 08/11 com `data_turno=2026-11-06`, sem justificativa prévia. Sítio: grava `j.batida_entrada` apontando a marca retratada. Geometria do juiz para 06/11: não contém essa batida nem entrada, pois a consulta a exclui. (`app/ponto/views.py:1598`; `app/ponto/views.py:1635`; `app/ponto/views.py:1637`; `app/ponto/turnos.py:1861`; `app/ponto/turnos.py:1862`; `app/ponto/turnos.py:1913`) | **`app/ponto/tests/test_vinculo_justificativa_cross_day.py:30`** |

Contagem desta fatia, **se a escolha da entrada for tratada como geometria do turno**: **3 com caso, 0 sem divergência, 0 sumidos, 6 sem juiz**. Se a pergunta exigir literalmente o juiz específico do FK da justificativa, a última linha passa a **nenhum**, resultando em **2 com caso e 7 sem juiz**. A fronteira decorre da pergunta registrada (`app/core/juizes.py:196`) e da gravação efetuada (`app/ponto/views.py:1637`).

## 08/11 16:40 · sessao da69c2b5513e · - · - · origem=Ronald (tmux)

Os oito sítios continuam nas linhas informadas no censo; os trechos atuais mostram as consultas nos mesmos endereços. (`app/chamados/management/commands/auditar_disputas.py:103`; `app/chamados/services/explicador.py:40`; `app/chamados/services/explicador.py:53`; `app/chamados/services/explicador.py:132`; `app/chamados/services/materializacao.py:94`; `app/chamados/services/materializacao.py:100`; `app/core/views.py:360`; `app/core/views.py:793`.)

| Sítio atual | Pergunta própria | Juiz registrado | Caso/comparação | RED próximo |
|---|---|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | Esta batida retroativa tem tipo invertido? (`:119-120`) | `decidir_tipo_estrito`, `app/ponto/turnos.py:2395`; registro `app/core/juizes.py:198`. | **Não achei divergência.** Chama exatamente `decidir_tipo_estrito(mk, localtime(timestamp), tipo)`, com os marcos do vínculo vigente naquele dia (`auditar_disputas.py:111-120`). O juiz é puro e não recebe nem filtra um universo de batidas: recebe esses mesmos três insumos (`app/ponto/turnos.py:2395`). A presença de retratada na consulta não faz o decisor discordar de si próprio. | `app/chamados/tests/test_auditar_disputas_flip_pelo_juiz.py:21`. |
| `app/chamados/services/explicador.py:40` | Existe vizinha ±20 min do slot e qual é seu tipo, para explicar a ausência de prova? (`:34-41`, `:61-65`) | **nenhum** para esse diagnóstico; os registros de pergunta viva e fio vivo são outras perguntas (`app/core/juizes.py:266`; `app/core/juizes.py:275`). | Sem comparação com juiz registrado. O diagnóstico pode dizer “batida existe, mas juiz não reconhece” inclusive quando a vizinha está retratada, pois a consulta não a exclui (`:40`, `:65`); isso não demonstra divergência com um juiz registrado da mesma pergunta. | `app/chamados/tests/test_explicador_disputa.py:65`. |
| `app/chamados/services/explicador.py:53` | Houve marcação na janela estendida do dia que justifique encaminhar o diagnóstico para cadastro? (`:47-59`, `:98-100`) | **nenhum** para causa/dono desse diagnóstico; `pergunta_viva` responde vida da pergunta (`app/core/juizes.py:112`, `:266`), não causa/dono. | Sem comparação com juiz registrado. A existência crua em `:53-55` basta para produzir `CADASTRO|…corrigir a escala` em `:56-59`; não existe autoridade registrada para esse encaminhamento nos registros consultados (`app/core/juizes.py:170`; `app/docs/JUIZES.md:19`). | `app/chamados/tests/test_explicador_disputa.py:89`. |
| `app/chamados/services/explicador.py:132` | Quais marcas alimentam os diagnósticos de todas as perguntas da disputa? (`:125-133`) | **nenhum** para diagnóstico/dono. Para a pergunta distinta “alguma pergunta está viva?”, existe `fio_vivo`, `app/chamados/juizes.py:1233`, registrado em `app/core/juizes.py:275`. | Quanto ao diagnóstico, sem comparação com juiz registrado. **Quanto ao fechamento, não achei divergência:** `pode = not fio_vivo(ps)` usa a mesma função e o mesmo universo `ps`; `_bts` não participa desse booleano (`explicador.py:121-138`). Não transformar a consulta de diagnósticos em um segundo veredito de fechamento. | `app/chamados/tests/test_explicador_disputa.py:65`; igualdade do fechamento também cabe em `:80`. |
| `app/chamados/services/materializacao.py:94` | A vizinha cronológica anterior na janela de 12 h tem o mesmo tipo do plantio e o barra? (`:85-99`) | **nenhum**. `decidir_tipo_estrito` decide flip por marcos, não adjacência/guarda de plantio (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`). | Sem comparação com juiz registrado. Exemplo do comportamento cru: plantio S19:00, única anterior S15:00 retratada → devolve essa S e barra, pois não lê `retratada_em` (`materializacao.py:94-99`). Não há resposta de juiz registrado da mesma pergunta para contrastar. | `app/chamados/tests/test_guarda_paridade_materializador.py:59`. |
| `app/chamados/services/materializacao.py:100` | A vizinha cronológica posterior na janela de 12 h tem o mesmo tipo do plantio e o barra? (`:100-105`) | **nenhum**, pela mesma distinção de pergunta (`app/core/juizes.py:198`; `app/ponto/turnos.py:2395`). | Sem comparação com juiz registrado. Exemplo do comportamento cru: plantio S12:00, nenhuma anterior, única posterior S15:00 retratada → devolve essa S e barra (`materializacao.py:100-105`). Não eleger o juiz de flip como juiz dessa guarda. | `app/chamados/tests/test_guarda_paridade_materializador.py:59`. |
| `app/core/views.py:360` | Há entrada nas últimas 24 h que retire o colaborador da lista de ausentes? (`:357-363`, `:388-405`) | Para a pergunta estrita de existência de entrada: **nenhum**. Para o julgamento do dia, o registro marca esse trecho como pendente de `_TT3`, cuja autoridade é `classificar_falta` (`app/core/juizes.py:696`, `:709`, `:741-742`). | **Caso se consolidado como julgamento de ausência/furo:** colab ativo visível; vínculo 6x1 08–17 sem intervalo, vigente; dia útil com `trabalha=True`, sem ausência; relógio 17/09/2026 18:00 local; única E07:00 retratada. Sítio: inclui a E e pula o colab em `:399`, não o lista ausente (`views.py:360`, `:399`). Juiz: após fim previsto (`servico_jornada.py:375-378`), universo vem de turnos sem retratadas (`:386-404`), esperado E/S (`:430-435`), vazio ⇒ `('E', 'saida_sem_entrada', 'ENTROU')` (`:468-472`). É dedução de leitura, sem execução. Se a comparação exigir literalmente “teve entrada nas últimas 24 h”, usar **nenhum**, sem generalizar o juiz de furo. | `app/core/tests/test_supervisor_turnos_pelo_juiz.py:60`; fixture de retratada próxima em `app/core/tests/test_e4_7_painel_entrada_retratada.py:59`. |
| `app/core/views.py:793` | Houve atividade hoje, para classificar cobertura do posto? (`:865-866`) | **nenhum** para essa pergunta. `turno_aberto_de` é autoridade de turno aberto vivo agora, pergunta distinta (`app/core/juizes.py:694`, `:707`). | Sem comparação com juiz registrado. Com posto contendo colab, `previsto=0`, `presente=0` e só batida retratada hoje, o cru produz `teve_atividade=True` (`views.py:793`, `:865`) e `classificar_cobertura` retorna `encerrado` (`app/ponto/selecao_periodo.py:237-243`). Essa função não está registrada como juiz nesses registros; não inventar resposta autoritativa alternativa. | `app/core/tests/test_painel_tv_turno_pelo_juiz.py:67`. |

Contagem deste lote, usando a pergunta estrita do sítio: **0 com caso comparável que morde; 1 sem divergência; 0 sumidos; 7 sem juiz**. Se a consolidação adotar o julgamento `_TT3` explicitamente atribuído ao supervisor no registro, a linha `core/views.py:360` passa a **1 caso que morde**, e os sem juiz caem para **6**. A ressalva decorre da diferença entre “existiu entrada nas últimas 24 h” e “o dia acusa falta/furo”, explicitada acima. (`app/core/views.py:360`; `app/core/views.py:399`; `app/core/juizes.py:696`; `app/core/juizes.py:741-742`.)

## 08/11 16:40 · sessao 80bb75efeb0a · - · - · origem=Ronald (tmux)

Censo estático: `test_o280_bateria_da_lei.py` possui **138 IDs em CASOS** (117 `mes`, 8 `periodo`, 13 `fora`) e **165 métodos `test_`**; `test_bateria_lavratura.py` possui **52 métodos**. Contagem por AST, sem importar a aplicação nem executar testes; fontes: `app/ponto/tests/test_o280_bateria_da_lei.py:278`, `:1073`, `:5991`; `app/ponto/tests/test_bateria_lavratura.py:169`, `:1549`.

Inventário de **oráculos positivos inscritos**, deduplicados por ID, excluindo gêmeos medidos e perguntas abertas:

| Rubrica | Casos positivos | Observação |
|---|---:|---|
| HE50 | **0** | A6 e H2 recusam definir as faixas sem lastro; único positivo `he50` direto é B-I28 medido. `app/ponto/tests/test_o280_bateria_da_lei.py:2206`, `:5117`, `:5994`. |
| HE100 não feriado | **0** | H2 afirma somente total e soma das partes, sem fixar 120/840. `app/ponto/tests/test_o280_bateria_da_lei.py:5991`. |
| HE100 feriado | **8 IDs / 10 métodos** | F10, F10b, F10c, F11, F11r, F11b, F11c, F11d. Normas individualizadas em `app/ponto/tests/test_o280_bateria_da_lei.py:305`, `:309`, `:314`, `:340`, `:346`, `:352`, `:356`, `:362`. |
| FT | **5 cenários / 7 métodos** | F3, F3b, F8, B-O263FT e cenário sem ID `test_v3_a_folga_trabalhada_segue_a_familia_1`; estes afirmam sobretudo minutos do motor, não necessariamente rubrica final da folha. `app/ponto/tests/test_o280_bateria_da_lei.py:1386`, `:1783`, `:2041`, `:2702`, `:4092`, `:4514`, `:4540`. |
| Noturno | **12 IDs / 16 métodos** | F2, F4, F4b, F4c, F10i, F11, F11r, F11b, F11c, F11d, B-O263, I26-H1. `app/ponto/tests/test_o280_bateria_da_lei.py:1330`, `:1399`, `:1420`, `:1446`, `:1628`, `:1654`, `:1675`, `:1697`, `:1714`, `:1769`, `:2689`, `:5966`. |
| Intra | **5 IDs / 5 métodos decididos** | B-O272b, B-SOBRA, F7d, F7e, B-P4b; F2b separado como aberto, com dois pinos. `app/ponto/tests/test_o280_bateria_da_lei.py:2552`, `:2760`, `:3738`, `:3803`, `:5253`, `:1354`, `:3607`. |
| Atraso + antecipada | **27 IDs / 29 métodos** | Relação completa enviada nas mensagens anteriores; inclui I26-D4, cujo esperado varia na tabela. `app/ponto/tests/test_o280_bateria_da_lei.py:976`, `:5710`. |
| Falta diurna exportada | **0 diretamente** | Há 2 IDs com literal positivo de **falta total**, F-FOTO e F-FOTOb; não conferem separação diurna/noturna. F1/F1b e A1 usam a própria autoridade de previsto para construir o esperado. `app/ponto/tests/test_o280_bateria_da_lei.py:1303`, `:1316`, `:2058`, `:2090`, `:2146`. |

O censo acima **não é o censo de todos os zeros**; a extração dos zeros enviada antes identifica chamadas diretas, mas não cobre integralmente dicionários intermediários, tabelas e asserções fora de `confere`. Portanto, não declarar aqueles zeros como inventário exaustivo. [não verificado]

As quatro propriedades:

- **Partes somam:** `test_P4_contador_igual_universo` e `test_v3_P4_contador_igual_universo`; lavratura também possui `test_PROPRIEDADE_as_partes_somam_o_total_em_A/B`. `app/ponto/tests/test_o280_bateria_da_lei.py:2874`, `:5463`; `app/ponto/tests/test_bateria_lavratura.py:1483`, `:1486`.
- **Idempotência:** `test_P2_idempotencia_na_mesma_instancia`, `test_v3_P2_idempotencia_da_releitura`; lavratura `test_PROPRIEDADE_lavrar_duas_vezes_e_uma_vez_em_A/B/F/G`. `app/ponto/tests/test_o280_bateria_da_lei.py:2801`, `:5408`; `app/ponto/tests/test_bateria_lavratura.py:1500`, `:1503`, `:1506`, `:1509`.
- **Previsto independente do realizado:** fora dos dois arquivos, existe `test_idempotente_e_previsto_nao_depende_do_realizado`, que cobra 440 antes e depois da mudança das batidas. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:236`.
- **Todo leitor mesmo número:** cobertura parcial. F11c confere FechamentoMensal e DiaPago; testes por fatia conferem espelho e PDF, em outro cenário. Não achei teste único ligando espelho, fechamento, export e DiaPago com resposta da regra; C7 do próprio núcleo está declarado `fora`. `app/ponto/tests/test_o280_bateria_da_lei.py:610`, `:1749`; `app/ponto/tests/test_regua_por_fatia_na_janela.py:186`, `:204`. O teste do export admite não atravessar `medir`/TXT. `app/folha/tests/test_porta_do_export.py:224`.

Alertas para consolidação: A6 atualmente contém um pino legal aberto/zero e testes antigos afirmando 1020/60/300; estes últimos são medição, segundo o próprio CASOS. A6b é medido/pergunta aberta. F2b, F7g e F10h têm pergunta aberta; C10 guarda comportamento atual. Não tratá-los como resposta legal decidida. `app/ponto/tests/test_o280_bateria_da_lei.py:457`, `:474`, `:1354`, `:426`, `:329`, `:639`.

## 08/11 16:38 · sessao 253d5dbeaf26 · - · - · origem=Ronald (tmux)

Confirmado: **sem aplicação e empate resultam em 548,57 minutos legais**, para 19–07 com pausa01–02. A regra registrada mantém a prorrogação quando CLT não está declarado: `(540−60) × 60/52,5 = 548,571428 min`. `app/docs/LEIS.md:83`; `app/core/regua_cct.py:192`; `app/core/regua_cct.py:204`.

Fontes exatas, para empresa `EMP`:

- `legal (empresa cct, sem aplicacao de convencao: EMP)`
- `legal (empresa cct, empate de convencao: EMP)`

`app/core/regua_cct.py:371`.

**SINEEPRES:** referências à cl.10 e à restrição22–05 existem, mas texto literal integral não achei; cabe **DUVIDA sem número**. `app/core/views_config.py:293`; `app/ponto/motor_calculo_v2.py:745`; `app/core/models.py:72`.

**8792:** usar ausência decidida cujo tipo desconta; zero batidas, sozinho, não estabelece a rubrica. `app/docs/LEIS.md:87`.

## 08/11 16:38 · sessao ae3c2c50aa96 · FIO-E0-V4-E2-FUNCAO · leitura · origem=Code

**1. MATRIZ DE TRANSIÇÕES**

Referências abreviadas:

- `T` = anexo `predesenho_e2_tarefa_do_dia_e93205c6.py`, numerado desde a primeira linha.
- `P` = `app/chamados/tests/test_e2_tarefa_do_dia.py` do diff; a coluna de prova identifica o teste escrito. Execução dos testes: [nao verificado].
- `E1` = arquivo da árvore fornecida; `E2` = arquivo de destino do diff.
- `NS↑` = define `nao_sei=True`; `[]` = nenhuma ação. Nos ramos por instância, outra instância pode já ter definido `nao_sei=True`. (`T:173`, `T:246`, `T:283`)
- `porta=P` = chamado retornado por `chamado_que_lista`, se estiver na fila; caso contrário, `None`. (`T:122`, `T:123`)

As linhas abaixo seguem a precedência do código. Os retornos antecipados do corpo acontecem **depois** da montagem da carga e da obtenção de grade, instâncias, marcos e cobertura. (`T:308`, `T:396`, `T:399`)

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| `agora=None`, argumento da porta (`T:416`) | Nenhum consultado | Levanta exceção | `ValueError` (`T:417`) | `SeloDeterminismoERelogio.test_MORDE_sem_agora_recusa` |
| Sem `carga` (`T:418`) | Resultados carregados para um colaborador e um dia | Abre carga e delega | Resultado de `carga.tarefa` (`T:419`, `T:422`) | `SeloDeterminismoERelogio.test_determinismo_e_lote_igual_a_casca` |
| Carga fornecida, mesmo `agora` (`T:420`) | Usa o instante da carga | Delega | Resultado de `carga.tarefa` (`T:422`) | `SeloDeterminismoERelogio.test_determinismo_e_lote_igual_a_casca` |
| `carga.agora != agora` (`T:420`) | Nenhum novo julgamento | Levanta exceção | `ValueError` (`T:421`) | sem teste | [nao verificado]
| Dia fora de `[carga.ini,carga.fim]` (`T:394`) | Nenhum julgamento desse dia | Levanta exceção | `ValueError` (`T:395`) | `Caso12Verificador.test_a_carga_so_responde_a_sua_janela` |
| Colaborador sem entrada em `_cadastro`, inclusive colaborador não carregado (`T:317`, `T:376`) | Grade não chega a ser consultada | Acesso direto ao mapa | `KeyError`; não produz `nao_sei` (`T:376`) | sem teste | [nao verificado]
| Grade sem `dias`/`isento_ponto`, ou retornando `None` (`T:376`, `T:378`) | Retorno sem o contrato esperado | Indexa o retorno sem proteção | `KeyError`/`TypeError`; não produz `nao_sei` (`T:378`) | sem teste | [nao verificado]
| Dia chega ao juiz sem `insumo_dia_sem_trabalho` ou sem suas chaves | Insumo ausente | Acesso direto às chaves | `KeyError` propagado (`T:199`; `app/ponto/supra_juiz.py:70`, `:71`) | sem teste |
| Autoridade indisponível, importação ou chamada lança exceção | Não há resposta utilizável | Não há `try/except` na porta/carga/corpo | Propaga a exceção (`T:308`, `T:347`, `T:367`, `T:376`, `T:422`) | sem teste | [nao verificado]
| Grade: `tipo_dia='fora_vinculo'` (`T:176`) | Leitor único declarou fora do vínculo | Encerra antes da isenção/tranca | Razão `fora_vinculo`; instâncias vazias; `nao_sei=False` (`T:177`) | `Caso13VinculoEIsencaoPeloLeitorUnico.test_demitido_na_vespera_sem_fato_nao_deve_nada`; `test_admitido_amanha_sem_fato_nao_deve_nada` |
| Grade: isenção verdadeira, sem retorno anterior (`T:180`) | Leitor único declarou isenção | Conserva instâncias, esvazia ações | `isento_ponto`; sem ações (`T:181`) | `Caso13VinculoEIsencaoPeloLeitorUnico.test_isento_de_ponto_nao_deve_marco` |
| Par `(colab,dia)` presente em `_trancado` (`T:367`, `T:187`) | `dias_trancados` verdadeiro | Conserva instâncias, esvazia ações | `competencia_trancada`; sem ações (`T:190`) | `Caso14DiaArquivadoPelaTranca.test_dia_trancado_nao_tem_tarefa` |
| Competência reaberta, ou toque humano posterior reconhecido (`T:367`) | Juiz da tranca falso | Segue o julgamento normal | Não retorna `competencia_trancada` (`T:187`) | `Caso14DiaArquivadoPelaTranca.test_reaberta_nao_tranca`; `test_desarquivado_pelo_O44_e_julgado_como_qualquer_dia`; `test_cobrar_e_resolver_dia_desarquivam_so_aquele_dia` |
| Juiz da tranca não encontra período fechado | Ausência de tranca resulta em falso | Segue o julgamento normal | Demais ramos (`E1 app/chamados/juizes.py:965`; `T:187`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| `gdia=None`, ou grade declara `sem_celula` (`T:196`) | Dia ausente/sem célula no leitor único | Encerra | `sem_celula`; instâncias vazias; `NS↑` (`T:197`) | `Caso2NoturnoSemIrA23.test_a_pergunta_da_saida_e_do_turno_de_21_pelo_dia_do_turno` — parcial: verifica célula e instâncias, não a razão/NS |
| Dia de trabalho sem instâncias; há marcos e nenhum venceu (`T:200`, `T:201`) | Teto: todos pendentes | Encerra | `nao_venceu`; instâncias vazias; `nao_sei=False` (`T:204`) | `CasoClassesDoDiffV6b.test_T_ata_sem_lampadas_antes_de_todo_teto_nada_e_exigivel` |
| Dia de trabalho sem instâncias; algum marco venceu (`T:200`) | Teto já exigível; leitor não fornece lâmpadas | Encerra | `ata_sem_lampadas`; `NS↑` (`T:206`) | `Caso12Verificador.test_dia_de_trabalho_sem_lampadas_e_nao_sei`; teste T acima | [nao verificado]
| Dia de trabalho sem instâncias e sem marcos (`T:200`) | Teto sem resposta: lista vazia | Mesmo ramo residual | `ata_sem_lampadas`; `NS↑` (`T:206`) | sem teste específico | [nao verificado]
| Dia sem trabalho, sem instâncias (`T:199`, `T:232`) | `dia_sem_trabalho=True` | Laço vazio | Instâncias vazias; razão geral permanece `''` (`T:173`, `T:286`) | `Caso5TrocaDePlantao.test_cada_um_deve_o_seu_turno` |
| Instância em dia sem trabalho (`T:238`) | `dia_sem_trabalho=True` | Não consulta a decisão da pergunta | `dia_sem_trabalho`; ações `[]` (`T:239`) | `Caso12Verificador.test_folga_com_lampadas_apagadas_nao_cobra` |
| Lâmpada `True` (`T:241`) | Leitor único: marco aceso | Não consulta decisão/cobertura para cobrar | `aceso`; ações `[]` (`T:242`) | `CasoClassesDoDiffV6b.test_L_lampada_acesa_responde_antes_da_pergunta` |
| Lâmpada `None`, inclusive chave `acesa_lavrada` ausente (`T:84`, `T:244`) | Leitor não sabe se aconteceu | Marca desconhecimento | `lampada_nao_sabe`; ações `[]`; `NS↑` (`T:245`) | sem teste com asserção específica desse retorno | [nao verificado]
| Lâmpada apagada sem marco correspondente por tipo/hora (`T:74`, `T:99`) | Teto ausente: `vencido=None` | Marca desconhecimento | `marco_fora_do_turno`; ações `[]`; `NS↑` (`T:248`) | sem teste | [nao verificado]
| Vínculo do dia ausente ou sem tipo de escala | Corpo do teto devolve `[]` | Sem casamento de teto; aplica os ramos anteriores conforme as lâmpadas | `marco_fora_do_turno` ou `ata_sem_lampadas`, salvo retorno anterior (`E2 app/ponto/services/cartorio.py:855`; `T:99`, `T:206`, `T:248`) | sem teste específico |
| Marco apagado ainda não vencido (`T:252`) | Teto falso | Aguarda o teto | `nao_venceu`; ações `[]` (`T:253`) | `Caso1DoisMarcosNoMesmoDia.test_o_teto_e_por_marco` |
| Agora antes do fim do turno, mas marco anterior vencido | Teto é individual por marco | Julga o marco anterior; mantém o fim pendente | Pode haver tarefa antes do fim do turno (`T:252`, `T:274`) | `Caso1DoisMarcosNoMesmoDia.test_o_teto_e_por_marco` |
| Agora depois do fim, antes de fim + 30 minutos | Teto da saída ainda falso | Aguarda | Saída `nao_venceu` (`E2 app/ponto/services/cartorio.py:859`; `T:252`) | `Caso2NoturnoSemIrA23.test_a_saida_de_22_vence_pelo_instante` |
| Agora alcançou marco + teto | Teto verdadeiro, por `<= agora` | Segue para cobertura/pergunta | Não há encerramento global por “turno terminou” (`E2 app/ponto/services/cartorio.py:859`; `T:255`) | `Caso2NoturnoSemIrA23.test_a_saida_de_22_vence_pelo_instante`; `SeloDeterminismoERelogio.test_relogio_1400_x_1401_sem_escrita` |
| Cobertura parcial lavrada (`T:208`) | `ausencia_parcial_cobra_o_resto=True` | Desconsidera cobertura geral; segue pelas lâmpadas/perguntas | Demais ramos (`T:208`) | sem teste específico | [nao verificado]
| Sem cobertura para o dia (`T:208`) | Cobertura ausente: `None` | Segue para pergunta | Demais ramos (`T:255`, `T:265`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| Cobertura aprovada, marco apagado e vencido (`T:255`) | Ausência `aprovada` | Dispensa ação | `ausencia_aprovada`; ações `[]` (`T:256`) | `Caso4Atestado.test_ausencia_sem_pergunta_pela_cobertura` |
| Cobertura aguardando documento (`T:258`) | Ausência `aguardando_documento` | Atribui ao colaborador | `ausencia_aguarda_documento`; `entregar_documento(colaborador,None)` (`T:259`) | `Caso12Verificador.test_ausencia_aguardando_documento_e_do_colaborador` |
| Outra cobertura não rejeitada (`T:262`) | Ausência ainda a decidir | Atribui ao admin | `ausencia_a_decidir`; `decidir_ausencia(admin,None)` (`T:263`) | `Caso4Atestado.test_ausencia_sem_pergunta_pela_cobertura` |
| Segunda ocorrência da posição, com pergunta na posição (`T:266`) | Identidade não separa a ocorrência | Não escolhe pergunta | `instancia_sem_identidade`; ações `[]`; `NS↑` (`T:268`) | sem teste | [nao verificado]
| Segunda ocorrência, sem pergunta na posição (`T:266`) | Nenhuma pergunta encontrada | Solicita emissão | `sem_pergunta`; `perguntar(sistema,emissor)` (`T:272`) | sem teste | [nao verificado]
| Primeira ocorrência sem pergunta (`T:274`) | Nenhuma pergunta encontrada | Solicita emissão | `sem_pergunta`; `perguntar(sistema,emissor)` (`T:275`) | `Caso1DoisMarcosNoMesmoDia.test_cada_instancia_tem_a_sua_tarefa` |
| Há perguntas candidatas (`T:265`) | Não consulta a canônica da E1; toma a última por `pk` | Define `pergunta_id` e consulta decisão | Resultado de `_pela_decisao` (`T:278`, `T:280`) | sem teste com candidatas concorrentes | [nao verificado]

A seleção de perguntas tem estes caminhos próprios; uma pergunta excluída deixa a instância seguir como `sem_pergunta` se nenhuma outra candidata permanecer. (`T:221`, `T:274`)

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| `conversa=True` (`T:322`) | Discriminador da pergunta do fato | Não carrega a conversa | Sem candidata dessa conversa (`T:321`) | `Caso12Verificador.test_a_conversa_nao_e_a_pergunta_do_marco` |
| `data_evento=NULL` ou fora da janela (`T:323`) | **Não consulta** `dia_do_turno_da_linha` | Query exclui a linha | Não há ramo `nao_sei` específico dessa pergunta (`T:321`, `T:326`) | sem teste para NULL | [nao verificado]
| Pergunta gravada no dia civil seguinte, diferente do turno pedido (`T:323`, `T:230`) | **Não relê** o dia pelo juiz do turno | Não associa à véspera | Turno pedido fica sem essa pergunta | `Caso2NoturnoSemIrA23.test_MORDE_data_evento_do_dia_civil_nao_e_relido_como_vespera` |
| Motivo fora do catálogo de marcos (`T:222`) | Catálogo não o identifica como marco | Ignora | Sem candidata desse motivo | sem teste específico | [nao verificado]
| Via fora do fato, inclusive duplicata (`T:224`) | Via atual exclui a identidade | Ignora, salvo exceção temporal da retratação | Sem candidata | sem teste específico para duplicata | [nao verificado]
| Via `motivo_retratado`, sem retratação posterior a `agora` (`T:224`) | Identidade atual excluída | Ignora | Sem candidata | `SeloHorizonteCortaOsFatosDaPergunta.test_no_presente_a_via_atual_decide_como_antes` |
| Via `motivo_retratado`, mas ato de retratação posterior a `agora` (`T:338`) | Ato ainda fora do horizonte | Permite que a pergunta participe | Decisão anterior continua aplicável (`T:224`) | `SeloHorizonteCortaOsFatosDaPergunta.test_retratacao_posterior_a_agora_nao_mata_a_pergunta_em_agora` |
| Última decisão até `agora` é retratação (`T:228`) | Sequência encerra a participação | Ignora | Sem candidata | mesmo teste anterior, leitura após a retratação | [nao verificado]
| Via limpa e resposta posterior à retratação (`T:224`, `T:228`) | Último ato é resposta | Mantém a pergunta | `aguarda_validacao` | `SeloHorizonteCortaOsFatosDaPergunta.test_no_presente_a_via_atual_decide_como_antes` |
| Decisão posterior a `agora` (`T:336`) | Ato não pertence ao horizonte | Não inclui em `_dec` | Julga pela última decisão até `agora`, ou pela ausência dela | `SeloHorizonteCortaOsFatosDaPergunta.test_decisao_posterior_a_agora_nao_existe_em_agora`; `test_ultima_decisao_e_a_ultima_ate_agora` |
| Passivo com `ato_em > agora` (`T:343`) | Autoridade da E1 devolve instante posterior | Não inclui no passivo | Não bloqueia a decisão anterior | `SeloHorizonteCortaOsFatosDaPergunta.test_passivo_posterior_a_agora_nao_existe_em_agora` |
| Batida com `criado_em > agora` (`T:355`) | Efeito ainda fora do horizonte | Não inclui em `_efeitos` | Dentro do SIM, segue sem esse efeito | `SeloHorizonteCortaOsFatosDaPergunta.test_efeito_posterior_a_agora_nao_existe_em_agora` |

Para os próximos ramos, a instância já passou por dia de trabalho, lâmpada apagada, teto vencido, cobertura e seleção de pergunta. (`T:238`, `T:255`, `T:280`) [nao verificado]

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| Pergunta no passivo (`T:124`) | `linhas_da_projecao_sem_linha`: projeção sem ato correspondente | Não inventa ação | `passivo_sem_linha`; ações `[]`; `NS↑` (`T:125`, `T:282`) | `Caso8PassivoSemInventar.test_projecao_sem_linha_e_nao_sei` |
| Nenhuma decisão até `agora` (`T:126`) | Sequência vazia | Cobra resposta | `aguarda_resposta`; `responder(colaborador,P)` (`T:127`) | `Caso1DoisMarcosNoMesmoDia.test_a_resposta_de_uma_nao_fecha_a_outra` |
| Última decisão resposta com texto vazio (`T:129`) | `texto_vazio=True` | Cobra nova resposta | `respondida_sem_texto`; `responder(colaborador,P)` (`T:131`) | sem teste | [nao verificado]
| Última decisão resposta com texto (`T:130`) | `texto_vazio=False` | Encaminha ao admin | `aguarda_validacao`; `validar(admin,P)` (`T:132`) | `Caso1DoisMarcosNoMesmoDia.test_a_resposta_de_uma_nao_fecha_a_outra` |
| Última decisão recusa (`T:133`) | Sequência: `RECUSA` | Devolve ao colaborador | `devolvida_ao_colaborador`; `responder(colaborador,P)` (`T:134`) | sem teste | [nao verificado]
| Última decisão reabertura (`T:133`) | Sequência: `REABERTURA` | Devolve ao colaborador | Mesmo retorno anterior | `CasoClassesDoDiffV6b.test_C_chamado_vivo_e_porta_nao_dono` |
| Última decisão devolução (`T:135`) | Sequência: materialização recusada | Encaminha ao admin | `materializacao_recusada`; `validar(admin,P)` (`T:136`) | `Caso3MaterializacaoRecusada.test_devolucao_volta_ao_admin` |
| Validação; lavra ausente, vazia ou indeterminada (`T:348`, `T:139`) | Sem SIM/NAO vigente | Não chama novamente o juiz caro | `veredito_sem_lavra_vigente`; ações `[]`; `NS↑` (`T:140`) | `Caso11ViaSemTestemunha.test_sem_lavra_vigente_e_nao_sei` |
| Validação; lavra vencida por insumo posterior ou expiração (`T:349`) | `esta_stale=True` | Exclui lavra utilizável | Mesmo retorno anterior | `Caso7EventoSemEntrega.test_o_efeito_e_o_veredito_do_juiz_e_nao_suposto`; `Caso12Verificador.test_lavra_com_relogio_virado_e_nao_sei` |
| Validação; `julgado_em > agora` | `esta_stale=True`: lavra futura | Exclui lavra utilizável | Mesmo retorno anterior (`T:349`, `T:140`; `E2 app/chamados/veredito_lavrado.py:199`) | `Selo0514LavraNoRelogioDaBorda.test_lavra_julgada_depois_de_agora_nao_existe_em_agora` |
| Validação; lavra vigente `nao` (`T:141`) | Juiz do fechamento lavrado: NAO | Solicita materialização | `validada_sem_efeito`; `materializar(sistema,materializacao)` (`T:142`) | `Caso7EventoSemEntrega.test_o_efeito_e_o_veredito_do_juiz_e_nao_suposto` |
| Validação; lavra vigente SIM e Batida apurável carregada (`T:143`) | SIM; efeito presente | Solicita novo julgamento da célula | `efeito_sem_lampada`; `rejulgar(sistema,cartorio)` (`T:144`) | mesmo teste anterior | [nao verificado]
| Validação; lavra vigente SIM, sem Batida apurável carregada (`T:145`) | SIM, inclusive via sem testemunha | Encerra | `encerrada_pelo_juiz`; ações `[]` | `Caso11ViaSemTestemunha.test_o_juiz_do_fechamento_responde_e_nao_a_batida` |
| Última decisão silêncio (`T:146`) | Sequência: `SILENCIO` | Encerra | `silencio_decidido`; ações `[]` (`T:147`) | `Caso6Concorrencia.test_ato_duplicado_e_a_mesma_tarefa` |
| Absorção; ausência aprovada (`T:150`) | Status atual da ausência: aprovada | Encerra | `absorvida_aprovada`; ações `[]` (`T:151`) | sem teste desse ramo: `Caso4` é interceptado pela cobertura | [nao verificado]
| Absorção; ausência rejeitada (`T:152`) | Status atual: rejeitada | Devolve ao colaborador | `absorcao_rejeitada`; `responder(colaborador,P)` (`T:153`) | `Caso4Atestado.test_absorcao_segue_a_decisao_da_ausencia` |
| Absorção; aguardando documento (`T:154`) | Status atual: aguardando documento | Atribui ao colaborador | `absorcao_aguarda_documento`; `entregar_documento(colaborador,None)` (`T:155`) | `Caso12Verificador.test_absorcao_aguardando_documento_e_do_colaborador` |
| Absorção; outro status, inclusive ausência/status ausente (`T:149`) | Status não corresponde aos três anteriores | Atribui ao admin | `absorcao_pendente`; `decidir_ausencia(admin,P)` (`T:156`) | sem teste desse ramo | [nao verificado]
| Ato não reconhecido (`T:157`) | Sequência sem interpretação neste corpo | Marca desconhecimento | `ato_desconhecido:<ato>`; ações `[]`; `NS↑` | sem teste | [nao verificado]
| Âncora ausente ou chamado fora da fila (`T:123`) | `chamado_que_lista=None`, ou fila não o contém | Mantém ação sem porta | Ação de responder/validar/decidir com `porta=None` | `CasoClassesDoDiffV6b.test_U_universo_soberano_sem_chamado_cada_dono_pela_sequencia`; `Caso12Verificador.test_a_porta_e_o_chamado_na_fila` |

Há duas distinções necessárias sobre **juiz devolvendo None/ausente**:

| Estado de entrada — leitura e origem | Resultado do juiz consultado | Operação | Retorno | Prova existente em P |
|---|---|---|---|---|
| Juiz do dia do episódio devolve `None` | A E2 **não consulta esse juiz** ao carregar as perguntas; usa `data_evento` gravado (`T:321`) | Não existe tradução desse None na tarefa | Não há retorno específico; as recusas pertencem às portas da E1 (`app/chamados/services/fio.py:1395`, `:1407`) | sem teste |
| Juiz do fechamento devolve `None` durante **lavrar**, fora da tarefa | O ternário de `lavrar` trata o retorno como falso | Lavra `nao`; se vigente e com validação, tarefa pede materialização | `validada_sem_efeito`, não `nao_sei` (`E2 app/chamados/veredito_lavrado.py:237`; `T:142`) | sem teste para o juiz retornando None |
| Juiz do fechamento lança exceção durante **lavrar** | `lavrar` captura e grava `indeterminado` | Tarefa exclui essa lavra | `veredito_sem_lavra_vigente`; `NS↑` (`E2 app/chamados/veredito_lavrado.py:239`, `:243`; `T:349`, `T:140`) | `Caso11ViaSemTestemunha.test_sem_lavra_vigente_e_nao_sei` verifica o valor indeterminado, não a falha do juiz |

A declaração “0 escritores” tem uma dependência que não permite afirmá-la incondicionalmente: o juiz da tranca chama `get_user_sistema`, cujo corpo usa `get_or_create`. O teste da tranca prepara esse usuário antes da medição; sua existência em produção não aparece no dado fornecido. (`E2 app/chamados/juizes.py:1124`; `app/core/utils.py:18`; `P::Selo14JuizEmLote._universo`; existência em produção: [nao verificado])

---

**2. O QUE O REBASE SOBRE A E1 MUDA**

“Não mudou” abaixo significa conteúdo integral igual ao blob OLD indicado no diff, conferido por hash do arquivo. “Mudou” significa conteúdo atual diferente daquele blob; histórico intermediário de edições/reversões: [nao verificado].

Os caminhos da tabela são relativos a `app/`.

| Arquivo do diff | E1 também mudou? | Encontro e significado — referências dos dois lados |
|---|---|---|
| `chamados/juizes.py` | **Sim** — `86a84167` → `8b80f4d4` | E1 usa `_dia_de_turno_da_pergunta`: conversa devolve `None`, sem competência de fato. O hunk E2 lê `data_evento` cru no lote de um, recolocando o dia civil da conversa na tranca. (`E1:332`, `:336`, `:948`; `E2:933`) |
| `chamados/management/commands/auditar_invariantes_chamados.py` | **Sim** — `e0faea82` → `e18325f6` | No trecho S6, continuam as chamadas antigas sem horizonte/agora. E2 acrescenta uma borda comum para marcos e staleness. Onde mais o arquivo dive

[truncado em 20000 de 28413 caracteres]

## 08/11 16:37 · sessao 1f01c7b4cabf · - · - · origem=Ronald (tmux)

**Lista emitida encontrada: 8 eventos, sujeitos ao cadastro de rubrica por empresa/global.** São sete eventos em horas e um em dias; os códigos FOPAG vigentes em produção não são verificáveis sem o cadastro. `app/folha/export.py:439-452`, `app/folha/export.py:732-736`, `app/folha/export.py:976-983`, `app/folha/models.py:47-54`.

| Evento exportado | Fonte em produção |
|---|---|
| `hora_extra_diurna` — HE50 | `app/folha/export.py:441` |
| `hora_extra_100_diurna` — HE100 normal | `app/folha/export.py:421`, `app/folha/export.py:442` |
| `hora_extra_feriado_diurna` — HE100 feriado | `app/folha/export.py:420-423`, `app/folha/export.py:443` |
| `folga_trabalhada_diurna` | `app/folha/export.py:446` |
| `adicional_noturno` | `app/folha/export.py:447` |
| `intrajornada_diurna` | `app/folha/export.py:448` |
| `atraso_diurno` — atraso **mais** saída antecipada | `app/folha/export.py:383-399`, `app/folha/export.py:451-452` |
| `falta_diurna` — dias decididos | `app/folha/export.py:169-185`, `app/folha/export.py:215-220`, `app/folha/export.py:732-736` |

Não confundir com os 24 itens possíveis de `EVENTO_CHOICES`: o gerador efetivo emite somente os oito acima. `app/folha/models.py:13-38`, `app/folha/export.py:722-736`.

HE noturna e hora reduzida não geram linha independente; o adicional inclui as horas noturnas trabalhadas, inclusive extras. DSR é excluído expressamente. Existem helpers de 8932, mas a montagem efetiva não os chama nem emite esse evento. `app/folha/export.py:424-437`, `app/folha/export.py:453-460`, `app/folha/export.py:193-206`, `app/folha/export.py:722-736`.

Códigos históricos/documentados: HE50 150, HE100/feriado/FT 200, adicional 25, intra 270, atraso+antecipada 8069, falta 8792; o código configurado vigente segue **[não verificado]**. Fontes: `app/folha/export.py:384`, `app/folha/export.py:430-458`, `app/folha/export.py:187-214`; HE100 global 200: `app/folha/migrations/0009_evento_he100_rubrica_200.py:9-10`; FT global passa a 200: `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`. Há demonstração explícita de intra global 243 e override por empresa 270: `app/folha/tests/test_r1_caracterizacao_rubricas_map.py:21-43`.

**Mutantes de exportação que escapam ao escopo das duas baterias.** Os arquivos `test_o280_bateria_da_lei.py` e `test_bateria_lavratura.py` não chamam os montadores de TXT/eventos; a primeira costura chama motor e calculador, a segunda cartório. A sobrevivência abaixo é conclusão de leitura; execução de mutantes: **[não verificado]**. `app/ponto/tests/test_o280_bateria_da_lei.py:128-159`, `app/ponto/tests/test_o280_bateria_da_lei.py:3538-3558`, `app/ponto/tests/test_o280_bateria_da_lei.py:4609-4636`, `app/ponto/tests/test_bateria_lavratura.py:134-145`.

| Evento | Mutação concreta | Caso mínimo que deve comparar também a emissão |
|---|---|---|
| HE50 | Substituir valor de `hora_extra_diurna` por zero. `app/folha/export.py:441` | Jornada CLT comum, uma hora extra autorizada: exige uma hora no evento HE50; art.59 §1 e L-097. `app/docs/LEIS.md:115` |
| HE100 normal | Retirar a subtração `- extra_100_feriado`. `app/folha/export.py:421` | Feriado decidido cuja HE100 toda é feriado: exigir **zero no evento normal** e total no evento feriado. Não exige decidir a faixa >2h. Regra da separação: `app/folha/export.py:420-443`; dúvida da faixa: `app/ponto/tests/test_o280_bateria_da_lei.py:2198-2206`. |
| HE100 feriado | Inverter/negar a condição que preserva dobra cadastrada no 12x36. `app/folha/export.py:422` | Par de fixtures 12x36 com sindicato `feriado_12x36_em_dobra=False/True`, exigindo ausência/presença da parcela. `app/folha/export.py:406-412`; cadastro e leitor: `app/core/models.py:56`, `app/core/regua_cct.py:477`. |
| FT | Zerar `horas_folga_trabalhada`. `app/folha/export.py:446` | Trabalho comprovado em folga com escala certa: horas em FT/100%; cortes 19 e 27/09 descritos em `app/ponto/calculador/regras.py:367-388`; rubrica global: `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`. |
| Noturno | Zerar `horas_noturnas`. `app/folha/export.py:447` | 12x36 com AplicacaoConvencao cl.38-d: hora de 60min mantém adicional, não zero. L-036: `app/docs/LEIS.md:84`. |
| Intra | Zerar `horas_intra_indenizada`. `app/folha/export.py:448` | Supressão **integral** de intervalo devido evita escolher entre integral e suprimido parcial: ambos coincidem neste caso; L-031/L-032: `app/docs/LEIS.md:79-80`. Para supressão parcial, manter DUVIDA conforme conflito declarado. |
| Atraso+antecipada | Suprimir a parcela de saída antecipada da chamada. `app/folha/export.py:451-452` | Dia com ambas as pontas devidas e não nulas; exigir soma no TXT, respeitando teto previsto−realizado. Soma da rubrica: `app/folha/export.py:383-399`; L-093: `app/docs/LEIS.md:111`. |
| Falta | Omitir `_buf.extend(montar_bloco_falta(...))`. `app/folha/export.py:735` | Um dia de falta **decidida/aprovada**, sem batidas, exige um dia e registro 11 com data; dia apenas sem batida não substitui a decisão para esta rubrica. L-039: `app/docs/LEIS.md:87`; emissão: `app/folha/export.py:169-185`, `app/folha/export.py:215-220`. |

**Normas que exigem cuidado na consolidação:**

- L-031 diz só o suprimido, mas sua própria linha declara que a leitura da flag foi adiada: não tratar descrição do motor como decisão nova. `app/docs/LEIS.md:79`.
- L-086 contém texto antigo sobre feriado previsto, explicitamente revogado na própria linha: feriado previsto dobra segundo cadastro/ciclo. `app/docs/LEIS.md:118`.
- A faixa HE100 acima de duas horas não tem lastro normativo achado na bateria; ela própria o declara. `app/ponto/tests/test_o280_bateria_da_lei.py:2198-2206`, `app/ponto/tests/test_o280_bateria_da_lei.py:3705-3712`. Use DUVIDA para um valor positivo dessa faixa.
- L-085: turno pertence ao início. `app/docs/LEIS.md:117`.
- L-035/L-036: prorrogação e redução devem seguir regime/cadastro/CCT, inclusive cl.38-d. `app/docs/LEIS.md:83-84`.
- O obstáculo declarado de construir caso completo que entra no TXT está mesmo na ponta citada pelo backlog: `app/folha/tests/test_porta_do_export.py:216-229`; não é uma prova de fechamento=export=DiaPago, porque o teste declara que passou a chamar a função de comparação isolada.

## 08/11 16:37 · sessao 253d5dbeaf26 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum motor/teste executado e nenhuma escrita realizada. [não verificado] [nao verificado]

**Normas utilizáveis na tabela**

| Regra | Evidência |
|---|---|
| Gabarito = regra aplicada a cadastro e batidas; 4 propriedades fixas | `app/docs/LEIS.md:136` |
| Fonte de previsto = grade; proibido derivador paralelo | `app/docs/LEIS.md:75` |
| Jornada pertence ao dia de início, inclusive feriado | `app/docs/LEIS.md:117` |
| Feriado previsto pode dobrar conforme ciclo/cadastro; cláusula antiga L-086 foi revogada | `app/docs/LEIS.md:118` |
| Teto de pontualidade = previsto − trabalhado; soma atraso + antecipada limitada; rateio proporcional | `app/docs/LEIS.md:111` |
| L-031 True indeniza somente suprimido, False preassinalado; registro também declara leitura da flag adiada | `app/docs/LEIS.md:79` |
| Piso legal tolerância 5 min/marcação e 10/dia | `app/core/regua_cct.py:39` |
| CLT 12x36 não prorroga noturno pós-05h; CLT comum prorroga; CCT depende do sindicato | `app/core/regua_cct.py:153` |
| Cl.38-d Vigilantes Londrina, PR000251/2026: hora 60 minutos no 12x36, adicional permanece | `app/core/regua_cct.py:127` |
| Empresa CCT sem aplicação/empate → piso legal com `fonte`; empate não escolhe pela ordem | `app/core/regua_cct.py:1`, `app/core/regua_cct.py:269`, `app/core/regua_cct.py:366` |

**Lacuna principal do recorte dos cinco casos**

A fixture base cria empresa sem `regime_trabalhista` (`app/ponto/tests/test_o280_bateria_da_lei.py:1113`), e a costura chama `get_motor` diretamente (`app/ponto/tests/test_o280_bateria_da_lei.py:146`, `app/ponto/tests/test_o280_bateria_da_lei.py:156`). Portanto os casos F4b/F4c de flags não provam a escolha pelo cadastro Empresa + AplicacaoConvencao + Sindicato. O F4c troca o fator por argumento direto e compara resultados (`app/ponto/tests/test_o280_bateria_da_lei.py:4358`). Criação de AplicacaoConvencao/Sindicato nesses dois arquivos: não achei. [não verificado]

**Casos mínimos sugeridos para a tabela 2**

| id | cadastro e batidas locais | resposta pela regra | norma |
|---|---|---|---|
| CCT-REAL-38D | 12x36 19–07, pausa 01–02; regime CCT, aplicação vigente Vigilantes, redução afastada e pós5=False, dia sem feriado; E19/S01/E02/S07 | Noturno relógio `(22–01)=180 + (02–05)=180 = 360 min`; legais `360 × 1 = 360 min = 6h`; adicional continua devido, não multiplicar as horas da rubrica por 20% | `app/core/regua_cct.py:127` |
| CCT-SEM-APLICACAO | Mesmas batidas, empresa CCT sem aplicação | Noturno relógio 360; legais `360 × 60/52,5 = 411,428571 min = 6,857142h`; `fonte` informa sem aplicação | `app/core/regua_cct.py:153`, `app/core/regua_cct.py:366` |
| CCT-EMPATE | Mesmas batidas; duas aplicações vigentes no mesmo nível | Mesmo piso: `360 × 60/52,5 = 411,428571 min`; fonte informa empate; repetir invertendo a ordem das aplicações | `app/core/regua_cct.py:269`, `app/core/regua_cct.py:366` |
| CCT-6X1 | 6x1 noturno 19–07 com pausa 01–02; empresa CCT + aplicação SINEEPRES, pós5=False | Com cláusula comprovada restringindo a 22–05, relógio360 e legais411,428571; texto primário da cl.10 não achei nesta busca; vinculação legal específica permanece **DUVIDA** | Restrição configurável descrita em `app/core/models.py:72`; regra cadastro em `app/core/regua_cct.py:166` |
| CCT-FERIADO-12X36 | 12x36 07–19, pausa12–13, empresa CCT com aplicação e sindicato feriado_12x36_em_dobra=True; feriado previsto | Jornada trabalhada `300+360=660 min`; pela regra cadastrada, dobra660 min; cláusula específica que conceda a dobra não achei, então **DUVIDA** para nome/cláusula da CCT | `app/core/models.py:49`, `app/folha/export.py:402` |
| LEI-71 | 5x2 08–17 pausa cadastral12–13, pausa realizada12–12:30; empresa CLT; sem feriado | **DUVIDA**: aplicar só suprimido pós-2017 ou critério “INTEGRAL” declarado no comentário? Não fechar número por iniciativa própria | `app/docs/LEIS.md:79`, `app/ponto/motor_calculo_v2.py:66` |
| LEI-58 | 5x2 08–17 pausa12–13; CLT; E08:06/S12/E13/S17; sem feriado | **DUVIDA**: prevalece literal 5/marcação e 10/dia ou decisão declarada 10/ponta? O caso isola marcação >5 com total diário <10 | `app/core/regua_cct.py:45`, `app/ponto/motor_calculo_v2.py:52`, `app/ponto/tests/test_o280_bateria_da_lei.py:650` |
| LEI-DEGRAU | Dia normal com excesso superior a120 minutos e pares reconhecidos | **DUVIDA**: qual cláusula cria 100% após duas horas? Art.59 só fixa mínimo50%; não inventar repartição | `app/ponto/tests/test_o280_bateria_da_lei.py:2203` |

**Correção indispensável para o inventário**

F9 começa anunciando 5/10 legal, mas os asserts reais esperam atraso **0 em08:07** e **11 em08:11**. Não contar F9 como teste de cobrança literal após cinco minutos. `app/ponto/tests/test_o280_bateria_da_lei.py:3294`, `app/ponto/tests/test_o280_bateria_da_lei.py:3351`, `app/ponto/tests/test_o280_bateria_da_lei.py:3355`.

F1 calcula o esperado pela própria autoridade de previsto, sem número legal independente; não contar como caso com gabarito aberto independente do código. `app/ponto/tests/test_o280_bateria_da_lei.py:1302`, `app/ponto/tests/test_o280_bateria_da_lei.py:1316`.

**Outras lacunas/limites**

Troca de empresa no meio da competência não tem cadastro temporal correspondente; o próprio CASOS F5b declara fora, pois colaborador tem uma empresa e a troca reescreve a janela inteira. `app/ponto/tests/test_o280_bateria_da_lei.py:379`.

A dificuldade da prova completa export/fechamento está documentada: a fixture precisa integração Domínio, fechamento com horas, células julgadas em todos os dias e rubricas homologadas; o teste referido acabou medindo função parcial. `app/folha/tests/test_porta_do_export.py:216`.

Para 8932, o ponto exporta dias aprovados dos tipos que abonam, exceto exclusões do catálogo; férias ficam em módulo próprio. `app/folha/export.py:193`, `app/folha/export.py:207`. Não tratar abonos do motor como prova automática da linha8932: essa rubrica consulta Ausencia diretamente. `app/folha/export.py:201`.

Para mutantes do export, todos os casos “pela porta real” devem ir até o TXT: mesmo F11c mede FechamentoMensal e DiaPago, sem provar emissão do TXT. `app/ponto/tests/test_o280_bateria_da_lei.py:1732`, `app/ponto/tests/test_o280_bateria_da_lei.py:1745`.

## 08/11 16:34 · sessao b38605435c83 · FIO-E0-V4-E1 · censo · origem=Code

Convenção: as referências abaixo numeram cada anexo colado desde a linha 1, incluindo linhas em branco.

**Não há suporte para encaixar todos os achados em a–e.** `∅` indica classificação não demonstrável: falta a resposta sobre custo; a exceção sem captura foi vista, mas não há triagem fornecida que demonstre descarte. (`codex_e1_v15r2.q:11`; `codex_e1_v15d_reuso.r:15`; ausência dos documentos correspondentes: [nao verificado])

| laudo | achado | classe | trecho pergunta | trecho resposta | motivo da triagem |
|---|---|---|---|---|---|
| `opus_final3_e1_76644dc8.md:6` | Reuso mantém `data_evento=D0`, com célula/destino de DIA | **(b)** | “QUEM ESCREVE e QUEM LE cada campo/valor [...] algum supoe outro significado?” (`codex_e1_v15b.q:12`) | “Reuso pode atribuir célula/destino de DIA e conservar `data_evento=D0`”; descreve também os efeitos em C6 e tranca. (`codex_e1_v15b.r:132`) | “leitores intocados pelo aval E1-VOLTA-15; residual vai a E3”. (`triagem_e1_v15b.txt:7`) |
| `opus_final3_e1_76644dc8.md:7` | Portão 15c2: FK da célula velha pode apontar data fora da medida | **(e)** | **nao ha trecho** sobre essa relação FK/data/medida. As perguntas identificam `d41caaf6` e `21e7341c`. (`codex_e1_v15w.q:1`; `codex_e1_v15r2.q:1`; `codex_e1_v15b.q:1`) | **nao ha trecho** sobre esse achado. [nao verificado] | **nao ha trecho** correspondente. [nao verificado] |
| `opus_final3_e1_76644dc8.md:8` | Origem inexistente/de outro colaborador recusada somente nas portas | **(e)** — quanto à lacuna fora das portas | **nao ha trecho** perguntando essa lacuna. A pergunta sobre admissão no fecho especifica “linha de episodio retratado pela ponte A2”. (`codex_e1_v15b.q:9`) | Há diagnóstico próximo, mas distinto: “inclusive origem inexistente ou incompatível com o colaborador”, sobre **motivo impreciso** da recusa. **nao ha trecho** diagnosticando a lacuna fora das portas. (`codex_e1_v15b.r:141`) | Para o motivo impreciso: “PROCEDENTE -> volta 15c (B)”; isso registra correção, sem demonstrar descarte da lacuna do laudo. (`triagem_e1_v15b.txt:8`) |
| `opus_final3_e1_76644dc8.md:9` | Ponte de dois dias recusada na costura → `reatas=None`, possíveis I8b/I22 | **(e)** | **nao ha trecho** sobre ponte de dois dias. A pergunta de fechamento especifica “pergunta viva sobre episodio retratado”. (`codex_e1_v15b.q:6`) | “O plano devolve `reatas=None`” aparece no percurso de **retratada**, não no contraexemplo da ponte de dois dias. **nao ha trecho** sobre este último. (`codex_e1_v15b.r:54`) | **nao ha trecho** correspondente. [nao verificado] |
| `opus_final3_e1_76644dc8.md:10` | Custo do juiz por abrir/vigia | **∅** | “Custo: resolvedora_valida agora pergunta ao juiz. Liste os chamadores [...] quais estao em laco sobre muitas perguntas, e se ha memo compartilhado.” (`codex_e1_v15r2.q:11`) | **nao ha trecho**: a resposta `codex_e1_v15r2.r` não foi fornecida. Não demonstra “Codex não viu”. [nao verificado] | **nao ha trecho** correspondente. [nao verificado] |
| `opus_final3_e1_76644dc8.md:11` | Contexto fundido herda `ultima_batida` errada | **(a)** — tema da escrita/derivação da origem presente | “QUEM ESCREVE e QUEM LE [...] batida_origem_id”. (`codex_e1_v15b.q:12`) | “Explícita ou derivada antes do helper; inserção deriva quando ausente.” **nao ha trecho** identificando a herança errada pelo contexto fundido. (`codex_e1_v15b.r:128`) | **nao ha trecho** correspondente. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:6` | Reuso com `data_evento=NULL` pula rechaveamento | **(e)** | **nao ha trecho** pedindo cobertura dos reusos que evitam a exceção. A pergunta delimita caminhos que “pode[m] receber essa excecao”; o outro pedido é censo de escritores/leitores. (`codex_e1_v15d_reuso.q:2`, `:4`) | “Reuso de `abrir`” é descrito passando por `rechavear_no_reuso`. **nao ha trecho** identificando o desvio por NULL. (`codex_e1_v15d_reuso.r:108`) | **nao ha trecho**; descarte não demonstrado. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:7` | Contador exclui a classe NULL | **(e)** | “se fica trilha/contador da recusa”; **nao ha trecho** pedindo conferir cobertura da classe NULL que evita a recusa. (`codex_e1_v15d_reuso.q:2`) | “varre linhas datadas com episódio”. **nao ha trecho** apontando a exclusão de NULL como defeito. (`codex_e1_v15d_reuso.r:88`) | **nao ha trecho**; descarte não demonstrado. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:8` | Exceção sem captura, com persistência anterior | **∅** — visto, sem descarte demonstrado | “quem a captura [...] se a transacao externa e revertida”. (`codex_e1_v15d_reuso.q:2`) | “Nenhum `except` envolve a chamada da emissão”; “Sem transação externa, permanecem gravadas.” Portanto, o diagnóstico foi registrado. (`codex_e1_v15d_reuso.r:15`) | **nao ha trecho**. Sem triagem fornecida, não cabe afirmar **(b)**. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:9` | Porta sem veto próprio de competência | **(e)** | **nao ha trecho** perguntando a localização do veto; os pedidos são propagação da exceção, comportamento anterior e censo de escrita/leitura. (`codex_e1_v15d_reuso.q:2–4`) | **nao ha trecho** sobre ausência de veto próprio. O trecho sobre recusa trata de colisão. (`codex_e1_v15d_reuso.r:98`) | **nao ha trecho**. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:10` | DRY toma travas | **(e)** | **nao ha trecho** sobre aquisição de travas pelo DRY. (`codex_e1_v15d_reuso.q:2–4`) | “chama o rechaveamento real em dry”; **nao ha trecho** diagnosticando as travas. (`codex_e1_v15d_reuso.r:88`) | **nao ha trecho**. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:11` | Ressalva `destino=None`, O261-E F1 | **(e)** | **nao ha trecho** sobre essa ressalva. (`codex_e1_v15d_reuso.q:2–4`) | **nao ha trecho** correspondente. [nao verificado] | **nao ha trecho**. [nao verificado] |
| `opus_final4_e1_b0c4297c.md:12` | Regex de `test_contract_pergunta_fato` | **(e)** | **nao ha trecho** pedindo revisão desse teste. (`codex_e1_v15d_reuso.q:2–4`) | **nao ha trecho** correspondente. [nao verificado] | **nao ha trecho**. [nao verificado] | [nao verificado]
| `opus_final4_e1_b0c4297c.md:13` | Cabeçalhos dos testes | **(e)** | **nao ha trecho** pedindo revisão documental dos testes. (`codex_e1_v15d_reuso.q:2–4`) | **nao ha trecho** correspondente. [nao verificado] | **nao ha trecho**. [nao verificado] | [nao verificado]
| `opus_final4_e1_b0c4297c.md:14` | Pontos cegos da catraca: lista em variável | **(e)** | **nao ha trecho** pedindo análise dos escapes da catraca nesta janela; pede-se conferir escritores da árvore. (`codex_e1_v15d_reuso.q:4`) | “a catraca AST [...] declara limites para escrita dinâmica”; **nao ha trecho** específico sobre lista em variável. (`codex_e1_v15d_reuso.r:112`) | **nao ha trecho**. A triagem anterior declara escape semelhante fora, mas refere-se à 15b, não demonstra triagem da janela 4. (`triagem_e1_v15b.txt:6`) |
| `opus_final4_e1_b0c4297c.md:15` | Helper recebe dia civil de conversa | **(e)** — semântica não perguntada nesta janela, embora registrada | **nao ha trecho** perguntando civil versus turno; pede-se quem escreve/lê e passagem pelo helper. (`codex_e1_v15d_reuso.q:4`) | “Portanto, conversa continua gravando **dia civil**, distinguida por `conversa=True`.” O comportamento foi registrado espontaneamente. (`codex_e1_v15d_reuso.r:107`) | **nao ha trecho** que demonstre descarte. [nao verificado] |

As parcelas “não conferido nas 4 linhas da sombra”, “não medido na frota” e “frota não medida” dependem de dado de produção; não transformam automaticamente o respectivo achado estático inteiro em **(c)**. (`opus_final3_e1_76644dc8.md:7`, `:9`, `:10`)

| janela | hash perguntado × hash reprovado | pré-verificação documentada |
|---|---|---|
| Opus 3 | `d41caaf6` nas perguntas 15w/15r2; diff `6695e972..21e7341c` na 15b × merge reprovado `76644dc8`, com 15c2 `3d8ebe87`. (`codex_e1_v15w.q:1`; `codex_e1_v15r2.q:1`; `codex_e1_v15b.q:1`; `opus_final3_e1_76644dc8.md:2`) | **Não documentada sobre o mesmo hash reprovado.** As perguntas fornecidas identificam árvores anteriores. Existência de outra consulta sobre 15c/15c2: [nao verificado]. |
| Opus 4 | Pergunta `b0c4297c` × reprovação `b0c4297c`. (`codex_e1_v15d_reuso.q:1`; `opus_final4_e1_b0c4297c.md:2`) | **Sim, consulta Codex sobre a mesma árvore**, nos três assuntos perguntados. A pergunta declara “LEITURA” e não identifica um intervalo de diff; revisão integral do diff: [nao verificado]. (`codex_e1_v15d_reuso.q:1–4`) |

Contagem das classificações acima:

| classe | opus 3 | opus 4 | total |
|---|---:|---:|---:|
| (a) | 1 | 0 | **1** |
| (b) | 1 | 0 | **1** |
| (c) | 0 | 0 | **0** |
| (d) | 0 | 0 | **0** |
| (e) | 3 | 9 | **12** |
| ∅ — classificação não demonstrada | 1 | 1 | **2** |
| **Total** | **6** | **10** | **16** |

## 08/11 16:33 · sessao 5928cfc3000d · - · - · origem=Ronald (tmux)

Cruzamento da janela 4; linhas dos anexos colados, contando linhas em branco desde 1.

| Achado (`opus_final4_e1_b0c4297c.md`) | Resultado estrito | Pergunta | Resposta |
|---|---|---|---|
| Reuso NULL pula rechaveamento (:6) | **e**, com leitura restrita do escopo | Não há trecho pedindo cobertura de todos os reusos. Q:2 limita-se a caminhos que “pode[m] receber essa excecao”; Q:4 pede censo de escritores/leitores e passagem pelo helper. | R:3 descreve a exceção; R:108 descreve o reuso pelo rechaveamento. Não há trecho identificando o bypass NULL. |
| Contador cego ao NULL (:7) | **a**, se “tema contador” for a unidade de classificação | Q:2: “se fica trilha/contador da recusa.” | R:88: “varre linhas datadas com episódio”; “Não cobre linhas sem episódio.” Não há trecho apontando a exclusão de NULL como defeito. |
| Item 1: exceção sem captura (:8) | **Nenhuma das cinco classes serve** | Q:2: “quem a captura”; “se a transacao externa e revertida”. | R:15: “Nenhum `except` envolve a chamada da emissão.”; “Sem transação externa, permanecem gravadas.” R:21: “não captura a exceção de `:444`”. Foi visto e perguntado; não existe triagem fornecida para classificá-lo como b. |
| Item 2: porta sem veto próprio (:9) | **e** | Não há trecho. | R:98 fala da recusa por colisão; não há trecho sobre ausência de veto próprio de competência. |
| Item 3: dry toma travas (:10) | **e** | Não há trecho sobre travas do dry. | R:88: “chama o rechaveamento real em dry”; não há trecho sobre aquisição de travas. |
| Item 4: destino=None (:11) | **e** | Não há trecho. | Não há trecho sobre a ressalva O261-E F1. |
| Item 5: regex contratual (:12) | **e** | Não há trecho. | Não há trecho. |
| Item 6: cabeçalhos dos testes (:13) | **e** | Não há trecho. | Não há trecho. | [nao verificado]
| Item 7: catraca/lista em variável (:14) | **e**, com escopo literal | Não há trecho pedindo revisão da catraca ou seus escapes. Q:4 pede conferir escritores da árvore. | R:112: “a catraca AST [...] declara limites para escrita dinâmica”; não há trecho específico sobre lista em variável. |
| Dois significados/conversa (:15) | **e**, se distinguir censo de semântica; caso contrário, impasse como item 1 | Não há trecho perguntando sobre significados civil/turno; Q:4 pede escritores/leitores e helper. | R:107: “Portanto, conversa continua gravando **dia civil**, distinguida por `conversa=True`.” O comportamento foi visto. Se considerar o tema presente em Q:4, nenhuma classe serve sem triagem. |

Referências acima: Q = `codex_e1_v15d_reuso.q`; R = `codex_e1_v15d_reuso.r`.

A classificação b para o item 1 seria inventada: o anexo prova que o Codex viu, mas nenhum trecho fornecido prova descarte por triagem. (`codex_e1_v15d_reuso.q:2`; `codex_e1_v15d_reuso.r:15`, `:21`)

## 08/11 16:32 · sessao 95d49bbd9f9d · - · - · origem=Ronald (tmux)

Parecer para os seis achados do opus 3:

| Achado | Classe sustentada | Evidência |
|---|---|---|
| Reuso DIA/D0 | **b** | `codex_e1_v15b.q:12` pergunta escritores/leitores e significado; `codex_e1_v15b.r:132` descreve literalmente a divergência. `triagem_e1_v15b.txt:7`: “leitores intocados pelo aval E1-VOLTA-15; residual vai a E3”. |
| Portão 15c2/FK fora da medida | **e**, pelo mecanismo específico | Não há trecho de pergunta ou resposta sobre data da FK distinta do dia velho. A ausência de medição na sombra é aspecto de produção separado. |
| Origem inválida recusada somente nas portas | **e**, pelo mecanismo específico | `codex_e1_v15b.r:141` trata **motivo impreciso**, não a lacuna fora das portas; `:81` registra recusa na porta. A triagem não descarta literalmente a lacuna atual. |
| Ponte de dois dias → `reatas=None` | **e**, pelo mecanismo específico | `codex_e1_v15b.q:6` pergunta fechamento por **retratada**; `codex_e1_v15b.r:54` responde `reatas=None` por **retratada**. Não há trecho sobre ponte de dois dias. |
| Custo por abrir/vigia | **Indeterminável na taxonomia** | `codex_e1_v15r2.q:11` pergunta explicitamente custo, laços e memo; não existe resposta fornecida. Isso impede concluir “Codex não viu”. “Frota não medida” isoladamente pertence a **c**. |
| Contexto fundido herda `ultima_batida` errada | **e**, pelo mecanismo específico | Não há trecho sobre fusão/herança. `codex_e1_v15b.q:12` faz censo amplo de campos; `codex_e1_v15b.r:128` descreve derivação de origem, sem identificar esse defeito. |

Numeração conferida em memória: `codex_e1_v15b.r` possui **147 linhas**, contando brancas e linhas de código; nenhum arquivo criado.

## 08/11 16:32 · sessao 9a6e53947e78 · - · - · origem=Ronald (tmux)

A taxonomia não é exaustiva: “Codex viu, mas não há triagem demonstrada” não cabe em nenhuma classe. O exemplo inequívoco é a exceção sem captura do opus 4: pergunta 1 e resposta 1 a registram; ausência de triagem impede (b), presença na resposta impede (a)/(d), e presença na pergunta impede (e). Classificação: **[nao verificado]**, explicando a lacuna.

- **Opus 4, reuso NULL:** prefiro **(e)**. A pergunta 1 limita-se aos caminhos que recebem `ReusoSemRechave`; esse caminho a evita. A pergunta 2 fixa cenário D0/DIA. A pergunta 3 pede censo e passagem das escritas pelo helper, não exige provar que todo reuso efetivamente escreve o dia. Classificar (a) exige ampliar o tema além do texto.
- **Opus 4, contador NULL:** **(e)** é defensável. “se fica trilha/contador da recusa” pede existência/propagação, não prova de cobertura do universo NULL. A resposta registra que o contador varre “linhas datadas”, mas não apresenta a cegueira para NULL como defeito.
- **Opus 3, custo:** o tema foi expressamente perguntado em `codex_e1_v15r2.q:11`, mas falta a resposta. Isso impede provar (a), “Codex não viu”. **(c)** cabe somente à medição de frota, expressamente ausente no laudo; não prova classificação do aspecto estático do custo.
- **Opus 3, ponte de dois dias:** resposta sobre retratada causando `reatas=None` não comprova que Codex viu a causa distinta “ponte de dois dias”. Não transferir esse diagnóstico automaticamente para (b). A medição da frota é (c); a falha estática precisa de correspondência textual própria.
- **Opus 3, origem inexistente/outro colaborador:** a resposta 15b viu a origem inconsistente e a recusa pelas portas. Entretanto, a triagem separa o ramo antigo (“coberto na porta ... fica”) da precisão do motivo (“PROCEDENTE -> volta 15c (B)”). Não tratar a aceitação da correção do motivo como descarte do achado distinto de admissão pelo fecho.
- **(d)** exige prova de “nenhum viu”, além de ausência nas perguntas. Ausência de registro nos anexos demonstra apenas ausência de registro; não autoriza essa afirmação universal.

## 08/11 16:30 · sessao 0f267e96ba3e · - · - · origem=Ronald (tmux)

Comparação integral por `git hash-object` (sem `-w`, sem escrita): os prefixos abaixo foram comparados com os `index OLD` do anexo; isso permite distinguir arquivo inteiro igual de mero contexto semelhante. Atribuição histórica de cada diferença à E1 depende do dado fornecido, pois esta cópia não tem `.git` [nao verificado].

| Arquivo | E1 mexeu desde o OLD do diff? | Comparação e significado |
|---|---|---|
| `app/chamados/juizes.py` | **Sim**, blob atual `8b80f4d4`, OLD `86a84167` | **Conflito semântico direto:** E1 lê o dia pelo discriminador `_dia_de_turno_da_pergunta`, que devolve `None` para conversa; o avulso usa esse helper. E2 troca o corpo por lote de um lendo `getattr(p,'data_evento',None)` cru, incluindo o dia civil de conversa. Preservar apenas o hunk E2 elimina essa proteção da E1. E1: `app/chamados/juizes.py:332`, `:336`, `:946`, `:948`; E2: anexo `app/chamados/juizes.py:934` (retorno `dias_trancados`, linha nova do primeiro hunk). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | **Sim**, atual `e18325f6`, OLD `e0faea82` | No trecho afetado pela E2, o S6 atual ainda chama `marcos_do_lote` sem horizonte e `esta_stale` sem `agora`; o hunk E2 acrescenta uma borda única e passa esse instante aos dois leitores. Nenhuma alteração semântica da E1 **nesse trecho** aparece em relação ao OLD do anexo; onde mais o arquivo mudou, não achei por comparação histórica integral. E1: `.../auditar_invariantes_chamados.py:1162`, `:1166`; E2: mesmo caminho `:1160`, `:1162`, `:1165` no hunk novo. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py` | **Não**, atual `31bd3c86` = OLD | Continua chamando `expira_em(p.data_evento)`; E2 passa também `hoje`, acompanhando a assinatura nova. E1: `.../backfill_veredito_lavrado.py:111`; E2: mesmo caminho `:111`. |
| `app/chamados/services/declaracao_texto.py` | **Sim**, atual `ed85ee27`, OLD `58138f03` | A regra de posição do hunk permanece inline na E1 (`primeiro=inicio`, `último=fim`, meio por E/S); E2 extrai `posicao_do_marco`. Porém a E1 também mudou a escolha da pergunta: `_segura_da_lista` agora chama `canonica_do_grupo`; o próprio comentário registra que até v5 era a última por pk. E2 ainda usa `p=ps[-1]`, podendo escolher outro alvo quando coexistirem candidatas. E1: `.../declaracao_texto.py:123`, `:128`, `:251`, `:255`, `:269`; `app/chamados/services/fio.py:1080`, `:1086`, `:1088`. E2: `.../declaracao_texto.py:75`, `:134`; `.../tarefa_do_dia.py:282`. |
| `app/chamados/services/painel.py` | **Não**, atual `2bb2cbe6` = OLD | E1 continua comparando `silenciado_ate` com `timezone.now()`; E2 acrescenta `agora` opcional. É comparação entre instantes, não datas; o estado do chamado continua atual. E1: `.../painel.py:158`, `:188`; E2: mesmo caminho `:158`, `:190`. |
| `app/chamados/services/raiox.py` | **Não**, atual `a8e4f3db` = OLD | E2 muda a data de expiração para a data local de uma borda única, corta marcos por essa borda e considera lavra futura stale; E1 usa `now().date()` e `_jul.date()`. E1: `.../raiox.py:35`, `:36`, `:39`, `:41`; E2: mesmo caminho `:35`, `:37`, `:40`, `:42`. |
| `app/chamados/services/tarefa_do_dia.py` | Novo na E2; não achei na árvore | Não existe versão E1 para comparação. O próprio diff o apresenta como arquivo novo (`/dev/null`, hunk `+1,422`). Ausência na árvore aferida por busca, sem arquivo:linha [nao verificado]. |
| `app/chamados/veredito_lavrado.py` | **Não**, atual `8fe2079b` = OLD | A E2 exige `hoje` em `expira_em` e `agora` em `esta_stale`, acrescenta `ate` aos marcos, usa datas locais e rejeita lavra futura. E1 ainda tem relógio interno e `.date()` sem conversão local. Há uma mudança de **entrada lida** mesmo com o arquivo igual: `p.data_evento` chega pelas portas novas da E1 como dia do turno; conversa continua dia civil, apesar da docstring antiga de `expira_em` associar conversa a `None`. E1: `.../veredito_lavrado.py:95`, `:107`, `:112`, `:173`, `:181`, `:182`, `:185`, `:229`; `app/chamados/services/fio.py:790`, `:800`, `:1421`. E2: `.../veredito_lavrado.py:95`, `:125`, `:187`, `:201`, `:205`, `:246`. |
| `app/escala/services/leitor_celula.py` | **Não**, atual `27c89d48` = OLD | E2 adiciona alimentação de células/vínculos e expõe `acesa_lavrada`. O contrato atual `missing` converte `acesa=None` em “não missing”; E2 transporta o triestado cru para que tarefa consiga responder `nao_sei`. A grade atual já expõe `fora_vinculo` e `isento_ponto`. E1: `.../leitor_celula.py:119`, `:120`, `:133`, `:345`, `:372`, `:376`, `:428`; E2: mesmo caminho `:137`, `:174`, `:350`. |
| `app/ponto/services/cartorio.py` | **Não**, atual `17fbc851` = OLD | E2 extrai `marcos_com_teto`; a unidade continua instante do marco mais minutos, comparado por `<= agora`. O teto padrão atual é 30 minutos; não há regra “fim do turno encerra tudo”. E1: `.../cartorio.py:828`, `:831`, `:845`, `:846`; E2: mesmo caminho `:843`, `:848`, `:859`. |
| `app/ponto/turnos.py` | **Não**, atual `164db997` = OLD | E2 extrai filtro/corpo de cobertura e acrescenta métodos de lote para vínculo, teto e cobertura. A cobertura atual é por datas, vence maior `(data,pk)`, recortada em `[ini,fim]`; `batidas_apuraveis` exclui retratadas pelo estado atual, sem reconstrução histórica. E1: `.../turnos.py:2735`, `:2755`, `:2759`, `:2771`, `:2846`, `:2866`; E2: mesmo caminho `:2757`, `:2762`, `:2770`, `:3049`, `:3056`, `:3064`. |

Referências adicionais relevantes para consolidar o bloco 2:

- **O ponto único não grava Chamado:** `gravar_dia_do_fato` declara e atribui especificamente `PerguntaDisputa.data_evento`; não salva. Seus chamadores salvam no mesmo ato. Não achei campo `ChamadoColaborador.data_evento`. `app/chamados/services/fio.py:790`, `:791`, `:796`, `:800`, `:801`; campo da Pergunta: `app/chamados/models.py:2383`. Ausência do campo no Chamado [nao verificado].
- **Dia gravado já não basta para identidade no acervo:** E1 `_dia_do_fato` lê episódio pelo juiz `_T1`, recusa episódio retratado e distingue chave gravada de identidade; `dia_do_turno_da_linha` centraliza o resultado usado pelos escritores e pela costura. A E2 filtra e agrupa por `data_evento` cru, sem chamar esse juiz/fecho; não se pode presumir que o acervo tenha sido migrado. `app/chamados/services/fio.py:1308`, `:1313`, `:1316`, `:1320`, `:1412`, `:1437`; anexo E2 `app/chamados/services/tarefa_do_dia.py:240`, `:324`, `:329`.
- **Canônica:** resposta do colaborador vence; empate, menor pk. A E2 usa maior pk. Isso é divergência entre leituras escritas, não pergunta sem lei. `app/chamados/services/fio.py:1080`, `:1083`, `:1088`; anexo E2 `app/chamados/services/tarefa_do_dia.py:282`.
- **Instância:** E1 ainda nasce pela posição derivada do motivo, sem índice da segunda pausa; o comentário explicitamente deixa a chave que separa pausas à E2 quando o emissor a passar. `app/chamados/services/declaracao_texto.py:48`, `:50`, `:55`; `app/chamados/models.py:2391`, `:2394`. A E2 já cria rótulos `int_ida:2`, mas busca candidatas por `i['pos']` e declara `instancia_sem_identidade` quando há candidatas. Anexo E2 `app/chamados/services/tarefa_do_dia.py:64`, `:266`, `:267`.
- **Fuso configurado:** São Paulo, `USE_TZ=True`. `app/config/settings/base.py:93`, `:95`.
- **Juiz None de episódio na E1:** `dia_do_turno_da_batida` pode devolver `None`; a porta `recusa_batida_retratada` traduz as causas em exceção. A E2 não consulta esse juiz para classificar a pergunta carregada: não tem um ramo específico para esse None. `app/ponto/turnos.py:2917`, `:2920`, `:2926`; `app/chamados/services/fio.py:1395`, `:1401`, `:1409`; anexo E2 `app/chamados/services/tarefa_do_dia.py:324`.
- **Passivo:** E1 fornece linhas por correspondência exata `(pergunta,ato,ato_em)`, lendo a projeção em memória. A E2 corta pelo `ato_em` que essa autoridade devolve, preservando essa unidade. `app/chamados/models.py:3818`, `:3824`, `:3831`, `:3835`; anexo E2 `app/chamados/services/tarefa_do_dia.py:349`.

## 08/11 16:30 · sessao b791b0234cbc · - · - · origem=Ronald (tmux)

Comparação integral por `git hash-object` sem `-w`: os nove arquivos abaixo coincidem com o blob OLD de `predesenho_e2_proprio.diff`; portanto, **E1 não os modificou desde a base do diff**. A referência de linha local aponta o contexto conferido; o índice OLD do anexo prova a igualdade integral.

| Arquivo | E1 mudou? | Referências e significado no rebase |
|---|---|---|
| `app/chamados/tests/test_porta_veredito_pergunta.py` | Não; OLD/atual `ef664599` | E1:270,275 ainda chama `esta_stale(p)`; E2:270,275 passa `(p, None, timezone.now())`. Adapta o teste à assinatura obrigatória de `agora`; sem disputa textual. |
| `app/chamados/tests/test_selo_concordancia_varredura_stale.py` | Não; `acbe9f88` | E1:58-59 usa marcos sem horizonte e `esta_stale` sem agora; E2:58-60 usa a mesma borda nos dois leitores, acrescenta caso de lavra futura em E2:93-101. |
| `app/chamados/tests/test_selo_lavrado_orfaos.py` | Não; `ace36828` | E1:60-61,85-86,97,103,115-130 conserva assinaturas antigas; E2:60-61,85-86,97,103,115-130 injeta hoje/agora. |
| `app/core/juizes.py` | Não; `02dc1c1f` | E1:149 termina `_C18`; E1:278 termina o registro da família chamado. E2:156-167 e E2:291-292 acrescenta `_C19` e `_C20`, sem autoridade concorrente já adicionada pela E1. |
| `app/docs/ARQUITETURA.mmd` | Não; `0fd088d4` | E1:338,343 / E2:338,343: 18→20 perguntas com juiz; +7→+8 arquivos. Metadados da documentação. |
| `app/docs/CORTES.md` | Não; `e5ed6080` | E1 termina em :423; E2:424-425 acrescenta os cortes de nascimento de `tarefa_do_dia` e declaração do juiz da tranca. |
| `app/docs/JUIZES.md` | Não; `8734ebb4` | E1:70-73 / E2:73-85: as duas entradas são novas, antes de FERIADO/PRAZO. O registro explica função pura, último ato por `ato_em`, lavra vigente e tranca por autoridade. |
| `app/docs/MAPA-JUIZES.md` | Não; `3fd429ab` | E1:103-110 / E2:105-112 recebe as duas perguntas; E1:143 / E2:145 altera 13→12 pendentes de feriado; E1:184-189 / E2:186-191 ajusta contagens. A alteração 13→12 pertence ao diff E2 apresentado; não veio desta E1. |
| `app/docs/MAPA.md` | Não; `38385d46` | E1:88 / E2:88 altera chamado 18/0→20/0; E1:176 / E2:177-178 acrescenta os dois registros. |
| `app/escala/tests/test_signal_ancora.py` | **Sim**; OLD `3c51c993`, atual `fd7bcbc8` | E1:50-55 e :67-69 **já substituiu** a expectativa de apagar por uma pergunta conservada com `VIA_MOTIVO_RETRATADO`. E2:50-62 extrai `_ancora_retratada`, verifica zero vivas, uma retratada e uma `DecisaoPergunta.RETRATACAO`; E2:74 reutiliza o helper. Mesma regra de conservação, diferença de organização e prova adicional da sequência. Não restaurar expectativa de `count()==0`. |
| `app/chamados/tests/test_e2_tarefa_do_dia.py` | Arquivo novo no diff (`/dev/null`, E2:1-1402); não achei na árvore | A inexistência foi conferida por `ls` e `rg --files`; ausência não tem linha local [nao verificado]. |
| `app/escala/tests/test_grade_alimentada.py` | Arquivo novo no diff (`/dev/null`, E2:1-61); não achei na árvore | Mesma limitação de citação para inexistência [nao verificado]. O anexo E2:44-61 prova o contrato alimentada=consulta e triestado. |

Impactos indiretos dos testes: [nao verificado]

- A E1 escreve o dia da pergunta pelo ponto único `app/chamados/services/fio.py:790-801`, recebe o resultado do juiz e não calcula nem salva ali; o nascimento chama esse ponto e depois salva em `app/chamados/models.py:2919-2923`. A rechave chama o mesmo ponto, troca célula, lavra e salva em `app/chamados/services/fio.py:989-995`.
- A conversa canônica **tem data civil, nunca NULL**, conforme `app/chamados/models.py:2750-2758` e `app/chamados/services/fio.py:1421`. Os testes imutados ainda dizem “conversa sem dia” em `app/chamados/tests/test_porta_veredito_pergunta.py:277-281` e `app/chamados/tests/test_selo_lavrado_orfaos.py:92-97`; o primeiro fabrica a linha nula diretamente em :291, portanto não prova a porta `abrir_conversa`. O diff E2 adapta as assinaturas, mas conserva essa prosa antiga nos hunks desses testes.
- A E1 atual **ainda permite o reuso nulo pular a rechave**, pela guarda `ex.data_evento is not None` de `app/chamados/models.py:2862`; portanto o contrato 15e anexado não pode ser considerado implementado nesta árvore. O rascunho 15e reconhece isso na linha A10 da própria matriz anexada; sem numeração de linha fornecida para esse Markdown [nao verificado].
- A identidade em E1 inclui dia do turno, motivo e instância em `app/chamados/models.py:2387-2395`, mas `instancia_marco` ainda vem de `instancia_do_motivo` (:2391-2392). Isso não acrescenta por si um índice da segunda pausa; a E2 declara o limite em `predesenho_e2_tarefa_do_dia_e93205c6.py:273-281`.
- Não executei testes nem acessei banco; resultados de execução da composição E1+E2 não verificados [nao verificado]. [nao verificado]

## 08/11 16:30 · sessao b7b0fe58cca6 · - · - · origem=Ronald (tmux)

Recomendo **“perguntas de lei: 0 — não achei”**, sem afirmar que a implementação está correta. A lei exige procurar resposta existente antes de pedir corte e distingue decisão técnica de pergunta de lei. (`CLAUDE.md:25`, `CLAUDE.md:266`, `CLAUDE.md:273`)

Pontos que não justificam, por si, pergunta de lei:

- **Duas pausas:** a própria E1 declara que a identidade capaz de separá-las pertence à E2; atualmente recusa `instancia_marco` diferente da derivada do motivo. É pendência de desenho/contrato diante da missão por instância, não ausência demonstrada de lei. (`app/chamados/models.py:3341`, `app/chamados/models.py:3349`; corte FIO do anexo `predesenho_e2_proprio.diff`, hunk CORTES `+424`)
- **Juiz ausente/None:** mostrar na matriz o comportamento efetivo, a propagação e os caminhos sem teste. Não há autorização para recalcular a autoridade em outro leitor. (`CLAUDE.md:20`, `CLAUDE.md:166`) [nao verificado]
- **Antes/depois do turno:** a comparação precisa ser por instante ou fato encerrado; não falta lei sobre o teto temporal. (`CLAUDE.md:185`)
- **Tranca:** o aval fornecido manda consultar o juiz e determina dia trancado sem tarefa, com toque humano devolvendo julgamento normal. (`aval_lei_tranca_e2_juiz.txt:1`)
- **Devido pela grade:** o aval fornecido escolhe expressamente `grade_da_celula` e proíbe leitura da ata crua pela tarefa. (`aval_e2_devido_pela_grade.txt:1`)
- **Horizonte da obra:** há regra escrita para obra nova do horizonte para frente, passado como foto até fato novo. (`app/docs/LEIS.md:145`, `app/docs/CORTES.md:419`)

Atenção à premissa **Chamado/PerguntaDisputa.data_evento**: o ponto único declara e atribui **PerguntaDisputa.data_evento**; não achei declaração de `ChamadoColaborador.data_evento`. Para o chamado, o código mostrado consulta `dia_do_fato(ch, fortes=True)`. (`app/chamados/services/fio.py:790`, `app/chamados/services/fio.py:800`, `app/chamados/models.py:2383`, `app/chamados/services/fio.py:84`)

Referências úteis adicionais:

- Lâmpada: `CLAUDE.md:166`.
- L-051, arquivo reversível por toque: `app/docs/LEIS.md:92`.
- Relógio recebido do chamador: `app/docs/CORTES.md:370`.
- Autoridade de dia sem trabalho e tratamento de `trabalha=None`: `app/docs/CORTES.md:416`.
- E1: juiz sem resposta não cai na data gravada; episódio sem dia tem recusas nomeadas: `app/chamados/services/fio.py:1318`, `app/chamados/services/fio.py:1340`.
- Conversa guarda dia civil, por porta própria: `app/chamados/models.py:2750`.

## 08/11 15:16 · sessao 156252195190 · FIO-E0-V4-E1 · leitura · origem=Code

**1) `ReusoSemRechave`: origem, propagação e limites dos lotes**

A exceção nasce quando a canônica tem `data_evento != dia_do_fato` e `rechavear_no_reuso` devolve recusa; o lançamento acontece antes da atualização de célula/destino, dentro do `atomic` de `_nascer`. A hierarquia é `ReusoSemRechave → FatoEmOutroDia → ResolvedoraCruzada → ValueError`. (`app/chamados/models.py:2862`, `app/chamados/models.py:2867`, `app/chamados/models.py:2870`, `app/chamados/models.py:2770`, `app/chamados/models.py:2335`, `app/chamados/models.py:2291`, `app/chamados/models.py:2284`)

Há quatro sítios de chamada direta a `PerguntaDisputa.abrir`: dois na emissão e dois na declaração humana. **Chamar `abrir` não prova que o caminho alcance esta exceção:** os chamadores de `:834` e das declarações humanas produzem motivos MARCO; o cenário D0/DIA de episódio BATIDA exige distinguir essa família. (`app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/declaracao_texto.py:348`, `app/chamados/services/declaracao_texto.py:636`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/catalogo/motivos.py:42`, `app/chamados/catalogo/motivos.py:48`)

**Caminho datado que aceita motivos BATIDA — `disputa_emissao.py:444`.**

A cadeia comum é:

`criar_ou_atualizar_disputa` → `_abrir_ou_agregar_disputa` → `gerar_perguntas_disputa` → laço dos motivos → `PerguntaDisputa.abrir` → `_nascer` → `raise ReusoSemRechave`. (`app/chamados/services/disputa_emissao.py:662`, `app/chamados/services/disputa_emissao.py:670`, `app/chamados/services/disputa_emissao.py:682`, `app/chamados/services/disputa_emissao.py:312`, `app/chamados/services/disputa_emissao.py:444`, `app/chamados/models.py:2746`, `app/chamados/models.py:2867`)

| Borda até a cadeia comum | Quem captura | Resto do lote e transação |
|---|---|---|
| API: rota → `api_bater_ponto` → emissão. (`app/api/urls.py:67`, `app/api/views.py:400`, `app/api/views.py:794`) | Nenhum `except` envolve a chamada da emissão. Os `except Exception` posteriores protegem foto e geofence, não essa chamada. (`app/api/views.py:794`, `app/api/views.py:803`, `app/api/views.py:812`) | Abandona os motivos restantes e o restante da view. O `atomic` de `_nascer` é desfeito; a abertura/agregação anterior e perguntas anteriores não estão num único `atomic` dessa função. Sem transação externa, permanecem gravadas. Transação acrescentada pelo ambiente da request: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:312`, `app/chamados/models.py:2770`, `app/chamados/services/disputa_emissao.py:521`, `app/chamados/services/disputa_emissao.py:669`) |
| API core: rota → `api_bater_ponto` → emissão. (`app/api/urls_core.py:14`, `app/api/views_core.py:387`, `app/api/views_core.py:794`) | Mesmo limite: nenhuma captura envolvendo a emissão; foto/geofence têm capturas posteriores. (`app/api/views_core.py:794`, `app/api/views_core.py:803`, `app/api/views_core.py:812`) | Mesmo abandono dos motivos e da view, com rollback somente dos blocos atravessados pela exceção. Transação externa da request: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:312`, `app/chamados/services/disputa_emissao.py:521`, `app/chamados/models.py:2770`) |
| Cron da selfie → command → laço de batidas → emissão. (`app/config/crons.py:686`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:58`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`) | Sem captura e sem `atomic` envolvendo o lote no command. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:94`) | Uma exceção propagada abandona as batidas/colaboradores seguintes; não incrementa `n` naquela iteração nem imprime o resumo final. Iterações anteriores não são revertidas por um bloco global do command. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:106`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:107`) |

**Precisão sobre a selfie:** o command passa `foto_ausente`, mas não passa `data` nem `ultima_batida`; numa disputa recém-nascida, isso conduz ao ramo `abrir_conversa`, não automaticamente a `:444`. Na disputa existente, o merge preserva outras chaves do contexto; a passagem efetiva pelo ramo datado depende desse contexto. Sua ocorrência no dado fornecido é **[não verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/services/disputa_emissao.py:371`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:439`, `app/chamados/services/disputa_emissao.py:548`, `app/chamados/services/disputa_emissao.py:555`)

O `except Exception` em `disputa_emissao.py:466` **não captura a exceção de `:444`**: seu `try` começa depois da abertura e protege somente o reatamento da pergunta já criada, com savepoint próprio. (`app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:462`, `app/chamados/services/disputa_emissao.py:463`, `app/chamados/services/disputa_emissao.py:466`)

**Agrupador e chamada `disputa_emissao.py:834`.**

As entradas do helper são os intervalos, os complementos de marcos e os resultados de `classificar_falta`; todos correspondem aos quatro motivos MARCO. Para MARCO, `do_fato` consulta o dia exato; a busca por episódio que atravessa datas gravadas é restrita à família BATIDA. Portanto, **esses caminhos não demonstram o alcance do cenário BATIDA D0/DIA**. (`app/chamados/services/disputa_emissao.py:1055`, `app/chamados/services/disputa_emissao.py:873`, `app/chamados/services/disputa_emissao.py:802`, `app/escala/servico_jornada.py:267`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2677`, `app/chamados/models.py:2686`, `app/chamados/models.py:2713`)

O limite transacional existente, para uma exceção propagada de `abrir`, é um único `atomic` envolvendo **todo o agrupamento daquele colaborador**: complemento inicial, intervalos, ata, grupos com origem, chamados sem origem e entradas pendentes. Não há captura em torno de `:834`; o `except Exception` de `:865` termina antes do laço que chama o helper em `:878`. A propagação abandona os demais laços e desfaz todo esse agrupamento. (`app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:990`, `app/chamados/services/disputa_emissao.py:1062`, `app/chamados/services/disputa_emissao.py:1084`, `app/chamados/services/disputa_emissao.py:1122`, `app/chamados/services/disputa_emissao.py:1153`, `app/chamados/services/disputa_emissao.py:1179`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/disputa_emissao.py:865`, `app/chamados/services/disputa_emissao.py:878`)

As bordas desse agrupador e seus tratamentos são:

| Borda → agrupador | Captura, continuação e trilha |
|---|---|
| Cron de alertas avançados → laço de colaboradores → agrupador. (`app/config/crons.py:363`, `app/ponto/management/commands/processar_alertas_avancados.py:29`, `app/ponto/management/commands/processar_alertas_avancados.py:158`) | `except Exception` em `:159`; imprime a falha em stdout e segue outros colaboradores. O chamado aberto antes fica fora do rollback do agrupador; `processados` ainda aumenta, sem ser contador da recusa. (`app/ponto/management/commands/processar_alertas_avancados.py:159`, `app/ponto/management/commands/processar_alertas_avancados.py:160`, `app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/ponto/management/commands/processar_alertas_avancados.py:164`) |
| Cron de alertas de turno → laço → `_marco_critico` → `_abrir_chamado_critico` → agrupador. (`app/config/crons.py:362`, `app/ponto/management/commands/processar_alertas_turno.py:81`, `app/ponto/management/commands/processar_alertas_turno.py:141`, `app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:384`) | `except Exception` em `:385`; imprime erro, retorna o chamado e permite continuar o processamento. O chamado nasceu antes do agrupador; não há contador específico da recusa nesse tratamento. (`app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_turno.py:386`, `app/ponto/management/commands/processar_alertas_turno.py:387`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Cron de ausências → laços → `_abrir_chamado` → agrupador. (`app/config/crons.py:364`, `app/ponto/management/commands/detectar_ausencias.py:101`, `app/ponto/management/commands/detectar_ausencias.py:138`, `app/ponto/management/commands/detectar_ausencias.py:439`, `app/ponto/management/commands/detectar_ausencias.py:801`) | `except Exception` em `:802`; imprime em stderr e continua. O chamado anterior permanece; o contador `abertos` ainda aumenta e não conta recusas de pergunta. (`app/ponto/management/commands/detectar_ausencias.py:802`, `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:448`) |
| Fio/modal → `processar_post_fio` → `acao_iniciar_disputa_orfaos` → agrupador. (`app/chamados/urls.py:21`, `app/chamados/urls.py:39`, `app/chamados/services/fio.py:551`, `app/chamados/services/acoes_disputa.py:221`) | Sem captura aplicável nessa cadeia; o agrupamento é revertido e o log de sucesso posterior não é alcançado. Tratamento externo à aplicação: **[não verificado]**. (`app/chamados/services/acoes_disputa.py:210`, `app/chamados/services/acoes_disputa.py:226`, `app/chamados/services/disputa_emissao.py:964`) |
| Command de passivo → `pergunta_no_app.perguntar` → laço de colaboradores → agrupador. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`, `app/chamados/services/pergunta_no_app.py:175`, `app/chamados/services/pergunta_no_app.py:184`, `app/chamados/services/pergunta_no_app.py:186`) | Sem captura/`atomic` global do lote: abandona os colaboradores seguintes; os agrupamentos anteriores já terminaram. O contador posterior daquela iteração e o resumo não são alcançados. (`app/chamados/services/pergunta_no_app.py:184`, `app/chamados/services/pergunta_no_app.py:187`, `app/chamados/services/pergunta_no_app.py:188`) |
| “Cobrar dia” → cartório e, depois, agrupador. (`app/chamados/urls.py:32`, `app/chamados/views_cobrar.py:325`, `app/chamados/services/cobrar_dia.py:39`, `app/chamados/services/cobrar_dia.py:47`) | Sem captura envolvendo essas chamadas. O `except ValueError` da view só cobre o parse anterior da data. A trilha `cobrar_dia_manual` é anterior e registra o toque, não a recusa. (`app/chamados/views_cobrar.py:320`, `app/chamados/views_cobrar.py:322`, `app/chamados/services/cobrar_dia.py:37`) |

**Sinais e cartório.**

A cadeia de emissão é `julgar_celula → julgar_colab → perguntar_pela_celula → perguntar → criar_ou_atualizar_disputa/gerar_perguntas_disputa → abrir(:444)`. Ela gera os motivos de marcos da ata/lei; não constitui demonstração do cenário de episódio BATIDA D0/DIA. (`app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:186`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/fabrica_da_celula.py:151`, `app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/fabrica_da_celula.py:47`, `app/chamados/services/fabrica_da_celula.py:55`)

Os limites de captura dessa cadeia são:

| Entrada no julgamento | Captura e efeito sobre o lote |
|---|---|
| `post_save` de Chamado → cartório. (`app/chamados/signals.py:188`, `app/chamados/signals.py:203`) | `engolir` envolve o julgamento inteiro; captura `Exception` em `core/observ.py:90`. Uma exceção propagada encerra os motivos/células restantes desse julgamento e volta ao escritor original. Não atravessa o `atomic` externo desse escritor; o receiver não tem um `atomic` próprio que reverta todo o julgamento anterior. Registra ERROR e incrementa contador em memória por operação. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/core/observ.py:75`, `app/core/observ.py:77`, `app/core/observ.py:79`, `app/ponto/services/cartorio.py:995`) |
| Save/delete de Batida, save/delete de Ausencia e save de Justificativa → `_julgar_dias` → cartório. (`app/ponto/signals.py:135`, `app/ponto/signals.py:140`, `app/ponto/signals.py:147`, `app/ponto/signals.py:152`, `app/ponto/signals.py:165`, `app/ponto/signals.py:93`) | O mesmo `except Exception` de `engolir`, mas envolvendo **todo o laço de dias**: a primeira exceção abandona os dias seguintes desse receiver. Não propaga para reverter a escrita original; deixa ERROR e contador em memória. (`app/ponto/signals.py:88`, `app/ponto/signals.py:92`, `app/core/observ.py:90`, `app/core/observ.py:75`, `app/core/observ.py:79`) |
| Reescrita de célula → `_o_chao_do_dia_mudou` → cartório. (`app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:434`) | `except Exception` **por dia**, em `:437`; registra `logger.exception` e segue os demais dias e o restante da operação. Não há rollback global por propagação dessa exceção. (`app/ponto/portas/celula.py:430`, `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:439`, `app/ponto/portas/celula.py:445`) |
| Pós-vínculo → laço de dias → cartório. (`app/colaboradores/services/vinculo.py:330`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:237`, `app/colaboradores/services/pos_vinculo.py:63`, `app/colaboradores/services/pos_vinculo.py:72`) | `except Exception` **por dia**, em `:75`; registra a falha e continua os dias e as etapas posteriores. O placar de julgadas só aumenta depois do retorno; não existe contador específico da recusa nesse tratamento. (`app/colaboradores/services/pos_vinculo.py:73`, `app/colaboradores/services/pos_vinculo.py:75`, `app/colaboradores/services/pos_vinculo.py:76`, `app/colaboradores/services/pos_vinculo.py:77`, `app/colaboradores/services/pos_vinculo.py:80`) |

**Reconciliador.**

O reconciliador entra por batida ou pelo callback do filho que saiu de vivo. Ele não chama `PerguntaDisputa.abrir` diretamente: a reemissão/alteração de Chamado pode acionar o sinal do cartório acima. (`app/ponto/signals.py:215`, `app/chamados/signals.py:355`, `app/chamados/reconciliador.py:910`, `app/chamados/reconciliador.py:523`, `app/chamados/signals.py:188`)

| Cadeia | Captura e continuação |
|---|---|
| Laço de órfãs → `reatar_pergunta_orfa` → reemissão → `ChamadoColaborador.abrir` → sinal → cartório → fábrica → `abrir(:444)`. (`app/chamados/reconciliador.py:1520`, `app/chamados/reconciliador.py:1522`, `app/chamados/reconciliador.py:1617`, `app/chamados/reconciliador.py:910`, `app/chamados/signals.py:203`) | A captura mais próxima de uma exceção da emissão é o `engolir` do sinal. Por essa cadeia, o `except Exception` do laço de órfãs não a recebe: o sinal já a consumiu. O laço pode continuar e a transação externa não é revertida pela propagação dessa exceção. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/chamados/reconciliador.py:1524`) |
| Laço de premissas → `_retratar` → `atomic` → `ch.reconciliar` → save de estado → mesmo sinal. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1460`, `app/chamados/reconciliador.py:522`, `app/chamados/reconciliador.py:523`, `app/chamados/models.py:296`) | Mesmo consumo no sinal, antes do `except Exception` externo de `:1462`; os outros chamados continuam e os blocos externos não são desfeitos por essa propagação. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/chamados/reconciliador.py:1462`) |
| Construção do contexto C2 → `julgar_celula` → fábrica → `abrir(:444)`. (`app/chamados/reconciliador.py:691`, `app/chamados/reconciliador.py:702`) | Aqui há captura própria: `except Exception` em `:709`; registra a falha e continua devolvendo contexto, sem o enriquecimento de marcos que não terminou. Não há savepoint próprio envolvendo todo esse julgamento. (`app/chamados/reconciliador.py:709`, `app/chamados/reconciliador.py:710`, `app/chamados/reconciliador.py:690`) |

O cron atual `reconciliar_chamados --alarme` é vigia: consulta e conta; não executa essas cadeias escritoras. (`app/config/crons.py:395`, `app/chamados/management/commands/reconciliar_chamados.py:49`, `app/chamados/management/commands/reconciliar_chamados.py:71`, `app/chamados/management/commands/reconciliar_chamados.py:102`)

**Demais crons/commands que chegam à fábrica pelo cartório.** São caminhos de emissão MARCO, com estes limites para exceções propagadas do julgamento; não provam alcance do cenário BATIDA. (`app/chamados/services/fabrica_da_celula.py:47`, `app/chamados/services/fabrica_da_celula.py:55`)

| Borda → cartório | Captura / lote / rollback |
|---|---|
| Cron `processar_cartorio --apply` → laço de colaboradores → `julgar_colab`. (`app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`) | Sem captura ou `atomic` global do command: abandona colaboradores seguintes; gravações anteriores não são revertidas por um bloco global. O resumo final não é alcançado. (`app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:66`) |
| Cron `vigia_de_hora` → command → `rodar` → laço → `julgar_celula`. (`app/config/crons.py:367`, `app/ponto/management/commands/vigia_de_hora.py:28`, `app/ponto/services/vigia_de_hora.py:115`, `app/ponto/services/vigia_de_hora.py:131`) | Sem captura; abandona o restante do lote. O `finally` libera o advisory lock; não reverte os itens anteriores. Os contadores posteriores ao retorno não aumentam para o item interrompido. (`app/ponto/services/vigia_de_hora.py:132`, `app/ponto/services/vigia_de_hora.py:134`) |
| Cron encadeado `disparar_perguntas_competencia` → `_rodar` → laço → `perguntar` → geração. (`app/config/crons.py:1516`, `app/chamados/management/commands/disparar_perguntas_competencia.py:273`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`) | Em apply, um `atomic` engloba todos os itens: uma exceção propagada aborta e reverte **todo o lote desse bloco**. No dry, `except RuntimeError` não captura `ReusoSemRechave`, que é `ValueError`. O caminho da célula é apenas vigia; quem escreve aqui é o fio mudo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:290`, `app/chamados/management/commands/disparar_perguntas_competencia.py:307`, `app/chamados/management/commands/disparar_perguntas_competencia.py:316`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240`, `app/chamados/models.py:2284`) |
| `cobrar_furos_sem_cobranca --apply` → `cobrar` → laço → `julgar_celula`. (`app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`, `app/ponto/services/furos_sem_cobranca.py:124`, `app/ponto/services/furos_sem_cobranca.py:128`) | Sem captura/`atomic` global: abandona os itens seguintes; os anteriores permanecem. O contador daquela iteração fica depois da chamada interrompida. (`app/ponto/services/furos_sem_cobranca.py:124`, `app/ponto/services/furos_sem_cobranca.py:131`) |
| `rejulgar_ausencia_em_folga --apply` → laço → `julgar_celula`. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:77`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`) | Sem captura/`atomic` global; abandona os dias seguintes e o resumo. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:77`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:80`) |
| `curar_residuo_bug115` → laços → `_lavrar` → `julgar_celula`. (`app/ponto/management/commands/curar_residuo_bug115.py:97`, `app/ponto/management/commands/curar_residuo_bug115.py:99`, `app/ponto/management/commands/curar_residuo_bug115.py:143`) | Sem captura envolvendo essa chamada; abandona o restante dos laços, sem rollback global deles. (`app/ponto/management/c

[truncado em 20000 de 33247 caracteres]

## 08/11 15:11 · sessao 4cbd0bcd30a1 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são relativas à raiz da cópia.

**Núcleo e propagação**

- `ReusoSemRechave` herda `FatoEmOutroDia` → `ResolvedoraCruzada` → `ValueError`; o lançamento está em `app/chamados/models.py:2867`, dentro do `atomic` de `_nascer` em `app/chamados/models.py:2770`. A recusa ocorre antes da costura de célula/destino em `app/chamados/models.py:2870`; não há captura nesse núcleo. (`app/chamados/models.py:2335`, `app/chamados/models.py:2291`, `app/chamados/models.py:2284`)
- `gerar_perguntas_disputa` percorre motivos em `app/chamados/services/disputa_emissao.py:312` e chama `abrir` em `app/chamados/services/disputa_emissao.py:444`, sem captura. O `except Exception` de `app/chamados/services/disputa_emissao.py:466` envolve apenas o reatamento posterior (`:463–465`): **não captura a exceção de `:444`**. A exceção interrompe os motivos restantes e não alcança `criadas.append` daquela pergunta em `app/chamados/services/disputa_emissao.py:470`.
- `criar_ou_atualizar_disputa` abre/agrega antes, por `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:662`), cujo `atomic` está em `app/chamados/services/disputa_emissao.py:521`; depois chama o gerador em `:670` ou `:682` e pendentes em `:685`. Sem uma transação externa, a abertura/agregação anterior já terminou seu bloco; não há um `atomic` único envolvendo o gerador nessa função. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:669`)
- `perguntar_entradas_pendentes` percorre dias em `app/chamados/services/disputa_emissao.py:287`, chama o gerador em `:294` sem captura e só salva contexto depois em `:296`. Uma exceção abandonaria os dias restantes e esse `save`. Contudo seus motivos vêm de `classificar_falta` (`:288`), isto é, MARCO; veja restrição de alcance abaixo.
- A fábrica de célula percorre motivos em `app/chamados/services/fabrica_da_celula.py:145`, chama `criar_ou_atualizar_disputa` em `:149` e o gerador em `:151`, sem captura. `perguntar_pela_celula` a chama em `:186`; `cartorio.julgar_colab` chama essa porta no laço de células em `app/ponto/services/cartorio.py:995` e `:997`. O contador de perguntas só aumenta depois do retorno em `app/ponto/services/cartorio.py:999`; o `finally` começa em `:1051`, sem capturar a exceção.

**Bordas diretas de `criar_ou_atualizar_disputa` → gerador `:444`**

| Borda/cadeia | Captura e efeito observável |
|---|---|
| `app/api/urls.py:67` → `app/api/views.py:400` → `:794` → emissão `:662` → `:670/:682` → gerador `:444` | Não há `except` envolvendo `:794`; os `except Exception` de `app/api/views.py:803` e `:812` são de operações posteriores. A view não declara `atomic` em sua entrada (`app/api/views.py:398`). Tratamento final pelo framework/deployment: **[não verificado]**. |
| `app/api/urls_core.py:14` → `app/api/views_core.py:387` → `:794` → mesma cadeia | Mesma ausência de captura no ponto de chamada; capturas posteriores em `app/api/views_core.py:803` e `:812`; entrada em `app/api/views_core.py:385`. Tratamento externo: **[não verificado]**. |
| `app/ponto/urls.py:11` ou `:12` → `app/ponto/views.py:242` → `:350` → `ramo_registrar_es`, `app/ponto/services/triagem_batida.py:616` → `:734` → mesma cadeia | Não há captura em torno de `:734` nem da delegação em `app/ponto/views.py:350`. Esse ramo emite apenas `saida_sem_entrada` (`app/ponto/services/triagem_batida.py:736`), portanto **não alcança o cenário BATIDA D0→DIA** pelas identidades normais da árvore. |
| Cron `app/config/crons.py:686` → `marcar_foto_ausente_retro.handle`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:58` → laço `:95` → `:96` → mesma cadeia | Sem captura/`atomic` de lote no comando. Exceção abandona batidas restantes e não imprime o resumo final em `:107`; o contador `n` só aumenta em `:106` e não conta recusas. **Atenção:** o contexto passado em `:98` não tem `data`/`ultima_batida`; com disputa recém-nascida o gerador acaba em `abrir_conversa` (`emissão :439`), não em `:444`. Disputa existente conserva outras chaves no merge (`emissão :548/:555/:559`), portanto o ramo datado depende do contexto existente; sua ocorrência em produção é **[não verificado]**. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:273` → `:282` → fábrica `:149/:151` → gerador `:444` | `--apply`: um único `atomic` para todos os itens em `:290`, sem captura aplicável; exceção abandonaria o lote e reverteria esse bloco todo; resumo de sucesso `:296` não é alcançado. Dry: `atomic :307`; `except RuntimeError :316` não captura `ReusoSemRechave`. É caminho MARCO, não cenário BATIDA. Encadeamento de cron em `app/config/crons.py:1516`. |
| `smoke_chamados.handle`, `app/chamados/management/commands/smoke_chamados.py:39` → `_bloco_a_motivos :41` → laço `:91` → `:102` → gerador `:444` | `except Exception` em `:114`, incrementa `resumo["erro"]` em `:115` e guarda texto em `problemas :116`; segue próximo motivo. A transação externa permanece até o rollback proposital `_Rollback` em `:43`, capturado em `:44`. Command de smoke manual, registrado como tal em `app/config/crons.py:1428`; não confundir com emissor operacional. |

**Agrupador de órfãos: bloco e bordas**

O agrupador tem um `atomic` único em `app/chamados/services/disputa_emissao.py:964`, envolvendo complemento inicial (`:974/:990`), intervalos (`:1050/:1052/:1062`), ata (`:1077/:1084`), grupos (`:1103/:1122/:1127`), sem origem (`:1140/:1153/:1158`) e pendentes (`:1179`). `_criar_pergunta_se_couber` chama `abrir` em `:834`, sem captura; complemento percorre marcos em `:870` e chama o helper em `:878`, fora do `try` que termina no `except Exception :865`. Uma exceção propagada de `abrir` desfaz todo o agrupamento daquele colaborador e abandona seus demais laços. (`app/chamados/services/disputa_emissao.py:834`, `:865`, `:878`, `:964`)

| Borda → agrupador | Captura / resto do lote / trilha |
|---|---|
| Cron `app/config/crons.py:363` → `processar_alertas_avancados.handle`, `app/ponto/management/commands/processar_alertas_avancados.py:19` → laço colabs `:29` → agrupador `:158` | `except Exception :159`; imprime `fio disputa falhou` em stdout `:160`; segue o laço. A criação do chamado ocorreu antes (`:121`) e permanece fora do bloco interno do agrupador. O contador `processados :164` ainda aumenta; não é contador da recusa. |
| Cron `app/config/crons.py:362` → `processar_alertas_turno.handle`, `app/ponto/management/commands/processar_alertas_turno.py:32` → laço turnos `:81` → `_marco_critico :141` → `_abrir_chamado_critico :316` → agrupador `:384` | `except Exception :385`; imprime erro stdout `:386`; retorna chamado em `:387`; `_marco_critico` ainda pode enviar alerta e abrir justificativa (`:319/:321`), e o laço continua. Chamado foi aberto antes em `:335`, fora do agrupador. Não há contador específico de recusa. |
| Cron `app/config/crons.py:364` → `detectar_ausencias.handle`, `app/ponto/management/commands/detectar_ausencias.py:61` → laços `:101/:138` → `_abrir_chamado :439` → agrupador `:801` | `except Exception :802`; imprime erro stderr `:803`; volta ao chamador, que ainda aumenta `abertos :448` e segue. Chamado aberto antes em `:693`, fora do agrupador. Contadores em `:455` não são contador dessa recusa. |
| `app/chamados/urls.py:21` ou `:39` → `fio_colaborador :674` ou `modal_fio_colaborador :954`, em `app/chamados/views.py` → `processar_post_fio`, `app/chamados/services/fio.py:551` → `acao_iniciar_disputa_orfaos`, `app/chamados/services/acoes_disputa.py:221` → agrupador | Sem captura aplicável na cadeia. O log específico de sucesso em `app/chamados/services/acoes_disputa.py:226` não é alcançado em caso de exceção. Não há transação de lote declarada nessas cascas (`app/chamados/views.py:658`, `:868`; `app/chamados/services/acoes_disputa.py:210`). Tratamento externo: **[não verificado]**. |
| `perguntar_chamados_sem_pergunta.handle`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19` → `:29` → `pergunta_no_app.perguntar`, `app/chamados/services/pergunta_no_app.py:175` → laço colabs `:184` → agrupador `:186` | Sem captura/`atomic` externo de lote. Exceção abandonaria colaboradores seguintes; blocos anteriores do agrupador já terminaram. `n += len(chs)` em `:187` não ocorre para o colaborador recusado; o retorno/resumo `:188` e a impressão de `APLICADO` não são alcançados. |
| `app/chamados/urls.py:32` → `app/chamados/views_cobrar.py:325` → `app/chamados/services/cobrar_dia.py:39` (cartório) e depois agrupador `:47` | Sem captura aplicável; `except ValueError` em `app/chamados/views_cobrar.py:322` só envolve parse anterior da data. A trilha de toque `cobrar_dia_manual` é gravada antes, em `app/chamados/services/cobrar_dia.py:37`; não é trilha da recusa. |

**Restrição decisiva: chamada `:834` não recebe o cenário BATIDA descrito pelos chamadores atuais**

Os motivos enviados ao helper `:834` são:

- Intervalos, definidos em `app/chamados/services/disputa_emissao.py:1055`.
- Complementos derivados de `_MOTIVO_POR_MARCO` em `app/chamados/services/disputa_emissao.py:873`, cuja tabela só contém `saida_sem_entrada`, `orfao_14h`, `intervalo_saida`, `intervalo_volta` em `app/chamados/services/declaracao_texto.py:42`.
- Grupos/sem origem derivados de `_classificar_falta`, que delega em `app/chamados/services/disputa_emissao.py:802`; os retornos cobradores são `saida_sem_entrada`/`orfao_14h` em `app/escala/servico_jornada.py:445`, `:447`, `:458`, `:463`, `:465`, `:470`, `:471`.

Esses quatro motivos são MARCO (`app/chamados/catalogo/motivos.py:34`, `:35`, `:48`, `:49`); `batida_origem_do_nascimento` retorna `None` fora da família BATIDA (`app/chamados/models.py:3365`). Para MARCO, a consulta usa a data exata (`app/chamados/models.py:2686`, `:2713`), e o dia do fato sem episódio vem da própria data do marco (`app/chamados/services/fio.py:1334`). Portanto **não se deve apresentar `:834`, fábrica de célula, declarações de MARCO ou agrupador como execução comprovada de `ReusoSemRechave` do episódio BATIDA D0→DIA**. A tabela de agrupador acima documenta limites de propagação do código, não comprova essa alcançabilidade.

**Contador não é trilha de tentativas**

A recusa de reuso é `raise`, em `app/chamados/models.py:2867`; o contador `fio.reuso_sem_rechave` varre estado persistido (`app/chamados/services/fio.py:1046`), confirma candidatas chamando a porta real em dry (`:1059`) e retorna `n/pks/universo` (`:1061`). Não incrementa um contador persistido a cada exceção nem cria registro de recusa nesse ponto. Seu universo exclui linhas sem episódio e conversas (`app/chamados/services/fio.py:1040`, `:1046`).

## 08/11 15:11 · sessao 90837d85b73f · - · - · origem=Ronald (tmux)

Caminhos conferidos, todos relativos a `app/`.

1. **Sinal do chamado que pode iniciar emissão**

`ChamadoColaborador.save()` → receiver `chamados/signals.py:188` → `cartorio.julgar_celula` em `chamados/signals.py:203` → `julgar_colab` em `ponto/services/cartorio.py:1989` → laço das células em `ponto/services/cartorio.py:995` → `perguntar_pela_celula` em `ponto/services/cartorio.py:997` → `perguntar` em `chamados/services/fabrica_da_celula.py:186` → `criar_ou_atualizar_disputa` em `chamados/services/fabrica_da_celula.py:149` → geração em `chamados/services/disputa_emissao.py:670` ou `:682` → `PerguntaDisputa.abrir` em `chamados/services/disputa_emissao.py:444`.

A captura é o `engolir` que envolve **todo julgamento** em `chamados/signals.py:195`: `except Exception as exc` em `core/observ.py:90`. Esse contexto não abre `atomic`; registra o engolido em `core/observ.py:91`. O registro incrementa contador total e por operação, em memória do processo, e escreve log ERROR com causa/traceback: `core/observ.py:75`, `:77`, `:79`.

O `atomic` da abertura está em `chamados/models.py:2770`; a recusa levantada em `chamados/models.py:2867` sai dele antes da captura do sinal. Interrompe o restante da geração/julgamento atual — laços em `chamados/services/disputa_emissao.py:312`, `chamados/services/fabrica_da_celula.py:145` e `ponto/services/cartorio.py:995` — e retorna ao escritor que disparou o sinal. O sinal não propaga essa recusa para desfazer a transação externa; o `save` original pode terminar e seu chamador continuar, pois o `except` está em `core/observ.py:90`. Não há rollback global do julgamento no receiver: `chamados/signals.py:194`.

2. **Reconciliador: a recusa pode nascer na emissão indireta, mas é capturada no sinal anterior**

Entrada por batida: `ponto/signals.py:207` → `reconciliar_chamados_colab` em `ponto/signals.py:215`. Entrada por filho que morre: `chamados/signals.py:337` → callback em `chamados/signals.py:350` → `reconciliar_chamados_colab` em `chamados/signals.py:355`, registrado por `on_commit` em `chamados/signals.py:356`.

O reconciliador passa por perguntas absolvidas (`chamados/reconciliador.py:1428`), órfãs (`:1429`) e premissas mortas (`:1437`).

* Órfã: laço `chamados/reconciliador.py:1520` → `reatar_pergunta_orfa` em `:1522` → reemissão em `:1617` → `ChamadoColaborador.abrir` em `:910` → sinal `chamados/signals.py:188` → cadeia do item 1.
* Premissa morta: laço `chamados/reconciliador.py:1452` → `_retratar` em `:1460` → `atomic` em `:522` → `ch.reconciliar` em `:523` → `_gravar_estado`, cujo `atomic` está em `chamados/models.py:280` e `save` em `:296` → mesmo sinal e cadeia do item 1.

**Os `except Exception` externos do reconciliador em `chamados/reconciliador.py:1524` e `:1462` não recebem `ReusoSemRechave` por essas cadeias: ela já foi engolida pelo sinal em `core/observ.py:90`.** Assim, por essa recusa, continuam o laço de outras órfãs (`chamados/reconciliador.py:1520`) e o de outros chamados (`:1452`); os `atomic` externos `_retratar`/estado não são desfeitos por ela (`chamados/reconciliador.py:522`; `chamados/models.py:280`).

Na geração há outro `except Exception`, em `chamados/services/disputa_emissao.py:466`, que protege apenas o **reatamento posterior ao nascimento** (`atomic` em `:463`, chamada em `:464`). Ele não envolve o `abrir` em `:444`; portanto, não captura a recusa levantada nessa abertura. O laço só adicionaria o resultado depois, em `:470`.

O command atual `reconciliar_chamados` é vigia: importa consultas em `chamados/management/commands/reconciliar_chamados.py:49`, percorre consultas em `:71`/`:91`/`:97` e imprime contagens em `:102`; não chama o escritor `reconciliar_chamados_colab` nem `PerguntaDisputa.abrir` nesses caminhos.

3. **Outros sinais do cartório**

`ponto/signals.py:132`/`:139` (save/delete Batida), `:144`/`:151` (save/delete Ausencia), `:162` (save Justificativa) → `_julgar_dias`, em `ponto/signals.py:85` → laço de dias em `:92` → `cartorio.julgar_celula` em `:93` → cadeia do item 1.

Aqui `engolir` envolve **todo laço de dias** (`ponto/signals.py:88`): a primeira recusa encerra os demais dias desse receiver; é capturada como `Exception` em `core/observ.py:90`, com log/contador em `:75`/`:79`. A escrita original não é desfeita por propagação dessa recusa. Os outros receivers seguem; por exemplo, o reconciliador da batida é outro receiver em `ponto/signals.py:207`.

4. **Bordas humanas: censo dos dois outros callsites diretos**

* URL `chamados/urls.py:42` → view `chamados/views.py:1041` → `declarar_hora_colab` em `chamados/views.py:1090` → `atomic` em `chamados/services/declaracao_texto.py:324` → `PerguntaDisputa.abrir` em `:348`.
* URL `chamados/urls.py:14` → view `chamados/views.py:359`, ação em `:478` → `aplicar_declaracao` em `:480` → `atomic` em `chamados/services/declaracao_texto.py:626` → `PerguntaDisputa.abrir` em `:636`.

Não há `except` envolvendo essas chamadas nos corpos apresentados; uma exceção levantada atravessa e desfaz esses `atomic`. O trabalho posterior não executa: resposta em `chamados/services/declaracao_texto.py:657`/`:663`, validação em `:666`, ou push/auditoria da declaração em `chamados/views.py:1094`/`:1098`.

**Ressalva de alcançabilidade:** esses dois callsites humanos escolhem exclusivamente os quatro motivos MARCO (`chamados/services/declaracao_texto.py:42`, `:322`, `:601`), e não fornecem `batida_origem` (`:348`, `:636`); a derivação de episódio retorna `None` para MARCO (`chamados/models.py:3364`). O código procura MARCO pelo dia (`chamados/models.py:2686`, `:2713`), e seu dia do fato vem da própria data gravada (`chamados/services/fio.py:1334`). Portanto, são callsites de `abrir`, mas ocorrência neles do cenário **BATIDA D0/DIA** descrito pelo usuário não foi demonstrada: [não verificado].

5. **Capturas dos demais receivers de chamados**

Os `except Exception` em `chamados/signals.py:46`, `:116`, `:141` e `:164` protegem respectivamente fechamento do pai (`:42`), resolução (`:115`), retomada de regularizações (`:137`) e materialização (`:163`). Não são capturas diretas de `PerguntaDisputa.abrir`; eventual emissão pelo save de chamado/batida volta aos sinais do cartório já descritos. Seus próprios logs estão em `chamados/signals.py:47`, `:117`, `:142`, `:165`.

A ocorrência real de qualquer desses caminhos em produção e o conteúdo efetivo de seus logs/contadores não estão no anexo fornecido: [não verificado].

## 08/11 15:10 · sessao 50d338b99a3b · - · - · origem=Ronald (tmux)

Censo estático concluído sem banco, logs ou alteração de arquivos.

- A única atribuição encontrada em produção é `chamados/services/fio.py:800`, dentro de `gravar_dia_do_fato`; os caminhos rastreados são nascimento (`chamados/models.py:2922`), costura de linha sem dia (`chamados/services/fio.py:863`) e rechaveamento (`chamados/services/fio.py:989`).
- O reuso passa por esse rechaveamento (`chamados/models.py:2864`, `chamados/services/fio.py:1026`); migração e reversão também (`chamados/management/commands/migrar_data_evento_dia_do_turno.py:384`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:457`).
- A conversa nasce pelo mesmo escritor, recebendo o dia civil (`chamados/models.py:2757`, `chamados/models.py:2922`).
- A catraca exclui testes e migrações e declara limitações para formas dinâmicas; portanto não equivale a uma garantia contra qualquer escrita arbitrária (`chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`, `chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:15`).
- O censo completo de leitores, comandos, templates e migrações foi enviado ao agente principal. Fora de `app/`, há leitura do payload em `mensageria/nucleo/ferramentas.py:127`; escrita dinâmica por nome indireto permanece [não verificado].

## 08/11 12:12 · sessao b0a85d04359f · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADA a garantia universal da 15b; PROCEDENTE a cura dos contraexemplos nomeados nas portas alteradas.** Permanecem o retorno da chave com A2 vazio, a emissão de conversa sobre retratada e a admissão de retratada no fecho. (`app/chamados/services/fio.py:1310`, `app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/fio.py:1467`)

Os contraexemplos abaixo são construções pelo código. Ocorrência deles, números reais e migração integral em produção: **[nao verificado]**.

**1. Itens 3 e 5 anteriores: cura PROCEDENTE nos casos específicos; universalidade NÃO PROCEDENTE.**

| Contraexemplo | Trajeto na 15b |
|---|---|
| **3a, episódio retratado** | `_nascer` deriva a origem e consulta o helper; este chama a recusa, que transforma o dia nulo em `BatidaRetratadaNaoEFato`. Não alcança a busca nem o `create`. Repetindo sem mudança dos insumos, repete a recusa. (`app/chamados/models.py:2760`, `app/chamados/models.py:2769`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1268`; escrita em `app/chamados/models.py:2867`) |
| **3b, K sem episódio, respondida, gravada D0, admitida em DIA por A2** | O episódio vivo do pedido fornece DIA; o universo alcança K, a eleição prefere sua resposta e a costura recebe explicitamente `_dia_fato=DIA`. Não relê D0 como identidade nessa costura. (`app/chamados/models.py:2769`, `app/chamados/models.py:2800`, `app/chamados/models.py:2814`, `app/chamados/models.py:2829`; `app/chamados/services/fio.py:1005`; `app/chamados/juizes.py:2006`) |
| **Variante R, canônica respondida de episódio retratado** | O fecho ainda pode eleger R, mas `_nascer` chama `recusa_batida_retratada(ex)` antes de alterar célula ou destino, inclusive sem destino pedido. Agora recusa pelo motivo correto. (`app/chamados/models.py:2814`, `app/chamados/models.py:2819`, `app/chamados/models.py:2822`) |
| **3a/3b originais, episódio vivo** | Dia, célula, busca e nascimento usam `_dia_fato`; no reuso, a costura recebe esse mesmo dia. A data gravada antiga é preservada. (`app/chamados/models.py:2769`, `app/chamados/models.py:2789`, `app/chamados/models.py:2800`, `app/chamados/models.py:2829`, `app/chamados/models.py:2867`; `app/chamados/tests/test_e1_v15.py:98`) |

A resolvedora chamada diretamente também passou a consultar A2 para K e a recusar R. (`app/chamados/juizes.py:2008`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1309`)

**2. Retorno do pedido/gravado: PROCEDENTE para acervo com A2 vazio.**

Para linha de **fato com episódio**, não há mais esse fallback: recusa quando o juiz não responde; caso contrário, retorna `_dia_do_fato`. Conversa retorna antes desse ramo, portanto continua dispensada mesmo se receber origem explícita. (`app/chamados/services/fio.py:1298`, `app/chamados/services/fio.py:1300`)

Para acervo sem episódio e sem marco, permanece literalmente:

```python
alc = _dias_alcancados_sem_dia(...)
if not alc:
    return d
```

`d` é pedido, quando fornecido, senão gravada. Logo, “nenhum caminho devolve pedido/gravado para acervo” é **NÃO PROCEDENTE**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`)

**A admissão do vazio é compartilhada; a atribuição de um dia não é a mesma operação.** Universo e fecho aceitam alcance vazio porque `set() <= {dia}`. Isso prova compatibilidade com um dia fornecido, não identifica um dia único. Sem dia fornecido e sem alcance, o fecho conserva `_dia=None`. (`app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`, `app/chamados/services/fio.py:1413`)

Contraexemplo: K isolada, sem episódio/marco, gravada D0. O helper retorna D0; com `pedido=DIA`, retorna DIA. O fecho de K sem dia não infere nenhum; com `dia=DIA`, conserva K. Portanto o helper escolhe a chave onde A2 não respondeu um dia. Esse percurso foi reproduzido em memória com as funções reais e vizinhança vazia; persistência Django: **[nao verificado]**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1311`, `app/chamados/services/fio.py:1414`, `app/chamados/services/fio.py:1433`)

**3. Censo de chamadores e efeito da recusa.**

Censo das chamadas explícitas de produção, incluindo os apelidos encontrados; chamadas dinâmicas arbitrárias: **[nao verificado]**.

| Função | Chamador e efeito |
|---|---|
| `resolvedora_valida` | **Plano de fechamento:** uma pergunta retratada que precise reatar deixa de ter destino admissível. (`app/chamados/models.py:392`) |
| `resolvedora_valida` | **Nascimento, proxy inicial:** usa o dia já calculado; a origem retratada já foi recusada pelo helper. (`app/chamados/models.py:2769`, `app/chamados/models.py:2781`) |
| `resolvedora_valida` | **Reuso:** usa o dia explícito do fato, após recusa da canônica retratada. (`app/chamados/models.py:2819`, `app/chamados/models.py:2829`) |
| `resolvedora_valida` | **Reapontamento:** retratada com destino não nulo recusa; descostura sem datação continua permitida pelo retorno antecipado para `chamado=None`. (`app/chamados/services/fio.py:865`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Rechaveamento:** recusa retratada com destino; sem destino, retorna antes da recusa e pode persistir o dia recebido, passando as demais guardas. (`app/chamados/services/fio.py:970`, `app/chamados/services/fio.py:974`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Auditoria I8b:** retratada viva com destino/data passa pela classificação de resolvedora inválida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`, `app/chamados/management/commands/auditar_invariantes_chamados.py:896`) |
| `resolvedora_valida` | **Auditoria I22:** chamado vivo cujas resolvedoras de fato sejam todas recusadas entra na violação; conversas são excluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1045`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1048`) |
| `dia_do_turno_da_linha` | **Resolvedora**, **nascimento** e **datação no reapontamento**. (`app/chamados/juizes.py:2008`; `app/chamados/models.py:2769`; `app/chamados/services/fio.py:846`) |
| `recusa_batida_retratada` | **Resolvedora com dia explícito**, **reuso da canônica**, **ramo de episódio do helper** e **confirmação do contador**. (`app/chamados/juizes.py:2005`; `app/chamados/models.py:2819`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1544`) |

**Fechamento recusado: PROCEDENTE, condicionalmente.** O caminho exato é:

1. `_ancoradas` reúne não validadas pelo destino ou pelo chamado da disputa. `_vivas_entre` conserva as de competência aberta que continuam vivas pelo C6 no pós-estado. (`app/chamados/models.py:344`, `app/chamados/models.py:364`, `app/chamados/models.py:368`)
2. O plano procura sobrevivente usando **data gravada**; havendo sobrevivente, consulta a resolvedora. (`app/chamados/models.py:390`, `app/chamados/models.py:392`)
3. Resolvedora → helper → recusa retratada → `(False, texto)`. O plano devolve `reatas=None`. (`app/chamados/juizes.py:2008`, `app/chamados/juizes.py:2009`; `app/chamados/services/fio.py:1301`; `app/chamados/models.py:393`)
4. O gate devolve `False`; reconciliação/cancelamento abortam antes de gravar o fechamento. (`app/chamados/models.py:422`, `app/chamados/models.py:431`, `app/chamados/models.py:685`, `app/chamados/models.py:786`)

Isso pode impedir um fechamento que antes conseguiria reatar a pergunta. Não vale para toda retratada: ela precisa sobreviver aos filtros anteriores. Sem viva, o gate permite. (`app/chamados/models.py:364`, `app/chamados/models.py:368`, `app/chamados/models.py:420`)

O motivo específico é perdido nesse plano: o registro usa `pergunta_viva_na_competencia_aberta` e afirma ausência de cobrança receptora, inclusive quando a cobrança existe e a pergunta foi recusada. (`app/chamados/models.py:392`, `app/chamados/models.py:425`)

**4. Caminho que ainda gera conversa sobre retratada: PROCEDENTE.**

Contraexemplo: `ultima_batida.id=br` retratada, motivo `gps_ausente`, sem data no contexto do motivo e sem conversa anterior. O motivo é BATIDA e sua resposta é `escolha`. (`app/chamados/catalogo/motivos.py:42`; `app/chamados/catalogo/perguntas.py:88`)

O derivador aceita a origem pelo dono, sem filtrar retratação; assim, não entra no dedup inicial reservado à ausência de episódio. O juiz retorna `None`, `_dia_p` fica nulo e somente perguntas de `hora` são descartadas. (`app/chamados/models.py:3316`; `app/chamados/services/disputa_emissao.py:325`, `app/chamados/services/disputa_emissao.py:391`, `app/chamados/services/disputa_emissao.py:424`)

O emissor então chama **`abrir_conversa`**. A conversa não deriva origem, o helper retorna o dia civil antes da recusa e a resolvedora também aprova conversa antecipadamente. É o bug no chamador abrangido pelo aval fornecido. (`app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/disputa_emissao.py:439`; `app/chamados/models.py:2756`, `app/chamados/models.py:3297`; `app/chamados/services/fio.py:1298`; `app/chamados/juizes.py:1997`)

Também há reabertura sem a recusa nova: pergunta respondida sobre retratada, sem materialização, pode entrar em `perguntas_corrigiveis`, alcançar `reabrir_por_colab` e voltar a pendente pelo `update` de `reabrir_para_correcao`. Se “reatar pergunta” abrange voltar a pedi-la, é outro escape. (`app/chamados/juizes.py:1412`, `app/chamados/juizes.py:1245`; `app/chamados/models.py:2226`, `app/chamados/models.py:2235`; `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1533`)

**5. Ramo antigo `FatoEmOutroDia`: inalcançável com identidade íntegra; não há prova universal de inacessibilidade.**

Com episódio vivo, o helper obtém D e o memo por episódio fornece D às linhas desse episódio. Havendo linha da mesma base, ela semeia `datadas`; o universo não fica vazio. Episódio retratado recusa antes. Portanto o comentário de que o ramo “sobra só para retratada” ficou obsoleto. (`app/chamados/services/fio.py:1243`, `app/chamados/services/fio.py:1465`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1301`; `app/chamados/models.py:2807`)

Há, porém, este **contraexemplo de acervo inconsistente**:

- P pertence à disputa de C2 e tem origem explícita b, batida viva de C1. O `save` confere colaborador contra disputa, mas só deriva a origem quando ausente; a checagem do dono existe no derivador. (`app/chamados/models.py:3282`, `app/chamados/models.py:3297`, `app/chamados/models.py:3316`)
- `abrir(C1,b)` obtém D. `do_fato` encontra P pelo episódio/motivo sem filtrar colaborador. (`app/chamados/models.py:2769`, `app/chamados/models.py:2647`)
- O universo recusa P pela **base C2**, fica vazio e entrega P nas recusadas. O ramo antigo encontra o mesmo episódio e levanta `FatoEmOutroDia`, embora a divergência seja de dono. (`app/chamados/services/fio.py:1463`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1486`; `app/chamados/models.py:2802`, `app/chamados/models.py:2810`)

Esse acervo e um chamador atual de produção que o persista: **[nao verificado]**. A porta `abrir` 15b de C2 já recusaria a origem incompatível antes desse nascimento. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1301`)

**6. Retratada admitida no fecho por A2: PROCEDENTE.**

`_dia_do_fato` retorna nulo para a retratada; universo e fecho tratam esse nulo pelo mesmo ramo de A2, sem chamar a recusa. A premissa do teste novo confirma que R pode ser a canônica do universo de DIA. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`; `app/chamados/tests/test_e1_v15b.py:132`)

Consequências concretas:

- **Eleição:** R respondida vence A viva; `_nascer` recusa R depois da eleição, sem tentar outra canônica. Uma solicitação legítima sobre A pode, portanto, ser barrada por R. (`app/chamados/services/fio.py:1005`; `app/chamados/models.py:2814`, `app/chamados/models.py:2819`)
- **Dedup/backfill:** continuam elegendo sobre esse universo. O plano conta R como episódio; A+R pode produzir `episodios_unidos`, impedindo retratação. (`app/chamados/management/commands/dedup_perguntas_fato.py:76`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`; `app/chamados/services/fio.py:1060`, `app/chamados/services/fio.py:1065`)
- **Migração de resolvedoras:** também elege R; quando tenta reapontá-la, pode receber a recusa nova. A eleição em si não foi saneada. (`app/chamados/services/fio.py:1753`, `app/chamados/services/fio.py:1772`)
- **Migração de data:** R pode integrar o universo do grupo e seu dia gravado participar do veto de competência. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:254`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:277`)

**7. Contador: PROCEDENTE no universo declarado; não conta todas as perguntas/conversas sobre retratada.**

Conta linhas persistidas com `conversa=False`, origem preenchida, fora de `VIAS_FORA_DO_FATO`, cuja origem esteja fora de `batidas_apuraveis(None)`. Não filtra viva, resposta, validação, destino ou horizonte. (`app/chamados/services/fio.py:1537`)

A autoridade consultada inclui todos os colaboradores e define apuráveis por `retratada_em IS NULL`. Cada candidata é confirmada pela **função real** da recusa, acumulando somente `BatidaRetratadaNaoEFato`. (`app/ponto/turnos.py:2865`, `app/ponto/turnos.py:2871`; `app/chamados/services/fio.py:1544`)

O leitor imprime número, esperado, motivo e pks no backfill, antes da distinção entre DRY e apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:534`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`)

Não conta tentativas recusadas sem INSERT, conversas do escape do item 4 ou linhas já fora do fato. Portanto não serve como prova de ausência universal do bug nem como contador de tentativas recusadas. (`app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1538`; `app/chamados/services/disputa_emissao.py:439`)

**8. Catraca: PROCEDENTE a mordida oficial; garantia semântica completa NÃO PROCEDENTE.**

Os 11 métodos AST de P13/V15 passaram na execução em memória, substituindo `SimpleTestCase` por `unittest.TestCase`, sem executar Django/BD. Isso inclui os 13 mutantes oficiais e o mutante adicional do parâmetro do fecho. Suíte Django/BD: **[nao verificado]**. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:141`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:235`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:263`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:304`)

Os escapes demonstrados em memória incluem:

| Escape | Por que passa |
|---|---|
| `alc = set()` | Fiscaliza formato dos retornos, não a procedência de `alc`; passa a devolver sempre a chave no acervo. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:148`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:157`) |
| Recusa chamada com proxy `conversa=True` | Verifica nome/posição, não argumentos; esse proxy é dispensado pela recusa. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:164`; `app/chamados/services/fio.py:1266`) |
| Guarda inicial da resolvedora trocada por `if True` | A fiscalização das atribuições não prova que o fluxo chega nelas. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:121`; retorno em `app/chamados/juizes.py:1997`) |
| `_dia += timedelta(days=1)` | A ligação ao juiz só acompanha `Assign`, ignorando `AugAssign`. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:71`) |
| `p.data_evento: object = data_evento` dentro da porta | V15 ignora `AnnAssign`; P13 detecta localização e aceita a porta. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:90`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:119`) |
| `update(**kw)` / `bulk_update(ps, campos)` com variáveis | P13 não acompanha os valores dessas variáveis. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`) |

Rechaveamento continua fiscalizado pelo **arquivo chamador**, sem provar o valor recebido ou escrito. A aceitação explícita de `return d` com A2 vazio também mantém verde o problema do item 2. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:217`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:260`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:155`)

**9. Escritores, leitores e significados.**

| Campo/valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| `data_evento` | Nascimento `_dia_fato`; datação `_dia`; rechaveamento `novo_dia`. Apply obtém juiz; reversão obtém arquivo. (`app/chamados/models.py:2867`; `app/chamados/services/fio.py:849`, `app/chamados/services/fio.py:968`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`) | Busca/índice pela gravada; marco pela gravada; C6, tranca e plantio continuam lendo gravada. Costura usa helper ou parâmetro explícito. (`app/chamados/models.py:2678`, `app/chamados/models.py:3332`; `app/chamados/services/fio.py:1250`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:2006`; `app/chamados/services/materializacao.py:345`) |
| `celula` | Nascimento/reuso escolhem por `_dia_fato`; rechaveamento por `novo_dia`. (`app/chamados/models.py:2789`, `app/chamados/models.py:2823`, `app/chamados/models.py:2868`; `app/chamados/services/fio.py:965`, `app/chamados/services/fio.py:969`) | C6 exige compatibilidade entre dia da célula e `data_evento`; guarda terminal consulta o pós-estado. (`app/chamados/juizes.py:638`; `app/chamados/services/fio.py:779`) |
| `chamado_destino` | Nascimento, reuso após guardas, reapontamento. (`app/chamados/models.py:2868`, `app/chamados/models.py:2839`; `app/chamados/services/fio.py:877`) | Âncora do fio, fechamento, guarda terminal e plantio. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:345`; `app/chamados/services/fio.py:770`; `app/chamados/services/materializacao.py:339`) |
| `batida_origem_id` | Explícita ou derivada antes do helper; inserção deriva quando ausente. (`app/chamados/models.py:2760`, `app/chamados/models.py:3297`) | Busca do episódio, juiz, recusa, contador, chave da família e veto de episódios unidos. (`app/chamados/models.py:2647`; `app/chamados/services/fio.py:1242`, `app/chamados/services/fio.py:1266`, `app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1131`, `app/chamados/services/fio.py:1060`) |
| `dia_da_chave` | Helper fornece pedido/gravada. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`) | Modifica a consulta inicial de vizinhança e sua chave de cache; passos seguintes usam as chaves dos vizinhos. (`app/chamados/services/fio.py:1336`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1367`, `app/chamados/services/fio.py:1204`) |
| Parâmetro `dia_do_fato` | `_nascer` fornece `_dia_fato` ao proxy e à canônica. (`app/chamados/models.py:2781`, `app/chamados/models.py:2829`) | Resolvedora faz a recusa e usa o parâmetro diretamente. (`app/chamados/juizes.py:2004`) |

**Divergência de significado residual: PROCEDENTE.** Reuso pode atribuir célula/destino de DIA e conservar `data_evento=D0`; a costura aceita DIA, C6 descarta a célula divergente e tranca calcula competência por D0. O teste preserva expressamente essa divergência. (`app/chamados/models.py:2823`, `app/chamados/models.py:2839`; `app/chamados/tests/test_e1_v15.py:98`; `app/chamados/juizes.py:2006`, `app/chamados/juizes.py:638`, `app/chamados/juizes.py:948`)

O uso civil da conversa tem discriminador explícito. A premissa de que todo fato já teria “gravada == juiz” não está demonstrada pela cópia. (`app/chamados/models.py:2717`, `app/chamados/models.py:2722`; `app/chamados/tests/test_e1_v15.py:99`)

**10. Classes de reprova.**

| Classe | Veredito |
|---|---|
| **S

[truncado em 20000 de 22550 caracteres]

_2893 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
