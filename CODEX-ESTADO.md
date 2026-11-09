# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 16:07** · publicado em **09/11/2026 16:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 16:07 · sessao 7e56db703d9c · - · - · origem=Ronald (tmux)

Fecho dos chamadores por AST, sem importar a aplicação:

| Leitor | Campos/significado | Chamadores |
|---|---|---|
| `app/chamados/juizes.py:1084 tranca_da_consulta` | Dia do turno pelo juiz, resultado da tranca e memo restrito à consulta; memória, sem persistência. | P: `app/chamados/services/pergunta_no_app.py:56`. T indireto: `app/chamados/tests/test_e3_v4_orm.py:108`. |
| `app/chamados/juizes.py:1450 alimentar_fila` | Fila, célula e verbos do painel; leitura indireta de status/silêncio/urgência. | P: `app/chamados/juizes.py:1495`; `app/chamados/services/tarefa_do_dia.py:409`. T: `app/chamados/tests/test_e3_portas_tarefa.py:119`. |
| `app/chamados/services/tarefa_do_dia.py:396 CargaDaTarefa.tarefa` | Perguntas, decisões, efeitos, passivo, lavra e tranca carregados; agora alimenta painel sem chamar C6. | P: `app/chamados/services/tarefa_do_dia.py:452`. T: `app/chamados/tests/test_e3_portas_tarefa.py:121`. |
| `app/chamados/services/tarefa_do_dia.py:418 _portas_da_tarefa` | Ações exigíveis, fila e verbos; publica rota/fila/verbos/fio. | P: `app/chamados/services/tarefa_do_dia.py:412`. T indireto: `app/chamados/tests/test_e3_portas_tarefa.py:121`. |
| `app/chamados/management/commands/diff_e3_sombra.py:42 candidata` | Projeta rota/fila/verbos/fio da tarefa. | P operacional: mesmo arquivo `:126`. T: `app/chamados/tests/test_e3_diff_sombra.py:135`. |
| `app/chamados/management/commands/diff_e3_sombra.py:47 projecoes` | Campos consumidos pelos três leitores C17. | P operacional: mesmo arquivo `:118`, `:127`. T: `app/chamados/tests/test_e3_diff_sombra.py:141`. |
| `app/chamados/management/commands/diff_e3_sombra.py:61 casos_do_par` | Fila, perguntas vivas, âncoras e razão da tarefa; classifica casos do instrumento. | P operacional: mesmo arquivo `:128`. T direto: `[nao verificado]`. |
| `app/chamados/management/commands/diff_e3_sombra.py:108 Command.comparar` | Compara C17 e tarefa por colaborador/dia; utiliza helpers anteriores. | P operacional: mesmo arquivo `:99`. T indireto: `app/chamados/tests/test_e3_diff_sombra.py:20`. |

`diff_e3_sombra` é instrumento declarado fora do pipeline; declaração não demonstra execução. (`app/config/crons.py:771`) Execução produtiva de todos os chamadores acima: **[nao verificado]**.

Correções indispensáveis do v1: `_estado_da_decisao` não existe no AST atual; o corpo correspondente é `_pela_decisao`, em `app/chamados/services/tarefa_do_dia.py:115`. O diff fornecido não demonstra renomeação desse corpo: **[nao verificado]** quanto à origem do erro do v1. A referência `data_conferida_por` em `app/chamados/services/validacao.py:666` é atribuição, não leitura; a guarda lê `data_conferida_em` em `:662`. `_resolv` existe em `app/chamados/services/becos.py:21`; `universo_da_linha` existe em `app/chamados/services/fio.py:1906`.

## 09/11 16:07 · sessao 5bf5272c2c2f · - · - · origem=Ronald (tmux)

A conferência AST manteve as 16 portas históricas e os seis nascimentos nas mesmas linhas do v1. Nenhum arquivo desses escritores é alterado pelo diff fornecido. A soma **22 é incompleta para a unidade declarada**: quatro gravadores adicionais de estado já constavam da lista do v1, mas ficaram fora da soma; o total de projeção é **20 alterações + 6 nascimentos = 26, diferença +4**, sem escritor criado pelo diff. (`app/chamados/services/materializacao.py:851`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/validacao.py:590`; `app/chamados/services/validacao.py:666`)

As quatro portas adicionais afetam ação: pendência do admin muda a explicação antes do encerramento; recusa mantém aplicação pendente ou terminal conforme a causa; conferência de data libera plantio bloqueado por divergência. (`app/chamados/services/explicador.py:84`; `app/chamados/services/materializacao.py:1083`; `app/chamados/services/materializacao.py:1193`)

Inventário das 26 portas:

- `app/chamados/models.py:201` `ChamadoColaborador._gravar_estado` — status/resolução/via/autor/urgência/atualização — efeito|decisao.
- `app/chamados/models.py:1852` `DisputaSupervisao.fechar` — F; delega carimbo excepcional — efeito|decisao.
- `app/chamados/models.py:2123` `DisputaSupervisao.reabrir_admin` — F, contador, motivos — decisao|fica.
- `app/chamados/models.py:2160` `DisputaSupervisao.reabrir_sistema` — F — efeito.
- `app/chamados/models.py:3092` `PerguntaDisputa.carimbar` — C; condicionalmente R/V/L — efeito|decisao|fica.
- `app/chamados/models.py:3157` `PerguntaDisputa.carimbar_identidade` — C, recusa opcional — efeito; decisão vem do chamador.
- `app/chamados/models.py:3192` `PerguntaDisputa.descarimbar` — C nulo — efeito.
- `app/chamados/models.py:3241` `PerguntaDisputa.descarimbar_em_lote` — C nulo, `outros` — efeito.
- `app/chamados/models.py:3257` `PerguntaDisputa.encerrar_por_fato_no_espelho` — via — efeito.
- `app/chamados/services/validacao.py:127` `_carimbar_validacao` — V — decisao.
- `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` — V nulo — decisao.
- `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` — R, limpa V, recusa e C condicionais — decisao.
- `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` — limpa R/V, C opcional — decisao.
- `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` — R — decisao.
- `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` — instante original da resposta — decisao.
- `app/chamados/services/relavra_via.py:106` `relavrar` — via/L — efeito|fica.
- `app/chamados/services/materializacao.py:784` `_materializar_resposta_hora` — pendência/recusa — efeito.
- `app/chamados/services/materializacao.py:1083` `_carimbar_recusa` — recusa; delega C na colisão — efeito.
- `app/chamados/services/validacao.py:533` `recusar_pergunta` — recusa; delega R/V — decisao.
- `app/chamados/services/validacao.py:625` `conferir_data_do_fato` — data conferida/autor — decisao.
- `app/chamados/models.py:833` `ChamadoColaborador.abrir` — identidade/contexto/status inicial — fica.
- `app/chamados/models.py:1054` `ChamadoColaborador.criar` — identidade/contexto/status inicial — fica.
- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa` — nascimento da Disputa — fica.
- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos` — nascimento da Disputa — fica.
- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado` — nascimento da Disputa — fica.
- `app/chamados/models.py:2822` `PerguntaDisputa._nascer` — identidade/defaults R/V/C; cache no reuso — fica.

Demais escritores do v1 permanecem:

- `app/chamados/veredito_lavrado.py:240` `gravar_lavras_em_lote` — L — fica/cache.
- `app/chamados/veredito_lavrado.py:254` `lavrar` — L — fica/cache.
- `app/chamados/services/arquivo.py:141` `silenciar` — silêncio/prazo/autoria/motivo/atualização — fica.
- `app/chamados/services/arquivo.py:177` `levantar` — silêncio/resgate/atualização — fica.
- `app/chamados/services/fio.py:804` `reapontar_resolvedora` — destino/célula/dia/L — fica.
- `app/chamados/services/fio.py:970` `rechavear_data_evento` — dia/célula/L — fica.
- `app/chamados/models.py:1968` `DisputaSupervisao.remover_motivo` — próprios motivos/contexto; decisão e carimbo delegados — decisao|fica.
- `app/chamados/models.py:1949` `DisputaSupervisao.aparar_motivo_respondido` — motivos/contexto — fica.
- `app/chamados/models.py:2179` `DisputaSupervisao.reabrir_por_colab` — contador; reabertura delegada — fica|decisao.
- `app/chamados/models.py:2247` `DisputaSupervisao.limpar_resolucao_residual` — texto residual — fica.
- `app/chamados/models.py:3402` `PerguntaDisputa.save` — executor genérico — fica.
- `app/chamados/models.py:563` `ChamadoColaborador.apagar_em_ensaio` — exclusão restrita a ensaio — fica.
- `app/chamados/models.py:349` `_vivas_entre`; `:1468` `_universo_da_ressuscitacao`; `:3086` `_escrever_carimbo`; `:3148` `_desfazer_em_memoria` — memória/simulação, sem persistência própria — fica.

Omissão da lista de escritores do v1: `app/chamados/models.py:3932` `DecisaoPergunta.registrar`, `bulk_create` em `:3934`, campos do ato, autoria, instante, conteúdo, motivo, foto anterior e ligação de absorção — **decisao**. É a persistência canônica da sequência, sem ato novo independente: apresentá-la separadamente das 26 portas da projeção evita somar novamente todas as decisões que já a chamam. Seu executor `DecisaoPergunta.save` fica em `app/chamados/models.py:3892`.

Chamadores produtivos da sequência:

- `app/chamados/models.py:1921` `fechar`: SILENCIO.
- `app/chamados/models.py:2053` `remover_motivo`: RETRATACAO.
- `app/chamados/services/materializacao.py:563`: ABSORCAO.
- `app/chamados/services/validacao.py:139`, `:205`.
- `app/chamados/services/disputa_emissao.py:1508`, `:1612`, `:1653`, `:1674`.
- `app/ponto/services/veredito_celula.py:194`: SILENCIO.
- `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`: reconstrução operacional.
- Testes exemplos: `app/chamados/tests/test_e2_tarefa_do_dia.py:120`, `:757`, `:779`; `app/chamados/tests/test_e1_volta2.py:84`, `:88`.

Omissões antigas de metadados/conteiner, fora do N literal de estado da pergunta:

- `app/chamados/models.py:488` `rebaixar`, save `:498`: urgência/original/atualização.
- `app/chamados/models.py:513` `escalar_urgencia`, save `:553`: urgência/atualização.
- `app/chamados/models.py:618` `_rebaixar_sem_post_save`, update `:636`: urgência/original/atualização.
- `app/chamados/models.py:573` `definir_prazo_sla`, save `:591`: prazo/atualização.
- `app/chamados/models.py:594` `marcar_escalonado_dp`, save `:610`: recibo do push/atualização.
- `app/chamados/models.py:2072` `anotar_contexto`, save `:2102`.
- `app/chamados/models.py:2105` `renovar_contexto`, save `:2120`.
- `app/chamados/services/disputa_emissao.py:257` `perguntar_entradas_pendentes`, save `:296`: contexto da Disputa.
- `app/chamados/services/contexto.py:62`, `:82`, `:108`, `:131`: `anotar_contexto`, `renovar_contexto`, `retirar_contexto`, `fundir_contexto`; contexto_json/atualização.
- `app/chamados/services/vinculos.py:81`, `:93`, `:105`, `:121`, `:139`: justificativa/célula/ticket/responsável.

Chamadores das principais omissões: `_rebaixar_sem_post_save` é chamado por `app/chamados/signals.py:72`; escalada por `app/ponto/services/triagem_batida.py:292`, `:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:132`, `app/ponto/management/commands/processar_alertas_avancados.py:176`; prazos por `app/chamados/services/cobranca.py:178` e `app/chamados/services/disputa_emissao.py:1715`; recibo DP por `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`; anotar contexto da Disputa por `app/chamados/views.py:1343` e `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`; renovar por `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; entradas pendentes por `app/chamados/services/disputa_emissao.py:685`, `:1267`. `rebaixar` teve somente chamadores diretos de teste localizados: `app/chamados/tests/test_selo_e4_gates.py:73`, `:84`, `:89`; chamada dinâmica produtiva [nao verificado].

## 09/11 16:05 · sessao 6ed3e3a2c318 · - · - · origem=Ronald (tmux)

D1–D15 permanecem; o diff não resolve nem elimina as distinções de estado já catalogadas. D2, D9 e D11 precisam incorporar os novos leitores e os limites explícitos abaixo. (`app/chamados/models.py:3023`; `app/chamados/models.py:3092`; `app/chamados/juizes.py:1322`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:105`)

- **D2 ampliada:** pré-filtro SQL, julgamento C6, partição de apresentação e tarefa são operações diferentes. `consulta_do_app` particiona as C6 vivas: pendente exige `respondida_em is None` **e** disputa aberta; demais vivas vão ao histórico. A carga da tarefa agora chama `alimentar_fila`, sem executar novamente C6. (`app/chamados/models.py:3325`; `app/chamados/services/pergunta_no_app.py:69`; `app/chamados/services/tarefa_do_dia.py:407`; `app/chamados/juizes.py:1450`)
- **D9 ampliada:** janela C6 continua inclusiva e usa `date.today()`; expiração da lavra usa dia local do `agora` recebido, com `janela_expira_em <= hoje_local`. O instrumento passou a declarar que `--agora` não controla aquele relógio C6. Divergência efetiva em produção: **[nao verificado]**. (`app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:104`; `app/chamados/veredito_lavrado.py:116`; `app/chamados/management/commands/diff_e3_sombra.py:105`)
- **D11 ampliada:** disputa fechada continua sem demonstrar ausência de tarefa. A alteração põe pergunta viva de disputa fechada no histórico do app; o teste mantém a aceitação pelo POST. Atenção: `reabrir_por_colab` recusa disputa fechada antes de delegar a correção; a afirmação ampla do v1 sobre correção sem reabrir o continente deve referir-se à porta `reabrir_para_correcao`, não ao wrapper de colaborador. (`app/chamados/services/pergunta_no_app.py:70`; `app/api/tests/test_hx_api_estado.py:54`; `app/chamados/models.py:2221`; `app/chamados/models.py:2234`; `app/chamados/services/disputa_emissao.py:1559`)

Duas decisões adicionais justificadas, omitidas no v1:

- **D16 — “No app” e “pendente no app” têm universos diferentes.** `perguntas_do_app` devolve todas as C6 vivas; `consulta_do_app` extrai pendentes de disputa aberta e mantém as restantes no histórico. O leitor C17 `_no_app` continua usando a coleção completa de `perguntas_do_app`. Isso já tem corte de unidade; não é pergunta de lei nova. (`app/chamados/services/pergunta_no_app.py:44`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/juizes.py:1509`; `app/chamados/juizes.py:1519`; `app/docs/PROMPTS.md:2002`)
- **D17 — `rota/fila/verbos/fio` de C17 e tarefa não prometem a mesma coisa.** C17 parte da alcançabilidade pela fila e pelas perguntas C6; a tarefa parte das ações exigíveis e publica somente suas portas. `fio` C17 pode ser `chamado`; o da tarefa também exprime `sistema` e `nao_sei`. O corte já escolheu a resposta da tarefa e condicionou a troca dos três chamadores ao DIFF. (`app/chamados/juizes.py:1532`; `app/chamados/juizes.py:1575`; `app/chamados/services/tarefa_do_dia.py:425`; `app/chamados/services/tarefa_do_dia.py:431`; `app/docs/PROMPTS.md:2007`)

Correção de inventário do v1: **`_estado_da_decisao` não existe nesta cópia**. As referências v1 de passivo e validação pertencem a `_pela_decisao`, definida na linha 115; passivo na 127 e validação na 140. O diff fornecido não renomeia essa função. (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:140`)

Chamadores produtivos C17 confirmados pelo scanner AST extraído sem importar a aplicação: `vigia_fio.censo_alcance`, `cadastro_realidade.beco_em_lote`, `fora_da_folha._porta`. O instrumento chama `_porta` existente; não cria quarto chamador direto. (`app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`; `app/folha/fora_da_folha.py:67`; `app/chamados/management/commands/diff_e3_sombra.py:124`)

**Perguntas de lei: 0.** O v1 não tinha perguntas abertas; unidade do app e destino de C17 já têm respostas literais, inclusive a emenda posterior que condiciona a fachada ao DIFF. (`app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

Leis/cortes checados: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4; LEI-TRANCA-E2-JUIZ; LEI-E3-UNIDADE-E-C17; LEI-C17-UM-JUIZ. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`; `app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

## 09/11 15:48 · sessao 43a71806087d · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Cura escrita para a unidade **pergunta + testemunha-batida**, com A factual, escrita apenas da pergunta, ordem por instante e paridade final. [materializacao.py:72](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:72)

Foram escritos 13 testes novos, reforçada a fixture `sem_par` e acrescentados M41–M50. O M41 recoloca a cegueira à batida contraparte e aponta para o teste (a). [Testes e mutantes](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:117) [nao verificado]

A entrega contém todos os chamadores, efeitos nos leitores, endereço de cada hunk e **comandos individuais**. [Entrega v2](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:1)

Somente verificações estáticas foram executadas. **RED/GREEN, mortes, concorrência e efeitos em produção: [nao verificado].** C7/fechar permanecem para os hunks da sessão. [Pendências](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:110)

## 09/11 15:47 · sessao 6b3547444afa · - · - · origem=Ronald (tmux)

Não encontrei falha concreta no diff que impeça os testes positivos ou inclua a batida no universo de escrita. Revisão estática, execução [nao verificado]. [nao verificado]

Conferi:

- Identidade usa o DNA do dia, alinhamento existente e `batidas_apuraveis`; exige uma candidata: `chamados/services/materializacao.py:113`.
- `perguntas_do_par` separa seleção/escritas da testemunha: `chamados/services/materializacao.py:67`.
- Projeção da testemunha é A e seus IDs não entram em `par_ids`: `chamados/services/materializacao.py:570`, `:634`.
- Ordem compara instantes antes da escrita e instantes reais depois; paridade examina ambas as metades: `chamados/services/materializacao.py:1443`, `:1457`, `:1467`.
- ON distingue A factual da testemunha de A da pergunta; replay de aplicação retorna antes de decisões/escritas: `chamados/services/materializacao.py:1537`, `:1542`.
- Casca aceita uma pergunta, template não cria entrada horária da testemunha: `chamados/services/acoes_disputa.py:23`, `templates/chamados/partials/_par_intervalo.html:37`.
- Testes positivos exigem uma chamada à porta, ordem integral E–S–E–S e preservação da batida existente: `chamados/tests/test_bug_par_unidade.py:428`, `:456`.
- Testes de universo e O330 trabalham somente com a pergunta: `chamados/tests/test_bug_par_unidade.py:622`, `:634`.

Nenhum arquivo editado por este explorer; nenhum runtime executado [nao verificado].

## 09/11 15:46 · sessao 5275290d9dd3 · FIO-E0-V4-E3 · verificador · origem=Code

**PROCEDENTE adicional no TOTAL:** `tarefa_do_dia` ainda pode alcançar escrita indireta: carga → tranca → toque → `get_user_sistema()` → `User.objects.get_or_create`. Com tranca vigente e usuário ausente, a leitura pode criar usuário. Cura: leitura estrita do usuário, com erro se ausente, e teste que proíba criação nesse cenário. (`app/chamados/services/tarefa_do_dia.py:370`, `app/chamados/juizes.py:1173`, `app/core/utils.py:18`)

As leis literais, triagem v3b, delta e entregas citadas não foram localizados na cópia; sua conferência literal e a identificação exata de F1/F2 ficam **[nao verificado]**. Abaixo, caminhos relativos a `app/`.

1. **P1–P5: cura e mordida**

| Item | Classificação e sítio | Teste que morde; passaria sem cura? | [nao verificado]
|---|---|---|
| P1 | **NAO PROCEDE no sítio corrigido.** Aviso e cobrança delegam à mesma lista central. `chamados/services/aviso_pendencia.py:9`, `chamados/services/cobranca.py:48`. | Fechada viva + aberta, e somente fechada: `chamados/tests/test_e3_v4_orm.py:20`, `:37`. **Não:** o filtro anterior incluiria a fechada. |
| P2 | **NAO PROCEDE no sítio corrigido.** Lista nativa usa pendentes + histórico julgados por C6. `api/views.py:1058`. | O teste prova que a morta por tranca permanece em `q_vivas` e exige somente viva + respondida na API. **Não:** o SELECT anterior incluiria a morta; somente pendentes perderia a respondida. `chamados/tests/test_e3_v4_orm.py:42`, `:60`, `:66`. |
| P3 | **NAO PROCEDE pelo `handle` do instrumento.** Lê sistema e fornece seu PK às dependências. `chamados/management/commands/diff_e3_sombra.py:94`, `:98`. | Ausência de sistema e tranca real: **não passariam** reintroduzindo criação ou retirando a semente. `chamados/tests/test_e3_diff_sombra.py:26`, `:41`. |
| P4 | **NAO PROCEDE para exclusões, atribuição simples e `getattr` literal.** Models/registro entram na varredura. `chamados/tests/test_e3_pendentes_app.py:69`, `:75`, `:96`. | **Não:** inclusão e resolução são exigidas em `:119`, `:124`. O contador isolado de `:144` poderia passar com detector incompleto. |
| P5 | **NAO PROCEDE no isolamento/custo corrigidos da consulta.** Memo novo, alimentação única e restauração do contexto. `chamados/juizes.py:1091`, `:1099`, `:1102`. | Uma/cinco perguntas frias e frescor após tranca, toque, retirada do toque e reabertura. **Não:** retirar lote ou reutilizar memo persistente viola essas asserções. `chamados/tests/test_e3_v4_orm.py:85`, `:101`. |

A equivalência desses rótulos com o texto integral do v3b permanece **[nao verificado]**.

2. **UNIDADE: ainda existem leitores externos**

**FILA — o contador literal não é zero:**

- Relatório calcula C6 + ausência de resposta; pauta semanal calcula `q_vivas + sem resposta + disputa aberta`. `chamados/relatorios_disputa.py:18`, `:21`; `chamados/services/cobranca.py:228`.
- Painel/placar separa respondidas e não respondidas fora da consulta. `chamados/services/raiox.py:135`, `:139`, `:140`; consumidor `chamados/services/verbos.py:34`.
- UI web combina `p.viva` e `respondida_em`; o fio também calcula `todas_respondidas`. `templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:155`; `chamados/services/fio.py:340`.

Esses cálculos não foram acrescentados pelo TOTAL fornecido. Qual deles corresponde exatamente a F1/F2, e quais exceções a lei autoriza, fica **[nao verificado]**.

**NAO PROCEDE — kernels e dashboard corrigidos:** ambos os kernels e o dashboard contam perguntas da consulta central. `api/views.py:325`, `api/views_core.py:314`, `core/views.py:253`.

3. **P5: memo, autoridade, equivalência e N+1**

- **NAO PROCEDE — persistência do memo da consulta:** o `ContextVar` é global, mas o conteúdo nasce por chamada e o token é restaurado no `finally`. `chamados/juizes.py:1080`, `:1091`, `:1102`.
- **NAO PROCEDE — predicado duplicado:** C6 chama o avulso; o avulso delega ao lote; toque avulso também delega ao lote. Empresa/competência e trilha são julgadas nesses mesmos corpos. `chamados/juizes.py:1327`, `:951`, `:1022`, `:1145`.
- **FILA — equivalência irrestrita é falsa:** fora da consulta, o avulso conserva `_tranca_memo` no request/colaborador. Pode ler “não trancado”, ocorrer tranca e continuar retornando falso; a consulta seguinte usa memo novo e retorna verdadeiro. Reproduzido em memória. Cura eventual: invalidar o memo ou explicitar seu escopo de validade. `chamados/juizes.py:1064`, `:1068`, `:1024`, `:1091`.
- **O `assertNumQueries` morde N+1 de tranca/toque no cenário exercitado:** exige nove consultas tanto com uma quanto com cinco perguntas, em instâncias frias. Não prova custo constante nos demais caminhos de C6. `chamados/tests/test_e3_v4_orm.py:90`, `:91`, `:97`, `:98`.

4. **P3: escrita transitiva**

**NAO PROCEDE para o comando normal:** a semente impede alcançar o `get_or_create`; todos os aliases recebem a guarda SQL, e há teste que injeta escritor numa dependência. `chamados/management/commands/diff_e3_sombra.py:92`, `:98`; `chamados/juizes.py:1173`; `chamados/tests/test_e3_diff_sombra.py:71`.

**FILA da guarda:** aceita `SELECT … FOR UPDATE`; isso não demonstra caminho concreto de trava/escrita nesta árvore, mas impede considerá-la bloqueio universal de travas. Cura: rejeitar também essa forma e acrescentar caso negativo. `chamados/management/commands/diff_e3_sombra.py:35`.

5. **Classes reincidentes no TOTAL**

- **PROCEDENTE — escritor transitivo na tarefa:** é o achado inicial. O selo de zero escritas exercita cenário sem tranca e, portanto, pode passar apesar desse caminho. `chamados/services/tarefa_do_dia.py:370`, `chamados/juizes.py:1176`, `core/utils.py:18`, `chamados/tests/test_e2_tarefa_do_dia.py:878`.
- **NAO PROCEDE — derivação paralela nos pontos examinados:** posição, passivo, lavra, tranca e verbos são lidos das autoridades existentes. `chamados/services/tarefa_do_dia.py:65`, `:346`, `:350`, `:367`, `:408`.
- **NAO PROCEDE — fachada C17 pousada antecipadamente:** o instrumento compara resultados; não redireciona C17 para tarefa. `chamados/management/commands/diff_e3_sombra.py:124`, `:125`, `:127`.
- **NAO PROCEDE — relógio novo dentro da tarefa/lavra leitora:** recebem instante; decisões/efeitos incluem igualdade, lavra futura é rejeitada e silêncio é estritamente futuro. `chamados/services/tarefa_do_dia.py:339`, `:358`; `chamados/veredito_lavrado.py:224`; `chamados/services/painel.py:190`. O fallback antigo de C6 ainda lê `date.today()`. `chamados/juizes.py:575`.
- **FILA — selo C17 incompleto:** import relativo e alias por desempacotamento escapam do detector; nenhum quarto chamador real foi demonstrado. Cura: resolver `ImportFrom.level` e alvos compostos, com REDs. `chamados/tests/test_e3_pendentes_app.py:46`, `:78`.
- **FILA — ocorrência repetida:** tarefa distingue `int_ida:2`, mas a identidade persistente exige posição derivada do motivo; a tarefa abstém ou pede emissão. Cura futura exige migrar identidade, emissão e leitura juntas. `chamados/services/tarefa_do_dia.py:67`, `:268`; `chamados/models.py:3425`. Exigibilidade nesta etapa **[nao verificado]**.

6. **Escritores → leitores principais, inclusive fora do diff**

| Dado/função | Quem produz/escreve → quem lê |
|---|---|
| `pendentes`, `historico` | Consulta particiona em memória: `chamados/services/pergunta_no_app.py:69` → kernels, dashboard e API: `api/views.py:325`, `:1023`, `:2561`; `api/views_core.py:314`; `core/views.py:253`. |
| `perguntas_pendentes_do_colab` | Delega à consulta → aviso textual. `chamados/services/aviso_pendencia.py:9`, `:13`. Cobrança delega diretamente: `chamados/services/cobranca.py:48`. |
| Saída da tarefa: instâncias, ações, razão, rota/fila/verbos/fio | Produção em memória: `chamados/services/tarefa_do_dia.py:237`, `:283`, `:433` → instrumento/projeções: `chamados/management/commands/diff_e3_sombra.py:125`, `:44`, `:51`. |
| `data_evento`, identidade | Atribuição: `chamados/services/fio.py:800`; persistência do rechaveamento: `:1034`; identidade: `chamados/models.py:3425` → tarefa: `chamados/services/tarefa_do_dia.py:233`, `:324`; C6: `chamados/juizes.py:1386`. |
| Resposta/validação e sequência | Projeção: `chamados/models.py:3121`, `:3124`, `:3141`; sequência: `:3934` → consulta: `chamados/services/pergunta_no_app.py:70`; passivo/decisões: `chamados/services/tarefa_do_dia.py:337`, `:346`. |
| Ata/lâmpadas | Persistência pela porta: `ponto/portas/celula.py:969` → leitor único: `escala/services/leitor_celula.py:130` → tarefa: `chamados/services/tarefa_do_dia.py:84`; C6: `chamados/juizes.py:925`. |
| Lavra e marcos | Escritores: `chamados/veredito_lavrado.py:250`, `:270` → validade: `:224`; tarefa: `chamados/services/tarefa_do_dia.py:350`; leitor legado: `chamados/services/raiox.py:184`. |
| Ausência/cobertura | Estado: `ponto/services/ausencia.py:824`, `:835` → cobertura: `ponto/turnos.py:3064`; tarefa: `chamados/services/tarefa_do_dia.py:211`, `:152`. |
| Status/silêncio, fila e verbos | Status: `chamados/models.py:296`; silêncio: `chamados/services/arquivo.py:166`, `:202` → painel: `chamados/services/painel.py:188`; verbos: `chamados/services/verbos.py:47`, `:51` → alimentação: `chamados/juizes.py:1460`. |
| Tranca/toque e lote C6 | Período: `ponto/services/fechamento.py:1400`, `:1432`; trilha: `chamados/services/cobrar_dia.py:39`, `ponto/services/veredito_celula.py:355`, `chamados/views_arquivar.py:77` → lote: `chamados/juizes.py:1026`, `:1215`; consumidores: `:1327`, `chamados/models.py:1843`, `chamados/reconciliador.py:667`, `chamados/services/disputa_emissao.py:1844`, `chamados/services/tarefa_do_dia.py:370`. |

7. **Merge futuro**

**FILA — acoplamentos previsíveis:** preservar seleção/histórico nos três pontos da API; manter identidade do modelo compatível com a tarefa; preservar a regra compartilhada de posição. Alterações nesses pontos podem mudar comportamento sem conflito textual. Conflito efetivo com o outro main **[nao verificado]**. `api/views.py:325`, `:1058`, `:2561`; `chamados/models.py:3425`; `chamados/services/tarefa_do_dia.py:108`; `chamados/services/declaracao_texto.py:79`, `:134`.

Resultado da suíte inteira e “48/48 mortos”: **[nao verificado]**. [nao verificado]

## 09/11 15:46 · sessao 3efb1b2451e1 · CODEX-2-RAIAS-PORTAO-CPU · leitura · origem=Code

**A matriz fixa quatro divergências procedentes: P1 na representação dos nomes; P2 no julgamento da pasta-pai; P3 no significado declarado de “run”; P4 nos esperados dos mutantes de carga.** O eco de diagnóstico é uma fragilidade adicional do classificador, sem mutante atual demonstrado que a produza. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:287`, `bin/codex_vagas_medida.py:114`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:747`; ocorrência do eco em mutante atual: [não verificado])

Nesta matriz, **E** significa esperado e **A** significa atual. `E=A` indica concordância. O esperado aplica “arquivo em comum”, preserva arquivo exato sem `/`, pasta por componente com `/` e a recusa conservadora do mesmo nome com/sem `/`. (`CLAUDE.md:306`, `CLAUDE.md:307`, `CLAUDE.md:278`, `bin/codex_pergunta.sh:258`)

Os resultados abaixo são leitura do código e dos casos escritos. GREEN atual da árvore e ocorrência em produção permanecem **[não verificado]**.

**Convenções de persistência e retorno usadas em A e B**

| Sigla | Esperado e atual, salvo divergência expressa |
|---|---|
| **P0** | Recusa anterior ao `ini`: nenhum novo par `ini/fim` nem novo `.rc` desta tentativa. Pode haver criação dos arquivos de lock; estado eventualmente publicado antes de uma recusa posterior é removido pelo EXIT. Artefatos antigos no mesmo nome não equivalem a sinais desta tentativa. (`bin/codex_pergunta.sh:228`, `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:392`, `bin/codex_pergunta.sh:428`) |
| **PV** | Recusa de portão/teto/meta na admissão: P0 mais tentativa de append em `vaga2.log`, campos `hora, obra, motivo, load5, fonte`. Falha desse append não impede rc 2. (`bin/codex_pergunta.sh:249`, `bin/codex_pergunta.sh:250`) |
| **P+** | Admitida: `.estado` temporário por rename, com `toca=`; depois `chamadas.log ini`. Após retorno do Codex: `.inventario`; no EXIT, `fim`, `.rc` e remoção do estado. `.id` somente com RC 0 e resposta nova desta chamada. (`bin/codex_pergunta.sh:310`, `bin/codex_pergunta.sh:311`, `bin/codex_pergunta.sh:428`, `bin/codex_pergunta.sh:560`, `bin/codex_pergunta.sh:229`, `bin/codex_pergunta.sh:232`, `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:603`) |
| **P!** | P+ com `.inventario` proibido/falho, `.recusa`, `.rc rc=3`, `fim`; sem novo `.id`. A mensagem manda descartar a WT, mas não a remove automaticamente. (`bin/codex_pergunta.sh:576`, `bin/codex_pergunta.sh:578`, `bin/codex_pergunta.sh:579`, `bin/codex_pergunta.sh:587`, `bin/tests/test_codex_pergunta.sh:169`) |
| **R0** | Admitida e execução/inventário verdes: retorno do CLI, 0 no controle; mensagem `sessao=… rc=0 jsonl=…`. Admissão, sozinha, não garante sucesso final. (`bin/codex_pergunta.sh:551`, `bin/codex_pergunta.sh:608`, `bin/codex_pergunta.sh:609`) |
| **R2** | `construtor: … (rc 2; Codex nao chamado)`. (`bin/codex_pergunta.sh:244`) |
| **RV2** | `VAGA2: … (rc 2; Codex nao chamado)`. (`bin/codex_pergunta.sh:250`) |
| **R3** | `RECUSA rc 3 … wt DESCARTADA`; inventário proibido/falho vence qualquer retorno do CLI. (`bin/codex_pergunta.sh:557`, `bin/codex_pergunta.sh:578`, `bin/codex_pergunta.sh:587`) |

**A. PORTA `--toca`**

As comparações da admissão operam sobre declarações. O fim compara caminhos encontrados no inventário com essas declarações; só processa registros cuja primeira coluna seja `TOCADO`. (`bin/codex_pergunta.sh:270`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:280`)

| Célula / estado do caminho | Argumento: parser/validação | Admissão: outra vaga viva | Inventário: classe, serialização e releitura | Juiz final: fora / comum | Persistido | Retorno | Prova: caso / mutante |
|---|---|---|---|---|---|---|---|
| **A1 Arquivo exato** `app/folha/views.py` | E=A: recebe em `TOCA[]`; sintaxe válida. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:323`) | E=A: mesmo arquivo cruza; arquivo distinto não cruza, ressalvada cobertura por pasta. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A para nome simples: `TOCADO`, TSV cru; leitor recupera o segundo campo. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`) | E=A: igualdade cobre somente esse arquivo; outro arquivo fica fora; cobertura da outra raia recusa. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | E=A: P+ se válido; P! se violar escopo. | E=A: R0/R3; conflito declarado na abertura dá RV2. | `c_toca_exato`, `c_toca_fim` / `toca_exato`, `toca_fim`; E1/E3. (`bin/tests/test_codex_pergunta.sh:559`, `bin/tests/test_codex_pergunta.sh:562`, `bin/tests/test_codex_pergunta.sh:732`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A2 Pasta/** `app/folha/` | E=A: `/` final declara pasta. (`bin/codex_pergunta.sh:321`) | E=A: cruza descendentes e pastas ancestrais declaradas. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A: descendentes modificados são inventariados; pasta nova também ganha registro terminado em `/`. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:217`) | E=A: prefixo com `/` cobre por componente. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+/P!. | E=A: R0/R3; sobreposição declarada dá RV2. | `c_toca_exato/controle_pasta` / `toca_exato`; E1. (`bin/tests/test_codex_pergunta.sh:567`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A3 Pasta-pai criada para arquivo nomeado**, sem outra declaração sob o pai | E=A: declara `app/novo/a.py`, sem exigir declaração ampla de `app/novo/`. (`bin/codex_pergunta.sh:323`) | E=A: compara o arquivo declarado com a outra declaração. (`bin/codex_pergunta.sh:273`) | E=A: cria registros `app/novo/` e `app/novo/a.py`; ambos em TSV cru. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:217`) | E=A: pai necessário é aceito pelo meu escopo; arquivo continua exato. (`bin/codex_pergunta.sh:284`) | E=A: P+. | E=A: R0. | `c_toca_exato/controle_pasta_nova`; falta mutante específico da aceitação do pai. E6. (`bin/tests/test_codex_pergunta.sh:569`; mutante específico: [não verificado]) |
| **A4 Arquivos irmãos sob pai novo**: minha `app/novo/a.py`, outra `app/novo/b.py` | E=A: ambas declarações válidas. (`bin/codex_pergunta.sh:323`) | E=A: admite, pois arquivos diferentes não cruzam. (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:273`) | E=A: a pasta-pai nova também é `TOCADO`. (`bin/codex_pergunta.sh:196`) | **DIVERGE P2. E:** pai necessário não constitui arquivo comum; aceita. **A:** aceita o pai no meu escopo, mas `cruza(outra, pai)` o considera comum com `b.py`. (`CLAUDE.md:306`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | **E: P+. A: P!**, com falso motivo de arquivo comum. (`bin/codex_pergunta.sh:288`, `bin/codex_pergunta.sh:579`) | **E: R0. A: R3.** (`bin/codex_pergunta.sh:587`) | E6: caso irmãos novos / mutante restaura colisão pelo pai. E7: controle outra pasta cobrindo deve recusar. Novos: [não verificado]. Sítio atual: `bin/codex_pergunta.sh:287`. |
| **A5 Pasta irmã / prefixo parecido**: `app/folhas/` × `app/folha/` | E=A: válida. (`bin/codex_pergunta.sh:323`) | E=A: disjuntas. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A: nomes permanecem distintos no TSV simples. (`bin/codex_pergunta.sh:217`) | E=A: `folha/` não cobre `folhas/`; tocar esta última sob declaração da primeira fica fora. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+ quando declarada; P! quando fora. | E=A: R0/R3. | `c_v2_toca/controle`, `c_toca_exato/folha_nao_cobre_folhas` / `toca`, `toca_exato`; E1/E3. (`bin/tests/test_codex_pergunta.sh:530`, `bin/tests/test_codex_pergunta.sh:564`, `bin/tests/test_codex_pergunta.sh:725`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A6 Mesmo nome com e sem `/`** | E=A: ambas grafias válidas, com significados distintos de cobertura. (`bin/codex_pergunta.sh:258`, `bin/codex_pergunta.sh:323`) | E=A: recusa conservadora por igualdade após retirar `/`. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:266`) | E=A: se já recusado, não alcança inventário final. Sem outra viva, inventário mantém arquivo e pasta com suas grafias. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:306`) | E=A: sem `/` não autoriza descendentes; sobreposição com outra declaração continua proibida. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:287`) | E=A: PV na admissão; P+/P! nos cenários sem conflito inicial. | E=A: RV2; ou R0/R3 conforme o toque. | `c_v2_toca/app/folha`, `c_toca_exato/arquivo_nao_e_pasta` / `toca`, `toca_exato`; E1/E3. (`bin/tests/test_codex_pergunta.sh:529`, `bin/tests/test_codex_pergunta.sh:563`, `bin/tests/test_codex_pergunta.sh:725`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A7 Nome com TAB**: arquivo real `views.py<TAB>fora`, declaração exata `views.py` | E=A: argumento contendo TAB é recusado; a declaração simples passa. (`bin/codex_pergunta.sh:323`) | E=A sobre a declaração simples; desconhece o futuro nome com TAB. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** nome com controle é PROIBIDO, com motivo próprio inequívoco. **A:** imprime nome cru; TAB divide campos, e só `c[1]` vira caminho. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`, `bin/codex_pergunta.sh:282`) | **E:** recusa o nome real. **A:** compara apenas `views.py`, podendo aprová-lo como arquivo exato. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** pode ficar P+, com `.inventario` cujo segundo campo truncado aparenta ser permitido. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível** no cenário isolado descrito. (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:587`, `bin/codex_pergunta.sh:609`) | E4/TAB / mutante remove proibição de controle. Caso e mutante próprios atuais: [não verificado]. Lista atual: `bin/tests/test_codex_pergunta.sh:617`. |
| **A8 Nome com newline** | E=A: argumento completo recusado; declaração simples passa. (`bin/codex_pergunta.sh:323`) | E=A sobre a declaração simples. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** PROIBIDO por controle. **A:** nome cru quebra registro em linhas; fragmento inicial é lido como caminho, fragmentos sem classe `TOCADO` são ignorados. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:280`) | **E:** recusa. **A:** prefixo anterior ao newline pode casar com arquivo exato declarado. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** P+ possível; registros físicos deixam de representar uma entrada por linha. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível.** (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:609`) | E4/newline / mutante remove proibição de controle. Novos: [não verificado]. Sítios atuais: `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`. |
| **A9 Nome com CR** | E=A: argumento completo recusado. (`bin/codex_pergunta.sh:323`) | E=A sobre eventual declaração simples. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** PROIBIDO por controle. **A:** escrita crua e leitura textual com tradução de quebras permitem que CR separe o nome durante a leitura. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`) | **E:** recusa. **A:** pode comparar somente o prefixo anterior ao CR. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** P+ possível, sem preservação do nome real no julgamento. (`bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível.** (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:609`) | E4/CR / mutante remove proibição de controle. Novos: [não verificado]. |
| **A10 Nome com espaço** | E=A: argumento com espaço recusado pela gramática atual restritiva. (`bin/codex_pergunta.sh:323`) | E=A: pode declarar pasta ancestral sem espaço. (`bin/codex_pergunta.sh:264`) | E=A: espaço não separa campos TSV; nome encontrado é preservado. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`) | E=A: sob pasta declarada pode passar; `views.py espaço` não equivale ao arquivo exato `views.py`. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+/P!, conforme cobertura. | E=A: R0/R3; argumento com espaço dá R2. | E5: controles positivo sob pasta e negativo contra arquivo exato / mutante reduz nome ao primeiro trecho separado por espaço. Novos: [não verificado]. |
| **A11 Symlink tocado** | E=A: valida nome, não o tipo futuro. (`bin/codex_pergunta.sh:323`) | E=A: compara declarações. (`bin/codex_pergunta.sh:273`) | E=A: `lstat`; walk não segue diretório symlink; link tocado é PROIBIDO, motivo `symlink`. (`bin/codex_pergunta.sh:188`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:215`) | E=A: já proibido pelo inventário; o juiz de escopo não o reautoriza. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P! no fim; se já inventariado no pré-check, P0. | E=A: R3 no fim; R2 no pré-check sujo. (`bin/codex_pergunta.sh:399`, `bin/codex_pergunta.sh:587`) | `c_p_link` / `p_link`; E8. (`bin/tests/test_codex_pergunta.sh:366`, `bin/tests/test_codex_pergunta.sh:663`) |
| **A12 Hardlink** | E=A: argumento não concede exceção ao tipo. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:216`) | E=A: comparação lexical não substitui inventário. (`bin/codex_pergunta.sh:273`, `bin/codex_pergunta.sh:397`) | E=A: `nlink>1` entra mesmo com conteúdo igual; motivo `hardlink`. (`bin/codex_pergunta.sh:201`, `bin/codex_pergunta.sh:216`) | E=A: pré-check/revalidação recusam; se criado na execução, inventário final proíbe. (`bin/codex_pergunta.sh:399`, `bin/codex_pergunta.sh:530`, `bin/codex_pergunta.sh:560`) | E=A: P0 antes do `ini`; P! no fim. | E=A: R2 antes da execução; R3 no fim. | `hardlink_ini`, `p_hardlink_ident`, `revalida_hardlink`, `p_hardlink` / `inv_nlink`, `p_hardlink`; E8. (`bin/tests/test_codex_pergunta.sh:296`, `bin/tests/test_codex_pergunta.sh:403`, `bin/tests/test_codex_pergunta.sh:406`, `bin/tests/test_codex_pergunta.sh:656`, `bin/tests/test_codex_pergunta.sh:664`) |
| **A13 `..`** | E=A: qualquer ocorrência de `..` é recusada, inclusive dentro de componente. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. (`bin/codex_pergunta.sh:324`) | E=A: não alcança nesta chamada; inventário usa nomes relativos encontrados, não a grafia da operação. (`bin/codex_pergunta.sh:189`, `bin/codex_pergunta.sh:193`) | E=A: não alcança por esse argumento. | E=A: P0. | E=A: R2, `invalido`. | `c_toca_forma` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A14 `//`** | E=A: recusado. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. | E=A: não alcança por esse argumento. | E=A: não alcança. | E=A: P0. | E=A: R2, `invalido`. (`bin/codex_pergunta.sh:324`) | `c_toca_forma/app//x` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A15 Absoluto** | E=A: recusado por não começar com `app/` ou `bin/`. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. | E=A: não alcança por esse argumento. | E=A: não alcança. | E=A: P0. | E=A: R2, `invalido`. (`bin/codex_pergunta.sh:324`) | `c_toca_forma//abs` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A16 Fora de `app/` e `bin/`** | E=A: declaração `ops/` recusada. (`bin/codex_pergunta.sh:323`) | E=A: não alcança por essa declaração. | E=A: se execução com outro argumento criar `FORA.txt` na WT, inventário marca PROIBIDO, `fora de app/ e bin/`. (`bin/codex_pergunta.sh:151`, `bin/codex_pergunta.sh:210`) | E=A: proibição independe do próprio `--toca`. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P0 no argumento; P! no toque encontrado. | E=A: R2/R3. | `c_toca_forma`, `c_p_fora` / `toca_forma`, `p_fora`; E2/E10. (`bin/tests/test_codex_pergunta.sh:352`, `bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:689`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A17 `bin/` comum** | E=A: permitido declarar. (`bin/codex_pergunta.sh:323`) | E=A: mesmas regras de arquivo/pasta comum. (`bin/codex_pergunta.sh:266`) | E=A: TOCADO se não houver outro motivo proibido; declaração `bin/...` concede root adicional `<wt>/bin`. (`bin/codex_pergunta.sh:151`, `bin/codex_pergunta.sh:539`, `bin/codex_pergunta.sh:543`) | E=A: exige cobertura própria e ausência de cobertura conflitante. (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | E=A: P+/P!. | E=A: R0/R3. | `c_bin_ok`; E9. Caso atual: `bin/tests/test_codex_pergunta.sh:582`; mutante específico da concessão do root: [não verificado]. |
| **A18 `bin/` protegido** | E=A: declaração pode passar; declarar não revoga proteção. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:162`) | E=A: pode admitir se declarações disjuntas. (`bin/codex_pergunta.sh:273`) | E=A: PROIBIDO pela lista protegida, mesmo coberto pelo `--toca`. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:210`) | E=A: já proibido; não reautoriza. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P!. | E=A: R3, motivo `protegido (segue do opus)`. | `c_bin_protegido` / `lista_protegida`, `p_protegido`; E9. (`bin/tests/test_codex_pergunta.sh:592`, `bin/tests/test_codex_pergunta.sh:736`, `bin/tests/test_codex_pergunta.sh:737`) |
| **A19 `.claude/`** | E=A: `.claude/` na raiz é argumento inválido; `app/.claude/...` pode passar na sintaxe. (`bin/codex_pergunta.sh:323`) | E=A: eventual declaração válida passa só se disjunta. (`bin/codex_pergunta.sh:273`) | E=A: componente `.claude` é PROIBIDO em qualquer nível. (`bin/codex_pergunta.sh:147`) | E=A: cobertura própria não revoga proibição. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P0 ou P!, conforme fase. | E=A: R2/R3. | `c_p_claude` / `p_claude`; E10. (`bin/tests/test_codex_pergunta.sh:348`, `bin/tests/test_codex_pergunta.sh:685`) |

**ESMERIL-SEMÂNTICA:** em **A4/juiz final**, a admissão diz “arquivos disjuntos”, mas o fim transforma a pasta-pai necessária em arquivo comum. Em **A7–A9/inventário e juiz final**, o nome encontrado deixa de ser a unidade comparada: a representação admite delimitadores que o leitor interpreta estruturalmente. (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:287`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`, `bin/codex_pergunta.sh:282`)

A linha de symlink refere-se ao **tocado/inventariado**; não demonstra proibição de todo symlink rastreado intacto. Escrita transitória desfeita antes do inventário, ou fora da WT sem vestígio nela, permanece **[não verificado]**. (`bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:209`, `bin/codex_pergunta.sh:215`)

**B. VAGA E PORTÕES**

“1ª” abaixo significa nenhuma vaga viva; “2ª”, uma viva. A aplicação dos portões depende da **outra vaga viva**, inclusive quando a vaga disponível é a 1 e a ocupada é a 2. (`bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:389`, `bin/codex_pergunta.sh:390`, `bin/codex_pergunta.sh:391`)

| Célula / situação | Juiz: E × A | Admite/recusa: E × A | Persistido: E × A | Retorno: E × A | Prova: caso / mutante |
|---|---|---|---|---|---|
| **B1 1ª chamada** | E=A: meta-trava, tenta vaga 1, confirma outra livre. (`bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:390`) | E=A: dispensa portões da segunda simultânea; publica estado. (`bin/codex_pergunta.sh:391`, `bin/codex_pergunta.sh:392`) | E=A: P+. | E=A: R0 se restante verde. | `c_positivo`, primeira chamada de `c_v2_admite`; E11. (`bin/tests/test_codex_pergunta.sh:315`, `bin/tests/test_codex_pergunta.sh:506`) Controle com portões vermelhos e nenhuma outra viva: [não verificado]. |
| **B2 2ª chamada** | E=A: outra viva exige pausa, estado, carga, janela, desligador e escopo verdes. (`bin/codex_pergunta.sh:293`, `bin/codex_pergunta.sh:391`) | E=A: admite só após todos; publica sob meta. (`bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:392`, `bin/codex_pergunta.sh:393`) | E=A: P+ com `vaga=2` no caso em que a primeira ocupa vaga 1. (`bin/

[truncado em 20000 de 60850 caracteres]

## 09/11 15:44 · sessao c2a5482fbf2c · - · - · origem=Ronald (tmux)

- **P3 — NAO PROCEDE no instrumento:** o comando lê o usuário e fornece `sistema_pk` antes das dependências; o contexto restaura o memo ao sair. Assim, o fallback escritor fica inacessível pelo `handle` normal (`app/chamados/management/commands/diff_e3_sombra.py:94`, `app/chamados/management/commands/diff_e3_sombra.py:98`, `app/chamados/juizes.py:1111`, `app/chamados/juizes.py:1173`). Os testes de usuário ausente e de tranca real falhariam sem as respectivas curas (`app/chamados/tests/test_e3_diff_sombra.py:26`, `app/chamados/tests/test_e3_diff_sombra.py:41`). A guarda intercepta todos os aliases e o teste injeta escritor transitivo (`app/chamados/management/commands/diff_e3_sombra.py:92`, `app/chamados/tests/test_e3_diff_sombra.py:71`).

- **PROCEDENTE fora do instrumento — leitura nova pode escrever:** `CargaDaTarefa` chama `dias_trancados`; com tranca e memo sem usuário, chega a `get_user_sistema().pk`, cujo corpo executa `get_or_create`. Portanto, usuário ausente permite INSERT durante `tarefa_do_dia` (`app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/juizes.py:1036`, `app/chamados/juizes.py:1173`, `app/core/utils.py:18`). Cura: obter o usuário por leitura estrita ou exigir sua identidade na borda; acrescentar teste de tranca com usuário ausente, bloqueando qualquer tentativa de criação. O teste atual de zero escritas não cria tranca (`app/chamados/tests/test_e2_tarefa_do_dia.py:878`).

- **P4 — NAO PROCEDE para os defeitos nomeados:** atribuição simples e `getattr` literal são resolvidos; models e registro entram no universo (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:69`, `app/chamados/tests/test_e3_pendentes_app.py:75`, `app/chamados/tests/test_e3_pendentes_app.py:96`). Os testes de inclusão e resolução falhariam sem cura; o contador isolado poderia passar perdendo o quarto chamador (`app/chamados/tests/test_e3_pendentes_app.py:119`, `app/chamados/tests/test_e3_pendentes_app.py:124`, `app/chamados/tests/test_e3_pendentes_app.py:144`).

- **FILA — alcance residual do selo:** imports relativos ignoram `ImportFrom.level`; atribuição por desempacotamento não registra aliases. Ambos podem esconder um quarto chamador; uso real nesta árvore não demonstrado (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:78`). Cura sugerida: resolver esses dois formatos e acrescentar respectivos REDs.

- **FILA — guarda SQL:** aceita `SELECT … FOR UPDATE`; nenhum caminho concreto com essa trava foi demonstrado. Não constitui escritor P3 comprovado, mas impede chamar a guarda de bloqueio universal de travas (`app/chamados/management/commands/diff_e3_sombra.py:35`).

## 09/11 15:44 · sessao 76b09c74f4a9 · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — segundo juiz/escritor no recorte auditado:** tarefa lê a decisão ordenada por `ato_em,pk`, passivo pela autoridade existente, lavra por `esta_stale`, tranca por `dias_trancados`, fila/verbos por `alimentar_fila`; publica apenas portas das ações já julgadas. `app/chamados/services/tarefa_do_dia.py:337`, `:346`, `:350`, `:367`, `:407`, `:425`. O helper de dia sem trabalho delega ao juiz existente. `app/ponto/supra_juiz.py:65`.
- **NAO PROCEDE — pouso antecipado da fachada C17:** os únicos consumidores produtivos encontrados de tarefa/carga estão no instrumento da sombra. `app/chamados/management/commands/diff_e3_sombra.py:115`, `:125`. A tarefa usa `alimentar_fila`, sem chamar C17. `app/chamados/services/tarefa_do_dia.py:407`.
- **FILA — identidade de segunda pausa:** a saída diferencia `int_ida:2`; a pergunta persistente ainda exige a posição derivada do motivo, recusando outro valor. A tarefa declara desconhecimento quando existe pergunta da posição; sem pergunta, solicita emissão sem identidade persistente específica para a repetição. Isso impede afirmar resolução completa de pausas repetidas; obrigação nesta etapa depende da lei literal ausente. `app/chamados/services/tarefa_do_dia.py:67`, `:268`; `app/chamados/models.py:3425`. Cura futura: identidade persistente por ocorrência, emissão e leitor migrados juntos. [nao verificado]
- **NAO PROCEDE — ambiguidade temporal introduzida no julgamento:** turno selecionado por `data_evento` com `conversa=False`, intervalo inclusivo; decisão e efeito incluem igualdade com `agora`; teto vence na igualdade; lavra futura é rejeitada. `app/chamados/services/tarefa_do_dia.py:324`, `:339`, `:358`; `app/ponto/services/cartorio.py:858`; `app/chamados/veredito_lavrado.py:224`. A fila exclui silêncio estritamente futuro. `app/chamados/services/painel.py:188`.

Mapa de escrita → leitura:

| Dado | Escritor/autoridade | Leitor novo |
|---|---|---|
| Decisão | `app/chamados/models.py:3932` (`bulk_create`) | tarefa `:337` |
| Passivo | `app/chamados/models.py:3903` | tarefa `:346` |
| Dia do fato | `app/chamados/services/fio.py:800`; rechaveamento salva `:1034` | tarefa `:324`, `:233` |
| Identidade/posição | `app/chamados/models.py:3414`; posição comum `app/chamados/services/declaracao_texto.py:79` | tarefa `:65`, `:108`; declaração `:134` |
| Ata/lâmpada | `app/ponto/services/cartorio.py:638`, `:1811` → `app/ponto/portas/celula.py:969` | `app/escala/services/leitor_celula.py:130` → tarefa `:84` |
| Lavra | `app/chamados/veredito_lavrado.py:250`, `:270` | tarefa `:350`; validade `app/chamados/veredito_lavrado.py:224` |
| Batida apurável | universo `app/ponto/turnos.py:2875`; criação datada `app/ponto/models.py:79` | tarefa `:358` |
| Status do chamado | `app/chamados/models.py:228`, `:296` | fila `app/chamados/services/painel.py:188` |
| Silêncio | `app/chamados/services/arquivo.py:166`, `:202` | painel `:190`; verbos `app/chamados/services/verbos.py:51` |
| Âncora | leitura comum `app/chamados/services/fio.py:116` | tarefa `:125`, `:362` |
| Rota/fila/verbos/fio | produção exclusivamente em tarefa `:425` | projeção da sombra `app/chamados/management/commands/diff_e3_sombra.py:43`, `:47` |

**FILA — merge futuro:** pontos de acoplamento concretos são as três consultas da API (`app/api/views.py:325`, `:1023`, `:2561`), a identidade do modelo (`app/chamados/models.py:3414`) e a posição compartilhada (`app/chamados/services/declaracao_texto.py:79`). Ressuscitar método antigo, mudar identidade ou substituir a regra de posição pode alterar o comportamento; conflito efetivo com main não é demonstrável sem o outro lado. [nao verificado]

Anexos literais e triagem não foram localizados; essa ausência limita a classificação da fila de pausas e a conferência literal dos pré-requisitos. [nao verificado]

## 09/11 15:43 · sessao 97912f6ce043 · - · - · origem=Ronald (tmux)

- **P1 — NAO PROCEDE, curado na causa:** `perguntas_pendentes_do_colab` delega a `consulta_do_app()['pendentes']`; `_pendentes` faz o mesmo. O teste contém fechada viva e aberta, exige apenas aberta nos dois leitores e nos kernels; só fechada deve produzir nenhum aviso. Repor o filtro antigo incluiria a fechada e falharia nessas asserções. Execução não realizada. `app/chamados/services/aviso_pendencia.py:7`, `app/chamados/services/cobranca.py:44`, `app/chamados/tests/test_e3_v4_orm.py:20`, `app/chamados/tests/test_e3_v4_orm.py:37`.

- **P2 — NAO PROCEDE, curado na causa:** API lista `pendentes + historico`, excluindo disputa fechada; não reconstrói vitalidade com SELECT próprio. O teste constrói pergunta morta por tranca que **continua no q_vivas**, confirma exclusão central e exige viva + respondida na lista nativa. Sem a cura, o SELECT antigo devolveria também a morta; usar só pendentes perderia a respondida. `app/api/views.py:1058`, `app/chamados/tests/test_e3_v4_orm.py:42`, `app/chamados/tests/test_e3_v4_orm.py:59`, `app/chamados/tests/test_e3_v4_orm.py:66`.

- **UNIDADE literal não chegou a zero:** relatório ainda calcula C6 + sem resposta para `AGUARDA_COLAB`; pauta semanal ainda calcula `q_vivas + respondida_em NULL + disputa aberta`; painel/placar ainda separa respondida/não respondida em `creditos_perguntas`. São leitores externos à consulta central. A classificação como F1/F2 preexistentes ou violação nova depende da triagem/leis não disponibilizadas na cópia [nao verificado]. `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/cobranca.py:228`, `app/chamados/services/cobranca.py:257`, `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/verbos.py:34`.

- **UI web conserva cálculo próprio:** `fio` carrega perguntas da disputa aberta sem C6 naquele ramo; template chama `p.viva` e lê `respondida_em` para mostrar/reenvio; `todas_respondidas` também é derivado localmente. Essa trilha não consulta `consulta_do_app`. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:235`, `app/chamados/services/fio.py:340`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:155`.

- **FILA adicional, se a lei pretende abranger toda indicação de pendência:** respostas administrativas ainda contam sem resposta diretamente ao reabrir questionário e no fallback de validar inline, podendo anunciar “aguardando colaborador” para pergunta morta; o segundo nem sequer aplica q_vivas. Nenhum desses sítios aparece no diff total fornecido; autoria nova não demonstrada. `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:99`, `app/chamados/views_cobrar.py:175`, `app/chamados/views_cobrar.py:192`.

- **Nomes de fila F1/F2 e leis literais:** arquivos `triagem_preverif_v3b.txt`, `aval_lei_e3_unidade_c17.txt`, `aval_lei_c17_um_juiz.txt` e `leis_sitios_v6.txt` não localizados pela busca na cópia; conteúdo e correspondência exata dos rótulos [nao verificado].

## 09/11 15:39 · sessao ef93569ce1ce · - · - · origem=Ronald (tmux)

Escolha técnica recomendada para P3: **“run na linha diária” = intervalo reconstituído de um `ini` cuja contribuição de tempo ao dia consultado é estritamente positiva**. A aquisição existe mesmo com 0 segundos, mas não aumenta este contador de ocupação. Esta definição preserva o código e o selo atuais; a docstring precisa deixar de prometer inclusão de todo órfão. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:68`; escolha técnica: `CLAUDE.md:273`)

**C — matriz do histórico**

Use `d = max(min(fim, fim_dia) − max(ini, ini_dia), 0)`; números abaixo são dedução do juiz atual, sem execução do selo. (`bin/codex_vagas_medida.py:125`)

| Célula | Escritor → leitor | Número esperado pela definição acima / atual | Rótulo e prova |
|---|---|---|---|
| C1 `ini A` + `fim A` | Após adquirir flock escreve `ini`; trap escreve `fim` e rc. Leitor fecha somente se `quem` coincide. (`bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`, `bin/codex_vagas_medida.py:144`) | `d>0`: `(d,1,0)`; `d=0`: `(0,0,0)`, esperado=atual. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`) | “trava de suite presa … em 1 run(s) (0 ini sem fim)”; selo testa 2 cópias/2 runs e preservação de rc7/ini/fim. (`bin/codex_vagas_medida.py:203`, `bin/tests/test_trava_hist_unico.sh:40`, `bin/tests/test_trava_hist_unico.sh:44`, `bin/tests/test_trava_teste.sh:146`, `bin/tests/test_trava_teste.sh:148`) |
| C2 `ini A` sem fim seguido de `ini B` | Próximo `ini` fecha o intervalo estimado de A, incrementa órfãos e abre B. (`bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:142`, `bin/codex_vagas_medida.py:143`) | A contribui `d(A,B)` e 1 run somente se positivo; órfãos +1. Selo A01:00/B02:00/fimB02:30/C02:46:40, trava solta: `(5400,2,2)`. (`bin/codex_vagas_medida.py:125`, `bin/tests/test_trava_hist_unico.sh:54`, `bin/tests/test_trava_hist_unico.sh:57`) | Estimativa pode incluir intervalo ocioso A→B: o código não sabe quando A soltou. Não chamar esse total de duração factual exata. (`bin/codex_vagas_medida.py:111`, `bin/codex_vagas_medida.py:142`) Mutante existente remove soma do órfão e exige falha 0h30/1 run. (`bin/tests/test_trava_hist_unico.sh:82`) |
| C3 último `ini` sem fim, trava presa | Leitor consulta `--presa` após terminar leitura; `--presa` tenta o próprio flock e devolve rc0/presa ou rc1/livre. (`bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:39`, `bin/trava_teste.sh:40`) | Se presa: soma até agora recortado ao dia, n+1 se d>0, órfãos+1. Selo: `(81800,3,2)`, mostrado 22h43m. (`bin/codex_vagas_medida.py:147`, `bin/codex_vagas_medida.py:153`, `bin/tests/test_trava_hist_unico.sh:68`) | Vida significa “trava presa agora”, não identificação do antigo `quem`; corrida nova aquisição/novo `ini` permanece possível. (`bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`) F7 permanece fila fornecida, ocorrência real `[não verificado]`. |
| C4 último `ini` sem fim, trava solta | Não chama `soma`; só órfãos++. (`bin/codex_vagas_medida.py:147`, `bin/codex_vagas_medida.py:152`) | Isolado: `(0,0,1)`; esperado=atual segundo definição escolhida. **DIVERGE P3 na declaração** “todo ini sem fim entra no n de runs”. (`bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`) | Selo já exige exclusão do último solto; mutante existente ignora estado do lock e deve falhar com 22h43/3. (`bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:83`) |
| C5 histórico ausente | `--hist` devolve o caminho, sem verificar existência; leitor abre e lança exceção; `_parte` converte em “nao sei”. (`bin/trava_teste.sh:38`, `bin/codex_vagas_medida.py:115`, `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`) | Esperado/atual: desconhecido, não `(0,0,0)`; linha diária segue rc0, com componente desconhecido. (`app/docs/LEIS.md:61`, `bin/codex_vagas_medida.py:213`, `bin/codex_vagas_medida.py:233`) | Mutante `--hist`→`--quem` já exige “nao sei”, mas não há caso específico de histórico ausente na lista atual. (`bin/tests/test_trava_hist_unico.sh:84`, `bin/tests/test_trava_hist_unico.sh:39`, `bin/tests/test_trava_hist_unico.sh:51`) |
| C6 relógio regressivo | Escritor usa epoch civil `date +%s`; leitor não rejeita timestamp menor que anterior nem une intervalos. (`bin/trava_teste.sh:32`, `bin/codex_vagas_medida.py:136`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:145`) | Sequência `ini A 0/fim A 86400/ini B 0/fim B 86400` produz `(172800,2,0)` para dia de 86400s. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:127`) | Rótulo atual pode imprimir 48h; tratamento restritivo de regressão é F6 da triagem, não P novo. Não há teste/mutante correspondente no selo atual. (`bin/codex_vagas_medida.py:203`, `bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`) Ocorrência real `[não verificado]`. |
| C7 cruza meia-noite | Mesmos eventos; dia inicia à meia-noite local, fim é ini+86400; soma recorta intervalo. (`bin/codex_vagas_medida.py:119`, `bin/codex_vagas_medida.py:120`, `bin/codex_vagas_medida.py:125`) | `23:50→00:10`: `(600,1,0)` em cada dia regular de 86400s; n diário não é contador global de execuções únicas. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`) | Caso direto/mutante de retirar recorte ainda exigidos `[não verificado]`; selo atual só usa eventos no mesmo dia. (`bin/tests/test_trava_hist_unico.sh:54`) |

**Precisões do contrato C:** `orfaos` conta todos os órfãos do histórico lido, independentemente do dia consultado, pois incrementa fora do recorte diário; o rótulo atual apenas diz “N ini sem fim”. Não declarar esse número como “órfãos iniciados no dia”. (`bin/codex_vagas_medida.py:141`, `bin/codex_vagas_medida.py:148`, `bin/codex_vagas_medida.py:203`)

`--presa` toma exclusividade brevemente quando consegue o flock; não prova FIFO ou impossibilidade de disputar com chamador real. O real espera `-w`, a sonda tenta `-n`; impacto real `[não verificado]`. (`bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`)

**D — classificador**

| Célula | Esperado | Atual | Prova |
|---|---|---|---|
| D1 morte pela mensagem declarada | MORTO quando todas as falhas declaradas aparecem como asserções do caso. (`bin/tests/test_codex_pergunta.sh:640`, `bin/tests/test_codex_pergunta.sh:641`) | MORTO se cada substring `"FALHA: $e"` aparece em algum lugar da saída. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:748`) | Autoteste positivo atual. (`bin/tests/test_codex_pergunta.sh:752`) |
| D2 eco em diagnóstico | Só diagnóstico, sem linha real de asserção: VIVO; com falha real diferente: ERRADO. É exigência de endurecimento F5, não reprovação P nova. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:642`; definição da linha real a fixar `[não verificado]`) | **DIVERGE F5:** `diagnostico: FALHA: <esperado>` conta MORTO, porque buscas não ancoram linha nem distinguem produtor. (`bin/tests/test_codex_pergunta.sh:744`, `bin/tests/test_codex_pergunta.sh:747`) | Autotestes atuais não incluem eco. Mutante atual que produza efetivamente esse eco `[não verificado]`. (`bin/tests/test_codex_pergunta.sh:750`, `bin/tests/test_codex_pergunta.sh:755`) |
| D3 mais de uma mensagem | Todos os esperados exigidos; esperado+outras falhas ainda MORTO; faltou algum=ERRADO; nenhum evento de falha=VIVO. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:642`) | Igual, ressalva D2: cada esperado é substring; quantidade de falhas extras não impede MORTO. (`bin/tests/test_codex_pergunta.sh:744`, `bin/tests/test_codex_pergunta.sh:747`) | Autoteste “dois esperados/bastou um” exige ERRADO; vazio exige ERRADO; arquivo vazio exige VIVO. Falta positivo com ambos em linhas distintas. (`bin/tests/test_codex_pergunta.sh:753`, `bin/tests/test_codex_pergunta.sh:754`, `bin/tests/test_codex_pergunta.sh:755`) |
| D4 mensagem em outra guarda | Outra asserção sem a declarada=ERRADO; mensagem deve identificar a guarda pretendida. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:751`) | Outra asserção distinta=ERRADO; mensagem textual idêntica não tem identidade adicional no classificador. (`bin/tests/test_codex_pergunta.sh:747`) O acidente concreto `saida_symlink` foi desambiguado no caso. (`bin/tests/test_codex_pergunta.sh:201`) | Guarda de saída escreve `saida … e symlink`; a seguinte escreve `codex: trava … e symlink`. (`bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`) Mutante `saida_symlink` exige falha de motivo começando `'saida`. (`bin/tests/test_codex_pergunta.sh:666`) |

ERRADO e VIVO aumentam `falhas`; não contam como sucesso do selo. (`bin/tests/test_codex_pergunta.sh:777`, `bin/tests/test_codex_pergunta.sh:779`, `bin/tests/test_codex_pergunta.sh:787`)

**P4 é divergência de prova em B/load e D/declaração:** cenário usa `NPL=nproc−2` e mensagem dinâmica; duas declarações de mutantes exigem literalmente `load5 6.00`. Se nproc≠8, a falha pretendida produz outro número e recebe ERRADO. (`bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:102`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`, `bin/tests/test_codex_pergunta.sh:747`) nproc ou falso vermelho ocorrido na produção `[não verificado]`.

**E — testes/mutantes necessários deste recorte** [nao verificado]

1. `hist_unico_duas_copias` — `bin/tests/test_trava_hist_unico.sh:39`; C1/fonte única; mutantes já existentes “HIST relativo à cópia” e `--hist→--quem`. (`bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`)
2. `hist_orfao_fecha_no_proximo_ini` — `bin/tests/test_trava_hist_unico.sh:54`; C2; mutante já existente remove soma do órfão. (`bin/tests/test_trava_hist_unico.sh:82`)
3. `hist_ultimo_orfao_preso_solto` — `bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:68`; C3/C4; mutante já existente conta até agora mesmo solto. (`bin/tests/test_trava_hist_unico.sh:83`)
4. `hist_run_contribuinte_contrato_unico` — mesmo selo; C4/P3: exigir declaração do significado escolhido e tuplas `(0,0,1)` para órfão solto, `(0,0,0)` para ini/fim no mesmo instante, positivo=1. Mutante exigido: contar run sem contribuição positiva. Teste/mutante novos `[não verificado]`; sítios atuais da contradição `bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`.
5. `hist_ausente_nao_sei` — mesmo selo; C5; mutante exigido troca exceção por `(0,0,0)`. Novo `[não verificado]`; juiz atual `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`.
6. `hist_recorte_meia_noite` — mesmo selo; C7; mutante exigido retira recorte diário. Novo `[não verificado]`; juiz atual `bin/codex_vagas_medida.py:125`.
7. `classifica_eco_diagnostico` — `bin/tests/test_codex_pergunta.sh`, D2/F5; diagnóstico ecoando esperado sozinho deve ser VIVO; diagnóstico+asserção alheia deve ser ERRADO. Mutante exigido restaura busca sem âncora. Novo `[não verificado]`; sítio atual `bin/tests/test_codex_pergunta.sh:747`.
8. `classifica_todos_e_somente_morte_declarada` — mesmo selo; D1/D3/D4; conservar autotestes atuais e acrescentar dois esperados presentes e esperado+falha extra. Mutante `echo MORTO`, mutante que aceita só um esperado, mutante que aceita qualquer FALHA. Cobertura atual parcial `bin/tests/test_codex_pergunta.sh:751`, `bin/tests/test_codex_pergunta.sh:755`; novos cenários/mutantes `[não verificado]`.
9. `v2_carga_portavel` — mesmo selo; B/load/P4; cenário e esperado precisam falar do mesmo `NPL`, exercitado com nproc≠8. Mutantes atuais `portao` e `carga` devem continuar morrer pela falha dinâmica, e mutante exigido recrava 6.00 para morder portabilidade. Atual `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`; novo teste de portabilidade `[não verificado]`.
10. `trava_hist_preserva_rc` — `bin/tests/test_trava_teste.sh:146`; C1/escritor; comando exit7 deve continuar rc7 e fim rc7. Mutantes exigidos remover `ini`, remover `fim`, engolir rc. Teste atual `bin/tests/test_trava_teste.sh:147`, `bin/tests/test_trava_teste.sh:148`; mutantes `[não verificado]`.

F6 relógio regressivo e F7 identidade/race são filas do dado; não alegar que os mutantes atuais os cobrem. A lista atual tem só os quatro mutantes acima. (`bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`)

**F — escritores/leitores deste recorte**

| Campo/arquivo | Escritor → leitor |
|---|---|
| `TRAVA` | Definido em `bin/trava_teste.sh:23`; aquisição de execução fd9 em `bin/trava_teste.sh:47`, `bin/trava_teste.sh:48`; sonda fd8 em `bin/trava_teste.sh:39`. |
| `HIST`/caminho | Derivado uma vez em `bin/trava_teste.sh:31`; publicado por `--hist` em `bin/trava_teste.sh:38`; medida pergunta em `bin/codex_vagas_medida.py:115`; selo pergunta em `bin/tests/test_trava_hist_unico.sh:47`; selo antigo consulta antes de leitura direta em `bin/tests/test_trava_teste.sh:112`, `bin/tests/test_trava_teste.sh:148`. |
| Histórico `epoch,evento,quem,rc` | `_hist` escreve best effort em `bin/trava_teste.sh:32`; ini/fim chamados em `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`; medida lê epoch/evento/quem em `bin/codex_vagas_medida.py:132`, `bin/codex_vagas_medida.py:136`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:144`; rc não entra na soma. |
| `TRAVA.quem` | Escritor antes da execução e trap em `bin/trava_teste.sh:52`, `bin/trava_teste.sh:54`; leitores `--quem` e diagnóstico timeout em `bin/trava_teste.sh:37`, `bin/trava_teste.sh:49`. |
| `segundos,n,orfaos` | `_ocupacao_trava` produz em `bin/codex_vagas_medida.py:154`; linha diária lê e rotula em `bin/codex_vagas_medida.py:202`, `bin/codex_vagas_medida.py:203`; selo lê texto em `bin/tests/test_trava_hist_unico.sh:22`. |
| `chamadas.log`/vaga2 | Medida filtra data, evento ini e último campo `vaga=2` em `bin/codex_vagas_medida.py:160`, `bin/codex_vagas_medida.py:164`; contagem de recusas lê `vaga2.log` por prefixo data em `bin/codex_vagas_medida.py:166`, `bin/codex_vagas_medida.py:168`. |
| SAR média/máximo/n | `sar -q` ou arquivo em `bin/codex_vagas_medida.py:38`, `bin/codex_vagas_medida.py:43`; parser do cabeçalho em `bin/codex_vagas_medida.py:57`; consumidores desligador e linha em `bin/codex_vagas_medida.py:88`, `bin/codex_vagas_medida.py:198`. |
| Sessões `timestamp,primary.used_percent,window_minutes,resets_at` | Produtor CLI declarado em `bin/codex_vagas_medida.py:15`; escrita efetiva CLI `[não verificado]`; leitor canônico em `bin/codex_limite_estado.py:154`, `bin/codex_limite_estado.py:155`, `bin/codex_limite_estado.py:159`, `bin/codex_limite_estado.py:161`; medida reutiliza em `bin/codex_vagas_medida.py:78`; desligador consome em `bin/codex_vagas_medida.py:89`, `bin/codex_vagas_medida.py:95`. |
| `cota.json`: `seven_day_pct,seven_day_resets_at,hora` | Statusline escreve por replace em `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`; medida lê atual/anterior e compara reset/hora em `bin/codex_vagas_medida.py:174`, `bin/codex_vagas_medida.py:178`, `bin/codex_vagas_medida.py:180`, `bin/codex_vagas_medida.py:182`; disjuntor lê pct/reset em `bin/disjuntor.py:1167`. Produtor da foto anterior `[não verificado]`. |
| Mutantes/esperados/saída/tabela | Declaração em `bin/tests/test_codex_pergunta.sh:643`; fábrica em `bin/tests/test_codex_pergunta.sh:769`; caso produz saída em `bin/tests/test_codex_pergunta.sh:773`; classificador lê em `bin/tests/test_codex_pergunta.sh:747`; tabela escrita em `bin/tests/test_codex_pergunta.sh:776`; selo histórico usa sua própria busca substring em `bin/tests/test_trava_hist_unico.sh:97` (mesma fragilidade de eco). |

Não executei nenhum selo nem consulta real de `--presa`; os próprios selos criam arquivos e `--presa` abre o lock com append. (`bin/tests/test_trava_hist_unico.sh:18`, `bin/tests/test_codex_pergunta.sh:21`, `bin/trava_teste.sh:39`)

## 09/11 15:38 · sessao 3e8b758c395a · - · - · origem=Ronald (tmux)

Matriz B — “E=A” significa esperado igual ao atual; admissão pressupõe validações anteriores verdes. O critério de simultaneidade é arquivo comum, não app, e o teto é dois construtores. (`CLAUDE.md:306`, `CLAUDE.md:288`; `bin/codex_pergunta.sh:375`)

| Linha | Juiz → admite/recusa | Persistido | Retorno | Prova atual / lacuna |
|---|---|---|---|---|
| 1ª chamada, nenhuma outra viva | E=A: sob meta-trava, tenta vaga 1; outra livre dispensa portões; publica estado. (`bin/codex_pergunta.sh:380`, `:383`, `:390`, `:392`) | Estado publicado por rename; depois `ini` com vaga; no EXIT `fim`, `.rc`, remoção de estado. (`bin/codex_pergunta.sh:310`, `:311`, `:428`, `:229`, `:232`, `:236`) | Admissão não determina rc final; controle positivo termina 0. (`bin/codex_pergunta.sh:609`; `bin/tests/test_codex_pergunta.sh:315`) | `c_positivo`, `c_v2_admite`; mutantes de guardas operacionais, porém controle explícito “portões vermelhos com primeira vaga livre ainda admite” inexistente na lista atual. (`bin/tests/test_codex_pergunta.sh:315`, `:506`, `:611`) |
| 2ª chamada, primeira viva | E=A: ocupa vaga 2 e passa todos os portões, depois publica estado. (`bin/codex_pergunta.sh:385`, `:390`, `:391`, `:392`) | `.estado`, `chamadas.log ini/fim vaga=2`; estado sai no EXIT. (`bin/codex_pergunta.sh:311`, `:428`, `:229`, `:236`) | 0 se execução/inventário verdes; caso recusa portão: 2, `VAGA2:`. (`bin/codex_pergunta.sh:250`, `:587`, `:609`) | `c_v2_admite`; mutantes `portao`, `carga`, `janela`, `desligador`, `toca`, `estado_outra`, `pausa_segunda`. (`bin/tests/test_codex_pergunta.sh:506`, `:720`) |
| 3ª, duas vivas | E=A: falham os dois flocks, recusa teto dois. (`bin/codex_pergunta.sh:383`, `:386`) | Uma tentativa de append `vaga2.log`; não publica estado nem alcança `chamadas.log ini`. (`bin/codex_pergunta.sh:249`, `:386`, `:392`, `:428`) | 2, `VAGA2: vaga ... ocupada nas 2 vagas`. (`bin/codex_pergunta.sh:250`, `:386`) | `c_vaga`, inclusive outro LOGD; mutante `vaga`. (`bin/tests/test_codex_pergunta.sh:239`, `:679`) |
| load5 abaixo de nproc−2 | E=A: leitura segunda coluna e comparação estrita; aceita este portão se nproc>2. (`bin/codex_pergunta.sh:297`, `:298`) | Admitida segue estado/chamadas; recusas posteriores registram load lido. (`bin/codex_pergunta.sh:249`, `:311`, `:428`) | Não fixa rc final. (`bin/codex_pergunta.sh:609`) | Fixture `load_1`; caso de admissão. Não há matriz explícita abaixo/igual/acima por nproc simulado. (`bin/tests/test_codex_pergunta.sh:100`, `:101`, `:506`) |
| load5 igual | E=A no wrapper: recusa igualdade. **DIVERGE no selo P4**: esperado de mutantes crava 6.00, cenário usa nproc−2. (`bin/codex_pergunta.sh:298`; `bin/tests/test_codex_pergunta.sh:100`, `:520`, `:720`, `:722`) | `vaga2.log` com motivo, `load5=NPL.00`, fonte teste. (`bin/codex_pergunta.sh:249`; `bin/tests/test_codex_pergunta.sh:521`) | Wrapper 2; classificador pode ERRADO quando NPL≠6. (`bin/codex_pergunta.sh:250`; `bin/tests/test_codex_pergunta.sh:747`, `:750`, `:720`) | `c_v2_carga`; mutantes `portao`, `carga`, `log_segunda`. (`bin/tests/test_codex_pergunta.sh:519`, `:720`, `:722`, `:728`) |
| load5 acima | E=A: recusa. (`bin/codex_pergunta.sh:298`) | `vaga2.log`; sem estado desta chamada/ini. (`bin/codex_pergunta.sh:249`, `:306`, `:392`, `:428`) | 2, mensagem load ≥ limiar ou ilegível. (`bin/codex_pergunta.sh:299`, `:250`) | Caso específico acima não consta; atual `c_v2_carga` cobre igualdade. (`bin/tests/test_codex_pergunta.sh:519`) |
| load real / cravado | E=A: default `/proc/loadavg`; teste pode substituir arquivo sob guarda topológica; fonte `real/teste`. (`bin/codex_pergunta.sh:111`, `:112`, `:114`, `:116`, `:118`) | Fonte vai ao `vaga2.log`; load de admissão não é relido durante execução. (`bin/codex_pergunta.sh:249`, `:297`, `:565`) | Env teste em topologia proibida: 2 antes da admissão. (`bin/codex_pergunta.sh:114`) | `c_teste_guarda`, mutante `teste_guarda`; fixtures falsas em todas chamadas `_roda`. (`bin/tests/test_codex_pergunta.sh:153`, `:598`, `:729`) |
| 06:00 | E=A: recusa. (`bin/codex_pergunta.sh:301`) | `vaga2.log`, motivo janela. (`bin/codex_pergunta.sh:249`, `:302`) | 2, `VAGA2: janela 06:00-07:29...`. (`bin/codex_pergunta.sh:250`, `:302`) | `c_v2_janela/0600`; mutante janela. (`bin/tests/test_codex_pergunta.sh:524`, `:723`) |
| 07:29 | E=A: recusa. (`bin/codex_pergunta.sh:301`) | Igual linha anterior. (`bin/codex_pergunta.sh:249`) | 2. (`bin/codex_pergunta.sh:250`) | `c_v2_janela/0729`. (`bin/tests/test_codex_pergunta.sh:525`) |
| 07:30 | E=A: passa janela. (`bin/codex_pergunta.sh:301`) | Estado/chamadas se demais verdes. (`bin/codex_pergunta.sh:392`, `:428`) | 0 no controle. (`bin/tests/test_codex_pergunta.sh:526`, `:493`) | `c_v2_janela/0730`. (`bin/tests/test_codex_pergunta.sh:526`) |
| Desligador ilegível | E=A: medida retorna “não sei” rc2; wrapper converte todo não zero em recusa. (`bin/codex_vagas_medida.py:90`, `:91`; `bin/codex_pergunta.sh:303`, `:304`) | `vaga2.log` motivo desligador, última linha limitada a 200 caracteres. (`bin/codex_pergunta.sh:249`, `:304`) | Wrapper 2 com `VAGA2: desligador: nao sei...`. (`bin/codex_pergunta.sh:250`, `:304`) | `c_v2_desligador/sem_leitura` e `/sem_sar`; mutante desligador hoje exige falha de cenário sar alto, não individualmente das duas fontes ausentes. (`bin/tests/test_codex_pergunta.sh:535`, `:536`, `:724`) |
| Vaga órfã após SIGKILL | E=A quanto ao juiz: estado/pid não provam vida, `_viva` tenta flock; lock livre permite reutilizar e substituir estado; lock preso com estado ilegível recusa. (`bin/codex_pergunta.sh:253`, `:255`, `:291`, `:296`, `:311`, `:383`) | SIGKILL não possui trap próprio; único cleanup é EXIT, logo não há promessa demonstrada de estado/fim após kill9. (`bin/codex_pergunta.sh:226`, `:236`, `:240`; execução SIGKILL [não verificado]) | Nova tentativa depende do flock real, não existência do estado. (`bin/codex_pergunta.sh:390`, `:391`) | Sem caso explícito SIGKILL na lista de casos. (`bin/tests/test_codex_pergunta.sh:611`) |
| Meta ocupada na admissão | E=A: espera default10 s, teste1 s; falhando recusa. (`bin/codex_pergunta.sh:119`, `:381`; `bin/tests/test_codex_pergunta.sh:548`) | `vaga2.log`, `load5=-` pois carga ainda não lida; sem estado/ini. (`bin/codex_pergunta.sh:247`, `:249`, `:381`, `:392`, `:428`) | 2, `VAGA2: meta-trava...`. (`bin/codex_pergunta.sh:250`, `:381`) | `c_meta_trava`; mutante `meta_trava`. (`bin/tests/test_codex_pergunta.sh:546`, `:719`) |
| Meta ocupada no fim | E=A restritivo: inventário ganha `FALHA vagas meta-trava ... nao presa no fim`; `_IRC=3`. (`bin/codex_pergunta.sh:565`, `:572`) | `.inventario`, `.recusa`, `.rc rc3`, `fim`; estado removido no EXIT. (`bin/codex_pergunta.sh:572`, `:579`, `:232`, `:229`, `:236`) | 3 prevalece. (`bin/codex_pergunta.sh:587`) | Não há caso próprio nem #G próprio na linha de falha do fim; mutante futuro deve neutralizar essa recusa e morrer pela falta da FALHA declarada. (`bin/codex_pergunta.sh:572`; `bin/tests/test_codex_pergunta.sh:611`, `:719`) |
| rc3 na saída | E=A: inventário proibido/falha vence qualquer RC; grava recusa e sai3; `.id` não nasce. (`bin/codex_pergunta.sh:557`, `:576`, `:579`, `:587`, `:603`) | `.inventario`, `.recusa`, `.rc3`, chamadas fim, limpa estado. (`bin/codex_pergunta.sh:560`, `:579`, `:232`, `:229`, `:236`) | 3 + `RECUSA rc 3... wt DESCARTADA`. (`bin/codex_pergunta.sh:578`, `:587`) | `_inv_recusa`, casos de inventário, mutante `inventario`. (`bin/tests/test_codex_pergunta.sh:169`, `:374`, `:693`) |
| Timeout do Codex | E=A pelo fluxo: timeout com kill adicional após30; captura RC e sempre inventaria quando retorna controle; inventário ruim vence, senão RC retornado;124 não vira LIMITE. (`bin/codex_pergunta.sh:541`, `:551`, `:560`, `:587`, `:588`, `:609`) | `.inventario`; `.recusa` só inventário ruim; `.rc/fim` no EXIT, sem `.id` se RC≠0. (`bin/codex_pergunta.sh:560`, `:576`, `:603`, `:232`, `:229`) | 124 quando timeout retorna124 e inventário verde;137 possível depende do timeout/processos [não verificado]; nunca afirmar timeout=3 sempre. (`bin/codex_pergunta.sh:551`, `:587`, `:588`, `:609`) | Não há cenário timeout próprio na lista. (`bin/tests/test_codex_pergunta.sh:611`) |
| kill9 do wrapper | Expectativa revisável: sem conclusão verde nem lastro novo completo [não verificado]. Atual: só há EXIT trap; sem caminho de inventário independente do wrapper. (`bin/codex_pergunta.sh:240`, `:560`, `:603`) | Estado e `ini` podem sobrar; `.inventario/.rc/fim` não são garantidos pelo código nessa morte. (`bin/codex_pergunta.sh:236`, `:428`, `:560`, `:232`; ocorrência [não verificado]) | Wrapper morto não emite `RECUSA` nem `sessao=...`; status observado pelo pai [não verificado]. (`bin/codex_pergunta.sh:578`, `:608`) | Necessário caso isolado de kill9 para provar lastro ausente e decisão pelo flock [não verificado]. |

**Cuidado com herança do lock:** fd6 é aberto e adquirido em `:382/:386` e continua aberto ao lançar `timeout ... codex` em `:541`; não há fechamento explícito pré-filho. Portanto não afirmar “matou o wrapper, a vaga soltou imediatamente”: filhos podem manter descritor; a prova útil permanece `_viva`. (`bin/codex_pergunta.sh:382`, `:386`, `:541`, `:253`; retenção efetiva por Codex [não verificado])

F — escritores/leitores de B:

| Campo/arquivo | Escritor → leitor |
|---|---|
| Caminhos vaga1/vaga2/meta/estados | Git common-dir → wrapper deriva nomes e reserva respostas; aquisição e consulta usam esses nomes. (`bin/codex_pergunta.sh:81`, `:85`, `:93`, `:97`, `:380`, `:389`) |
| Flock da vaga | Wrapper fd6 → `_viva` abre outro fd, tenta não bloquear; falha de abertura conta viva. (`bin/codex_pergunta.sh:382`, `:383`, `:386`, `:254`, `:255`) |
| Meta-flock | Wrapper fd3 → admissão e snapshot do fim; fecha antes do Codex e após leitura final. (`bin/codex_pergunta.sh:380`, `:381`, `:393`, `:565`, `:570`) |
| `.estado` `vaga,pid,obra,wt,hash,hora,toca` | `_publica_estado` grava temporário e renomeia; `_toca_de` lê apenas linhas `toca=`; `_saida` remove. Não há leitor desses outros campos no juiz de admissão/fim. (`bin/codex_pergunta.sh:310`, `:311`, `:291`, `:295`, `:567`, `:236`) |
| Load5 | Kernel `/proc/loadavg` ou fixture → awk segunda coluna, portão compara com `nproc`; recusa escreve valor. (`bin/codex_pergunta.sh:118`, `:297`, `:298`, `:249`) |
| Hora | `TZ=America/Sao_Paulo date +%H%M` ou fixture → juiz janela. (`bin/codex_pergunta.sh:300`, `:301`) |
| Pausas | Produtor externo não examinado [não verificado] → wrapper consulta `codex-construtor` e `codex-vaga2`. (`bin/codex_pergunta.sh:328`, `:294`) |
| Sar | `sar -q` real ou arquivo teste → parser média/máximo/amostras → desligador média>7. (`bin/codex_vagas_medida.py:38`, `:43`, `:49`, `:93`) |
| Codex used/reset | Leitor semanal canônico → desligador, >85 com reset futuro/ausente desliga. Escrita real do CLI [não verificado]. (`bin/codex_vagas_medida.py:75`, `:78`, `:95`, `:96`) |
| `vaga2.log` hora/obra/motivo/load/fonte | `_recusa_v2` append → contador diário e selo; append é best effort, falha não altera rc2. (`bin/codex_pergunta.sh:249`, `:250`; `bin/codex_vagas_medida.py:166`, `:168`; `bin/tests/test_codex_pergunta.sh:503`) |
| `chamadas.log` hora/obra/papel/hash12/pid/evento/pergunta ou sessão/rc/duração/vaga | Wrapper ini/fim → leitor posicional canônico; medida conta `ini` cujo último campo é vaga2; disjuntor reutiliza leitor. (`bin/codex_pergunta.sh:428`, `:229`; `bin/codex_limite_estado.py:49`, `:63`, `:67`; `bin/codex_vagas_medida.py:164`; `bin/disjuntor.py:2121`) |
| `.inventario` | Inventário final + validação gitfile + snapshot e TOCA → decisão `_IRC`, recusa. (`bin/codex_pergunta.sh:560`, `:561`, `:568`, `:574`, `:576`) |
| `.recusa` | Wrapper filtra linhas não TOCADO → stderr e selo; instrução de descarte é mensagem, não remoção automática da WT. (`bin/codex_pergunta.sh:578`, `:579`, `:580`; `bin/tests/test_codex_pergunta.sh:171`) |
| `.rc` | EXIT publica por rename com id da chamada, rc, fim → selo; não é prova de vida. (`bin/codex_pergunta.sh:231`, `:232`, `:233`; `bin/tests/test_codex_pergunta.sh:174`, `:330`) |
| Resposta/JSONL/err e `.id` | Codex cria saída temporária/JSONL/stderr; wrapper move resposta e publica `.id` apenas RC0+resposta desta chamada. (`bin/codex_pergunta.sh:545`, `:553`, `:603`, `:604`) |

P4 deve entrar no contrato E como teste de portabilidade dos dois registros mutantes `portao/carga`: cenário e esperado precisam variar juntos com NPL, incluindo host simulado distinto de8; o mutante que retorna “6.00” fixo deve morrer pela asserção de esperado incorreto, não por outra falha. O defeito atual está nos literais; execução desse futuro teste [não verificado]. (`bin/tests/test_codex_pergunta.sh:100`, `:520`, `:720`, `:722`)

## 09/11 15:38 · sessao 11601509dba7 · - · - · origem=Ronald (tmux)

A matriz A pode usar estes sítios comuns: argumento `bin/codex_pergunta.sh:71`, `:321`, `:323`; admissão `:270`, `:305`; inventário `:196`, `:217`; leitura `:278`, `:279`, `:282`; juiz final `:284`, `:287`; estado `:310`, `:311`, leitura `:291`; persistência e retorno `:560`, `:574`, `:579`, `:587`, `:603`, `:609`. A lei de arquivo comum está em `CLAUDE.md:306`, `:307`; cura restritiva em `CLAUDE.md:278`.

| Linha A | Esperado × atual e localização |
|---|---|
| Arquivo exato | Concordam: argumento sem `/` cobre só igualdade (`bin/codex_pergunta.sh:264`); outro arquivo é admitido, mesmo arquivo recusa (`:266`, `:273`); TOCADO fora recusa rc 3 (`:284`, `:587`). Caso `c_toca_exato` em `bin/tests/test_codex_pergunta.sh:563`, `:565`; mutante `toca_exato` em `:733`, `toca_fim` em `:732`. |
| Pasta `/` | Concordam: prefixo inclui `/`, logo pasta cobre descendentes por componente (`bin/codex_pergunta.sh:264`). Casos `controle_pasta` e `folha_nao_cobre_folhas` em `bin/tests/test_codex_pergunta.sh:564`, `:567`; mutante `toca_exato` em `:733`. |
| Pasta pai criada para arquivo nomeado | Sem outra raia, concordam: pasta nova vira TOCADO (`bin/codex_pergunta.sh:196`), pai necessário é aceito (`:284`); caso existente em `bin/tests/test_codex_pergunta.sh:569`. Com meu `app/novo/a.py` e outro `app/novo/b.py`, **DIVERGE P2**: admissão disjunta (`bin/codex_pergunta.sh:266`), fim considera `app/novo/` comum com `b.py` (`:287`). Lei exige arquivo comum (`CLAUDE.md:306`). Falta caso concorrente; futuro `[não verificado]`. |
| Pasta irmã | Concordam: `app/folhas/` não cruza `app/folha/` (`bin/codex_pergunta.sh:264`, `:266`); caso admissão em `bin/tests/test_codex_pergunta.sh:530`; caso fim `:564`. |
| Mesmo nome com e sem `/` | Concordam na recusa restritiva da admissão: igualdade após remover `/` (`bin/codex_pergunta.sh:266`, `CLAUDE.md:278`); caso `app/folha` contra `app/folha/` em `bin/tests/test_codex_pergunta.sh:529`. Sem `/` continua arquivo exato no fim (`bin/codex_pergunta.sh:264`), não permissão para descendentes; caso `bin/tests/test_codex_pergunta.sh:563`. |
| TAB | Argumento recusa (`bin/codex_pergunta.sh:323`). Nome encontrado no FS não recebe validação equivalente: **DIVERGE P1**. Inventário imprime cru (`:217`); leitor separa TAB e usa só segundo campo (`:279`, `:282`). `app/folha/views.py<TAB>fora` passa por declaração exata `app/folha/views.py`. Falta caso/mutante próprio no CASOS/lista atuais (`bin/tests/test_codex_pergunta.sh:617`, `:618`, `:730`, `:737`). |
| Newline | Argumento recusa (`bin/codex_pergunta.sh:323`). **DIVERGE P1**: impressão crua cria outra linha (`:217`); leitura linha a linha aceita primeiro fragmento como nome exato (`:278`, `:282`), demais fragmentos sem classe TOCADO são ignorados (`:280`). Falta caso/mutante próprio (`bin/tests/test_codex_pergunta.sh:617`, `:618`). |
| CR | Argumento recusa (`bin/codex_pergunta.sh:323`). **DIVERGE P1**: `open` em modo texto usa leitura universal de quebras; CR do nome vira quebra antes da comparação (`:278`, `:282`); impressão é crua (`:217`). Falta caso/mutante próprio (`bin/tests/test_codex_pergunta.sh:617`, `:618`). |
| Espaço | Argumento recusa (`bin/codex_pergunta.sh:323`). No FS, TSV conserva o espaço; nome com espaço sob pasta declarada pode ser TOCADO permitido, enquanto `views.py espaço` está fora de declaração exata `views.py` (`:217`, `:279`, `:284`). Não confundir a gramática restritiva do argumento com proibição de espaço em todo arquivo descoberto. Caso específico atual `[não verificado]`. |
| Symlink tocado | Nome sintaticamente válido pode ser declarado (`bin/codex_pergunta.sh:323`); inventário usa `lstat` (`:194`), marca symlink PROIBIDO (`:215`), rc 3 (`:219`, `:587`). Caso `c_p_link`/mutante `p_link`: `bin/tests/test_codex_pergunta.sh:366`, `:663`. Symlink transitório desfeito antes do fim `[não verificado]`. |
| Hardlink | Preexistente entra em TOCADO mesmo com conteúdo igual (`bin/codex_pergunta.sh:201`), causando pré-check não vazio rc 2 (`:397`, `:399`); criado no trabalho vira PROIBIDO (`:216`) e rc 3 (`:587`). Casos `p_hardlink`, `p_hardlink_ident`, `revalida_hardlink`: `bin/tests/test_codex_pergunta.sh:367`, `:403`, `:406`; mutantes `inv_nlink`/`p_hardlink`: `:656`, `:657`, `:658`, `:664`. |
| `..`, `//`, absoluto | Argumentos recusados antes da admissão (`bin/codex_pergunta.sh:323`, `:324`). Casos atuais `bin/tests/test_codex_pergunta.sh:554`; mutante `toca_forma` `:731`. Inventário produz caminhos relativos da WT (`bin/codex_pergunta.sh:189`, `:193`), não a grafia original de uma operação. Escape transitório ou escrita fora da WT `[não verificado]`. |
| Fora de `app/` e `bin/` | Argumento recusa (`bin/codex_pergunta.sh:323`); arquivo encontrado dentro da WT mas fora dessas raízes é PROIBIDO (`:151`). Caso `p_fora` `bin/tests/test_codex_pergunta.sh:352`, mutante `:689`. |
| `bin/` comum | Permitido quando coberto pelo próprio TOCA e sem cruzamento (`bin/codex_pergunta.sh:151`, `:284`, `:287`); writable root extra é ativado por declaração `bin/...` (`:539`, `:543`). Caso `bin_ok` `bin/tests/test_codex_pergunta.sh:582`. Mutante específico para concessão do writable root `[não verificado]`. |
| `bin/` protegido | Argumento pode passar e admissão também; inventário proíbe independentemente do TOCA (`bin/codex_pergunta.sh:157`, `:162`, `:210`). Casos todos protegidos `bin/tests/test_codex_pergunta.sh:590`, `:592`; mutantes `lista_protegida` e `p_protegido` `:736`, `:737`. |
| `.claude/` | Raiz `.claude/` é argumento inválido; `app/.claude/...` ou `bin/.claude/...` são argumentos sintaticamente válidos (`bin/codex_pergunta.sh:323`), porém inventário sempre proíbe componente `.claude` (`:147`). Caso `p_claude` `bin/tests/test_codex_pergunta.sh:348`; mutante `:685`. |

Confirmei em memória, sem criar arquivos, o trecho real `_TOCA_PY`: `views.py<TAB>fora`, `views.py<NL>fora` e `views.py<CR>fora` retornam 0 contra declaração exata `views.py`; `views.py espaço` retorna 3. Os sítios que determinam os resultados são `bin/codex_pergunta.sh:217`, `:278`, `:279`, `:282`, `:284`.

Para P2, o teste novo deve provar **admissão e fim rc 0** para irmãos novos `a.py`/`b.py`, e também **fim rc 3** quando a outra declaração passa a cobrir `app/novo/`; os controles distinguem arquivo comum de pai necessário (`CLAUDE.md:306`, `bin/codex_pergunta.sh:284`, `:287`). O mutante exigido deve restaurar a colisão simétrica sobre a pasta pai e morrer no controle positivo; outro deve eliminar a recusa quando a outra raia cobre a pasta e morrer no controle negativo. Esses casos/mutantes futuros estão `[não verificado]`.

Persistência comum: argumento válido admitido é publicado como linhas `toca=` cruas e rename (`bin/codex_pergunta.sh:310`, `:311`), relido por `sed` (`:291`); nomes do inventário são TSV cru (`:217`), acrescidos de PROIBIDO pelo juiz final (`:575`), e `.recusa` retém linhas que não começam com TOCADO (`:579`). Em saída normal, estado é removido (`:236`), chamadas.log ganha `fim` (`:229`), `.rc` registra o retorno (`:232`); inventário proibido força 3 (`:587`), sucesso com resposta nova produz `.id` (`:603`). Como o filtro é por linha crua, fragmentos de nome com newline podem sobreviver em `.recusa` sem estrutura de registro (`:217`, `:579`).

A célula precisa de ESMERIL-SEMÂNTICA é **pai novo × outra declaração de arquivo irmão no juiz final**: admissão diz disjunto (`bin/codex_pergunta.sh:266`, `:273`), fim diz arquivo comum por causa da pasta TOCADA (`:196`, `:287`). P1 acrescenta divergência de representação: argumento validado não contém controles (`:323`), mas inventário e leitor não preservam o nome encontrado (`:217`, `:278`, `:282`).

## 09/11 15:34 · sessao 6b3547444afa · - · - · origem=Ronald (tmux)

Censo textual runtime completo de chamadas diretas nesta cópia:

| Chamador | Dependência da forma atual | Adaptação necessária |
|---|---|---|
| `chamados/services/materializacao.py:129` — `reabrir_selecao` | Todos os elementos têm PK de pergunta | Seleção, trava administrativa e reabertura abrangem só perguntas; testemunha fica factual |
| `chamados/services/materializacao.py:524` — projeção | `metade(q)` lê campos/decisões de pergunta | Projetar testemunha como A, identificar sua origem e manter `par_ids` exclusivo das perguntas |
| `chamados/services/materializacao.py:1433` — ato | Validação, novas, replay, recusa e correção percorrem perguntas | Escrever apenas perguntas; não chamar decisão, carimbo ou resposta sobre testemunha |
| `chamados/services/materializacao.py:1539` — `_materializar_uma` | `{q.pk for q in par}` vira universo/processadas | Testemunha não é pergunta selecionável nem escrita nova |
| `chamados/services/validacao.py:721` | Unidade intersecta seleção elegível | Extrair IDs só das perguntas |
| `chamados/services/passivo_validado.py:385` | DRY verifica universo e conta unidade | Considerar uma pergunta autorizada + testemunha factual, sem acrescentar ID de batida a `autorizadas` |
| `chamados/management/commands/validar_classe_a.py:90` | DRY compara IDs com seleção de perguntas | Mesma extração exclusiva de perguntas |

As chamadas de teste atuais ficam em `chamados/tests/test_bug_par_unidade.py:386`, `:475`, `:586`; há substituição do identificador por tupla artificial em `:502`. Âncoras de mutantes referem o identificador em `chamados/tests/bug_par_mutantes.py:27`, `:55`, `:58`, `:79`, `:103`.

Riscos concretos:

- `chamados/services/acoes_disputa.py:23` exige exatamente duas perguntas; precisa aceitar um ou dois IDs e deixar o ato canônico conferir a identidade completa.
- `chamados/services/materializacao.py:1467` bloqueia recusa se qualquer metade for A. Deve considerar A de pergunta separadamente da testemunha A, para recusar a pergunta e preservar o chão.
- `templates/chamados/partials/_par_intervalo.html:37` gera um campo obrigatório para cada metade. A testemunha deve ser exibida e excluída dos campos de correção.
- `chamados/services/materializacao.py:1499` usa `par[0]` como pergunta; com saída batida, primeiro item pode ser testemunha.
- `chamados/services/materializacao.py:1519` passa `n_mat=2`; precisa contar perguntas efetivamente aplicadas.

Leitores indiretos:

| Leitor | Efeito esperado da projeção canônica |
|---|---|
| Vigia carimbo, `chamados/services/vigia_carimbo.py:107` | Continua julgando a pergunta pelo seu `codigo`; testemunha A não absolve pergunta V/R |
| Vigia fio, `chamados/services/vigia_fio.py:168` | Continua consumindo obrigação conjunta |
| Explicador, `chamados/services/explicador.py:73` | Transporta estado/capacidades novos; pergunta + batida pode estar pronta |
| Auditoria I6/I18, `chamados/management/commands/auditar_invariantes_chamados.py:537`, `:557` | Obrigação só desaparece quando pergunta e testemunha completam aplicação |
| O330, `chamados/services/passivo_validado.py:325` | Inventário autorizado contém perguntas; testemunha-batida não entra em reversão, seleção nem contador de perguntas |
| O330 contagem, `chamados/services/passivo_validado.py:565` | Já conta apenas perguntas presentes no grupo, se `processadas/aplicadas` mantiverem IDs de perguntas |
| Projeção textual, `chamados/services/declaracao_texto.py:477` | `pode_decidir_par` pode ser verdadeiro com uma pergunta respondida e contraparte batida |
| API, `api/views.py:1126` | Transporta esse resultado por `estado_par`, `estado_metade`, `aplicada` e capacidades |
| Tag, `chamados/templatetags/chamados_tags.py:18` | Já consulta a autoridade sem fórmula paralela |
| Cartões/modal, `templates/chamados/partials/_card_atendimento.html:77`, `_pergunta_card.html:20`, `modal_fio.html:160` | Todos usam o include único `_par_intervalo.html` |
| Fechamento explicado, `chamados/services/explicador.py:142` | Continua lendo C7; nenhuma nova fórmula local |

Somente leitura realizada; nenhum runtime ou banco executado. Resultados comportamentais [nao verificado].

## 09/11 15:31 · sessao 08d8d268322f · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

A construção está escrita. A [entrega completa](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:1) contém:

| Entrega | Endereço |
|---|---|
| Tabela arquivo:linha de cada hunk | [relatório:41](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:41) |
| 47 testes e o que cada um morde | [relatório:173](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:173) | [nao verificado]
| Comando individual de cada teste | [relatório:225](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:225) | [nao verificado]
| 40 mutantes, âncoras e comandos | [relatório:283](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:283) |

O juiz do par permanece na ampliação de [estado_aplicacao_pergunta](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:512). A porta conjunta está em [operar_par_intervalo](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:1406); a obrigação pronta para C7 consumir está em [obrigacoes_par_intervalo](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:112). É a autoridade existente de aplicação ampliada para a unidade PAR; a justificativa está no [relatório:7](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:7).

`models.py` permanece para a sessão. Os trechos exatos para C7, captura de `pelo_juiz` antes da linha 1905 e reconsulta após a trava terminada na linha 1920 estão no [relatório:12](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:12). Até esses hunks, ficam previstos RED: V/T técnico, V/V, `fechar`, ambas as transições por S127 e releitura sob trava. **Execução: [nao verificado].**

Foram conferidas sintaxe, âncoras e catracas estáticas; **Django e mutantes não foram executados**. RED/GREEN, mortes, concorrência e efeitos em produção permanecem **[nao verificado]**; não há certificação nesta entrega. ([registro das verificações](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:375))

## 09/11 15:14 · sessao f5a6761ef894 · CODEX-2-RAIAS-PORTAO-CPU · verificador · origem=Code

**REPROVA.** Procedentes: **P1 — escape do `--toca`; P2 — divergência entre admissão e fim; P3 — contador de runs contradiz o contrato declarado; P4 — falso vermelho condicional no selo.** Evidências abaixo. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:284`, `bin/codex_vagas_medida.py:114`, `bin/tests/test_codex_pergunta.sh:720`)

1. **T1 — semântica e escapes**

   **P1: nome com TAB escapa do arquivo exato.** O inventário imprime o nome sem codificação; o leitor separa por TAB e considera somente `c[1]`. Assim, o arquivo real `app/folha/views.py<TAB>fora` é interpretado como `app/folha/views.py` e passa por `--toca app/folha/views.py`. Reproduzi rc 0 executando a função real somente em memória. A validação dos caracteres do argumento não restringe os nomes encontrados no filesystem. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:323`)

   **P2: arquivos novos irmãos admitem e depois recusam.** `--toca app/novo/a.py` e `app/novo/b.py` são disjuntos na admissão. Porém, criar `a.py` também inventaria `app/novo/`: o fim aceita essa pasta como pai necessário ao meu arquivo e simultaneamente a considera cruzada com o arquivo da outra raia. Reproduzi admissão rc 0 e fim rc 3 em memória. Isso contraria o critério escrito de **arquivo em comum**, sem bloquear por família/app. (`bin/codex_pergunta.sh:263`, `bin/codex_pergunta.sh:270`, `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`, `CLAUDE.md:306`)

   A primitiva `cobre/cruza` é compartilhada: arquivo usa igualdade; pasta usa prefixo terminado em `/`. A divergência P2 surge do tratamento adicional das pastas inventariadas. (`bin/codex_pergunta.sh:263`, `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:284`)

   `..`, `/./`, barra dupla, absoluto e raiz diferente de `app/` ou `bin/` são recusados no argumento. Symlink tocado é proibido; a varredura usa `lstat` e não segue diretórios symlink. Ausência de escape transitório durante a execução: **[não verificado]**. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:188`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:215`)

   O chamador fornece `--toca`; o parser preenche `TOCA[]`; `_publica_estado` grava `toca=`; `_toca_de` lê; `_portoes` compara na admissão e o fim relê a outra vaga. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:311`, `bin/codex_pergunta.sh:291`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:567`)

2. **T2 — MORTO/ERRADO/VIVO**

   Não encontrei morte acidental concreta nos casos atuais. O acidente de `saida_symlink` foi corrigido: o caso exige `saida … e symlink`, enquanto a guarda seguinte escreve `codex: trava … e symlink`. (`bin/tests/test_codex_pergunta.sh:201`, `bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`)

   O controlador exige todos os esperados; ERRADO e VIVO aumentam as falhas. O autoteste morde: trocar a decisão por `echo MORTO` reprova outra asserção, dois esperados e esperado vazio. Isso foi exercitado em memória. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:751`, `bin/tests/test_codex_pergunta.sh:753`, `bin/tests/test_codex_pergunta.sh:754`, `bin/tests/test_codex_pergunta.sh:775`)

   Existe uma fragilidade: a busca não ancora a linha; um diagnóstico contendo `FALHA: <esperado>` também pode contar MORTO. Os autotestes não cobrem esse eco; um mutante da lista que efetivamente o produza permanece **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:750`)

   **P4: regressão condicional de portabilidade.** O cenário calcula `NPL=nproc-2`, mas dois esperados cravam `load5 6.00`. Com `nproc != 8`, classificam ERRADO uma morte pela asserção pretendida. É falso vermelho; ocorrência no host de produção: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`)

3. **T3 — histórico e trava**

   A derivação paralela foi corrigida: `HIST="$TRAVA.hist"` tem um sítio; a medida pergunta `--hist`. Os eventos produtivos encontrados são escritos por `_hist`, com `ini` após aquisição e `fim` no trap. Os selos também escrevem históricos, em cenários isolados. (`bin/trava_teste.sh:31`, `bin/trava_teste.sh:32`, `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`, `bin/codex_vagas_medida.py:115`, `bin/tests/test_trava_hist_unico.sh:54`)

   Portanto, **nem todo leitor literalmente pergunta**: o selo antigo usa diretamente `iso.lock.hist`, mas antes confere a resposta de `--hist`; o leitor produtivo pergunta. (`bin/tests/test_trava_teste.sh:112`, `bin/tests/test_trava_teste.sh:145`, `bin/codex_vagas_medida.py:115`)

   **`--presa` pode atrasar um chamador real.** Ele adquire exclusividade brevemente com `flock -n`; o chamador real disputa o mesmo lock com espera finita. Usar `-w` não prova ausência de colisão nem impossibilidade de rc 75 no limite da espera. Ocorrência e impacto reais: **[não verificado]**. L-074 exige exclusividade e rc correto, sem garantir FIFO; trataria a interferência como decisão técnica/fila, corrigindo a alegação absoluta da entrega. (`bin/trava_teste.sh:39`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:50`, `app/docs/LEIS.md:101`)

   A regra declarada pode **inflar** ocupação incluindo o intervalo ocioso entre um órfão e o próximo `ini`, ou **zerar** ocupação efetiva do último órfão quando a trava está livre. Isso é consequência da regra explicitada, não motivo novo para reprovar por preferência. (`bin/codex_vagas_medida.py:110`, `bin/codex_vagas_medida.py:142`, `bin/codex_vagas_medida.py:152`)

   A prova de vida não identifica o órfão: leitura do histórico e sondagem são separadas; outro chamador pode adquirir a trava antes de publicar seu `ini`. Nesse intervalo, o órfão antigo recebe a vida da chamada nova. (`bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`)

   **Mais de 24h ainda é possível com relógio regressivo.** A sequência temporal `[ini A 00h, fim A 24h, ini B 00h, fim B 24h]` retorna 48h: o escritor usa relógio civil e o leitor soma sem validar regressão ou unir sobreposições. Com timestamps não decrescentes, a máquina de um único aberto elimina a sobreposição antiga dos órfãos. Recuo real do relógio: **[não verificado]**. (`bin/trava_teste.sh:32`, `bin/codex_vagas_medida.py:123`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:145`)

   **P3: o contador contradiz a declaração.** A docstring promete que todo órfão entra em `n de runs`; o código só incrementa `n` com duração positiva. Um órfão solto retorna `(0 segundos, 0 runs, 1 órfão)`. O selo exige essa exclusão. (`bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:126`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`)

4. **DECISÃO-ABERTA 1–7**

   | Nº | Resposta pela lei ou encaminhamento |
   |---|---|
   | 1 | Retenção/caminho é decisão técnica do Code; não achei exigência de persistência histórica. A limpeza efetiva de `/tmp` no reboot é **[não verificado]**, embora afirmada no comentário. (`CLAUDE.md:273`, `bin/trava_teste.sh:30`) |
   | 2 | **L-003 responde:** fonte ausente não autoriza inventar zero. A exceção vira “não sei”. (`app/docs/LEIS.md:61`, `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`) |
   | 3 | A opção restritiva já cabe na regra de cura mais restritiva; não pede lei nova. O mesmo caminho com/sem `/` cruza expressamente. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:266`) |
   | 4 | Política técnica de estimativa/retenção; a implementação fecha pelo próximo `ini` sem consultar boot. Não há resposta legal específica identificada. (`CLAUDE.md:273`, `bin/codex_vagas_medida.py:108`, `bin/codex_vagas_medida.py:142`) |
   | 5 | Questão técnica/fila; “nenhum chamador perde a vez” não está demonstrado pelo uso de espera finita. (`CLAUDE.md:273`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`) |
   | 6 | Conferência de ambiente, não pergunta de lei. Ruff do container e equivalência com o resultado entregue: **[não verificado]**. |
   | 7 | O leitor produtivo agora usa `--hist`; existência/conteúdo dos históricos antigos e necessidade real de migração: **[não verificado]**. Não sustento “nada a migrar” como fato de produção. (`bin/codex_vagas_medida.py:115`) |

5. **Regressão no delta**

   Vagas, portões de CPU e lista protegida de `bin/` mantêm os sítios operacionais apresentados na volta 1; as mudanças de comportamento encontradas no delta são P2, P4 e a nova contagem/sondagem do histórico. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:293`, `bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:284`, `bin/tests/test_codex_pergunta.sh:720`, `bin/codex_vagas_medida.py:139`)

   `vaga=N` continua no último campo. O leitor posicional conserva os índices anteriores: sessão em `p[6]`, rc em `p[7]`, duração em `p[8]`; não exige tamanho exato. Disjuntor e gerador de estado usam esse leitor. Não encontrei quebra posicional nesses consumidores. (`bin/codex_pergunta.sh:229`, `bin/codex_pergunta.sh:428`, `bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:67`, `bin/disjuntor.py:2121`, `bin/gerar_codex_estado.py:176`)

   A leitura semanal reutiliza `leituras_semanais`, sem novo juiz para esse insumo. O relógio da medida entra em `main` e é passado às funções; a borda de horário do wrapper permanece na admissão. (`bin/codex_vagas_medida.py:78`, `bin/codex_vagas_medida.py:226`, `bin/codex_pergunta.sh:300`)

6. **Quem escreve / quem lê**

   | Campo ou arquivo | Escritor → leitor |
   |---|---|
   | `--toca`, `TOCA[]` | Chamador → parser, validação, prompt, admissão e fim. (`bin/codex_pergunta.sh:71`, `:323`, `:465`, `:305`, `:574`) |
   | Caminhos das vagas/meta/estados | Wrapper deriva do common-dir → reserva nomes, abre locks e consulta estados. (`bin/codex_pergunta.sh:85`, `:93`, `:97`, `:380`, `:389`) |
   | Flock das vagas | Wrapper adquire → `_viva` testa o próprio flock. (`bin/codex_pergunta.sh:383`, `:386`, `:253`) |
   | `.estado`: `vaga,pid,obra,wt,hash,hora,toca` | `_publica_estado` grava por rename; `_saida` remove. Decisão lê `toca=`; os demais campos não são lidos por essa decisão. (`bin/codex_pergunta.sh:310`, `:311`, `:236`, `:291`) |
   | Load5, hora, pausas | `/proc/loadavg`, relógio e arquivos de pausa → portões; entradas falsas passam pela guarda de teste. (`bin/codex_pergunta.sh:118`, `:294`, `:297`, `:300`, `:328`, `:112`) |
   | `chamadas.log`: hora, obra, papel, hash, pid, evento, pergunta/sessão, rc, duração, vaga | Wrapper → leitor posicional e contador de vaga 2. (`bin/codex_pergunta.sh:229`, `:428`, `bin/codex_limite_estado.py:49`, `bin/codex_vagas_medida.py:160`) |
   | `vaga2.log`: hora, obra, motivo, load5, fonte | `_recusa_v2` → contagem diária e selo. (`bin/codex_pergunta.sh:249`, `bin/codex_vagas_medida.py:166`, `bin/tests/test_codex_pergunta.sh:521`) |
   | Arquivos da WT, HEAD, gitfile, blobs, modos, links e ignorados | Construtor/git → mesmo inventário no pré-check, revalidação e fim. (`bin/codex_pergunta.sh:174`, `:181`, `:194`, `:397`, `:530`, `:560`) |
   | `.inventario`: classe, caminho, motivo | Inventário e verificações finais → `_TOCA_PY`, decisão rc 3 e `.recusa`. (`bin/codex_pergunta.sh:217`, `:278`, `:560`, `:575`, `:579`) |
   | Resposta/JSONL/stderr; `.id`, `.rc` | Codex escreve saída; wrapper publica resposta e sidecars. `.id` é lido pela triagem; `.rc` é cobrado no selo. (`bin/codex_pergunta.sh:545`, `:553`, `:232`, `:604`, `bin/disjuntor.py:854`, `bin/tests/test_codex_pergunta.sh:174`) |
   | `TRAVA`, `HIST`, eventos `epoch,ini/fim,quem,rc` | `trava_teste.sh` define/publica e escreve eventos → medida pede caminho e lê eventos; `rc` não participa da soma. (`bin/trava_teste.sh:23`, `:31`, `:32`, `:38`, `bin/codex_vagas_medida.py:115`, `:132`, `:144`) |
   | `TRAVA.quem` | Trava escreve → `--quem` e diagnóstico de timeout. (`bin/trava_teste.sh:52`, `:54`, `:37`, `:49`) |
   | Sar: `ldavg-5`, Average e amostras | `sar -q` → parser, desligador e linha diária. (`bin/codex_vagas_medida.py:43`, `:57`, `:66`, `:88`, `:198`) |
   | Sessões: timestamp, janela, used_percent, resets_at | Codex CLI é o produtor declarado; leitor canônico → desligador. Escrita efetiva do CLI: **[não verificado]**. (`bin/codex_vagas_medida.py:15`, `bin/codex_limite_estado.py:154`, `:155`, `:159`, `bin/codex_vagas_medida.py:78`) |
   | Cota: `seven_day_pct,resets_at,hora`; foto anterior | Statusline grava cota → medida calcula diferença entre fotos. Produtor da foto anterior: **[não verificado]**. (`bin/statusline_cota.py:51`, `:54`, `bin/codex_vagas_medida.py:178`, `:180`, `:182`) |
   | Mutantes, esperados, saídas e tabela | Selos fabricam cenários/mutantes → classificadores e resultado final. (`bin/tests/test_codex_pergunta.sh:643`, `:769`, `:775`, `bin/tests/test_trava_hist_unico.sh:80`, `:95`, `:97`) |

**Veredito final: REPROVA — P1 e P2 exigem cura do controle de escopo; P3 exige alinhar contagem e contrato; P4 exige alinhar o esperado ao cenário dinâmico.** GREEN integral da volta 2 não foi reproduzido nesta verificação somente leitura. (`bin/codex_pergunta.sh:217`, `:284`, `bin/codex_vagas_medida.py:114`, `:128`, `bin/tests/test_codex_pergunta.sh:100`, `:720`; GREEN: **[não verificado]**)

## 09/11 15:12 · sessao aafe6042da94 · - · - · origem=Ronald (tmux)

T3 ficou com fonte única de caminho: `TRAVA` em `bin/trava_teste.sh:23`, `HIST="$TRAVA.hist"` em `:31`, publicação por `--hist` em `:38`; o leitor pergunta por `_trava('--hist')` em `bin/codex_vagas_medida.py:103-118`.

- **Escritores/leitores encontrados:** produção escreve eventos exclusivamente por `_hist` (`bin/trava_teste.sh:32`), chamado com `ini` após adquirir a trava (`:47-53`) e `fim` no trap (`:54`). A medida lê os quatro campos, mas ignora `rc` (`bin/codex_vagas_medida.py:130-146`). Os testes escrevem históricos sintéticos isolados (`bin/tests/test_trava_hist_unico.sh:30-34,54`; `bin/tests/test_trava_teste.sh:105-112,145-149`). O teste antigo também conhece `iso.lock.hist` diretamente, porém confere que `--hist` devolve esse caminho (`bin/tests/test_trava_teste.sh:112,145-149`).
- **`--presa` interfere na trava:** abre o lock e tenta adquirir exclusividade com `flock -n`, soltando depois (`bin/trava_teste.sh:39-40`); o chamador real disputa o mesmo lock com espera finita (`:47-50`). Assim, pode ocupar um intervalo livre antes do chamador real e atrasá-lo. A conclusão da entrega “nenhum chamador vira rc75 pela colisão” não decorre do código: o timeout pode vencer durante essa ocupação. Ocorrência real, duração e frequência: **[não verificado]**. L-074 exige exclusividade e rc correto, sem promessa FIFO nem proibição expressa de sondagem (`app/docs/LEIS.md:101`); classificaria a interferência como decisão técnica/fila, sem inventar violação de lei.
- **Regra dos órfãos:** próximo `ini` fecha o anterior (`bin/codex_vagas_medida.py:139-143`); último órfão solto soma zero, preso soma até `agora` (`:147-153`). Pode incluir lacuna ociosa ou zerar ocupação efetiva, mas isso foi explicitamente permitido pela triagem v1 e declarado (`:110-114`); não é motivo novo de reprovação por preferência.
- **Identidade não é provada:** leitura do histórico e `--presa` são operações distintas (`bin/codex_vagas_medida.py:130-153`). Se após EOF um novo chamador prende o lock antes de publicar seu `ini`, o `--presa` pode atribuir vida a um órfão antigo; essa janela existe entre adquirir lock e `_hist ini` (`bin/trava_teste.sh:48-53`). O teste “presa” segura apenas o flock, sem identidade nem evento novo (`bin/tests/test_trava_hist_unico.sh:60-68`), portanto não distingue esse caso.
- **Mais de 24h ainda possível com relógio regressivo:** executei somente em memória a função real extraída por AST, substituindo `open` por `StringIO` e `_trava` por retorno sintético. Eventos `[ini A 00h, fim A 24h, ini B 00h, fim B 24h]` retornam `(172800.0, 2, 0)`, ou 48h. O escritor usa wallclock (`bin/trava_teste.sh:32`); o leitor soma intervalos sem validar sequência temporal nem unir sobreposições (`bin/codex_vagas_medida.py:123-145`). Com eventos temporalmente não decrescentes, a máquina de um único aberto elimina a inflação antiga por sobreposição de órfãos (`:139-146`). Recuo real do relógio: **[não verificado]**.
- **Inconsistência documental concreta:** a docstring promete que todo órfão entra em `n de runs` (`bin/codex_vagas_medida.py:113-114`), mas `n` só aumenta com duração positiva (`:123-128`). Em memória, um único órfão solto retorna `(0.0, 0, 1)`. O selo cristaliza essa exclusão do último órfão solto (`bin/tests/test_trava_hist_unico.sh:54-58`).

Decisões abertas:

1. Retenção em `/tmp`: caminho/encaixe técnico do Code, não pergunta de lei (`CLAUDE.md:273-278`); limpeza efetiva no reboot do host **[não verificado]**, apenas afirmada no comentário (`bin/trava_teste.sh:30`).
2. Ausência → “não sei”: L-003 responde contra inventar número sem fonte (`app/docs/LEIS.md:61`); implementação por exceção de `open` convertida em “não sei” (`bin/codex_vagas_medida.py:130,189-193`).
4. Órfãos anteriores ao boot: mesma regra declarada, sem consulta ao uptime (`bin/codex_vagas_medida.py:108-153`); política de retenção/estimativa é técnica (`CLAUDE.md:273-278`).
5. Probe concorrente: decisão técnica; corrigir a alegação absoluta de inocuidade da entrega (`bin/trava_teste.sh:39-40,47-50`).
7. Migração: não há leitor produtivo remanescente do antigo caminho encontrado no código; único leitor produtivo agora pede `--hist` (`bin/codex_vagas_medida.py:115-130`). Existência/conteúdo real dos históricos antigos: **[não verificado]**; não afirmar “nada a migrar” como fato de produção.

## 09/11 15:12 · sessao 8b64751089c9 · - · - · origem=Ronald (tmux)

T2: **não encontrei morte acidental concreta entre os mutantes declarados**; o classificador agora exige todos os fragmentos esperados, e ERRADO/VIVO reprovam o selo. `bin/tests/test_codex_pergunta.sh:742`, `bin/tests/test_codex_pergunta.sh:775`.

- O acidente anterior de `saida_symlink` foi corrigido: o caso exige a mensagem específica `saida <...>.lock e symlink`; a guarda seguinte escreve `codex: trava <...>.lock e symlink`. `bin/tests/test_codex_pergunta.sh:201`, `bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`.
- `_recusa` cobra motivo, rc e ausência de chamada; `_inv_recusa` procura PROIBIDO e motivo no terceiro campo. Assim, os esperados “motivo ausente” e “sem PROIBIDO” identificam asserções determinadas, mesmo quando outra guarda ainda recusa a chamada. `bin/tests/test_codex_pergunta.sh:163`, `bin/tests/test_codex_pergunta.sh:169`, `bin/tests/test_codex_pergunta.sh:644`.
- O autoteste morde o controlador convertido em `echo MORTO`: os testes de outra asserção, dois esperados e esperado vazio falham. Reproduzi a função em memória, com leitura simulada pelo `grep`, sem escrever arquivos: original `ERRADO|MORTO|ERRADO|ERRADO|VIVO`; mutado `MORTO|MORTO|MORTO|MORTO|VIVO`. `bin/tests/test_codex_pergunta.sh:750`.
- Limite: o classificador usa substring sem ancoragem; uma linha `diagnostico: FALHA: <esperado>` também conta MORTO. Os autotestes não exercitam esse eco. Não localizei um caso atual que produza essa situação; sua ocorrência na lista permanece **[não verificado]**. `bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:750`.
- **Regressão objetiva de portabilidade do selo:** `NPL=nproc-2` e o motivo do caso são dinâmicos, mas dois esperados cravam `load5 6.00`. Com `nproc!=8`, esses mutantes dão ERRADO mesmo morrendo pela asserção pretendida. É falso vermelho, não falso verde; a VM da autorização tem 8 CPUs segundo o dado recebido **[não verificado na cópia]**. `bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`.

Não executei os selos completos, pois criam e removem arquivos; os resultados GREEN da entrega não foram reproduzidos. `bin/tests/test_codex_pergunta.sh:22`, `bin/tests/test_codex_pergunta.sh:23`, `bin/tests/test_codex_pergunta.sh:769`.

## 09/11 15:11 · sessao 22223ccca92c · - · - · origem=Ronald (tmux)

Dois procedentes em T1:

1. **Escape do `--toca` por TAB ou newline no nome real.** O inventário emite `t` sem escape em TSV (`bin/codex_pergunta.sh:217`), enquanto o consumidor separa por linha/TAB e usa somente `c[1]` (`bin/codex_pergunta.sh:278-282`). Assim, o arquivo real `app/folha/views.py<TAB>fora` é julgado como `app/folha/views.py` e passa por `--toca app/folha/views.py`, embora seja outro arquivo; reproduzi o rc 0 em memória executando `_TOCA_PY` real com `StringIO`, sem escrever arquivo. A lista de caracteres do `--toca` não protege nomes de arquivos encontrados pelo inventário (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:217`).

2. **Admissão e fim divergem para arquivos novos irmãos sob pai novo.** `app/novo/a.py` versus `app/novo/b.py` não cruzam na admissão (`bin/codex_pergunta.sh:263-275`). Criar `a.py` produz também `TOCADO app/novo/` (`bin/codex_pergunta.sh:195-196`); o fim aceita esse pai como necessário ao meu arquivo, mas o considera cruzado com o arquivo da outra raia (`bin/codex_pergunta.sh:283-288`). Reproduzi em memória admissão rc 0 e fim rc 3. É falsa recusa por pasta comum, contrária ao critério de arquivo comum (`CLAUDE.md:306-308`).

Demais verificações:

- A primitiva de interpretação é compartilhada: sem `/` final é igualdade exata; com `/` é prefixo por componente; admissão e fim chamam a mesma definição (`bin/codex_pergunta.sh:263-287`).
- `..`, `/./`, `//`, caminho absoluto e raiz diversa de `app/` ou `bin/` são recusados no argumento (`bin/codex_pergunta.sh:323-324`). `app/.` terminal passa na forma, mas não cobre descendentes; não identifiquei escape por isso (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:323`).
- Symlink tocado é `PROIBIDO`; o walk não segue symlink e usa `lstat` (`bin/codex_pergunta.sh:188-194`, `bin/codex_pergunta.sh:215`). Ausência de escape transitório durante execução: [não verificado].
- O chamador fornece `--toca`; o parser escreve `TOCA[]`, a validação restringe o formato, `_publica_estado` grava `toca=`, `_toca_de` lê esse campo, `_portoes` julga a admissão e o fim relê o estado da outra raia (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:321-324`, `bin/codex_pergunta.sh:308-312`, `bin/codex_pergunta.sh:291`, `bin/codex_pergunta.sh:295-306`, `bin/codex_pergunta.sh:565-574`).
- Os controles novos cobrem arquivo exato, pasta por componente e pai novo com uma raia; não cobrem TAB/newline nem dois arquivos irmãos sob pai novo (`bin/tests/test_codex_pergunta.sh:562-570`).

## 09/11 14:51 · sessao 8edd85065351 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · sonda · origem=Code

**O anexo permite contar pares candidatos, mas não quantificar disputas distintas que passariam a ficar retidas.** São **112 pares em disputas abertas e 18 em fechadas**, nas classes citadas. [nao verificado — cálculo sobre o anexo]

Não localizei `sonda_par.py` nesta cópia; portanto, não conferi seu conteúdo nem o hash anunciado. Uso `[nao verificado]` nas contagens do anexo e nas somas derivadas delas, sem confirmação independente de PROD. [nao verificado]

1. **Censo por combinação**

“Fechada” abaixo é total menos aberta, condicionado a ambos os dicionários cobrirem o mesmo recorte. [nao verificado]

| Classe rotulada | Pares totais | Em abertas | Em fechadas | Evidência |
|---|---:|---:|---:|---|
| A/A | 3.793 | 1.494 | 2.299 | [nao verificado] |
| A/R | 49 | 43 | 6 | [nao verificado] |
| A/S | 53 | 52 | 1 | [nao verificado] |
| A/T | 1 | 0 | 1 | [nao verificado] |
| A/V | 5 | 2 | 3 | [nao verificado] |
| R/R | 172 | 168 | 4 | [nao verificado] |
| R/S | 18 | 17 | 1 | [nao verificado] |
| S/S | 2.514 | 2.482 | 32 | [nao verificado] |
| S/V | 4 | 4 | 0 | [nao verificado] |
| T/T | 196 | 0 | 196 | [nao verificado] |
| V/V | 18 | 11 | 7 | [nao verificado] |
| **Total** | **6.823** | **4.273** | **2.550** | [nao verificado] |

R/T, R/V, S/T e T/V não aparecem nos dicionários: contribuição zero **na saída apresentada**, sem comprovar zero na projeção correta da matriz. [nao verificado]

Os 6.823 pares correspondem a **13.646 metades**, distribuídas pelos rótulos: **S=5.103, R=411, V=45, A=7.694, T=393**. Das 18.424 metades declaradas, **4.778 ficam fora dessa tabela**; sua distribuição por estado não foi fornecida. A conta dos grupos fecha: `9.900 − 2.756 sem_par − 321 ambíguos = 6.823`. [nao verificado]

**N não é zero conhecido.** A recusa humana registra `ato='recusa'` na trilha e limpa resposta e validação da projeção; sem E1, uma metade recusada pode parecer S. Isso esconde, por exemplo, S/N dentro de S/S e A/N dentro de A/S. (`app/chamados/services/validacao.py:591`; `app/chamados/services/disputa_emissao.py:1590`; exemplos de redistribuição [nao verificado])

2. **Retenção pelo C7 ampliado e legado**

| Classe candidata | Pares em abertas | Pares em fechadas | Evidência |
|---|---:|---:|---|
| A/R | 43 | 6 | [nao verificado] |
| A/S | 52 | 1 | [nao verificado] |
| A/V | 2 | 3 | [nao verificado] |
| A/T | 0 | 1 | [nao verificado] |
| S/V | 4 | 0 | [nao verificado] |
| V/V | 11 | 7 | [nao verificado] |
| **Total** | **112** | **18** | [nao verificado] |

V/R e V/T não acrescentam ocorrências nos dicionários apresentados. O total separado `V_sem_A=47` não deve ser somado aos pares: mistura unidades e pode sobrepor essas ocorrências. [nao verificado]

**Não dá para chamar esses números de 112 disputas novas retidas e 18 disputas legadas.** Os exemplos repetem `disputa_pk`, inclusive entre combinações, e não fornecem a relação completa para deduplicação. [nao verificado — anexo `EXEMPLOS`]

Também falta demonstrar **“que hoje fecham”**: C7 consulta todas as perguntas da disputa, exigindo ausência de resposta sem veredito e de fio vivo; outra pergunta pode já impedir seu fechamento. (`app/chamados/models.py:1850`; `app/chamados/models.py:1861`)

A brecha para V está comprovada no código: C6 considera qualquer pergunta validada morta imediatamente, e `respostas_sem_veredito()` exclui as validadas, mesmo sem aplicação. Isso demonstra a possibilidade de escape, sem quantificar a população afetada. (`app/chamados/juizes.py:1153`; `app/chamados/models.py:1851`)

Os 18 pares fechados são ocorrências candidatas de legado, sem autorização para correção automática do dado pela L-099; a porta `fechar()` também não reabre disputa já fechada. (`CLAUDE.md:176`; `app/chamados/models.py:1887`; quantidade [nao verificado])

3. **`ESPERA_PAR_INTERVALO`**

O anexo informa **`espera_gravada=0`**. A expressão também não apareceu na busca textual desta cópia. A afirmação histórica “v5 nunca pousou” permanece sustentada apenas pelo anexo. [nao verificado] [nao verificado]

Zero gravado não significa zero obrigação pendente: o próprio anexo declara 47 ocorrências de `V_sem_A`. [nao verificado]

4. **Rótulo versus predicado — LEI-AKITA 8**

Sem o script, **não é possível certificar seu predicado nem atribuir-lhe uma inversão específica de precedência**. A lei exige medir pela função real, com rótulo correspondente ao universo contado. (`CLAUDE.md:35`; implementação da sonda [nao verificado])

Os pontos verificáveis para confrontá-lo com `T > A factual > N atual > V > R/S` são:

- **T antes de A:** linhas fora do fato são identificadas pelas vias `duplicata` e `motivo_retratado`; `fora_do_fato()` não depende de carimbo. Classificar pelo timestamp antes dessa identidade pode encobrir T. (`app/chamados/models.py:2401`; `app/chamados/models.py:3193`)
- **A factual:** `materializada_em` sozinho não prova aplicação. `estado_aplicacao_pergunta()` reconhece explicitamente carimbo de recusa sem batida; o juiz de lastro dispõe de verificações de testemunha factual. (`app/chamados/services/materializacao.py:434`; `app/chamados/juizes.py:645`; `app/chamados/juizes.py:789`)
- **N atual:** `recusa_motivo` também guarda recusa técnica; recusa humana histórica não basta para identificar a decisão vigente. A sequência contém respostas e validações e ordena por `ato_em, pk`. (`app/chamados/models.py:2567`; `app/chamados/models.py:3857`; `app/chamados/models.py:3889`)
- **A função atual não entrega a matriz:** exige pergunta tipo hora com texto e retorna estados de aplicação, sem implementar aquela sequência S/R/V/A/N/T. (`app/chamados/services/materializacao.py:430`; `app/chamados/services/materializacao.py:432`)

5. **O que exige outra leitura**

Permanecem indeterminados: classificação correta das 18.424 metades; quantidade de N; identidade dos pares nos grupos ambíguos; classes das metades fora dos pares únicos; sobreposição dos 28 `pendencia_admin`; e correspondência entre os 47 `V_sem_A` e os 45 rótulos V dos pares únicos. [nao verificado]

Para medir **disputas adicionais retidas**, falta uma leitura que associe cada par à disputa, deduplique os IDs, projete os estados com E1 e testemunha factual e compare, na mesma disputa, C7 atual com a obrigação ampliada. O agregado fornecido não contém esse cruzamento. (`app/chamados/models.py:1861`; `app/chamados/models.py:3839`; `app/chamados/juizes.py:645`; disponibilidade desse cruzamento [nao verificado])

_3197 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
