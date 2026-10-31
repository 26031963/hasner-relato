# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 04:44** · publicado em **31/10/2026 04:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 04:44 · sessao 9f8804bc43d8 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — regressão: rollback apaga o redirecionamento que `reabrir()` executa intencionalmente ao recusar.** Com irmão vivo do mesmo turno, `reabrir()` migra resolvedoras, escreve nota indicando onde continuar e registra `reabertura_redirecionada`, então retorna `False` (`app/chamados/models.py:1359`, `app/chamados/models.py:1362`, `app/chamados/models.py:1365`, `app/chamados/models.py:1368`, `app/chamados/models.py:1374`). O novo `atomic` + `set_rollback(True)` desfaz esses efeitos também, embora o comentário diga “nada escrito, como antes” (`app/chamados/views_arquivar.py:65`, `app/chamados/views_arquivar.py:72`, `app/chamados/views_arquivar.py:81`). A migração não marca rollback em seu caminho normal: salva o novo destino e retorna sucesso (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`, `app/chamados/services/fio.py:849`). **Contraexemplo mínimo:** fechado pela tranca com irmão vivo; mesmo sem perguntas migráveis, a nota e a trilha de redirecionamento sempre eram escritas, e agora desaparecem (`app/chamados/models.py:1365`, `app/chamados/models.py:1368`). Persistência anterior demonstrada pelo próprio diff fornecido, que chamava `reabrir()` antes de qualquer `atomic` externo [nao verificado quanto a execução].

2. **PROCEDENTE — lacuna residual: segundo resgate depois de retrancar não gera o novo toque necessário.** `levantar()` guarda o primeiro `resgatado_em`; véu limpo com esse carimbo vira no-op sem trilha (`app/chamados/services/arquivo.py:193`, `app/chamados/services/arquivo.py:196`, `app/chamados/services/arquivo.py:204`). Retrancar atualiza `fechado_em` (`app/ponto/services/fechamento.py:1383`); a regra só protege resgate posterior à nova tranca (`app/chamados/services/competencia_trancada.py:104`) e pode encerrar novamente, sem escrever véu (`app/chamados/models.py:1534`, `app/chamados/models.py:1551`). No segundo clique, não nasce log posterior à nova tranca; C6 continua calando a pergunta apesar da reabertura do chamado (`app/chamados/views_arquivar.py:73`, `app/chamados/views_arquivar.py:80`, `app/chamados/juizes.py:965`, `app/chamados/juizes.py:972`, `app/chamados/juizes.py:1030`). Isso vale também para pergunta pendente, sem novo carimbo de materialização (`app/chamados/juizes.py:1025`).

3. **PROCEDENTE — lacuna residual: toque de desarquivar ignora chamado sem `celula_id`.** O leitor enumera os chamados exclusivamente por célula ligada, embora o juiz do dia reconheça datas pelo contexto (`app/chamados/juizes.py:967`, `app/chamados/juizes.py:968`, `app/chamados/juizes.py:972`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:102`). **Contraexemplo:** chamado da tranca datado pelo contexto, `celula_id=None`, pergunta em dia trancado; `levantar` registra o resgate, mas essa linha não pertence aos `chs` consultados pelo C6 (`app/chamados/services/arquivo.py:209`, `app/chamados/juizes.py:968`). O novo teste liga explicitamente a célula ao chamado, cobrindo somente o caso favorável (`app/chamados/tests/test_competencia_trancada.py:188`).

4. **DUVIDA — dependência obrigatória construída sobre trilha best-effort.** `cobrar_dia` prossegue sem verificar se o registro foi criado (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:39`); `registrar_log` engole exceções e `evento` usa savepoint próprio, devolvendo `None` se falhar (`app/core/auditoria.py:17`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`). `levantar` igualmente retorna `True` sem confirmar a trilha (`app/chamados/services/arquivo.py:208`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:210`). **Contraexemplo condicional:** falha isolada na criação de `LogAuditoria`; o negócio continua, mas C6 não reconhece toque e não ressuscita (`app/chamados/juizes.py:969`, `app/chamados/juizes.py:1030`). Ocorrência de tal falha em produção [nao verificado].

5. **DUVIDA — memo não invalidado após toque.** O resultado é cacheado na request, ou no colaborador fora dela, e retorna antes de consultar novos logs (`app/chamados/juizes.py:943`, `app/chamados/juizes.py:947`, `app/chamados/juizes.py:957`, `app/chamados/juizes.py:959`, `app/chamados/juizes.py:973`). Nenhum dos escritores alterados invalida esse cache (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/arquivo.py:209`). Sequência que consulte C6, grave toque e consulte novamente na mesma request preserva `False`; caminho HTTP atual que necessariamente faça essa sequência [nao verificado].

6. **Resposta à precondição:** as guardas de `recusa_da_reabertura()` não consultam `resgatado_em` nem `silenciado_*`; consultam veto, resolução administrativa, premissa, agrupamento, irmão e transição (`app/chamados/models.py:1285`, `app/chamados/models.py:1290`, `app/chamados/models.py:1296`, `app/chamados/models.py:1299`, `app/chamados/models.py:1302`, `app/chamados/models.py:1306`). Agrupamento e irmão também usam status, sem véu (`app/chamados/models.py:783`, `app/chamados/models.py:791`, `app/chamados/juizes.py:1683`). O principal efeito pretendido da nova ordem é disponibilizar o log antes da ressuscitação (`app/chamados/models.py:1389`, `app/chamados/models.py:1442`).

7. **Resposta ao dia sem escala:** sim, a linha fica antes do retorno `None` por célula inexistente (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:40`, `app/ponto/services/cartorio.py:1658`). Não demonstrei violação de contador: o selo existente exige uma linha por toque, explicitamente “não por efeito” (`app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:275`). O novo registro pode futuramente liberar C6 se a célula aparecer; esse leitor usa existência da linha por colaborador/data, sem exigir sucesso da cobrança original (`app/chamados/juizes.py:967`, `app/chamados/juizes.py:969`). Aprovação ou proibição desse efeito pelo contrato [nao verificado].

8. **Escritores/leitores dos logs:** `cobrar_dia` → `registrar_log` → `core.observ.evento` → `LogAuditoria.objects.create`; `desarquivar_chamado` → `arquivo.levantar` → `_trilha` → o mesmo `evento` (`app/chamados/services/cobrar_dia.py:37`, `app/core/auditoria.py:22`, `app/core/observ.py:134`, `app/chamados/views_arquivar.py:73`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:137`). O leitor funcional é `_admin_tocou_o_dia_depois`: exige usuário humano, posterior à tranca, ação/modelo/objeto certos e, para cobrar, substring `dia=ISO` (`app/chamados/juizes.py:965`, `app/chamados/juizes.py:969`, `app/chamados/juizes.py:972`). Não há segundo escritor de banco introduzido nesses trechos (`app/core/observ.py:134`).

## 31/10 04:42 · sessao bf8e6edd6249 · - · - · origem=Ronald (tmux)

Cartório — conclusões com leitura estática:

1. **PROCEDENTE: a previsão pode fundamentar escrita sem que a lavra aconteça.** `ressuscitar_perguntas()` lê a projeção e grava `materializada_em=None` etc. (`app/chamados/models.py:1434`, `:1442`, `:1485`). Depois, antes da lavra (`app/ponto/services/cartorio.py:1520`), há chamadas que podem lançar: `_w` em `:1402`, `_espelhar_marcos` em `:1516`. Esta última grava contexto (`app/ponto/management/commands/detectar_ausencias.py:558`) e nota (`:563`; INSERT em `app/chamados/services/nota.py:103`/`:107`) sem capturar a exceção. `julgar_colab` possui `try/finally`, sem atomic englobando o corpo (`app/ponto/services/cartorio.py:747`, `:853`, `:854`); `julgar_celula` também não abre atomic (`:1670`). Chamadores diretos de produção igualmente não o fazem: `cobrar_dia` (`app/chamados/services/cobrar_dia.py:37`) e command (`app/ponto/management/commands/processar_cartorio.py:59`). Contraexemplo: célula gravada concorde, agora com furo parcial; pergunta antes carimbada é ressuscitada pela previsão acusadora; falha de INSERT da nota ao espelhar marcos impede a lavra, deixando a pergunta desmarcada sobre a célula antiga concorde. Ocorrência em produção: [nao verificado].

2. **Igualdade normal, com ressalva de persistência.** A previsão e a lavra usam exatamente `cel`, `via='cartorio'`, `_vd`, `cods` (`app/ponto/services/cartorio.py:1214`, `:1520`), e a lavra chama a mesma derivação (`app/ponto/portas/celula.py:698`). Não há reatribuição de `_vd`/`cods` entre ambas. Admin permanece com a dupla original na previsão (`app/ponto/portas/celula.py:656`) e na lavra (`:692`). A lavra pode apenas atualizar ata ou retornar sem alteração; “mesma dupla final” não significa “sempre grava” (`:693`, `:705`). `carimbar` persiste cel+evento em atomic próprio, sem abranger a ressuscitação anterior (`app/ponto/portas/celula.py:746`, `:639`).

3. **Caminhos sem lavra.** `apply_=False` ainda publica previsão (`app/ponto/services/cartorio.py:1214`), mas não entra em `:1517`; callback `write` pode consultá-la nos relatórios de emissão/furo parcial (`:1463`, `:1501`; callback definido em `:746`). Não achei consumidor de produção que consulte nesse callback: [nao verificado]. Sem vivos não impede lavra: a condição de `:1517` exige apenas `apply_`. `calado` somente troca/restaura `_tl.ativo`, sem limpar mapas (`:72`–`:77`); não constitui sozinho caminho sem lavra. O ramo `nunca_bateu` lavra separadamente, sem publicar mapas e sem ressuscitação (`:1012`, `:1034`).

4. **Mapa escritores/leitores.** `_tl.atas`: criação/escrita `app/ponto/services/cartorio.py:1146`/`:1148`; leitura pelo accessor `:55`. `_tl.vereditos`: criação/escrita `:1212`/`:1214`; leitura pelo accessor `:64`. Ambos chegam à projeção em `app/chamados/juizes.py:393`/`:394` e são limpos no finally em `app/ponto/services/cartorio.py:856`/`:857`. São mapas acumulados do julgamento inteiro, inclusive passadas anteriores, até o finally (`:776`, `:841`, `:856`).

5. **Leitores anteriores não ganham mudança funcional de veredito no fluxo normal.** `celula_do_dia_agora` alimenta `_celula_furada` (`app/chamados/juizes.py:437`) e `premissa_regularizada` (`app/chamados/models.py:1228`). O primeiro lê ata/trabalha, sem consultar veredito (`app/chamados/juizes.py:443`, `:444`, `:453`). O segundo lê somente `veredito_via=='admin'`, preservado pelo helper, antes de trabalhar com ata/marcos (`app/chamados/models.py:1230`, `:1232`, `:1236`, `:1245`). A mudança funcional pretendida concentra-se na ressuscitação: marco sobre ata projetada (`app/chamados/models.py:1436`; `app/chamados/juizes.py:865`, `:886`) e C6 sobre veredito projetado (`app/chamados/models.py:1442`; `app/chamados/juizes.py:1074`).

6. **Ata projetada não é literalmente a ata final.** A lavra preserva `precedencia` da ata anterior, enquanto `_tl.atas` recebe a ata bruta (`app/ponto/portas/celula.py:94`, `:684`, `:686`; `app/ponto/services/cartorio.py:1148`). Não encontrei leitor de `precedencia` nos consumidores examinados; efeito adverso desse detalhe nesta obra: [nao verificado].

7. **Juiz duplicado: não demonstrado no caminho cartório.** C6 continua registrado em `app/core/juizes.py:238`, é chamado pela ressuscitação (`app/chamados/models.py:1442`); a previsão reutiliza a função da lavra (`app/ponto/portas/celula.py:698`). Há pequena divergência contratual do helper para entrada arbitrária: não valida `via`, enquanto a lavra valida (`app/ponto/portas/celula.py:655`, `:662`, `:675`). Contraexemplo: helper aceita `via='invalida', veredito='trabalhou'`; lavra lança. Todos os consumidores atuais do helper passam cartório (`app/ponto/services/cartorio.py:1214`, `app/ponto/portas/celula.py:698`), portanto não o classificaria como falha atual de produção.

## 31/10 04:42 · sessao 31b18887450b · - · - · origem=Ronald (tmux)

Não identifiquei vazamento ou `save()` da cópia nos caminhos atuais: `celula_deste_julgamento` alimenta o carregador da premissa e a cópia de pergunta; a ressuscitação devolve IDs e escreve por outro queryset. (`app/chamados/juizes.py:378`, `app/chamados/models.py:1434`, `app/chamados/models.py:1442`, `app/chamados/models.py:1483`)

- **Cópia de Pergunta:** `_state` e `fields_cache` são explicitamente separados; a troca de `celula` ocorre depois dessa separação. `_pv_memo` é removido antes da avaliação e reescrito somente em `_q`. (`app/chamados/models.py:1427`, `app/chamados/models.py:1430`, `app/chamados/models.py:1439`, `app/chamados/juizes.py:823`)
- **Aliases mutáveis — DUVIDA:** a cópia rasa conserva referências dos campos mutáveis, e `out.ata` recebe diretamente o dict da ata em curso. Uma mutação aninhada poderia alterar os objetos compartilhados; os consumidores examinados apenas leem esses valores. (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:400`, `app/chamados/juizes.py:869`, `app/chamados/juizes.py:886`, `app/ponto/services/lastro.py:148`)
- **Selo insuficiente — DUVIDA:** o teste cujo comentário promete isolamento da FK não cria uma pergunta nem exercita `fields_cache`; compara somente `veredito` e `ata` de células. Não comprova o isolamento implementado na pergunta. (`app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:185`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:188`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:197`, `app/chamados/models.py:1431`)
- **Juiz duplicado:** a porta consulta o C6 registrado, sem nova derivação de “pergunta viva” nesse trecho. (`app/core/juizes.py:238`, `app/chamados/models.py:1442`)
- **Leitores:** `fato_do_marco_no_espelho` passa a receber a ata em curso; C6 recebe também o veredito previsto. A mudança alcança o guarda da lâmpada e os ramos de veredito acusatório/provisório. (`app/chamados/models.py:1434`, `app/chamados/models.py:1436`, `app/chamados/models.py:1442`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1088`)
- **FK nula — DUVIDA:** perguntas podem ter `celula=None`; nesse caso não recebem a previsão do julgamento e o guarda do marco responde `None`. C6 segue pela classificação do dia ou pelo padrão vivo. Não estabeleci regressão introduzida pelo delta4 nesse caso. (`app/chamados/models.py:2191`, `app/chamados/juizes.py:390`, `app/chamados/juizes.py:865`, `app/chamados/juizes.py:1090`, `app/chamados/juizes.py:1100`)
- **Memo de toque — DUVIDA:** `_tranca_memo` vive no request ou colaborador e retorna o resultado anterior sem reconsultar trilhas. Consultar C6 antes do toque e novamente depois, no mesmo dono do memo, conserva o resultado antigo; a ocorrência desse encadeamento numa rota normal permanece [nao verificado]. (`app/chamados/juizes.py:943`, `app/chamados/juizes.py:947`, `app/chamados/juizes.py:957`, `app/chamados/juizes.py:973`)

## 31/10 04:21 · sessao 47b7718fd31d · O261-E-F1-CLASSE-4 · verificador · origem=Code

Há dois riscos de ordem no novo filtro: ele pode consultar a célula **antes da lavra atual** e a tranca **antes da trilha humana que autoriza o resgate**. Ambos podem deixar a pergunta carimbada após a tentativa de ressuscitação. Ocorrência e quantidade em produção: [nao verificado]. (`app/ponto/services/cartorio.py:1359`, `app/ponto/services/cartorio.py:1503`, `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`.)

A análise abaixo é estática. Execução dos testes, resultados de mutantes e correspondência Git de `7b76efa7..HEAD`: [nao verificado].

**1. Quem escreve, quem lê e todos os chamadores de produção**

Na tabela, **M** = `materializada_em`; **V** = `validada_em`; **R** = `respondida_em`; **Via** = `via_resolucao`. São os campos declarados em `app/chamados/models.py:2202`, `app/chamados/models.py:2203`, `app/chamados/models.py:2288`, `app/chamados/models.py:2648`.

| Escritor/porta | Campos e operação |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa M/V/R/Via e `validada_por`; é a escrita persistente usada pelo diff. `app/chamados/models.py:1477`. |
| `_universo_da_ressuscitacao` | Limpa M/V/R/Via **somente na cópia** consultada pelo C6. `app/chamados/models.py:1430`, `app/chamados/models.py:1432`. |
| `PerguntaDisputa.fechar_por_admin` | Grava M/Via. `app/chamados/models.py:2488`. |
| `fechar_por_competencia_trancada` | Grava M/Via. `app/chamados/models.py:2502`. |
| `encerrar_por_fato_no_espelho` | Troca Via. `app/chamados/models.py:2515`. |
| `encerrar_fantasma` / `reabrir_fantasma` | Gravam/limpam M/Via. `app/chamados/models.py:2527`, `app/chamados/models.py:2542`. |
| `reabrir_por_competencia` | Limpa M/Via. `app/chamados/models.py:2572`. |
| `reabrir_por_resposta` | Prepara a limpeza de M/Via em memória; o núcleo da resposta salva. `app/chamados/models.py:2562`, `app/chamados/services/disputa_emissao.py:1265`. |
| `responder_pergunta_disputa_core` | Grava R, desfaz V anterior e salva as limpezas acima. `app/chamados/services/disputa_emissao.py:1248`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/disputa_emissao.py:1266`. |
| `reabrir_para_correcao` | Limpa R/V; opcionalmente M/Via. `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1360`. |
| `responder_automaticamente` / `carimbar_resposta_pela_trilha` | Gravam R. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1409`. |
| `resolver_perguntas_por_chamado_resolvido` | Grava M/Via; conforme texto e ramo, também R/V. `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/disputa_emissao.py:1644`. |
| Materializador de ausência | Grava M/Via e completa R/V quando há texto. `app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:526`. |
| Materialização de hora, recusa por colisão e pendentes aprovadas | Gravam M/Via. `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1042`, `app/chamados/services/materializacao.py:1339`. |
| `_materializar_uma` | Grava M/Via e completa R/V quando há texto. `app/chamados/services/materializacao.py:1149`, `app/chamados/services/materializacao.py:1177`, `app/chamados/services/materializacao.py:1190`. |
| `_carimbar_validacao` / `devolver_validacao_recusada` | Gravam/desfazem V. `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`. |
| Reconciliador: `lavrar_sem_turno_aberto` e E2 | Gravam M/Via. `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:621`. |
| `fio.retratar_duplicata` | Grava Via e, se necessário, M. `app/chamados/services/fio.py:892`. |
| `relavrar` / `desfazer_carimbo_sem_lastro` | Trocam Via / limpam M/Via. `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:231`. |

O nascimento passa por `PerguntaDisputa.abrir`, que aceita `**campos` e os entrega ao `create`; portanto também pode inicializar esses valores. (`app/chamados/models.py:2364`, `app/chamados/models.py:2442`.)

Os leitores diretos, além das próprias guardas dos escritores acima, distribuem-se assim:

| Leitor | Campos usados |
|---|---|
| `estado`, `respondida`, `q_vivas` | M/V/R na tradução de estado; R na propriedade `respondida`; os quatro no filtro SQL. `app/chamados/models.py:2465`, `app/chamados/models.py:2675`, `app/chamados/models.py:2603`. |
| Disputa: pendentes e respostas sem veredito | R; M/V/R. `app/chamados/models.py:1712`, `app/chamados/models.py:1758`. |
| C6 e seu memo | Os quatro; V mata primeiro, R participa da resposta, M/Via das vias e do lastro. `app/chamados/juizes.py:768`, `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1080`. |
| Juiz de materialização/testemunha | M/Via; trilha de retratação também exige M. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:629`, `app/chamados/juizes.py:275`. |
| Filas corrigíveis, respondidas a validar e irmãs sem consumo | R; M/V/R nas irmãs. `app/chamados/juizes.py:1120`, `app/chamados/juizes.py:1127`, `app/chamados/juizes.py:1476`. |
| Lavra de cache e transição do chamado | M/R nos marcos do lote; V na seleção de perguntas afetadas pela transição. `app/chamados/veredito_lavrado.py:163`, `app/chamados/models.py:293`, `app/chamados/models.py:327`. |
| Cartório | M no universo; Via no ramo “todas duplicatas”. `app/chamados/models.py:1421`, `app/ponto/services/cartorio.py:1336`. |
| Emissão de disputa | M na derivação de destino, órfãos, âncora e exclusão da fábrica; R na recobrança. `app/chamados/services/disputa_emissao.py:211`, `app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:874`, `app/chamados/services/disputa_emissao.py:894`, `app/chamados/services/disputa_emissao.py:1117`, `app/chamados/services/disputa_emissao.py:1445`. |
| Materialização | M/Via no estado aplicado e absorção; M/V/R nas filas/devoluções. `app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:577`, `app/chamados/services/materializacao.py:620`, `app/chamados/services/materializacao.py:1287`. |
| Validação | M/V/R em elegibilidade, recusa, lote e fila. `app/chamados/services/validacao.py:363`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:534`, `app/chamados/services/validacao.py:685`, `app/chamados/services/validacao.py:877`. |
| Fábrica, relavra e reabertura de competência | M/Via nos filtros. `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:121`, `app/chamados/services/relavra_via.py:153`, `app/chamados/services/relavra_via.py:179`, `app/chamados/services/competencia_trancada.py:244`. |
| Reconciliador | M/V nos universos de E2 e órfãs; M no reatamento. `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:1004`, `app/chamados/reconciliador.py:1411`. |
| Fio, ações e declaração | R no POST/resposta/correção; V na resposta validada e correção administrativa. `app/chamados/services/fio.py:336`, `app/chamados/services/fio.py:656`, `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/acoes_disputa.py:341`, `app/chamados/services/declaracao_texto.py:208`, `app/chamados/services/declaracao_texto.py:363`, `app/chamados/services/declaracao_texto.py:632`. |
| Painel, verbos, histórico e raio-X | M/V/R nos estados/filtros; Via no explicador. `app/chamados/services/painel.py:67`, `app/chamados/services/painel.py:268`, `app/chamados/services/verbos.py:237`, `app/chamados/services/verbos.py:308`, `app/chamados/services/historico.py:99`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:173`, `app/chamados/services/explicador.py:95`. |
| Aviso, cobrança, becos e relatório | R. `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/cobranca.py:229`, `app/chamados/services/becos.py:30`, `app/chamados/relatorios_disputa.py:20`. |
| Vigias e serviços de ponto | V no canal/carimbo; R no vigia de resposta e supra-juiz; M/V na procedência; Via/V no flip. `app/chamados/services/furo_sem_canal.py:81`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/services/vigia_fio.py:417`, `app/ponto/supra_juiz.py:485`, `app/ponto/services/procedencia_batida.py:74`, `app/ponto/services/flip_auto.py:129`. |
| Views HTML e REST | M/R na resposta HTML; R nas listas e respostas da API; R na validação inline/mensageria. `app/chamados/views.py:1257`, `app/chamados/views.py:1280`, `app/chamados/views.py:1416`, `app/api/views.py:1009`, `app/api/views.py:1175`, `app/api/views.py:1187`, `app/api/views.py:2634`, `app/api/views.py:2688`, `app/api/views.py:2705`, `app/chamados/views_cobrar.py:169`, `app/api/views_mensageria.py:569`. |
| Templates | R/V no modal e pergunta; R/Via no cartão de atendimento. `app/templates/chamados/partials/modal_fio.html:184`, `app/templates/chamados/partials/modal_fio.html:223`, `app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:24`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:186`. |

Também há leitores administrativos: auditorias de disputa/invariantes, cura de limbo, encerramento por fato, dedup, relavra, recuperação de órfãs, reconciliação de grade/fantasmas e passivo validado. Eles leem os carimbos ou a Via para selecionar, diagnosticar ou fotografar reversão. (`app/chamados/management/commands/auditar_disputas.py:71`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1568`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/relavrar_via_i6.py:58`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `app/ponto/management/commands/reconciliar_grade.py:453`, `app/ponto/management/commands/reconciliar_fantasmas.py:212`, `app/chamados/services/passivo_validado.py:64`.) O censo externo também lê R/V. (`bin/censo_folha.py:28`.)

O grafo completo de chamadas diretas das portas do diff é:

| Valor/porta | Produtor | Todos os chamadores diretos de produção encontrados |
|---|---|---|
| `_universo_da_ressuscitacao` → `(ids, marco, c6)` | Classificação e retorno em `app/chamados/models.py:1425`, `app/chamados/models.py:1435`. | `perguntas_a_ressuscitar`, `perguntas_puladas_na_ressuscitacao`, `ressuscitar_perguntas`: `app/chamados/models.py:1398`, `app/chamados/models.py:1403`, `app/chamados/models.py:1468`. |
| `perguntas_a_ressuscitar` → IDs | `app/chamados/models.py:1398`. | Somente `reabrir_orfas_enterradas`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. A seleção vale para dry **e** apply: `app/chamados/management/commands/reabrir_orfas_enterradas.py:121`. |
| `perguntas_puladas_na_ressuscitacao` → dicionário | `app/chamados/models.py:1404`. | Somente cartório, quando `_n == 0`: `app/ponto/services/cartorio.py:1360`, `app/ponto/services/cartorio.py:1370`. |
| `ressuscitar_perguntas` → quantidade atualizada | `app/chamados/models.py:1477`, `app/chamados/models.py:1507`. | `reabrir` e cartório: `app/chamados/models.py:1389`, `app/ponto/services/cartorio.py:1359`. |
| `fio_marco_aceso` / `fio_c6_morta` | Leitura/incremento no próprio cartório: `app/ponto/services/cartorio.py:1372`, `app/ponto/services/cartorio.py:1376`. | Retorno genérico em `out`: `app/ponto/services/cartorio.py:1668`. Nenhum consumidor específico identificado; o resumo batch não os imprime: `app/ponto/management/commands/processar_cartorio.py:66`. |
| `[FIO-MARCO-ACESO]` / `[FIO-C6-MORTA]` | `app/ponto/services/cartorio.py:1373`, `app/ponto/services/cartorio.py:1377`. | Callback `write`; sem callback, descarte: `app/ponto/services/cartorio.py:737`. Batch passa `stdout.write`: `app/ponto/management/commands/processar_cartorio.py:65`. Parsers externos: [nao verificado]. |
| `[FIO-DUP-C8]` | Somente após fechar outros chamados: `app/ponto/services/cartorio.py:1353`, `app/ponto/services/cartorio.py:1356`. | Mesma cadeia `write`/stdout. `app/ponto/services/cartorio.py:737`, `app/ponto/management/commands/processar_cartorio.py:65`. |

A trilha persistente recebe `pulados_marco_aceso` e `pulados_c6_morta` **somente quando alguma pergunta voltou**; com zero, a porta retorna antes da trilha. (`app/chamados/models.py:1474`, `app/chamados/models.py:1482`, `app/chamados/models.py:1499`, `app/chamados/models.py:1501`.)

**2. Cópia consultada pelo C6 versus registro atualizado**

Para os quatro campos perguntados, a transformação coincide: ambos deixam M/V/R nulos e Via vazia. A diferença é `validada_por`: o `update` limpa, a cópia conserva. O C6 e sua chave de memo não leem esse autor; portanto essa diferença não altera o resultado atual do juiz. (`app/chamados/models.py:1432`, `app/chamados/models.py:1477`, `app/chamados/juizes.py:767`, `app/chamados/juizes.py:1005`.)

Nenhum dos dois limpa `materializada_por`, nem os campos de veredito lavrado da pergunta. Esses valores tampouco entram na decisão do C6 sobre essa cópia sem materialização. (`app/chamados/models.py:1432`, `app/chamados/models.py:1477`, `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1080`.)

Sobre a cópia, o C6:

- Dá **morta** se o dia está trancado sem toque humano admitido. (`app/chamados/juizes.py:1010`, `app/chamados/juizes.py:907`.)
- Dá **viva** se a célula acusa. (`app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1056`.)
- Dá **morta** se a célula tem veredito não acusatório definitivo; o provisório sem fato do marco pode prosseguir vivo. (`app/chamados/juizes.py:1067`, `app/chamados/juizes.py:1068`.)
- Nos caminhos restantes, acaba dando viva porque `materializada_em` foi zerada. (`app/chamados/juizes.py:1070`, `app/chamados/juizes.py:1080`.)

O guarda anterior continua indispensável: o degrau do marco aceso exige M presente, mas a cópia já tem M nula. Por isso a porta pergunta pelo marco **antes** de limpar a cópia. (`app/chamados/models.py:1427`, `app/chamados/models.py:1432`, `app/chamados/juizes.py:1050`.)

Quanto aos caches:

- `_pv_memo` é removido da cópia; o C6 calcula e cria outro memo. Não há reaproveitamento desse resultado anterior. (`app/chamados/models.py:1431`, `app/chamados/juizes.py:792`, `app/chamados/juizes.py:803`.)
- `celula` e `disputa` vêm carregadas pela consulta. O C6 lê diretamente essa célula. A chave consulta `_state.fields_cache` para o destino, mas a cópia sem M não chega ao ramo de testemunha que usa o estado do destino. (`app/chamados/models.py:1424`, `app/chamados/juizes.py:758`, `app/chamados/juizes.py:1017`, `app/chamados/juizes.py:1080`.)
- Existe outro cache: `_tranca_memo`, na request ou no colaborador. Ele conserva a resposta de tranca e de toque humano durante sua vida; limpar `_pv_memo` não o limpa. (`app/chamados/juizes.py:899`, `app/chamados/juizes.py:919`, `app/chamados/juizes.py:937`.)
- A implementação instalada de `copy.copy(Model)` e o compartilhamento exato de `_state` não estão demonstrados pelo código da aplicação: [nao verificado]. O caminho mostrado não altera as FKs na cópia. (`app/chamados/models.py:1430`.)

Assim, **com os mesmos fatos externos, a cópia e o registro relido após o `update` recebem a mesma resposta do C6**. Isso não vale para reutilizar uma instância antiga: a porta faz `QuerySet.update`, sem atualizar os objetos já carregados pelo chamador. (`app/chamados/models.py:1475`, `app/chamados/models.py:1477`.)

A discrepância relevante encontrada é temporal: no cartório, a célula carregada pode representar a passada anterior, embora a rodada já tenha calculado outra ata. Isso é tratado no item 3. (`app/ponto/services/cartorio.py:1134`, `app/ponto/services/cartorio.py:1359`, `app/ponto/services/cartorio.py:1503`.)

**3. Comportamento por chamador e casos de retorno incorreto**

Nos caminhos que chegam a `reabrir`, a mudança comum é: uma candidata materializada e sem texto, antes aceita após o guarda do marco, agora fica carimbada se o C6 sobre sua versão limpa disser morta. A reabertura do chamado ainda pode retornar `True` mesmo com zero perguntas ressuscitadas. (`app/chamados/models.py:1420`, `app/chamados/models.py:1434`, `app/chamados/models.py:1389`, `app/chamados/models.py:1390`.)

Todos os chamadores de produção de `reabrir` encontrados são:

| Chamador | Caso atingido |
|---|---|
| Renascimento por premissa viva | Candidatas mudas do chamado reaproveitado passam pelo novo filtro; é chamado pelo dedup de `abrir` e pela resposta tardia. `app/chamados/models.py:978`, `app/chamados/models.py:881`, `app/chamados/services/resposta_tardia.py:45`. |
| Desarquivar humano | Perguntas do chamado encerrado pela competência passam pelo C6 antes da trilha do resgate. `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`. |
| Adesão | Candidatas mudas do pai reaberto recebem o filtro comum. `app/chamados/services/adesao.py:92`, `app/chamados/models.py:1434`. |
| Reabrir competência | A porta específica já limpa primeiro as perguntas com Via da tranca; outras candidatas do chamado passam pelo filtro comum. `app/chamados/services/competencia_trancada.py:247`, `app/chamados/services/competencia_trancada.py:258`. |
| Reabrir disputa administrativamente | Abre uma nova pergunta de confirmação; candidatas antigas do chamado passam pelo filtro comum. `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:266`. |
| Resposta aguardando validação | Reabre o pai; perguntas com texto continuam fora do universo, mas suas irmãs mudas podem mudar. `app/chamados/services/disputa_emissao.py:1604`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/models.py:1423`. |
| Recuperar órfãs enterradas | A pergunta-alvo é aberta antes do pai; o novo filtro afeta as demais candidatas do pai. `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`. |
| Recuperar chamado de ausência aberta | Candidatas mudas passam pelo filtro comum, se a reabertura for aceita. `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`, `app/chamados/models.py:1389`. |
| Reconciliar grade | Mesmo efeito nas candidatas mudas do chamado reaberto. `app/ponto/management/commands/reconciliar_grade.py:522`, `app/chamados/models.py:1434`. |
| Detector, ramo de limbo | Mesmo efeito nas candidatas mudas do chamado reaberto. `app/ponto/management/commands/detectar_ausencias.py:301`, `app/chamados/models.py:1434`. |

O outro chamador direto, **cartório**, deixa de vetar a ressuscitação por `_protesta(x)` no dia julgado e passa a julgar a pergunta pela célula dela. Isso permite o caso legítimo D+1 em folga cobrando D furado, coberto pelo novo teste. (`app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1359`, `app/chamados/models.py:1434`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`.)

As entradas diretas de `julgar_colab` são batch, defesa de escala, comando de célula velha e wrapper singular. O corpo também roda na segunda passada e na releitura. (`app/ponto/management/commands/processar_cartorio.py:65`, `app/escala/regua_defesa.py:692`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/services/cartorio.py:1654`, `app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cartorio.py:832`.)

Os chamadores diretos de `julgar_celula` são os sinais de chamado e ponto, reconciliador, cobrar dia, pós-vínculo, recuperação de ausência em folga, cura BUG115, vigia de hora, furos sem cobrança e porta de célula. (`app/chamados/signals.py:180`, `app/ponto/signals.py:93`, `app/chamados/reconciliador.py:675`, `app/chamados/services/cobrar_dia.py:29`, `app/col

[truncado em 20000 de 33283 caracteres]

## 31/10 04:16 · sessao 9e4a2c2c0086 · - · - · origem=Ronald (tmux)

Inventário enviado ao pai em mensagens, com leitores, escritores e chamadores diretos por `arquivo:linha`.

Achados principais:

- O diff não cria escritor persistente dos quatro campos: altera a seleção do `update` existente; a atribuição nova ocorre na cópia em memória. (`app/chamados/models.py:1430`, `app/chamados/models.py:1477`)
- `_tem_resposta_do_colab` significa “existe texto após `strip`”, sem exigir `respondida_em`; esse critério coincide com as proteções do reconciliador, materializador, seleção da canônica e prévia da folha. (`app/ponto/services/cartorio.py:1327`, `app/chamados/reconciliador.py:365`, `app/chamados/services/materializacao.py:769`, `app/chamados/services/fio.py:874`, `app/folha/previa.py:142`)
- Esse critério difere de `PerguntaDisputa.respondida` — somente timestamp —, do degrau de resposta do C6 — timestamp e texto —, e de `respostas_sem_veredito` — timestamp ou texto, sujeito aos demais filtros. (`app/chamados/models.py:2675`, `app/chamados/juizes.py:1020`, `app/chamados/models.py:1758`)
- Há discrepância prévia de normalização: texto formado apenas por espaços não isenta o chamado no cartório, mas tampouco pertence ao universo da ressuscitação, que aceita somente `NULL` ou `''` exato. Se esse universo ficar vazio, o novo diagnóstico não escreve nenhum dos dois marcadores; ocorrência real [nao verificado]. (`app/ponto/services/cartorio.py:1334`, `app/chamados/models.py:1423`, `app/chamados/models.py:1474`, `app/ponto/services/cartorio.py:1371`)
- A declaração documental de escritor único de `validada_em` não descreve todas as atribuições existentes: materialização e resolução automática também carimbam o campo, enquanto resposta/correção/ressuscitação o limpam. Essa multiplicidade antecede a alteração analisada. (`app/docs/JUIZES.md:38`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/models.py:1477`)
- `via_resolucao` já está explicitamente fora da declaração de escritor único. (`app/core/portas.py:352`)

## 31/10 04:16 · sessao af9e322f8a5d · - · - · origem=Ronald (tmux)

Inventário e achados por leitura estática; testes e manifestações em produção não executados [nao verificado]. [nao verificado]

1. Chamadores diretos dos métodos novos/alterados:

| Valor/método | Escritor/produtor | Leitores/chamadores de produção |
|---|---|---|
| `_universo_da_ressuscitacao()` → `(ids, marco, c6)` | Monta listas e retorna em `app/chamados/models.py:1420` e `:1435` | `perguntas_a_ressuscitar`, `models.py:1398`; `perguntas_puladas_na_ressuscitacao`, `:1403`; `ressuscitar_perguntas`, `:1468`. |
| `perguntas_a_ressuscitar()` → IDs | `app/chamados/models.py:1398` | Único chamador direto encontrado: comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`, para vetar irmãs que ressuscitariam fora do escopo; não é apenas dry, a seleção precede tanto dry quanto apply (`:104`, `:121`). |
| `perguntas_puladas_na_ressuscitacao()` → dict | `app/chamados/models.py:1403` e `:1404` | Único chamador direto encontrado: `app/ponto/services/cartorio.py:1370`, somente quando ressuscitação retornou zero (`:1360`). |
| `ressuscitar_perguntas()` → contagem | `app/chamados/models.py:1477`, `:1507` | Dois: `ChamadoColaborador.reabrir`, `models.py:1389`; `_julgar_colab_corpo`, `cartorio.py:1359`. |
| `fio_marco_aceso` | Incremento/read-modify-write em `app/ponto/services/cartorio.py:1372` | Nenhum consumidor específico de produção encontrado; `julgar_celula` devolve `out` em `:1668`; o resumo batch não imprime este contador (`app/ponto/management/commands/processar_cartorio.py:66`). |
| `fio_c6_morta` | Incremento/read-modify-write em `app/ponto/services/cartorio.py:1376` | Mesma ausência de consumidor específico e mesmo retorno genérico; `cartorio.py:1668`, `processar_cartorio.py:66`. |
| `[FIO-MARCO-ACESO]` | `app/ponto/services/cartorio.py:1373` | Vai para callback `_w`, definido como `write` ou descarte em `:737`; batch passa `stdout.write` em `processar_cartorio.py:65`; parsers externos [nao verificado]. |
| `[FIO-C6-MORTA]` | `app/ponto/services/cartorio.py:1377` | Mesma cadeia `_w`/`write`; `cartorio.py:737`, `processar_cartorio.py:65`; parsers externos [nao verificado]. |
| `[FIO-DUP-C8]` | `app/ponto/services/cartorio.py:1356` | Mesma cadeia; emitido somente se lista `_out_dup` não vazia (`:1353`); parsers externos [nao verificado]. |

Os dois contadores não existem no dict inicial de `contadores()`; são chaves esparsas abertas pelos incrementos (`app/ponto/services/cartorio.py:1571`, `:1372`, `:1376`).

2. Entradas de produção que chegam a `reabrir()`:

- `_renascer_por_premissa_viva` em `app/chamados/models.py:978`, chamado pelo dedup de `abrir` em `:881` e por resposta tardia em `app/chamados/services/resposta_tardia.py:45`.
- Desarquivar por humano: `app/chamados/views_arquivar.py:68`.
- Adesão: `app/chamados/services/adesao.py:92`.
- Reabertura de competência: `app/chamados/services/competencia_trancada.py:258`.
- Reabertura administrativa de disputa: `app/chamados/services/acoes_disputa.py:266`.
- Resposta aguardando validação: `app/chamados/services/disputa_emissao.py:1606`.
- Correção de órfãs: `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.
- Correção de ausência aberta: `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`.
- Reconciliação de grade: `app/ponto/management/commands/reconciliar_grade.py:522`.
- Detector, caso de limbo: `app/ponto/management/commands/detectar_ausencias.py:301`.

Para todos esses, o novo filtro recusa perguntas cuja versão sem carimbos o C6 dá por morta, incluindo dia trancado (`app/chamados/models.py:1432`, `:1434`; `app/chamados/juizes.py:1010`) ou célula definitiva que não acusa (`juizes.py:1053`, `:1068`). As guardas que recusam a própria reabertura continuam antes da ressuscitação (`models.py:1338`, `:1375`, `:1389`).

Entradas diretas de `julgar_colab`: batch `app/ponto/management/commands/processar_cartorio.py:65`; defesa de escala `app/escala/regua_defesa.py:692`; comando de célula velha `app/ponto/management/commands/celula_veredito_velho.py:144`; wrapper singular `app/ponto/services/cartorio.py:1654`. O corpo é chamado pela primeira passada, segunda passada e releitura em `cartorio.py:744`, `:767`, `:832`.

Chamadores diretos de `julgar_celula`: `app/chamados/signals.py:180`, `app/chamados/reconciliador.py:675`, `app/ponto/signals.py:93`, `app/chamados/services/cobrar_dia.py:29`, `app/colaboradores/services/pos_vinculo.py:72`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71`, `app/ponto/management/commands/curar_residuo_bug115.py:143`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/portas/celula.py:372`.

3. Achado forte: decisão antes da lavra pode perder pergunta que deveria voltar.

O cartório calcula e expõe a ata desta rodada em `app/ponto/services/cartorio.py:1134` e `:1138`, mas chama ressuscitação em `:1359`, antes de persistir veredito e ata em `:1503` e `:1518`. O novo universo carrega `p.celula` do banco (`app/chamados/models.py:1424`); o C6 lê essa FK diretamente em `app/chamados/juizes.py:1017`, `:1053` e `:1068`; o juiz do marco também usa diretamente `p.celula.ata` (`juizes.py:845`, `:866`). O mecanismo `celula_do_dia_agora` que vê a ata em andamento é usado por `_celula_furada` (`juizes.py:417`), não por esses degraus do C6.

Caso dedutível: célula anteriormente `concorde`, agora furada; chamado permanece vivo; pergunta materializada/sem texto, marco antigo apagado ou desconhecido. A cópia é recusada pelo C6 a partir do `concorde` antigo; `_n=0` não enfileira `rejulgar`, pois isso só ocorre no ramo `_n` (`cartorio.py:1378`, `:1384`). O chamado vivo cala emissão (`:1421`). A impressão já foi calculada com fio morto (`:1095`, `:1104`) e será gravada (`:1518`), podendo fazer a rodada seguinte sem novo insumo pular (`:1113`). Portanto pode ficar preso até novo insumo ou `forcar`, e não somente uma rodada; ocorrência real no dado fornecido [nao verificado].

O inverso também é permitido pela ordem: célula antiga acusa, ata atual absolve; a cópia pode ser ressuscitada usando estado antigo, antes da lavra atual (`cartorio.py:1359`, `:1503`; `juizes.py:1056`). Outros blocos podem impedir alguns desses cenários, conforme guardas/reconciliação (`cartorio.py:1219`, `:1259`); não afirmar incidência real [nao verificado].

4. Achado forte: desarquivar em competência trancada consulta o pré-requisito antes de escrevê-lo.

`desarquivar_chamado` chama `ch.reabrir(humano=True)` em `app/chamados/views_arquivar.py:68`; só depois registra o toque humano `desarquivar_chamado`, em `:75`. O novo C6 consulta tranca (`app/chamados/juizes.py:1010`), cuja exceção depende de trilha humana posterior à tranca (`:904`, `:907`, `:945`, `:952`), inclusive a ação `TOQUE_DESARQUIVAR` (`:916`).

Sem toque anterior, o novo universo pula a pergunta antes de o próprio clique escrever a trilha. `arquivo.levantar` salva e registra a trilha em `app/chamados/services/arquivo.py:208` e `:209`; esse save não muda status e o signal não rejulga (`app/chamados/signals.py:169`). A view não ressuscita novamente (`views_arquivar.py:75`, `:80`). Se a pergunta conservou `via_resolucao=competencia_trancada` e `materializada_em`, mesmo a nova leitura após a trilha continua morta pelo degrau explícito dessa via (`juizes.py:1015`). O método pode devolver sucesso de reabertura com pergunta ainda fora da fila (`models.py:1389`, `:1390`). Manifestação em produção [nao verificado].

5. Efeito no comando que lê `perguntas_a_ressuscitar`.

Irmã materializada sem texto cujo C6 hipotético é morto deixa de aparecer na lista `irmas`; o comando deixa de recusar por `ressuscitaria_irmas` exclusivamente por essa irmã (`app/chamados/management/commands/reabrir_orfas_enterradas.py:106`, `:107`). No apply, abre primeiro a pergunta-alvo por `reabrir_fantasma`, depois disputa e pai (`:146`, `:154`, `:161`); a própria pergunta-alvo já saiu do universo materializado quando o pai ressuscita (`app/chamados/models.py:1421`). A promessa de segurança de irmãs depende de a seleção e o apply lerem o mesmo estado; não existe trava de linha nessas leituras (`reabrir_orfas_enterradas.py:104`, `:145`); concorrência real [nao verificado].

6. Checklist.

- Juiz duplicado de vitalidade: não é introduzido; o registro vivo aponta C6 para `pergunta_viva` (`app/core/juizes.py:112`, `:238`) e a porta chama essa função (`app/chamados/models.py:1434`). `_protesta` continua respondendo outra pergunta, sobre o chamado no dia julgado (`app/ponto/services/cartorio.py:1156`).
- Escritor fora de porta: o delta adiciona mutações somente à cópia, enquanto o update persistente continua no escritor existente (`app/chamados/models.py:1430`, `:1477`). A porta já é declarada no censo (`app/core/portas.py:246`; `app/core/censo_escritas.py:27`).
- Relógio fora da borda: o novo caminho para C6 pode chegar a `_dentro_da_janela_viva` em `app/chamados/juizes.py:1070`, que lê `datetime.date.today()` dentro do juiz em `:518`, sem receber `agora` do cartório. Não é leitura adicionada à função, mas passa a ser alcançada na ressuscitação.
- Derivação paralela: mesma regra, porém duas execuções; `ressuscitar_perguntas` chama universo (`app/chamados/models.py:1468`), e o cartório, quando zero, repete universo através de `perguntas_puladas` (`cartorio.py:1370`; `models.py:1403`). Não recebe os motivos do resultado que acabou de produzir zero; mudança concorrente pode alterar diagnóstico [nao verificado].
- Ordem de trava: o delta não introduz `atomic`/`select_for_update`; seleção e update permanecem separados (`app/chamados/models.py:1468`, `:1477`). O caminho de estado faz save de chamado antes de relavrar perguntas (`models.py:274`, `:279`, `:292`), preservando a ordem documentada chamado→pergunta (`:263`). Ausência de deadlock em produção [nao verificado].
- Pouso antes do pré-requisito: os dois casos acima, C6 antes da lavra corrente e C6 antes da trilha do desarquivar, são pontos concretos (`app/ponto/services/cartorio.py:1359`, `:1503`; `app/chamados/views_arquivar.py:68`, `:75`).
- Selo/RED: execução efetiva e resultado de mutações [nao verificado]; o agente de testes deve consolidar a análise estática. [nao verificado]

## 31/10 04:15 · sessao 99f570337b55 · - · - · origem=Ronald (tmux)

Auditoria estática; não executei testes nem mutações. Verde/vermelho abaixo é inferência pelo código, não resultado medido [nao verificado]. [nao verificado]

| Teste novo/alterado | O que efetivamente protege | Mutação de produção que sobrevive ao teste individual | [nao verificado]
|---|---|---|
| `test_MORDE_dia_em_competencia_trancada...destrancar...` | Executa E2 por `_caso`; exige ausência de carimbo durante tranca e C6 vivo após remover a tranca. Retirar somente o guarda de tranca do E2 faria carimbar e falharia. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:47`, `:67`, `:84`, `:96`, `:103`; `app/chamados/reconciliador.py:611`, `:617`, `:621`; `app/chamados/juizes.py:1010`. | Substituir a delegação `if pergunta_viva(p): continue` por uma cópia do teto: `if marcos_pendentes(p.celula) and fato_do_marco_no_espelho(p) is not True: continue`, mantendo o guarda de tranca. O caso continua pulando pela condição provisória e todos os quatro testes deste arquivo continuam com os resultados esperados por leitura: aceso/provisório carimba; apagado/provisório pula; trancado/apagado/provisório pula; definitivo carimba. Portanto não prova “SO ao C6”. `app/chamados/reconciliador.py:611`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:70`, `:75`, `:93`, `:106`; teto original `app/chamados/juizes.py:1067`. |
| `test_MORDE_fio_dup_c8_nao_fecha_o_velho_com_resposta_do_colab` | Protege o velho respondido contra fechamento/reaponte neste cenário, em duas ordens de nascimento. A fixture coloca simultaneamente texto e timestamp. `app/ponto/tests/test_fechar3_troca_escala_gate.py:330`, `:334`, `:339`, `:343`. | Trocar o predicado para `any(q.respondida_em is not None for q in perguntas)`: a fixture não distingue timestamp de texto. Restaurar os dois `any(...strip()...)` inline também passa: o diff apenas os extrai para um helper. Desativar todo o fallback FIO-DUP-C8 também preserva as três asserções negativas deste teste; o controle existente de reaponte, porém, cobraria positividade. `app/ponto/services/cartorio.py:1327`, `:1341`, `:1351`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:339`, `:343`, `:345`, controle `:313`, `:325`. |
| `test_MORDE_vespera_turno_aberto_na_folga_religa...` | Executa cartório de D+1, mantendo pergunta da célula D acusatória; observa materializada limpa e C6 vivo. Morde restaurar o bloqueio `_protesta` do dia julgado, pela diferença D/D+1 descrita e construída. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`, `:65`, `:75`, `:81`, `:86`, `:90`. | Retirar o filtro C6 da porta e incluir toda pergunta não pulada pelo marco: a célula desta fixture já acusa, portanto o teste continua ressuscitando. `app/chamados/models.py:1427`, `:1434`; fixture `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`. |
| `test_MORDE_reabrir_nao_apaga_o_celula_absolveu...` | Executa `reabrir`, verifica manutenção de `via_resolucao`, materialização e lista `c6_morta`; a célula é lavrada com batidas e validada como definitiva. Morde aceitar toda pergunta apenas porque o marco é desconhecido: `manual_admin` não tem marco próprio. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:94`, `:106`, `:126`, `:132`, `:136`, `:139`, `:142`; `app/chamados/juizes.py:823`, `:842`. | Remover a chamada de ressuscitação de `reabrir`: a pergunta já nasce materializada com a via esperada; a consulta posterior `perguntas_puladas...` ainda pode classificá-la como morta. O controle positivo seguinte serve para cobrar a ligação. `app/chamados/models.py:1389`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:60`, `:101`, `:139`, `:144`. |
| `test_controle_reabrir_com_celula_que_acusa_religa` | Cobra positividade: reabertura precisa limpar materialização e devolver pergunta viva na célula acusatória. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:144`, `:149`, `:150`. | Retirar a consulta ao C6 e aceitar toda candidata sem marco aceso preserva este controle, mas seria capturado pelo teste negativo anterior. `app/chamados/models.py:1427`, `:1434`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:132`, `:144`. |

Duas mutações sobrevivem **aos três testes novos de ressuscitação conjuntamente**: [nao verificado]

- Trocar `pergunta_viva(_q)` por `_q.celula.veredito in CelulaDia.VEREDITOS_ACUSAM`: as fixtures cobrem somente `furo/concorde/furo`, sem distinguir o juiz completo de uma derivação paralela por veredito. Isso perde os degraus tranca, resposta, identidade e teto do C6, mas preserva as asserções presentes. `app/chamados/models.py:1434`; `app/chamados/juizes.py:1006`, `:1010`, `:1020`, `:1044`, `:1068`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`, `:126`, `:129`, `:90`, `:142`, `:149`.
- Omitir `via_resolucao=''` do `update`: positivos só cobram materialização limpa/C6 vivo, e suas vias `completude_turno`/`celula_absolveu` não integram o conjunto que encerra por identidade. O negativo exige justamente conservar a via. Também não há fixture de `respondida_em` ou `validada_por` preenchidos para morder a omissão dessas limpezas. `app/chamados/models.py:1477`; `app/chamados/juizes.py:1039`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:56`, `:60`, `:81`, `:101`, `:139`, `:149`.

A nova classificação observável não tem mordida nesses testes: remover os contadores/linhas `fio_marco_aceso`, `fio_c6_morta`, `[FIO-MARCO-ACESO]`, `[FIO-C6-MORTA]` ou a chave de auditoria `pulados_c6_morta` preserva suas asserções; apenas a lista pública `c6_morta` é verificada no negativo. `app/ponto/services/cartorio.py:1370`, `:1372`, `:1376`; `app/chamados/models.py:1501`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:90`, `:139`, `:142`, `:149`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:343`.

O cabeçalho E2 cita um log de mutação, mas esse comentário não prova a execução dos testes deste delta; o RED histórico citado permanece [nao verificado]. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:5`.

## 31/10 03:49 · sessao 80bb87b1c8db · O261-E-F5C · verificador · origem=Code

1. **Pertença à fila: não localizei leitor residual usando `verbo` como conjunto da fila.** O produtor separa `fila_ids` de `verbo`; o juiz consulta `fila_ids`, e o vigia já lê `alim['fila_ids']`. A folha apenas encaminha a alimentação ao juiz. (`app/chamados/juizes.py:1161`, `app/chamados/juizes.py:1231`, `app/chamados/services/vigia_fio.py:567`, `app/folha/fora_da_folha.py:59`)

   O teste de alimentação usa o produtor real; os dicionários manuais encontrados simulam **a saída da porta**, não `alimentar_portas()`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`, `app/folha/tests/test_haiku_fora_da_folha.py:101`, `app/folha/tests/test_haiku_fora_da_folha.py:129`, `app/chamados/tests/test_o261e_f5_alcance.py:307`)

2. **`.get(anc)` mistura ausência de chave com `None` legítimo.** Auditoria recebe pilha `registro`, cujo verbo é `None`, e a partição grava esse valor; nesse caso, o antigo `[]` também devolveria `None`. A mudança efetiva é aceitar silenciosamente uma âncora presente em `fila_ids`, mas ausente do mapa, antes um `KeyError`. (`app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:52`, `app/chamados/juizes.py:1231`, `app/chamados/juizes.py:1232`)

   O produtor normal inclui a fila no universo vivo e grava cada linha no mapa; ocorrência dessa ausência em produção: **[nao verificado]**. (`app/chamados/services/painel.py:186`, `app/chamados/services/verbos.py:44`)

   O tratamento pelos leitores é:

   - **Juiz:** `{anc: None}` continua sendo porta; se não houver `cobrar` ou `validar`, resulta em fio `chamado`. Para chamados da célula, `None` também passa pelo filtro de visibilidade. (`app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1233`, `app/chamados/juizes.py:1236`)
   - **Folha:** lê `fio`, sem interpretar `verbos`; nesse caso chega a `chamado`, grupo C e `DESTRAVA['chamado']`. (`app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:80`, `app/folha/fora_da_folha.py:199`)
   - **Vigia:** conta a porta, registra papel textual `"None"` e esse valor sozinho não incrementa `alcance_porta_exige_o_admin`. (`app/chamados/services/vigia_fio.py:599`)
   - **Testes:** aceitam explicitamente `None` na escada e numa porta mockada; esses casos não exercitam uma chave ausente na nova linha `.get(anc)`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:247`, `app/folha/tests/test_haiku_fora_da_folha.py:129`)

3. **`DESTRAVA['admin']` cobre os ramos atuais de `validar`.** São dois: módulo em `MODULOS_DO_ADMIN_VALIDAR` — atualmente `gps_fora_geofence` — ou resposta creditada. Auditoria sai antes como `None`; silêncio substitui o verbo não nulo por `arquivado`/`historico`. A frase nova contempla resposta e chamado que o admin valida. (`app/folha/fora_da_folha.py:41`, `app/chamados/catalogo/natureza.py:29`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:59`, `app/chamados/services/verbos.py:48`)

   Ressalva: nesse crédito, “respondida” significa `respondida_em is not None`, sem conferir texto; a frase promete acompanhar o que o painel põe em Validar, portanto não contradiz esse comportamento. (`app/chamados/services/raiox.py:137`, `app/folha/fora_da_folha.py:41`)

4. **O teste novo da ficha tem mordida parcial.** [nao verificado]

   - **Tupla literal:** isoladamente, não detecta voltar a `('arquivado', 'historico')`: essa é exatamente a constante atual, e os dois resultados cobrados permanecem iguais. O AST adjacente detectaria essa escrita literal porque proíbe `'historico'`; porém, exigir a presença do nome da constante no módulo não prova que ele governa `dono`. (`app/chamados/services/verbos.py:18`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`, `app/colaboradores/tests/test_ficha_do_colab.py:128`)
   - **Truncamento `[:1]`:** detecta, pois exige dono `arquivado` para os dois verbos produzidos pelas duas datas. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:113`, `app/chamados/services/verbos.py:51`)
   - **Mudança da fonte:** não testa se o consumidor acompanha uma constante alterada. Adicionar/remover membro pode derrubar a igualdade final mesmo com a ficha obedecendo à fonte, pois os estímulos continuam produzindo apenas os dois verbos atuais; reordenar passa pela comparação de conjuntos. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`, `app/chamados/services/verbos.py:51`)
   - **Relógio solto:** sim, transitivamente. O teste não congela localmente; chama a ficha, que lê `timezone.now()`, e a partição lê outro. `self.hoje` também vem de `localdate()`. O runner só congela se houver `CONGELAR_EM`. (`app/colaboradores/tests/test_ficha_do_colab.py:18`, `app/colaboradores/tests/test_ficha_do_colab.py:30`, `app/colaboradores/tests/test_ficha_do_colab.py:98`, `app/colaboradores/services/ficha.py:117`, `app/chamados/services/verbos.py:35`, `app/core/runner_quarentena.py:94`)

5. **Checklist, restrito ao delta:**

   | Item | Resultado |
   |---|---|
   | 2º juiz | A folha delega ao juiz da porta; não reproduz a tradução de verbos. (`app/folha/fora_da_folha.py:59`, `app/folha/fora_da_folha.py:70`) |
   | 2º escritor fora da porta | A troca para `.get` apenas altera leitura e montagem do retorno. (`app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1237`) |
   | Relógio fora da borda | Permanece alcançado pelo teste novo, sem freeze local. (`app/colaboradores/tests/test_ficha_do_colab.py:111`, `app/colaboradores/services/ficha.py:117`, `app/chamados/services/verbos.py:35`) |
   | Derivação paralela de fonte única | A ficha atual usa a constante; o teste comportamental novo não prova essa dependência. (`app/colaboradores/services/ficha.py:160`, `app/colaboradores/tests/test_ficha_do_colab.py:114`) |
   | Selo sem mordida | Mordida parcial na ficha; os testes de `None` citados não distinguem valor legítimo de chave ausente. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:247`, `app/folha/tests/test_haiku_fora_da_folha.py:129`) |

Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

## 31/10 03:48 · sessao 2ffa2e9478c8 · - · - · origem=Ronald (tmux)

Item 2 — `.get(anc)` tem dois `None` diferentes:

- **Chave presente, valor `None`:** módulos de auditoria recebem pilha `registro`, cujo verbo é `None`; a partição grava esse valor no mapa. O silêncio não o substitui porque exige `v is not None`. [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:44), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:63), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:47). O `[]` anterior já devolvia esse `None`. [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1232)
- **Chave ausente:** a alteração passa a esconder alimentação inconsistente, devolvendo o mesmo `None`; anteriormente haveria `KeyError`. O produtor normal deriva a fila de `qs_vivos` e grava todas as linhas na partição; ocorrência de ausência entre consultas ou em produção é [não verificado]. [painel.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/painel.py:186), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:52), [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1232)

Os consumidores não rejeitam `None`:

- O juiz conserva `{anc: None}` como porta existente; sem `cobrar` ou `validar`, produz fio `chamado`. `None` também passa pelo filtro de visibilidade, pois não pertence a `FORA_DA_LISTA_PADRAO`. [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1183), [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1233)
- A folha lê somente `porta['fio']`: recebe `chamado`, classifica como C e usa `DESTRAVA['chamado']`. [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:68), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:80), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:199)
- O vigia contabiliza a porta, não exige admin por esse valor e imprime papel literal `"None"`; se houver também `validar`/`decidir`, a interseção ainda exige admin. [vigia_fio.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/vigia_fio.py:599)
- O teste F5B cobre apenas a escada isolada com `{1: None}`; o teste da folha injeta porta já pronta com `None` e exige `[555]`. Esses casos não exercitam a nova ausência de chave em `.get(anc)`. [test_o261e_f5b_juiz_porta.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/tests/test_o261e_f5b_juiz_porta.py:243), [test_haiku_fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/tests/test_haiku_fora_da_folha.py:129)

Item 3 — **O texto novo cobre os dois ramos atuais que resultam em `validar`:** módulo em `MODULOS_DO_ADMIN_VALIDAR` (`gps_fora_geofence`) ou presença no conjunto de respostas creditadas. Auditoria vence antes; pergunta pendente e demais casos resultam em outros verbos. O silêncio substitui qualquer verbo não nulo por `arquivado`/`historico`, retirando-o de `validar`. [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:41), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:29), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:44), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:48)

A ressalva é que “resposta creditada” significa `respondida_em is not None` no crédito, sem verificar texto nesse ponto; a nova frase limita-se ao que o painel põe em Validar. [raiox.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/raiox.py:133), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:41)

## 31/10 03:48 · sessao 388eeedeb856 · - · - · origem=Ronald (tmux)

- **O novo teste não morde a tupla literal atual.** Hoje `FORA_DA_LISTA_PADRAO` é exatamente `('arquivado', 'historico')`; trocar o uso da constante na ficha por essa tupla preserva as duas respostas cobradas pelo teste (`app/chamados/services/verbos.py:13`, `app/chamados/services/verbos.py:18`, `app/colaboradores/services/ficha.py:160`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`).

- **O AST adjacente morde essa escrita literal**, porque proíbe qualquer constante `'historico'`; contudo, exigir que o nome `FORA_DA_LISTA_PADRAO` apareça em algum ponto do módulo não comprova que governa `dono` (`app/colaboradores/tests/test_ficha_do_colab.py:125`, `app/colaboradores/tests/test_ficha_do_colab.py:128`, `app/colaboradores/tests/test_ficha_do_colab.py:129`).

- **O novo teste morde truncar para `FORA_DA_LISTA_PADRAO[:1]`**, pois exige `dono='arquivado'` nos dois casos produzidos pelo silêncio vencido/vigente; o segundo verbo ficaria sem esse dono (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:113`, `app/chamados/services/verbos.py:51`, `app/colaboradores/services/ficha.py:160`).

- **Mudança de conteúdo da constante não é exercitada por mutação.** Os estímulos continuam produzindo somente `HISTORICO` e `ARQUIVADO`; adicionar/remover um membro da constante faz a igualdade final falhar mesmo com a ficha obedecendo à constante. Reordená-la passa, pois a comparação usa conjuntos (`app/chamados/services/verbos.py:51`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`).

- **Há relógio solto transitivo, sem congelamento local:** o teste chama a ficha, que captura `timezone.now()`, e esta chama a partição, que captura outro `timezone.now()`; `self.hoje` vem de `timezone.localdate()` (`app/colaboradores/tests/test_ficha_do_colab.py:30`, `app/colaboradores/tests/test_ficha_do_colab.py:98`, `app/colaboradores/tests/test_ficha_do_colab.py:111`, `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`, `app/chamados/services/verbos.py:35`). O congelamento global depende de `CONGELAR_EM`, e esse arquivo já está na lista de relógios soltos (`app/core/runner_quarentena.py:94`, `app/core/runner_quarentena.py:99`, `app/core/tests/testes_sem_relogio.txt:233`).

## 31/10 03:48 · sessao 052b0de4e58d · - · - · origem=Ronald (tmux)

Item 1: **nenhum leitor residual localizado usando `verbo` como conjunto da fila**. O produtor separa `fila_ids`/`fila_cel`, vindos de `qs_na_fila`, do mapa `verbo`, vindo de `qs_vivos` (`app/chamados/juizes.py:1153`, `app/chamados/juizes.py:1161`).

- O juiz decide pertença por `fila_cel` e `fila_ids`; usa `verbo` apenas para consultar valores e visibilidade (`app/chamados/juizes.py:1227`, `app/chamados/juizes.py:1228`, `app/chamados/juizes.py:1231`, `app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1236`).
- O vigia **já lê `alim['fila_ids']`**; o teste de pertença no véu consulta esse conjunto (`app/chamados/services/vigia_fio.py:567`, `app/chamados/services/vigia_fio.py:620`).
- A folha passa a alimentação ao juiz, sem interpretar `verbo` (`app/folha/fora_da_folha.py:54`, `app/folha/fora_da_folha.py:59`).
- O teste de alimentação usa o produtor real (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`).
- Os dicts manuais localizados são **saídas da porta**, devolvidas por mock do juiz; não simulam `alimentar_portas` (`app/folha/tests/test_haiku_fora_da_folha.py:101`, `app/folha/tests/test_haiku_fora_da_folha.py:106`, `app/folha/tests/test_haiku_fora_da_folha.py:129`, `app/folha/tests/test_haiku_fora_da_folha.py:143`).

## 31/10 03:40 · sessao 6c4eeff007e0 · O261-E-F5C · verificador · origem=Code

A cura separa corretamente **verbo** de **pertencimento à fila**, mas ainda há falhas de custo, garantias textuais excessivas e cobertura incompleta dos testes. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1158`, `app/chamados/services/vigia_fio.py:549`, `app/colaboradores/tests/test_ficha_do_colab.py:110`)

1. **Quem escreve e quem lê o dict**

   | Chave | Escritor | Todos os leitores diretos encontrados |
   |---|---|---|
   | `verbo` | `particionar_verbos(qs_vivos())`, `app/chamados/juizes.py:1158` | Fila, âncora e seleção dos números visíveis: `app/chamados/juizes.py:1224`, `:1228`, `:1232`. |
   | `fila_ids` | Inicialização e inclusão: `app/chamados/juizes.py:1148`, `:1151`; retorno: `:1158` | Porta da pergunta: `app/chamados/juizes.py:1227`; vigia: `app/chamados/services/vigia_fio.py:566`, usado em `:619`. |
   | `fila_cel` | `app/chamados/juizes.py:1149`, `:1153` | Porta da fila: `app/chamados/juizes.py:1223`. |
   | `vivas` | `app/chamados/juizes.py:1154`, `:1157` | Porta: `app/chamados/juizes.py:1225`; vigia: `app/chamados/services/vigia_fio.py:571`, `:576`, `:595`, `:616`. |
   | `colab` | `app/chamados/juizes.py:1159` | Identificação do colaborador para consultar o app: `app/chamados/juizes.py:1227`. |
   | `no_app` | Vazio em `app/chamados/juizes.py:1159`; preenchimento lazy em `:1167` | `_no_app`: `app/chamados/juizes.py:1163`, `:1168`; chamadas pela porta em `:1227` e pelo vigia em `app/chamados/services/vigia_fio.py:580`. |

   A folha e as pautas transportam a alimentação ao juiz; o teste de equivalência também usa alimentação real. (`app/folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `app/ponto/services/pautas_dos_discordantes.py:74`, `:76`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`)

   Não encontrei leitor atual desse dict usando `verbo` como conjunto da fila: os dois pontos relevantes agora usam `fila_ids`. Também não encontrei teste construindo manualmente a **alimentação**; os dicts manuais e mocks encontrados representam a **saída da porta**. (`app/chamados/juizes.py:1227`; `app/chamados/services/vigia_fio.py:566`; `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`)

   **Sobrou uma referência de teste errada:** `_verbo_do_painel` do teste do vigia continua particionando `qs_na_fila()` e chamando isso de verbo do painel. Seus casos simples não expõem o defeito curado. (`app/chamados/tests/test_o261e_f5_alcance.py:281`, `:285`, `:287`)

2. **Igualdade com painel e badge**

   **Painel padrão, sem filtros, em estado estável: sim, o motor recebe o mesmo universo.** O painel monta a base sem filtros e particiona seus vivos; o juiz particiona `qs_vivos()` diretamente. `select_related`, prefetch, anotações e ordenação não introduzem outro recorte; a partição remove o prefetch antes de ler suas colunas. (`app/chamados/services/painel.py:96`, `:135`, `:198`, `:228`; `app/chamados/juizes.py:1158`; `app/chamados/services/verbos.py:44`)

   **Com filtros, pode divergir.** Contraexemplo deduzido: C normal; D urgente e vivo; pergunta respondida com destino D e pai C. Globalmente, D recebe a pergunta e C fica `decidir`. Filtrando urgência normal, D sai do conjunto e o crédito volta a C, que vira `validar`. O crédito prefere o destino somente quando ele pertence ao conjunto recebido. Ocorrência desse cenário em produção: `[nao verificado]`. (`app/chamados/services/painel.py:117`, `:228`; `app/chamados/services/raiox.py:54`, `:56`, `:140`; `app/chamados/catalogo/natureza.py:48`)

   **Empresas visíveis não restringem o painel padrão:** alimentam os seletores. O vigia restringe os dias a essas empresas, mas a alimentação continua particionando todos os vivos. (`app/chamados/services/painel.py:96`, `:127`, `:436`; `app/chamados/services/vigia_fio.py:534`; `app/chamados/juizes.py:1158`)

   **O badge não garante igualdade no instante atual:** normalmente devolve `PlacarNavbar.badge_chamados`; somente sem lavra usa a partição atual. Em outra exceção, devolve simplesmente `qs_na_fila().count()`. Além disso, mostra uma soma de `validar` e `decidir`, sem verbo individual por chamado. (`app/core/context_processors.py:68`, `:84`, `:88`, `:93`; `app/chamados/services/verbos.py:22`, `:25`)

   `registrado` continua vivo e pode receber crédito, embora não abra porta nem apareça na lista padrão. Silêncio vencido pode abrir porta com verbo `historico`; silêncio vigente fica fora da fila. Verbo `None` pode abrir porta para chamado aberto/em análise de auditoria, produzir fio `chamado` e aparecer no painel como `Registro`. (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`, `:314`; `app/chamados/services/verbos.py:48`, `:125`; `app/chamados/juizes.py:1224`, `:1232`; `app/chamados/catalogo/natureza.py:44`, `:63`)

3. **Pertencimento a `qs_na_fila`**

   Com alimentação real e estado estável, **as duas portas continuam exigindo fila**: `fila_cel` e `fila_ids` nascem da mesma iteração de `qs_na_fila`; a primeira porta usa `fila_cel`, a segunda exige `anc in fila_ids`. Não encontrei passagem estável de âncora fora da fila. (`app/chamados/juizes.py:1150`, `:1153`, `:1223`, `:1227`)

   Uma âncora na fila pode legitimamente não abrir a porta da pergunta: ainda são necessárias pergunta viva, presença no app e resposta não vazia quando respondida. A âncora é o destino; o pai só serve quando não existe destino. (`app/chamados/juizes.py:1155`, `:1227`; `app/chamados/services/fio.py:116`)

   **Há uma fragilidade de consistência:** fila e mapa são lidos separadamente, sem revalidação da alimentação. Se o chamado deixar de ser vivo entre essas leituras, a porta da fila pode receber `None` pelo `.get()`, enquanto a porta da pergunta pode lançar `KeyError` pelo acesso direto. Ocorrência em produção: `[nao verificado]`. (`app/chamados/juizes.py:1150`, `:1158`, `:1222`, `:1224`, `:1228`)

4. **A chave `fila` pode nomear chamado oculto no painel padrão?**

   **Não encontrei esse caso em estado estável e sem filtros.** O juiz exige pertencimento à fila e remove `FORA_DA_LISTA_PADRAO`; o painel exige `COBRANCA` e remove a mesma constante. Paginação pode deixar o chamado fora da página corrente, mas não o elimina do conjunto. (`app/chamados/juizes.py:1150`, `:1232`; `app/chamados/services/painel.py:314`, `:326`, `:340`)

   Silêncio vencido conserva a porta, mas perde o número em `fila`; silêncio vigente sequer abre a porta. Isso está coberto pelo teste correspondente. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:51`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:144`)

5. **Checklist das classes de reprovação**

   - **Segundo juiz:** a porta foi registrada e a folha lê seu `fio`. `pergunta_sem_acao_possivel` responde outra pergunta, conforme a ressalva posterior do corte; mantê-la não demonstra duplicação da porta do dia. (`app/core/juizes.py:253`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/raiox.py:151`; `app/docs/BACKLOG.md:778`)
   - **Segundo escritor:** os trechos novos de alimentação/porta escrevem mapas em memória; o command publica pelo carimbador existente. Não identifiquei nova escrita de estado de domínio nesses trechos. (`app/chamados/juizes.py:1148`, `:1167`, `:1233`; `app/chamados/management/commands/vigiar_fio_morto.py:206`)
   - **Relógio fora da borda:** o command captura `agora`, mas passa apenas `hoje`; `qs_na_fila` e `particionar_verbos` continuam lendo relógios próprios. Portanto a rodada não tem um instante único para silêncio/verbos. Os testes congelados não exercitam essa travessia. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `:167`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:35`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:28`)
   - **Derivação paralela:** a tupla de verbos ocultos saiu dos leitores de produção e ficou na constante compartilhada. A permissão do Resolver dia, porém, continua copiada em literal no vigia e no drawer; o teste verifica coincidência textual, sem transformar isso em fonte única. (`app/chamados/services/verbos.py:18`; `app/chamados/services/painel.py:327`; `app/colaboradores/services/ficha.py:160`; `app/chamados/services/vigia_fio.py:184`; `app/colaboradores/services/drawer.py:130`; `app/chamados/tests/test_o261e_f5_alcance.py:357`)
   - **Mordida da cura principal:** os novos casos distinguem a regressão para `particionar_verbos(qs_na_fila())`: nesse mutante C recebe a resposta e deixa de ser `decidir`. O teste da âncora silenciada distingue a troca indevida de `fila_ids` por `verbo`. Essa conclusão é estática; execução Django/RED nesta revisão: `[nao verificado]`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:192`, `:213`, `:226`, `:236`)
   - **Selo fraco da ficha:** o mutante `verbo in FORA_DA_LISTA_PADRAO[:1]` passa nas três asserções AST e classifica `historico` como `registro`. Conferi essas expressões em memória: o teste exige presença do nome e ausência do literal, mas não exige uso integral da constante. (`app/colaboradores/services/ficha.py:160`; `app/colaboradores/tests/test_ficha_do_colab.py:108`, `:110`, `:111`, `:112`)
   - **Ordem de trava:** não identifiquei aquisição nova de trava nos trechos acrescentados; o command compartilha a lista e lê sua falha depois dos dois censos. (`app/chamados/juizes.py:1129`; `app/chamados/services/vigia_fio.py:187`; `app/chamados/management/commands/vigiar_fio_morto.py:166`)
   - **Pré-requisito:** o código consumidor e o juiz estão presentes no pouso fornecido, mas a documentação contradiz a ordem: a linha diz “ANTES da F5” e termina “depois da F5”. A sequência real de pousos, suite e opus não está demonstrada pelos anexos: `[nao verificado]`. (`app/docs/BACKLOG.md:777`; `app/docs/CORTES.md:400`)

6. **Custo**

   **Existe multiplicação comprovável no vigia:** para K pares empresa×competência, `censo_alcance` chama K vezes `fora_da_folha`, que alimenta o juiz; depois alimenta novamente. Havendo dias retidos, são **K+1 partições globais**, inclusive alimentações de lotes vazios. (`app/chamados/services/vigia_fio.py:546`, `:549`, `:565`; `app/folha/fora_da_folha.py:138`; `app/chamados/juizes.py:1146`, `:1158`)

   **Também há consultas por pergunta dentro da alimentação:** o carregamento inclui somente `disputa` e `celula`, mas C6 acessa `disputa.colaborador`, empresa e tranca. Fora de request, o memo pertence à instância do colaborador, permitindo repetir consultas. (`app/chamados/juizes.py:1155`, `:1008`, `:888`, `:896`, `:900`, `:919`; `app/ponto/services/fechamento.py:1438`)

   `_no_app` acrescenta consultas por colaborador; as perguntas do app carregam apenas `disputa`, enquanto o juiz acessa também `celula`, abrindo outro caminho de N+1. (`app/chamados/juizes.py:1163`, `:757`; `app/chamados/services/pergunta_no_app.py:42`)

   Não identifiquei chamada de produto por célula/request; os caminhos encontrados alimentam lotes em vigia, lavrador e pautas. Tempo e quantidade efetiva de consultas em produção: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:565`; `app/folha/management/commands/lavrar_retrato_folha.py:54`; `app/ponto/services/pautas_dos_discordantes.py:74`; `app/folha/views_retrato.py:8`)

7. **Textos que o código não garante**

   - **“Consultas por lote, nunca por dia”** e **“UMA vez por rodada”** excedem as garantias, pelos acessos de C6 e pelas alimentações indiretas acima. (`app/chamados/juizes.py:1130`, `:888`; `app/chamados/services/vigia_fio.py:514`, `:549`; `app/folha/fora_da_folha.py:138`)
   - **“Mesmo universo do painel e do badge”** precisa distinguir painel sem filtros, painel filtrado e badge lavrado. (`app/chamados/juizes.py:1136`, `:1206`; `app/chamados/services/vigia_fio.py:173`; `app/core/context_processors.py:68`)
   - **“Pergunta sem crédito deixa a âncora em decidir”** não vale universalmente: auditoria pode dar `None`, geofence dá `validar`, outra pergunta pode fornecer crédito e silêncio pode sobrepor `historico`. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44`, `:46`, `:48`; `app/chamados/services/verbos.py:51`)
   - **`DESTRAVA['admin']` promete resposta na mesa de disputa**, mas um chamado `gps_fora_geofence` da célula pode produzir `validar`/fio `admin` sem pergunta. O texto não acompanha esse contrato mais amplo. (`app/folha/fora_da_folha.py:41`; `app/chamados/catalogo/natureza.py:46`, `:59`; `app/chamados/juizes.py:1175`, `:1224`)
   - **“Ninguém está esperando ninguém”** também é excessivo: sem essas duas portas ainda pode haver decisão pendente, ausência aberta ou mesa do admin. (`app/folha/fora_da_folha.py:46`; `app/chamados/juizes.py:1214`; `app/chamados/services/vigia_fio.py:288`, `:291`, `:295`)
   - Restaram descrições antigas: grupo C como “chamado vivo sem pergunta viva” e folha julgando diretamente por `.viva`; o leitor atual recebe o fio da porta. (`app/folha/fora_da_folha.py:10`, `:67`; `app/chamados/services/vigia_fio.py:129`)

## 31/10 03:39 · sessao e426ca6c50df · - · - · origem=Ronald (tmux)

Custo confirmado por leitura estática:

- **Partição global repetida:** cada `alimentar_portas` particiona `qs_vivos()` inteiro, mesmo com lote vazio (`app/chamados/juizes.py:1146`, `:1158`). `censo_alcance` chama `fora_da_folha` por empresa×competência (`app/chamados/services/vigia_fio.py:546`), cada chamada alimenta (`app/folha/fora_da_folha.py:138`), e depois alimenta novamente (`app/chamados/services/vigia_fio.py:565`). São K+1 partições quando há dias retidos, sendo K os pares empresa×competência.
- **N+1 dentro da alimentação:** carrega apenas `disputa` e `celula` (`app/chamados/juizes.py:1155`), mas C6 acessa `disputa.colaborador` (`:888`) e `colab.empresa` (`:896`). Fora de request, o memo pertence à instância do colaborador (`:919`), podendo repetir também `periodo_trancado` (`:900`; `app/ponto/services/fechamento.py:1438`).
- **Consultas posteriores:** `_no_app` faz consulta do colaborador e das perguntas por colaborador (`app/chamados/juizes.py:1163`; `app/chamados/services/pergunta_no_app.py:42`). Estas últimas carregam só `disputa`, enquanto C6 acessa `celula` (`app/chamados/juizes.py:757`), introduzindo consultas por pergunta.
- **Não identifiquei chamador de produção por célula ou request** na busca dos símbolos; caminhos encontrados são vigia, lavrador e pautas. A tela declara ler o retrato gravado (`app/folha/views_retrato.py:8`); lavrador chama por empresa×competência (`app/folha/management/commands/lavrar_retrato_folha.py:54`, `:65`; `app/folha/retrato_folha.py:1157`); pautas alimenta por empresa (`app/ponto/services/pautas_dos_discordantes.py:66`, `:74`). Tempo e contagem observados em produção: `[nao verificado]`.

Textos/docstrings que excedem a garantia:

- **“Consultas por lote, nunca por dia”** (`app/chamados/juizes.py:1130`) não descreve os acessos de C6 e `_no_app` acima.
- **“Alimentado UMA vez por rodada”** (`app/chamados/services/vigia_fio.py:514`) ignora as alimentações indiretas por empresa×competência (`:549`; `app/folha/fora_da_folha.py:138`).
- **`DESTRAVA['admin']` promete validar resposta na disputa** (`app/folha/fora_da_folha.py:41`), mas `gps_fora_geofence` recebe `validar` independentemente de pergunta (`app/chamados/catalogo/natureza.py:29`, `:46`, `:59`); chamado desse módulo ligado à célula abre porta e produz fio `admin` (`app/chamados/juizes.py:1223`, `:1175`).
- **Pergunta sem crédito não garante âncora em `decidir`**, como afirma `app/chamados/juizes.py:1211`: auditoria pode resultar em `None`, geofence em `validar`, outra pergunta creditada em `cobrar`/`validar`, e silêncio em `historico` (`app/chamados/catalogo/natureza.py:44`; `app/chamados/services/verbos.py:47`).
- **`DESTRAVA['nada']` diz “ninguém está esperando ninguém”** (`app/folha/fora_da_folha.py:46`), embora ausência de porta ainda possa corresponder a decisão pendente, ausência aberta ou mesa do admin (`app/chamados/services/vigia_fio.py:288`, `:609`). O próprio juiz exclui essas outras entradas do conceito de porta (`app/chamados/juizes.py:1214`).
- **Prosa residual desatualizada:** grupo C ainda é descrito como “chamado vivo sem pergunta viva” (`app/folha/fora_da_folha.py:10`); vigia ainda diz que folha julga via `.viva` (`app/chamados/services/vigia_fio.py:129`), mas o leitor atual lê `porta['fio']` (`app/folha/fora_da_folha.py:67`).

## 31/10 03:38 · sessao df5e7c1b8947 · - · - · origem=Ronald (tmux)

Conclusões dos itens 2 e 4:

- **Sem filtros e no mesmo estado/instante, o motor é igual:** juiz usa `particionar_verbos(qs_vivos())`; painel usa `particionar_verbos(qs_vivos(_base=montar_qs_filtrado(...)))`. Sem filtros, as bases coincidem. `select_related`, `prefetch_related`, ordenação e anotações não acrescentam filtro; a partição remove prefetch e lê colunas. (`app/chamados/juizes.py:1158`; `app/chamados/services/painel.py:96-155,198,228`; `app/chamados/services/verbos.py:34-46`)

- **Filtros podem mudar o verbo do mesmo chamado:** C normal na fila; D urgente vivo; pergunta respondida com destino D e disputa.chamado=C. Global: C=`decidir`, D=`validar`. Com `urgencia=normal`, D sai do universo e o crédito volta para C, que vira `validar`. É consequência dos filtros antes da partição e da preferência pelo destino somente se pertencer ao conjunto recebido. O caso é deduzido do código; ocorrência em produção `[nao verificado]`. (`app/chamados/services/painel.py:117-130,228`; `app/chamados/services/raiox.py:54-56,133-149`; `app/chamados/catalogo/natureza.py:48-64`)

- **Empresas visíveis não filtram a lista padrão nem seu universo de verbos:** servem aos dropdowns. Empresa inativa/de diagnóstico pode entrar no universo global. O vigia, por sua vez, recorta seu universo de dias a empresas visíveis, sem limitar a partição global do juiz. (`app/colaboradores/queries.py:78-87`; `app/chamados/services/painel.py:96-132,436,542`; `app/chamados/services/vigia_fio.py:535-539`; `app/chamados/juizes.py:1158`)

- **O badge normalmente não particiona ao vivo:** devolve `PlacarNavbar.badge_chamados`; só sem lavra usa a partição atual, e em outra exceção devolve a quantidade em `qs_na_fila`. A lavra vem do painel sem filtros e soma apenas `validar`+`decidir`. Portanto igualdade temporal com juiz/painel não é garantida. (`app/core/context_processors.py:68-93,110-114`; `app/core/management/commands/lavrar_badge_navbar.py:39-41,72-74,91-93`; `app/chamados/services/verbos.py:22-27`)

- **Registrado/silêncio:** `registrado` permanece no universo vivo e pode receber crédito, mas sai tanto da porta quanto da lista padrão; silêncio futuro sai da fila, passado entra na fila mas recebe `historico`, depois removido de `fila` e da lista padrão. (`app/chamados/catalogo/motor.py:24-29`; `app/chamados/services/painel.py:155,186-188,314,326-329`; `app/chamados/services/verbos.py:48-51`; `app/chamados/juizes.py:1232`)

- **Verbo `None` não é ausência de porta:** chamado de auditoria aberto/em análise entra na fila, pode aparecer em `fila`, e produz fio `chamado`. Painel mostra `Registro`; resumo transforma `None` em `registro`; badge não o conta como admin. Isso é coerente com a implementação, mas não autoriza tratar todos os verbos de porta como ações. (`app/chamados/catalogo/natureza.py:18-23,44-45,63`; `app/chamados/juizes.py:1224,1232-1234`; `app/chamados/services/painel.py:381-386,472-484`; `app/chamados/services/verbos.py:125-135,22-27`)

- **Item 4:** em estado estável, não encontrei número de `fila` eliminado pelas regras da lista padrão sem filtros. `fila` exige cobrança e remove os mesmos verbos ocultos; painel exige cobrança e remove a mesma constante. Paginação e grupos inicialmente recolhidos podem impedir exibição imediata, sem eliminar o chamado do conjunto. Concorrência entre consultas/requests não tem snapshot garantido. (`app/chamados/juizes.py:1150-1158,1223,1232`; `app/chamados/services/painel.py:314,326-329,340-341,496-497`; `app/templates/chamados/partials/_lista_resumo.html:57-61`)

Falhas de prova/texto:

- O auxiliar de teste do vigia **continua chamando a fila de universo do painel**; seus casos simples não expõem a divergência curada. (`app/chamados/tests/test_o261e_f5_alcance.py:281-299`)
- Os novos testes do juiz comparam outro chamado ao mesmo motor global, sem exercitar `montar_contexto_painel` nem o badge real. Mordem a regressão para `qs_na_fila`, mas não provam a promessa ampla de tela/badge. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87-92,192-224`)
- A afirmação “pergunta viva que o crédito não vê deixa o verbo da âncora em decidir” é excessiva: auditoria dá `None`; geofence dá `validar`; silêncio passado pode sobrepor `historico`; outras perguntas creditadas à âncora também alteram o verbo. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44-51`; `app/chamados/services/verbos.py:48-51`)
- As docstrings que dizem “mesmo universo do painel e badge” precisam restringir painel sem filtros e distinguir o badge lavrado do fallback vivo. (`app/chamados/juizes.py:1135-1139,1206-1208`; `app/chamados/services/vigia_fio.py:172-173`; `app/core/context_processors.py:68-93`)

## 31/10 03:38 · sessao c5fa694cf9ae · - · - · origem=Ronald (tmux)

Contrato de `alimentar_portas` — escritor inicial único: `app/chamados/juizes.py:1148` a `:1159`; leitores por chave:

| Chave | Escrita | Leitura direta |
|---|---|---|
| `verbo` | `particionar_verbos(qs_vivos())['mapa']`, `juizes.py:1158` | `porta_alcancavel_do_dia`: `.get()` na fila, `:1224`; índice na âncora, `:1228`; filtro do número visível, `:1232` |
| `fila_ids` | inicializa/adiciona em `juizes.py:1148`, `:1151`; retorna `:1158` | pertinência da âncora, `juizes.py:1227`; `censo_alcance`, `vigia_fio.py:566`, usado para véu em `:619` |
| `fila_cel` | `juizes.py:1149`, `:1153`, `:1158` | `porta_alcancavel_do_dia`, `juizes.py:1223` |
| `vivas` | `juizes.py:1154` a `:1158` | porta, `juizes.py:1225`; vigia, `vigia_fio.py:571`, com usos em `:576`, `:595`, `:616` a `:645` |
| `colab` | `juizes.py:1159` | porta consulta colaborador da célula em `juizes.py:1227` |
| `no_app` | inicial vazio, `juizes.py:1159`; cache lazy `_no_app`, `:1167` | `_no_app`, `juizes.py:1163`, `:1168`; chamados em porta `:1227` e vigia `vigia_fio.py:580` |

Passadores do contrato, sem acesso direto a suas chaves: `folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `ponto/services/pautas_dos_discordantes.py:74`, `:76`; teste de equivalência de lote em `chamados/tests/test_o261e_f5b_juiz_porta.py:187`, `:188`, `:189`. A chamada avulsa do juiz cria alimentação em `chamados/juizes.py:1222`.

Não encontrei leitor atual deste dict tomando as chaves de `verbo` como conjunto da fila: o antigo caso do vigia agora lê `fila_ids` (`app/chamados/services/vigia_fio.py:566`), e a porta usa `fila_ids` (`app/chamados/juizes.py:1227`). Não encontrei dict manual de alimentação em testes; os dicts manuais e mocks são **saída da porta**: `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`. O teste do lote usa o alimentador real (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`).

Há helper antigo que ainda chama o universo errado e se apresenta como painel: `app/chamados/tests/test_o261e_f5_alcance.py:281` a `:285` particiona `qs_na_fila()`; seus usos estão em `:293`, `:298`. O helper F5B foi corrigido para `qs_vivos()` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87` a `:92`). Isso não é leitor de `alimentar_portas`, mas deixa uma referência de teste enganosa.

Pertinência da porta, com alimentação real e estado estável:

- Fila e âncora continuam provenientes de `qs_na_fila`: `fila_cel` e `fila_ids` nascem da mesma iteração (`app/chamados/juizes.py:1150` a `:1153`); a fila lê `fila_cel` (`:1223`), a pergunta exige `anc in fila_ids` (`:1227`). A âncora é destino, com fallback para chamado da disputa (`app/chamados/services/fio.py:116` a `:117`).
- Estar na fila é condição necessária da âncora, não suficiente da pergunta: ainda exige pergunta viva, resposta não vazia se respondida e presença no app (`app/chamados/juizes.py:1155` a `:1157`, `:1227`). Os testes cobrem resposta vazia e fora do app (`app/chamados/tests/test_o261e_f5_alcance.py:145`, `:194`).
- Silêncio vigente fica fora da fila; silêncio vencido pode estar nela, pois o predicado exclui somente `silenciado_ate > now` (`app/chamados/services/painel.py:186` a `:188`). O vencido conserva porta, mas perde número em `fila` se o verbo for `historico` (`app/chamados/juizes.py:1232`; teste `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).
- `registrado` está em `VIVOS`, mas não em `COBRANCA`, portanto não entra em `qs_na_fila` (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`), coberto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:250` a `:257`.

Ressalva de consistência: fila e mapa de verbos são consultas separadas (`app/chamados/juizes.py:1150`, `:1158`), e `alimentacao` fornecida não é revalidada (`:1222`). Se um chamado deixar `VIVOS` entre essas consultas, a fila pode produzir verbo `None` pelo `.get()` (`:1224`), enquanto a mesma situação na porta da pergunta pode lançar `KeyError` pelo índice (`:1228`); ocorrência em produção `[nao verificado]`.

## 31/10 03:37 · sessao fa4f12636b7a · O261-E-F1C4 · verificador · origem=Code

**Não aprovaria o delta:** a nova guarda da ressurreição elimina uma recuperação legítima prevista pelas portas existentes; além disso, protege apenas o chamador cartório, deixando outro caminho capaz de desfazer `celula_absolveu`. São conclusões estáticas; ocorrência em produção [não verificado]. (`app/ponto/services/cartorio.py:1355`; `app/chamados/models.py:1232`; `app/chamados/models.py:1389`; `app/chamados/models.py:1453`)

1. **Tranca: mesma autoridade. Isenção da resposta: regra copiada.**

O E2 chama **a mesma função** `dia_da_pergunta_trancado(p)` usada no trecho indicado do emissor. Essa função determina a competência por `janela_atual`, consulta `periodo_trancado` e considera o toque humano posterior. `periodo_trancado` é a autoridade registrada em `_K7`; o filtro novo não reproduz sua lógica. (`app/chamados/reconciliador.py:579`; `app/chamados/reconciliador.py:617`; `app/chamados/services/disputa_emissao.py:1541`; `app/chamados/services/disputa_emissao.py:1551`; `app/chamados/juizes.py:891`; `app/chamados/juizes.py:907`; `app/core/juizes.py:283`)

Já a isenção do `y` é **uma segunda expressão da mesma regra**: o laço testa texto com `any((p.resposta_colab or '').strip()...)`; a compreensão repete isso para `y`. Não divergem hoje, mas não consultam um predicado compartilhado. Portanto, o comentário “a MESMA isenção” descreve equivalência atual, não unicidade de implementação. (`app/ponto/services/cartorio.py:1328`; `app/ponto/services/cartorio.py:1344`; `app/ponto/services/cartorio.py:1346`)

2. **O novo `_protesta(x)` tem contraexemplo de regressão.**

O problema é a unidade consultada: `_protesta` usa **a célula do dia em julgamento**, enquanto a porta de ressurreição examina **o marco de cada pergunta**. Essas datas podem legitimamente diferir numa jornada que cruza a meia-noite. ([cartorio.py:1156](/home/ronald/codex_ws/7b76efa7d2dcaaa47c339314360f3867852d8dd0/app/ponto/services/cartorio.py:1156); `app/chamados/models.py:1403`; `app/chamados/models.py:1410`; `app/chamados/juizes.py:1719`)

Contraexemplo permitido pelo código:

- Há um `turno_aberto` em D+1, dia de folga, cobrando a véspera D ainda furada. A premissa preserva explicitamente `turno_aberto*` nessa situação: não regulariza automaticamente pela folga e consulta a véspera. (`app/chamados/models.py:1232`; `app/chamados/juizes.py:424`)
- Sua resolvedora pertence a D, tem o marco apagado, está materializada e validada, sem texto, pela via `completude_turno`. Esse estado é usado pelos próprios testes de ressurreição; o C6 inicialmente a considera morta por `validada_em`. (`app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:93`; `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:104`; `app/chamados/juizes.py:1006`)
- Sendo o único chamado da cobrança, ele permanece vivo: a premissa segue furada e não há outro sobrevivente para justificar dedup. (`app/chamados/models.py:699`; `app/chamados/models.py:709`; `app/chamados/models.py:728`)
- Antes da guarda acrescentada, a porta limpava os carimbos dessa pergunta; com a célula de D acusando, ela voltava a viver. Agora `_protesta=False`, porque **D+1 é folga**, e o `continue` impede essa recuperação. ([cartorio.py:1355](/home/ronald/codex_ws/7b76efa7d2dcaaa47c339314360f3867852d8dd0/app/ponto/services/cartorio.py:1355); `app/chamados/models.py:1453`; `app/chamados/juizes.py:1056`)

Logo, “não protesta neste dia” não basta para concluir “nenhuma pergunta deste chamado deve ressuscitar”. O próprio coletor da premissa já distingue o caso da véspera; a guarda nova perde essa distinção. (`app/ponto/services/cartorio.py:1157`; `app/chamados/models.py:1232`; `app/chamados/juizes.py:424`)

O censo direto de produção é:

| Operação | Chamadores |
|---|---|
| `ressuscitar_perguntas()` | Cartório, `app/ponto/services/cartorio.py:1365`; `ChamadoColaborador.reabrir`, `app/chamados/models.py:1389`. |
| `perguntas_a_ressuscitar()` | Comando de reabrir órfãs, para detectar irmãs que voltariam junto: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. |
| Universo compartilhado | Wrapper de leitura e escritor consultam `_universo_da_ressuscitacao`: `app/chamados/models.py:1398`; `app/chamados/models.py:1444`. |

A entrada `reabrir()` recebe pedidos de renascimento, desarquivamento, adesão, disputa administrativa, competência, resposta visível e comandos de reparação; todos podem alcançar a ressurreição quando suas guardas permitem. (`app/chamados/models.py:978`; `app/chamados/views_arquivar.py:68`; `app/chamados/services/adesao.py:92`; `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/competencia_trancada.py:258`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`; `app/ponto/management/commands/reconciliar_grade.py:522`; `app/ponto/management/commands/detectar_ausencias.py:301`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`)

**Existe caminho residual de apagar o carimbo:** `reabrir → ressuscitar_perguntas`. O universo da porta exige materialização, resposta nula/vazia e marco não comprovadamente aceso; não exclui `celula_absolveu` nem exige C6 viva. A escrita apaga a via e os carimbos. (`app/chamados/models.py:1389`; `app/chamados/models.py:1403`; `app/chamados/models.py:1410`; `app/chamados/models.py:1453`)

Um contraexemplo é um container `disputa_supervisao_manual`, fora do reconhecimento de chamado de furo, com resolvedora sem texto, célula `concorde` definitiva e motivo sem marco próprio. O E2 carimba; uma reabertura aceita apaga; o próximo E2 carimba novamente. A guarda de premissa da reabertura é condicionada a `e_chamado_de_furo`, portanto não cobre todo container. Isso demonstra a possibilidade de vai-e-volta do carimbo; repetição automática na mesma madrugada [não verificado]. (`app/chamados/catalogo/modulos.py:146`; `app/chamados/models.py:1192`; `app/chamados/models.py:1296`; `app/chamados/juizes.py:842`; `app/chamados/juizes.py:1068`; `app/chamados/reconciliador.py:623`; `app/chamados/models.py:1380`; `app/chamados/models.py:1453`)

Os outros desfazedores têm restrições diferentes: `desfazer_carimbo_sem_lastro` exige C6 viva; competência e fantasma restringem a via; resposta restringe a `VIAS_SEM_LASTRO`; devolução de absorção seleciona `ausencia_dia`/`declaracao`. Eles não demonstram esse mesmo ciclo de `celula_absolveu` sob célula definitiva estável. (`app/chamados/services/relavra_via.py:227`; `app/chamados/models.py:2544`; `app/chamados/models.py:2514`; `app/chamados/models.py:2534`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/materializacao.py:628`)

3. **Quem escreve e quem lê.**

`celula_absolveu` é valor de `PerguntaDisputa.via_resolucao`. “Retratada”, nesse percurso, designa uma operação; não é o nome do campo gravado pelo delta. (`app/chamados/models.py:2212`; `app/chamados/models.py:2262`; `app/chamados/reconciliador.py:505`)

| Campo/estado | Quem escreve | Quem lê |
|---|---|---|
| Pergunta: `via_resolucao`, `materializada_em`, `materializada_por`, na absolvição | E2 carimba e salva: `app/chamados/reconciliador.py:621`. | C6 e juiz de lastro: `app/chamados/juizes.py:603`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1080`; tradução em `estado`: `app/chamados/models.py:2439`. |
| Pergunta: carimbos desfeitos na ressurreição | A porta limpa `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao`: `app/chamados/models.py:1453`. | Universo da porta: `app/chamados/models.py:1403`; C6: `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1080`. |
| Pergunta: `resposta_colab`/`respondida_em` | Escritor da resposta: `app/chamados/services/disputa_emissao.py:1247`; correção limpa resposta/carimbos: `app/chamados/services/disputa_emissao.py:1358`. | Isenções do cartório: `app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1346`; porta de ressurreição: `app/chamados/models.py:1406`; C6: `app/chamados/juizes.py:1020`. |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Núcleo `_gravar_estado`: `app/chamados/models.py:222`, `app/chamados/models.py:237`, `app/chamados/models.py:279`. `reconciliar` e `reabrir` delegam: `app/chamados/models.py:618`, `app/chamados/models.py:1380`. | Cartório: `app/ponto/services/cartorio.py:1323`, `app/ponto/services/cartorio.py:1342`; signals: `app/chamados/signals.py:33`, `app/chamados/signals.py:81`; auditor: `app/chamados/management/commands/auditar_invariantes_chamados.py:571`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1066`. |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | E2 pede `fechar`: `app/chamados/reconciliador.py:639`; porta grava: `app/chamados/models.py:1809`; ressurreição pede reabertura: `app/chamados/models.py:1479`, `app/chamados/models.py:2035`. | E2: `app/chamados/reconciliador.py:627`; receiver que tenta fechar o pai: `app/chamados/signals.py:28`; ressurreição: `app/chamados/models.py:1479`. |
| `out['fio_nao_protesta']` | Incrementado por chamado que alcança a guarda: `app/ponto/services/cartorio.py:1322`, `app/ponto/services/cartorio.py:1362`. | Só a leitura do próprio incremento foi localizada; consumidor específico [não verificado]. `julgar_celula` devolve o dicionário, mas o resumo do comando omite a chave: `app/ponto/services/cartorio.py:1668`; `app/ponto/management/commands/processar_cartorio.py:66`. |

Não há novo escritor de status fora da porta nesse delta: `y.reconciliar()` converge no núcleo existente. O E2 também já está declarado como porta do carimbo de absolvição. Isso **não torna `via_resolucao` um campo de escritor único**: o próprio registro declara essa dívida. (`app/ponto/services/cartorio.py:1348`; `app/chamados/models.py:618`; `app/core/portas.py:306`; `app/core/portas.py:352`)

4. **Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não no filtro da tranca:** ambos consultam a mesma função e `_K7`. (`app/chamados/reconciliador.py:617`; `app/chamados/services/disputa_emissao.py:1551`; `app/core/juizes.py:283`) |
| Segundo escritor fora da porta | **Não acrescentado para status.** Há, porém, política nova em apenas um chamador da ressurreição, enquanto o outro continua apagando o mesmo estado. (`app/chamados/models.py:618`; `app/ponto/services/cartorio.py:1355`; `app/chamados/models.py:1389`) |
| Relógio fora da borda | **Permanece no sítio tocado:** E2 usa `_tz.now()` internamente; a função não recebe instante. É anterior a este delta, mas contraria a regra fornecida. (`app/chamados/reconciliador.py:551`; `app/chamados/reconciliador.py:621`; `app/docs/CORTES.md:369`) |
| Derivação paralela de fonte única | **Sim:** isenção de resposta repetida; e `_protesta` usado como critério global de ressurreição perde a distinção da véspera preservada pela premissa. (`app/ponto/services/cartorio.py:1328`; `app/ponto/services/cartorio.py:1346`; `app/ponto/services/cartorio.py:1157`; `app/chamados/models.py:1232`) |
| Selo/RED sem mordida suficiente | **Cobertura incompleta.** Os casos FIO-DUP citados criam perguntas sem resposta, portanto não exercitam a isenção nova do `y`. A flexibilização do prefixo preserva as verificações de sobrevivente e trilha. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:283`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:298`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:302`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:319`) |
| Ordem de trava | Inversão nova **não demonstrada**. O fechamento continua pelo `atomic` do gate e pela ordem documentada chamado → pergunta. Ausência de deadlock em execução [não verificado]. (`app/chamados/models.py:614`; `app/chamados/models.py:261`) |
| Pouso antes do pré-requisito | RED/GREEN, suite e pouso deste hash **[não verificado]**. Os pré-requisitos estão escritos, mas o anexo não comprova seu cumprimento. (`CLAUDE.md:28`; `CLAUDE.md:542`) | [nao verificado]

O teste novo da tranca verifica corretamente **não carimbar**. Ele não prova “destrancar devolve viva”: sua célula é `concorde`, definitiva e com todas as lâmpadas acesas; mesmo sem tranca, o C6 continua considerando essa pergunta morta. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:80`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:89`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:92`; `app/chamados/juizes.py:1068`)

## 31/10 03:34 · sessao 95afd20c65d1 · - · - · origem=Ronald (tmux)

Mapa de campos e portas:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| `celula_absolveu` | É valor de `PerguntaDisputa.via_resolucao`, definido em `app/chamados/models.py:2212`; E2 grava junto com `materializada_em` e `materializada_por` em `app/chamados/reconciliador.py:621`–625. | C6 lê via/carimbo em `app/chamados/juizes.py:1015`, `:1044`, `:1050`, `:1080`; lastro consulta `VIAS_SEM_TESTEMUNHA` em `app/chamados/juizes.py:603`, cujo conjunto contém essa via em `app/chamados/models.py:2875`. |
| Ressuscitação da pergunta | `ressuscitar_perguntas` limpa `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao` em `app/chamados/models.py:1453`; reabre a disputa em `:1479`. | Universo da própria porta lê materialização, texto e lâmpada em `app/chamados/models.py:1403`; C6 lê os carimbos em `app/chamados/juizes.py:1020`, `:1044`, `:1050`, `:1080`; propriedade `estado` traduz o juiz em `app/chamados/models.py:2432`. |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Núcleo `_gravar_estado`: status em `app/chamados/models.py:222`, fechamento em `:237`–243 e persistência em `:279`. `reconciliar` delega em `:618`; reabrir delega em `:1380`. | Delta lê status em `app/ponto/services/cartorio.py:1323`, `:1342` e chama a porta em `:1348`. Signals leem em `app/chamados/signals.py:33`, `:70`, `:81`; auditor lê fechamento em `app/chamados/management/commands/auditar_invariantes_chamados.py:571` e via em `:1066`. |
| Disputa: fechamento | E2 pede fechamento pela porta em `app/chamados/reconciliador.py:639`; porta grava `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` em `app/chamados/models.py:1809`. | Juiz `pode_fechar` em `app/chamados/models.py:1737`; receiver lê fechamento e pede fechamento do chamado-pai em `app/chamados/signals.py:28`–45; ressuscitação lê disputa fechada em `app/chamados/models.py:1479`. |
| `out['fio_nao_protesta']` | Incremento em `app/ponto/services/cartorio.py:1362`. | A única ocorrência nominal na cópia é o próprio incremento; sem consumidor específico localizado. O resultado completo é devolvido em `app/ponto/services/cartorio.py:1668`, mas o resumo do comando não inclui a chave em `app/ponto/management/commands/processar_cartorio.py:66`–68. |

`retratada` não é campo nem via da pergunta no modelo: as vias declaradas estão em `app/chamados/models.py:2227`–2244. Retratação de chamado é operação `_retratar`, delegando a `reconciliar`, resolução de perguntas e nota em `app/chamados/reconciliador.py:505`–542; deduplicação de pergunta usa `VIA_DUPLICATA` em `app/chamados/services/fio.py:878`–897.

O delta não introduz segundo escritor de status fora da porta: chama `y.reconciliar()` em `app/ponto/services/cartorio.py:1348`, que converge no núcleo em `app/chamados/models.py:618`. E2 também já é porta registrada para o carimbo de absolvição em `app/core/portas.py:306`–310. Contudo, **o campo `via_resolucao` não tem escritor único**: a dívida existente é explicitamente reconhecida em `app/core/portas.py:352`–356; as portas de pergunta autorizadas aparecem em `:388`–395.

Relógio interno nos sítios alcançados: E2 em `app/chamados/reconciliador.py:621`, núcleo de chamado em `app/chamados/models.py:238`, fechamento da disputa em `:1809`; essas leituras não foram acrescentadas por este delta, conforme anexo fornecido [não verificado para execução/produção].

## 31/10 03:34 · sessao ea87990a7bc2 · - · - · origem=Ronald (tmux)

Censo de chamadas de produção na cópia:

| Porta | Chamador direto |
|---|---|
| `ressuscitar_perguntas()` | `ChamadoColaborador.reabrir`, após gravar o estado reaberto: `app/chamados/models.py:1380`, `app/chamados/models.py:1389`. |
| `ressuscitar_perguntas()` | HX-CARTORIO-RESSUSCITA: `app/ponto/services/cartorio.py:1365`. |
| `perguntas_a_ressuscitar()` | Guarda que impede reabrir irmãs fora do escopo no comando: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. |
| Universo compartilhado | O wrapper de leitura e a porta usam `_universo_da_ressuscitacao`: `app/chamados/models.py:1398`, `app/chamados/models.py:1444`. |

O universo da porta seleciona pergunta materializada, resposta nula/vazia e marco que não seja comprovadamente aceso; **não consulta C6, veredito, status do chamado nem via de resolução**. Portanto inclui `celula_absolveu` com marco `False` ou `None`: `app/chamados/models.py:1403`, `app/chamados/models.py:1410`. A escrita apaga `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao`: `app/chamados/models.py:1453`.

**Existe caminho residual de desfazer `celula_absolveu` fora do novo `_protesta`: `reabrir → ressuscitar_perguntas`.** O novo filtro protege apenas o chamador cartório: `app/ponto/services/cartorio.py:1355`; a outra chamada permanece em `app/chamados/models.py:1389`.

Contraexemplo estático, sem afirmar ocorrência em produção:

1. Chamado de container, por exemplo `disputa_supervisao_manual`, com categoria/subcategoria vazias, resolvido pelo sistema, sem irmão nem agrupamento; esses módulos são containers e estão fora de `CALADOS_POR_ISENCAO`: `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:232`. A guarda de premissa só se aplica a `e_chamado_de_furo`, cujo reconhecimento depende do módulo/categoria/subcategoria: `app/chamados/models.py:1192`, `app/chamados/models.py:1296`.
2. Uma resolvedora sem texto aponta para célula `concorde` definitiva e tem motivo sem marco próprio ou marco ausente/apagado. C6 mata pela célula definitiva: `app/chamados/juizes.py:1068`. E2 carimba `celula_absolveu`: `app/chamados/reconciliador.py:611`, `app/chamados/reconciliador.py:623`.
3. Uma reabertura aceita grava o novo estado e chama a porta: `app/chamados/models.py:1377`, `app/chamados/models.py:1389`. Um chamador real é a reabertura administrativa de disputa: `app/chamados/services/acoes_disputa.py:265`.
4. O marco `False`/`None` deixa a resolvedora no universo, a porta limpa o carimbo e a passada seguinte do E2 volta a carimbá-la: `app/chamados/models.py:1410`, `app/chamados/models.py:1453`, `app/chamados/reconciliador.py:623`. Existência desse conjunto em produção: **[não verificado]**.

Não generalizar esse contraexemplo a todo chamado de furo: a reabertura desses passa por `premissa_regularizada`, que lê a célula atual, decisão administrativa, dia sem trabalho, marcos acesos/inexistentes e `_celula_furada`; uma premissa regularizada recusa a reabertura: `app/chamados/models.py:1228`, `app/chamados/models.py:1249`, `app/chamados/models.py:1296`.

Chamadores indiretos de `reabrir()` que podem chegar à porta, sujeitos às respectivas guardas:

- Renascimento por premissa: `app/chamados/models.py:978`; seus chamadores são o dedup de abertura e resposta tardia: `app/chamados/models.py:881`, `app/chamados/services/resposta_tardia.py:45`.
- Desarquivar tranca: `app/chamados/views_arquivar.py:68`.
- Adesão: `app/chamados/services/adesao.py:92`.
- Reabrir disputa administrativa: `app/chamados/services/acoes_disputa.py:266`.
- Reabrir competência: `app/chamados/services/competencia_trancada.py:258`.
- HX-RESPOSTA-VISIVEL: `app/chamados/services/disputa_emissao.py:1606`.
- Reabrir órfãs enterradas: `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.
- Reconciliar grade: `app/ponto/management/commands/reconciliar_grade.py:522`.
- Detector de ausência, ramo de âncora: `app/ponto/management/commands/detectar_ausencias.py:301`.
- Reabrir chamados de ausência aberta: `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`.

Outros desfazedores não demonstram esse mesmo ciclo em estado estável:

- `desfazer_carimbo_sem_lastro` relê a pergunta travada e só limpa se C6 a considera viva; C6 morta por célula definitiva impede a limpeza: `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`.
- `reabrir_por_competencia` só desfaz `competencia_trancada`: `app/chamados/models.py:2544`.
- `reabrir_fantasma` só desfaz `fantasma`, e seu comando exige marco apagado e C6 viva: `app/chamados/models.py:2514`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:77`.
- `reabrir_por_resposta` só desfaz `VIAS_SEM_LASTRO`: `app/chamados/models.py:2534`.

Os testes da porta cobrem marco aceso versus apagado, ausência de célula, idempotência e reabertura da disputa; esses casos não são um teste de `E2 → reabrir → E2` com célula definitiva que absolve: `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:104`, `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:147`, `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:167`, `app/ponto/tests/test_hx_cartorio_ressuscita.py:85`.

## 31/10 03:34 · sessao 0ac2dc173d1d · - · - · origem=Ronald (tmux)

Achado de regressão por análise estática: o novo guarda confunde **dia do chamado** com **dia/marco da pergunta**. `_protesta` lê a célula da volta do cartório e retorna falso em qualquer folga (`app/ponto/services/cartorio.py:1156-1157`), enquanto o universo da ressurreição lê cada pergunta e seu marco (`app/chamados/models.py:1403-1410`).

Contraexemplo válido pelas portas existentes:

- Um `turno_aberto` está ancorado em D+1, folga; a véspera D tem saída apagada. A própria premissa preserva essa cobrança: exclui `turno_aberto*` da regularização automática por folga (`app/chamados/models.py:1232`) e consulta a véspera (`app/chamados/juizes.py:424-428`). A resolvedora em D pode legitimamente resolver o chamado em D+1 (`app/chamados/juizes.py:1719-1725`).
- A pergunta está materializada e validada, sem resposta escrita, por `celula_absolveu` ou `completude_turno`; assim seu fio está morto por `validada_em` (`app/chamados/juizes.py:1006-1007`). Esse é um estado explicitamente coberto pelos testes existentes de ressurreição correta (`app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:93-117`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:288-291`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:315-327`).
- Sozinho, o chamado permanece vivo: premissa furada não permite reconciliar; o próprio sobrevivente C8 não fecha por dedup (`app/chamados/models.py:699-731`).
- Antes, o laço chegava a `ressuscitar_perguntas`, cuja porta limpa os carimbos e devolve a saída apagada à fila (`app/chamados/models.py:1453-1454`). Agora encontra `_protesta=False` e sai antes (`app/ponto/services/cartorio.py:1355-1365`).

Portanto há chamado corretamente ressuscitável que perde a recuperação automática; ocorrência e quantidade em produção: [não verificado]. O comentário que justifica o guarda pressupõe “célula que não acusa” sobre a pergunta, mas o predicado consulta a célula da volta do chamado (`app/ponto/services/cartorio.py:1156-1157`, `app/ponto/services/cartorio.py:1358-1361`).

Sobre a isenção do `y`: é **a mesma expressão lógica copiada**, não uma chamada à mesma autoridade. O laço verifica `any((p.resposta_colab or '').strip()...)` em `app/ponto/services/cartorio.py:1328`; a compreensão repete o teste em `app/ponto/services/cartorio.py:1346-1347`. Não encontrei divergência atual entre essas duas expressões; a duplicação permite evolução independente nesses dois sítios (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1346-1347`).

O teste flexibilizado de `[FIO-DUP]` para `[FIO-DUP` continua verificando sobrevivente, terminal e trilha de dedup; portanto essa edição, isoladamente, não torna o teste vazio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:302-307`). Ela não testa a proteção de resposta do `y`: suas duas perguntas são criadas sem texto (`app/ponto/tests/test_fechar3_troca_escala_gate.py:283-289`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:298-299`). O outro caso FIO-DUP-C8 também cria a pergunta do velho sem texto (`app/ponto/tests/test_fechar3_troca_escala_gate.py:319-320`).

O contador novo é incrementado pelo número de **chamados** que atingiram o guarda, não por perguntas (`app/ponto/services/cartorio.py:1322`, `app/ponto/services/cartorio.py:1362`). Seu único uso nominal encontrado foi a própria leitura/incremento; leitor externo ou selo dedicado: [não verificado].

## 31/10 03:26 · sessao 62aed3dacb80 · FIO-ESTADO-LE-O-ALCANCE · verificador · origem=Code

**O escritor F5 já existe; a manchete e o selo ainda seguem o contador antigo.** O comando grava `censo.fio_morto_alcance` e `alcance`, mas o ESTADO continua imprimindo `fio_morto`. O próprio teste F5 caracteriza **antigo=0 e alcance=1** — portanto os números não são intercambiáveis. (`app/chamados/management/commands/vigiar_fio_morto.py:216-218`; `bin/fio_estado.py:101-104`; `app/chamados/tests/test_o261e_f5_alcance.py:430-434`.)

O anexo não contém uma medição produtiva: valores atuais, último recarimbo e resultado publicado em produção são **[nao verificado]**. [nao verificado]

| Campo | Quem escreve | Quem lê atualmente |
|---|---|---|
| `censo.fio_morto_alcance` | `censo_alcance()` inicializa em zero e incrementa os dias sem porta; o comando copia para `censo`. (`app/chamados/services/vigia_fio.py:539`, `:649`; `app/chamados/management/commands/vigiar_fio_morto.py:217`.) | O comparador genérico o consulta **somente se estiver em `esperado`**. Não integra o retorno nominal nem a manchete. (`bin/fio_estado.py:72-92`, `:101-104`.) |
| `alcance.alcance_cego` | Calculado como `not dias`; persistido no bloco inteiro. (`app/chamados/services/vigia_fio.py:553`; `app/chamados/management/commands/vigiar_fio_morto.py:218`.) | `linhas_alcance()` lê o resultado em memória para a saída do comando; o leitor host ignora esse bloco. (`app/chamados/services/vigia_fio.py:662-664`; `bin/fio_estado.py:69-92`.) |
| `alcance.alcance_universo` | `len(dias)`, dos dias retidos por empresa visível e competência aberta; persistido em `alcance`. (`app/chamados/services/vigia_fio.py:534-552`; `app/chamados/management/commands/vigiar_fio_morto.py:218`.) | Usado na saída textual do comando, mas ignorado pelo leitor host. (`app/chamados/services/vigia_fio.py:659-662`; `bin/fio_estado.py:69-92`.) |
| `esperado` | O comando declara os esperados antigos, acrescenta `fio_morto_alcance=0` e grava uma cópia. (`app/chamados/management/commands/vigiar_fio_morto.py:87-95`, `:211`.) | `ler()` compara todas as chaves presentes com `censo`; `linha()` ainda seleciona o esperado antigo para a manchete. (`bin/fio_estado.py:70-73`, `:103`.) |
| `medido_em` | O relógio é capturado **antes dos censos** e serializado em horário local no carimbo. É instante de início, não de conclusão. (`app/chamados/management/commands/vigiar_fio_morto.py:162-168`, `:207`.) | `ler()` calcula idade; `linha()` mostra data, idade e aviso de velho. (`bin/fio_estado.py:74-84`, `:112-117`.) |
| `censo.fio_morto` antigo | `censo()` parte dos chamados vivos com célula, inicializa e incrementa o contador; o comando preserva `d` no carimbo. (`app/chamados/services/vigia_fio.py:317`, `:340`, `:417`; `app/chamados/management/commands/vigiar_fio_morto.py:216`.) | Ainda determina a manchete e participa da comparação com `esperado`. O alarme também consulta o contador em memória. (`bin/fio_estado.py:72-73`, `:86`, `:101-104`; `app/chamados/management/commands/vigiar_fio_morto.py:82`, `:170-171`.) |

Os pontos de falha são estes:

1. **Carimbo anterior ao F5 pode parecer bom sem medir alcance.** Sem `alcance` e sem o novo esperado, o leitor aceita o arquivo como `medido=True`, compara apenas as chaves antigas e pode mostrar `fio morto: 0` sem divergências. O selo chama exatamente essa combinação de “VERDE”; não existe uma validação de versão/esquema. (`bin/fio_estado.py:69-92`, `:101-104`; `bin/tests/test_fio_estado.sh:40-47`, `:56-60`.)

2. **Universo vazio produz zero, mas significa cegueira.** O produtor retorna `fio_morto_alcance=0`, `alcance_universo=0` e `alcance_cego=True`. Hoje o host ignora os dois últimos campos; olha somente `censo.cego`, que pertence ao universo antigo. Assim, igualdade `0=0` pode coexistir com ausência de universo de alcance, sem aviso correspondente. (`app/chamados/services/vigia_fio.py:538-555`; `bin/fio_estado.py:72-73`, `:91`, `:109-111`.)

   O contrato precisa também fechar **subchaves ausentes ou incoerentes**: só exigir o bloco `alcance` e rejeitar `alcance_cego=True` não garante que exista `alcance_universo>0`. O escritor fornece ambas; o leitor atual não verifica nenhuma. (`app/chamados/services/vigia_fio.py:552-555`; `bin/fio_estado.py:69-92`.)

3. **Contador ausente/`None` não suprime a manchete numérica.** Se seu esperado existir, `None != 0` aparece em `FORA DO ESPERADO`, mas a manchete continua mostrando o número antigo. Se faltar também `esperado.fio_morto_alcance`, nem essa divergência será calculada. (`bin/fio_estado.py:72-73`, `:101-108`.)

   **`esperado` ausente vira `{}`**, deixando a comparação vazia. Portanto a presença do esperado do novo contador também precisa ser condição de medição válida; ausência não pode virar aprovação por falta de comparação. (`bin/fio_estado.py:70-73`.)

4. **Recorte é protegido pelo escritor, não pelo conteúdo validado pelo leitor.** `--empresa` vira uma lista e só `empresas is None` permite carimbar; recortes declaram “NAO carimbado”. Há teste específico dessa proteção. Portanto o comando atual não produz normalmente um carimbo de recorte nesse destino. (`app/chamados/management/commands/vigiar_fio_morto.py:152-156`, `:205`, `:223-225`; `app/chamados/tests/test_o261_vigia_fio.py:801-814`.)

   Se um arquivo recortado chegar ao caminho por outra origem, o leitor não valida escopo/proveniência e pode publicar seu número como frota. A ocorrência disso em produção é **[nao verificado]**. (`bin/fio_estado.py:63-92`.)

5. **A proteção temporal não cobre data ausente ou inválida.** Nesses casos, a idade fica desconhecida, `velho=False` e a manchete permanece numérica. Não há fallback efetivo para `hoje`. Com data válida antiga, aparece a advertência de velho, mas o número continua na manchete; a idade ainda é arredondada antes da comparação com 26 horas. (`bin/fio_estado.py:74-84`, `:101-117`.)

6. **“Exceção de leitura = ILEGIVEL” não está implementado.** Ausência, erro de abertura/permissão e JSON inválido convergem para `medido=False` e **NUNCA MEDIDO**. JSON de formato errado pode lançar `AttributeError`; `medido_em` não textual pode lançar `TypeError`, pois a captura posterior aceita apenas `ValueError`. Essas falhas podem interromper o gerador antes da publicação. (`bin/fio_estado.py:63-80`, `:97-100`; `bin/gerar_estado.py:260-261`, `:364`.)

   Exceção no produtor antes da gravação também pode deixar o carimbo anterior intacto e ainda dentro das 26 horas. Já uma rodada que apenas **alarma** grava antes de sair: esse caso preserva a medição. (`app/chamados/management/commands/vigiar_fio_morto.py:167-189`, `:205-228`; `bin/fio_estado.py:74-84`.)

7. **O selo atual não prova a mudança proposta.** Suas fixtures não contêm alcance; os casos numéricos verificam `medido`, `velho` e nomes de divergências, sem conferir a manchete. O caso CEGO só procura essa palavra e permite conservar o número. Um mutante que restitua apenas a manchete antiga não seria detectado por essas verificações. (`bin/tests/test_fio_estado.sh:40-51`, `:56-91`, `:104-107`.)

   A fixture decisiva deve ter **antigo=0/alcance=N** e conferir a manchete publicada; o caso inverso distingue o novo resultado da regressão antiga. A primeira divergência já está caracterizada nos testes F5. (`bin/fio_estado.py:101-108`; `app/chamados/tests/test_o261e_f5_alcance.py:430-434`.)

Sobre outro leitor: o ESTADO delega a `ler()`/`linha()`, e os modos JSON e `--linhas` usam o mesmo `ler()`. Não identifiquei outro leitor operacional do arquivo; consumidores externos ou construídos inteiramente por caminhos dinâmicos são **[nao verificado]**. (`bin/gerar_estado.py:260-261`; `bin/fio_estado.py:64-65`, `:123-129`.)

**Existe, porém, uma regra operacional diferente no alarme**, que mede ao vivo e não lê o carimbo: cobra `cego` e passivos antigos, deixando `alcance_cego` e `fio_morto_alcance` fora do alarme. Logo um ESTADO corrigido poderá acusar alcance cego ou passivo enquanto o comando não alarma por esses campos — divergência atualmente explícita e testada. (`app/chamados/management/commands/vigiar_fio_morto.py:81-95`, `:167-171`, `:227-228`; `app/chamados/tests/test_o261e_f5_alcance.py:454-455`.)

_881 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
