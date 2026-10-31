# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 03:01** · publicado em **31/10/2026 03:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 03:01 · sessao 0b50d4b4a7dc · O261-E-F1-CLASSE-4 · sonda · origem=Code

**(1) tipo_dia_por_trabalha**

| trabalha | ata.tipo_dia | Contagem | Referência |
|---|---|---:|---|
| True | 'trabalho' | 46093 | [nao verificado] |
| False | 'folga' | 22128 | [nao verificado] |
| True | None | 10409 | [nao verificado] |
| False | None | 5382 | [nao verificado] |
| True | 'ausencia' | 2904 | [nao verificado] |
| False | 'ausencia' | 1609 | [nao verificado] |
| True | 'falta' | 375 | [nao verificado] |
| True | 'folga' | 1885 | [nao verificado] |
| None | 'trabalho' | 52007 | [nao verificado] |
| False | 'trabalho' | 154 | [nao verificado] |
| None | 'folga' | 222 | [nao verificado] |
| None | 'ausencia' | 78 | [nao verificado] |
| None | None | 66 | [nao verificado] |

**(2) universo_chamados_vivos_com_celula: 2964** [nao verificado]

**(3) divergem**

| Chave (trabalha, tipo_dia, em CALADOS_POR_ISENCAO, modulo) | Contagem | Chamado de exemplo | Referência |
|---|---:|---:|---|
| (None, 'trabalho', False, 'ancora_limbo') | 1 | 12982 | [nao verificado] |
| (True, 'ausencia', False, 'batida_fora_escala_leve') | 1 | 17793 | [nao verificado] |
| (True, 'ausencia', False, 'colaborador_em_ferias') | 1 | 28767 | [nao verificado] |
| (True, 'ausencia', False, 'desligamento_revisao') | 10 | 35918 | [nao verificado] |
| (True, 'ausencia', False, 'disputa_supervisao') | 3 | 20782 | [nao verificado] |
| (True, 'ausencia', False, 'sem_previsao_escala') | 5 | 35217 | [nao verificado] |
| (True, 'falta', False, 'desligamento_revisao') | 4 | 35915 | [nao verificado] |
| (True, 'falta', False, 'disputa_supervisao') | 1 | 15402 | [nao verificado] |
| (True, 'folga', False, 'desligamento_revisao') | 1 | 16004 | [nao verificado] |
| (True, 'folga', False, 'sem_previsao_escala') | 1 | 32564 | [nao verificado] |
| (True, 'folga', True, 'batida_ausente') | 10 | 34867 | [nao verificado] |

## 31/10 02:37 · sessao 6a09fec44f3d · O261-E-F5C · verificador · origem=Code

**FINAL: BLOQUEIA.** O diff mantém divergência entre painel e juiz, promete uma ação que o fio não garante e acrescenta um teste que falha antes das asserções pretendidas (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`, `app/folha/fora_da_folha.py:43`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).

1. **Escritores e leitores**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `porta['fila']` | Tupla ordenada produzida pelo juiz em `app/chamados/juizes.py:1226`, devolvida em `app/chamados/juizes.py:1227`. A entrada anterior, `fila_cel`, vem de `alimentar_portas`, em `app/chamados/juizes.py:1148`. | `o_que_segura`, em `app/folha/fora_da_folha.py:196`; teste direto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; comparações do dict inteiro em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164` e `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`. |
| `FORA_DA_LISTA_PADRAO` | Declaração em `app/chamados/services/verbos.py:18`. | Painel em `app/chamados/services/painel.py:327`; juiz em `app/chamados/juizes.py:1226`. |
| `GRUPOS['C']` | Texto em `app/folha/fora_da_folha.py:36`. | Tooltip via `retrato_folha.ajuda`, em `app/folha/retrato_folha.py:70`; enumeração das chaves em `app/folha/fora_da_folha.py:139`; testes em `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276` e `app/folha/tests/test_retrato_folha_quadros.py:1285`. |
| `DESTRAVA['chamado']` | Texto em `app/folha/fora_da_folha.py:43`. | Ferramenta em `app/folha/fora_da_folha.py:197`; drawer em `app/folha/retrato_folha.py:1069`; template exibe `r.valor` em `app/templates/folha/_retrato_o_que_fazer.html:37`. Testes em `app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119` e `app/folha/tests/test_retrato_folha_drawer.py:226`. |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista em `app/folha/fora_da_folha.py:196`, incluída no retorno em `app/folha/fora_da_folha.py:199`. | Asserção do teste novo em `app/folha/tests/test_haiku_fora_da_folha.py:138`. Consumidor externo em produção: **[nao verificado]**. |

O vigia lê `verbos` e `rota`, **não a nova `fila`**. Assim, uma porta com `fila=()` e verbo `historico` continua sendo contada como porta alcançável (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`).

As pautas dos discordantes leem indiretamente o juiz por `_fio_do_dia`, comparando apenas com `FIO_SEM_PORTA`; não consomem os números de `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`, `app/ponto/services/pautas_dos_discordantes.py:76`).

Os dois asserts diretos do dict inteiro foram atualizados. A comparação lote versus individual compara duas respostas atuais; o mock antigo do vigia continua sem `fila`, mas seu consumidor acessa somente `rota/verbos` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`, `app/chamados/tests/test_o261e_f5_alcance.py:307`, `app/chamados/services/vigia_fio.py:598`).

2. **Painel padrão versus `fila`: não são equivalentes — bloqueia**

O filtro final por verbo usa a mesma constante, mas o universo anterior difere: o juiz começa por `qs_na_fila`, que elimina qualquer silêncio futuro; a lista padrão elimina os verbos `arquivado/historico` depois da partição (`app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`).

Contraexemplo de código: chamado **da célula**, status `aberto`, módulo `gps_ausente`, silêncio futuro:

- O módulo é auditoria e produz verbo `None`; o silêncio só substitui verbos diferentes de `None` (`app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`).
- O painel padrão mostra: `aberto` pertence a `COBRANCA`, e `None` não pertence à constante de exclusão (`app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/services/verbos.py:18`).
- O juiz exclui antes da partição, pelo silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`).

Para os demais resultados possíveis: `validar/cobrar/decidir` sem silêncio entram nos dois; `arquivado` sai nos dois; `historico` sai da lista e da nova `fila`, embora permaneça em `verbos`; `None` sem silêncio futuro entra nos dois, quando o status é de cobrança (`app/chamados/catalogo/natureza.py:57`, `app/chamados/services/verbos.py:48`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1226`).

No mesmo estado e instante, sem filtros extras/paginação e comparando chamados da célula, `fila` é subconjunto da lista padrão; não há caso inverso nessa composição de filtros. A exclusão de âncoras de outra célula é adicional e intencional (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Ocorrência desses contraexemplos em produção: **[nao verificado]**.

3. **Universalidade das frases**

**`GRUPOS['C']`: compatível com a escada, sem bloqueio isolado.** Nas duas rotas, `chamado` significa que os verbos da porta não contêm `cobrar` nem `validar`. Isso não garante visibilidade na lista padrão (`app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:36`).

**`DESTRAVA['chamado']`: não universal — bloqueia.** A escada também devolve `chamado` para `None` e `historico`; portanto não garante a ação `decidir` (`app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:51`, `app/folha/fora_da_folha.py:43`).

Contraexemplo: chamado aberto da célula, silêncio vencido, nenhuma pergunta. Ele passa por `qs_na_fila`, vira `historico`, produz `porta_fila`, fio `chamado` e `fila=()`. A frase manda abrir um chamado da fila ou uma pergunta no app, embora nesse caso não haja nenhum desses alvos visíveis nomeados (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`, `app/folha/fora_da_folha.py:43`).

Na `porta_pergunta`, a existência da pergunta também não obriga o verbo a ser `decidir`: o juiz aceita o verbo da âncora, e a natureza de auditoria pode produzir `None` mesmo com pergunta (`app/chamados/juizes.py:1221`, `app/chamados/juizes.py:1222`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`).

4. **Drawer: igualdade da frase, incoerência da ação — bloqueia**

Para o mesmo fio, ferramenta e drawer usam literalmente o mesmo `DESTRAVA`; essa parte está coerente (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`).

Porém o botão de `chamado` continua **“Validar no fio”**, enquanto a nova frase manda **decidir** e afirma ausência de validar. O botão chega ao template com esse rótulo (`app/folha/retrato_folha.py:977`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:62`, `app/folha/fora_da_folha.py:43`).

O drawer também não recebe o novo dado numérico: o retrato grava fio, datas e outros eixos, e a view lê essa linha gravada; não chama `o_que_segura` nesse fluxo (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`, `app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Paridade temporal entre retrato e ferramenta em produção: **[nao verificado]**.

5. **Checklist e mordida dos testes** [nao verificado]

| Item | Veredito |
|---|---|
| Segundo juiz registrado | Não identificado no diff: a porta já está registrada, a escada permanece nela e a folha lê seu resultado (`app/core/juizes.py:253`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:68`). |
| Segundo escritor fora da porta | Não identificado nas alterações produtivas: os novos valores são retornos de leitura, sem nova persistência (`app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:199`). |
| Relógio fora da borda | Nenhuma leitura nova no diff; a cadeia existente lê instantes internos separados em `qs_na_fila` e `particionar_verbos`. Não sustenta promessa de fotografia temporal única (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:35`, `app/chamados/juizes.py:1153`). |
| Derivação paralela | A constante é compartilhada, mas isso não unifica os filtros anteriores; permanece a divergência demonstrada para `None` com silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`). **Bloqueia a equivalência.** |
| Ordem de trava | Ordem efetivamente executada nesta obra: **[nao verificado]**. |
| Pouso antes do pré-requisito | Sequência real de suite/verificação/pouso: **[nao verificado]**. A regra exige suite inteira verde antes do opus final (`CLAUDE.md:600`). | [nao verificado]

O teste **`test_MORDE_a_frase...` morde a redação anterior fornecida no diff**: suas substrings proibidas rejeitam “sem pergunta viva” e “chamado N”. Contudo, os dois mocks usam somente `decidir`; não exercitam a porta real, `None`, `historico` ou o botão do drawer. Portanto não demonstram a universalidade da nova frase (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`).

O teste **`test_MORDE_silenciado...` não oferece RED útil como escrito — bloqueia**: importa `Chamado`, inexistente no módulo; o modelo e o helper usam `ChamadoColaborador`. Isso causa `ImportError` antes dos subtestes, também no pai descrito pelo diff (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`).

Mesmo corrigindo esse import, **2099 continua quebrando na versão curada**: o silêncio futuro elimina o chamado antes da partição; sem pergunta, o juiz retorna `None`, mas o teste indexa `porta['verbos']` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1223`). O ramo de 2020 poderia distinguir a ausência de `fila` no pai, mas está inacessível pelo erro anterior (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`).

**RED/GREEN observado por suite Django contra `dc3ba57f` e contra esta obra: [nao verificado].**

## 31/10 02:36 · sessao f790b02d836e · - · - · origem=Ronald (tmux)

**Bloqueia: o teste novo de silenciamento não certifica a cura.** [nao verificado]

- `test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` importa `Chamado`, símbolo inexistente no módulo; o modelo é `ChamadoColaborador`. Portanto termina em `ImportError` antes dos subtestes, tanto na cópia quanto no pai reconstruído pelo diff. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, helper correto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`.
- Corrigido somente esse import, o caso de 2099 continua errado: `qs_na_fila` exclui o silenciamento futuro antes de alimentar o juiz; sem perguntas, este retorna `None`, e `porta['verbos']` produz `TypeError`. Referências: `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`, `app/chamados/juizes.py:1146`, `app/chamados/juizes.py:1153`, `app/chamados/juizes.py:1223`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`.
- O caso de 2020, depois dessa correção, alcança `porta_fila`, `historico`, `fila=()`, `fio=chamado` na cópia; o pai não devolvia `fila`, de modo que a leitura nova encontra `KeyError`, não comparação comportamental com número indevido. Referências: `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1227`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:149`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; ausência da chave no pai consta do diff anexo `[nao verificado]` quanto ao checkout histórico.
- O teste novo da frase detecta os textos antigos: `sem pergunta viva`, `nao ha pergunta` e `chamado N` violam suas asserções. Porém a primeira falha textual impede chegar aos mocks naquele código antigo; isso demonstra mordida textual, sem demonstrar por si a correção universal da frase nova. Referências: `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:125`, `app/folha/tests/test_haiku_fora_da_folha.py:126`; textos antigos no diff anexo `[nao verificado]` quanto ao checkout histórico.

**Demais itens do checklist:**

- **Segundo juiz registrado: não identifiquei criação pelo diff.** A autoridade já está registrada, a escada continua no juiz e a folha lê sua saída: `app/core/juizes.py:132`, `app/core/juizes.py:253`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:55`, `app/folha/fora_da_folha.py:68`.
- **Segundo escritor fora da porta: não identifiquei escrita persistente nova.** As alterações produzem valores de retorno e textos de apresentação: `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:198`.
- **Relógio: nenhuma leitura nova introduzida pelo diff.** A cadeia herda leituras independentes em `app/chamados/services/painel.py:188` e `app/chamados/services/verbos.py:35`; o queryset da fila é consumido para células e novamente para a partição em `app/chamados/juizes.py:1146` e `app/chamados/juizes.py:1153`. Logo não há instante único recebido pela borda nessa cadeia; não atribuir sua introdução à F5C.
- **Fonte única: a constante é compartilhada, mas isso não prova identidade dos universos.** O juiz chega à constante depois do filtro adicional de `qs_na_fila`: `app/chamados/services/verbos.py:18`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`.
- **Ordem de trava e pouso antes de pré-requisito: execução desta obra `[nao verificado]`.** A norma exige suite inteira verde antes do opus final (`CLAUDE.md:600`) e impede o Codex de aprovar sozinho o pouso (`CLAUDE.md:599`). A dependência documental é F5B antes de F5, mas a mesma linha traz estado “registrada (depois da F5)”; isso é inconsistência documental, não prova da sequência executada: `app/docs/BACKLOG.md:777`. Sem prova da sequência, não acusar pouso indevido; também não certificá-lo `[nao verificado]`.

Verifiquei o import e os resultados 2020/2099 com AST e execução das funções reais isoladas, em memória, com dependências substituídas. Não rodei Django, suite, banco ou logs; resultados integrados e RED empírico em `dc3ba57f` permanecem `[nao verificado]`.

## 31/10 02:36 · sessao 2c86673a353d · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **DESTRAVA['chamado'] não é universal — bloqueia.** O juiz devolve `chamado` para qualquer conjunto sem `cobrar` nem `validar`, incluindo `None` e `historico`; não garante `decidir` (`app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`). Porém a frase manda abrir “o chamado da fila ou a pergunta viva no app” **e decidir** (`app/folha/fora_da_folha.py:43`). Contraexemplo de código: chamado da célula, aberto, silêncio vencido, sem pergunta: entra em `qs_na_fila`, recebe `historico`, mantém porta/fio `chamado`, mas devolve `fila=()` (`app/chamados/services/painel.py:186`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Não há chamado visível nem pergunta no app nesse caso; ocorrência em produção [nao verificado].

2. **GRUPOS['C'] é compatível com a escada, nas duas rotas — não bloqueia isoladamente.** “Tem porta aberta” vale quando o juiz retorna dict; fio `chamado` significa ausência de `cobrar` e `validar`, independentemente da rota (`app/folha/fora_da_folha.py:36`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1223`). Isso descreve os verbos; não garante que alguma porta esteja visível na lista padrão (`app/chamados/juizes.py:1226`).

3. **Drawer: frase igual; ação do link incoerente — bloqueia a alegação de coerência completa.** A ferramenta usa `DESTRAVA[_f]` e o drawer lê o mesmo mapa por `FIO_DO_CARTORIO`; portanto a frase para o mesmo fio é literalmente igual (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`). Mas o fio `chamado` oferece **“Validar no fio”**, enquanto a nova frase diz **“decidir”** e afirma que o painel não põe a porta em validar (`app/folha/retrato_folha.py:977`, `app/folha/fora_da_folha.py:43`). Esse link é materializado por `portas_da_linha` e renderizado no template (`app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:60`).

4. **Drawer não recebe o novo dado `chamado`.** Seu retrato grava `dono` derivado do grupo, datas e contagens; não grava números de chamados (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`). A view lê a linha gravada e chama `o_que_fazer`/`portas_da_linha`; não chama `o_que_segura` (`app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Logo a igualdade demonstrada é de frase para o mesmo fio, não de dados atuais ou números apresentados (`app/folha/fora_da_folha.py:196`, `app/folha/retrato_folha.py:1069`). Coerência temporal em produção [nao verificado].

Censo de escritores/leitores:

| Valor | Escritor | Leitores |
|---|---|---|
| `GRUPOS['C']` | `app/folha/fora_da_folha.py:36` | `retrato_folha.ajuda`, pelo mapa fio→letra (`app/folha/retrato_folha.py:70`, `app/folha/retrato_folha.py:177`); chaves de grupos em `fora_da_folha` (`app/folha/fora_da_folha.py:139`); testes (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276`, `app/folha/tests/test_retrato_folha_quadros.py:1285`). |
| `DESTRAVA['chamado']` | `app/folha/fora_da_folha.py:43` | `o_que_segura` (`app/folha/fora_da_folha.py:197`), drawer (`app/folha/retrato_folha.py:1069`), template via `retrato_fazer` (`app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:37`), testes (`app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`, `app/folha/tests/test_retrato_folha_drawer.py:226`). |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista (`app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:199`) | Teste novo (`app/folha/tests/test_haiku_fora_da_folha.py:138`). Consumidor produtivo externo ou em mensageria [nao verificado]. |
| `porta['fila']` | `app/chamados/juizes.py:1226`, `app/chamados/juizes.py:1227` | `o_que_segura` (`app/folha/fora_da_folha.py:196`); testes de dict inteiro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`) e teste novo (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`). |

Vigia e pautas continuam leitores **indiretos** da porta: o vigia lê `verbos` e `rota`, sem usar `fila` (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`); pautas dos discordantes leem `_fio_do_dia` e comparam com `FIO_SEM_PORTA`, sem usar o dado `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`).

O teste novo de frase verifica substrings proibidas e dois mocks com verbo `decidir`; não cobre `None`, `historico` nem a porta real e não chama o drawer (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`). Ele pode reprovar a frase antiga, mas não prova a universalidade da nova (`app/folha/tests/test_haiku_fora_da_folha.py:124`).

## 31/10 02:35 · sessao 5f07ffeaa5e5 · - · - · origem=Ronald (tmux)

**Bloqueia: o teste novo de silenciamento não chega à verificação pretendida.** Ele importa `Chamado`, mas o modelo é `ChamadoColaborador` e não há definição/alias `Chamado` no módulo; o próprio helper usa o nome correto. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`.

**Bloqueia também após corrigir o import:** o caso com silêncio em 2099 exige uma porta com verbo `'arquivado'`, mas `alimentar_portas` consulta `qs_na_fila`, que exclui silêncio futuro. Sem pergunta, o juiz devolve `None`, e o teste tenta `porta['verbos']`. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`, `app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1223`.

**Bloqueia a promessa de equivalência universal painel/fila:** existe contraexemplo de código para verbo `None`. Chamado DA CÉLULA, status `'aberto'`, módulo `'gps_ausente'`, silêncio futuro:

- Natureza retorna `None`, pois auditoria tem precedência; o silêncio somente converte verbos diferentes de `None`. Referências: `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:47`, `app/chamados/services/verbos.py:48`.
- Painel padrão mantém: status está em `COBRANCA` e `None` não está em `FORA_DA_LISTA_PADRAO`. Referências: `app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/services/verbos.py:18`.
- Juiz exclui antes da partição, pelo silêncio futuro, portanto não fornece esse número em `fila`. Referências: `app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1226`.
- Existência desse caso em produção: **[nao verificado]**.

Comparação no mesmo instante, sem filtros extras/paginação e restrita aos chamados da célula:

| Verbo/condição | Painel padrão | `fila` do juiz | Fonte |
|---|---:|---:|---|
| `validar`, `cobrar`, `decidir`, sem silêncio | inclui | inclui | `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1226` |
| `arquivado` | exclui | exclui antes de formar a porta | `app/chamados/services/verbos.py:18`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326` |
| `historico` | exclui | exclui da nova `fila`; pode permanecer em `verbos` | `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1226` |
| `None`, sem silêncio ou silêncio vencido | inclui se status `COBRANCA` | inclui | `app/chamados/services/painel.py:314`, `app/chamados/services/verbos.py:48`, `app/chamados/juizes.py:1226` |
| `None`, silêncio futuro | **inclui** | **exclui** | referências do contraexemplo acima |

Sob essas condições, a nova `fila` é subconjunto da lista padrão: o juiz primeiro restringe `COBRANCA` sem silêncio futuro e depois aplica a mesma exclusão de verbos. Não há caminho inverso demonstrado no código, de `fila` incluir chamado da célula que o painel padrão exclui. Referências: `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1226`.

Censo solicitado:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `FORA_DA_LISTA_PADRAO` | declaração única `app/chamados/services/verbos.py:18` | painel `app/chamados/services/painel.py:227`, `app/chamados/services/painel.py:327`; juiz `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1226` |
| `porta['fila']` | juiz monta tupla ordenada `app/chamados/juizes.py:1226` e retorna `app/chamados/juizes.py:1227` | `o_que_segura`: `app/folha/fora_da_folha.py:196`; teste direto `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; comparações do dict completo `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182` |
| `alimentacao['fila_cel']` — entrada, distinta da nova saída `fila` | `alimentar_portas`: `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1153` | juiz `app/chamados/juizes.py:1217` |
| `porta['verbos']` | juiz `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1222`, `app/chamados/juizes.py:1227` | vigia `app/chamados/services/vigia_fio.py:598`; testes `app/chamados/tests/test_o261e_f5b_juiz_porta.py:130`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156` |
| `porta['rota']` | juiz `app/chamados/juizes.py:1227` | vigia `app/chamados/services/vigia_fio.py:599`, `app/chamados/services/vigia_fio.py:601` |
| `porta['fio']` | escada `app/chamados/juizes.py:1175`, retorno `app/chamados/juizes.py:1228` | folha `app/folha/fora_da_folha.py:68` |

O vigia não lê a nova `fila`; continua contando portas e papéis a partir de `rota` e `verbos`. Assim, porta com `verbos={id:'historico'}` e `fila=()` continua entrando no balde da porta e exibindo papel `'historico'`. Referências: `app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`.

Os dois asserts diretos de dict completo foram atualizados no diff; a comparação lote versus individual compara os dois resultados atuais. Um mock antigo omite `fila`, mas o vigia somente acessa `rota/verbos`, portanto essa omissão não quebra esse teste. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`, `app/chamados/tests/test_o261e_f5_alcance.py:306`, `app/chamados/services/vigia_fio.py:598`.

Nenhuma suite foi executada; resultado observado de execução e RED efetivo contra `dc3ba57f`: **[nao verificado]**.

## 31/10 02:35 · sessao b70c86402761 · - · - · origem=Ronald (tmux)

Numeração dos anexos colados, contando a primeira linha de conteúdo como linha 1. Classificação referente à volta FINAL fornecida; conteúdo e respostas das duas chamadas de contrato: **[nao verificado]**.

| achado | classe | trecho da pergunta | trecho da resposta | linha da triagem (se houver) |
|---|---|---|---|---|
| **A — subshell** | **(a) o Codex não viu o caso específico.** Apontou outros defeitos do parser (`dj_codex_final.resp:16`). | “onde pode deixar passar o que eles mandam negar?” (`dj_codex_final.q:4`); “algum caso passa sem morder” (`dj_codex_final.q:8`). | “`… CODEX_VOLTA=final bash bin/codex_pergunta.sh …` perde papel/volta no parser”. O trecho identifica o invólucro `bash`, sem apontar subshell (`dj_codex_final.resp:16`). | **P16, procedente:** “Cura o parser”; não foi descartado (`dj_triagem_final.txt:19`). |
| **B — literal/regex de B3** | **(b) o Codex viu e a triagem descartou.** (`dj_codex_final.resp:12`; `dj_triagem_final.txt:29`). | “Obra com 3 reprovacoes vira pergunta de desenho, sem 4a volta.” (`dj_codex_final.q:2`). | “Exige meta `verificador`, prompt com `LEITURA FINAL`, `obra <ID>` conforme regex”; “Outra grafia, canal ou verificação via Codex não recebe a mesma guarda.” (`dj_codex_final.resp:12`). | **D6:** “identificacao de reprovacao por regex/LEITURA FINAL: forma declarada do verificador da casa”; “Declarar.” (`dj_triagem_final.txt:29`). |
| **C — comando no RELATO aciona Stop / expõe argv** | **(a) o Codex não viu a interação específica.** O diagnóstico correlato trata do registro, sem apontar esses efeitos (`dj_codex_final.resp:19`). | “trilha de cada negacao no RELATO” (`dj_codex_final.q:2`); “alteram alguma decisao de Stop que ja existia (libera/bloqueia)” (`dj_codex_final.q:7`). | “o registro automático no RELATO contém regra, ferramenta e alvo, sem garantir essa anotação na linha da obra.” Esse trecho não denuncia o comando cru como afirmação de ato nem sua exposição pública (`dj_codex_final.resp:19`). | Nenhum descarte específico de C identificado **[nao verificado]**. P8 trata de garantir o registro; P9, da concorrência entre escritores (`dj_triagem_final.txt:11`; `dj_triagem_final.txt:12`). |

Em B, o Codex identificou a restrição de reconhecimento; o resultado quantitativo “4 de 9” aparece no achado posterior, não nesse diagnóstico (`dj_codex_final.resp:12`; `achados_opus.txt:3`).

## 31/10 02:33 · sessao 50280be317b3 · - · - · origem=Ronald (tmux)

1. **Estado de A, B e C**

**A — a falha de subshell descrita foi corrigida.** Este comando agora é reconhecido; os parênteses separam trechos, e o selo exige a negativa por orçamento nesse exemplo (`bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/tests/test_disjuntor.sh:487`):

```bash
cd /x && (CODEX_OBRA=X CODEX_PAPEL=verificador bin/codex_pergunta.sh --anexo d.diff h q r)
```

Isso não fecha o reconhecimento de Bash: há falsos negativos e falsos positivos novos, detalhados no item 2 (`bin/disjuntor.py:751`, `bin/disjuntor.py:787`, `bin/disjuntor.py:817`).

**B — parcialmente corrigido; ainda existem verificadores finais que desaparecem da contagem.** Caixa e acentos são normalizados, e os exemplos novos com markdown/dois-pontos são cobertos (`bin/disjuntor.py:459`, `bin/disjuntor.py:473`, `bin/tests/test_disjuntor.sh:345`). Entretanto, estes prompts concretos dão `_eh_final=False`:

```text
FINAL revisão da obra OBRA-X
```

```text
Revisão
FINAL da obra **OBRA-X**
```

A expressão só aceita `final` antes de `review`, e não atravessa quebras de linha. Com esses prompts e um `SubagentHandback` contendo `REPROVADO`, `_veredito_e_obra` retorna `(None, None)`; o agente não entra em `b3` **nem em `b3_sem_obra`** (`bin/disjuntor.py:88`, `bin/disjuntor.py:495`, `bin/disjuntor.py:520`).

Há também inconsistências de identificação:

| Prompt | Obra extraída |
|---|---|
| `LEITURA FINAL da obra O261 (C)` | `O261 (C)` |
| `LEITURA FINAL da obra **O261** (C)` | `O261` |
| `LEITURA FINAL da obra obra-x` | nenhuma |

O markdown impede a captura da cauda; no último caso, a busca usa `re.I`, mas a validação posterior usa `fullmatch` sem essa opção. Isso pode repartir reprovações entre chaves ou impedir a atribuição à obra pretendida (`bin/disjuntor.py:83`, `bin/disjuntor.py:473`, `bin/disjuntor.py:481`, `bin/disjuntor.py:532`). A situação atual dos “4 de 9” mencionados no anexo é **[não verificado]**: seus nove prompts não foram fornecidos.

**C — não encontrei reprodução do vazamento original pelo comando cru.** Para o payload abaixo, a guarda nega, mas o comando vira somente hash/tamanho na linha pública; seu texto segue para `ALVO` (`bin/disjuntor.py:135`, `bin/disjuntor.py:178`, `bin/disjuntor.py:184`):

```json
{"tool_name":"Bash","tool_input":{"command":"echo deploy NO AR e FEITO; touch DISJUNTOR_DESLIGADO"}} [nao verificado]
```

A proteção ainda permite texto escolhido nos metadados e linhas que casam `ATOS`, como mostro no item 4 (`bin/disjuntor.py:186`, `bin/relato_afirma_com_prova.py:45`).

2. **Chamada real perdida e mera menção reconhecida**

Uma chamada real que agora escapa:

```bash
bin/codex_pergunta.sh</dev/null --anexo app/docs/LEIS.md h q r
```

Bash admite esse redirecionamento colado ao nome do comando. O novo regex exige espaço ou fim imediatamente depois de `.sh`, portanto `_codex_cmds` retorna `[]`; ficam sem aplicação tanto ANEXO-ENXUTO quanto B4-CODEX. O regex anterior fornecido no diff aceitava a fronteira antes de `<` (`bin/disjuntor.py:751`, `bin/disjuntor.py:841`, `bin/disjuntor.py:845`; comparação anterior: `delta.diff`, hunk de `_INVOC_RE`).

Outra regressão concreta:

```bash
cat <<<EOF
bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r
```

O wrapper é executado depois do `cat`, mas o scanner encontra `<<EOF` a partir do **segundo** `<` do here-string. Sem terminador posterior, pula até o fim e perde a chamada (`bin/disjuntor.py:787`, `bin/disjuntor.py:790`, `bin/disjuntor.py:799`).

Um falso positivo novo:

```bash
f() { bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r; }
```

Isso apenas define `f`. O parser separa `()` e remove `{`, tratando o corpo como chamada imediata. Com esse anexo, produz ANEXO-ENXUTO; sem o anexo, pode produzir B4-CODEX quando o orçamento estiver excedido (`bin/disjuntor.py:753`, `bin/disjuntor.py:754`, `bin/disjuntor.py:817`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`).

3. **Quem escreve e quem lê**

| Valor | Escritor | Leitor |
|---|---|---|
| Estado persistido `b3_sem_obra` | `_veredito_e_obra` sinaliza obra ilegível com `''`; `varrer_b3` coleta o agente e acrescenta seu ID à lista, sem duplicá-lo. `_com_estado` persiste o JSON (`bin/disjuntor.py:497`, `bin/disjuntor.py:520`, `bin/disjuntor.py:527`, `bin/disjuntor.py:242`). | A própria varredura consulta a lista para deduplicar; `_b3_sem_obra` lê e calcula seu tamanho (`bin/disjuntor.py:529`, `bin/disjuntor.py:1058`). |
| Chave persistida `b3` | `varrer_b3` cria registros, acrescenta reprovações e marca disparos; `rearma_b3` escreve `rearmado`, zera reprovações e desmarca o disparo (`bin/disjuntor.py:526`, `bin/disjuntor.py:532`, `bin/disjuntor.py:541`, `bin/disjuntor.py:558`). | Varredura, rearme e `b3_disparados`; os resultados alimentam PreToolUse e Stop (`bin/disjuntor.py:538`, `bin/disjuntor.py:553`, `bin/disjuntor.py:565`, `bin/disjuntor.py:912`, `bin/hook_stop_fila1.py:1022`). |
| Evento `ALVO` | `relato_negacao` prepara o evento; `trilha` faz o append físico (`bin/disjuntor.py:185`, `bin/disjuntor.py:107`). | Leitor explícito encontrado: o teste 26. `ler` percorre o arquivo, mas descarta `ALVO`, pois filtra outros eventos; consumidor funcional nos cinco arquivos examinados: não encontrei (`bin/tests/test_disjuntor.sh:557`, `bin/disjuntor.py:998`, `bin/disjuntor.py:1004`). |
| Linha do RELATO | Construída e validada em `relato_negacao`, delegada a `no_relato`; escrita física por temporário e `os.replace` (`bin/disjuntor.py:187`, `bin/disjuntor.py:194`, `bin/disjuntor.py:201`, `app/core/esteira_vigia.py:879`). | A trava lê `ln` antes da escrita; o escritor lê o RELATO anterior; o checador lê o documento completo. Stop executa esse checador e usa seu retorno para bloquear (`bin/disjuntor.py:194`, `app/core/esteira_vigia.py:860`, `bin/relato_afirma_com_prova.py:88`, `bin/hook_stop_fila1.py:1150`, `bin/hook_stop_fila1.py:1172`). |
| `d['b3_sem_obra']` apresentado por `linha()` | `_b3_sem_obra` calcula; `ler` atribui o resultado ao dicionário de apresentação (`bin/disjuntor.py:1058`, `bin/disjuntor.py:1052`). | `linha` formata o valor; `--linha`, o gerador do ESTADO e o teste 16 consomem esse fluxo (`bin/disjuntor.py:1122`, `bin/disjuntor.py:1130`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:355`). |

A varredura que atualiza o estado é chamada pelo PreToolUse e pelo Stop. `ler` apenas conta o estado já acumulado; não descobre agentes novos (`bin/disjuntor.py:912`, `bin/hook_stop_fila1.py:1022`, `bin/disjuntor.py:1058`).

4. **Texto livre, delimitadores e alcance da trava**

**Texto escolhido pelo payload: sim, limitado. `|` e crase: não encontrei passagem para a linha construída.** `limpa` permite letras/dígitos ASCII, `_` e `-`, até 40 caracteres, nos campos agente, ferramenta e subtipo. Não valida esses campos contra um catálogo; portanto `agent_id="texto|livre\`aqui"` vira `textolivreaqui` (`bin/disjuntor.py:186`, `bin/disjuntor.py:189`).

**Uma linha efetivamente aceita pode casar `ATOS`.** Payload concreto:

```json
{"tool_name":"Bash","agent_id":"FEITO-NADA","tool_input":{"command":"touch DISJUNTOR_DESLIGADO"}}
```

A guarda nega pelo comando. Na linha gerada, `FEITO` casa `ATOS`, mas `NADA` casa `NEGA`, que dispensa **a linha inteira**, independentemente da relação entre as palavras. Assim, `RA.afirmacoes(ln)` aceita a linha (`bin/disjuntor.py:135`, `bin/disjuntor.py:189`, `bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:45`).

A cobertura exata é:

- Rejeita os padrões maiúsculos `APLICADO`, `APLIQUEI`, `APLICAMOS`, `NO AR`, `DEPLOYADO`, `DEPLOIADO` e `FEITO`, com fronteiras de palavra, em título ou linha contendo `**`, sem negação/prova. A linha do disjuntor sempre contém `**` (`bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:42`, `bin/disjuntor.py:187`).
- Não detecta outras caixas, como `feito`, palavras fora dessa lista ou ocorrências sem fronteira de palavra; também aceita a linha inteira se encontrar uma das negações previstas (`bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:43`, `bin/relato_afirma_com_prova.py:45`).
- A função geral aceita `PROVA:` na própria linha ou nas seis seguintes. Na chamada nova há somente `ln`; não há vizinhas, e os campos limpos não permitem injetar `PROVA:` (`bin/relato_afirma_com_prova.py:47`, `bin/disjuntor.py:186`, `bin/disjuntor.py:194`).
- A trava nova não aplica a base histórica de exceções; essa filtragem pertence ao `main` do checador executado pelo Stop (`bin/disjuntor.py:194`, `bin/relato_afirma_com_prova.py:94`).

Sem uma negação, o payload `tool_name="FEITO"` com `file_path="/r/DISJUNTOR_DESLIGADO"` é barrado na saída: não acrescenta linha e registra `FALHA relato_negacao`. A negativa da ferramenta já foi emitida (`bin/disjuntor.py:137`, `bin/disjuntor.py:195`, `bin/disjuntor.py:203`, `bin/disjuntor.py:947`).

5. **Força dos casos novos e dos mutantes**

`_passa` exige stdout vazio e nenhuma `FALHA` nova, mas não exige evidência de reconhecimento. Um parser retornando `[]` também satisfaz essa condição (`bin/tests/test_disjuntor.sh:91`, `bin/disjuntor.py:841`).

| Casos | Avaliação |
|---|---|
| B3 14–16 | **14 passa se aquele prompt não for reconhecido.** 15 exige a negativa B3; 16 exige numericamente o contador igual a 1 (`bin/tests/test_disjuntor.sh:349`, `bin/tests/test_disjuntor.sh:351`, `bin/tests/test_disjuntor.sh:356`). |
| Codex 24–32 | 24, 26–28 exigem negativas concretas. **25 passa também quando a chamada desaparece.** 29–32 verificam ausência de negativa; isoladamente, não distinguem exclusão correta de omissão do parser (`bin/tests/test_disjuntor.sh:487`, `bin/tests/test_disjuntor.sh:488`, `bin/tests/test_disjuntor.sh:492`). |
| Trilha 2–3 e 21–29 | 2–3 exigem linhas completas. 21, 23, 25 e 28 verificam ausência. Há contrapartes positivas em 22, 24, 26, 27 e 29, mas 25 aceita qualquer saída do Stop sem o nome do checador (`bin/tests/test_disjuntor.sh:542`, `bin/tests/test_disjuntor.sh:544`, `bin/tests/test_disjuntor.sh:553`, `bin/tests/test_disjuntor.sh:560`). |

**Há uma lacuna específica em C:** depois do comando com `NO AR/FEITO`, os casos 22–26 não exigem aumento do número de linhas no RELATO. Omitir esse registro depois de escrever `ALVO` satisfaria esse bloco: a negativa existe, o texto contaminante não aparece, o checador passa e `ALVO` existe. Isso não comprova a exigência de uma linha segura para aquela negativa (`bin/tests/test_disjuntor.sh:553`, `bin/tests/test_disjuntor.sh:557`).

Quanto aos mutantes:

- **`A-sem-continuacao`:** o caso 25 continua passando por ausência; quem o mata é 26, que exige ANEXO-ENXUTO. Logo, 25 sozinho não demonstra a continuação reconhecida (`bin/tests/test_disjuntor.sh:488`, `bin/tests/test_disjuntor.sh:489`, `bin/tests/test_disjuntor.sh:725`).
- **`C-alvo-cru`:** troca o campo ferramenta pelo alvo. Já cai no caso 2 porque desaparece `Agent general-purpose`; mistura quebra de formato e vazamento, sem isolar as verificações posteriores de privacidade/ATOS (`bin/tests/test_disjuntor.sh:666`, `bin/tests/test_disjuntor.sh:542`, `bin/disjuntor.py:189`).
- **`C-sem-trava-de-saida`:** tem verificação específica: ferramenta `FEITO`, nenhuma linha adicional e presença do `ValueError` esperado (`bin/tests/test_disjuntor.sh:560`, `bin/tests/test_disjuntor.sh:562`, `bin/tests/test_disjuntor.sh:667`).

Não encontrei por inspeção um mutante novo necessariamente sobrevivente; resultado integral dos mutantes **[não verificado]**. A suíte cria temporários, escreve cópias e altera arquivos nessas cópias, incompatível com a regra desta análise (`bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:612`, `bin/tests/test_disjuntor.sh:621`).

6. **Exceções, efeitos colaterais e regressões**

**Há uma exceção nova não tratada em `--linha`, condicionada a estado com tipo inválido.** Um JSON válido como:

```json
{"b3_sem_obra":1}
```

passa pela validação de `ler_estado`, que só exige objeto. `_b3_sem_obra` executa `len(1)` e captura apenas `RuntimeError`; o `TypeError` sobe por `ler` e por `main --linha`. O escritor normal produz lista; existência desse estado em produção **[não verificado]** (`bin/disjuntor.py:264`, `bin/disjuntor.py:527`, `bin/disjuntor.py:1058`, `bin/disjuntor.py:1130`). O gerador do ESTADO captura esse erro e apresenta “indisponível” (`bin/gerar_estado.py:282`).

Os demais efeitos identificados:

- **Import e `sys.path`:** cada tentativa de relato acrescenta outra entrada no início de `sys.path`. Importar RA não executa seu `main` nem escreve a base histórica; essa escrita está dentro de `main`, protegido pelo teste de `__name__` (`bin/disjuntor.py:192`, `bin/relato_afirma_com_prova.py:100`, `bin/relato_afirma_com_prova.py:123`).
- **I/O adicional:** `ALVO` é escrito antes da validação. A trilha normaliza espaços e corta o detalhe inteiro em 300 caracteres; portanto não preserva necessariamente o alvo completo, embora hash e tamanho sejam calculados sobre ele (`bin/disjuntor.py:108`, `bin/disjuntor.py:184`, `bin/disjuntor.py:190`).
- **Persistência não garantida:** falha ao escrever `ALVO` é engolida; a linha pública ainda pode afirmar “na trilha privada” (`bin/disjuntor.py:110`, `bin/disjuntor.py:185`, `bin/disjuntor.py:190`).
- **“Privada” não impõe permissão própria:** a abertura pede modo `0644`, sujeito ao umask. Permissões efetivas de produção **[não verificado]** (`bin/disjuntor.py:99`, `bin/orcamento_contexto.py:74`).
- **Exceções do registro são contidas:** falhas comuns de hash/import/validação/escrita entram em `FALHA relato_negacao`; a negativa já emitida permanece (`bin/disjuntor.py:203`, `bin/disjuntor.py:947`).
- **Compatibilidade anterior:** além das regressões Bash do item 2, IDs puramente alfabéticos antes aceitos pelo regex fornecido, como `ABC`, agora não satisfazem `_OBRA_ID`. Uso desses IDs em produção **[não verificado]** (`bin/disjuntor.py:81`, `bin/disjuntor.py:473`; comparação anterior: `delta.diff`, hunk de `OBRA_RE`).

## 31/10 02:30 · sessao 54ab8bc9f7f1 · - · - · origem=Ronald (tmux)

Resultados conferidos por leitura e probes Python `-B` em memória; não executei o selo, que cria temporários e cópias (`bin/tests/test_disjuntor.sh:37-49`, `:612-621`).

1. **B foi ampliado, mas permanece incompleto.** Os prompts novos do selo são reconhecidos (`bin/tests/test_disjuntor.sh:345-351`), porém:
   - `Revisão\nFINAL da obra **OBRA-X**` e `FINAL revisão da obra OBRA-X` dão `_eh_final=False`: a expressão não atravessa quebra de linha e só admite `final` antes de `review`, não antes de `revisão` (`bin/disjuntor.py:88`, `:463-465`). `_veredito_e_obra` retorna `(None,None)` e esses agentes não entram nem em `b3_sem_obra` (`:495-497`, `:520-523`).
   - `LEITURA FINAL da obra obra-x` dá obra `None`: os padrões usam `re.I`, mas o `fullmatch` posterior não usa (`bin/disjuntor.py:84-85`, `:473-474`). Entra no contador sem obra, sem incrementar a obra pretendida (`:497`, `:520-523`).
   - `LEITURA FINAL da obra O261 (C)` produz `O261 (C)`; `LEITURA FINAL da obra **O261** (C)` produz `O261`. A cauda exige whitespace imediatamente depois do ID; markdown separa as chaves (`bin/disjuntor.py:83-85`, `:481-482`).
   - `obra ABC` era aceito pela expressão anterior fornecida no diff; agora exige hífen ou letras+algarismos (`bin/disjuntor.py:81`). Uso desses formatos em produção: [não verificado].

2. **Testes por ausência de sinal.** `_passa` exige stdout vazio e nenhuma `FALHA` nova; não exige comprovação de que a invocação foi reconhecida (`bin/tests/test_disjuntor.sh:91-94`).
   - B3 caso14 passa também se aquele prompt final não for reconhecido; caso15 fornece a contraparte positiva (`:349-351`). Caso16 exige numericamente um agente sem obra (`:353-356`).
   - Codex25 passa também com parser retornando `[]`; confirmei isso aplicando somente o mutante `A-sem-continuacao` em memória (`:488`, `:725`). O mesmo mutante também retorna `[]` para o comando26, mas **26 o mata** porque exige `ANEXO-ENXUTO` (`:489`). Codex29–32 também verificam autorização por ausência; 24,26–28 verificam negativas concretas (`:487-495`).
   - Trilha21,23,25,28 são verificações de ausência (`:544`, `:554`, `:556`, `:561`). Há contrapartes positivas: 2/3 exigem linhas com formato completo;22/27 exigem negativas;24 executa o checador;26 exige ALVO;29 exige a falha específica da trava (`:542-562`). Particularmente25 aceita qualquer saída do Stop sem o nome do checador, inclusive vazio por outra falha (`:556`).

3. **Mutantes novos.**
   - `C-alvo-cru` troca a ferramenta pelo alvo, alterando simultaneamente formato e privacidade (`bin/tests/test_disjuntor.sh:666`; `bin/disjuntor.py:189`). Já cai no caso2, que exige `Agent general-purpose`; não isola as verificações de vazamento/ATOS posteriores (`bin/tests/test_disjuntor.sh:542`, `:544`, `:553-557`).
   - `C-sem-trava-de-saida` tem isolamento melhor: ferramenta `FEITO` deve manter a negativa CHAVE, não acrescentar linha e produzir o `ValueError` específico (`bin/tests/test_disjuntor.sh:560-562`, `:667`).
   - Não encontrei, por inspeção, mutante novo necessariamente sobrevivente; execução integral dos mutantes: [não verificado].

4. **Escritores/leitores B3.**
   - `b3_sem_obra`: produtor lógico `_veredito_e_obra`, que emite `''` (`bin/disjuntor.py:497`); coleta em `varrer_b3` (`:520-521`); escritor da lista `_fn` (`:527-530`); persistência `_com_estado` (`:237-243`); leitura e contagem `_b3_sem_obra` (`:1056-1060`); produtor do campo de apresentação `ler` (`:1052`); leitor final `linha` (`:1122`).
   - `b3`: criação/escrita de reprovados/disparado em `varrer_b3` (`bin/disjuntor.py:526`, `:532-546`); leitura e escrita de rearme em `rearma_b3` (`:553-559`); leitor `b3_disparados` (`:564-565`); consumidores da varredura PreToolUse (`:909-916`) e Stop (`bin/hook_stop_fila1.py:1022-1026`).
   - Chamadores que provocam escrita pela varredura: os mesmos PreToolUse e Stop; `ler` apenas conta o estado já acumulado (`bin/disjuntor.py:912`, `:1052-1058`; `bin/hook_stop_fila1.py:1022`).

5. **Exceção nova concreta.** Com estado lógico `{'b3_sem_obra':7}`, `_b3_sem_obra` levanta `TypeError`, pois executa `len(7)` e só captura `RuntimeError`; confirmei com `ler_estado` substituído em memória (`bin/disjuntor.py:1056-1060`). `--linha` não contém captura ao redor de `ler/linha` (`:1129-1131`). Strings e dicionários são contados silenciosamente por `len`, embora o escritor normal produza lista (`:527-530`, `:1058`). Estado desse tipo em produção: [não verificado].

## 31/10 02:30 · sessao ef602f7d040e · - · - · origem=Ronald (tmux)

Achados de C e perguntas 3/4/6, obtidos só por leitura e probes Python `-B` com escrita substituída por funções em memória:

- **C original, comando cru no RELATO:** não reproduzi. `tool_input.command` vira `alvo`, que entra apenas no hash/tamanho da linha; o texto vai a `trilha('ALVO', ...)` (`bin/disjuntor.py:178-190`). Com `{"tool_name":"Bash","tool_input":{"command":"echo deploy NO AR e FEITO; touch DISJUNTOR_DESLIGADO"}}`, a linha resultante contém `Bash -- alvo sha1:e02b244fde (52 car., na trilha privada)`, sem o comando; a própria negação decorre de `guarda_chave` (`bin/disjuntor.py:135-136`).

- **Ainda há texto escolhido pelo payload:** `limpa` aceita qualquer sequência de letras/dígitos ASCII, `_` e `-`, até 40 caracteres, nos campos `agent_id`, `tool_name` e `subagent_type`; não valida catálogo desses campos (`bin/disjuntor.py:186-190`). Exemplo: `agent_id="texto-livre|`XYZ`"` produz `agente texto-livreXYZ`. `|`, crase, estrelas e quebras de linha não passam pela whitelist nesses campos; alvo cru só gera digest hex e tamanho (`bin/disjuntor.py:184-190`).

- **Uma linha gravada pode casar `ATOS`:** payload concreto:
  ```json
  {"tool_name":"Bash","agent_id":"FEITO-NADA","tool_input":{"command":"touch DISJUNTOR_DESLIGADO"}}
  ```
  A negação ocorre por `bin/disjuntor.py:135-136`; a linha contém `agente FEITO-NADA`. `FEITO` casa `ATOS`, mas `NADA` casa `NEGA` e faz `RA.afirmacoes` ignorar a linha toda (`bin/relato_afirma_com_prova.py:30-49`). Probe confirmou gravação. Outra variante: `tool_name="FEITO", agent_id="NADA", file_path="/r/DISJUNTOR_DESLIGADO"`; o caminho dispara a guarda independentemente do nome da ferramenta (`bin/disjuntor.py:137-138`).

- **Cobertura real da trava:** a linha gerada sempre possui `**`; por isso é “forte”, mesmo quando o ato aparece fora do trecho em negrito. A trava rejeita os sete padrões maiúsculos de `ATOS`, com fronteira de palavra, salvo presença de negação em qualquer lugar da linha. Também aceita prosa sem `**`/título, outras caixas (`feito`), palavras fora de `ATOS`, formas embutidas em identificador (`SEM_EFEITO`), e linhas com `PROVA:` na própria linha ou nas seis seguintes (`bin/relato_afirma_com_prova.py:30-50`). Para a chamada nova `RA.afirmacoes(ln)`, só existe uma linha, sem vizinhas; `PROVA:` não é gerável pelos campos limpos (`bin/disjuntor.py:186-195`).

- **Payload `FEITO` sem negação:** `{"tool_name":"FEITO","tool_input":{"file_path":"/r/DISJUNTOR_DESLIGADO"}}` provoca `ValueError` na trava; não entra no RELATO, mas deixa `ALVO` e `FALHA relato_negacao`. Probe confirmou. A exceção é capturada e a negativa já foi emitida antes de tentar o relato (`bin/disjuntor.py:194-204`, `bin/disjuntor.py:947-950`).

**Escritores/leitores pedidos:**

| Valor | Escritor | Leitor |
|---|---|---|
| Evento `ALVO` | Preparado em `bin/disjuntor.py:185`; append físico em `bin/disjuntor.py:103-109` | Leitor explícito localizado: teste `bin/tests/test_disjuntor.sh:557`. `ler` lê o arquivo, mas filtra e descarta `ALVO`, pois só percorre os eventos de `bin/disjuntor.py:998-1005`; consumidor funcional de `ALVO` nos cinco arquivos: não encontrei. |
| Linha do RELATO | Construída em `bin/disjuntor.py:187-190`, validada em `:194`, delegada em `:199-201`; escritor físico `app/core/esteira_vigia.py:859-884` | RA lê o arquivo em `bin/relato_afirma_com_prova.py:86-96`; Stop invoca o checador em `bin/hook_stop_fila1.py:1148-1155`, bloqueia por retorno não limpo em `:1172-1177`; testes em `bin/tests/test_disjuntor.sh:542-562`. O escritor também lê o RELATO anterior em `app/core/esteira_vigia.py:860` e `:104-107`. |
| `d['b3_sem_obra']` de `linha` | `_b3_sem_obra` calcula em `bin/disjuntor.py:1056-1060`; `ler` atribui no dicionário em `:1052` | `linha` formata em `bin/disjuntor.py:1122`; chamada de apresentação `:1130`; teste `bin/tests/test_disjuntor.sh:355`. |

**Efeitos colaterais e exceções:**

- Novo `sys.path.insert(0, ...)` permanece no processo e acrescenta mais uma entrada a cada `relato_negacao`; probe confirmou crescimento de um elemento por chamada (`bin/disjuntor.py:192-193`). O import de RA não chama `main` nem escreve a base: `main` está protegido por `if __name__ == '__main__'` (`bin/relato_afirma_com_prova.py:123-124`); a escrita da base ocorre só dentro de `main`, em `:100-110`. Eventual criação de bytecode pelo interpretador em execução normal: [não verificado].
- `ALVO` acrescenta uma escrita para cada negativa antes da validação do RELATO (`bin/disjuntor.py:185-195`). A trilha normaliza espaços e limita **todo o detalhe a 300 caracteres**, logo não guarda o alvo inteiro apesar de o SHA1 e o tamanho serem calculados sobre ele (`bin/disjuntor.py:108-109`, `:184-190`).
- A designação “privada” não impõe proteção própria: a trilha usa `_abre` (`bin/disjuntor.py:107`), que delega a `orcamento_contexto.abre` (`:97-100`); esta pede modo `0o644` na criação (`bin/orcamento_contexto.py:73-74`). Permissões/umask efetivos de produção: [não verificado].
- Falha de `trilha('ALVO')` é engolida e não impede a linha de dizer “na trilha privada” (`bin/disjuntor.py:103-111`, `:185-190`). Não há confirmação de persistência nem referência garantida ao texto.
- **Exceção nova não tratada em `--linha`:** estado JSON válido `{"b3_sem_obra":1}` passa pela validação que só exige dicionário (`bin/disjuntor.py:264-266`), mas produz `TypeError: object of type 'int' has no len()` em `_b3_sem_obra`; só `RuntimeError` é capturado (`:1056-1060`). O erro sobe por `ler` e por `main --linha`, que não contém `try` (`:1052`, `:1129-1131`). Probe em memória confirmou.
- As exceções comuns introduzidas no caminho de `relato_negacao` são contidas pelo `except Exception`, gerando falha de registro; a negação da ferramenta continua emitida (`bin/disjuntor.py:203-204`, `:947-952`).

## 31/10 02:30 · sessao 3c700e287ce2 · - · - · origem=Ronald (tmux)

- **A original foi corrigido:** `cd /x && (CODEX_OBRA=X CODEX_PAPEL=verificador bin/codex_pergunta.sh --anexo d.diff h q r)` retorna uma chamada em `_codex_cmds`; parênteses agora separam trechos e o selo exige negativa por orçamento nesse exemplo. `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/tests/test_disjuntor.sh:487`.

- **Regressão de reconhecimento, com redirecionamento colado:**
  ```bash
  bin/codex_pergunta.sh</dev/null --anexo app/docs/LEIS.md h q r
  ```
  Bash invoca o wrapper com cinco argumentos; `_codex_cmds` retorna `[]`, porque depois de `.sh` agora exige espaço ou fim. Assim não aplica ANEXO-ENXUTO nem B4-CODEX. A regex antiga fornecida aceitava o limite de palavra antes de `<`. `bin/disjuntor.py:751`, `bin/disjuntor.py:841`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`; regex antiga: `delta.diff` do usuário [não verificado em arquivo local].

- **Outra regressão causada pelo novo separador:**
  ```bash
  cat <<<EOF
  bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r
  ```
  Bash executa a chamada após o `cat`; `_codex_cmds` retorna `[]`. O primeiro `<` de `<<<` não entra no ramo heredoc, mas a iteração seguinte reconhece `<<EOF` dentro do here-string; como não encontra terminador, salta ao fim e descarta a chamada real. `bin/disjuntor.py:787`, `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:797`, `bin/disjuntor.py:799`, `bin/disjuntor.py:800`.

- **Falso positivo novo — só define uma função:**
  ```bash
  f() { bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r; }
  ```
  `_codex_cmds` retorna uma chamada com `volta=meio` e o anexo; `codex_decide` nega com ANEXO-ENXUTO, embora Bash apenas defina `f`. Sem `--anexo`, orçamento excedido produz B4-CODEX. O parser divide `()` e elimina `{`, perdendo o contexto de definição. `bin/disjuntor.py:753`, `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/disjuntor.py:823`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`.

- **Falso positivo de heredoc:** em `cat <<EOF`, uma linha contendo tabulação + `EOF` ou `EOF ` não encerra o heredoc de Bash, mas o scanner a aceita como terminador. Uma linha posterior do corpo que mencione o wrapper passa a ser interpretada como chamada. `bin/disjuntor.py:799`.

Os exemplos foram conferidos com importação `python3 -B`, orçamento substituído em memória e uma função Bash também em memória no lugar do wrapper; nenhum arquivo foi criado. A suíte completa não foi executada [não verificado]. [nao verificado]

## 31/10 02:29 · sessao 273b3485f29d · O261-E-F5C · verificador · origem=Code

**Bloqueia:** a nova `fila` ainda pode nomear chamado oculto no painel padrão; as frases negam ações que o juiz não exclui; e o drawer mantém “chamado N” literal. (`app/chamados/services/painel.py:188`, `:329`; `app/folha/fora_da_folha.py:36`, `:43`, `:200`; `app/folha/retrato_folha.py:1069`.)

`git show 366e07d4` falhou: a cópia não contém repositório Git. Portanto, diff exato, conteúdo de `dc3ba57f`, RED executado e sequência dos commits: **[nao verificado]**. Os achados abaixo são do código disponível; ocorrência e quantidade em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

| Valor | Escritor/origem | Leitores |
|---|---|---|
| `porta['fila']` | `alimentar_portas` monta `fila_cel` usando `qs_na_fila`; o juiz seleciona a célula e escreve a tupla ordenada. (`app/chamados/juizes.py:1144`, `:1148`, `:1216`, `:1224`.) | `o_que_segura` lê e interpola os números na frase. (`app/folha/fora_da_folha.py:199`, `:200`, `:202`.) |
| `GRUPOS['C']` | Literal do módulo. (`app/folha/fora_da_folha.py:36`.) | `retrato_folha.ajuda` lê a prosa pelo mapeamento fio→letra; vira `title` e chega ao template da barra. (`app/folha/retrato_folha.py:71`, `:177`, `:196`, `:825`; `app/templates/core/_barra_gestao.html:67`; inclusão em `app/templates/folha/_retrato_barra.html:122`.) `fora_da_folha` também percorre `GRUPOS`, mas somente suas chaves. (`app/folha/fora_da_folha.py:139`.) |
| `DESTRAVA['chamado']` | Literal do módulo. (`app/folha/fora_da_folha.py:43`.) | `o_que_segura` lê e personaliza; `retrato_folha.o_que_fazer` lê literalmente pelo fio gravado. (`app/folha/fora_da_folha.py:197`, `:200`; `app/folha/retrato_folha.py:1058`, `:1069`.) A view fornece o bloco e o template imprime `r.valor`. (`app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:33`, `:37`.) |

Os testes consumidores são: [nao verificado]

- **Dict inteiro do juiz:** expectativas com `fila` nas duas rotas, e igualdade singular/lote. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `:145`, `:163`.)
- **Teste novo:** injeta `fila` por mock e verifica a frase personalizada. (`app/folha/tests/test_haiku_fora_da_folha.py:125`, `:127`, `:133`, `:136`, `:139`.)
- **Prosa de `GRUPOS`:** ausência de substring no teste novo; propagação para tooltips; comparação das chaves. (`app/folha/tests/test_haiku_fora_da_folha.py:122`; `app/folha/tests/test_retrato_folha_quadros.py:154`, `:1276`, `:1285`.)
- **`DESTRAVA` literal:** igualdade do drawer, inclusive `chamado`, e restrição aos valores dos mapas. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:114`, `:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`; `app/folha/tests/test_retrato_folha_drawer.py:224`, `:229`.) Os testes de `colab` também consomem o mapa. (`app/folha/tests/test_retrato_folha_drawer.py:187`; `app/colaboradores/tests/test_retrato_no_drawer.py:210`.)

O **vigia** consome datas da folha e `verbos`/`rota` do juiz, sem ler a nova `fila` ou essas frases nesses caminhos. (`app/chamados/services/vigia_fio.py:549`, `:550`, `:596`, `:598`, `:601`.) **Pautas** consome o fio por `_alimentar`/`_fio_do_dia`, também sem essas frases. (`app/ponto/services/pautas_dos_discordantes.py:60`, `:74`, `:76`.)

2. **As frases não são verdadeiras para todo `fio='chamado'` — bloqueia**

A garantia efetiva de `fio_pelos_verbos` é: nenhum valor do mapa é `cobrar` ou `validar`. Qualquer outro valor, inclusive `historico` e `None`, resulta em `chamado`. Isso não garante ausência de pergunta pendente nem de resposta aguardando validação. (`app/chamados/juizes.py:1170`, `:1177`, `:1181`.)

**Contraexemplo permitido pelo código, nas duas rotas:** chamado aberto com `silenciado_ate` vencido e pergunta viva pendente.

- `qs_na_fila` admite silêncio vencido: exclui somente `silenciado_ate > agora`. (`app/chamados/services/painel.py:186`, `:188`.)
- A partição substitui o verbo original por `historico` quando o silêncio venceu. Assim, pode encobrir tanto `cobrar` quanto `validar`. (`app/chamados/services/verbos.py:44`, `:45`, `:48`.)
- O app continua selecionando perguntas vivas, sem filtro de silêncio; pergunta viva sem resposta é `PENDENTE`. (`app/chamados/services/pergunta_no_app.py:39`, `:42`, `:44`; `app/chamados/models.py:2207`, `:2208`.)
- Com o chamado ligado à célula, o juiz devolve `porta_fila`; com somente a pergunta ligada à célula e a âncora fora dela, devolve `porta_pergunta`. Em ambos, `historico` permite `fio='chamado'`. (`app/chamados/juizes.py:1216`, `:1218`, `:1220`, `:1224`, `:1225`.)

Logo, “nenhuma pede resposta do COLAB nem validação” pode ser falsa nas duas rotas. (`app/folha/fora_da_folha.py:36`, `:43`; cadeia acima.)

**Também há chamado nomeado invisível no painel padrão.** Na `porta_fila`, esse chamado histórico permanece em `fila`, recebe número na frase, mas o painel padrão exclui IDs com verbo `historico`. A chave nova separa chamados da célula de âncoras externas; não prova visibilidade. (`app/chamados/juizes.py:1224`; `app/folha/fora_da_folha.py:199`, `:201`; `app/chamados/services/painel.py:322`, `:327`, `:329`.)

3. **Drawer versus `o_que_segura`: incoerente — bloqueia**

`o_que_segura` troca o trecho genérico por números reais de `fila` ou somente pela pergunta no app. O drawer entrega o literal completo, incluindo **“chamado N na fila, ou…”**. (`app/folha/fora_da_folha.py:200`, `:202`; `app/folha/retrato_folha.py:1069`; `app/templates/folha/_retrato_o_que_fazer.html:37`.)

O retrato grava o fio em `dono`, mas sua linha não carrega `fila` nem `rota` para realizar essa escolha. (`app/folha/retrato_folha.py:1177`, `:1186`, `:1188`.) Além disso, o contrato já declara “N literal na tela” um bug de rótulo. (`app/docs/CORTES.md:353`.)

Os testes atuais preservam essa incoerência ao exigir igualdade com o literal, inclusive para `chamado`. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`.)

4. **Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando registrado | **Sem bloqueio identificado nos blocos examinados:** porta registrada, escada no juiz e folha lendo seu fio. (`app/core/juizes.py:253`; `app/chamados/juizes.py:1175`; `app/folha/fora_da_folha.py:68`.) |
| Segundo escritor fora da porta | **Sem bloqueio identificado nesses blocos:** montam mapas, texto e retorno, sem escrita persistente. (`app/chamados/juizes.py:1215`, `:1224`; `app/folha/fora_da_folha.py:191`, `:207`.) |
| Relógio fora da borda | Há leituras transitivas em `qs_na_fila` e `particionar_verbos`; introdução ou alteração pelo commit **[nao verificado]**. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:32`.) |
| Derivação paralela de fonte única | A alimentação consulta as fontes existentes; o defeito comprovado é tratar pertencimento a `qs_na_fila` como garantia de exibição. **Bloqueia pela conclusão indevida**, descrita no item 2. (`app/chamados/juizes.py:1140`, `:1153`; `app/chamados/services/painel.py:329`.) |
| Selo/RED sem mordida | O teste novo mocka o juiz e verifica apenas `GRUPOS['C']` e a saída de `o_que_segura`; deixa passar o drawer e não exercita silêncio vencido. **Insuficiente como selo dessas garantias.** (`app/folha/tests/test_haiku_fora_da_folha.py:121`, `:125`, `:131`, `:139`.) |
| Mordida sobre `dc3ba57f` | **[nao verificado]**: não há árvore anterior nem resultado de execução disponível. O nome “MORDE” e as asserções não evidenciam o RED exigido. (`app/folha/tests/test_haiku_fora_da_folha.py:114`; `CLAUDE.md:28`.) |
| Ordem de trava / pouso antes do pré-requisito | Corte de nascimento e teste de `registrado` fora da fila existem. (`app/docs/CORTES.md:400`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177`, `:184`.) Cumprimento temporal, suíte verde e pouso: **[nao verificado]**; suíte antes do opus é requisito explícito. (`CLAUDE.md:600`.) |

## 31/10 02:28 · sessao 41c42f95852f · - · - · origem=Ronald (tmux)

- **RED sobre `dc3ba57f`: [nao verificado].** O teste contém casos com juiz inteiramente mockado, não uma execução da árvore anterior: `app/folha/tests/test_haiku_fora_da_folha.py:125-139`. A obrigação exige vermelho evidenciado: `CLAUDE.md:28-29`, `CLAUDE.md:348-353`. **Bloqueia certificação do RED**, sem demonstrar sozinho defeito da cura.

- **Cobertura incompleta do novo teste:** verifica `GRUPOS['C']` por substring e a saída personalizada de `o_que_segura`; não verifica `DESTRAVA['chamado']` literal, drawer ou produção real da chave `fila`: `app/folha/tests/test_haiku_fora_da_folha.py:121-139`. Pode passar deixando o drawer incoerente. **Bloqueia se usado como selo dessa coerência.**

- **Juiz duplicado/fonte paralela:** não encontrei nos blocos examinados. O juiz está registrado em `app/core/juizes.py:253`; a escada continua em `app/chamados/juizes.py:1170-1181`; folha lê o fio em `app/folha/fora_da_folha.py:60-68` e a fila em `app/folha/fora_da_folha.py:195-202`. Alimentação consulta a fonte existente em `app/chamados/juizes.py:1140-1154`. **Sem bloqueio identificado nesses blocos.**

- **Segundo escritor:** os blocos de apresentação apenas montam retorno e texto: `app/folha/fora_da_folha.py:191-207`; o juiz monta retorno em `app/chamados/juizes.py:1215-1225`. **Sem escrita persistente identificada nesses blocos.**

- **Relógio:** a dependência `particionar_verbos` lê `timezone.now()` e deriva histórico: `app/chamados/services/verbos.py:32-48`. Sua introdução pelo diff é **[nao verificado]**. **Não atribuir como regressão sem histórico.**

- **Pré-requisito `registrado` fora da fila:** existe teste explícito consultando `COBRANCA` e `qs_na_fila`: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177-184`. Resultado executado **[nao verificado]**.

- **Ordem de trava/pouso:** nascimento autorizado consta em `app/docs/CORTES.md:400`; suite verde antes do opus é exigida em `CLAUDE.md:600-602`. Execução de RED → cura → GREEN → suite e cumprimento do pouso **[nao verificado]**.

## 31/10 02:28 · sessao 145bbd00c71a · - · - · origem=Ronald (tmux)

Bloqueia: `fila` nova continua podendo nomear chamado invisível ao admin.

- `qs_na_fila` aceita chamado aberto/em análise com `silenciado_ate` passado: `app/chamados/services/painel.py:186`.
- A partição muda qualquer verbo não nulo desse chamado para `historico`: `app/chamados/services/verbos.py:43`.
- `fio_pelos_verbos` devolve `chamado` para `historico`: `app/chamados/juizes.py:1175`.
- `alimentar_portas` inclui esse chamado em `fila_cel`: `app/chamados/juizes.py:1144`.
- O juiz devolve seu número em `fila`: `app/chamados/juizes.py:1224`.
- `o_que_segura` escreve “chamado N na fila”: `app/folha/fora_da_folha.py:199`.
- O painel padrão exclui justamente `historico`: `app/chamados/services/painel.py:322`.

Também bloqueia a afirmação universal das frases: sob uma pergunta viva e pendente, cujo chamado âncora tem silêncio vencido, a rota pode ser `porta_pergunta`, verbo `historico` e fio `chamado`; o app continua mostrando a pergunta, pois não filtra silêncio nem verbo histórico. Logo “nenhuma pede resposta do COLAB” não decorre da garantia do juiz. A pergunta viva sem `respondida_em` é pendente. Fontes: `app/chamados/juizes.py:1216`, `app/chamados/services/pergunta_no_app.py:39`, `app/chamados/models.py:2207`, `app/folha/fora_da_folha.py:36`, `app/folha/fora_da_folha.py:43`.

Outro caso que excede a garantia é chamado de módulo de auditoria gravado aberto: o queryset aceita pelo status, mas a natureza produz verbo `None`; o fio também será `chamado`, embora “decidir” não seja o verbo desse chamado. Fontes: `app/chamados/services/painel.py:187`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/juizes.py:1181`.

Censo da chave `fila`:

| Papel | Local |
|---|---|
| Escritor de produção | `app/chamados/juizes.py:1224` |
| Origem do valor | `app/chamados/juizes.py:1216`, alimentada em `app/chamados/juizes.py:1146` |
| Leitor de produção | `app/folha/fora_da_folha.py:199`; interpolação em `app/folha/fora_da_folha.py:200` |
| Teste compara dict inteiro, porta_fila | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138` |
| Teste compara dict inteiro, porta_pergunta | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:145` |
| Teste compara retorno inteiro singular/lote | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:163` |
| Teste novo injeta `fila` falsa | `app/folha/tests/test_haiku_fora_da_folha.py:125` |
| Teste novo verifica nomes | `app/folha/tests/test_haiku_fora_da_folha.py:136` |
| Vigia lê somente `verbos`/`rota` | `app/chamados/services/vigia_fio.py:596` |

As combinações acima são contraexemplos permitidos pelo código; existência delas em produção [nao verificado]. O caminho citado nas docstrings como `chamados/painel.py` corresponde na cópia ao serviço `app/chamados/services/painel.py:196`.

## 31/10 02:28 · sessao 919d5ab1e0d0 · - · - · origem=Ronald (tmux)

**Bloqueia: drawer mantém o “N” literal e diverge de `o_que_segura`.** A fonte contém `chamado N na fila, ou a pergunta viva...` (`app/folha/fora_da_folha.py:43`); `o_que_segura` escolhe a rota e substitui o trecho pelos IDs de `fila` ou pela pergunta (`app/folha/fora_da_folha.py:198–202`). O drawer lê `DESTRAVA` literalmente pelo fio gravado (`app/folha/retrato_folha.py:1057–1069`), e o template exibe esse valor (`app/templates/folha/_retrato_o_que_fazer.html:33–38`). O contrato declara “N literal na tela” um bug de rótulo (`app/docs/CORTES.md:353`). Ocorrência em produção: [nao verificado].

O lavrador grava o fio em `dono`, sem `fila` nem `rota` (`app/folha/retrato_folha.py:1177–1188`); a view passa essa linha a `o_que_fazer` (`app/colaboradores/views.py:980–985`). Os testes perpetuam o texto literal, inclusive para `chamado` (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:114–120`; `app/folha/tests/test_haiku_fora_da_folha.py:484`).

| Valor | Escritor | Leitores |
|---|---|---|
| `fila` | Retorno de `porta_alcancavel_do_dia`; `_fila` vem do mapa alimentado por `qs_na_fila` (`app/chamados/juizes.py:1144–1147`, `1216`, `1224`) | Leitor funcional direto: `o_que_segura` (`app/folha/fora_da_folha.py:199–202`). Testes de dict inteiro: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138–139`, `145–146`. Mock do teste novo: `app/folha/tests/test_haiku_fora_da_folha.py:125–139`. |
| `GRUPOS['C']` | Literal (`app/folha/fora_da_folha.py:36`) | Tooltip `ajuda` (`app/folha/retrato_folha.py:70–72`), produzido como `title` (`825`) e exibido no partial da barra (`app/templates/core/_barra_gestao.html:67`; inclusão em `app/templates/folha/_retrato_barra.html:122`). `fora_da_folha` lê apenas as chaves (`app/folha/fora_da_folha.py:139`). |
| `DESTRAVA['chamado']` | Literal (`app/folha/fora_da_folha.py:43–44`) | Ferramenta (`197–202`), drawer (`app/folha/retrato_folha.py:1069`) e template acima. Testes: `app/folha/tests/test_retrato_folha_drawer.py:224–229`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`, `app/folha/tests/test_haiku_fora_da_folha.py:484`. |

Outros leitores de `GRUPOS`: testes de chaves (`app/folha/tests/test_retrato_folha_quadros.py:154`), de propagação da prosa (`1276`, `1285`) e teste novo (`app/folha/tests/test_haiku_fora_da_folha.py:122–123`).

O vigia lê datas do universo da folha e `verbos`/`rota` do juiz; não consome a prosa nem a nova `fila` nesses caminhos (`app/chamados/services/vigia_fio.py:549–551`, `596–601`). Pautas lê o fio por `_alimentar`/`_fio_do_dia`, sem consumir a prosa (`app/ponto/services/pautas_dos_discordantes.py:60`, `74–77`).

## 31/10 00:53 · sessao 5bb56cea7746 · O261-E-F1-CLASSE-4 · verificador · origem=Code

1. **Os dois laços explícitos obedecem ao C8, mas isso não garante a ordem de todos os fechamentos do dia.** O imediato ordena antes de chamar `reconciliar()`; o adiado também. Havendo um não protestante com fio, o delta envia todos os não protestantes juntos para os adiados. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`)

   `sobrevivente_por_ultimo` põe por último o escolhido por `cobranca_viva_do_dia`, que busca o mais novo por `-criado_em, -pk`; preserva a ordem dos demais. (`app/chamados/juizes.py:1627`, `app/chamados/juizes.py:1645`)

   **Sim, existem fechamentos fora dessa ordenação.** Antes do laço adiado, E2 pode fechar uma disputa, cujo signal reconcilia o chamado-pai sem passar pelo ordenador. Além disso, os adiados continuam em `vivos`: o ramo `FIO-DUP` pode reconciliar um deles ainda no primeiro corpo, também sem C8. (`app/ponto/services/cartorio.py:753`, `app/chamados/reconciliador.py:635`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1329`)

2. **Em trabalho, o delta também desloca o sem fio para o lote adiado**, desde que ambos tenham os próprios marcos acesos: ambos não protestam e basta um deles estar em `_com_fio`. O sem fio deixa de tentar fechar em `_porm` antes da lavra e passa normalmente a tentar depois da lavra/E2, **antes da segunda execução do corpo**; o com fio já seguia esse caminho. (`app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1218`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:766`)

   O delta não acrescenta veto: marcos acesos regularizam a premissa e o gate aceita essa regularização. A tentativa no laço pode ser um no-op se E2/signal já fechou o chamado. A ressalva é o sem fio composto de perguntas duplicatas mortas: ele ainda pode fechar pelo `FIO-DUP` no primeiro corpo; sem perguntas, esse bloco o pula. (`app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`)

   Nos testes conferidos, não encontrei exigência de que esse par misto feche em etapas distintas: o teste do evento verifica fechamento no mesmo evento; o v5 usa um único adiado e verifica a ordem das datas; o teste de ressuscita verifica resultado e contadores com um chamado. Ausência dessa dependência em toda a suíte: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:409`)

3. **Sim, a fixture produz o velho com fio, inclusive pela leitura de `pergunta_viva`.** Ela cria a pergunta sem informar `celula`, cuja FK aceita nulo. Em folga, `classificar_falta` retorna sem falta; porém C6 continua até `materializada_em is None` e retorna `True`. `fio_vivo` consulta esse C6, exatamente como `_com_fio`. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:194`, `app/chamados/models.py:2150`, `app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`, `app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`)

   Essa pergunta **não é absolvida pelo E2**, que exige célula associada. Com o delta, o velho tenta primeiro, fecha por dedup e reata a pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:418`)

   **Pelo contrafactual do delta fornecido, as asserções finais ficam vermelhas sem a alteração de `_adiar`:** o novo sem fio fecharia imediatamente apoiado no velho; depois o velho ficaria sem irmão, com a premissa da véspera ainda viva, e teria o fechamento recusado. (`app/ponto/services/cartorio.py:1225`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`, `app/chamados/juizes.py:424`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:200`)

4. **Sim, por leitura, `[RECONCILIA-MARCO] ` é o caminho nas duas ordens da fixture anterior.** Em `trabalha=False`, nenhum protesta; sem fio, seguem para `_porm`, ordenado pelo C8. O velho fecha por dedup apoiado no novo, produzindo a linha nesse bloco. O B5.3b depende dos códigos de cobrança, cujo insumo agora recebe zero chamados protestantes. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:137`, `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1231`, `app/ponto/services/cartorio.py:1252`, `app/ponto/supra_juiz.py:441`)

Execução dos testes e da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:53 · sessao 7ab06b1c856c · - · - · origem=Ronald (tmux)

- Em trabalho, `marcos_acesos=True` faz ambos não protestarem; havendo um com fio, `_adiar` recebe ambos. O sem fio deixa de tentar `_porm` no primeiro corpo e normalmente tenta fechar após lavra/E2, ainda antes do segundo corpo; o com fio já seguia esse caminho. `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:756`, `app/ponto/services/cartorio.py:766`.

- O delta não acrescenta veto ao fechamento: marcos acesos regularizam a premissa, e o gate aceita a premissa regularizada. A tentativa pode resultar em no-op se E2/signal já fechou o chamado. `app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/reconciliador.py:638`, `app/chamados/signals.py:42`, `app/chamados/models.py:615`.

- Ressalva concreta à ordem: adiados continuam em `vivos`; se o sem fio possui somente perguntas `VIA_DUPLICATA`, `FIO-DUP` tenta reconciliá-lo ainda no primeiro corpo, sem C8. Sem perguntas, esse bloco pula o chamado. `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1321`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`.

- Nos testes conferidos, nenhum exige que o par misto trabalho/com fio/sem fio feche em corpos distintos: o teste do evento verifica fechamento no mesmo evento e ausência de terminal com pergunta viva; o v5 usa um único adiado e verifica ordem das datas; o teste de ressuscita usa um chamado e verifica resultado/contadores, pelo caminho B5.3b. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:416`. Ausência dessa dependência em toda a suíte: [não verificado].

## 31/10 00:52 · sessao 5b464716b57a · - · - · origem=Ronald (tmux)

**3. Sim, por leitura estática a fixture alcança o lote misto e distingue o delta.**

- O velho recebe a pergunta via `objects.create(... chamado_destino=velho, data_evento=DIA)`, sem `celula`; a FK permite nulo. A associação automática à célula pertence a `abrir()`, que a fixture não chama. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:193`, `app/chamados/models.py:2150`, `app/chamados/models.py:2363`)
- `trabalha=False` faz `classificar_falta()` retornar `(None, None, None)`, mas isso apenas impede o retorno antecipado `True` naquele degrau de C6. Como a pergunta está sem célula e sem materialização, C6 chega a `materializada_em is None` e retorna `True`. Portanto ela está viva mesmo no dia de folga. (`app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`)
- `fio_vivo` consulta `pergunta_viva`; `_com_fio` consulta `fio_vivo` nas resolvedoras do chamado. Assim, o velho entra em `_com_fio`, enquanto o novo não recebeu pergunta. (`app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:194`)
- O E2 não absolve essa pergunta da fixture: seu filtro exige `celula__isnull=False`. O fechamento do velho pode ocorrer pela exceção de dedup com reata da pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/models.py:702`, `app/chamados/models.py:371`, `app/chamados/models.py:418`)
- Com o delta, ambos vão ao lote adiado e C8 põe o novo por último. Sem a alteração de `_adiar` da volta 1, o novo sem fio seria reconciliado imediatamente sozinho, apoiado no velho; depois o velho ficaria sem irmão e seria recusado pela premissa ainda viva da véspera. As expectativas finais ficariam invertidas: velho vivo e novo resolvido. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/chamados/juizes.py:424`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`)

Resultado efetivo de execução do teste ou da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:46 · sessao 9bae963174b4 · O261-E-F1-CLASSE-4 · verificador · origem=Code

O diff `HEAD × HEAD~1` ficou **[nao verificado]**: `git show HEAD` falhou porque a cópia entregue não contém repositório Git. Abaixo estão conclusões sobre o código atual; atribuí-las à introdução neste commit seria **[nao verificado]**. Ocorrência e quantidade em produção também são **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha` | Porta da célula: regeneração, reversão e geração (`app/ponto/portas/celula.py:231`, `:260`, `:532`, `:612`). | Juiz `EscalaColaborador.eh_dia_trabalho` (`app/escala/models.py:1072`); cartório (`app/ponto/services/cartorio.py:1156`); premissa dos chamados (`app/chamados/juizes.py:389`); leitor da grade (`app/escala/services/leitor_celula.py:344`). |
| `dna['marcos']` | Conteúdo produzido por `montar_dna`, somente quando `trabalha is True` (`app/ponto/management/commands/gerar_celulas.py:79`, `:83`, `:100`); persistido pelas operações da porta (`app/ponto/portas/celula.py:260`, `:530`, `:614`). | Juiz dos marcos do vínculo (`app/escala/models.py:1099`); produtor das lâmpadas (`app/ponto/services/cartorio.py:434`); juiz do marco perguntado (`app/chamados/juizes.py:849`). |
| `ata['lampadas']` | Produzida por `lampadas_do_dia`/`ata_do_dia`; teto aplicado por `_ata_ate_o_teto` (`app/ponto/services/cartorio.py:418`, `:555`, `:703`). Persistência por `lavrar_veredito` e `carimbar` (`app/ponto/portas/celula.py:690`, `:728`). | Lastro: acesos, inexistentes, apagados e pendentes (`app/ponto/services/lastro.py:89`, `:115`, `:128`, `:148`); pergunta (`app/chamados/juizes.py:866`); grade (`app/escala/services/leitor_celula.py:354`); folha (`app/folha/services/sem_furo.py:22`). |
| Veredito `cobrado` | Escolhido pelo cartório quando há fio que ainda protesta (`app/ponto/services/cartorio.py:1195`); persistido pela porta (`app/ponto/portas/celula.py:692`). | Pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`); lido pelo C6 (`app/chamados/juizes.py:1056`), E2 (`app/chamados/reconciliador.py:598`), fábrica (`app/chamados/services/fabrica_da_celula.py:86`) e retenção da folha (`app/folha/export.py:1215`). |
| Pergunta materializada/retratada | Neste caminho, E2 escreve `materializada_em`, autor e `VIA_CELULA_ABSOLVEU` (`app/chamados/reconciliador.py:620`). Há outros escritores efetivos: materializador, emissão, dedup e portas do modelo (`app/chamados/services/materializacao.py:509`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/fio.py:894`; `app/chamados/models.py:2456`). | C6 lê carimbo, via, resposta e célula (`app/chamados/juizes.py:1020`, `:1044`, `:1050`, `:1080`); `estado` delega a ele (`app/chamados/models.py:2435`). Desfazer carimbo também consulta C6 (`app/chamados/services/relavra_via.py:227`). |
| Estado de `ChamadoColaborador` | Transições persistem por `_gravar_estado` (`app/chamados/models.py:222`, `:279`); nascimentos têm portas próprias (`app/chamados/models.py:1039`). | Motor decide transições (`app/chamados/models.py:637`); cartório seleciona vivos (`app/ponto/services/cartorio.py:1089`); C8 escolhe cobrança sobrevivente (`app/chamados/juizes.py:1625`); C6 também considera o estado do destino (`app/chamados/models.py:313`). |

O modelo desses caminhos chama-se **`PerguntaDisputa`**, com estado `PENDENTE`; E2 “retrata” pelo carimbo e pela via de resolução (`app/chamados/models.py:2142`, `:2310`; `app/chamados/reconciliador.py:620`).

**Sim, há leitores com critérios diferentes:**

- O cartório exclui do protesto `trabalha=False` ou cobrança com todos os próprios marcos acesos; `dia_sem_trabalho` considera também `trabalha=None` e qualquer `ata.tipo_dia` diferente de trabalho. Os critérios divergem (`app/ponto/services/cartorio.py:1156`; `app/chamados/juizes.py:389`).
- Os comandos `supra_juiz` e `emitir_furo_retroativo` continuam entregando **todos os chamados vivos** ao classificador, enquanto o cartório entrega `_protestam`. Portanto, a mesma classificação recebe insumos diferentes (`app/ponto/management/commands/supra_juiz.py:207`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1159`).
- `q_vivas` mantém toda célula provisória como candidata, sem a exceção pelo próprio marco aceso. Isso é uma seleção mais larga, permitida pelo contrato explícito de **continência**, com julgamento posterior; isoladamente, não é defeito (`app/chamados/models.py:2577`, `:2556`).
- A absolvição de **chamados de turno** ainda exige ausência de qualquer marco pendente; E2 agora individualiza a pergunta. São universos diferentes: dia do chamado versus marco perguntado (`app/chamados/reconciliador.py:983`, `:610`).

2. **Onde o desenho falha ou permanece incompleto**

**A partição entre imediatos e adiados quebra a escolha do sobrevivente C8.** Considere dois `turno_aberto` no dia de folga, com véspera furada: o mais velho tem pergunta viva sob o veredito anterior acusador; o mais novo não tem fio. Esse módulo pertence ao universo lido pelo cartório, e C6 pode manter a pergunta viva pelo veredito anterior (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/juizes.py:1054`).

O caminho fica assim:

- `_com_fio` é calculado antes da nova lavra; o velho vai para `_adiar`, o novo para `_porm` (`app/ponto/services/cartorio.py:1094`, `:1214`, `:1222`).
- Cada subconjunto tem um elemento; o ordenador retorna sem consultar C8. O novo fecha por dedup apoiado no velho ainda vivo (`app/chamados/juizes.py:1645`; `app/chamados/models.py:702`).
- Depois da lavra e de E2, o velho é tentado sozinho. Para `turno_aberto`, a folga não regulariza diretamente: a premissa consulta a véspera furada. Sem outro vivo, o fechamento é recusado (`app/ponto/services/cartorio.py:753`, `:756`; `app/chamados/models.py:1225`; `app/chamados/juizes.py:424`; `app/chamados/models.py:721`).

**Resultado do caminho: novo fechado, velho vivo**, contrariando C8, que escolhe o mais novo por `-criado_em, -pk`. Ordenar separadamente os subconjuntos não ordena o lote inteiro. Reprodução com banco: **[nao verificado]** (`app/chamados/juizes.py:1627`; `app/ponto/services/cartorio.py:1222`, `:756`).

**Há leitura obsoleta no memo do C6.** A chave não inclui ata nem DNA, embora a decisão dependa deles. Confirmei com funções extraídas em memória: pergunta provisória inicialmente viva; depois, alterando só a ata para acender seu marco, a mesma instância continua retornando `True`, enquanto `_pergunta_viva_calc` retorna `False`. Reprodução integrada com banco: **[nao verificado]** (`app/chamados/juizes.py:767`, `:793`, `:849`, `:866`, `:1068`).

**O relógio não está inteiramente na borda recebida.** `julgar_celula(agora=...)` calcula `hoje` pelo relógio real e compara o fim do turno com outro `now()`, antes de encaminhar o `agora` fornecido. Assim, o instante injetado não governa todas as guardas (`app/ponto/services/cartorio.py:1597`, `:1618`, `:1621`).

**Não constatei segundo escritor persistente novo nesses caminhos.** O cartório continua chamando a porta da célula; a sobreposição de ata em `celula_do_dia_agora` é somente em memória. E2 já é escritor sancionado no censo de `materializada_em`; esse campo possui escritores plurais declarados (`app/ponto/services/cartorio.py:1470`, `:1485`; `app/chamados/juizes.py:382`; `app/core/tests/test_contract_escritores_estado.py:83`).

**Há risco de ordem inversa entre duas travas de chamados.** `reconciliar` trava o próprio chamado; a reata escolhe outro vivo e a porta de costura trava esse destino. Duas transações A→B e B→A podem solicitar as linhas em ordem inversa; não há ordenação conjunta por PK nesse caminho. Deadlock reproduzido: **[nao verificado]** (`app/chamados/models.py:672`, `:371`, `:418`; `app/chamados/services/fio.py:762`).

**Idempotência não é uniforme.** O fechamento sequencial relê o banco e o motor impede novo `RECONCILIAR` sobre resolvido; a recusa de fechamento deduplica a trilha. Porém `reabrir` recusado por premissa regularizada ou admin sem fato novo escreve `reabertura_recusada` novamente em cada chamada: mesmo estado, várias linhas (`app/chamados/models.py:636`, `:378`, `:1335`, `:1341`; `app/chamados/catalogo/motor.py:111`).

**Existe caminho de vai-e-volta por assimetria dia/turno.** O fechamento por dedup aceita outro vivo do **dia**; a reabertura procura irmão do **turno**, exigindo `entrada_timestamp`. O teste dos dois chamados em folga omite essa chave. Com véspera furada e vínculo sem veto, o fechado por sistema pode reabrir sem fato novo e voltar a fechar por dedup; a exigência de fato novo é específica do fechamento administrativo. Reprodução integrada: **[nao verificado]** (`app/chamados/models.py:702`, `:1283`, `:1294`, `:1373`; `app/chamados/juizes.py:1674`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:131`, `:145`).

3. **Chamado legítimo fechado e pergunta provisória sem batida**

**O gate atual permite fechar chamado legítimo em ausência parcial.** A grade mantém `tipo_dia='ausencia'`, marca cobertura parcial e conserva as células faltantes; o classificador reconhece isso como dia de cobrança. Entretanto, `dia_sem_trabalho` responde verdadeiro para esse tipo, e `premissa_regularizada` aceita a resposta **antes de consultar o marco apagado** (`app/escala/utils.py:1180`, `:1190`, `:1195`; `app/ponto/supra_juiz.py:279`; `app/chamados/juizes.py:389`; `app/chamados/models.py:1225`).

Logo, se `reconciliar()` for chamado para esse furo e não houver pergunta viva que retenha o fechamento, ele pode ser resolvido mesmo com marco realmente faltante; a mesma premissa depois impede reabrir. É defeito do gate atual, sem prova de introdução pelo HEAD ou ocorrência no dado de produção: **[nao verificado]** (`app/chamados/models.py:646`, `:649`, `:618`, `:1289`).

**Nos ramos `_adiar`/`_porm`, trabalho verdadeiro com próprio marco apagado ou pendente continua protestando.** `marcos_acesos` não retorna verdadeiro nesse caso, portanto esses conjuntos não o selecionam somente pela mudança examinada (`app/ponto/services/lastro.py:175`, `:179`; `app/ponto/services/cartorio.py:1156`, `:1214`, `:1222`).

**Pergunta provisória cujo próprio marco continua sem batida permanece protegida na nova condição.** Lâmpada `acesa=None`/`False` não prova o fato; C6 e E2 exigem literalmente `is True` para dispensar o teto. E2 ainda consulta `pergunta_viva` antes de carimbar (`app/chamados/juizes.py:869`, `:1068`; `app/chamados/reconciliador.py:610`, `:616`).

4. **Ordem, conjunto reconciliado e testes existentes** [nao verificado]

**Há um teste existente cuja expectativa de caminho precisa mudar:** ele exige `[RECONCILIA] `, mas os chamados de folga agora entram em `_porm`, que registra `[RECONCILIA-MARCO]`. As expectativas de estado e sobrevivente desse fixture continuam pertinentes (`app/ponto/tests/test_fechar3_troca_escala_gate.py:153`, `:154`, `:158`; `app/ponto/services/cartorio.py:1222`, `:1228`).

**Esse teste não cobre a falha da partição:** cria ambos sem perguntas. Falta a variante “velho com fio, novo sem fio”, que percorre os dois subconjuntos e pode preservar o velho; isso pede cobertura adicional e correção da ordem global, não trocar a expectativa para aceitar o sobrevivente errado (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`; `app/ponto/services/cartorio.py:1214`, `:1222`, `:756`; `app/chamados/juizes.py:1627`).

**O teste da segunda passada continua compatível:** a lista é ordenada por data antes do corpo; seu chamado falso único não provoca consulta C8, pois o ordenador retorna para lote menor que dois (`app/ponto/services/cartorio.py:766`; `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `:183`; `app/chamados/juizes.py:1645`).

**Os testes antigos de provisória sem batida devem continuar exigindo pergunta viva e nenhum carimbo.** Eles usam justamente o próprio marco `acesa=None, pendente=True`, que não satisfaz a exceção nova (`app/chamados/tests/test_o261_pergunta_viva_teto.py:54`; `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:27`, `:64`). Resultado da suíte completa: **[nao verificado]**.

## 31/10 00:45 · sessao 6a0aa9eab820 · - · - · origem=Ronald (tmux)

A partição quebra a escolha do sobrevivente C8 no cenário proposto, por leitura do caminho; execução com banco **[não verificado]**.

1. `turno_aberto` pertence a `FURO_MODULES` (`app/chamados/catalogo/furo_modulos.py:15`) e ao universo do gate (`app/chamados/catalogo/modulos.py:237`, `app/chamados/models.py:1187`).
2. A pergunta do velho pode estar viva com `trabalha=False` e veredito anterior acusador: C6 retorna True pelo veredito, sem consultar `trabalha` (`app/chamados/juizes.py:1054`). `_com_fio` é calculado antes da nova lavra (`app/ponto/services/cartorio.py:1094`).
3. Na folga, ambos deixam de protestar (`app/ponto/services/cartorio.py:1155`). O velho com fio vai para `_adiar` (`app/ponto/services/cartorio.py:1214`); o novo sem fio vai para `_porm` (`app/ponto/services/cartorio.py:1222`).
4. Cada subconjunto contém um elemento: `sobrevivente_por_ultimo` retorna sem consultar C8 (`app/chamados/juizes.py:1645`). O novo fecha por dedup, apoiado no velho ainda vivo (`app/chamados/models.py:702`).
5. A lavra grava o veredito da folga (`app/ponto/services/cartorio.py:1470`); E2 pode então carimbar a pergunta do velho, pois a célula não acusa e não tem pendentes (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:610`, `app/chamados/juizes.py:1068`).
6. O laço adiado tenta fechar o velho sozinho (`app/ponto/services/cartorio.py:755`). Para `turno_aberto`, folga não regulariza diretamente a premissa (`app/chamados/models.py:1225`); ela consulta a véspera furada (`app/chamados/juizes.py:424`). Sem outro vivo, o gate recusa (`app/chamados/models.py:702`, `app/chamados/models.py:721`).

**Resultado: novo fechado, velho vivo**, contrariando o C8, que escolhe o mais novo por `-criado_em, -pk` (`app/chamados/juizes.py:1627`). O teste existente de sobrevivência cria ambos sem perguntas e não cobre a partição por fio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`).

## 31/10 00:44 · sessao edb37687a75c · - · - · origem=Ronald (tmux)

Achados conferidos, somente por leitura:

- **Chamado legítimo pode fechar por ausência parcial.** A grade conserva `tipo_dia='ausencia'`, `cobertura_parcial=True` e os marcos faltantes em `app/escala/utils.py:1180` e `app/escala/utils.py:1184`; o suprajuiz considera essa combinação dia de cobrança em `app/ponto/supra_juiz.py:274`. Entretanto, `dia_sem_trabalho` responde True para qualquer `ata.tipo_dia != 'trabalho'`, mesmo com `cel.trabalha=True` (`app/chamados/juizes.py:386`). `premissa_regularizada` aceita essa resposta antes de consultar as lâmpadas (`app/chamados/models.py:1220`). Assim, um chamado de furo sem pergunta viva ancorada pode passar pelo gate e ser reconciliado (`app/chamados/models.py:646`, `app/chamados/models.py:649`, `app/chamados/models.py:618`); depois a mesma premissa impede sua reabertura (`app/chamados/models.py:1289`). A ata não conserva `cobertura_parcial` (`app/ponto/services/cartorio.py:557`). Introdução desse defeito no HEAD: [não verificado].

- **Provisória sem a batida continua protegida no novo E2.** O carimbo exige ausência de pendentes ou prova positiva do próprio marco; depois ainda consulta C6 (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:616`). `fato_do_marco_no_espelho` exige o marco do DNA, lâmpadas correspondentes e todas acesas (`app/chamados/juizes.py:842`, `app/chamados/juizes.py:863`, `app/chamados/juizes.py:869`). Uma lâmpada própria com `acesa=None, pendente=True` não satisfaz isso. Os testes de provisória sem batida seguem coerentes (`app/chamados/tests/test_o261_pergunta_viva_teto.py:52`, `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`).

- **A irmã dos chamados conserva o teto global.** `_absolvido_pela_celula` continua recusando qualquer célula com marcos pendentes (`app/chamados/reconciliador.py:981`). O novo E2 individualiza a pergunta, enquanto essa irmã examina o dia inteiro (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:983`).

- **Não nasceu escritor novo de estado de chamado nesse caminho.** `_retratar` chama `ch.reconciliar` (`app/chamados/reconciliador.py:524`); `reconciliar` usa o motor e `_gravar_estado` (`app/chamados/models.py:614`). A escrita persistida de `status_local` está em `app/chamados/models.py:222`, e a trilha precede o save no mesmo atomic (`app/chamados/models.py:274`). Os nascimentos são exceções próprias (`app/chamados/models.py:890`, `app/chamados/models.py:1039`).

- **E2 já é escritor sancionado de materialização.** A gravação continua em `app/chamados/reconciliador.py:620`; o censo declara esse arquivo em `app/core/tests/test_contract_escritores_estado.py:83`. Há pluralidade de escritores preexistente; não é correto dizer que todo `materializada_em` tem uma porta única global (`app/core/tests/test_contract_escritores_estado.py:83`).

- **Idempotência sequencial do fechamento:** a segunda chamada relê o banco e o motor recusa novo RECONCILIAR sobre resolvido (`app/chamados/models.py:636`, `app/chamados/catalogo/motor.py:111`). A recusa de fechamento também deduplica trilha (`app/chamados/models.py:378`). **Contraexemplo atual ao contrato “2×, uma linha”**: `reabrir` recusado por admin sem fato novo ou premissa regularizada grava `reabertura_recusada` sem dedup a cada chamada (`app/chamados/models.py:1335`, `app/chamados/models.py:1341`). Introdução no HEAD: [não verificado].

- **Risco estático de ciclo entre duas travas de chamados:** reconciliar trava primeiro o próprio chamado (`app/chamados/models.py:669`), escolhe outro vivo para receber a pergunta (`app/chamados/models.py:371`) e a costura trava esse destino (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`). C8 aceita o outro excluindo o próprio (`app/chamados/juizes.py:1625`). Duas transações A→B e B→A podem portanto pedir as duas linhas em ordem inversa; reprodução de deadlock: [não verificado]. Os testes atuais conferem trava própria antes do plano e trava do destino isoladamente (`app/chamados/tests/test_o261e_f1_codex_v5.py:25`, `app/chamados/tests/test_o261e_f1_codex_v5.py:65`).

- **SQL com regra global própria:** `q_vivas` ainda usa qualquer lâmpada pendente para manter candidata (`app/chamados/models.py:2575`), enquanto C6 passou a absolver pelo próprio marco (`app/chamados/juizes.py:1068`). Isso amplia candidatos mortos, porém o contrato documentado é continência, com julgamento posterior, e não igualdade (`app/chamados/models.py:2556`). A property `.viva` delega ao C6 (`app/chamados/models.py:2597`).

- **Ordenação:** os adiados passam agora por `sobrevivente_por_ultimo` (`app/ponto/services/cartorio.py:754`); o fechamento imediato também (`app/ponto/services/cartorio.py:1221`). Esse ordenador consulta C8 e põe o sobrevivente por último, preservando a ordem dos demais (`app/chamados/juizes.py:1644`). A segunda passada continua ordenada por data (`app/ponto/services/cartorio.py:766`), como exige o teste existente (`app/chamados/tests/test_o261e_f1_codex_v5.py:164`).

_854 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
