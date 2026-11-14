# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 20:42** · publicado em **14/11/2026 20:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 20:42 · sessao dfbffc8f5424 · - · - · origem=Ronald (tmux)

Os 16 casos abaixo são **C** pela definição estrita de F: nenhuma destas falhas decorre de assinatura de dublê; as expectativas tratam C6 vivo ou ausência de chamado acionável como suficientes para decidir a porta, enquanto a lei exige a tarefa publicada e admite Resolver como ação direta. (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:46`, `app/chamados/tests/e3_lu2_entrega.txt:22`)

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: com os fatos sem vínculo desta fixture, esperar ambos mortos, `fio_morto=2`, `fio_vivo=0`; C6 vivo sozinho não constitui instância `responder` exigível (`app/chamados/tests/test_o261_vigia_fio.py:145`, `app/chamados/tests/test_o261_vigia_fio.py:87`). |
| `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: conservar os asserts de delegação C6/property, mas esperar ambas as células sem porta e `fio_morto=2`, `fio_vivo=0` nos fatos atuais sem vínculo (`app/chamados/tests/test_o261_vigia_fio.py:249`). |
| `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:630`; `app/chamados/services/vigia_fio.py:259` | LU-03/M1/M7: a resposta não validada sem âncora, vínculo ou oferta não abre porta; esperar `cel_n` no morto e contagens `(escala_nao_descreve_o_dia,fio_morto,fio_vivo)=(1,2,0)` (`app/chamados/tests/test_o261_vigia_fio.py:110`, `app/chamados/tests/test_o261_vigia_fio.py:657`, `app/chamados/tests/test_o261_vigia_fio.py:662`). |
| `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640` | LU-02/07: o command usa a alimentação real, cujo residual `decidir` dá porta; substituir o requisito `fio_morto>=1` por zero nesses fatos, preservando a igualdade carimbo/censo (`app/chamados/tests/test_o261_vigia_fio.py:797`, `app/chamados/tests/test_o261_vigia_fio.py:812`, `app/chamados/tests/test_o261_vigia_fio.py:831`). |
| `test_MORDE_sem_dia_de_fio_morto_a_PORTA_nao_e_varrida` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: a pergunta C6 viva sem marco exigível não garante porta; nos fatos presentes esperar `fio_vivo=0`, `fio_morto=1`, e construir um caso separado com instância responder alcançável para conservar o propósito de custo (`app/chamados/tests/test_o261_vigia_fio.py:460`). |
| `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:656` | LU-03/07/M4/M7: nesta mistura somente Resolver constitui porta, portanto esperar `fio_vivo=1`, mantendo as igualdades de partição e os demais baldes (`app/chamados/tests/test_o261_vigia_fio.py:674`, `app/chamados/tests/test_o261_vigia_fio.py:684`, `app/chamados/tests/test_o261_vigia_fio.py:686`). |
| `test_MORDE_chamado_TERMINAL_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: terminal não cria porta residual, mas Resolver oferecido cria `admin/porta_acao/resolver_dia`; retirar a inclusão obrigatória dessa célula na amostra sem canal (`app/chamados/tests/test_o66_furo_sem_canal.py:55`, `app/chamados/tests/test_o66_furo_sem_canal.py:117`). |
| `test_a_celula_sem_canal_entra_no_contador` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: ausência de pergunta/chamado não impede Resolver; esperar a célula fora da amostra e contabilizada em `com_porta_acao` (`app/chamados/tests/test_o66_furo_sem_canal.py:41`, `app/chamados/tests/test_o66_furo_sem_canal.py:95`). |
| `test_esperado_e_zero_e_o_contador_nao_e_juiz` | C | `app/chamados/services/furo_sem_canal.py:104` | LU-03/M4: manter `esperado=0`, mas aceitar `total=0` nesta fixture cujos quatro dias ganharam porta; exigir total positivo requer contraprova sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:134`, `app/chamados/services/tarefa_do_dia.py:656`). |
| `test_o_contador_separa_furo_de_cobrado` | C | `app/chamados/services/furo_sem_canal.py:105` | LU-03/M4: esperar `por_veredito={}` nestes fatos com portas e reservar a separação furo/cobrado para uma entrada sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:128`). |
| `test_pergunta_aberta_crua_SEM_porta_da_tarefa_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: preservar que `respondida_em=None` cru não decide canal, mas esperar exclusão da amostra quando o juiz publica a porta independente de Resolver (`app/chamados/tests/test_o66_furo_sem_canal.py:98`). |
| `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:656` | LU-02 + LU-03/M4: manter que verbo `None` não dá dono residual, mas substituir o fio Folha esperado por `admin`, com `porta_acao/resolver_dia` oferecido independentemente desse membro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:292`). |
| `test_registrado_na_celula_nao_e_porta` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: conservar o C17 histórico sem porta e esperar Folha `admin/porta_acao/resolver_dia`, pois registrado não impede a oferta (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:200`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/folha/fora_da_folha.py:249` | LU-03/M4: substituir `nada/SEM PORTA/grupo D` por `admin`, ação `resolver_dia`, respectivo link e grupo A (`app/folha/tests/test_haiku_fora_da_folha.py:74`, `app/folha/fora_da_folha.py:99`). |
| `test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: o furo sem chamado já tem Resolver; esperar fios `{'admin':2}`, preservando os dois bloqueados e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:491`). |
| `test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/chamados/services/tarefa_do_dia.py:637` | LU-03/07/M4: atualizar a transição para `admin/Resolver → chamado/porta_fila → admin/porta_fila`, preservando paridade ferramenta/retrato e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:517`, `app/folha/tests/test_haiku_fora_da_folha.py:522`, `app/folha/tests/test_haiku_fora_da_folha.py:535`). |

P3: nos três casos `'nada' != 'admin'`, quem decide é a segunda passagem: tarefas sem rota são enviadas ao juiz plural (`app/chamados/services/tarefa_do_dia.py:588`); oferta verdadeira acrescenta ação `resolver_dia` e publica `admin/porta_acao` (`app/chamados/services/tarefa_do_dia.py:656`). O predicado da autoridade exige último recurso, passado e status em `falta/irregular/aberto/sem_certidao` (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`). A Folha apenas lê o fio publicado (`app/folha/fora_da_folha.py:85`). Pela lei M4, **admin tem razão nestes fatos**, inclusive nos dois casos cujo C17 histórico continua sem porta ou cujo residual tem verbo `None` (`app/chamados/tests/e3_lu2_entrega.txt:22`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:545`).

A principal ressalva interpretativa nos quatro controles positivos do vigia é explícita: a lei pede **instância com responder + pergunta pendente no app**, não apenas `pergunta_viva=True`; o cenário sem vínculo perde a primeira condição em `marco_fora_do_turno` (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:293`). Modernizar a fixture para fabricar um vínculo real preservaria o objetivo original, mas classificá-la F ampliaria a definição restrita de F dada pelo usuário. (`app/chamados/tests/test_o261_vigia_fio.py:87`)

## 14/11 20:41 · sessao 6a39842ebef1 · - · - · origem=Ronald (tmux)

Sete **C** e uma **R** neste recorte, pela definição de `fila` como portas do dono e pela preservação explícita de F6 na entrega (`app/chamados/services/tarefa_do_dia.py:41`; `app/chamados/tests/e3_lu2_entrega.txt:117`).

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `PortasDaTarefaTest.test_fila_A_pergunta_sob_B` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_app', (), {a.pk:'decidir', b.pk:'cobrar'}, 'colab')`; a pergunta pendente no app pertence ao colaborador (`app/chamados/tests/test_e3_portas_tarefa.py:53`; contraprova atualizada `:427`). |
| `PortasDaTarefaTest.test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_pergunta', (b.pk,), {b.pk:'validar'})`, preservando passivo/`nao_sei` (`app/chamados/tests/test_e3_portas_tarefa.py:190`; contraprova `:609`). |
| `PortasDaTarefaTest.test_porta_fila_quando_ancora_e_da_celula` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M7: exigir `('porta_app', (), {ch.pk:'cobrar'}, 'colab')` (`app/chamados/tests/test_e3_portas_tarefa.py:83`; lei `app/chamados/services/tarefa_do_dia.py:43`). |
| `PortasDaTarefaTest.test_v8_R10_fora_do_fato_conserva_ancora` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_fila', (b.pk,), {b.pk})`, preservando ações vazias (`app/chamados/tests/test_e3_portas_tarefa.py:309`; contraprova `:625`). |
| `PortasDaTarefaTest.test_v8_R4_encerrada_ancora_externa` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)` e incluir `acao=None, ausencia=None` na projeção esperada (`app/chamados/tests/test_e3_portas_tarefa.py:254`; contraprova `:550`; projeção `app/chamados/services/tarefa_do_dia.py:681`). |
| `PortasDaTarefaTest.test_v8_R8_escritor_real_sem_celula_dia_forte` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(ch.pk,)`, `acao=None, ausencia=None`, preservando porta/fio do chamado ligado pelo dia forte (`app/chamados/tests/test_e3_portas_tarefa.py:291`; contraprova `:598`; ligação `app/chamados/services/tarefa_do_dia.py:618`). |
| `PortasDaTarefaTest.test_v8_R9_ancora_com_nao_sei` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)`, `acao='validar', ausencia=None`, preservando `nao_sei=True` (`app/chamados/tests/test_e3_portas_tarefa.py:299`; contraprova `:609`). |
| `F3Beco1NaListaTest.test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | R | `app/escala/services/cadastro_realidade.py:988` | Preservar a consulta ao dono atual do dia mesmo quando LU-03 exclui C11: o exemplo deixa somente C9 lavrado `BATIDA` e o atalho de assinatura única conserva o dono antigo, contrariando `CADASTRO` exigido e prometido na entrega (`app/escala/tests/test_f3_beco1_folga_na_lista.py:287`; `app/escala/services/cadastro_realidade.py:469`; `app/chamados/tests/e3_lu2_entrega.txt:117`). |

P2: a comparação histórica exata com `de702cb2f` permanece **[não verificado]**: a cópia não contém `.git`, e o executor de base busca fontes por `git show`, sem embuti-las (`app/chamados/tests/e3_degrau_mutantes.py:178`). A evidência de antes disponível são as expectativas antigas dos testes e o mutante que repõe `da_celula` (`app/chamados/tests/e3_degrau_mutantes.py:117`).

| Rota | Antes documentado pelos testes | Depois | [nao verificado]
|---|---|---|
| `porta_app` | Chamados locais, exemplos `(a.pk,)` e `(ch.pk,)` (`app/chamados/tests/test_e3_portas_tarefa.py:53`; `:83`). | `()` porque o dono é colaborador (`app/chamados/services/tarefa_do_dia.py:669`). |
| `porta_pergunta` | `()` para âncora externa (`app/chamados/tests/test_e3_portas_tarefa.py:192`; `:299`). | Âncoras na fila pertencentes ao dono, exemplos `(b.pk,)` (`app/chamados/services/tarefa_do_dia.py:644`; `:669`). |
| `porta_fila` | Chamados da célula; âncora externa/dia forte podia publicar `()` (`app/chamados/tests/test_e3_portas_tarefa.py:254`; `:291`; `:309`). | Portas vivas do dono, incluindo externa/dia forte; exclusão de residual de outro dono (`app/chamados/services/tarefa_do_dia.py:646`; `:669`; M5 `app/chamados/tests/test_e3_degrau_dono_e_porta.py:551`). |
| `porta_acao` | Rota anterior na base: **[não verificado]**. | Ação administrativa sem chamado na fila; `fila=()` (`app/chamados/services/tarefa_do_dia.py:655`; `:669`). |
| `None` | Valor anterior exato: **[não verificado]**. | Sem porta publicada e `fila=()` nos casos sem dono (`app/chamados/services/tarefa_do_dia.py:648`; `:669`). |

Leitores efetivos de `fila`: `o_que_segura` converte a tupla em `chamado` para o dia (`app/folha/fora_da_folha.py:247`; `:254`); `diff_e3_sombra` compara a projeção e imprime sua quantidade (`app/chamados/management/commands/diff_e3_sombra.py:50`; `:143`). `porta_da_tarefa` transporta o campo (`app/chamados/services/tarefa_do_dia.py:681`).

P4: a mensagem exata nasce em `motivo_de` quando o módulo não pertence a `MOTIVOS` nem aos aposentados (`app/chamados/catalogo/modulos.py:208`; `:216`). O caminho de abertura consulta o catálogo, captura esse `KeyError` e registra `abrir_fora_da_lei`; o comentário declara que a emissão continua (`app/chamados/models.py:988`; `:994`; `:995`; `:997`). O módulo concreto e o traceback dessa ocorrência não aparecem no dado dos 55: **[não verificado]**. O relato do diff não lista esses dois arquivos entre as alterações, mas a confirmação histórica de que o código não mudou nesta obra é **[não verificado]** (`app/chamados/tests/e3_lu2_entrega.txt:129`).

## 14/11 20:41 · sessao 1b8bfc9ca4a2 · - · - · origem=Ronald (tmux)

Os **26 ERROR são F no ponto exibido**, pela propagação prevista em M8 (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:609`): a produção chama a plural com `agora=self.agora` em `app/chamados/services/tarefa_do_dia.py:589`, e os três dublês antigos rejeitam esse keyword (`app/chamados/tests/test_e3_bloqueantes.py:68`; `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`; `app/chamados/tests/test_e4_backfill.py:36`). Eventuais falhas após corrigir o dublê: [não verificado].

Formas exatas usadas nas linhas abaixo:

- **B:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_bloqueantes.py:68`.
- **L:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`.
- **E:** `_juiz(sim)` deve retornar `lambda _colab, datas, *, agora=None: dict.fromkeys(datas, sim)` em `app/chamados/tests/test_e4_backfill.py:36`; o patch herdado está em `app/chamados/tests/test_e4_backfill.py:74`.

| teste | classe | sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `BloqueantesE3Test.test_MORDE_8_chamado_TERMINAL_segue_sem_canal` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:195` | Atualizar o dublê para B. |
| `BloqueantesE3Test.test_MORDE_8_o_vigia_nao_le_o_predicado_cru` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:201` | Atualizar o dublê para B. |
| `ChamadoresDoC17LeemATarefaTest.test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:184` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_dias_sem_fio_nao_inclui_dia_so_fila` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_fora_da_folha_o_dia_vai_ao_grupo_C` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:135` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_o_que_segura_chamado_da_celula_sem_tarefa_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_dias_sem_fio_pergunta_na_fila_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:159` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_fora_da_folha_pergunta_na_fila_e_grupo_B` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:143` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_o_que_segura_pergunta_na_fila_e_colab` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:131` | Atualizar o dublê para L. |
| `BackfillE4bTest.test_backfill_2x_mesmo_estado_uma_trilha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:150` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_backfill_dry_usa_porta_e_nao_persiste` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:119` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_exportada_nao_muda_hash_fm_txt` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:169` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_sem_competencia_so_lista_e_nao_chama_porta` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:137` | Atualizar `_juiz` para E. |
| `PassivoE4bTest.test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:57` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:72` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classes_e_universos_nao_somem_para_zerar` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:98` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_mutante_sem_porta_morde_caracterizacao_classe_e` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:92` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_exportada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:446` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:346` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_DRY_diff_por_colaborador_le_dinheiro_da_autoridade` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:550` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_desconhecido_nomeado_e_fila_revertida` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:171` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_real_de_retomada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:149` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_escrita_EventoCelula_por_porta_aparece_no_diff` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:202` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:509` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:349` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_tabela_nao_fotografada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:189` | Atualizar `_juiz` herdado para E. |

P1:

- As assinaturas novas documentadas são `resolver_dia_oferecido_nas_datas(colaborador, datas, *, agora=None)` (`app/colaboradores/services/calendario.py:117`) e `contexto_calendario(colaborador, GET, *, datas_em_aberto=None, agora=None)` (`app/colaboradores/services/calendario.py:159`); o contrato M8 exige recebimento/reenvio e contexto sem nova leitura de relógio (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:575`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:613`).
- Cadeia principal correta: `CargaDaTarefa.tarefa` envia `self.agora` à plural (`app/chamados/services/tarefa_do_dia.py:589`); plural envia `agora` ao contexto (`app/colaboradores/services/calendario.py:132`); contexto usa o instante recebido, só chama `timezone.now()` quando omitido (`app/colaboradores/services/calendario.py:163`), usa o mesmo instante no desfazer (`app/colaboradores/services/calendario.py:250`) e no D4 (`app/colaboradores/services/calendario.py:606`).
- Os dois censos agora enviam o instante ao `ListaDoAdmin` (`app/chamados/services/vigia_fio.py:332`; `app/chamados/services/vigia_fio.py:573`); o construtor já aceita `agora` (`app/chamados/services/vigia_fio.py:177`) e `_carregar` repassa à lista (`app/chamados/services/vigia_fio.py:194`), distinção expressa na entrega (`app/chamados/tests/e3_lu2_entrega.txt:52`). O comando abre o relógio em `app/chamados/management/commands/vigiar_fio_morto.py:171`, envia à lista em `:175` e a ambos os censos em `:180`/`:181`.
- Os demais chamadores do contexto são view (`app/colaboradores/views.py:1949`), drawer (`app/colaboradores/services/drawer.py:121`) e relatório (`app/relatorios/furos.py:160`), todos sem `agora`; a entrega preserva defaults dos demais chamadores (`app/chamados/tests/e3_lu2_entrega.txt:51`).
- Há omissão concreta de um instante já disponível no relatório: `dataset_furos` recebe `agora` (`app/relatorios/furos.py:103`) e o envia ao D4 (`app/relatorios/furos.py:123`), mas não ao calendário do card (`app/relatorios/furos.py:160`/`:164`), cujo default lê outro relógio (`app/colaboradores/services/calendario.py:163`). Drawer também lê `agora` no início (`app/colaboradores/services/drawer.py:19`) e o omite no calendário (`app/colaboradores/services/drawer.py:121`), além de leituras próprias de data (`app/colaboradores/services/drawer.py:43`; `:111`). Serem regressões introduzidas neste commit: [não verificado sem base/diff].
- Não há dublê de produção encontrado entre as referências estáticas dos símbolos; os dublês que quebraram estão nos testes citados. Chamada/dublê dinâmico: [não verificado]. [nao verificado]

## 14/11 20:27 · sessao defe70908db2 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

A [entrega completa]( /home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:1) contém diff por função, matriz M1–M8, leitores dos campos, custo por carga, remoções L-111 e classificação dos 65 métodos do recorte.

Execuções locais:

- **159 testes OK**, sem banco. [Saída](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_suite.txt:167) [nao verificado]
- **45/45 mutantes mortos**, com controle verde. [Saída e comandos individuais](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_mutantes.txt:1478) [nao verificado]
- **RED anterior à cura preservado**; M7 foi controle verde. [Saída original](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_red.txt:1) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_lu2_suite
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base
```

O último comando reproduz os REDs na base e retorna **1 esperado**. [Registro](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:8)

Suite ORM completa, frota e timeout de 30 segundos: **[não verificado]**. Certificação do Code e opus final: **[não verificado]**. [nao verificado]

## 14/11 20:22 · sessao 9138e639ec97 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE-candidato — P2 — dinheiro: duas contas de trabalhado alteram o crédito.** `realizado_do_dia` desconta intervalo fixo não batido; o motor comercial desconta o intervalo efetivamente recebido e mede após a janela HE (`app/ponto/turnos.py:760`; `app/ponto/motor_calculo_v2.py:3236`; `app/ponto/motor_calculo_v2.py:3254`). Cenário sintético: previsto 480, escala 08–17, pausa 12–13, E08/S15 sem pausa, justificativa `outro`, sem recorte HE. A primeira conta dá **360**, portanto crédito **120**; o motor dá **420**, totalizando **540 com o crédito**, acima dos 480. É derivação paralela, transportada para `horas_trabalhadas` (`app/ponto/turnos.py:767`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:296`). Ocorrência produtiva: **[nao verificado]**.

2. **PROCEDENTE-candidato — P2 — estrutura: o dia também tem duas atribuições.** O realizado agrupa estritamente por `data_turno`; o motor lê esse dia e depois o sobrescreve quando o intervalo entre períodos é de até seis horas (`app/ponto/turnos.py:719`; `app/ponto/motor_calculo_v2.py:2408`; `app/ponto/motor_calculo_v2.py:2413`). Cenário sintético: DNA diurno 08–17, E19/S23 em 26/09 e E03/S07 em 27/09. O pareador mantém dois dias, com 240 minutos cada; a véspera já fechada não recebe o segundo turno (`app/ponto/turnos.py:1311`; `app/ponto/turnos.py:767`). No ramo-base do motor, o segundo período herda 26/09 pelo intervalo de quatro horas; essa atribuição alimenta a presença usada em falta/abono/DSR (`app/ponto/motor_calculo_v2.py:2413`; `app/ponto/motor_calculo_v2.py:2465`; `app/ponto/motor_calculo_v2.py:2583`). Impacto produtivo: **[nao verificado]**.

3. **PROCEDENTE-candidato — P2 — estrutura/texto: cobertura inteira passou a contar zero `dias_abono`.** A fonte inclui `cobre=None` no mapa; o motor continua interpretando presença nesse mapa como motivo para excluir o dia da contagem (`app/ponto/services/efeito_ausencia.py:47`; `app/ponto/motor_calculo_v2.py:2629`). Cenário: `outro`, sem batidas, previsto 480 → **8 horas de abono e zero dias** (`app/ponto/services/efeito_ausencia.py:122`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:2630`; `app/ponto/motor_calculo_v2.py:2633`). Espelho/PDF leem esse zero; o resumo pela lavratura conta um dia com minutos abonados positivos: conceitos diferentes para `dias_abono` (`app/ponto/services/espelho.py:1253`; `app/relatorios/pdf_espelho.py:479`; `app/ponto/services/dia_pago.py:701`; `app/relatorios/cartao_pela_celula.py:190`).

4. **PROCEDENTE-candidato — P3 — texto/estrutura: o motivo denuncia excedente que não existe no trabalho.** `pago_mais_trabalhado_passa_do_previsto` testa **bruto + trabalhado**, embora o pagamento final já tenha sido substituído pelo saldo (`app/ponto/services/efeito_ausencia.py:143`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/services/efeito_ausencia.py:146`). O próprio cenário col788 do teste tem previsto 420, trabalho 59 e crédito 361, mas recebe o motivo; a Central apresenta “Hora além do turno — revisar HE” (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:185`; `app/templates/ponto/fila_justificativas.html:116`). Nesse cenário, o excedente de trabalho sobre o previsto é **zero**, pela mesma fixture (`app/ponto/tests/test_justificativa_soma_sem_banco.py:180`).

5. **PROCEDENTE-candidato — P3 — estrutura: `revisao` é segunda lista, sem integração com a porta HE.** O filtro monta uma projeção por motivo; esse ramo não grava decisão HE (`app/ponto/views.py:1738`; `app/ponto/views.py:1747`; `app/ponto/views.py:1753`). O caminho existente é **pontas fora dos marcos → espelho → retrato → Gestão HE**, e já recebe os dias com pontas reconhecidas (`app/ponto/janela_he.py:204`; `app/ponto/services/espelho.py:727`; `app/ponto/services/he_pendente_lavrado.py:132`; `app/ponto/services/gestao_he.py:236`).

   Cenário de duplicação: janela ativa, previsto 480, pausa de uma hora, E07:50/S17:10 contra marcos 08–17, justificativa `outro`: as pontas entram na Gestão e o motivo também entra na Central. Uma decisão “Não” é lida pela Gestão, mas não participa do predicado da Central, que permanece baseado no bruto/trabalhado (`app/ponto/janela_he.py:205`; `app/ponto/janela_he.py:207`; `app/ponto/services/gestao_he.py:278`; `app/ponto/services/efeito_ausencia.py:146`).

   **Trabalhado > previsto não garante entrada na HE:** com janela desligada, o espelho mantém a lista vazia e o retrato ignora o dia; o novo aviso não fornece encaminhamento à ação existente (`app/ponto/services/espelho.py:721`; `app/ponto/services/espelho.py:722`; `app/ponto/services/he_pendente_lavrado.py:133`; `app/ponto/views.py:1753`). Falta essa ligação com a pendência/decisão existente, cuja porta exige minutos positivos (`app/ponto/portas/he.py:138`; `app/ponto/portas/he.py:169`).

O rastreamento dos campos de **P2** ficou assim:

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `minutos` | Saldo por `j.data_turno`: `app/ponto/services/efeito_ausencia.py:118`, `:145` | Feed `app/ponto/services/efeito_ausencia.py:47`; grade `app/escala/utils.py:1257`, `:1277` |
| `cobre` | Juiz, conservação e união: `app/ponto/services/efeito_ausencia.py:208`, `:129`, `:133` | Inclusão no feed `app/ponto/services/efeito_ausencia.py:46`; dispensa dos marcos `app/escala/utils.py:1251`, `:1267` |
| `motivo` | Literal do excedente: `app/ponto/services/efeito_ausencia.py:147`; motivo da âncora vira `conserva`: `:129` | Projeção `app/ponto/services/efeito_ausencia.py:153`; Central `app/ponto/views.py:1750`; apresentação `app/templates/ponto/fila_justificativas.html:116` |
| `min_justificados` | Mapa da fonte `app/ponto/services/efeito_ausencia.py:47`; ausência inteira ABONA remove a chave em `:63` | Leitor da célula `app/escala/services/leitor_celula.py:380`; fechamento `app/ponto/services/fechamento.py:667`; linha da grade → abono `app/ponto/services/dia_pago.py:399` |
| `minutos_justificados` do motor | Fechamento `app/ponto/services/fechamento.py:755`, `:831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; esmeril `app/ponto/services/esmeril_espelho.py:166`; comandos `app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62` | Ramo de abono `app/ponto/motor_calculo_v2.py:2628`, com conversão minutos→horas em `:2634` |
| `dias_abono` / `horas_abono` | Motor `app/ponto/motor_calculo_v2.py:2630`, `:2633`; alternativa gravada: `app/ponto/services/dia_pago.py:701`, `app/relatorios/cartao_pela_celula.py:186` | Espelho `app/ponto/services/espelho.py:1253`; PDF `app/relatorios/pdf_espelho.py:479`, `:480`; resumo pela lavratura `app/relatorios/cartao_pela_celula.py:190` |

A unidade numérica transportada é minuto, convertido explicitamente para hora; as divergências encontradas estão na **conta do trabalhado, atribuição ao dia e significado de dia abonado** (`app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:2634`; achados com fontes acima). Contagens e consequências em produção: **[nao verificado]**.

## 14/11 20:22 · sessao 885796c9d60e · JUSTIFICATIVA-PAGA · verificador · origem=Code

**P5 — PROCEDENTE-candidato · estrutura: revisão sem limite temporal e com consultas por dia.** A UI carrega todas as aprovadas, sujeitas apenas aos filtros opcionais de empresa/nome; por colaborador, calcula entre a menor e a maior `data_turno`, sem janela de competência ou paginação (`app/ponto/views.py:1707`, `:1714`, `:1720`, `:1732`, `:1747`). Cenário: duas aprovadas distantes fazem carregar células e parear batidas de todo o intervalo intermediário (`app/ponto/services/efeito_ausencia.py:109`, `:114`).

A chamada custa **7 consultas fixas + condicionais + consultas por dia coberto**; no ramo com carga de DNA e célula com geradora, **8 + 2D**, excluindo a consulta inicial e os cards da UI. As sete vêm das três cargas da fonte e quatro do carregador de turnos; o DNA acrescenta uma (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`; `app/ponto/turnos.py:1879`, `:1900`, `:1903`, `:1955`, `:1609`, `:1925`). O **2D** ocorre porque `realizado_do_dia` busca novamente vínculo/célula e intervalo por dia, apesar da carga anterior (`app/ponto/turnos.py:781`, `:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1182`). A configuração declara `gthread` e `timeout=30`; estouro efetivo em produção **[nao verificado]** (`app/gunicorn.conf.py:19`, `:21`).

**P6 — PROCEDENTE-candidato · estrutura: comparação sensível ao relógio interno.** `recalcular_fechamento_mes` não recebe `agora`, colhe `timezone.now()` e usa esse instante para filtrar furos (`app/ponto/services/fechamento.py:448`, `:616`, `:1120`). Cenário: dia sem par e sem aberta viva, fim previsto às 19h; a classificação muda entre 18:59 e 19:01, sem mudança nas batidas (`app/ponto/turnos.py:2132`, `:2140`). Atribuição aos nove colaboradores **[nao verificado]**.

Não vi relógio novo nos blocos do delta c5+c6: fonte, leitura do previsto, overlay e revisão (`app/ponto/services/efeito_ausencia.py:93`; `app/escala/services/leitor_celula.py:354`; `app/escala/utils.py:1257`; `app/ponto/views.py:1738`). Novidade dos demais relógios desde `14b61c124` **[nao verificado]**.

**P7 — uma linha por classe:**

- **Segundo juiz registrado — PROCEDENTE-candidato · estrutura:** o registro aponta `minutos_previstos_do_dia`, mas o novo helper prefere a ata; cenário sintético ata=440, marcos 12:50–21:20 sem pausa: helper=440, registrado=510. A justificativa passa a consumir esse caminho (`app/core/juizes.py:185`; `app/escala/services/leitor_celula.py:361`; `app/escala/utils.py:2288`, `:2292`; `app/ponto/services/efeito_ausencia.py:138`).
- **Segundo escritor — não vi** nos trechos lidos: fonte consulta e produz dicionários; overlay modifica grade em memória; persistência de `DiaPago` permanece em `lavrar` (`app/ponto/services/efeito_ausencia.py:98`; `app/escala/utils.py:1251`; `app/ponto/services/dia_pago.py:359`).
- **Relógio fora da borda — PROCEDENTE-candidato · estrutura:** instante interno sem parâmetro para repetir a comparação; cenário de passagem pelo fim do turno descrito em P6 (`app/ponto/services/fechamento.py:448`, `:616`; `app/ponto/turnos.py:2140`).
- **Derivação paralela — PROCEDENTE-candidato · estrutura:** permanece o medidor antigo que declara responder quanto a justificativa “paga”, sustentado por testes; previsto480, E16 sem saída, hf17: ele responde60, a fonte responde480 — classe residual vedada pela L-111 (`app/ponto/turnos.py:2769`, `:2779`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:49`; `app/docs/LEIS.md:137`).
- **Selo/RED sem a mordida anunciada — PROCEDENTE-candidato · estrutura:** `feed_motor_le_bruto` altera o mapa da fonte; sua testemunha executa o alimentador do PDF com fábrica mockada, sem executar o leitor do motor. Prova emissão do mapa bruto, não consumo pelo motor (`app/ponto/tests/mutantes_justificativa_soma.py:30`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:248`, `:254`).
- **Campo com dois sentidos — PROCEDENTE-candidato · texto:** `motivo` nasce de **bruto + trabalhado**, mas a tela o apresenta como trabalho “além do turno”; justificativa `outro`, previsto480 e trabalho180 dentro do turno gera motivo com pagamento300, sem demonstrar HE real (`app/ponto/services/efeito_ausencia.py:122`, `:143`, `:145`, `:146`; `app/templates/ponto/fila_justificativas.html:116`).

**P8 — PROCEDENTE-candidatos · texto:**

- **DSR anunciado além do trecho executado:** `test_bloco_real_motor_abona_361_sem_falta_ou_perda_dsr` verifica `dsr_ok` num ramo isolado; não executa nem confere os contadores semanais. Uma alteração na contabilização posterior fica fora do trecho selecionado (`app/ponto/tests/test_justificativa_soma_sem_banco.py:267`, `:276`, `:277`; `app/ponto/motor_calculo_v2.py:2641`, `:2667`).
- **Propriedade com nome falso para parte do próprio domínio:** “coberto implica soma dentro do previsto” exclui da checagem pagamentos zero. Com trabalho120 e previsto100, a soma continua120; o teste aceita pela fórmula vigente (`app/ponto/tests/test_justificativa_soma_sem_banco.py:163`, `:168`, `:172`, `:174`).
- **Docstrings contradizem as asserções atuais:** o cabeçalho ainda diz C5=690, sem âncora não justifica e D1=420; os testes atuais exigem720, cobertura inteira conservada e510. D3 diz “nada se cobre”, mas exige cobertura inteira e477 pagos (`app/ponto/tests/test_justificativa_paga_o_periodo.py:17`, `:18`, `:20`, `:458`, `:484`, `:565`, `:509`, `:516`).

## 14/11 20:22 · sessao c246f3ad8879 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE-candidato — P1(d), dinheiro: a guarda de sobreposição impede pagar o saldo devido.** Ela esvazia `cobre` antes da fórmula; depois, `periodos_justificados` só calcula o saldo quando existe cobertura. Assim, ainda é possível devolver **0 onde faltam 60 minutos**. Fontes: `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:218`, `app/ponto/services/efeito_ausencia.py:137`.

Os quatro casos abaixo são sintéticos, sem intervalo cadastrado; “crédito” significa minutos pagos pela justificativa. A referência é a fórmula literal registrada em `app/ponto/services/efeito_ausencia.py:83`.

| P1 | Cadastro, batidas e justificativa | REGRA em minutos | Código |
|---|---|---|---|
| **(a)** | Previsto **480**, 08h–16h; nenhuma batida; `esqueci_saida` aprovada, sem âncora nem saída informada. | Trabalho 0; crédito **480**. | `cobre=None`, crédito **480**, `conserva='sem_ancora'`. Caminho: `app/ponto/turnos.py:2694`, `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:145`. |
| **(b)** | Previsto **420**, 06h–13h; E03:54/S04:53; E06:00/S12:30/E12:45; `esqueci_saida` aprovada ancorada em E06, sem saída informada. | Trabalho **59+390=449**; crédito **0**; excedente **29** para revisão. | Crédito **0**, `cobre={'hf'}`, motivo de revisão. Soma dos turnos: `app/ponto/turnos.py:719`, `app/ponto/turnos.py:770`; saldo e motivo: `app/ponto/services/efeito_ausencia.py:145`. |
| **(c)** | Previsto **480**, 08h–16h; E08/S17; nenhuma justificativa; janela HE vigente, piso 0 e saída ativa, sem autorização. | Crédito **0**; excedente **60** para decisão do admin. | Justificativas devolvem `{}`; os **60** seguem pela pendência HE existente, independentemente de justificativa. Fontes: `app/ponto/services/efeito_ausencia.py:105`, `app/ponto/janela_he.py:206`, `app/ponto/janela_he.py:228`, `app/ponto/services/espelho.py:727`. |
| **(d)** | Previsto **480**, **22h–06h**; E18 aberta; E22/S05 do dia seguinte; `esqueci_saida` aprovada ancorada em E18, sem saída informada. | Trabalho **420**; crédito **60**, pertencente ao dia inicial. | O trecho 22h–06h cruza o trabalho 22h–05h: retorna **`cobre=frozenset(), minutos=0`** e revisão `{}`. **Divergência: −60 minutos.** Fontes: `app/escala/utils.py:346`, `app/ponto/turnos.py:2738`, `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:137`, `app/ponto/services/efeito_ausencia.py:153`. |

2. **PROCEDENTE-candidato — P4, estrutura: cobertura vazia apaga a condição de justificado e também desaparece da revisão.** Exemplo: previsto **480**, 08h–16h; E01:10 aberta e E10/S19 fechadas (**540 trabalhados**); justificativa aprovada ancorada em E01:10. A sobreposição produz `cobre` vazio; o dia sai de `justificadas`, perde o mapa explícito de zero e não recebe `motivo` nem `conserva`. A grade deixa eventuais marcos faltantes sem dispensa. Fontes: `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:46`, `app/ponto/services/efeito_ausencia.py:137`, `app/ponto/services/efeito_ausencia.py:153`, `app/escala/utils.py:1251`, `app/escala/utils.py:1258`.

A distinção entre zero, ausência de valor e mapa vazio fica assim:

| Leitor | Tratamento |
|---|---|
| `efeitos_da_ausencia` | **Zero não exclui o dia**; quem decide é `cobre`. `cobre=None` conserva cobertura inteira; `cobre` vazio exclui o dia dos dois mapas. `app/ponto/services/efeito_ausencia.py:46`. |
| `_marcar_ausencias` | Copia crédito **0** quando `cobre` é inteiro ou preenchido. Com cobertura vazia, nenhum ramo marca o dia como justificado. `app/escala/utils.py:1251`, `app/escala/utils.py:1257`, `app/escala/utils.py:1277`. |
| Motor | **`{dia: 0}` distingue-se de `{}`**: testa presença da chave e soma zero. `None` no mapa inteiro vira `{}`. Ausência da chave só aciona abono integral se o dia estiver em `_abono`. `app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`. |
| Grade da célula | Preserva zero pela presença da data no mapa. `app/escala/services/leitor_celula.py:448`. |
| `por_dia_da_grade` | O teste de verdade reúne crédito **0**, `None` e valor ausente: não cria entrada de abono; esse trecho não decide DSR. `app/ponto/services/dia_pago.py:398`. |

**Falta/DSR adicional no caso todo trabalhado: não demonstrado.** Mesmo removido de `datas_justificadas`, o dia reconhecido em `datas_trabalho` escapa da falta e mantém o DSR pelos predicados lidos; o ramo comercial também exige ausência dos dois conjuntos. Fontes: `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2610`, `app/ponto/motor_calculo_v2.py:3490`. Ocorrência produtiva de desconto ou perda de DSR por esse caminho: **[nao verificado]**.

## 14/11 20:21 · sessao 513b12f5fdae · - · - · origem=Ronald (tmux)

Cenário sintético **executado em memória** com `parear_turnos` e `realizado_dos_turnos` reais, extraídos por AST:

- DNA diurno 08–17; intervalo 12–13.
- 26/09: E19/S23.
- 27/09: E03/S07.
- Pareador devolveu dois turnos: `data_turno=26/09` e `data_turno=27/09`; realizado: **240 minutos em cada dia** (`app/ponto/turnos.py:719`, `app/ponto/turnos.py:767-771`).

O cenário evita absorção como pausa porque o intervalo entre S23 e E03 é de quatro horas, acima do teto de três horas (`app/ponto/turnos.py:382`, `app/ponto/turnos.py:1484-1491`). A véspera já tem turno fechado e não recebe o segundo turno (`app/ponto/turnos.py:1311-1314`).

O motor começa lendo o dia do juiz, mas **sobrescreve o dia do segundo período por 26/09**, pois o gap é de quatro horas, dentro de `0..6` (`app/ponto/motor_calculo_v2.py:2408-2413`). Portanto, há derivação paralela também da atribuição ao dia: realizado separa os dois dias; o motor os reúne no primeiro (`app/ponto/turnos.py:719`, `app/ponto/motor_calculo_v2.py:2411-2414`).

Execução completa do motor e ocorrência produtiva: **[nao verificado]**.

## 14/11 20:20 · sessao 090dd8640d32 · - · - · origem=Ronald (tmux)

**P2 — PROCEDENTE-candidato, severidade estrutura/texto: `dias_abono` passou a excluir também cobertura inteira sem trabalho.** A fonte agora coloca todos os dias cobertos no mapa, inclusive `cobre=None` (`app/ponto/services/efeito_ausencia.py:47`), mas o motor ainda usa **ausência no mapa** para contar dias abonados (`app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2630`). Cenário sintético: justificativa `outro` sem batidas, previsto 480 → fonte paga 480 (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:145`); motor produz **8 horas de abono e 0 dias**, embora o campo signifique “dias previstos sem trabalho mas justificados” (`app/ponto/motor_calculo_v2.py:284`, `app/ponto/motor_calculo_v2.py:2633`). Executei somente esse ramo real por AST em memória e obtive esse resultado; ocorrência produtiva [nao verificado].

O espelho transporta esse zero (`app/ponto/services/espelho.py:1253`); PDF também (`app/relatorios/pdf_espelho.py:479`), e oculta o badge quando zero (`app/relatorios/pdf_espelho.py:787`). Já o ramo de lavratura usa outra definição de `dias_abono`: conta linhas com `minutos_abonados>0` (`app/ponto/services/dia_pago.py:701`, `app/relatorios/cartao_pela_celula.py:190`). Assim, a mesma justificativa pode contar zero pelo motor e um pela lavratura; o ramo de escolha está em `app/relatorios/cartao_pela_celula.py:341`.

Mapa solicitado, para consolidar:

| Campo | Escritor/fornecedor | Leitores |
|---|---|---|
| `minutos` | Inicializado por dia de `j.data_turno`; valor final `max(0, previsto-trabalhado)` (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/efeito_ausencia.py:145`). | Feed `min_justificados` (`app/ponto/services/efeito_ausencia.py:47`); overlays da grade (`app/escala/utils.py:1257`, `app/escala/utils.py:1277`). |
| `cobre` | Resultado de `_cobertura_de_uma`, conservação e união (`app/ponto/services/efeito_ausencia.py:125`, `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:133`); juiz chamado em `app/ponto/services/efeito_ausencia.py:208`. | Inclusão em justificadas/mapa (`app/ponto/services/efeito_ausencia.py:46`); dispensa de marcos (`app/escala/utils.py:1251`, `app/escala/utils.py:1258`, `app/escala/utils.py:1267`). |
| `motivo` excedente | Comparação **bruto+trabalhado**, seguida do literal (`app/ponto/services/efeito_ausencia.py:146`, `app/ponto/services/efeito_ausencia.py:147`). | Projeção revisão (`app/ponto/services/efeito_ausencia.py:153`); Central (`app/ponto/views.py:1747`, `app/ponto/views.py:1750`); texto HE (`app/templates/ponto/fila_justificativas.html:116`). |
| `min_justificados` | Mapa dia→minutos (`app/ponto/services/efeito_ausencia.py:47`); ausência inteira ABONA remove a chave (`app/ponto/services/efeito_ausencia.py:63`). Linha da grade transporta o valor (`app/escala/services/leitor_celula.py:449`). | Fechamento (`app/ponto/services/fechamento.py:667`); espelho (`app/ponto/services/espelho.py:1134`); PDF (`app/relatorios/pdf_espelho.py:407`); esmeril (`app/ponto/services/esmeril_espelho.py:166`); comandos (`app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62`). |
| `minutos_justificados` motor | Nasce `{}` (`app/ponto/motor_calculo_v2.py:813`); alimentado pelos leitores acima e pelos dois motores do fechamento (`app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:831`). | Ramo de abono usa presença da chave e converte minutos para horas dividindo por 60 (`app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`). |
| Abono gravado | `por_dia_da_grade` soma `min_justificados` na data da linha (`app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:399`); fechamento soma mapa em `minutos_abonados` (`app/ponto/services/fechamento.py:1066`, `app/ponto/services/fechamento.py:1067`). | Resumo converte o gravado para horas (`app/relatorios/cartao_pela_celula.py:118`, `app/relatorios/cartao_pela_celula.py:186`). |

Os transportadores examinados mantêm **minutos por chave de data** até a conversão explícita `/60`; a comparação entre a atribuição do dia e o trabalho usado pelo motor ficou com você (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/dia_pago.py:391`, `app/ponto/motor_calculo_v2.py:2634`).

## 14/11 20:20 · sessao d24ad1288d7e · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura — segundo juiz residual:** `minutos_do_periodo_justificado` continua respondendo “minutos que […] paga” pela duração do trecho, enquanto a fonte paga saldo diário. Cenário: previsto480, E16 sem S, hf17: antigo60, fonte480. A função permanece sustentada por testes, contrariando L-111. (`app/ponto/turnos.py:2769`, `app/ponto/turnos.py:2779`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:49`, `app/docs/LEIS.md:137`).
- **PROCEDENTE-candidato — estrutura — previsto com duas autoridades:** registro aponta `minutos_previstos_do_dia`; novo helper prefere ata e só usa o registrado como fallback. Cenário sintético: ata440, marcos12:50–21:20 sem pausa: helper440, registrado510. A prioridade da ata já existia na grade; novidade demonstrada é a extração pública e seu consumo pela justificativa. (`app/core/juizes.py:185`, `app/escala/services/leitor_celula.py:354`, `app/escala/services/leitor_celula.py:361`, `app/escala/utils.py:2288`, `app/escala/utils.py:2292`, `app/ponto/services/efeito_ausencia.py:138`).
- **PROCEDENTE-candidato — texto — excedente hipotético apresentado como HE real:** `motivo` testa bruto+trabalhado, embora o pagamento já tenha virado saldo; a UI imprime “Hora além do turno”. Cenário sintético: justificativa `outro`, previsto480 e180 trabalhados inteiramente dentro do turno: bruto480 gera motivo, pago300, sem demonstrar qualquer trabalho além do turno. (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:143`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/services/efeito_ausencia.py:146`, `app/templates/ponto/fila_justificativas.html:116`).
- **Segundo escritor: não vi nos trechos lidos.** Fonte consulta e devolve estruturas; overlay altera grade em memória; fechamento recebe o mapa pela derivação existente e `lavrar` persiste `DiaPago`. Censo global de escritores [nao verificado]. (`app/ponto/services/efeito_ausencia.py:98`, `app/escala/utils.py:1251`, `app/escala/utils.py:1277`, `app/ponto/services/fechamento.py:1065`, `app/ponto/services/dia_pago.py:359`).

## 14/11 20:20 · sessao 850decff23c9 · - · - · origem=Ronald (tmux)

**P5 — PROCEDENTE-candidato, severidade estrutura:** `revisao` materializa **todas as aprovadas**, com filtros opcionais de empresa/nome, sem janela de competência nem paginação; para cada colaborador pergunta pelo intervalo entre a aprovação mais antiga e a mais recente. Cenário: duas aprovadas distantes fazem carregar e parear todo o histórico intermediário. (`app/ponto/views.py:1707`, `:1714`, `:1720`, `:1732`, `:1747`; `app/ponto/services/efeito_ausencia.py:98`, `:109`, `:114`.)

**Consultas por colaborador:** não há número universal; são **7 fixas + consultas condicionais + consultas por dia coberto**. No caminho usual com DNA de até um intervalo e célula com geradora em cada dia, a projeção faz **8 + 2D**, sendo D o número de dias cobertos; isso exclui a consulta inicial e os cards da UI. (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`; `app/ponto/turnos.py:1879`, `:1900`, `:1903`, `:1931`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1574`, `:1609`, `:1925`, `:2003`; `app/escala/utils.py:100`.)

O termo **2D** vem de chamar `realizado_do_dia` sem fornecer intervalo/modo: para cada dia ele busca novamente a célula com geradora e depois a pausa em outra consulta, embora `periodos_justificados` já tenha carregado células e vínculos. Cenário sintético: 100 colaboradores × 20 dias cobertos ⇒ **4.800 consultas na projeção usual**, além da UI. (`app/ponto/services/efeito_ausencia.py:109`, `:110`, `:136`, `:140`; `app/ponto/turnos.py:781`, `:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1163`, `:1182`.)

Há consultas adicionais condicionais: geradoras ausentes da candidatura, agregação da última batida e fallbacks em dias sem célula/previsto. (`app/escala/alimentacao.py:155`; `app/ponto/turnos.py:1914`; `app/escala/servico_jornada.py:64`.)

A configuração UI declara `gthread`, quatro threads e `timeout = 30`; duração real e estouro em produção **[nao verificado]**. (`app/gunicorn.conf.py:19`, `:20`, `:21`.)

## 14/11 20:20 · sessao 513b12f5fdae · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P3 — texto/estrutura: falso “hora além do turno”.** O motivo nasce de `bruto + trabalhado > previsto`, embora o crédito final seja `max(0, previsto - trabalhado)` (`app/ponto/services/efeito_ausencia.py:143-147`). No cenário col788 do teste, há previsto 420, par trabalhado de 59 minutos e crédito de 361; ainda assim recebe esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178-188`). A Central imprime “Hora além do turno — revisar HE” mesmo sem trabalho excedendo o previsto (`app/templates/ponto/fila_justificativas.html:116`).

2. **PROCEDENTE-candidato — P3 — estrutura: segunda lista independente da decisão HE.** A Central seleciona motivos da justificativa sem consultar decisão HE (`app/ponto/views.py:1747-1754`); a Gestão de HE consulta decisões vigentes para determinar o estado do dia (`app/ponto/services/gestao_he.py:86-89`, `app/ponto/services/gestao_he.py:278-280`). Portanto, decidir HE não retira o motivo da Central: o predicado continua dependendo de bruto e trabalhado (`app/ponto/services/efeito_ausencia.py:143-147`). É segunda projeção/lista; não há novo escritor persistente nesse ramo (`app/ponto/views.py:1738-1754`).

3. **PROCEDENTE-candidato — P3 — estrutura: aviso não equivale à porta HE.** O caminho existente recebe exclusivamente `he_fora_da_janela`; dia sem essa lista é ignorado pelo retrato (`app/ponto/services/he_pendente_lavrado.py:132-150`). O espelho popula essa lista por pontas fora dos marcos ou dia sem previsto, condicionado à janela ativa e à vigência (`app/ponto/services/espelho.py:721-749`); trabalho maior que previsto não é seu critério geral de entrada (`app/ponto/janela_he.py:194-230`). Cenário sintético: janela desligada e trabalho acima do previsto deixam `he_fora_da_janela=[]`, enquanto a justificativa pode receber motivo e aparecer na Central (`app/ponto/services/espelho.py:721-722`, `app/ponto/services/efeito_ausencia.py:143-147`). Falta ligação desse aviso à pendência/ação existente: o filtro apenas monta a lista (`app/ponto/views.py:1747-1754`), e a porta real exige minutos positivos de pendência (`app/ponto/portas/he.py:138-172`).

Quando existem pontas reconhecidas, o dia já chega pelo caminho anterior: marcação das pontas → espelho → retrato → Gestão HE (`app/ponto/janela_he.py:218-230`, `app/ponto/services/espelho.py:727-742`, `app/ponto/services/he_pendente_lavrado.py:132-150`, `app/ponto/services/gestao_he.py:236-250`). Nessa classe, a Central acrescenta outra lista (`app/ponto/views.py:1738-1754`). Alcance produtivo e contagens: **[nao verificado]**.

## 14/11 20:19 · sessao 6ab72d5695ec · - · - · origem=Ronald (tmux)

P6 — não vi relógio novo no delta c5+c6: os corpos acrescentados/alterados em `app/ponto/services/efeito_ausencia.py:93`, `app/escala/services/leitor_celula.py:354`, `app/escala/utils.py:1257` e `app/ponto/views.py:1738` não consultam `now()`/`today()`. Novidade dos demais relógios desde `14b61c124`: [nao verificado].

**PROCEDENTE-candidato — estrutura: DIFF sensível ao instante de execução.** `recalcular_fechamento_mes` não recebe `agora` (`app/ponto/services/fechamento.py:448`), consulta `timezone.now()` em cada execução (`app/ponto/services/fechamento.py:616`) e usa esse instante para filtrar os furos entregues ao coletor (`app/ponto/services/fechamento.py:1120`). O filtro pergunta ao juiz com esse instante (`app/ponto/services/fechamento.py:377`), cujo resultado muda quando `agora >= fim` (`app/ponto/turnos.py:2140`). Cenário: dia sem par, fim previsto 19h, sem aberta viva; às 18:59 não afirma furo, às 19:01 afirma, sem alteração das batidas (`app/ponto/turnos.py:2132`). Isso pode contaminar a comparação entre rodadas; atribuição aos nove colaboradores fornecidos: [nao verificado].

Relógios fora da borda encontrados na cópia, com novidade [nao verificado]: fallbacks em `app/ponto/turnos.py:2113`, `app/ponto/turnos.py:2159`, `app/ponto/turnos.py:2307`, `app/ponto/turnos.py:2341`, `app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2893`, `app/escala/utils.py:692` e `app/escala/utils.py:979`.

## 14/11 20:15 · sessao dfc90e576e7f · - · - · origem=Ronald (tmux)

**P4 — PROCEDENTE-candidato, dinheiro: há caminho concreto para falta inteira e DSR perdido com `minutos=0` e `cobre` vazio.** A reprodução em memória usou pareador real, trecho real de atribuição de dia do motor e ramo real de falta/DSR; execução integral com ORM permanece [nao verificado].

Cenário: template 22–06; DNA congelado D−1=22–06 e D=08–16, sem pausa, previsto de D=480. Batidas E22(D−1), S06(D), E08(D), S17(D), S18(D); `esqueci_entrada` aprovada na S18, `data_turno=D`. Ata somente com previsto, sem lâmpadas que substituam os papéis do pareador (`app/ponto/turnos.py:1217`; `app/ponto/motor_calculo_v2.py:488`).

O pareador real entrega `[22–06, D−1]`, `[08–17, D]` e `[S18 órfã, D]`; a órfã não produz período do motor (`app/ponto/turnos.py:1507`; `app/ponto/turnos.py:1512`; `app/ponto/adaptador_motor.py:30`).

O realizado de D é540; o prefixo justificado 08–16 cruza 08–17, portanto a fonte devolve `{cobre: frozenset(), minutos:0}` e exclui D de `justificadas` (`app/ponto/turnos.py:719`; `app/ponto/turnos.py:2739`; `app/ponto/turnos.py:2763`; `app/ponto/services/efeito_ausencia.py:46`).

O motor reatribui 08–17 a D−1 porque começa duas horas depois do fim anterior; a órfã S18 não deixa outro período em D (`app/ponto/motor_calculo_v2.py:2408`; `app/ponto/motor_calculo_v2.py:2412`; `app/ponto/adaptador_motor.py:36`). Assim, `datas_trabalho={D−1}`, D não está justificado, e o ramo real produziu `datas_falta=[D]`, `horas_falta=8` e `dsr_ok=False`; esse resultado incrementa semanas de DSR perdido (`app/ponto/motor_calculo_v2.py:2582`; `app/ponto/motor_calculo_v2.py:2605`; `app/ponto/motor_calculo_v2.py:2623`; `app/ponto/motor_calculo_v2.py:2666`).

A premissa do template importa: usando template08–16, o modo duração pode absorver S06/E08 como pausa antes da aplicação das bordas por dia; a reprodução confirmada usou template22–06, no qual S06 coincide com a saída prevista (`app/ponto/turnos.py:436`; `app/ponto/turnos.py:453`).

## 14/11 20:13 · sessao dfc90e576e7f · - · - · origem=Ronald (tmux)

Achados PROCEDENTE-candidato:

1. **Dinheiro/estrutura — a guarda de sobreposição ainda vence a lei do saldo.** Cadastro 07–19, previsto 720; batidas E01:10, E07:00, S11:00; `esqueci_saida` aprovada na E01:10. O pareador real forma `[01:10 aberta]` e `[07–11 fechado]` (`app/ponto/turnos.py:1433`, `:1507`). A regra pede **480 min**; a fonte devolve **`cobre=frozenset(), minutos=0`** porque a sobreposição remove `hf` (`app/ponto/turnos.py:2763`) e a fórmula só roda para cobertura None ou não vazia (`app/ponto/services/efeito_ausencia.py:137`). O dia sai também de `justificadas`, do mapa de minutos e da revisão (`app/ponto/services/efeito_ausencia.py:46`, `:47`, `:153`). Reprodução executada em memória com pareador e fonte reais; ORM/prod [nao verificado].

2. **Dinheiro/estrutura — realizado da justificativa e trabalhado do motor são contas distintas.** Cadastro 08–17, pausa fixa 12–13, previsto 480; E08, S16, E16:30, justificativa aprovada na última entrada. Pareador real: `[08–16 fechado]`, `[16:30 aberto]` (`app/ponto/turnos.py:1433`, `:1507`). `realizado_do_dia` desconta a pausa não batida: **420** (`app/ponto/turnos.py:760`); fonte paga **60** (`app/ponto/services/efeito_ausencia.py:145`). O corpo real de `Motor12x36.calcular_periodo`, com pontas dentro da janela e nenhuma pausa batida, produz **480 trabalhados**, porque desconta somente `_intra_real`, que vale zero (`app/ponto/motor_calculo_v2.py:2773`, `:2783`, `:2786`, `:2797`). Motor + justificativa = **540**, previsto 480; `horas_trabalhadas` soma esse trabalhado (`app/ponto/motor_calculo_v2.py:295`). A fonte nem marca revisão: bruto 30 + realizado 420 < 480 (`app/ponto/services/efeito_ausencia.py:146`). Executados pareador/fonte e corpo real do motor até a atribuição de trabalhado; execução integral ORM [nao verificado].

P1 — resultados concretos:

| Caso | Cadastro, batidas, justificativa | Regra / código |
|---|---|---|
| (a) | 08–16, previsto480, nenhuma batida; esqueci_saida aprovada sem âncora | **480 / 480**, `cobre=None`, `conserva=sem_ancora` (`app/ponto/turnos.py:2694`; `app/ponto/services/efeito_ausencia.py:129`, `:141`, `:145`). Divergência monetária nesse cenário isolado: não vi; `dias_abono` perde a contagem porque agora há chave no mapa (`app/ponto/motor_calculo_v2.py:2629`). |
| (b) | 06–13, previsto420; E03:54/S04:53, E06/S12:30, E12:45; esqueci_saida na última E | Trabalhado **449**, regra/código **0**, excedente29; fonte `hf`, minutos0, motivo de revisão (`app/ponto/turnos.py:719`, `:734`, `:770`; `app/ponto/services/efeito_ausencia.py:140`). Pareador real confirmou três turnos, todos do mesmo D (`app/ponto/turnos.py:1433`, `:1507`). |
| (c) | 08–16, previsto480, E08/S17, nenhuma justificativa | Crédito **0**; fonte devolve `{}` e não gera revisão, pois retorna antes de ler trabalho (`app/ponto/services/efeito_ausencia.py:98`, `:105`). Caminho HE examinado pelo outro explorer. |
| (d) | 22–06, previsto480; E20 solta, E22/S02(+1); esqueci_saida na E20 | Regra **240**; código **0**, `cobre` vazio, sem revisão: mesma guarda de sobreposição do achado1 (`app/ponto/turnos.py:2737`, `:2763`; `app/ponto/services/efeito_ausencia.py:137`). Pareador real confirmou aberta20 e fechada22–02, ambas D (`app/ponto/turnos.py:1433`, `:1507`). |

Noturno simples E22/S02(+1), E03(+1), justificativa na E03 com `data_turno=D`: fonte paga240; não vi divergência desse cenário isolado (`app/ponto/turnos.py:719`; `app/ponto/services/efeito_ausencia.py:196`, `:211`, `:145`).

P2 — dia/unidade: fonte usa `j.data_turno`, realizado filtra `t.data_turno==data` (`app/ponto/services/efeito_ausencia.py:118`; `app/ponto/turnos.py:719`). O motor lê o mapa do juiz, mas pode herdar o dia do período anterior por gap ≤6h (`app/ponto/motor_calculo_v2.py:2408`, `:2410`). Base usa dia da jornada no DSR; Comercial refaz o DSR por data civil (`app/ponto/motor_calculo_v2.py:2582`, `:3477`). Portanto identidade de dia em todos os leitores não está assegurada por esses caminhos; alcance produtivo [nao verificado].

P4 — distinções:

- `cobre=None` conserva cobertura; vazio exclui cobertura; rótulos conservam cobertura mesmo quando minutos0 (`app/ponto/services/efeito_ausencia.py:46`; `app/escala/utils.py:1251`, `:1258`).
- Grade conserva `min_justificados=0` nos ramos None/rótulos; com cobre vazio não entra em nenhum desses ramos (`app/escala/utils.py:1257`, `:1277`).
- Leitor da célula distingue presença da chave e transporta zero (`app/escala/services/leitor_celula.py:448`).
- Motor distingue `{D:0}` de `{}` por pertinência; `None` e `{}` são colapsados por `or {}` (`app/ponto/motor_calculo_v2.py:2628`, `:2629`, `:2634`).
- `dia_pago` usa truthiness e colapsa zero/None/ausente na formação do mapa de abono (`app/ponto/services/dia_pago.py:398`).
- **Não provei falta/DSR adicional causada apenas por cobre vazio em dia trabalhado acima do previsto**: Base inclui a data de todo período normal em `datas_trabalho`, independentemente dos minutos, e exige ausência dessa data e da justificativa para registrar falta (`app/ponto/motor_calculo_v2.py:2582`, `:2605`). A grade pode ficar sem `justificado` e manter furo, mas transformar esse furo em falta/DSR produtivo é [nao verificado].

## 14/11 20:09 · sessao b46ca7bab667 · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura/texto, P3:** “Revisar aprovadas” é segunda lista, sem ligação com a decisão de HE: calcula motivos em `app/ponto/views.py:1747`, acrescenta linhas em `:1753` e abre o painel da justificativa em `app/templates/ponto/fila_justificativas.html:154`; a porta de decisão efetiva permanece `app/ponto/portas/he.py:138`, com gravação em `:181` e recalculo em `:202`. Não vi segundo escritor de `DecisaoHE` nesses trechos.

  Cenário: escala 08–17, pausa12–13, previsto480, batidas E08/S18, trabalho540 e `esqueci_saida` aprovada na E08. A fonte conserva cobertura por turno fechado e devolve crédito0/motivo (`app/ponto/services/efeito_ausencia.py:128`, `:145`); o HE existente recebe a ponta60 (`app/ponto/services/espelho.py:722`, `:727`, `:776`; `app/ponto/services/he_pendente_lavrado.py:132`, `:148`). O mesmo dia entra também na revisão (`app/ponto/views.py:1747`). Sem justificativa, a revisão nem avalia o dia (`app/ponto/services/efeito_ausencia.py:98`, `:105`), embora o caminho existente de HE continue lendo a ponta (`app/ponto/services/he_pendente_lavrado.py:132`).

- **PROCEDENTE-candidato — texto/estrutura, P3:** o motivo/UI não significa necessariamente trabalho excedente: compara **bruto da justificativa + trabalho**, antes do limite, com previsto (`app/ponto/services/efeito_ausencia.py:143`, `:146`), mas a UI declara “Hora além do turno” (`app/templates/ponto/fila_justificativas.html:116`). O próprio caso420/59/pago361 exige esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178`, `:185`, `:188`): trabalho59 não excede previsto420.

- **PROCEDENTE-candidato — estrutura/desempenho, P5:** o request processa todas as aprovadas, sem competência/janela/paginação (`app/ponto/views.py:1707`, `:1714`, `:1732`), e cada colaborador recebe intervalo do menor ao maior dia histórico (`:1745`, `:1747`). A contagem estática típica é **8 + 2D consultas por colaborador**, com `D` dias cobertos, além de opcionais por cadastro:
  - 3 próprias: justificativas, células e vínculos (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`);
  - 4 no carregamento de turnos: batidas, células, candidatos e vínculo usado pela ata (`app/ponto/turnos.py:1879`, `:1902`, `:1903`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1609`, `:1574`);
  - normalmente1 DNA (`app/ponto/turnos.py:1928`, `:2005`; `app/escala/utils.py:100`);
  - 2 por dia com célula/geradora: `realizado_do_dia` recarrega vínculo/célula e pausa, pois o chamador fornece somente turnos/previsto (`app/ponto/services/efeito_ausencia.py:140`; `app/ponto/turnos.py:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1187`).
  
  Há extras condicionais para geradora fora dos candidatos, agregação de última batida e dia sem geradora (`app/escala/alimentacao.py:157`; `app/ponto/turnos.py:1917`; `app/escala/servico_jornada.py:64`). Tempo real e timeout observado: **[nao verificado]**. A configuração é gthread/30s (`app/gunicorn.conf.py:19`, `:21`); o código registra que esse timeout não é teto rígido para request longo com gthread (`app/config/crons.py:1393`).

- **P6, não vi relógio novo no delta fornecido:** funções novas de cálculo/projeção/leitura não chamam `now()`/`today()` (`app/ponto/services/efeito_ausencia.py:98`, `:136`, `:151`; `app/escala/services/leitor_celula.py:354`). `tz.localdate()` da Central aparece como contexto preexistente no delta e alimenta SLA/cards (`app/ponto/views.py:1727`, `:1733`, `:1759`). O fechamento captura um instante vivo por rodada (`app/ponto/services/fechamento.py:616`), transmite-o ao filtro de furos (`:1120`) e consulta encerramento (`:377`; `app/ponto/turnos.py:2108`). Atribuir os nove furos de14/11 a isso, ou verificar novidade desde14b61c124: **[nao verificado]**.

## 14/11 20:08 · sessao 2dfc1fb51f5e · - · - · origem=Ronald (tmux)

P7/P8 — achados para consolidação:

1. **PROCEDENTE-candidato — texto: contrato antigo contradiz os testes atuais.** A docstring declara pagamento pelo período, C5=690, C7 sem justificativa e D1=420; as asserções atuais esperam respectivamente 720, cobertura inteira com 720 e 510. Referências: `app/ponto/tests/test_justificativa_paga_o_periodo.py:1`, `:17`, `:18`, `:20`, `:462`, `:492`, `:564`. D2/D3 dizem “nada se cobre”, porém esperam `cobre=None`, que representa cobertura inteira: `:496`, `:502`, `:509`, `:516`; semântica declarada em `app/ponto/services/efeito_ausencia.py:78`.

2. **PROCEDENTE-candidato — estrutura/texto: teste “sem perda DSR” não executa o cálculo semanal completo.** Ele extrai apenas o `If` que contém `_mj`, executa esse bloco e confere `dsr_ok`; não verifica `semanas_dsr_perdido`. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:262`, `:267`, `:273`, `:276`, `:277`. O contador real está depois do bloco e existe outro cálculo em `MotorComercial`: `app/ponto/motor_calculo_v2.py:2641`, `:2667`, `:3474`, `:3490`, `:3515`. Prova executada somente em memória: mudar `:3474` para iniciar `semanas_dsr_perdido=1` mantém os 24 testes `SomaDoDiaTest` verdes; resultado de execução sem arquivo persistido [nao verificado].

3. **PROCEDENTE-candidato — texto: “bordas ORM substituídas” omite substituição de autoridade de domínio.** A docstring diz isso em `app/ponto/tests/test_justificativa_soma_sem_banco.py:5`, mas `turnos_do_colab` inteiro é substituído por `self.ts` em `:43`, e os turnos são fabricados usando o tipo gravado para decidir entrada/saída em `:53`–`:58`. Assim, esses testes não exercitam o pareamento real, a atribuição do dia pelo envelope nem a aplicação real da L-087. Referências: `:43`, `:57`, `:58`.

4. **PROCEDENTE-candidato — texto: nome da propriedade promete uma desigualdade que o próprio teste permite violar.** `test_propriedade_coberto_implica_soma_dentro_do_previsto` só registra violação quando o crédito é positivo. Com previsto=100, trabalho=120 e crédito=0, a cobertura permanece e a soma=120 passa pelo teste — comportamento compatível com a lei atual, mas incompatível com o nome. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:163`, `:165`, `:168`, `:172`, `:174`.

**Mutantes declarados:** não vi mutante declarado que faça algo incompatível com seu nome. Li as 12 substituições em `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`, o mecanismo de carregamento em `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:27`–`:43` e o critério de morte em `app/ponto/tests/mutantes_justificativa_soma.py:94`. Executei `--todos`: controle com 24 testes; 12/12 mortos por asserção; retorno 0. Resultado de execução sem arquivo persistido [nao verificado]. Nenhum desses 12 muta o pareador, o motor semanal ou a porta de HE: `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`.

Não executei a suíte ORM; seus resultados permanecem [nao verificado]. [nao verificado]

## 14/11 20:06 · sessao 93faa118c4dc · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhuma edição nem execução com banco. O recorte contém **65 métodos únicos**, correspondendo às **70 ocorrências**: cinco subtests A/S acrescentam quatro ocorrências; dois subtests R2 acrescentam uma. Isso corresponde aos 69 failures + 1 error fornecidos. As linhas abaixo são da base inspecionada.

Classificação proposta: **60 métodos (a)**, **1 método (b), com cinco subtests**, e **4 métodos (c)**. A exclusão dos quatro métodos (c) depende da remoção efetiva do balde e do censo exigido por L-111.

| Arquivo | Método | Classe | Adaptação proposta |
|---|---|---|---|
| `chamados/tests/test_o261_vigia_fio.py:624` | `test_MORDE_a_lista_e_perguntada_no_UNIVERSO_DA_TELA` | a | Usar dia efetivamente sem porta; a fixture atual publica residual `decidir`, evitando a lista. |
| mesmo arquivo`:418` | `test_a_abertura_por_PERGUNTA_soma_o_universo_da_PORTA` | a | Adequar contagem aos dias que continuam sem porta após o juiz. |
| mesmo arquivo`:455` | `test_a_LISTA_com_dono_leva_a_CLASSE_de_cada_dia_sem_precedencia` | a | Preservar classificação da lista apenas para dias sem porta publicada. | [nao verificado]
| mesmo arquivo`:727` | `test_as_linhas_do_log_NAO_LEVAM_NOME_de_ninguem` | a | Preservar ausência de PII; ajustar `FIO_MORTO=1` à fixture sem porta. |
| mesmo arquivo`:680` | `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | a | Tirar parcela do balde extinto; Resolver oferecido integra `fio_vivo`. |
| mesmo arquivo`:368` | `test_as_TRES_classes_de_DIA_somam_o_fio_morto` | a | Classes contam apenas dias sem porta, não residual `decidir`. |
| mesmo arquivo`:280` | `test_dia_SEM_pergunta_nenhuma_e_a_classe_mais_crua` | a | Sem pergunta com `decidir` tem porta; inverter consequência ou construir ausência real de porta. |
| mesmo arquivo`:557` | `test_MORDE_aguardando_DOCUMENTO_tambem_e_bola_do_DP` | a | Manter contador LU-04/R3, com fixture realmente sem porta. |
| mesmo arquivo`:224` | `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | a | Preservar asserts C6/property; consequência no vigia agora considera residual com porta. |
| mesmo arquivo`:540` | `test_MORDE_ausencia_ABERTA_cobrindo_o_dia_AGUARDA_O_DP_e_nao_e_fio_morto` | a | Ausência decidível tem ação/porta; balde permanece somente para ausência sem porta. |
| mesmo arquivo`:513` | `test_MORDE_chamado_em_analise_SEM_resolver_dia_e_FIO_MORTO` | a | Manter teste vivo; `decidir` residual alcançável implica fio vivo; retirar só assert do balde apagado. | [nao verificado]
| mesmo arquivo`:586` | `test_MORDE_dono_BATIDA_ou_CADASTRO_NA_LISTA_e_fio_do_admin_e_o_resto_fica` | a | Manter subdivisão do sem-porta e testar com dias sem porta. |
| mesmo arquivo`:614` | `test_MORDE_falha_da_lista_e_UMA_chamada_por_rodada_e_todos_ficam` | a | Preservar custo/falha da lista com fixture que efetivamente chega à lista. |
| mesmo arquivo`:659` | `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | a | Preservar subdivisão; porta residual prevalece antes do contador. |
| mesmo arquivo`:789` | `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | a | Preservar comparação com censo; mudar mistura e contadores antigos, passar instante do carimbo também ao censo. |
| mesmo arquivo`:163` | `test_MORDE_o_dia_de_HOJE_fica_FORA_do_contador_alarmado` | a | Testar teto somente em dia sem porta, pois porta válida vence essa classificação. |
| mesmo arquivo`:485` | `test_MORDE_o_dia_que_OFERECE_resolver_dia_e_DECISAO_PENDENTE_e_nao_fio_morto` | c | Exclusivo do balde; remover com L-111; cobertura nova M4 prova porta administrativa. |
| mesmo arquivo`:389` | `test_MORDE_o_dia_SO_de_plantio_fora_do_slot_NAO_e_ALVO` | a | ALVO continua vivo; montar universo sem porta ou adequar contagem. |
| mesmo arquivo`:128` | `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | a | Pergunta morta não remove porta residual `decidir`; escolher par que difira na porta publicada. | [nao verificado]
| mesmo arquivo`:521` | `test_MORDE_o_passivo_ALVO_nao_some_dentro_da_decisao_pendente` | c | Exclusivo do balde e de seu contador ALVO; remover com balde. |
| mesmo arquivo`:197` | `test_MORDE_o_VOCABULARIO_do_cartorio_e_quem_diz_quais_dias_cobram` | a | Preservar vocabulário e recorte; adaptar fixture à porta válida. |
| mesmo arquivo`:145` | `test_MORDE_registrado_e_VIVO_mas_nao_COBRA_ninguem` | a | `registrado`/`historico` não dão porta LU-02; caso aberto com `decidir` dá porta. |
| mesmo arquivo`:404` | `test_o_dia_MISTO_conta_UMA_vez_e_no_ALVO` | a | Preservar unidade colab-dia/ALVO; contar apenas sem porta. |
| mesmo arquivo`:564` | `test_o_dia_que_OFERECE_resolver_dia_fica_na_DECISAO_DO_DIA_mesmo_com_ausencia_aberta` | c | Exclusivo da precedência do balde extinto. |
| mesmo arquivo`:495` | `test_o_juiz_REAL_oferece_o_dia_que_acusa_sem_lavra` | c | Assert exclusivo do balde; a integração real útil pode migrar para M4/M8. |
| mesmo arquivo`:606` | `test_sem_resposta_da_lista_o_dia_FICA_no_alarme` | a | Preservar falha explícita, com entrada sem porta. |
| `chamados/tests/test_e3_tarefa_admin_sem_banco.py:133` | `test_B_252_29_set_hipotese_ultima_resposta_textual` | a | Âncora existente fora da fila dá `admin/porta_acao/validar`. |
| mesmo arquivo`:136` | `test_B_841_10_out_hipotese_ultima_resposta_textual` | a | Mesma inversão LU-03. |
| `chamados/tests/test_e3_c8b_sem_banco.py:196` | `test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` | a | Carga passa âncoras existentes: esperar `admin/porta_acao`, preservar custo e idempotência. |
| mesmo arquivo`:48` | `test_cinco_formas_acusadoras_sem_pergunta_viva` — cinco subtests | b | A/S sem oferta de Resolver não dá dono; manter `nada`, curar juiz e acrescentar controle oferecido. |
| mesmo arquivo`:166` | `test_grave_respondida_em_disputa_fechada_nao_e_filtrada_por_motivo` | a | Passar mapa de âncoras existente e esperar administração alcançável. |
| mesmo arquivo`:184` | `test_sem_trabalho_sem_lampadas_nao_esconde_resposta_viva` | a | Mesma inversão, preservando exposição da pergunta/marco. |
| `chamados/tests/test_e4_passivo.py:57` | `test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | a | Após registrar resposta E1, passa a `admin/porta_acao`; segunda rodada não seleciona mais sem-porta; preservar uma linha E1. |
| `chamados/tests/test_o261e_f5_alcance.py:484` | `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | a | Resposta ancorada em registrado dá `porta_acao/admin`, sem causa sem-porta. |
| mesmo arquivo`:213` | `test_c_respondida_com_destino_FECHADO` | a | Âncora terminal existente também dá modal: `porta_acao/admin`. |
| `chamados/tests/test_e3_portas_tarefa.py:465` | `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | a | Com app: `porta_app/colab`; sem app: preservar admin. |
| mesmo arquivo`:483` | `test_historico_tem_porta_mas_nao_e_nomeado_na_lista_padrao` | a | LU-02: projeção `None`, sem dono; manter verbo como evidência. |
| mesmo arquivo`:419` | `test_RED_A_na_fila_aparece_com_acao_sob_B` | a | Com app: `porta_app`; sem app: `porta_fila/chamado`; manter fila contextual. |
| mesmo arquivo`:115` | `test_tarefa_propaga_relogio_a_alimentacao` | a | A pergunta pendente no app continua dando `porta_app`; manter verificação do verbo/instante. |
| mesmo arquivo`:505` | `test_v8_R1_colab_alcancavel_e_leitores` | a | Trocar `porta_pergunta` por `porta_app`; todos os leitores seguem publicação. |
| mesmo arquivo`:231` | `test_v8_R2_alcance_admin_com_pergunta_colab` | a | `porta_app/colab`, preservando verbo administrativo contextual. |
| mesmo arquivo`:525` | `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — dois subtests | a | Ambos `porta_app/colab`, sem alterar ações originais. |
| mesmo arquivo`:265` | `test_v8_R7_membro_da_fila_sem_verbo` | a | LU-02: None não dá dono/porta. |
| mesmo arquivo`:572` | `test_v8_R7_membro_sem_verbo_conservado` | a | Alimentação conserva membro/None; projeção da tarefa é `None`. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:259` | `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | a | C17 histórico pode continuar sem porta; tarefa/Folha dão `admin/porta_acao/validar`. |
| mesmo arquivo`:276` | `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | a | C17 histórico mantém assert próprio; tarefa/Folha passam a sem porta por LU-02. |
| mesmo arquivo`:135` | `test_MORDE_ricardo_a_folha_le_o_juiz_e_diz_nada` | a | Folha deve ler `admin`, pois há âncora existente para validar. |
| mesmo arquivo`:140` | `test_MORDE_ricardo_folha_e_vigia_dizem_a_MESMA_coisa` | a | Ambos leem `porta_acao/admin`, sem causa c. |
| `escala/tests/test_f3_beco1_folga_na_lista.py:137` | `test_MORDE_competencia_anterior_NAO_exportada_vai` | a | Resposta ancorada tem modal; C11 vazio. |
| mesmo arquivo`:351` | `test_MORDE_F5_queries_nao_crescem_com_os_dias` | a | Preservar custo; ajustar cardinalidade dos resultados à exclusão por porta. |
| mesmo arquivo`:199` | `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | a | Porta residual `decidir/chamado` permanece; atualizar projeção com `acao`, se publicada. | [nao verificado]
| mesmo arquivo`:219` | `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | a | Agora `admin/porta_acao`, C11 vazio. |
| mesmo arquivo`:100` | `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | a | Fixture respondida/ancorada já tem modal; esperar porta e ausência no C11. |
| mesmo arquivo`:244` | `test_RED_F1_lista_le_o_instante_de_quem_chama` | a | Preservar instante explícito e sua propagação, sem exigir o C11 antigo. |
| mesmo arquivo`:292` | `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | a | Preservar dono C9, retirar expectativa de C11 para dia que tem porta administrativa. |
| `chamados/tests/test_f3_beco1_reata_limbo.py:67` | `test_MORDE_dry_nao_escreve` | a | Dia já tem porta: zero candidatos, nenhuma escrita. |
| mesmo arquivo`:149` | `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | a | Âncora atual já fornece modal; não há candidato a reata nesse cenário. |
| mesmo arquivo`:76` | `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | a | Não cria pai; dia com modal também não entra C11. |
| mesmo arquivo`:49` | `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | a | Já sai pelo modal: zero reatas, destino preservado. |
| mesmo arquivo`:124` | `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | a | Nenhuma seleção nesse cenário; preservar destino sem reata. |
| mesmo arquivo`:105` | `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | a | Sem seleção/reata; excluir expectativa de C11 para dia com modal. |
| `chamados/tests/test_e3_bloqueantes.py:179` | `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | a | Assert agora exige `admin/porta_acao`; furo não inclui dia. |
| mesmo arquivo`:170` | `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | a | Mesma inversão LU-03. |
| `chamados/tests/test_n1_perguntas_do_app.py:383` | `test_RED_censo_dono_fora_do_fio_le_uma_instancia_do_colab` | a | Fixture tem três `decidir`: não chega ao balde; verificar zero chamadas ou construir entrada sem porta para continuar testando identidade. |
| `chamados/tests/test_sombra_c1_resolver_em_lote.py:35` | `test_RED_censo_monta_um_calendario_por_colaborador` | a | Três dias com residual `decidir`: `fio_vivo=3`, zero calendários; acrescentar entrada sem dono para prova positiva uma vez por colaborador. |

Pontos adicionais fora do recorte:

- `chamados/tests/test_e3_c8b_sem_banco.py:81`: schema exato precisa incluir `acao`, caso publicada na tarefa.
- `chamados/tests/test_o261_vigia_fio.py:483`: stub plural `lambda colab, datas` precisa aceitar `agora` keyword.
- `chamados/tests/test_o261e_f5_alcance.py:293`: teste exclusivo do balde extinto, elegível a remoção L-111.
- `chamados/tests/test_o261e_f5_alcance.py:406`: teste misto continua vivo; oferta agora deve resultar `porta_acao/admin`, sem papel inventado `regularizar_batida`.
- `chamados/tests/test_sombra_c1_resolver_em_lote.py:49`: a prova de custo deve acompanhar a carga da tarefa; forçar `porta_da_tarefa=None` depois do julgamento já não força consulta ao Resolver.

Os B sem banco possuem âncoras existentes em `forma_b`, mas os testes diretos não passam o mapa ao juiz (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:103`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:127`). Para provar LU-03, passar `{p.pk: chamado_que_lista(p)}`; `_pela_decisao` atualmente reduz a porta a None fora da fila (`chamados/services/tarefa_do_dia.py:140`).

Comentários V6: substituir a restrição “admin só chega por chamado na fila” por **LU-03: ação administrativa com tela dá porta, inclusive validação com âncora existente fora da fila**; None sem âncora não dá dono. Pontos: `test_e3_bloqueantes.py:97`, `test_o261e_f5_alcance.py:487`, `test_o261e_f5b_juiz_porta.py:264`, `folha/tests/test_haiku_fora_da_folha.py:160`.

Comando sem banco recomendado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_degrau_dono_e_porta
```

Não executei esse comando para evitar disputar o RED/GREEN do construtor. Os demais arquivos listados importam `django.test.TestCase` e suas fixtures escrevem no banco; execução proibida nesta sessão (`chamados/tests/test_o261_vigia_fio.py:24`, `escala/tests/test_f3_beco1_folga_na_lista.py:18`, `chamados/tests/test_f3_beco1_reata_limbo.py:17`).

## 14/11 20:04 · sessao 0a6e6dad6330 · - · - · origem=Ronald (tmux)

Censo realizado somente por leitura.

- `vigia_fio.fio_do_dia` não tem chamador de produção encontrado: a busca encontra somente sua definição em `chamados/services/vigia_fio.py:156`. Os homônimos da Folha e do serviço de veredito são funções distintas. Comando:  
  `git grep -n -E '(vigia_fio import.*fio_do_dia|vigia_fio\.fio_do_dia|fio_do_dia\()' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `_resolver_da_rodada` tem somente os dois chamadores internos dos censos, em `chamados/services/vigia_fio.py:440` e `chamados/services/vigia_fio.py:661`; sua definição fica em `chamados/services/vigia_fio.py:258`. Depois de transferir a consulta para a carga da tarefa e remover essas chamadas, cabe apagá-lo com lápide L-111.

- O balde `decisao_do_dia_pendente` não tem consumidor de produção fora de `vigia_fio.py`: inicialização `:365`, classificação `:443`, passivo `:494`, enumeração `:547` e impressão `:906`. Comando:  
  `git grep -n 'decisao_do_dia_pendente' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia` e `fio_do_admin` são atualmente subdivisões dos dias sem porta: o censo já passou pela guarda da porta em `chamados/services/vigia_fio.py:428`; o alcance passou pela guarda em `chamados/services/vigia_fio.py:669`. As classificações em `chamados/services/vigia_fio.py:308` não escrevem campos da tarefa. Os comentários ainda atribuem “bola” e deveriam declarar explicitamente essa limitação LU-03.

- As duas construções avulsas de `ListaDoAdmin` perdem o instante em `chamados/services/vigia_fio.py:391` e `chamados/services/vigia_fio.py:645`. Devem receber `agora=agora`. A classe já encaminha `self.agora` em `chamados/services/vigia_fio.py:216`; o default de `escala/services/cadastro_realidade.py:775` deixa de ser acionado nesse caminho.

Leitores encontrados:

| Leitor | Uso atual |
|---|---|
| `folha/fora_da_folha.py:244` | Usa `fila` como números de chamados exibidos; a documentação `:241` declara chamados exigíveis da célula. |
| `folha/retrato_folha.py:1190` | Grava fio agregado como `dono`; `portas_da_linha`, `:1015`, consulta mapas de fio/tipo. |
| `chamados/services/vigia_fio.py:412` | Testa presença; o alcance `:673` conta rota e `:675` publica fio. |
| `chamados/services/furo_sem_canal.py:97` | Testa presença e conta rota em `:99`. |
| `escala/services/cadastro_realidade.py:469` | Exclui o beco quando existe porta. |
| `ponto/services/pautas_dos_discordantes.py:81` | Lê fio por projeção da Folha. |
| `chamados/management/commands/diff_e3_sombra.py:50` | Diagnóstico de `fila`; `:143` imprime rota, fio e tamanho. |

Nenhum deles interpreta `acao` atualmente, pela busca textual acima. Incidência dinâmica: `[não verificado]`.

Para M5, conservar `fila=(101,)` faz a Folha exibir 101 ao lado da porta administrativa da pergunta 202 (`folha/fora_da_folha.py:244`). Recomendo alinhar `fila` aos chamados da célula pertencentes às portas do dono e passar âncora/ação separadamente para produzir o link correto.

Consequências nos testes: [nao verificado]

| Teste/local | Tratamento sugerido | [nao verificado]
|---|---|
| `test_o261_vigia_fio.py:485` | Classe c: comportamento específico do balde removido; M4 passa a provar porta administrativa. |
| `test_o261_vigia_fio.py:521` | Classe c: contador `decisao_do_dia_pendente_com_alvo` removido. |
| `test_o261_vigia_fio.py:564` | Classe c quanto à precedência do balde; M4/M6 devem preservar a cobertura de porta. |
| `test_o261_vigia_fio.py:495` | Preservar: prova o juiz real; inverter resultado para `fio_vivo`, sem balde. |
| `test_o261_vigia_fio.py:513` | Preservar a contraprova sem oferta; retirar somente consulta à chave apagada. |
| `test_o261_vigia_fio.py:680` | Preservar soma; retirar termo do balde e contar o dia oferecido em `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:35` | Preservar desempenho; trocar termo do balde por `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:70` | Preservar paridade e fragmentação da plural, que continua viva. |
| `test_n1_perguntas_do_app.py:383` | Fixar oferta falsa para os dias chegarem ao subdivisor; a transferência da consulta para a tarefa muda o caminho. |

A fixture `_oferece` em `chamados/tests/test_o261_vigia_fio.py:483` precisa aceitar `**kw` para receber `agora`. A constante `PERMISSAO_DO_RESOLVER_DIA` tem teste de tela em `chamados/tests/test_o261e_f5_alcance.py:451`; não removê-la inadvertidamente junto do balde sem adaptar essa prova viva.

## 14/11 20:02 · sessao 941f6ed772c9 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na fonte: `pago = max(0, previsto - trabalhado_do_dia)`, preservando `cobre` e nomeando o excedente para revisão. O bruto informa o motivo; não impõe outro teto (`ponto/services/efeito_ausencia.py:138`, `ponto/services/efeito_ausencia.py:145`).

**Q1.** O motor recebe o crédito pelo mapa alimentado em `ponto/services/fechamento.py:755`, `ponto/services/espelho.py:1134` e `relatorios/pdf_espelho.py:407`. Quando um dia justificado está ausente desse mapa, o motor usa o previsto inteiro (`ponto/motor_calculo_v2.py:2629`).

Com `cobre` vazio, a fonte exclui o dia de `justificadas`; portanto, os dados fornecidos não demonstram como os 420 chegaram ao motor. Além disso, o campo medido `minutos_abonados` é gravado da grade, por `por_dia_da_grade`, em `ponto/services/fechamento.py:1066`. A origem exata dos 420 produtivos permanece **[nao verificado]**. A cura utiliza o feed existente, sem editar o motor (`ponto/services/efeito_ausencia.py:47`).

**Q2.** A recusa integral podia retirar a justificativa usada pelo DSR. O leitor exige que o dia obrigatório tenha trabalho reconhecido ou esteja em `datas_justificadas`; não exige pagamento integral (`ponto/motor_calculo_v2.py:2605`, `ponto/motor_calculo_v2.py:3490`). O teste local executa o ramo real do motor com crédito 361, sem falta e com `dsr_ok=True` (`ponto/tests/test_justificativa_soma_sem_banco.py:262`). A explicação exata do 4→3 produtivo permanece **[nao verificado]**.

**Q3.** Existe `decidir_he`, destinada a minutos reais fora da janela (`ponto/portas/he.py:138`). A Gestão de HE lê essas pontas do retrato (`ponto/services/he_pendente_lavrado.py:19`). Para o excedente da justificativa, acrescentei a projeção `justificativas_para_revisao`, consumida pela Central no filtro “Revisar aprovadas”, uma linha por dia-colaborador (`ponto/services/efeito_ausencia.py:151`, `ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:56`).

**Q4.** `cobre` mantém os marcos; somente o crédito muda. A grade dispensa os marcos cobertos e transporta o número; `dia_pago` soma esse número sem exigir a duração integral do período (`escala/utils.py:1266`, `escala/utils.py:1277`, `ponto/services/dia_pago.py:398`). Cobertura inteira também recebe mapa explícito, inclusive zero, evitando o fallback do motor (`ponto/services/efeito_ausencia.py:47`).

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `previsto` | Ata: `ponto/services/cartorio.py:739`; leitor comum: `escala/services/leitor_celula.py:354` | `ponto/services/efeito_ausencia.py:138` |
| `trabalhado` | Autoridade `realizado_do_dia`: `ponto/turnos.py:774`; sem turno retorna `None`: `ponto/turnos.py:721` | Adaptação para zero e conta: `ponto/services/efeito_ausencia.py:141` |
| `cobre` | Cobertura e união existentes: `ponto/services/efeito_ausencia.py:125`, `ponto/services/efeito_ausencia.py:133` | Efeito: `ponto/services/efeito_ausencia.py:46`; grade: `escala/utils.py:1257` |
| `minutos` | Saldo diário: `ponto/services/efeito_ausencia.py:145` | Feed: `ponto/services/efeito_ausencia.py:47`; grade: `escala/utils.py:1277` |
| `motivo` | `ponto/services/efeito_ausencia.py:147` | Projeção: `ponto/services/efeito_ausencia.py:153`; Central: `ponto/views.py:1747`; apresentação: `templates/ponto/fila_justificativas.html:116` |
| Feed do motor | Três chamadores citados em Q1, incluindo órfão em `ponto/services/fechamento.py:831` | Abono: `ponto/motor_calculo_v2.py:2628` |

Outros alimentadores permanecem lendo o mesmo mapa: esmeril (`ponto/services/esmeril_espelho.py:166`), extrato (`relatorios/management/commands/diff_extrato_parcial.py:62`) e comparação de alimentação (`ponto/management/commands/diff_espelho_alimentacao.py:124`).

Diff por arquivo::função:

- `efeito_ausencia.py::periodos_justificados`: substitui recusa integral pelo saldo diário; trata ausência de turno (`ponto/services/efeito_ausencia.py:136`).
- `efeitos_da_ausencia` e `utils.py::_marcar_ausencias`: transportam crédito explícito para cobertura inteira (`ponto/services/efeito_ausencia.py:47`, `escala/utils.py:1257`).
- `views.py::fila_justificativas` e template: revisão pelo motivo da fonte (`ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:116`).
- Testes: S1–S6 invertidos, T1/T7, propriedades, feed real, ramo do motor e Central (`ponto/tests/test_justificativa_soma_sem_banco.py:83`, `ponto/tests/test_justificativa_soma_sem_banco.py:163`, `ponto/tests/test_justificativa_soma_sem_banco.py:185`).
- Caracterizações anteriores ajustadas à fórmula nova; ORM escrito, com relógio fixado (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:209`, `ponto/tests/test_justificativa_paga_o_periodo.py:600`).

RED antes da implementação: **20 testes, 15 falhas por asserção**, preservado em [justificativa_soma_red.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:179). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
```

GREEN local: **39 testes passando**, em [justificativa_soma_green.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:42). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
```

Mutantes: **controle verde e 12/12 mortos por asserção**. Os comandos individuais e resultados estão em [justificativa_soma_mutantes.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma recusa_integral_c5
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_max_zero
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma feed_motor_le_bruto
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
```

ORM escrito para a sessão executar; resultado **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo --settings=config.settings.ci
```

Não executei ORM, banco, rede, Docker, reconstrução, commit, merge, push, deploy ou crons. Não alterei motor, portas protegidas, modelos, migrations ou docs. Certificação independente dessas restrições: **[nao verificado]**. Frota dos 41, contagem produtiva por motivo, 9 colaboradores da competência 11 e exportadas: **[nao verificado]**. A certificação fica com Code/suíte e opus final. [nao verificado]

`LEI-AKITA: origem=Ronald 14/11 19:4x (ponto/services/efeito_ausencia.py:83); testemunha=feed real PDF e ramo real motor (ponto/tests/test_justificativa_soma_sem_banco.py:242, :262); RED=ponto/tests/justificativa_soma_red.txt:181; quem-mais-le=grade/fechamento/espelho/PDF/esmeril/extrato; juizes novos=0; padrao=single source of truth / um juiz por pergunta (docs/PLANO-PADROES.md:46).`

_5265 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
