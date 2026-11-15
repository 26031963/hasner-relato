# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 09:45** · publicado em **15/11/2026 09:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 09:45 · sessao 0dbb05660749 · - · - · origem=Ronald (tmux)

Censo das perguntas 2 e 3, somente código.

| Candidata | Predicado exato, universo e fonte | Quem chama |
|---|---|---|
| `competencia_entregue` (_K6) | Por `(empresa, mes, ano)`: seleciona o `ExportacaoDominio` mais recente, desempate `(gerado_em,pk)`, e exige `invalidada_em is None`; retorna bool ou o registro. É entrega por TXT, não pagamento externo. `app/folha/export.py:559,583-597`; `app/folha/models.py:203-225`. | `folha/views.py:354`; `folha/export.py:621,658`; `folha/services/ranking_he.py:89`; `ponto/services/fechamento.py:1550`; `pautas/management/commands/pauta_pago_x_devido_exportada.py:124`; `pauta_folga_nao_definida_exportada.py:93`; `ponto/services/esmeril_espelho.py:58`; `chamados/services/foto_e4b.py:245`; `vigia_fio.py:846,848`; `tarefa_do_dia.py:442`; `ponto/services/he_pendente_lavrado.py:223`; `dia_pago.py:717`; `chamados/management/commands/backfill_decisoes_pergunta.py:471,498`; `ponto/management/commands/lavrar_status_competencia.py:43`. Todos caminhos com prefixo `app/`. |
| `empresas_exportadas_no_escopo` | Projeção do mesmo K6: empresa indicada; senão empresas dos colaboradores indicados; senão empresas ativas. Retorna IDs cujo `marcos_da_competencia()['exportada']` é verdadeiro; `[]` de colaboradores é vazio. `app/ponto/services/fechamento.py:1509-1528`. | O próprio recalculador/carimbo L-113 `app/ponto/services/fechamento.py:539`; demais consumidores ficam com o outro explorador. |
| `marcos_da_competencia` | Por empresa+competência. `aprovada`: existe FM de colaborador da empresa com status aprovado/exportado. `exportada`: K6. `trancada`: K4. `paga`: existe `Holerite` na competência com **lote publicado**, atribuído pela `empresa_folha` ou, se nula, cadastro atual do colaborador. Um único holerite satisfaz o `exists()`, não exige todos pagos. `app/ponto/services/fechamento.py:1531-1557`. | Wrapper exportadas `:1528`, `marco_da_competencia :1568`; `app/folha/management/commands/pre_fechamento.py:74`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`. |
| `marco_da_competencia` | Retorna o marco verdadeiro mais adiantado na ordem aprovada/exportada/trancada/paga; caso contrário aberta. Não é outro predicado de pagamento. `app/ponto/services/fechamento.py:1474,1555-1557,1566-1568`. | Leitores de tela/placar: conferir censo do agente de consumidores. |
| `competencia_trancada` (_K4) / `periodo_trancado` (_K7) | Mesmíssimo filtro `PeriodoFechado(empresa_id,mes,ano,reaberto_em IS NULL)`. K4 retorna bool; K7 retorna linha/None. `ativo` não decide. Sem empresa, falso/None. `app/ponto/services/fechamento.py:1477-1493`. | K4: marcos `:1551`, `app/chamados/models.py:1091`. K7: `app/ponto/views.py:1030,1129,1543`; `app/chamados/juizes.py:1066`; `app/chamados/services/competencia_trancada.py:95`. |
| `ponto.janelas.competencia_fechada` (_F6, “mês fechado”) | Por colaborador+dia: timestamp convertido a data local; `janela_atual` com corte da empresa; procura FM daquela competência; aprovado/exportado devolve mensagem de recusa, demais devolvem `''`. Não lê TXT, tranca ou pagamento. `app/ponto/janelas.py:75-89`. | `app/ponto/registro_batida.py:135`; `app/ponto/services/flip_batida.py:22`; `app/escala/management/commands/lavrar_foto_folgas.py:95`. |
| `competencia_fechada_para_celula` (_K9) | K6 **OU** holerite do próprio colaborador na competência com lote publicado; sem colaborador só TXT conta. `fontes=True` informa exportação/holerite. `app/folha/export.py:600-631`. | `app/ponto/management/commands/gerar_celulas.py:182,212`; `app/ponto/management/commands/aplicar_col900.py:64`. |
| `_competencias_exportadas` / `fonte_da_lavra` / `autoridade_da_lavra` | Enumera pares de ED e holerites publicados, pergunta ao K9, converte à janela da empresa. As outras funções apresentam a fonte e a janela que intercepta o recorte. Não julgam pagamento. `app/ponto/management/commands/gerar_celulas.py:135-189,192-229`. | `app/ponto/portas/celula.py:221,607,613,821`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`; `app/escala/services/vigencia_impossivel.py:42`; `app/chamados/management/commands/auditar_invariantes_chamados.py:372,396,422,1549`; `app/ponto/management/commands/tripwire_celulas.py:319`. |
| `dia_em_competencia_trancada` | Por empresa+dia: enumera `PeriodoFechado` vivos e verifica se dia pertence à `janela_fechamento`; sem empresa/dia falso, tratamento de exceção fail-open. `app/chamados/juizes.py:2041-2059`. | `app/ponto/services/furos_sem_cobranca.py:85`. O cartório registra a retirada desse leitor em `app/ponto/services/cartorio.py:1713`. |
| `dia_da_pergunta_trancado` / `dias_trancados` (_C20) | Por colaborador+dia de turno: tranca viva da competência **e nenhum toque administrativo posterior nesse colab/dia**. Consulta `PeriodoFechado` via K7 e trilha `LogAuditoria` de cobrar/resolver/desarquivar; conversa não tem competência de fato. `app/chamados/juizes.py:974-992,1004-1008,1062-1093`. | C6 `app/chamados/juizes.py:1405`; lote `:1149`; `app/chamados/models.py:1902`; reconciliador `:668,748`; `services/tarefa_do_dia.py:422`; `services/disputa_emissao.py:1970`. |
| `ChamadoColaborador._dia_em_competencia_trancada` | Excetua módulos fora da tranca, obtém competência do chamado e pergunta K4. É escopo de renascimento de chamado. `app/chamados/models.py:1080-1091`. | `app/chamados/models.py:1056`; `app/ponto/services/cluster_espurio.py:254`. |
| `competencia_do_dia_entregue` | Composição data→`janela_atual`→K6; `None` sem dia, memo por empresa+competência. Mesma entrega TXT, com entrada por data. `app/chamados/services/vigia_fio.py:827-849`. | `app/chamados/models.py:364`; `reconciliador.py:745`; `services/fio.py:777,1317`; `foto_e4b.py:238`; `disputa_emissao.py:1837`; `vigia_fio.py:821`; `fabrica_da_celula.py:231`; `management/commands/backfill_fio_morto_e4b.py:173,255,303`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`. |
| `autorizacao_he_periodo._exportada` | Pergunta se empresa do colaborador pertence ao resultado de `empresas_exportadas_no_escopo`. Mesma entrega TXT. `app/ponto/services/autorizacao_he_periodo.py:113-118`. | `app/ponto/services/autorizacao_he_periodo.py:288`. |
| `tripwire_celulas._exportada` | Verifica data dentro das janelas retornadas por `_competencias_exportadas(empresa)`; sem colaborador, portanto só a metade TXT de K9. `app/ponto/management/commands/tripwire_celulas.py:318-322`. | Mesmo command `:346`. |
| `ExportacaoDominio.e_vigente` / `anotar_vigencia` / `so_vigentes` | “Qual registro é mais recente?” por empresa+competência. Não excluem `invalidada_em`. `app/folha/models.py:120-141,192-225`. | K6 `app/folha/export.py:591-594`; memória `app/folha/haiku_export.py:67-75`; listas/histórico e command de regeração são outros consumidores. |
| `exportacoes_da_competencia` / `passivo_validado._esta_exportada` | O primeiro devolve mais recente por `e_vigente`; o segundo testa `'vigente' is not None`. **Não são estritamente equivalentes a K6**: um último TXT invalidado sem substituto ainda aparece aqui, enquanto K6 responde falso. `app/folha/haiku_export.py:59-75`; `app/chamados/services/passivo_validado.py:116-128`; comparação K6 `app/folha/export.py:594-597`. | `_esta_exportada` chamado em `app/chamados/services/passivo_validado.py:358`. |

Registro vivo confirma F6 em `app/core/juizes.py:309` e K4/K6/K7/K9 em `app/core/juizes.py:326-347`; o próprio registro distingue “fechada por colaborador/status” de “trancada por empresa” em `app/core/juizes.py:76-85`.

Não encontrei modelo/campo específico para declarar “paga fora do sistema”; inexistência absoluta além da cópia pesquisada [nao verificado]. O modelo estruturalmente mais próximo por **empresa+competência com autor/data** é `PeriodoFechado`, que registra **tranca**, com reabertura, e não pagamento: `app/ponto/models.py:920-949`. Escrevem nele `trancar_periodo` e `reabrir_periodo`, em `app/ponto/services/fechamento.py:1413-1465`; a tela aprovar chama trancar quando há empresa e ninguém foi pulado, `app/ponto/views.py:1365-1370`; reabrir chama a porta e registra log em `app/ponto/views.py:1543-1548`. Não está registrado no admin de ponto mostrado em `app/ponto/admin.py:1-48`.

`ExportacaoDominio` guarda nome/hash/conteúdo do TXT, autor e geração, não declaração externa: `app/folha/models.py:75-86`; seu escritor `registrar` cria histórico de gerações em `app/folha/models.py:173-188`. `FechamentoMensal` guarda por colaborador status aberto/aprovado/exportado, aprovação e observação, sem status pago: `app/ponto/models.py:398-410,471-485`. `Holerite`/`LoteHolerite` permitem representar documento publicado, não pagamento manual declarado: `app/holerite/models.py:6-30,103-123`; quem carimba publicação do lote é `app/holerite/management/commands/processar_lote_holerite.py:123-130`.

A própria cópia documenta o desencontro entre pagamento externo e o marco “paga”: julho/agosto pagos à mão em `app/ponto/services/fechamento.py:500-504`, e julho com `marcos['paga']=False` apesar de paga fora em `app/ponto/management/commands/passivo_disputa_retro.py:37-38`. Para outubro há comentário “Comp <= 10 conserva [...] inclusive a 10 paga fora do sistema”, em `app/ponto/services/cartorio.py:282`; isso é corte temporal em código, não cadastro. Estado real dessas competências em produção [nao verificado].

## 15/11 09:45 · sessao cf072147f76f · - · - · origem=Ronald (tmux)

Q1 — leitores diretos encontrados:

| Leitor | Leitura e sentido de `(mes, ano)` | Empresa? |
|---|---|---|
| `recalcular_fechamento_mes` | `GUARDA_COMPETENCIA.get((mes, ano))`; usa a competência da folha, janela por corte da empresa; exclui do universo somente os colaboradores das empresas exportadas. `app/ponto/services/fechamento.py:539`, `:554`, `:560`, `:562`, `:564`; janela em `app/ponto/janelas.py:10`, `:54`. | Sim; consulta `empresas_exportadas_no_escopo`; a competência listada sozinha não barra empresa sem entrega. `app/ponto/services/fechamento.py:561`. |
| `dia_pago.retratar_exportada` | Testa presença da competência na guarda e entrega da empresa do colaborador; retorna `barrado_pela_guarda=True`. `app/ponto/services/dia_pago.py:457`, `:459`, `:467`. | Sim, via `colaborador_ids=[colab.pk]`. `app/ponto/services/dia_pago.py:460`. |
| `barrados_pela_guarda` | Lê `ACAO_BARRADO_GUARDA`; conta **linhas de LogAuditoria** cujo `descricao` começa por `MM/AAAA `. Não calcula janela, não soma colaboradores e não filtra empresa. `app/ponto/services/fechamento.py:47`, `:50`. | Não. `app/ponto/services/fechamento.py:50`. |
| command `recalcular_fechamento` | Importa guarda e contador; depois do apply imprime o acumulado da competência e `['saida']`. Mesmo com `--empresa`, passa só mês/ano ao contador. `app/ponto/management/commands/recalcular_fechamento.py:101`, `:118`, `:123`, `:126`. | O recalculo/foto podem ser por empresa; o contador impresso não é. `app/ponto/management/commands/recalcular_fechamento.py:60`, `:61`, `:126`. |
| `test_bob_b2_abono_feriado_guarda` | Usa empresa com corte 21; cria entrega 09 e fechamentos 09/10. Exige evento 09 preservado e contador 1; evento 10 regravado e contador 0; leitura 09 permitida; retrato 09 barrado. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:79`, `:169`, `:175`, `:192`, `:200`, `:206`, `:211`. | Sim no cenário montado; contador segue global. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:175`, `:198`. |
| `test_reverter_regen_recalcula_dinheiro` | Lê contador antes/depois da reversão de célula em 09 exportada; exige célula revertida, dinheiro preservado e contador aumentado. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:197`, `:203`, `:205`, `:211`, `:212`. | Cenário de um colaborador/empresa; contador não recebe empresa. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:205`. |
| `test_justificativa_c9_sem_banco` | Injeta módulo falso com `GUARDA_COMPETENCIA=set()` e ação `'guarda'` para testar retrato sem ata; não caracteriza calendário nem produção. `app/ponto/tests/test_justificativa_c9_sem_banco.py:138`, `:149`, `:153`. | Não exercita entrega real; fornecedor falso retorna conjunto vazio. `app/ponto/tests/test_justificativa_c9_sem_banco.py:151`. |

`ACAO_BARRADO_GUARDA` é usada para escrever o evento do recalculo barrado (`modelo='FechamentoMensal'`) e do retrato barrado (`modelo='DiaPago'`); seu leitor funcional é `barrados_pela_guarda`. `app/ponto/services/fechamento.py:44`, `:50`, `:569`; `app/ponto/services/dia_pago.py:463`.

O contador mede chamadas/eventos, não quantidade de pessoas: uma chamada barrando vários colaboradores gera uma linha, com quantidade no texto e IDs no JSON; o retrato gera outra linha por chamada/colaborador. `app/ponto/services/fechamento.py:569`, `:570`, `:573`; `app/ponto/services/dia_pago.py:463`.

As menções documentais ao nome estão em `app/docs/RELATO.md:1372`, `:1374`, `:1695`, `:2567`, `:4754`, `:4758`; não são leitores executáveis da variável. As ocorrências de `barrado_pela_guarda` em `eval/juiz.py:23`, `:60`, `:227` pertencem a outro veredito, descrito como motivo `numero_fora_dos_dados`. `eval/juiz.py:60`.

Q4 — 10/2026 e inventário dos escritores:

**A guarda desta cópia não impede regravação da 10:** só contém `(9, 2026)`; a aplicação exige simultaneamente chave guardada e empresa exportada. `app/ponto/services/fechamento.py:36`, `:37`, `:560`, `:561`. O teste existente exige explicitamente que evento na 10 regrave. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`.

`status='aprovado'` não impede o escritor de recalculo: aberto recebe `save()`; demais estados recebem `queryset.update()` dos números calculados, mantendo os carimbos de aprovação. `app/ponto/services/fechamento.py:1150`, `:1157`, `:1159`, `:1173`. A proteção do model é apenas no `save()` de linha originalmente aprovada que continua aprovada; não alcança o `update()` do recalculo. `app/ponto/models.py:492`, `:498`, `:503`; `app/ponto/services/fechamento.py:1173`. O contrato AST exige que `recalcular_fechamento_mes` não consulte tranca. `app/ponto/tests/test_contract_juiz_fechamento.py:117`, `:123`, `:130`.

| Escritor funcional | O que grava | Passa na guarda? |
|---|---|---|
| `recalcular_fechamento_mes` | Cria FM, salva/atualiza campos calculados; chama lavratura DP `motor`, e DP `oraculo` desde 10/2026. `app/ponto/services/fechamento.py:903`, `:1157`, `:1173`, `:1214`, `:1278`, `:1290`. | Sim, exclui colaboradores antes do laço. `app/ponto/services/fechamento.py:560`, `:564`. |
| `dia_pago.lavrar` | Apaga e recria todas as linhas do colaborador/competência/versão. `app/ponto/services/dia_pago.py:135`, `:359`, `:360`. | Sem guarda própria; as duas chamadas funcionais ficam no recalculo guardado. `app/ponto/services/fechamento.py:1214`, `:1290`. |
| `dia_pago.retratar_exportada` | Atualiza/cria três campos de grade, ajuste DSR/banco e carimbos de retrato. `app/ponto/services/dia_pago.py:486`, `:495`, `:518`, `:552`, `:556`, `:559`. | Sim, guarda própria antes de ler FM. `app/ponto/services/dia_pago.py:459`, `:469`. |
| `dia_pago.apurar_furo_da_exportada` | Somente `furo_apurado` e `lavrado_em` em linhas já existentes com furo NULL. `app/ponto/services/dia_pago.py:659`, `:664`, `:685`, `:687`. | Não consulta guarda; colhe motor em leitura e escreve por porta própria. `app/ponto/management/commands/apurar_furo_da_exportada.py:111`, `:131`. |
| `aprovar_lote` / `aprovar` | Status, usuário, data e eventualmente observação. `app/ponto/services/fechamento.py:1336`, `:1342`, `:1345`, `:1348`, `:1353`. | Não. `app/ponto/services/fechamento.py:1336`. |
| `reabrir_lote` / `reabrir` | Status aberto e limpeza da aprovação. `app/ponto/services/fechamento.py:1357`, `:1359`, `:1362`, `:1367`. | Não. `app/ponto/services/fechamento.py:1357`. |
| `restaurar_fechamento` | Atualiza campos fornecidos, exige motivo, invalida `derivado_em` por padrão. `app/ponto/services/fechamento.py:1371`, `:1391`, `:1404`, `:1406`. | Não. `app/ponto/services/fechamento.py:1386`. |
| `reabrir_periodo` | Reabre PeriodoFechado e devolve FM aprovado para aberto. `app/ponto/services/fechamento.py:1445`, `:1450`, `:1462`, `:1465`. | Não. `app/ponto/services/fechamento.py:1445`. |
| `invalidar_previsto` | Zera `previsto_em` e atualiza carimbo. `app/ponto/services/fechamento.py:1620`, `:1624`, `:1626`. | Não. `app/ponto/services/fechamento.py:1620`. |
| `invalidar_fechamento` | Zera `derivado_em` e atualiza carimbo nas competências do fato. `app/ponto/services/fechamento.py:1662`, `:1665`, `:1669`. | Não. `app/ponto/services/fechamento.py:1629`. |
| `apagar_em_ensaio` | Delete de FM, exigindo banco de ensaio. `app/ponto/services/fechamento.py:1777`, `:1779`, `:1781`. | Não; tem guarda de banco de ensaio. `app/ponto/services/fechamento.py:1780`. |
| `FechamentoMensal.save` | Escrita ORM, com proteção aprovado→aprovado. `app/ponto/models.py:492`, `:498`, `:503`. | Não consulta guarda de competência. `app/ponto/models.py:492`. |
| `bin/restore_relavratura_10_2026.py` | Salva FM por `save(update_fields)`, apaga FM excedentes, apaga/recria DP motor. `bin/restore_relavratura_10_2026.py:74`, `:81`, `:83`, `:85`, `:86`. | Não passa pelo serviço guardado. `bin/restore_relavratura_10_2026.py:69`. |
| `bin/restore_o263_comp10.py` | Mesmos atos, limitados ao alvo. `bin/restore_o263_comp10.py:43`, `:50`, `:52`, `:54`, `:56`. | Não passa pelo serviço guardado. `bin/restore_o263_comp10.py:40`. |
| `bin/restore_badge_linha_09_10.py` | Apaga DP das competências fotografadas e recria snapshot. `bin/restore_badge_linha_09_10.py:60`, `:61`, `:63`. | Não passa pelo serviço guardado. `bin/restore_badge_linha_09_10.py:59`. |

Chamadores de escrita que chegam ao recalculo, portanto à guarda: botão `app/ponto/views.py:1255`; evento `app/ponto/services/fechamento.py:1823`; command normal `app/ponto/management/commands/recalcular_fechamento.py:118`; REGEN `app/ponto/portas/regen_exportada.py:184`; `app/folha/management/commands/simular_folha.py:123`; commands `aplicar_09_corte_b.py:212`, `plano_b_no_dinheiro.py:110`, `folga_que_sumiu.py:70`, `desvio_o68b.py:94`, `celula_veredito_velho.py:149`, `diff_reclassificar_partido.py:161` e `:206`, todos sob `app/ponto/management/commands/`. Também chegam os scripts `bin/aplicar_o263_comp10.py:51`, `bin/relavrar_o118_col221.py:53`, `bin/relavrar_o119_col221.py:52`, `bin/relavrar_o120_col221_09.py:74`.

`lavrar_dias_pagos` roteia empresa não exportada para `somente_leitura=True, lavrar_dias_pagos=True` e exportada para `retratar_exportada`; ambos caminhos passam por guarda de escrita. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `:106`, `:123`. O modo leitura puro não consulta exportadas para bloquear e não persiste FM/DP. `app/ponto/services/fechamento.py:539`, `:540`, `:1141`, `:1204`.

**REGEN não contorna a guarda 09 nesta cópia:** chama o recalculo e depois o retrato, e as duas funções guardam 09 exportada; não há parâmetro de bypass nessas chamadas. `app/ponto/portas/regen_exportada.py:184`, `:193`; `app/ponto/services/fechamento.py:560`; `app/ponto/services/dia_pago.py:459`.

`restaurar_fechamento` é chamado diretamente pelos commands de reversão/aplicação sem passar pela guarda do recalculo: `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `:266`; `app/ponto/management/commands/aplicar_09_he_intra.py:149`; `app/ponto/management/commands/desvio_o68b.py:197`; `app/ponto/management/commands/diff_reclassificar_partido.py:224`.

A data da competência não é mês civil: com corte 21, 25/09 pertence a 10; há comentário explícito no restore que conserva `mes/ano` da linha justamente por esse motivo. `app/ponto/janelas.py:11`, `:13`; `bin/restore_badge_linha_09_10.py:44`. O corte real cadastrado de Juliani em produção permanece [nao verificado].

## 15/11 09:45 · sessao c053ba9ea9dc · - · - · origem=Ronald (tmux)

Censo da pergunta 5, somente código; sem medição de produção.

**A distinção que os consumidores fazem hoje:** `empresas_exportadas_no_escopo` devolve empresas cujo marco `exportada` é verdadeiro; este marco chama `competencia_entregue`. O predicado de entrega é existir TXT vigente da tripla empresa/mês/ano, com vigente não invalidado. O marco `paga` consulta holerite em lote publicado; são fontes e perguntas diferentes. `app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1550`, `app/ponto/services/fechamento.py:1552`, `app/folha/export.py:591`.

**Consumidores diretos de `empresas_exportadas_no_escopo`:**

| Consumidor | Uso da resposta |
|---|---|
| Recalculo | Consulta apenas quando pode escrever `FechamentoMensal` ou `DiaPago`; carimba L-113 quando exportada; combina com `GUARDA_COMPETENCIA` para excluir colaboradores das empresas exportadas. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`, `app/ponto/services/fechamento.py:560` |
| `lavrar_dias_pagos` | Não exportada: motor em leitura com lavratura; exportada: `retratar_exportada` usando gravado. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `app/ponto/management/commands/lavrar_dias_pagos.py:106`, `app/ponto/management/commands/lavrar_dias_pagos.py:123` |
| `lavrar_do_motor` | Combina presença da competência na guarda com empresa exportada para recusar lavratura. `app/ponto/services/dia_pago.py:458` |
| Porta do export | Separa divergências de dinheiro congelado, grade e linha versus badge; exclui exportada do contador de lavratura sem furo apurado. Na falha da consulta assume aberta. `app/folha/porta_export.py:343`, `app/folha/porta_export.py:346`, `app/folha/porta_export.py:424`, `app/folha/porta_export.py:430`, `app/folha/porta_export.py:452`, `app/folha/porta_export.py:461` |
| Cobrança da folha | `muda_a_folha`: dia sem data retorna falso; competência pelo corte da empresa; exportada retorna falso e conta motivo. A tela de cobrar filtra com essa resposta. `app/chamados/services/cobranca_folha.py:54`, `app/chamados/services/cobranca_folha.py:63`, `app/chamados/services/cobranca_folha.py:68`, `app/chamados/views_cobrar.py:95` |
| Autorização HE por período | `_exportada` pergunta pela empresa; ato com autorização efetiva e exportada produz Pauta DP. `app/ponto/services/autorizacao_he_periodo.py:113`, `app/ponto/services/autorizacao_he_periodo.py:288`, `app/ponto/services/autorizacao_he_periodo.py:299` |

**Telas e Pauta DP que leem entregue/exportada por outros caminhos:**

- Preview mostra “Já exportado em … por …” usando o registro devolvido por `competencia_entregue(..., registro=True)`. A emissão do TXT também mostra aviso de reexportação pelo `ja_existia` retornado por `ExportacaoDominio.registrar`; é outro caminho, histórico de geração. `app/folha/views.py:354`, `app/folha/views.py:361`, `app/folha/views.py:297`, `app/folha/views.py:306`.
- Ranking HE: sem linhas=`nao_apurado`; com linhas e todas as empresas entregues=`lavrado`; demais=`parcial`. `app/folha/services/ranking_he.py:85`.
- Pauta pago versus devido aceita somente empresa cuja competência o K6 considera entregue; “pago” vem do `FechamentoMensal` gravado e “devido” do recalculo em leitura; escreve cabeçalho e filhas com os dois valores. Competência sem TXT vigente é classificada `nao_entregue`. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`.
- Pauta folga não definida exportada percorre empresas ativas, exige entregue e usa janela da empresa; a não entregue permanece atribuída à lista do admin quando coberta por `competencias_a_lavrar`. `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:92`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:96`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:104`.
- Limbo (`trabalha=None`) com motivos em competência entregue vai para Pauta DP em lugar de perguntar ao colaborador; dia de trabalho permanece em outro ramo. `app/chamados/services/fabrica_da_celula.py:214`, `app/chamados/services/fabrica_da_celula.py:223`, `app/chamados/services/fabrica_da_celula.py:268`.
- Órfãs enterradas: competência exportada entra no balde Pauta DP e sai antes da reabertura. `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:99`.
- Pré-fechamento relata marco e violações da competência anterior; não cria trava própria por exportada. `app/folha/management/commands/pre_fechamento.py:69`.

**REGEN-EM-EXPORTADA e carimbo L-113:**

- A porta exige colaborador individual, staff, motivo de dez caracteres e intervalo válido; regenera células com `apesar_da_lavra=motivo`; depois chama o recalculo comum e `retratar_exportada`. `app/ponto/portas/regen_exportada.py:109`, `app/ponto/portas/regen_exportada.py:111`, `app/ponto/portas/regen_exportada.py:114`, `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`, `app/ponto/portas/regen_exportada.py:193`.
- Essa chamada de recalculo **não tem bypass da guarda de fechamento**: passa apenas competência, colaborador e motivo; portanto encontra o mesmo filtro de `GUARDA_COMPETENCIA`. O comentário da porta dizendo “não há mais guarda para destravar” diverge desse código atual. `app/ponto/portas/regen_exportada.py:181`, `app/ponto/portas/regen_exportada.py:184`, `app/ponto/services/fechamento.py:560`.
- A própria porta registra hashes, campos, réguas antes/depois e retorna o resultado; o escritor de Pauta é o chamador `passivo_validado`, que chama a porta quando o par é exportado e teve plantio, e escreve Pauta quando mudou ou Pauta de saída do DIFF quando não mudou. `app/ponto/portas/regen_exportada.py:204`, `app/ponto/portas/regen_exportada.py:211`, `app/ponto/portas/regen_exportada.py:221`, `app/chamados/services/passivo_validado.py:435`, `app/chamados/services/passivo_validado.py:444`, `app/chamados/services/passivo_validado.py:448`, `app/chamados/services/passivo_validado.py:456`.
- O carimbo L-113 usa `_exportadas`, não o marco `paga`, e escreve aviso antes do filtro da guarda; uma chamada barrada pode ter também o aviso de competência exportada. Leitura pura não carimba; leitura com lavratura carimba. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`, `app/ponto/services/fechamento.py:560`.

**Demais consumidores de entrega:**

- `dias_com_abono` conserva a contagem das linhas positivas lavradas quando entregue; fora disso consulta ata e períodos justificados. `app/ponto/services/dia_pago.py:715`, `app/ponto/services/dia_pago.py:717`, `app/ponto/services/dia_pago.py:720`.
- `competencias_a_lavrar` mantém atual e anterior enquanto anterior não entregue. A janela da lista do Esmeril recua até primeira competência entregue ou até acabar o histórico de células. `app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/ponto/services/esmeril_espelho.py:55`.
- `competencia_do_dia_entregue` compõe janela da empresa com K6; leitores: classificação de carimbos por dono, reconciliador, modelos, fio, emissão de disputa, fábrica da célula e backfill E4b. `app/chamados/services/vigia_fio.py:827`, `app/chamados/services/vigia_fio.py:821`, `app/chamados/reconciliador.py:745`, `app/chamados/models.py:364`, `app/chamados/services/fio.py:777`, `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/fabrica_da_celula.py:231`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:173`.
- `veto_de_competencia` veta acervo da E1 quando algum dia é entregue; é usado pelo backfill de decisões e migração de data do turno. `app/chamados/services/fio.py:1307`.
- Tarefa do dia carrega entrega em lote, remove essas candidatas da consulta C6 e transmite o atributo `exportada` para a tarefa. `app/chamados/services/tarefa_do_dia.py:430`, `app/chamados/services/tarefa_do_dia.py:483`, `app/chamados/services/tarefa_do_dia.py:526`.
- Rederivação de minutos H06 recusa dia em competência entregue; lavratura de status apenas informa entrega, sem usá-la como guarda. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`, `app/ponto/management/commands/lavrar_status_competencia.py:43`.
- Vigia do TXT e regerador comparam hash do TXT vigente com o gravado atual por `txt_vigente_x_gravado`; o vigia distingue vigente revogado sem substituta como incomparável e alarma movimentação de valores. `app/folha/export.py:658`, `app/folha/services/vigia_txt.py:220`, `app/folha/services/vigia_txt.py:244`, `app/folha/services/vigia_txt.py:265`, `app/folha/management/commands/regerar_txt_dominio.py:106`.

**Guarda de células é outra pergunta:** `competencia_fechada_para_celula` compõe entrega por empresa com holerite publicado do próprio colaborador; `_competencias_exportadas` enumera candidatos e consulta esse juiz. Regenerar, editar e reverter célula usam esse conjunto, com exceção textual `apesar_da_lavra`. Seus consumidores indiretos incluem corrigir escala retroativa, vigência impossível, tripwire e auditoria de invariantes. `app/folha/export.py:600`, `app/folha/export.py:621`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:820`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`, `app/escala/services/vigencia_impossivel.py:42`, `app/ponto/management/commands/tripwire_celulas.py:319`, `app/chamados/management/commands/auditar_invariantes_chamados.py:372`.

**Testes que explicitam contratos desses consumidores:** K6 entrega e integração em marcos/preview/hash/carimbo/gerador; fechamento de célula por TXT ou holerite; contrato de competência lavrada; Pauta pago/devido e folga exportada; autorização HE; cobrança folha; janela da lista; REGEN e passivo. `app/folha/tests/test_k6_competencia_entregue.py:61`, `app/folha/tests/test_k6_competencia_entregue.py:96`, `app/folha/tests/test_k6_competencia_entregue.py:112`, `app/folha/tests/test_k6_competencia_entregue.py:147`, `app/folha/tests/test_k6_competencia_entregue.py:165`, `app/folha/tests/test_k6_competencia_entregue.py:232`, `app/folha/tests/test_k6_fechada_para_celula.py:103`, `app/ponto/tests/test_contract_competencia_lavrada.py:63`, `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:407`, `app/pautas/tests/test_f3_beco1_pauta_exportada.py:43`, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:405`, `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:157`, `app/ponto/tests/test_janela_da_lista_le_o_k6.py:46`, `app/ponto/tests/test_regen_em_exportada.py:54`.

**Limite factual da mudança hipotética:** trocar somente a pergunta da guarda não troca automaticamente os consumidores de entrega listados; hoje eles chamam seus respectivos predicados diretamente. Se a alteração pretendida também modificar o significado de `empresas_exportadas_no_escopo` ou do K6, os consumidores acima deixam de receber a mesma pergunta histórica. O desenho concreto dessa alteração ainda é [nao verificado]. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/folha/views.py:354`.

## 15/11 09:41 · sessao c3740e289d15 · - · - · origem=Ronald (tmux)

As quatro linhas registradas de `chamados/juizes.py` coincidem com a cópia; nenhum deslocamento observado:

| Sítio | Registro | Fonte atual |
|---|---|---|
| `_dia_da_batida` | `core/tests/test_medida_turno_do_par.py:258` → `[53]` | `chamados/juizes.py:53` |
| `_dia_do_contexto_fraco` | `core/tests/test_medida_turno_do_par.py:259` → `[169]` | `chamados/juizes.py:169` |
| `_plantio_na_hora_do_slot` | `core/tests/test_medida_turno_do_par.py:260` → `[274]` | `chamados/juizes.py:274` |
| `dias_do_fato` | `core/tests/test_medida_turno_do_par.py:261` → `[203]` | `chamados/juizes.py:203` |

Para `padrao=`, o termo literal é `single source of truth / um juiz por pergunta` (`docs/PLANO-PADROES.md:46–50`).

Censo textual de leitores de produção, conferido por leitura:

- `_portas_da_tarefa`: `chamados/services/tarefa_do_dia.py:591`, `:616`.
- `porta_da_tarefa`: `escala/services/cadastro_realidade.py:472`; `chamados/services/vigia_fio.py:354`, `:589`; `chamados/services/furo_sem_canal.py:97`; `folha/fora_da_folha.py:72`, `:261`; alias `candidata` em `chamados/management/commands/diff_e3_sombra.py:43`, usado em `:146`.
- `porta_alcancavel_do_dia`: único chamador de produção restante é o lado “antes” de `chamados/management/commands/diff_e3_sombra.py:144`.

Censo real e suite: [nao verificado]. Nenhum arquivo editado. [nao verificado]

## 15/11 08:04 · sessao 74371de003cb · DISPUTA-ZUMBI-1110 · censo · origem=Code

**O HEAD fornecido mantém carimbadores sem consulta geral de fechamento após a escrita. Porém, `lavrar_sem_turno_aberto` consulta C7 e chama `fechar()` depois do carimbo; a descrição “sem leitor depois” não corresponde a esse fluxo atual.** (`app/chamados/reconciliador.py:406-414`; `app/chamados/services/fio.py:1398-1402`; `app/chamados/services/disputa_emissao.py:1681-1689`.)

Os números e as causas individuais das disputas dos anexos não foram reconferidos em produção; faltam linhas individualizadas, instantes dos carimbos e resultados das tentativas de fechamento. **[não verificado]**

Para compactar as referências: caminhos sem prefixo abaixo são relativos a **`app/chamados/`**; caminhos iniciados por `app/` ou `bin/` são relativos à raiz do archive. As referências são desta cópia; correspondência com o hash Git informado e histórico contra `eac3f25dd`: **[não verificado]**.

**1. Carimbadores e fechamento no mesmo fluxo**

Na tabela, **B** significa `PerguntaDisputa.save → post_save → on_commit → fechar_beco_da_disputa`. Esse caminho somente consulta C7 se a disputa estiver aberta, o pai for terminal e não houver pergunta sem `respondida_em` cujo destino não seja terminal; também há proteção contra reentrada. Portanto, **B não é uma consulta universal de C7 após qualquer carimbo**. (`signals.py:229-240`; `services/becos.py:25-33,66-80`.)

| Carimbador / sítio de escrita | Campos ou ato | Consulta C7 / chama `fechar()`? Condição adicional |
|---|---|---|
| `carimbar` | Materialização, autor, via; opcionalmente resposta/validação | A porta consulta **C6**, desfaz em memória se a pergunta continuar viva e salva somente se aceita. Não consulta C7 diretamente; o fechamento depende do chamador e de B. (`models.py:3186-3239`.) |
| `carimbar_identidade` | Materialização, autor, via; motivo opcional | Grava identidade sem consulta C6/C7 própria; fechamento depende do chamador e de B. Recusa devolver linha fora do fato por outra via. (`models.py:3251-3274`.) |
| `lavrar_sem_turno_aberto` | Via `sem_turno_aberto` | **Sim:** carimbo aceito → disputa aberta + C7 → `fechar`. Também B. (`reconciliador.py:406-414`.) |
| `retratar_perguntas_absolvidas_por_celula` | Via `celula_absolveu` | **Sim, após o lote:** relê fechamento, exige aberta + C7; captura falhas do fechamento. Também B. (`reconciliador.py:593-617`.) |
| `encerrar_orfas_pela_lampada` → `encerrar_fantasma` | Via `fantasma` | **Sim, após o lote:** relê disputa, exige aberta, nenhuma resposta sem veredito e C7. Também B. (`reconciliador.py:1148-1177`; `models.py:3378-3386`.) |
| `_materializar_ausencia_de_resposta` | Carimbo das irmãs; pode completar resposta/validação | **Sim:** aberta + C7 → `fechar`. Ausência ainda aberta não encerra o destino. Também B. (`services/materializacao.py:555-592`.) |
| `_materializar_uma` | Materialização/via; pode completar resposta/validação | **Sim pela cauda do lote:** `materializar_perguntas_validadas_da_disputa → _fechar_disputa_se_quitada`, aberta + C7. Também B. (`services/materializacao.py:1283-1284,1322-1339,1381-1385`.) |
| Recusa por paridade, dentro da materialização | Identidade `paridade_recusada` | Mesma cauda C7 do materializador; também B. (`services/materializacao.py:930,1381-1385`.) |
| `_carimbar_recusa`, colisão | Identidade `fato_ocupado` | Mesma cauda C7 do materializador; exige ausência de carimbo e recusa `COLISAO`. Também B. (`services/materializacao.py:1099-1106,1303,1381-1385`.) |
| `materializar_pendentes_aprovadas` | Via `juiz_aprovou` | Sem C7 próprio: o signal o chama quando a disputa **já está fechada**. Exige lastro aprovado e pula pergunta em voo. Também B. (`signals.py:155-167`; `services/materializacao.py:1431-1438`.) |
| S143: perguntas ligadas ao chamado resolvido | Via pelo fato; resposta/validação quando cabíveis | **Sim na cauda:** relê disputa sob nova consulta de tranca; aberta + C7. **Pula a disputa se alguma pergunta estiver em voo.** Também B. (`services/disputa_emissao.py:1973-1994,2088-2099`.) |
| S143: perguntas sem credor vivo | Via pelo fato; pode completar `respondida_em` | Mesma cauda C7; exige pai/destinos fora de `VIVOS`, pergunta fora de voo e C6 morta. Também B. (`services/disputa_emissao.py:2039-2067,2088-2099`.) |
| `responder_pergunta_disputa_core` | Texto + instante; limpa validação anterior; pode desfazer carimbo sem lastro | Sem C7 geral próprio. Escape que vira ausência chama o materializador com C7; demais respostas têm B. (`services/disputa_emissao.py:1467-1485,1503-1512,1538-1541`.) |
| `responder_automaticamente` | Texto + instante | Sem C7 próprio; o chamador da foto valida pelo dono e pode fechar posteriormente. Também B. (`services/disputa_emissao.py:1659-1664`; `app/api/foto_recebida.py:48-51,71-80`.) |
| `carimbar_resposta_pela_trilha` | `respondida_em`, usando instante da nota | **Somente B.** O comando `curar_perguntas_em_limbo --apply` não acrescenta consulta C7 depois. (`services/disputa_emissao.py:1677-1689`; `management/commands/curar_perguntas_em_limbo.py:64-70`.) |
| `_carimbar_validacao` no fluxo normal de `validar_pergunta` | Validação + autor | **Sim pela materialização integral**, depois do tratamento da recusa; também B. (`services/validacao.py:159-166,460,488-494`; `services/materializacao.py:1322-1339`.) |
| Aceite de folga | Validação e identidade `admin_decidiu` | Sem C7 próprio nesse ramo; fecha o fio do dia, que pode reconciliar chamados e acionar S143. Também B. (`services/validacao.py:313-321,345-354`; `app/ponto/services/veredito_celula.py:190-203`.) |
| `devolver_validacao_recusada` | Limpa validação + autor | Sem C7 próprio; pode **reabrir** disputa fechada quando `reabrir=True`. Também B. (`services/validacao.py:224-243`.) |
| `recusar_pergunta` | Devolve questionário para correção | Delega à reabertura em lote; sem fechamento C7 acrescentado nessa porta. (`services/validacao.py:588,628-640`; `services/disputa_emissao.py:1634-1636`.) |
| `relavrar` | Troca via derivada e atualiza cache da lavra | **Somente B.** O comando `relavrar_via_i6 --apply` também não acrescenta C7. (`services/relavra_via.py:118-139`; `management/commands/relavrar_via_i6.py:96-104`.) |
| `retratar_para_canonica → retratar_duplicata` | Identidade `duplicata`; preserva instante existente | **Somente B.** A porta de retratação exige as travas e aplica os vetos antes do carimbo. (`services/fio.py:1373-1402`.) |
| `remover_motivo` | Retratação na sequência + identidade `motivo_retratado` | **Com motivos restantes:** retorna, apenas B. **Sem motivos restantes:** chama `fechar()` diretamente, sem consulta prévia literal a C7. (`models.py:2120-2136`.) |
| `fechar_por_admin` | Identidade `admin_decidiu` | Sem C7 próprio. É usado dentro das exceções de `Disputa.fechar`, no aceite de folga e no fechamento do fio do dia. Também B. (`models.py:1982-1985,3147`; `services/validacao.py:314,354`; `app/ponto/services/veredito_celula.py:190`.) |
| `descarimbar` | Limpa materialização, autor e via | Sem C7 próprio; B somente quando `salvar=True`. (`models.py:3286-3296`.) |
| `descarimbar_em_lote` | `QuerySet.update`: limpa carimbo/via e campos extras | **Sem C7 e sem post_save/B.** Exige colaboradores travados e exclui linhas fora do fato. (`models.py:3335-3365`.) |
| `reabrir_para_correcao` | Limpa texto, resposta, validação, autores, materialização e via | **Sem C7/B:** usa a porta do lote e emite eventos de célula explicitamente. (`services/disputa_emissao.py:1595,1634-1636`.) |
| `ressuscitar_perguntas` | Lote: limpa carimbo, resposta e validação | **Sem C7/B:** emite eventos e reabre disputas fechadas afetadas. (`models.py:1613-1622`.) |
| `desfazer_carimbo_sem_lastro` | Descarimba pergunta que C6 considera viva | Sem C7 próprio; B. Pode reabrir disputa fechada, conforme `reabrir_fechada`. (`services/relavra_via.py:238-261`.) |
| `encerrar_por_fato_no_espelho` | Troca somente via para `fato_materializado` | A porta tem B. Seu comando chamador, com `--apply`, depois exige disputa aberta e `fio_vivo=False`, chama `fechar()` e captura `ValueError`; não consulta C7 literalmente antes. (`models.py:3367-3376`; `management/commands/encerrar_perguntas_com_fato.py:53-55,68,81-93`.) |
| `reabrir_fantasma` | Descarimba somente a via fantasma materializada | Sem C7 próprio; B. O comando pode reabrir disputa e pai. (`models.py:3395-3398`; `management/commands/reabrir_orfas_enterradas.py:157-172`.) |
| `reabrir_por_resposta` | Descarimba em memória as vias sem lastro | Sem C7 próprio; o escritor da resposta salva depois, acionando B. (`models.py:3412-3414`; `services/disputa_emissao.py:1485,1508`.) |
| `reabrir_por_competencia` | Descarimba a antiga via `competencia_trancada` | Sem C7 próprio; B. (`models.py:3419-3422`; `services/competencia_trancada.py:279`.) |
| `_nascer` | Insere pergunta com campos recebidos | Sem C7 próprio; nascimento também passa pelo signal B. Preserva os atos projetados na sequência de decisões. (`models.py:3097-3102`; `signals.py:229-240`.) |

O escritor básico do trio materialização/autor/via é `_escrever_carimbo`; `encerrar_por_fato_no_espelho` e `relavrar` também têm escritas específicas de **via**, e o lote tem a escrita SQL própria já indicada. (`models.py:3180-3184,3363-3375`; `services/relavra_via.py:125-128`.)

`gravar_lavras_em_lote` **continua sem C7/post_save**, mas grava somente `veredito_lavrado`, `julgado_em` e `janela_expira_em`. Não resolve diretamente pelos campos enumerados na pergunta; C6 lê a projeção dos carimbos e consulta o lastro, não esse cache. Essa classificação da triagem precisa ser corrigida. (`veredito_lavrado.py:237-250`; `juizes.py:1399-1400,1476-1485`.)

Além dos carimbos, existem alterações de âncora/dia/célula que afetam os insumos de C6: reuso da pergunta, reapontamento da resolvedora e rechaveamento de data. Esses sítios não acrescentam C7 próprio; seus saves passam por B. (`models.py:3086-3090`; `services/fio.py:959-973,1076-1084`; `models.py:3303-3305`.)

O evento de célula é outro mecanismo: saves dos campos relevantes emitem outbox; a escrita por lote precisa emitir explicitamente. **Emitir evento não equivale a consultar C7 imediatamente nesse ato.** (`signals.py:243-255`; `services/disputa_emissao.py:1609-1611,1634-1635`; `models.py:3314-3331`.)

**2. Escritores do fechamento e reabridores**

Há **um escritor positivo de `DisputaSupervisao.fechada_em` encontrado**: `DisputaSupervisao.fechar`, que grava instante, autor, resolução e motivo juntos. Não há coluna `status/estado` nessa entidade; `aberta` deriva de `fechada_em is None`. (`models.py:1804,1869-1871,1996-2000`.)

Os sítios diretos de chamada da porta são:

| Chamador | Condição anterior à porta |
|---|---|
| `admin_encerra` | Motivo escrito obrigatório; usa exceção nomeada. (`models.py:2011-2014`.) |
| `remover_motivo` | Nenhum motivo restante. (`models.py:2130-2136`.) |
| Desligamento | Disputa aberta e nenhuma resposta sem veredito; exceção `desligamento`. O corpo atual não faz o antigo bulk de fechamento. (`app/colaboradores/services/desligamento.py:157-167`.) |
| Comando de fato no espelho | `--apply`, aberta, sem fio vivo; captura `ValueError`. (`management/commands/encerrar_perguntas_com_fato.py:53-55,81-93`.) |
| Reconciliador, sem turno | Carimbo aceito, aberta e C7. (`reconciliador.py:406-414`.) |
| Reconciliador, absolvição pela célula | Após lote, refresh, aberta e C7. (`reconciliador.py:609-617`.) |
| Reconciliador, fantasma pela lâmpada | Refresh, aberta, sem resposta sem veredito e C7. (`reconciliador.py:1159-1177`.) |
| Foto recebida | Aberta, batida coincide, nenhum motivo restante; valida antes. (`app/api/foto_recebida.py:61-80`.) |
| Beco | Resolve destinos terminais, refresh, aberta e C7; entrada por evento exige o recorte de beco. (`services/becos.py:25-33,42-57,74-78`.) |
| Competência trancada | Aplicação habilitada; medição exclui pai vivo fora do conjunto encerrável; consulta C7 novamente no ato. (`services/competencia_trancada.py:178-183,192,221-230`.) |
| Emissão/atualização da disputa | Após tentar perguntar, C7; **não fecha se houve abstenção por ata ausente**. (`services/disputa_emissao.py:1270-1287`.) |
| S143 | Refresh, nenhuma pergunta em voo, aberta e C7. (`services/disputa_emissao.py:2088-2099`.) |
| Materialização de ausência | Aberta e C7. (`services/materializacao.py:587-592`.) |
| Cauda da materialização granular | Aberta e C7. (`services/materializacao.py:1322-1339`.) |
| Entrada sincronizada | Pai manual vivo que agrupa somente aquele órfão; disputa aberta e C7. (`app/ponto/services/entrada_sincronizada.py:48-62`.) |

As reaberturas efetivas da disputa são:

| Porta | Escrita e restrições | Chamadores encontrados |
|---|---|---|
| `reabrir_admin` | Limpa fechamento/autor/resolução/motivo; exige fechada, usuário e menos de duas reaberturas administrativas. (`models.py:2202-2225`.) | Ação administrativa que busca a fechada mais recente. (`services/acoes_disputa.py:247-259`.) |
| `reabrir_sistema` | Limpa os mesmos campos; idempotente se já aberta; sem teto administrativo; ocorrência no pai. (`models.py:2231-2244`.) | Ressuscitação (`models.py:1620-1622`); desfazer carimbo sem lastro (`services/relavra_via.py:260-261`); devolução de validação, condicionada à flag (`services/validacao.py:242-243`); ausência rejeitada (`services/materializacao.py:681-696`); comando de órfãs enterradas (`management/commands/reabrir_orfas_enterradas.py:165-172`). |

`reabrir_por_colab` reabre **o questionário de disputa que permanece aberta**; não é terceiro escritor da reabertura da disputa. (`models.py:2257-2266,2288-2304`.)

**3. Cron/vigia e contador geral**

O agendamento comprovado é **`fechar_becos_disputa --alarme`**, dependente de `reconciliar_perguntas_orfas`. A configuração não pede aplicação de fechamento. (`app/config/crons.py:663`.)

O comando percorre abertas, classifica pelo recorte de beco, conta fecháveis/barradas e, em `--alarme`, termina com código 2 se houver candidatas. Só aplica com `--apply` e sem `--alarme`. (`management/commands/fechar_becos_disputa.py:30-70`.)

Não localizei um vigia agendado cujo universo seja exatamente **todas as disputas abertas com C7 verdadeiro**; a existência por invocação dinâmica ou configuração externa permanece **[não verificado]**. Os mecanismos próximos têm universos diferentes:

- Relatório sob demanda: usa C7, mas separa `SEM_PERGUNTA` **antes** da consulta; `PRONTA_FECHAR` não inclui as vazias. (`relatorios_disputa.py:12-25,45-58`; `app/config/crons.py:1463`.)
- Auditor C2: conta **perguntas** por carimbos e veredito da célula; não consulta C7 e nem filtra disputa aberta, apesar do rótulo. (`management/commands/auditar_invariantes_chamados.py:1601-1605`.)
- `bin/censo_folha.py` chama uma variável de `pviva`, mas conta carimbo de resposta nulo em disputa aberta; não é C6/C7. (`bin/censo_folha.py:28-29`.)

**4. Leitores de C7 e homônimos**

As chamadas diretas encontradas ao juiz são: relatório (`relatorios_disputa.py:24`); reconciliador (`reconciliador.py:412,612,1171`); materialização (`services/materializacao.py:591,1322`); beco (`services/becos.py:33,56`); competência (`services/competencia_trancada.py:178,227`); explicador (`services/explicador.py:139`); emissão/S143 (`services/disputa_emissao.py:1278,2098`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`). O registro em `core/juizes.py` é endereço declarativo, não chamada. (`app/core/juizes.py:287`.)

**A divergência do explicador apontada na triagem foi removida:** ele consulta C7 e publica esse resultado na chave `pode_fechar`. (`services/explicador.py:134-165`.)

Os consumidores dessa chave são:

| Consumidor | Significado empregado |
|---|---|
| `desenlace` | Traduz verdadeiro como **“Tudo validado e gravado”**. Isso afirma mais que C7: o predicado também aceita universo vazio e não exige que toda pergunta tenha carimbo de validação/materialização. (`services/desenlace.py:63-65`; `models.py:1905-1917`; `juizes.py:1403-1406`.) |
| Modal | Usa a chave para cor e aviso “Nada pendente… pode fechar/regularizar”. (`app/templates/chamados/partials/modal_fio.html:435,446`.) |

Outro significado próximo, porém sem a chave homônima: `disputa_viva_do_chamado` considera **sem perguntas OU fio vivo**; uma disputa vazia pode, portanto, travar esse leitor enquanto C7 aceita o universo vazio. (`juizes.py:2437-2444`; `models.py:1916-1917`.)

**5. Quando `fechar()` recusa ou falha apesar de C7 verdadeiro**

Para **o mesmo universo e estado**, não encontrei diferença no predicado normal de negócio: C7 exige nenhuma resposta sem veredito e nenhum fio vivo; `fechar()` consulta a mesma lista e `pergunta_viva`. Ele não chama `pode_fechar()` literalmente. (`models.py:1915-1917,1950-1967`; `juizes.py:1504-1513`.)

| Situação adicional | Resultado |
|---|---|
| Motivo explícito fora das portas permitidas | `ValueError`. (`models.py:1940-1941`.) |
| Disputa já fechada | Mesma entrada retorna `False`; entrada diferente levanta `ValueError`. C7 não testa se já está fechada. (`models.py:1915-1917,1943-1947`.) |
| Usuário ausente/falso | `ValueError`. (`models.py:1948-1949`.) |
| C7 recebeu `perguntas=` incompleto ou antigo | A porta relê a relação inteira e pode encontrar resposta/viva que a consulta anterior não recebeu. Possibilidade estática; ocorrência nas disputas anexas **[não verificado]**. (`models.py:1889-1890,1916,1950,1959`.) |
| Estado mudou entre consulta e fechamento | As leituras dos predicados antecedem as travas internas e não são repetidas depois delas. Ocorrência concreta **[não verificado]**. (`models.py:1950-1976`.) |
| Exceção na trava, carimbos das exceções, save ou trilha | Pode impedir a conclusão do ato; qual operação teria falhado nos casos anexos **[não verificado]**. (`models.py:1974-2004`.) |

A tranca **já participa do predicado**, tanto na lista de respostas quanto em C6; não é uma recusa adicional posterior de `fechar()`. (`models.py:1903-1909`; `juizes.py:1403-1406`.)

**O168 pode reter o pai mesmo após a disputa fechar.** O signal pede a reconciliação do pai, aceita recusa e captura exceções. O juiz do CSF usa o mesmo corpo de resposta sem veredito, mas com `ignorar_tranca=True` e universo de origem mais destino; C7 conserva a isenção da tranca. Pai aberto, isoladamente, não prova falha do fechamento da disputa. (`signals.py:29-47`; `models.py:699-719`; `app/ponto/supra_juiz.py:561-574`.)

**6. Campos: significado, escritores e leitores**

| Campo solicitado | Significado e escritor |
|---|---|
| `validada_em` | Instante da validação; escritor normal `_carimbar_validacao`; a porta `carimbar(... resposta='validar')` também pode preenchê-lo. Resposta nova, devolução e reabertura podem limpá-lo. (`models.py:2527,3217-3219`; `services/validacao.py:159,224`; `services/disputa_emissao.py:1480,1595`.) |
| `materializada_em` | Instante do carimbo de resolução; a via declara o tipo de resolução, não necessariamente plantio de batida. Portas de carimbo/descarimbo e lote estão na tabela anterior. (`models.py:2542-2545,3180-3184,3291,3363-3365,3502`.) |
| `respondida_em` | Instante da resposta; pode ser o instante atual ou o testemunhado pela nota. Não é dia do fato. (`models.py:2526`; `services/disputa_emissao.py:1462-1468,1660,1672-1681`.) |
| `resposta` | O campo real é `resposta_colab`, texto cujo formato depende de `tipo_resposta`: escolha, hora, data/hora etc. Escritores: resposta normal, automática e limpeza da reabertura. (`models.py:2520-2526`; `services/disputa_emissao.py:1435-1459,1467,1595,1659`.) |
| `status/estado` da pergunta | `estado` é propriedade calculada; não há escritor de coluna de estado. (`models.py:3116-3131`.) |
| Retratação da pergunta | Via `duplicata`/`motivo_retratado` e ato na sequência de decisões; não é `Batida.retratada_em`. (`models.py:2573,2577-2580,2120-2125,4001-4009`; `services/fio.py:1398-1401`.) |
| `via` | O campo é `via_resolucao`: código textual da resolução; não é instante nem dia. (`models.py:2639-2641`.) |
| Fechamento da disputa | `fechada_em` é instante; `aberta` é 

[truncado em 20000 de 31970 caracteres]

## 15/11 08:02 · sessao 5d38228c71a2 · DISPUTA-ZUMBI-1110 · sonda · origem=Code

```python
# Django shell; executar no envelope de leitura, tenant juliani ja ativo.
# OBRA: DISPUTA-ZUMBI-1110
# HASH informado: 880331a8c8d394724b78217edc84bede94b99cb7.
# Conferido na copia; correspondencia ao HEAD Git: [nao verificado].
# A copia disponibilizada nao contem .git.
# Execucao, resultados e atualidade do dump: [nao verificado].
# Causa historica de nao terem fechado: [nao verificado].
#
# U = fechada_em IS NULL, sem filtro no chamado-pai:
# app/chamados/models.py:1804, :1870-1871.
# Empresa = empresa do colaborador DA DISPUTA:
# app/chamados/models.py:1783-1786; app/colaboradores/models.py:321.
# C7 real: DisputaSupervisao.pode_fechar(perguntas=...):
# app/chamados/models.py:1911-1917.
# C6 real: fio_vivo(perguntas), que chama pergunta_viva:
# app/chamados/juizes.py:1504-1513, :862.
#
# Carimbo usado para "resolvida": materializada_em:
# app/chamados/models.py:3117-3130, :3502-3505.
# Via gravada: via_resolucao:
# app/chamados/models.py:2639-2641; atribuicao conjunta em :3180-3184.
# Pergunta pode morrer por leitura sem carimbo:
# app/chamados/juizes.py:1400-1406.
#
# fechar() NAO sera chamado. Suas recusas de estado sao medidas pelos
# leitores reais:
# respostas_sem_veredito: app/chamados/models.py:1873-1909, :1950-1958;
# pergunta viva/C6: app/chamados/models.py:1959-1966.
# Tranca e lida pelo juiz e retira perguntas dessas recusas:
# app/chamados/models.py:1903-1909; app/chamados/juizes.py:974-992.
# fechar exige user: app/chamados/models.py:1948-1949.
# Usuario de eventual tentativa historica: [nao verificado].
# A etapa de lock ADQUIRE as travas, sem consulta separada de disponibilidade:
# app/chamados/models.py:1974-1976, :2707-2727, :2744-2771.
# Sua disponibilidade: "nao mensuravel sem escrever" neste envelope.
#
# Convencoes da sonda:
# Ultimo carimbo = MAX(materializada_em) entre as perguntas atuais da disputa.
# Empates preservam todas as vias; a combinacao conta uma vez no histograma.
# Cortes usam data local: ANTES < corte; EM_OU_DEPOIS >= corte.
# Sem carimbo fica em balde proprio, fora de ambas as faixas dos cortes.
# data_evento pertence a pergunta, nao a disputa:
# app/chamados/models.py:2494-2503.
# A amostra preserva todas as datas distintas, inclusive null.
# Via gravada nao prova o emissor/call site historico: [nao verificado].
# Qualquer ERRO invalida a conclusao; contagens podem ser parciais.
# Sem only/defer, troca de tenant, atomic, SET ou chamada de lock.

erros = 0
logging_mod = sys_mod = None
nivel_anterior = bytecode_anterior = None


def emitir(chave, valor):
    print("{} {}".format(chave, valor), flush=True)


class FalhaInterna(BaseException):
    def __init__(self, original):
        self.original = original


class ConsultaRecusada(BaseException):
    pass


def erro(exc):
    global erros
    if isinstance(exc, FalhaInterna):
        exc = exc.original
    erros += 1
    emitir("ERRO", "{} {}".format(erros, type(exc).__name__))


try:
    import sys

    sys_mod = sys
    bytecode_anterior = sys.dont_write_bytecode
    sys.dont_write_bytecode = True

    import logging

    logging_mod = logging
    nivel_anterior = logging.root.manager.disable
    logging.disable(10**9)

    import json
    import re
    from collections import Counter
    from contextlib import ExitStack
    from datetime import date
    from unittest.mock import patch

    from django.conf import settings
    from django.db import connection, connections
    from django.db.models import Prefetch
    from django.utils import timezone

    alias = connection.alias

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias
            or not isinstance(sql, str)
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b"
                r"|\bINTO\b|\b(?:INSERT|UPDATE|DELETE|MERGE|CALL|COPY)\b"
                r"|\b(?:set_config|nextval|setval|pg_advisory\w*)\s*\(",
                sql,
                re.I,
            )
        ):
            raise ConsultaRecusada()
        return execute(sql, params, many, context)

    # Instrumentacao apenas de falha interna, em memoria.
    # O C6 registra excecoes em app/chamados/juizes.py:1473-1488;
    # o registrador escreve em logging: app/core/observ.py:73-82.
    # Propaga em BaseException para nao medir um fallback apos erro interno.
    # Nenhum predicado de negocio e substituido.
    def falha_interna(logger_nome, operacao, exc, **contexto):
        raise FalhaInterna(exc)

    def emitir_json(chave, valor):
        emitir(chave, json.dumps(valor, ensure_ascii=True, sort_keys=True))

    with ExitStack() as pilha:
        for nome_alias in connections:
            pilha.enter_context(
                connections[nome_alias].execute_wrapper(somente_select)
            )

        # Classes: app/chamados/models.py:27, :1757, :2484.
        from chamados.models import (
            DisputaSupervisao,
            PerguntaDisputa,
            ChamadoColaborador,
        )
        from colaboradores.models import Empresa  # app/colaboradores/models.py:40
        # app/chamados/catalogo/motor.py:30-31.
        from chamados.catalogo.motor import TERMINAIS
        # app/chamados/juizes.py:1504, :974.
        from chamados.juizes import fio_vivo, dia_da_pergunta_trancado
        from core import observ  # app/core/observ.py:73

        pilha.enter_context(
            patch.object(observ, "registrar_engolido", new=falha_interna)
        )
        # Fuso configurado: app/config/settings/base.py:93, :95.
        pilha.enter_context(timezone.override(timezone.get_default_timezone()))
        if not settings.USE_TZ:
            raise RuntimeError("use_tz_incompativel")

        # Banco lateral e tenant: app/config/settings/sombra.py:7-14, :29.
        # Apenas confere; nao troca banco/schema.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_database(), current_schema()")
            banco, schema = cursor.fetchone()
        if banco != "sombra" or schema != "juliani":
            raise RuntimeError("envelope_incorreto")

        emitir("BANCO", banco)
        emitir("TENANT", schema)
        emitir("INSTANTE_LOCAL", timezone.localtime(timezone.now()).isoformat())
        emitir("FUSO_LOCAL", str(timezone.get_current_timezone()))
        emitir("DUMP_DE_HOJE", "[nao verificado]")
        emitir("CAUSA_HISTORICA_NAO_FECHOU", "[nao verificado]")
        emitir("M2_LOCK", "nao mensuravel sem escrever")
        emitir("M2_USER", "nao informado; fechar exige user")
        emitir("M2_CORTE_REGRA", "ANTES < corte; EM_OU_DEPOIS >= corte")
        emitir("M2_CARIMBO_CAMPO", "materializada_em")
        emitir("M2_VIA_CAMPO", "via_resolucao")

        cortes = (date(2026, 10, 22), date(2026, 11, 1))
        fixas = [
            "M1_U",
            "M1_MEDIDOS",
            "M1_PODE_FECHAR_TRUE",
            "M1_COM_PERGUNTA_VIVA_C6",
            "M1_SEM_PERGUNTA_VIVA_C6",
            "M1_SEM_PERGUNTA_NENHUMA",
            "M2_MEDIDOS",
            "M2_SEM_CARIMBO",
            "M2_PAI_TERMINAL",
            "M2_PAI_NAO_TERMINAL",
            "M2_SEM_PAI",
            "M2_RECUSA_RESPOSTAS_SEM_VEREDITO",
            "M2_RECUSA_PERGUNTA_VIVA",
            "M2_PERGUNTAS_TRANCADAS",
            "M2_COM_PERGUNTA_TRANCADA",
        ]
        for corte in cortes:
            for lado in ("ANTES", "EM_OU_DEPOIS"):
                fixas.append(
                    "M2_ULTIMO_CARIMBO_{}_{}".format(lado, corte.isoformat())
                )

        contagens = {
            pk: Counter()
            for pk in Empresa.objects.using(alias).values_list("pk", flat=True)
        }
        # Vias catalogadas: app/chamados/models.py:2603-2621.
        vias_catalogadas = {v for v, _ in PerguntaDisputa.VIAS}
        # STATUS_CHOICES / status_local: app/chamados/models.py:28-36, :52.
        status_catalogados = {s for s, _ in ChamadoColaborador.STATUS_CHOICES}

        # Nunca imprime texto livre, nome, CPF, objetos ou mensagens de excecao.
        def via_segura(valor):
            if not valor:
                return "vazia"
            return valor if valor in vias_catalogadas else "fora_do_catalogo"

        # Relacoes: app/chamados/models.py:1783, :1796, :2492-2497, :3494-3500;
        # empresa do colaborador: app/colaboradores/models.py:321.
        universo = (
            DisputaSupervisao.objects.using(alias)
            .filter(fechada_em__isnull=True)
            .select_related("colaborador__empresa", "chamado")
            .prefetch_related(
                Prefetch(
                    "perguntas",
                    queryset=PerguntaDisputa.objects.using(alias)
                    .select_related("celula", "chamado_destino")
                    .order_by("pk"),
                )
            )
            .order_by("pk")
        )

        ultimo_pk = None
        amostras = 0
        while True:
            pagina_qs = universo
            if ultimo_pk is not None:
                pagina_qs = pagina_qs.filter(pk__gt=ultimo_pk)
            pagina = list(pagina_qs[:100])
            if not pagina:
                break

            for disputa in pagina:
                try:
                    empresa_pk = disputa.colaborador.empresa_id
                    cont = contagens.setdefault(empresa_pk, Counter())
                    cont["M1_U"] += 1
                    perguntas = list(disputa.perguntas.all())

                    # Juizes REAIS:
                    # app/chamados/models.py:1911;
                    # app/chamados/juizes.py:1504.
                    pode = disputa.pode_fechar(perguntas=perguntas)
                    viva = fio_vivo(perguntas)

                    cont["M1_MEDIDOS"] += 1
                    cont["M1_PODE_FECHAR_TRUE"] += int(pode)
                    cont["M1_COM_PERGUNTA_VIVA_C6"] += int(viva)
                    cont["M1_SEM_PERGUNTA_VIVA_C6"] += int(not viva)
                    cont["M1_SEM_PERGUNTA_NENHUMA"] += int(not perguntas)
                    if not pode:
                        continue

                    # Mesmo leitor chamado por fechar():
                    # app/chamados/models.py:1950.
                    pendentes = disputa.respostas_sem_veredito(
                        perguntas=perguntas
                    )
                    # Tranca real, por leitura: app/chamados/juizes.py:974-992.
                    trancadas = sum(
                        int(dia_da_pergunta_trancado(p)) for p in perguntas
                    )

                    carimbadas = [
                        p for p in perguntas if p.materializada_em is not None
                    ]
                    ultimo = max(
                        (p.materializada_em for p in carimbadas), default=None
                    )
                    local = timezone.localtime(ultimo) if ultimo else None
                    empatadas = [
                        p for p in carimbadas if p.materializada_em == ultimo
                    ]
                    vias = sorted(
                        {via_segura(p.via_resolucao) for p in empatadas}
                    )
                    via_grupo = "__".join(vias) if vias else "sem_carimbo"

                    pai = disputa.chamado
                    # Predicado terminal: status_local in TERMINAIS,
                    # app/chamados/catalogo/motor.py:30-31.
                    terminal = (
                        pai.status_local in TERMINAIS if pai is not None else None
                    )
                    pai_status = (
                        pai.status_local
                        if pai is not None
                        and pai.status_local in status_catalogados
                        else ("sem_pai" if pai is None else "fora_do_catalogo")
                    )

                    delta = Counter({
                        "M2_MEDIDOS": 1,
                        "M2_SEM_CARIMBO": int(local is None),
                        "M2_PAI_TERMINAL": int(terminal is True),
                        "M2_PAI_NAO_TERMINAL": int(terminal is False),
                        "M2_SEM_PAI": int(pai is None),
                        "M2_RECUSA_RESPOSTAS_SEM_VEREDITO": int(bool(pendentes)),
                        "M2_RECUSA_PERGUNTA_VIVA": int(viva),
                        "M2_PERGUNTAS_TRANCADAS": trancadas,
                        "M2_COM_PERGUNTA_TRANCADA": int(trancadas > 0),
                        "M2_PAI_STATUS_" + pai_status: 1,
                        "M2_VIAS_ULTIMO_CARIMBO_" + via_grupo: 1,
                        "M2_ULTIMO_CARIMBO_DIA_" + (
                            local.date().isoformat()
                            if local else "sem_carimbo"
                        ): 1,
                    })
                    if local is not None:
                        for corte in cortes:
                            lado = (
                                "ANTES" if local.date() < corte
                                else "EM_OU_DEPOIS"
                            )
                            delta["M2_ULTIMO_CARIMBO_{}_{}".format(
                                lado, corte.isoformat()
                            )] += 1
                    for chave, quantidade in delta.items():
                        cont[chave] += quantidade

                    # Uma linha para CADA disputa pode_fechar=True medida.
                    detalhe = {
                        "disputa_pk": disputa.pk,
                        "empresa_pk": empresa_pk,
                        "pai_status": pai_status,
                        "pai_terminal": terminal,
                        "ultimo_carimbo_local": (
                            local.isoformat() if local else None
                        ),
                        "via": vias,
                        "perguntas_ultimo_carimbo": [p.pk for p in empatadas],
                        "respostas_sem_veredito": len(pendentes),
                        "recusa_por_vivas": bool(viva),
                        "perguntas_trancadas": trancadas,
                    }
                    emitir_json("M2_DISPUTA", detalhe)

                    if amostras < 25:
                        # app/chamados/models.py:2501-2503.
                        dias = sorted({
                            p.data_evento.isoformat()
                            for p in perguntas if p.data_evento
                        })
                        if any(p.data_evento is None for p in perguntas):
                            dias.append(None)
                        amostra = dict(detalhe)
                        amostra["colaborador_pk"] = disputa.colaborador_id
                        amostra["data_evento"] = dias
                        emitir_json("M3_AMOSTRA", amostra)
                        amostras += 1

                except BaseException as exc:
                    erro(exc)

            ultimo_pk = pagina[-1].pk

        # Todas as empresas, inclusive zero; total = soma das empresas.
        chaves = sorted(
            set(fixas).union(*(set(c) for c in contagens.values()))
        )
        totais = Counter()
        for empresa_pk in sorted(contagens):
            cont = contagens[empresa_pk]
            for chave in chaves:
                emitir("{}_EMPRESA_{}".format(chave, empresa_pk), cont[chave])
                totais[chave] += cont[chave]
        for chave in chaves:
            emitir(chave + "_TOTAL", totais[chave])
        emitir("M3_AMOSTRAS_TOTAL", amostras)

except BaseException as exc:
    erro(exc)
finally:
    try:
        if logging_mod is not None and nivel_anterior is not None:
            logging_mod.disable(nivel_anterior)
        if sys_mod is not None and bytecode_anterior is not None:
            sys_mod.dont_write_bytecode = bytecode_anterior
    except BaseException as exc:
        erro(exc)
    emitir("ERROS_TOTAL", erros)
    emitir("FIM", "ok={}".format(int(erros == 0)))
```

## 15/11 08:01 · sessao df4e760110f8 · - · - · origem=Ronald (tmux)

Censo concluído em leitura; listas de referências por campo foram enviadas ao pai.

- O explicador mudou: usa agora `disputa.pode_fechar(perguntas=ps)`; caiu a divergência `not fio_vivo` da triagem. A chave publicada é o resultado real de C7. (`app/chamados/services/explicador.py:139,165`)
- Seus consumidores são `desenlace.py:63` e `modal_fio.html:435,446`. O desenlace ainda traduz `True` como “Tudo validado e gravado”, expressão mais forte que C7: uma disputa vazia também satisfaz o predicado. (`app/chamados/services/desenlace.py:64`; `app/chamados/models.py:1916-1917`)
- O cron continua exclusivamente no recorte de beco: `--alarme`, sem escrita; aberta, pai terminal e nenhuma pergunta visível. `--apply` permanece como ação manual. (`app/config/crons.py:663`; `app/chamados/services/becos.py:27-33`; `app/chamados/management/commands/fechar_becos_disputa.py:30,57,67-70`)
- O relatório sob demanda lê C7, mas separa as vazias antes da consulta; portanto sua categoria `PRONTA_FECHAR` não conta todo o universo aberta+C7. (`app/chamados/relatorios_disputa.py:12-25`; `app/config/crons.py:1463`)
- O auditor C2 também não é esse contador: conta perguntas por carimbos e veredito da célula, sem C7 e sem filtro de disputa aberta, apesar do rótulo. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1601-1605`)
- Pergunta não tem `status/estado` persistido: `estado` é propriedade. Abertura da disputa é `fechada_em is None`. Retratação da pergunta usa `via_resolucao` e ato da `DecisaoPergunta`. (`app/chamados/models.py:3117-3131,1871,2580,4001`)
- Os carimbos são instantes; `data_evento` é dia do turno. A projeção temporal de C6 desconsidera carimbos posteriores a `agora`; a tarefa também corta decisões por instante do ato. (`app/chamados/models.py:2526-2527,3502,1804`; `app/chamados/juizes.py:1378-1396`; `app/chamados/services/tarefa_do_dia.py:232-235,410-420`)
- A lista de resposta sem veredito hoje aceita carimbo **ou texto**, inclusive carimbo sem texto. CSF usa o mesmo corpo com `ignorar_tranca=True`; C7 conserva a isenção de dia trancado. (`app/chamados/models.py:1900-1909`; `app/ponto/supra_juiz.py:567-574`)

Leitores diretos C7: `relatorios_disputa.py:24`; `reconciliador.py:412,612,1171`; `services/materializacao.py:591,1322`; `becos.py:33,56`; `competencia_trancada.py:178,227`; `explicador.py:139`; `disputa_emissao.py:1278,2098`; `app/ponto/services/entrada_sincronizada.py:59`.

Limite: inventário por leitura estática, incluindo atributos, `getattr`, filtros, projeções SQL, templates e fotografia genérica; execução em produção, estado atual e causas das medições anexas permanecem [não verificado].

## 15/11 07:59 · sessao 2ceba740f9a0 · - · - · origem=Ronald (tmux)

Censo do recorte `models.py`, `reconciliador.py`, `services/` e `signals.py`, sem escrita.

**Convenção B:** todo `PerguntaDisputa.save()` agenda `fechar_beco_da_disputa` por `post_save → on_commit`; somente chega ao C7 se disputa aberta, pai terminal e nenhuma pergunta sem `respondida_em` com destino não terminal. Há guarda de reentrada. (`app/chamados/signals.py:229`, `app/chamados/signals.py:240`, `app/chamados/services/becos.py:25`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:70`.)

| Carimbador/fluxo | Escrita | C7/fechar no fluxo e condição |
|---|---|---|
| `lavrar_sem_turno_aberto` | `carimbar(SEM_TURNO_ABERTO)` | Direto: aberta + C7 → `fechar`; também B. (`app/chamados/reconciliador.py:406`, `app/chamados/reconciliador.py:412`.) |
| `retratar_perguntas_absolvidas_por_celula` | Carimbo `CELULA_ABSOLVEU` | Após lote, relê `fechada_em`; aberta + C7 → `fechar`; exceções registradas. Também B. (`app/chamados/reconciliador.py:593`, `app/chamados/reconciliador.py:596`, `app/chamados/reconciliador.py:609`, `app/chamados/reconciliador.py:612`.) |
| `encerrar_orfas_pela_lampada` | `encerrar_fantasma` → carimbo | Depois do lote: aberta, nenhuma resposta sem veredito, C7 → fechar; também B. (`app/chamados/reconciliador.py:1148`, `app/chamados/reconciliador.py:1159`, `app/chamados/reconciliador.py:1164`, `app/chamados/reconciliador.py:1171`.) |
| `_materializar_ausencia_de_resposta` | Carimba irmãs com resposta=`validar`, via declaração/ausência; registra absorção | Aberta + C7 → fechar ao final; também B. Ausência aberta não fecha destino. (`app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567`, `app/chamados/services/materializacao.py:569`, `app/chamados/services/materializacao.py:587`, `app/chamados/services/materializacao.py:591`.) |
| `_materializar_uma` | Carimbo+resposta=`validar`+lavra | Chamador `materializar_perguntas_validadas_da_disputa` termina em `_fechar_disputa_se_quitada`: aberta + C7 → fechar; também B. (`app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1381`, `app/chamados/services/materializacao.py:1384`, `app/chamados/services/materializacao.py:1322`, `app/chamados/services/materializacao.py:1339`.) |
| Materializador: paridade recusada | Carimbo identidade `PARIDADE_RECUSADA` | Mesmo fluxo da unidade/materialização acima; também B. (`app/chamados/services/materializacao.py:930`.) |
| `_carimbar_recusa`: colisão | Carimbo identidade `FATO_OCUPADO` | Mesmo fluxo da unidade/materialização acima; também B. Exige sem carimbo e código `COLISAO`. (`app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1303`.) |
| `materializar_pendentes_aprovadas` | Carimbo `JUIZ_APROVOU` | Chamado pelo signal de disputa já fechada; não pergunta C7 diretamente. Pula pergunta em voo e exige lastro; também B. (`app/chamados/signals.py:155`, `app/chamados/signals.py:165`, `app/chamados/services/materializacao.py:1431`, `app/chamados/services/materializacao.py:1433`, `app/chamados/services/materializacao.py:1437`.) |
| S143, perguntas com destino | Carimbo via fato, resposta=`validar`, lavra | Após os laços, relê disputa; pula se alguma pergunta em voo; aberta + C7 → fechar. Antes: pula C6 viva, dia trancado e resposta sem lastro. Também B. (`app/chamados/services/disputa_emissao.py:1973`, `app/chamados/services/disputa_emissao.py:1977`, `app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:1993`, `app/chamados/services/disputa_emissao.py:2088`, `app/chamados/services/disputa_emissao.py:2096`, `app/chamados/services/disputa_emissao.py:2098`.) |
| S143, sem credor vivo | Carimbo via fato, resposta=`responder` | Mesma cauda C7 acima; exige pai/destinos fora `VIVOS`, pergunta não em voo e C6 morta. Também B. (`app/chamados/services/disputa_emissao.py:2039`, `app/chamados/services/disputa_emissao.py:2043`, `app/chamados/services/disputa_emissao.py:2052`, `app/chamados/services/disputa_emissao.py:2055`, `app/chamados/services/disputa_emissao.py:2067`.) |
| `responder_pergunta_disputa_core` | Resposta+instante; desfaz validação antiga; desfaz carimbo sem lastro | Não C7 direto. Escape que vira ausência chama fluxo acima; demais têm B. (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1508`, `app/chamados/services/disputa_emissao.py:1538`.) |
| `responder_automaticamente` | Texto+instante | Sem C7 direto; caller API posteriormente valida; também B. (`app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1662`, `app/api/foto_recebida.py:48`, `app/api/foto_recebida.py:51`.) |
| `carimbar_resposta_pela_trilha` | `respondida_em` testemunhada pela nota | Sem C7 direto; B. (`app/chamados/services/disputa_emissao.py:1677`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/disputa_emissao.py:1683`.) |
| `_carimbar_validacao` / `validar_pergunta` | `validada_em`+autor | Validação normal chama materialização integral, cuja cauda pergunta C7; também B. (`app/chamados/services/validacao.py:159`, `app/chamados/services/validacao.py:162`, `app/chamados/services/validacao.py:460`, `app/chamados/services/validacao.py:494`.) |
| Aceite de folga | Validação e `fechar_por_admin` | Sem C7 direto neste ramo; `fechar_o_fio_do_dia` pode reconciliar chamados e disparar S143 condicionalmente; também B. (`app/chamados/services/validacao.py:313`, `app/chamados/services/validacao.py:314`, `app/chamados/services/validacao.py:321`, `app/chamados/services/validacao.py:345`, `app/chamados/services/validacao.py:354`, `app/ponto/services/veredito_celula.py:203`.) |
| `devolver_validacao_recusada` | Limpa validação e autor | Sem C7 direto; pode reabrir disputa fechada, condicionado a `reabrir`; B. (`app/chamados/services/validacao.py:224`, `app/chamados/services/validacao.py:227`, `app/chamados/services/validacao.py:242`.) |
| `relavrar` | Troca somente via derivada, relavra cache | Sem C7 direto; B. (`app/chamados/services/relavra_via.py:118`, `app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:127`.) |
| `retratar_duplicata` / `retratar_para_canonica` | Identidade `DUPLICATA`, preserva instante existente | Sem C7 direto nos dois; B. Porta exige ordem de travas e recusa conversa. (`app/chamados/services/fio.py:1373`, `app/chamados/services/fio.py:1375`, `app/chamados/services/fio.py:1380`, `app/chamados/services/fio.py:1398`.) |
| `remover_motivo` | Linha retratação + identidade `MOTIVO_RETRATADO` | Restantes: retorna sem C7/fechar, somente B. Zero restantes: `fechar` direto, que julga perguntas. (`app/chamados/models.py:2120`, `app/chamados/models.py:2125`, `app/chamados/models.py:2130`, `app/chamados/models.py:2136`.) |
| `fechar_por_admin` | Identidade `ADMIN_DECIDIU` | Método não pergunta C7; está dentro de `Disputa.fechar` nas exceções desligamento/admin, ou nos fluxos de dia/folga acima; B. (`app/chamados/models.py:3147`, `app/chamados/models.py:1982`, `app/chamados/models.py:1985`, `app/ponto/services/veredito_celula.py:190`.) |
| `descarimbar` | Zera materialização/autor/via | Sem C7 direto; B somente se salvar=True. (`app/chamados/models.py:3291`, `app/chamados/models.py:3294`.) |
| `descarimbar_em_lote` | `QuerySet.update` dos três campos + extras | Nenhum C7 nem B; exclui linhas fora do fato e exige colaboradores travados. (`app/chamados/models.py:3357`, `app/chamados/models.py:3363`.) |
| `reabrir_para_correcao` | Lote: zera resposta, respondida, validação, autor, materialização e via | Não C7/B; chama lote e emite eventos de célula. (`app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1635`.) |
| `ressuscitar_perguntas` | Lote: limpa validação/respondida+carimbo | Não C7/B; emite eventos e reabre disputas fechadas afetadas. (`app/chamados/models.py:1613`, `app/chamados/models.py:1615`, `app/chamados/models.py:1620`.) |
| `desfazer_carimbo_sem_lastro` | Descarimba quando C6 viva | Sem C7 direto; B; reabre disputa fechada quando admitida pelo recorte `reabrir_fechada`. (`app/chamados/services/relavra_via.py:238`, `app/chamados/services/relavra_via.py:242`, `app/chamados/services/relavra_via.py:246`, `app/chamados/services/relavra_via.py:260`.) |
| `encerrar_por_fato_no_espelho` | Troca via `FATO_MATERIALIZADO` | Sem C7 direto; B. (`app/chamados/models.py:3372`, `app/chamados/models.py:3374`.) |
| `reabrir_fantasma` | Descarimba somente via fantasma com materialização | Sem C7 direto; B. (`app/chamados/models.py:3395`, `app/chamados/models.py:3397`.) |
| `reabrir_por_resposta` | Descarimba em memória somente vias sem lastro | Sem C7 direto; salvamento é do escritor da resposta; B naquele save. (`app/chamados/models.py:3412`, `app/chamados/models.py:3414`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1508`.) |
| `reabrir_por_competencia` | Descarimba somente via competência trancada | Sem C7 direto; B. (`app/chamados/models.py:3419`, `app/chamados/models.py:3421`, `app/chamados/services/competencia_trancada.py:279`.) |
| `_nascer` | Insere pergunta com `**campos` recebidos | Sem C7 direto; B também no nascimento. Recusa respondida sem texto e registra projeção de decisões. (`app/chamados/models.py:2922`, `app/chamados/models.py:3097`, `app/chamados/models.py:3100`, `app/chamados/models.py:3102`.) |

A porta básica de materialização é `_escrever_carimbo`; `carimbar` aplica os campos em memória, exige C6 morta e salva; `carimbar_identidade` grava sem perguntar C6, pois a via determina identidade. Nenhuma das duas pergunta C7 diretamente. (`app/chamados/models.py:3180`, `app/chamados/models.py:3212`, `app/chamados/models.py:3222`, `app/chamados/models.py:3235`, `app/chamados/models.py:3251`, `app/chamados/models.py:3273`.)

O `estado` de pergunta é propriedade derivada; não há coluna de estado gravada nesse modelo. Retratação da pergunta é via+linha de decisão, não um campo `retratada_em` próprio. (`app/chamados/models.py:3117`, `app/chamados/models.py:3124`, `app/chamados/models.py:2577`, `app/chamados/models.py:2580`.)

`gravar_lavras_em_lote` permanece sem C7 e sem post_save: grava por `bulk_update` somente cache da lavra. É incorreto apresentá-lo como resolução direta pelos campos pedidos: o C6 lê os cinco campos projetados e consulta lastro, não `veredito_lavrado` nessa função. (`app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/juizes.py:1399`, `app/chamados/juizes.py:1483`.)

Delta verificável contra texto da triagem: `reabrir_para_correcao` agora usa sempre a porta do lote e desfaz também o carimbo/via; continua sem C7/B. A própria fonte documenta a troca do antigo ramo `update` cru em 12/11. `lavrar_sem_turno_aberto` continua perguntando C7; a alegação “não se achou chamada” do aval não descreve esse HEAD. A data efetiva da alteração histórica depende do git ausente [não verificado]. (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/reconciliador.py:412`.)

Para ampliar ao conjunto de insumos C6: reuso/nascimento, reapontar resolvedora e rechavear data alteram célula/dia/destino e lavra, sem C7 direto; saves passam por B. (`app/chamados/models.py:3086`, `app/chamados/models.py:3089`, `app/chamados/services/fio.py:959`, `app/chamados/services/fio.py:964`, `app/chamados/services/fio.py:966`, `app/chamados/services/fio.py:1076`, `app/chamados/services/fio.py:1082`.)

Timestamps são instantes: a projeção C6 descarta validação/resposta/materialização posteriores a `agora`; `data_evento` é dia do turno para fato, enquanto conversa usa dia civil. (`app/chamados/juizes.py:1378`, `app/chamados/juizes.py:1385`, `app/chamados/juizes.py:1387`, `app/ponto/services/veredito_celula.py:178`.)

## 15/11 07:58 · sessao f1124a8c2bc8 · - · - · origem=Ronald (tmux)

Censo somente da cópia; referências abaixo são do código atual. Não li banco/log nem executei a aplicação [não verificado]. [nao verificado]

**Escritores do fechamento**

- `DisputaSupervisao` não tem campo persistido `status`/`estado`: o estado aberto é `fechada_em is None`; `__str__` deriva “aberta/fechada” dessa propriedade (`app/chamados/models.py:1803`, `:1869`, `:2340`).
- Único escritor positivo encontrado de `fechada_em`: `DisputaSupervisao.fechar`, que grava instante `timezone.now()`, autor, resolução e motivo juntos (`app/chamados/models.py:1996-2000`). A definição é `DateTimeField(null=True, blank=True)`, portanto instante, não dia (`app/chamados/models.py:1804`).
- Reabertura administrativa: zera `fechada_em`, autor, resolução e motivo; incrementa contador; exige disputa fechada, usuário e menos de duas reaberturas (`app/chamados/models.py:2202-2225`).
- Reabertura sistêmica: mesmos quatro campos, idempotente quando já aberta, sem teto/usuário, anexa ocorrência ao pai (`app/chamados/models.py:2231-2244`).
- `reabrir_por_colab` reabre questionário de disputa que **continua aberta**, e recusa disputa fechada (`app/chamados/models.py:2257-2266`, `:2288-2304`).
- Nascimentos de disputa estão em `app/chamados/services/disputa_emissao.py:587`, `:1123`, `:1358`; `fechada_em` fica no default nulo do modelo (`app/chamados/models.py:1804`).
- O desligamento atual usa a porta `fechar`; seu comentário declara removido o antigo `.update(fechada_em=...)` (`app/colaboradores/services/desligamento.py:157-167`). Atenção: `bin/esmeril_chamado_censo.py:64-71` ainda descreve a exceção bulk antiga, divergindo do corpo atual.

**Os 15 sítios que chamam diretamente `DisputaSupervisao.fechar`**

| Sítio | Consulta C7/condição anterior |
|---|---|
| `app/chamados/models.py:2014` (`admin_encerra`) | Exige motivo textual não vazio (`:2011-2013`); porta de exceção. |
| `app/chamados/models.py:2136` (`remover_motivo`) | Só se não restarem motivos; com restantes retorna antes (`:2130-2136`). |
| `app/colaboradores/services/desligamento.py:167` | Aberta e sem respostas sem veredito (`:164-166`); porta `desligamento`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Aberta e `fio_vivo=False`; não consulta C7 literalmente; captura `ValueError` (`:81-93`). |
| `app/chamados/reconciliador.py:413` | Aberta e C7 após `carimbar(VIA_SEM_TURNO_ABERTO)` retornar verdadeiro (`:406-414`). |
| `app/chamados/reconciliador.py:613` | Após lote de absolvidas pela célula; refresh, aberta e C7; captura exceção (`:596-617`). |
| `app/chamados/reconciliador.py:1172` | Após `encerrar_fantasma`; refresh, aberta, sem resposta sem veredito, C7; captura exceção (`:1148`, `:1159-1177`). |
| `app/api/foto_recebida.py:80` | Disputa aberta, última batida coincide e não restam motivos; valida perguntas antes (`:61-80`). |
| `app/chamados/services/becos.py:57` | Resolve perguntas ligadas a chamados terminais; refresh, aberta e C7 (`:42-57`). Entrada de evento exige `classificar=FECHAVEL`, incluindo pai terminal (`:25-33`, `:74-78`). |
| `app/chamados/services/competencia_trancada.py:229` | `aplicar`, competência trancada e ações (`:192`); medição exige C7 e não admite pai vivo fora do conjunto encerrável (`:173-183`); execução consulta C7 novamente (`:221-230`). |
| `app/chamados/services/disputa_emissao.py:1284` | Após perguntar entradas pendentes, C7 e **sem** abstenção por ata ausente (`:1270-1287`). |
| `app/chamados/services/disputa_emissao.py:2099` | Refresh sob consulta nova de tranca; pula pergunta “em voo”; aberta e C7 (`:2088-2099`). |
| `app/chamados/services/materializacao.py:592` | Após absorção de irmãs por ausência; aberta e C7 (`:587-592`). |
| `app/chamados/services/materializacao.py:1339` | `_fechar_disputa_se_quitada`: aberta e C7 (`:1319-1339`). |
| `app/ponto/services/entrada_sincronizada.py:60` | Pai manual vivo, contém órfão e agrupa **só esse** órfão; disputa aberta e C7 (`:48-62`). |

Não há chamada de produção a `admin_encerra()` encontrada além da própria definição [não verificado quanto a invocação dinâmica]; a definição chama `fechar` (`app/chamados/models.py:2007-2014`).

**Reabridores: chamadores**

| Porta | Chamadores |
|---|---|
| `reabrir_admin` | Ação administrativa: exige `admin_ti`, motivo válido, justificativa ≥20 caracteres, busca disputa fechada mais recente e chama porta (`app/chamados/services/acoes_disputa.py:247-259`). |
| `reabrir_sistema` | Ressuscitação por chamado, se lote descarimbou e disputa está fechada (`app/chamados/models.py:1613-1622`). |
| `reabrir_sistema` | Desfaz carimbo sem lastro, se pergunta voltou viva e disputa fechada (`app/chamados/services/relavra_via.py:242-261`). |
| `reabrir_sistema` | Devolução de validação, somente se flag `reabrir` e disputa fechada após refresh (`app/chamados/services/validacao.py:234-244`). |
| `reabrir_sistema` | Ausência rejeitada: reabre perguntas e respectivas disputas no mesmo atomic (`app/chamados/services/materializacao.py:681-696`). |
| `reabrir_sistema` | Comando de órfãs enterradas: após `reabrir_fantasma`, se disputa fechada; tenta reabrir pai separadamente (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157-172`). |

**Diferença `pode_fechar` × `fechar`**

O predicado de negócio é equivalente para o **mesmo universo e estado**: C7 usa `not respostas_sem_veredito` e `not fio_vivo` (`app/chamados/models.py:1915-1917`), `fio_vivo` é `any(pergunta_viva)` (`app/chamados/juizes.py:1513`), e `fechar` consulta a mesma lista de respostas e a mesma `pergunta_viva` (`app/chamados/models.py:1950`, `:1959-1966`). `fechar` não chama `pode_fechar` literalmente (`app/chamados/models.py:1937-1967`).

Mesmo com C7 verdadeiro:

1. Motivo explícito desconhecido levanta `ValueError` (`app/chamados/models.py:1940-1941`).
2. Disputa já fechada: mesma entrada retorna `False`; entrada diferente levanta `ValueError`. C7 não testa `fechada_em` (`app/chamados/models.py:1915-1917`, `:1943-1947`).
3. Usuário falso/ausente levanta `ValueError` (`app/chamados/models.py:1948-1949`).
4. C7 aceita `perguntas=` fornecidas pelo chamador; `fechar` relê toda a relação. Se o universo passado estiver incompleto/desatualizado, as respostas/vivas podem divergir — inferência estática, ocorrência em produção [não verificado] (`app/chamados/models.py:1889-1890`, `:1916`, `:1950`, `:1959`).
5. As leituras dos predicados antecedem a transação/travas do próprio `fechar`; não há reavaliação depois da trava. Alteração entre leituras é possível — ocorrência em produção [não verificado] (`app/chamados/models.py:1950-1976`).
6. Travas, gravação e trilha são operações que podem propagar exceção; qual delas falhou no dado raspado [não verificado] (`app/chamados/models.py:1974-1976`, `:1985-2004`).

A tranca da competência **participa do C7**, não é recusa adicional de `fechar`: respostas em dia trancado saem da lista (`app/chamados/models.py:1903-1909`), e C6 dá pergunta como morta pelo mesmo juiz (`app/chamados/juizes.py:1403-1406`).

**O168 / pai**

Fechar disputa não garante fechar pai: o signal chama `ch.reconciliar`, e só escreve nota se este efetivamente transicionar; exceções são capturadas (`app/chamados/signals.py:29-47`). O gate do pai passa pela premissa, conversa e pergunta viva (`app/chamados/models.py:683-693`). A guarda O168 aplica-se a chamado-pai categoria `disputa_supervisao`, considera perguntas de origem e destino e pode recusá-lo por resposta sem veredito; o próprio código declara que a disputa fecha normalmente (`app/chamados/models.py:699-719`). Portanto pai aberto não prova falha de `DisputaSupervisao.fechar` (`app/chamados/signals.py:43-47`, `app/chamados/models.py:708-710`).

**Delta contra triagem**

- `lavrar_sem_turno_aberto` continua consultando C7 e chamando a porta depois do carimbo (`app/chamados/reconciliador.py:406-414`).
- “Um escritor de fechamento” continua correto no corpo atual (`app/chamados/models.py:1996-2000`); há **15** sítios diretos identificados acima, frente aos “14” da triagem, sem identificar qual foi introduzido desde o hash anterior [não verificado].
- Reabridores de disputa continuam dois; reabertura de questionário do colaborador é outro ato (`app/chamados/models.py:2190`, `:2227`, `:2257-2266`).
- C7/fechar ainda usam corpos semanticamente equivalentes, sem chamada literal de um ao outro (`app/chamados/models.py:1915-1917`, `:1950-1967`).
- A proteção atual contra resposta sem veredito inclui **carimbo OU texto**, inclusive carimbo com texto vazio (`app/chamados/models.py:1900-1909`); relação cronológica exata contra a cópia de 10/11 [não verificado].
- O168 mantém pai aberto independentemente do resultado do fechamento da disputa (`app/chamados/models.py:699-719`, `app/chamados/signals.py:43-47`).

**Não consegui verificar:** hash Git real do archive, histórico exato dos deltas, execução em produção, datas dos últimos carimbos, quais 18 disputas fecharam/recusaram/falharam ou mudaram após a medição [não verificado].

## 15/11 07:15 · sessao 60a0a64256ab · CONTRATO-DE-ENTRADA-DO-FIO · sonda · origem=Code

```python
# Django shell; tenant juliani já ativo no envelope de leitura.
# Execução/resultados de produção: [nao verificado].
# Correspondência desta cópia ao HEAD Git: [nao verificado].
# HASH informado pelo solicitante: 880331a8c8d394724b78217edc84bede94b99cb7.
#
# Escritor atual: agora = timezone.now(), depois agora.isoformat():
# app/api/views.py:497, app/api/views.py:761;
# app/api/views_core.py:483, app/api/views_core.py:755.
# Com USE_TZ=True, agora sai aware em UTC: offset +00:00.
# Configuração: app/config/settings/base.py:93, app/config/settings/base.py:95.
# Novo escritor solicitado: timezone.localtime(ts_efetivo).isoformat().
# Esta sonda mede o contrafactual pedido, usando Batida.timestamp;
# execução futura da alteração em produção: [nao verificado].
#
# Leitores reais: app/chamados/juizes.py:87, app/chamados/juizes.py:171.
# Último degrau lê ultima_batida.timestamp e extrai date() no offset gravado:
# app/chamados/juizes.py:159, app/chamados/juizes.py:166.
# O singular consulta o catálogo; o lote lê data_turno diretamente:
# app/chamados/juizes.py:113, app/chamados/juizes.py:187.
# Aqui são feitas literalmente as duas chamadas ao LOTE solicitadas.
#
# Convenções da sonda:
# U_CHAMADOS: não terminais com referência a Batida existente.
# U_DISPUTAS: disputas abertas cujo chamado tem referência a Batida existente;
# não acrescenta filtro de terminal ao chamado desse segundo universo.
# Disputas contam por PK de disputa; amostras deduplicam PK de chamado.
# Empresa da medição/exportação = empresa do colaborador do chamado.
# Disputa sem chamado é contabilizada na empresa do colaborador da disputa.
# None não pertence a competência alguma.
# Contadores são parciais se ERRO > 0. Nenhuma exceção imprime seu conteúdo.

erros = 0
logging_anterior = None


def emitir(chave, valor):
    print("{} {}".format(chave, valor), flush=True)


def erro(etapa, exc, chamado_pk=None):
    global erros
    erros += 1
    emitir(
        "ERRO_ITEM",
        "etapa={} chamado_pk={} classe={}".format(
            etapa,
            chamado_pk if chamado_pk is not None else "-",
            type(exc).__name__,
        ),
    )


try:
    import logging
    import re
    from collections import Counter, defaultdict
    from contextlib import ExitStack
    from copy import copy, deepcopy
    from datetime import timedelta

    from django.conf import settings
    from django.db import connection, connections
    from django.utils import timezone

    logging_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)

    alias_leitura = connection.alias

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias_leitura
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
                sql,
                re.I,
            )
        ):
            raise RuntimeError("consulta_recusada")
        return execute(sql, params, many, context)

    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )
        # Usa o fuso configurado da aplicação:
        # app/config/settings/base.py:93.
        pilha.enter_context(timezone.override(timezone.get_default_timezone()))

        # Model/campos: app/colaboradores/models.py:40,
        # app/colaboradores/models.py:46, app/colaboradores/models.py:321.
        from colaboradores.models import Empresa
        # app/ponto/models.py:8, app/ponto/models.py:23.
        from ponto.models import Batida
        # Chamado: app/chamados/models.py:27, :38, :47, :50, :52.
        # Disputa: app/chamados/models.py:1783, :1796, :1804.
        from chamados.models import ChamadoColaborador, DisputaSupervisao
        # Terminal = status_local em TERMINAIS; inclui resolvido, fechado,
        # cancelado e superado: app/chamados/catalogo/motor.py:30,
        # app/chamados/catalogo/motor.py:31.
        from chamados.catalogo.motor import TERMINAIS
        from chamados.juizes import dias_do_fato  # app/chamados/juizes.py:171
        # Autoridade exportada: app/folha/export.py:556; retorno em :594.
        from folha.export import competencia_entregue
        # Autoridade do intervalo/corte da Empresa:
        # app/ponto/janelas.py:60, app/ponto/janelas.py:36.
        from ponto.janelas import janela_atual
        # Importa só a constante; não executa Command.handle:
        # app/ponto/management/commands/marcar_foto_ausente_retro.py:45.
        from ponto.management.commands.marcar_foto_ausente_retro import (
            DIAS_PARA_TRAS,
        )

        # Sem troca de tenant, transação, SET ou savepoint.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise RuntimeError("tenant_incorreto")

        if not settings.USE_TZ:
            raise RuntimeError("use_tz_incompativel")

        empresas = {
            e.pk: e
            for e in Empresa.objects.using(alias_leitura).only(
                "pk", "dia_inicio_competencia"
            )
        }
        agora = timezone.now()
        local_agora = timezone.localtime(agora)
        emitir("FUSO_LOCAL", str(timezone.get_current_timezone()))
        emitir("ESCRITOR_ANTES_OFFSET", agora.isoformat()[-6:])
        emitir("ESCRITOR_DEPOIS_OFFSET_AGORA", local_agora.isoformat()[-6:])

        cache_exportada = {}

        def exportada_no_dia(empresa_pk, dia):
            if dia is None:
                return False
            # Converte dia em competência pela autoridade, sem copiar o corte:
            # app/ponto/janelas.py:60.
            _, fim = janela_atual(dia, empresas[empresa_pk])
            chave = (empresa_pk, fim.month, fim.year)
            if chave not in cache_exportada:
                # Não consulta holerite nem replica vigência/exportação:
                # app/folha/export.py:580, app/folha/export.py:588.
                cache_exportada[chave] = bool(competencia_entregue(*chave))
            return cache_exportada[chave]

        def blocos(queryset):
            # Paginação por PK; não abre cursor servidor via iterator().
            ultimo = None
            while True:
                pagina = queryset.order_by("pk")
                if ultimo is not None:
                    pagina = pagina.filter(pk__gt=ultimo)
                pagina = list(pagina[:200])
                if not pagina:
                    return
                yield pagina
                ultimo = pagina[-1].pk

        def referencia_batida(contexto):
            if not isinstance(contexto, dict):
                return None
            ub = contexto.get("ultima_batida")
            if not isinstance(ub, dict):
                return None
            bruto = ub.get("id")
            if isinstance(bruto, bool):
                return None
            if isinstance(bruto, int):
                return bruto
            if isinstance(bruto, str) and re.fullmatch(r"[+-]?[0-9]+", bruto):
                return int(bruto)
            return None

        metricas = (
            "CANDIDATOS",
            "SEM_CHAMADO",
            "SEM_REFERENCIA_EXISTENTE",
            "U",
            "MEDIDOS",
            "DIVERGENTES",
            "DIVERGENTES_EXPORTADAS",
            "DIVERGENTES_EXPORTACAO_INDETERMINADA",
        )

        def varrer(escopo, queryset, por_empresa, disputas=False):
            amostrados = set()
            for pagina in blocos(queryset):
                preparados = []
                for item in pagina:
                    c = None
                    try:
                        c = item.chamado if disputas else item
                        empresa_pk = (
                            c.colaborador.empresa_id
                            if c is not None
                            else item.colaborador.empresa_id
                        )
                        cont = por_empresa[empresa_pk]
                        cont["CANDIDATOS"] += 1
                        if c is None:
                            cont["SEM_CHAMADO"] += 1
                            continue
                        bid = referencia_batida(c.contexto_json)
                        if bid is None:
                            cont["SEM_REFERENCIA_EXISTENTE"] += 1
                            continue
                        preparados.append((item, c, empresa_pk, bid))
                    except BaseException as exc:
                        erro(escopo + "_preparar", exc, getattr(c, "pk", None))

                batidas = {
                    b.pk: b
                    for b in Batida.objects.using(alias_leitura)
                    .filter(pk__in={r[3] for r in preparados})
                    .only("pk", "timestamp")
                }

                for item, c, empresa_pk, bid in preparados:
                    cont = por_empresa[empresa_pk]
                    batida = batidas.get(bid)
                    if batida is None:
                        cont["SEM_REFERENCIA_EXISTENTE"] += 1
                        continue
                    cont["U"] += 1
                    try:
                        # Não altera o objeto original nem chama save().
                        c2 = copy(c)
                        c2.contexto_json = deepcopy(c.contexto_json)
                        c2.contexto_json["ultima_batida"]["timestamp"] = (
                            timezone.localtime(batida.timestamp).isoformat()
                        )
                        # Leitor REAL, sem reproduzir qualquer degrau:
                        # app/chamados/juizes.py:171.
                        dia_hoje = dias_do_fato([c])[c.pk]
                        dia_novo = dias_do_fato([c2])[c.pk]
                        cont["MEDIDOS"] += 1
                        if dia_hoje == dia_novo:
                            continue
                        cont["DIVERGENTES"] += 1
                        try:
                            exp_hoje = exportada_no_dia(empresa_pk, dia_hoje)
                            exp_novo = exportada_no_dia(empresa_pk, dia_novo)
                        except BaseException:
                            cont["DIVERGENTES_EXPORTACAO_INDETERMINADA"] += 1
                            raise
                        exportada = exp_hoje or exp_novo
                        cont["DIVERGENTES_EXPORTADAS"] += int(exportada)
                        if c.pk not in amostrados and len(amostrados) < 20:
                            amostrados.add(c.pk)
                            emitir(
                                escopo + "_DIVERGENTE",
                                (
                                    "empresa_pk={} chamado_pk={} colaborador_pk={} "
                                    "disputa_pk={} dia_hoje={} dia_novo={} exportada={}"
                                ).format(
                                    empresa_pk, c.pk, c.colaborador_id,
                                    item.pk if disputas else "-",
                                    dia_hoje, dia_novo,
                                    "s" if exportada else "n",
                                ),
                            )
                    except BaseException as exc:
                        erro(escopo + "_medir", exc, c.pk)

        chamados = (
            ChamadoColaborador.objects.using(alias_leitura)
            .exclude(status_local__in=TERMINAIS)
            .select_related("colaborador")
            .only(
                "pk", "contexto_json", "celula_id", "colaborador_id",
                "colaborador__empresa_id",
            )
        )
        # Aberta equivale a fechada_em is None:
        # app/chamados/models.py:1870, app/chamados/models.py:1871.
        # O leitor recebe d.chamado; não recebe d.contexto:
        # app/chamados/models.py:1792, app/chamados/models.py:1796;
        # app/chamados/juizes.py:226.
        disputas = (
            DisputaSupervisao.objects.using(alias_leitura)
            .filter(fechada_em__isnull=True)
            .select_related("colaborador", "chamado__colaborador")
            .only(
                "pk", "colaborador__empresa_id", "chamado_id",
                "chamado__pk", "chamado__contexto_json",
                "chamado__celula_id", "chamado__colaborador_id",
                "chamado__colaborador__empresa_id",
            )
        )

        for escopo, queryset, eh_disputa in (
            ("M1_CHAMADOS", chamados, False),
            ("M1_DISPUTAS", disputas, True),
        ):
            contagens = defaultdict(Counter)
            try:
                varrer(escopo, queryset, contagens, disputas=eh_disputa)
            except BaseException as exc:
                erro(escopo + "_varredura", exc)
            totais = Counter()
            for empresa_pk in sorted(set(empresas) | set(contagens)):
                cont = contagens[empresa_pk]
                for chave in metricas:
                    emitir(
                        "{}_EMPRESA_{}_{}".format(escopo, empresa_pk, chave),
                        cont[chave],
                    )
                    totais[chave] += cont[chave]
            for chave in metricas:
                emitir("{}_TOTAL_{}".format(escopo, chave), totais[chave])

        # Janela real: agora - timedelta(days=DIAS_PARA_TRAS), inclusiva
        # nas duas pontas; não são apenas três datas civis:
        # app/ponto/management/commands/marcar_foto_ausente_retro.py:63,
        # app/ponto/management/commands/marcar_foto_ausente_retro.py:65,
        # app/ponto/management/commands/marcar_foto_ausente_retro.py:67.
        desde_local = timezone.localtime(
            agora - timedelta(days=DIAS_PARA_TRAS)
        )
        dias = []
        dia = desde_local.date()
        while dia <= local_agora.date():
            dias.append(dia)
            dia += timedelta(days=1)

        emitir("M2_HOJE_LOCAL", local_agora.date())
        emitir("M2_DIAS_PARA_TRAS", DIAS_PARA_TRAS)
        emitir("M2_INICIO_INCLUSIVO", desde_local.isoformat())
        emitir("M2_FIM_INCLUSIVO", local_agora.isoformat())
        emitir("M2_PRIMEIRO_DIA_LOCAL", dias[0])
        emitir("M2_ULTIMO_DIA_LOCAL", dias[-1])
        emitir("M2_DATAS_LOCAIS", len(dias))

        totais_dias = Counter()
        total_classificados = 0
        total_exportados = 0
        for empresa_pk, empresa in sorted(empresas.items()):
            prefixo = "M2_EMPRESA_{}".format(empresa_pk)
            try:
                # Início vem da autoridade de Empresa, sem literal de corte:
                # app/colaboradores/models.py:46, app/ponto/janelas.py:60.
                ini, fim = janela_atual(local_agora.date(), empresa)
                ant_ini, ant_fim = janela_atual(
                    ini - timedelta(days=1), empresa
                )
                emitir(prefixo + "_COMPETENCIA_ATUAL", "{}_a_{}".format(ini, fim))
                emitir(
                    prefixo + "_COMPETENCIA_ANTERIOR",
                    "{}_a_{}".format(ant_ini, ant_fim),
                )
                # Datas de execução dentro da competência atual.
                # Consequência da janela retroativa inclusiva, fonte acima.
                emitir(prefixo + "_PRIMEIRA_DATA_TOCA_ANTERIOR", ini)
                emitir(
                    prefixo + "_PRIMEIRA_DATA_DEIXA_DE_TOCAR_ANTERIOR",
                    ini + timedelta(days=DIAS_PARA_TRAS),
                )
                emitir(
                    prefixo + "_PROXIMA_VIRADA_TOCA_ANTERIOR",
                    fim + timedelta(days=1),
                )
                emitir(
                    prefixo + "_HOJE_TOCA_ANTERIOR",
                    int(desde_local.date() <= ant_fim
                        and local_agora.date() >= ant_ini),
                )
            except BaseException as exc:
                erro(prefixo + "_competencia", exc)

            classificados = exportados = 0
            for dia in dias:
                try:
                    exp = exportada_no_dia(empresa_pk, dia)
                    emitir(
                        prefixo + "_DIA",
                        "{} exportada={}".format(dia, "s" if exp else "n"),
                    )
                    classificados += 1
                    exportados += int(exp)
                    totais_dias[dia] += int(exp)
                    total_classificados += 1
                    total_exportados += int(exp)
                except BaseException as exc:
                    erro(prefixo + "_dia_" + dia.isoformat(), exc)
            emitir(prefixo + "_DIAS_CLASSIFICADOS", classificados)
            emitir(prefixo + "_DIAS_EXPORTADOS", exportados)

        for dia in dias:
            emitir(
                "M2_TOTAL_EMPRESAS_EXPORTADAS_NO_DIA",
                "{} quantidade={}".format(dia, totais_dias[dia]),
            )
        emitir("M2_TOTAL_EMPRESA_DIAS_CLASSIFICADOS", total_classificados)
        emitir("M2_TOTAL_EMPRESA_DIAS_EXPORTADOS", total_exportados)

except BaseException as exc:
    erro("sonda", exc)
finally:
    try:
        if logging_anterior is not None:
            logging.disable(logging_anterior)
    except BaseException as exc:
        erro("restaurar_logging", exc)
    emitir("ERRO", erros)
    emitir("FIM", "ok={}".format(int(erros == 0)))
```

## 15/11 07:12 · sessao 1454451ab25f · - · - · origem=Ronald (tmux)

- **Terminal:** importar `TERMINAIS` de `chamados.catalogo.motor` e usar `.exclude(status_local__in=TERMINAIS)`. Definição: `ENCERRADOS = ('resolvido', 'fechado')`; `TERMINAIS = ENCERRADOS + ('cancelado', 'superado')` em `app/chamados/catalogo/motor.py:30` e `app/chamados/catalogo/motor.py:31`. `registrado` é não terminal, conforme `VIVOS` em `app/chamados/catalogo/motor.py:24`.

- **Disputa aberta:** `.filter(fechada_em__isnull=True)`, equivalente à propriedade `DisputaSupervisao.aberta`, que retorna `self.fechada_em is None` em `app/chamados/models.py:1870`. Campo em `app/chamados/models.py:1804`.

- **Ligação ao chamado:** `DisputaSupervisao.chamado` é FK nullable, com reverso `disputa_origem`, em `app/chamados/models.py:1796`. Para universo de disputas, passar `d.chamado` à função real e alterar somente a cópia de `chamado.contexto_json`; `d.contexto` é outro campo, em `app/chamados/models.py:1792`. Por ser FK, a contagem de disputas precisa preservar a multiplicidade de disputas por chamado; convém reportar `SEM_CHAMADO`, já que a FK admite nulo (`app/chamados/models.py:1796`).

- **Leitor exigido:** `dias_do_fato(chamados)` em `app/chamados/juizes.py:171`. Apenas monta dicionários e consulta; chamadas de ORM nas linhas `app/chamados/juizes.py:200` e `app/chamados/juizes.py:210`. O degrau de disputa consulta todas as disputas relacionadas, sem recorte por fechamento, e ignora perguntas `conversa` (`app/chamados/juizes.py:210`, `app/chamados/juizes.py:213`). Não aplicar pré-filtro adicional ao fallback.

- **Último degrau:** lote chama `_dia_do_contexto_fraco(c.contexto_json)` em `app/chamados/juizes.py:226`; singular em `app/chamados/juizes.py:138`. O helper lê `ultima_batida.timestamp` em `app/chamados/juizes.py:159` e executa `datetime.fromisoformat(bruto).date()` em `app/chamados/juizes.py:166`, preservando o dia do offset gravado.

- **Atenção à execução real:** embora a docstring diga “mesmo juiz”, o singular consulta a chave de data do catálogo (`app/chamados/juizes.py:113`), enquanto o lote lê diretamente `data_turno` (`app/chamados/juizes.py:187`). Para satisfazer o pedido, chamar literalmente `dias_do_fato([c])[c.pk]` e `dias_do_fato([c2])[c.pk]`; não substituir pelo singular nem reproduzir a escada.

## 15/11 06:25 · sessao 7c8db30e6497 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura, diff, RED/GREEN e censo registrados na [entrega c17](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1).

- [129 testes sem banco passaram](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1032). [nao verificado]
- [2/2 mutantes mortos por asserção](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:327).
- [Ruff e py_compile passaram, sem gravar bytecode](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1038).

[Suite com banco reservada ao Code; sem commit](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:128). [nao verificado]

## 15/11 06:13 · sessao 09489d17e7f7 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

1. **Existe, mas não concentra toda a decisão de cobertura.** O registro aponta para `marcos_cobertos_por_justificativa`, definida na árvore; a fonte intermediária também decide cobertura sem passar por ela. (`app/core/juizes.py:265`; `app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:143`; `app/ponto/services/efeito_ausencia.py:187`.)

| Sítio de produção | Decisão / relação com a função registrada |
|---|---|
| `app/ponto/turnos.py:2668` | Define quais tipos nomeiam marco: `esqueci_saida→hf`, `esqueci_entrada→hi`. |
| `app/ponto/turnos.py:2686` | `ancora_nao_cobre` decide ausência de âncora, papel errado e ponta já fechada. É chamada pela função registrada em `app/ponto/turnos.py:2723` e separadamente pela fonte em `app/ponto/services/efeito_ausencia.py:204`. |
| `app/ponto/turnos.py:2720` | Função registrada: devolve dia inteiro (`None`), nenhum marco (`frozenset()`) ou o marco nomeado (`frozenset({alvo})`). |
| `app/ponto/services/efeito_ausencia.py:121` | Define o universo: justificativas aprovadas do colaborador, `data_turno` não nula e dentro do intervalo. |
| `app/ponto/services/efeito_ausencia.py:141` | Cobertura inteira anterior prevalece; tipo sem marco recebe `cobre=None` diretamente, **sem chamar a registrada**. |
| `app/ponto/services/efeito_ausencia.py:148` | Motivo conserva o inteiro; demais aprovações do dia têm os conjuntos unidos em `app/ponto/services/efeito_ausencia.py:153`. |
| `app/ponto/services/efeito_ausencia.py:185` | `_cobertura_de_uma`: sem vínculo/marcos ou sem entrada/saída previstas, conserva inteiro com `celula_sem_marco`, **sem chamar a registrada**. |
| `app/ponto/services/efeito_ausencia.py:195` | Seleciona o turno da âncora e exige o mesmo dia; resolve papéis, consulta `ancora_nao_cobre` e retorna inteiro quando há motivo. Só o restante chama a registrada, por parâmetro, em `app/ponto/services/efeito_ausencia.py:207`. |
| `app/escala/utils.py:1208` | A grade aplica precedência da ausência; aplica justificativa somente em dia de trabalho, em `app/escala/utils.py:1252` e `app/escala/utils.py:1261`. Lê a fonte intermediária em `app/escala/utils.py:1197`. |
| `app/escala/utils.py:1270` | Converte `hi/hf` em extremos da cadeia e dispensa somente células faltantes; decide `justificado` pela ausência de `missing` em `app/escala/utils.py:1283`. Usa o resultado da fonte, sem repetir o predicado de âncora. |
| `app/ponto/services/cartorio.py:802` | Lê `dia['justificado']` e dispensa lâmpadas restantes; não chama a registrada. A escrita ocorre em `app/ponto/services/cartorio.py:808`. |
| `app/escala/services/leitor_celula.py:85` | Reconstrói `justificado` pelas lâmpadas dispensadas e ausência de apagadas; transporta para o dia em `app/escala/services/leitor_celula.py:472`. Não chama a registrada. |

Também há decisões financeiras próprias:

| Sítio | Predicado efetivo |
|---|---|
| `app/ponto/services/efeito_ausencia.py:170` | Pagamento = `max(0, previsto − trabalhado)`, depois de montar a cobertura. |
| `app/ponto/services/efeito_ausencia.py:55` | Todos os dias presentes no mapa entram em `justificadas`; o mapa financeiro depende de `trabalhado != 0` e de haver apuração, em `app/ponto/services/efeito_ausencia.py:57`. |
| `app/ponto/motor_calculo_v2.py:2581` | O motor usa existência de período para `datas_trabalho`; separa abono inteiro pela ausência da chave em `minutos_justificados`, em `app/ponto/motor_calculo_v2.py:2629`. Não consulta o juiz de marcos. |
| `app/ponto/services/dia_pago.py:395` | A projeção financeira trata `sem_ata` como desconhecido, ausência pelo saldo previsto e justificativa pelo crédito recebido, em `app/ponto/services/dia_pago.py:401`. |
| `app/ponto/services/dia_pago.py:717` | Competência entregue conserva a contagem gravada; fora dela, conta dia inteiro por `tipo_dia='ausencia'` ou realizado zero, em `app/ponto/services/dia_pago.py:737`. |

**Predicado e universo:** a função pura recebe tipo, papel da âncora, papéis e marcos; a fonte acrescenta aprovação, colaborador, datas, existência da célula e composição entre justificativas. Portanto, função registrada e fonte completa não têm o mesmo universo. (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:121`; `app/ponto/services/efeito_ausencia.py:141`; `app/ponto/services/efeito_ausencia.py:185`.)

Pelo delta fornecido, esses corpos não foram alterados: houve registro e troca de docstring. A comparação histórica independente com a autoridade anterior a esse registro está **[nao verificado]**.

2. **Sim, a frase existe com o nome exato `marcos_cobertos_por_justificativa`.** Consta no corte geral e no registro específico, tanto em Markdown quanto em JSON. (`app/docs/CORTES.md:341`; `app/docs/CORTES.md:342`; `app/docs/CORTES.json:1097`; `app/docs/CORTES.json:1104`.)

3. **Os dois arquivos citados leem a cadeia, mas de formas diferentes.** `efeito_ausencia.py` importa a registrada, passa-a para `_cobertura_de_uma` e a executa apenas no ramo restante; `escala/utils.py` chama `periodos_justificados`, portanto a leitura é indireta e sujeita aos desvios descritos acima. (`app/ponto/services/efeito_ausencia.py:118`; `app/ponto/services/efeito_ausencia.py:146`; `app/ponto/services/efeito_ausencia.py:207`; `app/escala/utils.py:1197`.)

Outros leitores não enumerados no comentário do registro:

| Leitor | Caminho |
|---|---|
| `efeitos_da_ausencia` | Chama a fonte em `app/ponto/services/efeito_ausencia.py:54`. |
| Fase financeira da grade | Chama a fonte com o realizado do julgamento em `app/escala/utils.py:1297`. |
| Grade da folha | Chama a fonte em `app/folha/export.py:339`. |
| Grade do espelho | Chama a fonte em `app/ponto/services/espelho.py:520`. |
| Contador de dias abonados | Chama a fonte em `app/ponto/services/dia_pago.py:722`. |
| Fechamento, PDF e esmeril | Leem por `efeitos_da_ausencia`: `app/ponto/services/fechamento.py:665`; `app/relatorios/pdf_espelho.py:332`; `app/ponto/services/esmeril_espelho.py:161`. |
| Comandos de comparação | Leem por `efeitos_da_ausencia`: `app/relatorios/management/commands/diff_extrato_parcial.py:41`; `app/ponto/management/commands/diff_espelho_alimentacao.py:117`. |

4. **Restou “fallback” em comentário identificado nominalmente com a obra.** O bloco começa com `JUSTIFICATIVA-PAGA-O-PERIODO (verificador v2 A3, 14/11)` e contém “o fallback dizia 0%% onde o gravado tem 630”. Descreve o caminho anterior, mas a palavra permanece. (`app/folha/export.py:1105`; `app/folha/export.py:1108`.)

A docstring alvo do delta já usa `de minutos_previstos_do_dia`. (`app/escala/services/leitor_celula.py:370`.)

Persistem ainda docstrings com “a cura e do motor e espera `!`; ao curar, esta assercao TROCA”, nos quatro pontos abaixo; a entrega c9 registra que os casos foram deixados como caracterização com lei/pergunta na docstring. (`app/ponto/tests/test_justificativa_c8_sem_banco.py:126`; `app/ponto/tests/test_justificativa_c8_sem_banco.py:269`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:325`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:653`; `app/ponto/tests/c9_entrega.txt:98`.)

A introdução histórica dessas ocorrências pelo **diff integral**, excluindo tudo que já existia antes da obra, está **[nao verificado]**.

5. **Escritores e leitores dos valores envolvidos no delta e no contrato que ele documenta:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Pergunta e endereço em `JUIZES['ausencia/ferias']` | Declaração em `app/core/juizes.py:265`. | Selo de existência em `app/ponto/tests/test_contract_juiz_ausencia.py:39`; gerador em `app/core/management/commands/gerar_diagrama.py:729` e `app/core/management/commands/gerar_diagrama.py:781`. |
| Retornos `None` / `frozenset` | Registrada em `app/ponto/turnos.py:2722`, `app/ponto/turnos.py:2724`, `app/ponto/turnos.py:2726` e `app/ponto/turnos.py:2727`; a fonte também produz cobertura inteira em `app/ponto/services/efeito_ausencia.py:144` e `app/ponto/services/efeito_ausencia.py:149`. | Fonte recebe e compõe em `app/ponto/services/efeito_ausencia.py:151` e `app/ponto/services/efeito_ausencia.py:153`; grade lê em `app/escala/utils.py:1252` e `app/escala/utils.py:1270`. |
| `celulas={(colaborador_id,data):celula}` | Carregador em `app/escala/alimentacao.py:73`; conversão do mapa por data em `app/escala/services/leitor_celula.py:407` e `app/escala/services/leitor_celula.py:442`. | Fonte passa ao helper em `app/ponto/services/efeito_ausencia.py:159`; grade passa em `app/escala/services/leitor_celula.py:443`; helper encaminha ao juiz do previsto em `app/escala/services/leitor_celula.py:375`. |
| `ata['minutos_previstos']` | Cartório monta em `app/ponto/services/cartorio.py:739` e entrega à lavra em `app/ponto/services/cartorio.py:1853`; porta atribui a ata em `app/ponto/portas/celula.py:1064` e `app/ponto/portas/celula.py:1091`. | Helper lê em `app/escala/services/leitor_celula.py:372`. |
| Previsto devolvido pelo helper | Usa ata ou `minutos_previstos_do_dia`, com zero para célula sem trabalho no segundo caminho, em `app/escala/services/leitor_celula.py:372` e `app/escala/services/leitor_celula.py:375`. | Saldo pago em `app/ponto/services/efeito_ausencia.py:170`; saída da grade em `app/escala/services/leitor_celula.py:453`. |

O registro é consumido como catálogo; o contador `fora_de_autoridade` apenas conta `PENDENTES`, e seu zero não demonstra unicidade de chamadas. (`app/core/management/commands/gerar_diagrama.py:729`; `app/core/juizes.py:1486`.)

## 15/11 06:12 · sessao 8734c0e0a169 · - · - · origem=Ronald (tmux)

Q5 — leitura estrita do delta fornecido:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `JUIZES['ausencia/ferias']['que marcos do turno a justificativa aprovada cobre?']` e endereço da função | Declaração em `app/core/juizes.py:265`; família em `app/core/juizes.py:251`. | Selo confere existência por AST em `app/ponto/tests/test_contract_juiz_ausencia.py:39`; gerador lê referências em `app/core/management/commands/gerar_diagrama.py:729`, `:770`, `:781`; diagrama agrega arquivos e contagens em `:344`. |
| `None` / `frozenset` descritos no comentário | Função produz `None` em `app/ponto/turnos.py:2722`, `:2724`, conjunto vazio em `:2726`, conjunto do marco em `:2727`. | Fonte recebe retorno por função injetada em `app/ponto/services/efeito_ausencia.py:146`; combina cobertura em `:151`, `:153`. |
| Contrato `celulas={(colaborador_id,data):celula}` nomeado pela docstring | Carregador produz mapa em `app/escala/alimentacao.py:73`; efeito o carrega em `app/ponto/services/efeito_ausencia.py:131`. Grade converte seu mapa `{data:celula}` ao contrato de pares em `app/escala/services/leitor_celula.py:407`, `:442`. | Efeito passa mapa ao helper em `app/ponto/services/efeito_ausencia.py:159`; grade passa em `app/escala/services/leitor_celula.py:443`; helper encaminha à autoridade em `:375`; autoridade usa vínculo, dia de trabalho, marcos e pausas em `app/escala/utils.py:2307`, `:2313`, `:2316`, `:2338`. |
| `ata['minutos_previstos']`, lido pelo helper cuja docstring mudou | `ata_do_dia` monta valor em `app/ponto/services/cartorio.py:739`; entrega ata à lavra em `:1851`, `:1853`; método delega em `app/escala/models.py:1826`; porta atribui em `app/ponto/portas/celula.py:1064`, `:1091` e salva em `:1066`, `:1098`. | Helper lê em `app/escala/services/leitor_celula.py:372`; resultado vai à saída da grade em `:453` e ao saldo pago em `app/ponto/services/efeito_ausencia.py:170`. |

O comentário do registro não cria despacho: API usa apenas nomes de famílias em `app/api/views_mensageria.py:1448`; contador `fora_de_autoridade` conta `PENDENTES`, não chamadas nem unicidade, em `app/core/juizes.py:1486`.

O delta fornecido altera registro e documentação; os campos de dados acima são o contexto executável referido pela docstring, sem alteração executável no helper mostrado em `app/escala/services/leitor_celula.py:372`.

## 15/11 06:11 · sessao e30ac8989913 · - · - · origem=Ronald (tmux)

Q4 tem ocorrência remanescente identificada como parte da obra: o comentário começa com `JUSTIFICATIVA-PAGA-O-PERIODO (verificador v2 A3, 14/11)` em `app/folha/export.py:1105` e contém **“o fallback dizia 0%% onde o gravado tem 630”** em `app/folha/export.py:1108`. Descreve o caminho anterior; a palavra continua no comentário. A introdução histórica pelo diff integral é [nao verificado].

O alvo do delta foi corrigido: a docstring agora diz `de minutos_previstos_do_dia` em `app/escala/services/leitor_celula.py:370`. A docstring A3 atual descreve “previsto injetado”, sem `fallback`, em `app/ponto/tests/test_justificativa_soma_sem_banco.py:425-426`.

Persistem docstrings da obra com **“a cura e do motor e espera !; ao curar, esta assercao TROCA”** em:

- `app/ponto/tests/test_justificativa_c8_sem_banco.py:126-127`;
- `app/ponto/tests/test_justificativa_c8_sem_banco.py:269-270`;
- `app/ponto/tests/test_justificativa_paga_o_periodo.py:325-326`;
- `app/ponto/tests/test_justificativa_paga_o_periodo.py:653-654`.

A entrega c9 declara a alteração desses casos para caracterização e registra que deixaram “lei/pergunta na docstring”, em `app/ponto/tests/c9_entrega.txt:98-100`. O dado fornecido já assinala que o `!` fora concedido; não inferi uma aprovação nova.

Existem outras palavras `fallback`/`band-aid` nos arquivos tocados, mas atribuí-las à obra inteira, sem comparação com a base, é [nao verificado]. Não há base suficiente para afirmar **“todos os comentários introduzidos pela obra foram saneados”** [nao verificado].

## 15/11 06:11 · sessao 5ca348ddc426 · - · - · origem=Ronald (tmux)

A função registrada existe em `app/ponto/turnos.py:2703`; o retorno efetivo é `None`, `frozenset()` ou `frozenset({alvo})` em `app/ponto/turnos.py:2720-2727`. Ela chama `ancora_nao_cobre` (`app/ponto/turnos.py:2723`), cujo predicado fica em `app/ponto/turnos.py:2686-2700`.

Não é o único sítio que determina cobertura na composição de produção:

| Sítio | Decisão e relação com juiz |
|---|---|
| `app/ponto/services/efeito_ausencia.py:121-124` | Define universo ORM: justificativas aprovadas do colaborador com `data_turno` no intervalo e não nula. |
| `app/ponto/services/efeito_ausencia.py:141-145` | Dia previamente coberto inteiro prevalece; tipo que não nomeia marco recebe `cobre=None` diretamente, sem chamada do juiz. |
| `app/ponto/services/efeito_ausencia.py:148-153` | Motivo conserva dia inteiro diretamente; demais coberturas do mesmo dia são unidas. |
| `app/ponto/services/efeito_ausencia.py:185-188` | Sem vínculo/marcos, ou sem entrada/saída prevista, retorna cobertura inteira e `celula_sem_marco`, sem chamada do juiz. Este caso não existe na assinatura do juiz registrado. |
| `app/ponto/services/efeito_ausencia.py:195-207` | Define turno da âncora, exige mesmo dia, resolve papéis; chama `ancora_nao_cobre` e conserva inteiro antes do juiz quando há motivo. Só o ramo restante chama o juiz, por parâmetro `juiz`, na linha 207. |
| `app/escala/utils.py:1197-1199` | Lê `periodos_justificados`, portanto juiz indiretamente e apenas nos ramos anteriores que chegam nele. |
| `app/escala/utils.py:1252-1260` | Aplica cobertura inteira apenas a `tipo_dia='trabalho'`; apaga marcos faltantes e órfãs. |
| `app/escala/utils.py:1261-1284` | Aplica cobertura parcial aos marcos extremos da cadeia e determina `justificado` se não restar `missing`; lê `cobre`, sem refazer tipo/âncora do juiz. |
| `app/ponto/services/cartorio.py:800-810` | Lê `dia['justificado']` e dispensa todas as lâmpadas ainda não acesas; não consulta o juiz. |
| `app/escala/services/leitor_celula.py:83-86` | Reconstrói `justificado` de lâmpada dispensada por justificativa e ausência de lâmpada apagada; lê ata, não o juiz. |

O universo do juiz puro é tipo/papel da âncora/papéis/marcos, sem status, colaborador, data ou precedência (`app/ponto/turnos.py:2703`). A fonte acrescenta essas restrições e compõe aprovações (`app/ponto/services/efeito_ausencia.py:121-153`). A aplicação da grade restringe novamente a dias de trabalho e dá precedência à ausência (`app/escala/utils.py:1208`, `app/escala/utils.py:1252`, `app/escala/utils.py:1261`). Portanto, juiz puro, fonte por dia e efeito da grade não têm o mesmo universo (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:121-153`; `app/escala/utils.py:1208-1261`). Comparação histórica com implementação anterior ao registro: [nao verificado].

O comentário do registro cita dois arquivos, mas o leitor direto do juiz é somente `efeito_ausencia.py`, com importação em `app/ponto/services/efeito_ausencia.py:118-119`, passagem em `app/ponto/services/efeito_ausencia.py:146-147` e chamada em `app/ponto/services/efeito_ausencia.py:207`. `escala/utils.py` lê a fonte intermediária em `app/escala/utils.py:1197-1199`.

Outros leitores da fonte intermediária, ausentes do comentário do registro:

- `efeitos_da_ausencia`: `app/ponto/services/efeito_ausencia.py:54`.
- Fase financeira do julgamento: `app/escala/utils.py:1295-1302`.
- Grade do fechamento: `app/folha/export.py:336-341`.
- Grade do espelho: `app/ponto/services/espelho.py:519-522`.
- Contagem de dias com abono: `app/ponto/services/dia_pago.py:720-723`.

Leitores financeiros por `efeitos_da_ausencia`: fechamento em `app/ponto/services/fechamento.py:664-667`, PDF em `app/relatorios/pdf_espelho.py:332-336`; crédito chega aos motores em `app/ponto/services/fechamento.py:755`, `app/relatorios/pdf_espelho.py:407` e `app/ponto/services/espelho.py:1141`.

Separação necessária: cobertura de marcos e pagamento não usam o mesmo predicado. A fonte paga `max(0, previsto - trabalhado)` após montar cobertura (`app/ponto/services/efeito_ausencia.py:158-170`), inclui todo dia do mapa em `justificadas` (`app/ponto/services/efeito_ausencia.py:54-58`), o motor distingue dias sem período e presença da chave do crédito (`app/ponto/motor_calculo_v2.py:2581-2586`, `app/ponto/motor_calculo_v2.py:2611-2634`), e `dias_com_abono` conta por `tipo_dia='ausencia'` ou realizado zero (`app/ponto/services/dia_pago.py:729-738`).

## 15/11 06:02 · sessao 004e56fcc326 · CORTES-RECEBIDO-FORA-DA-PAUSA · censo · origem=Code

```text
O148-EDITAR | B | app/docs/BACKLOG.md:817; app/docs/BACKLOG.md:1459 | Registrada, não construir agora; complemento O148/O152. Edição por versão ainda aguarda corte de lei; frase não entregue.
IA-RBAC-INCLUDE-DO-COPILOTO | C | app/docs/RELATO.md:3603; app/docs/RELATO.md:3606; app/docs/RELATO.md:3608; app/docs/CORTES.md:351 / 329e5eca7, 4e877efa1, 44ae9d5f | Pouso integral relatado, instrumento e smoke OK; falta linha NO AR/FECHADA no BACKLOG/TICKETS. Casa documental: IA-PERGUNTAR-SEM-RBAC.
REJULGAR-COMP10-RESTO-DA-FROTA | B | app/docs/BACKLOG.md:819; app/docs/BACKLOG.md:820; app/docs/RELATO.md:3188; app/docs/RELATO.md:3476 | Atos aplicados e lista HE regenerada, 184→249. SOMBRA-X-PROD-COL988-124639 segue registrada; fechamento integral [nao verificado].
CONTRATO-20-TERMOMETRO-E-TOL | D | app/docs/CORTES.md:350; app/docs/RELATO.md:3548 / 9269eae07, 47ce5996d | Superado em parte por CONTRATO-20-TOL-DUAS-PONTAS: “(2.3) ... RETIRADO”; “TOL segue só pela saída”. (2.2) pousou; frase original inteira não.
BO-ADMIN-2710 | B | app/docs/BACKLOG.md:830; app/docs/RELATO.md:3537; app/docs/RELATO.md:160; app/docs/RELATO.md:165 / 57877acf5 | Linha ainda “medindo”; revisão mantém 8.2/8.3 não fechados. 57877acf5 instrumenta a GRADE, não comprova fechamento dos oito achados.
SEM-SELO-CAUSA-ESPELHO-09 | D | app/docs/CORTES.md:363; app/docs/RELATO.md:3516; app/docs/BACKLOG.md:1134 / 2359b756d | RESPOSTAS-TOPO-2710-1720 substitui a condição: só causa/hash nos 5 da 10; 33 da 09 intactos, col417/931→O210. Produto pousou; original não integral.
BO-[nome]-FECHA | B | app/docs/BACKLOG.md:833; app/docs/BACKLOG.md:832; app/docs/BACKLOG.md:1928; app/docs/RELATO.md:160 / 475318fa9 | Pousos parciais; 475318fa9 entrega D. TENTATIVA-DO-ADMIN-VIRA-COBRANCA registrada; justificativa/abono em obra e 8.3 não fechado. Pacote não encerrado.
BO-[nome]-NOMES | B | app/docs/BACKLOG.md:839; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:847; app/docs/BACKLOG.md:1928 | Nomes incorporados às obras existentes, sem novo ID. Encaminhamento aplicado; justificativa/abono permanece em obra. Não equivale a fechamento das obras.
BO-ADMIN-2710-LEIS | B | app/docs/TICKETS.md:156; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:1928; app/docs/BACKLOG.md:770 / 97bcb01c, b3bf529e | Ausência parcial e juiz do par pousaram; JUSTIFICATIVA-PAGA-O-PERIODO voltou à ordem viva, com complemento EM CURSO. Entrega apenas parcial.
JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES | D | app/docs/RELATO.md:3066; app/docs/RELATO.md:3063; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:1932 / b3bf529e, 77c6193c | JUIZ-DO-PAR-RUBRICA-E-RESIDUO substitui “não pousa”: “obra fecha com o resíduo declarado”, corrigido para 573. Cura pousou; resto em MOTOR-LE-O-TURNO.
COBERTURA-PARCIAL-PERIODO | D | app/docs/CORTES.md:381; app/docs/TICKETS.md:156; app/docs/BACKLOG.md:1928 / 97bcb01c; anexo 399565438 WIP, 973d645ce RED | COBERTURA-ABONO-DO-PERIODO substitui o pouso conjunto: “a (A) se SEPARA”. Ausência NO AR; justificativa/abono segue em construção. Original não integral.
FIO-EXPORTADA-X-O44 | A | app/docs/BACKLOG.md:847; app/docs/BACKLOG.md:1133; app/chamados/tests/test_fio_le_o_juiz.py:277; app/chamados/tests/test_fio_le_o_juiz.py:291 / 7c9e4c27 | NO AR documental, frase inteira: exportada igual, Validar sem reabrir/push; dono-do-dia registrado. Descrição “só leitura” do BACKLOG está desatualizada; prod atual [nao verificado].
REGISTRADO-NAO-E-VIVO-DESVIO | B | app/docs/BACKLOG.md:834; app/docs/BACKLOG.md:835; app/docs/BACKLOG.md:836; app/core/tests/test_tripwire_contagem_de_pendencia.py:137; app/docs/CORTES.md:380 / 475318fa9, 5302ac95d, 667c94081 | Pouso parcial: falta um leitor em SILENCIO-VENCIDO-VOLTA, INTERROMPIDA; “TODA pendência” não entregue. REGISTRADO-CARD-3524 atualizou o card aceito para 3524.
```

## 15/11 06:01 · sessao 8fb299e5c2f7 · - · - · origem=Ronald (tmux)

**C**, pela definição estrita: A exige linha NO AR/FECHADA no BACKLOG/TICKETS. Registre o pouso integral relatado e a falta dessa casa documental, sem negar a implementação. Evidência: `app/docs/RELATO.md:3603`, `app/docs/RELATO.md:3609`, `app/docs/CORTES.md:351`.

Estado sugerido: “Pouso integral relatado, com instrumento e smoke; sem casa de encerramento [nao verificado]. Deveria constar como IA-PERGUNTAR-SEM-RBAC no BACKLOG/TICKETS.” [nao verificado]

## 15/11 06:00 · sessao 3e6ba30ae74a · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Entrega em [cef_c5_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:1), com diff, comandos e saídas.

- Teste novo verde; controles rc=0 e seis mutantes rc=1, incluindo `sem_fechada`. [registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:48) [nao verificado]
- `py_compile` rc=0, com gravação de bytecode interceptada. [registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:113)
- Ruff indisponível; lint **[nao verificado]**. [tentativas](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:100)

## 15/11 06:00 · sessao 3599e782e15d · - · - · origem=Ronald (tmux)

Classificações sugeridas, com os pontos que impedem confundir pouso parcial com A: [nao verificado]

- **JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES — D.** A exigência “NAO pousa com o residuo” de `app/docs/CORTES.md:369` foi substituída por **JUIZ-DO-PAR-RUBRICA-E-RESIDUO**, que autoriza “a obra fecha com o residuo declarado” (`app/docs/RELATO.md:3066`), depois corrigido para 573 (`app/docs/RELATO.md:3063`). A cura na fonte pousou em **b3bf529e**, com prova **77c6193c**, mas há resíduo em **MOTOR-PASSA-O-TURNO-A-VOLTA** (`app/docs/BACKLOG.md:829`, `app/docs/BACKLOG.md:866`), incorporado ao **MOTOR-LE-O-TURNO** (`app/docs/BACKLOG.md:1932`). Código: `_match_marcos` lê papel em `app/escala/utils.py:585` e `app/escala/utils.py:613`. Estado ≤200: `Superado por RUBRICA-E-RESIDUO: aceitou resíduo; cura na fonte NO AR b3bf529e, 573 seguem MOTOR-LE-O-TURNO.`

- **COBERTURA-PARCIAL-PERIODO — D.** **COBERTURA-ABONO-DO-PERIODO** substitui o pouso conjunto: “a (A) se SEPARA”, ausência pousa, justificativa espera quem pague o período (`app/docs/CORTES.md:381`). Ausência parcial **NO AR 97bcb01c** (`app/docs/TICKETS.md:156`, `app/docs/RELATO.md:8272`); **JUSTIFICATIVA-PAGA-O-PERIODO** permanece viva, retomada após HJ-090 (`app/docs/BACKLOG.md:1928`, estado mais recente em `app/docs/RELATO.md:51`). Os commits do anexo **399565438** e **973d645ce** dizem WIP/RED, não encerramento [nao verificado quanto ao conteúdo integral dos commits]. Estado ≤200: `Superado por COBERTURA-ABONO-DO-PERIODO: ausência NO AR 97bcb01c; justificativa separada, ainda em construção.`

- **FIO-EXPORTADA-X-O44 — A**, com **7c9e4c27** documentado em `app/docs/BACKLOG.md:847` e prova de deploy em `app/docs/RELATO.md:3227`. O **1fcc61e0** do anexo era WIP: o relato registra “NAO POUSA” por falta de prova (`app/docs/RELATO.md:3480`). A regra pedida existe no código: exportada ou não, validar sem reabrir (`app/chamados/services/fio.py:638`, `app/chamados/services/fio.py:647`), selo de modal e POST em competência exportada (`app/chamados/tests/test_fio_le_o_juiz.py:276`, `app/chamados/tests/test_fio_le_o_juiz.py:291`). A obra própria solicitada foi registrada (`app/docs/BACKLOG.md:1133`). A descrição antiga de BACKLOG:847 ainda diz “exportada só leitura”, contradizendo o corte e código citados. Estado ≤200: `NO AR integral pela árvore: fio igual nas exportadas, Validar sem reabrir/push; JUIZ-DE-DONO-DO-DIA registrado. Estado atual de prod [nao verificado].`

- **REGISTRADO-NAO-E-VIVO-DESVIO — B**, se julgar literalmente “TODA pendencia pelo juiz”. Produto **NO AR 475318fa** (`app/docs/BACKLOG.md:833`, `app/docs/RELATO.md:3488`); instrumento **a8730557** pousado (`app/docs/BACKLOG.md:834`); dois leitores curados pelos commits do anexo **5302ac95d/667c94081**, mas um leitor ainda não migrou e permanece em **SILENCIO-VENCIDO-VOLTA**, interrompida (`app/docs/BACKLOG.md:835`, `app/docs/BACKLOG.md:836`). O selo confirma **SEM_JUIZ** e **TETO_SEM_JUIZ=1** (`app/core/tests/test_tripwire_contagem_de_pendencia.py:137`, `app/core/tests/test_tripwire_contagem_de_pendencia.py:146`). `registrado` segue VIVOS (`app/chamados/catalogo/motor.py:24`); fila usa COBRANCA (`app/chamados/services/painel.py:189`). Também houve substituição explícita do número por **REGISTRADO-CARD-3524**: “aceito 4165 -> 3524 [...] o 3687 era do commit anterior” (`app/docs/CORTES.md:380`); D é defensável se priorizar esse corte posterior, mas A integral não é. Estado ≤200: `Pouso parcial 475318fa; AST pousado a8730557; falta 1 leitor em SILENCIO-VENCIDO-VOLTA, INTERROMPIDA. Card aceito passou a 3524.`

_5543 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
