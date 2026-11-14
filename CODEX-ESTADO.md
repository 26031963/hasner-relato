# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 19:19** · publicado em **14/11/2026 19:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 19:19 · sessao e4253e35603f · - · - · origem=Ronald (tmux)

Achados para perguntas 2/3/8:

1. **PROCEDENTE — carga e consulta não têm o mesmo universo.** A carga lê `colaborador_id`, `conversa=False` e `data_evento` na janela (`app/chamados/services/tarefa_do_dia.py:395`); o app lê `disputa__colaborador` e `q_vivas`, sem recorte de data/conversa (`app/chamados/services/pergunta_no_app.py:52`). A identidade dos dois colaboradores é exigida na porta de escrita (`app/chamados/models.py:3573`), portanto divergência nesse campo em dados válidos não está demonstrada [nao verificado].

2. **PROCEDENTE — mesmo recorte e mesmo `agora` histórico também podem divergir.** `q_vivas` exige `validada_em IS NULL` no estado atual (`app/chamados/models.py:3468`), enquanto C6 considera inexistente a validação posterior a `agora` (`app/chamados/juizes.py:1387`). A carga não aplica `q_vivas` (`app/chamados/services/tarefa_do_dia.py:395`). Pergunta sem resposta, disputa aberta e célula acusadora, validada depois do instante consultado, pode estar nas pendentes da carga e ausente de `consulta_do_app(..., agora=agora)` (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/juizes.py:1450`; `app/chamados/models.py:3468`).

3. **NÃO PROCEDENTE — predicado local duplicado de partição.** Ambos usam `particao_do_app`: sem resposta e disputa aberta (`app/chamados/services/pergunta_no_app.py:73`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:514`). Contudo, a partição lê `respondida_em` e `fechada_em` atuais, sem projeção temporal; resposta futura carimbada já manda a pergunta ao histórico em ambos (`app/chamados/services/pergunta_no_app.py:81`). C6 projeta resposta futura para inexistente, criando duas semânticas temporais dentro da composição (`app/chamados/juizes.py:1390`).

4. **PROCEDENTE — contraexemplo sintético reproduzido sem banco e sem arquivos.** Executei corpos reais por AST usando os helpers existentes (`app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e3_portas_apoio.py:18`). Hipótese: pergunta 303 de `orfao_14h`, sem resposta/validação/materialização, disputa aberta, célula `furo`, marco `fim` aceso e vencido, sem tranca, fila vazia. C6 retornou viva e a consulta/partição retornou pendente 303; a tarefa publicou `fio='nada', rota=None`, com instância `razao='aceso', pergunta_id=None, acoes=[]`. C6 mantém viva a pergunta sem materialização sobre célula acusadora (`app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1450`); a tarefa pula a lâmpada acesa antes de associar a pergunta (`app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:321`); `_portas` exige simultaneamente pendência e instância RESPONDER (`app/chamados/services/tarefa_do_dia.py:610`). Incidência em produção [nao verificado].

   O mesmo bloqueio estrutural pode ocorrer em isento, sem célula/ata, sem trabalho, marco sem luz/teto, ausência ou motivo fora do mapa: esses ramos removem/excluem RESPONDER antes da publicação (`app/chamados/services/tarefa_do_dia.py:203`; `app/chamados/services/tarefa_do_dia.py:219`; `app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:287`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:333`). Cada classe exige confirmar C6 vivo; não são ocorrências de produção demonstradas [nao verificado].

5. **NÃO PROCEDENTE — cópia do corpo C6 na carga.** A carga chama o juiz registrado com `hoje`, `agora` e carregador, reutilizando resultado para perguntas candidatas já julgadas (`app/chamados/services/tarefa_do_dia.py:518`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:523`). C6 encaminha carga ao corpo único (`app/chamados/juizes.py:888`), cuja tranca lê a alimentação da carga (`app/chamados/juizes.py:1405`; `app/chamados/services/carga_pergunta_viva.py:146`).

6. **NÃO PROCEDENTE — C17 ativo como juiz paralelo nos chamadores de produção rastreados.** A busca de chamadas encontrou a definição e somente o instrumento de comparação `diff_e3_sombra`, que o lê como “antes” (`app/chamados/juizes.py:1664`; `app/chamados/management/commands/diff_e3_sombra.py:127`). Seu corpo antigo continua aplicando `cobrar -> colab` (`app/chamados/juizes.py:1650`; `app/chamados/juizes.py:1709`); `_portas` exclui cobrar antes de consultar essa tabela (`app/chamados/services/tarefa_do_dia.py:618`).

7. **NÃO PROCEDENTE — relógio novo interno no caminho tarefa/carga examinado.** O instante da carga segue ao C6 e ao alimentador (`app/chamados/services/tarefa_do_dia.py:519`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:565`), e a casca recusa instante nulo ou carga de instante diferente (`app/chamados/services/tarefa_do_dia.py:657`). A API chama a consulta sem instante, conservando C6 atual e seu fallback de calendário (`app/api/views.py:994`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`). Igualdade de relógio com uma carga externa, portanto, não é garantida nesses leitores (`app/api/views.py:2534`).

8. **PROCEDENTE — docstring da carga contradiz implementação.** Promete “uma consulta do app por colab (memo da carga)” (`app/chamados/services/tarefa_do_dia.py:362`), mas implementa uma partição em lote seguida do carregador C6 (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/services/tarefa_do_dia.py:518`). A afirmação de igualdade integral com o universo do app também não vale sem as ressalvas acima (`app/chamados/services/tarefa_do_dia.py:512`).

**PERGUNTA:** a conversa pendente do app participa de algum colab-dia? O app admite conversa, mas o juiz de dia dela devolve ausência de dia de turno e a carga exclui conversa (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:114`; `app/chamados/services/tarefa_do_dia.py:396`). Não atribuí esse caso a um dia sem corte adicional [nao verificado].

## 14/11 19:19 · sessao 9a633ee7c38b · - · - · origem=Ronald (tmux)

**Pergunta 1 — mapa dos campos e leitores.**

| Valor | Escritor/fornecedor | Leitores de produção |
|---|---|---|
| `fio`, `rota`, `fila`, `verbos` publicados | `_portas_da_tarefa`: computa `verbos`/`fila` em `app/chamados/services/tarefa_do_dia.py:608`, donos em `:615`, fio em `:622`, rota em `:629`, publica todos em `:635`. | Projeção `porta_da_tarefa` em `app/chamados/services/tarefa_do_dia.py:645`; leitores abaixo. |
| `donos` | **Não é campo da tarefa**: conjunto local, criado em `app/chamados/services/tarefa_do_dia.py:615`, acrescido em `:617` e `:621`, consumido para `fio` em `:622`; não aparece no `out.update` de `:635`. | Apenas a seleção de fio do próprio juiz, `app/chamados/services/tarefa_do_dia.py:622`. |
| `porta_da_tarefa` | Copia `rota`, `fila`, `verbos`, `fio`, `nao_sei`, ou retorna `None` quando `rota` é falsa, `app/chamados/services/tarefa_do_dia.py:645`. | `fora_da_folha`, `vigia_fio`, `furo_sem_canal`, `beco_em_lote` e instrumento sombra, endereços abaixo. |
| Pendentes/histórico | `particao_do_app`, `app/chamados/services/pergunta_no_app.py:79`; predicado único sem resposta e disputa aberta, `:81`; `consulta_do_app` fornece vivas em `:73`. | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514`. |
| Vivas do app | `perguntas_do_app` seleciona pelo colaborador e `q_vivas`, `app/chamados/services/pergunta_no_app.py:52`; C6 em `:61`. | Consulta `:73`; C17 `_no_app`, `app/chamados/juizes.py:1642`; ficha, `app/colaboradores/services/ficha.py:166`; diagnóstico `pergunta_no_app.py:208`, `:231`. |

**Leitores diretos da porta, com o que efetivamente usam:**

- Folha `_porta` chama projeção em `app/folha/fora_da_folha.py:70`; `_fio_da_porta` usa `porta['fio']` ou `FIO_SEM_PORTA` em `:85`; o agregado lê esse fio em `:193`; `o_que_segura` lê fio, fila e `nao_sei` em `:238`–`:248`.
- Vigia monta portas em `app/chamados/services/vigia_fio.py:649`, preserva `nao_sei` em `:656`, conta pela rota em `:662`, admin pelo fio em `:663`, publica rota/fio em `:664`.
- Furo lê presença e rota em `app/chamados/services/furo_sem_canal.py:97`–`:99`, preserva desconhecimento em `:100`–`:102`, conta ausências em `:104`, divide rotas em `:114`–`:116`.
- Beco exclui dia com qualquer porta em `app/escala/services/cadastro_realidade.py:469`; não traduz rota/fio.
- Pauta usa indiretamente `_fio_do_dia` e compara com `FIO_SEM_PORTA`, `app/ponto/services/pautas_dos_discordantes.py:79`–`:82`.
- Sombra projeta fio/fila/rota em `app/chamados/management/commands/diff_e3_sombra.py:49`–`:55`, imprime rota/fio/fila/verbos em `:143`–`:144`.
- E4b lê **instâncias e ações**, não os campos publicados, em `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`–`:107`.

**Pergunta 3 — segundo escritor publicado: NÃO PROCEDENTE nos leitores acima.** A publicação fica em `app/chamados/services/tarefa_do_dia.py:635`, e a projeção apenas copia em `:645`; folha lê em `app/folha/fora_da_folha.py:85`, vigia lê em `app/chamados/services/vigia_fio.py:662`, furo lê em `app/chamados/services/furo_sem_canal.py:99`, beco testa presença em `app/escala/services/cadastro_realidade.py:469`.

**C17 ainda existe, mas o chamado de produção encontrado é o instrumento sombra:** `app/chamados/management/commands/diff_e3_sombra.py:127`; sua definição e escrita antigas estão em `app/chamados/juizes.py:1664` e `:1709`. A tarefa reutiliza a autoridade `fio_pelos_verbos` em `app/chamados/services/tarefa_do_dia.py:620`–`:621`, excluindo residual cobrar antes em `:618`; tabela única em `app/chamados/juizes.py:1650`–`:1661`.

**Há outros “donos” derivados, com outra unidade — PROCEDENTE como diferença semântica, não prova de segundo juiz do colab-dia:**

- Ficha deriva dono **do chamado** diretamente do verbo: cobrar → colaborador, `app/colaboradores/services/ficha.py:159`–`:162`.
- Relatório classifica **disputa** como `AGUARDA_COLAB` por `aguarda_o_colaborador`, `app/chamados/relatorios_disputa.py:24`–`:28`; essa função usa C6 e `espera_o_texto_do_colaborador`, `app/chamados/services/fio.py:172`–`:174`; respondida sem texto ainda espera colaborador, `:187`.
- Portanto uma pergunta com `respondida_em` e texto vazio está no histórico da consulta (`app/chamados/services/pergunta_no_app.py:81`–`:84`), mas pode sustentar `AGUARDA_COLAB` no relatório (`app/chamados/services/fio.py:187`, `app/chamados/relatorios_disputa.py:26`). Não chamar esses rótulos de “fio da tarefa”.

**Pergunta 6 — semântica dos leitores.**

- `rota=None` implica `porta_da_tarefa=None`, independentemente de `fio`, por `app/chamados/services/tarefa_do_dia.py:645`; há exceção explícita `fio='sistema', rota=None` em `:634`.
- `porta_app` nasce quando colab tem porta e não há `portas_vivas`, `app/chamados/services/tarefa_do_dia.py:628`–`:634`; por isso não significa necessariamente fila vazia: o cobrar residual pode continuar em `fila`/`verbos`, calculados em `:608`–`:609`.
- `porta_fila`/`porta_pergunta` são escolhidas conforme a composição de portas, `app/chamados/services/tarefa_do_dia.py:632`; não identificam necessariamente a porta particular do dono vencedor: fio é escolhido antes em `:622`.
- Folha não restringe quais strings de rota aceita: só recebe a projeção e lê fio, `app/folha/fora_da_folha.py:70`, `:85`; beco só testa presença, `app/escala/services/cadastro_realidade.py:469`; furo reconhece as três rotas no contador, `app/chamados/services/furo_sem_canal.py:114`–`:116`; vigia conta dinamicamente a chave da rota em `app/chamados/services/vigia_fio.py:662` e inicializa app em `:577`.
- `nada` e `nao_sei` são distintos na tarefa, `app/chamados/services/tarefa_do_dia.py:623`; ambos ficam sem rota, `:629`–`:630`. Folha os colapsa para `FIO_SEM_PORTA='nada'` pela ausência de porta (`app/folha/fora_da_folha.py:85`, `app/chamados/juizes.py:1652`), mas `o_que_segura` preserva a flag `nao_sei` separadamente, `fora_da_folha.py:248`. Vigia e furo também preservam a flag, `vigia_fio.py:656`, `furo_sem_canal.py:100`–`:102`.

**Retrato, templates e PDF — detalhe importante para “sem porta explícito”.**

- Retrato é agregado por pessoa: lê `grupo` do `fora_da_folha` e grava `dono` por `_fio_cru`, `app/folha/retrato_folha.py:1158`–`:1190`; grupo D torna-se `ninguem`, `:177`–`:178`, `:368`. Leitor prefere o gravado, mas tem derivação legada por status/motivo sem grupo, `:428`–`:441`.
- `ninguem` vira **balde Admin**, `app/folha/retrato_folha.py:192`; se todos os dias forem atuais/futuros, pode virar **Hoje**, `:477`–`:481`. A UI rotula esses baldes em `:197`.
- Drawer mostra a frase SEM PORTA por `FIO_DO_CARTORIO` (`app/folha/retrato_folha.py:183`) e `DESTRAVA` (`:1068`–`:1069`; origem `app/folha/fora_da_folha.py:54`); template apenas renderiza os valores em `app/templates/folha/_retrato_o_que_fazer.html:33`–`:40`.
- Lista mostra LED já montado, `app/templates/folha/_retrato_lista.html:47`; PDF mostra o mesmo rótulo do LED em `app/folha/views_retrato.py:411`–`:413`. O PDF **não imprime a frase SEM PORTA** nesse registro: colunas em `:407`–`:418`.
- Vigia de retrato conta `ninguem` pelo gravado, `app/folha/vigia_retido_calado.py:46`–`:49`.

**PERGUNTA de lei:** a exigência nova “sem porta explícito” substitui também o agrupamento legado `ninguem → Admin/Hoje` da barra/PDF, ou admite o balde com identificação no drawer? O código mantém agrupamento em `app/folha/retrato_folha.py:192`, `:477`–`:481`, rótulo Admin em `:197`, PDF pelo LED em `app/folha/views_retrato.py:413`, e SEM PORTA apenas na frase do drawer em `app/folha/retrato_folha.py:1069`.

**Pergunta 7 — comentários que permanecem incompatíveis/incompletos.**

- **PROCEDENTE:** “TODO chamado VIVO na fila ... são publicadas” no cabeçalho, `app/chamados/services/tarefa_do_dia.py:27`–`:29`, e “CHAMADO VIVO NA FILA TEM PORTA”, `:580`; residual cobrar perde dono e rota em `:618`, `:629`–`:630`.
- **PROCEDENTE:** “ações exigíveis e chamados vivos ligados ao dia conservam suas portas”, `app/chamados/services/tarefa_do_dia.py:641`, não ressalva residual cobrar que não publica rota, `:618`, `:629`–`:630`.
- **PROCEDENTE:** `_fio_do_dia` documenta somente admin/colab/nada, `app/folha/fora_da_folha.py:74`, mas retorna qualquer fio publicado, `:85`, inclusive chamado do juiz, `app/chamados/services/tarefa_do_dia.py:622`.
- **PROCEDENTE:** retrato ainda descreve grupo D como “ninguém está esperando ninguém”, `app/folha/retrato_folha.py:361`–`:362`; nova frase diz ausência de ação alcançável, `app/folha/fora_da_folha.py:54`. Há chamado cobrar e ação interna possíveis sem porta, `app/chamados/services/tarefa_do_dia.py:618`–`:635`.
- **PROCEDENTE:** doc do beco diz `None = nenhuma ação ... por porta da fila`, `app/escala/services/cadastro_realidade.py:419`–`:420`; a projeção agora também publica app sem fila, `app/chamados/services/tarefa_do_dia.py:634`, e None também cobre sistema sem rota, `:614`, `:634`.

## 14/11 19:19 · sessao 42afa79ed43b · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **PROCEDENTE — bateria de mutantes incompleta.** `--todos` aborta no 12º registro, `sem_memo_por_colab`, porque o texto alvo já não existe; `carga_sem_relogio` também está obsoleto. Ambos ainda apontam para um teste removido. A validação do alvo ocorre antes do controle verde, portanto esses dois nem chegam a executar teste. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`; implementação atual em `app/chamados/services/tarefa_do_dia.py:514`. Execução nesta sessão: 19 registros morreram por assertion após controle verde; dois produziram `ValueError`. Resultado sem artefato persistido: [nao verificado em arquivo].

2. **PROCEDENTE — selo L129 não prova leitura efetiva da tarefa.** Ele proíbe imports/nomes e exige a presença dos imports autorizados, mas não exige chamadas nem consumo da resposta. Em mutação somente em memória, substituí todas as chamadas `carga_da_tarefa(...)` e `porta_da_tarefa(...)` por `None`, preservando imports: os três testes do selo passaram. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; o degrau apenas executa esse selo em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

3. **NÃO PROCEDENTE — vacuidade específica de `assertEqual({}, por_colab)` por ninguém ser chamado.** A mesma execução exige `particao_do_app` exatamente uma vez. O teste de `o_que_segura` exige também um dia produzido. Ainda assim, esses testes medem chamadas; não demonstram equivalência do conjunto ao app nem consumo das pendentes retornadas. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:110`, `:114`, `:115`, `:161`, `:164`.

4. **NÃO PROCEDENTE — nova asserção de `test_e3_portas_tarefa` ficou vazia.** Exige exatamente `[p.pk]` no C6, uma alimentação do painel, verbos específicos e repetição sem queries; C6 ausente ou adicional nesta fixture falha. Referências: `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:148`, `:152`.

5. **NÃO PROCEDENTE — `pos_suite` perdeu toda mordida contra C6 redundante.** O contrato ficou mais estreito: pergunta respondida na grade, fora dos pendentes. O mutante registrado que remove a exclusão das perguntas da grade continua produzindo assertion após controle verde. Referências: `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:52`, `:65`, `:67`; mutante em `app/chamados/tests/e3_pos_suite_sem_banco.py:31`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

6. **NÃO PROCEDENTE — E4b mascara residual `cobrar` como resolvido.** A fixture mudou para disputa fechada; o teste agora exige `segue_sem_porta`, chamado reaberto, nota única e idempotência. O teste do universo exige dois dias antes e um sem porta depois. Referências: `app/chamados/tests/test_e4_passivo.py:42`, `:76`, `:81`, `:83`, `:86`, `:89`, `:117`, `:122`, `:125`.

7. **PROCEDENTE, alcance limitado — teste `o261e` deixou de demonstrar ausência de consulta paralela pelo vigia.** Antes o mock de app devia receber zero chamadas; agora a partição deve receber chamadas e a saída deve mudar. Isso demonstra influência do insumo sobre tarefa e vigia, mas um vigia com consulta paralela à mesma partição também poderia mudar e satisfazer esse teste. Referências: `app/chamados/tests/test_o261e_f5_alcance.py:264`, `:273`, `:274`, `:278`, `:280`.

8. **Item 3 coberto nos corpos testados.** C4 elimina o dono por `cobrar`; C14 conserva a porta `validar` do admin quando o colab não tem pendência; C15 publica app quando o único residual é `cobrar`. Os respectivos mutantes morreram com controle verde nesta sessão. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`; implementação em `app/chamados/services/tarefa_do_dia.py:610`, `:618`, `:629`. Equivalência real de universo/C6 não é demonstrada pela matriz: o teste da carga usa C6 mockado e `_perg` já pronto, em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:229`, `:236`, `:246`.

Executados juntos, sem banco: degrau 33, pós-suite 7 e portas-v6 10, total 50 testes verdes, processo rc 0. Referências das suítes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`; resultado sem artefato persistido: [nao verificado em arquivo]. Os testes ORM n1/portas/E4/o261 foram avaliados por leitura; execução: [nao verificado].

## 14/11 19:04 · sessao 062f239e93bc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura local implementada para revisão. **18 testes sem banco passaram; RED contra a base: 19 assertions, zero errors; 15 mutantes mortos.** Runner e catálogo: [test_bo_disputa_c25_sem_banco.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/test_bo_disputa_c25_sem_banco.py:410), [bo_disputa_c25_mutacoes.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_mutacoes.py:2). [nao verificado]

**O selo real permanece VERMELHO em models.py**, que ficou intocado. O hunk abaixo zera essas violações quando aplicado somente em memória. Há também uma expectativa contraditória no terceiro teste HX: ele exige aberta com o mesmo classificador silencioso dos dois testes restaurados; permaneceu intocado (`chamados/tests/test_hx_disputa_muda.py:44`, `:64`, `:72`). Suíte ORM e certificação: [nao verificado]. Ruff não executou: módulo indisponível; lint: [nao verificado].

Arquivos tocados por item:

| Item | Arquivos |
|---|---|
| 1 — eleitor/escritor | `chamados/services/fio.py:752`, `:784`; `chamados/services/disputa_emissao.py:97`; `chamados/services/declaracao_texto.py:412` |
| 2 — push | `chamados/services/pergunta_no_app.py:84`; `disputa_emissao.py:1830`; `acoes_disputa.py:110`; `cobranca.py:197`; `validacao.py:641`; `chamados/views.py:1448`; `chamados/views_cobrar.py:23`; `ponto/management/commands/alertar_disputas_pendentes.py:56`; `fio.py:681` |
| 3 — restaurar os dois protegidos | `chamados/tests/test_hx_disputa_muda.py:44`, `:64`: removidas somente as duas chamadas de `_lavrar_ata_do_dia` |
| 4 — autoridade do dia | `chamados/services/disputa_emissao.py:212` |
| RED, mutantes e censos | `chamados/tests/test_bo_disputa_c25_sem_banco.py:89`; `bo_disputa_c25_mutacoes.py:2`; `bo_disputa_c25_censos.py:159` |

O eleitor único considera o fato explícito do intervalo antes do chamado genérico do dia; empate dentro do fato usa menor PK. Pergunta existente conserva seu candidato. `_destino_do_intervalo` e S160 agora delegam a ele (`chamados/services/fio.py:747`, `:752`; `chamados/services/disputa_emissao.py:97`). O escritor recebe o destino decidido, inclusive `None`, sem reeleger (`chamados/services/fio.py:784`).

Na base, S160 filtrava chamados vivos por `motivos_atendidos`, excluía candidato com pergunta não materializada, aceitava comparação de dia parcialmente desconhecida e terminava com `None` na ambiguidade após o desempate por turno/estágio (`0c8adfe5b:app/chamados/services/disputa_emissao.py:167`). Dependiam desse `None` o HIST/c1/c2 do reatamento e o reatamento imediato da pergunta recém-criada (`chamados/reconciliador.py:1559`; `chamados/services/disputa_emissao.py:361`). Agora:

- Ambiguidade do mesmo fato resolve por menor PK; esses caminhos deixam de receber esse caso (`chamados/services/fio.py:773`).
- Zero candidatos continua chegando ao caminho de órfã; candidato sem dia forte não entra na eleição (`chamados/services/fio.py:774`; `chamados/services/disputa_emissao.py:109`).
- Pergunta sem dia e sem chave deixa de obter candidato/data pela existência de um único chamado; continua sem destino derivado (`chamados/services/fio.py:767`; `chamados/reconciliador.py:1574`).
- `ESTAGIOS_TURNO` foi preservado para os outros leitores C8/I4b (`chamados/services/disputa_emissao.py:94`; `chamados/juizes.py:2212`; `chamados/management/commands/auditar_invariantes_chamados.py:698`).

A autoridade do dia é **`fio.dia_do_turno_da_linha`**, já chamada pelo nascimento: episódio passa pelo turno da batida; marco passa pelo dia da célula (`chamados/services/fio.py:1675`; `chamados/models.py:2936`; `ponto/turnos.py:2927`, `:2939`). O contexto fornece somente a chave da consulta. Texto e dedup usam a resposta da autoridade; sem resposta, o motivo é recusado como `InsumoAusente`, com `motivo_sem_dia_do_fato`, sem cascata grade/contexto/batida (`chamados/services/disputa_emissao.py:212`, `:239`).

O caminho genérico de órfãos voltou a ouvir o classificador antes de aplicar a espera por ata. Silêncio fecha a disputa muda; motivo efetivamente solicitado sem ata continua aberto, sem pergunta e sem push, contado em I7b (`chamados/services/disputa_emissao.py:1278`, `:1353`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:332`, `:344`; `chamados/management/commands/auditar_invariantes_chamados.py:74`). Nos anexos fornecidos não há aval literal que autorize estender a espera ao primeiro caminho; a extensão saiu (`chamados/services/disputa_emissao.py:1278`).

Todos os envios de disputa abaixo passam pela lista **`consulta_do_app()['pendentes']`**: C6 `pergunta_viva`, resposta nula e disputa aberta. O callback relê essa lista após o commit; rollback descarta o envio (`chamados/services/pergunta_no_app.py:45`, `:61`, `:84`).

| Sítio de envio | Predicado adicional conservado |
|---|---|
| `chamados/services/disputa_emissao.py:706` | motivos novos e disputa aberta |
| `chamados/services/disputa_emissao.py:1104` | `notificar`, complemento, dona diferente da principal |
| `chamados/services/disputa_emissao.py:1369` | `notificar`, motivos e pendentes da disputa |
| `chamados/services/disputa_emissao.py:1459` | emissão da âncora; gate comum decide o aviso |
| `chamados/services/disputa_emissao.py:1804` | recobrança com pendentes; zero retorna sem cobrar |
| `chamados/services/disputa_emissao.py:1830` | silêncio por ContextVar; WebPush/FCM dentro do callback |
| `ponto/management/commands/alertar_disputas_pendentes.py:56` | carimbo diário; disputa muda nem recebe carimbo |
| `chamados/services/acoes_disputa.py:110` | reabertura de perguntas corrigíveis |
| `chamados/services/acoes_disputa.py:296` | reabertura administrativa autorizada |
| `chamados/views.py:1448` | reabertura pela supervisão |
| `chamados/services/validacao.py:641` | recusa da resposta |
| `chamados/services/cobranca.py:197` | pendências e prazo; driver compartilhado permanece em `:107`, `:112` |
| `chamados/views_cobrar.py:47` | cobrança individual/massa, permissões e throttle dos chamadores |
| `chamados/services/acoes_disputa.py:152`, `:158`; `chamados/services/fio.py:681`, `:688` | avisos de regularização também passam pelo gate literal; sem pendente são suprimidos |

“Colaborador notificado” depende agora de entrega confirmada por algum canal; a quantidade vem das perguntas pendentes, sem usar `len(orfaos)` (`chamados/services/acoes_disputa.py:233`). Agendamento e entrega têm campos separados no recibo (`chamados/services/pergunta_no_app.py:98`).

O censo completo, com **arquivo:linha, função, campo e forma de acesso**, é reproduzível pelo [censo AST](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_censos.py:170):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/bo_disputa_c25_censos.py --censo
```

Ele inclui atributos, filtros, argumentos, literais de `values`/`only`/`save` e templates; a classificação do modelo exige ler o sítio indicado (`chamados/tests/bo_disputa_c25_censos.py:170`). Os escritores de interesse depois da mudança são:

| Campo | Escritores |
|---|---|
| `PerguntaDisputa.chamado_destino` | `chamados/services/fio.py:792`, `:794`, `:797`; pendências reais em `chamados/models.py:367`, `:3043`, `:3067`, `:3075` |
| `DS.fechada_em` | `chamados/models.py:1988` (`fechar`), `:2205` (`reabrir_admin`), `:2229` (`reabrir_sistema`) |
| `DS.motivos` | `chamados/models.py:2022`, `:2119`, `:2215`; `chamados/services/disputa_emissao.py:422`; nascimentos em `:533`, `:1175`, `:1439` |
| `PerguntaDisputa.respondida_em` | `chamados/models.py:1555` (simulação), `:1613` (ressuscitação delegada), `:3193`, `:3224`, `:3323`; `chamados/services/disputa_emissao.py:1546`, `:1671`/`:1710`, `:1731`, `:1752` |
| push | drivers dos callbacks inventariados na tabela anterior; porta comum em `chamados/services/pergunta_no_app.py:84` |

Os principais consumidores de `chamado_destino`, incluindo FK, consultas e projeções, ficam nos sítios abaixo; o comando de censo discrimina cada ocorrência por função (`chamados/tests/bo_disputa_c25_censos.py:170`):

```text
api/views.py:1031,2548,2567,2568
chamados/models.py:304,310,311,345,361,362,366,436,443,444,1967,2083,3010,3029,3051,3057
chamados/juizes.py:779,843,857,1860,1987,2034,2346
chamados/reconciliador.py:652,1052,1502
chamados/relatorios_disputa.py:43
chamados/views_cobrar.py:188
chamados/services/acoes_chamado.py:367
chamados/services/acoes_disputa.py:362
chamados/services/becos.py:30,46,47
chamados/services/competencia_trancada.py:121,132,165
chamados/services/declaracao_texto.py:302,365,412,653
chamados/services/disputa_emissao.py:345,350,361,630,631,633,635,867,878,1004,1005,1116,1137,1333,1403,1455,1500,1870,1978,2107
chamados/services/explicador.py:122
chamados/services/fio.py:79,80,116,134,225,226,330,395,396,816,817,819,827,833,860,1079,1106,1262,1263,1264,1269,2113
chamados/services/foto_e4b.py:259
chamados/services/historico.py:96
chamados/services/janela_colab.py:64,65
chamados/services/limbo_folgas.py:205,240,241
chamados/services/materializacao.py:342,533,587,588,717,800,1000,1082,1111,1113,1248,1269,1271
chamados/services/pergunta_no_app.py:240,263,266
chamados/services/raiox.py:57,62,137
chamados/services/relavra_via.py:101,103,156
chamados/services/tarefa_do_dia.py:377
chamados/services/validacao.py:309,341,455,870
chamados/services/vigia_carimbo.py:103
chamados/management/commands/auditar_disputas.py:151
chamados/management/commands/auditar_invariantes_chamados.py:610,616,621,672,673,679,942,978,979,982,1247,1662
chamados/management/commands/backfill_veredito_lavrado.py:101
chamados/management/commands/lavrar_perguntas_stale.py:58,88
chamados/management/commands/migrar_data_evento_dia_do_turno.py:377,472
chamados/management/commands/reabrir_orfas_enterradas.py:69
chamados/management/commands/reatar_beco_folga_ao_limbo.py:87,122,124
chamados/management/commands/relavrar_via_i6.py:56
colaboradores/services/calendario.py:474
ponto/services/veredito_celula.py:187
ponto/management/commands/reconciliar_fantasmas.py:212
ponto/management/commands/reconciliar_grade.py:444
```

Leitores de `DS.fechada_em`:

```text
api/foto_recebida.py:62
api/views.py:1013,1029,2671
chamados/models.py:190,1621,1871,1935,2087,2194,2223,2280,2318,2729
chamados/juizes.py:2396
chamados/reconciliador.py:412,551,600,611,612,1050,1161,1162,1504
chamados/relatorios_disputa.py:41
chamados/signals.py:29,131,155
chamados/views.py:471,495,512,530,911,1329
chamados/views_cobrar.py:184,187
chamados/services/acoes_chamado.py:346
chamados/services/acoes_disputa.py:263,355
chamados/services/becos.py:27,56,59
chamados/services/cobranca.py:238
chamados/services/competencia_trancada.py:176,221
chamados/services/descarte_disputa.py:9
chamados/services/disputa_emissao.py:42,414,468,492,695,705,1013,1103,1145,1368,1412,1600,1980,2075,2099,2101,2160,2162
chamados/services/fila_por_causa.py:225,259,262
chamados/services/fio.py:130,134,600,633
chamados/services/foto_e4b.py:182
chamados/services/materializacao.py:591,646,1322
chamados/services/pergunta_no_app.py:71
chamados/services/regularizacao_ext.py:60
chamados/services/relavra_via.py:183,260
chamados/services/validacao.py:171,237,240,242
chamados/services/vigia_fio.py:753
chamados/management/commands/auditar_chamados_orfaos.py:41
chamados/management/commands/auditar_disputas.py:72,87,136,152,167
chamados/management/commands/auditar_invariantes_chamados.py:63,620,676,890,1851
chamados/management/commands/encerrar_perguntas_com_fato.py:82
chamados/management/commands/fechar_becos_disputa.py:32
chamados/management/commands/reabrir_orfas_enterradas.py:96,131,164,165
colaboradores/painel_cell.py:32
colaboradores/services/desligamento.py:164
colaboradores/services/drawer.py:70
colaboradores/services/situacional.py:117
colaboradores/signals.py:73
escala/signals.py:170
ponto/services/entrada_sincronizada.py:57
ponto/management/commands/alertar_disputas_pendentes.py:35
ponto/management/commands/escalonar_chamados_supervisao.py:58,149
ponto/management/commands/reconciliar_fantasmas.py:214
templates/chamados/partials/modal_fio.html:379
```

Leitores de `PerguntaDisputa.respondida_em`:

```text
api/views.py:1160,1172,2556,2570,2613,2630
api/views_mensageria.py:569,570
chamados/models.py:1900,3106,3109,3188,3192,3409,3559,3985,3992,3993,3994
chamados/juizes.py:348,852,1385,1528,1673,1680,1683
chamados/relatorios_disputa.py:21
chamados/veredito_lavrado.py:207,208
chamados/views.py:1280,1416
chamados/views_cobrar.py:192,199
chamados/services/acoes_disputa.py:96,132
chamados/services/becos.py:30
chamados/services/cobranca.py:237
chamados/services/declaracao_texto.py:406,408,674
chamados/services/disputa_emissao.py:1697,1748,1892,1910,2050,2123,2140
chamados/services/fio.py:339,660
chamados/services/foto_e4b.py:180
chamados/services/furo_sem_canal.py:81
chamados/services/historico.py:99
chamados/services/materializacao.py:675,1376
chamados/services/painel.py:67
chamados/services/passivo_validado.py:66
chamados/services/pergunta_no_app.py:71
chamados/services/raiox.py:138
chamados/services/validacao.py:416,604,609,633,952
chamados/services/verbos.py:335,336,342,345
chamados/services/vigia_fio.py:680,788
chamados/management/commands/auditar_invariantes_chamados.py:261,676,1191,1658,1689
chamados/management/commands/backfill_decisoes_pergunta.py:56,60
chamados/management/commands/curar_perguntas_em_limbo.py:46,69
ponto/management/commands/reconciliar_grade.py:445
templates/chamados/partials/modal_fio.html:185,217,220
templates/chamados/partials/_card_atendimento.html:94,100,137,155,384,385
templates/chamados/partials/_pergunta_card.html:24,29,32
```

Leitores de `DS.motivos`, somente respondendo à fila:

| Leitor | Uso / pressuposto |
|---|---|
| `chamados/models.py:2017`, `:2088`, `:2212` | lista restante, remoção/fecho e acréscimo; pressupõem agregado completo |
| `chamados/services/disputa_emissao.py:400`, `:418`, `:1127`, `:1148`, `:1804` | recorrência, união, complemento, seed e mensagem |
| `chamados/services/descarte_disputa.py:9`, `:10` | fotografia e diferença do agregado fechado |
| `chamados/services/fio.py:680`; `acoes_disputa.py:151` | corpo da notificação |
| `chamados/relatorios_disputa.py:52` | exibição |
| `chamados/management/commands/relatorio_pendencias_disputa.py:39` | exibição dos labels recebidos |
| `chamados/management/commands/auditar_disputas.py:88`, `:142`, `:169`, `:257` | contexto, relatório e comparação com perguntas; `:169` pressupõe todos agregados |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | universo dos dias; pressupõe todos agregados |
| `ponto/management/commands/alertar_disputas_pendentes.py:74` | log |
| `api/foto_recebida.py:63`, `:70` | descoberta por foto e restantes para aparar/fechar; pressupõe agregado |
| `escala/signals.py:171` | descoberta exige âncora agregada |
| `colaboradores/signals.py:74` | descoberta exige posto agregado |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | descoberta exige device agregado |
| `templates/chamados/partials/modal_fio.html:327` | habilita fluxo da âncora pelo agregado |
| `templates/chamados/detalhe_local.html:301` | exibição |

Portanto, omitir motivo sem pergunta local pode excluir a disputa dos filtros de foto, âncora, posto e aparelhos; esse comportamento ficou na fila (`api/foto_recebida.py:63`; `escala/signals.py:171`; `colaboradores/signals.py:74`; `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49`).

O selo por função detecta atribuição, `setattr`, constructor/create/update, dict local, `save(update_fields)` e apelidos de import. Hoje acusa os quatro sítios abaixo; os encaminhamentos de `models.py:2846` e `:2858` também estão no hunk, embora sejam passagem de argumento (`chamados/tests/bo_disputa_c25_censos.py:58`):

```text
chamados/models.py:367   ChamadoColaborador._vivas_entre: atributo
chamados/models.py:3043  PerguntaDisputa._nascer: atributo
chamados/models.py:3067  PerguntaDisputa._nascer: save(update_fields)
chamados/models.py:3075  PerguntaDisputa._nascer: create/update
```

Hunk exato para a sessão, com âncoras únicas verificadas pelo gerador; predicados e campos de persistência preservados (`chamados/tests/bo_disputa_c25_censos.py:17`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:398`):

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -364,7 +364,8 @@
                 if competencia_do_dia_entregue(_p.data_evento, _p.disputa.colaborador.empresa, _memo) is not False:
                     continue
                 if _p.chamado_destino_id == self.pk:
-                    _p.chamado_destino = self
+                    from chamados.services.fio import gravar_destino_da_pergunta
+                    gravar_destino_da_pergunta(_p, self)
                 if pergunta_viva(_p):
                     _vivas.append(_p)
         finally:
@@ -2842,8 +2843,9 @@
         PROPRIA, `abrir_conversa`. Selo: chamados/tests/test_selo_p13_data_evento_porta_unica.py."""
         if data_evento is None:
             raise ValueError('PerguntaDisputa.abrir exige data_evento (dia do turno); conversa e abrir_conversa')
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=data_evento, conversa=False,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -2854,8 +2856,9 @@
         de plantio nao a le como dia do turno; `ancora_ausente` e `escolha` nao plantam por hora do dia).
         Devolve (pergunta, criada)."""
         from django.utils import timezone as _tz_b2
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=_tz_b2.localdate(), conversa=True,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -3040,8 +3043,8 @@
                         from chamados.services.fio import _costura_nao_mata_o_fio
                         _ok_f, _mot_f = _costura_nao_mata_o_fio(ex, chamado_destino)
                         if _ok_f:
-                            ex.chamado_destino = chamado_destino
-                            upd.append('chamado_destino')
+                            from chamados.services.fio import gravar_destino_da_pergunta
+                            upd.append(gravar_destino_da_pergunta(ex, chamado_destino))
                         else:
                             from core.observ import engolir as _eng_cr
                             with _eng_cr('chamados.abrir', 'trilha_costura_recusada', pergunta=ex.pk):
@@ -3064,7 +3067,8 @@
                         from chamados.veredito_lavrado import lavrar as _lv_s2
                         _lv_s2(ex, salvar=False)
                         upd += ['veredito_lavrado', 'julgado_em', 'janela_expira_em']
-                        ex.save(update_fields=upd)
+                        from chamados.services.fio import gravar_destino_da_pergunta
+                        gravar_destino_da_pergunta(ex, ex.chamado_destino, salvar=True, campos=upd)
                         cls.acordar_celulas(_linhas_antes_reuso)
                     return ex, False, _dia_fato
             # FIO-E0-V4-E1 (b): `colaborador` e `instancia_marco` da identidade sao derivados no `save` de insercao
@@ -3072,10 +3076,10 @@
             # volta 15d2 (aval E1-VOLTA-15D-UM-PONTO-DE-ESCRITA): o dia do juiz entra pelo UNICO ponto de escrita
             # (`fio.gravar_dia_do_fato`); a insercao e a mesma do `create` (instancia + `save(force_insert=True)`).
             from chamados.services.fio import gravar_dia_do_fato
-            p = cls(disputa=disputa, motivo=motivo, chamad

[truncado em 20000 de 24327 caracteres]

## 14/11 19:00 · sessao 0c7544dcf12d · PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA · censo · origem=Code

# CENSO — PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA

**Fechamento global: `[nao verificado]`.** A leitura identificou portas administrativas que corrigem resposta, decidem o dia, renovam cobrança e encerram perguntas; portanto, a validação não é a única intervenção possível. A correção pode partir de `'(sem resposta)'`, enquanto “Resolver dia” pode registrar fatos e encerrar perguntas com decisão de silêncio. (`app/chamados/services/acoes_disputa.py:353`; `app/chamados/services/acoes_disputa.py:371`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:323`)

**Alcances reais nos 2.381 dias: `[nao verificado]`.** Não houve execução contra banco. A sonda abaixo distingue guardas chamáveis de corpos que misturam guarda, gravação, sinais e notificações; estes últimos não são executados. (`app/chamados/services/validacao.py:398`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/disputa_emissao.py:1733`)

**Denominador do anexo não reconciliado:** a transcrição conferida por grupos somou **2.357 dias**, diferença de **24** para os 2.381 declarados. Não atribuí colaborador, prazo ou comportamento aos 24 não contabilizados. A contagem não equivale a provar que faltam linhas no original. `[nao verificado]`

Árvore/hash identificado pelo pedido: `3a95114a5e25f0212bff04e861bdc62960d5c042`; correspondência independente entre conteúdo e hash `[nao verificado]`. Nenhum arquivo foi criado, editado ou apagado; este Markdown é a entrega textual para gravação pela sessão. `[nao verificado]`

## (a) Método e inventário das portas

A varredura executou `ast.parse` nos Python de produção sob `app`, inspecionou `Import`, `ImportFrom`, `Call`, funções, classes, registros de admin e declarações de URLs. Foram parseados 765 arquivos, sem erro de sintaxe nesse recorte. Esse resultado não demonstra sozinho fechamento de chamadas dinâmicas, herança externa e todas as ações indiretas da árvore. `[nao verificado]`

O censo legado não foi usado como prova de fechamento: ele exclui `api/` e `admin/` e identifica POST/guardas por texto e regex. (`app/core/portas_da_tela.py:20`; `app/core/portas_da_tela.py:53`; `app/core/portas_da_tela.py:58`)

### Apelidos de import resolvidos

| Apelido | Destino | Evidência |
|---|---|---|
| `vreg` | `chamados.views_regularizacao` | `app/chamados/urls.py:6` |
| `_ac` | `chamados.services.acoes_chamado` | `app/chamados/views.py:423` |
| `_ad` | `chamados.services.acoes_disputa` | `app/chamados/services/fio.py:665` |
| `_ac_geo` | `chamados.services.acoes_chamado` | `app/chamados/services/fio.py:572` |
| `_corrig` | `chamados.models.perguntas_corrigiveis` | `app/chamados/views.py:1419` |
| `_reabrir2` | `chamados.models.reabrir_para_correcao` | `app/chamados/views.py:1435` |
| `_muda_a_folha` | `chamados.services.cobranca_folha.muda_a_folha` | `app/chamados/views_cobrar.py:88` |
| `_vc` | `ponto.services.veredito_celula.veredito_celula` | `app/ponto/views.py:3354` |

### Autoridade e significado de “sem resposta”

`is_staff` permite entrar no fio como gestor, mas várias ações exigem também permissão nominal e autoridade sobre o departamento do chamado; staff, permissão de ação e autoridade contextual são guardas distintas. (`app/chamados/views.py:35`; `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`)

O universo de pergunta devida não pode ser convertido em `respondida_em IS NULL`: a tarefa mantém `responder/colaborador` quando a última decisão de resposta tem texto vazio. Já a validação examina o carimbo `respondida_em`. Logo, **validar = 0 nos 2.381** não está demonstrado pelo anexo. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/validacao.py:418`)

Os três relógios também não são equivalentes: C9 compara `prazo_sla`; o prazo de cobrança admite derivação pelo nascimento; `sla.vencido` usa referência de followup/nascimento e urgência. (`app/chamados/catalogo/motor.py:54`; `app/chamados/catalogo/motor.py:59`; `app/chamados/services/sla.py:17`; `app/chamados/services/sla.py:33`)

### Pergunta, disputa, fio e painel

As rotas abaixo partem de `/chamados/`, prefixo publicado no projeto. Alcance quantitativo de cada linha: `[nao verificado]`. (`app/config/urls.py:28`)

| Ação e porta | Guarda que decide | O que grava / trilha |
|---|---|---|
| **Validar pergunta** — detalhe `local/<id>/`; fio `colaborador/<id>/`; modal `modal/<id>/` | Contexto por chamado; pergunta deve estar listada. Serviço exige resposta carimbada, não validada e confirmação quando implausível. Detalhe e fio diferem quanto à disputa fechada. (`app/chamados/services/fio.py:604`; `:636`; `app/chamados/views.py:491`; `app/chamados/services/validacao.py:418`; `:429`) | `PerguntaDisputa.validada_em/validada_por`; `DecisaoPergunta.VALIDACAO`; materialização; logs. A recusa da materialização pode desfazer a validação. (`app/chamados/services/validacao.py:159`; `:164`; `:488`; `:489`; `:504`) |
| **Recusar pergunta** — mesmas telas | Motivo; não validada; respondida; não fora do fato. No fio, pergunta de disputa aberta e listada pelo chamado. (`app/chamados/services/validacao.py:591`; `:602`; `:605`; `:609`; `app/chamados/services/fio.py:671`) | `recusa_motivo`; reabertura com decisão `recusa`; nota, evento, log e push. (`app/chamados/services/validacao.py:619`; `:628`; `:633`; `:637`; `:643`; `:647`) |
| **Corrigir resposta** — mesmas telas | HH:MM, motivo, pergunta existente, não validada, disputa aberta e tipo `hora`. **Não exige resposta anterior.** (`app/chamados/services/acoes_disputa.py:336`; `:345`; `:347`; `:349`; `:353`) | Nota com conteúdo anterior; core escreve `resposta_colab/respondida_em` e decisão `RESPOSTA`; log `corrigir_resposta_admin`; depois valida/materializa. Pode ficar resposta corrigida com materialização recusada. (`app/chamados/services/acoes_disputa.py:359`; `:371`; `:375`; `:379`; `:382`; `app/chamados/services/disputa_emissao.py:1508`; `:1511`) |
| **Regularizar disputa — detalhe** | Resolução e nenhuma pergunta viva sem carimbo de resposta; guarda dentro da ação. (`app/chamados/services/acoes_disputa.py:124`; `:130`) | Materialização granular; fechamento conforme resultado; log. (`app/chamados/services/acoes_disputa.py:139`; `:144`) |
| **Regularizar disputa — fio/modal** | Possui guarda própria inline: resolução e nenhuma viva sem resposta. Não delega a guarda do detalhe. (`app/chamados/services/fio.py:684`; `:686`; `:698`) | Materialização granular; log; notificações. (`app/chamados/services/fio.py:708`; `:713`; `:719`) |
| **Reabrir disputa — fio/modal** | Usa `perguntas_corrigiveis`; sem corrigíveis recusa. Este ramo não executa a alternativa de recobrança do endpoint específico. (`app/chamados/services/acoes_disputa.py:95`; `:97`; `:98`) | `reabrir_para_correcao`; nota, push e log. (`app/chamados/services/acoes_disputa.py:103`; `:106`; `:108`; `:113`) |
| **Reabrir ou recobrar** — `local/<id>/reabrir-disputa/` | Gate condicional: só consulta `pode_agir_chamado` se não tiver `gerir_chamados`. Resolve disputa, corrigíveis e pendentes vivas dentro da view. (`app/chamados/urls.py:20`; `app/chamados/views.py:1404`; `:1410`; `:1417`; `:1420`) | Corrigíveis: reabre perguntas, nota e log. Sem corrigíveis, com pendente viva: recobra. (`app/chamados/views.py:1425`; `:1436`; `:1440`; `:1456`) |
| **Recobrança efetiva** — ramo anterior | Seleção fica inline na view; serviço não é guarda pura. (`app/chamados/views.py:1421`; `app/chamados/services/disputa_emissao.py:1697`) | Renova `ChamadoColaborador.prazo_sla`; nota, push e log `recobrar_questionario`. (`app/chamados/services/disputa_emissao.py:1725`; `:1728`; `:1735`; `:1741`) |
| **Reabertura administrativa** — fio/modal | Staff/admin_ti; motivo; justificativa mínima; disputa fechada. (`app/chamados/services/acoes_disputa.py:245`; `:251`; `:253`; `:255`) | Reabre disputa; cria pergunta de conversa; reabre chamado; nota, pushes e log. (`app/chamados/services/acoes_disputa.py:259`; `:263`; `:270`; `:274`; `:297`) |
| **Conferir data** — `disputa/pergunta/<id>/conferir-data/` | POST e `gerir_chamados`; serviço exige pergunta, justificativa, divergência pelo juiz e ausência de conferência anterior. (`app/chamados/urls.py:18`; `app/chamados/views.py:1307`; `:1309`; `app/chamados/services/validacao.py:686`; `:690`; `:693`; `:697`) | `data_conferida_em/por`; log com antes/depois e justificativa. (`app/chamados/services/validacao.py:699`; `:701`; `:702`) |
| **Aplicar declaração de texto — detalhe** | Consulta propostas pelo followup; guardas adicionais no aplicador. (`app/chamados/views.py:478`; `app/chamados/services/declaracao_texto.py:649`; `:668`) | Emite/reusa disputa/pergunta; responde pelo core; valida pelo serviço canônico. (`app/chamados/services/declaracao_texto.py:659`; `:666`; `:693`; `:696`) |
| **Cobrar por push** — `cobrar/<id>/` | `gerir_chamados`, POST, departamento, throttle por colaborador/dia; envio precisa ter sucesso. (`app/chamados/urls.py:25`; `app/chamados/views_cobrar.py:25`; `:31`; `:36`; `:51`) | Push e nota `[COBRANCA]`; esta porta não chama o renovador de prazo. (`app/chamados/views_cobrar.py:44`; `:55`) |
| **Cobrar em massa** — `cobrar/massa/` | Fila/verbo, contexto, `muda_a_folha`, throttle; `previa=1` retorna a prévia antes do envio. (`app/chamados/urls.py:31`; `app/chamados/views_cobrar.py:78`; `:92`; `:95`; `:102`; `:117`) | Push e nota por colaborador alcançado. (`app/chamados/views_cobrar.py:123`; `:132`) |
| **Cobrar colaborador pelo copiloto** — `/ia/perguntar/` | `tem_acao` e guarda real `checar_cobranca`: pergunta pendente no app e ausência de prazo futuro entre os pais. (`app/core/urls.py:15`; `app/core/views_ia.py:841`; `app/chamados/services/cobranca.py:75`; `:88`) | Prazo nos pais, notas, push e log `cobrar_colaborador`. (`app/chamados/services/cobranca.py:178`; `:181`; `:187`; `:188`) |
| **Cobrar dia** — `cobrar/dia/` | POST, `regularizar_batida`, departamento/data; serviço recusa dia ainda não terminado. Guardas posteriores estão misturadas à execução. (`app/chamados/urls.py:32`; `app/chamados/views_cobrar.py:314`; `:317`; `app/chamados/services/cobrar_dia.py:28`) | **Log antes do resultado final**; cartório com `toque=True`; possível emissão de disputa/perguntas. (`app/chamados/services/cobrar_dia.py:39`; `:41`; `:49`) |
| **Validar inline** — `validar-inline/<id>/` | Permissão, área, disputa e seleção de perguntas vivas respondidas. (`app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:149`; `:158`; `:162`; `:169`) | Delega à validação canônica. (`app/chamados/views_cobrar.py:208`) |
| **Validar lote JSON/tela** — `validar-lote/`, `validar-lote/tela/` | Permissão/contexto, pergunta elegível e classe real do lote; guarda integral não isolada. (`app/chamados/urls.py:34`; `:37`; `app/chamados/views_cobrar.py:249`; `app/chamados/services/validacao.py:767`; `:770`) | Validação canônica; tela acrescenta log do lote. (`app/chamados/services/validacao.py:781`; `app/chamados/views_cobrar.py:383`) |
| **Arquivar individual** — `arquivar/<id>/` | Permissão, POST e área; endpoint individual não exige idade. (`app/chamados/urls.py:26`; `app/chamados/views_arquivar.py:26`; `:28`; `:32`) | `silenciado_ate/por/motivo/atualizado_em`, evento e nota. (`app/chamados/services/arquivo.py:166`; `:171`; `:173`; `app/chamados/views_arquivar.py:44`) |
| **Arquivar lote** — `arquivar/lote/` | Corte positivo; filtro, verbo, idade, silêncio e contexto. (`app/chamados/urls.py:28`; `app/chamados/views_arquivar.py:105`; `:125`; `:135`; `:137`) | Mesmo escritor do véu; evento por chamado. (`app/chamados/views_arquivar.py:140`; `app/chamados/services/arquivo.py:173`) |
| **Desarquivar** — `desarquivar/<id>/` | Permissão/POST/área; via de tranca tenta reabertura que pode recusar. (`app/chamados/urls.py:27`; `app/chamados/views_arquivar.py:54`; `:60`; `:70`) | Limpa véu, carimba `resgatado_em`; pode ressuscitar perguntas; nota. (`app/chamados/services/arquivo.py:202`; `:206`; `app/chamados/views_arquivar.py:86`; `:88`) |
| **Assumir/liberar responsável** — `local/<id>/atribuir/` | Gestor e contexto do chamado. (`app/chamados/urls.py:41`; `app/chamados/views.py:1010`; `:1015`) | `atribuido_a/em/atualizado_em`; nota e log quando muda. (`app/chamados/services/vinculos.py:132`; `app/chamados/views.py:1027`; `:1034`) |
| **Mensagem rápida** — `colaborador/<id>/mensagem-rapida/` | Permissão e mensagem não vazia. (`app/chamados/urls.py:40`; `app/chamados/views.py:967`; `:971`) | Abre chamado `mensagem_dp`, nota e push. (`app/chamados/views.py:973`; `:986`; `:989`) |
| **Resposta livre/status** — detalhe/fio | Contexto; conteúdo/status; transição tem guardas posteriores próprias. (`app/chamados/services/fio.py:747`; `:752`; `app/chamados/models.py:128`) | Nota pode preceder transição recusada; transição e pauta pelo executor; logs. (`app/chamados/services/fio.py:756`; `:760`; `app/chamados/services/acoes_chamado.py:304`; `:328`) |
| **Abrir modal / marcar lidas** | Gestor. **GET tem escrita.** (`app/chamados/views.py:872`; `:877`) | `FollowupChamado.lido_dp_em`, via `QuerySet.update`. (`app/chamados/views.py:880`; `app/chamados/services/nota.py:126`; `:132`) |
| **Regularização externa: criar** | Gestor/departamento e motivo. (`app/chamados/urls.py:49`; `app/chamados/views_regularizacao.py:19`; `:49`) | `RegularizacaoExterna`, chamado de rastro e vínculo. (`app/chamados/services/regularizacao_ext.py:237`; `:247`; `:257`; `:310`) |
| **Regularização externa: adicionar item** | Estado do lote, data/horários, alvo pertencente ao colaborador e consistência dos horários. (`app/chamados/urls.py:51`; `app/chamados/views_regularizacao.py:80`; `:87`; `:109`; `:120`) | `RegularizacaoItemBatida`: data, horas, ação, alvo e intervalo. (`app/chamados/services/regularizacao_ext.py:326`) |
| **Regularização externa: excluir item** | Estado do lote. (`app/chamados/urls.py:52`; `app/chamados/views_regularizacao.py:143`) | Exclui o item. (`app/chamados/services/regularizacao_ext.py:336`) |
| **Regularização externa: enviar** | Estado e existência de itens. (`app/chamados/urls.py:53`; `app/chamados/views_regularizacao.py:160`; `:163`) | Estado `aguardando` e notificações. A materialização exige confirmação e recusa qualquer disputa aberta do colaborador. (`app/chamados/services/regularizacao_ext.py:343`; `app/chamados/views_regularizacao.py:170`; `app/chamados/services/regularizacao_ext.py:52`; `:59`) |

### Outros verbos dos roteadores

São ramos reais, e sua aplicabilidade aos dias do universo depende de dados não presentes no anexo. Alcances: `[nao verificado]`.

| Ação | Porta / guarda | Gravação / trilha |
|---|---|---|
| `aprovar_excecao` | Detalhe; `_pode_aprovar`; serviço recusa encerrado, sem exigir categoria nesse ponto. (`app/chamados/views.py:426`; `app/chamados/services/acoes_chamado.py:25`) | Batida, foto, nota, transição e log. (`app/chamados/services/acoes_chamado.py:44`; `:67`; `:69`; `:71`; `:77`) |
| `autorizar_geofence` / `_advertir` | Detalhe e fio; contexto e estado. (`app/chamados/views.py:537`; `app/chamados/services/fio.py:571`; `app/chamados/services/acoes_chamado.py:182`) | Pode criar `Advertencia`; nota, transição e log. (`app/chamados/services/acoes_chamado.py:199`; `:224`; `:229`; `:230`) |
| `recusar_geofence` | Estado, motivo e batida vinculada ao colaborador. (`app/chamados/services/acoes_chamado.py:260`; `:263`; `:265`) | Retratação da batida; nota, transição e log. (`app/chamados/services/acoes_chamado.py:277`; `:281`; `:286`; `:287`) |
| `aceitar_folga_limbo` | Sugestão e template existentes. (`app/chamados/views.py:432`; `app/chamados/services/limbo_folgas.py:85`; `:88`) | Altera folgas de `TipoEscala`; log e reconciliação. (`app/chamados/services/limbo_folgas.py:92`; `:95`; `:99`) |
| `corroborar_regua` | Vínculo existente e ainda não corroborado. (`app/chamados/views.py:441`; `app/escala/regua_defesa.py:655`; `:657`) | Régua, confiança e carimbos de corroboração; nota. (`app/escala/regua_defesa.py:662`; `:666`; `:672`) |
| `realocar_regua` | Destino/template, restrição de ciclo e `julgar_vinculo`. (`app/chamados/views.py:452`; `app/escala/regua_defesa.py:305`; `:310`; `:316`) | `executar_vinculo`; nota. (`app/escala/regua_defesa.py:321`; `:333`) |
| `pedir_confirmacao_ancora` | Categoria, área e resultado da fábrica. (`app/chamados/services/acoes_disputa.py:185`; `:187`; `:190`) | Cria disputa de âncora; log. (`app/chamados/services/acoes_disputa.py:189`; `:192`) |
| `iniciar_disputa_orfaos` | Guarda real `chamado_orfao_e_regularizavel`; recusa sem alvos ou repasse recusado. (`app/chamados/services/acoes_disputa.py:219`; `:220`; `:225`) | Fábrica de disputa/perguntas; log. (`app/chamados/services/acoes_disputa.py:222`; `:229`) |
| `aplicar_proposta_escala` | Gate do registro de intenções, assinatura/vínculo recalculados e restrições da proposta. (`app/chamados/services/proposta_no_fio.py:136`; `:138`; `:141`) | Despacha ao escritor `escala.services.propositor.aplicar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:151`; `app/core/intake.py:51`) |
| `recusar_proposta_escala` | Gate, motivo e proposta recalculada. (`app/chamados/services/proposta_no_fio.py:168`; `:170`; `:173`) | `propositor.recusar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:176`) |

### Formulários anunciados sem ramo correspondente

| Form do modal | Resultado do rastreio |
|---|---|
| `aplicar_declaracao_texto` | Form em `app/templates/chamados/partials/modal_fio.html:140`; handler encontrado no detalhe, `app/chamados/views.py:478`, mas não no roteador do fio. |
| `confirmar_correcao_pergunta` | Form em `app/templates/chamados/partials/modal_fio.html:190`; handler de produção correspondente `[nao verificado]`. |
| `apagar_pergunta_cega` | Form em `app/templates/chamados/partials/modal_fio.html:197`; handler de produção correspondente `[nao verificado]`. |

O POST desses forms, sem conteúdo que acione o ramo de mensagem, chega ao retorno “Nada a fazer” do roteador; não foi contado como operação funcional sobre a pergunta. (`app/chamados/services/fio.py:746`; `:781`)

### Portas sobre o dia fora de chamados

| Ação / URL | Guarda | Gravação / trilha |
|---|---|---|
| **Resolver dia:** `/ponto/celula/veredito/` — trabalhou/faltou/justificado | POST e `regularizar_batida`; guardas de negócio inline no serviço. (`app/ponto/urls.py:58`; `app/ponto/views.py:3355`; `app/ponto/services/veredito_celula.py:221`; `:243`; `:249`; `:309`; `:334`) | Batidas ou ausência aprovada; `fechar_por_admin`; decisão `SILENCIO`; chamados; lavra da célula; log. (`app/ponto/services/veredito_celula.py:323`; `:337`; `:190`; `:195`; `:203`; `:352`; `:356`) |
| **Declarar folga/falta pelo copiloto** — `/ia/perguntar/` | Gates diferentes: `editar_escalas`/`regularizar_batida`; juiz real `checar_declaracao`. (`app/core/intake.py:76`; `:80`; `:90`; `:94`; `app/ponto/services/declarar_dia.py:126`) | Folga pelo escritor de folgas e fechamento do fio; falta pelo veredito da célula. A escrita pode recusar depois da prévia. (`app/ponto/services/declarar_dia.py:167`; `:169`; `:191`; `:206`; `:215`) |
| **Inverter batida** — `/ponto/batida/<id>/flip/` | Retratação, autoria, competência e tipo dentro do executor. (`app/ponto/urls.py:59`; `app/ponto/services/flip_batida.py:15`; `:19`; `:22`; `:26`) | `Batida.tipo`, trilha e recomputação. (`app/ponto/services/flip_batida.py:30`; `:39`) |
| **Retratar batida** — `/ponto/batida/<id>/retratar/` | `_pode` chamável; motivo e estado inline. (`app/ponto/urls.py:61`; `app/ponto/services/retratar_humano.py:131`; `:166`; `:170`) | Retratação e motivo estruturado pela porta canônica. (`app/ponto/services/retratar_humano.py:179`; `:181`) |
| **Desfazer retratação** — `/ponto/batida/<id>/desfazer-retratacao/` | Autoria/permissão, estado, origem e janela; `pode_desfazer` sozinho não cobre tudo. (`app/ponto/urls.py:62`; `app/ponto/services/retratar_humano.py:194`; `:197`; `:199`; `:203`) | Aprovação e limpeza da retratação, com trilha.

[truncado em 20000 de 49883 caracteres]

## 14/11 18:52 · sessao c3a4441e6bc6 · - · - · origem=Ronald (tmux)

A transcrição reconciliada soma **2.357 dias de 300 colabs**, diferença de **24** para os 2.381 declarados; não ajustei contagens para fechar o universo. `[nao verificado]`

| Emp | 09/2026 | 10/2026 | 11/2026 |
|---|---:|---:|---:|
| 1 | 0 | 12 | 5 |
| 2 | 545 | 615 | 790 |
| 3 | 94 | 117 | 156 |
| 4 | 6 | 8 | 9 |

Fonte: anexo textual transcrito; conferência independente dos grupos pelos outros explorers. `[nao verificado]`

Top 10, desempate pelo menor colab: **534:77; 607:72; 667:61; 717:54; 317:45; 832:40; 650:39; 114:38; 144:38; 915:38**. Soma: **502 dias**. `[nao verificado]`

No corpus de 2.357 dias:

- **37 colabs → 1.186 dias (50,32%)**; os primeiros 36 somam 1.168. `[nao verificado]`
- **108 colabs → 1.888 dias (80,10%)**; os primeiros 107 somam 1.882. `[nao verificado]`

Para os limiares do universo declarado de 2.381, usando somente os dias transcritos:

- **38 colabs → 1.203 dias**, ultrapassando 1.191; os primeiros 37 somam 1.186. `[nao verificado]`
- **111 colabs → 1.906 dias**, ultrapassando 1.905; os primeiros 110 somam 1.900. `[nao verificado]`

Esses limiares não certificam concentração do universo completo, pois faltam reconciliar os 24 dias. `[nao verificado]`

Assumindo prazos em UTC e idade em dias completos, a distribuição dos 2.357 dias é: `[nao verificado]`

| Faixa | Dias |
|---|---:|
| <0 | 0 |
| 0–7 | 33 |
| 8–15 | 111 |
| 16–30 | 99 |
| >30 | 2.082 |
| Prazo `None` | 32 |
| Total | 2.357 |

O anexo removeu o offset dos prazos. Interpretá-los em `-03:00` transfere os três dias de col988, prazo `2026-10-14T17:30`, de `>30` para `16–30`: essas faixas passam a **2.079** e **102**, respectivamente. `[nao verificado]`

## 14/11 18:51 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

**Emp2 / 09/2026: 545 linhas, 123 colabs**, após cruzamento dos mapas por colaborador. Fonte: TSV colado; arquivo:linha `[nao verificado]`.

A diferença para os 544 da outra agente é **col836 / 2026-09-19**, uma linha omitida do mapa dela. Col832 tem 12 linhas; col667 tem 6. Fonte: anexo colado; arquivo:linha `[nao verificado]`.

Maiores: col534=31; col788=21; col607=18; col312=15; col650=15; col209=14; col832=12; col883=11; col346/362/599/820/877=10 cada. Fonte: anexo colado; arquivo:linha `[nao verificado]`.

Não ajustei a soma ao universo declarado de 2381. As demais fatias não foram recontadas nesta conferência `[nao verificado]`.

## 14/11 18:50 · sessao 8dc621e17fdc · GRADE-RESOLVE · censo · origem=Code

(A) `resolve.tsv`

```tsv
caso_fonte	caso_id	obra_id	estado_obra	prova_arquivo:linha	lateral_cen	lateral_lei	toca_congelado
HJ-INGESTAO-0511	HJ-005-U1	candidata sem prova: BO-ADMIN-2710	SEM OBRA	app/docs/BACKLOG.md:820;app/docs/GRADE-MESTRA.md:137;cobertura individual [nao verificado]	—	—	—
HJ-INGESTAO-0511	HJ-005-U2	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
HJ-INGESTAO-0511	HJ-005-U4	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
HJ-INGESTAO-0511	HJ-012-U2	nenhuma obra registrada	SEM OBRA	app/docs/RELATO.md:8390;cobertura [nao verificado]	—	—	—
HJ-INGESTAO-0511	HJ-017-U1	O232	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1239;app/docs/GRADE-MESTRA.md:853	sim	sim	não, nas linhas citadas
HJ-INGESTAO-0511	HJ-030-U1	FIO-LE-O-JUIZ	NO AR	app/docs/BACKLOG.md:2098;app/docs/BACKLOG.md:837	—	—	—
BO2910-ESTRUTURA	01	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	02	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	05	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	06	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	08	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO-ADMIN-2710	1	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
BO-ADMIN-2710	2	JUIZ-DO-PAR-DUAS-ENTRADAS	NO AR	app/docs/GRADE-MESTRA.md:38;app/docs/BACKLOG.md:819	—	—	—
BO-ADMIN-2710	3	JUSTIFICATIVA-PAGA-O-PERIODO	ESPINHA A FAZER	app/docs/BACKLOG.md:1918;app/docs/BACKLOG.md:9;app/docs/GRADE-MESTRA.md:269	sim	sim	não, nas linhas citadas
BO-ADMIN-2710	5	JUIZ-DO-PAR-DUAS-ENTRADAS	NO AR	app/docs/BACKLOG.md:819	—	—	—
BO-ADMIN-2710	6	FIO-LE-O-JUIZ	NO AR	app/docs/BACKLOG.md:837	—	—	—
BO-ADMIN-2710	7	REGISTRADO-NAO-E-VIVO	NO AR	app/docs/BACKLOG.md:823;app/docs/RELATO.md:3371;app/docs/GRADE-MESTRA.md:139	não	sim	não, nas linhas citadas
BO-ADMIN-2710	8.1	TRILHA-DO-VINCULO-MENTE	NO AR	app/docs/BACKLOG.md:821;app/docs/GRADE-MESTRA.md:138	sim	sim	não, nas linhas citadas
```

Cen/lei seguem as duas primeiras casas da grade: `+` feito, `.` falta. “Toca congelado” considera somente os arquivos citados nas linhas das obras. app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1102; app/docs/BACKLOG.md:1239; app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:823; app/docs/BACKLOG.md:821.

Ressalvas do cruzamento:

- **HJ-005-U1:** BO-ADMIN-2710 registra medição e encaminhamento; seu `fecha BO=3` agregado não individualiza cobertura de 01/10. A COBPARC nomeia col42 em 08/15/22-10. Por isso U1 permanece candidata sem prova, contada como SEM OBRA. app/docs/BACKLOG.md:820; app/docs/GRADE-MESTRA.md:137; app/docs/BACKLOG.md:829.
- **HJ-017-U1:** O232 combina a indicação do mapa fornecido com o caso nominal col923 e `fecha BO=1` do mapa HJ. O dia 27/09 não aparece na linha da obra: cobertura específica dessa data [nao verificado]. O complemento da ata permanece pendente; não foi contado como NO AR pelos pousos A/B. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853.
- **Estados divergentes:** a COBPARC tem fechamento posterior ao PARADA da grade; JUSTIFICATIVA-PAGA-O-PERIODO voltou à ordem viva e recebe o item 3, expressamente separado da obra A. app/docs/GRADE-MESTRA.md:39; app/docs/RELATO.md:8127; app/docs/BACKLOG.md:993; app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:9.
- **BO-ADMIN/2:** a prova nas fontes autorizadas é pela classe de divergência entre motor e ata; o encaminhamento nominal do caso está registrado no relato. app/docs/GRADE-MESTRA.md:38; app/docs/BACKLOG.md:819; app/docs/RELATO.md:3423.

(B) Coluna `resolve` por obra

| obra_id | resolve | Prova |
|---|---|---|
| COBERTURA-PARCIAL-UM-JUIZ | HJ-005-U2; HJ-005-U4; BO-ADMIN-2710/1 | app/docs/BACKLOG.md:829 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | BO-ADMIN-2710/2; BO-ADMIN-2710/5 | app/docs/GRADE-MESTRA.md:38; app/docs/BACKLOG.md:819 |
| FIO-LE-O-JUIZ | HJ-030-U1; BO-ADMIN-2710/6 | app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:837 |
| REGISTRADO-NAO-E-VIVO | BO-ADMIN-2710/7 | app/docs/BACKLOG.md:823 |
| TRILHA-DO-VINCULO-MENTE | BO-ADMIN-2710/8.1 | app/docs/BACKLOG.md:821 |
| JUSTIFICATIVA-PAGA-O-PERIODO | BO-ADMIN-2710/3 | app/docs/BACKLOG.md:1918 |
| O232 | HJ-017-U1 | app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853 |
| BO2910-ESTRUTURA | BO2910-ESTRUTURA/01; /02; /05; /06; /08 | app/docs/BACKLOG.md:1102; app/docs/GRADE-MESTRA.md:170 |

(C) QUADRO

| Parcela | Casos | Lastro |
|---|---:|---|
| NO AR | 9 | app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/BACKLOG.md:819; app/docs/BACKLOG.md:837; app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:823; app/docs/RELATO.md:3371; app/docs/BACKLOG.md:821 |
| EM VOO | 0 | Nenhum vínculo acima possui esse estado; fontes das respectivas linhas em (A). |
| ESPINHA A FAZER | 1 | app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:9 |
| LATERAL PRONTA | 1 | O232: cen+lei, nenhum congelado citado. app/docs/GRADE-MESTRA.md:853; app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1239 |
| LATERAL NÃO PRONTA | 5 | BO2910-ESTRUTURA: cen não fechado. app/docs/GRADE-MESTRA.md:170; app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1102 |
| SEM OBRA | 2 | HJ-005-U1: candidata sem prova; HJ-012-U2: cobertura [nao verificado]. app/docs/BACKLOG.md:820; app/docs/RELATO.md:8400 |

**ESTRUTURA: 18 = 9 + 0 + 1 + 1 + 5 + 2.** Soma dos casos discriminados em (A), com as provas acima. Encerramento atual dos casos em produção: **[nao verificado]**.

## 14/11 18:50 · sessao 58e554e940f1 · E3-X2-NUNCA-JULGADAS · censo · origem=Code

**E3-X2-NUNCA-JULGADAS — leitura**

A lista contém **136 colab-dias únicos**. Quantos são nunca lavrados, vencidos ou indeterminados, e os exemplos de cada estado, permanecem **[nao verificado]**: o anexo não contém `veredito_lavrado`, `julgado_em`, `janela_expira_em`, marcos ou perguntas. Fonte: anexo `u136.tsv:1–137` — numeração considerando o cabeçalho como linha 1.

| Empresa | Competência | Colab-dias | Fonte no anexo |
|---|---|---:|---|
| 1 | 10/2026 | 17 | `u136.tsv:2–18` |
| 1 | 11/2026 | 20 | `u136.tsv:19–38` |
| 2 | 09/2026 | 56 | `u136.tsv:39–94` |
| 2 | 10/2026 | 21 | `u136.tsv:95–115` |
| 2 | 11/2026 | 9 | `u136.tsv:116–124` |
| 3 | 09/2026 | 9 | `u136.tsv:125–133` |
| 3 | 10/2026 | 4 | `u136.tsv:134–137` |

**O recorte exige uma ressalva.** Os 136 representam o recorte sem dono informado. Não representam necessariamente todos os dias com `t['nao_sei'] == True`: a escolha do dono precede o fallback `nao_sei`, e uma porta publicada conserva essa flag. Excluir os outros 390 é coerente com “sem dono”, mas reduz o universo se o item 4 pretendia **todos** os dias com a flag. O número 390 não foi recontado aqui **[nao verificado]**. Fontes: `app/chamados/services/tarefa_do_dia.py:604–614`, `:629–636`; anexo `leitura_nao_fica_ociosa_complemento.txt:3`.

**Como nasce o desconhecimento**

A implementação de `esta_stale` está em `app/chamados/veredito_lavrado.py:213`. A carga admite em `_lavra` somente valores `sim/nao` que não estejam stale. Quando a última decisão é validação e a pergunta ficou fora desse mapa, `_pela_decisao` devolve `veredito_sem_lavra_vigente`; a tarefa então marca `nao_sei=True`. Fontes: `app/chamados/services/tarefa_do_dia.py:153–156`, `:320–325`, `:419–422`.

| Situação da pergunta | Ramo real |
|---|---|
| Campo vazio ou `julgado_em` ausente | `esta_stale=True`; `app/chamados/veredito_lavrado.py:224–225` |
| Lavra posterior ao instante consultado | Também stale; não prova que nunca houve lavra. `app/chamados/veredito_lavrado.py:224–225` |
| Relógio venceu a janela e a lavra antecede a expiração | Stale. A comparação usa datas locais. `app/chamados/veredito_lavrado.py:114–116`, `:228–229` |
| Marco do colaborador posterior à lavra | Stale. `app/chamados/veredito_lavrado.py:230–233` |
| Nenhum desses motivos | `esta_stale=False`. `app/chamados/veredito_lavrado.py:234` |
| Valor persistido `indeterminado` | Nasce quando o juiz lança exceção; não é, sozinho, motivo de stale. `app/chamados/veredito_lavrado.py:261–269`, `:224–234` |

`nao_sei` também nasce fora da lavra: célula ausente, ata sem lâmpadas, lâmpada desconhecida, marco sem teto, identidade insuficiente e outros retornos sem decisão. Portanto, não cabe converter automaticamente os 136 em “perguntas nunca julgadas”. Fontes: `app/chamados/services/tarefa_do_dia.py:216–226`, `:140–141`, `:173`, `:284–290`, `:305–310`, `:322–325`.

O próprio anexo contém **20 dias do colab 924 sem célula identificada**; isso não comprova ausência de pergunta ou de lavra. Fonte: `u136.tsv:74–93`; situação das perguntas desses dias **[nao verificado]**.

**Quem deveria lavrar**

| Estado/evento | Rotina e alcance demonstrado |
|---|---|
| Nunca lavrado ou vencido, com destino vivo | `lavrar_perguntas_stale`: seleciona perguntas cujo `chamado_destino` está em `VIVOS`, limita a rodada a 500 por padrão e chama `lavrar(p)` com `--apply`. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `:57–62`, `:82–89` |
| Papel do cron | Cadastrado a cada cinco minutos, com `--apply`, estágio `continuo`, sem dependências; papel **`lavra`**. Cadastro não comprova execução. `app/config/crons.py:390–391`, `:2264`; execução **[nao verificado]** |
| Indeterminado com carimbos vigentes | Não há nova tentativa nesse cron apenas pelo valor `indeterminado`; precisa entrar por algum motivo de stale. `app/chamados/services/raiox.py:40–47` |
| Materialização | O materializador chama `carimbar(..., lavrar_veredito=True)`; a porta chama `lavrar(salvar=False)` e salva os campos juntos. `app/chamados/services/materializacao.py:1283–1284`; `app/chamados/models.py:3232–3235` |
| Fechamento do chamado | O signal S143 aciona o resolvedor; este seleciona perguntas do destino, não materializadas e com disputa aberta, e pode carimbar com lavra. `app/chamados/signals.py:91–117`; `app/chamados/services/disputa_emissao.py:1913–1917`, `:1993–1994` |
| Mudança do estado do destino | `_gravar_estado` relavra no ramo de cruzamento da fronteira de encerramento, para perguntas daquele destino sem validação. `app/chamados/models.py:303–312` |
| Reapontamento, mudança de dia ou via derivada | Relavram junto da alteração correspondente. `app/chamados/services/fio.py:964–967`, `:1081–1082`; `app/chamados/services/relavra_via.py:118–128` |
| Backfill | É sob ordem, não rotina; seu recorte compara o valor do veredito. `app/config/crons.py:1420`; `app/chamados/management/commands/backfill_veredito_lavrado.py:115–132` |

Assim, sem os destinos e campos das perguntas, atribuir falha individual a cron, signal ou porta permanece **[nao verificado]**. O leitor GET não é o lavrador atual. Fonte: `app/chamados/services/raiox.py:112–119`.

**Sonda proposta**

A sonda abaixo usa o mesmo universo, janela, `_carga` e chamadas de tarefa da remedida; reutiliza as perguntas carregadas e chama `esta_stale` real. Fontes: `app/folha/fora_da_folha.py:58–79`, `:109–144`; `app/chamados/services/tarefa_do_dia.py:392–422`, `:533–560`.

Para não inventar prioridade entre perguntas, o dia recebe `nunca_lavrado` ou `vencido` somente quando suas perguntas causais concordam. Ausência de causa de lavra exposta ou causas mistas ficam em `indeterminado`, identificadas como **classificação não resolvida [nao verificado]**, separadamente do valor persistido `VL_INDETERMINADO`. A necessidade dessa distinção decorre dos vários ramos de `nao_sei`. Fonte: `app/chamados/services/tarefa_do_dia.py:216–325`.

O congelamento abrange o shell porque o universo contém `localdate()` implícito. O bootstrap desativa os handlers de arquivo antes do Django iniciar e usa `-B`. Fontes: `app/folha/export.py:1238`; `app/requirements.txt:43`; `app/config/settings/saas.py:136–166`. Execução no ambiente de banco e resultados: **[nao verificado]**.

```bash
# Executar a partir de app/. Saida apenas em stdout; sem redirecionamento.
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import datetime as dt
import importlib
import json
import os
import sys
from collections import Counter, defaultdict

# app/manage.py:9; app/config/settings/saas.py:136,160.
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.saas")
os.environ["PGOPTIONS"] = (
    os.environ.get("PGOPTIONS", "") + " -c default_transaction_read_only=on"
).strip()
config = importlib.import_module(os.environ["DJANGO_SETTINGS_MODULE"])
config.LOGGING_CONFIG = None

# Anexo u136.tsv:2-137. Cada linha agrupa datas do mesmo emp/comp/colab.
DADOS = """1 10/2026 950 2026-09-29 2026-10-08 2026-10-09 2026-10-14 2026-10-15 2026-10-16 2026-10-19 2026-10-20
1 10/2026 969 2026-10-07 2026-10-08 2026-10-09 2026-10-13 2026-10-14 2026-10-15 2026-10-16 2026-10-18 2026-10-20
1 11/2026 950 2026-10-21 2026-10-22 2026-10-23 2026-10-27 2026-10-30 2026-11-03 2026-11-11 2026-11-12 2026-11-13
1 11/2026 969 2026-10-22 2026-10-24 2026-10-26 2026-10-28 2026-10-30 2026-11-01 2026-11-03 2026-11-05 2026-11-07 2026-11-09 2026-11-13
2 09/2026 192 2026-08-23
2 09/2026 207 2026-08-22 2026-08-26 2026-08-30 2026-09-05 2026-09-13 2026-09-19
2 09/2026 209 2026-08-21 2026-08-23
2 09/2026 268 2026-08-21 2026-08-23
2 09/2026 301 2026-08-23
2 09/2026 327 2026-08-23
2 09/2026 382 2026-08-27
2 09/2026 435 2026-09-20
2 09/2026 438 2026-09-07
2 09/2026 439 2026-09-15
2 09/2026 451 2026-08-21 2026-08-23
2 09/2026 468 2026-09-17
2 09/2026 491 2026-09-20
2 09/2026 556 2026-09-14
2 09/2026 726 2026-09-08
2 09/2026 789 2026-08-26 2026-09-01 2026-09-13
2 09/2026 822 2026-08-28
2 09/2026 849 2026-09-18 2026-09-19
2 09/2026 915 2026-09-07
2 09/2026 921 2026-09-05 2026-09-12 2026-09-19
2 09/2026 922 2026-09-18
2 09/2026 923 2026-09-06
2 09/2026 924 2026-09-01 2026-09-02 2026-09-03 2026-09-04 2026-09-05 2026-09-06 2026-09-07 2026-09-08 2026-09-09 2026-09-10 2026-09-11 2026-09-12 2026-09-13 2026-09-14 2026-09-15 2026-09-16 2026-09-17 2026-09-18 2026-09-19 2026-09-20
2 09/2026 953 2026-09-16
2 10/2026 207 2026-10-10
2 10/2026 243 2026-09-27
2 10/2026 252 2026-09-29
2 10/2026 297 2026-09-21
2 10/2026 821 2026-09-30 2026-10-10 2026-10-12
2 10/2026 841 2026-10-10
2 10/2026 849 2026-10-02 2026-10-03
2 10/2026 922 2026-09-29 2026-10-17
2 10/2026 924 2026-09-22
2 10/2026 957 2026-09-21
2 10/2026 988 2026-10-10 2026-10-15 2026-10-16 2026-10-18 2026-10-19
2 10/2026 991 2026-10-11 2026-10-14
2 11/2026 265 2026-10-31 2026-11-14
2 11/2026 407 2026-10-21 2026-10-29 2026-11-02
2 11/2026 516 2026-10-22
2 11/2026 673 2026-10-21
2 11/2026 821 2026-10-24
2 11/2026 922 2026-10-23
3 09/2026 129 2026-08-28 2026-08-30 2026-09-08 2026-09-13 2026-09-15 2026-09-20
3 09/2026 515 2026-09-09
3 09/2026 829 2026-08-25 2026-09-15
3 10/2026 155 2026-09-27 2026-10-04 2026-10-11 2026-10-12"""

AGORA = dt.datetime.fromisoformat("2026-11-14T18:45:00+00:00")
ESTADOS = ("nunca_lavrado", "vencido", "indeterminado")


def sonda():
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from folha import fora_da_folha as F
    from colaboradores.models import Empresa
    from chamados.catalogo.motor import VIVOS
    from chamados.veredito_lavrado import (
        esta_stale, marcos_do_lote, relogio_virou,
    )
    from ponto.janelas import janela_fechamento

    alvos = []
    for linha in DADOS.splitlines():
        emp, comp, cid, *dias = linha.split()
        alvos.extend(
            (int(emp), comp, int(cid), dt.date.fromisoformat(d))
            for d in dias
        )
    assert len(alvos) == len({
        (cid, d) for _, _, cid, d in alvos
    }) == 136

    def j(obj):
        return json.dumps(
            obj, ensure_ascii=False, sort_keys=True, default=str,
        )

    def diagnostico(p, marcos):
        # Funcao REAL: app/chamados/veredito_lavrado.py:213.
        stale = esta_stale(p, marcos, AGORA)
        if not p.veredito_lavrado or p.julgado_em is None:
            estado = "nunca_lavrado"
            motivo = "campo_vazio_ou_julgado_ausente"
        elif stale:
            estado = "vencido"
            if p.julgado_em > AGORA:
                motivo = "lavra_posterior_a_agora"
            elif relogio_virou(
                p.julgado_em, p.janela_expira_em, AGORA,
            ):
                motivo = "relogio"
            else:
                motivo = "marco_posterior"
        elif p.veredito_lavrado not in ("sim", "nao"):
            estado = "indeterminado"
            motivo = "valor_da_lavra:" + p.veredito_lavrado
        else:
            estado, motivo = "vigente", "lavra_vigente"

        destino = p.chamado_destino
        no_cron = bool(destino and destino.status_local in VIVOS)
        return {
            "pergunta": p.pk,
            "motivo": p.motivo,
            "instancia": p.instancia_marco,
            "estado": estado,
            "diagnostico": motivo,
            "esta_stale": stale,
            "veredito_lavrado": p.veredito_lavrado,
            "vl_indeterminado_gravado":
                p.veredito_lavrado == "indeterminado",
            "julgado_em": p.julgado_em,
            "janela_expira_em": p.janela_expira_em,
            "marco": marcos.get(p.disputa.colaborador_id),
            "chamado_destino": p.chamado_destino_id,
            "status_destino":
                destino.status_local if destino else None,
            "no_universo_cron": no_cron,
            "elegivel_cron_stale": no_cron and stale,
            "fonte": (
                "app/chamados/veredito_lavrado.py:224; "
                "app/chamados/management/commands/"
                "lavrar_perguntas_stale.py:57"
            ),
        }

    resultados = []
    # Mesma carga/janela da remedida: fora_da_folha.py:58.
    with timezone.override("America/Sao_Paulo"):
        with schema_context("juliani"), transaction.atomic():
            with connection.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL "
                    "REPEATABLE READ, READ ONLY"
                )
                cur.execute("SHOW transaction_read_only")
                assert cur.fetchone()[0] == "on"
                cur.execute("SELECT current_schema()")
                assert cur.fetchone()[0] == "juliani"

            for emp_id, comp in sorted({
                (e, c) for e, c, _, _ in alvos
            }):
                mes, ano = map(int, comp.split("/"))
                emp = Empresa.objects.get(pk=emp_id)
                u = F.dias_que_seguram(emp, mes, ano)
                colabs, por = u["colabs"], u["dias"]
                retidos = [colabs[c] for c in por if por[c][1]]
                carga = (
                    F._carga(
                        retidos,
                        *janela_fechamento(mes, ano, emp),
                        AGORA,
                    )
                    if retidos else None
                )
                ids = {c.pk for c in retidos}
                marcos = (
                    marcos_do_lote(ids, ate=AGORA) if ids else {}
                )
                universo = {
                    (cid, d["data"])
                    for cid, (_, ds) in por.items()
                    for d in ds
                }

                for e, c, cid, dia in alvos:
                    if (e, c) != (emp_id, comp):
                        continue

                    r = {
                        "emp": e, "comp": c,
                        "colab": cid, "dia": dia,
                        "no_universo_atual": (cid, dia) in universo,
                        "estado": "indeterminado",
                        "perguntas": [],
                        "diagnostico":
                            "colab_fora_da_carga:[nao verificado]",
                        "fio": None,
                        "nao_sei": None,
                        "confere_recorte": False,
                    }

                    if carga is not None and cid in ids:
                        # As mesmas duas chamadas da remedida.
                        fio = F._fio_do_dia(
                            colabs[cid], dia, carga,
                        )
                        t = carga.tarefa(colabs[cid], dia)

                        causais = {
                            i["pergunta_id"]
                            for i in t["instancias"]
                            if i.get("razao")
                                == "veredito_sem_lavra_vigente"
                            and i.get("pergunta_id") is not None
                        }
                        # Objetos reais da carga:
                        # tarefa_do_dia.py:392-398.
                        ps = carga._perg.get((cid, dia), [])
                        diags = [
                            diagnostico(p, marcos) for p in ps
                        ]
                        escolhidas = [
                            p for p in diags
                            if p["pergunta"] in causais
                        ]
                        estados = {
                            p["estado"] for p in escolhidas
                        }

                        if (
                            escolhidas
                            and len(estados) == 1
                            and estados <= set(ESTADOS)
                            and len(escolhidas) == len(causais)
                        ):
                            r["estado"] = next(iter(estados))
                            r["diagnostico"] = (
                                "lavra_indeterminada"
                                if r["estado"] == "indeterminado"
                                else "perguntas_causais_concordam"
                            )
                        elif escolhidas:
                            r["diagnostico"] = (
                                "causas_mistas_ou_incompletas:"
                                "[nao verificado]"
                            )
                        else:
                            r["diagnostico"] = (
                                "nao_sei_sem_causa_de_lavra_exposta:"
                                "[nao verificado]"
                            )

                        r.update(
                            fio=fio,
                            fio_tarefa=t["fio"],
                            nao_sei=t["nao_sei"],
                            razao=t["razao"],
                            razoes_instancias=[
                                {
                                    "pergunta": i["pergunta_id"],
                                    "razao": i["razao"],
                                }
                                for i in t["instancias"]
                            ],
                            perguntas=escolhidas,
                            perguntas_do_dia=diags,
                            confere_recorte=(
                                r["no_universo_atual"]
                                and fio == "nada"
                                and t["nao_sei"]
                            ),
                        )
                    resultados.append(r)

    # Erro de carga/banco aborta: nao vira contagem aparente.
    assert len(resultados) == 136

    print("**E3-X2-NUNCA-JULGADAS — sonda de leitura**")
    print("\nAGORA=" + AGORA.isoformat() + "; tenant=juliani.")
    print(
        "\nFonte da carga: app/folha/fora_da_folha.py:58; "
        "fonte da tarefa: "
        "app/chamados/services/tarefa_do_dia.py:533; "
        "fonte stale: app/chamados/veredito_lavrado.py:213."
    )
    print(
        "\nIndeterminado do DIA inclui falta de classificacao "
        "unica; o valor persistido fica separado em "
        "veredito_lavrado. Convencao diagnostica "
        "[nao verificado]."
    )

    print(
        "\n| emp | comp | colab | dia | estado | "
        "perguntas causais |"
    )
    print("|---|---|---|---|---|---|")
    for r in resultados:
        pks = ",".join(
            str(p["pergunta"]) for p in r["perguntas"]
        ) or "[nao verificado]"
        print(
            "| {emp} | {comp} | {colab} | {dia} | "
            "{estado} | ".format(**r) + pks + " |"
        )

    print("\nDetalhes por dia, sem nomes, CPF ou respostas:")
    print("\n~~~jsonl")
    for r in resultados:
        print(j(r))
    print("~~~")

    cont = Counter(
        (r["estado"], r["comp"], r["emp"])
        for r in resultados
    )
    exemplos = defaultdict(list)
    for r in resultados:
        chave = r["estado"], r["comp"], r["emp"]
        if len(exemplos[chave]) < 3:
            exemplos[chave].append({
                "colab": r["colab"],
                "dia": r["dia"],
                "perguntas": [
                    p["pergunta"] for p in r["perguntas"]
                ],
                "diagnostico": r["diagnostico"],
            })

    print(
        "\n| estado | comp | emp | colab-dias | "
        "ate 3 exemplos |"
    )
    print("|---|---|---|---|---|")
    for comp, emp in sorted({
        (c, e) for e, c, _, _ in alvos
    }):
        for estado in ESTADOS:
            k = estado, comp, emp
            print(
                "| %s | %s | %s | %s | %s |" % (
                    estado, comp, emp,
                    cont[k], j(exemplos[k]),
                )
            )

    print(
        "\nSubtipos de indeterminado "
        "(nao confundir com VL_INDETERMINADO):"
    )
    for motivo, n in sorted(Counter(
        r["diagnostico"] for r in resultados
        if r["estado"] == "indeterminado"
    ).items()):
        print("- %s: %s" % (motivo, n))

    print("\nE3X2_TOTAL " + j(dict(Counter(
        r["estado"] for r 

[truncado em 20000 de 20915 caracteres]

## 14/11 18:49 · sessao 505e81710761 · - · - · origem=Ronald (tmux)

Estatística independente do anexo, agregada só em memória:

| Empresa | 09/2026 | 10/2026 | 11/2026 | Total |
|---|---:|---:|---:|---:|
| 3 | 94 | 117 | 156 | 367 |
| 4 | 6 | 8 | 9 | 23 |

Fonte: `universo_dias.tsv` do prompt; sem arquivo local numerado, portanto `[nao verificado]` para citação arquivo:linha.

**Correção certa:** empresa 2, competência 10, col607 tem **30 linhas**, de 21/09 a 20/10 consecutivos; contar 10 omite as 20 linhas de outubro. Fonte: anexo do prompt `[nao verificado]`.

Meu mapa preliminar empresa 2/10 soma **615**, encaminhado integralmente para comparação; ainda há diferença de 2 contra a contagem informada de 593 após corrigir col607 (+20). Não ajustei ao universo declarado. `[nao verificado]`

## 14/11 18:49 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

Conferência independente de **emp2 / 11/2026: 790 linhas, 192 colabs**. Contagem por grupos do TSV colado; arquivo materializado `[nao verificado]`.

O mapa da outra agente divergia somente nestes grupos:

- **col371: 1 dia**, `2026-10-30`; ela havia contado 4.
- **col655: 5 dias**, `2026-10-22`, `2026-10-26`, `2026-11-02`, `2026-11-09`, `2026-11-10`; ela havia contado 9.

Assim, `797 − 3 − 4 = 790`, coincidente com minha transcrição independente. Fonte: anexo `universo_dias.tsv` colado; números de linha `[nao verificado]`.

Maiores grupos dessa fatia: col667=25; col915=25; col607=24; col717=24; col317=20; col921=20; col926=19; col534=16; col532=14; col349/696/820/832/967=13 cada. Fonte: anexo colado; números de linha `[nao verificado]`.

A conferência cobre somente emp2/11; **não ajustei ao total declarado de 2381**. Demais fatias `[nao verificado]` por esta conferência.

## 14/11 18:49 · sessao d4893d2abeca · - · - · origem=Ronald (tmux)

| Caso | Vínculo recomendado | Prova e estado |
|---|---|---|
| HJ-005-U1 | **candidata sem prova → SEM OBRA** | BO-ADMIN-2710 apenas mede/encaminha; não demonstra cobertura de col42 em 01/10. COBPARC nomeia 08/15/22-10 e ausência parcial, característica não demonstrada para U1 pelo anexo. app/docs/BACKLOG.md:820; app/docs/BACKLOG.md:829; app/docs/GRADE-MESTRA.md:137 |
| HJ-005-U2 | **COBERTURA-PARCIAL-UM-JUIZ — NO AR** | Nomeia col42 em 08/10 e FATO_EM_AUSENCIA. Pouso posterior ao PAROU antigo documentado no BACKLOG; fechamento explícito no RELATO. app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/RELATO.md:8127 |
| HJ-005-U4 | **COBERTURA-PARCIAL-UM-JUIZ — NO AR** | Nomeia col42 em 15/10; mesmas provas de estado. app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/RELATO.md:8127 |
| HJ-012-U2 | **nenhuma obra registrada — SEM OBRA** | O mapa fornecido diz sem obra. A referência a col239 no feriado apresenta cobrança legítima encaminhada ao DP, sem obra de cobertura demonstrada; preservar dono ESTRUTURA imposto pelo universo. app/docs/BACKLOG.md:1541; app/docs/RELATO.md:8390. Ausência de outra cobertura: [nao verificado]. |
| HJ-017-U1 | **O232 — LATERAL PRONTA** | O mapa fornecido indica O232; a obra nomeia col923, borda/ata e dias estruturais, embora não individualize 27/09. A GRADE confirma uma unidade do mapa HJ e mantém complemento da ATA pendente. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853 |
| HJ-030-U1 | **FIO-LE-O-JUIZ — NO AR** | Encaminhamento exato “col94 16/10 já em FIO-LE-O-JUIZ”; obra registra NO AR 7c9e4c27 e caso col94/#44764/#6984. app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:837 |

O232: **cen=sim; lei=sim**, pela grade `+++....++` e legenda. **toca_congelado=não nas linhas examinadas**: aparecem cartorio.py, escala/utils.py, ponto/turnos.py e teste; nenhum dos seis congelados. A colisão com `ponto/turnos.py` não altera esse campo nem o critério literal de LATERAL PRONTA solicitado. app/docs/GRADE-MESTRA.md:7; app/docs/GRADE-MESTRA.md:853; app/docs/BACKLOG.md:1239.

Ressalva de interpretação: O232 tem prova nominal de col923 + vínculo fornecido no mapa, mas não dia 27/09 no registro da obra; se a consolidação exigir também igualdade de data, marcar **candidata sem prova**. O critério do usuário permite caso, classe **ou** sítio; por isso recomendo aceitar o vínculo sem afirmar resolução efetiva em produção. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853.

## 14/11 18:48 · sessao 98db6cf4ab8a · - · - · origem=Ronald (tmux)

| Caso BO-ADMIN-2710 | Obra | Prova de cobertura | Estado documentado |
|---|---|---|---|
| 1 | COBERTURA-PARCIAL-UM-JUIZ | `app/docs/BACKLOG.md:829` nomeia item 1, col42 e `supra_juiz` RAMO 2 | GRADE registra PARADA (`app/docs/GRADE-MESTRA.md:39`); BACKLOG registra deploy posterior COBPARC `e1c08457`, 02/11 (`app/docs/BACKLOG.md:993`). Divergência documental. |
| 2 | JUIZ-DO-PAR-DUAS-ENTRADAS | Classe “motor e ata formarem os mesmos turnos” em `app/docs/GRADE-MESTRA.md:38`; mesma batida/marco produzindo turnos diferentes em `app/docs/BACKLOG.md:819`. Vínculo nominal col789 e `_match_marcos` em `app/docs/CORTES.md:340`. | NO AR `b3bf529e`, com resíduos (`app/docs/BACKLOG.md:819`). |
| 3 | JUSTIFICATIVA-PAGA-O-PERIODO | Caso RT-04 col789 11/10 e separação da obra A explícitos (`app/docs/BACKLOG.md:1918`). | ESPINHA A FAZER por ordem viva (`app/docs/BACKLOG.md:9`; `app/docs/BACKLOG.md:1918`), embora balde LATERAL na GRADE (`app/docs/GRADE-MESTRA.md:269`). |
| 5 | JUIZ-DO-PAR-DUAS-ENTRADAS | RT-02 col247 24/10, pergunta errada de intervalo explicitada (`app/docs/BACKLOG.md:819`). | NO AR `b3bf529e` (`app/docs/BACKLOG.md:819`). |
| 6 | FIO-LE-O-JUIZ | Carregar perguntas vivas independentemente do estado da disputa (`app/docs/BACKLOG.md:837`). | NO AR `7c9e4c27` (`app/docs/BACKLOG.md:837`). |
| 7 | REGISTRADO-NAO-E-VIVO | Item 7 e chamado #28883 explicitados (`app/docs/BACKLOG.md:823`). | NO AR na linha; hash `475318fa` aparece no RELATO (`app/docs/BACKLOG.md:823`; `app/docs/RELATO.md:3371`). |
| 8.1 | TRILHA-DO-VINCULO-MENTE | Item 8.1 explicitado (`app/docs/BACKLOG.md:821`). | NO AR `5e9a0a51`; resíduos RT-03 são outros problemas (`app/docs/BACKLOG.md:821`). |

A parte de justificativa do item 3 saiu expressamente da COBERTURA-PARCIAL para JUSTIFICATIVA-PAGA-O-PERIODO; não atribuir ambas ao mesmo caso (`app/docs/CORTES.md:352`; `app/docs/BACKLOG.md:1918`).

Para laterais, a convenção das casas é `cen-lei-...`, com `+` feito e `.` falta (`app/docs/GRADE-MESTRA.md:7`):

| Lateral | Cen | Lei | Congelado citado na linha da obra |
|---|---|---|---|
| JUSTIFICATIVA-PAGA-O-PERIODO | sim | sim | não; linha cita motor/efeito de ausência (`app/docs/GRADE-MESTRA.md:269`; `app/docs/BACKLOG.md:1918`). |
| REGISTRADO-NAO-E-VIVO | não | sim | não (`app/docs/GRADE-MESTRA.md:139`; `app/docs/BACKLOG.md:823`). |
| TRILHA-DO-VINCULO-MENTE | sim | sim | não; cita serviços/views de colaboradores e signal de escala (`app/docs/GRADE-MESTRA.md:138`; `app/docs/BACKLOG.md:821`). |

`chamados/services/painel.py`, citado no histórico da obra REGISTRADO, é caminho diferente do congelado `chamados/painel.py` (`app/docs/RELATO.md:3381`).

A linha de prova nominal da obra para item 2 está em CORTES, fora das duas fontes autorizadas para a coluna de prova; nas fontes autorizadas há prova pela classe em GRADE:38/BACKLOG:819. Se a classe for considerada insuficiente, registrar “candidata sem prova”, sem promover CORTES a prova permitida (`app/docs/GRADE-MESTRA.md:38`; `app/docs/BACKLOG.md:819`; `app/docs/CORTES.md:340`).

## 14/11 18:47 · sessao eb0432a81e21 · - · - · origem=Ronald (tmux)

Os cinco casos têm cobertura literal em **BO2910-ESTRUTURA**, cuja coluna `resolve` enumera “casos 01, 02, 05, 06, 08”; balde LATERAL-ESTRUTURA, estado REGISTRADA, grade `.+.......`. Portanto, **cen não; lei sim; LATERAL NÃO PRONTA**. (app/docs/GRADE-MESTRA.md:170; legenda app/docs/GRADE-MESTRA.md:7)

```tsv
caso_fonte	caso_id	obra_id	estado_obra	prova_arquivo:linha	lateral_cen	lateral_lei	toca_congelado
BO2910-ESTRUTURA	01	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	02	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	05	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	06	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	08	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
```

Provas específicas, todas em **app/docs/BACKLOG.md:1102**:

- **01:** `vincular_posto` engole exceção e troca posto sem empresa; sítios `colaboradores/views.py:333-351` e `transferir_posto :1366`.
- **02:** chamado de texto livre sem leitor da célula; sítio `reconciliador.py:336`, `MODULOS_DE_TURNO`.
- **05:** parcial aprovada sem período pela tela; período obrigatório na aprovação/edição.
- **06:** desligamento grava `data_fim` sem conferir vigência; reativação não reabre vínculo.
- **08:** veredito `admin` sobrevive à regeneração da célula com marcos novos; sítio `veredito_celula.py:222-224`.

**Resolve por obra:** BO2910-ESTRUTURA → BO2910-ESTRUTURA/01, /02, /05, /06, /08. (app/docs/GRADE-MESTRA.md:170; app/docs/BACKLOG.md:1102)

**Subtotal:** 5 = NO AR 0 + EM VOO 0 + ESPINHA A FAZER 0 + LATERAL PRONTA 0 + LATERAL NÃO PRONTA 5 + SEM OBRA 0. (app/docs/GRADE-MESTRA.md:170; app/docs/GRADE-MESTRA.md:7)

A referência `BACKLOG:1059` impressa na grade está desatualizada: a linha efetiva da obra nesta cópia é **app/docs/BACKLOG.md:1102**. Nenhum dos seis congelados é citado nessa linha; alteração efetiva futura desses arquivos permanece **[nao verificado]**.

## 14/11 18:47 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

Censo parcial consolidável, todo por leitura; **não sustento fechamento de toda a árvore** com esta fatia. Os caminhos abaixo foram identificados em AST e conferidos no fonte; permissões efetivamente atribuídas em produção permanecem `[nao verificado]`.

**Método executado:** `python3 -B`, `ast.parse`, `ast.walk`, inspeção de `Call`, `FunctionDef` e mapa de `ImportFrom` com `asname`. Leitura de URLs, chamadas e escritores complementar; nenhum arquivo criado.

**Portas especialmente relevantes fora de chamados**

| Ação | Porta e URL | Guarda real | Gravação/trilha |
|---|---|---|---|
| Resolver dia: trabalhou/faltou/justificado | `app/ponto/urls.py:58`; `app/ponto/views.py:3352` | POST + `tem_acao(...,'regularizar_batida')`, `views.py:3355`; alias `_vc` importado em `3354`; guardas de negócio **inline** em `services/veredito_celula.py:220`, `243`, `249`, `309`, `334` | Batida via `registrar_batida`, `:323`; Ausencia aprovada via `criar_ausencia`, `:337`; perguntas via `fechar_por_admin`, `:190`; decisão SILENCIO, `:195`; chamado reconciliado e ocorrência, `:203`; `CelulaDia.lavrar_veredito`, `:352`; LogAuditoria, `:356`. |
| Inverter batida | `urls.py:59`; `views.py:3300` | View `:3303`; guardas inline `flip_batida.py:15` retratada, `:19` autor, `:22` `competencia_fechada`, `:26` tipo | `Batida.tipo`, via `registro_batida.anotar`, `flip_batida.py:30`; trilha antes/depois e recompute `:39`. |
| Retratar batida | `urls.py:61`; `views.py:3314` | `_retratar` → `retratar_humano.retratar`, import `:3316`; `_pode` chamável `retratar_humano.py:131`; motivo/ativo inline `:166`, `:170` | `retratar_batida`, `:179`, motivo estruturado `:181`; efeito no dia. |
| Desfazer retratação | `urls.py:62`; `views.py:3326` | `_desfazer` → `retratar_humano.desfazer`, import `:3328`; `_pode`, `:194`; retratada/origem/prazo inline `:197`, `:199`, `:203`; `pode_desfazer(batida,agora=AGORA)` existe em `:105` mas **não cobre a guarda toda** | `Batida.aprovada=True,retratada_em=None`, `:207`, trilha via `anotar`. |
| Aprovar/rejeitar justificativa | `urls.py:40`, `43`; `views.py:1914`, `1926` | POST + `avaliar_justificativas`, `:1916`, `:1928`; executores `justificativa.py:132`, `262`; guardas inline | Aprovação `status/aprovada_por/aprovada_em/batida_gerada`, `justificativa.py:204`; Batida S, `:188`; invalida fechamento `:215`; encerra origem `:217`; trilha `:230`. Rejeição campos `:283`, chamado `:291`. |
| Aprovar justificativas em lote | `urls.py:41`; `views.py:1766` | Inline `:1769`, pendente `:1783`, `:1797`, batida recusada `:1825`, saída já utilizada `:1829` | Escrita **própria** `Justificativa.status/aprovada_por/aprovada_em`, `:1799`; batida S `:1818`; `batida_gerada` `:1834`; invalidação `:1808`; encerra origem `:1840`. |
| Rejeitar justificativas em lote | `urls.py:42`; `views.py:1862` | Inline POST/permissão `:1865`, pendente `:1877` | Campos próprios `status/aprovada_por/aprovada_em/motivo_rejeicao`, `:1878`; save `:1882`; notificações. Trilha própria de sucesso não observada nessa função `[nao verificado]`. |
| Materializar saída retroativa | `urls.py:44`; `views.py:3146` | `materializar_saida` `:3158`; hora e motivo mínimo `:3173`, `:3175`; executor `justificativa.materializar_saida_dp`, `:3185` | Executor pode registrar saída e justificativa; campos completos `[nao verificado]`. |
| Lançar ausência | `urls.py:57`; `views.py:2400` | `editar_ausencias`, `:2402`; `corpo_chegou_inteiro`, `:2431`; guardas inline `ausencia.criar_ausencia`, `ausencia.py:242`, `247`, `249`, `261` e sobreposição posterior | Ausencia aprovada `views.py:2447`; afastamento altera `Colaborador.situacao`, `:2465`. |
| Operar ausência existente | `urls.py:54`–`56`, `65`–`76`; funções em `views.py:2279`, `2320`, `2356`, `2601`, `2631`, `2697`, `2715`, `2758`, `2791` | `_a4_decidir` `views.py:85` chama `ponto.services.ausencia.decidir`; edição e reversão usam serviços respectivos; guardas completas `[nao verificado]` | Estado/range/dados de Ausencia; campos completos por variante `[nao verificado]`. |
| Recalcular/aprovar/reabrir fechamento | `urls.py:31`–`36`; funções `views.py:1229`, `1260`, `1386`, `1437`, `1464`, `1523` | `editar_folha`/POST; `aprovar_colaborador` lê `fechamento_lido` `:1483` e `_classificar_export` alias `folha.export.classificar_export` `:1508`; seleção de travas inline `:1512` | Serviços `fechamento.aprovar_lote:1323`, `aprovar:1335`, `reabrir_lote:1344`, `reabrir:1349`, `trancar_periodo:1400`, `reabrir_periodo:1432`; status/carimbos e período, com efeitos adicionais. |
| Exportar folha | `folha/urls.py:14`; `folha/views.py:102` | Guarda completa `[nao verificado]` | `ExportacaoDominio.registrar`, `aprovar_lote`, `registrar_log` na função. |

O serviço `veredito_celula` **não pode ser chamado por uma sonda somente leitura**: guarda e execução estão misturadas e ele inicia transação/travas em `app/ponto/services/veredito_celula.py:224`, antes da decisão final. Uma sonda não deve copiar essas condições.

**Porta administrativa adicional**

`/admin/` é publicado em `app/config/urls.py:24`. `JustificativaAdmin` deriva diretamente de `admin.ModelAdmin`, sem `SoLeitura`, sem `readonly_fields` e sem override de permissão (`app/ponto/admin.py:18`). Campos editáveis incluem colaborador, turno, batida de entrada, tipo, saída informada, status, aprovador, carimbo, rejeição, batida gerada e chamado de origem (`app/ponto/models.py:136`–`179`). Quem possui as permissões Django em produção `[nao verificado]`; trilha `LogAuditoria` da aplicação por esta porta `[nao verificado]`.

Batida, ImportacaoAEJ e Ausencia herdam `SoLeitura` (`app/ponto/admin.py:11`, `25`, `37`): recusas reais de add/change/delete retornam `False` em `app/core/admin.py:59`, `62`, `65`; todos os campos readonly em `:68`.

**Comandos de escrita identificados por AST**

Caminhos abaixo relativos a `app/chamados/management/commands/`:

| Comando | Chamada de escrita | Guarda/restrição observada |
|---|---|---|
| `curar_perguntas_em_limbo` | `:67 carimbar_resposta_pela_trilha`; `:69 reabrir_para_correcao` | Comparação exata com conjunto MEDIDAS inline `:46`–`53`; nota encontrada `:55`; `--apply` `:64`. |
| `fechar_becos_disputa` | `:57 becos.fechar_beco` | Guarda **chamável** `becos.classificar(d)` `:39`; implementação `services/becos.py:25`–`33`, usa `d.pode_fechar()`. |
| `carimbar_por_batida_coincidente` | `:98 _ctx.renovar_contexto`; `:100 _vinc.apontar_celula`; trilha `:101` | `_ctx` → `chamados.services.contexto`; `_vinc` → `.vinculos` imports `:58`; seleção inline `:60`–`85`. |
| `encerrar_perguntas_com_fato` | `:68 p.encerrar_por_fato_no_espelho`; `:88 d.fechar` | Seleção inline `:31`–`43` exige materializada, não validada, via sem história e `fato_do_marco_no_espelho(p) is True`. |
| `perguntar_chamados_sem_pergunta` | `:29 pergunta_no_app.perguntar` | Guarda integral `[nao verificado]`. |
| `silenciar_chamados_isentos` | `:49 silencio_isentos.silenciar` | Guarda chamável `silencio_isentos.alvos()` `:36`; implementação `services/silencio_isentos.py:20`: flag isento, estado vivo, ainda sem silêncio. |
| `arquivar_prescritos` | `:94 arquivo.silenciar` | Recortes inline `:43`, `49`, `53`, `60`; verbo validar/cobrar/decidir; idade criado_em; não silenciado nem resgatado. |
| `arquivar_sem_dia_irrecuperavel` | `:94 arquivo.silenciar` | Recortes inline `:58`–`68`; escopo DIA, sem dia do fato e anterior à corrente. |
| `arquivar_competencia_encerrada` | `:288 arquivo.silenciar` | `_disputa_viva` chamável `:148`; conjunto/corte integral inline `handle`. |
| `reabrir_orfas_enterradas` | `:157 p.reabrir_fantasma`; `:165 d.reabrir_sistema`; `:172 pai.reabrir` | Recortes inline `:67`–`112`; via fantasma, lâmpada False, pergunta viva, janela fixa do carimbo, exportada fora, recusa/irmãs do pai. |
| `encerrar_por_competencia_trancada` | `:29 competencia_trancada.encerrar` | Guarda completa de planejamento **chamável** `services/competencia_trancada.medir`, `:90`–`185`; `encerrar(...,aplicar=False)` `:188`–`193` só devolve medida. |
| `nomear_marco_por_lampada` | `:109 contexto.renovar_contexto`; evento `:111` | Guardas inline `:47`–`85`; dia apurável, lâmpada apagada e desempate da hora/origem/única. |
| `relavrar_via_i6` | `:102 relavra_via.relavrar` | `planejar` chamável `:44`–`70`; executa `universo_i6()`, vias derivadas, exclusão por ordem, `via_de_hoje`. |
| `backfill_decisoes_pergunta` | `:324 DecisaoPergunta.registrar` | Guardas do apply misturadas à execução; detalhes `[nao verificado]`. |
| `dedup_perguntas_fato` | `:105 aplicar` → `backfill_decisoes_pergunta.aplicar` import `:104` | Grupos `:58`; `plano_de_retratacao` real `:81`; recorte/horizonte/merito/exportada no apply. |
| `lavrar_perguntas_stale` | `:89 veredito_lavrado.lavrar` | Guarda chamável `raiox.pks_stale_por_varredura` `:60`; candidatos pelo destino vivo `:57`–`58`; teto inline `:61`. |
| `backfill_veredito_lavrado` | `:143`, `:146 gravar_lavras_em_lote` | Juiz real `_pergunta_materializada_de_fato` `:105`; divergência e não apagar lavra por indeterminação inline `:120`, `124`. |
| `backfill_fio_morto_e4b` | `_executar_dia:125`: reabrir credor `:135`, registrar decisão `:141`, perguntar `:143`, materializar `:147` | Planejamento chamável `ato_do_dia:82`; usa `T.tarefa_do_dia` e `reabrir_credor_de_pergunta_viva(...executar=False)` `:90`. |
| `validar_classe_a` | `:100 validacao.validar_pergunta` | Recorte inline `:44`–`67`; só respostas sem veredito, classificação real CLASSE_A; autor existente `:90`. |
| `reatar_beco_folga_ao_limbo` | `limbo_folgas.reatar_perguntas_ao_limbo`, import `:50` | Guarda real de seleção `_folga_nao_definida_sem_porta(...agora=AGORA)` `:54`; pai na `qs_na_fila` `:59`; sem pai/fora da fila `:63`–`68`; reversão obrigatória `:39`–`43`. |
| `retratar_chamados_lei_cala` | `:59 retratar` → `chamados.reconciliador.retratar`, import `:29` | Guardas inline `:32`–`47`: dia passado e `classificar_falta(...)[1] is None`. |
| `auditar_chamados_orfaos --fechar` | `:68 transicionar_com_pauta(...ato=ch.reconciliar)`; ocorrência `:73` | Recorte inline `:35`–`44`; gate real dentro da transição. |
| `materializar_passivo_validado` | `:104`, `:113 passivo_validado.reparar` | Mais restrições/autor em `:70`–`83`; **até dry escreve arquivo** `:85`, `:117`–`130`. |
| `geofence_registrados_para_analise` | `:32 ch.analisar` | Guarda integral `[nao verificado]`. |
| `alertar_chamados_sla` | `:44 nota.anexar_ocorrencia`; push `:48` | Guardas reais chamáveis `sla.vencido`, `sla.ja_avisado`, `sla.vence_em` `:32`–`34`; teto da passada inline `:37`. | [nao verificado]

`arquivo.silenciar` grava `ChamadoColaborador.silenciado_ate`, `.silenciado_por`, `.silenciado_motivo`, `.atualizado_em` e trilha `evento` (`app/chamados/services/arquivo.py:156`–`173`). Não muda `status_local`.

`relavra_via.relavrar` grava `PerguntaDisputa.via_resolucao`, `.veredito_lavrado`, `.julgado_em`, `.janela_expira_em` e evento por pergunta (`app/chamados/services/relavra_via.py:118`–`138`).

`competencia_trancada.encerrar` hoje não carimba perguntas (`app/chamados/services/competencia_trancada.py:133`–`135`, `203`). Encerra chamados pela transição (`:209`), anexa ocorrência e evento (`:210`–`219`), fecha disputa pela porta (`:229`), pode criar Pauta DP (`:232`, `256`).

**Cuidados necessários para a sonda**

- Não chamar comandos em dry presumindo ausência de escrita: `materializar_passivo_validado` cria pasta/JSON mesmo sem apply (`:85`, `118`).
- Não chamar serviço mutador e fazer rollback: isso já executa escrita e sinais; não satisfaz a restrição.
- Guardas inline devem sair como **não sondável**, sem contagem inventada.
- `core.portas_da_tela` não fecha este censo: exclui API/admin em `app/core/portas_da_tela.py:24` e detecta POST por texto em `:52`; guardas por regex em `:57`.
- `core.views.painel_operacional` e seu lote são leitura (`app/core/views.py:589`, `621`), não outra porta de decisão.
- `regularizar_retorno` usa `request.user.colaborador` (`app/ponto/views.py:3208`): é ação do próprio colaborador, não uma porta administrativa genérica.
- Leitura completa dos comandos de outros apps, API, escala/cadastro, férias e ações de IA ainda é necessária para declarar **TODA ação**. `[nao verificado]`

## 14/11 18:47 · sessao 505e81710761 · - · - · origem=Ronald (tmux)

Leitura somente; AST executado com `python3 ast.parse` em `app/chamados/{urls,views,views_cobrar,views_arquivar,views_regularizacao}.py` e `services/{fio,acoes_disputa}.py`. `app/chamados/admin.py` está vazio; não encontrei forms Python nesse app. O fechamento global da árvore e os commands ficam com sua consolidação. [nao verificado]

**Método e aliases.** Resolver `ast.ImportFrom` inclusive dentro de funções, `ast.Call(path)` nas URLs e os ramos `acao` dos roteadores; seguir chamadas aos serviços. Aliases importantes:

| Alias | Destino e evidência |
|---|---|
| `vreg` | `chamados.views_regularizacao`, `app/chamados/urls.py:6` |
| `_ac` | `chamados.services.acoes_chamado`, `app/chamados/views.py:423` |
| `_ad` | `chamados.services.acoes_disputa`, `app/chamados/services/fio.py:665` |
| `_ac_geo` | `chamados.services.acoes_chamado`, `app/chamados/services/fio.py:572` |
| `_corrig` | `chamados.models.perguntas_corrigiveis`, `app/chamados/views.py:1419` |
| `_reabrir2` | `chamados.models.reabrir_para_correcao`, `app/chamados/views.py:1435` |
| `_viva_pend` | `chamados.models.pergunta_viva`, `app/chamados/views.py:1415` |
| `_muda_a_folha` | `services.cobranca_folha.muda_a_folha`, `app/chamados/views_cobrar.py:88` |
| `_nota` | `chamados.services.nota`, várias importações, p.ex. `app/chamados/views.py:879` |

**Portas HTTP e roteadores.**

- `/chamados/local/<id>/`: `urls.py:14` → `views.py:359`; POST tem guarda contextual `pode_agir_chamado` em `views.py:419`; `staff` passa `_is_gestor` em `views.py:35`.
- `/chamados/colaborador/<id>/`: `urls.py:21` → `views.py:659`; `_is_gestor` em `:662`; POST delega a `processar_post_fio` em `:674`.
- `/chamados/modal/<id>/`: `urls.py:39` → `views.py:869`; `_is_gestor` em `:872`; usa o mesmo roteador `services/fio.py:523`.
- Roteador fio/modal resolve chamado pertencente ao colaborador e guarda `pode_agir_chamado` em `services/fio.py:601` e `:604`; exige disputa aberta, exceto reabertura administrativa e validação, em `:611`.
- O detalhe exige disputa aberta para validar/recusar/corrigir por filtro, mas o fio permite validar pergunta viva em disputa fechada: `views.py:491`, `:510`, `:529`; `services/fio.py:633`, `:638`, `:644`. Isto é diferença real entre portas.

**Tabela de ações principais.** Todos os nomes de arquivo abreviados abaixo partem de `app/`.

| Ação/porta | Guarda real | Gravação/trilha |
|---|---|---|
| `validar_pergunta`, detalhe/fio/modal | `chamados/services/validacao.py:418` exige carimbo de resposta; `:421` não validada; `:429` implausível exige confirmação; entrada detalhe `views.py:483`, fio `services/fio.py:623` | `_carimbar_validacao`: `validada_em/validada_por` e `DecisaoPergunta.VALIDACAO`, `validacao.py:159`, `:164`; materialização em `:488`, `:494`; logs `:504`, `:517`; recusa de materialização pode desfazer carimbo, `:489` |
| `recusar_pergunta`, detalhe/fio/modal | Entrada `views.py:503`, `services/fio.py:663`; `validacao.py:591` motivo, `:602` não validada, `:605` respondida, `:609` não fora do fato | `recusa_motivo`, `validacao.py:619`, `:625`; reabertura e `DecisaoPergunta` ato `recusa`, `:628`; nota `:633`, evento `:637`, log `:643`, push `:647` |
| `corrigir_resposta`, detalhe/fio/modal | Entrada `views.py:522`, `services/fio.py:663`; `acoes_disputa.py:336` HH:MM, `:338` motivo, `:345` não validada, `:347` disputa aberta, `:349` tipo hora; **não exige resposta anterior** | Nota preserva `'(sem resposta)'` em `:353`, `:359`; core escreve `resposta_colab/respondida_em` em `disputa_emissao.py:1467`, `:1508`, decisão `RESPOSTA` em `:1511`; log `acoes_disputa.py:375`; validação/materialização em `:379`. Pode responder pergunta muda |
| `regularizar_disputa`, detalhe | Entrada `views.py:463`; disputa aberta em `:467`; `acoes_disputa.py:124` resolução, `:130` bloqueia viva sem carimbo de resposta | Materializa validadas em `:139`; log `:144`; fechamento/modelo depende resultado |
| `regularizar_disputa`, fio/modal | `services/fio.py:684`; resolução `:686`; **guarda própria inline** viva sem resposta em `:698` | Materialização `:708`, log `:713`; esta é cópia de guarda, não delega `acao_regularizar_disputa` |
| `reabrir_disputa`, fio/modal | `services/fio.py:679`; `acoes_disputa.py:95` conta pendentes, `:97` chama `perguntas_corrigiveis`; sem corrigíveis recusa em `:98` | `reabrir_para_correcao` em `:103`; nota `:106`, push `:108`, log `:113`; este ramo **não recobra** questionário já pendente |
| Endpoint reabrir/recobrar | URL `chamados/urls.py:20`; `views.py:1372`; gate especial `:1404`: só consulta `pode_agir_chamado` se **não** tem `gerir_chamados`; `_disputa_do_chamado` `:1410`; vivas `:1417`; corrigíveis `:1420` | Havendo corrigíveis, reabre em `:1436`, nota `:1440`, log `:1456`; sem corrigíveis e com pendente viva, chama `recobrar_questionario` em `:1425` |
| Recobrança efetiva | Escolha é inline em `views.py:1421`–`:1434`; serviço em `disputa_emissao.py:1697` não é juiz puro | Renova `ChamadoColaborador.prazo_sla` em `:1725`; nota `:1728`; push `:1735`; log `:1741`; não escreve resposta | [nao verificado]
| Reabertura administrativa de disputa fechada | Fio `:619` → `acoes_disputa.py:235`; `:245` staff/admin_ti; motivo válido `:251`; justificativa ≥20 `:253`; disputa fechada `:255` | `reabrir_admin` `:259`; nova pergunta de conversa `:263`; reabre chamado `:270`; nota `:274`, pushes `:276`/`:287`, log `:297` | [nao verificado]
| Conferir data | URL `urls.py:18`, view `views.py:1292`; POST `:1307`, `gerir_chamados` `:1309`; serviço `validacao.py:686` existente, `:690` justificativa, `:693` `data_do_fato_de`, `:694` divergência, `:697` ainda não conferida | `PerguntaDisputa.data_conferida_em/por`, `validacao.py:699`; save `:701`; log antes/depois `:702` |
| Aplicar declaração de texto | **Só detalhe:** `views.py:478`; `declaracao_texto.py:649` consulta propostas reais pelo followup; `:668` recusa pergunta já validada sem registro | Emite/reusa disputa `:659`; abre pergunta `:666`; core de resposta `:693`; valida com confirmação implausível em `:696` |
| Cobrar chamado | URL `urls.py:25`; `views_cobrar.py:24`; `gerir_chamados` `:25`, POST `:27`, `recusa_de_area` `:32`; throttle por colab/dia `:36`–`:42`; push precisa sucesso `:51` | Push `:43`; nota `[COBRANCA]`, `:55`; **não renova prazo** nesta porta | [nao verificado]
| Cobrar massa | URL `urls.py:31`; `views_cobrar.py:61`; fila filtrada `:71`, verbo `cobrar` `:79`, contextual `:92`, guarda real `_muda_a_folha` `:95`; throttle `:114`; `previa=1` leitor `:101` | Push `:119`; nota `:132`; sem mudança de resposta/prazo | [nao verificado]
| Cobrar dia | URL `urls.py:32`; `views_cobrar.py:306`; POST+`regularizar_batida` `:314`, departamento `:317`, data `:322`; serviço `cobrar_dia.py:28` recusa dia >=hoje | **Log antes do cartório**, `cobrar_dia.py:39`; `cartorio.julgar_celula(...forcar=True,toque=True)` `:41`; possível fábrica de disputa/perguntas `:50`; não é leitura nem mesmo quando resultado posterior recusa |
| Validar inline | URL `urls.py:33`; `views_cobrar.py:145`; permissão `:149`, área `:158`, disputa `:162`–`:167`; só perguntas vivas com `respondida_em`, `:169`; sem elas recusa `:193` | Delega `acao_validar_pergunta`, `:211`; mesmos efeitos da validação |
| Validar lote JSON/tela | URLs `urls.py:34`, `:37`; views `views_cobrar.py:232`, `:372`; gate contextual/permissão `:247`, `_pode_validar_lote` `:334`; classe real `validacao.py:770`, já validada `:767` | Delega `acao_validar_pergunta` `validacao.py:781`; tela acrescenta log `validou_lote_tela`, `views_cobrar.py:383` |
| Arquivar | URL `urls.py:26`; `views_arquivar.py:24`; `gerir_chamados`, POST, área `:26`, `:28`, `:32`; não exige idade no endpoint individual | `arquivo.silenciar` `:37`; `silenciado_ate/por/motivo/atualizado_em`, `services/arquivo.py:166`–`:172`; evento `:173`; nota `views_arquivar.py:44`; não muda `status_local` |
| Arquivar lote | URL `urls.py:28`; `views_arquivar.py:93`; idade >0 `:105`; filtro e verbos `:115`, `:125`; criado antes corte e silêncio nulo `:135`; contexto `:137` | `arquivo.silenciar`, `:140`; evento por chamado; sem nota individual nesta porta |
| Desarquivar | URL `urls.py:27`; `views_arquivar.py:52`; permissão/POST/área `:54`, `:56`, `:60`; via tranca tenta `ch.reabrir(humano=True)` `:70`, que pode recusar | `arquivo.levantar` `:77`, limpa véu e carimba `resgatado_em`; se tranca ressuscita perguntas `:86`; nota `:88`; reabertura pode ter efeitos próprios até na recusa |
| Assumir/liberar | URL `urls.py:41`; `views.py:1008`; `_is_gestor` `:1010`, `pode_agir_chamado` `:1015` | `_vinc=services.vinculos` `:1019`; `assumir_responsavel` `vinculos.py:121`, `atribuido_a/em/atualizado_em` `:132`–`:135`; liberar `:139`; nota `views.py:1027`/`:1031` e log `:1034` só se mudou |
| Mensagem rápida | URL `urls.py:40`; `views.py:961`; `gerir_chamados` `:967`, mensagem não vazia `:971` | `ChamadoColaborador.abrir` `:973` com módulo `mensagem_dp`; nota `:986`; push `:989`; ação é por colaborador, não carimba pergunta/dia | [nao verificado]
| Resposta livre e status | Detalhe `views.py:544` → `acoes_chamado.py:292`; fio `services/fio.py:746`; conteúdo e/ou status válido, contexto já guardado | Nota `acoes_chamado.py:301` / `fio.py:756`; logs `:304`, `:328`; `transicionar_com_pauta`, `:325` / `fio.py:760`, que pode lançar recusa depois da nota |
| Abrir modal/“ler” | URL `urls.py:39`, `views.py:869`; `_is_gestor` `:872` | **GET escreve** `FollowupChamado.lido_dp_em`: `views.py:880` → `services/nota.py:126`–`:133`. Não usar view inteira na sonda |
| Regularização externa criar | URL `urls.py:49`; `views_regularizacao.py:42`; `_gate` `:19` = `_is_gestor` e departamento supervisão; motivo `:49` | `regularizacao_ext.abrir_lote` `:310`; `RegularizacaoExterna` `:237`, chamado de rastro `:247`, vínculo `:257`; sem materializar neste ato |
| Reg externa adicionar/corrigir/remover item | URL `urls.py:51`; `views_regularizacao.py:74`; status `:80`; data/hora `:87`, `:97`; alvo do colab `:109`; horários exigidos `:114`, `:117`, diferentes `:120` | `RegularizacaoItemBatida.get_or_create`, `regularizacao_ext.py:326`: data, horas, acao, batida_alvo, intervalo |
| Reg externa excluir item | URL `urls.py:52`; `views_regularizacao.py:137`; status `:143` | `regularizacao_ext.remover_item` `:332`, `item.delete()` `:336` |
| Reg externa enviar | URL `urls.py:53`; `views_regularizacao.py:154`; status `:160`, itens `:163` | `regularizacao_ext.enviar_lote` `:340`, status `aguardando` em `:343`; pushes `views_regularizacao.py:170`, `:177`. Só colab confirma; materialização recusa `status != confirmada`, `regularizacao_ext.py:52`, ou qualquer disputa aberta do colab `:59`–`:66` | [nao verificado]

**Ramos genéricos adicionais do detalhe/fio, não omitir pelo nome do chamado:**

| Ação | Porta/guarda | Escrita |
|---|---|---|
| `aprovar_excecao` | `views.py:426`, `_pode_aprovar` `:41`; `acoes_chamado.py:25` só encerrou anteriormente; **serviço não exige categoria** | `registrar_batida` `acoes_chamado.py:44`, conflito `:54`, foto `:67`, nota `:69`, transição `:71`, log `:77` |
| `autorizar_geofence` e `_advertir` | `views.py:537`, fio `services/fio.py:571`; contexto `:579`; `acoes_chamado.py:182` encerrado; **serviço não exige categoria** | `_advertir` cria `Advertencia` `:199`; nota `:224`; transição `:229`; log `:230` |
| `recusar_geofence` | `views.py:540`, fio `:571`; `acoes_chamado.py:260` encerrado, `:263` motivo, `:265`–`:273` batida vinculada e pertencente ao colab | `retratar_batida` `:277`, nota `:281`, transição `:286`, log `:287` |
| `aceitar_folga_limbo` | `views.py:432`; `limbo_folgas.py:85` sugestão fixa/dias/template, `:88` template existente | `salvar_tipo` `:92`, altera folgas de `TipoEscala`; log `:95`; reconcilia chamados LIMBO do mesmo template `:99`–`:104` |
| `corroborar_regua` | `views.py:441`, procura vínculo ativo por template; `escala/regua_defesa.py:655` vínculo, `:657` ainda não corroborado | `gravar_regua` `:662`, confiança 0, dias baixo 2, avaliada/corroborada em/por `:666`–`:667`; nota `:672` |
| `realocar_regua` | `views.py:452`; `regua_defesa.realocar_para_destino` `:326`; destino/template `:305`, `:308`; cíclico recusado `:310`; **juiz puro existente** `colaboradores.services.vinculo.julgar_vinculo` chamado `:316`; no-op `:319` | `executar_vinculo` `:321`; nota `:333` |
| `pedir_confirmacao_ancora` | `views.py:459`, fio `:559`; `acoes_disputa.py:185` categoria, `:187` área; helper pode recusar `:190` | `criar_disputa_ancora_de_chamado` `:189`; log `:192`; fábrica em `disputa_emissao.py:1302` |
| `iniciar_disputa_orfaos` | Só fio/modal `services/fio.py:587`; `acoes_disputa.py:219` juiz real `chamado_orfao_e_regularizavel`, nenhum `:220`, repasse recusado `:225` | Fábrica `:222`, log `:229` |
| `aplicar_proposta_escala` | Fio/modal `services/fio.py:547`; `proposta_no_fio.py:136` gate do `INTENCOES`; `_recalcular` `:138` e `:96`; grade dia a dia recusada `:141` | `escritor_de(CHAVE_APLICAR)` `:151` = `escala.services.propositor.aplicar` pelo registro; efeitos finais desse escritor [nao verificado] |
| `recusar_proposta_escala` | Mesmo roteador; `proposta_no_fio.py:168` gate, `:170` motivo, `_recalcular` `:173` | `escala.services.propositor.recusar` `:176`; campos exatos [nao verificado] |

**Fechamento de status não é “permitido” só por `pode_transicionar`:**

`ChamadoColaborador.pode_transicionar` chama o motor em `models.py:119`–`:126`, mas `transicionar` tem guardas seguintes: justificativa pendente `:136`, disputa viva `:153`–`:160`, perguntas vivas ancoradas `:165`–`:171`, premissa de furo `:181`–`:198`. Assim, sonda que só chama `pode_transicionar` precisa rotular “geometria da transição”, não “alcance final da ação”. Há leitor puro do **reconciliar** `reconciliaria`, `models.py:658`–`:662`; ele não substitui todas as guardas da transição humana.

**Forms e diferença entre anunciado/executável:**

- Modal tem forms validar `templates/chamados/partials/modal_fio.html:263`, `:279`; recusar `:290`; corrigir `:301`; conferir data `:173`; regularizar `:353`; reabrir `:368`; administrativo `:388`; geofence `:503`, `:514`, `:525`; folga `:548`; corroborar `:564`; realocar `:573`; exceção `:596`; órfãos `:625`; âncora `:641`.
- Detalhe tem reabrir `templates/chamados/detalhe_local.html:9`, exceção `:256`, geofence `:285`, regularizar `:317`, resposta livre/status `:385`.
- Reg externa tem criar `templates/chamados/regularizacao_painel.html:14`; del item `regularizacao_editar.html:30`, add `:45`, enviar `:63`.
- Cartão de detalhe tem validar `partials/_pergunta_card.html:70`, `:95`, recusar `:112`. Não há corrigir nesse template.
- **Três forms do modal não têm ramo do roteador correspondente:** `aplicar_declaracao_texto` em `modal_fio.html:140`, `confirmar_correcao_pergunta` `:190`, `apagar_pergunta_cega` `:197`; o conjunto de ações AST de `services/fio.py:523` não os inclui e POST sem conteúdo chega a `:781` “Nada a fazer”. `aplicar_declaracao_texto` tem handler **só no detalhe** em `views.py:478`. Os outros dois só apareceram no template e tests, sem handler de produção na busca. Portanto forms anunciados não podem ser contados como porta operante.

**Chamadas reais puras úteis para sonda, com limites:**

| Chamável | Uso correto |
|---|---|
| `_is_gestor(user)`, `_pode_aprovar(user)` | Guardas de papel, `views.py:35`, `:41` |
| `pode_agir_chamado`, `pode_agir_departamento`, `recusa_de_area` | Guardas RBAC reais, `utils.py:68`, `:52`, `:75` |
| `classe_do_lote(perg)` | Guarda classificatória real chamada pelo lote, `validacao.py:880`; lote usa em `:770`; não basta para todos os gates |
| `motivo_implausivel_validacao(perg)` | Mesma guarda lida pelo validar em `validacao.py:429`; origem exportada em models, implementação não auditada aqui |
| `data_do_fato_de(perg)` | Mesma autoridade de divergência chamada por conferir em `validacao.py:693`; não imita as outras guardas |
| `propostas_do_chamado(ch)` | Guarda real de proposta da aplicação em `declaracao_texto.py:649`; retorno pode ter `aceita=False` `:633`–`:637`; não equivale sucesso final |
| `perguntas_corrigiveis(d)` | Conjunto real do reabrir, `acoes_disputa.py:97`, `views.py:1420` |
| `_disputa_do_chamado(ch)` | Resolver real de disputa, `services/fio.py:120`, usado `views.py:1410` |
| `chamado_orfao_e_regularizavel(ch)` | Guarda da emissão órfãos usada `acoes_disputa.py:219` |
| `muda_a_folha(ch, contador, cache)` | Guarda real adicional da cobrança massa, `views_cobrar.py:95`; identidade resolvida pelo alias `:88` |
| `ch.pode_transicionar(status, perfil)` | Só uma parte da guarda humana, `models.py:119`; não confundir com sucesso da porta |
| `ch.reconciliaria()` | Consulta real e explícita sem escrita da reconciliação, `models.py:658` |
| `julgar_vinculo(colab, post, perfil)` | Juiz real da realocação chamado em `regua_defesa.py:316`; gates prévios do destino seguem inline |
| `_recalcular(colab, assinatura, vinculo)` | Guarda de identidade da proposta, `proposta_no_fio.py:96`; pureza transitiva do propositor [nao verificado] |

**Não chamar mutators na sonda**, ainda que retornem dicionário/recusa: `validar_pergunta`, `recusar_pergunta`, `conferir_data_do_fato`, `acao_corrigir_resposta`, `aplicar_declaracao`, `cobrar_dia`, `materializar_regularizacao_externa`, `ch.reabrir`, `arquivo.*`, `acao_*` e views. Guardas completas dessas ações estão embutidas em corpos que escrevem; não há modo dry na assinatura auditada. Mesmo `cobrar_dia` escreve log antes de descobrir resultado final (`cobrar_dia.py:39`). A sonda pode imprimir cobertura dos **subgates reais** e marcar a decisão final não sondável em leitura, sem copiar seus `if`s.

Não inferir validar=0: `validacao.py:418` só examina carimbo, e universo de pergunta devida pode incluir carimbo com texto vazio; medir os campos reais e os juízes, não tratar o nome “sem resposta” como equivalência de `respondida_em is None`.

**Perguntas de lei sugeridas, todas como perguntas:**

1. Uma pergunta devida muda pode receber a hora do ADMIN pela ação `corrigir_resposta`, que não exige fala anterior do colaborador? (`acoes_disputa.py:345`–`:353`, `:371`)
2. A recobrança que renova `prazo_sla` deve alterar a idade e o efeito da pergunta já vencida? (`disputa_emissao.py:1725`)
3. O ADMIN tem a mesma autoridade contextual no endpoint de reabrir/recobrar e no fio, dado o gate condicional do primeiro? (`views.py:1404`, `services/fio.py:604`)
4. Pergunta viva de disputa fechada deve ser tratável igualmente pelo detalhe e pelo fio? (`views.py:491`, `services/fio.py:638`–`:647`)
5. Os forms do modal que não têm handler contam como ação disponível ao ADMIN? (`modal_fio.html:140`, `:190`, `:197`; `services/fio.py:781`)
6. É permitido oferecer regularização externa para dia retido por pergunta cuja disputa aberta bloqueia sua materialização? (`views_regularizacao.py:54`, `regularizacao_ext.py:59`)
7. Ações de exceção e geofence precisam estar vinculadas à categoria correspondente para o servidor aceitá-las? (`acoes_chamado.py:25`, `:182`)
8. Abrir o modal deve ser considerado ação de escrita no censo? (`views.py:880`, `services/nota.py:132`)

## 14/11 18:44 · sessao f2a8e5e329cd · - · - · origem=Ronald (tmux)

- `F._carga` chama a função real `carga_da_tarefa`, sem adaptação: `app/folha/fora_da_folha.py:58`, `:63`.
- O universo contém relógio implícito: `dias_que_seguram` → `_dias_que_retem` → `motivos_retencao_celula` → `_dias_sem_decisao`, cujo `hoje` usa `timezone.localdate()` quando ausente. Evidências: `app/folha/fora_da_folha.py:105`, `:144`; `app/folha/export.py:1212`, `:1238`. Portanto, passar `AGORA` apenas à carga não fixa toda a execução.
- `freezegun==1.5.5` já está declarado: `app/requirements.txt:43`. Usar `freeze_time(AGORA, tick=False)` ao redor de todas as leituras, com `timezone.override('America/Sao_Paulo')`; o fuso da aplicação é esse em `app/config/settings/base.py:93`.
- A carga passa seu instante à fila, C6, consulta do app e partição: `app/chamados/services/tarefa_do_dia.py:509`, `:549`, `:555`; `app/chamados/juizes.py:1570`, `:1573`. Os fallbacks reais ainda leem relógio quando não recebem instante: `app/chamados/juizes.py:592`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:35`.
- O catálogo de ausências usa cache **somente em memória do processo**, sem cache externo: `_CACHE` é um dict (`app/ponto/catalogo/ausencias.py:257`); `vigente` lê `time.monotonic()` e atribui o dict (`:274`, `:279`); `_do_banco` apenas consulta os tipos (`:249`). Não é necessário substituir `vigente`.
- **Há escrita de arquivos já no startup padrão do shell:** `manage.py` usa `config.settings.saas` (`app/manage.py:9`), cujo logging configura `RotatingFileHandler` para `errors.log` e `ponto.log` (`app/config/settings/saas.py:136`, `:160`). Para respeitar nenhum arquivo, iniciar Python com `-B`, importar o módulo de settings e definir nele `LOGGING_CONFIG = None` **antes** de chamar `manage.py` via `runpy.run_path`; só então executar `shell -c` com o corpo em memória. Essa configuração proposta não foi executada [nao verificado].
- Proteção proposta de banco: dentro de `transaction.atomic()`, primeiro comando SQL `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY`; aplicar `schema_context('juliani')` antes das consultas e validar `SHOW transaction_read_only`. Compatibilidade dessa sequência com o ambiente de produção não executada [nao verificado].

## 14/11 18:44 · sessao b8e11c4362a3 · - · - · origem=Ronald (tmux)

- **Cron responsável pelo acervo stale:** `lavrar_perguntas_stale` seleciona exclusivamente perguntas cujo `chamado_destino` está em `VIVOS`; consulta a varredura, limita a rodada a 500 por padrão e chama `lavrar(p)` quando recebe `--apply`. Pergunta sem destino ou com destino fora de `VIVOS` não entra nesse universo. (`app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `:57`, `:58`, `:60`, `:61`, `:87`, `:89`)
- **Cadastro:** execução `*/5 * * * *`, `--apply`, estágio `continuo`, sem dependências; papel explícito **`lavra`**. Isso prova cadastro, não execução em produção. (`app/config/crons.py:390`, `:391`, `:2264`; execução: [nao verificado])
- **Indeterminado isolado não é stale:** a varredura aceita vazio, julgamento ausente/futuro, relógio vencido ou marco posterior; não testa o valor `indeterminado`. Portanto uma lavra indeterminada com carimbos vigentes não ganha nova tentativa exclusivamente por ser indeterminada. (`app/chamados/services/raiox.py:41`, `:43`, `:45`, `:46`)
- **GET não lavra:** o código registra expressamente a transferência dessa escrita para o cron, aceitando até um ciclo de idade do campo. (`app/chamados/services/raiox.py:112`, `:116`, `:117`, `:119`)
- **Nascimento pode ficar vazio:** campo tem `default=''`; nascimento de uma pergunta nova insere a instância sem chamada explícita a `lavrar` nesse ramo. Reuso com mudança de vínculos, ao contrário, relavra antes do save. (`app/chamados/models.py:2663`, `:2664`, `:3097`, `:3100`, `:3080`, `:3086`, `:3087`, `:3089`)
- **Porta de escrita:** `lavrar` julga, transforma exceção em `VL_INDETERMINADO`, atribui os três campos e salva. (`app/chamados/veredito_lavrado.py:254`, `:261`, `:265`, `:269`, `:270`, `:272`, `:273`, `:275`)
- **Evento de materialização:** materializador chama `p.carimbar(...lavrar_veredito=True)`; a porta executa `lavrar(salvar=False)` e grava o carimbo e a lavra no mesmo save. (`app/chamados/services/materializacao.py:1283`, `:1284`; `app/chamados/models.py:3232`, `:3233`, `:3234`, `:3235`)
- **Signal de fechamento do chamado:** S143 reage ao save de status encerrado e chama o resolvedor; este seleciona destino correspondente, materialização ausente e disputa aberta, depois carimba com `lavrar_veredito=True`. Há guardas adicionais, inclusive tranca e materialização em voo. (`app/chamados/signals.py:91`, `:97`, `:105`, `:117`; `app/chamados/services/disputa_emissao.py:1908`, `:1913`, `:1915`, `:1916`, `:1967`, `:1993`, `:1994`)
- **Outro signal:** S144 dispara `materializar_pendentes_aprovadas` quando uma disputa existente fecha; é gatilho da porta de materialização. (`app/chamados/signals.py:149`, `:155`, `:164`, `:165`)
- **Mudança do destino:** `_gravar_estado` relavra ao cruzar a fronteira de `ENCERRADOS`, para perguntas apontando ao chamado e sem validação. (`app/chamados/models.py:201`, `:273`, `:303`, `:309`, `:310`, `:312`)
- **Outras portas:** `reapontar_resolvedora` relavra junto da troca do destino; `rechavear_data_evento` relavra junto do dia/célula; `relavra_via.relavrar` relavra quando muda uma via derivada. (`app/chamados/services/fio.py:843`, `:964`, `:965`, `:966`, `:1018`, `:1076`, `:1081`, `:1082`; `app/chamados/services/relavra_via.py:106`, `:118`, `:121`, `:125`, `:126`, `:127`)
- **Batida/célula não significam relavra direta da pergunta:** `_julgar_dias` chama o cartório; o comentário registra que a relavra reativa de via foi retirada desse caminho pelo custo. (`app/ponto/signals.py:85`, `:93`, `:94`, `:102`, `:103`)
- **Backfill é sob ordem, não rotina:** cadastro declara esse papel; comando só inclui mudança do valor do veredito, deixa divergência exclusiva de relógio fora e não substitui lavra preenchida por indeterminado. (`app/config/crons.py:1420`; `app/chamados/management/commands/backfill_veredito_lavrado.py:115`, `:120`, `:122`, `:124`, `:131`, `:138`, `:143`, `:146`)

**Limite do censo:** não se pode atribuir falha de cron, signal ou porta a qualquer um dos 136 dias sem os campos das perguntas e seus destinos; execução e causa individual permanecem [nao verificado].

## 14/11 18:43 · sessao bcac34a25ccd · BO-DISPUTA-SEM-PERGUNTA · censo · origem=Code

Numerei as linhas dos anexos a partir da primeira linha de conteúdo, incluindo linhas vazias.

**Há duas limitações para a classificação:** a última pergunta examina `9bba8132b`, não o hash final; e mencionar uma triagem não comprova que ela descartou um achado. (`preverif6.q:1`; `opus_final_1_reprovado.txt:1`; `CLAUDE.md:384-386`)

Na tabela, **(a)\*** significa tema perguntado, mas achado específico não apontado nas respostas fornecidas. A presença da alteração final no hash anteriormente examinado fica **[nao verificado]**.

| Achado do opus | Classe | Trecho da pergunta | Trecho da resposta e justificativa |
|---|---|---|---|
| **1. Eleitores divergentes, escritor único e selo ausentes** | **Parcialmente visto; (a)\* no confronto específico com S160. (b) [nao verificado].** | `preverif6.q:4`: “Censo de TODO escritor de PerguntaDisputa.chamado_destino”; `preverif6.q:8`: “2o juiz, 2o escritor fora da porta […] derivacao paralela”. | `preverif6.r:1`: “não estabelece a regra nos escritores”; `preverif6.r:27`: “A eleição do chamado continua divergente entre lote e avulso”. Portanto, a incompletude **foi vista**. O confronto final `_destino_do_intervalo` × S160 não foi apontado nesses trechos; hoje os dois estão em `app/chamados/services/fio.py:747` e `app/chamados/services/disputa_emissao.py:167`. Descarte pela triagem: **[nao verificado]**. |
| **2. Ampliação do push diário para disputas abertas sem pergunta** | **(a)\*** para o encadeamento com o alerta diário; **(c)** para quantidade em produção. | `preverif3.q:12-14`: “Push […] e qualquer outro envio”, “em cada caminho”, “push sem pergunta local criada no ato”. | `preverif4.r:31`: “sem ata, permanece aberta”; `preverif4.r:40` identifica push direto “no caso HX sem pergunta”. Não aponta a ampliação pelo leitor diário, cujo filtro seleciona abertas sem exigir pergunta (`app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`). Quantidade atingida: **[nao verificado]**. |
| **3. Protegidos receberam ata e perderam cobertura sem ata** | **(a)\*** quanto à perda de cobertura; integridade histórica **[nao verificado]**. | `preverif3.q:19-22`: “cada teste morde?” e protegidos “seguem intocados e valendo?”; `preverif4.q:22-24`: “seguem intocados?”. | `preverif4.r:95`: “Os asserts protegidos estão presentes”, mas integridade histórica “**[nao verificado]**”. Não aponta o deslocamento da premissa para ata presente. Na cópia final, ambos chamam o helper em `app/chamados/tests/test_hx_disputa_muda.py:45` e `:66`. |
| **4. Três fontes de dia em `_data_fallback`** | **(a)**: código descrito, violação específica não apontada. | `preverif2.q:11-13`: comparar “ramo a ramo: texto, destino, dia”; `preverif2.q:22-24`: “derivacao paralela de fonte unica”. | `preverif2.r:52` enumera “recuperação pela grade; data declarada do motivo; batida pelo turno” como ramos preservados. Reconhece as fontes, mas não as apresenta como o achado do opus. A cadeia permanece em `app/chamados/services/disputa_emissao.py:376`, `:398` e `:413`. |
| **5. Qualquer pergunta, inclusive respondida/morta, habilita push repetido** | **(a)\***. | `preverif3.q:12-14`: push “em cada caminho”, “com quais motivos”; `preverif4.q:13-15`: órfãos com `motivos_atualizados`. | `preverif4.r:40`: envia `motivos_atualizados` “inclusive na repetição do mesmo ato”. Não aponta a diferença entre existência de linha e pergunta viva. A guarda final exige somente `.exists()` em `app/chamados/services/disputa_emissao.py:1553`. |
| **6. Push no atomic; âncora/recobrança sem guarda; mensagem falsa** | **Visto parcialmente; (b) [nao verificado] para o atomic. (a)\* nos demais subachados.** | `preverif4.q:13-18`: push em cada caminho e efeitos externos escapando de rollback; `preverif3.q:12`: “qualquer outro envio”. | `preverif6.r:23`: “complementos e principal notificam dentro do atomic externo”; `preverif4.r:51`: envio imediato pode anteceder rollback. Esse risco **foi visto**. `preverif4.r:43` descreve o envio da âncora, sem apontar sua falta de condição. Os trechos não apontam recobrança com zero pendentes nem a mensagem baseada em `len(orfaos)` (`app/chamados/services/disputa_emissao.py:1976`, `:1992`; `app/chamados/services/acoes_disputa.py:232`). |
| **7. Agregação restrita a motivos com pergunta local; leitores não verificados** | **Restrição vista; (a) para a lacuna de leitores. (b) [nao verificado].** | `preverif3.q:24-25` e `preverif4.q:25-26`: “QUEM ESCREVE e QUEM LE” `DS.motivos`, incluindo significado divergente. | `preverif4.r:21`: “os motivos admitidos em A vêm das perguntas locais”. Portanto, a restrição **foi descrita**. O achado final é também a ausência de verificação dos leitores, apesar da pergunta explícita; a restrição está em `app/chamados/services/disputa_emissao.py:679-688`. Descarte desse risco pela triagem: **[nao verificado]**. |
| **8. Trilha engolida/I23 cego e pendentes fora do núcleo** | **Visto. Classe (b) [nao verificado] por faltar comprovação do descarte.** | `preverif5.q:4`: “Algum caminho […] sai sem trilha?”; `preverif6.q:3`: estado de cada achado anterior. | `preverif6.r:16`: “gravação pós-commit pode falhar”; `preverif6.r:21`: pendentes “após o retorno”. Os dois componentes **foram apontados**. O RELATO registra a triagem como encaminhada, sem apresentar seu resultado nessa linha (`app/docs/RELATO.md:858`). |
| **9. ERROR no RED c22 e mordida c24 fora do diff** | **(c)**. | `preverif6.q:8`: pergunta pelos “mutantes da entrega”; essa entrega é a c21 (`preverif6.q:1`). | `preverif6.r:19` trata dos mutantes anteriores; `preverif6.r:32` mantém suite e protegido ORM como não verificados. Não certifica RED c22 nem mutações c24. O ERROR e a ressalva sobre c24 constam do parecer final (`opus_final_1_reprovado.txt:10`); causa por fixture: **[nao verificado]**. | [nao verificado]

**Não há descarte de triagem comprovado para fechar (b), nem evidência suficiente para atribuir (d) MODO-COMUM.** A regra exige as linhas de triagem e define modo comum como omissão de ambos os leitores; esses elementos não estão demonstrados para os achados acima. (`CLAUDE.md:384-387`; `CLAUDE.md:393`; **[nao verificado]**)

## 14/11 18:42 · sessao 988e591b1cf0 · O50-DESLIGAR-ENCERRA-TUDO · verificador · origem=Code

**23 CONFERE, 2 DIVERGE, 0 SUMIU**, conforme as linhas abaixo. **F05 e F06 caem como provas do caminho RET→TXT/vigia**: a saída por rescisão ocorre antes da retenção. Os campos crus continuam presentes. (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`, `app/folha/export.py:1241`, `app/folha/export.py:1269`)

F01 aparece uma vez na tabela, pois foi repetido na amostra fornecida. A identidade Git do HEAD informado é **[não verificado]**.

Nos contraexemplos, **S** significa situação atual discordante de vínculo válido; **A**, EC inativa com vigência cobrindo o dia ou ativa com início futuro; **B**, igualdade entre dia, demissão e fim do vínculo. A autoridade aceita a igualdade e vínculos inativos datados, sem consultar situação ou demissão do colaborador. São possibilidades de código; ocorrência em produção é **[não verificado]**. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`)

| ID | Arquivo:linha na cópia | Cru lido? | Predicado confere? | Alcance folha/fio e caminho comprovado | CE procede? | Veredito |
|---|---|---|---|---|---|---|
| **F01** | `app/folha/export.py:60` | **Sim**, D | **Sim:** D não nula e D≤fim. | **Folha sim.** `gerar_dominio_txt` (`app/folha/views.py:212`) → `classificar_export` (`app/folha/export.py:866`) → `sai_pela_rescisao` (`app/folha/export.py:60`). Retorna `fora/rescisao` e continua (`app/folha/export.py:867`, `app/folha/export.py:871`); a montagem recebe esses itens (`app/folha/views.py:268`) e exclui quem não está em `entra` (`app/folha/export.py:716`, `app/folha/export.py:726`). | **Sim**, como divergência entre rota financeira e vínculo válido, conforme autoridade acima. | **CONFERE** |
| **F02** | `app/folha/export.py:807`; `app/folha/export.py:810` | **Sim**, A+S | **Sim:** A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos (`app/folha/export.py:808`, `app/folha/export.py:809`, `app/folha/export.py:811`). | **Folha sim.** `aprovar_fechamento` → `classificar_export` (`app/ponto/views.py:1299`) → seleção crua (`app/folha/export.py:806`). Produz `sem_ancora_12x36` e `travas_aprovacao` (`app/folha/export.py:835`, `app/folha/export.py:848`); o consumidor lê a trava, exclui os PKs e aprova somente os restantes (`app/ponto/views.py:1301`, `app/ponto/views.py:1311`, `app/ponto/views.py:1315`, `app/ponto/views.py:1365`). | **Sim**, S/A, conforme autoridade acima. | **CONFERE** |
| **F05** | `app/folha/export.py:1241`, agora em `_dias_sem_decisao` | **Sim**, D | **Sim localmente:** corta fim até D inclusive; janela colapsada retorna vazio (`app/folha/export.py:1242`, `app/folha/export.py:1243`, `app/folha/export.py:1246`). **Há guarda anterior nova:** desligado não chega ao helper pelo retentor (`app/folha/export.py:1207`, `app/folha/export.py:1212`). | **Não alcança TXT/vigia pelo caminho alegado.** Corte efetivo exige D<fim; F01 já dá `continue` antes do retentor (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). `dias_que_seguram` chama `_dias_que_retem` somente para classificados `furo_espelho` (`app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`). Persiste diagnóstico: `recalcular_fechamento_mes` → `motivos_retencao_espelho` (`app/ponto/services/fechamento.py:1067`) → `motivos_retencao_celula` (`app/folha/export.py:1307`) → helper (`app/folha/export.py:1212`), gravado em `motivos_espelho` (`app/ponto/services/fechamento.py:1186`). | **Sim localmente**, para S≠desligado; **não demonstra a trava alegada**. Para desligado, o helper serve ao informativo (`app/folha/export.py:1226`, `app/folha/export.py:1228`). | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **F06** | `app/folha/export.py:1269`; `app/folha/export.py:1271` | **Sim**, D | **Sim localmente:** célula ausente só gera `sem_celula` com d<hoje e d≠D; também há exceção para célula sem veredito (`app/folha/export.py:1272`, `app/folha/export.py:1295`). Mesma guarda anterior de desligado (`app/folha/export.py:1207`). | **Não alcança TXT/vigia pelo caminho alegado.** A igualdade precisa ocorrer no laço d≤fim (`app/folha/export.py:1265`, `app/folha/export.py:1269`), portanto F01 já excluiu antes da retenção (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). Restam diagnóstico de fechamento e leitura individual: `o_que_segura` → `_dias_que_retem` → retentor (`app/folha/fora_da_folha.py:224`, `app/folha/fora_da_folha.py:98`), seguida de leitura do fio (`app/folha/fora_da_folha.py:228`). | **Sim localmente**, B com S≠desligado; **não como prova de trava TXT/vigia**, pelo bloqueio anterior citado. | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **P34** | `app/ponto/services/fechamento.py:432`; `app/ponto/services/fechamento.py:438`; `app/ponto/services/fechamento.py:440` | **Sim**, S+D | **Sim:** S≠desligado OU desligado com D≥início da janela da empresa; sem teto superior (`app/ponto/services/fechamento.py:441`). | **Folha sim.** `recalcular_fechamento_mes` → `universo_da_competencia` (`app/ponto/services/fechamento.py:554`) → queryset retornado (`app/ponto/services/fechamento.py:445`) → laço (`app/ponto/services/fechamento.py:619`) → gravação do fechamento (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`). | **Sim:** desligado com D nula fica fora mesmo com EC válida, pelo predicado citado e autoridade acima. | **CONFERE** |
| **C35** | `app/escala/services/leitor_celula.py:365`; `app/escala/services/leitor_celula.py:389` | **Sim**, D | **Sim:** d>D OU d=D sem fato; também ramo anterior à admissão. Fato = minutos realizados OU lâmpadas não missing (`app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:386`). | **Folha e fio sim.** Folha: `recalcular_fechamento_mes` → `grade_do_fechamento` (`app/ponto/services/fechamento.py:1014`) → `grade_da_celula` (`app/folha/export.py:338`) → `fora_vinculo` (`app/escala/services/leitor_celula.py:393`), excluído dos dias internos e trabalho/ausência (`app/ponto/services/fechamento.py:1043`, `app/ponto/services/fechamento.py:1046`), alterando previsto (`app/ponto/services/fechamento.py:1083`). Fio: `gerar_perguntas_disputa` → `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`) → grade (`app/chamados/services/disputa_emissao.py:83`); fora de trabalho retorna None (`app/chamados/services/disputa_emissao.py:85`, `app/chamados/services/disputa_emissao.py:87`), podendo causar `continue` antes de abrir pergunta (`app/chamados/services/disputa_emissao.py:366`, `app/chamados/services/disputa_emissao.py:444`). | **Sim**, B, conforme autoridade inclusiva acima. | **CONFERE** |
| **P39** | `app/ponto/services/he_pendente_lavrado.py:92` | **Sim**, S | **Sim:** universo da empresa com S=ativo; produz retrato (`app/ponto/services/he_pendente_lavrado.py:160`, `app/ponto/services/he_pendente_lavrado.py:169`). | **Folha sim, condicional.** `lavrar_he_pendente.handle` injeta revogador e chama `apurar` (`app/ponto/management/commands/lavrar_he_pendente.py:56`, `app/ponto/management/commands/lavrar_he_pendente.py:62`) → callback por dia (`app/ponto/services/he_pendente_lavrado.py:143`) → `revogar_recusa_do_sistema` → marcação revogada (`app/ponto/portas/he.py:472`, `app/ponto/portas/he.py:375`) → decisões vigentes em `estado_por_dia` (`app/ponto/services/gestao_he.py:86`) → `medir` (`app/folha/porta_export.py:684`, `app/folha/porta_export.py:686`) → trava condicional e recusa (`app/folha/porta_export.py:230`, `app/folha/porta_export.py:853`). A recusa automática de ponta pequena estável, isoladamente, não comprova trava: ela rejeita acima do limite, faixa que o portão verifica (`app/ponto/portas/he.py:313`, `app/folha/porta_export.py:230`). | **Sim**, S pode omitir dia e impedir revogação pelo callback citado. | **CONFERE — alcance condicional.** |
| **P44** | `app/ponto/services/vigia_de_hora.py:117` | **Sim**, S | **Sim:** pula desligado; também pula isento (`app/ponto/services/vigia_de_hora.py:119`). | **Fio sim.** `rodar` → `julgar_celula` (`app/ponto/services/vigia_de_hora.py:131`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → emissão sob aplicação e demais guardas (`app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1829`) → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:694`). | **Sim**, S; inclui candidatos de hoje/ontem (`app/ponto/services/vigia_de_hora.py:114`, `app/ponto/services/vigia_de_hora.py:115`). | **CONFERE** |
| **P66** | `app/chamados/services/pergunta_no_app.py:186` | **Sim**, S | **Sim:** chamados em cobrança, módulos selecionados e S=ativo (`app/chamados/services/pergunta_no_app.py:185`). | **Fio sim, sob apply.** `Command.handle` → `medir` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21`) → achados (`app/chamados/services/pergunta_no_app.py:201`) → `perguntar` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`) → `criar_disputa_a_partir_de_chamados_orfaos` (`app/chamados/services/pergunta_no_app.py:246`) → `_complementar_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:1175`) → `_criar_pergunta_se_couber` (`app/chamados/services/disputa_emissao.py:881`) → `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:837`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **P65** | `app/chamados/services/limbo_folgas.py:65` | **Sim**, A | **Sim:** conta vínculos A=True do template. | **Não alcança decisão de folha/fio.** `aplicar_sugestao` → `texto_seco` → `reescrever_texto` (`app/chamados/services/limbo_folgas.py:70`); a contagem forma apenas sufixo textual (`app/chamados/services/limbo_folgas.py:51`). Aceitação usa outros dados (`app/chamados/services/limbo_folgas.py:82`, `app/chamados/services/limbo_folgas.py:85`). | **Sim**, A para o universo contado; efeito é textual, como citado. | **CONFERE** |
| **P22** | `app/ponto/selecao_periodo.py:278` | **Sim**, A | **Sim:** jornada de EC ativa; fallback 24h (`app/ponto/selecao_periodo.py:287`). | **Não alcança no fluxo indicado.** `turnos_em_turno_stored` retorna mapa (`app/ponto/selecao_periodo.py:293`) → `colabs_em_turno_stored` (`app/ponto/selecao_periodo.py:253`) → dashboard usa tamanho (`app/core/views.py:1160`, `app/core/views.py:1161`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **C64** | `app/ferias/management/commands/alertar_retornos.py:30` | **Sim**, S | **Sim:** S=ativo, hoje folga e amanhã trabalha (`app/ferias/management/commands/alertar_retornos.py:34`). | **Não alcança folha/fio decisório.** `handle` monta retornos (`app/ferias/management/commands/alertar_retornos.py:35`) → `push_dp`, aviso de disponibilidade (`app/ferias/management/commands/alertar_retornos.py:45`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T13** | `app/colaboradores/views.py:619` | **Sim**, S | **Sim:** S=ativo, em outro posto. | **Não alcança.** `buscar_colabs_posto` termina em JSON limitado a 30 (`app/colaboradores/views.py:622`, `app/colaboradores/views.py:626`); o POST resolve o colaborador separadamente (`app/colaboradores/views.py:640`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T26** | `app/colaboradores/views.py:2900` | **Sim**, S | **Sim:** após juiz de férias, exclui S=desligado. | **Não alcança.** `disponibilidade` acrescenta em `parados` (`app/colaboradores/views.py:2905`) e renderiza painel (`app/colaboradores/views.py:2933`). | **Sim**, S, havendo férias cobrindo hoje no fluxo citado. | **CONFERE** |
| **T39** | `app/colaboradores/views.py:48` | **Sim**, A | **Sim:** prefetch somente A=True. | **Não alcança.** `vincular_colaboradores` usa o prefetch para `sem_escala` (`app/colaboradores/views.py:71`, `app/colaboradores/views.py:73`), filtros (`app/colaboradores/views.py:97`) e render (`app/colaboradores/views.py:126`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **T52** | `app/colaboradores/views_fase.py:45` | **Sim**, A | **Sim:** última A=True por início. | **Não alcança.** `fase_12x36` usa template/âncora (`app/colaboradores/views_fase.py:47`, `app/colaboradores/views_fase.py:49`), passa EC aos previews (`app/colaboradores/views_fase.py:98`) e renderiza (`app/colaboradores/views_fase.py:115`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **P51** | `app/ponto/worklist.py:49` | **Sim**, A | **Sim:** primeira A=True fornece posto. | **Não alcança.** `coletar_gaps_dp` inclui posto no resultado (`app/ponto/worklist.py:50`, `app/ponto/worklist.py:66`) → `painel_inconsistencias_posto` agrupa e renderiza (`app/colaboradores/views.py:3025`, `app/colaboradores/views.py:3029`). | **Sim**, A para agrupamento, conforme autoridade acima. | **CONFERE** |
| **C11** | `app/escala/views_plano_folgas.py:176` | **Sim**, A | **Sim:** exige A=True; ausência gera erro `vinculo` e pula item (`app/escala/views_plano_folgas.py:178`, `app/escala/views_plano_folgas.py:179`). | **Folha e fio sim.** `plano_folgas_aplicar` → `aplicar_folgas_em_lote` (`app/escala/views_plano_folgas.py:185`) → `relavrar_celulas_da_foto` (`app/escala/folgas.py:418`) → `regenerar_celulas_vinculo` (`app/escala/folgas.py:293`) → `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:392`) → julgamento (`app/ponto/portas/celula.py:472`), recálculo (`app/ponto/portas/celula.py:496`) e reavaliação/rejunção (`app/ponto/portas/celula.py:536`, `app/ponto/portas/celula.py:539`). | **Sim**, A, para vínculo inativo cobrindo dia editável. | **CONFERE** |
| **C26** | `app/escala/services/cadastro_realidade.py:575` | **Sim**, S | **Sim:** exclui S=desligado. | **Não alcança decisão de folha/fio por esse corte.** `_afastado_sem_registro` produz C4 sem data (`app/escala/services/cadastro_realidade.py:600`) → `lista` (`app/escala/services/cadastro_realidade.py:794`). Existe consumo estrutural pelo vigia: `ListaDoAdmin._carregar` (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:218`) → `colab_na_lista` (`app/chamados/services/vigia_fio.py:251`) → causa d/e (`app/chamados/services/vigia_fio.py:682`). Porém desligados já ficam sem dias nesse universo pelo retentor (`app/chamados/services/vigia_fio.py:586`, `app/folha/export.py:1207`); C4 sem data também não entra no mapa de dias (`app/escala/services/cadastro_realidade.py:1033`). | **Sim**, S quanto à lista C4; não comprova alteração da trava/vigia, pelos bloqueios citados. | **CONFERE** |
| **C40** | `app/ponto/management/commands/reconciliar_grade.py:106` | **Sim**, S | **Sim:** S=ativo e universo de batidas/chamados/operação (`app/ponto/management/commands/reconciliar_grade.py:84`, `app/ponto/management/commands/reconciliar_grade.py:95`, `app/ponto/management/commands/reconciliar_grade.py:105`). | **Fio sim sob apply.** `handle` → `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:250`, `app/ponto/management/commands/reconciliar_grade.py:254`) → `ChamadoColaborador.abrir` ou `reabrir` (`app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:530`); outro ramo chama `retratar` (`app/ponto/management/commands/reconciliar_grade.py:309`). Cron está sem apply (`app/config/crons.py:462`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **C53** | `app/ponto/management/commands/reconciliar_fantasmas.py:174` | **Sim**, A | **Sim:** primeira A=True fornece diagnóstico. | **Não decide a transição.** EC → `_completude` → `detalhe` (`app/ponto/management/commands/reconciliar_fantasmas.py:205`, `app/ponto/management/commands/reconciliar_fantasmas.py:206`). Decisão usa núcleo e premissa separadamente (`app/ponto/management/commands/reconciliar_fantasmas.py:248`, `app/ponto/management/commands/reconciliar_fantasmas.py:256`); detalhe entra apenas no motivo passado à retratação (`app/ponto/management/commands/reconciliar_fantasmas.py:278`, `app/ponto/management/commands/reconciliar_fantasmas.py:280`). | **Sim**, A no diagnóstico, conforme autoridade acima. | **CONFERE** |
| **C70** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1476` | **Sim**, S | **Sim:** S≠ativo é pulado em A4. | **Não alcança emissão/reabertura.** O corpo alimenta resultado A4 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1485`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1505`), stdout e HTML opcional (`app/chamados/management/commands/auditar_invariantes_chamados.py:1841`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1853`). | **Sim**, S no universo auditado, conforme autoridade acima. | **CONFERE** |
| **A24** | `app/relatorios/furos.py:117` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `dataset_furos` monta e retorna cards (`app/relatorios/furos.py:142`, `app/relatorios/furos.py:167`) → view (`app/relatorios/views.py:1541`) → PDF ou render (`app/relatorios/views.py:1562`, `app/relatorios/views.py:1566`). | **Sim**, S em consulta histórica, conforme autoridade acima. | **CONFERE** |
| **A37** | `app/inteligencia/calculadores.py:139` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `preenchimento_marcos` calcula e retorna linhas (`app/inteligencia/calculadores.py:162`, `app/inteligencia/calculadores.py:177`) → `metricas.calcular` (`app/inteligencia/metricas.py:42`) → grava `MetricaSnapshot` (`app/inteligencia/metricas.py:47`). | **Sim**, S em janela histórica, conforme autoridade acima. | **CONFERE** |
| **A50** | `app/inteligencia/views.py:209` | **Sim**, S | **Sim:** S∈{ativo,ferias,afastado}, com empresa opcional. | **Não alcança.** A base de `api_kpi` alimenta ramos de JSON: ativos, sem escala, sem ausência, aniversariantes e disponíveis (`app/inteligencia/views.py:222`, `app/inteligencia/views.py:228`, `app/inteligencia/views.py:235`, `app/inteligencia/views.py:263`, `app/inteligencia/views.py:295`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |

**Consolidação das 25 linhas: 23 conferem, 2 divergem, 0 sumiram.** Dos seis “folha diretamente”, **F05 e F06 caem na fundamentação RET→TXT/vigia**, enquanto **F01, F02, P34 e C35 mantêm os caminhos diretos documentados acima**. A consequência exata sobre antecipação/prazo depende do aval não fornecido: **[não verificado]**.

_5235 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
