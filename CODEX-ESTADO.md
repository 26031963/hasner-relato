# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 02:26** · publicado em **06/11/2026 02:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 02:26 · sessao a4b5542bebe9 · H12H06-B · verificador · origem=Code

**REPROVA quanto à abrangência da invalidação:** há janela que o leitor alcança e a nova chamada deixa de fora. (`app/ponto/services/ausencia.py:519`; `app/ponto/turnos.py:2762`)

1. **Alta — ausência aberta deixa projeções futuras carimbadas.**  
   O fim vazio vira `max(data, hoje)`, enquanto a cobertura de `TIPOS_EM_ABERTO` se estende até o fim da competência consultada. (`app/ponto/services/ausencia.py:515–520`; `app/ponto/turnos.py:2753–2767`)

   Contraexemplo: em 06/11, corte 21, INSS aprovado desde 01/10, sem fim, com projeção de dezembro existente; editar para atestado de um dia em 01/10 invalida outubro/novembro, mas dezembro perde a supressão sem perder `derivado_em`. O escritor permite calcular e gravar dezembro; não garante a premissa “competência futura não tem projeção”. Existência desse caso em produção: **[não verificado]**. (`app/ponto/janelas.py:10–33`; `app/ponto/services/ausencia.py:496–520`; `app/ponto/services/ausencia.py:940`; `app/ponto/services/fechamento.py:430–436`, `:610–630`, `:674–692`, `:1127–1128`)

   Para ranges finitos válidos, o envelope cobre antigo/novo, deslocamentos para trás/para frente e competências intermediárias. `NULL` de tipo não aberto pode invalidar competências extras, pois sua cobertura efetiva é um dia. (`app/ponto/services/ausencia.py:516–520`; `app/ponto/services/fechamento.py:1578–1583`; `app/ponto/services/dia_pago.py:712–716`; `app/ponto/turnos.py:2762–2767`)

2. **Alta — permanece caminho de alteração de aprovada sem invalidação.**  
   `curar_ausencia_sem_fim` inclui aprovadas e chama `rederivar_fim`, que apenas salva/relê. O modelo então pode preencher `data_fim`, alterando a cobertura de várias competências sem invalidá-las. É caminho legado; existência de alvos atuais em produção: **[não verificado]**. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29–31`, `:46`; `app/ponto/services/ausencia.py:1289–1294`; `app/ponto/models.py:902–908`)

   Os sinais não fecham esse buraco: julgam células e reconciliam chamados, sem invalidar o fechamento nesses caminhos. (`app/ponto/signals.py:85–93`, `:143–147`, `:250–272`)

3. **Média — “dentro do atomic do chamador” não vale para a tela.**  
   No comando, edição e invalidação estão no `atomic` de `tratar_linha`; `_Desfaz` desfaz também o carimbo. Na view, não há `atomic` envolvendo a chamada; os blocos internos de `invalidar_fechamento` começam depois do `save` da ausência. As configurações copiadas tampouco habilitam `ATOMIC_REQUESTS`. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–191`; `app/ponto/views.py:2750–2803`; `app/ponto/services/ausencia.py:509–520`; `app/ponto/services/fechamento.py:1632–1640`; `app/config/settings/saas.py:83–92`; `app/config/settings/prod.py:9–18`)

   Portanto, rollback do comando restaura o carimbo; falha posterior na tela não desfaz automaticamente os atos anteriores. Além disso, falha da invalidação é engolida, retorna zero e não impede confirmar a edição — limite herdado do helper utilizado. (`app/ponto/services/fechamento.py:1645–1649`; `app/ponto/services/ausencia.py:520`)

4. **Média, condicional — contesto descartar a corrida com exportação incondicionalmente.**  
   O apply trava somente a ausência e consulta “entregue” antes de editar; a exportação aceita competência solicitada, calcula sua fotografia e registra o TXT sem compartilhar essa trava. É possível consultar “não entregue”, exportar a fotografia anterior e confirmar a edição concorrentemente. Ocorrência em produção: **[não verificado]**. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–170`; `app/folha/views.py:109–120`, `:207–209`, `:297–300`; `app/folha/models.py:174–188`)

   “Antes de 21/11” não constitui garantia pelo código: o portão verifica carimbo do dia/competência e falhas, sem exigir encerramento temporal. A ausência operacional de exportação concorrente continua **[não verificado]**. (`app/folha/porta_export.py:800–867`)

5. **Baixa — teste de rollback pode passar vazio isoladamente.**   [nao verificado]
   `test_linha_desfeita_nao_vence_a_projecao` exige apenas carimbo preenchido; também passa se o comando pular antes de editar. O irmão da trilha verifica resumo e minutos restaurados, mas o teste novo não demonstra que chegou a invalidar antes do rollback. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:254–259`, `:163–169`)

   O teste de range cobre somente avanço de um atestado aguardando documento, de outubro para novembro — competências 11→12. Não cobre aberto, retorno para trás, fim antigo mais distante nem exportada/barrada. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:72–76`, `:261–272`)

**Rastreio de `derivado_em`:**

| Papel | Quem / comportamento | Evidência |
|---|---|---|
| Preenche | Recalcular fechamento: `save` para aberto, `update` para demais status | `app/ponto/services/fechamento.py:1121–1128`, `:1144–1173` |
| Esvazia | `invalidar_fechamento`; `restaurar_fechamento`, salvo valor explicitamente fornecido | `app/ponto/services/fechamento.py:1635–1640`, `:1364–1377` |
| Lê `NULL` | Queryset/contador de projeções vencidas e comando que conta/lista; não rederivam | `app/ponto/services/fechamento.py:1675–1683`, `:1705`; `app/ponto/management/commands/projecoes_vencidas.py:47–68` |
| `decidir` | Chama invalidação; não consulta o carimbo | `app/ponto/services/ausencia.py:942–944` |
| Evento | Recalcula após commit, independentemente do carimbo | `app/ponto/services/fechamento.py:1791–1808` |
| `fechamento_lido` | Deriva em memória, independentemente do carimbo | `app/ponto/services/fechamento.py:1743–1745` |
| Leitores do gravado | Os caminhos padrão de classificação/export carregam valores sem testar o carimbo | `app/folha/export.py:709–724`, `:775–780` |
| TXT pela view | Injeta fechamentos derivados em memória | `app/folha/views.py:207–212` |

O que fica vazio é **o carimbo**, não as rubricas. A invalidação alcança também exportadas/barradas dentro da janela; a guarda da 09 pode impedir reconstrução, deixando valores antigos com carimbo nulo. (`app/ponto/services/fechamento.py:1635–1640`, `:549–555`)

Os testes positivos têm mordida: começam com carimbos preenchidos e exigem `None`; o DSR consulta `FechamentoMensal` gravado, usa motor real e exige delta exato `(+1, −1)` para 720/0, com contraste 479. O teste de 720 exige perda prévia; há mock da lavra como `{}`, limitando a integração comprovada. Execução GREEN/RED/M16/M17: **[não verificado]**. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:241–252`, `:261–272`; `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:130–178`)

Nos leitores examinados, a decisão de parcial continua delegada ao juiz existente; não identifiquei nova derivação paralela nesses sítios. O relógio é lido dentro da porta de edição; classificá-lo sozinho como “fora da borda” não está demonstrado. (`app/ponto/services/fechamento.py:681–685`; `app/ponto/services/efeito_ausencia.py:33–55`; `app/ponto/services/credito_parcial.py:20–27`; `app/ponto/turnos.py:2583–2600`; `app/ponto/services/ausencia.py:513–519`)

O DIFF de impacto anterior ao pouso permanece **[não verificado]**; “medida em curso” não demonstra cumprimento do pré-requisito. A exigência continua registrada, e eventos continuam podendo regravar competências exportadas fora da guarda da 09. (`app/docs/LEIS.md:112`, `:142`; `app/ponto/services/fechamento.py:530–555`, `:1793–1794`)

## 06/11 02:26 · sessao b2671fcd93c3 · - · - · origem=Ronald (tmux)

Achados principais:

1. **Alta — atomicidade prometida não existe na tela.** `app/ponto/services/ausencia.py:509` salva a ausência antes da invalidação em `:520`; a função começa em `:465` sem `atomic`. O chamador de tela tem apenas `@login_required` (`app/ponto/views.py:2750`) e chama a porta em `:2794`. A configuração copiada não ativa `ATOMIC_REQUESTS` (`app/config/settings/saas.py:83`). Portanto, o comentário “Dentro do atomic do chamador” (`app/ponto/services/ausencia.py:512`) só corresponde ao comando: a tela deixa escrita e carimbo em transações separadas. Configuração efetiva de produção: `[não verificado]`.

2. **Alta, caminho legado — outro escritor altera insumo de aprovada sem invalidar.** `curar_ausencia_sem_fim` seleciona todas as não rejeitadas, incluindo aprovadas (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`), e chama `rederivar_fim` em `:46`. Essa função apenas salva e relê (`app/ponto/services/ausencia.py:1289`), sem invalidar. O modelo preenche `data_fim` (`app/ponto/models.py:902`) e também pode preencher minutos de falta aprovada zero (`:877`). É outro caminho de escrita dos campos perguntados, embora dentro do arquivo declarado como porta. O comando é classificado como cura de acervo, uma vez (`app/config/crons.py:1257`). Existência atual de alvos: `[não verificado]`.

3. **Alta, contestação do descarte — executar antes de 21/11 não elimina estruturalmente a corrida com exportação.** O comando trava somente Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`), consulta a entrega em `:160` e depois escreve em `:169`. O export recebe mês/ano sem exigir competência encerrada (`app/folha/views.py:109`); `conferir` verifica carimbo do dia, competência e falhas (`app/folha/porta_export.py:800`), sem barreira de data. Lê a projeção online (`app/folha/views.py:207`) e registra o TXT posteriormente (`:297`), usando `exists/create`, sem trava comum (`app/folha/models.py:174`). Interleaving possível: cerca passa → export lê dados antigos → apply confirma → export registra TXT antigo. Ocorrência real e carimbo verde atual: `[não verificado]`.

Conferências:

- **Comando:** `transaction.atomic()` em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137` contém porta e invalidação. `_Desfaz` por diferença de campos/valor ou auditoria ausente (`:175`, `:178`, `:186`) desfaz também o carimbo.
- **Signals não cobrem o segundo escritor:** o receiver da Ausencia chama cartório (`app/ponto/signals.py:143`); `_julgar_dias` julga células (`:85`). O consumidor apenas reconcilia chamados após commit (`:250`).
- **Admin não é escritor adicional:** AusenciaAdmin usa SoLeitura (`app/ponto/admin.py:37`), que nega adicionar/editar/apagar (`app/core/admin.py:59`).
- **Form não é escritor adicional:** AusenciaForm deriva de `forms.Form` (`app/ponto/forms.py:41`).
- **Férias não edita esses campos de Ausencia existente:** o modelo delega o espelho (`app/ferias/models.py:379`); o espelho mantém existentes e cria ausentes (`app/ponto/services/ausencia.py:1194`).

## 06/11 02:25 · sessao 78bcf75af9d3 · - · - · origem=Ronald (tmux)

**Achado: janela incompleta para ausência aberta com projeção futura existente — gravidade alta, condicional à existência desse gravado.** `editar_ausencia_registro` substitui `data_fim=NULL` pelo máximo entre início e hoje, limitando a invalidação (`app/ponto/services/ausencia.py:515-520`); o leitor de cobertura estende ausência de `TIPOS_EM_ABERTO` até o fim de qualquer janela consultada (`app/ponto/turnos.py:2753-2767`).

Contraexemplo por código: em 06/11, corte21, ausência aprovada INSS iniciada em 01/10, sem fim, e projeção de 12/2026 existente; editar para atestado de um dia em 01/10 invalida somente competências10/11, mas a12 perde toda a supressão antiga sem perder seu carimbo (`app/ponto/services/ausencia.py:496-520`; `app/ponto/janelas.py:10-33`; `app/ponto/services/fechamento.py:674-692`). O escritor aceita competência futura: seu universo não limita mês/ano por hoje e a gravação usa a janela pedida (`app/ponto/services/fechamento.py:430-436`, `:610-630`, `:1121-1128`). Existência desse caso em produção: `[não verificado]`. A premissa “competência futura não tem projeção gravada” aparece no comentário de `decidir`, mas não é garantida pelo escritor (`app/ponto/services/ausencia.py:938-943`; `app/ponto/services/fechamento.py:430-436`).

Para ranges finitos válidos, o envelope mínimo dos inícios/máximo dos fins cobre antigo e novo, inclusive deslocamentos nos dois sentidos; sua tradução em competências percorre todas as janelas pelo corte da empresa (`app/ponto/services/ausencia.py:496-520`; `app/ponto/services/fechamento.py:1578-1583`; `app/ponto/services/dia_pago.py:700-716`). Para `NULL` de tipo não aberto, o limite hoje apenas pode invalidar competências extras, pois a cobertura efetiva é um dia (`app/ponto/services/ausencia.py:519`; `app/ponto/turnos.py:2762-2767`).

**Quem escreve/lê `derivado_em`.**

- Escritores com carimbo preenchido: recalcular fechamento aberto por `save`, e demais status por `update` (`app/ponto/services/fechamento.py:1121-1128`, `:1144-1173`).
- Escritores que apagam: `invalidar_fechamento` (`app/ponto/services/fechamento.py:1632-1640`); `restaurar_fechamento`, salvo se o chamador fornecer explicitamente outro valor (`:1364-1377`). O default do modelo é `NULL` (`app/ponto/models.py:454-464`).
- Leitores explícitos do carimbo: `projecoes_vencidas_qs`, contador e comando de listagem; detectam `NULL` e imprimem/contam, sem rederivar (`app/ponto/services/fechamento.py:1675-1683`, `:1705`; `app/ponto/management/commands/projecoes_vencidas.py:47-68`). O default observa somente a competência corrente (`app/ponto/services/fechamento.py:1676-1679`; `app/ponto/janelas.py:102-108`).
- `decidir` escreve invalidação, sem ler o carimbo (`app/ponto/services/ausencia.py:942-944`); `recalcular_por_evento` recalcula incondicionalmente após commit (`app/ponto/services/fechamento.py:1791-1808`); `fechamento_lido` deriva incondicionalmente em memória (`:1743-1745`).
- A folha continua carregando o gravado sem filtro de `derivado_em`: `classificar_export` (`app/folha/export.py:775-780`) e `montar_linhas_export` (`:709-724`). Portanto, carimbo vazio alarma, mas não força atualização dos números consumidos nesses caminhos (`app/ponto/services/fechamento.py:1675-1683`; `app/folha/export.py:709-724`).
- O carimbo é excluído das comparações de fotografia/hash (`app/ponto/portas/regen_exportada.py:35-38`; `app/ponto/management/commands/carimbo_gravado.py:27-30`).

**Exportada/barrada:** a invalidação não filtra status, exportação nem guarda da09, portanto também apaga esses carimbos (`app/ponto/services/fechamento.py:1635-1640`). A reconstrução da09 exportada é barrada pelo universo do escritor (`:549-555`), enquanto a leitura online segue disponível pelo modo `somente_leitura=True` (`:530-531`, `:1743-1745`). O gravado barrado pode permanecer `NULL`, com os números antigos ainda disponíveis ao export (`app/folha/export.py:709-724`). Isso decorre do desenho existente; introdução desse comportamento pelo delta `[não verificado]`.

## 06/11 02:25 · sessao 9456bae2f482 · - · - · origem=Ronald (tmux)

1. **Não bloqueante — lacuna de cobertura da janela.** O teste de range cobre somente mover `27/10` para `23/11`, com `data_fim=None` e status `aguardando_documento`; não cobre fim antigo posterior ao novo, edição para trás, intervalo aberto nem exportada/barrada. O nome promete mais que os casos exercitados. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:72`, `:261-272`.

2. **Não bloqueante — rollback pode passar vazio isoladamente.** `test_linha_desfeita_nao_vence_a_projecao` verifica apenas que `derivado_em` continua preenchido: passa também se o comando pular antes do escritor. Não afirma resumo, minutos restaurados ou que a invalidação aconteceu dentro da transação. O teste irmão da trilha cobre resumo `puladas=2` e minutos restaurados; portanto a suíte tem contraparte, mas este teste sozinho não comprova reversão do carimbo após invalidar. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:254-259`, `:163-169`. O caminho real invalida antes da falha de trilha, que lança `_Desfaz` fora do `atomic`. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `:169-191`; `app/ponto/services/ausencia.py:509-520`.

3. **Testes positivos têm mordida estrutural.** O apply começa com duas projeções preenchidas e exige ambas nulas; o teste direto começa com comp11/12 preenchidas e exige ambas nulas. Suprimir a invalidação deixa esses valores iniciais vivos, salvo outro efeito ainda não demonstrado. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:241-252`, `:261-272`. Execução dos RED/M16/M17: **[não verificado]**; os logs citados no commit não foram encontrados na listagem de `logs`/`app/logs` **[não verificado quanto à existência fora desses diretórios]**.

4. **A cura do GRAVADO é efetiva pelo código do teste.** O helper chama `recalcular_fechamento_mes`, espia `get_motor_cct` devolvendo o motor real, exige que o motor tenha sido chamado e lê `FechamentoMensal` persistido. Exige delta exato `(ok+1, perdido−1)` para 720 e 0; 720 ainda exige semana previamente perdida. Isso não passa por saída vazia. `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:130-139`, `:152-178`. O escritor persiste os campos do resultado real do motor, cuja regra usa presença/justificação dos dias obrigatórios. `app/ponto/services/fechamento.py:1085-1086`, `:1127-1128`; `app/ponto/motor_calculo_v2.py:2595-2608`, `:2629-2630`. Limite: `esmeril_espelho.ler_lavra` é mockado como `{}`, portanto estes testes não demonstram convivência com lavra de produção. `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:136`.

5. **Classes arquiteturais:** os leitores de dinheiro consultam o mesmo juiz `cobre_parte_do_dia`, cujo corpo centraliza tipo parcial, dia único e limite 480; não identifiquei juiz paralelo novo nesses sítios. `app/ponto/services/fechamento.py:681-685`; `app/ponto/services/efeito_ausencia.py:33-55`; `app/ponto/services/credito_parcial.py:20-27`; `app/ponto/turnos.py:2583-2600`. A composição `TIPOS_PAGOS and cobre_parte_do_dia` continua repetida nos quatro sítios, como no apontamento anterior. Mesmas referências.

6. **Relógio:** o delta lê `localdate()` dentro da porta, sem entrada `hoje/agora`; os testes congelam globalmente o relógio. `app/ponto/services/ausencia.py:513-519`; `app/ponto/tests/test_h12h06b_rederivar_minutos.py:36`. A classificação “relógio lido fora da borda” consta do checklist, mas regra específica que faça essa leitura, isoladamente, bloquear a obra: **[não verificado]**. `CLAUDE.md:365-367`.

## 06/11 02:18 · sessao e1c59897b38c · FILA1-ESTADO · leitura · origem=Code

“NO AR” abaixo significa **deploy registrado nos anexos**, sem confirmação independente de produção [nao verificado]. Os hashes são os citados nesses registros; a cópia não permite conferir o histórico Git, e o `git log --oneline -60` não veio no dado fornecido [nao verificado]. [nao verificado]

**MANHA-0511 — estado por item**, conforme a enumeração de `app/docs/BACKLOG.md:1738`:

| Item | Estado atual com prova |
|---|---|
| **1 — F3-BECO-1** | **NO AR**, árvore certificada `3b8d4a3e`, deploy `9c0fd9d3`, rc=0. Permanecem os itens de fila do opus e a lei **F3-PERGUNTAS-DO-APP-463**; portanto, pouso comprovado não equivale a encerramento integral. `app/docs/RELATO.md:6441`, `app/docs/RELATO.md:6444`, `app/docs/RELATO.md:6446`, `app/docs/RELATO.md:59`. |
| **2 — JUIZ-1 vinculo_do_dia** | **NO AR**, v5, deploy `8ece7834` às 13:49, com instalação do crontab registrada. O hash foi depois amendado para `98ce09f8`; outra reescrita preservou as árvores e terminou no HEAD `630131b1`. Correspondência individual final após essa reescrita: [nao verificado]. `app/docs/RELATO.md:78`, `app/docs/RELATO.md:79`, `app/docs/RELATO.md:80`. |
| **3 — 12x36 (B)** | **Incluído na v5 do JUIZ-1 NO AR**: a medida registra os casos col334/col727 passando de feriado para trabalho; o aval vincula expressamente a frota (B) à volta 5. Mesmo deploy `8ece7834` do item 2. `app/docs/RELATO.md:94`, `app/docs/RELATO.md:97`, `app/docs/RELATO.md:78`. |
| **4 — JUIZ-3 (a), outbox** | **NO AR `de9fd69b`**, suite da árvore `0f918814`, deploy rc=0. O registro posterior mede a rodada das 20:30; ainda declara que o crontab ficou sem `--alarme`. `app/docs/RELATO.md:6517`, `app/docs/RELATO.md:6522`, `app/docs/RELATO.md:6523`, `app/docs/RELATO.md:6525`. |
| **5 — JUIZ-4 (i), FIO MUDO** | **Em construção**, como **C20-FIO-MUDO**: abertura do construtor registrada e posteriormente mantida entre os trabalhos ativos. **Hash da implementação e pouso: [nao verificado]**. O `8238da4c` comprova somente **JUIZ-4A**, não este ramo (i). `app/docs/RELATO.md:6512`, `app/docs/RELATO.md:6570`, `app/docs/RELATO.md:109`; distinção de escopo em `app/docs/PROMPTS.md:1875`. |
| **6 — JUIZ-2, retido no TOTAL** | **NO AR `0cd6261a`**, merge da cópia `0c99a2cf`, contendo o produto `78b755f8` e a confirmação de que o retido fica no TOTAL; deploy rc=0. Isso comprova esta parte, não o rebaixamento completo do cron, tratado depois como LASTRO-PARTE-2. `app/docs/RELATO.md:111`, `app/docs/BACKLOG.md:1740`. |
| **7 — O261 item 3, PEP** | **Sem implementação comprovada nesta retomada**: aguardava vaga; depois recebeu pré-código e casos para o futuro construtor. **Hash próprio de cura/pouso: [nao verificado]**. `9c0fd9d3` e `3a514a4e` são bases das leituras registradas. `app/docs/RELATO.md:6427`, `app/docs/RELATO.md:6507`, `app/docs/RELATO.md:6599`, `app/docs/RELATO.md:6602`. |
| **8 — OTIMIZA-3** | **Em raia `wt-qc-224741 @ d1354f77`, sem construtor na última indicação explícita**. A reprovação/B3 antiga está em `RELATO:126`, mas MANHA-0511 declara essa parada respondida; posteriormente a obra continuava aguardando abertura. **Novo hash ou pouso: [nao verificado]**. `app/docs/RELATO.md:126`, `app/docs/RELATO.md:104`, `app/docs/RELATO.md:6472`, `app/docs/RELATO.md:6507`. |

**NOITE-0411 — cobertura da lista que permanece no BACKLOG**, incluindo seus desdobramentos posteriores (`app/docs/BACKLOG.md:1737`):

| Item | Estado atual com prova |
|---|---|
| **1 — E4-8-SAIDA-I** | **NO AR `115196f4`**. O `!` por pouso fora da janela foi **respondido: manter**, pelo aval POUSO-E4-8-MANTEM. `app/docs/RELATO.md:6316`, `app/docs/RELATO.md:212`. |
| **2 — OTIMIZA 1-3** | **OTIMIZA-1:** em raia `wt-si-2235 @ 2d91d590`; faltavam suite, replay, verificador e pouso. PRONTO 5 ficou em fila. **OTIMIZA-2:** registrado parado pela cota, sem retorno à ordem da noite; execução posterior [nao verificado]. **OTIMIZA-3:** estado descrito no item 8 acima. `app/docs/RELATO.md:179`, `app/docs/RELATO.md:169`, `app/docs/BACKLOG.md:1791`, `app/docs/RELATO.md:6472`. |
| **3 — espelho.py::_status_dia** | **NO AR `7f275efd`**, deploy rc=0; catraca viva caiu para 1. `app/docs/RELATO.md:6327`. |
| **4 — E4-APP-MAPA** | **NO AR `b051f0ae`**, deploy rc=0. `app/docs/RELATO.md:6338`. |
| **5 — catraca=0** | **FECHADO por medida viva**, no pouso `b051f0ae`: número 0, itens e novos vazios. `app/docs/RELATO.md:6338`. |
| **6 — E4-1/E4-6 SAIDA-II + JUIZ-1/JUIZ-2** | **E4-1/E4-6 NO AR `fb84b97d`**; **JUIZ-1 NO AR**, conforme item 2 da manhã; **JUIZ-2 parte 1 NO AR `0cd6261a`**. **LASTRO-PARTE-2** permanece em investigação de origem, com cura+vigia aguardando construtor; hash dessa parte [nao verificado]. `app/docs/RELATO.md:210`, `app/docs/RELATO.md:78`, `app/docs/RELATO.md:111`, `app/docs/RELATO.md:48`. |
| **6b — F3-DORMENTE, beco 1** | **NO AR**, deploy `9c0fd9d3`, com a lei residual indicada no item 1 da manhã. `app/docs/RELATO.md:6446`, `app/docs/RELATO.md:59`. |
| **6c — juízes 3 e 4** | **JUIZ-3 NO AR `de9fd69b`**. **JUIZ-4A NO AR `8238da4c`**; **FIO MUDO em construção**, sem hash/pouso comprovado. `app/docs/RELATO.md:6517`, `app/docs/RELATO.md:109`, `app/docs/RELATO.md:6512`. |
| **7 — OTIMIZA 4-5** | Último estado explícito: **aguardavam contrato ao Codex/cota**. Construção, hash ou pouso posterior: **[nao verificado]**. `app/docs/RELATO.md:152`. |
| **9 — motor v3 pela bateria** | **Sem abertura comprovada desta retomada**: o registro diz que não abre sem Codex. Hash ou conclusão desse item da noite: **[nao verificado]**. `app/docs/RELATO.md:134`. |

Nos desdobramentos da noite também há **N1-PERGUNTAS-DO-APP NO AR `26ee6979`**, **LAVRAR-HE NO AR `95544d22`**, **BUG-SW NO AR `4d5fd8df`** e **N1-ALIMENTACAO-UNICA NO AR `e44d7efa`**. `app/docs/RELATO.md:193`, `app/docs/PROMPTS.md:1867`, `app/docs/RELATO.md:184`, `app/docs/RELATO.md:149`.

**Substituições e alterações posteriores:**

- **NOITE-NON-STOP**, de 05/11 00:3x, reordena os pousos da noite; **MANHA-0511**, de 08:3x, declara expressamente substituir as versões anteriores e a ordem da NOITE-0411. O marcador continua literalmente em NOITE-0411. `app/docs/PROMPTS.md:1851`, `app/docs/PROMPTS.md:1853`, `app/docs/BACKLOG.md:1738`, `app/docs/BACKLOG.md:8`.
- Após MANHA, há alterações explícitas: **CRON-MARCA-COBRE-INSTALL** insere (c)(1) antes do item 4; **F3-PORTA-E-CRON-PRAZO** substitui o encerramento previsto no item 1; **LASTRO-PARTE-2** ganha encaixe após JUIZ-1; **CONTRATO-20-DECISOES** libera e redefine JUIZ-3/FIO MUDO numa sequência local. `app/docs/PROMPTS.md:1854`, `app/docs/PROMPTS.md:1857`, `app/docs/PROMPTS.md:1859`, `app/docs/PROMPTS.md:1875`.
- Uma **substituição integral posterior de MANHA-0511** não está comprovada [nao verificado]. O complemento do piloto de 16:4x diz expressamente **“ordem e fila intocados”**. `app/docs/PROMPTS.md:1881`.

## 06/11 02:13 · sessao ef5279bc9e8c · H12H06-B · censo · origem=Code

Nas referências aos anexos, conto as linhas do conteúdo colado, começando em 1 e incluindo linhas vazias.

1. **BLOQUEANTE 1 — (b), com ressalva: visto e adiado para medição; não houve descarte técnico documentado.** O alcance sobre exportadas foi apontado pelo Codex. A triagem deixou o problema sem tratamento nesta obra e encaminhou a medição à sessão. (`codex_v1.r:5`; `codex_v2.q:7`)

   **Pergunta literal:**
   > “O complemento move dinheiro em quem? (classes de ausencia/tipo com minutos>=480 ou range com minutos), e quais leitores de  
   > tela/PDF mudam de numero.” (`codex_v1.q:23-24`)

   **Resposta literal:**
   > “A cerca do comando não protege esse caminho do complemento.” (`codex_v1.r:5`)

   **Motivo registrado na triagem:**
   > “#2 recalculo de exportada pelo complemento: nao curado nesta obra; vai como pergunta a sessao (medir na frota exportada).” (`codex_v2.q:7`)

   A quantificação efetiva do DIFF pertence a **(c), dado de produção**. O Codex delimitou isso expressamente:
   > “Pessoas e valores efetivamente alterados em produção: **[não verificado]**.” (`codex_v1.r:49`)

   Portanto, o risco foi visto; a exigência específica de DIFF com filtro e sentido não aparece formulada na resposta. (`codex_v1.r:5`; `codex_v1.r:49`; `opus_final_4e9380e5.md:2`)

2. **BLOQUEANTE 2 — (a): não apontado, com o tema presente na pergunta.** A pergunta cobrava efeitos da edição que escapassem à conferência local; a resposta enumerou efeitos externos, mas não apontou a falta de invalidação do fechamento. (`codex_v1.q:14-16`; `codex_v1.r:39-41`)

   **Pergunta literal:**
   > “Ha efeitos colaterais do editar_ausencia_registro + Ausencia.save (signals, rejulgar, cartorio,  
   > prazo_documento, aprovada_em) que a conferencia "so minutos mudou" nao pega, ou que pegaria e desfaria indevidamente?” (`codex_v1.q:15-16`)

   **Resposta literal:**
   > “A comparação não alcança efeitos em `CelulaDia`, eventos, chamados ou perguntas.” (`codex_v1.r:41`)

   A cópia permite conferir a omissão: `editar_ausencia_registro` salva e retorna sem chamar `invalidar_fechamento`; a chamada aparece em `decidir`. (`app/ponto/services/ausencia.py:508-515`; `app/ponto/services/ausencia.py:925-932`)

3. **NÃO BLOQUEANTE — comentário desatualizado: (e), NÃO PERGUNTADO.** As perguntas fornecidas não cobram atualização de comentários; as respostas não registram esse comentário como achado. Não há trecho literal específico de pergunta ou resposta para reproduzir. (`codex_v1.q:10-28`; `codex_v2.q:17-24`; `codex_v3.q:14-20`; `codex_v4.q:13-19`; `codex_v1.r:1-53`; demais respostas fornecidas)

   O comentário citado pelo opus existe na cópia:
   > “somam `minutos` de `TIPOS_PAGOS` aprovados” (`app/ponto/services/ausencia.py:920`)

   Atribuir à triagem um descarte desse comentário seria **[não verificado]**.

4. **NÃO BLOQUEANTE — predicado composto repetido: (a), pelo tema amplo perguntado.** A pergunta cobrou duplicação de juiz e derivação paralela. A resposta verificou a chamada ao juiz existente, sem registrar como achado a repetição da combinação com `TIPOS_PAGOS`. (`codex_v1.q:25-26`; `codex_v1.r:51`; `opus_final_4e9380e5.md:7`)

   **Pergunta literal:**
   > “2o juiz que duplica juiz registrado” (`codex_v1.q:25`)
   >
   > “derivacao paralela de fonte unica” (`codex_v1.q:26`)

   **Resposta literal:**
   > “não encontrei juiz novo nos três leitores migrados: todos chamam `cobre_parte_do_dia`” (`codex_v1.r:51`)

   A pergunta não nomeou expressamente a repetição do **predicado composto**; a classe (a) decorre da abrangência do tema cobrado. (`codex_v1.q:25-26`)

5. **NÃO BLOQUEANTE — teste não afirma o resultado em FechamentoMensal: VISTO, mas sem descarte documentado.** Aqui a classificação oferecida não fecha: não cabe (a), pois o Codex descreveu precisamente a limitação; para afirmar (b), falta o registro do descarte e seu motivo. (`codex_v1.r:53`; `triagem_v4.md:2-6`) [nao verificado]

   **Pergunta literal:**
   > “selo/RED sem mordida (mock que nao morde, teste vacuo)” (`codex_v1.q:26`) [nao verificado]

   **Resposta literal:**
   > “Porém ele verifica a alimentação `datas_justificadas`, não os valores financeiros persistidos ou o PDF.” (`codex_v1.r:53`)

   **Motivo de eventual descarte pela triagem: [não verificado].** Classificá-lo como “Codex não viu” ou “NENHUM viu” contrariaria esse trecho explícito. (`codex_v1.r:53`)

## 06/11 02:12 · sessao 08d6671f0bc7 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE 1: visto; encaminhado à medição.** Aproxima-se de **(b)**, mas “descarte” não está documentado: foi mantido aberto. `codex_v1.q`, pergunta 6: “O complemento move dinheiro em quem? [...] e quais leitores de tela/PDF mudam de numero.” `codex_v1.r`, achado 2: “o complemento pode mudar exportada por recálculo genérico.” `codex_v2.q`, triagem: “#2 recalculo de exportada pelo complemento: nao curado nesta obra; vai como pergunta a sessao (medir na frota exportada).” A medição efetiva do DIFF é **(c)**; resultado não fornecido. Linhas dos anexos: [não verificado].

2. **BLOQUEANTE 2: (a).** Tema presente em `codex_v1.q`, pergunta 2: “Ha efeitos colaterais do editar_ausencia_registro + Ausencia.save [...] que a conferencia ‘so minutos mudou’ nao pega”. O próprio texto do commit anexado diz “FechamentoMensal fica stale ate recalculo”. A resposta percorre efeitos, mas não aponta ausência de invalidação: “A comparação não alcança efeitos em `CelulaDia`, eventos, chamados ou perguntas.” (`codex_v1.r`). Linhas dos anexos: [não verificado]. [nao verificado]

3. **NB comentário: (e).** Nenhuma pergunta fornecida cobra comentário desatualizado ou sincronização da documentação de `ausencia.py:920-921`. Não existe trecho positivo de `.q` ou `.r` para esse achado; ausência constatada nos anexos, linhas [não verificado].

4. **NB predicado composto: (a), pelo tema amplo; fronteira com (e).** `codex_v1.q`, pergunta 7: “2o juiz que duplica juiz registrado; [...] derivacao paralela de fonte unica”. `codex_v1.r`: “não encontrei juiz novo nos três leitores migrados: todos chamam `cobre_parte_do_dia`”. A resposta conferiu o componente chamado, sem registrar a repetição da combinação. A pergunta não nomeia explicitamente duplicação do predicado composto; se exigir literalidade estrita do objeto, seria (e). Linhas dos anexos: [não verificado].

5. **NB teste FechamentoMensal: visto, sem destino documentado — taxonomia incompleta.** `codex_v1.q`, pergunta 7: “selo/RED sem mordida (mock que nao morde, teste vacuo)”. `codex_v1.r`: “Porém ele verifica a alimentação `datas_justificadas`, não os valores financeiros persistidos ou o PDF.” Isso impede (a), (d) e (e); a triagem fornecida não registra descarte nem motivo, portanto **(b) não está comprovada**. Linhas dos anexos e motivo do eventual descarte: [não verificado]. [nao verificado]

Não classificaria nenhum dos cinco como **(d)**: há achados vistos pelo Codex e omissões de pergunta; “nenhum viu” também seria incompatível com o próprio achado do opus. [não verificado]

## 06/11 02:07 · sessao 657abac16215 · H12H06-B · sonda · origem=Code

**Parte A — funções reais e limite da conferência.** O conteúdo de `sonda_h12h06b.py` não foi localizado na cópia nem consta do anexo transcrito; portanto, seus imports, argumentos, filtros e eventuais reconstruções de lógica ficam **[nao verificado]**. A vinculação da cópia ao HEAD informado também fica **[nao verificado]**. O identificador entre colchetes no cabeçalho do envelope é o SHA-256 abreviado **do script**, calculado em `bin/sonda_leitura.sh:219` e impresso em `bin/sonda_leitura.sh:221`.

As quatro funções existem com estas assinaturas:

| Função | Assinatura real | Fonte |
|---|---|---|
| `parciais_vigentes` | `parciais_vigentes()` | `app/ponto/catalogo/ausencias.py:336` |
| `cobre_parte_do_dia` | `cobre_parte_do_dia(a, parciais=None)` | `app/ponto/turnos.py:2583` |
| `janela_atual` | `janela_atual(data, empresa=None)` | `app/ponto/janelas.py:60` |
| `competencia_entregue` | `competencia_entregue(empresa_id, mes, ano, registro=False)` | `app/folha/export.py:556` |

`parciais_vigentes()` seleciona a flag de parcial do catálogo vigente; `cobre_parte_do_dia()` exige tipo nesse conjunto, registro de um único dia e **`0 < minutos < 480`**. `janela_atual()` recebe a data primeiro e aplica o corte da empresa. `competencia_entregue()` consulta a exportação vigente e exige que não esteja invalidada. (`app/ponto/catalogo/ausencias.py:357`; `app/ponto/turnos.py:2580`; `app/ponto/turnos.py:2595`; `app/ponto/turnos.py:2598`; `app/ponto/janelas.py:63`; `app/folha/export.py:580`)

**O filtro `status='aprovada', minutos>0` não é o universo integral dos leitores.** Para folha, o carregador filtra aprovadas, mas não exige minutos positivos; para cobrança, exclui apenas rejeitadas. Os carregadores também consideram interseção de datas e os intervalos abertos permitidos. (`app/ponto/turnos.py:2514`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2529`; `app/ponto/turnos.py:2753`)

Quem lê e como trata minutos:

| Leitor | Leitura efetiva | Fonte |
|---|---|---|
| Fechamento mensal | Carrega ausências para folha; soma minutos de tipos pagos e pula o efeito de dia inteiro quando esses minutos são não zero | `app/ponto/services/fechamento.py:630`, `:665`, `:685` |
| Efeitos da ausência | Usa os carregadores para folha e o mesmo teste de tipo pago com minutos não zero | `app/ponto/services/efeito_ausencia.py:43`, `:44`, `:49`, `:51` |
| Crédito parcial | Acumula minutos de tipos pagos pela data inicial | `app/ponto/services/credito_parcial.py:23`, `:24` |
| Exportação | Filtra aprovadas, tipos solicitados e interseção da janela, sem exigir minutos positivos | `app/folha/export.py:87`, `:88`, `:90` |

Nos três primeiros leitores, esses testes locais usam **tipo pago + verdade lógica de `minutos`**, sem chamar `cobre_parte_do_dia()` nesses pontos; portanto, não aplicam ali o limite de 480 nem a condição de dia único do juiz. São tratamentos locais distintos do predicado real. Reconstruções dentro **da sonda** permanecem indeterminadas. (`app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/turnos.py:2598`) **[nao verificado — conteúdo da sonda]**

**Parte B — saída fornecida.** Nela, **`falta` não está em `parciais_vigentes()`**: o resultado é `false`, e a lista contém `abon_hr`, `atestado`, `atraso`, `declaracao`, `saida_antecipada`. **[nao verificado — dado fornecido, sem nova consulta]**

A tabela abaixo preserva `n` e os identificadores recebidos, inclusive quando há vários registros para um mesmo identificador. **[nao verificado — dado fornecido]**

| Empresa | Competência | Exportada | Tipo | n | Colaboradores | Evidência |
|---:|:---:|:---:|---|---:|---|---|
| 2 | 06/2026 | false | falta | 1 | col43 | [nao verificado] |
| 4 | 06/2026 | true | falta | 3 | col39, col40, col47 | [nao verificado] |
| 3 | 07/2026 | true | abono | 1 | col148 | [nao verificado] |
| 3 | 07/2026 | true | falta | 2 | col64 | [nao verificado] |
| 4 | 07/2026 | true | falta | 1 | col676 | [nao verificado] |
| 2 | 08/2026 | true | abono | 3 | col666, col819, col905 | [nao verificado] |
| 2 | 08/2026 | true | atestado | 7 | col284, col342, col400, col433, col707, col842 | [nao verificado] |
| 2 | 08/2026 | true | falta | 46 | col187, col200, col228, col237, col272, col328, col335, col338, col344, col347, col366, col406, col433, col442, col489, col493, col552, col568, col718, col760, col783, col823, col874 | [nao verificado] |
| 2 | 08/2026 | true | saida_antecipada | 1 | col649 | [nao verificado] |
| 3 | 08/2026 | false | atestado | 1 | col159 | [nao verificado] |
| 3 | 08/2026 | false | falta | 8 | col58, col60, col122, col128, col743, col769 | [nao verificado] |
| 4 | 08/2026 | false | abono | 1 | col712 | [nao verificado] |
| 4 | 08/2026 | false | atestado_inss | 1 | col42 | [nao verificado] |
| 4 | 08/2026 | false | falta | 3 | col46, col50 | [nao verificado] |
| 2 | 09/2026 | true | atestado | 3 | col518, col885, col906 | [nao verificado] |
| 2 | 09/2026 | true | falta | 114 | col174, col200, col268, col304, col325, col327, col331, col338, col370, col406, col417, col437, col443, col492, col493, col558, col566, col572, col576, col584, col594, col622, col659, col666, col667, col673, col697, col704, col726, col735, col782, col786, col814, col823, col824, col855, col877, col894, col905, col906 | [nao verificado] |
| 2 | 09/2026 | true | suspensao | 1 | col417 | [nao verificado] |
| 3 | 09/2026 | true | atestado | 2 | col111, col643 | [nao verificado] |
| 3 | 09/2026 | true | falta | 16 | col92, col154, col161, col173, col502, col515, col643, col746, col876 | [nao verificado] |
| 3 | 09/2026 | true | suspensao | 1 | col90 | [nao verificado] |
| 4 | 09/2026 | true | atestado | 1 | col40 | [nao verificado] |
| 4 | 09/2026 | true | falta | 2 | col50, col510 | [nao verificado] |
| 1 | 10/2026 | false | falta | 1 | col950 | [nao verificado] |
| 2 | 10/2026 | false | atestado | 17 | col204, col252, col354, col389, col446, col511, col518, col558, col566, col618, col626, col889, col905, col920 | [nao verificado] |
| 2 | 10/2026 | false | falta | 83 | col55, col200, col206, col207, col248, col304, col307, col335, col370, col373, col427, col432, col444, col507, col569, col576, col577, col622, col655, col666, col697, col699, col718, col736, col833, col885, col889, col893, col924, col928, col929, col944, col951, col954, col966, col968 | [nao verificado] |
| 3 | 10/2026 | false | falta | 6 | col113, col173, col643, col752, col769 | [nao verificado] |
| 4 | 10/2026 | false | falta | 2 | col510, col857 | [nao verificado] |
| 2 | 11/2026 | false | atestado | 4 | col200, col701, col891, col967 | [nao verificado] |
| 2 | 11/2026 | false | falta | 20 | col298, col304, col364, col444, col566, col570, col723, col823, col869, col885, col905, col924, col928, col966 | [nao verificado] |
| 3 | 11/2026 | false | falta | 2 | col769, col984 | [nao verificado] |
| 2 | 09/2029 | false | falta | 1 | col667 | [nao verificado] |

**As linhas detalhadas somam 355, não 448.** Os 93 de `parcial_pelo_juiz` aparecem somente no agregado; sua distribuição por empresa, competência, exportada, tipo e colaboradores é **indeterminada**. **[nao verificado — cálculo e limites do dado fornecido]**

| Parcela | n | Evidência |
|---|---:|---|
| Linhas detalhadas com `exportada=false` — coincide numericamente com `aberta` | 151 | [nao verificado] |
| Linhas detalhadas com `exportada=true` — coincide com `exportada` | 204 | [nao verificado] |
| Subtotal detalhado | 355 | [nao verificado] |
| `parcial_pelo_juiz`, sem detalhamento fornecido | 93 | [nao verificado] |
| **Universo: 151 + 204 + 93** | **448** | [nao verificado] |

Combinações ausentes não foram preenchidas com zero; a composição individual dos 93 permanece indeterminada. **[nao verificado — ausência de detalhamento no dado]**

**Parte C(A) — competência exportada grava, com exceção explícita para 09/2026.** No modo de escrita, exportação gera aviso; os fechamentos abertos são salvos com `save()`, e os demais recebem `update()` dos campos calculados. Entretanto, `GUARDA_COMPETENCIA` contém **09/2026**, e os colaboradores das empresas exportadas nessa competência são excluídos do processamento, preservando seus fechamentos nessa chamada. (`app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:532`; `app/ponto/services/fechamento.py:551`; `app/ponto/services/fechamento.py:555`; `app/ponto/services/fechamento.py:1121`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1144`)

Com `somente_leitura=True`, não grava `FechamentoMensal`; a combinação com `lavrar_dias_pagos=True` pode escrever `DiaPago`. (`app/ponto/services/fechamento.py:1112`; `app/ponto/services/fechamento.py:1175`)

**Parte C(B) — sim, há caminho indireto de cron para competência anterior.** `detectar_par_relampago` está agendado com `--dias 2 --apply --retratar`; a busca retrocede dois dias e pode atravessar a virada da competência. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:83`; `app/ponto/management/commands/detectar_par_relampago.py:335`)

A cadeia é: comando chama `_retratar(alvo)` → `retratar_batida()` → `_fechamento_por_evento()` com o timestamp da batida → `recalcular_por_evento()` → `recalcular_fechamento_mes()` na competência **do fato**, após commit. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/registro_batida.py:281`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)

O comando mensal `recalcular_fechamento` está declarado fora do pipeline; isso não elimina o caminho indireto acima. (`app/config/crons.py:767`; `app/config/crons.py:1107`) Ocorrência efetiva desse recálculo em produção: **[nao verificado]**.

**Parte C(C) — outros caminhos capazes de recalcular o passado, além do botão:**

| Caminho | Chamada e evidência |
|---|---|
| Registro e retratação de batida | `app/ponto/registro_batida.py:155`, `:160`, `:178`, `:281` |
| Decisão de HE referente ao dia informado | `app/ponto/portas/he.py:202` |
| Folga aceita e resposta validada | `app/chamados/services/validacao.py:331`, `:407` |
| Regeneração de células do vínculo, por competência tocada | `app/ponto/portas/celula.py:414`, `:422`, `:427` |
| Regeneração de exportada, chamada pelo passivo validado | `app/chamados/services/passivo_validado.py:429`, `:438`; `app/ponto/portas/regen_exportada.py:179` |
| Comando `recalcular_fechamento --mes --ano --apply` | `app/ponto/management/commands/recalcular_fechamento.py:78`, `:79`, `:110`, `:118` |
| Comando `aplicar_09_corte_b` | `app/ponto/management/commands/aplicar_09_corte_b.py:212` |
| Comando `diff_reclassificar_partido`, com `--aplicar` | `app/ponto/management/commands/diff_reclassificar_partido.py:206`, `:213`, `:232` |
| Comando `aplicar_janela_he_total`, delegando ao comando mensal | `app/ponto/management/commands/aplicar_janela_he_total.py:145` |
| Scripts para 10/2026, nos respectivos ramos de aplicação | `bin/aplicar_o263_comp10.py:26`, `:48`, `:51`; `bin/relavrar_o118_col221.py:19`, `:52`, `:53`; `bin/relavrar_o119_col221.py:18`, `:51`, `:52`; `bin/r5_idempotencia_frota.py:26`, `:117` |

Esses caminhos continuam sujeitos à guarda de 09/2026 no escritor. (`app/ponto/services/fechamento.py:551`) Já `fechamento_lido()` também recalcula competências solicitadas, mas usa `somente_leitura=True`, sem persistir `FechamentoMensal`. (`app/ponto/services/fechamento.py:1743`)

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual; essa chamada é incompatível com a cópia. (`app/ponto/services/fechamento.py:439`)

A execução efetiva desses caminhos sobre os registros do dado fornecido permanece **[nao verificado]**.

## 06/11 02:04 · sessao a2114b554fe1 · - · - · origem=Ronald (tmux)

C(A): **exportada em geral grava `FechamentoMensal`**, mas **09/2026 exportada é preservada pela guarda ativa**: competência declarada em `app/ponto/services/fechamento.py:36`; empresas exportadas detectadas em `:530`; colaboradores dessas empresas excluídos do universo em `:551–555`. Para os demais casos, `status='aberto'` usa `save()` em `:1121–1128`; outros status usam `update()` dos campos calculados em `:1144–1174`. `somente_leitura=True` não grava o fechamento (`:1112–1120`), embora a combinação com `lavrar_dias_pagos=True` possa gravar `DiaPago` (`:1175`).

C(C): além do botão, recalculam competências passadas pelos seguintes caminhos do código:

- **Eventos retroativos:** `recalcular_por_evento` determina a competência pela data do fato e chama o escritor (`app/ponto/services/fechamento.py:1793–1794`), após commit (`:1808`). Chamadores: batida (`app/ponto/registro_batida.py:155`, `:160`, `:178`); decisão HE (`app/ponto/portas/he.py:202`); folga aceita (`app/chamados/services/validacao.py:331`); resposta validada (`:407`); regeneração de vínculo, para cada competência tocada (`app/ponto/portas/celula.py:414–429`).
- **Retificação de exportada:** `regenerar_em_exportada` chama diretamente o escritor (`app/ponto/portas/regen_exportada.py:179`); o serviço de passivo validado chama essa porta quando houve plantio em exportada (`app/chamados/services/passivo_validado.py:429–439`).
- **Comando operacional:** `recalcular_fechamento --mes --ano --apply` (`app/ponto/management/commands/recalcular_fechamento.py:77–81`, `:110–120`).
- **Outros comandos que podem persistir:** `aplicar_09_corte_b.py:212`; `diff_reclassificar_partido.py:206` com commit no ramo `--aplicar` (`:213–214`, `:232–238`); `aplicar_janela_he_total.py:145` delega ao comando operacional. [nao verificado]
- **Scripts existentes:** `bin/aplicar_o263_comp10.py:26`, `:48–52`; `bin/relavrar_o118_col221.py:19`, `:52–53`; `bin/relavrar_o119_col221.py:18`, `:51–52`; `bin/r5_idempotencia_frota.py:26`, `:117`.

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual (`app/ponto/services/fechamento.py:439–442`); essa chamada está incompatível com a cópia.

Medidores com recálculo seguido de rollback: `desvio_o68b.py:93–95`, `folga_que_sumiu.py:69–71`, `plano_b_no_dinheiro.py:109–112`. `celula_veredito_velho.py` desliga recálculo no apply (`:95–99`); no ensaio, recalcula e reverte (`:146–157`).

Ocorrência efetiva desses caminhos em produção: **[não verificado]**.

## 06/11 02:04 · sessao 573d59eacfa9 · - · - · origem=Ronald (tmux)

**C(B): sim, existe caminho indireto de cron para recálculo de competência anterior.** O cron `detectar_par_relampago` roda com `--dias 2 --apply --retratar`; o recorte usa hoje menos dois dias, sem limitar pela competência corrente. Assim, na virada da janela, pode alcançar a anterior. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:335`; `app/ponto/management/commands/detectar_par_relampago.py:343`)

Cadeia executável:

- O comando chama `_retratar(alvo)` ou `_retratar` nas duas batidas do par. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:399`)
- `_retratar` chama `retratar_batida`. (`app/ponto/management/commands/detectar_par_relampago.py:224`)
- `retratar_batida` chama `_fechamento_por_evento` com o **timestamp da batida retratada**. (`app/ponto/registro_batida.py:281`)
- O helper chama `recalcular_por_evento`; este resolve a competência daquele fato e chama `recalcular_fechamento_mes`, após commit. (`app/ponto/registro_batida.py:177`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)
- A competência é obtida pela data do fato, através de `competencia_do_fato` → `competencias_do_fato` → `competencias_do_intervalo`. (`app/ponto/services/fechamento.py:1588`; `app/ponto/services/fechamento.py:1578`; `app/ponto/services/fechamento.py:1583`)

A declaração de que `recalcular_fechamento` está fora do pipeline refere-se ao comando mensal direto; ela não elimina essa cadeia indireta. (`app/config/crons.py:767`; `app/config/crons.py:1107`)

O cron `pre_fechamento --apply` escreve pautas e apenas consulta os marcos da competência anterior; seu corpo não chama o recálculo mensal. (`app/config/crons.py:406`; `app/folha/management/commands/pre_fechamento.py:59`; `app/folha/management/commands/pre_fechamento.py:61`; `app/folha/management/commands/pre_fechamento.py:73`; `app/folha/management/commands/pre_fechamento.py:74`)

Ocorrência efetiva desse recálculo indireto em produção, frequência e colaboradores atingidos: **[não verificado]**.

## 06/11 02:04 · sessao 07f1d044bfe0 · - · - · origem=Ronald (tmux)

- `status='aprovada'` é o universo por status da folha: `_pelo_efeito(..., para='folha')` filtra aprovada; cobrança lê todas exceto rejeitada. `minutos>0` não integra esse carregador: `ausencias_do_periodo` e `cobertura_ausencia_periodo` leem interseção de datas, incluindo ranges abertos permitidos, sem filtro de minutos. Portanto aprovada + minutos>0 é recorte dos leitores, não seu universo integral. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2753`)

- Quem lê esse universo: fechamento carrega `ausencias_do_periodo(..., para='folha')`; efeitos da ausência e crédito parcial fazem a mesma chamada. (`app/ponto/services/fechamento.py:629`, `app/ponto/services/fechamento.py:630`, `app/ponto/services/efeito_ausencia.py:43`, `app/ponto/services/credito_parcial.py:23`)

- As três implementações de dinheiro usam **tipo pago e minutos não zero**, sem perguntar `cobre_parte_do_dia`: fechamento soma minutos e pula o efeito de dia inteiro; efeitos da ausência faz o mesmo; crédito parcial acumula por data. Isso difere do juiz, que exige tipo em parciais, registro de um só dia e `0 < minutos < 480`. Ressalva: o código usa verdade lógica de `minutos`, não comparação literal `>0`; o campo é `IntegerField`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2595`, `app/ponto/turnos.py:2598`, `app/ponto/models.py:755`)

- A tela do espelho e o PDF leem `efeitos_da_ausencia`; o fechamento também usa `creditos_por_dia`. (`app/ponto/services/espelho.py:1048`, `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`, `app/relatorios/pdf_espelho.py:333`, `app/ponto/services/fechamento.py:978`, `app/ponto/services/fechamento.py:983`)

- Exportação tem outro leitor: `ausencias_na_janela` filtra aprovadas, tipos pedidos e interseção da janela, sem filtro de minutos; `minutos_de_falta_por_dia` usa tipos que descontam e rateia minutos pelos dias. (`app/folha/export.py:87`, `app/folha/export.py:88`, `app/folha/export.py:90`, `app/folha/export.py:138`, `app/folha/export.py:140`, `app/folha/export.py:141`)

- Não localizei `sonda_h12h06b.py` nem com `rg --files -uuu`; ausência do script impede conferir as chamadas que ele efetivamente fez ou identificar reconstruções dentro dele. [não verificado]

## 06/11 01:57 · sessao 43ff11e3f048 · H12H06-B · verificador · origem=Code

**Alta — a cura de #2 não fecha.** `mudar_estado` grava status, minutos e range sem mover `atualizado_em`; depois tenta registrar a trilha. Essa tentativa pode falhar sem desfazer a ausência, e `registrar_log` não verifica o retorno. (`app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:820`, `app/core/auditoria.py:22`, `app/core/observ.py:105`, `app/core/observ.py:139`)

Contraexemplo: depois do apply, rejeitar e reabrir um atestado, com falha nas duas trilhas, devolve status/minutos/identidade/carimbo aos valores esperados. A reversão não encontra log posterior e restaura 720 sobre o ato posterior. Há também uma janela sem falha definitiva: duas chamadas podem concluir os saves da ida/volta e ficar suspensas antes dos respectivos logs. (`app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:1125`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`) Ocorrência em produção: [não verificado].

**Média — #3 fecha o relato da linha, mas permanece parcialmente aberto para recuperação.** O resultado recebe a linha antes da confirmação, conforme pretendido. Porém, se uma falha de escrita deixar a confirmação final truncada, o leitor executa `json.loads` sobre todas as linhas e aborta antes de montar qualquer reversão, inclusive das linhas anteriores corretamente confirmadas. Falta tolerância ao fragmento final do diário. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:254`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222`) Reprodução dessa falha: [não verificado].

**Alta — contesto o descarte categórico da corrida com exportação.** O comando trava somente a ausência, consulta a cerca e depois escreve. A consulta de competência entregue não estabelece trava compartilhada com o exportador, que registra `ExportacaoDominio` independentemente. Portanto, um export concorrente pode entregar a competência entre a conferência e a alteração. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:160`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`, `app/folha/export.py:581`, `app/folha/models.py:182`, `app/folha/views.py:297`) Ocorrência e fundamento do descarte anterior: [não verificado].

Quanto às demais curas e hipóteses:

- **#1 fecha o caso descrito:** o objeto é recarregado e o carimbo retornado dentro da transação que mantém a trava. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:173`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189`)
- **PK repetido:** a confirmação consome a última foto pendente daquele PK; não reutiliza indiscriminadamente todas as fotos anteriores. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:233`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:235`)
- **PK de log versus commit:** o filtro compara um identificador automático, não uma sequência de commits. Não identifiquei escritor concreto com log menor e alteração posterior da mesma ausência que atravesse todas as demais guardas; esse contraexemplo permanece [não verificado]. (`app/core/migrations/0002_log_auditoria.py:19`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Trilha própria da reversão:** ela também grava evento e confirma seu próprio PK; o filtro estritamente `pk__gt` não conta essa mesma trilha como posterior. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Push do apply desfeito:** existe caminho síncrono entre save, julgamento e envio antes das verificações que podem provocar rollback. Entretanto, alcançar esse envio nas 30 linhas depende de condições não fornecidas; não o promovo a achado específico desta obra. (`app/ponto/services/ausencia.py:508`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:778`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175`) Alcance nas 30: [não verificado].

**Quem escreve LogAuditoria com `modelo='Ausencia'`:**

| Origem | Arquivo:linha |
|---|---|
| Criação, estado, documento e período | `app/ponto/services/ausencia.py:441`, `:820`, `:854`, `:915` |
| Cancelamento em lote, aprovação apesar do anexo, cancelamento individual e edição | `app/ponto/views.py:2357`, `:2614`, `:2739`, `:2808` |
| Lançamento pela IA | `app/core/views_ia.py:991` |
| Devolução de perguntas absorvidas | `app/chamados/services/materializacao.py:663` |
| Cura do fim | `app/ponto/management/commands/curar_ausencia_sem_fim.py:47` |
| Apply e reversão desta obra | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182` |
| Migrações históricas, diretamente | `app/ponto/migrations/0057_a4_backfill_pendente.py:44`, `app/ponto/migrations/0058_a4_relogio_recomeca.py:43`, `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:37` |

O escritor físico atual é `evento`; `registrar_log` é adaptador. (`app/core/observ.py:134`, `app/core/auditoria.py:22`)

**Quem escreve Ausencia sem trilha própria:**

- `editar_ausencia_registro` e `rederivar_fim` fazem save completo; os chamadores identificados registram a trilha. (`app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:1280`, `app/ponto/views.py:2808`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`)
- A rejeição de férias não registra trilha da ausência, mas move explicitamente `atualizado_em`. (`app/ponto/services/ausencia.py:1267`)
- Há exclusões sem trilha própria; a reversão detecta que a linha desapareceu. (`app/ponto/services/ausencia.py:1174`, `app/ponto/services/ausencia.py:1197`, `app/ponto/services/ausencia.py:1254`, `app/ponto/services/ausencia.py:1274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:140`)
- Há saves de metadados sem carimbo/log próprio: prazo; decisão sem mudança de estado e sem período; documento/descrição/prazo na reabertura. Eles não demonstram, isoladamente, alteração invisível de minutos/identidade/status. (`app/ponto/services/ausencia.py:536`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:1114`, `app/ponto/services/ausencia.py:1124`, `app/ponto/services/ausencia.py:1132`)

**Modelo/objeto diferentes:** o cancelamento de férias altera ausências e registra o ato em `AgendamentoFerias`, com o PK do agendamento; a alteração das ausências carimba `atualizado_em`. O aviso de recusa registra `Colaborador`, mas não altera uma ausência. Nenhum desses caminhos fornece o contraexemplo invisível procurado. (`app/ferias/services.py:356`, `app/ferias/views.py:410`, `app/ponto/services/ausencia.py:1267`, `app/api/views.py:1937`)

## 06/11 01:57 · sessao d295a10a7b09 · - · - · origem=Ronald (tmux)

**Achado adicional sólido, média — #3 continua parcial se a confirmação ficar truncada.** O diário faz `write`, `flush` e `fsync`; uma falha que deixe um fragmento JSON final não é tratada ([comando:254](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:254)). A reversão interpreta todas as linhas com `json.loads` antes de montar o plano; o fragmento aborta também a reversão das linhas anteriores confirmadas ([comando:222](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222)). Ocorrência real: [não verificado].

Se a última confirmação falhar e o `finally` conseguir gravar o resultado, `completo` será `true`: a última linha já entrou em `saida` antes da confirmação ([comando:270](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270), [comando:285](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:285)). **Não elevar isoladamente:** o próprio predicado define completude como quantidade de linhas processadas, que pode estar completa apesar da confirmação ausente ([comando:285](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:285)).

**Sem novo achado de `on_commit`:** o consumidor de Ausencia envolve o trabalho em `engolir`; não há nesse callback um caminho útil desprotegido demonstrado que impeça o retorno após commit ([signals:254](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:254), [signals:260](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:260), [signals:263](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:263)). A trilha auxiliar `perguntas_devolvidas_ausencia_rejeitada` exige status rejeitada, ausente da LISTA ([materializacao:638](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/chamados/services/materializacao.py:638), [comando:51](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:51)).

**PK versus commit:** escritor concreto com log anterior à escrita, pk menor e alteração pós-apply capaz de passar todas as guardas: [não verificado]. Não promover esse risco genérico. [nao verificado]

## 06/11 01:55 · sessao d295a10a7b09 · - · - · origem=Ronald (tmux)

- **Alta — cura #2 não fecha quando a trilha falha.** `mudar_estado` salva `status`, `minutos` e `data_fim` sem `atualizado_em`, depois chama a auditoria; não exige sucesso dela ([ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:807), [ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:818)). `evento` engole a falha e retorna `None`, e `registrar_log` descarta esse retorno ([observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:105), [observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:139), [auditoria.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/auditoria.py:22)). Contraexemplo: após apply, rejeitar e reabrir o atestado com falha nos dois logs deixa identidade, minutos e carimbo iguais; a reversão aceita e restaura 720, porque só procura logs existentes com pk maior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152)). Ocorrência em produção: [não verificado].

- **Alta — mesma cura possui janela mesmo sem falha definitiva.** A escrita de `mudar_estado` termina antes da chamada de auditoria, sem transação englobando ambas ([ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:792), [ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:819)). Duas chamadas intercaladas podem completar a ida/volta de estado e parar antes dos respectivos logs; nesse intervalo a reversão não encontra trilha posterior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157)). Interleaving executado: [não verificado].

- **Cura #1 fecha o caso descrito:** o objeto é recarregado e o carimbo devolvido dentro do `atomic` que mantém a trava ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:173), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189)).

- **Trilha própria da reversão:** ela grava evento com o próprio pk e confirma esse pk; o filtro usa `pk__gt`, portanto não conta essa mesma trilha como ato posterior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157)).

- **PK versus commit:** o modelo não contém campo de ordenação de commits; `pk__gt` mede pk, e a criação acontece dentro da transação do chamador ([models.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/models.py:285), [observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:133)). Um escritor concreto com log menor e alteração da mesma ausência depois do apply: [não verificado]; não apresento a hipótese genérica como achado.

## 06/11 01:55 · sessao b77f0a7fd3d0 · - · - · origem=Ronald (tmux)

- **Exportação — corrida estrutural, gravidade alta se descarte negava a possibilidade.** A trava cobre somente Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`); a cerca consulta entrega antes da escrita (`:160`, `:169`), enquanto `competencia_entregue` apenas consulta (`app/folha/export.py:581`) e o export registra sem trava compartilhada (`app/folha/models.py:182`, chamado em `app/folha/views.py:297`). Um export pode registrar entre a cerca e a alteração; ocorrência ou disponibilidade operacional na produção: [não verificado].

- **Push do apply desfeito — caminho síncrono existe, mas alcance específico não demonstrado.** O save (`app/ponto/services/ausencia.py:508`) dispara julgamento síncrono (`app/ponto/signals.py:143`, `:147`, `:93`), que pode emitir (`app/ponto/services/cartorio.py:1740`, `:1778`) e enviar push (`app/ponto/management/commands/detectar_ausencias.py:778`) antes dos checks que provocam rollback (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175`, `:178`, `:186`). Gravidade média condicional; células, vínculos, chamados e gates necessários para alcançar essas emissões nas 30 linhas: [não verificado]. O ramo flexível cala o push (`app/ponto/management/commands/detectar_ausencias.py:777`), e a cobertura parcial depende de tipo cadastrado e minutos abaixo de 480 (`app/ponto/turnos.py:2595`).

Autores explícitos de LogAuditoria com modelo `Ausencia` fora de `services/ausencia.py`:

- `app/core/views_ia.py:991`: lançamento por IA.
- `app/ponto/views.py:2357`: cancelamento em lote.
- `app/ponto/views.py:2614`: confirmação de aprovação apesar do anexo.
- `app/ponto/views.py:2739`: cancelamento individual.
- `app/ponto/views.py:2808`: edição.
- `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`: apply e reversão.
- `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`: cura de fim.
- `app/chamados/services/materializacao.py:663`: devolução de perguntas absorvidas após rejeição; grava no pk da ausência, embora altere perguntas/disputas (`:657`–`:665`).

Todos esses chamadores usam `evento` direta ou indiretamente: `app/core/auditoria.py:22`, escritor físico em `app/core/observ.py:134`.

Os anexos originais de triagem/opus mencionados não foram disponibilizados no dado: [não verificado].

## 06/11 01:55 · sessao 674011c3259a · - · - · origem=Ronald (tmux)

**Alta — #2 permanece aberto quando a trilha falha.** `mudar_estado` persiste `status`, `minutos` e `data_fim` sem `atualizado_em`, depois tenta auditoria (`app/ponto/services/ausencia.py:807`, `:818`, `:819`, `:820`). `evento` admite falha e retorna `None`, preservando a alteração de negócio; `registrar_log` ignora o retorno (`app/core/observ.py:105`, `:133`, `:139`; `app/core/auditoria.py:22`). Assim, ida e volta de status com falhas nas duas trilhas pode deixar identidade/minutos/carimbo iguais e passar pela reversão (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`, `:154`, `:157`). Ocorrência em produção: [não verificado].

**Quem registra `modelo='Ausencia'`:**

- `app/ponto/services/ausencia.py:441`: criação.
- `app/ponto/services/ausencia.py:820`: mudança de estado.
- `app/ponto/services/ausencia.py:854`: documento anexado.
- `app/ponto/services/ausencia.py:915`: período parcial.
- `app/ponto/views.py:2357`: cancelamento em lote.
- `app/ponto/views.py:2614`: aprovação apesar do anexo.
- `app/ponto/views.py:2739`: cancelamento individual.
- `app/ponto/views.py:2808`: edição.
- `app/core/views_ia.py:991`: criação pela IA.
- `app/chamados/services/materializacao.py:663`: devolução das perguntas absorvidas.
- `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`: cura do fim.
- `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`: apply **e reversão**.
- Migrações históricas: `app/ponto/migrations/0057_a4_backfill_pendente.py:44`, `app/ponto/migrations/0058_a4_relogio_recomeca.py:43`, `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:37`.

Escritor físico atual: `app/core/observ.py:134`; adaptador: `app/core/auditoria.py:22`.

**Quem escreve Ausencia sem trilha própria:**

- `editar_ausencia_registro`: `app/ponto/services/ausencia.py:508`; save completo, com log pelos chamadores conhecidos `app/ponto/views.py:2808` e comando `:182`.
- `rederivar_fim`: `app/ponto/services/ausencia.py:1280`; log no chamador `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`.
- `rejeitar_ausencias_de_ferias`: `app/ponto/services/ausencia.py:1265`; sem log, mas move explicitamente `atualizado_em` em `:1267`.
- Exclusões sem trilha da Ausencia: `app/ponto/services/ausencia.py:1174`, `:1197`, `:1254`, `:1274`; a reversão recusa linha ausente (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:140`).
- Escritas de metadados sem carimbo/log próprio: `app/ponto/services/ausencia.py:536`, `:911` sem período e estado inalterado, `:1114`, `:1124`, `:1132`. Esses caminhos isolados não demonstram mudança de minutos/identidade/status: [não verificado].

Log com outro modelo encontrado: aviso de recusa em `Colaborador`, sem criação da ausência (`app/api/views.py:1937`); correção de férias em `AgendamentoFerias`, com exclusão/recriação do espelho (`app/ferias/views.py:497`; `app/ferias/services.py:385`, `:387`). Escritor invisível que altere a mesma ausência preservada nesses caminhos: [não verificado].

## 06/11 01:16 · sessao 9d598bca1a77 · C20-FIO-MUDO · verificador · origem=Code

**Não certificaria a obra completa.** O delta corrige captura do callback, cerca de competência e savepoint, mas continuam sem cobertura quatro atos de perda; há também uma janela concorrente na adoção. A linha permanece **ABERTA**. (`app/chamados/models.py:468`, `app/chamados/models.py:521`, `app/docs/PROMPTS.md:1882`, `app/docs/BACKLOG.md:1764`; janela detalhada no item 3.)

Execução dos testes, RED/GREEN, mutantes e efeitos atuais em produção: **[nao verificado]**. [nao verificado]

1. **Achados do opus final**

   | Achado | Avaliação do delta |
   |---|---|
   | **(1) Callback sem captura** | **Resolve.** O wrapper agora captura a exceção com `engolir`; este registra ERROR e incrementa o contador de engolidos. R20a exige execução do callback seguinte. (`app/chamados/models.py:521`, `app/core/observ.py:73`, `app/core/observ.py:86`, `app/chamados/tests/test_c20_fio_mudo.py:337`) |
   | **(2) Cerca contra L-050 e sem teste que morde** | **Resolve o achado específico.** A adoção deixou de consultar competência entregue; R20c cria exportação vigente e exige reapontamento. A prova não cobre o fluxo posterior inteiro. (`app/chamados/models.py:475`, `app/chamados/models.py:479`, `app/chamados/tests/test_c20_fio_mudo.py:385`, `app/chamados/tests/test_c20_fio_mudo.py:397`, `app/docs/LEIS.md:91`) |
   | **(3) Cinco atos/estoque incompletos** | **Não resolve.** Os pontos de entrada continuam sendo nascimento efetivo e reabertura; o dedup de chamado já vivo retorna antes da adoção. A obra segue nomeada ABERTA. (`app/chamados/models.py:965`, `app/chamados/models.py:983`, `app/chamados/models.py:1033`, `app/chamados/models.py:1502`, `app/docs/BACKLOG.md:1764`) |
   | **(4) M3 sobrevivente sem equivalência declarada** | **Não demonstrado como resolvido.** Definição do M3, nova execução e justificativa de equivalência: **[nao verificado]**. |
   | **(5) `savepoint=False` tornando emissão refém** | **Resolve a estrutura transacional apontada.** Agora existe savepoint próprio, com captura por fora; R20b alcança erro SQL e consulta depois dele. Falta provar rollback após escrita parcial. (`app/chamados/models.py:468`, `app/chamados/models.py:526`, `app/chamados/tests/test_c20_fio_mudo.py:362`) |

   Os achados **(3) e (4) continuam pendentes**; o primeiro está nomeado no BACKLOG, enquanto a resolução documental do M3 permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1764`.)

2. **Quem escreve e quem lê**

   | Campo/ato | Escritores | Leitores relevantes |
   |---|---|---|
   | `PerguntaDisputa.chamado_destino` | `fio.reapontar_resolvedora`, com julgamento e lavra no mesmo save; nascimento em `PerguntaDisputa.abrir`. **O ramo de identidade de `abrir` também reaponta pergunta existente diretamente.** (`app/chamados/services/fio.py:829`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:2550`, `app/chamados/models.py:2574`, `app/chamados/models.py:2576`) | Âncora, resolução da disputa do chamado, API e fila de validação. (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:131`, `app/api/views.py:1007`, `app/chamados/services/validacao.py:793`) |
   | `PerguntaDisputa.via_resolucao` | Núcleo `_escrever_carimbo`, portas de carimbar/identidade/descarimbar e limpeza em lote; atos próprios em `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. (`app/chamados/models.py:2656`, `app/chamados/models.py:2662`, `app/chamados/models.py:2725`, `app/chamados/models.py:2746`, `app/chamados/models.py:2763`, `app/chamados/models.py:2773`, `app/chamados/services/relavra_via.py:123`) | C6, exclusão de duplicatas na adoção e idempotência da retratação. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:481`, `app/chamados/services/fio.py:882`) |
   | Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `fechar` grava os quatro; `reabrir_admin` e `reabrir_sistema` limpam os quatro. `limpar_resolucao_residual` é porta estreita para somente resolução. (`app/chamados/models.py:1961`, `app/chamados/models.py:2163`, `app/chamados/models.py:2187`, `app/chamados/models.py:2270`) | Propriedade `aberta`, idempotência do fechamento, resolução do fio e API de disputas abertas. (`app/chamados/models.py:1864`, `app/chamados/models.py:1920`, `app/chamados/services/fio.py:131`, `app/api/views.py:1028`) |
   | `ChamadoColaborador.status_local` | Transições por `_gravar_estado`; nascimento pelo estado decidido pelo motor nas portas de criação. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/models.py:994`, `app/chamados/models.py:1030`, `app/chamados/models.py:1155`) | Adoção, guarda da costura, fila e painel. (`app/chamados/models.py:469`, `app/chamados/models.py:486`, `app/chamados/services/fio.py:764`, `app/chamados/services/fila.py:126`, `app/chamados/services/painel.py:211`) |
   | Evento `pergunta_reatada` | Gate de fechamento e nova adoção; gravação física por `core.observ.evento`. (`app/chamados/models.py:423`, `app/chamados/models.py:508`, `app/core/observ.py:94`) | Auditoria genérica consulta `LogAuditoria`; consumidor específico dessa ação: **[nao verificado]**. (`app/core/views.py:705`) |
   | Ocorrência `disputa_reaberta_sistema` | `reabrir_sistema`, pela porta de nota, **no chamado-pai da disputa**, quando existe. (`app/chamados/models.py:2192`, `app/chamados/services/nota.py:102`) | Followups do fio e histórico. (`app/chamados/services/fio.py:171`, `app/chamados/services/historico.py:80`) |
   | `FechamentoMensal` | Porta de fechamento: criação/cálculo, atualização, aprovação, reabertura e restauração. (`app/ponto/services/fechamento.py:889`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1307`, `app/ponto/services/fechamento.py:1342`, `app/ponto/services/fechamento.py:1433`) | Montagem do TXT lê fechamentos e classificação de entrada. (`app/folha/export.py:692`, `app/folha/export.py:695`, `app/folha/export.py:698`) |
   | `ExportacaoDominio` | `registrar` cria conteúdo/hash históricos; `invalidar` altera invalidadores preservando conteúdo/hash. (`app/folha/models.py:143`, `app/folha/models.py:174`, `app/folha/models.py:182`) | Competência entregue, vigência, histórico e download da cópia guardada. (`app/folha/export.py:569`, `app/folha/models.py:121`, `app/folha/views.py:554`) |

   **Existe segundo sítio de reapontamento fora de `reapontar_resolvedora`, preexistente ao delta.** Ele consulta os mesmos juízes, mas repete a escrita no ramo de identidade de `PerguntaDisputa.abrir`; o selo admite ambos os arquivos e não impede essa duplicação de sítio. (`app/chamados/models.py:2538`, `app/chamados/models.py:2550`, `app/chamados/models.py:2574`, `app/core/tests/test_contract_escritores_estado.py:61`.)

   **Existe estado intermediário observável:** pergunta já reatada, disputa ainda fechada. `_disputa_do_chamado` e a API que seleciona disputas abertas não a encontram nesse intervalo; o fio do admin admite vivas em disputas fechadas. Não cabe afirmar invisibilidade universal ao colaborador, porque `perguntas_do_app` consulta C6 sem excluir disputa fechada. (`app/chamados/models.py:524`, `app/chamados/services/fio.py:131`, `app/api/views.py:1028`, `app/chamados/services/fio.py:209`, `app/chamados/services/pergunta_no_app.py:46`.)

3. **Transação, exceções e travas**

   - **Em `abrir()`:** a transação externa trava Colaborador e cria o chamado antes da adoção. O `engolir` envolve o `atomic()` interno; assim, uma exceção propagada desfaz primeiro o savepoint e só depois é capturada. Para `DatabaseError` recuperável, com rollback bem-sucedido, a estrutura preserva a transação externa. Recuperação observada nesta cópia: **[nao verificado]**. (`app/chamados/models.py:961`, `app/chamados/models.py:964`, `app/chamados/models.py:1030`, `app/chamados/models.py:468`, `app/core/observ.py:86`.)

   - **O `savepoint=False` de `reapontar_resolvedora` continua existindo**, mas fica dentro do novo savepoint da adoção. Erro propagado desse núcleo alcança a proteção externa; isso preserva a emissão, sem garantir a costura. (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:468`, `app/chamados/models.py:503`, `app/chamados/models.py:526`.)

   - **Em `reabrir()` direto:** não existe atomic envolvendo o método inteiro. `_gravar_estado` tem atomic próprio, depois vêm ressurreição e adoção. Sem atomic do chamador, falha capturada da adoção não desfaz a reabertura anterior; falha na ressurreição pode ocorrer antes de alcançar a nova proteção. (`app/chamados/models.py:274`, `app/chamados/models.py:1492`, `app/chamados/models.py:1501`, `app/chamados/models.py:1604`, `app/chamados/models.py:1632`.)

   - **Callback levantando:** seu serviço tem atomic próprio, e a captura fica por fora. A falha não desfaz a costura já comprometida; pode deixar a disputa fechada, com ERROR e contador de sobra. O contador declara dono ADMIN e “NAO alarma”; ele não é reparador nem retentativa. (`app/chamados/services/fio.py:912`, `app/chamados/models.py:521`, `app/core/observ.py:79`, `app/chamados/services/vigia_fio.py:740`, `app/chamados/services/vigia_fio.py:748`.)

   - **Ordem explícita:** adoção por `abrir`: Colaborador → chamado adotante → escrita de Pergunta; adoção direta: chamado adotante → escrita de Pergunta. O callback trava Pergunta e Disputa, ordenadas por disputa/pk, e depois julga C6. A relavra existente também toma Pergunta+Disputa. (`app/chamados/models.py:964`, `app/chamados/models.py:469`, `app/chamados/services/fio.py:847`, `app/chamados/services/fio.py:913`, `app/chamados/services/relavra_via.py:221`.)

   - **Lacuna concorrente:** candidatas e status das âncoras antigas são lidos sem trava. Outra costura pode transferir a pergunta a outro chamado vivo e comprometer antes da escrita da adoção; esta usa `_anc` antigo e sobrescreve o destino sem reler a Pergunta. A trava do Colaborador não serializa a porta independente de reapontamento. É um intercalamento permitido pela estrutura; ocorrência em produção: **[nao verificado]**. (`app/chamados/models.py:479`, `app/chamados/models.py:485`, `app/chamados/models.py:491`, `app/chamados/models.py:503`, `app/chamados/services/fio.py:829`, `app/chamados/services/fio.py:845`.)

   - **Não há prova de ausência de deadlock.** O callback não trava explicitamente Chamado/Colaborador, mas insere nota com FK no chamado-pai; locks implícitos e adiamento das constraints efetivas: **[nao verificado]**. Também não verifica que o chamado continua vivo: verifica FK de destino e C6. (`app/chamados/models.py:2194`, `app/chamados/services/nota.py:103`, `app/chamados/services/fio.py:915`, `app/chamados/services/fio.py:917`.)

4. **Retirada da cerca de competência**

   **L-050 governa chamado/pergunta:** a fronteira do exportado foi retirada por desenho. Para gravado, recalculo e TXT, é necessário ler também L-113, que altera L-092/L-052 e preserva TXT anterior, trilha e DIFF antes de ato de frota. CORTES reafirma dia exportado editável. (`app/docs/LEIS.md:91`, `app/docs/LEIS.md:150`, `app/docs/CORTES.md:219`.)

   A adoção escreve pergunta/lavra, identidade das irmãs, auditoria e agenda reabertura; não aparece nela escrita direta de fechamento, exportação ou célula. **Isso não prova imutabilidade do fluxo posterior:** validação chama recalculo do fechamento pela data do fato, e o fechamento tem escritor dos valores gravados. (`app/chamados/models.py:503`, `app/chamados/models.py:516`, `app/chamados/models.py:524`, `app/chamados/services/validacao.py:404`, `app/chamados/services/validacao.py:407`, `app/ponto/services/fechamento.py:1144`.)

   A célula conserva sua autoridade separada, `competencia_fechada_para_celula`, composta de TXT vigente e holerite publicado; retirar a cerca da adoção não remove essa guarda. (`app/folha/export.py:578`, `app/folha/export.py:599`, `app/ponto/management/commands/gerar_celulas.py:146`.)

   Há uma borda adicional sem prova: o callback reabre **o continente inteiro**, embora tenha selecionado uma pergunta reatada. CORTES distingue a apresentação de vivas em disputa fechada, “sem reabrir, sem push”; o efeito sobre outras perguntas de uma disputa mista não é exercitado pelo R20c. Conformidade desse cenário completo: **[nao verificado]**. (`app/chamados/services/fio.py:920`, `app/api/views.py:1043`, `app/docs/CORTES.md:327`, `app/chamados/tests/test_c20_fio_mudo.py:390`.)

5. **Os testes mordem?** [nao verificado]

   - **R20a: sim, captura e continuidade.** Mock no endereço correto, callback executado enquanto o patch está ativo, ERROR e callback seguinte exigidos. Não prova commit real nem liberação de locks, pois usa `TestCase` e captura artificial de callbacks. (`app/chamados/tests/test_c20_fio_mudo.py:32`, `app/chamados/tests/test_c20_fio_mudo.py:337`, `app/chamados/tests/test_c20_fio_mudo.py:346`.)
   - **R20b: sim, erro SQL e transação utilizável nesse cenário; parcialmente quanto ao rollback prometido.** Quebra na eleição, antes da primeira costura. “Destino ficou igual” passaria sem qualquer escrita anterior; falta escrever primeiro e falhar depois. (`app/chamados/tests/test_c20_fio_mudo.py:362`, `app/chamados/tests/test_c20_fio_mudo.py:372`, `app/chamados/models.py:500`, `app/chamados/models.py:503`.)
   - **R20c: sim, antiga cerca.** Exportação vigente impede vacuidade do cenário. Compara todos os campos do fechamento, mas só hash/conteúdo da exportação; não executa callback, não fotografa célula nem percorre resposta/materialização posterior. (`app/chamados/tests/test_c20_fio_mudo.py:385`, `app/chamados/tests/test_c20_fio_mudo.py:398`, `app/chamados/tests/test_c20_fio_mudo.py:401`, `app/folha/export.py:569`.)
   - **R08j:** prova adiamento, respeito ao resultado C6 e idempotência; continuaria passando se C6 fosse avaliado antes da trava. Não demonstra intercalamento concorrente. (`app/chamados/tests/test_c20_fio_mudo.py:246`, `app/chamados/tests/test_c20_fio_mudo.py:254`, `app/chamados/tests/test_c20_fio_mudo.py:262`.)
   - **R08g e R19b:** alcançam os ramos, exigindo chamada efetiva da porta e pergunta viva sob outro chamado após o nascimento inicial. (`app/chamados/tests/test_c20_fio_mudo.py:273`, `app/chamados/tests/test_c20_fio_mudo.py:425`, `app/chamados/tests/test_c20_fio_mudo.py:433`.)
   - **R08i/R19c:** idempotência verifica retorno/quantidade, sem foto integral; o teste de futuro roda o vigia hoje e olha perguntas amanhã, sem tentar abertura futura. (`app/chamados/tests/test_c20_fio_mudo.py:305`, `app/chamados/tests/test_c20_fio_mudo.py:441`.)

6. **Cinco atos e estoque**

   | Ato | Cobertura final pelo evento |
   |---|---|
   | **Reabrir** | **Coberto pelo novo ponto de chamada**, depois da ressurreição; sujeito às falhas e concorrência acima. (`app/chamados/models.py:1501`, `app/chamados/models.py:1502`) |
   | **Retratar duplicata** | **Sem restauração de cobertura na origem que perdeu a pergunta.** Marca identidade e retorna. (`app/chamados/services/fio.py:878`, `app/chamados/services/fio.py:891`) |
   | **Validação** | **Sem evento C20 de recomposição.** C6 mata pela validação; o fluxo existente materializa/recalcula, sem chamar adoção. (`app/chamados/juizes.py:1135`, `app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:430`) |
   | **Morte por leitura** | **Juiz existente nomeado: C6, `pergunta_viva`/`_pergunta_viva_calc`.** Não há evento novo de recomposição nessa leitura. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:1135`) |
   | **Reapontar** | **Sem recomposição da âncora anterior.** A porta salva destino/lavra e retorna. (`app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:849`) |

   **O evento não percorre automaticamente o estoque já vivo:** o dedup retorna antes da adoção. Uma nova abertura pode encontrar perguntas antigas; uma nova reabertura pode adotar órfãs, mas o número efetivamente alcançado dos 494 é **[nao verificado]**. (`app/chamados/models.py:965`, `app/chamados/models.py:983`, `app/chamados/models.py:479`, `app/chamados/models.py:1502`.)

   O contrato declara **494**; o cabeçalho dos testes relata outro retrato de **495**, incluindo 3 excluídos e 34 perguntas com destino morto/sem destino. Esses números não demonstram 34 curas nem reconciliam o universo contratual; conciliação quantitativa: **[nao verificado]**. (`app/docs/PROMPTS.md:1875`, `app/chamados/tests/test_c20_fio_mudo.py:13`.)

**Falta para certificar:** cobrir os quatro atos restantes e demonstrar alcance do estoque; esclarecer M3; fechar a janela de sobrescrita concorrente; provar rollback após escrita parcial e o caso de disputa fechada/mista em competência entregue. Esses pontos não estão provados pelos atuais pontos de chamada e testes. (`app/docs/PROMPTS.md:1882`, `app/chamados/models.py:479`, `app/chamados/models.py:1033`, `app/chamados/models.py:1502`, `app/chamados/tests/test_c20_fio_mudo.py:362`, `app/chamados/tests/test_c20_fio_mudo.py:374`; M3: **[nao verificado]**.)

## 06/11 01:14 · sessao 6def20f82445 · - · - · origem=Ronald (tmux)

- **Adoção levantando dentro de `abrir()`: corrigido quanto ao savepoint.** `abrir()` abre transação e trava o Colaborador antes do dedup (`app/chamados/models.py:961-965`); cria o chamado antes de adotar (`app/chamados/models.py:1030-1033`). A adoção entra em `engolir` **antes** de entrar em `atomic()`, portanto a saída do `atomic` desfaz o savepoint antes de a exceção ser capturada; retorna `[]` depois do ERROR (`app/chamados/models.py:467-468`, `app/chamados/models.py:525-526`; captura e contador em `app/core/observ.py:73-91`). Para erro de banco recuperável com rollback bem sucedido, essa forma preserva a transação externa; funcionamento observado nesta cópia não foi executado [nao verificado].

- **`atomic(savepoint=False)` interno da porta não elimina a proteção nova.** `reapontar_resolvedora` ainda usa `atomic(savepoint=False)` e salva destino/lavra (`app/chamados/services/fio.py:837-848`), mas agora está dentro do savepoint da adoção (`app/chamados/models.py:468`, `app/chamados/models.py:503`). Erro que propague desse núcleo chega ao savepoint e é capturado somente depois de sua saída (`app/chamados/models.py:467-468`). A emissão pode prosseguir sem a costura; não significa fio garantido (`app/chamados/models.py:526`, `app/chamados/models.py:1033-1041`).

- **`reabrir()` direto tem outra unidade transacional.** Não há transação envolvendo o método inteiro: chama `_gravar_estado`, depois `ressuscitar_perguntas`, depois adota (`app/chamados/models.py:1492-1503`). `_gravar_estado` tem seu próprio atomic (`app/chamados/models.py:274-295`); portanto, sem transação externa do chamador, a falha capturada da adoção não desfaz a reabertura já gravada (`app/chamados/models.py:468`, `app/chamados/models.py:1492-1503`). A proteção nova também não cobre falha anterior na ressurreição: ela descarimba perguntas e chama `reabrir_sistema` diretamente (`app/chamados/models.py:1604`, `app/chamados/models.py:1631-1632`).

- **Callback levantando: corrigido quanto à propagação.** O wrapper registra captura própria e chama o serviço dentro dela (`app/chamados/models.py:521-524`); o serviço usa atomic próprio (`app/chamados/services/fio.py:912-922`), portanto uma exceção propagada do serviço passa pela saída do atomic antes do `engolir`. O callback falho não precisa cortar callbacks seguintes; R20a injeta no endereço efetivamente importado, observa ERROR, costura anterior e callback posterior (`app/chamados/tests/test_c20_fio_mudo.py:337-350`). Commit real, recuperação de conexão e rollback fatal não foram exercitados [nao verificado].

- **Ordem explícita da adoção:** `Colaborador → chamado adotante → UPDATE Pergunta`, quando chamada por `abrir`; `chamado adotante → UPDATE Pergunta`, quando chamada diretamente sem outra transação/trava (`app/chamados/models.py:964`, `app/chamados/models.py:469`, `app/chamados/models.py:503`; escrita em `app/chamados/services/fio.py:845-848`). A adoção não executa `select_for_update` das candidatas nem das âncoras anteriores: coleta perguntas e status em consultas comuns (`app/chamados/models.py:479-493`). A trava na porta da costura é da linha **do destino novo**, sem reler a pergunta sob trava (`app/chamados/services/fio.py:758-765`, `app/chamados/services/fio.py:829-848`).

- **Lacuna concorrente concreta por inspeção:** entre a coleta e a escrita, outro escritor pode reapontar a mesma pergunta para um chamado vivo; a adoção continua usando `_anc` e `_grupo` antigos, e `reapontar_resolvedora` escreve por cima sem conferir o destino corrente da pergunta (`app/chamados/models.py:485-503`; `app/chamados/services/fio.py:829-848`). A trava do Colaborador serializa quem passa por `abrir()`, mas a porta de reapontamento não toma essa trava (`app/chamados/models.py:964`; `app/chamados/services/fio.py:836-848`). Execução concorrente e ocorrência no dado de produção: [nao verificado].

- **Ordem explícita do callback:** trava Pergunta e Disputa na consulta, ordenada por disputa/pk; só depois julga C6 e reabre (`app/chamados/services/fio.py:913-920`). `reabrir_sistema` limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`, salva e insere nota no chamado **pai da disputa** (`app/chamados/models.py:2187-2194`). Não chama `ChamadoColaborador.reabrir`; os receivers de fechamento da disputa saem quando `fechada_em` é nulo (`app/chamados/signals.py:28`, `app/chamados/signals.py:106`, `app/chamados/signals.py:130`).

- **Não certificaria “sem qualquer trava de Chamado” só pela docstring.** A nota grava `FollowupChamado` com FK no chamado pai (`app/chamados/models.py:2192-2194`; `app/chamados/services/nota.py:87-103`). Um ciclo envolvendo a trava implícita dessa FK e `Chamado → Pergunta` depende das constraints efetivas, adiamento e modos de trava; deadlock desse cenário não foi demonstrado [nao verificado]. A cadeia proposta `reabrir_sistema → chamado.reabrir → Colaborador` não existe nesse código (`app/chamados/models.py:2177-2195`).

- **Callback não garante destino ainda vivo.** Sua consulta exige o mesmo `chamado_destino_id` e disputa fechada; o predicado seguinte é C6, sem filtro do status do chamado (`app/chamados/services/fio.py:915-920`). Assim, o que ele protege explicitamente é a pergunta ainda apontar para aquele pk e continuar viva, não o chamado ainda estar vivo (`app/chamados/services/fio.py:915-920`). A disputa fechada pode ser observada entre commit e callback e continua assim se ele falhar; o próprio R20a exige essa sobra no contador (`app/chamados/tests/test_c20_fio_mudo.py:345-350`).

- **Limite do R20b:** o erro SQL é real, mas injetado em `canonica_do_grupo`, antes de qualquer costura/retratação; a asserção “destino voltou” também passa se nenhuma escrita chegou a ocorrer (`app/chamados/tests/test_c20_fio_mudo.py:362-372`; eleição antes da escrita em `app/chamados/models.py:500-516`). Ele morde transação envenenada e falha de emissão, mas não comprova rollback de adoção parcialmente escrita (`app/chamados/tests/test_c20_fio_mudo.py:368-372`).

## 06/11 01:14 · sessao 541a416d38e7 · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e pontos relevantes, por inspeção estática:

| Campo/ato | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | `fio.reapontar_resolvedora`, com E4-c, guarda e lavra no mesmo save (`app/chamados/services/fio.py:829-848`). **Há outro sítio preexistente:** `PerguntaDisputa.abrir` reaponta a existente diretamente, com os mesmos juízes e lavra, sem chamar aquela porta (`app/chamados/models.py:2538-2574`). Nascimento grava a FK em `models.py:2576-2577`. | Âncora/listagem: `fio.chamado_que_lista` (`fio.py:97-117`); resolução de disputa aberta (`fio.py:131-137`); lista do app (`app/api/views.py:1007-1010`); validação (`app/chamados/services/validacao.py:793-795`). |
| `PerguntaDisputa.via_resolucao` | Núcleo do carimbo `_escrever_carimbo` (`models.py:2656-2660`), usado pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` (`models.py:2662`, `2725`, `2746`); limpeza em lote (`models.py:2763-2764`). Exceções/atos próprios: `encerrar_por_fato_no_espelho` (`models.py:2773-2774`) e `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:123-126`). | C6 lê validação, tranca, resposta e depois vias de identidade (`app/chamados/juizes.py:1135-1172`); adoção exclui duplicata previamente carimbada (`models.py:481`); retratação testa idempotência (`fio.py:882-883`). |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `DisputaSupervisao.fechar` escreve os quatro juntos (`models.py:1961-1965`); `reabrir_admin` limpa quatro e incrementa seu contador (`models.py:2163-2175`); `reabrir_sistema` limpa quatro sem esse contador (`models.py:2187-2191`). Porta estreita `limpar_resolucao_residual` apaga só resolução de disputa aberta (`models.py:2270-2274`). | `aberta` lê `fechada_em` (`models.py:1864-1865`); fechamento lê estado/autor/resolução/motivo para idempotência (`models.py:1920-1924`); resolução de disputa do fio exige aberta (`fio.py:131-137`); API indexa abertas (`app/api/views.py:1028-1031`); contador lê `motivo_fechamento` (`app/chamados/services/vigia_fio.py:734`). |
| `ChamadoColaborador.status_local` | Transições passam por `_gravar_estado`, atribuição em `models.py:222`, save em `models.py:279`; nascimento calcula estado por motor e insere (`models.py:994-1030`); outra porta de nascimento insere em `models.py:1155`. | Adoção verifica status travado do próprio chamado (`models.py:466-469`) e status das outras âncoras (`models.py:486-492`); guarda da costura lê destino fresco (`fio.py:758-765`); fila e painel filtram estado (`app/chamados/services/fila.py:126`, `app/chamados/services/painel.py:211-222`). |
| Evento `pergunta_reatada` | Gate de fechamento já emitia (`models.py:423-427`); adoção emite (`models.py:508-512`); escrita física de auditoria passa por `core.observ.evento` (`app/core/observ.py:94-138`). | Auditoria genérica consulta e filtra `LogAuditoria` por ação/modelo (`app/core/views.py:705-714`). Consumidor específico dessa ação não localizado; uso adicional [nao verificado]. |
| Ocorrência `disputa_reaberta_sistema` | `reabrir_sistema` escreve **no chamado-pai da disputa**, quando existe (`models.py:2192-2194`), pela porta `nota.anexar_ocorrencia`; inserção física em `app/chamados/services/nota.py:102-109`. | Fio prefetch de followups (`fio.py:171-176`) e histórico (`app/chamados/services/historico.py:80-85`). |

**Segundo escritor relevante:** a reivindicação de exclusividade de `reapontar_resolvedora` (`fio.py:787-796`) não corresponde ao ramo de identidade de `PerguntaDisputa.abrir` (`models.py:2538-2574`). Não é introduzido pelo delta; o selo por arquivo admite `models.py` e `fio.py`, portanto não impede essa duplicação de sítio (`app/core/tests/test_contract_escritores_estado.py:61`).

**Leitores podem observar meio-estado entre commit e callback, ou indefinidamente se o callback falhar:** pergunta reatada a chamado vivo, C6 viva, disputa ainda fechada. O callback é agendado depois da costura (`models.py:508-526`); `_disputa_do_chamado` exclui a disputa fechada (`fio.py:131-137`). O fio do admin admite perguntas vivas em disputas fechadas, enquanto o fio do colaborador usa abertas (`fio.py:209-221`). Porém **não afirmar inacessibilidade universal do colaborador**: `perguntas_do_app` não filtra disputa fechada, só universo/C6 (`app/chamados/services/pergunta_no_app.py:42-53`). A sobra é contada por `viva_em_disputa_fechada`; sua documentação declara dono ADMIN e “NAO alarma” (`app/chamados/services/vigia_fio.py:737-759`).

**Cinco atos:** contrato literal em `app/docs/PROMPTS.md:1875`; enumeração e exigência de nomear o juiz da morte por leitura em `app/docs/PROMPTS.md:1882`; linha da obra segue `ABERTA` (`app/docs/BACKLOG.md:1764`).

- `reabrir`: agora chama adoção depois de ressuscitar suas próprias perguntas (`models.py:1492-1502`).
- `retratar_duplicata`: grava identidade, não chama adoção nem restaura cobertura do chamado que perdeu pergunta (`fio.py:878-895`). Duplicata respondida pode permanecer viva no C6 porque resposta é julgada antes da identidade (`juizes.py:1150-1172`); a adoção deliberadamente a exclui (`models.py:481`).
- `validacao`: carimba e materializa, em transação própria; não chama adoção (`app/chamados/services/validacao.py:395-397`). O carimbo de validação torna C6 falso no primeiro degrau (`juizes.py:1135-1137`).
- `morte por leitura`: autoridade existente é C6, `pergunta_viva` → `_pergunta_viva_calc` (`juizes.py:835-848`), com validação/tranca/identidade/marco/célula/janela (`juizes.py:1135-1208`). A leitura não chama adoção nesse fluxo (`juizes.py:835-848`).
- `reapontar`: porta grava destino/lavra, não chama adoção para a âncora anterior (`fio.py:837-849`).

O evento novo roda apenas no nascimento real (`models.py:1031-1033`) e reabertura (`models.py:1501-1502`); existente vivo no dedup retorna antes da adoção (`models.py:965-983`). Logo, não há varredura/cura automática do estoque já vivo por essa mudança; quantidade efetivamente alcançada em produção [nao verificado].

## 06/11 01:14 · sessao 51d4d44fe872 · H12H06-B · verificador · origem=Code

1. **Alta — a confirmação pode confirmar o ato de outro. Cura (1) incompleta.** A transação devolve seu carimbo, mas o laço o substitui por uma leitura **depois de liberar a trava** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`).

   Contraexemplo: apply grava atestado `720→0`; antes dessa leitura, outra edição salva descrição e reafirma `minutos=0`, preservando identidade. Esse escritor faz `save()` completo, movendo o carimbo (`app/ponto/services/ausencia.py:496`, `app/ponto/services/ausencia.py:501`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:814`). A confirmação captura o carimbo dessa edição; a reversão aceita identidade, minutos e carimbo e tenta devolver `720`, desfazendo a reafirmação alheia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:141`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:163`).

2. **Alta — há novos atos que não movem a testemunha. Cura (1) incompleta também sem corrida.** `mudar_estado` salva status e minutos sem incluir `atualizado_em`; o modelo não acrescenta esse campo ao `update_fields` (`app/ponto/services/ausencia.py:807`, `app/ponto/services/ausencia.py:818`, `app/ponto/models.py:908`).

   Contraexemplo: após apply de atestado aprovado `720→0`, rejeitar e aprovar novamente são transições permitidas. Ao terminar, identidade, minutos e carimbo podem ser iguais aos confirmados; a reversão atravessa as três verificações apesar das novas decisões (`app/ponto/services/ausencia.py:754`, `app/ponto/services/ausencia.py:759`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:141`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:148`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`).

   Nos dois contraexemplos, restaurar minutos positivos depende do cadastro aceitar esse tipo por minutos; os testes contemplam restauração do atestado e recusa da falta. Cadastro efetivo e ocorrência em produção: **[nao verificado]** (`app/ponto/services/ausencia.py:480`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:286`).

3. **Média — commit pode ficar sem confirmação e sem linha no resultado. Cura (4) parcial.** Depois do commit, a consulta ou a escrita/`flush`/`fsync` da confirmação podem falhar; os contadores e `saida` só são atualizados depois dessas operações (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:238`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:256`).

   Nesse intervalo, a ausência já mudou, mas o resultado parcial pode omiti-la e a reversão excluir sua foto sem confirmação. `completo=False` informa interrupção, porém não identifica essa escrita efetivada (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:225`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:257`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270`). Falha efetiva em produção: **[nao verificado]**.

Os demais contraexemplos pedidos:

- **PK repetido — fragilidade condicional, gravidade média.** A última confirmação por PK vence, e todas as fotos desse PK recebem seu carimbo; falta vínculo entre tentativa e confirmação. Uma foto anterior malsucedida pode ser habilitada pela confirmação posterior (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:219`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:221`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:225`). A LISTA desta cópia não repete PK; arquivo concatenado ou com tentativas distintas repetidas: **[nao verificado]** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50`).
- **Reversão de reversão — sem defeito isolado demonstrado.** O leitor aceita esse snapshot; a foto troca novamente origem e destino, permitindo `720→0→720→0`, sujeita às mesmas guardas (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:221`).
- **`on_commit` regravando a própria ausência — [nao verificado].** O callback rastreado chama devolução de perguntas, reconciliação e fechamento de chamados. A confirmação posterior lê somente o carimbo, sem repetir a conferência do estado (`app/ponto/signals.py:254`, `app/ponto/signals.py:269`, `app/ponto/signals.py:271`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`).
- **Fuso — sem falha normal demonstrada.** Ambos os lados usam `isoformat()` de leituras do banco, com `USE_TZ=True` na cópia; configuração da conexão de produção: **[nao verificado]** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:255`, `app/config/settings/base.py:95`).
- **Cura (5) — resolve colisão apenas por igualdade do segundo; não garante unicidade.** Mesmo microssegundo ou repetição do relógio ainda pode colidir, mas `open('x')` interrompe antes do laço, sem sobrescrever o snapshot (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:209`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:234`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:247`).

**Quem escreve `Ausencia.atualizado_em`:**

| Caminho | Referência |
|---|---|
| `auto_now` em criação, edição completa e rederivação do fim | `app/ponto/models.py:814`; `app/ponto/services/ausencia.py:418`; `app/ponto/services/ausencia.py:508`; `app/ponto/services/ausencia.py:1280` |
| Declaração/edição de período parcial inclui expressamente o campo | `app/ponto/services/ausencia.py:878`; `app/ponto/services/ausencia.py:911` |
| Rejeição em lote de férias escreve `timezone.now()` explicitamente; tipo fora desta LISTA | `app/ponto/services/ausencia.py:1265`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50` |

**Quem altera a ausência sem mover esse carimbo:** mudança de estado, anexo, prazo, decisão sem período e campos da reabertura (`app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:843`, `app/ponto/services/ausencia.py:536`, `app/ponto/services/ausencia.py:896`, `app/ponto/services/ausencia.py:1114`, `app/ponto/services/ausencia.py:1124`, `app/ponto/services/ausencia.py:1132`).

**Quem lê:** snapshot, comparação antes/depois, guarda da reversão, retorno da transação e confirmação posterior (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:123`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:95`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`). Há outro leitor funcional: `cartorio.houve_fato_novo`, além da exibição no admin (`app/ponto/services/cartorio.py:355`, `app/ponto/admin.py:47`).

**Sobre os descartes:**

- **(2), exportação:** permanece risco potencial **alto**, fora do escopo conforme o aval informado. A cerca antecede a escrita, enquanto o exportador calcula e só depois registra a entrega; não há serialização entre esses atos nos trechos examinados (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/folha/views.py:208`, `app/folha/views.py:297`). Ocorrência: **[nao verificado]**.
- **(3), push:** contesto tratá-lo como tecnicamente resolvido por sair `pulada`. O `save()` dispara cartório síncrono, que pode enviar push antes das verificações que provocam rollback; o resultado não recolhe esse efeito externo. **Gravidade média**, mantendo o aceite residual informado (`app/ponto/services/ausencia.py:508`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:778`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`). Emissão concreta nessas 30 linhas: **[nao verificado]**.

_2080 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
