# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 10:24** · publicado em **21/11/2026 10:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 10:24 · sessao 915e6f0db115 · E4C-F5 · leitura · origem=Code

## 1. Contagem e lista de 31

O vPRONTO6 marca **35 itens** como `SO POR EXECUCAO` na tabela de 01–113 e **6** nos avais posteriores, totalizando **41**. A conta chega a **36** ao descontar os cinco que a execução 4 registra como fechados — **11, 12, 57, 59 e 73**. Os cinco continuam com a marca no vPRONTO6. A tabela abaixo lista os **41 números e nomes**, mais 32, 80 e 102. `vPRONTO6_r.txt:5–143,240`, `execucoes.txt:29`, `app/docs/RELATO.md:12–13`

**Existe uma lista de 31 no anexo.** É a seção `FALTA-EXECUCAO` do vPRONTO6 — **10, 13, 20, 22, 27, 28, 39, 43, 44, 47, 48, 50, 52, 53, 54, 60, 79, 80, 83, 84, 87, 88, 90, 91, 96, 98, 108, 109, 117, 120 e 128**. O aval usa “os 31 de execucao” sem enumerá-los. A lista do anexo inclui o **80**, que o aval já cita separadamente, o **128**, marcado `FALTA`, e o **120**, identificado como fora da F5. A identidade entre essa lista e os “31” do aval fica **[nao verificado]**. `aval.txt:7,9`, `vPRONTO6_r.txt:84,135,143,185–217`

## 2. Onde roda e o que a prova cobre

Os rótulos Django começam em `chamados`, `ponto` ou `escala`, com `app/` como raiz montada. **NAO EXISTE na cópia** indica que falta o instrumento específico para a medida pedida. Quando o RELATO nomeia um instrumento externo, deixo o nome registrado e o conteúdo **[nao verificado]**. `bin/suite.sh:316–325`, `bin/sombra.sh:13–14`, `app/docs/RELATO.md:45,70,90`

A prova disponível da bateria de **163** termina em **EM CURSO**. O resultado de `bateria.out` e a conciliação **163 → 124 por nome** estão **[nao verificado]**. A suíte de `b0935c1f1` tem `rc=0` agregado. As provas de sombra citadas abaixo são de `9e90d`, com identidade de produção declarada no anexo. A confirmação independente do hash por Git está **[nao verificado]**. `execucoes.txt:1–2,29–31`, `aval.txt:6`, `vPRONTO6_r.txt:247`

| Item | O que precisa ser medido — trecho literal do PRONTO | Onde roda | Bateria de 163 ou suíte `rc=0` basta? | [nao verificado]
|---|---|---|---|
| **10. Mutantes tira-lote** | “colher a asserção de cada mordida” dos 33 seletores, 9 A + 14 B + 10 C. `vPRONTO6_r.txt:14,187` | **M** `F5_VOLTA_A_MUTANTE`, `F5_VOLTA_B_MUTANTE` e `F5_VOLTA_C_MUTANTE`, com os 33 valores nas tabelas. **T** módulos `chamados.tests.test_e4c_f5_voltaA_lotes`, `chamados.tests.test_e4c_f5_voltaB_lotes` e `chamados.tests.test_e4c_f5_voltaC_lotes`. Os dois casos destacados são `chamados.tests.test_e4c_f5_voltaA_lotes.CarregadoresEmLoteTest.test_RED_k16_pendentes_s128` e `chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k47_escolhas_sem_credor`. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,58–71`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:11–27,83`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:11–31`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:112–146`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222` | **Não.** Falta o placar nominal dos 33, inclusive K16 e K47. `execucoes.txt:2,30`, `vPRONTO6_r.txt:14` |
| **11. Sombra dos juízes e do passivo** | “51.953 julgadas de cada lado, 82 chaves comparadas e zero blocos diferentes” e “série completa por volta”. `vPRONTO6_r.txt:15` | **S** `bin/sombra.sh --rodar e4c_f5_pronto_item11.py` em MAIN e F5. **NAO EXISTE na cópia** o script externo nomeado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:33,35,45` | **Não.** A prova é a execução 4, que registra o recorte como fechado em `9e90d`. Série completa por volta **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:12,33` |
| **12. Sombra das decisões por porta** | “141 tentativas, 152 decisões iguais e rollback conferido em 141”, em amostra. `vPRONTO6_r.txt:16` | **S** `bin/sombra.sh --rodar e4c_f5_pronto_item12.py`. **NAO EXISTE na cópia** o script externo nomeado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:29,35,45` | **Não.** A execução 4 fecha a amostra em `9e90d`. O RELATO limita a medida a até 20 por porta e empresa. `execucoes.txt:29`, `app/docs/RELATO.md:12,29` |
| **13. Entregues e exportadas** | “comparar por PK as decisões de validar, recusar e materializar com a guarda de competência entregue”. `vPRONTO6_r.txt:17,188` | **S** `bin/sombra.sh --rodar e4c_f5_ab_entregue.py` e `bin/sombra.sh --rodar e4c_f5_6a1_v2.py` são as sondas externas registradas. **NAO EXISTE na cópia** a sonda dedicada à lista geral das decisões alteradas em exportadas. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:66,78,90`, `vPRONTO6_r.txt:17,188` | **Não.** A execução 4 cobre A/B entregue e 6-A1 nos recortes medidos. Lista geral por PK **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:27,66,78` |
| **16. Suíte inteira pré-Opus** | “Placar nominal da execução 1, recibo da Pista 2 e precedência da publicação completa”. `vPRONTO6_r.txt:20` | **T**, suíte inteira `bin/suite.sh --dir <cópia> --motivo "pre-opus E4C-F5"`. O executor chama `manage.py test` com os rótulos da régua. `bin/suite.sh:23–33,159,316–325`, `bin/regua.sh:23` | **Sim** para a suíte inteira verde no integrado. **Não** para os três recibos adicionais. `execucoes.txt:2`, `vPRONTO6_r.txt:20` |
| **20. RED da partição do app** | “Mordidas nominais no hash final” de `a1_app_coluna` e `a1_app_texto_do_ato`. `vPRONTO6_r.txt:24,189` | **T** `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao`. **M** `CORTES_F2_MUTANTE=a1_app_coluna` e `a1_app_texto_do_ato`, um por rodada, definidos no próprio teste. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:45–53`, `app/chamados/tests/cortes_f2_f9_apoio.py:77–87` | **Não.** Falta o resultado com cada seletor ativo. `execucoes.txt:2,30` |
| **22. RED do fio** | “três variantes dirigidas, coluna, texto do ato e composição local”. `vPRONTO6_r.txt:26,190` | **T** `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_GUARDA_fio_instante_do_ato_texto_da_projecao` e `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_MORDE_fio_composicao_local`. **M** `CORTES_F2_MUTANTE=a1_fio_coluna`, `a1_fio_texto_do_ato` e `a1_fio_composicao_local`, definidos no teste. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71`, `app/chamados/tests/cortes_f2_f9_apoio.py:77–87` | **Não.** Faltam as três mordidas nominais. `execucoes.txt:2,30` |
| **27. Contagem preservada no vigia** | “Comparação antes × depois com a mesma fixture”, com `(0,2)`, `(1,2)`, `(1,1)` e `(1,3)`. `vPRONTO6_r.txt:31,191` | **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_divergencia_e_universo`. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38` | **Não.** O `rc=0` é do integrado e falta a execução da mesma fixture contra a base. `execucoes.txt:2`, `vPRONTO6_r.txt:31` |
| **28. RED de consultas do vigia** | “3 SELECTs de decisões para 5 perguntas em lotes de 2” e “RED nominal contra a carga anterior”. `vPRONTO6_r.txt:32,192` | **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_RED_decisoes_em_lotes_de_chunk`. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` | **Não.** Falta o RED da carga anterior e o recibo nominal do controle final. `execucoes.txt:2`, `vPRONTO6_r.txt:32` |
| **32. Triagem de `casefold`** | “Falta decisão motivada para o Opus” e “o destino do sobrevivente”. `vPRONTO6_r.txt:36,171` | **D** decidir e publicar o destino. **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_diferenca_so_de_caixa_conta`. **M** seletor de `casefold` **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/chamados/services/vigia_texto_resposta.py:19–25`, `app/docs/RELATO.md:94,1374,1377` | **Não.** O verde agregado não resolve a divergência de pareceres. A mordida específica pedida pelo aval está **[nao verificado]**. `execucoes.txt:2,30`, `aval.txt:7`, `app/docs/RELATO.md:94` |
| **39. REDs 3 × 6 da A** | “11 comparativos de lote sobre 9 K e um O261 por candidata” e “Placar nominal e RED da passagem”. `vPRONTO6_r.txt:43,193` | **T** módulo `chamados.tests.test_e4c_f5_voltaA_lotes`. Rótulos por classe incluem `chamados.tests.test_e4c_f5_voltaA_lotes.TarefaEmLoteTest.test_RED_k1_tarefa`, `chamados.tests.test_e4c_f5_voltaA_lotes.CarregadoresEmLoteTest.test_RED_k16_pendentes_s128`, `chamados.tests.test_e4c_f5_voltaA_lotes.O261PorCandidataTest.test_RED_o261_selects_de_decisoes_por_candidata` e `chamados.tests.test_e4c_f5_voltaA_lotes.BecoC11EmLoteTest.test_RED_k2_c11_vivas_do_beco`. **M** seletores A do 10. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23–240`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,58–71` | **Não.** Faltam placar por método e RED da passagem. `execucoes.txt:2,30`, `vPRONTO6_r.txt:43` |
| **43. Tetos existentes E2 e C11** | “Diff de preservação e placar nominal”. `vPRONTO6_r.txt:47,194` | **T** `chamados.tests.test_e2_tarefa_do_dia.SeloDeterminismoERelogio.test_lote_tem_queries_constantes` e `escala.tests.test_f3_beco1_folga_na_lista.F3Beco1NaListaTest.test_MORDE_F5_queries_nao_crescem_com_os_dias`. **D** conferir o diff base → final. `app/chamados/tests/test_e2_tarefa_do_dia.py:876,913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:41,378–402` | **Não.** O verde agregado não fornece diff nem placar nominal. `execucoes.txt:2`, `vPRONTO6_r.txt:47` |
| **44. O261 por candidata, teto 9** | “colher o teto 9 e as contagens de SELECT para 3 e 6 candidatas”. `vPRONTO6_r.txt:48,195` | **T** `chamados.tests.test_o261_julgamento_desfaz_carimbo.JulgamentoDesfazCarimboSemLastroTest.test_custo_por_candidata_e_CONSTANTE` e `chamados.tests.test_e4c_f5_voltaA_lotes.O261PorCandidataTest.test_RED_o261_selects_de_decisoes_por_candidata`. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:90,187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` | **Não.** Falta a remedição nominal final dos dois testes. `execucoes.txt:2`, `vPRONTO6_r.txt:48` |
| **47. Quatro casos verdes do chokepoint** | “1 e 3 furos × fecha e fica” e “colher as contagens por cenário”. `vPRONTO6_r.txt:51,196` | **T** `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_ela_fecha`, `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ela_fecha`, `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_fica` e `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ficam`. `app/ponto/tests/test_selo_chokepoint_escrita.py:298,467–475,502–510` | **Não.** Falta o número da rodada final por cenário. `execucoes.txt:2`, `vPRONTO6_r.txt:51` |
| **48. Atribuição SQL e escritores novos** | “Captura final por SQL, verbo e sítio”, com zero `INSERT/UPDATE/DELETE/SAVEPOINT` a mais. `vPRONTO6_r.txt:52,197` | **T** quatro rótulos do 47, cuja captura alimenta a contagem. **NAO EXISTE na cópia** comando dedicado ao relatório comparativo completo por SQL, verbo e sítio. As anotações apontam a captura anterior. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267,438–510` | **Não.** O verde verifica os tetos. A atribuição final da diferença permanece **[nao verificado]**. `execucoes.txt:2`, `vPRONTO6_r.txt:52` |
| **50. Onze famílias verdes** | “saída nominal cruzada com a partição do 49”. `vPRONTO6_r.txt:54,198` | **T** módulos `chamados.tests.test_pergunta_viva` e `chamados.tests.test_e4c_f5_duble_conforme`. Rótulos existentes incluem `chamados.tests.test_pergunta_viva.PerguntaVivaOrdemTest.test_0_validada_morre` e `chamados.tests.test_e4c_f5_duble_conforme.DublePerguntaConformeTest.test_contagem_dos_asserts_igual_ao_main`. **D** cruzar os 11 e os 6 métodos com o inventário do 49. `app/chamados/tests/test_pergunta_viva.py:15–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:38–87`, `aval.txt:7` | **Não.** Faltam saída nominal e vínculo por família. `execucoes.txt:2`, `vPRONTO6_r.txt:54,198` |
| **52. Medida de `respondida`** | “MAIN × F5 na property `respondida`, pelo mesmo PK, nos dois sentidos da divergência”. `vPRONTO6_r.txt:56,199` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. O teste `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_3791_respondida_texto_da_projecao` cobre a fixture. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92,782`, `app/chamados/models.py:3838–3844`, `app/chamados/tests/test_e4c_f5_opus_models.py:75,87–89` | **Não.** A comparação de frota registrada é anterior. `execucoes.txt:2,29`, `app/docs/RELATO.md:782` |
| **53. Medida de `sem_veredito_entre`** | “comparando os PKs devolvidos” por `DisputaSupervisao.sem_veredito_entre`. `vPRONTO6_r.txt:57,200` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. A fixture tem o rótulo `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_2004_sem_veredito_entre_texto_da_projecao`. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92`, `app/chamados/models.py:2002–2027`, `app/chamados/tests/test_e4c_f5_opus_models.py:75–81` | **Não.** Falta o comparativo final dos PKs. `execucoes.txt:2,29`, `vPRONTO6_r.txt:57` |
| **54. Medida de `estado`** | “comparando o rótulo de cada PK”. `vPRONTO6_r.txt:58,201` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. A fixture tem o rótulo `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_3235_estado_texto_da_projecao`. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92`, `app/chamados/models.py:3249–3268`, `app/chamados/tests/test_e4c_f5_opus_models.py:75,83–85` | **Não.** Falta o comparativo final por rótulo e PK. `execucoes.txt:2,29`, `vPRONTO6_r.txt:58` |
| **57. Catraca publicada por volta** | “36 fora das permitidas na base, 24 na A, 10 na B, 0 na C e 0 em `9e90d`, com 44 reconhecidas no final”. `vPRONTO6_r.txt:61` | **T** `chamados.tests.test_e4c_f5_selo_chamada.SeloChamadaTest.test_producao_respeita_catraca` para o estado atual. Para a série, o RELATO registra o scanner do selo sobre cada árvore obtida por `git archive`. Comando dedicado à série **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160,188–193`, `app/docs/RELATO.md:13,37` | **Não para a série.** A execução 4 registra o item fechado. A origem do 19 histórico está **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:12–13,37` |
| **59. Limite da exceção de folha** | “diff restrito ao prefetch e ao argumento composto do juiz”. `vPRONTO6_r.txt:63` | **D** conferência do diff registrado de `app/folha` entre MAIN e F5. O RELATO identifica `59_diff_folha_9e90d.txt` e as duas âncoras. Teste dedicado ao diff **NAO EXISTE na cópia**. `app/docs/RELATO.md:12,35,58`, `app/folha/previa.py:64–67,139–146` | **Não.** A prova do fechamento é o diff da execução 4. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| **60. Prévia da competência 11** | “Q1 `montar_previa` na competência 11, com a mesma foto e relógio, por colaborador e dia”. `vPRONTO6_r.txt:64,202` | **S** envelope `bin/sombra.sh --rodar`, chamando `montar_previa`. Sonda Q1 específica **NAO EXISTE na cópia**. `bin/sombra.sh:13–14`, `app/folha/previa.py:28,64–67,135–146`, `app/docs/RELATO.md:782` | **Não.** Os 4/450/5.861 e o diff zero são da cópia anterior. `app/docs/RELATO.md:782`, `execucoes.txt:2,29` |
| **73. Medida vigente de display** | “universo 51.953, erros 0, literal 16, LIKE 0 e CRU 0”. `vPRONTO6_r.txt:77` | **S** `bin/sombra.sh --rodar`. O RELATO identifica `f5_property_e_display.py` como sonda externa e `item73_display.out` como a saída válida. A versão corrigida do script **NAO EXISTE na cópia**. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:12–13,58,749` | **Não.** A prova válida é `item73_display.out` na execução 4. A primeira rodada teve 36.212 erros de instrumento. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| **79. Selo do par inteiro verde e mutantes** | “controle verde e mordidas nominais de `limpa_sem_ato`, `preenche_e_desfaz` e `escritor_novo` em ambos os modos”, mais `v_ato_sem_coluna`. `vPRONTO6_r.txt:83,203` | **T** `chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest.test_escritores_fecham_o_par` e `chamados.tests.test_e4c_f5_volta3_selos.SelosVolta3Test.test_RED_validacao_iniciada_pelo_ato_tambem_grava_a_coluna`. **M** `F5_VOLTA_E_MUTANTE` com os três primeiros nomes e `F5_VOLTA3_MUTANTE=v_ato_sem_coluna` para o último. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:535–546`, `app/chamados/tests/test_e4c_f5_volta3_selos.py:41–59`, `app/chamados/tests/e4c_f5_voltaE_mutantes.py:5–25`, `app/chamados/tests/e4c_f5_volta3_mutantes.py:10,53–56` | **Não.** Os três da E têm mordida nos dois modos em `9e90d`. A mordida de `v_ato_sem_coluna` e o placar nominal integrado seguem **[nao verificado]**. `execucoes.txt:4–11,30` |
| **80. K47 e REDs da D na classe vii** | “esperados anteriores por adaptação e o resultado nominal final da D”. `vPRONTO6_r.txt:84,182,204` | **D** tabela antes → depois. **T** `chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k47_escolhas_sem_credor` e `chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest`, com 18 métodos. **M** `F5_VOLTA_B_MUTANTE=k47` e os 14 valores de `F5_VOLTA_D_MUTANTE`. A classe D aciona diretamente três seletores — `fecho_sem_lote`, `tela_sem_lote` e `lote_entrada_sem_prefetch`, nos métodos `test_RED_fecho_3x6`, `test_RED_tela_3x6` e `test_RED_lote_entrada_3x6`. Executor local dos demais **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:11–23`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:25,84–398`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:8–66` | **Não.** Faltam a tabela e o placar nominal final. O placar anterior registra oito sobreviventes D. `app/docs/RELATO.md:62,94`, `execucoes.txt:30`, `aval.txt:4–5` |
| **82. Sombra A/B e censo de frota** | “Frota 51.953, A=0 e B=0. Entregue 25.766, A=0 e B=0, zero sem dia apurável”. `vPRONTO6_r.txt:86` | **S** `manage.py vigiar_fio_morto` imprime A/B e universo da frota. Para o entregue, `bin/sombra.sh --rodar e4c_f5_ab_entregue.py` é a chamada do script externo registrado. O script **NAO EXISTE na cópia**. `app/chamados/management/commands/vigiar_fio_morto.py:194–199,231–234`, `bin/sombra.sh:13–14`, `app/docs/RELATO.md:27,35,90` | **Não.** A execução 4 registra ambos os recortes em `9e90d`, iguais aos de `99a74`. `execucoes.txt:29`, `app/docs/RELATO.md:12,27` |
| **83. Mutante `retry_reativa`** | “Mordida nominal final”. `vPRONTO6_r.txt:87,205` | **T** `chamados.tests.test_bug_par_volta.BugParVoltaTest.test_saida_primeiro_par_inteiro_retry_idempotente`. **M** `python -m chamados.tests.bug_par_mutantes --django --mutante retry_reativa`. `app/chamados/tests/test_bug_par_volta.py:40,61–63`, `app/chamados/tests/bug_par_mutantes.py:68–71,189–209,245` | **Não.** A mordida por asserção registrada é de `c8eb8eddf`. `app/docs/RELATO.md:98`, `execucoes.txt:30` |
| **84. Texto efetivamente mostrado nos 16** | “Renderização nominal MAIN × cópia final”. `vPRONTO6_r.txt:88,206` | **S** `bin/sombra.sh --rodar f5_display_16.py` em ambas as árvores. **NAO EXISTE na cópia** o script externo registrado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:769–780`, `app/templates/chamados/partials/_pergunta_card.html:31`, `app/templates/chamados/partials/_card_atendimento.html:106,143,186,394` | **Não.** O 16/16 é da comparação anterior. A execução 4 traz o placar do 73. `app/docs/RELATO.md:12,769`, `execucoes.txt:29` |
| **87. REDs e mutantes da devolução** | “Três REDs, um controle e quatro variantes de guarda, foto e cache”. `vPRONTO6_r.txt:91,207` | **T** `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_A_devolve_ato_e_deixa_trilha`, `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_B_limpa_coluna_mesmo_sem_validacao_vigente`, `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_PREFETCH_enxerga_validacao_posterior

[truncado em 20000 de 33739 caracteres]

## 21/11 10:19 · sessao 1706a82be043 · E4C-F5 · leitura · origem=Code

## A. Logs citados no RELATO

Os cinco itens têm uma referência comum em `c8eb8eddf`: “tabelas nominais inteiras em logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt, montadas pelo Codex sobre c8eb8eddf, amostra da sessao 9 de 9”. A mesma linha registra as lacunas de cada tabela. Conteúdo dos logs brutos [nao verificado]. `app/docs/RELATO.md:94`

| Item | Caminho, hash e trecho literal |
|---|---|
| **01 — Matriz e censo de entrada** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:43-59`: “[01 06] 15 sitios x selo: tabela nominal em ra.txt:43-59”. **`3308f7599`**, `logs/ESPINHA-BRIEFS/F5/brief_c3.md`: “Etapa A fechou -- matriz 15 sitios (logs/ESPINHA-BRIEFS/F5/brief_c3.md, amostra 9/9 arquivo:linha conferidos no 3308f7599), perguntas de lei 0, CENSO fechado pelo wrapper (disjuntor --censo, brief canonico logs/E4C-F5-CONSTROI/brief.md)”. O despacho do censo cita `logs/ESPINHA-BRIEFS/F5/c3/etapaA2`: “F5 Etapa A relancada sobre 3308f7599 (main POUSADO, F6F7+F9 no ar): Codex censo logs/ESPINHA-BRIEFS/F5/c3/etapaA2”. `app/docs/RELATO.md:94,1503,1511` |
| **40 — REDs e mutantes da B** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:84-121`: “[35 36 40 41] mapa K: 33 K com carregador e arquivo:linha + K40 (ra.txt:84-121)”. A rodada cita `logs/ESPINHA-BRIEFS/F5/vB/alvo_B.log` e registra “RED dos 30 testes novos: base ec7134368 FAILED 28/30 (vB/base_novos.log), B com o import de DIA OK 30/30 (vB/B_novos.log)” e “Volta B COMMITADA na copia: d0210a400”. Os caminhos relativos dos mutantes e da passagem aparecem como “Em curso: 15 mutantes (vB/mut/”, “Mutantes da B: 15/15 MORTOS” e “VOLTA B VERDE (00:5x, vB3/volta.log, worktree d9319a215 contra ec7134368)”. A conciliação 29×27 segue listada como falta. `app/docs/RELATO.md:94,1296` |
| **41 — REDs e mutantes da C** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:84-121`, pelo mesmo trecho “[35 36 40 41] mapa K”. Na volta C, o RELATO registra “57582ee14: 25 K + sobra dedup” e “triagem C no Codex leitura (logs/ESPINHA-BRIEFS/F5/vC/triagem)”. Para C+D, cita “F5 VOLTA C+D VERMELHA 01:32 (logs/ESPINHA-BRIEFS/F5/vC/volta.log, copia wt-f5-vC-copia-011631 = 9d386d1b1)”. Para o placar agregado, registra “mutantes tira-lote A+B+C 33 de 33 mordem pela medida” na **`a3c23d10b`**, cuja passagem cita `logs/ESPINHA-BRIEFS/F5/vC5/volta.log`. Na **`8938552e1`**, repete “33 de 33 mutantes MORDEM” com o caminho relativo `vC5/green_c6.log`. O vínculo nominal das três M segue listado como falta. `app/docs/RELATO.md:94,790,793,1283,1289` |
| **49 — Harness nas exceções** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:161-171`: “[49 51] 9 modulos de harness com asserts MAIN = copia (ra.txt:161-171)”. A volta A cita o recorte `logs/ESPINHA-BRIEFS/F5/vA/prova/` sobre o **MAIN `eb1e49c1e`** e registra a migração em **`cda246732`**: “helper + migracao + selos (a)(b) -> Codex construtor 20:09 (logs/ESPINHA-BRIEFS/F5/vA/duble/; base (b) medida no MAIN em base_asserts_eb1e49c1e.txt)”. A partição nominal das 11 famílias segue listada como falta. `app/docs/RELATO.md:94,1312,1314` |
| **51 — Décima segunda família** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt`, no bloco “[49 51]”, que nomeia “os dois RED-8 nomeados (test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco, test_red_8_custo_montar_chamados_fio_uma_e_cinco)” e registra “FALTA: a particao nominal das 11 familias e o ordinal da 12a.” Para os RED-8, há registros históricos em `logs/ESPINHA-BRIEFS/F5/v2/prova/RED_base.log`, **`9fa5ad533`**, com “red_8 (custo) ERROR na fixture”, e `logs/ESPINHA-BRIEFS/F5/opus_models/`, **`18bf86f01`**, com “GREEN Ran 54, 2 ERROR test_red_8_* da base”. Para o RED do fio sem prefetch, a **`d40f1c9dc`** cita o caminho relativo `quatro/red_sem_lote.log`: “3 not less than or equal to 1”. `app/docs/RELATO.md:94,1309,1320,1372` |

## B. Item 68

O **“contrato de fechamento”** é `test_porta_humana_de_disputa_presta_contas`, em `app/chamados/tests/test_contract_dedup_status.py:135–151`. O RELATO usa o rótulo “contrato de fechamento” no item 68 e nomeia o módulo ao registrar “FAIL test_contract_dedup_status (ancora de TEXTO 'self.respostas_sem_veredito()' deslocada pelo item D, classe i)”. `app/docs/RELATO.md:94,1283`

A tabela usa os blocos localizáveis na árvore final e o antes registrado no RELATO ou em comentários. O conteúdo exato da base `ebfdcc4dff23c46983c63e5a62e08cf671246a52`, os limites dos hunks e a completude do diff [nao verificado].

| Uso e bloco | Antes | Depois | Efeito | Só ato × coluna? |
|---|---|---|---|---|
| **Contrato — guarda de fechamento** | O RELATO registra a âncora antiga `self.respostas_sem_veredito()`. A expressão completa do assert antigo [nao verificado]. `app/docs/RELATO.md:1283` | O `assertIn` exige `_pend = self.respostas_sem_veredito(perguntas=perguntas)` entre as atribuições do corpo de `fechar`, via AST. `app/chamados/tests/test_contract_dedup_status.py:14–16,135–144` | O teste passa a cobrar a chamada com o lote já carregado. `fechar` carrega perguntas e decisões uma vez e entrega essas instâncias à guarda. `app/chamados/models.py:2068–2073`, `app/docs/RELATO.md:1289` | **Outro efeito.** Reuso do lote no fechamento e troca do esperado estrutural. `app/chamados/models.py:2068–2072`, `app/chamados/tests/test_contract_dedup_status.py:142–144` |
| **Expand — dublês da devolução** | A devolução passou a guardar por ato ou coluna e a fotografar a composta. A forma anterior dos dublês na base pedida [nao verificado]. `app/docs/RELATO.md:303` | `Pergunta.refresh_from_db` é um no-op. O dublê de `composta` devolve `validada_em` da própria coluna. A11 executa a devolução duas vezes e cobra `True`, depois `None`, resposta preservada, um ato e uma gravação. `app/chamados/tests/test_e4_expand.py:72–83,148–161` | Dá suporte à releitura e à foto da devolução na fixture. O dublê representa a mesma validação pelos dois lados. `app/chamados/tests/test_e4_expand.py:72–83,148–161`, `app/chamados/services/validacao.py:267–296` | **Só invariante**, no efeito identificado desse suporte à devolução. `app/chamados/services/validacao.py:267–296`, `app/chamados/models.py:4393–4399` |
| **Expand — A13, `values_list`** | “A13 por flat=” era um dos vermelhos do dublê. A assinatura anterior exata [nao verificado]. `app/docs/RELATO.md:303` | `values_list(*campos, flat=False)` devolve lista plana com `flat=True` e lista de tuplas no outro ramo. `app/chamados/tests/test_e4_expand.py:178–183` | Acomoda a consulta que monta o conjunto de donos para a trava do lote. `app/chamados/services/disputa_emissao.py:1642–1647` | **Outro efeito.** Compatibilidade do dublê com a consulta da trava. `app/chamados/tests/test_e4_expand.py:178–183`, `app/chamados/services/disputa_emissao.py:1642–1647` |
| **Expand — A13, releitura da reabertura** | “Pergunta validada so no ATO era limpa sem linha REABERTURA”. A forma anterior do dublê de `objects` [nao verificado]. `app/docs/RELATO.md:294` | `objects.filter(pk__in=...).prefetch_related(...)` devolve a pergunta se o PK estiver no lote. `app/chamados/tests/test_e4_expand.py:189–192` | Dá suporte à releitura pela composta quando as colunas estão vazias, para registrar o ato compensador quando ainda há validação pelo ato. `app/chamados/services/disputa_emissao.py:1652–1683` | **Só invariante**, no efeito identificado da releitura. `app/chamados/services/disputa_emissao.py:1652–1683`, `app/chamados/models.py:4393–4399` |
| **Expand — A14, import de `fio`** | A14 isolado dava `ImportError`. O comentário diz que o import dependia de outro módulo já ter carregado `fio`. `app/docs/RELATO.md:303`, `app/chamados/tests/test_e4_expand.py:101–104` | O harness disponibiliza `chamados.services.fio` com o corpo de `recusa_par_ilegal` lido da fonte e os dois predicados de resposta no dublê de `juizes`. `app/chamados/tests/test_e4_expand.py:92–104` | Permite executar A14 isoladamente pela guarda do par **instante de resposta × texto**. `app/chamados/tests/test_e4_expand.py:101–104,218–225`, `app/chamados/services/disputa_emissao.py:1689–1707`, `app/chamados/services/fio.py:1704–1722` | **Outro efeito.** Independência do import e exercício da guarda de resposta da L-130. `app/chamados/tests/test_e4_expand.py:97–104`, `app/chamados/services/fio.py:1704–1722` |

**B.3 — Mudança de assert.** Sim, no contrato. A âncora antiga registrada era `self.respostas_sem_veredito()`. O esperado final é a atribuição com `perguntas=perguntas`, procurada no AST do corpo de `fechar`. No Expand, os blocos identificados adaptam a instrumentação. Remoção, relaxamento ou troca de esperado no diff do Expand [nao verificado]. `app/docs/RELATO.md:1283`, `app/chamados/tests/test_contract_dedup_status.py:135–144`, `app/chamados/tests/test_e4_expand.py:148–161,178–225`

- **A 01** | **log citado**. `app/docs/RELATO.md:94,1503,1511`
- **A 40** | **log citado**. `app/docs/RELATO.md:94,1296`
- **A 41** | **log citado**. `app/docs/RELATO.md:94,790,793,1283,1289`
- **A 49** | **log citado**. `app/docs/RELATO.md:94,1312,1314`
- **A 51** | **log citado**. `app/docs/RELATO.md:94,1309,1320,1372`
- **B — Contrato de fechamento** | **outro efeito: reuso do lote de perguntas e decisões no fechamento, com troca do esperado estrutural. PAREI.** `aval.txt:7`, `app/chamados/models.py:2068–2072`, `app/chamados/tests/test_contract_dedup_status.py:142–144`
- **B — `test_e4_expand.py`** | **outro efeito: compatibilidade de `values_list(flat=True)` com a consulta da trava e execução isolada de A14 pela guarda de instante × texto. PAREI.** `aval.txt:7`, `app/chamados/tests/test_e4_expand.py:92–104,178–183`, `app/chamados/services/disputa_emissao.py:1642–1647,1689–1707`

## 21/11 10:18 · sessao 7556fe64fda6 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO — RESPOSTAS-2111-1010, item 2.** As sete referências precisam ser corrigidas antes da construção. A unidade declarada é o mutante, e o código aponta cinco comparações alteradas. `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:14`, `e4c_f5_voltaD_mutantes.py:23`, `e4c_f5_voltaD_mutantes.py:32`.

## 1. As premissas conferem? NAO

Nenhuma das sete linhas citadas contém a comparação mutada. Nos cinco pontos efetivos, há uma sequência permitida pelo código que produz a divergência na instância. O escritor grava a coluna e o ato no mesmo `atomic`. O leitor conserva a instância do primeiro `SELECT` e recebe as decisões do segundo, diretamente ou via prefetch. A composta usa essas decisões para calcular a validação vigente. `validacao.py:161`, `disputa_emissao.py:1478`, `disputa_emissao.py:1532`, `models.py:4361`, `models.py:4370`.

| Linha do aval | Comparação efetiva e sequência alcançável |
|---|---|
| `acoes_disputa.py:344` | **SIM em `:349`.** O `get` de `:345` carrega `perg` com a coluna nula. `validar_pergunta` confirma a validação pelo `_carimbar_validacao` antes da consulta às decisões. A composta vê a assinatura e devolve `info`. A troca pela coluna passa pela guarda. `acoes_disputa.py:345`, `acoes_disputa.py:349`, `validacao.py:746`, `validacao.py:760`. |
| `declaracao_texto.py:475` | **SIM em `:229`, pela projeção.** `:475` escolhe o caminho que carrega as perguntas com prefetch em `:476`. `_carimbar_validacao`, via `validar_pergunta`, confirma entre o `SELECT` das perguntas e o prefetch das decisões. Com a coluna ainda nula na instância, a composta produz `aceita=False` e o mutante produz `True`. `declaracao_texto.py:229`, `declaracao_texto.py:280`, `declaracao_texto.py:475`, `declaracao_texto.py:480`, `validacao.py:760`. |
| `declaracao_texto.py:634` | **SIM na mesma `:229`, pelas propostas.** `:634` consulta o memo por fato e `:636` carrega as perguntas e as decisões. O mesmo escritor confirma entre esses dois `SELECTs`, com efeito em `aceita`. `aplicar_declaracao` seleciona pelo `followup_id` e refaz a guarda sob trava, então o resultado da aplicação por si só deixa passar essa diferença. `declaracao_texto.py:634`, `declaracao_texto.py:639`, `declaracao_texto.py:651`, `declaracao_texto.py:658`, `declaracao_texto.py:670`, `validacao.py:760`. |
| `materializacao.py:195` | **SIM em `:203`.** `:195` busca a irmã `q`. No caminho da tela, `validar_pergunta(q.pk, ..., confirmar_implausivel=True)` pode confirmar a assinatura pelo `_carimbar_validacao` entre o `SELECT` de `q` e a leitura das decisões em `:200`. A composta pula o juiz da irmã. A coluna antiga, nula, faz o mutante chamar o juiz. A confirmação precisa passar pelo veto do U163, que antecede o carimbo. `materializacao.py:145`, `materializacao.py:195`, `materializacao.py:200`, `materializacao.py:203`, `validacao.py:714`, `validacao.py:720`, `validacao.py:760`. |
| `materializacao.py:925` | **SIM na mesma `:203`.** `:925` abre o ramo da testemunha da tela e `:928` chama `par_do_intervalo`. É a mesma sequência, com a confirmação da irmã entre o `SELECT` de `q` e o das decisões. O efeito verificável é passar ou cair na recusa por implausibilidade da irmã. A original ainda pode cair em outra guarda do par. `materializacao.py:925`, `materializacao.py:928`, `materializacao.py:195`, `materializacao.py:200`, `materializacao.py:203`, `materializacao.py:231`, `validacao.py:760`. |
| `validacao.py:1074` | **SIM em `:1082`.** `:1074` inicia a carga do lote. `_carimbar_validacao`, via `validar_pergunta`, confirma entre o `SELECT` das perguntas e o prefetch das decisões. A original devolve `Ja validada.` na entrada. A coluna antiga, nula, leva o mutante à reclassificação. `validacao.py:760`, `validacao.py:1074`, `validacao.py:1082`, `validacao.py:1085`. |
| `validacao.py:1102` | **SIM em `:1104`.** Uma resposta humana passa pela guarda pública enquanto a pergunta ainda está viva. O lote valida a pergunta e o `refresh_from_db()` de `:1103` lê a coluna preenchida. Antes da consulta fresca às decisões, `responder_pergunta_disputa_core` relê sob trava, zera a coluna e grava `RESPOSTA` que desfaz a validação. A original põe o item em `recusados`. O mutante conta a validação lida no refresh. `views.py:1239`, `views.py:1278`, `disputa_emissao.py:1481`, `disputa_emissao.py:1503`, `disputa_emissao.py:1535`, `validacao.py:1096`, `validacao.py:1103`, `validacao.py:1110`. |

## 2. Colide com aval vigente? NAO

Acrescentar os testes de intercalação é compatível com os controles da classe vii, que afirmam o par ato × coluna após a escrita. Os testes atuais recarregam a pergunta para afirmar o par, enquanto as sequências acima observam uma instância já carregada durante uma segunda leitura. A fonte da decisão continua sendo a composta. `RELATO.md:283`, `PROMPTS.md:2688`, `test_e4c_f5_voltaD_portas.py:45`, `test_e4c_f5_voltaD_portas.py:60`, `models.py:4382`, `models.py:4393`. [nao verificado]

## 3. Alguma condição não fecha no literal? SIM

- **A unidade é o mutante, com um teste que o mate.** As sete referências descrevem caminhos de cinco trocas. Projeção e propostas chegam à mesma guarda de `declaracao`. A busca da irmã e a chamada da tela chegam à mesma guarda de `irma_implausivel`. `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:13`, `declaracao_texto.py:229`, `declaracao_texto.py:480`, `declaracao_texto.py:635`, `materializacao.py:195`, `materializacao.py:203`, `materializacao.py:928`. [nao verificado]
- **Nenhuma das cinco trocas exige forjar a divergência.** Para `lote_releitura`, o caso positivo atual usa texto e força `LOTE_A`. Esse arranjo não demonstra a entrada real no lote, pois o classificador exige `hora` ou `data_hora` com hora legível. A sequência da Q1 cabe numa pergunta de hora que entre pela classe A real e cuja assinatura fique até o refresh. O teste que apaga só a coluna é um diagnóstico separado. `test_e4c_f5_voltaD_portas.py:40`, `test_e4c_f5_voltaD_portas.py:350`, `test_e4c_f5_voltaD_portas.py:376`, `validacao.py:1213`, `test_fila_validar_lote.py:103`. [nao verificado]
- **O seletor atual precisa ser acionado pelo teste ou pela bateria.** Definir `F5_VOLTA_D_MUTANTE` só aplica a troca dentro de `mutar_sitio` com o nome correspondente. Os três usos atuais nesse módulo selecionam mutantes de desempenho. A morte dos cinco no hash informado é [nao verificado]. `e4c_f5_voltaD_mutantes.py:49`, `test_e4c_f5_voltaD_portas.py:163`, `test_e4c_f5_voltaD_portas.py:199`, `test_e4c_f5_voltaD_portas.py:344`. [nao verificado]

## 4. Toca exportada, dinheiro ou `!` fora do declarado? NAO

O item autoriza testes e o seletor troca a função em memória, restaurando-a ao sair. Os cenários cabem em competência de teste aberta. O fluxo existente pode conferir a entrega e recalcular por evento, e a classificação do lote lê a tolerância do motor. O item não exige escrita em competência exportada, alteração no motor ou em escala/vínculo de produção, nem exclusão de arquivo. `aval.txt:4`, `aval.txt:9`, `e4c_f5_voltaD_mutantes.py:55`, `validacao.py:757`, `validacao.py:769`, `validacao.py:1147`, `CLAUDE.md:265`. [nao verificado]

Identidade do hash por metadados Git e ocorrência das intercalações em produção [nao verificado].

- **Q1 — PROCEDE (aval devolvido, item 2).** `acoes_disputa.py:344`, `acoes_disputa.py:349`.
- **Q2 — NAO PROCEDE.** `RELATO.md:283`, `models.py:4393`.
- **Q3 — PROCEDE (aval devolvido, item 2).** `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:13`.
- **Q4 — NAO PROCEDE.** `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:55`.

## 21/11 06:09 · sessao f238c58b558f · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

**A rejeição tira a FALTA da cobertura lida pelo E6 e aciona o cartório no próprio `save`. A retenção do export vem do veredito da célula.** O commit libera um consumidor de chamados e perguntas. A saída fornecida mede a foto B antes desse consumidor e deixa o resultado pós-commit em produção [nao verificado]. `app/ponto/turnos.py:2523`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/folha/export.py:1273`, `sonda_ab4.py:354`, `sonda_ab4.py:378`.

Analisei a cópia em modo somente leitura. A cópia está sem metadados Git, então a correspondência com o hash informado fica [nao verificado]. As referências aos anexos contam as linhas a partir do início de cada trecho colado.

### 1. Condição exata de `sem_lavratura`

A linha do dia precisa existir, `isencao` precisa ser falsa, `ausencia` precisa ser `None` e **tanto `minutos_realizados` quanto `pago_h` precisam ser `None`**. Zero numérico passa pelo leitor. O E6 incrementa `dia_sem_lavratura_no_espelho` e pula a comparação. A sonda dá a esse ramo o rótulo `sem_lavratura`. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:336`, `sonda_ab4.py:167`.

- **`minutos_realizados`** é uma chave do dicionário escrita pelo montador do espelho a partir de `realizado_do_dia`. O cálculo retorna `None` quando nenhum turno pareado tem `data_turno` igual à data. O pareador lê batidas não retratadas e usa a ata e a escala como insumos. `app/ponto/services/espelho.py:267`, `app/ponto/services/espelho.py:628`, `app/ponto/services/espelho.py:689`, `app/ponto/services/espelho.py:797`, `app/ponto/turnos.py:719`, `app/ponto/turnos.py:795`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1944`.
- **`pago_h`** é outra chave escrita pelo montador. Ela lê `DiaPago` (`ponto_diapago`), com `versao='motor'` e `tipo='dia'`. `pago_do_dia` devolve `None` se faltar a linha ou se todas as rubricas que ele testa forem zero. Se passar pelo teste, soma `horas_trabalhadas`, `horas_folga_trabalhada` e `detalhe['folga_feriado_min'] / 60`. Uma falha na consulta também deixa o mapa vazio no espelho. `app/ponto/services/espelho.py:420`, `app/ponto/services/espelho.py:825`, `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:854`, `app/ponto/services/dia_pago.py:861`, `app/ponto/services/dia_pago.py:890`, `app/ponto/management/commands/apurar_furo_da_exportada.py:179`.
- **Escritas em `DiaPago` na cópia.** `dia_pago.lavrar` substitui as linhas no recálculo gravável ou no backfill. `retratar_exportada` atualiza os minutos da grade e pode criar linhas com rubricas zeradas. `apurar_furo_da_exportada` só atualiza `furo_apurado` e `lavrado_em`. Há também três scripts de restauração que repõem linhas. `app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/dia_pago.py:472`, `app/ponto/services/dia_pago.py:538`, `app/ponto/services/dia_pago.py:671`, `bin/restore_relavratura_10_2026.py:85`, `bin/restore_o263_comp10.py:54`, `bin/restore_badge_linha_09_10.py:61`.

### 2. O que a rejeição escreve e quando

`decidir` abre um `atomic`. `mudar_estado(..., _acorda=False)` grava `Ausencia.status='rejeitada'`, e o `save` dos carimbos pode disparar os receivers de novo. A cobertura do espelho vem de uma consulta a `Ausencia` para cobrança, que já exclui a rejeitada na leitura dentro da transação. No fim do ato, `_acordar_projecao` invalida `FechamentoMensal.derivado_em` nas competências afetadas. Essa atualização só escreve os carimbos do fechamento. `app/ponto/services/ausencia.py:975`, `app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1104`, `app/ponto/services/ausencia.py:1123`, `app/ponto/services/ausencia.py:195`, `app/ponto/services/fechamento.py:1652`, `app/ponto/services/espelho.py:596`, `app/ponto/services/espelho.py:859`, `app/ponto/turnos.py:2530`, `app/ponto/turnos.py:2768`.

**Cartório, síncrono.** O `post_save` de `Ausencia` chama `_julgar_dias`, que calcula as datas via `_dias_ausencia` e chama `cartorio.julgar_celula` dentro de `engolir`. `engolir` conta e registra a exceção sem propagá-la. `em_julgamento()` consulta uma flag da thread e corta a reentrada. `julgar_colab` liga essa flag e a limpa no `finally`. Para uma célula elegível, o cartório relê a cobertura, considera-a na impressão e grava `veredito`, `protestos`, `ata` e `ausencia` na célula quando há lavra. Dia futuro, célula inexistente, turno de hoje ainda em curso e impressão já em dia têm guardas próprias. `app/ponto/signals.py:85`, `app/ponto/signals.py:115`, `app/ponto/signals.py:143`, `app/core/observ.py:73`, `app/core/observ.py:86`, `app/ponto/services/cartorio.py:59`, `app/ponto/services/cartorio.py:993`, `app/ponto/services/cartorio.py:1151`, `app/ponto/services/cartorio.py:1385`, `app/ponto/services/cartorio.py:1427`, `app/ponto/services/cartorio.py:1898`, `app/ponto/services/cartorio.py:2073`, `app/ponto/portas/celula.py:1090`, `app/ponto/portas/celula.py:1194`.

**Consumidor, após o commit.** Outro `post_save` registra `transaction.on_commit(_depois)`. Ele devolve perguntas absorvidas quando cabível, reconcilia cobranças e fecha chamados de ausência decidida. Uma mudança de estado de chamado com dia identificável pode acionar o cartório de novo. O callback da ausência não chama diretamente o recálculo que grava `DiaPago`. `app/ponto/signals.py:250`, `app/ponto/signals.py:275`, `app/chamados/services/materializacao.py:1154`, `app/chamados/services/materializacao.py:1180`, `app/chamados/signals.py:189`, `app/chamados/signals.py:210`, `app/ponto/services/fechamento.py:1807`.

### 3. O que a sonda impede

- **O rollback corta o `on_commit`.** A foto B chama `decidir`, relê o status gravado e mede export e E6 antes de reverter o savepoint. O `atomic` externo também termina em rollback. O cartório síncrono pode gravar e ser lido dentro de B, antes de suas escritas serem desfeitas. `sonda_ab4.py:224`, `sonda_ab4.py:289`, `sonda_ab4.py:354`, `sonda_ab4.py:378`, `sonda_ab4.py:470`, `app/ponto/signals.py:143`, `app/ponto/signals.py:272`.
- **Os patches não acionam `em_julgamento()`.** A sonda silencia push, bloqueia as operações de socket e storage listadas e intercepta `Logger.handle`. O wrapper de `linhas_do_periodo` chama o leitor original e registra qualquer exceção antes de relançá-la. O JSON informa 19 conclusivos e zero falhas reportadas. `sonda_ab4.py:409`, `sonda_ab4.py:420`, `sonda_ab4.py:429`, `sonda_ab4.py:440`, `sonda_ab4.py:459`, `app/chamados/services/disputa_emissao.py:1777`, `app/ponto/services/cartorio.py:59`, `saida_sonda4.json:735`.

### 4. Pós-commit em produção e duração [nao verificado]

**O mesmo estado é possível após um commit real.** Com a cobertura retirada, o E6 devolve `sem_lavratura` enquanto os dois valores continuarem `None`. A rejeição não agenda uma relavratura de `DiaPago` pelo seu caminho direto. O estado pode acabar quando o pareamento passar a devolver um turno, quando uma relavratura fornecer uma rubrica que sustente `pago_h` ou quando o dia voltar a ter cobertura ou isenção. Um evento de batida pode agendar recálculo após commit. A cópia também prevê recálculo pelo DP ou por comando e guarda de escrita para 09/2026 já exportada. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:336`, `app/ponto/services/ausencia.py:1123`, `app/ponto/services/fechamento.py:1652`, `app/ponto/registro_batida.py:176`, `app/ponto/registro_batida.py:195`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:558`, `app/config/crons.py:1148`.

**Não há prazo fixo pelo cron.** `processar_cartorio --apply` está configurado a partir das **06:27**, por empresa, e revisita células passadas. Esse caminho pode atualizar a célula que o export lê. Ele não executa a lavratura do `DiaPago`. O recálculo gravável é quem chama essa lavratura. Para os 30 dias da foto B, o estado depois dos callbacks, a retenção numa nova geração e a duração efetiva ficam [nao verificado]. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1898`, `app/ponto/services/fechamento.py:1191`, `sonda_ab4.py:354`, `sonda_ab4.py:470`.

### 5. O que o export retém

O export usa **outro predicado**. Para quem passa pelas guardas de isenção e desligamento, ele pode reter dia passado com vínculo sem célula, célula sem veredito nas condições do laço ou célula cujo `veredito` esteja em `VEREDITOS_ACUSAM`. Nesse último ramo, dispensa o dia se todos os protestos forem não cobráveis ou se o único protesto for `FURO_PARCIAL` e a ata confirmar furo só de pausa. Um motivo de espelho torna o colaborador `fora / furo_espelho`, e a montagem do TXT só inclui quem ficou `entra`. O teste de minutos pertence ao E6. `app/folha/export.py:1207`, `app/folha/export.py:1247`, `app/folha/export.py:1267`, `app/folha/export.py:1293`, `app/folha/export.py:1173`, `app/folha/export.py:1149`, `app/escala/models.py:1767`, `app/folha/export.py:968`, `app/folha/export.py:716`, `app/folha/export.py:725`, `app/ponto/management/commands/e6_oraculo.py:342`.

A sonda monta o TXT em memória com o fechamento gravado que carregou. A geração real passa pelo crivo da porta, classifica com fechamentos recalculados em leitura e cria um novo `ExportacaoDominio` com o conteúdo gerado. A foto B demonstra mudança na montagem da sonda. Uma mudança no TXT efetivamente entregue em produção fica [nao verificado]. `sonda_ab4.py:131`, `sonda_ab4.py:358`, `app/folha/views.py:130`, `app/folha/views.py:209`, `app/folha/views.py:267`, `app/folha/views.py:297`, `app/folha/models.py:174`.

**nao sei** — para esses dias em produção, o elo decisivo é a leitura pós-commit dos turnos e do `DiaPago` para o E6, e da `CelulaDia` para a retenção. A sonda leu os dois antes de executar o `on_commit`. `app/ponto/management/commands/e6_oraculo.py:263`, `app/folha/export.py:1273`, `app/ponto/signals.py:272`, `sonda_ab4.py:354`, `sonda_ab4.py:378`.

## 21/11 05:59 · sessao c2ebda6cddfe · E4C-F5-CONSTROI · leitura · origem=Code

As oito trocas comparam **a validação vigente pela leitura composta** com **a coluna `PerguntaDisputa.validada_em` da instância ORM**. A composta lê as decisões por `ato_em` e PK, incorpora a validação da coluna quando falta a linha correspondente e aplica os atos que desfazem a validação. A coluna é um `DateTimeField` anulável. Os oito sítios decidem pela presença do instante. [models.py:2659](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:2659) [models.py:4318](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4318) [models.py:4361](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4361) [models.py:4382](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4382)

O **sim** abaixo indica uma sequência de execução permitida pelo código da cópia. Uma gravação pareada pode terminar entre o SELECT da pergunta e o SELECT das decisões, deixando a coluna antiga na instância do leitor. A ocorrência dessas sequências e a existência de registros já divergentes em produção são [nao verificado]. [config/settings/saas.py:83](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/config/settings/saas.py:83) [models.py:911](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:911) [models.py:4361](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4361)

## Escritores e selos comuns aos oito sítios

| Escritor | Lado da coluna | Lado do ato |
|---|---|---|
| `_carimbar_validacao` | Preenche a coluna | Grava `VALIDACAO` para o mesmo PK, com o mesmo instante e no mesmo `atomic`. [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161) |
| `PerguntaDisputa.carimbar` com `resposta='validar'` | Preenche a coluna vazia quando há texto | A composta pode incorporar o carimbo como validação sintética. A porta restaura a instância e recusa pelo par se a coluna ficou preenchida e a composta ficou vazia. [models.py:3367](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3367) [models.py:3384](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3384) [models.py:4318](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4318) |
| `responder_pergunta_disputa_core` | Limpa a coluna sob a trava do colaborador | Grava `RESPOSTA` com `resposta_mantem_validacao=False` no mesmo ato. [disputa_emissao.py:1478](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1478) [disputa_emissao.py:1503](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1503) [disputa_emissao.py:1532](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1532) |
| `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` | Limpam a coluna | Gravam `DEVOLUCAO` no mesmo bloco da escrita. [validacao.py:284](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:284) [validacao.py:347](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:347) [validacao.py:365](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:365) |
| `reabrir_para_correcao`, incluindo a chamada de `recusar_pergunta`, e `ressuscitar_perguntas` | Limpam pelo `descarimbar_em_lote` | Gravam `REABERTURA` ou `RECUSA` no mesmo `atomic`. [disputa_emissao.py:1630](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1630) [disputa_emissao.py:1664](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1664) [validacao.py:938](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:938) [models.py:1646](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:1646) [models.py:1713](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:1713) |
| Nascimento em `PerguntaDisputa.abrir` e backfill E4b | O nascimento salva os campos recebidos. O backfill lê a projeção existente | Ambos registram as linhas faltantes com o instante da projeção. Resposta automática e resposta pela trilha preservam a validação vigente. [models.py:3229](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3229) [backfill_fio_morto_e4b.py:147](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/management/commands/backfill_fio_morto_e4b.py:147) [disputa_emissao.py:1711](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1711) [disputa_emissao.py:1733](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1733) |

O invariante é **validação vigente pelo ato ⇔ coluna preenchida**, definido em `par_ato_coluna`. `test_escritores_fecham_o_par` e `test_atos_que_desfazem_gravam_a_coluna` verificam o pareamento das escritas. O selo da Volta 3 verifica o sentido `VALIDACAO` física → coluna com o mesmo instante. O teste do carimbo sintético cobre uma resposta posterior que faria a composta perder a validação. Esses selos verificam as escritas. Nos caminhos travados abaixo, a trava do colaborador também protege o intervalo entre as leituras. [models.py:4393](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4393) [models.py:2875](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:2875) [test_e4c_f5_selo_par_ato_coluna.py:536](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:536) [test_e4c_f5_selo_par_ato_coluna.py:548](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:548) [test_e4c_f5_selo_par_ato_coluna.py:715](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:715) [test_e4c_f5_volta3_selos.py:12](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_volta3_selos.py:12)

## Os oito sítios

**1. `corrigir`.** A original testa `DecisaoPergunta.composta(perg).validada_em`. A mutante testa `perg.validada_em`. **Sim.** A correção carrega `perg` antes de consultar as decisões, sem trava nessa guarda. Se `validar_pergunta` confirmar a assinatura entre as consultas, a composta já vê o ato e devolve `info`, enquanto a coluna da instância continua nula. [e4c_f5_voltaD_mutantes.py:14](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:14) [acoes_disputa.py:344](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/acoes_disputa.py:344) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [validacao.py:760](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:760)

**Teste e fixture mínima.** `RED_corrigir` usa o auxiliar que carimba os dois lados ou limpa os dois por uma nova resposta. Para morder, usar uma pergunta de hora respondida e ainda sem validação, pausar a correção depois do `get`, validar o mesmo PK em outra transação e retomar. Exigir `info` e preservação da resposta e da assinatura. [test_e4c_f5_voltaD_portas.py:60](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:60) [test_e4c_f5_voltaD_portas.py:84](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:84) [acoes_disputa.py:349](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/acoes_disputa.py:349)

**2. `declaracao`.** `_resposta_validada` testa `DecisaoPergunta.composta(perg).validada_em is not None`. A mutante testa `perg.validada_em is not None`. **Sim, na projeção e nas propostas.** Esses chamadores chegam ao mesmo sítio via `_porta_recusaria_por_carimbo`. Uma validação confirmada entre o SELECT da pergunta e o prefetch das decisões faz a original devolver `aceita=False` e a mutante, `True`. As guardas das duas portas de escrita leem a pergunta depois de adquirir a trava do colaborador. [e4c_f5_voltaD_mutantes.py:16](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:16) [declaracao_texto.py:217](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:217) [declaracao_texto.py:280](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:280) [declaracao_texto.py:350](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:350) [declaracao_texto.py:475](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:475) [declaracao_texto.py:634](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:634) [declaracao_texto.py:655](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:655)

**Teste e fixture mínima.** `RED_declaracao` e `RED_aplicar_declaracao` montam pares alinhados. Usar um marco faltante, uma proposta e uma pergunta respondida. Confirmar a validação entre as duas consultas da proposta e verificar `aceita=False`. É preciso acrescentar essa asserção ou testar a projeção diretamente. `aplicar_declaracao` seleciona a proposta pelo `followup_id`, sem usar `aceita`, e refaz a guarda sob trava. [test_e4c_f5_voltaD_portas.py:103](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:103) [test_e4c_f5_voltaD_portas.py:122](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:122) [declaracao_texto.py:634](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:634) [declaracao_texto.py:651](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:651) [declaracao_texto.py:664](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:664)

**3. `irma_plantio`.** Com `irma_validada=True`, a original rejeita a irmã se `leitura_da_irma.validada_em is None`. A mutante usa `q.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** `_gravar_irma_do_par` chega a essa guarda com o colaborador travado. A busca carrega uma instância fresca da irmã dentro do ato. Vale também para o plantio pelo reparador do passivo. A fixture de `RED_irma_plantio` testa irmã com ambos os lados preenchidos ou ambos nulos. [e4c_f5_voltaD_mutantes.py:20](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:20) [materializacao.py:139](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:139) [materializacao.py:195](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:195) [materializacao.py:1766](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:1766) [materializacao.py:2153](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2153) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [passivo_validado.py:377](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/passivo_validado.py:377) [test_e4c_f5_voltaD_portas.py:213](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:213) [test_e4c_f5_voltaD_portas.py:228](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:228)

**4. `irma_implausivel`.** A original consulta o juiz de implausibilidade quando `leitura_da_irma.validada_em is None`. A mutante usa `q.validada_em is None`. **Sim, no caminho da tela.** A tela chama `par_do_intervalo` sem a trava do plantio. O admin pode confirmar a hora implausível da irmã depois do SELECT de `q`, antes do SELECT das decisões. A composta vê a assinatura e pula o juiz. A mutante vê a coluna antiga nula e chama o juiz. [e4c_f5_voltaD_mutantes.py:23](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:23) [materializacao.py:145](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:145) [materializacao.py:200](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:200) [materializacao.py:925](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:925) [validacao.py:720](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:720) [validacao.py:760](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:760)

**Teste e fixture mínima.** `RED_irma_implausivel` usa os mesmos pares alinhados de `RED_irma_plantio`, com `exigir_validada=False` e o juiz de implausibilidade substituído. Manter esse juiz e os demais controles da fixture. Começar com a irmã respondida e sem validação, gravar a assinatura pelo escritor real em outra instância depois da busca de `q` e antes da composta, e exigir que o par passe sem chamar o juiz da irmã. [test_e4c_f5_voltaD_portas.py:213](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:213) [test_e4c_f5_voltaD_portas.py:238](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:238) [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161)

**5. `materializar`.** A original testa `DecisaoPergunta.composta(p).validada_em is None`. A mutante testa `p.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** A primeira seleção exige coluna preenchida. O sítio mutado vem depois da trava e de um novo `get` por PK. `RED_materializar_ato_mudou_sob_trava` grava uma nova resposta em outra instância e, num subcaso, volta a validar. O `get` posterior faz o sítio receber `None` nos dois lados ou `AGORA` nos dois lados. [e4c_f5_voltaD_mutantes.py:25](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:25) [materializacao.py:2085](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2085) [materializacao.py:2142](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2142) [materializacao.py:2153](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2153) [test_e4c_f5_voltaD_portas.py:248](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:248) [test_e4c_f5_voltaD_portas.py:256](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:256)

**6. `validar_par`.** A original decide se precisa carimbar a irmã com `DecisaoPergunta.composta(q).validada_em is None`. A mutante usa `q.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** `validar_pergunta` já mantém a trava quando `_validar_o_par` busca a irmã e decide se chama `_carimbar_validacao`. `RED_validar_par` valida as duas metades e, no subcaso negativo, responde de novo à irmã, limpando ambas as faces. A própria fixture trava antes da chamada. [e4c_f5_voltaD_mutantes.py:27](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:27) [validacao.py:648](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:648) [validacao.py:655](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:655) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [validacao.py:786](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:786) [test_e4c_f5_voltaD_portas.py:272](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:272) [test_e4c_f5_voltaD_portas.py:284](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:284)

**7. `lote_entrada`.** A original testa `DecisaoPergunta.composta(perg).validada_em` para devolver `Ja validada.`. A mutante testa `perg.validada_em`. **Sim.** O lote carrega a coluna e depois faz o prefetch. Uma validação confirmada entre as consultas deixa a coluna nula na instância e a assinatura presente no prefetch. [e4c_f5_voltaD_mutantes.py:32](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:32) [validacao.py:1074](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1074) [validacao.py:1082](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1082) [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161)

**Teste e fixture mínima.** `RED_lote_entrada` monta os dois estados alinhados e força classe A. Usar uma única pergunta respondida e sem assinatura. Confirmar a validação por outra transação entre o SELECT principal e o prefetch. Exigir `Ja validada.` na recusa de entrada e `validadas=0`. Com a classe A forçada, a mutante chega à ação individual, que relê `ja_validada`, e o lote acaba contando a assinatura. [test_e4c_f5_voltaD_portas.py:324](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:324) [validacao.py:693](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:693) [validacao.py:1082](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1082) [validacao.py:1094](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1094)

**8. `lote_releitura`.** Depois da ação e do `refresh_from_db()` integral, a original testa `DecisaoPergunta.composta(perg).validada_em` para contar a validação. A mutante testa `perg.validada_em`. **Sim.** O refresh descarta o prefetch anterior. Uma nova resposta humana, cujo pedido passou pela guarda pública enquanto a pergunta ainda estava viva, pode terminar depois do refresh e antes da consulta às decisões. O core relê sob trava, limpa a coluna e grava a `RESPOSTA` compensadora. A original vê a validação desfeita e a mutante conserva o instante lido no refresh. [e4c_f5_voltaD_mutantes.py:35](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:35) [validacao.py:1096](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1096) [views.py:1239](/home/ronald/codex_ws/b0935c1f106d232dc

[truncado em 20000 de 23913 caracteres]

## 21/11 05:57 · sessao baf7883bee21 · - · - · origem=Ronald (tmux)

- **`corrigir` — sim, por intercalamento.** O original lê a validação vigente pela sequência. O mutante lê a coluna da instância carregada. O `GET` e a consulta às decisões acontecem separadamente, sem trava no leitor. Uma validação entre as duas consultas faz o original devolver `info` e o mutante passar pela guarda. Uma nova resposta após uma validação física permite o sentido inverso. A fixture atual valida antes de chamar a correção e exercita somente pares alinhados. Fixture mínima: carregar pergunta de hora respondida e ainda sem validação, intercalar outra transação que valide a pergunta, retomar a correção e exigir `info`, resposta e assinatura preservadas. [app/chamados/services/acoes_disputa.py:344] [app/chamados/models.py:2659] [app/chamados/models.py:4361] [app/chamados/models.py:4382] [app/chamados/services/validacao.py:161] [app/chamados/services/validacao.py:746] [app/chamados/services/disputa_emissao.py:1485] [app/chamados/services/disputa_emissao.py:1503] [app/chamados/services/disputa_emissao.py:1532] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:60] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:84]

- **`declaracao` — sim no leitor compartilhado de tela/propostas.** `_resposta_validada` testa a presença da validação composta, o mutante testa presença do campo. A projeção e as propostas carregam coluna e `decisoes` por consultas separadas sem trava. A validação canônica entre as duas pode alterar `aceita`. As duas guardas de escrita dos REDs consultam o alvo sob trava do colaborador. O `aplicar_declaracao` usa propostas antes da trava, mas filtra apenas por `followup_id`, então a divergência de `aceita` não muda os asserts atuais. A fixture comum entra com par alinhado. Fixture mínima: um marco faltante e uma pergunta respondida ainda sem validação quando sua coluna é carregada, intercalar o escritor de validação antes do prefetch das decisões e verificar `aceita=False` em `projecao_por_marco` ou `propostas_do_chamado`. [app/chamados/services/declaracao_texto.py:217] [app/chamados/services/declaracao_texto.py:280] [app/chamados/services/declaracao_texto.py:290] [app/chamados/services/declaracao_texto.py:350] [app/chamados/services/declaracao_texto.py:380] [app/chamados/services/declaracao_texto.py:471] [app/chamados/services/declaracao_texto.py:634] [app/chamados/services/declaracao_texto.py:650] [app/chamados/services/declaracao_texto.py:670] [app/chamados/services/validacao.py:161] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:60] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:103] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:122]

- **`irma_plantio` — não pelos escritores ordinários no trecho efetivo.** A guarda original testa a presença da validação composta da irmã `q`, o mutante sua coluna. A única passagem `irma_validada=True` vem de `_gravar_irma_do_par`. A irmã é buscada novamente dentro do ato sob trava do colaborador, nas entradas da validação, do materializador e do passivo. Os escritores pareados passam pela mesma trava de colaborador. A fixture constrói duas metades complementares e fixa as outras guardas, mas alterna entre `(VALIDACAO_EM, VALIDACAO_EM)` e `(None, None)`. Uma divergência persistida no banco de produção é [nao verificado]. [app/chamados/services/materializacao.py:139] [app/chamados/services/materializacao.py:195] [app/chamados/services/materializacao.py:1435] [app/chamados/services/materializacao.py:1758] [app/chamados/services/materializacao.py:2153] [app/chamados/services/validacao.py:746] [app/chamados/services/validacao.py:786] [app/chamados/services/passivo_validado.py:377] [app/chamados/services/passivo_validado.py:528] [app/chamados/models.py:2874] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:213] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:228]

- **Escritores e selos.** `_carimbar_validacao` escreve coluna e `VALIDACAO` com o mesmo instante no mesmo `atomic`. `carimbar` pode preencher a coluna e a leitura sintetiza `VALIDACAO` quando falta a linha correspondente, com recusa de `(False, True)` antes de salvar. Devoluções, resposta humana, reabertura/recusa e ressuscitação limpam a coluna e escrevem o ato compensador. `responder_automaticamente` e `carimbar_resposta_pela_trilha` escrevem RESPOSTA marcada para manter a validação e não alteram a coluna. O selo AST compara escrituras e atos nos dois sentidos, e há testes específicos para a recusa do carimbo sintético e os horários sob trava na reabertura/resposta. [app/chamados/services/validacao.py:161] [app/chamados/models.py:3367] [app/chamados/models.py:3384] [app/chamados/models.py:4318] [app/chamados/services/validacao.py:277] [app/chamados/services/validacao.py:347] [app/chamados/services/disputa_emissao.py:1503] [app/chamados/services/disputa_emissao.py:1532] [app/chamados/services/disputa_emissao.py:1642] [app/chamados/services/disputa_emissao.py:1664] [app/chamados/models.py:1702] [app/chamados/services/disputa_emissao.py:1708] [app/chamados/services/disputa_emissao.py:1728] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:536] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:548] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:715] [app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:38] [nao verificado]

A aplicação das trocas da bateria externa é [nao verificado]. O helper local só aplica a troca quando o teste entra no seu contexto com o nome do mutante. As chamadas da classe estão nos três testes de consultas. [app/chamados/tests/e4c_f5_voltaD_mutantes.py:49] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:163] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:199] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:344] [nao verificado]

## 21/11 05:56 · sessao 60b01236ac3f · - · - · origem=Ronald (tmux)

**`irma_implausivel`: sim, por uma corrida de leitura.** A busca da irmã carrega `q` sem prefetch de decisões. A tela chega a `par_do_intervalo` sem a trava do plantio e o ato é consultado depois da leitura da linha ORM. Uma irmã respondida e ainda não validada pode ser carregada com `q.validada_em = None`, e então o admin confirma sua hora implausível e grava a validação antes da consulta às decisões. O registro físico e a coluna são gravados juntos pelo escritor, mas o leitor retém a coluna da instância anterior. A original vê a validação e pula o juízo. A mutante vê `None` e consulta o implausível. A ocorrência efetiva em produção é [nao verificado]. `app/chamados/services/materializacao.py:139-150,195-208,925-928`, `app/chamados/templatetags/chamados_tags.py:131-136`, `app/templates/chamados/partials/_pergunta_card.html:42-43,75-81`, `app/chamados/services/validacao.py:161-175,720-723,746-760`, `app/chamados/models.py:4361-4379,4382-4390,2659`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:23-24`.

A fixture mapeada alterna validação pareada e resposta humana que limpa ambas as faces. Para fazê-la morder, conservar o mock de implausibilidade, começar com a irmã respondida e não validada, interpor `_carimbar_validacao` em outra instância depois que a busca real retorna a irmã e antes da consulta à sequência, e esperar que o par seja aceito e que o juiz de implausibilidade da irmã não seja chamado. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:35-67,213-226,238-246`, `app/chamados/services/validacao.py:161-175`.

**`materializar`: não pelos escritores atuais censados, partindo de par coerente.** O queryset inicial filtra a coluna não nula. O sítio alterado vem depois de adquirir a trava e reler a pergunta por PK. A versão original lê a validação vigente composta da instância fresca, a mutante lê a coluna da mesma instância. A fixture intercala resposta de outra instância e opcional nova validação, mas o `GET` posterior faz ambas lerem `None` ou `AGORA`. Uma linha de passivo divergente real que atravesse o filtro é [nao verificado]. `app/chamados/services/materializacao.py:2085-2103,2142-2164`, `app/chamados/models.py:2659,2875-2878,2901-2903,4361-4390`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:25-26`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:56-58,248-270`.

**`validar_par`: não pelos escritores atuais censados, partindo de par coerente.** A original lê a validação composta da irmã selecionada por uma consulta fresca e a mutante lê a coluna dessa irmã. Ambas controlam a decisão de chamar `_carimbar_validacao` dentro do savepoint que planta o par. A entrada real de `_validar_o_par` já tem a trava do colaborador. A fixture cria duas respostas, carimba ambas e só no subcaso negativo responde de novo à irmã, apagando a validação nos dois lados. A própria fixture trava antes de chamar. Passivo divergente real naquela irmã é [nao verificado]. `app/chamados/services/validacao.py:648-660,746-760,786-788`, `app/chamados/services/materializacao.py:139-150,195-200`, `app/chamados/models.py:2659,2875-2878,4361-4390`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:27-28`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:272-298`.

**Escritores comuns.** `_carimbar_validacao` grava a coluna e `VALIDACAO` da mesma pergunta, com a mesma data e no mesmo `atomic`. `PerguntaDisputa.carimbar` pode preencher a coluna para validação virtual e recusa se a composta ficar sem validação. A resposta humana baixa a coluna e grava `RESPOSTA` com `resposta_mantem_validacao=False`. Devolução ordinária, BUG-PAR, reabertura/recusa e ressuscitação baixam a coluna e emitem o respectivo compensador. Respostas automáticas e promovidas da trilha declaram preservação. O selo AST verifica o pareamento e a validação sintética. Seu escopo exclui migrações e testes. Um teste vizinho simula passivo B registrando `DEVOLUCAO` sozinho. `app/chamados/services/validacao.py:161-175,284-296,347-374`, `app/chamados/models.py:1701-1722,3367-3374,3384-3400,4313-4322,4330-4345`, `app/chamados/services/disputa_emissao.py:1499-1537,1630-1648,1664-1683,1714-1715,1735-1737`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:428-491,524-556,715-732`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:56-61`.

A ativação do mutante pelo mecanismo local exige entrar em `mutar_sitio`. O módulo dos testes mapeados chama esse contexto explicitamente só para desempenho. Uma ativação externa para os oito sítios é [nao verificado]. `app/chamados/tests/e4c_f5_voltaD_mutantes.py:49-66`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:163,199,344`.

## 21/11 05:55 · sessao 40ff2e051b8f · - · - · origem=Ronald (tmux)

`lote_entrada` e `lote_releitura` podem observar divergência durante uma escrita concorrente, inclusive quando o escritor mantém a paridade persistida. Os testes mapeados não montam essa leitura intercalada. A configuração usa PostgreSQL, os leitores não abrangem as duas consultas com uma trava e o código descreve `READ COMMITTED`. Ocorrência no ambiente real [nao verificado]. (app/config/settings/saas.py:83–92, app/chamados/models.py:904–912, app/chamados/services/validacao.py:1074–1084, 1094–1104)

Nos dois sítios, a original usa o instante da validação vigente no histórico `DecisaoPergunta`, ordenado por instante e PK, com validação sintética da projeção para o passivo sem linha e com os atos que desfazem a validação. A mutante lê a coluna `DateTimeField` anulável da instância. Ambas decidem por presença do instante. (app/chamados/models.py:2659, 4313–4322, 4330–4379, 4382–4390, app/chamados/tests/e4c_f5_voltaD_mutantes.py:32–37)

- **`lote_entrada`** lê primeiro as perguntas com as colunas, depois as decisões pelo prefetch. A validação real pode gravar a coluna e o ato com o mesmo instante e fazer commit entre as consultas. A instância do lote conserva a coluna nula, mas a composta lê o ato novo. A fixture mapeada parte de validação pareada ou de nova resposta que limpa as duas leituras. Fixture mínima com uma pergunta respondida e ainda sem validação, barreira após o SELECT principal, validação real em outra conexão antes do prefetch. Com a classe A forçada como no teste mapeado, o original recusa com `Ja validada.` e conta zero. O mutante chama a ação que relê `ja_validada` e depois conta uma validação em `nao_gravados`. (app/chamados/services/validacao.py:161–175, 693–704, 746–760, 1074–1109, app/chamados/services/acoes_disputa.py:18–29, app/chamados/tests/test_e4c_f5_voltaD_portas.py:35–67, 324–336)
- **`lote_releitura`** executa a ação em outra instância e recarrega integralmente a pergunta, descartando o cache das decisões. A coluna e as novas decisões são consultadas em etapas distintas. Uma resposta cujo pedido passou pela guarda pública enquanto a pergunta estava viva pode terminar depois que o lote vê a coluna preenchida. O core relê sob trava, limpa a coluna e grava a resposta que desfaz a validação. A composta consulta a nova sequência e devolve nulo, e a mutante fica com o instante carregado no refresh. A fixture existente usa um texto que conserva a validação e uma hora devolvida pela porta, com par concordante em ambos. Fixture mínima com a mesma pergunta respondida do ramo positivo, resposta humana iniciada antes da validação e liberada com instante posterior depois do SELECT do refresh e antes do SELECT de decisões. Original conta zero e põe em `recusados`. Mutante conta uma e, com o ramo de texto do teste, põe em `nao_gravados`. (app/chamados/views.py:1239–1246, 1274–1278, app/chamados/services/disputa_emissao.py:1478–1517, 1532–1537, app/chamados/services/validacao.py:1094–1114, app/chamados/tests/test_e4c_f5_voltaD_portas.py:350–386)

A coluna ganha preenchimento físico em `_carimbar_validacao` ou sintético em `carimbar`, com guarda para `(False, True)`. A sequência ganha a validação em `_carimbar_validacao` ou preserva o passivo ao materializar a linha faltante. A nova resposta humana, as duas devoluções, a reabertura/recusa e a ressuscitação limpam a coluna e registram o compensador. A paridade de presença consta em `par_ato_coluna` e é guardada pelos selos de escritores, de atos compensadores, de validação no sentido ato → coluna e de classificação da resposta. Esses selos verificam escritas, enquanto os dois sítios do lote consultam snapshots que podem ser distintos. (app/chamados/models.py:1646, 1702–1722, 3367–3400, 4313–4322, 4392–4399, 4428–4448, app/chamados/services/validacao.py:161–175, 284–296, 347–374, 938–944, 1074–1084, 1094–1104, app/chamados/services/disputa_emissao.py:1512–1514, 1532–1537, 1630–1683, app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:428–491, 524–556, app/chamados/tests/test_e4c_f5_volta3_selos.py:12–29, 41–59, app/chamados/tests/test_f2_resposta_mantem_validacao.py:18–25, 74–79)

## 21/11 05:40 · sessao 23ac1ea19728 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

A sonda abaixo seleciona os 19 casos a partir do JSON caso a caso da volta 3. O critério é `CONCLUSIVO`, `mudou_txt=True` e transição `entra ↔ fora/furo_espelho`. A cópia confirma 17 saídas em 16 PKs distintos e duas entradas, nos PKs 161 e 504. As 19 chaves completas estão [nao verificado]. Com o anexo resumido, a seleção falha antes de abrir a transação. `app/docs/RELATO.md:7`, `app/folha/export.py:837–842,968–981`.

A captura observa a própria execução de `classificar_export`. Guarda em memória `motivos_espelho`, os dias devolvidos pelo retentor, o PK e o veredito da célula, seus protestos e a ata quando o leitor da pausa a consulta. `sem_celula` pode sair com PK nulo ou com uma célula sem veredito. Os protestos participam das exceções do crivo como conjunto. A assinatura ainda recebe `inconsistencias`, mas o corpo atual ignora esse argumento. `app/folha/export.py:968–973,1148–1155,1173,1212–1213,1247–1298,1301–1307`, `app/ponto/juiz_batida.py:149–161`.

| Insumo do juiz | Caminho reutilizado |
|---|---|
| Colaborador, janela, `hoje` e `agora` | O registro do colaborador no fechamento, `janelas.janela_fechamento` e a data local do instante do ato. `app/ponto/management/commands/e6_oraculo.py:285–316`, `app/ponto/janelas.py:54–57`. |
| `horas_do_dia` | `InsumosDoDono` lê `batidas_apuraveis`, monta `minutos_do_oraculo` e passa `horas` do dia. `app/ponto/management/commands/e6_oraculo.py:128–154,182–195`, `app/ponto/turnos.py:2891–2899`. |
| `em_cadastro_x_realidade` | `espelho_do_colab` fornece `resultado`. `InsumosDoDono.cxr()` lê `resultado.dias_cadastro_x_realidade`. `app/ponto/management/commands/e6_oraculo.py:162–172,195,316–328`, `app/ponto/services/espelho.py:1241–1245,1310–1312`, `app/ponto/motor_calculo_v2.py:2719`. |
| `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho` | Os três vêm de `cadastro_do_dia_carregado`. `app/ponto/management/commands/e6_oraculo.py:182–188`, `app/ponto/supra_juiz.py:179–214`. |
| `batida_fora_de_turno` | `InsumosDoDono.turnos()` carrega a janela com bordas e `batida_fora_de_turno()` consulta o realizado e a batida solta apurável. `app/ponto/management/commands/e6_oraculo.py:156–160,185,198–212`. |
| `fase_deslocada` | `InsumosDoDono.fase()` lê `dias_de_fase_as_of` até `hoje`. `app/ponto/management/commands/e6_oraculo.py:174–187`, `app/escala/services/fase_12x36.py:117–148`. |

Todos os argumentos do juiz estão disponíveis pelo carregador compartilhado. O índice diário `dias_esp` e a sequência de cortes que decide **se o E6 registra uma divergência** são locais de `Command.handle`, pois pertencem ao laço de comparação do comando. A sonda reproduz esses cortes para o placar e chama `InsumosDoDono.dono()` em todos os dias da união A/B, nas duas fotos. `retornos_do_juiz` conta todas as chamadas. `divergencias_e6_por_dono` conta as que passam pelos cortes do comando. `app/ponto/management/commands/e6_oraculo.py:182–195,215–269,320–320,328–381`.

### `sonda_ab4.py`

```python
import io
import json
import logging
import socket
import sys
from collections import Counter
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from copy import deepcopy
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from chamados.services.disputa_emissao import push_disputa_silenciado [nao verificado]
from chamados.services.materializacao import ESCAPES_QUE_VIRAM_AUSENCIA
from colaboradores.mixins import tem_acao
from folha import export as E
from ponto import janelas
from ponto.juiz_batida import periodos_do_dia
from ponto.management.commands import e6_oraculo as E6
from ponto.models import Ausencia, FechamentoMensal
from ponto.services import dia_pago
from ponto.services.ausencia import decidir
from ponto.services.espelho import espelho_do_colab


ENTRA, FURO = ("entra", None), ("fora", "furo_espelho")
CAMINHOS = {(ENTRA, FURO): "sai", (FURO, ENTRA): "entra"}
TIPO, ESTADO = ESCAPES_QUE_VIRAM_AUSENCIA["FALTA_DIA"]
MEDICOES = {}


class SondaFalhou(RuntimeError):
    pass


def exigir(condicao, erro):
    if not condicao:
        raise SondaFalhou(erro)


def caminho(a, b):
    return CAMINHOS.get(((a["status"], a["motivo"]), (b["status"], b["motivo"])))


def selecionar(origem):
    exigir(
        origem["universo"]["percorrido_igual_ao_listado"] is True
        and origem["placar"]["status"] == "CONCLUSIVO",
        "selecao_inconclusiva",
    )
    casos = {}
    por_caminho = {"sai": [], "entra": []}
    for c in origem["casos"]:
        if c["status"] != "CONCLUSIVO" or c["mudou_txt"] is not True:
            continue
        grupo = caminho(c["A"]["classificador"], c["B"]["classificador"])
        if grupo is None:
            continue
        chave = tuple(c[k] for k in ("empresa_pk", "mes", "ano", "colaborador_pk"))
        exigir(all(type(n) is int and n > 0 for n in chave), "chave_invalida")
        exigir(chave not in casos, "caso_repetido")
        faltas = tuple(sorted(c["ausencia_pks"]))
        exigir(
            faltas and len(set(faltas)) == len(faltas)
            and all(type(p) is int and p > 0 for p in faltas),
            "faltas_invalidas",
        )
        casos[chave] = grupo, faltas
        por_caminho[grupo].append(chave[-1])
    exigir(
        len(por_caminho["sai"]) == 17
        and len(set(por_caminho["sai"])) == 16
        and sorted(por_caminho["entra"]) == [161, 504],
        "lote_diferente_de_19",
    )
    return casos


def classificar_com_prova(empresa, mes, ano, fech):
    # app/folha/export.py:1212-1213,1247,1259-1262,1298
    laco = E._dias_sem_decisao.__code__
    pausa = E._furo_so_da_pausa.__code__
    periodos = periodos_do_dia.__code__
    por_motivo, atas_lidas = {}, {}

    def observar(frame, evento, retorno):
        if evento != "return" or retorno is None:
            return
        codigo = frame.f_code
        if codigo is periodos and frame.f_back.f_code is pausa:
            cel = frame.f_locals["celula"]
            atas_lidas[cel.data] = {
                "celula_pk": cel.pk,
                "ata": deepcopy(cel.ata),
                "periodos": deepcopy(retorno),
            }
        elif codigo is laco:
            _, dias = retorno
            celulas = frame.f_locals.get("cels", {})
            for dia in dias:
                data = dia["data"]
                cel = celulas.get(data)
                por_motivo.setdefault(dia["motivo"], []).append({
                    **dia,
                    "protestos": deepcopy(cel.protestos) if cel is not None else None,
                    "leitura_ata": atas_lidas.get(data),
                })

    exigir(sys.getprofile() is None, "perfilador_ja_ativo")
    sys.setprofile(observar)
    try:
        item, = E.classificar_export(empresa, mes, ano, fechamentos=[fech])
    finally:
        sys.setprofile(None)

    motivos = deepcopy(item.get("motivos_espelho"))
    exigir(
        item["colaborador_id"] == fech.colaborador_id
        and (motivos is not None) == (item["motivo"] == "furo_espelho")
        and (motivos or []) == sorted(por_motivo),
        "rastro_do_espelho_incompleto",
    )
    return item, motivos, por_motivo, atas_lidas


def fotografar(empresa, mes, ano, colab_pk):
    fech = FechamentoMensal.objects.select_related("colaborador__empresa").get(
        colaborador_id=colab_pk, colaborador__empresa=empresa, mes=mes, ano=ano,
    )
    item, motivos, dias, atas = classificar_com_prova(empresa, mes, ano, fech)
    linhas, _ = E.montar_linhas_export(
        empresa, mes, ano, itens=[item], fechamentos=[fech],
    )

    matricula = item["codigo_dominio"].zfill(10)
    pai = None
    tipos = {"10": Counter(), "11": Counter()}
    for linha in linhas:
        partes = E.partes_da_linha(linha)
        exigir(partes is not None, "layout_txt")
        reg = partes["reg"]
        if reg == "10":
            pai = partes["matricula"]
            exigir(pai == matricula, "matricula_txt")
        else:
            exigir(reg == "11" and pai is not None, "layout_txt")
        tipos[reg][linha] += 1

    return {
        "status": item["status"],
        "motivo": item["motivo"],
        "motivos_espelho": motivos,
        "dias_por_motivo": dias,
        "atas_lidas": atas,
        "fechamento_pk": fech.pk,
        "colaborador": fech.colaborador,
        "linhas": linhas,
        "tipos": tipos,
    }


def situacao_e6(ins, linha, chave):
    # app/ponto/management/commands/e6_oraculo.py:336-369
    if linha is None:
        return "sem_linha"
    if linha.get("isencao") or linha.get("ausencia") is not None:
        return "isento_ou_ausencia"
    m_esp, _ = E6.minutos_do_espelho(linha)
    if m_esp is None:
        return "sem_lavratura"
    m_e6 = ins.oraculo.minutos.get(chave, 0.0)
    if m_esp == 0 and m_e6 == 0:
        return "ambos_zero"
    diferenca = m_e6 - m_esp
    if E6.diferenca_e_a_janela_declarada(diferenca, linha):
        return "desconto_janela_declarada"
    return "ate_10_min" if abs(diferenca) <= 10 else "divergencia"


def julgar(foto, empresa, mes, ano, dias, agora):
    # app/ponto/management/commands/e6_oraculo.py:285,297,304,316,320,328,378
    hoje = timezone.localtime(agora).date()
    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
    c = foto["colaborador"]
    esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
    ins = E6.InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
    linhas = {str(l.get("data"))[:10]: l for l in (esp.get("dias") or [])}
    foto["e6"] = {}
    for data in sorted(dias):
        exigir(ini <= data <= fim, "dia_fora_da_janela_e6")
        dono = ins.dono(data)
        exigir(dono in ("CADASTRO", "BATIDA", "ESTRUTURA"), "juiz_sem_resposta")
        chave = str(data)
        foto["e6"][data] = (dono, situacao_e6(ins, linhas.get(chave), chave))
    exigir(timezone.localdate() == hoje, "mudou_dia_do_ato")


class Descartar(io.TextIOBase):
    def __init__(self, falhas=None):
        self.falhas = falhas

    def write(self, texto):
        erro = sys.exc_info()[1]
        if self.falhas is not None and erro is not None:
            self.falhas.append(erro)
        return len(texto)


def contar_erros(falhas):
    objetos = {id(e): e for e in falhas if isinstance(e, BaseException)}
    totais = Counter(
        str(e) if isinstance(e, SondaFalhou) else type(e).__name__
        for e in objetos.values()
    )
    totais.update(e for e in falhas if isinstance(e, str))
    return dict(totais)


def etapa(falhas, acao):
    inicio = len(falhas)
    valor = None
    try:
        with transaction.atomic():
            try:
                valor = acao()
                exigir(not transaction.get_rollback(), "transacao_marcada_para_rollback")
            finally:
                transaction.set_rollback(True)
    except Exception as erro:
        falhas.append(erro)
    erros = contar_erros(falhas[inicio:])
    del falhas[inicio:]
    return valor, erros


def preparar_b():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        dp = next((u for u in ativos if tem_acao(u, "editar_ausencias")), None)
    exigir(dp is not None, "sem_decisor_dp")

    with connection.cursor() as cursor:
        cursor.execute("""
            DO $sonda$
            DECLARE
                s record;
                qualificada text;
                ultimo bigint;
                usada boolean;
                proximo numeric;
            BEGIN
                FOR s IN
                    SELECT n.nspname, c.relname, p.seqcache, p.seqincrement,
                           p.seqmin, p.seqmax, p.seqcycle
                    FROM pg_catalog.pg_sequence p
                    JOIN pg_catalog.pg_class c ON c.oid = p.seqrelid
                    JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    WHERE n.nspname IN ('juliani', 'public')
                    ORDER BY n.nspname, c.relname
                LOOP
                    qualificada := format('%I.%I', s.nspname, s.relname);
                    EXECUTE format('ALTER SEQUENCE %s CACHE %s', qualificada, s.seqcache);
                    EXECUTE 'SELECT last_value, is_called FROM ' || qualificada
                        INTO ultimo, usada;
                    proximo := ultimo::numeric;
                    IF usada THEN
                        proximo := proximo + s.seqincrement;
                    END IF;
                    IF proximo NOT BETWEEN s.seqmin AND s.seqmax THEN
                        IF NOT s.seqcycle THEN
                            CONTINUE;
                        END IF;
                        proximo := CASE WHEN s.seqincrement > 0
                                       THEN s.seqmin ELSE s.seqmax END;
                    END IF;
                    EXECUTE format('ALTER SEQUENCE %s RESTART WITH %s', qualificada, proximo);
                END LOOP;
            END
            $sonda$;
        """)
    return dp


def rejeitar(colab_pk, pks, dp):
    # app/ponto/services/ausencia.py:1057-1088,1090-1104
    ausencias = list(
        Ausencia.objects.filter(pk__in=pks, colaborador_id=colab_pk)
        .select_related("colaborador__empresa").order_by("pk")
    )
    exigir(
        tuple(a.pk for a in ausencias) == pks
        and all((a.tipo, a.status) == (TIPO, ESTADO) for a in ausencias),
        "faltas_mudaram",
    )
    agora = timezone.now()
    for aus in ausencias:
        decidir(
            aus, "rejeitada", por=dp, decisor=dp, aprovada_em=agora,
            motivo="sonda de falta aprovada",
        )
    gravadas = list(
        Ausencia.objects.filter(pk__in=pks).order_by("pk").values_list("pk", "status")
    )
    exigir(gravadas == [(pk, "rejeitada") for pk in pks], "rejeicao_incompleta")


def publicar_foto(foto):
    dias_retidos = [d for ds in foto["dias_por_motivo"].values() for d in ds]
    return {
        "fechamento_pk": foto["fechamento_pk"],
        "motivos_espelho": len(foto["motivos_espelho"] or []),
        "dias_retidos": len(dias_retidos),
        "celula_pks_retidas": sorted({
            d["celula_id"] for d in dias_retidos if d["celula_id"] is not None
        }),
        "dias_por_situacao_e6": dict(Counter(s for _, s in foto["e6"].values())),
        "divergencias_e6_por_dono": dict(Counter(
            d for d, s in foto["e6"].values() if s == "divergencia"
        )),
        "retornos_do_juiz": dict(Counter(d for d, _ in foto["e6"].values())),
    }


def medir(falhas, chave, esperado, dp):
    emp_pk, mes, ano, colab_pk = chave
    grupo, faltas = esperado
    fotos = MEDICOES[chave] = {}
    erros = {}
    pub = {
        "empresa_pk": emp_pk, "colaborador_pk": colab_pk,
        "ausencia_pks": faltas, "erros": erros,
    }
    hoje_ato = timezone.localdate()

    def foto_a():
        vig = E.competencia_entregue(emp_pk, mes, ano, registro=True)
        exigir(vig is not None, "competencia_sem_entrega")
        empresa = vig.empresa
        pub["exportacao_pk"] = vig.pk
        ini, fim = E.janela_competencia(empresa, mes, ano)
        atuais = tuple(
            E.ausencias_na_janela(colab_pk, TIPO, ini, fim).filter(status=ESTADO)
            .order_by("pk").values_list("pk", flat=True)
        )
        exigir(atuais == faltas, "faltas_diferentes_da_sonda3")
        fotos["A"] = fotografar(empresa, mes, ano, colab_pk)
        return empresa

    def foto_b():
        rejeitar(colab_pk, faltas, dp)
        fotos["B"] = fotografar(empresa, mes, ano, colab_pk)
        a, b = fotos["A"], fotos["B"]
        exigir(
            caminho(a, b) == grupo and a["linhas"] != b["linhas"],
            "delta_diferente_da_sonda3",
        )
        dias = {
            d["data"] for foto in (a, b)
            for registros in foto["dias_por_motivo"].values() for d in registros
        }
        exigir(dias, "nenhum_dia_retido")
        agora = timezone.now()
        exigir(timezone.localtime(agora).date() == hoje_ato, "mudou_dia_do_ato")
        _, falhas_e6 = etapa(falhas, lambda: julgar(b, empresa, mes, ano, dias, agora))
        if falhas_e6:
            erros["E6_B"] = falhas_e6
        return dias, agora

    empresa, falhas_a = etapa(falhas, foto_a)
    if falhas_a:
        erros["A"] = falhas_a
        return pub
    contexto, falhas_b = etapa(falhas, foto_b)
    if falhas_b:
        erros["B"] = falhas_b
        return pub

    dias, agora = contexto
    _, falhas_e6 = etapa(
        falhas, lambda: julgar(fotos["A"], empresa, mes, ano, dias, agora),
    )
    if falhas_e6:
        erros["E6_A"] = falhas_e6

    if not erros:
        pub.update({estado: publicar_foto(foto) for estado, foto in fotos.items()})
        a, b = (fotos[estado]["tipos"] for estado in ("A", "B"))
        pub["delta_txt"] = {
            reg: {
                "A": sum(a[reg].values()),
                "B": sum(b[reg].values()),
                "adicionadas": sum((b[reg] - a[reg]).values()),
                "removidas": sum((a[reg] - b[reg]).values()),
            }
            for reg in a
        }
    return pub


def principal(origem=None):
    falhas = []
    saida = {"esperados": 19, "selecionados": 0, "casos": []}

    def observar_log(_logger, registro):
        erro = registro.exc_info[1] if registro.exc_info else None
        args = registro.args if isinstance(registro.args, tuple) else ()
        excecoes = [a for a in args if isinstance(a, BaseException)]
        if erro is not None:
            falhas.append(erro)
        elif excecoes:
            falhas.extend(excecoes)
        elif registro.levelno >= logging.ERROR:
            falhas.append("erro_registrado_sem_tipo")

    def bloquear(*args, **kwargs):
        erro = SondaFalhou("efeito_externo_bloqueado")
        falhas.append(erro)
        raise erro

    def push_suprimido(*args, **kwargs): [nao verificado]
        return {"ok": False, "motivo": "sonda", "token_invalido": False}

    ler_dia_pago = dia_pago.linhas_do_periodo

    def observar_dia_pago(*args, **kwargs):
        # app/ponto/services/espelho.py:420-426
        try:
            return ler_dia_pago(*args, **kwargs)
        except Exception as erro:
            falhas.append(erro)
            raise

    try:
        with ExitStack() as pilha:
            pilha.enter_context(redirect_stdout(Descartar()))
            pilha.enter_context(redirect_stderr(Descartar(falhas)))
            pilha.enter_context(patch.object(logging.Logger, "handle", observar_log))
            pilha.enter_context(timezone.override(settings.TIME_ZONE))

            casos = selecionar(json.load(sys.stdin) if origem is None else origem)
            saida["selecionados"] = len(casos)
            exigir(
                settings.SETTINGS_MODULE == "config.settings.sombra"
                and getattr(settings, "HASNER_SOMBRA", False) is True
                and set(settings.DATABASES) == {"default"}
                and connection.vendor == "postgresql"
                and connection.settings_dict["NAME"] == "sombra"
                and connection.settings_dict["HOST"] == "sombra_db"
                and getattr(connection, "schema_name", None) == "juliani"
                and not connection.in_atomic_block
                and connection.get_autocommit(),
                "guarda_sombra",
            )
            pilha.enter_context(push_disputa_silenciado()) [nao verificado]
            pilha.enter_context(patch.object(push_utils, "_enviar_push_usuario", push_suprimido)) [nao verificado]
            pilha.enter_context(patch.object(fcm_utils, "push_fcm", push_suprimido)) [nao verificado]
            pilha.enter_context(patch.object(dia_pago, "linhas_do_periodo", observar_dia_pago))
            pilha.enter_context(patch.object(socket, "getaddrin

[truncado em 20000 de 23631 caracteres]

## 21/11 05:36 · sessao 305a78b7faaf · E4C-F5-CONSTROI · leitura · origem=Code

**PUBLICÁVEIS: 35, 36 e 55.** O pré-desenho identifica os 69 K e os cinco R originais. O aval 1830 identifica o sexto R. A matriz identifica os 96 chamadores, e a cópia permite ligá-los às voltas. `predesenho.md:28–109`, `matriz.md:18–118`, `app/docs/PROMPTS.md:2659`, `app/docs/RELATO.md:37,1306,1314,1320`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:251–266`

As linhas de `matriz.md` e `predesenho.md` são as dos anexos colados, contando o título como linha 1 e incluindo as linhas em branco. Nas tabelas, os caminhos de código partem de `app/`, salvo `bin/`. A comparação é por sítio e comportamento observável. A identidade Git dos dois snapshots e o diff integral estão [nao verificado]. As contagens atuais de chamadas e de asserts foram feitas por leitura estática. A execução dos testes nesta auditoria está [nao verificado]. [nao verificado]

## 01 · Matriz de 15 sítios

**NAO FECHA o vínculo com a numeração original do brief.** A tabela nominal dos sítios fecha em **15 de 15**, com os três campos adicionais do histórico. A tabela nova põe a devolução na posição 15 do cruzamento. O RELATO cita `matriz_c3.txt:13` para a mesma devolução. Essa citação é um endereço de arquivo. Sua equivalência ao ordinal do `brief_c3` está [nao verificado]. `matriz.md:151–170`, `app/docs/RELATO.md:94,752,768,1503`

Na coluna de prova, `test_e4c_f5_*` refere-se a `chamados/tests/`.

| Sítio e fonte | Sítio atual e delta | Selo ou RED atual |
|---|---|---|
| Projeção temporal, `matriz.md:151` | `chamados/juizes.py:1383–1403`. Lê os campos recebidos e aplica o horizonte. A leitura ORM foi para o chamador em `:1408–1409`. | `test_e4c_f5_selo_chamada.py:126–149,271–275`, `test_e4c_f5_volta2_juizes.py:77–92` |
| Respondida, `matriz.md:152` | `chamados/juizes.py:1551–1560`. Julga o instante e a presença de texto do objeto recebido. O ramo de leitura ORM saiu do juiz. | `test_e4c_f5_selo_chamada.py:126–149,271–275`, `test_e4c_f5_volta2_juizes.py:94–105` |
| Corrigir resposta, `matriz.md:153` | `chamados/services/acoes_disputa.py:349–350`. Guarda de validação pela composta. | `test_e4c_f5_selo_catraca.py:18`, `test_e4c_f5_voltaD_portas.py:84–95` |
| Guarda de declaração, `matriz.md:154` | `chamados/services/declaracao_texto.py:217–229`. A guarda comum lê a validação pela composta. | `test_e4c_f5_selo_catraca.py:19`, `test_e4c_f5_voltaD_portas.py:103–134` |
| Resolver sem credor, `matriz.md:155` | `chamados/services/disputa_emissao.py:2107–2112`. Prefetch e guarda de validação pela composta. | `test_e4c_f5_selo_catraca.py:20`, `test_e4c_f5_voltaD_portas.py:142–170` |
| Esperar o texto, `matriz.md:156` | `chamados/services/fio.py:178–190`. A composta substitui a montagem local. O instante vem do ato e o texto da coluna. | `test_e4c_f5_selo_catraca.py:21`, `test_e4c_f5_voltaA_sitios.py:55–71` |
| Irmã exigida validada, `matriz.md:157` | `chamados/services/materializacao.py:200–202`. Primeira guarda sobre a composta da irmã. | `test_e4c_f5_selo_catraca.py:16,22`, `test_e4c_f5_voltaD_portas.py:228–236` |
| Irmã implausível, `matriz.md:158` | `chamados/services/materializacao.py:203–208`. Segunda guarda sobre a mesma leitura. | `test_e4c_f5_selo_catraca.py:16,22`, `test_e4c_f5_voltaD_portas.py:238–246` |
| Materializar validadas, `matriz.md:159` | `chamados/services/materializacao.py:2142–2163`. Seleção inicial SQL e guarda pela composta após releitura sob trava. | `test_e4c_f5_selo_catraca.py:22`, `test_e4c_f5_voltaD_portas.py:248–270` |
| Validar o par, `matriz.md:160` | `chamados/services/validacao.py:655–659`. Guarda pela composta da outra metade. | `test_e4c_f5_selo_catraca.py:23–24`, `test_e4c_f5_voltaD_portas.py:272–298` |
| Validar pergunta, `matriz.md:161` | `chamados/services/validacao.py:697–704`. Uma composta fornece o par julgado e a validação vigente. | `test_e4c_f5_selo_catraca.py:24`, `test_e4c_f5_voltaD_portas.py:300–310` |
| Recusar pergunta, `matriz.md:162` | `chamados/services/validacao.py:915–923,930–932`. A guarda lê a composta e a foto anterior lê a coluna. | `test_e4c_f5_selo_catraca.py:24`, `test_e4c_f5_voltaD_portas.py:312–322` |
| Validar classe A em lote, `matriz.md:163` | `chamados/services/validacao.py:1074–1084,1102–1105`. Guarda pela composta na entrada e após releitura integral. | `test_e4c_f5_selo_catraca.py:25`, `test_e4c_f5_voltaD_portas.py:324–374` |
| Resposta na tela, `matriz.md:164` | `chamados/templatetags/chamados_tags.py:166–171`. Permanece o atalho por texto corrente seguido do classificador. | `test_e4c_f5_selo_catraca.py:27`, `app/docs/RELATO.md:1503` |
| Devolver validação, `matriz.md:165` | `chamados/services/validacao.py:267–296`. Descarta o cache e entra pela validação do ato ou da coluna, com escrita pareada. | `test_e4c_f5_selo_catraca.py:25`, `test_e4c_f5_item1_devolucao_ato_ou_coluna.py:69–128` |
| Histórico R, `matriz.md:166` | `chamados/services/historico.py:98–107`. Lê e filtra o instante da resposta. | `test_e4c_f5_selo_catraca.py:26`, `test_e4c_f5_volta1_leem_o_ato.py:75–87` |
| Histórico V, `matriz.md:167` | `chamados/services/historico.py:98–107`. A mesma leitura fornece o instante da validação. | `test_e4c_f5_selo_catraca.py:26`, `test_e4c_f5_volta1_leem_o_ato.py:75–87` |
| Histórico T, `matriz.md:168` | `chamados/services/historico.py:110–116`. O rótulo usa o texto da coluna. | `test_e4c_f5_selo_catraca.py:26`, `test_e4c_f5_volta1_leem_o_ato.py:75–87` |

A recontagem é `13 funções da catraca − 1 histórico + 1 segunda guarda da irmã + 2 juízes = 15 sítios`. Os três campos adicionais cabem na mesma função do histórico. A amostra **9/9** do brief e a amostra **5/5** da MATRIZ-F5-A têm referências distintas. A identidade das nove linhas da primeira amostra está [nao verificado]. `chamados/tests/test_e4c_f5_selo_catraca.py:14–28`, `matriz.md:3,166–170`, `app/docs/RELATO.md:1503`

## 32 · Triagem de `casefold`

**NAO FECHA.** O registro dá o encaminhamento **como anexo ao Opus, sem decisão da sessão**. A decisão motivada, o patch exato do sobrevivente e a execução nominal no integrado estão [nao verificado]. `app/docs/RELATO.md:94,1374,1377,1439`

| Registro | Fato e sítio |
|---|---|
| Opus anterior da volta 1 | `casefold` ficou entre as pendências que aquele laudo tratou como não bloqueantes. `app/docs/RELATO.md:1390`, `app/docs/BACKLOG.md:2064` |
| Verificador 4 | Zero bloqueios no delta. O sobrevivente `casefold` foi registrado como plausível, sem execução, para o anexo ao Opus. `app/docs/RELATO.md:1439` |
| Replays | Terra **NAO BLOQUEIA**. Sol 6.1 e Sol 6.2 **BLOQUEIAM** para `casefold`. Sol 6.2 registra separadamente `strip()` só no texto do ato. Conta de pareceres sobre caixa **1 + 2 = 3**. `app/docs/RELATO.md:1374,1377` |
| Teste de caixa e sobrevivente | `test_diferenca_so_de_caixa_conta` usa `Ato` no ato e `ato` na coluna, esperando `(1, 1)`. Pela leitura do teste, `casefold` nos dois lados ou só no ato derruba esse asserto. Só na coluna já minúscula preserva o resultado. O lado alterado pelo patch histórico está [nao verificado]. `chamados/tests/test_e4c_f5_contador_texto.py:21–24,50–53`, `chamados/services/vigia_texto_resposta.py:19–25`, `app/docs/RELATO.md:1439` |
| Teste de espaço | As duas variações põem os espaços na coluna. O ato já vem sem espaços. Isso explica por leitura estática a sobrevivência de `strip()` só no ato nesses exemplos. `chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–48`, `app/docs/RELATO.md:1377,1440` |

| Leitor | Tratamento na cópia |
|---|---|
| Compositor e origem do texto | A sequência conserva o texto vigente do ato e incorpora o passivo. A composta recebe o texto da coluna. É a diferença de fontes descrita em `matriz.md:12`, `predesenho.md:115–117`. `chamados/models.py:4302–4323,4348–4390` |
| `vigia_texto_resposta.contar` | Comparação literal `!=`, com `or ''` na coluna. A diferença só de caixa conta. A carga usa prefetch por bloco. `predesenho.md:127`, `chamados/services/vigia_texto_resposta.py:11–25` |
| `validar_classe_a` | Segundo comparador literal. Separa o divergente antes do classificador e imprime um prefixo do texto do ato nos casos listados. `chamados/management/commands/validar_classe_a.py:60–70,80–84,88–103` |
| `vigiar_fio_morto` e ESTADO | O comando lê divergentes e universo, imprime os totais e carimba a divergência com esperado zero. O ESTADO compara o carimbo com o esperado. `chamados/management/commands/vigiar_fio_morto.py:104–110,194–197,230–232,274–282`, `bin/fio_estado.py:121–126,191–194`, `bin/gerar_estado.py:260–261` |
| Fotos de reabertura, silêncio e retratação | Copiam o texto do ato. `chamados/models.py:1707–1719,2104–2112,2236–2246` |
| `resposta_display` | Lê o texto do ato, resolve rótulos de IDs e compara opções literalmente. API e cartões usam a propriedade. `chamados/models.py:3865–3891`, `api/views.py:1163–1164,2579–2580`, `templates/chamados/partials/_pergunta_card.html:31` |
| Tarefa e juízes | A tarefa testa a presença de `ato_texto`. Os predicados de presença usam `strip()`. O temporal repassa o texto recebido ou o esvazia pelo horizonte. `chamados/services/tarefa_do_dia.py:159–168,427–429`, `chamados/juizes.py:1391–1403,1533–1560` |
| Histórico, API e mensageria | O texto da coluna é usado no rótulo ou na serialização e conserva a caixa. `matriz.md:168`, `chamados/services/historico.py:98–113`, `api/views.py:1163,2579`, `api/views_mensageria.py:555–569` |
| Classificadores | `classificar_resposta_validacao` usa `lower()` no ramo de escolha. `classe_do_lote` usa `lower()` e remoção de acentos sobre a coluna. `chamados/juizes.py:2043–2056`, `chamados/services/validacao.py:1181–1183,1196–1214` |

## 35 · Mapa nominal dos carregadores

**PUBLICÁVEL.** Os **69 de 69 K** são identificáveis. A tabela histórica tem **10 L + 29 Q + 5 R + 25 M**. K40 migrou de M para R, dando **10 L + 29 Q + 6 R + 24 M = 69**. A sobra é um sítio adicional. `matriz.md:174,178–190`, `predesenho.md:28–107`, `app/docs/PROMPTS.md:2659`, `app/docs/RELATO.md:788,1286`

A coluna atual dá o ponto de carga ou de releitura. **Lote** indica prefetch de `decisoes` no percurso mostrado, com as exceções de localização explicitadas na própria linha.

| K e fonte | Classe | Sítio atual e delta |
|---|---|---|
| 1 · `predesenho.md:28` | L | Tarefa, lote em `chamados/services/tarefa_do_dia.py:533–545`. |
| 2 · `predesenho.md:29` | L | `alimentar_portas`, lote em `chamados/juizes.py:1664–1669`. |
| 3 · `predesenho.md:30` | R | O261 mantém releitura em `chamados/services/relavra_via.py:229–242`. |
| 4 · `predesenho.md:31` | L | `perguntas_vivas_de`, lote em `chamados/juizes.py:1512–1513`. |
| 5 · `predesenho.md:32` | M | I21a, lote em `chamados/management/commands/auditar_invariantes_chamados.py:1587–1592`. |
| 6 · `predesenho.md:33` | M | I2, lote em `chamados/management/commands/auditar_invariantes_chamados.py:591–595`. |
| 7 · `predesenho.md:34` | M | F1, lote aninhado em `chamados/management/commands/auditar_invariantes_chamados.py:1643–1646`. |
| 8 · `predesenho.md:35` | M | Universo do grupo, lote em `chamados/services/fio.py:2079–2081`. |
| 9 · `predesenho.md:36` | Q | Órfãs reatáveis. Seletor em `chamados/reconciliador.py:1554`, lote no consumidor em `:1575`. |
| 10 · `predesenho.md:37` | Q | Órfãs abertas, lote em `chamados/reconciliador.py:1100–1103`. |
| 11 · `predesenho.md:38` | Q | Beco pelo evento, lote em `chamados/services/becos.py:86–87`. |
| 12 · `predesenho.md:39` | M | Plano do backfill, lote em `chamados/management/commands/backfill_fio_morto_e4b.py:89–90`. |
| 13 · `predesenho.md:40` | M | Execução do backfill, lote e releitura após ato em `chamados/management/commands/backfill_fio_morto_e4b.py:133–144`. |
| 14 · `predesenho.md:41` | M | Exportadas, lote em `chamados/management/commands/backfill_fio_morto_e4b.py:175–178`. |
| 15 · `predesenho.md:42` | L | `_ancoradas`, lote em `chamados/models.py:349–366`. |
| 16 · `predesenho.md:43` | Q | Pendentes S128, lote em `chamados/services/disputa_emissao.py:1978–1982`. |
| 17 · `predesenho.md:44` | Q | Universo do fato, lote em `chamados/services/fio.py:1963–1964`. |
| 18 · `predesenho.md:45` | Q | Vizinhos, lote em `chamados/services/fio.py:1595–1597`. |
| 19 · `predesenho.md:46` | R | Materialização mantém releitura sob trava em `chamados/services/materializacao.py:2149–2163`. |
| 20 · `predesenho.md:47` | R | Validação mantém `.get()` da porta em `chamados/services/validacao.py:689–704`. |
| 21 · `predesenho.md:48` | M | Contador de paridade, lote em `chamados/services/materializacao.py:328–329`. |
| 22 · `predesenho.md:49` | R | Busca da irmã complementar permanece em `chamados/services/materializacao.py:284–294`. |
| 23 · `predesenho.md:50` | Q | Prévia, lote em `folha/previa.py:64–67`. |
| 24 · `predesenho.md:51` | Q | Cartório, lote aninhado em `ponto/services/cartorio.py:145–146`. |
| 25 · `predesenho.md:52` | Q | Declaração por marco, lote em `chamados/services/declaracao_texto.py:475–477`. |
| 26 · `predesenho.md:53` | Q | Declaração por lote de fatos, `chamados/services/declaracao_texto.py:589–590`. |
| 27 · `predesenho.md:54` | Q | Proposta por fato, lote em `chamados/services/declaracao_texto.py:634–636`. |
| 28 · `predesenho.md:55` | M | DRY de decisões, lote em `chamados/management/commands/backfill_decisoes_pergunta.py:531–534`. |
| 29 · `predesenho.md:56` | Q | Corrigíveis, lote em `chamados/juizes.py:1765`. |
| 30 · `predesenho.md:57` | Q | Fila a validar, lote em `chamados/juizes.py:1772–1775`. |
| 31 · `predesenho.md:58` | Q | Dias com vivas, lote em `chamados/juizes.py:2435–2438`. |
| 32 · `predesenho.md:59` | L | Disputa viva, lote no chamador em `chamados/juizes.py:2490–2491`. |
| 33 · `predesenho.md:60` | M | F1t, lote em `chamados/management/commands/auditar_invariantes_chamados.py:1724–1727`. |
| 34 · `predesenho.md:61` | M | I6, lote em `chamados/management/commands/auditar_invariantes_chamados.py:538–540`. |
| 35 · `predesenho.md:62` | M | I8, lote em `chamados/management/commands/auditar_invariantes_chamados.py:861–864`. |
| 36 · `predesenho.md:63` | M | I8b, lote em `chamados/management/commands/auditar_invariantes_chamados.py:897–900`. |
| 37 · `predesenho.md:64` | M | F2, lote em `chamados/management/commands/auditar_invariantes_chamados.py:1782–1786`. |
| 38 · `predesenho.md:65` | M | F3, lote em `chamados/management/commands/auditar_invariantes_chamados.py:1795–1799`. |
| 39 · `predesenho.md:66` | M | Fio mudo, lote aninhado em `chamados/management/commands/disparar_perguntas_competencia.py:107–111`. |
| 40 · `predesenho.md:67` | M → R | Migração mantém `.get(pk=pk)` em `chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`. |
| 41 · `predesenho.md:68` | Q | Ressuscitação, lote em `chamados/models.py:1549–1555`. A cópia para C6 recebe cache próprio recortado em `:1569,1585–1595`. |
| 42 · `predesenho.md:69` | Q | A segunda carga de `fechar` foi absorvida pela lista única, com lote e reuso pela guarda. `chamados/models.py:2068–2072,2081`, `app/docs/RELATO.md:1289` |
| 43 · `predesenho.md:70` | Q | Absolvíveis, lote em `chamados/reconciliador.py:702–703`. |
| 44 · `predesenho.md:71` | Q | Detalhe, lote em `chamados/services/acoes_chamado.py:351–352`. |
| 45 · `predesenho.md:72` | Q | Medição por competência, lote em `chamados/services/competencia_trancada.py:119–121`. |
| 46 · `predesenho.md:73` | Q | Medição por chamado, lote em `chamados/services/competencia_trancada.py:165–167`. |
| 47 · `predesenho.md:74` | Q | Sem credor, lote próprio e guarda composta em `chamados/services/disputa_emissao.py:2107–2112`. |
| 48 · `predesenho.md:75` | Q | Migração, destino com lote em `chamados/services/fio.py:2297–2298`. |
| 49 · `predesenho.md:76` | Q | Migração, origem com lote em `chamados/services/fio.py:2301–2303`. |
| 50 · `predesenho.md:77` | Q | Marco já perguntado, lote em `chamados/services/pergunta_no_app.py:186–187`. |
| 51 · `predesenho.md:78` | M | Vigia de disputa fechada, lote em `chamados/services/vigia_fio.py:713–714`. |
| 52 · `predesenho.md:79` | M | Vigia de carimbos, lote em `chamados/services/vigia_fio.py:772–774`. |
| 53 · `predesenho.md:80` | Q | Reabrir disputa, lote em `chamados/views.py:1412–1413`. |
| 54 · `predesenho.md:81` | Q | Calendário, lote em `colaboradores/services/calendario.py:504–505`. |
| 55 · `predesenho.md:82` | Q | Dia do colaborador, lote em `ponto/services/dia_do_colab.py:64–65`. |
| 56 · `predesenho.md:83` | M | Foto inicial L-130 com lote em `chamados/management/commands/estoque_l130_instante_sem_texto.py:37–38`. |
| 57 · `predesenho.md:84` | M | Releitura do conjunto L-130 com lote em `chamados/management/commands/estoque_l130_instante_sem_texto.py:97–98`. |
| 58 · `predesenho.md:85` | R | Porta L-130 mantém releitura por ID em `chamados/services/validacao.py:444–448`. |
| 59 · `predesenho.md:86` | M | Arquivamento, lote em `chamados/management/commands/arquivar_competencia_encerrada.py:158–159`. |
| 60 · `predesenho.md:87` | M | Todas as perguntas do vigia, lote em `chamados/services/vigia_fio.py:570–571`. |
| 61 · `predesenho.md:88` | Q | Explicador sem coleção recebida, lote em `chamados/services/explicador.py:160–161`. |
| 62 · `predesenho.md:89` | L | Foto, lote no chamador em `api/foto_recebida.py:80–81`. |
| 63 · `predesenho.md:90` | L | Encerrar por fato, lote no chamador em `chamados/management/commands/encerrar_perguntas_com_fato.py:82–85`. |
| 64 · `predesenho.md:91` | L | Limbo, lote no chamador em `chamados/services/limbo_folgas.py:203–205`. |
| 65 · `predesenho.md:92` | Q | Supra-juiz, lote externo em `ponto/management/commands/supra_juiz.py:190–192`. |
| 66 · `predesenho.md:93` | M | Emissão retroativa, lote externo em `ponto/management/commands/emitir_furo_retroativo.py:82–83`. |
| 67 · `predesenho.md:94` | L | Reconciliar grade, lote no chamador em `ponto/management/commands/reconciliar_grade.py:456–462`. |
| 68 · `predesenho.md:95` | L | Dedup da emissão, lote no chamador em `chamados/services/disputa_emissao.py:1429–1436`. |
| 69 · `predesenho.md:96` | M | Beco pelo comando, lote externo em `chamados/management/commands/fechar_becos_disputa.py:32–33`. |
| Sobra · `predesenho.md:97` | M fora dos 69 | Dedup por grupo ganha prefetch em `chamados/management/commands/dedup_perguntas_fato.py:74–77`. |

Os **59 pontos** originais batem na unidade do desenho, `5 L comuns + 29 Q + 25 M`. Incluindo a sobra, eram **60**. Mantida essa unidade após a reclassificação de K40, a conta é **58**, ou **59** com a sobra. Na implementação atual, `fio_vivo` itera a coleção recebida e os seis K desse grupo têm prefetches nos chamadores. Contando os sítios concretos da tabela, são **63 K com lote + 6 R**, ou **64 pontos de lote** com a sobra. K42 conta pela lista que hoje o abastece, cuja antiga segunda carga saiu. `predesenho.md:97,101–111`, `chamados/juizes.py:1516–1526`, `chamados/models.py:2068–2081`, `app/docs/RELATO.md:1286,1289,1329`

## 36 · Seis R

**PUBLICÁVEL.** São **cinco R do pré-desenho + K40 pelo 1830 = seis**. Cada percurso mantém a leitura por item na cópia. `predesenho.md:30,46–49,67,85,105`, `app/docs/PROMPTS.md:2659`, `app/docs/RELATO.md:788,1286`

| R | Fonte | Sítio atual e razão |
|---|---|---|
| K3 | `predesenho.md:30` | O261 relê a candidata por PK após travar o dono. `chamados/services/relavra_via.py:229–242` |
| K19 | `predesenho.md:46` | Materialização relê cada candidata após seleção e trava, antes da guarda de validação. `chamados/services/materializacao.py:2149–2163` |
| K20 | `predesenho.md:47` | A porta valida por ID, inclusive quando chamada pelo lote. `chamados/services/validacao.py:689–704,1095–1096` |
| K22 | `predesenho.md:49` | O contador busca a irmã complementar da pergunta corrente. `chamados/services/materializacao.py:284–294,341–346` |
| K40 | `predesenho.md:67`, `app/docs/PROMPTS.md:2659` | `_descarimbados` percorre PKs, relê e julga cada candidata no retrato da migração. `chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` |
| K58 | `predesenho.md:85` | A porta L-130 relê por ID sob trava e rechecagem do impedimento. `chamados/serv

[truncado em 20000 de 53012 caracteres]

## 21/11 05:30 · sessao 6049758e46f8 · - · - · origem=Ronald (tmux)

**01 — NAO FECHA no vínculo ao brief original.** `matriz.md:151–165` identifica 15 sítios, todos com correspondência na cópia. `matriz.md:166–168` identifica três campos do histórico na mesma função adicional. O cadastro atual tem 13 funções em oito arquivos e zero acusações na leitura AST. A conta fecha em `13 − 1 histórico + 1 segunda guarda de par + 2 juízes = 15`. A devolução aparece como #15 no cruzamento e em `matriz_c3.txt:13` na citação histórica. A relação entre a numeração do cruzamento e os ordinais do `brief_c3`, assim como a identidade das nove verificações da amostra original, permanece [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_catraca.py:14–28,332–349`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–149,271–275`, `app/docs/RELATO.md:94,752,768,1503`.

**55 — PUBLICAVEL.** As 96 expressões históricas têm correspondência na cópia. O recorte fecha em 41 diretas, 53 chamadas C6 e dois acessos externos a `.viva`, com oito avulsas e 88 em laço ou lista. A partição nominal das diretas é quatro usos Opus (#10–13), uma exceção SQL (#31), doze A1 (#3,4,5,14,15,20,21,22,23,24,30,41), catorze B (#1,2,16–19,32,33,35–40) e dez C (#6–9,25–29,34). As 55 indiretas passam pelo compositor introduzido no C6 na A1. `matriz.md:18–58,62–118`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:25–28,251–266`, `app/docs/RELATO.md:1306,1314,1316–1317,1320`, `app/chamados/juizes.py:893–899,1406–1428`.

O scanner atual conta 44 diretas em 22 arquivos, com 39 R compostas, quatro P compostas e uma R SQL permitida. As três adicionais estão em `cadastro_realidade.py:500` (R, B2) e `materializacao.py:1061,1065` (P/R, absorção A1(c)). Zero fora das permitidas na leitura AST. A série registrada fecha em `36 → 24 → 10 → 0`. A origem do 19 histórico e o diff Git integral seguem [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160`, `app/escala/services/cadastro_realidade.py:497–502`, `app/chamados/services/materializacao.py:1056–1067`, `app/docs/RELATO.md:37,276,1296,1317,1320`.

## 21/11 05:29 · sessao acc0c17b903c · - · - · origem=Ronald (tmux)

- **49 — NAO FECHA.** A matriz identifica 12 categorias de falha. A soma dá 112 ocorrências em 104 métodos. Retirada a fixture ilegal dos dois RED-8, sobram 11 categorias, 110 ocorrências e 102 métodos. A identidade dessas 11 com as protegidas pelo aval 1710 e o pareamento completo das exceções continuam [nao verificado]. A recontagem estática do selo reproduz os 9 módulos e os 486 asserts cadastrados. `matriz.md:194,198–209` · `predesenho.md:168–170` · `app/docs/RELATO.md:94,1314` · `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`
- **51 — NAO FECHA pelo ordinal histórico.** A fixture ilegal é a oitava linha da tabela de 12 famílias e é a única que o pré-desenho identifica como exigindo mudança no harness. Os dois RED-8 pertencem a `JuizesVolta2Test`. A fixture agora nasce sem texto e usa a porta de ato. O cálculo exige zero consultas e `LeituraComposta`. O fio exige até um SELECT de decisões, confere os PKs e o cache e percorre quatro perguntas mortas até a quinta viva. O ordinal histórico 12 permanece [nao verificado]. `matriz.md:205` · `predesenho.md:168–170` · `app/docs/RELATO.md:94,1308–1309,1338` · `app/chamados/tests/test_e4c_f5_volta2_juizes.py:26,119–186`
- **80 — NAO FECHA.** Os anexos ligam K47 e as guardas D aos sítios históricos. O K47 atual usa as portas de resposta e validação, confirma equivalência e espera zero resolvidas, uma disputa fechada e zero materializações. A D tem 18 métodos atuais, com 16 REDs e dois controles, além do K47 em outro arquivo. O registro de 18 esperados antigos no corpo de `604505878` não permite identificar os 18 casos atuais correspondentes [nao verificado]. O único esperado anterior literal recuperado é zero DEVOLUÇÃO em A, diante de uma DEVOLUÇÃO hoje. Falta também a execução nominal final dos mutantes. `matriz.md:153–165` · `predesenho.md:74` · `app/docs/RELATO.md:62,64,283,752,762,786,790` · `app/chamados/services/disputa_emissao.py:2107–2120` · `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222` · `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–396`

## 21/11 05:29 · sessao 06a1e74a8a57 · - · - · origem=Ronald (tmux)

**35 e 36 — PUBLICÁVEIS.** O mapa nominal cobre os 69 K e a sobra. Os 69 se repartem em 10 L, 29 Q, 6 R e 24 M depois da reclassificação de K40. A cópia tem prefetch nos 63 sítios K de lote e na sobra. Os seis R preservam releituras por item. O pré-desenho contava 59 pontos na unidade lógica, agrupando seis cargas em `fio_vivo`; a cópia põe o prefetch nos seis chamadores. `predesenho.md:28–111`, `app/docs/PROMPTS.md:2659`, `app/chamados/juizes.py:1516–1526`

**40 — NÃO FECHA.** B planejada tem 7 L + 22 Q. Os 14 seletores atuais cobrem 3 L + 11 Q; K41 e K42 acrescentam 2 Q no despacho. K41 é a ressuscitação com cópia e cache próprio. K42 é a lista agora única de `fechar`, partilhada com a guarda pela D. Os 13 IDs restantes do plano correspondem a 4 L + 9 Q e têm prefetch na cópia. A conciliação histórica 29 cobertos × 27 editados e o subconjunto 5 L + 3 Q seguem [nao verificado]. `predesenho.md:28–96,133,135`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/docs/RELATO.md:1296,1329`, `app/chamados/models.py:1549–1555,1591–1595,2068–2072`

**41 — NÃO FECHA pelo recorte das três M.** A manutenção fecha em 24 M + sobra depois da saída de K40. Nove M têm seletor C, K13 tem RED de releitura e as 14 restantes têm carga atual. Há um trio verificável de M de auditoria, K5/I21a, K6/I2 e K7/F1, com REDs próprios. Sua identidade como o trio histórico pedido segue [nao verificado]. `predesenho.md:28–97,137`, `app/docs/RELATO.md:94,1286,1289`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`, `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:65–85`

## 21/11 05:25 · sessao 6697e55f32e6 · E4C-F5-CONSTROI · leitura · origem=Code

# E4C-F5-CONSTROI · auditoria de publicação

Usei a cópia entregue como `b0935c1f106d232dc39b263676a588d66eddff54`. A cópia não contém metadados Git, portanto a identidade do HEAD e o diff integral contra `c8eb8eddf` ficam **[nao verificado]**. Conferi as âncoras atuais e marquei os deltas documentados. A igualdade dos `.py` de produção entre os dois hashes é premissa do dado fornecido. O RELATO também registra deltas intermediários só de teste. `app/docs/RELATO.md:9,14,17–18,39`

## 01 · Matriz e censo de entrada

O aval 0815, registrado no fechamento da Etapa A, dá a matriz de 15 sítios, amostra de 9/9 e zero perguntas de lei. O item exige publicar a identidade de cada sítio, seu número no `brief_c3`, o selo que o cobre e seu vínculo com o PRONTO. A ordem abaixo é a da reconstrução. A numeração original só apareceu, por citação no RELATO, para a devolução. `app/docs/RELATO.md:94,167,752,768,787,1503`

| Sítio reconstituído | Leitura na cópia | Selo ou prova do PRONTO |
|---|---|---|
| `espera_o_texto_do_colaborador` | Instante pela composta, texto corrente no predicado. `app/chamados/services/fio.py:178–190` | Catraca e RED do fio. `app/chamados/tests/test_e4c_f5_selo_catraca.py:21`, `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71` |
| `leitura_da_resposta` | Guarda pelo texto corrente e delegação ao classificador. `app/chamados/templatetags/chamados_tags.py:145,166–171` | Catraca, sítio da volta 1. `app/chamados/tests/test_e4c_f5_selo_catraca.py:27`, `app/docs/RELATO.md:1503` |
| `projecao_temporal_da_pergunta` | Lê os campos recebidos e aplica o horizonte. `app/chamados/juizes.py:1383–1403` | Selo de chamada e RED de horizonte. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–149`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:77–92` |
| `pergunta_respondida` | Julga instante e presença de texto no objeto recebido. `app/chamados/juizes.py:1533–1535,1551–1560` | Selo de chamada e RED de fontes divergentes. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–149`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:94–105` |
| `acao_corrigir_resposta` | Guarda de validação pela composta. `app/chamados/services/acoes_disputa.py:349–350` | Catraca e RED da correção. `app/chamados/tests/test_e4c_f5_selo_catraca.py:18`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–95` |
| `_resposta_validada` | Guarda comum a declarar e aplicar declaração. `app/chamados/services/declaracao_texto.py:217–229` | Catraca e dois REDs de entrada. `app/chamados/tests/test_e4c_f5_selo_catraca.py:19`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:103–134` |
| `_resolver_perguntas_por_chamado_resolvido`, sem credor | Carga de decisões e guarda pela composta. `app/chamados/services/disputa_emissao.py:2107–2112` | Catraca, RED de fecho e lote. `app/chamados/tests/test_e4c_f5_selo_catraca.py:20`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:142–170` |
| `par_do_intervalo`, irmã exigida validada | Primeira guarda composta da irmã. `app/chamados/services/materializacao.py:200–202` | Catraca no corpo comum e RED de plantio. `app/chamados/tests/test_e4c_f5_selo_catraca.py:16,22`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:228–236` |
| `par_do_intervalo`, irmã implausível | Segunda guarda sobre a mesma leitura. `app/chamados/services/materializacao.py:203–208` | Catraca no corpo comum e RED de implausibilidade. `app/chamados/tests/test_e4c_f5_selo_catraca.py:16,22`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:238–246` |
| `materializar_perguntas_validadas_da_disputa` | Seleção inicial SQL, releitura sob trava e guarda composta. `app/chamados/services/materializacao.py:2142–2163` | Catraca e RED de mudança sob trava. `app/chamados/tests/test_e4c_f5_selo_catraca.py:22`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:248–270` |
| `_validar_o_par` | Guarda composta da outra metade antes de carimbar. `app/chamados/services/validacao.py:655–659` | Catraca e RED do par. `app/chamados/tests/test_e4c_f5_selo_catraca.py:23–24`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:272–298` |
| `validar_pergunta` | Instante, texto e validação pela composta. `app/chamados/services/validacao.py:697–704` | Catraca e RED da validação. `app/chamados/tests/test_e4c_f5_selo_catraca.py:24`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:300–310` |
| `recusar_pergunta` | Guarda pela composta e foto anterior pela coluna. `app/chamados/services/validacao.py:915–923,930–932` | Catraca e RED da recusa. `app/chamados/tests/test_e4c_f5_selo_catraca.py:24`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:312–322` |
| `validar_lote_classe_a` | Guardas na entrada e após releitura integral. `app/chamados/services/validacao.py:1074–1084,1102–1105` | Catraca e dois percursos de RED. `app/chamados/tests/test_e4c_f5_selo_catraca.py:25`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:324–374` |
| `devolver_validacao_recusada`, linha 13 da matriz citada | Guarda por ato ou coluna, descarte do cache e escrita pareada. `app/docs/RELATO.md:752,768`, `app/chamados/services/validacao.py:267–296` | Catraca e REDs de A, B e prefetch velho. `app/chamados/tests/test_e4c_f5_selo_catraca.py:25`, `app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:69–128` |
| Adicional ao brief, `_eventos_dos_chamados` | Instantes do ato e texto da projeção nos três eventos. `app/docs/RELATO.md:1390,1503`, `app/chamados/services/historico.py:96–116` | Catraca e RED-5 do histórico. `app/chamados/tests/test_e4c_f5_selo_catraca.py:26`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:75–87` |

A reconstrução tem **15 sítios mais o histórico adicional**. A catraca atual enumera **13 funções em oito arquivos**, incluindo o histórico. Ao retirar esse adicional da conta da matriz, desdobrar os dois ramos de `par_do_intervalo` e agregar os dois juízes puros, a recontagem dá `13 − 1 + 1 + 2 = 15`. O visitante atual encontrou zero acusações nesse cadastro. Os outros ordinais e a identidade da amostra 9/9 estão [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_catraca.py:14–28,332–349`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–149`, `app/docs/RELATO.md:94,1503`

**Delta confirmado.** A observação de `ra.txt` de que o visitante deixava `update_fields` passar foi superada. A cópia acusa o campo quando falta atribuição ao mesmo campo no mesmo receptor, cobre concatenação e `+=`, e tem o controle da escrita correspondente. O RELATO registra a ampliação sem aumento da lista de funções. **NAO FECHA** por publicação e evidência histórica da matriz, com 1 dos 15 ordinais recuperado por citação. `app/docs/RELATO.md:18,21,39,64,94,752`, `app/chamados/tests/test_e4c_f5_selo_catraca.py:200–228,304–320,402–440`

## 32 · Triagem de `casefold`

O complemento 1435 pôs o caso 3e na obra. O PRONTO exige publicar a triagem motivada e o destino do sobrevivente para o Opus. `app/docs/RELATO.md:94,167,1373,1383,1387,1439`

| Registro | Parecer ou prova |
|---|---|
| Opus anterior da volta 1 | `casefold` entrou nas pendências que aquele laudo tratou como não bloqueantes. `app/docs/RELATO.md:1390`, `app/docs/BACKLOG.md:2064` |
| Verificador 4 | Zero bloqueios no delta, com `casefold` registrado como sobrevivente plausível, sem execução, destinado a anexo. `app/docs/RELATO.md:1439` |
| Replay Terra | **NAO BLOQUEIA** para `casefold`. `app/docs/RELATO.md:1374` |
| Replay Sol, identificado como 6.1 no registro seguinte | **BLOQUEIA** para `casefold`. `app/docs/RELATO.md:1374,1377` |
| Replay Sol 6.2 | **BLOQUEIA** para `casefold`. Também registra o sobrevivente distinto de `strip()` aplicado só à leitura do ato. `app/docs/RELATO.md:1377` |
| Teste atual `test_diferenca_so_de_caixa_conta` | Grava `Ato` no ato e `ato` na coluna, congela o relógio e exige `(1, 1)`. Na leitura estática do teste, aplicar `casefold` aos dois operandos, ou só ao texto do ato, torna o par igual e derruba o asserto. Aplicar só à coluna já minúscula preserva o resultado. `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,50–53`, `app/chamados/services/vigia_texto_resposta.py:19–25` |
| Teste atual `test_diferenca_so_de_espaco_conta` | Põe os espaços nas extremidades do texto da coluna. O ato da fixture já vem sem espaços, o que explica a sobrevivência de `strip()` só no ato para esses exemplos. `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–48`, `app/docs/RELATO.md:1377,1440` |
| Sobrevivente de `casefold` | O registro trata da variante que normaliza caixa na comparação de texto. O sítio atual dessa comparação no vigia é `contar`, linha 23. O patch histórico exato, o lado que ele alterava e a mordida no integrado estão [nao verificado]. `app/docs/RELATO.md:94,1374,1439`, `app/chamados/services/vigia_texto_resposta.py:19–25` |

O texto do ato vem de `DecisaoPergunta.leitura`, que conserva o texto do evento vigente e incorpora o passivo sem linha. O texto da composta vem da coluna. Estes são os comparadores do par, os leitores diretos do texto devolvido por `leitura` e os leitores pertinentes ao tratamento de caixa. `app/chamados/models.py:4302–4323,4348–4390`

| Leitor de produção | Valor lido e tratamento |
|---|---|
| `vigia_texto_resposta.contar` | Compara os dois textos por `!=`, com `or ''` na coluna. Diferença só de caixa conta. `app/chamados/services/vigia_texto_resposta.py:19–25` |
| `validar_classe_a.Command.handle` | Segundo comparador literal do par. Incrementa a classe de divergentes e exclui o caso antes do classificador. Também imprime um prefixo do texto do ato. O comando está registrado como operação assinada do DP. `app/chamados/management/commands/validar_classe_a.py:60–70,80–84,88–103`, `app/config/crons.py:1358–1362` |
| `vigiar_fio_morto.Command.handle` | Lê os dois **totais** do vigia, imprime divergência e universo, e carimba a divergência com esperado zero. `app/chamados/management/commands/vigiar_fio_morto.py:104–110,194–197,230–232,274–282` |
| ESTADO, `fio_estado._ler_carimbo` e `linha` | Compara o número do carimbo com o esperado e exibe o desvio. `app/bin/fio_estado.py` [nao verificado]. Implementação em `bin/fio_estado.py:121–126,191–194`, chamada em `bin/gerar_estado.py:260–261` |
| `ChamadoColaborador.ressuscitar_perguntas` | Copia o texto do ato para a foto da REABERTURA. `app/chamados/models.py:1705–1719` |
| `DisputaSupervisao.fechar` | Copia o texto do ato para a foto de SILÊNCIO. `app/chamados/models.py:2104–2112` |
| Registro da RETRATAÇÃO por motivo | Copia o texto do ato para a foto da RETRATAÇÃO. `app/chamados/models.py:2236–2246` |
| `PerguntaDisputa.resposta_display` | Lê o texto do ato. Converte IDs de posto ou escala em rótulo e casa opções por igualdade literal. A API, o envio após resposta e os cartões consomem essa property. `app/chamados/models.py:3865–3891`, `app/api/views.py:1163–1164,2579–2580`, `app/chamados/services/disputa_emissao.py:1571`, `app/templates/chamados/partials/_pergunta_card.html:31`, `app/templates/chamados/partials/_card_atendimento.html:106,143,186,394` |
| Tarefa do dia, `_pela_decisao` | Lê `ato_texto` do evento para testar vazio com `strip()`. A `LeituraDoAto` carregada separadamente fornece `validada_em` a esse percurso. `app/chamados/services/tarefa_do_dia.py:159–168,427–429`, `app/chamados/juizes.py:1533–1535` |
| `projecao_temporal_da_pergunta`, `pergunta_respondida`, `respondida_sem_texto` e `texto_sem_carimbo` | O temporal repassa o texto recebido ou o esvazia pelo horizonte. Os três predicados de resposta testam presença depois de `strip()`. `app/chamados/juizes.py:1391–1403,1533–1560` |
| Histórico e serializações da coluna | O histórico de chamados exibe o prefixo da coluna. A API também entrega o texto da coluna, pela composta ou diretamente, e a mensageria lê a coluna. Essas expressões preservam a caixa. `app/chamados/services/historico.py:98–113`, `app/api/views.py:1163,2579`, `app/api/views_mensageria.py:555–569`, `app/chamados/models.py:4382–4390` |
| `classificar_resposta_validacao` | Usa `lower()` sobre a coluna no ramo de resposta por escolha. `app/chamados/juizes.py:2043–2056` |
| `classe_do_lote` | Usa `lower()` e remoção de acentos sobre a coluna para a classificação. `app/chamados/services/validacao.py:1181–1183,1196–1214` |

**NAO FECHA.** Há um teste nominal de caixa e dois comparadores literais de produção. Os dois leitores que normalizam caixa classificam o texto da coluna em percursos próprios. A decisão motivada e o destino final do sobrevivente estão [nao verificado]. A execução nominal da variante no integrado também está [nao verificado]. `app/docs/RELATO.md:94,1374,1377,1439`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/chamados/management/commands/validar_classe_a.py:62–70`, `app/chamados/juizes.py:2050–2056`, `app/chamados/services/validacao.py:1196–1214`

## 35 · Censo dos carregadores e mapa K

Publicar **K → carregador → classe L/Q/R/M → ponto de toque** para os 69 K. O aval 1655 aparece no despacho do pré-desenho e o item é reiterado no PRONTO. A coluna A/B/C indica a volta comprovada pelo teste. A classe individual está [nao verificado] onde a tabela só identifica a volta. `app/docs/PROMPTS.md:2655–2657`, `app/docs/RELATO.md:94,167,1332,1335,1338`

| K | Volta ou classe | Carregador e ponto de toque | Vínculo do K |
|---|---|---|---|
| K1 | A | `_carga_da_tarefa_cls`. `app/chamados/services/tarefa_do_dia.py:533–545` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:10` |
| K2 | A | `alimentar_portas`. `app/chamados/juizes.py:1664–1669` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:11` |
| K9 | A | `reconciliar_perguntas_orfas_colab`. `app/chamados/reconciliador.py:1575` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:12` |
| K10 | A | `_orfas_abertas`. `app/chamados/reconciliador.py:1100–1103` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:13` |
| K16 | A | `_resolver_perguntas_por_chamado_resolvido`, prefetch do S128. `app/chamados/services/disputa_emissao.py:1978–1982` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:14,21,37–43` |
| K17 | A | `universo_do_fato`. `app/chamados/services/fio.py:1963–1964` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:15` |
| K18 | A | `_vizinhos_do_fato`. `app/chamados/services/fio.py:1595–1597` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:16` |
| K24 | A | `chamados_que_falam_de_batida`. `app/ponto/services/cartorio.py:145–146` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:17` |
| K43 | A | `perguntas_absolvidas_pela_celula`. `app/chamados/reconciliador.py:702–703` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:18` |
| K4 | B | `perguntas_vivas_de`. `app/chamados/juizes.py:1512–1513` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:13` |
| K11 | B | `fechar_beco_da_disputa`. `app/chamados/services/becos.py:86–87` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:14` |
| K23 | B | `montar_previa`. `app/folha/previa.py:64–67` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:15` |
| K26 | B | `insumos_em_regularizacao`. `app/chamados/services/declaracao_texto.py:589–590` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:16` |
| K29 | B | `perguntas_corrigiveis`. `app/chamados/juizes.py:1765` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:17` |
| K30 | B | `perguntas_respondidas_a_validar`. `app/chamados/juizes.py:1772–1775` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:18` |
| K31 | B | `_dias_das_perguntas_vivas`. `app/chamados/juizes.py:2435–2437` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:19` |
| K32 | B | `disputa_viva_do_chamado`. `app/chamados/juizes.py:2490–2491` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:20` |
| K45 | B | `medir`, perguntas da competência. `app/chamados/services/competencia_trancada.py:119–126` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:21` |
| K46 | B | `medir`, perguntas do chamado. `app/chamados/services/competencia_trancada.py:165–167` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:22` |
| K47 | B | `_resolver_perguntas_por_chamado_resolvido`, prefetch sem credor. `app/chamados/services/disputa_emissao.py:2107–2112` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:23`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222` |
| K54 | B | `contexto_calendario`. `app/colaboradores/services/calendario.py:504–505` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:24` |
| K55 | B | `dia_do_colab`. `app/ponto/services/dia_do_colab.py:64–65` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:25` |
| K68 | B | `_ja_perguntou`. `app/chamados/services/disputa_emissao.py:1436` | `app/chamados/tests/e4c_f5_voltaB_mutantes.py:26` |
| K8 | C | `universo_do_grupo`. `app/chamados/services/fio.py:2079–2081` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:13` |
| K13 | C | `_executar_dia`, carga e releitura após escrita. `app/chamados/management/commands/backfill_fio_morto_e4b.py:133–144` | `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264` |
| K21 | C | `respostas_intervalo_recusadas_paridade`. `app/chamados/services/materializacao.py:328–329` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:14` |
| K28 | C | `backfill_decisoes_pergunta.Command.handle`. `app/chamados/management/commands/backfill_decisoes_pergunta.py:531–534` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:15` |
| K34 | C | `_i6_pares`. `app/chamados/management/commands/auditar_invariantes_chamados.py:538–540` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:16` |
| K51 | C | `viva_em_disputa_fechada`. `app/chamados/services/vigia_fio.py:713–714` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:17` |
| K52 | C | `carimbos_sem_lastro_por_dono`. `app/chamados/services/vigia_fio.py:772–774` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:18` |
| K60 | C | `censo_alcance`. `app/chamados/services/vigia_fio.py:570–571` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:19` |
| K66 | C | `emitir_furo_retroativo.Command.handle`. `app/ponto/management/commands/emitir_furo_retroativo.py:82–83` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:20` |
| K69 | C | `fechar_becos_disputa.Command.handle`. `app/chamados/management/commands/fechar_becos_disputa.py:32–33` | `app/chamados/tests/e4c_f5_voltaC_mutantes.py:21` |
| K40 | R | `_descarimbados`, `get(pk=pk)` e julgamento por candidata. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` | `app/docs/PROMPTS.md:2659`, `app/docs/RELATO.md:1286,1325` |
| K41, K42 | B | Carregadores, classes e pareamento individual [nao verificado]. | `app/docs/RELATO.md:94,1329` |
| K3, K5, K6, K7, K12, K14, K15, K19, K20, K22, K25, K27, K33, K35, K36, K37, K38, K39, K44, K48, K49, K50, K53, K56, K57, K58, K59, K61, K62, K63, K64, K65, K67 | [nao verificado] | Pontos de toque [nao verificado]. | `app/docs/RELATO.md:94` |
| Sobra dedup | C | Prefetch por grupo em `dedup_perguntas_fato.Command.handle`. `app/chamados/management/commands/dedup_perguntas_fato.py:74–77` | Seletor `sobra` e RED próprios. `app/chamados/tests/e4c_f5_voltaC_mutantes.py:22`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:230–237` |

O **69** do censo bate com a soma antiga das classes, `10 + 29 + 5 + 25`. Há **34 K com ponto de toque** na tabela, `9 A + 14 B + 10 C + K40`. A linha 94 fecha a diferença de **35**, com 33 IDs no parêntese mais K41/K42 citados à parte. Os três cadastros dão **33 seletores**, dos quais 32 têm ID K e um é a sobra. K13 entra pela releitura e K40 pelo percurso avulso. K16 e K47 são dois prefetches na mesma função. `app/docs/RELATO.md:94,1332,1338`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,37–43`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`

**NAO FECHA.** Faltam a publicação dos 35 vínculos e a classificação individual do mapa histórico. `app/docs/RELATO.md:94,1332–1338`

## 36 · Seis R por sítio

Publicar os seis R, com o sítio e a razão da leitura por item. O pré-desenho contava cinco R. O aval 1830 registra K40 como R, e o PRONTO posterior pede a lista de seis. `app/docs/RELATO.md:94,167,788,1286,1332`, `app/docs/PROMPTS.md:2659`

| R | Sítio e prova |
|---|---|
| K3

[truncado em 20000 de 58879 caracteres]

## 21/11 05:20 · sessao a93264f21afd · - · - · origem=Ronald (tmux)

**45 — PUBLICAVEL.** Os quatro casos do item fecham em 4/4. O aval 0110 item 2 exige remedição pós-C com atribuição por sítio. A medida compara `eb1e49c1e` a `aa3bdd88c` e foi registrada na C4 `3c351fee4`. A estimativa de aproximadamente dez selos no pré-desenho tem inventário [nao verificado]. `app/docs/RELATO.md:94,167,793,797,1292,1332`

| Caso | Antes → depois | Atribuição |
|---|---|---|
| Um chamado fecha | 230 → 234 | +1 K43, +1 `_ancoradas` pelo plano, +1 `_ancoradas` pelo S143, +2 K24 no cartório, +1 lote de `fechar`, −2 reuso por `sem_veredito_entre`. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206` |
| Três chamados fecham | 294 → 298 | Mesma decomposição. `app/ponto/tests/test_selo_chokepoint_escrita.py:220–230` |
| Um chamado fica | 145 → 146 | +1 K17 `universo_do_fato` via `_nascer`. `app/ponto/tests/test_selo_chokepoint_escrita.py:248–253` |
| Três chamados ficam | 219 → 221 | +1 K17 e +1 consulta de decisões por `composta` em `lavrar_sem_turno_aberto`. `app/ponto/tests/test_selo_chokepoint_escrita.py:260–267` |
| Modal leve `chamados/modal/N` | 90 na A → 92 da B até C2 → teto 90 na cura C3 | Prefetch disparado duas vezes. A cura consulta o juiz uma vez por disputa/render. `app/docs/RELATO.md:798–799` `app/core/tests/test_selo_performance.py:143,377–380` `app/chamados/templatetags/chamados_tags.py:232–237` |
| Registro de linhas do plantio | `[434,455]` → `[437,458]` → `[438,459]` | Deslocamento das âncoras na C2 e no H4. `app/docs/RELATO.md:100,120,128,799` `app/core/tests/test_medida_turno_do_par.py:271,559` |
| O261 | 8 era o teto requerido no 1710, 9 medido e aceito na A2 | Releitura por candidata sob trava, um SELECT de decisões por candidata em 3 e 6. `app/docs/RELATO.md:1305,1307,1309,1329` `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207` `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` |
| `_ancoradas` | 1 consulta no selo anterior → 2 para 1 e 5 ancoradas | Uma carga e um SELECT de decisões em lote, aval 1830 7-vii. `app/docs/RELATO.md:1325` `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569–593` |
| E2 e C11 | 30→36 e 48→56 nos vermelhos intermediários. Valores nominais após cura [nao verificado] | Regra de igualdade preservada, verdes na A2 segundo o RELATO. `app/docs/RELATO.md:783,1305,1309` `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938` `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402` |
| Modal prod-like | Teto 2.837 para 10 chamados e 330 perguntas, com remedições anteriores à F5 | A menção na volta C trata de label errado e registra tetos preservados. Remedição F5 [nao verificado]. `app/docs/RELATO.md:1286` `app/chamados/tests/test_selo_teto_modal.py:32–33,81–97,166–184` |

No chokepoint C4 houve apenas SELECT em `chamados_decisaopergunta` e `chamados_perguntadisputa`, com zero INSERT, UPDATE, DELETE ou SAVEPOINT adicionais. SQL e contagem nominal no integrado [nao verificado] para os itens 47/48. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205,220–229,248–252,260–266` `execucoes.txt:2` `relato_b.txt:700,704`

**49 — NAO FECHA.** O 1710 proíbe tocar o harness das 11 famílias, o DUBLE-1945 e 2005 abrem a exceção do dublê comum. O mapa nominal de 12 famílias/112 ocorrências ainda é necessário para ligar cada uma das 11 às exceções. Recontagem AST pela regra do selo atual dá 486 asserts, iguais aos 486 declarados para o MAIN em nove módulos. `app/docs/RELATO.md:94,1309,1311–1314,1338` `app/docs/BACKLOG.md:2055` `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`

| Módulo em `app/chamados/tests/` | MAIN declarado → cópia | Alteração visível e linha |
|---|---:|---|
| `e3_v8_apoio` | 0→0 | Dublê da pergunta e modelo falso. `e3_v8_apoio.py:12,77,167,197` `test_e4c_f5_duble_conforme.py:16` |
| `test_e1_v10` | 31→31 | Fábrica de pergunta. `test_e1_v10.py:23,121` `test_e4c_f5_duble_conforme.py:17` |
| `test_e3_c8b_sem_banco` | 86→86 | Dublê de modelo. `test_e3_c8b_sem_banco.py:14,32` `test_e4c_f5_duble_conforme.py:18` |
| `test_e3_degrau_dono_e_porta` | 134→134 | Dublê de pergunta e modelo. `test_e3_degrau_dono_e_porta.py:20,264–273,296,311,338,350` `test_e4c_f5_duble_conforme.py:19` |
| `test_e3_pos_suite_sem_banco` | 29→29 | Fábrica de pergunta. `test_e3_pos_suite_sem_banco.py:13,54` `test_e4c_f5_duble_conforme.py:20` |
| `test_e3_tarefa_admin_sem_banco` | 7→7 | Dublê de pergunta e modelo. `test_e3_tarefa_admin_sem_banco.py:23,87,112,185,202` `test_e4c_f5_duble_conforme.py:21` |
| `test_e4b_c4v7_memoria` | 65→65 | Dublê de modelo. `test_e4b_c4v7_memoria.py:19,359,381` `test_e4c_f5_duble_conforme.py:22` |
| `test_e4b_classe_e_memoria` | 122→122 | Dublês de pergunta e modelo, âncoras classe i e prefetch identidade classe v. `test_e4b_classe_e_memoria.py:17,39–42,51,193–194,286,336` `test_e4c_f5_duble_conforme.py:23` `app/docs/RELATO.md:1305,1309` |
| `test_pergunta_viva` | 12→12 | Fábrica comum. O RELATO o coloca fora da lista fechada. `test_pergunta_viva.py:5,12` `test_e4c_f5_duble_conforme.py:24` `app/docs/RELATO.md:1314` |

Reúsos nominais dos dublês comprovados por imports: C11 e portas V8 → `e3_v8_apoio`; C6, C4v14, C4 tranca, C11 → `test_e4b_classe_e_memoria`; C8c → C8b; volta 7 → `e3_volta7_apoio`. `app/chamados/tests/test_e3_r3_c11_sem_banco.py:28` `app/chamados/tests/test_e3_portas_tarefa.py:509` `app/chamados/tests/test_e4b_c6_memoria.py:10` `app/chamados/tests/test_e4b_c4v14_memoria.py:13` `app/chamados/tests/test_e4b_c4_tranca_memoria.py:15` `app/chamados/tests/test_e4b_c11_memoria.py:8` `app/chamados/tests/test_e3_c8c_sem_banco.py:11` `app/chamados/tests/test_e3_volta7_sem_banco.py:11`

Os usos adicionais documentados são C7 classe ii/v e suprajuiz DUBLE-1945, e B3 com dublê da volta 7 classe ii, dublê de queryset CEF classe v e âncoras CEF/K6/M1/C7 classe i. A classificação como ii para carregar o juiz real em Volta7/C5/C6/degrau foi registrada como contestável. `app/docs/RELATO.md:1296,1304` `app/chamados/tests/test_e3_r3_c7_sem_banco.py:13,181–192` `app/ponto/tests/test_supra_juiz_executor.py:16` `app/chamados/tests/e3_volta7_apoio.py:12,79,111` `app/chamados/tests/test_cef_c1_sem_banco.py:88,310` `app/chamados/tests/e3_r3_c6_mutantes.py:48–51` `app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:113` `app/chamados/tests/test_e3_r3_c5_sem_banco.py:176` `app/chamados/tests/test_e3_r3_c6_sem_banco.py:296` `app/chamados/tests/test_e3_degrau_dono_e_porta.py:512`

Na A1, o RELATO discrimina 92 testes E3/E4b em 14 módulos, mais 11 `test_pergunta_viva` e 11 F5/tetos, total 114. São unidades diferentes das 11 famílias e dos nove módulos do selo. `app/docs/RELATO.md:1314,1338` `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–24`

**51 — NAO FECHA.** Dois RED-8 confirmados em `JuizesVolta2Test`, a 12ª posição histórica [nao verificado]. Os avais citados nos testes são 1735 item 7(i) para a fixture via porta e 2045 itens 3 e 4 para tipo e custo. `app/docs/RELATO.md:94,167,1308–1309,1338` `app/chamados/tests/test_e4c_f5_volta2_juizes.py:26,125,142,163,174`

| RED-8 | Prova da cópia |
|---|---|
| `test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco` | Carga custa igual para 1 e 5, cálculo pré-carregado custa zero queries e juiz recebe `LeituraComposta`. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:129–152` |
| `test_red_8_custo_montar_chamados_fio_uma_e_cinco` | Mesmo total para 1 e 5, até um SELECT de decisões, todos os PKs lidos com prefetch, na fixture maior só a quinta é viva. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:154–186` |

**80 — NAO FECHA.** O 0545 linha 6 autoriza a classe vii e o RELATO pede a publicação nominal dos esperados antigos. O texto está citado como conteúdo do corpo de `604505878` e está [nao verificado]. Placar nominal no integrado [nao verificado]. `app/docs/RELATO.md:62,94,167,283,781` `execucoes.txt:30`

| Caso | Esperado na cópia |
|---|---|
| K47 `escolhas_sem_credor` | Validada pela porta, ambas as validações presentes e iguais em presença, coluna em `AGORA`, 0 resolvidas, 1 disputa fechada, zero materializadas, mesmos SELECTs para 3×6 e PKs lidos. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40–52,197–222` |
| `corrigir` | `info` preserva resposta e carimbo, `sucesso` troca texto e instantes. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–95` |
| `declaracao` | `validada_sem_registro` preserva, `ok` grava resposta e permanece sem validação. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:103–120` |
| `aplicar_declaracao` | `validada_sem_registro` ou `validada`, mesmo PK com 1 ou 2 atos de validação. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:122–134` |
| `fecho` | Contadores 0/0 no isolador, retida intacta e aberta materializada via `CHAMADO_ENCERRADO`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:142–153` |
| `fecho_3x6` | Preserva todos os PKs validados e não materializados, SELECTs `[1,1]`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:155–170` |
| `propostas_3x6` | Encontra todas as notas e nenhuma proposta aceita, SELECTs `[1,1]`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:172–187` |
| `tela_3x6` | Aceita nos dias alternados esperados, SELECTs `[1,1]`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:189–211` |
| `irma_plantio` | Forma par quando validada, ou impede com `respondida, sem validacao`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:213–236` |
| `irma_implausivel` | Forma par quando validada, ou impede com `hora implausivel`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:213–226,238–246` |
| `materializar_ato_mudou_sob_trava` | Revalidação em outra instância produz 1 materializada, resposta sem revalidar produz 0. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:248–270` |
| `validar_par` | Os dois PKs são materializados, a segunda metade preserva ou recebe nova validação. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:272–298` |
| `validar` | `ja_validada` ou `ja_no_espelho`, 1 ou 2 atos, respectivamente sem ou com materialização. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:300–310` |
| `recusar` | `ja_validada`/`recusada`, contagem de RECUSA 0/1, texto preservado ou limpo. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:312–322` |
| `lote_entrada` | Recusa `Ja validada.` ou 1 validada com novo ato. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:324–336` |
| `lote_entrada_3x6` | Todos os PKs recusados por validação anterior, SELECTs `[1,1]`. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:338–348` |
| `lote_rele_ato_escrito_em_outra_instancia` | Texto permanece validado e entra em `nao_gravados`, hora devolvida entra em `recusados`, zero materialização. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:350–374` |
| `diagnostico_refresh_parcial_e_integral` | Refresh parcial conserva cache antigo, integral remove cache e vê o ato. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:376–386` |
| `devolucao_e_o_escritor_da_coluna` | `True`, coluna e composta nulas, 1 DEVOLUÇÃO. O anterior registrado era zero DEVOLUÇÃO em A. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:388–396` `app/docs/RELATO.md:762` |

A D tem 18 métodos `test_`, 16 com prefixo `test_RED_` e dois controles/diagnósticos. Das 14 variantes cadastradas, 11 trocam a fonte e três tiram lote. Todas as 14 âncoras textuais têm uma ocorrência única na função atual. O módulo da D aciona por nome apenas `fecho_sem_lote`, `tela_sem_lote` e `lote_entrada_sem_prefetch`. O RELATO registra 14 mortes na vD2, oito sobreviventes em `c8eb8eddf` (`corrigir`, `declaracao`, `irma_plantio`, `irma_implausivel`, `materializar`, `validar_par`, `lote_entrada`, `lote_releitura`) e mordida por outro teste para `fecho`, `validar`, `recusar`. O mutante nominal do K47 retira o segundo prefetch e seu resultado anterior foi 8≠14. Necessidade de alteração de código no integrado [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–396` `app/chamados/tests/e4c_f5_voltaD_mutantes.py:13–66` `app/chamados/tests/e4c_f5_voltaB_mutantes.py:23,83–114` `app/docs/RELATO.md:62,784,786,790` `execucoes.txt:30`

## 21/11 05:18 · sessao 9c23c0df2de7 · - · - · origem=Ronald (tmux)

**55 — NAO FECHA.** A recontagem pela catraca encontrou 44 chamadas diretas em 22 arquivos, 40 ao juiz de resposta e quatro ao temporal. São 43 com composta e a exceção SQL de `creditos_perguntas`. A soma histórica 41 + 55 fecha em 96, mas a lista histórica por identidade e volta está [nao verificado]. O RELATO distingue 41 reconhecidas de 36 fora das permitidas na base e mede 24, 10 e zero fora nas voltas A, B e C. Enviei a tabela das 44 chamadas e a atribuição nominal recuperável de B e C. `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160,188–193`, `app/chamados/services/raiox.py:135–147`, `app/docs/RELATO.md:37,94,1306,1314,1317,1338`

**102 — NAO FECHA.** As 53 chamadas explícitas de produção se repartem em 40 que decidem pelo instante, uma que só serializa o instante e 12 que usam somente a validação. Entre as 40, o histórico da API também serializa o instante e a passagem de ausência também registra o instante na trilha. A mesma API obtém `p.estado` por outra leitura, inclusive ao serializar pendentes. O APK está [nao verificado]. O histórico informa 42 DECIDE, incluindo três por dúvida, e um EXIBE. A frase histórica que enumera sítios tem 41 âncoras, correspondentes a 39 chamadas ao juiz de resposta mais `_slot_esperado` e `espera_o_texto_do_colaborador`. A diferença de unidade e a falta da classificação individual dos três por dúvida impedem equiparar 40 a 42. Enviei a tabela nominal das 53 chamadas e dos dois leitores diretos históricos. `app/docs/RELATO.md:92,148–149,157,160`, `app/api/views.py:2566–2601`, `app/chamados/models.py:1707–1719,3259–3268,4382–4399`, `app/chamados/juizes.py:351–354`, `app/chamados/services/fio.py:189–190`, `app/chamados/services/materializacao.py:1060–1067,1098–1104`, `app/chamados/services/historico.py:98–107`

**32, evidência adicional.** O comando `validar_classe_a` também compara literalmente os dois textos e exclui o caso divergente da lista de candidatos. O `vigiar_fio_morto` lê o par de totais, imprime a contagem e carimba apenas a divergência. O ESTADO compara a chave do carimbo genericamente com o esperado. Dois leitores de classificação usam `lower()` no texto projetado, enquanto `resposta_display` casa a opção pelo valor literal. O RED `Ato` × `ato` acusa normalização que transforme o lado do ato em `ato`, inclusive a bilateral. Uma normalização aplicada apenas ao lado já minúsculo preserva o desfecho desta fixture. O sobrevivente adicional de `strip()` unilateral está documentado separadamente, e as duas variações da fixture de espaços acrescentam espaços na projeção. `app/chamados/management/commands/validar_classe_a.py:60–70,80–84`, `app/chamados/management/commands/vigiar_fio_morto.py:104–110,194–197,230–232,263–282`, `bin/fio_estado.py:121–126,191–194`, `app/chamados/juizes.py:2043–2056`, `app/chamados/services/validacao.py:1196–1214`, `app/chamados/models.py:3865–3891`, `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–53`, `app/docs/RELATO.md:1377,1439`

## 21/11 05:18 · sessao 4d03090590a1 · - · - · origem=Ronald (tmux)

**35 — NAO FECHA.** O censo registra 69 carregadores. A cópia identifica 34 K por sítio, sendo 9 da A, 14 da B, 10 da C e K40. A lista entre parênteses no RELATO tem 33 lacunas e K41/K42 aparecem logo depois, totalizando os 35 faltantes. Os 33 seletores incluem a sobra dedup e deixam K13 e K40 fora dessa contagem de seletores. Falta o mapa histórico de classe e carregador. `app/docs/RELATO.md:94,1332,1338` · `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27` · `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23` · `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`

**36 — NAO FECHA.** O pré-desenho registra cinco R e o 1830 acrescenta K40, que relê e julga cada candidata em `_descarimbados`. A tabela confirma 1 dos 6 percursos, com K3, K19, K20, K22 e K58 sem sítio histórico demonstrado. `app/docs/RELATO.md:94,788,1286,1325,1332` · `app/docs/PROMPTS.md:2659` · `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` · `vPRONTO6_r.txt`, item 36.

**40 — NAO FECHA.** A cópia tem 14 carregadores K, 15 métodos de lote por repetição de K46, 14 leitores identificáveis e cinco variantes funcionais. O recorte nominal oferece 28 âncoras de K e leitores com unidades distintas. Os 29 cobertos e 27 editados são históricos, e o `24→27` da B2 conta asserts de testes de lote. Faltam a conciliação por sítio, os dois carregadores de K41/K42 e a identificação de 5 L + 3 Q. `app/docs/RELATO.md:94,1296,1306,1329` · `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:54–251` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:66–296` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–55` · `vPRONTO6_r.txt`, item 40.

**41 — NAO FECHA.** O registro de construção declara 25 K + sobra e a triagem contabiliza 24 dos 25 K + sobra, com K40 avulso em R. A cópia confirma dez K, contando K13 pela releitura, uma sobra e dez leitores de manutenção. O vínculo histórico dos K restantes e das três M permanece aberto. `app/docs/RELATO.md:94,1286,1289,1306` · `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:89–264` · `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:65–143`

As linhas nominais de produção de `ra.txt` para esses itens foram conferidas e encaminhadas ao agente principal. Os pontos e as âncoras coincidem com a cópia. O diff textual entre hashes está [nao verificado].

## 21/11 05:11 · sessao 7e758928db7f · E4C-F5-CONSTROI · leitura · origem=Code

## 1. PRONTO na árvore final

| Item | Estado | Prova ou pendência |
|---|---|---|
| 01. Matriz e censo de entrada | FALTA | O RELATO registra 15 sítios, amostra 9/9 e zero perguntas de lei. Falta o vínculo nominal entre os 15 sítios do brief, os selos e o PRONTO. `app/docs/RELATO.md:94,1503` |
| 02. Testemunha da leitura | DEMONSTRADO | `leitura` devolve `LeituraDoAto` com 3 campos. `composta` devolve `LeituraComposta` com 5. `app/chamados/models.py:19–25,4348–4390` |
| 03. Conservação do passivo sem ato | DEMONSTRADO | A leitura intercala o passivo sintético com os atos. O teste fixa os dois juízes e o efeito do horizonte no passivo. `app/chamados/models.py:4302–4323,4361–4379`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:59–75` |
| 04. RED-5 do histórico | DEMONSTRADO | Teste e mutante distinguem instante de resposta, validação e materialização, com texto da projeção nos três eventos. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:75–87`, `app/chamados/services/historico.py:96–111` |
| 05. SILÊNCIO e dono ADMIN | RETIRADO-POR-AVAL | 1255 item 3, “o SILENCIO dono admin SAI da F5 e vira fatia propria E4C-F5-SILENCIO-ADMIN”. A regressão permanece. `avais_posteriores.txt:129–131`, `app/docs/BACKLOG.md:1967`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343–344` |
| 06. Catraca de leitores crus | DEMONSTRADO | Recontagem AST de 13 funções em 8 arquivos, zero acusações. A regra acusa `update_fields` sem atribuição ao campo no mesmo receptor. O controle de escrita passa e três mutantes acusam nos dois modos. A lacuna editorial do mapa está no 01. `app/chamados/tests/test_e4c_f5_selo_catraca.py:17–28,214–228,304–349,402–440`, `execucoes.txt:14–15,24–27` |
| 07. Três passagens | HISTORICO | 1945 item 3 manda valer pelo log e hash de A/B/C. O instrumento da volta está na cópia. Os recibos das três passagens estão [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `bin/volta_intermediaria.sh:4,32` |
| 08. Ordem, commits e RED antes da cura | HISTORICO | Valem a ordem de commits e o RED no hash de cada volta. Logs, ordem completa e pureza no primeiro commit da A [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:271–304` |
| 09. Verificador do delta por volta | HISTORICO | Valem os laudos nos hashes das voltas. Há registro da triagem c3. Laudos completos de A/B/C [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/docs/RELATO.md:100` |
| 10. Mutantes tira-lote | SO POR EXECUCAO | 33 seletores, 9 A + 14 B + 10 C. A execução 5 está em curso. Resultado nominal, inclusive K16 e K47, EXECUCAO NAO LISTADA. `execucoes.txt:30`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23` |
| 11. Sombra dos juízes e do passivo | SO POR EXECUCAO | Execução 4 refez `item11` em `9e90d`. O RELATO registra 51.953 julgadas de cada lado, 82 chaves comparadas e zero blocos diferentes. Série completa por volta [nao verificado]. `execucoes.txt:29`, `app/docs/RELATO.md:12,33` |
| 12. Sombra das decisões por porta | SO POR EXECUCAO | Execução 4 refez `item12`. O RELATO registra 141 tentativas, 152 decisões iguais e rollback conferido em 141. A medida é amostral. `execucoes.txt:29`, `app/docs/RELATO.md:12,29,45` |
| 13. Entregues e exportadas | SO POR EXECUCAO | Execução 4 cobre A/B entregue e 6-A1 MAIN × F5. A/B deu 0/0 em 25.766 entregues. O 6-A1 registra três perguntas com movimento de TXT reproduzido no MAIN. Uma lista geral de decisões alteradas nas exportadas está [nao verificado]. `execucoes.txt:29`, `app/docs/RELATO.md:22,27,66,78` |
| 14. CHECK-DE-TELA | ETAPA FUTURA | Comparativo de resultado, consultas e escritas no espelho, Central, tarefa pelo serviço e API. `avais_depois_do_vPRONTO5.txt:29`, `app/ponto/urls.py:10`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825–832`, `app/api/urls.py:92` |
| 15. Verificador inteiro com zero bloqueios | HISTORICO | O 1945 preserva o laudo da volta. O teste do memo C6 está presente. Laudo integral com zero bloqueios [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_voltaFINAL.py:12–13` |
| 16. Suíte inteira pré-Opus | SO POR EXECUCAO | Execução 1 em `b0935c1f1`, `rc=0`. Execução 2 em `9e90d`, `rc=1`, 17.274 testes e 16 erros. Placar nominal da execução 1, recibo da Pista 2 e precedência da publicação completa [nao verificado]. `execucoes.txt:2–3`, `app/docs/RELATO.md:2–3,10`, `bin/suite.sh:23–33,236–257` |
| 17. Opus final único | ETAPA FUTURA | Estado definido pelo 1945. A tentativa da execução 6 em `b0935c1f1` terminou REPROVADO. `avais_depois_do_vPRONTO5.txt:29`, `execucoes.txt:31`, `opus.r:1` |
| 18. Compositor nos três sítios | DEMONSTRADO | Um compositor tipado. Partição do app, property e fio delegam a ele, com instante do ato e texto da projeção. `app/chamados/models.py:3838–3844,4382–4390`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:187–190` |
| 19. Grep publicado | DEMONSTRADO | O RELATO enumera o compositor e as três delegações. As quatro âncoras batem na cópia. `app/docs/RELATO.md:94`, `app/chamados/models.py:3844,4382`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/fio.py:189` |
| 20. RED da partição do app | SO POR EXECUCAO | `a1_app_coluna` e `a1_app_texto_do_ato` têm teste dirigido. Mordidas nominais no hash final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32–53` |
| 21. RED da property | HISTORICO | Valem log e hash da base. O teste fixa texto divergente e projeção carimbada após reabertura. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:37–54,87–89` |
| 22. RED do fio | SO POR EXECUCAO | Há três variantes dirigidas, coluna, texto do ato e composição local. Mordidas nominais no hash final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71` |
| 23. RED de `sem_veredito_entre` | HISTORICO | Valem log e hash da base. Há dois casos de divergência no teste. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:76–81` |
| 24. RED de `estado` | HISTORICO | Valem log e hash da base. O teste exige `PENDENTE` nos dois casos divergentes. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:83–85` |
| 25. Consumidores por consequência | DEMONSTRADO | Fechamento, CSF e Pauta chegam ao leitor comum. Identidade histórica dos arquivos [nao verificado]. `app/chamados/models.py:1998–2027,2035,2072`, `app/ponto/supra_juiz.py:573–574`, `app/chamados/services/competencia_trancada.py:124–126` |
| 26. Vigia de texto em lotes | DEMONSTRADO | Prefetch de decisões e `iterator(chunk_size=lote)`, com lote padrão de 2.000. `app/chamados/services/vigia_texto_resposta.py:8–25` |
| 27. Contagem preservada no vigia | SO POR EXECUCAO | O teste fixa `(0,2)`, `(1,2)`, `(1,1)` e `(1,3)`. Comparação antes × depois com a mesma fixture, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38` |
| 28. RED de consultas do vigia | SO POR EXECUCAO | O teste exige 3 SELECTs de decisões para 5 perguntas em lotes de 2. RED nominal contra a carga anterior, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` |
| 29. Remoção da chave sem leitor | DEMONSTRADO | O carimbo conserva `divergencia_texto_resposta`. O teste exige a ausência de `divergencia_texto_resposta_universo`. `app/chamados/management/commands/vigiar_fio_morto.py:274–284`, `app/chamados/tests/test_e4c_f5_contador_texto.py:56–70` |
| 30. Decisão técnica publicada | DEMONSTRADO | Remoção, custo e reversão estão registrados. `app/docs/RELATO.md:1382` |
| 31. Selo AST por identidade | DEMONSTRADO | O selo segue parâmetro, aliases e alvo do laço. Dois testes conferem a renomeação e a detecção da pergunta inteira. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:164–185,220–251`, `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38` |
| 32. Triagem de `casefold` | FALTA | O teste de diferença só de caixa existe. O RELATO mantém os pareceres divergentes e registra o sobrevivente como plausível. Falta decisão motivada para o Opus. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/docs/RELATO.md:94,1374,1377` |
| 33. PRONTO publicado | FALTA | A publicação de 113 itens identifica `c8eb8eddf`. A atualização de `9e90d` resume cinco itens, sombra e voltaE. Falta o quadro consolidado do hash integrado com as pendências. `app/docs/RELATO.md:12–13,92–94`, `execucoes.txt:1,30–31` |
| 34. Pureza AST dos dois juízes | DEMONSTRADO | Recontagem AST com zero impurezas e uma isenção nominal no juiz temporal. Identidade textual com a base [nao verificado]. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:163–185,271–304` |
| 35. Censo dos carregadores e mapa K | FALTA | As tabelas A/B/C identificam seletores e a cópia tem K13 e K40. Falta a conciliação dos 69 K com carregador, classe e ponto de toque. O RELATO explicita 35 K sem carregador nominal, além do vínculo K41/K42. `app/docs/RELATO.md:94`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264` |
| 36. Seis R declarados | FALTA | K40 tem percurso identificável. Faltam os vínculos nominais de K3, K19, K20, K22 e K58 e a declaração por sítio. `app/docs/RELATO.md:94`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` |
| 37. K40 por item | DEMONSTRADO | `_descarimbados` faz `get(pk=pk)` e julga cada candidata. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` |
| 38. Sobra por grupo | DEMONSTRADO | Deduplicação carrega as decisões por grupo, com teste 3 × 6 e seletor da sobra. `app/chamados/management/commands/dedup_perguntas_fato.py:75–76`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:230–237`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:22` |
| 39. REDs 3 × 6 da A | SO POR EXECUCAO | Recontados 12 métodos no módulo, com 11 comparativos de lote sobre 9 K e um O261 por candidata. A fixture do fio tem 1 × 5. Placar nominal e RED da passagem, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:44–240`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:154–186` |
| 40. REDs e mutantes da B | FALTA | Recontados 15 métodos para 14 K, com K46 em dois métodos. Há 14 tira-lote e 5 variantes funcionais. Falta conciliar os 29 cobertos com os 27 editados, K41/K42 e 5 L + 3 Q. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:54–251`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–52`, `app/docs/RELATO.md:94` |
| 41. REDs e mutantes da C | FALTA | Recontados 10 testes de lote, 9 K e uma sobra, mais o teste de releitura K13. Falta vincular nominalmente manutenção, 3 M e sobra. `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:89–264`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/docs/RELATO.md:94` |
| 42. RED novo 3 × 6 do C11 | DEMONSTRADO | K2 e K1 cobram SELECTs de decisões, PKs julgados e PKs devolvidos pelo C11. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23–40,207–240` |
| 43. Tetos existentes E2 e C11 | SO POR EXECUCAO | Os dois selos estão na cópia. A suíte final dá verde agregado. Diff de preservação e placar nominal, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402` |
| 44. O261 por candidata, teto 9 | SO POR EXECUCAO | Um teste cobra igualdade de custo por candidata e teto 9. Outro cobra um SELECT de decisões por candidata. Remedição nominal final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` |
| 45. Tabela de selos remedidos | FALTA | O RELATO publica os quatro deltas do chokepoint e remete a atribuição nominal ao anexo. Falta consolidar antes/depois, motivo e rodada por selo. `app/docs/RELATO.md:94`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205–207`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:578–593` |
| 46. Quatro tetos do chokepoint | DEMONSTRADO | Tetos anotados com sítio e classe de leitura, 230→234, 145→146, 294→298 e 219→221. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267` |
| 47. Quatro casos verdes do chokepoint | SO POR EXECUCAO | Existem quatro casos, 1 e 3 furos × fecha e fica. Verde agregado na execução 1. Remedição nominal final, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/ponto/tests/test_selo_chokepoint_escrita.py:467–475,502–510` |
| 48. Atribuição SQL e escritores novos | SO POR EXECUCAO | As anotações atribuem o delta anterior a SELECT e registram zero INSERT/UPDATE/DELETE/SAVEPOINT a mais. Captura final por SQL, verbo e sítio, EXECUCAO NAO LISTADA. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205,220–229,248–252,260–266` |
| 49. Harness dentro das exceções autorizadas | FALTA | O selo cadastra nove módulos e compara contagem de asserts. Falta a partição nominal em 11 famílias e o vínculo de cada alteração à exceção. `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`, `app/docs/RELATO.md:94` |
| 50. Onze famílias verdes | SO POR EXECUCAO | Há 11 métodos em `test_pergunta_viva` e seis no selo do dublê. A execução 1 dá verde agregado. Placar por família, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/chamados/tests/test_pergunta_viva.py:16–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:51–87` |
| 51. Décima segunda família | FALTA | O RELATO nomeia os dois RED-8. Falta a identificação da família no inventário de 12. `app/docs/RELATO.md:94`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:145–186` |
| 52. Medida de `respondida` | SO POR EXECUCAO | O RELATO documenta a comparação anterior nos dois sentidos. Sonda específica final da property, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92,782`, `app/chamados/models.py:3838–3844` |
| 53. Medida de `sem_veredito_entre` | SO POR EXECUCAO | O leitor está na cópia e o RELATO aponta medição anterior. Comparativo específico final por decisão, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92`, `app/chamados/models.py:2002–2027` |
| 54. Medida de `estado` | SO POR EXECUCAO | O leitor está na cópia e o RELATO aponta medição anterior. Comparativo específico final por rótulo, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92`, `app/chamados/models.py:3249–3268` |
| 55. Conversão dos 96 chamadores | FALTA | Recontados 44 chamadores diretos em 22 arquivos, 40 do juiz de resposta e 4 do temporal, com 43 compostas e uma exceção. Falta a lista histórica dos 41 diretos + 55 indiretos por volta. `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160`, `app/docs/RELATO.md:37,94` |
| 56. Selo de chamada final em zero | DEMONSTRADO | Zero chamadas fora da catraca na recontagem AST, com uma exceção nominal para `creditos_perguntas`. `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160,188–193`, `app/chamados/services/raiox.py:135–147` |
| 57. Catraca publicada por volta | SO POR EXECUCAO | O RELATO publica 36 fora das permitidas na base, 24 na A, 10 na B, 0 na C e 0 em `9e90d`, com 44 reconhecidas no final. A execução 4 dá o item por fechado. Origem do número histórico 19 [nao verificado]. `app/docs/RELATO.md:12–13,37`, `execucoes.txt:29` |
| 58. Commit Opus de `models.py` | HISTORICO | Recontados 9 métodos, 6 com RED nominal. Valem log e hash do commit próprio. Ordem anterior à A e vínculo K41/K42 [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:57–167`, `app/docs/RELATO.md:94` |
| 59. Limite da exceção de folha | SO POR EXECUCAO | Execução 4 registra diff restrito ao prefetch e ao argumento composto do juiz. Os dois sítios estão na cópia. `execucoes.txt:29`, `app/folha/previa.py:64–67,139–146` |
| 60. Prévia da competência 11 | SO POR EXECUCAO | O RELATO registra 4 empresas, 450 colaboradores, 5.861 linhas e zero diferenças em cópia anterior. Comparativo final por colaborador e dia, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:782`, `app/folha/previa.py:64–67,135–146` |
| 61. Fixtures dos dois RED-8 | DEMONSTRADO | Há dois casos, com ato pela porta e projeção passiva. O fio chega à quinta pergunta. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:27–35,119–186` |
| 62. Chamadas ORM da volta 2 | DEMONSTRADO | Casos de fonte passam `D.composta(p)`. A instância ORM do passivo serve de controle. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–117` |
| 63. Mutante mira o compositor | DEMONSTRADO | A transformação renomeia o parâmetro e troca a chamada ao compositor pela instância ORM. `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38` |
| 64. Tabela F2 por leitor | DEMONSTRADO | Sete leitores, cinco `False` e dois `True`, com motivo por leitor. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498–521` |
| 65. Instante e validação pelo ato | DEMONSTRADO | Dois casos opõem a fonte do ato à projeção. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:523–544` |
| 66. Leitor F1 B2 | DEMONSTRADO | O teste exige uma delegação ao juiz e argumento `LeituraComposta`. `app/chamados/tests/test_f1nova_b2_leitores.py:87–97` |
| 67. `_ancoradas` | DEMONSTRADO | O selo verifica 2 consultas com 1 e com 5 ancoradas, incluindo o prefetch. `app/chamados/models.py:349–366`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569–593` |
| 68. Inventário das adaptações | FALTA | O RELATO aponta a tabela e identifica dois usos sem aval individual, contrato de fechamento e `test_e4_expand`. Falta fechar o inventário por uso, antes/depois e aval. `app/docs/RELATO.md:94`, `app/chamados/tests/e4b_c12_ast.py:18–27`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:76–80` |
| 69. Placar raio-X pela face SQL | DEMONSTRADO | `creditos_perguntas` conserva `values_list` e adapta a tupla para o juiz. `app/chamados/services/raiox.py:135–147`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28` |
| 70. Registro nominal do raio-X | DEMONSTRADO | O complemento da E4D nomeia `raiox.py:135` e a face SQL. `app/docs/BACKLOG.md:2048` |
| 71. `resposta_display` lê o ato | DEMONSTRADO | A property formata a resposta lida do ato. A tabela F2 fixa a expectativa. `app/chamados/models.py:3865–3891`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:511–521` |
| 72. Registro nominal de `resposta_display` | DEMONSTRADO | O complemento da E4D nomeia a property e o texto de exibição pelo ato. `app/docs/BACKLOG.md:2048` |
| 73. Medida vigente de display | SO POR EXECUCAO | Execução 4, `item73_display.out`, universo 51.953, erros 0, literal 16, LIKE 0 e CRU 0. A primeira sonda teve 36.212 erros de instrumento e está registrada separadamente como inválida. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| 74. Pouso único | ETAPA FUTURA | Certificação e pouso integral da F5. A pré-Opus consta no 16. `avais_depois_do_vPRONTO5.txt:29`, `app/docs/BACKLOG.md:2053`, `execucoes.txt:2,31` |
| 75. Smoke de leitura do app | ETAPA FUTURA | GET com badge e pendentes nas duas cascas, por cinco minutos, com registro de traceback. `avais_depois_do_vPRONTO5.txt:29`, `app/api/urls.py:92`, `app/api/views.py:2536–2556` |
| 76. Smoke humano do modal | ETAPA FUTURA | Abertura do modal após pouso. A entrada marca notas como lidas. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/views.py:869–880`, `app/chamados/services/nota.py:126–133` |
| 77. Censo dos escritores da coluna | DEMONSTRADO | Recontados 9 escopos e 14 ocorrências, 7 atribuições, 5 `save` e 2 descarimbos em lote, com zero acusações pelo analisador. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:34–44,439–501,536–546`, `app/chamados/models.py:1582,1721,3372,3400,3412`, `app/chamados/services/validacao.p

[truncado em 20000 de 59464 caracteres]

## 21/11 05:00 · sessao ca360470c6b3 · E4C-F5-CONSTROI · censo · origem=Code

| Achado | Classe | Trecho |
|---|---|---|
| **1. Bateria completa de mutantes no hash final** (`opus.r:3–9`) | **(c) Execução** | **Pergunta:** “Pela bateria anexa: existe outra entrada da tabela que so morde num modo e fica verde no outro?” (`volta4_verif.q:5`). **Resposta:** “Na bateria anterior, os 11 mutantes que ficaram na tabela retornaram `rc=1` nos dois modos.” e “A execução Django no hash atual [nao verificado].” (`volta4_verif.r:9,5`). A sessão registrou “155 mutantes = MORDE 145, NAO MORDE 10” em `c8eb8eddf` e prometeu “A bateria RODA DE NOVO no hash final”. A publicação de `9e90d9253` registra “11 entradas mordem nos DOIS modos”. A rerrodagem dos 155 no hash final e um `PAREI LISTA` final com os dez ficam **[nao verificado]**. (`linhas_triagem_relato.txt:66`; `app/docs/RELATO.md:12,62`; `opus.r:5–8`) |
| **2. PRONTO aberto na abertura do opus** (`opus.r:10`) | **(e) Não perguntado** | Ao 2º leitor foi pedido um “delta SO DE TESTE”. Sua resposta delimita a conferência a “quatro `.py`, todos de `app/chamados/tests`”. Faltou pedir o confronto item a item do PRONTO com as provas e os avais no hash integrado, incluindo quais pendências exigiam `PAREI` antes do opus. Essa conferência entrou no brief final como “Algum item do PRONTO esta marcado fechado sem a prova que o aval exige?”. A sessão já havia publicado “NAO FECHA” e mantinha “INCOMPLETO (nada disto toca .py): os FALTA acima”. O `PAREI CODIGO` anterior aparece como respondido para os itens 78 e 06. (`volta3_verif.q:1,8–10`; `volta3_verif.r:44`; `volta4_verif.q:1–5`; `brief_opus.md:34`; `linhas_triagem_relato.txt:4,96,100`; `app/docs/RELATO.md:94,96`) |
| **3. Limites da seção 7 mantidos em fila** (`opus.r:11–17`) | **(b) Visto e deixado fora da rodada** | **3(a) Par de alvo e valor.** Pergunta: “E quais formas vazias reais escapam?” Resposta: “O selo de atos ainda aceita presença da coluna como evidência”, com `alvo_lido.exclude(pk__in=ids)` entre as variantes verdes. Triagem: “Restos descartados (append/extend/**kwargs, filtro por dado) vao anexos ao opus.” A sessão registrou zero sítios de produção acusados. (`volta3_verif.q:4`; `volta3_verif.r:16,48`; `linhas_triagem_relato.txt:21–22`)<br>**3(b) Projeção de chamadas.** Pergunta: “o salto por auxiliar (quantos saltos, mesmo arquivo ou nao)”. Resposta: “Continua de um salto no mesmo arquivo” e “Os gravadores alcançados por essas chamadas entram diretamente no censo.” A triagem aplicou o corte geral da “volta 4 estreita”: “nada da secao 7 do verificador entra, vai anexo ao opus”. Motivo específico na triagem **[nao verificado]**. O laudo final cita o scanner com “0 fora das permitidas”. (`verif_delta.q:6`; `volta3_verif.q:9`; `volta3_verif.r:49`; `linhas_triagem_relato.txt:21`; `app/docs/RELATO.md:13`; `opus.r:13`)<br>**3(c) Catraca parcial.** Pergunta: “Que formas de update_fields escapam do visitante (lista montada fora, soma de listas, variavel, tupla, kwargs)?” Resposta: “Continuam escapando `append`/`extend`, `**kwargs`”, com confirmação no `save` da RECUSA. Triagem: “sem ampliar a lista de funcoes, o resto descartado e anexado ao opus”, pelo escopo literal do aval 1945. (`verif_delta.q:14`; `volta3_verif.q:9`; `volta3_verif.r:50`; `linhas_triagem_relato.txt:22,25`)<br>**3(d) Relógio nos asserts ORM.** Pergunta: “Algum dos tres arquivos le relogio, banco, env ou arquivo fora da arvore de um jeito que faca o veredito variar entre rodadas?” Resposta: “Se a reabertura sair anterior à validação, a validação pode continuar vigente.” Triagem: “pre-existente, fica na fila sem cura nesta volta”, reiterado em “(f) relogio :756/:774 fica (fila)”. (`verif_delta.q:17`; `volta3_verif.q:9`; `volta3_verif.r:51`; `linhas_triagem_relato.txt:22,25`) |
| **4. Comentários `Transitorio (B/C)`** (`opus.r:18`) | **(e) Não perguntado** | A pergunta mais próxima pede “Para cada um campo/valor que o delta le (arquivo:linha), quem escreve e quem le em producao.” A resposta mapeia “`validada_em` e `respondida_em` vigentes” e cita os juízes. Faltou perguntar se os comentários sobre chamadores ORM ainda descreviam o fluxo depois da volta C. A cópia contém literalmente “Transitorio (B/C): chamador que ainda entrega ORM le as colunas da pergunta.” nos dois sítios. (`volta3_verif.q:10`; `volta3_verif.r:57`; `volta4_verif.q:1–5`; `app/chamados/juizes.py:1390,1559`) |
| **5. Escopo declarado como não verificado pelo leitor final** (`opus.r:19`) | **(e) Não perguntado** | O 2º leitor recebeu “Para cada um campo/valor que o delta le [...] quem escreve e quem le em producao” e respondeu, por exemplo, “Para `devolucao`, escrevem as duas portas de devolução.” Faltou pedir o censo completo de escritores e leitores do **diff de produção** em `materializacao.py` e `validacao.py`, o vínculo de cada sítio com a coluna `PROTEGE` e a comparação de `JUIZES_POR_VARREDURA` com a base. O brief final pede “QUEM ESCREVE e QUEM LE” nos dois arquivos. O laudo registra que só confirmou a L-130 e considerou o cron inaplicável. A comparação do cron com a base fica **[nao verificado]**. (`volta3_verif.q:10`; `volta3_verif.r:58`; `verif_delta.q:4–19`; `volta4_verif.q:1–5`; `brief_opus.md:29–31`; `opus.r:19`; `app/docs/LEIS.md:151`; `app/config/crons.py:2061`) |

No **item 3**, a volta 4 teve “0 achado”. O `PAREI SITIO` já constava da triagem como “EXCLUSAO antes da porta, nunca o False do par”, e a sessão publicou a justificativa pelos dois seletores com `validada_em__isnull=True`. O par literal de pergunta e resposta do 2º leitor sobre esses sítios fica **[nao verificado]**. O laudo também registra a falta de entradas nominais no BACKLOG para **3(a)** e **3(d)**. (`linhas_triagem_relato.txt:18,134`; `app/docs/RELATO.md:92,116`; `app/chamados/reconciliador.py:681,1101`; `opus.r:12,15–17`)

**Contagem por item numerado:** (a) **0** · (b) **1** · (c) **1** · (d) **0** · (e) **3**. (`opus.r:3,10–11,18–19`)

**Identidade da cópia:** hash `b0935c1f106d232dc39b263676a588d66eddff54` indicado no brief e no relato raspado. Confirmação do `HEAD` local **[nao verificado]**. (`brief_opus.md:3–4`; `linhas_triagem_relato.txt:4`)

## 21/11 04:55 · sessao ed762561508b · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

## 1. Dono dos caminhos A → B

Na saída da sombra, **53 casos de colaborador por competência, de 47 PKs distintos, mudaram as linhas recompostas**. A é a foto com a falta aprovada. B é a foto após `decidir(..., "rejeitada")`, dentro do savepoint revertido. A sonda compara as linhas montadas a partir do `FechamentoMensal` gravado. O efeito sobre reexportação, importação ou pagamento está [nao verificado]. `fa_txt_sonda3.out:3`, `sonda_ab.py:172–188,286–308,311–333,352–370,381–390`, `app/folha/views.py:209–214,270,299–302`.

| Caminho com mudança | Crivo que decidiu | Dono pela L-099 e pelo E6 |
|---|---|---|
| `entra → entra`, **34 casos / 29 PKs** | Passa pelos crivos nas duas fotos. A montagem consulta `datas_falta` e, com a rubrica `falta_diurna`, emite um tipo 10 com a quantidade e um tipo 11 por dia. O delta é compatível com retirar ou reduzir esse bloco. `fa_txt_sonda3.out:3`, `app/folha/export.py:87–92,169–185,215–221,740–744,837–842,968–981`. | **Nenhum dos três responde por esse delta.** A comparação troca um insumo que o montador lê. A L-099 cobra coerência com os dados existentes e o E6 julga uma divergência de dia-colaborador. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,350–378`. |
| `entra → fora / furo_espelho`, **17 casos / 16 PKs** | Em B, `motivos_retencao_espelho` devolve motivo, o classificador encerra ali e o montador pula o fechamento inteiro. `fa_txt_sonda3.out:3`, `app/folha/export.py:716–727,968–973,1301–1307`. | **Nenhum dos três é atribuível pelo caminho observado.** `furo_espelho` é o resultado do crivo da folha. O dono de eventual divergência diária está [nao verificado]. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/folha/export.py:1264–1298`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,182–195`. |
| `fora / furo_espelho → entra`, **2 casos / 2 PKs** | A retenção aparece em A. Em B, o espelho passa, a homologação também passa e o montador inclui o fechamento. `fa_txt_sonda3.out:3`, `app/folha/export.py:716–739,837–842,968–981`. | **Nenhum dos três é atribuível pelo caminho observado.** O fim da retenção responde à pergunta de entrada no TXT. O dono de eventual divergência diária está [nao verificado]. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,182–195`. |

A ordem executada por `dono_da_divergencia` é **CADASTRO** para folga indefinida ou falta de vínculo, **BATIDA** para batida fora de turno ou contagem ímpar, **CADASTRO** para fase deslocada ou dia na lista `dias_cadastro_x_realidade`, e **ESTRUTURA** no residual de uma divergência diária. A sonda registra `status` e `motivo` do exportador e agrega o delta por tipo de linha. Os insumos diários do E6 estão [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110,162–195,198–212`, `sonda_ab.py:322–349,357–370`.

## 2. Por que `fora → entra`

**O crivo que deixou de reter foi `furo_espelho`**, nos PKs **161** e **504**. Cada um passou de zero para uma linha tipo 10. O retorno que retinha em A está em `export.py:968–973`, antes da homologação em `export.py:974–979`. `fa_txt_sonda3.out:3`, `app/folha/export.py:968–981`.

O `save` da rejeição aciona o cartório dos dias da ausência. O cartório consulta a cobertura para cobrança, que exclui rejeitadas, e pode relavrar o veredito, os protestos e a ata que o espelho lê. Um caminho previsto no código é um turno encerrado com fato em dia coberto por ausência aprovada. Isso pode gerar `FATO_EM_AUSENCIA`, que vira `fato_em_ausencia` e pode reter o fechamento. O dia, o protesto e o veredito que efetivamente deixaram de reter cada PK estão [nao verificado]. A sonda guardou o motivo geral e descartou `motivos_espelho`. `app/ponto/services/ausencia.py:975–988,1084–1088`, `app/ponto/signals.py:85–93,143–147`, `app/ponto/turnos.py:2523–2532`, `app/ponto/services/cartorio.py:1382–1385,1500–1511,1898–1922`, `app/ponto/supra_juiz.py:392–418`, `app/escala/models.py:1619,1767–1768`, `app/folha/export.py:968–973,1247–1298`, `sonda_ab.py:322–329`.

## 3. O “1 sem classe”

O **PK 676, empresa 4, 07/2026**, tinha falta candidata e nenhum `FechamentoMensal` na competência em A ou B. A sonda lista candidatos pela falta, consulta o fechamento depois e passa `[]` ao classificador. Sem item retornado, grava `status=null` e `motivo=null`. Com zero linhas nas duas fotos e zero exceções, o caso é **conclusivo e sem delta**. O “sem classe” é a ausência de veredito do exportador para esse caso. A razão de o fechamento estar ausente está [nao verificado]. `fa_txt_sonda3.out:3`, `sonda_ab.py:263–283,311–333,394–420`, `app/folha/export.py:716–727,782–788,980–981`.

## 4. Leis e cortes da competência exportada

- **L-092 e L-113.** A redação da L-092 diz “COMPETENCIA EXPORTADA NAO MUDA O GRAVADO” e manda a diferença para “Pauta DP com os dois numeros”. A L-113 diz “o sistema calcula o que tem” e “Se o export ficou diferente, refaz”. Ela revoga a recusa geral de recálculo, a guarda de regeneração **como trava** e a Pauta DP **como precondição**. Conserva a versão anterior do TXT intacta, a trilha e o DIFF de impacto publicado antes de ato de frota. No serviço, a recusa geral deu lugar a `WARNING` quando há escrita em escopo exportado. `app/docs/LEIS.md:125,155`, `app/ponto/services/fechamento.py:485–498,525–547`.
- **L-052 e REGEN-EM-EXPORTADA.** A redação antiga diz “DIA QUE VIROU FOLHA NAO SE TOCA, nem para corrigir” e está marcada como superada em parte. A consolidação de 31/10 diz que “o GRAVADO (`FechamentoMensal`) nao muda por caminho generico” e exige `!`, DIFF de frota publicado antes, reversão em `logs/`, prova depois e passagem pela REGEN-EM-EXPORTADA. Exige staff, trilha com quem, quando, motivo e hashes antes/depois, TXT parcial de retificação com hash publicado e Pauta DP com pago e devido. Na cópia, o regenerador ainda pula a janela lavrada salvo `apesar_da_lavra` com motivo. A porta de retificação recebe um colaborador por chamada e devolve o material antes/depois. O limite original de “UM colaborador por ato” foi revogado no corte de 30/09. `app/docs/LEIS.md:93,155`, `CLAUDE.md:401–417`, `app/ponto/portas/celula.py:213–221,284–288`, `app/ponto/portas/regen_exportada.py:26–27,107–119,174–185,204–225`, `app/docs/LAPIDES.md:45–48,654–658`.
- **TXT-E-FOTOGRAFIA e K6.** “Correcao provada entra em QUALQUER competencia, a qualquer momento.” Há um TXT vigente por empresa e competência. Gerar de novo substitui o vigente, as versões anteriores ficam guardadas com conteúdo e hash, e importação e pagamento são do Domínio. O K6 define entregue como “existe ExportacaoDominio vigente para empresa+competencia”. Para a guarda de geração de células, o outro corte inclui também holerite publicado. `app/docs/PROMPTS.md:286`, `CLAUDE.md:418–422`, `app/docs/CORTES.md:374–375`, `app/folha/export.py:580–594,597–628`, `app/folha/models.py:143–170,174–187`.
- **EXPORTADO-SEM-FRONTEIRA, FIO-EXPORTADA-X-O44 e L-124.** O primeiro corte diz “Dia exportado segue EDITAVEL, celula soberana, folha/TXT recalcula online”. O segundo retira “exportada nunca vai ao app, so leitura” e, para as perguntas vivas em disputa fechada daquele corte, estabelece “so Validar, sem reabrir, sem push”. A L-124 explicita “fato novo rejulga normal em qualquer competencia; correcao provada em exportada segue a porta de hoje”. Os cortes K6 de 15 e 16/11 sobre plantio de metade de intervalo têm esse escopo, e o de 16/11 exclui expressamente `AUSENCIA`. `app/docs/LEIS.md:91,145`, `app/docs/CORTES.md:318,426,461,520,530`.
- **Guarda específica de 09/2026.** O corte BO-B diz “09 nao recalcula por evento ate DIFF da 09 [...] publicado + reversao + Pauta DP” e “TXT novo da 09 so quando o DP pedir”. Na cópia, `GUARDA_COMPETENCIA` ainda exclui do recálculo em modo de escrita os colaboradores das empresas que entregaram 09/2026. A montagem que a sonda chamou lê o fechamento e as fontes do exportador, sem passar por esse recálculo. `app/docs/PROMPTS.md:1370`, `app/ponto/services/fechamento.py:19–43,539–547,558–575`, `sonda_ab.py:311–333`.

O aval desta obra determina “Nada se toca; dono L-099 depois do numero”. O **53** mede mudanças nas linhas recompostas da sombra. O número de alterações no arquivo entregue e o impacto financeiro estão [nao verificado]. A identidade Git da cópia também está [nao verificado]. `respostas_2011_1945.txt:5`, `app/docs/BACKLOG.md:2060`, `fa_txt_sonda3.out:3`, `sonda_ab.py:311–333,357–370`.

_7637 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
