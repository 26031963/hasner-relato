# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 19:38** · publicado em **06/11/2026 19:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 19:38 · sessao f4d553ce3cb5 · - · - · origem=Ronald (tmux)

Censo do recorte solicitado, somente por leitura; nenhum teste executado [nao verificado]. [nao verificado]

**Achados que afetam a decisão**

- Há dependência histórica real de `disputa_id`: `perguntas_absorvidas_por()` identifica um ato de absorção por `(disputa_id, materializada_em)`, buscando o escape respondido e suas irmãs; mover apenas a irmã muda pode fazê-la sair desse grupo, impedindo sua devolução quando a ausência for rejeitada. A guarda “sem resposta” não exclui uma irmã muda já absorvida. Fontes: `app/chamados/services/materializacao.py:531`, `:585`, `:605`, `:609`, `:614`, `:647`, `:658`. Teste existente cobre devolução da irmã muda, sem reaponte: `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; teste com reaponte [nao verificado].
- O signal de pergunta captura somente `instance.disputa_id` depois do save; não guarda nem visita o ID anterior. A porta de beco ignora disputas fechadas. Assim, um save do reaponte não reabre nem recompõe a origem fechada que ficou vazia: `app/chamados/signals.py:191`, `:195`, `:202`; `app/chamados/services/becos.py:25`, `:27`, `:67`.
- O auditor I6 parte das perguntas precisamente para excluir disputas fechadas sem pergunta; portanto a origem vazia não será acusada por I6. A auditoria de sincronismo `motivos ↔ perguntas` só percorre abertas: `app/chamados/management/commands/auditar_invariantes_chamados.py:715`, `:737`; `app/chamados/management/commands/auditar_disputas.py:166`, `:182`.
- Reabrir disputa e reapontar FK são atos diferentes. Os caminhos existentes de reabertura abaixo leem a FK atual da pergunta e reabrem essa disputa; depois do reaponte não reencontram a origem vazia: `app/chamados/services/materializacao.py:660`, `app/chamados/services/relavra_via.py:243`, `app/chamados/services/validacao.py:191`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`.
- A validação/recusa recebendo `(pergunta_id, disputa anterior)` passa a retornar `nao_encontrada`; isso é efeito de vínculo atual, não quebra de integridade: `app/chamados/services/validacao.py:355`, `:359`, `:361`, `:526`, `:531`, `:533`.
- A resposta futura passa a produzir ocorrência no chamado-pai da disputa nova; os textos antigos não são migrados por esse leitor/escritor: `app/chamados/services/disputa_emissao.py:1344`, `:1349`, `:1355`. A cobrança também passa a consultar prazo e pai atuais: `app/chamados/services/cobranca.py:51`, `:61`, `:84`.

**Tabela de serviços**

| Sítio | Escritor/leitor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `services/materializacao.py:531`, `:567`, `:1344`, `:1393` | Lê irmãs/FK/reverse; escreve batidas, carimbos e fechamento | Trabalha com a composição atual | Nova disputa ganha unidade de materialização; origem perde a pergunta. Fechamento conta vivas atuais e só fecha aberta (`:1289`, `:1291`) | `tests/test_validar_materializa_wiring.py:51`; `tests/test_fechar_disputa_materializada.py:51`; reaponte [nao verificado] |
| `services/materializacao.py:585`, `:609`, `:614` | Leitor de vínculo histórico da absorção | **Sim**, grupo usa FK como parte da identidade do ato | Irmã muda movida pode deixar de ser devolvida na rejeição | `tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; reaponte [nao verificado] |
| `services/materializacao.py:660`, `:686` | Leitor; reabre disputa/desarquiva chamado | FK atual/fallback ao pai atual | Reabre nova disputa, ou usa pai novo no resgate | `tests/test_o261_e1_ausencia_rejeitada_devolve.py:99`; reaponte [nao verificado] |
| `signals.py:191`, `:195`; `services/becos.py:29`, `:45`, `:67` | Leitor; dispara fechamento | Só conhece FK atual; não registra origem | Avalia nova disputa no commit; fechada anterior é ignorada (`becos.py:27`) | `tests/test_varredura_becos_evento_pergunta.py:47`, `:60`, `:81`, `:94` |
| `signals.py:124`, `:139` | Gatilho da materialização no fechamento | Composição atual | Um fechamento posterior da nova disputa alcança a pergunta movida; save de pergunta não equivale a save da origem | `services/materializacao.py:1393`; teste de reaponte [nao verificado] |
| `services/validacao.py:138`, `:191`, `:249`, `:407`, `:424`, `:430`, `:458`, `:471`, `:531`, `:548`, `:566`, `:699`, `:795`, `:828`, `:847` | Leitor; valida/recusa/materializa/loga | Autoriza por vínculo atual; fallback e reabertura usam pai/FK atuais | Contexto novo; requisição amarrada à disputa antiga falha (`:355`, `:526`). Recusa reabre somente disputa atual (`:199`) | `tests/test_validar_materializa_wiring.py:51`; reaponte [nao verificado] |
| `services/acoes_disputa.py:95`, `:97`, `:129`, `:260`, `:338`, `:343`, `:350`, `:375` | Leitor; ações de questionário/reabertura/criação | Composição atual | Contador perde/ganha pergunta; origem vazia pode responder “Nenhuma pergunta cadastrada” (`:101`). Reabrir admin chama `abrir`, não atribui FK (`:259`) | `tests/test_recobrar_questionario.py:80`, `:112`, `:122`; `tests/test_reabrir_disputa_admin_sem_aberta.py:22` |
| `services/acoes_chamado.py:342`, `:348`, `:388` | Leitor de detalhe e `todas_validadas` | Composição atual | Origem fica sem perguntas; `bool(_todas_perg)` impede tratar vazio como “todas validadas” (`:388`) | Reaponte [nao verificado] |
| `services/disputa_emissao.py:300`, `:371`, `:387`, `:909`, `:912`, `:1237` | Leitor de dedup/complementação | Dedup local usa disputa atual; complementação segue FK encontrada | Origem deixa de satisfazer dedup local; complemento passa a ocorrer na nova disputa | Reaponte [nao verificado] |
| `services/disputa_emissao.py:1485`, `:1580`, `:1621`, `:1634`, `:1657`, `:1666`, `:1715` | Leitor; resolve perguntas e fecha disputas | Composição e estado atuais | Pergunta movida entra no universo de disputa aberta; fecho/reabertura do pai usa a nova disputa. Fechadas são explicitamente puladas (`:1635`, `:1661`, `:1717`) | `tests/test_b2d_resposta_visivel.py:66`, `:78`, `:87`, `:113`; `tests/test_n1_perguntas_do_app.py:225` |
| `services/disputa_emissao.py:1088`, `:1112` | Escritor de motivos e autofecho, durante emissão | Metadados não são derivados automaticamente da FK | Emissão pode atualizar motivos da nova disputa; simples atribuição da FK não executa essa escrita | `tests/test_hx_disputa_muda.py:24`, `:35`, `:52`; `tests/test_hx_disputa_muda_sem_ata.py:51`, `:63` |
| `services/declaracao_texto.py:227`, `:325`, `:536`, `:614`, `:644` | Leitor de reutilização/fato; chama `abrir`/validação | Busca local por disputa, busca global por pessoa/fato | Reutilização local segue nova disputa; agrupamento global é preservado se pessoa/motivo/data forem preservados | Reaponte [nao verificado] |
| `services/relavra_via.py:152`, `:181`, `:221`, `:243` | Leitor; relavra/desfaz carimbo/reabre disputa | Estado atual da FK | Pergunta entra no recorte “aberta”; desfazer carimbo reabre somente a disputa atual | `tests/test_o261_julgamento_desfaz_carimbo.py:135`, `:161`, `:208`; `tests/test_porta_veredito_pergunta.py:424` |
| `services/passivo_validado.py:333`, `:354`, `:516`, `:521`, `:522` | Leitor; materializa passivo e agrupa fechamento por disputa | FK atual | Contagem e quitação migram para nova disputa | `tests/test_o317_passivo_pela_porta.py:418`; reaponte [nao verificado] |
| `services/cobranca.py:61`, `:91`, `:230`, `:234` | Leitor; calcula pais/prazos, agrupa pessoa/disputa | Pai atual participa do prazo | Migra crédito da cobrança e prazo lido; aberta passa a integrar escalonamento | `tests/test_recobrar_questionario.py:88`, `:96`; reaponte [nao verificado] |
| `services/explicador.py:33`, `:122`, `:124`, `:132`, `:137` | Leitor por disputa | Composição atual | Origem vazia produz “Tudo resolvido — a disputa pode fechar” (`:139`); não a reabre | Reaponte [nao verificado] |
| `services/raiox.py:31`, `:55`, `:60`, `:135` | Leitor de stale/crédito/placar | Pai atual é fallback de crédito | Pessoa preservada mantém marcos; crédito passa ao pai novo quando destino não vence (`:142`–`:147`) | `tests/test_selo_defer_json_creditos.py:12`; reaponte [nao verificado] |
| `services/painel.py:262`, `:264`; `services/verbos.py:310`, `:312` | Leitores de empresa/praça/atividade | Dependem da pessoa, não do nascimento | Preservar pessoa preserva recorte e contagem desses leitores | Reaponte [nao verificado] |
| `services/pergunta_no_app.py:46`, `:109`; `services/aviso_pendencia.py:10`; `services/janela_colab.py:62` | Leitores por pessoa e vitalidade | Não exigem pertença original | Mesmo fato/pessoa mantém universo; `perguntas_do_app` não filtra disputa aberta nesta cópia (`pergunta_no_app.py:46`–`:53`) | `tests/test_n1_perguntas_do_app.py:58`, `:65`, `:138` |
| `services/fabrica_da_celula.py:118`, `:120`, `:137` | Leitor global do fato e estado de disputa; chama emissão | Fato por pessoa; elegibilidade também lê aberta | Mesmo fato continua no conjunto `ja`; estado “desfazível em aberta” pode mudar | `tests/test_o261_julgamento_desfaz_carimbo.py:88`, `:135` |
| `services/competencia_trancada.py:107`, `:110`, `:118`, `:140`, `:251` | Leitor de pauta/resgate/reabertura de competência | Usa pessoa e pai atuais | Pessoa/data preservam competência; fallback de resgate e associação ao chamado-pai migram | `tests/test_n1_perguntas_do_app.py:213`; reaponte [nao verificado] |
| `services/limbo_folgas.py:202`, `:238`, `:243` | Leitor; escreve **destino**, chamando `reapontar_resolvedora` | Não atribui `disputa_id` | Vínculo com pessoa mantido; reatar limbo continua sendo reaponte da resolvedora | `tests/test_f3_beco1_reata_limbo.py:49` |
| `services/vigia_fio.py:399`, `:748`, `:758`, `:785`, `:809`, `:816` | Leitor/detector | Contador “viva em fechada” depende do estado atual da FK | Pergunta deixa balde de fechadas e respectivo ID de disputa; detector não reabre (`:737`–`:759`) | `tests/test_n1_perguntas_do_app.py:162`; `tests/test_o261_vigia_fio.py:708` |
| `services/vigia_carimbo.py:102`, `:103` | Leitor/detector por empresa e juiz | Pessoa e fatos atuais | Mesmo colaborador mantém recorte; mudança no contexto de disputa pode alterar juiz chamado (`:106`, `:110`) | `tests/test_o261_vigia_fio.py:415`, `:434` |
| `reconciliador.py:368`, `:411`, `:589`, `:618`, `:1038`, `:1079`, `:1146`, `:1185`, `:1292`, `:1296`, `:1487`, `:1542`, `:1545` | Leitor; fecha disputas/carimba/reata **destino** | Filtros de aberta e derivação pelo contexto atual | Novo vínculo amplia elegibilidade de aberta; derivação recebe contexto da nova disputa; fechos só visitam disputas atuais e pulam fechadas (`:601`, `:1150`) | `tests/test_n1_perguntas_do_app.py:151`, `:302`; reaponte [nao verificado] |

**Commands: demais sítios completos do recorte**

| Arquivo/sítio | Tipo e efeito | Teste | [nao verificado]
|---|---|---|
| `management/commands/dedup_perguntas_fato.py:56`, `:57`, `:68`, `:81` | Leitor agrupa por pessoa/motivo/data; mesmo colaborador preserva grupo | Reaponte [nao verificado] |
| `management/commands/backfill_veredito_lavrado.py:101`; `lavrar_perguntas_stale.py:86` | Carregam disputa/pessoa para juiz e lavra; efeito depende dos insumos da nova disputa | `tests/test_porta_veredito_pergunta.py:424` cobre outra porta; reaponte [nao verificado] |
| `management/commands/auditar_disputas.py:72`, `:75`, `:78`, `:152`, `:156`, `:182` | Leitor de abertas, contexto e sincronismo motivos/perguntas; pergunta movida entra no universo da nova; origem fechada fica fora | `tests/test_auditar_disputas_padroes.py:80`, `:94` |
| `management/commands/auditar_invariantes_chamados.py:75`, `:144`, `:162`, `:265`, `:271`, `:297`, `:325`, `:348`, `:528`, `:536`, `:592`, `:739`, `:857`, `:893`, `:910`, `:924`, `:1160`, `:1574`, `:1705`, `:1762`, `:1777`, `:1801` | Leitores de pessoa/fato, fechada/aberta e pai; baldes dependentes do container mudam; os de pessoa preservada não. I6 exclui fechada vazia (`:715`–`:740`) | Reaponte [nao verificado] |
| `management/commands/reatar_beco_folga_ao_limbo.py:117` | Leitor para reatar **destino**, não FK de disputa | `tests/test_f3_beco1_reata_limbo.py:49` |
| `management/commands/arquivar_competencia_encerrada.py:158` | Leitor pergunta se disputa tem alguma viva; origem vazia retorna `None` | Reaponte [nao verificado] |
| `management/commands/reabrir_orfas_enterradas.py:85`, `:96`, `:97`, `:102`, `:117`, `:133`, `:136`, `:144`, `:153` | Leitor; reabre disputa/pai **atuais**, não atribui FK | `tests/test_reabrir_orfas_enterradas.py:119`; reaponte [nao verificado] |
| `management/commands/encerrar_perguntas_com_fato.py:50`, `:83` | Agrupa disputas atuais, verifica fio e fecha por porta | Reaponte [nao verificado] |
| `management/commands/curar_perguntas_em_limbo.py:29`, `:31` | Busca ocorrência no pai atual para recuperar limbo; após reaponte pode perder pista que está no pai antigo | Reaponte [nao verificado] |
| `management/commands/disparar_perguntas_competencia.py:212` | Dedup global por pessoa/motivo/data preservado | Reaponte [nao verificado] |
| `management/commands/validar_classe_a.py:100` | Validação passando disputa; ID+disputa antiga pode deixar de casar com a pergunta | Reaponte [nao verificado] |
| `management/commands/fechar_becos_disputa.py:55` | Mostra quantidade atual; porta ignora fechadas (`services/becos.py:27`) | `tests/test_varredura_becos_evento_pergunta.py:94` |
| `management/commands/smoke_chamados.py:103` | Consulta pergunta por disputa/motivo | Reaponte [nao verificado] |

`app/chamados/detector_anomalias.py` contém `detectar_anomalias` e `detectar_batida_fora_escala`, sem acesso direto encontrado à relação censada; efeito indireto de reaponte [nao verificado]. Funções: `app/chamados/detector_anomalias.py:19`, `:124`.

Neste recorte não encontrei atribuição de `PerguntaDisputa.disputa`/`disputa_id` fora da criação; os reapontamentos existentes do limbo/reconciliador escrevem **`chamado_destino`**, através de `reapontar_resolvedora`: `app/chamados/services/limbo_folgas.py:224`, `:243`; `app/chamados/reconciliador.py:1535`. Ausência absoluta de outro escritor no restante do repositório [nao verificado].

## 06/11 19:38 · sessao b78a0bd72132 · - · - · origem=Ronald (tmux)

Censo somente por leitura; testes não executados. As referências abaixo distinguem teste de vencimento, teste apenas com prazo futuro e teste estrutural. [nao verificado]

**Casos que fixam explicitamente o comportamento (2)**

| Leitor atingido | Caso existente | O que fixa |
|---|---|---|
| `particionar_verbos` | `app/chamados/tests/test_f1_arquivar.py::test_prazo_vencido_vira_historico_e_nao_volta_pra_fila` — definição :48, vencido :52, asserts :55-57 | Vencido vira `historico`; deixa `decidir` e `arquivado` zerados. |
| `particionar_verbos` | `app/chamados/tests/test_f7_janela_arquivo.py::test_vencida_a_janela_vira_historico_e_NAO_volta_pra_fila` — :41-46 | Mesmo contrato de vencido fora da cobrança. |
| `particionar_verbos` | `app/chamados/tests/test_f7_janela_arquivo.py::test_historico_nao_conta_como_registro` — :48-51 | Vencido mantém verbo não nulo; não prova sozinho exclusão da cobrança. |
| Lista padrão do painel | `app/chamados/tests/test_f8_lista_esconde.py::test_lista_padrao_esconde_arquivado_e_historico` — :36-43 | Prazo futuro e vencido ficam fora da lista padrão. |
| Pílula Arquivados | `app/chamados/tests/test_f8_lista_esconde.py::test_pilula_arquivados_mostra_so_os_da_janela` — :45-51 | Apenas futuro entra na pílula; vencido fica fora. Essa expectativa pode continuar válida após a cura. |
| Busca histórica | `app/chamados/tests/test_f9_busca_historico.py::test_so_arquivados_nunca_o_vivo` — :51-56; fixture vencida :32 | Campo preenchido, inclusive vencido, pertence à busca; sem campo não pertence. Isso não prova que o vencido precise sair da cobrança. |
| Busca histórica | `app/chamados/tests/test_f9_busca_historico.py::test_busca_por_colab` — :58-61; fixture :32 | Busca encontra o arquivado vencido. |
| Ficha do colaborador | `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado` — :98-114 | Datas 2000 e 2999 (:109-110) produzem dono `arquivado` (:113); exige exercitar ambos os verbos de `FORA_DA_LISTA_PADRAO` (:114). |
| Contrato estrutural da ficha | `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_o_dono_arquivado_le_FORA_DA_LISTA_PADRAO` — :118-130 | Exige uso da constante e presença de `historico` nela (:129-130). |
| Porta alcançável do dia | `app/chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` — :144-165 | Com vencido, conserva `{id: 'historico'}` e `fila=()` (:162-164); futuro nem abre porta (:159-160). |

**Leitores com testes apenas de prazo futuro — não atribuir cobertura de vencido** [nao verificado]

- `janela_colab`: `app/chamados/tests/test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, :55-60; futuro em :58.
- Fio com janela: `app/chamados/tests/test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, :41-48; helper põe futuro em :27.
- Complemento “meus arquivados”: `app/chamados/tests/test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, :45-52; helper põe futuro em :33.
- Porta da pergunta: `app/chamados/tests/test_selo_i6_ouve_a_lei.py::test_MORDE_destino_silenciado_nao_da_porta`, :136-149; futuro em :143.
- API mensageria: `app/api/tests/test_api_mensageria_fila.py::test_MORDE_o_arquivado_viaja_separado_do_total`, :159-189; futuro em :178.
- Isentos/evento: `app/chamados/tests/test_varredura_isentos_evento.py::test_MORDE_o_resgate_de_isento_volta_ao_veu_pelo_evento`, :57-68, fixa ressilenciamento após limpar o campo, sem testar prazo vencido.
- Prescrição: `app/chamados/tests/test_f6_prescricao.py::test_idempotente`, :54-61, repete imediatamente após silenciar; não testa vencido.

**Casos existentes que fixam (1), inclusive vencido**

- `app/chamados/tests/test_f9_contador_bate_com_a_lista.py::test_silencio_VENCIDO_volta_sozinho`, :90-96: vencido pertence a `qs_na_fila`.
- `app/escala/tests/test_ranking_chamados_pela_fila.py::test_silencio_vencido_volta_a_fila`, :62-64: fixture vencida para ranking.
- `app/chamados/tests/test_f3_beco1_reata_limbo.py::test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata`, :149-160: vencido recebe reata.
- `app/chamados/tests/test_fio_contador_ativos_pela_fila.py::test_o_numero_e_o_da_fila_do_painel_para_o_colab`, :59-65: fixture vencida em :64; contador igual à fila em :65.

**Demais testes indiretos: limites** [nao verificado]

- `app/inteligencia/tests/test_intel_juiz.py::test_01_MORDE_na_fila_e_o_verbo_nao_o_status`, :35-42: mock de mapa com `validar`, `None`, `arquivado`; fixa seleção por verbo, sem testar vencido ou `historico`.
- `app/chamados/tests/test_badge_topo_coerente.py::test_badge_igual_soma_dos_verbos`, :9-21: compara badge com `validar + decidir`, sem fixture silenciada.
- `app/core/tests/test_badge_dois_numeros.py::test_MORDE_o_headline_NAO_e_a_soma_dos_verbos`, :65-74: comentário cita histórico, mas fixture não põe silêncio.
- `app/chamados/tests/test_e5b_cobrar_massa.py`, :52-81: deduplicação, filtros e prévia, sem fixture silenciada; cobertura específica de vencido nesse envio [nao verificado].
- `app/chamados/tests/test_q2ja_v2_corte_por_janela.py`, :90-104, e `app/chamados/tests/test_rotulo_solicitacao.py`, :79-82: testam regra de corte do arquivador, sem testar vencido.

**Inventário dos 29 arquivos de testes que contêm literalmente `silenciado_ate` ou `PRAZO_ARQUIVO_DIAS`** [nao verificado]

| Arquivo | Escrita/leitura encontrada |
|---|---|
| `app/api/tests/test_api_mensageria_fila.py:178` | Fixture de futuro; saída da API em :182-185. |
| `app/colaboradores/tests/test_ficha_do_colab.py:110` | Fixture vencido/futuro; dono e verbos em :113-114; AST em :118-130. |
| `app/core/tests/test_registrado_nao_e_pendencia.py:108` | Fixture de futuro para card do posto. |
| `app/core/tests/test_contract_escritores_estado.py:35` | Declara escritor único permitido. |
| `app/escala/tests/test_ranking_chamados_pela_fila.py:54` | Fixtures futuro :54 e vencido :64. |
| `app/ponto/tests/test_contract_juiz_feriado.py:56` | Fonte sintética; AST da definição única em :66-72. |
| `app/chamados/tests/test_agir_por_dono.py:56` | Assert campo nulo após recusa. |
| `app/chamados/tests/test_sla_pela_fila.py:72` | Fixture futuro; exclusão do SLA em :74. |
| `app/chamados/tests/test_f3_beco1_reata_limbo.py:113` | Fixtures futuro :113/:140 e vencido :157. |
| `app/chamados/tests/test_c1_janela_colab.py:34` | Helper escreve fixture; teste futuro :55-60. |
| `app/chamados/tests/test_c2_endpoints_janela.py:27` | Helper monta atualização; salva em :28. |
| `app/chamados/tests/test_c3_meus_arquivados.py:33` | Helper monta atualização; salva em :34. |
| `app/chamados/tests/test_fio_contador_ativos_pela_fila.py:56` | Fixtures futuro :56 e vencido :64. |
| `app/chamados/tests/test_c4_resgate.py:33` | Fixture; asserts nulo/não nulo em :44/:48/:58/:71. |
| `app/chamados/tests/test_f1_arquivar.py:35` | Fixtures e retorno em :35-57. |
| `app/chamados/tests/test_f9_contador_bate_com_a_lista.py:50` | Helper escreve fixture; contrato vencido em :90-96. |
| `app/chamados/tests/test_porta_arquivo_silencio.py:87` | Leitura genérica dos campos; asserts :136/:153/:181/:202/:226/:264/:400; fontes sintéticas :441-454; regex do prazo :456-505. |
| `app/chamados/tests/test_f6_prescricao.py:41` | Asserts :41/:46-47; captura e compara prazo :57/:60. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:157` | Fixtures vencido/futuro :157; futuros :202/:233. |
| `app/chamados/tests/test_varredura_isentos_evento.py:43` | Asserts :43/:46/:55/:64/:68/:75/:85/:107. |
| `app/chamados/tests/test_selo_i6_ouve_a_lei.py:54` | Fixtures futuro :54-55 e :143-144. |
| `app/chamados/tests/test_f4_arquivar_lote.py:44` | Asserts :44-45/:57. |
| `app/chamados/tests/test_silenciar_isentos.py:12` | Inspeção textual; aceita menção inclusive em comentário. |
| `app/chamados/tests/test_f3_arquivar_porta.py:42` | Asserts :42/:49-50/:72; fixture :65-67. |
| `app/chamados/tests/test_s4_pendente_suspende_cobranca.py:58` | Asserts :58-59/:79/:105; fixtures :98/:113. |
| `app/chamados/tests/test_o261e_f5_alcance.py:88` | Helper de fixture futuro. |
| `app/chamados/tests/test_f9_busca_historico.py:40` | Helper escreve fixture :40-41; vencido preparado em :32. |
| `app/chamados/tests/test_f7_janela_arquivo.py:27` | Helper escreve fixture :27-28; expectativas :35-56. |
| `app/chamados/tests/test_f8_lista_esconde.py:27` | Helper escreve fixture :27-28; expectativas :36-51. |

O teste do prazo padrão é `app/chamados/tests/test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo`, :386-402: importa a constante em :387 e compara `silenciado_ate` com `now + PRAZO_ARQUIVO_DIAS` em :399-402. O gate de soma única é `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta`, :456-490; a autoridade única é `app/ponto/tests/test_contract_juiz_feriado.py::test_MORDE_prazo_de_arquivo_definido_uma_vez`, :66-72.

## 06/11 19:38 · sessao 2434b4ff5fe8 · - · - · origem=Ronald (tmux)

Censo complementar concluído, somente leitura.

**Estado além do silêncio:** não existe `arquivado` nos estados de `ChamadoColaborador`; os campos de arquivo/silêncio são `silenciado_ate`, `silenciado_por`, `silenciado_motivo`, `resgatado_em`, sem `arquivado_em` na declaração do modelo (`app/chamados/models.py:22-95`; estados catalogados em `app/chamados/catalogo/motor.py:19-31`). Há, porém, **arquivo por encerramento da competência trancada**: `via_encerramento='competencia_trancada'`, independente de silêncio (`app/chamados/catalogo/motor.py:90-94`; `app/chamados/models.py:1552-1569`).

| Sítio | Papel / efeito | Cura do relógio |
|---|---|---|
| `app/chamados/views_arquivar.py:165` | Inclui no arquivo `silenciado_ate` preenchido **ou** via `competencia_trancada`. | Via continua no arquivo mesmo sem silêncio. |
| `app/chamados/views_arquivar.py:64` | Detecta via da tranca e chama `reabrir(humano=True)` antes de levantar véu. | Sem alteração só pelo relógio. |
| `app/chamados/services/disputa_emissao.py:1572` | Não resolve perguntas pela reconciliação comum quando encerramento veio da tranca. | Sem alteração. |
| `app/chamados/management/commands/arquivar_prescritos.py:54` | `resgatado_em` preenchido impede prescrição automática. | Vencido retornar à cobrança não implica ser novamente prescrito. |
| `app/chamados/services/competencia_trancada.py:104` | Resgate posterior a `fechado_em` imuniza contra nova tranca; aplicação da proteção em `:136-138`. | Sem alteração. |
| `app/chamados/services/silencio_ausencia.py:66` | Prefixo de `silenciado_motivo` identifica propriedade do silêncio por ausência. | Não constitui arquivo independente. |
| `app/chamados/services/arquivo.py:123-126` | Lê autor, motivo, resgate e prazo para fotografia auditável. | Outro; não decide fila. |

Teste de arquivo **sem silêncio**: `app/chamados/tests/test_competencia_trancada.py::test_o_arquivo_lista_o_encerrado_pela_tranca` (`:203-209`). Teste de resgate/proteção: `::test_MORDE_desarquivar_devolve_e_a_segunda_passada_respeita_o_admin` (`:164-175`).

**Consumidores diretos da partição** — propagam tipo (2), sem ler o campo diretamente:

- `app/colaboradores/services/ficha.py:155`, dono em `:160`: vencido troca `historico`/dono `arquivado` pelo verbo e dono efetivos.
- `app/chamados/views_arquivar.py:124`: seleção da prescrição em massa; ainda existe trava própria `isnull=True` em `:136`.
- `app/chamados/views_cobrar.py:77-78`: vencidos de verbo cobrar entram na cobrança em massa, sujeitos aos demais gates.
- `app/chamados/juizes.py:1287`, filtro visível em `:1385`: vencido deixa de ser escondido de `fila`; verbo alimenta também a escada do fio em `:1386-1387`.
- `app/chamados/services/fila.py:55`: redistribui contagens `historico` para verbo.
- `app/chamados/services/painel.py:228`, filtro em `:327`: vencido volta à lista padrão e à pílula efetiva.
- `app/chamados/services/verbos.py:171`: lente passa tratar vencido pelo verbo efetivo.
- `app/chamados/services/na_fila.py:19-25`: vencido passa nos contadores por colaborador/empresa quando verbo pertence a `VERBOS`.
- `app/chamados/management/commands/arquivar_prescritos.py:50`: partição usada, porém `:53` continua protegendo vencidos pela presença.
- `app/api/views_mensageria.py:395`, saída `:421`: JSON de pendências mantém itens, muda o `verbo`.
- `app/api/views_mensageria.py:453`, buckets `:459-470`: histórico redistribuído por verbo.
- `app/api/views_mensageria.py:1149-1151`: vencidos com verbo decidir entram no endpoint decidir.
- `app/core/context_processors.py:90`: fallback do badge admin inclui vencidos que sejam validar/decidir.

Consumidores de `FORA_DA_LISTA_PADRAO`: `app/chamados/services/painel.py:327`; `app/chamados/juizes.py:1385`; `app/colaboradores/services/ficha.py:160`.

Testes indiretos que fixam **vencido** como tipo (2), encontrados pelo agente de testes: [nao verificado]

- `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado` (`:98-114`).
- `app/chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` (`:144-165`).

`na_fila` tem teste de delegação, mas não de vencido/histórico: `app/inteligencia/tests/test_intel_juiz.py::test_01_MORDE_na_fila_e_o_verbo_nao_o_status` (`:35-42`).

**Janela do colaborador:** consumidores diretos são `app/chamados/services/fio.py:171`, `app/chamados/views_arquivar.py:195` e `app/api/views.py:1016`. Fio do colaborador ativa esse recorte em `app/chamados/views.py:569`; o complemento para o drawer exclui IDs visíveis em `app/chamados/views_arquivar.py:196-197`. Curar `janela_colab.py:73` devolve vencidos que também satisfaçam janela/pergunta e módulos permitidos; remove esses mesmos itens do complemento do drawer (`app/chamados/services/janela_colab.py:67-74`).

**Consumidores diretos de `qs_na_fila`**, todos propagando (1):

| Arquivo | Linhas |
|---|---|
| `app/chamados/views.py` | `:901` |
| `app/chamados/juizes.py` | `:1290` |
| `app/chamados/services/sla.py` | `:62` |
| `app/chamados/services/fila.py` | `:92`, `:150` |
| `app/chamados/services/painel.py` | `:210` |
| `app/chamados/services/fila_por_causa.py` | `:160` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py` | `:47` |
| `app/chamados/management/commands/alertar_chamados_sla.py` | `:30` |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py` | `:59`, `:100` |
| `app/core/services/painel_op.py` | `:119`, com `annotate(total=Count(...))` |
| `app/core/views.py` | `:211`, `:1193` |
| `app/core/context_processors.py` | `:93`, `:150`, `:187` |
| `app/escala/services/furos_diarios.py` | `:212`, com `annotate(n=Count(...))` |
| `app/escala/services/propositor.py` | `:632` |
| `app/ponto/services/lastro.py` | `:239`, `:387` |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `:102` |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `:72` |

**Prazo:** leitor numérico runtime único atual é `arquivo.prazo_padrao`, que importa a constante em `app/chamados/services/arquivo.py:117` e soma em `:118`; definição `app/chamados/catalogo/motor.py:84`. Registro em `app/core/juizes.py:285`, catraca de definições em `:1387`.

Testes do prazo: [nao verificado]

- `app/chamados/tests/test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo` (`:386-402`).
- `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta` (`:456-490`).
- `::test_MORDE_o_proprio_varredor_do_teto_ve_CODIGO` (`:492-505`).
- `app/ponto/tests/test_contract_juiz_feriado.py::test_MORDE_o_selo_ve_corte_e_prazo_na_mao` (`:55-60`).
- `::test_MORDE_prazo_de_arquivo_definido_uma_vez` (`:66-72`).

Serialização de chamados encontrada é manual: `app/api/views.py:1121` em diante; `app/api/views_mensageria.py:411-422`. Não localizei arquivo com nome contendo `serializ` na cópia; eventual serialização dinâmica não identificada é [não verificado].

## 06/11 19:38 · sessao 1b606e328507 · - · - · origem=Ronald (tmux)

Censo externo, somente leitura. “Efeito” abaixo é inferência do código para reaponte entre disputas do **mesmo colaborador**; execução desse desenho em testes ou produção [nao verificado]. [nao verificado]

| Sítio | Leitor/escritor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/api/views.py:1007` | Leitor declarado: `_pend`, filtro colaborador/disputa aberta/sem resposta, `values_list(destino,pai)` até 1010 | Não demonstra: `_pend` não é consumido posteriormente | **Esse trecho isolado não lista nada:** constrói QuerySet sem avaliação; listagem efetiva abaixo | Teste desse trecho isolado [nao verificado] |
| `app/api/views.py:1028`, `1043`, `1047`, `1158` | Leitor efetivo da lista v2: somente perguntas com FK em disputas abertas; destino vivo ou fallback para `p.disputa.chamado_id` | Usa contêiner atual, sem memória do nascimento | A pergunta antes excluída pela FK fechada entra no conjunto da viva; fallback de chamado muda para pai novo | `app/api/tests/test_chamados_v2_destino.py:54` fixa destino; `:60` fixa fallback; `:67` fixa permanência na janela |
| `app/api/views.py:1182` | Leitor de disputa e quota do card: `disputa_id`, `reaberturas_colab`, `pode_solicitar_correcao` até 1188 | Quota pertence à disputa atual | Pergunta passa a usar cabeçalho/quota de correção da nova disputa | Teste específico de troca da FK [nao verificado] |
| `app/api/views.py:2551`, `2563`, `2573`, `2586`, `2590` | Lista `/disputa/perguntas/`: fonte `perguntas_do_app`; serializa FK de cada pergunta e usa primeira disputa no envelope/fallback | Não exige disputa natal; envelope depende da primeira pergunta | FK serializada muda; pai de fallback muda; identidade da pergunta permanece `p.pk` em 2575 | `app/chamados/tests/test_exibicao_so_viva.py:60`; `app/chamados/tests/test_pergunta_no_app.py:89` |
| `app/api/views.py:2628`; `app/chamados/views.py:1239` | Leitores de autorização da resposta: PK + colaborador via disputa; vitalidade pelo juiz | Não; não filtram disputa aberta | Mesmo colaborador conserva acesso; não dependem do ID antigo; mudar colaborador alteraria autorização | Teste específico de reaponte [nao verificado] |
| `app/api/views.py:2619`, `2647` | Escritor de **log**, não FK: eventos por modelo `PerguntaDisputa` e PK | Preserva identidade por PK | Trilha de tentativas/sucesso continua associada à mesma pergunta | Teste de continuidade após troca [nao verificado] |
| `app/chamados/views.py:399` | Releitura do cartão HTMX pelo PK, `select_related(disputa__colaborador)` | Não | Re-render usa relação atual | Teste de FK móvel [nao verificado] |
| `app/chamados/views.py:493`, `510`, `528` | Leitores dos gates validar/recusar/corrigir: FK deve apontar disputa aberta e `chamado_que_lista` deve coincidir | Supõem disputa **operacional** atual aberta; não natal | Reaponte torna pergunta elegível nesses gates; validação recebe `_perg_pre.disputa` em 501 | `app/chamados/tests/test_fio_disputa_por_destino.py:21` fixa resolução por destino, não troca FK |
| `app/chamados/views.py:1410`, `1416`, `1420` | Leitor da reabertura/recobrança: resolve disputa do card, consulta perguntas atuais, corrigíveis e pendentes vivas | Trabalha sobre membros atuais | Recobrança e correção passam a agir pela nova disputa; antiga perde membro | Mensagem para conjunto vazio em `app/chamados/views.py:1434`; teste específico [nao verificado] |
| `app/chamados/views_cobrar.py:160`, `163`, `169`, `175` | Leitor inline: pai aberto ou reverse `perguntas__chamado_destino`, depois perguntas da disputa | Não registra origem | Encontra nova disputa pela pergunta; contagem/espera usam os membros atuais | Teste de reaponte [nao verificado] |
| `app/api/foto_recebida.py:51`, `61`, `73`, `79` | Leitor de FK e reverse; escreve resposta/validação/fechamento via portas, não FK | **Contexto é da disputa atual:** motivo e `contexto.ultima_batida`, 63-68 | Nova disputa governa seleção por foto; se contexto novo não corresponde à pergunta antiga, fluxo de foto pode diferir | Teste dessa combinação [nao verificado] |
| `app/api/views_mensageria.py:539`, `560` | Leitor da fila de validação: serializa FK e colaborador até 562 | Não | Próximas leituras devolvem FK nova; mesma pessoa conserva nome/ID | Teste de FK móvel [nao verificado] |
| `app/chamados/canal.py:64`, `69`, `72`, `77` | Leitor cru de perguntas mudas; cruza LogAuditoria por **colaborador + dia**, não disputa | Não | Cruzamento com fala antiga persiste com mesma pessoa/data; `disputa_id` de saída muda | Teste dessa continuidade [nao verificado] |
| `app/colaboradores/services/ficha.py:166`, `169` | Leitor da fonte do app e agrupamento pelas disputas atuais para respostas sem veredito | Não | Grupos mudam, mesma pergunta continua cruzada por PK | `app/chamados/tests/test_n1_perguntas_do_app.py:138` fixa consumo da fonte |
| `app/colaboradores/services/calendario.py:473` | Leitor por colaborador/dia, exclui `resposta_colab=''`, juiz em 476 | Não | Pergunta **sem resposta**, escopo proposto, já está fora dessa seleção; reaponte isolado não a inclui | Teste de reaponte [nao verificado] |
| `app/ponto/services/dia_do_colab.py:64` | Leitor por colaborador/dia, retorna chamado **da disputa**, 66-68 | Apresenta pai atual como `chamado`; não guarda natal | Diagnóstico do dia passa a citar novo pai, mesmo que `chamado_destino` seja outro | `app/chamados/tests/test_pergunta_no_app.py:145` cobre diagnóstico; troca FK [nao verificado] |
| `app/ponto/supra_juiz.py:543` | Leitor de conversa: perguntas via disputas originadas no chamado e via `perguntas_que_resolvem` | O primeiro caminho associa conversa ao contêiner atual | Sem `respondida_em`, mover a pergunta não altera esse veto; se houver carimbo sem texto, perder o membro pode alterar o primeiro caminho | Condição literal em `app/ponto/supra_juiz.py:545`, `548`; teste de meio-estado [nao verificado] |
| `app/ponto/services/veredito_celula.py:174` | Leitor por colaborador/dia e `q_vivas`; escritor de fechamento da pergunta pela porta `fechar_por_admin`, 177 | Não | Mesmo universo por pessoa/data; não escreve FK | Teste de reaponte [nao verificado] |
| `app/folha/previa.py:64` | Leitor por colaborador/data, agrupa por esses eixos em 67 | Não | Universo e agrupamento conservados com mesma pessoa/data | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/mapa_divergencia.py:270` | Leitor de cobertura por pergunta, colaborador e data | Não | Medida conserva data/pessoa, sem recorte por disputa aberta | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/reconciliar_fantasmas.py:211` | Leitor de diagnóstico: destino + não materializada + disputa aberta; imprime PK→FK em 217 | Não | Pergunta passa a entrar no diagnóstico e imprime nova FK | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/reconciliar_grade.py:460` | Leitor por destino/respondida + `fio_vivo`; não filtra FK | Não | Conserva seleção se destino/carimbos preservados | Teste de reaponte [nao verificado] |
| `app/chamados/relatorios_disputa.py:12` | Leitor/classificador do reverse atual | Quantifica membros atuais, sem origem | Antiga sem perguntas classifica **`SEM_PERGUNTA`**, 13-14; nova passa a contá-la | `app/chamados/tests/test_relatorio_pendencias.py:34`, `:37`, `:41` |
| `app/chamados/relatorios_disputa.py:40` | Leitor/coletor só de disputas abertas; prefetch `perguntas__celula`/destino em 43 | Não exige natal | **Fechada vazia não aparece no relatório de pendências**; não a reabre nem escreve estado | `app/chamados/tests/test_relatorio_pendencias.py:61` fixa ignorar fechadas |
| `bin/censo_folha.py:28` | Leitor/contador cru de perguntas sem resposta em disputas abertas; contador de respondidas em 29 | Equipara “viva” a contêiner aberto + carimbo | `pviva` aumenta com reaponte; `pval` conserva seleção se carimbos não mudam | Teste [nao verificado] |
| `app/chamados/views.py:909` | Contador de **disputas** abertas, não perguntas | Não | Mover FK sem mudar `fechada_em` não altera quantidade de disputas abertas | Teste específico [nao verificado] |

Complementos:

- A migração da pergunta declara apenas FK obrigatória `CASCADE`, `related_name='perguntas'`; opções só rótulos/ordenação, sem unicidade nessa criação (`app/chamados/migrations/0014_perguntadisputa.py:27`, `:30`). Nenhuma migração localizada altera a FK ou cria unique sobre `disputa_id`; confirmação do schema efetivamente instalado [nao verificado].
- Nenhum escritor de FK fora da criação foi localizado no escopo externo; portas externas escrevem resposta, validação ou logs, usando a disputa atual (`app/api/views.py:2642`; `app/chamados/views.py:1281`; `app/api/foto_recebida.py:48`). Ausência global de escritor deve ser consolidada com os outros censos [nao verificado].
- PDF de histórico de chamados lê **campos de chamados e followups**, não `PerguntaDisputa.disputa`: `app/relatorios/pdf_chamados.py:113`, `:137`, `:166`. O histórico de followups continua por chamado; nenhuma transferência dessa trilha resulta da troca isolada da FK nesses leitores.
- PDF de espelho consulta `motor_bloqueado_por_disputa(colab)` (`app/relatorios/pdf_espelho.py:1084`), wrapper de `colaborador_em_disputa` (`app/ponto/services/disputa.py:15`, `:31`); não lê FK diretamente. O efeito final depende do helper em models e deve ser consolidado pelo pai.
- Batida aponta **para a pergunta por PK**, não para disputa (`app/ponto/models.py:40`); procedência relê pergunta pelo PK e autores/carimbos (`app/ponto/services/procedencia_batida.py:65`, `:70`, `:74`). Reaponte isolado não muda esse elo.
- `app/ponto/services/flip_auto.py:130` faz `select_related('disputa')`, mas o laço 131-140 não usa a disputa; não demonstra dependência de pertença.
- Templates recebem listas já montadas: `app/templates/chamados/partials/modal_fio.html:156`, `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/detalhe_local.html:307`; IDs/status/cor da disputa exibidos vêm de `chamado.disputa_obj` (`app/templates/chamados/partials/modal_fio.html:75`, `:124`, `:374`). O acesso direto à FK localizado em template é apenas nome do colaborador no lote (`app/templates/chamados/partials/_lote_preview.html:24`).
- Existe reabertura **humana explícita** de disputa fechada até quando vazia: teste cria disputa fechada sem nenhuma pergunta e POST administrativo a reabre (`app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22`, `:30`, `:35`, `:42`). Portanto “não há leitor automático que reabra” não deve virar “jamais pode reabrir”.
- Nenhum dos testes citados executa troca de `PerguntaDisputa.disputa` no cenário proposto; cobertura específica do reaponte e seus efeitos históricos [nao verificado]. [nao verificado]

## 06/11 19:38 · sessao 498c20dfabdf · - · - · origem=Ronald (tmux)

Censo periférico, exclusivamente leitura:

| Sítio | Papel | Supõe / saída com mudança para relógio |
|---|---|---|
| `app/chamados/views_arquivar.py:36`, `:37` | Escritor indireto: `prazo_padrao` → `silenciar` | Prazo padrão; resposta entrega `ate` em `:48`. |
| `app/chamados/views_arquivar.py:77` | Escritor indireto: `levantar(resgatado=True)` | Limpa silêncio; saída específica para tranca exige `reabrir` antes, em `:64`, `:70`. |
| `app/chamados/views_arquivar.py:124`, `:136`, `:140` | Leitor de verbos + guarda de presença + escritor | Verbos fixam universo; `isnull=True` impede repetir escrita sobre vencido. Com ambas as guardas curadas, lote poderia renovar silêncio vencido. |
| `app/chamados/views_arquivar.py:165`, `:166` | Leitor direto: `Q(isnull=False)`, `F(...).desc()` | Consulta inclui vencidos e via de tranca; presença define acervo consultável, sem excluir fila. Pode continuar assim com HISTÓRICO apenas exibido. |
| `app/chamados/views_arquivar.py:195`, `:196` | Leitor indireto da janela; complemento | Vencido hoje fica no drawer; ao curar janela, desaparece do drawer se voltar à lista. |
| `app/templates/chamados/partials/_busca_arquivados.html:46`, `:49` | Leitor direto de exibição | Já lê relógio: futuro “Arquivado até”; vencido “Histórico”. Não decide fila. |
| `app/templates/chamados/partials/_meus_arquivados.html:33` | Leitor direto de presença | Mostra “Arquivado” mesmo vencido. Relógio retiraria esse rótulo ou permitiria substituí-lo por “Histórico”. |
| `app/templates/chamados/partials/_lista_chamados.html:85` | Leitor direto de presença | Exibe botão Resgatar mesmo vencido. Relógio retiraria botão para vencido. |
| `app/chamados/signals.py:256` | Leitor do nome do campo em `update_fields` | Outro: detecta evento de resgate/escrita; não lê prazo. |
| `app/chamados/signals.py:261` | Leitor direto de presença | Outro: impede recursão/repetição do silêncio de isento. Relógio permitiria prosseguir com vencido, mas `silencio_isentos.alvos` ainda o excluiria. |
| `app/chamados/signals.py:242`, `:269` | Escritores indiretos por `silenciar_do_colab` | Evento de colaborador/chamado alimenta mesma porta de isentos. |
| `app/chamados/services/silencio_isentos.py:30` | Leitor direto, `isnull=True` | Outro: idempotência por presença. Relógio faria expirados virar alvos novamente. |
| `app/chamados/services/silencio_isentos.py:39`, `:43`, `:58` | Escritores indiretos | Padrão → `arquivo.silenciar`; mesma execução para evento e command. |
| `app/chamados/management/commands/silenciar_chamados_isentos.py:36`, `:49` | Leitor/escritor indireto | `--alarme` acusa alvos em `:44–47`; guardas curadas fariam expirados alarmarem; `--apply` renovaria. |
| `app/chamados/management/commands/arquivar_prescritos.py:50`, `:53`, `:93` | Leitor indireto de verbos + direto de presença | Outro: não repete arquivo já marcado. Curar verbos e guardas pode selecionar vencidos para novo silêncio. |
| `app/chamados/management/commands/arquivar_prescritos.py:84`, `:94` | Escritor indireto | Usa prazo padrão. |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:100`, `:274`, `:288` | Leitor de presença + escritor indireto | Evita já marcado; escreve horizonte explícito de dez anos (`:286–287`), independente do prazo padrão. Relógio reabriria seleção de expirados. |
| `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59`, `:88`, `:94` | Leitor de presença + escritor indireto | Evita já marcado; horizonte explícito de cinco anos (`:30`). Relógio reabriria seleção de expirados. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:645` | Leitor direto | (1): somente outro silêncio futuro dispensa marcador de ausência em `:646`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:112` | Leitor indireto `silenciado_em(agora)` | (1): chama predicado temporal do model. |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:46` | Leitor direto `exclude(__gt=agora)` | (1): vencido pode escalar. |

API/serialização e agregações:

- `app/api/views.py:1016` lê janela do colaborador; serialização manual começa em `:1121`, objeto em `:1141`. Cura da janela acrescenta vencidos elegíveis à resposta.
- `app/api/views_mensageria.py:395` publica verbos por chamado; `:453–470` agrupa sinais por verbo; `:1149–1152` seleciona `decidir`. Cura de verbos altera rótulos/grupos e inclui vencidos em decidir conforme natureza.
- `app/api/views_mensageria.py:668`, `:687` publica serviços `fila`; `:1589` publica `fila_por_causa`. Universo desses serviços herda `qs_na_fila`.
- `app/escala/services/furos_diarios.py:212` faz `qs_na_fila(...).values(...).annotate(n=Count('id'))`.
- `app/core/services/painel_op.py:119` faz `qs_na_fila().values(...).annotate(total=Count('id'))`.
- Nenhuma ocorrência literal do campo nem arquivo de serializer dedicado apareceu em `app/api`; não confundir isso com ausência de leitores indiretos [não verificado além dos caminhos rastreados].

Outros consumidores indiretos de verbos:

`app/chamados/views_cobrar.py:77–78`; `app/chamados/services/na_fila.py:19–25`; `app/core/management/commands/lavrar_badge_navbar.py:80`; `app/inteligencia/views.py:251`; `app/inteligencia/management/commands/calcular_scores.py:54`; `app/colaboradores/services/ficha.py:155`; `app/core/context_processors.py:90`; `app/chamados/juizes.py:1287–1290`. `na_fila.py:23` só conta verbos em `VERBOS`, portanto vencidos classificados HISTÓRICO ficam fora.

Crons:

- `app/config/crons.py:363`: `processar_alertas_avancados`, a cada 30 minutos.
- `app/config/crons.py:369`: `escalonar_chamados_supervisao`, a cada cinco minutos.
- `app/config/crons.py:609`: `silenciar_chamados_isentos --alarme`, 07:05.
- `app/config/crons.py:1435`: `arquivar_prescritos` explicitamente desligado.
- `app/config/crons.py:1294–1298`, `:1341–1344`: sem-dia e competência encerrada são atos manuais, excluídos de cron.

Estado além de `silenciado_ate`:

- Não há estado `arquivado` no catálogo `app/chamados/catalogo/motor.py:19–20`; campo `status_local` em `app/chamados/models.py:46`.
- Existe arquivo por competência trancada, independente de silêncio: `via_encerramento` em `app/chamados/models.py:48`, valor `VIA_COMPETENCIA_TRANCADA` em `app/chamados/catalogo/motor.py:92`; `app/chamados/models.py:1569` grava estado terminal e via.
- Escritor chamador: `app/chamados/services/competencia_trancada.py:184`.
- Leitores dessa via: `app/chamados/views_arquivar.py:64` e `:165`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/services/disputa_emissao.py:1572`.
- `resgatado_em` é proteção contra rearquivar, não marca arquivo: `app/chamados/models.py:93–95`, `app/chamados/management/commands/arquivar_prescritos.py:54`; isentos ignoram isso expressamente em `app/chamados/services/silencio_isentos.py:6`.
- `silenciado_por` e `silenciado_motivo` são metadados do véu: `app/chamados/models.py:88–92`.
- Campo `arquivado_em` no model não encontrado; sustentar pela enumeração dos campos `app/chamados/models.py:35–95`, ou marcar ausência global como [não verificado].

## 06/11 19:37 · sessao 335dcc654113 · - · - · origem=Ronald (tmux)

Censo dos arquivos delegados, somente por leitura. As linhas citadas pertencem à cópia; números históricos em comentários não constituem dado de produção verificado. [nao verificado]

| sítio | escritor/leitor | supõe pertença fixa? | efeito do reaponte | teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/chamados/services/fio.py:116` (`chamado_que_lista`) | Lê FK/container como fallback | Não expressamente; usa relação atual | Com destino, a âncora independe da disputa; sem destino, troca para o chamado da nova disputa. `fio.py:117` | `app/chamados/tests/test_fio_ancora_destino.py:72` cobre fallback; mudança de FK [nao verificado]. |
| `app/chamados/services/fio.py:135` (`_disputa_do_chamado`) | Filtra `disputa__fechada_em` e devolve FK | Não; usa relação atual | A pergunta transferida passa a permitir encontrar a disputa nova pelo destino. `fio.py:135-137` | `app/chamados/tests/test_fio_disputa_por_destino.py:21`. |
| `app/chamados/services/fio.py:218` (`montar_chamados_fio`) | Filtra `disputa_id__in`, lê FK e estado | Universo depende da disputa atual | Sai do universo de fechadas, entra no das abertas; deixa de carregar `em_disputa_fechada`; passa ao universo do colab, sujeito à janela. `fio.py:211-240` | `app/chamados/tests/test_fio_le_o_juiz.py:105`, `:115`, `:121` cobrem situação anterior; reaponte [nao verificado]. |
| `app/chamados/services/fio.py:252` | Herda disputa da pergunta no card sem disputa própria | Não expressamente | Flags do card passam a usar disputa nova. `fio.py:250-254` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:392` | Query pai → destinos | Trata relação atual como mapa dos filhos | Filho desaparece do mapa do pai antigo e entra no novo; muda `filhos_vivos` do desenlace. `fio.py:392-403` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:591` | POST validar lê pessoa, disputa e fechamento | Não; valida relação atual | Usa disputa nova e via normal; deixa de executar ramo `sem_reabrir` e log específico de validação em fechada. `fio.py:594-613` | `app/chamados/tests/test_fio_le_o_juiz.py:192` cobre via em fechada. |
| `app/chamados/services/fio.py:629` | Recusar/corrigir exige disputa aberta | Não; autoriza por estado atual | Pergunta transferida torna-se elegível a essa seleção. `fio.py:628-635` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:769` (`_costura_nao_mata_o_fio`) | Lê empresa pela disputa; julga pós-destino | Preserva identidade da pessoa implicitamente | Mesma pessoa mantém empresa; trocar FK não substitui a guarda do destino terminal. A guarda lê destino fresco e, em transação, trava-o. `fio.py:758-780` | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:252`; `app/chamados/tests/test_o261e_f1_codex_v5.py:49`, `:65`. |
| `app/chamados/services/fio.py:831` (`reapontar_resolvedora`) | **Escreve destino, não disputa** | Conserva FK porque não a escreve | Relavra após destino/data e salva junto; não existe transferência de disputa nesta porta. `fio.py:831-848` | `app/chamados/tests/test_porta_reapontamento_pergunta.py:245`, `:289`, `:309`, `:326`. |
| `app/chamados/services/fio.py:931` (`migrar_resolvedoras_vivas`) | Lê perguntas por destino; chama porta acima | Conserva FK no mecanismo atual | Migra resolvedora entre chamados; não constitui precedente de reaponte de disputa. Audita `resolvedoras_migradas` no chamado velho. `fio.py:931-959` | Teste específico de FK [nao verificado]. |
| `app/chamados/juizes.py:72`, `:205` | `dia_do_fato`/lote leem `d.perguntas` | Usa associação atual como evidência do dia do container | Pai antigo perde os dias dessas perguntas; novo ganha. Só atinge degrau fraco quando os superiores não resolvem. `juizes.py:53-77`, `:199-211` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:346`, `:361` | Slot/materialização e completude leem disputa | **Contexto da disputa é insumo vivo**, não fotografia da origem | Mesmo mantendo pessoa, mudar `contexto[motivo]` pode alterar derivação; completude usa contexto como fallback quando falta `data_evento`. `juizes.py:343-371` | Transferência entre contextos [nao verificado]. |
| `app/chamados/juizes.py:664`, `:752`, `:774`, `:932`, `:1200`, `:1528` | Leitores da pessoa/empresa/grade pela FK | Supõem identidade consistente | Mesma pessoa preserva esses insumos; não há exigência de “mesma disputa natal” nesses acessos. `juizes.py:664-666`, `:749-774`, `:931-954`, `:1197-1203`, `:1523-1532` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:811` (`_chave_memo_pv`) | Cache de `pergunta_viva` | **Omissão relevante:** não contém FK nem contexto | Mudar somente disputa na mesma instância pode reaproveitar juízo anterior quando a nova disputa altera insumo de contexto; cache retorna se tupla permanece igual. `juizes.py:811-819`, `:835-847` | Teste específico de troca da FK [nao verificado]. |
| `app/chamados/juizes.py:1021` | Identity map lê disputa e atribui `d.colaborador` | Supõe pk da pessoa constante | Só substitui instância da mesma pessoa; não escreve a FK da pergunta. `juizes.py:1013-1027` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1230`, `:1392` | `perguntas_vivas_de`, `perguntas_corrigiveis` | Associação atual define conjunto | A disputa antiga perde pergunta e a nova ganha nos conjuntos. `juizes.py:1228-1230`, `:1390-1394` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1300`, `:1399` | Universo por célula/fila global, com FK carregada | Não exige origem fixa | Mantendo célula/pessoa/carimbos, seleção principal não muda; juízo continua sujeito aos insumos via disputa. `juizes.py:1300-1302`, `:1399-1403` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1751` | Irmãs respondidas com fallback para pai | Usa container atual como crédito quando destino não está vivo | Resposta futura passa ao pai novo; não ao antigo. A proposta sem resposta não ativa imediatamente esse filtro de respondidas. `juizes.py:1747-1753` | Referências dos selos em `juizes.py:1741-1742`; transferência [nao verificado]. |
| `app/chamados/juizes.py:1948` (`resolvedora_valida`) | Juiz de dia/turno | **Não valida identidade da disputa nem da pessoa** | Mesmo dia permite; ±1 depende do turno do colaborador do chamado; não protege sozinho uma troca indevida da FK. `juizes.py:1963-1999` | `app/chamados/tests/test_selo_resolvedora_do_dia.py:67`, `:81`, `:94`, `:109`, `:146`. |
| `app/chamados/juizes.py:2029` | Datas vivas por `disputa__chamado_id`; competência | Usa associação atual como fatos do container | Antigo perde e novo ganha datas que compõem competência do chamado. `juizes.py:2024-2041`, `:2050-2051` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:2080` | `disputa_viva_do_chamado` | Aberta sem pergunta trava encerramento | **Não olha disputa fechada**: fechada esvaziada não é reaberta por esse leitor; aberta esvaziada seria bloqueadora. `juizes.py:2080-2086` | Reaponte de FK [nao verificado]. |
| `app/chamados/veredito_lavrado.py:162` | Marcos agrupados por `disputa__colaborador_id` | Identidade da pessoa | Mesma pessoa mantém agrupamento; FK não é um marco temporal. `veredito_lavrado.py:162-169` | `app/chamados/tests/test_selo_lavrado_orfaos.py:121`. |
| `app/chamados/veredito_lavrado.py:187` | `esta_stale` lê pessoa da FK | **Mudança da FK não invalida por si** | Só compara relógio/lavra e marcos da pessoa; troca de contexto requer relavra explícita para evitar cache anterior. `veredito_lavrado.py:179-190`; mecanismo de relavra em `fio.py:845-848` | `app/chamados/tests/test_selo_lavrado_orfaos.py:121`; transferência [nao verificado]. |
| `app/chamados/services/historico.py:96` | Histórico seleciona por destino, apenas carrega disputa | Não usa disputa como origem histórica | Trocar só FK preserva seleção; trocar também destino move os carimbos ao histórico do novo chamado. Logs considerados ali são mudanças de estado de chamado, não evento de transferência da pergunta. `historico.py:86-114` | `app/chamados/tests/test_historico.py:130`, `:138`; transferência [nao verificado]. |
| `app/chamados/juizes.py:280` | Testemunha de retratação por `LogAuditoria` | Identidade é pk da pergunta | Preservando pk, mantém testemunha externa; não depende de disputa natal. `juizes.py:277-282` | Transferência de FK [nao verificado]. |

Dois pontos para a consolidação:

- A lista dos escritores protegidos declara `chamado_destino` e `data_evento`, com `models.py` e `fio.py`; essa passagem não declara `disputa_id`. Portanto, não confundir a exclusividade de resolvedora com uma trava já existente para a FK. `app/core/tests/test_contract_escritores_estado.py:48-62`.
- O memo de `data_do_fato_de` retorna `_dfd_memo` sem comparar campos; depois usa o fallback para disputa quando falta destino. Uma instância reapontada após essa primeira leitura merece revisão do memo, além do memo de `pergunta_viva`. `app/chamados/services/fio.py:74-88`, `:116-117`.

## 06/11 19:36 · sessao 4d0acf7e758f · H12H06-B-PAUTA-08 · leitura · origem=Code

**O código atual usa a mesma coluna e a mesma unidade nominal nos dois lados. Isso não comprova que o PAGO tenha sido produzido com o mesmo algoritmo e os mesmos insumos históricos.** O campo é inteiro anulável; a origem histórica dos valores do anexo é **[nao verificado]**. (`app/ponto/models.py:446`)

**1. Origem, unidade e universo de `dias_previstos`**

- **PAGO:** `valores_do_gravado()` busca o `FechamentoMensal` do colaborador/mês/ano e lê seus atributos segundo `CAMPOS`. Não reconstrói os dias que produziram aquele número. (`app/ponto/portas/regen_exportada.py:77–84`)
- **DEVIDO:** monta a grade e conta suas linhas com `tipo_dia` igual a **`trabalho` ou `ausencia`**. Não exige minutos previstos positivos. (`app/ponto/services/fechamento.py:997–998`, `app/ponto/services/fechamento.py:1030–1033`, `app/ponto/services/fechamento.py:1067–1069`)
- **Escritor do fluxo atual:** o recalculador atribui o campo e persiste por `save()` ou `update()`. Em leitura, devolve o atributo sem persistir. Quem efetivamente escreveu os números históricos desses três colaboradores é **[nao verificado]**. (`app/ponto/services/fechamento.py:1068`, `app/ponto/services/fechamento.py:1112–1128`, `app/ponto/services/fechamento.py:1144–1166`)
- **Leitores:** o comando lê os dois dicionários e compara suas chaves; a prontidão da folha também consulta `dias_previstos` no fechamento. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–139`, `app/folha/export.py:1037–1043`)

O universo diário do DEVIDO resulta destas etapas:

| Etapa | Regra |
|---|---|
| Competência | Usa o corte da empresa atual. Corte 21 produz **21/07–20/08** para 08/2026; o corte efetivo dos três casos não consta do anexo: **[nao verificado]**. (`app/ponto/janelas.py:10–21`, `app/ponto/janelas.py:36–57`) |
| Janela da grade | `grade_do_fechamento()` resolve a competência pela empresa e chama `grade_da_celula()`. (`app/folha/export.py:329–338`) |
| Admissão/demissão | A grade aplica as datas atuais do colaborador e fatos da ata para decidir `fora_vinculo`. Aqui, esse nome não significa simplesmente “fora da vigência de EscalaColaborador”. (`app/escala/services/leitor_celula.py:351–376`) |
| Célula e ata | Sem célula: `sem_celula`. Havendo célula: prevalece `ata.tipo_dia`; sem esse valor, usa `cel.trabalha`. (`app/escala/services/leitor_celula.py:377–381`) |
| Contagem | Entram somente `trabalho` e `ausencia`; folga, indefinido, sem célula e fora do vínculo ficam fora. (`app/ponto/services/fechamento.py:1030–1033`) |

Portanto, há **a mesma unidade pretendida — contagem de dias —**, mas a igualdade do universo histórico do PAGO com o universo atual do DEVIDO é **[nao verificado]**. O PAGO contém um agregado; o DEVIDO conta a grade disponível hoje. (`app/ponto/portas/regen_exportada.py:81–84`, `app/ponto/services/fechamento.py:1030–1033`)

**2. Entradas que podem alterar uma competência passada**

| Entrada | Como interfere |
|---|---|
| Célula regenerada/editada ou ata alterada | Pode alterar `trabalha` ou `tipo_dia`, que alimentam diretamente a classificação contada. A regeneração escreve `trabalha`, geradora e DNA. (`app/ponto/portas/celula.py:263–277`, `app/escala/services/leitor_celula.py:380–381`) |
| Vínculo encerrado/novo | Muda a resolução do vínculo do dia. **Isoladamente, não muda o tipo contado quando a ata/célula continua igual**; pode interferir por regeneração posterior ou no fallback de minutos. (`app/escala/alimentacao.py:269–286`, `app/escala/services/leitor_celula.py:380–395`) |
| Escala/template trocado | Pode chegar à contagem por regeneração das células e julgamento posterior. No leitor, consultar a escala para obter minutos não substitui a regra de classificação do tipo. (`app/ponto/portas/celula.py:255–277`, `app/escala/services/leitor_celula.py:380–395`) |
| Admissão/demissão alterada | Interfere diretamente em `fora_vinculo`: dias posteriores à demissão saem; o próprio dia depende de fato lavrado. Antes da admissão, fato lavrado pode preservar o dia. (`app/escala/services/leitor_celula.py:368–378`) |
| Desligamento | O serviço escreve situação/demissão e fecha vínculos ativos. São efeitos distintos sobre os insumos. (`app/colaboradores/services/desligamento.py:103–124`) |
| Empresa/corte alterado | Pode mudar a própria janela lida para a competência passada. (`app/folha/export.py:336–338`, `app/ponto/janelas.py:36–57`) |

Há ainda um filtro de **pessoas**: desligado com demissão anterior ao início da janela, ou sem data de demissão, fica fora do universo. Isso produz ausência do colaborador no retorno; o comando recusa o ato. Não produz `dias_previstos=1`. (`app/ponto/services/fechamento.py:422–433`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135–144`)

**Qual delas levou 16 a 1 no col842: [nao verificado].** Pelo caminho normal desta cópia, o resultado 1 significa exatamente **uma linha da grade classificada como `trabalho`/`ausencia`**. Essa linha pode ter zero minuto previsto, pois contagem e soma são operações separadas. (`app/ponto/services/fechamento.py:1030–1033`)

Também não cabe deduzir que `horas_trabalhadas × 60` deva igualar `minutos_realizados`: estes vêm da grade, com teto por dia, enquanto horas trabalhadas recebem outros componentes, inclusive minutos órfãos de vínculo. (`app/ponto/services/dia_pago.py:384–400`, `app/ponto/services/fechamento.py:893–908`)

**3. Comparação e ESMERIL-SEMANTICA**

O comando compara **24 chaves**, excluindo `mes/ano` das 26 de `CAMPOS`, sem reconciliar universo histórico ou aplicar conversão específica por campo. Entretanto, não compara por igualdade literal. (`app/ponto/management/commands/aplicar_09_corte_b.py:67–74`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`)

`_diferente()` converte `a or 0` e `b or 0` para `float` e exige diferença absoluta **maior que 0,015**. Logo, aplica a mesma tolerância numérica a horas, minutos e contagens, além de equiparar **`None` e zero**. (`app/ponto/portas/regen_exportada.py:248–252`)

Esse último é um problema semântico demonstrável: o recalculador usa `None` para ausência de resposta da grade; zero pode ser uma resposta calculada. O comparador apaga essa distinção. (`app/ponto/services/fechamento.py:1027–1033`, `app/ponto/services/fechamento.py:1071–1083`)

**Campo homônimo comprovadamente com definição diferente entre PAGO e DEVIDO: [nao verificado].** Na implementação atual, ambos recortam os mesmos atributos, e o modo de escrita persiste os atributos calculados. Isso não demonstra a definição usada pelo escritor histórico do PAGO. (`app/ponto/portas/regen_exportada.py:77–84`, `app/ponto/services/fechamento.py:1112–1128`, `app/ponto/services/fechamento.py:1144–1169`)

**4. Sonda de leitura na SOMBRA**

A sonda abaixo mostra **quais insumos atuais produzem a contagem**, os dias incluídos/excluídos, vínculos resolvidos, atas e vestígios de regeneração. Identificar **qual entrada mudou desde o PAGO** depende de evidência histórica correspondente: `dna_anterior` guarda somente a versão imediatamente anterior, sem garantir correspondência com aquele fechamento. (`app/escala/models.py:1714–1729`)

Para colar em um **shell Django já iniciado na SOMBRA**, sem salvar arquivo. A configuração da cópia fixa banco `sombra`, host `sombra_db` e mantém o tenant `juliani`; a disponibilidade desse ambiente é **[nao verificado]**. (`app/config/settings/sombra.py:13–14`, `app/config/settings/sombra.py:29`, `app/config/settings/sombra.py:47–51`)

Usa as funções reais e mantém `lavrar_dias_pagos=False`: essa opção importa porque `somente_leitura=True` combinado com lavratura pode escrever. A execução da sonda e seus resultados permanecem **[nao verificado]**. (`app/ponto/services/fechamento.py:524–531`)

```python
import json
import logging
from collections import Counter
from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

ERROS = {}
# Anexo fornecido; reprodução no banco [nao verificado].
ANEXO = {842: (16, 1), 284: (15, 21), 433: (15, 18)}

def emitir(rotulo, valor):
    print(rotulo, json.dumps(valor, ensure_ascii=False, default=str))

def campos(obj, nomes):
    return {k: getattr(obj, k) for k in nomes}

def dna_resumo(dna):
    if dna is None:
        return None
    return {k: dna.get(k) for k in (
        "dna_versao", "vinculo_id", "tipo_escala_id", "tipo_ciclo", "marcos"
    )}

# Intercepta logging em memória, sem emitir para handlers de arquivo.
# Erros registrados pelo recalculador também entram no resultado final.
def colher_log(self, record):
    if record.levelno >= logging.ERROR:
        ERROS["log_%s" % len(ERROS)] = {
            "arquivo": record.pathname, "linha": record.lineno,
            "funcao": record.funcName,
        }

logging_anterior = logging.Logger.callHandlers
logging.Logger.callHandlers = colher_log
try:
    assert getattr(settings, "HASNER_SOMBRA", False), "settings fora da SOMBRA"
    assert connection.settings_dict["NAME"] == "sombra", "banco inesperado"
    assert connection.settings_dict["HOST"] == "sombra_db", "host inesperado"
    assert not connection.in_atomic_block, "exige transacao nova"

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SHOW transaction_read_only")
            assert cur.fetchone()[0] == "on"
            cur.execute("SELECT current_database(), current_schema()")
            identidade = cur.fetchone()
            assert identidade == ("sombra", "juliani"), identidade
            emitir("AMBIENTE", identidade)

        from colaboradores.models import Colaborador
        from escala.models import EscalaColaborador
        from escala.alimentacao import (
            celulas_do_periodo, escalas_do_periodo,
            vinculo_do_dia, vinculo_cobre_a_data,
        )
        from folha.export import grade_do_fechamento
        from ponto.janelas import janela_fechamento
        from ponto.models import FechamentoMensal
        from ponto.portas.regen_exportada import (
            valores_do_gravado, hash_do_gravado, _diferente,
        )
        from ponto.services.fechamento import (
            recalcular_fechamento_mes, universo_da_competencia,
        )

        for pk in (842, 284, 433):
            try:
                with transaction.atomic():
                    c = Colaborador.objects.select_related("empresa").get(pk=pk)
                    ini, fim = janela_fechamento(8, 2026, c.empresa)
                    h0 = hash_do_gravado(c, 8, 2026)
                    fe = FechamentoMensal.objects.filter(
                        colaborador=c, mes=8, ano=2026
                    ).first()
                    assert fe is not None, "sem PAGO"
                    pago = valores_do_gravado(c, 8, 2026)

                    emitir("CADASTRO", {
                        "col": pk, **campos(c, (
                            "empresa_id", "situacao",
                            "data_admissao", "data_demissao",
                        )),
                        "corte": c.empresa.dia_inicio_competencia,
                        "janela": [ini, fim],
                        "no_universo": universo_da_competencia(8, 2026)
                            .filter(pk=pk).exists(),
                        "carimbos_fechamento": campos(fe, (
                            "previsto_em", "derivado_em", "atualizado_em",
                        )),
                    })

                    cels = celulas_do_periodo([pk], ini, fim)
                    geradoras = {
                        v.escala_geradora_id for v in cels.values()
                        if v.escala_geradora_id
                    }
                    selecionados = sorted(
                        escalas_do_periodo(pk, ini, fim, mais=geradoras),
                        key=lambda e: e.data_inicio,
                    )
                    todos = list(
                        EscalaColaborador.objects.filter(colaborador=c)
                        .select_related("tipo_escala")
                        .order_by("data_inicio", "pk")
                    )
                    emitir("VINCULOS", {
                        "col": pk,
                        "selecionados": [e.pk for e in selecionados],
                        "todos": [{
                            **campos(e, (
                                "pk", "data_inicio", "data_fim", "ativa",
                                "tipo_escala_id", "posto_id",
                                "data_ancora_colaborador",
                                "trabalha_em_feriado",
                            )),
                            "ciclo_atual": e.tipo_escala.tipo_ciclo,
                        } for e in todos],
                    })

                    # folha/export.py:329-338; fechamento.py:997-998.
                    grade = grade_do_fechamento(fe)
                    dias = sorted(grade.get("dias") or [], key=lambda d: d["data"])
                    emitir("TIPOS", {
                        "col": pk,
                        "contagens": dict(Counter(d["tipo_dia"] for d in dias)),
                    })

                    for d in dias:
                        cel = cels.get((pk, d["data"]))
                        ata = (cel.ata or {}) if cel else {}
                        ec = vinculo_do_dia(
                            pk, d["data"], selecionados, celulas=cels
                        )
                        emitir("DIA", {
                            "col": pk, "data": d["data"],
                            **{k: d.get(k) for k in (
                                "tipo_dia", "minutos_previstos",
                                "minutos_realizados", "sem_ancora",
                            )},
                            "conta": d.get("tipo_dia") in ("trabalho", "ausencia"),
                            "cobrem_por_cadastro": [
                                e.pk for e in todos
                                if vinculo_cobre_a_data(e, d["data"])
                            ],
                            "vinculo_resolvido": ec.pk if ec else None,
                            "celula": {
                                **campos(cel, (
                                    "pk", "trabalha", "origem",
                                    "escala_geradora_id", "gerada_em",
                                    "regenerada_em", "regeneracoes",
                                    "editada_em", "julgada_em",
                                )),
                                "dna": dna_resumo(cel.dna),
                                "dna_anterior": dna_resumo(cel.dna_anterior),
                            } if cel else None,
                            "ata": {k: ata.get(k) for k in (
                                "tipo_dia", "minutos_previstos",
                                "minutos_realizados", "n_celulas", "n_missing",
                            )},
                        })

                    # Mesma chamada do comando: pauta...py:135.
                    retorno = recalcular_fechamento_mes(
                        8, 2026, colaborador_ids=[pk],
                        somente_leitura=True, lavrar_dias_pagos=False,
                    )
                    assert pk in retorno, "recalculo omitiu colaborador"
                    dev = retorno[pk]

                    # Conferência: fechamento.py:1027-1033,1071-1083.
                    dentro = [
                        d for d in dias if d.get("tipo_dia") != "fora_vinculo"
                    ]
                    sem_resposta = bool(dentro) and all(
                        d.get("tipo_dia") == "sem_celula" for d in dentro
                    )
                    contagem = sum(
                        d.get("tipo_dia") in ("trabalho", "ausencia")
                        for d in dias
                    )
                    esperado = None if sem_resposta else contagem
                    assert dev["dias_previstos"] == esperado, (
                        "DEVIDO diverge da grade lida separadamente"
                    )

                    emitir("COMPARACAO", {
                        "col": pk, "pago": pago, "devido": dev,
                        "dias_no_anexo": ANEXO[pk],
                        "reproduziu_dias_anexo": (
                            pago["dias_previstos"], dev["dias_previstos"]
                        ) == ANEXO[pk],
                        "campos_pauta": [
                            k for k in pago if k not in ("mes", "ano")
                            and _diferente(pago[k], dev.get(k))
                        ],
                        "mudanca_none": [
                            k for k in pago if k not in ("mes", "ano")
                            and ((pago[k] is None) != (dev.get(k) is None))
                        ],
                    })
                    assert hash_do_gravado(c, 8, 2026) == h0, "hash mudou"

            except Exception as exc:
                ERROS["col%s" % pk] = {
                    "tipo": type(exc).__name__, "erro": str(exc),
                }

        transaction.set_rollback(True)

except Exception as exc:
    ERROS["sonda"] = {"tipo": type(exc).__name__, "erro": str(exc)}
finally:
    logging.Logger.callHandlers = logging_anterior
    emitir("ERROS", ERROS)
```

A última linha imprime `ERROS {}` quando nenhuma falha foi capturada; se houver falhas, imprime o dicionário correspondente, sem ocultá-las. **Resultado dessa execução: [nao verificado].**

## 06/11 19:33 · sessao 85381df901b3 · - · - · origem=Ronald (tmux)

A sonda pode explicar **qual entrada atual produz 1/21/18**, mas identificar **qual entrada mudou desde o PAGO** exige uma versão histórica vinculada à gravação. `dna_anterior` guarda apenas a versão imediatamente anterior, sem equivalência garantida com o PAGO (`app/escala/models.py:1714-1717`).

Referências essenciais:

- Modo correto: `somente_leitura=True, lavrar_dias_pagos=False`; a combinação com `lavrar_dias_pagos=True` escreve (`app/ponto/services/fechamento.py:524-531`).
- Retorno canônico: `dict` dos campos de `CAMPOS` (`app/ponto/services/fechamento.py:1112-1120`).
- Grade real: `folha.export.grade_do_fechamento` chama `grade_da_celula`, na janela da empresa (`app/folha/export.py:329-338`).
- O contador depende de `tipo_dia in ('trabalho','ausencia')`, inclusive com minutos previstos zero (`app/ponto/services/fechamento.py:1030-1033`).
- O cadastro de admissão/demissão determina `fora_vinculo`; os fatos lavrados interferem nessa decisão (`app/escala/services/leitor_celula.py:351-376`).
- `ata.tipo_dia` prevalece; sem ele, entra `cel.trabalha` (`app/escala/services/leitor_celula.py:380-381`).
- O vínculo interfere no fallback de **minutos** quando a ata não tem `minutos_previstos`, através da função real `minutos_previstos_do_dia` (`app/escala/services/leitor_celula.py:382-395`).
- Essa função devolve zero sem vínculo vigente ou sem marcos completos (`app/escala/utils.py:2251-2255`, `2281-2292`).
- A rotina pode engolir erro da grade e devolver campos `None`, e pode engolir erro do colaborador omitindo seu retorno; a sonda precisa verificar ambos (`app/ponto/services/fechamento.py:1071-1083`, `1287-1294`).

**Não usar `bin/sonda_leitura.sh` para cumprir “nenhum arquivo escrito”**: ele reescreve `logs/.env_leitor`, configura role/GRANT e pode iniciar a sombra (`bin/sonda_leitura.sh:103-104`, `121-143`, `157-159`).

Segue núcleo para shell Django **já iniciado** na SOMBRA. A configuração fixa banco `sombra`, host `sombra_db`; o schema continua `juliani` (`app/config/settings/sombra.py:13-14`, `29`, `47-51`). Não executei.

```python
# Executar como texto em shell SOMBRA existente; não salvar arquivo.
import json
import logging
from collections import Counter
from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

ERROS = {}

def emitir(rotulo, valor):
    print(rotulo, json.dumps(valor, ensure_ascii=False, default=str))

def campos(obj, nomes):
    return {nome: getattr(obj, nome) for nome in nomes}

# Evita handlers de logging escreverem arquivos durante a sonda.
_logging_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)

try:
    cfg = connection.settings_dict
    assert getattr(settings, "HASNER_SOMBRA", False)
    assert cfg["NAME"] == "sombra"
    assert cfg["HOST"] == "sombra_db"
    assert not connection.in_atomic_block

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SHOW transaction_read_only")
            assert cursor.fetchone()[0] == "on"

        from colaboradores.models import Colaborador
        from escala.models import EscalaColaborador
        from escala.alimentacao import (
            celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
        )
        from folha.export import grade_do_fechamento
        from ponto.janelas import janela_fechamento
        from ponto.models import FechamentoMensal
        from ponto.portas.regen_exportada import (
            valores_do_gravado, hash_do_gravado,
        )
        from ponto.services.fechamento import (
            recalcular_fechamento_mes, universo_da_competencia,
        )

        for pk in (842, 284, 433):
            try:
                with transaction.atomic():
                    c = Colaborador.objects.select_related("empresa").get(pk=pk)
                    ini, fim = janela_fechamento(8, 2026, c.empresa)
                    antes = hash_do_gravado(c, 8, 2026)
                    fe = FechamentoMensal.objects.filter(
                        colaborador=c, mes=8, ano=2026
                    ).first()
                    assert fe is not None, "sem FechamentoMensal"

                    emitir("COLAB", {
                        "id": pk,
                        **campos(c, (
                            "empresa_id", "situacao",
                            "data_admissao", "data_demissao",
                        )),
                        "janela": [ini, fim],
                        "dia_inicio_competencia":
                            c.empresa.dia_inicio_competencia,
                        "no_universo":
                            universo_da_competencia(8, 2026)
                            .filter(pk=pk).exists(),
                    })
                    emitir("PAGO", {
                        "id": pk,
                        "valores": valores_do_gravado(c, 8, 2026),
                        "carimbos": campos(fe, (
                            "previsto_em", "derivado_em", "atualizado_em",
                        )),
                        "hash": antes,
                    })

                    celulas = celulas_do_periodo([pk], ini, fim)
                    geradoras = {
                        cel.escala_geradora_id
                        for cel in celulas.values()
                        if cel.escala_geradora_id
                    }
                    selecionadas = escalas_do_periodo(
                        pk, ini, fim, mais=geradoras
                    )
                    emitir("VINCULOS", {
                        "id": pk,
                        "selecionados": [e.pk for e in selecionadas],
                        "todos": list(
                            EscalaColaborador.objects.filter(colaborador=c)
                            .order_by("data_inicio", "pk").values(
                                "id", "data_inicio", "data_fim", "ativa",
                                "tipo_escala_id", "posto_id", "regua_classe",
                            )
                        ),
                    })

                    # Mesma função que a derivação usa; não remonta a grade.
                    grade = grade_do_fechamento(fe)
                    dias = sorted(
                        grade.get("dias") or [], key=lambda d: d["data"]
                    )
                    emitir("TIPOS_GRADE", {
                        "id": pk,
                        "tipos": dict(Counter(d["tipo_dia"] for d in dias)),
                    })

                    for dia in dias:
                        data = dia["data"]
                        cel = celulas.get((pk, data))
                        ec = vinculo_do_dia(
                            pk, data, selecionadas, celulas=celulas
                        )
                        ata = (cel.ata or {}) if cel else {}
                        emitir("DIA", {
                            "id": pk,
                            "data": data,
                            "tipo_grade": dia.get("tipo_dia"),
                            "conta_dias_previstos":
                                dia.get("tipo_dia") in ("trabalho", "ausencia"),
                            "previsto_grade": dia.get("minutos_previstos"),
                            "realizado_grade": dia.get("minutos_realizados"),
                            "vinculo_resolvido": ec.pk if ec else None,
                            "celula": campos(cel, (
                                "pk", "trabalha", "origem",
                                "escala_geradora_id", "gerada_em",
                                "regenerada_em", "regeneracoes",
                                "editada_em", "julgada_em",
                                "dna", "dna_anterior",
                            )) if cel else None,
                            "ata": {
                                k: ata.get(k) for k in (
                                    "tipo_dia", "minutos_previstos",
                                    "minutos_realizados", "n_celulas",
                                    "n_missing", "sem_ancora",
                                )
                            },
                        })

                    retorno = recalcular_fechamento_mes(
                        8, 2026, colaborador_ids=[pk],
                        somente_leitura=True, lavrar_dias_pagos=False,
                    )
                    assert pk in retorno, "recalculo omitiu colaborador"
                    dev = retorno[pk]
                    emitir("DEVIDO", {"id": pk, "valores": dev})

                    dentro = [
                        d for d in dias
                        if d.get("tipo_dia") != "fora_vinculo"
                    ]
                    sem_resposta = bool(dentro) and all(
                        d.get("tipo_dia") == "sem_celula" for d in dentro
                    )
                    esperado = None if sem_resposta else sum(
                        d.get("tipo_dia") in ("trabalho", "ausencia")
                        for d in dias
                    )
                    assert dev["dias_previstos"] == esperado, (
                        "dias_previstos diverge da grade independente"
                    )
                    assert hash_do_gravado(c, 8, 2026) == antes, (
                        "gravado mudou"
                    )
            except Exception as exc:
                ERROS["col%s" % pk] = {
                    "tipo": type(exc).__name__, "erro": str(exc),
                }
        transaction.set_rollback(True)
except Exception as exc:
    ERROS["sonda"] = {
        "tipo": type(exc).__name__, "erro": str(exc),
    }
finally:
    logging.disable(_logging_anterior)
    emitir("ERROS", ERROS)
```

`ERROS {}` só será impresso se não houver falhas. A sonda não atribui causalidade histórica por aproximação.

## 06/11 19:32 · sessao e88d81ca5b20 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos somente por leitura:

- O PAGO lê atributos do `FechamentoMensal` gravado, sem transformar valores: `app/ponto/portas/regen_exportada.py:77-84`. O DEVIDO entrega os mesmos atributos do objeto recalculado, recortados pelo mesmo `CAMPOS`: `app/ponto/services/fechamento.py:1112-1120`. São 26 chaves, incluindo `mes/ano`: `app/ponto/management/commands/aplicar_09_corte_b.py:67-74`.
- O comando compara 24 dessas chaves, excluindo `mes/ano`, sem reconciliar universo histórico nem converter unidades específicas de cada campo: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`. Contudo, não é igualdade literal: `_diferente` converte `a or 0` e `b or 0` para `float`, usando tolerância absoluta `> 0.015`; na exceção compara `(a or None) != (b or None)`: `app/ponto/portas/regen_exportada.py:248-252`. Assim, `None` e zero são equiparados.
- Essa equiparação perde distinção real: a derivação grava `None` quando não consegue obter resposta da grade ou todos os dias internos são `sem_celula`; zero pode ser resposta da grade: `app/ponto/services/fechamento.py:1027-1033`, `app/ponto/services/fechamento.py:1071-1083`.
- `_fmt` só apresenta valores com até quatro casas; não corrige semântica ou universo: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:45-50`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:176-177`.
- No código atual, não achei prova de que algum campo homônimo tenha definição diferente entre os dois lados: a derivação escreve os mesmos atributos que o modo de escrita persiste: `app/ponto/services/fechamento.py:1067-1087`, `app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1169`. Sem histórico de entrada/escrita, a semântica efetivamente usada quando o PAGO nasceu permanece **[nao verificado]**.
- `dias_previstos` conta **linhas da grade cujo `tipo_dia` é `trabalho` ou `ausencia`**, mesmo que tenham zero minutos: `app/ponto/services/fechamento.py:1030-1033`. A grade é lida na janela da competência: `app/folha/export.py:329-338`. Não é contagem de batidas, marcos ou turnos.
- O tipo vem prioritariamente de `ata.tipo_dia`, depois de `cel.trabalha`; os minutos vêm de `ata.minutos_previstos`, e só na ausência dessa chave consultam a autoridade atual: `app/escala/services/leitor_celula.py:380-395`. A grade aplica admissão/demissão atuais, com exceções por fato lavrado: `app/escala/services/leitor_celula.py:351`, `app/escala/services/leitor_celula.py:368-378`. Portanto, encerrar ou criar vínculo de escala não implica, sozinho, alterar a contagem se a célula/ata e o envelope de emprego continuarem iguais.
- Para col842, **condicionalmente a esse caminho normal**, `dias_previstos=1` significa exatamente uma linha contável; `minutos_previstos=0` não impede essa contagem: `app/ponto/services/fechamento.py:1030-1033`. Qual linha e qual mudança fizeram isso: **[nao verificado]**.
- Evitar supor `horas_trabalhadas × 60 == minutos_realizados`: estes últimos vêm da grade, com teto por dia e universo `trabalho/ausencia`: `app/ponto/services/dia_pago.py:384-400`, `app/ponto/services/fechamento.py:1044-1048`. `horas_trabalhadas` inclui minutos órfãos de vínculo e folga trabalhada sem escala certa: `app/ponto/services/fechamento.py:893-908`, `app/ponto/services/fechamento.py:964-969`. O próprio modelo distingue relógio da grade de hora líquida da folha: `app/ponto/models.py:440-445`.
- Leitor de `dias_previstos` além do comando: `prontidao` verifica o snapshot e usa o campo para detectar denominador zero: `app/folha/export.py:1037-1043`; no fallback reconta o mesmo universo `trabalho/ausencia`: `app/folha/export.py:1087-1091`.
- DSR vem do resultado do motor, e não da contagem da grade: `app/ponto/services/fechamento.py:1085-1087`; na composição por fatias, semanas e reflexo são somados: `app/ponto/services/fechamento.py:98-99`. Não confundir aumento de `semanas_dsr_ok` com prova de aumento dos mesmos dias contados em `dias_previstos`.

## 06/11 19:32 · sessao 192804a2c8ca · - · - · origem=Ronald (tmux)

Achados com referências da cópia:

- **PAGO** é o valor atual de `FechamentoMensal.dias_previstos`: `valores_do_gravado` consulta colaborador/mês/ano e devolve os atributos definidos em `CAMPOS`, sem recalcular nem transformar (`app/ponto/portas/regen_exportada.py:77-84`). O campo é inteiro anulável, descrito como “Dias de trabalho previstos” (`app/ponto/models.py:446`).
- **DEVIDO** atribui `dias_previstos = len(_dt_trab)`, com `_dt_trab` sendo os dias da grade cujo `tipo_dia` é `trabalho` ou `ausencia`; não exige minutos positivos (`app/ponto/services/fechamento.py:1030-1034`, `app/ponto/services/fechamento.py:1067-1069`).
- A grade vem da função real `grade_do_fechamento`, que resolve a janela pela empresa atual e chama `grade_da_celula` (`app/folha/export.py:329-338`). O corte é lido de `empresa.dia_inicio_competencia`, com default 21 e clamp 2–28 (`app/ponto/janelas.py:36-57`); corte 21 produz 21 do mês anterior a 20 do mês da competência (`app/ponto/janelas.py:10-21`). Corte efetivo histórico dos três colabs: **[nao verificado]**.
- O universo diário da contagem é decidido por `grade_da_celula`: primeiro admissão/demissão atuais e fatos da ata determinam `fora_vinculo`; depois ausência de célula vira `sem_celula`; havendo célula, prevalece `ata.tipo_dia`, com fallback para `cel.trabalha` (`app/escala/services/leitor_celula.py:350-381`). `fora_vinculo` aqui usa datas de vida do colaborador; não corresponde simplesmente à vigência de `EscalaColaborador` (`app/escala/services/leitor_celula.py:351-381`).
- A demissão exclui dias posteriores sempre; no próprio dia exclui quando não há fato na ata. Antes da admissão, fato na ata pode preservar o dia (`app/escala/services/leitor_celula.py:368-376`).
- A alteração isolada de `EscalaColaborador` não aparece na seleção de `tipo_dia` que alimenta a contagem; a escala entra no fallback de **minutos**, quando a ata não os contém (`app/escala/services/leitor_celula.py:380-395`). Troca/encerramento de vínculo pode mudar a contagem por alterações subsequentes nas células/atas; atribuir isso aos três casos é **[nao verificado]**.
- Regeneração efetivamente altera `cel.trabalha`, geradora e DNA, preservando apenas o DNA imediatamente anterior, com carimbo/contador (`app/ponto/portas/celula.py:263-277`). A regeneração normalmente pula competência lavrada (`app/ponto/portas/celula.py:250-254`); existe abertura por ato com motivo (`app/ponto/portas/celula.py:197-210`). Ter ocorrido abertura nesses casos: **[nao verificado]**.
- A unidade pretendida é contagem de dias nos dois lados, e o escritor atual persiste exatamente a mesma derivação: `save()` quando aberto; `update(... dias_previstos=...)` nos demais estados (`app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1166`). A versão do algoritmo e as entradas que produziram o PAGO histórico não constam do anexo: **[nao verificado]**.
- Uma grade com todos os dias internos `sem_celula` produz `None`, não zero nem 1; cobertura parcial não aciona essa abstenção (`app/ponto/services/fechamento.py:1027-1029`, `app/ponto/services/fechamento.py:1071-1083`).
- Portanto, **o caminho normal que devolveu 1 contou exatamente um dia `trabalho`/`ausencia`, mesmo com minutos previstos zero** (`app/ponto/services/fechamento.py:1030-1033`). Qual entrada histórica mudou de 16 para 1 é **[nao verificado]**. O anexo não permite escolher entre mudança nas datas de vida, tipos nas atas/células, cobertura de células, janela empresarial ou algoritmo histórico.
- O universo de pessoas é outra etapa: desligados entram somente com demissão não anterior ao início da janela; desligado com demissão nula fica fora (`app/ponto/services/fechamento.py:422-433`). O filtro `colaborador_ids` apenas restringe esse universo (`app/ponto/services/fechamento.py:545-548`). Estar fora não gera `dias_previstos=1`: gera ausência do retorno, e o comando recusa o ato (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135-144`).
- Leitor adicional do campo: prontidão utiliza `dias_previstos` junto a `minutos_previstos` quando aceita o snapshot (`app/folha/export.py:1037-1043`).

## 06/11 19:27 · sessao 23b336eb4620 · OTIMIZA-3 · verificador · origem=Code

**REPROVA: a classe “alvo carregado por loader continua com zero” permanece aberta.** Há contraexemplos que deixam **arquivo e símbolo com zero**, sem sequer registrar opacidade. O contrato trata esses zeros como afirmações. (`bin/quem_chama.py:8`, `bin/quem_chama.py:554`, `bin/quem_chama.py:684`, `bin/quem_chama.py:703`, `bin/quem_chama.py:1113`)

As reproduções abaixo foram feitas em memória. Execução integral do selo, histórico Git e comportamento em produção: **[não verificado]**.

**1. BLOQUEIA — P1(h): o valor completo montado é calculado, mas descartado.**

Para um alvo hipotético `app/pkg/mod.py::f`, estes contraexemplos deixam **ambas as facetas com zero**:

```python
importlib.import_module('pkg.' + 'mod').f()
runpy.run_path('app/pkg/mo' + 'd.py')
```

`_prefixo` calcula o resultado completo e devolve `inteiro=True`. O consumidor de caminho apenas marca `literal=True`; o de nome só acrescenta nomes completos nos casos relativo/fromlist. Depois, a recuperação lexical procura um literal completo que essas expressões não contêm. (`bin/quem_chama.py:554`, `bin/quem_chama.py:684`, `bin/quem_chama.py:703`, `bin/quem_chama.py:775`, `bin/quem_chama.py:779`)

O comportamento das bordas pedidas é:

| Prefixo | Resultado |
|---|---|
| Vazio | Os produtores descartam; nome vira opaco, sem proteger o alvo. Chamar diretamente `sob_prefixo('nome', '')` ligaria todos; caminho vazio não liga nenhum. (`bin/quem_chama.py:507`, `bin/quem_chama.py:509`, `bin/quem_chama.py:686`, `bin/quem_chama.py:695`) |
| Um caractere | É aceito. Nome `'a'` alcança todos os módulos pela alternativa `'app.' + modulo`; caminho `'a'` alcança todos os caminhos iniciados por `app`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:508`, `bin/quem_chama.py:513`) |
| Sem separador final | É prefixo textual: `'pkg'` também alcança `pkgx`; `'app/pkg'` também alcança `app/pkgx`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:513`) |
| Com separador final | `'pkg.'` delimita o nome; `partes.append('')` preserva a barra de `'app/pkg/'`, excluindo `app/pkgx/`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:510`) |

Há outro falso zero concreto:

```python
APP_DIR = 'app'
leaf = 'mod' + '.py'
runpy.run_path(os.path.join(APP_DIR, 'pkg', leaf))
```

A raiz variável é pulada, produzindo `pkg/`. Esse prefixo não corresponde ao caminho armazenado `app/pkg/mod.py`; o sítio é considerado resolvido e não entra em OPACO. (`bin/quem_chama.py:569`, `bin/quem_chama.py:572`, `bin/quem_chama.py:513`, `bin/quem_chama.py:686`)

**2. BLOQUEIA — P1(a,b): `getattr` imediato foi corrigido; aliases atribuídos continuam invisíveis.**

Funcionam o `getattr(importlib, 'import_module')(literal)`, o apelido de `from importlib import import_module as im` e atributos como `il.import_module`. A identificação usa o `getattr` imediato, os `ImportFrom` e o último atributo. (`bin/quem_chama.py:600`, `bin/quem_chama.py:602`, `bin/quem_chama.py:607`)

Este contraexemplo mantém o símbolo com zero e não registra OPACO:

```python
im = getattr(importlib, 'import_module')
im('pkg.mod').f()
```

O mesmo ocorre com `g = getattr; g(importlib, 'import_module')('pkg.mod')`: atribuições de funções não alimentam o mapa de apelidos. (`bin/quem_chama.py:600`, `bin/quem_chama.py:604`, `bin/quem_chama.py:610`)

O prefixo literal `'app.'` foi acrescentado às chaves de loader, às regex textuais e aos pontos de entrada. Portanto `import_module('app.pkg.mod')` passa a ligar `app/pkg/mod.py`. (`bin/quem_chama.py:450`, `bin/quem_chama.py:451`, `bin/quem_chama.py:455`, `bin/quem_chama.py:780`)

Entretanto, o leitor de imports AST conserva a comparação antiga. O contraexemplo `import app.pkg.mod as m; m.f()` não produz achado para esse alvo: os dois ramos de import comparam somente `alvo['modulo']`, sem consultar as novas chaves. (`bin/quem_chama.py:747`, `bin/quem_chama.py:753`)

**3. BLOQUEIA — P1(c,d): builtin e subprocesso têm lacunas que o aviso textual não cobre.**

| Mecanismo | Liga | Deixa escapar |
|---|---|---|
| `exec`/`compile` AST | Nomes simples e aliases de `ImportFrom`; caminhos literais ou prefixos reconhecidos. (`bin/quem_chama.py:106`, `bin/quem_chama.py:612`, `bin/quem_chama.py:618`) | `builtins.exec` e `builtins.compile`: qualquer atributo com esses nomes é excluído, junto com `re.compile`. (`bin/quem_chama.py:603`) |
| Texto `.sh/config/string` | Os loaders de `LOADERS_CAMINHO`; o conjunto expandido do AST não alimenta a regex textual. (`bin/quem_chama.py:111`, `bin/quem_chama.py:487`) | Python embutido usando `exec(open(...).read())`. (`bin/quem_chama.py:487`, `bin/quem_chama.py:877`) |
| Python por subprocesso | A presença de string reconhecida por `_RX_PYTHON` ou da cadeia exata `sys.executable` ativa a associação lexical de caminhos do arquivo. (`bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:775`) | Não examina a expressão do argumento do script nem resolve aliases de `sys`. (`bin/quem_chama.py:666`, `bin/quem_chama.py:674`) |

Contraexemplos reproduzidos:

```python
builtins.exec(open('app/pkg/mod.py').read(), G)
G['f']()                              # símbolo zero; OPACO vazio

subprocess.run(['python3', 'app/pkg/mo' + x])
# x == 'd.py': arquivo e símbolo zero; OPACO vazio

import sys as s
subprocess.run([s.executable, 'app/pkg/mod.py'])
# símbolo zero; OPACO vazio
```

Os resultados decorrem, respectivamente, da exclusão de atributos builtin, da ausência de análise dos argumentos de subprocesso e da exigência da cadeia exata `sys.executable`. (`bin/quem_chama.py:603`, `bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:682`)

Também permanece zero para o símbolo no shell:

```sh
python3 -c "exec(open('app/pkg/mod.py').read())"
```

A citação protege o arquivo; não há marcador LOADER nem import reconhecido para proteger o símbolo. Isso é execução Python embutida, embora `exec cat arquivo.py` de shell deva continuar excluído. (`bin/quem_chama.py:111`, `bin/quem_chama.py:872`, `bin/quem_chama.py:875`, `bin/quem_chama.py:877`)

**4. BLOQUEIA — P1(e): o ponto fixo exclui arquivos necessários antes de começar.**

O invólucro externo de um nível da fixture funciona. Porém `cand` exige que o arquivo contenha um token de loader conhecido **antes** da descoberta de invólucros. Um segundo arquivo que apenas chama o primeiro fica fora das três rodadas. (`bin/tests/test_quem_chama.sh:308`, `bin/tests/test_quem_chama.sh:318`, `bin/quem_chama.py:1038`, `bin/quem_chama.py:1041`)

Contraexemplo:

```python
# one.py
def one(path):
    return run_path(path)

# two.py
from one import one
def two(path):
    return one(path)

# caller.py
two('app/pkg/mod.py')
```

O símbolo do alvo continua com zero. Também escapam o alias `from one import one as x; x(caminho)` e uma cadeia de quatro níveis, mesmo fazendo todos os arquivos passarem pelo filtro textual. (`bin/quem_chama.py:612`, `bin/quem_chama.py:1039`, `bin/quem_chama.py:1041`)

Além disso, o retorno guarda apenas `nome_curto → tipo`. Duas funções `load`, em arquivos diferentes, uma de caminho e outra de nome, colidem em `novo_.update`; o chamador pode receber o tipo errado e manter zero. (`bin/quem_chama.py:639`, `bin/quem_chama.py:1044`)

**5. BLOQUEIA — P1(f,g): atribuições e relativos não compõem suas resoluções.**

`_valores` acompanha somente `Assign` cujo destino seja `Name`, por até dois saltos. Não acompanha `AnnAssign`, atributos, desempacotamento nem valores internos de expressões. O `package=` relativo não passa por `_valores`. (`bin/quem_chama.py:653`, `bin/quem_chama.py:658`, `bin/quem_chama.py:714`)

Contraexemplos com alvo efetivamente carregado e facetas ainda zeradas:

```python
a = 'pkg.' + input()
b = a
c = b
import_module(c)                     # terceiro salto

x: str = 'pkg.' + input()
import_module(x)                     # AnnAssign

import_module('.' + leaf, package='pkg')
import_module('.mo' + suffix, package='pkg')
```

No primeiro relativo, o prefixo só de pontos é descartado e o sítio vira opaco; no segundo, `.mo` vira prefixo absoluto que não encontra `pkg.mod`, sem opacidade. O pacote só é aplicado quando o nome já foi considerado inteiro. (`bin/quem_chama.py:695`, `bin/quem_chama.py:700`, `bin/quem_chama.py:713`)

`import_module('.mod', package='pkg')` literal funciona no AST e agora também no texto. A regex textual exige a grafia `import_module(...)` e pacote literal; aliases textuais continuam fora desse reconhecimento. (`bin/quem_chama.py:113`, `bin/quem_chama.py:499`, `bin/quem_chama.py:714`)

**6. BLOQUEIA — P1(i), P2: OPACO é aviso global, não proteção contra falso zero.**

Contraexemplo entre arquivos:

```python
# tabela.py
NOME = 'pkg.mod'

# caller.py
from tabela import NOME
importlib.import_module(NOME).f()
```

O loader aparece no cabeçalho, mas não fica ligado à faceta de `f`; ela continua varrida e zerada. `opacos` é anexado como metadado, enquanto apenas os conjuntos `loader` e `embutido` alteram tipos e achados. (`bin/quem_chama.py:701`, `bin/quem_chama.py:1085`, `bin/quem_chama.py:1095`, `bin/quem_chama.py:1113`, `bin/quem_chama.py:1129`)

O censo da cópia encontrou **31 sítios únicos emitidos** em `bin/**` e `app/**`, aplicando a descoberta de invólucros e a deduplicação do render: (`bin/quem_chama.py:1041`, `bin/quem_chama.py:1054`, `bin/quem_chama.py:1129`)

```text
app/api/tests/test_me_posto_geo.py:113,126
app/chamados/tests/test_contract_crons.py:22
app/chamados/tests/test_o137_esmeril_chamado.py:160
app/chamados/tests/test_selo_minimo_modulos.py:54
app/core/erros.py:51
app/core/intake.py:487,494
app/core/management/commands/describe.py:80
app/core/tests/test_contract_direcao_a16.py:157
app/core/tests/test_contract_import_local_resolve.py:75,86
app/core/tests/test_marca_cron15.py:64
app/core/tests/test_selo_contratos_estruturais.py:24,76,86,93,113,116,126,179,202,204,207
app/core/tests/test_selo_estrutural.py:28,31
app/escala/tests/test_selo_grade_pede_o_instante.py:207
app/ponto/tests/test_a4_estados_com_porta.py:191
bin/leitor_dia_estado.py:32
bin/prova_de_casca.py:86
bin/recusa_estado.py:31
```

Esse número **não significa 31 falsos zeros**. Há sítios classificados como opacos apesar de o próprio arquivo conter literais suficientes para ligar seus alvos, como `bin/leitor_dia_estado.py:25` e `bin/prova_de_casca.py:68`. O cabeçalho diz “sem literal no arquivo”, mas o cálculo considera a expressão do argumento. (`bin/quem_chama.py:682`, `bin/quem_chama.py:695`, `bin/quem_chama.py:1130`)

Remover somente a exceção que silencia `exec/compile` acrescenta **quatro sítios**, levando esse censo operacional a **35**: (`bin/quem_chama.py:689`)

```text
app/core/tests/test_access_log_persistido.py:76
app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:43
app/ponto/tests/test_o219_versao_da_regra.py:253
bin/sondas_rollback.py:53
```

Os dois testes de `ponto` executam fragmentos AST; esses quatro sítios não demonstram quatro módulos falsamente zerados. Quantidade de alvos reais do range carregados **exclusivamente** por opacos externos e ainda com zero no RESUMO: **[não verificado]**. (`app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:38`, `app/ponto/tests/test_o219_versao_da_regra.py:247`)

`_COM_NOME_NA_FRENTE` retira corretamente o primeiro argumento posicional dos quatro loaders, evitando usar o nome lógico como evidência de caminho. Mas exclui somente o keyword `name`: `SourceFileLoader(fullname='unused', path=externo)` aceita `fullname` como literal de caminho e silencia OPACO. `run_path(externo, run_name='__main__')` tem o mesmo problema. (`bin/quem_chama.py:108`, `bin/quem_chama.py:586`, `bin/quem_chama.py:587`, `bin/quem_chama.py:684`)

**7. FILA — P3: o prefixo falso esconde três zeros do próprio contador.**

O sítio `test_detectores_isencao.py:8` recebe apenas três nomes: `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados`. Entretanto, seu prefixo alcança os 104 `.py` daquele diretório: os três argumentos, o pacote pai e **100 arquivos adicionais atribuídos ao sítio**. (`app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`, `app/ponto/tests/test_detectores_isencao.py:15`, `app/ponto/tests/test_detectores_isencao.py:18`, `bin/quem_chama.py:507`)

Em uma simulação em memória com todos esses comandos como alvos, **121 símbolos em 47 arquivos** recebem LOADER exclusivamente por essa atribuição falsa. A lista é:

```text
app/ponto/management/commands/aplicar_09_corte_b.py:30
app/ponto/management/commands/aplicar_09_he_intra.py:32
app/ponto/management/commands/aplicar_col900.py:23
app/ponto/management/commands/aplicar_janela_he_total.py:33
app/ponto/management/commands/apurar_furo_da_exportada.py:49
app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:20
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:29
app/ponto/management/commands/backfill_b5_insumos.py:38
app/ponto/management/commands/carimbo_gravado.py:29
app/ponto/management/commands/celula_veredito_velho.py:30
app/ponto/management/commands/censo_col900.py:50
app/ponto/management/commands/censo_fase_12x36.py:28
app/ponto/management/commands/curar_residuo_bug115.py:36
app/ponto/management/commands/desvio_o68b.py:20
app/ponto/management/commands/detectar_intervalo_ausente.py:26
app/ponto/management/commands/detectar_vinculo_divergente.py:18
app/ponto/management/commands/diff_col900.py:19
app/ponto/management/commands/diff_espelho_alimentacao.py:28
app/ponto/management/commands/diff_janela_he.py:22
app/ponto/management/commands/diff_janela_he_total.py:53
app/ponto/management/commands/diff_reclassificar_partido.py:26
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48
app/ponto/management/commands/escalonar_chamados_supervisao.py:24
app/ponto/management/commands/fechar_chamados_meio_periodo.py:30
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:18
app/ponto/management/commands/folga_que_sumiu.py:28
app/ponto/management/commands/gerar_batidas_smoke_motor.py:22
app/ponto/management/commands/gerar_celulas.py:16
app/ponto/management/commands/marcar_foto_ausente_retro.py:45
app/ponto/management/commands/passivo_disputa_retro.py:42
app/ponto/management/commands/plano_b_no_dinheiro.py:40
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:13
app/ponto/management/commands/recalcular_fechamento.py:38
app/ponto/management/commands/reconciliar_fantasmas.py:46
app/ponto/management/commands/recusar_ponta_pequena.py:49
app/ponto/management/commands/rederivar_minutos_h06_comp11.py:45
app/ponto/management/commands/rejulgar_ausencia_em_folga.py:20
app/ponto/management/commands/reverter_situacao_afastado.py:44
app/ponto/management/commands/supra_juiz.py:45
app/ponto/management/commands/testar_pwa.py:21
app/ponto/management/commands/tripwire_celulas.py:12
app/ponto/management/commands/tripwire_tipo_batida.py:18
app/ponto/management/commands/vigia_b5_ausencia.py:46
app/ponto/management/commands/vigia_pareador_celula.py:16
app/ponto/management/commands/vigia_plantio_overnight.py:16
app/ponto/management/commands/vigiar_ausencia_x_veredito.py:21
app/ponto/management/commands/vigiar_motor_autoridade.py:12
```

Retirar somente os achados de prefixo faz estes três símbolos voltarem a **zero estático**:

- `_chaves_marcos` — `app/ponto/management/commands/gerar_celulas.py:19`.
- `_competencia_de` — `app/ponto/management/commands/passivo_disputa_retro.py:45`.
- `WARN` — `app/ponto/management/commands/testar_pwa.py:27`.

Isso demonstra zeros do contador escondidos pelo falso vínculo. Ausência de consumidores externos/dinâmicos e “zero verdadeiro de produção” para aplicação da L-111: **[não verificado]**. A lei exige zero de produção; o contrato do instrumento admite falsos positivos. (`app/docs/LEIS.md:137`, `bin/quem_chama.py:97`)

Os três testes de settings também atribuem seus sítios a seis perfis adicionais: `ci`, `ci_tenants`, `dev`, `smoke`, `sombra`, `teste_motor`. Seus símbolos já eram não varridos por serem settings; não são novos rebaixamentos. (`app/core/tests/test_selo_erro_com_mensagem.py:80`, `app/core/tests/test_selo_log_nao_e_cego.py:120`, `app/core/tests/test_trilha_com_contexto.py:103`, `bin/quem_chama.py:384`)

Os prefixos de `FORMATADOR` e `sonda:` não acrescentaram outro alvo falso observado na cópia. O primeiro aponta para `relatorios.lote_espelho`; o segundo é um nome de código compilado. (`app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:37`, `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:68`, `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:126`, `bin/sondas_rollback.py:53`)

**8. FILA — P4: mapa de escritores/leitores e semânticas incompatíveis.**

| Valor | Quem escreve/define | Quem lê |
|---|---|---|
| `atrib` | `bin/quem_chama.py:651`, `bin/quem_chama.py:656` | `bin/quem_chama.py:661` |
| `_valores` | `bin/quem_chama.py:658` | Recursão `bin/quem_chama.py:662`; caminho `bin/quem_chama.py:682`; nome `bin/quem_chama.py:695` |
| `Alvos.por_mod`, incluindo `app.` | `bin/quem_chama.py:446`, `bin/quem_chama.py:448`, `bin/quem_chama.py:450` | Texto relativo `bin/quem_chama.py:500`; nomes compostos `bin/quem_chama.py:736`; literais `bin/quem_chama.py:780` |
| `Alvos.sob_prefixo` | `bin/quem_chama.py:504` | `bin/quem_chama.py:739` |
| Retorno de `involucros` | `bin/quem_chama.py:639`, `bin/quem_chama.py:641` | `bin/quem_chama.py:1044` |
| `novo_` | `bin/quem_chama.py:1042`, `bin/quem_chama.py:1044` | Comparação `bin/quem_chama.py:1045`; transferência `bin/quem_chama.py:1047` |
| `opacos` local | `bin/quem_chama.py:664`, `bin/quem_chama.py:690`, `bin/quem_chama.py:701`, `bin/quem_chama.py:718` | Retorno `bin/quem_chama.py:719`; recepção/acúmulo `bin/quem_chama.py:730`, `bin/quem_chama.py:732` |
| `opacos` global/metadado | `bin/quem_chama.py:1037`, `bin/quem_chama.py:1096` | Primeiro alvo `bin/quem_chama.py:1129`; cabeçalho/contador `bin/quem_chama.py:1130`, `bin/quem_chama.py:1132` |
| `LOADERS_CAMINHO` | `bin/quem_chama.py:103` | `bin/quem_chama.py:106`, `bin/quem_chama.py:111` |
| `LOADERS_CAMINHO_AST` | `bin/quem_chama.py:106` | `bin/quem_chama.py:612`, `bin/quem_chama.py:618`, `bin/quem_chama.py:1039` |
| `_SO_BUILTIN` | `bin/quem_chama.py:107` | `bin/quem_chama.py:603` |
| `_COM_NOME_NA_FRENTE` | `bin/quem_chama.py:108` | `bin/quem_chama.py:586` |
| `_RX_LOADER_CAMINHO` | `bin/quem_chama.py:111` | `bin/quem_chama.py:487` |
| `_RX_PYTHON` | `bin/quem_chama.py:109` | `bin/quem_chama.py:669` |
| `_RX_REL_TXT` | `bin/quem_chama.py:113` | `bin/quem_chama.py:499` |

Há quatro incompatibilidades demonstradas:

- `inteiro=True` agora inclui valor calculado, mas os consumidores ainda dependem de encontrar um `Constant` completo: contraexemplo do achado 1. (`bin/quem_chama.py:559`, `bin/quem_chama.py:684`, `bin/quem_chama.py:779`)
- `linhas['caminho']` significa chamada real **ou mera presença** de interpretador; ambos são apresentados como “loader de caminho”. (`bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:678`, `bin/quem_chama.py:733`)
- OPACO significa argumento não resolvido, embora o cabeçalho anuncie ausência de literal no arquivo: contraexemplo `prova_de_casca`. (`bin/prova_de_casca.py:68`, `bin/prova_de_casca.py:86`, `bin/quem_chama.py:1130`)
- Imports AST mantêm a semântica anterior de módulo único, apesar das novas chaves `app.`: contraexemplo do achado 2. (`bin/quem_chama.py:450`, `bin/quem_chama.py:747`, `bin/quem_chama.py:753`)

**9. BLOQUEIA — P5: o selo tem verificações mascaradas de mecanismos novos.**

Nas fixtures r9/r10 exercitadas em memória, sobreviveram estes mutantes:

| Mutante | Por que passa |
|---|---|
| Retirar `compile` de `_kind` | A fixture usa `exec(compile(...))`; o `exec` externo ainda protege `ld_cp`. (`bin/quem_chama.py:618`, `bin/tests/test_quem_chama.sh:295`, `bin/tests/test_quem_chama.sh:595`) |
| Reduzir `_valores` de dois para um salto | A fixture `ld_cte` já contém o basename completo, suficiente para associação lexical. (`bin/quem_chama.py:658`, `bin/quem_chama.py:775`, `bin/tests/test_quem_chama.sh:196`) |

[truncado em 20000 de 23183 caracteres]

## 06/11 19:26 · sessao f9837b72f319 · ESMERIL-SEMANTICA · censo · origem=Code

**Encontrei pares novos divergentes em 6 campos de modelo, 4 parâmetros e no payload C9. Dois campos ficaram como “não sei”.** Contagem consolidada dos sítios examinados, sem medida de incidência em produção: **[nao verificado]**.

As famílias abaixo seguem `app/core/contratos_estruturais.py:45`; os endereços das autoridades de turno, ausência, chamado e fechamento constam em `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:50`, `app/docs/JUIZES.md:54` e `app/docs/JUIZES.md:78`.

**(2) Pares divergentes**

1. **`Batida.timestamp`: data crua × data local na fatia.** O escritor preserva o instante recebido; o fechamento usa `timestamp.date()` para decidir quais batidas pertencem à fatia. Escritor: `app/ponto/registro_batida.py:158`. Leitor: `app/ponto/services/fechamento.py:252`. **Famílias: batida → folha/export.**

   Caso: fatia 01–03/10/2026, dias próprios `{01,03}`; E/S em `02/10 00:15Z` e `02/10 01:15Z`, correspondentes a **01/10 21:15 e 22:15 locais**. O filtro elimina as duas por enxergar dia 02; o motor recebe a lista sem esse par. Fuso configurado: `app/config/settings/base.py:93`; filtro e entrega ao motor: `app/ponto/services/fechamento.py:252`, `app/ponto/services/fechamento.py:278`. Efeito monetário final: **[nao verificado]**.

2. **`Batida.timestamp`: meia-noite seguinte tratada como fim inclusivo.** O escritor aceita o instante; dois consumidores constroem o primeiro instante posterior ao período e o entregam a uma função cujo fim usa `<=`. Escritor: `app/ponto/registro_batida.py:158`. Leitores: `app/ponto/services/abriu_nao_bateu.py:173` e `app/ponto/services/dias_em_aberto.py:121`; inclusão efetiva: `app/ponto/turnos.py:2868`. **Famílias: batida; folha/export no passivo.**

   Casos: única batida em **07/11 00:00 local**, consulta de **06/11** → `_bateu_no_dia=True`; competência terminando em **20/10**, dez dias em aberto e única batida em **21/10 00:00 local** → ela entra no contador e permite `quase_nunca_bateu`, em lugar de `nunca_bateu`. Sítios: `app/ponto/services/abriu_nao_bateu.py:174`, `app/ponto/services/dias_em_aberto.py:122`, `app/ponto/services/dias_em_aberto.py:57`.

3. **`Batida.timestamp`: fração do último segundo excluída.** O escritor grava o timestamp sem truncamento; `dia_do_colab` encerra o dia em `23:59:59`, com comparação inclusiva. Escritor: `app/ponto/registro_batida.py:158`. Leitor: `app/ponto/services/dia_do_colab.py:56`, passando por `app/ponto/turnos.py:2868`. **Família: batida.**

   Caso: batida em **06/11/2026 23:59:59.500000−03:00** → não aparece nas batidas de 06/11, cujo teto é `23:59:59.000000`. Montagem da janela e consulta: `app/ponto/services/dia_do_colab.py:55`, `app/ponto/services/dia_do_colab.py:60`.

4. **`Batida.timestamp`: dia civil usado como início de uma consulta por dia de turno.** O juiz pode atribuir uma entrada após meia-noite à véspera; a regularização começa a busca na data civil dessa entrada. Escritor do instante: `app/ponto/registro_batida.py:158`; atribuição do turno: `app/ponto/turnos.py:299`, `app/ponto/turnos.py:306`; leitor: `app/ponto/services/regularizacao.py:60`. **Família: turno/marcos.**

   Caso: E em **21/09 00:00 local**, turno da véspera **23:50–07:50** → juiz responde **20/09**; consulta iniciada em **21/09** perde esse turno e a regularização responde “Esta batida nao inicia um turno aberto”. Filtro por dia de turno: `app/ponto/turnos.py:1993`; recusa: `app/ponto/services/regularizacao.py:64`. Esse caso **não chega à gravação** de `Justificativa.data_turno`: `app/ponto/services/regularizacao.py:94`.

5. **`Justificativa.data_turno`: escritor grava dia civil; leitores esperam dia do turno.** A triagem conserva a data local da entrada, mesmo tendo recebido o turno pareado, e depois grava essa data na justificativa. Escritores: `app/ponto/services/triagem_batida.py:180`, `app/ponto/services/triagem_batida.py:260`. Leitores: `app/ponto/services/efeito_ausencia.py:43` e `app/ponto/services/fechamento.py:654`. **Origem: turno/marcos; consumo: folha/export.**

   Caso: a mesma E de **21/09 00:00**, pertencente ao turno **20/09**, produz justificativa com `data_turno=21/09`. Depois de aprovada, consultar justificadas de **20/09–20/09** não a encontra. A triagem carrega o universo sem corte por data e mantém a entrada na lista: `app/ponto/services/triagem_batida.py:175`, `app/ponto/services/triagem_batida.py:178`, `app/ponto/services/triagem_batida.py:185`; exclusão pelo intervalo: `app/ponto/services/efeito_ausencia.py:43`.

6. **Fila de flip: produtor fornece dia civil; leitor pergunta por dia do turno.** `fila_flip_recusas` produz `dia=localtime(timestamp).date()`; `avaliar_fila` entrega esse dia a `dia_encerrado`. Produtor: `app/ponto/services/flip_auto.py:140`; leitor: `app/ponto/services/flip_auto.py:285`; contrato do juiz: `app/ponto/turnos.py:2043`. **Família: turno/marcos.**

   Caso: vizinha **S21/09 07:00**, do turno **20/09 19:00–21/09 07:00**, já encerrado; agora **21/09 08:00**, com próximo turno noturno previsto → a fila consulta **21/09**, conta `dia_em_curso` e pula o item do turno anterior. Consumo e saída: `app/ponto/services/flip_auto.py:268`, `app/ponto/services/flip_auto.py:286`. O parâmetro `etiquetas_pela_celula.dia` também recebe essa data e consulta o DNA dela; o recuo só ocorre quando a célula consultada diz folga: `app/ponto/services/flip_auto.py:201`, `app/ponto/services/flip_auto.py:208`, `app/ponto/services/flip_auto.py:310`.

7. **`PerguntaDisputa.data_evento`: dia da saída × dia do turno no fechamento.** Para `orfao_14h`, o produtor pode resolver o dia da saída esperada; o fechamento do fio filtra esse campo pela data do turno que o admin resolveu. Produtor: `app/escala/servico_jornada.py:491`, `app/chamados/services/disputa_emissao.py:1041`; gravação: `app/chamados/models.py:2476`; leitor novo A2: `app/ponto/services/veredito_celula.py:176`. **Família: chamado, com geometria de turno/marcos.**

   Caso: turno **20/09 19:00–21/09 07:00**, pergunta de saída com `data_evento=21/09`; resolver o turno **20/09** consulta `data_evento=20/09` e não seleciona essa pergunta para fechamento. Data da saída esperada: `app/escala/servico_jornada.py:203`, `app/escala/servico_jornada.py:209`; fechamento: `app/ponto/services/veredito_celula.py:174`. O próprio nascimento reconhece que evento e chamado podem diferir por cruzamento da meia-noite: `app/chamados/models.py:2411`.

8. **`Ausencia.data_fim`: nulo aberto × nulo de um único dia.** O escritor usa `None` para intervalos abertos; leitores reduzem esse valor ao início ou exigem início igual ao dia consultado. Escritor: `app/ponto/services/ausencia.py:70`, `app/ponto/services/ausencia.py:343`; leitores novos A2: `app/ponto/services/dia_decidido.py:552`, `app/ponto/services/dia_decidido.py:558`, `app/ponto/services/atestados_lote.py:58`, `app/ponto/services/cartorio.py:356`. **Família: ausencia/ferias.**

   Caso: afastamento aberto iniciado em **01/10**, consulta **01–06/11** → mapa de decisões `{}` e relatório sem a linha. No cartório, uma alteração desse afastamento também não satisfaz a consulta de ausência para **06/11**, pois o ramo nulo exige `data=06/11`. Sítios: `app/ponto/services/dia_decidido.py:560`, `app/ponto/services/atestados_lote.py:58`, `app/ponto/services/cartorio.py:355`.

9. **`Ausencia.aprovada_em` e `Ausencia.criado_em`: instante cru no CSV × apresentação local no painel.** O escritor carimba o instante; `quem_quando` o repassa sem conversão e o CSV aplica `strftime` diretamente. Escritores: `app/ponto/services/ausencia.py:349`, `app/ponto/models.py:813`, `app/ponto/models.py:873`; leitor: `app/ponto/services/atestados_lote.py:131`, via `app/ponto/services/dia_decidido.py:508`. **Família: ausencia/ferias.**

   Caso: `2026-11-06T01:30Z` → CSV **06/11 01:30**; painel **05/11 22:30**. O mesmo ocorre com `criado_em` quando `aprovada_em` está nulo. Conversão local do painel: `app/templates/ponto/partials/painel_ausencia.html:261`, `app/templates/ponto/partials/painel_ausencia.html:188`; configuração: `app/config/settings/base.py:93`.

10. **Payload C9: dia do turno + hora de marco lidos como mesmo dia civil.** O produtor grava dia da célula e marcos faltantes; o fallback de `lastro.julgar` combina todas as horas com esse mesmo dia, sem encadear a madrugada. Produtor: `app/ponto/services/cartorio.py:1778`, `app/ponto/management/commands/detectar_ausencias.py:706`; leitor: `app/ponto/services/lastro.py:305`. **Famílias: chamado → turno/marcos.**

    Caso: `data_turno=20/09`, marco **S07:00**, batidas **E20/09 19:00 / S21/09 07:00**, sem lâmpada correspondente → alvo **20/09 07:00**, distância de **1.440 minutos**, classificação `outro_horario`. Esse caminho depende da ausência da lâmpada: `app/ponto/services/lastro.py:289`, `app/ponto/services/lastro.py:299`, `app/ponto/services/lastro.py:308`, `app/ponto/services/lastro.py:320`.

**(1) Quadro restrito a “sim” e “não sei”**

E conta funções escritoras explícitas em A2; L conta a **amostra de funções leitoras abertas**, não o total global. Os sítios externos de suporte estão identificados. Completude global dos escritores/leitores fora de A2: **[nao verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores — n e sítios | Leitores — n, amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `Batida.timestamp` | Instante preservado: `app/ponto/registro_batida.py:158`; data crua: `app/ponto/services/fechamento.py:252`; limites civis incompatíveis: `app/ponto/services/dia_do_colab.py:56` | **0 diretos A2**; porta externa **1**, `app/ponto/registro_batida.py:94` | Amostra **8**: os consumidores dos pares 1–4, fila de recusas, avaliação do flip e lastro, nos sítios acima | **sim** | Pares **1–4 e 6** |
| `Justificativa.data_turno` | Data do turno no contrato: `app/ponto/models.py:144`; data civil da entrada no escritor: `app/ponto/services/triagem_batida.py:260` | **2**: `app/ponto/services/regularizacao.py:94`; `app/ponto/services/triagem_batida.py:260` | Amostra **4**: `app/ponto/services/efeito_ausencia.py:43`; `app/ponto/services/fechamento.py:654`; `app/ponto/services/justificativa.py:200`; `app/ponto/services/cartorio.py:359` | **sim** | Par **5**; regularização tem a recusa anterior descrita no par **4** |
| `PerguntaDisputa.data_evento` | Dia da saída esperada: `app/escala/servico_jornada.py:491`; igualdade com dia do turno resolvido: `app/ponto/services/veredito_celula.py:176` | **0 A2**; suporte externo lido: nascimento em `app/chamados/models.py:2476` e reapontamento em `app/chamados/services/fio.py:834` | **2**: `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176` | **sim** | Par **7** |
| `Ausencia.data_fim` | Fim inclusivo ou aberto: `app/ponto/services/ausencia.py:67`; nulo reduzido ao início: `app/ponto/services/dia_decidido.py:558` | **2 explícitos**: `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:500`; derivação adicional no modelo: `app/ponto/models.py:907` | Amostra **4**: `app/ponto/services/dia_decidido.py:552`; `app/ponto/services/atestados_lote.py:58`; `app/ponto/services/cartorio.py:356`; `app/ponto/services/vigia_ausencia_veredito.py:48` | **sim** | Par **8** |
| `Ausencia.aprovada_em` | Instante aware: `app/ponto/services/ausencia.py:349`; texto sem conversão local: `app/ponto/services/atestados_lote.py:131` | **2 A2**: `app/ponto/services/ausencia.py:349`, `app/ponto/services/ausencia.py:917`; auto-carimbo externo: `app/ponto/models.py:873` | Amostra **3**: `app/ponto/services/dia_decidido.py:508`; `app/ponto/services/atestados_lote.py:131`; `app/ponto/services/cartorio.py:357` | **sim** | Par **9** |
| `Ausencia.criado_em` | Instante automático: `app/ponto/models.py:813`; fallback cru para apresentação: `app/ponto/services/dia_decidido.py:508` | **0 explícitos A2**; automático em `app/ponto/models.py:813` | Amostra **2**: `app/ponto/services/dia_decidido.py:508`; `app/ponto/services/atestados_lote.py:131` | **sim** | Par **9**, quando não há aprovação |
| `_bateu_no_dia.dia` | Dia civil convertido em intervalo que inclui a meia-noite seguinte: `app/ponto/services/abriu_nao_bateu.py:172` | **1 produtor A2**: `app/ponto/services/abriu_nao_bateu.py:130` | **1**: `app/ponto/services/abriu_nao_bateu.py:174` | **sim** | Par **2** |
| `dia_do_colab.data` | Dia civil convertido em intervalo sem a fração final do segundo: `app/ponto/services/dia_do_colab.py:56` | **0 chamadas A2**; origem externa completa **[nao verificado]** | **1**: `app/ponto/services/dia_do_colab.py:60` | **sim** | Par **3** |
| `etiquetas_pela_celula.dia` | Data civil produzida pela fila: `app/ponto/services/flip_auto.py:140`; dia do DNA/turno: `app/ponto/services/flip_auto.py:201` | **1 chamada A2**: `app/ponto/services/flip_auto.py:310` | **1**: `app/ponto/services/flip_auto.py:201` | **sim** | Par **6** |
| `fechar_o_fio_do_dia.data` | Dia do turno declarado/resolvido: `app/ponto/services/veredito_celula.py:326`; comparação direta com dia do evento: `app/ponto/services/veredito_celula.py:176` | **2 chamadas A2**: `app/ponto/services/veredito_celula.py:325`; `app/ponto/services/declarar_dia.py:204` | **1**: `app/ponto/services/veredito_celula.py:174` | **sim** | Par **7** |
| `OrdemSubstituicao.data_inicio` e `.data_fim` — **2 campos** | Disponibilidade usa sobreposição estrita: `app/ponto/utils.py:205`; atividade inclui as duas pontas: `app/ponto/models.py:1147` | **2 funções por campo**: `app/ponto/services/cobertura.py:105`; `app/ponto/services/cobertura.py:291` | Amostra: disponibilidade chamada em `app/ponto/services/cobertura.py:83`; atividade em `app/ponto/models.py:1147`, `app/ponto/models.py:1155` | **não sei** | Há desacordo nas bordas; intenção do escritor e saída temporal incorreta alcançável **[nao verificado]**. Duas ordens ativas adjacentes não servem de prova: constraints em `app/ponto/models.py:1124`, `app/ponto/models.py:1130` |
| Payload `data_turno` + hora de marco | Dia do turno produzido: `app/ponto/services/cartorio.py:1779`; hora combinada no mesmo dia civil: `app/ponto/services/lastro.py:305` | **1 caminho novo lido**: `app/ponto/services/cartorio.py:1778` → `app/ponto/management/commands/detectar_ausencias.py:706` | **1**: `app/ponto/services/lastro.py:305` | **sim** | Par **10**, condicionado à ausência da lâmpada |

**Demais examinados:** 43 campos de modelo e 117 parâmetros sem novo desacordo identificado nos sítios lidos; isso não certifica unanimidade global fora do recorte. **[nao verificado: consolidação da leitura e da enumeração AST, sem artefato persistido]**

**(3) Contagem**

| Universo examinado | Total | Divergentes | Não sei | Sem novo desacordo encontrado |
|---|---:|---:|---:|---:|
| Campos de modelo | **51** | **6** | **2** | **43** |
| Parâmetros | **121** | **4** | **0** | **117** |
| Payload C9, separado dos anteriores | **1** | **1** | **0** | **0** |

Contagens consolidadas: **[nao verificado: enumeração AST em memória e união das identidades examinadas]**. Os dois “não sei” são os limites de `OrdemSubstituicao`, pelo motivo indicado no quadro; incidência dos casos em produção: **[nao verificado]**.

Dos 51 campos, **46 estão nas linhas CAMPO fornecidas** e **5 foram encontrados no código além delas**: `MetricaSnapshot.data_ini/data_ref/calculado_em`, `LogAuditoria.criado_em` e `LoteHolerite.publicado_em`; exemplos de uso e declaração em `app/inteligencia/models.py:90`, `app/inteligencia/models.py:96`, `app/ponto/services/retratar_humano.py:39` e `app/ponto/services/fechamento.py:1523`. Dos 121 parâmetros, **44** têm nome `dia`/`data_*`, incluindo quatro `data_` internos do cartório, e **77** são as extensões temporais `data`, `ini`, `fim` e `dia_*`: **[nao verificado: contagem AST sem artefato persistido]**.

## 06/11 19:26 · sessao 2e9b4e1cb20c · ESMERIL-SEMANTICA · censo · origem=Code

**Encontrei 4 campos divergentes, em 3 desacordos. O censo integral de campos de A1 não ficou fechado; as contagens abaixo distinguem a cobertura realizada da cobertura ainda [nao verificado].** (app/chamados/services/validacao.py:826; app/chamados/veredito_lavrado.py:182; app/chamados/services/janela_colab.py:73)

**(1) Quadro**

`E` conta sítios de atribuição/criação abertos, incluindo limpeza ou restauração; `R` conta funções leitoras identificadas na amostra indicada. Não conta `update_fields` como outro escritor. A quantidade total de leitores por modelo, além dessas amostras, é **[nao verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores — n e sítios | Leitores — n, amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `PerguntaDisputa.data_evento` | **Dia do turno/célula**, inclusive para saída amanhã: app/chamados/services/declaracao_texto.py:123, app/chamados/services/declaracao_texto.py:326. **Dia civil da hora respondida**, na busca de conflito: app/chamados/services/validacao.py:826. | **3 sítios finais**: criação em app/chamados/models.py:2476; preenchimento e restauração em app/chamados/services/fio.py:834 e app/chamados/services/fio.py:843. Também abertos os chamadores da declaração e emissão: app/chamados/services/declaracao_texto.py:326, app/chamados/services/declaracao_texto.py:615, app/chamados/services/disputa_emissao.py:396 e app/chamados/services/disputa_emissao.py:777. | **2 funções na amostra decisiva**: classificação em app/chamados/services/validacao.py:798; plantio em app/chamados/services/materializacao.py:344, com ajuste em app/chamados/services/materializacao.py:420. | **Sim** | Turno **20/10/2026 19h → 21/10 07h**; `data_evento=20/10`, resposta e marco `07:00`; batida existente **21/10 07:05**. A classificação procura **20/10 06h–08h** e devolve `LOTE_A`, cujo rótulo diz “sem batida perto”; o plantio deriva **21/10 07h**. (app/chamados/services/validacao.py:765; app/chamados/services/validacao.py:826; app/chamados/services/validacao.py:830; app/chamados/juizes.py:310) |
| `PerguntaDisputa.julgado_em` | **Instante da lavra**, escrito com `timezone.now()`; depois, **data crua desse instante**, comparada com a data de expiração. (app/chamados/veredito_lavrado.py:228; app/chamados/veredito_lavrado.py:185) | **3**: lavra em app/chamados/veredito_lavrado.py:228; restauração em memória em app/chamados/veredito_lavrado.py:244; backfill em app/chamados/management/commands/backfill_veredito_lavrado.py:139. | **4 funções**: `esta_stale`, `foto_da_lavra`, varredura e backfill. (app/chamados/veredito_lavrado.py:173; app/chamados/veredito_lavrado.py:237; app/chamados/services/raiox.py:31; app/chamados/management/commands/backfill_veredito_lavrado.py:139) | **Sim** | Expiração **21/11**; relavra **21/11 01:00Z = 20/11 22h local**; consulta **21/11 03:30Z = 21/11 00:30 local**. Retorna `False` para stale porque `julgado_em.date()==21/11`, embora a lavra seja da véspera local. (app/chamados/veredito_lavrado.py:185; app/config/settings/base.py:93) |
| `PerguntaDisputa.janela_expira_em` | **Data civil da virada da janela**, inclusive na classificação do backfill por `localdate()`; leitor compara com `now().date()` cru. (app/chamados/veredito_lavrado.py:116; app/chamados/management/commands/backfill_veredito_lavrado.py:94; app/chamados/management/commands/backfill_veredito_lavrado.py:113; app/chamados/services/raiox.py:36) | **3**: app/chamados/veredito_lavrado.py:229; restauração em app/chamados/veredito_lavrado.py:245; app/chamados/management/commands/backfill_veredito_lavrado.py:140. | **4 funções**, mesmas amostras da linha anterior; comparações em app/chamados/veredito_lavrado.py:182 e app/chamados/services/raiox.py:41. | **Sim** | Expiração **21/11**, lavra **20/11 20:00Z**, agora **21/11 00:30Z = 20/11 21:30 local** → stale `True`, **2h30 antes da virada civil**. (app/chamados/veredito_lavrado.py:182; app/chamados/veredito_lavrado.py:185; app/config/settings/base.py:93) |
| `ChamadoColaborador.silenciado_ate` | **Fim temporário da supressão**, após o qual pode reaparecer na fila; versus **marca persistente de arquivo**, cujo vencimento vira histórico e cuja presença continua excluindo a lista do colaborador. (app/chamados/services/arquivo.py:150; app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) | **2**: colocar prazo em app/chamados/services/arquivo.py:166; limpar em app/chamados/services/arquivo.py:201. | **4 funções na amostra**: predicado do modelo, fila, verbos e lista do colaborador. (app/chamados/models.py:480; app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) | **Sim** | Chamado vivo, dentro da competência, `silenciado_ate=06/11/2026 12:00Z`, agora `13:00Z`: entra na fila do painel, vira `HISTORICO` nos verbos e continua excluído da lista do colaborador. **Qual política deve prevalecer: [nao verificado].** (app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) |
| `PerguntaDisputa.materializada_em` — novo leitor I6 | Instante de processamento; auditor extrai `.date()` cru para separar o piso histórico “até 24/08”. A origem/fuso dos carimbos históricos dessa via é **[nao verificado]**. (app/chamados/models.py:2827; app/chamados/management/commands/auditar_invariantes_chamados.py:452; app/chamados/management/commands/auditar_invariantes_chamados.py:478) | **2 sítios centrais abertos**: atribuição em app/chamados/models.py:2558; limpeza em app/chamados/models.py:2700. **Escritor histórico de `legado_sem_lastro`: [nao verificado]**; o auditor declara ausência de escritor vivo em app/chamados/management/commands/auditar_invariantes_chamados.py:445. | **2 funções na amostra**: estado em app/chamados/models.py:2499; universo I6 em app/chamados/management/commands/auditar_invariantes_chamados.py:455. | **Não sei** | Carimbo **25/08/2026 01:00Z = 24/08 22h local** cai em violação pela data crua; pela data local cairia no piso. Não fechei o par com o escritor histórico. (app/chamados/management/commands/auditar_invariantes_chamados.py:478; app/config/settings/base.py:93) |
| `chamados_sem_dia_do_fato.data_ini` | Parâmetro declarado, **sem consumo no corpo**; significado pretendido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:75; app/chamados/services/juizes_do_dia.py:92) | **0 fornecimentos explícitos localizados**: o chamador aberto usa os defaults. (app/chamados/services/fila.py:83) | **0 usos no corpo**: consulta todos os vivos. (app/chamados/services/juizes_do_dia.py:92) | **Não sei** | Informar `data_ini=01/11/2026` não restringe a consulta; resultado devido para esse argumento **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:92) |
| `chamados_sem_dia_do_fato.data_fim` | Parâmetro declarado, **sem consumo no corpo**; significado pretendido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:75; app/chamados/services/juizes_do_dia.py:92) | **0 fornecimentos explícitos localizados**: app/chamados/services/fila.py:83. | **0 usos no corpo**: app/chamados/services/juizes_do_dia.py:92. | **Não sei** | Informar `data_fim=31/10/2026` não restringe a consulta; resultado devido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:92) |

Ocorrência desses casos no acervo de produção: **[nao verificado]**.

**(2) Somente os divergentes — pares e famílias**

- **`data_evento`:** escritor da declaração **app/chamados/services/declaracao_texto.py:326**, persistido em **app/chamados/models.py:2476**, × leitor **app/chamados/services/validacao.py:826**. O desacordo aparece contra a interpretação de turno usada em **app/chamados/services/materializacao.py:420**. Família **chamado**, com geometria **turno/marcos**. (app/core/contratos_estruturais.py:48; app/core/contratos_estruturais.py:51; app/docs/JUIZES.md:24)
- **`julgado_em` e `janela_expira_em`:** escritores **app/chamados/veredito_lavrado.py:228** e **app/chamados/veredito_lavrado.py:229** × leitores **app/chamados/veredito_lavrado.py:182**, **app/chamados/veredito_lavrado.py:185** e sua varredura em **app/chamados/services/raiox.py:41**. Família **chamado**. (app/core/contratos_estruturais.py:51)
- **`silenciado_ate`:** escritor temporário **app/chamados/services/arquivo.py:166**, cuja renovação evita reaparecimento conforme **app/chamados/services/arquivo.py:150**, × leitores **app/chamados/services/janela_colab.py:73** e **app/chamados/services/verbos.py:51**. Família **chamado**. (app/core/contratos_estruturais.py:51)

**(3) Contagem e cobertura**

| Cobertura realizada | Examinados | Divergentes | Não sei | Sem divergência encontrada na amostra |
|---|---:|---:|---:|---:|
| Campos: 28 declarados em `chamados`, mais início/fim/cancelamento de cobertura | **31** | **4** | **1** | **26** |
| Parâmetros temporais, incluindo `data` temporal e os quatro `ini/fim` do anexo | **65** | **0** | **2** | **63** |
| **Total dessa cobertura** | **96** | **4** | **3** | **89** |

As declarações dos campos estão em app/chamados/models.py:54, app/chamados/models.py:1602, app/chamados/models.py:1686, app/chamados/models.py:2218, app/chamados/models.py:3181, app/chamados/models.py:3218 e app/chamados/models.py:3260; a cobertura foi comparada entre app/ponto/services/cobertura.py:57 e app/chamados/detector_anomalias.py:71. **Exaustão dos chamadores de todos os 65 parâmetros: [nao verificado].**

Separadamente, examinei `dia_corte` como **inteiro de corte**, não data; os dois parâmetros FCM chamados `data` são **payloads**, não temporais. (app/chamados/management/commands/arquivar_competencia_encerrada.py:181; app/chamados/fcm_utils.py:50; app/chamados/fcm_utils.py:102)

**O total completo de campos externos alcançados pelo inventário A1 e a concordância de todos os seus escritores não foram fechados: [nao verificado]. Portanto, 96 é a cobertura acima, não um certificado de conclusão de todo A1.**

## 06/11 19:25 · sessao 93a650db3dbe · ESMERIL-SEMANTICA · censo · origem=Code

**Há divergências demonstráveis, mas esta volta não fechou o censo universal de escritores.** Os números abaixo contam sítios efetivamente abertos; totais globais de escritores/leitores permanecem **[não verificado]**. Os casos são deduções estáticas do código, sem execução nem confirmação de incidência em produção **[não verificado]**.

**(2) Pares divergentes — prioridade**

| ID | Campo/parâmetro | Escritor ou fornecedor × leitor que discorda | Família estrutural |
|---|---|---|---|
| D1 | `OrdemSubstituicao.data_fim` | A designação recebe uma janela cuja disponibilidade usa fim **exclusivo** (`app/ponto/services/cobertura.py:83`, `:106`; `app/ponto/utils.py:143`, `:206`). `esta_ativa_em` e `ativa_para` incluem o instante final (`app/ponto/models.py:1147`, `:1156`). | **escala**, conforme `app/core/contratos_estruturais.py:145`. |
| D2 | `Justificativa.data_turno` | O cron grava o **dia da jornada** recebido do turno (`app/ponto/management/commands/processar_alertas_turno.py:393`). O formulário recebe somente o timestamp da entrada, oferece seu **dia civil** e grava esse dia (`app/ponto/views.py:1584`; `app/templates/ponto/justificativa_form.html:30`; `app/ponto/views.py:1638`). Os leitores procuram cobertura e invalidam fechamento por `j.data_turno` (`app/ponto/views.py:1681`, `:1805`). | **turno/marcos**, pelo domínio do dia do turno (`app/docs/JUIZES.md:20`). |
| D3 | `ChamadoColaborador.criado_em` — par novo | O nascimento usa carimbo automático (`app/chamados/models.py:61`). `ping_geo` calcula o filtro diário com `agora = tz.now()` e `agora.date()` cru (`app/ponto/views.py:2946`, `:3015`), enquanto outros leitores diários usam `timezone.localdate()` (`app/ponto/utils.py:29`, `:34`). | **chamado** (`app/core/contratos_estruturais.py:51`). |
| D4 | `DisputaSupervisao.aberta_em` | Carimbo de nascimento automático (`app/chamados/models.py:1686`) × idade calculada subtraindo `.date()` cru do carimbo do **dia local** atual (`app/ponto/management/commands/alertar_disputas_pendentes.py:31`, `:52`, `:73`). | **chamado** (`app/core/contratos_estruturais.py:51`). |
| D5 | `Ausencia.data` e `data_fim` — par novo | O escritor mantém um **intervalo inclusivo**, possivelmente aberto (`app/ponto/services/ausencia.py:66`, `:75`, `:342`). O painel de fechamento seleciona apenas ausências cujo **início** está na competência (`app/ponto/views.py:1004`), embora a cobertura considere sobreposição (`app/ponto/turnos.py:2753`). | **ausencia/ferias**, com consumo em **folha/export** (`app/core/contratos_estruturais.py:134`, `:158`). |
| D6 | `AgendamentoFerias.data_inicio` e `data_fim` — pares novos | O escritor grava dias inclusivos, `fim = início + dias − 1` (`app/ferias/services.py:313`, `:322`). A disponibilidade converte uma janela de instantes **[início,fim)** em duas datas e consulta sobreposição inclusiva (`app/ponto/utils.py:143`, `:198`; `app/ferias/services.py:51`). Outro leitor apresenta o próprio último dia como “Retorno previsto” (`app/ponto/utils.py:111`). | **ausencia/ferias** (`app/core/contratos_estruturais.py:134`). |
| D7 | `DecisaoHE.data` | A porta grava a decisão sobre o dia informado (`app/ponto/portas/he.py:67`; `app/ponto/views.py:524`, `:529`). Um consumidor procura autorização pelo **dia da jornada** (`app/ponto/motor_calculo_v2.py:1456`, `:2461`); outro recompõe **dia civil da entrada** e consulta essa chave (`:1528`, `:1653`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D8 | `Empresa.janela_he_desde` e `_empresa_da_janela_do_dia.dia` | O fornecedor grava o **primeiro dia da competência** (`app/ponto/management/commands/diff_janela_he_total.py:104`, `:106`; porta em `app/colaboradores/services/estrutura.py:17`). A comparação de vigência recebe jornada em um caminho e civil da entrada em outro (`app/ponto/motor_calculo_v2.py:1393`, `:1451`, `:1528`, `:1529`). | **folha/export**, com geometria de **turno/marcos** (`app/docs/JUIZES.md:29`, `:35`). |
| D9 | `Batida.timestamp → dia derivado` — pares novos de DSR | O motor atribui e guarda o dia da jornada (`app/ponto/motor_calculo_v2.py:2403`, `:2408`, `:2460`). O DSR usa esse dia para presença, mas usa civil da entrada para extras e denominador (`:2578`, `:2639`, `:2643`). O comercial agrupa pela jornada e depois refaz DSR por civil (`:3313`, `:3317`, `:3441`, `:3454`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D10 | `_feriado_dobra_no_dia.dia` | O contrato exige **início do plantão** (`app/ponto/motor_calculo_v2.py:928`). Dois fornecedores passam `e.date()` do segmento (`:2809`, `:3255`); outros passam dia da jornada (`:3092`, `:3337`; `app/ponto/calculador/regras.py:338`, `:351`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D11 | `tripwire_celulas`: `ini/fim` de duas funções | Os defaults prometem competência da empresa, mas são produzidos sem empresa, portanto com corte padrão 21 (`app/ponto/management/commands/tripwire_celulas.py:78`, `:85`, `:232`, `:247`; `app/ponto/janelas.py:50`). Os leitores aplicam a mesma janela às células de várias empresas (`tripwire_celulas.py:97`, `:257`). | **celula/precedencia**, recorte de **folha/export** (`app/core/contratos_estruturais.py:47`, `:52`; `app/docs/JUIZES.md:35`). |

**(3) Contagem e limite de fechamento**

| Universo efetivamente examinado | Quantidade | Classificação |
|---|---:|---|
| Campos de modelo | **34** | **11 divergentes**, **6 não sei específicos**, **17 sem divergência na amostra**, mas sem fechamento universal dos escritores **[não verificado]**. |
| Posições de parâmetros, contando separadamente cada função/argumento | **186** | **6 divergentes**: os dois parâmetros de D8/D10 e os quatro extremos de D11; **180 sem fechamento dos fornecedores** **[não verificado]**. |
| Total examinado | **220** | **17 divergentes; 203 não sei para a pergunta universal; nenhum “não” universal demonstrado** **[não verificado quanto à exaustividade global]**. |

A contagem de parâmetros compreende **45** posições no cálculo/janelas, **96** no núcleo/portas, **44** nos comandos examinados e **1** em `utils.alertas_turno_visiveis`; inclui os nomes adicionais presentes no inventário, como `ini/fim`, `dia_corte`, booleanos e dicionários. As definições estão, respectivamente, em `app/ponto/calculador/regras.py:576`, `app/ponto/motor_calculo_v2.py:82`, `app/ponto/turnos.py:705`, `app/ponto/management/commands/tripwire_celulas.py:70` e `app/ponto/utils.py:222`; o detalhamento integral da contagem não foi persistido **[não verificado]**.

**(1) Quadro — divergentes e não sei**

“N” abaixo significa **sítios lidos**, não total global. Onde há fornecedores ou escritores adicionais sem censo encerrado, a totalidade é **[não verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores: n e sítios | Leitores: n e amostra lida | Divergente? | Caso concreto → resultado incompatível |
|---|---|---|---|---|---|
| D1 — `OrdemSubstituicao.data_fim` | Instante final exclusivo × inclusivo (`app/ponto/utils.py:206`; `app/ponto/models.py:1147`). | **2**: `app/ponto/services/cobertura.py:106`, `:291`. | **5**: `app/ponto/utils.py:206`; `app/ponto/models.py:1147`, `:1156`; `app/ponto/management/commands/detectar_ausencias.py:370`; `app/ponto/management/commands/reconciliar_grade.py:409`. | **sim** | Ordem 06/11 07:00–19:00, consulta às 19:00: método declara ativa; disponibilidade para janela iniciando às 19:00 não reconhece sobreposição (`app/ponto/models.py:1147`; `app/ponto/utils.py:206`). |
| D2 — `Justificativa.data_turno` | Dia da jornada × dia civil da entrada (`app/ponto/management/commands/processar_alertas_turno.py:393`; `app/templates/ponto/justificativa_form.html:30`). | **3**: cron `processar_alertas_turno.py:393`; web `app/ponto/views.py:1638`; API `app/api/views.py:2010`. | **3**: busca da pendente, cobertura e invalidação (`app/ponto/views.py:1622`, `:1681`, `:1805`). | **sim** | Jornada 20/09, marcos 19:00–07:00, entrada 21/09 00:05 atribuída à véspera: cron procura/grava 20/09; formulário envia 21/09 e procura outra pendência (`app/ponto/turnos.py:299`, `:306`; `app/ponto/views.py:1622`). |
| D3 — `ChamadoColaborador.criado_em` | Instante de nascimento × chave diária UTC crua × chave diária local (`app/chamados/models.py:61`; `app/ponto/views.py:3015`; `app/ponto/utils.py:29`). | **1 mecanismo automático**, acionado pelo nascimento em `app/chamados/models.py:932`. | **3 amostras**: `app/ponto/views.py:3015`; `app/ponto/utils.py:34`, `:97`. | **sim** | Em 07/11 01:00Z, ainda 06/11 22:00 local, o emissor procura dia 07/11; o chamado recém-criado pertence ao dia local 06/11. A repetição pode deixar de encontrá-lo (`app/ponto/views.py:2946`, `:3015`; `app/chamados/models.py:867`). |
| D4 — `DisputaSupervisao.aberta_em` | Instante de abertura × idade por data UTC/local misturadas (`app/chamados/models.py:1686`; `app/ponto/management/commands/alertar_disputas_pendentes.py:52`). | **1 mecanismo automático**; **3 nascimentos lidos** em `app/chamados/services/disputa_emissao.py:523`, `:962`, `:1182`. | **2**, ambos lidos: `app/ponto/management/commands/alertar_disputas_pendentes.py:52`, `:73`. | **sim** | Abertura 06/11 02:30Z = 05/11 23:30 local; execução 06/11 08:00 local → “hoje”, embora tenha sido “ontem” (`alertar_disputas_pendentes.py:52`, `:54`, `:56`; fuso em `app/config/settings/base.py:93`, `:95`). |
| D5 — `Ausencia.data/data_fim` | Intervalo inclusivo/aberto × seleção somente pelo início (`app/ponto/services/ausencia.py:66`; `app/ponto/views.py:1004`). | **2 atos da porta**: criação `app/ponto/services/ausencia.py:342`; edição `:499`, `:500`; normalização adicional em `app/ponto/models.py:907`. | **2 amostras**: painel `app/ponto/views.py:1004`; cobertura `app/ponto/turnos.py:2753`. | **sim**, 2 campos | Ausência 01/09–30/09, competência 21/09–20/10 → cobre 21–30/09, mas não aparece na lista de ausências do painel porque começou antes de 21/09 (`app/ponto/views.py:1004`). |
| D6 — `AgendamentoFerias.data_inicio/data_fim` | Dias inclusivos × projeção de janela exclusiva; último dia × retorno (`app/ferias/services.py:313`; `app/ponto/utils.py:143`, `:198`, `:111`). | **1 ato de criação lido**: `app/ferias/services.py:322`; demais atos globais **[não verificado]**. | **2 amostras**: disponibilidade `app/ponto/utils.py:198`; descrição `:111`. | **sim**, 2 campos | Cobertura `[06/11 19:00,07/11 00:00)` e férias iniciando em 07/11 → pessoa excluída apesar de não haver interseção de instantes. Férias até 20/11 → texto anuncia “Retorno previsto: 20/11”, embora diga que volta no dia seguinte (`app/ponto/utils.py:198`, `:111`). |
| D7 — `DecisaoHE.data` | Dia autorizado/jornada × civil da entrada (`app/ponto/portas/he.py:67`; `app/ponto/motor_calculo_v2.py:1456`, `:1528`). | **1 sítio da data**: `app/ponto/portas/he.py:67`. | **3**: carga `app/ponto/motor_calculo_v2.py:1417`; consultas `:1456`, `:1653`. | **sim** | Autorização em 20/09; jornada 20/09 com entrada 21/09 02:30 e saída 07:00 → consulta por 21/09 ignora autorização. Com marco 19:00, a entrada pode ser deslocada para 21/09 19:00 (`app/ponto/motor_calculo_v2.py:1528`, `:1653`; `app/ponto/janela_he.py:94`, `:102`). |
| D8 — `Empresa.janela_he_desde`; `_empresa_da_janela_do_dia.dia` | Vigência a partir da competência/jornada × dia civil do segmento (`app/colaboradores/models.py:119`; `app/ponto/motor_calculo_v2.py:1451`, `:1529`). | Campo: **2 fornecedores A3 lidos**, `app/ponto/management/commands/diff_janela_he.py:103`, `diff_janela_he_total.py:106`, pela porta `app/colaboradores/services/estrutura.py:17`. Parâmetro: **2 chamadas**, motor `:1451`, `:1529`. | **1 comparação de vigência**: `app/ponto/motor_calculo_v2.py:1393`. | **sim**, 1 campo + 1 parâmetro | Vigência 21/09; jornada 20/09 com entrada civil em 21/09 → ramo da jornada considera anterior à vigência; ramo civil aplica a janela. Delta monetário exato **[não verificado]** (`app/ponto/motor_calculo_v2.py:1393`, `:1528`). |
| D9 — `Batida.timestamp → dia derivado` | Instante preservado; atribuição posterior à jornada × reextração civil no DSR (`app/ponto/registro_batida.py:153`; `app/ponto/motor_calculo_v2.py:2460`, `:2639`). | **2 criações da batida**: `app/ponto/registro_batida.py:153`, `:158`; **2 atribuições de dia examinadas**: motor `:2460`, `:3313`. | **5 grupos de leitura**: presença `:2578`; reflexo `:2639`, `:2643`; banco comercial `:3423`; presença comercial `:3441`, `:3454`; reflexo comercial `:3466`, `:3470`. | **sim**, no dia derivado | Competência termina domingo 20/09; período atribuído a 20/09, entrada civil 21/09 e 60 min de HE → presença entra na semana 14–20, mas seus 60 min ficam fora do reflexo dessa semana. O comercial ainda pode marcar falta em 20/09 (`app/ponto/motor_calculo_v2.py:2429`, `:2431`, `:2639`, `:3454`). |
| D10 — `_feriado_dobra_no_dia.dia` | Início do plantão × data civil do segmento (`app/ponto/motor_calculo_v2.py:928`, `:2809`). | **5 fornecedores lidos**: motor `:2809`, `:3255`, `:3092`, `:3337`; `app/ponto/calculador/regras.py:351`. | **1 predicado**: `app/ponto/motor_calculo_v2.py:934`. | **sim** | Jornada 11/10, cauda 12/10 03:00–07:00, feriado em 12/10, dobra ligada e janela desligada → ramo por segmento lança 240 min de HE100 feriado; consulta pela jornada 11/10 devolve zero (`app/ponto/motor_calculo_v2.py:2809`, `:2812`; `app/ponto/calculador/regras.py:351`). |
| D11 — `medir_pausa_faltando.ini/fim`; `medir_sobra_com_tipo_trocado.ini/fim` | Competência empresarial prometida × competência padrão 21–20 (`app/ponto/management/commands/tripwire_celulas.py:78`, `:85`, `:232`, `:247`). | **1 atribuição conjunta por função**: `tripwire_celulas.py:86`, `:248`; chamadas sem argumentos em `:378`, `:389`. | **1 filtro por função**: `tripwire_celulas.py:97`, `:257`. | **sim**, 4 parâmetros | Empresa corte 5; referência 06/11/2026 → deveria consultar 05/11–04/12, mas consulta 21/10–20/11. Inclui dias da competência anterior e omite 21/11–04/12 (`app/ponto/janelas.py:24`, `:50`, `:63`). |
| `PerguntaDisputa.data_evento` — par novo | Dia do fato; para conversa, dia local da emissão sem identidade factual (`app/chamados/models.py:2403`, `:2422`, `:2427`). O mapa usa igualdade da data como “furo coberto por pergunta” (`app/ponto/management/commands/mapa_divergencia.py:264`, `:275`). | **2 atos abertos**: nascimento `app/chamados/models.py:2476`; preenchimento no reapontamento `app/chamados/services/fio.py:832`, `:847`. Total **[não verificado]**. | **1 filtro + 1 extração**: `app/ponto/management/commands/mapa_divergencia.py:272`, `:273`. | **não sei** | Conversa `ancora_ausente` emitida em 06/11 e furo em 06/11 → `furo_com_pd` sobe 1 e o dia sai do restante. Isso ocorre pelo código; se a métrica pretende cobertura **factual** ou presença de qualquer pergunta permanece **[não verificado]** (`mapa_divergencia.py:275`, `:276`). |
| `EventoCelula.data` | Snapshot do dia da célula (`app/ponto/portas/celula.py:891`). | **1**: `app/ponto/portas/celula.py:891`. | **0 leitores semânticos encontrados**; consumidor aberto agrupa por colaborador (`app/chamados/reconciliador.py:1340`). Total **[não verificado]**. | **não sei** | Não há par leitor demonstrado nem saída errada demonstrada **[não verificado]**. |
| `FuroDiario.avaliada_em` | Instante da avaliação (`app/ponto/portas/celula.py:1127`). | **1**: `app/ponto/portas/celula.py:1127`. | **0 leitores do carimbo encontrados**; leitor aberto usa a data do furo (`app/escala/services/furos_diarios.py:133`). Total **[não verificado]**. | **não sei** | Consumidor temporal do carimbo e caso que quebra **[não verificado]**. |
| `Colaborador.data_demissao` | Data do desligamento; dia ainda não é “posterior à demissão” no suprajuiz, mas já exclui disponibilidade quando igual ao fim solicitado (`app/colaboradores/services/desligamento.py:105`; `app/ponto/supra_juiz.py:194`; `app/ponto/utils.py:180`). | **2 caminhos abertos**: agendamento `app/colaboradores/services/desligamento.py:85`; efetivação `:105`. Total **[não verificado]**. | **2 amostras**: `app/ponto/supra_juiz.py:194`; `app/ponto/utils.py:180`. | **não sei** | Demissão em 20/11 e cobertura terminando em 20/11 → disponibilidade exclui; outro leitor só considera posterior a partir de 21/11. Se é política específica de disponibilidade ou discordância do último dia útil **[não verificado]**. |
| `Justificativa.aprovada_em` | Instante da decisão, incluindo rejeição (`app/ponto/views.py:1798`, `:1877`). | **3 sítios abertos**: `app/ponto/views.py:1798`, `:1877`; `app/ponto/services/justificativa.py:370`. Total **[não verificado]**. | Leitor semântico do valor não fechado **[não verificado]**. | **não sei** | Caso que quebra **[não verificado]**. |
| `Justificativa.atualizado_em` | Última gravação automática × card de decisões “hoje” (`app/ponto/models.py:180`; `app/ponto/views.py:1738`, `:1740`). | **1 mecanismo automático**; acionadores completos **[não verificado]**. | **1 filtro lido**: `app/ponto/views.py:1740`. | **não sei** | Uma alteração posterior em justificativa decidida poderia deslocar o card; caminho concreto posterior não demonstrado **[não verificado]**. |
| `montar_lampadas.data`, incluído nos 180 parâmetros sem fechamento | Dia âncora; com `hi==hf`, a lâmpada final fica em D, enquanto o encadeamento da autoridade produz D+1 (`app/ponto/management/commands/gerar_celulas.py:67`; `app/escala/servico_jornada.py:82`). | **1 produtor aberto**: `app/ponto/management/commands/gerar_celulas.py:36`. | **1 consumidor aberto**, protegido por veto à igualdade (`app/ponto/management/commands/reconciliar_grade.py:159`, `:169`; `app/ponto/marcos.py:108`). | **não sei** | D=06/11, `hi=hf=15:00` → lâmpada final 06/11 15:00 versus autoridade 07/11 15:00. O leitor examinado não alcança o caso; outro consumidor desprotegido **[não verificado]**. |

**Demais 17 campos: sem divergência nas amostras, sem “não” universal.** São os seis campos examinados de `CelulaDia` (`data`, `regenerada_em`, `editada_em`, `julgada_em`, `veredito_em`, `insumos_em`), `FuroDiario.data`, `DecisaoHE.revogada_em`, os dois campos de `TurnoMaterializado`, `OrdemSubstituicao.data_inicio`, `Justificativa.saida_informada/criado_em`, `Ausencia.aprovada_em/prazo_documento`, `PingGeo.timestamp` e `Colaborador.notificacao_negada_em`. As amostras incluem confrontos de carimbos em `app/escala/services/regeneracao.py:84`, vigência em `app/ponto/models.py:1257`, jornada/fim previsto em `app/ponto/management/commands/processar_alertas_turno.py:97`, `:100`, materialização da saída em `app/ponto/views.py:1810`, prazo em `:2555`, ordenação dos pings em `:3008` e presença do carimbo de negativa em `:1959`; a totalidade dos escritores globais desses 17 campos permanece **[não verificado]**.

**Demais parâmetros:** dos **180 sem fechamento**, incluindo `montar_lampadas.data` acima, não foi exaurida a origem de todos os argumentos; ausência de contradição no corpo não prova concordância de todos os fornecedores **[não verificado]**. Há também nomes não temporais: sete `dia` são dicionários da grade (`app/ponto/propositor.py:34`; `app/ponto/turnos.py:2644`; `app/ponto/status_dia.py:166`; `app/ponto/supra_juiz.py:60`, `:65`, `:109`, `:204`), além de outros dois dicionários nos comandos (`app/ponto/management/commands/e6_oraculo.py:203`, `:230`); `dia_de_trabalho` é booleano (`app/ponto/calculador/regras.py:577`).

## 06/11 19:25 · sessao 8ee5489b7bff · - · - · origem=Ronald (tmux)

Censo somente leitura, reproduzido com `python3 -B`, carregando `bin/quem_chama.py` por `exec(compile(...))` e preservando tudo em memória.

1. **Cabeçalho emitido: 31 sítios únicos.** O conjunto é recolhido por `varrer_py` e deduplicado no render. (`bin/quem_chama.py:731`, `bin/quem_chama.py:1130`)

   - `app/api/tests/test_me_posto_geo.py:113,126`
   - `app/chamados/tests/test_contract_crons.py:22`
   - `app/chamados/tests/test_o137_esmeril_chamado.py:160`
   - `app/chamados/tests/test_selo_minimo_modulos.py:54`
   - `app/core/erros.py:51`
   - `app/core/intake.py:487,494`
   - `app/core/management/commands/describe.py:80`
   - `app/core/tests/test_contract_direcao_a16.py:157`
   - `app/core/tests/test_contract_import_local_resolve.py:75,86`
   - `app/core/tests/test_marca_cron15.py:64`
   - `app/core/tests/test_selo_contratos_estruturais.py:24,76,86,93,113,116,126,179,202,204,207`
   - `app/core/tests/test_selo_estrutural.py:28,31`
   - `app/escala/tests/test_selo_grade_pede_o_instante.py:207`
   - `app/ponto/tests/test_a4_estados_com_porta.py:191`
   - `bin/leitor_dia_estado.py:32`
   - `bin/prova_de_casca.py:86`
   - `bin/recusa_estado.py:31`

   “Opaco” **não significa ausência de literal no arquivo**: `bin/prova_de_casca.py:68` fornece nomes completos, `bin/leitor_dia_estado.py:25` fornece basename e `app/chamados/tests/test_selo_minimo_modulos.py:23` fornece módulos completos. Essas chamadas entram no cabeçalho porque sua expressão de argumento não foi resolvida; os mesmos arquivos ainda podem ligar os alvos lexicalmente. (`bin/quem_chama.py:695`, `bin/quem_chama.py:781`, `bin/quem_chama.py:785`)

2. **Omissões operacionais do cabeçalho: quatro `exec`.** Remover somente a condição `nome not in ('exec', 'compile')` adiciona exatamente estes quatro sítios, levando o conjunto a 35. (`bin/quem_chama.py:689`)

   - `app/core/tests/test_access_log_persistido.py:76`
   - `app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:43`
   - `app/ponto/tests/test_o219_versao_da_regra.py:253`
   - `bin/sondas_rollback.py:53`

   Nem todos carregam um módulo inteiro: os dois testes de `ponto` executam fragmentos AST. O caso `test_access_log_persistido` executa as configurações nomeadas no próprio arquivo, que continuam ligadas pelos basenames; portanto a omissão do cabeçalho não prova zero nesse caso. (`app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:38`, `app/ponto/tests/test_o219_versao_da_regra.py:247`, `app/core/tests/test_access_log_persistido.py:37`, `app/core/tests/test_access_log_persistido.py:73`)

3. **Prefixos reais: sete ocorrências.**

   - `config.settings.` nos três testes: `app/core/tests/test_selo_erro_com_mensagem.py:82`, `app/core/tests/test_selo_log_nao_e_cego.py:122`, `app/core/tests/test_trilha_com_contexto.py:105`.
   - `ponto.management.commands.`: `app/ponto/tests/test_detectores_isencao.py:8`.
   - `relatorios.lote_espelho`: `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:68,126`.
   - Caminho `sonda:`: `bin/sondas_rollback.py:53`.

   Os dois últimos prefixos não acrescentam falso alvo real observado: o único módulo começando por `relatorios.lote_espelho` é `app/relatorios/lote_espelho.py`; nenhum caminho-alvo começa por `sonda:`. Existência/comparação verificadas pela cópia; execução real desses caminhos [não verificado].

4. **Falsos positivos por sítio.**

   Os três testes de settings enumeram apenas `base`, `saas`, `prod`, mas associam também os seis perfis `ci`, `ci_tenants`, `dev`, `smoke`, `sombra`, `teste_motor`. O pacote `__init__` não deve ser contado como falso: importar filhos carrega o pai. Essas facetas de símbolo já eram não varridas por serem settings; o prefixo não muda seu tipo. (`app/core/tests/test_selo_erro_com_mensagem.py:80`, `app/core/tests/test_selo_log_nao_e_cego.py:120`, `app/core/tests/test_trilha_com_contexto.py:103`, `bin/quem_chama.py:384`)

   O teste dos detectores chama somente três commands: `detectar_ausencias`, `processar_alertas_turno`, `processar_alertas_avancados`. O prefixo associa os 104 arquivos `.py` existentes naquele diretório: três chamados, o pacote pai e **100 atribuições falsas àquele sítio**. Desses 100, 50 têm algum símbolo originalmente varrível; os demais contêm apenas `Command`/membros ou nenhum símbolo, sem mudança para LOADER. (`app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`, `app/ponto/tests/test_detectores_isencao.py:15`, `app/ponto/tests/test_detectores_isencao.py:18`, `bin/quem_chama.py:380`, `bin/quem_chama.py:382`)

5. **Rebaixamentos efetivamente novos na simulação com todos os commands tocados.**

   Montei em memória um range hipotético alterando todas as linhas dos 104 commands, com o restante da árvore fornecido pela cópia. **125 símbolos de 49 arquivos** ficam LOADER exclusivamente pelos achados de prefixo. Desses, 121 símbolos em **47 arquivos falsamente atribuídos ao sítio**; os outros quatro símbolos pertencem aos dois commands efetivamente carregados `detectar_ausencias` e `processar_alertas_turno`. Isso é simulação sobre a cópia, não o RESUMO histórico do range real. (`bin/quem_chama.py:739`, `bin/quem_chama.py:1089`)

   Dos 50 candidatos já enviados ao pai, excluir `detectar_par_relampago.py`, `e6_oraculo.py` e `reconciliar_grade.py`: já recebem LOADER por outros sítios, portanto seu tipo não passa a LOADER exclusivamente pelo prefixo.

6. **Três zeros estáticos reais da cópia ficam escondidos por associação falsa de prefixo.** Ao retirar somente os achados “por nome/caminho montado”, estes três símbolos voltam a zero:

   - `app/ponto/management/commands/gerar_celulas.py:19` — `_chaves_marcos`.
   - `app/ponto/management/commands/passivo_disputa_retro.py:45` — `_competencia_de`.
   - `app/ponto/management/commands/testar_pwa.py:27` — `WARN`.

   Os dois primeiros nomes aparecem somente em suas definições no universo `app/**, bin/**`, segundo busca textual na cópia. O sítio `test_detectores_isencao.py:8` não carrega esses commands. O quarto zero estático escondido é `processar_alertas_turno.py:26::TIPOS_POS`, mas esse módulo é efetivamente carregado pelo teste, logo seu rebaixamento é correto. (`app/ponto/tests/test_detectores_isencao.py:15`)

   **Ausência de consumidores externos/dinâmicos em produção e “zero verdadeiro” de produção: [não verificado].** Há prova de zero do próprio contador na cópia e de que o prefixo falso o remove, suficiente para documentar o custo admitido pela triagem.

7. **Zeros globais do range real e quantidade de alvos carregados exclusivamente por opacos externos: [não verificado].** A cópia não contém `.git`; o diff fornecido toca apenas o instrumento e seu selo, não todos os módulos candidatos. Não confundir os 31 sítios emitidos com 31 falsos zeros: vários possuem literais locais suficientes, e outros visam testes/settings já fora da contagem. (`bin/quem_chama.py:360`, `bin/quem_chama.py:384`, `bin/quem_chama.py:781`, `bin/quem_chama.py:785`)

## 06/11 19:23 · sessao c91fdee52e15 · - · - · origem=Ronald (tmux)

Achados novos do bloco, por leitura estática; não há medida de incidência em PROD [nao verificado].

1. **`Justificativa.data_turno`: divergente.** `triagem_bater_ponto` lê todas as batidas, sem recorte pelo dia, e guarda a data civil local da entrada no contexto; `ramo_regularizar_saida` grava essa data em `Justificativa.data_turno` (`app/ponto/services/triagem_batida.py:175`, `:178`, `:180`, `:185`, `:239`, `:260`). O leitor `efeitos_da_ausencia` usa o campo como dia a justificar no intervalo da folha (`app/ponto/services/efeito_ausencia.py:40`, `:43`); `recalcular_fechamento_mes` também (`app/ponto/services/fechamento.py:651`, `:654`). Caso: entrada **21/09/2026 00:00 −03**, turno previsto da véspera **23:50–07:50**, sem turno anterior ocupando a véspera: `_data_do_turno` responde **20/09**, mas a triagem grava **21/09** (`app/ponto/turnos.py:283`, `:299`, `:306`). Depois de aprovada, consultar justificadas de **20/09–20/09** exclui essa justificativa (`app/ponto/services/efeito_ausencia.py:43`). A aprovação ainda invalida o fechamento pelo dia **21/09** (`app/ponto/services/justificativa.py:200`). Família do leitor: **folha/export**; origem da discordância: **turno/marcos**, conforme `app/core/contratos_estruturais.py:48`, `:52` e `app/docs/JUIZES.md:20`.

   Acessibilidade: `_tad` só muda o contexto `turno_aberto`; não retira a entrada de `entradas_sem_saida`, cuja guarda é duração superior a meia hora (`app/ponto/services/triagem_batida.py:191`, `:199`, `:205`). **Não atribuir esse mesmo exemplo ao escritor `regularizacao.py:94`**: seu recorte anterior pode recusar antes da gravação [nao verificado neste bloco].

2. **Dia produzido pela fila de flip: divergente, novo par para `Batida.timestamp`.** `fila_flip_recusas` produz `dia=localtime(timestamp).date()` — dia civil (`app/ponto/services/flip_auto.py:138`, `:140`). `avaliar_fila` entrega esse dia a `dia_encerrado`, que responde pelo dia do turno (`app/ponto/services/flip_auto.py:268`, `:285`; `app/ponto/turnos.py:2043`, `:2046`). Caso: vizinha **S21/09 07:00**, pertencente ao turno **20/09 19:00–21/09 07:00**, já fechado; agora **21/09 08:00**, com próximo turno previsto **21/09 19:00–22/09 07:00**. A fila pergunta por **21/09**, registra `dia_em_curso` e pula o item; perguntou pelo turno seguinte (`app/ponto/services/flip_auto.py:285`, `:286`). A primeira fonte também produz dia civil e o consome como dia do turno em `batidas_do_dia_turno` (`app/ponto/services/flip_auto.py:63`, `:104`). Família: **turno/marcos** (`app/docs/JUIZES.md:20`).

   O parâmetro **`flip_auto.etiquetas_pela_celula.dia`** participa da mesma mistura: recebe o dia civil da fila, busca DNA desse dia e só tenta a véspera quando a célula consultada diz folga (`app/ponto/services/flip_auto.py:201`, `:208`, `:214`, `:310`).

3. **Payload `data_turno` + hora de marco em `lastro.julgar`: divergente, par novo C9.** A emissão recebe o dia da célula e os marcos faltantes; grava `data_turno` e `marcos_faltantes` juntos (`app/ponto/services/cartorio.py:1778`, `:1779`; `app/ponto/management/commands/detectar_ausencias.py:706`, `:709`). O fallback de `lastro.julgar` combina toda hora com aquele mesmo dia civil, sem encadear a madrugada (`app/ponto/services/lastro.py:304`, `:305`, `:306`, `:308`). Caso: `data_turno=20/09/2026`, marco **S07:00**, batidas **E20/09 19:00**, **S21/09 07:00**, sem lâmpada correspondente na ata: o alvo vira **20/09 07:00**, a saída fica a **1.440 min**, excedendo a tolerância de **90 min**, e o leitor classifica `outro_horario` (`app/ponto/services/lastro.py:59`, `:289`, `:299`, `:308`, `:320`). Família do payload: **chamado**; geometria discordante: **turno/marcos** (`app/docs/JUIZES.md:24`, `:56`). O fallback é condicional à ausência da lâmpada; com lâmpada encontrada, prevalece sua leitura (`app/ponto/services/lastro.py:289`, `:298`).

**Campos distintos encontrados neste bloco: 19**, contando os três de `MetricaSnapshot` e `LogAuditoria.criado_em`, ausentes das linhas CAMPO do anexo, mas usados pelos serviços (`app/inteligencia/models.py:90`, `:91`, `:96`; `app/ponto/services/retratar_humano.py:39`):

| Campo | Leitura/escrita examinada; resultado local |
|---|---|
| `Batida.timestamp` | Divergência da fila flip acima; outros usos locais convertem para hora local ou comparam instantes (`app/ponto/services/flip_auto.py:138`, `:140`, `:285`; `app/ponto/services/triagem_batida.py:669`, `:674`). |
| `Batida.retratada_em` | Instante usado como presença/ausência e prazo de sete dias; borda inclusiva coerente entre botão e execução (`app/ponto/services/retratar_humano.py:53`, `:109`, `:203`). |
| `Justificativa.data_turno` | Divergente acima (`app/ponto/services/triagem_batida.py:260`). |
| `Justificativa.saida_informada` | Datetime local aware, com data explícita ou virada do dia; sem divergência observada nesse escritor (`app/ponto/services/triagem_batida.py:247`, `:254`, `:256`, `:264`). |
| `EscalaColaborador.data_inicio` | Ordenação por início de vigência (`app/ponto/services/faltas_de_hoje.py:36`). |
| `EscalaColaborador.data_fim` | Teste de ausência de fim (`app/ponto/services/cluster_espurio.py:119`). |
| `EscalaColaborador.regua_corroborada_em` | Carimbo devolvido sem transformação (`app/ponto/services/furos_vetados.py:70`). |
| `CelulaDia.data` | Dia da célula; intervalos inclusivos e consultas por igualdade coerentes nos sítios amostrados (`app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/vigia_entrada.py:120`; `app/ponto/services/passivo_s84.py:94`). |
| `CelulaDia.julgada_em` | Instante repassado ao julgamento anterior (`app/ponto/services/cluster_espurio.py:104`). |
| `FuroDiario.data` | Dia da lâmpada, intervalo inclusivo (`app/ponto/services/deslogados.py:99`, `:101`). |
| `TurnoMaterializado.data_turno` | Dia do turno usado para recorte de recomputação (`app/ponto/services/flip_batida.py:38`, `:39`). |
| `PerguntaDisputa.validada_em` | Instante: corte móvel e procedência (`app/ponto/services/flip_auto.py:126`, `:130`; `app/ponto/services/procedencia_batida.py:74`). |
| `PerguntaDisputa.materializada_em` | Instante alternativo de procedência (`app/ponto/services/procedencia_batida.py:74`). |
| `ChamadoColaborador.criado_em` | Janelas de instante e apresentação local nos sítios lidos (`app/ponto/services/triagem_batida.py:455`; `app/ponto/services/faltas_de_hoje.py:84`). |
| `ChamadoColaborador.prazo_sla` | Produzido pelo juiz do prazo sobre `timezone.now()` (`app/ponto/services/triagem_batida.py:346`, `:469`). |
| `LogAuditoria.criado_em` | Ordenação por instante, exibição em localtime (`app/ponto/services/retratar_humano.py:39`, `:94`; `app/ponto/services/flip_batida.py:77`). |
| `MetricaSnapshot.data_ini` | Dia da medição nestas duas chaves (`app/ponto/services/teto_na_lista.py:115`; `app/ponto/services/vinculo_na_lista.py:120`). |
| `MetricaSnapshot.data_ref` | Dia da medição e ordenação por data (`app/ponto/services/teto_na_lista.py:114`, `:124`; `app/ponto/services/vinculo_na_lista.py:119`, `:129`). |
| `MetricaSnapshot.calculado_em` | Instante automático e apresentação ISO (`app/inteligencia/models.py:96`; `app/ponto/services/teto_na_lista.py:128`; `app/ponto/services/vinculo_na_lista.py:133`). |

Os **17 campos restantes** não apresentaram divergência nos sítios locais acima; a unanimidade de todos os escritores externos desses campos não foi certificada por este explorador [nao verificado].

Parâmetros do inventário neste bloco: **10**, já enviados com identidades; **1 divergente** (`etiquetas_pela_celula.dia`), **9 sem divergência local observada**. Esses nove são `furos_vetados.{vinculo_do_emissor.data,motivo_do_veto.data,furos_vetados_por_regua.ini/fim}`, `lastro.{marcos_acesos_do_dia.data,_texto_da_nota.dia}`, `passivo_s84.competencia_de.dia`, `simular_dia.grade_dia.data`, `vigia_entrada.do_dia.data` (`app/ponto/services/furos_vetados.py:18`, `:27`, `:47`, `:61`; `app/ponto/services/lastro.py:185`, `:193`, `:343`, `:354`; `app/ponto/services/passivo_s84.py:38`, `:46`; `app/ponto/services/simular_dia.py:5`, `:7`; `app/ponto/services/vigia_entrada.py:111`, `:120`).

## 06/11 19:23 · sessao 4d7422dcc947 · - · - · origem=Ronald (tmux)

Recorte auditado: 18 arquivos atribuídos, 33 parâmetros temporais, sendo 17 `dia/data_*` e 16 `data/ini/fim`; identidades e declarações constam nos sítios listados abaixo. Contagens referem funções distintas deste recorte, não execuções. [nao verificado: medição por AST em memória]

| Campo/parâmetro | Divergência e caso concreto | Escritores no recorte | Leitores no recorte |
|---|---|---|---|
| `Ausencia.data_fim` | `None` significa intervalo aberto para `TIPOS_EM_ABERTO`, mas leitores o reduzem ao início. Ausência aberta em 01/10, consulta 01–06/11 → `decididos_do_periodo` devolve `{}` e relatório em lote devolve `[]`. Escrita: `app/ponto/services/ausencia.py:70`, `:197`, `:343`; leituras: `app/ponto/services/dia_decidido.py:552`, `:558`; `app/ponto/services/atestados_lote.py:58`. | 2 explícitos: `criar_ausencia`, `editar_ausencia_registro` — `app/ponto/services/ausencia.py:343`, `:500`. `mudar_estado:834` também repassa o campo ao `save`, cuja derivação está em `app/ponto/models.py:902`. | 8 diretos: criar `ausencia.py:286`; editar `:497`; abrir chamado `:652`; decidir `:947`; decididos `dia_decidido.py:552`; linha do tempo `:616`; vigia `vigia_ausencia.py:92`; censo `vigia_ausencia_veredito.py:48`. Mais leitor derivado: `atestados_lote.py:58`. |
| `Ausencia.aprovada_em` | Instante aware entregue cru ao CSV, enquanto painel o apresenta em fuso local. `2026-11-06T01:30Z` → CSV `06/11/2026 01:30`, painel `05/11/2026 22:30`. Escrita: `app/ponto/services/ausencia.py:349`; transporte: `app/ponto/services/dia_decidido.py:508`; CSV: `app/ponto/services/atestados_lote.py:131`; painel: `app/templates/ponto/partials/painel_ausencia.html:261`; fuso/configuração: `app/config/settings/base.py:93`, `:95`. | 2: criar `ausencia.py:349`; decidir `:917` — este aceita carimbo passado pelo chamador. O modelo auto-carimba quando ausente: `app/ponto/models.py:873`. | 1 direto: `quem_quando`, `dia_decidido.py:508`; mais CSV derivado `atestados_lote.py:131`. |
| `Ausencia.criado_em` | Mesmo desacordo UTC/local quando `aprovada_em` é nulo, pois vira o carimbo do CSV. Mesmo caso concreto acima. Fallback: `app/ponto/services/dia_decidido.py:508`; CSV: `app/ponto/services/atestados_lote.py:131`; painel do mesmo DTO: `app/templates/ponto/partials/painel_ausencia.html:188`. | 0 explícitos no recorte; `auto_now_add` em `app/ponto/models.py:813`. | 2 diretos: `quem_quando`, `dia_decidido.py:508`; `vigia_ausencia.contadores`, `vigia_ausencia.py:109`, `:188`, `:193`. Mais CSV derivado `atestados_lote.py:131`. |
| `_bateu_no_dia.dia` | Dia civil é transformado em janela terminando na meia-noite seguinte, mas o juiz inclui essa ponta. Batida única em **07/11/2026 00:00 local**, consulta por **06/11** → `True`; consumidor pode excluir a pessoa como quem “já bateu hoje”. Montagem: `app/ponto/services/abriu_nao_bateu.py:172`; fim inclusivo: `app/ponto/turnos.py:2868`; exclusão: `app/ponto/services/abriu_nao_bateu.py:130`. | 1 produtor da janela: `app/ponto/services/abriu_nao_bateu.py:172`. | 1 consumidor direto do parâmetro: mesma função, chamada `:174`; efeito em `com_nomes:130`. |

Os três casos de CSV/ausência aberta foram caracterizados usando os corpos reais extraídos por AST, com objetos e cargas em memória; nenhum Django, banco, log ou arquivo foi escrito. [nao verificado: execução da caracterização sem artefato persistido]

Pares divergentes e famílias:

- **`ausencia/ferias`**: escritor `app/ponto/services/ausencia.py:343` × leitores `app/ponto/services/dia_decidido.py:552`, `:558` e `app/ponto/services/atestados_lote.py:58`. Família declarada em `app/core/contratos_estruturais.py:126`.
- **`ausencia/ferias`**: escritor `app/ponto/services/ausencia.py:349` e auto-carimbo `app/ponto/models.py:873` × leitor `app/ponto/services/atestados_lote.py:131`, via `app/ponto/services/dia_decidido.py:508`. Família: `app/core/contratos_estruturais.py:126`.
- **`ausencia/ferias`**: escritor automático `app/ponto/models.py:813` × leitor CSV `app/ponto/services/atestados_lote.py:131`, pelo fallback `app/ponto/services/dia_decidido.py:508`. Família: `app/core/contratos_estruturais.py:126`.
- **`batida`**, com borda de janela: escritor do instante `app/ponto/registro_batida.py:158` × leitor `app/ponto/services/abriu_nao_bateu.py:172`, que chama o intervalo inclusivo de `app/ponto/turnos.py:2868`. Família declarada em `app/core/contratos_estruturais.py:46`.

Há um desacordo adicional de **dia civil × dia do turno** em `regularizacao`, mas o caso é barrado antes da escrita: `app/ponto/services/regularizacao.py:60` começa a busca na data civil da entrada; `turnos_do_colab` filtra por data do turno (`app/ponto/turnos.py:1849`, `:1993`). Para E=21/09 00:00 local e turno da véspera 23:50–07:50, o juiz atribui 20/09 (`app/ponto/turnos.py:299`, `:306`); a busca iniciada em 21/09 perde o turno e responde “Esta batida nao inicia um turno aberto” (`app/ponto/services/regularizacao.py:64`). Não afirmar que `Justificativa.data_turno` errado foi gravado: esse ramo não chega à escrita `regularizacao.py:94`.

Identidades examinadas para união pelo pai:

- `Batida.timestamp`, `Batida.retratada_em`: `app/ponto/services/adesao.py:35`, `:45`; `AlertaTurno.data_turno`: `app/ponto/services/alerta_turno.py:25`.
- `Ausencia.data/data_fim/prazo_documento/aprovada_em/atualizado_em/criado_em`: `app/ponto/services/ausencia.py:342`, `:343`, `:345`, `:349`, `:1283`; `app/ponto/services/vigia_ausencia.py:109`.
- `AgendamentoFerias.data_inicio/data_fim`: `app/ponto/services/ausencia.py:1184`.
- `Justificativa.data_turno/saida_informada/aprovada_em/atualizado_em`: `app/ponto/services/regularizacao.py:94`, `:97`, `:106`; `app/ponto/services/justificativa.py:192`.
- `ChamadoColaborador.criado_em`: `app/ponto/services/justificativa.py:54`; `DisputaSupervisao.fechada_em`: `app/ponto/services/entrada_sincronizada.py:56`; `CelulaDia.data`: `app/ponto/services/vigia_ausencia_veredito.py:53`.
- Descobertos fora da lista CAMPO fornecida: `MetricaSnapshot.data_ref/calculado_em`, declarados em `app/inteligencia/models.py:91`, `:96`, lidos em `app/ponto/services/abriu_nao_bateu.py:157`, `:161`.

Nos 33 parâmetros, apenas `_bateu_no_dia.dia` apresentou divergência própria comprovada; os erros de intervalo aberto pertencem ao significado de `Ausencia.data_fim`, sem tornar `data_ini/data_fim` dos leitores unidades diferentes. [nao verificado: classificação consolidada do recorte]

## 06/11 19:23 · sessao 53ef591127f2 · - · - · origem=Ronald (tmux)

Achados e mapa para consolidação, todos em `bin/quem_chama.py`.

1. **BLOQUEIA — valores completos montados são calculados e descartados.** `_prefixo` retorna `('pkg.mod', True)` para `'pkg.'+'mod'` (`:554–559`); o ramo nome só emite associação completa para `__import__` com fromlist ou relativo (`:703–718`). A recuperação lexical posterior exige um `Constant` inteiro (`:779–782`). Reproduzi `importlib.import_module('pkg.'+'mod').f()` contra `app/pkg/mod.py::f`: arquivo e símbolo com zero, RESUMO com dois zeros. Equivalente em caminho: `runpy.run_path('app/pkg/mo'+'d.py')`; `:684–685` só marca `literal=True`, não emite o caminho calculado, e `:775–778` não encontra literal completo/basename.

2. **BLOQUEIA — `getattr` direto foi coberto, seus apelidos não.** `:600–601` reconhece apenas `getattr(...)(...)` imediato; apelidos de loader só vêm de `ImportFrom` (`:607–614`). Reproduzi `im=getattr(importlib,'import_module'); im('pkg.mod').f()`: símbolo zero, OPACO vazio. Também `g=getattr; g(importlib,'import_module')('pkg.mod').f()` deixa símbolo zero. Formas `from importlib import import_module as im`, atributos `il.import_module`, e `getattr(il,'import_module')(literal)` funcionam pelos mesmos trechos.

3. **BLOQUEIA — exclusão de atributos builtin também exclui builtins verdadeiros.** `:602–603` devolve `None` para qualquer atributo `exec/compile`, protegendo `re.compile`, mas também escondendo `builtins.exec` e `builtins.compile`. Reproduzi `builtins.exec(open('app/pkg/mod.py').read(),G); G['f']()`: símbolo zero, OPACO vazio. Texto usa apenas `LOADERS_CAMINHO` (`:111/:487`), portanto não resolve `exec(open(...))` embutido; AST usa o conjunto expandido (`:106/:618`).

4. **BLOQUEIA — subprocesso só aciona o detector lexical antigo.** `:666–671` marca “caminho” pela simples presença de `sys.executable` ou string de interpretador; não extrai expressão de script nem registra opacidade. Reproduzi `x='d.py'; subprocess.run(['python3','app/pkg/mo'+x])`: ambas facetas zero, OPACO zero. `import sys as s; subprocess.run([s.executable,'app/pkg/mod.py'])` deixa símbolo zero, pois `_cadeia` precisa resultar exatamente `sys.executable` (`:666`). Literais completos com `python3` e `sys.executable` são protegidos por `:775–778`.

5. **BLOQUEIA — “ponto fixo” não é ponto fixo completo e exclui wrappers usuais.** Candidatos exigem token de loader builtin no próprio arquivo (`:1038–1040`), antes de descobrir wrappers; há teto de três rodadas (`:1041`). Reproduzi:
   - `one(path)->run_path(path)` em `one.py`, `two(path)->one(path)` em `two.py` sem token builtin, e chamada `two('app/pkg/mod.py')`: símbolo zero.
   - `from one import one as x; x('app/pkg/mod.py')`: símbolo zero; alias de wrapper não é resolvido.
   - cadeia com quatro wrappers, todos passando o filtro por comentário contendo `run_path`: quarto wrapper deixa símbolo zero.
   
   `extra` também guarda somente `fn.name→kind` (`:639`), e `novo_.update` sobrescreve colisões entre arquivos (`:1044`). Dois `load`, um de caminho e outro de nome, podem classificar o chamador pelo tipo errado e produzir símbolo zero.

6. **BLOQUEIA — prefixo de caminho relativo à raiz `app` pode não ligar nenhum alvo.** `join(APP_DIR,'pkg',leaf)` perde a raiz não literal e produz `'pkg/'` (`:569–578`). `sob_prefixo` compara esse prefixo diretamente a `a['rel']`, sem adicionar `app/` (`:509–513`). Com `APP_DIR='app'`, folha externa `mod.py` e alvo `app/pkg/mod.py`, a execução carrega o alvo, mas o prefixo não o encontra. `_loaders_py` considera resolvido e não registra OPACO (`:686–690`).

7. **BLOQUEIA/FILA — limites adicionais de atribuição/relativo.** `_valores` escreve apenas `Assign` com alvo `Name`, resolve dois saltos e não percorre expressões internas (`:651–663`). Reproduzi `a='pkg.'+input(); b=a; c=b; import_module(c)` e `x: str='pkg.'+input(); import_module(x)`: ambos deixam duas facetas zero e OPACO. O nome relativo montado `import_module('.'+leaf,package='pkg')` também vira OPACO sem vínculo: prefixo só de pontos é descartado (`:695–701`); package só é usado quando o nome já foi considerado inteiro (`:713–718`). O texto relativo só aceita grafia literal `import_module(...)`, com package literal (`:113/:499–501`).

8. **FILA — `_COM_NOME_NA_FRENTE` não normaliza os keywords dos loaders.** A exclusão só remove primeiro posicional; keywords excluem somente `name` (`:586–587`). Reproduzi `SourceFileLoader(fullname='unused',path=external)` → `opacos=[]`: `fullname` é interpretado como caminho literal (`:684–690`). Isso cala o aviso mesmo sem caminho conhecido. Analogamente `run_path(external,run_name='__main__')` trata o nome de execução como evidência de caminho. A emissão efetiva do falso zero para alvo carregado por `external` segue do consumidor lexical `:775–778`, que não recebe o valor externo.

9. **P4 — mapa de escritores e leitores.**

| Campo/constante | Escreve/define | Lê |
|---|---|---|
| `atrib` | `:651/:656` | `_valores :661` |
| `_valores` | `:658–663` | `:682/:695` |
| `Alvos.por_mod` | `:446/:448/:450` | `:500/:736/:780` |
| `Alvos.sob_prefixo` | `:504–513` | `:739` |
| retorno `involucros` | `:639–641` | `:1044` |
| `novo_` | `:1042/:1044` | `:1045/:1047` |
| `extra` | `:1037/:1047` | `:1044/:1054`, `_kind :622/:636/:675` |
| `opacos` local | `:664/:690/:701/:718` | retorno `:719`, recepção `:730`, acumulação `:732` |
| `opacos` global | `:1037/:732` | `:1054/:1096` |
| `a['opacos']` | `:1096` | só `alvos[0]`, `:1129` |
| cabeçalho/contagem de opacos | `:1130–1132` | RESUMO `:1144–1145` não usa isso para proteger zeros |
| `LOADERS_CAMINHO` | `:103–104` | `:106/:111` |
| `LOADERS_CAMINHO_AST` | `:106` | `:612/:618/:1039` |
| `_SO_BUILTIN` | `:107` | `:603` |
| `_COM_NOME_NA_FRENTE` | `:108` | `:586` |
| `_RX_LOADER_CAMINHO` | `:111` | `:487` |
| `_RX_PYTHON` | `:109` | `:669` |
| `_RX_REL_TXT` | `:113` | `:499` |

**Semânticas incompatíveis:** `_prefixo` agora devolve valor completo calculado, mas leitores de `inteiro` ainda presumem que algum literal inteiro será recuperado na varredura de `Constant` (`:684–685/:703–718/:774–782`). `linhas['caminho']` agora significa chamada de loader **ou** mera presença de string `python3`/atributo `sys.executable`; `ldr_txt` apresenta ambos como sítio de loader (`:666–670/:678/:733–734`). OPACO é anunciado como “sem literal no arquivo” (`:1130`), embora seja decidido por argumentos da chamada e exclua `exec/compile` (`:682–701`).

**P1(h):** prefixo vazio de nome faria `sob_prefixo` ligar todos os módulos (`:507–508`), mas os produtores filtram vazio/pontos (`:695–701`); caminho vazio produziria candidatos vazios (`:509–513`). Prefixo de um caractere é aceito e usa `startswith`, sem exigir `.` ou `/` (`:507–513`). Em particular prefixo nome `'a'` liga todo módulo de app por causa da alternativa `'app.'+modulo` (`:508`). Barra final preservada por `partes.append('')` impede `app/mp/` de casar `app/mpx/` (`:510–513`), mas sem barra final o casamento parcial é propositalmente amplo.

## 06/11 19:22 · sessao 2567ac09899c · - · - · origem=Ronald (tmux)

Dois pares novos confirmados para `Batida.timestamp`; ambos no consumidor A2, com escritor de suporte em `app/ponto/registro_batida.py:153` e `:158`.

| Campo | Divergência | Escritores A2 | Leitores no meu lote | Caso concreto |
|---|---|---:|---|---|
| `Batida.timestamp` | Instante aware é interpretado como data crua para selecionar a fatia, embora os limites sejam datas de competência/vínculo: `app/ponto/services/fechamento.py:204`, `:245`, `:252`. O próprio fechamento usa data localizada em outro ramo: `:784`. | 0; escritor de suporte: `app/ponto/registro_batida.py:153`, `:158`. | 7 funções: `fechamento.apurar_por_fatia:245`; `fechamento.recalcular_fechamento_mes:784`; `diagnostico_escala.diagnostico_escala:160`; `esmeril_espelho.assinaturas_do_colab:108`; `espelho.montar_dias:455`; `dias_em_aberto.passivo_da_competencia:122`; `piso_intervalo.pares_abaixo_do_piso:42`. | Fatia 01–03/10/2026, dias próprios `{01,03}`, dia 02 atribuído a outro vínculo. E/S UTC `02/10 00:15`, `02/10 01:15`, equivalentes locais a `01/10 21:15`, `22:15`. O filtro real remove ambas em `fechamento.py:252`; `motor.calcular_mes` recebe lista vazia em `:278`. Efeito monetário final [nao verificado]. |
| Mesmo campo, segundo par | Limite produzido como primeiro instante **posterior** ao período é consumido inclusivamente: `app/ponto/services/dias_em_aberto.py:121`, `:122` × `app/ponto/turnos.py:2868`. | Mesmo acima. | Mesmo conjunto acima. | Competência até 20/10/2026, dez dias em aberto e única batida `21/10 00:00-03:00`. Ela entra no contador (`dias_em_aberto.py:122`, `:129`) e classifica como `quase_nunca_bateu`, enquanto excluí-la resulta em `nunca_bateu` (`:57`, `:60`; limiar dez em `:30`). |

As expressões dos filtros e a função `classificar` foram executadas por extração AST em memória: resultados `2 → 0` batidas no primeiro caso e `quase_nunca_bateu → nunca_bateu` no segundo. Reprodução integrada e ocorrência em PROD [nao verificado].

Pares e famílias:

- `registro_batida.py:153` × `services/fechamento.py:252`: família do campo **batida**, família do sítio consumidor **folha/export**; a matriz nomeia o escritor de Batida em `app/core/contratos_estruturais.py:100` e o fechamento em `:158`.
- `registro_batida.py:158` × `services/dias_em_aberto.py:122`, passando pelo limite inclusivo de `turnos.py:2868`: mesmas famílias; competência e fechamento são encaminhados aos juízes indicados em `app/docs/JUIZES.md:78`.

Campos únicos examinados no meu lote, pertencentes ao inventário: **19** — `Batida.{timestamp,retratada_em}`, `EscalaColaborador.{data_inicio,data_fim}`, `CelulaDia.data`, `FolgaDia.data`, `Colaborador.data_demissao`, `Justificativa.data_turno`, `OrdemSubstituicao.{data_inicio,data_fim,push_enviado_em}`, `DiaPago.{data,lavrado_em}`, `FechamentoMensal.{previsto_em,derivado_em,atualizado_em,aprovado_em}`, `PeriodoFechado.{fechado_em,reaberto_em}`. Identidades sustentadas pelas respectivas referências do inventário fornecido; contagem reproduzível desta enumeração [nao verificado]. [nao verificado]

**1 campo divergente, dois pares novos**; nos demais 18 não confirmei novo par divergente neste lote. Isso não prova concordância universal de escritores/leitores fora do recorte [nao verificado].

Além do inventário, encontrei quatro campos reais usados nesses arquivos: `MetricaSnapshot.{data_ini,data_ref,calculado_em}` (`app/inteligencia/models.py:90`, `:91`, `:96`; escrita `he_pendente_lavrado.py:184`, `:185` e `esmeril_espelho.py:325`; leitura `he_pendente_lavrado.py:204`, `:206` e `esmeril_espelho.py:347`), e `LoteHolerite.publicado_em` (`app/holerite/models.py:30`; leitura `fechamento.py:1523`). Não confirmei divergência nesses quatro; escritores externos completos [nao verificado].

Retiro o candidato de duas `OrdemSubstituicao` ativas em janelas adjacentes: as constraints por substituto/posto impedem esse estado (`app/ponto/models.py:1124`, `:1130`). A disponibilidade usa sobreposição estrita (`app/ponto/utils.py:205`), e a atividade inclui a borda (`app/ponto/models.py:1147`), mas não fechei um par **escritor × leitor** com efeito temporal incorreto alcançável para esses campos [nao verificado].

## 06/11 19:22 · sessao 4346034ea03c · - · - · origem=Ronald (tmux)

Nenhum par temporal novo comprovadamente divergente neste recorte. O dia da jornada é derivado em `app/ponto/turnos.py:284` e `app/ponto/turnos.py:297`, copiado para `TurnoMaterializado` em `app/ponto/nucleo.py:50` e lido como jornada em `app/ponto/status_dia.py:273`. As bordas dos períodos de dias são inclusivas em `app/ponto/turnos.py:1993`, `app/ponto/turnos.py:2163` e `app/ponto/precedencia.py:335`.

**Contagem local:** 13 campos com escritores abertos; 96 slots de parâmetros com corpos examinados, dos quais 50 chamados `data`, `data_*` ou `dia` e 46 chamados `ini`/`fim`. Esta contagem foi obtida por AST somente leitura dos 15 módulos atribuídos; não significa fornecedores/chamadores exaustivamente rastreados. [não verificado quanto à totalidade dos fornecedores]

**Dois campos ficam “não sei”:**

| Campo | Escritores A3 abertos | Leitores | Motivo |
|---|---|---|---|
| `EventoCelula.data` | 1: copia `cel.data`, `app/ponto/portas/celula.py:891` | Nenhum leitor semântico encontrado; o consumidor trabalha por colaborador, `app/chamados/reconciliador.py:1340` | Sem leitor para confrontar; total global de leitores [não verificado]. |
| `FuroDiario.avaliada_em` | 1: `timezone.now()`, `app/ponto/portas/celula.py:1127` | Nenhum leitor efetivo encontrado; ranking filtra a **data** em `app/escala/services/furos_diarios.py:133` | A documentação diz “apurado em”, `app/ponto/portas/celula.py:31`, mas consumidor desse carimbo [não verificado]. |

**Outros 11 campos, sem divergência nas amostras lidas:**

| Campo | Escritores A3: n e linhas | Amostra de leitores aberta |
|---|---|---|
| `CelulaDia.data` | 3: `app/ponto/portas/celula.py:259`, `:600`, `:850` | Chave de dia: `app/ponto/turnos.py:1584`; período fechado: `app/ponto/portas/celula.py:219`. |
| `CelulaDia.regenerada_em` | 2: `app/ponto/portas/celula.py:274`, `:792` | Instante de reescrita, comparação estrita: `app/escala/services/regeneracao.py:84`, `:120`. |
| `CelulaDia.editada_em` | 2: `app/ponto/portas/celula.py:601`, `:607` | Mesmo confronto: `app/escala/services/regeneracao.py:84`, `:120`. |
| `CelulaDia.julgada_em` | 1: `app/ponto/portas/celula.py:1060` | Marco de invalidação: `app/chamados/veredito_lavrado.py:154`; existência de julgamento: `app/ponto/management/commands/vigia_b5_ausencia.py:55`. |
| `CelulaDia.veredito_em` | 1: `app/ponto/portas/celula.py:1032` | Marco de invalidação: `app/chamados/veredito_lavrado.py:153`. |
| `CelulaDia.insumos_em` | 2: `app/ponto/portas/celula.py:1064`, `:1100` | Distingue insumos examinados de ausentes: `app/ponto/management/commands/vigia_b5_ausencia.py:71`. |
| `FuroDiario.data` | 1: `app/ponto/portas/celula.py:1125` | Dia da grade produzido em `app/escala/services/furos_diarios.py:51` e filtrado inclusivamente em `:133`. |
| `DecisaoHE.data` | 1: `app/ponto/portas/he.py:67` | Chave `date` no cálculo: `app/ponto/motor_calculo_v2.py:1417`. |
| `DecisaoHE.revogada_em` | 2: limpa em `app/ponto/portas/he.py:83`; marca em `:373` | Vigência por nulidade: `app/ponto/models.py:1257`. |
| `TurnoMaterializado.data_turno` | 1: `app/ponto/nucleo.py:50` | Dia da jornada na cobertura: `app/ponto/management/commands/processar_alertas_turno.py:97`. |
| `TurnoMaterializado.dt_fim_previsto` | 1: `app/ponto/nucleo.py:55` | Instante previsto: `app/ponto/selecao_periodo.py:290`; `app/ponto/management/commands/processar_alertas_turno.py:100`. |

Números de escritores acima contam sítios sintáticos A3; leitores globais não foram contados. [não verificado]

**Candidato que não deve virar divergência afirmada:** `date.today()` em `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:816` e `app/ponto/turnos.py:2728` convive com `timezone.localdate()` em `app/ponto/signals.py:122` e `app/ponto/status_dia.py:254`. A configuração declara São Paulo e `USE_TZ=True` em `app/config/settings/base.py:93`; fuso efetivo do processo/SO e eventual ativação de outro fuso [não verificado]. Portanto, diferença de resultados desse candidato [não verificado].

**Parâmetros: atenção ao significado não temporal.** Sete `dia` são dicionários da grade, não datas: `app/ponto/propositor.py:34`, `app/ponto/turnos.py:2644`, `app/ponto/status_dia.py:166`, `app/ponto/supra_juiz.py:60`, `:65`, `:109`, `:204`. Não são divergências entre escritores e leitores: seus corpos usam `.get()`/indexação do dicionário nos respectivos sítios.

**Lista dos 50 slots `data`/`data_*`/`dia`, para consolidar sem duplicar:**

- `turnos.py`: `realizado_dos_turnos:705(data)`, `realizado_do_dia:772(data)`, `dia_encerrado:2031(data)`, `dia_encerrado_motivo:2090(data)`, `_dia_encerrado_puro:2100(data)`, `_dia_encerrado_motivo_puro:2105(data)`, `ausencia_cobre:2536(data_ref)`, `horas_reais_do_dia:2644(dia)`, `afastamentos_no_dia:2705(dia)`, `afastado_hoje:2723(dia)`, `dias_da_ausencia:2774(data,data_fim)`, `batidas_do_dia_turno:3013(dia)`, `turno_aberto_ha:3115(dia)`, `CargaDaJanela.celula:2970(data)`.
- `nucleo.py`: `dt_fim_previsto_de:31(data_turno)`.
- `selecao_periodo.py`: `janela_turno_de:43(data_ref)`.
- `marcos.py`: `marco_vencido:70(data_turno)`, `celula_veta_cobranca:122(dia)`, `_lei_cala_no_dia:166(dia)`.
- `propositor.py`: `dia_degenerado:34(dia)`, `confianca_do_dia:228(data)`, `dia_posicional_completo:325(dia)`.
- `precedencia.py`: `folgas_lancadas_do_periodo:194(data_ini,data_fim)`, `fatos_do_dia:242(data)`, `fatos_do_periodo:259(data_ini,data_fim)`, `veto_de_lancamento:430(data,data_fim)`.
- `status_dia.py`: `fatos_do_status:166(dia,data)`, `status_do_dia_lavrado:201(data)`.
- `supra_juiz.py`: `com_insumo_dia_sem_trabalho:60(dia)`, `_dia_sem_trabalho_pelo_juiz:65(dia)`, `motivo_retratacao:109(dia)`, `cadastro_do_dia:142(data_demissao)`, `_f:160(data)`, `cadastro_do_dia_carregado:166(data)`, `_insumos_do_vinculo:181(data,data_demissao)`, `classificar_dia:204(dia,data)`.
- `portas/celula.py`: `editar_dia:534(dia)`, `regenerar_celulas_dia:703(dia)`, `reverter_regeneracao_dia:740(dia)`.
- `portas/he.py`: `_gravar:44(data)`, `decidir_he:138(data)`, `recusar_ponta_pequena:269(data)`, `revogar_recusa_do_sistema:393(data)`.

Todos os caminhos acima são relativos a `app/ponto/`. Os 46 slots `ini`/`fim` adicionais são: turnos 36, núcleo 6, turno_leitura 2 e portas/celula 2; confira a soma: os 15 slots estritos de turnos mais seus 36 = 51. [contagem AST; arquivos e linhas da lista acima e definições do inventário fornecido]

Não examinei `PerguntaDisputa.data_evento` nesta divisão; nenhum par novo dele foi demonstrado aqui. [não verificado]

_2469 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
