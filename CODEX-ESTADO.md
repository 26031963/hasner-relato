# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 17:00** · publicado em **14/11/2026 17:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 17:00 · sessao 8fb89e816b92 · E3-V10-DEGRAU-PUB · censo · origem=Code

**Nenhum B identificado nos 40 testes.** [não verificado: conclusão da triagem estática, sem reexecução] [nao verificado]

Referências relativas a `app/`. Nomes dos testes sem o prefixo `test_`. [nao verificado]

Regras usadas: **1** = responder + pergunta pendente no app, independentemente da âncora/prazo (`chamados/services/tarefa_do_dia.py:43`); **2** = ação administrativa/sistema alcançável (`:45`); **3** = residual sem `cobrar` (`:46`). **4*** = publicar rota somente com dono (`chamados/services/tarefa_do_dia.py:612`); sua numeração literal como item 4 é **[não verificado]**, pois o topo enumera apenas 1–3.

| teste | arquivo:linha do assert/erro | CLASSE | o que mudar e onde | [nao verificado]
|---|---|---|---|
| MORDE_o_universo_e_o_mesmo_do_fora_da_folha | chamados/tests/test_n1_alimentacao_unica.py:127 | F | Helper `_conta_pda`, em `:90`, precisa receber/repassar `em_lote` e `agora`; assinatura exigida em `chamados/services/pergunta_no_app.py:74`. Preservar igualdade dos universos. |
| RED_a_porta_e_alimentada_uma_vez_por_rodada | chamados/tests/test_n1_alimentacao_unica.py:99 | F | Mesmo helper `:90`: repassar os argumentos novos. Preservar alimentação única em `:101`; consulta do app é memorizada separadamente por colab (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_esta_no_app_se_pergunta_uma_vez_por_colaborador | chamados/tests/test_n1_alimentacao_unica.py:106 | F | Corrigir helper `:90`; depois, regra **1**: trocar expectativa zero de `:107` por uma consulta por colab utilizado na carga, constante em dias (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_segura_semeia_o_mapa_com_o_colaborador_pedido | chamados/tests/test_n1_alimentacao_unica.py:147 | F | `_le`, em `:141`, deve repassar kwargs. Regra **1**: atualizar zero leituras em `:149`; preservar proibição de busca individual em `:153` (`chamados/services/pergunta_no_app.py:59`). |
| a_saida_do_dia_nao_muda | chamados/tests/test_n1_alimentacao_unica.py:111 | F | Corrigir `_conta_pda` em `:90`. Regra **1** conserva a porta existente neste cenário pendente; manter verificações de saída (`chamados/tests/test_n1_alimentacao_unica.py:113`). |
| v8_R10_ancora_fora_do_fato_sem_acao | chamados/tests/test_e3_portas_tarefa.py:601 → chamados/services/tarefa_do_dia.py:551 | F | Inicializar `_pendentes_app` no objeto criado por `__new__`, `chamados/tests/e3_v8_apoio.py:127`; fornecer pendentes vazios ou mock de consulta. Inicialização real em `chamados/services/tarefa_do_dia.py:388`. |
| lote_responde_o_mesmo_que_o_avulso_e_com_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:1180 | F | Separar custo do lote e nova consulta por colab; preservar igualdade lote/avulso. Parcela nova constante com dias, crescente com colabs; não fixar simplesmente 26 (`chamados/services/tarefa_do_dia.py:551`). |
| lote_tem_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:938 | F | Comparação altera simultaneamente dias e colabs. Medir dias com colabs fixos e contabilizar consulta por colab; não substituir por teto 45 (`chamados/tests/test_e2_tarefa_do_dia.py:931`; `chamados/services/tarefa_do_dia.py:551`). |
| RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:168 | L | **1**: novo `porta_app/colab`; atualizar também `:170` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:161 | L | **1**: novo `porta_app/colab`; atualizar também `:163` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| classe_e_viva_sob_terminal_reabre_pela_porta_com_nota | chamados/tests/test_e4_passivo.py:73 → chamados/tests/test_e4_backfill.py:112 | L | **1**: pendente sob terminal já tem `porta_app/colab`; esperar plano desse dia zero, sem reabertura/nota. Atualizar caracterização em `:73` (`chamados/services/tarefa_do_dia.py:43`; `chamados/services/vigia_fio.py:658`). |
| mutante_sem_porta_morde_caracterizacao_classe_e | chamados/tests/test_e4_passivo.py:90 | L | **1**: neste cenário, reabrir credor deixou de ser necessário. Reenquadrar teste de `:87` para verificar ausência dessa chamada; trocar apenas regex esconderia a mudança (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_dry_nao_escreve | chamados/tests/test_f3_beco1_reata_limbo.py:72 | L | **1**: novo zero candidatos/pais C11; preservar ausência de escrita. Para exercitar DRY com candidatos, montar cenário realmente sem porta (`escala/services/cadastro_realidade.py:469`). |
| MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata | chamados/tests/test_f3_beco1_reata_limbo.py:159 | L | **1**: pergunta já tem porta do app; novo zero reatas. Refazer cenário sem porta para continuar cobrindo silêncio vencido (`escala/services/cadastro_realidade.py:469`). |
| MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce | chamados/tests/test_f3_beco1_reata_limbo.py:82 | L | **1**: novo sem linha `SEM PAI`, pois não entra em C11. Preservar teste de ausência de pai com cenário sem porta (`escala/services/cadastro_realidade.py:469`). |
| RED_apply_reata_ao_pai_e_o_dia_sai_do_beco | chamados/tests/test_f3_beco1_reata_limbo.py:53 | L | **1**: novo `_c11()==[]` antes do apply; zero reatas. Refazer cenário sem porta para testar costura (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:145 | L | **1**: cenário atual não seleciona C11, portanto não alcança a corrida. Criar candidato sem porta e preservar guarda contra reata após silêncio (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_fora_da_fila_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:119 | L | **1**: novo sem linha `PAI FORA DA FILA`; há porta do app. Refazer cenário sem porta para conservar essa cobertura (`escala/services/cadastro_realidade.py:469`). |
| MORDE_F5_queries_nao_crescem_com_os_dias | escala/tests/test_f3_beco1_folga_na_lista.py:322 | L | **1**: novo `(d1,d5)=(0,0)`. Refazer cenário C11 sem porta antes de medir queries; consulta nova fica uma por colab, constante com dias (`escala/services/cadastro_realidade.py:469`; `chamados/services/tarefa_do_dia.py:551`). |
| MORDE_competencia_anterior_NAO_exportada_vai | escala/tests/test_f3_beco1_folga_na_lista.py:110 | L | **1**: novo C11 `[]`; competência anterior não elimina porta da pergunta pendente (`chamados/services/tarefa_do_dia.py:43`; `escala/services/cadastro_realidade.py:469`). |
| RED_F1_lista_le_o_instante_de_quem_chama | escala/tests/test_f3_beco1_folga_na_lista.py:219 | L | **1**: novo C11 `[]` nesse instante. Refazer cenário sem porta para preservar prova do relógio (`escala/tests/test_f3_beco1_folga_na_lista.py:215`; `escala/services/cadastro_realidade.py:469`). |
| RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa | escala/tests/test_f3_beco1_folga_na_lista.py:265 | L | **1** remove C11; novo conjunto `{('C9','BATIDA')}`, preservando dono lavrado. Refazer C11 sem porta para continuar testando reconciliação de donos (`escala/tests/test_f3_beco1_folga_na_lista.py:262`; `escala/services/cadastro_realidade.py:988`). |
| RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:180 | L | **1**: manter rota/fila/verbos; novo `fio='colab'` pela pergunta pendente sob outra âncora. C11 continua vazio (`escala/tests/test_f3_beco1_folga_na_lista.py:173`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:204 | L | **1**: novo C11 `[]`; pendentes sob terminal/registrado têm porta do app (`escala/tests/test_f3_beco1_folga_na_lista.py:193`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO | escala/tests/test_f3_beco1_folga_na_lista.py:88 | L | **1**: novo C11 `[]`, sem dono C11. Refazer cenário sem porta para testar destino CADASTRO (`escala/services/cadastro_realidade.py:469`). |
| MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta | chamados/tests/test_o261e_f5b_juiz_porta.py:272 | L | **1**: novo fio da folha `colab`, com `porta_app`; arquivamento da âncora não impede resposta pendente (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_porta_vem_da_TAREFA_e_nao_do_app | chamados/tests/test_o261e_f5_alcance.py:256 | L | **1, 4***: carga deve consultar app; mock vazio em `:253` implica `nada/None`. Atualizar também contadores em `:255`, além de zero→uma consulta (`chamados/services/tarefa_do_dia.py:551`; `:616`). |
| a_ancora_VIVA_fora_da_fila_tambem_e_veu | chamados/tests/test_o261e_f5_alcance.py:460 → helper :134 | L | **1**: novo `porta_app/colab`; retirar expectativa de causa sem porta `a/registrado` (`chamados/services/tarefa_do_dia.py:43`). |
| b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa | chamados/tests/test_o261e_f5_alcance.py:196 | L | **1, 4***: carimbo respondido exclui dos pendentes; novo `nada/None`, não colab. A cópia já suprime a rota sem dono exibida no anexo (`chamados/services/pergunta_no_app.py:75`; `chamados/services/tarefa_do_dia.py:616`). |
| e_pergunta_viva_sob_chamado_TERMINAL_tem_nome | chamados/tests/test_o261e_f5_alcance.py:222 → helper :134 | L | **1**: novo `porta_app/colab`; não pertence à lista sem porta da causa `e` (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante | folha/tests/test_haiku_fora_da_folha.py:173 | M | **4***: renovar substrings para texto `SEM PORTA`, preservando garantia de ação alcançável (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA | folha/tests/test_haiku_fora_da_folha.py:81 | M | **4***: `SEM FIO`→`SEM PORTA`; preservar verificação `fio=nada` (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| endpoint_do_app_filtra_por_q_vivas | chamados/tests/test_contract_pergunta_viva.py:76 | M | Atualizar string para `perguntas_do_app(colab, em_lote=True, agora=agora)`; conservar lote/C6 e encaminhamento do relógio (`chamados/services/pergunta_no_app.py:74`). |
| todo_mutante_da_E3_tem_alvo_unico | chamados/tests/test_e3_mutantes_alvo.py:25 | M | Retargetar `consulta_vazia` e `consulta_sem_lote` para assinatura com `agora=agora`. **Já ajustados na cópia**; manter unicidade (`chamados/tests/e3_mutantes.py:34`; `:59`). |
| cobrar_precede_validar_e_decidir_sem_depender_dos_donos | chamados/tests/test_e3_portas_tarefa.py:478 | L | **2, 3**: novo `fio='admin'`; conservar fila/verbos. `cobrar` residual não dá dono colab (`chamados/services/tarefa_do_dia.py:45`; `:46`). |
| fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar | chamados/tests/test_e3_portas_tarefa.py:457 | L | **1, 4***: entrada não fornece pergunta pendente; novo `nada/None`. Esse `validar` está na porta da ação, não no residual (`chamados/services/tarefa_do_dia.py:584`; `:606`). |
| RED_A_na_fila_aparece_com_acao_sob_B | chamados/tests/test_e3_portas_tarefa.py:413 | L | **1, 3**: novo `fio='chamado'` pelo residual `decidir`; conservar rota/fila/verbos. Não há pendente fornecida (`chamados/tests/test_e3_portas_tarefa.py:411`; `chamados/services/tarefa_do_dia.py:46`). |
| v8_R1_colab_alcancavel_e_leitores | chamados/tests/test_e3_portas_tarefa.py:490 | L | **1, 4***: entrada atual dá `nada/None`. Para preservar cenário nominal colab, fornecer `pendentes_app={303}` em `:489`; então colab é justificado pela regra **1** (`chamados/services/tarefa_do_dia.py:43`). |
| v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia — ambos subtestes | chamados/tests/test_e3_portas_tarefa.py:514 | L | **1, 2**: entrada atual dá `porta_fila/admin`, mantendo verbos/ações. Para provar simultaneidade com colab, fornecer identidade da pergunta e pendentes em `:509`/`:513` (`chamados/services/tarefa_do_dia.py:43`; `:45`). |
| alvos | chamados/tests/test_e3_volta7_sem_banco.py:92 → chamados/tests/test_e3_mutantes_alvo.py:25 | M | Mesmo selo delegado: conservar execução; alvos com relógio já atualizados na cópia (`chamados/tests/e3_mutantes.py:34`; `:59`). |

**B: nenhum identificado. Contagem: L=27; F=8; M=5; B=0; ?=0 — 40 testes distintos.** [não verificado por execução; consolidação das linhas acima] [nao verificado]

## 14/11 16:59 · sessao 47b7900a56ce · - · - · origem=Ronald (tmux)

| Teste | Assert/erro | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_v8_R10_ancora_fora_do_fato_sem_acao` | `app/chamados/tests/test_e3_portas_tarefa.py:601`; erro `app/chamados/services/tarefa_do_dia.py:551` | F | Inicializar `_pendentes_app={1: frozenset()}` no helper que usa `__new__`, `app/chamados/tests/e3_v8_apoio.py:127`. A carga real inicializa o memo em `app/chamados/services/tarefa_do_dia.py:388`. Não introduz queries no helper; na carga real, consulta uma vez por colab, constante com os dias, crescente com colabs (`app/chamados/services/tarefa_do_dia.py:551`). |
| `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:161` | L | Item 1: pergunta pendente com responder tem porta independente da âncora; novo `rota='porta_app'`, `fio='colab'` (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também a exclusão do furo: `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:163`; porta publicada sai dos sem canal (`app/chamados/services/furo_sem_canal.py:104`). |
| `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:168` | L | Item 1: novo `porta_app/colab`, independentemente da âncora registrada (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também para `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:170`, conforme consumo da porta em `app/chamados/services/furo_sem_canal.py:98`. |
| `test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | `app/chamados/tests/test_e4_passivo.py:73` → helper `app/chamados/tests/test_e4_backfill.py:112` | L | Item 1: responder pendente abre `porta_app/colab` sem reabrir terminal (`app/chamados/services/tarefa_do_dia.py:43`). Novo cenário: plano desse dia `0`, sem reabertura/nota; adaptar verificações em `app/chamados/tests/test_e4_passivo.py:73`. O vigia trata porta antes da classe sem porta (`app/chamados/services/vigia_fio.py:658`). |
| `test_mutante_sem_porta_morde_caracterizacao_classe_e` | `app/chamados/tests/test_e4_passivo.py:90` | L | Mesma mudança do item 1: reabrir credor deixou de ser necessário para esta pendente (`app/chamados/services/tarefa_do_dia.py:43`). Reenquadrar o selo em `app/chamados/tests/test_e4_passivo.py:87` para verificar que a porta de reabertura não é chamada; não basta trocar o regex, pois o alvo comportamental mudou (`app/chamados/services/vigia_fio.py:658`). |
| `test_RED_A_na_fila_aparece_com_acao_sob_B` | `app/chamados/tests/test_e3_portas_tarefa.py:413` | L | Itens 1 e 3: sem pergunta pendente fornecida, responder não dá colab; residual A `decidir` dá `chamado`. Novo `('porta_fila',(101,),{101:'decidir',202:'cobrar'},'chamado')`; modificar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:414`, pelas regras `app/chamados/services/tarefa_do_dia.py:43` e `:46`. |
| `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | `app/chamados/tests/test_e3_portas_tarefa.py:457` | L | Itens 1 e 4: insumo atual não informa pergunta pendente; novo `fio='nada'`, `rota=None` (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). `validar` pertence à porta da ação, portanto não é residual separado (`app/chamados/services/tarefa_do_dia.py:584`, `:606`). Alterar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:457`. |
| `test_cobrar_precede_validar_e_decidir_sem_depender_dos_donos` | `app/chamados/tests/test_e3_portas_tarefa.py:478` | L | Itens 2 e 3: validar/admin alcançável permanece; cobrar residual não cria colab. Novo mesmo `fila/verbos`, `fio='admin'`; modificar `app/chamados/tests/test_e3_portas_tarefa.py:479`, conforme `app/chamados/services/tarefa_do_dia.py:45`. |
| `test_v8_R1_colab_alcancavel_e_leitores` | `app/chamados/tests/test_e3_portas_tarefa.py:490` | L | Itens 1 e 4: embora tenha `pergunta_id=303`, não fornece pendentes; novo `fio='nada'`, `rota=None`, `porta_da_tarefa=None`, mantendo verbos (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). Atualizar assert e projeções em `app/chamados/tests/test_e3_portas_tarefa.py:490`. Para preservar o cenário nominal alcançável, fornecer explicitamente `pendentes_app={303}` na chamada de `:489`. |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — ambos subtestes | `app/chamados/tests/test_e3_portas_tarefa.py:514` | L | Itens 1 e 2: fixture não fornece pergunta identificada nem pendentes; `dia_plano` usa `pergunta_id=None` (`app/chamados/tests/e3_portas_apoio.py:29`). Novo `('porta_fila','admin',verbos)` em ambos, mantendo ações; mudar `app/chamados/tests/test_e3_portas_tarefa.py:514`, conforme `app/chamados/services/tarefa_do_dia.py:43`. Para continuar provando simultaneidade com colab pendente, fornecer identidade e `pendentes_app` em `:509`/`:513`. |

Nestes **10 testes distintos: L=9, F=1, B=0**; R2 aparece duas vezes por subtestes (`app/chamados/tests/test_e3_portas_tarefa.py:507`). A cópia já impede publicar rota com `fio=nada`, embora o anexo anterior mostre `porta_pergunta/nada` (`app/chamados/services/tarefa_do_dia.py:616`).

O topo enumera somente itens 1–3 (`app/chamados/services/tarefa_do_dia.py:43`); usei “item 4” como a regra explícita do aval 16:16 sobre publicação somente com dono, registrada em `app/chamados/services/tarefa_do_dia.py:612`. A numeração literal 1–4 no corte 16:16 permanece [não verificado].

## 14/11 16:58 · sessao af390de1e0e6 · - · - · origem=Ronald (tmux)

Na cópia, os itens numerados são apenas 1–3; a regra de publicação às 16:16 aparece separadamente, sem número (`app/chamados/services/tarefa_do_dia.py:43`, `:45`, `:46`, `:612`). O item 4 literal em CORTES é [não verificado].

| Teste | Assert/erro na cópia | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:272` | L | Item 1: pergunta pendente + responder dá `colab`, mesmo com âncora arquivada; sem fila, `porta_app` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). Atualizar expectativa da folha. |
| `test_MORDE_a_porta_vem_da_TAREFA_e_nao_do_app` | `app/chamados/tests/test_o261e_f5_alcance.py:256` | L | Item 1 torna a consulta necessária: carga consulta uma vez por colaborador (`app/chamados/services/tarefa_do_dia.py:551`). O mock deliberadamente vazio em `app/chamados/tests/test_o261e_f5_alcance.py:253` significa pergunta fora dos pendentes: esperar `nada`, sem porta, e não conservar `(fio_morto=0, porta_pergunta=1)` (`app/chamados/services/tarefa_do_dia.py:598`, `:616`). |
| `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | `app/chamados/tests/test_o261e_f5_alcance.py:460`; helper `:134` | L | Item 1: trocar causa `a/registrado` por `porta_app/colab`; a fila da âncora não limita o colaborador (`app/chamados/services/tarefa_do_dia.py:43`). |
| `test_b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa` | `app/chamados/tests/test_o261e_f5_alcance.py:196` | L | Item 1: carimbo de resposta vazia não integra pendentes (`app/chamados/tests/test_o261e_f5_alcance.py:109`; `app/chamados/services/pergunta_no_app.py:75`). Não esperar `colab`; sem outro dono, `fio=nada`, `rota=None` pela publicação (`app/chamados/services/tarefa_do_dia.py:612`). O anexo mostra `porta_pergunta/nada`, estado já eliminado na cópia por `:616`. |
| `test_e_pergunta_viva_sob_chamado_TERMINAL_tem_nome` | `app/chamados/tests/test_o261e_f5_alcance.py:222`; helper `:134` | L | Item 1: pendente a responder conserva `porta_app/colab` sob terminal; não é linha sem porta da causa `e` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). |
| `test_MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante` | `app/folha/tests/test_haiku_fora_da_folha.py:173` | M | Renovar as três substrings antigas de `:172` para o contrato textual publicado, `SEM PORTA: a tarefa nao aponta acao alcancavel neste dia; alguem precisa olhar` (`app/folha/fora_da_folha.py:54`), coerente com publicação apenas com dono (`app/chamados/services/tarefa_do_dia.py:612`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | `app/folha/tests/test_haiku_fora_da_folha.py:81` | M | Selo textual mudou de `SEM FIO` para `SEM PORTA`; preservar assert `fio=nada` em `:80` e verificar o texto publicado (`app/folha/fora_da_folha.py:54`; `app/chamados/services/tarefa_do_dia.py:616`). |

Contagem deste subconjunto: **L=5, M=2, B=0** (classificações e fundamentos nas sete linhas acima).

## 14/11 16:58 · sessao 2b92adb14c68 · - · - · origem=Ronald (tmux)

Os **13 testes deste recorte são L**: `_beco` cria pergunta de marco sem resposta em disputa aberta (`app/escala/tests/test_f3_beco1_folga_na_lista.py:55-57`), pendente no app (`app/chamados/services/pergunta_no_app.py:74-76`). Pela lei **item 1**, responder+pendente dá `fio='colab'`, independentemente da âncora (`app/chamados/services/tarefa_do_dia.py:43-44`). C11 só admite tarefa sem porta (`app/escala/services/cadastro_realidade.py:469`).

| teste | assert/erro | CLASSE | mudança | [nao verificado]
|---|---|---|---|
| `test_MORDE_dry_nao_escreve` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:72` | L | Item 1: cenário atual tem `porta_app/colab`; esperar zero candidatos/zero pais no DRY. Refazer cenário sem porta para conservar cobertura do command (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:159` | L | Item 1: cenário atual tem porta do app; zero reatas. Refazer cenário sem porta para testar vencimento do silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:82` | L | Item 1: não entra em C11; sem linha `SEM PAI`, zero reatas. Refazer cenário sem porta para testar ausência de pai (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:53` | L | Item 1: `_c11()` novo é `[]` antes do apply; zero reatas. Refazer cenário sem porta para cobrir reapontamento (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:145` | L | Item 1: cenário atual nem seleciona C11; não alcança a disputa de concorrência. Refazer cenário sem porta, conservando a proibição de reatar após silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:119` | L | Item 1: cenário atual não entra em C11; não há linha `PAI FORA DA FILA`. Refazer cenário sem porta para testar a guarda (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:322` | L | Item 1: neste cenário `(d1,d5)` novo é `(0,0)`; para preservar o teste de custo, refazer cenário realmente sem porta e manter assert de queries constantes por dia (`app/escala/services/cadastro_realidade.py:429-432,469`). |
| `test_MORDE_competencia_anterior_NAO_exportada_vai` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:110` | L | Item 1: aberta/anterior não suprime a porta do app; C11 novo `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F1_lista_le_o_instante_de_quem_chama` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:219` | L | Item 1: no instante passado fornecido a pergunta está pendente; C11 novo `[]`. Refazer cenário sem porta para conservar a prova do relógio (`app/escala/tests/test_f3_beco1_folga_na_lista.py:215-218`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:265` | L | Item 1 remove C11; resta só C9 lavrado `BATIDA`, portanto novo conjunto `{('C9','BATIDA')}` e `dias[...]='BATIDA'`. Sem dois donos, não há reconciliação; refazer cenário C11 sem porta para continuar testando divergência (`app/escala/tests/test_f3_beco1_folga_na_lista.py:262`; `app/escala/services/cadastro_realidade.py:988-989`). |
| `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:180` | L | Item 1: manter rota/fila/verbos, mudar `fio` para `colab` pela pergunta pendente sob a outra âncora; C11 continua `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:173-175`). |
| `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:204` | L | Item 1: perguntas pendentes têm porta do app mesmo sob terminal/registrado; C11 novo `[]` (`app/escala/tests/test_f3_beco1_folga_na_lista.py:193-198`; `app/chamados/services/tarefa_do_dia.py:43-44`). |
| `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:88` | L | Item 1: dia com porta do app não é C11; novo `c11=[]`, sem dono C11 publicado. Refazer cenário sem porta para testar `CADASTRO` (`app/escala/services/cadastro_realidade.py:469,512`). |

**B: nenhum neste recorte. Contagem: L=13, F=0, M=0, B=0, ?=0** — enumeração das linhas da tabela acima. [não verificado]

## 14/11 16:53 · sessao 870e29998817 · E3-V10-DEGRAU-PUB · verificador · origem=Code

**PROCEDENTE: há perda de informação de fila na Folha, rótulo administrativo insuficiente e uma exceção que C13 aceita apesar do “sse” anunciado.** Não encontrei quebra de vocabulário por `porta_acao` nos contadores alterados. (`app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:46`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`, `app/chamados/services/furo_sem_canal.py:117`, `app/chamados/services/vigia_fio.py:577`)

A identidade criptográfica da cópia e a comparação Git contra `52110c313` ficaram **[nao verificado]**: a pasta entregue não contém `.git`. Contraexemplo nominal de produção também fica **[nao verificado]**; os anexos fornecidos não apresentam uma linha real com todos os insumos necessários.

**1. Escritores, leitores e mudança de baldes**

O escritor dos quatro campos **da tarefa** é `_portas_da_tarefa`: constrói `verbos`, `fila`, escolhe `fio` e publica `rota`. `porta_da_tarefa` apenas projeta esses campos quando a rota existe; caso contrário, devolve `None`, descartando a estrutura inteira na projeção. (`app/chamados/services/tarefa_do_dia.py:596`, `app/chamados/services/tarefa_do_dia.py:597`, `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`)

| Leitor executável | Com `porta_acao` | Com `None`, embora reste `cobrar` na fila |
|---|---|---|
| Folha: `_porta`, `_fio_do_dia`, `fora_da_folha` | Lê `fio=admin`; soma em admin/grupo A. | Converte em `nada`; soma em nada/grupo D. Não perde o dia do universo. (`app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:195`) |
| Folha: `o_que_segura` | Usa fio admin e seu texto fixo. | Publica `chamado=[]`, mesmo que a tarefa conserve fila residual. **PROCEDENTE: perda do dado.** (`app/folha/fora_da_folha.py:239`, `app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:247`) |
| Furo: `medir` | Soma em `com_porta_acao`; exclui de `total`. | Inclui em `sem_canal/total`; conserva a leitura de `nao_sei`. (`app/chamados/services/furo_sem_canal.py:98`, `app/chamados/services/furo_sem_canal.py:100`, `app/chamados/services/furo_sem_canal.py:104`, `app/chamados/services/furo_sem_canal.py:117`) |
| Vigia: `censo_alcance` | A chave dinâmica existe; soma rota, papel admin e contador de exigência do admin. | Passa ao teto temporal, aos quatro baldes legais ou às causas a–e de fio morto. **Não significa automaticamente `fio_morto_alcance`.** (`app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:666`, `app/chamados/services/vigia_fio.py:672`, `app/chamados/services/vigia_fio.py:714`) |
| Beco: `beco_em_lote` | Presença de porta exclui o dia. | Torna o dia candidato, ainda condicionado a perguntas vivas, âncoras e demais critérios do beco. (`app/escala/services/cadastro_realidade.py:468`, `app/escala/services/cadastro_realidade.py:469`, `app/escala/services/cadastro_realidade.py:474`) |
| Pauta: `dias_sem_fio` | Admin é excluído. | Um dia `discordante` passa a ser elegível ao universo de pauta. (`app/ponto/services/pautas_dos_discordantes.py:72`, `app/ponto/services/pautas_dos_discordantes.py:81`, `app/ponto/services/pautas_dos_discordantes.py:83`) |
| `diff_e3_sombra` | Compara valores sem catálogo fechado de rotas; lê os quatro campos. | Projeta fio nada, fila vazia e ausência de porta para seus consumidores simulados. (`app/chamados/management/commands/diff_e3_sombra.py:26`, `app/chamados/management/commands/diff_e3_sombra.py:49`, `app/chamados/management/commands/diff_e3_sombra.py:56`, `app/chamados/management/commands/diff_e3_sombra.py:143`) |
| `lavrar_furo_sem_canal` | Imprime o contador novo; pode gravar o resultado calculado pelo serviço. | Imprime/grava o novo `total`; não reinterpreta a tarefa. (`app/chamados/management/commands/lavrar_furo_sem_canal.py:33`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`) |

**NAO PROCEDENTE — `KeyError` provocado por `porta_acao` nesses leitores.** A Folha indexa pelo fio admin, não pela rota; o vigia inicializa `alcance_porta_acao`; o furo enumera essa rota. (`app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:245`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/furo_sem_canal.py:117`)

**PROCEDENTE — rótulo administrativo insuficiente.** A tarefa pode publicar `decidir_ausencia/admin` sem chamado, obtendo `porta_acao`, enquanto `DESTRAVA['admin']` fala somente em validar/recusar o que o painel põe em Validar. O retrato reutiliza esse texto. Ocorrência nominal: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:303`, `app/chamados/services/tarefa_do_dia.py:621`, `app/folha/fora_da_folha.py:46`, `app/folha/retrato_folha.py:1069`)

**2. Segundo escritor fora da porta**

**PROCEDENTE — existe outro escritor literal.** C17 publica seu próprio `{rota, fila, verbos, fio}`, limitado a `porta_fila/porta_pergunta`. Com residual `cobrar`, pode publicar porta e fio colab onde a tarefa atual publica `None`. (`app/chamados/juizes.py:1650`, `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1709`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:616`)

**NAO PROCEDENTE — leitor atual confundindo as duas fontes.** O chamador remanescente é o comparador: chama C17 como `atual`, a tarefa como candidata e mantém as assinaturas separadas. Essa fronteira está declarada no registro e cobrada pela catraca de chamadores. (`app/chamados/management/commands/diff_e3_sombra.py:127`, `app/chamados/management/commands/diff_e3_sombra.py:129`, `app/chamados/management/commands/diff_e3_sombra.py:141`, `app/core/juizes.py:136`, `app/chamados/tests/test_e3_pendentes_app.py:145`)

**3. Regras próprias de dono/porta nos leitores**

**PROCEDENTE — o vigia conserva uma classificação adicional.** Depois de receber `None`, consulta `dono_fora_do_fio`, que compõe Resolver dia, ausência aberta, folga validada e lista do admin. Portanto, Folha e vigia podem dar destinos diferentes ao mesmo resultado sem porta. Essa cadeia não republica `rota`, mas é uma decisão adicional de balde e papel. (`app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:308`, `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:672`)

**NAO PROCEDENTE — retrato recalculando alcance por ações/verbos.** Ele recebe o grupo da Folha, grava o fio correspondente e aplica o agrupamento de apresentação `chamado/ninguem → admin`. Seus botões são mapas próprios de navegação por fio e tipo. (`app/folha/retrato_folha.py:177`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`, `app/folha/retrato_folha.py:1015`)

**NAO PROCEDENTE — template da Folha derivando a rota.** O partial imprime os dados e botões preparados em Python. A API consultada usa `consulta_do_app` para a própria lista de perguntas, não a rota publicada pela tarefa. (`app/templates/folha/_retrato_o_que_fazer.html:33`, `app/api/views.py:993`, `app/api/views.py:2533`)

**4. Vocabulário de rotas**

**NAO PROCEDENTE — falta de `porta_acao` no vocabulário executável alterado.** Ela consta no publicador, no contador do furo, na inicialização e impressão do vigia, no command e na soma contratual do alcance. (`app/chamados/services/tarefa_do_dia.py:621`, `app/chamados/services/furo_sem_canal.py:117`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:730`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:39`, `app/chamados/tests/test_o261e_f5_alcance.py:124`)

**NAO PROCEDENTE — exigir `porta_acao` em `PORTA_DO_FIO`.** Esse mapa enumera fios gravados, não rotas da tarefa. Admin tem `None` explícito; seu botão depende do tipo. Assim, publicar `porta_acao` não garante que o drawer ofereça um botão específico para aquela ação. (`app/folha/retrato_folha.py:175`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:986`, `app/folha/retrato_folha.py:1015`)

**PROCEDENTE — contratos antigos permanecem incompatíveis.** Há testes esperando colab sem fornecer pendentes do app e outro esperando que `cobrar` residual prevaleça sobre validar/decidir. Essas expectativas conflitam com os predicados atuais. (`app/chamados/tests/test_e3_portas_tarefa.py:409`, `app/chamados/tests/test_e3_portas_tarefa.py:451`, `app/chamados/tests/test_e3_portas_tarefa.py:473`, `app/chamados/tests/test_e3_portas_tarefa.py:485`, `app/chamados/tests/test_e3_portas_tarefa.py:503`, `app/chamados/services/tarefa_do_dia.py:598`, `app/chamados/services/tarefa_do_dia.py:606`)

**5. ESMERIL-SEMANTICA: `None` versus existência de chamado**

**PROCEDENTE na saída detalhada da Folha.** A tarefa conserva `fila/verbos` mesmo com rota `None`; a projeção elimina tudo; `o_que_segura` transforma a projeção ausente em fila vazia. É uma inferência operacional de “sem porta” para “nenhum chamado a publicar”. Quebra de link numa tela consumidora desse campo: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`, `app/folha/fora_da_folha.py:244`)

**NAO PROCEDENTE no vigia, beco e furo como pressuposição de inexistência de chamado.** O vigia continua examinando chamados vivos; o beco examina perguntas e âncoras; o furo mede presença de porta publicada, sem testar inexistência de chamados. (`app/chamados/services/vigia_fio.py:681`, `app/chamados/services/vigia_fio.py:683`, `app/escala/services/cadastro_realidade.py:471`, `app/escala/services/cadastro_realidade.py:474`, `app/chamados/services/furo_sem_canal.py:104`)

**6. Classes de reprovação**

| Classe | Avaliação |
|---|---|
| Segundo juiz duplicando autoridade registrada | **NAO PROCEDENTE como clone novo nesta publicação.** O publicador continua dentro da tarefa registrada; C17 permanece isolado como comparação histórica. (`app/core/juizes.py:297`, `app/core/juizes.py:299`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |
| Segundo escritor fora da porta | **PROCEDENTE quanto à existência; NAO PROCEDENTE quanto à contaminação dos leitores.** É o C17 descrito no item 2. (`app/chamados/juizes.py:1709`, `app/core/juizes.py:136`) |
| Relógio fora da borda | **NAO PROCEDENTE no caminho alterado.** A carga repassa o mesmo `agora` à fila e ao app. Isso não equivale a certificar todos os caminhos legados do repositório. (`app/chamados/services/tarefa_do_dia.py:549`, `app/chamados/services/tarefa_do_dia.py:556`, `app/chamados/services/vigia_fio.py:330`) |
| Derivação paralela | **PROCEDENTE como classificação adicional do vigia**, com os limites do item 3; a Folha lê a tarefa e tem selo AST restringindo suas dependências de chamados. (`app/chamados/services/vigia_fio.py:672`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:53`) |
| Ordem de trava | **NAO PROCEDENTE como inversão demonstrada pelo trecho publicado.** A tarefa recebe o julgamento da tranca antes da publicação dos campos aditivos. Travas operacionais da entrega: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:544`, `app/chamados/services/tarefa_do_dia.py:557`) |
| Pouso antes do pré-requisito | **[nao verificado].** A cópia registra condições de pouso e um relato anterior de parada; isso não comprova a execução dos pré-requisitos finais nem um pouso posterior. (`app/docs/CORTES.md:401`, `app/docs/RELATO.md:4`) |
| Campo com dois significados | **PROCEDENTE como inconsistência do contrato documentado:** `None = sem dono`, mas o código e C13 aceitam dono sistema com `None`. Além disso, fila existente deixa de implicar projeção existente. (`app/chamados/services/tarefa_do_dia.py:40`, `app/chamados/services/tarefa_do_dia.py:621`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`) |

**NAO PROCEDENTE — C11/C12, contador e vigia sem mordida nos alvos específicos.** C11 verifica publicação/projeção/Folha; C12 verifica nada e nao_sei sem rota; o contador verifica ação direta, residual cobrar e soma; o vigia verifica contador, papel e texto. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:92`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:103`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:151`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:256`)

**PROCEDENTE — C13 não prova o alcance nem o “sse” geral.** Compara dois resultados do próprio juiz em seis exemplos e depois exige a exceção sistema/None. Não há oráculo independente de ação alcançável. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:115`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:122`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`)

Os mutantes novos exigem falha de assert sem erros/skips, mas não fazem controle verde antes de cada mutação; o executor v6 faz esse controle e recusa carimbar um teste já vermelho. (`app/chamados/tests/e3_degrau_mutantes.py:77`, `app/chamados/tests/e3_degrau_mutantes.py:89`, `app/chamados/tests/e3_portas_v6_mutantes.py:92`)

Execuções em memória reportaram: degrau **27 OK**, mutantes novos **22/22 mortos**, v6 sem banco **10 OK**; bateria antiga **26 testes, 6 falhas e 1 erro**, e mutantes v6 interrompidos antes do terceiro mutante por controle já vermelho. Saída persistida em arquivo: **[nao verificado]**. A incompatibilidade é localizável nas expectativas antigas acima e no helper que cria a carga com `__new__` sem inicializar `_pendentes_app`. (`app/chamados/tests/e3_v8_apoio.py:127`, `app/chamados/tests/e3_v8_apoio.py:146`, `app/chamados/services/tarefa_do_dia.py:551`)

**7. Contraexemplo à lei 16:16**

**Dia real contraditório: [nao verificado].** Não transformo comentários “33 dias”, “31 dias” ou “0 dias” dos testes em linhas observadas de produção. Esses números aparecem como comentários, sem os respectivos insumos nominais no dado fornecido. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:94`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:123`)

- **Sistema sem chamado — PROCEDENTE como contraexemplo ao contrato universal de C13:** o corpo real pode produzir `perguntar/sistema/emissor`; o publicador escolhe sistema e devolve rota `None`. Isso prova a exceção no código, mas não prova um dia real nem uma porta humana alcançável contrariando a ordem colab > admin. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:602`, `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/services/tarefa_do_dia.py:621`)
- **Colab com fila somente `cobrar` — NAO PROCEDENTE, por si só, como contradição:** se há responder exigível e pergunta pendente no app, existe porta do colab. A rota fica `porta_fila` pela classificação da fila, embora o alcance do dono tenha sido estabelecido pelo app. (`app/chamados/services/tarefa_do_dia.py:598`, `app/chamados/services/tarefa_do_dia.py:604`, `app/chamados/services/tarefa_do_dia.py:619`)
- **Ação direta admin mais residual cobrar — lacuna concreta de cobertura:** `decidir_ausencia/admin/None` recebe dono sem fila, mas perde-o quando existe apenas residual cobrar, pois `not portas` deixa de valer e o residual é excluído. Alcance efetivo dessa ação na tela e ocorrência real: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:303`, `app/chamados/services/tarefa_do_dia.py:603`, `app/chamados/services/tarefa_do_dia.py:606`)
- **Entregar documento pelo colab — outra exclusão explícita:** a tarefa produz essa ação, mas o degrau colab aceita somente `responder`; C10 exige nada/None nesse caso. A existência de caminho alcançável para entregar o documento, num dia real fornecido, permanece **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:300`, `app/chamados/services/tarefa_do_dia.py:599`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:87`)

## 14/11 16:51 · sessao c28e1f3a7ed3 · - · - · origem=Ronald (tmux)

- **PROCEDENTE — C13 não sela o “sse” anunciado.** A propriedade compara `rota` com `fio`, sem testar alcance independente; os casos omitem `sistema`, e depois o teste exige expressamente `('sistema', None)`. Portanto aceita a exceção que a descrição universal nega. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `:115`, `:122`, `:123`, `:125`. A publicação produz essa exceção por `.get(fio)` sem entrada `sistema`. `app/chamados/services/tarefa_do_dia.py:602`, `:610`, `:621`. Dia real dessa classe: **[nao verificado]**.

- **NAO PROCEDENTE — novos C11/C12 e mutantes específicos sem mordida.** C11 verifica publicação, projeção e leitura pela Folha; C12 verifica os dois estados sem dono e retorno `None`; o contador verifica ação direta e residual cobrar, inclusive soma do universo. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:92`, `:103`, `:151`. Vigia verifica contador, papel e texto publicado; command verifica ambos os números. `:231`, `:256`. Executei `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_degrau_dono_e_porta`: **27 OK**; `python3 -m chamados.tests.e3_degrau_mutantes --todos`: **22/22 mortos por assert, zero erros, RC0**. O executor exige failures sem errors/skips. `app/chamados/tests/e3_degrau_mutantes.py:72`, `:77`, `:82`, `:106`.

- **PROCEDENTE — bateria antiga não acompanha o contrato atual.** `python3 -m chamados.tests.e3_portas_regra`: **26 testes, 6 failures e 1 error, RC2**. Failures: [nao verificado]
  - `ResidualComAcaoTest.test_RED_A_na_fila_aparece_com_acao_sob_B`. `app/chamados/tests/test_e3_portas_tarefa.py:409`.
  - `ContraprovasPortasTest.test_cobrar_precede_validar_e_decidir_sem_depender_dos_donos`. `:473`.
  - `ContraprovasPortasTest.test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar`. `:451`.
  - `MatrizV8Test.test_v8_R1_colab_alcancavel_e_leitores`. `:485`.
  - `MatrizV8Test.test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia`, dois subtests `porta_colab=101/202`. `:503`.
  
  Esses casos esperam colab sem fornecer `pendentes_app`, cujo default é vazio e agora participa da atribuição. `app/chamados/services/tarefa_do_dia.py:564`, `:598`. Error: `MatrizV8Test.test_v8_R10_ancora_fora_do_fato_sem_acao`; o helper instancia com `__new__`, sem `_pendentes_app`, acessado pela carga. `app/chamados/tests/test_e3_portas_tarefa.py:593`, `app/chamados/tests/e3_v8_apoio.py:127`, `:146`, `app/chamados/services/tarefa_do_dia.py:551`.

- **PROCEDENTE — execução completa dos mutantes v6 interrompida por baseline vermelho.** `python3 -m chamados.tests.e3_portas_v6_mutantes --mutante todos` mata os dois primeiros e aborta em `residual_A_some` **antes da mutação**, porque seu teste já falha. O executor recusa carimbar isso como morto. `app/chamados/tests/e3_portas_v6_mutantes.py:30`, `:90`, `:92`, `:98`. Separadamente, `python3 -m unittest chamados.tests.test_e3_portas_v6_sem_banco`: **10 OK**.

- **Contraexemplos sintéticos confirmados, ocorrência real [nao verificado]:**
  - `perguntar/sistema/emissor`, fila vazia → `fio=sistema, rota=None`. O corpo real produz ações desse tipo. `app/chamados/services/tarefa_do_dia.py:312`, `:316`, `:621`.
  - Colab com responder pendente no app, única fila `cobrar` → `fio=colab, rota=porta_fila, fila=(101,), verbos={101:'cobrar'}`. Há dono alcançável pelo app; o nome da rota é determinado pela existência da fila, sem testar se ela conduz à ação vencedora. `:598`, `:604`, `:618`, `:619`.
  - `entregar_documento/colaborador/None`, fila vazia → `nada/None`: a ação existe no corpo real, mas o filtro de colab aceita somente responder. `:298`, `:300`, `:598`, `:599`; o teste C10 exige precisamente esse resultado. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:87`.
  - `decidir_ausencia/admin/None` com residual cobrar → `nada/None`; sem fila → `admin/porta_acao`. A presença do residual bloqueia a ação direta pelo predicado de alcance; esse predicado antecede o diff de publicação. `app/chamados/services/tarefa_do_dia.py:302`, `:303`, `:602`, `:603`, `:606`, `:621`.

- **Pouso/pré-requisitos: [nao verificado].** A cópia encerra `CORTES.md` no aval de 15:37 e o topo do relato mantém “PAREI” com condições anteriores; não comprova matriz final conferida, remedida final, suite integral ou pouso. `app/docs/CORTES.md:401`, `app/docs/RELATO.md:4`. O anexo fornecido atualiza a lei aplicável; a documentação da cópia não prova sua execução.

## 14/11 16:51 · sessao d54d687beb51 · - · - · origem=Ronald (tmux)

- **PROCEDENTE: há segundo escritor literal de `rota`, preservado como comparação histórica.** C17 devolve `porta_fila/porta_pergunta` e fio próprio em `app/chamados/juizes.py:1709`. O único chamador encontrado em produção é o lado `atual` de `diff_e3_sombra`; a candidata vem separadamente da tarefa, e as assinaturas distinguem `C17`/`candidata`: `app/chamados/management/commands/diff_e3_sombra.py:127`, `:129`, `:141`. **NAO PROCEDENTE: contaminação dos leitores atuais pelo C17.** O registro declara essa fronteira em `app/core/juizes.py:132`; o selo exige um único chamador nomeado em `app/chamados/tests/test_e3_pendentes_app.py:145`.

- **NAO PROCEDENTE: vocabulário runtime esqueceu `porta_acao`.** Está no publicador `app/chamados/services/tarefa_do_dia.py:621`, no contador furo `app/chamados/services/furo_sem_canal.py:117`, na inicialização e impressão do vigia `app/chamados/services/vigia_fio.py:577`, `:730`, e na soma contratual `app/chamados/tests/test_o261e_f5_alcance.py:124`. O C17 mantém suas duas rotas por contrato histórico `app/chamados/tests/test_e3_pendentes_app.py:171`. `PORTA_DO_FIO` enumera fios, não rotas; adicionar `porta_acao` ali não corresponde à chave desse mapa: `app/folha/retrato_folha.py:974`.

- **NAO PROCEDENTE no caminho alterado: relógio novo fora da borda.** A carga repassa `self.agora` tanto à alimentação quanto ao app em `app/chamados/services/tarefa_do_dia.py:549`, `:556`; a alimentação repassa-o ao painel e à partição em `app/chamados/juizes.py:1570`, `:1573`. Defaults internos de relógio existem, mas são condicionais à ausência de `agora`: `app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:190`.

- **PROCEDENTE: C13 não sela a propriedade universal anunciada.** O laço verifica apenas seis exemplos, e depois afirma explicitamente o caso `('sistema', None)`: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `:125`. Ações de sistema sem chamado podem nascer do corpo real em `app/chamados/services/tarefa_do_dia.py:312`, `:316`; sistema recebe dono por `:601`, mas não rota por `:621`. Dia nominal de produção dessa classe: **[nao verificado]**.

- **Possível rótulo incorreto já identificável por código:** `decidir_ausencia` é ação admin em `app/chamados/services/tarefa_do_dia.py:303`; pode publicar `porta_acao` por `:621`, enquanto a descrição admin da Folha fala apenas em validar/recusar o que o painel põe em Validar: `app/folha/fora_da_folha.py:46`. Ocorrência nominal de produção: **[nao verificado]**.

Verificação executada sem bytecode: CatracaC17Test passou 7 testes; selo AST da Folha passou 3 testes por extração dos corpos; suíte degrau passou 27 testes. Corpos respectivos: `app/chamados/tests/test_e3_pendentes_app.py:118`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:52`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`; saída persistida em arquivo: **[nao verificado]**.

## 14/11 16:50 · sessao 89574cb0d293 · - · - · origem=Ronald (tmux)

Inventário de leitores em código executável:

| Leitor | Campos/efeito de `porta_acao` | Efeito de `None` com fila residual |
|---|---|---|
| `folha/fora_da_folha.py:_porta/_fio_da_porta` | Lê a projeção e usa `fio='admin'`; a rota não indexa mapa local. `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:82` | Converte projeção ausente em `FIO_SEM_PORTA`; cai no fio `nada`, grupo D e contador `dias_por_fio['nada']`. `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:195` |
| `folha/fora_da_folha.py:o_que_segura` | Usa fio admin e rótulo fixo de validar. `app/folha/fora_da_folha.py:239`, `app/folha/fora_da_folha.py:245`, `app/folha/fora_da_folha.py:46` | **Perde a fila preservada na tarefa:** lê fila exclusivamente da projeção; `None` produz `chamado=[]`. `app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:247` |
| `furo_sem_canal.medir` | Inclui em `com_porta_acao`, retirando de `total`; partição contém as quatro rotas. `app/chamados/services/furo_sem_canal.py:97`, `app/chamados/services/furo_sem_canal.py:104`, `app/chamados/services/furo_sem_canal.py:114` | Vai para `sem_canal` mesmo que haja chamado na tarefa. `app/chamados/services/furo_sem_canal.py:98`, `app/chamados/services/furo_sem_canal.py:104` |
| `vigia_fio.censo_alcance` | Chave dinâmica existe; soma `alcance_porta_acao`, registra papel admin e publica log. `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:730` | Vai para teto temporal, quatro baldes legais ou causas a–e; chamado vivo ainda é consultado independentemente da projeção. `app/chamados/services/vigia_fio.py:666`, `app/chamados/services/vigia_fio.py:672`, `app/chamados/services/vigia_fio.py:683` |
| `cadastro_realidade.beco_em_lote` | Presença da projeção exclui do beco. `app/escala/services/cadastro_realidade.py:469` | Pode entrar na investigação do beco, mas continua exigindo perguntas vivas e considera suas âncoras; não pressupõe ausência de chamados. `app/escala/services/cadastro_realidade.py:468`, `app/escala/services/cadastro_realidade.py:471`, `app/escala/services/cadastro_realidade.py:474` |
| `pautas_dos_discordantes.dias_sem_fio` | Usa `_fio_do_dia`; admin é excluído. `app/ponto/services/pautas_dos_discordantes.py:64`, `app/ponto/services/pautas_dos_discordantes.py:81` | Discordante com residual cobrar passa a ser elegível ao universo de pauta, pois o critério é fio sem porta. `app/ponto/services/pautas_dos_discordantes.py:72`, `app/ponto/services/pautas_dos_discordantes.py:81` |
| `diff_e3_sombra` | Lê todos os quatro campos, sem vocabulário fechado dos valores; compara C17/candidata separadamente. `app/chamados/management/commands/diff_e3_sombra.py:26`, `app/chamados/management/commands/diff_e3_sombra.py:46`, `app/chamados/management/commands/diff_e3_sombra.py:141` | Projeta fio nada, fila vazia, sem porta/beco/pauta; isso reflete suas próprias projeções, sem executar os consumidores. `app/chamados/management/commands/diff_e3_sombra.py:47`, `app/chamados/management/commands/diff_e3_sombra.py:49` |
| `lavrar_furo_sem_canal` | Imprime contador novo e grava todo detalhe no snapshot quando aplicado. `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50` | Imprime/grava o `total` calculado pelo serviço. `app/chamados/management/commands/lavrar_furo_sem_canal.py:33`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:52` |

Achados:

- **PROCEDENTE — outro escritor de rota existe:** C17 produz sempre `porta_fila/porta_pergunta` quando possui verbos e deriva fio dos verbos; pode divergir da tarefa nos residuais cobrar. Seu único chamador executável encontrado fora dos testes é o comparador, que distingue explicitamente C17 da candidata. Não constitui consumidor confundindo as fontes. `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1709`, `app/chamados/management/commands/diff_e3_sombra.py:126`, `app/core/juizes.py:136`.

- **PROCEDENTE — perda do dado de chamado no retorno Folha:** `porta_da_tarefa(None)` elimina a estrutura inteira, e `o_que_segura` tira `fila` dessa projeção, não da tarefa. Em residual cobrar, retorna `chamado=[]` apesar da fila original. Link quebrado numa tela consumidora desse campo: **[nao verificado]**. `app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`, `app/folha/fora_da_folha.py:244`.

- **NÃO PROCEDENTE — `None` implica nenhum chamado no vigia/beco/furo:** vigia consulta os vivos da célula separadamente e distingue seu véu; beco consulta perguntas e âncoras separadamente; furo só pergunta presença de porta, sem afirmar inexistência de chamado. `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:681`, `app/escala/services/cadastro_realidade.py:468`, `app/chamados/services/furo_sem_canal.py:104`.

- **NÃO PROCEDENTE — `PORTA_DO_FIO` precisa enumerar `porta_acao`:** esse mapa enumera fios gravados, não rotas da tarefa. Admin possui `None` explícito e seu botão depende do tipo; o mapa não recebe `porta['rota']`. `app/folha/retrato_folha.py:175`, `app/folha/retrato_folha.py:974`, `app/folha/retrato_folha.py:1015`.

- **PROCEDENTE como limitação preexistente de apresentação:** novo `porta_acao` chega à Folha como admin, mas o texto admin promete validar/recusar o que o painel põe em Validar; o botão por fio admin é `None`. O drawer pode ficar sem botão conforme o tipo, embora o juiz publique porta. Não há evidência de dia real nessa combinação. `app/folha/fora_da_folha.py:46`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, **[nao verificado]**.

- **PROCEDENTE — regra de dono fora da rota publicada existe no vigia:** após projeção None, `dono_fora_do_fio` compõe Resolver dia, ausência, folga validada e lista do admin; são quatro baldes próprios, não republicação da rota. O nome dos baldes e a ordem estão materializados no código. `app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:672`.

- **NÃO PROCEDENTE — retrato recalcula dono por ações/verbos:** recebe o grupo de `fora_da_folha`, grava fio por tradução A/B/C/D, e a UI agrupa chamado/ninguém em admin e trata hoje. Há regra de apresentação, não nova leitura de ações alcançáveis. `app/folha/retrato_folha.py:177`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:368`, `app/folha/retrato_folha.py:477`, `app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`.

- **NÃO PROCEDENTE — templates Folha derivam rota:** o partial imprime linhas e botões já produzidos pelo Python. API consulta o juiz do app diretamente para sua própria lista; não lê a rota da tarefa. `app/templates/folha/_retrato_o_que_fazer.html:33`, `app/api/views.py:993`, `app/api/views.py:2533`.

Limite: buscas foram com `rg`, pois a cópia não contém `.git`; identidade efetiva do hash entregue não foi verificável via Git. **[nao verificado]**

## 14/11 16:39 · sessao 5f078705d999 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE — o período do motor não chega ao abono gravado.**

O motor soma `minutos_justificados`, mas o fechamento grava `minutos_abonados` por outra derivação: `por_dia_da_grade`. Ela só abona dias classificados como `ausencia`, por `previsto − realizado`; ignora `min_justificados`. A justificativa mantém o dia como `trabalho`. (`app/ponto/motor_calculo_v2.py:2633`; `app/ponto/services/fechamento.py:1065`; `app/ponto/services/dia_pago.py:395`; `app/escala/utils.py:1257`)

Contraexemplo sintético: dia `trabalho`, previsto 720, realizado 0, `min_justificados=690`: a função real `por_dia_da_grade`, executada isoladamente em memória, devolve **abono vazio**. Esse mapa alimenta tanto o fechamento quanto o `DiaPago`. (`app/ponto/services/dia_pago.py:387`; `app/ponto/services/fechamento.py:1067`; `app/ponto/services/fechamento.py:1217`)

Na competência, o resumo do PDF/espelho recebe `minutos_abonados/60` do fechamento ou da lavratura, sobrescrevendo o número apurado pelo motor. Portanto, podem existir **11,5 h no motor e 0 h no resumo**. (`app/relatorios/pdf_espelho.py:480`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:186`; `app/relatorios/cartao_pela_celula.py:346`)

| Campo | Quem escreve/alimenta | Quem lê |
|---|---|---|
| `minutos_justificados` | Inicialização: `app/ponto/motor_calculo_v2.py:813`; fechamento normal/órfão: `app/ponto/services/fechamento.py:755`, `:831`; espelho: `app/ponto/services/espelho.py:1134`; PDF: `app/relatorios/pdf_espelho.py:407` | Conta mensal: `app/ponto/motor_calculo_v2.py:2628`; contrato de memo: `app/ponto/calculador/nucleo.py:424` |
| `min_justificados` | Efeito: `app/ponto/services/efeito_ausencia.py:47`, `:67`; grade: `app/escala/utils.py:1276`; fechamento monta mapa equivalente: `app/ponto/services/fechamento.py:667` | Alimentação do espelho/PDF: `app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`. A derivação do abono gravado não lê essa chave: `app/ponto/services/dia_pago.py:387` |
| `dias_com_ausencia_inteira` | Fonte: `app/ponto/turnos.py:2829`; inicialização: `app/ponto/motor_calculo_v2.py:808`; fechamento: `app/ponto/services/fechamento.py:754`, `:830`; espelho/PDF: `app/ponto/services/espelho.py:1133`; `app/relatorios/pdf_espelho.py:406` | Teto de pontualidade: `app/ponto/motor_calculo_v2.py:1090`; calculador: `app/ponto/calculador/regras.py:120`, `app/ponto/calculador/alimentacao.py:532`; envio ao oráculo: `app/ponto/services/fechamento.py:1281` |
| `horas_abono` | Inicialização: `app/ponto/motor_calculo_v2.py:292`; conta: `:2631`, `:2633`; composição de fatias: `app/ponto/services/fechamento.py:99` | PDF: `app/relatorios/pdf_espelho.py:480`; resumo substitui pela outra fonte: `app/relatorios/cartao_pela_celula.py:118`, `:186` |
| `dias_abono` | Inicialização: `app/ponto/motor_calculo_v2.py:284`; conta: `:2630`; composição: `app/ponto/services/fechamento.py:99`; resumos: `app/ponto/services/espelho.py:1253`, `app/relatorios/pdf_espelho.py:479` | Badge PDF: `app/relatorios/pdf_espelho.py:787`; ramo de lavratura substitui por contagem de dias com abono positivo: `app/relatorios/cartao_pela_celula.py:190` |
| `resumo_dias_abono` | Não localizado pela busca literal na cópia: [nao verificado] | [nao verificado] |

**PROCEDENTE — chamador de sombra ficou sem alimentação.** `diff_espelho_alimentacao` passa justificadas e ausência inteira, mas não passa o mapa novo, inclusive no degrau denominado “cura completa”. O memo permanece vazio: aplica o comportamento anterior — previsto inteiro quando o dia entra em `_abono`, nenhum crédito parcial quando não entra. (`app/ponto/management/commands/diff_espelho_alimentacao.py:41`; `:120`; `app/ponto/motor_calculo_v2.py:813`; `:2611`; `:2628`)

Outros caminhos fora do diff:

- `diff_extrato_parcial` consulta justificativas diretamente e calcula sem o memo novo; `esmeril_espelho` também não o alimenta, mas usa o resultado para assinaturas de pontualidade/HE. Divergência de **pagamento** nesses dois caminhos: [nao verificado]. (`app/relatorios/management/commands/diff_extrato_parcial.py:42`; `:67`; `app/ponto/services/esmeril_espelho.py:160`; `:170`)
- TXT Domínio monta eventos sem ler `horas_abono` ou o mapa novo; a rubrica 8932 deriva de tipos de `Ausencia`. Igualdade entre período justificado e pagamento no Domínio: [nao verificado]. (`app/folha/export.py:439`; `app/folha/export.py:201`)
- Prontidão lê `minutos_abonados`; seu fallback ainda soma o previsto inteiro dos dias de ausência, constituindo outra derivação de abono. (`app/folha/export.py:1078`; `app/folha/export.py:1100`)
- Tela e app recebem o resumo do espelho; Haiku consulta classificação de exportação e cópias dos TXT. O holerite é recebido por upload. Esses caminhos não comprovam que o período novo foi pago. (`app/ponto/views.py:161`; `app/api/views.py:1354`; `app/api/views_mensageria.py:283`; `app/folha/haiku_export.py:79`; `app/holerite/models.py:6`; pagamento efetivo [nao verificado])

2. **PROCEDENTE — a fonte perde a segunda pausa.**

A fonte desempacota quatro marcos e reconstrói apenas `hi/hii/hfi/hf`. O medidor desconta somente pausas presentes nesse mapa. O previsto, entretanto, consulta e desconta todas as pausas do dia. (`app/ponto/services/efeito_ausencia.py:156`; `app/ponto/services/efeito_ausencia.py:163`; `app/ponto/turnos.py:2786`; `app/escala/utils.py:2310`; `app/escala/utils.py:2324`)

Contraexemplo confirmado em memória: jornada 07–19, pausas 10–11:25 e 14–15:25, entrada 07:00 e saída justificada: **635 minutos pela cadeia construída pela fonte; 550 minutos com ambas as pausas**. O motor aceita o valor sem teto pelo previsto. (`app/ponto/services/efeito_ausencia.py:158`; `app/ponto/turnos.py:2791`; `app/ponto/motor_calculo_v2.py:2633`)

Quanto às demais perguntas semânticas:

- A chave é **dia do turno**, `Justificativa.data_turno`; a fonte recusa usar a âncora de turno com outra data. A cadeia encadeia a saída noturna no dia seguinte. (`app/ponto/services/efeito_ausencia.py:107`; `:165`; `:163`; `app/escala/utils.py:345`)
- O Base compara pelo dia da jornada, mas o Comercial refaz DSR pela **data civil da entrada**. Essa derivação paralela preexistente continua alcançável quando a justificativa deixa de integrar `datas_justificadas`. (`app/ponto/motor_calculo_v2.py:2581`; `:3473`; `:3477`; `:3490`)
- Os caminhos identificados convertem minutos para horas por `/60`; não encontrei erro de fator nesses sítios. (`app/ponto/motor_calculo_v2.py:2634`; `app/relatorios/pdf_espelho.py:480`; `app/relatorios/cartao_pela_celula.py:118`)
- A seleção de dias inclui as duas datas; a duração é diferença entre instantes, sem minuto adicional; toque entre extremidades não configura sobreposição, pois o teste usa `<`. (`app/ponto/services/efeito_ausencia.py:100`; `app/ponto/turnos.py:2764`; `app/ponto/turnos.py:2783`)

3. **PROCEDENTE — duas âncoras do mesmo turno podem pagar a mesma cauda duas vezes.**

A agregação une os rótulos cobertos, mas **soma os minutos**, sem deduplicar trechos. Ambas as justificativas usam a última entrada do turno para medir a saída coberta. (`app/ponto/services/efeito_ausencia.py:133`; `:134`; `:179`)

Contraexemplo: turno `E07:30/S11:00/E12:00`, fim previsto 19:00; uma justificativa aprovada ancorada na primeira entrada e outra na segunda: ambas medem 12–19, produzindo **420 + 420 = 840 minutos**. A unicidade impede repetir a mesma âncora, mas permite âncoras distintas; a unicidade por dia restringe apenas pendentes. (`app/ponto/services/efeito_ausencia.py:168`; `:179`; `app/ponto/turnos.py:2738`; `app/ponto/models.py:187`; `:189`)

O motor soma esses 840 sem deduplicação nem limite pelo previsto. Ocorrência produtiva: [nao verificado]. (`app/ponto/motor_calculo_v2.py:2633`)

Um dia em `_mj` fora de `_abono` por ter trabalho **não desaparece**: a soma nova independe de `_abono`. Já dia fora dos obrigatórios — inclusive folga corretamente excluída dos previstos — não recebe esse crédito. (`app/ponto/motor_calculo_v2.py:2611`; `:2601`; `:2634`)

**PROCEDENTE — precedência incompatível com ausência inteira.** O efeito mantém o mapa parcial mesmo quando uma ausência `ABONA` cobre o mesmo dia; a grade dá precedência à ausência, enquanto o motor continua usando `_mj` para excluir o abono inteiro. (`app/ponto/services/efeito_ausencia.py:47`; `:61`; `app/ponto/services/fechamento.py:667`; `:700`; `app/escala/utils.py:1207`; `app/ponto/motor_calculo_v2.py:2629`)

A troca não altera diretamente o predicado de DSR do Base; altera sua alimentação de justificadas. O Comercial conserva a rederivação por data civil descrita no item 2. **Zero falta inteira adicional e zero semana perdida no universo informado: [nao verificado].** (`app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2605`; `:3477`)

4. **nada quanto a demonstrar que os dez dias viraram falta inteira.**

Sem âncora, papel errado e turno fechado retornam motivo de conservação; célula sem marcos também conserva. A fonte transforma esses motivos em `cobre=None`. (`app/ponto/turnos.py:2694`; `:2700`; `:2703`; `app/ponto/services/efeito_ausencia.py:154`; `:129`)

O caminho para **vazio sem `conserva`** vem depois: o juiz cobre o marco, o filtro encontra cruzamento com um turno fechado e remove a cobertura; a função retorna vazio com motivo `None`. O efeito então exclui o dia tanto de `justificadas` quanto de `min_justificados`. (`app/ponto/services/efeito_ausencia.py:176`; `:182`; `:183`; `app/ponto/turnos.py:2764`; `app/ponto/services/efeito_ausencia.py:46`)

Esse mecanismo tem caso sintético e referência a col451 no teste; a reprodução individual de col206 31/08 e col551 27/10 não está demonstrada pelo dado fornecido. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:191`; `:383`; demais resultados [nao verificado])

Com vazio, o dia recebe **nenhum crédito da justificativa**; conserva o trabalho que o motor calcular. Vazio não implica automaticamente falta: o Base inclui períodos abertos em `datas_trabalho`, sem exigir minutos positivos. Os valores individuais dos dez dias permanecem [nao verificado]. (`app/ponto/motor_calculo_v2.py:2499`; `:2581`; `:2605`; `:2634`)

5. **nada de falha nova de relógio demonstrada.**

A fonte usa datas explícitas, ordenação por `data_turno, pk` e timestamps explícitos nas conversões locais. O previsto consulta escala, marcos e pausas, sem depender das batidas nesse cálculo. (`app/ponto/services/efeito_ausencia.py:98`; `:122`; `:179`; `app/escala/utils.py:2288`; `:2310`)

Os testes repetem o juiz puro e a consulta da fonte; não demonstram duas execuções completas do fechamento. **Idempotência integral do recálculo: [nao verificado].** (`app/ponto/tests/test_justificativa_paga_o_periodo.py:134`; `:320`)

6. **PROCEDENTE — há mordida localizada, mas falta prova do percurso até o gravado.**

O teste principal exige `horas_abono=11.5`; essa asserção distingue a conta nova da antiga. O controle de dia inteiro passaria com o motor velho, pois exige apenas um dia abonado e horas positivas. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:255`; `:258`; `:262`; `:265`)

Os testes da fonte conferem seus próprios minutos e a grade; as propriedades somam trabalho com crédito da fonte, sem conferir o abono materializado que apresenta a falha do item 1. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:320`; `:325`; `:338`; `:350`; `app/ponto/services/fechamento.py:1067`)

- `test_cobparc_marco_no_periodo.py`: **só atualiza a fixture**, acrescentando mock da fonte sem banco; mantém asserções de dispensa, ausência de hora fabricada e preservação da batida. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:60`; `:75`)
- `test_s5b_porta_unica.py`: **só amplia lista e fixture** com o memo; preserva o teste que remove cada atributo e exige exceção nominal. Contudo, aceita `{}` e não detecta o chamador que esqueceu de alimentar o mapa. (`app/ponto/tests/test_s5b_porta_unica.py:989`; `:1000`; `:1006`; `:1014`)
- O caso de papel errado é sintético; reprodução dos três dias produtivos exigidos no aval: [nao verificado]. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:429`)
- RED histórico e suíte verde efetivamente executada: [nao verificado]. [nao verificado]

7. **PROCEDENTE — derivação paralela, selo insuficiente e campo com dois significados.**

A cadeia manual de quatro marcos duplica uma composição já existente que recebe todas as pausas, causando os 85 minutos indevidos do item 2. (`app/ponto/services/efeito_ausencia.py:156`; `app/escala/utils.py:353`; `app/escala/utils.py:363`)

`dias_abono` também possui dois significados: no motor, exclui todo dia em `_mj`; no resumo da lavratura, conta qualquer dia com minutos abonados positivos. (`app/ponto/motor_calculo_v2.py:2629`; `app/ponto/services/dia_pago.py:698`; `app/relatorios/cartao_pela_celula.py:190`)

A cobertura ainda muda **depois** da resposta do juiz anunciado, pelo filtro de cruzamento. Isso demonstra decisão adicional de cobertura; duplicação de um juiz **já registrado**, especificamente: [nao verificado]. (`app/ponto/services/efeito_ausencia.py:176`; `:182`; `app/ponto/turnos.py:2751`; `app/core/juizes.py:207`; `:251`)

Segundo escritor persistente novo, relógio novo fora da borda ou toque adicional no motor além da inicialização e do bloco de abono apresentado: **nada demonstrado**. A falha comprovada é que essa leitura nova permanece desconectada do abono gravado. (`app/ponto/motor_calculo_v2.py:813`; `:2628`; `app/ponto/services/fechamento.py:1067`)

DIFF produtivo 0/0, hashes iguais dos TXT exportados e aplicação restrita à competência 11: **[nao verificado]**.

## 14/11 16:36 · sessao 2224a6af9587 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

Cura implementada no juiz existente: COLAB exige `responder` ligado a pergunta pendente no app; `cobrar` residual não atribui dono; dia com porta apenas no app publica `porta_app`. A ordem permanece colab → admin → chamado → sistema. ([tarefa_do_dia.py:607](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/services/tarefa_do_dia.py:607))

Diff resumido:

- `tarefa_do_dia.py::CargaDaTarefa`: consulta do app uma vez por colaborador, memorizada na carga, com `self.agora`; `_portas_da_tarefa` decide o dono; `porta_da_tarefa` projeta a nova rota. (`chamados/services/tarefa_do_dia.py:389`, `:556`, `:573`, `:626`)
- `pergunta_no_app.py::consulta_do_app`: recebe `agora=None` e `em_lote=True`; conserva o predicado existente. (`chamados/services/pergunta_no_app.py:64`)
- `furo_sem_canal.py::medir`: acrescenta `com_porta_app`. (`chamados/services/furo_sem_canal.py:116`)
- `vigia_fio.py::censo_alcance/linhas_alcance`: acrescenta contador e saída de `porta_app`. (`chamados/services/vigia_fio.py:577`, `:730`)
- `lavrar_furo_sem_canal.py::handle`: exibe o contador do app. (`chamados/management/commands/lavrar_furo_sem_canal.py:37`)
- `fora_da_folha.py::DESTRAVA`: explicita “SEM PORTA” para `nada`, inclusive quando existe chamado residual na fila. (`folha/fora_da_folha.py:54`)
- Testes anteriores receberam a nova alimentação; alvos literais dos mutantes foram atualizados. (`chamados/tests/test_e3_portas_v6_sem_banco.py:19`, `test_e3_c8b_sem_banco.py:224`, `e3_portas_apoio.py:121`, `e3_volta7_apoio.py:146`, `e3_mutantes.py:34`, `e3_portas_v6_mutantes.py:58`)

Quem escreve e quem lê os campos:

| Campo | Escritor | Leitores |
|---|---|---|
| `rota`, `fila`, `verbos`, `fio` publicados | `_portas_da_tarefa`, `chamados/services/tarefa_do_dia.py:621` | Projeção `:633`; consumidores abaixo |
| Verbos de entrada | `alimentar_fila`, `chamados/juizes.py:1570` | Juiz da tarefa, `chamados/services/tarefa_do_dia.py:605` |
| `pendentes` do app | `consulta_do_app`, `chamados/services/pergunta_no_app.py:74` | Carga, `chamados/services/tarefa_do_dia.py:564` |
| Memo de pendentes | Carga, `chamados/services/tarefa_do_dia.py:563` | Mesma carga e juiz, `:556`, `:567`, `:607` |
| `pergunta_id`, ações | `_tarefa_pura`, `chamados/services/tarefa_do_dia.py:263` | Juiz das portas, `:607` |
| Identidade do sistema | Leitura em `chamados/services/tarefa_do_dia.py:392` | Contextos de tranca, `:444`, `:562` |

Censo dos consumidores:

| Leitor | Comportamento com `porta_app` |
|---|---|
| `tarefa_do_dia.py::porta_da_tarefa`, `:633` | Aceita rota preenchida, sem enumeração própria. |
| `folha/fora_da_folha.py:70`, `:85`, `:244` | Lê fio e fila publicados; fila vazia é aceita. |
| `folha/retrato_folha.py:183`, `:974`, `:1015` | Usa fio, sem consultar rota: `colab` conserva “Cobrar no fio”; `ninguem` corresponde a `nada`. |
| `chamados/services/furo_sem_canal.py:97`, `:116` | Reconhece a porta e agora a conta separadamente. |
| `chamados/services/vigia_fio.py:662`, `:730` | A indexação dinâmica precisava da nova chave; contador e apresentação adaptados. |
| `escala/services/cadastro_realidade.py:469` | Verifica presença de porta; o teste novo exclui **um dia sintético** do beco. (`chamados/tests/test_e3_degrau_dono_e_porta.py:267`) |
| `chamados/management/commands/diff_e3_sombra.py:43`, `:53`, `:143` | Projeta, compara e imprime a rota recebida. |
| `ponto/services/pautas_dos_discordantes.py:81` | Lê indiretamente o fio da Folha, comparando com `FIO_SEM_PORTA`. |
| `chamados/management/commands/backfill_fio_morto_e4b.py:87` | Consome instâncias e ações; não interpreta a rota. |

A saída completa do `git grep -n` está em [e3_degrau_censo.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_censo.txt:1). Os consumidores diretos anteriores de `consulta_do_app` conservam os defaults; `chamados/models.py:1846` é uma lápide, não um leitor ativo. (`api/views.py:994`, `:2534`; `core/views.py:253`; `chamados/services/aviso_pendencia.py:9`; `chamados/services/cobranca.py:48`)

O `em_lote=True` abre `tranca_da_consulta`, carrega tranca/toque e restaura o contexto. Existe escrita indireta possível: o toque chama `get_user_sistema()`, cujo corpo usa `get_or_create`. Por isso a carga usa `em_lote=False` e fornece ao contexto existente a identidade obtida por `User.objects.get`. Usuário ausente provoca falha, sem criação. (`chamados/juizes.py:1128`, `:1152`, `:1225`; `core/utils.py:18`; `chamados/services/tarefa_do_dia.py:392`, `:562`)

Resultados locais, sem certificação:

- RED anterior à cura: **18 testes, 12 falhas de asserção**. ([e3_degrau_red.txt:161](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_red.txt:161)) [nao verificado]
- Casos novos: **24 verdes**; rodada com regressões: **88 verdes**, incluindo execução dos corpos originais do selo L-129. ([e3_degrau_green.txt:27](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_green.txt:27), [e3_degrau_regressoes.txt:92](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_regressoes.txt:92))
- **16 mutantes**, cada um vermelho por asserção, sem erro de execução. Os dois exigidos mordem C4 e C3. Cada comando individual está registrado junto ao resultado. ([e3_degrau_mutantes.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:1), [:21](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:21), [:346](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:346))

Comandos de reprodução:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_degrau_dono_e_porta
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
```

Não alterei arquivos proibidos, ignorados ou a allowlist L-129; não fiz commit, merge, push ou deploy. HEAD permanece na base e `git diff --check` passou. Não executei ORM nem remedida de produção. ([e3_degrau_auditoria.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_auditoria.txt:1), [:27](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_auditoria.txt:27)) [nao verificado]

Registro solicitado: **PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA**, número recebido **2333**, pergunta “o que o admin pode fazer quando o colab não responde”; obra não construída nesta fatia. Remedida colab/admin/sem porta e número de produção: **[não verificado]**.

## 14/11 16:35 · sessao 6615c2d1b8dd · - · - · origem=Ronald (tmux)

5. **Nada de falha de relógio demonstrada no caminho novo.** A fonte ordena justificativas por `data_turno, pk`, mede com timestamps explícitos e chama o pareador com datas explícitas (`app/ponto/services/efeito_ausencia.py:98`, `:122`, `:179`, `:181`). O previsto consulta escala/marcos/pausas, sem batidas (`app/escala/utils.py:2275`, `:2285`, `:2288`, `:2310`). Os testes de idempotência repetem somente o juiz puro e a consulta da fonte; não repetem o fechamento completo (`app/ponto/tests/test_justificativa_paga_o_periodo.py:134`, `:320`). Idempotência integral de produção: **[nao verificado]**.

6. **O RED principal tem mordida localizada.** O teste exige `horas_abono == 11.5`, zero faltas e zero DSR perdido (`app/ponto/tests/test_justificativa_paga_o_periodo.py:255`). Reverter somente a conta nova do motor elimina o crédito explícito de 690 minutos que esse teste exige (`app/ponto/motor_calculo_v2.py:2628`). O controle de dia inteiro passaria com o motor anterior; exige apenas um dia abonado e horas positivas (`app/ponto/tests/test_justificativa_paga_o_periodo.py:262`). As propriedades de “pago + trabalhado” somam `minutos_trabalhados` dos períodos, não conferem `horas_abono` ou o fechamento gravado (`:338`, `:350`, `:380`, `:391`). Suíte executada/RED histórico: **[nao verificado]**.

As mudanças perguntadas preservam asserções: `test_cobparc` acrescenta mock da fonte de justificativas para manter a fixture sem banco (`app/ponto/tests/test_cobparc_marco_no_periodo.py:57`), preservando a cobrança de saída dispensada e hora não fabricada (`:75`). `test_s5b` amplia a lista e o motor fictício com o memo novo (`app/ponto/tests/test_s5b_porta_unica.py:988`, `:996`); continua removendo cada atributo e exigindo exceção nominal (`:1014`). Não há afrouxamento desses asserts.

7. **PROCEDENTE — derivação paralela de cadeia perde a segunda pausa.** A fonte reconstrói manualmente apenas `hi/hii/hfi/hf` (`app/ponto/services/efeito_ausencia.py:156`), embora a entidade declare também `hii2/hfi2` (`app/escala/models.py:1652`) e já exista `cadeia_do_dia`, que recebe todas as pausas (`app/escala/utils.py:353`). O medidor desconta somente pares presentes no dicionário recebido (`app/ponto/turnos.py:2786`), enquanto o previsto desconta todas as pausas (`app/escala/utils.py:2310`, `:2324`). Exemplo sintético confirmado em memória pelos métodos reais: 07–19, pausas 10–11:25 e 14–15:25, entrada 07:00: a cadeia produzida pela fonte paga **635 minutos**; completa, **550 minutos**. Nenhum arquivo foi escrito; ocorrência produtiva desse exemplo: **[nao verificado]**.

Não demonstrar “segundo juiz duplicando juiz registrado” apenas pela ausência de registro: o catálogo de turno e ausência não lista o juiz novo (`app/core/juizes.py:207`, `:251`), e o documento distingue lookup de registro vivo (`app/docs/JUIZES.md:7`). O corte fornecido autoriza expressamente seu nascimento. Há, porém, uma decisão posterior que ainda altera `cobre`: a fonte chama o juiz e depois remove cobertura por cruzamento (`app/ponto/services/efeito_ausencia.py:176`, `:182`; `app/ponto/turnos.py:2750`). Isso mostra cobertura decidida em duas etapas, não prova duplicação de autoridade já registrada.

## 14/11 16:35 · sessao 3c37fe314776 · - · - · origem=Ronald (tmux)

**PROCEDENTE — há derivação paralela e o período novo não chega ao abono materializado.**

- O motor lê `minutos_justificados` e soma o período em `horas_abono` (`app/ponto/motor_calculo_v2.py:2628-2634`), mas `FechamentoMensal.minutos_abonados` vem exclusivamente de `por_dia_da_grade` (`app/ponto/services/fechamento.py:1065-1067`).
- Essa função só abona `tipo_dia == 'ausencia'`, usando `previsto - realizado`; ignora `min_justificados` e `justificado` (`app/ponto/services/dia_pago.py:387-401`). A justificativa parcial mantém `tipo_dia='trabalho'` e apenas acrescenta o campo de minutos (`app/escala/utils.py:1257-1278`).
- A grade usada pelo fechamento lê a ATA (`app/folha/export.py:335-338`); o cartório não lavra `min_justificados` (`app/ponto/services/cartorio.py:738-739`) e o leitor não o transporta (`app/escala/services/leitor_celula.py:422-444`).
- O mesmo mapa inadequado alimenta a lavratura `DiaPago` (`app/ponto/services/fechamento.py:1217-1218`). Na competência, PDF/espelho recebem `horas_abono=minutos_abonados/60` do fechamento online (`app/relatorios/cartao_pela_celula.py:69-76,118`) ou da lavratura (`app/relatorios/cartao_pela_celula.py:186`), sobrescrevendo o resumo (`app/relatorios/cartao_pela_celula.py:323-346`).
- Contraexemplo sintético da função real, executada isolada por AST, sem banco e sem escrita: dia `trabalho`, previsto 720, realizado 0, `min_justificados=690`, `justificado=True` devolve `abono={}`. É consequência direta de `app/ponto/services/dia_pago.py:395-401`.

**PROCEDENTE — chamador de sombra continua com memo vazio.**

- `diff_espelho_alimentacao` monta motor, alimenta os demais memos e calcula sem alimentar `minutos_justificados` (`app/ponto/management/commands/diff_espelho_alimentacao.py:41-59`), inclusive no degrau anunciado como completo, que recebe `ef['justificadas']` e `ef['dia_inteiro']` (`app/ponto/management/commands/diff_espelho_alimentacao.py:113-122`).
- Como o memo nasce `{}` (`app/ponto/motor_calculo_v2.py:813`), esse degrau pode dar abono previsto inteiro enquanto os leitores migrados pagam o período (`app/ponto/motor_calculo_v2.py:2628-2634`).
- `diff_extrato_parcial` consulta as justificativas por conta própria e monta motor sem o novo memo (`app/relatorios/management/commands/diff_extrato_parcial.py:42-68`); seus campos retornados não incluem abono (`app/relatorios/management/commands/diff_extrato_parcial.py:70-76`).
- `esmeril_espelho` também não alimenta o memo e usa o resultado somente para assinaturas de pontualidade/HE (`app/ponto/services/esmeril_espelho.py:160-177`). `vigiar_motor_autoridade` não alimenta os efeitos, mas compara geometria/trabalhado (`app/ponto/management/commands/vigiar_motor_autoridade.py:57-68`). Esses dois caminhos não provam diferença de abono pago.

Mapa dos campos:

| Campo | Escritores/fontes | Leitores |
|---|---|---|
| `minutos_justificados` | Inicialização `app/ponto/motor_calculo_v2.py:813`; alimentação fechamento normal/órfão `app/ponto/services/fechamento.py:755,831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407` | Conta mensal `app/ponto/motor_calculo_v2.py:2628-2634`; declaração de memo `app/ponto/calculador/nucleo.py:424` |
| `min_justificados` | Efeito deriva de `periodos_justificados` (`app/ponto/services/efeito_ausencia.py:45-47,67`); grade escreve o valor (`app/escala/utils.py:1276`); fechamento deriva novamente o mapa (`app/ponto/services/fechamento.py:665-667`) | Espelho/PDF alimentam memo (`app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`); nenhum leitor de dinheiro da grade usa essa chave (`app/ponto/services/dia_pago.py:387-401`) |
| `dias_com_ausencia_inteira` | Fonte `app/ponto/turnos.py:2829-2846`; efeito `app/ponto/services/efeito_ausencia.py:68`; fechamento `app/ponto/services/fechamento.py:688-689,754,830`; espelho `app/ponto/services/espelho.py:1133`; PDF `app/relatorios/pdf_espelho.py:406`; diagnósticos `app/ponto/services/esmeril_espelho.py:164`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`, `app/ponto/management/commands/diff_espelho_alimentacao.py:57` | Teto de pontualidade `app/ponto/motor_calculo_v2.py:1090-1109`; contexto calculador `app/ponto/calculador/regras.py:117-120`; alimentação por dia `app/ponto/calculador/alimentacao.py:532`; fechamento envia ao oráculo `app/ponto/services/fechamento.py:1281` |
| `horas_abono` | Dataclass `app/ponto/motor_calculo_v2.py:292`; cálculo `app/ponto/motor_calculo_v2.py:2631-2634`; composição de fatias `app/ponto/services/fechamento.py:99` | PDF primeiro lê motor+parcial (`app/relatorios/pdf_espelho.py:480`), depois autoridade do resumo usa fechamento/lavratura (`app/relatorios/cartao_pela_celula.py:118,186,346`) |
| `dias_abono` | Dataclass `app/ponto/motor_calculo_v2.py:284`; motor `app/ponto/motor_calculo_v2.py:2630`; composição `app/ponto/services/fechamento.py:99`; resumo espelho/PDF `app/ponto/services/espelho.py:1253`, `app/relatorios/pdf_espelho.py:479` | PDF badge `app/relatorios/pdf_espelho.py:787`; ramo lavratura sobrescreve usando dias com minutos abonados positivos (`app/relatorios/cartao_pela_celula.py:190`; `app/ponto/services/dia_pago.py:687-699`) |
| `resumo_dias_abono` | Não localizado na cópia pela busca literal; existência/caminho [nao verificado] | [nao verificado] |

**PROCEDENTE — campo `dias_abono` possui dois significados conforme a fonte.** O motor exclui todo dia em `_mj`, mesmo com minutos pagos (`app/ponto/motor_calculo_v2.py:2629-2630`); o resumo da lavratura conta qualquer dia com `minutos_abonados>0` (`app/ponto/services/dia_pago.py:687-699`; `app/relatorios/cartao_pela_celula.py:190`).

**PROCEDENTE — ausência inteira coexistente não limpa o mapa parcial.** `efeitos_da_ausencia` conserva `min_justificados` ao acrescentar uma ausência ABONA (`app/ponto/services/efeito_ausencia.py:47,55-67`); fechamento faz o mesmo (`app/ponto/services/fechamento.py:667,695-701`). O motor então exclui esse dia do abono inteiro e paga apenas `_mj`, embora exista ausência inteira (`app/ponto/motor_calculo_v2.py:2628-2634`).

Fora do diff:

- TXT Domínio monta os eventos do fechamento, sem evento de abono/período na lista (`app/folha/export.py:439-461,730-732`). `datas_abonadas` usa `Ausencia`/catálogo para dias da rubrica 8932, não o mapa de justificativas (`app/folha/export.py:193-206`). Impacto financeiro efetivo no Domínio [nao verificado].
- Prontidão soma `minutos_abonados` do fechamento (`app/folha/export.py:1076-1079`), mas seu fallback rederiva abono pelo previsto inteiro de dias de ausência (`app/folha/export.py:1095-1111`).
- Haiku “fora da folha” lê `classificar_export` (`app/folha/fora_da_folha.py:115-119,216-222`); não lê os seis campos diretamente nesses sítios. Holerite nesta cópia recebe arquivos de folha, com lote de upload (`app/holerite/models.py:6-20`); equivalência de valores importados com os novos minutos [nao verificado].

## 14/11 16:35 · sessao 2a67c3406a0b · - · - · origem=Ronald (tmux)

2. **PROCEDENTE — segunda pausa perdida e crédito acima do previsto.** A fonte constrói somente `hi/hii/hfi/hf`, via quatro valores de `marcos_do_dia`; a segunda pausa nunca chega ao medidor (`app/ponto/services/efeito_ausencia.py:156`, `app/escala/models.py:1131`). O medidor só desconta pares presentes no mapa (`app/ponto/turnos.py:2786`). Já o previsto lê todas as pausas (`app/escala/models.py:1197`, `app/escala/utils.py:2310`, `app/escala/utils.py:2324`). Contraexemplo: jornada 07–19, duas pausas de 85 minutos, entrada 07:00 e saída justificada: crédito calculado 635 minutos, previsto 550; o motor soma o crédito sem teto (`app/ponto/turnos.py:2738`, `app/ponto/turnos.py:2791`, `app/ponto/motor_calculo_v2.py:2633`).

A chave é `Justificativa.data_turno`, e a fonte exige que o turno da âncora tenha essa mesma data (`app/ponto/services/efeito_ausencia.py:107`, `app/ponto/services/efeito_ausencia.py:165`). A cadeia dos marcos começa nessa data, permitindo saída no dia seguinte (`app/ponto/services/efeito_ausencia.py:163`). No Base, a comparação usa o dia da jornada; no Comercial, o DSR é refeito por data civil da entrada, constituindo derivação paralela preexistente (`app/ponto/motor_calculo_v2.py:2581`, `app/ponto/motor_calculo_v2.py:3473`, `app/ponto/motor_calculo_v2.py:3477`). Ocorrência desse desvio nos dias raspados: [nao verificado].

3. **PROCEDENTE — duas âncoras do mesmo turno podem duplicar minutos.** A cobertura une os rótulos, mas soma os minutos de cada justificativa (`app/ponto/services/efeito_ausencia.py:133`). Duas justificativas aprovadas, uma ancorada na entrada 07:30 e outra na volta 12:00 do mesmo turno `E07:30/S11:00/E12:00`, medem ambas a cauda 12–19: 420 + 420 = 840 minutos (`app/ponto/services/efeito_ausencia.py:168`, `app/ponto/services/efeito_ausencia.py:179`, `app/ponto/turnos.py:2738`). A unicidade impede repetir a mesma âncora, mas permite âncoras distintas; a restrição por dia vale apenas para pendentes (`app/ponto/models.py:187`, `app/ponto/models.py:189`). O motor soma os 840 sem deduplicar nem limitar ao previsto (`app/ponto/motor_calculo_v2.py:2633`). Ocorrência em produção: [nao verificado].

Um dia em `_mj` pode estar fora de `_abono` porque teve período de trabalho; a soma nova o inclui expressamente, independentemente de `_abono` (`app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2633`). Dias fora dos obrigatórios não recebem esse crédito (`app/ponto/motor_calculo_v2.py:2601`, `app/ponto/motor_calculo_v2.py:2634`).

4. **PROCEDENTE — cobertura vazia tem caminho distinto de conserva.** Sem âncora, papel errado e turno fechado retornam motivo; célula sem marcos também conserva (`app/ponto/turnos.py:2694`, `app/ponto/turnos.py:2700`, `app/ponto/turnos.py:2703`, `app/ponto/services/efeito_ausencia.py:154`). Entretanto, depois desse predicado, o filtro de sobreposição pode remover o marco e retornar vazio sem motivo (`app/ponto/services/efeito_ausencia.py:182`, `app/ponto/services/efeito_ausencia.py:183`, `app/ponto/turnos.py:2764`). O efeito exclui esse dia de `justificadas` e de `min_justificados` (`app/ponto/services/efeito_ausencia.py:46`). Há caso sintético correspondente e referência nominal a col451 no teste (`app/ponto/tests/test_justificativa_paga_o_periodo.py:212`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:383`).

Vazio não implica automaticamente falta: o Base considera todo período presente como dia trabalhado, inclusive aberto, sem testar minutos positivos (`app/ponto/motor_calculo_v2.py:2499`, `app/ponto/motor_calculo_v2.py:2581`). Portanto, nesses dias, paga o trabalho calculado e nenhum crédito de justificativa; falta/DSR dependem de o dia estar em `datas_trabalho` (`app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2633`). Resultado individual de col206, col451 e col551, e condição global 0/0: [nao verificado].

O filtro ainda compara contra envelope bruto entrada–fim, sem retirar pausas, enquanto o motor calcula pares com intervalos; sobreposição detectada não prova sobreposição com minutos efetivamente pagos (`app/ponto/turnos.py:2745`, `app/ponto/turnos.py:2748`, `app/ponto/motor_calculo_v2.py:2551`, `app/ponto/motor_calculo_v2.py:2558`).

## 14/11 16:06 · sessao dd2c6ced8087 · E3-V10 · leitura · origem=Code

**A classe C é explicável pelo residual `cobrar`; a causa exata da mudança de verbo em B não está demonstrada; e `sla.vencido=True` não comprova vencimento do prazo de resposta da pergunta.** (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/juizes.py:1650`, `app/chamados/services/sla.py:33`, `app/chamados/catalogo/motor.py:54`; causa concreta de B: [nao verificado])

Uso os resultados de produção conforme o anexo. Campos brutos ausentes e execução no banco ficam [nao verificado]. O código literal de `91207281b` não está disponível nesta cópia; o percurso anterior abaixo é o C17 preservado nela, cuja leitura histórica pela Folha está documentada. (`app/folha/fora_da_folha.py:22`; equivalência integral ao main indicado: [nao verificado])

**1. Matriz de transições**

O estado bruto comum está incompleto: a sonda informa IDs, ações e resultados de SLA, mas não traz `status_local`, `prazo_sla`, texto/carimbo da resposta, sequência de decisões ou situação da disputa. Portanto, não permite reconstituir integralmente “o que o banco tem” para cada linha. [nao verificado]

Na cópia, pertença à fila implica `status_local` em `COBRANCA`, isto é, `aberto` ou `em_analise`, e ausência de silêncio futuro; qual desses status cada chamado possui não consta do anexo. (`app/chamados/services/painel.py:188`, `app/chamados/catalogo/motor.py:29`; status individual: [nao verificado])

| Classe/árvore | Estado informado → juiz → dono mostrado → possibilidades |
|---|---|
| **A — main** | Os 53 dias informados aparecem como `admin`, com portas `porta_fila` ou `porta_pergunta`, verbos predominantemente `validar` e alguns `decidir`. [nao verificado] No percurso C17 preservado, `porta_alcancavel_do_dia` reúne portas e chama `fio_pelos_verbos`: `validar` produz `admin`; `decidir` sozinho produz `chamado`. (`app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1709`, `app/chamados/juizes.py:1650`) A disponibilidade concreta dos botões no main não pode ser certificada pela árvore atual. [nao verificado] |
| **A — v10** | A sonda mostra perguntas devidas e ações `responder/colaborador`, eventualmente coexistindo com `validar/admin`; todos os chamados publicados têm o SLA operacional vencido, sem aviso e sem escalonamento. [nao verificado] `tarefa_do_dia` lê a última decisão; ausência de decisão, resposta vazia ou devolução ao colaborador produzem `responder`; a agregação das portas prioriza `colab`, mesmo havendo ação administrativa. (`app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:143`, `app/chamados/services/tarefa_do_dia.py:147`, `app/chamados/services/tarefa_do_dia.py:588`, `app/chamados/services/tarefa_do_dia.py:595`) As portas efetivas de cada lado estão discriminadas abaixo. |
| **B — main** | c204/07-09, chamado `33525`, e c903/14-09, chamado `23072`: `porta_fila`, verbo `validar`, fio `admin`, conforme anexo. [nao verificado] O C17 preservado traduz esse verbo em `admin`; ele não calcula o verbo. (`app/chamados/juizes.py:1700`, `app/chamados/juizes.py:1709`) |
| **B — v10** | A sonda mostra `33525→cobrar`, perguntas `28256/29043→responder`; `23072→cobrar`, pergunta `28150→responder`; nenhuma ação administrativa da tarefa. [nao verificado] O dono vem dessas ações alcançáveis e resulta `colab`; o mapa de verbos é apenas copiado da alimentação. (`app/chamados/services/tarefa_do_dia.py:583`, `app/chamados/services/tarefa_do_dia.py:588`, `app/chamados/services/tarefa_do_dia.py:595`) A linha do painel oferece cobrança em lugar do botão inline de validar. (`app/templates/chamados/partials/_lista_chamados.html:81`, `app/templates/chamados/partials/_lista_chamados.html:83`) |
| **C — main** | emp3/c996/30-10: somente `38666→validar`, fio `admin`, conforme anexo. [nao verificado] Esse resultado é compatível com a tradução do C17 preservado. (`app/chamados/juizes.py:1650`, `app/chamados/juizes.py:1709`) |
| **C — v10** | A sonda mostra apenas `validar/admin/38666/pergunta52035`, mas publica também `33362→cobrar`, sem pergunta devida pelo colaborador. [nao verificado] `38666` fornece dono administrativo pela ação; `33362`, residual, fornece `colab` pelo verbo; `colab` vence na agregação final. (`app/chamados/services/tarefa_do_dia.py:588`, `app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:594`, `app/chamados/services/tarefa_do_dia.py:595`) |

As classes se sobrepõem: os dois B aparecem em A; C também aparece em A2. Não são contagens independentes somáveis. [nao verificado]

**Portas do colaborador, aplicáveis à matriz.** A ação da tarefa usa como porta `chamado_que_lista(p)` quando essa âncora pertence à fila; essa função escolhe destino da pergunta, senão chamado da disputa. Uma ação com `porta=None` não comprova alcance pela fila. (`app/chamados/services/tarefa_do_dia.py:135`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/services/fio.py:116`)

A resposta efetiva passa pelo endpoint `responder_pergunta_disputa`, que verifica vínculo do usuário, propriedade da pergunta e C6 vivo, depois chama `responder_pergunta_disputa_core`. A casca não exige que o fio da tarefa seja `colab` nem verifica SLA. (`app/chamados/views.py:1228`, `app/chamados/views.py:1233`, `app/chamados/views.py:1239`, `app/chamados/views.py:1242`, `app/chamados/views.py:1283`)

**Ação `responder` não garante pergunta visível como pendente no app:** a consulta do app exige C6 vivo e coloca em pendentes somente pergunta sem `respondida_em` e com disputa aberta; as demais vivas vão ao histórico. Isso importa especialmente para o estado “carimbo de resposta com texto vazio”. (`app/chamados/services/pergunta_no_app.py:52`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:73`; presença efetiva dos IDs da sonda na tela: [nao verificado])

**Portas do admin, aplicáveis à matriz.**

- **Validar:** a linha `validar` oferece `validar_inline`; clicar na linha abre o fio. No modal há ação individual `validar_pergunta`, condicionada ao estado da pergunta, sem depender do dono publicado pela tarefa. (`app/templates/chamados/partials/_lista_chamados.html:42`, `app/templates/chamados/partials/_lista_chamados.html:81`, `app/templates/chamados/partials/modal_fio.html:217`, `app/templates/chamados/partials/modal_fio.html:228`, `app/templates/chamados/partials/modal_fio.html:279`)
- **Cobrar:** a linha `cobrar` oferece push, salvo cobrança já registrada no dia. O endpoint exige `gerir_chamados`, POST, área permitida e sucesso do canal de envio. (`app/templates/chamados/partials/_lista_chamados.html:83`, `app/chamados/views_cobrar.py:25`, `app/chamados/views_cobrar.py:30`, `app/chamados/views_cobrar.py:35`, `app/chamados/views_cobrar.py:44`)
- **Decidir:** o verbo `decidir` aparece como rótulo na linha; a decisão é feita nas portas do caso. “Regularizar disputa” não permite fechar pergunta viva sem resposta. Existe também “Resolver dia”, condicionado a último recurso, dia passado, status que pede decisão e permissão `regularizar_batida`. (`app/templates/chamados/partials/_lista_chamados.html:86`, `app/chamados/services/fio.py:684`, `app/chamados/services/fio.py:698`, `app/colaboradores/services/calendario.py:97`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/ponto/views.py:3355`)

Logo, nos A com ações simultâneas, o fio `colab` não remove por si a validação administrativa. Nos A contendo somente `responder`, a tarefa não fornece ação administrativa de validar; o serviço de validação recusa pergunta sem resposta. O alcance concreto por usuário, disputa e pergunta não é demonstrado pela sonda. (`app/chamados/services/tarefa_do_dia.py:143`, `app/chamados/services/validacao.py:418`; alcance individual: [nao verificado])

**2. B: caminho exato de `validar` para `cobrar`**

O caminho comprovado **na cópia** é:

`alimentar_fila` → `particionar_verbos(qs_vivos())` → `creditos_perguntas` → `verbo_do_chamado` → mapa consumido pelo C17 ou pela tarefa. Ambos usam essa alimentação; `_portas_da_tarefa` não converte `validar` em `cobrar`. (`app/chamados/juizes.py:1570`, `app/chamados/juizes.py:1616`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`, `app/chamados/services/tarefa_do_dia.py:543`, `app/chamados/services/tarefa_do_dia.py:583`)

Há uma mudança documentada capaz de produzir essa conversão: o crédito exige **carimbo e texto não vazio** para contar resposta. Carimbo sem texto entra no conjunto sem resposta; a natureza traduz crédito de resposta em `validar` e crédito de pergunta sem resposta em `cobrar`. (`app/chamados/services/raiox.py:139`, `app/chamados/services/raiox.py:143`, `app/chamados/services/raiox.py:144`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/catalogo/natureza.py:50`, `app/chamados/catalogo/natureza.py:58`)

**Não está provado que esse seja o motivo de `33525/23072`.** Faltam o código anterior e os campos que determinaram os créditos, inclusive texto, carimbos, destino e universo vivo. Atribuir a mudança somente à substituição de C17 pela tarefa seria incorreto diante da alimentação compartilhada. (`app/chamados/juizes.py:1562`; causalidade desses IDs: [nao verificado])

O admin conserva uma **porta estrutural de validação pelo fio/modal**, ação `validar_pergunta`, além do detalhe local quando aplicável. O botão individual não depende do verbo `cobrar/validar` da linha. Porém, a validação exige pergunta respondida e ainda não validada; não é possível certificar sucesso para os dois B. (`app/templates/chamados/partials/modal_fio.html:217`, `app/templates/chamados/partials/modal_fio.html:279`, `app/chamados/services/fio.py:623`, `app/chamados/services/fio.py:644`, `app/chamados/services/validacao.py:418`, `app/chamados/services/validacao.py:421`; sucesso concreto: [nao verificado])

**3. C: ramo que produz `colab` sem ação do colaborador**

O encadeamento é:

1. `CargaDaTarefa.tarefa` chama `_portas_da_tarefa`, levando âncoras de todas as perguntas carregadas do dia. (`app/chamados/services/tarefa_do_dia.py:535`, `app/chamados/services/tarefa_do_dia.py:547`)
2. As portas publicadas incluem portas das ações, chamados da célula, âncoras na fila e chamados sem célula ligados pelo dia forte. (`app/chamados/services/tarefa_do_dia.py:571`, `app/chamados/services/tarefa_do_dia.py:573`, `app/chamados/services/tarefa_do_dia.py:575`, `app/chamados/services/tarefa_do_dia.py:577`)
3. `38666`, porta da ação `validar`, fornece `admin`. O `33362` não pertence às portas das ações, portanto entra em `residuais = portas - portas_acoes`. (`app/chamados/services/tarefa_do_dia.py:588`, `app/chamados/services/tarefa_do_dia.py:591`; associação dos IDs: [nao verificado])
4. O residual `cobrar` vira `colab` por `fio_pelos_verbos`; a ordem final escolhe `colab` antes de `admin`. (`app/chamados/services/tarefa_do_dia.py:594`, `app/chamados/juizes.py:1650`, `app/chamados/services/tarefa_do_dia.py:595`)
5. `porta_da_tarefa` apenas projeta esse resultado; `_fio_do_dia` apenas o lê. (`app/chamados/services/tarefa_do_dia.py:607`, `app/folha/fora_da_folha.py:83`, `app/folha/fora_da_folha.py:89`)

Portanto, **o `colab` de C vem do dono do verbo residual, não de uma ação `responder` nem de `nao_sei`**. Qual das ligações incluiu `33362` — célula, âncora ou dia forte — não consta da sonda. (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:595`; ligação concreta: [nao verificado])

**4. Definições de “vencida” e seus campos**

| Definição | Regra, unidade e autoridade |
|---|---|
| **`sla.vencido` da sonda** | Última nota `dp_rh`, senão criação do chamado, mais SLA por urgência; vence com **`< agora`**, apenas em status de cobrança. Limites: 1/2/24/48 horas. Não lê `prazo_sla` nem resposta da pergunta. (`app/chamados/services/sla.py:17`, `app/chamados/services/sla.py:26`, `app/chamados/services/sla.py:33`, `app/chamados/catalogo/motor.py:35`) |
| **Prazo de cobrança — C9** | `prazo_estourou(prazo_sla, agora)` é verdadeiro com **`prazo_sla <= agora`**; sem prazo retorna falso. O helper `prazo_de_cobranca_do_chamado` usa prazo explícito ou criação +20 minutos. (`app/chamados/catalogo/motor.py:45`, `app/chamados/catalogo/motor.py:54`, `app/chamados/catalogo/motor.py:59`) |
| **Prazo da pergunta** | A pergunta não tem campo próprio de prazo. A cobrança por pessoa grava prazo no chamado **pai da disputa**, compartilhado: 48h por padrão ou prazo informado. Recobrança por chamado usa 20min. (`app/chamados/models.py:2499`, `app/chamados/models.py:2525`, `app/chamados/services/cobranca.py:51`, `app/chamados/services/cobranca.py:170`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1725`) |
| **Marco vencido da instância** | Horário previsto do marco +30min, comparado ao instante. É exigibilidade da marcação, não prazo para responder pergunta. (`app/ponto/services/cartorio.py:882`, `app/ponto/services/cartorio.py:892`, `app/chamados/services/tarefa_do_dia.py:102`) |
| **Lavra vencida/stale** | Ausência/futuro da lavra, expiração por dia civil local ou insumo posterior. Significa que o veredito não responde com segurança. (`app/chamados/veredito_lavrado.py:95`, `app/chamados/veredito_lavrado.py:213`, `app/chamados/services/tarefa_do_dia.py:415`) |
| **Silêncio/arquivo e atraso** | `silenciado_ate` determina exclusão temporal da fila; o painel também distingue arquivado/histórico. “Atrasado” das pílulas conta idade em dias, com limite 3; não é C9. (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:48`, `app/chamados/services/verbos.py:12`, `app/chamados/services/verbos.py:57`) |
| **Prazo de justificativa DP** | Outro domínio: dias civis locais desde criação, vencendo quando a quantidade supera o prazo do tipo. (`app/ponto/catalogo/justificativas.py:21`, `app/ponto/catalogo/justificativas.py:34`) |

Os escritores e leitores relevantes são:

| Campo/insumo | Quem escreve → quem lê |
|---|---|
| `status_local`, criação e urgência | Nascimento pelas portas do modelo; transições por `_gravar_estado`; criação tem `auto_now_add`; subida de urgência por `escalar_urgencia`. SLA operacional lê status/criação/urgência; fila lê status. (`app/chamados/models.py:1101`, `app/chamados/models.py:201`, `app/chamados/models.py:67`, `app/chamados/models.py:514`, `app/chamados/services/sla.py:17`, `app/chamados/services/sla.py:37`, `app/chamados/services/painel.py:188`) |
| Nota DP: autor, conteúdo, criação | Porta `nota.anexar_ocorrencia`; criação automática do followup. SLA lê a última nota DP como referência. (`app/chamados/services/nota.py:75`, `app/chamados/services/nota.py:88`, `app/chamados/models.py:1721`, `app/chamados/services/sla.py:19`) |
| `prazo_sla` | Emissores gravam na abertura; recobrança e cobrança por pessoa passam por `definir_prazo_sla`. Abertura manual pode transformar data em 23:59 local. C9 é lido pelo escalonamento, pauta de calados e gestão da fila. (`app/chamados/services/disputa_emissao.py:581`, `app/chamados/services/disputa_emissao.py:1117`, `app/chamados/services/disputa_emissao.py:1352`, `app/chamados/services/disputa_emissao.py:1725`, `app/chamados/services/cobranca.py:178`, `app/chamados/models.py:574`, `app/chamados/services/abertura.py:154`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/cobranca.py:255`, `app/chamados/services/fila.py:167`) |
| Texto/carimbo e decisão de resposta | O core grava texto, `respondida_em` e linha `RESPOSTA` no mesmo ato; a tarefa lê decisões ordenadas por `ato_em,pk`, até `agora`. Projeção sem linha é reconhecida como passivo. (`app/chamados/services/disputa_emissao.py:1460`, `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1508`, `app/chamados/services/tarefa_do_dia.py:403`, `app/chamados/services/tarefa_do_dia.py:412`) |
| `ja_avisado` | **Não é campo boolean persistido:** é derivado de nota de sistema contendo `SLA`. O cron escreve a nota; consulta e cron leem o predicado. (`app/chamados/services/sla.py:42`, `app/chamados/management/commands/alertar_chamados_sla.py:32`, `app/chamados/management/commands/alertar_chamados_sla.py:44`, `app/chamados/services/sla.py:66`) |
| `escalonado_para_dp` | O cron chama `marcar_escalonado_dp` depois do push; a porta grava boolean e instante, com guarda para não repetir. O cron lê o boolean para evitar novo N1. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`, `app/chamados/models.py:595`, `app/chamados/models.py:604`) |

A configuração usa `America/Sao_Paulo` e `USE_TZ=True`. Entretanto, **não são a mesma unidade nem a mesma fronteira**: SLA e C9 comparam instantes, com `<` versus `<=`; lavra e justificativa usam dia civil local; a pergunta da tarefa pertence ao **dia do turno**, por `data_evento`, inclusive em turno noturno. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/services/sla.py:39`, `app/chamados/catalogo/motor.py:56`, `app/chamados/veredito_lavrado.py:104`, `app/ponto/catalogo/justificativas.py:34`, `app/chamados/services/tarefa_do_dia.py:227`)

O juiz existente candidato ao degrau é **C9, `chamados/catalogo/motor.py::prazo_estourou`**, registrado no lookup e no registro vivo. A pergunta a lhe entregar é o prazo de cobrança associado à pergunta relevante; isso não autoriza escolher qualquer chamado vencido publicado no dia. (`app/docs/JUIZES.md:63`, `app/core/juizes.py:289`, `app/chamados/services/cobranca.py:51`)

**Não é possível contar quantos dos 53 A satisfazem esse C9:** a sonda mediu outro SLA e não fornece os prazos nem o vínculo pergunta→pai necessário para essa conclusão. [nao verificado]

**5. Contrato de saída do degrau**

**O degrau ainda não existe no julgamento mostrado.** Os ramos sem resposta continuam entregando `responder/colaborador`, sem consultar prazo; a escolha final também não consulta vencimento de resposta. (`app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:144`, `app/chamados/services/tarefa_do_dia.py:147`, `app/chamados/services/tarefa_do_dia.py:588`)

O ponto de saída que precisa satisfazer o contrato é a composição final **dentro do mesmo juiz**, em `_portas_da_tarefa`, antes da projeção. Alterar somente `_pela_decisao` não garante `admin`, porque um residual `cobrar` pode acrescentar `colab` novamente. Alterar somente a Folha criaria uma segunda decisão do dono e violaria a dependência exigida pelo selo. (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:595`, `app/chamados/services/tarefa_do_dia.py:598`, `app/chamados/services/tarefa_do_dia.py:607`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16`)

O contrato pedido é devolver **o dono do dia** ao admin quando estiver estabelecida a condição “pergunta vencida sem resposta”; isso não fabrica resposta, validação ou decisão de silêncio. As portas de escrita continuam exigindo seus próprios requisitos. (`app/chamados/services/validacao.py:418`, `app/chamados/services/fio.py:698`; complemento literal recebido apenas no anexo: [nao verificado])

Todos os leitores efetivos encontrados, incluindo aliases e o leitor indireto da pauta, são:

| Leitor | Consequência de mudar somente `fio: colab→admin` |
|---|---|
| Folha: `_porta`/`_fio_do_dia` | Lê o novo dono literalmente. (`app/folha/fora_da_folha.py:71`, `app/folha/fora_da_folha.py:83`) |
| Folha: `fora_da_folha` | Muda contagem por fio e possivelmente grupo da pessoa; outro dia `colab` pode manter seu grupo B. (`app/folha/fora_da_folha.py:197`, `app/folha/fora_da_folha.py:200`, `app/folha/fora_da_folha.py:92`) |
| Folha: `o_que_segura` | Muda `fio` e frase de destravamento. (`app/folha/fora_da_folha.py:241`, `app/folha/fora_da_folha.py:249`) |
| Vigia: `censo_alcance` | Muda papel listado e contador `alcance_porta_exige_o_admin`. (`app/chamados/services/vigia_fio.py:646`, `app/chamados/services/vigia_fio.py:662`) |
| `furo_se

[truncado em 20000 de 29318 caracteres]

## 14/11 16:01 · sessao 47e77db94853 · - · - · origem=Ronald (tmux)

Não há AGENTS.md listado no escopo pela busca de arquivos; não executei banco/Django nem escrita. [nao verificado] [nao verificado]

**1. Caminho C17 e tarefa, inclusive B.**

- `alimentar_portas` (C17) chama `alimentar_fila`; tarefa chama a mesma função. Essa função chama `particionar_verbos(qs_vivos(), agora=agora)`, obtém pertença em `qs_na_fila(agora=agora)` e guarda mapa de verbos. Portanto **trocar apenas o leitor de C17 para tarefa não transforma `validar` em `cobrar`**. Provas: `app/chamados/juizes.py:1559`, `:1570`, `:1573`, `:1616`; `app/chamados/services/tarefa_do_dia.py:541`, `:543`, `:583`.
- C17 forma portas de chamados da célula na fila e de pergunta C6 viva, não respondida sem texto, no app e com âncora na fila; retorna `fio_pelos_verbos`. `app/chamados/juizes.py:1695`, `:1699`, `:1701`, `:1703`, `:1709`.
- `fio_pelos_verbos` prioriza `cobrar -> colab`, depois `validar -> admin`, resto `chamado`. `app/chamados/juizes.py:1650`, `:1655`.
- Tarefa lê verbos sem alterá-los; forma donos das ações alcançáveis mais dono dos verbos residuais; prioriza colab, admin, chamado, sistema. `app/chamados/services/tarefa_do_dia.py:570`, `:583`, `:588`, `:591`, `:594`, `:595`.
- Folha projeta a porta e lê `porta['fio']`, retornando `nada` sem porta. `app/folha/fora_da_folha.py:71`, `:77`, `:86`.
- Candidato de mudança de verbo na cópia: crédito passou a exigir **carimbo E texto**; `_resp` usa `respondida_em != None && !texto_vazio`; `_naoresp` inclui texto vazio. O comentário registra explicitamente a alteração b6. `app/chamados/services/raiox.py:135`, `:139`, `:143`, `:144`, `:155`.
- Crédito atribui ao destino vivo, senão chamado da disputa. `app/chamados/services/raiox.py:146`.
- A partição chama crédito e depois `verbo_do_chamado`. `app/chamados/services/verbos.py:34`, `:47`.
- Natureza prioriza módulo próprio de validar, depois crédito de resposta, depois crédito de pergunta viva; a tradução é `admin_validar -> validar`, `colab -> cobrar`. `app/chamados/catalogo/natureza.py:41`, `:46`, `:48`, `:50`, `:58`.
- Essa mudança de tratamento do par resposta pode explicar B; **não se prova que os chamados 33525/23072 tenham carimbo sem texto**, nem que essa era a implementação de `91207281b`, pois sonda não traz esses campos e main não está disponível. [nao verificado]

**2. Classe C: ramo exato.**

- `CargaDaTarefa.tarefa` chama `_portas_da_tarefa` com âncoras **de todas as perguntas carregadas do dia**, não só ações. `app/chamados/services/tarefa_do_dia.py:535`, `:547`.
- Portas incluem ações, chamados da célula, âncoras das perguntas na fila e vivos sem FK ligados pelo dia forte. `app/chamados/services/tarefa_do_dia.py:571`, `:573`, `:574`, `:575`, `:577`.
- No dado C há ação admin via 38666 e verbo residual 33362=`cobrar`. Se 38666 é porta da ação e 33362 não é, `residuais = portas - portas_acoes` inclui 33362; `fio_pelos_verbos` retorna colab, que ganha de admin. Esse é o ramo compatível e suficiente para explicar o resultado, sem ação colab alguma. `app/chamados/services/tarefa_do_dia.py:588`, `:591`, `:594`, `:595`; `app/chamados/juizes.py:1650`.
- Qual caminho incluiu 33362 — célula, âncora de pergunta ou dia forte — não é determinado pela sonda. [nao verificado]
- Não é `nao_sei` que cria colab: esse fallback só é usado depois da busca por donos, e C traz falso na sonda. `app/chamados/services/tarefa_do_dia.py:595`.

**3. Portas reais do admin e colab.**

- Lista do admin abre fio por clique na linha. `app/templates/chamados/partials/_lista_chamados.html:42`.
- `validar` oferece botão `validar_inline`; `cobrar` oferece push, salvo já cobrado hoje; `decidir` apenas rótulo na coluna. `app/templates/chamados/partials/_lista_chamados.html:81`, `:82`, `:83`, `:84`, `:86`.
- Assim B perde o botão inline **na linha**, porque verbo virou cobrar; isso não prova inexistência de validação no fio. `app/templates/chamados/partials/_lista_chamados.html:81`.
- `validar_inline` exige `gerir_chamados`, POST e área; busca disputa aberta direta ou por destino; percorre perguntas vivas com carimbo de resposta e chama `acao_validar_pergunta`. `app/chamados/views_cobrar.py:145`, `:149`, `:151`, `:156`, `:160`, `:168`, `:208`.
- Se não houver resposta candidata, retorna aguardando colab/nada aguardando validação. `app/chamados/views_cobrar.py:169`, `:175`, `:192`, `:198`.
- Fio completo e modal exigem gestor, carregam `montar_chamados_fio` sem recorte de janela; essa montagem carrega todos os chamados do colaborador e perguntas de disputas abertas; em disputas fechadas só as C6 vivas. `app/chamados/views.py:659`, `:662`, `:676`, `:869`, `:872`, `:889`; `app/chamados/services/fio.py:190`, `:204`, `:246`, `:251`, `:265`, `:267`.
- Perguntas aparecem sob `chamado_destino_id`, senão chamado da disputa, via `chamado_que_lista`. `app/chamados/services/fio.py:97`, `:116`, `:117`, `:270`, `:274`.
- Modal desenha Validar quando há `respondida_em`, sem pendência administrativa e sem `validada_em`; não depende do verbo nem dono da tarefa. `app/templates/chamados/partials/modal_fio.html:217`, `:222`, `:228`, `:239`, `:279`, `:281`, `:284`.
- POST do fio exige `pode_agir_chamado`, confirma pergunta sob mesmo chamado e colaborador e chama `acao_validar_pergunta`; disputa fechada requer C6 viva e usa `sem_reabrir`. `app/chamados/services/fio.py:596`, `:601`, `:604`, `:623`, `:636`, `:638`, `:644`, `:647`.
- `acao_validar_pergunta` delega à autoridade `validacao.validar_pergunta`; esta recusa pergunta sem `respondida_em` e já validada. `app/chamados/services/acoes_disputa.py:13`, `:18`, `:19`; `app/chamados/services/validacao.py:398`, `:418`, `:421`.
- Portanto há porta estrutural de validar na árvore: **fio/modal, ação `validar_pergunta`**, e detalhe local para disputa aberta. Não se pode afirmar sucesso/alcance concreto em B: sonda não fornece resposta atual, disputa aberta/fechada, validação, permissão/área do usuário; suas ações só têm responder. [nao verificado]
- Outra casca é detalhe local `/chamados/local/<id>/`: para gestor, valida se pergunta em disputa aberta e `chamado_que_lista` corresponde. `app/chamados/urls.py:14`; `app/chamados/views.py:483`, `:493`, `:495`, `:499`, `:501`.
- URLs: fio colaborador em `/chamados/colaborador/<id>/`, responder em `/chamados/disputa/responder/<pergunta_id>/`, cobrar em `/chamados/cobrar/<cid>/`, inline em `/chamados/validar-inline/<cid>/`. `app/chamados/urls.py:17`, `:21`, `:25`, `:33`.
- Push cobrar exige `gerir_chamados`, POST e área, limita uma cobrança por colab/dia local e depende de push ativo; não valida a pergunta. `app/chamados/views_cobrar.py:23`, `:25`, `:27`, `:30`, `:35`, `:36`, `:41`, `:44`, `:51`.
- Colab responder: endpoint verifica usuário vinculado ao colaborador, pergunta desse colaborador e C6 viva, depois chama `responder_pergunta_disputa_core`; não consulta dono da tarefa nem SLA nessa casca. `app/chamados/views.py:1228`, `:1233`, `:1239`, `:1242`, `:1281`, `:1283`.
- App: `perguntas_do_app` seleciona q_vivas e C6; UI pendentes exige `respondida_em is None` e disputa aberta, demais vivas vão para histórico. `app/chamados/services/pergunta_no_app.py:45`, `:52`, `:61`, `:64`, `:74`.
- Ação responder com porta `None` na sonda **não garante porta de fila publicada nem pergunta visível no app**; é preciso conferir C6 e estado da disputa para a tela real. `app/chamados/services/tarefa_do_dia.py:137`, `:141`; `app/chamados/services/pergunta_no_app.py:74`.

**4. Consequência para contrato.**

- Mudar somente `out['fio']` em `_tarefa_pura` seria sobrescrito pela projeção `_portas_da_tarefa`; esta recalcula o fio das ações e residuais. O degrau precisa sobreviver a essa etapa do mesmo juiz, inclusive a residual `cobrar` de C. `app/chamados/services/tarefa_do_dia.py:537`, `:547`, `:588`, `:591`, `:595`, `:598`.
- Passar dono a admin não cria resposta nem validação: `validar_pergunta` permanece recusando sem resposta, e o painel continua tomando botões de `item.verbo`. `app/chamados/services/validacao.py:418`; `app/templates/chamados/partials/_lista_chamados.html:81`.
- “Admin passa a poder validar todas as A” seria conclusão incorreta: muitas só têm ação responder e o dado não demonstra resposta validável. [nao verificado]

## 14/11 16:01 · sessao 9cd5c4b7cffd · - · - · origem=Ronald (tmux)

Censo dos leitores efetivos, sem executar aplicação, banco ou testes. Não há `AGENTS.md` encontrado no workspace/ancestrais consultados [nao verificado]. [nao verificado]

| Leitor | Caminho exato | Efeito de mudar apenas `fio: colab → admin` |
|---|---|---|
| Folha: carga e porta | `app/folha/fora_da_folha.py:62-89` | `_fio_do_dia` devolve literalmente o fio publicado; sem porta devolve `FIO_SEM_PORTA`. |
| Folha: relatório agregado | `app/folha/fora_da_folha.py:185-215` | Muda contagem por fio e possivelmente grupo da pessoa; `_grupo` prioriza nada/chamado/colab/admin, logo outro dia colab pode manter grupo B (`:92-103`). |
| Folha: detalhe individual | `app/folha/fora_da_folha.py:238-252` | Muda `fio` e frase `DESTRAVA`; mantém ids da fila se a porta mantiver a fila. |
| Vigia: `censo_alcance` | `app/chamados/services/vigia_fio.py:646-663` | Muda papel listado e `alcance_porta_exige_o_admin`; não traduz novamente o verbo. |
| Furo sem canal: `medir` | `app/chamados/services/furo_sem_canal.py:93-102` | Lê rota, existência de porta e `nao_sei`; mudar só fio não muda seu resultado. |
| Beco: `beco_em_lote` | `app/escala/services/cadastro_realidade.py:464-469` | Lê somente porta ser `None`; mudar só fio não muda inclusão. |
| Pauta de discordantes, indireto via Folha | `app/ponto/services/pautas_dos_discordantes.py:64,79-82` | Compara fio com `FIO_SEM_PORTA`; colab→admin continua tendo fio, sem mudança de inclusão. |
| Backfill: alias `T.tarefa_do_dia` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:84-117` | Lê instâncias, ações, razões e `nao_sei`; não lê `fio`. Se degrau mudar somente dono, seus atos permanecem. |
| Instrumento sombra: alias `candidata = porta_da_tarefa` | `app/chamados/management/commands/diff_e3_sombra.py:43,117-130` | Projeta novos fios para Folha/Vigia e igualdade de porta para beco/pauta; projeções em `:46-60`. |

Leitura indireta do retrato: `montar_linhas` chama Folha em `app/folha/retrato_folha.py:1156-1159` e guarda fio derivado do grupo em `:1190`. A gravação ocorre apenas com `--apply`, em `app/folha/management/commands/lavrar_retrato_folha.py:69,76-80`; o render lê o gravado em `app/folha/retrato_folha.py:428-440`. Portanto, trocar o juiz não muda imediatamente os retratos já gravados. O mapa da UI é `:192`.

**Atenção concreta ao alcance na Folha:** a linha com fio colab tem atalho “Cobrar no fio”; a linha admin tem `None` nesse eixo, dependendo de porta por tipo. Isso está em `app/folha/retrato_folha.py:974-995`, consumido em `:1015-1020`. A mudança do dono sozinha pode retirar esse atalho; não se pode afirmar que a porta admin permanece alcançável por esse caminho sem considerar outros atalhos. A frase admin passa a validar/recusar, conforme `app/folha/fora_da_folha.py:45-48`, selecionada em `:249-251` e em `app/folha/retrato_folha.py:1068-1069`.

O julgamento final do dono reside em `_portas_da_tarefa`, `app/chamados/services/tarefa_do_dia.py:553-598`. Há dois fornecedores internos de dono: ações cuja porta está na fila (`:570-588`) e chamados residuais, que ganham dono por `fio_pelos_verbos` (`:591-594`); a precedência final colab/admin/chamado/sistema está em `:595-598`. `porta_da_tarefa` só projeta o resultado (`:601-607`).

Assim, **um degrau restrito a `_pela_decisao` não garante a saída admin**: o residual `cobrar` pode acrescentar colab novamente e vencer a precedência final. O contrato precisa alcançar a saída final do mesmo juiz, considerando esse caminho residual; essa observação descreve o fluxo existente, sem propor implementação. `app/chamados/services/tarefa_do_dia.py:588-598`.

A sequência atual decide ausência de resposta→colab (`:140-141`), resposta com texto→admin validar (`:143-146`), resposta vazia→colab (`:144-145`), recusa/reabertura→colab (`:147-148`), silêncio decidido→nenhuma ação (`:160-161`). As decisões são carregadas por `(pergunta, ato_em, pk)` e cortadas em `ato_em <= agora` (`:403-408`). Logo, depois de resposta tardia efetiva, **a pergunta isolada continua no admin, agora para validar**; outro pedido/residual do dia pode conservar o fio colab pela agregação (`:588-596`). Não afirmar “respondeu, volta colab”.

O selo AST existente é exclusivamente de **dependências do leitor Folha**, não de semântica de vencimento. Allowlist de `carga_da_tarefa`, `porta_da_tarefa` e `FIO_SEM_PORTA` em `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16-21`; inspeção de imports/nomes/atributos/strings/imports dinâmicos em `:24-49`; antívacuidade exige imports da carga e projeção em `:57-63`; nove mutantes em `:65-82`. Ele não prova prazo, resposta ou devolução admin.

Propriedades fixas pertinentes ao contrato, apoiadas no que já existe:

- Mesma entrada e `agora`→mesma saída; zero escrita; lote=avulso: `app/chamados/services/tarefa_do_dia.py:52-54,615-625`; testes em `app/chamados/tests/test_e2_tarefa_do_dia.py:879-911`.
- Um relógio passado pela borda, nenhum `now/localdate()` implícito dentro do juiz: `app/chamados/services/tarefa_do_dia.py:619-624`; testes runtime em `app/chamados/tests/test_e2_tarefa_do_dia.py:668-691` e AST em `:1004-1044`.
- Mesmo dono pela tarefa e sua projeção; Folha e Vigia apenas consomem a projeção: `app/chamados/services/tarefa_do_dia.py:607`; testes `app/chamados/tests/test_e3_portas_tarefa.py:485-495,534-547`.
- `nao_sei` independente do dono: retorno separado em `app/chamados/services/tarefa_do_dia.py:595-607`; teste `app/chamados/tests/test_e3_portas_tarefa.py:534-547`.
- Preservar ações/portas/verbos quando só muda responsabilidade: união existente em `app/chamados/services/tarefa_do_dia.py:570-584`; teste de ações preservadas em `app/chamados/tests/test_e3_portas_tarefa.py:503-515`.
- Não usar vencimento de marco como prazo de resposta: `vencido` de instância é teto do marco, vindo de `app/ponto/services/cartorio.py:882-893`, transportado por `app/ponto/turnos.py:3060-3066` e `app/chamados/services/tarefa_do_dia.py:102-113`.
- Previsto não depende do realizado e “todo leitor lê o mesmo número” são propriedades expressas em `app/docs/LEIS.md:136` (L-110). O degrau não deve alterar grade/DNA/previsto; seus dados de marco são lidos de autoridades em `app/chamados/services/tarefa_do_dia.py:95-113,518-527`.
- Universo/contagens/exportação não devem mudar apenas por classificação do fio: Folha obtém universo antes da carga em `app/folha/fora_da_folha.py:178-185`; os testes existentes comparam universo e ausência de mutações em `app/chamados/tests/test_e3_portas_tarefa.py:350-383`. Export real preservado em todas as competências concretas da sonda permanece [nao verificado].

A L-129 original está registrada em `app/docs/LEIS.md:149`; o complemento “vencida sem resposta” veio no texto do usuário e não encontrei registro correspondente nos arquivos consultados [nao verificado].

## 14/11 16:01 · sessao 32de8a9570c1 · - · - · origem=Ronald (tmux)

**Achado decisivo:** o `sla.vencido=True` da sonda não prova que venceu o prazo de resposta da pergunta: o SLA de tela mede horas por urgência desde a última nota do DP ou o nascimento; o C9 mede `prazo_sla <= agora`, com opção de derivar nascimento + 20 minutos. Os valores de `prazo_sla` e a associação pergunta→pai não constam da sonda. (`app/chamados/services/sla.py:17–39`; `app/chamados/catalogo/motor.py:54–72`; campos concretos da sonda: [nao verificado])

**Definições distintas e candidatos:**

| Definição | Regra / campos lidos | Escritor / leitor |
|---|---|---|
| SLA operacional de tela | `status_local in COBRANCA` e `referencia + timedelta(hours=sla_limite_horas(urgencia)) < agora`; referência = última nota `dp_rh` ou `criado_em`. Não lê resposta da pergunta nem `prazo_sla`. | Regra: `app/chamados/services/sla.py:17–39`; tabela 1/2/24/48h: `app/chamados/catalogo/motor.py:35–41`; leitores tela `app/chamados/services/painel.py:372–379`, cron `app/chamados/management/commands/alertar_chamados_sla.py:25–46`; timestamps automáticos de chamado/nota: `app/chamados/models.py:67`, `app/chamados/models.py:1721`; notas pela porta `app/chamados/services/nota.py:75–109`; subida de urgência pela porta `app/chamados/models.py:514–545`. |
| Prazo de cobrança C9 | `bool(prazo_sla and prazo_sla <= agora)`; helper retorna prazo explícito, senão `criado_em+20min`. | `app/chamados/catalogo/motor.py:45–72`; registro vivo `app/core/juizes.py:289`; lookup `app/docs/JUIZES.md:59–63`; escritores nascimento `app/chamados/services/disputa_emissao.py:581`, `:1117`, `:1352`; recobrança `:1725`; cobrança em lote `app/chamados/services/cobranca.py:170–178`; gate único `app/chamados/models.py:574–593`; leitores `app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/cobranca.py:255`, `app/chamados/services/fila.py:165–169`. |
| Prazo “da pergunta” | Não há campo prazo na pergunta; a cobrança guarda prazo no **pai da disputa**, compartilhado pelas perguntas. Cobrança em lote usa 48h por padrão ou prazo explícito; recobrança usa 20min. | Campos `app/chamados/models.py:2499–2527`; associação ao pai `app/chamados/services/cobranca.py:51–65`; 48h `app/chamados/services/cobranca.py:38`, `:165–178`; 20min `app/chamados/services/disputa_emissao.py:1692–1725`. |
| Marco previsto vencido | `quando+30min <= agora`; marco do turno encadeado atravessa meia-noite. Não é prazo para responder. | `app/ponto/services/cartorio.py:862`, `:882–893`; `app/escala/servico_jornada.py:69–94`; fábrica `app/chamados/services/fabrica_da_celula.py:35–49`; tarefa lê o teto em `app/chamados/services/tarefa_do_dia.py:82–113`. |
| Lavra vencida / stale | Lavra ausente, posterior a `agora`, janela civil local expirada ou insumo posterior; não é atraso de resposta. | Escritor `app/chamados/veredito_lavrado.py:254–275`; leitor `:213–231`; relógio civil local `:95–116`; tarefa `app/chamados/services/tarefa_do_dia.py:415–418`. |
| Silêncio / arquivo | `silenciado_ate > agora` esconde; vencimento devolve à fila. Constante de arquivo = 30 dias. | `app/chamados/models.py:87–92`, `:509–512`; `app/chamados/catalogo/motor.py:83–84`; fila `app/chamados/services/painel.py:158`. |
| Justificativa DP | Dias civis locais desde nascimento; vence quando `dias > prazo_do_tipo`. Outro domínio. | `app/ponto/catalogo/justificativas.py:21–41`; registro `app/core/juizes.py:311`. |

**Candidata do degrau:** o prazo de cobrança entregue ao C9 `catalogo/motor.py::prazo_estourou`, já registrado para a pergunta “o prazo (SLA) do chamado estourou?”. Usar `sla.vencido` da sonda substituiria essa pergunta por horas de ausência de resposta do DP; usar marco vencido substituiria por horário de batida. A escolha de qual pai/prazo corresponde à pergunta concreta precisa respeitar a associação existente, pois cobrança em lote grava nos pais compartilhados. (`app/core/juizes.py:115`, `:289`; `app/docs/JUIZES.md:63`; `app/chamados/services/sla.py:17–39`; `app/chamados/services/cobranca.py:51–65`)

**Relógio:** a configuração é `America/Sao_Paulo`, `USE_TZ=True`; SLA de tela e C9 comparam instantes, com limites distintos (`<` versus `<=`), horas versus prazo explícito/fallback em minutos; marcos têm instante aware encadeado e teto de 30min; a lavra e a justificativa usam dia civil local; identidade da pergunta da tarefa usa **dia do turno** `data_evento`. Portanto não são o mesmo relógio semântico nem a mesma unidade. (`app/config/settings/base.py:93–95`; `app/chamados/services/sla.py:30–39`; `app/chamados/catalogo/motor.py:45–72`; `app/escala/servico_jornada.py:89–93`; `app/ponto/services/cartorio.py:892`; `app/chamados/veredito_lavrado.py:104–116`; `app/ponto/catalogo/justificativas.py:34–41`; `app/chamados/services/tarefa_do_dia.py:227–230`)

**Sem resposta não deve ser reconstruído apenas de `respondida_em`:** a tarefa usa última `DecisaoPergunta` até `agora`: ausência de decisão, resposta vazia, recusa e reabertura produzem responder/colaborador; resposta com texto produz validar/admin. O carregador ordena por `ato_em,pk`, corta o horizonte e reconhece projeção sem linha como passivo. (`app/chamados/services/tarefa_do_dia.py:126–150`, `:400–413`)

**Escritores de resposta:** o core de resposta grava texto e `respondida_em` no mesmo instante da linha de decisão; resposta nova invalida validação anterior; reabertura zera resposta/carimbo por porta e registra o ato; resposta automática e recuperação pela trilha também registram decisões. (`app/chamados/services/disputa_emissao.py:1460–1485`, `:1562–1608`, `:1640–1664`, `:1668–1685`)

**Aviso e escalonamento:**

- `ja_avisado` é derivado, não campo boolean: nota de sistema cujo conteúdo contém `'SLA'`. O cron registra a nota via `anexar_ocorrencia`; passivo anterior ao corte e teto por passada podem deixar chamado vencido sem aviso. (`app/chamados/services/sla.py:42–45`; `app/chamados/management/commands/alertar_chamados_sla.py:32–46`; `app/chamados/services/nota.py:75–109`)
- `escalonado_para_dp` é recibo de push do N1: o cron compara C9, envia push e chama `marcar_escalonado_dp`; o gate escreve boolean, instante e trilha, com guarda idempotente. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:93–107`; `app/chamados/models.py:595–616`)
- O escalonamento N2 pode alterar urgência para emergência, afetando o SLA operacional baseado em urgência; isso é efeito indireto no prazo operacional, não autorização para usar flags de aviso como dono do dia. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:109–138`; `app/chamados/services/sla.py:26–39`)
- A tarefa não recebe aviso/escalonamento como critério no julgamento da última decisão; nesse julgamento o lado resulta do ato da pergunta. (`app/chamados/services/tarefa_do_dia.py:126–150`)
- Na sonda, as 53 linhas da classe A provam o agrupamento pelo SLA operacional informado, mas não permitem afirmar quantas satisfariam o C9 no prazo associado à pergunta; faltam prazo explícito, nascimento, última nota DP e associação pergunta→pai. [nao verificado]

## 14/11 15:39 · sessao 3eb93b59aecc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Corrigi o lado **TESTE** nos três casos: [nao verificado]

- **(a)** I agora encerra pela porta real, sem pergunta ancorada; depois nasce P e o reaponte para G continua exigido. Nenhuma guarda é substituída. [ORM:181](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:181)
- **(b)** A fixture antiga perdia elegibilidade e retornava `None` antes de criar disputa. Agora invalida somente a data do marco, preserva tipo/hora e exige disputa produzida e fechada. [ORM:300](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:300)
- **(c)** Dona/local empatavam no relógio congelado. Agora a local é efetivamente posterior e o teste verifica o pedido G na porta real, mantendo **uma trilha**. A seleção efetiva na execução anexada é [nao verificado]. [ORM:116](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:116) [nao verificado]

A guarda grava `valor_depois={'chamado_destino_atual': I.pk, 'chamado_destino_pedido': G.pk}`; `models._nascer` evita duplicar essa chave. [fio:790](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/services/fio.py:790), [models:2990](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/models.py:2990)

Foram tocados quatro arquivos: teste ORM, suplemento em memória, mutações e entrega. Motivos, três hipóteses e comandos individuais estão no [inventário](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:5). [nao verificado]

Em memória: **8 testes verdes**, RED da fixture base com **1 failure/0 errors**, nove mutantes mortos por assertion e seis contrastes anteriores verdes. [Resultados e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:60) [nao verificado]

Certificação ORM: **[nao verificado]**, reservada à sessão. Ruff indisponível (`No module named ruff`); lint limpo: **[nao verificado]**. [Limites](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:87)

## 14/11 15:20 · sessao 5573ac6b03cc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Implementação, inventário dos 13 arquivos, censo e comandos estão na [entrega c22](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:14).

- Verificação local: 83 testes legados, 7 suplementares e 35 da catraca passaram; 19 mutantes e o registro antigo morreram por assertion. [Resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:103). [nao verificado]
- RED/GREEN ORM, incluindo P3 vermelho: **[não verificado]**. P3 foi entregue sem `expectedFailure`; o hunk protegido continua pendente da sessão. [Comando](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:108).
- Ruff limpo: **[não verificado]**, ferramenta ausente. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:11).

Ressalva explícita: `[G]` normalizado pede I e espera zero recusas; o teste da guarda força o pedido G na entrada e exige uma trilha em duas passadas. [Detalhe](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:37). [nao verificado]

## 14/11 15:11 · sessao fbfa3d68b664 · E3-V10 · leitura · origem=Code

**A árvore não preserva o main por leitor:** Folha muda o fio nos quatro estados; a API conserva o fio, mas muda porta/fila/verbos em P19; o vigia troca sua própria projeção de papel e admin. (`M:app/folha/fora_da_folha.py:65–78`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `M:app/chamados/services/vigia_fio.py:648–651`; `E:app/folha/fora_da_folha.py:74–89`; `E:app/chamados/services/tarefa_do_dia.py:570–598`; `E:app/chamados/services/vigia_fio.py:661–663`)

Uso **M** para linhas do main reconstruídas do lado antigo do patch fornecido e **E** para linhas da cópia. Não atribuo ao main arquivos ausentes do patch. Identidade integral desses arquivos entre main e cópia: `[nao verificado]`.

**Valores das portas**

Notação: `PF=porta_fila`, `PP=porta_pergunta`; cada saída abaixo é **fio / rota / fila / verbos**. Essas são as chaves devolvidas pelas duas implementações. (`M:app/chamados/juizes.py:1667–1668`; `M:app/chamados/services/tarefa_do_dia.py:553–554`; `E:app/chamados/services/tarefa_do_dia.py:597–598`)

Para P13/P14, o dado admite porta na própria célula ou âncora exclusivamente externa. Portanto, não há uma localização única demonstrada: na primeira, rota/fila são `PF/(A,)`; na segunda, `PP/()`. O mapa conserva o verbo informado. Localização concreta em produção: `[nao verificado]`. (`M:app/chamados/juizes.py:1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598`)

| Estado | C17 main | API tarefa main | Tarefa na árvore | Evidência M / E |
|---|---|---|---|---|
| P13: responder; painel validar | **admin** / PF ou PP / `(A,)` ou `()` / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | `M:app/chamados/juizes.py:1608–1619,1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598` |
| P14: validar; painel cobrar | **colab** / PF ou PP / `(A,)` ou `()` / `{A:cobrar}` | **admin** / mesma geometria / `{A:cobrar}` | **admin** / mesma geometria / `{A:cobrar}` | Mesmos trechos da linha anterior |
| P19: A residual da célula decidir; ação admin sob B externo cobrar | **colab / PF / `(A,)` / `{A:decidir,B:cobrar}`** | **admin / PP / `()` / `{B:cobrar}`** | **admin / PF / `(A,)` / `{A:decidir,B:cobrar}`** | `M:app/chamados/juizes.py:1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598` |
| Simultâneo do anexo: colab+admin; único verbo validar | **admin** / geometria da porta / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | `M:app/chamados/juizes.py:1608–1619,1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/tests/test_e3_portas_tarefa.py:507–515` |

O C17 continua na árvore com a escada `cobrar→colab`, `validar→admin`, demais→chamado. Sua resposta própria foi preservada; seus antigos leitores passaram a receber outra resposta. (`M:app/chamados/juizes.py:1608–1619,1653–1668`; `E:app/chamados/juizes.py:1650–1661,1695–1710`; `E:app/core/juizes.py:132–138`)

**Comparação por leitor**

As referências “C17”, “tarefa main” e “tarefa árvore” nesta tabela remetem às saídas completas e às linhas da tabela anterior.

| Leitor × estado | Main | Árvore | IGUAL/DIFERE |
|---|---|---|---|
| Folha/`fora_da_folha` — P13 | C17: **admin** | tarefa: **colab**; porta/fila/verbos conservados | **DIFERE fio** |
| Folha/`fora_da_folha` — P14 | C17: **colab** | tarefa: **admin**; porta/fila/verbos conservados | **DIFERE fio** |
| Folha/`fora_da_folha` — P19 | C17: **colab**, PF, `(A,)`, `{A:decidir,B:cobrar}` | **admin**, PF, `(A,)`, mesmo mapa | **DIFERE fio** |
| Folha/`fora_da_folha` — simultâneo | C17: **admin** | tarefa: **colab**; porta/fila/verbos conservados | **DIFERE fio** |
| API `tarefa_do_dia` — P13 | tarefa main: colab | colab, mesma porta/fila/verbos | **IGUAL** nesses campos |
| API `tarefa_do_dia` — P14 | tarefa main: admin | admin, mesma porta/fila/verbos | **IGUAL** nesses campos |
| API `tarefa_do_dia` — P19 | admin, **PP, `()`, `{B:cobrar}`** | admin, **PF, `(A,)`, `{A:decidir,B:cobrar}`** | **DIFERE porta/fila/verbos** |
| API `tarefa_do_dia` — simultâneo | tarefa main: colab | colab, mesma porta/fila/verbos | **IGUAL** nesses campos |

A Folha lê o fio da porta e `o_que_segura` também publica os chamados de `fila`; não publica o mapa completo de verbos. Fontes da comparação Folha: `M:app/folha/fora_da_folha.py:65–78,225–235`; `E:app/folha/fora_da_folha.py:71–89,241–252`. Fontes da comparação API: `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:570–598`.

O vigia precisa de tabela própria: **seu papel no main era a lista de verbos, não o fio C17**. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:661–663`)

| Vigia × estado | Main: papel; incremento admin | Árvore: papel; incremento admin | IGUAL/DIFERE |
|---|---|---|---|
| P13 | `validar`; `bool({'validar'} ∩ VERBOS_DO_ADMIN_main)` | `colab`; **0** | **DIFERE papel**; número main `[nao verificado]` |
| P14 | `cobrar`; `bool({'cobrar'} ∩ VERBOS_DO_ADMIN_main)` | `admin`; **1** | **DIFERE papel**; número main `[nao verificado]` |
| P19 | `cobrar,decidir`; `bool({'cobrar','decidir'} ∩ VERBOS_DO_ADMIN_main)` | `admin`; **1** | **DIFERE papel**; número main `[nao verificado]` |
| Simultâneo | `validar`; `bool({'validar'} ∩ VERBOS_DO_ADMIN_main)` | `colab`; **0** | **DIFERE papel**; número main `[nao verificado]` |

Fontes: `M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:661–663`. A rota recebida acompanha C17 no main e tarefa na árvore, conforme a primeira tabela; em P19 ambos recebem PF. (`M:app/chamados/services/vigia_fio.py:640,649`; `E:app/chamados/services/vigia_fio.py:648,661`)

**Corrijo o limite da atualização anterior:** só está comprovado na cópia que `VERBOS_DO_ADMIN=('validar','decidir')`. O arquivo correspondente do main não veio no patch; portanto, os números main **1/0/1/1** e a igualdade numérica de P19 ficam `[nao verificado]`. (`E:app/chamados/services/verbos.py:22`)

| Demais leitores × estado | Main | Árvore | IGUAL/DIFERE |
|---|---|---|---|
| `furo_sem_canal` — P13 | Não devolve fio/porta/verbos/fila; cobertura pelos registros crus da célula | Não devolve esses campos; havendo a porta descrita, exclui a célula de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |
| `furo_sem_canal` — P14 | Mesma regra crua; carimbos/FKs concretos `[nao verificado]` | Havendo a porta descrita, exclui de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |
| `furo_sem_canal` — P19 | A vivo da própria célula fornece cobertura | A porta fornece cobertura | **IGUAL cobertura da célula; DIFERE formato** |
| `furo_sem_canal` — simultâneo | Mesma regra crua; carimbos/FKs concretos `[nao verificado]` | Havendo a porta descrita, exclui de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |

O main testa **pergunta da célula com `respondida_em IS NULL` OU chamado da célula em VIVOS**. A árvore testa presença da porta. O main publica `com_pergunta_aberta/com_chamado_vivo`; a árvore publica `com_porta_fila/com_porta_pergunta/nao_sei`. A entrada no universo também depende de janela/veredito; totais de produção `[nao verificado]`. (`M:app/chamados/services/furo_sem_canal.py:80–86,95–96`; `E:app/chamados/services/furo_sem_canal.py:75–87,97–116`)

| Leitor | P13 | P14 | P19 | Simultâneo | Evidência e limite |
|---|---|---|---|---|---|
| Painel | Mapa dado: `{A:validar}` | `{A:cobrar}` | `{A:decidir,B:cobrar}` | `{A:validar}` | Na árvore, fila e verbos vêm de `qs_na_fila/particionar_verbos`; não há fio/rota de dia nesse contrato. **IGUAL/DIFERE do corpo main `[nao verificado]`**: falta seu arquivo main. (`E:app/chamados/services/painel.py:188–190,212–232`) |
| Pauta `dias_sem_fio` | Árvore não inclui como sem fio | Mesmo resultado | Mesmo resultado | Mesmo resultado | Só compara fio com `FIO_SEM_PORTA`; todos os fios acima são diferentes de nada. **Main `[nao verificado]`**, arquivo ausente. (`E:app/ponto/services/pautas_dos_discordantes.py:79–84`) |
| Beco `cadastro_realidade` | Árvore não inclui como sem porta | Mesmo resultado | Mesmo resultado | Mesmo resultado | Só observa existência da porta nesse ramo. **Main `[nao verificado]`**, arquivo ausente. (`E:app/escala/services/cadastro_realidade.py:464–471`) |
| Retrato/tela da Folha | Valor agregado `[nao verificado]` | Idem | Idem | Idem | Recebe os grupos da fonte; outros dias podem determinar o grupo da pessoa. Foto efetivamente lavrada não foi fornecida. (`E:app/folha/fora_da_folha.py:92–103,197–215`; `E:app/folha/retrato_folha.py:1158–1190`) |

**Quem escreve e quem lê**

“Escrever” aqui distingue produzir o resultado em memória de persistir uma foto. A tarefa produz saída pura; a persistência do retrato ocorre no command próprio. (`E:app/chamados/services/tarefa_do_dia.py:597–618`; `E:app/folha/management/commands/lavrar_retrato_folha.py:69–78`)

| Informação | Quem produz | Quem lê nos pontos relevantes |
|---|---|---|
| Verbo do chamado | `verbo_do_chamado`; `particionar_verbos` escreve `mapa[ch_id]` | Painel e `alimentar_fila`. (`E:app/chamados/catalogo/natureza.py:41–75`; `E:app/chamados/services/verbos.py:47–52`; `E:app/chamados/services/painel.py:229–232`; `E:app/chamados/juizes.py:1570`) |
| Pertença à fila | `qs_na_fila`; `alimentar_fila` monta `fila_ids/fila_cel/fila_dia` | C17 e tarefa. (`E:app/chamados/services/painel.py:188–190`; `E:app/chamados/juizes.py:1573–1589,1699–1708`; `E:app/chamados/services/tarefa_do_dia.py:571–584`) |
| Porta/fila/verbos/fio C17 | `porta_alcancavel_do_dia`, com `fio_pelos_verbos` | Na árvore, lado antigo da sombra. (`E:app/chamados/juizes.py:1655–1661,1699–1710`; `E:app/chamados/management/commands/diff_e3_sombra.py:127`) |
| Porta/fila/verbos/fio da tarefa | `_portas_da_tarefa`; `porta_da_tarefa` apenas projeta | API, Folha, vigia, furo, beco. (`E:app/chamados/services/tarefa_do_dia.py:570–607`; `E:app/folha/fora_da_folha.py:74–89`; `E:app/chamados/services/vigia_fio.py:648–663`; `E:app/chamados/services/furo_sem_canal.py:97–115`; `E:app/escala/services/cadastro_realidade.py:469`) |
| Papel/admin do vigia | Main: verbos/interseção; árvore: fio/igualdade | Contadores e linhas de alcance. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:635–663`) |

**Menor ponto de cura de cada diferença**

| DIFERE | Menor retorno ao contrato do mesmo leitor no main | Restrição |
|---|---|---|
| Folha — quatro estados | Voltar sua porta à autoridade **C17 existente**, no limite `_porta/_fio_do_dia`; o leitor continua apenas lendo fio/fila | Não mudar globalmente o fio da tarefa: isso quebraria a API main em P13/P14/simultâneo. (`M:app/folha/fora_da_folha.py:65–78`; `M:app/chamados/services/tarefa_do_dia.py:550–554`; `E:app/folha/fora_da_folha.py:71–89`) |
| API — P19 | Em `_portas_da_tarefa`, restaurar a projeção original **selecionada pelas ações**, antes da união residual | O trecho que acrescenta A é a união com `da_celula`; dele saem a mudança de rota, fila e mapa. (`M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–584,597–598`) |
| Vigia — quatro papéis | Restaurar a projeção main: papel é a lista ordenada de verbos | Trocar apenas o fio não reproduz o papel textual main. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:662–663`) |
| Vigia — regra administrativa | Restaurar a pergunta main: **existe verbo administrativo?** | `fio == admin` responde outra pergunta. A regra main deve continuar usando sua autoridade `VERBOS_DO_ADMIN`; seu conteúdo main falta. (`M:app/chamados/services/vigia_fio.py:650`; `E:app/chamados/services/vigia_fio.py:662`; `E:app/chamados/services/verbos.py:22`) |
| Furo — formato/cobertura | Restaurar o retorno e o teste main no próprio `medir` | Esse teste cru era **regra própria do leitor**. Restaurá-lo literalmente mantém um derivador paralelo à porta da tarefa; o dado não demonstra equivalência que permita substituí-lo sem mudar comportamento. (`M:app/chamados/services/furo_sem_canal.py:80–96`; `E:app/chamados/services/furo_sem_canal.py:97–116`) | [nao verificado]

**Não existe um único valor de `fio` que reproduza simultaneamente Folha-main e API-main nesses estados.** O registro vivo já distingue as perguntas: C17 responde alcançabilidade com verbo do painel; C19 responde ações exigíveis por instância. Preservar essas autoridades nas respectivas perguntas evita inventar regra no leitor. Unificá-las sob uma única resposta exige mudar pelo menos um contrato main. (`E:app/core/juizes.py:138,153–159`; `M:app/chamados/juizes.py:1608–1619`; `M:app/chamados/services/tarefa_do_dia.py:550–554`)

No vigia, a projeção antiga também era regra do leitor. Para ele apenas ler respostas prontas, seria necessário **mover essa mesma projeção para uma autoridade existente**, conservando a pergunta “algum verbo exige admin”; deduzi-la novamente do fio não serve. Já no furo, uma cura sem sua regra própria ou sem transferir integralmente essa regra não está demonstrada. (`M:app/chamados/services/vigia_fio.py:648–651`; `M:app/chamados/services/furo_sem_canal.py:80–86`; `E:app/core/juizes.py:138,159`)

**O que os testes fixam** [nao verificado]

| Teste | Espera na árvore | Relação com o main | [nao verificado]
|---|---|---|
| `test_e3_portas_v6_sem_banco.py:16–20` | P13: responder + validar → **colab** | Preserva tarefa-main; fixa divergência da Folha/C17-main, que dá admin. (`M:app/chamados/juizes.py:1608–1619`; `M:app/chamados/services/tarefa_do_dia.py:550–554`) |
| Mesmo arquivo `:22–26` | P14: validar + cobrar → **admin** | Preserva tarefa-main; contraria Folha/C17-main colab. Mesmas fontes main acima. |
| Mesmo arquivo `:28–42` | Ação admin/cobrar + residual decidir → **admin** | Fixa conflito de fio com C17. **Não fixa a geometria exata de P19:** põe ambos os chamados na célula e espera fila `(101,202)`. (`E:app/chamados/tests/test_e3_portas_v6_sem_banco.py:38–42`) |
| `test_e3_portas_tarefa.py:85–97` | Variante P13: A residual decidir, B externo validar, ação responder → colab | Espera tarefa, enquanto C17 dá admin para decidir+validar. (`M:app/chamados/juizes.py:1608–1619`) |
| `test_v8_R2_hipotese_acao_admin_isolada`, `test_e3_portas_tarefa.py:496–501` | Admin isolado → admin | **Não cobre simultâneo.** (`E:app/chamados/tests/test_e3_portas_tarefa.py:497–501`) |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia`, mesmo arquivo `:503–515` | Simultâneo → **colab** | Mesma porta, só validar: contraria C17 admin. Portas distintas, validar+cobrar: C17 também colab; essa variante não demonstra divergência de fio. (`M:app/chamados/juizes.py:1608–1619`; `E:app/chamados/tests/test_e3_portas_tarefa.py:507–514`) |
| `test_v8_R2_alcance_admin_com_pergunta_colab`, mesmo arquivo `:203–225` | Fixture ORM consulta crédito/fila e espera validar/colab | Fixa a expectativa E3/tarefa-main; execução ORM nesta auditoria `[nao verificado]`. (`E:app/chamados/tests/test_e3_portas_tarefa.py:203–225`) |
| `test_e3_diff_sombra.py:157–164` | Validar com fio colab → `exige_admin=False` | Fixa a nova projeção do vigia; não testa sua expressão main por interseção. (`M:app/chamados/services/vigia_fio.py:648–651`) |
| `test_e3_pendentes_app.py:145–159` | Só a sombra pode chamar C17; acusa Folha voltando a ele | A catraca impede o retorno direto à fonte main; não prova equivalência das respostas. (`E:app/chamados/tests/test_e3_pendentes_app.py:145–159`) |

Os testes v6 usam corpos reais extraídos por AST com fornecedores em memória; não substituem ORM. O executor puro remove a classe ORM. Foram executados 10 testes v6 e os dois R2 puros, todos passaram; resultado de terminal sem arquivo persistido: `[nao verificado]`. (`E:app/chamados/tests/test_e3_portas_v6_sem_banco.py:1–4`; `E:app/chamados/tests/e3_portas_apoio.py:18–26`; `E:app/chamados/tests/e3_portas_regra.py:22–29`) [nao verificado]

Para fechar os pontos main ainda não verificáveis, faltam **`app/chamados/services/verbos.py`**, **`app/chamados/services/painel.py`**, **`app/ponto/services/pautas_dos_discordantes.py`** e **`app/escala/services/cadastro_realidade.py`** do main. Eles não estão nos hunks fornecidos; suas respostas integrais no main permanecem `[nao verificado]`.

_5191 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
