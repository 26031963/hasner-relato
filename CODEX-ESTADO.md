# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 19:32** · publicado em **12/11/2026 19:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 19:32 · sessao 1d6600f7c1f6 · TRANCA-VIVAS-08 · verificador · origem=Code

1. **Classe: campo com significado impreciso — ressalva, sem bloqueante novo demonstrado.** `ficam_com_o_admin` agora é a união, sem duplicatas, de respostas destinadas ao DP, resgatadas vivas e vivas por toque do admin. Portanto pode aumentar mesmo sem resposta do colaborador. O CLI continua publicando esse agregado como `decidir_pelo_dp`, e a ferramenta da mensageria ainda descreve “resposta do colaborador sem veredito”. Essa descrição já era incompleta para as resgatadas; a obra amplia a diferença para as tocadas. (`app/chamados/services/competencia_trancada.py:343`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; `mensageria/nucleo/ferramentas.py:2031`; `app/chamados/tests/test_tranca_vivas_08.py:131`; `app/chamados/tests/test_c7_red_minimos.py:110`)

   Outra precisão necessária: `resgatado_em` não significa exclusivamente “resgatado pelo admin”. A porta também aceita resgate por resposta do colaborador e conserva o **primeiro** carimbo. O contador reutiliza esse discriminador existente; não verifica novamente quem realizou o resgate. (`app/chamados/services/arquivo.py:180`; `app/chamados/services/arquivo.py:190`; `app/chamados/services/materializacao.py:724`; `app/chamados/services/competencia_trancada.py:117`)

2. **Classe: censo de escritores e leitores de `veredito_celula`.** Os escritores produtivos encontrados são estes; nenhum deles produz ato humano sem `via` e sem `acoes`. (`app/ponto/portas/celula.py:1027`; `app/ponto/portas/celula.py:1042`; `app/ponto/services/veredito_celula.py:356`)

   | Papel | Escritor/leitor e significado |
   |---|---|
   | Escritor físico da auditoria | `evento` grava `acao`, usuário e `valor_depois`; `registrar_log` delega a ele. (`app/core/observ.py:134`; `app/core/auditoria.py:22`) |
   | Lavra da porta | Tanto atualização apenas de status quanto lavra completa gravam `valor_depois` com `via`, `veredito`, `protestos` e `status_dia`. (`app/ponto/portas/celula.py:1027`; `app/ponto/portas/celula.py:1042`) |
   | Despachante Resolver dia | Grava `veredito`, `acoes` e `erros`; também solicita lavra pela via `admin`. (`app/ponto/services/veredito_celula.py:352`; `app/ponto/services/veredito_celula.py:356`) |
   | Chamadores humanos | A view Resolver dia e declarar falta chegam ao mesmo despachante. (`app/ponto/views.py:3339`; `app/ponto/services/declarar_dia.py:213`) |
   | Chamadores de sistema | O cartório chama a porta com `via='cartorio'` nos três caminhos encontrados. (`app/ponto/services/cartorio.py:934`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1853`) |
   | Usuário herdado | Quando `ator` não vem preenchido, `evento` pode usar o usuário autenticado da request. Logo, usuário preenchido sozinho não demonstra ato humano. (`app/core/observ.py:108`; `app/core/observ.py:113`) |
   | Leitor do toque | Lê objeto, instante e JSON; aceita `via='admin'` **ou presença** de `acoes`. (`app/chamados/juizes.py:1295`; `app/chamados/juizes.py:1298`) |
   | Outro leitor específico | `decisao_do_dia` seleciona células atualmente com via `admin`, depois lê todas as linhas dessa ação para obter o último usuário/ator. **Não filtra a via do JSON do log.** (`app/ponto/services/veredito_celula.py:110`; `app/ponto/services/veredito_celula.py:115`; `app/ponto/services/veredito_celula.py:122`) |
   | Consumidores dessa decisão | Calendário e API do espelho. (`app/colaboradores/services/calendario.py:415`; `app/api/views.py:1410`) |
   | Leitores genéricos | Tela de auditoria, foto E4B, contagem de escritas dos crons e censo de portas; esses caminhos não aplicam o novo predicado humano sobre `via`/`acoes`. (`app/core/views.py:705`; `app/chamados/services/foto_e4b.py:322`; `app/chamados/services/vigia_loop.py:86`; `app/core/portas_da_tela.py:152`) |

   Nos testes, escrevem logs sintéticos os helpers E2 e SNC2 e o teste novo; `test_signal_ancora` fornece a tupla simulada. Também leem essas linhas os testes de porta, status lavrado e folga sem adesão. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1068`; `app/chamados/tests/test_snc2_toque_em_lote.py:59`; `app/chamados/tests/test_tranca_vivas_08.py:90`; `app/chamados/tests/test_tranca_vivas_08.py:105`; `app/escala/tests/test_signal_ancora.py:247`; `app/ponto/tests/test_porta_celula.py:90`; `app/ponto/tests/test_porta_celula.py:149`; `app/ponto/tests/test_e3_status_lavrado.py:257`; `app/ponto/tests/test_x1_folga_sem_adesao.py:305`)

3. **Classe: mudança de comportamento do juiz — alcance exato.** Comparado ao trecho anterior fornecido, o novo filtro remove do conjunto de toques as linhas cujo JSON não seja dicionário ou não contenha nenhuma das assinaturas aceitas. Isso inclui lavra `cartorio`, JSON vazio/nulo e qualquer outra via sem `acoes`; não é uma exclusão codificada apenas para a palavra `cartorio`. Atualmente a porta admite somente `cartorio` e `admin`. (`app/chamados/juizes.py:1298`; `app/escala/models.py:1769`)

   `dia_da_pergunta_trancado` muda de **False para True** quando havia tranca viva e o único fundamento para destrancar eram essas linhas agora rejeitadas: nenhum cobrar válido, nenhum Resolver dia aceito e nenhum desarquivar válido posterior permanece. `pergunta_viva` pode então mudar de **True para False**, porque a tranca é examinada antes da resposta e dos demais fundamentos de vida. O filtro não acrescenta fundamento novo para ressuscitar pergunta. (`app/chamados/juizes.py:1069`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1275`; `app/chamados/juizes.py:1302`; `app/chamados/juizes.py:1325`; `app/chamados/juizes.py:1401`; `app/chamados/juizes.py:1405`)

   Os caminhos humanos atuais encontrados permanecem reconhecidos pelas duas assinaturas. Histórico sem `via`, mas com `acoes`, também permanece reconhecido — inclusive `acoes=[]` ou `acoes=None`, pois se testa a existência da chave. Histórico sem ambas passa a ser ignorado, independentemente da natureza real do ato. Que todos os atos humanos antigos possuíam essas assinaturas: **[não verificado]**. (`app/ponto/services/veredito_celula.py:352`; `app/ponto/services/veredito_celula.py:358`; `app/chamados/juizes.py:1298`)

4. **Classe: autoridade única — escritores dos retornos e leitores da tranca.** Não há campo persistido novo nesses retornos: os produtores são as funções abaixo, com caches locais/de consulta já existentes. (`app/chamados/juizes.py:902`; `app/chamados/juizes.py:1149`; `app/chamados/juizes.py:1232`)

   | Retorno | Produtor e leitores produtivos diretos |
   |---|---|
   | `_toque_humano_nos_dias`: conjunto de índices | Produzido em `app/chamados/juizes.py:1262` e retornado em `app/chamados/juizes.py:1327`; consumido por `_admin_tocou_os_dias_depois` em `app/chamados/juizes.py:1229`. |
   | `_admin_tocou_os_dias_depois`: lista de booleanos | Produzida em `app/chamados/juizes.py:1210`; lida pelo avulso em `app/chamados/juizes.py:1195`, pelo lote da tranca em `app/chamados/juizes.py:1077` e pelo contador em `app/chamados/services/competencia_trancada.py:155`. |
   | `_admin_tocou_o_dia_depois`: booleano | Delega ao lote em `app/chamados/juizes.py:1195`; leitor em `app/chamados/juizes.py:1076`. |
   | `dias_trancados`: lista de booleanos | Produzida em `app/chamados/juizes.py:1023` e preenchida em `app/chamados/juizes.py:1079`; leitores: avulso `app/chamados/juizes.py:991`, consulta `app/chamados/juizes.py:1149` e tarefa do dia `app/chamados/services/tarefa_do_dia.py:422`. |
   | `dia_da_pergunta_trancado`: booleano | Lê consulta ou lote de um em `app/chamados/juizes.py:987` e `app/chamados/juizes.py:991`; leitores: C6 `app/chamados/juizes.py:1405`, respostas sem veredito `app/chamados/models.py:1847`, reconciliador `app/chamados/reconciliador.py:668` e `app/chamados/reconciliador.py:748`, emissão `app/chamados/services/disputa_emissao.py:1970`. |
   | `pergunta_viva`: booleano | Corpo em `app/chamados/juizes.py:1399`; fachada/memo em `app/chamados/juizes.py:862`. O modelo reexporta a autoridade e a property `viva` delega a ela. (`app/chamados/models.py:4084`; `app/chamados/models.py:3376`) |

   Os leitores de teste diretamente relacionados à tranca incluem E2, SNC2, Signal Âncora, E1-v9, pergunta somente ao C6, chamado do dia e o teste novo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1135`; `app/chamados/tests/test_e2_tarefa_do_dia.py:1194`; `app/chamados/tests/test_snc2_toque_em_lote.py:228`; `app/escala/tests/test_signal_ancora.py:267`; `app/chamados/tests/test_e1_v9.py:430`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:94`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`; `app/chamados/tests/test_tranca_vivas_08.py:73`)

5. **Classe: quem mais lê C6 — censo das chamadas produtivas diretas.** Além da fachada/property acima, encontrei estes consumidores; os aliases importados de `chamados.models` também chegam à mesma autoridade. (`app/chamados/models.py:4084`)

   | Arquivo | Linhas das chamadas |
   |---|---|
   | `app/chamados/juizes.py` | 1496, 1501, 1513, 1583, 1684, 2346 |
   | `app/chamados/models.py` | 368, 1510, 1897, 3045, 3140, 3376 |
   | `app/chamados/reconciliador.py` | 399, 662, 759, 1087, 1595 |
   | `app/chamados/views.py` | 1242, 1257, 1417 |
   | `app/chamados/relatorios_disputa.py` | 18 |
   | `app/chamados/services/fio.py` | 238, 606, 782, 961, 2129, 2134 |
   | `app/chamados/services/disputa_emissao.py` | 1821, 1966, 2048 |
   | `app/chamados/services/competencia_trancada.py` | 144, 167 |
   | `app/chamados/services/pergunta_no_app.py` | 58, 170 |
   | `app/chamados/services/vigia_fio.py` | 755, 813 |
   | `app/chamados/services/relavra_via.py` | 242 |
   | `app/chamados/services/acoes_chamado.py` | 355 |
   | `app/chamados/services/tarefa_do_dia.py` | 491 |
   | `app/chamados/management/commands/auditar_invariantes_chamados.py` | 202, 540, 858, 894, 1653, 1710, 1769, 1782 |
   | `app/chamados/management/commands/disparar_perguntas_competencia.py` | 109 |
   | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py` | 449 |
   | `app/chamados/management/commands/reabrir_orfas_enterradas.py` | 83 |
   | `app/api/views.py` | 2608 |
   | `app/colaboradores/services/calendario.py` | 476 |
   | `app/ponto/services/dia_do_colab.py` | 71 |

   Leitores diretos adicionais de teste, por módulo, foram encontrados nos seguintes sítios; a lista identifica módulos consumidores, não certifica sua execução. (`app/chamados/tests/test_c7_red_minimos.py:124`)

   - `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:56`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:367`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:239`; `app/ponto/tests/test_hx_cartorio_ressuscita.py:56`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:386`; `app/ponto/tests/test_o261_cobrado_le_o_fio.py:134`; `app/ponto/tests/test_cods_do_ato.py:124`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:80`.
   - `app/chamados/tests/test_e2_tarefa_do_dia.py:1135`; `app/chamados/tests/test_contract_pergunta_viva.py:56`; `app/chamados/tests/test_e4b_c4_orm.py:473`; `app/chamados/tests/test_orfa_sem_candidato.py:70`; `app/chamados/tests/test_selo_juizes_orfaos.py:73`; `app/chamados/tests/test_o261_lavra_le_a_lampada.py:138`; `app/chamados/tests/test_pergunta_no_app.py:85`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:186`; `app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:177`; `app/chamados/tests/test_e1_v9.py:171`.
   - `app/chamados/tests/test_limbo_nao_fecha_com_filho_no_defeito.py:85`; `app/chamados/tests/test_selo_i8b_resolvedora_cruzada.py:83`; `app/chamados/tests/test_e4a_relogio_injetado.py:34`; `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:98`; `app/chamados/tests/test_viva_delega_ao_juiz.py:42`; `app/chamados/tests/test_tranca_de_verdade.py:52`; `app/chamados/tests/test_reabrir_orfas_enterradas.py:103`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:95`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:75`; `app/chamados/tests/test_invariantes_disputa.py:145`.
   - `app/chamados/tests/test_c7_red_minimos.py:124`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:76`; `app/chamados/tests/test_e4b_c11_orm.py:145`; `app/chamados/tests/test_o261_vigia_fio.py:134`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:668`; `app/chamados/tests/test_e4b_c12_orm.py:61`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:106`; `app/chamados/tests/test_e4a2_nunca_bateu.py:158`; `app/chamados/tests/test_resposta_nao_e_fantasma.py:86`; `app/chamados/tests/test_bug117_marco_no_espelho.py:128`.
   - `app/chamados/tests/test_t34_furo_sob_limbo.py:55`; `app/chamados/tests/test_pergunta_viva.py:18`; `app/chamados/tests/test_competencia_trancada.py:127`; `app/chamados/tests/test_selo_i6_ouve_a_lei.py:114`; `app/chamados/tests/test_e4b_c6_orm.py:80`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150`; `app/chamados/tests/test_e1_v6_horizonte.py:538`; `app/chamados/tests/test_o261_pergunta_viva_teto.py:55`; `app/chamados/tests/test_selo_i8_fio_solto.py:95`; `app/chamados/tests/test_hx_tipo_pelo_marco.py:161`.
   - `app/chamados/tests/test_o261e_f2_fila.py:246`; `app/chamados/tests/test_porta_veredito_pergunta.py:427`; `app/chamados/tests/test_adesao_por_colab.py:61`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:104`; `app/chamados/tests/test_reconciliador_c1_escopo.py:48`; `app/chamados/tests/test_selo_celula_absolveu.py:72`; `app/chamados/tests/test_o261_resposta_vazia.py:113`; `app/chamados/tests/test_e1_entradas.py:152`; `app/chamados/tests/test_selo_memo_pergunta_viva.py:86`; `app/chamados/tests/test_o261_recusa_fala.py:105`.
   - `app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_snc2_toque_em_lote.py:90`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`; `app/chamados/tests/test_reabrir_pendente_pelo_juiz.py:67`; `app/chamados/tests/test_orfas_pelo_evento.py:102`; `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:151`; `app/chamados/tests/test_b2d_resposta_visivel.py:99`. O teste novo também chega ao C6 pelo helper herdado. (`app/chamados/tests/test_tranca_vivas_08.py:86`; `app/chamados/tests/test_c7_red_minimos.py:124`)

6. **Classe: contador versus universo — conforme no recorte examinado.** `medir()` não mede todas as perguntas globais: mede as da empresa e da janela do PF. Para perguntas de fato, esse recorte coincide com o do C20: mesmo `data_evento`, mesma empresa e janelas da mesma autoridade. As duas pontas da janela são inclusivas. Conversas saem pelo discriminador existente; o contador consulta efetivamente `pergunta_viva`, sem reproduzir seus degraus. (`app/chamados/services/competencia_trancada.py:99`; `app/chamados/services/competencia_trancada.py:119`; `app/chamados/services/competencia_trancada.py:141`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/juizes.py:334`; `app/chamados/juizes.py:1062`; `app/ponto/janelas.py:54`; `app/ponto/janelas.py:60`)

   A classificação é: conversa fora do universo de fatos; fato vivo resgatado em lista própria; demais fatos vivos separados pelo mesmo juiz do toque usado pelo C20. Portanto **residual não significa total de vivas**: significa vivas sem as exclusões declaradas. As exclusões não criam um segundo juiz de vitalidade. (`app/chamados/services/competencia_trancada.py:141`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:145`; `app/chamados/services/competencia_trancada.py:155`; `app/chamados/services/competencia_trancada.py:158`; `app/chamados/juizes.py:1077`)

   | Campo/lista | Escritor e leitores encontrados |
   |---|---|
   | Todas as listas | Inicializadas por `medir`. (`app/chamados/services/competencia_trancada.py:89`) |
   | `fora_conversa` | Acrescida em `medir`; lida nominalmente nos testes novos. Sem consumidor produtivo nominal adicional encontrado. (`app/chamados/services/competencia_trancada.py:142`; `app/chamados/tests/test_tranca_vivas_08.py:151`) |
   | `vivas_resgatadas` | Acrescida em `medir`; lida na união de `medir_frota` e na exclusão do teste de conversa. (`app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/tests/test_tranca_vivas_08.py:154`) |
   | `vivas_tocadas_pelo_admin` | Acrescida em `medir`; lida por `medir_frota` e testes novos. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/tests/test_tranca_vivas_08.py:137`; `app/chamados/tests/test_tranca_vivas_08.py:143`) |
   | `vivas_apesar_da_tranca` | Acrescida em `medir`; convertida em quantidade e somada por `medir_frota`; lida pelo CLI, API e testes novos/mutante. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:347`; `app/chamados/services/competencia_trancada.py:348`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1574`; `app/chamados/tests/test_tranca_vivas_08.py:129`; `app/chamados/tests/test_tranca_vivas_08.py:162`; `app/chamados/tests/test_tranca_vivas_08.py:193`) |
   | `perguntas_lidas_trancadas` | Acrescida quando o C6 devolve falso; lida pelos testes N1 e novo. O nome não demonstra que a tranca foi a causa específica da morte. (`app/chamados/services/competencia_trancada.py:151`; `app/chamados/services/competencia_trancada.py:152`; `app/chamados/tests/test_n1_perguntas_do_app.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:144`) |
   | `decidir_pelo_dp` | Acrescida antes da separação das conversas, preservando a seleção de escrita; consumida pela pauta, agregado da frota e comando de encerramento. (`app/chamados/services/competencia_trancada.py:137`; `app/chamados/services/competencia_trancada.py:139`; `app/chamados/services/competencia_trancada.py:232`; `app/chamados/services/competencia_trancada.py:246`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:38`) |

   Outros leitores do dicionário de `medir` são `encerrar` e `disparar_no_ato`; ambos continuam dirigidos pelas listas de ação e sua quantidade, sem transformar as novas exclusões em escrita. (`app/chamados/services/competencia_trancada.py:190`; `app/chamados/services/competencia_trancada.py:206`; `app/chamados/services/competencia_trancada.py:221`; `app/chamados/services/competencia_trancada.py:303`)

7. **Classe: semântica temporal — não encontrei mudança de unidade, fuso ou fronteira.** A comparação continua usando o instante de criação da **trilha**, estritamente posterior a `fechado_em`; igualdade não conta. Não se compara o dia civil da request nem `veredito_em`. Os campos são timestamps, e a configuração usa timezone habilitado em São Paulo. (`app/chamados/juizes.py:1260`; `app/chamados/juizes.py:1302`; `app/core/models.py:307`; `app/ponto/models.py:926`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`)

   O dia da pergunta continua sendo o dia do turno para fatos; conversa devolve `None`. A tarefa do dia documenta expressamente que sua tranca é a atual, não uma reconstrução da tranca no instante `agora`. A obra não introduz outra leitura de relógio nesse filtro. (`app/chamados/juizes.py:334`; `app/chamados/juizes.py:1291`; `app/chamados/services/tarefa_do_dia.py:359`)

8. **Classe: consumidor da mensageria/observabilidade.** O campo novo é escrito no JSON exclusivamente com o total recebido de `medir_frota`; o cliente HTTP transporta o JSON, a ferramenta acrescenta `cliente`, e o montador incorpora o bloco quando solicitado. Não há derivação paralela nesse caminho. (`app/api/views_mensageria.py:1563`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2035`; `mensageria/nucleo/ferramentas.py:2037`; `mensageria/nucleo/ferramentas.py:2059`; `mensageria/nucleo/ferramentas.py:419`)

   O placar antigo **não lê o residual novo

[truncado em 20000 de 25920 caracteres]

## 12/11 19:28 · sessao a0a9efc3b6f7 · - · - · origem=Ronald (tmux)

1. **Universo do contador coincide com o recorte do C20 para perguntas de fato.** `medir()` seleciona empresa e `data_evento` entre início/fim inclusivos; elimina conversa pelo discriminador `_dia_de_turno_da_pergunta` e pergunta a `pergunta_viva` para cada fato. O C20 usa o mesmo dia, empresa e janela de competência. Não encontrei regra paralela de competência introduzida. (`app/chamados/services/competencia_trancada.py:99`, `:119`, `:131`, `:141`, `:144`; `app/chamados/juizes.py:334`, `:991`, `:1062`; `app/ponto/janelas.py:54`, `:60`)

2. **Exclusões reutilizam fontes existentes.** Conversa: `_dia_de_turno_da_pergunta`; toque: `_admin_tocou_os_dias_depois`, também usado pelo C20; resgatada: mantém o discriminador armazenado `resgatado_em > fechado_em`, aplicado aos chamados vivos cuja competência o C14 reconhece. O primeiro carimbo de resgate é escrito por `arquivo.levantar`; tanto desarquivar humano quanto resposta do colaborador podem solicitar esse carimbo — “resgatada pelo admin” é descrição mais estreita que o campo existente. (`app/chamados/services/competencia_trancada.py:101`, `:105`, `:117`, `:132`, `:141`, `:155`; `app/chamados/juizes.py:1076`; `app/chamados/services/arquivo.py:180`, `:190`, `:205`; `app/chamados/views_arquivar.py:77`; `app/chamados/services/materializacao.py:724`)

3. **Censo dos escritores/leitores dos campos novos e alterados.** `medir` inicializa todas as listas; escreve `fora_conversa`, `vivas_resgatadas`, `perguntas_lidas_trancadas`, `vivas_tocadas_pelo_admin` e `vivas_apesar_da_tranca`. `medir_frota` lê as três listas de vivas, calcula `ficam_com_o_admin` por união de IDs, calcula o residual por tamanho e soma por PF com `reaberto_em=NULL`. `fora_conversa` não tem consumidor produtivo nominal adicional encontrado; `perguntas_lidas_trancadas` é verificada pelo teste N1 e pelos testes novos. (`app/chamados/services/competencia_trancada.py:89`, `:142`, `:149`, `:152`, `:158`, `:341`, `:343`, `:347`, `:348`; `app/chamados/tests/test_n1_perguntas_do_app.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:129`, `:137`, `:144`, `:151`, `:154`, `:162`, `:190`)

4. **Outros consumidores de `medir`.** `encerrar` recebe todo o dicionário, mas só usa listas de escrita e `decidir_pelo_dp`; `disparar_no_ato` usa tranca e quantidade de ações. O comando de encerramento imprime listas de ação/exclusões antigas, sem ler nominalmente as novas listas. Não encontrei outro escritor produtivo das novas listas. (`app/chamados/services/competencia_trancada.py:190`, `:192`, `:206`, `:221`, `:232`, `:246`, `:303`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`, `:36`)

5. **Consumidor mensageria: transporte integral, sem derivação nova.** API lê `medir_frota` e publica o novo residual; `core_client` obtém o JSON, `ferramentas.chamados_em_competencia_trancada` acrescenta `cliente` e devolve o dicionário integral; `competencia_trancada_da_pergunta` o inclui quando a pergunta pede competência trancada; o montador de contexto incorpora esse bloco. Portanto o campo novo chega a esse consumidor sem filtro próprio. (`app/api/views_mensageria.py:1562`, `:1574`; `app/api/urls.py:36`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2035`, `:2037`, `:2051`, `:2059`, `:419`)

6. **Classe: campo/rótulo com significado impreciso — dívida anterior ampliada, não bloqueante novo isolado.** O agregado `ficam_com_o_admin` passa a incluir também perguntas vivas por toque, mesmo sem resposta, e elimina repetição de IDs. A ferramenta ainda o descreve como “resposta do colaborador sem veredito”; o CLI imprime esse agregado sob `decidir_pelo_dp`. Contudo o diff anexado demonstra que ambos já recebiam resgatadas vivas antes, portanto a imprecisão não nasceu nesta obra. O caso novo sem resposta está explícito nas fixtures. (`app/chamados/services/competencia_trancada.py:335`, `:343`; `mensageria/nucleo/ferramentas.py:2031`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; `app/chamados/tests/test_tranca_vivas_08.py:131`; `app/chamados/tests/test_c7_red_minimos.py:110`)

7. **Classe: observabilidade — placar antigo não acompanha o residual.** CLI imprime residual próprio, mas `bin/placar_code.sh` extrai apenas `chamados_em_competencia_trancada`, a soma antiga de chamados/perguntas/disputas. Assim o placar pode mostrar zero mesmo com residual positivo; a API e a ferramenta mensageria recebem o residual separadamente. Não encontrei no aval fornecido exigência explícita de migrar esse placar, portanto não elevaria a bloqueante só por isso. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`, `:25`; `bin/placar_code.sh:287`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/ferramentas.py:2037`)

8. **Testes antigos de resgate não dependem da semântica antiga do residual.** Defendem que desarquivar devolve chamado/pergunta e que a passada seguinte respeita o admin; não leem `vivas_apesar_da_tranca`. O teste do placar continua cobrando só o agregado antigo. O teste novo cobre residual positivo, toque excluído e conversa separada, mas falta caso novo nominal para `vivas_resgatadas`/união sem duplicata; execução e GREEN permanecem [não verificado]. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:201`, `:219`; `app/chamados/tests/test_tranca_vivas_08.py:123`, `:131`, `:146`, `:158`)

**Neste subescopo, nenhum bloqueante novo comprovado; aprovação final depende dos demais eixos da revisão e da evidência de testes [não verificado].** [nao verificado]

## 12/11 19:27 · sessao 6c1e9266396f · - · - · origem=Ronald (tmux)

1. **Escritores atuais: nenhum ato humano de produção encontrado sem `via` e sem `acoes`.** A porta escreve as duas variantes do log com `via`: atualização só de status (`app/ponto/portas/celula.py:1027`) e lavra completa (`app/ponto/portas/celula.py:1042`). O despachante escreve `acoes` e `erros` (`app/ponto/services/veredito_celula.py:356`). Ambos chegam ao escritor físico único `evento`, diretamente ou por `registrar_log` (`app/core/auditoria.py:22`; `app/core/observ.py:134`). O método do modelo apenas delega (`app/escala/models.py:1826`).

2. **Chamadores de produção da porta:** despachante humano com `via='admin'` (`app/ponto/services/veredito_celula.py:352`); cartório com `via='cartorio'` em completamento de status, ramo nunca bateu e julgamento normal (`app/ponto/services/cartorio.py:934`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1853`). `ator=None` pode herdar o usuário autenticado da request, explicando por que presença de usuário não distingue o ato (`app/core/observ.py:108`; `app/core/observ.py:113`; `app/core/observ.py:118`). O middleware instala/resetta esse contexto (`app/core/middleware.py:76`).

3. **Ato humano atual preservado.** A view Resolver dia chama o despachante (`app/ponto/views.py:3339`); declarar falta chama o mesmo despachante (`app/ponto/services/declarar_dia.py:213`). As assinaturas desses caminhos continuam aceitas pelo novo filtro (`app/chamados/juizes.py:1298`). Declarar folga escreve folga e fecha o fio, sem produzir esta ação diretamente (`app/ponto/services/declarar_dia.py:191`; `app/ponto/services/declarar_dia.py:205`).

4. **Histórico: aceitação e limite preciso.** Log antigo sem `via`, mas com chave `acoes`, continua contando; `acoes=[]` ou `acoes=None` também contam porque o filtro testa presença da chave. Log nulo, não dicionário ou dicionário sem `via='admin'` e sem `acoes` passa a ser ignorado (`app/chamados/juizes.py:1298`). Que **todos os atos humanos históricos** traziam uma dessas assinaturas é **[não verificado]**: o escritor presente demonstra o formato atual, não versões anteriores (`app/ponto/services/veredito_celula.py:356`). O teste de linha sem assinatura codifica rejeição; não comprova a natureza do acervo (`app/chamados/tests/test_tranca_vivas_08.py:88`).

5. **Leitores específicos da ação.**
   - Juiz do toque: lê objeto, instante e JSON; admite assinatura humana e exige instante estritamente posterior ao fechamento (`app/chamados/juizes.py:1295`; `app/chamados/juizes.py:1302`).
   - `decisao_do_dia`: filtra a **via atual da célula**, não a via do JSON do log (`app/ponto/services/veredito_celula.py:110`). Em seguida lê todas as linhas dessa ação para obter o último usuário/ator (`app/ponto/services/veredito_celula.py:115`). Portanto a frase do brief “já filtra via admin” precisa dessa precisão. A pergunta dele é quais dias o admin decidiu, e não se estão destrancados (`app/ponto/services/veredito_celula.py:85`).
   - Consumidores de `decisao_do_dia`: calendário (`app/colaboradores/services/calendario.py:415`) e espelho API (`app/api/views.py:1410`).

6. **Leitores genéricos capazes de incluir essas linhas:** tela de auditoria, com seleção livre de ação (`app/core/views.py:705`; `app/core/views.py:709`), cuja tabela exibe metadados/descrição e não interpreta `via`/`acoes` (`app/templates/core/log_auditoria.html:56`; `app/templates/core/log_auditoria.html:73`); foto E4B, por modelo/objeto, preservando auditoria (`app/chamados/services/foto_e4b.py:311`; `app/chamados/services/foto_e4b.py:322`); contador de escritas por cron, indiferente à ação/JSON (`app/chamados/services/vigia_loop.py:86`). O censo de portas também pode contar ações detectadas como exclusivas, sem interpretar esse JSON (`app/core/portas_da_tela.py:149`; `app/core/portas_da_tela.py:152`).

7. **Escritores/leitores de teste além da produção:** helpers adaptados escrevem logs sintéticos (`app/chamados/tests/test_e2_tarefa_do_dia.py:1068`; `app/chamados/tests/test_snc2_toque_em_lote.py:59`); teste novo cria linha sem assinatura e chama `registrar_log` com `acoes` (`app/chamados/tests/test_tranca_vivas_08.py:90`; `app/chamados/tests/test_tranca_vivas_08.py:105`). O novo teste também lê/ajusta timestamps e confere `via`/usuário (`app/chamados/tests/test_tranca_vivas_08.py:43`; `app/chamados/tests/test_tranca_vivas_08.py:66`). Leitores de contagem de lavras existentes: porta (`app/ponto/tests/test_porta_celula.py:90`; `app/ponto/tests/test_porta_celula.py:149`), status lavrado (`app/ponto/tests/test_e3_status_lavrado.py:257`) e folga sem adesão (`app/ponto/tests/test_x1_folga_sem_adesao.py:305`). `test_signal_ancora` alimenta tuplas simuladas desse log (`app/escala/tests/test_signal_ancora.py:244`).

8. **Adaptações preservam as assertivas observadas.** E2 continua exigindo ação `perguntar/sistema`, agora com JSON equivalente ao escritor real (`app/chamados/tests/test_e2_tarefa_do_dia.py:1109`; `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`). SNC2 conserva a matriz esperada de vivos/mortos, incluindo outro colaborador e data antes da tranca (`app/chamados/tests/test_snc2_toque_em_lote.py:126`; `app/chamados/tests/test_snc2_toque_em_lote.py:136`). Signal âncora conserva `{0}` para dois itens com `desde` diferentes e os testes que matam `>`→`>=` (`app/escala/tests/test_signal_ancora.py:258`; `app/escala/tests/test_signal_ancora.py:269`). Não encontrei afrouxamento dessas asserções nesses trechos.

9. **Sem defeito corrente comprovado no filtro desta subtarefa.** A ação possui dois formatos documentáveis, ambos preservados conforme seu escritor; o filtro passa a distinguir suas assinaturas (`app/ponto/portas/celula.py:1044`; `app/ponto/services/veredito_celula.py:358`; `app/chamados/juizes.py:1298`). A completude histórica permanece **[não verificado]**. Execução de testes e produção também **[não verificado]**.

## 12/11 19:27 · sessao 2fd4fb45de82 · - · - · origem=Ronald (tmux)

1. **Adaptações preservam as assertivas anteriores.** Em `test_e2_tarefa_do_dia`, a fixture passa a reproduzir o payload real com `acoes`; continuam as verificações de tranca antes do toque e tarefa após Resolver dia, por colaborador (`app/chamados/tests/test_e2_tarefa_do_dia.py:1099-1111`; escritor real: `app/ponto/services/veredito_celula.py:356-358`). Em `test_snc2_toque_em_lote`, permanece o mapa exato de nove respostas e a igualdade lote/avulso; ambos os logs, inclusive o de outro colaborador, recebem a assinatura (`app/chamados/tests/test_snc2_toque_em_lote.py:118-140`). Em `test_signal_ancora`, permanece `{0}` para dois itens com instantes diferentes, as exclusões de usuário e o SQL estritamente `>`; os mutantes `>=` continuam nas três etapas (`app/escala/tests/test_signal_ancora.py:237-278`).

2. **O teste da lavra/cartório tem mordida efetiva no código descrito.** A fixture chama a porta real sob `REQUISICAO_DA_TRILHA` humana e confere usuário herdado e `via='cartorio'`; depois exige dia trancado e pergunta morta (`app/chamados/tests/test_tranca_vivas_08.py:56-86`). O mutante remove precisamente o filtro no fonte real e exige que a mesma fixture passe a destrancar (`app/chamados/tests/test_tranca_vivas_08.py:169-186`). Há contraexemplos separados para lavra admin, `registrar_log` com `acoes` e ato anterior à tranca (`app/chamados/tests/test_tranca_vivas_08.py:95-118`).

3. **O teste do contador é artificial, mas testa a ligação ao juiz e o residual.** Ele substitui exclusivamente `dia_da_pergunta_trancado`, confere que `pergunta_viva` ficou verdadeira e chama `medir` real, exigindo o ID no residual; a frota também é chamada realmente (`app/chamados/tests/test_tranca_vivas_08.py:123-129`; `app/chamados/tests/test_tranca_vivas_08.py:158-163`). A mordida transforma o destino do ID no corpo real de `medir` e demonstra residual vazio no mutante (`app/chamados/tests/test_tranca_vivas_08.py:188-195`). **Limite:** o mutante não restaura literalmente o antigo ramo resgatado, apesar do nome; força toda viva à exclusão admin (`app/chamados/tests/test_tranca_vivas_08.py:190-191`).

4. **Cobertura incompleta declarada:** o arquivo novo verifica conversa, toque admin, cartório e soma da frota, mas não contém caso de pergunta resgatada em `vivas_resgatadas` (`app/chamados/tests/test_tranca_vivas_08.py:121-163`). Cobertura dessa exclusão em outros testes: [não verificado].

5. **Relógio e travas:** os testes novos congelam o tempo e deslocam explicitamente a trilha relativamente a `fechado_em`; não introduzem leitura nova de relógio (`app/chamados/tests/test_tranca_vivas_08.py:30-49`). O trecho alterado do juiz continua sendo consulta de logs e classificação em memória, sem aquisição de trava (`app/chamados/juizes.py:1291-1303`); o trecho alterado de `medir` apenas consulta autoridades e preenche listas (`app/chamados/services/competencia_trancada.py:131-158`). Ordem global do serviço Resolver dia permanece colaborador primeiro, antes dos demais atos (`app/ponto/services/veredito_celula.py:224-227`).

6. **Prova executada:** RED anterior à cura, GREEN, suite inteira, cronologia dos testes e prova de frota pós-cura: [não verificado]. A declaração “escritos antes da cura” aparece apenas no docstring (`app/chamados/tests/test_tranca_vivas_08.py:10`).

**Conclusão deste recorte:** nenhum bloqueante próprio demonstrado nas três adaptações ou na mordida dos testes; execução e resultado permanecem [não verificado]. [nao verificado]

## 12/11 19:26 · sessao ecf65b15b5ad · TIPO-NA-FONTE · verificador · origem=Code

1. **BLOQUEANTE — regressão de comportamento / fuso no plantio.** O novo `_plantio_preenche_furo_na_data` passa `instante_marco` e `instante_luz` ao propositor, mas esses valores vêm sem fuso: `montar_lampadas` produz ISO local *naive*, e `periodos_do_dia` o reconstrói assim. O propositor rejeita esse valor com `ValueError('marca exige fuso')`; o materializador captura a exceção e devolve `False`. (`app/chamados/services/materializacao.py:182`; `app/chamados/services/materializacao.py:189`; `app/ponto/management/commands/gerar_celulas.py:69`; `app/ponto/juiz_batida.py:237`; `app/ponto/propositor.py:25`; `app/chamados/services/materializacao.py:194`)

   Contraexemplo: entrada acesa às 08:03, saída 17:00 apagada, `orfas=[]`, plantio às 17:04. A atribuição anterior com HH:MM aceita; o novo caminho encontra a exceção acima. Isso atinge contagem **conhecida e zero**, independentemente de legado desconhecido. O teste existente exige precisamente a aceitação desse plantio. (`app/chamados/tests/test_hx_plantio_celula.py:30`; `app/chamados/tests/test_hx_plantio_celula.py:34`; `app/chamados/tests/test_hx_plantio_celula.py:35`; `app/ponto/propositor.py:269`)

2. **BLOQUEANTE — migração incompleta dos testes existentes.** Permaneceram cinco assertivas incompatíveis com os produtores novos, que entregam `datetime` na grade e ISO na ata. (`app/escala/utils.py:622`; `app/escala/utils.py:663`; `app/escala/services/ata.py:150`)

   | Expectativa antiga remanescente | Sítio |
   |---|---|
   | `'13:41'` dentro das órfãs da grade | `app/escala/tests/test_ata_x_turno_entrada.py:142` |
   | Grade igual a `['15:00']` | `app/escala/tests/test_saida_valida_cross_meia_noite.py:102` |
   | Grade igual a `['14:03']`, em arquivo parcialmente adaptado | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:135` |
   | Ata igual a `['03:10', '22:50']` | `app/ponto/tests/test_bateria_lavratura.py:637` |
   | Ata igual a `['08:00', '17:00']` | `app/ponto/tests/test_bateria_lavratura.py:1057` |

   Há também fixture sem a chave `orfas` chamando `ata_do_dia`; a nova leitura obrigatória `dia['orfas']` produz `KeyError`, onde antes o diff fornecido usava `.get(... ) or []`. (`app/chamados/tests/test_emis_ausencia_parcial_cobra.py:60`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:68`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:72`; `app/ponto/services/cartorio.py:769`; versão-base local [nao verificado])

3. **BLOQUEANTE — selo existente perdeu mordida.** A bateria continua comparando `lampada['luz']`, em HH:MM, com `ata['orfas']`, agora em ISO. A interseção fica vazia mesmo se a mesma batida estiver simultaneamente acesa e órfã; esse teste passa sem defender sua invariante. (`app/ponto/tests/test_bateria_lavratura.py:959`; `app/ponto/tests/test_bateria_lavratura.py:960`; `app/escala/services/ata.py:150`)

   Os testes semelhantes foram adaptados em outros arquivos, mas essa bateria ficou de fora. (`app/ponto/tests/test_o118_orfa_que_acendeu.py:167`; `app/ponto/tests/test_lampada_acende_pela_batida.py:84`)

4. **BLOQUEANTE — catraca não intercepta escritas comuns.** `AtaVigiada` intercepta atribuição individual, mas herda `dict.update`, `dict.__ior__` e `dict.__delitem__`. Depois de uma leitura legítima pelo acessor, `a.update(orfas=...)`, `a |= {'orfas': ...}` e `del a['orfas']` deixam `acessos_crus_ata=0` e satisfazem `exigir_catraca`. Isso permite um segundo escritor passar pelo selo. (`app/escala/services/ata.py:46`; `app/escala/services/ata.py:77`; `app/escala/services/ata.py:114`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:17`)

   A fiscalização também depende da injeção explícita de `AtaVigiada`; os dirigidos não tornam automaticamente vigiada toda `CelulaDia.ata` usada pela suite Django. Os mutantes existentes cobrem get, subscript, cópia, dict, pop e setdefault, mas deixam essas escritas herdadas de fora. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:25`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:43`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:55`; `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`)

5. **BLOQUEANTE — leitor fora da porta / `None` tratado como zero / derivação paralela.** `cartorio.insumos_da_ata` continua lendo `n_orfas` diretamente com `or 0`. Sem lâmpadas, tanto `{'orfas': ['08:00']}` quanto `{'orfas': None}` devolvem `(False, False)`, embora o acessor responda, respectivamente, quantidade `1` e `None`. O próprio comentário do ramo distingue ausência de batida de órfã sem papel atribuído. (`app/ponto/services/cartorio.py:831`; `app/ponto/services/cartorio.py:835`; `app/ponto/services/cartorio.py:837`; `app/escala/services/ata.py:25`; `app/escala/services/ata.py:32`)

   Outro leitor fora do diff ainda deriva quantidade usando `len(dia.get('orfas') or [])`: o cálculo de `n_bat` no cartório. Hoje ele recebe a grade construída naquele julgamento, mas conserva uma segunda regra de contagem que transforma `None` em zero. (`app/ponto/services/cartorio.py:1414`; `app/ponto/services/cartorio.py:1422`; `app/ponto/services/cartorio.py:1424`)

   No archive há ainda leitura crua de `ata.orfas` por truthiness no script de snapshot. (`bin/snapshot_o118_col221.py:32`)

6. **BLOQUEANTE — ESMERIL-SEMANTICA: dia do turno versus dia civil no vigia.** As órfãs encontradas pelo match são guardadas no dia do turno; apenas as sobras ainda não consumidas passam pelo agrupamento civil. O vigia continua buscando a órfã exclusivamente na célula do dia civil da borda. (`app/escala/utils.py:1706`; `app/escala/utils.py:1763`; `app/escala/utils.py:1816`; `app/escala/utils.py:1825`; `app/escala/utils.py:1839`; `app/ponto/services/vigia_pareador_celula.py:114`)

   Contraexemplo: ata D contém a órfã de D+1 às 06:29 e a última lâmpada apagada; ata D+1 contém `orfas=[]`; o turno fechado de D termina naquele instante. O vigia consulta a lista vazia de D+1, perde o caso e pode devolver `cego=0`, pois seu universo continua preenchido. Ocorrência dessa configuração em produção: **[nao verificado]**. (`app/ponto/services/vigia_pareador_celula.py:102`; `app/ponto/services/vigia_pareador_celula.py:114`; `app/ponto/services/vigia_pareador_celula.py:118`; `app/ponto/services/vigia_pareador_celula.py:120`)

   O PDF tem risco equivalente: marca pela ata da linha apresentada, enquanto uma batida não reclamada por turno pode aparecer no dia civil. A integração desse contraexemplo no PDF permanece **[nao verificado]**; o calendário passou a procurar na união dos instantes. (`app/ponto/turnos.py:2962`; `app/ponto/services/espelho.py:575`; `app/relatorios/pdf_espelho.py:644`; `app/colaboradores/services/calendario.py:623`)

7. **BLOQUEANTE — `nao_medidos` desaparece em consumidores operacionais.** O esmeril publica `nao_medidos` na frota, mas a API da frota seleciona somente quatro campos e o descarta; o command também imprime contadores sem publicar a parte não medida. Assim, esses leitores não permitem distinguir “nenhuma ocorrência medida” de cobertura incompleta de A5. (`app/ponto/services/esmeril_espelho.py:235`; `app/ponto/services/esmeril_espelho.py:245`; `app/api/views_mensageria.py:1790`; `app/ponto/management/commands/esmeril_espelho.py:21`; `app/ponto/management/commands/esmeril_espelho.py:23`)

   A apuração de furos igualmente acrescenta `nao_medidos` ao retorno, mas seu command continua imprimindo apenas empresa, janela, colaboradores e dias. (`app/escala/services/furos_diarios.py:150`; `app/escala/management/commands/apurar_furos_diarios.py:13`; `app/escala/management/commands/apurar_furos_diarios.py:14`)

8. **Censo — escritores, leitores e contratos localizados.** A contagem e a identidade temporal são contratos diferentes: lista HH:MM conserva quantidade conhecida, mas não produz instantes; lista ausente/inválida/incompleta produz `None` na leitura temporal. A quantidade também fica `None` quando inválida ou inconsistente. (`app/escala/services/ata.py:20`; `app/escala/services/ata.py:27`; `app/escala/services/ata.py:29`; `app/escala/services/ata.py:119`; `app/escala/services/ata.py:130`)

   | Campo/API | Escritores e leitores de produção/bin localizados |
   |---|---|
   | JSON `ata.orfas` | Leitor no acessor: `app/escala/services/ata.py:17`; escritor: `app/escala/services/ata.py:151`; leitor cru externo: `bin/snapshot_o118_col221.py:32`. |
   | JSON `ata.n_orfas` | Escritor: `app/escala/services/ata.py:152`; leitores: `app/escala/services/ata.py:25`, `app/escala/services/ata.py:125`, `app/ponto/services/cartorio.py:837`. |
   | `gravar_orfas` | Chamado pela lavra e pela ata sintética da grade: `app/ponto/services/cartorio.py:769`; `app/escala/utils.py:1903`. |
   | Retorno `contar_orfas` | `app/folha/causas.py:44`; `app/escala/services/furos_diarios.py:24`; `app/escala/services/leitor_celula.py:191`; `app/ponto/status_dia.py:188`; `app/ponto/management/commands/detectar_par_relampago.py:143`; `app/ponto/supra_juiz.py:380`; `app/ponto/services/bordas_realizado.py:50`. |
   | Retorno `contagem_orfas_da_ata` | `app/chamados/juizes.py:511`; `app/escala/services/leitor_celula.py:421`. |
   | Retorno `orfas_da_ata` | `app/chamados/services/materializacao.py:179`; `app/escala/services/leitor_celula.py:421`; `app/ponto/juiz_batida.py:162`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/services/vigia_pareador_celula.py:77`; `app/ponto/services/cluster_espurio.py:30`. |
   | `tem_batida` dos fatos | Escrito em `app/ponto/status_dia.py:192`; transportado pelo cartório em `app/ponto/services/cartorio.py:933`, `app/ponto/services/cartorio.py:1294`, `app/ponto/services/cartorio.py:1855`; delegado pelo modelo em `app/escala/models.py:1827`; consumido pela porta em `app/ponto/portas/celula.py:963` e pela regra em `app/ponto/status_dia.py:139`. |
   | `AtaVigiada` | Estado escrito em `app/escala/services/ata.py:54`, `app/escala/services/ata.py:63`, `app/escala/services/ata.py:66`; fiscalização em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:18` e `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:20`; injeções em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:31` e `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:209`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:240`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:287`. |

   As projeções em memória também têm escritores: os dois builders, os overlays que zeram órfãs, o leitor da célula e o retorno do juiz de batida. Não são escritas adicionais no JSON persistido, mas pertencem ao contrato mudado. (`app/escala/utils.py:763`; `app/escala/utils.py:789`; `app/escala/utils.py:800`; `app/escala/utils.py:964`; `app/escala/utils.py:968`; `app/escala/utils.py:1009`; `app/escala/utils.py:1036`; `app/escala/utils.py:1045`; `app/escala/utils.py:1157`; `app/escala/utils.py:1222`; `app/escala/utils.py:1257`; `app/escala/utils.py:1262`; `app/escala/utils.py:1727`; `app/escala/utils.py:1738`; `app/escala/utils.py:1807`; `app/escala/utils.py:1816`; `app/escala/utils.py:1837`; `app/escala/utils.py:1839`; `app/escala/utils.py:2045`; `app/escala/services/leitor_celula.py:391`; `app/escala/services/leitor_celula.py:421`; `app/ponto/juiz_batida.py:165`)

   Seus leitores diretos localizados incluem adaptador de realizado, calendário, classificador de órfãs, propositor, cartório, simulador, supra, cobertura parcial e PDF. Os usos de truthiness que apenas perguntam se uma lista **já tipada** está vazia não são, por si, leitura do formato antigo; os contraexemplos são os achados 2, 3 e 5. (`app/escala/utils.py:1089`; `app/escala/utils.py:1114`; `app/colaboradores/services/calendario.py:380`; `app/escala/orfa_classe.py:54`; `app/ponto/propositor.py:36`; `app/ponto/propositor.py:81`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:1424`; `app/ponto/services/simular_dia.py:12`; `app/ponto/supra_juiz.py:516`; `app/ponto/turnos.py:2667`; `app/relatorios/pdf_espelho.py:204`)

   `nao_medidos` tem unidades distintas por resultado, que não devem ser somadas indiscriminadamente. (`app/escala/services/furos_diarios.py:53`; `app/ponto/services/esmeril_espelho.py:35`; `app/relatorios/pdf_espelho.py:218`)

   | Resultado | Unidade/escritor | Leitor/destino |
   |---|---|---|
   | Status | Indicador de quantidade de órfãs desconhecida naquele dia | `app/ponto/status_dia.py:195`; aceito, mas não usado diretamente pela decisão em `app/ponto/status_dia.py:114`. |
   | Furos | Datas não medidas por colaborador; agregado em dias-colaborador | `app/escala/services/furos_diarios.py:53`; `app/escala/services/furos_diarios.py:85`; `app/escala/services/furos_diarios.py:134`; preservação nas exclusões em `app/escala/services/furos_diarios.py:144` e `app/colaboradores/services/pos_vinculo.py:86`. |
   | Causa da folha | Dias úteis com quantidade desconhecida | `app/folha/causas.py:46`; persistido com a causa em `app/ponto/services/fechamento.py:1080`; retrato lê somente a palavra da causa em `app/folha/retrato_folha.py:1116`. |
   | Bordas | Dias passados sem turno cuja presença não foi medida | `app/ponto/services/bordas_realizado.py:85`; command publica todos os nomes em `app/ponto/management/commands/bordas_realizado.py:26`. |
   | Score geométrico | Dias da grade sem quantidade conhecida | `app/ponto/management/commands/detectar_par_relampago.py:145`; bloqueia melhora/perfeição em `app/ponto/management/commands/detectar_par_relampago.py:162` e `app/ponto/management/commands/detectar_par_relampago.py:275`. |
   | Tripwire | Células elegíveis sem identidade temporal de órfãs | `app/ponto/management/commands/tripwire_celulas.py:237`; publicado em `app/ponto/management/commands/tripwire_celulas.py:363`. |
   | Esmeril | Dias não medidos por assinatura A5 | `app/ponto/services/esmeril_espelho.py:135`; agregado/publicado em `app/ponto/services/esmeril_espelho.py:215` e `app/ponto/services/esmeril_espelho.py:245`; perdas nos leitores: achado 7. |
   | PDF | Indicador por dia e quantidade de períodos abertos não medidos | `app/relatorios/pdf_espelho.py:184`; `app/relatorios/pdf_espelho.py:218`; observação por dia em `app/relatorios/pdf_espelho.py:995`. |

   Há homônimos que não são a lista JSON: `FuroDiario.orfas` é inteiro, `contexto_json.orfas` recebe ISO, e as “órfãs” do reconciliador/diagrama são perguntas. (`app/escala/models.py:1883`; `app/ponto/portas/celula.py:1132`; `app/ponto/services/cluster_espurio.py:212`; `app/chamados/reconciliador.py:1340`; `app/core/management/commands/gerar_diagrama.py:783`)

   **Completude de todos os aliases, acessos dinâmicos e sítios de fixtures no archive inteiro: [nao verificado].** O inventário acima não deve ser convertido em declaração de zero global; o próprio instrumento declara cobertura restrita aos caminhos executados. (`app/escala/services/ata.py:47`)

9. **Comportamento dos quatro julgamentos — diferenças em relação ao diff-base fornecido.** A disponibilidade local do objeto Git `0a747193e` é **[nao verificado]**; a comparação abaixo usa os trechos anteriores presentes no anexo.

   | Julgamento | Entradas em que muda | Classe |
   |---|---|---|
   | `_celula_furada` | Sem lâmpada explicitamente apagada, quantidade ausente/inválida/inconsistente passa a devolver `None`; lista HH:MM sem `n_orfas` continua conhecida por comprimento, e quantidade positiva conhecida conserva a consulta à régua. | **(a)** mudanças quando a contagem resulta desconhecida. (`app/chamados/juizes.py:508`; `app/chamados/juizes.py:511`; `app/chamados/juizes.py:520`; `app/escala/services/ata.py:32`) |
   | `fatos_do_status` / `status_da_lavra` | Sem casada nem realizado positivo, quantidade desconhecida produz `tem_batida=None` e `sem_certidao`, antes dos ramos falta/ausência/folga/isento. Na API direta, `orfas=None,n_orfas=2` passa a provar presença. | **(a) e (b)** na API; no pipeline antigo a projeção por quantidade já podia preservar essa presença. (`app/ponto/status_dia.py:139`; `app/ponto/status_dia.py:188`; `app/ponto/status_dia.py:192`; `app/escala/services/leitor_celula.py:421`) |
   | `classificar_causa` | Quantidade positiva conhecida sem instantes passa a integrar `com_orfa`; desconhecimento pode trocar a causa por `indeterminado`. A causa conhecida da régua permanece prioritária quando há previsão. | **(a) e (b)** na API direta. (`app/folha/causas.py:44`; `app/folha/causas.py:64`; `app/folha/causas.py:68`; `app/folha/causas.py:75`; `app/folha/causas.py:78`) |
   | `classificar_dia` | `orfas=None,n_orfas>0` passa a provar fato; com chave presente e `None`, o fallback por batidas civis é suprimido. Isso também muda entrada de quantidade **conhecida zero**: `orfas=None,n_orfas=0`, previsto positivo, realizado zero e batidas civis positivas. | **(a) e (b)** na API direta. (`app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:381`; `app/ponto/supra_juiz.py:518`; `app/ponto/supra_juiz.py:526`) |

   **Supra e produção:** não demonstrei ata corrente válida, gravada pelo cartório anterior com lista e quantidade consistentes, que troque os códigos nesses dois blocos: o novo leitor conserva sua quantidade e positividade. Existência de entrada real divergente em produção e identidade global da retratação: **[nao verificado]**. (`app/escala/services/ata.py:20`; `app/escala/services/leitor_celula.py:421`; `app/ponto/supra_juiz.py:381`; `app/ponto/supra_juiz.py:519`)

   O command do supra usa esse classificador para placar, exemplos e alarme; a retratação lê `cobranca_indevida` e protestos **lavrados**. Portanto uma diferença da chamada direta não prova, isoladamente, diferença na retratação. (`app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/supra_juiz.py:239`; `app/ponto/management/commands/supra_juiz.py:246`)

10. **Vocabulário de status — `sem_certidao` já é reconhecido.** Ele está no vocabulário do núcleo e nas escolhas do modelo; o app traduz para `neutro`, o calendário conta e oferece decisão, o espelho tem estilo próprio e o relatório de furos cobra apenas `irregular`. (`app/ponto/status_dia.py:45`; `app/escala/models.py:1813`; `app/ponto/status_dia.py:84`; `app/colaboradores/services/calendario.py:94`; `app/colaboradores/services/calendario.py:791`; `app/templates/ponto/espelho.html:122`; `app/relatorios/furos.py:128`)

   Os consumidores do status lavrado localizados são calendário, espelho, relatório de furos, API e painel. Um status realmente desconhecido pode atravessar D4 se possuir rótulo gravado; sem rótulo, o lookup falha. O adaptador do app também lança `KeyError` para palavra fora do vocabulário. (`app/colaboradores/services/calendario.py:606`; `app/ponto/services/espelho.py:170`; `app/relatorios/furos.py:123`; `app/api/views.py:1483`; `app/core/services/painel_op.py:336`; `app/ponto/status_dia.py:292`; `app/ponto/status_dia.py:294`; `app/ponto/status_dia.py:91`)

11. **Testes adaptados e demais classes de reprovação.** As três adaptações perguntadas preservam suas defesas principais: COBPARC mantém previsto/furado/órfãs e acrescenta o indicador; sobra mantém os contrastes e acrescenta identidade com data/segundos; propositor conserva os resultados de tipo incoerente, composto e coerente. A troca de `['?']` por `None` acompanha o contrato novo, com rejeição do legado ainda testada no acessor. (`app/escala/tests/test_cobparc_rotulo_furos_diarios.py:34`; `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:46`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:324`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:361`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:456`; `app/ponto/tests/test_propositor.py:165`; `app/ponto/tests/test_propositor.py:170`; `app/ponto/tests/test_propositor.py:176`; `app/ponto/tests/test_propositor.py:198`; `app/escala/tests/test_tipo_na_fonte_orfas.py:25`)

   Os novos testes não são apenas mocks de saída: extraem funções reais, atravessam grade→lavra→leitor e executam mutantes em memória. Entretanto, isolam 

[truncado em 20000 de 22291 caracteres]

## 12/11 19:22 · sessao 515104c557cc · TIPO-NA-FONTE · verificador · origem=Code

**REPROVA — o ciclo foi corrigido, mas retorno misto continua sem lacuna; esse bloqueante permanece.** (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:60`)

1. **Bloqueante: retorno misto foi documentado, não corrigido.** `retorna_json()` aceita a função ao encontrar **algum** retorno JSON; depois, uma chamada à função aceita recebe proveniência JSON sem avaliar o ramo escolhido. A própria v5 declara “retorno misto […] semeia: SEM lacuna”. (`bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:243`, `bin/leitores_de_chave.py:405`, `bin/leitores_de_chave.py:60`)

   Contraexemplo executado em memória, com `f` declarada em RETURNS:

   ```python
   def f(c, b):
       if b:
           return c.ata
       return 42

   def sonda(c):
       return f(c, False)["k"]
   ```

   A ferramenta reconhece `sonda:k`, sem lacuna RETURNS ou de apelido. Há evidência de um ramo JSON, mas nenhuma prova de que essa chamada devolva JSON. O vocabulário correto da saída é **evento reconhecido**, não “acesso provado”; isso não satisfaz a correção específica JSON-ou-lacuna solicitada. (`bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:405`, `bin/leitores_de_chave.py:7`, `bin/leitores_de_chave.py:29`)

2. **Contraexemplos adicionais: ciclos fechados; apelidos conservadores nos ensaios.** Resultados obtidos em memória chamando `_verifica_returns()` e `File.scan()`, sem gravar arquivos; os mecanismos responsáveis estão nas referências de cada linha. (`bin/leitores_de_chave.py:702`, `bin/leitores_de_chave.py:771`)

   | Contraexemplo | Resultado |
   |---|---|
   | `f → g → h → f`, sem base JSON | Três lacunas RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:725`) |
   | Recursão direta `return f()` | Uma lacuna RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:725`) |
   | Ciclo de três funções retornando por variáveis intermediárias | Três lacunas RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:336`, `bin/leitores_de_chave.py:725`) |
   | JSON em um ramo, `42` no outro | Sonda reconhecida, sem lacuna: bloqueante do achado 1. (`bin/leitores_de_chave.py:243`, `bin/leitores_de_chave.py:60`) |
   | JSON em um ramo, queda implícita para `None` | Mesmo resultado: sonda reconhecida sem lacuna. (`bin/leitores_de_chave.py:240`, `bin/leitores_de_chave.py:245`) |
   | `a=c.ata; b=a; return b` | Sonda reconhecida com cadeia sintática de proveniência. (`bin/leitores_de_chave.py:340`, `bin/leitores_de_chave.py:346`, `bin/leitores_de_chave.py:357`) |
   | `a=42; return a` | Uma lacuna RETURNS; sonda não reconhecida. (`bin/leitores_de_chave.py:340`, `bin/leitores_de_chave.py:421`, `bin/leitores_de_chave.py:725`) |
   | `apelido=f; return apelido(c)` | Função intermediária cai em lacuna RETURNS; não ganhou suporte novo para alias de função. (`bin/leitores_de_chave.py:320`, `bin/leitores_de_chave.py:410`, `bin/leitores_de_chave.py:725`) |

3. **O selo morde os defeitos comportamentais principais, mas não todas as alterações documentais.** A tabela separa mordida demonstrada por ensaio em memória de ausência de asserção constatada no shell. (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:125`, `bin/tests/test_leitores_de_chave.sh:229`)

   | Verificação | Mutante que a derruba |
   |---|---|
   | `(j)`: preservar acessos anteriores | Escolher o último binding do arquivo, ignorando a posição do acesso: perde `k_antes` e `k_laco_vazio`. (`bin/leitores_de_chave.py:346`, `bin/tests/test_leitores_de_chave.sh:233`) |
   | `(j)`: limitar sementes de laço | Aplicar a semente ao escopo inteiro: aparecem os caminhos proibidos. (`bin/leitores_de_chave.py:362`, `bin/tests/test_leitores_de_chave.sh:234`) |
   | `(g)/(j)`: declarar reatribuição | Retirar a emissão de lacuna `apelido`. (`bin/leitores_de_chave.py:344`, `bin/tests/test_leitores_de_chave.sh:194`, `bin/tests/test_leitores_de_chave.sh:235`) |
   | `(k)`: `return 42` | Fazer `_returns()` devolver sempre o conjunto declarado: a sonda continua reconhecida indevidamente. (`bin/leitores_de_chave.py:108`, `bin/tests/test_leitores_de_chave.sh:108`) |
   | `(l)`: ciclo sem base | Restaurar maior ponto fixo descendente: o ciclo se sustenta, desaparecem as duas lacunas e a sonda permanece reconhecida. (`bin/leitores_de_chave.py:707`, `bin/tests/test_leitores_de_chave.sh:128`, `bin/tests/test_leitores_de_chave.sh:129`) |
   | Bloco JSON `dimensao` | Remover uma chave do bloco; o teste compara o esquema, não os valores. (`bin/tests/test_leitores_de_chave.sh:69`, `bin/tests/test_leitores_de_chave.sh:72`) |
   | Nova ressalva do stderr | **Nenhum:** apagar somente a frase “NAO acessos reais” preserva as asserções existentes. É lacuna de cobertura, não bloqueante independente. (`bin/leitores_de_chave.py:836`, `bin/tests/test_leitores_de_chave.sh:61`, `bin/tests/test_leitores_de_chave.sh:246`) |

   **(k) também passa com maior ponto fixo descendente**: esse algoritmo consegue eliminar `return 42`; quem distingue especificamente o ciclo é `(l)`. Isso foi ensaiado em memória; reversão exata para `a0f3b84f6`: **[nao verificado]**. (`bin/tests/test_leitores_de_chave.sh:101`, `bin/tests/test_leitores_de_chave.sh:121`, `bin/tests/test_leitores_de_chave.sh:128`)

   O comentário “só a proveniência RETURNS liga x ao JSON” é excessivo: existe resolução local alternativa. O controle `K0` impede que isso torne `(k)` vazio; `(l)` isolado não tem controle equivalente de ausência prévia da lacuna. (`bin/tests/test_leitores_de_chave.sh:96`, `bin/leitores_de_chave.py:409`, `bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:127`)

4. **Não encontrei promessa de completude nem catraca alimentada pela ferramenta nos textos abertos.** Docstring admite falsos positivos e construções sem lacuna; stderr rejeita completude; a linha final do selo explicita eventos reconhecidos, não acessos reais. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:51`, `bin/tests/test_leitores_de_chave.sh:273`)

   `N_REAL` e `N_DNA` vêm da ferramenta, mas recebem apenas validação de formato, rc e positividade; não há teto ou comparação histórica de catraca. (`bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:253`, `bin/tests/test_leitores_de_chave.sh:256`)

5. **Não encontrei fallback calado após falha da análise na CLI.** Raiz ausente, parse inválido e declaração sem sítio retornam rc 2 antes da emissão numérica. O fallback interno de `_returns()` para declarados existe, mas o caminho normal inicializa `RET_EF` vazio antes da análise; não é recuperação silenciosa de erro nesse caminho. (`bin/leitores_de_chave.py:810`, `bin/leitores_de_chave.py:818`, `bin/leitores_de_chave.py:823`, `bin/leitores_de_chave.py:108`, `bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:766`)

6. **Leitores e classes anteriores.** O consumidor direto encontrado na cópia é o selo, que lê stdout, stderr, rc e JSON; a régua executa esse selo e consome seu rc. Não encontrei nesse circuito rederivação paralela nem interpretação da dimensão como acessos reais. (`bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:43`, `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/tests/test_leitores_de_chave.sh:273`)

   `disjuntor.py:1565` valida referências textuais de CENSO, sem consumir o JSON ou a contagem da ferramenta. “Cota inferior” está delimitada como eventos reconhecidos; `ESCRITOR` inclui explicitamente montagem de cópia, portanto não prova persistência. (`bin/disjuntor.py:1565`, `bin/leitores_de_chave.py:7`, `bin/leitores_de_chave.py:61`)

   Mensagens de commit, delta exato contra v4, leitores externos/briefs e execução integral do selo: **[nao verificado]**. O selo integral cria e remove temporários, incompatível com a restrição desta verificação. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:35`)

## 12/11 19:22 · sessao 5e4f295d8fcb · - · - · origem=Ronald (tmux)

Achados confirmados:

1. **Leitor cru fora do acessor**: `bin/snapshot_o118_col221.py:32` lê `CelulaDia.ata['orfas']` por `.get()` e truthiness. A catraca cobre só objetos `AtaVigiada` injetados nos testes, não esse script (`app/escala/services/ata.py:46`, `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:25`).

2. **Quantidade com duas interpretações / None vira ausência**: `insumos_da_ata` continua lendo `n_orfas` cru com `or 0` (`app/ponto/services/cartorio.py:837`). Execução isolada da função real:
   - `{'orfas':['08:00'],'lampadas':[]}`: acessor conta `1`; `insumos_da_ata` devolve `(False,False)`.
   - `{'orfas':None,'lampadas':[]}`: acessor conta `None`; `insumos_da_ata` devolve `(False,False)`.
   
   Isso contradiz a explicação do próprio ramo, que prevê `(None,None)` quando houve órfã sem papel atribuído (`app/ponto/services/cartorio.py:835`, `app/escala/services/ata.py:20`).

3. **Derivação paralela da quantidade**: cartório ainda calcula `n_bat` usando `len(dia.get('orfas') or [])`, fora do acessor (`app/ponto/services/cartorio.py:1422`, `app/ponto/services/cartorio.py:1424`). O caminho lê grade construída naquele julgamento, mas continua tendo regra própria de contagem e colapsando `None` em zero (`app/ponto/services/cartorio.py:1414`, `app/ponto/services/cartorio.py:1424`).

4. **ESMERIL-SEMANTICA: vigia procura dia civil, produtor também guarda pelo turno**: `_match_marcos` devolve órfãs do turno, guardadas em `orfas_por_data[d]` e transportadas para a linha D (`app/escala/utils.py:1706`, `app/escala/utils.py:1763`, `app/escala/utils.py:1816`). Somente restos não consumidos passam pelo agrupamento civil (`app/escala/utils.py:1819`, `app/escala/utils.py:1825`, `app/escala/utils.py:1839`). O vigia procura a órfã exclusivamente na célula do dia civil da borda (`app/ponto/services/vigia_pareador_celula.py:114`).

   Contraexemplo executado sem banco: ata D tem órfã ISO de D+1 06:29 e última lâmpada apagada; ata D+1 tem `orfas=[]`; turno fechado D termina exatamente nesse instante. Função real `censo` retorna `casos=[]`, universo completo com um turno posicionado, `cego=0`. O lookup que perde o fato e o falso verde estão em `app/ponto/services/vigia_pareador_celula.py:114` e `app/ponto/services/vigia_pareador_celula.py:120`. Ocorrência dessa entrada em produção [nao verificado].

5. **Possível mesma mistura no PDF**: batida não reclamada por turno é apresentada no dia civil (`app/ponto/turnos.py:2962`, `app/ponto/services/espelho.py:575`). PDF marca órfãs consultando só a lista da ata correspondente à linha (`app/relatorios/pdf_espelho.py:644`). Uma órfã noturna armazenada na ata D, mas apresentada em D+1, perde a marca se D+1 tiver lista vazia; integração dessa entrada [nao verificado]. O calendário passou a consultar a união dos instantes justamente para permitir dono diferente da linha (`app/colaboradores/services/calendario.py:622`, `app/colaboradores/services/calendario.py:628`).

6. **Suite ficou com expectativas antigas incompatíveis**: [nao verificado]
   - JSON novo comparado a `['03:10','22:50']` e `['08:00','17:00']`: `app/ponto/tests/test_bateria_lavratura.py:637`, `app/ponto/tests/test_bateria_lavratura.py:1057`.
   - Lista de datetimes comparada a `'13:41'`: `app/escala/tests/test_ata_x_turno_entrada.py:142`.
   - Lista de datetimes comparada a `['15:00']`: `app/escala/tests/test_saida_valida_cross_meia_noite.py:102`.
   - Arquivo tocado ainda compara lista de datetimes a `['14:03']`: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:135`.
   
   Escritores novos entregam datetimes na grade e ISO no JSON (`app/escala/utils.py:622`, `app/escala/utils.py:663`, `app/escala/services/ata.py:150`). Execução Django [nao verificado].

Inventário de APIs em produção:

- `contar_orfas`: definição `app/escala/services/ata.py:20`; consumidores `app/folha/causas.py:44`, `app/escala/services/furos_diarios.py:24`, `app/escala/services/leitor_celula.py:191`, `app/ponto/status_dia.py:188`, `app/ponto/management/commands/detectar_par_relampago.py:143`, `app/ponto/supra_juiz.py:380`, `app/ponto/services/bordas_realizado.py:50`.
- `contagem_orfas_da_ata`: definição `app/escala/services/leitor_celula.py:188`; consumidores `app/chamados/juizes.py:511`, `app/escala/services/leitor_celula.py:421`.
- `orfas_da_ata`: definição `app/escala/services/leitor_celula.py:182`; consumidores `app/chamados/services/materializacao.py:179`, `app/escala/services/leitor_celula.py:421`, `app/ponto/juiz_batida.py:162`, `app/ponto/management/commands/tripwire_celulas.py:235`, `app/ponto/services/esmeril_espelho.py:133`, `app/ponto/services/vigia_pareador_celula.py:77`, `app/ponto/services/cluster_espurio.py:30`.
- JSON `orfas`: leitura no acessor `app/escala/services/ata.py:17`; escrita `app/escala/services/ata.py:151`; exceção externa `bin/snapshot_o118_col221.py:32`.
- JSON `n_orfas`: leituras `app/escala/services/ata.py:25`, `app/escala/services/ata.py:125`, `app/ponto/services/cartorio.py:837`; escrita `app/escala/services/ata.py:152`.
- Chamadores de `gravar_orfas`: `app/ponto/services/cartorio.py:769`, `app/escala/utils.py:1903`.
- `AtaVigiada`: escritores internos dos contadores `app/escala/services/ata.py:54`, `app/escala/services/ata.py:63`, `app/escala/services/ata.py:66`; leitores da fiscalização `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:18`, `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:20`; injeções adicionais `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:209`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:240`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:287`.
- Homônimos: `FuroDiario.orfas` é inteiro, não lista JSON (`app/escala/models.py:1883`, `app/ponto/portas/celula.py:1132`); `contexto_json.orfas` recebe ISO de órfãs (`app/ponto/services/cluster_espurio.py:212`); contagem de perguntas órfãs do reconciliador é outra unidade (`app/chamados/reconciliador.py:1340`, `app/chamados/reconciliador.py:1377`); perguntas sem juiz do diagrama são outro conceito (`app/core/management/commands/gerar_diagrama.py:783`).

Busca literal archive inteiro por `censo_orfas` não encontrou referências; chamadores dinâmicos [nao verificado]. Inventários detalhados de todos os sítios literais dos testes e projeções foram enviados nas mensagens anteriores; completude por análise de aliases/dinâmicos [nao verificado]. [nao verificado]

## 12/11 19:21 · sessao b314db41c707 · - · - · origem=Ronald (tmux)

**RETURNS misto continua bloqueante.** A v5 aceita a função quando encontra *algum* retorno JSON (`bin/leitores_de_chave.py:237`) e documenta que retorno misto semeia “SEM lacuna” (`bin/leitores_de_chave.py:60`). Isso mantém o contraexemplo `if b: return c.ata; return 42`: reconhecer a chamada com `b=False` não prova retorno JSON (`bin/leitores_de_chave.py:237`). O aval fornecido exige JSON ou lacuna, e o opus v4 nomeia retorno misto sem lacuna como bloqueante; anexos sem arquivo local **[nao verificado]**. O rótulo de instrumento não revoga essa correção específica (`bin/leitores_de_chave.py:29`, `:31`, `:60`).

**Remover a ressalva stderr sem derrubar o selo é observação de cobertura.** O selo testa o rótulo, mas não a frase adicional sobre acessos reais (`bin/tests/test_leitores_de_chave.sh:61`, `:246`; `bin/leitores_de_chave.py:836`). A remoção ainda preservaria “eventos reconhecidos” e a negação de completude (`bin/leitores_de_chave.py:50`, `:51`). O aval fornecido exige mutantes comportamentais específicos e rótulos nas três saídas, não mutante para cada frase documental; anexo sem arquivo local **[nao verificado]**.

## 12/11 19:20 · sessao f8513951ddd0 · - · - · origem=Ronald (tmux)

1. **(l) morde o defeito de ciclo.** Asserções exigem exatamente as duas lacunas RETURNS e `LS1=1`; mutante com maior ponto fixo descendente mantém o ciclo, elimina as lacunas e reconhece a sonda, derrubando (l). Local: `bin/tests/test_leitores_de_chave.sh:125-130`; implementação corrigida: `bin/leitores_de_chave.py:707-727`. Medição em memória sobre AST de cartório; selo shell completo não executado [nao verificado].

2. **(k) não distingue menor de maior ponto fixo descendente.** Ambos rejeitam `return 42`, produzem a lacuna e deixam de reconhecer a sonda; portanto (k) passa também no mutante com maior ponto fixo descendente. Ele mata “RETURNS sempre declarado” e “RET_EF iniciado inteiro com o laço crescente atual”. Asserções: `bin/tests/test_leitores_de_chave.sh:100-109`; reconhecimento de chamadas: `bin/leitores_de_chave.py:405-407`. Reversão exata para `a0f3b84f6` [nao verificado]: fonte histórica indisponível.

3. **Ressalva nova no stderr não tem mordida.** Apagar somente o `print` “eventos reconhecidos por posicao, NAO acessos reais” não altera nenhuma asserção do selo; o selo exige apenas o rótulo anterior e “NAO exclui o acessor”. Texto: `bin/leitores_de_chave.py:836-837`; verificações: `bin/tests/test_leitores_de_chave.sh:61-62`, `bin/tests/test_leitores_de_chave.sh:246-247`. A ressalva final é apenas emitida, sem asserção: `bin/tests/test_leitores_de_chave.sh:273`.

4. **(j) tem mordida para laço e reatribuição.** Em memória:
   - Mutante substituir `if self.dentro(...)` por aceitação de qualquer laço do mesmo escopo produz os três caminhos proibidos; cai na asserção `NAO`. Origem: `bin/leitores_de_chave.py:361-363`; asserção: `bin/tests/test_leitores_de_chave.sh:231-234`.
   - Mutante escolher o último binding do arquivo, ignorando posição do acesso, perde `k_antes` e `k_laco_vazio`; cai em `TEM`. Origem: `bin/leitores_de_chave.py:346-347`; asserção: `bin/tests/test_leitores_de_chave.sh:230-233`.
   - Mutante retirar emissão de lacuna de apelido cai em (g) e (j). Origem: `bin/leitores_de_chave.py:344-345`; asserções: `bin/tests/test_leitores_de_chave.sh:193-194`, `bin/tests/test_leitores_de_chave.sh:232-235`.

5. **Controle positivo de (l) sozinho não prova participação de RETURNS.** Mutante `retorna_json` sempre falso ainda passa (l): a sonda original reconhece `_ata_ate_o_teto` pela resolução local do retorno `dict`, e a versão cíclica deixa de reconhecer. A resolução local permite isso em `bin/leitores_de_chave.py:409-420`; (l) só verifica `LS0=0`, sem exigir ausência de lacuna prévia, em `bin/tests/test_leitores_de_chave.sh:120-130`. O mesmo mutante cai em (k), pois `K0` exige ausência da lacuna antes da mutação: `bin/tests/test_leitores_de_chave.sh:100-107`. O conjunto k+l cobre esse mutante.

6. **Limite documental:** o comentário “só a proveniência RETURNS liga x ao JSON” é mais forte que o controle implementado, pois a resolução local também reconhece o retorno original. Comentário: `bin/tests/test_leitores_de_chave.sh:96`; caminho alternativo explícito: `bin/leitores_de_chave.py:409-420`.

## 12/11 19:20 · sessao 6bfa5bcf567e · - · - · origem=Ronald (tmux)

Não encontrei bloqueante adicional nos quatro classificadores; há diferenças de API também com contagem conhecida, mas elas não demonstram divergência dos códigos para uma ata válida HH:MM+n consistente atravessando o leitor antigo/novo. (app/escala/services/leitor_celula.py:421; app/escala/services/ata.py:20; app/ponto/supra_juiz.py:379; app/ponto/supra_juiz.py:516)

1. **Comportamento — `_celula_furada`.** Sem lâmpada explicitamente apagada, a contagem desconhecida agora devolve `None`; antes a ausência de lista/n positivos acabava em `False`. Lista HH:MM sem n continua conhecida por `len`; n positivo sem lista continua acusando se a régua for confiável. (app/chamados/juizes.py:505; app/chamados/juizes.py:510; app/chamados/juizes.py:520; app/escala/services/ata.py:24)

2. **Comportamento — status.** `orfas=None,n_orfas=2`, nenhuma casada e realizado zero agora dá `tem_batida=True`, diferença também com quantidade conhecida; no pipeline antigo a projeção `['?']*n` já preservava a presença. Com quantidade desconhecida e nenhuma prova positiva, `tem_batida=None` passa diretamente a `sem_certidao`, inclusive antes dos ramos falta/ausência/folga/isento. (app/ponto/status_dia.py:187; app/ponto/status_dia.py:192; app/ponto/status_dia.py:139; app/ponto/status_dia.py:141; app/ponto/status_dia.py:152)

3. **Comportamento — causas.** Contagem conhecida positiva sem instantes passa a integrar `com_orfa`; quantidade desconhecida produz `indeterminado`, salvo causa conhecida da régua ou presença positiva no ramo sem previsão. (app/folha/causas.py:43; app/folha/causas.py:64; app/folha/causas.py:68; app/folha/causas.py:75; app/folha/causas.py:78)

4. **Comportamento — supra.** Na API direta, `orfas=None,n_orfas=2` agora abre os ramos de fato e acusa `BATIDA_ORFA_FORA_TOLERANCIA`; `orfas=None` com quantidade desconhecida agora impede o fallback civil que antes entrava com `_orfas is None`. Na travessia de ata válida e consistente do escritor anterior, a positividade e quantidade são preservadas pelo novo leitor; não identifiquei contraexemplo corrente com códigos diferentes. Existência de entrada real divergente em produção: **[nao verificado]**. (app/ponto/supra_juiz.py:379; app/ponto/supra_juiz.py:382; app/ponto/supra_juiz.py:518; app/ponto/supra_juiz.py:526; app/escala/services/leitor_celula.py:421)

5. **Retratação.** O command do supra usa `classificar_dia` para placar/exemplos/alarme; sua retratação lê o veredito `cobranca_indevida` e os protestos lavrados. Portanto diferença no classificador, isoladamente, não comprova diferença na retratação desse command. (app/ponto/management/commands/supra_juiz.py:223; app/ponto/management/commands/supra_juiz.py:239; app/ponto/management/commands/supra_juiz.py:246)

6. **Vocabulário compatível.** `sem_certidao` já existe no vocabulário, modelo e migração. App traduz para `neutro`; calendário conta e oferece resolver; relatório de furos cobra somente `irregular`; espelho possui CSS próprio; painel imprime a palavra e rótulo. Palavra desconhecida pode atravessar D4 se houver rótulo gravado, mas sem rótulo causa `KeyError`; adaptador do app e tag de rótulo também causam `KeyError`. (app/ponto/status_dia.py:45; app/escala/models.py:1813; app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:16; app/ponto/status_dia.py:84; app/ponto/status_dia.py:89; app/ponto/status_dia.py:292; app/colaboradores/services/calendario.py:94; app/colaboradores/services/calendario.py:791; app/relatorios/furos.py:128; app/templates/ponto/espelho.html:122; app/templates/core/_painel_card.html:66; app/core/templatetags/hasner_filters.py:7)

7. **Inventário status.** `fatos_do_status` escreve `tem_batida` e `nao_medidos`; cartório entrega o dict à porta via modelo; `_status_alvo` lê `tem_batida` pela regra, com precedência do admin. `nao_medidos` é aceito na assinatura, mas não participa da decisão. D4 alimenta calendário, espelho, relatório de furos, API e painel. (app/ponto/status_dia.py:192; app/ponto/status_dia.py:195; app/ponto/services/cartorio.py:933; app/ponto/services/cartorio.py:1294; app/ponto/services/cartorio.py:1855; app/escala/models.py:1820; app/ponto/portas/celula.py:951; app/ponto/status_dia.py:114; app/colaboradores/services/calendario.py:606; app/ponto/services/espelho.py:170; app/relatorios/furos.py:123; app/api/views.py:1483; app/core/services/painel_op.py:336)

8. **Leitor fora da porta / None→0.** `insumos_da_ata`, fora do diff, ainda lê n cru com `or 0`: sem lâmpadas, ata desconhecida vira `(False,False)`; lista conhecida positiva sem n também vira ausência, embora o acessor saiba contar. Isso merece consolidação como achado de semântica/fonte paralela; o escritor corrente sempre grava ambos os campos. (app/ponto/services/cartorio.py:831; app/ponto/services/cartorio.py:837; app/escala/services/ata.py:32; app/escala/services/ata.py:151)

A autoridade do status já está registrada; não identifiquei juiz novo duplicado nesses quatro classificadores. (app/core/juizes.py:195; app/docs/JUIZES.md:111)

## 12/11 19:20 · sessao dbeb5aada79d · - · - · origem=Ronald (tmux)

1. **Bloqueante — suite incompleta / leitores antigos nos testes.** Cinco assertivas ainda exigem órfãs HH:MM, incompatíveis com datetime na grade e ISO no JSON: `app/escala/tests/test_ata_x_turno_entrada.py:142`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:102`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:135`; `app/ponto/tests/test_bateria_lavratura.py:637`; `app/ponto/tests/test_bateria_lavratura.py:1057`. Os produtores novos preservam datetime (`app/escala/utils.py:663`) e gravam ISO (`app/escala/services/ata.py:150`). O terceiro arquivo foi parcialmente adaptado, deixando sua assertiva antiga (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:184`).

2. **Bloqueante — fixture existente quebra por chave agora obrigatória.** `_dia` não fornece `orfas`, mas os testes chamam `ata_do_dia`; agora a lavra executa `dia['orfas']`, produzindo `KeyError 'orfas'`. Reproduzi extraindo a fixture por AST e executando a lavra real isolada. (`app/chamados/tests/test_emis_ausencia_parcial_cobra.py:60`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:68`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:72`; `app/ponto/services/cartorio.py:769`)

3. **Selo sem mordida — teste existente ficou vacuamente verde.** O teste de exclusividade compara `lampada['luz']` HH:MM com `ata['orfas']` ISO: a interseção fica vazia mesmo que a mesma batida seja simultaneamente acesa e órfã. (`app/ponto/tests/test_bateria_lavratura.py:959`; `app/ponto/tests/test_bateria_lavratura.py:960`; `app/escala/services/ata.py:150`). Os testes similares adaptados projetam ISO para HH:MM explicitamente. (`app/ponto/tests/test_o118_orfa_que_acendeu.py:167`; `app/ponto/tests/test_lampada_acende_pela_batida.py:84`)

4. **Catraca incompleta — segundo escritor pode passar verde.** `AtaVigiada` cobre atribuição individual, mas herda `dict.update`, `dict.__ior__` e `dict.__delitem__` sem interceptação; após uma leitura legítima, escrever `a.update(orfas=...)`, `a |= {...}` ou apagar `orfas` passa em `exigir_catraca` com zero sítios crus. Reproduzi em memória. (`app/escala/services/ata.py:46`; `app/escala/services/ata.py:77`; `app/escala/services/ata.py:114`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:17`). Os mutantes publicados cobrem get/subscript/copy/dict/pop/setdefault, deixando essas escritas de fora. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:43`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:55`)

5. **Adaptações solicitadas preservam as defesas principais.** COBPARC conserva os três valores anteriores e acrescenta `nao_medidos=0`, mantendo também o caso de corrida dos deslogados. (`app/escala/tests/test_cobparc_rotulo_furos_diarios.py:34`; `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:41`; `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:46`; `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:50`). Sobra mantém contraste ponta acesa/apagada, par distante, par de ontem e par correto; acrescenta segundos e uma negativa absoluta na virada. (`app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:324`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:336`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:344`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:361`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:372`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:456`). Propositor acrescenta instante às fixtures conservando os resultados de tipo incoerente, papel composto e coerência; o teste de legado passa a usar `None`, conforme o novo contrato do acessor. (`app/ponto/tests/test_propositor.py:165`; `app/ponto/tests/test_propositor.py:170`; `app/ponto/tests/test_propositor.py:176`; `app/ponto/tests/test_propositor.py:198`; `app/escala/tests/test_tipo_na_fonte_orfas.py:25`)

6. **Os novos REDs executam funções reais, com isolamento material.** Há extração AST de funções, stubs de ORM/juízes e patches de dependências; a travessia grade→lavra→leitor realmente compara datas e segundos, e os mutantes de perda de data/persistência HH:MM executam código modificado. (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:19`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:70`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:107`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:123`; `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:39`). O teste do command verifica os argumentos passados à DP real usando `wraps`; não é só saída textual. (`app/escala/tests/test_tipo_na_fonte_orfas_v2.py:137`; `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:140`). O teste do supra com legado protege a acusação, mas não demonstra paridade com a base para atas correntes. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:74`; `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:248`)

Executei os cinco módulos novos com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ... -v`: **47 testes passaram**; resultado da minha execução [nao verificado por arquivo persistido]. Suite Django/ORM não executada [nao verificado]. [nao verificado]

**Veredito deste recorte: REPROVA**, bloqueantes 1–3; a lacuna da catraca está no achado 4. (`app/escala/tests/test_ata_x_turno_entrada.py:142`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:60`; `app/ponto/tests/test_bateria_lavratura.py:960`; `app/escala/services/ata.py:46`)

## 12/11 19:19 · sessao b314db41c707 · - · - · origem=Ronald (tmux)

1. **Único consumidor direto encontrado na cópia: o selo.** Ele executa a ferramenta e lê stdout/stderr/rc, depois interpreta o JSON em `_jq` (`bin/tests/test_leitores_de_chave.sh:32`, `:39`, `:43`). A régua chama o selo genericamente e usa seu rc, exibindo a saída somente na falha (`bin/regua.sh:138`, `:140`, `:141`). Outros leitores externos, briefs e histórico: **[nao verificado]**.

2. **Não há número de catraca vindo da ferramenta no selo.** `N_REAL` e `N_DNA` vêm de `--contar-crus`, mas só recebem checagens de formato, rc e positividade; não há teto nem comparação histórica (`bin/tests/test_leitores_de_chave.sh:57`, `:59`, `:60`, `:253`, `:255`, `:256`). A linha final os apresenta como dimensão de eventos reconhecidos, explicitamente “nao de acessos reais” (`bin/tests/test_leitores_de_chave.sh:273`).

3. **Os textos abertos não prometem completude.** Docstring admite construções sem lacuna e falsos positivos (`bin/leitores_de_chave.py:5`, `:6`, `:7`); stderr rejeita completude e acessos reais (`bin/leitores_de_chave.py:51`, `:836`); saída textual também rejeita completude (`bin/leitores_de_chave.py:872`). O selo descreve leitores/escritores reconhecidos e separa a trava do acessor (`bin/tests/test_leitores_de_chave.sh:3`, `:4`, `:5`). Mensagens de commit: **[nao verificado]**.

4. **`disjuntor.py:1565` não lê essa ferramenta.** O trecho valida se os campos textuais `leitores`/`escritores` do CENSO contêm referência arquivo:linha; não lê sua contagem nem JSON (`bin/disjuntor.py:1565`, `:1566`, `:1567`).

5. **Falhas explícitas da análise não viram número default na CLI.** Raiz ausente, erro de parse e declaração sem sítio retornam rc 2 antes da produção de `dim`/stdout numérico (`bin/leitores_de_chave.py:810`, `:812`, `:818`, `:821`, `:823`, `:826`, `:832`, `:838`). Existe fallback interno de `_returns` para declarados quando `RET_EF is None`, porém o caminho CLI inicializa o conjunto vazio e verifica RETURNS antes de construir/scannear os arquivos (`bin/leitores_de_chave.py:108`, `:109`, `:707`, `:766`, `:768`).

6. **Campo “cota inferior” está delimitado como eventos reconhecidos nos textos abertos**, incluindo a linha final do selo; não há evidência de um leitor da cópia reinterpretando-o como acessos reais (`bin/leitores_de_chave.py:7`, `:38`, `:39`, `bin/tests/test_leitores_de_chave.sh:273`). A correção de RETURNS misto e a mordida dos novos testes precisam da consolidação dos outros exames; este censo não concede APROVA global (`bin/leitores_de_chave.py:60`).

## 12/11 19:11 · sessao d9121af81e22 · TIPO-NA-FONTE · construtor · origem=Code

Entrega registrada no [relato v2](docs/tipo_na_fonte_orfas_relato_v2.txt:1), com os oito itens do censo, REDs, comandos e INCOMPLETO.

- **47 testes dirigidos passaram, rc=0.** [Registro](docs/tipo_na_fonte_orfas_execucao_v2.txt:251)
- **Sete mutantes foram rejeitados.** [Comandos e resultados](docs/tipo_na_fonte_orfas_execucao_v2.txt:307)
- Suite Django e integração completa do PDF não executadas; certificação Code/opus pendente. [Limites](docs/tipo_na_fonte_orfas_relato_v2.txt:101)

## 12/11 19:00 · sessao 16931418c3a8 · - · - · origem=Ronald (tmux)

Achados para corrigir ou declarar:

- **Alta:** legado direto `orfas=['08:00']` alcança `h.strftime` e quebra; RED atual cobre somente projeção `None` com `n_orfas`. `ponto/supra_juiz.py:520`.
- **Alta:** um único dia desconhecido transforma a janela PDF inteira em `None`, apagando identificações conhecidas no contador. `relatorios/pdf_espelho.py:492`.
- **Alta:** período sem entrada correspondente nas batidas passa a suprimir todos os alertas, ampliando o comportamento anterior. `relatorios/pdf_espelho.py:195`, `relatorios/pdf_espelho.py:990`.
- **Média:** `_furos_do_dia` mudou de três para quatro posições; três expectativas vizinhas precisam acompanhar. `escala/tests/test_cobparc_rotulo_furos_diarios.py:34`, `:46`, `:50`.
- **Limite comprovado:** `copy().get`, `dict(ata).get` e `pop` escapam da contagem de `AtaVigiada`; executei os três sem Django: `leituras_acessor=1`, `acessos_crus_ata=0`. Interceptadores atuais: `escala/services/ata.py:68`.
- **Limite de cobertura:** a catraca dirigida executa tripwire, esmeril e duas portas; não sustenta afirmação de unicidade global. `escala/tests/test_selo_tipo_na_fonte_orfas.py:25`.

Não escrevi arquivos nem executei Django/banco/rede. [nao verificado]

## 12/11 18:55 · sessao 81eaddfdbeb2 · TRANCA-VIVAS-08 · sonda · origem=Code

Transcrição integral das linhas de ids e contagens fornecidas; valores de produção [nao verificado]:

```text
comp emp2 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 07/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 08/2026 perguntas=0 chamados=207 disputas=10 com_o_admin=4 vivas_apesar_da_tranca=0
comp emp3 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 07/2026 perguntas=0 chamados=1 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 08/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 07/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 08/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
TOTAL perguntas=0 chamados=208 disputas=10 com_o_admin=4 vivas_apesar_da_tranca=0
emp2 08/2026 perguntas n=0 ids=[]
emp2 08/2026 vivas_apesar_da_tranca n=0 ids=[]
emp2 08/2026 vivas_resgatadas n=0 ids=[]
emp2 08/2026 vivas_tocadas_pelo_admin n=0 ids=[]
emp2 08/2026 fora_conversa n=0 ids=[]
emp2 08/2026 decidir_pelo_dp n=4 ids=[20599, 47879, 47881, 47884]
alvo_ainda_vivas_apesar=[]
total_erros=0
```

Montagem das seis listas explicitadas para emp2 08/2026, na cópia:

| Lista | Linha e condição |
|---|---|
| `perguntas` | Inicializada vazia em [competencia_trancada.py:90](app/chamados/services/competencia_trancada.py:90); permanece vazia por construção, conforme [competencia_trancada.py:135](app/chamados/services/competencia_trancada.py:135). |
| `vivas_apesar_da_tranca` | Recebe `q.pk` quando `tocou` é falso, em [competencia_trancada.py:158](app/chamados/services/competencia_trancada.py:158). |
| `vivas_resgatadas` | Recebe `q.pk` quando a pergunta é resgatada e viva, em [competencia_trancada.py:149](app/chamados/services/competencia_trancada.py:149). |
| `vivas_tocadas_pelo_admin` | Recebe `q.pk` quando `tocou` é verdadeiro, em [competencia_trancada.py:158](app/chamados/services/competencia_trancada.py:158); consulta do toque em [competencia_trancada.py:155](app/chamados/services/competencia_trancada.py:155). |
| `fora_conversa` | Recebe `q.pk` quando `_dia_de_turno_da_pergunta(q)` retorna `None`, em [competencia_trancada.py:142](app/chamados/services/competencia_trancada.py:142). |
| `decidir_pelo_dp` | Recebe `q.pk` quando não é resgatada e pertence a `_sem_veredito`, em [competencia_trancada.py:139](app/chamados/services/competencia_trancada.py:139); origem desse conjunto em [competencia_trancada.py:126](app/chamados/services/competencia_trancada.py:126). |

As listas representadas pelas outras contagens são montadas em [competencia_trancada.py:171](app/chamados/services/competencia_trancada.py:171) (`chamados`) e [competencia_trancada.py:183](app/chamados/services/competencia_trancada.py:183) (`disputas`). `com_o_admin` corresponde à contagem da união, sem repetição, de `decidir_pelo_dp`, `vivas_resgatadas` e `vivas_tocadas_pelo_admin`, formada em [competencia_trancada.py:343](app/chamados/services/competencia_trancada.py:343).

**O degrau efetivamente decisivo por id não está demonstrado pelo dado fornecido [nao verificado].**

| ID pedido | O que consta no dado | Degrau efetivamente percorrido |
|---|---|---|
| `20798` | Não aparece na saída transcrita [nao verificado]. | [nao verificado] |
| `47879` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |
| `47881` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |
| `47884` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |

O código permite localizar o degrau da **leitura da tranca**: retorna `False` em [juizes.py:1405](app/chamados/juizes.py:1405). Entretanto, a validação já retorna `False` antes dele, em [juizes.py:1401](app/chamados/juizes.py:1401), e existem outros retornos posteriores; portanto, resultado agregado não identifica sozinho o ramo executado. Atribuir especificamente o degrau da tranca a cada id permanece [nao verificado].

Além disso, `decidir_pelo_dp` é preenchida **antes** de consultar `pergunta_viva`, em [competencia_trancada.py:139](app/chamados/services/competencia_trancada.py:139) e [competencia_trancada.py:144](app/chamados/services/competencia_trancada.py:144). Sua presença nessa lista não registra qual degrau do juiz decidiu a pergunta.

## 12/11 18:52 · sessao 02c4b526a44e · TIPO-ITEM1-DIMENSAO · verificador · origem=Code

**1. PROCEDENTE — cura parcial; o “PRONTO” do aval ainda não está demonstrado.** Os contraexemplos exatos de acesso anterior à reatribuição e de contaminação dos dois sítios reais foram corrigidos, mas permanecem perdas, proveniências falsas e mutantes sobreviventes. (`bin/leitores_de_chave.py:318`, `bin/leitores_de_chave.py:351`, `bin/tests/test_leitores_de_chave.sh:204`)

Executei reproduções com ASTs e mutantes somente em memória, usando `python3 -B`. O selo integral, que cria e remove temporários, **não foi executado**. Identidade Git do commit e comportamento em produção: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:41`)

**2. PROCEDENTE — destino de cada achado do preverif v2.**

| Achado | Resultado |
|---|---|
| Semente de laço com prioridade e escopo inteiro | **Curado nos exemplos exatos; parcial no geral.** A semente de laço agora soma ao alias e exige pertencimento ao corpo/comprehension. Na cópia, desapareceram as contaminações em materialização e diagnóstico, preservando `lampadas.[].luz`. Persistem reatribuição interna, sombreamento e laços aninhados, detalhados abaixo. (`bin/leitores_de_chave.py:239`, `bin/leitores_de_chave.py:351`; `app/chamados/services/materializacao.py:177`; `app/ponto/services/diagnostico_escala.py:154`) |
| Reatribuição posterior apaga acesso válido anterior | **Curado no caso linear exato:** `a=c.ata; a["k"]; a={}` preserva `k` e registra lacuna. Não resolve fluxo de ramos nem todos os ciclos de aliases. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:335`, `bin/leitores_de_chave.py:337`) |
| RETURNS aceita `return 42` / `return None` | **Curado quando são os únicos retornos reconhecíveis.** Ambos retiram a função de `RET_EF` e geram lacuna. Retornos mistos, definições duplicadas e ciclos ainda permitem proveniência indevida. (`bin/leitores_de_chave.py:228`, `bin/leitores_de_chave.py:708`, `bin/leitores_de_chave.py:712`) |
| Falta mutante de RETURNS | **Curada a ausência; cobertura parcial.** k existe e exige a lacuna após trocar os retornos por `42`, mas não exige que chamadas deixem de receber proveniência. (`bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:101`) |
| “Catraca” no selo | **Curado literalmente:** o selo apresenta dimensão e cota inferior. Ainda imprime `acessos_crus_ata = N` em a, preservando o nome associado à antiga catraca. (`bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:247`) |

**3. PROCEDENTE — posição do valor não representa execução nem conclusão da atribuição.**

Os dois exemplos abaixo foram reproduzidos: o primeiro perde `k` apesar de o ramo não executar; o segundo atribui `ys.k`, sem lacuna, embora o ramo escolhido use `xs`. A lacuna depende de misturar proveniência vazia e não vazia; dois caminhos JSON diferentes não a acionam. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:335`, `bin/leitores_de_chave.py:338`)

```python
def perda(c):
    a = c.ata
    if False:
        a = {}
    a["k"]

def caminho_errado(c):
    if True:
        a = c.ata["xs"]
    else:
        a = c.ata["ys"]
    a["k"]
```

Há também erro sem ramo: cada componente da tupla recebe a posição final do respectivo valor, antes de Python concluir toda a avaliação do lado direito. Neste exemplo, a ferramenta conta a leitura como de `c.ata`, mas Python ainda lê o dicionário anterior. (`bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:337`)

```python
def atribuicao(c):
    a = {"k": 0}
    a, b = c.ata, a["k"]
```

**4. PROCEDENTE — `try`, `while`, `with`, `global/nonlocal` e `AugAssign` continuam fora da resolução correta.** Nem todos são defeitos introduzidos nesta obra: o novo resolvedor continua indexando somente atribuições específicas e alvos simples de `for`; não passou a interpretar os demais bindings. (`bin/leitores_de_chave.py:159`, `bin/leitores_de_chave.py:174`, `bin/leitores_de_chave.py:318`)

- **`try`:** uma atribuição textualmente anterior pode ser inalcançável. A reprodução abaixo perde `k` e emite apenas lacunas de alias. (`bin/leitores_de_chave.py:337`)
- **`while`:** `a=c.ata; while False: a={}` seguido de `a["k"]` perde `k`. Na direção inversa, a atribuição ao final do corpo não determina o binding da próxima volta. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:338`)
- **`with`:** `optional_vars` não entra no ambiente; o exemplo abaixo atribui indevidamente `k` à ata, sem lacuna. (`bin/leitores_de_chave.py:159`, `bin/leitores_de_chave.py:174`)
- **`global/nonlocal`:** a consulta usa somente `env[s]`, sem seguir essas declarações. Um alias da ata no módulo ou na função externa pode desaparecer no consumidor, sem lacuna. (`bin/leitores_de_chave.py:254`, `bin/leitores_de_chave.py:332`)
- **`AugAssign`:** há tratamento de papéis para subscripts, mas `a |= {"k":1}` não é indexado nem emitido como escrita; com `a=c.ata`, essa é mutação real do dicionário. (`bin/leitores_de_chave.py:160`, `bin/leitores_de_chave.py:511`, `bin/leitores_de_chave.py:641`)

```python
def tentativa(c):
    a = c.ata
    try:
        raise ValueError()
        a = {}
    except ValueError:
        pass
    a["k"]

def contexto(c, cm):
    a = c.ata
    with cm() as a:  # suponha __enter__ devolvendo {"k": 0}
        a["k"]
```

**5. PROCEDENTE — a sentinela de `For` corrige um caso e cria perdas; a semente corporal ainda contamina.**

A sentinela é instalada somente para alvo `Name`, na posição do iterável, independentemente de haver iteração. A semente, por sua vez, vale em qualquer descendente do corpo, mesmo após outro binding do mesmo nome. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:180`, `bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:351`)

| Contraexemplo reproduzido | Erro observado |
|---|---|
| `a=c.ata; for a in []: pass`, seguido de `a["k"]` | Perde `k`, embora o laço vazio preserve `a` em Python; aparece lacuna de alias. (`bin/leitores_de_chave.py:183`, `bin/leitores_de_chave.py:338`) |
| `for a in c.ata["xs"]: pass`, com `a["k"]` no `else` | Perde `xs.[].k` **sem lacuna**, inclusive quando `xs` contém um dicionário e o laço termina normalmente. O `else` está fora da região semeada. (`bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:335`) |
| Laço externo sobre `xs`, laço interno vazio reutilizando `a`, depois `a["k"]` | A semente externa sobrevive à sentinela interna. Se o laço interno receber outro dicionário, também permanece a atribuição indevida a `xs.[].k`. (`bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:354`) |
| Alvo `(a,b)` após `a=c.ata` | Não recebe a sentinela; o alias anterior pode continuar valendo dentro e depois do laço, embora Python tenha religado `a`. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:281`) |

Este contraexemplo ainda conta `xs.[].k` sem lacuna: a reatribuição não elimina a semente corporal. (`bin/leitores_de_chave.py:351`)

```python
def reatribuicao_no_corpo(c):
    for a in c.ata["xs"]:
        a = {}
        a["k"]
```

Lambda dentro do corpo também continua contaminável: `for x in c.ata["xs"]:` seguido de `f=lambda x: x["k"]` atribui `xs.[].k`, apesar do parâmetro próprio da lambda. O indexador não cria escopo para lambda. (`bin/leitores_de_chave.py:260`, `bin/leitores_de_chave.py:252`)

**6. PROCEDENTE — a regra de escrita em alvo `Name` perde escrita real e ainda conta rebinding como escrita.**

O exemplo abaixo termina atribuindo `{"k":1}` a `c.ata`, mas a ferramenta não emite o escritor de `k`. Remover somente o novo filtro de `Name`, em memória, recupera esse evento. A perda combina o filtro com o limite de um salto de `write_dict`. (`bin/leitores_de_chave.py:445`, `bin/leitores_de_chave.py:652`)

```python
def escrita_perdida(c):
    a = c.ata
    a = {"k": 1}
    b = a
    d = b
    c.ata = d
```

Na direção contrária, `a=c.ata; a=dict(a,k=1)` emite `k` como ESCRITOR, embora apenas construa uma cópia e religue `a`, sem modificar `c.ata`. Interseção de caminhos não prova identidade do objeto nem destino da cópia. (`bin/leitores_de_chave.py:393`, `bin/leitores_de_chave.py:475`, `bin/leitores_de_chave.py:653`)

Sementes declaradas também mantêm o problema: numa função declarada com parâmetro `ata`, `ata={"k":1}` é contado como escrita, e acessos posteriores seguem recebendo raiz JSON, sem lacuna de reatribuição. (`bin/leitores_de_chave.py:156`, `bin/leitores_de_chave.py:349`, `bin/leitores_de_chave.py:652`)

**Limite da evidência:** a comparação da árvore desta cópia com o mutante que remove somente esse filtro não encontrou eventos adicionais. Portanto, a regressão está demonstrada pelo contraexemplo sintético; ocorrência concreta dessa perda na aplicação atual: **[não verificado]**. (`bin/leitores_de_chave.py:652`)

**7. PROCEDENTE — RETURNS: encadeamento direto funciona; duplicação, ciclos e retornos mistos não.**

Nas reproduções abaixo, `f`/`g` foram declaradas no mapa RETURNS do módulo sintético. (`bin/leitores_de_chave.py:83`, `bin/leitores_de_chave.py:693`)

| Situação | Resultado |
|---|---|
| `f(): return g()`; `g(): return 42` | **Curado:** caem `g` e depois `f`, com duas lacunas; chamadas deixam de semear. (`bin/leitores_de_chave.py:700`, `bin/leitores_de_chave.py:712`) |
| `def f(): return {}` e depois `def f(): return 42` | **Furo:** `defs` guarda a última definição, mas os nós das duas são acumulados no mesmo escopo. O retorno antigo mantém `f` efetiva, sem lacuna. (`bin/leitores_de_chave.py:256`, `bin/leitores_de_chave.py:268`, `bin/leitores_de_chave.py:231`) |
| `f(): return g()`; `g(): return f()` | **Furo:** ambas permanecem efetivas sem origem JSON independente. O ponto fixo começa assumindo todas válidas e o ciclo se sustenta. (`bin/leitores_de_chave.py:397`, `bin/leitores_de_chave.py:697`) |
| Um ramo retorna `{}`, outro retorna `42`/`None` | **Furo:** basta **algum** retorno reconhecível; a chamada que retorna escalar ainda recebe proveniência JSON. (`bin/leitores_de_chave.py:229`, `bin/leitores_de_chave.py:236`) |
| Função declarada em arquivo de teste, retornando `None` | **Separação correta no caso reproduzido:** cai com `secao="testes"`; não aumenta as lacunas de produção da dimensão. (`bin/leitores_de_chave.py:715`, `bin/leitores_de_chave.py:726`) |

Há ainda distinção de saída: `return None` produz lacuna, mas `return` sem expressão também reprova a guarda de declaração e chega a rc 2. (`bin/leitores_de_chave.py:225`, `bin/leitores_de_chave.py:688`)

**8. DUVIDA — não confirmei dependência de ordem entre ata e dna.**

`RET_EF` é global, mas tem entradas separadas por campo, e `_verifica_returns` reinicializa a entrada consultada. A execução em memória `ata → dna → ata` produziu os mesmos eventos e lacunas nas duas passagens de ata. Não há contraprova de contaminação entre esses campos. (`bin/leitores_de_chave.py:96`, `bin/leitores_de_chave.py:100`, `bin/leitores_de_chave.py:697`)

Existe outra persistência global: `run(--sem-declaracoes)` altera permanentemente `PARAMS[field]` e `RETURNS[field]` naquele processo. Isso afeta chamadas posteriores do mesmo campo; não é defeito novo de `RET_EF`. O selo inicia um processo por chamada. (`bin/leitores_de_chave.py:799`; `bin/tests/test_leitores_de_chave.sh:41`)

**9. PROCEDENTE — os três modos comunicam dimensão, mas resta promessa indevida no docstring.**

| Saída normal de medição | Rótulo |
|---|---|
| stderr de `--contar-crus` | Contém “cota inferior”, “nem prova de completude” e o total de lacunas por tipo. (`bin/leitores_de_chave.py:46`, `bin/leitores_de_chave.py:819`) |
| Bloco `dimensao` do JSON | `nota` contém a ressalva; `rotulo` contém os números e tipos. O valor isolado de `rotulo` não contém “cota inferior”. (`bin/leitores_de_chave.py:728`, `bin/leitores_de_chave.py:834`) |
| Linha textual | Contém literalmente “cota inferior, nao e completude”, mais números e tipos. (`bin/leitores_de_chave.py:855`) |

**Permanece falsa** a frase “O que o AST nao reconhece vai a `lacunas`, que saem SEMPRE”: `with`, `nonlocal`, `AugAssign` e `for...else` demonstram omissões sem lacuna. Além disso, erros anteriores à medição retornam sem bloco de dimensão. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:160`, `bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:803`)

`NAO_COBERTOS` não promete completude; o selo não chama mais a medida de catraca. Entretanto, os falsos positivos demonstrados impedem interpretar o número como limite inferior garantido dos **acessos reais ao JSON**: ele dimensiona eventos classificados pelo reconhecedor, sujeito também a classificação errada. (`bin/leitores_de_chave.py:49`, `bin/leitores_de_chave.py:351`, `bin/leitores_de_chave.py:724`; `bin/tests/test_leitores_de_chave.sh:4`)

**10. PROCEDENTE — mordida dos casos novos é parcial.**

| Caso | Mordida e sobreviventes |
|---|---|
| **j, materialização real** | Cobra ausência da contaminação, mas não exige testemunha positiva nesse sítio. Suprimir toda a análise desse arquivo satisfaria essa asserção: há vacuidade possível do teste negativo isolado. (`bin/tests/test_leitores_de_chave.sh:66`, `bin/tests/test_leitores_de_chave.sh:70`) |
| **j, diagnóstico real** | Mais forte: exige ausência de `lampadas.[].entrada` **e presença** de `lampadas.[].luz`. (`bin/tests/test_leitores_de_chave.sh:67`, `bin/tests/test_leitores_de_chave.sh:71`) |
| **j sintético** | Morde as regressões exatas da ferramenta velha. Porém o mutante novo que devolve a união de todos os bindings passou nas três asserções reproduzidas: TEM, NAO e GAP. Falta proibir, por exemplo, `k_reatrib` de um alias religado depois. **O g existente captura esse mutante global**, pela comparação exata; a sobrevivência é de j isoladamente. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:204`) |
| **k RETURNS** | Não é vazio quanto à troca de retorno: exige ausência prévia da lacuna e pelo menos um `return 42`. Morde a ferramenta velha e o mutante “não emitir lacuna RETURNS”. (`bin/tests/test_leitores_de_chave.sh:97`, `bin/tests/test_leitores_de_chave.sh:102`) |
| **k, retirada da proveniência** | **Não morde.** Substituir somente `RET_EF[field] -= set(caem)` por `pass` mantém exatamente K1, mas chamadas continuam semeadas. Reproduzido: a chamada mutada ainda emite uma chave fictícia. (`bin/leitores_de_chave.py:712`; `bin/tests/test_leitores_de_chave.sh:101`) |
| **Rótulo a/h** | Mordem a ausência do novo prefixo na ferramenta velha. Não cobram a ressalva de completude; h cobra apenas “NAO exclui o acessor” e os números. (`bin/tests/test_leitores_de_chave.sh:61`, `bin/tests/test_leitores_de_chave.sh:220`) |
| **JSON/texto** | j verifica somente as chaves do bloco, não os valores nem a ressalva; não há asserção nova do conteúdo da linha textual. (`bin/tests/test_leitores_de_chave.sh:69`, `bin/tests/test_leitores_de_chave.sh:72`, `bin/leitores_de_chave.py:855`) |

Logo, “todos os casos mordem” não demonstra todas as propriedades anunciadas, especialmente “RETURNS inválido nunca semeia”. (`bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:247`)

**11. PROCEDENTE — QUEM ESCREVE e QUEM LÊ: consumidor comprovado é o selo; disjuntor lê outra coisa.**

| Valor | Escritor | Leitor comprovado |
|---|---|---|
| stdout numérico | `run()`, `len(top)`. (`bin/leitores_de_chave.py:815`, `bin/leitores_de_chave.py:821`) | Selo: positividade real, igualdade sintética 10, zero e DNA positivo. (`bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:178`, `bin/tests/test_leitores_de_chave.sh:220`, `bin/tests/test_leitores_de_chave.sh:228`) |
| JSON | `run()`: eventos, resumo global, dimensão e lacunas. (`bin/leitores_de_chave.py:831`) | `_jq()` e asserções do selo. (`bin/tests/test_leitores_de_chave.sh:43`, `bin/tests/test_leitores_de_chave.sh:163`) |
| Texto | `run()`: listagens e linha de dimensão. (`bin/leitores_de_chave.py:849`) | Selo usa sobretudo rc e diagnósticos; não confere a dimensão textual. (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:106`) |
| CENSO de brief | Autor do brief: **[não verificado]**. | `disjuntor.py` exige referências `arquivo:linha` em leitores/escritores; não consulta a ferramenta nem confronta seus números. (`bin/disjuntor.py:1543`, `bin/disjuntor.py:1565`) |

A busca na cópia, inclusive arquivos ocultos, não encontrou outro consumidor nominal da ferramenta ou de `CONTA_REATRIBUIDO`; briefs efetivos, logs e leitores externos: **[não verificado]**. O consumo demonstrável está nas chamadas e extrações do selo. (`bin/tests/test_leitores_de_chave.sh:32`, `bin/tests/test_leitores_de_chave.sh:41`, `bin/tests/test_leitores_de_chave.sh:43`)

O JSON conserva as chaves anteriores e acrescenta `dimensao`; não há remoção/renomeação de chave no diff fornecido. A remoção de `CONTA_REATRIBUIDO` acompanha a troca da regra de resolução, sem consumidor externo encontrado na cópia. Dependência externa dessa constante: **[não verificado]**. (`bin/leitores_de_chave.py:48`, `bin/leitores_de_chave.py:318`, `bin/leitores_de_chave.py:833`)

**12. PROCEDENTE — classes de reprovação presentes, com limites.**

- **Selo/RED sem mordida: presente parcialmente.** k não distingue “emitir lacuna” de “retirar proveniência”; j sintético não distingue resolução temporal de união indiscriminada, embora g capture esta última globalmente. (`bin/tests/test_leitores_de_chave.sh:101`, `bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:204`)
- **ESMERIL-SEMÂNTICA: presente.** `ESCRITOR` agrega mutação do JSON, construção de cópia e rebinding declarado; o filtro por interseção não separa esses significados. Já o total da dimensão e `operacoes_topo` derivam do mesmo `top`, portanto não achei duas contas diferentes para esse total. (`bin/leitores_de_chave.py:475`, `bin/leitores_de_chave.py:652`, `bin/leitores_de_chave.py:815`, `bin/leitores_de_chave.py:836`)
- **Derivação paralela: permanece na proveniência manual de PARAMS/RETURNS**, agora filtrada por uma verificação sintática parcial. Não encontrei nova derivação paralela da quantidade entre as três saídas: compartilham `dimensao()`. Implementação da catraca no acessor e seu consumo em produção: **[não verificado]**. (`bin/leitores_de_chave.py:56`, `bin/leitores_de_chave.py:83`, `bin/leitores_de_chave.py:228`, `bin/leitores_de_chave.py:817`)

## 12/11 18:52 · sessao 9c408b3bfeef · - · - · origem=Ronald (tmux)

Recomendações concretas, sem alterações:

- A projeção precisa transportar a contagem pelo novo acessor: hoje lê `n_orfas` diretamente e perde a quantidade do legado HH:MM sem esse campo. [escala/services/leitor_celula.py:414](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/services/leitor_celula.py:414)
- `_celula_furada` precisa decidir pela contagem conhecida, inclusive lista HH:MM sem `n_orfas`; quantidade desconhecida continua `None`. Hoje mistura instantes com leitura direta da contagem. [chamados/juizes.py:510](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/juizes.py:510)
- `furos_diarios._furos_do_dia` transforma desconhecimento em zero; deve usar a contagem e publicar `nao_medidos`. Atenção: a porta exige tuplas de quatro valores e está proibida nesta obra; preservar esse contrato e proteger dias desconhecidos contra exclusão demanda adaptação nos chamadores. [escala/services/furos_diarios.py:22](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/services/furos_diarios.py:22), [ponto/portas/celula.py:1125](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/portas/celula.py:1125), [colaboradores/services/pos_vinculo.py:82](/home/ronald/wt-codex-tipo-na-fonte-184856/app/colaboradores/services/pos_vinculo.py:82)
- `fatos_do_status` deve conservar quantidade conhecida, publicar `nao_medidos` e produzir `tem_batida=None` quando nenhuma outra prova permite decidir presença. Qualquer campo aditivo exige parâmetro correspondente em `status_da_lavra`, pois a porta expande o dicionário com `**fatos`. [ponto/status_dia.py:184](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/status_dia.py:184), [ponto/status_dia.py:113](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/status_dia.py:113), [ponto/portas/celula.py:963](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/portas/celula.py:963)
- `bordas_realizado` deve usar presença trivalente e contar separadamente os desconhecidos. Acrescentar `nao_medidos` em `NOMES` também o publica no command existente. [ponto/services/bordas_realizado.py:49](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/bordas_realizado.py:49), [ponto/services/bordas_realizado.py:80](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/bordas_realizado.py:80), [ponto/management/commands/bordas_realizado.py:22](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/bordas_realizado.py:22)
- `_score_geometria` deve carregar `nao_medidos` ao lado das quatro dimensões; score desconhecido não pode provar melhora nem geometria perfeita. Seus consumidores são `_melhora_estrita`, `escolher_retratavel` e `_par_embutido_retratavel_geometria`. [ponto/management/commands/detectar_par_relampago.py:140](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:140), [ponto/management/commands/detectar_par_relampago.py:154](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:154), [ponto/management/commands/detectar_par_relampago.py:244](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:244), [ponto/management/commands/detectar_par_relampago.py:267](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:267)
- `classificar_causa` deve selecionar dias com órfãs pela quantidade, incluir `nao_medidos` no resultado e impedir conclusões baseadas em ausência quando essa ausência é desconhecida. O resultado é transportado integralmente ao fechamento, permitindo campo aditivo sem mudar modelo. [folha/causas.py:43](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:43), [folha/causas.py:93](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:93), [folha/causas.py:100](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:100), [ponto/services/fechamento.py:1080](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/fechamento.py:1080)

## 12/11 18:34 · sessao e230e00fb7f6 · TIPO-NA-FONTE · construtor · origem=Code

**Entrega parcial; não pronta para pouso.** A fonte preserva instantes, mas restam leitores sem contrato de “não sei”. Resultado dirigido: **24 testes, 22 passaram e 2 selos globais falharam**. [Evidência](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:68) [nao verificado]

O [relato completo](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:15) registra arquivos::funções alterados, comandos individuais dos mutantes e limitações.

Foram alterados:

- `ata.py::ler_orfas/gravar_orfas` e `leitor_celula.py::orfas_da_ata/grade_da_celula`: ISO aware; `None` como único desconhecimento; leitor privado apagado. [ata.py:13](/home/ronald/wt-codex-tipo-na-fonte-181049/app/escala/services/ata.py:13), [leitor_celula.py:180](/home/ronald/wt-codex-tipo-na-fonte-181049/app/escala/services/leitor_celula.py:180)
- `utils.py::_celula_do_marco/_match_marcos/montar_grade_prevista/montar_realizado_grade/montar_grade_prevista_periodo_por_turno`: conservação, promoções, dry-run, reconciliação e ata sintética por instante. [Inventário](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:18)
- `cartorio.py::lampadas_do_dia/ata_do_dia/_julgar_colab_corpo`: comparação absoluta e escrita pelo acessor. [cartorio.py:599](/home/ronald/wt-codex-tipo-na-fonte-181049/app/ponto/services/cartorio.py:599)
- Calendário, classificador, propositor, ausência parcial, suprajuiz, espelho, simulação e render do detector receberam adaptações; as limitações restantes estão discriminadas no relato. [Funções e referências](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:26)

Destino de cada leitor do censo:

| Leitor | Resultado e “não sei” |
|---|---|
| `_celula_furada` | Migrado; `None` sem prova escalar de presença. `chamados/juizes.py:510` |
| `_plantio_preenche_furo_na_data` | Migrado; dado ausente não absolve, conforme contrato existente. `chamados/services/materializacao.py:143` |
| `_orfas_da_ata` / `grade_da_celula` | Privado apagado; grade transporta `None`. `escala/services/leitor_celula.py:180`, `:414` |
| `periodos_do_dia` | Migrado; transporta `None` e registra pergunta. `ponto/juiz_batida.py:161`, `:254` |
| Cluster espúrio | Migrado; `None` permanece não medido. `ponto/services/cluster_espurio.py:29` |
| Vigia pareador | Migrado; desconhecimento publica `cego=1`. `ponto/services/vigia_pareador_celula.py:120` |
| Cartão pela célula | Função sem chamador produtivo encontrado apagada; chamadas dinâmicas [não verificado]. `relatorios/cartao_pela_celula.py:277` |
| Tripwire | **Pendente:** `(achados, universo)` não representa não medidos. `ponto/management/commands/tripwire_celulas.py:237` |
| Esmeril A5 | **Pendente:** saída só contém assinaturas positivas. `ponto/services/esmeril_espelho.py:79` |

RED antes/depois:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest escala.tests.test_tipo_na_fonte_orfas -v
```

Antes: seis erros por ausência da API/acessor, observados nesta sessão; transcrição anterior em arquivo [não verificado]. Depois: os mesmos seis passaram. **O RED anterior é de contrato/API**; execução da travessia completa no HEAD original [não verificado]. [Registro](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:39)

Os mutantes de acesso cru/alias, comparação por HH:MM, perda da data na grade, fabricação de instante e escritor persistindo HH:MM foram mordidos. Cada comando está no [relato dos mutantes](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:52); resultados em [testes:7](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:7) e [testes:19](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:19). [nao verificado]

Catraca:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 tools/censo_orfas.py CelulaDia.ata orfas
```

Antes pelo mesmo scanner: **[não verificado]**. O anexo informa 21 entradas incluindo cópias; não afirmo redução comparável `21→2`. Depois: **2 acessos crus**, teto declarado **2**, meta zero pendente. [Censo](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_censo.txt:1)

Quem escreve agora: `ata.py::gravar_orfas`, chamado pelo cartório e pela ata sintética. Quem lê o JSON: `ata.py::ler_orfas`, pela porta pública; **tripwire e esmeril ainda leem cru**. [Escrita e leitura](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:87)

**INCOMPLETO:**

- Perguntas de desenho pendentes para tripwire e esmeril A5. `ponto/management/commands/tripwire_celulas.py:237`; `ponto/services/esmeril_espelho.py:79`.
- PDF: contagem desconhecida e supressão de anomalias sem identidade temporal. `relatorios/pdf_espelho.py:513`, `:716`.
- Contadores de projeção, cadeia completa do propositor e fixtures dos pontos interrompidos. [Lista detalhada](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:96)
- Suite Django, integração/render e certificação Code/opus **[não verificado]**; `django.test` indisponível nesta cópia. [Registro](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:49) [nao verificado]

LEI-AKITA: origem=cartorio.ata_do_dia/escala.utils (grade), testemunha=leitor_celula.orfas_da_ata, RED=test_tipo_na_fonte_orfas+test_tipo_na_fonte_orfas_fluxo, quem-mais-le=tripwire/esmeril (docs/tipo_na_fonte_orfas_censo.txt:3), juizes novos=0, padrao=Single Source of Truth

## 12/11 18:18 · sessao 0d22f717f4c0 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**P1 foi corrigido; P2 continua PROCEDENTE.** A trilha tornou-se obrigatória nos dois ramos, mas o relógio ainda é lido no interior e em vários instantes do mesmo Salvar. (`app/ponto/portas/celula.py:1145`, `app/ponto/portas/celula.py:1163`; `app/colaboradores/services/vinculo.py:646`, `app/colaboradores/services/vinculo.py:314`; `app/docs/PLANO-PADROES.md:92`)

1. **PROCEDENTE 1 — trilha e atomicidade**

- **Sim, ambos levantam se `evento()` devolver `None`:** conservado em `app/ponto/portas/celula.py:1151`; desfeito em `app/ponto/portas/celula.py:1163`. O retorno `None` após falha engolida está em `app/core/observ.py:105` e `app/core/observ.py:139`.
- **DNA, limpeza e evento desfazem juntos no banco nos três pontos de chamada direta:** chamadas `:262` e `:309` sob o `atomic` iniciado em `:220`; chamada `:834` sob o `atomic` iniciado em `:818`. A limpeza possui ainda seu próprio `atomic`, incluindo `salvar_e_emitir` e a trilha obrigatória. (`app/ponto/portas/celula.py:220`, `app/ponto/portas/celula.py:262`, `app/ponto/portas/celula.py:309`, `app/ponto/portas/celula.py:818`, `app/ponto/portas/celula.py:834`, `app/ponto/portas/celula.py:1145`; `app/ponto/portas/celula.py:953`)
- **Ressalva:** o objeto Python já recebeu os `None`; o código não restaura seus atributos quando o banco faz rollback. (`app/ponto/portas/celula.py:1156`)
- **Existe ancestral que captura e segue:** `censo_fase_12x36` captura `Exception`, imprime `RECUSADO` e continua o lote. A captura ocorre depois da saída de `executar_vinculo`, que é `@transaction.atomic`; portanto não confirma aquele ato como sucesso. (`app/ponto/management/commands/censo_fase_12x36.py:421`, `app/ponto/management/commands/censo_fase_12x36.py:427`; `app/colaboradores/services/vinculo.py:169`)
- Os catches históricos de `salvar_tipo` e dos receivers de vínculo cobrem a parte até hoje; o suplemento futuro está fora deles. A view encaminha a exceção para código que relança `RuntimeError`; o chamador do snapshot captura apenas `ValueError`. (`app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:328`; `app/escala/signals.py:73`, `app/escala/signals.py:81`, `app/escala/signals.py:125`, `app/escala/signals.py:132`; `app/colaboradores/views.py:438`; `app/core/erros.py:75`; `app/chamados/services/acoes_chamado.py:148`)

**O teste de banco tem mordida por construção:** exige `RuntimeError`, faz `refresh_from_db()` e confere ata, julgamento e veredito anteriores. Um mutante que ignore novamente o retorno falha já no `assertRaises`; o ramo conservado também exige a exceção. **Execução efetiva desse teste no banco: [nao verificado].** (`app/ponto/tests/test_cadastro_futuro_c5d_banco.py:38`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:43`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:56`)

2. **PROCEDENTE 2 — censo do relógio e chamadores**

O serviço `cadastro_futuro` ficou sem relógio, com `hoje` obrigatório e recusa explícita de `None`; isso corrige aquele arquivo, mas não o caminho inteiro. (`app/ponto/services/cadastro_futuro.py:9`, `app/ponto/services/cadastro_futuro.py:24`, `app/ponto/services/cadastro_futuro.py:49`, `app/ponto/services/cadastro_futuro.py:55`)

Censo do caminho de cadastro/materialização, incluindo geração, reversão e caudas imediatas; cada posição abaixo identifica a leitura na cópia:

| Arquivo:função | Leitura(s) | Classe |
|---|---|---|
| `app/core/signals.py::_feriado_regenerar_futuras` | `localdate`, `:36` | BORDA, receiver `:28` |
| `app/core/signals.py::_feriado_excluido_regenerar_futuras` | `localdate`, `:43` | BORDA, receiver `:39` |
| `app/escala/signals.py::_ec_vigencia_regenera_celulas` | `localdate`, `:67`, `:68` | BORDA, receiver `:34`; duas leituras |
| `app/escala/signals.py::_ec_ancora_regenera_celulas` | `localdate`, `:119` | BORDA, receiver `:86` |
| `app/escala/admin.py::TipoEscalaAdmin.save_model` | `localdate`, `:15` | BORDA admin |
| `app/colaboradores/views.py::vincular_folga` | `date.today`, `:242`, `:243` | BORDA view |
| `app/colaboradores/views.py::painel_vinculo` | `date.today`, `:280` | BORDA view |
| `app/colaboradores/views.py::vincular_posto` | `localdate`, `:331` | BORDA view |
| `app/escala/views.py::escala_remove_colab` | `date.today`, `:269` | BORDA view |
| `app/escala/views.py::aplicar_folga_calendario` | `date.today`, `:393` | BORDA view |
| `app/escala/views_plano_folgas.py::plano_folgas` | `date.today`, `:31` | BORDA view |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py::Command._rodar / _aplicar` | `localdate`, `:67`, `:227` | BORDA command |
| `app/ponto/management/commands/gerar_celulas.py::Command.handle` | `date.today`, `:339` | BORDA command |
| `app/colaboradores/services/vinculo.py::julgar_vinculo` | `date.today`, `:71` | INTERIOR |
| `app/escala/models.py::EscalaColaborador.recusa_ancora_futura` | `localdate`, `:965` | INTERIOR |
| `app/colaboradores/services/vinculo.py::_copiar_folga_padrao` | `date.today`, `:163` | INTERIOR |
| `app/colaboradores/services/vinculo.py::executar_vinculo` | `localdate`, `:314` | INTERIOR |
| `app/colaboradores/services/vinculo.py::trocar_calendario_de_folga` | `localdate`, `:460` | INTERIOR |
| `app/colaboradores/services/vinculo.py::fechar_vigencia` | `localdate`, `:646` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reabrir_vigencia_impossivel` | `date.today`, `:728`; `localdate`, `:740` | INTERIOR |
| `app/colaboradores/services/vinculo.py::marcar_trabalha_em_feriado` | `localdate`, `:885` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reverter_para_snapshot` | `localdate`, `:1021` | INTERIOR |
| `app/colaboradores/services/estrutura.py::_evento_do_municipio` | `localdate`, `:62` | INTERIOR |
| `app/escala/services/cadastro_tipo.py::salvar_tipo` | `localdate`, `:310` | INTERIOR |
| `app/escala/folgas.py::salvar_calendario_de_folga` | `localdate`, `:599` | INTERIOR |
| `app/escala/folgas.py::relavrar_celulas_da_foto` | `localdate`, `:280` | INTERIOR |
| `app/escala/folgas.py::aplicar_folgas_em_lote` | `_tz.localdate`, `:401` | INTERIOR |
| `app/escala/folgas.py::reparentar_folga` | `_tz.localdate`, `:512` | INTERIOR |
| `app/escala/services/escala_auto_executor.py::inicio_do_efeito / aplicar_no_vinculo / desfazer` | `localdate`, `:29`, `:98`, `:127` | INTERIOR |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `date.today`, `:188`; `now`, `:258`, `:303`; `localdate`, `:329` | INTERIOR |
| `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas` | `now`, `:264` | INTERIOR, apesar do arquivo de command |
| `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` | `localdate`, `:459` | INTERIOR |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `_tz.localdate`, `:56` | INTERIOR |
| `app/ponto/portas/celula.py::reverter_regeneracao_dia` | `now`, `:826`; `localdate`, `:831` | INTERIOR |
| `app/ponto/portas/celula.py::gerar_celulas_periodo` | `date.today`, `:860` | INTERIOR |

Na cauda de julgamento/apuração também permanecem `cartorio.julgar_celula`: `localdate` em `:2016`, `now` em `:2037` e `:2040`; `lavrar_veredito`: `now` em `:1086`; e `gravar_furos`: `now` em `:1252`, todos INTERIOR. Os `now` de regeneração/furo são carimbos; os `localdate` citados decidem alcance ou elegibilidade temporal. (`app/ponto/services/cartorio.py:2016`, `app/ponto/services/cartorio.py:2037`, `app/ponto/services/cartorio.py:2040`; `app/ponto/portas/celula.py:1086`, `app/ponto/portas/celula.py:1252`, `app/ponto/portas/celula.py:333`, `app/ponto/portas/celula.py:464`)

**Restaram defaults/fallbacks no caminho:**

| Função | Prova |
|---|---|
| `regenerar_celulas_vinculo` | `hoje=None`: `app/ponto/portas/celula.py:113`; `hoje or date.today()`: `:188` |
| `regenerar_celulas_dia` | `hoje=None`: `app/ponto/portas/celula.py:736` |
| `reverter_regeneracao_dia` | `hoje=None`: `app/ponto/portas/celula.py:773`; `hoje or`: `:831` |
| `gerar_celulas_periodo` | `hoje=None`: `app/ponto/portas/celula.py:855`; `hoje or`: `:860` |
| `aplicar_folgas_em_lote` | `hoje=None`: `app/escala/folgas.py:385`; `hoje or`: `:401` |
| `reparentar_folga` / wrapper do colaborador | `app/escala/folgas.py:471`, `app/escala/folgas.py:512`, `app/escala/folgas.py:529` |
| `recusa_ancora_futura` | `app/escala/models.py:938`, `app/escala/models.py:965` |
| executor automático: início/aplicar/desfazer | `app/escala/services/escala_auto_executor.py:24`, `:29`, `:87`, `:98`, `:118`, `:127` |
| `apurar_colab`, na cauda pós-vínculo | `app/escala/services/furos_diarios.py:25`, `app/escala/services/furos_diarios.py:30` |

**Não identifiquei chamada da cópia que passe a quebrar por omitir `hoje` nas sete assinaturas alteradas.** Os receivers passam explicitamente; os demais chamadores encontrados também. Não apareceu importação com apelido nem `getattr` literal dessas sete funções; resolução dinâmica externa permanece [nao verificado]. (`app/core/signals.py:35`, `app/core/signals.py:43`; `app/escala/signals.py:81`, `app/escala/signals.py:132`; `app/escala/admin.py:15`; `app/escala/folgas.py:298`, `:601`, `:640`; `app/colaboradores/services/estrutura.py:62`; `app/colaboradores/services/vinculo.py:333`, `:460`, `:650`, `:738`, `:898`, `:949`, `:1023`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`; `app/escala/services/cadastro_tipo.py:323`, `:328`, `:366`, `:376`, `:388`; `app/ponto/services/cadastro_futuro.py:77`)

Há chamadas sem `hoje` a funções **que continuam opcionais**, portanto ainda não quebram: `julgar_vinculo → recusa_ancora_futura` e o receiver que chama `reparentar_folgas_do_colaborador`. São pendências da classe, não novos `TypeError` deste delta. (`app/colaboradores/services/vinculo.py:133`; `app/escala/signals.py:204`)

3. **As sete “portas do ato” são INTERIOR; meia-noite continua relevante**

| Porta | Chamador que demonstra composição interna |
|---|---|
| `fechar_vigencia` | `executar_vinculo`: `app/colaboradores/services/vinculo.py:208`, `:235` |
| `reabrir_vigencia_impossivel` | command: `app/escala/management/commands/corrigir_vinculo_vigencia.py:84` |
| `reverter_para_snapshot` | serviço: `app/chamados/services/acoes_chamado.py:149` |
| `trocar_calendario_de_folga` | view: `app/colaboradores/views.py:239` |
| `salvar_calendario_de_folga` | view: `app/escala/views.py:356` |
| `_evento_do_municipio` | outras portas: `app/colaboradores/services/estrutura.py:72`, `:97` |
| `salvar_tipo` | inclusive outro serviço: `app/chamados/services/limbo_folgas.py:92` |

**Sim, podem discordar.** O executor chama `fechar_vigencia` duas vezes antes de capturar seu próprio `_hoje`; cada fechamento lê seu relógio. O comentário “UM relógio no ato” não descreve esse caminho. (`app/colaboradores/services/vinculo.py:208`, `:235`, `:314`, `:646`)

Consequência possível: fechamento antes da meia-noite de D trata D+1 como futura e limpa seu julgamento; executor depois da meia-noite captura D+1, inclui o mesmo dia na regeneração até hoje e pode executar o pós-vínculo. Outra passagem permite selecionar uma célula como futura pelo `_hoje` recebido e depois elegê-la para julgamento pela nova leitura da cauda. São decisões temporais diferentes no mesmo Salvar; ocorrência ou dano em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:246`, `:262`, `:459`, `:464`; `app/colaboradores/services/vinculo.py:319`, `:340`)

4. **QUEM ESCREVE / QUEM LÊ**

**`hoje` é dia civil de referência**, usado para competência e futuro estrito. O dia da célula/turno permanece separado em `dia` ou `cel.data`; não identifiquei dois significados nesse parâmetro alterado. `date.today()` usa a data do processo; equivalência efetiva com a data local do Django em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:26`, `:27`, `:43`, `:45`; `app/ponto/portas/celula.py:1137`; `app/ponto/management/commands/gerar_celulas.py:339`; `app/config/settings/base.py:93`)

| Parâmetro/estado | Escritor ou produtor | Leitor/efeito |
|---|---|---|
| `hoje` | bordas e serviços enumerados no item 2 | janela, filtro `> hoje`, classificação da limpeza: `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:60`; `app/escala/folgas.py:623`; `app/ponto/portas/celula.py:1137` |
| `desde`, `datas`, `substitui_dono` | chamador do evento | seleção e proteção do dono: `app/ponto/services/cadastro_futuro.py:27`, `:33`, `:37` |
| `autor/ator`, `motivo` | chamador; request/processo complementa autoria | descrição, usuário e antes/depois da trilha: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:108`, `:118`, `:134` |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `status_dia`, `status_rotulo` | `lavrar_veredito`; D1 limpa pela mesma porta: `app/ponto/portas/celula.py:1082`, `:1103`, `:1156` | grade, precedência admin, stale e status: `app/escala/services/leitor_celula.py:379`, `:424`; `app/ponto/portas/celula.py:970`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/services/cartorio.py:1084`; `app/ponto/status_dia.py:287` |
| `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar` e operação de insumos; D1 limpa: `app/ponto/portas/celula.py:1185`, `:1223`, `:1156` | cache/idempotência, stale e vigia B5: `app/ponto/services/cartorio.py:1260`, `:1409`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/portas/celula.py:1181`; `app/ponto/management/commands/vigia_b5_ausencia.py:71` |
| ações `julgamento_da_futura_conservado/desfeito` | D1 solicita; `evento` grava `LogAuditoria`: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:134` | leitores nominais encontrados nos testes; leitura genérica na auditoria: `app/ponto/tests/test_cadastro_futuro_c5.py:157`, `:204`; `app/ponto/tests/test_cadastro_futuro_c5d.py:78`, `:87`; `app/core/views.py:705` |

O vigia futuro **lê os campos da célula**, não a trilha nominal: `julgada_em`, `ata`, `veredito` e a distinção administrativa por `veredito_via`. (`app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`, `:47`)

A autoridade permanece a indicada em `JUIZES.md`: `vinculo_do_dia` e `vinculo_cobre_a_data`; o suplemento usa o carregador que chama essa juíza, e a janela vem de `janela_atual`. (`app/docs/JUIZES.md:120`; `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358`; `app/ponto/management/commands/gerar_celulas.py:299`)

5. **Classes que já reprovaram obra**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz | **Ausente no delta examinado:** permanece chamada à autoridade existente. `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358` |
| Segundo escritor fora da porta | **Ausente no delta examinado:** limpeza é operação declarada da porta; auditoria continua em `evento`. `app/ponto/portas/celula.py:80`, `:1156`; `app/core/observ.py:134` |
| Relógio fora da borda | **Presente:** exemplos novos `app/colaboradores/services/vinculo.py:646`, `:1021`; exemplo remanescente `app/ponto/portas/celula.py:188` |
| Derivação paralela de fonte única | **Não introduzida pelo delta:** dono, janela e calendário continuam nas derivações existentes. `app/ponto/services/cadastro_futuro.py:38`; `app/ponto/management/commands/gerar_celulas.py:299`; `app/escala/folgas.py:629` |
| Selo/RED sem mordida | **Problema presente:** censo do relógio cobre somente sete funções e compara nomes finais; não resolve apelidos/`getattr`. `app/ponto/tests/test_cadastro_futuro_c5d.py:25`, `:185`, `:195` |
| Ordem de trava | **Nenhuma inversão nova demonstrada:** snapshot mantém vínculo→célula; fechamento escreve vínculo antes do suplemento. Ausência global de deadlock: [nao verificado]. `app/colaboradores/services/vinculo.py:1008`, `:1013`, `:629`, `:650`; `app/ponto/services/cadastro_futuro.py:31` |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia/delta não demonstram a ordem efetiva dos pousos. | [nao verificado]
| Campo com dois significados | **Não identificado em `hoje` no delta:** referência civil separada do dia-alvo. `app/ponto/services/cadastro_futuro.py:27`, `:43`; `app/ponto/portas/celula.py:1137` |

**Há regressão concreta no instrumento anterior:** `desfazer_apaga_admin` perdeu a âncora. Ele procura o corpo antigo, enquanto o novo possui `atomic` e `_t = evento`; falha na preparação com “mutante sem alvo unico”, antes de testar a preservação administrativa. Esse vermelho não prova mais aquela propriedade. (`app/ponto/tests/test_cadastro_futuro_c5.py:37`, `:78`, `:103`, `:199`; `app/ponto/portas/celula.py:1145`, `:1147`)

6. **LEIS dos sítios**

- **L-007:** o ponto específico da triagem foi atendido: recusa de trilha ausente e rollback conjunto. (`app/docs/LEIS.md:65`; `app/ponto/portas/celula.py:1145`, `:1151`, `:1163`)
- **Clock Injection permanece descumprido no caminho:** a exigência é leitura uma vez na borda; serviços compostos continuam lendo seu próprio relógio. (`app/docs/PLANO-PADROES.md:91`, `:92`, `:94`; `app/colaboradores/services/vinculo.py:646`, `:314`)
- **L-112 preservada pelo delta:** o executor continua consumindo início e âncora declarados, gravando a âncora explícita e fechando a inativa sobreposta pela porta. O delta não converte a sugestão por batida em limite da declaração. (`app/docs/LEIS.md:154`; `app/colaboradores/services/vinculo.py:179`, `:235`, `:274`, `:285`)
- **L-021 e L-020:** o delta mantém o filtro de vigência, a escrita pela porta e a proteção de célula editada; não demonstra ampliação nova do passado. (`app/docs/LEIS.md:71`, `:72`; `app/colaboradores/services/vinculo.py:629`; `app/ponto/portas/celula.py:292`; `app/ponto/services/cadastro_futuro.py:27`, `:32`)

7. **DESCARTADO 3**

**Continua DESCARTADO.** O cron conserva o alcance de hoje consultando a juíza, passa `hoje` explicitamente e não usa `ativa=True` como resposta sobre o dono daquele dia. Vínculo inativo com fim que cobre o dia continua admitido pela autoridade; o sem vigência declarada continua recusado pela porta. O delta não altera esses critérios. (`app/ponto/management/commands/gerar_celulas.py:303`, `:321`, `:324`; `app/escala/alimentacao.py:272`, `:282`; `app/ponto/portas/celula.py:148`)

## 12/11 18:16 · sessao 452d55645847 · - · - · origem=Ronald (tmux)

Mapa de projeções, somente leitura:

- `calendario`: grava sets por `(dia,mês)` e compara `strftime('%H:%M')`; trocar por data completa e timestamps aware. Para legado, `b.orfa=None` preserva classificação desconhecida; o render usa timestamp da batida. (`colaboradores/services/calendario.py:380`, `colaboradores/services/calendario.py:622`, `templates/colaboradores/partials/_calendario_grade.html:121`)
- PDF: `_entrada_espuria`, associação de batidas e supressão de alertas devem comparar `periodo.entrada`/`b.timestamp` diretamente com instantes. As agregações `or []` e `or set()` apagam desconhecimento. (`relatorios/pdf_espelho.py:174`, `relatorios/pdf_espelho.py:477`, `relatorios/pdf_espelho.py:552`, `relatorios/pdf_espelho.py:607`, `relatorios/pdf_espelho.py:960`)
- **PERGUNTA DE DESENHO:** filtro de anomalias do PDF recebe mensagens textuais e as identifica por substring HH:MM. Não há identidade temporal disponível nesse caminho; preservar comportamento por instante exige mudança do contrato de anomalias. (`relatorios/pdf_espelho.py:716`, `relatorios/pdf_espelho.py:719`)
- `orfa_classe`: parser por fatia e diferença de minutos devem virar diferenças entre datetimes; `classificar_orfas` precisa propagar desconhecido em vez de `{}`. (`escala/orfa_classe.py:31`, `escala/orfa_classe.py:44`, `escala/orfa_classe.py:58`)
- `horas_reais_do_dia`: já possui saída `[int|None]`; pode calcular deslocamentos dos instantes reais desde a origem do dia e publicar `None` para legado. O consumidor já responde `'nao_sei'`. (`ponto/turnos.py:2616`, `ponto/turnos.py:2643`, `ponto/turnos.py:2646`, `ponto/turnos.py:2667`)
- `supra_juiz`: `join(_orfas)` quebra com datetime; formatar no texto final. O fallback por batidas deve ficar restrito à chave ausente, sem executar quando `orfas=None`. (`ponto/supra_juiz.py:514`, `ponto/supra_juiz.py:520`, `ponto/supra_juiz.py:521`)
- Espelho: transportar `None`; templates devem formatar `o|date:"H:i"` e renderizar órfãs independentemente de `realizado_grade`. (`ponto/services/espelho.py:689`, `ponto/services/espelho.py:844`, `templates/ponto/espelho.html:151`, `templates/ponto/espelho.html:240`)
- Contadores: `furos_diarios`, `status_dia`, bordas e causas devem usar `n_orfas` conhecido quando identidades forem desconhecidas. (`escala/services/furos_diarios.py:22`, `ponto/status_dia.py:188`, `ponto/services/bordas_realizado.py:49`, `folha/causas.py:43`)
- **PERGUNTA DE DESENHO:** sem lista e sem contagem, `_furos_do_dia` retorna tupla numérica e `_teve_batida_na_ata` retorna bool consumido por `int`; não possuem saída atual para desconhecido. (`escala/services/furos_diarios.py:12`, `escala/services/furos_diarios.py:23`, `ponto/services/bordas_realizado.py:21`, `ponto/services/bordas_realizado.py:80`)
- `classificar_causa` já tem resultado `indeterminado`; `dia_perfeito` pode negar prova de perfeição quando órfãs forem desconhecidas. (`folha/causas.py:34`, `folha/causas.py:80`, `ponto/services/simular_dia.py:12`)
- Render do detector de cluster precisa formatar datetime antes de `join`; detector de par relâmpago também apaga desconhecimento na contagem. (`ponto/management/commands/detectar_cluster_espurio.py:87`, `ponto/management/commands/detectar_par_relampago.py:140`)

_4400 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
