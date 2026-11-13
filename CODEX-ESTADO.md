# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 15:47** · publicado em **13/11/2026 15:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 15:47 · sessao f90fd80293fd · - · - · origem=Ronald (tmux)

Nova partição: **31 costuras nominais**, normalizadas por `(estrutura retornada, módulo emissor, módulo consumidor)`, com **6 desencontros literais de chaves emitidas sem leitura nominal de produção encontrada**. Não executei código da aplicação nem testes; existência do selo não significa resultado verde [nao verificado]. [nao verificado]

Prefixos: `S=app/chamados/services/`, `C=app/chamados/management/commands/`, `T=app/chamados/tests/`.

| # | Estrutura / emissor | Chaves emitidas | Consumidor / leitura | Desencontro literal | Selo localizado |
|---|---|---|---|---|---|
| 1 | Foto, `S/foto_e4b.py:252`, `:329` | índice `tabela:pk → {campos,alcance[{dia,comp,entregue}]}` | `C/backfill_fio_morto_e4b.py:283`, `:289`, `:302`, `:318-320`, `:346-348`: foto inteira para reversão/diff; itera nomes `dinheiro_online:*` | nenhum identificado | `T/test_e4b_c4_orm.py:509` verifica fotografia sob trava e reversão antes do ato; `T/test_e4b_foto_memoria.py:18-48` sela shape da cerca/relato |
| 2 | Delta, `S/foto_e4b.py:118`, `:141` | índice dinâmico `→ {antes,depois}`, cada lado foto anterior | `C/backfill_fio_morto_e4b.py:320-323`, `:387-393`: índices dinâmicos, lados diretos, `alcance`, `comp`, `.get('dia')=None` | nenhum identificado | `T/test_e4b_c4_orm.py:189`; cobertura precisa de toda chave do par [nao verificado] |
| 3 | Relato, `S/foto_e4b.py:199-206` | `nascimentos,push,dinheiro,linhas`; linhas têm `campos_alterados,alcance` | `C/backfill_fio_morto_e4b.py:322`, `:348`, `:397-401`: guarda tentativa e serializa **todo** relato com `json.dumps` | nenhum identificado | `T/test_e4b_foto_memoria.py:48-71` sela conteúdo do retorno; selo do par completo com comando [nao verificado] |
| 4 | Reparo passivo, `S/passivo_validado.py:319-322`, `:359-361`, `:411` | `universo,com_registro,alvo,plantada,recusada,fora_do_escopo,competencias,reversao,pauta,pauta_sem_movimento,apply,comp_pelo_instante_diverge,erros`; `disputas_fechadas` só quando há fechamento. Competência: `empresa_id,colab_id,mes,ano,exportada,alvo,plantada,regen,pauta` | `C/materializar_passivo_validado.py:104-115`, `:129`, `:244-272`, `:286-288`, `:305-308`: serializa todos recursivamente; lê contadores diretos, `pauta_sem_movimento` default `0`; nested regen `hash_depois` default `'-'`, `mudou` default `None` | nenhum identificado | `T/test_o317_passivo_pela_porta.py:288`, `:526` selam partição/reversão do retorno; par com comando [nao verificado] |
| 5 | Censo fio, `S/vigia_fio.py:356-372`, `:374`, `:488-490` | todos contadores/listas explicitados no bloco; `porta_` + classes dinâmicas; `cego` em ambos retornos | `C/vigiar_fio_morto.py:175`, `:185`, `:214-215`, `:239`: alarme por `d[k]`, `celulas_do_fio_morto`, **todo** dict preservado em carimbo | nenhum identificado | **Sim**, `T/test_o261_vigia_fio.py:787`, compara contadores reais com carimbo |
| 6 | Mesmo censo fio | idem #5 | `C/materializar_passivo_validado.py:205-211`: itera todas chaves, retém valores `int/bool` via `f.get(k)` | nenhum global: outras chaves são lidas por #5 | selo do par [nao verificado] |
| 7 | Censo alcance, `S/vigia_fio.py:571-575`, `:589`, `:629` | `alcance_universo,alcance_competencias,alcance_porta_fila,alcance_porta_pergunta,alcance_de_hoje_pra_frente,fio_morto_alcance,alcance_porta_exige_o_admin,alcance_e_detalhe,alcance_por_rota_papel,alcance_dias,alcance_dias_omitidos,alcance_cego`, e `alcance_` + causas/baldes | `C/vigiar_fio_morto.py:176`, `:189`, `:240`, `:243`: encaminha para linhas, lê `fio_morto_alcance`, persiste **todo** alcance | nenhum identificado | `T/test_o261_vigia_fio.py:787` também confere `fio_morto_alcance` em `:817`; demais shape [nao verificado] |
| 8 | Mesmo alcance | idem #7; linhas `alcance_dias=[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]`, `S/vigia_fio.py:629` | `C/backfill_fio_morto_e4b.py:78-79`: `alcance_dias`, índice `4` para `sem_porta`; `:412`: `alcance_universo,fio_morto_alcance` diretos | nenhum identificado | selo do par [nao verificado] |
| 9 | Vivas em disputa fechada, `S/vigia_fio.py:759` | `perguntas,disputas` | `C/vigiar_fio_morto.py:177`, `:194`, `:245`: ambas diretas e dict inteiro em carimbo | nenhum identificado | selo específico do par [nao verificado] |
| 10 | Materializada viva, `S/vigia_fio.py:851`; chaves da origem `:810` | `aberta,exportada`, valores contagens | `C/vigiar_fio_morto.py:179`, `:202`, `:239`, `:244`: ambas diretas | nenhum identificado | selo específico do par [nao verificado] |
| 11 | Censo carimbo, `S/vigia_carimbo.py:121-123`, `:132` | `universo,com_registro,resposta_enviada_sem_gravacao,sem_plantio_nenhum,plantio_fora_do_slot,slot_nao_derivavel,perguntas_sem_gravacao,cego` | `C/vigiar_carimbo_sem_registro.py:63-74`: lista `perguntas_sem_gravacao`, alarme por `d[k]`, encaminha dict inteiro a `linhas` | nenhum identificado | `T/test_o306_vigia_carimbo.py:148`, `:196`, `:225` selam partições, linhas, alarmes; contrato completo do comando [nao verificado] |
| 12 | Mesmo censo carimbo | idem #11 | `C/materializar_passivo_validado.py:205-210`: lê por subscript todas chaves **menos** lista de perguntas; lista consumida por #11 | nenhum identificado | selo do par [nao verificado] |
| 13 | Diagnóstico reuso, `S/fio.py:1114-1117` | `fonte,n,esperado,motivo,pks,universo,universo_rotulo` | `C/backfill_decisoes_pergunta.py:553-556`: lê diretamente todas **menos `fonte`** | **`fonte` sem leitura nominal de produção encontrada** | contador selado em `T/test_e1_v15d_um_ponto_de_escrita.py:320`; selo da leitura/omissão de `fonte` [nao verificado] |
| 14 | Diagnóstico sem dia, `S/fio.py:1181-1184` | mesmas 7 chaves #13 | `C/backfill_decisoes_pergunta.py:559-562`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | `T/test_e1_v15e_contrato_de_saida.py:293` sela contador; par [nao verificado] |
| 15 | Diagnóstico destino alheio, `S/fio.py:1206-1208` | mesmas 7 chaves #13 | `C/backfill_decisoes_pergunta.py:565-568`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | selo específico do par [nao verificado] |
| 16 | Diagnóstico batida retratada, `S/fio.py:1902-1903` | `fonte,n,esperado,motivo,pks` | `C/backfill_decisoes_pergunta.py:545-547`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | `T/test_e1_v15b.py:165` sela contador; par [nao verificado] |
| 17 | Fora do índice, `S/fio.py:2001-2007` | `marco_sem_dia,batida_sem_origem,batida_sem_dia` | `C/backfill_decisoes_pergunta.py:486`, `:489`, `:574`: três chaves diretas | nenhum identificado | selo do par [nao verificado] |
| 18 | Plano retratação, `S/fio.py:1281-1282`, `:1287` | `retrata,merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` | `C/backfill_decisoes_pergunta.py:290`, `:396`, `:510-514`: `retrata,merito_recusado` e categorias de `NAO_RETRATA` por chaves dinâmicas; leitura de `canonica_sem_episodio` neste módulo não localizada [nao verificado] | nenhum global: `canonica_sem_episodio` lida por #19 | `T/test_e1_v10.py:142-167` sela plano; par com backfill [nao verificado] |
| 19 | Mesmo plano retratação | idem #18 | `C/dedup_perguntas_fato.py:81-88`: categorias `NAO_RETRATA`, `canonica_sem_episodio,retrata`, todos subscripts | nenhum identificado | **Sim**, `T/test_e1_v10.py:183-201`, executa comando e confere contagem do plano |
| 20 | Resultado retratação, `S/fio.py:1343` | `retratadas`, mais `merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` do plano após `pop('retrata')` em `:1342` | `C/backfill_decisoes_pergunta.py:331`: só `['retratadas']`; chamadas internas em `S/fio.py:2181` também só leem essa chave, `:922` ignora retorno | **quatro metadados restantes sem leitura nominal de produção encontrada desse retorno** | testes do ato existem `T/test_e1_v15g.py`; selo dessas chaves do retorno [nao verificado] |
| 21 | Resultado migração, `S/fio.py:2080`; enriquecido por `_migrar_sob_trava` em `:2121` | `migradas,duplicatas,recusadas`; listas | `app/chamados/models.py:1416`, `:1430`, `:1436`: lê `migradas` para mensagem; espalha **todo** resultado em payload de auditoria | nenhum identificado | `T/test_e1_v5_chave_familia.py:114` sela resumo/trilha; verificar alcance exato do reabrir [nao verificado] |
| 22 | Resultado consumidor outbox, `app/chamados/reconciliador.py:1340-1341` | `eventos,colabs,rejuntados,perguntas,retratados,premissa,orfas,lastro,disputas_barradas,adiados,falhas` | `C/consumir_eventos_celula.py:31-35`: interpolação lê `eventos,colabs,retratados,premissa,orfas,disputas_barradas,adiados,falhas` | **`rejuntados,perguntas,lastro` sem leitura nominal de produção encontrada no único caller** | `T/test_c20_juiz3_outbox.py` e `T/test_orfas_nao_morre_na_guarda_da_porta.py:146-152` leem resultado em testes; selo desse par de saída [nao verificado] |
| 23 | Alimentação fila, `app/chamados/juizes.py:1549` | `verbo,fila_ids,fila_cel` | `S/tarefa_do_dia.py:529-532`, `:546-549`: três diretas; `fila_cel.get(celula_id,())` | nenhum identificado | **Sim**, `T/test_e3_portas_tarefa.py:113-127` exige uma alimentação e confere verbos; outros cenários em `:32-100` |
| 24 | Alimentação portas, `app/chamados/juizes.py:1585-1587` | anteriores + `vivas,colab,no_app,instancias` | `S/vigia_fio.py:604-621`: `fila_ids,vivas,instancias`, e `_no_app` recebe todo mapa; porta também recebe mapa em `:635` | nenhum identificado | `T/test_n1_alimentacao_unica.py:74-86`; `T/test_n1_perguntas_do_app.py:395-418` |
| 25 | Mesma alimentação portas | idem #24 | `app/folha/fora_da_folha.py:62`, `:67`: repassa estrutura inteira ao juiz da porta, sem acesso direto às chaves | nenhum identificado | `T/test_o261e_f5b_juiz_porta.py:180-189` sela paridade lote/avulso; par folha em `:114-119` |
| 26 | Mesma alimentação portas | idem #24 | `app/escala/services/cadastro_realidade.py:484-488`: `vivas` direto, `.get(pk) → ()`; repassa mapa inteiro à porta | nenhum identificado | selo específico desse par [nao verificado] |
| 27 | Mesma alimentação portas | idem #24 | `C/diff_e3_sombra.py:114`, `:124`, `:128`, `:62-64`: `fila_cel.get(cel_id,())`, `vivas.get(cel_id,())`, `fila_ids`; repassa à porta | nenhum identificado | `T/test_e3_diff_sombra.py`; cobertura específica de chaves [nao verificado] |
| 28 | Porta alcançável, `app/chamados/juizes.py:1667-1668` | `rota,fila,verbos,fio`, ou `None` em `:1655/:1664` | `app/folha/fora_da_folha.py:78`, `:225`: `fio` direto; `(porta or {}).get('fila') or ()` | nenhum identificado | **Sim**, `T/test_o261e_f5b_juiz_porta.py:114-119`, `:167` |
| 29 | Mesma porta | idem #28 | `S/vigia_fio.py:641-646`: `verbos,rota` diretos; guarda `is not None` | nenhum identificado | **Sim**, `T/test_o261e_f5b_juiz_porta.py:119`, `:125` |
| 30 | Mesma porta | idem #28 | `app/escala/services/cadastro_realidade.py:488`: só compara estrutura com `None`; não lê chave | nenhum identificado | selo específico desse par [nao verificado] |
| 31 | Mesma porta | idem #28 | `C/diff_e3_sombra.py:47-57`, `:124`, `:139-140`: `fio,fila,rota,verbos`; se não há porta usa `'nada',(),None,[]/False`; compara estrutura com `None` | nenhum identificado | `T/test_e3_diff_sombra.py`; cobertura precisa das quatro chaves [nao verificado] |

Nos **seis desencontros** (#13–16, #20, #22), o dono provável é **ESTRUTURA** pela natureza do contrato no código, sem conclusão de impacto de produção [nao verificado]. Há repetição em todos os caminhos desses retornos, portanto são **classe sim como desencontro literal**; isso não demonstra erro de negócio [nao verificado].

Estruturas examinadas sem costura externa nominal encontrada: `S/foto_e4b.py:169` (`nascimentos`, só consumida internamente por `relato` em `:199`), `S/vigia_fio.py:810` (`carimbos_sem_lastro_por_dono`, só reduzida internamente em `:850-851`), `S/fio.py:2032-2033` (`batida_sem_origem_no_horizonte`, nenhum caller nominal de produção encontrado [nao verificado]). O último emite `fonte,n,esperado,dono,universo,novas,acervo,por_dono`; não contei um par inexistente.

Quanto à partição anterior: `nomear_marco_por_lampada.py:109` de fato repete o shape com `motivo='lampada_apagada'`, confirmado por `T/test_emissor_nomeia_o_marco.py:159-162`; é origem distinta do mesmo escritor físico `services/contexto.py:102`, para a normalização que você escolheu.

## 13/11 15:47 · sessao bcbd0a8413ab · - · - · origem=Ronald (tmux)

Censo adicional por AST das chamadas `JsonResponse`, `render`, `resposta_render`, `render_to_string`, seguido das rotas e dos leitores JS/templates. Nenhum teste executado. `app/chamados/urls.py:24-45` prende a identidade das rotas JSON; templates emitem as URLs concretas em `app/templates/chamados/partials/_lista_chamados.html:82-85`, `app/templates/core/_copiloto.html:16`, `app/templates/chamados/painel_gestao.html:59`, `:302`.

**JSON: 8 pares com leitura de corpo comprovada.** Agrupei ramos sucesso/erro da mesma função e todos os handlers do mesmo arquivo consumidor.

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | Desencontro | Selo que prende |
|---|---|---|---|---|---|---|
| Resposta cobrança individual | `app/chamados/views_cobrar.py:33`, `:43`, `:53`, `:57` | `ok`; erro `erros[]`; sucesso `hora` | `app/templates/chamados/partials/_js_cobranca.html:51-56` | `ok`, `hora`; `erros || ['Falha na cobranca.']` | Não localizado | Par JS/schema: não localizado [nao verificado] |
| Resposta cobrança em massa | `app/chamados/views_cobrar.py:109`, `:139` | Prévia: `ok,previa,colabs,chamados,ja_cobrados,sem_app,fora_da_folha_exportada,fora_da_folha_sem_data,sem_data_por_modulo,a_enviar`; execução: `ok,enviados,ja_cobrados,sem_app,falhas,colabs,fora_da_area` | `app/templates/chamados/painel_gestao.html:59-69` | Prévia: `a_enviar,chamados,colabs,ja_cobrados,sem_app`; execução: `enviados,ja_cobrados,sem_app,falhas` | Consumidor omite exclusões da folha/área e `ok`; ausência de outros consumidores desses campos [nao verificado] | Par exato: não localizado [nao verificado] |
| Resposta validar inline | `app/chamados/views_cobrar.py:159`, `:167`, `:196-198`, `:213` | `ok,erros`; conforme ramo `aguardando_colab,resposta_no_fio[]`; sucesso `validadas,avisos,pendentes` | `app/templates/chamados/partials/_lista_chamados.html:145-161` | `ok,validadas,pendentes,aguardando_colab`; `erros[0] || avisos[0] || fallback`; `resposta_no_fio[].data || ''`, `.texto_do_fio || ''` | Não localizado | `app/chamados/tests/test_e3_validar_inline.py:33`, `:40`, `:48` cobre endpoint; par JS exato [nao verificado] |
| Resposta validar lote JSON | `app/chamados/views_cobrar.py:251-267` | `ok,validadas,recusados`; erros `erros[]`; sucesso `nao_gravados[]` | `app/templates/core/_copiloto.html:248-255` | `recusados && recusados.length`, `validadas`; não lê `ok`, `erros`, `nao_gravados` | `nao_gravados` não informado por este consumidor; `recusados=[]` produz “Todos entraram” mesmo que existam `nao_gravados`. Leitor HTML separado existe em `_lote_preview.html:9`, portanto não é chave órfã global | Par exato: não localizado [nao verificado] |
| Proposta lote JSON | `app/chamados/views_cobrar.py:285`, `:290-302` | `ok,refs,elegiveis,erros` na recusa; sucesso `elegiveis_chamados,rotulo_lote,amostra[{ref,colaborador,pergunta,resposta,hora_prevista,diferenca_min}],fora_do_lote,sem_classificacao` | `app/templates/core/_copiloto.html:228-242` | `fora_do_lote[*],elegiveis,sem_classificacao,amostra[].{colaborador,resposta,hora_prevista},amostra.length,refs` | Não lê `elegiveis_chamados,rotulo_lote,amostra[].{ref,pergunta,diferenca_min}`; ausência global desses leitores [nao verificado] | Par exato: não localizado [nao verificado] |
| Resposta desarquivar | `app/chamados/views_arquivar.py:62`, `:71`, `:89` | Recusa área `{ok:false,erro}`; outra recusa `{ok:false,erros[]}`; sucesso `{ok:true}` | `app/templates/chamados/partials/_lista_chamados.html:85`, `:120-134` | `ok`; `erros || ['Falha ao arquivar.']`; sucesso com `resgatar=true` não lê `ate` | **Confirmado: recusa área emite `erro`, leitor só procura `erros` e mostra fallback** | `test_f3_arquivar_porta.py:64-75` prende sucesso/trilha, não o par de erro |
| Resposta colaborador | `app/chamados/views.py:1045`, `:1055`, `:1068-1102`, `:1124` | `erro`; conforme ramo `estado`; sucesso `status,estado,msg,exige_doc` ou `status,aviso` | `app/templates/chamados/meu_atendimento.html:404-416`, `:432-444`; handler livre `:217-225` | Escapes/hora: `status`, `msg || fallback`, `erro || msg || fallback`. Handler livre não lê corpo nem status HTTP | `exige_doc`, `estado`, `aviso` não lidos por estes handlers. Em resposta livre, `.then()` mostra mensagem enviada sem inspecionar recusa HTTP (`:218-224`) | Par exato: não localizado [nao verificado] |
| Placar JSON | `app/chamados/views.py:846-862`; payload calculado em `app/chamados/services/painel.py:291-299` | Canônico calculado: `verbos_contagens,registros_n,cobranca,arquivados,equipe_janelas,resolvidos_auto,aceite`; ramo lavrado copia payload e acrescenta `lavrado_em` | `app/templates/chamados/painel_gestao.html:302-321` | `verbos_contagens.{validar,cobrar,decidir,arquivado}.n`; `equipe_janelas.{hoje,h48,h72,d7}`; `resolvidos_auto.{hoje,d7}`; se `aceite`, `{enviados,abertos,respondidos}.{hoje,d7}`. Sem default para subchaves obrigatórias | `lavrado_em,registros_n,cobranca,arquivados` não lidos por este JS; writer fora chamados do payload lavrado não foi recenseado nesta partição | Par schema exato: não localizado [nao verificado] |

Nos dois desencontros fortes de interpretação do resultado (`validar_lote→copiloto`, `desarquivar→JS`), dono provável **ESTRUTURA**, classe **sim**, pelos pares acima. Incidência em produção: [nao verificado].

Cinco emissores JSON diretos adicionais ficaram **sem leitor de corpo localizado**, portanto não formei pares artificiais:

- `app/chamados/views.py:1011`, `:1016`, `:1036`, `atribuir_chamado`: `{erro,area}` ou `{status,atribuido_a}`. Forms fazem POST normal em `app/templates/chamados/fio_colaborador.html:212`, `:220`; não foi localizado JS que leia o JSON.
- `app/chamados/views.py:1132-1136`, `marcar_lido_colaborador`: `{erro}` ou `{status}`; leitor nominal não localizado [nao verificado].
- `app/chamados/views.py:1144-1152`, `marcar_lidos_colaborador`: `{erro}` ou `{status,n}`. `app/templates/chamados/meu_atendimento.html:165-167` dispara fetch e ignora resposta.
- `app/chamados/views_arquivar.py:34`, `:48`, `arquivar_chamado`: `{ok,erro}` ou `{ok,ate}`. Existe função JS que suportaria o caso, mas a única chamada concreta localizada usa **desarquivar**, em `_lista_chamados.html:85`; par arquivar→JS [nao verificado].
- `app/chamados/views_arquivar.py:106`, `:148-149`, `arquivar_lote`: `{ok,erros}` ou `{ok,arquivados,fora_da_area,ate}`; consumidor UI nominal não localizado [nao verificado].

**Render: pares principais localizados.** Todos os campos abaixo são chaves de contexto; acesso de variável Django ausente nos corpos indicados não contém `.get` explícito. Defaults escritos nos templates são assinalados.

| Estrutura | Emissor/ponte | Gravadas | Consumidor e lidas | Desencontro forte / selo |
|---|---|---|---|---|
| Form abertura | `views.py:156-162`, `:187-214`, `abrir_chamado` | `modulo,titulo_pagina,taxonomia_json,taxonomia,tipos_para_colab,pode_abrir_para_colab` | `app/templates/chamados/form_abertura.html:2`, `:8`, `:21`, `:32`, `:39`, `:145`, `:179`: todas | Nenhum localizado; selo do par [nao verificado] |
| Form abertura para colaborador | `views.py:131-134`, `_abrir_para_colaborador` | Mesmas, recebidas por `ctx_taxonomia` | Mesmo template/linhas | Nenhum localizado; selo [nao verificado] |
| Detalhe GLPI | `views.py:262-269`, `detalhe_chamado` | `ticket,followups,local,chamado,erro,is_gestor,titulo_pagina` | `app/templates/chamados/detalhe.html:4`, `:23-32`, `:73-80`: `chamado,is_gestor,followups` | `ticket,local,erro,titulo_pagina` sem leitura nominal no template; `chamado=local` pode ser `None` apesar de `ticket` existir (`views.py:258-266`), mas template usa só chamado. Efeito sem local [nao verificado]; selo [nao verificado] |
| Tabela parcial GLPI | `views.py:340`, `partial_lista_chamados` | `tickets,is_gestor` | `app/templates/chamados/partials/tabela_chamados.html:1`, `:7`, `:17`, `:23`: ambas | Nenhum localizado; selo [nao verificado] |
| Painel chamado turno | `views.py:353`, `painel_chamado_turno` | `chamado,followups` | `app/templates/chamados/partials/painel_chamado_turno.html:8-13`, `:42-45`: ambas | Nenhum localizado; selo [nao verificado] |
| Detalhe local | `app/chamados/services/acoes_chamado.py:409-424`; ponte `views.py:550-551` | `agenda_proposta,chamado,followups,is_gestor,pode_agir,area_label,titulo_pagina,disputa_aberta,perguntas_disputa,todas_validadas,folha_julga_pontualidade,advertencias_ativas` | `app/templates/chamados/detalhe_local.html:5-6`, `:45-47`, `:105`, `:184`, `:289`, `:309-314`: todas exceto `titulo_pagina` e folha, esta repassada ao include | Folha é lida no include `_pergunta_card.html:43`; selo `app/chamados/tests/test_ui_resposta_diz_o_que_e.py:229`, `:258` |
| Meu atendimento | `views.py:633-651`, `meu_atendimento` | `colaborador,chamados,justificativas,pendentes_just,nao_lidos,pendencias,solicitacoes,nova_solicitacao_no_app,aba_solicitacoes,nao_lidos_pendencias,nao_lidos_solicitacoes,titulo_pagina,cor_disputas_map,regularizacoes_pendentes,ferias_pendentes,is_admin_ti,escapes_colab` | `app/templates/chamados/meu_atendimento.html:6`, `:48`, `:63`, `:68-81`, `:117`, `:134`: regularizações, flags, contadores, justificativas, pendências, solicitações; outros consumidos pelos includes abaixo | Campos extras sem leitura própria não bastam para ausência global; selo [nao verificado] |
| Fio colaborador | `views.py:697`, `fio_colaborador` | `colaborador,ficha,ultima_geo,geo_historico,colab_posto,chamados,resumo,voltar_url,is_gestor,is_admin_ti,pode_agir_map,area_label_map,cor_disputas_map,titulo_pagina` | `app/templates/chamados/fio_colaborador.html:5`, `:10`, `:15-22`, `:79`, `:112-126`, `:140`, `:164`, `:211`, `:225`: conjunto salvo flags/título sem leitura nominal própria | Selo contexto `app/chamados/tests/test_paridade_fio_modal_f8.py:73-83`; não cobre cada chave/template |
| Painel completo | `app/chamados/services/painel.py:413-445`, `:530-548`; `views.py:727`, `:762-767` | `chamados_data,total,abertos,arquivados,em_analise,resolvidos,emergencias,page_obj,paginator,f_status,f_colab,f_verbo,verbos_contagens,registros_n,resolvidos_janelas,resolvidos_auto,validacoes_janelas,equipe_janelas,dias_atraso,f_urgencia,f_q,f_empresa,f_praca,empresa_fora_rollout,empresas,pracas,fechados,ativos,historico,cancelados,titulo_pagina`; resumo acrescenta `modo_resumo,grupos,total_colabs`; view acrescenta `he_central` ou `None` | `app/templates/chamados/painel_gestao.html:41-44`, `:78-82`, `:105-120`, `:127`, `:136-191`, `:239`: verbos, contagens, HE, filtros, empresas, praças, modo; encaminha lista aos includes | Schema grande com chaves sem consumo direto; total de ausência global [nao verificado]; selo [nao verificado] |
| Lista detalhada painel | Mesmo serviço; ponte `views.py:740-743` e include `painel_gestao.html:239` | Mesmo contexto painel; `chamados_data[]` shape em `services/painel.py:385-405` | `app/templates/chamados/partials/_lista_chamados.html:3-8`, `:30-97`: filtros/page/rollout e itens `{obj,verbo,rotulo_linha,origem_rotulo,origem_destaque,estado_title,cobrado_hoje,n_followups,ultimo_fu,nao_lido,n_nao_lidos,horas_sem_resposta,sla_estourado,sla_alerta,sla_limite}` | Desencontro de chaves forte não localizado; cobertura integral [nao verificado] |
| Lista resumo painel | `app/chamados/services/painel.py:469-489`, `:493-494`, `:529-548`; ponte `views.py:740-743` | Contexto resumo; grupos `{cid,colab,chamados,cont,urgente,mais_antigo,n,acao,tem_proposta}`; itens `{id,obj,criado_em,verbo,ord,rotulo_linha,estado_title,origem_rotulo,origem_destaque}` | `app/templates/chamados/partials/_lista_resumo.html:5-28`, `:33-71`: filtros/page/paginator/contagens/rollout/grupos e seus itens | Internos de ordenação não lidos no template; ausência global [nao verificado]; selo [nao verificado] |
| Modal completo | `views.py:927-948`, `_resp_modal_full` | `colaborador,folha_julga_pontualidade,ficha,chamados,proposta_viva,pode_agir_map,area_label_map,is_gestor,is_admin_ti,modo_beneficio,chamados_orfaos_count,proximo_fio_pk,encerrados_juiz,contador_ativos,contador_msgs_mes,contador_disputas_abertas,cor_disputas_map` | `app/templates/chamados/partials/modal_fio.html:9-19`, `:26`, `:40`, `:74-76`, `:123`, `:172`, `:250`, `:382`, `:500`, `:606-651` | **Lê `pode_conferir_data` em `:172`, omitida por este emissor.** Parcial a grava em `fio.py:474`. Selo GET `test_paridade_fio_modal_f8.py:62-71` não prende essa chave |
| Modal parcial | `app/chamados/services/fio.py:461-482`; ponte `views.py:1398-1400`, e `services/fio.py:497` | Mesmo núcleo do completo, mais `pode_conferir_data`; **sem** três contadores nem `cor_disputas_map` | Mesmo template, especialmente `modal_fio.html:14-19`, `:76`, `:172` | **Lê contadores e `cor_disputas_map` que este emissor omite; completo grava `views.py:945-948`.** Selo do par/paridade de construtores não localizado [nao verificado] |
| Mensagens rápidas | `views.py:1000-1002`, `abrir_chamado_rapido` | `chamados` | `app/templates/colaboradores/partials/msgs_lista.html:1-9`: itera chamados/followups | Nenhum localizado; selo schema [nao verificado] |
| Verificação PDF | `views.py:1203`, `:1218`, `verificar_pdf` | Erro `erro,hash_consultado`; sucesso `registro,hash_consultado,autentico` | `app/templates/chamados/verificar_pdf.html:30-42`, `:54`: todas segundo ramo | Nenhum localizado; selo [nao verificado] |
| Lente raio-X | `views.py:1482-1489`, `raiox_lente` | `grupos,total,verbo,rotulo_verbo,cor_verbo,placar,empresas,aceite,empresa_id` | `app/templates/chamados/partials/_raiox_lente.html:3-8`, `:18`, `:27-40`: todas | Nenhum localizado; selo [nao verificado] |
| Regularização painel | `app/chamados/views_regularizacao.py:34`, `regularizacao_painel` | `colaborador,lotes` | `app/templates/chamados/regularizacao_painel.html:2-14`, `:23-31`, `:45`: ambas | Nenhum localizado; selo [nao verificado] |
| Regularização edição | `app/chamados/views_regularizacao.py:67`, `regularizacao_editar` | `reg,itens,editavel` | `app/templates/chamados/regularizacao_editar.html:2-7`, `:17-29`, `:42`: todas | Nenhum localizado; selo [nao verificado] |
| Busca arquivados | `app/chamados/views_arquivar.py:178`, `busca_arquivados` | `chamados,total,q,de,ate,agora` | `app/templates/chamados/partials/_busca_arquivados.html:7`, `:12`, `:17`, `:26`, `:29-46`: todas | Nenhum localizado; selo [nao verificado] |
| Meus arquivados | `app/chamados/views_arquivar.py:205`, `meus_arquivados` | `chamados,total,de,ate,agora` | `app/templates/chamados/partials/_meus_arquivados.html:9`, `:14`, `:23-35`: todas exceto `agora` | `agora` sem leitura nominal neste template; selo [nao verificado] |
| Prévia lote HTML | `app/chamados/views_cobrar.py:358`, `:362-368`, `lote_preview` | Recusa `recusa`; sucesso `linhas,n,rotulo_lote,total,fora,sem_marco,sem_marco_rotulo` | `app/templates/chamados/partials/_lote_preview.html:2-3`, `:13-26`: todas exceto `n`; linhas `{ref,pergunta,detalhe}` com `detalhe.esperada|default:'—'`, `delta_min` condicional; sem_marco `{nome,matricula|default:'-',perguntas,cadastro}` | `n` gravada sem leitura nominal; schema dos subitens vem do serviço de validação, não inventado pela view. Selo par exato [nao verificado] |
| Resultado lote HTML | `app/chamados/views_cobrar.py:378`, `:386`, `validar_lote_tela` | Recusa `recusa`; execução `resultado,quem` | `app/templates/chamados/partials/_lote_preview.html:2-9`: `resultado.{validadas,recusados[].{ref,motivo},nao_gravados[].ref}`, `quem` | Não lê `resultado.erros` mesmo quando o serviço devolve erro. Esquema/efeito desse ramo precisa consolidar com responsável por `validacao.py`; selo par [nao verificado] |

Nos dois pares modal, dono provável **ESTRUTURA**, classe **sim**, pelas omissões concretas acima; dados reais ou impacto de produção [nao verificado]. Acrescentar **`cor_disputas_map`** ao achado do parcial, além dos três contadores: comparação de `services/fio.py:461-482` com `views.py:948`, consumo `modal_fio.html:76`.

Leitores em includes que devem ser contados separadamente caso “módulo consumidor” inclua cada template:

| Emissor da estrutura | Consumidor adicional | Chaves |
|---|---|---|
| `views.py:633-651`, meu atendimento | `app/templates/ferias/_meu_atendimento_ferias.html:1-9` | `ferias_pendentes` |
| `views.py:633-651`, meu atendimento | `app/templates/chamados/partials/_card_atendimento.html:12`, `:219`, `:398` | `chamado` é alias do laço de `pendencias/solicitacoes` do pai (`meu_atendimento.html:117-144`); `escapes_colab` e `is_admin_ti` aparecem no corpo do cartão; linhas exatas de cada acesso além das citadas [nao verificado] |
| `views.py:697`, fio | `app/templates/core/_voltar.html:3-6` | `voltar_url` |
| `views.py:697`, fio; `views.py:930`, modal completo; `services/fio.py:464`, modal parcial | `app/templates/colaboradores/partials/_cabecalho_ficha.html:7-9` | `ficha` — são três emissores para um leitor |
| `services/acoes_chamado.py:422`, detalhe local | `app/templates/chamados/partials/_pergunta_card.html:43` | `folha_julga_pontualidade`; `pode_agir` em `:42` |
| `services/painel.py:413-445`, painel | `app/templates/chamados/partials/_paginacao_chamados.html:3-11` | `paginator,page_obj,f_status,f_urgencia,f_q,f_empresa,f_praca,f_verbo,total` |

A tabela principal contém **22 pares render**, além dos **8 pares JSON**; os includes acima são expansão adicional, não estão embutidos nesses números. Não some os totais aos outros agentes antes de deduplicar pares `services/painel→template`, `services/fio→template`, `services/acoes_chamado→template`, que posso ter sobreposto ao recorte deles.

Limites: JavaScript de terceiros/minificado foi excluído depois de uma busca inicialmente ruidosa; não atribuí consumo a formas HTML que só transportam POST; `resposta_acao`/`resposta_render` recebem parâmetros de envelope e têm escritor físico em `core/respostas.py`, fora do recorte próprio desta tarefa. Ainda há essas portas a consolidar se o responsável do núcleo não as tiver inventariado (`app/chamados/views.py:19`, `views_regularizacao.py:16`, `views_cobrar.py:311`).

## 13/11 15:47 · sessao 823646fecf8a · - · - · origem=Ronald (tmux)

Complemento de renderização, somente leitura. Fiz inventário AST de `render`/`resposta_render` nos três módulos pedidos e busca lexical de referências Django nos templates, removendo `{% comment %}`, separando variáveis de loop, aliases de `{% url ... as ... %}` e parâmetros locais de includes. Os caminhos dinâmicos da triagem vêm de `app/ponto/views.py:225-235`; os aliases de espelho e formulário estão em `:192` e `:1654`. Esquemas aninhados e todos os includes transitivos não ficaram fechados; portanto este complemento não constitui censo exaustivo de costuras. [não verificado]

As 28 linhas abaixo são pares de módulo renderizador × template direto; não são uma contagem completa de gravadores, pois `triagem_batida.py` também muta o mesmo contexto entregue por `views.py` (`app/ponto/services/triagem_batida.py:200-223`, `:299-300`, `:378-387`, `:478-486`, `:758-766`).

Abreviações: `V=app/ponto/views.py`; `T=app/templates/ponto/`. `R` lista raízes lidas no template, sem globais e variáveis locais. A ausência de leitura declarada abaixo é limitada ao template direto e aos includes expressamente examinados; ausência global permanece [não verificado].

| Estrutura / emissor | Chaves gravadas | Consumidor / raízes lidas | Desencontro / selo |
|---|---|---|---|
| Contexto espelho, `V:193` | `colab_param,colaborador,dias,semanas,hoje,escala_resumo,resumo_mes,carimbo_convencao,apur_ini,apur_fim,disputa_aberta,ultimas_batidas,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox` | `T/espelho.html:7`: `colaborador,disputa_aberta,carimbo_convencao,apur_ini,apur_fim,resumo_mes,ultimas_batidas,escala_resumo,semanas,hoje,dias` | Navegação `mes/ano/...` e `colab_param` não são lidos diretamente; todos os aninhados [não verificado]. Teste parcial `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46` |
| Contexto triagem apto, `V:249`, `:330`, `:337`, `:358` | Base `colaborador`; união dos enriquecimentos abaixo | `T/triagem/apto.html:4`: `sucesso,tipo_batida,hora_registro,colaborador,saida_sem_entrada,tem_regularizacao_pendente,qtd_regularizacao_pendente,turno_aberto` | `saida_sem_entrada` lida em `:39` não é escrita no contexto pelos escritores inspecionados; o serviço grava esse nome somente no contexto da disputa em `triagem_batida.py:737-739`. Selo do desencontro [não verificado] |
| Contexto triagem regularizar, mesmos renders | mesma união | `T/triagem/turno_regularizar.html:3`: `regularizacao_enviada,entradas_sem_saida,regularizacao_erros` | Campos existem nos escritores `triagem_batida.py:205`, `:299-300`; porém o seletor usa outra chave para chegar a esse template, descrita adiante. Teste dos dados `app/ponto/tests/test_f7_ramos_frios.py:62-75` |
| Contexto triagem confirmar, mesmos renders | base + `aguardando_confirmacao=True` em `V:329`; `turno_aberto` do serviço | `T/triagem/turno_confirmar.html:6`: `turno_aberto.entrada` | Compatibilidade da raiz constatada; selo do ramo `app/ponto/tests/test_f7_ramos_frios.py:128-142` |
| Contexto fora horário, `V:343` e seletor | base + `bloqueio_horario`, `anuencia_enviada` ou `anuencia_erro` do serviço | `T/triagem/fora_horario.html:3`: `anuencia_enviada`; `bloqueio_horario.{hora_prevista,hora_tentativa,diferenca_min,escala,chamado_pk,tipo_batida}` em `:15-25` | `anuencia_erro` escrita em `triagem_batida.py:384-387` não é lida pelo template nem pelo `base_triagem.html` examinado. `test_f7_ramos_frios.py:121-125` só confere a presença no contexto |
| Contexto bater V2, `V:357` | mesma união da triagem | `T/bater_ponto_v2.html:39`: `colaborador,sucesso,tipo_batida,hora_registro,aviso_do_dia,bloqueio_horario,aguardando_confirmacao,tipo_sugerido`; `colaborador.posto.{latitude,longitude,raio_metros}` em `:139-140` | Nenhuma ausência entre essas raízes constatada; selo integral [não verificado] |
| Contexto turnos abertos, `V:409` | `alertas,total,criticos,altos,normais,nivel_filtro` | `T/turnos_abertos.html:34-58`: todas as seis raízes | Sem desencontro de raiz; selo integral [não verificado] |
| Contexto offline, `V:488` | `pendentes,total_pendentes,com_gps,sem_gps` | `T/batidas_offline_pendentes.html:19-31`: todas | Sem desencontro de raiz; selo integral [não verificado] |
| Contexto prévia HE, `V:712` | `empresa,mes,ano,colab,motivo,voltar,r,q,linhas,itens,mostrado` | `T/gestao_he_periodo_previa.html:21-91`: todas; `r.periodo.0/.1,r.dias,r.exportada,r.erros`; `linhas[].{rotulo,antes,depois,diff}` | Esquema `linhas` produzido no mesmo render (`V:717-718`) coincide com leitura `:50-54`; demais aninhados [não verificado] |
| Contexto HE, `V:951` | `empresas,empresa,mes,ano,meses,anos,retrato,calculado_em,colabs,pracas,postos,totais,filtrou,teto_tira,q,f_praca,f_posto,f_estado,estados,pode_autorizar` | `T/gestao_he.html:23-325`: `pode_autorizar,empresas,empresa,meses,mes,anos,ano,q,retrato,calculado_em,totais,teto_tira,colabs,filtrou` | `pracas,postos,f_praca,f_posto,f_estado,estados` não são lidas no template direto examinado; aninhados [não verificado]. Teste parcial `app/ponto/tests/test_tela_gestao_he.py:82` |
| Contexto painel fechamento, `V:1006` | `colab,fechamento,mes,ano,justificativas,ausencias,fechamento_pk,periodo_fechado` | `T/partials/painel_fechamento.html:6-100`: todas | A referência `fechamento.pk` em `:92` está dentro de comentário Django e não deve contar. Guarda usa `fechamento_pk` em `:80`; hidden em `:98`. Selo de tranca `app/ponto/tests/test_m4_painel_e_reavaliar_leem_o_juiz.py:93-111` |
| Contexto lote fechamento, `V:1087` | `resumos,proximo_offset,restantes,qs_lotes` | `T/_fech_lote.html:8-9`: todas; inclui `_fech_linha.html` | **Faltam `empresa_id` e `periodo_fechado` exigidas pelo include**, conforme linha adicional abaixo. Testes de lotes `app/ponto/tests/test_selo_lotes_fechamento.py:57-94` cobrem quantidade/ordem, não essas duas chaves |
| Contexto fechamento principal, `V:1179` | `empresas,empresa_id,periodo_fechado,resumos,proximo_offset,restantes,qs_lotes,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox,total_aprovados,total_pendentes,pode_aprovar,total,sem_calculo,sem_escala,n_ativos_u,n_deslig_u,n_outros_u,n_sem_ancora,n_turno_aberto,n_rescisao,n_fx0,n_fx50,n_fx80,n_fxok,n_fxcego` | `T/fechamento.html:2-266`: todas as raízes fornecidas | Sem ausência de raiz constatada; `resumos` inclui `_fech_linha.html`. Testes parciais de lotes acima |
| Contexto formulário justificativa, `V:1652`, `:1655` | `turnos_abertos,titulo_pagina` | `T/justificativa_form.html:16`, `:29`: `turnos_abertos` como pares `(entrada,saida)` | `titulo_pagina` não é lida diretamente; a navegação `voltar_url_tmp` é variável local criada por `{% url ... as ... %}` em `:4`, não chave ausente. Selo [não verificado] |
| Contexto painel justificativa, `V:1685` | `j,colab,historico,ausencias_dia,chamado,followups` | `T/partials/painel_justificativa.html:4-131`: todas | Sem ausência de raiz constatada; objetos/aninhados [não verificado] |
| Contexto fila justificativas, `V:1749` | `justificativas,status_filtro,empresa_filtro,q,total_pendentes,total_sla,total_hoje,total_backlog,empresas,titulo_pagina` | `T/fila_justificativas.html:40-98`: todas, salvo `titulo_pagina` | `titulo_pagina` não lida diretamente; selo integral [não verificado] |
| Contexto painel ausência, `V:2079` | `a,raiz,irmaos,cobre_parte_do_dia,tipos,parciais,pode_editar,escala_rotulo`; expansão `status_fundo,status_cor` de `V:2046-2049`; expansão `historico,resumo_historico,periodo_aquisitivo,pdf_historico_url` de `V:2032-2036` | `T/partials/painel_ausencia.html:23-309`: todas, mais `volta_url` em `:282` | **`volta_url` não é gravada pelo render nem pelos helpers inspecionados.** Os processors configurados estão em `app/config/settings/base.py:76-84`; nenhum processor próprio inspecionado fornece essa chave. Selo da chave [não verificado]. Teste parcial de histórico `app/ponto/tests/test_drawer_ausencia_no_molde.py:112-121` |
| Contexto lista ausências, `V:2244` | `ausencias,pendentes,colaboradores,tipos,ids_alerta_atestado,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox,filtro_colaborador,filtro_tipo,filtro_efeito,cnt,count_total,count_pendente,count_aprovada,count_rejeitada,filtro_status,de,ate` | `T/ausencias.html:2-142`: todas, exceto `pendentes`; `cnt.{abona,desconta,suprime}` em `:65-67` | `pendentes` não lida diretamente; aninhados [não verificado] |
| Contexto lançar ausência, `V:2424`, `:2459`, `:2460` | `colaboradores,tipos,parciais` | `T/lancar_ausencia.html:19-93`: todas | Sem ausência de raiz; `voltar_url_tmp` é alias local do template `:4`. Selo [não verificado] |
| Contexto submeter ausência, `V:2559`, `:2560` | `tipos,tipos_pagos,parciais,titulo_pagina` | `T/submeter_ausencia.html:24-77`: `tipos,tipos_pagos,parciais` | `titulo_pagina` não lida diretamente; selo [não verificado] |
| Contexto histórico atestados, `V:2882` | `colaborador,atestados,total_dias,empresa_paga,inss_assume,ano,alerta_15dias` | `T/historico_atestados.html:2-37`: todas | Sem ausência de raiz; selo [não verificado] |
| Contexto saída retroativa, `V:3124` | `justificativa,titulo_pagina` | `T/materializar_saida_retroativa.html:21-32`: `justificativa` | `titulo_pagina` não lida diretamente; selo [não verificado] |
| Contexto regularizar retorno, `V:3235` | `batida_entrada,data_turno,horas_aberto,dias_ausente,turnos_orfaos,titulo_pagina` | `T/regularizar_retorno.html:18-20`: `batida_entrada,horas_aberto,dias_ausente` | `data_turno,turnos_orfaos,titulo_pagina` não lidas diretamente; selo [não verificado] |
| Contexto geo, `V:3254` | `batida,resumo` | `T/partials/geo_detalhe_turno.html:4-17`: `batida.{colaborador.nome,timestamp}`, `resumo.{total_pings,pings_dentro,pings_fora,pings_mock,distancia_maxima,amostras}`; amostras `.{mock,dentro,timestamp,distancia,provider}` em `:27-34` | Raízes compatíveis; esquema do produtor de `resumo` [não verificado] |
| Contexto fila flip, `V:3307` | `itens,placar,dias,n_auto` | `T/flip_fila.html:5-15`: todas; `itens[].{cid,nome,dia,hora,bid,de,para,motivo}` | Raízes compatíveis; esquema de `itens` [não verificado] |
| Contexto tipos ausência, `app/ponto/views_tipos_ausencia.py:37` | `grupos,usos,total,titulo_pagina`; `grupos[].{efeito,rotulo,itens}` em `:36` | `T/tipos_ausencia.html:24-45`: `total,grupos[].{rotulo,itens},usos`; `itens` são objetos `TipoAusencia` | `grupos[].efeito` e `titulo_pagina` não lidas diretamente; selo [não verificado] |
| Contexto painel tipo ausência, `app/ponto/views_tipos_ausencia.py:48` | `t,usos,efeitos` | `T/partials/painel_tipo_ausencia.html:1-40`: todas; `t.{pk,nome,codigo,efeito,exige_documento,bloqueia_ponto,parcial,medico,ativo}` | Sem ausência de raiz; atributos de modelo não tratados como chaves de dict. Selo [não verificado] |
| Contexto motivos, `app/ponto/views_motivos_retratacao.py:38` | `motivos,usos,total_usos,titulo_pagina` | `T/motivos_retratacao.html:12-31`: `motivos,total_usos,usos.items` | `titulo_pagina` não lida diretamente; selo [não verificado] |

Duas costuras transitivas que merecem linha própria na consolidação:

| Estrutura | Emissor | Consumidor | Chaves lidas / desencontro | Selo / dono / classe |
|---|---|---|---|---|
| Contexto do lote herdado pelo include | `app/ponto/views.py:1087-1092`: `resumos,proximo_offset,restantes,qs_lotes` | `app/templates/ponto/_fech_linha.html:13`, `:42`, `:47`, alcançado por `_fech_lote.html:8` | Lê `empresa_id`, `periodo_fechado`, além de `r` local a `resumos`. Os dois primeiros faltam no contexto do lote. A página principal os grava em `views.py:1181-1182` | Selo específico não localizado [não verificado]; ESTRUTURA provável; classe sim por ausência entre gravador e leitor |
| Contexto da triagem no seletor | `app/ponto/services/triagem_batida.py:206`: `tem_bloqueio_turno` | `app/ponto/views.py:229`: `.get('entrada_bloqueada_turno_aberto')` | Escritor e seletor usam nomes diferentes. O ramo regularizar depende da segunda chave (`views.py:230`). Nenhum escritor da segunda no corpo inspecionado; total global [não verificado] | `app/ponto/tests/test_f7_triagem_caracterizacao.py:78` afirma `tem_bloqueio_turno`; não prende correspondência com o seletor. ESTRUTURA provável; classe sim |

Globais não devem virar falsos desencontros: `request` e `messages` vêm dos processors configurados em `app/config/settings/base.py:78-80`; `base_tpl` vem de `app/core/context_processors.py:210`. Nomes de loops (`r`, `dia`, `c`, `h` etc.), aliases de URL e parâmetros locais de includes não são exigências do dict raiz. Os atributos dentro de objetos Django também não foram confundidos com chaves de dict.

As lacunas precisas são: todos os campos aninhados de `dias/semanas/resumo_mes/escala_resumo`, `resumos[].{pront,folha,causa}`, `colabs` da HE, `historico/resumo_historico` e todas as referências herdadas dos layouts/includes comuns. Seus escritores e consumidores completos não foram fechados neste complemento. Total de costuras do módulo e quantidade completa sem selo permanecem [não verificado].

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

_4657 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
